# DSMBC Day 2 Data Preprocessing

Notebook ini digunakan pada sesi Day 2 Data Science Mini Bootcamp untuk mempelajari proses data preprocessing setelah peserta menyelesaikan Exploratory Data Analysis pada sesi sebelumnya. Fokus utama notebook adalah menyiapkan data agar lebih bersih, konsisten, dan siap digunakan pada tahap pemodelan machine learning.

Materi disusun dengan pendekatan yang tetap ramah untuk pemula. Setiap teknik diperkenalkan melalui konsep singkat, percobaan pada data, visualisasi, serta pembahasan mengenai alasan pemilihan treatment yang digunakan.

## Tujuan Pembelajaran

Setelah menyelesaikan notebook ini, peserta diharapkan mampu memahami hubungan antara hasil EDA dan keputusan preprocessing, mengenali masalah kualitas data, membagi data dengan tepat, menangani missing value, memeriksa outlier berdasarkan statistik dan domain, melakukan transformasi pada distribusi yang sangat skewed, memahami feature engineering, mengubah fitur kategorikal menjadi numerik, serta melakukan feature scaling.

Peserta juga diperkenalkan pada prinsip pencegahan data leakage agar proses preprocessing tidak menggunakan informasi yang seharusnya hanya tersedia pada validation data atau test data.

## Alur Materi

1. Instalasi dan import library

2. Memuat dataset

3. Ringkasan masalah data yang akan ditangani

4. Data cleaning

5. Feature dan target separation

6. Train, validation, dan test split

7. Missing value handling

8. Outlier detection dan domain validation

9. Skewness handling

10. Feature engineering

11. Categorical encoding

12. Feature scaling

13. Final preprocessing check

14. Ringkasan treatment final

15. Mini assignment

## Data Cleaning

Data cleaning dilakukan untuk menangani masalah yang sudah dapat dinilai sebagai masalah kualitas data. Exact duplicate dihapus agar observasi yang sama tidak muncul lebih dari satu kali. Nilai yang tidak masuk akal juga diperiksa berdasarkan konteks domain sebelum menentukan treatment.

Nilai yang terlihat ekstrem secara statistik tidak langsung dianggap salah. Pemeriksaan domain tetap diperlukan untuk membedakan nilai yang benar benar tidak valid dengan nilai ekstrem yang masih mungkin terjadi.

## Train, Validation, dan Test Split

Dataset dibagi menjadi training data, validation data, dan test data. Stratification digunakan agar proporsi kelas target tetap relatif konsisten pada setiap subset.

Setelah proses pembagian data, seluruh parameter preprocessing dipelajari hanya dari training data. Validation data dan test data hanya menerima transformasi berdasarkan parameter yang telah diperoleh dari training data.

Prinsip utama yang digunakan adalah:

**Fit pada training data, kemudian transform validation data dan test data.**

Pendekatan ini digunakan untuk mengurangi risiko data leakage.

## Missing Value Handling

Notebook memperkenalkan beberapa pendekatan sederhana untuk menangani missing value, yaitu penghapusan observasi, mean imputation, median imputation, dan mode imputation.

Perbandingan mean dan median dilakukan agar peserta dapat melihat bahwa pemilihan metode perlu mempertimbangkan karakteristik distribusi data. Treatment akhir menggunakan median imputation pada fitur numerik yang memiliki missing value karena pendekatan ini lebih tahan terhadap keberadaan nilai ekstrem.

## Outlier Detection dan Domain Validation

Outlier dideteksi menggunakan IQR dan Z Score sebagai metode awal untuk menemukan observasi yang perlu diperiksa lebih lanjut.

Hasil deteksi statistik tidak langsung digunakan sebagai dasar untuk menghapus data. Kandidat outlier diperiksa kembali berdasarkan konteks domain untuk menentukan apakah nilainya merupakan kesalahan data atau hanya nilai ekstrem yang masih valid.

Capping juga diperkenalkan sebagai salah satu alternatif ketika nilai ekstrem perlu dibatasi tanpa menghapus seluruh observasi.

## Skewness Handling

Distribusi fitur numerik diperiksa kembali setelah data cleaning. Pada fitur dengan distribusi yang sangat menceng ke kanan, beberapa transformasi sederhana dibandingkan.

Notebook memperkenalkan log transformation dan square root transformation. Log transformation digunakan sebagai treatment akhir pada fitur pendapatan karena mampu mengurangi skewness secara signifikan dan menghasilkan distribusi yang lebih seimbang.

## Feature Engineering

Feature engineering digunakan untuk membentuk representasi data yang lebih informatif dari fitur yang sudah tersedia.

Notebook memperkenalkan ratio feature dan binning sebagai contoh sederhana. Sebelum membuat fitur baru, peserta juga diajak memeriksa apakah informasi serupa sebenarnya sudah tersedia dalam dataset agar tidak menghasilkan fitur yang redundant.

## Categorical Encoding

Beberapa teknik encoding diperkenalkan berdasarkan karakteristik fitur kategorikal.

1. Binary mapping digunakan untuk fitur yang hanya memiliki dua kategori.

2. Ordinal encoding digunakan untuk kategori yang memiliki urutan alami.

3. One Hot Encoding digunakan untuk kategori nominal yang tidak memiliki urutan.

4. Frequency encoding diperkenalkan sebagai alternatif yang mempertahankan satu kolom dengan mengganti kategori berdasarkan frekuensi kemunculannya.

Frequency encoding membantu menunjukkan bahwa One Hot Encoding bukan satu satunya pilihan, terutama ketika jumlah kategori cukup banyak dan penambahan kolom perlu dipertimbangkan.

## Feature Scaling

Notebook memperkenalkan StandardScaler, MinMaxScaler, dan RobustScaler agar peserta dapat melihat perbedaan cara setiap metode mengubah skala data.

StandardScaler digunakan sebagai treatment akhir untuk fitur numerik kontinu. Parameter scaler dipelajari dari training data kemudian diterapkan pada validation data dan test data.

## Final Preprocessing Check

Pada bagian akhir dilakukan pemeriksaan untuk memastikan bahwa hasil preprocessing sudah konsisten.

Pemeriksaan mencakup jumlah missing value, struktur kolom pada train validation dan test, tipe data akhir, jumlah fitur, serta konsistensi transformasi pada seluruh subset.

Hasil akhir preprocessing menghasilkan data numerik dengan struktur fitur yang sama pada training data, validation data, dan test data sehingga siap digunakan pada tahap modeling.

## Mini Assignment

Peserta melanjutkan eksplorasi melalui dua topik berikut.

1. Feature Selection

Peserta mengevaluasi apakah seluruh fitur perlu digunakan atau terdapat fitur yang redundant maupun kurang informatif.

2. Class Imbalance Handling

Peserta mempelajari beberapa pendekatan untuk menangani distribusi kelas yang tidak seimbang. Treatment imbalance hanya dilakukan pada training data agar tidak menyebabkan data leakage.

## Library yang Digunakan

Notebook menggunakan beberapa library utama dalam ekosistem Python.

1. pandas untuk pengolahan data

2. NumPy untuk operasi numerik

3. Matplotlib untuk visualisasi

4. Seaborn untuk visualisasi statistik

5. SciPy untuk perhitungan statistik

6. scikit learn untuk train test split, imputation, encoding, dan scaling


