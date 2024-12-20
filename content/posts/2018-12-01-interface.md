---
title:  "Interfészek"
date:   2018-12-01 10:00:00 +0100
tags:
 - oop
 - programozas
---

Az interfész (interface) szó azt jelenti, hogy határfelület. A szerepe, hogy két különböző, egymástól független dolog között létesítsen kapcsolatot anélkül, hogy a két dolognak ismernie kelljen egymást:

* Egy felhasználói interfész segítségével a felhasználó használni tudja a programot anélkül, hogy ő maga fejlesztő lenne
* Egy hálózati interfész segítségével két számítógép képes egymással kommunikálni anélkül, hogy ismernék egymás belső felépítését

## Programozási szempontból

Informálisan egy modul, egy komponens, egy osztály interfészének azt nevezzük, ami "kívülről", a modult felhasználó programkódból elérhető. A tanult programnyelvekben ez a publikus függvényeket, változókat, osztályokat jelenti.

Ha egy osztály interfésze megváltozik, akkor nagy valószínűleg az azt használó programkódot is módosítani kell. Vegyük az osztályt, és az azt használó kódot:

{{< highlight csharp >}}
// Osztály:
class Kutya {
    public void Ugat()
    {
        Console.WriteLine("Vau");
    }
}

// Az osztályt meghívó kód:
var k = new Kutya();
k.Ugat();
{{< /highlight >}}

Ha az Ugat() metódust módosítjuk:

{{< highlight csharp >}}
class Kutya {
    public void Ugat(string postasNeve)
    {
        Console.WriteLine("Vau vau " + postasNeve);
    }
}
{{< /highlight >}}

Akkor a hívó kód érvényetelen lesz, ezért azt is módosítani kell:

{{< highlight csharp >}}
var k = new Kutya();
k.Ugat("Jozsi");
{{< /highlight >}}

Ezzel szemben állnak a privát tagok, illetve a publikus függvények kódja - egy szóval a _privát implementáció_. Ha ezen módosítunk, attól még az azt meghívó kódon nem biztos, hogy módosítani kell.

{{< highlight csharp >}}
class Kutya {
    public void Ugat()
    {
        // A módosított kód
        Console.WriteLine("Miau");
    }
}

// Az osztályt meghívó kódot nem kell átírni
var k = new Kutya();
k.Ugat();
{{< /highlight >}}

Épp ezért fontos, hogy milyen függvényeket, változókat nevezünk ki publikusnak. Amint egy függvény része az osztály interfészének, az utólagos módosítás sokkal nehezebbé, költségesebbé válhat.

## Az interface kulcsszó

Sok OOP nyelvben szerepel az interfész, mint nyelvi elem. Ebben az értelemben az interfész nem más, mint egy függvényekből álló lista, amely azt sorolja fel, hogy egy adott osztálynak milyen függvényeket kell megvalósítania.

{{< highlight csharp >}}
interface IAllat {
    void HangotAd();
    void Eszik(string etel);
}
{{< /highlight >}}

Az interfész csak egy leírás, nem egy konkrét dolog, ezért nem lehet belőle példányt létrehozni. Nincs olyan, hogy "Állat", van "Macska", "Kutya", "Papagáj", de az "Állat" az csak egy gyűjtőfogalom.

Ez azt jelenti, hogy interfészből nem lehet példányt létrehozni, a "new Allat()" érvénytelen. Emellett a függvényeihez sem lehet kódot írni, és minden függvényéhez feltételezzük, hogy publikus.

## Implementálás

Vegyük az alábbi interfészt:

{{< highlight csharp >}}
interface IKolcsonozheto
{
    string MegjelenitendoNev();
    int MeddigKolcsonozheto();
}
{{< /highlight >}}

Az implementálás azt jelenti, hogy megmondjuk az adott osztálynak, hogy az interfész függvényeit meg kell valósítani:

{{< highlight csharp >}}
class Konyv: IKolcsonozheto
{
    string cim;
    string szerzo;
    string isbn;

    public Konyv(string cim, string szerzo, string isbn)
    {
        this.cim = cim;
        this.szerzo = szerzo;
        this.isbn = isbn;
    }

    public string MegjelenitendoNev()
    {
        return szerzo + ": " + cim;
    }

    public int MeddigKolcsonozheto()
    {
        return 14;
    }
}
{{< /highlight >}}

Ha az interfészben szereplő függvények nem szerepelnek az osztályban, akkor a kód érvénytelen:

{{< highlight csharp >}}
class Ujsag: IKolcsonozheto
{
    string cim;
    int kiadasEv;
    int kiadasHonap;

    public Ujsag(string cim, int kiadasEv, int kiadasHonap)
    {
        this.cim = cim;
        this.kiadasEv = kiadasEv;
        this.kiadasHonap = kiadasHonap;
    }

    // Hibás, a MegjelenitendoNev-nek publikusnak kell lennie
    private string MegjelenitendoNev()
    {
        return string.Format("{0} {1} / {2}",
            cim, kiadasEv, kiadasHonap);
    }

    // Hibás, a MeddigKolcsonozheto függvény nem szerepel
}
{{< /highlight >}}

Helyesen:

{{< highlight csharp >}}
class Ujsag: IKolcsonozheto
{
    string cim;
    int kiadasEv;
    int kiadasHonap;

    public Ujsag(string cim, int kiadasEv, int kiadasHonap)
    {
        this.cim = cim;
        this.kiadasEv = kiadasEv;
        this.kiadasHonap = kiadasHonap;
    }

    public string MegjelenitendoNev()
    {
        return string.Format("{0} {1} / {2}",
            cim, kiadasEv, kiadasHonap);
    }

    public int MeddigKolcsonozheto()
    {
        return 0;
    }
}
{{< /highlight >}}

## Az interfészek célja

### Közös kezelés

Az interfészek segítségével tudjuk, hogy a különböző osztályokon szerepelnek ugyanazok a függvények. Így bár IKolcsonozheto példányt nem hozhatunk létre, változóként felvehetjük:

{{< highlight csharp >}}
IKolcsonozheto k = new Ujsag("Dormogo Domotor", 1995, 5);
Console.WriteLine(k.MegjelenitendoNev());
{{< /highlight >}}

Vagy egy vegyes listát, ún. _heterogén listát_ is létrehozhatunk:
1
{{< highlight csharp >}}
var katalogus = new List<IKolcsonozheto>();
katalogus.Add(new Ujsag("Dormogo Domotor", 1995, 5));
katalogus.Add(new Konyv("Harry Potter", "J. K. Rowling", "1234567891"));
katalogus.Add(new Ujsag("Kiskegyed", 2018, 11));

foreach(var e in katalogus)
{
    Console.WriteLine(e.MegjelenitendoNev());
}
{{< /highlight >}}

Mindegyik objektum tudja magáról, hogy kell a megjelenítendő nevét kiszámolni, így mind a könyveknek, mind az újságoknak megmarad az egyedisége, mégis tudjuk őket közösen, egy for ciklusban kezelni.

### Különböző rendszerek összekapcsolása

Ha olyan kódot készítünk, ami interfészeken dolgozik konkrét osztályok helyett, akkor:
* A programot sokkal könnyeb lesz bővíteni, az új osztálynak csak a megfelelő függvényeit kell elkészíteni, és a régi kóddal együtt is működni fog
* Az eredeti készítőnek nem kell tisztában lenni az új programkóddal, új osztályokkal

Erre a leglátványosabb példa az [IComparable](https://docs.microsoft.com/en-us/dotnet/api/system.icomparable-1) interfész, amely a CompareTo metódust követeli meg:

{{< highlight csharp >}}
class Dvd: IComparable<Dvd>
{
    string cim;
    int hossz; // perc

    public Dvd(string cim, int hossz)
    {
        this.cim = cim;
        this.hossz = hossz;
    }

    // Az aktuális és az "other" DVD-t hasonlítja össze,
    // a visszatérési érték pozitív, negatív vagy 0
    // attól függően, melyik elem a nagyobb.
    public int CompareTo(Dvd other)
    {
        if (this.hossz < other.hossz)
        {
            return -1;
        }
        else if (this.hossz > other.hossz)
        {
            return 1;
        }
        else
        {
            return 0;
        }
    }
}
{{< /highlight >}}

Ezt implementálva egy DVD-kből alló listát rendezhetünk a beépített rendező függvényyel:

{{< highlight csharp >}}
var dvdk = new List<Dvd>();
dvdk.Add(new Dvd("Star Wars IV", 210));
dvdk.Add(new Dvd("Star Wars V", 180));
dvdk.Add(new Dvd("Star Wars VI", 200));
dvdk.Sort(); // Rendezés hossz szerint
{{< /highlight >}}

A List.Sort() függvény készítőinek fogalma sem volt arról, hogy valaha DVD-ket fogunk hossz szerint rendezni, minket pedig nem érdekel, hogy a Sort függvény hogyan van implementálva. Mégis, a két rendszer gond nélkül működik egymással, az interfész segítségével.

## Más nyelvekben

C#-ban a fentiekben láttunk példát interfész definiálására és implementálására. Mivel a property-k is valójában függvények, implementáció nélkül gettereket és settereket is felvehetünk.

{{< highlight csharp >}}
interface IAllat
{
    void HangotAd();
    void Eszik(string etel);
    string Nev { get; set; }
}
{{< /highlight >}}

C#-ban az interfészeket nagy I betűvel szokás kezdeni.

### Java

A kód nagyon hasonló:

{{< highlight java >}}
interface Allat {
    void hangotAd();
    void eszik(String etel);
    String getNev();
    void setNev(String ujNev);
}
{{< /highlight >}}

Implementálás:

{{< highlight java >}}
class Papagaj implements Allat {
    public void hangotAd() { /* ... */ }
    public void eszik(String etel) { /* ... */ }
    public String getNev() { /* ... */ }
    public void setNev(String ujNev) { /* ... */ }
}
{{< /highlight >}}

### PHP

{{< highlight php >}}
interface Allat {
    function hangotAd();
    function eszik(string $etel);
    function getNev() : string;
    function setNev(string $ujNev);
}
{{< /highlight >}}

{{< highlight php >}}
class Papagaj implements Allat {
    public function hangotAd() { /* ... */ }
    public function eszik(string $etel) { /* ... */ }
    public function getNev() : string { /* ... */ }
    public function setNev(string $ujNev) { /* ... */ }
}
{{< /highlight >}}

Mivel a PHP-ben a változóknak nincsen típusa (csak az értékeknek), interfészek nélkül sem gond pl. egy ilyen tömb létrehozása:

{{< highlight php >}}
$katalogus = [
    new DVD("Star Wars"),
    new Konyv("A Gyűrűk Ura"),
];
foreach($katalogus as $e) {
    // Nem gond, ha nincs interfész,
    // amíg ez a függvény létezik minden osztályon:
    print($e->getMegjelenitendoNev());
}
{{< /highlight >}}

A cél itt főleg a hibaellenőrzés és a dokumentálás


