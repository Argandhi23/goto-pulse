# Goto Pulse

Analisis dan visualisasi data review sentimen pengguna untuk ekosistem GoTo (Gojek & Tokopedia).

## Deskripsi
Proyek ini berisi visualisasi dan analisis data sentiment dari ulasan pengguna Gojek dan Tokopedia.

## Struktur File
- `app.py`: Aplikasi utama Streamlit (5 Modul: Multi-Aspect Issue Classifier, App Version Bug Radar, Customer Churn Risk Radar, Live Demo Predictor, & Inspeksi Arsitektur Dual Database SQL/NoSQL + Figma)
- `database_manager.py`: Modul pengelola & migrasi Dual Database (SQLite3 Relasional + NoSQL Realtime Database JSON Tree / Cloud Firebase RTDB)
- `goto_pulse_sql.db`: Database SQL Relasional (6 tabel terindeks hasil olahan CSV)
- `goto_pulse_nosql_realtime.json`: Database NoSQL / Realtime Database (data menu Figma, *executive summary*, *critical alerts*, dan *live prediction stream*)
- `FIGMA_UI_DAN_NOSQL_MENU_SPEC.md`: Blueprint desain antarmuka Figma & spesifikasi pemetaan data menu NoSQL
- `BAB_II_KAJIAN_PUSTAKA_UPDATED.md`: Dokumen Bab II Proposal yang telah dilengkapi landasan teori SQL, NoSQL Realtime Database, dan Figma
- `goto_pulse_processed.csv`: Data review yang telah diproses
- `tokopedia_reviews.csv`: Sampel data ulasan Tokopedia
- Dokumen presentasi & proposal seminar proposal (Sempro).

> **Catatan:** File `gojek_reviews.csv` tidak disertakan di repositori karena ukurannya melebihi batas upload GitHub (>100 MB).
