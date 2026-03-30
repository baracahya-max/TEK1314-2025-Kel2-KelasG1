# TEK1314-2025-Kel2-KelasG1

## Keamanan Siber  
**Kelompok 2**

### Anggota Kelompok
| Nama | NIM |
|------|-----|
| Niefa Efrilia Violenic | J0404231092 |
| Baracahya Panata Cendikia Rahayu | J0404231014 |
| Aldy Arifyan Putra | J0404231078 |
| Rizki Maariz Senjaya | J0404231139 |

# 🖥️ Network Topology Overview

## 📌 Network Hosts Information

| Hostname | Role | IP Address | Network | Operating System | Open Ports |
|----------|------|------------|---------|------------------|------------|
| **PC-Client** | User / Tester | 192.168.30.10 | 192.168.30.0/24 | Windows | - |
| **Windows-Web** | Target Server | 192.168.10.10 | 192.168.10.0/24 | Windows Server + IIS | 80/tcp, 443/tcp |
| **CyberOps-Sec** | IDS / Defender | 192.168.20.10 | 192.168.20.0/24 | Security Onion | Monitoring Mode |

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
