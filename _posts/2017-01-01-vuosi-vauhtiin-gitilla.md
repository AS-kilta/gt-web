---
layout: post
title:  "Vuosi vauhtiin Gitillä"
date:   2017-01-01 12:00:00 +0200
categories: tekniikka
author: Konsta Hölttä
ingress: Git on toinen yleisimmin käytetyistä ja tunnetuimmista järkevistä avoimen lähdekoodin versionhallintajärjestelmistä; jotkut vääräuskoiset tykkäävät käyttää Mercurialia.
---

Linus Torvalds kehitti Gitin korvaamaan BitKeeperin
käytön Linux-kernelin kehityksessä joskus 2005. Nykyisellään Gitiä kehittää ja
ylläpitää kasa muitakin ihmisiä. Käyttäjiä sillä on `SIMOSTI()`, ja jollet kuulu
joukkoon niin ei hätää - voit kääntää uuden sivun elämässäsi nyt
vuodenvaihteen kunniaksi! Tässä lyhykäinen potkustartti peruskäyttöön; jatkoa
seuraa myöhemmissä numeroissa. Gittiin kannattaa tuupata kaikki projektit,
joiden parissa puuhailu kestää pidempään kuin yhden illan.

Gitiä käytetään tiedostojen säilömiseen ja niiden muutos- eli versiohistorian
hallintaan. Kerralla säilöttyä dataa yhden hakemiston alla kutsutaan
repositoryksi tai repoksi. Joistain muista systeemeistä poiketen Git versioi
koko repon kaikkien tiedostojen tilan kerralla, eli eri tiedostoista ei ole
yksitellen erillisiä versioita. Jokaisesta tilasta lasketaan sen yksilöivä
sha1-tiiviste (hash), eikä "versionumero" tarkoita mitään, sillä historia ei
välttämättä ole täysin lineaarinen vaan itseasiassa graafi. Git on nimensä
mukaisesti "tyhmä", mikä tavallaan tarkoittaa, että se sisimmässään tietää
hyvin vähän datan formaatista ja käsittelee kaiken binääridiffeinä. Tästä
syystä se soveltuukin ihan hyvin suunnilleen kaikkeen, havaitsee
heuristiikoilla mm. tiedoston siirtämisen (uudelleennimeämisen) ja sallii
kaikenlaiset ulkoiset lisätyökalut.

Git toimii hajautetusti (distributed), mikä on erityisesti lähdekoodin ja
vastaavan mössön hallintaan rutkasti järkevämpää kuin keskitetty (centralized)
vaihtoehto. Tämä tarkoittaa sitä, että järjestelmällä on koko historia
paikallisesti saatavilla, eikä esim. tiedostojen muutoslokin tarkastelemiseksi
tarvitse ottaa yhteyttä mihinkään keskusserveriin. Tätä ei kannata kuitenkaan
sekoittaa siihen, että Gitin kanssakin toimii hyvin keskitetty devausmalli,
jossa kaikki devaajat kommunikoivat esim. Githubin kautta. Se vain ei ole
välttämätöntä. Kullakin saman repon käyttäjällä on oma historiatietokantansa,
jossa jokaisella voi olla toisille vielä julkaisemattomia muutoksia.

Gitin kaikki toiminnot pyörivät vahvasti sen sisäisen tietomallin ympärillä.
Git on mainio hakkerityökalu viitseliäille macgyvereille, mutta sen varmaan
suurin haittapuoli on juurikin se, että itse työkalu olettaa käyttäjän
tietävän melko tarkkaan, miten se sisäisesti toimii. Siispä on hyvä tietää
perusteet sekä komentorivihommista haluttuihin toimintoihin että siitä, mitä
nämä toiminnot ovat Gitin mielestä. Git säilöö historian sisäisesti
tietorakenteena, joka yleisesti tunnetaan DAGina (directed acyclic graph).
Commitit ovat solmuja verkossa, ja se siitä. Joillakin solmuilla on erityinen
nimi: haara (branch). Uudet commitit tehdään yleensä johonkin haaraan, jolloin
haaran nimilappu etenee juuri commitattuun solmuun.

Tässä komentorivityökaluja joilla pääsee alkuun, joista kaikille on toki vielä
N parametria, googlaa toki lisää tai yritä tajuta manpageja (haha):

* git init - kerro gitille, että haluaisit träkätä nykyistä hakemistoa
* git status - tarkastele repon nykytilaa
* git diff - vertaile hakemiston tilaa committien (tai nykytilan) välillä
* git add - lisää muuttuneita tiedostoja commitattavaksi (-p on paras juttu)
* git commit - lisää verkkoon uusi solmu hiljattain addatuista muutoksista
* git log - tarkastele muutoshistoriaa
* git rebase - muokkaa historiaa (-i on paras juttu)
* git branch - muokkaa olemassaolevia haaranimiä
* git checkout - siirry työskenneltävästä haarasta tai commitista toiseen
* git merge - yhdistä kaksi haaraa eli lisää verkkoon uusi solmu, jolla on
  kaksi parent-solmua; näin liitetään rinnakkain tehdyt työt yhteen
* git cherry-pick - kopioi commitin muutos jostain muualta ja länttää se
  nykytilan päälle, yleensä toiseen branchiin kuin alkuperäinen

Lopuksi vielä käteviä aliaksia konffeihin helpottamaan elämää:

```
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.df diff
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.sh show
git config --global alias.cp cherry-pick
git config --global alias.lol ‘log --graph --decorate --pretty=oneline
                                  --abbrev-commit --all’
git config --global pull.rebase true
git config --global core.editor vim
```

<br>

Jatkolukemista innokkaille:

[http://xkcd.com/1296/]()

[https://git-man-page-generator.lokaltog.net/]()

[http://git-animals.tumblr.com/]()

[http://wheningit-blog.tumblr.com/]()

[https://www.linux.fi/wiki/Git]()

[http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html]()

[http://chris.beams.io/posts/git-commit/]()

[http://eagain.net/articles/git-for-computer-scientists/]()

[https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control]()

[https://git-scm.com/documentation]()

[http://gitready.com/]()

[http://gitolite.com/gitk.html]()

[https://lostechies.com/joshuaflanagan/2010/09/03/use-gitk-to-understand-git/]()

[https://github.com/tpope/vim-fugitive]()