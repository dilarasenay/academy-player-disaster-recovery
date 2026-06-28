# Test Planı

## Doküman Bilgileri

| Alan | Değer |
|------|--------|
| Doküman Kodu | DOC-08 |
| Doküman Adı | Test Planı |
| Proje | Akademi Player Felaket Kurtarma Planı |
| Hazırlayan | Dilara Şenay |
| Görev | Test, İletişim & Operasyon Lideri |
| Sürüm | 1.0 |
| Tarih | 27.06.2026 |

---

# 1. Amaç

Bu test planının amacı, felaket kurtarma (Disaster Recovery) sürecinin ardından Akademi Player platformunun tüm kritik bileşenlerinin doğru şekilde çalıştığını doğrulamak ve sistemin güvenli olarak yeniden hizmete alınmasını sağlamaktır.

---

# 2. Kapsam

Bu plan aşağıdaki sistem bileşenlerini kapsamaktadır:

- PostgreSQL Veritabanı
- Kimlik Doğrulama Servisi
- Backend API
- S3 Uyumlu Nesne Depolama
- Frontend
- CDN
- Ödeme Servisi

---

# 3. Test Ortamı

| Bileşen | Açıklama |
|----------|----------|
| Test Ortamı | Disaster Recovery Test Ortamı |
| Veritabanı | PostgreSQL (Restore Edilmiş) |
| Nesne Depolama | S3 Uyumlu Bucket |
| API | Staging Backend |
| Frontend | Test Ortamı |
| İzleme | Cloud Monitoring & Loglama |

---

# 4. Test Senaryoları

## 4.1 Veritabanı Testi

| Test | Beklenen Sonuç |
|------|----------------|
| PostgreSQL bağlantısı | Başarılı |
| Kullanıcı sorguları | Doğru sonuç döndürür |
| Kurs kayıtları | Erişilebilir |
| İlerleme verileri | Doğru görüntülenir |

---

## 4.2 Kimlik Doğrulama Testi

| Test | Beklenen Sonuç |
|------|----------------|
| Kullanıcı girişi | Başarılı |
| Şifre doğrulama | Başarılı |
| Oturum oluşturma | Başarılı |
| Yetkilendirme | Doğru çalışır |

---

## 4.3 API Testleri

| Test | Beklenen Sonuç |
|------|----------------|
| API Health Check | HTTP 200 |
| Kurs Listeleme | Başarılı |
| Kurs Satın Alma | Başarılı |
| Kurs İlerlemesi Kaydetme | Başarılı |

---

## 4.4 Nesne Depolama Testleri

| Test | Beklenen Sonuç |
|------|----------------|
| Video erişimi | Başarılı |
| PDF erişimi | Başarılı |
| Dosya indirme | Başarılı |
| Bucket erişimi | Başarılı |

---

## 4.5 Frontend Testleri

| Test | Beklenen Sonuç |
|------|----------------|
| Ana sayfa açılıyor | Başarılı |
| Kullanıcı paneli | Başarılı |
| Kurs sayfası | Başarılı |
| Video oynatma | Başarılı |

---

## 4.6 Ödeme Testleri

| Test | Beklenen Sonuç |
|------|----------------|
| Test ödemesi | Başarılı |
| Ödeme kaydı | Veritabanına yazılır |
| Sipariş oluşturma | Başarılı |

---

# 5. Entegrasyon Testleri

Aşağıdaki iş akışları uçtan uca test edilmelidir.

| Senaryo | Beklenen Sonuç |
|----------|----------------|
| Kullanıcı giriş → Kurs seç → Video izle | Başarılı |
| Kurs satın al → Ödeme → Kurs erişimi | Başarılı |
| Video izle → İlerleme kaydet → Tekrar giriş | İlerleme korunur |
| İçerik yükleme → S3 → Frontend görüntüleme | Başarılı |

---

# 6. Başarı Kriterleri

Test süreci aşağıdaki koşullar sağlandığında başarılı kabul edilir.

- Tüm kritik servisler çalışmaktadır.
- API Health Check başarılıdır.
- Kullanıcı giriş işlemleri sorunsuzdur.
- Eğitim içerikleri görüntülenebilmektedir.
- Kurs ilerleme bilgileri kaydedilmektedir.
- Ödeme işlemleri tamamlanabilmektedir.
- Kritik hata veya veri kaybı bulunmamaktadır.

---

# 7. Hata Yönetimi

Test sırasında kritik bir hata tespit edilmesi durumunda:

1. İlgili test durdurulur.
2. Hata kayıt altına alınır.
3. Teknik ekip bilgilendirilir.
4. Düzeltme sonrası test yeniden çalıştırılır.
5. Sonuçlar test raporuna eklenir.

---

# 8. Sonuç

Bu test planı, Disaster Recovery sürecinin ardından sistemin güvenli, tutarlı ve eksiksiz şekilde yeniden hizmet verebildiğini doğrulamak amacıyla hazırlanmıştır. Tüm testlerin başarıyla tamamlanması, Akademi Player platformunun operasyonel olarak tekrar kullanılabilir durumda olduğunu gösterecektir.