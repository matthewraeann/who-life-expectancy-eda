# WHO Life Expectancy — Exploratory Data Analysis (2000–2015)

EDA terhadap data angka harapan hidup (*life expectancy*) dari 193 negara pada tahun 2000–2015. Notebook ini dibuat sebagai case study open recruitment COMPFEST.

- **Dataset:** Global Health Observatory (WHO) + data ekonomi PBB, 2.938 baris × 22 fitur
- **Target:** `Life expectancy` (tugas regresi)
- **Fokus:** menilai kualitas data dan memahami hubungan antar fitur sebelum modeling

## Pendekatan

Analisis disusun dalam lima phase:

1. **Data Quality Audit**: missing values, nilai 0 yang sebenarnya data hilang, inkonsistensi logis, kelengkapan data per negara, outlier
2. **Target Distribution & Segmentation**: distribusi `Life expectancy` dan perbedaan negara Developed vs Developing
3. **Correlation & Multicollinearity Mapping**: prediktor terkuat dan fitur yang redundan
4. **Thematic Deep-Dive**: analisis tematik, termasuk efek imunisasi dan korelasi `Alcohol` yang tidak intuitif
5. **Synthesis**: rangkuman temuan dan implikasinya untuk modeling

## Temuan utama

1. **Pola missing value bergantung pada `Status` negara.** `BMI`, `Polio`, `Diphtheria`, dan `thinness` hanya hilang pada negara Developing, sedangkan `Hepatitis B` justru lebih sering hilang pada negara Developed (33,79% vs 15,66%). Karena itu imputasi sebaiknya dilakukan per grup.
2. **Nilai 0 pada `percentage expenditure`, `Schooling`, dan `Income composition of resources` diperlakukan sebagai data hilang** (masing-masing 611, 28, dan 130 baris), lalu diubah menjadi NaN.
3. **Beberapa kolom tidak sesuai definisinya.** Ada 283 baris dengan `BMI` di bawah 10 dan lebih dari separuh baris di atas 40 (tidak masuk akal untuk rata-rata nasional), `Population` tidak konsisten antar tahun (misalnya Maldives 2003 = 34), dan `percentage expenditure` tampaknya berisi total pengeluaran, bukan persentase.
4. **Banyak fitur sangat *right-skewed*** (`GDP`, `Measles`, `Population`, `HIV/AIDS`, dan lainnya), sehingga ditransformasi dengan `log1p`.
5. **Label `Status` tidak akurat.** France, Canada, Finland, dan beberapa negara maju lain berlabel Developing. Sebagai gantinya dibuat `development_index` (gabungan `Schooling`, `Income composition of resources`, dan `log_GDP`) yang berkorelasi jauh lebih tinggi dengan target (r = 0,80 vs 0,48).
6. **Korelasi positif `Alcohol` adalah efek confounding.** Korelasinya +0,40, tetapi berubah menjadi −0,11 setelah dikontrol `development_index` (partial correlation).
7. **Imunisasi menunjukkan diminishing returns.** Kenaikan life expectancy terbesar terjadi antara coverage rendah (< ~78%) dan menengah; di atas ~97% hampir tidak ada perbedaan.

## Cara menjalankan

1. Clone repo ini.
2. Install library: `pip install pandas numpy matplotlib seaborn scipy scikit-learn`
3. Buka `life-expectancy-eda.ipynb` dengan Jupyter atau Google Colab. File `Life Expectancy Data.csv` harus berada di folder yang sama dengan notebook.

## Isi repo

| File | Keterangan |
|---|---|
| `life-expectancy-eda.ipynb` | Notebook EDA lengkap dengan output dan visualisasi |
| `Life Expectancy Data.csv` | Dataset |
| `README.md` | Dokumen ini |
