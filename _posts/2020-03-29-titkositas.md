---
layout: post
title:  "Titkosítás, biztonság"
date:   2020-03-29 10:00:00 +0100
tags: 14evfolyam titkositas biztonsag
---

Amikor titkosításról és biztonságról beszélünk, meg kell különböztetnünk a különféle eseteket, amelyek ellen védekezni szeretnénk, pl.:

* Véletlen, nem szándékos meghibásodás ellen
* Szándékos rongálás ellen
* Hamisítás ellen
* Illetéktelen hozzáféréssel szemben
* stb.

Az esettől függően különféle eszközök állnak a rendelkezésünkre.

A történetünknek legyen három szereplője:

* Alice: szeretne egy titkos üzenetet küldeni Bobnak
* Bob: az üzenet fogadója
* Eve: szeretné az üzenetet útközben megszerezni, elolvasni, és ha tudja, akár meg is hamisítani.

## A probléma

Alice és Bob között a kapcsolat sajnos nem megbízható. Nem tudhatjuk, hogy épp ki hallgatja a telefonbeszélgetésüket, nem néznek-e bele az email-ekbe, a chat-ekbe.

Alice-nak és Bob-nak csak a saját eszközeikre (telefon, számítógép) van ráhatása.

* Telefonos beszélgetés:
  * A rádióhullámokat bárki befoghatja
  * Az adótornyokat a telefon-szolgáltató üzemelteti, ott összegyűjthetnek minden beszélgetést/SMS-t
* Internetes chat:
  * Wifi-t befoghatja mindenki a közelben
  * Az internetszolgáltató lát minden elküldött adatcsomagot
  * Városok, országok közti kommunikáció esetében több szolgáltatón is keresztül megy minden adat
* Posta
  * A postások bármelyik levélet kibonthatják
  * A helyi postán minden helyi levél átfut

Ha csak egy láncszem is gyenge a fentiek közül, akkor Eve könnyedén meg tudja szerezni az üzenetet, pl. a postás lefizetésével.

Amikor biztonságról beszélünk, akkor két alapvető kritériumot kell teljesítenünk:
* **A rendszernek akkor is biztonságosnak kell maradnia, ha az alatta élvő kommunikációs csatorna nem az**: a postás minden levelet elolvas, a szolgáltató megvizsgál minden adatcsomagot stb.
* **A rendszernek akkor is biztonságosnak kell maradnia, ha a támadó ismeri titkosítási algoritmust**: a [titkolózáson alapuló biztonság](https://hu.wikipedia.org/wiki/Bizonytalans%C3%A1gon_alapul%C3%B3_biztons%C3%A1g) (security through obscurity) valójában nem biztonság, csak annak a látszatát kelti.

## Hagyományos egykulcsos/szimmetrikus (jelszavas) titkosítás

Az ilyen jellegű titkosítási algoritmusokat azért nevezzük szimmetrikusnak, mert a titkosítási kulcs kell az üzenet visszafejtéséhez.

Az egyik legrégebb óta ismert titkosítási forma az ún. [Caesar-kód](https://hu.wikipedia.org/wiki/Caesar-rejtjel), amelyben az üzenet betűit N darab karakterrel odébb toljuk (Z esetén az A betűtől indulunk újra).

Pl. ha N=3

<pre>
Eredeti:     HELLO
Titkosított: KHOOR
</pre>

Ebben az esetben, még ha Eve útközben megkaparintja a levelet, abban annyi fog szerepelni, hogy KHOOR, amely nem értelmes. Viszont ha Bob kézhez kapja a levelet, ő tudni fogja, hogy a betűk 3-mal el lettek tolva. Ha a műveletet elvégzi visszafelé (az ABC másik irányába tolja el a betűket 3-mal), akkor visszakapja az eredeti üzenetet.

Ez a konkrét algoritmus az ókorban még működhetett, ma már azoban az ilyen egyszerű, behelyettesítésen alapuló algoritmusokat nagyon könnyű visszafejteni. A ma használt szimmetrikus algoritmusok ennél összetettebbek, hogy a visszafejtést megelőzzék.

A szimmetrikus algoritmusok gyengéje, hogy a titkos kulcsot először valahogyan el kell juttatni Bobhoz. Ha Bob nem tudja, hogy 3-mal kell a betűket eltolni, ő sem fogja tudni elolvasni az üzenetet. Ha a titkos kulcsot Alice elküldi Bob-nak, azt Eve lehallgathatja, ami után már ő is el tudja olvasni az üzeneteket.

A leggyakrabban használt és ajánlott algoritmus: AES

Fontos! A szoftverfejlesztői OKJ tételek közt szerepel a DES/3DES algoritmus. Ezek mai szemmel már gyengék, elavultnak számítanak, új alkalmazásban ne használjuk őket!

## Kétkulcsos/aszimmetrikus titkosítás

Tegyük fel, hogy Alice és Bob nem tudjnak egy titkos kódban megegyezni előre, mégis szeretnének üzenetet váltani, leghallgatás nélkül.

Először is nézzünk egy kis gondolkodtató, találós kérdést. Alice-nak van egy doboza, amire rátehet egy lakatot. Ekkor nem tud más belenézni. De ha Bob megkapja a lezárt dobozt, ő sem tudja kinyitni.

Mi a megoldás?

<br>
<br>
<br>
<br>
<br>
<br>
<br>

Segítség: az alfejezet címe.

<br>
<br>
<br>
<br>
<br>
<br>
<br>

* Alice lezárja a dobozt, és elküldi Bobnak
* Bob megkapja a lezárt doboz, amit nem tud kinyitni, ezért *ő is rátesz egy lakatot*, majd visszaküldi Alice-nak
  * Most útközben már két lakat is van rajta
* Alice leveszi a saját lakatját. Ő már nem tudja kinyitni, mert Bob lakatja rajta van
* Ismét elküldi Bobnak, aki most már a saját kulcsával ki tudja nyitni a dobozt

A módszer lényege, hogy egy időpillanatban sem volt olyan, hogy a doboz lakat nélkül utazott volna, és a kulcs sem hagyta el a tulajdonosait.

Eve persze használhatott volna erőt is, pl. levágja magát a lakatot, vagy darabokra szedi magát a dobozt - a gyakorlatban nem lesz használható. Ettől függetlenül ez a találós kérdés rávilágít arra, hogy nem muszáj megosztani a kulcsokat/jelszavakat a titkosításhoz, létezhet megoldás.

### Az üzenet titkosítása

Az algoritmus nem egy titkosítási kulcsot használ, hanem egy ún. összetartozó *kulcspárt*:
* K1
* K2

Amit K1-gyel titkosítottunk, azt csak K2-vel lehet visszafejteni, és viszont: amit K2-vel titkosítottunk, azt csak K1-gyel lehet visszafejteni:

Eredeti üzenet → K1 → *titkos* → K2 → Eredeti üzenet

vagy

Eredeti üzenet → K2 → *titkos* → K1 → Eredeti üzenet

Ezt a tulajdonságát felhasználhatjuk a fenti probléma megoldására:

* Nevezzük ki K1-et **publikus kulcs**nak, és osszuk meg mindenkivel
* Legyen K2 **privát kulcs**, és tartsuk titokban, magunknál.

Ennek két haszna is lesz:

### Publikus kulcsú titkosítás

Az alaphelyzet megint ugyanez: Alice megint üzenetet akar küldeni Bobnak, de most az interneten keresztül, így nincs fizikai doboz, amibe elrejthetné az üzenetet.

Viszont ismeri Bob publikus kulcsát.

* Alice titkosítja az üzenetét Bob publikus kulcsával
* Eve el tudja olvasni a titkosított üzenetet, de mivel nem tudja Bob privát kulcsát, nem tudja értelmezni
* Bob vissza tudja fejteni a saját kulcsával

### Digitális aláírás

Mivel Eve-nek most már nincs lehetősége a leveleket elolvasni, más támadási formák után néz. Az alábbi üzenetet írja:

> Kedves Bob!
>
> Tudod, milyen jó barátok vagyunk, és segítünk egymásnak a bajban.
> Sajnos anyagilag megszorultam egy kissé. Ki tudnál segíteni egy kis összeggel?
> Erre a számlaszámra küldd légyszi: XXX-YYY-ZZZ
>
> Köszi,  
> Alice

Bob honnan fogja tudni, hogy az üzenetet tényleg Alice küldte?

A kétkulcsú titkosítás ezt is meg tudja oldani.

Ha Alice egy hiteles üzenetet szeretne küldeni, akkor először titkosítja a *saját privát kulcsával*. De ennek mi értelme? A publikus kulcsot mindenki ismeri, bárki elolvashatja az üzenetet, nem?

A cél nem is ez. Ha valakinek sikerült visszafejteni az üzenetet Alice publikus kulcsával, akkor az üzenet csak tőle származhat: mert más nem tudta volna így titkosítani. Épp ezért ezt nem is titkosításnak szokták nevezni, hanem **digitális aláírás**nak.

Mivel Eve nem ismeri Alice privát kulcsát, ezért nem tud az ő nevében üzenetet küldeni.

### A kettő együtt

Tehát ha Alice szeretne egy üzenet küldeni Bobnak, amit Eve se elolvasni, se meghamisítani nem tud, az alábbiakat kell tennie:

*Eredeti üzenet* → Alice privát kulcsa → *Aláírt üzenet* → Bob publikus kulcsa → *Aláírt és titkosított üzenet*

Bob pedig ezeket a lépéseket fogja végrehajtani:

*Aláírt és titkosított üzenet* → Bob privát kulcsa → *Aláírt üzenet*

> Szóval az üzenet nekem szólt, más nem olvashatta el!

*Aláírt üzenet* → Alice publikus kulcsa → *Eredeti üzenet*

> Szóval ez tényleg Alice-tól jött!

A lépéseket természetesen szoftver végzi, így történik a kommunikáció pl. a webszerver és a böngésző között (HTTPS), de pl. [PGP](https://hu.wikipedia.org/wiki/PGP) segítségével tetszőleges dokumentumra, emailre is alkalmazható.

### RSA, DSA

A két leggyakrabban használt algoritmus. A böngészőben, a lakat ikonra kattintva megnézhetjük, hogy ez az oldal is RSA titkosítást használ.

Saját kulcspárt generálhatunk pl. OpenSSH segítségével. Ha szeretnénk egy 4096 bites, RSA kulcsot:

<pre><code class="bash">ssh-keygen -t rsa -b 4096
</code></pre>

Ha alapértelmezett fájlnéven hagyjuk, akkor két fájlt generál a "~/.ssh" mappában:

* id_rsa: ez a privát kulcs
* id_rsa.pub: ez a publikus kulcs

Windows alatt egy grafikus alkalmazás a [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) ssh klienshez mellékelt puttygen.exe, amivel szintén lehet kulcspárokat generálni.

## Egyirányú titkosítás

Bizonyos szituációkban nem fontos, hogy a titkosítás visszafordítható legyen. Ilyen eset pl. a jelszavak tárolása. Ha van módszerünk a jelszavak egyezésének az ellenőrzésére, akkor a visszafejtésre nem is kell, hogy létezzen módszer. Sőt, ez biztonsági hibának számítana!

Azokat az algoritmusokat, amik erre alkalmasak, **kriptográfiai hash** függvényeknek nevezzük.

[Emlékeztetőül, a hash függvény:]({% post_url 2018-12-01-hash %})

* Egyirányú, a hash-ből nem lehet visszaszerezni az eredeti adatot
* Fix intervallumra vetíti le az eredeti adatot, így nem derül ki, hogy az eredeti 1 bájt vagy 1 gigagbájt volt: a bcrypt eredménye mindig 184 bites lesz.

Nem minden hash függvény alkalmas jelszavak tárolására. A CRC32-t például véletlen hibák ellen találták ki, nagyon egyszerű kulcsütközést generálni.

A jelszó helyességének az ellenőrzéséhez az adatbázisban eltárolt jelszót nem kell visszafejteni, hanem elég a felhasználó begépelt jelszavát hash-elni. Ha a most kiszámolt, és az adatbázisban tárolt hash megegyezik, akkor a user eltalálta a jelszót.

Ha valaki pedig megszerezné az adatbázisban tárolt hash-eket, akkor nem menne velük sokra: a hash-ekből nem lehet visszaszerezni az eredeti jelszót.

### Salt

A jelszót titkosítás előtt meg is "sózhatjuk", ezt azt jelenti, hogy egy kis méretű, véletlenszerű adatot hozzáfűzünk. Ennek az a célja, hogy ha a rendszerben két user véletlenül ugyanazt a jelszót választja, ez ne derüljön ki a hash-ek egyezéséből.

Salt nélkül:

* User1:
  * hash(jelszo1) = asdf1234
* User2:
  * hash(jelszo1) = asdf1234

Viszont ha a jelszavakat megsózzuk:

* User1
  * salt = ab
  * hash(jelszo1ab) = a1223445
* User2
  * salt = cd
  * hash(jelszo1cd) = 89as5zb5

Ugyanaz a jelszó, de a hash különbözik.

### PHP példakód

Regisztrációkor:

<pre><code class="php">// Hash kiszámítása, ezt fogjuk az adatbázisba tenni
$db_hash = password_hash($_POST['password'], PASSWORD_DEFAULT);
</code></pre>

Bejelentkezéskor:
<pre><code class="php">$db_hash = ... // Ezt olvassuk ki az adatbázisból
if (password_verify($_POST['password'], $db_hash)) {
    // Sikeres login
    $_SESSION['user_id'] = $user_id;
} else {
    $error_message = 'Sikertelen login';
}
</code></pre>

Javasolt kriptográfiai hash algoritmusok:

* bcrypt (PHP alapértelmezett)
* Argon2 (PHP beépített):
  * password_hash($pw, PASSWORD_ARGON2I);
* PBKDF2 + SHA-512

**Ellenjavalt algoritmusok:**
* MD5, SHA1
* bármilyen algoritmus, amit nem lehet paraméterezni az iterációk számával


