# İş Kaybı Özeti

## Doküman Bilgileri

| Alan | Değer |
|------|--------|
| Doküman Kodu | DOC-01 |
| Doküman Adı | İş Kaybı Özeti |
| Proje | Akademi Player Felaket Kurtarma Planı |
| Hazırlayan | Dilara Şenay |
| Görev | Teknik Lider & Analiz Mimarı |
| Sürüm | 1.0 |
| Tarih | 27.06.2026 |

---

# 1. Amaç

Bu dokümanın amacı, Akademi Player platformunda meydana gelen bölgesel bulut kesintisinin iş süreçleri üzerindeki etkisini değerlendirmek, etkilenen kritik hizmetleri belirlemek ve kesintinin kullanıcılar, operasyonlar ve finansal süreçler üzerindeki olası sonuçlarını ortaya koymaktır.

---

# 2. Kapsam

Bu değerlendirme aşağıdaki kritik sistem bileşenlerini kapsamaktadır:

- PostgreSQL Veritabanı
- S3 Uyumlu Nesne Depolama
- Kimlik Doğrulama Servisi
- Backend API Servisleri
- İçerik Dağıtım Ağı (CDN)

---

# 3. Kesinti Özeti

Bulut sağlayıcısının birincil bölgesinde meydana gelen altyapı arızası sonucunda PostgreSQL veritabanı ve S3 uyumlu nesne depolama servisi aynı anda erişilemez hale gelmiştir.

Bu kesinti nedeniyle;

- Kullanıcılar platforma giriş yapamamaktadır.
- Eğitim videolarına erişilememektedir.
- Kurs ilerlemeleri kaydedilememektedir.
- Yeni abonelik işlemleri gerçekleştirilememektedir.
- İçerik üreticileri yeni eğitim içerikleri yükleyememektedir.

Platform, kritik bileşenler yeniden çalışır duruma getirilene kadar hizmet verememektedir.

---

# 4. Etkilenen İş Hizmetleri

| İş Hizmeti | Durum | İş Etkisi |
|------------|--------|-----------|
| Kullanıcı Girişi | Kullanılamıyor | Kullanıcılar sisteme erişemiyor |
| Video Yayını | Kullanılamıyor | Eğitim içerikleri görüntülenemiyor |
| Kurs İlerleme Takibi | Kullanılamıyor | İlerleme bilgileri kaydedilemiyor |
| Abonelik İşlemleri | Kullanılamıyor | Yeni satın alma işlemleri yapılamıyor |
| Ödeme Kayıtları | Kısmen Kullanılabilir | İşlemler gerçekleşse bile kayıtlar yazılamıyor |
| İçerik Yükleme | Kullanılamıyor | Yeni içerik yayınlanamıyor |

---

# 5. İş Etkisi Analizi

## Kullanıcı Etkisi

- Öğrenciler satın aldıkları eğitimlere erişememektedir.
- Kurumsal müşterilerin eğitim süreçleri durmaktadır.
- Öğrenme ilerlemeleri kaydedilememektedir.
- Kullanıcı memnuniyetinde düşüş yaşanmaktadır.

## Operasyonel Etki

- Destek taleplerinde artış beklenmektedir.
- Teknik ekip tüm kaynaklarını felaket kurtarma sürecine yönlendirmektedir.
- Planlanan yazılım güncellemeleri ertelenmektedir.

## Finansal Etki

Aşağıdaki değerler senaryo kapsamında oluşturulan varsayımsal verilerdir.

| Gösterge | Tahmini Değer |
|-----------|--------------:|
| Toplam Aktif Kullanıcı | 284.000 |
| Anlık Etkilenen Kullanıcı | 12.400 |
| Günlük Yeni Abonelik | 3.800 |
| Saatlik Tahmini Gelir Kaybı | 47.500 ₺ |
| Tahmini 6 Saatlik İş Kaybı | 285.000 ₺ |

---

# 6. Risk Altındaki Kritik Veriler

| Veri Türü | Risk Seviyesi | Açıklama |
|------------|---------------|----------|
| Kullanıcı Profilleri | Düşük | Düzenli yedeklerle korunmaktadır |
| Kurs İlerleme Verileri | Orta | Son işlemlerde veri kaybı yaşanabilir |
| Ödeme Kayıtları | Yüksek | Son işlemlerin doğrulanması gerekebilir |
| Video İçerikleri | Düşük | Nesne versiyonlama ile korunmaktadır |
| Aktif Oturumlar | Orta | Kullanıcıların yeniden giriş yapması gerekecektir |

---

# 7. Varsayımlar

Bu dokümanda kullanılan değerlendirmeler aşağıdaki varsayımlara dayanmaktadır:

- Sistem tek bir bulut bölgesinde çalışmaktadır.
- PostgreSQL üretim veritabanı olarak kullanılmaktadır.
- Eğitim içerikleri S3 uyumlu nesne depolama üzerinde tutulmaktadır.
- Kesinti süresinin yaklaşık 6 saat olduğu varsayılmıştır.
- Sayısal veriler senaryo amacıyla oluşturulmuş olup gerçek sistem verilerini temsil etmemektedir.

---

# 8. İş Öncelikleri

| Öncelik | Hizmet |
|----------|---------|
| Kritik | PostgreSQL Veritabanı |
| Kritik | Kimlik Doğrulama Servisi |
| Yüksek | Backend API |
| Yüksek | Nesne Depolama (S3) |
| Orta | CDN |
| Orta | İzleme ve Raporlama Servisleri |

---

# 9. Sonuç

Bölgesel bulut kesintisi, Akademi Player platformunun temel iş süreçlerini doğrudan etkileyen kritik bir olaydır. PostgreSQL veritabanı ile nesne depolama servislerinin aynı anda devre dışı kalması, kullanıcı erişimini, eğitim içeriklerinin sunulmasını, ödeme süreçlerini ve kurs ilerlemelerinin kaydedilmesini durdurmuştur.

İş sürekliliğinin sağlanabilmesi ve finansal kayıpların en aza indirilebilmesi için Felaket Kurtarma Planı'nın hızlı ve planlı bir şekilde uygulanması gerekmektedir.