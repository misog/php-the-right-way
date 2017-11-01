---
title: Dátum a čas
isChild: true
anchor: date_and_time
---

## Dátum a čas {#date_and_time_title}

Pre pomoc s čítaním, zapisovaním, porovnávaním, alebo výpočtom dátumu môžete použiť triedu DateTime. Okrem triedy
DateTime je v PHP pre prácu s dátumom mnoho ďalších funkcií. Trieda DateTime však poskytuje pekné objektovo orientované
rozhranie pre najbežnejšie použitie. Okrem iných dokáže napríklad narábať s časovými pásmami.

Pre začiatok práce s triedou DateTime môžete skonvertovať „surový“ dátum a čas na objekt pomocou metódy továrne
`createFromFormat()`, alebo vytvorením novej inštancie pomocou `new DateTime`. Použitím novej inštancie triedy získate
objekt s aktuálnym dátumom a časom. Pre konverziu objektu späť do reťazca môžete použiť metódu `format()`.

{% highlight php %}
<?php
$raw = '22. 11. 1968';
$start = DateTime::createFromFormat('d. m. Y', $raw);

echo 'Start date: ' . $start->format('Y-m-d') . "\n";
{% endhighlight %}

Výpočty pre DateTime je možné robiť pomocou triedy DateInterval. Trieda DateTime má metódy ako `add()` pre pripočítanie
a `sub()` pre odpočítanie dátumov, ktorých argumentom je DateInterval. Kód, ktorý predpokladá rovnaký počet sekúnd každý
deň nie je príliš vhodný. Zmeny v zimnom a letnom čase, ako aj časové pásma totiž môžu tento predpoklad narušiť. 
Na miesto toho je vhodné použiť časové intervaly. Pre vypočítanie rozdielu medzi dátumami použite metódu `diff()`.
Táto metóda vráti objekt typu DateInterval, ktorý je následne jednoduchý na zobrazenie.

{% highlight php %}
<?php
// vytvorí kópiu premennej $start a pridá jeden mesiac a šesť dní
$end = clone $start;
$end->add(new DateInterval('P1M6D'));

$diff = $end->diff($start);
echo 'Difference: ' . $diff->format('%m month, %d days (total: %a days)') . "\n";
// Difference: 1 month, 6 days (total: 37 days)
{% endhighlight %}

Na objektoch typu DateTime môžete použiť štandardné porovnávanie:

{% highlight php %}
<?php
if ($start < $end) {
    echo "Start is before end!\n";
}
{% endhighlight %}

Posledným príkladom je demonštrácia použitia triedy DatePeriod, ktorá slúži na iteráciu cez opakujúce sa udalosti.
Argumentom konštruktora triedy sú dva objekty typu DateTime, začiatok a koniec, a interval, pre ktorý vráti
všetky udalosti.

{% highlight php %}
<?php
// vráti všetky štvrtky medzi $start a $end
$periodInterval = DateInterval::createFromDateString('first thursday');
$periodIterator = new DatePeriod($start, $periodInterval, $end, DatePeriod::EXCLUDE_START_DATE);
foreach ($periodIterator as $date) {
    // vypíše všetky dátumy v časovom rozsahu
    echo $date->format('Y-m-d') . ' ';
}
{% endhighlight %}

* [Prečítajte si o DateTime][datetime]
* [Precitajte si o formátovaní dátumu][dateformat]

[datetime]: http://php.net/book.datetime
[dateformat]: http://php.net/function.date
