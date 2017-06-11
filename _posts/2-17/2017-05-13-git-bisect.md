---
layout: post
title:  "GT suosittelee: git bisect"
date:   2017-05-03 12:00:00 +0200
categories: tekniikka
author: Konsta Hölttä
ingress: Softa yleensä hajoaa sitä kehitettäessä aika ajoin eikä vikaa välttämättä löydetä ihan viimeisimmistä muutoksista. Git auttaa rikkinäisen commitin löytämisessä hyvinkin tehokkaasti kunhan vian voi testata luotettavasti.
magazine: 2/2017
print_order: 10
---
Muiden testausartsujen lomaan sopii hömppää tälläisestä lempityökalusta kuha puhutaan testaamisesta. Puolitushakuhan on yleisesti ottaen todettu erinomaiseksi menetelmäksi aina kun pitää hakea järjestetystä joukosta juttuja vauhdikkaasti. Versionhallinnassa muodostuu joskus sellainen järjestys, että versio *A* toimii ja versio *B* ei toimi. Näiden välistä täytyy löytää *ensimmäinen* versio, joka ei toimi. Voitaisiin kovin tehottomasti kokeilla jokaista versiota siellä välissä, mutta se olisi tyhmää. [Git bisect][1] auttaa testaamaan.

Git bisectiä voi ajatella tavallisena yksiulotteisena puolitushakuna (binäärihakuna), vaikka todellisuudessa asia [ei ole näin yksinkertainen][2] - lyhyesti sanottuna bisect toimii kuitenkin oikein vaikka *A:n* ja *B:n* välissä olisi mergecommitteja.

Puolitushakuhan toimii sillä lailla, että käytössä on etsittävän alueen rajat ja alueen tiedetään olevan järjestetty. Näin sitten alueen sisälle osuvan asian sijainti löytyy O(log n)-ajassa, kun alue voidaan joka iteraatiokierroksella jakaa kahtia uusiin rajoihin. [(Wikipedia.)][3] Softabugien bisectailussa on sama ajatus.

Oletetaan, että softa toimi joskus muinoin pisteessä *A*. Myöhemmin pisteessä *B* huomataan bugi. Bugi on mennyt muhimaan joskus *A:n* ja *B:n* välissä. Komennetaan `git bisect start`, `git bisect good <A:n commit-id>`, ja `git bisect bad <B:n commit-id>`. (Kelpaa myös suoraan `git bisect start <B:n commit-id> <A:n commit-id>`). Seuraavaksi git antaa jonkun commitin, jonka toimivuus testataan (ensimmäinen testattava on tasan *A:n* ja *B:n* välissä). Jos toimi, komennetaan `git bisect good`. Jos ei, `git bisect bad`. Näin git osaa kääntyä oikeaan suuntaan kokeilemaan uudempia tai vanhempia committeja. Lopulta vika löytyy kun good/bad-merkintöjä toistetaan.

Käsin testaaminen voi olla kurjaa varsinkin jos testattavia askelia on monta ja testien ajaminen kestää huomattavan määrän aikaa. Tällöin koko touhun voi automatisoida. Huitaistaan hihasta skripti, joka ajaa testit ja kertoo paluuarvollaan menikö läpi. Tälläisen testin tekeminen sivuutetaan triviaalina. Sitten komennetaan bisectin starttailun jälkeen `git bisect run <testikomento> <parametrit>`. (pelkkä komento voi riittää jos parametreja ei tarvita.) Git tekee temppunsa ja löytää ongelman.

Maagista, eikö vain. Tämä toimii, kunhan vain *A:n* ja *B:n* välillä järjestys on oikeasti lineaarinen: *B:ssä* ilmenevän bugin täytyy ilmaantua jossain välicommitissa *A:n* jälkeen ja pysyä *B:hen* asti. Muutoin voi löytyä välistä false positive joka toimii vahingossa. Jos joku pönttöpää on "rikkonut bisectin" committaamalla roskaa joka ei esim. käänny tai testin ajaminen muutoin ei onnistu, `git bisect skip` ohittaa kyseisen commitin testaamisen. Ja jos testi ei ole täysin luotettava, sen voi vaikka ajaa monta kertaa ja päätellä probabilistisesti että saattaapi toimia tai olla toimimatta; tähänkin on sitten työkaluja gitin ulkopuolella.

Bisectausta on helppo käyttää jos vain projekti on yhden repon sisällä. Monesta reposta koostuvan projektin bisectailuun pelkkä git bisect ei enää riitä; esim. Android-käyttöjärjestelmä [koostuu sadoista gittirepoista][4]. Sama idea voi silti toimia jos koko projektin historian tiloihin pääsee käsiksi.

[1]: https://git-scm.com/docs/git-bisect
[2]: http://yarchive.net/comp/linux/git_bisect.html
[3]: https://en.wikipedia.org/wiki/Binary_search_algorithm
[4]: https://android.googlesource.com/platform/manifest/+/master/default.xml
