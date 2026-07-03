# BIST 12 Hisse Senedinin Sektörel CAPM Regresyon Analizi

<div align="center">

![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=flat-square&logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat-square&logo=jupyter&logoColor=white)
![yfinance](https://img.shields.io/badge/yfinance-0.2.40+-brightgreen?style=flat-square)
![scikit--learn](https://img.shields.io/badge/scikit--learn-1.5+-orange?style=flat-square&logo=scikit-learn&logoColor=white)
![License](https://img.shields.io/badge/License-Academic-blue?style=flat-square)

**Borsa İstanbul · 6 Sektör · 12 Hisse · CAPM Regresyon · Risk Analizi · Backtest**

*İstanbul Üniversitesi-Cerrahpaşa — Veri Bilimi ve Analitiği Dönem Projesi (2026)*

</div>

---

## Proje Hakkında

Bu proje, Borsa İstanbul (BIST) bünyesindeki **6 farklı sektörden** seçilen **12 öncü hisse senedinin** ve **BIST 100 endeksinin (XU100)** son **5 yıllık (2021–2026)** günlük verilerini analiz etmektedir. Amaç; sektörel risk profillerini karşılaştırmak, makroekonomik şokların piyasa etkisini ölçmek ve CAPM lineer regresyon modeliyle her hissenin sistematik riskini (Beta) ortaya koymaktır.

### Araştırma Soruları

1. **Sektörel Beta:** Farklı sektörlerdeki hisse senetleri BIST 100'e karşı ne kadar duyarlıdır? Sektörel risk profilleri nasıl farklılaşmaktadır?
2. **Volatilite & Makro Şoklar:** Makroekonomik şok dönemlerinde (deprem, seçim, faiz krizi) hangi sektörler daha dayanıklı kalmaktadır?
3. **Teknik Analiz vs Al-Tut:** SMA50/SMA200 kesişim stratejisi, Türkiye'nin enflasyonist yükseliş ortamında Al-Tut stratejisini geçebilir mi?

---

## Kullanılan Sektörler ve Hisseler

| Sektör | Hisse 1 | Hisse 2 |
|---|---|---|
| Ulaştırma | THYAO (Türk Hava Yolları) | PGSUS (Pegasus) |
| Bankacılık | AKBNK (Akbank) | GARAN (Garanti BBVA) |
| Sanayi / Demir-Çelik | EREGL (Erdemir) | KRDMD (Kardemir D) |
| Savunma / Teknoloji | ASELS (Aselsan) | OTKAR (Otokar) |
| Perakende Ticaret | BIMAS (BİM) | MGROS (Migros) |
| Enerji / Rafineri | TUPRS (Tüpraş) | ENJSA (Enerjisa) |
| **Piyasa Göstergesi** | **XU100 (BIST 100 Endeksi)** | — |

---

## Temel Bulgular

| Bulgu | Değer |
|---|---|
| En yüksek Beta (agresif) | KRDMD — Sanayi (β = **1.204**) |
| En düşük Beta (defansif) | OTKAR — Savunma (β = **0.828**) |
| En yüksek R² | THYAO — Ulaştırma (R² = **0.555**) |
| En volatil sektör | Bankacılık (ort. **%44.2** yıllık) |
| En stabil sektör | Perakende (ort. **%37.4** yıllık) |
| Endeks volatilitesi | BIST 100: **%26.8** — tüm sektörlerin altında ✓ |
| THYAO 5 yıllık Al-Tut | **+2.432%** kümülatif getiri |
| THYAO SMA Stratejisi | +356% (Al-Tut'tan −2.076 pp geride) |

---

## Proje Yapısı

```
Veri Bilimi Odev/
│
├── notebooks/                                         # Analiz notebook'ları
│   ├── BIST_Hisse_Analizi_ve_CAPM_Modellemesi.ipynb  ← TEK TESLİM (Master)
│
├── data/
│   ├── raw/
│   │   └── raw_data.csv               # Ham API verisi — 13 varlık × 5 yıl (1.9 MB)
│   └── processed/                     # 17 adet işlenmiş CSV analiz çıktısı
│       ├── adj_close_prices.csv
│       ├── daily_returns.csv
│       ├── returns_summary_stats.csv
│       ├── capm_regresyon_sonuclari.csv
│       ├── sector_comparison.csv
│       ├── backtest_sonuclari.csv
│       └── ...
│
├── reports/                           # Görsel ve yazılı çıktılar
│   ├── proje_rapor_taslagi.md         # Dönem projesi raporu (7-8 sayfa)
│   ├── sektorel_capm_regresyon.png
│   ├── korelasyon_isi_haritasi.png
│   ├── sektorel_yillik_oynaklik.png
│   ├── getiri_dagilim_histogram_kde.png
│   ├── backtest_kesisim_basarisi.png
│   └── ...                           # Toplam 24 görsel çıktı
│
├── requirements.txt                   # Python bağımlılık listesi
├── .gitignore
└── README.md
```

---

## Kurulum ve Çalıştırma

### Gereksinimler
- Python 3.10+
- pip

### Adım 1 — Sanal Ortam Kur

```powershell
# Sanal ortam oluştur
python -m venv .venv

# PowerShell üzerinde etkinleştir
.venv\Scripts\Activate.ps1
```

```bash
# macOS / Linux üzerinde etkinleştir
source .venv/bin/activate
```

### Adım 2 — Bağımlılıkları Yükle

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### Adım 3 — Jupyter Başlat

```bash
jupyter notebook
```

### Adım 4 — Master Notebook'u Çalıştır

Jupyter arayüzünde `notebooks/BIST_Hisse_Analizi_ve_CAPM_Modellemesi.ipynb` dosyasını aç.  
`.venv` kernel'ının seçili olduğunu doğrula ve **Kernel → Restart & Run All** ile çalıştır.

> **Not:** Notebook, `yfinance` API üzerinden canlı veri çekeceğinden **internet bağlantısı** gerektirir.  
> Toplam çalıştırma süresi yaklaşık **3–5 dakikadır**.

---

## Kullanılan Kütüphaneler

| Kütüphane | Min. Versiyon | Kullanım Amacı |
|---|---|---|
| pandas | 2.2.0 | Veri manipülasyonu, zaman serisi yönetimi |
| numpy | 1.26.0 | Matrisel hesaplamalar, volatilite formülleri |
| yfinance | 0.2.40 | Yahoo Finance API ile canlı veri çekimi |
| matplotlib | 3.9.0 | Temel grafik ve görselleştirme |
| seaborn | 0.13.0 | Isı haritaları, dağılım grafikleri |
| scikit-learn | 1.5.0 | CAPM lineer regresyon (OLS) modeli |
| scipy | 1.13.0 | İstatistiksel analizler, skewness/kurtosis |
| statsmodels | 0.14.0 | Detaylı regresyon çıktıları |
| jupyter | 1.0.0 | Geliştirme ve sunum ortamı |

---

## Veri Kaynağı ve Lisans

| | |
|---|---|
| **Veri Kaynağı** | [Yahoo Finance](https://finance.yahoo.com/) — `yfinance` kütüphanesi aracılığıyla |
| **Analiz Dönemi** | 5 Temmuz 2021 – 1 Temmuz 2026 (1.251 işlem günü) |
| **Veri Lisansı** | Yahoo Finance verileri akademik ve kişisel araştırma amaçlı kullanım için ücretsizdir |
| **yfinance Paketi** | Apache License 2.0 |

---

## Prompt Günlüğü

Proje geliştirme sürecinde AI kodlama asistanına (Antigravity) gönderilen 15 kronolojik prompt:

| # | Aşama | Amaç | Prompt Özeti |
|---|---|---|---|
| 1 | Aşama 1 | Ortam Kurulumu | "Proje dizininde sanal ortamı kur, `requirements.txt` ve `.gitignore` dosyalarını finansal veri analitiğine uygun şekilde oluştur." |
| 2 | Aşama 2 | Veri Toplama | "BIST 100 endeksi ile belirlenen 6 sektörden 12 hissenin son 5 yıllık günlük fiyat ve hacim verilerini yfinance API ile çeken ve `data/raw/raw_data.csv` olarak kaydeden kodu yaz." |
| 3 | Aşama 3 | Veri Ön İşleme | "İndirilen ham veriyi yükle, Date sütununu datetime'a çevirip saat dilimini arındır. Fiyat ve hacim tablolarını pivot yapıp processed klasörüne kaydet." |
| 4 | Aşama 4 | Veri Temizleme | "Fiyat ve hacim tablolarındaki boşlukları ffill ve bfill ile doldur. Getiri oranları üzerinden IQR yöntemiyle aykırı değerleri tespit edip raporla ancak silme." |
| 5 | Aşama 5 | İstatistikler | "Düzeltilmiş fiyatlar üzerinden günlük yüzde getirileri hesapla. Skewness, Kurtosis ve oynaklık içeren detaylı bir özet istatistik tablosu oluştur ve CSV olarak kaydet." |
| 6 | Aşama 6 | EDA Görsel 1 | "Sektörel bazda normalize edilmiş kümülatif getiri fiyatları ile zaman serisi bazında borsa hacim trendlerini alt paneller halinde gösteren grafik çizdir." |
| 7 | Aşama 7 | Korelasyon | "12 hisse ve endeksin günlük getirileri arasındaki Pearson korelasyon katsayılarını hesapla ve Seaborn ile şık bir korelasyon ısı haritası grafiği oluştur." |
| 8 | Aşama 8 | Beta Analizi | "Araştırma Sorusu 1 kapsamında, endeks varyansı ve kovaryans formüllerini kullanarak 12 hisse senedinin Betasını hesapla ve sektörel karşılaştırma tablosu yap." |
| 9 | Aşama 9 | Dağılım Analizi | "Seçilen temsili hisselerin getiri dağılımlarını histogram ve KDE eğrileriyle çizdir. Dağılımların normalliğe uygunluğunu referans bir normal dağılış eğrisiyle kıyasla." |
| 10 | Aşama 10 | Volatilite | "30 günlük hareketli standart sapmayı yıllıklandırarak sektörel volatilite trendlerini çiz. Türkiye'deki 5 büyük makro şok dönemini grafik üzerinde axvspan ile gölgele." |
| 11 | Aşama 11 | SMA Crossover | "THYAO ve PGSUS için SMA50 ve SMA200 serilerini hesapla. Golden Cross ve Death Cross kesişim noktalarını işaretleyerek fiyatlar üzerindeki zamanlamasını görselleştir." |
| 12 | Aşama 12 | Backtest | "Mutlak getiri ile hacim korelasyonunu bul. THYAO ve PGSUS için SMA50/200 kesişim stratejisi backtestini yaz (shift(1) ile lookahead bias'ı engelle) ve kümülatif getirileri Al-Tut ile karşılaştır." |
| 13 | Aşama 13 | CAPM Regresyon | "Her hisse için BIST 100 endeksine karşı Scikit-Learn ile lineer regresyon eğit. Alfa, Beta ve R-kare değerlerini özetle ve 6 temsilci hisse için 2x3 regresyon doğrusu subplot'u çiz." |
| 14 | Aşama 14 | Rapor | "Analiz ve modelleme sonuçlarım bitti. Gerçek grafikler ve sayısal sonuçlar gömülü, 7-8 sayfalık profesyonel bir dönem projesi raporu yaz." |
| 15 | Aşama 15 | README & GitHub | "GitHub için profesyonel README yaz. Projeyi GitHub'a yüklerken görünmemesi gereken hiçbir şey kalmasın — .gitignore, temiz klasör yapısı, badge'ler." |

---

## Proje Sahibi

| | |
|---|---|
| **Ad Soyad** | Mustafa Baraz |
| **Öğrenci No** | 1306230052 |
| **Ders** | Veri Bilimi ve Analitiği Dönem Projesi |
| **Kurum** | İstanbul Üniversitesi-Cerrahpaşa (İÜC) |
| **E-posta** | mustafabaraz@ogr.iuc.edu.tr |
