---
title: Konfiguračné súbory
isChild: true
anchor: configuration_files
---

## Konfiguračné súbory {#configuration_files_title}

Ak pre vašu aplikáciu vytvárate konfiguračné súbory, snažte sa dodržiavať nasledujúce doporučené postupy:

* Konfiguračné súbory je doporučené ukladať na miestach, ktoré nie sú priamo prístupné a nahrané cez súborový systém. 
* Ak musí byť konfiguračný súbor uložený v koreňovom adresári, potom pre súbor použite príponu `.php`. Toto zabezpečí,
že výstupom nebude plain text, ani v prípade, keď bude skript pristupovaný priamo.
* Informácie v konfiguračných súboroch by mali byť taktiež zabezpečené. Buď šifrovaním, alebo súborovými systémovými
oprávneniami pre skupinu, alebo užívateľa.
* Taktiež sa ubezpečte, že senzitívne informácie, ako napríklad heslá, alebo API tokeny, nie sú uložené vo vašom 
 verziovaciom systéme.
