# 🎨 Blueprint Desain Antarmuka Figma & Spesifikasi Data Menu NoSQL — GoTo Pulse

Dokumen ini disusun untuk memenuhi ketentuan tugas:
> *"Untuk bagian tampilan aplikasi, data yang ditampilkan diarahkan berasal dari Realtime Database. Figma digunakan untuk merancang tampilan aplikasi, sekaligus menentukan data menu yang nantinya akan diambil dari NoSQL."*

---

## 1. Arsitektur Integrasi Figma → NoSQL Realtime Database → Aplikasi (`app.py`)

```
+-------------------------------------------------------------------------+
| 1. DESAIN UI/UX DI FIGMA (5 Frame Utama + Sidebar Navigation Component) |
|    Menentukan struktur menu (menu_id, title, icon, figma_frame_id,      |
|    nosql_source_nodes, sql_source_tables)                               |
+------------------------------------+------------------------------------+
                                     |
                                     v
+-------------------------------------------------------------------------+
| 2. DATABASE NOSQL / REALTIME DATABASE (goto_pulse_nosql_realtime.json   |
|    & Cloud Firebase Realtime Database)                                  |
|    - Node `/figma_navigation_menus`     --> Menyimpan daftar menu UI    |
|    - Node `/realtime_dashboard_summary` --> Menyimpan Summary & KPI     |
|    - Node `/realtime_critical_alerts`   --> Menyimpan Peringatan Churn  |
|    - Node `/live_prediction_stream`     --> Menyimpan Stream Prediksi   |
+------------------------------------+------------------------------------+
                                     |
                                     v
+-------------------------------------------------------------------------+
| 3. DATABASE SQL RELASIONAL (SQLite3: goto_pulse_sql.db)                 |
|    - Tabel `platforms` (2 baris)                                        |
|    - Tabel `app_versions` (227 versi rilis & Z-Score regresi)           |
|    - Tabel `reviews_processed` (42.197 baris ulasan terolah)            |
|    - Tabel `model_benchmarks` (3 model komparasi IndoBERT vs SVM/LR)    |
|    - Tabel `bertopic_clusters` (5 klaster topik bug c-TF-IDF)           |
|    - Tabel `live_predictions_log` (Audit log prediksi real-time)        |
+------------------------------------+------------------------------------+
                                     |
                                     v
+-------------------------------------------------------------------------+
| 4. TAMPILAN APLIKASI WEB INTERAKTIF STREAMLIT (`app.py`)                |
|    - Sidebar merender menu secara dinamis dari `/figma_navigation_menus`|
|    - Kartu Summary & Grafik Ringkasan dirender dari Realtime Database   |
|    - Tabel Analitik & Filter dieksekusi dari Database SQL               |
+-------------------------------------------------------------------------+
```

---

## 2. Spesifikasi Desain Sistem (Design System) di Figma

Jika ingin menampilkan kanvas Figma saat presentasi progres, gunakan spesifikasi komponen berikut:

### A. Ukuran Frame & Grid (Desktop Web Dashboard)
- **Ukuran Kanvas Frame:** `1440 x 1024 px` (Desktop 16:10 / Web Dashboard)
- **Layout Grid:**
  - **Sidebar Kiri (Fixed Navigation):** Lebar `300 px`, Background `#0F172A` (Slate Dark) / `#F8FAFC` (Light Mode)
  - **Konten Utama (Main Viewport):** Lebar `1140 px`, Padding kiri-kanan `32 px`, Grid `12 Columns` (Gutter `20 px`)

### B. Palet Warna (Color Tokens — Ekosistem GoTo)
- **Gojek Green (Primary Brand):** `#00AA13`
- **Tokopedia Emerald:** `#42B549`
- **GoPay Blue (Info / Tech Accent):** `#00AED6`
- **High Churn / Critical Alert (Red):** `#E74C3C`
- **Moderate Risk / Warning (Amber):** `#F1C40F`
- **Low Risk / Stable (Green):** `#2ECC71`

---

## 3. Wireframe Layout & Pemetaan 5 Frame Figma ke Data Menu NoSQL

Berikut adalah 5 Frame utama pada rancangan Figma beserta struktur JSON yang disimpan di node `/figma_navigation_menus` pada **Database NoSQL**:

| ID Menu NoSQL | Nama Frame di Figma | Judul Menu di Aplikasi | Node Data Realtime Database (NoSQL) | Tabel Pendukung (SQL) |
| :--- | :--- | :--- | :--- | :--- |
| **`M01`** | `Frame-101:MultiAspectClassifier` | `1. Multi-Aspect Issue Classifier` | `/figma_navigation_menus/M01`, `/realtime_dashboard_summary/issue_triage_summary` | `reviews_processed`, `model_benchmarks`, `platforms` |
| **`M02`** | `Frame-102:VersionBugRadar` | `2. App Version Bug Radar (BERTopic)` | `/figma_navigation_menus/M02`, `/realtime_dashboard_summary/version_regression_summary` | `app_versions`, `bertopic_clusters`, `reviews_processed` |
| **`M03`** | `Frame-103:ChurnRiskRadar` | `3. Customer Churn Risk Radar (0-100%)` | `/figma_navigation_menus/M03`, `/realtime_dashboard_summary/churn_risk_summary`, `/realtime_critical_alerts` | `reviews_processed` |
| **`M04`** | `Frame-104:LiveDemoPredictor` | `4. Live Demo Predictor (Real-Time)` | `/figma_navigation_menus/M04`, `/live_prediction_stream` | `live_predictions_log` |
| **`M05`** | `Frame-105:DualDatabaseInspector` | `5. Arsitektur Dual Database (SQL & NoSQL) + Figma` | `/figma_navigation_menus`, `/realtime_dashboard_summary`, `/live_prediction_stream` | Seluruh 6 Tabel SQL |

---

## 4. Sketsa Tata Letak (Wireframe) Setiap Frame di Figma

```
+------------------------+---------------------------------------------------------------+
| SIDEBAR (300px)        | HEADER HALAMAN & BANNER SUMBER DATA REALTIME DB + SQL         |
| [Diambil dari NoSQL    +---------------+---------------+---------------+---------------+
|  /figma_navigation_    | KPI CARD 1    | KPI CARD 2    | KPI CARD 3    | KPI CARD 4    |
|  menus]                | (Realtime DB) | (Realtime DB) | (Realtime DB) | (Realtime DB) |
|                        +---------------+---------------+---------------+---------------+
| 🚀 GoTo Pulse          | PANEL KIRI (Visualisasi       | PANEL KANAN (Tabel Analitik   |
|                        | Summary dari Realtime DB:     | & Benchmarking dari SQL DB:   |
| (o) M01: Issue Triage  | - Pie Chart Distribusi Kelas  | - Tabel Benchmark Model       |
| ( ) M02: Bug Radar     | - Heatmap Regresi Versi       | - Confusion Matrix            |
| ( ) M03: Churn Radar   | - Histogram & Bar Zona Churn  | - Tabel c-TF-IDF & Z-Score    |
| ( ) M04: Live Demo     | - Gauge Chart Skor 0-100%)    | - Rincian Skor Churn)         |
| ( ) M05: Dual DB Spec  +-------------------------------+-------------------------------+
|                        | TABEL BAWAH (Data Detail / Live Stream Prediksi Real-Time)    |
| ---------------------- | - Modul 1-3: Tabel Filter Interaktif dari SQL Database        |
| Status Dual DB:        | - Modul 4: Live Stream Riwayat Prediksi dari NoSQL            |
| 🟢 SQL: 42.197 Baris   |   `/live_prediction_stream`                                   |
| 🟢 NoSQL: Realtime DB  |                                                               |
+------------------------+---------------------------------------------------------------+
```

---

## 5. Struktur JSON Menu di Database NoSQL (`/figma_navigation_menus`)

Berikut adalah potongan dokumen JSON aktual yang disimpan di dalam `goto_pulse_nosql_realtime.json` (dan disinkronkan ke Firebase Realtime Database):

```json
{
  "figma_navigation_menus": {
    "M01": {
      "menu_id": "M01",
      "menu_key": "1. Multi-Aspect Issue Classifier",
      "short_title": "Auto-Bug Triage",
      "icon": "🛠️",
      "figma_frame_id": "Frame-101:MultiAspectClassifier",
      "subtitle": "Pemisahan otomatis keluhan Operasional Lapangan vs Bug Teknis IT & Pembayaran",
      "nosql_source_nodes": [
        "/figma_navigation_menus/M01",
        "/realtime_dashboard_summary/issue_triage_summary"
      ],
      "sql_source_tables": [
        "reviews_processed",
        "model_benchmarks",
        "platforms"
      ],
      "order": 1,
      "enabled": true
    }
  }
}
```
