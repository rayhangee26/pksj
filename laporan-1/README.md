## Tugas 1

### Pendahuluan
Pada saat ini keamanan jaringan merupakan bagian yang penting dalam keberlangsungan sistem komputer. Keamanan jaringan sangat dibutuhkan agar sebuah sistem komputer tahan dari serangan atau peretasan oleh hacker. Kami akan membahas cara penggunaan dari beberapa tool yang biasanya digunakan dalam hacking. Dalam seni hacking atau penetration testing kita mengenal beberapa teknik yang dapat digunakan untuk meretas sebuah sistem yang kita inginkan, salah satunya merupakan teknik Password Cracking. Pada laporan ini kami akan menjelaskan persiapan apa saja yang dibutuhkan, penjelasan mengenai tool yang akan digunakan, dan cara menggunakannya untuk mencoba Password Cracking. Selain menunjukkan cara mempraktekkan teknik password cracking kami juga akan menunjukkan cara menggunakan tool untuk melakukan tindakan balasan dari teknik password cracking.  

### Dasar Teori
#### Password Cracking
Password Cracking merupakan salah satu teknik yang sering digunakan dalam seni hacking. Password Cracking sendiri merupakan proses untuk mendapatkan password rahasia dari data yang telah disimpan maupun ditransmisikan dari sebuah sistem komputer. Pada tugas ini kami menggunakan teknik Brute Force Attack. Brute Force Attack sendiri adalah metode untuk mendapatkan password dengan cara login dan mencoba semua kemungkinan kombinasi kata pada password. Pada Brute Force Attack jika login berhasil, maka password dapat ditemukan. Dan jika password yang digunakan pada suatu sistem cukup kuat dengan kombinasi huruf, angka, dan simbol proses ini akan memakan waktu hingga berjam - jam, hari, minggu, bahkan bulanan. Tools hacking yang menggunakan metode Brute Force biasanya bergantung pada dictionary / kamus yang berisi kata-kata yang mungkin dijadikan password dari sistem yang akan diserang.

penjelasan OS dan semua tools yang digunakan
os : ubuntu server dan ubuntu desktop
tools : openssh, hydra, ncrack, medusa, fail2ban

### Uji Penetrasi
#### Langkah instalasi ubuntu server
  1. Buka VirtualBox, pilih New 
  2. Masukkan konfigurasi virtual machine lalu create
  3. Klik kanan virtual machine yang dibuat, pilih setting
  4. Pilih bagian network, tambahkan adapter 2 dengan opsi host-only adapter
  5. Pilih bagian storage, klik bagian disk image, pilih choose virtual optical disk lalu masukkan lokasi iso ubuntu server, ok
  6. Run virtual machine tadi lalu install ubuntu server seperti biasanya
  7. Karena kita membutuh ip untuk ssh, tambahkan ip address di /etc/network/interfaces
  
#### Langkah instalasi OS untuk penetrasi (Ubuntu Desktop)
  1. Buka VirtualBox, pilih New 
  2. Masukkan konfigurasi virtual machine lalu create
  3. Klik kanan virtual machine yang dibuat, pilih setting
  4. Pilih bagian network, tambahkan adapter 2 dengan opsi host-only adapter
  5. Pilih bagian storage, klik bagian disk image, pilih choose virtual optical disk lalu masukkan lokasi iso ubuntu desktop, ok
  6. Run virtual machine tadi lalu install ubuntu desktop seperti biasanya
  
#### Instalasi SSH Server
  Pada ubuntu server dan ubuntu desktop jalankan command berikut 
  ```bash
  sudo apt-get install openssh-server
  ```
  
#### Langkah uji penetrasi dengan SSH brute force tools
1. Uji dengan password yang dicantumkan dalam file
*   File yang berisi sejumlah list password beserta password asli
*   File yang berisi sejumlah list password tanpa password
2. Uji dengan metode brute force tanpa dictionary

#### Hydra

Untuk menginstall hydra
  ```bash
  sudo apt-get install hydra hydra-gtk
  ```
Atau dapat juga diinstall dengan
  ```bash
  wget https://nmap.org/ncrack/dist/ncrack-0.5.tar.gz
  ./configure
  make
  make install
  ```

Untuk menggunakan hydra melalui command-line
  ```bash
  hydra -l ubuntu -x 6:6:a 192.168.26.102 ssh -V
  ```

#### Ncrack
Untuk menginstall Ncrack jalankan command-line berikut
  ```bash
  wget http://nmap.org/ncrack/dist/ncrack-0.4ALPHA.tar.gz
  sudo apt-get install build-essential checkinstall libssl-dev  libssh-dev
  tar xvfz ncrack-0.4ALPHA.tar.gz
  cd ncrack-0.4ALPHA/ 
  ./configure
  make
  sudo make install
  ```
Untuk menggunakannya jalankan command-line berikut 
  ```bash
  ncrack -p 22 --user ubuntu -P pass.txt 192.168.56.102
  ```
#### Medusa
Medusa sudah terdaftar dalam package list ubuntu, untuk menginstall pada ubuntu
  ```bash
  sudo apt-get install medusa
  ```
Atau dapat juga di install dengan
  ```bash
  wget http://www.foofus.net/jmk/tools/medusa-2.0.tar.gz
  ./configure
  make
  make install
  ```
untuk menjalankan medusa
  ```bash
  medusa -u [username] -P [password list file] -M [metode yang ingin digunakan (ssh, mysql)]
  ```

#### Fail2Ban
  ```bash
  sudo apt-get install fail2ban
  ```
Kemudian atur configurasi di `` /etc/fail2ban/jail.conf ``. Setelah configurasi selesai dilakukan, jalankan command-line berikut untuk menjalankan fail2ban pada server
  ```bash
  sudo service fail2ban start
  ```
Jika ada yang mencoba bruteforce maka akan diban sementara sesuai konfigurasi. Hasilnya dapat terlihat dengan menjalankan command 
```bash
sudo iptables -S
```
### Kesimpulan

#### Countermeasure
Dalam penetrasi sistem dengan teknik password cracking salah satu countermeasure yang dapat kita gunakan adalah dengan membatasi percobaan login dari sumber yang melakukan percobaan login berkali2 dengan cara brute force. Untuk melakukan tindakan balasan brute force kami menggunakan tool fail2ban. Pada dasarnya fail2ban bekerja dengan mengamati log dari server yang sedang aktif. bila ada host yang dicurigai melakukan percobaan akses kepada server dalam percobaan tertentu maka fail2ban akan mengedit aturan firewall iptables.
