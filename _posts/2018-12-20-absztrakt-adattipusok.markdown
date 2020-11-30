---
layout: post
title:  "Absztrakt adattípusok"
date:   2018-12-20 10:00:00 +0100
tags: 14evfolyam adatszerkezetek oop programozas
---

Az adatszerkezeteket kétféleképp is megkülönböztethetjük.

Felépítés szerint (pl. tömb, fa, hash tábla), vagy pedig a rajtuk elvégezhető műveletek szerint:

* Adatszerkezetek (a felépítés szerinti)
* Absztrakt adattípusok (műveletek szerint)

Itt most az utóbbiból mutatok be néhányat.

## Verem

* Angolul stack
* LIFO (Last-in First-out)

A verem két műveletet definiál:

* push -> beleteszegy elemet
* pop -> kiveszi azt az elemet, amelyet _legutóljára_ tettünk bele.

Az alábbi módon tudjuk elképzelni:

<pre>
v = új üres Verem()    // []
v.push(5)              // [5]
v.push(10)             // [5, 10]
v.push(-1)             // [5, 10, -1]
Kiír(v.pop())          // [5, 10]       Ki: -1
Kiír(v.pop())          // [5]           Ki: 10
</pre>

## Sor

* Angolul queue
* FIFO (First-in First-out)

A sor szintén két műveletet definiál:

* enqueue -> beleteszegy elemet
* dequeue -> kiveszi azt az elemet, amelyet _legelőször_ tettünk bele.

<pre>
s = új üres Sor()         // []
s.enqueue(5)              // [5]
s.enqueue(10)             // [5, 10]
s.enqueue(-1)             // [5, 10, -1]
Kiír(s.dequeue())         // [10, -1]       Ki:  5
Kiír(s.dequeue())         // [-1]           Ki: 10
</pre>

## Prioritási sor

* Angolul priority queue
* A kivétel sorrendje nem féltétlen a berakás sorrendjében történik
  * Az elemeket sorrendbe kell tudni rendezni
  * A legkisebb értéket veszi ki

<pre>
ps = új üres PriritásiSor()    // []
ps.hozzáad(5)                  // [5]
ps.hozzáad(10)                 // [5, 10]
ps.hozzáad(-1)                 // [5, 10, -1]
Kiír(ps.kivesz())              // [5, 10]       Ki: -1
Kiír(ps.kivesz())              // [10]          Ki:  5
</pre>

## Halmaz

* Angolul set
* Az elemeknek nincs előre definiált sorrendje
* Minden elem csak egyszer szerepelhet
* A leggyakrabban használt művelet a "bennevan(): logikai", általában erre van optimizálva

<pre>
h = új üres Halmaz()     // []
h.hozzáad(5)             // [5]
h.hozzáad(1)             // [5, 1]
h.hozzáad(11)            // [5, 1, 11]
h.hozzáad(5)             // [5, 1, 11]
h.bennevan(2)            // Hamis
h.bennevan(5)            // Igaz
</pre>

## Szótár

* Angolul map
  * Map: párosítás
  * Dictionary: szótár
  * Asszociatív tömb: associative array
* Minden értékhez tartozik egy **egyedi** kulcs, amely alapján lehet az értékeket kikeresni

<pre>
sz = új üres Szótár()
sz.hozzáad(5, "Gyurika")
sz.hozzáad(9, "Petike")
sz[5]                       // "Gyurika"
sz[10]                      // HIBA: nincs 10-es kulcs
</pre>

A kulcs nem csak szám lehet, persze:

<pre>
sz = új üres Szótár()
sz.hozzáad("Gyurika", 5.6)
sz.hozzáad("Petike", -8.7)
sz["Petike"]                // -8.7
sz["Csőrike"]               // HIBA: nincs "Csőrike" kulcs
</pre>

## Programkódban

Programkódban természetesen nem csak a fenti pár művelet van definiálva, pl. szinte minden osztályon létezik a Count tulajdonság, a Contains() függvény stb. Érdemes használatkor átnézni őket.

### C#

<pre><code class="csharp">var stack = new Stack&lt;int>();
stack.Push(5);
stack.Push(10);
stack.Push(-1);
Console.WriteLine(stack.Pop()); // -1
Console.WriteLine(stack.Pop()); // 10

var queue = new Queue&lt;int>();
queue.Enqueue(5);
queue.Enqueue(10);
queue.Enqueue(-1);
Console.WriteLine(queue.Dequeue()); // 5
Console.WriteLine(queue.Dequeue()); // 10

// .NET-ben nincs beépített prioritási sor osztály,
// de az alábbi NuGet csomag segít:
// https://www.nuget.org/packages/OptimizedPriorityQueue/
// Az értéknek nem kell megegyeznie a prioritással!
var pq = new SimplePriorityQueue&lt;int, int>();
pq.Enqueue(5, 5);
pq.Enqueue(10, 10);
pq.Enqueue(-1, -1);
Console.WriteLine(pq.Dequeue()); // 5
Console.WriteLine(pq.Dequeue()); // 10

var s = new HashSet&lt;int>();
s.Add(5);
s.Add(1);
s.Add(11);
s.Add(5);   // false visszatérési értékkel jelez, hogy már benne van
Console.WriteLine(s.Count);         // 3
Console.WriteLine(s.Contains(2));   // false
Console.WriteLine(s.Contains(5));   // true

var d = new Dictionary&lt;string, double>();
d.Add("Gyurika", 5.6);
d.Add("Petike", -8.7);
Console.WriteLine(d.ContainsKey("Csőrike"));  // hamis
Console.WriteLine(d["Petike"]);               // -8.7
Console.WriteLine(d["Csőrike"]);              // KeyNotFoundException
</code></pre>

## Java

Java-ban a verem és sor adatszerkezeteket az ArrayDeque osztályon keresztül használhatjuk.

<pre><code class="java">Deque&lt;Integer> stack = new ArrayDeque&lt;>();
// A végére rekjuk az elemet
stack.addLast(5);
stack.addLast(10);
stack.addLast(-1);
// ...és onnan is vesszük ki
System.out.println(stack.removeLast()); // -1
System.out.println(stack.removeLast()); // 10

// Ugyanaz az osztály, de a Queue interface-en keresztül használjuk
Queue&lt;Integer> queue = new ArrayDeque&lt;>();
queue.add(5);
queue.add(10);
queue.add(-1);
System.out.println(queue.remove()); // 5
System.out.println(queue.remove()); // 10

Queue&lt;Integer> pq = new PriorityQueue&lt;>();
pq.add(5);
pq.add(10);
pq.add(-1);
System.out.println(pq.remove()); // 5
System.out.println(pq.remove()); // 10

Set&lt;Integer> s = new HashSet&lt;>();
s.add(5);
s.add(1);
s.add(11);
s.add(5);   // false visszatérési értékkel jelez, hogy már benne van
System.out.println(s.size());        // 3
System.out.println(s.contains(2));   // false
System.out.println(s.contains(5));   // true

Map&lt;String, Double> d = new HashMap&lt;>();
d.put("Gyurika", 5.6);
d.put("Petike", -8.7);
System.out.println(d.containsKey("Csőrike"));  // hamis
System.out.println(d.get("Petike"));           // -8.7
System.out.println(d.get("Csőrike"));          // null
</code></pre>

### PHP

A PHP csak korlátozottan taralmaz dedikált adatszerkezeteket, amelyekkel a műveletek megvalósíthatók.

A tömbhöz azonban rengeteg segédfüggvény jár, amivel a verem/sor megoldható:

<pre><code class="php">$stack = [];
$stack[] = 5;  // push művelet
$stack[] = 10;
$stack[] = -1;
print( array_pop($stack) ); // -1
print( array_pop($stack) ); // 10

$queue = [];
$queue[] = 5;  // enqueue művelet, ua. mint fent
$queue[] = 10;
$queue[] = -1;
print( array_shift($queue) ); // 5  - az elejéről veszi le, azaz dequeue
print( array_shift($queue) ); // 10
</code></pre>

A PHP-s prioritási sor "fordítva" működik, először a legnagyobb értéket veszi ki.
Itt is meg lehet adni különböző prioritást az elemhez képest:

<pre><code class="php">$pq = new SplPriorityQueue();
$pq->insert(5, 5);
$pq->insert(10, 10);
$pq->insert(-1, -1);
print( $pq->extract() ); // 10 (legnagyobb)
print( $pq->extract() ); // 5
</code></pre>

Ha az elemeink, kulcsaink egész vagy string típusúak, akkor a tömbből akár halmaz és szótár is lehet.

<pre><code class="php">$set = [];
$set[5] = true;            // Hozzáadás a halmazhoz
$set[1] = true;
$set[11] = true;
$set[5] = true;            // Duplán hozzáadás sem gond
print( count($set) );      // 3
print( isset($set[2]) );   // false
print( isset($set[5]) );   // true
unset($set[11]);           // Törlés

$dict = [];
$dict["Gyurika"] = 5.6;            // Hozzáadás a szótárhoz
$dict["Petike"] = -8.7;
print( isset($dict["Csőrike"]) );  // hamis
print( $dict["Petike"] );          // -8.7
print( $dict["Csőrike"] );         // Notice: Undefined index: Csőrike
unset($dict["Petike"]);            // Törlés
</code></pre>

Mivel egy tömb kulcsa csak egész szám vagy szöveg lehet, ez a módszer más típusokra nem működik. Ha halmaz elem, vagy a szótár kulcsa objektum, használhatjuk még az [SplObjectStorage](http://php.net/manual/en/class.splobjectstorage.php) osztályt.
