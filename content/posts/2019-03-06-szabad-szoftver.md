---
title:  "Nyílt forráskód, szabad szoftver"
date:   2019-03-06 11:00:00 +0100
tags:
 - jog
---

Nyílt forráskódú szoftvernek azt nevezzük, amikor a szoftver forráskódja a fejlesztők számára szabadon elérhető, és az módosítható, akár saját programba be is építhető. Köznyelvben a nyílt forráskódú (open-source) és a szabad (free/libre) szoftver fogalma rokon értelmű.

Attól, hogy a forráskód elérhető, még nem lesz a szoftver nyílt: a Windows operációs rendszer forráskódja bizonyos célokra (pl. hardverfejlesztés) bizonyos feltételek mellett (pl. több ezer fős cég, titoktartási nyilatkozat) megtekinthető.

## Szabad ≠ ingyenes

Az angol "free software" kifejezés félrevezető lehet, ezért szokás az "Open source" vagy a "Free/Libre" kifejezést használni.

Szoftver lehet ingyenes, de nem nyílt:
* Visual Studio Community
* Unity Personal

Illetve nyílt forrású szoftverért is lehet pénzt kérni, üzleti célre felhasználni:
* [Ubuntu terméktámogatással](https://buy.ubuntu.com/)
* [Red Hat előfizetés](https://www.redhat.com/en/store/linux-platforms)

## Licencek

A szabad szoftverekhez sokszor nem írnak saját licencet, hanem egy meglévőt használnak fel. Sok esetben ezek a licencek átestek a "tűzkeresztségen", bíróság előtt is megállták a helyüket.

[Egy nem teljes lista gyakran használt szabad szoftver licencekről](https://www.gnu.org/licenses/license-list.html)

### Copyleft

Copyleft licenc alatt azt értjük, hogy a származtatott műveket is ugyanazon a licenc alatt kell kiadni.

Ilyen pl. a GNU GPL: ha GPL-es komponenst használunk a szoftverben, akkor a GPL feltételei szerint ki kell adni a saját programunk forrását is, GPL alatt licencelve.

### Nem copyleft, megengedő

A szoftvert nem kötelező nyílt forrás alatt kiadni, de a legtöbb esetben az eredeti szerzőt meg kell jelölni a származtatott művekben.

Ilyen pl. az MIT licenc, amely alatt a példakódjaim szerepelnek.

Nézzük meg pl. a Chrome böngészőben:
* Help -> "Google Chrome is made possible by the Chromium open source project and other open source software." szövegben a linkre kattintva
* Vagy csak gépeljük be, hogy chrome://credits/

### Nem szoftver licencek

Képek, hanganyagok, dokumentáció esetében nem mindig értelmezhető a "forráskód" kifejezés. Ilyen esetekre a [Creative Commons licenc-családot](https://creativecommons.org/choose/) szokták sokan használni.

## Előnyök, hátrányok

* Több szem többet lát hatás: ha a fejlesztőn, cégen kívül mások is belenézhetnek a forráskódba, akkor a hibákat könnyebben lehet megtalálni és javítani
  * Új funkciókat nem csak az eredeti fejlesztő készíthet
  * [Egy 2018-as rangsor szerint](http://web.archive.org/web/20220818010628/https://www.infoworld.com/article/3253948/who-really-contributes-to-open-source.html) github-on a Microsoft alkalmazottai adtak a legtöbbet nyílt forrású projektekhez.
* A legtöbb esetben ingyenes, vagyis magáncélra történő felhasználásra, kipróbálásra nem kell hatalmas összeget befektetni
  * A Windows Server-nek 180 napos a próbaverziója, utána meg kell venni
  * Unity esetében csak bizonyos bevétel/cégméret alatt ingyenes
* Sok esetben szabad szoftver esetében is lehet terméktámogatást venni, lásd fejlebb

Szabad szoftver azonban nem minden esetben alkalmazható:

* Üzleti titok esetében
  * A szoftver forráskódjából kiolvasható lenne minden
* Ha más, nem szabad szoftvert szeretnénk integrálni, aminek a forrását nem fedhetjük fel
* Ahol a maga a szoftver licencelés a cég bevételi forrása
  * Játékok
  * Adobe Photoshop, Illustrator
  * 3D renderelő, videóvágó szoftverek
  * Ezeknek általában nincs kellően jó minőségű, szabad alternatívája
* Megszokás
  * A LibreOffice gyakorlatilag mindenre képes, amire egy átlagos Office felhasználónak szüksége van

A nyílt forráskód és a szoftverminőség nem függ össze közvetlenül. Rengeteg jó és gyenge minőségű nyílt és zárt szoftver is létezik.

(Egy pont, amiben _általában_ a fizetős szoftverek előbbre járnak, az a User Interface design)
