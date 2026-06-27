# RPO / RTO Hesapları

## Doküman Bilgileri

| Alan | Değer |
|------|--------|
| Doküman Kodu | DOC-05 |
| Doküman Adı | RPO / RTO Hesapları |
| Proje | Akademi Player Felaket Kurtarma Planı |
| Hazırlayan | Nehir Doğan |
| Sürüm | 1.0 |
| Tarih | 27.06.2026 |

---

# 1. Amaç

Bu doküman, her kritik bileşen için kabul edilebilir maksimum veri kaybı süresini (RPO) ve maksimum kabul edilebilir geri dönüş süresini (RTO) iş gerekçesiyle birlikte tanımlamaktadır. DOC-01 (İş Kaybı Özeti), DOC-02 (Sistem Bileşen Envanteri) ve DOC-04 (Bağımlılık Diyagramı) ile tutarlıdır.

---

# 2. RPO / RTO Tablosu

| Bileşen | RPO | RTO | İş Gerekçesi |
|---|---|---|---|
| PostgreSQL | 15 dk | 2 saat | Ödeme ve abonelik kayıtları içerir (DOC-01 §6'da "Yüksek risk"); 15 dakikadan fazla veri kaybı finansal sorun doğurabilir. WAL arşivleme ile desteklenir. |
| Kimlik Doğrulama | 15 dk (PG ile aynı) | 2 saat 15 dk | PostgreSQL'e bağımlı; PG hazır olduktan kısa süre sonra ayağa kalkar. |
| Backend API | RPO yok (stateless) | 2 saat 30 dk | Durum tutmadığı için veri kaybı riski yok; PG+Auth hazır olunca devreye girer. |
| S3 Nesne Depo | 1 saat | 4–6 saat | Video içerik write-once; tam senkronizasyon bant genişliğine bağlı (Bölüm 3). |
| CDN | RPO yok (cache) | Otomatik (~15 dk) | Veri kaynağı değil, S3 geri gelince kendiliğinden ısınır. |
| Vektör İndeksi / Ajan-RAG | 6 saat (kabul edilebilir) | 6+ saat (son öncelik) | Türetilen veri; PG+S3 tam hazır olmadan yeniden inşa edilemez. |

---

# 3. Bant Genişliği Notu (S3 RTO Gerekçesi)

Video kütüphanesi binlerce GB ölçeğinde olduğu için, S3'ün tam geri yükleme süresi ağ bant genişliği ile sınırlıdır:

- Varsayım: Toplam video kütüphanesi ~5.000 GB, yedekten geri yükleme bant genişliği ~1 Gbps (~125 MB/s)
- Teorik minimum süre: 5.000.000 MB ÷ 125 MB/s ≈ 11 saat (tam senkronizasyon için)
- "4–6 saat" RTO hedefi, en çok izlenen/kritik içeriğin öncelikli ve kademeli geri yüklenmesi anlamına gelir; tüm kütüphanenin %100 senkronizasyonu daha uzun sürebilir.

---

# 4. Kurtarma Sırası ile Tutarlılık

- PostgreSQL en düşük RPO/RTO değerine sahip → kurtarma sırasında 1. öncelik
- S3 daha yüksek RTO toleransına sahip → kademeli/paralel geri yükleme
- Vektör İndeksi/Ajan en son ve en toleranslı → son sıra

---

# 5. Sonuç

RPO/RTO hedefleri, PostgreSQL'in finansal veri içermesi nedeniyle dakika seviyesinde sıkı tutulmuş; S3'ün video içeriğinin toleranslı doğası ve bant genişliği kısıtı nedeniyle saat seviyesinde daha gevşek belirlenmiştir.