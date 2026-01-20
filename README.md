<div align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/Selenium-43B02A?style=for-the-badge&logo=selenium&logoColor=white" />
  <img src="https://img.shields.io/badge/Data_Mining-FF6F00?style=for-the-badge&logo=google-cloud&logoColor=white" />
  <br>
  <h1>🚀 İnsan Kaynakları Yetenek Analizi</h1>
  <h3>Web Madenciliği Dersi Proje Çalışması</h3>
  <p><b>16.000+ İş İlanı Üzerinden Birliktelik Kuralları (Apriori) ile Beceri Madenciliği</b></p>
</div>

---

## 📋 1. Proje Özeti ve Giriş

Bu çalışma, **Web Madenciliği** disiplininin veri toplama, temizleme ve anlamlandırma süreçlerini uçtan uca kapsamaktadır. Proje kapsamında, Türkiye’nin en büyük iş ilanı platformlarından (LinkedIn, Kariyer.net, Yenibiriş, Eleman.net vb.) **API** ve **Selenium** tabanlı web scraping yöntemleri kullanılarak toplam **16.718 adet** güncel iş ilanı çekilmiştir.

Elde edilen devasa veri kümesi; Python kütüphaneleri ile standardize edilerek, iş dünyasının "yetenek haritasını" çıkarmak için analiz edilmiştir. Çalışma, sadece akademik bir egzersiz değil, İK profesyonelleri için veri temelli bir karar destek mekanizması niteliğindedir.

### 👤 Geliştirici Bilgileri
* **Hazırlayan:** Şeyma Seda Yükseloğlu
* **Ders:** Web Madenciliği
* **Teknoloji Yığını:** Python (Pandas, Selenium, Mlxtend), Apriori Algoritması, Veri Görselleştirme.

---

## 🎯 2. Problem Tanımı ve Analiz Amacı

Günümüz İK süreçlerinde "ideal aday" tanımı genellikle subjektif yorumlara dayanmaktadır. Bu proje, bu önyargıları kırarak aşağıdaki kritik iş sorularına **16.718 verinin gücüyle** yanıt aramaktadır:

### 🔍 Temel Sorular:

1.  **Stratejik Eğitim Yatırımı:** Şirketler bütçelerini teknik araçlara (Python, SQL) mı yoksa sosyal yetkinliklere (İletişim, Liderlik) mi ayırmalı? 
    * *Bulgu:* İlanların **%88'inde** sosyal becerilerin baskın olması, yatırımın yönünü belirlemektedir.
  
2.  **Unicorn Aday Efsanesi:** Sektörde "her şeyi bilen" (Unicorn) adaylar mı aranıyor? 
    * *Bulgu:* 6 ve üzeri beceri isteyen ilanların oranının **%0.01** olması, piyasanın sanılanın aksine daha rasyonel olduğunu göstermektedir.

3.  **Yetenek Birliktelikleri:** Hangi yetkinlikler ayrılmaz bir bütün haline gelmiştir?
    * *Yöntem:* **Apriori Algoritması** kullanılarak beceriler arasındaki güven (confidence) ve destek (support) ilişkileri matematiksel olarak modellenmiştir.

### 🛠️ Analitik Yaklaşım:
Veriler ham metin formatından **Binary (0/1)** matris formatına dönüştürülmüş, karma beceriler modellenmiş ve işveren beklentilerinin gerçekçiliği sektör standartlarıyla kıyaslanmıştır.

---
