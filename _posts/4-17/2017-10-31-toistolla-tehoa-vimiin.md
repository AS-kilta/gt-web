---
layout: post
title:  "Toistolla tehoa Vimiin"
date:   2017-10-31 19:00:00 +0200
categories: tekniikka
author: Konsta Hölttä
ingress: Monista editoreista löytyy sellainen moderni multicursor-nimeä kantava ominaisuus, jolla voi toistaa saman editoinnin moneen kohtaan. Missä Vimin visual block -tila ei riitä eivätkä purkkaskriptit maistu, voi käyttää makroja, eli livenä talletettavia komentosarjoja. Makrot tietty sopivat vähän kaikenlaiseen muuhunkin, ja multicursorhömppäeditoreista yleensä löytyy nekin. Jos editorissa ei ole makroja, niin se soveltuu korkeintaan ostoslistan kirjoittamiseen.
magazine: 5/2017
image: /static/2017-04/kermit.gif
---
Kunhan sen Vimin kieliopin sisäistää, niin makroista tulee harvinaisen luontevia käyttää, sillä Vimissä liikkuminen on niin mahtavaa. Pelkät liikeet ja yksinkertaiset muokkaukset eivät toki ole ainoita joita makroon saa sekaan. Normalmode-äksön tallentuu, ja suunnilleen kaikki mitä näppiksellä voi tehdä.

Makrotalletustilaan mennään napilla `q` ja rekisterinimi, talletus lopetetaan taas `q`:lla ja makro toistetaan näppäilemällä `@` ja rekisterinimi. Nimeksi valitaan yleensä joku muistisääntökirjain.

Visual-modeen viitattiin jo kielioppiartsussa lehden numerossa yksi. Vimillä voi kätevästi vaikka piirtää ascii-art-laatikoita visual block -makroilla. `qa<C-v>jjllr.q` (`<C-v>` tarkoittaa ctrl-v:n painamista) tallettaa *a*-rekisteriin makron, joka korvaa tekstin seasta kursorista alkaen 3x3-kokoisen alueen pisteillä.

Ai niin, rekistereitä ei olla vissiin vielä käyty läpi eikä siten copypasteakaan. No, copypaste on niin perusjuttu (vaikkakin erittäin vaarallisten bugien lähde) että noheva lukija on varmaan ehtinyt googlata. Silti, vimissä on [vino pino][1] *rekistereitä* eli mm. copypasteen ja makroihin käytettäviä buffereita joilla voi tehdä [vähän kaikkea][2]. Siinä missä `y` kopioi ja `p` liittää *oletusrekisterin* kautta, `"ky` ja `"kp` kopioi ja liittää *k*-rekisterin kautta.

Rekistereitä on monta. On suositeltavaa opetella ottamaan niistä hyöty irti; monesti eri rekistereissä sopii pitää useita koodinpätkiä tai makroja, ja rekkarikirjaimen voi valita jonkun tapauskohtaisen muistisäännön mukaan.

Normalmoden `p` on joskus kurja jos kesken editoimisen pitää pasteta. Ei hätää, insertmoodissakin voi pastettaa minkä vaan rekkarin: `<C-r><rekisterimerkki>`. Joissain erikoistapauksissa `<C-r>` pitää syöttää [kahdesti][3]. Tekstiä korvatessa taas oletusrekisteri voi ärsyttää. Ei vieläkään hätää: siinä missä oletusrekisteri helposti hukkuu, rekisteri *0* on [vain yankeille][4].

Koska makrot ovat ihan tavallista tekstiä Vimin rekistereissä joissa voi copypastatakin kamaa, tuon äsken mainitun makron saisi vaikka pastettua bufferiin sanomalla `"ap`, jolloin bufferissa näkyisi `^Vjjkkr.` (`^V` eli ctrl-v-merkki on yleensä omalla erikoismerkkivärillään). Samaten makroa voi muokata ihan editoimalla bufferia (esim. lisäämällä yhden `j`:n tähän esimerkkiin niin että alueen korkeus kasvaa yhdellä) valitsemalla haluamansa makrosisällön ja näppäilemällä `"ay`.

Vielä parempi tapa editoida makroa on `:let @a='<C-r>a'`. Joo, kuten insert-moodissa, myös komentorivillä `<C-r>`:llä voi pastettaa rekisterin. Ennen enterin painamista voi vielä muokata sisältöä; näin makron voi uudelleenmääritellä.

*Rekursiivinen* makro kutsuu itseään. Sanotaan vaikka, että tiedostossa on tekstivirkkeitä, joiden ensimmäinen sana pitää saada isoiksi kirjaimiksi ja lopusta puuttuu piste. Yhden tälläisen muokkauksen suorittaa vaikka silmät sidottuna näppäilemällä `gUw$a.<ESC>j0` eli eka sana isoksi, rivin loppuun, piste perään ja seuraavalle riville alkuun. [toim. huom. escape (joka C-r-rendautuu `^[` harmaalla värillä) oli unohtunut lehden painetusta versiosta]

Talletetaan makro rekisteriin *w*. Naputellaan ensin `qwq`, joka tyhjentää tämän rekisterin menemällä talletustilaan ja heti poistumalla. Sitten uudestaan `qw`, tehdään toistettavat liikkeet ja naputellaan `@w`, joka ajaa makron *w* joka tällä hetkellä on tyhjä mutta makron toistaminen tallettuu tähän rekisteriin. Lopulta poistutaan painamalla `q`. Nyt makron voi ajaa naputtelemalla `@w`, ja magiaa tapahtuu. Intuitiivisesti rekursio päättyy, kun makron suoritus ei enää onnistu. Näin esim. alaspäin liikkuva makro pysähtyy viimeiselle riville sen sijaan, että vetäisi editorin tukkoon tai tulisi näytön alareunasta läpi ja pöydälle.

Toinen tapa ajaa makro monta kertaa on ihan tavallinen Vimin tapa suorittaa jokin monta kertaa, eli numero vaan alkuun: `42@w` ja jos numero on joku tolkuttoman iso joka kattaa koko tiedoston, niin rekursiivisen makron kikkaa ei tarvitse muistaa.

Komentorivimoodissa makron m voi ajaa komentamalla `:normal @m`, eli esimerkiksi visual-maalatuille riveille `:'<,'> normal @m` (visual-moodissahan nuo mark-määreet syöttyvät itsestään).

Makrojen juju on se, että editoitavan datan täytyy olla säännönmukaista. Siispä juuri Vimin kielioppi tanssii niiden kanssa luonnollisesti, ja itseasiassa regexpit eli suomeksi säännölliset lausekkeet menevät makroihin oivallisesti. Se, joka esittää pikkASjouluissa toimitukselle hienoimman hyödyllisen makron, saa yllätyspalkinnon. Tästä liikeelle: `:help recording` ja `:help registers`.

[1]: http://vimdoc.sourceforge.net/htmldoc/change.html#registers
[2]: http://of-vim-and-vigor.blogspot.fi/2012/01/rap-on-vims-registers.html
[3]: http://vimdoc.sourceforge.net/htmldoc/insert.html#i_CTRL-R_CTRL-R
[4]: http://vimcasts.org/episodes/meet-the-yank-register/
