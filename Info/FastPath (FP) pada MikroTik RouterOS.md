# Mengenal FastPath (FP) pada MikroTik RouterOS

Dokumen ini berisi penjelasan mendalam mengenai **FastPath (FP)** pada perangkat MikroTik, cara kerjanya, arti kolom monitoring di Winbox, serta perumpamaan sederhananya.

---

## 💡 Perumpamaan Sederhana untuk Orang Awam
Bayangkan Anda sedang berkendara di jalan raya menuju luar kota:
* **Jalur Reguler (Slow Path):** Setiap mobil wajib berhenti di gerbang tol untuk diperiksa KTP, STNK, dan dicatat petugas satu per satu. Ini sangat aman dan teliti, tetapi jika mobil yang lewat ada ribuan, antrean akan mengular dan membuat jalan macet (CPU router bekerja sangat berat).
* **Jalur Khusus / E-Pass Non-Stop (FastPath):** Mobil-mobil tertentu yang sudah terverifikasi dan memenuhi syarat bisa melewati **jalur khusus tol tanpa henti** dengan kecepatan tinggi karena gerbangnya otomatis terbuka tanpa perlu pengecekan manual yang rumit. Hasilnya? Perjalanan jauh lebih cepat dan jalanan tidak macet.

---

## 1. Apa itu FastPath (FP)?
**FastPath** adalah fitur akselerasi tingkat kernel di **MikroTik RouterOS** yang memungkinkan router untuk meneruskan (*forward*) paket data secara instan. 

Fitur ini memotong sebagian besar tahapan pemrosesan paket standar (seperti pengecekan *firewall* yang kompleks atau pencatatan *connection tracking* reguler yang berulang) untuk paket-paket yang sudah dikenali sesi komunikasinya.

---

## 2. Mengapa FastPath Penting?
1. **Menghemat Beban CPU (*CPU Load*):** Tanpa FastPath, router harus mengevaluasi setiap paket satu per satu melalui antrean CPU. Dengan FastPath, beban CPU bisa turun secara drastis (misalnya dari 90% menjadi di bawah 10% saat melayani trafik besar).
2. **Meningkatkan Throughput:** Memungkinkan router meneruskan jutaan paket per detik (Mpps) mendekati batas kemampuan fisik (*wire-speed*) dari *interface* jaringan perangkat.

---

## 3. Penjelasan Kolom Statistik FastPath di Winbox
Saat Anda memantau *interface* jaringan di Winbox, Anda akan melihat kolom-kolom khusus yang diawali dengan huruf **FP**:

* **FP Tx (Bits per Second):** Besarnya kapasitas bandwidth data yang berhasil dikirimkan keluar (*Transmit*) menggunakan jalur percepatan FastPath.
* **FP Rx (Bits per Second):** Besarnya kapasitas bandwidth data yang diterima masuk (*Receive*) melalui jalur percepatan FastPath.
* **FP Tx Packet (p/s):** Jumlah total paket data per detik yang dikirimkan melalui jalur FastPath.
* **FP Rx Packet (p/s):** Jumlah total paket data per detik yang diterima melalui jalur FastPath.

*Catatan: Jika angka pada kolom FP ini bergerak naik seiring tingginya trafik, itu artinya router Anda bekerja sangat efisien karena sebagian besar trafik ditangani oleh FastPath.*

---

## 4. Kapan FastPath Tidak Bekerja?
FastPath tidak bisa digunakan untuk semua jenis trafik. Trafik akan otomatis turun ke jalur reguler (Slow Path) jika Anda menggunakan fitur-fitur tertentu yang membutuhkan pemeriksaan mendalam, seperti:
* Menggunakan **Simple Queues** atau **Queue Tree** (pada beberapa versi RouterOS tertentu yang belum mendukung *queue acceleration*).
* Aturan *Firewall Nat* atau *Filter Rules* tertentu yang mewajibkan pelacakan koneksi aktif secara ketat.
* Penggunaan protokol VPN tertentu (seperti EoIP atau IPsec murni tanpa hardware offload).

---
*Dokumentasi ini dibuat untuk referensi pengelolaan dan optimasi performa jaringan MikroTik.*
