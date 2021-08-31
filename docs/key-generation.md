---
layout: default
title: Raktų generavimas
parent: Webservisas
nav_order: 6
---

# Raktų generavimas

Raktų porą galite sugeneruoti:
- naudodami ["online" generavimo įrankius](https://www.google.lt/search?q=generate++rsa+keys+online);
- Linux arba Windows operacinės įrangos aplinkoje.

## Raktų generavimas Linux OS aplinkoje

Sugeneruoti raktus galite naudojant [OpenSSL](https://www.openssl.org/) programinę įrangą.

Generuojame privatų raktą:

```bash
$ openssl genrsa -out privatus_raktas.key 2048
Generating RSA private key, 2048 bit long modulus
...........................................................................................................................................+++
..............................+++
e is 65537 (0x10001)
```

Generuojame viešą raktą naudojant prieš tai sugeneruotą privatų raktą:

```bash
$ openssl rsa -in privatus_raktas.key -pubout -out viesas_raktas.pem
writing RSA key
```

rezultate turime du raktus, kurie atrodo taip:

privatus raktas, kuris bus **naudojamas kliento sistemoje pasirašyti siunčiamą turinį** ([`signature`](signature.md) elementas):
```bash
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAvMMlz6PVuKly4WKw9kGCzo33gMY2hLzokoZkQe/5jSOQtDzN
t3L2Bt1IJeQv5Ivlpjy+Sz0FZAgMb1T7m5nXDUN2ZwK2srXC4SzYTK0qD0i+AGfz
jeHbGUD2ETBQ+S9js5VpMX5Q957mAyeDWmbAv3UGQ6CAnAzYQLpoApxhmIgy0Ers
/RwtqKZBnFcyeGXqq7ft4HtWH1UnAclxTC7bYAHH+sKsqpIWvZFGDcct0zeUVnr7
KfTQmS3Za505SdEL45Kow2+GoLIdup2+IqPAhG0uqAEKwxs5/DMCH8U+dGspMll9
ltsQrDYw1UTkL6zLnCSw77+3Xu3XwY2vJZ3r9wIDAQABAoIBAH8JLsdBUbKHh6Mb
0lDI4gm1DZ8CxuoqYLNL8ulVYbOU/evvB9uwaNdR0R5/JaRAanuoYcEs/hXGPOgo
X3Tm4g4xGtxUvTQkk1UL4z4nRCkpIYYQb59LIzMpvvDufXBWblkL8tG2WzNrIw14
aDRM9udjEKYuvJ9JHbjiOuGW8S+/P+yxqdbLxHHrnd5yZXahtXUg7ub1iYXdkplZ
7WPDcByWtyKFNtMkyjKCyV46S0L/tffT2yCq/QCM04Lfac6CDD1MgVrQnTWNgf9J
SQjylvcUyCrhk2irwtaXR4voLPTCW/mVXBovlLxBsteC7DB7bIGB2sqfG/ZFeYHn
fbzM85ECgYEA9pFPR6PmzT/QhTe1edlM4aWzU5UwqLIBZWbJmBb8gwDurcrkAguf
/yJar4Jl2qqecYRG6EJQ2oP9oG13j2quasM3GZurx+vbZiJkDlx7HR6hTxJMc9rt
eMxda3+qL8aTTVGVLZcyhmrnPYkpk5kfBK+5kHF7zfkmXp9+G718E4UCgYEAw/u8
6ThY8AHYR4T1poGj9ssgVcI9i6lk7xri1bQwDx9LFGAooJzD+dK9+j47k8lhZgNg
qvLXhxYAgAm0JxeEon/+sygrzW+9y+qUWKnNNgZ1RHmvKqs3W42kH8uC18/22Vv7
dEcNRbtPZZA269qXxxqVPY7Rxkkn+Z5Ps+KnpEsCgYBaogs8UDkUlTJ25YVlpsSl
5RzHyn06ZUQyG9haeYiUNxGE/KFXRyKmy9/9x7bc6/6Vx4Ow+D90MzRVdieOpi46
vEtStHAuaroZDucsiD4Q9CNjR1ym8YB8+NIWI7VRHnMi5qwpN/ywgDdD3VlVEeHe
/SD+wVg63CyId6QJWltzaQKBgE49zil9qWQIGIRU/I1A7Gi2p24VYeTD99vNbAnN
KfKfl6XGzfFxJHw0OkwRVE+n8g58Are8w3bWvdRgC2Af73/AgbqcqwAVQDyMpjTP
dXHAGkkAG4J5YFxYq9FVuiLWj8IvCrBdPVs9cHEnpgV6+2Uto68zuWPkCOWK4l76
edsJAoGBAO0BCo0AV/c5l2jbSNcxEjS9c4fDQfjiClne6Od8ZTTS3l7Xpb9IOx5x
P7l21MuLOh+BDZ+vuHYYOQ6Og7oKM9TI+7+ez72qSZgRHHB6jyIRmQtFIxciEt/H
E7Pq5AcIxX/giE5R/QgehZifRhUsEd8Dz3SheNtNehc7v5K0MFdd
-----END RSA PRIVATE KEY-----
```
viešas raktas, kurį **reikės perduoti GoSign.lt administracijai**:
```bash
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvMMlz6PVuKly4WKw9kGC
zo33gMY2hLzokoZkQe/5jSOQtDzNt3L2Bt1IJeQv5Ivlpjy+Sz0FZAgMb1T7m5nX
DUN2ZwK2srXC4SzYTK0qD0i+AGfzjeHbGUD2ETBQ+S9js5VpMX5Q957mAyeDWmbA
v3UGQ6CAnAzYQLpoApxhmIgy0Ers/RwtqKZBnFcyeGXqq7ft4HtWH1UnAclxTC7b
YAHH+sKsqpIWvZFGDcct0zeUVnr7KfTQmS3Za505SdEL45Kow2+GoLIdup2+IqPA
hG0uqAEKwxs5/DMCH8U+dGspMll9ltsQrDYw1UTkL6zLnCSw77+3Xu3XwY2vJZ3r
9wIDAQAB
-----END PUBLIC KEY-----
```


## Raktų generavimas Windows OS aplinkoje

Sugeneruoti raktus galite naudojant [OpenSSL](https://slproweb.com/products/Win32OpenSSL.html) programinę įrangą.

Generuojame privatų raktą:

```bash
$ openssl genrsa -out C:\Users\user\Documents\privatus_raktas.key 2048
openssl genrsa -out C:\Users\user\Documents\privatus_raktas.key 2048
Generating RSA private key, 2048 bit long modulus (2 primes)
..........................................+++++
.........+++++
e is 65537 (0x010001)
```

Generuojame viešą raktą naudojant prieš tai sugeneruotą privatų raktą:

```bash
$ openssl rsa -in C:\Users\user\Documents\privatus_raktas.key -pubout -out C:\Users\user\Documents\viesas_raktas.pem
writing RSA key
```

rezultate turime du raktus, kurie atrodo taip:

privatus raktas, kuris bus **naudojamas kliento sistemoje pasirašyti siunčiamą turinį** ([`signature`](signature.md) elementas):
```bash
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEA2PqeJNCrDWTNI9EPicc6X0unhQ9Gsd/ndWxApCBy9Kah7fr2
YLnTwOaLPBTVLvSChHXL67SDD7SBgiAWmy/6Alzcm3Y2CK1040ehdYL4ybll21W6
u+OjnuKy8+WNymZkhcrP1CAM1jLTe/eXXzi1qLqJ5QUsiKrk0GHZ6kOyWP8cmc3C
mfFs4XXCxm0IWTTg9gFtC7l++GTXl5b+R97kqkWc+QZgNwaY3mXtI8pIlpSlCmfS
uAfTTflYtcRhn/UOaqSlZyTDroDDZMDZsTZzYNcPKonaQk5h8mfZVf/nuYpTVak1
ym96BwBL3KSeC7bKWuXgwjad3/t5yKkn9un6swIDAQABAoIBAQCEo9gPC1y1qFxb
O87y286MONRkW+1MiWKV/qIZcxizBDZTI6p/gLm433ZYOSgFN0WeMeCB62x/KkpN
QM5w+cgsr5XQl1f6wAaHdd921aS+tE5W4bZwa303gMACpt6hzyw+ObgIpbsTKijs
THPFqwYp6janwRzzQvzzgg3TzqR1EAYW6T0elVdVOfds84Mo5TeRl/aXUCk7P/Jx
lx8RrRLMV58BHnkf/Ryo0Mu+AU4jjuNDXiTWpZdvXQUdjYXmkTZK2RKQrJd21pH3
D5cQhBsa6sXCdY1wtXwXHDDNVi/KyH/bKQb8WfMYiQi4AG490CxFr+xXzz5fzXp4
bhwCMlCZAoGBAPx7zho809nfeDYotIX1owsfPartV8k99OwYv4wOdRuEZRPgYuDE
1xfH6EhnhenVFjW5/o69COUxvAs32Yp6spC1PlnnQoaqjjM13LO0ePnv5fwD6oIy
wABDRi08sUNnZATSyV0TfCAc7TLmWFsQ2WZn7xIUbjVRCwadIJKz7L7/AoGBANwA
OdGUd/Xehj0WZSOeANNV1OAwHYaZkPORpMAoP7sEZCJQ12u/0l+I5lHigQ+Sl3I4
SwBh9eNJOmRorfRG1Ox4qtSq4xMU+FdKHx7MQ9aF8jogxGtGDM7INdH+g6bnElWE
SzCtFBwM35SJbn2wabtnEwmyr8Li3ZkLNaON03hNAoGAZDVTENDRmGh8UqqHM5/R
bUmh9SQsMmAXxFjyNUlLq3c5ktD9DY6ye+rIw2vrF2qOXRaL9OUMEcNSifVJrw+R
raNxssb5fW9V7vdSuDRJy5Eua362ZaR01eXdhXjQNhtj9BIg/4MLQceZURlhOguO
7XkUxs07DIg04xQb6H3m2csCgYBKSAk9qlOWwLuyhI1BqWe9840c5SITAGbg1pw6
BVz/WEw3CfSyfOIbP64El+XbzDM2batlRa4wP9lnbbDOedwKu/NyOwDQwJPZZT18
uJtvI2rWFZo8XjqU1yTU8oqhIAQgu7pnhQj1L6OOE5kq9xW8IOFctOiDdPY9ZnuF
7a0pQQKBgB8QBddym67POmneNSI79e5SgQ9ujuuxTiKAWIaG6RIJ1oR9fNkMq/Q7
UILcdha/0TGP5gFXV5eJEuJIqoNwWRQv2Le+8NopGcuSOCY4JQaY187Jlwe4sxiU
gphzYu93fM267hQTG1SZFDvGQG7FJ3lb6c1PlIJaUMRMfWze4z+z
-----END RSA PRIVATE KEY-----
```
viešas raktas, kurį **reikės perduoti GoSign.lt administracijai**:
```bash
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA2PqeJNCrDWTNI9EPicc6
X0unhQ9Gsd/ndWxApCBy9Kah7fr2YLnTwOaLPBTVLvSChHXL67SDD7SBgiAWmy/6
Alzcm3Y2CK1040ehdYL4ybll21W6u+OjnuKy8+WNymZkhcrP1CAM1jLTe/eXXzi1
qLqJ5QUsiKrk0GHZ6kOyWP8cmc3CmfFs4XXCxm0IWTTg9gFtC7l++GTXl5b+R97k
qkWc+QZgNwaY3mXtI8pIlpSlCmfSuAfTTflYtcRhn/UOaqSlZyTDroDDZMDZsTZz
YNcPKonaQk5h8mfZVf/nuYpTVak1ym96BwBL3KSeC7bKWuXgwjad3/t5yKkn9un6
swIDAQAB
-----END PUBLIC KEY-----
```

**Užtikrinti paslaugos saugumą, Prod ir Test aplinkoms sugeneruokite po atskirą raktų porą.**
