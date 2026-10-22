# 📘 Panduan Belajar Kilat — Analisis Multivariat

Cakupan: Pert. 1 (Pendahuluan & Matriks) · Pert. 2 (Normal Multivariat) · Pert. 3 (Inferensi Vektor Nilai Tengah) · Pert. 4 (MANOVA one-way) · Pert. 5 (MANOVA two-way) · Pert. 6 (Analisis Profil) · Pert. 7 (Analisis Faktor)

> **Cara pakai:** baca bagian **"🎯 Tips kertas jawaban"** di tiap materi dulu, lalu rumus. Bagian itu berisi apa yang **selalu ditulis** di lembar jawaban agar tetap dapat poin parsial (hipotesis, statistik uji, df, aturan keputusan, kesimpulan) walau hitungan tidak selesai.
> Catatan: file ini ringkasan dari slide. Rumus yang di slide berupa gambar saya tulis ulang dari rumus standar Johnson & Wichern. Cocokkan dengan slide bila ada beda notasi.

---

## 🔥 0. Prioritas & Strategi Umum (baca ini kalau waktu sangat mepet)

**Urutan prioritas belajar (peluang keluar × kemudahan poin):**

1. **MANOVA (Wilks' Lambda, tabel B/W/T)** → soal contoh obat 2 kelompok bisa dihitung tangan (ada di bagian 4).
2. **Analisis Profil (3 uji: paralel → berhimpit → level)** → urutan & hipotesisnya hafalan, mudah dapat poin.
3. **T² Hotelling (1 sampel & 2 sampel)** → rumus pendek, hafalkan.
4. **Analisis Faktor** → model, asumsi, Σ = LL' + Ψ, komunalitas, kriteria jumlah faktor.
5. **Normal multivariat** → rumus pdf, jarak Mahalanobis, chi-square plot.
6. **Aljabar matriks / statistik deskriptif** → rumus S, R, V^½.

**Template jawaban 5 baris (pakai untuk SEMUA soal uji hipotesis):**

```
1. Hipotesis      : H0: ...   vs   H1: ...
2. Statistik uji  : (tulis rumus, walau angka belum dihitung)
3. Titik kritis   : distribusi F/χ² dengan df = ...  pada α = ...
4. Keputusan      : Tolak H0 jika ...  → (tulis tolak/terima)
5. Kesimpulan     : kalimat dalam konteks soal (bukan cuma "tolak H0")
```

**Trik poin parsial:**
- Selalu tulis **diketahui** (n, p, g, b, α) di awal. Dosen sering memberi poin untuk identifikasi parameter.
- Tulis **df dengan benar** (sering jadi poin tersendiri).
- Kalau hitungan buntu, **tetap tulis keputusan bersyarat**: *"Jika T² > c² maka tolak H0; ..."* + interpretasinya.
- Kalau soal minta asumsi → tulis asumsi walau tidak diuji.
- Tulis **kesimpulan dalam kalimat konteks** (mis. "profil lansia pikun dan tidak pikun tidak sejajar").

---

## 1️⃣ PERTEMUAN 1 — Pendahuluan & Review Aljabar Matriks

### Konsep inti
- **Analisis multivariat**: analisis data dengan banyak variabel (IV/DV) sekaligus.
- **Dependensi** (ada IV vs DV jelas): ANOVA/MANOVA, regresi, diskriminan.
- **Interdependensi** (semua variabel setara, eksplorasi): analisis faktor, klaster, MDS.
- **5 Tujuan**: (1) reduksi data (PCA, faktor) (2) pengelompokan (klaster, diskriminan) (3) ketergantungan antar variabel (regresi multivariat, CCA, MANOVA) (4) prediksi (5) pembentukan & uji hipotesis.

### Notasi data
- `p` = jumlah variabel, `n` = jumlah objek. `x_jk` = variabel ke-k pada objek ke-j. Matriks **X** berukuran **n × p** (baris = objek, kolom = variabel).

### Statistik deskriptif multivariat

| Besaran | Rumus |
|---|---|
| Rata-rata | x̄_k = (1/n) Σ_j x_jk → vektor x̄ (p×1) |
| Varians | s_kk = (1/n) Σ (x_jk − x̄_k)² *(slide: 1/n; tabel perbandingan: 1/(n−1))* |
| Kovarians | s_ik = (1/n) Σ (x_ji − x̄_i)(x_jk − x̄_k) |
| Korelasi | r_ik = s_ik / (√s_ii · √s_kk) |
| Matriks kovarians | S (p×p, simetris) |
| Matriks korelasi | R (diagonal = 1) |

⚠️ **Perhatikan pembagi** (n vs n−1) sesuai yang diminta soal. Tulis pembaginya di jawaban.

### Contoh toko buku (hafal hasilnya, sering jadi soal)
Data: V1 = (42, 52, 48, 58), V2 = (4, 5, 4, 3)
- x̄ = [50, 4]'
- S_n = [[34, −1.5], [−1.5, 0.5]]  (pembagi n = 4)
- r₁₂ = −1.5 / (√34 · √0.5) = **−0.36**

### Matriks acak & vektor rata-rata
- E(X) = matriks nilai harapan tiap elemen. Sifat: **E(X+Y) = E(X)+E(Y)**, **E(AXB) = A E(X) B**.
- Diskrit: E(X) = Σ x·p(x).
- **Σ = E[(X−μ)(X−μ)']** → diagonal = varians σ_ii, off-diagonal = kovarians σ_ik.
- Contoh slide: μ = [0.1, 0.2]', Σ = [[0.69, −0.08], [−0.08, 0.16]].

### Hubungan Σ dan ρ
- **V^½** = matriks diagonal berisi simpangan baku (√σ_ii).
- **Σ = V^½ ρ V^½**  dan  **ρ = (V^½)⁻¹ Σ (V^½)⁻¹**
- Contoh: Σ = [[4,1,2],[1,9,−3],[2,−3,25]] → V^½ = diag(2,3,5) → ρ₁₂ = 1/6, ρ₁₃ = 1/5, ρ₂₃ = −1/5.

### Latihan perkalian matriks (slide)
A (2×3) = [[3,−1,2],[1,5,4]], B (3×1) = [−2, 7, 9]', C (2×2) = [[2,0],[1,−1]]
- **AB** = [3(−2)+(−1)(7)+2(9), 1(−2)+5(7)+4(9)]' = **[5, 69]'**
- **BC tidak terdefinisi** (3×1 dikali 2×2: kolom B ≠ baris C). Ini jebakan!

### 🎯 Tips kertas jawaban (Pert. 1)
- Tulis: **S = (1/(n−1)) Σ (x_j − x̄)(x_j − x̄)'**, **R_ik = s_ik/√(s_ii s_kk)**, **ρ = (V^½)⁻¹ Σ (V^½)⁻¹**.
- Sebelum mengalikan matriks, **cek ukuran dulu** (a×b)(b×c) = (a×c). Tulis ukuran di jawaban.
- Σ dan S selalu **simetris**; R diagonalnya 1.

---

## 2️⃣ PERTEMUAN 2 — Sebaran Normal Multivariat

### Konsep
- Normal multivariat dasar banyak metode (MANOVA, T², faktor). Data nyata tak selalu normal, tapi dipakai sebagai pendekatan + **Central Limit Theorem** (rata-rata sampel besar n ≥ 30 mendekati normal).
- **Univariat**: X ~ N(μ, σ²), f(x) = (1/√(2πσ²)) exp[−((x−μ)/σ)²/2]. P(μ±σ) ≈ 0.68; P(μ±2σ) ≈ 0.95 (3σ ≈ 0.997).

### Dari univariat ke multivariat
- Jarak kuadrat terstandar univariat: ((x−μ)/σ)² = (x−μ)(σ²)⁻¹(x−μ)
- **Jarak Mahalanobis / jarak tergeneralisasi**: **(x − μ)' Σ⁻¹ (x − μ)**  (x, μ: p×1; Σ: p×p simetris & *positive definite*)
- **PDF normal multivariat** N_p(μ, Σ):

```
f(x) = 1 / [ (2π)^(p/2) · |Σ|^(1/2) ] · exp[ −(x−μ)' Σ⁻¹ (x−μ) / 2 ]
```

### 5 Sifat normal multivariat (HAFAL, sering jadi soal teori)
1. **Kombinasi linear**: a'X ~ N(a'μ, a'Σa). X normal multivariat ⇔ setiap kombinasi linearnya normal univariat.
2. **Subset**: sebagian komponen X tetap normal (ambil μ dan Σ yang bersesuaian). Marginal X_i ~ N(μ_i, σ_ii).
3. **Kovarians nol ⇒ saling bebas** (khusus normal multivariat). Σ₁₂ = 0 ⇔ X₁ dan X₂ independen. *(Umumnya hanya: bebas ⇒ kov = 0.)*
4. **Distribusi bersyarat**: X₁ | X₂ = x₂ normal dengan
   - Mean = μ₁ + Σ₁₂ Σ₂₂⁻¹ (x₂ − μ₂)
   - Kovarians = Σ₁₁ − Σ₁₂ Σ₂₂⁻¹ Σ₂₁ *(tidak bergantung pada x₂)*
5. **Mahalanobis ~ χ²_p**: (x−μ)' Σ⁻¹ (x−μ) ~ χ²_p. (Univariat: ((X−μ)/σ)² ~ χ²₁.)

### Uji normalitas multivariat: Chi-Square Plot
1. Hitung d_j² = (x_j − x̄)' S⁻¹ (x_j − x̄), urutkan: d²₍₁₎ ≤ … ≤ d²₍ₙ₎
2. Plot pasangan **( q_{c,p}((j − ½)/n) , d²₍ⱼ₎ )**, q = kuantil χ² dengan df = p
3. **Normal** → titik membentuk garis lurus lewat (0,0) slope 1. **Melengkung** → tidak normal. **Titik jauh di kanan atas** → outlier.

### Deteksi outlier (4 langkah)
1. Dot plot tiap variabel. 2. Scatter plot tiap pasangan variabel. 3. Nilai z: z_jk = (x_jk − x̄_k)/√s_kk; **|z| > 3.5** → outlier univariat (sampel sedang). 4. Hitung **d_j²** dan bandingkan dengan persentil atas χ²_p.
- Outlier belum tentu salah data (bisa fenomena baru). Titik bisa jadi outlier multivariat walau tiap variabel tampak normal.

### Tugas slide (4.2) — jawaban
Diketahui: μ₁=0, μ₂=2, σ₁₁=2, σ₂₂=1, ρ₁₂=0.5
- σ₁₂ = ρ √(σ₁₁σ₂₂) = 0.5 √2 = **0.7071**
- Σ = [[2, 0.7071],[0.7071, 1]], |Σ| = 2·1 − 0.5 = **1.5**
- Σ⁻¹ = (1/1.5) [[1, −0.7071],[−0.7071, 2]]
- **(a)** f(x₁,x₂) = 1/(2π√1.5) · exp(−d²/2) ≈ 0.1299 · exp(−d²/2)
- **(b)** d² = (2/3)[ x₁² − 1.4142 x₁(x₂−2) + 2(x₂−2)² ] = **(2/3)x₁² − 0.9428 x₁(x₂−2) + (4/3)(x₂−2)²**

### 🎯 Tips kertas jawaban (Pert. 2)
- Tulis rumus pdf dan d² **dulu** (poin besar), baru substitusi angka.
- Kalau ditanya sifat: tulis **5 nama sifat** + satu kalimat masing-masing.
- Chi-square plot: tulis **3 langkah + kriteria garis lurus slope 1**.
- Selalu cari **|Σ|** dan **Σ⁻¹** untuk 2×2: Σ⁻¹ = (1/|Σ|)[[σ₂₂, −σ₁₂],[−σ₁₂, σ₁₁]].

---

## 3️⃣ PERTEMUAN 3 — Inferensi Vektor Nilai Tengah

### Perbandingan univariat vs multivariat

| | Univariat | Multivariat |
|---|---|---|
| Nilai tengah | skalar μ | vektor μ (p×1) |
| Selang kepercayaan | selang | **elipsoid** (daerah kepercayaan) |
| 1 populasi | uji t | **T² Hotelling** |
| 2 populasi | uji t | **T² Hotelling** |
| Banyak populasi | ANOVA | **MANOVA** |

### A. Satu populasi — H₀: μ = μ₀ vs H₁: μ ≠ μ₀
- Review univariat: t = (x̄ − μ₀)/(s/√n) ~ t_{n−1}; tolak jika |t| > t_{n−1}(α/2) (⇔ t² besar).
- **T² Hotelling**:

```
T² = n (x̄ − μ₀)' S⁻¹ (x̄ − μ₀)
```
- **Daerah penolakan** (eksak): tolak H₀ jika
```
T² > c² = [ (n−1) p / (n−p) ] · F_{p, n−p}(α)
```
  (F dengan df **p** dan **n − p**). Sampel besar: T² ≈ χ²_p.
- **Wilks' Lambda (likelihood ratio)**: Λ = |Σ(x_j−x̄)(x_j−x̄)'| / |Σ(x_j−μ₀)(x_j−μ₀)'|, hubungan: **Λ^(−2/n) = 1 + T²/(n−1)**.
- **Contoh slide**: T² = 9.74 > 8.18 → **tolak H₀ pada α = 10%**.

### B. Setelah H₀ ditolak → uji lanjut (3 pilihan)
1. **Daerah kepercayaan simultan (elips)**: semua μ dengan
   `n (x̄ − μ)' S⁻¹ (x̄ − μ) ≤ c²`
   - Sumbu elips: arah **e_i** (vektor eigen S), setengah panjang sumbu = **√λ_i · c/√n**
   - Langkah: (1) cari akar ciri λ & vektor eigen e dari S (2) hitung panjang sumbu (3) gambar elips berpusat di x̄.
2. **Selang kepercayaan simultan (T²)**: x̄_i ± √c² · √(s_ii/n)  → *lebih lebar, aman untuk semua kombinasi linear*.
3. **Selang Bonferroni**: x̄_i ± t_{n−1}(α/(2p)) · √(s_ii/n)  → *lebih sempit jika hanya sedikit komponen diuji*.

### C. Dua populasi independen — H₀: μ₁ = μ₂
Syarat: sampel acak, **saling bebas**, normal multivariat.

**Kasus ragam sama (Σ₁ = Σ₂):**
```
S_pooled = [(n₁−1)S₁ + (n₂−1)S₂] / (n₁+n₂−2)

T² = (x̄₁ − x̄₂)' [ (1/n₁ + 1/n₂) S_pooled ]⁻¹ (x̄₁ − x̄₂)

Tolak H₀ jika T² > c² = [ (n₁+n₂−2) p / (n₁+n₂−p−1) ] · F_{p, n₁+n₂−p−1}(α)
```

**Kasus ragam tidak sama (Σ₁ ≠ Σ₂):**
```
T² = (x̄₁ − x̄₂)' [ S₁/n₁ + S₂/n₂ ]⁻¹ (x̄₁ − x̄₂)
Sampel besar: tolak H₀ jika T² > χ²_p(α)   (versi sampel kecil: pendekatan F dengan df ν yang dihitung dari S₁, S₂)
```

**Contoh kura-kura (n₁ = n₂ = 24, p = 2)**: ragam sama → T² = 4.995 > c² = 4.988 → **tolak H₀ pada α = 1%**. Ragam tidak sama → tolak H₀ pada α = 5%.

### 🎯 Tips kertas jawaban (Pert. 3)
- **Hafalkan 3 rumus c²** (satu sampel, dua sampel sama, χ² untuk tidak sama). Tulis walau tidak sempat hitung.
- Pembeda df: satu sampel `(p, n−p)`; dua sampel `(p, n₁+n₂−p−1)`.
- Selalu tulis: *"T² besar ⇒ x̄ jauh dari μ₀ ⇒ tolak H₀"*.
- Bila H₀ ditolak, tulis **"perlu uji lanjut: elips kepercayaan / selang simultan / Bonferroni"** (poin konsep).
- Gambar elips: **pusat x̄, sumbu searah e_i, panjang √λ_i·c/√n**.

---

## 4️⃣ PERTEMUAN 4 — MANOVA (One-Way)

### Konsep
- **MANOVA** = perluasan ANOVA untuk **≥ 2 DV (metrik) sekaligus**, IV **kategorik** (kelompok). Dikembangkan **S.S. Wilks (1932)**.
- Menguji apakah **vektor rata-rata** antar kelompok sama (bukan satu angka tunggal).
- **Mengapa pakai MANOVA, bukan banyak ANOVA?** (1) mengendalikan **kesalahan tipe I (α)** (2) mempertimbangkan **korelasi antar DV** (ANOVA menganggap respons saling bebas).
- **Dua kondisi pemakaian**: (1) DV berkorelasi dan ingin 1 tes menyeluruh (2) ingin tahu pengaruh IV terhadap *pola* DV.
- **Tipe**: one-way (≥2 DV, 1 IV), two-way (≥2 DV, 2 IV), multiway.
- **Alur setelah H₀ ditolak**: ANOVA tiap DV → bila signifikan, **post hoc Scheffé**.

### Asumsi (6 poin, HAFAL)
1. DV kontinu (interval/rasio), IV kategorik (1 atau 2 faktor)
2. **Linearitas** antar DV (korelasi → uji Pearson)
3. **Normalitas multivariat** (skewness & kurtosis)
4. **Independensi** pengamatan
5. **Sampel acak**
6. **Homogenitas matriks varians-kovarians** (Levene's test & **Box's M**)

### Model & hipotesis
```
x_ℓj = μ + τ_ℓ + e_ℓj ;  j = 1..n_ℓ, ℓ = 1..g ;  e ~ N_p(0, Σ) independen
Σ n_ℓ τ_ℓ = 0
H0: τ₁ = τ₂ = … = τ_g = 0       H1: minimal satu τ_ℓ ≠ 0
```
(μ = rata-rata total, τ_ℓ = efek perlakuan ke-ℓ)

### Tabel MANOVA one-way

| Sumber | Matriks SSP | df |
|---|---|---|
| Perlakuan (**B**) | Σ n_ℓ (x̄_ℓ − x̄)(x̄_ℓ − x̄)' | g − 1 |
| Residual (**W**) | Σ Σ (x_ℓj − x̄_ℓ)(x_ℓj − x̄_ℓ)' | Σ n_ℓ − g |
| Total (**B+W**) | Σ Σ (x_ℓj − x̄)(x_ℓj − x̄)' | Σ n_ℓ − 1 |

### Wilks' Lambda
```
Λ* = |W| / |B + W|        (kecil ⇒ tolak H0)
```
**Transformasi ke F (eksak) — hafal tabel ini:**

| p | g | Statistik ~ F |
|---|---|---|
| 1 | ≥2 | [(Σn−g)/(g−1)] · [(1−Λ*)/Λ*] ~ F(g−1, Σn−g) |
| 2 | ≥2 | [(Σn−g−1)/(g−1)] · [(1−√Λ*)/√Λ*] ~ F(2(g−1), 2(Σn−g−1)) |
| ≥1 | 2 | [(Σn−p−1)/p] · [(1−Λ*)/Λ*] ~ F(p, Σn−p−1) |
| ≥1 | 3 | [(Σn−p−2)/p] · [(1−√Λ*)/√Λ*] ~ F(2p, 2(Σn−p−2)) |

Sampel besar (Bartlett): −[n − 1 − (p+g)/2] ln Λ* ~ χ² dengan df = p(g−1).

### Pilihan uji statistik MANOVA

| Uji | Paling cocok |
|---|---|
| **Wilks' Lambda** | kondisi ideal/standar, asumsi terpenuhi, sampel seimbang |
| **Pillai's Trace** | asumsi **terlanggar**, sampel kecil, ukuran kelompok tak seimbang (paling robust) |
| **Hotelling's Trace** | hanya 2 kelompok / beda terkonsentrasi di satu dimensi |
| **Roy's Largest Root** | ingin efek pembeda maksimum (satu eigenvalue dominan) |

### ✍️ CONTOH SOAL SLIDE — dikerjakan lengkap (obat tekanan darah)
Data (Y1 sistolik, Y2 diastolik), 2 ulangan:
- Obat 1: ulangan 1 = (1, 2), ulangan 2 = (3, 4)
- Obat 2: ulangan 1 = (4, 6), ulangan 2 = (4, 4)

**1) Vektor rataan**
- x̄₁ = [2, 3]', x̄₂ = [4, 5]', x̄ (total) = [3, 4]'

**2) Matriks B, W, T**
- B = 2·(−1,−1)(−1,−1)' + 2·(1,1)(1,1)' = **[[4, 4],[4, 4]]**
- W grup 1: deviasi (−1,−1), (1,1) → [[2,2],[2,2]]; grup 2: deviasi (0,1), (0,−1) → [[0,0],[0,2]]
  → **W = [[2, 2],[2, 4]]**
- **T = B + W = [[6, 6],[6, 8]]**

| Sumber | SSP | df |
|---|---|---|
| Perlakuan | B = [[4,4],[4,4]] | g−1 = 1 |
| Residual | W = [[2,2],[2,4]] | Σn−g = 2 |
| Total | T = [[6,6],[6,8]] | Σn−1 = 3 |

**3) Uji Wilks (α = 5%)**
- H₀: τ₁ = τ₂ = 0 (vektor rata-rata kedua obat sama) vs H₁: minimal ada τ ≠ 0
- |W| = 2·4 − 2·2 = **4**; |T| = 6·8 − 6·6 = **12** → **Λ* = 4/12 = 1/3**
- p = 2, g = 2 → pakai baris "p ≥ 1, g = 2": F = [(4−2−1)/2]·[(1−1/3)/(1/3)] = 0.5 × 2 = **1**, df = (2, 1)
- F tabel (2,1; 0.05) = **199.5** → 1 < 199.5 → **terima H₀**
- **Kesimpulan:** pada α = 5%, tidak cukup bukti bahwa kedua jenis obat memberi respons (penurunan sistolik & diastolik) yang berbeda. *(Catatan: sampel sangat kecil, kuasa uji rendah.)*

### 🎯 Tips kertas jawaban (Pert. 4)
- **Wajib tulis**: `Λ* = |W|/|B+W|`, rumus B, W, df tabel, "Tolak H₀ jika Λ* kecil / F hitung > F tabel".
- Hitung **vektor rata-rata dulu** (x̄ tiap grup & x̄ total) → sudah dapat poin walau B, W belum selesai.
- Determinan 2×2: |A| = ad − bc. Tulis langkahnya.
- Cek: **B + W = T**. Kalau tidak cocok, ada salah hitung (bisa untuk verifikasi cepat).
- Pemilihan uji: *"asumsi dilanggar → Pillai's trace"*.
- Tulis alur lengkap: **MANOVA → (tolak) → ANOVA tiap DV → post hoc Scheffé**.
- Tulis 6 asumsi sebagai daftar singkat.

---

## 5️⃣ PERTEMUAN 5 — MANOVA Two-Way

### Model
```
X_ℓkr = μ + τ_ℓ + β_k + γ_ℓk + e_ℓkr
ℓ = 1..g (faktor 1) ; k = 1..b (faktor 2) ; r = 1..n (ulangan)
Σ τ_ℓ = Σ β_k = Σ_ℓ γ_ℓk = Σ_k γ_ℓk = 0 ; semua vektor p×1 ; e ~ NIID
```
μ = rata-rata keseluruhan · τ_ℓ = efek faktor 1 · β_k = efek faktor 2 · γ_ℓk = efek interaksi.

### Hipotesis
- **Interaksi**: H₀: γ₁₁ = γ₁₂ = … = γ_gb = 0 (tidak ada interaksi) vs H₁: minimal satu γ ≠ 0
- **Faktor 1**: H₀: τ₁ = … = τ_g = 0
- **Faktor 2**: H₀: β₁ = … = β_b = 0

### Tabel MANOVA two-way

| Sumber | SSP | df |
|---|---|---|
| Faktor 1 | Σ b·n (x̄_ℓ· − x̄)(x̄_ℓ· − x̄)' | g − 1 |
| Faktor 2 | Σ g·n (x̄_·k − x̄)(x̄_·k − x̄)' | b − 1 |
| Interaksi | Σ Σ n (x̄_ℓk − x̄_ℓ· − x̄_·k + x̄)(…)' | (g−1)(b−1) |
| Residual | Σ Σ Σ (x_ℓkr − x̄_ℓk)(x_ℓkr − x̄_ℓk)' | gb(n−1) |
| Total | Σ Σ Σ (x_ℓkr − x̄)(x_ℓkr − x̄)' | gbn − 1 |

### Statistik uji (Wilks)
- **Interaksi**: Λ* = |SSP_res| / |SSP_int + SSP_res|
- **Faktor 1**: Λ₁* = |SSP_res| / |SSP_fac1 + SSP_res|
- **Faktor 2**: Λ₂* = |SSP_res| / |SSP_fac2 + SSP_res|

F untuk interaksi (bila (g−1)(b−1) = 1):
```
F = [(1−Λ*)/Λ*] · [ (gb(n−1) − p + 1)/2 ] / [ (|(g−1)(b−1) − p| + 1)/2 ]
df:  ν₁ = |(g−1)(b−1) − p| + 1 ,   ν₂ = gb(n−1) − p + 1
```
F faktor 1 dan 2: sama, hanya ganti (g−1)(b−1) dengan **(g−1)** atau **(b−1)**, ν₁ = |(g−1) − p| + 1 atau |(b−1) − p| + 1, ν₂ tetap gb(n−1) − p + 1.
**Tolak H₀ jika F hitung > F tabel.**
Sampel besar interaksi (Bartlett): −[gb(n−1) − (p + 1 − (g−1)(b−1))/2] ln Λ* ~ χ²_{(g−1)(b−1)p}.

### ✍️ Contoh slide: data film plastik (g = b = 2, n = 5, p = 3)
Respons: X₁ tahan sobek, X₂ kilap, X₃ kekeruhan (opacity). Faktor 1 = laju ekstrusi, Faktor 2 = jumlah aditif.
- **Interaksi:** Λ* = 0.7771 → F = 1.34, df (3, 14), F tabel = 3.34 → 1.34 < 3.34 → **terima H₀: tidak ada interaksi** (chi-square 3.66 < 7.81 → kesimpulan sama).
- **Faktor 1:** Λ₁* = 275.7098/722.0212 = 0.3819 → F₁ = **7.55** > 3.34 → **tolak H₀** (ada efek faktor 1).
- **Faktor 2:** Λ₂* = 275.7098/527.1347 = 0.5230 → F₂ = **4.26** > 3.34 → **tolak H₀** (ada efek faktor 2).
- **Kesimpulan:** laju ekstrusi dan jumlah aditif **sama-sama berpengaruh** pada respons, secara **aditif** (tanpa interaksi).

### 🎯 Tips kertas jawaban (Pert. 5)
- **Urutan wajib: uji INTERAKSI dulu.** Jika interaksi signifikan, efek utama sulit ditafsirkan sendiri.
- Hafal **df tabel**: g−1, b−1, (g−1)(b−1), gb(n−1), gbn−1.
- Tulis 3 set hipotesis (faktor 1, faktor 2, interaksi) + 3 rumus Λ*.
- Pola jawaban sama untuk ketiganya → kerjakan satu dengan lengkap, yang lain tinggal ganti SSP.
- Kesimpulan akhir: sebut *"aditif"* jika interaksi tidak signifikan.

---

## 6️⃣ PERTEMUAN 6 — Analisis Profil (Profile Analysis)

### Konsep
- Dipakai ketika **p perlakuan (variabel) diukur pada ≥ 2 kelompok** subjek, dan dibandingkan **profil** (grafik rata-rata tiap variabel per kelompok). Menurut Morrison: uji hipotesis nilai tengah multivariat berbasis grafik.
- **Asumsi**: (1) semua respons dalam **satuan yang sama** (2) respons antar grup **saling bebas**.

### 3 Tahap pengujian — URUTAN HARUS DIHAFAL
```
Tahap 1: PARALEL?  → jika H0 diterima →
Tahap 2: BERHIMPIT (coincident)?  → jika H0 diterima →
Tahap 3: LEVEL / HORIZONTAL (rata)?
```
**Matriks kontras C** ((p−1) × p): baris = [−1 1 0 … 0], [0 −1 1 … 0], …, [0 … −1 1]

#### Tahap 1 — Uji kesejajaran (paralel)
- H₀₁: **Cμ₁ = Cμ₂** (selisih antar variabel berurutan sama di kedua grup) vs H₁: Cμ₁ ≠ Cμ₂
```
T² = (x̄₁−x̄₂)' C' [ (1/n₁ + 1/n₂) C S_pooled C' ]⁻¹ C (x̄₁−x̄₂)
c² = [ (n₁+n₂−2)(p−1) / (n₁+n₂−p) ] · F_{p−1, n₁+n₂−p}(α)
Tolak H₀ jika T² > c²
```
- S_pooled = [(n₁−1)S₁ + (n₂−1)S₂]/(n₁+n₂−2)
- Arti jika **tidak ditolak**: peningkatan/penurunan skor antar sub-tes **sama** di kedua kelompok.

#### Tahap 2 — Uji keberhimpitan (coincident), *dengan syarat paralel*
- H₀₂: **1'μ₁ = 1'μ₂** (jumlah rata-rata sama) vs H₁₂: 1'μ₁ ≠ 1'μ₂
```
T² = [ 1'(x̄₁−x̄₂) / √( (1/n₁ + 1/n₂) · 1' S_pooled 1 ) ]²
Tolak H₀ jika T² > t²_{n₁+n₂−2}(α/2) = F_{1, n₁+n₂−2}(α)
```
- Jika diterima → kedua kelompok berasal dari **populasi yang sama**.

#### Tahap 3 — Uji level/kesamaan (horizontal), *dengan syarat berhimpit*
- H₀₃: **Cμ = 0** (semua rata-rata konstan) vs H₁₃: Cμ ≠ 0
```
T² = (n₁+n₂) x̄' C' [ C S C' ]⁻¹ C x̄      (x̄ = rata-rata gabungan n₁+n₂, S = kovarians gabungan semua pengamatan)
c² = [ (n₁+n₂−1)(p−1) / (n₁+n₂−p+1) ] · F_{p−1, n₁+n₂−p+1}(α)
Tolak H₀ jika T² > c²
```

### ✍️ Contoh slide: lansia (pikun vs tidak pikun, n₁ = 37 tidak pikun, n₂ = 12 pikun, p = 4 sub-tes: informasi, similaritas, aritmetik, gambar)
- x̄₁ (tidak pikun) = [12.57, 9.49, 11.51, 7.97]', x̄₂ (pikun) = [8.75, 5.33, 8.51, 4.75]'
- Eksplorasi grafik: kedua profil tampak **sejajar tapi tidak berhimpit**, skor tidak pikun lebih tinggi.

| Uji | T² | c² | Keputusan | Makna |
|---|---|---|---|---|
| Paralel | 1.22 | 8.81 | **Terima H₀** | profil sejajar |
| Berhimpit | 17.44 | 4.05 (F₁,₄₇) | **Tolak H₀** | profil tidak berhimpit (skor tidak pikun lebih tinggi) |
| Level | 165.74 | 4.13* | **Tolak H₀** | rata-rata antar sub-tes tidak konstan |

*⚠️ c² level di slide dihitung dengan p = 2; dengan p = 4 yang benar nilainya ≈ 8.8. Keputusan tetap **tolak H₀** karena 165.74 jauh lebih besar.*

**Kesimpulan akhir:** profil kedua kelompok **sejajar**, **tidak berhimpit**, dan **tidak level** → pola naik-turun antar sub-tes sama, tetapi tingkat skor kelompok tidak pikun lebih tinggi.

### 🎯 Tips kertas jawaban (Pert. 6)
- **Tulis urutan 3 tahap** (paralel → berhimpit → level) + hipotesis masing-masing. Poin aman.
- **Definisi lisan**: *paralel = selisih antar variabel sama; berhimpit = profil menumpuk (total sama); level = garis datar (semua rata-rata sama).*
- Tulis matriks kontras C **dengan ukuran (p−1)×p** sesuai p di soal.
- Hafal 3 df: `(p−1, n₁+n₂−p)`, `(1, n₁+n₂−2)`, `(p−1, n₁+n₂−p+1)`.
- Kalau tidak sempat menghitung, **gambar profil rata-rata** (sumbu x = variabel, y = rata-rata, 2 garis) dan tulis interpretasi visualnya.
- Aturan lanjut: **hanya lanjut ke tahap berikutnya kalau H₀ tahap sebelumnya diterima.**

---

## 7️⃣ PERTEMUAN 7 — Analisis Faktor

### Konsep
- Teknik **interdependensi**; perluasan analisis komponen utama (PCA). **Tujuan**: reduksi variabel dengan mengelompokkan variabel berkorelasi tinggi menjadi **faktor bersama (common factors)**.
- **Faktor** = variabel *laten/unobservable/konstruk*. **X** = variabel *manifest/observable/indikator*.
- Syarat reduksi: jumlah faktor **m < p** dan sebagian besar ragam X tersimpan di faktor.
- **EFA** (Eksploratori): struktur faktor belum diketahui, dicari dari data. **CFA** (Konfirmatori): menguji struktur yang sudah ditentukan teori.

### Model faktor ortogonal
```
X − μ = L F + ε
```
- X (p×1) ~ N_p(μ, Σ); **F** = m faktor bersama; **ε** = p faktor spesifik (error); **L** (p×m) = matriks **factor loading**, l_ij = loading variabel ke-i pada faktor ke-j.
- **Asumsi**: E(F) = 0, Cov(F) = **I**; E(ε) = 0, Cov(ε) = **Ψ** (diagonal); **F dan ε independen** (Cov(ε,F) = 0).

### Struktur kovarians (INTI, sering keluar)
```
Σ = L L' + Ψ
Var(X_i)   = l_i1² + … + l_im² + ψ_i  =  h_i² + ψ_i
Cov(X_i,X_k) = l_i1 l_k1 + … + l_im l_km
Cov(X, F) = L
```
- **Komunalitas h_i² = Σ_j l_ij²** = bagian ragam X_i yang dijelaskan faktor bersama.
- **Ragam spesifik ψ_i** = s_ii − h_i² (bagian yang tidak dijelaskan).

### Jumlah parameter (m vs p)
- m = p → Σ = LL', Ψ bisa nol (tidak ada penyederhanaan).
- m < p → model lebih sederhana. Contoh slide: p = 12, m = 2: elemen Σ = p(p+1)/2 = 78; parameter l_ij & ψ_i = mp + p = 36.
- **Rotasi**: untuk T orthogonal (TT' = T'T = I), L* = LT menghasilkan Σ yang sama → **keambiguan loading** ⇒ dasar **rotasi faktor** (mis. varimax) untuk mempermudah interpretasi.

### Metode estimasi
Syarat awal: jika korelasi antar variabel mendekati nol, analisis faktor **tidak perlu**.

**1) Metode komponen utama (principal component)**
- Eigen Σ (atau S/R): (λ_i, e_i), λ₁ ≥ λ₂ ≥ … ≥ λ_p ≥ 0.
- Loading kolom ke-j = **√λ_j · e_j** → L̂ = [√λ₁e₁ … √λ_m e_m]
- Ψ̂_i = s_ii − Σ_j l̂_ij²
- Jika satuan berbeda → **standarisasi** (pakai matriks korelasi **R**; Z standar, Cov(Z) = R).
- **Kontribusi faktor ke-j** terhadap total ragam: **λ_j / tr(S)** (jika pakai R: **λ_j / p**).
- **Jumlah faktor m**: pilih eigenvalue **> 1** (kriteria Kaiser, pada R), atau proporsi kumulatif ragam cukup besar (mis. ≥ 70–80%), atau scree plot.

**2) Maximum Likelihood (MLE)**
- Memaksimumkan fungsi likelihood. Asumsi: **faktor bersama dan spesifik normal ⇒ X normal**.
- Hasil: L̂ dan Ψ̂; komunalitas ĥ_i² = Σ_j l̂_ij².
- Proporsi faktor ke-j = Σ_i l̂_ij² / (total ragam).

**Memilih metode terbaik:** bandingkan **matriks residual** = S − (L̂L̂' + Ψ̂). **Metode dengan elemen residual lebih kecil lebih baik.** (Slide: MLE residual lebih kecil daripada komponen utama.)

### ✍️ Contoh slide: penilaian konsumen (7 skala semantik, matriks korelasi)
- Pengelompokan: variabel 1 & 3 satu grup, variabel 2 & 5 satu grup, variabel 4 lebih dekat ke (2,5).
- Eigenvalue R > 1: **λ₁ = 2.85309, λ₂ = 1.80633** → **m = 2**, proporsi kumulatif = (2.853 + 1.806)/p.
- Langkah: (1) cari eigen R (2) tentukan m (Kaiser) (3) hitung loading √λ·e, komunalitas, ragam spesifik (4) hitung residual.

### Contoh Johnson (harga saham)
- F1 = **faktor pasar (market factor)** karena semua loading positif.
- F2 = **faktor industri** karena ada loading positif dan negatif (kontras antar industri).

### 🎯 Tips kertas jawaban (Pert. 7)
- Tulis di awal: **X − μ = LF + ε**, **Σ = LL' + Ψ**, asumsi (E(F)=0, Cov(F)=I, Cov(ε)=Ψ diagonal, F ⊥ ε). Poin dasar hampir pasti ada.
- **Komunalitas = jumlah kuadrat loading baris**; **ψ = s_ii − h²** (atau 1 − h² jika pakai R). Bisa dihitung cepat jika loading diberikan.
- **Proporsi ragam faktor = λ_j/p (R)**; kumulatif = jumlah proporsi.
- **Kriteria jumlah faktor**: eigenvalue > 1.
- Beda **EFA vs CFA**, beda **faktor bersama vs spesifik**, beda **manifest vs laten**: tulis 1 kalimat tiap pasangan.
- **Rotasi**: alasannya "L tidak unik (L* = LT), rotasi memudahkan interpretasi".
- Interpretasi loading: cari variabel dengan loading **besar** pada tiap faktor, beri **nama** faktor (market factor, industri factor).

---

## 📝 Ringkasan Kilat 1 Halaman (untuk dibaca sebelum masuk ruang ujian)

| Topik | Hafal ini |
|---|---|
| Normal multivariat | f(x) = 1/[(2π)^(p/2)|Σ|^½] exp(−d²/2); d² ~ χ²_p; 5 sifat |
| T² 1 sampel | n(x̄−μ₀)'S⁻¹(x̄−μ₀); c² = (n−1)p/(n−p) · F(p, n−p) |
| T² 2 sampel | (x̄₁−x̄₂)'[(1/n₁+1/n₂)S_p]⁻¹(x̄₁−x̄₂); c² = (n₁+n₂−2)p/(n₁+n₂−p−1) · F(p, n₁+n₂−p−1) |
| MANOVA 1 arah | B, W, T; Λ = |W|/|B+W|; df g−1, Σn−g, Σn−1 |
| MANOVA 2 arah | uji interaksi dulu; df g−1, b−1, (g−1)(b−1), gb(n−1) |
| Profil | Paralel → Berhimpit → Level; matriks kontras C (p−1)×p |
| Faktor | X−μ = LF+ε; Σ = LL'+Ψ; h² = Σl²; Kaiser λ>1; residual terkecil = metode terbaik |

**Terakhir:** kalau blank total, tulis **hipotesis + rumus statistik uji + aturan penolakan + kesimpulan konteks**. Itu hampir selalu bernilai.

Semangat! 💪
