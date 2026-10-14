# DSMBC Day 2 Data Preprocessing

Notebook ini digunakan pada Day 2 Data Science Mini Bootcamp untuk mempelajari proses data preprocessing setelah Exploratory Data Analysis. Fokus utama notebook adalah menyiapkan data agar lebih bersih, konsisten, dan siap digunakan pada tahap modeling.

Setiap tahap membahas pemeriksaan data, perbandingan metode, alasan pemilihan metode, dan treatment final berdasarkan karakteristik data.

## Tujuan Pembelajaran

Peserta diharapkan mampu melakukan data cleaning, domain validation, menangani outlier dan missing value, melakukan feature engineering, menangani skewness, melakukan categorical encoding dan feature scaling, mengenal metode class imbalance handling, serta memahami prinsip pencegahan data leakage.

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
10. Class imbalance handling
11. Final preprocessing check
12. Export processed data

## Pemeriksaan Awal Data

Pemeriksaan awal dilakukan pada data mentah sebelum cleaning:

1. Tipe data, jumlah missing value, dan jumlah exact duplicate
2. Jumlah nilai unik setiap variabel
3. Value counts variabel kategorikal dan target, dalam jumlah dan persen
4. Lima nilai yang paling sering muncul pada setiap variabel numerik

Variabel kategorikal hanya memiliki 2 sampai 7 nilai unik, sehingga One Hot Encoding tidak menambah banyak kolom. Target `loan_status` terdiri dari 78,18 persen kelas 0 dan 21,82 persen kelas 1 pada data mentah.

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

Untuk setiap kandidat, transformasi final adalah transformasi yang menghasilkan absolute skewness paling kecil, walaupun hasilnya masih di atas 1.

Treatment final yang digunakan:

1. **Log1p** untuk `person_income`, `person_age`, `interest_burden_proxy`, dan `cb_person_cred_hist_length`
2. **Square root** untuk `person_emp_length`, `loan_amnt`, dan `loan_percent_income`

Skewness `person_age` turun dari 1,936 menjadi 1,177 setelah log1p, tetapi masih di atas 1 karena sebagian kecil peminjam berusia jauh lebih tua daripada mayoritas. Sisa kemiringan tersebut ditangani pada tahap scaling dengan RobustScaler. Transformasi lain seperti Box-Cox atau Yeo-Johnson tidak digunakan agar pipeline tetap sederhana.

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

## Class Imbalance Handling

Training data memiliki kelas 0 sekitar 78 persen dan kelas 1 sekitar 22 persen. Notebook mendemonstrasikan tiga metode resampling:

1. **Random Undersampling**, yaitu membuang sebagian data kelas mayoritas secara acak
2. **Random Oversampling**, yaitu menyalin data kelas minoritas secara acak
3. **SMOTE**, yaitu membuat data sintetis kelas minoritas di antara satu data dan tetangga terdekatnya

**Rumus SMOTE:**

`X_syn = X_i + (X_knn − X_i) × δ`

Notebook juga memuat contoh hitung satu data sintetis. Penerapan pada training data menggunakan **SMOTENC** agar kolom kategorikal tetap berisi kategori yang valid dan kolom One Hot Encoding tidak bernilai pecahan.

| Training data | Kelas 0 | Kelas 1 | Total |
|---|---|---|---|
| Original | 17.729 | 4.962 | 22.691 |
| Random Undersampling | 4.962 | 4.962 | 9.924 |
| Random Oversampling | 17.729 | 17.729 | 35.458 |
| SMOTE | 17.729 | 17.729 | 35.458 |

Resampling **hanya dilakukan pada training data setelah split**. Validation dan test tidak di-resample agar tetap merepresentasikan distribusi data asli. Encoder dan scaler di-fit dari training asli sebelum SMOTENC, lalu hanya dipakai untuk transform.

Hasil resampling tersimpan sebagai `X_train_under`, `X_train_over`, dan `X_train_smote`, sedangkan `X_train_final` tetap menjadi baseline. Metode terbaik ditentukan pada tahap modeling.

## Final Preprocessing Check

Pemeriksaan akhir memastikan:

1. Tidak terdapat missing value
2. Tidak terdapat nilai infinity
3. Seluruh predictor telah numerik
4. Target tidak berada pada feature matrix
5. Kolom train, validation, dan test sama
6. Urutan kolom konsisten

Setelah seluruh pemeriksaan terpenuhi, data siap digunakan pada tahap modeling.

## Export Processed Data

Notebook menghasilkan empat file:

1. `credit_train_processed.csv`
2. `credit_validation_processed.csv`
3. `credit_test_processed.csv`
4. `credit_train_smote_processed.csv`, yaitu file opsional berisi training hasil SMOTENC

## Mini Assignment

1. **Feature Selection**
2. **Class Imbalance Handling**

Pada tugas class imbalance, peserta melatih model yang sama pada `X_train_final`, `X_train_under`, `X_train_over`, dan `X_train_smote`, lalu membandingkan recall dan precision kelas 1 pada validation. Peserta juga dapat mencoba `class_weight="balanced"`.

Treatment class imbalance hanya dilakukan pada training data agar validation dan test tetap merepresentasikan distribusi data asli.

## Library yang Digunakan

Notebook menggunakan `pandas`, `NumPy`, `Matplotlib`, dan `scikit-learn` untuk pengolahan data, visualisasi, pembagian data, imputasi, encoding, dan scaling, serta `imbalanced-learn` untuk resampling.
