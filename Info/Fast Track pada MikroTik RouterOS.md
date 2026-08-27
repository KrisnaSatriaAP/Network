# Mengenal Fast Track pada MikroTik RouterOS

Dokumen ini berisi penjelasan mendalam mengenai **Fast Track** pada perangkat MikroTik, perbedaannya dengan FastPath, serta perumpamaan sederhananya.

---

## 💡 Perumpamaan Sederhana untuk Orang Awam
Bayangkan Anda sedang antre masuk ke sebuah area khusus atau wahana taman bermain:
* **Pemeriksaan Reguler:** Setiap kali Anda berjalan melewati pos penjagaan, petugas harus menghentikan Anda, membuka tas, mengecek tiket satu per satu, dan mencatat nama Anda secara detail. Ini sangat memakan waktu.
* **Jalur Fast Track (VIP Pass):** Pada pemeriksaan **pertama kali**, petugas memeriksa tiket Anda secara teliti dan memberikan gelang VIP khusus (*Established Connection*). Untuk kunjungan-kunjungan berikutnya di hari yang sama, penjaga cukup melihat gelang VIP Anda dan langsung mempersilakan Anda masuk tanpa pemeriksaan ulang yang panjang. Hasilnya? Antrean langsung cair dan sangat cepat.

---

## 1. Apa itu Fast Track?
**Fast Track** adalah fitur di **MikroTik RouterOS** (khususnya pada *IP Firewall*) yang dirancang untuk mempercepat pemrosesan paket data pada koneksi yang sudah mapan (*established*).

Ketika sebuah koneksi baru masuk, paket pertama akan diperiksa secara normal melalui aturan *Firewall* dan *NAT*. Namun, jika koneksi tersebut disetujui, MikroTik akan menandai sesi tersebut agar paket-paket selanjutnya di dalam sesi yang sama bisa **melewati (bypassing)** sebagian besar aturan *Firewall Filter*, *NAT*, dan *Mangle* secara otomatis.

---

## 2. Perbedaan Utama: Fast Track vs FastPath (FP)
Meskipun keduanya bertujuan untuk mempercepat kerja router dan menghemat CPU, ada perbedaan lapisan tempat mereka bekerja:
* **FastPath (FP):** Bekerja di tingkat paling bawah sistem (Driver/Kernel Interface), membuat router meneruskan paket langsung dari satu *interface* fisik ke *interface* fisik lainnya tanpa menyentuh CPU sama sekali.
* **Fast Track:** Bekerja di tingkat **Firewall / Connection Tracking**. Paket tetap masuk ke sistem routing, tetapi melewati pemeriksaan aturan *firewall* yang berat sehingga CPU tidak perlu mengevaluasi ribuan aturan berulang kali untuk koneksi yang sama.

---

## 3. Mengapa Fast Track Penting?
1. **Menghemat Beban CPU Secara Signifikan:** Sangat berguna bagi router rumahan atau kantor kecil-menengah (seperti hEX atau RB750Gr3) yang melayani ratusan pengguna dengan trafik unduh/unggah yang tinggi.
2. **Meningkatkan Throughput Internet:** Mencegah lonjakan *CPU load* hingga 100% saat trafik sedang padat (misalnya saat banyak pengguna melakukan *streaming* atau unduh file besar).

---

## 4. Contoh Konfigurasi Dasar di MikroTik
Secara default, konfigurasi standar MikroTik (Default Configuration) untuk jaringan rumahan sudah menyertakan aturan Fast Track pada menu Firewall Filter:

```text
/ip firewall filter
add chain=forward action=fasttrack-connection connection-state=established,related comment="defconf: fasttrack"
```

---

## 5. Kapan Fast Track Harus Dimatikan?
Meskipun sangat menghemat CPU, Fast Track memiliki beberapa batasan. Anda **harus mematikan atau menyesuaikan** aturan Fast Track jika menggunakan fitur-fitur tertentu yang memerlukan pemeriksaan paket secara terus-menerus, seperti:
* **Simple Queues / Queue Tree:** Beberapa jenis manajemen bandwidth (*queue*) tidak akan berjalan akurat jika paket sudah keburu dilewatkan oleh Fast Track (kecuali menggunakan *Queue Tree* dengan penandaan mark-packet yang sesuai atau bantuan *queue acceleration*).
* **IPsec VPN:** Terkadang trafik VPN tertentu mengalami kendala jika jalur koneksinya di-fast-track secara sembarangan.
* **Child Control / Web Proxy:** Filter konten atau proxy transparan tidak bisa memindai paket yang melewati jalur Fast Track.

---
*Dokumentasi ini dibuat untuk referensi pengelolaan dan optimasi jaringan MikroTik.*
