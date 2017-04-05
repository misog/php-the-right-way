---
title: Registrácia globálnych premenných
isChild: true
anchor:  register_globals
---

## Registrácia globálnych premenných {#register_globals_title}

**POZNÁMKA:** Konfiguračné natavenie `register_globals` bolo v PHP verzií 5.4.0 odstránené a nemôže byť už naďalej
použité. Tento odsek je zahrnutý len ako upozornenie pre niekoho, kto je v štádiu aktualizácie zastaranej aplikácie.

Pokiaľ je povolené, konfiguračné nastavenie `register_globals` vytvára niekoľko druhov premenných, ktoré sú prístupné
v globálnom rozsahu vašej aplikácie. To zahŕňa aj premenné z `$_POST`, `$_GET` a `$_REQUEST`. Tieto vytvorené premenné
môžu veľmi jednoducho viesť k bezpečnostným problémom, nakoľko vaša aplikácia nedokáže efektívne určiť zdroj dát.

Príklad: S povoleným nastavením `register_globals` by bola hodnota `$_GET['foo']` dostupná cez premennú `$foo`.
Táto premenná môže prepísať hodnoty v premenných, ktoré neboli deklarované. Ak používate verzie PHP menšie ako 5.4.0,
**ubezpečte sa**, že hodnota nastavenia `register_globals` je **off**.

* [Registrácia globálnych premenných v PHP manuáli](http://php.net/security.globals)
