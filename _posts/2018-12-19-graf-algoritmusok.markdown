---
layout: post
title:  "A fontosabb gráf-algoritmusok"
date:   2018-12-18 11:00:00 +0100
tags: 14evfolyam oop algoritmusok adatszerkezetek
---

A gráfokon különféle algoritmusokat végezhetünk el, ehhez pedig először egy konkrét, objektum-orientált adatszerkezetet kell definiálnunk:

![OOP gráf](/assets/img/graf-diagram.png)

A gráf külső iterfészét a *Graf* osztály fogja jelenteni. Az ehhez tartozó metódusokkal fogjuk a gráfot kezelni, az algoritmusokat lefuttatni - a gráf használójának nem kell tudni a belső felépítésről, a Csucs és El osztályokról.

A tárolás módja _éllista_ lesz, ahol minden élt az *El* osztály egy példánya fogja jelenteni. A könnyebb kezelhetőség kedvéért az élt úgy tároljuk el, hogy _mindkét irányt_ felvesszük, vagyis ha szerepel az 1-3 él, akkor a 3-1 is szerepelni fog.

Emellett még eltároljuk a gráf csúcsait is egy-egy objektumban.

Ezt a két osztályt a későbbiekben ki lehet egészíteni, ha egy-egy élhez vagy csúcshoz még további adatot szeretnénk eltárolni.

A gráf *egyszerű* és *irányítatlan* lesz, de egyéb adatokkal (pl. súly, szín) ki lehet egészíteni.

A kontruktorban előre meg kell adni a csúcsok számát, ez később nem módosítható. Kezdetben a gráf egy élt sem tartalmaz.

A *hozzad* függvény segíségével adhatunk hozzá új éleket, a csúcs indexek (0 ... N-1) megadásával. Törlésre jelenleg nincs mód.

A fenti osztályt az alábbi három példa-projekt meg is valósítja:

* [C# megvalósítás](https://github.com/hgabor/GrafFeladat_CSharp)
* [Java megvalósítás](https://github.com/hgabor/GrafFeladat_Java)
* [PHP megvalósítás](https://github.com/hgabor/GrafFeladat_PHP)

## Bejárás

Egy gráfot kétféleképp lehet bejárni. Kiválasztunk egy kezdőpontot, aztán:

* Széllességben folytatjuk
  * Először megvizsgáljuk a pont összes szomszédját
  * Majd azoknak az összes szomszédját
  * Stb.
  * [Ezen az oldalon az első ábra rendkívül jól illusztrálja](http://tamop412.elte.hu/tananyagok/algoritmusok/lecke24_lap1.html)
* Mélységben folytatjuk
  * Először megvizsgáljuk a pont **egy** szomszédját
  * Majd annak **egy** szomszédját
  * Ha nem tudunk tovább menni, **visszalépünk**, és a pont egy másik szomszédjával folytatjuk
  * Ha elfogy, megint visszalépünk
  * Ha a kezdőponból is visszalépnénk, végeztünk
  * [Ezen az oldalon az első pár ábra rendkívül jól illusztrálja](http://tamop412.elte.hu/tananyagok/algoritmusok/lecke29_lap1.html)

### A szélességi bejárás algoritmusa

A szélességi bejárásnál az elemeket egy _sor_ adatszerkezetbe fűzzük, majd ebből kivéve vizsgáljuk az elemeket és a szomszédjaikat.

<pre>
Gráf.SzelességiBejár(kezdopont: egész):
    // Kezdetben egy pontot sem jártunk be
    bejárt = új üres Halmaz()

    // A következőnek vizsgált elem a kezdőpont
    következők = új üres Sor()
    következők.hozzáad(kezdőpont)
    bejárt.hozzáad(kezdőpont)

    // Amíg van következő, addig megyünk
    Ciklus amíg következők nem üres:
        // A sor elejéről vesszük ki
        k = következők.kivesz()

        // Elvégezzük a bejárási műveletet, pl. a konzolra kiírást:
        Kiír(k)

        Ciklus él = this.élek elemei:
            // Megkeressük azokat az éleket, amelyek k-ból indulnak
            // Ha az él másik felét még nem vizsgáltuk, akkor megvizsgáljuk
            Ha (él.csúcs1 == k) és (bejárt nem tartalmazza él.csúcs2-t):
                // A sor végére és a bejártak közé szúrjuk be
                következők.hozzáad(él.csúcs2)
                bejárt.hozzáad(él.csúcs2)

        // Jöhet a sor szerinti következő elem
</pre>

### A mélységi bejárás bejárás algoritmusa (1. változat)

Szinte teljes mértékben megegyezik a szélességi bejárással. A különbség, hogy a sor helyet _verem_ adatszerkezetbe tesszük a pontokat:

<pre>
Gráf.MélységiBejár(kezdopont: egész):
    // Kezdetben egy pontot sem jártunk be
    bejárt = új üres Halmaz()

    // A következőnek vizsgált elem a kezdőpont
    következők = új üres Verem()
    következők.hozzáad(kezdőpont)
    bejárt.hozzáad(kezdőpont)

    // Amíg van következő, addig megyünk
    Ciklus amíg következők nem üres:
        // A verem tetejéről vesszük le
        k = következők.kivesz()

        // Elvégezzük a bejárási műveletet, pl. a konzolra kiírást:
        Kiír(k)

        Ciklus él = this.élek elemei:
            // Megkeressük azokat az éleket, amelyek k-ból indulnak
            // Ha az él másik felét még nem vizsgáltuk, akkor megvizsgáljuk
            Ha (él.csúcs1 == k) és (bejárt nem tartalmazza él.csúcs2-t):
                // A verem tetejére és a bejártak közé adjuk hozzá
                következők.hozzáad(él.csúcs2)
                bejárt.hozzáad(él.csúcs2)

        // Jöhet a sor szerinti következő elem
</pre>

### A mélységi bejárás bejárás algoritmusa (2. változat)

A verem helyett a _hívási stack_-et is felhasználhatjuk, így egy könnyebben megérthető, rekurzív algoritmust kapunk:

<pre>
// A fő függvény, amivel a rekurziót elindítjuk
Graf.MélységiBejár(kezdőpont: egész):
    bejárt = új üres Halmaz()
    bejárt.hozzáad(kezdőpont)
    this.MélységiBejárRekurzív(k, bejárt)

// Segédfüggvény, amely magát a rekurziót végzi
Gráf.MélységiBejárRekurzív(k: egész, bejárt: Halmaz):
    // Elvégezzük a bejárási műveletet, pl. a konzolra kiírást:
    Kiír(k)

    Ciklus él = this.élek elemei:
        // Megkeressük azokat az éleket, amelyek k-ból indulnak
        // Ha az él másik felét még nem vizsgáltuk, akkor megvizsgáljuk
        Ha (él.csúcs1 == k) és (bejárt nem tartalmazza él.csúcs2-t):
            bejárt.hozzáad(él.csúcs2)
            Gráf.MélységiBejárRekurzív(él.csúcs2, bejárt)
</pre>

## Összefüggőség

Az összefüggőség vizsgálatához a gráfot be kell járni egy tetszőleges pontból, tetszőleges módszerrel. Ha minden elemet bejártunk, akkor a gráf összefüggő.

Úgy is elképzelhetjük, mint egy képszerkesztő alkalmazásban a _flood fill_ eszközt: ha kimarad egy elem, akkor nem összefüggő, ha pedig az egészet sikerül beszínezni, akkor összefüggő.

Alakítsuk át egy picit mondjuk a szélességi bejárás algoritmusát:

<pre>
Gráf.Összefüggő(): logikai
    bejárt = új üres Halmaz()

    következők = új üres Sor()
    következők.hozzáad(0) // Tetszőleges, mondjuk 0 kezdőpont
    bejárt.hozzáad(0)

    Ciklus amíg következők nem üres:
        k = következők.kivesz()

        // Bejárás közben nem kell semmit csinálni

        Ciklus él = this.élek elemei:
            Ha (él.csúcs1 == k) és (bejárt nem tartalmazza él.csúcs2-t):
                következők.hozzáad(él.csúcs2)
                bejárt.hozzáad(él.csúcs2)
    
    // A végén megvizsgáljuk, hogy minden pontot bejártunk-e
    Ha bejárt.elemszám == this.csúcsokSzáma:
        Vissza: igaz
    Különben:
        Vissza: hamis
</pre>

## Feszítőfa készítése

A feszítőfa egy olyan fa, amely a pontosan az eredeti gráf pontjait tartalmazza, de az elék közül csak azokat, amelyekkel még fa (azaz körmentes) lesz a gráf.

Az algoritmus módszere, hogy egy bejárási algoritmus lényegében egy fát ír le - mindössze meg kell jegyeznünk, hogy melyik él mentén haladtunk végig. Ha olyan pontba futunk, amit már vizsgáltunk, akkor azt az élt már nem vesszük figyelembe, mert kört okozna.

Az algoritmus nyilván csak akkor ad helyes eredményt, ha a gráf összefüggő.

<pre>
Gráf.Feszitőfa(): Gráf
    // Új, kezdetben él nélküli gráf
    fa = új Gráf(this.csúcsokSzáma)

    // Bejáráshoz szükséges adatszerkezetek
    bejárt = új üres Halmaz()
    következők = új üres Sor()

    // Tetszőleges, mondjuk 0 kezdőpont
    következők.hozzáad(0)
    bejárt.hozzáad(0)

    // Szélességi bejárás
    Ciklus amíg következők nem üres:
        k = következők.kivesz()

        Ciklus él = this.elek elemei:
            Ha él.csúcs1 == aktuálisCsúcs:
                Ha bejárt nem tartalmazza él.Csúcs2-t
                    bejárt.hozzaad(él.csúcs2)
                    következők.hozzáad(él.Csúcs1)
                    // A fába is vegyük bele az élt
                    fa.hozzaad(él.Csucs1, él.csúcs2)

    // Az eredményül kapott gráf az eredeti gráf feszítőfája
    vissza: fa
</pre>

