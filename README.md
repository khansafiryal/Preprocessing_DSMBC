# DSMBC Day 2 Data Preprocessing

Notebook ini digunakan pada Day 2 Data Science Mini Bootcamp untuk mempelajari proses data preprocessing setelah Exploratory Data Analysis. Fokus utama notebook adalah menyiapkan data agar lebih bersih, konsisten, dan siap digunakan pada tahap modeling.

Setiap tahap membahas pemeriksaan data, perbandingan metode, alasan pemilihan metode, dan treatment final berdasarkan karakteristik data.

## Tujuan Pembelajaran

Peserta diharapkan mampu melakukan data cleaning, domain validation, menangani outlier dan missing value, melakukan feature engineering, menangani skewness, melakukan categorical encoding dan feature scaling, serta memahami prinsip pencegahan data leakage.

Prinsip utama yang digunakan adalah **fit pada training data, kemudian transform validation dan test data**.

## Alur Materi

1. Data loading dan pemeriksaan awal
2. Data cleaning dan domain validation
3. Train, validation, dan test split
4. Outlier detection
5. Missing value handling
6. Feature engineering
7. Skewness handling
8. Categorical encoding
9. Feature scaling
10. Final preprocessing check

## Data Cleaning dan Domain Validation

Exact duplicate dihapus agar observasi yang sama tidak muncul lebih dari satu kali. Domain validation juga dilakukan pada fitur numerik berdasarkan aturan yang digunakan pada latihan.

Usia di luar rentang 1 sampai 100 tahun dianggap tidak wajar untuk konteks latihan ini. Lama bekerja yang melebihi usia juga dianggap tidak valid. Nilai invalid diubah menjadi missing value untuk ditangani pada tahap imputasi.

## Outlier Detection

Outlier dideteksi menggunakan **boxplot dan IQR**. Kandidat outlier tidak langsung dihapus, tetapi diperiksa kembali berdasarkan domain. Nilai ekstrem yang masih masuk akal tetap dipertahankan sebagai valid extreme value.

## Missing Value Handling

Beberapa metode diperkenalkan dan dibandingkan, yaitu mean, median, mode, constant, dan KNN Imputer.

Treatment final yang digunakan:

1. `person_age` menggunakan **median imputation**
2. `person_emp_length` menggunakan **median imputation**
3. `loan_int_rate` menggunakan **mean imputation**

KNN Imputer diperkenalkan sebagai metode yang memanfaatkan kemiripan antarobservasi, tetapi tidak digunakan sebagai treatment final agar baseline preprocessing tetap sederhana dan mudah dipahami.

## Feature Engineering

Notebook menambahkan dua fitur baru yang memiliki makna jelas.

### Interest Burden Proxy

Fitur ini menggambarkan kombinasi jumlah pinjaman dan tingkat bunga.

**Rumus:**

`Interest Burden Proxy = Loan Amount × (Interest Rate / 100)`

Implementasi:

```python
interest_burden_proxy = loan_amnt * (loan_int_rate / 100)
```

Nilai yang lebih besar menunjukkan kombinasi jumlah pinjaman dan tingkat bunga yang lebih tinggi. Fitur ini hanya digunakan sebagai proxy dan tidak merepresentasikan cicilan atau total bunga aktual.

### Employment Age Ratio

Fitur ini menunjukkan lama bekerja relatif terhadap usia peminjam.

**Rumus:**

`Employment Age Ratio = Employment Length / Age`

Implementasi:

```python
employment_age_ratio = person_emp_length / person_age
```

Nilai yang lebih tinggi menunjukkan lama bekerja yang lebih besar apabila dibandingkan dengan usia peminjam.

## Skewness Handling

Skewness diperiksa pada seluruh fitur numerik training data. Fitur dengan absolute skewness di atas 1 dibandingkan menggunakan:

1. Original
2. Log1p Transformation
3. Square Root Transformation

Transformasi final dipilih berdasarkan perubahan distribusi dan nilai skewness. Fitur tidak harus ditransformasi apabila hasil transformasi tidak memberikan perbaikan yang berarti.

## Categorical Encoding

Encoding disesuaikan dengan jenis fitur kategorikal:

1. Binary mapping untuk `cb_person_default_on_file`
2. Ordinal mapping untuk `loan_grade`
3. One Hot Encoding untuk `person_home_ownership` dan `loan_intent`

Unseen category pada validation dan test juga diperiksa. `handle_unknown="ignore"` digunakan agar kategori yang tidak muncul pada training data tidak menyebabkan error.

## Feature Scaling

StandardScaler, MinMaxScaler, dan RobustScaler dibandingkan terlebih dahulu.

**RobustScaler digunakan sebagai treatment final** sebab dataset masih memiliki valid extreme values yang dipertahankan.

Scaling terutama relevan untuk model yang sensitif terhadap skala seperti KNN, SVM, Logistic Regression, dan Neural Network. Model berbasis tree seperti Decision Tree, Random Forest, Gradient Boosting, dan XGBoost umumnya tidak memerlukan scaling.

## Final Preprocessing Check

Pemeriksaan akhir memastikan:

1. Tidak terdapat missing value
2. Tidak terdapat nilai infinity
3. Seluruh predictor telah numerik
4. Target tidak berada pada feature matrix
5. Kolom train, validation, dan test sama
6. Urutan kolom konsisten

Setelah seluruh pemeriksaan terpenuhi, data siap digunakan pada tahap modeling.

## Mini Assignment

1. **Feature Selection**
2. **Class Imbalance Handling**

Treatment class imbalance hanya dilakukan pada training data agar validation dan test tetap merepresentasikan distribusi data asli.

## Library yang Digunakan

Notebook menggunakan `pandas`, `NumPy`, `Matplotlib`, dan `scikit-learn` untuk pengolahan data, visualisasi, pembagian data, imputasi, encoding, dan scaling.
