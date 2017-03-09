# GT-web

GT-web on Automaatio- ja systeemitekniikan killan kiltalehden, Kultaisen Tomaatin verkkoversio.
GT-webiä ylläpitää Kultainen toimitus.

## Yleistä

GT-web käyttää pohjanaan Jekylliä, joka on staattisten sivustojen generoimiseen tarkoitettu Ruby-kirjasto.

## Kirjoittaminen

Artikkelit lisätään `_posts` kansioon. Artikkelit kirjoitetaan Markdown-syntaksilla. Alla esimerkki artikkelista.

```
---
layout: post
title:  "ASD aaaaa"
date:   2016-12-31 23:39:46 +0200
categories: pääkirjoitus
author: Juuso Mikkonen
ingress: Asd asd asd.
magazine: 1/2017
print_order: 6
image: "/static/2017-01/kuva.jpg"
caption: Kuvassa Nikke Knatterton
---

Lorem ipsum dolor sit amet, consectetur adipisicing elit.
Nulla repudiandae veniam, omnis animi voluptatibus ea fuga odit vero quam cupiditate, ex, id ipsum itaque commodi culpa assumenda libero.
Dolorum, eveniet!
```

Ennen leipätekstiä täytyy lisätä muutama metatieto. Suuren osan voi copy-pasteta. Tärkeimmät ovat:

- `layout`: tämän voi aina asettaa arvoon `post`
- `title`: artikkelin otsikko
- `date`: artikkelin julkaisuaika. Päivämäärä näkyy saitilla, kellonaika ei. Aika ei saa olla tulevaisuudessa, vaan pitää olla kulunut julkaisuhetkellä.
- `categories`: aseta *yksi* kategoria. nämä kirjoitetaan aina pienellä. Välilyönnit korvataan alaviivalla, esim GT testaa -juttuihin tulee `gt_testaa`.
- `author`: kirjoittajan nimi
- `ingress`: artikkelin ingressi. Ei pakollinen, mutta suositeltava. Noin yhden - kolmen lauseen mittainen.
- `image`: jos jutulla on pääkuva, asetetaan polku tähän. Kuvat menevät `/static/YYYY-NN` kansioon.
- `caption`: jos jutulla on pääkuva, sille voidaan asettaa kuvateksti. Yksi - neljä lausetta.
- `magazine`: jos juttu on julkaistu lehdessä, tähän tulee lehden numero muodossa `N/YYYY` jossa N on lehden numero
- `print_order`: lehdessä julkaistun jutun järjestysnumero. olemassa siksi, että jutut ovat uusin lehti -sivulla oikeassa järjestyksessä
- `featured`: jos juttu halutaan nostaa etusivun kärkeen, asetetaan tähän riittävän suuri arvo

Pahoittelut artikkelien latojalle parametrien suuresta määrästä, tää ei oo mikään wordpress t. webiukko
