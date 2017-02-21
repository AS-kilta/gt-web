---
layout: post
title:  "Git-historia nätiksi"
date:   2017-01-24 23:39:46 +0200
categories: tekniikka
author: Konsta Hölttä
ingress: Muille julkaistu tai muutoin valmis gitticommitti on ideaalisesti siisti, yhtenäinen kokonaisuus. Julkaisemattomille ei ole sääntöjä; niitä kannattaa tehdä tiheään, ja niistä finaaliversion trimmaamiseen on hyvät työkalut.
---

git add -p: `Stage this hunk [y,n,q,a,d,/,K,j,J,g,e,?]?`. Mitä kävi sille vanhalle kunnon "Abort, Retry, Fail?" -virheviestille DOSista?

Viime GT:ssä esiteltiin tusinahko komentorivi-gittiloitsua, joita googlaamalla pitäisi kunnon aasin olla tässä vaiheessa jo saanut muutamat ensimmäiset commitit kirjoitettua. Tyypillisen aurinkoisen maariyön jälkeen Gitin historia näyttää helposti jotakuinkin tältä: (lähde https://xkcd.com/1296/)

![](https://imgs.xkcd.com/comics/git_commit.png)

Kunhan et vielä ole git pushannut, niin ei hätää! Kukaan harkkaryhmäläinen ei tule näkemään häpeääsi (varo kuitenkin olan takaa kurkkivia)! On vieläpä ihan normaalia devata tuolla lailla, koska kannattaa committailla ihan koko ajan. Huhu kertoo, että robokättä devatessakin yksi vaivainen `git reset --hard` aiheutti harmia koska git ei vielä tiennyt muutoksista.

Pienemmät commitit voi yhdistää toisiinsa ja niiden järjestystäkin voi säätää, yhdellä ehdolla - muille julkistettuja tuotoksia ei sovi muokata, sillä julkinen historia on kiveen hakattu. Pidä siis devausräpellys omana lokaalina tietonasi kunnes se on valmis, tai korkeintaan pushaa johonkin henkilökohtaiseen devibranchiin. Jotkut tykkäävät useiden pikkucommittien tekemisen sijaan `git commit --amend`ata yhteen pätsiin pieniä muutoksia lisää koodirupeaman aikana; tälläinen on aavistuksen hankalampi irrottaa jälkeenpäin useammaksi erilliskokonaisuudeksi, mutta silti täysin mahdollista.

Järkevä committi on selkeä, yksittäinen osakokonaisuus. Jos commit-titlessä esiintyy sana "ja", harkitse pilkkomista. Jos koodi ei käänny kahden commitin välissä, yhdistä ne. Esimerkiksi jonkin APIn uuden featuren esittelyn ja sen eri backendien implementaation voi ja ne kannattaa erottaa toisistaan; yksittäiset riippumattomat muutokset ovat myös helpompia hahmottaa aina kun täytyy kummastella että mitä on tapahtunut.

Commithan on muokkaus repon aiemman tilan päälle. Manuaalisin tapa pyöritellä committeja ympäriinsä on `git cherry-pick`. Se ottaa commitin muutokset ja (yrittää)suorittaa ne nykytilan päälle, oli mikä oli. Esimerkiksi jos branch B on N committia edellä branchia A, ja nämä commitit ovat keskenään riippumattomia, niin onnistuu aina checkouttaaminen mihin tahansa committiin siltä väliltä ja koodi kääntyy, ja minkä tahansa uudemmista commiteista saa cherry-pickattua siihen. Ohessa mielivaltainen havainnekuva 1 jossa N=3, ja z on laitettu x:n päälle commitiksi z' johon on vielä luotu branch nimeltä C.

[ Hei taittaja, tähän tästä drivefolderista löytyvä "gitrebase.eps", kuvatekstiksi "Kuva 1" ]

Rebase on näppärämpi temppu, jolla saa otettua useamman commitin ja lätkäistyä ne muualle pohjautumaan (base) eri kohdasta, mistä tulee nimi "re-base" (v1.7.8.6 manpagesta: "Forward-port local commits to the updated upstream head". Allright selvä. Uusimman version manpage sen sijaan on jopa luettava ja hyödyllinen!). Tätä ei varsinaisesti tarvitse mitenkään joka sekunti. Sen sijaan vipu `-i` (`--interactive`) soveltuu käytettäväksi devaamisen aikana: interaktiivisella rebasella voi muokata aiempia committeja, yhdistellä niitä, committailla väliin lisää ja myös poistaa yksittäisiä välistä. Sitä voi ajatella ikäänkuin puoliautomatisoituna rivinä cherry-pickejä lisäominaisuuksin (pelkästään cherry-pickillä ei saa committeja yhdistettyä).

Rebase käytännössä ajetaan sanomalla mistä commitista halutaan nykyhetkeä aiemmat commitit alkavan. Ilman interactive-modea sillä saadaan vaikka jokin lokaali featurebranch alkamaan uudemmasta pisteestä jos master on muuttunut. Interaktiivisena se avaa vielä tekstieditorin, jossa jokainen commit on omalla rivillään, ja niitä edeltää sana `pick`, joka tarkoittaa pätsin valitsemista. Pickin tilalle saa jonkun näistä: `edit`, `reword`, `drop` (tai poista koko rivi), `squash` tai `fixup`.

Kuvan 1 tilanteeseen päästäisiin rebasella siten, että B:ssä ollessa checkoutattaisiin uuteen branchiin C, komennettaisiin `git rebase -i A`, poistettaisiin ilmestyvästä editorista y:tä vastaava rivi ja poistuttaisiin editorista. Kokeile!

Jos jo etukäteen tietää mihin aiempaan committiin jokin seuraava pikkucommit pitää yhdistää devailun aikana, niin rebasehommasta selviää hieman helpommalla. Commit ottaa parametrin `--fixup=<commit-ref>`, joka luo tälle uudelle commitille sopivasti formatoidun otsikon siten, että `git rebase --interactive --autosquash` venkslaa pätsit suoraan sopivaan järjestykseen. Commitille voi myös sanoa vastaavasti `--squash=<commit-ref>`; fixupin ja squashin ero on se, että fixuppien commitviestit heitetään pätsejä yhdistellessä mäkeen kun squashissa Git länttää ne alkuperäisen mukaan.

`git add` osaa myös interaktiivisen tilan (`-i` tai `--interactive`). Tuon tilan parhaalle toiminnolle on oikopolku `add -p` (`--patch`). Git kysyy muutos muutokselta, lisätäänkö kutakin indeksiin vai ei. Patch-tila toimii myös mm. checkoutille ja resetille. `git reset -p`:stä saa erityisesti kätevän "undon", sillä se on manpagenkin mukaan vastakohta `add -p`:lle: `git config --global alias.undo checkout -p HEAD^ --` ja `git config --global alias.undosoft reset -p HEAD^ --`. Undo nyt hakee osan tiedostosta ennen muutoksia myös worktreehen, ja undosoft undoaa muutokset vain indeksiin siten, että undoamisen voi commitata suoraan mutta worktree jää ennalleen redoamaan nuo taas. Näillä kahdella voi hyvän harjoittelun jälkeen järjestellä isompia committeja pienemmiksi tai vaikka vaihtaa yhden rivin commitista toiseen, jos devasi useampaa featurea joista yhden muutokset livahtivat väärään committiin. Harjoittele, hajoile ja opi!

Rebase on mainio juttu myös pullatessa: `git pull --rebase`. (Miksi ohjelmoijat ovat niin paksuja? Joka päivä pullaa, hah hah. Toisaalta rintalihakset pysyvät kunnossa kun pushaa säännöllisesti.) Konffitiedostoista saa tämän asetuksen myös oletukseksi. Näin `git pull` tekee lokaaleille commiteille rebasen sen sijaan että historia menisi jojoon mergecommiteista, jotka pienemmän tiimin workflowssa kuten jossain harjoitustyökursseilla vain sotkevat pakkaa - toisinaan lineaarinen historia on nätimpi. (Jotkut fanaatikot voivat olla eri mieltä; se on ihan ok.)
