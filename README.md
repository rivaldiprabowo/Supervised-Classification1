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
*Catatan: label yang dipilih berdasarkan analisis adalah `is_canceled`
  </p>
