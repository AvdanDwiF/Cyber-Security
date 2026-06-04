# 🛡️ Wazuh SIEM Attack Detection Lab
### Simulasi Attack & Defense Server — PSAS Kelompok 5

**Defend Team** | Analisis Log & Keamanan Jaringan Server

---

## 📌 Deskripsi

Modul praktikum ini dirancang sebagai panduan operasional berskala laboratorium untuk memahami bagaimana serangan umum pada layanan jaringan bekerja, serta bagaimana **Wazuh SIEM** mendeteksi aktivitas mencurigakan tersebut secara real-time. 

Setiap skenario serangan dipetakan langsung dengan hasil analisis log deteksi pada dasbor Wazuh untuk membantu tim bertahan (Defender) mengenali pola insiden siber secara mendalam.

---

## 🧪 Attack Scenarios

| # | Serangan | Tool | Target Service | Port / Indikator Wazuh |
|---|----------|------|----------------|------------------------|
| 1 | Port Scanning & Reconnaissance | *Automated* | Network Discovery | Rule ID 40111 |
| 2 | SSH Brute Force | `hydra` | OpenSSH (Target: root) | Rule ID 100001 (Lvl 10) |
| 3 | FTP Brute Force | `hydra` | vsftpd (Target: root) | Rule ID 100083 (Lvl 10) |
| 4 | Directory Brute Force | `dirb` | Apache Web Server | Rule ID 100005 (Lvl 10) |
| 5 | Web Vulnerability Scanning | `nikto` | Apache Web Server | Rule ID 100005 (Lvl 10) |
| 6 | SMB Enumeration | `smbclient` / `enum4linux` | Samba / Shared Folder | Rule ID 100004 (Lvl 10) |

---

## 🏗️ Environment Lab

| Role | Spesifikasi / Alamat IP | Detail Layanan Aktif |
|------|-------------|----------------------|
| **Hypervisor Platform** | Proxmox OS — `175.17.0.237` | Virtualization Environment |
| **Wazuh Manager** | Wazuh SIEM Monitor — `175.17.0.239` | Dashboard & Log Collector Hub |
| **Target Server** | Vulnerable Target — `175.17.0.240` | SSH, FTP, SMB, Apache2 |
| **Attacker Machine** | Kali Linux Machine | IP: `17.17.17.2` (Identitas: `KALI`) |

> ⚠️ Semua pengujian dilakukan dalam environment lab terisolasi khusus untuk keperluan edukasi. IP address dan konfigurasi yang tertera hanya berlaku di dalam jaringan lab internal.

---

## 📂 Struktur Repository


```

wazuh-siem-attack-detection-lab/
├── README.md
├── Modul Panduan Attacking.pdf
└── PPT_SOC_PAS_KELOMPOK_5.pdf

```

---

## 👥 Anggota Kelompok 5 

| No | Nama Anggota[cite: 2] | NIS[cite: 2] |
|----|-----------------------|-----------------|
| 1  | Arfian Hananta Yudo  | 541241415 |
| 2  | Avdan Dwi Firlanda   | 541241424 |
| 3  | Davin Elian Noor     | 541251530 |
| 4  | Fabian Ezar Prasetyo | 541241438 |

---

## 🎓 Informasi Akademik

| Keterangan | Detail |
|------------|--------|
| **Sekolah** | SMK Telkom Purwokerto |
| **Program Keahlian** | Teknik Jaringan Komputer dan Telekomunikasi (TJKT) |
| **Mata Pelajaran** | Keamanan Jaringan (MK2-B) |
| **Jenis Penilaian** | PSAS - Penilaian Sumatif Akhir Semester |
| **Kelas** | XI TJKT 2 |
| **Tahun Ajaran** | 2025/2026 |

---

## 📚 Tools & Sintaks yang Digunakan

| Tool | Fungsi / Deskripsi | Contoh Perintah Eksploitasi |
|------|--------------------|-----------------------------|
| `hydra` | Network login bruteforcer | `hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://175.17.0.240` |
| `dirb` | Web directory fuzzer / bruteforcer | `dirb http://175.17.0.240` |
| `nikto` | Web server vulnerability scanner | `nikto -h http://175.17.0.240` |
| `smbclient` | SMB/Samba client enumeration | `smbclient -L//175.17.0.240 -N` |
| `Wazuh` | Open source SIEM & Monitoring | Berperan sebagai sistem deteksi berbasis Rule ID. |

---

## 💡 Poin Pembelajaran & Temuan Lab

- **Analisis Frekuensi Serangan:** Volume kegagalan otentikasi yang tinggi dalam waktu singkat (seperti **427 kali** pada FTP dan **153 kali** pada SSH) menjadi indikator valid adanya serangan berbasis *automated tools*.
- **Sidik Jari Penyerang (Fingerprinting):** Log Samba berhasil mengidentifikasi identitas mesin penyerang menggunakan nama host `KALI` dan user `kali` dengan status alert `NT_STATUS_NO_SUCH_USER`.
- **Web Directory Traversal:** Deteksi HTTP `GET` dengan respons `404` sebanyak **331 kali** membuktikan bahwa penyerang mencoba memetakan folder sensitif seperti `/level/54/exec//show`.
- **Pentingnya SIEM:** Melalui decoder seperti `sshd`, `vsftpd`, `samba_no_user`, dan `web-accesslog`, log mentah (*raw log*) dapat diurai menjadi peringatan keamanan berstatus *High Severity* (Level 10) secara akurat.

---

> **Disclaimer:** Semua teknik yang didokumentasikan dalam repositori ini hanya untuk keperluan edukasi dalam environment lab yang terisolasi. Penggunaan teknik ini terhadap sistem tanpa izin resmi adalah tindakan **ilegal dan melanggar hukum**.

```
