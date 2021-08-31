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
- *privatus raktas* (naudoja kliento sistema), siunčiamam **turiniui pasirašyti**;
- *viešas kliento raktas* (perduodamas kliento GoSign.lt administratoriui), **klientui ir** jo siunčiamam **turiniui patvirtinti**;
- *viešas GoSign.lt raktas* (perduodamas klientui GoSign.lt administratorių), užklausų **atsakymų vientisumui užtikrinti**.

Kaip ir kur generuoti minėtus raktus rasite mūsų pateiktoje [instrukcijoje](key-generation.md).
