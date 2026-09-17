# Instagram Scraping - Restoran Sehat Indonesia

Project ini mengumpulkan dan membersihkan data akun Instagram restoran/bisnis makanan berkonsep sehat (dengan klaim gizi) di Indonesia.

## Struktur Folder

```
scrapping_instagram/
├── Raw_Dataset/            # Data mentah hasil scraping (CSV per run)
├── Preprocessing_Data/     # Hasil data yang sudah dibersihkan
└── Code/
    └── DatasetProcessing.ipynb   # Notebook untuk proses cleaning
```

## Tools yang Digunakan

- **Apify Instagram Scraper** (`apify/instagram-scraper`) — untuk mengumpulkan data profil Instagram berdasarkan keyword maupun hashtag.

## Metode Pengumpulan Data

Data dikumpulkan melalui dua pendekatan:
1. **Keyword search** — menggunakan fitur "Search by query" di Instagram Scraper (sumber: Google Search & Facebook Ads Library).
2. **Hashtag search** — menelusuri post-post dengan tagar tertentu untuk menemukan akun terkait.

**Keyword yang digunakan:**
- Restoran Sehat
- Healthy food
- Makanan diet sehat
- Gizi seimbang
- Salad bar Indonesia
- Bebas gula tanpa pemanis

## Alur Preprocessing (`DatasetProcessing.ipynb`)

Semua tahap dijalankan berurutan di satu notebook, data diproses di memory dan baru disimpan sekali di akhir.

| Step | Proses |
|------|--------|
| 1 | Load semua CSV dari `Raw_Dataset/` dan gabungkan jadi satu dataframe |
| 2 | Hapus baris yang seluruh kolomnya kosong/NaN |
| 3 | Hapus duplikat berdasarkan `biography` (baris dengan bio kosong tetap disimpan, tidak ikut dedupe) |
| 4 | Hapus akun private dan akun dengan `followersCount = 0` |
| 5 | Pilih kolom yang relevan saja (buang kolom teknis seperti `latestPosts`, `profilePicUrl`, dll) |
| 6 | Filter bahasa (Indonesia/Inggris) via `langdetect`, lalu filter sinyal lokasi Indonesia (nomor telepon, nama kota, bank/kurir lokal, kata kunci Indonesia) |
| 7 | Urutkan berdasarkan `followersCount` (terbesar ke terkecil) |
| 8 | Simpan hasil akhir ke `Preprocessing_Data/combined_final.csv` |

## Output Akhir

File `Preprocessing_Data/combined_final.csv` berisi daftar akun Instagram yang:
- Relevan dengan topik restoran/makanan sehat berklaim gizi
- Berbahasa Indonesia atau Inggris
- Terindikasi berasal dari Indonesia
- Bukan akun private, punya minimal 1 followers
- Tidak ada duplikat

## Cara Menjalankan

1. Pastikan library yang dibutuhkan sudah terinstall:
   ```
   pip install pandas langdetect
   ```
2. Letakkan semua file CSV hasil scraping Apify ke folder `Raw_Dataset/`.
3. Jalankan seluruh cell di `DatasetProcessing.ipynb` secara berurutan.
4. Hasil akhir akan tersimpan otomatis di `Preprocessing_Data/combined_final.csv`.
