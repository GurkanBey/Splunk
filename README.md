Splunk SIEM Nedir? Temeller ve Mimarisiyle Giriş (1. Bölüm)  

Siber güvenlik dünyasında tehditler giderek karmaşıklaşırken, kurumlar bu tehditleri gerçek zamanlı olarak tespit etmek ve yanıtlamak için güçlü araçlara ihtiyaç duyuyor. Bu araçlardan biri de SIEM sistemleridir. Bu yazı serisinin ilk bölümünde, SIEM dünyasının en çok tercih edilen çözümlerinden biri olan Splunk’a temel düzeyde bir giriş yapacağız. Splunk nedir, nasıl çalışır, mimarisi nasıldır, neden bu kadar popülerdir? Gelin birlikte inceleyelim.  

📌 SIEM Nedir?  
SIEM, Security Information and Event Management anlamına gelir ve temel olarak, bir işletmenin altyapısındaki güvenlikle ilgili tüm olayları izlemek ve analiz etmek için tüm kullanıcılar, sunucular, ağ cihazları ve güvenlik duvarları tarafından oluşturulan günlükleri ve olay verilerini toplayan güvenlik yazılımıdır.  

SIEM (Security Information and Event Management), temel olarak şu adımlarla çalışır:  
	• Veri Toplama (Collect): Ağ cihazları, sunucular, uygulamalar gibi pek çok kaynaktan loglar toplanır.  
	• Normalize & Korelasyon: Toplanan veriler tek bir standart formatta normalize edilir.  
	• Analiz & Tespit: Anormal davranışlar veya tehditler analiz edilir.  
	• Uyarı & Yanıt: Şüpheli aktiviteler tespit edildiğinde, alarmlar üretilir ve güvenlik ekipleri aksiyon alır.  

🟧 Splunk Nedir?  
Splunk, ilk etapta bir log yönetim ve analiz platformu olarak doğsa da bugün geldiği noktada SIEM, SOAR, APM gibi pek çok alanda çözümler sunan devasa bir platform haline gelmiştir. Splunk, yapılandırılmış ya da yapılandırılmamış tüm verileri işleyip anlamlandırılmış bir çıktı üretir, tehdit tespiti yapar ve görselleştirir.  

🏗️ Splunk Mimarisi ve Temel Bileşenleri  
Splunk’un SIEM altyapısı oldukça modüler ve ölçeklenebilirdir. Temel bileşenlerini aşağıda kısaca açıklayalım:  

1. Forwarder (Veri Toplayıcı)  
Veriyi kaynak sistemden toplayıp Splunk ortamına gönderir. İki türü vardır:  
	• Universal Forwarder (UF): Hafiftir, sadece veri iletir.  
	• Heavy Forwarder (HF): Veriyi iletmeden önce ön işleme (parse, filter) yapabilir.  

3. Indexer (Veri Dizinleyici)
Gelen logları işler, olaylara ayırır, indeksler ve saklar. En kritik işlevleri:  
	• Timestamp çıkarımı  
	• Field extraction (alan çıkarımı)  
	• Event breaking (olay ayırma)  

5. Search Head (Arama ve Görselleştirme Katmanı)  
	• Kullanıcının dashboard, alarm ve rapor oluşturduğu katmandır.  
	• Arama taleplerini indexer’lara dağıtır, sonuçları birleştirip gösterir.  
	• SPL (Splunk Process Language) sorgulamaları bu aşamada yapılır.  
	• SPL kullanımı ile toplanan loglar üzerinde en detaylı sonuçlara ulaşmak mümkündür.

7. Cluster Master ve Deployment Server (Opsiyonel)  
	• Cluster Master: Indexer cluster'larının yönetiminden sorumludur (replikasyon, failover vb.).
	• Deployment Server: Yüzlerce forwarder varsa, merkezi konfigürasyon sağlar.


## ⚙️ Cluster Mimarisi: Dağıtık Yapı ve Yüksek Erişilebilirlik

### 📌 Indexer Cluster:
- Veriler birden fazla indexer’da replikasyon faktörüne göre çoğaltılır.
- Bir indexer çökerse, diğerinde veri aynen mevcuttur.
- Ölçeklenebilirlik ve veri bütünlüğü açısından kritiktir.
- Ancak bu yapı yüksek bir maliyet oluşturmaktadır.

### 📌 Search Head Cluster:
- Kullanıcı sorguları birden fazla node’a dağıtılır.
- Yük dengelenir, hizmet kesintisi önlenir.
- Aralarında bir node “Captain” olarak yönetici rolü üstlenir.
- Birden fazla sorgulama işlemi gerçekleştirilebilir.

---

## 💡 Peki Cluster Mimarisi Nedir?  

### 🔸Cluster nedir?  
- En basit haliyle cluster, birden fazla sunucunun (node) tek bir sistem gibi birlikte çalıştığı bir yapıdır.  

### 🔸Amaçları  
- Yük paylaşımı (load balancing): İş yükü birden fazla sunucuya dağıtılır.  
- Yüksek erişilebilirlik (high availability): Bir sunucu çökse bile hizmet devam eder.  
- Ölçeklenebilirlik (scalability): Yeni sunucular eklenerek kapasite artırılır.  
- Hata toleransı (fault tolerance): Veriler veya işlemler yedeklenerek kayıp önlenir.  

### 🔸Cluster’ın temel yapısı  
- Bir cluster genelde aşağıdaki parçalardan oluşur:  

#### 🔹 Node (Düğüm)  
- Cluster içindeki tek bir sunucuya "node" denir.  
- Örneğin: 5 sunucudan oluşan bir indexer cluster varsa, her biri bir "node"tur.  

#### 🔹 Master/Coordinator  
- Cluster’ın yöneticisidir.  
- Görevleri:  
  - Hangi node hangi işi yapacak, koordine etmek  
  - Replikasyon kurallarını yönetmek  
  - Sağlık durumlarını izlemek  

#### 🔹 Replication (Kopyalama)  
- Cluster’daki verilerin birden fazla node’da yedeği tutulur.  
- Örneğin: Replikasyon faktörü (RF) 3 ise, her veri 3 farklı node’a kopyalanır.  

---

## 🔁 Splunk Cluster'da Veri Akışı Nasıl Gerçekleşir?  

Splunk mimarisinde log verisinin cluster içinde nasıl işlendiğini basit bir örnek üzerinden inceleyelim:  

- 🔸Forwarder log verisini gönderir.  
  - Kaynak sistemdeki loglar, Universal veya Heavy Forwarder aracılığıyla Splunk ortamına iletilir.  

- 🔸Veri bir Indexer’a ulaşır.  
  - Bu ilk Indexer, logları işler, timestamp çıkarır, alanları ayırır ve dizinleme işlemini başlatır.  

- 🔸Replikasyon başlar.  
  - İlk Indexer, işlediği verinin bir kopyasını başka bir Indexer’a gönderir. Böylece veri iki farklı yerde saklanmış olur.  

- 🔸Node'lardan biri arızalansa bile veri kaybolmaz.  
  - Replikasyon sayesinde veri bütünlüğü korunur, sistem yüksek erişilebilirliğe sahip olur.  

- 🔸Search Head, arama sorgularını Indexer node’lara gönderir.  
  - Kullanıcı tarafından yapılan bir arama, Search Head tarafından cluster’daki tüm Indexer’lara dağıtılır. Her Indexer kendi verisini tarar, sonuçlar toplanır ve kullanıcıya sunulur.  

---

## 🧩 Splunk SIEM’in Artı ve Eksileri  

| ✅ Avantajları                              | ❌ Zorluklar                           |
|---------------------------------------------|-----------------------------------------|
| • Büyük veri hacmini hızlı işler            | • Yüksek lisans ve donanım maliyeti     |
| • SPL dili ile esnek arama ve korelasyon    | • Kurulum ve yapılandırma karmaşıktır   |
| • Modüler, ölçeklenebilir mimari            | • Özelleştirme zaman alabilir           |
| • Güçlü dashboard ve raporlama              |                                         |

## 🟦 Splunk ve UEBA  

- UEBA (User and Entity Behavior Analytics), kullanıcıların ve sistem varlıklarının normal davranış kalıplarını öğrenip, bu kalıplardan sapmaları tespit eden bir güvenlik analiz yaklaşımıdır.  
- Özellikle klasik SIEM kuralları, sabit eşik değerlerine (ör: 5 başarısız giriş denemesi) dayanır. Fakat UEBA:  
  - Davranış temelli çalışır (ör: kullanıcının her gün sabah 9’da İstanbul’dan bağlanması normal, ama gece 3’te Çin’den bağlanması anormal).  
  - Makine öğrenmesi ve istatistiksel modellemeyi kullanır.  
  - Hem kullanıcılar (user) hem de varlıklar (entity) için analiz yapılır (ör: sunucular, IoT cihazları, servis hesapları).  
- Splunk, klasik SIEM işlevlerinin yanında UEBA (User and Entity Behavior Analytics) modülüyle kullanıcı ve sistem davranışlarını da öğrenir.  
- Makine öğrenmesi ile desteklenen bu yapı, iç tehditleri ve anomali tespitini daha etkili hale getirir.  

---

## 🎯 Sonuç ve Gelecek Bölüm  

Bu yazıda Splunk’un temel yapısını ve SIEM mimarisini özetledik. Bir sonraki bölümde artık SPL (Search Processing Language) ile Splunk üzerinde temel log analizi nasıl yapılır, nasıl dashboard ve uyarılar tanımlanır, örneklerle öğreneceğiz.  


