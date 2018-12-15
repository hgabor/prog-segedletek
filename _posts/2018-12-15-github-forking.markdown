---
layout: post
title:  "GitHub - fork-olás"
date:   2018-12-15 10:00:00 +0100
tags: 14evfolyam verziokezeles
---

Nyílt forráskódú projekt esetében előfordul, hogy más kódjához mi magunk is hozzá szeretnénk nyúlni, szeretnénk bele fejleszteni, ki szeretnénk egészíteni.
Érthető okokból azonban a projektek tulajdonosai nem adnak bárkinek hozzáférést a repóhoz. Emiatt szükség lesz egy saját változatra.

Megtehetjük azt, hogy saját magunk leklónozzuk a projektet, majd módosítva a "remote" beállításokat egy saját, üres repóba feltöltjük.
Ezt hívjuk a repó **fork**-olásának (a szó azt jelenti, hogy elágazás: ahogy az út egyből kettéágazik, vagy ahogy a villa egy nyeléből több kisebb "ág" nő ki).

Ha a platform megengedi, akkor ezt egy kattintással is elvégezhetjük. A példában a [GitHub](https://github.com) weboldalt használom.

## Forkolás

A projekt weboldalán kattintsunk erre a gombra:

![Fork](/assets/img/github_fork.png)

Ez után a saját projektjeink között meg fog jelenni a repó, együtt a teljes történelmével, változtatásaival.

A kapcsolat nem szűnik meg a két repó között, a név alatt látszik, hogy melyik volt az eredeti repó:

![Fork-olt projekt](/assets/img/github_forked.png)

Forkolás után a két projektet különbözőnek lehet tekinteni, a saját repónkba gond nélkül tudunk saját kódot push-olni - de ha szeretnénk, akkor az eredetit is nyomon tudjuk követni, néhány parancs kiadásával az eredeti projekt változtatásait is tudjuk integrálni.

Ilyen módon lehet karban tartani pl. egy népszerű, Linux-os projekt Windows-os változatát, ha az eredeti készítő erre nem tud időt szánni:

* Az eredeti projekt a Linux-os kódot tartalmazza
* A forkolt projekt tartalmazza a szükséges Windows-os módosításokat, miközben naprakészen tud maradni az eredetivel

A két projekt innentől kezdve párhuzamosan fut.

## Pull request

Ha a saját módosításunkat az eredeti projekt fejlesztői is szívesen látnának, akkor készíthetünk egy ún. **pull request**-et. Ez egy felkérés arra, hogy bizonyos commitokat az eredeti fejlesztő pull-oljon a mi projektünkből. Ezt ő pedig elfogadhatja, vagy visszautasíthatja.

[Példa egy pull request-re](https://github.com/dotnet/roslyn/pull/31801): lehet comment-elni, látszanak a bele tartozó commitok.

Github-on a pull request az elsődleges módszer a különböző fejlesztők közös munkájához. Cégen belüli, privát git repók esetében is léteznek hasonló funkciók, amely egy repón belül, a különböző fejlesztési ágak között valósítja meg ugyanezt (ún. **merge request** - merge-elésre felkérés).
