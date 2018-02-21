---
layout: post
title:  "Git bevezető"
date:   2018-01-17 15:50:00 +0100
tags: 14evfolyam verziokezeles
---

Egy verziókezelő rendszer az alábbi problémákat oldja meg:

* Több fejlesztőnek kell egyszerre ugyanazon a projekten dolgozni
* Meg akarjuk tartani az időközbeni változásokat is:
  * Könnyebb a hibakeresés
  * Ha szükséges, vissza lehet állni
* A forráskód legfrissebb változata(i) egy központi helyen lesznek elérhetők

A **[git](https://git-scm.com/)** egy a sok verziókezelő rendszer közül, talán a legelterjedtebb. (Népszerű alternatívák még: mercurial, subversion)

A git alapvetően konzolról vezérelt eszköz, de szinte minden grafikus fejlesztőkörnyezet
(Visual Studio, Netbeans, Eclipse, IntelliJ, Andorid Studio) támogatja.
Van, amelyik beépítetten, van amelyikhez szükséges a konzolos változat telepítése.

Ebben a leírásban elsősorban az általános fogalmakra, és a GUI-s felületekre koncentrálok.
A git teljes funkciókészletével sokkal többet is lehet lehet kezdeni (pl. több párhuzamos fejlesztési ág kezelése, repó történelmének módosítása, script-ek automatikus futtatása stb.), ezekbe akkor érdemes beletanulni, ha már az alapvető munkamenetet biztonsággal kezeljük.

## Alapfogalmak

* *Repository (röviden repó):* A projektünk összes korábbi változatát tartalmazza. Gitben ez a projektünk mellett, a <code>.git</code> nevű rejtett mappában található.
* *Commit:* Egy konkrét változtatás, a projektünk egy aktuális állapota.
  A változtatások mellett tartalmazza a szerzőt és a commit létrehozásának időpontját is. A repó lényegében a commit-ok összessége.
  Igeként egy commit létrehozását is jelenti (commit-olni).
* *Munkapéldány (working copy):* A fájlok, amelyekkel éppen dolgozunk. A munkapéldányt megváltoztatva tudunk commit-ot létrehozni.
A munkapéldányt *tisztának* nevezzük, ha nincs benne nem commit-olt változtatás.
* *Checkout:* Kiválasztjuk, hogy a repóból melyik committal szeretnénk dolgozni, és ezt lesz az új munkapéldányunk.

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

TODO

## Visual Studio UI

Visual Studio-ban a verziókezelési műveleteket a "Team Explorer" panelen találhatjuk meg.

![Repók közötti műveletek](/assets/img/git_vs_home.png)

A "Manage Connections" ikonra kattintva (vagy a felső menüsávban a "Team/Manage Connection"-re)
előjön az alábbi panel:

![Repók közötti műveletek](/assets/img/git_vs_connect.png)

Itt tudunk létező repót klónozni, vagy már klónozott repókat kezelni.
Ha innen választjuk ki a projektet a File/Open helyett, akkor ne felejtsük el
a solution-t sem megnyitni!

A Home panelről érhetjük még el az alábbiakat:

* Changes
  * Új commit létrehozása
  * A repó történelmének (commitok) megtekintése
* Branches
  * Ágak kezelése (átváltás, létrehozás, törlés)
* Sync
  * Repók közötti műveletek: fetch, pull, push
* Settings
  * Git beállítások

Ha az új projektünkhöz helyi repót szeretnénk létrehozni, klónozás nélkül,
akkor azt az új projekt létrehozásakor kell kiválasztani:

![Repók közötti műveletek](/assets/img/git_vs_init.png)

Ha még nem használtuk a funkciót, a jelölőnégyzet felirata első alkalommal valószínűleg "Add to Source Control" lesz,
ekkor ki kell választani, hogy git-tel szeretnénk dolgozni Team Foundation helyett.

## NetBeans UI

TODO