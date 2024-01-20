### nmap
```python
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 f0:14:2f:d6:f6:76:8c:58:9a:8e:84:6a:b1:fb:b9:9f (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDafqZxGEa6kz/5SjDuHy4Hs02Ns+hQgiygUqck+jWWnO7A8+mzFovIR0z76dugf8sTv9P6hq++1nNkPvvdovIkCQ00Ci9VrNyRePh9ZjXUf6ohbRLa9bJ45zHSY3Icf56IeuIy3TVn6d05ed5EtaTjAYA8KyCvwm2nDZQ7DH081jnL1g1uJ4aeeA/IlNXYLV610u7lkQem8VwXQWEs7F8JH6RMX8/8oGe7oBnpvKUeACtB9NXN/5tGsiMXqx7+JB8nfpMRIyLiXA7HjV9S7mmtmBduJ5EyfvX5hdwSCEYF1E7/YowqF5KbTpmZeDI9vJharuKqB97iu1h87u1qc37zT7emxD0QxCOAT3mKGXB26u159ZjAvjJ2EUhSjfbgjTx0s0w2bysXJNrpw5oS1AMm/XD6dSCRfg0kS2LzwDFJvv3dCy56bdOdW+Xe/tkBgvNio11OiP8E2qvdZ+cSgnXi+d8m2TkFUJEfavQPES7iXuZ3gMEaVPdbILVz3zRGh58=
|   256 8a:52:f1:d6:ea:6d:18:b2:6f:26:ca:89:87:c9:49:6d (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBASqbHaEEuWmI5CrkNyO/jnEdfqh2rz9z2bGFBDGoHjs5kyxBKyXoDSq/WBp7fdyvo1tzZdZfJ06LAk5br00eTg=
|   256 4b:0d:62:2a:79:5c:a0:7b:c4:f4:6c:76:3c:22:7f:f9 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDDy2RWM3VB9ZBVO+OjouqVM+inQcilcbI0eM3GAjnoC
3306/tcp open  mysql   syn-ack ttl 62 MySQL 5.7.40
| mysql-info: 
|   Protocol: 10
|   Version: 5.7.40
|   Thread ID: 19
|   Capabilities flags: 65535
|   Some Capabilities: Support41Auth, Speaks41ProtocolOld, SupportsLoadDataLocal, ConnectWithDatabase, DontAllowDatabaseTableColumn, SwitchToSSLAfterHandshake, ODBCClient, SupportsCompression, LongPassword, Speaks41ProtocolNew, IgnoreSigpipes, SupportsTransactions, InteractiveClient, FoundRows, IgnoreSpaceBeforeParenthesis, LongColumnFlag, SupportsMultipleStatments, SupportsAuthPlugins, SupportsMultipleResults
|   Status: Autocommit
|   Salt: J&e\x0C2\x15`6L5"Y\x16ZN)9nI+
|_  Auth Plugin Name: mysql_native_password
| ssl-cert: Subject: commonName=MySQL_Server_5.7.40_Auto_Generated_Server_Certificate
| Issuer: commonName=MySQL_Server_5.7.40_Auto_Generated_CA_Certificate
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-12-22T10:04:49
| Not valid after:  2032-12-19T10:04:49
| MD5:   c512:bd8c:75b6:afa8:fde3:bc14:0f3e:7764
| SHA-1: 8f11:0b77:1387:0438:fc69:658a:eb43:1671:715c:d421
| -----BEGIN CERTIFICATE-----
| MIIDBzCCAe+gAwIBAgIBAjANBgkqhkiG9w0BAQsFADA8MTowOAYDVQQDDDFNeVNR
| TF9TZXJ2ZXJfNS43LjQwX0F1dG9fR2VuZXJhdGVkX0NBX0NlcnRpZmljYXRlMB4X
| DTIyMTIyMjEwMDQ0OVoXDTMyMTIxOTEwMDQ0OVowQDE+MDwGA1UEAww1TXlTUUxf
| U2VydmVyXzUuNy40MF9BdXRvX0dlbmVyYXRlZF9TZXJ2ZXJfQ2VydGlmaWNhdGUw
| ggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC8KqoE91ydQZJDUqWE/nfs
| 6akfHB2g3D1VJoX+DeuTxEubjmWy+jGOepvEbKEhjrLMl9+LIj3vkKlj1bpRw0x1
| 7tbY7NXPtz5EsOCqDcuGl8XjIBE6ck+4yK8jmzgCMOHhJjoAtcsgAOcnal0WCCyB
| 7IS4uvHi7RSHKPrcAf9wgL5sUZylaH1HWiPXDd0141fVVpAtkkdjOUCPwZtF5MKC
| W6gOfgxMsvYoqY0dEHW2LAh+gw10nZsJ/xm9P0s4uWLKrYmHRuub+CC2U5fs5eOk
| mjIk8ypRfP5mdUK3yLWkGwGbq1D0W90DzmHhjhPm96uEOvaomvIK9cHzmtZHRe1r
| AgMBAAGjEDAOMAwGA1UdEwEB/wQCMAAwDQYJKoZIhvcNAQELBQADggEBAGkpBg5j
| bdmgMd30Enh8u8/Z7L4N6IalbBCzYhSkaAGrWYh42FhFkd9aAsnbawK+lWWEsMlY
| +arjrwD0TE6XzwvfdYsVwOdARPAwm4Xe3odcisBvySAeOE6laaCnIWnpH/OqGDEk
| GBYfI8+e0CBdjhDNpeWVJEkGv4tzaf6KE1Ix9N2tTF/qCZtmHoOyXQQ7YwBPMRLu
| WnmAdmtDYqVEcuHj106v40QvUMKeFgpFH37M+Lat8y3Nn+11BP5QzRLh+GFuQmVc
| XaDxVdWXCUMWsbaPNNS+NM9FT7WNkH7xTy2NuBdSFvl88tXNZpnz8nkRxXLarLD8
| 2AE6mQqpFHhaSRg=
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
5000/tcp open  http    syn-ack ttl 62 Docker Registry (API: 2.0)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Site doesn't have a title.
8080/tcp open  http    syn-ack ttl 62 Node.js (Express middleware)
|_http-open-proxy: Proxy might be redirecting requests
|_http-title: Login
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```
### Ports: 22/ssh - 3306/msql -5000/http - 8080/http

