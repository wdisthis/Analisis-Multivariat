# LATIHAN ANALISIS DISKRIMINAN (Versi Tulis Tangan)
Metode: Fisher dan Smallest Squared Distance

Keterangan: Grup 1 = bangkrut, Grup 2 = tidak bangkrut. n1 = n2 = 5, p = 2.
Simbol: x̄ = rata-rata, Sp = kovarians gabungan, Sp^-1 = inversnya.

---

## LANGKAH 1. Rata-rata tiap grup

```
Grup 1:  x̄11 = (-0.45 -0.56 +0.06 -0.07 -0.10)/5 = -1.12/5 = -0.224
         x̄12 = (-0.41 -0.31 +0.02 -0.09 -0.09)/5 = -0.88/5 = -0.176

Grup 2:  x̄21 = (0.51 +0.08 +0.38 +0.19 +0.32)/5 = 1.48/5 = 0.296
         x̄22 = (0.10 +0.02 +0.11 +0.05 +0.07)/5 = 0.35/5 = 0.070
```

```
x̄1 - x̄2 = (-0.224 - 0.296 , -0.176 - 0.070) = (-0.520 , -0.246)
```

---

## LANGKAH 2. Selisih terhadap rata-rata grup sendiri

**Grup 1** (pusat: -0.224 ; -0.176)

| No | x1 - x̄11 | x2 - x̄12 | (dx1)² | (dx2)² | dx1·dx2 |
|---|---|---|---|---|---|
| 1 | -0.226 | -0.234 | 0.051076 | 0.054756 | 0.052884 |
| 2 | -0.336 | -0.134 | 0.112896 | 0.017956 | 0.045024 |
| 3 | 0.284 | 0.196 | 0.080656 | 0.038416 | 0.055664 |
| 4 | 0.154 | 0.086 | 0.023716 | 0.007396 | 0.013244 |
| 5 | 0.124 | 0.086 | 0.015376 | 0.007396 | 0.010664 |
| **Jumlah** | | | **0.28372** | **0.12592** | **0.17748** |

**Grup 2** (pusat: 0.296 ; 0.070)

| No | x1 - x̄21 | x2 - x̄22 | (dx1)² | (dx2)² | dx1·dx2 |
|---|---|---|---|---|---|
| 6 | 0.214 | 0.030 | 0.045796 | 0.0009 | 0.00642 |
| 7 | -0.216 | -0.050 | 0.046656 | 0.0025 | 0.01080 |
| 8 | 0.084 | 0.040 | 0.007056 | 0.0016 | 0.00336 |
| 9 | -0.106 | -0.020 | 0.011236 | 0.0004 | 0.00212 |
| 10 | 0.024 | 0.000 | 0.000576 | 0 | 0 |
| **Jumlah** | | | **0.11132** | **0.0054** | **0.02270** |

---

## LANGKAH 3. Matriks kovarians gabungan (Sp)

Rumus: Sp = (jumlah kuadrat grup 1 + jumlah kuadrat grup 2) / (n1 + n2 - 2) = (...)/8

```
S11 = (0.28372 + 0.11132)/8 = 0.39504/8 = 0.04938
S22 = (0.12592 + 0.00540)/8 = 0.13132/8 = 0.016415
S12 = (0.17748 + 0.02270)/8 = 0.20018/8 = 0.025023

Sp = | 0.04938    0.025023 |
     | 0.025023   0.016415 |
```

---

## LANGKAH 4. Invers Sp

```
det(Sp) = (0.04938)(0.016415) - (0.025023)²
        = 0.00081057 - 0.00062615
        = 0.00018442   (≈ 0.000184)

Sp^-1 = (1/det) x |  0.016415  -0.025023 |
                  | -0.025023   0.04938  |

      = |  88.996   -135.662 |
        | -135.662    267.719 |
```

---

## METODE A. FISHER

### A1. Koefisien fungsi diskriminan

Rumus: a = Sp^-1 (x̄1 - x̄2)

```
a1 = (88.996)(-0.520) + (-135.662)(-0.246)
   = -46.278 + 33.373
   = -12.905

a2 = (-135.662)(-0.520) + (267.719)(-0.246)
   = 70.544 - 65.859
   = 4.686
```

**Fungsi diskriminan Fisher:**

```
ŷ = -12.905 x1 + 4.686 x2
```

### A2. Skor rata-rata tiap grup dan titik pemisah

```
ȳ1 = (-12.905)(-0.224) + (4.686)(-0.176) = 2.8907 - 0.8247 = 2.066
ȳ2 = (-12.905)(0.296)  + (4.686)(0.070)  = -3.8199 + 0.3280 = -3.492

m̂ = (ȳ1 + ȳ2)/2 = (2.066 - 3.492)/2 = -0.713
```

### A3. Aturan klasifikasi

```
Jika ŷ ≥ -0.713  --> masuk Grup 1 (bangkrut)
Jika ŷ <  -0.713 --> masuk Grup 2 (tidak bangkrut)
```

### A4. Klasifikasi tiap observasi

| No | Grup asli | ŷ = -12.905 x1 + 4.686 x2 | ŷ | Dibanding -0.713 | Prediksi | Benar? |
|---|---|---|---|---|---|---|
| 1 | 1 | 5.807 - 1.921 | 3.886 | ≥ | 1 | Ya |
| 2 | 1 | 7.227 - 1.453 | 5.774 | ≥ | 1 | Ya |
| 3 | 1 | -0.774 + 0.094 | -0.681 | ≥ | 1 | Ya |
| 4 | 1 | 0.903 - 0.422 | 0.482 | ≥ | 1 | Ya |
| 5 | 1 | 1.291 - 0.422 | 0.869 | ≥ | 1 | Ya |
| 6 | 2 | -6.582 + 0.469 | -6.113 | < | 2 | Ya |
| 7 | 2 | -1.032 + 0.094 | -0.938 | < | 2 | Ya |
| 8 | 2 | -4.904 + 0.515 | -4.388 | < | 2 | Ya |
| 9 | 2 | -2.452 + 0.234 | -2.218 | < | 2 | Ya |
| 10 | 2 | -4.130 + 0.328 | -3.802 | < | 2 | Ya |

Catatan: obs. 3 (ŷ = -0.681) hanya sedikit di atas batas -0.713, jadi sangat dekat batas.

### A5. Hit ratio Fisher

| | | Observasi | |
|---|---|---|---|
| | | Grup 1 | Grup 2 |
| **Estimasi** | Grup 1 | 5 | 0 |
| | Grup 2 | 0 | 5 |

```
Hit ratio = (5 + 5)/10 x 100% = 100%
```

---

## METODE B. SMALLEST SQUARED DISTANCE

### B1. Rumus

```
Di² = (x - x̄i)' Sp^-1 (x - x̄i)
```

Dengan u = x1 - x̄i1 dan v = x2 - x̄i2, rumus dijabarkan menjadi:

```
Di² = 88.996 u² - 271.324 u v + 267.719 v²
```

(271.324 = 2 x 135.662)

Aturan: objek masuk ke grup dengan **Di² terkecil**.

### B2. Contoh hitung (observasi 1)

```
Ke grup 1: u = -0.226 , v = -0.234
 D1² = 88.996(0.051076) - 271.324(0.052884) + 267.719(0.054756)
     = 4.546 - 14.349 + 14.659
     = 4.856

Ke grup 2: u = -0.746 , v = -0.480
 D2² = 88.996(0.556516) - 271.324(0.35808) + 267.719(0.2304)
     = 49.527 - 97.157 + 61.682
     = 14.052   (≈ 14.054 jika memakai angka lebih teliti)

D1² < D2²  --> Grup 1
```

### B3. Hasil seluruh observasi

| No | Asli | u1 | v1 | D1² | u2 | v2 | D2² | Terkecil | Prediksi | Benar? |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | 1 | -0.226 | -0.234 | 4.856 | -0.746 | -0.480 | 14.054 | D1² | 1 | Ya |
| 2 | 1 | -0.336 | -0.134 | 2.638 | -0.856 | -0.380 | 15.613 | D1² | 1 | Ya |
| 3 | 1 | 0.284 | 0.196 | 2.360 | -0.236 | -0.050 | 2.424 | D1² | 1 | Ya |
| 4 | 1 | 0.154 | 0.086 | 0.497 | -0.366 | -0.160 | 2.886 | D1² | 1 | Ya |
| 5 | 1 | 0.124 | 0.086 | 0.455 | -0.396 | -0.160 | 3.618 | D1² | 1 | Ya |
| 6 | 2 | 0.734 | 0.276 | 13.375 | 0.214 | 0.030 | 2.575 | D2² | 2 | Ya |
| 7 | 2 | 0.304 | 0.196 | 2.343 | -0.216 | -0.050 | 1.891 | D2² | 2 | Ya |
| 8 | 2 | 0.604 | 0.286 | 7.496 | 0.084 | 0.040 | 0.145 | D2² | 2 | Ya |
| 9 | 2 | 0.414 | 0.226 | 3.541 | -0.106 | -0.020 | 0.532 | D2² | 2 | Ya |
| 10 | 2 | 0.544 | 0.246 | 6.229 | 0.024 | 0.000 | 0.051 | D2² | 2 | Ya |

(u1, v1 = selisih terhadap rata-rata grup 1; u2, v2 = selisih terhadap rata-rata grup 2.)

### B4. Hit ratio

| | | Observasi | |
|---|---|---|---|
| | | Grup 1 | Grup 2 |
| **Estimasi** | Grup 1 | 5 | 0 |
| | Grup 2 | 0 | 5 |

```
Hit ratio = 10/10 x 100% = 100%
```

---

## PERBANDINGAN KEDUA METODE

| Aspek | Fisher | Smallest Squared Distance |
|---|---|---|
| Yang dibandingkan | ŷ dengan m̂ = -0.713 | D1² dengan D2² |
| Salah klasifikasi | 0 | 0 |
| Hit ratio | 100% | 100% |
| Obs. paling dekat batas | Obs. 3 (selisih 0.032) | Obs. 3 (D2² - D1² = 0.065) |

Hubungan keduanya: **D2² - D1² = 2(ŷ - m̂)**.
Cek obs. 3: 2.424 - 2.360 = 0.064, sedangkan 2(-0.681 + 0.713) = 0.064. Sama.

---

## KESIMPULAN

1. Fungsi diskriminan Fisher: **ŷ = -12.905 x1 + 4.686 x2**, titik pemisah **m̂ = -0.713**.
2. Perusahaan dengan ŷ ≥ -0.713 diklasifikasikan **bangkrut**, dan yang ŷ < -0.713 diklasifikasikan **tidak bangkrut**.
3. Kedua metode mengklasifikasikan **10 dari 10 observasi dengan benar** (hit ratio 100%).
4. Hasil kedua metode **sama persis**, karena untuk 2 grup dengan Sp yang sama, aturan jarak kuadrat terkecil setara dengan aturan Fisher.
5. Perusahaan bangkrut cenderung punya rasio arus kas/utang (X1) dan laba/aset (X2) yang rendah atau negatif.
6. Hit ratio 100% dihitung pada data yang sama dengan data pembentuk model dan sampelnya kecil, sehingga hasilnya cenderung optimistis. Obs. 3 grup 1 juga sangat dekat dengan batas keputusan.
