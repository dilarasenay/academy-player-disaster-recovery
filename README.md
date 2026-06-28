# 🎯 Academy Player Disaster Recovery Planning

> PostgreSQL ve S3 tabanlı çevrim içi eğitim platformu için Disaster Recovery (Felaket Kurtarma) Planı

---

## 📌 Proje Hakkında

Bu proje, çevrim içi eğitim hizmeti sunan **Academy Player** isimli örnek platform için hazırlanmış kapsamlı bir **Disaster Recovery (Felaket Kurtarma) Planı** dokümantasyonudur.

Senaryo kapsamında, platformun çalıştığı bulut bölgesinde meydana gelen büyük ölçekli bir altyapı kesintisi sonucunda PostgreSQL veritabanı ve S3 uyumlu nesne depolama servisi aynı anda kullanılamaz hale gelmektedir.

Bu çalışma ile olası bir felaket durumunda sistemin güvenli şekilde yeniden ayağa kaldırılması, veri kaybının en aza indirilmesi ve iş sürekliliğinin sağlanması hedeflenmiştir.

---

# 🚨 Senaryo

Bulut sağlayıcısında yaşanan bölgesel kesinti nedeniyle;

* PostgreSQL veritabanı erişilemez hale gelmiştir.
* S3 uyumlu nesne depolama servisi devre dışı kalmıştır.
* Kullanıcılar sisteme giriş yapamamaktadır.
* Eğitim videolarına erişilememektedir.
* Kurs ilerleme bilgileri kaydedilememektedir.
* Satın alma işlemleri gerçekleştirilememektedir.

Bu senaryo doğrultusunda uçtan uca bir Felaket Kurtarma Planı hazırlanmıştır.

---

# 📂 Doküman İçeriği

| Doküman                      | Açıklama                                     |
| ---------------------------- | -------------------------------------------- |
| 01 - İş Kaybı Özeti          | Kesintinin iş süreçlerine etkisi             |
| 02 - Bileşen Envanteri       | Kritik sistem bileşenleri                    |
| 03 - Kritiklik Analizi       | Sistem bileşenlerinin önceliklendirilmesi    |
| 04 - Bağımlılık Diyagramı    | Servis bağımlılıkları                        |
| 05 - RPO / RTO               | Kurtarma hedefleri                           |
| 06 - Kurtarma Sırası         | Servislerin geri yükleme sırası              |
| 07 - Recovery Runbook        | Adım adım kurtarma süreci                    |
| 08 - Test Planı              | Felaket kurtarma testleri                    |
| 09 - İletişim Planı          | Kriz iletişim süreci                         |
| 10 - Müşteri Duyurusu        | Kullanıcı bilgilendirme metinleri            |
| 11 - Risk Analizi            | Olası risklerin değerlendirilmesi            |
| 12 - Gelecek İyileştirmeleri | Altyapının geliştirilmesine yönelik öneriler |

---

# 🏗️ Örnek Sistem Mimarisi

```text
Kullanıcılar
      │
      ▼
Load Balancer
      │
      ▼
Backend API
      │
 ┌────┴─────┐
 │          │
 ▼          ▼
PostgreSQL  S3 Object Storage
 │          │
 ▼          ▼
Backup     Backup
```

---

# 🔄 Disaster Recovery Süreci

1. Kesintinin tespit edilmesi
2. Kriz ekibinin bilgilendirilmesi
3. PostgreSQL veritabanının geri yüklenmesi
4. Kimlik doğrulama servislerinin devreye alınması
5. Backend API servislerinin başlatılması
6. S3 nesne depolama servisinin geri yüklenmesi
7. Uygulama testlerinin gerçekleştirilmesi
8. Kullanıcılara hizmetin yeniden açılması

---

# 🎯 Proje Hedefleri

* İş sürekliliğinin sağlanması
* Veri kaybının en aza indirilmesi
* Kritik servislerin hızlı şekilde yeniden devreye alınması
* RPO ve RTO hedeflerinin karşılanması
* Kriz yönetim sürecinin standartlaştırılması
* Felaket senaryolarına karşı hazırlıklı olunması

---

# 🛠️ Kullanılan Teknolojiler

* PostgreSQL
* Amazon S3 (S3 Compatible Object Storage)
* Git
* GitHub
* Markdown
* Disaster Recovery Planning
* Business Continuity Planning (BCP)

---

# 📌 Sonuç

Bu proje kapsamında, Academy Player platformu için kapsamlı bir Felaket Kurtarma Planı hazırlanmış; olası bölgesel bulut kesintilerinde uygulanabilecek teknik ve operasyonel süreçler dokümante edilmiştir. Hazırlanan çalışma sayesinde sistem bileşenleri analiz edilmiş, kurtarma öncelikleri belirlenmiş, iletişim planları oluşturulmuş ve iş sürekliliğini destekleyecek öneriler geliştirilmiştir.

---

> Bu proje eğitim ve dokümantasyon amacıyla hazırlanmıştır. Kullanılan marka, sistem ve senaryolar örnekleme amacı taşımaktadır.

