# Sistem Bileşen Envanteri

## Doküman Bilgileri

| Alan | Değer |
|------|-------|
| Doküman Kodu | DOC-02 |
| Doküman Adı | Sistem Bileşen Envanteri |
| Proje | Akademi Player Felaket Kurtarma Planı |
| Hazırlayan | Ahmet Özdoğan |
| Görev | Cloud & Altyapı Mimarı |
| Sürüm | 1.0 |
| Tarih | 27.06.2026 |

---

## 1. Amaç

Bu dokümanın amacı, Akademi Player platformunu oluşturan sistem bileşenlerini listelemek; her bileşenin iş sürekliliği açısından kritiklik seviyesini, bağımlılıklarını ve geri yükleme duyarlılığını belirlemektir.

---

## 2. Kapsam

Bu envanter aşağıdaki bileşenleri kapsamaktadır:

- PostgreSQL Veritabanı
- Backend API
- S3 Uyumlu Nesne Depolama
- Kimlik Doğrulama Servisi
- CDN
- İzleme ve Loglama Servisleri
- Yedekleme Altyapısı
- Ajan Bileşeni / RAG Destek Botu

---

## 3. Bileşen Envanteri

| Bileşen | Görevi | Kritiklik | Kesinti Durumu |
|---|---|---|---|
| PostgreSQL | Kullanıcı, kurs, ödeme ve ilerleme verilerini saklar | Kritik | Hizmet dışı |
| Backend API | İş kurallarını ve servisler arası iletişimi yönetir | Kritik | Veritabanı olmadan çalışamaz |
| S3 Nesne Depolama | Video, poster ve eğitim dosyalarını saklar | Kritik | Hizmet dışı |
| Kimlik Doğrulama | Kullanıcı giriş ve oturum işlemlerini yönetir | Kritik | Veritabanına bağımlıdır |
| CDN | Statik içeriklerin hızlı dağıtımını sağlar | Orta | S3 erişimi olmadan sınırlıdır |
| İzleme Sistemi | Alarm, log ve sağlık kontrolü üretir | Yüksek | Kesintiyi tespit eder |
| Yedekleme Sistemi | Geri yükleme için snapshot ve arşiv sağlar | Kritik | Kurtarma sürecinde kullanılır |
| RAG Destek Botu | İç destek sorularına yanıt verir | Orta | Vektör indeksi ve önbelleğe bağımlıdır |

---

## 4. Bağımlılık Özeti

| Bileşen | Bağımlı Olduğu Servis |
|---|---|
| Frontend | Backend API |
| Backend API | PostgreSQL, S3, Kimlik Doğrulama |
| Kimlik Doğrulama | PostgreSQL |
| CDN | S3 Nesne Depolama |
| RAG Destek Botu | Vektör İndeksi, Araç Çıktısı Önbelleği |
| İzleme Sistemi | Tüm sistem bileşenleri |

---

## 5. Geri Yükleme Duyarlılığı

| Bileşen | Duyarlılık | Gerekçe |
|---|---|---|
| PostgreSQL | Çok Yüksek | Platformun temel veri kaynağıdır |
| Kimlik Doğrulama | Çok Yüksek | Kullanıcı erişimi için zorunludur |
| Backend API | Yüksek | İş süreçlerini tekrar çalıştırır |
| S3 Nesne Depolama | Yüksek | Video içerikleri için gereklidir |
| CDN | Orta | Performans ve erişim kalitesini etkiler |
| RAG Destek Botu | Orta | İç destek süreçlerini etkiler |
| İzleme Sistemi | Yüksek | Kurtarma sürecinin doğrulanması için gereklidir |

---

## 6. Sonuç

Sistem bileşenleri incelendiğinde PostgreSQL, kimlik doğrulama, Backend API ve S3 nesne depolama servislerinin platformun en kritik bileşenleri olduğu görülmektedir. Bu bileşenlerin geri yüklenme sırası, hem iş sürekliliği hem de RPO/RTO hedefleri açısından belirleyicidir.