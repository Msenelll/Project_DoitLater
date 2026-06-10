# PROJECT CONTEXT & ENGINEERING CONSTITUTION (CONTEXT.md)

Bu doküman, **AI-Powered Smart Bookmarking System** projesinin mimari standartlarını, teknoloji yığınını, kodlama kurallarını ve Git/Versiyonlama protokolünü belirleyen ana anayasadır. Projedeki tüm geliştirici ajanlar bu kurallara uymakla yükümlüdür.

---

## 1. TEKNOLOJİ YIĞINI (TECH STACK)

Proje, hem mobil hem de web platformlarında tutarlı bir deneyim sunacak şekilde tasarlanmıştır:

* **Backend Mimarisi:**
  * **Dil & Framework:** Python 3.11+ ve FastAPI (Asenkron API desteği ve yüksek performans).
  * **Database ORM:** SQLAlchemy veya Prisma Client Python.
  * **Task Queue (Asenkron İşler):** Celery (Redis backend ile birlikte URL kuyruk yönetimi ve asenkron web scraping/AI zenginleştirme işlemleri için).
* **Veritabanı:**
  * **Veritabanı:** PostgreSQL (İlişkisel veri yönetimi, kullanıcılar, yer imleri, etiketler ve kategoriler için).
* **Web Frontend:**
  * Next.js (React) ve Tailwind CSS, UI bileşenleri için shadcn/ui.
* **Mobile Frontend:**
  * React Native (Expo) (Web projesiyle tip ve iş mantığı paylaşımını kolaylaştırmak için).

---

## 2. DİZİN YAPISI (DIRECTORY STRUCTURE)

Proje kök dizini altında aşağıdaki yapı kurulacaktır:
```
/
├── backend/                  # FastAPI & Celery & Postgres kod tabanı
├── frontend-web/             # Next.js web uygulaması
├── frontend-mobile/          # React Native Expo mobil uygulaması
├── docs/                     # Proje dokümantasyonu
│   ├── standards/            # Ajan kılavuzları
│   ├── neededAgents/         # Ajan rol tanımları
│   └── skills/               # Teknik yetenek kılavuzları
```

---

## 3. GIT VE SÜRÜM YÖNETİM PROTOKOLÜ (STRICT RULES)

* **Dallanma (Branch) Kuralları:**
  * `master`: Üretim (Production) ortamıdır. Kullanıcının açık izni olmadan bu dala doğrudan commit atılamaz veya merge yapılamaz.
  * `develop`: Ana geliştirme dalıdır. Tüm testlerden geçmiş ve onaylanmış özellikler buraya birleştirilir.
  * `feature/*`: Yeni özellik veya hata düzeltmeleri için `develop` üzerinden açılan çalışma dallarıdır (örn: `feature/auth-setup`).
* **Otomatik Entegrasyon:**
  * Ajan, geliştirdiği özellikleri ilgili `feature/*` dalında tamamlayıp test ettikten sonra, kullanıcının onayına gerek duymadan doğrudan `develop` branch'ine merge edebilir.
* **Sürüm ve Commit Biçimi:**
  * Tüm commit başlıkları **`vA.B:C - Commit Mesajı`** biçiminde olmalıdır:
    * **A (Master):** Master ana sürüm numarası.
    * **B (Develop):** Develop geliştirme sürüm numarası.
    * **C (Feature/Patch):** Feature veya yama seviyesi.
  * **Örnekler:**
    * `v0.1:0 - Proje dizin yapısı ve dokümantasyon kurulumu yapıldı` (İlk kurulum)
    * `v0.1:1 - Veritabanı bağlantısı ve modeller eklendi` (Feature dalında geliştirme)
    * `v0.1:0 - Feature/auth-setup develop dalına merge edildi` (Develop'a birleştirme)
    * `v1.0:0 - Canlı sürüme geçiş yapıldı` (Master dalında release)

---

## 4. KOD KALİTESİ VE GÜVENLİK STANDARTLARI

1. **Explicit Error Handling:** Tüm API uçlarında ve asenkron işçilerde (Celery workers) hata yakalama (`try-except` blokları) ve uygun HTTP hata kodları/loglama yapılacaktır.
2. **Strict Data Validation:** Backend tarafında `Pydantic` modelleri, frontend tarafında ise `TypeScript` interface'leri kullanılacaktır.
3. **Structured AI Outputs:** AI API çağrılarında json çıktısının doğruluğunu garanti altına almak için strict JSON şemaları (Instructor veya OpenAI Structured Outputs gibi kütüphaneler ile) zorunlu kılınacaktır.
4. **Asenkron Yapı:** Web scraping ve AI API entegrasyonu gibi zaman alan işlemler kesinlikle ana API thread'ini engellememeli, Celery üzerinden asenkron arka plan görevleri olarak yürütülmelidir.
