# BIST Hisse Senedi Analizi ve CAPM Modellemesi - Dönem Projesi Yol Haritası (Genişletilmiş Kapsam)

Bu belge, **Finans & Ekonomi** teması altında **BIST 100 Endeksi (XU100.IS)** ve 6 farklı sektörü temsil eden 12 öncü hisse senedinin son 5 yıllık günlük verileri üzerinden yürütülecek veri bilimi projesinin 15 aşamalı planını içermektedir.

Sektörel bazlı karşılaştırmaların ve derinliğin artırılması amacıyla her sektörden ikişer şirket seçilmiştir.

---

## Proje Parametreleri

*   **Tema:** Finans & Ekonomi
*   **Veri Kaynağı:** Yahoo Finance (`yfinance` kütüphanesi)
*   **Zaman Aralığı:** Son 5 yıl (günlük veriler)
*   **Piyasa Göstergesi:** `XU100.IS` (BIST 100 Endeksi)
*   **Sektörler ve Hisse Senetleri (6 Sektör, 12 Hisse):**
    1.  **Ulaştırma:** `THYAO.IS` (Türk Hava Yolları), `PGSUS.IS` (Pegasus)
    2.  **Bankacılık:** `AKBNK.IS` (Akbank), `GARAN.IS` (Garanti BBVA)
    3.  **Sanayi / Demir-Çelik:** `EREGL.IS` (Erdemir), `KRDMD.IS` (Kardemir D)
    4.  **Savunma / Teknoloji:** `ASELS.IS` (Aselsan), `OTKAR.IS` (Otokar)
    5.  **Perakende Ticaret:** `BIMAS.IS` (BİM), `MGROS.IS` (Migros)
    6.  **Enerji / Rafineri:** `TUPRS.IS` (Tüpraş), `ENJSA.IS` (Enerjisa)
*   **Modelleme:** Lineer Regresyon ile CAPM (Sermaye Varlıklarını Fiyatlandırma Modeli) analizi
*   **Çalışma Şekli:** Bireysel (Tek Kişi)
*   **Raporlama Dili:** Türkçe

---

## AŞAMALI YOL HARİTASI VE PROMPT GÜNLÜĞÜ

### AŞAMA 1: Geliştirme Ortamı Kurulumu ve Klasör Yapısı
*   **Açıklama:** Proje için izole bir sanal ortam oluşturulması, gerekli kütüphanelerin belirlenmesi ve temiz bir Git repository yapısının kurulması.
*   **Çıktı:** `requirements.txt`, `.gitignore` ve klasör yapısı.

```text
PROMPT 1:
"BIST sektörel hisse senedi analizi yapacağım kapsamlı bir veri bilimi dönem projesi için Python ortamı hazırlamak istiyorum. 
Projemde pandas, numpy, yfinance, matplotlib, seaborn, scikit-learn, scipy ve jupyter kütüphanelerini kullanacağım.
Lütfen bana:
1. Gerekli kütüphaneleri içeren bir requirements.txt içeriği hazırla.
2. Python projeleri için standart, temiz bir .gitignore dosyası içeriği oluştur.
3. Proje klasör yapısını (veri, notebook, rapor ve dökümanlar için) gösteren bir şema öner."
```

---

### AŞAMA 2: yfinance ile Genişletilmiş Veri Toplama (Data Acquisition)
*   **Açıklama:** Belirlenen 12 hisse kodu ve BIST 100 endeksi için son 5 yıllık günlük kapanış fiyatları ve işlem hacimlerinin indirilmesi.
*   **Çıktı:** `veri_toplama.py` veya ilk notebook hücresi, ham verilerin CSV olarak kaydedilmesi.

```text
PROMPT 2:
"Python ve yfinance kütüphanesini kullanarak BIST 100 Endeksi (XU100.IS) ile şu 12 hissenin son 5 yıllık günlük verilerini indirecek bir Python kodu yazar mısın?
Hisseler: THYAO.IS, PGSUS.IS, AKBNK.IS, GARAN.IS, EREGL.IS, KRDMD.IS, ASELS.IS, OTKAR.IS, BIMAS.IS, MGROS.IS, TUPRS.IS, ENJSA.IS.
İndirilen veriler 'Date', 'Open', 'High', 'Low', 'Close', 'Adj Close', 'Volume' sütunlarını içermeli.
Verileri indirdikten sonra 'data/raw_data.csv' adıyla bir klasöre kaydetsin. 
Kodun içine hata yönetim mekanizması (veri çekilemezse uyarı verme vb.) ekle."
```

---

### AŞAMA 3: Veri Ön İşleme ve Sektörel Düzenleme (Preprocessing)
*   **Açıklama:** İndirilen verilerin incelenmesi, tarih sütununun indeks yapılması, veri tiplerinin kontrolü ve analiz için veri çerçevelerinin birleştirilmesi.
    *   *Önemli Metodolojik Not:* Canlı API (yfinance) üzerinden çekilen ham veriler, zaman dilimi uyumsuzluğu ve saat verisi içerebilir. Bu aşamada tarih formatının standart `datetime` biçimine getirilmesi ve saat dilimlerinin arındırılması kritik bir veri hazırlama adımıdır.
*   **Çıktı:** Temizlenmiş ve birleştirilmiş sektörel fiyat/hacim dataframe yapıları.

```text
PROMPT 3:
"Daha önce yfinance ile kaydettiğim 'data/raw_data.csv' dosyasını okuyup ön işleme tabi tutacak bir Python kodu hazırlar mısın?
İşlemler şunlar olmalı:
1. Tarih (Date) sütununu datetime tipine dönüştür, timezone (saat dilimi) bilgisini kaldır ve index yap.
2. Varlıkların sadece 'Adj Close' (Düzeltilmiş Kapanış) ve 'Volume' (İşlem Hacmi) sütunlarını filtrele.
3. Her bir varlığın Adj Close fiyatlarını tek bir DataFrame'de (sütun isimleri XU100, THYAO, PGSUS, AKBNK, GARAN, EREGL, KRDMD, ASELS, OTKAR, BIMAS, MGROS, TUPRS, ENJSA olacak şekilde) birleştir. Aynı işlemi Volume verileri için de yap.
4. Eksik veri (NaN) veya veri tipi uyuşmazlığı olup olmadığını kontrol eden kodları ekle."
```

---

### AŞAMA 4: Eksik Veri ve Aykırı Değer Yönetimi (Data Cleaning)
*   **Açıklama:** Finansal verilerde resmi tatiller veya borsa kesintileri nedeniyle oluşabilecek eksik verilerin doldurulması ve günlük getirilerdeki aykırı değerlerin (outliers) tespiti.
    *   *Önemli Metodolojik Not:* 12 farklı hisse senedi ve endeks verisi birleştirildiğinde, resmi tatiller ve yarım gün seansları gibi nedenlerle satırlarda eksik veriler (`NaN`) oluşur. Bu verilerin doldurulması (`ffill` - forward fill) ve ekstrem fiyat hareketlerinin (aykırı değerler) borsa devre kesicileri veya piyasa şokları bağlamında incelenmesi finansal veri temizliğinin en temel aşamasıdır.
*   **Çıktı:** Eksik verilerden arındırılmış ve aykırı değer analizi yapılmış veri seti.

```text
PROMPT 4:
"BIST hisse fiyatları DataFrame'imizde borsa tatilleri veya veri kesintileri nedeniyle oluşabilecek eksik verileri (NaN) kontrol etmemiz gerekiyor.
Lütfen bana:
1. DataFrame'de eksik değer olup olmadığını kontrol eden ve varsa bunları bir önceki günün fiyatıyla (forward fill yöntemi) dolduran,
2. Fiyatlardaki ani ve sıra dışı değişimleri (aykırı değerleri) tespit etmek için IQR (Çeyrekler Açıklığı) yöntemini kullanan,
3. Bulunan aykırı değerlerin tarihlerini listeleyen bir Python kodu yaz. (Finansal analizde bu aykırı değerlerin silinmeyip neden korunması gerektiğini, devre kesiciler ve piyasa şokları gibi finansal gerçeklerle yorum satırlarında açıkla)."
```

---

### AŞAMA 5: Günlük Getirilerin (Returns) Hesaplanması ve Özet İstatistikler
*   **Açıklama:** Fiyat analizinde standart olan günlük basit veya logaritmik getirilerin hesaplanması ve veri setinin istatistiksel özetinin çıkarılması.
*   **Çıktı:** Getiri sütunları eklenmiş DataFrame ve tanımlayıcı istatistik tablosu.

```text
PROMPT 5:
"Birleştirilmiş düzeltilmiş kapanış fiyatlarını (Adj Close) kullanarak her bir varlık (XU100 ve 12 hisse) için günlük yüzde getirileri (daily returns) hesaplayan bir Python kodu yazar mısın?
Ek olarak:
1. Hesaplanan getirilerin ortalama, medyan, standart sapma (oynaklık), minimum, maksimum, çarpıklık (skewness) ve basıklık (kurtosis) değerlerini içeren detaylı bir özet istatistik tablosu oluştur.
2. Bu özet istatistik tablosunu pandas.DataFrame formatında ekrana yazdır ve sonuçları finansal risk/getiri açısından kısaca yorumla."
```

---

### AŞAMA 6: EDA Görselleştirme 1 - Zaman Serisi Fiyat ve Hacim Trendleri
*   **Açıklama:** Hisselerin ve endeksin 5 yıllık fiyat gelişimlerinin ve işlem hacimlerinin görselleştirilmesi. (Zorunlu 4 görselleştirmeden birincisi).
*   **Çıktı:** Zaman serisi fiyat ve hacim grafikleri (Matplotlib/Seaborn).

```text
PROMPT 6:
"Projenin ilk zorunlu görselleştirmesi için Matplotlib/Seaborn kullanarak şunları çizen bir kod hazırlar mısın:
1. XU100 endeksi ile diğer 12 hissenin 5 yıllık düzeltilmiş kapanış fiyatlarının zaman serisi grafiği. (Fiyatlar farklı ölçeklerde olduğu için hisselerin fiyatlarını başlangıç tarihindeki değere göre normalize ederek -yani 100'e eşitleyerek- kümülatif getiri olarak çizdir). Grafik çok kalabalık olmasın diye hisseleri sektörlerine göre renklendir veya 6 alt panel (subplots) halinde sektör sektör göster.
2. Alt panelde hisselerin günlük işlem hacimlerini (Volume) gösteren grafikler ekle.
Grafiklerin başlıkları, eksen etiketleri, lejantları (legend) net ve Türkçe olsun. Görsel tasarım şık ve profesyonel olmalı."
```

---

### AŞAMA 7: EDA Görselleştirme 2 - Sektörel Korelasyon Isı Haritası (Correlation Heatmap)
*   **Açıklama:** Hisse getirilerinin birbirleriyle ve BIST 100 endeksiyle olan doğrusal ilişkilerinin incelenmesi. (Zorunlu 4 görselleştirmeden ikincisi).
*   **Çıktı:** Korelasyon matrisi ve ısı haritası grafiği.

```text
PROMPT 7:
"Hisselerin ve endeksin günlük getirileri arasındaki korelasyonu analiz etmek istiyoruz. Lütfen:
1. Varlıkların günlük getirilerini içeren veri çerçevesi üzerinden Pearson Korelasyon Matrisini hesapla.
2. Seaborn kütüphanesini kullanarak bu matrisi şık bir Isı Haritası (Heatmap) olarak çizdir. Korelasyon katsayılarını kutucukların içinde göster (annot=True). Hisse sırasını sektörlerine göre gruplanmış şekilde ayarla ki sektörel korelasyon blokları net görülebilsin.
3. Renk paleti olarak finansal analizlere uygun bir palet (örn. coolwarm veya RdBu) seç.
4. Isı haritasından çıkan sonuçlara göre kendi sektörü içinde korelasyonu en yüksek ve sektörler arası korelasyonu en düşük hisse çiftlerini yorumla."
```

---

### AŞAMA 8: Araştırma Sorusu 1 - Sektörel Korelasyonlar ve Beta Katsayıları
*   **Açıklama:** "Farklı sektördeki hisseler ile BIST 100 arasındaki ilişki nasıldır, sektör içi ve sektörler arası dinamikler nasıl değişmektedir?" sorusunun yanıtlanması.
*   **Çıktı:** Hisse senetlerinin sektörel bazda gruplanmış kovaryans, varyans ve Beta değerleri tablosu.

```text
PROMPT 8:
"Araştırma Sorusu 1: '6 farklı sektördeki (Ulaştırma, Bankacılık, Sanayi, Savunma, Perakende, Enerji) 12 hisse senedi ile BIST 100 endeksi arasındaki ilişki nasıldır? Hangi sektörler ve hisseler endeks hareketlerine karşı daha duyarlıdır (Beta)? Sektör içi şirketlerin Betaları ne kadar benzerdir?'
Bu soruyu yanıtlamak için:
1. Her bir hissenin günlük getirilerinin BIST 100 getirileri ile olan kovaryansını (covariance) hesapla.
2. BIST 100 endeks getirisinin varyansını (variance) hesapla.
3. Formülü kullanarak (Beta = Kovaryans(Hisse, Endeks) / Varyans(Endeks)) her hissenin Beta katsayısını bul.
4. Çıkan Beta değerlerini sektörlerine göre gruplayarak bir DataFrame'de topla. Her sektörün kendi içindeki iki şirketinin Betalarını karşılaştır (Örn: THYAO vs PGSUS, AKBNK vs GARAN). Sektörel olarak risk düzeylerini finansal açıdan yorumla."
```

---

### AŞAMA 9: EDA Görselleştirme 3 - Günlük Getiri Dağılımları (Histograms & KDE)
*   **Açıklama:** Getirilerin normal dağılıma uyup uymadığının görsel olarak analiz edilmesi. (Zorunlu 4 görselleştirmeden üçüncüsü).
*   **Çıktı:** Histogram ve Kernel Density Estimate (KDE) grafikleri.

```text
PROMPT 9:
"Hisselerin günlük getirilerinin dağılımını incelemek istiyoruz. Lütfen:
1. Sektörleri temsil eden 6 ana hisse senedi (THYAO, AKBNK, EREGL, ASELS, BIMAS, TUPRS) ve endeks için günlük getirilerin dağılımını gösteren histogram ve üzerine bindirilmiş yoğunluk eğrisi (KDE) grafiklerini 2x4 boyutunda bir subplot paneli olarak çizdir.
2. Grafiklerin üzerine normal dağılış eğrisini (kırmızı kesikli çizgiyle) referans olarak ekle ki gerçek getiri dağılımlarının yapısı (şişman kuyrukluluk, sivrilik vb.) net görülebilsin.
3. Bu dağılımların basıklık ve çarpıklık sonuçlarıyla (Aşama 5'te hesaplanan) nasıl örtüştüğünü yorum satırlarında açıkla."
```

---

### AŞAMA 10: Araştırma Sorusu 2 - Sektörel Oynaklık (Volatilite) ve Risk Analizi
*   **Açıklama:** "Hisselerin risk/oynaklık özellikleri nasıldır ve makroekonomik olaylar sektörel oynaklıkları nasıl etkilemiştir?" sorusunun yanıtlanması.
*   **Çıktı:** Sektörel hareketli oynaklık (rolling volatility) analizi ve yorumlar.

```text
PROMPT 10:
"Araştırma Sorusu 2: 'Hisselerin günlük getiri oynaklıkları (volatilite) sektörel olarak zaman içinde nasıl değişmiştir? Makroekonomik veya piyasa bazlı şok dönemlerinde hangi sektörler daha dayanıklı kalırken hangilerinin oynaklığı tavan yapmıştır?'
Bu soruyu yanıtlamak için:
1. Her varlık için günlük getirilerin 30 günlük hareketli standart sapmasını (rolling standard deviation) hesapla.
2. Bu standart sapmayı yıllıklandırmak için 252'nin kareköküyle çarp (Annualized Rolling Volatility).
3. Sektör bazında karşılaştırmayı kolaylaştırmak için her sektörün iki hissesinin hareketli oynaklık ortalamasını alarak sektörel oynaklık trendlerini tek bir zaman serisi grafiğinde çizdir.
4. Grafik üzerinde oynaklığın tavan yaptığı (spike) dönemleri tespit et ve bu dönemlerde Türkiye piyasasında gerçekleşen önemli makroekonomik olaylar/şoklar (örn. faiz kararları, döviz şokları) ile bağ kurarak yorumla."
```

---

### AŞAMA 11: EDA Görselleştirme 4 - Sektörel Teknik Göstergeler ve Hareketli Ortalamalar (SMA)
*   **Açıklama:** Fiyat trendlerini yumuşatmak ve trend dönüşlerini izlemek için hareketli ortalamaların çizilmesi. (Zorunlu 4 görselleştirmeden dördüncüsü).
*   **Çıktı:** 50 ve 200 günlük SMA çizgileri eklenmiş fiyat grafikleri.

```text
PROMPT 11:
"Teknik analizde trendleri izlemek için kullanılan hareketli ortalamaları çizmek istiyoruz. Lütfen:
1. Ulaştırma sektöründen THYAO ve Havayolu rakibi PGSUS hisselerinin düzeltilmiş kapanış fiyatlarını al.
2. Her iki hisse için 50 günlük (kısa vadeli) ve 200 günlük (uzun vadeli) Basit Hareketli Ortalamaları (SMA50 ve SMA200) hesapla.
3. Üst üste iki panel (subplot) halinde, hisselerin fiyat grafiğiyle birlikte bu iki hareketli ortalamayı çizdir.
4. Grafikte 50 günlük ortalamanın 200 günlüğü yukarı kestiği (Golden Cross) ve aşağı kestiği (Death Cross) bölgeleri belirginleştirecek görsel işaretçiler veya açıklamalar ekle. İki rakip şirketin kesişim sinyallerinin zamanlamasını karşılaştır."
```

---

### AŞAMA 12: Araştırma Sorusu 3 - Fiyat/Hacim ve Hareketli Ortalama Kesişim Başarısı
*   **Açıklama:** "İşlem hacmi ile fiyat değişimleri arasında ilişki var mıdır ve SMA kesişimleri karlı sinyaller üretmiş midir?" sorusunun yanıtlanması.
*   **Çıktı:** Basit backtest simülasyon sonuçları ve korelasyon tablosu.

```text
PROMPT 12:
"Araştırma Sorusu 3: 'İşlem hacmi artışı ile fiyat değişimleri arasında bir ilişki var mıdır? Basit hareketli ortalama kesişimleri (Golden/Death Cross) sektörel rakipler (THYAO ve PGSUS) için ne derece başarılı alım-satım sinyalleri üretmiştir?'
Bu soruyu yanıtlamak için:
1. Günlük mutlak fiyat değişimleri ile günlük işlem hacmi (Volume) arasındaki korelasyonu hesapla.
2. THYAO ve PGSUS hisseleri özelinde basit bir simülasyon (backtest) yap: SMA50, SMA200'ü yukarı kestiğinde al (Golden Cross), aşağı kestiğinde sat (Death Cross).
3. Bu stratejinin 5 yılın sonundaki kümülatif getirisini, hisseyi sadece alıp tutma (Buy and Hold) stratejisinin getirisiyle karşılaştır. Sonuçları tablo halinde gösterip hangi şirkette teknik analizin daha başarılı çalıştığını yorumla."
```

---

### AŞAMA 13: Basit Modelleme - Sektörel Lineer Regresyon ile CAPM Modellemesi
*   **Açıklama:** 12 hissenin endekse göre duyarlılığını ölçen Lineer Regresyon modellerinin eğitilmesi ve katsayıların çıkarılması.
*   **Çıktı:** Scikit-Learn veya Statsmodels ile regresyon modelleri, R-kare ve katsayılar tablosu, regresyon doğrusu grafikleri.

```text
PROMPT 13:
"Projenin modelleme bileşeni için Sermaye Varlıklarını Fiyatlandırma Modelini (CAPM) kuracağız. Lütfen:
1. Bağımsız değişken (X) olarak BIST 100 endeks getirisini (XU100), bağımlı değişkenler (Y) olarak ise sırasıyla 12 adet hisse senedinin günlük getirilerini belirle.
2. Her bir hisse için ayrı bir Lineer Regresyon modeli kur (Scikit-Learn veya Statsmodels kullanarak).
3. Modellerden elde edilen:
   - Eğim katsayısını (Slope - CAPM Betası)
   - Sabit terimi (Intercept - CAPM Alfası)
   - Belirlilik katsayısını (R-squared - Endeksin hisse getirisini açıklama oranı)
   değerlerini bir tabloda özetle ve sektörel olarak gruplandır.
4. Temsili olarak her sektörden birer hissenin (toplam 6 grafik) regresyon doğrusunu ve gerçek veri noktalarının saçılım (scatter) grafiğini 2x3 subplot paneli olarak çizdir."
```

---

### AŞAMA 14: Bulguların Yorumlanması ve Rapor Taslağının Oluşturulması
*   **Açıklama:** Analiz ve model sonuçlarının finansal bağlamda yorumlanması ve teslim edilecek PDF raporunun yapısının oluşturulması.
*   **Çıktı:** Rapor ana başlıkları ve içerik taslağı.

```text
PROMPT 14:
"BIST 12 hisse analizi, sektörel karşılaştırma ve CAPM modelleme çalışmam bitti. Elde ettiğim sonuçları teslim etmem gereken 3-5 sayfalık PDF rapora dönüştürmek istiyorum.
Lütfen bana şu ana başlıklardan oluşan profesyonel bir rapor taslağı (Word veya Markdown formatında yazabileceğim bir içerik şablonu) oluştur:
1. Problem Tanımı ve Projenin Amacı (Sektörel analiz vurgusuyla)
2. Kullanılan Veri Seti, Kaynağı ve Metodolojik Tercih Gerekçesi (Canlı API/yfinance verilerinin neden tercih edildiği ve hazır dosyalara göre akademik üstünlüğü)
3. Metot / Yöntem (Tarih düzeltmeleri, borsa tatili senkronizasyonu / ffill temizliği, hesaplanan istatistikler ve CAPM regresyon formülü)
4. Bulgular (3 araştırma sorusuna ve modele ait somut sonuçlar, sektörel karşılaştırma tabloları)
5. Projenin Sınırlılıkları (Örn: Sadece teknik veri kullanımı, makro değişkenlerin olmaması)
6. Bu projeden veri bilimi adına öğrenilenler."
```

---

### AŞAMA 15: README, Prompt Günlüğü ve GitHub Dağıtımı
*   **Açıklama:** Projenin son dökümantasyonunun yapılması, kullanılan promptların kronolojik olarak listelenmesi ve GitHub repository hazırlığı.
*   **Çıktı:** Nihai `README.md` ve prompt günlüğü tablosu.

```text
PROMPT 15:
"Dönem projemin teslimi için GitHub repository'sinde bulunması gereken README.md dosyasının içeriğini hazırlar mısın? 
İçerik şunları barındırmalı:
1. Proje Başlığı (BIST 12 Hisse Senedinin Sektörel CAPM Regresyon Analizi)
2. Proje Ekibi Bilgileri (Bireysel çalışma: Öğrenci Adı Soyadı, Numarası - doldurulabilir yer tutucularla)
3. Projeyi Çalıştırma Talimatları (virtualenv kurma, pip install, jupyter notebook çalıştırma adımları)
4. Kullanılan Kütüphaneler ve Versiyonları
5. Veri Kaynağının Adı (Yahoo Finance) ve Lisansı
6. Ek olarak: Bu geliştirme sürecinde kullandığım en az 10-15 önemli promptu içeren, hangi aşamada ne amaçla kullanıldığını gösteren kronolojik bir 'Prompt Günlüğü' markdown tablosu oluştur."
```
