---
layout: post
title:  "Hogyan tartsunk verziókezelés órát?"
date:   2018-09-01 10:00:00 +0100
tags: 14evfolyam-tanar verziokezeles
---

## Cél

Az óra célja, hogy a diákok biztosan tudják kezelni a git verziókezelő rendszer alapfunkcióit:

* Új projekt/repó létrehozása, létező klónozása
* Commit, push, pull
* Konfliktusok feloldása

Ha ezekkel az ismeretekkel rendelkeznek, az nem csak azért fontos, mert előnybe helyezi őket a többi frissen végzett diákkal szemben, hanem a saját munkákat is megkönnyítjük vele: a házi feladatok, záródolgozatok begyűjtésekor nem kell zip fájlokkal szórakozni, harcolni a levelezőrendszer fájlméret korlátaival és a becsomagolt .exe fájlokkal, valamint ha a diák nem küld levelet / fenn akad a spam szűrőn, de a feladat fent van a github oldalán, akkor is lesz értékelhető munkája (és nem tud kifogással élni, hogy "Én pedig elküldtem"). A saját tapasztalatom az volt, hogy kis gyakorlással a fentieket minden 14-es diák képes volt készségszinten elsajátítani.

A választásom azért a git-re esett, mert az eddigi tapasztalatom alapján mind céges, mind hobbi környezetben a leggyakrabban használt, és az elosztott megvalósításnak köszönhetően akár internetelérés nélkül is használható. Mind a NetBeans, mind a Visual Studio beépített támogatást nyújt hozzá. Nekem személyesen kevesebb félelmem van azzal kapcsolatban, hogy elveszne a munkám, mint amikor még SVN-t használtam.

## Tanári alapismeretek

Első körben tisztában kell lennünk a verziókezelő rendszer működésével. Amit érdemes átolvasni:

* [Git bevezető, összefoglaló segédlet]({% post_url 2018-02-21-git-bevezeto %})
* [Kicsit hosszabb/részletesebb, angol nyelvű leírás](https://try.github.io/)

Ha már már más verziókezelőt (pl. SVN) ismerünk:

* [Git - SVN Crash Course](https://git-scm.com/course/svn.html)

Emellett ismerkedjünk meg a github használatával.

## Előkészületek

### Az órai feladat

Készítsünk egy egyszerű példa-projektet. A szintje legyen bőven a diákok készségszintje alatt - azzal ne kelljen időt tölteni, hogy magát a projektet magyarázzuk. Pl. egy grafikus alkalmazás, amelyben a gombra kattintásra a felirat színt vált.

Két ilyen példa-projekt:

* [PHP](https://github.com/hgabor/petrik-2017-13t-kozos)
* [C#](https://github.com/hgabor/petrik-2017-14s-kozos)

Készítsük el a kezdeti projektet. Tartalmazzon két commit-ot:

* Initial commit - tartalmazza a .gitignore és a LICENCE fájlokat (ez jó esetben létrejön, amikor github-on létrehozzuk a repót).
* Alap fájlok - a projekt kezdeti állapota

Találjunk ki egy olyan feladatot, amely csak egy-két sor módosítást igényel. A fenti példánál maradva, változtassuk meg a felirat színét, mert az ügyfélnek nem tetszik.

A projektet push-oljuk github-ra.

A tárgyalni kívánt környezetet (pl. C#-ban vagy PHP-ben dolgozzunk-e) én a diákokra bíztam, de ez a tanár szakterületétől függően nem biztos, hogy mindig megoldható.

### Github profilok

Adjuk ki "feladatnak" előre, hogy minden diák regisztráljon github-ra. A profil URL-eket gyűjtsük be pl. egy google form segítségével.
Ha valaki elfelejti, az óra elején pár perc alatt pótolhatja, de a cél, hogy ezzel ne teljen sok idő.

A profil URL-eket felhasználhatjuk, hogy a jövőben szintén github-on kérjük be a házi feladatokat.

## Elméleti alapozás (részletességtől függően 1-2h)

Ezt célszerű az elméleti órán megejteni a többi csapatmunkát tárgyaló témakörrel együtt. (TODO: csapatmunkás segédlet)

Be kell mutatni a verziókezelés alapfogalmait, ezek hogyan jelennek meg github-on. Érdemes összevetni egy kis projektet (pl. a saját példánkat) egy nagyobbal (pl. [a hivatalos C# fordító](https://github.com/dotnet/roslyn)) rávilágítva, hogy a kettő ugyanarra az alapokra épül.

## Gyakorlat (4-6h)

Az óra elején ismételjük át az alapfogalmakat. 

### Közös, vezetett munka

Klónozzuk a projektet az IDE segítségével. Fordítsuk / futtassuk, hogy lássuk, a klónozás után itt van minden fontos fájl.

Végezzük el a korábban kitalált feladatot. Mutassuk be projekt panelen a jelöléseket/színkódolást, az új/módosított fájlokhoz, ill. a szerkesztőben a kódsoroknál.

Commit-oljuk a változtatásokat. Itt fel kell hívni a figyelmet, hogy a commit csak a saját gépen lévő repóban létezik, github-on nem! Próbáljuk push-olni, látni fogjuk, hogy nem sikerült, jogosultsághiba miatt. Itt be lehet mutatni a github jogosultságkezelését (vagy legalábbis pár szót ejteni róla).

A kivetítőn mi is végezzük el a változtatást, egy kis variációval, ami a diákok feladatától eltér (pl. válasszunk egy másik színt). Fontos, hogy ez olyan legyen, amely pontosan ugyanazt a kódsort érinti!

A diákok végezzenek el egy git pull-t. Konfliktus keletkezett! El kell mondani, hogy ez a csapatmunkának egy rendszeres része lesz, és meg kell tanulni kezelni.

Itt kérjünk pár perc türelmet a diákoktól, mert nálunk nem jelentkezik a konfliktus. Végezzük el az alábbi műveletsort (konzolból a leggyorsabb, mert elég másolni/beilleszteni):

* `git checkout HEAD^` (checkout-oljuk az eggyel korábbi verziót)
* `git checkout -b branch-2` (hozzunk létre egy új branch-et)
* Módosítsuk a válaszott színt egy másmilyenre, mint amit bemutattunk
* `git add .` (adjunk hozzá minden módosított fájlt)
* `git commit -m "Szin modositas"`
* `git merge master` (ezzel mi is ugyanabba a konfliktusba kerülünk, mint a diákok)

Mutassuk be, az IDE hogyan mutatja a konfliktust, és hozzuk fel a konfliktus-feloldó ablakot. Mutassuk be a működését, ill. jegyezzük meg, hogy előfordulhat, hogy vagy a saját, vagy a másik módosítás lesz a helyes, és néha mindkettő vagy egyik sem. A feloldás végeztével commit-oljuk a változtatásokat.

A saját verzióban módosítsunk egy teljesen különböző sort (pl. írjuk át az ablak címét). Mi: commit/push, Diákok: pull. Láthatjuk, hogy mivel nem történt változtatás *ugyanabban a kódsorban*, ezért nincs konfliktus. Ez lesz a helyzet az esetek 90%-ban.

Nézzük meg még:
* A "version history" funkciót, hogy minden commithoz ott van a szerző/idő
* Az annotations / blame funkciót, és hogy mire jó

Foglaljuk össze, hogy éles helyzetben általában ez lesz a munkamenet:
* Pull
* Fejlesztés
* Commit
* Pull ismét, mert lehet, hogy időközben egy másik kolléga és dolgozott
  * Konfliktus-feloldás, ha szükséges
* Push

### Önálló munka

A diákok készítsenek egy saját (üres) projektet github-on. Klónozzák le a saját gépükre. Oldjanak meg egy egyszerű, kiadott feladatot, amit pedig push-oljanak. A kész megoldás tartalmazzon minimum 3 commit-ot:

1. Ami az IDE által legenerált projekt fájlokat tartalmazza (pl. egy üres formot, az nbproject mappát stb.)
2. Ami a megvalósított feladatot
3. A feladaton végezzenek önállóan egy apró módosítást.

Az előkészítést (projekt létrehozása) lehet közösen végezni, ha szükséges.

**Javasolt értékelés:**

* 0.5-ös szorzó
* 5-ös, ha feladatot elkészítette gond nélkül
* 4-es, ha csak 2 commit-ot tartalmaz
* 3-as, ha csak 1-et
* 1-es, ha nem sikerült elkészíteni

(2-es értékelést erre nem tudtam kitalálni, javaslatokat szívesen várok!)
