---
layout: default
title: Webservisas
nav_order: 3
has_children: true
---

## Webservisas

Srautinio pasirašymo paslaugos web servisai **naudoja `SOAP` protokolą**. Dokumento pasirašymas gali būti vykdomas dviem būdais:
- `OneSign`, kai norima vienu metu pasirašyti **tik vieną dokumentą stacionariu arba mobiliu parašu**;
- `MultiSign`, kai norima vienu metu pasirašyti **daugiau nei vieną dokumentą tik stacionariu parašu**.

Pasirašymo paslaugos **saugumas ir kliento autentifikavimas užtikrinamas naudojant asimetrinę kriptografiją**:
- *privatus raktas* (naudoja kliento sistema. Jei naudojate [PHP Client biblioteką](https://github.com/registrucentras/onesign), šis raktas yra perduodamas per [private rakto parametrą](https://github.com/registrucentras/onesign/blob/master/src/Client.php#L110)), siunčiamam **turiniui pasirašyti**;
- *viešas kliento raktas* (perduodamas kliento GoSign.lt administratoriui), **klientui ir** jo siunčiamam **turiniui patvirtinti**;
- *viešas GoSign.lt raktas*, užklausų **atsakymų vientisumui užtikrinti**.

JAVA aplinkoms skirti vieši GoSign.lt raktai:

- [test aplinkai](https://raw.githubusercontent.com/registrucentras/gosign-api-integration/master/docs/keys/java_test.key);
- [prod aplinkai](https://raw.githubusercontent.com/registrucentras/gosign-api-integration/master/docs/keys/java_prod.key);

PHP aplinkoms skirti vieši GoSign.lt raktai:

- [test aplinkai](https://raw.githubusercontent.com/registrucentras/gosign-api-integration/master/docs/keys/php_test.key);
- [prod aplinkai](https://raw.githubusercontent.com/registrucentras/gosign-api-integration/master/docs/keys/php_prod.key).

Šie raktai yra naudojami validuoti iš pasirašymo serverio gautą atsakymą. Jei naudojate [PHP Client biblioteką](https://github.com/registrucentras/onesign), šie raktai, priklausomai nuo aplinkos tipo, paduodami per [public rakto parametrą](https://github.com/registrucentras/onesign/blob/master/src/Client.php#L112).

Kaip ir kur generuoti savo raktų porą rasite mūsų pateiktoje [instrukcijoje](key-generation.md).
