# PROJECT BACKLOG & SPRINT PLANNING (BACKLOG.md)

Bu doküman projenin tüm sürüm hedeflerini, sprint planlarını ve geliştirme iş listesini (tickets) takip eder.

---

## SPRINT 1: Backend Initialization & Core Database
* **Hedef:** Proje altyapısının kurulması, PostgreSQL veritabanı şemalarının oluşturulması, kullanıcı kimlik doğrulama (JWT Auth) ve temel yer imi (bookmark) ekleme/listeleme API'lerinin tamamlanması.
* **Durum:** `BEKLEMEDE`

### İş Listesi (Tickets)
| Ticket ID | Başlık / Tanım | Durum | Öncelik | Atanan |
| :--- | :--- | :--- | :--- | :--- |
| `T1.1` | FastAPI Proje Yapısının Kurulması ve Bağımlılıkların Tanımlanması | `BEKLEMEDE` | Yüksek | @arch |
| `T1.2` | SQLAlchemy / Prisma Modellerinin ve PostgreSQL Bağlantısının Yazılması | `BEKLEMEDE` | Yüksek | @arch |
| `T1.3` | Kullanıcı Kayıt / Giriş (JWT Auth) endpoints ve Şifre Hashleme | `BEKLEMEDE` | Yüksek | @arch |
| `T1.4` | CRUD API: Yer imi ekleme, silme, güncelleme ve listeleme endpoints | `BEKLEMEDE` | Orta | @arch |

---

## SPRINT 2: Async Scraping Engine & AI Enrichment
* **Hedef:** Celery ve Redis altyapısının kurulması, asenkron URL kazıma (scraping) ve AI API entegrasyonu ile yer imlerinin otomatik sınıflandırılması.
* **Durum:** `BEKLEMEDE`

### İş Listesi (Tickets)
| Ticket ID | Başlık / Tanım | Durum | Öncelik | Atanan |
| :--- | :--- | :--- | :--- | :--- |
| `T2.1` | Redis ve Celery Entegrasyonunun Tamamlanması | `BEKLEMEDE` | Yüksek | @arch |
| `T2.2` | Web Scraping Servisi (HTML Parse, Meta etiketleri ve İçerik Temizleme) | `BEKLEMEDE` | Yüksek | @arch |
| `T2.3` | AI API Entegrasyonu ve Instructor/Structured Output ile JSON Çıktı Üretimi | `BEKLEMEDE` | Yüksek | @arch |
| `T2.4` | Arka Plan Görev Durumu Takip API'si (Task Status check) | `BEKLEMEDE` | Orta | @arch |

---

## SPRINT 3: Next.js Web Dashboard & CSV Bulk Import
* **Hedef:** Next.js web uygulamasının ayağa kaldırılması, Tailwind CSS ve shadcn/ui ile premium arayüz tasarımı, yer imleri yönetimi ve toplu CSV yükleme paneli.
* **Durum:** `BEKLEMEDE`

### İş Listesi (Tickets)
| Ticket ID | Başlık / Tanım | Durum | Öncelik | Atanan |
| :--- | :--- | :--- | :--- | :--- |
| `T3.1` | Next.js Proje Kurulumu ve shadcn/ui Entegrasyonu | `BEKLEMEDE` | Yüksek | @virtuoso |
| `T3.2` | Dashboard Arayüz Tasarımı (Responsive Sidebar, Yer imleri listesi ve filtreler) | `BEKLEMEDE` | Yüksek | @virtuoso |
| `T3.3` | FastAPI API İletişim Servisi ve Zustand State Yönetimi | `BEKLEMEDE` | Yüksek | @arch |
| `T3.4` | Toplu CSV Dosyası Yükleme Arayüzü ve Backend Entegrasyonu | `BEKLEMEDE` | Orta | @prime |

---

## SPRINT 4: React Native Mobile Client & Sync
* **Hedef:** React Native (Expo) mobil uygulamasının kurulması, API entegrasyonu ve mobil üzerinden yer imi ekleme/görüntüleme.
* **Durum:** `BEKLEMEDE`

### İş Listesi (Tickets)
| Ticket ID | Başlık / Tanım | Durum | Öncelik | Atanan |
| :--- | :--- | :--- | :--- | :--- |
| `T4.1` | React Native Expo Projesinin İlklendirilmesi ve Tip Tanımları | `BEKLEMEDE` | Yüksek | @arch |
| `T4.2` | Mobil Giriş (Auth) ve Token Saklama (Secure Store) Yapısı | `BEKLEMEDE` | Yüksek | @arch |
| `T4.3` | Mobil Dashboard (Yer imleri listesi, arama ve filtreleme ekranları) | `BEKLEMEDE` | Yüksek | @virtuoso |
| `T4.4` | Paylaşım Eklentisi (Share Extension) Entegrasyonu Kılavuzu | `BEKLEMEDE` | Orta | @prime |
