# Motorspor Kariyer Analitiği

Bu proje, Python ile Veri Bilimi dersi dönem sonu projesi kapsamında hazırlanmıştır. Çalışmada motorsporları ve performans otomotiv sektörleriyle ilişkili teknik becerilerin kariyer fırsatları üzerindeki etkisi büyük ölçekli iş ilanı verisi üzerinden analiz edilmiştir.

## Proje Başlığı

**Motorsporları ve Performans Otomotiv Sektörlerinde Teknik Beceri ve Kariyer Fırsatı Analitiği**

## Projenin Amacı

Bu çalışmanın amacı, LinkedIn iş ilanları üzerinden veri bilimi, yazılım mühendisliği, gömülü sistemler, simülasyon, MATLAB, CFD, telemetri, kalibrasyon ve araç dinamiği gibi teknik becerilerin kariyer fırsatı sınıfı üzerindeki etkisini analiz etmektir.

## Veri Kaynakları

Projede ana veri kaynağı olarak LinkedIn Jobs and Skills 2024 veri seti kullanılmıştır.

- `job_postings.csv`: 1.348.510 iş ilanı kaydı
- `job_skills.csv`: 1.296.381 ilan-beceri eşleşmesi
- `job_summary.csv`: 4.821.976 açıklama kaydı

Ham veri dosyaları büyük boyutlu olduğu için bu repoya eklenmemiştir. Ana veri kaynağı Kaggle üzerinden erişilebilir:

**LinkedIn Jobs and Skills 2024 Dataset:**  
https://www.kaggle.com/datasets/asaniczka/1-3m-linkedin-jobs-and-skills-2024

İkinci veri kaynağı olarak proje kapsamında oluşturulan `external_skill_reference.csv` adlı dış beceri referans tablosu kullanılmıştır. Bu tabloda motorsporları ve performans otomotiv alanıyla ilişkili teknik becerilere dışsal önem ağırlıkları atanmıştır.

## Data Fusion

Ana LinkedIn iş ilanı verisi ile dış beceri referans tablosu birleştirilmiştir. Bu işlem sonucunda her ilan için `external_weighted_skill_score` değişkeni oluşturulmuştur.

## Üretilen Özellikler

Projede iş mantığına dayalı olarak aşağıdaki değişkenler oluşturulmuştur:

- `data_skill_score`
- `engineering_skill_score`
- `motorsport_skill_score`
- `career_value_score`
- `external_weighted_skill_score`
- `career_class`

## Modelleme

Kariyer fırsatı sınıfını tahmin etmek için Logistic Regression ve Linear SVC modelleri karşılaştırılmıştır. Final model olarak Linear SVC seçilmiştir.

Final model sonuçları:

| Model | Accuracy | Macro F1 | Weighted F1 |
|---|---:|---:|---:|
| Linear SVC | 0.8248 | 0.79 | 0.82 |

## Açıklanabilirlik

Model açıklanabilirliği için Linear SVC katsayılarına dayalı feature importance analizi yapılmıştır. En etkili değişkenler arasında simulation, calibration, control systems, Python, data acquisition, CFD, MATLAB, embedded, powertrain, SQL ve Simulink yer almıştır.

## Maliyet / Fayda Simülasyonu

İki aday senaryosu karşılaştırılmıştır:

- Temel aday: Python + SQL
- Gelişmiş aday: Python + SQL + MATLAB + simulation + telemetry + embedded + data analysis

Simülasyon sonucunda gelişmiş adayın eriştiği toplam ilan sayısı daha düşük olsa da, High sınıfı ilan oranı %13,84'ten %31,51'e yükselmiştir. Bu durum gelişmiş teknik becerilerin daha niş fakat daha yüksek kariyer değerine sahip ilanlara erişim sağladığını göstermektedir.

## Dosya Yapısı

```text
.
├── motorsport_job_analytics.ipynb
├── motorsport_yonetici_raporu.pdf
├── data/
│   ├── external_skill_reference.csv
│   ├── motorsport_performance_focus_final_enriched.csv
│   ├── score_summary_by_career_class.csv
│   ├── candidate_skill_simulation_summary.csv
│   ├── linear_svc_feature_importance_top50.csv
│   ├── final_model_results.csv
│   └── external_weighted_score_summary.csv
└── figures/
    ├── 01_skill_scores_by_career_class.png
    ├── 02_external_weighted_score.png
    ├── 03_linear_svc_feature_importance.png
    ├── 04_candidate_skill_simulation.png
    └── 05_fun_fact_high_skill_share.png
