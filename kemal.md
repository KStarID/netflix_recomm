# Laporan Proyek Machine Learning - Kemal
## Project Overview
Sistem rekomendasi telah menjadi komponen penting dalam berbagai platform digital saat ini, terutama dalam industri hiburan seperti Netflix. Dengan pertumbuhan konten yang sangat pesat, pengguna sering kali kesulitan menemukan konten yang sesuai dengan preferensi mereka. Netflix, sebagai salah satu platform streaming terbesar di dunia, memiliki ribuan film dan acara TV yang membuat pengguna kewalahan dalam memilih konten yang ingin ditonton.

Proyek ini bertujuan untuk mengembangkan sistem rekomendasi yang dapat membantu pengguna menemukan konten Netflix yang sesuai dengan preferensi mereka. Dengan memanfaatkan dataset Netflix Shows yang berisi informasi tentang film dan acara TV yang tersedia di platform tersebut, sistem rekomendasi ini akan memberikan saran konten yang mungkin disukai pengguna berdasarkan karakteristik konten atau pola menonton pengguna lain.

Menurut penelitian yang dilakukan oleh Gomez-Uribe dan Hunt (2015), sistem rekomendasi Netflix bertanggung jawab atas sekitar 80% dari konten yang ditonton oleh pengguna platform tersebut [1]. Hal ini menunjukkan betapa pentingnya sistem rekomendasi yang efektif dalam meningkatkan pengalaman pengguna dan engagement pada platform streaming.

Selain itu, McKinsey melaporkan bahwa sistem rekomendasi yang baik dapat meningkatkan pendapatan hingga 35% melalui peningkatan engagement dan retensi pengguna [2]. Dengan demikian, pengembangan sistem rekomendasi yang efektif tidak hanya bermanfaat bagi pengguna tetapi juga bagi platform itu sendiri.

## Referensi
[1] C. A. Gomez-Uribe and N. Hunt, "The Netflix Recommender System: Algorithms, Business Value, and Innovation," ACM Transactions on Management Information Systems, vol. 6, no. 4, pp. 1-19, 2015.
[2] McKinsey & Company, "How retailers can keep up with consumers," McKinsey & Company, 2013.

## Business Understanding
### Problem Statements
Berdasarkan latar belakang di atas, berikut adalah rumusan masalah yang akan diselesaikan dalam proyek ini:

- Bagaimana cara membantu pengguna menemukan konten Netflix yang sesuai dengan preferensi mereka di tengah banyaknya pilihan yang tersedia?
- Bagaimana cara memanfaatkan informasi konten (seperti genre, deskripsi, aktor, dll.) untuk memberikan rekomendasi yang relevan?

### Goals
Tujuan dari proyek ini adalah:

- Mengembangkan sistem rekomendasi yang dapat menyarankan konten Netflix yang relevan dengan preferensi pengguna.
- Memanfaatkan informasi konten untuk membuat sistem rekomendasi berbasis konten (content-based filtering) yang dapat merekomendasikan konten serupa dengan yang disukai pengguna.
- Melakukan test case dan mendapatkan analisis dari model yang telah dibuat.

### Solution Statements
Untuk mencapai tujuan di atas, berikut adalah pendekatan solusi yang akan diimplementasikan yaitu Content-Based Filtering:

1. Content-Based Filtering
   
   - Menggunakan teknik TF-IDF (Term Frequency-Inverse Document Frequency) untuk mengekstrak fitur dari deskripsi konten dan genre.
   - Menghitung kesamaan kosinus (cosine similarity) antara konten untuk menemukan konten yang serupa.
   - Merekomendasikan konten yang memiliki kesamaan tertinggi dengan konten yang disukai pengguna.

## Data Understanding

Dataset yang digunakan dalam proyek ini adalah Netflix Shows dataset yang diambil dari Kaggle. Dataset ini berisi informasi tentang film dan acara TV yang tersedia di platform Netflix hingga tahun 2021. Dataset dapat diakses melalui tautan berikut: [Netflix Shows Dataset](https://www.kaggle.com/datasets/shivamb/netflix-shows/data)

Dataset ini terdiri dari 8807 baris dan 12 kolom, dengan setiap baris mewakili satu konten (film atau acara TV) yang tersedia di Netflix. Dataset ini mencakup berbagai informasi seperti judul, jenis konten, direktur, aktor, negara, tanggal rilis, rating, durasi, genre, dan deskripsi.

Berikut adalah deskripsi variabel-variabel dalam dataset:
- show_id : ID unik untuk setiap konten di Netflix.
- type : Jenis konten (Film atau Acara TV).
- title : Judul konten.
- director : Nama direktur konten.
- cast : Daftar aktor yang berperan dalam konten.
- country : Negara tempat konten diproduksi.
- date_added : Tanggal konten ditambahkan ke Netflix.
- release_year : Tahun rilis konten.
- rating : Rating konten (seperti TV-MA, PG-13, dll.).
- duration : Durasi konten (dalam menit untuk Film, dalam season untuk Acara TV).
- listed_in : Genre atau kategori konten.
- description : Deskripsi singkat tentang konten.

## Exploratory Data Analysis (EDA)
Untuk memahami dataset dengan lebih baik, beberapa analisis eksplorasi data telah dilakukan:

 1. Distribusi Jenis Konten

Dari analisis, ditemukan bahwa dataset terdiri dari 69.1% Film dan 30.9% Acara TV. Hal ini menunjukkan bahwa Netflix memiliki lebih banyak Film dibandingkan Acara TV dalam katalognya.

 2. Distribusi Tahun Rilis

Analisis menunjukkan bahwa mayoritas konten di Netflix dirilis setelah tahun 2000, dengan peningkatan signifikan pada konten yang dirilis setelah tahun 2015. Hal ini mencerminkan fokus Netflix pada konten yang lebih baru.

 3. Distribusi Genre

Genre yang paling umum di Netflix adalah International Movies, Dramas, dan Comedies. Hal ini menunjukkan bahwa Netflix memiliki fokus yang kuat pada konten internasional dan drama.

 4. Analisis Missing Values

Dari analisis missing values, ditemukan bahwa beberapa kolom memiliki nilai yang hilang:

- director: 30.7% missing
- cast: 10.2% missing
- country: 8.3% missing
- date_added: 0.1% missing
- rating: 0.5% missing
Nilai yang hilang ini perlu ditangani dalam tahap data preparation.

## Data Preparation
Beberapa teknik data preparation yang dilakukan dalam proyek ini adalah:

### 1. Penanganan Missing Values
Untuk menangani nilai yang hilang dalam dataset, beberapa pendekatan diambil:

- Untuk kolom director dan cast yang memiliki banyak nilai hilang, nilai yang hilang diisi dengan string "Unknown" karena informasi ini mungkin tidak tersedia untuk semua konten.
- Untuk kolom country , nilai yang hilang diisi dengan "Unknown" karena informasi ini mungkin tidak tersedia untuk semua konten.
- Untuk kolom date_added , nilai yang hilang diisi dengan tanggal median dari dataset karena hanya sedikit nilai yang hilang.
- Untuk kolom rating , nilai yang hilang diisi dengan "Not Rated" karena mungkin konten tersebut belum mendapatkan rating.

Penanganan missing values ini penting untuk memastikan bahwa model tidak mengalami masalah saat memproses data.

### 2. Text Preprocessing
Untuk kolom teks seperti description dan listed_in (genre), dilakukan preprocessing teks untuk mempersiapkan data untuk model content-based filtering. Preprocessing teks ini melibatkan konversi ke huruf kecil, penghapusan tanda baca, dan penghapusan spasi berlebih. Hal ini penting untuk mengurangi noise dalam data teks dan meningkatkan kualitas fitur yang diekstrak.

### 3. Feature Engineering
Untuk model collaborative filtering, perlu dibuat dataset interaksi pengguna-konten. Karena dataset asli tidak memiliki informasi rating dari pengguna, dibuat dataset simulasi interaksi pengguna. Feature engineering ini penting untuk mempersiapkan data untuk model collaborative filtering, yang membutuhkan informasi tentang interaksi pengguna dengan konten.

## Modeling
Dalam proyek ini, dua pendekatan sistem rekomendasi diimplementasikan: Content-Based Filtering dan Collaborative Filtering.

### 1. Content-Based Filtering
Model Content-Based Filtering merekomendasikan konten berdasarkan kesamaan karakteristik konten. Dalam implementasi ini, digunakan teknik TF-IDF untuk mengekstrak fitur dari deskripsi konten dan genre, kemudian dihitung kesamaan kosinus antara konten.

Kelebihan:

- Tidak memerlukan data dari pengguna lain, sehingga dapat memberikan rekomendasi untuk konten baru atau yang jarang ditonton.
- Dapat memberikan rekomendasi yang sangat spesifik berdasarkan karakteristik konten.
- Transparan, karena rekomendasi didasarkan pada kesamaan konten yang dapat dijelaskan.
Kekurangan:

- Tidak dapat merekomendasikan konten di luar preferensi yang sudah diketahui pengguna.
- Tidak memperhitungkan faktor popularitas atau kualitas konten.
- Terlalu bergantung pada metadata konten yang tersedia.

## Evaluation
Dalam proyek ini, evaluasi dilakukan untuk mengukur seberapa baik model rekomendasi dalam memberikan rekomendasi yang relevan kepada pengguna. Beberapa metrik evaluasi yang umum digunakan dalam sistem rekomendasi adalah:

1. **Precision**: Mengukur proporsi item yang direkomendasikan yang relevan dengan preferensi pengguna.
2. **Recall**: Mengukur proporsi item relevan yang berhasil direkomendasikan dari seluruh item relevan yang tersedia.
3. **F1-Score**: Rata-rata harmonik dari precision dan recall, memberikan keseimbangan antara kedua metrik tersebut.
4. **Mean Average Precision (MAP)**: Mengukur rata-rata precision pada berbagai tingkat recall.
5. **Normalized Discounted Cumulative Gain (NDCG)**: Mengukur kualitas peringkat rekomendasi dengan mempertimbangkan posisi item yang relevan.

### Result
Pada proyek ini, hasil rekomendasi yang diberikan adalah top-10 judul yang diurutkan berdasarkan skor tertinggi sesuai dengan judul yang telah dipilih oleh pengguna.

#### Content-Based Filtering
***TESTCASE 1 - CONTENT-BASED FILTERING***
Karena Anda menyukai film/acara TV 'Stranger Things', mungkin Anda juga menyukai:
1. Beyond Stranger Things - TV Show - Stand-Up Comedy & Talk Shows, TV Mysteries, TV Sci-Fi & Fantasy (Skor Kesamaan: 0.5122)
2. Safe Haven - Movie - Dramas, Romantic Movies (Skor Kesamaan: 0.2104)
3. The Umbrella Academy - TV Show - TV Action & Adventure, TV Mysteries, TV Sci-Fi & Fantasy (Skor Kesamaan: 0.2024)
4. Sleepless Society: Nyctophobia - TV Show - International TV Shows, TV Mysteries, TV Thrillers (Skor Kesamaan: 0.2021)
5. Anjaan: Special Crimes Unit - TV Show - International TV Shows, TV Horror, TV Mysteries (Skor Kesamaan: 0.2010)
6. Manifest - TV Show - TV Dramas, TV Mysteries, TV Sci-Fi & Fantasy (Skor Kesamaan: 0.1862)
7. The 4400 - TV Show - TV Dramas, TV Mysteries, TV Sci-Fi & Fantasy (Skor Kesamaan: 0.1829)
8. The OA - TV Show - TV Dramas, TV Mysteries, TV Sci-Fi & Fantasy (Skor Kesamaan: 0.1804)
9. Sakho & Mangane - TV Show - Crime TV Shows, International TV Shows, TV Dramas (Skor Kesamaan: 0.1742)
10. Kiss Me First - TV Show - British TV Shows, Crime TV Shows, International TV Shows (Skor Kesamaan: 0.1741)

***TESTCASE 2 - CONTENT-BASED FILTERING***
Karena Anda menyukai film/acara TV 'Breaking Bad', mungkin Anda juga menyukai:
1. Better Call Saul - TV Show - Crime TV Shows, TV Comedies, TV Dramas (Skor Kesamaan: 0.3195)
2. Hormones - TV Show - International TV Shows, Romantic TV Shows, TV Dramas (Skor Kesamaan: 0.2386)
3. The Show - Movie - Dramas (Skor Kesamaan: 0.1881)
4. Servant of the People - TV Show - International TV Shows, TV Comedies (Skor Kesamaan: 0.1809)
5. Have You Ever Fallen in Love, Miss Jiang? - TV Show - Crime TV Shows, International TV Shows, TV Dramas (Skor Kesamaan: 0.1793)
6. W/ Bob & David - TV Show - TV Comedies (Skor Kesamaan: 0.1700)
7. Dear White People - TV Show - TV Comedies, TV Dramas (Skor Kesamaan: 0.1696)
8. Tunnel - TV Show - Crime TV Shows, International TV Shows, TV Dramas (Skor Kesamaan: 0.1686)
9. ThirTEEN Terrors - TV Show - International TV Shows, TV Horror, TV Mysteries (Skor Kesamaan: 0.1667)
10. The Judgement - TV Show - Crime TV Shows, International TV Shows, TV Dramas (Skor Kesamaan: 0.1638)

### Analisis Hasil Evaluasi

Untuk mengevaluasi hasil rekomendasi, kami menggunakan metrik precision. Precision dihitung dengan rumus:

**Precision = (Jumlah Rekomendasi yang Relevan / Total Rekomendasi) × 100%**

Dalam konteks ini, rekomendasi dianggap relevan jika memiliki genre yang sama atau serupa dengan konten yang disukai pengguna.

**Evaluasi Content-Based Filtering:**

**Testcase 1 (Stranger Things)**: 
   - Genre utama: TV Mysteries, TV Sci-Fi & Fantasy
   - Dari 10 rekomendasi, 8 memiliki genre yang sama atau serupa
   - Precision@10 = (8/10) × 100% = 80%

**Testcase 2 (Breaking Bad)**:
   - Genre utama: Crime TV Shows, TV Dramas
   - Dari 10 rekomendasi, 7 memiliki genre yang sama atau serupa (Crime TV Shows, TV Dramas)
   - Precision@10 = (7/10) × 100% = 70%

## Kesimpulan
Dalam proyek ini, telah berhasil diimplementasikan pendekatan sistem rekomendasi untuk konten Netflix dengan Content-Based Filtering. Pendekatan ini memiliki kelebihan dan kekurangan, dan dapat digunakan untuk memberikan rekomendasi yang relevan kepada pengguna.

Untuk pengembangan lebih lanjut, dapat dipertimbangkan untuk mengimplementasikan pendekatan hybrid, menggunakan data interaksi pengguna yang lebih besar, dan menambahkan fitur-fitur tambahan seperti popularitas konten dan tren terkini.

**---Ini adalah bagian akhir laporan---**