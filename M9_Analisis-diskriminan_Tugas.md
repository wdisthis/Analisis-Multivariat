# Jawaban Latihan Analisis Diskriminan
**Metode: Fisher dan Smallest Squared Distance (Jarak Kuadrat Terkecil)**

---

## 1. Soal

Data kebangkrutan perusahaan dengan dua variabel:

- X1 = CF/TD = arus kas / total utang
- X2 = NI/TA = laba bersih / total aset
- Grup 1 = perusahaan **bangkrut**, Grup 2 = perusahaan **tidak bangkrut**

| No | Grup | x1 | x2 |
|---|---|---|---|
| 1 | 1 | -0.45 | -0.41 |
| 2 | 1 | -0.56 | -0.31 |
| 3 | 1 | 0.06 | 0.02 |
| 4 | 1 | -0.07 | -0.09 |
| 5 | 1 | -0.10 | -0.09 |
| 6 | 2 | 0.51 | 0.10 |
| 7 | 2 | 0.08 | 0.02 |
| 8 | 2 | 0.38 | 0.11 |
| 9 | 2 | 0.19 | 0.05 |
| 10 | 2 | 0.32 | 0.07 |

Tugas: lakukan analisis diskriminan dengan metode Fisher dan smallest squared distance, buat kesimpulan klasifikasi, lalu bandingkan kedua metode.

Dengan n₁ = n₂ = 5 dan p = 2 variabel.

---

## 2. Langkah Awal (Dipakai Kedua Metode)

### 2.1 Vektor rata-rata

**Grup 1 (bangkrut)**

- x̄₁₁ = (-0.45 - 0.56 + 0.06 - 0.07 - 0.10) / 5 = -1.12 / 5 = **-0.224**
- x̄₁₂ = (-0.41 - 0.31 + 0.02 - 0.09 - 0.09) / 5 = -0.88 / 5 = **-0.176**

**Grup 2 (tidak bangkrut)**

- x̄₂₁ = (0.51 + 0.08 + 0.38 + 0.19 + 0.32) / 5 = 1.48 / 5 = **0.296**
- x̄₂₂ = (0.10 + 0.02 + 0.11 + 0.05 + 0.07) / 5 = 0.35 / 5 = **0.070**

$$\bar{\mathbf{x}}_1 = \begin{bmatrix} -0.224 \\ -0.176 \end{bmatrix}, \qquad \bar{\mathbf{x}}_2 = \begin{bmatrix} 0.296 \\ 0.070 \end{bmatrix}$$

Selisih rata-rata:

$$\bar{\mathbf{x}}_1 - \bar{\mathbf{x}}_2 = \begin{bmatrix} -0.520 \\ -0.246 \end{bmatrix}$$

Rata-rata grup 1 (bangkrut) lebih kecil pada kedua rasio. Artinya perusahaan bangkrut punya arus kas dan laba yang relatif rendah dibanding total utang dan asetnya.

### 2.2 Matriks kovarians tiap grup

**Grup 1**

$$\mathbf{S}_1 = \begin{bmatrix} 0.07093 & 0.04437 \\ 0.04437 & 0.03148 \end{bmatrix}$$

**Grup 2**

$$\mathbf{S}_2 = \begin{bmatrix} 0.02783 & 0.005675 \\ 0.005675 & 0.00135 \end{bmatrix}$$

### 2.3 Matriks kovarians gabungan (pooled)

$$\mathbf{S}_{pooled} = \frac{(n_1-1)\mathbf{S}_1 + (n_2-1)\mathbf{S}_2}{n_1+n_2-2} = \frac{4\mathbf{S}_1 + 4\mathbf{S}_2}{8}$$

$$\mathbf{S}_{pooled} = \begin{bmatrix} 0.049380 & 0.025023 \\ 0.025023 & 0.016415 \end{bmatrix}$$

Determinan: |S_pooled| = 0.000184447

### 2.4 Invers matriks kovarians gabungan

$$\mathbf{S}_{pooled}^{-1} = \begin{bmatrix} 88.9957 & -135.6621 \\ -135.6621 & 267.7189 \end{bmatrix}$$

---

## 3. Metode 1: Fungsi Diskriminan Linier Fisher

### 3.1 Fungsi diskriminan

$$\hat{y} = \hat{\mathbf{a}}'\mathbf{x} = (\bar{\mathbf{x}}_1 - \bar{\mathbf{x}}_2)'\,\mathbf{S}_{pooled}^{-1}\,\mathbf{x}$$

Koefisien:

$$\hat{\mathbf{a}} = \mathbf{S}_{pooled}^{-1}(\bar{\mathbf{x}}_1 - \bar{\mathbf{x}}_2) = \begin{bmatrix} 88.9957 & -135.6621 \\ -135.6621 & 267.7189 \end{bmatrix}\begin{bmatrix} -0.520 \\ -0.246 \end{bmatrix} = \begin{bmatrix} -12.9049 \\ 4.6855 \end{bmatrix}$$

Jadi fungsi diskriminan Fisher:

$$\boxed{\hat{y} = -12.9049\,x_1 + 4.6855\,x_2}$$

### 3.2 Rata-rata skor tiap grup dan titik pemisah

- ȳ₁ = â'x̄₁ = (-12.9049)(-0.224) + (4.6855)(-0.176) = **2.0660**
- ȳ₂ = â'x̄₂ = (-12.9049)(0.296) + (4.6855)(0.070) = **-3.4919**

Titik pemisah (midpoint):

$$\hat{m} = \tfrac{1}{2}(\bar y_1 + \bar y_2) = \tfrac{1}{2}(2.0660 - 3.4919) = \boxed{-0.7129}$$

### 3.3 Aturan klasifikasi Fisher (Rule 11-35)

- Jika ŷ₀ ≥ m̂ = -0.7129, klasifikasikan ke **Grup 1 (bangkrut)**
- Jika ŷ₀ < m̂ = -0.7129, klasifikasikan ke **Grup 2 (tidak bangkrut)**

Bentuk setara seperti di materi (dengan konstanta digabung):

$$\hat{y} - \hat{m} = -12.9049\,x_1 + 4.6855\,x_2 + 0.7129$$

Jika bernilai ≥ 0 maka Grup 1, jika < 0 maka Grup 2.

### 3.4 Perhitungan skor dan klasifikasi tiap observasi

| No | Grup Asli | Perhitungan ŷ | ŷ | ŷ − m̂ | Prediksi | Benar? |
|---|---|---|---|---|---|---|
| 1 | 1 | (-12.9049)(-0.45)+(4.6855)(-0.41) | 3.8861 | 4.5991 | 1 | ✅ |
| 2 | 1 | (-12.9049)(-0.56)+(4.6855)(-0.31) | 5.7742 | 6.4871 | 1 | ✅ |
| 3 | 1 | (-12.9049)(0.06)+(4.6855)(0.02) | -0.6806 | 0.0323 | 1 | ✅ |
| 4 | 1 | (-12.9049)(-0.07)+(4.6855)(-0.09) | 0.4816 | 1.1946 | 1 | ✅ |
| 5 | 1 | (-12.9049)(-0.10)+(4.6855)(-0.09) | 0.8688 | 1.5817 | 1 | ✅ |
| 6 | 2 | (-12.9049)(0.51)+(4.6855)(0.10) | -6.1129 | -5.4000 | 2 | ✅ |
| 7 | 2 | (-12.9049)(0.08)+(4.6855)(0.02) | -0.9387 | -0.2258 | 2 | ✅ |
| 8 | 2 | (-12.9049)(0.38)+(4.6855)(0.11) | -4.3884 | -3.6755 | 2 | ✅ |
| 9 | 2 | (-12.9049)(0.19)+(4.6855)(0.05) | -2.2177 | -1.5047 | 2 | ✅ |
| 10 | 2 | (-12.9049)(0.32)+(4.6855)(0.07) | -3.8016 | -3.0887 | 2 | ✅ |

Catatan: observasi ke-3 grup 1 (ŷ = -0.6806) hanya sedikit di atas m̂ = -0.7129 (selisih 0.0323), sehingga ia **sangat dekat dengan batas** meskipun terklasifikasi benar.

### 3.5 Confusion matrix dan hit ratio (Fisher)

| | | Observasi | |
|---|---|---|---|
| | | **Grup 1** | **Grup 2** |
| **Estimasi** | **Grup 1** | 5 | 0 |
| | **Grup 2** | 0 | 5 |

$$\text{Hit ratio} = \frac{5 + 5}{10} \times 100\% = \boxed{100\%}$$

Tingkat kesalahan klasifikasi (APER) = 0%.

---

## 4. Metode 2: Smallest Squared Distance (Jarak Kuadrat Terkecil)

### 4.1 Rumus

Untuk setiap objek x, hitung jarak kuadrat (Mahalanobis) ke pusat tiap grup:

$$D_i^2(\mathbf{x}) = (\mathbf{x} - \bar{\mathbf{x}}_i)'\,\mathbf{S}_{pooled}^{-1}\,(\mathbf{x} - \bar{\mathbf{x}}_i), \quad i = 1, 2$$

**Aturan:** objek dimasukkan ke grup dengan nilai D² **terkecil**.

### 4.2 Contoh perhitungan (observasi 1)

x = (-0.45, -0.41)

- Terhadap grup 1: x − x̄₁ = (-0.226, -0.234)
  D₁² = 88.9957(-0.226)² + 2(-135.6621)(-0.226)(-0.234) + 267.7189(-0.234)² ≈ **4.856**
- Terhadap grup 2: x − x̄₂ = (-0.746, -0.510)
  D₂² ≈ **14.054**

D₁² < D₂², sehingga observasi 1 masuk **Grup 1**.

### 4.3 Hasil untuk seluruh observasi

| No | Grup Asli | D₁² | D₂² | D² terkecil | Prediksi | Benar? |
|---|---|---|---|---|---|---|
| 1 | 1 | 4.8560 | 14.0542 | D₁² | 1 | ✅ |
| 2 | 1 | 2.6383 | 15.6126 | D₁² | 1 | ✅ |
| 3 | 1 | 2.3597 | 2.4244 | D₁² | 1 | ✅ |
| 4 | 1 | 0.4973 | 2.8864 | D₁² | 1 | ✅ |
| 5 | 1 | 0.4550 | 3.6184 | D₁² | 1 | ✅ |
| 6 | 2 | 13.3747 | 2.5747 | D₂² | 2 | ✅ |
| 7 | 2 | 2.3427 | 1.8912 | D₂² | 2 | ✅ |
| 8 | 2 | 7.4957 | 0.1447 | D₂² | 2 | ✅ |
| 9 | 2 | 3.5413 | 0.5318 | D₂² | 2 | ✅ |
| 10 | 2 | 6.2286 | 0.0513 | D₂² | 2 | ✅ |

Observasi ke-3 grup 1 punya D₁² = 2.3597 dan D₂² = 2.4244. Selisihnya sangat tipis, tetapi tetap masuk Grup 1.

### 4.4 Confusion matrix dan hit ratio (Smallest Squared Distance)

| | | Observasi | |
|---|---|---|---|
| | | **Grup 1** | **Grup 2** |
| **Estimasi** | **Grup 1** | 5 | 0 |
| | **Grup 2** | 0 | 5 |

$$\text{Hit ratio} = \frac{10}{10} \times 100\% = \boxed{100\%}$$

---

## 5. Perbandingan Kedua Metode

| Aspek | Fisher | Smallest Squared Distance |
|---|---|---|
| Ide dasar | Proyeksi data ke satu dimensi (skor ŷ), lalu dibandingkan dengan titik tengah m̂ | Menghitung jarak Mahalanobis objek ke pusat tiap grup |
| Kuantitas yang dibandingkan | ŷ vs m̂ = -0.7129 | D₁² vs D₂² |
| Aturan | ŷ ≥ m̂ → Grup 1 | D² terkecil → grupnya |
| Matriks yang dipakai | S_pooled⁻¹ | S_pooled⁻¹ |
| Asumsi | Kovarians homogen | Kovarians homogen |
| Observasi salah klasifikasi | 0 | 0 |
| Hit ratio | **100%** | **100%** |
| Observasi paling dekat batas | Observasi 3 grup 1 (ŷ − m̂ = 0.0323) | Observasi 3 grup 1 (D₂² − D₁² = 0.0647) |

**Mengapa hasilnya sama?** Untuk dua grup dengan kovarians gabungan yang sama dan prior sama, aturan jarak kuadrat terkecil secara aljabar setara dengan aturan Fisher. Membandingkan D₁² dan D₂² sama dengan membandingkan ŷ terhadap m̂. Buktinya:

$$D_2^2 - D_1^2 = 2(\bar{\mathbf{x}}_1 - \bar{\mathbf{x}}_2)'\mathbf{S}_{pooled}^{-1}\mathbf{x} - (\bar{\mathbf{x}}_1 - \bar{\mathbf{x}}_2)'\mathbf{S}_{pooled}^{-1}(\bar{\mathbf{x}}_1 + \bar{\mathbf{x}}_2) = 2(\hat{y} - \hat{m})$$

Jadi D₂² − D₁² selalu sama dengan 2(ŷ − m̂). Contoh observasi 3: 2.4244 − 2.3597 = 0.0647 = 2 × 0.0323. ✅

---

## 6. Kesimpulan

1. **Fungsi diskriminan Fisher** yang terbentuk adalah ŷ = −12.9049·X1 + 4.6855·X2, dengan titik pemisah m̂ = −0.7129.
2. Koefisien X1 (|−12.90|) lebih besar daripada X2 (|4.69|). Namun karena skala X1 dan X2 berbeda dan kedua variabel saling berkorelasi, perbandingan ini hanya indikasi awal. Untuk kesimpulan yang lebih kuat, koefisien sebaiknya distandardisasi lebih dulu.
3. Perusahaan dengan ŷ ≥ −0.7129 diklasifikasikan **bangkrut**, sedangkan yang ŷ < −0.7129 diklasifikasikan **tidak bangkrut**. Perusahaan bangkrut cenderung memiliki rasio arus kas/utang dan laba/aset yang rendah (negatif).
4. Kedua metode mengklasifikasikan **seluruh 10 observasi dengan benar**, yaitu hit ratio **100%** dan tanpa misklasifikasi.
5. Kedua metode **memberikan hasil yang identik**. Hal ini sesuai teori, karena dengan kovarians gabungan yang sama dan prior sama, kedua aturan ekuivalen.
6. **Kehati-hatian:** hit ratio 100% dihitung pada data yang sama dengan data pembentuk model (resubstitution), dan sampel hanya 10 observasi. Estimasi ini cenderung optimistis. Observasi 3 grup 1 sangat dekat dengan batas keputusan. Untuk validasi yang lebih andal, gunakan cross-validation (misalnya leave-one-out) atau data uji terpisah.
7. Asumsi yang perlu diperiksa sebelum menyimpulkan: normalitas multivariat, perbedaan vektor rata-rata antar grup, dan homogenitas matriks kovarians. Dari S₁ dan S₂ terlihat perbedaan cukup besar (S₁ jauh lebih besar dari S₂), sehingga uji Box's M perlu dilakukan. Jika asumsi homogenitas tidak terpenuhi, analisis diskriminan kuadratik bisa dipertimbangkan.

---

## Lampiran: Kode Verifikasi (Python)

```python
import numpy as np

g1 = np.array([[-0.45,-0.41],[-0.56,-0.31],[0.06,0.02],[-0.07,-0.09],[-0.1,-0.09]])
g2 = np.array([[0.51,0.1],[0.08,0.02],[0.38,0.11],[0.19,0.05],[0.32,0.07]])

m1, m2 = g1.mean(0), g2.mean(0)
Sp = (4*np.cov(g1.T) + 4*np.cov(g2.T)) / 8
Si = np.linalg.inv(Sp)

a = Si @ (m1 - m2)                 # koefisien Fisher
m = (a @ m1 + a @ m2) / 2          # titik pemisah

X = np.vstack([g1, g2])
for i, x in enumerate(X, 1):
    y = a @ x
    d1 = (x - m1) @ Si @ (x - m1)
    d2 = (x - m2) @ Si @ (x - m2)
    print(i, round(y, 4), 1 if y >= m else 2, round(d1, 4), round(d2, 4), 1 if d1 < d2 else 2)
```
