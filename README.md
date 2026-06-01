# TEK1314-2025-Kel2-KelasG1

## Keamanan Siber  
**Kelompok 2**

### Anggota Kelompok
| Nama | NIM | Phase 1 | Phase 2-3 |
|------|-----|------|-----|
| Baracahya Panata Cendikia Rahayu | J0404231014 | Team Lead | Sec.Analyst |
| Niefa Efrilia Violenic | J0404231092 | Sec.Analyst1 | Red Team |
| Rizki Maariz Senjaya | J0404231139 | Sec.Analyst2 | Red Team |
| Aldy Arifyan Putra | J0404231078 | Sec.Engineer | Blue Team |


# 🖥️ Network Topology Overview

## 📌 Network Hosts Information

| Hostname | Role / Fungsi | IP Address | Network Subnet | Operating System |
| :--- | :--- | :--- | :--- | :--- |
| **PC-PT Attacker-Kel02G** | Attacker (Penyerang / Penguji) | `192.168.30.10` | `192.168.30.0/24` | Cybersecurity |
| **PC-PT Defender-Kel02G** | Defender (Hardening / Monitoring) | `192.168.20.10` | `192.168.20.0/24` | Security Onion |
| **Server-PT Windows-Web-Kel02G** | Target Server | `192.168.2.10` | `192.168.2.0/24` | Windows Server |

# Baseline Report

## Deskripsi Sistem
Lingkungan pengujian dibuat menggunakan beberapa mesin virtual yang terdiri dari mesin tester, server target, dan sistem monitoring. Sistem ini digunakan untuk melakukan simulasi pengujian keamanan jaringan serta memantau aktivitas jaringan.


## Pengujian Konektivitas
Pengujian dilakukan menggunakan perintah berikut untuk memastikan koneksi antara tester dan server target berjalan dengan baik.

```bash
sudo hping3 -1 192.168.10.10
```
Lalu pada SecOnion untuk mendeteksi lalulintas jaringan menuju Server.

```bash
sudo tshark -i eth1
sudo tcpdump -i eth1 udp port 37008 -X
```

## Analisis dan Pembahasan Eksekusi

Pada tahapan **Phase 2 - Attack**, dilakukan simulasi serangan ofensif terhadap server target `192.168.2.10` yang menghosting situs web `sman1nusantara.com`. Berikut adalah rincian aktivitas yang terekam pada dokumentasi di atas:

#### 1. Eksekusi Serangan Ofensif (THC-Hydra)
* **Metode Serangan:** *Brute Force Attack* terhadap form login HTTP POST (`http-form-post`).
* **Tools yang Digunakan:** `Hydra v9.2`
* **Perintah yang Dijalankan:**
```bash
  hydra -l admin -P /usr/share/wordlists/rockyou.txt 192.168.2.10 http-form-post "/login.php:user=^USER^&pass=^PASS^:F=Login Failed"
```
* **Analisis Perintah:** Penyerang mencoba masuk ke akun `admin` dengan menguji ribuan kombinasi kata sandi secara otomatis yang diambil dari *wordlist* `rockyou.txt`. Permintaan (request) dikirimkan dalam volume tinggi tanpa jeda waktu (*delay*) menuju target server `192.168.2.10`.
* **Hasil Pengujian (Output Hydra):** Proses audit otentikasi selesai dengan status `1 of 1 target completed, 0 valid password found`. Serangan ini tidak berhasil menemukan kata sandi yang valid, namun intensitas request yang sangat tinggi memberikan dampak sekunder pada server.

#### 2. Dampak Pada Sisi Target (Web Server)
* **Kondisi Layanan:** Akibat dibanjiri oleh ribuan request login palsu per menit dari program Hydra, alokasi *resource* CPU dan memori pada `Server-PT Windows-Web-Kel02G` mengalami saturasi (beban penuh).
* **Status Kendala:** Saat tim mencoba memvalidasi akses web melalui browser Firefox, situs web `sman1nusantara.com` gagal dimuat dan memunculkan pesan galat **"The connection has timed out"**. 
* **Kesimpulan Pengujian:** Aktivitas *Brute Force* yang agresif secara tidak langsung memicu kondisi **Denial of Service (DoS)**, membuat layanan aplikasi web lumpuh total bagi pengguna sah selama serangan berlangsung.

---

# 🛡️ Phase 3 - Defense & Hardening Report

## 📌 Deteksi Insiden (Security Onion & Kibana Logs)
Selama fase serangan berlangsung, tim Blue Team berhasil mengidentifikasi dan merekam aktivitas ilegal tersebut melalui sistem IDS Snort dan visualisasi dasbor Kibana Discover.

### 1. Rekaman Log Peringatan Dini (ICMP Sweep Detection)
Sebelum melancarkan Brute Force, penyerang terdeteksi melakukan *scanning* jaringan menggunakan protokol ICMP untuk memetakan status *up-time* host. Log terstruktur berhasil diekstrak dari `sguild.log` dengan rincian berikut:
* **Event ID:** 2206
* **Signature Alert:** `ICMP DETECTED - ALL`
* **Protokol:** ICMP (Event Type: Snort)
* **Waktu Kejadian:** `2026-03-31T10:41:47.000Z`

### 2. Rekaman Log Aktivitas Akses Jaringan (Port 22/SSH Log)
Selain menembak port web (80), dasbor Kibana juga merekam adanya anomali traffic berupa percobaan koneksi menuju port manajemen internal (`destination_port: 22` / SSH) pada subnet Defender (`192.168.20.10`) yang diinisiasi oleh IP eksternal penyerang `10.251.92.111`.

---

## 🛠️ Langkah-Langkah Pengerasan Sistem (System Hardening)
Untuk mengatasi kelumpuhan layanan web serta mencegah eksploitasi berulang di masa mendatang, tim Defender telah menerapkan tiga strategi mitigasi utama:

### 1. Isolasi Instan via Firewall Router (Containment)
Memanfaatkan `Mikrotik-Kel02G` sebagai gerbang utama jaringan, diterapkan aturan *drop rule* khusus untuk memutus komunikasi langsung dari segmen Attacker menuju Web Server guna memulihkan memori server yang terbebani:
```bash
/ip firewall filter add action=drop chain=forward src-address=192.168.30.10 dst-address=192.168.2.10 comment="Isolasi IP Attacker dari Akses Web Server"
