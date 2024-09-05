# Laporan Proyek Machine Learning - Irgi Setiawan

## Domain Proyek
Sistem rekomendasi buku menjadi penting karena semakin banyaknya pilihan buku yang tersedia, baik di toko buku fisik maupun digital. Pengguna sering kali kesulitan menemukan buku yang sesuai dengan preferensi mereka, sehingga sistem rekomendasi dapat membantu menyaring pilihan yang relevan. Dengan meningkatnya volume data pengguna, sistem rekomendasi berbasis konten (content-based filtering) menjadi solusi yang efektif untuk memberikan rekomendasi yang dipersonalisasi berdasarkan kesamaan fitur buku.

Sistem rekomendasi ini penting karena pengguna cenderung menginginkan rekomendasi yang sesuai dengan minat mereka tanpa perlu mencari secara manual. Dengan menerapkan pendekatan content-based filtering, rekomendasi bisa diberikan berdasarkan buku yang pernah disukai pengguna sebelumnya, sehingga mempercepat proses penemuan buku baru. Dalam konteks industri buku, sistem ini juga meningkatkan keterlibatan pengguna dan potensi pembelian. Menurut penelitian, sistem rekomendasi berbasis konten sering digunakan karena mampu memberikan rekomendasi yang relevan meskipun pengguna tidak memiliki riwayat interaksi yang lengkap (Adomavicius & Tuzhilin, 2005) .

Referensi:
- Adomavicius, G., & Tuzhilin, A. "Toward the Next Generation of Recommender Systems: A Survey of the State-of-the-Art and Possible Extensions." IEEE Transactions on Knowledge and Data Engineering, 17(6), 734-749 (2005). [Toward the Next Generation of Recommender Systems](https://ieeexplore.ieee.org/document/1423975).

## Business Understanding
### Problem Statements
- Bagaimana cara membangun sistem rekomendasi buku yang dapat memberikan rekomendasi personal kepada pengguna berdasarkan buku yang pernah mereka sukai?
- Bagaimana cara menilai keefektifan sistem rekomendasi buku yang dibuat?

### Goals
- Membangun model rekomendasi berbasis konten yang dapat memberikan rekomendasi buku berdasarkan kesamaan fitur buku.
- Mengevaluasi model rekomendasi menggunakan metrik evaluasi seperti precision, recall, dan F1-score.

### Solution statements
- Menggunakan metode content-based filtering untuk memberikan rekomendasi berdasarkan kemiripan fitur buku seperti judul, pengarang, dan penerbit.
- Sistem akan dievaluasi menggunakan metrik precision, recall, dan F1-score untuk memastikan rekomendasi yang dihasilkan akurat dan relevan.

## Data Understanding
Dataset yang digunakan berasal dari **Bookcrossing Dataset** yang diunduh dari [Kaggle](https://www.kaggle.com/datasets/ruchi798/bookcrossing-dataset). Dataset ini terdiri dari tiga file utama: **BX_Books.csv**, **BX-Book-Ratings.csv**, dan **BX-Users.csv**. Berikut rincian setiap dataset:

1. **BX_Books.csv**:
   - Jumlah Data: 271,379 baris dan 8 kolom.
   - Kondisi Data: Terdapat beberapa nilai kosong pada kolom *Book-Author* dan *Publisher* yang perlu dihapus atau diisi.
   - Kolom Fitur:
     - **ISBN**: Identifikasi unik untuk buku.
     - **Book-Title**: Judul buku.
     - **Book-Author**: Pengarang buku.
     - **Year-Of-Publication**: Tahun penerbitan buku.
     - **Publisher**: Penerbit buku.
     - **Image-URL-S**, **Image-URL-M**, **Image-URL-L**: URL gambar sampul buku dengan ukuran berbeda.

2. **BX-Book-Ratings.csv**:
   - Jumlah Data: 1,149,780 baris dan 3 kolom.
   - Kondisi Data: Tidak ada nilai yang hilang pada dataset ini.
   - Kolom Fitur:
     - **User-ID**: Identifikasi unik untuk pengguna.
     - **ISBN**: Identifikasi unik untuk buku.
     - **Book-Rating**: Nilai rating yang diberikan pengguna untuk buku (skala 0-10).

3. **BX-Users.csv**:
   - Jumlah Data: 278,858 baris dan 3 kolom.
   - Kondisi Data: Kolom *Age* memiliki banyak nilai kosong, sehingga perlu diproses lebih lanjut (seperti pengisian atau pemfilteran).
   - Kolom Fitur:
     - **User-ID**: Identifikasi unik untuk pengguna.
     - **Location**: Lokasi geografis pengguna.
     - **Age**: Usia pengguna.

## Data Preparation
Tahap persiapan data ini mencakup beberapa langkah untuk memastikan data bersih, terstruktur, dan siap digunakan untuk pemodelan sistem rekomendasi berbasis konten.

1. **Memuat Data**: 
   - Dataset yang digunakan adalah *BX_Books.csv*, *BX-Book-Ratings.csv*, dan *BX-Users.csv*.
   - Langkah pertama adalah memuat dataset menggunakan pandas dan melakukan pemeriksaan data awal, seperti melihat lima baris pertama data dan informasi terkait jumlah baris, kolom, dan tipe data.

2. **Pemeriksaan Nilai Kosong dan Statistik Deskriptif**:
   - Dilakukan pemeriksaan terhadap nilai kosong pada dataset buku, penilaian, dan pengguna.
   - Untuk dataset buku, beberapa kolom seperti *Book-Author* dan *Publisher* memiliki nilai kosong yang perlu ditangani.
   - Pada dataset pengguna, kolom *Age* juga memiliki banyak nilai kosong.
   - Selain itu, dilakukan analisis statistik deskriptif pada kolom *Year-Of-Publication* dan *Age* untuk memahami distribusi data.

3. **Pembersihan Data**:
   - **Menghapus Nilai Kosong**: Pada dataset buku, baris dengan nilai kosong pada kolom penting seperti *ISBN*, *Book-Title*, *Book-Author*, *Year-Of-Publication*, dan *Publisher* dihapus.
   - **Menghapus Duplikasi**: Menghapus entri duplikat berdasarkan *ISBN* untuk memastikan setiap buku hanya muncul satu kali.
   - **Memfilter Usia Pengguna**: Pengguna dengan usia di bawah 5 tahun dan di atas 100 tahun difilter untuk menghindari data yang tidak valid.

4. **Ekstraksi Fitur**:
   - Untuk sistem rekomendasi berbasis konten, dibuat fitur gabungan dari kolom *Book-Title*, *Book-Author*, dan *Publisher*. Kolom ini digunakan untuk menciptakan representasi teks yang digunakan oleh model rekomendasi.
   - Teknik **TF-IDF (Term Frequency-Inverse Document Frequency)** digunakan untuk mengubah fitur teks gabungan tersebut menjadi representasi numerik. Ini memungkinkan model untuk mengukur kemiripan antar buku berdasarkan kata-kata penting yang terdapat dalam fitur gabungan tersebut.
   - **Ukuran TF-IDF Matrix**: Matriks TF-IDF yang dihasilkan memiliki dimensi sesuai dengan jumlah buku dan kata-kata unik yang relevan.

5. **Persiapan Model Nearest Neighbors**:
   - Setelah fitur diekstrak, dilakukan fitting menggunakan model **Nearest Neighbors** dengan metrik **cosine similarity**. Model ini akan digunakan untuk mencari buku yang paling mirip berdasarkan kesamaan fitur-fitur teks yang sudah diekstraksi.

6. **Penggabungan Dataset Penilaian dan Buku**:
   - Dataset penilaian (*ratings_clean*) kemudian digabungkan dengan dataset buku (*books_clean*) menggunakan *ISBN* sebagai kunci penghubung, untuk menyediakan data lengkap yang dapat digunakan dalam evaluasi rekomendasi.

- Proses ini penting untuk menghindari kesalahan dalam pemodelan dan memastikan akurasi hasil rekomendasi. Penghapusan nilai kosong, duplikasi, dan pemfilteran data pengguna mencegah error yang dapat mempengaruhi performa model.
- Penggabungan fitur teks bertujuan untuk meningkatkan kualitas rekomendasi dengan menyediakan informasi kaya yang relevan bagi model berbasis konten.

## Modeling
Pada tahap ini, digunakan algoritma **Nearest Neighbors** untuk membangun model rekomendasi berbasis konten. Algoritma ini bekerja dengan menghitung kesamaan antara buku-buku berdasarkan fitur yang telah diekstrak (menggunakan TF-IDF) dan kemudian merekomendasikan buku yang paling mirip.

### Cara Kerja Nearest Neighbors:
Algoritma **Nearest Neighbors** mencari buku yang paling dekat (mirip) dengan buku yang dipilih oleh pengguna. Dalam proyek ini, digunakan metrik **cosine similarity** untuk mengukur seberapa mirip dua buku berdasarkan fitur teks yang telah diekstrak. Cosine similarity menghitung sudut antara dua vektor (dalam hal ini representasi numerik buku) untuk menentukan tingkat kesamaan, dengan hasil berkisar antara 0 (tidak mirip) hingga 1 (sangat mirip).

### Parameter yang Digunakan:
- **metric='cosine'**: Metrik yang digunakan untuk mengukur kesamaan antara buku berdasarkan vektor TF-IDF.
- **algorithm='brute'**: Algoritma brute force digunakan untuk menghitung kesamaan karena dataset relatif kecil, sehingga efisien.
- **n_neighbors=10**: Menentukan jumlah buku yang akan direkomendasikan (Top-N) berdasarkan buku yang dipilih.

### Rekomendasi Top-N Berdasarkan Judul Buku:
Sebagai contoh, untuk buku *"Harry Potter and the Sorcerer's Stone"*, sistem rekomendasi memberikan 10 rekomendasi buku yang paling mirip berdasarkan kesamaan fitur:

- **Harry Potter and the Chamber of Secrets**
- **Harry Potter and the Prisoner of Azkaban**
- **Harry Potter and the Goblet of Fire**
- **Harry Potter and the Order of the Phoenix**
- **Harry Potter and the Half-Blood Prince**
- **Harry Potter and the Deathly Hallows**
- **The Hobbit**
- **The Lord of the Rings**
- **The Chronicles of Narnia**
- **The Golden Compass**

### Rekomendasi Top-N Berdasarkan User ID:
Untuk pengguna dengan **User ID 276798**, berikut rekomendasi 10 buku teratas yang disajikan:
- **Ein Mord für Kay Scarpetta**
- **Ein Fall für Kay Scarpetta.**
- **Brandherd 5 CDs. Ein Fall für Kay Scarpetta.**
- **Blinder Passagier. Ein neuer Fall für Kay Scarpetta.**
- **All That Remains (Kay Scarpetta Mysteries)**
- **Postmortem (Kay Scarpetta Mysteries)**
- **Body of Evidence (Kay Scarpetta Mysteries)**
- **Trace (Kay Scarpetta Mysteries)**
- **POSTMORTEM (Kay Scarpetta Mysteries)**

### Kelebihan dan Kekurangan Nearest Neighbors:
**Kelebihan:**
- Mudah diimplementasikan dan diinterpretasikan.
- Dapat menghasilkan rekomendasi yang relevan jika fitur-fitur buku terdefinisi dengan baik.
  
**Kekurangan:**
- Memiliki skala komputasi yang besar pada dataset yang lebih besar, karena menghitung kesamaan antara setiap pasangan buku.
- Bergantung pada kualitas fitur yang diekstrak. Jika fitur tidak cukup informatif, rekomendasi yang diberikan bisa kurang akurat.

## Evaluation

Model dievaluasi menggunakan tiga metrik utama:
- **Precision**: Mengukur seberapa akurat rekomendasi yang diberikan.
- **Recall**: Mengukur seberapa banyak rekomendasi relevan yang berhasil diambil dari total yang seharusnya.
- **F1-score**: Menggabungkan precision dan recall untuk memberikan metrik tunggal yang seimbang.

### Hasil Evaluasi:
- **Rata-rata Precision**: 0.0817
- **Rata-rata Recall**: 1.0
- **Rata-rata F1-score**: 0.0884

Hasil ini menunjukkan bahwa model memiliki **recall yang sangat tinggi**, artinya model berhasil merekomendasikan hampir semua buku yang relevan kepada pengguna. Namun, **precision yang rendah** menandakan bahwa banyak buku yang direkomendasikan tidak selalu relevan dengan preferensi pengguna, sehingga model memberikan rekomendasi yang terlalu luas atau tidak tepat.

### Dampak Evaluasi terhadap Business Understanding:
1. **Apakah sudah menjawab problem statement?**
   Ya, model ini berhasil menjawab problem statement terkait cara menemukan buku yang relevan untuk pengguna. Dengan recall yang tinggi, model dapat menangkap semua buku yang relevan. Namun, precision yang rendah menunjukkan bahwa sistem masih perlu ditingkatkan agar lebih akurat dalam menyeleksi rekomendasi.

2. **Apakah berhasil mencapai goals yang diharapkan?**
   Model sebagian besar berhasil mencapai tujuan dalam menyediakan rekomendasi buku yang dipersonalisasi berdasarkan preferensi pengguna, meskipun masih ada ruang untuk perbaikan dalam meningkatkan akurasi rekomendasi (precision). Rekomendasi yang dihasilkan mencakup semua buku yang relevan, tetapi jumlah rekomendasi yang tidak sesuai menunjukkan bahwa masih ada langkah perbaikan lebih lanjut.

3. **Apakah solusi statement berdampak?**
   Solusi berbasis content-based filtering dengan Nearest Neighbors memberikan dampak positif dalam hal menyediakan rekomendasi yang relevan. Namun, precision yang rendah menunjukkan bahwa pendekatan ini mungkin perlu dikombinasikan dengan teknik filtering lain (misalnya, collaborative filtering) atau dilakukan penyempurnaan dalam pemrosesan fitur agar model dapat memberikan hasil yang lebih akurat.

**Cara bekerja ketiga metrik tersebut**: 
1. **Precision**: Mengukur proporsi rekomendasi yang relevan dari keseluruhan rekomendasi yang diberikan. Formula:  
   Precision = TP / (TP + FP),  
   di mana TP adalah true positives, dan FP adalah false positives.
   
2. **Recall**: Mengukur seberapa banyak rekomendasi relevan yang berhasil ditangkap. Formula:  
   Recall = TP / (TP + FN),  
   di mana TP adalah true positives, dan FN adalah false negatives.

3. **F1-score**: Kombinasi dari precision dan recall, memberikan metrik yang seimbang. Formula:  
   F1 = 2 * (Precision * Recall) / (Precision + Recall).  
   F1-score sangat berguna saat precision dan recall memiliki nilai yang berbeda.

