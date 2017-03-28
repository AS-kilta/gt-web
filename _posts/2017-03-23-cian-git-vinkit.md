---
layout: post
title:  "CIA:n Git-vinkit"
date:   2017-03-23 12:00:00 +0200
categories: tekniikka
magazine: Rankka/2017
author: Konsta Hölttä
ingress: Wikileaksin täysin legit CIA-julkistus toi ilmi, että Git on osa CIA:n(kin) "Hacking Toolseja". Vault 7:stä löytyi ainakin monta Git-aiheista sivua. GT esittelee tässä tipsit ja tricksit lyhyesti; muita varten voi käydä lukemassa tarkemmin vaikka rrrRankkabussissa ajanvietteeksi.
---
Joku pääsivu: [https://wikileaks.org/ciav7p1/cms/space_1736707.html](https://wikileaks.org/ciav7p1/cms/space_1736707.html). Howtoja ja muuta kivaa on laidasta laitaan, mutta samat löytyvät triviaalisti itsekin googlaamalla. Jutuista kyllä selviää rivien välistä miten workflowt kummastuttavat, mitä yleisimpiä käyttöpatterneita kavereilla on ja mitkä pikkujutut yleensä askarruttavat. Toisaalta tuonne kokoelmaan on vaan tuupattu iso kasa talteen muualta copypastettuja (ns. lootattuja) tutoriaaleja.

Käy katsomassa linkkien takaa yksityiskohdat; tässä tiivistelmä [tipseistä ja tenpuista](https://wikileaks.org/ciav7p1/cms/page_1179773.html). Voi kun samoja voisi soveltaa myös IRL vaikka rrrRankkatoilailuun.

* The "I can never remember that alias I set" Trick, eli CIA:nkin kaverit ovat hajamielisiä.
* Gitignore, eli CIA:nkin kaverit tunkkaavat repohakemistoon kaikkea turhaa jota ei commitata.
* The "The Git URL is too long" Trick mutta sisältö puuttuu, olisi kiva tietää mistä tässä on kyse.
* The "I forgot something in my last commit" Trick, eli CIA:kin olisi hyötynyt [aiemman GT:n amend-vinkeistä](/tekniikka/2017/02/23/git-historia-natiksi.html).
* The "Oh crap I didn't mean to commit yet" Trick, eli kts. edellinen.
* The "That commit sucked!  Start over!" Trick, eli kts. edellinen.
* The "Oh no I should have been working in a branch" Trick, eli CIA:n kaveritkaan eivät branchaa lokaalisti niin usein kuin kannattaisi tai ajattele edeltä.
* The "OK, which commit broke the build!?" Trick, eli git bisect on paras juttu.
* The "I have merge conflicts, but I know that one version is the correct one" Trick, a.k.a. "Ours vs. Theirs" eli CIA:llakin kaikki tunkkaavat kaikkea päällekkäin.
* The "Workaround Self-signed Certificates" Trick, eli CIA:n kavereillakin on custom-ssl-purkkaviritelmät ilman kunnon serttejä.
* Split a subdirectory into a new repository/project, eli joskus projektit rönsyilevät niin että pieni osa reposta forkkaa uudeksi.
* Local Branch Cleanup, eli kyllä niitä työbrancheja nämäkin jäbät käyttävät.
