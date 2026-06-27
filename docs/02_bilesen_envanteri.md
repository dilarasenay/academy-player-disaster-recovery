# Sistem Bileşen Envanteri

## Doküman Bilgileri

| Alan | Değer |
|------|--------|
| Doküman Kodu | DOC-02 |
| Doküman Adı | Sistem Bileşen Envanteri |
| Proje | Akademi Player Felaket Kurtarma Planı |
| Hazırlayan | Takım |
| Sürüm | 1.0 |
| Tarih | 27.06.2026 |

---

# 1. Amaç

Bu dokümanın amacı, Akademi Player platformunu oluşturan kritik sistem bileşenlerini tanımlamak, her bileşenin işlevini açıklamak ve felaket senaryosunda sistem üzerindeki etkisini değerlendirmektir.

---

# 2. Sistem Mimarisi

Akademi Player platformu aşağıdaki temel bileşenlerden oluşmaktadır:

- Web Uygulaması (Frontend)
- Backend API
- PostgreSQL Veritabanı
- S3 Uyumlu Nesne Depolama
- CDN (Content Delivery Network)
- Kimlik Doğrulama Servisi
- İzleme ve Loglama Servisleri
- Yedekleme Altyapısı

---

# 3. Bileşen Envanteri

| Bileşen | Görevi | Kritiklik | Kesinti Durumu |
|----------|--------|-----------|----------------|
| Frontend | Kullanıcı arayüzünü sunar | Orta | Çalışır ancak hizmet veremez |
| Backend API | Tüm iş kurallarını yönetir | Kritik | Veritabanına erişemediği için çalışamaz |
| PostgreSQL | Kullanıcı, kurs ve ödeme verilerini saklar | Kritik | Tamamen hizmet dışı |
| S3 Nesne Depolama | Video, PDF ve medya dosyalarını saklar | Kritik | Tamamen hizmet dışı |
| Kimlik Doğrulama | Kullanıcı girişlerini yönetir | Kritik | Giriş işlemleri yapılamaz |
| CDN | Statik içerikleri dağıtır | Orta | Kaynak dosyalara ulaşamaz |
| İzleme Sistemi | Sistem sağlık durumunu izler | Yüksek | Alarm üretmeye devam eder |
| Yedekleme Sistemi | Verilerin geri yüklenmesini sağlar | Kritik | Kurtarma sürecinde aktif kullanılır |

---

# 4. Bileşen Bağımlılıkları

Aşağıdaki tablo, sistem bileşenleri arasındaki temel bağımlılıkları göstermektedir.

| Bileşen | Bağımlı Olduğu Servis |
|----------|-----------------------|
| Frontend | Backend API |
| Backend API | PostgreSQL |
| Backend API | S3 |
| Backend API | Authentication |
| CDN | S3 |
| Authentication | PostgreSQL |
| Monitoring | Tüm Sistem |

---

# 5. Felaket Senaryosundaki Etki

Bölgesel bulut kesintisi sonucunda aşağıdaki kritik bileşenler aynı anda kullanılamaz hale gelmektedir.

| Bileşen | Etki |
|----------|------|
| PostgreSQL | Veri erişimi tamamen durur |
| S3 | Eğitim içeriklerine erişilemez |
| Authentication | Kullanıcı girişleri başarısız olur |
| Backend API | İş süreçleri durur |
| Frontend | Kullanıcıya hata ekranı gösterir |

---

# 6. Kurtarma Önceliği

Felaket kurtarma sürecinde bileşenlerin aşağıdaki sırayla geri yüklenmesi planlanmaktadır.

| Öncelik | Bileşen | Gerekçe |
|----------|----------|---------|
| 1 | PostgreSQL | Tüm iş süreçlerinin temel veri kaynağıdır. |
| 2 | Kimlik Doğrulama | Kullanıcı erişiminin yeniden sağlanması gerekir. |
| 3 | Backend API | İş mantığının tekrar çalışması sağlanır. |
| 4 | S3 Nesne Depolama | Eğitim içeriklerinin erişilebilir olması sağlanır. |
| 5 | Frontend | Kullanıcı arayüzü yeniden hizmet vermeye başlar. |
| 6 | CDN | Performans optimizasyonu tekrar aktif edilir. |

---

# 7. Sonuç

Sistem bileşenleri incelendiğinde PostgreSQL veritabanı ile S3 uyumlu nesne depolama servislerinin platformun en kritik altyapı bileşenleri olduğu görülmektedir. Bu servislerin aynı anda kullanılamaz hale gelmesi platformun temel işlevlerini durdurmaktadır. Bu nedenle felaket kurtarma sürecinde bağımlılık ilişkileri dikkate alınarak bileşenlerin doğru sırayla geri yüklenmesi kritik öneme sahiptir.