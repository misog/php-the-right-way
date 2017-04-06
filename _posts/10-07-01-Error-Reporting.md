---
title: Hlásenie chýb
isChild: true
anchor: error_reporting
---

## Hlásenie chýb {#error_reporting}

Záznam chýb môže byť veľmi nápomocný pri hľadaní problémových bodov vo vašej aplikácií, ale môže taktiež odhaliť
informácie o štruktúre vašej aplikácie vonkajšiemu svetu. Efektívne zabezpečenie vašej aplikácie voči chybám,
ktoré môžu byť spôsobené zobrazením takýchto chybových hlášok, vyžaduje rozdielnu konfiguráciu pre vývojové
prostredie a pre produkčné prostredie.

### Vývojové prostredie

Pre zobrazenie všetkých možných chýb počas **vývoja** použite v `php.ini` nasledujúce nastavenie: 

{% highlight ini %}
display_errors = On
display_startup_errors = On
error_reporting = -1
log_errors = On
{% endhighlight %}

> Použitie hodnoty `-1` zabezpečí zobrazenie všetkých možných chýb, i v prípade pridania nových konštánt pre úrovne
> hlásenia chýb v budúcich verziách PHP. Od PHP 5.4 má použitie konštanty `E_ALL` rovnaké správanie -
> [php.net](http://php.net/function.error-reporting)

V PHP verzií 5.3.0 bola zavedená konštanta pre úroveň hlásenia chýb `E_STRICT`, ktorá nebola súčasťou `E_ALL`. Vo verzií
5.4.0 sa však stala jej súčasťou. Čo to v praxi znamená? Pre zobrazenie všetkých možných chýb je vo verzií 5.3
potrebné použiť hodnotu `-1`, alebo `E_ALL | E_STRICT`.

**Hlásenie všetkých možných v rôznych verziách PHP**

* &lt; 5.3 `-1`, alebo `E_ALL`
* &nbsp; 5.3 `-1`, alebo `E_ALL | E_STRICT`
* &gt; 5.3 `-1`, alebo `E_ALL`

### Produkčné prostredie

Pre ukrytie chýb vo vašom **produkčnom** prostredí použite v `php.ini` nasledujúce nastavenie:

{% highlight ini %}
display_errors = Off
display_startup_errors = Off
error_reporting = E_ALL
log_errors = On
{% endhighlight %}

S použitím týchto nastavení vo vašom produkčnom prostredí budú chyby naďalej zaznamenávané v súbore s chybovými
hláseniami vášho webového servera, ale nebudú zobrazené užívateľovi. Viac informácií o nastaveniach nájdete v PHP manuáli: 

* [error_reporting](http://php.net/errorfunc.configuration#ini.error-reporting)
* [display_errors](http://php.net/errorfunc.configuration#ini.display-errors)
* [display_startup_errors](http://php.net/errorfunc.configuration#ini.display-startup-errors)
* [log_errors](http://php.net/errorfunc.configuration#ini.log-errors)
