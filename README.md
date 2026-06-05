# ⚡ Jarvis BMS Monitor

Aplikasi Android untuk monitoring **JK BMS** via **Bluetooth BLE** dengan notifikasi suara ala **Jarvis (Iron Man)**.

---

## ✨ Fitur

| Fitur | Keterangan |
|-------|-----------|
| 🔵 BLE Connect | Konek langsung ke JK BMS tanpa internet |
| 🗣️ Suara Jarvis | Notifikasi suara (Indonesia / English) |
| 🔋 Alert Baterai Rendah | Peringatan saat SOC di bawah threshold |
| 🚨 Alert Kritis | Peringatan darurat saat SOC sangat rendah |
| ⚡ Alert Charging 90% | Pemberitahuan charging hampir penuh |
| ✅ Alert Charging 100% | Pemberitahuan baterai penuh, cabut charger |
| 🌡️ Alert Suhu Tinggi | Peringatan jika suhu melewati batas |
| 📊 Dashboard Realtime | SOC, Voltage, Current, Power, Suhu, Cell |
| 📝 Log History | Riwayat data tersimpan 7 hari |
| 🔄 Auto Reconnect | Otomatis konek ulang jika terputus |
| 📱 Auto Start | Service jalan otomatis saat HP menyala |
| ✈️ Full Offline | Tidak butuh internet sama sekali |

---

## 🛠️ Cara Build di Android Studio

### Prasyarat
- **Android Studio** Hedgehog (2023.1.1) atau lebih baru
- **JDK 17**
- **Android SDK** minimal API 26 (Android 8.0)
- **HP Android** dengan Bluetooth BLE

### Langkah-langkah

1. **Clone / buka project**
   ```
   File → Open → pilih folder JarvisBMS
   ```

2. **Sync Gradle**
   - Klik "Sync Now" yang muncul di bagian atas
   - Atau: File → Sync Project with Gradle Files

3. **Tambahkan icon launcher** (wajib)
   - Klik kanan `app/src/main/res` → New → Image Asset
   - Pilih gambar ikon sesuai keinginan
   - Atau gunakan icon default bawaan Android Studio

4. **Build APK**
   ```
   Build → Build Bundle(s) / APK(s) → Build APK(s)
   ```
   APK akan ada di: `app/build/outputs/apk/debug/app-debug.apk`

5. **Install ke HP**
   - Aktifkan "Developer Options" dan "USB Debugging" di HP
   - Sambungkan HP ke laptop via USB
   - Klik tombol ▶ Run (hijau) di Android Studio

---

## 📱 Cara Pakai Aplikasi

1. **Buka aplikasi** → Tap **"Cari BMS"**
2. Pastikan **JK BMS sudah aktif** dan Bluetooth HP menyala
3. Pilih perangkat dari daftar yang muncul
4. Tap **"Hubungkan"** → tunggu sampai status berubah jadi hijau
5. Dashboard akan menampilkan data realtime
6. Buka **Pengaturan** (ikon ⚙️) untuk atur threshold notifikasi

---

## ⚙️ Pengaturan yang Bisa Disesuaikan

| Pengaturan | Default | Keterangan |
|-----------|---------|-----------|
| Peringatan Baterai Rendah | 20% | SOC di bawah ini = notifikasi |
| Peringatan Kritis | 10% | SOC di bawah ini = alert merah |
| Notifikasi Charging 90% | ON | Jarvis akan memberitahu |
| Notifikasi Charging 100% | ON | Jarvis akan memberitahu |
| Bahasa Suara | Indonesia | Bisa ganti ke English |
| Batas Suhu Tinggi | 45°C | Di atas ini = alert suhu |
| Auto Reconnect | ON | Otomatis konek ulang |

---

## 🔧 Protokol JK BMS BLE

| Item | Value |
|------|-------|
| Service UUID | `0000ffe0-0000-1000-8000-00805f9b34fb` |
| Notify UUID | `0000ffe1-0000-1000-8000-00805f9b34fb` |
| Write UUID | `0000ffe1-0000-1000-8000-00805f9b34fb` |
| Polling Interval | 2 detik |
| Frame Header | `4E 57` (NW) |

---

## 📁 Struktur Project

```
JarvisBMS/
├── app/src/main/
│   ├── java/com/jarvis/bmsmonitor/
│   │   ├── bluetooth/
│   │   │   ├── JkBmsProtocol.kt   ← Parser data BMS
│   │   │   └── BleManager.kt      ← Koneksi BLE
│   │   ├── model/
│   │   │   └── BmsDatabase.kt     ← Room database log
│   │   ├── service/
│   │   │   ├── BmsService.kt      ← Foreground service utama
│   │   │   └── BootReceiver.kt    ← Auto start saat boot
│   │   ├── ui/
│   │   │   ├── MainActivity.kt    ← Dashboard utama
│   │   │   ├── ScanActivity.kt    ← Scan perangkat BLE
│   │   │   ├── SettingsActivity.kt← Pengaturan
│   │   │   └── LogActivity.kt     ← Riwayat log
│   │   └── util/
│   │       ├── JarvisVoice.kt     ← Text-to-Speech Jarvis
│   │       ├── AppPreferences.kt  ← SharedPreferences
│   │       └── NotifHelper.kt     ← Notification channels
│   ├── res/layout/                ← Layout XML
│   └── AndroidManifest.xml
└── README.md
```

---

## ❗ Troubleshooting

**BMS tidak terdeteksi saat scan?**
- Pastikan Bluetooth aktif di HP
- Pastikan lokasi (GPS) diizinkan - Android butuh ini untuk BLE scan
- Coba restart JK BMS

**Suara Jarvis tidak keluar?**
- Cek volume HP (bukan volume call, tapi volume media)
- Buka Pengaturan → tap "Test Suara Jarvis"
- Pastikan HP sudah install TTS engine (biasanya sudah ada di Android)

**Koneksi sering putus?**
- Jauhkan dari interferensi WiFi 2.4GHz
- Pastikan tidak ada banyak perangkat BLE aktif di sekitar
- Aktifkan "Auto Reconnect" di Pengaturan

---

## 📋 Izin yang Dibutuhkan

- `BLUETOOTH_SCAN` & `BLUETOOTH_CONNECT` - untuk scan & konek ke BMS
- `ACCESS_FINE_LOCATION` - wajib untuk BLE scan di Android
- `POST_NOTIFICATIONS` - untuk notifikasi alert
- `FOREGROUND_SERVICE` - agar service jalan di background
- `RECEIVE_BOOT_COMPLETED` - auto start saat HP nyala
- `WAKE_LOCK` - agar service tidak mati saat layar off

---

*Jarvis BMS Monitor v1.0 | Dibuat dengan ❤️ untuk pengguna JK BMS*
