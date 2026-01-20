<html lang="tr">
<head>
    <meta charset="UTF-8">
    <title>İnsan Kaynakları Yetenek Analizi | Birliktelik Kuralları</title>
    <style>
        body {
            font-family: Arial, Helvetica, sans-serif;
            line-height: 1.6;
            margin: 40px;
        }
        h1, h2, h3 {
            color: #2c3e50;
        }
        ul {
            margin-left: 20px;
        }
        .highlight {
            background-color: #f4f6f7;
            padding: 15px;
            border-left: 5px solid #3498db;
        }
        .result {
            background-color: #eafaf1;
            padding: 15px;
            border-left: 5px solid #2ecc71;
        }
        .warning {
            background-color: #fdecea;
            padding: 15px;
            border-left: 5px solid #e74c3c;
        }
    </style>
</head>

<body>

<h1>İnsan Kaynakları Yetenek Analizi</h1>
<h3>Birliktelik Kuralları (Apriori) ile İş İlanı Tabanlı Beceri İncelemesi</h3>

<hr>

<h2>GİRİŞ</h2>
<p>
Günümüzde insan kaynakları yönetimi, yalnızca aday sayısını artırmaya değil,
<strong>doğru yetenek bileşimini</strong> belirlemeye odaklanmaktadır.
Bu projede, Türkiye’deki farklı iş ilanı platformlarından toplanan veriler
kullanılarak işverenlerin hangi becerileri ne ölçüde talep ettiği analiz edilmiştir.
</p>

<p>
Çalışmanın temel amacı;
<ul>
    <li>Eğitim bütçelerinin teknik mi yoksa sosyal becerilere mi yönlendirilmesi gerektiğini belirlemek,</li>
    <li>Piyasada “unicorn” olarak adlandırılan aşırı nitelik talep eden ilanların oranını ölçmek,</li>
    <li>Bazı becerilerin artık birlikte talep edilip edilmediğini birliktelik kurallarıyla incelemektir.</li>
</ul>
</p>

<hr>

<h2>1. Veri Toplama ve Veri Setinin Oluşturulması</h2>

<p>
Veriler, farklı platformlardan <strong>Selenium tabanlı web scraping</strong> ve
<strong>API</strong> yöntemleri kullanılarak toplanmıştır.
Her platform için ayrı veri toplama betikleri geliştirilmiştir.
</p>

<ul>
    <li>LinkedIn – Selenium</li>
    <li>Kariyer.net – Selenium</li>
    <li>Yenibiriş – Selenium</li>
    <li>Eleman.net – Selenium</li>
    <li>API tabanlı ilan kaynakları</li>
</ul>

<p>
Toplanan tüm CSV dosyaları Python kullanılarak tek bir veri setinde birleştirilmiş,
tekrar eden ilanlar temizlenmiştir.
</p>

<div class="highlight">
<strong>Toplam analiz edilen ilan sayısı:</strong> 16.718
</div>

<hr>

<h2>2. Veri Ön İşleme ve Analize Hazırlık</h2>

<p>
Farklı kaynaklardan gelen veriler sütun isimleri ve içerik açısından
birbirinden farklı olduğu için kapsamlı bir ön işleme süreci uygulanmıştır.
</p>

<h3>2.1 Sütun Standardizasyonu</h3>
<ul>
    <li>Pozisyon, şirket, lokasyon ve açıklama alanları tek tip hale getirildi</li>
    <li>Eksik alanlar “Belirtilmemiş” olarak dolduruldu</li>
</ul>

<h3>2.2 Beceri Dönüşümü</h3>
<p>
Aşağıdaki beceriler için her ilan binary (0/1) formata dönüştürülmüştür:
</p>

<ul>
    <li>Teknik: Python, SQL, Excel, Agile</li>
    <li>Sosyal: İletişim, Liderlik, Takım Çalışması</li>
    <li>Karma: Analiz, İngilizce</li>
</ul>

<p>
Ayrıca her ilan için toplam istenen beceri sayısını gösteren
<strong>Beceri_Sayısı</strong> değişkeni oluşturulmuştur.
</p>

<hr>

<h2>3. Genel Beceri Talep Analizi</h2>

<p>
Beceri frekanslarına bakıldığında sosyal becerilerin teknik becerilere
kıyasla çok daha fazla talep edildiği görülmektedir.
</p>

<div class="result">
<strong>En çok talep edilen beceriler:</strong>
<ul>
    <li>Takım Çalışması</li>
    <li>İletişim</li>
    <li>Liderlik</li>
</ul>
</div>

<p>
Python, SQL ve Agile gibi teknik becerilerin ilanlarda çok sınırlı yer alması,
işverenlerin teknik detaydan çok adayın davranışsal özelliklerine
öncelik verdiğini göstermektedir.
</p>

<hr>

<h2>4. Teknik vs Sosyal Beceri Karşılaştırması</h2>

<p>
Beceriler kategorilere ayrıldığında aşağıdaki dağılım ortaya çıkmaktadır:
</p>

<ul>
    <li><strong>Sosyal beceriler:</strong> %88</li>
    <li><strong>Teknik beceriler:</strong> %12</li>
</ul>

<div class="result">
<strong>İşletmeler için öneri:</strong><br>
Eğitim bütçesinin büyük kısmı iletişim, liderlik ve takım çalışması
gibi sosyal becerilere ayrılmalıdır.
</div>

<hr>

<h2>5. Unicorn Profil Analizi</h2>

<p>
Unicorn profil, bir ilanda <strong>6 veya daha fazla becerinin</strong>
aynı anda talep edilmesi olarak tanımlanmıştır.
</p>

<div class="result">
<ul>
    <li>Unicorn ilan sayısı: 1</li>
    <li>Toplam ilanlara oranı: %0.01</li>
</ul>
</div>

<p>
Bu sonuç, incelenen ilanların büyük çoğunluğunun sektör standartlarıyla
uyumlu olduğunu ve işverenlerin gerçekçi beklentilere sahip olduğunu göstermektedir.
</p>

<hr>

<h2>6. Birliktelik Kuralları (Apriori) Analizi</h2>

<p>
Beceri kombinasyonlarını analiz etmek için Apriori algoritması uygulanmıştır.
Ancak düşük teknik beceri sıklığı nedeniyle güçlü birliktelik kuralları
elde edilememiştir.
</p>

<div class="highlight">
Bu durum, ilanların büyük kısmının yalnızca 0–1 beceri içermesinden kaynaklanmaktadır.
</div>

<p>
Yine de sosyal becerilerin birbirine paralel şekilde talep edildiği
frekans analizleriyle desteklenmiştir.
</p>

<hr>

<h2>7. İşletmeler İçin Stratejik Öneriler</h2>

<h3>7.1 Eğitim ve Gelişim</h3>
<ul>
    <li>Sosyal beceri eğitimlerine öncelik verilmelidir</li>
    <li>Takım içi iletişim ve liderlik programları yaygınlaştırılmalıdır</li>
</ul>

<h3>7.2 İş İlanı Stratejisi</h3>
<ul>
    <li>Gereksiz teknik beceri listelerinden kaçınılmalıdır</li>
    <li>Rol bazlı, net ve sade ilanlar tercih edilmelidir</li>
</ul>

<h3>7.3 Aday Deneyimi</h3>
<ul>
    <li>Daha az ama anlamlı beceri talebi, başvuru sayısını artıracaktır</li>
    <li>Gerçekçi ilanlar işveren markasını güçlendirir</li>
</ul>

<hr>

<h2>8. Sonuç</h2>

<p>
Bu çalışma, iş ilanı verilerinin yalnızca işe alım değil,
<strong>stratejik insan kaynakları kararları</strong> için de güçlü bir veri kaynağı
olduğunu göstermektedir.
</p>

<p>
Analiz sonuçları, günümüz iş dünyasında teknik becerilerin değil,
<strong>insan odaklı yetkinliklerin</strong> ön planda olduğunu açıkça ortaya koymaktadır.
</p>

<hr>

<p><em>Analiz Tarihi: 2026-01-20</em></p>

</body>
</html>

