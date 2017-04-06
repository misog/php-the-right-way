---
title: Filtrácia dát
isChild: true
anchor:  data_filtering
---

## Filtrácia dát {#data_filtering_title}

Cudzím dátam vstupujúcim do vášho PHP kódu netreba vo všeobecnosti nikdy veriť. Pred použitím cudzích vstupných dát
vo vašom kóde, dáta vždy sanitujte a validujte. Sanitáciu a validáciu textových formátov (napríklad emailov) môžete
vykonať pomocou funkcií `filter_var()` a `filter_input()`.

Cudzie vstupné dáta môžu byť akékoľvek dáta z `$_GET` a `$_POST`, niektoré hodnoty zo superglobálnej premennej
`$_SERVER` a dáta z tela HTTP požiadavky nadobudnuté pomocou `fopen('php://input', 'r')`. Majte však na pamäti, že
cudzie dáta nie sú limitované len na dáta odoslané užívateľmi z formulárov. Cudzie dáta sú aj nahrané a stiahnuté súbory,
hodnoty uložené v relácii, dáta zo súborov cookie, a taktiež dáta z webových služieb tretích strán.

Dáta sa pokladajú za cudzie akonáhle môžu byť uložené, kombinované s inými dátami a pristupované. Zakaždým si dobre
premyslite, či sú dáta, ktoré spracovávate, zobrazujete na výstupe, spájate s inými dátami, alebo inak využívate
vo vašom kóde správne filtrované a dôveryhodné.

Dáta sa môžu podľa účelu _filtrovať_ rozdielne. Nefiltrované cudzie dáta zobrazené na výstupe HTML stránky môžu
napríklad spustiť JavaScript, alebo pozmeniť HTML kód. Takýto druh útoku sa nazýva Cross-Site Scripting (XSS) a môže byť
veľmi nebezpečný. Spôsobom, ako predísť útokom typu XSS, je odstránenie HTML znakov z užívateľmi vygenerovaných vstupov
pred ich zobrazením. HTML znaky možno odstrániť pomocou funkcie `strip_tags()`. Ďalším spôsobom je zámena znakov
so špeciálnym významom za príslušné HTML entity pomocou funkcií `htmlentities()`, alebo `htmlspecialchars()`.

Ďalším príkladom je predávanie parametrov pre použitie v príkazovom riadku. Použitie takýchto parametrov obyčajne nie je
najlepší nápad, pretože takýto prístup môže byť extrémne nebezpečný. Pomocou vstavanej funkcie `escapeshellarg()`
je však tieto parametre možné sanitovať.

Posledným príkladom je použitie cudzieho vstupu na určenie súboru pre načítanie zo súborového systému. Takto prijatý
vstup môže byť zneužitý tak, že útočník zmení názov súboru za cestu. S upraveným vstupom je následne možné načítať
skryté, neverejné, alebo senzitívne súbory. Z cesty k súboru je preto potrebné odstrániť `"/"`, `"../"`,
[null bajty][6], alebo iné znaky. Mnoho z týchto bezpečnostných chýb je už v novších verziách PHP opravených.

* [Naučte sa viac o filtrácii dát][1]
* [Naučte sa o funkcii `filter_var`][4]
* [Naučte sa o funkcii `filter_input`][5]
* [Naučte sa ako narábať s null bajty][6]

### Sanitácia

Sanitácia odstraňuje nepovolené, alebo nebezpečné znaky z cudzieho vstupu.

Príkladom je sanitácia cudzích vstupov pred ich vypísaním pomocou HTML, alebo uloženie vstupu pomocou "surového" SQL
dopytu. Vstup ukladaný v databáze je napríklad možné sanitovať s použitím parametrizovaného dopytu pomocou
[PDO](#databases), čím je možné predchádzať útokom typu SQL injection.

Niekedy je žiadané, aby mal vstup povolené niektoré bezpečné HTML znaky. Jedná sa napríklad o formátovaný vstup,
ktorý má byť formátovaný aj na výstupe HTML. Sanitácia takéhoto vstupu je však zložitá a mnoho projektov preto používa
obmedzené formátovanie napríklad s použitím Markdown, alebo BBCode. Je však možné použiť knižnicu, ako napríklad
[HTML Purifier][html-purifier], ktorá poskytuje zoznam povolených znakov, odstraňuje nebezpečný kód (XSS prevencia)
a slúžia presne na tento účel.

[Pozrite si sanitačné filtre][2]

### Deserializácia

Užívateľské dáta, alebo dáta z iných nedôveryhodných zdrojov je pomocou funkcie `unserialize()` nebezpečné
deserializovať. Podpora takéhoto druhu vstupu umožňuje zlomyseľným užívateľom vytvoriť inštanciu objektu 
(s užívateľsky definovanými parametrami), ktorého deštruktor bude spustený, **i keď samotný objekt použitý nebude**.
Mali by ste sa preto, pokiaľ možno, vyhnúť použitia deserializovaných dát z nedôveryhodných zdrojov.

Ak sa deserializácii neviete vyhnúť a dáta z nedôveryhodných zdrojov musíte použiť, potom pre obmedzenie typu objektov,
ktoré môže funkcia deserializovať, použite nastavenie [`allowed_classes`][unserialize] (>= PHP 7).

### Validácia

Validácia zabezpečuje, aby cudzie vstupy mali taký formát, aký aplikácia požaduje. Príkladom môže byť validácia
emailovej adresy, telefónneho čísla, alebo veku pri spracovaní registračného formulára.

[Pozrite si validačné filtre][3]


[1]: http://php.net/book.filter
[2]: http://php.net/filter.filters.sanitize
[3]: http://php.net/filter.filters.validate
[4]: http://php.net/function.filter-var
[5]: http://php.net/function.filter-input
[6]: http://php.net/security.filesystem.nullbytes
[html-purifier]: http://htmlpurifier.org/
[unserialize]: https://secure.php.net/manual/en/function.unserialize.php
