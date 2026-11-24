---
title: "Kaivonkannet konenäöllä – YOLO vai Faster R-CNN?"
description: Tiimiprojekti yrityksen aineistolla. Annotointiprosessi, esikäsittelykokeet ja kahden kohteentunnistusmallin vertailu.
---

# Kaivonkannet konenäöllä – YOLO vai Faster R-CNN?

Kevään 2026 projektikurssilla kolmen hengen tiimimme sai tehtäväksi kouluttaa neuroverkon, joka löytää kaivonkannet yrityksen kuva-aineistosta. Malli piti opettaa sekä paikantamaan kannet kuvasta että luokittelemaan ne umpi- tai ritiläkansiksi. Aineistossa oli yli 10 000 kuvaa.

Toimin tiimissä Product Ownerina. Vastasin annotointiprosessin suunnittelusta, testasin kuvakoon ja väriesikäsittelyn vaikutusta ja vastasin vertailun toisesta mallista, Faster R-CNN:stä.

!!! note "Miksi koodia tai kuvia ei ole mukana"
    Aineisto ja projektin koodi kuuluvat toimeksiannon yhteydessä tehtyyn työhön, joten niitä ei ole julkaistu. Tällä sivulla kerron työstä yleisellä tasolla ilman aineiston kuvia.

**Pino:** Python, PyTorch, Ultralytics YOLO, torchvision Faster R-CNN, Label Studio, PostgreSQL, marimo, Docker · **Työtapa:** viikon sprintit, muutokset merge requestien kautta

## Aineisto ei ollut helppo

Kuvat oli otettu oikeissa olosuhteissa, eivätkä ne olleet siistejä harjoitusaineistoja. Suurimmat haasteet olivat:

- **Luokkaepätasapaino.** Umpikansia oli selvästi enemmän kuin ritiläkansia.
- **Kuvanlaatu.** Osa kuvista oli epätarkkoja, varjoisia tai ylivalottuneita, ja joissakin sormi peitti osan linssistä.
- **Pienet kohteet.** Osa kansista oli kaukana ja kuvassa hyvin pieniä, ja samassa kuvassa saattoi olla useita.
- **Peittymät.** Hiekka, kasvillisuus tai muut esineet peittivät kansia osittain.

## Annotointiprosessi

Suunnittelin annotoinnin kaksivaiheiseksi:

1. **Käsin annotointi.** Annotoimme ensin noin 600 kuvaa käsin Label Studiossa.
2. **Malliavusteinen annotointi.** Näillä kuvilla koulutettu YOLO-malli kytkettiin Label Studioon ehdottamaan rajauslaatikoita. Ihminen tarkisti ja korjasi ehdotukset, mikä nopeutti työtä huomattavasti.

Annotaatiot tallennettiin PostgreSQL-tietokantaan, josta datan jako koulutus-, validointi- ja testijoukkoihin tehtiin sovituilla ehdoilla.

Label Studion vientidata tarkistettiin ennen tietokantaan tuontia, ja selvät klikkausvirheet, kuten lähes nollakokoiset rajauslaatikot, poistettiin. Lisäksi aineistosta löytyi vioittuneita ja vajaita kuvatiedostoja. Korjasimme vioittuneet JPEG-otsakkeet ja estimme vajaiden kuvien pääsyn koulutusdataan: datasetin muodostava skripti avaa jokaisen kuvan ja ohittaa ne, joita ei voi käsitellä. Rikkinäisistä kuvista tuli hyvä opetus: YOLO pystyi käsittelemään ne, mutta Faster R-CNN:n koulutus kaatui niihin. Jos olisimme käyttäneet vain YOLOa, ongelma olisi voinut jäädä huomaamatta.

## Esikäsittelykokeet

Aineiston haasteista nousi kolme kysymystä, joita testasimme ennen lopullista koulutusta:

| Koe | Mitä testattiin | Tulos |
|---|---|---|
| **Kuvakoko** | 640 vs. 960 pikseliä, koska osa kohteista oli pieniä | Suurempi kuvakoko paransi tuloksia hieman, eikä koulutusaika kasvanut merkittävästi |
| **Väriesikäsittely** | Harmaasävy, kontrastin muutos ja matalampi värikylläisyys | Pidemmässä koulutuksessa alkuperäiset kuvat toimivat parhaiten |
| **Downsampling** | Umpikansien vähentäminen koulutusdatasta | Auttoi mallia erottamaan harvinaisemman ritiläluokan |

Tein kuvakoko- ja väriesikäsittelykokeet itse. Lopulliseksi asetelmaksi valikoitui 960 pikselin kuvakoko, tasapainotettu aineisto ja alkuperäiset kuvat ilman väriesikäsittelyä.

## Faster R-CNN: YOLOa tuli ikävä

Vastasin vertailun toisesta mallista, Faster R-CNN:stä (ResNet50-FPN v2). Työ osoittautui paljon työläämmäksi kuin YOLOn kanssa. YOLO hoitaa datan lukemisen, koulutuksen ja arvioinnin pitkälti valmiiksi, mutta Faster R-CNN:lle piti rakentaa itse muun muassa YOLO-muotoisten annotaatioiden muunnos mallin ymmärtämään muotoon, koulutus, validointi, metriikoiden laskenta ja virheanalyysi. Sen jälkeen ymmärrän paljon paremmin, mitä valmiit kirjastot tekevät käyttäjän puolesta.

Ero näkyy hyvin koodissa. YOLOn koulutus on käytännössä yksi funktiokutsu:

```python title="YOLOn koulutus (ote omasta notebookistani)"
model = YOLO("yolo26s.pt")

model.train(
    data="data/yolo_downsampled/data.yaml",
    epochs=300,
    imgsz=640,
    device=0,   # GPU
    batch=16,
    patience=40,
)
```

Faster R-CNN:lle sama data piti ensin muuntaa. YOLO tallentaa rajauslaatikon keskipisteenä, leveytenä ja korkeutena suhteessa kuvan kokoon, mutta Faster R-CNN odottaa laatikon kulmapisteitä pikseleinä. Tämä muunnos oli vasta yksi palanen: sen lisäksi tarvittiin oma Dataset-luokka, koulutussilmukka, validointi ja metriikoiden laskenta. Koko koulutus- ja validointinotebook kasvoi lähes 1 300 riviin.

```python title="YOLO-laatikon muunnos Faster R-CNN:n muotoon (lyhennetty ote omasta notebookistani)"
def convert_yolo_box_to_xyxy(x_center, y_center, width, height, image_width, image_height):
    # YOLO: keskipiste ja koko suhteellisina arvoina (0–1)
    box_x_center = x_center * image_width
    box_y_center = y_center * image_height
    box_width = width * image_width
    box_height = height * image_height

    # Faster R-CNN: vasen yläkulma ja oikea alakulma pikseleinä
    x_min = box_x_center - box_width / 2
    y_min = box_y_center - box_height / 2
    x_max = box_x_center + box_width / 2
    y_max = box_y_center + box_height / 2

    # Varmistetaan, ettei laatikko mene kuvan ulkopuolelle.
    x_min, x_max = max(0.0, x_min), min(x_max, image_width)
    y_min, y_max = max(0.0, y_min), min(y_max, image_height)

    return [x_min, y_min, x_max, y_max]
```

## Tulokset: kumpi malli on parempi?

| Malli | mAP50 | mAP50-95 | Precision | Recall |
|---|---:|---:|---:|---:|
| YOLO26s | 0,907 | 0,707 | 0,941 | 0,906 |
| Faster R-CNN ResNet50-FPN v2 | 0,918 | 0,613 | 0,837 | 0,939 |

*Testijoukon tulokset.*

Mallit päätyivät lähes samaan mAP50-tulokseen, mutta ne tekevät eri asioita hyvin. **YOLO osuu useammin oikeaan** (korkeampi precision) mutta jättää useamman kannen löytämättä. **Faster R-CNN löytää enemmän kansia** (korkeampi recall) mutta tekee enemmän vääriä havaintoja. YOLO paikantaa kannet myös tarkemmin, mikä näkyy korkeampana mAP50-95-arvona.

Yhtä oikeaa vastausta ei siis ole. Jos tärkeintä on löytää kaikki kannet ja ihminen tarkistaa tulokset joka tapauksessa, Faster R-CNN:n korkeampi recall on arvokkaampi. Jos väärät havainnot tulevat kalliiksi, YOLO on parempi valinta.

Virheanalyysissä mallien virheet jakautuivat muutamaan tyyppiin: saman kannen tuplaennusteet, kokonaan löytämättä jääneet kannet ja matalan varmuuden ennusteet. Osa "virheistä" paljastui itse asiassa virheellisiksi annotaatioiksi: malli oli oikeassa ja merkintä väärin. Tarkistimme jokaisen malliavusteisen ehdotuksen käsin, ja silti virheitä jäi. Sellaista se on human in the loop -työskentelyssä: välillä silmukan heikoin lenkki on se ihminen. 😄

Lopuksi teimme mallin ympärille yksinkertaisen demosovelluksen, joka käy läpi kansiollisen kuvia ja tallentaa havainnot CSV-tiedostoon.

## Mitä opin

**Datan laatu ratkaisee.** Rikkinäinen kuva kaatoi toisen mallin koulutuksen, ja virheelliset annotaatiot näyttivät virheanalyysissä mallin virheiltä. Malli on vain niin hyvä kuin data, jolla se koulutetaan – *garbage in, garbage out*.

**Mallin valinta ei ole pelkkä luku.** Kaksi mallia, joilla on lähes sama mAP50, voivat sopia aivan eri käyttötarkoituksiin. Valinta riippuu siitä, kumpi virhe on kalliimpi: löytämättä jäänyt kansi vai väärä havainto.

---

!!! note "Projektin tausta"
    Projektikurssin tiimityö kevät 2026, toimeksiantona yrityksen aineisto. Tiimissä oli kolme opiskelijaa. Sivulla kerrotut tulokset ovat tiimin yhteisiä, ja omat osuuteni on mainittu erikseen.
