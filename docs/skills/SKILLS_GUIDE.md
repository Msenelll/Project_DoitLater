# PROJECT SKILLS GUIDE (SKILLS_GUIDE.md)

Bu doküman, projede kullanılan teknik dikey yetenekleri ve otonom ajanların sahip olması gereken pratik yetkinlikleri tanımlar.

## 1. Web Scraping & Content Extraction (Kazıma Yeteneği)
* **Kapsam:** Verilen URL'lerin HTML içeriğini indirmek, `<title>`, `<meta>` (description, open-graph) etiketlerini çıkarmak ve ana body içeriğini sade metin olarak temizlemek.
* **Kütüphaneler:** BeautifulSoup4, HTTPX, Playwright/Selenium (JS gerektiren karmaşık sayfalar için opsiyonel).

## 2. AI Enrichment (Yapay Zeka Zenginleştirme Yeteneği)
* **Kapsam:** Kazınan ham metin ve meta verileri kullanarak OpenAI/Anthropic/Gemini API aracılığıyla:
  * Başlığı temizlemek ve optimize etmek.
  * Hiyerarşik bir kategori atamak.
  * İlgili anahtar kelimeleri ve etiketleri üretmek.
  * Kaynak türünü (YouTube, Medium vb.) belirlemek.
* **Standart:** Instructor veya OpenAI Structured Outputs kullanılarak %100 kararlı JSON çıktı elde edilmelidir.

## 3. Background Processing & Task Queues (Arka Plan Görev Yeteneği)
* **Kapsam:** Uzun süren kazıma ve API çağrılarını FastAPI istek hattını engellemeden asenkron olarak yürütmek.
* **Kütüphaneler:** Celery, Redis.
