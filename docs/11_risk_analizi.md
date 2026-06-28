# Risk Analizi

## Doküman Bilgileri

| Alan | Değer |
|------|--------|
| Doküman Kodu | DOC-11 |
| Doküman Adı | Risk Analizi |
| Proje | Akademi Player Felaket Kurtarma Planı |
| Hazırlayan | Dilara Şenay |
| Görev | Teknik Lider & Analiz Mimarı |
| Sürüm | 1.0 |
| Tarih | 27.06.2026 |

---

# 1. Amaç

Bu dokümanın amacı, Akademi Player platformunda meydana gelebilecek olası riskleri belirlemek, bu risklerin iş sürekliliği üzerindeki etkilerini değerlendirmek ve alınabilecek önleyici tedbirleri ortaya koymaktır.

---

# 2. Risk Değerlendirme Metodolojisi

Riskler aşağıdaki iki kritere göre değerlendirilmiştir.

| Kriter | Açıklama |
|---------|----------|
| Olasılık | Riskin gerçekleşme ihtimali |
| Etki | Gerçekleşmesi durumunda iş süreçlerine etkisi |

Risk seviyesi, olasılık ve etki birlikte değerlendirilerek belirlenmiştir.

---

# 3. Risk Matrisi

| Risk | Olasılık | Etki | Risk Seviyesi |
|------|-----------|------|---------------|
| Bölgesel bulut kesintisi | Orta | Çok Yüksek | Kritik |
| PostgreSQL veritabanı arızası | Düşük | Çok Yüksek | Kritik |
| S3 nesne depolama erişim kaybı | Orta | Yüksek | Yüksek |
| Ağ bağlantısı kesintisi | Orta | Yüksek | Yüksek |
| Yanlış yapılandırma (Configuration Error) | Orta | Orta | Orta |
| İnsan kaynaklı operasyon hatası | Orta | Yüksek | Yüksek |
| Yedekleme başarısızlığı | Düşük | Çok Yüksek | Kritik |
| Güvenlik ihlali | Düşük | Çok Yüksek | Kritik |
| Donanım arızası | Düşük | Yüksek | Orta |

---

# 4. Risklerin İş Süreçlerine Etkisi

| Risk | İş Etkisi |
|------|-----------|
| Bölgesel Bulut Kesintisi | Platform tamamen hizmet dışı kalır. |
| PostgreSQL Arızası | Kullanıcı verileri ve kurs ilerlemeleri erişilemez hale gelir. |
| S3 Kesintisi | Eğitim videoları ve medya içeriklerine erişim sağlanamaz. |
| Ağ Problemleri | Kullanıcı bağlantıları kesilebilir. |
| Güvenlik İhlali | Veri gizliliği ve kurumsal itibar zarar görebilir. |
| İnsan Hatası | Kurtarma süresi uzayabilir ve yanlış işlemler yapılabilir. |

---

# 5. Risk Azaltma Önlemleri

| Risk | Alınacak Önlem |
|------|----------------|
| Bulut kesintisi | Çok bölgeli (Multi-Region) mimari kullanımı |
| PostgreSQL arızası | Düzenli yedekleme ve replikasyon |
| S3 kesintisi | Farklı bölgede yedek nesne depolama |
| Ağ kesintisi | Yedek internet bağlantıları ve yük dengeleme |
| İnsan hatası | Yazılı operasyon prosedürleri ve düzenli eğitim |
| Güvenlik ihlali | MFA, erişim kontrolü ve güvenlik izleme |
| Yedekleme hatası | Düzenli geri yükleme testleri |

---

# 6. Öncelikli Riskler

Felaket kurtarma planı kapsamında öncelikli olarak ele alınması gereken riskler aşağıdaki gibidir:

1. Bölgesel bulut kesintisi
2. PostgreSQL veritabanı arızası
3. Yedekleme başarısızlığı
4. Güvenlik ihlali
5. S3 nesne depolama erişim kaybı

Bu risklerin gerçekleşmesi durumunda platformun temel hizmetleri doğrudan etkilenmektedir.

---

# 7. Risk İzleme Süreci

Risklerin etkin şekilde yönetilebilmesi için aşağıdaki faaliyetler düzenli olarak yürütülmelidir.

- Sistem sağlık kontrollerinin sürekli izlenmesi
- Log ve alarm kayıtlarının düzenli incelenmesi
- Haftalık yedekleme doğrulama testleri
- Aylık felaket kurtarma tatbikatları
- Güvenlik açıklarının düzenli olarak taranması
- Risk değerlendirmesinin belirli aralıklarla güncellenmesi

---

# 8. Sonuç

Yapılan risk analizi sonucunda bölgesel bulut kesintisi, PostgreSQL veritabanı arızası, yedekleme başarısızlığı ve güvenlik ihlalleri Akademi Player platformu için en kritik riskler olarak belirlenmiştir. Bu risklere yönelik önleyici tedbirlerin uygulanması, düzenli yedekleme ve felaket kurtarma tatbikatlarının gerçekleştirilmesi iş sürekliliğinin sağlanması açısından büyük önem taşımaktadır.