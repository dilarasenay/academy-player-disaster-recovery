# Recovery Runbook

## Doküman Bilgileri

| Alan | Değer |
|------|--------|
| Doküman Kodu | DOC-07 |
| Doküman Adı | Recovery Runbook |
| Proje | Akademi Player Felaket Kurtarma Planı |
| Hazırlayan | Dilara Şenay |
| Görev | Database & Recovery Uzmanı |
| Sürüm | 1.0 |
| Tarih | 27.06.2026 |

---

# 1. Amaç

Bu doküman, Akademi Player platformunda meydana gelen bölgesel bulut kesintisi sonrasında sistemin güvenli ve kontrollü şekilde yeniden hizmete alınabilmesi için uygulanacak teknik kurtarma adımlarını tanımlamaktadır.

---

# 2. Ön Koşullar

Kurtarma işlemine başlamadan önce aşağıdaki şartların sağlanması gerekmektedir.

- Alternatif bölge (Region) kullanılabilir durumda olmalıdır.
- PostgreSQL yedekleri doğrulanmış olmalıdır.
- S3 nesne depolama yedeklerine erişim sağlanmalıdır.
- Teknik ekipler (Cloud, Database, DevOps) görev başında olmalıdır.
- Kesinti hakkında iç iletişim başlatılmış olmalıdır.

---

# 3. Kurtarma Adımları

## Adım 1 – Kesintiyi Doğrulama

- Bulut sağlayıcısının servis durumu kontrol edilir.
- Monitoring ve log kayıtları incelenir.
- Kesintinin kapsamı belirlenir.
- Olay kaydı (Incident) oluşturulur.

**Beklenen Çıktı:** Kesinti doğrulanmış ve olay yönetimi başlatılmıştır.

---

## Adım 2 – PostgreSQL Veritabanını Geri Yükleme

- En güncel yedek belirlenir.
- PostgreSQL sunucusu hazırlanır.
- Veritabanı yedekten geri yüklenir.
- Veri bütünlüğü kontrol edilir.
- Kritik tablolar doğrulanır.

**Beklenen Çıktı:** Veritabanı çalışır durumda ve erişilebilirdir.

---

## Adım 3 – Kimlik Doğrulama Servisini Aktifleştirme

- Authentication servisi yeniden başlatılır.
- Veritabanı bağlantısı doğrulanır.
- Test kullanıcıları ile giriş yapılır.

**Beklenen Çıktı:** Kullanıcı girişleri başarılıdır.

---

## Adım 4 – Backend API Servislerini Başlatma

- API servisleri yeniden başlatılır.
- Sağlık kontrolleri (Health Check) çalıştırılır.
- Veritabanı bağlantıları doğrulanır.
- Log kayıtları incelenir.

**Beklenen Çıktı:** İş servisleri tekrar çalışmaya başlamıştır.

---

## Adım 5 – S3 Nesne Depolamayı Doğrulama

- Bucket erişimleri kontrol edilir.
- Video içerikleri doğrulanır.
- Dosya bütünlüğü test edilir.
- Yetkilendirme politikaları doğrulanır.

**Beklenen Çıktı:** Eğitim içeriklerine erişim sağlanmıştır.

---

## Adım 6 – CDN ve Frontend Servislerini Aktifleştirme

- CDN önbelleği güncellenir.
- Frontend yayına alınır.
- Statik içerikler test edilir.

**Beklenen Çıktı:** Platform son kullanıcıya hizmet vermeye başlamıştır.

---

# 4. Doğrulama Kontrolleri

| Kontrol | Beklenen Sonuç |
|----------|----------------|
| Kullanıcı Girişi | Başarılı |
| Video İzleme | Başarılı |
| Kurs İlerlemesi | Kaydediliyor |
| Ödeme İşlemleri | Başarılı |
| API Sağlık Kontrolü | Başarılı |
| Veritabanı Bağlantısı | Başarılı |

---

# 5. Başarı Kriterleri

Recovery süreci aşağıdaki şartlar sağlandığında başarılı kabul edilir.

- Tüm kritik servisler çalışmaktadır.
- Kullanıcılar sisteme giriş yapabilmektedir.
- Eğitim içerikleri erişilebilirdir.
- Kurs ilerlemeleri kaydedilebilmektedir.
- Veritabanında veri bütünlüğü korunmuştur.
- Kritik hata kaydı bulunmamaktadır.

---

# 6. Başarısızlık Durumunda İzlenecek Yol

Eğer kurtarma sürecinde kritik bir hata oluşursa:

1. Kurtarma işlemi durdurulur.
2. Son başarılı yedeğe geri dönülür.
3. Teknik ekipler yeniden değerlendirme yapar.
4. Olay yöneticisi CTO'yu bilgilendirir.
5. Yeni kurtarma planı uygulanır.

---

# 7. Sonuç

Recovery Runbook, Akademi Player platformunun felaket sonrası en kısa sürede yeniden hizmet verebilmesi amacıyla hazırlanmıştır. Belirlenen adımların sırasıyla uygulanması, veri bütünlüğünün korunmasını ve sistemin kontrollü şekilde yeniden devreye alınmasını sağlayacaktır.