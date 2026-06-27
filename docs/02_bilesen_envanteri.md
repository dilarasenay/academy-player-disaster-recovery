# Sistem Bileşen Envanteri

## Doküman Bilgileri

| Alan         | Değer                                 |
| ------------ | ------------------------------------- |
| Doküman Kodu | DOC-02                                |
| Doküman Adı  | Sistem Bileşen Envanteri              |
| Proje        | Akademi Player Felaket Kurtarma Planı |
| Hazırlayan   | Ahmet Özdoğan                         |
| Görev        | Cloud & Altyapı Mimarı                |
| Sürüm        | 1.0                                   |
| Tarih        | 27.06.2026                            |

---

# 1. Amaç

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
