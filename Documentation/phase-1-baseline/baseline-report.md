# Baseline Report

## Deskripsi Sistem
Lingkungan pengujian dibuat menggunakan beberapa mesin virtual yang terdiri dari mesin tester, server target, dan sistem monitoring. Sistem ini digunakan untuk melakukan simulasi pengujian keamanan jaringan serta memantau aktivitas jaringan.

## Topologi dan Asset

| Hostname | IP Address | Role | OS |
|----------|------------|------|----|
| Security Lab VM | 192.168.10.30 | Tester | Linux |
| Windows Server | 192.168.10.11 | Server Target | Windows |
| Security Onion | 192.168.10.x | IDS Monitoring | Security Onion |

## Pengujian Konektivitas
Pengujian dilakukan menggunakan perintah berikut untuk memastikan koneksi antara tester dan server target berjalan dengan baik.

```bash
ping -c 10 192.168.10.11
