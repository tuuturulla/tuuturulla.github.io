# Sara Pyky – Portfolio

Lähdekoodi portfoliolleni osoitteessa **https://tuuturulla.github.io**.

Portfolio kokoaa projektejani ja kirjoituksiani datasta, koneoppimisesta ja tekoälystä. Sivusto on tehty [MkDocs Materialilla](https://squidfunk.github.io/mkdocs-material/).

*Source for my portfolio at https://tuuturulla.github.io: data, machine learning and AI projects and writing. The site is in Finnish, with an English front page.*

## Rakenne

```text
docs/
  index.md          Etusivu
  en/index.md       Englanninkielinen etusivu
  projektit/        Projektisivut
  kirjoitukset/     Kirjoitukset
  assets/           Kuvat
  stylesheets/      Oma tyyli
mkdocs.yml          Sivuston asetukset ja valikko
```

## Esikatselu ja julkaisu

```bash
uvx --with mkdocs-material mkdocs serve
uvx --with mkdocs-material mkdocs gh-deploy
```

`serve` avaa esikatselun osoitteeseen `http://127.0.0.1:8000`. `gh-deploy` rakentaa sivuston ja julkaisee sen `gh-pages`-haaraan.
