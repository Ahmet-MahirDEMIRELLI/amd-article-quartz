---
title: PKS Backend
tags:
  - net10
  - microservices
  - mediatr
  - clean-architecture
  - ddd
---

<div align="right">
  <a href="#tr">🇹🇷 Türkçe</a> | <a href="#en">English</a>
</div>

---

## <span id="tr">🇹🇷 Türkçe</span>

-----------------------------------------------------------------------------------
<b>Teknolojik Altyapı ve Tercih Nedenleri</b>

Projenin backend mimarisi, yüksek ölçeklenebilirlik, esneklik ve gevşek bağlılık (loose coupling) ilkelerini sağlamak amacıyla modern teknolojiler ve desenler üzerine kurulmuştur:

* **.NET 10:** En güncel performans optimizasyonları, minimal API geliştirmeleri ve güncellenmiş bağımlılık enjeksiyonu (DI) yetenekleri için tercih edilmiştir.
* **CQRS & MediatR:** Okuma (Query) ve yazma (Command) operasyonları ayrıştırılarak kodun okunabilirliği ve performans yönetimi artırılmıştır. Kompleks iş mantığı MediatR pipeline'ları ile izole edilmiştir.
* **Clean Architecture & DDD:** İş kuralları (Domain) teknolojik bağımlılıklardan (EF Core, PostgreSQL vb.) tamamen soyutlanmıştır. Zengin domain modelleri (Rich Domain Models) ve validasyon kuralları merkezileştirilmiştir.

-----------------------------------------------------------------------------------
<b>Mikroservis Sınırları ve Veri Yönetimi (Bounded Contexts)</b>

Sistem, iş süreçlerine göre mantıksal olarak aşağıdaki bağımsız servislerden oluşmaktadır:

1.  **PKS.Users (Kullanıcı Yönetim Servisi):** Sistemdeki tüm aktörlerin (Öğrenci, Asistan, Danışman, Admin) profil bilgilerini, rol tanımlamalarını ve sisteme kayıt süreçlerini yöneten temel servistir.
2.  **PKS.Terms (Dönem ve Süreç Yönetim Servisi):** Sistemen ana iş mantığını barındırır. Akademik dönemlerin oluşturulması, bitirme/ara proje gruplarının kurulması, öğrencilerin projelerle eşleştirilmesi, rapor yükleme süreçleri ve akademik takvime bağlı teslim tarihleri bu servis tarafından yönetilir.
3.  **PKS.IdentityVerification (Kimlik Doğrulama & Güvenlik Servisi):** Kullanıcının şifre sıfırlama veya kritik işlemlerde iki aşamalı doğrulama için gerekli olan tek kullanımlık doğrulama kodlarının (OTP) üretilmesi, geçerlilik sürelerinin takibi ve doğrulanmasından sorumludur.
4.  **PKS.Email (E-Posta Servisi):** Sistem genelinde tetiklenen tüm e-posta bildirimlerini (Hoş geldin mesajı, Acil durum uyarısı vb.) dış dünyaya iletmekle görevli izole bir altyapı servisidir.

> *Not: Veri tutarlılığı ve bağımsız canlıya alınabilirlik (independent deployability) için her mikroservis kendi **Database-per-Service** yaklaşımıyla tasarlanmıştır.*

-----------------------------------------------------------------------------------
<b>Trafik Yönetimi ve Güvenli Servisler Arası İletişim (Gateway)</b>

Sistemin kalbinde yer alan **PKS.Gateway**, hem dış dünyadan gelen istekleri karşılar hem de servislerin birbiriyle olan güvenli iletişimini koordine eder:

*   **Client-to-Service İletişimi:** Angular istemcisinden gelen tüm HTTP istekleri tek bir giriş noktasından (Gateway) kabul edilir. Gateway, kullanıcının rolünü ve JWT token'ını doğruladıktan sonra isteği ilgili mikroservise yönlendirir.
*   **Service-to-Service İletişimi (`internal_jwt` Mekanizması):** Mikroservislerin birbirlerinden veri talep etmesi gerektiğinde (Örneğin; `PKS.Terms` servisinin bir rapor işlemi sırasında kullanıcı bilgilerini doğrulamak için `PKS.Users` servisine gitmesi), bu trafik de güvenlik amacıyla yine Gateway üzerinden akar.

> *Note: İç ağdaki (internal) bu iletişimde, dış dünyadan gelen kullanıcı token'ı yerine sadece servislerin kendi arasında geçerli olan, kısa ömürlü ve yüksek yetkili bir **`internal_jwt`** kullanılır. Bu sayede servislerin iç API'leri dış dünyaya tamamen kapalı ve Zero-Trust (Sıfır Güven) modeline uygun olarak korunmuş olur.*

## <span id="en">English</span>

### Technological Infrastructure and Rationale

The project’s backend architecture is built on modern technologies and patterns to ensure high scalability, flexibility, and loose coupling:

* **.NET 10:** Selected for its cutting-edge performance optimizations, native support for Minimal APIs, and enhanced built-in dependency injection (DI) capabilities.
* **CQRS & MediatR:** By strictly separating read (Query) and write (Command) operations, codebase readability and specialized performance tuning have been optimized. Complex business logic is cleanly isolated using MediatR pipelines.
* **Clean Architecture & DDD:** Core business rules (Domain) are completely decoupled from external infrastructure and technological dependencies (such as EF Core, PostgreSQL, etc.). Rich domain models and validation boundaries are centralized.

---

### Microservice Boundaries and Data Management (Bounded Contexts)

The system is logically decomposed into the following autonomous services based on specific business processes:

1.  **PKS.Users (User Management Service):** The foundational service responsible for managing profiles, identity attributes, role definitions, and onboarding flows for all system actors (Students, Assistants, Advisors, and Admins).
2.  **PKS.Terms (Term and Process Management Service):** The core engine hosting the primary business logic. It orchestrates the lifecycle of academic terms, the formation of graduation and interim project groups, student-project matchmaking, report submission pipelines, and academic calendar deadlines.
3.  **PKS.IdentityVerification (Identity Verification & Security Service):** Handles the secure generation, lifecycle tracking, and validation of short-lived One-Time Passwords (OTP) required for password resets or step-by-step verification during critical operations.
4.  **PKS.Email (Email Service):** An isolated, stateless infrastructure service dedicated entirely to delivering system-triggered outbound email communications (such as transactional welcome notes or critical system alerts).

> 💡 **Note:** To guarantee eventual data consistency and independent deployability, each microservice is strictly designed around a **Database-per-Service** topology.

---

### Traffic Management and Secure Inter-Service Communication (Gateway)

Positioned at the edge of the ecosystem, **PKS.Gateway** intercepts all inbound client traffic and securely brokers communication between internal services:

* **Client-to-Service Communication:** All incoming HTTP traffic originating from the Angular SPA hits a single ingress endpoint (the Gateway). After validating the external user token and inspecting claim-based roles, the Gateway proxies the request to the target microservice.
* **Service-to-Service Communication (`internal_jwt` Mechanism):** When microservices must cross-reference data (e.g., the `PKS.Terms` service calling the `PKS.Users` service to verify a student's standing during an upload event), the request loops securely back through the Gateway.

> *Note: Within the internal network topology, standard external user tokens are discarded for inter-service calls. Instead, a short-lived, highly privileged **`internal_jwt`**—trusted exclusively between internal services—is dynamically generated. This ensures that internal service APIs remain completely hidden from the public internet, adhering strictly to a **Zero-Trust** security architecture.*