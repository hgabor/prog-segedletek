---
layout: post
title:  "Adattervezés"
date:   2018-09-26 10:00:00 +0100
tags: 14evfolyam oop adatbazis programozas
---

A program megvalósításának egyik leggyakoribb kezdőlépése, hogy difiniáljuk, milyen adatokkal szeretnénk dolgozni, mit szeretnénk tárolni - vagyis milyen osztályaink, milyen objektumaink legyenek. Ezt egy példán keresztül mutatom be.

## Feladat

Egy kisvasút menetrendjét szeretnénk tárolni. Kíváncsiak vagyunk arra, hogy melyik járat melyik állomásra mikor érkezik. Az adatokat egy szimpla webes felületen szeretnénk megjeleníteni (amit aztán ki is lehet nyomtatni, hogy az állomásokon is legyen belőle egy példány).

Egyéb extra funkciókkal (pl. online jegyvásárlás, késések adminisztrálása) nem kell foglalkozni.

## ER-diagram

A legegyszerűbb eszköz az adattervezésre az **ER (Entity-relationship - Egyed-kapcsolat) diagram**. A diagramon három fontos dolog jelenik meg:

* Milyen **entitásokkal** dolgozunk (entity)
  * Rokon értelmű szavak: objektum, dolog, izé, bigyó
* Az entitásokkal kapcsolatban még mit szeretnénk eltárolni, ezek a **tulajdonságok**, vagy **attribútumok** (attribute)
* Az entitások milyen **kapcsolatban** állnak egymással (relationship)

Az ER-diagramot könnyű akár papíron is elkészíteni, mivel csak egyszerű elemekből és vonalakból áll.

### Tervezés

Első lépésnek defináljuk az entitásokat és a tulajdonságokat:

* Mivel több különböző járat szerepelhet a programban, ezért valószínűleg a *járat* egy entitás lesz.
  * A járat azonosítására egy *járatszám*-ot használhatunk
  * Extra, hasznos információként meg eltárolhatjuk a kapacitást is (hány utas tud szállítani)
* Állomásból is lehet több, ezért az *állomás* is entitás lesz
  * Az állomásnak van *neve*, ami idálisan egyedi
  * Eltárolhatjuk még, hogy az állomáson lehet-e jegyet vásárolni
* A két entitás összekapcsolódik, amit *érinti*-nek nevezhetünk el
  * Kapcsolatnak is adhatunk tulajdonságot, ebben az esetben az időpontot
  * A kapcsolat *kardinálitása* (számossága) **több a többhöz** / **N:M** lesz, mivel egy állomást több járat is érint, illetve egy járat több álomáson is megáll.

![ER-diagram](/assets/img/er.svg)

### Egy a többhöz kapcsolat

Más esetekben a kapcsat **egy a többhöz** / **1:N** lesz, mint az alábbi példában:

![ER-diagram 1:N kapcsolattal](/assets/img/er2.svg)

Egy vonat ki tud szolgálni több járatot is (természetesen nem egyszerre), de egy járat mindig egy vonatból áll.

### Csak a probléma szempontjából fontos elemeket

A tervezés sokmindent leírhatunk, ami egy kisvasút szempontjából releváns:

* A járatot kiszolgáló vonatokat
* A vonatok állapotát, és hogy mikor kell őket legközelebb szervízelni
* A járaton éppen aktuálisan utazó utasok számát (amit valós időben akár lehet mérni is)
* ...

Fontos mindig észben tartani, hogy a probléma megoldásához mi szükséges, és mi nem, hogy ne tervezzünk olyan rendszert, ami potenciálisan megold minden jövőbeli problémát - amivel aztán sosem leszünk kész, és akár az ügyfél büdzséjén is túlmehet.

> Hofstadter törvénye: Minden hosszabb időt vesz igénybe, mint várnád, még akkor is, ha figyelembe veszed Hofstadter törvényét.
>
> – Douglas Hofstadter: Gödel, Escher, Bach: Egybefont gondolatok birodalma.

A továbbiakban csak az első ábrán lévő elemeket fogjuk használni.

#### Hogyan tovább?

Az ER-diagram egy remek kiindulópont, de közvetlenül még nem tudjuk felhasználni. Ezért a következő lépés az alkalmazás jellegétől függően az adatbázis (és diagram), az osztály (és a diagram), vagy akár mindkettő lesz.

### Az adatbázisleírás

A fenti ábrából három adattábla keletkezik:

* Járat
* Állomás
* Érinti (kapcsolótábla)

Bár alkalomszerűen lesz kivétel, általánosságban igaz, hogy:

* Az entitásoknak a táblák felelnek meg
* Az attribútumoknak az oszlopok
* 1:N kapcsolatoknak az idegen kulcsok
* N:M kapcsolatoknak a kapcsolótáblák

A fenti példából pl. az alábbi adatbázist írhatjuk le:

<pre><code class="sql">CREATE TABLE jarat (
    id INTEGER PRIMARY KEY AUTO_INCREMENT,
    jaratszam VARCHAR(100) UNIQUE,
    kapacitas INTEGER NOT NULL
);

CREATE TABLE allomas (
    id INTEGER PRIMARY KEY AUTO_INCREMENT,
    nev VARCHAR(150) NOT NULL UNIQUE,
    jegypenztar BOOLEAN NOT NULL
);

CREATE TABLE erinti (
    jarat_id INTEGER NOT NULL,
    allomas_id INTEGER NOT NULL,
    ido TIME NOT NULL,
    PRIMARY KEY (jarat_id, allomas_id),
    FOREIGN KEY (jarat_id) REFERENCES jarat(id),
    FOREIGN KEY (allomas_id) REFERENCES allomas(id)
);</code></pre>

Vizuálisan:

![Adatbázis terv](/assets/img/tervezes_db.png)

Érdemes megfigyelni az extra elemeket, amik az SQL kódban szerepelnek, de az eredeti ER-diagramon nem:

* Az id mezők
* Idegen kulcsok
* Adattípusok, egyéb megkötések

Ez az egyik fő ok, hogy a kezdeti tervezéshez egy egyszerűbb ábrázolási módot használunk - a kötelező, de nyilvánvaló részletek ne lassítsanak, és terheljenek minket, amikor még nem fontosak.


