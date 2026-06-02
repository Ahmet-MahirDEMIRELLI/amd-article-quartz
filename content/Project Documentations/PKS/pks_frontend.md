---
title: PKS Frontend
tags:
  - angular21
  - typescript
  - html
  - css
---

<div align="right">
  <a href="#tr">🇹🇷 Türkçe</a> | <a href="#en">English</a>
</div>

---

## <span id="tr">🇹🇷 Türkçe</span>

## Proje Yapısı ve Mimarisi

PKS Frontend uygulaması, **Angular 21** standartlarına uygun olarak modern, reactive ve yüksek performanslı bir SPA (Single Page Application) olarak geliştirilmiştir.

### 1. Klasör ve Modül Stratejisi

Projede modül karmaşasını önlemek ve lazy loading performansını maksimuma çıkarmak için tamamen **Standalone Components** mimarisi kullanılmıştır. Proje klasör yapısı şu şekildedir:

* **`src/app/core`:** Uygulamanın kalbi olan singleton servisler, authentication interceptor'ları ve guard'lar yer alır.
* **`src/app/shared`:** UI bileşenleri (custom düğmeler, tablolar, spinner'lar) ve pipe'lar.
* **`src/app/features`:** Role ve iş koluna göre ayrılmış sayfalar:
    * `/admin`: Kullanıcı tanımlama, dönem açma/kapama işlemleri.
    * `/advisor`: Proje önerisi sunma, proje takip ekranları.
    * `/student`: Proje önerisi sunma, dosya yükleme, revizyon takip ekranları.
    * `/assistant`: Rapor inceleme ve yükleme arayüzleri.

### 2. Rol Tabanlı Arayüz ve Güvenlik (Authorization)

Backend'den dönen JWT token içerisindeki `roles` claim'i Angular `AuthService` tarafından çözümlenir.
* **Guards:** Rol bilgisi kullanılarak yetkisiz kullanıcıların URL üzerinden `/admin` veya `/instructor` sayfalarına erişimi client-side seviyesinde engellenir.
* **Dinamik Menü Render:** Yapılandırılan Directive'ler (`*ifRole="['Admin', 'Asistan']"`) sayesinde, kullanıcı yetkisine sahip olmadığı butonları veya menü elemanlarını DOM üzerinde hiç görmez.

## <span id="en">English</span>

## Project Structure and Architecture

The PKS Frontend application is built as a modern, reactive, and high-performance SPA (Single Page Application) adhering strictly to **Angular 21** standards.

### 1. Folder and Module Strategy

To eliminate module complexity and optimize lazy-loading performance at scale, the architecture fully embraces **Standalone Components**. The directory layout is structured as follows:

* **`src/app/core`:** The core of the application, housing global singleton services, authentication interceptors, and route guards.
* **`src/app/shared`:** Reusable UI components (custom buttons, data tables, loading spinners) and generic pipes.
* **`src/app/features`:** Feature-driven pages partitioned by user roles and workflows:
    * `/admin`: User provisioning, workspace setups, and opening/closing academic terms.
    * `/advisor`: Project proposal evaluations and milestone tracking interfaces.
    * `/student`: Project proposal, deliverable document uploads, and revision tracking dashboards.
    * `/assistant`: Document pre-screening, and uploading interfaces.

### 2. Role-Based Interface and Security (Authorization)

The `roles` claim embedded within the backend-issued JWT token is decoded and managed by the frontend `AuthService`.
* **Guards:** Functional route guards utilize role metadata to block unauthorized users from navigating to restricted paths (e.g., preventing a student from hitting `/admin` routes) directly at the client-side level.
* **Dynamic Menu Rendering:** Structural directives (`*ifRole="['Admin', 'Assistant']"`) are utilized to handle conditional UI states. Users are dynamically restricted from viewing buttons or navigation elements they lack the authorization to access, completely omitting them from the DOM tree.