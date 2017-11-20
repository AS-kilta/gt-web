---
layout: post
title:  "Löydä syyllinen Gitillä"
date:   2017-10-31 19:00:00 +0200
categories: tekniikka
author: Konsta Hölttä
ingress: Askarruttaako koskaan, että kenen käsialaa on jokin merkillinen funktio, tai että minä aikoina jotakin tiedostoa ylipäätään on muokattu miltäkin riviltä? Viime GT:ssä käytiin läpi historiankaivelutyökalua, josta päästäänkin perspektiiviä hieman vaihtamalla git blameen. Blame kertoo tiedoston rivien revisiot, authorit ja päiväykset.
magazine: 5/2017
image: /static/2017-04/homer.gif
---
Komentamalla `git blame tiedostonimi` Git [tuuttaa esille](https://git-scm.com/docs/git-blame) tiedoston sisällön siten, että vasempaan laitaan on annotoitu kutakin riviä vastaavan commitin sha1-revisiokoodi, kirjoittaja, kirjoituspäivämäärä ja rivinumero. Näitä voi toki muokata monella ekstravivulla. Tässä oletustulosteesta esimerkiksi rivit 51-56 gt-webin tiedostosta index.html kirjoitushetkellä:

{% raw %}
    6d9bb72a (Juuso Mikkonen 2017-02-04 14:44:53 +0200 51)   <div class="sidebar">
    64423deb (Juuso Mikkonen 2017-02-23 15:35:19 +0200 52)     <div class="sidebar__new-magazine teaser teaser--magazine">
    64423deb (Juuso Mikkonen 2017-02-23 15:35:19 +0200 53)       {% include magazine.html %}
    64423deb (Juuso Mikkonen 2017-02-23 15:35:19 +0200 54)     </div>
    6d9bb72a (Juuso Mikkonen 2017-02-04 14:44:53 +0200 55)   </div>
    ^063bff2 (Juuso Mikkonen 2017-01-14 23:41:06 +0200 56)
{% endraw %}

Eipä painettuun lehteen viitsi pidempää tunkea, mutta muoto käy toivottavasti tästä ilmi. Hattu rivin alussa tarkoittaa, että kyseinen revisio on blame-rangen rajalla (tässä repon ensimmäinen commit kun ei speksattu rajaa). Tiedoston sidebar-divi on selvästi lisätty yhdessä commitissa, ja parikymmentä päivää myöhemmin sen sisältöä vähän pätsättiin. Komento `git show 64423deb` kertoo, että aiemmin sidebarissa oli pelkästään tuo include ja teaser-juttu on tosiaan kokonaan uutta. Revisiot lyhennetään oletuksena; vipu `-l` näyttää ne kokonaan.

Blamelle voi antaa tiettyjä muistakin komennoista tuttuja vipuja ja esim. `-w` jättää diffailussa whitespacet huomiotta. Tällöin tuo esimerkkiblamekin muuttuu hieman. Kokeileminen jätetään kotitehtäväksi, repohan löytyy osoitteesta [github.com/juusaw/gt-web](https://github.com/juusaw/gt-web).

Metadatasaraketta voi muokata vaikka siten, että sanotaan `-e` (`--show-email`) jolloin nimen sijaan tulee sähköposti, tai `-s` jolloin nimi ja timestamp katoavat kokonaan.

Kun bleimailun jälkeen on löytynyt rikkinäinen commit, se voidaan undottaa revertillä. `git revert <sha1>` applyttää pätsin käänteisesti eli poistaa sen lisäämät rivit jne. Vipu `-e` on oletuksena mukana eli Git antaa editoida revert-commitviestiä, johon kannattaa selittää minkä bugin johdosta muutos tehdään tekemättömäksi. Revertoidessa voi toki joutua resolvaamaan konfliktit jos revertoitavasta commitista on aikaa, ja mergecommitin undoaminen onkin jo toinen tarina.

Blamessa on muitakin toisinaan hyödyllisiä vipuja; manuska käteen ja oppimaan. Erikseen on olemassa stabiilimpi ja skriptiluettavampi formaatti `--porcelain` jota korkeamman tason työkalut voivat käyttää. Blamen outputista onkin vähän kurjaa copypastailla revisiota toiselle git blamelle tai vaikka git showlle elikkä erinäisten korkeamman tason käyttöliittymien (kiiltävämmän posliinin) harkitseminen on ihan suotavaa. Esim. Gitille on [Fugitive-niminen plugari](https://github.com/tpope/vim-fugitive): "a Git wrapper so awesome, it should be illegal", jolla blame-moodissa voi seikkailla pätsistä toiseen ja avata helposti commitin joka muutti jotain tiettyä riviä. Toisaalta  Gitissä itsessäänkin on ankeahko `git gui blame` pikaiseen käyttöön.

Lopuksi muutama varoituksen sana. Kun koodia blameilee, kannattaa varautua oheisen Homerin tavoin piiloon siltä varalta, että syylliseksi bugiin osoittautuukin vianetsijän oma committi kaukaisten aikojen takaa. Jos vaikka innostuu huonoon mouhoon ennakko-oletuksin, ja löytää lopulta itsensä kuin peiliin katsoen, voi kasvojen verenkierto yhtäkkiä vauhdittua kun `git revert` tehdäänkin omalle pätsille.
