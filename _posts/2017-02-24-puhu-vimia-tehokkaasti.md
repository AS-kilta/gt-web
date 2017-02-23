---
layout: post
title:  "Puhu Vimiä tehokkaasti"
date:   2017-02-23 14:00:00 +0200
categories: tekniikka
magazine: 1/2017
print_order: 7
author: Konsta Hölttä
ingress: Vimin erottaa muista editoreista vahvasti sen modaalinen luonne ja palasista yhdisteltävät näppäinkomennot.
---
Paha virhe aloittelevalla vimmaajalla on pysyä pitkiä aikoja insert-moodissa ja käyttää nuolinäppäimiä (hyi olkoon). Tähän usein ohjeistetaan korjausta: poista nuolinäppäimet käytöstä (helppoa Vimissä). Näppäimet hjkl kuljettavat kursoria normaalimoodissa vasemmalle, alas, ylös ja oikealle. Paljon parempi näin! Vai onko? Metsään mentiin pahemmin kuin Risujemmaaja. Ongelma on merkki tai sana kerrallaan ajattelu, eikä sitä korjaa noin vain. Vimissä on erinomaiset tekstiobjektit ja muotoilukomennot korkeamman tason ajattelulle.

Muokkauskompositio
Jos tekstiä editoidessa pysähtyy hetkeksi seuraamaan omia tekosiaan, niin löytää ennen pitkään säännönmukaisuuksia. (Samaa ajatusta kannattaa soveltaa ylimalkaan elämään, maailmankaikkeuteen ja Alkossa asiointiin.) Omien tekosien säännönmukaisuuksiin Vimin liikkumis- ja muokkauskomennot menevät kuin nenä päähän. Sanotaan, että Vimin tärkeimpiä filosofioita on ortogonaalisten näppäinkomentojen kompositio eli suomeksi suunnilleen yhdistely tai kokoaminen. Kompositio on ohjelmoinnissakin hyvä homma [0].

Viime GT:ssä mainittiin jo, että Vimissä on tavallaan oma kielioppinsa. Tämä on niin tärkeä juttu, että avataan sitä hieman enemmän jotta lukija pääsisi vauhtiin itseopiskelussa. Parhaita juttuja Vimissä on hyvä referenssi ja lukuisat tutoriaalit sekä lisäksi blogihajoamiset. Aallon kielikursseille pääsystä taas löytynee korkeintaan jälkimmäisiä.

Vimin komennoista voi suuren osan muotoilla kolmeen palaseen: verbi, määre (modifier) ja substantiivi. Määre ja substantiivi muodostavat eräänlaisen liikelausekkeen, joskus tekstiobjektin. Esimerkiksi: ci" (change, inside ja lainausmerkit kun tarvitsee korvata merkkijono jonka sisällä kursori on), dap (delete around paragraph poistaa kursorin kappaleen ja seuraavan rivinvaihdon) tai yt. (yank till ja piste, eli kopioi kursorista pisteeseen saakka).

Verbejä ovat mm. d (delete), c (change), y (yank), v (visual, eli maalaa-valitse). Määreisiin sisältyy mm. i (inside), a (around), f (find), t (till). Substantiivit ovat melko selkeitä: niitä on esimerkiksi w (word), s (sentence) ja p (paragraph). :help text-objects. Joskus tuo määre puuttuu komennosta, jolloin nappipainalluksia tulee kaksi, esimerkiksi d$ (poista kursorista rivin loppuun asti - tälle on lyhyempikin muoto: D).

Verbiä v (:help v) on hyvä käyttää harjoitellessa; kun muut suorittavat esim. muokkaus- tai kopiointikomennon heti, v maalaa liikelausekkeen alueen tekemättä vielä muuta. Maalatulle vaikutusalueelle voi komentaa sitten yhden verbin kun on tarkastellut että lauseke valitsi sopivan alueen tekstiä: esim. vi( maalaa sulkujen sisällön (va( myös sulut), ja sen voi sitten vaikka poistaa painamalla d. :help visual-start

Piste on paras juttu. Se toistaa aiemmin tehdyn muutoksen. :help single-repeat

Ympäri internetsin ihmemaailmaa on lukuisia hyviä lisälöpinöitä tästä aiheesta, kuha googlaa vaikka vim grammar. Muutamia oikein hyviä on mainittu Hacker Newsissä eräänä päivänä [1]. Sitä kannattaa seurata muutenkin.

Liikkuminen
Viime GT:ssä mainittiin jo Ted Naleidin värkkäämä "Vim Movement Shortcuts Wallpaper" [2]. Tämä on niin tärkeä juttu, että avataan sitä hieman enemmän. On myös hyvää jatkoa äskeiselle osalle. Tässä taas sama grafiikka, tällä kertaa pikselitaulukkona asciin sijaan:

![](https://bitbucket.org/tednaleid/vim-shortcut-wallpaper/raw/tip/vim-shortcuts.png)

Hyvin harvoin tarvitsee siirtyä etenkään samalla rivillä vain merkki kerrallaan. Laiskalla päällä olevat Viminkäyttäjät tamppaavatkin kompromissina w:tä ja b:tä, jotka siirtyvät sanan verran eteen- ja taaksepäin, mikä vastaa suunnilleen ctrl-nappia nuolilla Notepadissa (hyi olkoon). :help word-motions. Virkenänä etenkin kirjainten F, T, t ja f käyttö tuntuu allekirjoittaneella tulevan suoraan selkärangasta; usein ohjelmoidessa tarvitsee esim. poistaa tai muokata tekstiä kursorista vaikka seuraavaan sulkeeseen tai pilkkuun. :help left-right-motions

Ylös ja alas liikkuminen on myös rivi kerrallaan melko kömpelöä. Ihmisten tekstissä kappale kerrallaan liikkuminen (merkit { ja }) on luonnollista, ja noilla on ohjelmointimoodissakin usein (kielestä riippuen) jokin kätevä merkitys. Yhtään pidemmälle kannattaa rohkeasti käyttää hakua: /foo hakee foota kursorista eteenpäin, ?foo taaksepäin. Pieni ja iso n toistavat hakua samaan ja toiseen suuntaan. :help jump-motions jne.

Eestaas liikkuminen (ns. teleportaatio) kahden tai useamman sijainnin välillä onnistuu näppärästi sijaintimerkeillä, jotka ohjelmoidaan komentamalla m ja jokin kirjain. Heittomerkki ja sama kirjain vie kursorin tuollaiseen lokaatioon merkin riville; grave-aksenttimerkki ja kirjain vie täsmälliseen sijaintiin. Pienet kirjaimet ovat varattu bufferikohtaisiin merkkeihin ja isot kirjaimet toimivat globaalisti; niillä voi hyppiä vikkelästi bufferista toiseen. Kirjaimet ovat omaan käyttöön; muutamat merkit ovat maagisesti automaattisia: piste vie aiemman muokkauksen paikkaan, ja heittomerkki paikkaan mistä siirryttiin (eli näpyttämällä kaksi heittomerkkiä putkeen peräjälkeen voi hyppiä kahden sijainnin välillä). :help mark-motions

[0] [https://en.wikipedia.org/wiki/Composition_over_inheritance]()

[1] [https://news.ycombinator.com/item?id=7976493]()

[2] [http://naleid.com/blog/2010/10/04/vim-movement-shortcuts-wallpaper]()
