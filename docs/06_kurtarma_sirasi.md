# Kurtarma Sırası

## Doküman Bilgileri

| Alan | Değer |
|------|--------|
| Doküman Kodu | DOC-06 |
| Doküman Adı | Kurtarma Sırası |
| Proje | Akademi Player Felaket Kurtarma Planı |
| Hazırlayan | Nehir Doğan |
| Görev | Database & Recovery Uzmanı |
| Sürüm | 1.0 |
| Tarih | 27.06.2026 |

---

# 1. Amaç

Bu doküman, bölgesel kesinti sonrası felaket kurtarma sürecinde sistem bileşenlerinin hangi sırayla geri yükleneceğini ve bu sıralamanın teknik ve iş gerekçesini ortaya koymaktadır.

---

# 2. Temel Karar: Önce PostgreSQL, Sonra Nesne Deposu (S3)

Kurtarma sürecinde **PostgreSQL veritabanı önce, S3 uyumlu nesne depolama ardından** geri yüklenecektir.

## Gerekçe

**a) Bağımlılık yönü:**
Bağımlılık diyagramına göre Backend API ve Kimlik Doğrulama servisleri doğrudan PostgreSQL'e bağımlıdır. PostgreSQL olmadan sistem hiçbir temel işlevini yerine getiremez; oysa S3 geçici olarak erişilemez kalsa da kullanıcı girişi ve metin tabanlı işlemler (kurs listesi, ilerleme durumu görüntüleme) teorik olarak sürdürülebilir.

**b) Veri kaybı riski farkı:**
PostgreSQL, kritiklik skoru 5/5 ile ödeme, abonelik ve kullanıcı ilerleme kayıtları gibi geri alınamaz finansal veriler içerir. S3'teki video içerikleri ise "write-once" -bir kez yazılıp değişmeyen- yapıdadır; bir kayıp durumunda orijinal üretim hattından yeniden türetilebilir.

**c) Geri yükleme süresine duyarlılık:**
PostgreSQL'in duyarlılık skoru 5/5, S3'ün ise 3/5'tir. PostgreSQL'deki bir gecikme veri tutarsızlığı riskini hızla büyütürken, S3'ün geri yüklenmesi bant genişliği kısıtı nedeniyle saatler sürebilir; bu süre kabul edilebilir bir gecikme olarak değerlendirilmiştir.

**d) RPO/RTO hedefleriyle uyum:**
PostgreSQL için RPO 15 dakika / RTO 2 saat gibi dakika-saat seviyesinde sıkı hedefler belirlenmişken, S3 için RPO 1 saat / RTO 4-6 saat gibi daha gevşek hedefler tanımlanmıştır. Bu fark, PostgreSQL'in önceliğini doğrudan destekler.

**e) İş gerekçesi:**
İş Kaybı Özeti'nde "Ödeme Kayıtları" risk seviyesi Yüksek olarak işaretlenmiştir. Finansal verinin önceliklendirilmesi, kurumsal ve hukuki sorumluluk açısından da gereklidir.

---

# 3. Tam Kurtarma Sırası

| Sıra | Bileşen | Başlangıç Koşulu | Gerekçe Özeti |
|---|---|---|---|
| 1 | PostgreSQL | Kesinti tespiti sonrası hemen | Tüm sistemin temel veri kaynağı; finansal veri içerir |
| 2 | Kimlik Doğrulama | PostgreSQL bağlantısı doğrulandıktan sonra | PostgreSQL'e bağımlı, kullanıcı erişimi için zorunlu |
| 3 | Backend API | Adım 1-2 tamamlanınca | PostgreSQL + Auth hazır olunca ayağa kalkabilir |
| 4 | S3 Nesne Depo | Adım 1 ile paralel başlatılabilir, doğrulama Adım 3 sonrası | Write-once veri, toleranslı; bant genişliği sınırlı |
| 5 | CDN | S3 sonrası, otomatik | Kendi veri kaynağı yok, S3 geri gelince otomatik ısınır |
| 6 | Vektör İndeksi / Ajan-RAG Bot | PostgreSQL ve S3 tam doğrulandıktan sonra | Türetilen veri; kaynaklar hazır olmadan yeniden inşa edilemez |

**Önemli not:** S3'ün geri yüklenmesi PostgreSQL ile **paralel başlatılabilir** (ikisi birbirini engellemez), ancak kullanıcıya "sistem hazır" bildirimi PostgreSQL + Kimlik Doğrulama + Backend API doğrulandıktan sonra, S3 senkronizasyonu tamamen beklenmeden kademeli olarak verilebilir (örn. "Giriş yapabilirsiniz, video kütüphanesi senkronize ediliyor" mesajı).

---

# 4. Başarı / Başarısızlık Kriterleri

| Bileşen | Başarı Kriteri | Başarısızlık Durumunda Aksiyon |
|---|---|---|
| PostgreSQL | Bağlantı testi başarılı + son commit edilen WAL kaydı tutarlı + test sorgusu doğru sonuç döner | Önceki yedeğe (point-in-time recovery) geri dön, ekibe eskale et |
| Kimlik Doğrulama | Test kullanıcısıyla giriş denemesi başarılı | PostgreSQL bağlantısını yeniden doğrula, servisi yeniden başlat |
| Backend API | Health-check endpoint 200 OK döner + temel CRUD işlemi test edilir | Bağımlılıkları (PG, Auth) tekrar kontrol et |
| S3 Nesne Depo | Örnek dosya erişimi + checksum doğrulaması geçer | Yedekten kademeli geri yüklemeye devam et, kritik içeriği önceliklendir |
| Vektör İndeksi | Yeniden indeksleme job'ı hatasız tamamlanır | Job'ı yeniden tetikle, kaynak veri (PG/S3) bütünlüğünü kontrol et |

---

# 5. Sonuç

PostgreSQL'in önceliklendirilmesi; bağımlılık grafiği (DOC-04), kritiklik analizi (DOC-03), RPO/RTO hedefleri (DOC-05) ve iş kaybı değerlendirmesi (DOC-01) ile tam tutarlıdır. Bu sıralama veri bütünlüğünü ilk planda tutarken, video içeriği gibi toleranslı bileşenlerin kademeli ve paralel şekilde geri gelmesine izin vererek toplam kurtarma süresini optimize etmektedir.