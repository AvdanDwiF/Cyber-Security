# 🛡️ Wazuh SIEM Attack Detection Lab
### Simulasi Attack & Defense Server — PSAS Kelompok 5[cite: 1, 2]

Analisis Log & Keamanan Jaringan Server[cite: 1, 2]

---

## 📌 Deskripsi

Modul praktikum ini dirancang sebagai panduan operasional berskala laboratorium untuk memahami bagaimana serangan umum pada layanan jaringan bekerja, serta bagaimana **Wazuh SIEM** mendeteksi aktivitas mencurigakan tersebut secara real-time[cite: 1]. 

Setiap skenario serangan dipetakan langsung dengan hasil analisis log deteksi pada dasbor Wazuh untuk membantu tim bertahan (Defender) mengenali pola insiden siber secara mendalam[cite: 1, 2].

---

## 🧪 Attack Scenarios

| # | Serangan | Tool | Target Service | Port / Indikator Wazuh |
|---|----------|------|----------------|------------------------|
| 1 | Port Scanning & Reconnaissance[cite: 1] | *Automated*[cite: 1] | Network Discovery[cite: 1] | Rule ID 40111[cite: 1] |
| 2 | SSH Brute Force[cite: 1] | `hydra`[cite: 1] | OpenSSH (Target: root)[cite: 1] | Rule ID 100001 (Lvl 10)[cite: 2] |
| 3 | FTP Brute Force[cite: 1] | `hydra`[cite: 1] | vsftpd (Target: root)[cite: 2] | Rule ID 100083 (Lvl 10)[cite: 2] |
| 4 | Directory Brute Force[cite: 1] | `dirb`[cite: 1] | Apache Web Server[cite: 1] | Rule ID 100005 (Lvl 10)[cite: 2] |
| 5 | Web Vulnerability Scanning[cite: 1] | `nikto`[cite: 1] | Apache Web Server[cite: 1] | Rule ID 100005 (Lvl 10)[cite: 2] |
| 6 | SMB Enumeration[cite: 1] | `smbclient` / `enum4linux`[cite: 1] | Samba / Shared Folder[cite: 1] | Rule ID 100004 (Lvl 10)[cite: 2] |

---

## 🏗️ Environment Lab

| Role | Spesifikasi / Alamat IP | Detail Layanan Aktif |
|------|-------------|----------------------|
| **Hypervisor Platform** | Proxmox OS — `175.17.0.237`[cite: 1] | Virtualization Environment[cite: 1] |
| **Wazuh Manager** | Wazuh SIEM Monitor — `175.17.0.239`[cite: 1] | Dashboard & Log Collector Hub[cite: 1] |
| **Target Server** | Vulnerable Target — `175.17.0.240`[cite: 1] | SSH, FTP, SMB, Apache2[cite: 1] |
| **Attacker Machine** | Kali Linux Machine[cite: 2] | IP: `17.17.17.2` (Identitas: `KALI`)[cite: 2] |

> ⚠️ Semua pengujian dilakukan dalam environment lab terisolasi khusus untuk keperluan edukasi[cite: 1]. IP address dan konfigurasi yang tertera hanya berlaku di dalam jaringan lab internal[cite: 1].

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
| 1  | Arfian Hananta Yudo[cite: 2] | 541241415[cite: 2] |
| 2  | Avdan Dwi Firlanda[cite: 2]  | 541241424[cite: 2] |
| 3  | Davin Elianoor[cite: 2]      | 541251530[cite: 2] |
| 4  | Fabian Ezar P[cite: 2]       | 541241438[cite: 2] |

---

## 🎓 Informasi Akademik

| Keterangan | Detail |
|------------|--------|
| **Sekolah** | SMK Telkom Purwokerto[cite: 3] |
| **Program Keahlian** | Teknik Jaringan Komputer dan Telekomunikasi (TJKT)[cite: 3] |
| **Mata Pelajaran** | Keamanan Jaringan (MK2)[cite: 3] |
| **Jenis Penilaian** | PSAS - Penilaian Sumatif Akhir Semester[cite: 1] |
| **Kelas** | XI TJKT 2[cite: 3] |
| **Tahun Ajaran** | 2025/2026[cite: 3] |

---

## 📚 Tools & Sintaks yang Digunakan

| Tool | Fungsi / Deskripsi | Contoh Perintah Eksploitasi |
|------|--------------------|-----------------------------|
| `hydra` | Network login bruteforcer[cite: 1] | `hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://175.17.0.240`[cite: 1] |
| `dirb` | Web directory fuzzer / bruteforcer[cite: 1] | `dirb http://175.17.0.240`[cite: 1] |
| `nikto` | Web server vulnerability scanner[cite: 1] | `nikto -h http://175.17.0.240`[cite: 1] |
| `smbclient` | SMB/Samba client enumeration[cite: 1] | `smbclient -L//175.17.0.240 -N`[cite: 1] |
| `Wazuh` | Open source SIEM & Monitoring[cite: 1] | Berperan sebagai sistem deteksi berbasis Rule ID[cite: 1]. |

---

## 💡 Poin Pembelajaran & Temuan Lab

- **Analisis Frekuensi Serangan:** Volume kegagalan otentikasi yang tinggi dalam waktu singkat (seperti **427 kali** pada FTP dan **153 kali** pada SSH) menjadi indikator valid adanya serangan berbasis *automated tools*[cite: 2].
- **Sidik Jari Penyerang (Fingerprinting):** Log Samba berhasil mengidentifikasi identitas mesin penyerang menggunakan nama host `KALI` dan user `kali` dengan status alert `NT_STATUS_NO_SUCH_USER`[cite: 2].
- **Web Directory Traversal:** Deteksi HTTP `GET` dengan respons `404` sebanyak **331 kali** membuktikan bahwa penyerang mencoba memetakan folder sensitif seperti `/level/54/exec//show`[cite: 2].
- **Pentingnya SIEM:** Melalui decoder seperti `sshd`, `vsftpd`, `samba_no_user`, dan `web-accesslog`, log mentah (*raw log*) dapat diurai menjadi peringatan keamanan berstatus *High Severity* (Level 10) secara akurat[cite: 2].

---

> **Disclaimer:** Semua teknik yang didokumentasikan dalam repositori ini hanya untuk keperluan edukasi dalam environment lab yang terisolasi[cite: 1]. Penggunaan teknik ini terhadap sistem tanpa izin resmi adalah tindakan **ilegal dan melanggar hukum**.

```
