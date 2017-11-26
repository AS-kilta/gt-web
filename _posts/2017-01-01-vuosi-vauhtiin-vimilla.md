---
layout: post
title:  "Vuosi vauhtiin Vimillä"
date:   2017-01-01 12:00:00 +0200
categories: tekniikka
author: Konsta Hölttä
ingress: Tehokas tekstieditori on ammatikseen tai muutoin ahkerasti koodia tai muuta tekstipohjaista kirjoittavan tärkein työkalu. GT tarjoaa vuoden mittaisen artikkelisarjan Vimin käyttöön - mitä, miksi ja miten.
---

Vim on yksi maailman kolmesta[^1] eeppisimmästä editorista. Muinaisuudestaan
huolimatta se on edelleen kaikkeen käyttöön erityisen soveltuva ja jopa
moderni. Muinaisuutensa vuoksi se on erittäin nopea [^2] (vanhat koneet eivät
olleet, joten ennen oli editorien ohjelmoijat rautaa). Moderniutta pönkittää
valtava laajennettavuus [^3]. Vimistä forkattu Neovim [^4] on vieläkin
modernimpi, ja mikä parasta, luonnollinen käyttää vanhoille Vimmaajille.

Tässä artikkelisarjan ensimmäisessä osassa esitellään muutama oleellinen
viminkäyttöperiaate ja joitakin perussäätöjä. Erikoisuuksiensa takia Vim ei
varmasti sovellu kaikille, mutta ne joille sopii varmaankin tykkäävät.
Kokeilemiseen suositellaan varattavaksi aikaa, vaikka sanonta, että Vi(mi)n
oppimiskäyrä on askelfunktio [^5], ei olekaan totta (askelia on useita).

Asetuksien säätämiseen järkeväksi Vi:stä pohjautuvan historian päälle kannattaa
tehdä jotain heti ensimmäisellä käyttökerralla. Jo muutamalla yksinkertaisella
säädöllä saa paikattua historiallisia jäänteitä ja asetettua käteviä
lisätoiminnallisuuksia.

Vimissä on likimain äärettömien asetusten lisäksi paljon sille ominaisia
yksinkertaisia perusasioita, joita yhdistämällä emergoituu varsinainen
elämäntapa ja oma filosofianhaaransa. Tätä kaikkea ei voi opettaa yhdessä
introtekstissä - ei edes koko vuoden lehdissä - mutta tavoitteena on innostaa
kokeilemaan sekä ohjeistaa muutama protip.

Kyseessä on "modal editor" eli tässä härpättimessä on kuusi eri tilaa. Jotkin
näppäinyhdistelmät toimivat joissakin, eikä missään tunnu olevan täyttä järkeä.
Sanotaan, että Vimissä on kaksi eri moodia: toisessa se piippaa, ja toisessa se
poistaa kaiken. Tämä on oikeasti Vi. Vimin tilat ovat normal, insert, visual,
select, commandline ja ex. (Vi on Visual ex ja ex julkaistiin 1978.)

Suunnilleen kaikkiin toimintoihin on oma "kielioppi" [^6]. Jos huomaa vaikka
selailevansa ympäriinsä nuolinäppäimillä tai poistavansa tekstiä merkki
kerrallaan, niin on menossa ihan metsään. Erikoisliikkeitä on roppakaupalla, ja
nämä yleensä yhdistetään johonkin muokkaustoimintoon tuon kieliopin kautta.

Samaan "tabiin" saa useita "ikkunoita", joissa kussakin lojuu yksi "bufferi".
Varttuneemmalla vimmaajalla voi olla helposti vaikka kuusi tiedostoa näkyvillä
samanaikaisesti - tai sitten samasta tiedostosta useita eri näkymiä.

Jos editorista ei löydy makroja, se on suorastaan käyttökelvoton mihinkään
työhön, missä tarvitaan lukuisia määriä säännönmukaisia editointeja. Vimin
editointikielioppi tanssii kauniisti makrojen kanssa, ja makroja voikin
tallettamisen jälkeen jopa editoida merkki kerrallaan.

Alkuun pääsee käynnistämällä (g)vimin ja kirjoittamalla `:help`. Sen näyttämä
"quickref" on huikean kätevä ja sisältää kaikenlaista jota ei tässä tarvitse
turhaan toistaa. Kokeile!

Tässä "Movement cheat sheet" asciina; nähdään selvästi nuolinappien turhuus:

{% highlight text %}
                  gg
              # n ?text N
                  C-b C-u
                  H
                  { (
      ;    B gE   k           ,
 0 ^ Fx Tx b ge h   l e w tx fx $
      ,           j   E W     ;
                ) }
                  L
              C-d C-f
                N /text n *
                  G
{% endhighlight %}


Joitakin perussäätöjä, laita `$HOME/.vimrc`:hen:

{% highlight text %}
filetype plugin indent on " tiedostotyypin arvausmagiat päälle ja autoformatia
syntax enable " päälle ne vimin 16 tai jotain oletusväriä
set nocompatible " vi-purkkaa pois. lainausmerkki on btw kommentin alku
set ruler " yleishyödyllistä kivaa alariville
set number " rivinumerot
set background=dark " kellä muka on valkoinen tausta terminaalissa?
set incsearch " ala etsiä jo kun hakutekstiä kirjoittaa, ennen enteriä
set hlsearch " hakulöydöt on hyvä korostaa väreillä
set mouse=a " hiiri toimii joka tilassa kuten olettaisi
set backspace=indent,eol,start " moderni pyyhkimisnappi toimii notepadmaisesti
set smartindent " arvaa sisennykset suht järkevästi
set wildmenu wildmode=longest,list " suurruhtinaiden tab-täydennykset
set hidden " älä vaadi välitöntä tallentamista bufferia sulkiessa
{% endhighlight %}

Jatkolukemista innokkaille:

* [https://github.com/tpope/vim-sensible](https://github.com/tpope/vim-sensible)
* [https://mouhola.club/.vimrc](https://mouhola.club/.vimrc)
* [http://stackoverflow.com/questions/1218390#1220118](http://stackoverflow.com/questions/1218390#1220118)
* [http://www.viemu.com/a-why-vi-vim.html](http://www.viemu.com/a-why-vi-vim.html)
* [http://naleid.com/blog/2010/10/04/vim-movement-shortcuts-wallpaper](http://naleid.com/blog/2010/10/04/vim-movement-shortcuts-wallpaper)

<br>

[^1]: Loput kaksi kolmesta: ed ja emacs. Vain toinen on moderni.
[^2]: [https://pavelfatin.com/typing-with-pleasure/](https://pavelfatin.com/typing-with-pleasure/)
[^3]: [http://www.vim.org/scripts/](http://www.vim.org/scripts/)
[^4]: [https://neovim.io/](https://neovim.io/)
[^5]: [https://i.stack.imgur.com/7Cu9Z.jpg](https://i.stack.imgur.com/7Cu9Z.jpg)
[^6]: [https://benmccormick.org/2014/07/02/learning-vim-in-2014-vim-as-language/](https://benmccormick.org/2014/07/02/learning-vim-in-2014-vim-as-language/)
