---
title:  "Szoftverfejlesztési módszertanok"
date:   2019-03-07 10:00:00 +0100
tags:
 - modszertan
---

Egy szoftver fejlesztésének az életciklusa 4 fő lépésből áll:

* Követelmények összegyűjtése, specifikáció
* Tervezés
* Fejlesztés, megvalósítás
* Tesztelés
* _+1: Karbantartás (az első átadás/kiadás után folyamatos)_

Attól függően, hogy ez a négy elem hogyan kapcsolódik egymáshoz, különféle módszertanokat különböztetünk meg.

A gyakorlatban ahány cég/csapat, annyiféle módszer létezik, ezért most csak legfontosabbakkal, a főbb kategóriákkal ismerkedünk meg.

## Vízesés

A fázisok elkülönülnek egymástól, egymásra épülnek:

![Vízeses modell](/assets/img/modszertan-vizeses.svg)

A naiv elgondolás, hogy ez a módszer a logikus, hiszen hogyan is lehetne elkezdeni a tervezést a specifikáció nélkül, hogyan lehetne tesztelni kész szoftver nélkül stb.

A módszer legnagyobb problémája, hogy arra a feltételezésre épül, hogy az összes megelőző lépés helyes, hibátlan volt. A gyakorlatban azonban sokszor fordul elő, hogy a fejlesztés során találunk tervezési hibákat, sőt, akár specifikációbeli változásokat is. Visszatérni egy előző fázisba viszont azt jelenti, hogy az addig elvégzett munkát ki kell dobni.

Épp ezért, tisztán vízesés jelleggel csak nagyon kevés szoftvert fejlesztenek, főleg olyan területeken, ahol termék elkészülte után nincs lehetőség, vagy nagyon költséges a frissítés. Pl. egy műhold vezérlőszoftverét nem lehet iteratív módon fejleszteni, elsőre jól kell működnie - ez viszont azt is magával vonzza, hogy a specifikációs és a tervezési fázis _sokkal_ több ideig tart és sokkal költségesebb, mint egy átlagos szoftverprojektté.

## Iteratív

A legtöbb szoftver valamilyen iteratív módszerrel készül, vagyis nem egyszerre készül el a végleges szoftver, hanem kisebb, önállóan is használható egységekre kell bontani.

![Iteratív modell](/assets/img/modszertan-iterativ.svg)

Minden iteráció ugyanebből a négy elemből áll, de az időtartamuk rövidebb. Ezért ha pl. a megváltozott specifikáció vagy hibás tervezés miatt újra kell kezdeni a fejlesztést, akkor sokkal kevesebb időt/pénzt pazaroltunk.

Egy iteráció időben változó lehet, pár héttől pár hónapig is terjedhet a szoftvertől függően. Emellett, okos projektmendzsmenttel és verziókezeléssel az egyes funkciókat egymással párhuzamosan, ennél kisebb egységekben is lehet fejleszteni:
* A Windows 10-ből átlagosan fél évente jelenik meg új verzió
  * Ez nem jelenti azt, hogy a fejlesztés egy nagy iterációban történik, hanem sok kisebb iteráció segítségével készülnek az egyes funkciók
    * Amelyeknek van saját tervezési, fejlesztési, tesztelési fázisa
  * A kiadás előtt pedig történik egy nagyobb méretű, átfogó tesztelés

### Agilis módszerek

Az agilis módszertan az iteratív módszerek extrém példája, egy iteráció legfeljebb 2-3 hétből áll. Azt helyezi előtérbe, hogy sok esetben a követelmények _folyamatosan változnak_, és a jövőre nem lehet felkészülni. Ezért a cél, hogy egy működőképes szoftver minél hamarabb készüljön el, és a megrendelői / felhasználói visszajelzések segítsék a további tervezést.

Az egyik legnépszerűbb használati területe a weboldalak és app-ok készítése.

Egy népszerű, agilis módszertan a [Scrum](https://en.wikipedia.org/wiki/Scrum_(software_development)). (megj.: ebben az esetben a magyar nyelvű wikipédia szócikket NEM ajánlom).

Agilis módszerek is vannak limitációi, pl. nem alkalmaztóak olyan helyzetekben:
* ahol a fejlesztésre, tesztelésre szigorú előírások vonatkoznak, mint pl. orvosi eszközök készítése, vagy az autóipar (maga a tesztelés, engedélyeztetés több idő, mint egy iteráció)
* ha a projeknek sok külső függősége van, a csapatoknak egymásra / az ügyfélre kell várniuk
