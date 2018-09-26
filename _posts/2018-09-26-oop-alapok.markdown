---
layout: post
title:  "Objektum-orientált fejlesztés alapok"
date:   2018-09-26 10:00:00 +0100
tags: 14evfolyam oop programozas
---

## Az osztály és az objektum

Az objektum-orientált szemlélet célja, hogy a logikailag egybe tartozó adatokat egységben tudjuk kezelni a hozzá tartozó műveletekkel - mind a forráskódban, mind használat közben.

Definiálás:

<pre><code class="csharp">class Animal
{
    string name;
    int hungerLevel;

    void Eat(Animal a) { /* ... */ }

    void MakeSound() { /* ... */ }

    int GetHungerLevel() { return hungerLevel; }
}
</code></pre>

Használat:

<pre><code class="csharp">Animal hawk = new Animal("Hawk");
Animal mouse = new Animal("Mouse");
h.Eat(mouse);
h.hungerLevel -= 10;
</code></pre>

Az osztály egy *összetett adattípus*, ami azt jelenti, hogy több, más adattípusból áll össze. A fenti példában az *Animal* osztály egy névből és egy éhségszintből áll.

Az *Animal* ugyanúgy egy típus, mint az *int*, *bool* és a *string* - az egyedüli különbség, hogy mi definiáltuk. Épp ezért, a többi típushoz hasonlóan, a használatához konkrét értékekre van szükség.

Figyeljük meg a hasonlóságot:

<pre><code class="csharp">// Tipus valtozo = ertek
int characterCount = 5;
bool fileOpened = file.IsOpen;
double array = { 5, 7.8, -78 };
Animal hawk = new Animal("Mouse");
List&lt;string> list = new List&lt;string>();
</code></pre>

Ez azt jelenti, hogy pl. az Eat() függvényt, vagy a hungerLevel változót egy konkrét állat nélkül nem is lehet használni.

Elnevezések, szakszavak:

* Animal - típus: **osztály (class)**
* new Animal() - érték: **objektum (object), példány (instance)**
* Az összetartozó adatok (name, hungerlevel): **tag (member), objektum változói**
* Az elvégezhető műveletek (Eat, GetHungerLevel): **metódus (method), tagfüggvény (member function)**
* Új érték, objektum létrehozása: **példányosítás (instantiation, to instantiate)**, vagy csak simán **létrehozás (create)**

A tagváltozókat és a metódusokat a programozási nyelvek döntő többségében a . (pont) művelettel érhetjük el:

<pre><code class="csharp">t.Length;
date.Year;
file.ReadLine();
"Hello".ToUpper();
</code></pre>

PHP nyelven azonban a . már foglalt a string összefűzésre, ezért a C-ből kölcsönzött nyilat használjuk:

<pre><code class="php">$obj->setData(4);
$obj->data = 5; // A tagváltozó elé nem kell még egyszer dollárjel!
</code></pre>

## Metódusok és a this

<pre><code class="csharp">class Person
{
    strint name;
    DateTime birthDate;

    int CalculateAge()
    {
        return DateTime.Now.Year - this.birthDate.Year;
    }
}
</code></pre>

A hagyományos függvényekhez képest a metódusok elérhetik az aktuális objektumok tagváltozóit és más metódusait. Ehhez a speciálisan elnevezett *this* (PHP-ban $this) változót használhatjuk.

A this kulcsszó Java-ban és C#-ban elhagyható, ha egyértelmű, hogy a tagváltozóra hivatkozunk:

<pre><code class="csharp">    int CalculateAge()
    {
        return DateTime.Now.Year - birthDate.Year;
    }
</code></pre>

Ha van ugyanolyan nevű lokális változónk vagy paraméterünk, akkor azonban ki kell írni:

<pre><code class="csharp">    void SetBirthDate(DateTime birthDate)
    {
        this.birthDate = birthDate;
    }
</code></pre>

PHP-ban és JavaScript-ben mindig ki kell írni, nem lehet elhagyni.


## Objektumok létrehozása

Az objektumok használatának egy nagy előnye, hogy a létrehozáskor meg lehet adni paramétereket, és ez alapján készül el az objektum.

Ehhez egy konstruktort kell definiálnunk:

<pre><code class="csharp">class Animal
{
    string owner;
    int legCount;

    Animal(string owner, int legCount)
    {
        this.owner = owner;
        this.legCount = legCount;
    }
}
</code></pre>

C#-ban / Java-ban a konstruktor neve az osztály neve és nincs megadva visszatérési érték. Ez nem minden programnyelvben van így! (l. példákat lentebb)


## Láthatóság (visibility)

A fenti példában van egy potenciális hibaforrás. A kód nem akadályozza meg azt, hogy a változókba ne kerüljön érvénytelen érték:

<pre><code class="csharp">var a = new Animal("John Smith", 4);
a.legCount = -3;
</code></pre>

A -3 érték jöhet felhasználótól, vagy hibás programkódból - mindenesetre az biztos, hogy valahogy szeretnénk jeletni a fejlesztőnek, hogy hiba történt.

Sok programnyelvben meg lehet jelölni a változókat, függvényeket, konstruktorokat, hogy csak bizonyos környezetben legyen elérhetők, meghívhatók. Így az osztályt használó fejlesztőt meg akadályozni abban, hogy véletlenül kihagyja a hibaellenőrzés.

A két legfontosabb láthatóság:
* **public**: publikus, bárhonnan elérhető
* **private**: privát, csak az osztályon belül érhető el

## Példák osztály definiálására

### C\#

<pre><code class="csharp">class Animal
{
    string owner;
    int legCount;

    public Animal(string owner, int legCount)
    {
        this.owner = owner;
        this.legCount = legCount;
    }

    public SetLegCount(int legCount)
    {
        if (legCount >= 0) {
            this.legCount = legCount;
        }
    }
}
</code></pre>

C#-ban az alapértelmezett láthatóság a private, ezért nem szokás kiírni.

### Java

<pre><code class="java">class Animal {
    private String owner;
    private int legCount;

    public Animal(string owner, int legCount) {
        this.owner = owner;
        this.legCount = legCount;
    }

    public setLegCount(int legCount) {
        if (legCount >= 0) {
            this.legCount = legCount;
        }
    }
}
</code></pre>

Java-ban a default érték teljesen mást jelent, ezért a private-et mindig kiírjuk.

### PHP

<pre><code class="php">class Animal
{
    private $owner;
    private $legCount;

    public function __construct($owner, $legCount)
    {
        $this->owner = $owner;
        $this->legCount = $legCount;
    }

    public function setLegCount($legCount)
    {
        if ($legCount >= 0) {
            $this->legCount = $legCount;
        }
    }
}
</code></pre>

PHP-ban mindent ki kell írni. A konstruktor neve mindig *__construct*, az osztály nevétől függetlenül.

<pre><code class="javascript">class Animal {
    constructor(owner, legCount) {
        this.owner = owner;
        this.legCount = legCount;
    }

    setLegCount(legCount) {
        if (legCount >= 0) {
            this.legCount = legCount;
        }
    }
}
</code></pre>

JavaScript-ben nem létezik láthatóság, minden publikus. A privát tagokat lehet jelölni kezdeti aláhúzással (pl. `this._owner = owner`), de ez csak konvenció, a környezet nem jelez hibát, ha megszegjük.

Adattagokat nem kell külön kiírni, a konstruktorban definiáljuk őket értékadással.

A konstruktor neve mindig *constructor*, az osztály nevétől függetlenül.

### Használat

A létrehozás mind a négy esetben, mind a négy programnyelven:

<pre><code class="csharp">new Animal("Mary Sue", 6);
</code></pre>

### Ábrázolás

Az osztályokat lehet programozási nyelvtől független módon, úgynevezett osztálydiagramon ábrázolni.

![Repó és munkapéldány közti műveletek](/assets/img/class-single.svg)

A téglalap mindig három részből áll, akkor is, ha valamelyik üresen marad.

A felső harmad az osztály neve. A középső rész tartalmazza az adattagokat, az alsó pedig a metódusokat. Az ábráról egyértelműen leolvasható a változók típusa, a függvények paraméterei és visszatérési értéke.

A konstruktort tipikusan az osztály nevével jelöljük, a privát adattagokat - jellel, a publikusakat + jellel.

