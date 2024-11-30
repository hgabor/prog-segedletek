---
title: "Git bevezető"
date: 2018-02-21 10:00:00 +01:00
tags:
 - verziokezeles
---

*2023-09-28 frissítés: NetBeans-es részeket VS Code-ra cseréltem, a VS képeket frissítettem a 2022-es verzióra, néhány vizsgán előkerülő fogalmat hozzáadtam.*

Egy verziókezelő rendszer az alábbi problémákat oldja meg:

* Több fejlesztőnek kell egyszerre ugyanazon a projekten dolgozni
* Meg akarjuk tartani az időközbeni változásokat is:
  * Könnyebb a hibakeresés
  * Ha szükséges, vissza lehet állni
* A forráskód legfrissebb változata(i) egy központi helyen lesznek elérhetők

A **[git](https://git-scm.com/)** egy a sok verziókezelő rendszer közül, talán a legelterjedtebb. (Népszerű alternatívák még: mercurial, subversion)

A git alapvetően konzolról vezérelt eszköz, de szinte minden grafikus fejlesztőkörnyezet
(Visual Studio, VS Code, Netbeans, Eclipse, IntelliJ, Andorid Studio) támogatja.
Van, amelyik beépítetten, van amelyikhez szükséges a konzolos változat telepítése.

Ebben a leírásban elsősorban az általános fogalmakra, és a GUI-s felületekre koncentrálok.
A git teljes funkciókészletével sokkal többet is lehet kezdeni (pl. több párhuzamos fejlesztési ág kezelése, repó történelmének módosítása, script-ek automatikus futtatása stb.),
ezekbe akkor érdemes beletanulni, ha már az alapvető munkamenetet biztonsággal kezeljük.

## Alapfogalmak

* *Repository (röviden repó):* A projektünk összes korábbi változatát tartalmazza. Gitben ez a projektünk mellett, a <code>.git</code> nevű rejtett mappában található.
* *Commit:* Egy konkrét változtatás, a projektünk egy aktuális állapota.
  A változtatások mellett tartalmazza a szerzőt és a commit létrehozásának időpontját is. A repó lényegében a commit-ok összessége.
  Igeként egy commit létrehozását is jelenti (commit-olni).
* *Munkapéldány (working copy):* A fájlok, amelyekkel éppen dolgozunk. A munkapéldányt megváltoztatva tudunk commit-ot létrehozni.
A munkapéldányt *tisztának* nevezzük, ha nincs benne nem commit-olt változtatás.
* *Checkout:* Kiválasztjuk, hogy a repóból melyik committal szeretnénk dolgozni, és ez lesz az új munkapéldányunk.

![Repó és munkapéldány közti műveletek](/assets/img/repo-wc.png)

## Repók közötti kapcsolat

Bár az egyetlen, helyi gépen tárolt repó is hasznos lehet, a tényleges haszna akkor látszik, amikor van hozzá egy távoli repó is. Git repót tárolhatunk saját szerveren, vagy használhatunk külső szolgáltatásokat is ([GitHub](https://github.com/), [Bitbucket](https://bitbucket.org/) stb.). Az ilyen repókhoz nem tartozik munkapéldány.

* *Clone:* Egy már létező repó lemásolása a saját gépünkre. Ehhez kell a távoli repó URL-je.
* *Fetch:* A beállított távoli repóból lekérdezi az új commit-okat.
* *Pull:* Ugyanaz, mint a fetch, csak extraként beleolvasztja a jelenlegi fejlesztői ágba a távoli módosításokat. (pull = fetch + merge). Csak akkor végezzük el, ha a a munkapéldányunk tiszta - azaz a változtatásokat commit-oljuk vagy dobjuk el.
* *Push:* A helyi repóban lévő commit-okat elküldi a beállított távoli repóba.

Bármely távoli művelet meghiúsulhat, ha nincs hálózati kapcsolat, nem megfelelők a jogosultságok stb.
Egy github repóba pl. nem lehet push-olni, ha a tulajdonos nem adott hozzá minket a fejlesztők közé.

![Repók közötti műveletek](/assets/img/repo-repo.png)

## Konfliktus

Ha a pull/merge során a két változatban ugyanannak a fájlnak ugyanazt a sorát módosítjuk, a verziókezelő nem tudja automatikusan eldönteni, melyik a helyes. Ebben az esetben egy *konfliktus* keletkezik.

A *konfliktus feloldásának* a módja:
* A GUI felsorolja, mely fájlokban történt konfliktus.
* Az adott fájlhoz feldob egy három panelből álló ablakot (ún. *three-way merge*):
  * Egyik oldalon látszik a mi módosításunk
  * A másik oldalon a távoli módosítás
  * Alul vagy középen a végeredmény

![Repók közötti műveletek](/assets/img/konfliktus1.png)

* A menük, ill. a szövegszerkesztő segítségével válasszuk ki, hogy a mi módosításunk a helyes, esetleg a másik, mindkettő, vagy egyik sem. Néha lehet, hogy a kódon utólag kézzel is módosítani kell.

![Repók közötti műveletek](/assets/img/konfliktus2.png)

* Ismételjük, amíg az összes konfliktusos fájl helyes nem lesz.
* Commitoljuk a változtatást

A végeredményképp azt látjuk, hogy a commitok "elágaznak", majd újra "egybeolvadnak":

![Repók közötti műveletek](/assets/img/konfliktus3.png)

## Tipikus munkamenet

* Első alkalommal:
  * Ha van rávoli repó:
    * Klónozzuk
  * Ha nincs:
    * Létrehozunk egy távoli, teljesen üres repót
    * A saját projektünket verziókezelve hozzuk létre
    * A generált projektfájlokat commit-oljuk, a commit üzenet általában "Initial commit"
    * Push-oljuk, beállítjuk a távoli repó URL-jét
* Minden további alkalommal:
  * A munka megkezdésekor pull-olunk, hogy a legfrissebb változattal dolgozzunk
  * Amikor egy egységnyi munkát elvégeztünk, akkor commit-olunk
  * Pull-olunk, hogy időközben történt-e változás
    * Ha emiatt konfliktus keletkezik, akkor azt feloldjuk, a feloldást commit-oljuk
  * Push-olunk
* Ha internethozzáférés nélkül kell dolgozni, akkor csak a pull/push műveletek lesznek elérhetetlenek
  * Commit-olni ugyanúgy tudunk, nem kell amiatt aggódni, hogy a változások elvesznek
  * A következő pullnál feldoljuk a konfliktusokat, ha vannak
  * Push-olunk

## Tipikus UI-k

Előfordulhat, hogy a különböző fejlesztőeszközök másképp csoportosítják, helyezik el a parancsokat,
de a műveleteknek az elnevezése megegyezik - ha ezeket ismerjük, bármely GUI-n el tudunk igazodni.
Itt két példát szeretnék bemutatni, de a többi fejlesztőkörnyezet, ill. külső verziókezelő kliens
(pl. SourceTree, Github Desktop) is hasonlóan működik.

### Visual Studio

Visual Studio-ban a verziókezelési műveleteket fent található "Git" menüben találhatjuk meg.

Alternatívaként, a git-hez kapcsolódó panelek a "View" menü alatt, "Git Changes" és "Git Repository" alatt találhatók meg.

![Git elérhetősége](/assets/img/git_vs_home.png)

Itt tudunk létező repót klónozni, a projektünket verziókezelés alá helyezni, ill. a verziókezelési műveleteket végrehajtani.

Ha az új projektünkhöz helyi repót szeretnénk létrehozni, klónozás nélkül,
akkor a "Create Git Repository" opciót válasszuk ki:

![Új git repó](/assets/img/git_vs_init.png)

Itt eldönthetjük, hogy csak lokális repót szeretnénk, vagy össze szeretnénk kötni valamely népszerű git szolgáltatóval.

Ha a projektünket sikeresen verziókezelés alá helyeztük, itt tudunk commit-okat létrehozni, ill. egyszerű push és pull műveleteket végrehajtani.

![Git lokális műveletek](/assets/img/git_vs_changes.png)

A komplexebb, repók közötti műveletekhez pedig használhatjuk a "Git Repository" ablakot.

### Visual Studio Code

A git repókkal kapcsolatos minden művelet a bal oldali "Source Control" panelen található:

![Repók közötti műveletek](/assets/img/git_vscode_home.png)

A panelen elsősorban a commitolásra szolgál (itt válogathatjuk össze a módosásokat), a további a műveletek
(fetch, push, pull) pedig a felső sorban, ill. a "..." legördülő menüben érhetők el.

A klónozás művelet is itt jelenik meg, ha épp nincs nyitott projektünk.

Ha az új projektünkhöz helyi repót szeretnénk létrehozni, klónozás nélkül,
akkor először hozzuk létre a projektet a keretrendszernek megfelelő módon,
nyissuk meg a mappát VS Code-ban (File / Open Folder...), és ezen a panelen kattintsunk az "Initialize Repository" gombra.

## Egyéb hasznos git fogalmak

* **branch**: Fejlesztői ág. Több okunk is lehet arra, hogy a fejlesztés párhuzamosan fusson:
  * Különböző funkciók párhuzamos fejlesztése
  * A szofter különböző verzióinak a karbantartása (pl. a Windows 11 kiadása miatt nem állt le a 10-es verzió fejlesztése)
  * Kísérleti funkciók létrehozása, amelyek nem biztos, hogy bekerülnek a végleges verzióba
  * stb.
* **diff**: Két commit közötti különbség - mely fájlokban mely sorok hogyan térnek el egymástól.
* **main** v. **master**: A projektünk alapértelmezett fejlesztői ága.
* **origin**: Ha projektünkhöz tartozik távoli repó, ennek a távoli repónak az alapértelmezett megnevezése.
* **log** v. **history**: A múltbeli commit-ok listája.
* **staging area**: Amikor kiválogatjuk, hogy mit szeretnénk commit-olni, először egy képzeletbeli átmeneti helyre tesszük őket. Erre azért van szükség, mert könnyen előfordulhat, hogy nem minden változtatást szeretnénk egy commit-ba tenni, hanem 2-3 külön lépésben szeretnénk commit-olni.
