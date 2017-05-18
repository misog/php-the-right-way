---
layout: page
title: Základy
sitemap: true
---

# Základy

## Relačné operátory

Relačné operátory sú často prehliadaným aspektom PHP, čo môže viesť k mnohým nečakaným výsledkom. Jeden z takýchto
problémov pramení v použití striktného porovnávania (porovnávanie boolean ako integer). 

{% highlight php %}
<?php
$a = 5;                  // číslo 5 ako integer 

var_dump($a == 5);       // porovnanie hodnôt; návratová hodnota true
var_dump($a == '5');     // porovnanie hodnôt (ignorovanie typov); návratová hodnota true
var_dump($a === 5);      // porovnanie typov a hodnôt (integer vs. integer); návratová hodnota true
var_dump($a === '5');    // porovnanie typov a hodnôt (integer vs. string); návratová hodnota false

// Porovnávanie rovnosti
if (strpos('testovanie', 'test')) {  // 'test' sa nachádza na pozícii 0, ktorá je interpretovaná ako hodnota boolean 'false'
    // kód...
}

// ... oproti striktnému porovnávaniu
if (strpos('testovanie', 'test') !== false) {  //  na základe striktného porovnávania (0 !== false) je výsledok 'true'
    // kód...
}
{% endhighlight %}

* [Relačné operátory](http://php.net/language.operators.comparison)
* [Tabuľka pre porovnanie typov](http://php.net/types.comparisons)
* [Ťahák pre porovnanie typov](http://phpcheatsheets.com/index.php?page=compare)

## Príkazy vetvenia

### Príkaz if

Pri použití príkazov 'if/else' vo funkcií, alebo metóde triedy je bežnou mylnou predstavou, že pre deklaráciu možných
následkov musí byť v spojení s 'if' použitý aj príkaz 'else'. Ak je ale požadovaným výsledkom návratová hodnota,
potom príkaz 'else' nie je potrebný. Príkaz 'return' v tomto prípade ukončí funkciu, čím sa použitie 'else' stáva
diskutabilné.

{% highlight php %}
<?php
function test($a)
{
    if ($a) {
        return true;
    } else {
        return false;
    }
}

// oproti použitiu

function test($a)
{
    if ($a) {
        return true;
    }
    return false;    // 'else' nie je potrebné
}

// prípadne ešte kratšieho zápisu

function test($a)
{
    return (bool) $a;
}

{% endhighlight %}

* [Príkaz if v PHP manuále](http://php.net/control-structures.if)

### Príkaz switch

Použitie príkazu switch je výborným spôsobom ako predísť nekonečným vetveniam, ku ktorým môže dôjsť v prípade
použitia 'if' a 'elseif'. Pri použití je ale treba dávať pozor na niekoľko vecí:

- príkaz switch porovnáva hodnoty, ale neporovnáva typ (ekvivalent k '==')
- príkaz iteruje cez všetky vetvy až pokiaľ nenájde zhodu. Ak sa zhoda nenájde, potom použije vetvu default (ak je definovaná)
- bez použitia 'break' vo vetve, príkaz pokračuje v iterácii až pokiaľ na break, alebo return nenarazí 
- v prípade použitia príkazu 'return' je ďalšie použitie 'break' zbytočné, nakoľko return funkciu ukončí

{% highlight php %}
<?php
$answer = test(2);    // vykoná kód vo vetvách 'case 2' aj 'case 3'

function test($a)
{
    switch ($a) {
        case 1:
            // kód...
            break;            // príkaz break je určený pre ukončenie podmienky príkazu switch
        case 2:
            // kód...         // bez použitia break bude porovnávanie pokračovať ďalej vo vetve 'case 3'
        case 3:
            // kód...
            return $result;   // 'return' ukončí beh funkcie
        default:
            // kód...
            return $error;
    }
}
{% endhighlight %}

* [Príkaz switch v PHP manuále](http://php.net/control-structures.switch)
* [PHP switch](http://phpswitch.com/)

## Globálne menné priestory

Pri použití menných priestorov, sa vám môže stať, že interné funkcie budú skryté za funkciami, ktoré ste napísali vy.
Pre použitie takýchto globálnych funkcií, použite spätné lomítko pred názvom funkcie.

{% highlight php %}
<?php
namespace phptherightway;

function fopen()
{
    $file = \fopen();    // Názov vašej funkcie je rovnaký, ako názov internej funkcie.
                         // Vyvolá funkciu z globálneho menného priestory pomocou pridania '\'. 
}

function array()
{
    $iterator = new \ArrayIterator();    // ArrayIterator je interná trieda. Pri použití tejto triedy bez spätného lomítka
                                         // sa ju PHP najprv pokúsi nájsť v rámci vášho menného priestoru.
}
{% endhighlight %}

* [Globálne menné priestory](http://php.net/language.namespaces.global)
* [Globálne pravidlá](http://php.net/userlandnaming.rules)

## Reťazce

### Spájanie reťazcov

- ak dĺžka riadku presiahne doporučenú dĺžku (120 znakov), zvážte spájanie riadkov
- pre lepšiu čitateľnosť je lepšie použiť operátor zreťazenia než zreťazujúci operátor priradenia
- ak zreťazujete riadky v pôvodnom rozsahu premennej, potom nové zreťazené riadky zarovnajte


{% highlight php %}
<?php
$a  = 'Multi-line example';    // zreťazujúci operátor priradenia (.=)
$a .= "\n";
$a .= 'of what not to do';

// vs

$a = 'Multi-line example'      // operátor zreťazenia (.)
    . "\n"                     // odsadenie nových riadkov
    . 'of what to do';
{% endhighlight %}

* [Reťazcové operátory](http://php.net/language.operators.string)

### Druhy reťazcov

Reťazce sú sériou znakov, čo by malo znieť pomerne jednoducho. Je niekoľko druhov reťazcov. Každý druh má mierne
odlišnú syntax a mierne odlišné správanie. 

#### Jednoduché úvodzovky

Jednoduché úvodzovky slúžia na označovanie "doslovných reťazcov". Doslovné reťazce sa nepokúšajú parsovať
špeciálne znaky, alebo premenné.

Ak použijete jednoduché úvodzovky, názov premennej môžete vložiť do reťazca nasledovne: `'nejaká $vec'`. Na výstupe
následne uvidíte presne výstup `nejaká $vec`. Pri použití dvojitých úvodzoviek sa PHP pokúsi vyhodnotiť premennú `$vec`
a ak premenná nebola nájdená, potom zobrazí chyby.


{% highlight php %}
<?php
echo 'Toto je môj reťazec. Pozri aký je krásny.';    // jednoduchý reťazec nie je nutné parsovať

/**
 * Výstup:
 *
 * Toto je môj reťazec. Pozri aký je krásny.
 */
{% endhighlight %}

* [Jednoduché úvodzovky](http://php.net/language.types.string#language.types.string.syntax.single)

#### Dvojité úvodzovky

Dvojité úvodzovky sú švajčiarskym nožom reťazcov. Nielenže parsujú premenné, ako bolo spomenuté, ale 
aj všetky druhy špeciálnych znakov, ako `\n` pre nový riadok, `\t` pre tabulátor, atď.

{% highlight php %}
<?php 
echo 'phptherightway is ' . $adjective . '.'  // príklad reťazca s jednoduchými úvodzovkami,
    . "\n"                                    // ktorý používa niekoľko zreťazení pre
    . 'I love learning ' . $code . '!';       // premenné a špeciálne znaky

// vs

echo "phptherightway is $adjective.\n I love learning $code!" // Použitie dvojitých úvodzoviek na miesto zreťazenia
                                                              // umožňuje použitie parsovateľných reťazcov
                                                               
{% endhighlight %}

Dvojité úvodzovky môžu obsahovať premenné. Toto sa nazýva "interpolácia".

{% highlight php %}
<?php
$juice = 'plum';
echo "I like $juice juice";    // Output: I like plum juice
{% endhighlight %}

Pri použití interpolácie sa často stáva, že premenná interferuje s iným znakom. Toto môže mať za následok zmätok
v tom, čo je názov premennej a čo je doslovný znak.

Pre vyriešenie tohto problému možno obaliť premennú v kučeravých zátvorkách.

{% highlight php %}
<?php
$juice = 'plum';
echo "I drank some juice made of $juices";    // premenná $juice nemôže byť parsovaná

// vs

$juice = 'plum';
echo "I drank some juice made of {$juice}s";    // premenná $juice bude parsovaná

/**
 * Komplexné premenné budú taktiež parsované v rámci kučeravých zátvoriek
 */

$juice = array('apple', 'orange', 'plum');
echo "I drank some juice made of {$juice[1]}s";   // $juice[1] bude parsované
{% endhighlight %}

* [Dvojité úvodzovky](http://php.net/language.types.string#language.types.string.syntax.double)

#### Nowdoc syntax

Nowdoc syntax was introduced in 5.3 and internally behaves the same way as single quotes except it is suited toward the
use of multi-line strings without the need for concatenating.

{% highlight php %}
<?php
$str = <<<'EOD'             // initialized by <<<
Example of string
spanning multiple lines
using nowdoc syntax.
$a does not parse.
EOD;                        // closing 'EOD' must be on it's own line, and to the left most point

/**
 * Output:
 *
 * Example of string
 * spanning multiple lines
 * using nowdoc syntax.
 * $a does not parse.
 */
{% endhighlight %}

* [Nowdoc syntax](http://php.net/language.types.string#language.types.string.syntax.nowdoc)

#### Heredoc syntax

Heredoc syntax internally behaves the same way as double quotes except it is suited toward the use of multi-line
strings without the need for concatenating.

{% highlight php %}
<?php
$a = 'Variables';

$str = <<<EOD               // initialized by <<<
Example of string
spanning multiple lines
using heredoc syntax.
$a are parsed.
EOD;                        // closing 'EOD' must be on it's own line, and to the left most point

/**
 * Output:
 *
 * Example of string
 * spanning multiple lines
 * using heredoc syntax.
 * Variables are parsed.
 */
{% endhighlight %}

* [Heredoc syntax](http://php.net/language.types.string#language.types.string.syntax.heredoc)

### Which is quicker?

There is a myth floating around that single quote strings are fractionally quicker than double quote strings. This is
fundamentally not true.

If you are defining a single string and not trying to concatenate values or anything complicated, then either a single
or double quoted string will be entirely identical. Neither are quicker.

If you are concatenating multiple strings of any type, or interpolate values into a double quoted string, then the
results can vary. If you are working with a small number of values, concatenation is minutely faster. With a lot of
values, interpolating is minutely faster.

Regardless of what you are doing with strings, none of the types will ever have any noticeable impact on your
application. Trying to rewrite code to use one or the other is always an exercise in futility, so avoid this micro-
optimization unless you really understand the meaning and impact of the differences.

* [Disproving the Single Quotes Performance Myth](http://nikic.github.io/2012/01/09/Disproving-the-Single-Quotes-Performance-Myth.html)


## Ternary operators

Ternary operators are a great way to condense code, but are often used in excess. While ternary operators can be
stacked/nested, it is advised to use one per line for readability.

{% highlight php %}
<?php
$a = 5;
echo ($a == 5) ? 'yay' : 'nay';
{% endhighlight %}

In comparison, here is an example that sacrifices all forms of readability for the sake of reducing the line count.

{% highlight php %}
<?php
echo ($a) ? ($a == 5) ? 'yay' : 'nay' : ($b == 10) ? 'excessive' : ':(';    // excess nesting, sacrificing readability
{% endhighlight %}

To 'return' a value with ternary operators use the correct syntax.

{% highlight php %}
<?php
$a = 5;
echo ($a == 5) ? return true : return false;    // this example will output an error

// vs

$a = 5;
return ($a == 5) ? 'yay' : 'nope';    // this example will return 'yay'

{% endhighlight %}

It should be noted that you do not need to use a ternary operator for returning a boolean value. An example of this
would be.

{% highlight php %}
<?php
$a = 3;
return ($a == 3) ? true : false; // Will return true or false if $a == 3

// vs

$a = 3;
return $a == 3; // Will return true or false if $a == 3

{% endhighlight %}

This can also be said for all operations(===, !==, !=, == etc).

#### Utilising brackets with ternary operators for form and function

When utilising a ternary operator, brackets can play their part to improve code readability and also to include unions
within blocks of statements. An example of when there is no requirement to use bracketing is:

{% highlight php %}
<?php
$a = 3;
return ($a == 3) ? "yay" : "nope"; // return yay or nope if $a == 3

// vs

$a = 3;
return $a == 3 ? "yay" : "nope"; // return yay or nope if $a == 3
{% endhighlight %}

Bracketing also affords us the capability of creating unions within a statement block where the block will be checked
as a whole. Such as this example below which will return true if both ($a == 3 and $b == 4) are true and $c == 5 is
also true.

{% highlight php %}
<?php
return ($a == 3 && $b == 4) && $c == 5;
{% endhighlight %}

Another example is the snippet below which will return true if ($a != 3 AND $b != 4) OR $c == 5.

{% highlight php %}
<?php
return ($a != 3 && $b != 4) || $c == 5;
{% endhighlight %}

Since PHP 5.3, it is possible to leave out the middle part of the ternary operator.
Expression "expr1 ?: expr3" returns expr1 if expr1 evaluates to TRUE, and expr3 otherwise.

* [Ternary operators](http://php.net/language.operators.comparison)

## Variable declarations

At times, coders attempt to make their code "cleaner" by declaring predefined variables with a different name. What
this does in reality is to double the memory consumption of said script. For the example below, let us say an example
string of text contains 1MB worth of data, by copying the variable you've increased the scripts execution to 2MB.

{% highlight php %}
<?php
$about = 'A very long string of text';    // uses 2MB memory
echo $about;

// vs

echo 'A very long string of text';        // uses 1MB memory
{% endhighlight %}

* [Performance tips](http://web.archive.org/web/20140625191431/https://developers.google.com/speed/articles/optimizing-php)
