# PRODUCT REQUIREMENTS DOCUMENT (PRD.md)

## 1. GİRİŞ VE PROJE AMACI
**AI-Powered Smart Bookmarking & Curation System**, kullanıcıların internetten kaydettikleri bağlantıları (URL) veya toplu CSV dosyalarını asenkron olarak arka planda kazıyan (scrape), içeriklerini yapay zeka ile analiz eden ve bunları temiz başlıklar, akıllı kategoriler, etiketler ve kaynak türleri ile otomatik olarak sınıflandıran akıllı bir yer imi yönetim sistemidir.

Bu proje, geleneksel yer imi yöneticilerinin (Chrome Bookmarks, Pocket, Raindrop) sunduğu elle klasörleme/etiketleme zorluğunu, **otonom arka plan işçileri ve AI sınıflandırması** kullanarak ortadan kaldırır.

---

## 2. KULLANICI ROLLERİ & PERSONA
Sistemde tek tip son kullanıcı rolü bulunur. Her kullanıcı yalnızca kendi kaydettiği yer imlerine erişebilir, kendi özel kategorilerini yönetebilir ve verilerini CSV formatında dışa veya içe aktarabilir.

---

## 3. FONKSİYONEL GEREKSİNİMLER (FUNCTIONAL REQUIREMENTS)

### 3.1. Kullanıcı Yönetimi & Güvenlik (Authentication)
* Kullanıcılar e-posta ve şifre ile sisteme kayıt olabilmeli ve giriş yapabilmelidir.
* API erişimleri JWT (JSON Web Token) tabanlı yetkilendirme ile korunmalıdır.
* Her kullanıcının verileri (Bookmarks, Categories, Tags) veritabanı seviyesinde izole edilmelidir (User Isolation).

### 3.2. Yer İmi Ekleme Kanalları (Ingestion Channels)
* **Tekli URL Girişi:** Kullanıcı tarayıcıdan veya mobil uygulamadan tek bir URL yapıştırarak yer imi ekleyebilir.
* **Toplu CSV Girişi (Bulk Import):** Kullanıcı, içerisinde sadece URL'ler barındıran (başlıklı veya başlıksız) bir CSV dosyasını sisteme yükleyebilir. Sistem bu dosyayı okuyup her bir URL için arka planda bir kazıma görevi tetiklemelidir.

### 3.3. Asenkron Kazıma Motoru (Scraping Engine)
* Bir URL sisteme eklendiğinde, HTTP istek hattını (request thread) bloke etmemek için işlem asenkron olarak Celery kuyruğuna aktarılır.
* Arka plan işçisi ilgili URL'i güvenli şekilde (timeouts, user-agent simülasyonu vb.) indirir.
* HTML içeriğinden şu bilgileri ayıklar:
  * `<title>` (Ham başlık)
  * `<meta name="description">` ve Open Graph (`og:description`, `og:title`) etiketleri.
  * Sayfa içeriğinin sade metin (Plain Text) hali (reklamlar ve HTML script etiketleri temizlenmiş olarak).

### 3.4. AI ile Otomatik Zenginleştirme (AI Enrichment)
* Kazıma işlemi bittikten sonra, toplanan ham metin ve meta veriler AI API'sine (örn: OpenAI GPT-4o-mini veya muadili) gönderilir.
* AI'dan **kesinlikle doğrulanmış JSON şemasında** şu alanlar istenir:
  * `optimized_title`: Reklam, site adı veya gereksiz karakterlerden arındırılmış temiz başlık.
  * `primary_category`: Hiyerarşik yapıda bir ana kategori (örn: "Yazılım Geliştirme", "Finans", "Sağlık").
  * `tags`: İçeriği en iyi temsil eden 3-5 adet etiket dizisi.
  * `source_type`: Kaynağın türü. Seçenekler: `Medium`, `YouTube`, `Academic`, `Newsletter`, `Documentation`, `GitHub`, `Other`.
* AI çıktısının format güvenliği için Instructor veya yapılandırılmış çıktı kütüphaneleri (Structured Outputs) kullanılacaktır.

### 3.5. Durum Takibi (Task Status Management)
Yer imleri sisteme ilk eklendiğinde kuyruk durumuna göre şu statüleri alır ve kullanıcı arayüzünde bu statüler anlık veya periyodik olarak güncellenir:
* `Pending`: Kuyrukta bekliyor.
* `Scraping`: Web sayfası kazınıyor.
* `Enriched`: AI analizi tamamlandı ve yer imi hazır.
* `Failed`: Kazıma veya AI adımlarında hata oluştu.

### 3.6. Arama, Filtreleme ve Yönetim
* **Metin Arama:** Başlık, açıklama ve kazınan sayfa içeriğinde tam metin araması (Full-Text Search).
* **Kategoriye Göre Filtreleme:** Hiyerarşik kategori ağacı üzerinden filtreleme.
* **Etikete Göre Filtreleme:** Etiket bulutu veya çoklu etiket seçimi ile filtreleme.
* **Kaynak Türüne Göre Filtreleme:** (Örn: Sadece video veya sadece dokümantasyonları listeleme).
* **Manuel Düzenleme:** Otomatik atanan etiket, kategori ve başlıkların kullanıcı tarafından elle değiştirilebilmesi.

---

## 4. KULLANICI İŞ AKIŞLARI (USER FLOWS)

### 4.1. Web Paneli Akışı
1. Kullanıcı sisteme giriş yapar ve ana Dashboard ekranını görür.
2. Sağ üstteki "+ Yeni Ekle" butonuna tıklar.
3. Açılan modal pencerede "Tekli URL" veya "CSV Yükle" seçeneklerinden birini seçer.
4. "URL Ekle" butonuna bastığında, yer imi listede `Pending` statüsüyle belirir.
5. Arka planda Celery görevi çalışırken statü `Scraping` olarak güncellenir.
6. İşlem bittiğinde yer imi `Enriched` statüsüne geçer, AI tarafından temizlenen başlık, kategori ve etiketler listede görünür hale gelir.

### 4.2. Mobil Uygulama Akışı
1. Kullanıcı mobil uygulamayı açar ve JWT token ile otomatik login olur.
2. Cihazındaki tarayıcıdan bir bağlantıyı kopyalayıp uygulamaya girdiğinde "Kopyalanan URL'i Kaydetmek İster misiniz?" uyarısı alır.
3. Kaydet butonuna bastığında asenkron süreç başlar ve mobil arayüzde durum kartı güncellenir.

---

## 5. FONKSİYONEL OLMAYAN GEREKSİNİMLER (NON-FUNCTIONAL REQUIREMENTS)
* **Asenkron Yapı:** Web kazıma ve AI istekleri hiçbir koşulda web/mobil API isteklerini bekletmemelidir.
* **Hata Toleransı (Resiliency):** Kazınamayan (örn: Cloudflare korumalı veya erişilemeyen) sitelerde süreç patlamamalı, statü `Failed` yapılmalı ve kullanıcıya ham URL gösterilmeye devam edilmelidir.
* **Hız ve Caching:** Sıkça sorgulanan kategoriler ve etiketler Redis üzerinde önbelleğe alınmalıdır (caching).
* **Veri Güvenliği:** RLS (Row Level Security) veya yazılım katmanında katı kullanıcı filtreleri ile bir kullanıcının diğerinin bağlantılarını görmesi kesinlikle engellenmelidir.

---

## 6. KABUL KRİTERLERİ (ACCEPTANCE CRITERIA)
* Kullanıcı sisteme 50 satırlık bir CSV yüklediğinde, arayüz kilitlenmeden istek başarılı yanıtı dönmeli ve arka planda 50 bağımsız kazıma görevi sırayla çalışmalıdır.
* AI zenginleştirme API'si JSON şeması dışında veri döndüğünde sistem çökmemeli, hata loglanmalı ve yer imi `Failed` durumuna çekilerek kullanıcı uyarılmalıdır.
* Web dashboard tasarımı responsive olmalı; masaüstü, tablet ve mobil tarayıcılarda kayma olmaksızın kullanılabilmelidir.
