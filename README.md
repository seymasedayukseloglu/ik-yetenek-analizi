<html lang="tr">

<body>
<div class="wrap">

  <h1>İnsan Kaynakları Yetenek Analizi</h1>
  <p class="muted">
    <span class="badge b-ok">Yöntem: Birliktelik Kuralları</span>
    <span class="badge b-ok">Apriori (Market Basket)</span>
    <span class="badge b-warn">Kaynak: Selenium + API</span>
    <span class="badge b-ok">Çıktı: PNG + TXT + XLSX</span>
  </p>

  <div class="card">
    <p>
      Bu proje, farklı iş ilanı platformlarından toplanan ilanlar üzerinden <b>yetenek/ beceri talebi</b> analizi yapmayı amaçlar.
      Analizde <b>Birliktelik Kuralları (Association Rules)</b> yaklaşımı kullanılarak:
    </p>
    <ul>
      <li>Eğitim bütçesinin <b>teknik</b> (Python, SQL, Excel, Agile) mi yoksa <b>sosyal</b> (İletişim, Liderlik, Takım Çalışması) becerilere mi ayrılacağı,</li>
      <li>İlanlarda <b>“unicorn”</b> (çok fazla beceri talep eden, bulunması zor) profil arayıp aramadığımız,</li>
      <li>Hangi iki yetkinliğin <b>ayrılmaz ikili</b> haline geldiği (örn: Takım Çalışması ↔ İletişim) araştırılmıştır.</li>
    </ul>
  </div>

  <div class="card toc">
    <h2>İçindekiler</h2>
    <a href="#giris">GİRİŞ</a>
    <a href="#veri-toplama">1. Veri Toplama ve Veri Setinin Oluşturulması</a>
    <a href="#ornek1">Örnek 1: Himalayas Jobs API ile Veri Toplama</a>
    <a href="#on-isleme">2. Veri Ön İşleme ve Analize Hazırlık</a>
    <a href="#ornek2">Örnek 2: Çoklu CSV Birleştirme ve Duplicate Temizleme</a>
    <a href="#beceri-matrisi">2.1 Beceri Sütunlarının İşlenmesi (Binary Matris)</a>
    <a href="#nihai-dataset">2.2 Ön İşleme Sonucu Oluşturulan Nihai Veri Seti</a>
    <a href="#apriori">3. Birliktelik Kuralları (Apriori) ile Analiz</a>
    <a href="#bulgular">4. BULGULAR VE YORUM</a>
    <a href="#soru1">4.1 Eğitim Bütçesi: Teknik mi Sosyal mi?</a>
    <a href="#soru2">4.2 Unicorn Profil Analizi</a>
    <a href="#soru3">4.3 Ayrılmaz Yetkinlikler (En Güçlü İkililer)</a>
    <a href="#cikti">5. Üretilen Çıktılar</a>
    <a href="#kurulum">6. Kurulum ve Çalıştırma</a>
    <a href="#etik">7. Etik, Yasal Notlar ve Sınırlılıklar</a>
    <a href="#sonuc">8. SONUÇ</a>
  </div>

  <h2 id="giris">GİRİŞ</h2>
  <div class="card">
    <p>
      İş ilanları, şirketlerin hangi yetkinlikleri kritik gördüğünü yansıtan güçlü sinyaller içerir.
      Bu projede ilan metinleri üzerinden <b>beceri talep dağılımı</b> çıkarılmış; ayrıca becerilerin birlikte istenme
      örüntüleri <b>Apriori</b> ile modellenmiştir.
    </p>
    <p class="muted small">
      Not: Çalışmada hem web sayfalarından veri çekmek için <b>Selenium</b>, hem de yapılandırılmış veri almak için <b>REST API</b> yöntemi kullanılmıştır.
      Böylece farklı platformların erişim kısıtları aşılmış ve veri çeşitliliği artırılmıştır.
    </p>
  </div>

  <h2 id="veri-toplama">1. Veri Toplama ve Veri Setinin Oluşturulması</h2>
  <div class="card">
    <p><b>Toplama Yöntemleri</b></p>
    <div class="grid">
      <div class="card col6">
        <p><span class="badge b-warn">Selenium</span> Dinamik sayfalardan veri çekme</p>
        <ul>
          <li><b>Kariyer.net</b> arama ve sayfalama: <code>https://www.kariyer.net/is-ilanlari?kw=...</code></li>
          <li><b>Eleman.net</b> ilan listesi: <code>https://www.eleman.net/is-ilanlari?sy=...</code></li>
          <li><b>Yenibiriş</b> ilan listesi: <code>https://www.yenibiris.com/is-ilanlari?sayfa=...</code></li>
          <li><b>LinkedIn</b> login + jobs search: <code>https://www.linkedin.com/jobs/search/?keywords=...</code></li>
        </ul>
      </div>
      <div class="card col6">
        <p><span class="badge b-ok">API</span> Yapılandırılmış uç noktadan veri çekme</p>
        <ul>
          <li><b>Himalayas Jobs API</b>: <code>https://himalayas.app/jobs/api?limit=100&offset=...</code></li>
        </ul>
        <p class="muted small">
          API yaklaşımı; sayfa yapısı değişimlerinden daha az etkilenir ve düzenli alanlar sağlar.
        </p>
      </div>
    </div>

    <div class="hr"></div>

    <p><b>Ham Veri Çıktıları (Örnek Dosya Adları)</b></p>
    <ul>
      <li>API: <code>ik_yetenek_analizi_veriseti.csv</code></li>
      <li>Eleman.net: <code>eleman_net_hizli_analiz.csv</code></li>
      <li>Kariyer.net: <code>kariyer_toplu_ilanlar.csv</code>, <code>kariyer_detayli_liste.csv</code></li>
      <li>Yenibiriş: <code>ik_yetenek_matrisi.csv</code>, <code>is_ilanlari_1000_analiz.csv</code></li>
      <li>LinkedIn: (login gerektirdiği için çıktı adı konfigüre edilebilir)</li>
    </ul>
  </div>

  <h3 id="ornek1">Örnek 1: Himalayas Jobs API ile Veri Toplama</h3>
  <div class="card">
    <p>
      API üzerinden sayfalı şekilde ilanlar çekilerek CSV’e kaydedilmiştir.
      Bu aşamada teknik beceriler için anahtar kelime temelli etiketleme uygulanmıştır.
    </p>
    <pre><code># API örnek uç nokta
https://himalayas.app/jobs/api?limit=100&amp;offset={offset}

# Örnek çıktı dosyası
ik_yetenek_analizi_veriseti.csv</code></pre>
    <p class="muted small">
      Not: Teknik beceri sözlüğü; python, sql, java, aws, docker, kubernetes, excel, tableau, power bi vb. anahtar kelimeleri içerecek şekilde genişletilmiştir.
    </p>
  </div>

  <h2 id="on-isleme">2. Veri Ön İşleme ve Analize Hazırlık</h2>
  <div class="card">
    <p>
      Bu projede ön işleme yalnızca “temizleme” değil; <b>farklı kaynakları tek şemada birleştirme</b> ve
      <b>analize uygun binary yetenek matrisi</b> üretme sürecidir.
    </p>
    <ul>
      <li><b>Çoklu kaynak birleştirme:</b> Klasördeki tüm CSV’leri bir araya toplama</li>
      <li><b>Duplicate temizliği:</b> Tekrar eden ilanları kaldırma</li>
      <li><b>Kolon standardizasyonu:</b> farklı sitelerdeki başlık/şirket/lokasyon/metin alanlarını tekleştirme</li>
      <li><b>Binary beceri matrisi:</b> belirlenen yetkinlikleri 0/1’e çevirme</li>
      <li><b>Türetilmiş değişken:</b> ilan başına toplam beceri sayısı (<code>Beceri_Sayisi</code>)</li>
    </ul>
  </div>

  <h3 id="ornek2">Örnek 2: Çoklu CSV Birleştirme ve Duplicate Temizleme</h3>
  <div class="card">
    <p>Birden fazla kaynaktan gelen CSV dosyaları otomatik bulunarak birleştirilir ve tekrar eden kayıtlar silinir.</p>
    <pre><code># Özet akış:
# 1) klasördeki *.csv dosyalarını bul
# 2) hepsini oku ve concat et
# 3) drop_duplicates ile tekrarları temizle
# 4) tek bir final dosyaya yaz

# Örnek final çıktı adı:
tum_is_ilanlari_final.csv</code></pre>
  </div>

  <h3 id="beceri-matrisi">2.1 Beceri Sütunlarının İşlenmesi (Binary Matris)</h3>
  <div class="card">
    <p>
      Analizde kullanılan beceri seti aşağıdaki gibidir. Veri setinde bu sütunlar yoksa güvenli şekilde 0 atanır;
      varsa sayısala çevrilip 0/1 formatına normalize edilir.
    </p>

    <div class="grid">
      <div class="card col6">
        <p><b>Teknik Beceriler</b></p>
        <ul>
          <li>Python</li>
          <li>SQL</li>
          <li>Excel</li>
          <li>Agile</li>
        </ul>
      </div>
      <div class="card col6">
        <p><b>Sosyal + Karma Beceriler</b></p>
        <ul>
          <li>İletişim</li>
          <li>Liderlik</li>
          <li>Takım_Calısması</li>
          <li>Analiz (karma)</li>
          <li>İngilizce (karma)</li>
        </ul>
      </div>
    </div>

    <p class="muted small">
      Not: Karma beceriler (Analiz, İngilizce) hem teknik bağlamda (analitik düşünme, raporlama) hem de sosyal bağlamda (iletişim/raporlama) kritik görüldüğü için ayrı kategoride değerlendirilmiştir.
    </p>
  </div>

  <h3 id="nihai-dataset">2.2 Ön İşleme Sonucu Oluşturulan Nihai Veri Seti</h3>
  <div class="card">
    <p>
      Ön işleme sonunda analiz için tek bir standart şema oluşturulur:
    </p>
    <pre><code>Baslik | Sirket | Lokasyon | Metin | Python | SQL | Excel | İngilizce | İletişim | Liderlik | Analiz | Takım_Calısması | Agile | Beceri_Sayisi</code></pre>
    <p class="muted small">
      Analizde kullanılan dosya örneği: <code>analize_hazir_is_ilanlari.xlsx</code>
    </p>
  </div>

  <h2 id="apriori">3. Birliktelik Kuralları (Apriori) ile Analiz</h2>
  <div class="card">
    <p>
      Analizde Market Basket yaklaşımı uygulanmıştır: Her iş ilanı bir “sepet”, her beceri bir “ürün” gibi ele alınır.
      Böylece becerilerin birlikte görülme örüntüleri çıkarılır.
    </p>
    <ul>
      <li><b>Support:</b> Bir kombinasyonun ilanlarda görülme sıklığı</li>
      <li><b>Confidence:</b> A varsa B’nin görülme olasılığı</li>
      <li><b>Lift:</b> Birlikte görülme rastgele mi? (<code>Lift &gt; 1</code> anlamlı ilişki)</li>
    </ul>

    <div class="hr"></div>

    <p><b>Analiz Parametreleri (Örnek)</b></p>
    <ul>
      <li><code>min_support = 0.05</code></li>
      <li><code>min_confidence = 0.30</code></li>
      <li><b>Unicorn eşiği:</b> <code>6+</code> beceri isteyen ilanlar</li>
    </ul>

    <p class="muted small">
      Not: Apriori adımı manuel implementasyon ile (1’li → 4’lü itemset) çalıştırılmıştır. Bu, yöntemin mantığını “kütüphaneye bağımlı kalmadan” göstermek açısından avantaj sağlar.
    </p>
  </div>

  <h2 id="bulgular">4. BULGULAR VE YORUM</h2>

  <h3 id="soru1">4.1 Eğitim Bütçesi: Teknik mi Sosyal mi?</h3>
  <div class="card">
    <p>
      Eğitim bütçesi kararı için iki sinyal kullanılır:
      (1) Teknik/Sosyal toplam talep oranı, (2) En çok talep edilen ilk beceriler.
    </p>
    <p class="muted">
      Proje çıktısında örnek bulgu: sosyal beceriler (Takım Çalışması, İletişim, Liderlik) teknik becerilere kıyasla daha yüksek oranda talep edilebilir.
      Bu durumda öneri: eğitim bütçesinin ağırlığını sosyal becerilere kaydırmak.
    </p>
  </div>

  <h3 id="soru2">4.2 Unicorn Profil Analizi</h3>
  <div class="card">
    <p>
      Unicorn analizinde ilan başına toplam beceri sayısı dağılımı incelenir ve <b>6+</b> beceri isteyen ilanlar “yüksek beklenti” olarak işaretlenir.
      Unicorn oranı yüksekse ilanların sektör standardının üstünde bir profil aradığı; düşükse ilanların daha erişilebilir olduğu yorumu yapılır.
    </p>
    <p class="muted small">
      Ek not: Medyan ve mod beceri sayıları “sektör standardı” için pratik referans olarak kullanılmıştır.
    </p>
  </div>

  <h3 id="soru3">4.3 Ayrılmaz Yetkinlikler (En Güçlü İkililer)</h3>
  <div class="card">
    <p>
      Ayrılmaz ikililer; <b>yüksek confidence</b> ve <b>yüksek lift</b> değerlerine sahip 1→1 kurallarla belirlenir.
      Örnek yorum: “Takım Çalışması varsa İletişim de isteniyor” gibi kurallar, bu iki yetkinliğin birlikte paketlendiğini gösterir.
    </p>
    <pre><code># Örnek yorum şablonu:
# Eğer: Takım_Calısması
# O zaman: İletişim
# Support: %X.X | Confidence: %Y.Y | Lift: Z.Zx</code></pre>
  </div>

  <h2 id="cikti">5. Üretilen Çıktılar</h2>
  <div class="card">
    <p><b>Analiz sonunda üretilen ana çıktılar:</b></p>
    <ul>
      <li><code>ik_yetenek_analizi_rapor.png</code> — 8 grafik içeren görsel rapor</li>
      <li><code>ik_yetenek_analizi_ozet.txt</code> — metinsel özet rapor</li>
      <li><code>ik_yetenek_analizi_detay.xlsx</code> — detaylı excel rapor (genel istatistikler, frekanslar, kurallar, itemsetler, unicorn örnekleri)</li>
    </ul>
    <p class="muted small">
      Ham veri dosyaları ayrıca <code>data/raw/</code> altında saklanabilir. Dosya boyutu çok büyükse GitHub’a eklemek yerine Drive linki tercih edilmelidir.
    </p>
  </div>

  <h2 id="kurulum">6. Kurulum ve Çalıştırma</h2>
  <div class="card">
    <p><b>Önerilen</b>: Sanal ortam + requirements ile çalıştırma.</p>
    <pre><code># (Windows) sanal ortam
python -m venv .venv
.venv\Scripts\activate

# paketler
pip install -r requirements.txt</code></pre>

    <p><b>Notebook üzerinden çalıştırma</b></p>
    <ul>
      <li><code>api_himalayas.ipynb</code> — API ile veri çekme</li>
      <li><code>selenium_kariyer.net.ipynb</code> — Kariyer.net veri çekme</li>
      <li><code>selenium_eleman.net.ipynb</code> — Eleman.net veri çekme</li>
      <li><code>selenium_yenibiris.ipynb</code> — Yenibiriş veri çekme</li>
      <li><code>selenium_linkedln.ipynb</code> — LinkedIn login + jobs search</li>
    </ul>

    <p><b>Analiz akışı</b> (özet)</p>
    <ol>
      <li>Ham CSV’leri topla (<code>data/raw/</code>)</li>
      <li>Birleştir + temizle → <code>tum_is_ilanlari_final.csv</code></li>
      <li>Kolon standardizasyonu + binary beceriler → <code>analize_hazir_is_ilanlari.xlsx</code></li>
      <li>Apriori analizi → PNG/TXT/XLSX çıktıları</li>
    </ol>
  </div>

  <h2 id="etik">7. Etik, Yasal Notlar ve Sınırlılıklar</h2>
  <div class="card">
    <p>
      Bu proje akademik amaçlıdır. Web scraping yapılırken sitelerin kullanım koşulları/robots politikaları dikkate alınmalıdır.
      LinkedIn gibi platformlarda giriş ve erişim kısıtları olduğundan scraping süreci ortamdan ortama değişebilir.
    </p>
    <ul>
      <li>Veri alanları platforma göre farklılık gösterir → şema birleştirme gerekir.</li>
      <li>Anahtar kelime tabanlı beceri çıkarımı hataya açıktır (örn: eş anlamlılar, bağlam).</li>
      <li>Apriori manuel uygulamada itemset boyutu arttıkça kombinasyon maliyeti artar; bu proje 4’lü itemset ile sınırlandırılmıştır.</li>
    </ul>
  </div>

  <h2 id="sonuc">8. SONUÇ</h2>
  <div class="card">
    <p>
      Bu çalışma; iş ilanı metinlerinden beceri talep sinyalleri çıkararak, İK karar süreçlerine veri temelli katkı sunar.
      Eğitim bütçesi önerisi, unicorn profil riski ve ayrılmaz yetkinlik çiftleri; doğrudan raporlanabilir ve yönetici sunumlarına dönüştürülebilir.
    </p>
    <p class="muted">
      Hazırlayan: <b>Şeyma Seda Yükseloğlu</b> — Web Madenciliği / İK Yetenek Analizi Projesi
    </p>
  </div>

</div>
</body>
</html>
