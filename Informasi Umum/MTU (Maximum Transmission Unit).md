# Mengenal MTU (Maximum Transmission Unit)

Dokumen ini menjelaskan secara mendalam mengenai **MTU (Maximum Transmission Unit)** pada jaringan komputer.

---

## 💡 Perumpamaan Sederhana untuk Orang Awam
Bayangkan Anda ingin mengirimkan barang belanjaan menggunakan **kotak kardus utama**. 
* Aturan ekspedisi menetapkan bahwa satu kotak kardus standar maksimal hanya boleh berukuran **1500 unit** (ini ibarat **MTU 1500 bytes**). 
* Jika barang atau data yang ingin Anda kirimkan lebih besar dari ukuran tersebut, kardusnya terpaksa harus dibongkar dan dipecah menjadi dua bagian (yang dalam jaringan disebut **fragmentasi**) agar bisa dikirim secara bertahap.

---

## 1. Definisi Dasar
**MTU** adalah ukuran terbesar dari paket data (dalam satuan *bytes*) yang dapat dikirimkan melalui sebuah protokol pada lapisan jaringan (**Layer 3 / Network Layer** dalam model OSI, seperti IPv4 atau IPv6). 

* **Standar Ethernet Default:** Sebagian besar jaringan Ethernet menggunakan nilai MTU default sebesar **1500 bytes**.

---

## 2. Mengapa MTU Penting?
1. **Efisiensi & Latensi:** Paket yang terlalu besar berisiko mengalami fragmentasi di tengah jalan, yang meningkatkan beban CPU perangkat router dan menambah latensi. Sebaliknya, paket yang terlalu kecil membuat persentase *overhead* (ukuran header dibanding isi data) menjadi terlalu besar sehingga mengurangi efisiensi bandwidth.
2. **Kesesuaian Jalur (Path):** Data melewati berbagai *hop* (router) di internet yang mungkin memiliki batas MTU berbeda-beda.

---

## 3. Fragmentasi dan Path MTU Discovery (PMTUD)
* **Fragmentasi:** Ketika paket melebihi MTU pada suatu interface, router akan memecah paket tersebut menjadi fragmen-fragmen kecil. Di tujuan akhir, fragmen ini akan disatukan kembali.
* **Path MTU Discovery (PMTUD):** Mekanisme yang digunakan untuk menentukan ukuran MTU terkecil di sepanjang jalur antara pengirim dan penerima menggunakan flag `DF (Don't Fragment)` pada header IP. Jika router menemukan paket yang lebih besar dari MTU-nya dan bit DF aktif, router akan membuang paket tersebut dan mengirimkan pesan ICMP *Destination Unreachable: Fragmentation Needed*.

---

## 4. Masalah Umum & Solusi (Kasus PPPoE / VPN)
Pada koneksi **PPPoE** (umum digunakan oleh ISP rumahan), terdapat tambahan *header* PPPoE sebesar 8 bytes. 
* Jika MTU diatur tetap 1500 bytes, ukuran total paket yang keluar akan melebihi batas standar, menyebabkan fragmentasi atau *packet loss*.
* **Solusi:** Mengurangi nilai MTU menjadi **1492 bytes** pada interface PPPoE, atau mengaktifkan **TCP MSS Clamping** pada router (`change-tcp-mss`) agar ukuran segmen TCP disesuaikan secara otomatis.
