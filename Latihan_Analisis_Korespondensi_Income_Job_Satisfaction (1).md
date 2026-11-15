# Latihan Soal Analisis Korespondensi
## Income and Job Satisfaction Data (Table 12.15)

**Perintah soal:** analisis profil baris, analisis profil kolom, tabel korespondensi, dan interpretasi.

Keterangan kode kategori:

- Baris (Income): B1 = < $25.000, B2 = $25.000–$50.000, B3 = > $50.000
- Kolom (Job Satisfaction): K1 = Very dissatisfied, K2 = Somewhat dissatisfied, K3 = Moderately satisfied, K4 = Very satisfied

---

## Langkah 1 – Total Baris, Total Kolom, dan n

Data (baris per baris):

```
B1 : 42, 62, 184, 207
B2 : 13, 28,  81, 113
B3 :  7, 18,  54,  92
```

**Total baris**

```
B1 = 42 + 62 + 184 + 207 = 495
B2 = 13 + 28 + 81 + 113  = 235
B3 = 7 + 18 + 54 + 92    = 171
```

**Total kolom**

```
K1 = 42 + 13 + 7    = 62
K2 = 62 + 28 + 18   = 108
K3 = 184 + 81 + 54  = 319
K4 = 207 + 113 + 92 = 412
```

**Total keseluruhan**

```
n = 495 + 235 + 171 = 62 + 108 + 319 + 412 = 901
```

Pengamatan awal (modus): income terbanyak adalah **< $25.000** (495), kepuasan terbanyak adalah **Very satisfied** (412).

---

## Langkah 2 – Tabel Korespondensi (Matriks P)

Setiap elemen dibagi n = 901 (p_ij = n_ij / 901). Contoh: p₁₁ = 42 / 901 = 0,0466.

| Income | Very dissatisfied | Somewhat dissatisfied | Moderately satisfied | Very satisfied | Jumlah (Massa r) |
|---|---|---|---|---|---|
| < $25.000 | 0,0466 | 0,0688 | 0,2042 | 0,2297 | **0,5494** |
| $25.000–$50.000 | 0,0144 | 0,0311 | 0,0899 | 0,1254 | **0,2608** |
| > $50.000 | 0,0078 | 0,0200 | 0,0599 | 0,1021 | **0,1898** |
| **Jumlah (Massa c)** | **0,0688** | **0,1199** | **0,3541** | **0,4573** | **1,0000** |

**Vektor baris (r) dan vektor kolom (c)** (total marginal P):

```
r = 495/901 ; 235/901 ; 171/901 = 0,5494 ; 0,2608 ; 0,1898
c = 62/901 ; 108/901 ; 319/901 ; 412/901 = 0,0688 ; 0,1199 ; 0,3541 ; 0,4573
```

**Matriks diagonal**

```
Dr = diag(0,5494 ; 0,2608 ; 0,1898)
Dc = diag(0,0688 ; 0,1199 ; 0,3541 ; 0,4573)
```

---

## Langkah 3 – Analisis Profil Baris

Rumus: **R = Dr⁻¹ P** (setiap elemen baris dibagi jumlah barisnya).

**Baris B1 (< $25.000), dibagi 495**

```
42/495  = 0,0848
62/495  = 0,1253
184/495 = 0,3717
207/495 = 0,4182      (jumlah = 1)
```

**Baris B2 ($25.000–$50.000), dibagi 235**

```
13/235  = 0,0553
28/235  = 0,1191
81/235  = 0,3447
113/235 = 0,4809      (jumlah = 1)
```

**Baris B3 (> $50.000), dibagi 171**

```
7/171   = 0,0409
18/171  = 0,1053
54/171  = 0,3158
92/171  = 0,5380      (jumlah = 1)
```

**Massa (jumlah tiap kolom profil baris)**

```
K1 = 0,0848 + 0,0553 + 0,0409 = 0,1811
K2 = 0,1253 + 0,1191 + 0,1053 = 0,3497
K3 = 0,3717 + 0,3447 + 0,3158 = 1,0322
K4 = 0,4182 + 0,4809 + 0,5380 = 1,4370   <- terbesar
```

Profil rata-rata (pembanding) = vektor c = 0,0688 ; 0,1199 ; 0,3541 ; 0,4573.

**Interpretasi profil baris:**

1. Massa terbesar adalah **1,4370** pada **Very satisfied**, sama dengan modus data awal.
2. *Very dissatisfied*: nilai tertinggi pada B1 (0,0848). Responden berpenghasilan < $25.000 paling banyak berada pada kategori sangat tidak puas.
3. *Somewhat dissatisfied* dan *Moderately satisfied*: nilai tertinggi juga pada B1 (0,1253 dan 0,3717), tetapi selisih antar baris kecil.
4. *Very satisfied*: nilai tertinggi pada B3 (0,5380).
5. **Pola:** semakin tinggi income, proporsi *Very satisfied* naik (0,4182 → 0,4809 → 0,5380), dan proporsi *Very dissatisfied* turun (0,0848 → 0,0553 → 0,0409).

---

## Langkah 4 – Analisis Profil Kolom

Rumus: **C = P Dc⁻¹** (setiap elemen kolom dibagi jumlah kolomnya).

**Kolom K1 (Very dissatisfied), dibagi 62**

```
42/62 = 0,6774
13/62 = 0,2097
7/62  = 0,1129        (jumlah = 1)
```

**Kolom K2 (Somewhat dissatisfied), dibagi 108**

```
62/108 = 0,5741
28/108 = 0,2593
18/108 = 0,1667       (jumlah = 1)
```

**Kolom K3 (Moderately satisfied), dibagi 319**

```
184/319 = 0,5768
81/319  = 0,2539
54/319  = 0,1693      (jumlah = 1)
```

**Kolom K4 (Very satisfied), dibagi 412**

```
207/412 = 0,5024
113/412 = 0,2743
92/412  = 0,2233      (jumlah = 1)
```

**Massa (jumlah tiap baris profil kolom)**

```
B1 = 0,6774 + 0,5741 + 0,5768 + 0,5024 = 2,3307   <- terbesar
B2 = 0,2097 + 0,2593 + 0,2539 + 0,2743 = 0,9971
B3 = 0,1129 + 0,1667 + 0,1693 + 0,2233 = 0,6722
```

Profil rata-rata (pembanding) = vektor r = 0,5494 ; 0,2608 ; 0,1898.

**Interpretasi profil kolom:**

1. Massa terbesar adalah **2,3307** pada income **< $25.000**, sama dengan modus data awal.
2. Pada semua kategori kepuasan, B1 selalu memiliki proporsi terbesar karena memang kelompok terbanyak (54,94% dari total).
3. Pada *Very dissatisfied*, proporsi B1 = 0,6774, lebih besar dari rata-ratanya (0,5494). Kelompok ini **over-represented** pada ketidakpuasan.
4. Proporsi B3 naik dari 0,1129 (*Very dissatisfied*) menjadi 0,2233 (*Very satisfied*), melampaui rata-ratanya (0,1898). Kelompok ini **over-represented** pada kepuasan tertinggi.

---

## Langkah 5 – Matriks P − rc′

Setiap elemen = p_ij − (r_i × c_j).

```
Baris 1:
0,0466 − (0,5494 × 0,0688) =  0,0088
0,0688 − (0,5494 × 0,1199) =  0,0030
0,2042 − (0,5494 × 0,3541) =  0,0097
0,2297 − (0,5494 × 0,4573) = −0,0215

Baris 2:
0,0144 − (0,2608 × 0,0688) = −0,0035
0,0311 − (0,2608 × 0,1199) = −0,0002
0,0899 − (0,2608 × 0,3541) = −0,0024
0,1254 − (0,2608 × 0,4573) =  0,0062

Baris 3:
0,0078 − (0,1898 × 0,0688) = −0,0053
0,0200 − (0,1898 × 0,1199) = −0,0028
0,0599 − (0,1898 × 0,3541) = −0,0073
0,1021 − (0,1898 × 0,4573) =  0,0153
```

## Langkah 6 – Matriks Z

Rumus: **Z = Dr^(−1/2) (P − rc′) Dc^(−1/2)**, yaitu z_ij = (p_ij − r_i c_j) / √(r_i c_j).

Contoh:

```
z11 = 0,0088 / √(0,5494 × 0,0688) = 0,0088 / 0,1944 =  0,0453
z24 = 0,0062 / √(0,2608 × 0,4573) = 0,0062 / 0,3454 =  0,0178
z34 = 0,0153 / √(0,1898 × 0,4573) = 0,0153 / 0,2946 =  0,0520
```

Hasil lengkap:

```
Z = [  0,0453   0,0115   0,0220  -0,0428 ]
    [ -0,0263  -0,0011  -0,0080   0,0178 ]
    [ -0,0463  -0,0184  -0,0280   0,0520 ]
```

## Langkah 7 – Generalized Singular Value Decomposition (GSVD)

Dekomposisi Z = U Σ V′. Banyak dimensi maksimum = min(3, 4) − 1 = **2**, sehingga nilai singular ke-3 = 0 dan hanya 2 dimensi yang dipakai.

```
Nilai singular : λ1 = 0,1069 ; λ2 = 0,0106
Inertia (λ²)   : λ1² = 0,011438 ; λ2² = 0,000113
Total inertia  = 0,011438 + 0,000113 = 0,011551

Dimensi 1 = 0,011438 / 0,011551 = 99,02%
Dimensi 2 = 0,000113 / 0,011551 =  0,98%
```

Vektor singular (2 kolom pertama):

```
U = [  0,6272  -0,2392 ]        V = [  0,6503  -0,6661 ]
    [ -0,2956   0,8073 ]            [  0,1944   0,5933 ]
    [ -0,7206  -0,5394 ]            [  0,3400   0,3159 ]
                                    [ -0,6510  -0,3233 ]
```

---

## Langkah 8 – Koordinat Profil Baris dan Kolom

Rumus (normalisasi simetris seperti pada materi):

```
Koordinat baris : F = Dr^(−1/2) U Σ     (setara Dr⁻¹ A Du dengan A = Dr^(1/2) U)
Koordinat kolom : G = Dc^(−1/2) V Σ     (setara Dc⁻¹ B Du dengan B = Dc^(1/2) V)
```

Nilai pengali: 1/√r = 1,3491 ; 1,9581 ; 2,2955 dan 1/√c = 3,8121 ; 2,8883 ; 1,6806 ; 1,4789.

**Koordinat baris**

```
B1 (< $25.000)
Dim 1 = 1,3491 × 0,6272 × 0,1069   =  0,0905
Dim 2 = 1,3491 × (−0,2392) × 0,0106 = −0,0034

B2 ($25.000–$50.000)
Dim 1 = 1,9581 × (−0,2956) × 0,1069 = −0,0619
Dim 2 = 1,9581 × 0,8073 × 0,0106    =  0,0168

B3 (> $50.000)
Dim 1 = 2,2955 × (−0,7206) × 0,1069 = −0,1769
Dim 2 = 2,2955 × (−0,5394) × 0,0106 = −0,0131
```

**Koordinat kolom**

```
K1 (Very dissatisfied)
Dim 1 = 3,8121 × 0,6503 × 0,1069    =  0,2651
Dim 2 = 3,8121 × (−0,6661) × 0,0106 = −0,0270

K2 (Somewhat dissatisfied)
Dim 1 = 2,8883 × 0,1944 × 0,1069 =  0,0600
Dim 2 = 2,8883 × 0,5933 × 0,0106 =  0,0182

K3 (Moderately satisfied)
Dim 1 = 1,6806 × 0,3400 × 0,1069 =  0,0611
Dim 2 = 1,6806 × 0,3159 × 0,0106 =  0,0056

K4 (Very satisfied)
Dim 1 = 1,4789 × (−0,6510) × 0,1069 = −0,1030
Dim 2 = 1,4789 × (−0,3233) × 0,0106 = −0,0051
```

> Tanda (+/−) koordinat bisa terbalik di software lain (SPSS, R, Python) karena tanda vektor singular tidak unik. Posisi relatif antar titik tetap sama.

---

## Langkah 9 – Kode Python (hitung semua + plot) untuk Jupyter Notebook

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from scipy.stats import chi2

pd.options.display.float_format = "{:.6f}".format

# ------------------------------------------------------------
# 1. Tabel kontingensi
# ------------------------------------------------------------
baris = ["< $25.000", "$25.000-$50.000", "> $50.000"]
kolom = ["Very dissatisfied", "Somewhat dissatisfied",
         "Moderately satisfied", "Very satisfied"]

N = np.array([[42, 62, 184, 207],
              [13, 28,  81, 113],
              [ 7, 18,  54,  92]], dtype=float)

df_N = pd.DataFrame(N, index=baris, columns=kolom)
df_N["Jumlah"] = df_N.sum(axis=1)
df_N.loc["Jumlah"] = df_N.sum(axis=0)
print("=== Tabel Kontingensi ===")
print(df_N.astype(int))

n = N.sum()
print("\nn =", int(n))

# ------------------------------------------------------------
# 2. Tabel korespondensi (matriks P), vektor r, c, Dr, Dc
# ------------------------------------------------------------
P = N / n
r = P.sum(axis=1)       # massa baris
c = P.sum(axis=0)       # massa kolom
Dr = np.diag(r)
Dc = np.diag(c)

df_P = pd.DataFrame(P, index=baris, columns=kolom)
df_P["Jumlah (Massa r)"] = r
df_P.loc["Jumlah (Massa c)"] = list(c) + [1.0]
print("\n=== Tabel Korespondensi (P) ===")
print(df_P)
print("\nVektor r:", np.round(r, 4))
print("Vektor c:", np.round(c, 4))
print("\nDr =\n", np.round(Dr, 4))
print("\nDc =\n", np.round(Dc, 4))

# ------------------------------------------------------------
# 3. Profil baris: R = Dr^-1 P
# ------------------------------------------------------------
R = np.linalg.inv(Dr) @ P
df_R = pd.DataFrame(R, index=baris, columns=kolom)
df_R["Active Margin"] = R.sum(axis=1)
df_R.loc["Massa"] = list(R.sum(axis=0)) + [np.nan]
print("\n=== Profil Baris ===")
print(df_R)

# ------------------------------------------------------------
# 4. Profil kolom: C = P Dc^-1
# ------------------------------------------------------------
C = P @ np.linalg.inv(Dc)
df_C = pd.DataFrame(C, index=baris, columns=kolom)
df_C["Massa"] = C.sum(axis=1)
df_C.loc["Active Margin"] = list(C.sum(axis=0)) + [np.nan]
print("\n=== Profil Kolom ===")
print(df_C)

# ------------------------------------------------------------
# 5. Matriks Z dan SVD
# ------------------------------------------------------------
S = P - np.outer(r, c)                      # P - rc'
Z = np.diag(r**-0.5) @ S @ np.diag(c**-0.5) # Z = Dr^-1/2 (P-rc') Dc^-1/2
print("\n=== P - rc' ===")
print(np.round(S, 4))
print("\n=== Z ===")
print(np.round(Z, 4))

U, sv, Vt = np.linalg.svd(Z, full_matrices=False)
V = Vt.T

dim = min(P.shape) - 1                      # jumlah dimensi = 2
eig = sv**2
inertia_total = eig.sum()

df_eig = pd.DataFrame({
    "Dimensi": np.arange(1, len(sv) + 1),
    "Nilai Singular": sv,
    "Inertia (eigenvalue)": eig,
    "Persentase (%)": eig / inertia_total * 100,
    "Kumulatif (%)": np.cumsum(eig) / inertia_total * 100,
})
print("\n=== Ringkasan Dimensi ===")
print(df_eig)
print("Total inertia =", inertia_total)

# Uji chi-square (opsional, sebagai pelengkap)
chi_sq = n * inertia_total
df_chi = (N.shape[0] - 1) * (N.shape[1] - 1)
p_val = 1 - chi2.cdf(chi_sq, df_chi)
print(f"Chi-square = {chi_sq:.4f}, df = {df_chi}, p-value = {p_val:.4f}")

# ------------------------------------------------------------
# 6. Koordinat baris dan kolom (principal coordinates)
# ------------------------------------------------------------
F = np.diag(r**-0.5) @ U[:, :dim] @ np.diag(sv[:dim])      # koordinat baris
G = np.diag(c**-0.5) @ V[:, :dim] @ np.diag(sv[:dim])      # koordinat kolom

df_F = pd.DataFrame(F, index=baris, columns=["Dimensi 1", "Dimensi 2"])
df_G = pd.DataFrame(G, index=kolom, columns=["Dimensi 1", "Dimensi 2"])
print("\n=== Koordinat Baris ===")
print(df_F)
print("\n=== Koordinat Kolom ===")
print(df_G)

# ------------------------------------------------------------
# 7. Plot analisis korespondensi (Symmetrical Normalization)
# ------------------------------------------------------------
fig, ax = plt.subplots(figsize=(8, 6))

ax.scatter(F[:, 0], F[:, 1], c="green", marker="o", s=70, label="Income (baris)")
for i, nama in enumerate(baris):
    ax.annotate(nama, (F[i, 0], F[i, 1]), textcoords="offset points",
                xytext=(6, 6), color="green")

ax.scatter(G[:, 0], G[:, 1], c="blue", marker="^", s=70, label="Job Satisfaction (kolom)")
for j, nama in enumerate(kolom):
    ax.annotate(nama, (G[j, 0], G[j, 1]), textcoords="offset points",
                xytext=(6, -12), color="blue")

ax.axhline(0, color="gray", lw=0.8)
ax.axvline(0, color="gray", lw=0.8)
ax.set_xlabel(f"Dimensi 1 ({eig[0]/inertia_total*100:.2f}%)")
ax.set_ylabel(f"Dimensi 2 ({eig[1]/inertia_total*100:.2f}%)")
ax.set_title("Plot Analisis Korespondensi: Income vs Job Satisfaction")
ax.legend()
ax.grid(alpha=0.3)
plt.tight_layout()
plt.show()
```

---

## Langkah 10 – Interpretasi Akhir

### a. Interpretasi Dimensi

- **Dimensi 1** menjelaskan **99,02%** total inertia, sedangkan Dimensi 2 hanya 0,98%. Hampir seluruh hubungan income dan job satisfaction sudah terwakili Dimensi 1, sehingga Dimensi 2 dapat diabaikan.
- Dimensi 1 dapat dinamai **sumbu "kepuasan kerja yang berkaitan dengan besar income"**: sisi kanan (positif) berisi *< $25.000* dan *Very dissatisfied*, sisi kiri (negatif) berisi *> $50.000* dan *Very satisfied*. Penamaan dimensi bersifat subjektif.

### b. Interpretasi Plot / Koordinat

- **< $25.000** (Dim 1 = +0,0905) berada paling dekat dengan *Very dissatisfied* (+0,2651) dan *Somewhat dissatisfied* (+0,0600). Responden berpenghasilan rendah cenderung kurang puas.
- **> $50.000** (Dim 1 = −0,1769) paling dekat dengan *Very satisfied* (−0,1030). Responden berpenghasilan tinggi cenderung sangat puas.
- **$25.000–$50.000** (Dim 1 = −0,0619) berada di sisi yang sama dengan *Very satisfied*, tetapi lebih dekat ke pusat.
- *Somewhat dissatisfied* dan *Moderately satisfied* letaknya dekat titik pusat (0,0), artinya profilnya mirip profil rata-rata dan kurang membedakan antar kelompok income.
- Urutan kategori kepuasan pada Dimensi 1 (Very dissatisfied → Somewhat dissatisfied → Moderately satisfied → Very satisfied) sesuai dengan urutan ordinalnya, begitu juga urutan income.

### c. Kesimpulan

**Terdapat kecenderungan hubungan positif antara income dan job satisfaction:** semakin tinggi income, semakin tinggi kepuasan kerja, dan semakin rendah income, semakin besar kecenderungan merasa tidak puas. Ini didukung profil baris: proporsi *Very satisfied* naik (41,82% → 48,09% → 53,80%) dan proporsi *Very dissatisfied* turun (8,48% → 5,53% → 4,09%).

**Catatan:** total inertia hanya **0,01155**. Nilai Chi-square = 901 × 0,01155 ≈ **10,41** (df = 6) lebih kecil dari χ²(0,05; 6) = 12,59 (p ≈ 0,108), sehingga hubungan tersebut **lemah**. Analisis korespondensi bersifat eksploratif, bukan uji hipotesis, jadi kesimpulan ini adalah gambaran kecenderungan pola data.
