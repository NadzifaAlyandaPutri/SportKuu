# ⚡ SportKu — Kalkulator Kebugaran & Kalori Olahraga

Aplikasi Android (Kotlin, XML + ViewBinding) untuk **Tugas 2 — Logika Interaktif & Navigasi** (Pemrograman Mobile, Pertemuan 7).
Pengguna memasukkan data diri & olahraga, lalu aplikasi menghitung BMI, kalori terbakar, dan zona denyut jantung ideal.

## Alur: Input → Proses → Output
| Layar | Fungsi |
|---|---|
| `MainActivity` | Form input, **validasi berlapis**, perhitungan, kirim hasil lewat `Intent` |
| `HasilActivity` | Menerima extra, menampilkan dashboard hasil (BMI, kalori, zona jantung, saran) |

## Logika yang diterapkan
- **BMI** = berat (kg) / tinggi (m)² → kategori `when` (standar WHO: Kurus / Normal / Berat Berlebih / Obesitas)
- **Kalori terbakar** = MET × berat (kg) × durasi (jam), MET tergantung jenis olahraga
- **Zona latihan** = 60–80% dari denyut maksimal (220 − umur)

## Validasi input (berlapis, berhenti di kesalahan pertama)
1. Kolom kosong → `"... tidak boleh kosong"`
2. Format → `toDoubleOrNull()` / `toIntOrNull()` (aman dari crash saat paste huruf)
3. Rentang → berat 20–300 kg, tinggi 100–250 cm, umur 10–80 th, durasi 1–600 menit
4. Pesan error muncul di kolom yang salah (`EditText.error` + `requestFocus()`), **tidak pindah layar & tidak crash**

## Fitur tambahan
- Tema gelap bergaya "HUD" + warna kategori BMI dinamis
- Avatar inisial dari nama, indikator BMI pada skala
- Tombol **Isi Contoh** (demo cepat) & **Reset**
- Konstanta kunci Intent di `companion object` (pengirim & penerima tidak salah ketik)

## Struktur
```
app/src/main/java/com/example/sportku/
├── MainActivity.kt          # layar input, validasi, kirim Intent
├── HasilActivity.kt         # layar hasil
├── KebugaranCalculator.kt   # logika murni (BMI, kalori, zona, saran)
└── Olahraga.kt              # data class + daftar olahraga & nilai MET
app/src/main/res/layout/     # activity_main.xml, activity_hasil.xml, item spinner
```

## Cara menjalankan
1. Clone / ekstrak project
2. Android Studio → **Open** → pilih folder `SportKu`
3. Tunggu Gradle Sync (perlu JDK 17 — bawaan Android Studio terbaru sudah cukup)
4. Run ▶ di emulator / HP (minSdk 24)

## Skenario uji (untuk video demo)
| Kasus | Input | Hasil yang diharapkan |
|---|---|---|
| Valid | Budi Santoso, 65.5 kg, 170 cm, 21 th, Lari, 45 mnt | BMI 22.7 Normal, ±481 kkal, zona 119–159 bpm |
| Nama kosong | (kosongkan nama) | Error di kolom Nama, tetap di layar 1 |
| Berat kosong | (kosongkan berat) | Error di kolom Berat |
| Tinggi di luar rentang | 500 | Error "antara 100 dan 250 cm" |
| Umur di luar rentang | 5 | Error "antara 10 dan 80 tahun" |

> Hasil berupa estimasi umum dan bukan pengganti saran medis.
