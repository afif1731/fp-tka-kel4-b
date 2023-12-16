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

- App Workers

  Droplet app worker dibuat menggunakan image Ubuntu versi 22.04 (LTS) sebanyak dua buah. Meskipun droplet worker1 dan worker2 yang kami gunakan memiliki spesifikasi yang berbeda, konfigurasi yang dilakukan tetaplah sama. Cara masuk ke console worker tidak berbeda dengan database, cukup dengan menggunakan perintah `ssh`.

  Berikut merupakan langkah - langkah mengatur droplet worker

  + Setelah masuk ke console, buatlah file bernama `app.py` dengan isi berupa salinan [app.py](https://github.com/fuaddary/fp-tka/blob/main/app.py) dari soal FP.

    String yang digunakan pada app diubah menjadi string yang kita gunakan untuk mengakses database sebelumnya, namun dengan memberi tambahan ```/orders_db?authSource=admin``` agar app.py dapat melakukan autentikasi sekaligus terhubung dengan database orders_db. Isi dari `app.py` akan menjadi seperti berikut:

    ![isi_apppy](https://github.com/afif1731/fp-tka-kel4-b/blob/main/gambars/apppyisi.png)

  + Membuat file `gunicorn_config.py`

    `gunicorn` ini digunakan untuk menjalankan app.py secara daemon. Dengan demikian, kita dapat mengakses app.py meskipun console ditutup. Isi dari file `gunicorn_config.py` adalah sebagai berikut:

    ```
    # ini config gunicorn
    import os

    workers = int(os.environ.get('GUNICORN_PROCESSES', '2'))

    threads = int(os.environ.get('GUNICORN_THREADS', '4'))

    bind = os.environ.get('GUNICORN_BIND', '0.0.0.0:80')

    forwarded_allow_ips = '<YOUR LOAD BALANCER IPv4>' # Digunakan agar tidak semua orang dapat mengakses app.py ini

    secure_scheme_headers = { 'X-Forwarded-Proto': 'http' }
    ```

  + Mencatat keperluan di `requirement.txt`

    Agar penginstallan dependensi python yang diperlukan dapat dilakukan secara langsung maka semua dependensi dicatat pada satu buah file bernama `requirement.txt`, hal ini juga berguna apabila suatu saat terjadi error dan semua dependensi terhapus. Dependensi yang perlu dicatat pada file antara lain adalah sebagai berikut:

    ```
    Flask
    flask_pymongo
    bson
    gunicorn
    ```

  + Memasang firewall

    Ketika app.py nantinya dijalankan, sistem akan mendengarkan request akses dari IP manapun, termasuk dari IP yang tidak kita kenal sekalipun. Karena app.py kita terhubung dengan database, kita perlu memasang pelindung agar database kita tidak dibanjiri dengan data asing serta menghindari penghapusan dari pengguna tak dikenal. Firewall dipasang dari Digital Ocean dengan detail sebagai berikut:

    ![firewall_app](https://github.com/afif1731/fp-tka-kel4-b/blob/main/gambars/firewallapp.png)

    IP `157.245.147.229` adalah IP dari load balancer kami, sementara IP lainnya merupakan IPv4 dari laptop user yang saat itu sedang membuat app worker. IP milik kita sendiri dapat dicari melalui situs [https://whatismyipaddress.com](https://whatismyipaddress.com). namun IPv4 kita selalu berubah setiap beberapa jam, sehingga apabila ingin menambahkan IP kita sendiri ke firewall, kita harus melakukan update setiap kali ip kita berubah.

  + Menjalankan `app.py`

    Sebelum menjalankan aplikasi python ini, kami menggunakan virtual environment untuk menginstall dependensi yang diperlukan pada lingkungan yang terisolasi, dengan demikian error yang mungkin terjadi dari dependensi yang diinstall dapat direset cukup dengan menghapus virtual environment. Berikut merupakan rincian kami dalam membuat dan menggunakan virtual environment python

    - Menginstall `venv`
      
      ```
      sudo apt install python3-venv
      ```

    - Membuat virtual environment

      ```
      venv flaskapp
      ```
      nantinya akan muncul sebuah direktori bernama `flaskapp`

    - Menggunakan virtual environment

      ```
      source flaskapp/bin/activate
      ```
      nantinya sebelum tulisan root@ip pada input akan terdapat tambahan '**(flaskapp)**'

    Setelah virtual environment digunakan, install semua dependensi yang ada di `requirement.txt`
    ```
    pip3 install -r requirement.txt
    ```
    Begitu dependensi selesai diinstall, maka app.py sudah siap untuk dijalankan.

    Untuk menjalankan `app.py` gunakan perintah berikut, tambahan `-D` tidak perlu digunakan agar aplikasi tidak berjalan secara daemon, hal ini digunakan agar saat testing kami dapat melakukan debugging dengan mudah.
    ```
    gunicorn --config gunicorn_config.py -D app:app
    ```
    langkah-langkah ini dilakukan sekali lagi untuk pengaturan pada worker2

- Load Balancer

  Pembuatan load balancer menggunakan droplet dengan image nginx yang sudah tersedia di Digital Ocean.

  ![create_nginx](https://github.com/afif1731/fp-tka-kel4-b/blob/main/gambars/createnginx.png)

  Untuk mencegah akses dari user tidak dikenal, maka dibuatlah firewall yang hanya menerima request dari IPv4 terdaftar. Cara ini mungkin kurang efektif karena IPv4 dari user yang sama akan selalu berubah setiap beberapa jam, namun kami belum menemukan cara untuk mendaftarkan user tertentu pada firewall sedemikian rupa sehingga user dapat tetap mengakses meskipun IP-nya berubah.

  Berikut adalah pengaturan firewall yang dipasang pada load balancer kami.

  ![firewall_nginx](https://github.com/afif1731/fp-tka-kel4-b/blob/main/gambars/firewallnginx.png)

  Setelah melakukan pemasangan firewall, masuk ke dalam console droplet seperti biasa. Di sana, program default nginx seharusnya sedang aktif, apabila tidak aktif maka ada kemungkinan bahwa terjadi kesalahan saat membuat droplet.

  Kemudian dilakukan pengubahan default config nginx sehingga nginx dapat mengakses droplet worker1 dan 2 yang telah dibuat sebelumnya. Pengubahan config nginx dilakukan pada file `default.conf` yang dapat diakses melalui perintah berikut

  ```
  nano /etc/nginx/conf.d/default.conf
  ```
  isi dari `default.conf` diubah menjadi seperti berikut

  ```
  upstream app {
    least conn;
    server 165.232.170.202:80;
    server 159.223.66.135:80;
  }

  server {
    listen 80;
    server_name localhost;

    location / {
      proxy_pass http://app;
    }
  }
  ```
  `165.232.170.202:80` merupakan alamat worker1 kami dan `159.223.66.135:80` merupakan alamat worker2
  
  Service Nginx kemudian dimatikan dan dinyalakan ulang dengan perintah berikut:
  ```
  systemctl stop nginx
  ```
  ```
  systemctl start nginx
  ```

  Untuk memastikan service nginx telah aktif, gunakan
  ```
  systemctl status nginx
  ```

  Hasilnya adalah sebagai berikut:

  ![status_nginx](https://github.com/afif1731/fp-tka-kel4-b/blob/main/gambars/statusnginx.png)

  Setelah load balancer nginx berhasil dijalankan, maka keseluruhan arsitektur cloud telah dibuat dan siap untuk diuji.
   
### Pengujian Endpoint

Semua pengujian diarahkan pada ip yang sama, yaitu `157.245.147.229`

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
- **Test 1**

  Peak Concurency: 5000

  Spawn Rate: 25

  ![Screenshot 2023-12-15 145823](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/2a447ab8-8b59-45d1-bdde-41e7d1f6a355)

  Hasil:
    + Highest RPS: 78,9
  
- **Test 2**

  Peak Concurency: 10000

  Spawn Rate: 25

  ![Screenshot 2023-12-15 150017](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/e3aa46ad-6a1c-4584-be62-fc4ceb023d20)

  Hasil:
    + Peak Concurrency before failure: 800
  
- **Test 3**

  Peak Concurency: 10000

  Spawn Rate: 50

  ![Screenshot 2023-12-15 150108](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/02da8199-e562-4beb-b97b-e244b6db73ec)

  Hasil:
    + Peak Concurrency before failure: 950
  
- **Test 4**

  Peak Concurency: 10000

  Spawn Rate: 100

  ![Screenshot 2023-12-15 150150](https://github.com/afif1731/fp-tka-kel4-b/assets/128958228/4aa11380-0b57-4c23-8c65-393621a03a0d)

  Hasil:
    + Peak Concurrency before failure: 1300

### Kesimpulan

- Pengujian dengan Locust menunjukkan bahwa failure memiliki kemungkinan besar terjadi ketika jumlah user barada di antara 800 - 1000

- Arsitektur Awan dengan spesifikasi sedemikian masih tidak sanggup untuk menghadapi 1000 user sekaligus
  
