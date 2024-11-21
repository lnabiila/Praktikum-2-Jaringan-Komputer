[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/99wpTe72)
| Name           | NRP        | Kelas     |
| ---            | ---        | ----------|
| Hilmi Fawwaz Sa'ad | 5025221103 | Jaringan Komputer (A) |
| Lathiifah Nabiila Bakhtiar | 5025221130 | Jaringan Komputer (A) |

## Find your topology here!

- Link: https://drive.google.com/drive/folders/1ECQD6-cQkg0DzyflG-jSxJZaGaxg0KSU?usp=sharing

- Topology distribution for groups: https://docs.google.com/spreadsheets/d/1QKEZjixTStNbdXznOalJoJS0UQ6ed23o51pP8t8eAIM/edit?gid=1757558734#gid=1757558734

## Put your topology config image here!

![1](https://github.com/user-attachments/assets/e007c763-8228-49b4-a937-85682d9e0ccc)

<br>

## Soal 1

> Topologi terdiri dari node Wortel yang berupa DNS Master*. Selain itu, terdapat pula node Pokcoy sebagai DNS Slave*, yang bertugas sebagai cadangan dari node Wortel.
> <br> </br>
> Selanjutnya terdapat node Tomat dan Taoge yang bekerja sebagai Client*, tiga buah Web Server* yaitu Bayam, Buncis, dan Brokoli, serta Mayur sebagai Router*. Buatlah topologi sesuai dengan pembagian topologi [di sini](https://docs.google.com/spreadsheets/d/1QKEZjixTStNbdXznOalJoJS0UQ6ed23o51pP8t8eAIM/edit?usp=sharing) dan konfigurasi topologi [di sini](https://drive.google.com/drive/folders/1ECQD6-cQkg0DzyflG-jSxJZaGaxg0KSU?usp=sharing). Pastikan bahwa setiap node dapat terhubung ke Internet.

> _The topology consists of a Wortel node which is a DNS Master*. In addition, there is also a Pokcoy node as a DNS Slave*, which serves as a backup for the Wortel node._
> <br> </br>
> _Furthermore, there are Tomat and Taoge nodes that work as Client*, three Web Servers*, namely Bayam, Buncis, and Brokoli, then finally Mayur as Router*. Make a topology according to the topology division [here](https://docs.google.com/spreadsheets/d/1QKEZjixTStNbdXznOalJoJS0UQ6ed23o51pP8t8eAIM/edit?usp=sharing) and the topology configuration [here](https://drive.google.com/drive/folders/1ECQD6-cQkg0DzyflG-jSxJZaGaxg0KSU?usp=sharing). Make sure that each node can connect to the Internet._

**Answer:**

- Screenshot
  
  ![image](https://github.com/user-attachments/assets/9adc0dab-5e31-4947-a523-65202ebebde0)

- Explanation
  
  Konfigurasi untuk setiap node
  #### Router
  - MayurRouter
    ```
    auto eth0
    iface eth0 inet dhcp

    auto eth1
    iface eth1 inet static
    	address 10.14.1.1
    	netmask 255.255.255.0
    
    auto eth2
    iface eth2 inet static
    	address 10.14.2.1
    	netmask 255.255.255.0
    
    auto eth3
    iface eth3 inet static
    	address 10.14.3.1
    	netmask 255.255.255.0
    
    up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.14.0.0/16
    ```

  #### DNS
  - WortelDNSMaster
    ```
    auto eth0
    iface eth0 inet static
    	address 10.14.3.2
    	netmask 255.255.255.0
      gateway 10.14.3.1
      
    up echo 'nameserver 192.168.122.1' > /etc/resolv.conf
    ```
  - PokcoyDNSSlave
    ```
    auto eth0
    iface eth0 inet static
    	address 10.14.2.5
    	netmask 255.255.255.0
      gateway 10.14.2.1
      
    up echo 'nameserver 192.168.122.1' > /etc/resolv.conf
    ```

  #### Server
  - BayamWebServer
    ```
    auto eth0
    iface eth0 inet static
    	address 10.14.2.2
    	netmask 255.255.255.0
      gateway 10.14.2.1
    
    up echo 'nameserver 192.168.122.1' > /etc/resolv.conf
    ```
  - BuncisWebServer
    ```
    auto eth0
    iface eth0 inet static
    	address 10.14.2.3
    	netmask 255.255.255.0
      gateway 10.14.2.2
      
    up echo 'nameserver 192.168.122.1' > /etc/resolv.conf
    ```
  - BrokoliWebServer
    ```
    auto eth0
    iface eth0 inet static
    	address 10.14.2.4
    	netmask 255.255.255.0
      gateway 10.14.2.1
      
    up echo 'nameserver 192.168.122.1' > /etc/resolv.conf
    ```
      
  #### Client
  - TomatClient
    ```
    auto eth0
    iface eth0 inet static
    	address 10.14.1.2
    	netmask 255.255.255.0
      gateway 10.14.1.1
    
    up echo 'nameserver 192.168.122.1' > /etc/resolv.conf
    ```
  - TaugeClient
    ```
    auto eth0
    iface eth0 inet static
      address 10.14.1.3
    	netmask 255.255.255.0
    	gateway 10.14.1.1
      
    up echo 'nameserver 192.168.122.1' > /etc/resolv.conf
    ```
    
  Untuk mengecek apakah node sudah terhubung ke internet, dapat mengetes dengan ping google.com

  ![image](https://github.com/user-attachments/assets/7e0bf52c-12a7-4c6c-957a-8d436f68ff49)

<br>

## Soal 2

> Tambahkan konfigurasi untuk domain bayam.yyy.com yang mengarah ke IP node Bayam di DNS Master. Dengan cara yang sama, buat konfigurasi domain brokoli.yyy.com yang mengarah ke IP node Brokoli dan domain buncis.yyy.com yang mengarah ke IP node Buncis. Simpan semua konfigurasi dalam folder Jarkom. Selama pengerjaan soal, ubah yyy menjadi kode kelompok masing-masing (contoh: A02).
> <br> </br>
> Jangan lupa update konfigurasi kedua client agar dapat berkomunikasi dengan semua domain tersebut.


> _Add a configuration for bayam.yyy.com domain that points to the Bayam node IP in the DNS Master. In the same way, create a brokoli.yyy.com domain configuration that points to the Brokoli node IP and a buncis.yyy.com domain that points to the Buncis node IP. Save all configurations in a folder called Jarkom. For this practicum, substitute yyy with the code of each group (ex: A02).
> <br> </br> 
> Don't forget to update the configuration of both clients so that they can communicate with the domains._

**Answer:**

- Screenshot
  
  ![image](https://github.com/user-attachments/assets/d53d1558-ce71-47bf-b482-f1d4416be46f)

- Explanation
  
  #### Pada WortelDNSMaster
  Pertama, lakukan update dan install bind9 dengan command
  ```
  apt-get update
  apt-get install bind9 -y
  ```
  Kemudian, buat folder jarkom dan copy db.local
  ```
  mkdir /etc/bind/jarkom

  cp /etc/bind/db.local /etc/bind/jarkom/bayam.A13.com
  cp /etc/bind/db.local /etc/bind/jarkom/buncis.A13.com
  cp /etc/bind/db.local /etc/bind/jarkom/brokoli.A13.com
  ```
  Buat zone DNS pada file /etc/bind/named.conf.local
  ```
  zone "bayam.A13.com" {
        type master;
        file "/etc/bind/jarkom/bayam.A13.com";
  };

  zone "buncis.A13.com" {
        type master;
        file "/etc/bind/jarkom/buncis.A13.com";
  };

  zone "brokoli.A13.com" {
        type master;
        file "/etc/bind/jarkom/brokoli.A13.com";
  };
  ```
  Atur konfigurasi untuk tiap domain
  - /etc/bind/jarkom/bayam.A13.com
    ```
    $TTL    604800
    @       IN      SOA     bayam.A13.com. root.bayam.A13.com. (
                                  2         ; Serial
                             604800         ; Refresh
                              86400         ; Retry
                            2419200         ; Expire
                             604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      bayam.A13.com.
    @       IN      A       10.14.2.2
    @       IN      AAAA    ::1
    ```
  - /etc/bind/jarkom/buncis.A13.com
    ```
    $TTL    604800
    @       IN      SOA     buncis.A13.com. root.buncis.A13.com. (
                                  2         ; Serial
                             604800         ; Refresh
                              86400         ; Retry
                            2419200         ; Expire
                             604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      buncis.A13.com.
    @       IN      A       10.14.2.3
    @       IN      AAAA    ::1
    ```
  - /etc/bind/jarkom/brokoli.A13.com
    ```
    $TTL    604800
    @       IN      SOA     brokoli.A13.com. root.brokoli.A13.com. (
                                  2         ; Serial
                             604800         ; Refresh
                              86400         ; Retry
                            2419200         ; Expire
                             604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      brokoli.A13.com.
    @       IN      A       10.14.2.3
    @       IN      AAAA    ::1
    ```
  Lalu, restart bind9
  ```
  service bind9 restart
  ```

  #### Pada Client
  ```
  echo 'nameserver 10.14.3.2' > /etc/resolv.conf

  ping bayam.A13.com -4 -c 5
  ping brokoli.A13.com -4 -c 5
  ping buncis.A13.com -4 -c 5
  ```
<br>

## Soal 3

> Tambahkan domain alias berupa www.bayam.yyy.com pada alamat bayam.yyy.com dan www.brokoli.yyy.com pada alamat brokoli.yyy.com.

> _Add a domain alias in the form of www.bayam.yyy.com to the bayam.yyy.com address and www.brokoli.yyy.com to the brokoli.yyy.com address._

**Answer:**

- Screenshot
  
  ![image](https://github.com/user-attachments/assets/0db32624-669a-440d-876c-8fd1570f8afc)

- Explanation
  
  #### Pada WortelDNSMaster
  Atur konfigurasi untuk
  - /etc/bind/jarkom/bayam.A13.com
    ```
    $TTL    604800
    @       IN      SOA     bayam.A13.com. root.bayam.A13.com. (
                                  2         ; Serial
                             604800         ; Refresh
                              86400         ; Retry
                            2419200         ; Expire
                             604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      bayam.A13.com.
    @       IN      A       10.14.2.2
    www     IN      CNAME   bayam.A13.com.
    ```
  - /etc/bind/jarkom/brokoli.A13.com
    ```
    $TTL    604800
    @       IN      SOA     brokoli.A13.com. root.brokoli.A13.com. (
                                  2         ; Serial
                             604800         ; Refresh
                              86400         ; Retry
                            2419200         ; Expire
                             604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      brokoli.A13.com.
    @       IN      A       10.14.2.4
    www     IN      CNAME   brokoli.A13.com.
    ```
  Lalu, restart bind9
  ```
  service bind9 restart
  ```

  #### Pada Client
  ```
  echo 'nameserver 10.14.3.2' > /etc/resolv.conf

  ping www.bayam.A13.com -c 5
  ping www.brokoli.A13.com -c 5
  ping www.buncis.A13.com -c 5
  ```
<br>

## Soal 4

> Tambahkan record reverse domain untuk domain brokoli.yyy.com dan buncis.yyy.com.

> _Add a reverse domain record for brokoli.yyy.com and buncis.yyy.com domains._

**Answer:**

- Screenshot
  
  ![image](https://github.com/user-attachments/assets/d6329202-9727-4fde-98d7-945c2949a223)

- Explanation
  
  #### Pada WortelDNSMaster
  Pertama, copy db.local
  ```
  cp /etc/bind/db.local /etc/bind/jarkom/2.14.10.in-addr.arpa
  ```
  Tambahkan zone DNS untuk record reverse domain di /etc/bind/named.conf.local
  ```
  zone "2.14.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/2.14.10.in-addr.arpa";
  };
  ```
  Atur konfigurasi untuk /etc/bind/jarkom/2.14.10.in-addr.arpa
    ```
    $TTL    604800
    @       IN      SOA     buncis.A13.com. root.buncis.A13.com. (
                                  2         ; Serial
                             604800         ; Refresh
                              86400         ; Retry
                            2419200         ; Expire
                             604800 )       ; Negative Cache TTL
    ;
    2.14.10.in-addr.arpa       IN      NS      buncis.A13.com.
    3                          IN      PTR     buncis.A13.com.
    4                          IN      PTR     brokoli.A13.com.
    ```
  Lalu, restart bind9
  ```
  service bind9 restart
  ```
    
  #### Pada Client
  ```
  echo 'nameserver 192.168.122.1' > /etc/resolv.conf
  
  apt-get update
  apt-get install dnsutils

  echo 'nameserver 10.14.3.2' > /etc/resolv.conf

  host -t PTR 10.14.2.3
  host -t PTR 10.14.2.4
  ```
<br>

## Soal 5

> Ubah record SOA dari domain bayam.yyy.com sesuai dengan ketentuan berikut:
> - Lama waktu server slave menunggu untuk mengecek salinan baru server master adalah sebesar 2 jam.
> - Field yang mengatur revisi file zona ini diubah menjadi tanggal awal praktikum (format YYYYMMDD) kemudian diikuti dengan nomor kelompok (contoh untuk kelompok A02 maka nomornya 02).
> - Lamanya waktu server harus menunggu untuk meminta pembaruan lagi dari nameserver master yang tidak responsif sebesar 30 menit.
> - Lama waktu nama domain di-cache secara lokal sebelum kadaluarsa dan kembali ke nameserver otoritatif untuk informasi terbaru sebesar 12 jam.
> - Jika server slave tidak mendapatkan respons dari server master dalam waktu 2 minggu, server tersebut harus berhenti merespons kueri untuk zona tersebut.

> _Change the SOA record of the bayam.yyy.com domain according to the following conditions:_
> - The length of time the slave server waits to check for a new revision of the master server is 2 hours.
> - The field that regulates the revision of this zone file is changed to the start date of the practicum (YYYYMMDD format) then followed by the group number (ex: for A02 the group number would be 02).
> - The length of time the server has to wait to request another update from an unresponsive master nameserver is 30 minutes.
> - The length of time a domain name is cached locally before it expires and returns to an authoritative nameserver for up-to-date information is 12 hours.
> - If the slave server does not get a response from the master server within 2 weeks, it must stop responding to queries for that zone.

**Answer:**

- Screenshot
  
  ![image](https://github.com/user-attachments/assets/cde91bfa-7fc8-449d-8094-9d5c86bcf073)

- Explanation
  
    Ubah konfigurasi untuk /etc/bind/jarkom/bayam.A13.com
    ```
    $TTL    604800
    @       IN      SOA     bayam.A13.com. root.bayam.A13.com. (
                            2024100113      ; Serial
                            7200            ; Refresh
                            1800            ; Retry
                            1209600         ; Expire
                            43200  )        ; Negative Cache TTL
    ;
    @       IN      NS      bayam.A13.com.
    @       IN      A       10.14.2.2
    www     IN      CNAME   bayam.A13.com.
    ```
    - Lama waktu server slave menunggu untuk mengecek salinan baru server master adalah sebesar 2 jam.
      -> `Refresh = 2 jam = 7200 detik`
    - Field yang mengatur revisi file zona ini diubah menjadi tanggal awal praktikum (format YYYYMMDD) kemudian diikuti dengan nomor kelompok.
      -> `Serial = 2024100113`
    - Lamanya waktu server harus menunggu untuk meminta pembaruan lagi dari nameserver master yang tidak responsif sebesar 30 menit.
      -> `Retry = 30 menit = 1800 detik`
    - Lama waktu nama domain di-cache secara lokal sebelum kadaluarsa dan kembali ke nameserver otoritatif untuk informasi terbaru sebesar 12 jam.
      -> `Negative Cache TTL = 12 jam = 43200 detik`
    - Jika server slave tidak mendapatkan respons dari server master dalam waktu 2 minggu, server tersebut harus berhenti merespons kueri untuk zona tersebut.
      -> `Expire = 2 minggu = 1209600 detik`
<br>

## Soal 6

> Untuk menangani request yang berlebih dari client ke ketiga alamat yang tadi dibuat, konfigurasikan node Pokcoy sebagai DNS Slave yang bekerja untuk DNS Master Wortel.

> _To handle excess requests from the client to the three addresses created, configure the Pokcoy node as the DNS Slave that works for Wortel DNS Master._

**Answer:**

- Screenshot
  
  ![image](https://github.com/user-attachments/assets/7e63b6a6-27c9-4455-a8f9-316337b8d1f3)

- Explanation
  
  #### Pada WortelDNSMaster
  Edit zone pada /etc/bind/named.conf.local
  ```
  zone "bayam.A13.com" {
        type master;
        notify yes;
        also-notify { 10.14.2.5; }; 
        allow-transfer { 10.14.2.5; }; 
        file "/etc/bind/jarkom/bayam.A13.com";
  };
  
  zone "brokoli.A13.com" {
          type master;
          notify yes;
          also-notify { 10.14.2.5; }; 
          allow-transfer { 10.14.2.5; }; 
          file "/etc/bind/jarkom/brokoli.A13.com";
  };

  zone "buncis.A13.com" {
          type master;
          notify yes;
          also-notify { 10.14.2.5; }; 
          allow-transfer { 10.14.2.5; }; 
          file "/etc/bind/jarkom/buncis.A13.com";
  };
  ```
  Lalu, restart bind9
  ```
  service bind9 restart
  ```

  #### Pada PokcoyDNSSlave
  Lakukan update dan install bind9 dengan command
  ```
  apt-get update
  apt-get install bind9 -y
  ```
  Edit zone pada /etc/bind/named.conf.local
  ```
  zone "bayam.A13.com" {
          type slave;
          file "/etc/bind/slave/bayam.A13.com";
          masters { 10.14.3.2; }; 
  };
  
  zone "buncis.A13.com" {
          type slave;
          file "/etc/bind/slave/buncis.A13.com";
          masters { 10.14.3.2; }; 
  };

  zone "brokoli.A13.com" {
          type slave;
          file "/etc/bind/slave/brokoli.A13.com";
          masters { 10.14.3.2; }; 
  };
  ```
  Lalu, restart bind9
  ```
  service bind9 restart
  ```
  
  Setelah itu `service bind9 stop` pada WortelDNSMaster

  #### Pada client
  ```
  echo 'nameserver 10.14.2.5' > /etc/resolv.conf

  ping bayam.A13.com -c 5
  ping brokoli.A13.com -c 5
  ping buncis.A13.com -c 5
  ```
<br>

## Soal 7

> Karena membutuhkan tempat untuk menyimpan resep brokoli, buatlah subdomain berupa vitamin.brokoli.yyy.com dengan alias www.vitamin.brokoli.yyy.com dengan mendelegasikannya dari Wortel ke Pokcoy dengan alamat IP menuju Brokoli yang diatur di folder Vitamin.

> _Since we need a place to store Brokoli recipes, create a subdomain in the form of vitamin.brokoli.yyy.com with an alias of www.vitamin.brokoli.yyy.com by delegating it from Wortel to Pokcoy with an ip to the Brokoli node in a folder called Vitamin._

**Answer:**

- Screenshot
  
  ![image](https://github.com/user-attachments/assets/9fef78e8-8c27-431d-93de-e2fdfc7328cb)

- Explanation
  
  #### Pada WortelDNSMaster
  Edit file /etc/bind/named.conf.options
  ```
  options {
  	directory "/var/cache/bind";
  
  	dnssec-validation auto;
  
  	auth-nxdomain no;
  	listen-on { any; };
  	allow-query { any; };
  };
  ```
  Edit zone pada /etc/bind/named.conf.local
  ```
  zone "brokoli.A13.com" {
          type master;
          file "/etc/bind/jarkom/brokoli.A13.com";
          allow-transfer { 10.14.2.5; }; 
  };
  ```
  Tambahkan konfigurasi untuk /etc/bind/jarkom/brokoli.A13.com
  ```
  ns1      IN    A    10.14.2.5
  vitamin  IN    NS   ns1
  ```
  Lalu, restart bind9
  ```
  service bind9 restart
  ```
  
  #### Pada PokcoyDNSSlave
  Edit file /etc/bind/named.conf.options
  ```
  options {
  	directory "/var/cache/bind";
  
  	dnssec-validation auto;
  
  	auth-nxdomain no;
  	listen-on { any; };
  	allow-query { any; };
  };
  ```
  Kemudian, buat folder vitamin dan copy db.local
  ```
  mkdir /etc/bind/vitamin
  
  cp /etc/bind/db.local /etc/bind/vitamin/vitamin.brokoli.A13.com
  ```
  Buat zone DNS pada file /etc/bind/named.conf.local
  ```
  zone "vitamin.brokoli.A13.com" {
          type master;
          file "/etc/bind/vitamin/vitamin.brokoli.A13.com";
  };
  ```
  Atur konfigurasi untuk /etc/bind/vitamin/vitamin.brokoli.A13.com
    ```
    $TTL    604800
    @       IN      SOA     vitamin.brokoli.A13.com. root.vitamin.brokoli.A13.com. (
                                  2         ; Serial
                             604800         ; Refresh
                              86400         ; Retry
                            2419200         ; Expire
                             604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      vitamin.brokoli.A13.com.
    @       IN      A       10.14.2.5
    www     IN      CNAME   vitamin.brokoli.A13.com.
    ```
  Lalu, restart bind9
  ```
  service bind9 restart
  ```

  #### Pada client
  ```
  echo 'nameserver 10.14.2.5' > /etc/resolv.conf

  ping www.vitamin.brokoli.A13.com -c 5
  ```
<br>

## Soal 8

> Buatlah subdomain khusus untuk kandungan brokoli dengan akses k1.vitamin.brokoli.yyy.com dengan alias www.k1.vitamin.brokoli.yyy.com yang mengarah ke IP brokoli dan diatur di folder k1.  

> _Create a special subdomain for Brokoli content called k1.vitamin.brokoli.yyy.com with an alias called www.k1.vitamin.brokoli.yyy.com that point to Brokoli node and are organized in a folder called k1._

**Answer:**

- Screenshot
  
  ![image](https://github.com/user-attachments/assets/0d771326-cdb0-4c14-b3d9-a159d8fa62cf)

- Explanation
  
  #### Pada PokcoyDNSSlave
  Buat folder k1 dan copy db.local
  ```
  mkdir /etc/bind/k1
  
  cp /etc/bind/db.local /etc/bind/k1/k1.vitamin.brokoli.A13.com
  ```
  Buat zone DNS pada file /etc/bind/named.conf.local
  ```
  zone "k1.vitamin.brokoli.A13.com" {
          type master;
          file "/etc/bind/k1/k1.vitamin.brokoli.A13.com";
  };
  ```
  Atur konfigurasi untuk /etc/bind/k1/k1.vitamin.brokoli.A13.com
    ```
    $TTL    604800
    @       IN      SOA     k1.vitamin.brokoli.A13.com. root.k1.vitamin.brokoli.A13.com. (
                                  2         ; Serial
                             604800         ; Refresh
                              86400         ; Retry
                            2419200         ; Expire
                             604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      k1.vitamin.brokoli.A13.com.
    @       IN      A       10.14.2.5
    www     IN      CNAME   k1.vitamin.brokoli.A13.com.
    k1      IN      A       10.14.2.5
    www.k1  IN      A       10.14.2.5
    ```
  Lalu, restart bind9
  ```
  service bind9 restart
  ```
  
  #### Pada client
  ```
  echo 'nameserver 10.14.2.5' > /etc/resolv.conf

  ping www.k1.vitamin.brokoli.A13.com -c 5
  ```
<br>

## Soal 9

> Bayam, Brokoli, dan Buncis masing-masing berfungsi sebagai web server nginx yang menyajikan resep khusus untuk jenis sayuran yang mereka tangani. Untuk mengaktifkan web server pada masing-masing worker, lakukan deployment website menggunakan sumber yang tersedia di sayur_webserver_nginx. Tambahkan konfigurasi untuk log error ke file /var/log/nginx/error.log dan log access ke file /var/log/nginx/access.log.

> _Bayam, Brokoli, and Buncis each function as nginx web servers that serve special recipes for the type of vegetables they handle. To activate the web server on each worker, do the deployment using the resources available in sayur_webserver_nginx. Add configuration for error log to the file /var/log/nginx/error.log and access log to the file /var/log/nginx/access.log._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/5813c7d6-09af-46d5-b9cc-08411d48ac2e)
  ![image](https://github.com/user-attachments/assets/835c79a8-c9b1-4f9f-931b-bc9577533d50)
  ![image](https://github.com/user-attachments/assets/a762a961-eeb0-4eea-adc0-adea4fa3ab05)

- Explanation
  
  #### Pada masing-masing server
  Pertama, lakukan instalasi
  ```
  apt-get update
  apt install unzip -y
  apt-get install nginx php php-fpm -y
  apt-get install lynx
  ```
  Buat folder root
  ```
  /var/www/jarkom
  ```
  Download file dan unzip
  ```
  URL_ZIP="https://docs.google.com/uc?export=download&id=1tFDk7pKRQLd3BMUcyvfAfEL-drvIxdSl"
  wget --no-check-certificate -O sayur.zip $URL_ZIP
  unzip sayur.zip -d sayur
  cp -r sayur/sayur_webserver_nginx/* /var/www/jarkom
  rm -rf sayur.zip sayur
  ```
  Tambahkan ip server pada /etc/hosts agar bisa diakses
  ```
  10.14.2.x  (server).A13.com
  ```
  Start nginx dan edit konfigurasi /etc/nginx/sites-available/(server).A13.com
  ```
  service nginx start
  ```
  ```
  server {
  	listen 80;
  	root /var/www/jarkom;
  
  	index index.php;
  	server_name (server).A13.com;
  
  	location / {
  		try_files $uri $uri/ /index.php?$query_string;
  	}
  	
  	location ~ \.php$ {
  		include snippets/fastcgi-php.conf;
  		fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
  	}
  
  	error_log /var/log/nginx/error.log;
  	access_log /var/log/nginx/access.log;
  }
  ```
  Lalu, konfigurasi nginx dan pastikan konfigurasi nginx valid
  ```
  ln -s /etc/nginx/sites-available/(server).A13.com /etc/nginx/sites-enabled
  rm -f /etc/nginx/sites-enabled/default
  
  service php7.2-fpm start
  service nginx reload
  service nginx restart
  
  nginx -t
  ```

  #### Pada client
  Install lynx
  ```
  apt-get update
  apt-get install lynx
  ```
  Tambahkan ip setiap server pada /etc/hosts agar bisa diakses
  ```
  10.14.2.2  bayam.A13.com
  10.14.2.3  buncis.A13.com
  10.14.2.4  brokoli.A13.com
  ```
  Jalankan lynx (server).A13.com untuk melakukan pengecekan
<br>

## Soal 10

> Pada masing masing worker nginx, akan terdapat beberapa hal yang perlu diperbaiki pada resource yang diberikan untuk bisa menampilkan resep saat halaman dimuat. Analisis kesalahan yang ada di resource melalui file /var/log/nginx/error.log dan perbaiki hingga halaman bisa menampilkan resep sesuai dengan worker nya.

> _On each nginx worker, there will be several things that need to be fixed in the resources provided to be able to display recipes when the page is loaded. Analyze the errors in the resource through the /var/log/nginx/error.log file and fix it until the page can display recipes according to its worker._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/75abe3fa-ff76-4379-a064-2b0483840a88)
  ![image](https://github.com/user-attachments/assets/72c7dbd2-9370-45e1-b371-a8ebdd5cdbb4)
  ![image](https://github.com/user-attachments/assets/ae16abd9-6844-47e4-bbc6-fb19ee2c1692)

- Explanation

  Edit /var/www/jarkom/index.php di setiap server dengan menambahkan WebServer pada setiap percabangan `case` dan menghapus `_` pada `resep_` di setiap percabangan `if` sehingga switch casenya menjadi seperti berikut
  ```
  switch ($hostname) {
      case 'BayamWebServer':
          if (!@include 'resep1.php') {
              echo "<p>Gagal Memuat Resep! Cek Error Log untuk info lebih lanjut.</p>";
              error_log("File resep_1.php tidak ditemukan!\n", 3, "/var/log/nginx/error.log");
          }
          break;
      case 'BuncisWebServer':
          if (!@include 'resep2.php') {
              echo "<p>Gagal Memuat Resep! Cek Error Log untuk info lebih lanjut.</p>";
              error_log("File resep_2.php tidak ditemukan!\n", 3, "/var/log/nginx/error.log");
          }
          break;
      case 'BrokoliWebServer':
          if (!@include 'resep3.php') {
              echo "<p>Gagal Memuat Resep! Cek Error Log untuk info lebih lanjut.</p>";
              error_log("File resep_3.php tidak ditemukan!\n", 3, "/var/log/nginx/error.log");
          }
          break;
      default:
          echo "<p>Gagal Memuat Resep! Cek Error Log untuk info lebih lanjut.</p>";
          error_log("Hostname tidak ditemukan!\n", 3, "/var/log/nginx/error.log");
          break;
  }
  ```
  Jalankan lynx (server).A13.com untuk melakukan pengecekan
<br>

## Soal 11

> Setelah website berhasil dideploy pada masing-masing worker (Bayam, Brokoli, dan Buncis) dan halaman dapat menampilkan resep sayuran yang sesuai,  buatlah custom access log ke file /var/log/nginx/access.log di masing-masing web server worker menggunakan format log tertentu seperti di bawah:
> - Tanggal dan waktu akses dalam format standar log.
Nama worker yang sedang dilayani (misalnya: Bayam, Brokoli, atau Buncis).
> - Alamat IP klien yang mengakses website.
> - Metode HTTP dan URI yang diakses oleh klien.
> - Status respons HTTP yang diberikan oleh server.
> - Jumlah byte yang dikirimkan dalam respons.
> - Waktu yang dihabiskan oleh server untuk menangani permintaan.
> <br> </br>
> Contoh format log yang sesuai: 
[01/Oct/2024:11:30:45 +0000] Jarkom Node Bayam Access from 192.168.1.15 using method "GET /resep/bayam HTTP/1.1" returned stat


> _After successfully deploying the website on each worker (Bayam, Brokoli, and Buncis) and ensuring the pages display the appropriate vegetable recipes, create a custom access log file at /var/log/nginx/access.log on each web server worker using a specific log format as described below:_
> - _Access date and time in standard log format._
> - _Name of the worker serving the request (e.g., Bayam, Brokoli, or Buncis)._
> - _Client IP address accessing the website._
> - _HTTP method and URI accessed by the client._
> - _HTTP response status provided by the server.__
> - _Number of bytes sent in the response.
> - _Time taken by the server to handle the request._
> <br> </br>
> _Example of the appropriate log format:
[01/Oct/2024:11:30:45 +0000] Jarkom Node Bayam Access from 192.168.1.15 using method "GET /resep/bayam HTTP/1.1" returned status 200 with 2567 bytes sent in 0.038 seconds_


**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/84a04d09-d493-4542-8c27-a39dfa24d0fd)
  ![image](https://github.com/user-attachments/assets/a6fe17e5-0676-4e0e-8ebb-1ab6c465972e)
  ![image](https://github.com/user-attachments/assets/c9e68a11-30c7-4d5c-ad1a-15ed1f950ea5)

- Explanation

  Pada setiap server, edit /etc/nginx/nginx.conf dengan menambahkan potongan kode berikut dalam blok http
  ```
  map $server_name $short_server_name {
      (server).A13.com "(Server)";
  }
  
  log_format custom_access_log '[$time_local] Jarkom Node $short_server_name Access from $remote_addr '
                                   'using method "$request" returned status $status '
                                   'with $body_bytes_sent bytes sent in $request_time seconds';
  ```
  Juga, tambahkan custom_access_log pada #Logging Settings di sebelah …/access_log sehingga menjadi seperti berikut
  ```
  access_log /var/log/nginx/access.log custom_access_log;
  ```
  Kemudian, tambahkan custom_access_log di sebelah …/access_log pada/etc/nginx/sites-available/(server).A13.com sehingga menjadi seperti berikut
  ```
  access_log /var/log/nginx/access.log custom_access_log;
  ```
  Restart nginx
  ```
  service nginx reload
  service nginx restart
  nginx -t
  ```
  Buka lynx
  ```
  lynx (server).A13.com
  ```
  Terakhir, tampilkan access.log nya
  ```
  cat /var/log/nginx/access.log
  ```
<br>

## Soal 12

> Informasi vitamin pada sayur brokoli akan ditampilkan pada subdomain vitamin.brokoli.yyy.com di node brokoli, buatlah DocumentRoot yang disimpan pada /var/www/vitamin.brokoli.yyy. Konfigurasikan webserver dengan nama server vitamin.brokoli.yyy.com dan server alias www.vitamin.brokoli.yyy.com. Lakukan konfigurasi Apache Web Server pada Brokoli dengan menggunakan sumber yang tersedia di [sini](https://docs.google.com/uc?export=download&id=1QbGkKXo3jt4c68AdVAkl1hD4LolTUPg2).

> _For information on vitamins in brokoli will be displayed on the vitamin.brokoli.yyy.com subdomain on the brokoli node, create a DocumentRoot stored in /var/www/vitamin.brokoli.yyy. Configure the web server with the server name vitamin.brokoli.yyy.com and server alias www.vitamin.brokoli.yyy.com. Configure the Apache Web Server on Brokoli using [this resource](https://docs.google.com/uc?export=download&id=1QbGkKXo3jt4c68AdVAkl1hD4LolTUPg2)._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/eb9db792-80d4-41b6-9197-92fd4eb0d9c5)

- Explanation

  #### Pada BrokoliWebServer
  Lakukan instalasi
  ```
  apt-get update
  apt-get install apache2
  apt-get install lynx
  apt install unzip -y
  
  service apache2 start
  ```
  Buat folder untuk menyimpan DocumentRoot
  ```
  mkdir /var/www/vitamin.brokoli.A13
  ````
  Download file dan unzip
  ```
  URL_ZIP="https://docs.google.com/uc?export=download&id=1QbGkKXo3jt4c68AdVAkl1hD4LolTUPg2"
  wget --no-check-certificate -O vitamin-brokoli.zip $URL_ZIP
  unzip vitamin-brokoli.zip -d vitamin-brokoli
  cp -r vitamin-brokoli/vitamin.brokoli.yyy.com/* /var/www/vitamin.brokoli.A13
  rm -rf vitamin-brokoli.zip vitamin-brokoli
  ```
  Copy /etc/apache2/sites-available/000-default.conf kemudian edit isinya seperti berikut
  ```
  cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/vitamin.brokoli.A13.com.conf

  <VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName vitamin.brokoli.A13.com
    ServerAlias www.vitamin.brokoli.A13.com
    DocumentRoot /var/www/vitamin.brokoli.A13

    <Directory /var/www/vitamin.brokoli.A13>
  		Options Indexes FollowSymLinks
  		AllowOverride All
      Require all granted
  	</Directory>

    ErrorLog ${APACHE_LOG_DIR}/vitamin.brokoli.A13.com-error.log
    CustomLog ${APACHE_LOG_DIR}/vitamin.brokoli.A13.com-access.log combined
  </VirtualHost>
  ```
  Edit /etc/hosts
  ```
  127.0.0.1  vitamin.brokoli.A13.com www.vitamin.brokoli.A13.com
  127.0.0.1  localhost
  ::1        localhost ip6-localhost ip6-loopback
  fe00::0    ip6-localnet
  ff00::0    ip6-mcastprefix
  ff02::1    ip6-allnodes
  1102::2    ip6-allrouters
  10.14.2.4  vitamin.brokoli.A13.com www.vitamin.brokoli.A13.com
  ```
  Aktifkan konfigurasi situs
  ```
  a2ensite vitamin.brokoli.A13.com.conf
  ```
  Restart apache2
  ```
  service apache2 restart
  ```
  Jalankan lynx vitamin.brokoli.A13.com untuk melakukan pengecekan
<br>

## Soal 13

> Pada subdomain vitamin.brokoli.yyy.com, terdapat subfolder /nutrisi yang menyediakan informasi tentang berbagai vitamin dalam brokoli, seperti Vitamin A, C, dan K. Aktifkan directory listing untuk folder /nutrisi, dan buatlah rewrite rule di Apache untuk memperbaiki URL agar halaman seperti www.vitamin.brokoli.yyy.com/nutrisi/vitamin_a.php dapat diakses hanya dengan www.vitamin.brokoli.yyy.com/nutrisi/vitamin_a. Pastikan setiap halaman vitamin dapat diakses langsung melalui url yang telah disederhanakan.

> _On the vitamin.brokoli.yyy.com subdomain, there is a /nutrisi subfolder that provides information about various vitamins in brokoli, such as Vitamin A, C, and K. Activate directory listing for the /nutrisi folder, and create a rewrite rule in Apache to fix the URL so that pages like www.vitamin.brokoli.yyy.com/nutrisi/vitamin_a.php can be accessed only with www.vitamin.brokoli.yyy.com/nutrisi/vitamin_a. Make sure each vitamin page can be accessed directly through the simplified url._

**Answer:**

- Screenshot

  ![Screenshot 2024-10-04 143351](https://github.com/user-attachments/assets/9a6eddd9-704a-4847-8f9f-c889b04fe4ff)

- Explanation
  
  #### Pada BrokoliWebServer
  Lakukan instalasi
  ```
  apt-get update
  apt-get install apache2
  apt-get install php
  apt-get install lynx
  
  service apache2 start
  ```
  Edit konfigurasi /etc/apache2/sites-available/vitamin.brokoli.A13.com.conf
  ```
  <VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName vitamin.brokoli.A13.com
    ServerAlias www.vitamin.brokoli.A13.com
    DocumentRoot /var/www/vitamin.brokoli.A13

    <Directory /var/www/vitamin.brokoli.A13>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>

    <Directory /var/www/vitamin.brokoli.A13/nutrisi>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/vitamin.brokoli.A13.com-error.log
    CustomLog ${APACHE_LOG_DIR}/vitamin.brokoli.A13.com-access.log combined
  </VirtualHost>
  ```
  Buat .htaccess di dalam folder /var/www/vitamin.brokoli.A13/nutrisi yang berisi
  ```
  RewriteEngine On
  RewriteBase /nutrisi/
  
  RewriteRule ^vitamin_a$ vitamin_a.php [L]
  RewriteRule ^vitamin_c$ vitamin_c.php [L]
  RewriteRule ^vitamin_k$ vitamin_k.php [L]
  ```
  Aktifkan modul mod_rewrite dan jalankan apache2
  ```
  a2enmod rewrite
  service apache2 restart
  ```
  Jalankan lynx vitamin.brokoli.A13.com/nutrisi/vitamin_a untuk melakukan pengecekan
<br>

## Soal 14

> Tambahkan alias untuk folder /public/images/ pada subdomain www.vitamin.brokoli.yyy.com agar folder tersebut dapat diakses langsung melalui url www.vitamin.brokoli.yyy.com/img.

> _Add an alias for the /public/images/ folder on the www.vitamin.brokoli.yyy.com subdomain so that the folder can be accessed directly through the url www.vitamin.brokoli.yyy.com/img._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/371c9114-c69a-4d8a-885a-ae4cbdee7bce)

- Explanation

  #### Pada BrokoliWebServer
  Tambahkan konfigurasi untuk alias di /etc/apache2/sites-available/vitamin.brokoli.A13.com.conf
  ```
    Alias /img /var/www/vitamin.brokoli.A13/public/images
    <Directory /var/www/vitamin.brokoli.A13/public/images>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
  ```
  Aktifkan konfigurasi situs
  ```
  a2ensite vitamin.brokoli.A13.com.conf
  ```
  Restart apache2
  ```
  service apache2 restart
  ```
  Jalankan lynx vitamin.brokoli.A13.com/img untuk melakukan pengecekan
<br>

## Soal 15

> Karena terdapat resep rahasia di file /secret/recipe_secret.txt pada subdomain www.vitamin.brokoli.yyy.com, konfigurasikan folder /secret agar tidak dapat diakses oleh pengguna (dengan menampilkan 403 Forbidden).

> _Because there is a secret recipe in the /secret/recipe_secret.txt file on the www.vitamin.brokoli.yyy.com subdomain, configure the /secret folder so that it cannot be accessed by users (by displaying 403 Forbidden)._

**Answer:**

- Screenshot
  
  ![300664fc-925b-4a1c-8d01-04f356728979](https://github.com/user-attachments/assets/eb12b285-7915-4dfc-8349-025d0bd3c4cb)
  ![33209113-d6ea-4690-b802-baec44afb49f](https://github.com/user-attachments/assets/1864a8eb-b343-44f0-add1-a422ed161f46)

- Explanation
  
  #### Pada BrokoliWebServer
  Tambahkan konfigurasi untuk secret di /etc/apache2/sites-available/vitamin.brokoli.A13.com.conf
  ```
    <Directory /var/www/vitamin.brokoli.A13/secret>
        Require all denied
    </Directory>
  ```
  Aktifkan konfigurasi situs
  ```
  a2ensite vitamin.brokoli.A13.com.conf
  ```
  Restart apache2
  ```
  service apache2 restart
  ```
  Jalankan lynx vitamin.brokoli.A13.com/secret untuk melakukan pengecekan
  
<br>

## Soal 16

> Karena dinilai terlalu panjang coba ubah konfigurasi www.vitamin.brokoli.yyy.com/public/js menjadi www.vitamin.brokoli.yyy.com/js

> _Since it is considered too long, change the configuration from www.vitamin.brokoli.yyy.com/public/js to www.vitamin.brokoli.yyy.com/js._

**Answer:**

- Screenshot

  ![ca352b6d-b62f-4d2e-bb0c-d25769a70c04](https://github.com/user-attachments/assets/b7ee3781-8a97-4dc1-8b6f-b0f5327fbd09)

- Explanation

  #### Pada BrokoliWebServer
  Tambahkan konfigurasi untuk alias di /etc/apache2/sites-available/vitamin.brokoli.A13.com.conf
  ```
    Alias /js /var/www/vitamin.brokoli.A13/public/js
    <Directory /var/www/vitamin.brokoli.A13/public/js>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
  ```
  Aktifkan konfigurasi situs
  ```
  a2ensite vitamin.brokoli.A13.com.conf
  ```
  Restart apache2
  ```
  service apache2 restart
  ```
  Jalankan lynx vitamin.brokoli.A13.com/js untuk melakukan pengecekan
<br>

## Soal 17

> Supaya Web kita aman terkendali maka ubah konfigurasi www.k1.vitamin.brokoli.yyy.com menjadi hanya bisa di akses oleh port 9696 dan 8888

> _To keep our web secure, configure www.k1.vitamin.brokoli.yyy.com to only be accessible through ports 9696 and 8888._

**Answer:**

- Screenshot

  ![d11045ce-dc2d-4d82-bbbf-9a85474626e4](https://github.com/user-attachments/assets/71ef8380-fbac-4c76-bbf3-cb13a551aa75)

- Explanation

  #### Pada BrokoliWebServer
  Lakukan instalasi
  ```
  apt-get update
  apt-get install apache2
  apt-get install lynx
  apt-get install php
  apt-get install ufw
  apt install unzip -y
  
  service apache2 start
  ```
  Buat folder untuk menyimpan DocumentRoot
  ```
  mkdir /var/www/k1.vitamin.brokoli.A13
  ````
  Download file dan unzip
  ```
  URL_ZIP="https://docs.google.com/uc?export=download&id=1SRnelY4XrtmhJg_Ly1nUJo1Jf91SnmtB"
  wget --no-check-certificate -O k1.vitamin-brokoli.zip $URL_ZIP
  unzip k1.vitamin-brokoli.zip -d k1.vitamin-brokoli
  cp -r k1.vitamin-brokoli/k1.vitamin.brokoli.yyy.com/* /var/www/k1.vitamin.brokoli.A13
  rm -rf k1.vitamin-brokoli.zip k1.vitamin-brokoli
  ```
  Copy /etc/apache2/sites-available/000-default.conf kemudian edit isinya seperti berikut
  ```
  cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/k1.vitamin.brokoli.A13.com.conf

  <VirtualHost *:9696>
      ServerAdmin webmaster@localhost
      ServerName www.k1.vitamin.brokoli.A13.com
      DocumentRoot /var/www/k1.vitamin.brokoli.A13
  
      <Directory /var/www/k1.vitamin.brokoli.A13>
          Options Indexes FollowSymLinks
          AllowOverride None
          Require all granted
      </Directory>
  
      ErrorLog ${APACHE_LOG_DIR}/error-9696.log
      CustomLog ${APACHE_LOG_DIR}/access-9696.log combined
  </VirtualHost>
  
  <VirtualHost *:8888>
      ServerAdmin webmaster@localhost
      ServerName www.k1.vitamin.brokoli.A13.com
      DocumentRoot /var/www/k1.vitamin.brokoli.A13
  
      <Directory /var/www/k1.vitamin.brokoli.A13>
          Options Indexes FollowSymLinks
          AllowOverride None
          Require all granted
      </Directory>
  
      ErrorLog ${APACHE_LOG_DIR}/error-8888.log
      CustomLog ${APACHE_LOG_DIR}/access-8888.log combined
  </VirtualHost>
  ```
  Edit /etc/hosts
  ```
  127.0.0.1        k1.vitamin.brokoli.A13.com www.k1.vitamin.brokoli.A13.com
  127.0.0.1        vitamin.brokoli.A13.com www.vitamin.brokoli.A13.com
  127.0.0.1        localhost
  ::1              localhost ip6-localhost ip6-loopback
  fe00::0          ip6-localnet
  ff00::0          ip6-mcastprefix
  ff02::1          ip6-allnodes
  ff02::2          ip6-allrouters
  10.14.2.4        vitamin.brokoli.A13.com www.vitamin.brokoli.A13.com
  10.14.2.4        k1.vitamin.brokoli.A13.com www.k1.vitamin.brokoli.A13.com
  ```
  Izinkan akses untuk port 8888 dan 9696
  ```
  ufw allow 9696
  ufw allow 8888
  ```
  Tambahkan port yang perlu didengar di /etc/apache2/ports.conf
  ```
  Listen 9696
  Listen 8888
  ```
  Aktifkan konfigurasi situs
  ```
  a2ensite k1.vitamin.brokoli.A13.com.conf
  ```
  Restart apache2
  ```
  service apache2 restart
  ```
  Jalankan lynx www.k1.vitamin.brokoli.A13.com:8888 atau lynx www.k1.vitamin.brokoli.A13.com:9696 untuk melakukan pengecekan
<br>

## Soal 18

> Lanjutkan dari nomor sebelumnya buatlah autentikasi dengan username “Seblak” dan password “sehatyyy” dengan yyy adalah kode kelompok. Letakkan Document Root pada /var/www/k1.vitamin.brokoli.yyy.

> _Continuing from the previous point, create authentication with the username “Seblak” and the password “sehatyyy” where yyy is the group code. Set the Document Root to /var/www/k1.vitamin.brokoli.yyy._

**Answer:**

- Screenshot
  ![b6ed8349-2d94-4644-9b61-65992cfa1a6d](https://github.com/user-attachments/assets/2e7e5649-ad36-416c-b6ca-d67800c932df)
  ![81489625-3dae-4953-a411-cd6bc9a0bf6f](https://github.com/user-attachments/assets/fc84faad-401b-4953-8f35-a27f05711ffa)
  ![83a24c16-831d-4adc-80e8-2aa38191a1b3](https://github.com/user-attachments/assets/8913a9f9-a093-493a-bfda-ef7b10c08aa9)
  ![2f8d9813-0ac5-4cb3-ac64-c90037b1f638](https://github.com/user-attachments/assets/0f93d863-9b15-4264-9e1d-a01a94e4f8a6)

- Explanation

  #### Pada BrokoliWebServer
  Lakukan instalasi
  ```
  apt-get update
  apt-get install apache2
  apt-get install lynx
  apt-get install php
  apt-get install ufw
  apt install unzip -y
  
  service apache2 start
  ```
  Buat basic authentication
  ```
  htpasswd -c -b /etc/apache2/.htpasswd Seblak sehatA13
  ```
  Edit /etc/apache2/sites-available/k1.vitamin.brokoli.A13.com.conf seperti berikut
  ```
  <VirtualHost *:9696>
      ServerAdmin webmaster@localhost
      ServerName www.k1.vitamin.brokoli.A13.com
      ServerAlias www.k1.vitamin.brokoli.A13.com
      DocumentRoot /var/www/k1.vitamin.brokoli.A13
  
      <Directory /var/www/k1.vitamin.brokoli.A13>
          AuthType Basic
          AuthName "Restricted Content"
          AuthUserFile /etc/apache2/.htpasswd
          Require valid-user
      </Directory>
  
      ErrorLog ${APACHE_LOG_DIR}/error-9696.log
      CustomLog ${APACHE_LOG_DIR}/access-9696.log combined
  </VirtualHost>
  <VirtualHost *:8888>
      ServerAdmin webmaster@localhost
      ServerName www.k1.vitamin.brokoli.A13.com
      DocumentRoot /var/www/k1.vitamin.brokoli.A13
  
      <Directory /var/www/k1.vitamin.brokoli.A13>
          AuthType Basic
          AuthName "Restricted Content"
          AuthUserFile /etc/apache2/.htpasswd
          Require valid-user
      </Directory>
  
      ErrorLog ${APACHE_LOG_DIR}/error-8888.log
      CustomLog ${APACHE_LOG_DIR}/access-8888.log combined
  </VirtualHost>
  ```
  Aktifkan konfigurasi situs
  ```
  a2ensite k1.vitamin.brokoli.A13.com.conf
  ```
  Izinkan akses untuk port 8888 dan 9696
  ```
  ufw allow 9696
  ufw allow 8888
  ```
  Aktifkan modul mod_rewrite dan auth_basic lalu restart apache2
  ```
  a2enmod rewrite
  a2enmod auth_basic
  service apache2 restart
  ```
  Jalankan lynx k1.vitamin.brokoli.A13:8888 atau lynx k1.vitamin.brokoli.A13:9696 untuk melakukan pengecekan
<br>

## Soal 19

> Konfigurasikan agar setiap kali IP Brokoli diakses dengan lynx, secara otomatis akan dialihkan ke www.brokoli.yyy.com (alias).

> _Configure it so that every time Brokoli's IP is accessed using lynx, it is automatically redirected to www.brokoli.yyy.com (alias)._

**Answer:**

- Screenshot
  
![Screenshot 2024-10-09 220435](https://github.com/user-attachments/assets/7c12432d-0358-49d7-a33b-23123c56ca0b)

- Explanation

  #### Pada BrokoliWebServer
  Edit /etc/apache2/sites-available/brokoli.A13.com.conf (jika belum ada, berarti buat baru) seperti berikut
  ```
  <VirtualHost *:80>
	ServerName brokoli.A13.com
	ServerAlias www.brokoli.A13.com
  ServerAlias 10.14.2.4

	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/jarkom

	<Directory /var/www/jarkom>
		Options +FollowSymLinks -Multiviews
		AllowOverride All
	</Directory>

	ErrorLog ${APACHE_LOG_DIR}/brokoli.A13.com-error.log
	CustomLog ${APACHE_LOG_DIR}/brokoli.A13.com-access.log combined
  </VirtualHost>
  ```
  Aktifkan konfigurasi situs dan restart apache2
  ```
  a2ensite brokoli.A13.com.conf
  service apache2 restart
  ```
  Jalankan lynx 10.14.2.4 untuk melakukan pengecekan. Bisa coba di client
<br>

## Soal 20

> Karena jumlah pengunjung website www.vitamin.brokoli.yyy.com semakin meningkat dan terdapat banyak gambar random, ubah permintaan gambar yang mengandung substring "vitamin" agar diarahkan ke file vitamin.png.

> _Since the number of visitors to www.vitamin.brokoli.yyy.com is increasing and there are many random images, redirect image requests that contain the substring "vitamin" to the file vitamin.png._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/7444f7ce-4f7b-47db-96bb-9529d2609abd)

  ![Screenshot 2024-10-12 201325](https://github.com/user-attachments/assets/c2ca5b4b-1dd4-45dc-a8dd-764e7e57ec7f)

- Explanation

  ### Pada BrokoliWebServer
  Edit /etc/apache2/sites-available/vitamin.brokoli.A13.com.conf seperti berikut ini:
  ```
  <VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName vitamin.brokoli.A13.com
    ServerAlias www.vitamin.brokoli.A13.com
    DocumentRoot /var/www/vitamin.brokoli.A13

    <Directory /var/www/vitamin.brokoli.A13>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/vitamin.brokoli.A13.com-error.log
    CustomLog ${APACHE_LOG_DIR}/vitamin.brokoli.A13.com-access.log combined
  </VirtualHost> 
  ```
  Edit juga pada /var/www/vitamin.brokoli.A13/.htaccess untuk konfigurasi seperti ini:
  ```
  RewriteEngine On
  RewriteCond %{REQUEST_URI} !^/public/images/vitamin.png
  RewriteCond %{REQUEST_URI} vitamin
  RewriteRule \.(jpg|jpeg|png)$ /public/images/vitamin.png [L]
  ```
  Setelah itu jangan lupa untuk melakukan konfigutasi situs, mengaktifkan modul, dan restart apache2
  ```
  a2ensite vitamin.brokoli.A13.com.conf
  a2enmod rewrite
  service apache2 restart
  ```
  Jika sudah semua, jalankan lynx vitamin.brokoli.A13/vitaminsusu.jpg untuk melakukan testing. Bisa juga mengganti dengan /sususegardanbervitamin.png dan lain-lain.

<br>
  
## Problems

Sejauh ini belum menemukan problem yang signifikan. Mungkin hanya permasalahan IP yang belum ditambahkan pada hosts, kesalahan penulisan, konfigurasi yang tidak sesuai, dan hal-hal lain yang sebenarnya itu bisa di antisipasi. 

## Revisions (if any)

Revisi pada nomor 19 karena ternyata yang diminta adalah www.brokoli.A13.com yang sudah kita build di awal (nomor 9). Tetapi, dari kita ternyata redirect ke vitamin.brokoli.A13.com. Walaupun begitu, revisi sudah di lakukan `lynx 10.14.2.4` mengarah pada `www.brokoli.A13.com`
