---
layout: post
title:  "Gráfok"
date:   2018-12-18 10:00:00 +0100
tags: 14evfolyam adatszerkezetek
---

A gráf egy olyan adatszerkezet, amely csúcsokból/pontokból, és azokat összekötő élekből áll.

Ez pl. egy gráf:

![Gráf](/assets/img/graf-pelda.svg)

A gráfokat tipikusan olyan helyzetekben alkalmazzuk, amikor különféle dolgok kapcsolatáról beszélünk.

Ez lehet egy metróhálózat:

![Gráf metró](/assets/img/graf-metro.svg)

Vagy diákok, akiknek legjobb barátaik vannak:

![Gráf legjobb barátok](/assets/img/graf-barat.svg)

## Néhány hasznos kifejezés, szakszó

A gráf **összefüggő**, ha bármely pontjából bármely pontjába el lehet jutni.

Ez pl. egy nem összefüggő gráf:

![Nem összefüggő gráf](/assets/img/graf-osszefuggetlen.svg)

A gráf **irányított**, ha ez éleknek van egy kiinduló és egy végpontja. A fenti _legjobb barát_ gráf irányított, míg a _metróhálózat_ irányítatlan.

Egy gráf pedig **súlyozott**, ha az élekhez egy-egy számértéket rendelünk. A _metróhálózat_ gráf súlyozott, míg a _legjobb barát_ súly nélküli.

Egy gráf **egyszerű**, ha nincsenek benne olyan élek, amiknek:

* Ugyanazt a két pontot kötik össze (többszörös él)
* A kezdő- és a végpontja megegyezik (hurokél)

A korábbi példák mind egyszerű gráfok, az alábbi viszont nem:

![Nem egyszeru gráf](/assets/img/graf-nemegyszeru.svg)

Egy gráf pedig **fa**, ha összefüggő, és nincs benne kör - azaz ha egy pontból elindulunk, nem lehet visszajutni anélkül, hogy élen kétszer átmennénk. Érdemes összevetni az [adatszerkezeteknél tanult definícióval]({% post_url 2018-12-02-fa %}) - a kettő végeredményben ugyanazt jelenti, de a megközelítés, a felhasználás módja különbözik. E legelső példa az nem fa, mert tartalmazza az "1-2-3" pontokból álló kört.

Sok más hasznos tulajdonságot is elmondhatunk a gráfokról, itt csak a legfontosabbakat szedtem össze. [Egy teljesebb körű fogalomtár megtalálható itt.](https://hu.wikipedia.org/wiki/Gr%C3%A1felm%C3%A9leti_fogalomt%C3%A1r)

## Tárolási mód

A gráfokat természetesen el kell tárolni valahogy, hogy a számítógép dolgozni tudjon vele. Erre a két gyakran alkalmazott megoldási mód van: a csúcsmátrix és az éllista. Vegyük pl. az első példabeli gráfot:

![Gráf](/assets/img/graf-pelda.svg)

### Csúcsmátrix

Ez egy táblázat, ahol a sorok/oszlopok egy-egy csúcst jelképeznek. A táblázatban 1 szerepel, ha az adott élt összeköti egy él, és 0 (vagy üresen is hagyhatjuk), ha nem.

|       | 1 | 2 | 3 | 4 | 5 |
|:-----:|:-:|:-:|:-:|:-:|:-:|
| **1** |   | 1 | 1 | 1 | 1 |
| **2** | 1 |   | 1 |   |   |
| **3** | 1 | 1 |   |   |   |
| **4** | 1 |   |   |   |   |
| **5** | 1 |   |   |   |   |

Leolvasható, hogy az 1-3 pontokat összeköti egy él, mert a csúcsmátrixban az 1. sor 3. oszlopban szerepel egy 1-es. A 2-4 pontokat viszon nem köti össze, mert a 2. sor 4. oszlopában nincs 1-es.

### Éllista

Szimplán felsoroljuk az éleket:

* 1-2
* 1-3
* 1-4
* 1-5
* 2-3
