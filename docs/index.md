---
layout: default
title: Pagrindinis
nav_order: 1
description: "GoSign.lt paslaugų integracija"
---

## Įžanga

Šio dokumento tikslas yra **aprašyti sąsają tarp pasirašymo paslaugos** (GoSign.lt API) ir šią paslaugą naudojančių trečiųjų šalių informacinių sistemų.

Elektroninių dokumentų pasirašymas vyksta tokiais etapais:
- kliento IS **serveris iškviečia pasirašymo paslaugos** serverio web servisą perduodamas pasirašymo parametrus ir pasirašymui skirtus PDF failus. Web servisas klientui grąžina unikalią pasirašymo puslapio nuorodą (URL);
- naudotojo **naršyklėje atidaromas** pasirašymo paslaugos **web puslapis** (galima pasirinkti iš [kelių variantų](sign-templates.md)).
- pasirašymo paslauga **atvaizduoja** naudotojui pasirašymui skirtų dokumentų **sąrašą**. Naudotojas gali peržiūrėti pasirašomų dokumentų turinį;
- naudotojas pasirenka pasirašymo infrastruktūrą (būdą) ir **inicijuoja visų dokumentų pasirašymą**;
- pasirašant stacionaria infrastruktūra PIN kodas suvedamas vieną kartą (jei stacionarios pasirašymo infrastruktūros draiveris tai leidžia);
- naudotojas pasirašymo procesą **gali** bet kada **nutraukti**;
- pasirašius visus dokumentus, naudotojas **nukreipiamas į kliento IS puslapį**;
- kliento IS **serveris iškviečia** pasirašymo paslaugos serverio **web servisą ir gauna pasirašymo statusą**, informaciją apie pasirašiusį asmenį ir pasirašytų dokumentų sąrašą.

Pasirašymo **paslaugos saugumas užtikrinamas naudojant asimetrinę kriptografiją**: pasirašymo parametrai yra patvirtinami kliento informacinės sistemos.

## Pagalba

* [Klaidų ir klausimų registravimas](https://github.com/registrucentras/gosign-api-integration/issues)
