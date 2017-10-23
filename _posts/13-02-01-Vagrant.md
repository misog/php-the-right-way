---
isChild: true
anchor: vagrant
---

## Vagrant {#vagrant_title}

[Vagrant] vám pomôže vytvoriť virtuálne boxy na vrchole známych virtualizačných prostredí, a tieto
prostredia nakonfiguruje pomocou jediného konfiguračného súboru. Tieto boxy je možné nastaviť manuálne, 
alebo proces konfigurácie zautomatizovať za pomoci zaobstarávacieho (provisioning) softvéru, ako je 
[Puppet] alebo [Chef]. Zautomatizovaný proces konfigurácie základného virtuálneho boxu je výborným 
spôsobom, ako zabezpečiť identické nastavenia pre viaceré boxy. Tým pádom odpadá potreba použitia
komplikovaných „nastavovacích“ príkazov. Takto vytvorený box môžete jednoducho „zničiť“ a znovu vytvoriť
bez zbytočných manuálnych krokov, čo umožňuje ľahko vytvoriť „čerstvú“ inštaláciu. 

Vagrant vytvára priečinky na zdieľanie kódu medzi hostiteľom a virtuálnym strojom. To znamená, že súbory môžete
vytvárať a upravovať vo vašom hostiteľskom počítači a kód následne spúšťať vo vnútri virtuálneho stroja.

### Malá pomoc

Ak na začiatok potrebujete trochu pomôcť s použitím Vagrant-u, existujú niektoré služby, ktoré by vám mohli byť užitočné:

- [Rove][Rove]: služba, ktorá umožňuje generovať Vagrant konfiguráciu so zaužívanými nastaveniami.
Automatická konfigurácia je zabezpečená pomocou softvéru Chef.
- [Puphpet][Puphpet]: jednoduché grafické rozhranie na nastavenie virtuálnych strojov pre vývoj v PHP.
**Je silne zamerané na PHP**. Okrem lokálnych virtuálnych strojov je možné generovať nastavenie aj
pre použitie v cloud-e. Konfigurácia je zabezpečená pomocou softvéru Puppet.
- [Protobox][Protobox]: je vrstva na vrchole Vagrant-u a webové grafické rozhranie na nastavenie virtuálnych strojov
pre vývoj webových aplikácií. Jediný súbor YAML kontroluje všetko, čo je na virtuálnom počítači nainštalované.
- [Phansible][Phansible]: poskytuje ľahko použiteľné rozhranie, ktoré vám pomôže vygenerovať Ansible scenáre (playbooks)
pre projekty založené na PHP.


[Vagrant]: http://vagrantup.com/
[Puppet]: http://www.puppetlabs.com/
[Chef]: https://www.chef.io/
[Rove]: http://rove.io/
[Puphpet]: https://puphpet.com/
[Protobox]: http://getprotobox.com/
[Phansible]: http://phansible.com/
