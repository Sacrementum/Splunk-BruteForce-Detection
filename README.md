# Splunk SOC Analyst Lab: Brute-Force Detection
*Turkish translation is available below / Türkçe çevirisi aşağıdadır.*

## 🇬🇧 English - Objective
The goal of this project was to set up a local Security Information and Event Management (SIEM) environment using Splunk, ingest authentic Windows Event logs, and perform threat hunting to detect a Brute-Force attack. Finally, a custom alert was created to automate the detection of this specific threat.

### Tools & Environment
* **SIEM:** Splunk Enterprise
* **OS:** Windows 10
* **Log Source:** EVTX Attack Samples (Event ID 4625 - Failed Logon)

### Step 1: Data Ingestion
I downloaded authentic Windows security logs containing malicious activity (Brute-Force attempts) and successfully ingested the `.evtx` file into Splunk to create a realistic SOC analysis environment.

<img src="1-data ingestion.png">

### Step 2: Threat Hunting with SPL
Using Splunk Processing Language (SPL), I filtered the raw data to isolate failed logon attempts. 
* **Query used:** `index="main" EventCode=4625`

Through the analysis of the logs, I identified the targeted account and determined the attack vector:
* **Target Account Name:** `IEUser`
* **Source Network Address:** `-` (Indicates a local attack originating from the host machine itself, rather than an external IP).

<img src="2 spl-search.png.png">

### Step 3: Custom Rule Creation
To transition from manual hunting to automated detection, I wrote a custom SPL rule designed to trigger when multiple failed logon attempts occur on the same host, indicating a Brute-Force attack.
* **Rule Query:** `index="main" EventCode=4625 | stats count by host | where count >= 1`

<img src="3 Custom rule.png">

### Step 4: Alert Configuration
Finally, I configured Splunk to generate a High-Severity alert based on the custom rule. The system is now automated to detect and report repeated failed logon attempts.
* **Alert Title:** High Severity: Multiple Failed Logon Attempts

<img src="4 Alert.png">

## Conclusion
This lab demonstrates practical, hands-on experience with Splunk, covering the entire lifecycle of a SOC analyst's daily tasks: from data ingestion and SPL querying to log analysis and automated threat detection.

---

## 🇹🇷 Türkçe - Amacımız
Bu projenin amacı; Splunk üzerinde yerel bir SIEM ortamı kurmak, gerçek Windows Event (Olay) loglarını sisteme aktarmak ve bir Brute-Force (Kaba Kuvvet) saldırısını yakalamak için tehdit avcılığı (threat hunting) yapmaktır. Son aşamada ise bu saldırıyı otomatik olarak yakalayacak özel bir alarm oluşturulmuştur.

### Araçlar ve Ortam
* **SIEM:** Splunk Enterprise
* **İşletim Sistemi:** Windows 10
* **Log Kaynağı:** EVTX Saldırı Örnekleri (Event ID 4625 - Hatalı Giriş)

### 1. Aşama: Veri Aktarımı (Data Ingestion)
Gerçekçi bir SOC analiz ortamı oluşturmak için, Brute-Force denemeleri içeren orijinal Windows güvenlik loglarını (`.evtx` dosyası) indirip Splunk'a başarıyla entegre ettim.

<img src="1-data ingestion.png">

### 2. Aşama: SPL ile Tehdit Avcılığı
Ham veriler içindeki hatalı giriş (failed logon) denemelerini izole etmek için Splunk SPL kullanarak veriyi filtreledim.
* **Kullanılan Sorgu:** `index="main" EventCode=4625`

Logların analizi sonucunda, hedeflenen hesabı tespit ettim ve saldırı vektörünü belirledim:
* **Hedeflenen Hesap Adı:** `IEUser`
* **Kaynak Ağ Adresi:** `-` (Bu durum, saldırının dışarıdan bir IP yerine doğrudan makinenin kendi içinden (local) yapıldığını gösteriyor).

<img src="2 spl-search.png.png">

### 3. Aşama: Özel Kural Oluşturma (Custom Rule)
Manuel tehdit avcılığından otomatik tespite geçiş yapmak için, aynı makinede çok sayıda hatalı giriş denemesi (Brute-Force belirtisi) olduğunda tetiklenecek özel bir SPL kuralı yazdım.
* **Kural Sorgusu:** `index="main" EventCode=4625 | stats count by host | where count >= 1`

<img src="3 Custom rule.png">

### 4. Aşama: Alarm Konfigürasyonu
Son olarak, yazdığım bu kurala dayanarak Splunk'ta "High-Severity" (Yüksek Kritiklik) seviyesinde bir alarm yapılandırdım. Sistem artık art arda gelen hatalı giriş denemelerini otomatik olarak tespit edip raporluyor.
* **Alarm Başlığı:** Yüksek Kritiklik: Çoklu Hatalı Giriş Denemesi

<img src="4 Alert.png">

## Sonuç
Bu laboratuvar çalışması; veri aktarımından SPL sorgularına, log analizinden otomatik tehdit tespitine kadar bir SOC analistinin günlük görevlerinin tüm yaşam döngüsünü kapsayan pratik bir Splunk deneyimi sunmaktadır.
