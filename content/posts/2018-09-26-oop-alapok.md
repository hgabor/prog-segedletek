---
title:  "Objektum-orientált fejlesztés alapok"
date:   2018-09-26 10:00:00 +0100
tags:
 - oop
 - programozas
---

## Az osztály és az objektum

Az objektum-orientált szemlélet célja, hogy a logikailag egybe tartozó adatokat egységben tudjuk kezelni a hozzá tartozó műveletekkel - mind a forráskódban, mind használat közben.

Definiálás:

{{< highlight csharp >}}
class Animal
{
    string name;
    int hungerLevel;

    void Eat(Animal a) { /* ... */ }

    void MakeSound() { /* ... */ }

    int GetHungerLevel() { return hungerLevel; }
}
{{< /highlight >}}

Használat:

{{< highlight csharp >}}
Animal hawk = new Animal("Hawk");
Animal mouse = new Animal("Mouse");
h.Eat(mouse);
h.hungerLevel -= 10;
{{< /highlight >}}

Az osztály egy *összetett adattípus*, ami azt jelenti, hogy több, más adattípusból áll össze. A fenti példában az *Animal* osztály egy névből és egy éhségszintből áll.

Az *Animal* ugyanúgy egy típus, mint az *int*, *bool* és a *string* - az egyedüli különbség, hogy mi definiáltuk. Épp ezért, a többi típushoz hasonlóan, a használatához konkrét értékekre van szükség.

Figyeljük meg a hasonlóságot:

{{< highlight csharp >}}
// Tipus valtozo = ertek
int characterCount = 5;
bool fileOpened = file.IsOpen;
double array = { 5, 7.8, -78 };
Animal hawk = new Animal("Mouse");
List&lt;string> list = new List&lt;string>();
{{< /highlight >}}

Ez azt jelenti, hogy pl. az Eat() függvényt, vagy a hungerLevel változót egy konkrét állat nélkül nem is lehet használni.

Elnevezések, szakszavak:

* Animal - típus: **osztály (class)**
* new Animal() - érték: **objektum (object), példány (instance)**
* Az összetartozó adatok (name, hungerLevel): **tag (member), objektum változói**
* Az elvégezhető műveletek (Eat, GetHungerLevel): **metódus (method), tagfüggvény (member function)**
* Új érték, objektum létrehozása: **példányosítás (instantiation, to instantiate)**, vagy csak simán **létrehozás (create)**

A tagváltozókat és a metódusokat a programozási nyelvek döntő többségében a . (pont) művelettel érhetjük el:

{{< highlight csharp >}}
t.Length;
date.Year;
file.ReadLine();
"Hello".ToUpper();
{{< /highlight >}}

PHP nyelven azonban a . már foglalt a string összefűzésre, ezért a C-ből kölcsönzött nyilat használjuk:

{{< highlight php >}}
$obj->setData(4);
$obj->data = 5; // A tagváltozó elé nem kell még egyszer dollárjel!
{{< /highlight >}}

## Metódusok és a this

{{< highlight csharp >}}
class Person
{
    string name;
    DateTime birthDate;

    int CalculateAge()
    {
        return DateTime.Now.Year - this.birthDate.Year;
    }
}
{{< /highlight >}}

A hagyományos függvényekhez képest a metódusok elérhetik az aktuális objektumok tagváltozóit és más metódusait. Ehhez a speciálisan elnevezett **this** (PHP-ban **$this**) változót használhatjuk.

A this kulcsszó Java-ban és C#-ban elhagyható, ha egyértelmű, hogy a tagváltozóra hivatkozunk:

{{< highlight csharp >}}
    int CalculateAge()
    {
        return DateTime.Now.Year - birthDate.Year;
    }
{{< /highlight >}}

Ha van ugyanolyan nevű lokális változónk vagy paraméterünk, akkor azonban ki kell írni:

{{< highlight csharp >}}
    void SetBirthDate(DateTime birthDate)
    {
        this.birthDate = birthDate;
    }
{{< /highlight >}}

PHP-ban és JavaScript-ben mindig ki kell írni, nem lehet elhagyni.


## Objektumok létrehozása

Az objektumok használatának egy nagy előnye, hogy a létrehozáskor meg lehet adni paramétereket, és ez alapján készül el az objektum.

Ehhez egy konstruktort kell definiálnunk:

{{< highlight csharp >}}
class Animal
{
    string owner;
    int legCount;

    Animal(string owner, int legCount)
    {
        this.owner = owner;
        this.legCount = legCount;
    }
}
{{< /highlight >}}

C#-ban / Java-ban a konstruktor neve az osztály neve és nincs megadva visszatérési érték. Ez nem minden programnyelvben van így! (l. példákat lentebb)


## Láthatóság (visibility)

A fenti példában van egy potenciális hibaforrás. A kód nem akadályozza meg azt, hogy a változókba ne kerüljön érvénytelen érték:

{{< highlight csharp >}}
var a = new Animal("John Smith", 4);
a.legCount = -3;
{{< /highlight >}}

A -3 érték jöhet felhasználótól, vagy hibás programkódból - mindenesetre az biztos, hogy valahogy szeretnénk megakadályozni a hiba létrejöttét.

Sok programnyelvben meg lehet jelölni a változókat, függvényeket, konstruktorokat, hogy csak bizonyos környezetben legyen elérhetők, meghívhatók. Így az osztályt használó fejlesztőt meg lehet akadályozni abban, hogy véletlenül kihagyja a hibaellenőrzést.

A két legfontosabb láthatóság:
* **public**: publikus, bárhonnan elérhető
* **private**: privát, csak az osztályon belül érhető el

{{< highlight csharp >}}
class Animal
{
    private string name;

    public void WriteName()
    {
        // A "name" az osztályon belül elérhető:
        Console.WriteLine(this.name);
    }
}

class Program
{
    public static void Main(string[] args)
    {
        Animal a = new Animal();
        // A "name" az osztályon kívül nem érhető el, az alábbi sor hibás:
        Console.WriteLine(a.name);
        // A metódus viszont publikus, azt meg lehet hívni:
        a.WriteName();
    }
}
{{< /highlight >}}

## Példák osztály definiálására

{{< highlight csharp >}}
class Animal
{
    string owner;
    int legCount;

    public Animal(string owner, int legCount)
    {
        this.owner = owner;
        this.legCount = legCount;
    }

    public void SetLegCount(int legCount)
    {
        if (legCount >= 0) {
            this.legCount = legCount;
        }
    }
}
{{< /highlight >}}

C#-ban az alapértelmezett láthatóság a private, ezért nem szokás kiírni.

{{< highlight java >}}
class Animal {
    private String owner;
    private int legCount;

    public Animal(string owner, int legCount) {
        this.owner = owner;
        this.legCount = legCount;
    }

    public void setLegCount(int legCount) {
        if (legCount >= 0) {
            this.legCount = legCount;
        }
    }
}
{{< /highlight >}}

Java-ban a default érték teljesen mást jelent, ezért a private-et mindig kiírjuk.

{{< highlight php >}}
class Animal
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
{{< /highlight >}}

PHP-ban mindent ki kell írni. A konstruktor neve mindig *__construct*, az osztály nevétől függetlenül.

{{< highlight javascript >}}
class Animal {
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
{{< /highlight >}}

JavaScript-ben nem létezik láthatóság, minden publikus. A privát tagokat lehet jelölni kezdeti aláhúzással (pl. `this._owner = owner`), de ez csak konvenció, a környezet nem jelez hibát, ha megszegjük.

Adattagokat nem kell külön kiírni, a konstruktorban definiáljuk őket értékadással.

A konstruktor neve mindig *constructor*, az osztály nevétől függetlenül.

### Használat

A létrehozás mind a négy esetben, mind a négy programnyelven:

{{< highlight csharp >}}
new Animal("Mary Sue", 6);
{{< /highlight >}}

### Ábrázolás

Az osztályokat lehet programozási nyelvtől független módon, úgynevezett osztálydiagramon ábrázolni.

![Repó és munkapéldány közti műveletek](/assets/img/class-single.svg)

A téglalap mindig három részből áll, akkor is, ha valamelyik üresen marad.

A felső harmad az osztály neve. A középső rész tartalmazza az adattagokat, az alsó pedig a metódusokat. Az ábráról egyértelműen leolvasható a változók típusa, a függvények paraméterei és visszatérési értéke.

A konstruktort tipikusan az osztály nevével jelöljük, a privát adattagokat - jellel, a publikusakat + jellel.

## Property-k, getter, setter

Gyakran előfordul, hogy a tagváltozókat márpedig el szeretnénk érni, de a hibaellenőrzést meg szeretnénk tartani.

Java és PHP nyelven ezt segédfüggvények segítségével oldhatjuk meg.

Java példa:

{{< highlight java >}}
class Animal {
    private String owner;
    private int legCount;

    public int getLegCount(int legCount) {
        return legCount;
    }

    public void setLegCount(int legCount) {
        if (legCount >= 0) {
            this.legCount = legCount;
        }
    }
}
{{< /highlight >}}

Ha módosítani szeretnénk, pl. 1-gyel csökkenteni a tagváltozót, akkor azt így tehetjük meg.

{{< highlight java >}}
Animal bird = new Animal();
bird.setLegCount(bird.getLegCount() - 1);
{{< /highlight >}}

A kód aránylag nem szép, de Java-ban és PHP-ban ez az egyetlen lehetőségünk.

C# és JavaScript azonban bevezetett egy új fogalmat, a **property**-t, ami lehetővé teszi, hogy az osztályon kívülről úgy látszódjon egy függvény, mintha adattag lenne.

C# példa:

{{< highlight csharp >}}
class Animal
{
    string owner;
    int legCount;

    // Nincs zárójel!
    public int LegCount {
        get
        {
            return legCount;
        }
        set {
            // A value speciális érték tartalmazza az új értéket.
            if (value >= 0) {
                this.legCount = value;
            }
        }
    }

    public string Owner {
        get
        {
            return owner;
        }
    }
}
{{< /highlight >}}

Nem kötetelező mindkettőt definiálni. Ha csak a get-et adjuk meg, akkor a property csak olvasható lesz, ha csak set-et, akkor csak írható.

Használat:

{{< highlight csharp >}}
Animal centipede = new Animal();
centipede.LegCount = 100;
centipede.LegCount--;
Console.WriteLine(centipede.Owner);
// Ez a sor hibás, mert nincs setter:
centipede.Owner = "John";
{{< /highlight >}}

JavaScript példa:

{{< highlight javascript >}}
class Animal {
    constructor() {
        this._owner = "Default owner";
        this._legCount = 0;
    }

    // Mintha függvény lenne, csak a get/set kulcsszót elé írjuk
    get legCount() {
        return this._legCount;
    }

    set legCount(legCount) {
        if (legCount >= 0) {
            this._legCount = legCount;
        }
    }

    get owner() {
        return this._owner;
    }
}

let centipede = new Animal();
centipede.legCount = 100;
centipede.legCount--;
console.log(centipede.owner);
{{< /highlight >}}

Mindkét nyelven, az Owner/owner és a LegCount/legCount használat közben úgy tűnik, mintha változó lenne, mégis függvényeket hív meg a háttérben. Ezáltal olvashatóbb kódot tudunk készíteni:

`bird.setLegCount(bird.getLegCount() - 1)` ugyanazt jelenti, mint a `bird.LegCount--`

és mégsem vesztjük el a hibaellenőrzést, a legCount értéke sosem lesz negatív.

## Mi ellen véd a private?

Véletlen programhibák, elgépelések, számítási hibák ellen.

### Rossz szándékú fejlesztő ellen nem véd

Ha szeretnénk, könnyedén ki lehet játszani a private által nyújtott védelmet:

{{< highlight java >}}
import java.lang.reflect.Field;

class TestClass {
    // Privát, az osztály nem módosítja, így örökké 2 lesz, ugye?
    private int alwaysPositive = 2;
    
    public void print() {
        System.out.println(this.alwaysPositive);
    }
}

public class CircumventVisibility {
    public static void main(String[] args) throws NoSuchFieldException,
            IllegalArgumentException, IllegalAccessException {
        TestClass obj = new TestClass();
        obj.print(); // 2-t ír ki
        // Az alábbi sor hibás, mert az alwaysPositive változó privát
        // obj.alwaysPositive = -3;

        // Turkáljunk egy kicsit a Java belső működésébe
        Field declaredField = TestClass.class
            .getDeclaredField("alwaysPositive");
        declaredField.setAccessible(true);
        declaredField.setInt(obj, -3);
        obj.print(); // -3 -at ír ki. Upsz...
    }
}
{{< /highlight >}}

Viszont ilyen kódot véletlenül nem fogunk írni. Ha mégis használjuk, magunkkal szúrunk ki.

### A gép előtt ülő "hacker"-től nem véd

Ha rendszergazdák vagyunk, akkor a saját gépen amúgy is van hozzáférésünk mindenhez, csak egy kis energia kell hozzá.
Az .exe / .jar stb. fájlokból ki lehet olvasni az összes string-et, adatot, illetve a megfelelő eszközökkel a memóriából is.
