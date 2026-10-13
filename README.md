# Sesi 05 — Transfer Learning untuk Klasifikasi Citra dengan Data Terbatas

Perbandingan pelatihan dari nol (B1), *feature extraction* (B2), *progressive fine-tuning* (B3),
uji kontrol *data augmentation* (B4), dan arsitektur efisien EfficientNet-B0 / MobileNetV3-Small (B5)
pada subset 10 kelas **Oxford-IIIT Pet** (5 kucing + 5 anjing; split stratified 70/15/15; seed 42).

## Ringkasan hasil (sumber: `results/metrics_summary.csv`)

| Kondisi | Trainable params | Val acc | Test Macro F1 | Gap train-val |
|---|---|---|---|---|
| B1 Scratch (ResNet-18) | 11,181,642 | 0.6522 | 0.6548 | 0.0730 |
| B2 Feature Extraction | 5,130 | 0.9799 | 0.9668 | 0.0129 |
| B3 Progressive Fine-Tuning | 8,398,858 | 0.9900 | 0.9699 | 0.0065 |
| B4 B3 tanpa augmentasi | 8,398,858 | 0.9766 | 0.9732 | 0.0234 |
| B4 B3 dengan augmentasi | 8,398,858 | 0.9900 | 0.9699 | 0.0065 |
| B5 EfficientNet-B0 | 1,142,202 | 0.9933 | 0.9801 | 0.0060 |
| B5 MobileNetV3-Small | 1,245,738 | 0.9331 | 0.9669 | 0.0604 |

Catatan: single seed dan test set 300 citra (1 citra = 0.33 poin persentase), sehingga selisih kecil
antar-kondisi transfer learning berada dalam kisaran noise. Analisis lengkap ada di laporan.

## Struktur repositori
```
Sesi05_Transfer_Learning.ipynb   # seluruh kode eksperimen 
Laporan_DeepLearning.pdf               # laporan analisis 
requirements.txt
results/
  metrics_summary.csv            # ringkasan per eksperimen (sumber angka laporan)
  training_logs.csv              # log per-epoch (train/val loss dan accuracy)
  metrics_raw.json               # log mentah + konfigurasi
  consolidated_table.md, failure_samples.csv
  fig_loss_curves_B1_B2_B3.png, fig_confusion_matrix.png,
  fig_failure_analysis.png, fig_gradcam_B2_vs_B3.png
```
Checkpoint model (`ckpt_*.pt`) tidak disertakan karena ukurannya besar; dihasilkan ulang saat notebook dijalankan.

## Cara menjalankan
**Google Colab (disarankan):** unggah notebook, pilih Runtime -> Change runtime type -> T4 GPU, lalu Run all
(dataset diunduh otomatis; total sekitar 15-20 menit pada T4).
**Lokal:** `pip install -r requirements.txt`, lalu `jupyter notebook` (GPU sangat disarankan).

## Protokol eksperimen
ResNet-18 (`IMAGENET1K_V1`), input 224x224 dengan normalisasi ImageNet, batch 32, AdamW (weight decay 1e-2),
CrossEntropyLoss, early stopping pada validation loss (patience 5, maksimum 30 epoch), `seed = 42`
(PyTorch, NumPy, Python). BatchNorm pada lapisan yang dibekukan dijaga dalam mode `eval()`.
Model terbaik dipilih berdasarkan **validation accuracy**; test set hanya dipakai untuk pelaporan akhir.

## Reproduksibilitas
Seed tunggal (42). Pada percobaan kami, `B4_B3_aug` identik dengan `B3` pada seluruh metrik (menjalankan
konfigurasi yang sama dengan seed yang sama), meskipun GPU berpotensi menimbulkan variasi kecil di lingkungan lain.
