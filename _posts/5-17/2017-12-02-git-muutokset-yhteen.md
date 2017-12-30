---
layout: post
title:  Git-muutokset yhteen
date:   2017-12-02 13:00:00 +0200
categories: tekniikka
author: Konsta Hölttä
ingress: Oliskohan tähän asti käsitelty lähinnä itse lokaalisti tehtäviä git-taikatemppuja. Jokaisessa yli yhden hengen puuhassa jota kehitetään git-repositoryssä saa kanssakäydä muiden ihmisten kanssa. Jo GT:ssä 1/2017 esiteltiin git rebase, jolla sotkuhistorian muotoilee siistimmäksi kun töitänsä aikoo esitellä muille pull requestin tai git pushin muodossa. Monimutkaisemmissa ympäristöissä git merge on tarpeellinen.
magazine: 6/2017
image: /static/2017-05/branch.jpg
print_order: 8
---
Gitin brancheja eli bräntsejä eli haaroja on helppo luoda ja helppo mergata toisiinsa. Muinaisten versionhallintajärjestelmien kanssa näin ei ole aina ollut [^1]. Kun pellin alla oleva tietorakenne träkkää historiaa kunnolla, branchien luonti ja niiden liittäminen takaisin on jos ei vaivatonta niin ainakin kuolevaisen tehtävissä. Eri kehityshaaroja yhdistäessä -- oli yhdistäminen sitten merge tai rebase -- vaanii aina konfliktien mahdollisuus, mikäli eri haarat ovat muokanneet samoja osia.

## Mergepolitiikkaa

Projektin kehitysprosessi muotoilee työkalujenkin käyttöä. Gitin luojalla -- eli Linus Torvaldsilla -- on aivan erityinen näkemys versionhallinnan käyttöön Linux-kernelin tiimoilta. Kerneliä devataan suunnilleen silleen, että Linus kaapii muutokset eri subsystemien ylläpitäjiltä, joilla taas on edelleen alaisia johonkin saakka eli “chain of trust”. [^2] Geneerisessä ohjelmistofirmassa hierarkiaa saattaa olla vähemmän mutta product ownereita sitten enemmän.

Git on siitä kiva, että se taipuu vähän mihin vaan ja “branchausstrategioita” tai “branchausmalleja” voi suunnilleen vetää hatusta [^3], oli sitten kernel, bisnestuote tai leluprojekti. Bräntsäilystrategiaa voi muotoilla sekin, että miten devattavan softan tunkkaa CI-järjestelmään kiinni. Mergestrategia on muuten sitten eri asia. Niitä on mm. recursive ja octopus [^4] ja cthulhu [^5]. Jos sitten projekti koostuu useammasta reposta (vaikka Android), niin voi hommata itselleen päänvaivaa vaikka tusinalla branchilla niin, että tiettyjä repoja saa kirjoittaa vain tietyissä brancheissa.

Kun Linus kokoaa muutokset monesta osasta uuteen kernelversioon, git merge on nimenomaan oikea työkalu. Yhden ylimmäisen ukkelin mergaillessa myös commit-oikeudet ovat triviaaleja [^6]. Yksittäisen devaajan päivittäistunkkaamiseen sen sijaan git rebasestakin on hyötyä, vaikka Torvalds muistaakin muistuttaa sen paikottaisesta haitallisuudesta kerneldevauksessa [^7]. Toisesta päästä nähtynä devaaja puljaa pätsejänsä softan jonkin version päälle ja ehdottaa hienovaraisesti niiden nykäisemistä kerneliin lähettämällä sähköpostilla pätsin ja pyynnön, jotka on likimain analoginen Githubin kaltaisten hostauspalveluiden pull requestille. Subsystemiä X devatessa kernelin sorsapuusta löytyvä aputyökalu [^8] kertoo, kelle kannattaa puskea pätsiä.

Subsystem-maintaineri tyypillisesti ottaa pätsin omaan puuhunsa sellaisenaan. Githubissa ja muualla ihan jokaisesta yksittäisenkin pätsin pull requestista seuraa yleensä merge commit, mikä joidenkin mielestä sotkee historiaa ihan turhaan ja toisten mielestä taas erottelee hyödyllisesti ulkopuolisten kontribuoimat muutokset. Samoin lokaalisti devatessa oletusasetusten `git pull` luo helposti ylimääräisiä merge committeja, joilla ei ole loogista merkitystä.

Masterin devaaminen ja olemassaolevien releasejen ylläpito ovat tyypillisesti kaksi ihan eri asiaa. Julkaistuihin versioihin saattaa olla omat branchinsa, joihin voidaan vaikka cherry-pickata korjauksia haavoittuvuuksiin siten, ettei käyttäjän tarvitse päivittää softaa sen kummemmin eikä tarvitse taistella uusien tai deprekoitujen featureiden kanssa.

Pienessä puuhatiimissä, firmassa, tms. työskentelyyn voi keksiä minkä vaan bräntsäilymallin. Eräs suosittu malli jota internet tarjoaa on “git-flow” [^9], joka on toisaalta saanut myös rutkasti kritiikkiä erinäisistä syistä [^10] [^11]. GT ei kannusta minkään tietyn mallin käyttämiseen. Riippuu devaajien määrästäkin, joka voi vaihdella monta kertaluokkaa.

Se politiikasta. Politiikka pitäisi jättää jollekin Prodekon pöhisijälle. Kunnon devaaja valkkaa jonkun workflown ja toteaa että ei kun tunksuttelemaan.

## Merge työkaluna

Git merge ymmärtää eri haaroissa tehdyt muutokset ja yhdistää ne jälleen yhdeksi haaraksi. Kuvittele versionhallintajärjestelmätön maailma, jossa tiedostoja editoidaan globaalissa dropboxissa ja kaksi tyyppiä yhtäkkiä editoi samaa tiedostoa päällekkäin. Voisi harmittaa. Git poistaa harmin ja ottaa molemmat muutokset yhteen vaivattomasti. [^12] [^13] [^14]

Sanotaan, että halutaan liittää topic branch aka feature branch nimeltä **paras** master-branchiin josta se joskus aiemmin on haarautunut. Checkoutataan masteriin ja sanotaan että `git merge paras`. Ideaalisti konflikteja ei tule ja syntyy merge commit, jolla on kaksi parent-committia. Mergeillä merkataan myös git muistamaan haarojen yhteenliittäminen (“merge tracking”), ja mahdolliset seuraavat merget onnistunevat helpommin. Feature branchit kyllä tyypillisesti loppuvat siihen kun ne on mergattu masterin käyttöön.

Jostain pitkäaikaisesta releasebranchista saatetaankin mergailla jotain hotfixejä useampaan kertaan. Tosin mergejen reverttaaminen on vähän kinkkistä, sillä revert ei saa gittiä unohtamaan että merge olisi tehty ja seuraava merge ei onnistu [^15]. Pitkäaikaiset branchit ovatkin yleensä inhottavia, paitsi master.

Konflikteja syntyy, kun eri kehityshaaroissa on tehty muutoksia samoihin kohtiin tiedostoja. Matti haluaa maalata auton mustaksi ja Maija valkoiseksi. Valitaanko jompikumpi, värjätäänkö vaikka vain ovet ja konepelti toisella värillä tai muotoillaanko auto seepraksi? Konfliktien ns. resolvaaminen ei kuitenkaan ole mergespesifinen juttu, vaan samaan voi törmätä mm. rebasen ja cherry-pickinkin kanssa. Git sijoittaa tiedoston ongelmakohtaan “conflict markerit” ja kummankin version; käyttäjä editoi tilalle lopullisen tekstin. Jos muutokset ovat isoja, voi olla hyödyllistä myös nähdä molempia muutoksia edeltävä yhteinen versio; tällaisen moodin saa päälle pysyvästi loitsulla `git config --global merge.conflictstyle diff3`.

Hassujen markereiden käpistely tekstitiedostojen seasta on eittämättä primitiivistä. Git tukee “mergetoolia” eli työkalua, joka ymmärtää näyttää 3-way mergen eri versiot kerralla [^12]. Esimerkiksi Vim osaa tälläisen. Tiedostomuotokohtaisen mergetoolin konffaaminen on myös likimain pakollista, jos on binääritiedostoja. Toisaalta jos binääritiedoston editori (esim. joku photoshop) ei tue mergailua tai jos merge ei käy järkeen ylipäätään (vaikka käännetylle softalle), niin on yleensä vaan pulassa ja pitää valita jompikumpi: `git merge -s ours` tai `theirs` [^13].

Mikäli mergaa toistuvasti samoja muutoksia, **rerere** on suositeltavaa ottaa käyttöön. “Reuse recorded resolution” tallettaa kontekstin ja käyttää sitä uudelleen jos sama konflikti toistuu. [^14]

Haarautuminen ja haaroista yhteen kerääntyminen on tosiaan helppoa ja merkittävimmät hankaluudet aiheutuvat sopimisesta ihmisten kanssa. Gitin mergailutyökalut itsessään ovat tehokkaita ja mikä vaan onnistuu. Ei muuta kun manuska käteen ja harjoittelemaan.

## Lähteet

[^1]: <https://stackoverflow.com/questions/2471606/how-and-or-why-is-merging-in-git-better-than-in-svn>
[^2]: <https://www.kernel.org/doc/html/v4.11/process/2.Process.html>
[^3]: <https://www.google.com/search?q=git+branching+strategy>
[^4]: <https://git-scm.com/docs/merge-strategies>
[^5]: <https://marc.info/?l=linux-kernel&m=139033182525831>
[^6]: <https://marc.info/?l=git&m=113072612805233&w=2>
[^7]: <https://marc.info/?l=linux-kernel&m=128570555606885&w=2>
[^8]: <https://github.com/torvalds/linux/blob/master/scripts/get_maintainer.pl>
[^9]: <http://nvie.com/posts/a-successful-git-branching-model/>
[^10]: <https://barro.github.io/2016/02/a-succesful-git-branching-model-considered-harmful/>
[^11]: <http://endoflineblog.com/gitflow-considered-harmful>
[^12]: <https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging>
[^13]: <https://git-scm.com/book/en/v2/Git-Tools-Advanced-Merging>
[^14]: <https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows>
[^15]: <https://git-scm.com/blog/2010/03/02/undoing-merges.html>
[^14]: <https://git-scm.com/blog/2010/03/08/rerere.html>

*(kuva: [Wikipedia / Daniel Schwen](https://en.wikipedia.org/wiki/File:CTA_loop_junction.jpg))*
