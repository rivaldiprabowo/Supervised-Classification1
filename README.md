# Supervised-Classification
## Hotel Cancellation Cases
### By: Muhamamd Rivaldi Prabowo

![hotel portugal](https://user-images.githubusercontent.com/99151517/160754823-e0d86319-3646-4fb8-baf8-55e5f80593d4.jpg)

## Latar Belakang
<p align='justify' style="font-weight: bold;">
Dalam bisnis proses industri perhotelan secara umum terdapat dua jenis tamu hotel, yakni: 
1. tamu hotel yang langsung check in dan menginap di hotel tanpa melakukan booking terlebih dahulu.
2. tamu hotel yang melakukan booking terlebih dahulu sebelum menginap di hotel.

Dalam konteks bisnis perhotelan, untuk pengunjung hotel di kategori pertama, jika terdapat kamar kosong maka pihak hotel akan langsung menyiapkan segala kebutuhan tamu tersebut. Namun berbeda kasus apabila pengunjung hotel masuk ke dalam kategori kedua, dimana pihak hotel akan menyiapkan beberapa hal untuk menyambut kedatangan mereka, di antaranya:  
>* Menghubungi pengunjung terkait kapan perkiraan datang ke hotel,
>* Membersihkan, merapikan, dan menyiapkan kamar sesuai pesanan pengunjung,
>* Menyiapkan makanan dan minuman untuk menyambut kedatangan pengunjung,
>* Menolak pengunjung lain yang memesan kamar yang telah dipesan (*booked room*), dan
>* Memberi layanan penjemputan di bandara/stasiun/terminal apabila diperlukan.

Sehingga pembatalan pemesanan hotel (`booking hotel cancellation`) akan menjadi sesuatu yang merugikan bagi pihak hotel. Berdasarkan sumber dari <a href="https://championtraveler.com/price/cost-of-a-trip-to-portugal/)">Data Source</a> bahwa rata-rata biaya penginapan semalam di Portugal sebesar 75 USD (Data pada dataset diambil pada hotel di negara Portugal), bayangkan berapa banyak kerugian yang harus ditanggung oleh pihak pengusaha perhotelan apabila banyak calon tamu hotel yang tiba-tiba membatalkan pemesanan kamar hotel.

Oleh karena itu, saya yang dalam kasus ini memposisikan diri sebagai *Data Scientist* di salah satu perusahaan perhotelan ternama di Portugal akan membuat sebuah machine learning yang memiliki tujuan untuk mempelajari calon tamu yang seperti apa yang kemungkinan besar akan melakukan pembatalan pemesanan kamar hotel dan juga untuk mengantisipasi kerugian yang lebih besar.
  </p>

## Pemahaman Proses Bisnis dan Pemilihan Fitur
<p align='justify' style="font-weight: bold;">
Berdasarkan hasil dari proses memahami proses bisnis perhotelan, saya dapat membagi bisnis proses menjadi dua selang waktu, yang tentu saja disesuaikan dengan tujuan dari pembuatan project machine learning ini, yakni analisa pembatalan boking hotel.
1. Proses booking hotel oleh calon tamu hotel hingga check-in
2. Proses check-in hingga check-out.

**Secara logika, tamu hotel hanya dapat membatalkan pesanan hotelnya (is_canceled bernilai 1) hanya pada rentang waktu pemesanan booking hotel hingga sesaat sebelum check-in hotel (selang waktu 1). Sehingga fitur yang dipakai untuk prediksi hanya fitur yang dicatat pada rentang waktu tersebut.** Berikut ini adalah fitur/kolom pada kedua selang waktu yang telah dianalisa sebelumnya (`is_canceled` dan `reservation_status` tidak dimasukan karena termasuk ke dalam label):

1. Fitur yang tercatat selama selang waktu proses booking hotel oleh calon tamu hotel hingga check-in (fitur yang dipakai):
    * adults
    * agent
    * arrival_date_day_of_month
    * arrival_date_month
    * arrival_date_week_number
    * arrival_date_year
    * babies
    * booking_changes
    * children
    * company
    * country
    * customer_type
    * days_in_waiting_list
    * deposit_type
    * distribution_channel
    * hotel
    * is_repeated_guest
    * lead_time
    * market_segment
    * meal
    * previous_bookings_not_canceled
    * previous_cancellations
    * required_car_parking_spaces
    * reserved_room_type
    * total_of_special_requests
    * arrival_date (kolom tambahan)
    * week_lead_time (kolom tambahan)
2. Fitur yang dicatat selama selang waktu proses check-in hingga check-out (fitur yang tidak dipakai):
    * adr
    * assigned_room_type
    * reservation_status_date
    * stays_in_weekend_nights
    * stays_in_week_nights
    
Selanjutnya kita menganalisis dari fitur pada selang waktu nomor 1, mana fitur yang sekiranya secara logika dan secara data tidak mendukung prediksi pembatalan booking hotel ini. Hasil analisa sudah dipaparkan pada bagian sebelumnya, berikut adalah ringkasan dari fitur yang tidak dipakai:
1. Terlalu banyak missing value dan tidak ada hubungan dengan kolom lain (nilainya tidak bisa ditrack di kolom lain): `agent` dan `company`.
2. Hampir semua data pada kolom hanya berisikan satu nilai saja (persentase salah satu nilai unik pada kolom ini sangatlah besar >=95%, sehingga dapat digeneralisasi menjadi satu nilai saja), dan kolom-kolom ini saling berkaitan satu dengan lainnya: `is_repeated_guest`, `previous_bookings_not_canceled`, dan `previous_cancellations`.
Total terdapat lima kolom yang tidak dipakai dalam fitur machine learning.
Setelah melalui tahapan Data Cleansing, terdapat 492 data yang terbuang dari total 119390 data (0,4%), dengan kata lain data masih **tersisa 118898 data atau 99.59% dari total data**.
*Catatan: label yang dipilih berdasarkan analisis adalah `is_canceled`
  </p>

## Kesimpulan Data Analisis
![time series](https://user-images.githubusercontent.com/99151517/160755467-c6dc9698-0c5d-4753-9eb6-bba89041172c.JPG)
![date day](https://user-images.githubusercontent.com/99151517/160755468-4d99c2ff-3b79-4ad6-a03b-5632979efc4d.JPG)
![month](https://user-images.githubusercontent.com/99151517/160755475-d1e7b5af-b70b-42ea-89d1-4cd987599348.JPG)
![proportions](https://user-images.githubusercontent.com/99151517/160755462-78d2126e-256c-4ba7-ad11-df3ce3b9f0ec.JPG)
<p align='justify' style="font-weight: bold;">
Pada kasus di dataset ini, kita hanya tertarik kepada profil para tamu yang kemungkinan besar melakukan cancellation booking hotel. **Berikut adalah hasil analisa profil tamu hotel yang melakukan cancellation booking hotel berdasarkan statistik yang ada:**
* `adults` berjumlah 2 orang yang tidak membawa anak kecil (`children` berjumlah 0) dan bayi (`babies` berjumlah 0).
* Tamu hotel yang tidak menunggu hari reservasi hotel `days_in_waiting_lists`, tidak memesan fasilitas tambahan `total_of_special_requests`, dan tidak memerlukan lahan parkir mobil `required_car_parking_spaces`.
* Para calon tamu hotel banyak melakukan cancellation pada bulan `Juni`, dan `setiap pertengahan bulan`.
* Musim `Summer` menjadi musim yang paling banyak cancellation booking hotel.
* `Hotel City` menjadi hotel dengan pembatalan booking hotel paling banyak.
* Sebanyak **43,86%** cancellation booking hotel dari total data semua tamu hotel berasal dari negara Portugal `PRT`.
* Sebanyak **37%** cancellation booking hotel dari total data semua tamu hotel tidak melakukan deposit `No Deposit` dan juga customer `Transient`.
* Sebanyak **35%** cancellation booking hotel dari total data semua tamu hotel berasal dari distribution channel `TA/TO`.
* Sebanyak **30%** cancellation booking hotel dari total data semua tamu hotel memesan tipe kamar `A` dan juga memilih paket meal `BB`.
* Sebanyak **22%** cancellation booking hotel dari total data semua tamu hotel berasal dari market segment `Online TA`.
  </p>
  
  ## Saran Untuk Management Hotel
1. Karena banyak tamu hotel yang terdiri dari orang dewasa berjumlah dua orang, saya asumsikan bahwa mayoritas tamu yang menginap di hotel merupakan pasangan, sehingga agar lebih menarik calon tamu hotel dan untuk mengurangi booking cancellation, sebaiknya pihak hotel memberikan diskon atau paket yang menarik khusus untuk tamu yang datang secara berpasangan.
2. Karena mayoritas calon tamu yang melakukan booking cancellation tidak melakukan deposit di awal, maka agar menghindari kerugian, sebaiknya pihak management hotel harus memberikan peraturan bahwa setiap calon tamu hotel (baik itu city hotel atau resort hotel) memberikan deposit awal sejumlah 5-20% dari total biaya menginap, baik itu dari pemesanan via apapun (online, travel agent atau apapun itu).
3. Karena banyak pembatalan booking dilakukan pada musim panas/tengah tahun, maka pihak management hotel sebaiknya membuat banyak promo menarik khusus pada waktu tersebut, tujuannya untuk lebih menarik banyak calon tamu hotel. Berikan harga diskon dan fasilitas yang bagus, akan tetapi naikan pula biaya deposit awalnya, agar jika tamu melakukan pembatalan booking maka pihak hotel masih mendapatkan pemasukan.
4. Biaya deposit awal baiknya memang ada di semua jenis reserved_room, tetapi lebih baik jika kamar tipe A memiliki jumlah biaya deposit awal yang lebih besar, karena calon tamu hotel yang membatalkan pesanan banyak memesan kamar tipe ini.

## Kesimpulan Modelling
Secara umum, terdapat dua jenis model yang dilakukan pada project machine learning ini:
1. **Model tanpa Imbalance Learning**
    * Model Algoritma yang dipilih untuk cross validasi adalah: Logistic Regression, Decision Tree Classifier, Random Forest Classifier, dan XGBoost Classifier.
    * Metric evaluasi yang dipakai sesuai dengan kebutuhan bisnis adalah Recall.
    * ![cross_val model 1](https://user-images.githubusercontent.com/99151517/160755821-fb97bac7-3193-488a-9771-349c835c3330.JPG)
    * Model yang terpilih melalui proses cross validasi adalah XGBoost Regressor dengan score Recall sebesar 74.93%.
    * ![model 1 before param](https://user-images.githubusercontent.com/99151517/160755842-5580aeb9-80f9-4d47-9536-c46e62ecbf0f.JPG)
    * Hasil prediksi sebelum dilakukan tunning dan menggunakan data test menunjukan score Recall sebesar 75%.
    * ![model 1 after param](https://user-images.githubusercontent.com/99151517/160755847-3410e9d4-0a4c-4970-b880-55e0363d6c92.JPG)
    * Hasil prediksi setelah dilakukan tunning dengan RandomizedSearchCV (100 kali iterasi) dan menggunakan data test menunjukan score Recall sebesar 76%.
    * Model algoritma yang dipilih pada jenis ini adalah XGBoost Classifier after tunning dengan Recall skor sebesar 76%.


2. **Model Imbalance Learning**
    * Model Algoritma yang dipilih untuk cross validasi adalah: Logistic Regression, Decision Tree Classifier, Random Forest Classifier, dan XGBoost Classifier.
    * Metric evaluasi yang dipakai sesuai dengan kebutuhan bisnis adalah f2_score (karena imbalance learning).
    * Tambahan untuk model Imbalance, sebelumnya dilakukan pemilihan metoda pembobotan nilai/resampling, terdapat tiga jenis metoda yakni: SMOTE-OverSampling, NearMiss-UnderSampling, dan class_weight.
    * ![cross_val model 2](https://user-images.githubusercontent.com/99151517/160756145-ad1bcc1d-5bc4-41ec-bb24-fdd86f4b952c.JPG)
    * Model yang terpilih melalui proses cross validasi adalah XGBoost Regressor dengan metoda pembobotan class_weight serta menunjukkan score f2_score sebesar 82.46%.
    * ![model 2 before param](https://user-images.githubusercontent.com/99151517/160756114-affc9f71-0ad8-403d-bf54-b115d3ec9d71.JPG)
    * Hasil prediksi sebelum dilakukan tunning dan menggunakan data test menunjukan nilai f2_score sebesar 82.78%.
    * ![cross_val model 2](https://user-images.githubusercontent.com/99151517/160756145-ad1bcc1d-5bc4-41ec-bb24-fdd86f4b952c.JPG)
    * Hasil prediksi setelah dilakukan tunning dengan RandomizedSearchCV (100 kali iterasi) dan menggunakan data test menunjukan nilai f2_score sebesar 82.67%.
    * Model algoritma yang dipilih pada jenis ini adalah XGBoost Classifier-class_weight before tunning dengan nilai f2_score sebesar 82.78%.

Berikut adalah kesimpulan dari pemodelan menggunakan dua metode yang berbeda:
1. Model terbaik dari Model tanpa Imbalance adalah XGBoost Classifier after tunning dengan score Recall sebesar 75%
2. Model terbaik dari Model Imbalance adalah XGBoost Classifier class weight before tunning dengan nilai f2_score sebesar 82.78%.
Saya memilih Model Imbalance untuk dianalisa lebih lanjut dan dibuat pemodelan lanjutan (after model based feature selection) karena memberikan skor yang lebih baik daripada Model tanpa Imbalance.

Hasil dari pemodelan lanjutan (after model based feature selection), menunjukan penurunan nilai dari f2_score menjadi 82.45%. Berarti model yang paling cocok untuk project machine learning ini adalah **`XGBoost Classifier-class_weight (Imbalanced Method) before tunning`**.

Setelah dilakukan perbandingan antara Model tanpa Imbalance Learning dengan Model Imbalance Learning, maka model yang dipakai untuk kasus ini adalah:
* **`Model Imbalance Learning`** dengan algoritma machine learning **`XGBoost Classifier-class_weight`** sebagai model benchmarking.
* **`f2_score`** sebagai metric evaluasi dengan score `82.78`.
* ![importance](https://user-images.githubusercontent.com/99151517/160756373-beb543e2-9e47-4ae3-8cb3-8885e99d698a.jpg)
* Berdasarkan **`Feature Importance`**, feature `deposit_type` merupakan fitur yang paling penting dalam model ini, diikuti dengan `required_car_parking_spaces`, `tourist_type`, `market_segment`, dan `total_of_special_request`.

* **`Kombinasi hyperparameter`** yang paling baik di model ini adalah:
    * 'scale_pos_weight': 2 (parameter yang memberikan bobot lebih kepada nilai dengan jumlah lebih sedikit pada label, untuk kasus ini nilai unik majoritas dua kali lebih banyak dari minoritas.)
    * Untuk hyperparameter sisanya hanya menggunakan parameter default dari XGBoost Classifier.

## Analisa Dampak Pemakaian Model Terhadap Peningkatan Bisnis Proses Perhotelan
Pengaruh penggunaan machine learning terhadap performa bisnis dapat dilihat dari berapa banyak keuntungan yang diperoleh ataupun berapa banyak potensi kerugian yang bisa dikurangi dengan pemakaian machine learning. Dalam kasus ini hal yang ingin ditekan adalah kesalahan dalam memprediksi cancellation booking hotel atau tidak *(False Negative)*, dimana hal yang paling merugikan adalah saat kita memprediksi tamu akan datang akan tetapi kenyataannya tidak datang ke hotel (booking cancellation). Potensi kerugian yang dapat ditekan karena pemakaian machine learning adalah sebagai berikut:
* Terdapat 44224 kesalahan prediksi (default) dari total 119390 data (Sekitar 37% kesalahan dalam memprediksi).
* Model machine learning yang dibuat berdasarkan XGBoost Classifier memiliki skor 82.78% atau galat sekitar 17.22%.
* Harga hotel yang dipakai pada perhitungan ini adalah data rata-rata hotel di negara portugal (karena lokasi pengambilan data berada di negara portugal) yaitu sebesar (75 USD, sumber: https://championtraveler.com/price/cost-of-a-trip-to-portugal/).
* **Potensial loss tanpa machine learning** = jumlah kesalahan prediksi x harga rata-rata hotel = 44224 x 75 USD = **3.316.800 USD**.
* **Potensial loss dengan machine learning** = persentase galat machine learning x total data x harga rata-rata hotel = 17.22% x 119390 x 75 USD = **1.541.921 USD**.

**`Dapat dilihat bahwa penggunaan machine learning dapat mengurangi potensial loss dari 3.316.800 USD menjadi hanya 1.541.921 USD atau dapat mengurangi potensial loss sebesar 1.774.879 USD (dalam persentase berarti kerugian dapat berkurang hingga 53.51%).`**
