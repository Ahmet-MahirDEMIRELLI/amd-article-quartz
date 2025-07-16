---
title: Whispr Backend
tags:
  - NET
  - curve25519
---


<div style="text-align:center;">
  <img src="static/whispr/whispr.png" width="150" />
</div>

-----------------------------------------------------------------------------------
<b>PROJENİN AMACI</b>

Projenin amacı, kullanıcıların birbirleriyle uçtan uca şifreli ve anonim bir şekilde güvenli olarak mesajlaşabilmesini sağlamaktır.

-----------------------------------------------------------------------------------
<b>KULLANILAN TEKNOLOJİLER</b>

```text
Backend  -->  .NET 9
Database -->  MSSQL
```

-----------------------------------------------------------------------------------
<b>TEMEL ÖZELLİKLER</b>

<ul>
  <li>Kullanıcılar, cihazlarında anahtar çiftlerini oluşturur ve yalnızca <code>nickname</code> ile <code>public key</code> bilgilerini sunucuya göndererek kayıt olur.</li>
  <li>Bir kullanıcı, iletişim kurmak istediği kişinin <code>nickname</code> bilgisiyle sunucudan onun <code>public key</code> verilerini talep eder.</li>
  <li>Uygulama belirli aralıklarla veritabanını kontrol ederek kullanıcıya gelen mesaj olup olmadığını sorgular.</li>
</ul>

-----------------------------------------------------------------------------------
<b>GÜVENLİK NASIL SAĞLANDI</b>

<ul>
  <li>Anahtarlar yalnızca kullanıcı cihazında oluşturulur; sunucuya sadece <code>public</code> anahtarlar gönderilir.</li>
  <li>Gönderilecek mesaj, alıcının public key’i ve gönderenin key pair bilgileri kullanılarak Curve25519 (X25519) algoritmasıyla bir ortak gizli anahtar (shared secret) oluşturulur; ardından bu gizli anahtar kullanılarak mesaj AES-GCM algoritmasıyla şifrelenir.
  <br/>
  <b>Bu sayede yalnızca alıcı, kendi <code>private key</code>’i ve gönderenin <code>public key</code>’i ile aynı shared secret’ı hesaplayabilir ve mesajı çözebilir.</b></li>
  <li>Mesaj içeriği, gönderen ve alıcı bilgileri; gönderenin <code>private Ed25519</code> anahtarı ile imzalanır.</li>
  <li>İmza, mesaj veritabanına kaydedilmeden önce sunucu tarafında doğrulanır.</li>
  <li>Gönderilen mesajlar kullanıcı tarafında saklanmaz; böylece veritabanında ya da herhangi bir istemci cihazda mesajlar düz metin (<i>plain text</i>) olarak tutulmaz.</li>
</ul>
