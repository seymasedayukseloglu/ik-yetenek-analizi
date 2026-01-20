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

## 🛠️ 1. Veri Toplama ve Veri Setinin Oluşturulması

Bu aşamada, projenin temelini oluşturan **16.718 adet** iş ilanı, çok kanallı bir veri toplama mimarisiyle elde edilmiştir. Veri toplama süreci; dinamik içerikli sayfalar için **Selenium**, hızlı veri aktarımı için **API** ve verimlilik için **Multi-threading** (paralel işleme) teknikleri kullanılarak optimize edilmiştir.

### 🌐 Veri Kaynakları ve Metodoloji

Projede kullanılan veri kaynakları ve tercih edilen toplama yöntemleri aşağıdaki tabloda özetlenmiştir:

| Kaynak Platform | Yöntem | Toplanan Veri | Teknik Detay |
| :--- | :--- | :--- | :--- |
| **LinkedIn** | Selenium | Global & Yerel İlanlar | Dinamik scroll ve `job-card` analizi |
| **Kariyer.net** | Selenium (Undetected) | Sektörel Pozisyonlar | `undetected-chromedriver` ile bot engelini aşma |
| **Himalayas API** | REST API | Global Uzaktan Çalışma | JSON veri parsing ve sistematik offset yönetimi |
| **Eleman.net** | Hybrid (Selenium + BS4) | Kitle İlanları | `ThreadPoolExecutor` ile 15 kat daha hızlı veri çekme |
| **Yenibiriş** | Selenium | Kurumsal İlanlar | `ActionChains` ile hover-over (üzerine gelme) etkileşimleri |

---

### 💻 Teknik Uygulama Detayları

#### A. Dinamik İçerik Yönetimi ve Bot Güvenliği
Kariyer.net ve LinkedIn gibi platformlarda karşılaşılan bot korumalarını aşmak için **Undetected Chromedriver** kullanılmıştır. `time.sleep()` stratejileri ve manuel checkpoint'ler eklenerek veri çekme sürecinin sürekliliği sağlanmıştır.

#### B. Performans Optimizasyonu (Multi-threading)
Eleman.net gibi binlerce linkin taranması gereken aşamalarda, standart sıralı çekme yerine **Python `concurrent.futures`** modülü kullanılmıştır. 
* **Etki:** 15 farklı "worker" aynı anda çalıştırılarak veri toplama süresi saatlerden dakikalara indirilmiştir.

```python
# Kullanılan Paralel İşleme Mimarisi
with ThreadPoolExecutor(max_workers=15) as executor:
    sonuclar = list(executor.map(ilan_detay_cek, ilan_linkleri))
<h3>🔍 C. Akıllı Beceri Ayıklama (Keyword Mapping)</h3>
<p>
  Henüz ham veri aşamasındayken, ilan metinleri içerisinde geçen yetkinlikler önceden tanımlanmış bir <b>yetenek sözlüğü (dictionary)</b> üzerinden taranmıştır. 
  Bu işlem, yapılandırılmamış metin verisini istatistiksel analize uygun bir <b>Binary (0/1) Matrisine</b> dönüştürmüştür.
</p>

<table role="presentation" style="background-color: #f6f8fa; padding: 10px; border-radius: 6px; width: 100%;">
  <tr>
    <td>
      <b>📌 Not:</b> Toplanan tüm veriler; <i>İlan Başlığı, Şirket, Lokasyon, Teknik Beceriler</i> ve <i>Sosyal Beceriler</i> kolonları altında <code>ik_yetenek_analizi_veriseti.csv</code> ismiyle tek bir merkezde birleştirilmiştir.
    </td>
  </tr>
</table>

<br />
<hr />
