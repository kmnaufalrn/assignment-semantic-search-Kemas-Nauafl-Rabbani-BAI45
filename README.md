# Semantic Search & LLM Application
 menjawab pertanyaan umum pelanggan e-commerce dengan mencari FAQ yang paling mendekati maksud pertanyaan, lalu meminta Gemini menyusun jawaban berdasarkan FAQ tersebut.

## Tujuan 

Knowledge base yang digunakan adalah `data/ecommerce_faq_knowledge_base.csv`. Isinya 59 FAQ dengan kategori, pertanyaan, dan jawaban. File sumber dibiarkan apa adanya; setiap pasangan pertanyaan dan jawaban dijadikan satu dokumen untuk proses pencarian.

Alurnya dimulai dengan membuat embedding FAQ menggunakan `gemini-embedding-001`. Vektor dinormalisasi dan disimpan dalam FAISS, lalu query pengguna diubah menjadi embedding yang sama untuk mencari FAQ terdekat. Hasil pencarian beserta skor similarity menjadi konteks bagi gemini-3.8-flash.

## Menjalankan notebook

folder project jalankan:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
$env:GEMINI_API_KEY = "API_KEY_ANDA"
```

Setelah itu buka `notebook/starter_notebook_semantic_search.ipynb`, pilih kernel `.venv`, lalu jalankan cell dari atas. Notebook mencari CSV di folder `data/`, baik dijalankan dari root project maupun dari folder notebook.

## Hasil Pengujian

Saya mencoba lima pertanyaan. Untuk pertanyaan tentang batas waktu pembayaran, pencarian menempatkan FAQ 19 di urutan pertama dengan skor `0.866`. Pertanyaan parafrasa tentang mengganti alamat setelah checkout menemukan FAQ 5 dengan skor `0.793`, dan pertanyaan tentang paket yang terlambat menemukan FAQ 12 dengan skor `0.831`. Pertanyaan tentang cicilan kartu kredit juga menemukan FAQ 20, dengan skor `0.823`. Jadi, keempat pertanyaan yang memang punya jawaban di knowledge base mengambil FAQ yang diharapkan sebagai hasil teratas.

Saya juga mencoba bertanya tentang jasa pemasangan perabot, yang tidak dibahas di FAQ. FAISS tetap memberi hasil terdekat, yaitu FAQ 15, tetapi skornya hanya `0.663`. 

