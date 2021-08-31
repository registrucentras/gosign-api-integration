---
layout: default
title: Signature elementas
parent: Webservisas
nav_order: 5
---

# Signature elementas

Elemento `signature` reikšmė **užtikrina užklausos duomenų teisingumą**. Iš pasirašomų elementų sekos sudaroma tekstinė pasirašomos informacijos reprezentacija. Ši **reprezentacija sudaroma eilės tvarka apjungiant visų užklausos XML elementų tekstines reprezentacijas**. Jei elementas yra praleistas web serviso užklausoje, jis taip pat yra praleidžiamas generuojant parašo reikšmę. Kiekvienas nepraleistas elementas reprezentuojamas tokiu formatu: „<pavadinimas>reikšmė</pavadinimas>" (čia „pavadinimas" yra elemento pavadinimas užklausos ar atsakymo XML struktūroje, „reikšmė" yra to elemento reikšmė). Kiekvieno elemento reikšmė generuojama pagal elemento tipą naudojantis tokiomis taisyklėmis:
- **boolean** tipo elemento **reikšmė verčiama į tekstinę vertę** „true" arba „false";
- **kompleksiniai tipai** (angl. complex type) **formuojami be pradžios ir užveriančiosios elemento** pavadinimo gairių (angl. tags);
- **tekstinė pasirašomos informacijos reprezentacija** (`signature` elementas) **konvertuojama į baitų** seką naudojant UTF-8 koduotę;
- **elementų eiliškumas pagal** onesign ir multisign **instrukcijoje pateiktus** elementų **aprašymus**;
- **baitų seka pasirašoma** kliento sistemoje kliento **privačiu raktu**. Pasirašymui naudojamas SHA1 ir RSA algoritmai;
- pasirašomo arba pasirašomų **dokumento baitai** (`content` elementas) **nėra naudojami parašui skaičiuoti** (failo turinys nėra įtraukiamas į parašą - `signature` elementas).

**PHP pavyzdys**

[Sugeneruojame privatų ir viešą raktą](key-generation.md) sau patogiu būdu.

turime privatų [RSA raktą](https://www.ssh.com/academy/ssh/keygen):

```bash
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,42BC45A9B8B8B912

hAUGg985zGxjzZZFT12n+YTesn/mydDaobeI+NW/t/j5xWLMB3R53BgfL4PcUonk
S48oTgsIMovyi3UHsWYSjd2IelD9p6yi13dhEiSiA+3z8tJ5D4mEmDuRJtujab0S
DgL5pszaceR0i0WBMRY0DF6rHiAwWe7MCPKpLyZJPEtDJIgpsIHhihS8wUSHOLqz
Lybc3pHxTmp8+WthfcKnR5TKtu/Gw5cHriM8Ch0cYskZPa8zSTF3Mn74kJgFqrQO
7p33z9yQMKNKtan/XdrxilsxloiRCYG5C90R7OOT5ALvFQGQgty8h5wXkTD5FuCl
ZR2FqQmXLx3un9h5X28IYPBe745uE2sHYsFgb3bESUHrhRHiwj9xiwQXAz6ZTOe0
dtcVIuxQlpTPXSQrjsDXiConKEcWxDbQ4kVYG0L3ITMdzE5rvfJNMuQgH+8ssygB
VkLhWtI/oVtVOT0eONorEZuEtkbYU+LSDAi/eYjjAjRFV9gRtBo+xI8KPdtkaOJj
mvbKLgBVwjUXy35CvaovVOPom+qHvKFKWdFTSHX0j2HbL/un72Nd6a5qzZaSf9dN
9n+DyxVVRhldHtxjm82+FnUovB3n2ljJF2qjt/X/36dZ68/FI2rNUOXODlU3hdf4
Z93JhrSq6nLVVQLplyZSXdyvg0gKd+51CcplVYuvaGF0axfffYgSsqgjK0PnA+8U
r/3+53Y9ThPkfcVcUaRLuQh2KO+serCJsdeHwTzEjs1LcmfARmZ3HaGiLCDwuhyg
eVbR/Pm9i2Jj4hLUhtIDn4StIOiTW9AIATXGStx+KEM=
-----END RSA PRIVATE KEY-----
```

turime viešą [RSA raktą](https://www.ssh.com/academy/ssh/keygen), kurį perduodame GoSign.lt administratoriams:

```bash
-----BEGIN PUBLIC KEY-----
MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCZTsMmpwWuWRLTCJblBziSElIp
u0DrwNkLu9aY11qtlIGsR0XLuOTKq2m89f8K9mMqf/i9XAmTmKz/Xx6aqAXUPuxL
1bwI7W08yfmxmwn9K0YB9pdayr39KKsjbX8WxZrAvUQkrQ5+be41MOfelA7K6IxI
qcl2HbJLeoYAWOZpLQIDAQAB
-----END PUBLIC KEY-----
```

programinėmis priemonėmis sugeneruojame masyvą:

```php
[
    'clientInfo' =>
    [
        'clientId' => 'client_id'
        'signerPersonalCode' => 'personal_code'
        'responseUrl' => 'http://example.com/app',
    ],
    'signingType' => 'Signature',
    'files' => 
    [
        [0] => 
        [
            'fileDigest' => '4paS4paSTXLilpLilpJLbc27MeKWkkvilpJeVzkw4paS'
            'fileName' => 'pdf.pdf'
            'fileId' => '111111'
            'fileURL' => 'https://example.com/pdf.pdf',
        ],
    ],
]
```

serializuojame masyvą, kurį ir pasirašysime:

```xml
<clientId>client_id</clientId><signerPersonalCode>signer_code</signerPersonalCode><responseUrl>http://example.com/app</responseUrl><signingType>Signature</signingType><fileDigest>0v5NcpPFHEttzbsxm0urXlc5MIE=</fileDigest><fileName>pdf.pdf</fileName><fileId>111111</fileId><fileURL>https://example.com/pdf.pdf</fileURL>
```
atkreipkite dėmesį:
- rezultate **nėra įtraukiami kompleksinio tipo elementai** kaip: `clientInfo`, `files` ir t.t..;
- kadangi suaugmui užtikrinti užtenka `fileDigest` rezultate **nėra įtraukiamas `file` `content` elementas**. 

pasirašome naudojant PHP:

```php
function sign(string $content, string $privateKey)
{
	$sign = '';
		
	$key = openssl_get_privatekey($privateKey);
		
	openssl_sign($content, $sign, $key);
		
	return $sign;
}

$privateKey = '-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,42BC45A9B8B8B912

hAUGg985zGxjzZZFT12n+YTesn/mydDaobeI+NW/t/j5xWLMB3R53BgfL4PcUonk
S48oTgsIMovyi3UHsWYSjd2IelD9p6yi13dhEiSiA+3z8tJ5D4mEmDuRJtujab0S
DgL5pszaceR0i0WBMRY0DF6rHiAwWe7MCPKpLyZJPEtDJIgpsIHhihS8wUSHOLqz
Lybc3pHxTmp8+WthfcKnR5TKtu/Gw5cHriM8Ch0cYskZPa8zSTF3Mn74kJgFqrQO
7p33z9yQMKNKtan/XdrxilsxloiRCYG5C90R7OOT5ALvFQGQgty8h5wXkTD5FuCl
ZR2FqQmXLx3un9h5X28IYPBe745uE2sHYsFgb3bESUHrhRHiwj9xiwQXAz6ZTOe0
dtcVIuxQlpTPXSQrjsDXiConKEcWxDbQ4kVYG0L3ITMdzE5rvfJNMuQgH+8ssygB
VkLhWtI/oVtVOT0eONorEZuEtkbYU+LSDAi/eYjjAjRFV9gRtBo+xI8KPdtkaOJj
mvbKLgBVwjUXy35CvaovVOPom+qHvKFKWdFTSHX0j2HbL/un72Nd6a5qzZaSf9dN
9n+DyxVVRhldHtxjm82+FnUovB3n2ljJF2qjt/X/36dZ68/FI2rNUOXODlU3hdf4
Z93JhrSq6nLVVQLplyZSXdyvg0gKd+51CcplVYuvaGF0axfffYgSsqgjK0PnA+8U
r/3+53Y9ThPkfcVcUaRLuQh2KO+serCJsdeHwTzEjs1LcmfARmZ3HaGiLCDwuhyg
eVbR/Pm9i2Jj4hLUhtIDn4StIOiTW9AIATXGStx+KEM=
-----END RSA PRIVATE KEY-----';

$content = '<clientId>client_id</clientId><signerPersonalCode>signer_code</signerPersonalCode><responseUrl>http://example.com/app</responseUrl><signingType>Signature</signingType><fileDigest>0v5NcpPFHEttzbsxm0urXlc5MIE=</fileDigest><fileName>pdf.pdf</fileName><fileId>111111</fileId><fileURL>https://example.com/pdf.pdf</fileURL>';

$signature = sign($content, $privateKey);

echo $signature;

echo base64_encode($signature);

```

gauname:

```bash
6Lտ▒M▒▒$a▒▒~▒▒▒▒▒X▒ơ▒⫧▒▒▒땱p8▒▒[▒▒F!3▒iK▒▒▒▒▒7~
                                                            G▒O▒|▒u▒Ё▒T|t+*▒▒+▒▒▒▒▒;▒E▒4H▒x▒▒~▒▒▒▒▒~1▒1▒T"▒▒mv▒j▒E
NkzVv5BN4o8apCRhi6J+kJXXBOfMWJDGocniq6eM4errlbFwOOrsW67iRh0hM6scaQ5Lzuf35/c3fgtHHeNPqXzHdQTRDtCBBfNUfHQrKt3gK6UEzfChBtfeO9tFiR80SJJ4trt+kcP197kR+6y8fjHCMcJUIt/NbXaFahf5qkU=
```

base64 encode reikšmė perduodama per `signature` užklausos elementą:

```xml
<signature>NkzVv5BN4o8apCRhi6J+kJXXBOfMWJDGocniq6eM4errlbFwOOrsW67iRh0hM6scaQ5Lzuf35/c3fgtHHeNPqXzHdQTRDtCBBfNUfHQrKt3gK6UEzfChBtfeO9tFiR80SJJ4trt+kcP197kR+6y8fjHCMcJUIt/NbXaFahf5qkU=</signature>
```
