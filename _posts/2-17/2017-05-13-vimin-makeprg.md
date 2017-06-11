---
layout: post
title:  "GT suosittelee: Vimin makeprg"
date:   2017-05-03 12:00:00 +0200
categories: tekniikka
author: Konsta Hölttä
ingress: Mitään isompaa softaa ei käännetä ihan noin vaan eikä kääntäjäulinan seasta siirrytä koodiin ihan noin vaan. Vim ei ole IDE mutta komentorivillä asti ei silti tarvitse käydä aina komentamassa make ja kääntäjävirheetkin saa mapattua editoitavaan tekstiin. Sama toimii toki muuhunkin kuin kääntämiseen.
magazine: 2/2017
print_order: 3
---

Muiden testausartsujen lomaan sopii hömppää tälläisestä lempityökalusta kuha puhutaan testaamisesta. Vimhän on pelkkä tekstieditori eikä mikään integroitu kehitysympäristö (IDE). Testit tai kääntäjän tai mitä vaan voi silti ajaa Vimin sisältä ja saada tulokset editoriin sekaan. Homma ei tavallaan suinkaan ole integroitu koska kyseessä on jokin ulkoinen ohjelma jonka editori ajaa.

Vimin [quickfix-optio][1] on manuaalin mukaan ammennettu Manx's Aztec C compiler -nimisestä härpättimestä Amigassa (AMIGAAAAA!). Quickfix-listaan saa jutut, joista ulkoinen käännös- tai testisofta nillittää. Listaa voi tarkastella ja sen sisällön perusteella pääsee paikasta toiseen vikoja korjaamaan.

Vimistä löytyy Quickfix list ja Location list. Näiden ero on se, että [quickfix on globaali ja location on lokaali][2]. Asiaan. Kun komentaa `:make` (Viljami jätetään kotitehtäväksi), niin Vim ajaa komennon, jonka voi konffata [makeprg-konffin takaa][3]. Vim parsii komennon ulostulon talteen. Tulokset -- esimerkiksi kääntäjän ulisemat rivinumerot -- saa näkyville komentamalla `:copen`, ja jos ajaa vaikka testityökalua joka kertoo mikä kohta koodista on räsä, niin saman voi parsia listaan.

Kun komennon ajaa, niin Vim parsii. Tämä ei ole taikaa, vaan mekaaninen operaatio, joka säädetään [errorformaattistringillä][4]. Manuaalista löytyy ohjeet mm. LaTeXia ja muita vakiintuneita työkaluja varten; modernimpia web-pöhinäkieliä yms. varten täytyy varmaan säätää itse tai googlata jonkun muun säädöt.

GT suosittelee. Ongelmallisten koodirivien kaivaminen kääntäjän/lintin/testityökalun dumppaamasta wall of textistä on epämukavaa eikä sen täsmääminen tiedostoon ja riviin ja sarakkeeseen ole lainkaan ergonomista. Make vaan ja sitten voi näppäillä `:cn` ja `:cp` (pidemmin `:cnext` ja `:cprevious`) navigoidakseen seuraavaan ja edelliseen virheeseen. Kun kaikki on luultavasti korjattu, niin makea uudelleen kunnes kaikki on kunnossa.

Terveisiä muuten Artolta (Nexton). "Testaaminen on kuin käpylehmä"

[1]: http://vimdoc.sourceforge.net/htmldoc/quickfix.html
[2]: http://stackoverflow.com/questions/20933836/what-is-the-difference-between-location-list-and-quickfix-list-in-vim
[3]: http://vimdoc.sourceforge.net/htmldoc/options.html#%27makeprg%27
[4]: http://vimdoc.sourceforge.net/htmldoc/quickfix.html#errorformat
