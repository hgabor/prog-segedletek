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

