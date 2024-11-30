---
title:  "Tesztelési módszerek"
date:   2019-03-06 10:00:00 +0100
tags:
 - 14evfolyam
 - teszteles
 - modszertan
---

Két fő fogalmat kell tisztázni, mielőtt a tesztelés módszereibe belekezdenénk:

* **Verifikáció:** Amit elkészítünk, az helyesen működik-e?
* **Validáció:** A jó szoftvert készítjük-e, megoldja-e az ügyfél/felhasználó problémáját?

A _validációt_ akár már a tervezési fázisban, még az első programkódsor leírása előtt el tudjuk kezdeni. _Verifikálni_ viszont értelemszerűen csak elkészült programot lehet.

A tesztelési módszereket sokféleképp lehet csoportosítani, és ideálisan mindegyiket használjuk a megfelelő szinteken:

## Automatikus vs. Kézi

* Automatikus: a teszteket számítógép értékeli ki, és egyértelmű visszajelzést ad a sikerről.
  * Előny:
    * Könnyen megismételhető
    * Akár nagyon sok tesztesetet is végig tud nézni
    * Tesztelés közben (ideális esetben) nem hibázik
  * Hátrány:
    * Teszteseteket pontosan meg kell fogalmazni
    * Nem minden esetben alkalmazható
* Kézi: a teszteket ember végzi el.
  * Előny:
    * Olyan hibákra is ráakadhatunk, amelyeket előre nem specifikáltunk
    * "Szubjektív" hibákat is lehet ellenőrzni (pl. grafikus felület átláthatósága)
  * Hátrány:
    * Az emberi erőforrás drága
    * Tesztelés közben nagyobb a hiba esélye, főleg repetitív feladatoknál

## Szisztematikus vs. ad-hoc

* Szisztematikus
  * A teszteseteket előre definiáljuk, és előre megadott módszerrel értékeljük ki
  * Lehet automatikus vagy kézi
* Ad-hoc
  * A tesztelőnek szabad kezet adunk, nincsenek instrukciók
  * Csak kézi

## Feketedoboz vs. fehérdoboz

* Feketedoboz:
  * Nem ismerjük a szoftver belső felépítését
  * Adott bemenetre adott kimenetet kell adni, a belső felépítést nem nézzük
* Fehérdoboz:
  * A bemenetet úgy választjuk meg, hogy minden elágazás, ciklus, belső függvény stb. legyen tesztelve
  * [A kód-lefedettséget](https://en.wikipedia.org/wiki/Code_coverage) IDE-eszközökkel lehet mérni

## Tesztelési szintek

### Egységtesztelés

Egyetlen osztályt, függvényt tesztelünk, a programtól izoláltan.

* Automatikus
* Szisztematikus
* Fehérdoboz
  * Lehet kombinálni code coverage eszközökkel lehet fehérdoboz is, pl. a siker feltétele, hogy a tesztesetek 100%-os lefedetséget produkáljanak

[Egységtesztelés a gyakorlatban, NUnit segítségével]({% post_url 2019-03-02-nunit %})

### Integrációs tesztelés

Több komponenst együtt vizsgál, és azt dönti el, hogy az adott komponensek interfésze valóban megfelel-e a secifikáltaknak, és együtt is jól működnek-e.

* Többnyire automatikus
* Többnyire szisztematikus
* Fektetedoboz: a komponenseket kívülről vizsgáljuk

### Rendszerteszt

A teljes rendszert egészben, az összes komponensével vizsgáljuk, az éles környezettel megegyező körülmények közt.

* Többnyire kézi
* Szisztematikus és ad-hoc
* Feketedoboz

### Elfogadási teszt, végtesztelés

A megrendelő, ügyfél végzi, hogy az elkészült rendszer valóban megfelel-e a specifikáltaknak.

A többi teszttel ellentétben (amelyeket a fejlsztés során folyamatosan végzünk) ez általában egyszer, a szoftver átadásakor történik meg.

## Egyéb gyakori teszt-típusok

### Regressziós tesztelés

Nincs rendszer hiba nélkül. Ha egy hibát javítunk, akkor felvehetünk hozzá egy tesztesetet (egységtesztet, egy új esetet a kézi teszforgatókönyvbe stb.), hogy a későbbiekben ugyanez a hiba ne jöjjön elő.

Ezeket a teszteseteket hívjük összefoglaló néven regressziós teszteknek. Nem egy konkrét tesztelési szint, az egységteszttől a rendszertesztig bárhol jelentkezhet.

### Alfa- és béta-teszt

Az elfogadási/végtesztetlést el lehet végezni a rendszer elkészülte előtt is, hogy meggyőződjük a rendszer készültségéről, ismert hibáiról.

Hagyományosan alfa-tesztelésnek hívjuk, ha a tesztelést cégen belül végezzük, és béta-tesztelésnek, ha már egy zárt körű, de valódi felhasználók végzik.

A béta-tesztelés fogalma mára kezd kissé elmosódni, elég gyakori mindkét eset:
* A szoftver évekig béta-verzióban található, mert a fejlesztőcsapat nem meri rámondani, hogy készen van.
  * A Gmail 2004 áprilisától 2009 júliusáig volt béta verzióban, **5 évig**
  * Lásd az "örök béta" ([Perpetual beta](https://en.wikipedia.org/wiki/Perpetual_beta)) fogalmát
* A szoftvert kellő tesztelés nélkül adják ki, teli hibával, amelyek csak a későbbi frissítések során lesznek csak kijavítva
  * Ez általában annak a jele, hogy amit kiadtak, az valójában csak a béta-verzió lenne
  * Sokan ennek tartják a "Fallout 76" játék kezdeti kiadását

### Nem funkcionális tesztelés

Azok a tesztek, amelyek nem a helyes / helytelenséget, hanem az egyéb paramétereket vizsgálják:

* Stressz-tesztelés: erőforrásigény mérése, extrém mennyiségű bemenet (pl. egyszerre sok felhasználó, nagy mennyiségű adat stb.) kezelése
* Biztonsági tesztelés: hálózati, webes alkalmazások esetén kiemelkedően fontos
* Használhatósági teszt: a felhasználói interfész könnyen kezelhető, megérthető
  * Akadálymentesítés:
    * Az alkalmazás használható-e képernyő-felolvasó szoftverekkel
    * Nem okoz-e problémát színtévesztőknek
    * Támogat-e nagy kontrasztú módot
    * Használható-e csak billentyűzettel / csak egérrel
    * ...

## Folyamatos tesztelés (continuous testing)

Az automatikus teszteket nem csak igény szerint, a fejlesztő indíthatja el, hanem commitoláskor, vagy bizonyos időközönként automatikusan lefuthatnak.

* [A hivatalos C# fordító](https://github.com/dotnet/roslyn) - a tesztek rendszeresen lefutnak, és a kimenetet egy zöld succeeded / piros failed üzenet jelzi a táblázatban
* A [Mozilla Firefox böngésző legfrissebb változatából](https://www.mozilla.org/en-US/firefox/channel/desktop/#nightly) naponta 2x készül egy újabb változat. Ezt szokás "nightly", éjszakai verziónak hívni. A fordítás, tesztelés sok esetben több órás feladat, ezért szokás éjszakára beütemezni, hogy a fejlesztőknek ne napközben kelljen várni rá. Ha délután befejezte a munkáját, másnapra a teljes böngészővel együtt végezheti az integrációs / rendszertesztet.

## Dokumentumok

A kézi szisztematikus teszteléshez, azért hogy a tesztelő tudja, mit is kell vizsgálnia, két dokumentum szükséges:

* Tesztforgatókönyv, amely leírja, hogy pontosan hova kell kattintani, mit kell begépelnie a tesztelőnek
  * Szerepel benne az elvárt, helyes kimenet is
* Tesztjegyzőköny, amit a tesztelő készít a forgatókönyv alapján, szerepel benne:
  * Ki, mikor, hol, melyik eszközön végezte a tesztelést
  * Az elvégzett teszteket, azok tényleges kimenetét, és a teszt értékelését (helyes / helytelen)

Ha a jegyzőkönyvet sablonként készítjük el, a két dokumentum kombinálható is.

Jegyzőkönyvet készíthetünk ad-hoc teszteléshez is, ebben az esetben a jegyzőkönyvben kell szerepelnie az elvégzett lépéseknek, az elvárt és a tényleges kimenetnek.

## További olvasnivalók

Ez csak egy ízelítő a tesztelés témaköréből. Rengeteg teszmódszer létezik, és bármely említett elemet még ki lehetne fejteni részletesebben.

* [Egységtesztelés NUnit segítségével]({% post_url 2019-03-02-nunit %})
* [Wikipedia: Software testing](https://en.wikipedia.org/wiki/Software_testing)
