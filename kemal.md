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
Untuk mencapai tujuan di atas, berikut adalah pendekatan solusi yang akan diimplementasikan:

Content-Based Filtering:
   
- Menggunakan teknik TF-IDF (Term Frequency-Inverse Document Frequency) untuk mengekstrak fitur dari deskripsi konten dan genre.
- Menghitung kesamaan kosinus (cosine similarity) antara konten untuk menemukan konten yang serupa.
- Merekomendasikan konten yang memiliki kesamaan tertinggi dengan konten yang disukai pengguna.

## Data Understanding

Dataset yang digunakan dalam proyek ini adalah Netflix Shows dataset yang diambil dari Kaggle. Dataset ini berisi informasi tentang film dan acara TV yang tersedia di platform Netflix hingga tahun 2021. Dataset dapat diakses melalui tautan berikut: [Netflix Shows Dataset](https://www.kaggle.com/datasets/shivamb/netflix-shows/data)

Dataset ini terdiri dari 8806 baris dan 12 kolom, dengan setiap baris mewakili satu konten (film atau acara TV) yang tersedia di Netflix. Dataset ini mencakup berbagai informasi seperti judul, jenis konten, direktur, aktor, negara, tanggal rilis, rating, durasi, genre, dan deskripsi.

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

- director: 29.91% missing
- cast: 9.37% missing
- country: 9.44% missing
- date_added: 0.11% missing
- rating: 0.05% missing
- duration: 0.03% missing
Nilai yang hilang ini perlu ditangani dalam tahap data preparation.

## Data Preparation

Tahap data preparation sangat penting untuk memastikan data siap digunakan dalam pemodelan sistem rekomendasi. Berikut adalah langkah-langkah data preparation yang dilakukan sesuai dengan urutan di notebook:

### 1. Penanganan Missing Values

Langkah pertama dalam data preparation adalah menangani nilai-nilai yang hilang (missing values) dalam dataset. Dari analisis sebelumnya, diketahui bahwa beberapa kolom memiliki nilai yang hilang, terutama pada kolom 'director' (30.7%), 'cast' (10.2%), dan 'country' (8.3%). Penanganan missing values dilakukan dengan pendekatan berikut:

- Kolom 'director' diisi dengan 'Unknown Director' untuk menggantikan nilai yang hilang. Hal ini penting karena informasi direktur mungkin tidak tersedia untuk semua konten, tetapi kolom ini tetap diperlukan untuk fitur content-based filtering.
- Kolom 'cast' diisi dengan 'Unknown Cast' untuk menggantikan nilai yang hilang. Pendekatan ini memungkinkan model tetap mempertimbangkan konten meskipun informasi pemain tidak lengkap.
- Kolom 'country' diisi dengan 'Unknown Country' untuk menggantikan nilai yang hilang, memastikan konsistensi data geografis.
- Kolom 'description' diisi dengan 'No description available' untuk menggantikan nilai yang hilang, karena deskripsi sangat penting untuk content-based filtering.
- Kolom 'date_added' diisi dengan 'Unknown' untuk menggantikan nilai yang hilang.
- Kolom 'rating' diisi dengan 'Not Rated' untuk menggantikan nilai yang hilang, memberikan label yang jelas untuk konten tanpa rating.

Penanganan missing values ini memastikan bahwa dataset lengkap dan siap untuk pemrosesan lebih lanjut, menghindari error saat pemodelan dan meningkatkan kualitas rekomendasi.

### 2. Pembuatan Fitur Content-Based

Setelah menangani missing values, langkah selanjutnya adalah membuat fitur gabungan untuk content-based filtering. Pada tahap ini, dibuat kolom baru bernama 'content_features' yang menggabungkan informasi dari beberapa kolom penting:

```python
df['content_features'] = df['director'] + ' ' + df['cast'] + ' ' + df['listed_in'] + ' ' + df['description']
```

### 3. Text Preprocessing
Langkah ketiga adalah melakukan preprocessing pada fitur teks gabungan yang telah dibuat. Preprocessing teks dilakukan dengan fungsi clean_text yang menerapkan serangkaian teknik pemrosesan teks dalam urutan berikut:

1. Konversi ke Huruf Kecil : Semua teks diubah menjadi huruf kecil untuk memastikan konsistensi dan menghindari duplikasi kata yang sama dengan kapitalisasi berbeda. Contoh: 'Action' dan 'action' akan dianggap sebagai kata yang sama.
2. Penghapusan Karakter Khusus dan Angka : Karakter non-alfanumerik dan angka dihapus dari teks untuk mengurangi noise. Ini membantu fokus pada kata-kata yang bermakna dan menghilangkan simbol-simbol yang tidak relevan untuk analisis konten.
3. Tokenisasi : Teks dipecah menjadi token atau kata-kata individual menggunakan NLTK tokenizer. Proses ini penting untuk memungkinkan analisis pada tingkat kata dan penerapan teknik NLP lainnya.
4. Penghapusan Stopwords : Kata-kata umum yang tidak memberikan informasi penting (seperti "the", "and", "is") dihapus dari teks. Penghapusan stopwords mengurangi dimensi data dan memfokuskan analisis pada kata-kata yang lebih informatif dan diskriminatif, meningkatkan efektivitas model content-based.
5. Stemming : Kata-kata diubah ke bentuk akarnya menggunakan Porter Stemmer. Proses ini menggabungkan varian kata yang sama (misalnya "running", "runs", "ran" menjadi "run"), mengurangi dimensionalitas dan meningkatkan kemampuan model untuk mengenali konten serupa meskipun diekspresikan dengan bentuk kata yang berbeda.
Hasil dari preprocessing teks ini adalah kolom baru 'content_features_cleaned' yang berisi teks yang telah dibersihkan dan distandarisasi. Preprocessing teks ini sangat penting untuk meningkatkan kualitas fitur yang akan digunakan dalam model TF-IDF dan perhitungan kesamaan konten.

### 4. Simulasi Data Rating

Untuk keperluan pengembangan sistem rekomendasi, dibuat simulasi data rating pengguna terhadap konten Netflix. Simulasi ini penting untuk menguji dan mengevaluasi model rekomendasi dalam skenario nyata. Langkah-langkah simulasi data rating adalah sebagai berikut:

1. Membuat 1000 ID pengguna unik (user_1 hingga user_1000)
2. Untuk setiap pengguna, secara acak dipilih 20-50 item dari dataset
3. Untuk setiap item yang dipilih, diberikan rating acak antara 1-5 dengan distribusi probabilitas yang condong ke rating tinggi (lebih realistis)
4. Hasil simulasi menghasilkan dataset ratings_df dengan 34575 data rating

Simulasi data rating ini memungkinkan pengujian model rekomendasi dengan data yang menyerupai pola rating pengguna di dunia nyata, meskipun data tersebut dibuat secara sintetis.

## Modeling
Dalam proyek ini, sistem rekomendasi diimplementasikan menggunakan pendekatan Content-Based Filtering.

### Content-Based Filtering
Model Content-Based Filtering merekomendasikan konten berdasarkan kesamaan karakteristik konten. Dalam implementasi ini, digunakan teknik TF-IDF untuk mengekstrak fitur dari deskripsi konten dan genre, kemudian dihitung kesamaan kosinus antara konten.

Kelebihan:

- Tidak memerlukan data dari pengguna lain, sehingga dapat memberikan rekomendasi untuk konten baru atau yang jarang ditonton.
- Dapat memberikan rekomendasi yang sangat spesifik berdasarkan karakteristik konten.
- Transparan, karena rekomendasi didasarkan pada kesamaan konten yang dapat dijelaskan.

Kekurangan:

- Tidak dapat merekomendasikan konten di luar preferensi yang sudah diketahui pengguna.
- Tidak memperhitungkan faktor popularitas atau kualitas konten.
- Terlalu bergantung pada metadata konten yang tersedia.

### Result
Pada proyek ini, hasil rekomendasi yang diberikan adalah top-10 judul yang diurutkan berdasarkan skor tertinggi sesuai dengan judul yang telah dipilih oleh pengguna.

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

## Evaluation
Dalam proyek ini, evaluasi dilakukan untuk mengukur seberapa baik model rekomendasi dalam memberikan rekomendasi yang relevan kepada pengguna. Beberapa metrik evaluasi yang umum digunakan dalam sistem rekomendasi adalah:

1. **Precision**: Mengukur proporsi item yang direkomendasikan yang relevan dengan preferensi pengguna.
2. **Recall**: Mengukur proporsi item relevan yang berhasil direkomendasikan dari seluruh item relevan yang tersedia.
3. **F1-Score**: Rata-rata harmonik dari precision dan recall, memberikan keseimbangan antara kedua metrik tersebut.
4. **Mean Average Precision (MAP)**: Mengukur rata-rata precision pada berbagai tingkat recall.
5. **Normalized Discounted Cumulative Gain (NDCG)**: Mengukur kualitas peringkat rekomendasi dengan mempertimbangkan posisi item yang relevan.

### Analisis Hasil Evaluasi

Untuk mengevaluasi hasil rekomendasi, kami menggunakan metrik precision. Precision dihitung dengan rumus:

**Precision = (Jumlah Rekomendasi yang Relevan / Total Rekomendasi) × 100%**

Dalam konteks ini, rekomendasi dianggap relevan jika memiliki genre yang sama atau serupa dengan konten yang disukai pengguna.

**Evaluasi Content-Based Filtering:**

**Testcase 1 (Stranger Things)**: 
   - Genre utama: TV Mysteries, TV Sci-Fi & Fantasy
   - Dari 10 rekomendasi, 8 memiliki genre yang sama atau serupa
   - Precision@10 = (7/10) × 100% = 70%

**Testcase 2 (Breaking Bad)**:
   - Genre utama: Crime TV Shows, TV Dramas
   - Dari 10 rekomendasi, 7 memiliki genre yang sama atau serupa (Crime TV Shows, TV Dramas)
   - Precision@10 = (8/10) × 100% = 80%

## Kesimpulan
Dalam proyek ini, telah berhasil diimplementasikan pendekatan sistem rekomendasi untuk konten Netflix dengan Content-Based Filtering. Pendekatan ini memiliki kelebihan dan kekurangan, dan dapat digunakan untuk memberikan rekomendasi yang relevan kepada pengguna.

Untuk pengembangan lebih lanjut, dapat dipertimbangkan untuk mengimplementasikan pendekatan hybrid, menggunakan data interaksi pengguna yang lebih besar, dan menambahkan fitur-fitur tambahan seperti popularitas konten dan tren terkini.

**---Ini adalah bagian akhir laporan---**