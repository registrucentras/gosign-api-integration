---
layout: default
title: Atsakymo verifikavimas
parent: Webservisas
nav_order: 7
---

# Atsakymo verifikavimas

API **atsakymai turi būti patikrinti** naudojant GoSign.lt administracijos suteiktu viešu raktu. Taip būsite tikri, kad gražintas API atsakymas nebuvo pakeistas duomenų perdavimo metu. Atsakymo tikrinimas yra vykdomas tokiais pat principais kaip [`signature`](signature.md) užklausos elemento sudarymas. Iš atsakyme esančių elementų sekos sudaroma tekstinė informacijos reprezentacija. Ši **reprezentacija sudaroma eilės tvarka apjungiant visų atsakymo XML elementų tekstines reprezentacijas**. Jei elementas yra praleistas web serviso atsakyme, jis taip pat yra praleidžiamas generuojant parašo reikšmę. Kiekvienas nepraleistas elementas reprezentuojamas tokiu formatu: „<pavadinimas>reikšmė</pavadinimas>" (čia „pavadinimas" yra elemento pavadinimas atsakymo XML struktūroje, „reikšmė" yra to elemento reikšmė). Kiekvieno elemento reikšmė generuojama pagal elemento tipą naudojantis tokiomis taisyklėmis:
- **kompleksiniai tipai** (angl. complex type) **formuojami be pradžios ir užveriančiosios elemento** pavadinimo gairių (angl. tags);
- **dokumento turinys** (`content` elementas) **nėra naudojamas atsakymui patikrinti**.

Turime viešą raktą, kurį **perdavė GoSign.lt administracija**:
```bash
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA9l+OCK6T9jnn4e/kNQVP
oyePlH+GhK6Ik1gEW+OH71gpdXbjWatw9pBE2eeSOovQkSmjlsbh5WzSzkr5ywK/
BEoco+ns6fugIwjypAmRc2JJo1CUW2GGQnEF+ociysjNbpGLDqweawL+UzK+JL+M
nxOyRBw9JJZDj+fQvErpVk0mec3TUTEUlbjJd5WbwbtRXE4DooZFVgccbamg10la
0E4b9/DgSUJXRliOw5Cn1rXC/nn9aYSjsx89DeTRHZmKqPWCsmI6k+WTzhh/Kpdw
5xfPcgj3T7Hav0xXBTM2QR3XSyGg/EfYqvsV2FLTxDaDvVEBMc1pMX00ihsBTgoh
dQIDAQAB
-----END PUBLIC KEY-----
```

**Gavome atsakymą** iš API:

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

**Konvertuojame atsakymą** į tekstinę reprezentaciją:

```xml
<transactionId>445190</transactionId><signingUrl>https://example.com/unisign/ui/445190/282702111d7cced39fd1cbb699d6c1438646a109/main?locale=lt</signingUrl>
```

Eksportuojame [`signature`](signature.md) elementą:

```xml
IdUcXIAb4lafwo2GQWMX+t+tYQ5IeqgXX5pCzo+Lml4cDj9GA3WJp0V1Y1TfMF8CyjgWMhkkdfqBD76xP7lW1jWGD4SnokeZv75Y5BPZcE73qLJ56ynXSq+6fO9NZLpP4TvwZTn9FxyKnvZPLvUj5ZyH3sk0nEmz58w1R3FK/q/SHxC1m4p6nrZd8zLEdw9IJYvowcZicVTmqTJLjqp1CyPU00D5kA4xl1oAWfoO7yb3kmCYshTQnJqSjllFql5FZB4Jh1u61UFMJ+3QuwSRrz/ad5Jq9bVp9TfxeHx7N/p/V0yiFMxNjx0WJaoB52CdeOGxDj+44fs2nKiw4u9xaw==
```

Dekoduojame ([base64](https://en.wikipedia.org/wiki/Base64)) [`signature`](signature.md) elementą:

```bash
!\VAc߭aHz_BΏ^?FuEucT0_82$u?V5GXpNy)J|MdO;e9O.#圇4I5GqJz]2wH%bqT2Ku#@1ZY&`МYE^Ed	[AL'л?wji7x|{7WLM%`x?6qk
```

**Patikriname turinį** naudodami parašo tikrinimo [funkciją](https://www.google.com/search?q=openssl+verify+by+public+key).

**PHP pavyzdys**

```php
function validate(string $publicKey, string $content, string $signature): void {
		
	$publicKeyId = openssl_pkey_get_public($publicKey);

	$ok = openssl_verify($content, $signature, $publicKeyId);
		
	openssl_free_key($publicKeyId);
		
	if ($ok === 0) {
		return false;
	}
	else {
		return true;
	}
}

$publicKey = '-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvMMlz6PVuKly4WKw9kGC
zo33gMY2hLzokoZkQe/5jSOQtDzNt3L2Bt1IJeQv5Ivlpjy+Sz0FZAgMb1T7m5nX
DUN2ZwK2srXC4SzYTK0qD0i+AGfzjeHbGUD2ETBQ+S9js5VpMX5Q957mAyeDWmbA
v3UGQ6CAnAzYQLpoApxhmIgy0Ers/RwtqKZBnFcyeGXqq7ft4HtWH1UnAclxTC7b
YAHH+sKsqpIWvZFGDcct0zeUVnr7KfTQmS3Za505SdEL45Kow2+GoLIdup2+IqPA
hG0uqAEKwxs5/DMCH8U+dGspMll9ltsQrDYw1UTkL6zLnCSw77+3Xu3XwY2vJZ3r
9wIDAQAB
-----END PUBLIC KEY-----';

$content = '<transactionId>445190</transactionId><signingUrl>https://example.com/unisign/ui/445190/282702111d7cced39fd1cbb699d6c1438646a109/main?locale=lt</signingUrl>';

$signature = base64_decode('IdUcXIAb4lafwo2GQWMX+t+tYQ5IeqgXX5pCzo+Lml4cDj9GA3WJp0V1Y1TfMF8CyjgWMhkkdfqBD76xP7lW1jWGD4SnokeZv75Y5BPZcE73qLJ56ynXSq+6fO9NZLpP4TvwZTn9FxyKnvZPLvUj5ZyH3sk0nEmz58w1R3FK/q/SHxC1m4p6nrZd8zLEdw9IJYvowcZicVTmqTJLjqp1CyPU00D5kA4xl1oAWfoO7yb3kmCYshTQnJqSjllFql5FZB4Jh1u61UFMJ+3QuwSRrz/ad5Jq9bVp9TfxeHx7N/p/V0yiFMxNjx0WJaoB52CdeOGxDj+44fs2nKiw4u9xaw==');

if(validate($publicKey, $content, $signature)){
	echo 'Atsakymas tinkamas naudoti';
}
else{
	echo 'Atsakymas nėra tinkamas naudoti';
}
```
