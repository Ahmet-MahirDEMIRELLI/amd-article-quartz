---
title: PKS Architecture & DevOps
tags:
  - microservices
  - system-design
  - docker
---

<div align="right">
  <a href="#tr">🇹🇷 Türkçe</a> | <a href="#en">English</a>
</div>

---

## <span id="tr">🇹🇷 Türkçe</span>

## Sistem Mimarisi ve Servis Haberleşmesi

PKS, dağıtık bir sistem mimarisine sahiptir. İstemci (Angular) doğrudan mikroservislerle konuşmaz; tüm trafik merkezi bir kapıdan yönetilir.

### 1. Dağıtık Sistem Mimarisi

PKS, yüksek erişilebilirlik, gevşek bağlılık (loose coupling) ve modüler büyüme hedefleri doğrultusunda mikroservis mimarisiyle tasarlanmıştır. Sistem genelindeki ağ trafiği yönlendirmesi, güvenlik doğrulamaları ve servis sınırları belirli protokollerle koruma altına alınmıştır.

<div style="text-align:center;">
  <img src="static/pks/architecture.png" />
</div>

### 2. Servisler Arası İletişim ve Güvenlik Topolojisi

Sistemde iki temel trafik yönü bulunmaktadır: İstemci-Servis arası ve Servis-Servis arası.

#### İstemci-Servis
* **Merkezi Giriş Noktası:** Angular istemcisi doğrudan mikroservislerin IP veya port bilgisine sahip değildir. Tüm istekler PKS.Gateway üzerinden karşılanır.
* **Kimlik Doğrulama:** Kullanıcı sisteme giriş yaptığında edindiği standart JWT token ile istek atar. Gateway bu token'ın geçerliliğini kontrol ederek isteği ilgili servise yönlendirir.

#### Servis-Servis
* **Gevşek Bağlılık:** Mikroservisler, birbirlerinin internal network adreslerini doğrudan bilmezler. Bir servisin diğerinden veri talep etmesi gerektiğinde istek yine PKS.Gateway üzerinden aktarılır.
* **Zero-Trust Mimarisi:** İç ağda güvenlik zafiyeti oluşmasını engellemek adına, dış dünyaya gelen kullanıcı token'ları iç iletişimde kullanılmaz. Bunun yerine, sadece servislerin kendi arasında geçerli olan, yüksek yetkili ve kısa ömürlü bir internal_jwt üretilir.

### 3. Docker ile Konteynerizasyon ve Altyapı Yönetimi

Projenin tüm bağımlılıklarıyla birlikte tek bir komutla izole bir şekilde ayağa kaldırılabilmesi için `docker-compose` altyapısı kurgulanmıştır.

#### Dağıtık Veri Yönetimi
Sistemde veri tabanı seviyesindeki sıkı bağlılıkları (*tight coupling*) engellemek amacıyla her mikroservisin veri katmanı ve konteyner alanı birbirinden tamamen ayrıştırılmıştır:
* **`pks-users-db`:** `PKS.Users` servisine ait kullanıcı ve rol verilerini barındıran izole veri tabanı konteyneri.
* **`pks-terms-db`:** `PKS.Terms` servisinin dönem, proje ve rapor süreçlerine ait iş mantığı verilerini barındıran izole veri tabanı konteyneri.
* **`pks-verification-db`:** `PKS.IdentityVerification` servisinin OTP ve doğrulama kodlarını yönettiği izole veri tabanı/veri tutma konteyneri.

> *Not: `PKS.Email` servisi, stateless (durumsuz) bir altyapı servisi olarak tasarlandığı için kendisine ait bir veri tabanı barındırmaz; görevini tamamen bellek üzerinden yürütür.*

#### Çevre Değişkenleri ve Veri Güvenliği (.env Yönetimi)
Sistem mimarisindeki gizli bilgilerin (bağlantı dizgileri, JWT şifreleme anahtarları vb.) kaynak kod içerisine gömülmesini (hardcoded) engellemek amacıyla **Merkezi `.env` (Environment Variables)** yapısı kullanılmıştır:

* **Compose Entegrasyonu:** Tüm mikroservislerin ihtiyaç duyduğu çevre değişkenleri, proje kök dizininde yer alan tek bir `.env` dosyasından beslenir. `docker-compose.yml` dosyası, ayağa kalkarken bu değişkenleri okuyarak ilgili konteynerlerin içerisine dinamik olarak enjekte eder.
* **Güvenlik ve Esneklik:** Bu yöntem sayesinde teknik değerlendiricilere veya canlı ortama (production) geçiş yaparken kod üzerinde hiçbir değişiklik yapmadan, sadece `.env` dosyasını değiştirerek sistemin farklı portlarda veya farklı veri tabanlarında çalışabilmesi (lokal, test veya canlı ortam ayrımı) esnek bir şekilde sağlanmıştır.

### 4. Sistem Dayanıklılığı ve Hata Toleransı (Resilience & Monitoring)

Dağıtık sistemlerin doğası gereği oluşabilecek geçici ağ kesintileri veya bir servisin anlık olarak yanıt verememesi gibi durumlarda, sistemin çökmesini engellemek ve operasyonel devamlılığı sağlamak adına izlenebilirlik ve hata yönetim mekanizmaları kurgulanmıştır:

#### Merkezi ve Temiz Loglama Altyapısı
* **Yapılandırılmış Loglama:** Sistem genelindeki tüm mikroservislerde, hataların kaynağını hızlıca tespit edebilmek adına yapılandırılmış loglama mimarisi kullanılmıştır. Her hatayla birlikte; hatanın hangi serviste oluştuğu, ilgili isteğin benzersiz takip numarası, hata zamanı ve hata detayları temiz ve standart bir formatta kayıt altına alınır.
* **Hata İzolasyonu:** Bir serviste oluşan hata, loglama katmanı tarafından yakalanarak izole edilir. Böylece istemciye mimari detayları açık etmeyen temiz bir hata mesajı dönülürken, arka planda teknik analize uygun detaylı log üretilir.

#### Kritik Hataların Yöneticiye Bildirilmesi
Sistemin kendi kendini izleyebilmesi ve kritik kesintilere hızlı müdahale edilebilmesi amacıyla aktif bir bildirim mekanizması tasarlanmıştır:
* **Anlık Durum Takibi:** `PKS.Terms` servisinin tamamen erişilmez olması veya veri tabanı bağlantılarının kopması gibi sistemin işleyişini durdurabilecek kritik seviyedeki hatalar, sadece log dosyalarına yazılmakla kalmaz; doğrudan yöneticiye e-posta iletilir.

## <span id="en">English</span>

## System Architecture and Service Communication

PKS features a distributed system architecture. The client (Angular) does not communicate directly with the microservices; all traffic is managed and orchestrated through a centralized gateway.

### 1. Distributed System Architecture

PKS is designed using a microservices architecture to achieve high availability, loose coupling, and modular scalability. Network traffic routing across the system, security validations, and service boundaries are protected by industry-standard protocols.

<div style="text-align:center;">
  <img src="static/pks/architecture.png" />
</div>

### 2. Inter-Service Communication and Security Topology

There are two primary traffic flows within the system: **Client-to-Service** and **Service-to-Service**.

#### Client-to-Service
* **Central Entry Point:** The Angular client does not have direct access to the IP or port information of the underlying microservices. All incoming requests are routed exclusively through `PKS.Gateway`.
* **Authentication:** Upon logging into the system, users attach their standard JWT token to each request. The Gateway verifies the token’s validity, checks user roles, and forwards the request to the relevant service.

#### Service-to-Service
* **Loose Coupling:** Microservices do not directly know each other’s internal network addresses. When a service needs to request data from another, the communication loop is securely handled back through `PKS.Gateway`.
* **Zero-Trust Architecture:** To prevent security vulnerabilities within the internal network, external user tokens are not reused for inter-service communication. Instead, a high-privilege, short-lived **`internal_jwt`** is dynamically generated, which is valid exclusively between internal services.

### 3. Containerization and Infrastructure Management with Docker

The `docker-compose` infrastructure has been set up to allow the entire project, along with all its dependencies, to be deployed in an isolated local environment using a single command.

#### Distributed Data Management (Database-per-Service)
To prevent tight coupling at the database level, the data layer and container environment of each microservice have been completely decoupled:
* **`pks-users-db`:** An isolated database container hosting user profiles and role data for the `PKS.Users` service.
* **`pks-terms-db`:** An isolated database container holding the business logic data related to academic terms, projects, and reporting processes for the `PKS.Terms` service.
* **`pks-verification-db`:** An isolated database/data storage container that manages OTPs and verification codes for the `PKS.IdentityVerification` service.

> 💡 *Note: Since the `PKS.Email` service is designed as a stateless infrastructure service, it does not host its own database; it performs its functions entirely in memory.*

#### Environment Variables and Data Security (.env Management)
To prevent sensitive information (connection strings, JWT secret keys, etc.) from being hardcoded into the source code, a **centralized `.env` (Environment Variables)** structure is strictly utilized:

* **Compose Integration:** All environment variables required by the microservices are sourced from a single `.env` file located in the project root directory. The `docker-compose.yml` file reads these variables during startup and dynamically injects them into the respective containers.
* **Security and Flexibility:** This approach allows the system to operate on different ports or with different databases (seamlessly distinguishing between local, test, or production environments) without making any changes to the codebase—simply by modifying the `.env` file when sharing the project with technical evaluators or moving to production.

### 4. System Resilience and Fault Tolerance (Resilience & Monitoring)

To prevent cascading failures and ensure operational continuity during transient network outages or temporary service unavailability—which are inherent to distributed systems—robust monitoring and fault management mechanisms have been implemented:

#### Centralized and Clean Logging Infrastructure
* **Structured Logging:** A structured logging architecture is used across all microservices to quickly identify the root cause of errors. For each exception, the originating service, a unique **Correlation ID** (request tracking number), the timestamp, and the full exception stack trace are recorded in a clean, standardized format.
* **Error Isolation:** Any error occurring within a service is gracefully captured and isolated by the logging layer. This ensures that a sanitized error message is returned to the client without exposing architectural internal details, while detailed technical logs are generated in the background for analysis.

#### Notifying Administrators of Critical Errors
An active health-monitoring and notification mechanism has been designed to enable system self-monitoring and allow for rapid intervention in the event of critical outages:
* **Real-Time Status Monitoring:** Critical errors that could halt core system operations—such as the `PKS.Terms` service becoming completely unavailable or database connections being lost—are not only written to log files but are also immediately pushed as an alert to the administrator via email.