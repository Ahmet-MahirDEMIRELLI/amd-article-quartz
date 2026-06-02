---
title: PKS
---

<div align="right">
  <a href="#tr">🇹🇷 Türkçe</a> | <a href="#en">English</a>
</div>

---

## <span id="tr">🇹🇷 Türkçe</span>

<div style="text-align:center;">
  <img src="static/pks/ytu_logo.png" width="150" />
</div>

## PROJENİN AMACI & PROBLEM & ÇÖZÜM

PKS (Proje Kayıt ve Yönetim Sistemi); Yıldız Teknik Üniversitesi Bilgisayar Mühendisliği Bölümü bünyesinde yürütülen **Bitirme Projesi** ve **Ara Proje** derslerinin tüm yönetim, takip ve teslim süreçlerini dijitalleştirmek, merkezi bir yapıya taşımak ve otomatikleştirmek amacıyla geliştirilmiştir.

### 1. Çözülen Temel Problemler

Geleneksel yöntemlerde (e-posta trafiği, dağınık dökümanlar) yaşanan ve bu projenin doğrudan çözmeyi hedeflediği temel problemler şunlardır:
* **İletişim ve Takip Kopukluğu:** Öğrenci, asistan ve danışman hoca arasındaki rapor teslimi ve revizyon süreçlerinin e-postalar arasında kaybolması, süreç takibini zorlaştırmaktadır.
* **Süreç Takibinin Zorluğu (E-posta ve Evrak Karmaşası):** Dönem boyunca hangi tarihte hangi raporun teslim edileceği, hangi öğrencinin dökümanını teslim edip etmediği veya revizyon isteyen hocanın e-postasının nerede olduğu gibi süreçlerin manuel takibi tam bir karmaşaya yol açmaktadır.
* **Akademik Takvim ve Kaçırılan Süreler:** Ara rapor ve nihai proje teslim tarihlerinin takibinin manuel yapılması, geç teslimlerin tespitinde ve objektif değerlendirmede operasyonel yük oluşturmaktadır.


### 2. Getirilen Çözüm
PKS, bu karmaşık yapıyı modern bir yazılım mimarisiyle çözer:
* **Merkezi Süreç Yönetimi:** Proje teklifinden nihai notlandırmaya kadar tüm adımlar tek bir platform üzerinden yönetilir.
* **Otomasyon ve Zamanlanmış Görevler:** Akademik takvime ait kritik son teslim tarihleri ve dönem kapanışları sistem tarafından arka planda otomatik olarak yürütülür. İnsan hatası minimuma indirilir.
* **Güvenli ve Esnek Rol Yönetimi:** Geliştirilen rol tabanlı backend mimarisi sayesinde, her kullanıcının sadece kendi sorumluluk alanındaki verilere erişmesi (Örn: Bir asistanın sadece kendi atandığı projeleri görebilmesi) garanti altına alınır.
* **Ölçeklenebilir Mikroservis Altyapısı:** Sistem mimarisi iş kollarına göre ayrıştırılarak, gelecekte eklenebilecek yeni özelliklerin mevcut sistemi etkilemeden entegre edilebilmesine uygun olarak tasarlanmıştır.

## <span id="en">English</span>

<div style="text-align:center;">
  <img src="static/pks/ytu_logo.png" width="150" />
</div>

## PROJECT OBJECTIVE & PROBLEM & SOLUTION

PKS (Project Registration and Management System) was developed to digitize, centralize, and automate the entire lifecycle of management, tracking, and submission processes for **Graduation Project** and **Interim Project** courses within the Department of Computer Engineering at Yıldız Technical University.

### 1. Key Problems Addressed

The primary inefficiencies encountered in traditional, legacy methods (such as unorganized email traffic and scattered documents) that this project directly aims to resolve include:
* **Communication and Alignment Gaps:** Deliverable submissions and feedback loops between students, teaching assistants, and faculty advisors easily get lost within convoluted email threads, severely hindering transparent process tracking.
* **Operational Chaos (Document & Email Overhead):** Manually tracking milestones throughout the semester—such as monitoring specific report deadlines, verifying individual student submissions, or locating specific revision requests from faculty members—creates immense administrative chaos.
* **Academic Calendar & Missed Deadlines:** Relying on manual oversight for interim reports and final project hard deadlines imposes a heavy operational burden, making it difficult to detect late submissions instantly and maintain objective evaluations.

### 2. The Solution Provided
PKS orchestrates and resolves this complex workflow by introducing a modern, robust software architecture:
* **Centralized Workflow Management:** Every phase of the academic lifecycle, from the initial project proposal submission to the final graduation grading, is seamlessly managed through a unified platform.
* **Background Automation & Scheduled Tasks:** Critical deadlines, and term-end closures tied to the university's academic calendar are executed automatically by background workers, mitigating human error to the absolute minimum.
* **Granular, Role-Based Access Control (RBAC):** Built upon a secure role-based backend architecture, the system guarantees data isolation where users interact strictly within their operational boundaries (e.g., a teaching assistant can only inspect and audit the specific projects assigned to their queue).
* **Scalable Microservices Infrastructure:** The overall system architecture is decoupled into separate, autonomous business domains. This modular approach ensures that future feature expansions can be seamlessly integrated without disrupting or causing regressions in the existing production environment.