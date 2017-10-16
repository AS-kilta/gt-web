---
layout: post
title:  "Vimin identifier-hakutyökaluja"
date:   2017-09-20 12:00:00 +0200
categories: tekniikka
author: Konsta Hölttä
ingress: Koodia editoitaessa yksi IDE-härpättimien keskeisistä ominaisuuksista on funktioiden esittelyihin tai määrittelyihin hyppiminen pikanapeilla tai hiirellä klikkailemalla. Ehkäpä jopa voi listata kaikki paikat, joista funktiota X kutsutaan. Myös yleinen tekstihaku saattaa olla jotenkin kätevöitetty. Vim osaa tottahan toki nämä kaikki.
magazine: 4/2017
print_order: 12
---

Viime lehdessä käsiteltiin featuret nimiltään *makeprg*, *quickfix* ja *locationlist*, joilla pääsee navigoimaan ympäriinsä vaikka käännetyn softan seassa. Tai siis monestihan devattava softa ei heti käänny vaan siitä pitää ensin korjata bugit, missä se makeprg auttaakin. GT esittelee muutaman muunkin tavan navigoida koodissa ympäriinsä.

Jo siellä [Quickfixin ohjeessa][1] mainitaan `:vimgrep` heti esittelykappaleen jälkeen. Grephän on komentorivityökalu monimutkaistenkin säännönmukaisten (eng. pattern) tekstinpätkien etsimiseen tiedostoista. Vimgrep (lyhennettynä hassusti pelkästään `:vim`) on editoriin sisäänrakennettu greppi, jolla on vimille tyypillisesti lukuisia flageja ja eri versioita. Komentamalla `:vimgrep /frobnicate/ *.c` quickfix-lista täyttyy nykyhakemistossa olevien .c-päätteisten tiedostojen paikoista, joissa esiintyy teksti frobnicate. Listalla voi sitten navigoida niitä läpi sen sijaan, että etsisi jotenkin käsin. `:lvimgrep` käyttäytyy vastaavasti, mutta täyttää aktiivisen ikkunan location listin, ja muita löytyy manuskasta. (Hakemistoihin voi rekursoida näppärästi [*starstar-wildcardilla*][2] tyyliin `**/*.c`.)

Vimgrep toimii oivasti mihin vaan patterneihin, joissa voi olla vaikka välilyöntejä tai mitä vaan regexpejä kuten tavallisesti Vimissä hakua käyttäessä. Funktioiden määrittelyihin hyppiminen on kuitenkin niin yleisesti hyödylliseksi hyväksytty ominaisuus, että sitä tuetaan tavallaan haun erikoistapauksena. Komentorivihörhöt tuntevat sellaiset funktiokaltaiset identifierit *tageina*, joiden parsimiseen sorsasta on ihan omia [työkaluita][3] ja jotka tuottavat indeksitiedoston ("tags file") jota Vim [osaa lukea][4]. Yleensä tämä hoituu niinkin helposti, että ajetaan komentoriviltä `ctags -R .`, konffataan Vimiin että `:set tags=tags;/` ja ei kun navigoimaan. Ctagsin ajamisen voi vielä automatisoida tapahtumaan vaikka bufferintallennuksen jälkeen. Kursori funktion nimen kohdalle ja `Ctrl-]`, niin löytyy tagi. Kaikki listataan komentamalla `g]`, jos vaikka samaa on monella nimellä tai jos Vim ei arvaa C-ohjelmasta että haluttiinko tässä nyt declarationiin vai definitioniin. Jos kursorin alla ei ole mitään erityistä, voi hakea asian myös komentamalla `:tag asia`. Myöskin editorin voi käynnistää loitsulla `vim -t asia` jolloin asia löytyy heti.

Mikään ei kuitenkaan ole täydellistä. Ctagsissakin voi ainakin defaulteilla törmätä tilanteeseen, jossa isoa codebasea (vaikka Linux-kerneliä) selatessa löytyy oikea määrittely, ja samaan syssyyn kymmeniä tai peräti satoja harhaosumia. Haetaan vaikka `struct device`ä koska sen sisältö kiinnostaa, mutta C:n nimiavaruus ja kernelin kätevät nimikäytännöt aiheuttavat sen, että Vim löytää suunnilleen **375** osumaa, joista useimmat `struct device *device;` sillä ctags ja Vimin normihaku eivät erottele että haluttiinko funktiota, struktia, muuttujaa, tms. Pienellä parsimisella tästäkin pääsee toki yli. Parsiminen jätetään kotitehtäväksi.

Entä sitten kun pitää löytää funktioita toisinpäin, eli kaikki paikat jotka kutsuvat funktiota F? Tässä yleensä riittää ihan helposti pelkkä `:vimgrep funktionimi`, mikä toki löytää myös funktion itsensä sekä saman tekstin vaikkapa kommenttien seasta. Hieman älyllisempi työkalu on [*cscope*][5]. Cscope on vähän kinkkisempi käyttää Vimissä vaikkakin tuettu, mutta sepä löytää myös mm. paikat jotka kutsuvat funktiota F, funktiot joita funktio F kutsuu, ja muuta mukavaa. Cscopekin pitää ajaa etukäteen ja konffata Vimiin (`:cscope add cscope.out`) jotta saadaan rakennettua koodista nopeasti haettava indeksi. Sitten `g]`-kaltainen haku onnistuu komentamalla `:cs find g foobar`, ja foobaria kutsuvat paikat löytyy komentamalla `:cs find c foobar`. Nämä kannattaa mapata johonkin lempinäppiyhdistelmään, vaikka kätsysti `:nnoremap <Leader>c :cs find c <C-R>=expand("<cword>")<CR><CR>`.

Ctagsin ja cscopen kanssa on häx-ympäristötyyliin rutkasti säädettävää (vrt. täysautomaattinen pelle-GUI-IDE), eikä mikään ole ihan plug-and-play. GT suosittelee tutustumaan ja kehottaa lukemaan manuaaleja.

[1]: http://vimdoc.sourceforge.net/htmldoc/quickfix.html
[2]: http://vimdoc.sourceforge.net/htmldoc/editing.html#starstar-wildcard
[3]: https://en.wikipedia.org/wiki/Ctags
[4]: http://vimdoc.sourceforge.net/htmldoc/tagsrch.html
[5]: http://vimhelp.appspot.com/if_cscop.txt.html

