---
layout: post
title:  Kaikki tiedostot Vimiin
date:   2017-12-02 13:00:00 +0200
categories: tekniikka
author: Konsta Hölttä
ingress: Monen avonaisen tiedoston tuki löytyy jokaisesta itseään kunnioittavasta tekstieditorista. Vimissä tiedostoja voi editoida mm. vierekkäin ja allekkain kuin ikkunamanagerissa ikkunoita, ja eri vierekkäin-allekkain-näkymiä saa useita kuin ikkunamanagerissa virtuaalityöpöytiä.
magazine: 6/2017
image: /static/2017-05/gits.jpg
print_order: 3
---
Toimitus huomasi, että vaikka bufferien sulava hallinta on tekstieditorin tärkeimpiä ominaisuuksia, Vim-artikkelisarja on hädin tuskin sivunnut usean bufferin (KAIKKIEN) pyörittelyä. IDE-rätkyttimien yhtenä ongelmana on perinteisesti se, että ruudulta vie tilaa kaikenlainen muu ja koodieditoriin mahtuu vain yksi tiedosto kerralla. Toisaalta modernit IDE-rätkyttimet ovat oppineet antamaan tilaa sille tärkeimmälle, eli koodille, jota kirjoittaa sitten yhtä täysii kuin oheisen Ghost in the Shell -kuvan tyyppi.

Vimissähän buffereilla voi tehdä vähän kaikkea. Yleensä bufferi vastaa muokattavaa tiedostoa, mutta voipa olla myös nimettömiä buffereita, lisäosien omistamia taikabuffereita, isoisien omistamia buffereita (ei oikeesti) tai help-ympäristön bufferi (lol apua). Kannattaa tosin varoa käyttämästä turhan innokkaasti kaiken maailman plugaribuffereita, jotka kuluttavat kallisarvoisia pikseleitä aivan kuin turhanpäiväiset lisäpaneelit IDEissä.

Tyypillisin ruutuleiska lienee se yksi koko näytön ikkuna. Ikkuna on Vim-termistössä näkymä bufferiin; sama bufferi voi olla useammassa ikkunassa. Toiseksi tyypillisin lienee näkymä, jossa ruutu on jaettu kahteen tai kolmeen vierekkäiseen palstaan. Laajakuvaruudulle mahtuu hyvin kolmekin kernelkoodia vierekkäin, eli yhteensä 240 editoitavaa merkkiä ja vähän ikkunakehyksiä. Sitten niitä voi vielä katkoa päällekkäin moneen osaan. Näkymän ei toki tarvitse olla symmetrinen. Käyttäjäoppaassa on kaikesta tästä kattavat [ascii-art-esimerkit][1].

Ruutuleiskaa sopii ajatella vaikka puurakenteena, samaan tapaan kuin Lehtisen graffakursseilta tutut BSP-hakurakenteet ([binary space partitioning][2]). Joka splitti jakaa ruudun kahteen pienempään pseudoruutuun, joita voi jakaa edelleen. Pikseleitä, pikseleitä kaikkialla.

Ikkunoiden [referenssimanuaali][3] kattaa melko tarkkaan kaiken sen, miten ikkunoita käytetään karmeista ja kahvoista alkaen. Esitellään muutamat nappulat ja vähän käyttötarkoituksia. Ensinnä kannattaa `:set hidden`, ellei ole jo. Se manuska kertoo miksi. Ikkunoita voi luoda ja sulkea ja pyöritellä ja niissä voi liikkua.

Ikkunakomentoja ajetaan komentoriviltä tai `CTRL-W`:llä alkavina oikoteinä tai ratilla ja polkimilla. `:sp` tai `C-W s` splittaa ruudun päällekkäin. `:vsp` tai `C-W v` splittaa vierekkäin. `:q` tai `C-W q` poistuu. Näillä tekee jo paljon. Sitten `C-W w` ja `C-W W` menevät seuraavaan ja edelliseen ikkunaan, ja `C-W hjkl` vaihtaa kursorin vasemmalla, alhaalla, ylhäällä tai oikealla olevaan ikkunaan. Ikkunoita voi myös siirrellä paikasta toiseen; esimerkiksi aktiivista ikkunaa saa kutiteltua eri suuntiin: `C-W HJKL` eli shift pohjassa. Kolmesta vierekkäisestä `C-W K` veisi aktiivisen ylimmäksi ja kokoleveäksi siten, että sen alla on loput kaksi vierekkäin. Kiintoisa ikkunafeature on “alternate buffer” eli `#`, lisää googlaamalla sillä sen nyanssit eivät mahdu tähän. `C-^` tai `C-6` hyppii alternate buffien välillä alt-tabin tyyliin. Kaikki (KAIKKI!) bufferit löytää `:ls`. Paitsi unlistedit.

Toki samassa ikkunassakin pääsee bufferista toiseen. Jonkin tietyn bufferin saa auki komentamalla `:buf <pätkä tiedostonimeä><tab>`. `:e <tiedostonimi>` käy myös. `:bnext` ja `:bprev` kelaavat buffereiden listaa. VHS-nauhurin lukupää pitää välillä puhdistaa.

![](/static/2017-05/windowdiagram.png)
*Suurpiirteinen UI-hahmotelma ([Wikipedia / Ahp378](https://commons.wikimedia.org/wiki/File:WallPanelDiagram.svg))*

Kaikkeen tavalliseen editointiin yleensä auttaa edes kaksi vierekkäistä ikkunaa, ihan kuin toinen monitori tietokoneeseen tehostaa työskentelyä. Useamman hyödyntäminen hakee jo hieman harjaantuneisuutta. Jos vaikka pienessä projektissa eri tiedostot pitää tietyissä kohtaa ruudun splittirakennetta, niihin saa keksittyä muistisääntöjä, tyyliin “piilotin vihreän vaapukkamehun siihen vasemmanpuoleiseen pakastimeen”. Vim osaa myös sitoa tiedostojen scrollailun yhteen ja näyttää eroavaisuudetkin, mille on oma pikakäynnistinkin: `vimdiff mehu limu` näyttää mehun ja limun erot vierekkäin ja kun yhtä scrollailee niin toinen seuraa perässä. Three-way diff se vasta luksusta onkin Gitin mergetoolina.

Toisinaan täytyy editoida vaikka kahta eri codebasea joissa on vaikka eri sisennysasetukset käytössä. Jos ei ole vielä konffannut niitä säätymään itsestään, niin esimerkiksi `:setlocal ts=4 sw=4 sts=4 noet` ei vuoda muihin buffereihin kuin aktiiviseen. Tätä aihetta sivuaa `:bufdo` ja `:windo`, jotka suorittavat oheisen komennon joka bufferille tai joka ikkunalle.

Monesta codebasesta puheenollen, `:tabe` avaa nykyisen tiedoston uuteen tabiin; editorin yläreunaan ilmestyy tabirivi, jossa siirrytään eestaas näpyttämällä `gt` ja `gT`. Muutama eri codebase tai eri projektin osa menee kivasti eri tabeihin; paiskaa yhteen vaikka testit ja toiseen varsinainen softa (AINA testejä yhtä paljon kuin Länsimetrossa). Uusi tabi on myös oivallinen scratch space (lautanen) jollekin tiedostolle (pekonille) joka pitää nähdä monen splitin (ruokapöydän) seasta koko ruudussa sotkematta layouttia. `C-W s C-W T`. Nälkä. Joillekin näistä esitellyistä nappulakomboista on suositeltavaa bindailla jonkinlaiset oikotiet, ettei ctrl-sormi taivu mutkalle.

Ikkunaspliteistä päästäisiin aikamoiseen koodifilosofointiin, jos kehtaisi. Ehkä Stillistunnelmissa sitten paremmin. Allekirjoittanut analysoi silti mielellään sitä, millainen maailma olisi, jos koodiympäristöä (eli editoreita, kääntäjiä, debuggereita yms.) ei kiinnostaisi erilliset tiedostot tai ylipäätään funktioiden sijainnit. Mitä jos koodieditori olisi vaikka kaksiulotteinen VR-kangas, jossa voisi pyöritellä koodinpätkiä ylös ja alas, vasemmalle ja oikealle ja vaikka vielä eri tasoille piiloon “kankaan” “taakse”? Jonkinmoinen puhtauteen pyrkivä Lisp-koodihässäkkä voisikin olla kova kandidaatti tälläisen protoiluun.

Ei hävetä paljastaa, että ammoisina aikoina tuli ohjelmoitua Quick- ja Visual Basic -ympäristöissä, joissa sai halutessaan yhden funktion näkymään kerralla. Jo noin alkeelliset kielet mursivat sitä käsitettä, että asioiden esittelyjärjestyksellä olisi jotain väliä. Editoidaan koodia eikä tiedostoja. Tiedostoihin jako toki helpottaa ihmisiä hahmottamaan ja käsittelemään eri osakokonaisuuksia (ja mahdollistaa bufdo-tyyppisten ominaisuuksien käytön).

[1]: <http://vimdoc.sourceforge.net/htmldoc/usr_08.html>
[2]: <https://en.wikipedia.org/wiki/Binary_space_partitioning>
[3]: <http://vimdoc.sourceforge.net/htmldoc/windows.html>
