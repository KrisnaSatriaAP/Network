```
126.255.255.11 => atl26s43 : router: 	 "pr02.atl32" next_hop_address: "173.194.121.61" (126.255.255.0/24)

Debug Info:
o-AJlLUDjYuRHAxhlJnZ-jDVzo3pA_quNdQ3awXZiFtgSuEZlJSzq9acaatEmgPh92y6BtLK0SJSKDHo
_ao3HmkqxhqrunJVBWxCcZ6LYqY9BhhOV4I570m9IavAODqS-obzTdxyWOYu8CjF1HYgXdL5fk0hT6nq
YUr0yyzCwKZnSBXrtQeleSe0SNJkWKzRaYrZh2_Y_m0hz7IgWtCAWBd4AsnPUSFPxJo-TDZ44uIINGWc
YA-dB5nRGnAa4-8RTnCXPhXu7ujlIjypc_vPXhEevUgr4FqY8GpqKz43vs2SrgcGSzAbL4x3HSorAQuv
xhNtagZPa_n6OMt7nfTBxIb82TG6IbMvWS-1W8qsn5Q8SCWOI-c9chvx0NRJhvuRD8WlwyqPFxNd2JLb
Nsn8mAxui7C_rA7UBERONQJJVJTvZY7TqLtInN9-mV2g6ZRq9NuAM5RfDuyPvvs-7HOUtTb9qMIno_tp
VoxezlVIaCUHp-Is4Rti1CYbDh3OiMTzzc3H6W19286d9UYKIHF0uRQlpnNQ42KZRGh8TbSPRaUXGDmY
u2cB3pd7DhH9EG9RBRy-K0lxMp2aK8kBZ66m1vt_qICyt5jmvePibTIrF-LS1wdlGyNCJ0iJrJqAPdN9
GZ6-awtYxtG93rd2qxBqxL_o9GnX1TIxbm21ON0lhtMzo3BQ9gR9lLtHXdQOAQoOeP8--duFac34HP-r
```

# Google CDN Routing Debug Page

## Apa Itu?

Halaman diagnostik internal Google untuk melihat bagaimana IP publik ISP di-*route* ke server CDN YouTube/Google Video.

**URL:** `http://redirector.googlevideo.com/report_mapping`

---

## Contoh Output

```
126.255.255.11 => atl14s26 : router: "pr02.atl32" next_hop_address: "173.194.121.61" (126.255.255.0/24)
```

---

## Penjelasan Field

| Field | Nilai (Contoh) | Artinya |
|---|---|---|
| `126.255.255.11` | IP publik ISP | IP yang dikenali oleh Google |
| `atl14s26` | Atlanta, USA | Server CDN tujuan yang dipilih Google |
| `router: "pr02.atl32"` | Peering router Google | Router Google yang menangani koneksi ini |
| `next_hop_address: "173.194.121.61"` | IP internal Google | Alamat next hop di sisi jaringan Google |
| `(126.255.255.0/24)` | Subnet ISP | Blok IP ISP yang dikenali Google |

---

## Kode Lokasi CDN Server

| Kode Awalan | Lokasi | Status |
|---|---|---|
| `jkt` | Jakarta, Indonesia | ✅ Optimal |
| `sin` | Singapore | ✅ OK |
| `kul` | Kuala Lumpur, Malaysia | ⚠️ Cukup |
| `atl` | Atlanta, USA | ❌ Jauh — Peering bermasalah |
| `lax` | Los Angeles, USA | ❌ Jauh |

---

## Indikasi Masalah

Jika CDN server mengarah ke **Atlanta (`atl`) atau kota di luar Asia**, artinya:

1. ISP **tidak memiliki peering** dengan Google di IIX (Indonesia Internet Exchange) atau SGIX
2. **BGP routing** ke Google belum optimal
3. ISP belum terdaftar di program **Google GGC (Global Cache)** atau **Google Peering**

Dampaknya: latensi tinggi ke YouTube, kualitas video lebih buruk dari seharusnya.

---


## Debug Info

Blok teks panjang di bawah baris pertama adalah data **base64/encoded** berisi informasi routing internal Google. Tidak bisa dibaca secara manual — hanya digunakan oleh sistem internal Google.
