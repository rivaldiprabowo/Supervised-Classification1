# Supervised-Classification
## Hotel Cancellation Cases
### By: Muhamamd Rivaldi Prabowo

<p align="center">
<img src="https://https://github.com/rivaldiprabowo/Supervised-Classification1/blob/main/hotel%20portugal.jpg">
</p>

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
