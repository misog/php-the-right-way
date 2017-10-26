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

#### Syntax Nowdoc

Syntax Nowdoc bola zavedená v PHP 5.3 a interne sa správa rovnako ako jednoduché úvodzovky. Okrem toho je vhodná
pre použitie v mnohoriadkových reťazcoch bez potreby spájania.

{% highlight php %}
<?php
$str = <<<'EOD'             // inicializované s <<<
Example of string
spanning multiple lines
using nowdoc syntax.
$a does not parse.
EOD;                        // ukončenie 'EOD' musí byť na samostatnom riadku bez odsadenia

/**
 * Výstup:
 *
 * Example of string
 * spanning multiple lines
 * using nowdoc syntax.
 * $a does not parse.
 */
{% endhighlight %}

* [Syntax Nowdoc](http://php.net/language.types.string#language.types.string.syntax.nowdoc)

#### Syntax Heredoc

Syntax Heredoc sa interne správa rovnako ako dvojité úvodzovky. Okrem toho je vhodná pre použitie
v mnohoriadkových reťazcoch bez potreby spájania.

{% highlight php %}
<?php
$a = 'Variables';

$str = <<<EOD               // inicializované s <<<
Example of string
spanning multiple lines
using heredoc syntax.
$a are parsed.
EOD;                        // ukončenie 'EOD' musí byť na samostatnom riadku bez odsadenia

/**
 * Výstup:
 *
 * Example of string
 * spanning multiple lines
 * using heredoc syntax.
 * Variables are parsed.
 */
{% endhighlight %}

* [Syntax Heredoc](http://php.net/language.types.string#language.types.string.syntax.heredoc)

### Ktorá syntax je rýchlejšia?

Okolo reťazcov v jednoduchých úvodzovkách je mýtus že sú oveľa rýchlejšie ako tie v dvojitých. Toto v podstate
nie je pravda.

Ak zadefinujete jeden reťazec a nesnažíte sa spájať hodnoty, alebo niečo iné komplikované, potom oba reťazce
budú identické. Ani jeden z nich nie je rýchlejší.

Ak spájate niekoľko reťazcov akéhokoľvek typu, alebo interpolujete hodnoty v reťazcoch v dvojitých úvodzovkách, potom
sa hodnoty môžu líšiť. Ak pracujete s malým množstvom hodnôt, potom je zreťazenie rýchlejšie. Pre väčší počet
hodnôt je naopak rýchlejšia interpolácia.

Bez ohľadu na to, čo s reťazcami robíte, ani jeden z typov nebude mať nikdy na vašu aplikáciu výrazný vplyv. Snaha
o prepísanie kódu pre použitie jedného, alebo druhého je zbytočná práca, takže sa snažte vyhnúť tejto mikrooptimalizácii
pokiaľ naozaj nechápete význam a vplyv medzi rozdielmi.

* [Disproving the Single Quotes Performance Myth](http://nikic.github.io/2012/01/09/Disproving-the-Single-Quotes-Performance-Myth.html)


## Ternárne operátory

Ternárne operátory sú výbornou cestou, ako zhustiť kód. Často sú ale používané prebytočne. Pokiaľ môžu byť ternárne
operátory vetvené/vnorené, odporúča sa pre lepšiu čitateľnosť používať jeden na riadok.

{% highlight php %}
<?php
$a = 5;
echo ($a == 5) ? 'yay' : 'nay';
{% endhighlight %}

Pre porovnanie je tu príklad, ktorý na úkor redukcie príkazu na jeden riadok obetuje všetky formy čitateľnosti.

{% highlight php %}
<?php
echo ($a) ? ($a == 5) ? 'yay' : 'nay' : ($b == 10) ? 'excessive' : ':(';    // nadmerné vetvenie, obetovanie čitateľnosti
{% endhighlight %}

Pre návratovú hodnotu s ternárnymi operátormi použite správnu syntax. 

{% highlight php %}
<?php
$a = 5;
echo ($a == 5) ? return true : return false;    // tento príklad vráti chybu

// vs

$a = 5;
return ($a == 5) ? 'yay' : 'nope';    // návratová hodnota z tohto príkladu bude 'yay'

{% endhighlight %}

Treba poznamenať, že pre návratovú hodnoty typu bool nie je potrebné použiť ternárny operátor. Príklad:

{% highlight php %}
<?php
$a = 3;
return ($a == 3) ? true : false; // návratová hodnota bude true ak $a == 3, false v ostatných prípadoch

// vs

$a = 3;
return $a == 3; // návratová hodnota bude true ak $a == 3, false v ostatných prípadoch

{% endhighlight %}

Toto platí pre všetky operátory (===, !==, !=, == etc).

#### Použitie zátvoriek s ternárnymi operátormi pre formu a funkciu

Pri používaní ternárnych operátorov môžu zátvorky zvýšiť čitateľnosť kódu. Príklad, pri ktorom nie je potreba
použiť zátvorky:

{% highlight php %}
<?php
$a = 3;
return ($a == 3) ? "yay" : "nope"; // návratová hodnota bude 'yay' ak $a == 3, 'nope' v ostatných prípadoch

// vs

$a = 3;
return $a == 3 ? "yay" : "nope"; // návratová hodnota bude 'yay' ak $a == 3, 'nope' v ostatných prípadoch
{% endhighlight %}

Použitie zátvoriek taktiež poskytuje možnosť vytvorenia zložených podmienok v rámci výrazu, ktoré budú
vyhodnotené ako celok. Nasledujúci príklad bude mať návratovú hodnotu true ak obe podmienky `($a == 3 AND $b == 4)`
a súčasne `$c == 5` sú vyhodnotené ako true.

{% highlight php %}
<?php
return ($a == 3 && $b == 4) && $c == 5;
{% endhighlight %}

Ďalším príkladom je nasledujúci útržok kódu, ktorý má návratovú hodnotu true ak `($a != 3 AND $b != 4) OR $c == 5`.

{% highlight php %}
<?php
return ($a != 3 && $b != 4) || $c == 5;
{% endhighlight %}

Od PHP verzie 5.3 je možné vynechať strednú časť ternárneho operátora.
Výraz "`expr1 ?: expr3`" vráti `expr1` ak výraz `expr1` je vyhodnotený ako TRUE, inak vráti hodnotu expr3.

* [Ternárne operátory](http://php.net/language.operators.comparison)

## Deklarácia premenných

Kedysi sa programátori pokúšali deklaráciou preddefinovaných premenných s rozdielnymi menami spraviť
ich kód "čistejší". Čo toto v skutočnosti spôsobuje je dvojnásobné množstvo pamäte, ktorú program používa.
Povedzme, že v nasledujúcom príklade obsahuje reťazec 1MB dát. Vytvorením premennej vzrastie potreba pamäte na 2MB.

{% highlight php %}
<?php
$about = 'A very long string of text';    // používa 2MB pamäte
echo $about;

// vs

echo 'A very long string of text';        // používa 1MB pamäte
{% endhighlight %}

* [Performance tips](http://web.archive.org/web/20140625191431/https://developers.google.com/speed/articles/optimizing-php)
