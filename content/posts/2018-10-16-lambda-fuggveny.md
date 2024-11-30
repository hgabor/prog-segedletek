---
title:  "Névtelen/lambda függvények"
date:   2018-10-16 10:00:00 +0100
tags:
 - programozas
---

Függényeket általában akkor írunk, ha egy programkódot, kódrészletet többször, akár különböző paraméterekkel szeretnénk végrehajtani.
Akkor is hasznos, ha a kódunkat kisebb, logikai részekre szeretnénk bontani.

Van azonban egy fontos haszna még: a keretrendszer által generált különféle események (pl. egy gombra kattintás) kezelésére.
Ebben az esetben általában csak egy helyen használnánk a függényt: az eseményre feliratkozáskor. Pl.:

{{< highlight csharp >}}
public class Form1: Form
{
    Button btn = new Button();

    public Form1()
    {
        btn.Text = "Katt";
        this.Controls.Add(btn);
        // Nincs (), mert itt a btn_Clicked nem függvényhívás -
        // nem a végeredményt várjuk, hanem magára a függvényre hivatkozunk
        btn.Click += btn_Clicked;
    }

    private void btn_Clicked(object sender, EventArgs e)
    {
        MessageBox.Show("Kattintottal");
    }
}
{{< /highlight >}}

A példában a btn_Clicked függvény csak azért szerepel, hogy a kattintás eseményre reagálhassunk. Nem fogjuk máskor meghívni, többször felhasználni,
de mégis fel kellett vennünk egy külön függvényt. Vagy mégsem?

## Lambda függvények

A névtelen (anonymous) függvény, vagy **lambda függvény** egy olyan függvény, amit rögtön a felhasználás helyén definiálunk. Ekkor még nevet sem kell adnunk, mivel később úgysem fogunk hivatkozni rá. A legtöbb programnyelven még a típusokat sem kell kiírni, mert a felhasználás helyéből kiderül, hogy a paramétereknek/visszatérési értéknek milyen típusúnak kell lennie.

A fenti példával ekvivalens lambda függvény:

{{< highlight csharp >}}
(sender, e) => {
    MessageBox.Show("Kattintottal");
}
{{< /highlight >}}

A teljes kód pedig:

{{< highlight csharp >}}
public class Form1: Form
{
    Button btn = new Button();

    public Form1()
    {
        btn.Text = "Katt";
        this.Controls.Add(btn);
        btn.Click += (sender, e) => {
            MessageBox.Show("Kattintottal");
        };
    }
}
{{< /highlight >}}

Ha nincs paraméter, akkor üres zárójeleket írunk. Megadhatunk visszatérési értéket is:

{{< highlight csharp >}}
() => {
    Console.WriteLine("Esemeny");
    return 2;
}
{{< /highlight >}}

## Más programozási nyelveken

### JavaScript

{{< highlight html >}}
<button id='gomb'>Katt</button>
{{< /highlight >}}

{{< highlight javascript >}}
let button = document.getElementById('gomb');
button.addEventListener('click', () => { window.alert('Kattintottal'); });
{{< /highlight >}}

Régebbi, kompatibilis szintaxissal:

{{< highlight javascript >}}
let button = document.getElementById('gomb');
button.addEventListener('click', function() {
    window.alert('Kattintottal');
});
{{< /highlight >}}

### Java

Ugyanaz, mint C#-ban vagy JavaScript-ben, de a dupla => nyíl helyett -> szimpla nyilat használunk.

{{< highlight java >}}
public class NewJFrame extends javax.swing.JFrame {
    public NewJFrame() {
        initComponents();

        jButton1.addActionListener((e) -> {
            JOptionPane.showMessageDialog(this, "Kattintottal");
        });
    }
{{< /highlight >}}

## A lambda függvények egyéb felhasználási területei (példák):

### C#

Rendezzük ABC szerint csökkenő sorrendbe az alábbi osztály objektumait:

{{< highlight csharp >}}
class Szemely
{
    public string Nev { get; private set; }
    public DateTime SzulDat { get; private set; }

    public Szemely(string nev, DateTime szulDat)
    {
        this.Nev = nev;
        this.SzulDat = szulDat;
    }
}
{{< /highlight >}}

A rendezés:

{{< highlight csharp >}}
var list = new List&lt;Szemely>();
list.Add(new Szemely("Kovacs Bela", new DateTime(2000, 1, 1)));
list.Add(new Szemely("Molnar Julianna", new DateTime(1970, 10, 10)));
list.Add(new Szemely("Telapo", new DateTime(1600, 12, 6)));

// A Sort függvény egy összehasonlító függvényt vár paraméterként:
list.Sort((sz1, sz2) => {
    int compareResult = sz1.Nev.CompareTo(sz2.Nev);
    if (compareResult > 0)
    {
        return -1;
    }
    else if (compareResult < 0)
    {
        return 1;
    }
    else
    {
        return 0;
    }
});
{{< /highlight >}}

Ugyanez tömörebben:

{{< highlight csharp >}}
var list = new List&lt;Szemely>();
list.Add(new Szemely("Kovacs Bela", new DateTime(2000, 1, 1)));
list.Add(new Szemely("Molnar Julianna", new DateTime(1970, 10, 10)));
list.Add(new Szemely("Telapo", new DateTime(1600, 12, 6)));

// A negatív előjel miatt fordított sorrendű lesz a rendezés
list.Sort((sz1, sz2) => -(sz1.Nev.CompareTo(sz2.Nev)));
{{< /highlight >}}

A tömör példában, ha nem írunk kapcsos zárójeleket, akkor csak egy kifejezést lehet megadni, amit automatikusan visszatérési értékként használ fel.

### PHP

Válogassuk ki a tömb nemnegatív elemeit:

{{< highlight php >}}
$t = [4, -6, 234, -78.88, -9, 0];

// Az array_filter paramétere az egy elem kiválasztását eldöntő függvény
$kivalogatott = array_filter($t, function($elem) {
    return $elem >= 0;
});
{{< /highlight >}}

A legtöbb programozási tételre létezik beépített függvény, a kiválogatási, összehasonlítási feltételeket pedig (lambda vagy hagyományos) függvényként lehet átadni.
