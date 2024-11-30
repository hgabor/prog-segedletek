---
title:  "Adattervezés"
date:   2018-10-01 10:00:00 +0100
tags:
 - oop
 - adatbazis
 - programozas
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
  * Extra, hasznos információként meg eltárolhatjuk a kapacitást is (hány utast tud szállítani)
* Állomásból is lehet több, ezért az *állomás* is entitás lesz
  * Az állomásnak van *neve*, ami ideálisan egyedi
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

Fontos mindig észben tartani, hogy a probléma megoldásához mi szükséges, és mi nem, hogy ne tervezzünk olyan rendszert, ami potenciálisan megold minden jövőbeli problémát - amivel aztán sosem leszünk kész, és akár az ügyfél határidején, büdzséjén is túlmehet.

> Hofstadter törvénye: Minden hosszabb időt vesz igénybe, mint várnád, még akkor is, ha figyelembe veszed Hofstadter törvényét.
>
> – Douglas Hofstadter: Gödel, Escher, Bach: Egybefont gondolatok birodalma

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

{{< highlight sql >}}
CREATE TABLE jarat (
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
);{{< /highlight >}}

Vizuálisan:

![Adatbázis terv](/assets/img/tervezes_db.png)

Érdemes megfigyelni az extra elemeket, amik az SQL kódban szerepelnek, de az eredeti ER-diagramon nem:

* Az id mezők
* Idegen kulcsok
* Adattípusok, egyéb megkötések

Ez az egyik fő ok, hogy a kezdeti tervezéshez egy egyszerűbb ábrázolási módot használunk - a kötelező, de nyilvánvaló részletek ne lassítsanak, és ne terheljenek minket, amikor még nem fontosak.

### Osztályok

Csak az ER-diagram vagy az adatbázis nem elég ahhoz, hogy a teljes osztályt elkészítsük. Hiányzik néhány nagyon fontos információ - többek közt azt, hogy az osztálynak mi lesz a feladata, milyen műveleteket kell végrehajtania, mit kell kiszámolnia, a kapcsolatok iránya...

De egy félkész terv is jobb, mintha egyáltalán nincs terv.

Hasonlóan az adatbázishoz, itt is lesz pár irányelv, ami kiindulási alapot ad:

* Az entitásoknak az osztályok felelnek meg
* Az attribútumoknak a tagváltozók
* 1:N kapcsolatoknak:
  * A kapcsolat egyik oldalán egy tömb/lista
  * A kapcsolat másik oldalán egy egyedülálló tagváltozó
* N:M kapcsolatokat többféleképp lehet modellezni:
  * Ha szükség van a kapcsolótáblák adataira, akkor a kapcsolótáblának meglfeleltetünk egy új osztályt, és visszavezettük a problémát két 1:N-es kapcsolatra
  * Ha nincs, akkor felvehetünk egy-egy listát a két osztályban, és a kapcsolótábla meg sem jelenik az alkalmazásban.

#### Ha szükség van az "idő"-re

{{< highlight csharp >}}
class Jarat {
    string jaratszam;
    int kapacitas;
    // Egy járat több állomást is érint:
    List<Erinti> erinti;
}

class Allomas {
    string nev;
    bool jegypenztar;
    // Egy állomást több járat is érint:
    List<Erinti> erinti;
}

class Erinti {
    // Egy "érintés" egy-egy járatot és állomást tartalmaz:
    Jarat jarat;
    Allomas allomas;
    DateTime ido;
}

// Felhasználás (generált getterek segítségével):
Console.WriteLine(jarat.Erinti[5].Ido);
Console.WriteLine(jarat.Erinti[5].Allomas.Nev);
{{< /highlight >}}

#### Ha az "idő"-re nincs szükség

{{< highlight csharp >}}
class Jarat {
    string jaratszam;
    int kapacitas;
    // Egy járat több állomást is érint:
    List<Allomas> allomasok;
}

class Allomas {
    string nev;
    bool jegypenztar;
    // Egy állomást több járat is érint:
    List<Jarat> jaratok;
}

// Felhasználás (generált getterek segítségével):
Console.WriteLine(jarat.Allomasok[5].Nev);
{{< /highlight >}}

Az "int id" adatbázismezőket is célszerű lehet még felvenni az osztályokba, amennyiben az osztályok adatai az adatbázisból származnak, és módosítás után oda kerülnek visszaírásra.

Az osztályok tervezése természetesen itt még nem ér véget, de egy erős támpontot ad a továbbiakhoz, a metódusok megtervezéséhez.
