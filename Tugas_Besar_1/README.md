# TB1-JSTKelompokXX: Klasifikasi Muda vs Tua dengan Perceptron dan ADALINE

Tugas Besar 1 mata kuliah **Jaringan Syaraf Tiruan (IT6053)**

## Kasus

Klasifikasi biner pelanggan pusat perbelanjaan menjadi **Muda (+1)** jika `Age < 40` dan **Tua (-1)** jika `Age >= 40`, berdasarkan dua fitur: **Annual Income (k$)** dan **Spending Score (1-100)**. Dua model neuron tunggal, **Perceptron** dan **ADALINE (Delta Rule, batch gradient descent)**, diimplementasikan sendiri dengan Python dan NumPy, lalu dibandingkan pada delapan konfigurasi eksperimen. Pertanyaan utamanya: apakah kedua fitur cukup untuk memisahkan kelompok usia secara linear.

Kelas positif: **Muda (+1)**.

## Anggota Kelompok

| NIM | Nama |
|---|---|
| 71241153 | Felix Benedictus Setiawan |
| 71241169 | Rivano Putra Samudra |
| 71241162 | Dita Pranata Tandisau |
| 71241115 | Devon Novan Surya Putra |

## Struktur dan Tautan Berkas

| Berkas | Isi |
|---|---|
| [TB1_KelompokYOI.ipynb](TB1_KelompokXX.ipynb) | Notebook lengkap: kode, penjelasan, keluaran eksperimen, dan perhitungan manual |
| [TB1_KelompokYOI.pdf](TB1_KelompokXX.pdf) | Laporan |
| [dataset.csv](dataset.csv) | Data yang dipakai dalam eksperimen |
| [requirements.txt](requirements.txt) | Dependensi dan versi |
| [Presentasi_TB1.pdf](Presentasi_TB1.pdf) | Slide presentasi |
| [README.md](README.md) | Dokumen ini |

## Dataset

- **Berkas:** `dataset.csv`
- **Sumber:** https://www.kaggle.com/code/ahmedwaleed1903/mall-customer
- **Jumlah observasi:** 200 baris. Tidak ada *missing value* dan tidak ada data duplikat.

| Kolom | Keterangan | Dipakai |
|---|---|---|
| CustomerID | Pengenal unik pelanggan | Tidak |
| Gender | Jenis kelamin | Tidak |
| Age | Usia (tahun) | Dasar pembentukan label |
| Annual Income (k$) | Pendapatan tahunan (ribu dolar) | Fitur |
| Spending Score (1-100) | Skor belanja | Fitur |

**Label (kolom `Target`, dibuat di notebook):** `+1` (Muda) jika `Age < 40`, `-1` (Tua) jika `Age >= 40`. Distribusi: 116 Muda (58%) dan 84 Tua (42%).

**Transformasi:**
1. Pembagian 80:20 stratifikasi menurut kelas: 160 data latih (93 Muda, 67 Tua) dan 40 data uji (23 Muda, 17 Tua).
2. Standardisasi z-score pada kedua fitur. Rata-rata dan simpangan baku dihitung **hanya dari data latih**, lalu diterapkan ke data uji.

## Cara Menjalankan

**Versi:** Python [ISI, dari sel versi di notebook]. Versi paket lengkap ada di `requirements.txt`.

```bash
# 1. Unduh repository ini
git clone https://github.com/[ISI-USERNAME]/TB1-JSTKelompokXX.git
cd TB1-JSTKelompokXX

# 2. (Opsional) buat virtual environment
python -m venv venv
# Windows: venv\Scripts\activate    |    macOS/Linux: source venv/bin/activate

# 3. Pasang dependensi
pip install -r requirements.txt

# 4. Buka notebook
jupyter notebook TB1_KelompokXX.ipynb
```

Klik **Run All**. Notebook membaca `dataset.csv` dari folder yang sama dan dapat dijalankan dari awal sampai akhir tanpa berkas tambahan.

## Pengaturan Eksperimen

| Parameter | Nilai |
|---|---|
| Random seed (`train_test_split`) | 42 |
| Inisialisasi bobot dan bias | 0 |
| Urutan sampel | Tetap (tidak diacak) |
| Epoch | 50 per konfigurasi |
| Learning rate (η) | 0,001; 0,01; 0,1 (dengan standardisasi); 0,01 (tanpa standardisasi) |
| Loss ADALINE | MSE = (1/n) Σ(y − z)², dicatat sebelum pembaruan parameter |

Delapan konfigurasi: Perceptron dan ADALINE masing-masing pada tiga η dengan standardisasi, ditambah satu konfigurasi tanpa standardisasi (η = 0,01) untuk tiap model.

## Ringkasan Hasil

| Model | η | Standardisasi | Akurasi latih | Akurasi uji | Error / MSE akhir |
|---|---|---|---|---|---|
| Perceptron | 0,001 / 0,01 / 0,1 | Ya | 58,13% | 57,50% | 58 pembaruan |
| ADALINE | 0,001 | Ya | 66,25% | 60,00% | 0,9791 |
| ADALINE | 0,01 | Ya | 66,25% | 60,00% | 0,8601 |
| ADALINE | 0,1 | Ya | 66,88% | 60,00% | 0,7776 |
| Perceptron | 0,01 | Tidak | 63,12% | 65,00% | 82 pembaruan |
| ADALINE | 0,01 | Tidak | 41,88% | 42,50% | 3,69×10¹⁷⁸ (diverge) |

Baseline kelas mayoritas pada data uji: **57,50%**.

- Perceptron memprediksi seluruh sampel uji sebagai Muda (sama dengan baseline) dan tidak pernah mencapai nol pembaruan (berosilasi antara 58 dan 62 sejak epoch ke-16). Hasilnya identik di ketiga η karena bobot awal nol.
- ADALINE memprediksi kedua kelas (recall Muda 0,70; recall Tua 0,47), tetapi selisih akurasinya dengan Perceptron hanya satu sampel uji.
- Tanpa standardisasi, ADALINE diverge pada η = 0,01.
- Hasil mengindikasikan kedua kelas tidak terpisah secara linear dengan dua fitur ini. Keterbatasan: 40 data uji, satu kali pembagian data, dan hanya dua fitur.

## Bantuan AI

Kelompok memakai bantuan AI (Gemini, Claude) untuk memberi saran format struktur laporan dan sebagai rekan diskusi untuk memastikan narasi pembahasan sesuai dengan hasil. Penulisan kode implementasi Perceptron, ADALINE, dan eksperimen di notebook juga dibantu AI.

## Kontribusi Anggota

| Nama | Kontribusi |
|---|---|
| Felix Benedictus Setiawan | Eksperimen delapan konfigurasi, evaluasi dan visualisasi|
| Rivano Putra Samudra | Implementasi dan hasil Perceptron|
| Dita Pranata Tandisau | Persiapan dan pemeriksaan data, split dan standardisasi |
| Devon Novan Surya Putra | Implementasi dan hasil ADALINE|

## Referensi

- Rosenblatt, F. (1958). The perceptron: A probabilistic model for information storage and organization in the brain. *Psychological Review*, 65(6), 386-408. https://doi.org/10.1037/h0042519
- Widrow, B., & Hoff, M. E. (1960). Adaptive switching circuits. *IRE WESCON Convention Record*, Part 4, 96-104. https://isl.stanford.edu/~widrow/papers/c1960adaptiveswitching.pdf
- Gupta, V., dkk. (2025). Customer segmentation using machine learning. *IJIRT*, 11(11). https://ijirt.org/publishedpaper/IJIRT176215_PAPER.pdf
- Raschka, S. *Adaline: Adaptive linear neuron classifier*. Dokumentasi mlxtend. https://rasbt.github.io/mlxtend/user_guide/classifier/Adaline/
- Scikit-learn developers. *scikit-learn API Reference*. https://scikit-learn.org/stable/api/index.html
- https://www.kaggle.com/code/ahmedwaleed1903/mall-customer
