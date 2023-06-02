---
layout: default
title: OneSign webservisas
parent: Webservisas
nav_order: 1
---

# OneSign webservisas

***OneSign** - webservisas **leidžiantis** vienu metu **pasirašyti tik vieną dokumentą**. Yra galimybė pasirašyti **tiek mobiliu**, **tiek stacionariu** parašu*.

## Metodai

- *InitSigning*, naudojamas **inicijuoti** dokumento **pasirašymą**;
- *SigningResult*, naudojamas **pasirašymo būsenai sužinoti** (jei pasirašymo transakcija yra pasibaigusi sėkmingai, gražinamas pasirašytas dokumentas ir informaciją apie pasirašiusį asmenį);
- *SigningCancel*, naudojamas pasirašymo **transakcijai nutraukti**;
- *Seal*, naudojamas RC spaudui dėti ant PDF dokumentų (tik Registrų Centro sistemoms);
- *Timestamp*, naudojamas **dėti laiko žymoms** and dokumentų.

### InitSigning metodas

`InitSigning` metodo struktūrinis duomenų tipas sudarytas iš elementų:

| Elementas                  | Tipas | Aprašymas                                                                                                                                                            |
|----------------------------| ------------- |----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| clientInfo                 | [SignRequestWebClientInfo[1]](#signrequestwebclientinfo-struktūrinis-tipas)  | Informacija apie klientą                                                                                                                                             |
| signatureMetadata          | [StandardSignatureMetadata[1]](#standardsignaturemetadata-struktūrinis-tipas) | Parašo metaduomenys                                                                                                                                                  |
| signatureDisplayProperties | [SignatureDisplayProperties[1]](#signaturedisplayproperties-struktūrinis-tipas) | Parašo atvaizdavimo nustatymai                                                                                                                                       |
| mobileSigningText          | [string[0..1]](https://www.w3.org/TR/xmlschema-2/#string) | Mobiliajame įrenginyje pasirašančiajam rodomas tekstas patvirtinimo lange |                                                                                           |                                                                                                                                                                    |
| signingType                | [signingType[1]](#signingtype-tipas) | Reikšmė nurodanti ar elektroniniame paraše turi būti uždėta laiko žyma ar žyma su patikra ([OCSP](https://en.wikipedia.org/wiki/Online_Certificate_Status_Protocol)) |
| file                       | [SourceFileBinary[1..N]](#sourcefilebinary-struktūrinis-tipas)  | Failas pasirašymui. Pateikiamas failo turinys (tik PDF arba ADOC failai)                                                                                             |
| [signature](signature.md)  | [base64Binary[1]](https://www.w3.org/TR/xmlschema-2/#base64Binary)  | Kliento sistemos sugeneruotas parašas patvirtinantis užklausos duomenų teisingumą                                                                                    |

## InitSigning metode naudojami kiti struktūriniai tipai

### SignRequestWebClientInfo struktūrinis tipas

Struktūrinis duomenų tipas `SignRequestWebClientInfo` aprašo `SignRequestClientInfo` praplėtimą. Rezultate užklausa sudaryta iš:

| Elementas  | Tipas                                                     | Aprašymas |
| ------------- |-----------------------------------------------------------| ------------- |
| clientId  | [string[1]](https://www.w3.org/TR/xmlschema-2/#string)    | Unikalus pasirašymo paslaugos administratorių suteiktas kliento informacinės sistemos identifikatorius |
| signerPersonalCode  | [string[0..1]](https://www.w3.org/TR/xmlschema-2/#string) | Asmens, kuris turi pasirašyti dokumentą, asmens kodas. Jei šis parametras nenurodytas, dokumentą gali pasirašyti bet kuris asmuo |
| locale  | [string[0..1]](https://www.w3.org/TR/xmlschema-2/#string) | Pasirašymo paslaugos vartotojo sąsajos kalba ("lt" – Lietuvių kalba, "en" – Anglų kalba) |
| responseUrl  | [string[1]](https://www.w3.org/TR/xmlschema-2/#string)    | Web puslapio, kuris pasibaigus dokumento pasirašymui turi būti užkrautas naudotojo naršyklėje, adresas |
| remoteAddress  | [string[0..1]](https://www.w3.org/TR/xmlschema-2/#string) | Pasirašymo paslaugos vartotojo prisijungimo IP adresas. Naudojamas statistiniams duomenims rinkti |
| acceptableInfrastructure  | [infrastructureType[0..3]](#infrastructuretype-tipas)     | Sąrašas, nurodantis kokio tipo pasirašymo infrastruktūra gali būti naudojama pasirašymui. Galimos elemento reikšmės aprašytos skyriuje. Kiekvienai leistinai infrastruktūrai nurodomas atskiras acceptableInfrastructure elementas. Jei nėra nurodytas nei vienas acceptableInfrastructure elementas, leidžiama pasirašyti naudojant bet kurią infrastruktūrą |

### StandardSignatureMetadata struktūrinis tipas

Struktūrinis duomenų tipas `StandardSignatureMetadata` aprašo parašo meta-duomenys standartiniam pasirašymui. Galima perduoti šiuos elementus:

| Elementas  | Tipas | Aprašymas |
| ------------- | ------------- | ------------- |
| reason  | [string[0..1]](https://www.w3.org/TR/xmlschema-2/#string) | Parašo tekstas |
| location  | [string[0..1]](https://www.w3.org/TR/xmlschema-2/#string) | Parašo sudarymo vieta |
| contact  | [string[0..1]](https://www.w3.org/TR/xmlschema-2/#string) | Pasirašiusio asmens kontaktinė informacija |

### SignatureDisplayProperties struktūrinis tipas

Struktūrinis duomenų tipas `SignatureDisplayProperties` aprašo parašo atvaizdavimo nustatymus. Atvaizdavimą galima valdyti naudojant užklausos elementus:

| Elementas  | Tipas | Aprašymas |
| ------------- | ------------- | ------------- |
| [position](position.md)  | [string[1]](https://www.w3.org/TR/xmlschema-2/#string) | Parašo vizualizacijos vieta dokumente (todo) |
| displayValidity  | [boolean[0..1]](https://www.w3.org/TR/xmlschema-2/#boolean) | Reikšmė nurodanti ar parašo vizualizavime turi būti dinamiškai rodomas parašo teisingumas. Pasirašant 1.6 ir vėlesnių versijų PDF dokumentus Adobe rekomenduoja nenaudoti dinamiškai rodomo parašo teisingumo |
| signatureImageUrl  | [string[0..1]](https://www.w3.org/TR/xmlschema-2/#string) | Parašo vizualizacijoje naudojamo paveiksliuko nuoroda (URL). Gali būti nurodoma viešai prieinamo JPEG, PNG arba GIF tipo paveiksliuko pilna nuoroda. Jei parašo paveiksliukas yra nurodytas, parašo vizualizacijai skiriama vieta yra dalinama perpus, kairėje pusėje rodomas paveiksliukas, o dešinėje – parašo informacija. Geriausi rezultatai gaunami, kai parašo paveiksliukas yra pusiau skaidrus (t.y. per paveiksliuko neužimtas vietas persišviečia dokumento fonas) |
| backgroundImageUrl  | [string[0..1]](https://www.w3.org/TR/xmlschema-2/#string) | Parašo vizualizacijoje naudojamo fono paveiksliuko („vandens ženklų“) nuoroda (URL). Turi būti nurodoma viešai prieinamo JPEG, PNG arba GIF tipo paveiksliuko pilna nuoroda. Geriausi rezultatai gaunami, kai parašo paveiksliukas yra pusiau skaidrus (t.y. per fono paveiksliuką persišviečia dokumento fonas) |

### signingType tipas

Paprastas duomenų tipas `signingType` nurodo pasirašymo tipą:

| Reikšmė  | Aprašymas |
| ------------- | ------------- |
| Signature  | Uždedamas parašas |
| SignatureWithTimestamp | Uždedamas parašas su laiko žyma |
| SignatureWithTimestampOCSP | Uždedamas parašas su laiko žyma ir [OCSP](https://en.wikipedia.org/wiki/Online_Certificate_Status_Protocol) |

### infrastructureType tipas

Paprastas duomenų tipas `infrastructureType` nurodo galimus pasirašymo infrastruktūros tipus:

| Reikšmė    | Aprašymas                                                    |
|------------|--------------------------------------------------------------|
| Stationary | Leidžiama pasirašyti naudojant stacionarų elektroninį parašą |
| Mobile     | Leidžiama pasirašyti naudojant mobilų elektroninį parašą     |
| RasPerson  | Leidžiama pasirašyti naudojant LT ID elektroninį parašą      |
| RasCompany | Leidžiama pasirašyti naudojant LT ID elektroninį spaudą      |

### SourceFileBinary struktūrinis tipas

Struktūrinis duomenų tipas `SourceFile` aprašo pagrindinius pasirašomo failo atributus ir naudojamas praplėtimuose. Šį struktūrinį duomenų tipą sudaro tokių elementų seka:

| Elementas  | Tipas | Aprašymas |
| ------------- | ------------- | ------------- |
| fileId | [string[1]](https://www.w3.org/TR/xmlschema-2/#string) | Pasirašymui skirto dokumento identifikatorius Kliento sistemoje. Šis elementas yra privalomas. Gali būti generuojamas, kaip pvz. documentURL dalis. Maksimalus ilgis: 128 |
| [fileDigest](filedigest.md) | [base64Binary[1]](https://www.w3.org/TR/xmlschema-2/#base64Binary) | Pasirašymui skirto dokumento SHA-1 santrauka |
| fileName | [string[0..1]](https://www.w3.org/TR/xmlschema-2/#string) | Pasirašymui skirto dokumento failo vardas. Failo vardas turi baigtis prievardžiu „.pdf“. Šis elementas gali būti praleistas. Tokiu atveju Pasirašymo paslaugos vartotojo sąsajoje pasirašomo failo vardas nebus rodomas |
| content | [base64Binary[1]](https://www.w3.org/TR/xmlschema-2/#base64Binary) | Pasirašymui skirto dokumento turinys |

### Užklausos pavyzdys

```xml
<SOAP-ENV:Envelope
	xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
	xmlns:ns1="http://www.registrucentras.lt/onesignservice">
	<SOAP-ENV:Body>
		<ns1:InitOneSignRequest>
			<ns1:clientInfo>
				<clientId>client_id</clientId>
				<signerPersonalCode>client_code</signerPersonalCode>
				<locale>lt</locale>
				<responseUrl>http://example.com/app</responseUrl>
				<acceptableInfrastructure>Mobile</acceptableInfrastructure>
			</ns1:clientInfo>
			<ns1:signingType>Signature</ns1:signingType>
			<ns1:file>
				<fileDigest>v9AJ9QDAVxlf/eZvrmT5L6X1m3I=</fileDigest>
				<fileName>sample.pdf</fileName>
				<content>TjRMenUuUyhbOC8pQ1RFLV4mNDtndms1XyEwYkhgOW5AcSY8QEo4Ylp9WGc9dENLOGhtb1BxYmhiM1dVeTRjXzZ7VDosJEtpKVhjfE1EOyo8eWwpSmQ4bnZHTTIiKHpAamNDdWNzYHU8LkJcMSBZcGhNLlA7SmtONHNwNmNvMCfR08lbEdPXl4xOFhbdU09WV9vSz5+RVRObXMlR3hZSCZXeClDciZNRDAgPnV2NDJPPXZGTF11LWJUcjkkdG54UGZHUWl+fCsgX0pral4uL18lVTBsVVA=PEBXMCUpJE1pTl56UCVpTi1KWiUrXD00eUpXY3NdW1hfIk4hcn1ZbC88KzxERUgSGJ3TSk6dSkhYyRyMT8zajY1TCE3RkxQcUwhRW5IVCBxSmNKdyUydkpxLzxzZFhsdCEmYjc1L3YjR0RiNXFPakJpNEYsTSJoSGp+Z0dmN1clK1omYGdBeTt3XnY9djF8L15JWiE+KCozZVR2ZFRlalEiPUFHO0puMiJRZWs9SztscHhkUHo+WUxbTTo5e3ovNmgheSQ+bjB7Y3g0OmFtInZeSCE3Tmg6TldtT3wkWCBfRm5CNFVEYnlmbjVUODVkZSh+PD9kJFpcYSYuSztreCo5IDlXciEwVk1aRTw+JFRWSkh9RTxAayk/fUR0ZDwmWlBdfV9WJGcjSHdJO2h4fG4lYm9BREBZdj=fmxFdzRkIjE/ImxSMFpHPTBqI2lnMXdvZn16I0ZNOHdDdFdUWVw2e3NsKDhRQy5gJihoaHVuVzopOkRddXV2MSVzK3E/QmkhenV2XE56dVk8SF1LLF9efF93YH51bFJ5LSN2dVpxQ2pLXWJ+cGl6fn1zdT5PRnZHKSJLVW8qMH5KXlMnMEoLnklK1ZPYDIzT2llKioncnF+NSRuIGQgbS9uZyZzVE5jXHU5JVd7PkBFXiRWdmhxfl12UFM=fjYrJElZdikwRyF1S1RHOkUjZCwqT1xBdi9NVkB+NUR3eDAxO3xkJ0ZTSTA=</content>
			</ns1:file>
			<ns1:signature>PDOb98H0XaDAR4BZ6kFWuS6gHDYAiLs2JI7SCgMau8JcOxqP83vw/6yL3YSVBeQGWzD6VGm7SzreNdhB4mqfFGkX9GWEB9afBohEIs5txBGLHxiFvRlaDOteFAwDEVcPYw3Ne5opok36pVjfu55Ec3PmXigS1Q+NePus+B1FXsw=</ns1:signature>
		</ns1:InitOneSignRequest>
	</SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

### Atsakymo pavyzdys

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
   <SOAP-ENV:Header/>
   <SOAP-ENV:Body>
      <ns3:InitOneSignResponse xmlns:ns3="http://www.registrucentras.lt/onesignservice">
         <transactionId>445190</transactionId>
         <signingUrl>https://example.com/unisign/ui/445190/282702111d7cced39fd1cbb699d6c1438646a109/main?locale=lt</signingUrl>
         <signature>IdUcXIAb4lafwo2GQWMX+t+tYQ5IeqgXX5pCzo+Lml4cDj9GA3WJp0V1Y1TfMF8CyjgWMhkkdfqBD76xP7lW1jWGD4SnokeZv75Y5BPZcE73qLJ56ynXSq+6fO9NZLpP4TvwZTn9FxyKnvZPLvUj5ZyH3sk0nEmz58w1R3FK/q/SHxC1m4p6nrZd8zLEdw9IJYvowcZicVTmqTJLjqp1CyPU00D5kA4xl1oAWfoO7yb3kmCYshTQnJqSjllFql5FZB4Jh1u61UFMJ+3QuwSRrz/ad5Jq9bVp9TfxeHx7N/p/V0yiFMxNjx0WJaoB52CdeOGxDj+44fs2nKiw4u9xaw==</signature>
      </ns3:InitOneSignResponse>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## SigningResult metodas

Metodas naudojamas pasirašymo būsenai sužinoti (jei pasirašymo transakcija yra pasibaigusi sėkmingai, gražinamas pasirašytų dokumentų sąrašas ir informaciją apie pasirašiusį asmenį). `SigningResult` tipas sudarytas iš:

| Elementas  | Tipas | Aprašymas |
| ------------- | ------------- | ------------- |
| clientId | [string[1]](https://www.w3.org/TR/xmlschema-2/#string) | Unikalus pasirašymo paslaugos administratorių suteiktas kliento informacinės sistemos identifikatorius |
| transactionId | [long[1]](https://www.w3.org/TR/xmlschema-2/#long) | Unikalus pasirašymo transakcijos identifikatorius |
| [signature](signature.md) | [base64Binary[1]](https://www.w3.org/TR/xmlschema-2/#base64Binary)  | Kliento sistemos sugeneruotas parašas patvirtinantis užklausos duomenų teisingumą |

### Užklausos pavyzdys

```xml
<SOAP-ENV:Envelope
	xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
	xmlns:ns1="http://www.registrucentras.lt/onesignservice">
	<SOAP-ENV:Body>
		<ns1:SigningResultRequest>
			<clientId>client_id</clientId>
			<transactionId>445191</transactionId>
			<signature>PMuOewIFfS+uualQuTO2uAAbl/OFv219Xp6jtGC13eTbocAoVIJJeu/xmngJpt5rgcjldN0/mGuFY6rh9eDTBDRa8HDXK43VQYRBheHt/QQEJh3DvDmcblrUP30aV8nq0lowYR5xhmxIZDkFwTTaUn9fV476gaG63qBXhCJdx4k=</signature>
		</ns1:SigningResultRequest>
	</SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

### Atsakymų pavyzdžiai

*Laukiama pasirašymo*

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
   <SOAP-ENV:Header/>
   <SOAP-ENV:Body>
      <ns3:SigningResultResponse xmlns:ns3="http://www.registrucentras.lt/onesignservice">
         <status>InProgress</status>
         <file/>
         <signature>lDzM9em93bknUW/TdTGtqd97JyCFEdOBbuLUzWFxLNCRJAnoe/bF/zkj9jdByWl6CWwOj6ECqy3Sb6mZ9JoPWDvHdWnKYxd/QerqZMWA+IOuWTWbmAZxTyncHvVlP6yxZkCSVYQmkuywKPJG8Ra86W9h3n0HiXmgIo6Gf+rtty/AVA+zefhhuhoHwn6B8uXJ9mgNLuD6mtKZq+Iw5pStUFNTjRGID5HEtEQ9SmUgcKgjHCon1HcsKRxGulMWOCo3jqNejJt+08TVTTKa9DQmKY35nUFC5cehQwjX2E2XJtDHPxXoKhFJJvw4g27gZjUb1j/mZzkK0R3RE9WYp11sqQ==</signature>
      </ns3:SigningResultResponse>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

*Po pasirašymo*

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
   <SOAP-ENV:Header/>
   <SOAP-ENV:Body>
      <ns3:SigningResultResponse xmlns:ns3="http://www.registrucentras.lt/onesignservice">
         <status>Signed</status>
         <signerCertificate>PDV1WF47KldsSyYgbmByZ2dHfWNLOXEmP3gwcCV7OHNndlZffCZsbmlyNmZVI0R8T083NERSUmpsWHZ7Y3JUdzBPWVo+czMwYVxPTlZBaG1UQD5ic1tAWFBqbFNIKlttK0lbKy1abE8zJn5PTE0iVyg8cG4rdSZYUg3fmt1dzwpbkY+N2ZHbWllO1wgZ2FuXiRFSE9BLSB3QlkgWk8iWXMhY1NpMnowcipGTnMrdyFzfHBwYU0+eGInQmUlS2pDajJrNDkrfD9WZVMhWihOUUg9XD88MywiT0FVXlchNH1WLX5sdHtTY2tZQEFJYGkiME5QTXBeKUshInhld0Jc1k7Y3tsOipFZTZUVENScmo6SW95Y3Fsbn5TMjhsYWhPOntCeiFaMzcqIWA=bDpyIn49O0tHfGpHRklbJHxIfHwkMy0gRFVkIlJFWDpYPjowUExRWHg6IX0jfk9gWDU3d2UoNnchelIncWt+MzdcXlswUlptIld+MzZ1MEB6fEhZOHkjRG8R09fYmhhUlpqcDd6N0t2O0FBZ28tKWYrO0hjQlVXJ09WLiE+X15USzB6NnFJQ0tNK0NZakRjLFw+bT46JSROJ1I3KT8rN3EnRW9XMV5ZbSQkTmBWdkhcJTZdNzhBRCBFTklodFoicDlyYCphaVRFZHteZmllV1p2MTUgX345R312RUkrKFdPjlRaCpeKk9tS01wVnosRyx4QWBWbnlmTHtraXtMRElXWV92Q1xqSnMjQzQ8SEVxaUEtZHc9L0VUbn5ZPihCIGcuaTJ6LlBLeFx0XCY8RThXdVdKejdBbltxYmF+MXs2S2ZWRmNda2M4LiJpKHlnX0JURGdUMV9JemtaVFBNP2QiMGBQeQ=alAuNlphJCpcTXNmPXw0bXtIUCREaCI2OHR+eyMqWz4wTlgndCshNGc9Nk5+SiNEcSw1bkxzN2EyQlxqR2c1WWwlUmdAYSJmMzlUPTNkcj8=</signerCertificate>
         <signerCertificateTrusted>true</signerCertificateTrusted>
         <file>
            <fileDigest>b7ITt5heY+e6Lm+AXJnYgqBiLos=</fileDigest>
            <fileName>sample-s0812.pdf</fileName>
            <content>TjRMenUuUyhbOC8pQ1RFLV4mNDtndms1XyEwYkhgOW5AcSY8QEo4Ylp9WGc9dENLOGhtb1BxYmhiM1dVeTRjXzZ7VDosJEtpKVhjfE1EOyo8eWwpSmQ4bnZHTTIiKHpAamNDdWNzYHU8LkJcMSBZcGhNLlA7SmtONHNwNmNvMCfR08lbEdPXl4xOFhbdU09WV9vSz5+RVRObXMlR3hZSCZXeClDciZNRDAgPnV2NDJPPXZGTF11LWJUcjkkdG54UGZHUWl+fCsgX0pral4uL18lVTBsVVA=PEBXMCUpJE1pTl56UCVpTi1KWiUrXD00eUpXY3NdW1hfIk4hcn1ZbC88KzxERUgSGJ3TSk6dSkhYyRyMT8zajY1TCE3RkxQcUwhRW5IVCBxSmNKdyUydkpxLzxzZFhsdCEmYjc1L3YjR0RiNXFPakJpNEYsTSJoSGp+Z0dmN1clK1omYGdBeTt3XnY9djF8L15JWiE+KCozZVR2ZFRlalEiPUFHO0puMiJRZWs9SztscHhkUHo+WUxbTTo5e3ovNmgheSQ+bjB7Y3g0OmFtInZeSCE3Tmg6TldtT3wkWCBfRm5CNFVEYnlmbjVUODVkZSh+PD9kJFpcYSYuSztreCo5IDlXciEwVk1aRTw+JFRWSkh9RTxAayk/fUR0ZDwmWlBdfV9WJGcjSHdJO2h4fG4lYm9BREBZdj=fmxFdzRkIjE/ImxSMFpHPTBqI2lnMXdvZn16I0ZNOHdDdFdUWVw2e3NsKDhRQy5gJihoaHVuVzopOkRddXV2MSVzK3E/QmkhenV2XE56dVk8SF1LLF9efF93YH51bFJ5LSN2dVpxQ2pLXWJ+cGl6fn1zdT5PRnZHKSJLVW8qMH5KXlMnMEoLnklK1ZPYDIzT2llKioncnF+NSRuIGQgbS9uZyZzVE5jXHU5JVd7PkBFXiRWdmhxfl12UFM=fjYrJElZdikwRyF1S1RHOkUjZCwqT1xBdi9NVkB+NUR3eDAxO3xkJ0ZTSTA=</content>
         </file>
         <signature>Z+E+fLkWUcNk0yoHWWVjyPxbt4aws0bJBp8ldWn4Dxh+vMdB0HWJynTmSyjqP5NgEohF9u9EvQqMe69t/qAsNJayEuwzkFsS1ESxfjaFRljZ5kDP6Wv/rpF21cW6pMNcLtjPljXKbjJo7BwlC0vdxX1GW+7mmaodz84jbQNCWjBPGbkkQuvXMq+SU9RbONz3B//KjqNhOyElh3q/ZFFuNTQMqMxKVOb9B4mpbrziofedHwRBN6djmb6Pmg6hHxBAbDnwD0yf2Or/L5XsfBhVCbe+jkRPERL9ELXdDtPqY7Qf5jaUmcjWjuY5ufAkY/ltoVSE2NtyZvoATACy388Xug==</signature>
      </ns3:SigningResultResponse>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

*Pasirašymas atšauktas*

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
   <SOAP-ENV:Header/>
   <SOAP-ENV:Body>
      <ns3:SigningResultResponse xmlns:ns3="http://www.registrucentras.lt/onesignservice">
         <status>Canceled</status>
         <file>
            <status>Canceled</status>
            <file>
               <fileId>111111</fileId>
            </file>
         </file>
         <signature>OTlfleO0EHAErM6rzCvv4+7UWqxDATzgfRRKIKNlw/pKfKViKGlccUTr9GE0s703XWLUwb/e5xQZ/eDH1o22nzbLt+9kMCE5FjHotI9GXua19j7f63KKApTNhHwbeE9avXq9IehMbggz7G3c9e4LS5WEg4wYbn4fBixSikOyPUd/229E+Qi6yEtd+gSsb1pZ0XG4yKCSgK1orn9e9+ZaswUnZvJRvGO2liTms9qfjWfynJem4QmByVfKQSpDAEkHVrc53aZZOn8/PT6RMN4K7tu/mbS5j/Zt1FsBHdeHbYJBqY4xSP3t/sNQeGCW+F7I14QRbwJWalZqyPkErfjTcQ==</signature>
      </ns3:SigningResultResponse>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## SigningCancel metodas

Metodas naudojamas pasirašymo transakcijai nutraukti. `SigningCancel` tipas sudarytas iš:

| Elementas  | Tipas | Aprašymas |
| ------------- | ------------- | ------------- |
| clientId | [string[1]](https://www.w3.org/TR/xmlschema-2/#string) | Unikalus pasirašymo paslaugos administratorių suteiktas kliento informacinės sistemos identifikatorius |
| transactionId | [long[1]](https://www.w3.org/TR/xmlschema-2/#long) | Unikalus pasirašymo transakcijos identifikatorius |
| [signature](signature.md) | [base64Binary[1]](https://www.w3.org/TR/xmlschema-2/#base64Binary)  | Kliento sistemos sugeneruotas parašas patvirtinantis užklausos duomenų teisingumą |

### Užklausos pavyzdys

```xml
<SOAP-ENV:Envelope
	xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
	xmlns:ns1="http://www.registrucentras.lt/onesignservice">
	<SOAP-ENV:Body>
		<ns1:SigningCancelRequest>
			<clientId>client_id</clientId>
			<transactionId>445169</transactionId>
			<signature>D+1+GzXTALfhn+A9oSJhqKD8FCUYJwRrVmqDak55EVTrsieNilQh98IrUEZYfoDcwvsJJSX/uxpKuEvJSLTAQ1N6xfUR3Pt2CCTjZfeF4Fq3jeCQyfWAr547kNlSIdPXo7WZChyUFHNbiceA6KfaoWeZPeA7Q29Ei2O4qM7TrPU=</signature>
		</ns1:SigningCancelRequest>
	</SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

### Atsakymo pavyzdys

```bash
HTTP/1.1 202 Accepted
Date: Fri, 06 Aug 2021 07:59:54 GMT
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Pragma: no-cache
Expires: 0
Strict-Transport-Security: max-age=31536000 ; includeSubDomains
X-XSS-Protection: 1; mode=block
X-Content-Type-Options: nosniff
Content-Length: 0
Keep-Alive: timeout=1
Connection: Keep-Alive
Content-Type: text/plain
```

## Seal metodas (tik Registrų centro sistemoms)

Metodas naudojamas RC spaudui uždėti ant PDF dokumentų. `Seal` tipas sudarytas iš:

| Elementas  | Tipas | Aprašymas |
| ------------- | ------------- | ------------- |
| pin | [string[1]](https://www.w3.org/TR/xmlschema-2/#string) | Spaudo PIN kodas |
| clientInfo | [SignRequestClientId[1]](#signrequestclientid-struktūrinis-tipas)  | Informacija apie klientą |
| signatureDisplayProperties  | [SignatureDisplayProperties[1]](#signaturedisplayproperties-struktūrinis-tipas) | Parašo atvaizdavimo nustatymai |
| file  | [SourceFileBinary[1..N]](#sourcefilebinary-struktūrinis-tipas)  | Failas pasirašymui. Pateikiamas failo turinys (tik PDF failai) |
| [signature](signature.md)  | [base64Binary[1]](https://www.w3.org/TR/xmlschema-2/#base64Binary)  | Kliento sistemos sugeneruotas parašas patvirtinantis užklausos duomenų teisingumą |

## Seal metode naudojami kiti struktūriniai tipai

### SignRequestClientId struktūrinis tipas

Struktūrinis duomenų tipas `SignRequestClientId` aprašomas kaip:

| Elementas  | Tipas | Aprašymas |
| ------------- | ------------- | ------------- |
| clientId  | [string[1]](https://www.w3.org/TR/xmlschema-2/#string) | Unikalus pasirašymo paslaugos administratorių suteiktas kliento informacinės sistemos identifikatorius |

### Užklausos pavyzdys be parašo atvaizdavimo

```xml
<SOAP-ENV:Envelope
	xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
	xmlns:ones="http://www.registrucentras.lt/onesignservice">
	<SOAP-ENV:Body>
		<ones:SealRequest>
			<ones:pin>*******</ones:pin>
			<ones:clientInfo>
				<clientId>client_id</clientId>
			</ones:clientInfo>
			<ones:file>
				<fileDigest>v9AJ9QDAVxlf/eZvrmT5L6X1m3I=</fileDigest>
				<fileName>sample.pdf</fileName>
				<content>TjRMenUuUyhbOC8pQ1RFLV4mNDtndms1XyEwYkhgOW5AcSY8QEo4Ylp9WGc9dENLOGhtb1BxYmhiM1dVeTRjXzZ7VDosJEtpKVhjfE1EOyo8eWwpSmQ4bnZHTTIiKHpAamNDdWNzYHU8LkJcMSBZcGhNLlA7SmtONHNwNmNvMCfR08lbEdPXl4xOFhbdU09WV9vSz5+RVRObXMlR3hZSCZXeClDciZNRDAgPnV2NDJPPXZGTF11LWJUcjkkdG54UGZHUWl+fCsgX0pral4uL18lVTBsVVA=PEBXMCUpJE1pTl56UCVpTi1KWiUrXD00eUpXY3NdW1hfIk4hcn1ZbC88KzxERUgSGJ3TSk6dSkhYyRyMT8zajY1TCE3RkxQcUwhRW5IVCBxSmNKdyUydkpxLzxzZFhsdCEmYjc1L3YjR0RiNXFPakJpNEYsTSJoSGp+Z0dmN1clK1omYGdBeTt3XnY9djF8L15JWiE+KCozZVR2ZFRlalEiPUFHO0puMiJRZWs9SztscHhkUHo+WUxbTTo5e3ovNmgheSQ+bjB7Y3g0OmFtInZeSCE3Tmg6TldtT3wkWCBfRm5CNFVEYnlmbjVUODVkZSh+PD9kJFpcYSYuSztreCo5IDlXciEwVk1aRTw+JFRWSkh9RTxAayk/fUR0ZDwmWlBdfV9WJGcjSHdJO2h4fG4lYm9BREBZdj=fmxFdzRkIjE/ImxSMFpHPTBqI2lnMXdvZn16I0ZNOHdDdFdUWVw2e3NsKDhRQy5gJihoaHVuVzopOkRddXV2MSVzK3E/QmkhenV2XE56dVk8SF1LLF9efF93YH51bFJ5LSN2dVpxQ2pLXWJ+cGl6fn1zdT5PRnZHKSJLVW8qMH5KXlMnMEoLnklK1ZPYDIzT2llKioncnF+NSRuIGQgbS9uZyZzVE5jXHU5JVd7PkBFXiRWdmhxfl12UFM=fjYrJElZdikwRyF1S1RHOkUjZCwqT1xBdi9NVkB+NUR3eDAxO3xkJ0ZTSTA=</content>
			</ones:file>
			<signature>PMuOewIFfS+uualQuTO2uAAbl/OFv219Xp6jtGC13eTbocAoVIJJeu/xmngJpt5rgcjldN0/mGuFY6rh9eDTBDRa8HDXK43VQYRBheHt/QQEJh3DvDmcblrUP30aV8nq0lowYR5xhmxIZDkFwTTaUn9fV476gaG63qBXhCJdx4k=</signature>
		</ones:SealRequest>
	</SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

### Užklausos pavyzdys su parašo atvaizdavimu

```xml
<SOAP-ENV:Envelope
	xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
	xmlns:ones="http://www.registrucentras.lt/onesignservice">
	<SOAP-ENV:Body>
		<ones:SealRequest>
			<ones:pin>*******</ones:pin>
			<ones:clientInfo>
				<clientId>client_id</clientId>
			</ones:clientInfo>
			<ones:signatureDisplayProperties>
				<position>absolute, 1, 2cm, -6cm, 8cm, 4cm</position>
				<displayValidity>true</displayValidity>
				<signatureImageUrl>https://www.registrucentras.lt/img/logo/rc.jpg</signatureImageUrl>
				<backgroundImageUrl>https://www.registrucentras.lt/img/logo/rc.jpg</backgroundImageUrl>
			</ones:signatureDisplayProperties>
			<ones:file>
				<fileDigest>v9AJ9QDAVxlf/eZvrmT5L6X1m3I=</fileDigest>
				<fileName>sample.pdf</fileName>
				<content>TjRMenUuUyhbOC8pQ1RFLV4mNDtndms1XyEwYkhgOW5AcSY8QEo4Ylp9WGc9dENLOGhtb1BxYmhiM1dVeTRjXzZ7VDosJEtpKVhjfE1EOyo8eWwpSmQ4bnZHTTIiKHpAamNDdWNzYHU8LkJcMSBZcGhNLlA7SmtONHNwNmNvMCfR08lbEdPXl4xOFhbdU09WV9vSz5+RVRObXMlR3hZSCZXeClDciZNRDAgPnV2NDJPPXZGTF11LWJUcjkkdG54UGZHUWl+fCsgX0pral4uL18lVTBsVVA=PEBXMCUpJE1pTl56UCVpTi1KWiUrXD00eUpXY3NdW1hfIk4hcn1ZbC88KzxERUgSGJ3TSk6dSkhYyRyMT8zajY1TCE3RkxQcUwhRW5IVCBxSmNKdyUydkpxLzxzZFhsdCEmYjc1L3YjR0RiNXFPakJpNEYsTSJoSGp+Z0dmN1clK1omYGdBeTt3XnY9djF8L15JWiE+KCozZVR2ZFRlalEiPUFHO0puMiJRZWs9SztscHhkUHo+WUxbTTo5e3ovNmgheSQ+bjB7Y3g0OmFtInZeSCE3Tmg6TldtT3wkWCBfRm5CNFVEYnlmbjVUODVkZSh+PD9kJFpcYSYuSztreCo5IDlXciEwVk1aRTw+JFRWSkh9RTxAayk/fUR0ZDwmWlBdfV9WJGcjSHdJO2h4fG4lYm9BREBZdj=fmxFdzRkIjE/ImxSMFpHPTBqI2lnMXdvZn16I0ZNOHdDdFdUWVw2e3NsKDhRQy5gJihoaHVuVzopOkRddXV2MSVzK3E/QmkhenV2XE56dVk8SF1LLF9efF93YH51bFJ5LSN2dVpxQ2pLXWJ+cGl6fn1zdT5PRnZHKSJLVW8qMH5KXlMnMEoLnklK1ZPYDIzT2llKioncnF+NSRuIGQgbS9uZyZzVE5jXHU5JVd7PkBFXiRWdmhxfl12UFM=fjYrJElZdikwRyF1S1RHOkUjZCwqT1xBdi9NVkB+NUR3eDAxO3xkJ0ZTSTA=</content>
			</ones:file>
			<signature>PMuOewIFfS+uualQuTO2uAAbl/OFv219Xp6jtGC13eTbocAoVIJJeu/xmngJpt5rgcjldN0/mGuFY6rh9eDTBDRa8HDXK43VQYRBheHt/QQEJh3DvDmcblrUP30aV8nq0lowYR5xhmxIZDkFwTTaUn9fV476gaG63qBXhCJdx4k=</signature>
		</ones:SealRequest>
	</SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

### Atsakymo pavyzdys

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
   <SOAP-ENV:Header/>
   <SOAP-ENV:Body>
      <ns3:SealResponse xmlns:ns3="http://www.registrucentras.lt/onesignservice">
	  <status>Signed</status>
         <signerCertificate>PDV1WF47KldsSyYgbmByZ2dHfWNLOXEmP3gwcCV7OHNndlZffCZsbmlyNmZVI0R8T083NERSUmpsWHZ7Y3JUdzBPWVo+czMwYVxPTlZBaG1UQD5ic1tAWFBqbFNIKlttK0lbKy1abE8zJn5PTE0iVyg8cG4rdSZYUg3fmt1dzwpbkY+N2ZHbWllO1wgZ2FuXiRFSE9BLSB3QlkgWk8iWXMhY1NpMnowcipGTnMrdyFzfHBwYU0+eGInQmUlS2pDajJrNDkrfD9WZVMhWihOUUg9XD88MywiT0FVXlchNH1WLX5sdHtTY2tZQEFJYGkiME5QTXBeKUshInhld0Jc1k7Y3tsOipFZTZUVENScmo6SW95Y3Fsbn5TMjhsYWhPOntCeiFaMzcqIWA=bDpyIn49O0tHfGpHRklbJHxIfHwkMy0gRFVkIlJFWDpYPjowUExRWHg6IX0jfk9gWDU3d2UoNnchelIncWt+MzdcXlswUlptIld+MzZ1MEB6fEhZOHkjRG8R09fYmhhUlpqcDd6N0t2O0FBZ28tKWYrO0hjQlVXJ09WLiE+X15USzB6NnFJQ0tNK0NZakRjLFw+bT46JSROJ1I3KT8rN3EnRW9XMV5ZbSQkTmBWdkhcJTZdNzhBRCBFTklodFoicDlyYCphaVRFZHteZmllV1p2MTUgX345R312RUkrKFdPjlRaCpeKk9tS01wVnosRyx4QWBWbnlmTHtraXtMRElXWV92Q1xqSnMjQzQ8SEVxaUEtZHc9L0VUbn5ZPihCIGcuaTJ6LlBLeFx0XCY8RThXdVdKejdBbltxYmF+MXs2S2ZWRmNda2M4LiJpKHlnX0JURGdUMV9JemtaVFBNP2QiMGBQeQ=alAuNlphJCpcTXNmPXw0bXtIUCREaCI2OHR+eyMqWz4wTlgndCshNGc9Nk5+SiNEcSw1bkxzN2EyQlxqR2c1WWwlUmdAYSJmMzlUPTNkcj8=</signerCertificate>
         <signerCertificateTrusted>true</signerCertificateTrusted>
         <file>
            <fileDigest>b7ITt5heY+e6Lm+AXJnYgqBiLos=</fileDigest>
            <fileName>sample-s0812.pdf</fileName>
            <content>TjRMenUuUyhbOC8pQ1RFLV4mNDtndms1XyEwYkhgOW5AcSY8QEo4Ylp9WGc9dENLOGhtb1BxYmhiM1dVeTRjXzZ7VDosJEtpKVhjfE1EOyo8eWwpSmQ4bnZHTTIiKHpAamNDdWNzYHU8LkJcMSBZcGhNLlA7SmtONHNwNmNvMCfR08lbEdPXl4xOFhbdU09WV9vSz5+RVRObXMlR3hZSCZXeClDciZNRDAgPnV2NDJPPXZGTF11LWJUcjkkdG54UGZHUWl+fCsgX0pral4uL18lVTBsVVA=PEBXMCUpJE1pTl56UCVpTi1KWiUrXD00eUpXY3NdW1hfIk4hcn1ZbC88KzxERUgSGJ3TSk6dSkhYyRyMT8zajY1TCE3RkxQcUwhRW5IVCBxSmNKdyUydkpxLzxzZFhsdCEmYjc1L3YjR0RiNXFPakJpNEYsTSJoSGp+Z0dmN1clK1omYGdBeTt3XnY9djF8L15JWiE+KCozZVR2ZFRlalEiPUFHO0puMiJRZWs9SztscHhkUHo+WUxbTTo5e3ovNmgheSQ+bjB7Y3g0OmFtInZeSCE3Tmg6TldtT3wkWCBfRm5CNFVEYnlmbjVUODVkZSh+PD9kJFpcYSYuSztreCo5IDlXciEwVk1aRTw+JFRWSkh9RTxAayk/fUR0ZDwmWlBdfV9WJGcjSHdJO2h4fG4lYm9BREBZdj=fmxFdzRkIjE/ImxSMFpHPTBqI2lnMXdvZn16I0ZNOHdDdFdUWVw2e3NsKDhRQy5gJihoaHVuVzopOkRddXV2MSVzK3E/QmkhenV2XE56dVk8SF1LLF9efF93YH51bFJ5LSN2dVpxQ2pLXWJ+cGl6fn1zdT5PRnZHKSJLVW8qMH5KXlMnMEoLnklK1ZPYDIzT2llKioncnF+NSRuIGQgbS9uZyZzVE5jXHU5JVd7PkBFXiRWdmhxfl12UFM=fjYrJElZdikwRyF1S1RHOkUjZCwqT1xBdi9NVkB+NUR3eDAxO3xkJ0ZTSTA=</content>
         </file>
         <signature>lDzM9em93bknUW/TdTGtqd97JyCFEdOBbuLUzWFxLNCRJAnoe/bF/zkj9jdByWl6CWwOj6ECqy3Sb6mZ9JoPWDvHdWnKYxd/QerqZMWA+IOuWTWbmAZxTyncHvVlP6yxZkCSVYQmkuywKPJG8Ra86W9h3n0HiXmgIo6Gf+rtty/AVA+zefhhuhoHwn6B8uXJ9mgNLuD6mtKZq+Iw5pStUFNTjRGID5HEtEQ9SmUgcKgjHCon1HcsKRxGulMWOCo3jqNejJt+08TVTTKa9DQmKY35nUFC5cehQwjX2E2XJtDHPxXoKhFJJvw4g27gZjUb1j/mZzkK0R3RE9WYp11sqQ==</signature>
      </ns3:SealResponse>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Timestamp metodas

Metodas naudojamas dėti laiko žymoms ant dokumentų. Užklausa sudaryta iš `TimestampRequest` tipo, kuris sudarytas iš:

| Elementas  | Tipas | Aprašymas |
| ------------- | ------------- | ------------- |
| clientInfo  | [SignRequestWebClientInfo[1]](#signrequestwebclientinfo-struktūrinis-tipas)  | Informacija apie klientą |
| file  | [SourceFileBinary[1]](#sourcefilebinary-struktūrinis-tipas)  | Failas pasirašymui. Pateikiamas failo turinys (tik PDF failai) |
| [signature](signature.md) | [base64Binary[1]](https://www.w3.org/TR/xmlschema-2/#base64Binary)  | Kliento sistemos sugeneruotas parašas patvirtinantis užklausos duomenų teisingumą |

### Užklausos pavyzdys

```xml
<SOAP-ENV:Envelope
	xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
	xmlns:ns1="http://www.registrucentras.lt/onesignservice">
	<SOAP-ENV:Body>
		<ns1:TimestampRequest>
			<ns1:clientInfo>
				<clientId>client_id</clientId>
			</ns1:clientInfo>
			<ones:file>
				<fileDigest>v9AJ9QDAVxlf/eZvrmT5L6X1m3I=</fileDigest>
				<fileName>sample.pdf</fileName>
				<content>TjRMenUuUyhbOC8pQ1RFLV4mNDtndms1XyEwYkhgOW5AcSY8QEo4Ylp9WGc9dENLOGhtb1BxYmhiM1dVeTRjXzZ7VDosJEtpKVhjfE1EOyo8eWwpSmQ4bnZHTTIiKHpAamNDdWNzYHU8LkJcMSBZcGhNLlA7SmtONHNwNmNvMCfR08lbEdPXl4xOFhbdU09WV9vSz5+RVRObXMlR3hZSCZXeClDciZNRDAgPnV2NDJPPXZGTF11LWJUcjkkdG54UGZHUWl+fCsgX0pral4uL18lVTBsVVA=PEBXMCUpJE1pTl56UCVpTi1KWiUrXD00eUpXY3NdW1hfIk4hcn1ZbC88KzxERUgSGJ3TSk6dSkhYyRyMT8zajY1TCE3RkxQcUwhRW5IVCBxSmNKdyUydkpxLzxzZFhsdCEmYjc1L3YjR0RiNXFPakJpNEYsTSJoSGp+Z0dmN1clK1omYGdBeTt3XnY9djF8L15JWiE+KCozZVR2ZFRlalEiPUFHO0puMiJRZWs9SztscHhkUHo+WUxbTTo5e3ovNmgheSQ+bjB7Y3g0OmFtInZeSCE3Tmg6TldtT3wkWCBfRm5CNFVEYnlmbjVUODVkZSh+PD9kJFpcYSYuSztreCo5IDlXciEwVk1aRTw+JFRWSkh9RTxAayk/fUR0ZDwmWlBdfV9WJGcjSHdJO2h4fG4lYm9BREBZdj=fmxFdzRkIjE/ImxSMFpHPTBqI2lnMXdvZn16I0ZNOHdDdFdUWVw2e3NsKDhRQy5gJihoaHVuVzopOkRddXV2MSVzK3E/QmkhenV2XE56dVk8SF1LLF9efF93YH51bFJ5LSN2dVpxQ2pLXWJ+cGl6fn1zdT5PRnZHKSJLVW8qMH5KXlMnMEoLnklK1ZPYDIzT2llKioncnF+NSRuIGQgbS9uZyZzVE5jXHU5JVd7PkBFXiRWdmhxfl12UFM=fjYrJElZdikwRyF1S1RHOkUjZCwqT1xBdi9NVkB+NUR3eDAxO3xkJ0ZTSTA=</content>
			</ones:file>
			<signature>PMuOewIFfS+uualQuTO2uAAbl/OFv219Xp6jtGC13eTbocAoVIJJeu/xmngJpt5rgcjldN0/mGuFY6rh9eDTBDRa8HDXK43VQYRBheHt/QQEJh3DvDmcblrUP30aV8nq0lowYR5xhmxIZDkFwTTaUn9fV476gaG63qBXhCJdx4k=</signature>
		</ns1:TimestampRequest>
	</SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

### Atsakymo pavyzdys

```xml
<SOAP-ENV:Envelope
	xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
	<SOAP-ENV:Header/>
	<SOAP-ENV:Body>
		<ns3:TimestampResponse
			xmlns:ns3="http://www.registrucentras.lt/onesignservice">
			<status>Signed</status>
			<file>
				<fileDigest>b7ITt5heY+e6Lm+AXJnYgqBiLos=</fileDigest>
				<fileName>sample-s0812.pdf</fileName>
				<content>TjRMenUuUyhbOC8pQ1RFLV4mNDtndms1XyEwYkhgOW5AcSY8QEo4Ylp9WGc9dENLOGhtb1BxYmhiM1dVeTRjXzZ7VDosJEtpKVhjfE1EOyo8eWwpSmQ4bnZHTTIiKHpAamNDdWNzYHU8LkJcMSBZcGhNLlA7SmtONHNwNmNvMCfR08lbEdPXl4xOFhbdU09WV9vSz5+RVRObXMlR3hZSCZXeClDciZNRDAgPnV2NDJPPXZGTF11LWJUcjkkdG54UGZHUWl+fCsgX0pral4uL18lVTBsVVA=PEBXMCUpJE1pTl56UCVpTi1KWiUrXD00eUpXY3NdW1hfIk4hcn1ZbC88KzxERUgSGJ3TSk6dSkhYyRyMT8zajY1TCE3RkxQcUwhRW5IVCBxSmNKdyUydkpxLzxzZFhsdCEmYjc1L3YjR0RiNXFPakJpNEYsTSJoSGp+Z0dmN1clK1omYGdBeTt3XnY9djF8L15JWiE+KCozZVR2ZFRlalEiPUFHO0puMiJRZWs9SztscHhkUHo+WUxbTTo5e3ovNmgheSQ+bjB7Y3g0OmFtInZeSCE3Tmg6TldtT3wkWCBfRm5CNFVEYnlmbjVUODVkZSh+PD9kJFpcYSYuSztreCo5IDlXciEwVk1aRTw+JFRWSkh9RTxAayk/fUR0ZDwmWlBdfV9WJGcjSHdJO2h4fG4lYm9BREBZdj=fmxFdzRkIjE/ImxSMFpHPTBqI2lnMXdvZn16I0ZNOHdDdFdUWVw2e3NsKDhRQy5gJihoaHVuVzopOkRddXV2MSVzK3E/QmkhenV2XE56dVk8SF1LLF9efF93YH51bFJ5LSN2dVpxQ2pLXWJ+cGl6fn1zdT5PRnZHKSJLVW8qMH5KXlMnMEoLnklK1ZPYDIzT2llKioncnF+NSRuIGQgbS9uZyZzVE5jXHU5JVd7PkBFXiRWdmhxfl12UFM=fjYrJElZdikwRyF1S1RHOkUjZCwqT1xBdi9NVkB+NUR3eDAxO3xkJ0ZTSTA=</content>
			</file>
			<signature>lDzM9em93bknUW/TdTGtqd97JyCFEdOBbuLUzWFxLNCRJAnoe/bF/zkj9jdByWl6CWwOj6ECqy3Sb6mZ9JoPWDvHdWnKYxd/QerqZMWA+IOuWTWbmAZxTyncHvVlP6yxZkCSVYQmkuywKPJG8Ra86W9h3n0HiXmgIo6Gf+rtty/AVA+zefhhuhoHwn6B8uXJ9mgNLuD6mtKZq+Iw5pStUFNTjRGID5HEtEQ9SmUgcKgjHCon1HcsKRxGulMWOCo3jqNejJt+08TVTTKa9DQmKY35nUFC5cehQwjX2E2XJtDHPxXoKhFJJvw4g27gZjUb1j/mZzkK0R3RE9WYp11sqQ==</signature>
		</ns3:TimestampResponse>
	</SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```
