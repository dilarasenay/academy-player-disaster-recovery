# Kritiklik Analizi

## Doküman Bilgileri

| Alan | Değer |
|------|--------|
| Doküman Kodu | DOC-03 |
| Doküman Adı | Kritiklik Analizi |
| Proje | Akademi Player Felaket Kurtarma Planı |
| Hazırlayan | Dilara Şenay |
| Görev | Teknik Lider & Analiz Mimarı |
| Sürüm | 1.0 |
| Tarih | 27.06.2026 |

---

# 1. Amaç

Bu dokümanın amacı, Akademi Player platformunu oluşturan kritik sistem bileşenlerini iş sürekliliği açısından değerlendirmek, her bileşenin önem derecesini belirlemek ve felaket kurtarma sürecindeki öncelik sırasını ortaya koymaktır.

---

# 2. Değerlendirme Kriterleri

Kritiklik değerlendirmesi aşağıdaki kriterlere göre yapılmıştır.

| Kriter | Açıklama |
|---------|----------|
| İş Sürekliliği | Bileşen çalışmadığında temel hizmetlerin etkilenme düzeyi |
| Veri Kaybı Riski | Veri bütünlüğüne olan etkisi |
| Sistem Bağımlılığı | Diğer servislerin bu bileşene bağımlılığı |
| Kullanıcı Etkisi | Son kullanıcı üzerindeki etkisi |
| Kurtarma Önceliği | Felaket sonrası geri yükleme sırası |

---

# 3. Kritiklik Değerlendirmesi

| Bileşen | Kritiklik | Gerekçe |
|----------|-----------|----------|
| PostgreSQL Veritabanı | ⭐⭐⭐⭐⭐ (5/5) | Kullanıcı, ödeme, kurs ve ilerleme verilerinin tamamı bu bileşende tutulmaktadır. Sistemin temel veri kaynağıdır. |
| Kimlik Doğrulama Servisi | ⭐⭐⭐⭐⭐ (5/5) | Kullanıcıların sisteme erişebilmesi için zorunludur. Veritabanına doğrudan bağımlıdır. |
| Backend API | ⭐⭐⭐⭐☆ (4/5) | Tüm iş kurallarını yönetir ve uygulamanın çalışmasını sağlar. |
| S3 Nesne Depolama | ⭐⭐⭐⭐☆ (4/5) | Eğitim videoları ve medya içerikleri bu serviste tutulmaktadır. İçerik erişimi için gereklidir. |
| CDN | ⭐⭐⭐☆☆ (3/5) | Performansı artırır ancak tek başına iş sürekliliğini sağlamaz. |
| İzleme ve Loglama | ⭐⭐⭐☆☆ (3/5) | Kesinti tespiti ve kurtarma doğrulaması için önemlidir. |
| Yedekleme Sistemi | ⭐⭐⭐⭐⭐ (5/5) | Felaket kurtarma sürecinin başarıyla tamamlanabilmesi için kritik öneme sahiptir. |
| RAG Destek Botu | ⭐⭐☆☆☆ (2/5) | İç destek süreçlerini kolaylaştırır ancak temel hizmetlerin çalışması için zorunlu değildir. |

---

# 4. Öncelik Matrisi

| Öncelik | Bileşen | Gerekçe |
|----------|----------|----------|
| 1 | PostgreSQL Veritabanı | Tüm sistemin temel veri kaynağıdır. |
| 2 | Kimlik Doğrulama Servisi | Kullanıcı erişiminin yeniden sağlanması gerekir. |
| 3 | Backend API | İş süreçlerinin tekrar çalışması sağlanır. |
| 4 | S3 Nesne Depolama | Eğitim içerikleri erişilebilir hale getirilir. |
| 5 | CDN | İçerik dağıtım performansı yeniden sağlanır. |
| 6 | İzleme ve Loglama | Sistem sağlığı doğrulanır. |
| 7 | RAG Destek Botu | Destek hizmetleri yeniden aktif edilir. |

---

# 5. İş Etkisi Değerlendirmesi

| Kritiklik Seviyesi | İş Etkisi |
|--------------------|-----------|
| Çok Yüksek | Platform tamamen hizmet dışı kalır. |
| Yüksek | Temel iş süreçleri önemli ölçüde etkilenir. |
| Orta | Performans ve kullanıcı deneyimi olumsuz etkilenir. |
| Düşük | Operasyonel süreçlerde aksama yaşanır ancak temel hizmetler devam eder. |

---

# 6. Sonuç

Yapılan değerlendirme sonucunda PostgreSQL veritabanı, kimlik doğrulama servisi ve yedekleme altyapısı platformun en kritik bileşenleri olarak belirlenmiştir. Bu bileşenlerin aynı anda kullanılamaz hale gelmesi, kullanıcı erişimi, veri bütünlüğü ve temel iş süreçlerinin tamamen durmasına neden olmaktadır.

Bu nedenle felaket kurtarma planında öncelik, veri bütünlüğünü koruyacak şekilde PostgreSQL veritabanının geri yüklenmesine verilmelidir. Ardından kimlik doğrulama servisi ve Backend API yeniden devreye alınmalı, son aşamada içerik servisleri ve performans bileşenleri aktif hale getirilmelidir. Bu yaklaşım, hem iş sürekliliğini hem de kullanıcı deneyimini en kısa sürede yeniden sağlamayı hedeflemektedir.