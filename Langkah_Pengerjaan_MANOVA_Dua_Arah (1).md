# Langkah Pengerjaan MANOVA Dua Arah (Two-Way MANOVA)

**Contoh 6.11 — Data Pita Film Plastik (Plastic Film Data, Tabel 6.4)**

Dokumen ini membahas ulang satu-satunya contoh soal dalam modul *Analisis Multivariat — MANOVA Two-Way* (Tim Teaching Analisis Multivariat, Sains Data ITERA), langkah demi langkah, dari data mentah sampai keputusan akhir. Semua angka berasal dari data soal asli. Seluruh perhitungan dihitung ulang dengan kode (Python, aritmetika desimal eksak), lalu dibandingkan dengan angka di modul.

Catatan penulisan: desimal ditulis dengan titik (seperti di modul). Tanda $'$ pada vektor/matriks berarti transpos.

---

## 0. Yang Ada di Modul dan Cara Membaca Dokumen Ini

Modul memuat: (a) model dan hipotesis MANOVA dua arah, (b) tabel MANOVA dua arah berisi rumus $SSP$, (c) rumus Wilks' Lambda dan statistik $F$ untuk interaksi, faktor 1, dan faktor 2, (d) **satu contoh soal** (Contoh 6.11), dan (e) coretan tulisan tangan dosen yang menunjukkan cara mencari rata-rata dan urutan pengerjaan.

**Apa yang tidak ditampilkan modul (dan direkonstruksi di sini):** modul hanya menunjukkan hasil akhir $\Lambda^{\ast}$ dan $F$ (serta tiga nilai determinan: 275.7098, 722.0212, 527.1347). Elemen-elemen matriks $SSP_{fac1}$, $SSP_{fac2}$, $SSP_{int}$, $SSP_{res}$, semua rata-rata, dan determinan $\vert{}SSP_{int}+SSP_{res}\vert{}$ tidak ada di modul. Semuanya dihitung ulang di bawah ini dan dicocokkan dengan angka modul.

Penting: pada MANOVA dua arah di modul, matriks $SSP$ berperan sebagai matriks hipotesis $H$ (untuk faktor/interaksi yang diuji) dan matriks galat $E$ ($SSP_{res}$). Modul memakai nama $SSP$, bukan $H$ dan $E$, jadi dokumen ini juga memakai nama $SSP$. Statistik uji yang dipakai modul **hanya Wilks' Lambda** yang dikonversi ke $F$.

---

# CONTOH 6.11 — Analisis Multivariat Dua Arah Data Pita Film

## A. Soal dan Data yang Diketahui

**Soal (dari modul):** Syarat optimum yang ditekan pada pita film telah diuji menggunakan suatu teknologi yang disebut operasi evolusioner. Dalam pembicaraan ini terdapat tiga respon:

| Simbol | Respon (Dependent Variable, DV) |
|---|---|
| $x_1$ | Tahan arah (basah) — *tear resistance* |
| $x_2$ | Kehalusan permukaan — *gloss* |
| $x_3$ | Kedap cahaya — *opacity* |

Ketiganya diukur pada dua level, rata-rata dan jumlah aditif. Pengukuran dilakukan sebanyak 5 kali pada setiap kombinasi faktor.

**Faktor (Independent Variable, IV):**

| Faktor | Nama | Simbol indeks | Level |
|---|---|---|---|
| Faktor 1 | Perubahan laju ekstrusi (*change in rate of extrusion*) | $\ell = 1,2,\dots,g$ | $\ell=1$: Low (−10%); $\ell=2$: High (10%) |
| Faktor 2 | Jumlah aditif (*amount of additive*) | $k = 1,2,\dots,b$ | $k=1$: Low (1.0%); $k=2$: High (1.5%) |

Di modul, tulisan tangan dosen menjelaskan: IV = perlakuan (pengaruh) dan DV = hasil pengukuran. Di sini IV = 2 faktor (ekstrusi dan aditif), DV = 3 respon ($x_1,x_2,x_3$), sehingga MANOVA (DV $\geq 2$) dengan dua arah (IV = 2 faktor).

**Ukuran yang diketahui:** $g=2$ (jumlah level faktor 1), $b=2$ (jumlah level faktor 2), $n=5$ (ulangan per sel; di coretan modul ditulis $r=5$), $p=3$ (jumlah variabel respon).

**Model:** $\mathbf{X}_{\ell kr} = \boldsymbol{\mu} + \boldsymbol{\tau}_\ell + \boldsymbol{\beta}_k + \boldsymbol{\gamma}_{\ell k} + \mathbf{e}_{\ell kr}$, dengan $\ell=1,2$; $k=1,2$; $r=1,\dots,5$.
Setiap $\mathbf{X}_{\ell kr}$ adalah **vektor $3\times1$** satu pengamatan. Contoh dari coretan modul: $\mathbf{x}_{111}=[6.5;\,9.5;\,4.4]'$ ($\ell=1,k=1,r=1$, baris pertama sel kiri-atas) dan $\mathbf{x}_{215}=[6.8;\,8.5;\,3.4]'$ ($\ell=2,k=1,r=5$, baris terakhir sel kiri-bawah).
Total pengamatan: $g\cdot b\cdot n = 2\cdot2\cdot5 = 20$ vektor, jadi 20 kombinasi $(\ell,k,r)$.

**Data (Tabel 6.4):**

Faktor 1: $\ell=1$ — Low (−10%)

| $r$ | $k=1$ (Low 1.0%): $x_1$ | $x_2$ | $x_3$ | $k=2$ (High 1.5%): $x_1$ | $x_2$ | $x_3$ |
|---|---|---|---|---|---|---|
| 1 | 6.5 | 9.5 | 4.4 | 6.9 | 9.1 | 5.7 |
| 2 | 6.2 | 9.9 | 6.4 | 7.2 | 10.0 | 2.0 |
| 3 | 5.8 | 9.6 | 3.0 | 6.9 | 9.9 | 3.9 |
| 4 | 6.5 | 9.6 | 4.1 | 6.1 | 9.5 | 1.9 |
| 5 | 6.5 | 9.2 | 0.8 | 6.3 | 9.4 | 5.7 |

Faktor 1: $\ell=2$ — High (10%)

| $r$ | $k=1$ (Low 1.0%): $x_1$ | $x_2$ | $x_3$ | $k=2$ (High 1.5%): $x_1$ | $x_2$ | $x_3$ |
|---|---|---|---|---|---|---|
| 1 | 6.7 | 9.1 | 2.8 | 7.1 | 9.2 | 8.4 |
| 2 | 6.6 | 9.3 | 4.1 | 7.0 | 8.8 | 5.2 |
| 3 | 7.2 | 8.3 | 3.8 | 7.2 | 9.7 | 6.9 |
| 4 | 7.1 | 8.4 | 1.6 | 7.5 | 10.1 | 2.7 |
| 5 | 6.8 | 8.5 | 3.4 | 7.6 | 9.2 | 1.9 |

**Yang diminta:** menguji apakah ada pengaruh (1) interaksi faktor 1 × faktor 2, (2) faktor 1, (3) faktor 2, terhadap ketiga respon secara bersama-sama, pada tingkat signifikansi $\alpha=0.05$ (modul memakai $F_{.05}$). Soal di modul diawali dengan pertanyaan: apakah interaksi faktor 1 dan 2 mempunyai pengaruh signifikan? Setelah itu faktor 1 dan faktor 2 diuji.

---

## B. Peta Langkah Pengerjaan

| No | Langkah | Hasil | Dipakai untuk |
|---|---|---|---|
| 1 | Tentukan $g,b,n,p$ | $g=2,b=2,n=5,p=3$ | Derajat kebebasan dan rumus |
| 2 | Rata-rata tiap sel $\bar{\mathbf{x}}_{\ell k}$ (4 vektor) | 4 vektor $3\times1$ | $SSP_{int}$ dan $SSP_{res}$ |
| 3 | Rata-rata tiap level faktor 1 $\bar{\mathbf{x}}_{\ell\cdot}$ (2 vektor) | 2 vektor | $SSP_{fac1}$ dan $SSP_{int}$ |
| 4 | Rata-rata tiap level faktor 2 $\bar{\mathbf{x}}_{\cdot k}$ (2 vektor) | 2 vektor | $SSP_{fac2}$ dan $SSP_{int}$ |
| 5 | Rata-rata keseluruhan $\bar{\mathbf{x}}$ | 1 vektor | Semua $SSP$ |
| 6 | Hitung $SSP_{fac1}$ | matriks $3\times3$ | Uji faktor 1 |
| 7 | Hitung $SSP_{fac2}$ | matriks $3\times3$ | Uji faktor 2 |
| 8 | Hitung $SSP_{int}$ | matriks $3\times3$ | Uji interaksi |
| 9 | Hitung $SSP_{res}$ | matriks $3\times3$ | Penyebut semua $\Lambda^{\ast}$ |
| 10 | Hitung $SSP_{cor}$ dan cek $SSP_{cor}=SSP_{fac1}+SSP_{fac2}+SSP_{int}+SSP_{res}$ | matriks $3\times3$ | Verifikasi |
| 11 | Susun Tabel MANOVA dua arah | tabel | Ringkasan |
| 12 | Uji interaksi: determinan, $\Lambda^{\ast}$, $F$, $\nu_1,\nu_2$, $F_{tabel}$, keputusan | $\Lambda^{\ast}=0.7771$, $F=1.34$ | Dilakukan **pertama**, sesuai modul |
| 13 | Uji faktor 1 | $\Lambda_1^{\ast}=0.3819$, $F_1=7.55$ | Keputusan faktor 1 |
| 14 | Uji faktor 2 | $\Lambda_2^{\ast}=0.5230$, $F_2=4.26$ | Keputusan faktor 2 |
| 15 | Bandingkan dengan modul, simpulkan | — | Jawaban akhir |

Alasan urutan: modul menguji interaksi terlebih dahulu, karena jika interaksi nyata, pengaruh faktor tidak bisa ditafsirkan terpisah. Namun menurut modul, ketiga uji memakai bahan yang sama: $SSP_{res}$ plus $SSP$ efek yang diuji.

---

## C. Pengerjaan Langkah demi Langkah

### Langkah 1 — Tentukan $g$, $b$, $n$, $p$

**Tujuan:** Keempat bilangan ini muncul di semua derajat kebebasan dan rumus $F$.

**Keterangan:** $g$ = banyak level faktor 1; $b$ = banyak level faktor 2; $n$ = ulangan per sel; $p$ = banyak variabel respon.

**Substitusi / perhitungan:**

- Faktor 1 (ekstrusi) punya level Low dan High, jadi $g=2$ (coretan modul: $\ell=1,2$ sehingga $g=2$).
- Faktor 2 (aditif) punya level Low dan High, jadi $b=2$ (coretan modul: $k=1,2$ sehingga $b=2$).
- Tiap sel berisi 5 baris, jadi $n=5$.
- Ada tiga respon $x_1,x_2,x_3$, jadi $p=3$.
- Banyak sel $= g\cdot b = 4$; total pengamatan $= g\cdot b\cdot n = 20$.

**Hasil:** $g=2$, $b=2$, $n=5$, $p=3$.

**Penggunaan hasil:** dipakai sebagai pengali ($bn$, $gn$, $n$) dan di derajat kebebasan di Langkah 6–14.

### Langkah 2 — Rata-rata tiap sel $\bar{\mathbf{x}}_{\ell k}$

**Tujuan:** rata-rata sel dibutuhkan untuk $SSP_{res}$ (simpangan tiap data terhadap rata-rata selnya) dan $SSP_{int}$.

**Rumus:** $\bar{x}_{\ell k,j} = \dfrac{1}{n}\sum_{r=1}^{n} x_{\ell kr,j}$, yaitu jumlahkan 5 nilai variabel $j$ pada sel $(\ell,k)$, bagi 5. Dikerjakan untuk $j=1,2,3$ sehingga hasilnya vektor $3\times1$.

**Keterangan:** $\bar{x}_{\ell k,j}$ = rata-rata variabel ke-$j$ pada sel $(\ell,k)$. Dari coretan modul: $\bar{\mathbf{x}}_{11}$ dihitung dari kolom-kolom sel kiri-atas, dibagi 5.

**Sel $(\ell=1,k=1)$** — Faktor 1 Low (−10%), Faktor 2 Low (1.0%)

| Variabel | Substitusi | Jumlah | Rata-rata |
|---|---|---|---|
| $x_1$ | $\dfrac{6.5+6.2+5.8+6.5+6.5}{5}$ | 31.5 | $\dfrac{31.5}{5} = 6.3$ |
| $x_2$ | $\dfrac{9.5+9.9+9.6+9.6+9.2}{5}$ | 47.8 | $\dfrac{47.8}{5} = 9.56$ |
| $x_3$ | $\dfrac{4.4+6.4+3+4.1+0.8}{5}$ | 18.7 | $\dfrac{18.7}{5} = 3.74$ |

**Hasil:** $\bar{\mathbf{x}}_{11} = \begin{bmatrix}6.3\\9.56\\3.74\end{bmatrix}$

**Sel $(\ell=1,k=2)$** — Faktor 1 Low (−10%), Faktor 2 High (1.5%)

| Variabel | Substitusi | Jumlah | Rata-rata |
|---|---|---|---|
| $x_1$ | $\dfrac{6.9+7.2+6.9+6.1+6.3}{5}$ | 33.4 | $\dfrac{33.4}{5} = 6.68$ |
| $x_2$ | $\dfrac{9.1+10+9.9+9.5+9.4}{5}$ | 47.9 | $\dfrac{47.9}{5} = 9.58$ |
| $x_3$ | $\dfrac{5.7+2+3.9+1.9+5.7}{5}$ | 19.2 | $\dfrac{19.2}{5} = 3.84$ |

**Hasil:** $\bar{\mathbf{x}}_{12} = \begin{bmatrix}6.68\\9.58\\3.84\end{bmatrix}$

**Sel $(\ell=2,k=1)$** — Faktor 1 High (10%), Faktor 2 Low (1.0%)

| Variabel | Substitusi | Jumlah | Rata-rata |
|---|---|---|---|
| $x_1$ | $\dfrac{6.7+6.6+7.2+7.1+6.8}{5}$ | 34.4 | $\dfrac{34.4}{5} = 6.88$ |
| $x_2$ | $\dfrac{9.1+9.3+8.3+8.4+8.5}{5}$ | 43.6 | $\dfrac{43.6}{5} = 8.72$ |
| $x_3$ | $\dfrac{2.8+4.1+3.8+1.6+3.4}{5}$ | 15.7 | $\dfrac{15.7}{5} = 3.14$ |

**Hasil:** $\bar{\mathbf{x}}_{21} = \begin{bmatrix}6.88\\8.72\\3.14\end{bmatrix}$

**Sel $(\ell=2,k=2)$** — Faktor 1 High (10%), Faktor 2 High (1.5%)

| Variabel | Substitusi | Jumlah | Rata-rata |
|---|---|---|---|
| $x_1$ | $\dfrac{7.1+7+7.2+7.5+7.6}{5}$ | 36.4 | $\dfrac{36.4}{5} = 7.28$ |
| $x_2$ | $\dfrac{9.2+8.8+9.7+10.1+9.2}{5}$ | 47 | $\dfrac{47}{5} = 9.4$ |
| $x_3$ | $\dfrac{8.4+5.2+6.9+2.7+1.9}{5}$ | 25.1 | $\dfrac{25.1}{5} = 5.02$ |

**Hasil:** $\bar{\mathbf{x}}_{22} = \begin{bmatrix}7.28\\9.4\\5.02\end{bmatrix}$

**Penggunaan hasil:** $\bar{\mathbf{x}}_{11},\bar{\mathbf{x}}_{12},\bar{\mathbf{x}}_{21},\bar{\mathbf{x}}_{22}$ dipakai pada Langkah 8 ($SSP_{int}$) dan Langkah 9 ($SSP_{res}$). Selain itu, rata-rata level faktor (Langkah 3 dan 4) dapat dicek sebagai rata-rata dua sel.

### Langkah 3 — Rata-rata tiap level faktor 1 $\bar{\mathbf{x}}_{\ell\cdot}$

**Tujuan:** untuk $SSP_{fac1}$ dan $SSP_{int}$. Baris $\ell$ mengumpulkan data dari kedua kolom aditif: $b\cdot n = 10$ pengamatan.

**Rumus:** $\bar{x}_{\ell\cdot,j} = \dfrac{1}{bn}\sum_{k=1}^{b}\sum_{r=1}^{n} x_{\ell kr,j}$

**Keterangan:** titik pada $\ell\cdot$ berarti indeks $k$ dan $r$ dijumlahkan. Coretan modul: $\bar{\mathbf{x}}_{1\cdot}$ = baris 1 (sel $k=1$ dan $k=2$ digabung), dibagi 10.

**Level $\ell=1$** (Low (−10%)): gabungkan sel $(1,1)$ dan $(1,2)$ = 10 pengamatan.

| Variabel | Substitusi (jumlah sel $k=1$) + (jumlah sel $k=2$) | Jumlah | Rata-rata |
|---|---|---|---|
| $x_1$ | $\dfrac{31.5 + 33.4}{10}$ | 64.9 | $\dfrac{64.9}{10} = 6.49$ |
| $x_2$ | $\dfrac{47.8 + 47.9}{10}$ | 95.7 | $\dfrac{95.7}{10} = 9.57$ |
| $x_3$ | $\dfrac{18.7 + 19.2}{10}$ | 37.9 | $\dfrac{37.9}{10} = 3.79$ |

**Hasil:** $\bar{\mathbf{x}}_{1\cdot} = \begin{bmatrix}6.49\\9.57\\3.79\end{bmatrix}$

**Level $\ell=2$** (High (10%)): gabungkan sel $(2,1)$ dan $(2,2)$ = 10 pengamatan.

| Variabel | Substitusi (jumlah sel $k=1$) + (jumlah sel $k=2$) | Jumlah | Rata-rata |
|---|---|---|---|
| $x_1$ | $\dfrac{34.4 + 36.4}{10}$ | 70.8 | $\dfrac{70.8}{10} = 7.08$ |
| $x_2$ | $\dfrac{43.6 + 47}{10}$ | 90.6 | $\dfrac{90.6}{10} = 9.06$ |
| $x_3$ | $\dfrac{15.7 + 25.1}{10}$ | 40.8 | $\dfrac{40.8}{10} = 4.08$ |

**Hasil:** $\bar{\mathbf{x}}_{2\cdot} = \begin{bmatrix}7.08\\9.06\\4.08\end{bmatrix}$

**Penggunaan hasil:** $\bar{\mathbf{x}}_{1\cdot},\bar{\mathbf{x}}_{2\cdot}$ dipakai di Langkah 6 ($SSP_{fac1}$) dan Langkah 8 ($SSP_{int}$).

### Langkah 4 — Rata-rata tiap level faktor 2 $\bar{\mathbf{x}}_{\cdot k}$

**Tujuan:** untuk $SSP_{fac2}$ dan $SSP_{int}$. Kolom $k$ mengumpulkan data dari kedua baris ekstrusi: $g\cdot n=10$ pengamatan.

**Rumus:** $\bar{x}_{\cdot k,j} = \dfrac{1}{gn}\sum_{\ell=1}^{g}\sum_{r=1}^{n} x_{\ell kr,j}$

**Keterangan:** titik di depan $k$ berarti indeks $\ell$ dan $r$ dijumlahkan. Coretan modul: $\bar{\mathbf{x}}_{\cdot1}$ = kolom Low 1.0% (sel atas dan bawah digabung), $\bar{\mathbf{x}}_{\cdot2}$ = kolom High 1.5%, masing-masing dibagi 10.

**Level $k=1$** (Low (1.0%)): gabungkan sel $(1,1)$ dan $(2,1)$ = 10 pengamatan.

| Variabel | Substitusi (jumlah sel $\ell=1$) + (jumlah sel $\ell=2$) | Jumlah | Rata-rata |
|---|---|---|---|
| $x_1$ | $\dfrac{31.5 + 34.4}{10}$ | 65.9 | $\dfrac{65.9}{10} = 6.59$ |
| $x_2$ | $\dfrac{47.8 + 43.6}{10}$ | 91.4 | $\dfrac{91.4}{10} = 9.14$ |
| $x_3$ | $\dfrac{18.7 + 15.7}{10}$ | 34.4 | $\dfrac{34.4}{10} = 3.44$ |

**Hasil:** $\bar{\mathbf{x}}_{\cdot1} = \begin{bmatrix}6.59\\9.14\\3.44\end{bmatrix}$

**Level $k=2$** (High (1.5%)): gabungkan sel $(1,2)$ dan $(2,2)$ = 10 pengamatan.

| Variabel | Substitusi (jumlah sel $\ell=1$) + (jumlah sel $\ell=2$) | Jumlah | Rata-rata |
|---|---|---|---|
| $x_1$ | $\dfrac{33.4 + 36.4}{10}$ | 69.8 | $\dfrac{69.8}{10} = 6.98$ |
| $x_2$ | $\dfrac{47.9 + 47}{10}$ | 94.9 | $\dfrac{94.9}{10} = 9.49$ |
| $x_3$ | $\dfrac{19.2 + 25.1}{10}$ | 44.3 | $\dfrac{44.3}{10} = 4.43$ |

**Hasil:** $\bar{\mathbf{x}}_{\cdot2} = \begin{bmatrix}6.98\\9.49\\4.43\end{bmatrix}$

**Penggunaan hasil:** dipakai di Langkah 7 ($SSP_{fac2}$) dan Langkah 8 ($SSP_{int}$).

### Langkah 5 — Rata-rata keseluruhan $\bar{\mathbf{x}}$

**Tujuan:** titik acuan untuk semua efek.

**Rumus:** $\bar{x}_j = \dfrac{1}{gbn}\sum_{\ell}\sum_{k}\sum_{r} x_{\ell kr,j}$ (jumlah 20 nilai dibagi 20). Cara cepat yang konsisten: jumlahkan empat jumlah sel dari Langkah 2.

**Substitusi dan perhitungan:**

| Variabel | Jumlah sel (1,1)+(1,2)+(2,1)+(2,2) | Jumlah | Rata-rata |
|---|---|---|---|
| $x_1$ | 31.5+33.4+34.4+36.4 | 135.7 | $\dfrac{135.7}{20} = 6.785$ |
| $x_2$ | 47.8+47.9+43.6+47 | 186.3 | $\dfrac{186.3}{20} = 9.315$ |
| $x_3$ | 18.7+19.2+15.7+25.1 | 78.7 | $\dfrac{78.7}{20} = 3.935$ |

**Hasil:** $\bar{\mathbf{x}} = \begin{bmatrix}6.785\\9.315\\3.935\end{bmatrix}$

**Pemeriksaan:** rata-rata dua level faktor 1 harus sama dengan $\bar{\mathbf{x}}$: $x_1$: $(6.49+7.08)/2 = 6.785$ ✓; $x_2$: $(9.57+9.06)/2 = 9.315$ ✓; $x_3$: $(3.79+4.08)/2 = 3.935$ ✓.

**Penggunaan hasil:** $\bar{\mathbf{x}}$ menjadi pengurang di Langkah 6, 7, 8, dan 10.

**Rekap semua rata-rata:**

| Rata-rata | $x_1$ | $x_2$ | $x_3$ |
|---|---|---|---|
| $\bar{\mathbf{x}}_{11}$ | 6.3 | 9.56 | 3.74 |
| $\bar{\mathbf{x}}_{12}$ | 6.68 | 9.58 | 3.84 |
| $\bar{\mathbf{x}}_{21}$ | 6.88 | 8.72 | 3.14 |
| $\bar{\mathbf{x}}_{22}$ | 7.28 | 9.4 | 5.02 |
| $\bar{\mathbf{x}}_{1\cdot}$ | 6.49 | 9.57 | 3.79 |
| $\bar{\mathbf{x}}_{2\cdot}$ | 7.08 | 9.06 | 4.08 |
| $\bar{\mathbf{x}}_{\cdot 1}$ | 6.59 | 9.14 | 3.44 |
| $\bar{\mathbf{x}}_{\cdot 2}$ | 6.98 | 9.49 | 4.43 |
| $\bar{\mathbf{x}}$ | 6.785 | 9.315 | 3.935 |

### Langkah 6 — Hitung $SSP_{fac1}$

**Tujuan:** mengukur seberapa jauh rata-rata kedua level faktor 1 menyimpang dari rata-rata keseluruhan (matriks hipotesis untuk faktor 1).

**Rumus (dari tabel MANOVA modul):** $SSP_{fac1} = \sum_{\ell=1}^{g} bn\,(\bar{\mathbf{x}}_{\ell\cdot}-\bar{\mathbf{x}})(\bar{\mathbf{x}}_{\ell\cdot}-\bar{\mathbf{x}})'$

**Keterangan:** $\bar{\mathbf{x}}_{\ell\cdot}-\bar{\mathbf{x}}$ adalah vektor $3\times1$ ($=:\mathbf{d}_\ell$); $\mathbf{d}_\ell\mathbf{d}_\ell'$ adalah perkalian vektor kolom $3\times1$ dengan vektor baris $1\times3$, hasilnya matriks $3\times3$ dengan elemen $(i,j)=d_{\ell i}d_{\ell j}$; $bn = 2\cdot5 = 10$. Derajat kebebasan $g-1=1$.

**Substitusi vektor selisih:**

| Level | Variabel | Substitusi | $\mathbf{d}_\ell$ |
|---|---|---|---|
| $\ell=1$ | $x_1$ | $6.49 - 6.785$ | $-0.295$ |
| $\ell=1$ | $x_2$ | $9.57 - 9.315$ | $0.255$ |
| $\ell=1$ | $x_3$ | $3.79 - 3.935$ | $-0.145$ |
| $\ell=2$ | $x_1$ | $7.08 - 6.785$ | $0.295$ |
| $\ell=2$ | $x_2$ | $9.06 - 9.315$ | $-0.255$ |
| $\ell=2$ | $x_3$ | $4.08 - 3.935$ | $0.145$ |

```math
\mathbf{d}_1 = \begin{bmatrix}-0.295\\0.255\\-0.145\end{bmatrix},\qquad \mathbf{d}_2 = \begin{bmatrix}0.295\\-0.255\\0.145\end{bmatrix}
```

(Karena hanya dua level, $\mathbf{d}_2=-\mathbf{d}_1$; selisih ke rata-rata keseluruhan selalu berlawanan tanda.)

**Perhitungan:** $SSP_{fac1} = 10\,\mathbf{d}_1\mathbf{d}_1' + 10\,\mathbf{d}_2\mathbf{d}_2'$. Elemen yang berbeda dihitung satu per satu (matriks simetris, jadi elemen $(j,i)$ sama dengan $(i,j)$). Setiap suku ditulis $10\cdot d_{\ell i}\cdot d_{\ell j}$, suku pertama untuk $\ell=1$, kedua untuk $\ell=2$:

| Elemen | Substitusi (jumlah dari semua suku) | Hasil |
|---|---|---|
| $(1,1)$ | $10\cdot(-0.295)\cdot(-0.295) + 10\cdot0.295\cdot0.295$ = $0.87025 + 0.87025$ | $1.7405$ |
| $(1,2)$ | $10\cdot(-0.295)\cdot0.255 + 10\cdot0.295\cdot(-0.255)$ = $(-0.75225) + (-0.75225)$ | $-1.5045$ |
| $(1,3)$ | $10\cdot(-0.295)\cdot(-0.145) + 10\cdot0.295\cdot0.145$ = $0.42775 + 0.42775$ | $0.8555$ |
| $(2,2)$ | $10\cdot0.255\cdot0.255 + 10\cdot(-0.255)\cdot(-0.255)$ = $0.65025 + 0.65025$ | $1.3005$ |
| $(2,3)$ | $10\cdot0.255\cdot(-0.145) + 10\cdot(-0.255)\cdot0.145$ = $(-0.36975) + (-0.36975)$ | $-0.7395$ |
| $(3,3)$ | $10\cdot(-0.145)\cdot(-0.145) + 10\cdot0.145\cdot0.145$ = $0.21025 + 0.21025$ | $0.4205$ |

**Hasil:**

```math
SSP_{fac1} = \begin{bmatrix}1.7405&-1.5045&0.8555\\-1.5045&1.3005&-0.7395\\0.8555&-0.7395&0.4205\end{bmatrix}
```

**Pemeriksaan dimensi:** $3\times1$ kali $1\times3$ = $3\times3$ ✓; simetris ✓.

**Penggunaan hasil:** dijumlahkan dengan $SSP_{res}$ (Langkah 9) pada Langkah 13: $\vert{}SSP_{fac1}+SSP_{res}\vert{}$ (penyebut $\Lambda_1^{\ast}$).

### Langkah 7 — Hitung $SSP_{fac2}$

**Tujuan:** matriks hipotesis untuk faktor 2 (aditif).

**Rumus:** $SSP_{fac2} = \sum_{k=1}^{b} gn\,(\bar{\mathbf{x}}_{\cdot k}-\bar{\mathbf{x}})(\bar{\mathbf{x}}_{\cdot k}-\bar{\mathbf{x}})'$

(Di slide modul baris ini ditulis “SSP fac1” sebagai label di kolom Sumber Variasi “Faktor 2”; itu salah ketik. Rumusnya memakai $\bar{\mathbf{x}}_{\cdot k}$ dan pengali $gn$, jadi namanya $SSP_{fac2}$.)

**Keterangan:** $\mathbf{e}_k=\bar{\mathbf{x}}_{\cdot k}-\bar{\mathbf{x}}$; pengali $gn = 2\cdot5 = 10$; derajat kebebasan $b-1=1$.

**Substitusi vektor selisih:**

| Level | Variabel | Substitusi | $\mathbf{e}_k$ |
|---|---|---|---|
| $k=1$ | $x_1$ | $6.59 - 6.785$ | $-0.195$ |
| $k=1$ | $x_2$ | $9.14 - 9.315$ | $-0.175$ |
| $k=1$ | $x_3$ | $3.44 - 3.935$ | $-0.495$ |
| $k=2$ | $x_1$ | $6.98 - 6.785$ | $0.195$ |
| $k=2$ | $x_2$ | $9.49 - 9.315$ | $0.175$ |
| $k=2$ | $x_3$ | $4.43 - 3.935$ | $0.495$ |

```math
\mathbf{e}_1 = \begin{bmatrix}-0.195\\-0.175\\-0.495\end{bmatrix},\qquad \mathbf{e}_2 = \begin{bmatrix}0.195\\0.175\\0.495\end{bmatrix}
```

**Perhitungan:** $SSP_{fac2} = 10\,\mathbf{e}_1\mathbf{e}_1' + 10\,\mathbf{e}_2\mathbf{e}_2'$, suku pertama untuk $k=1$, kedua untuk $k=2$:

| Elemen | Substitusi (jumlah dari semua suku) | Hasil |
|---|---|---|
| $(1,1)$ | $10\cdot(-0.195)\cdot(-0.195) + 10\cdot0.195\cdot0.195$ = $0.38025 + 0.38025$ | $0.7605$ |
| $(1,2)$ | $10\cdot(-0.195)\cdot(-0.175) + 10\cdot0.195\cdot0.175$ = $0.34125 + 0.34125$ | $0.6825$ |
| $(1,3)$ | $10\cdot(-0.195)\cdot(-0.495) + 10\cdot0.195\cdot0.495$ = $0.96525 + 0.96525$ | $1.9305$ |
| $(2,2)$ | $10\cdot(-0.175)\cdot(-0.175) + 10\cdot0.175\cdot0.175$ = $0.30625 + 0.30625$ | $0.6125$ |
| $(2,3)$ | $10\cdot(-0.175)\cdot(-0.495) + 10\cdot0.175\cdot0.495$ = $0.86625 + 0.86625$ | $1.7325$ |
| $(3,3)$ | $10\cdot(-0.495)\cdot(-0.495) + 10\cdot0.495\cdot0.495$ = $2.45025 + 2.45025$ | $4.9005$ |

**Hasil:**

```math
SSP_{fac2} = \begin{bmatrix}0.7605&0.6825&1.9305\\0.6825&0.6125&1.7325\\1.9305&1.7325&4.9005\end{bmatrix}
```

**Pemeriksaan:** dimensi $3\times3$ ✓, simetris ✓.

**Penggunaan hasil:** dipakai pada Langkah 14: $\vert{}SSP_{fac2}+SSP_{res}\vert{}$ (penyebut $\Lambda_2^{\ast}$).

### Langkah 8 — Hitung $SSP_{int}$ (interaksi)

**Tujuan:** mengukur bagian rata-rata sel yang *tidak* bisa dijelaskan oleh penjumlahan efek faktor 1 dan faktor 2 saja. Jika tidak ada interaksi, rata-rata sel $\approx$ rata-rata baris + rata-rata kolom − rata-rata keseluruhan, sehingga selisihnya mendekati nol.

**Rumus:** $SSP_{int} = \sum_{\ell=1}^{g}\sum_{k=1}^{b} n\,(\bar{\mathbf{x}}_{\ell k}-\bar{\mathbf{x}}_{\ell\cdot}-\bar{\mathbf{x}}_{\cdot k}+\bar{\mathbf{x}})(\bar{\mathbf{x}}_{\ell k}-\bar{\mathbf{x}}_{\ell\cdot}-\bar{\mathbf{x}}_{\cdot k}+\bar{\mathbf{x}})'$

**Keterangan:** dari coretan modul, $\bar{\mathbf{x}}_{\ell k}$ rata-rata sel (Langkah 2), $\bar{\mathbf{x}}_{\ell\cdot}$ rata-rata baris (Langkah 3), $\bar{\mathbf{x}}_{\cdot k}$ rata-rata kolom (Langkah 4), $\bar{\mathbf{x}}$ rata-rata keseluruhan (Langkah 5). Ada $g\cdot b = 4$ suku (satu per sel), pengali $n=5$. Derajat kebebasan $(g-1)(b-1)=1$.

**Substitusi vektor selisih per sel** ($\mathbf{s}_{\ell k} = \bar{\mathbf{x}}_{\ell k}-\bar{\mathbf{x}}_{\ell\cdot}-\bar{\mathbf{x}}_{\cdot k}+\bar{\mathbf{x}}$):

| Sel | Variabel | Substitusi | $\mathbf{s}_{\ell k}$ |
|---|---|---|---|
| $(1,1)$ | $x_1$ | $6.3 - 6.49 - 6.59 + 6.785$ | $0.005$ |
| $(1,1)$ | $x_2$ | $9.56 - 9.57 - 9.14 + 9.315$ | $0.165$ |
| $(1,1)$ | $x_3$ | $3.74 - 3.79 - 3.44 + 3.935$ | $0.445$ |
| $(1,2)$ | $x_1$ | $6.68 - 6.49 - 6.98 + 6.785$ | $-0.005$ |
| $(1,2)$ | $x_2$ | $9.58 - 9.57 - 9.49 + 9.315$ | $-0.165$ |
| $(1,2)$ | $x_3$ | $3.84 - 3.79 - 4.43 + 3.935$ | $-0.445$ |
| $(2,1)$ | $x_1$ | $6.88 - 7.08 - 6.59 + 6.785$ | $-0.005$ |
| $(2,1)$ | $x_2$ | $8.72 - 9.06 - 9.14 + 9.315$ | $-0.165$ |
| $(2,1)$ | $x_3$ | $3.14 - 4.08 - 3.44 + 3.935$ | $-0.445$ |
| $(2,2)$ | $x_1$ | $7.28 - 7.08 - 6.98 + 6.785$ | $0.005$ |
| $(2,2)$ | $x_2$ | $9.4 - 9.06 - 9.49 + 9.315$ | $0.165$ |
| $(2,2)$ | $x_3$ | $5.02 - 4.08 - 4.43 + 3.935$ | $0.445$ |

Keempat vektor ini hanya berbeda tanda: $\mathbf{s}_{11}=\mathbf{s}_{22}=\begin{bmatrix}0.005\\0.165\\0.445\end{bmatrix}$ dan $\mathbf{s}_{12}=\mathbf{s}_{21}=-\mathbf{s}_{11}$. Ini sifat tabel $2\times2$. Karena itu keempat hasil kali $\mathbf{s}\mathbf{s}'$ sama.

**Perhitungan:** $SSP_{int} = 5\,\mathbf{s}_{11}\mathbf{s}_{11}' + 5\,\mathbf{s}_{12}\mathbf{s}_{12}' + 5\,\mathbf{s}_{21}\mathbf{s}_{21}' + 5\,\mathbf{s}_{22}\mathbf{s}_{22}'$. Suku berurutan: sel (1,1), (1,2), (2,1), (2,2):

| Elemen | Substitusi (jumlah dari semua suku) | Hasil |
|---|---|---|
| $(1,1)$ | $5\cdot0.005\cdot0.005 + 5\cdot(-0.005)\cdot(-0.005) + 5\cdot(-0.005)\cdot(-0.005) + 5\cdot0.005\cdot0.005$ = $0.000125 + 0.000125 + 0.000125 + 0.000125$ | $0.0005$ |
| $(1,2)$ | $5\cdot0.005\cdot0.165 + 5\cdot(-0.005)\cdot(-0.165) + 5\cdot(-0.005)\cdot(-0.165) + 5\cdot0.005\cdot0.165$ = $0.004125 + 0.004125 + 0.004125 + 0.004125$ | $0.0165$ |
| $(1,3)$ | $5\cdot0.005\cdot0.445 + 5\cdot(-0.005)\cdot(-0.445) + 5\cdot(-0.005)\cdot(-0.445) + 5\cdot0.005\cdot0.445$ = $0.011125 + 0.011125 + 0.011125 + 0.011125$ | $0.0445$ |
| $(2,2)$ | $5\cdot0.165\cdot0.165 + 5\cdot(-0.165)\cdot(-0.165) + 5\cdot(-0.165)\cdot(-0.165) + 5\cdot0.165\cdot0.165$ = $0.136125 + 0.136125 + 0.136125 + 0.136125$ | $0.5445$ |
| $(2,3)$ | $5\cdot0.165\cdot0.445 + 5\cdot(-0.165)\cdot(-0.445) + 5\cdot(-0.165)\cdot(-0.445) + 5\cdot0.165\cdot0.445$ = $0.367125 + 0.367125 + 0.367125 + 0.367125$ | $1.4685$ |
| $(3,3)$ | $5\cdot0.445\cdot0.445 + 5\cdot(-0.445)\cdot(-0.445) + 5\cdot(-0.445)\cdot(-0.445) + 5\cdot0.445\cdot0.445$ = $0.990125 + 0.990125 + 0.990125 + 0.990125$ | $3.9605$ |

**Hasil:**

```math
SSP_{int} = \begin{bmatrix}0.0005&0.0165&0.0445\\0.0165&0.5445&1.4685\\0.0445&1.4685&3.9605\end{bmatrix}
```

**Pemeriksaan:** $3\times3$, simetris ✓. Elemen-elemennya sangat kecil (terbesar 3.9605 pada elemen (3,3), lainnya lebih kecil), tanda awal bahwa interaksi lemah.

**Penggunaan hasil:** dipakai pada Langkah 12 untuk $\vert{}SSP_{int}+SSP_{res}\vert{}$.

### Langkah 9 — Hitung $SSP_{res}$ (galat/residual)

**Tujuan:** mengukur keragaman data di dalam sel (galat acak). Matriks galat; muncul di pembilang dan penyebut semua $\Lambda^{\ast}$.

**Rumus:** $SSP_{res} = \sum_{\ell=1}^{g}\sum_{k=1}^{b}\sum_{r=1}^{n}(\mathbf{x}_{\ell kr}-\bar{\mathbf{x}}_{\ell k})(\mathbf{x}_{\ell kr}-\bar{\mathbf{x}}_{\ell k})'$

**Keterangan:** untuk setiap dari 20 pengamatan, kurangkan rata-rata selnya (Langkah 2) menjadi vektor $\mathbf{d}=[d_1;d_2;d_3]'$, lalu bentuk $\mathbf{d}\mathbf{d}'$ (matriks $3\times3$ dengan elemen $d_id_j$). Jumlahkan 20 matriks itu. Derajat kebebasan $gb(n-1)=16$.

**Substitusi dan perhitungan:** di tabel berikut, $d_j = x_j - \bar{x}_{\ell k,j}$. Tiap baris menghitung $d_1^2,d_2^2,d_3^2,d_1d_2,d_1d_3,d_2d_3$; baris Jumlah menjumlahkan lima baris dalam sel. (Kolom $d_1d_2$ dll. adalah elemen $(1,2)$, $(1,3)$, $(2,3)$ dari $\mathbf{d}\mathbf{d}'$.)

**Sel $(l=1,k=1)$ — Faktor 1 = Low (−10%), Faktor 2 = Low (1.0%)**, pusat pengurangan $\mathbf{c}=\begin{bmatrix}6.3\\9.56\\3.74\end{bmatrix}$

| $r$ | $x_1$ | $x_2$ | $x_3$ | $d_1$ | $d_2$ | $d_3$ | $d_1^2$ | $d_2^2$ | $d_3^2$ | $d_1d_2$ | $d_1d_3$ | $d_2d_3$ |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | 6.5 | 9.5 | 4.4 | 0.2 | -0.06 | 0.66 | 0.04 | 0.0036 | 0.4356 | -0.012 | 0.132 | -0.0396 |
| 2 | 6.2 | 9.9 | 6.4 | -0.1 | 0.34 | 2.66 | 0.01 | 0.1156 | 7.0756 | -0.034 | -0.266 | 0.9044 |
| 3 | 5.8 | 9.6 | 3 | -0.5 | 0.04 | -0.74 | 0.25 | 0.0016 | 0.5476 | -0.02 | 0.37 | -0.0296 |
| 4 | 6.5 | 9.6 | 4.1 | 0.2 | 0.04 | 0.36 | 0.04 | 0.0016 | 0.1296 | 0.008 | 0.072 | 0.0144 |
| 5 | 6.5 | 9.2 | 0.8 | 0.2 | -0.36 | -2.94 | 0.04 | 0.1296 | 8.6436 | -0.072 | -0.588 | 1.0584 |
| **Jumlah** | | | | | | | **0.38** | **0.252** | **16.832** | **-0.13** | **-0.28** | **1.908** |

**Sel $(l=1,k=2)$ — Faktor 1 = Low (−10%), Faktor 2 = High (1.5%)**, pusat pengurangan $\mathbf{c}=\begin{bmatrix}6.68\\9.58\\3.84\end{bmatrix}$

| $r$ | $x_1$ | $x_2$ | $x_3$ | $d_1$ | $d_2$ | $d_3$ | $d_1^2$ | $d_2^2$ | $d_3^2$ | $d_1d_2$ | $d_1d_3$ | $d_2d_3$ |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | 6.9 | 9.1 | 5.7 | 0.22 | -0.48 | 1.86 | 0.0484 | 0.2304 | 3.4596 | -0.1056 | 0.4092 | -0.8928 |
| 2 | 7.2 | 10 | 2 | 0.52 | 0.42 | -1.84 | 0.2704 | 0.1764 | 3.3856 | 0.2184 | -0.9568 | -0.7728 |
| 3 | 6.9 | 9.9 | 3.9 | 0.22 | 0.32 | 0.06 | 0.0484 | 0.1024 | 0.0036 | 0.0704 | 0.0132 | 0.0192 |
| 4 | 6.1 | 9.5 | 1.9 | -0.58 | -0.08 | -1.94 | 0.3364 | 0.0064 | 3.7636 | 0.0464 | 1.1252 | 0.1552 |
| 5 | 6.3 | 9.4 | 5.7 | -0.38 | -0.18 | 1.86 | 0.1444 | 0.0324 | 3.4596 | 0.0684 | -0.7068 | -0.3348 |
| **Jumlah** | | | | | | | **0.848** | **0.548** | **14.072** | **0.298** | **-0.116** | **-1.826** |

**Sel $(l=2,k=1)$ — Faktor 1 = High (10%), Faktor 2 = Low (1.0%)**, pusat pengurangan $\mathbf{c}=\begin{bmatrix}6.88\\8.72\\3.14\end{bmatrix}$

| $r$ | $x_1$ | $x_2$ | $x_3$ | $d_1$ | $d_2$ | $d_3$ | $d_1^2$ | $d_2^2$ | $d_3^2$ | $d_1d_2$ | $d_1d_3$ | $d_2d_3$ |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | 6.7 | 9.1 | 2.8 | -0.18 | 0.38 | -0.34 | 0.0324 | 0.1444 | 0.1156 | -0.0684 | 0.0612 | -0.1292 |
| 2 | 6.6 | 9.3 | 4.1 | -0.28 | 0.58 | 0.96 | 0.0784 | 0.3364 | 0.9216 | -0.1624 | -0.2688 | 0.5568 |
| 3 | 7.2 | 8.3 | 3.8 | 0.32 | -0.42 | 0.66 | 0.1024 | 0.1764 | 0.4356 | -0.1344 | 0.2112 | -0.2772 |
| 4 | 7.1 | 8.4 | 1.6 | 0.22 | -0.32 | -1.54 | 0.0484 | 0.1024 | 2.3716 | -0.0704 | -0.3388 | 0.4928 |
| 5 | 6.8 | 8.5 | 3.4 | -0.08 | -0.22 | 0.26 | 0.0064 | 0.0484 | 0.0676 | 0.0176 | -0.0208 | -0.0572 |
| **Jumlah** | | | | | | | **0.268** | **0.808** | **3.912** | **-0.418** | **-0.356** | **0.586** |

**Sel $(l=2,k=2)$ — Faktor 1 = High (10%), Faktor 2 = High (1.5%)**, pusat pengurangan $\mathbf{c}=\begin{bmatrix}7.28\\9.4\\5.02\end{bmatrix}$

| $r$ | $x_1$ | $x_2$ | $x_3$ | $d_1$ | $d_2$ | $d_3$ | $d_1^2$ | $d_2^2$ | $d_3^2$ | $d_1d_2$ | $d_1d_3$ | $d_2d_3$ |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | 7.1 | 9.2 | 8.4 | -0.18 | -0.2 | 3.38 | 0.0324 | 0.04 | 11.4244 | 0.036 | -0.6084 | -0.676 |
| 2 | 7 | 8.8 | 5.2 | -0.28 | -0.6 | 0.18 | 0.0784 | 0.36 | 0.0324 | 0.168 | -0.0504 | -0.108 |
| 3 | 7.2 | 9.7 | 6.9 | -0.08 | 0.3 | 1.88 | 0.0064 | 0.09 | 3.5344 | -0.024 | -0.1504 | 0.564 |
| 4 | 7.5 | 10.1 | 2.7 | 0.22 | 0.7 | -2.32 | 0.0484 | 0.49 | 5.3824 | 0.154 | -0.5104 | -1.624 |
| 5 | 7.6 | 9.2 | 1.9 | 0.32 | -0.2 | -3.12 | 0.1024 | 0.04 | 9.7344 | -0.064 | -0.9984 | 0.624 |
| **Jumlah** | | | | | | | **0.268** | **1.02** | **30.108** | **0.27** | **-2.318** | **-1.22** |

**Menjumlahkan keempat sel** (setiap elemen = jumlah baris “Jumlah” dari 4 sel):

| Elemen | Sel (1,1) | Sel (1,2) | Sel (2,1) | Sel (2,2) | Jumlah $=SSP_{res}$ |
|---|---|---|---|---|---|
| $(1,1)=\sum d_1^2$ | 0.38 | 0.848 | 0.268 | 0.268 | **1.764** |
| $(1,2)=\sum d_1d_2$ | -0.13 | 0.298 | -0.418 | 0.27 | **0.02** |
| $(1,3)=\sum d_1d_3$ | -0.28 | -0.116 | -0.356 | -2.318 | **-3.07** |
| $(2,2)=\sum d_2^2$ | 0.252 | 0.548 | 0.808 | 1.02 | **2.628** |
| $(2,3)=\sum d_2d_3$ | 1.908 | -1.826 | 0.586 | -1.22 | **-0.552** |
| $(3,3)=\sum d_3^2$ | 16.832 | 14.072 | 3.912 | 30.108 | **64.924** |

**Hasil:**

```math
SSP_{res} = \begin{bmatrix}1.764&0.02&-3.07\\0.02&2.628&-0.552\\-3.07&-0.552&64.924\end{bmatrix}
```

**Pemeriksaan:** $3\times3$, simetris ✓. Elemen (3,3) = 64.924 jauh lebih besar daripada yang lain karena $x_3$ (opacity) sangat bervariasi antar-ulangan (mis. 0.8 sampai 6.4 pada sel (1,1)).

**Penggunaan hasil:** $SSP_{res}$ dipakai di semua uji: sebagai pembilang $\vert{}SSP_{res}\vert{}$ dan dijumlahkan dengan $SSP$ efek untuk penyebut.

### Langkah 10 — Hitung $SSP_{cor}$ dan verifikasi penguraian

**Tujuan:** modul mencantumkan $SSP_{cor}$ (total terkoreksi) di tabel MANOVA. Ia dipakai untuk memeriksa bahwa $SSP_{fac1}$, $SSP_{fac2}$, $SSP_{int}$, $SSP_{res}$ yang kita hitung konsisten: jumlahnya harus sama dengan $SSP_{cor}$.

**Rumus:** $SSP_{cor} = \sum_{\ell}\sum_{k}\sum_{r}(\mathbf{x}_{\ell kr}-\bar{\mathbf{x}})(\mathbf{x}_{\ell kr}-\bar{\mathbf{x}})'$, derajat kebebasan $gbn-1=19$. Identitas penguraian: $SSP_{cor}=SSP_{fac1}+SSP_{fac2}+SSP_{int}+SSP_{res}$.

**Substitusi dan perhitungan:** sekarang pusat pengurangan adalah rata-rata keseluruhan $\bar{\mathbf{x}}$ untuk semua sel, $d_j = x_j-\bar{x}_j$:

**Sel $(l=1,k=1)$ — Faktor 1 = Low (−10%), Faktor 2 = Low (1.0%)**, pusat pengurangan $\mathbf{c}=\begin{bmatrix}6.785\\9.315\\3.935\end{bmatrix}$

| $r$ | $x_1$ | $x_2$ | $x_3$ | $d_1$ | $d_2$ | $d_3$ | $d_1^2$ | $d_2^2$ | $d_3^2$ | $d_1d_2$ | $d_1d_3$ | $d_2d_3$ |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | 6.5 | 9.5 | 4.4 | -0.285 | 0.185 | 0.465 | 0.081225 | 0.034225 | 0.216225 | -0.052725 | -0.132525 | 0.086025 |
| 2 | 6.2 | 9.9 | 6.4 | -0.585 | 0.585 | 2.465 | 0.342225 | 0.342225 | 6.076225 | -0.342225 | -1.442025 | 1.442025 |
| 3 | 5.8 | 9.6 | 3 | -0.985 | 0.285 | -0.935 | 0.970225 | 0.081225 | 0.874225 | -0.280725 | 0.920975 | -0.266475 |
| 4 | 6.5 | 9.6 | 4.1 | -0.285 | 0.285 | 0.165 | 0.081225 | 0.081225 | 0.027225 | -0.081225 | -0.047025 | 0.047025 |
| 5 | 6.5 | 9.2 | 0.8 | -0.285 | -0.115 | -3.135 | 0.081225 | 0.013225 | 9.828225 | 0.032775 | 0.893475 | 0.360525 |
| **Jumlah** | | | | | | | **1.556125** | **0.552125** | **17.022125** | **-0.724125** | **0.192875** | **1.669125** |

**Sel $(l=1,k=2)$ — Faktor 1 = Low (−10%), Faktor 2 = High (1.5%)**, pusat pengurangan $\mathbf{c}=\begin{bmatrix}6.785\\9.315\\3.935\end{bmatrix}$

| $r$ | $x_1$ | $x_2$ | $x_3$ | $d_1$ | $d_2$ | $d_3$ | $d_1^2$ | $d_2^2$ | $d_3^2$ | $d_1d_2$ | $d_1d_3$ | $d_2d_3$ |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | 6.9 | 9.1 | 5.7 | 0.115 | -0.215 | 1.765 | 0.013225 | 0.046225 | 3.115225 | -0.024725 | 0.202975 | -0.379475 |
| 2 | 7.2 | 10 | 2 | 0.415 | 0.685 | -1.935 | 0.172225 | 0.469225 | 3.744225 | 0.284275 | -0.803025 | -1.325475 |
| 3 | 6.9 | 9.9 | 3.9 | 0.115 | 0.585 | -0.035 | 0.013225 | 0.342225 | 0.001225 | 0.067275 | -0.004025 | -0.020475 |
| 4 | 6.1 | 9.5 | 1.9 | -0.685 | 0.185 | -2.035 | 0.469225 | 0.034225 | 4.141225 | -0.126725 | 1.393975 | -0.376475 |
| 5 | 6.3 | 9.4 | 5.7 | -0.485 | 0.085 | 1.765 | 0.235225 | 0.007225 | 3.115225 | -0.041225 | -0.856025 | 0.150025 |
| **Jumlah** | | | | | | | **0.903125** | **0.899125** | **14.117125** | **0.158875** | **-0.066125** | **-1.951875** |

**Sel $(l=2,k=1)$ — Faktor 1 = High (10%), Faktor 2 = Low (1.0%)**, pusat pengurangan $\mathbf{c}=\begin{bmatrix}6.785\\9.315\\3.935\end{bmatrix}$

| $r$ | $x_1$ | $x_2$ | $x_3$ | $d_1$ | $d_2$ | $d_3$ | $d_1^2$ | $d_2^2$ | $d_3^2$ | $d_1d_2$ | $d_1d_3$ | $d_2d_3$ |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | 6.7 | 9.1 | 2.8 | -0.085 | -0.215 | -1.135 | 0.007225 | 0.046225 | 1.288225 | 0.018275 | 0.096475 | 0.244025 |
| 2 | 6.6 | 9.3 | 4.1 | -0.185 | -0.015 | 0.165 | 0.034225 | 0.000225 | 0.027225 | 0.002775 | -0.030525 | -0.002475 |
| 3 | 7.2 | 8.3 | 3.8 | 0.415 | -1.015 | -0.135 | 0.172225 | 1.030225 | 0.018225 | -0.421225 | -0.056025 | 0.137025 |
| 4 | 7.1 | 8.4 | 1.6 | 0.315 | -0.915 | -2.335 | 0.099225 | 0.837225 | 5.452225 | -0.288225 | -0.735525 | 2.136525 |
| 5 | 6.8 | 8.5 | 3.4 | 0.015 | -0.815 | -0.535 | 0.000225 | 0.664225 | 0.286225 | -0.012225 | -0.008025 | 0.436025 |
| **Jumlah** | | | | | | | **0.313125** | **2.578125** | **7.072125** | **-0.700625** | **-0.733625** | **2.951125** |

**Sel $(l=2,k=2)$ — Faktor 1 = High (10%), Faktor 2 = High (1.5%)**, pusat pengurangan $\mathbf{c}=\begin{bmatrix}6.785\\9.315\\3.935\end{bmatrix}$

| $r$ | $x_1$ | $x_2$ | $x_3$ | $d_1$ | $d_2$ | $d_3$ | $d_1^2$ | $d_2^2$ | $d_3^2$ | $d_1d_2$ | $d_1d_3$ | $d_2d_3$ |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | 7.1 | 9.2 | 8.4 | 0.315 | -0.115 | 4.465 | 0.099225 | 0.013225 | 19.936225 | -0.036225 | 1.406475 | -0.513475 |
| 2 | 7 | 8.8 | 5.2 | 0.215 | -0.515 | 1.265 | 0.046225 | 0.265225 | 1.600225 | -0.110725 | 0.271975 | -0.651475 |
| 3 | 7.2 | 9.7 | 6.9 | 0.415 | 0.385 | 2.965 | 0.172225 | 0.148225 | 8.791225 | 0.159775 | 1.230475 | 1.141525 |
| 4 | 7.5 | 10.1 | 2.7 | 0.715 | 0.785 | -1.235 | 0.511225 | 0.616225 | 1.525225 | 0.561275 | -0.883025 | -0.969475 |
| 5 | 7.6 | 9.2 | 1.9 | 0.815 | -0.115 | -2.035 | 0.664225 | 0.013225 | 4.141225 | -0.093725 | -1.658525 | 0.234025 |
| **Jumlah** | | | | | | | **1.493125** | **1.056125** | **35.994125** | **0.480375** | **0.367375** | **-0.758875** |

**Menjumlahkan keempat sel:**

| Elemen | Sel (1,1) | Sel (1,2) | Sel (2,1) | Sel (2,2) | Jumlah $=SSP_{cor}$ |
|---|---|---|---|---|---|
| $(1,1)=\sum d_1^2$ | 1.556125 | 0.903125 | 0.313125 | 1.493125 | **4.2655** |
| $(1,2)=\sum d_1d_2$ | -0.724125 | 0.158875 | -0.700625 | 0.480375 | **-0.7855** |
| $(1,3)=\sum d_1d_3$ | 0.192875 | -0.066125 | -0.733625 | 0.367375 | **-0.2395** |
| $(2,2)=\sum d_2^2$ | 0.552125 | 0.899125 | 2.578125 | 1.056125 | **5.0855** |
| $(2,3)=\sum d_2d_3$ | 1.669125 | -1.951875 | 2.951125 | -0.758875 | **1.9095** |
| $(3,3)=\sum d_3^2$ | 17.022125 | 14.117125 | 7.072125 | 35.994125 | **74.2055** |

**Hasil:**

```math
SSP_{cor} = \begin{bmatrix}4.2655&-0.7855&-0.2395\\-0.7855&5.0855&1.9095\\-0.2395&1.9095&74.2055\end{bmatrix}
```

**Verifikasi identitas** (elemen demi elemen):

| Elemen | $SSP_{fac1}$ | $SSP_{fac2}$ | $SSP_{int}$ | $SSP_{res}$ | Jumlah | $SSP_{cor}$ | Cocok? |
|---|---|---|---|---|---|---|---|
| (1,1) | 1.7405 | 0.7605 | 0.0005 | 1.764 | 4.2655 | 4.2655 | ✓ |
| (1,2) | -1.5045 | 0.6825 | 0.0165 | 0.02 | -0.7855 | -0.7855 | ✓ |
| (1,3) | 0.8555 | 1.9305 | 0.0445 | -3.07 | -0.2395 | -0.2395 | ✓ |
| (2,2) | 1.3005 | 0.6125 | 0.5445 | 2.628 | 5.0855 | 5.0855 | ✓ |
| (2,3) | -0.7395 | 1.7325 | 1.4685 | -0.552 | 1.9095 | 1.9095 | ✓ |
| (3,3) | 0.4205 | 4.9005 | 3.9605 | 64.924 | 74.2055 | 74.2055 | ✓ |

Semua enam elemen cocok persis, jadi Langkah 6–9 benar. Derajat kebebasan juga cocok: $1+1+1+16=19=gbn-1$ ✓.

**Penggunaan hasil:** hanya untuk verifikasi dan ditampilkan di tabel MANOVA; tidak dipakai pada statistik uji.

### Langkah 11 — Tabel MANOVA Dua Arah (rekap)

**Tujuan:** menyusun semua hasil dalam satu tabel seperti di modul.

| Sumber variasi | Matriks $SSP$ | Derajat kebebasan | Rumus df | Nilai df |
|---|---|---|---|---|
| Faktor 1 | $\begin{bmatrix}1.7405&-1.5045&0.8555\\-1.5045&1.3005&-0.7395\\0.8555&-0.7395&0.4205\end{bmatrix}$ | $g-1$ | $2-1$ | 1 |
| Faktor 2 | $\begin{bmatrix}0.7605&0.6825&1.9305\\0.6825&0.6125&1.7325\\1.9305&1.7325&4.9005\end{bmatrix}$ | $b-1$ | $2-1$ | 1 |
| Interaksi | $\begin{bmatrix}0.0005&0.0165&0.0445\\0.0165&0.5445&1.4685\\0.0445&1.4685&3.9605\end{bmatrix}$ | $(g-1)(b-1)$ | $(2-1)(2-1)$ | 1 |
| Residual (galat) | $\begin{bmatrix}1.7640&0.0200&-3.0700\\0.0200&2.6280&-0.5520\\-3.0700&-0.5520&64.9240\end{bmatrix}$ | $gb(n-1)$ | $2\cdot2\cdot4$ | 16 |
| Total (corrected) | $\begin{bmatrix}4.2655&-0.7855&-0.2395\\-0.7855&5.0855&1.9095\\-0.2395&1.9095&74.2055\end{bmatrix}$ | $gbn-1$ | $20-1$ | 19 |

**Penggunaan hasil:** Langkah 12–14 hanya memakai baris Faktor 1, Faktor 2, Interaksi, dan Residual.

---

### Langkah 12 — Uji Interaksi Faktor 1 × Faktor 2

**1. Hipotesis**

$H_0:\ (\tau\beta)_{11}=(\tau\beta)_{12}=(\tau\beta)_{21}=(\tau\beta)_{22}=0$ (atau $\boldsymbol\gamma_{11}=\boldsymbol\gamma_{12}=\boldsymbol\gamma_{21}=\boldsymbol\gamma_{22}=\mathbf{0}$): tidak ada pengaruh interaksi

$H_1:$ minimal ada satu $\boldsymbol\gamma_{\ell k}\neq\mathbf{0}$ (ada pengaruh interaksi)

**2. Komponen yang perlu dihitung:** $\vert{}SSP_{res}\vert{}$ (pembilang) dan $\vert{}SSP_{int}+SSP_{res}\vert{}$ (penyebut). Keduanya determinan matriks $3\times3$ yang sudah kita punya dari Langkah 6–9. Determinan dipakai karena Wilks' Lambda adalah rasio “volume” sebaran galat terhadap volume sebaran galat ditambah efek; determinan matriks kovarians-jumlah kuadrat adalah ukuran volume tersebut untuk beberapa variabel sekaligus.

**3. Rumus yang digunakan**

```math
\Lambda^{\ast} = \dfrac{\vert{}SSP_{res}\vert{}}{\vert{}SSP_{int}+SSP_{res}\vert{}},\qquad F=\left(\dfrac{1-\Lambda^{\ast}}{\Lambda^{\ast}}\right)\dfrac{\big(gb(n-1)-p+1\big)/2}{\big(\,\vert{}(g-1)(b-1)-p\vert{}+1\big)/2}
```

dengan $\nu_1 = \vert{}(g-1)(b-1)-p\vert{}+1$ dan $\nu_2 = gb(n-1)-p+1$. Rumus ini dipakai karena derajat kebebasan efek $(g-1)(b-1)=1$, sesuai syarat di modul (“For ... $=1$”).

**4. Substitusi dan perhitungan**

**4a. Determinan pembilang** $\vert{}SSP_{res}\vert{}$ (matriks dari Langkah 9):

Matriks yang dihitung determinannya: $SSP_{res} = \begin{bmatrix}1.764&0.02&-3.07\\0.02&2.628&-0.552\\-3.07&-0.552&64.924\end{bmatrix}$ (ukuran $3\times3$, simetris). Tulis elemen-elemennya sebagai $\begin{bmatrix}a&b&c\\b&e&f\\c&f&i\end{bmatrix}$ dengan $a=1.764$, $b=0.02$, $c=-3.07$, $e=2.628$, $f=-0.552$, $i=64.924$.

Rumus (ekspansi kofaktor sepanjang baris pertama): $\vert{}\,\cdot\,\vert{} = a(ei-f^2) - b(bi-fc) + c(bf-ec)$

| Bagian | Substitusi | Hasil |
|---|---|---|
| $ei-f^2$ | $(2.628)(64.924) - (-0.552)^2 = 170.620272 - 0.304704$ | $170.315568$ |
| $bi-fc$ | $(0.02)(64.924) - (-0.552)(-3.07) = 1.29848 - 1.69464$ | $-0.39616$ |
| $bf-ec$ | $(0.02)(-0.552) - (2.628)(-3.07) = (-0.01104) - (-8.06796)$ | $8.05692$ |

```math
\vert{}SSP_{res}\vert{} = a\cdot m_1 - b\cdot m_2 + c\cdot m_3 = (1.764)(170.315568) - (0.02)(-0.39616) + (-3.07)(8.05692) = 300.436661952 + 0.0079232 + (-24.7347444) = \mathbf{275.709840752}
```

Nilai ini dipakai sebagai pembilang di semua uji. Modul memuat nilai yang sama: 275.7098 ✓.

**4b. Matriks penyebut** $SSP_{int}+SSP_{res}$:

```math
SSP_{int}+SSP_{res} = \begin{bmatrix}0.0005&0.0165&0.0445\\0.0165&0.5445&1.4685\\0.0445&1.4685&3.9605\end{bmatrix} + \begin{bmatrix}1.7640&0.0200&-3.0700\\0.0200&2.6280&-0.5520\\-3.0700&-0.5520&64.9240\end{bmatrix} = \begin{bmatrix}1.7645&0.0365&-3.0255\\0.0365&3.1725&0.9165\\-3.0255&0.9165&68.8845\end{bmatrix}
```

Penjumlahan matriks dilakukan elemen demi elemen (dimensi sama, $3\times3$ + $3\times3$). Contoh: elemen (1,1) = 0.0005 + 1.764 = 1.7645; elemen (3,3) = 3.9605 + 64.924 = 68.8845.

**4c. Determinan penyebut:**

Matriks yang dihitung determinannya: $SSP_{int}+SSP_{res} = \begin{bmatrix}1.7645&0.0365&-3.0255\\0.0365&3.1725&0.9165\\-3.0255&0.9165&68.8845\end{bmatrix}$ (ukuran $3\times3$, simetris). Tulis elemen-elemennya sebagai $\begin{bmatrix}a&b&c\\b&e&f\\c&f&i\end{bmatrix}$ dengan $a=1.7645$, $b=0.0365$, $c=-3.0255$, $e=3.1725$, $f=0.9165$, $i=68.8845$.

Rumus (ekspansi kofaktor sepanjang baris pertama): $\vert{}\,\cdot\,\vert{} = a(ei-f^2) - b(bi-fc) + c(bf-ec)$

| Bagian | Substitusi | Hasil |
|---|---|---|
| $ei-f^2$ | $(3.1725)(68.8845) - (0.9165)^2 = 218.53607625 - 0.83997225$ | $217.696104$ |
| $bi-fc$ | $(0.0365)(68.8845) - (0.9165)(-3.0255) = 2.51428425 - (-2.77287075)$ | $5.287155$ |
| $bf-ec$ | $(0.0365)(0.9165) - (3.1725)(-3.0255) = 0.03345225 - (-9.59839875)$ | $9.631851$ |

```math
\vert{}SSP_{int}+SSP_{res}\vert{} = a\cdot m_1 - b\cdot m_2 + c\cdot m_3 = (1.7645)(217.696104) - (0.0365)(5.287155) + (-3.0255)(9.631851) = 384.124775508 + (-0.1929811575) + (-29.1411652005) = \mathbf{354.79062915}
```

**4d. Wilks' Lambda:**

```math
\Lambda^{\ast} = \dfrac{275.709840752}{354.79062915} = 0.7771\ (\text{pembulatan 4 desimal})
```

**4e. Konversi ke $F$** — hitung komponen satu per satu:

| Komponen | Perhitungan | Hasil |
|---|---|---|
| $\nu_1$ | $\vert{}(g-1)(b-1) - p\vert{} + 1 = \vert{}1 - 3\vert{} + 1$ | 3 |
| $\nu_2$ | $gb(n-1)-p+1 = 2(2)(4) - 3 + 1 = 16-3+1$ | 14 |
| $(1-\Lambda^{\ast})/\Lambda^{\ast}$ | $(1-0.7771)/0.7771 = 0.2229/0.7771$ | 0.28684 |
| $\dfrac{\nu_2/2}{\nu_1/2}$ | $\dfrac{14/2}{3/2} = \dfrac{7}{1.5}$ | 4.66667 |
| $F$ | 0.28684 $\times$ 4.66667 | **1.3386** |

Jadi $F_{hitung} = 1.3386 \approx \mathbf{1.34}$ (modul: $\Lambda^{\ast}=0.7771$, $F=1.34$, $\nu_1=3$, $\nu_2=14$).

**5. Nilai kritis dari Tabel $F$** (modul menamai $F_{tabel}$, simbol $F_{\nu_1,\nu_2}(\alpha)$):

- Tabel yang dipakai: tabel distribusi $F$ untuk $\alpha = 0.05$ (taraf nyata 5%, artinya kita bersedia salah menolak $H_0$ paling banyak 5%).
- Derajat kebebasan pembilang $\nu_1 = 3$ → pilih **kolom** bernomor 3.
- Derajat kebebasan penyebut $\nu_2 = 14$ → pilih **baris** bernomor 14.
- Nilai di perpotongan kolom 3 dan baris 14: $F_{3,14}(.05) = 3.34$ (nilai dari tabel, sama dengan nilai di modul). Perangkat lunak memberi 3.3439, yang dibulatkan menjadi 3.34.

**6. Aturan keputusan (modul):** tolak $H_0$ jika $F_{hitung} > F_{tabel}$; jika tidak, gagal menolak $H_0$.

**7. Keputusan:** $F_{hitung} = 1.34$ < $F_{tabel} = 3.34$, sehingga **$H_0$ gagal ditolak (tidak ditolak)**.

**Nilai-p (opsional, dari perangkat lunak, bukan dari tabel manual):** dari distribusi $F$ dengan $\nu_1=3$, $\nu_2=14$ dan nilai $F$ eksak 1.3385 (tanpa pembulatan $\Lambda^{\ast}$), diperoleh $p = 0.3018$. Karena $p$ > 0.05, keputusannya sama. Nilai-p adalah peluang mendapat $F$ sebesar atau lebih besar dari yang teramati jika $H_0$ benar; $p$ yang kecil berarti data jarang terjadi jika $H_0$ benar.

**8. Cek dengan pendekatan khi-kuadrat (modul menyebutnya sebagai cek tambahan):** statistik $-\left[gb(n-1)-\dfrac{p+1-(g-1)(b-1)}{2}\right]\ln\Lambda^{\ast}$ dibandingkan dengan $\chi^2_{p(g-1)(b-1)}(.05)$.

```math
-\left[2(2)(4)-\dfrac{3+1-1(1)}{2}\right]\ln(0.7771) = -(16-1.5)\ln(0.7771) = -14.5\times(-0.25219) = 3.6567
```

Modul menulis 3.66 ✓. Nilai kritis $\chi^2_3(.05)=7.81$ (derajat kebebasan $p(g-1)(b-1)=3$). Karena $3.66<7.81$, kesimpulannya sama dengan uji $F$: $H_0$ gagal ditolak.

**9. Interpretasi:** tidak ada bukti pada $\alpha=0.05$ bahwa interaksi antara laju ekstrusi dan jumlah aditif mempengaruhi ketiga respon. Artinya efek laju ekstrusi tidak bergantung pada level aditif (dan sebaliknya), yang mendukung model tanpa interaksi (additive).

**Kesimpulan uji interaksi:** gagal menolak $H_0$ ($F=1.34<3.34$).

---

### Langkah 13 — Uji Faktor 1 (Perubahan Laju Ekstrusi)

**1. Hipotesis**

$H_0:\ \boldsymbol\tau_1=\boldsymbol\tau_2=\mathbf{0}$ (tidak ada pengaruh faktor 1)

$H_1:$ minimal ada satu $\boldsymbol\tau_\ell\neq\mathbf{0}$, $\ell=1,2$

**2. Komponen yang perlu dihitung:** $\vert{}SSP_{res}\vert{}$ (pembilang) dan $\vert{}SSP_{fac1}+SSP_{res}\vert{}$ (penyebut). Keduanya determinan matriks $3\times3$ yang sudah kita punya dari Langkah 6–9. Determinan dipakai karena Wilks' Lambda adalah rasio “volume” sebaran galat terhadap volume sebaran galat ditambah efek; determinan matriks kovarians-jumlah kuadrat adalah ukuran volume tersebut untuk beberapa variabel sekaligus.

**3. Rumus yang digunakan**

```math
\Lambda^{\ast} = \dfrac{\vert{}SSP_{res}\vert{}}{\vert{}SSP_{fac1}+SSP_{res}\vert{}},\qquad F=\left(\dfrac{1-\Lambda^{\ast}}{\Lambda^{\ast}}\right)\dfrac{\big(gb(n-1)-p+1\big)/2}{\big(\,\vert{}(g-1)-p\vert{}+1\big)/2}
```

dengan $\nu_1 = \vert{}(g-1)-p\vert{}+1$ dan $\nu_2 = gb(n-1)-p+1$. Rumus ini dipakai karena derajat kebebasan efek $(g-1)=1$, sesuai syarat di modul (“For ... $=1$”).

**4. Substitusi dan perhitungan**

**4a. Determinan pembilang** $\vert{}SSP_{res}\vert{}$ (matriks dari Langkah 9):

Matriks yang dihitung determinannya: $SSP_{res} = \begin{bmatrix}1.764&0.02&-3.07\\0.02&2.628&-0.552\\-3.07&-0.552&64.924\end{bmatrix}$ (ukuran $3\times3$, simetris). Tulis elemen-elemennya sebagai $\begin{bmatrix}a&b&c\\b&e&f\\c&f&i\end{bmatrix}$ dengan $a=1.764$, $b=0.02$, $c=-3.07$, $e=2.628$, $f=-0.552$, $i=64.924$.

Rumus (ekspansi kofaktor sepanjang baris pertama): $\vert{}\,\cdot\,\vert{} = a(ei-f^2) - b(bi-fc) + c(bf-ec)$

| Bagian | Substitusi | Hasil |
|---|---|---|
| $ei-f^2$ | $(2.628)(64.924) - (-0.552)^2 = 170.620272 - 0.304704$ | $170.315568$ |
| $bi-fc$ | $(0.02)(64.924) - (-0.552)(-3.07) = 1.29848 - 1.69464$ | $-0.39616$ |
| $bf-ec$ | $(0.02)(-0.552) - (2.628)(-3.07) = (-0.01104) - (-8.06796)$ | $8.05692$ |

```math
\vert{}SSP_{res}\vert{} = a\cdot m_1 - b\cdot m_2 + c\cdot m_3 = (1.764)(170.315568) - (0.02)(-0.39616) + (-3.07)(8.05692) = 300.436661952 + 0.0079232 + (-24.7347444) = \mathbf{275.709840752}
```

Nilai ini dipakai sebagai pembilang di semua uji. Modul memuat nilai yang sama: 275.7098 ✓.

**4b. Matriks penyebut** $SSP_{fac1}+SSP_{res}$:

```math
SSP_{fac1}+SSP_{res} = \begin{bmatrix}1.7405&-1.5045&0.8555\\-1.5045&1.3005&-0.7395\\0.8555&-0.7395&0.4205\end{bmatrix} + \begin{bmatrix}1.7640&0.0200&-3.0700\\0.0200&2.6280&-0.5520\\-3.0700&-0.5520&64.9240\end{bmatrix} = \begin{bmatrix}3.5045&-1.4845&-2.2145\\-1.4845&3.9285&-1.2915\\-2.2145&-1.2915&65.3445\end{bmatrix}
```

Penjumlahan matriks dilakukan elemen demi elemen (dimensi sama, $3\times3$ + $3\times3$). Contoh: elemen (1,1) = 1.7405 + 1.764 = 3.5045; elemen (3,3) = 0.4205 + 64.924 = 65.3445.

**4c. Determinan penyebut:**

Matriks yang dihitung determinannya: $SSP_{fac1}+SSP_{res} = \begin{bmatrix}3.5045&-1.4845&-2.2145\\-1.4845&3.9285&-1.2915\\-2.2145&-1.2915&65.3445\end{bmatrix}$ (ukuran $3\times3$, simetris). Tulis elemen-elemennya sebagai $\begin{bmatrix}a&b&c\\b&e&f\\c&f&i\end{bmatrix}$ dengan $a=3.5045$, $b=-1.4845$, $c=-2.2145$, $e=3.9285$, $f=-1.2915$, $i=65.3445$.

Rumus (ekspansi kofaktor sepanjang baris pertama): $\vert{}\,\cdot\,\vert{} = a(ei-f^2) - b(bi-fc) + c(bf-ec)$

| Bagian | Substitusi | Hasil |
|---|---|---|
| $ei-f^2$ | $(3.9285)(65.3445) - (-1.2915)^2 = 256.70586825 - 1.66797225$ | $255.037896$ |
| $bi-fc$ | $(-1.4845)(65.3445) - (-1.2915)(-2.2145) = -97.00391025 - 2.86002675$ | $-99.863937$ |
| $bf-ec$ | $(-1.4845)(-1.2915) - (3.9285)(-2.2145) = 1.91723175 - (-8.69966325)$ | $10.616895$ |

```math
\vert{}SSP_{fac1}+SSP_{res}\vert{} = a\cdot m_1 - b\cdot m_2 + c\cdot m_3 = (3.5045)(255.037896) - (-1.4845)(-99.863937) + (-2.2145)(10.616895) = 893.780306532 + (-148.2480144765) + (-23.5111139775) = \mathbf{722.021178078}
```

**4d. Wilks' Lambda:**

```math
\Lambda^{\ast} = \dfrac{275.709840752}{722.021178078} = 0.3819\ (\text{pembulatan 4 desimal})
```

**4e. Konversi ke $F$** — hitung komponen satu per satu:

| Komponen | Perhitungan | Hasil |
|---|---|---|
| $\nu_1$ | $\vert{}(g-1) - p\vert{} + 1 = \vert{}1 - 3\vert{} + 1$ | 3 |
| $\nu_2$ | $gb(n-1)-p+1 = 2(2)(4) - 3 + 1 = 16-3+1$ | 14 |
| $(1-\Lambda^{\ast})/\Lambda^{\ast}$ | $(1-0.3819)/0.3819 = 0.6181/0.3819$ | 1.61849 |
| $\dfrac{\nu_2/2}{\nu_1/2}$ | $\dfrac{14/2}{3/2} = \dfrac{7}{1.5}$ | 4.66667 |
| $F$ | 1.61849 $\times$ 4.66667 | **7.5529** |

Jadi $F_{hitung} = 7.5529 \approx \mathbf{7.55}$ (modul: $\Lambda_1^{\ast}=.3819$, $F_1=7.55$, $\nu_1=3$, $\nu_2=14$).

**5. Nilai kritis dari Tabel $F$** (modul menamai $F_{tabel}$, simbol $F_{\nu_1,\nu_2}(\alpha)$):

- Tabel yang dipakai: tabel distribusi $F$ untuk $\alpha = 0.05$ (taraf nyata 5%, artinya kita bersedia salah menolak $H_0$ paling banyak 5%).
- Derajat kebebasan pembilang $\nu_1 = 3$ → pilih **kolom** bernomor 3.
- Derajat kebebasan penyebut $\nu_2 = 14$ → pilih **baris** bernomor 14.
- Nilai di perpotongan kolom 3 dan baris 14: $F_{3,14}(.05) = 3.34$ (nilai dari tabel, sama dengan nilai di modul). Perangkat lunak memberi 3.3439, yang dibulatkan menjadi 3.34.

**6. Aturan keputusan (modul):** tolak $H_0$ jika $F_{hitung} > F_{tabel}$; jika tidak, gagal menolak $H_0$.

**7. Keputusan:** $F_{hitung} = 7.55$ > $F_{tabel} = 3.34$, sehingga **$H_0$ ditolak**.

**Nilai-p (opsional, dari perangkat lunak, bukan dari tabel manual):** dari distribusi $F$ dengan $\nu_1=3$, $\nu_2=14$ dan nilai $F$ eksak 7.5543 (tanpa pembulatan $\Lambda^{\ast}$), diperoleh $p = 0.0030$. Karena $p$ < 0.05, keputusannya sama. Nilai-p adalah peluang mendapat $F$ sebesar atau lebih besar dari yang teramati jika $H_0$ benar; $p$ yang kecil berarti data jarang terjadi jika $H_0$ benar.

**8. Interpretasi:** laju ekstrusi (Low −10% vs High 10%) mempengaruhi ketiga respon secara bersama-sama pada $\alpha=0.05$.

**Kesimpulan uji faktor 1:** $H_0$ ditolak ($F_1=7.55>3.34$).

---

### Langkah 14 — Uji Faktor 2 (Jumlah Aditif)

**1. Hipotesis**

$H_0:\ \boldsymbol\beta_1=\boldsymbol\beta_2=\mathbf{0}$ (tidak ada pengaruh faktor 2)

$H_1:$ minimal ada satu $\boldsymbol\beta_k\neq\mathbf{0}$, $k=1,2$

**2. Komponen yang perlu dihitung:** $\vert{}SSP_{res}\vert{}$ (pembilang) dan $\vert{}SSP_{fac2}+SSP_{res}\vert{}$ (penyebut). Keduanya determinan matriks $3\times3$ yang sudah kita punya dari Langkah 6–9. Determinan dipakai karena Wilks' Lambda adalah rasio “volume” sebaran galat terhadap volume sebaran galat ditambah efek; determinan matriks kovarians-jumlah kuadrat adalah ukuran volume tersebut untuk beberapa variabel sekaligus.

**3. Rumus yang digunakan**

```math
\Lambda^{\ast} = \dfrac{\vert{}SSP_{res}\vert{}}{\vert{}SSP_{fac2}+SSP_{res}\vert{}},\qquad F=\left(\dfrac{1-\Lambda^{\ast}}{\Lambda^{\ast}}\right)\dfrac{\big(gb(n-1)-p+1\big)/2}{\big(\,\vert{}(b-1)-p\vert{}+1\big)/2}
```

dengan $\nu_1 = \vert{}(b-1)-p\vert{}+1$ dan $\nu_2 = gb(n-1)-p+1$. Rumus ini dipakai karena derajat kebebasan efek $(b-1)=1$, sesuai syarat di modul (“For ... $=1$”).

**4. Substitusi dan perhitungan**

**4a. Determinan pembilang** $\vert{}SSP_{res}\vert{}$ (matriks dari Langkah 9):

Matriks yang dihitung determinannya: $SSP_{res} = \begin{bmatrix}1.764&0.02&-3.07\\0.02&2.628&-0.552\\-3.07&-0.552&64.924\end{bmatrix}$ (ukuran $3\times3$, simetris). Tulis elemen-elemennya sebagai $\begin{bmatrix}a&b&c\\b&e&f\\c&f&i\end{bmatrix}$ dengan $a=1.764$, $b=0.02$, $c=-3.07$, $e=2.628$, $f=-0.552$, $i=64.924$.

Rumus (ekspansi kofaktor sepanjang baris pertama): $\vert{}\,\cdot\,\vert{} = a(ei-f^2) - b(bi-fc) + c(bf-ec)$

| Bagian | Substitusi | Hasil |
|---|---|---|
| $ei-f^2$ | $(2.628)(64.924) - (-0.552)^2 = 170.620272 - 0.304704$ | $170.315568$ |
| $bi-fc$ | $(0.02)(64.924) - (-0.552)(-3.07) = 1.29848 - 1.69464$ | $-0.39616$ |
| $bf-ec$ | $(0.02)(-0.552) - (2.628)(-3.07) = (-0.01104) - (-8.06796)$ | $8.05692$ |

```math
\vert{}SSP_{res}\vert{} = a\cdot m_1 - b\cdot m_2 + c\cdot m_3 = (1.764)(170.315568) - (0.02)(-0.39616) + (-3.07)(8.05692) = 300.436661952 + 0.0079232 + (-24.7347444) = \mathbf{275.709840752}
```

Nilai ini dipakai sebagai pembilang di semua uji. Modul memuat nilai yang sama: 275.7098 ✓.

**4b. Matriks penyebut** $SSP_{fac2}+SSP_{res}$:

```math
SSP_{fac2}+SSP_{res} = \begin{bmatrix}0.7605&0.6825&1.9305\\0.6825&0.6125&1.7325\\1.9305&1.7325&4.9005\end{bmatrix} + \begin{bmatrix}1.7640&0.0200&-3.0700\\0.0200&2.6280&-0.5520\\-3.0700&-0.5520&64.9240\end{bmatrix} = \begin{bmatrix}2.5245&0.7025&-1.1395\\0.7025&3.2405&1.1805\\-1.1395&1.1805&69.8245\end{bmatrix}
```

Penjumlahan matriks dilakukan elemen demi elemen (dimensi sama, $3\times3$ + $3\times3$). Contoh: elemen (1,1) = 0.7605 + 1.764 = 2.5245; elemen (3,3) = 4.9005 + 64.924 = 69.8245.

**4c. Determinan penyebut:**

Matriks yang dihitung determinannya: $SSP_{fac2}+SSP_{res} = \begin{bmatrix}2.5245&0.7025&-1.1395\\0.7025&3.2405&1.1805\\-1.1395&1.1805&69.8245\end{bmatrix}$ (ukuran $3\times3$, simetris). Tulis elemen-elemennya sebagai $\begin{bmatrix}a&b&c\\b&e&f\\c&f&i\end{bmatrix}$ dengan $a=2.5245$, $b=0.7025$, $c=-1.1395$, $e=3.2405$, $f=1.1805$, $i=69.8245$.

Rumus (ekspansi kofaktor sepanjang baris pertama): $\vert{}\,\cdot\,\vert{} = a(ei-f^2) - b(bi-fc) + c(bf-ec)$

| Bagian | Substitusi | Hasil |
|---|---|---|
| $ei-f^2$ | $(3.2405)(69.8245) - (1.1805)^2 = 226.26629225 - 1.39358025$ | $224.872712$ |
| $bi-fc$ | $(0.7025)(69.8245) - (1.1805)(-1.1395) = 49.05171125 - (-1.34517975)$ | $50.396891$ |
| $bf-ec$ | $(0.7025)(1.1805) - (3.2405)(-1.1395) = 0.82930125 - (-3.69254975)$ | $4.521851$ |

```math
\vert{}SSP_{fac2}+SSP_{res}\vert{} = a\cdot m_1 - b\cdot m_2 + c\cdot m_3 = (2.5245)(224.872712) - (0.7025)(50.396891) + (-1.1395)(4.521851) = 567.691161444 + (-35.4038159275) + (-5.1526492145) = \mathbf{527.134696302}
```

**4d. Wilks' Lambda:**

```math
\Lambda^{\ast} = \dfrac{275.709840752}{527.134696302} = 0.523\ (\text{pembulatan 4 desimal})
```

**4e. Konversi ke $F$** — hitung komponen satu per satu:

| Komponen | Perhitungan | Hasil |
|---|---|---|
| $\nu_1$ | $\vert{}(b-1) - p\vert{} + 1 = \vert{}1 - 3\vert{} + 1$ | 3 |
| $\nu_2$ | $gb(n-1)-p+1 = 2(2)(4) - 3 + 1 = 16-3+1$ | 14 |
| $(1-\Lambda^{\ast})/\Lambda^{\ast}$ | $(1-0.523)/0.523 = 0.477/0.523$ | 0.91205 |
| $\dfrac{\nu_2/2}{\nu_1/2}$ | $\dfrac{14/2}{3/2} = \dfrac{7}{1.5}$ | 4.66667 |
| $F$ | 0.91205 $\times$ 4.66667 | **4.2562** |

Jadi $F_{hitung} = 4.2562 \approx \mathbf{4.26}$ (modul: $\Lambda_2^{\ast}=.5230$, $F_2=4.26$, $\nu_1=3$, $\nu_2=14$).

**5. Nilai kritis dari Tabel $F$** (modul menamai $F_{tabel}$, simbol $F_{\nu_1,\nu_2}(\alpha)$):

- Tabel yang dipakai: tabel distribusi $F$ untuk $\alpha = 0.05$ (taraf nyata 5%, artinya kita bersedia salah menolak $H_0$ paling banyak 5%).
- Derajat kebebasan pembilang $\nu_1 = 3$ → pilih **kolom** bernomor 3.
- Derajat kebebasan penyebut $\nu_2 = 14$ → pilih **baris** bernomor 14.
- Nilai di perpotongan kolom 3 dan baris 14: $F_{3,14}(.05) = 3.34$ (nilai dari tabel, sama dengan nilai di modul). Perangkat lunak memberi 3.3439, yang dibulatkan menjadi 3.34.

**6. Aturan keputusan (modul):** tolak $H_0$ jika $F_{hitung} > F_{tabel}$; jika tidak, gagal menolak $H_0$.

**7. Keputusan:** $F_{hitung} = 4.26$ > $F_{tabel} = 3.34$, sehingga **$H_0$ ditolak**.

**Nilai-p (opsional, dari perangkat lunak, bukan dari tabel manual):** dari distribusi $F$ dengan $\nu_1=3$, $\nu_2=14$ dan nilai $F$ eksak 4.2556 (tanpa pembulatan $\Lambda^{\ast}$), diperoleh $p = 0.0247$. Karena $p$ < 0.05, keputusannya sama. Nilai-p adalah peluang mendapat $F$ sebesar atau lebih besar dari yang teramati jika $H_0$ benar; $p$ yang kecil berarti data jarang terjadi jika $H_0$ benar.

**8. Interpretasi:** jumlah aditif (1.0% vs 1.5%) mempengaruhi ketiga respon secara bersama-sama pada $\alpha=0.05$.

**Kesimpulan uji faktor 2:** $H_0$ ditolak ($F_2=4.26>3.34$).

---

## D. Perbandingan Modul vs Hitung Ulang

| Besaran | Modul | Hitung ulang | Status |
|---|---|---|---|
| $\vert{}SSP_{res}\vert{}$ | 275.7098 | 275.7098 | Cocok |
| $\vert{}SSP_{fac1}+SSP_{res}\vert{}$ | 722.0212 | 722.0212 | Cocok |
| $\vert{}SSP_{fac2}+SSP_{res}\vert{}$ | 527.1347 | 527.1347 | Cocok |
| $\vert{}SSP_{int}+SSP_{res}\vert{}$ | tidak ditulis | 354.7906 | Konsisten dengan $\Lambda^{\ast}=0.7771$ (275.7098/354.7906 = 0.7771) |
| $\Lambda^{\ast}$ (interaksi) | 0.7771 | 0.7771 | Cocok |
| $F$ (interaksi) | 1.34 | 1.3386 (dengan $\Lambda$ dibulatkan), 1.3385 (eksak) | Cocok |
| $\Lambda_1^{\ast}$ | 0.3819 | 0.3819 | Cocok |
| $F_1$ | 7.55 | 7.5529 (dibulatkan), 7.5543 (eksak) | Cocok |
| $\Lambda_2^{\ast}$ | 0.5230 | 0.523 | Cocok |
| $F_2$ | 4.26 | 4.2562 (dibulatkan), 4.2556 (eksak) | Cocok |
| $\nu_1,\nu_2$ | 3 dan 14 | 3 dan 14 | Cocok |
| $F_{3,14}(.05)$ | 3.34 | 3.3439 (perangkat lunak) | Cocok |
| Statistik $\chi^2$ interaksi | 3.66 | 3.6567 | Cocok |

**Tidak ada perbedaan angka yang mengubah keputusan.** Selisih kecil di digit ke-3/4 hanya karena pembulatan: modul memakai $\Lambda^{\ast}$ yang dibulatkan 4 desimal, hitung ulang eksak memberi nilai sedikit berbeda. Semua keputusan sama.

**Catatan salah ketik di modul (tidak mempengaruhi hasil):**

1. Slide Tabel MANOVA: baris “Faktor 2” diberi label “SSP fac1”; seharusnya $SSP_{fac2}$ (rumusnya memakai $\bar{\mathbf{x}}_{\cdot k}$ dan $gn$).
2. Slide hipotesis interaksi menulis $H_1$: minimal ada satu $\tau\beta_{gb}\neq0$. Lebih tepat: minimal ada satu $(\tau\beta)_{\ell k}\neq0$ (bukan hanya sel $gb$ terakhir).
3. Modul memakai $\tau\beta$ (di soal) dan $\gamma_{\ell k}$ (di model) untuk efek interaksi yang sama.

## E. Kesimpulan Contoh 6.11

**Kesimpulan lengkap:**

1. Interaksi laju ekstrusi × jumlah aditif: $\Lambda^{\ast}=0.7771$, $F=1.34<F_{3,14}(.05)=3.34$, sehingga $H_0$ (tidak ada interaksi) **gagal ditolak**. Tidak ada bukti interaksi pada taraf 5%. (Gagal menolak $H_0$ bukan berarti terbukti tidak ada interaksi; hanya bukti belum cukup.)
2. Faktor 1 (laju ekstrusi): $\Lambda_1^{\ast}=0.3819$, $F_1=7.55>3.34$, sehingga $H_0$ **ditolak**: laju ekstrusi mempengaruhi respon.
3. Faktor 2 (jumlah aditif): $\Lambda_2^{\ast}=0.5230$, $F_2=4.26>3.34$, sehingga $H_0$ **ditolak**: jumlah aditif mempengaruhi respon.
4. Karena interaksi tidak nyata sementara kedua faktor nyata, perubahan laju ekstrusi dan jumlah aditif sama-sama mempengaruhi respon, dan keduanya bekerja secara **aditif** (sesuai kesimpulan modul).

Modul menyebut bahwa sifat pengaruh faktor 1 dan 2 terhadap tiap komponen respon dieksplor lebih lanjut di Exercise 6.15 (selang kepercayaan simultan untuk kontras). Itu tidak termasuk dalam contoh yang dibahas modul, sehingga tidak dihitung di sini.

**Versi jawaban ujian:**

> Diketahui $g=2,b=2,n=5,p=3$, $\alpha=0.05$; $\nu_2=gb(n-1)-p+1=14$.
>
> **Interaksi:** $H_0:\gamma_{11}=\gamma_{12}=\gamma_{21}=\gamma_{22}=0$. $\Lambda^{\ast}=\vert{}SSP_{res}\vert{}/\vert{}SSP_{int}+SSP_{res}\vert{}=275.7098/354.7906=0.7771$; $F=\frac{1-0.7771}{0.7771}\cdot\frac{14/2}{3/2}=1.34$; $\nu_1=\vert{}1-3\vert{}+1=3$; $F_{3,14}(.05)=3.34$. $1.34<3.34$ → $H_0$ tidak ditolak: tidak ada interaksi.
>
> **Faktor 1:** $H_0:\tau_1=\tau_2=0$. $\Lambda_1^{\ast}=275.7098/722.0212=0.3819$; $F_1=7.55>3.34$ → $H_0$ ditolak: laju ekstrusi berpengaruh.
>
> **Faktor 2:** $H_0:\beta_1=\beta_2=0$. $\Lambda_2^{\ast}=275.7098/527.1347=0.5230$; $F_2=4.26>3.34$ → $H_0$ ditolak: jumlah aditif berpengaruh.
>
> **Kesimpulan:** laju ekstrusi dan jumlah aditif berpengaruh signifikan terhadap respon (tear resistance, gloss, opacity) secara bersama-sama pada $\alpha=5\%$, tanpa interaksi (efek aditif).

---

# RINGKASAN PROSEDUR PENGERJAAN MANOVA DUA ARAH

Panduan untuk soal baru (untuk jumlah level, ulangan, atau variabel yang berbeda). Rumus mengikuti modul. Penyesuaian untuk uji faktor ada di catatan akhir.

| No | Langkah | Rumus | Informasi yang harus ada | Hasil | Dipakai di |
|---|---|---|---|---|---|
| 1 | Tentukan ukuran | $g,b,n,p$ | Jumlah level F1, F2, ulangan per sel, jumlah respon | 4 bilangan | df dan rumus $F$ |
| 2 | Rata-rata sel | $\bar{\mathbf{x}}_{\ell k}=\frac1n\sum_r\mathbf{x}_{\ell kr}$ | Data tiap sel | $gb$ vektor $p\times1$ | $SSP_{int}$, $SSP_{res}$ |
| 3 | Rata-rata level F1 | $\bar{\mathbf{x}}_{\ell\cdot}=\frac1{bn}\sum_k\sum_r\mathbf{x}_{\ell kr}$ | Data baris | $g$ vektor | $SSP_{fac1}$, $SSP_{int}$ |
| 4 | Rata-rata level F2 | $\bar{\mathbf{x}}_{\cdot k}=\frac1{gn}\sum_\ell\sum_r\mathbf{x}_{\ell kr}$ | Data kolom | $b$ vektor | $SSP_{fac2}$, $SSP_{int}$ |
| 5 | Rata-rata keseluruhan | $\bar{\mathbf{x}}=\frac1{gbn}\sum\sum\sum\mathbf{x}_{\ell kr}$ | Semua data | 1 vektor | Semua $SSP$ |
| 6 | $SSP_{fac1}$ | $\sum_\ell bn(\bar{\mathbf{x}}_{\ell\cdot}-\bar{\mathbf{x}})(\cdot)'$ | Langkah 3, 5 | matriks $p\times p$ | Uji F1 |
| 7 | $SSP_{fac2}$ | $\sum_k gn(\bar{\mathbf{x}}_{\cdot k}-\bar{\mathbf{x}})(\cdot)'$ | Langkah 4, 5 | matriks | Uji F2 |
| 8 | $SSP_{int}$ | $\sum_\ell\sum_k n(\bar{\mathbf{x}}_{\ell k}-\bar{\mathbf{x}}_{\ell\cdot}-\bar{\mathbf{x}}_{\cdot k}+\bar{\mathbf{x}})(\cdot)'$ | Langkah 2–5 | matriks | Uji interaksi |
| 9 | $SSP_{res}$ | $\sum\sum\sum(\mathbf{x}_{\ell kr}-\bar{\mathbf{x}}_{\ell k})(\cdot)'$ | Data, Langkah 2 | matriks | Semua uji |
| 10 | Verifikasi | $SSP_{cor}=SSP_{fac1}+SSP_{fac2}+SSP_{int}+SSP_{res}$ | Langkah 6–9 | Cek cocok | — |
| 11 | Wilks' Lambda | $\Lambda^{\ast}=\frac{\vert{}SSP_{res}\vert{}}{\vert{}SSP_{efek}+SSP_{res}\vert{}}$ | $SSP_{efek}$ (fac1/fac2/int) dan $SSP_{res}$ | $\Lambda^{\ast}\in(0,1)$ | Langkah 12 |
| 12 | Statistik $F$ | $F=\frac{1-\Lambda^{\ast}}{\Lambda^{\ast}}\cdot\frac{(gb(n-1)-p+1)/2}{(\vert{}df_{efek}-p\vert{}+1)/2}$ | $\Lambda^{\ast}$, $g,b,n,p$ | $F_{hitung}$ | Langkah 13 |
| 13 | Nilai kritis | $\nu_1=\vert{}df_{efek}-p\vert{}+1$, $\nu_2=gb(n-1)-p+1$; cari $F_{\nu_1,\nu_2}(\alpha)$ di tabel $F$ | $\alpha$ | $F_{tabel}$ | Langkah 14 |
| 14 | Keputusan | Tolak $H_0$ jika $F_{hitung}>F_{tabel}$ | Langkah 12–13 | Tolak/gagal tolak | Kesimpulan |

**Catatan $df_{efek}$:** interaksi $=(g-1)(b-1)$; faktor 1 $=g-1$; faktor 2 $=b-1$. Rumus $F$ eksak di modul berlaku untuk $df_{efek}=1$ (kasus $g-1=1$ dan/atau $b-1=1$, atau $(g-1)(b-1)=1$). Untuk kasus df lebih besar, modul tidak memberikan rumusnya, jadi ikuti rumus lain dari buku acuan (jangan campur dengan statistik uji lain).

**Urutan uji:** uji interaksi lebih dulu. Bila interaksi tidak nyata, lanjutkan ke uji faktor 1 dan faktor 2 seperti pada Contoh 6.11. Bila interaksi nyata, pengaruh faktor tidak bisa ditafsirkan terpisah (ini penafsiran statistik umum, bukan perhitungan di modul).

---

# Bagian yang Tidak Dapat Direkonstruksi atau Perlu Diperhatikan

1. **Tabel $F$:** nilai $F_{3,14}(.05)=3.34$ diambil dari tabel distribusi $F$ standar (sama dengan modul); saya tidak punya tabel cetak, jadi nilai eksak 3.3439 dan nilai-p dihitung dengan perangkat lunak (SciPy), bukan dari tabel manual.
2. **Matriks $SSP$:** tidak ada di modul; semuanya direkonstruksi dari data. Satu-satunya angka pembanding di modul adalah tiga determinan, $\Lambda^{\ast}$, dan $F$, dan semuanya cocok. Lihat Bagian D.
3. **Exercise 6.15** (kontras dan selang kepercayaan simultan): disebut di modul tetapi data/langkahnya tidak ada, sehingga tidak dikerjakan.
4. Modul hanya berisi **satu** contoh soal (Contoh 6.11); tidak ada contoh lain.
