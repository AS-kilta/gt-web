---
layout: post
title:  "Gitin lokihakutyökaluja"
date:   2017-09-20 12:00:00 +0200
categories: tekniikka
author: Konsta Hölttä
image: /static/2017-03/pickaxe.jpg
ingress: Satunnainen gitinkäyttäjä käyttää git logia lähinnä lukeakseen mitä viime aikoina on tapahtunut. Tehokäyttäjä käyttää tätä tehokasta lokinmurskainta vaikkapa laskemaan omat committinsa tai etsimään että milloin jokin ominaisuus ilmestyi softaan ensimmäistä kertaa.
magazine: 4/2017
print_order: 6
---

Yksi parhaista "bugeista" on yleisesti tunnettu vitsi, että jollain softalla on enemmän komentorivillä käytettäviä vipuja kuin ls:llä. (ls:llä (v8.27) on [manuskan][1] mukaan noin 58 kpl.) No, git log [menee ohi että humahtaa][2].

Pelkän commithistorian selailu on joskus näppärää katsomalla vain otsikoita. Siis `git log --oneline`. Yksiriviselle muodolle soveltuu erityisen hyvin kaveriksi `--graph`, joka piirtää sellaisen junarataverkoston mergeistä ja eri brancheista. GT suosittelee: `git config --global alias.lol "log --graph --decorate --pretty=oneline --abbrev-commit --all"`. Tuolle prettylle on muuten n+1 muotoilumäärettä, tutustuminen suositeltavaa.

Commitlokihan on kaikki commitit koskaan (yleensä kuitenkin vain nykyisessä branchissa sijaitsevat, ellei sano vaikka `--all`). Kaikista commiteista koskaan voi etsiä vähän kaikenlaista. Perus `git log` vailla parametreja lienee tuttu kaikille. Vähän informatiivisempi loki on `git log --stat`, joka näyttää joka pätsin yhteydessä muuttuneet tiedostot ja muutoksien määrät eli diffstatin. `--shortstat` näyttää näistä pelkästään viimeisen rivin, eli yhteenvedon muuttuneiden tiedostojen määrästä ja rivimääristä. Monesti sitä kaipaa muutaman viimeisen pätsin sisältöä, ja `git show` kullekin peräjälkeen olisi vähän hankala. No, `git log -p` näyttää ne patchit eli kokonaiset muutokset ikäänkuin sisältö monesta `git show HEAD~<numero>` -komennosta olisi konkatenoitu peräjälkeen.

Välillä täytyy löytää jotain tiettyä. Lokia voikin filtteröidä monin tavoin. Esimerkiksi `git log --author=torvalds` suodattaa näkyviin vain ne, joiden author-rivi vastaa annettua regexpiä. Vipu `--committer` toimii vastaavasti. `git log --grep=kukkuu` greppaa esille ne, joiden commitviestissä näkyy haettu pattern.

Ajan perusteella on näppärää etsiä asioita. Mitä tapahtui viimeisen kahden viikon aikana? `git log --since="2 weeks ago"`. Vähän kuin vaikka `git log -n 42` joka näyttäisi viimeisimmät 42 committia, paitsi että aika on toisinaan lukumäärää kiinnostavampi. Entä jos muutokset parilta viime viikolta kiinnostaa poislukien tämä päivä? `git log --stat --since="2 weeks ago" --until=yesterday`. Untilia vastaava vipu numeromääreille on `--skip`. Aikamääreiksi käyvät myös absoluuttiset päivämäärät jne.

Kun committien metadatat on nyt käsitelty, voidaan todeta, että kyllä sieltä varsinaisesta sisällöstäkin voi kaivaa asioita git logilla ja se yleensä on mielekkäämpääkin. Parhaita lokitemppuja ovat `-S` ja `-G` (katso myös `--pickaxe-all`, `--pickaxe-regex`). `git log -S rommi` löytää commitit, joissa rommia löytyy eri määrä ennen ja jälkeen muutoksien. Näin selviää, minne rommi on kadonnut kun repon nykytilasta se ei greppaudu. Sitten `git log -G rommi` löytäisi myös ne vietävät commitit, jotka vaivihkaa koskevat rommiin mutta eivät vielä lisää tai poista sitä kokonaan.

Toisinaan filtteröinnistä ei ole iloa, vaan halutaan tavallaan diffata vaikka eri branchien tilaa. `git log phuksi..zombi` löytää ne commitit, jotka zombissa ovat mutteivät phuksissa. Kyseessä voi olla vaikka ihan raaka commit-sha1:kin -- branchnimi on vain erikoistapaus.

Lokin selaamisen yhteydessä yleensä kiinnostaa ne committien sisällöt paljon enemmän. Moni login manpagesta löytyvä asia on relevantti myös kun kyseessä on [`git diff`][3]: esim. `git diff phuksi..zombi` näyttää repon sisällön muutokset näiden kahden tilan välillä, kun log näyttää commitit. Diff on vähän kuin `git log -p` ilman commitviestejä ollenkaan. Diffillekin toimii mm. `-S` ja `-G`. On suositeltua tarkistaa mitä `--word-diff` tekee, tai vaikka `--color-words=.` (taikuutta).


[1]: http://man7.org/linux/man-pages/man1/ls.1.html
[2]: https://git-scm.com/docs/git-log
[3]: https://git-scm.com/docs/git-diff
