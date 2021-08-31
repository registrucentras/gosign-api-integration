---
layout: default
title: Position elementas
parent: Webservisas
nav_order: 4
---

# Position elementas

Elemento `position` reikšmė **nurodo parašo vizualizacijos vietą** pasirašytame dokumente. Galimos šio elemento reikšmės yra tokios:

| Reikšmė  | Aprašymas |
| ------------- | ------------- |
| `hidden`  | Elektroninis parašas nėra vizualizuojamas |
| `relative`, `page`, `relativeX`, `relativeY`, `width`, `height` | Elektroninio parašo vizualizacijos vieta yra reliatyvi puslapio matomos dalies atžvilgiu. Parametras `page` nurodo puslapio numerį, kuriame rodomas parašas. Puslapiai numeruojami nuo 1. Neigiama parametro reikšmė nurodo puslapio numerį skaičiuojant nuo dokumento galo (pvz. reikšmė -2 reiškia priešpaskutinį dokumento puslapį). Parametrai `relativeX` ir `relativeY` nurodo reliatyvią horizontalią ir vertikalią parašo vietą puslapyje. Galimas šių parametrų reikšmių intervalas yra [0, 1]. Nulinė reikšmė reiškia, kad parašas bus dedamas puslapio kairėje arba viršuje, vienetas – puslapio dešinėje arba apačioje. Parametrai `width` ir `height` nurodo parašo vizualizacijos plotį ir aukštį. Jei nurodyta tik reikšmė, laikoma, kad atstumas nurodytas matavimo vienetais point (1pt = 0.351mm). Jei po reikšmės seka santrumpos `mm` arba `cm`, laikoma, kad atstumas nurodytas atitinkamai milimetrais arba centimetrais (pvz. „10.3cm") |
| `absolute`, `page`, `x`, `y`, `width`, `height`  | Elektroninio parašo vizualizacijos vieta yra absoliuti puslapio atžvilgiu. Parametras `page` nurodo puslapio numerį, kuriame rodomas parašas. Puslapiai numeruojami nuo 1. Neigiama parametro reikšmė nurodo puslapio numerį skaičiuojant nuo dokumento galo (pvz. Reikšmė -2 reiškia priešpaskutinį dokumento puslapį). Parametrai `x` ir `y` nurodo absoliučią horizontalią ir vertikalią parašo kairio viršutinio kampo padėtį puslapyje. Teigiama reikšmė reiškia, kad atstumas matuojamas nuo kairio puslapio krašto arba nuo puslapio viršaus. Neigiama reikšmė reiškia, kad atstumas matuojamas nuo dešinio puslapio krašto arba nuo puslapio apačios. Parametrai `width` ir `height` nurodo parašo vizualizacijos plotį ir aukštį. Jei parametruose `x`, `y`, `width` ir `height` nurodyta tik reikšmė, laikoma, kad atstumas nurodytas matavimo vienetais `point` ( 1pt = 0.351mm). Jei po reikšmės seka santrumpos `mm` arba `cm`, laikoma, kad atstumas nurodytas atitinkamai milimetrais arba centimetrais |
| `named`, `fieldName`  | Elektroninis parašas yra dedamas į iš anksto sukurtą tuščią (nepasirašytą) elektroninio parašo lauką. Parametras `fieldName` nurodo dokumente egzistuojančio parašo lauko vardą |

**Pavyždžiai**

Parašas nerodomas:

```xml
<position>hidden</position>
```

Parašas dedamas antrame dokumento puslapyje, lapo dešiniame viršutiniame kampe. Parašas yra 8cm pločio ir 4cm aukščio:

```xml
<position>relative, 2, 1, 0, 8cm, 4cm</position>
```

Parašas dedamas paskutiniame dokumento puslapyje, lapo centre. Parašas yra 65mm pločio ir 45mm aukščio:

```xml
<position>relative, -1, 0.5, 0.5, 65mm, 45mm</position>
```

Parašas dedamas penktame dokumento puslapyje 2cm nuo lapo kairės pusės ir 2cm nuo lapo apačios (parašo viršus yra 6cm nuo lapo apačios). Parašas yra 8cm pločio ir 4cm aukščio:

```xml
<position>absolute, 5, 2cm, -6cm, 8cm, 4cm</position>
```

Parašas dedamas į dokumente jau esantį tuščią parašo lauką, kurio pavadinimas yra `signature1`:

```xml
<position>named, siganture1</position>
```
