# Exploratory Analysis

Bu klasör, **01_data_pipeline** aşamasında üretilmiş olan temiz ve tip-güvenli verinin, analitik olarak **anlamlı, tutarlı ve güvenilir** olup olmadığını doğrulamak amacıyla oluşturulmuştur.

Bu aşamanın temel soruları:

* Veri gerçek dünyadaki dağılımlarla uyumlu mu?
* Aykırı, dengesiz veya analizi bozabilecek yapılar var mı?
* İlçe, rating ve rekabet gibi temel değişkenler makul davranıyor mu?

> Bu katmanda **herhangi bir karar, skor optimizasyonu veya modelleme yapılmaz**.
> Amaç yalnızca **durum tespiti (descriptive & diagnostic analysis)** yapmaktır.

---

## Kullanılan Tablolar

Bu klasörde yalnızca aşağıdaki tablolar kullanılmıştır:

* `clean.cafes`
* `mart.district_summary`
* `mart.cafe_competition_2km`

Bu seçim bilinçlidir:

* Tüm tablolar **01_data_pipeline** tarafından üretilmiştir
* Hiçbiri downstream karar veya opportunity modeline ait değildir

---

## 1. Pazar Genel Görünümü (KPI Seviyesi)

### İncelenen Göstergeler
<img width="1151" height="88" alt="Ekran Resmi 2026-02-13 12 32 49" src="https://github.com/user-attachments/assets/d942740f-1bd8-495f-a0a0-1aa1d8345ec7" />

* Toplam cafe sayısı
* Ortalama rating
* Yüksek rating’li (≥ 4.5) cafe sayısı
* Ortalama review sayısı
* Operasyonel işletme oranı

Bu göstergeler, İstanbul cafe pazarının **ölçeğini ve genel kalite seviyesini** anlamak için kullanılır.

### Analitik Yorum

* Cafe sayısının yüksek olması, analiz edilen pazarın **rekabetçi ve doygun** bir yapıya sahip olduğunu gösterir.
* Ortalama rating’in 4+ seviyesinde olması, Google Places verisinde **pozitif bias** olabileceğine işaret eder. Bu durum ilerleyen aşamalarda mutlaka dikkate alınmalıdır.
* Operasyonel oranının %95+ olması, verinin büyük ölçüde **aktif işletmeleri** temsil ettiğini gösterir.

Bu metrikler, henüz **hangi ilçede ne yapılmalı** sorusuna cevap vermez; yalnızca pazarın genel fotoğrafını çeker.

---

## 2. İlçe Bazında Cafe Dağılımı



### Görsel: Total Number of Cafes by District
<img width="602" height="458" alt="Ekran Resmi 2026-02-13 14 37 49" src="https://github.com/user-attachments/assets/34aa222f-7bdf-47b4-8b98-494cccf1be3a" />

Bu analizde, `clean.cafes` tablosu kullanılarak ilçe bazında cafe sayıları incelenmiştir.

### Gözlemler

* Cafe sayısının ilçeler arasında **yüksek varyans** gösterdiği gözlemlenmektedir.
* Merkezi ve turistik ilçelerde belirgin bir yoğunlaşma vardır.
* Bazı ilçelerde düşük cafe sayısı, ilerleyen aşamalarda **arz eksikliği mi yoksa düşük talep mi?** sorusunu gündeme getirir.

Bu aşamada bu soruya cevap verilmez; sadece **farklılık tespit edilir**.

---

## 3. Rating ve Review Dağılımları

### 3.1 Rating Distribution (Fine-Grain)
<img width="602" height="451" alt="Ekran Resmi 2026-02-13 14 37 32" src="https://github.com/user-attachments/assets/752516ce-d8ec-4a04-ac32-e7aa9fa9e8a7" />

Rating dağılımı incelendiğinde:

* 4.0 – 4.8 aralığında yoğun bir kümelenme görülmektedir
* Düşük rating’li cafe sayısı görece azdır

Bu durum:

* Kullanıcıların memnuniyet bildiriminde **seçici** olduğunu
* Ya da düşük rating’li işletmelerin zamanla platformdan silindiğini

ima edebilir.


---

### 3.2 Review Count Distribution

Review sayısı dağılımı **sağa çarpık (right-skewed)** bir yapı sergilemektedir:

* Çok sayıda cafe düşük review sayısına sahiptir
* Az sayıda cafe çok yüksek review hacmi üretmektedir

Bu gözlem, ilerleyen analizlerde:

* Log-transform
* Güvenilirlik eşikleri (ör. minimum review sayısı)

kullanılmasının neden gerekli olduğunu açıkça göstermektedir.

---

## 4. 2km Rekabet Dağılımı (Mikro Düzey)

### Görsel: 2km Competition Distribution
<img width="600" height="458" alt="Ekran Resmi 2026-02-13 14 38 38" src="https://github.com/user-attachments/assets/aad31834-508d-4cdd-8352-22ecfeb57d20" />

Bu analizde, her bir cafe için 2km yarıçap içinde bulunan rakip sayısı incelenmiştir.

### Gözlemler

* Rekabet seviyesi geniş bir aralığa yayılmaktadır
* Bazı cafeler çok düşük rekabet ortamında faaliyet gösterirken
* Bazıları yüzlerce rakiple çevrilidir

Bu bulgu:

* İstanbul’da cafe rekabetinin **homojen olmadığını**
* Mekânsal analizlerin neden kritik olduğunu

göstermektedir.



