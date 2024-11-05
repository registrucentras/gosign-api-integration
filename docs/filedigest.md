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

**Java pavyzdys**

Šis pavyzdys demonstruoja, kaip nuskaityti failą iš vietinio disko, gauti jo turinį Base64 formatu, apskaičiuoti failo digest (hash) ir atspausdinti dvejetainę bei Base64 digest reprezentacijas.

## Kodo Pavyzdys

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Base64;

public class LocalFileProcessor {

    public static void main(String[] args) {
        // Kelias į vietinį tekstinį failą
        String filePath = "C:\\"; // Pakeiskite į faktinį failo kelią

        try {
            byte[] fileContent = getLocalFileContent(filePath);

            // Base64 koduojame failo turinį
            String base64EncodedContent = Base64.getEncoder().encodeToString(fileContent);
            System.out.println("Failo turinys (Base64 formatu): ");
            System.out.println(base64EncodedContent);

            // Digest apskaičiavimas
            byte[] fileDigest = encrypt(fileContent);

            // Išvedame dvejetainę digest reprezentaciją
            System.out.print("binary: ");
            for (byte b : fileDigest) {
                System.out.print((char) b);
            }
            System.out.println();

            // Išvedame Base64 koduotą digest
            String base64EncodedDigest = Base64.getEncoder().encodeToString(fileDigest);
            System.out.println("binary + Base64 kodavimas: " + base64EncodedDigest);

        } catch (IOException | NoSuchAlgorithmException e) {
            e.printStackTrace();
        }
    }

    private static byte[] getLocalFileContent(String filePath) throws IOException {
        Path path = Paths.get(filePath);
        return Files.readAllBytes(path);
    }

    private static byte[] encrypt(byte[] content) throws NoSuchAlgorithmException {
        MessageDigest digest = MessageDigest.getInstance("SHA-1");
        return digest.digest(content);
    }
}
```

**Sugeneruoto `fileDigest` ir `fileContent` pavyzdys**

```bash
File Content:
JVBERi0xLjQKJcOkwgxlEIFsgPEY3RDc3QjNEMjJCOUY5MjgyOUQ0OUZGNUQ3OEI4RjI4Pgo8RjdENzdCM0QyMkI5RjkyODI5RDQ5RkY1RDc4QjhGMjg+IF0KPj4Kc3RhcnR4cmVmCjEyNzg3CiUlRU9GCg==
binary: ﾐ￿ￒ5￘"ﾘﾂﾲxￅￃﾚ￬6
binary + Base64 kodavimas:: kP/SNZAI2CKYgh0Wshd4xcOa7DY=
```

## Paaiškinimas

1. Failo Skaitymas: Funkcija `getLocalFileContent` naudoja `Files.readAllBytes` metodą, kad nuskaitomą failą konvertuotų į baitų masyvą.
2. **Base64 Kodavimas**: Failo turinys koduojamas į Base64 formatą, kad galėtų būti lengvai išvedamas ar saugomas tekstiniame formate.
3. **Digest (Hash) Apskaičiavimas**: `encrypt` metodas naudoja `MessageDigest` su SHA-1 algoritmu digest apskaičiavimui.
4. **Dvejetainės ir Base64 digest Išvedimas**: Programa išveda maišos dvejetainę ir Base64 formatu užkoduotą reprezentaciją.

**Pastaba**: Prieš paleidžiant kodą įsitikinkite, kad nurodytas failo kelias yra teisingas.
