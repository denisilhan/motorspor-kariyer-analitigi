# Motorspor Kariyer Analitiği

Bu proje, Python ile Veri Bilimi dersi dönem sonu projesi kapsamında hazırlanmıştır. Çalışmada motorsporları ve performans otomotiv sektörleriyle ilişkili teknik becerilerin kariyer fırsatları üzerindeki etkisi büyük ölçekli iş ilanı verisi üzerinden analiz edilmiştir.

## Proje Başlığı

**Motorsporları ve Performans Otomotiv Sektörlerinde Teknik Beceri ve Kariyer Fırsatı Analitiği**

## Öğrenci Bilgileri

**Öğrenci:** Deniz İlhan
**Öğrenci Numarası:** 132230027
**Ders:** Python ile Veri Bilimi
**GitHub Repository:** https://github.com/denisilhan/motorspor-kariyer-analitigi

## Projenin Amacı

Bu çalışmanın amacı, motorsporları ve performans otomotiv sektörlerine geçiş sağlayabilecek teknik becerilerin (veri bilimi, yazılım mühendisliği, gömülü sistemler, simülasyon, MATLAB, CFD, telemetri, kalibrasyon, araç dinamiği vb.) kariyer fırsatı kalitesi üzerindeki etkisini kantitatif yöntemlerle analiz etmektir.

Temel araştırma sorusu: *"Motorsporları ve performans otomotiv alanlarına yakın teknik roller için hangi beceriler daha yüksek kariyer fırsatı oluşturur ve farklı beceri profillerine sahip adayların erişebildiği ilanların niteliği nasıl değişir?"*

## Büyük Veri Altyapısı ve Veri Kaynakları

Analiz süreci, devasa veri boyutu nedeniyle **Uludağ Üniversitesi Büyük Veri Laboratuvarı / JupyterHub** ortamında yürütülmüştür. Spark Connect bağlantısı test edilmiş ancak dosya yolu erişim kısıtları nedeniyle süreç, bellek dostu `chunk-based pandas` yaklaşımıyla başarıyla tamamlanmıştır.

Projede iki temel veri kaynağı kullanılmıştır:

1. **Ana Veri Seti (LinkedIn Jobs and Skills 2024):** Kaggle üzerinden erişilen; 1.348.510 iş ilanı, 1.296.381 ilan-beceri eşleşmesi ve 4.821.976 açıklama kaydı barındıran küresel veri setidir.

   * Ham verilerin Drive Linki: [Google Drive Bağlantısı](https://drive.google.com/file/d/1_Bu4QBctztG47Jl7lKFZKeoNHZfU_iD1/view?usp=sharing)
   * İşlenmiş final odak veri seti: [Google Drive Bağlantısı](https://drive.google.com/file/d/1BUd9y_26b-KYS_992OC3HVhWpgMXF7-B/view?usp=sharing)

2. **Dış Beceri Referans Tablosu:** Sektörel uzmanlığa dayalı olarak proje kapsamında oluşturulan `external_skill_reference.csv` dosyasıdır. Motorsporları için kritik 15 temel teknik beceriye dışsal önem ağırlıkları atanmıştır.

## Veri Harmanlama (Data Fusion)

Zorunlu Data Fusion kriteri kapsamında; ana LinkedIn iş ilanı verisi ile dış beceri referans tablosu birleştirilmiş ve ilan verisi motorsporları alan bilgisiyle zenginleştirilerek `external_weighted_skill_score` değişkeni türetilmiştir. İlgisiz sektörler filtrelendikten sonra analiz 25.570 ilanlık odak veri seti üzerinden gerçekleştirilmiştir.

## İş Mantığına Dayalı Özellik Mühendisliği (Feature Engineering)

Projede iş mantığına dayalı olarak aşağıdaki değişkenler oluşturulmuştur:

* `data_skill_score`: Veri bilimi araçlarını (Python, SQL vb.) içerme yoğunluğu.
* `engineering_skill_score`: Gömülü/kontrol sistemleri terimlerinin sıklığı.
* `motorsport_skill_score`: Araç dinamiği ve telemetri ilişkili beceriler.
* `career_value_score`: İlanın genel kariyer potansiyelini ifade eden bileşik skor.
* `external_weighted_skill_score`: Dış referans ağırlıklı teknik derinlik skoru.
* `career_class`: Üretilen skorlara göre **Low (%44.2)**, **Medium (%45.2)** ve **High (%10.6)** olarak sınıflandırılmış hedef değişken.

## Modelleme ve Veri Sızıntısı (Data Leakage) Önlemi

Kariyer fırsatı sınıfını (`career_class`) tahmin etmek için Logistic Regression ve Linear SVC modelleri karşılaştırılmıştır.

> **Kritik Not:** İlk denemelerde, hedef değişkeni oluşturan Feature Engineering skorları modele verildiğinde doğruluk %100'e yakınsamıştır. Veri sızıntısını (Data Leakage) önlemek adına bu türetilmiş skorlar final modelden çıkarılmış, model sadece ham metin (TF-IDF) ve kategorik değişkenlerle eğitilmiştir.

**Final Model:** Linear SVC
**Accuracy:** %82.48
**Weighted F1:** 0.82

Bu sonuç, öngörülen %70 başarı eşiğini aşmıştır.

## Açıklanabilirlik (XAI)

Model kararlarının şeffaflığı için Linear SVC katsayılarına dayalı feature importance analizi yapılmıştır. Analiz sonuçları, yüksek kariyer fırsatı sunan ilanların genel yazılım becerilerinden ziyade; `simulation`, `calibration`, `control systems`, `CFD`, `MATLAB`, `embedded` ve `powertrain` gibi doğrudan mühendislik ve performans otomotiv odaklı becerilerle daha güçlü biçimde ilişkili olduğunu göstermektedir.

## Kariyer Maliyet / Fayda Simülasyonu

Makine öğrenmesi modelinin çıktıları kullanılarak iki adayın kariyer kalitesi karşılaştırılmıştır:

* **Temel Aday (Python + SQL):** Toplam ilan havuzunun çok geniş olmasına rağmen "High" sınıflı pozisyon oranı yalnızca **%13.84**'tür.
* **Gelişmiş Aday (Python + SQL + MATLAB + simulation + telemetry + embedded + data analysis):** Hitap edilen toplam ilan hacmi daralmış olsa da, havuz içerisindeki "High" pozisyonların oranı **%31.51** seviyesine yükselmiştir.

Bu durum, sektörel yetkinliklerin ilan hacminden ziyade *fırsat kalitesi* yarattığını operasyonel olarak göstermektedir.

## Dosya Yapısı

```text
.
├── motorsport_job_analytics.ipynb
├── Motorspor_Kariyer_Analitigi_Yonetici_Raporu_Final.pdf
├── README.md
├── requirements.txt
├── data/
│   ├── external_skill_reference.csv
│   ├── score_summary_by_career_class.csv
│   ├── candidate_skill_simulation_summary.csv
│   ├── linear_svc_feature_importance_top50.csv
│   ├── final_model_results.csv
│   └── external_weighted_score_summary.csv
└── grafikler/
    ├── 01_skill_scores_by_career_class.png
    ├── 02_external_weighted_score.png
    ├── 03_linear_svc_feature_importance.png
    └── 04_candidate_skill_simulation.png
```

## Kullanılan Kütüphaneler

* pandas
* numpy
* matplotlib
* scikit-learn

## Not

Ham LinkedIn veri setleri ve boyutu yüksek işlenmiş final odak veri seti GitHub reposuna eklenmemiştir. Bu dosyalara yukarıdaki Google Drive bağlantılarından erişilebilir. GitHub reposunda notebook, yönetici raporu, küçük özet CSV çıktıları ve grafikler paylaşılmıştır.
