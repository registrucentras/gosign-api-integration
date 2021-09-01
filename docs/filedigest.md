---
layout: default
title: fileDigest elementas
parent: Webservisas
nav_order: 3
---

# fileDigest sudarymas

`fileDigest` - pasirašymui skirto **dokumento `SHA1` santrauka**, ši santrauka **generuojama panaudojant duomenų maišos** (angl. hash value) **funkcijas** webservisui perduodant duomenis "raw binary" formatu. `fileDigest` užtikrina perduotų failų vientisumą.

**PHP pavyzdys**

```php
function getRemoteFileContent(): string
{
	return file_get_contents('https://example.com/pdf.pdf');
}

function encrypt(string $content): string
{
	return hash('sha1', $content, true);
}

$fileDigest = encrypt(getRemoteFileContent());

echo 'binary: ' . $fileDigest;
echo 'binary + base64encode: ' . base64_encode($fileDigest);
```

**Sugeneruoto `fileDigest` pavyzdys**

```bash
binary: MrKmͻ1K^W90
binary + base64encode: 0v5NcpPFHEttzbsxm0urXlc5MIE=
```

