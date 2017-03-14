---
title: Výnimky
isChild: true
anchor: exceptions
---

## Výnimky {#exceptions_title}

Výnimky sú štandardnou súčasťou väčšiny populárnych programovacích jazykov, ale PHP programátormi sú často prehliadané.
Jazyky, ako napríklad Ruby, sú extrémne založené na výnimkách a výnimka je vyvolaná v prípade akejkoľvek chyby. Je jedno
či sa jedná o chybnú HTTP žiadosť, chybný dopyt na databázu, či cestu k obrázku. Ruby (alebo použité závislosti) vypíše
takéto výnimky na obrazovku, takže hneď viete, že nastala chyba.

Samotné PHP je pomerne laxné, čo sa toho týka, a napríklad zavolanie funkcie `file_get_contents()` vráti obyčajne
len `FALSE` a upozornenie. Mnoho starších PHP framework-ov, ako napríklad CodeIgniter, len vráti `FALSE`, zapíše chybu
do svojho proprietárneho súboru s chybovými hláseniami a prípadne vám pre zistenie chyby umožní použiť metódu
ako napríklad `$this->upload->get_error()`. Problémom s takýmto hlásením je, že na miesto očividného chybového hlásenia
musíte chybu hľadať, prípadne pre zistenie chyby skontrolovať dokumentáciu použitej triedy.

Ďalším problémom je, ak triedy automaticky vypíšu na obrazovku chybu a ukončia proces. Tento spôsob znemožňuje ďalším
programátorom spracovať chybou dynamicky. Na miesto vypísania chyby na obrazovku a ukončenia procesu by chyba mala
vyvolať výnimku. Takáto chyba sa dá následne jednoducho spracovať. Vezmime si príklad:

{% highlight php %}
<?php
$email = new Fuel\Email;
$email->subject('My Subject');
$email->body('How the heck are you?');
$email->to('guy@example.com', 'Some Guy');

try {
    $email->send();
} catch(Fuel\Email\ValidationFailedException $e) {
    // v prípade chybnej validácie
} catch(Fuel\Email\SendingFailedException $e) {
    // v prípade neúspešného odoslanie správy
} finally {
    // vykonané bez ohľadu na to, či nastala výnimka, alebo nie
}
{% endhighlight %}

### SPL výnimky

Generická trieda `Exception` poskytuje pre vývojárov len veľmi málo kontextu pre zisťovanie chýb. Je však možné vytvoriť
novú triedu, ktorá bude pre triedu `Exception` rozšírením:

{% highlight php %}
<?php
class ValidationException extends Exception {}
{% endhighlight %}

S takýmto prístupom môžete použiť viacero catch blokov a spracovať výnimky rozlične podľa druhu chyby. Toto však môže
viesť k vytvoreniu <em>množstva</em> na mieru vytvorených výnimiek. Dá sa tomu však čiastočne zabrániť s použitím SPL
výnimiek, ktoré sú súčasťou [SPL rozšírenia][splext].

Ak napríklad použijete magickú metódu `__call()` a požadovaná metóda je neplatná, potom na miesto vyvolania štandardnej
výnimky, alebo vytvorenia novej výnimky špeciálne pre tento prípad, môžete použiť `throw new BadMethodCallException;`.

* [Prečítajte si o výnimkách][exceptions]
* [Prečítajte si o SPL výnimkách][splexe]
* [Vetvenie výnimiek v PHP][nesting-exceptions-in-php]
* [Osvedčené postupy pri nakladaní s výnimkami v PHP 5.3][exception-best-practices53]


[splext]: /#standard_php_library
[exceptions]: http://php.net/language.exceptions
[splexe]: http://php.net/spl.exceptions
[nesting-exceptions-in-php]: http://www.brandonsavage.net/exceptional-php-nesting-exceptions-in-php/
[exception-best-practices53]: http://ralphschindler.com/2010/09/15/exception-best-practices-in-php-5-3
