# Panduan Teknis: MLD (Multicast Listener Discovery)

## 1. Apa itu MLD?
Secara sederhana, **MLD adalah IGMP versi IPv6**.

Jika **IGMP** digunakan oleh perangkat untuk mengelola trafik *multicast* di jaringan **IPv4**, maka **MLD** melakukan tugas yang sama persis di jaringan **IPv6**. Karena IPv6 tidak menggunakan IGMP, ia menggunakan protokol yang disebut MLD, yang merupakan bagian dari **ICMPv6** (Internet Control Message Protocol for IPv6).

## 2. Mengapa MLD Dibutuhkan?
Dalam IPv6, alamat *multicast* dimulai dengan prefix `FF00::/8`. Agar router tahu perangkat mana yang ingin "mendengarkan" (berlangganan) siaran *multicast* (seperti IPTV IPv6), router dan perangkat harus berbicara menggunakan bahasa MLD.

Tanpa MLD, router tidak akan tahu ke arah mana harus mengirim paket *multicast*, sehingga trafik akan terbuang atau tidak sampai ke tujuan.

## 3. Perbandingan Cepat: IGMP vs. MLD
Untuk memudahkan operasional NOC, gunakan tabel perbandingan ini:

| Fitur | IGMP (IPv4) | MLD (IPv6) |
| :--- | :--- | :--- |
| **Protokol Dasar** | IGMP | ICMPv6 |
| **Fungsi Utama** | Manajemen Multicast Group | Manajemen Multicast Group |
| **Pesan "Join"** | Membership Report | MLD Report |
| **Pesan "Leave"** | Leave Group | MLD Done |
| **Pesan "Query"** | Membership Query | MLD Listener Query |

## 4. Komponen MLD pada Perangkat (Sesuai Konfigurasi Router Anda)

Pada router Anda (FastLink FL327D), terdapat dua fitur utama yang terpampang di menu **MLD Settings**:

### A. MLD Snooping (Layer 2)
* **Fungsi:** Bekerja pada *Switch*.
* **Cara Kerja:** Switch "mengintip" pesan MLD yang lewat. Switch hanya akan meneruskan trafik *multicast* IPv6 ke port yang sudah mengirimkan "Report" (perangkat yang meminta siaran).
* **Manfaat:** Mencegah *multicast flooding* pada jaringan IPv6. Tanpa ini, trafik video IPv6 akan membanjiri semua port switch, yang bisa membuat perangkat lain (PC, Printer) mengalami penurunan performa.

### B. MLD Proxy (Layer 3)
* **Fungsi:** Bekerja pada *Router/Gateway*.
* **Cara Kerja:** Router bertindak sebagai perantara. Ia mengumpulkan permintaan "Join" dari perangkat di LAN (IPv6), lalu router sendiri yang mengirim permintaan ke ISP (sisi WAN) untuk mendapatkan siaran tersebut.
* **Manfaat:** Menjembatani *multicast* antara jaringan lokal dan jaringan penyedia layanan (ISP) menggunakan protokol IPv6.

## 5. Kapan Harus Mengaktifkan MLD?
Sebagai teknisi NOC, Anda perlu mengaktifkan MLD jika:
1. **Layanan IPTV menggunakan IPv6:** Jika ISP Anda mengirimkan *stream* TV melalui protokol IPv6, maka MLD Proxy wajib aktif di router pelanggan.
2. **Lingkungan IPv6 Penuh:** Jika jaringan internal Anda sudah mengimplementasikan IPv6 secara penuh (bukan dual-stack), maka MLD adalah standar yang harus dijalankan untuk efisiensi trafik.

## 6. Checklist Operasional untuk NOC

| Gejala | Masalah Umum | Tindakan |
| :--- | :--- | :--- |
| **TV/Video IPv6 macet** | MLD Proxy tidak aktif | Aktifkan `MLD Proxy` di sisi router/ONT. |
| **Network IPv6 Lambat** | MLD Snooping mati | Pastikan `MLD Snooping` aktif di semua Switch Layer 2. |
| **Trafik IPv6 "Banjir"** | Snooping tidak berjalan | Cek apakah *switch* mendukung MLD Snooping (beberapa switch murah tidak support). |
| **Tidak ada siaran** | WAN Connection salah | Pastikan `WAN Connection` di MLD Proxy menunjuk ke interface yang benar (sesuai dengan koneksi IPTV). |

## 7. Kesimpulan
* **MLD adalah IGMP untuk IPv6.**
* **Snooping** = Efisiensi di Switch (Layer 2).
* **Proxy** = Jembatan di Router (Layer 3).

Jika jaringan Anda belum menggunakan multicast berbasis IPv6, fitur ini biasanya bisa dibiarkan mati atau *default* saja. Namun, jika Anda mulai migrasi ke IPv6 untuk layanan *triple-play* (Internet + TV + VoIP), MLD adalah protokol wajib yang harus dipahami dan dikonfigurasi.
