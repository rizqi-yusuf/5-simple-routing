# 🌐 Cisco Packet Tracer: Implementasi 5 Protokol Routing (Static, EIGRP, RIPv2, OSPF, & BGP)

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco_Packet_Tracer-8.2+-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)
![Networking](https://img.shields.io/badge/Routing-Static_|_EIGRP_|_RIPv2_|_OSPF_|_BGP-success?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_&_Verified-brightgreen?style=for-the-badge)

Repositori ini berisi proyek simulasi jaringan komputer menggunakan **Cisco Packet Tracer** (`ROUTING.pkt`) yang mendemonstrasikan 5 metode routing berbeda dengan **topologi dan skema IP Address yang identik**. Proyek ini dirancang untuk mempermudah perbandingan cara kerja, konfigurasi CLI, serta analisis tabel routing antara protokol *Static*, *Interior Gateway Protocol* (IGP), dan *Exterior Gateway Protocol* (EGP).

---

## 📋 Daftar Isi
1. [Struktur Repositori](#-struktur-repositori)
2. [Cara Menggunakan File (.pkt)](#-cara-menggunakan-file-pkt)
3. [Topologi & Skema Pengalamatan IP](#-topologi--skema-pengalamatan-ip)
4. [Perbandingan Karakteristik 5 Protokol](#-perbandingan-karakteristik-5-protokol)
5. [Konfigurasi Dasar (IP Address Interface & PC)](#-konfigurasi-dasar-ip-address-interface--pc)
6. [Konfigurasi & Penjelasan Detail 5 Routing](#-konfigurasi--penjelasan-detail-5-routing)
   - [1. Static Routing](#1-static-routing)
   - [2. EIGRP](#2-eigrp-enhanced-interior-gateway-routing-protocol)
   - [3. RIPv2](#3-ripv2-routing-information-protocol-version-2)
   - [4. OSPF](#4-ospf-open-shortest-path-first)
   - [5. BGP](#5-bgp-border-gateway-protocol)
7. [Pengujian Koneksi (Ping & Traceroute)](#-pengujian-koneksi-ping--traceroute)
8. [Panduan Troubleshooting](#-panduan-troubleshooting)

---

## 📁 Struktur Repositori

Berikut adalah susunan file di dalam repositori ini:

```text
📦 cisco-5-routing-protocols
 ┣ 📂 assets/
 ┃ ┣ 🖼️ topology.png            # Screenshot topologi jaringan keseluruhan
 ┃ ┗ 🖼️ ping-verification.png   # Bukti pengujian ping antar PC
 ┣ 📂 configs/
 ┃ ┣ 📄 1-static-config.txt     # Script CLI lengkap untuk Static Routing
 ┃ ┣ 📄 2-eigrp-config.txt      # Script CLI lengkap untuk EIGRP
 ┃ ┣ 📄 3-ripv2-config.txt      # Script CLI lengkap untuk RIPv2
 ┃ ┣ 📄 4-ospf-config.txt       # Script CLI lengkap untuk OSPF
 ┃ ┗ 📄 5-bgp-config.txt        # Script CLI lengkap untuk eBGP
 ┣ 📄 ROUTING.pkt               # File utama simulasi Cisco Packet Tracer
 ┗ 📄 README.md                 # Dokumentasi proyek
```

---

## 🚀 Cara Menggunakan File `.pkt`

Ikuti langkah-langkah berikut untuk menjalankan simulasi di perangkat kamu:

### 1. Persyaratan Sistem (Prerequisites)
* Pastikan kamu sudah menginstal **Cisco Packet Tracer** (Disarankan **Versi 8.0 ke atas** agar file `.pkt` dapat dibuka tanpa kendala kompatibilitas).

### 2. Langkah Membuka Proyek
1. **Clone** repositori ini atau unduh langsung file `ROUTING.pkt`:
   ```bash
   git clone [https://github.com/username-kamu/nama-repo-kamu.git](https://github.com/username-kamu/nama-repo-kamu.git)
   ```
2. Buka aplikasi **Cisco Packet Tracer** dan *login* menggunakan akun Cisco NetAcad / Skills for All kamu.
3. Klik menu **File > Open**, lalu pilih file **`ROUTING.pkt`**.
4. Tunggu sekitar **30–50 detik** (atau tekan tombol **`Fast Forward Time`** `Alt + D` di pojok kiri bawah beberapa kali) agar proses *Spanning Tree Protocol* (STP) pada Switch dan konvergensi routing berubah dari indikator **Oranye** menjadi **Hijau**.

### 3. Cara Menguji Jaringan di Dalam Packet Tracer
* **Metode 1 (Simple PDU / Amplop):** Tekan tombol **`P`** pada keyboard, klik pada **PC Kiri** (`192.168.10.2`), lalu klik pada **PC Kanan** (`192.168.20.2`). Lihat status di pojok kanan bawah layar, pastikan tertulis **`Successful`**.
* **Metode 2 (Command Prompt):** Klik salah satu PC > pilih tab **Desktop** > buka **Command Prompt** > ketik `ping 192.168.20.2`.

---

## 🖧 Topologi & Skema Pengalamatan IP


![Gambar Topologi](<img width="611" height="394" alt="{42D61558-D337-4CD6-AA08-7988571F4DEE}" src="https://github.com/user-attachments/assets/0799e3bc-58b4-4c02-be9d-72cdb14cad82" />
)
*(Catatan: Ganti path gambar di atas sesuai dengan letak screenshot topologi kamu di repositori)*

Setiap blok topologi terdiri dari:
* **3x Router Cisco 2911** (Router Tengah/Atas, Router Kiri, dan Router Kanan)
* **2x Switch Cisco 2960-24TT** (Switch Kiri dan Switch Kanan)
* **4x PC-PT** (2 PC pada LAN Kiri, 2 PC pada LAN Kanan)

> 💡 **Catatan Penting Subnetting:**
> Pada label visual di topologi tertulis `/32` untuk jaringan antar-router (`200.10.10.0` dan `200.20.20.0`). Namun pada implementasi CLI Cisco IOS sebenarnya, digunakan **`/30` (`255.255.255.252`)** karena koneksi *point-to-point* antar dua router membutuhkan 2 alamat IP *host* yang valid, sedangkan untuk jaringan LAN ke PC menggunakan **`/24` (`255.255.255.0`)**.

### Tabel Alamat IP (Addressing Table) — *Berlaku Sama untuk Ke-5 Topologi*

| Perangkat | Interface | IP Address | Subnet Mask | Wildcard Mask | Default Gateway | Keterangan |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Router Tengah** | `Gig0/0` | `200.10.10.1` | `255.255.255.252` | `0.0.0.3` | - | Link ke Router Kiri (`Gig0/0`) |
| | `Gig0/1` | `200.20.20.1` | `255.255.255.252` | `0.0.0.3` | - | Link ke Router Kanan (`Gig0/0`) |
| **Router Kiri** | `Gig0/0` | `200.10.10.2` | `255.255.255.252` | `0.0.0.3` | - | Link ke Router Tengah (`Gig0/0`) |
| | `Gig0/1` | `192.168.10.1` | `255.255.255.0` | `0.0.0.255` | - | Gateway untuk LAN Kiri |
| **Router Kanan** | `Gig0/0` | `200.20.20.2` | `255.255.255.252` | `0.0.0.3` | - | Link ke Router Tengah (`Gig0/1`) |
| | `Gig0/1` | `192.168.20.1` | `255.255.255.0` | `0.0.0.255` | - | Gateway untuk LAN Kanan |
| **PC Kiri 1** | `Fa0` | `192.168.10.2` | `255.255.255.0` | - | `192.168.10.1` | Terhubung ke Switch Kiri (`Fa0/1`) |
| **PC Kiri 2** | `Fa0` | `192.168.10.3` | `255.255.255.0` | - | `192.168.10.1` | Terhubung ke Switch Kiri (`Fa0/2`) |
| **PC Kanan 1** | `Fa0` | `192.168.20.2` | `255.255.255.0` | - | `192.168.20.1` | Terhubung ke Switch Kanan (`Fa0/1`) |
| **PC Kanan 2** | `Fa0` | `192.168.20.3` | `255.255.255.0` | - | `192.168.20.1` | Terhubung ke Switch Kanan (`Fa0/2`) |

---

## 📊 Perbandingan Karakteristik 5 Protokol

| Parameter | Static Routing | EIGRP | RIPv2 | OSPF | BGP (eBGP) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Jenis Routing** | Static (Manual) | Dynamic (IGP) | Dynamic (IGP) | Dynamic (IGP) | Dynamic (EGP) |
| **Klasifikasi Algoritma**| Manual Entry | Advanced Distance-Vector (DUAL) | Distance-Vector (Bellman-Ford) | Link-State (Dijkstra SPF) | Path-Vector |
| **Admin Distance (AD)** | `1` | `90` | `120` | `110` | `20` |
| **Metrik Terbaik** | Manual | Bandwidth & Delay | Hop Count (Maks 15) | Cost (Bandwidth) | Path Attributes (AS-Path) |
| **Alamat Update** | Tidak Ada | Multicast `224.0.0.10` | Multicast `224.0.0.9` | Multicast `224.0.0.5` / `.6` | Unicast TCP Port `179` |
| **Kode di `sh ip route`**| `S` | `D` | `R` | `O` | `B` |

---

## ⚙️ Konfigurasi Dasar (IP Address Interface & PC)

Sebelum mengonfigurasi protokol routing apa pun, terapkan konfigurasi IP dasar ini pada ketiga router di masing-masing topologi:

<details>
<summary><b>👉 Klik untuk melihat Script Konfigurasi Dasar Ketiga Router</b></summary>

#### 1. Router Tengah (Atas)
```cisco
enable
configure terminal
hostname R-Tengah
interface GigabitEthernet0/0
 ip address 200.10.10.1 255.255.255.252
 no shutdown
exit
interface GigabitEthernet0/1
 ip address 200.20.20.1 255.255.255.252
 no shutdown
exit
```

#### 2. Router Kiri
```cisco
enable
configure terminal
hostname R-Kiri
interface GigabitEthernet0/0
 ip address 200.10.10.2 255.255.255.252
 no shutdown
exit
interface GigabitEthernet0/1
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
```

#### 3. Router Kanan
```cisco
enable
configure terminal
hostname R-Kanan
interface GigabitEthernet0/0
 ip address 200.20.20.2 255.255.255.252
 no shutdown
exit
interface GigabitEthernet0/1
 ip address 192.168.20.1 255.255.255.0
 no shutdown
exit
```
</details>

---

## 🛠️ Konfigurasi & Penjelasan Detail 5 Routing

### 1. Static Routing
**Penjelasan:**  
Pada Static Routing, administrator mendaftarkan setiap jaringan yang **tidak terhubung langsung** secara manual menggunakan perintah `ip route [Destination_Network] [Subnet_Mask] [Next_Hop_IP]`.
* *R-Tengah* membutuhkan rute menuju LAN Kiri (`192.168.10.0/24`) melalui *R-Kiri* (`200.10.10.2`) dan menuju LAN Kanan (`192.168.20.0/24`) melalui *R-Kanan* (`200.20.20.2`).
* *R-Kiri* dan *R-Kanan* mengarahkan seluruh paket menuju seberang melalui IP interface *R-Tengah*.

```cisco
! === KONFIGURASI PADA R-TENGAH ===
ip route 192.168.10.0 255.255.255.0 200.10.10.2
ip route 192.168.20.0 255.255.255.0 200.20.20.2

! === KONFIGURASI PADA R-KIRI ===
ip route 200.20.20.0 255.255.255.252 200.10.10.1
ip route 192.168.20.0 255.255.255.0 200.10.10.1

! === KONFIGURASI PADA R-KANAN ===
ip route 200.10.10.0 255.255.255.252 200.20.20.1
ip route 192.168.10.0 255.255.255.0 200.20.20.1
```

---

### 2. EIGRP (Enhanced Interior Gateway Routing Protocol)
**Penjelasan:**  
EIGRP adalah protokol *dynamic routing* cepat berbasis algoritma DUAL. Setiap router hanya perlu mengiklankan jaringan yang **terhubung langsung (*Directly Connected*)** pada dirinya.
* Menggunakan nomor **AS (Autonomous System) `10`** yang wajib sama di semua router.
* Menggunakan *Wildcard Mask* (`0.0.0.3` untuk `/30` dan `0.0.0.255` untuk `/24`) serta `no auto-summary` agar mendukung subnetting *classless*.

```cisco
! === KONFIGURASI PADA R-TENGAH ===
router eigrp 10
 network 200.10.10.0 0.0.0.3
 network 200.20.20.0 0.0.0.3
 no auto-summary
exit

! === KONFIGURASI PADA R-KIRI ===
router eigrp 10
 network 200.10.10.0 0.0.0.3
 network 192.168.10.0 0.0.0.255
 no auto-summary
exit

! === KONFIGURASI PADA R-KANAN ===
router eigrp 10
 network 200.20.20.0 0.0.0.3
 network 192.168.20.0 0.0.0.255
 no auto-summary
exit
```

---

### 3. RIPv2 (Routing Information Protocol Version 2)
**Penjelasan:**  
RIPv2 menentukan jalur terbaik berdasarkan jumlah lompatan router (*hop count*).
* Perintah `version 2` wajib diketik agar mendukung pengiriman informasi *subnet mask* (VLSM).
* Pendaftaran `network` pada RIP hanya menuliskan alamat jaringan induknya tanpa *subnet/wildcard mask*.

```cisco
! === KONFIGURASI PADA R-TENGAH ===
router rip
 version 2
 network 200.10.10.0
 network 200.20.20.0
 no auto-summary
exit

! === KONFIGURASI PADA R-KIRI ===
router rip
 version 2
 network 200.10.10.0
 network 192.168.10.0
 no auto-summary
exit

! === KONFIGURASI PADA R-KANAN ===
router rip
 version 2
 network 200.20.20.0
 network 192.168.20.0
 no auto-summary
exit
```

---

### 4. OSPF (Open Shortest Path First)
**Penjelasan:**  
OSPF adalah protokol *Link-State* yang membangun peta topologi menggunakan algoritma *Shortest Path First* (Dijkstra).
* `router ospf 1` mendefinisikan *Process ID* lokal bernilai `1`.
* Seluruh jaringan ditempatkan pada **`area 0`** (*Backbone Area*) dengan menyertakan *Wildcard Mask*.

```cisco
! === KONFIGURASI PADA R-TENGAH ===
router ospf 1
 network 200.10.10.0 0.0.0.3 area 0
 network 200.20.20.0 0.0.0.3 area 0
exit

! === KONFIGURASI PADA R-KIRI ===
router ospf 1
 network 200.10.10.0 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.255 area 0
exit

! === KONFIGURASI PADA R-KANAN ===
router ospf 1
 network 200.20.20.0 0.0.0.3 area 0
 network 192.168.20.0 0.0.0.255 area 0
exit
```

---

### 5. BGP (Border Gateway Protocol)
**Penjelasan:**  
BGP adalah protokol antar-ISP (*Autonomous System* berbeda / **eBGP**). BGP tidak mencari tetangga secara otomatis, melainkan harus didaftarkan secara manual dengan perintah `neighbor [IP_Lawan] remote-as [AS_Lawan]`, lalu mengiklankan jaringan lokal dengan `network [Net_ID] mask [Subnet_Mask]`.
* **R-Kiri:** `AS 100`
* **R-Tengah:** `AS 200`
* **R-Kanan:** `AS 300`

```cisco
! === KONFIGURASI PADA R-TENGAH (AS 200) ===
router bgp 200
 neighbor 200.10.10.2 remote-as 100
 neighbor 200.20.20.2 remote-as 300
 network 200.10.10.0 mask 255.255.255.252
 network 200.20.20.0 mask 255.255.255.252
exit

! === KONFIGURASI PADA R-KIRI (AS 100) ===
router bgp 100
 neighbor 200.10.10.1 remote-as 200
 network 200.10.10.0 mask 255.255.255.252
 network 192.168.10.0 mask 255.255.255.0
exit

! === KONFIGURASI PADA R-KANAN (AS 300) ===
router bgp 300
 neighbor 200.20.20.1 remote-as 200
 network 200.20.20.0 mask 255.255.255.252
 network 192.168.20.0 mask 255.255.255.0
exit
```

---

## ✅ Pengujian Koneksi (Ping & Traceroute)

Setelah konfigurasi selesai, lakukan pengujian dari **Command Prompt PC Kiri 1 (`192.168.10.2`)** menuju **PC Kanan 1 (`192.168.20.2`)**:

### 1. Uji Ping
```text
C:\> ping 192.168.20.2

Pinging 192.168.20.2 with 32 bytes of data:
Reply from 192.168.20.2: bytes=32 time<1ms TTL=125
Reply from 192.168.20.2: bytes=32 time<1ms TTL=125
Reply from 192.168.20.2: bytes=32 time<1ms TTL=125
Reply from 192.168.20.2: bytes=32 time<1ms TTL=125

Ping statistics for 192.168.20.2:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
```
*(Catatan: Ping pertama mungkin mengalami `Request timed out` karena proses resolusi ARP, namun ping kedua dan seterusnya akan `Reply` 100%).*

### 2. Uji Jalur Paket (Traceroute)
```text
C:\> tracert 192.168.20.2

Tracing route to 192.168.20.2 over a maximum of 30 hops:
  1   0 ms   0 ms   0 ms   192.168.10.1   (Gateway R-Kiri)
  2   0 ms   0 ms   0 ms   200.10.10.1    (Hop R-Tengah)
  3   0 ms   0 ms   0 ms   200.20.20.2    (Hop R-Kanan)
  4   0 ms   0 ms   0 ms   192.168.20.2   (Tujuan Akhir PC Kanan)

Trace complete.
```

---

## 🔧 Panduan Troubleshooting

Jika koneksi antar-PC gagal (*Request Timed Out* atau *Destination Host Unreachable*), periksa hal berikut:

1. **Muncul Pesan `Reply from 192.168.10.1: Destination host unreachable`:**
   * Artinya **R-Kiri belum memiliki rute** menuju jaringan `192.168.20.0`. Cek kembali tabel routing di R-Kiri dengan perintah `show ip route`.
2. **Muncul Pesan `Request timed out.` (RTO Semua):**
   * Kemungkinan paket berhasil sampai ke PC Kanan, tetapi **tidak tahu jalan pulang**, atau **Default Gateway pada PC** belum diisi (`192.168.10.1` untuk PC Kiri dan `192.168.20.1` untuk PC Kanan).
3. **Lampu Kabel Masih Merah:**
   * Pastikan kedua ujung interface router sudah diberikan perintah `no shutdown`.
4. **Rute BGP Tidak Muncul di `show ip route`:**
   * Pastikan subnet mask pada perintah `network ... mask ...` di BGP sama persis dengan subnet mask yang terpasang di interface router.

---

## 👤 Author
* **Nama:** [Rizqi Maulana Yusuf]
* **GitHub:** [@rizqi-yusuf](https://github.com/rizqi-yusuf)
