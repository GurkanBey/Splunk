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

2. Indexer (Veri Dizinleyici)
Gelen logları işler, olaylara ayırır, indeksler ve saklar. En kritik işlevleri:
	• Timestamp çıkarımı
	• Field extraction (alan çıkarımı)
	• Event breaking (olay ayırma)

3. Search Head (Arama ve Görselleştirme Katmanı)
	• Kullanıcının dashboard, alarm ve rapor oluşturduğu katmandır.
	• Arama taleplerini indexer’lara dağıtır, sonuçları birleştirip gösterir.
	• SPL (Splunk Process Language) sorgulamaları bu aşamada yapılır.
	• SPL kullanımı ile toplanan loglar üzerinde en detaylı sonuçlara ulaşmak mümkündür.

4. Cluster Master ve Deployment Server (Opsiyonel)
	• Cluster Master: Indexer cluster'larının yönetiminden sorumludur (replikasyon, failover vb.).
	• Deployment Server: Yüzlerce forwarder varsa, merkezi konfigürasyon sağlar.
<img width="806" height="830" alt="image" src="https://github.com/user-attachments/assets/f3256ee7-b36a-4b0e-9959-68d34f5a4093" />
