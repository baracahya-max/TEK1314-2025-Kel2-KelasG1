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
