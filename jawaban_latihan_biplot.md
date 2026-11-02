# Jawaban Latihan Analisis Biplot

Dikerjakan dengan cara di papan tulis: **|X'X − λI| = 0 → rumus kuadrat (ABC) → eigenvektor dengan OBE → normalisasi → SVD → G dan H → plot → interpretasi**.

---

# SOAL 1 — Biplot Nilai Fisika dan Matematika (α = 0,5)

## Data

| Observasi | X₁ (Fisika) | X₂ (Matematika) |
|---|---|---|
| 1 | 70 | 65 |
| 2 | 75 | 70 |
| 3 | 80 | 75 |
| 4 | 85 | 80 |
| 5 | 90 | 85 |
| 6 | 75 | 80 |
| 7 | 80 | 90 |
| 8 | 70 | 100 |
| 9 | 65 | 85 |
| 10 | 60 | 80 |

n = 10 objek, p = 2 peubah.

## Langkah 1–2. Matriks X\* dan X (dikoreksi rataan)

Rataan:

- X̄₁ = (70+75+80+85+90+75+80+70+65+60)/10 = 750/10 = **75**
- X̄₂ = (65+70+75+80+85+80+90+100+85+80)/10 = 810/10 = **81**

X = X\* − X̄ :

| Obs | X₁ − 75 | X₂ − 81 |
|---|---|---|
| 1 | −5 | −16 |
| 2 | 0 | −11 |
| 3 | 5 | −6 |
| 4 | 10 | −1 |
| 5 | 15 | 4 |
| 6 | 0 | −1 |
| 7 | 5 | 9 |
| 8 | −5 | 19 |
| 9 | −10 | 4 |
| 10 | −15 | −1 |

## Langkah 3. Hitung X'X

- Σx₁² = 25+0+25+100+225+0+25+25+100+225 = **750**
- Σx₂² = 256+121+36+1+16+1+81+361+16+1 = **890**
- Σx₁x₂ = 80+0−30−10+60+0+45−95−40+15 = **25**

$$
X'X=\begin{bmatrix}750 & 25\\ 25 & 890\end{bmatrix}
$$

## Langkah 4. Nilai eigen, |X'X − λI| = 0

$$
\begin{vmatrix}750-\lambda & 25\\ 25 & 890-\lambda\end{vmatrix}=0
$$

(750 − λ)(890 − λ) − (25)² = 0

667.500 − 890λ − 750λ + λ² − 625 = 0

**λ² − 1640λ + 666.875 = 0**  (a = 1, b = −1640, c = 666.875)

$$
\lambda_{1,2}=\frac{-b\pm\sqrt{b^2-4ac}}{2a}=\frac{1640\pm\sqrt{1640^2-4(666875)}}{2}=\frac{1640\pm\sqrt{22100}}{2}=\frac{1640\pm148{,}661}{2}
$$

- **λ₁ = 894,3303**
- **λ₂ = 745,6697**

Cek: λ₁ + λ₂ = 1640 = trace(X'X) ✔ ; λ₁·λ₂ = 666.875 = det(X'X) ✔

## Langkah 5. Eigenvektor, (X'X − λI)x = 0 dengan OBE

**Untuk λ₁ = 894,3303**

$$
\begin{bmatrix}750-894{,}33 & 25\\ 25 & 890-894{,}33\end{bmatrix}=\begin{bmatrix}-144{,}33 & 25\\ 25 & -4{,}33\end{bmatrix}
$$

OBE (baris 1 ÷ −144,33): −144,33·x₁ + 25·x₂ = 0 → x₁ = 0,1732·x₂

Ambil x₂ = 1, maka **v₁ = [0,1732 ; 1]**

Normalisasi:

- a₁₁ = 0,1732 / √(0,1732² + 1²) = 0,1732 / 1,0149 = **0,1707**
- a₂₁ = 1 / 1,0149 = **0,9853**

a₁ = [0,1707 ; 0,9853]

**Untuk λ₂ = 745,6697**

$$
\begin{bmatrix}750-745{,}67 & 25\\ 25 & 890-745{,}67\end{bmatrix}=\begin{bmatrix}4{,}33 & 25\\ 25 & 144{,}33\end{bmatrix}
$$

OBE: 4,33·x₁ + 25·x₂ = 0 → x₁ = −5,7732·x₂. Dengan x₁ = 1 maka x₂ = −0,1732, sehingga **v₂ = [1 ; −0,1732]**

Normalisasi:

- a₁₂ = 1 / 1,0149 = **0,9853**
- a₂₂ = −0,1732 / 1,0149 = **−0,1707**

a₂ = [0,9853 ; −0,1707]

$$
A=[a_1\;a_2]=\begin{bmatrix}0{,}1707 & 0{,}9853\\ 0{,}9853 & -0{,}1707\end{bmatrix}
$$

Cek: a₁'a₂ = 0,1682 − 0,1682 = 0 (ortogonal) ✔

> Catatan tanda: arah eigenvektor boleh dikali −1 (seperti pada contoh di slide). Hasil biplot hanya tercermin/terbalik, **interpretasi tidak berubah**.

## Langkah 6. Matriks L, U, G, dan H

**Matriks L** (akar dari nilai eigen)

$$
L=\begin{bmatrix}\sqrt{894{,}33} & 0\\ 0 & \sqrt{745{,}67}\end{bmatrix}=\begin{bmatrix}29{,}9054 & 0\\ 0 & 27{,}3070\end{bmatrix}
$$

**Matriks U**, Uᵢ = (1/√λᵢ) · X · aᵢ

- U₁ = (1/29,9054) · X · a₁
- U₂ = (1/27,3070) · X · a₂

Contoh observasi 1: U₁ = (−5·0,1707 + (−16)·0,9853)/29,9054 = −16,6165/29,9054 = −0,5557

| Obs | U₁ | U₂ |
|---|---|---|
| 1 | −0,5557 | −0,0804 |
| 2 | −0,3624 | 0,0688 |
| 3 | −0,1692 | 0,2179 |
| 4 | 0,0241 | 0,3671 |
| 5 | 0,2174 | 0,5163 |
| 6 | −0,0329 | 0,0063 |
| 7 | 0,3251 | 0,1242 |
| 8 | 0,5975 | −0,2992 |
| 9 | 0,0747 | −0,3858 |
| 10 | −0,1186 | −0,5350 |

Cek: U'U = I ✔ (kolom U orthonormal).

**Skor objek G = U·L^0,5** (α = 0,5)

L^0,5 = diag(√29,9054 ; √27,3070) = diag(5,4686 ; 5,2256)

Contoh observasi 1: G₁ = −0,5557 × 5,4686 = −3,04 ; G₂ = −0,0804 × 5,2256 = −0,42

| Obs | G₁ (Dim 1) | G₂ (Dim 2) |
|---|---|---|
| 1 | −3,0389 | −0,4202 |
| 2 | −1,9820 | 0,3593 |
| 3 | −0,9250 | 1,1388 |
| 4 | 0,1319 | 1,9182 |
| 5 | 1,1889 | 2,6977 |
| 6 | −0,1802 | 0,0327 |
| 7 | 1,7777 | 0,6488 |
| 8 | 3,2674 | −1,5633 |
| 9 | 0,4086 | −2,0162 |
| 10 | −0,6483 | −2,7957 |

**Skor peubah H' = L^0,5·A'**

$$
H'=\begin{bmatrix}5{,}4686 & 0\\ 0 & 5{,}2256\end{bmatrix}\begin{bmatrix}0{,}1707 & 0{,}9853\\ 0{,}9853 & -0{,}1707\end{bmatrix}=\begin{bmatrix}0{,}9333 & 5{,}3883\\ 5{,}1489 & -0{,}8919\end{bmatrix}
$$

Jadi (baris = peubah):

| Peubah | H₁ (Dim 1) | H₂ (Dim 2) |
|---|---|---|
| X₁ (Fisika) | 0,9333 | 5,1489 |
| X₂ (Matematika) | 5,3883 | −0,8919 |

Cek SVD: X = U·L·A' (selisih maksimum ≈ 0) ✔

## Keragaman yang dijelaskan

- Dimensi 1: λ₁/(λ₁+λ₂) = 894,33/1640 = **54,53 %**
- Dimensi 2: λ₂/(λ₁+λ₂) = 745,67/1640 = **45,47 %**
- Kumulatif = **100 %** (karena p = 2, biplot menyajikan seluruh informasi data tanpa ada yang hilang)

## Langkah 7–8. Plot biplot

Titik (objek) → G ; vektor dari titik pusat (0,0) (peubah) → H

```
Dim2
 3 |                  
   |          X1 (0,93 ; 5,15) → mengarah ke atas
 2 |        5 (1,19 ; 2,70)
   |     4 (0,13 ; 1,92)
 1 |   3 (−0,93 ; 1,14)       7 (1,78 ; 0,65)
   |          6 (≈ pusat)
 0 +--1-------------------------------→ Dim1
   |  (−3,04;−0,42)  2(−1,98;0,36)        8 (3,27 ; −1,56) 
-1 |                                 X2 (5,39 ; −0,89) →
-2 |             9 (0,41 ; −2,02)
-3 |         10 (−0,65 ; −2,80)
```

(Gambar kasar. Untuk gambar presisi, plot titik G dan panah H pada tabel di atas.)

## Langkah 9. Interpretasi

**1. Korelasi antarpeubah (sudut antarvektor).**
Vektor X₁ (0,93 ; 5,15) dan X₂ (5,39 ; −0,89) membentuk sudut sekitar **89°**, hampir tegak lurus. Artinya Nilai Fisika dan Nilai Matematika **hampir tidak berkorelasi** (korelasi mendekati nol). Ini sesuai dengan korelasi sebenarnya dari data: r = 25/√(750·890) = **0,03**.

**2. Keragaman peubah (panjang vektor).**
Panjang vektor X₁ ≈ 5,23 dan X₂ ≈ 5,46. Keduanya panjang dan hampir sama, sehingga kedua peubah terwakili dengan baik pada bidang dua dimensi. X₂ sedikit lebih panjang, jadi keragaman nilai Matematika sedikit lebih besar daripada Fisika (890 > 750).

**3. Makna dimensi.**
Dimensi 1 (sumbu mendatar) pada dasarnya menggambarkan **Matematika** (vektor X₂ hampir sejajar sumbu ini). Dimensi 2 (sumbu tegak) menggambarkan **Fisika** (vektor X₁ hampir sejajar sumbu ini).

**4. Kemiripan antarobjek (jarak titik).**
- Observasi 2 dan 3 relatif berdekatan, sehingga profil nilainya mirip (Fisika 75–80, Matematika 70–75).
- Observasi 4 dan 5 berdekatan di atas, sehingga keduanya sama-sama kuat di Fisika.
- Observasi 8 berada paling jauh di kanan, sehingga profilnya berbeda dengan observasi 1 yang paling jauh di kiri.

**5. Posisi objek terhadap vektor (nilai peubah pada objek).**

| Observasi | Posisi | Interpretasi |
|---|---|---|
| 8 (70 ; 100) | Kanan jauh, searah X₂ | Nilai Matematika **tertinggi** (di atas rata-rata) |
| 7 (80 ; 90) | Kanan, searah X₂ | Matematika di atas rata-rata |
| 5 (90 ; 85) | Atas, searah X₁ | Fisika **tertinggi** (di atas rata-rata) |
| 4 (85 ; 80) | Atas, searah X₁ | Fisika di atas rata-rata |
| 10 (60 ; 80) | Bawah, berlawanan dengan X₁ | Fisika **terendah** (di bawah rata-rata) |
| 9 (65 ; 85) | Bawah, berlawanan dengan X₁ | Fisika di bawah rata-rata |
| 1 (70 ; 65) | Kiri jauh, berlawanan dengan X₂ | Matematika **terendah** (di bawah rata-rata) |
| 6 (75 ; 80) | Hampir di titik pusat | Nilai dekat rata-rata (75 ; 81) pada kedua mata pelajaran |

## Kesimpulan Soal 1

1. Nilai Fisika dan Matematika **tidak saling berkorelasi** (vektor hampir tegak lurus, ≈ 89°). Nilai Fisika yang tinggi tidak berarti Matematika tinggi, begitu pula sebaliknya.
2. Dua dimensi menjelaskan **100 %** keragaman data (54,53 % + 45,47 %), sehingga biplot ini menggambarkan data secara lengkap.
3. Siswa 8 dan 7 unggul di Matematika, siswa 5 dan 4 unggul di Fisika, siswa 1 lemah di Matematika, siswa 10 dan 9 lemah di Fisika, dan siswa 6 berada di sekitar rata-rata.

---

# SOAL 2 — Survei Persepsi Masyarakat terhadap Bank BR

## a. Apa yang dimaksud dengan analisis biplot?

Analisis biplot adalah teknik statistika deskriptif **peubah ganda** yang menyajikan **objek pengamatan dan peubah sekaligus dalam satu grafik berdimensi rendah** (umumnya dua dimensi). Biplot diperkenalkan oleh Gabriel (1971) dan didasarkan pada **penguraian nilai singular (SVD)**, yang dapat dikaitkan dengan PCA. Dengan biplot, kita dapat:

1. melihat **kemiripan antarobjek** (dari jarak antartitik),
2. melihat **keragaman peubah** (dari panjang vektor),
3. melihat **korelasi antarpeubah** (dari sudut antarvektor),
4. melihat **nilai peubah pada suatu objek** (dari posisi objek terhadap arah vektor peubah).

Pada soal ini objeknya adalah **9 bank** dan peubahnya adalah **7 atribut** persepsi.

## b. Tahapan analisis biplot

1. Siapkan data dalam bentuk matriks **X\*** (n objek × p peubah). Pada soal ini 9 bank × 7 atribut.
2. Koreksi terhadap rataan: **X = X\* − X̄** (kolom dipusatkan).
3. Hitung **X'X**.
4. Cari nilai eigen **λᵢ** dari |X'X − λI| = 0 (rumus kuadrat/ABC), lalu **lᵢ = √λᵢ**.
5. Cari eigenvektor (X'X − λI)x = 0 dengan OBE, lalu normalisasikan sehingga diperoleh **A**.
6. Lakukan SVD: X = U L A' dengan Uᵢ = (1/√λᵢ)·X·aᵢ dan L = diag(√λᵢ).
7. Hitung skor objek **G = U L^α** dan skor peubah **H' = L^(1−α) A'**, dengan 0 ≤ α ≤ 1
   (α = 0: mempertahankan jarak antarobjek; α = 0,5: seimbang; α = 1: mempertahankan hubungan antarpeubah).
8. Hitung keragaman yang dijelaskan: λ₁/Σλ dan λ₂/Σλ untuk menilai kualitas biplot.
9. **Plot G (titik objek) dan H (vektor peubah)** pada satu bidang.
10. Buat **interpretasi** dan kesimpulan.

## c. Interpretasi biplot dan kesimpulan

> Nilai koordinat di bawah dibaca secara perkiraan dari gambar biplot yang diberikan.

### 1. Kualitas biplot

Dimensi 1 menjelaskan 77,4 % dan Dimensi 2 menjelaskan 16,2 %. Totalnya **93,6 %** keragaman data. Biplot ini sangat baik untuk menyajikan data 7 atribut dalam 2 dimensi, dan hanya sekitar 6,4 % informasi yang hilang.

### 2. Keragaman peubah (panjang vektor)

Semua vektor atribut relatif panjang. Vektor **layanan, ATM, cabang, produk, dan lokasi** paling panjang. Atribut-atribut ini terwakili dengan sangat baik dan paling membedakan bank-bank. Vektor **aman** dan **hadiah** sedikit lebih pendek.

### 3. Korelasi antaratribut (sudut antarvektor)

- **Cabang, produk, lokasi, dan aman** hampir berimpit searah sumbu Dim 1, sudutnya sangat kecil. Atribut-atribut ini **berkorelasi positif kuat**. Bank yang fasilitas cabangnya baik cenderung juga baik dalam produk, kemudahan lokasi, dan keamanan.
- **ATM** dan **hadiah** berada di bawah sumbu, sudutnya dengan kelompok di atas masih kurang dari 90°. Keduanya **berkorelasi positif sedang** dengan cabang, produk, dan lokasi. ATM dan hadiah juga cukup berdekatan, sehingga berkorelasi positif.
- **Layanan** mengarah ke atas (Dim 2). Sudutnya dengan lokasi atau cabang sekitar 55–60°, jadi korelasinya positif sedang. Sudutnya dengan hadiah mendekati 90°, jadi **hampir tidak berkorelasi**.
- Tidak ada pasangan atribut yang berlawanan arah (sudut mendekati 180°), sehingga **tidak ada korelasi negatif**.

### 4. Kemiripan antarbank (jarak titik)

- **BMD dan BC** sama-sama di sisi kanan dan berdekatan, sehingga profil penilaiannya mirip dan tinggi. BN juga di sisi kanan, agak ke bawah.
- **BR, BDN, dan BPM** berada di kiri-bawah dan berdekatan, sehingga profil persepsinya mirip.
- **BMG dan NI** berada di kiri-atas, sehingga profilnya mirip dan berbeda dari kelompok lain.
- Bank BR sangat berbeda dengan BMD/BC (jaraknya jauh).

### 5. Karakteristik bank (posisi bank terhadap vektor atribut)

| Bank | Posisi | Interpretasi |
|---|---|---|
| **BMD** | Kanan jauh, agak atas | Unggul pada hampir semua atribut, terutama cabang, produk, lokasi, dan juga layanan |
| **BC** | Kanan, di sumbu Dim 1 | Unggul pada lokasi, cabang, produk, dan aman |
| **BN** | Kanan-bawah | Unggul pada ATM (searah vektor ATM), cukup baik pada hadiah dan aman |
| **BMG, NI** | Kiri-atas | Layanan relatif baik (searah layanan), tetapi lemah pada cabang, produk, lokasi, ATM |
| **BPM** | Dekat pusat, agak bawah | Mendekati rata-rata, hadiah dan ATM sedikit lebih baik daripada atribut lain |
| **BDN** | Kiri-bawah jauh | Di bawah rata-rata pada hampir semua atribut, terburuk secara keseluruhan |
| **BR** | Kiri-bawah | Berlawanan arah dengan hampir semua vektor atribut |

### 6. Posisi Bank BR secara khusus

Bank BR berada di sisi **kiri-bawah**, berlawanan arah dengan vektor cabang, produk, lokasi, aman, ATM, dan layanan. Hasilnya:

- BR dipersepsikan **di bawah rata-rata** pada hampir semua atribut.
- Kelemahan terbesar BR ada pada atribut **Dim 1 (cabang, produk, lokasi) dan layanan**.
- Atribut yang **relatif paling baik (paling mendekati rata-rata)** adalah **hadiah** (BR berada di sisi bawah yang sama dengan vektor hadiah) dan sedikit pada ATM.
- BR sedikit lebih baik daripada BDN (lebih dekat ke pusat pada Dim 1), tetapi jauh tertinggal dari BMD, BC, dan BN.

## Kesimpulan Soal 2

1. Biplot (Dim 1 + Dim 2 = 93,6 %) merepresentasikan data dengan sangat baik.
2. Atribut **cabang, produk, lokasi, dan aman** berkorelasi positif kuat dan menjadi pembeda utama antarbank pada Dim 1. Atribut **layanan** membentuk dimensi tersendiri (Dim 2) dan tidak berkorelasi dengan hadiah.
3. Bank terbaik adalah **BMD dan BC** (juga BN untuk ATM). **BMG dan NI** unggul di layanan saja. **BDN dan BR** paling lemah.
4. **Saran untuk Bank BR:** prioritaskan perbaikan **jaringan kantor cabang, kemudahan lokasi layanan, kualitas produk, dan kualitas layanan petugas**. Perbaikan ATM dan keamanan juga perlu dilakukan. Bank BR dapat meniru strategi BMD/BC (cabang, produk, lokasi) dan BN (ATM). Hadiah merupakan atribut yang relatif sudah mendekati rata-rata sehingga prioritasnya lebih rendah.
