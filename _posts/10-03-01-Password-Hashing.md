---
title: Hašovanie hesiel
isChild: true
anchor: password_hashing
---

## Hašovanie hesiel {#password_hashing_title}

V konečnom dôsledku všetci vytvárajú PHP aplikácie, ktoré sú založené na prihlasovaní užívateľov. Užívateľské mená
a heslá sú uložené v databáze a následne používané na overovanie užívateľov počas prihlasovania.

Riadne [_hašovanie_][3] hesla je veľmi dôležité ešte predtým, ako je heslo uložené. Hašovanie hesla je jednosmerná
nezvratná operácia vykonávaná na hesle užívateľa. Táto operácia vytvorí reťazec s fixnou dĺžkou, ktorá sa nedá reálne
zvrátiť. To znamená, že haš môže byť porovnávaný s ďalším hašom pre určenie rovnakého zdrojového reťazca, ale na
základe hašu nie je možné určiť originálny reťazec. Ak heslá nie sú hašované, potom neoprávnený prístup k vašej databáze
z tretích strán spôsobí kompromitáciu všetkých užívateľských účtov. Niektorí užívatelia žiaľ používajú rovnaké heslo
pre ďalšie servisy, a preto je dôležité brať bezpečnosť vážne.

**Hašovanie hesiel pomocou `password_hash`**

Vo verzií PHP 5.5 bola zavedená funkcia `password_hash()`, ktorá momentálne používa BCrypt, čo je v súčastnosti
najsilnejší algoritmus podporovaný v PHP. V budúcnosti bude podľa potreby zavedená podpora pre ďalšie algoritmy.
Pre spätnú kompatibilitu vo verziách PHP >= 5.3.7 bola vytvorená knižnica `password_compat`.

V príklade overenie hesla nižšie, je reťazec hašovaný a následne porovnávaný s novým reťazcom. Pretože
oba pôvodné reťazce sú rozdielne ('tajne-heslo' a 'nespravne-heslo'), prihlásenie v tomto prípade nebude úspešné.

{% highlight php %}
<?php
require 'password.php';

$passwordHash = password_hash('tajne-heslo', PASSWORD_DEFAULT);

if (password_verify('nespravne-heslo', $passwordHash)) {
    // Správne heslo
} else {
    // Nesprávne heslo
}
{% endhighlight %}  


* [Naučte sa o funkcii `password_hash()`] [1]
* [Knižnica `password_compat` pre PHP >= 5.3.7 && < 5.5] [2]
* [Naučte sa o hašovaní v súvislosti s kryptografiu] [3]
* [PHP `password_hash()` RFC] [4]


[1]: http://php.net/function.password-hash
[2]: https://github.com/ircmaxell/password_compat
[3]: http://en.wikipedia.org/wiki/Cryptographic_hash_function
[4]: https://wiki.php.net/rfc/password_hash
