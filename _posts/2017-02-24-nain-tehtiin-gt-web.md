---
layout: post
title:  "Näin tehtiin GT web"
date:   2017-02-23 11:30:00 +0200
categories: tekniikka
magazine: 1/2017
print_order: 5
author: Juuso Mikkonen
ingress: GT web on nyt täällä! Ensimmäistä kertaa kiltalehden pitkässä historiassa koko sisältö on saatavilla paitsi printtinä ja pdf, myös natiivina webinä. Miten GT web tehtiin? Mitä ei tehty? Mitä on luvassa tulevaisuudessa?
---

Koko GT webin kehitys tapahtui äärimmäisen nopealla kehityssyklillä. Työkaluksi valittiin luotettava ja laajasti käytetty staattisten sivujen generointiin tarkoitettu Ruby-pohjainen Jekyll. Jekyllin valintaan vaikutti staattisten sivujen yksinkertaisuus, alustan laajennettavuus ja aiempi kokemus kirjaston käytöstä. Perinteinen Wordpress ei tullut kysymykseenkään – pelkkä nimi on monelle toimituksessa kirosana.

Jekyll generoi staattisia sivuja markdown-muotoillusta sisällöstä ja HTML-templateista. Sellaisenaan Jekyll soveltuu hyvin blogien ja muiden artikkelipohjaisten sivujen luomiseen. Sisällön ja templatejen lisäksi tarvitaan tyylit sekä mahdollinen Javascript. Jekyll tarjoaa valmiita teemoja Rubyn gem-paketteina, mutta todelliset web-artesaatit kirjottavat toki layoutinsa käsin. Jekyll kääntää tyylit oletuksena Sass-formaatista CSS:ksi selainta varten. 

Lisätoiminnallisuuksia sivustoon voidaan luoda Ruby-pohjaisilla plugineilla, joilla on mahdollista esimerkiksi luoda sivustokarttoja tai lisätä tuki uusille syntakseille (esimerkiksi Coffeescript, LESS).

GT-webiä varten tyylit kirjoitettiin valtaosin tyhjän päälle. Valtaosa Jekyllin valmiista scss:stä roskitettiin tai jätettiin vain käyttämättä. Tietyt valmiit ominaisuudet, kuten syntaksin korostus otettiin sellaisenaan käyttöön. Sen sijaan esimerkiksi typografia ja responsiivisuuden käsittely laitettiin uusiksi. Nopean kehitystahdin vuoksi tyylien ja templatejen rakenne on melkoisen karkea – esimerkiksi HTML-luokat ovat hassu sekoitus BEM-notaatiota ja Jekyllin valmista nimeämiskonventiota. Näitä on syytä yhtenäistää tulevaisuudessa.

Kauden tärkeimpiä tavoitteita webin osalta ovat saitin kehitys monipuolisemmaksi sekä projektin jatkuvuuden varmistaminen. Lisäksi tarkoituksena on luoda nykyistä parempi arkisto vanhoista lehdistä suoraan osaksi nykyistä webiä.

Kuten itse lehti, GT web on jatkuvasti kehittyvä ja muotoaan hakeva teos. Kehitys pyrkii olemaan mahdollisimman avointa – koko sivuston lähdekoodi on avoin ja  kiltalaisten kontribuutiot ja palaute otetaan mielellään vastaan. Iloisia lukuhetkiä!


GT web <http://gt.as.fi>

GT-webin lähdekoodi <https://github.com/juusaw/gt-web>
