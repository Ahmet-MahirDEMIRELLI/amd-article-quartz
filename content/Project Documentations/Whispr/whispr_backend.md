---
title: Whispr Backend
tags:
  - NET
  - curve25519
---

<div align="right">
  <a href="#tr">🇹🇷 Türkçe</a> | <a href="#en">English</a>
</div>

---

## <span id="tr">🇹🇷 Türkçe</span>

<div style="text-align:center;">
  <img src="static/whispr/whispr.webp" width="150" />
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

---

## <span id="en">English</span>

<div style="text-align:center;">
  <img src="static/whispr/whispr.webp" width="150" />
</div>

-----------------------------------------------------------------------------------
<b>PROJECT PURPOSE</b>

The goal of the project is to enable users to communicate securely with each other using end-to-end encryption and full anonymity.

-----------------------------------------------------------------------------------
<b>TECHNOLOGIES USED</b>

```text
Backend  -->  .NET 9  
Database -->  MSSQL
```

-----------------------------------------------------------------------------------
<b>CORE FEATURES</b>

<ul>
  <li>Users generate key pairs on their own devices and register by only sending their <code>nickname</code> and <code>public key</code> to the server.</li>
  <li>A user who wants to communicate with another requests that user’s <code>public key</code> from the server using their <code>nickname</code>.</li>
  <li>The application checks the database at intervals to see if any new messages have arrived for the user.</li>
</ul>

-----------------------------------------------------------------------------------
<b>HOW SECURITY IS ENSURED</b>

<ul>
  <li>Keys are generated only on the user's device; only <code>public</code> keys are sent to the server.</li>
  <li>To send a message, a shared secret is generated using the recipient’s public key and the sender’s key pair via the Curve25519 (X25519) algorithm. This shared secret is then used to encrypt the message with the AES-GCM algorithm.
  <br/>
  <b>This ensures that only the recipient can decrypt the message using their <code>private key</code> and the sender’s <code>public key</code>, as both parties derive the same shared secret.</b></li>
  <li>The message content and metadata (sender and recipient info) are digitally signed using the sender’s <code>private Ed25519</code> key.</li>
  <li>The server verifies the signature before storing the message in the database.</li>
  <li>Sent messages are not stored on the client side, so neither the database nor any client device stores messages in plain text.</li>
</ul>
