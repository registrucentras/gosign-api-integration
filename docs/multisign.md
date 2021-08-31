---
layout: default
title: MultiSign webservisas
parent: Webservisas
nav_order: 2
---

# MultiSign webservisas

***MultiSign** - webservisas leidžiantis vienu metu **pasirašyti daugiau nei vieną dokumentą**. Šiuo metodu **galima pasirašyti tik stacionariu parašu**.*

- *InitMultiSign*, naudojamas **inicijuoti** dokumentų **pasirašymą**;
- *SigningResult*, naudojamas pasirašymo **būsenai sužinoti** (jei pasirašymo transakcija yra pasibaigusi sėkmingai, gražinamas pasirašytų dokumentų sąrašas ir informaciją apie pasirašiusį asmenį);
- *SigningCancel*, naudojamas pasirašymo **transakcijai nutraukti**.

## InitMultiSign metodas

`InitMultiSign` metodo struktūrinis duomenų tipas sudarytas iš elementų:

| Elementas  | Tipas | Aprašymas |
| ------------- | ------------- | ------------- |
| clientInfo  | [SignRequestWebClientInfo[1]](#signrequestwebclientinfo-struktūrinis-tipas)  | Informacija apie klientą |
| signatureMetadata  | [StandardSignatureMetadata[1]](#standardsignaturemetadata-struktūrinis-tipas) | Parašo metaduomenys |
| signatureDisplayProperties  | [SignatureDisplayProperties[1]](#signaturedisplayproperties-struktūrinis-tipas) | Parašo atvaizdavimo nustatymai |
| signingType  | [signingType[1]](#signingtype-tipas) | Reikšmė nurodanti ar elektroniniame paraše turi būti uždėta laiko žyma ar žyma su patikra ([OCSP](https://en.wikipedia.org/wiki/Online_Certificate_Status_Protocol)) |
| file  | [SourceFileRemote[1..N]](#sourcefileremote-struktūrinis-tipas)  | Failas pasirašymui. Failai pateikiamai kaip nuorodos pasiekiamos iš pasirašymo serverio aplinkos (tik PDF failai) |
| [signature](signature.md)  | [base64Binary[1]](https://www.w3.org/TR/xmlschema-2/#base64Binary)  | Kliento sistemos sugeneruotas parašas patvirtinantis užklausos duomenų teisingumą |

## InitMultiSign metode naudojami kiti struktūriniai tipai

### SignRequestWebClientInfo struktūrinis tipas

Struktūrinis duomenų tipas `SignRequestWebClientInfo` aprašo `SignRequestClientInfo` praplėtimą. Rezultate užklausa sudaryta iš:

| Elementas  | Tipas | Aprašymas |
| ------------- | ------------- | ------------- |
| clientId  | [string[1]](https://www.w3.org/TR/xmlschema-2/#string) | Unikalus pasirašymo paslaugos administratorių suteiktas kliento informacinės sistemos identifikatorius |
| signerPersonalCode  | [string[0..1]](https://www.w3.org/TR/xmlschema-2/#string) | Asmens, kuris turi pasirašyti dokumentą, asmens kodas. Jei šis parametras nenurodytas, dokumentą gali pasirašyti bet kuris asmuo |
| locale  | [string[0..1]](https://www.w3.org/TR/xmlschema-2/#string) | Pasirašymo paslaugos vartotojo sąsajos kalba ("lt" – Lietuvių kalba, "en" – Anglų kalba) |
| responseUrl  | [string[1]](https://www.w3.org/TR/xmlschema-2/#string) | Web puslapio, kuris pasibaigus dokumento pasirašymui turi būti užkrautas naudotojo naršyklėje, adresas |
| remoteAddress  | [string[0..1]](https://www.w3.org/TR/xmlschema-2/#string) | Pasirašymo paslaugos vartotojo prisijungimo IP adresas. Naudojamas statistiniams duomenims rinkti |
| acceptableInfrastructure  | [infrastructureType[0..2]](#infrastructuretype-tipas) | Sąrašas, nurodantis kokio tipo pasirašymo infrastruktūra gali būti naudojama pasirašymui. Galimos elemento reikšmės aprašytos skyriuje. Kiekvienai leistinai infrastruktūrai nurodomas atskiras acceptableInfrastructure elementas. Jei nėra nurodytas nei vienas acceptableInfrastructure elementas, leidžiama pasirašyti naudojant bet kurią infrastruktūrą |

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
| signatureImage  | [string[0..1]](https://www.w3.org/TR/xmlschema-2/#string) | Parašo vizualizacijoje naudojamo paveiksliuko nuoroda (URL). Gali būti nurodoma viešai prieinamo JPEG, PNG arba GIF tipo paveiksliuko pilna nuoroda. Jei parašo paveiksliukas yra nurodytas, parašo vizualizacijai skiriama vieta yra dalinama perpus, kairėje pusėje rodomas paveiksliukas, o dešinėje – parašo informacija. Geriausi rezultatai gaunami, kai parašo paveiksliukas yra pusiau skaidrus (t.y. per paveiksliuko neužimtas vietas persišviečia dokumento fonas) |
| backgroundImage  | [string[0..1]](https://www.w3.org/TR/xmlschema-2/#string) | Parašo vizualizacijoje naudojamo fono paveiksliuko („vandens ženklų“) nuoroda (URL). Turi būti nurodoma viešai prieinamo JPEG, PNG arba GIF tipo paveiksliuko pilna nuoroda. Geriausi rezultatai gaunami, kai parašo paveiksliukas yra pusiau skaidrus (t.y. per fono paveiksliuką persišviečia dokumento fonas) |

### signingType tipas

Paprastas duomenų tipas `signingType` nurodo pasirašymo tipą:

| Reikšmė  | Aprašymas |
| ------------- | ------------- |
| Signature  | Uždedamas parašas |
| SignatureWithTimestamp | Uždedamas parašas su laiko žyma |
| SignatureWithTimestampOCSP | Uždedamas parašas su laiko žyma ir [OCSP](https://en.wikipedia.org/wiki/Online_Certificate_Status_Protocol) |

### infrastructureType tipas

Paprastas duomenų tipas `infrastructureType` nurodo galimus pasirašymo infrastruktūros tipus:

| Reikšmė  | Aprašymas |
| ------------- | ------------- |
| Stationary  | Leidžiama pasirašyti naudojant stacionarų elektroninį parašą |
| Mobile | Leidžiama pasirašyti naudojant mobilų elektroninį parašą |

### SourceFileRemote struktūrinis tipas

Struktūrinis duomenų tipas `SourceFileRemote` aprašo `SourceFile` tipo praplėtimą nuotolinio failo perdavimo atveju, kai su užklausa failo tūrinis nėra perduodamas, bet perduodamas jo URL. Šį tipą sudaro šie elementai:

| Elementas  | Tipas | Aprašymas |
| ------------- | ------------- | ------------- |
| [fileDigest](filedigest.md) | [base64Binary[1]](https://www.w3.org/TR/xmlschema-2/#base64Binary) | Pasirašymui skirto dokumento SHA-1 santrauka |
| fileName | [string[0..1]](https://www.w3.org/TR/xmlschema-2/#string) | Pasirašymui skirto dokumento failo vardas. Failo vardas turi baigtis prievardžiu „.pdf“. Šis elementas gali būti praleistas. Tokiu atveju Pasirašymo paslaugos vartotojo sąsajoje pasirašomo failo vardas nebus rodomas |
| fileId | [string[1]](https://www.w3.org/TR/xmlschema-2/#string) | Pasirašymui skirto dokumento identifikatorius Kliento sistemoje. Šis elementas yra privalomas. Gali būti generuojamas, kaip pvz. documentURL dalis. Maksimalus ilgis: 128 |
| fileURL | [anyURI[1]](https://www.w3.org/TR/xmlschema-2/#anyURI) | Unikalus dokumento URL. Pastaba: Šį URL srautinio pasirašymo paslauga naudos dokumentui atsisiųsti, todėl jis privalo būti viešai prieinamas |

### Užklausos pavyzdys

```xml
<SOAP-ENV:Envelope
	xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
	xmlns:ns1="http://www.registrucentras.lt/multisignservice">
	<SOAP-ENV:Body>
		<ns1:InitMultiSignRequest>
			<ns1:clientInfo>
				<clientId>client_id</clientId>
				<signerPersonalCode>signer_code</signerPersonalCode>
			</ns1:clientInfo>
			<ns1:signingType>Signature</ns1:signingType>
			<ns1:file>
				<fileDigest>0v5NcpPFHEttzbsxm0urXlc5MIE=</fileDigest>
				<fileName>pdf.pdf</fileName>
				<fileId>111111</fileId>
				<fileURL>https://example.com/sites/default/files/pdf.pdff</fileURL>
			</ns1:file>
<ns1:signature>l8iG/KR/FnueC05CLlFiotUkUrNBY3SSpoGsZnW/0FbqoZ5wWVpkfusjTn9eHnNelXy/g5f1/NGISDSb8bJfd9TqNZEKt0VyYm5b5ub5zmtCvyxLzgMVmQmBt6QNBi7f7a4zr7GwOmClMJAMVBclY7LnHdR+TeYMsGqCYhnZYqA=</ns1:signature>
		</ns1:InitMultiSignRequest>
	</SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

### Atsakymo pavyzdys

```xml
<SOAP-ENV:Envelope
	xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
	<SOAP-ENV:Header/>
	<SOAP-ENV:Body>
		<ns3:InitMultiSignResponse
			xmlns:ns3="http://www.registrucentras.lt/multisignservice">
			<transactionId>445153</transactionId>
			<signingUrl>https://example.com/unisign/ui/445153/84af48437eaeccf226e91c56b7230c1104a6c175/multisignMain?locale=lt</signingUrl>
			<signature>ksQ5eS+1hrav/GqaPioLIbbsOH3E0iVaTPbKZK4KZ26mUflkCTldn3aXvMPs8rqmuQWI/dPx0A1BR6wDTMW3R5UUAi28ejf/7cedX6pLRz1SqLLTxy+onS2VxG/gpdNHkqGWJRVwQLg0F8m5w/p/vzpcyqymFDcbO+4Bn0PipYNpxmEtEib7jwTinYN/aoFj21ZU8YHyCi3WizCrds2w734oGPwLnVZmRviCejPXYm4E2WEDOaXsBCW6DpElECkjMLHo1K+gU9nZ9Tug9bwVYXOns10OXrjUtOk5bSuiS0b6DeAyZuo75+n7zmOL7sf1y+tcCCIbJ8II8X+2arckjQ==</signature>
		</ns3:InitMultiSignResponse>
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
	xmlns:ns1="http://www.registrucentras.lt/multisignservice">
	<SOAP-ENV:Body>
		<ns1:MultiSignResultRequest>
			<clientId>client_id</clientId>
            <transactionId>445154</transactionId>
                
			<signature>l6kH7Q3dJuaIBJHMq1BAkSZZQFg+qqhhCIurNOn24Wi599PspFGjs41VNLmW49VVeHx+XR0i6Uxy7i3JHXzM9TuYHrWgl6qXS12pz/aSWVTBdNX762CnWoe9b+bv2ZIU+bmi9J56e1fQNlV+9YjSj5EkfbiioHxmXGrEtvxMivw=</signature>
		</ns1:MultiSignResultRequest>
	</SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

### Atsakymų pavyzdžiai

*Laukiama pasirašymo*

```xml
<SOAP-ENV:Envelope
	xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
	<SOAP-ENV:Header/>
	<SOAP-ENV:Body>
		<ns3:MultiSignResultResponse
			xmlns:ns3="http://www.registrucentras.lt/multisignservice">
            <status>InProgress</status>
                <signature>rIZ/zyP5FtB19rQiYkpM1LIJFbTJlpgBNcG7iN1iSY1ugmlTWfi2+KCr8gjR8GfF5GhA7Yk6KvPHuSAzvJ/BSiOdfvc8CrIDanlDkohUuuSkaRju+bgBfjzkmJkn1lFJ7iNqtSCzu6xMDiUDA6Z6uuu/eBcLvjIsvaAYLBKNTwcYInrLADAgKrFrNBTLyZUduqbrpJb+RwQNsADZdcQ7mI632P/mwEfyIrgcPHyrBC6M+NhcxHiYlIV3jqn1Ye2m3wpsjLrC3nsrQyxB+5fSfmPc2mqzzBvTCSO14gozBOGFeN4pIeLITQXlqad7+Duytlmt2VnogNM/d4CoRhPoTQ==</signature>
		</ns3:MultiSignResultResponse>
	</SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

*Po pasirašymo*

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
   <SOAP-ENV:Header/>
   <SOAP-ENV:Body>
      <ns3:MultiSignResultResponse xmlns:ns3="http://www.registrucentras.lt/multisignservice">
         <status>Signed</status>
         <signerCertificate>PDV1WF47KldsSyYgbmByZ2dHfWNLOXEmP3gwcCV7OHNndlZffCZsbmlyNmZVI0R8T083NERSUmpsWHZ7Y3JUdzBPWVo+czMwYVxPTlZBaG1UQD5ic1tAWFBqbFNIKlttK0lbKy1abE8zJn5PTE0iVyg8cG4rdSZYUg3fmt1dzwpbkY+N2ZHbWllO1wgZ2FuXiRFSE9BLSB3QlkgWk8iWXMhY1NpMnowcipGTnMrdyFzfHBwYU0+eGInQmUlS2pDajJrNDkrfD9WZVMhWihOUUg9XD88MywiT0FVXlchNH1WLX5sdHtTY2tZQEFJYGkiME5QTXBeKUshInhld0Jc1k7Y3tsOipFZTZUVENScmo6SW95Y3Fsbn5TMjhsYWhPOntCeiFaMzcqIWA=bDpyIn49O0tHfGpHRklbJHxIfHwkMy0gRFVkIlJFWDpYPjowUExRWHg6IX0jfk9gWDU3d2UoNnchelIncWt+MzdcXlswUlptIld+MzZ1MEB6fEhZOHkjRG8R09fYmhhUlpqcDd6N0t2O0FBZ28tKWYrO0hjQlVXJ09WLiE+X15USzB6NnFJQ0tNK0NZakRjLFw+bT46JSROJ1I3KT8rN3EnRW9XMV5ZbSQkTmBWdkhcJTZdNzhBRCBFTklodFoicDlyYCphaVRFZHteZmllV1p2MTUgX345R312RUkrKFdPjlRaCpeKk9tS01wVnosRyx4QWBWbnlmTHtraXtMRElXWV92Q1xqSnMjQzQ8SEVxaUEtZHc9L0VUbn5ZPihCIGcuaTJ6LlBLeFx0XCY8RThXdVdKejdBbltxYmF+MXs2S2ZWRmNda2M4LiJpKHlnX0JURGdUMV9JemtaVFBNP2QiMGBQeQ=alAuNlphJCpcTXNmPXw0bXtIUCREaCI2OHR+eyMqWz4wTlgndCshNGc9Nk5+SiNEcSw1bkxzN2EyQlxqR2c1WWwlUmdAYSJmMzlUPTNkcj8=</signerCertificate>
         <signerCertificateTrusted>true</signerCertificateTrusted>
         <file>
            <status>Signed</status>
            <file>
               <fileDigest>rH/J9Sjw+pUOhy3xOqvx9Gafofs=</fileDigest>
               <fileName>pdf-s0812.pdf</fileName>
               <fileId>111111</fileId>
               <fileURL>https://example.com/unisign/ui/445525/a27c0a62feef1315112085897ce407ad65f6b20e/signedDocument?download=true&amp;documentId=529093</fileURL>
            </file>
         </file>
         <signature>bSvtrGk1qU7F9yWuw+iIE8MBJrFO+Jcb/aW2ziRAzK136I0nABql8EKrjGkH6xJfLQBvNdqLeIBvuiAVKozcsMCjGWhGnuQ0alljwuQPW/rum+92sXwI0dzmgT+DYNQvByGnsw+Jl6Fu040zXDWe8L/4ifA1XHzM66wg62OurC+vMW5y8uidjo9Y8yc7k+kZFwFFfLwcolmTC/GL+pOyF2T7fjTycIhJMSDZH2ztMRKHiLTBwI1tGNl8J0CPtwtn7E+SmUfZr1rrO6w42ZBSSot6gKa6P95ISk9JJoLN+IbCIMEBovkztPjqGAr0D0PSlpZ9kZ5ZaiX3/fP/Uc5QPg==</signature>
      </ns3:MultiSignResultResponse>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

*Pasirašymas atšauktas*

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
   <SOAP-ENV:Header/>
   <SOAP-ENV:Body>
      <ns3:MultiSignResultResponse xmlns:ns3="http://www.registrucentras.lt/multisignservice">
         <status>Canceled</status>
         <file>
            <status>Canceled</status>
            <file>
               <fileId>111111</fileId>
            </file>
         </file>
         <signature>OTlfleO0EHAErM6rzCvv4+7UWqxDATzgfRRKIKNlw/pKfKViKGlccUTr9GE0s703XWLUwb/e5xQZ/eDH1o22nzbLt+9kMCE5FjHotI9GXua19j7f63KKApTNhHwbeE9avXq9IehMbggz7G3c9e4LS5WEg4wYbn4fBixSikOyPUd/229E+Qi6yEtd+gSsb1pZ0XG4yKCSgK1orn9e9+ZaswUnZvJRvGO2liTms9qfjWfynJem4QmByVfKQSpDAEkHVrc53aZZOn8/PT6RMN4K7tu/mbS5j/Zt1FsBHdeHbYJBqY4xSP3t/sNQeGCW+F7I14QRbwJWalZqyPkErfjTcQ==</signature>
      </ns3:MultiSignResultResponse>
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
	xmlns:ns1="http://www.registrucentras.lt/multisignservice">
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
