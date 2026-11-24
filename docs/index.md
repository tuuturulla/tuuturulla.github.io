---
hide:
  - navigation
---

<div class="hero" markdown>

[In English →](en/index.md){ .lang-switch }

# Sara Pyky

Opiskelen tieto- ja viestintätekniikan insinööriksi Datasta tekoälyyn -linjalla ja haen harjoittelupaikkaa pääkaupunkiseudulta kesälle tai syksylle 2027. Tähän portfolioon olen koonnut kokeiluja ja havaintoja datan, koneoppimisen ja tekoälyn parissa: mitä mallit tekevät hyvin ja missä ne menevät pieleen.

</div>

## Minusta

Minua kiinnostavat erityisesti koneoppiminen, tekoälyn käytännön hyödyntäminen ja niiden taustalla virtaava bensiini eli data. Myös tekoälyn eettiset ja tietoturvaan liittyvät kysymykset kiinnostavat minua.

Minun mielestäni parhaiten oppii, kun on sormet sopassa eli tekee itse. Projektien kautta olen päässyt kokeilemaan erilaisia koneoppimisen menetelmiä sekä työskentelemään niin datan valmistelun kuin feature engineeringin parissa.

Ennen opintojani työskentelin kaksi vuotta kehitysvammaisten parissa eri yksiköissä, osan ajasta tiiminvetäjänä. Vastasin palavereista ja vuosikellon mukaisista tehtävistä, toteutin lääkehoitoa ja tein moniammatillista yhteistyötä. Sieltä mukanani kulkee tarkkuus, vastuunkanto ja taito toimia hyvin erilaisten ihmisten kanssa – ja ne ovat yllättävän lähellä sitä, mitä datatyö vaatii.

Olen utelias kuin kissa, tykkään brainstormata ja etenen sitkeästi – mutta en laput silmillä. Osaan tarkastella asioita kriittisesti ja vaihtaa suuntaa, jos jokin ei toimi. Jos en osaa jotakin, otan selvää. Jos näen mahiksen, sanon sen ääneen – vaikka ajatus vaatisikin vielä kierroksen tai pari. Missioni onkin oppia joka päivä jotain, mitä en tiennyt vielä eilen. Kunnianhimoinen tavoite siltä kantilta, että tämä ala tuntuu muuttuvan joka tiistai.

<div class="profile-links" markdown>

[GitHub](https://github.com/tuuturulla) · [sara.pyky@gmail.com](mailto:sara.pyky@gmail.com)
</div>

## Projektit

<div class="article-card" markdown>

<p class="card-meta">Oma projekti · Web-ohjelmoinnin lopputyö</p>

### Menomittari – mihin rahat menevät?

Suunnittelin full stack -sovelluksen, joka vertaa omaa kulutusta kahden ajanjakson välillä. Talousdata pysyy käyttäjän selaimessa. Toteutin sen koodausagentin kanssa: minä tein suunnitelmat ja säännöt ja testasin työn, agentti kirjoitti koodin.

<p class="card-tags">React · TypeScript · FastAPI · SQLite · IndexedDB · Docker · Vitest · pytest</p>

[Lue projektista →](projektit/menomittari.md){ .card-link }

</div>

<div class="article-card" markdown>

<p class="card-meta">Kurssityön pohjalta · Data-alustat</p>

### JunaJumis – asemat, joissa aikataulu käy kahvilla

Rakensin dataputken, joka hakee junadatan Fintrafficin rajapinnasta, mallintaa sen dbt:llä DuckDB:hen ja näyttää dashboardilla, millä asemilla junat myöhästyvät eniten. Koko ketju päivittyy yhdellä komennolla.

<p class="card-tags">Python · REST API · dbt · DuckDB · SQL · Evidence · Docker</p>

[Lue projektista →](projektit/junajumis.md){ .card-link }

</div>

<div class="article-card" markdown>

<p class="card-meta">Tiimiprojekti · yrityksen aineisto</p>

### Kaivonkannet konenäöllä – YOLO vai Faster R-CNN?

Koulutimme ja vertailimme kohteentunnistusmalleja yli 10 000 kuvan aineistolla. Olin Product Owner, suunnittelin annotointiprosessin, testasin esikäsittelyn vaikutusta ja vastasin Faster R-CNN -mallista.

<p class="card-tags">Python · PyTorch · YOLO · Faster R-CNN · Label Studio · PostgreSQL · Scrum</p>

[Lue projektista →](projektit/kaivonkannet.md){ .card-link }

</div>

<div class="article-card" markdown>

<p class="card-meta">Tiimiprojekti · neljä opiskelijaa</p>

### Kickstarter-kampanjan onnistumisen ennustaja

Ennustaa, kuinka todennäköisesti Kickstarter-projekti onnistuu. Sovellus antaa myös tekoälyn tuottamia parannusehdotuksia sen perusteella, mitkä piirteet vaikuttivat mallin ennusteeseen. Olin tiimissä Project Owner ja kehittäjä ja mukana lähes kaikissa vaiheissa: datan esikäsittelyssä, feature engineeringissä, mallien vertailussa ja Streamlit-sovelluksessa.

<p class="card-tags">Python · scikit-learn · feature engineering · Streamlit · Perplexity API · Scrum</p>

[Koodi GitHubissa →](https://github.com/tuuturulla/ml-kickstarter-project){ .card-link }

</div>

## Kirjoitukset

Kirjoitukset pohjautuvat pääosin kurssitöihini, ja jokaisen lopussa kerrotaan sen tausta. Hesh'thar on oma projektini.

<div class="article-card" markdown>

<p class="card-meta">Kurssityön pohjalta · Syväoppiminen I</p>

### Monimutkaisempi malli ei ole automaattisesti parempi

Vertasin neuroverkkoja ja valmiita malleja yksinkertaisempiin ratkaisuihin. Useassa kokeilussa yksinkertainen malli ylsi lähes samaan, ja vahvakin malli kompastui dataan.

<p class="card-tags">PyTorch · siirtovaikutus · Naive Bayes · RoBERTa · MobileNet · InceptionV3</p>

[Lue kirjoitus →](kirjoitukset/monimutkaisempi-malli.md){ .card-link }

</div>

<div class="article-card" markdown>

<p class="card-meta">Kurssityön pohjalta · Syväoppiminen I</p>

### Sanoista numeroiksi – miten konteksti kulkee mukana?

Testasin spaCyn suomenkielistä mallia sekä kahta käännösmallia ja löysin käännöksen, joka kuulostaa sujuvalta mutta kääntää merkityksen päinvastaiseksi.

<p class="card-tags">Python · spaCy · sanavektorit · LSTM · GRU · Transformer</p>

[Lue kirjoitus →](kirjoitukset/kieli-ja-konteksti.md){ .card-link }

</div>

<div class="article-card" markdown>

<p class="card-meta">Oma projekti</p>

### Kun malli vaihtuu: hahmon jatkuvuus generatiivisessa kuvassa

Tein generatiivisen lyhytvideon, jossa sama hahmo kulkee kohtauksesta toiseen. Myöhemmin vertasin, miten uudempi kuvamalli säilyttää hahmon ja millaisia uusia virheitä se tuo mukanaan.

<p class="card-tags">Nano Banana 2 · Sora · Kling · DaVinci Resolve · promptaus</p>

[Lue kirjoitus →](kirjoitukset/genai-hahmon-jatkuvuus.md){ .card-link }

</div>

<div class="article-card" markdown>

<p class="card-meta">Kurssityön pohjalta · Syväoppiminen I</p>

### Aikasarjadata tarvitsee ehjän aikajanan

Paikkasin aukkoisen aikajanan ja jaoin datan niin, ettei ennustemalli pääse kurkkimaan tulevaisuuteen.

<p class="card-tags">Python · Pandas · aikasarjat · data leakage · neuroverkot</p>

[Lue kirjoitus →](kirjoitukset/aikasarjojen-aikajarjestys.md){ .card-link }

</div>

<div class="article-card" markdown>

<p class="card-meta">Kurssiesseiden pohjalta</p>

### Milloin datasta tulee tietoa – ja arvoa?

Miten datasta tulee päätöksiä, mitä datastrategia käytännössä tarkoittaa ja miten datasta saatavaa hyötyä voi mitata.

<p class="card-tags">Datastrategia · datan laatu · tietosuoja · liiketoiminta</p>

[Lue kirjoitus →](kirjoitukset/datasta-tietoa-ja-arvoa.md){ .card-link }

</div>
