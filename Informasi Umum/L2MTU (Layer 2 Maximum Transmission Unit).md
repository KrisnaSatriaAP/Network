# Mengenal L2MTU (Layer 2 Maximum Transmission Unit)

Dokumen ini menjelaskan secara mendalam mengenai **L2MTU (Layer 2 Maximum Transmission Unit)** pada jaringan komputer.

---

## 💡 Perumpamaan Sederhana untuk Orang Awam
Kotak kardus utama (MTU) tadi tidak dikirim begitu saja ke jalan raya, melainkan harus dimuat ke dalam sebuah **truk box pengangkut**. 
* Di dalam truk tersebut, selain memuat kotak kardus utama, ada juga ruang yang terpakai untuk menaruh kayu palet, nomor resi pengiriman, stiker khusus, atau segel pengaman tambahan (ini ibarat *header* Layer 2, seperti tambahan tanda *VLAN* atau *PPPoE*). Ruang total truk beserta seluruh atributnya inilah yang disebut **L2MTU**.
* **Masalah yang sering terjadi:** Jika ukuran truk (**L2MTU**) disamakan persis dengan ukuran kotak kardus (1500 unit), maka begitu stiker resi dan segel tambahan dipasang, muatan truk akan kelebihan batas toleransi pintu. Akibatnya? Pihak ekspedisi menolak truk tersebut dan **barangnya langsung dibuang begitu saja (*packet loss*)**.

---

## 1. Definisi Dasar
**L2MTU** adalah ukuran maksimum frame data yang dapat ditransmisikan pada lapisan tautan data (**Layer 2 / Data Link Layer** dalam model OSI), seperti pada media Ethernet, Wi-Fi, atau PPP. 

L2MTU mencakup keseluruhan data Layer 3 (MTU) ditambah dengan informasi tambahan Layer 2, yaitu **Ethernet Header** dan **Trailer (FCS)**, serta tambahan tag khusus seperti **VLAN Tagging (802.1Q)**, **MPLS**, atau **PPPoE Header**.

---

## 2. Struktur Frame Ethernet Standar
Sebuah frame Ethernet standar tanpa tambahan tag terdiri dari:
* **Destination MAC Address:** 6 bytes
* **Source MAC Address:** 6 bytes
* **EtherType / Length:** 2 bytes
* **Payload (Data / Layer 3 Packet):** Biasanya 46 hingga 1500 bytes (atau sesuai MTU)
* **FCS (Frame Check Sequence / CRC):** 4 bytes
* *Total Standar:* 6 + 6 + 2 + 1500 + 4 = **1518 bytes**.

---

## 3. Mengapa L2MTU Harus Lebih Besar dari MTU?
Jika Anda menggunakan tambahan seperti **VLAN Tag** (menambah 4 bytes per tag) atau enkapsulasi **PPPoE** (menambah 8 bytes), ukuran total frame fisik akan bertambah. 

* Jika L2MTU pada interface perangkat (Router atau Switch) diset pas di 1500 bytes, maka frame yang membawa VLAN atau PPPoE akan dianggap sebagai *oversized frame* atau *Giant* dan **langsung dibuang (dropped)** oleh perangkat keras.
* Perangkat modern (seperti MikroTik atau managed switch) umumnya mendukung L2MTU yang jauh lebih tinggi (misal: 1598, 2028, hingga 9000 bytes untuk *Jumbo Frames*).

---

## 4. Praktik Terbaik Konfigurasi L2MTU
1. **Selalu Pastikan L2MTU Cukup:** Pastikan nilai L2MTU pada interface fisik lebih besar daripada MTU dan mampu menampung seluruh *overhead* tambahan (VLAN, MPLS, QinQ).
2. **Cek Kapasitas Hardware:** Pada perangkat seperti MikroTik, Anda bisa melihat batas kemampuan *chipset switch* pada kolom `max-l2mtu` di menu interface untuk memastikan perangkat mendukung ukuran frame yang diinginkan.
