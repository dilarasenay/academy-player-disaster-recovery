# Sistem Bileşen Envanteri

## Doküman Bilgileri

<<<<<<< HEAD
| Alan | Değer |
|------|--------|
| Doküman Kodu | DOC-02 |
| Doküman Adı | Sistem Bileşen Envanteri |
| Proje | Akademi Player Felaket Kurtarma Planı |
| Hazırlayan | Takım |
| Sürüm | 1.0 |
| Tarih | 27.06.2026 |
=======
| Alan         | Değer                                 |
| ------------ | ------------------------------------- |
| Doküman Kodu | DOC-02                                |
| Doküman Adı  | Sistem Bileşen Envanteri              |
| Proje        | Akademi Player Felaket Kurtarma Planı |
| Hazırlayan   | Ahmet Özdoğan                         |
| Görev        | Cloud & Altyapı Mimarı                |
| Sürüm        | 1.0                                   |
| Tarih        | 27.06.2026                            |
>>>>>>> 3e7a9cd39d1fab35adefa2c835305e0bf0c9d31f

---

# 1. Amaç

<<<<<<< HEAD
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
=======
Bu dokümanın amacı, Akademi Player platformunu oluşturan sistem bileşenlerini listelemek; her bileşenin iş sürekliliği açısından **kritiklik** seviyesini ve **geri yükleme süresine duyarlılığını** belirlemek; bu sıralamanın RPO/RTO hesaplarına ve kurtarma runbook'una temel oluşturmasını sağlamaktır.

---

# 2. Kapsam

Bu envanter aşağıdaki bileşenleri kapsamaktadır:

- PostgreSQL Veritabanı (birincil)
- Uygulama Katmanı (API / Backend)
- S3 Uyumlu Nesne Depolama (video, poster)
- İçerik Dağıtım Ağı (CDN)
- Ajan Bileşeni — RAG Destek Botu (vektör indeksi + araç çıktısı önbelleği)

---

# 3. İş Kaybı Özeti (Altyapı Perspektifi)

Akademi Player'da video posterleri ve ham video dosyaları S3 uyumlu nesne depoda, kurs metaverisi ve kullanıcı ilerleme kayıtları PostgreSQL'de tutulmaktadır. Depolama bölgesinin 6 saat erişilemez olması durumunda yeni video oynatma tamamen durur; kullanıcılar giriş yapabilse de uygulama katmanı video teslimini doğrulamak için nesne depoya bağımlı olduğundan ders ilerlemesi görüntülenemez veya kaydedilemez. İş Kaybı Özeti dokümanında (DOC-01) belirtilen finansal varsayımlarla tutarlı olarak, bu envanterdeki kritiklik sıralaması doğrudan o dokümandaki "İş Öncelikleri" tablosunu (PostgreSQL ve Kimlik Doğrulama → Kritik; API ve S3 → Yüksek; CDN → Orta) teyit etmekte ve detaylandırmaktadır.

---

# 4. Bileşen Envanteri

Sıralama, **kritiklik** (1 = en yüksek öncelik) ve **geri yükleme süresine duyarlılık** (RTO duyarlılığı: kesinti süresi uzadıkça etkinin ne kadar hızlı büyüdüğü) kriterlerine göre yapılmıştır.

| #   | Bileşen                                             | Açıklama                                                                                       | Kritiklik (1-5) | Geri Yükleme Duyarlılığı (1-5) | Gerekçe                                                                                                                                                                                                                         |
| --- | --------------------------------------------------- | ---------------------------------------------------------------------------------------------- | --------------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **PostgreSQL — Birincil Veritabanı**                | Kullanıcı hesapları, oturum, ilerleme, ödeme/abonelik kayıtları                                | **5**           | **5**                          | Veri kaybı veya bozulması doğrudan finansal/hukuki sonuç doğurur (ödeme kayıtları). Uygulama hiçbir işlevini bu olmadan yürütemez.                                                                                              |
| 2   | **Uygulama Katmanı (API / Backend)**                | İş mantığı, kimlik doğrulama, video teslim koordinasyonu                                       | **5**           | **4**                          | Postgres ve S3'e bağımlı; kendisi durum tutmadığı (stateless) için en hızlı yeniden ayağa kalkabilecek bileşendir, ama bağımlılıkları olmadan çalışmaz.                                                                         |
| 3   | **S3-Uyumlu Nesne Depo — Video & Poster Dosyaları** | Ham video, transkodlanmış format, poster/küçük görsel                                          | **4**           | **3**                          | Kritiklik yüksektir (asıl ürün budur) ama geri yükleme duyarlılığı nispeten düşüktür; video verisi genelde değişmeyen (immutable) içeriktir ve yedekten geri yüklenebilir. Bant genişliği sınırlayıcı faktördür (bkz. Bölüm 6). |
| 4   | **Ajan Bileşeni — RAG Destek Botu**                 | Kurs içeriği ve SSS üzerinden yanıt veren destek botu; vektör indeksi + araç çıktısı önbelleği | **2**           | **2**                          | Müşteri/öğrenci deneyimini doğrudan durdurmaz; ancak destek yükünü insan ekibe taşıdığı için orta gecikmeyle iş etkisi büyür. Detay Bölüm 5.                                                                                    |
| 5   | **CDN / Önbellek Katmanı**                          | Statik içerik dağıtımı, video segment önbelleği                                                | **3**           | **1**                          | Kendi başına veri kaynağı değildir; S3 geri geldiğinde CDN cache'i otomatik olarak yeniden ısınır. Geri yükleme süresine en az duyarlı bileşendir.                                                                              |

**Genel kritiklik sırası (yüksekten alçağa):**
PostgreSQL > Uygulama Katmanı > S3 Nesne Depo > CDN > Ajan/RAG Bot

**Geri yükleme süresine duyarlılık sırası (yüksekten alçağa):**
PostgreSQL > Uygulama Katmanı > S3 Nesne Depo > Ajan/RAG Bot > CDN

> **Not:** PostgreSQL hem en kritik hem en duyarlı bileşendir çünkü hem "olmadan hiçbir şey çalışmaz" hem de "yanlış/eski geri yükleme veri tutarsızlığına yol açar" riskini taşır. S3 kritiklik olarak yüksek ama duyarlılık olarak daha toleranslıdır; video dosyaları write-once (bir kez yazılıp değişmeyen) içerik olduğundan kayıp olsa bile orijinal üretim hattından yeniden türetilebilir, sadece zaman alır.

---

# 5. Ajan Bileşeni — Kurtarma Sırasındaki Yeri

Ajan (RAG destek botu) iki alt bileşenden oluşur ve bunların kurtarma önceliği farklıdır:

- **Vektör indeksi (embedding store):** Kaynak veri (kurs içerik metinleri, SSS) PostgreSQL veya S3'te zaten saklı kabul edilir. Vektör indeksi bu kaynaktan yeniden türetilebilir (derived/rebuildable) bir yapıdır; kendi başına birincil veri kaynağı değildir. Bu nedenle kurtarma sırasında **en son** ele alınır: önce Postgres ve S3 ayağa kalkar, ardından indeksleme işi yeniden tetiklenir.
- **Araç çıktısı önbelleği (tool-call cache):** Botun daha önce ürettiği yanıtların/araç sonuçlarının önbelleğidir. Kaybı sadece performans regresyonuna (ilk isteklerde daha yavaş yanıt) yol açar, veri kaybı riski taşımaz. Kurtarma sırasında önbellek boş başlar; yeniden ısınma zaman içinde organik olarak gerçekleşir, aktif bir geri yükleme adımı gerektirmez.

**Sonuç:** Ajan bileşeni, kurtarma runbook'unun **son aşamasına** (Postgres → S3/Uygulama → Ajan/İndeks yeniden inşası) yerleştirilmiştir. Kesinti süresince destek botu devre dışı kabul edilir; öğrencilere "destek botu şu an kullanılamıyor" mesajı ve insan destek hattına yönlendirme yeterli kabul edilmiştir.

---

# 6. Bağımlılık Akışı (Özet)

```
PostgreSQL (kullanıcı / ilerleme / ödeme)
        │
        ▼
Uygulama Katmanı (API) ──► S3 Nesne Depo (video / poster)
        │                         │
        ▼                         ▼
   Ajan / RAG Bot ◄── Vektör İndeksi (Postgres + S3'ten türetilir)
        │
        ▼
     CDN (S3'in önbelleklenmiş kopyası)
```

Bu akış, DOC-04 (Bağımlılık Diyagramı) için temel veri girdisidir.

---

# 7. Bant Genişliği Notu

Video verisinin binlerce GB ölçeğinde olması durumunda, S3 nesne deposunun yedekten geri yükleme süresi disk/depolama hızından değil **ağ bant genişliğinden** sınırlanır. Bu nedenle S3'ün geri yükleme duyarlılığı (Bölüm 4'te 3/5) PostgreSQL'den daha düşük tutulmuştur; kritik kullanıcı ve ödeme verisi öncelikli olarak hızla geri gelirken, video kütüphanesinin tam senkronizasyonu saatler sürebilir; bu kabul edilebilir bir gecikme olarak değerlendirilmiştir. Kesin saatlik rakamlar DOC-05 (RPO/RTO Hesapları) dokümanında detaylandırılacaktır.
>>>>>>> 3e7a9cd39d1fab35adefa2c835305e0bf0c9d31f

#ahmetindi