# Laporan Final Project TKA (Lagi diedit afif, tolong jangan diapa2in)

### Oleh: Kelompok 4 Kelas B

Anggota:
|        Nama        |    NRP     |
| :----------------  | :--------  |
|   Atha Rahma A.    | 5027221030 |
| Gavriel Pramuda K. | 5027221031 |
|   Muhammad Afif    | 5027221032 |
|  Azzahra Sekar R.  | 5027221035 |
|   Michael Wayne    | 5027221037 |

### Permasalahan

Anda adalah seorang lulusan Teknologi Informasi, sebagai ahli IT, salah satu kemampuan yang harus dimiliki adalah Keampuan merancang, membangun, mengelola aplikasi berbasis komputer menggunakan layanan awan untuk memenuhi kebutuhan organisasi.(menurut kurikulum IT ITS 2023 😙)

Pada suatu saat teman anda ingin mengajak anda memulai bisnis di bidang digital marketing, anda diberikan sebuah aplikasi berbasis API File: app.py dengan spesifikasi yang tertera pada soal

### Rancangan Arsitektur Cloud

- Berikut adalah rancangan arsitektur yang telah kami buat untuk final project kami:
  ![Screenshot 2023-12-15 094659](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/502b2b63-6d21-4ecd-81f4-b86bca61ee81)

- Kami menggunakan Digital Ocean sebagai lingkungan cloud yang akan kami gunakan. Berikut adalah tabel harga spesifikasi VM yang kami buat:
  
  ![tabel_harga](https://github.com/afif1731/fp-tka-kel4-b/blob/main/gambars/tabelfix.png)

### Implementasi Arsitektur Cloud

- Droplets
  ![Screenshot 2023-12-15 093626](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/4de9f2f3-4749-4e98-b22a-c1675920e624)

- Firewall
  ![Screenshot 2023-12-15 095004](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/d99239c1-f52c-4b34-8b54-213805ecdf68)

- MongoDB database
  ![Screenshot 2023-12-15 093746](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/76641009-436b-4057-b05f-14a7132cb20f)

### Langkah Pembuatan

- MongoDB database

  Sesuai dengan yang tertera pada tabel spesifikasi di atas, kami membuat droplet dengan image MongoDB serta spesifikasi yang telah ditentukan sebelumnya

  ![create_mongo](https://github.com/afif1731/fp-tka-kel4-b/blob/main/gambars/createmongo.png)

  image MongoDB yang tersedia di Digital Ocean merupakan VM ubuntu yang telah terinstall mongodb di dalamnya. Tak hanya itu, instalasi mongodb yang ada juga disertai dengan akun admin yang digunakan untuk mengakses database dari manapun. Keamanan image ini juga cukup terjamin karena secara default sistem akan memblokir semua port selain 80 (http), 443 (https), dan juga 27017 (mongodb). Namun supaya lebih meyakinkan, kami juga menambahkan firewall tambahan dari Digital Ocean pada droplet ini sehingga database hanya dapat diakses melalui port 22 dan 27017 saja.

  ![firewall_db](https://github.com/afif1731/fp-tka-kel4-b/blob/main/gambars/firewalldb.png)

  Setelah droplet dibuat, maka akan didapat address IPv4 dari droplet tersebut, IP tersebut digunakan untuk masuk ke dalam console droplet. Caranya cukup dengan menjalankan perintah berikut pada cmd ataupun terminal semacam:

  ```
  ssh root@<your_IPv4>
  ```
  Apabila droplet diatur menggunakan password, maka console kemudian juga akan meminta input password sebelum kemudian kita dapat masuk ke dalam console droplet.

  Setelah masuk ke dalam console droplet, maka kita seharusnya mendapat pesan seperti ini

  ![mongo_console](https://github.com/afif1731/fp-tka-kel4-b/blob/main/gambars/mongoconsole.png)

  Pesan juga akan menampilkan password admin yang digunakan untuk mengakses database. Perlu diperhatikan agar password tersebut tidak terpublikasi.

  Untuk memeriksa apakah database sudah berjalan, dilakukan percobaan akses menggunakan dua cara. Yaitu dengan menggunakan `mongosh` dan `MongoDB Compass`

  + `mongosh`
    Cukup masukkan perintah `mongosh` ke dalam console droplet. Apabila kemudian muncul text `test` pada bagian input, maka hal itu menunjukkan bahwa koneksi sudah berhasil dan kita sedang mengakses database `test`

    ![mongosh_tes](https://github.com/afif1731/fp-tka-kel4-b/blob/main/gambars/mongoshtes.png)

  + `MongoDB Compass`
    Untuk menggunakan compass, pastikan aplikasi **MongoDB Compass** sudah terinstall sebelumnya. Apabila telah terinstall, buat koneksi baru dan masukkan string link akses database yang sebelumnya ditunjukkan pada pesan login console

   ![compass_tes](https://github.com/afif1731/fp-tka-kel4-b/blob/main/gambars/compasstes.png)

   Apabila sudah berhasil terhubung, maka tampilan seharusnya berupa seperti ini

   ![compass_res](https://github.com/afif1731/fp-tka-kel4-b/blob/main/gambars/compasstes2.png)

  Kemudian dibuat sebuah database baru sesuai dengan ketentuan soal FP dengan nama **orders_db** beserta collection **orders**. Database juga diisi dengan sejumlah dummy data yang dapat dibuat dari berbagai sumber di internet, salah satunya adalah melalui [Mockaroo](mockaroo.com).
   
### Pengujian Endpoint

- Get All Orders
  ![Screenshot 2023-12-15 093931](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/870551b1-c5bb-4196-8de9-77bb48b6a07e)

- Get a Specific Orders by ID
  ![Screenshot 2023-12-15 094051](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/522943b5-084e-4074-9523-d22261b8491e)

- Create a New Order
  ![Screenshot 2023-12-15 094136](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/fb9e68ca-5688-4bb1-b9cc-2541a87f5a15)

- Update an Order by ID
  ![Screenshot 2023-12-15 094218](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/8634b57d-423b-44e7-93fd-f18cbb474765)

- Delete an Order by ID
  ![Screenshot 2023-12-15 094354](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/37004177-3a40-4c46-90ca-d1181559e2f2)

### Loadtesting menggunakan Locust
- Peak Concurency: 5000
  Spawn Rate: 25
  ![Screenshot 2023-12-15 145823](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/2a447ab8-8b59-45d1-bdde-41e7d1f6a355)
- Peak Concurency: 10000
  Spawn Rate: 25
  ![Screenshot 2023-12-15 150017](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/e3aa46ad-6a1c-4584-be62-fc4ceb023d20)
- Peak Concurency: 10000
  Spawn Rate: 50
  ![Screenshot 2023-12-15 150108](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/02da8199-e562-4beb-b97b-e244b6db73ec)
- Peak Concurency: 10000
  Spawn Rate: 100
  ![Screenshot 2023-12-15 150150](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/4aa11380-0b57-4c23-8c65-393621a03a0d)

