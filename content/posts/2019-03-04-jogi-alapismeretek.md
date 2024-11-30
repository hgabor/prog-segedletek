---
title:  "Jogi alapismeretek"
date:   2019-03-04 10:00:00 +0100
tags:
 - jog
---

A jogi ismeretek legfontosabb szabálya, hogy **ha bármi nem 100%-osan világos, akkor ügyvédet kell megkérdezni**. Jogi problémák esetében sokat veszthetünk, ha "Úgy hallottam, hogy...", "Szerintem..." típusú tanácsokra hagyatkozunk. Ebbe természetesen az alábbi összefoglaló is beletartozik, és nem tekinthető jogi tanácsadásnak. A leírás pedig csak összefoglaló, nem teljeskörű, és csak a magyar jogrendszerre érvényes.

Ettől függetlenül azért célszerű tisztában lenni a legfontosabb, szoftverfejlesztés során előkerülő jogi szabályokkal:

* Tudjuk, hogy milyen szoftvert, asset-et használhatunk fel, és milyen feltételek mellett
* Tudjuk, hogy a saját termékünket milyen módokon lehet megvédeni.

Itt most három fajta oltalmat fogunk megvizsgálni:

* Szerzői jog
* Védjegy
* Szabadalom

## Szerzői jog (copyright)

A szerzői jog a kreatív alkotásokat védi. Ilyen például egy regény, vers, festmény, és természetesen egy számítógépes program is. Ebbe beleértjük a kész programot, annak a forráskódját, és bármely átmeneti állapotban lévő változatát is.

A szerzői jog azt jelenti, hogy a szerző engedélye nélkül:

* A műről nem szabad másolatot készíteni
* Nem szabad nyilvánosan előadni
* Nem szabad származtatott művet készíteni

Ez alól csak [nagyon kevés, korlátozott esetben](https://hu.wikipedia.org/wiki/Szerz%C5%91i_jog#A_szabad_felhaszn%C3%A1l%C3%A1s) lehet eltérni.

Származtatott mű az, amelynek az eredeti mű a szerves részét képezi, pl.
* Egy zeneszám remix-elt változata
* A zeneszám dalszövege szöveges formában
* Egy regény megfilmesítése
* Egy regény, amely másik regény szereplőit használja
* **Egy program, amely egy másik programot / osztálykönyvtárt használ.**

A szerzői jog **automatikus**, a mű megalkotása pillanatától érvényben van, külön nem kell kérelmezni.

A szerzői jog a (legutolsó) **szerző halála utáni 70 évig** védi a művet, amely ezután közkinccsé válik. Előzetes engedély nélkül, bármilyen célra fel lehet használni. A szerző persze saját maga is közkinccsé teheti a művét.

Gyakori tévhitek:

* A forrásmegjelölés kötelező
  * Csak akkor, ha a szerző előírja. Sok zárt forrású szoftver esetében pl. nem kötelező.
  * Közkincs esetében nincs ilyen előírás.
* Ha a forrás/szerző jelölve van, akkor az ingyenes terjesztés (pl. YouTube-ra feltöltés, saját programban felhasználás) legális
  * A szerző engedélye nélkül nem az.
* Záródolgozat esetében azért tilos a plágium, mert a szerzőtől nem kértünk engedélyt
  * A plágium akkor is tilos, ha a szerzőtől amúgy kaptunk engedélyt.
  * A plágium azért tilos, mert a dolgozat célja a saját tudás bemutatása, nem másoké.
* NuGet-tel / más csomagkezelővel letöltött library-t minden gond nélkül fel lehet használni
  * Bár az esetek többségében a felhasználás valóban ingyenes, a licencfeltételeket attól még be kell tartani, pl. a szerző megjelölését.
* Ötletet le lehet védetni
  * Csak konkrét műveket véd a szerzői jog

Gyakori kérdések és válaszok:  
[https://euipo.europa.eu/ohimportal/hu/web/observatory/faqs-on-copyright-hu](https://euipo.europa.eu/ohimportal/hu/web/observatory/faqs-on-copyright-hu)

A Berni Egyezmény szerint a szerzői jog [a világ szinten minden országában](https://en.wikipedia.org/wiki/Berne_Convention#/media/File:Berne_Convention_signatories.svg) érvényes és gyakorolható (bár apró eltérések lehet a konkrét törvényekben, pl. az oltalom hosszát illetően), és egymás szerzői műveit elismerik.

### Licenc(szerződés)

A licencszerződés akkor készül, ha a szerző másokat szeretne a fenti jogokkal, vagy egy részével felruházni. Ahhoz, hogy egy regényt ki tudjon adni, ahhoz például szükséges a nyomdának, a kiadónak, a könyvesboltoknak engedélyt adni a sokszorosításra.

Hasonlóan, ha egy szoftvert "megveszünk", akkor nem kapjuk meg automatikusan az összes jogot, csak azokat, amelyek a licencszerződésben szerepelnek.

Licencszerződés készülhet egyoldalúan:

* Egy átlag felhasználó vagy elfogadja a feltételeket, vagy nem veszi meg / nem használja a szoftvert.
  * Szabad szoftverek esetében is

De lehet személyre szabott is:

* Pl. Megrendelésre készült szoftver esetén
* Több szabad szoftvernek van zárt forrású változata is, ahol a szabad változat ingyenes, a nem nyílt változat fizetős

A szabad szoftvereket részletesebben egy külön bejegyzés alatt tárgyalom. (TODO)

## Védjegy (trademark)

Termékek, szolgáltatások **megkülönböztetésére** szolgál, pl.: márka, logó. A célja, hogy más nem használhassa ugyanazt, vagy hasonló megnevezést, jelölést a vásárlók megtévesztésére.

A védelemet kérni kell, **nem automatikus**, és folyamatos díja van. Nincs maximális ideje.

Az, hogy milyen termék/szolgáltatás eshet védjegyoltalom alá, és milyen szövegre, ábrára, hangra, alakzatra stb. adható meg a védjegyoltalmat, rendkívül részletes. Irányelvként:
* Nem lehet fajtanév, pl. "Alma" márkájú gyümölcs
  * De [számítógép](https://en.wikipedia.org/wiki/Apple_Inc.), vagy [zenekiadó](https://en.wikipedia.org/wiki/Apple_Records) igen
* Termékkategóriánként lehet ugyanaz a levédett kifejezés, amennyiben nincs félreértésre adó ok
  * Az Apple példára ez is igaz
  * Senki nem fogja összekeverni az [operációs rendszert](https://en.wikipedia.org/wiki/Unix) az [autóalkatrészekkel](https://www.unixauto.hu/)
* El lehet veszteni, többek közt:
  * Nem használat esetén
  * Köznévvé válás esetén, pl. a bakelit, cellux, celofán, kuka, walkman szavak eredetileg mind védjegyek voltak.
    * [Egy részletesebb lista](https://hu.wikipedia.org/wiki/Fajtan%C3%A9vv%C3%A9_v%C3%A1lt_v%C3%A9djegy#P%C3%A9ld%C3%A1k_fajtan%C3%A9vv%C3%A9_v%C3%A1lt_v%C3%A9djegyekre)

A védjegyoltalmat minden országban külön-külön kell kérelmezni (nemzetközi egyezmények előfordulhatnak, pl. az EU esetében).

## Szabadalom (patent)

Szabadalmaztatni **új**, gyakorlatban is alkalmazható **találmányokat** lehet.

Azt, hogy pontosan, mit értünk újnak és találmánynak, több oldalon keresztül lehetne tárgyalni, és a végső döntést az adott ország szabadalmi hivatala (itthon a Szellemi Tulajdon Nemzeti Hivatala) hozza meg.

Ami számunkra fontos, a szoftver, önmagában nem szabadalmaztatható, mert a programozás a matematika egy alkalmazott ága, ezért nem minősül találmánynak. Azonban szoftver által vezérelt találmány egészében már szabadalmaztatható:
* Önvezető autó (szoftverrel együtt)
* Új rendszerű kazán (a vezérlővel együtt) stb.

Ha mégis sikerült egy személynek / cégnek szoftvert szabadalmaztatni, az az esetek többségében az USÁ-ban történik, és mint az egyik legnagyobb piac, nem mehetünk el mellette. A szoftverszabadalmak oka kettős:

* Az USÁ-ban találmányok mellett ún. _üzleti jellegű eljárás_-t is lehet szabadalmaztatni, amelyet sokan használnak a tisztán szoftveres szabadalmak "elbújtatására"
* A szabadalmi hivatal nem feltétlen tudja megállapítani, hogy mi számít újnak, és mi nyilvánvalónak, így sok érvénytelen szabadalom keletkezik, amelyeket csak hosszas bírósági útan lehet csak érvényteleníteni

Ezért ha itthon nem is, amerikában tevékenykedő cégek esetében a szoftvert is érdemes megpróbálni szabadalmaztatni.

A szabadalom természetesen **nem automatikus**, kérvényezni kell, és éves díja van. Az oltalmi idő **maximálisan 20 év**.

A szabadalom oltalmazásáért a tulajdonos kizárólagos jogot kap a találmány hasznosítására, még akkor is, ha tőle függetlenül egy másik feltaláló ugyanazt megvalósítja. Cserébe a szabadalmat nyilvánosságra kell hozni, és a védelem a 20 év leteltével megszűnik.

A szabadalmakat országonként kell kérelmezni.
