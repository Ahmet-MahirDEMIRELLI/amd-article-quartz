---
title: JWT CTF
tags:
  - skydays25
  - easy
---

#### Author: AMD

Repository: [SKYDAYS25 JWT CTF](https://github.com/skylab-kulubu/SKYDAYS-CTF-25/tree/main/web/jwt-amd)

-----------------------------------------------------------------------------------
<b>Ana Sayfa'ya git</b>

```link
http://dizci.ctf
```

-----------------------------------------------------------------------------------
<b>Giriş bilgilerini al ve login sayfasına git</b>

```link
http://dizci.ctf/login
```

-----------------------------------------------------------------------------------
<b>Burp Suite ile dinlemeye başla</b>

-----------------------------------------------------------------------------------
<b>Sağlanan bilgiler ile giriş yap ve token'ı yakala</b>

```json
   {
     "token": "...",
     "homework": "..."
   }
```

-----------------------------------------------------------------------------------
<b>Token'ı çöz</b>

```text
https://jwt.io sitesini kullanarak token içeriğine bak. (HS256)
```

```json
   {
     "nameid": "...",
     "role": "...",
     "nbf": "...",
     "exp": "...",
     "iat": "...",
   }
```
-----------------------------------------------------------------------------------
<b>Rol alanını https://gchq.github.io/CyberChef/ ile çöz</b>

```text
sonuç  -->  student (FROM Base64)
```

-----------------------------------------------------------------------------------
<b>To Base64 seçerek 'teacher' şifrele</b>

```text
sonuç  -->  dGVhY2hlcg==
```

-----------------------------------------------------------------------------------
<b>Teacher karşılığını jwt.io'da rol kısmına yapıştır ve Encoded alanı kopyala</b>

-----------------------------------------------------------------------------------
<b>Burp Suite'de token'ı güncelle ve paketin geçmesine izin ver</b>

-----------------------------------------------------------------------------------
<b>Yönlendirilen http://dizci.ctf/students sayfasını incele</b>

```text
Öğrencileri tek tek gezerek 100 alan öğrenciyi bul ve cevabını kopyala.
```

-----------------------------------------------------------------------------------
<b>Flag'i al</b>

```text
Sağlanan bilgiler ile giriş yap.

Kopyaladığın cevabı yapıştır ve ödevi kaydet.

Flag window.alert ile gelecek.
```
