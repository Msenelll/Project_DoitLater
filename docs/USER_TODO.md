# USER TODO LIST & MANUAL ACTIONS (USER_TODO.md)

Bu dosya, yapay zeka ekibinin doğrudan yapamayacağı, kullanıcının elle yapması veya sağlaması gereken varlıkları (credentials, harici servis hesapları vb.) içerir.

---

## 1. ÇEVRESEL DEĞİŞKENLER & API ANAHTARLARI
Lütfen aşağıdaki değerleri içeren `.env` dosyalarını ilgili proje klasörleri altında oluşturun:

### Backend (`/backend/.env`)
```env
DATABASE_URL="postgresql://user:password@localhost:5432/bookmark_db"
REDIS_URL="redis://localhost:6379/0"
JWT_SECRET="YOUR_SUPER_SECRET_JWT_KEY"
OPENAI_API_KEY="your-openai-api-key"
```

### Frontend Web (`/frontend-web/.env.local`)
```env
NEXT_PUBLIC_API_URL="http://localhost:8000"
```

---

## 2. EXPO / MOBİL UYGULAMA KURULUMU
* React Native (Expo) mobil uygulamasının test edilebilmesi için mobil cihazınızda **Expo Go** uygulamasının yüklü olması gerekmektedir.
* Expo hesabı oluşturarak CLI üzerinden oturum açmanız gerekebilir.

---

## 3. VERİTABANI BAŞLANGIÇ KURULUMU
* PostgreSQL veritabanınızın aktif ve erişilebilir olduğundan emin olun.
* `backend/` dizini kurulduğunda veritabanı migrasyonunu tetiklemek için terminalde `prisma db push` veya `alembic upgrade head` komutlarını (seçilen ORM'e göre) çalıştırın.
