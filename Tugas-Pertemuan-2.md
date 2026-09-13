# Tugas 4.2 — Distribusi Normal Bivariat

## Soal

Diketahui populasi normal bivariat dengan:
- μ₁ = 0
- μ₂ = 2
- σ₁₁ = 2
- σ₂₂ = 1
- ρ₁₂ = 0,5

**(a)** Tuliskan fungsi kepadatan normal bivariat.
**(b)** Tuliskan ekspresi jarak tergeneralisasi kuadrat (x − μ)′Σ⁻¹(x − μ) sebagai fungsi dari x₁ dan x₂.

---

## Langkah 1: Menyusun Vektor Rata-Rata (μ)

Untuk kasus bivariat (p = 2), vektor rata-rata berukuran 2×1:

$$
\boldsymbol{\mu} = \begin{bmatrix} \mu_1 \\ \mu_2 \end{bmatrix} = \begin{bmatrix} 0 \\ 2 \end{bmatrix}
$$

## Langkah 2: Menyusun Matriks Varians-Kovarians (Σ)

Matriks Σ berukuran p×p = 2×2, dan bersifat simetris:

$$
\boldsymbol{\Sigma} = \begin{bmatrix} \sigma_{11} & \sigma_{12} \\ \sigma_{12} & \sigma_{22} \end{bmatrix}
$$

Nilai σ₁₁ dan σ₂₂ sudah diketahui langsung dari soal (σ₁₁ = 2, σ₂₂ = 1). Namun σ₁₂ (kovarians) belum diketahui secara langsung — yang diketahui adalah korelasinya (ρ₁₂ = 0,5). Maka σ₁₂ dicari menggunakan hubungan antara korelasi dan kovarians:

$$
\rho_{12} = \frac{\sigma_{12}}{\sqrt{\sigma_{11}\sigma_{22}}} \quad \Rightarrow \quad \sigma_{12} = \rho_{12}\sqrt{\sigma_{11}\sigma_{22}}
$$

Substitusi nilai:

$$
\sigma_{12} = 0{,}5 \times \sqrt{2 \times 1} = 0{,}5\sqrt{2} \approx 0{,}7071
$$

Sehingga matriks Σ menjadi:

$$
\boldsymbol{\Sigma} = \begin{bmatrix} 2 & 0{,}7071 \\ 0{,}7071 & 1 \end{bmatrix}
$$

## Langkah 3: Menghitung Determinan |Σ|

Determinan matriks 2×2 dihitung dengan:

$$
|\boldsymbol{\Sigma}| = \sigma_{11}\sigma_{22} - \sigma_{12}^2
$$

Substitusi nilai:

$$
|\boldsymbol{\Sigma}| = (2)(1) - (0{,}7071)^2 = 2 - 0{,}5 = 1{,}5
$$

*(Catatan: karena σ₁₂² = ρ₁₂²·σ₁₁σ₂₂, maka |Σ| = σ₁₁σ₂₂(1 − ρ₁₂²) = (2)(1)(1 − 0,25) = 1,5 — hasil sama)*

Karena |Σ| = 1,5 > 0, maka Σ memenuhi syarat *positive definite*, sehingga fungsi kepadatan normal bivariat valid untuk dibentuk.

## Langkah 4: Menghitung Invers Matriks Σ⁻¹

Untuk matriks 2×2, invers dihitung dengan rumus:

$$
\boldsymbol{\Sigma}^{-1} = \frac{1}{|\boldsymbol{\Sigma}|}\begin{bmatrix} \sigma_{22} & -\sigma_{12} \\ -\sigma_{12} & \sigma_{11} \end{bmatrix}
$$

Substitusi nilai:

$$
\boldsymbol{\Sigma}^{-1} = \frac{1}{1{,}5}\begin{bmatrix} 1 & -0{,}7071 \\ -0{,}7071 & 2 \end{bmatrix} = \begin{bmatrix} 0{,}6667 & -0{,}4714 \\ -0{,}4714 & 1{,}3333 \end{bmatrix}
$$

---

## Bagian (a): Fungsi Kepadatan Normal Bivariat

Bentuk umum fungsi kepadatan peluang (pdf) untuk normal multivariat dengan p variabel:

$$
f(\mathbf{x}) = \frac{1}{(2\pi)^{p/2}|\boldsymbol{\Sigma}|^{1/2}} \, e^{-(\mathbf{x}-\boldsymbol{\mu})'\boldsymbol{\Sigma}^{-1}(\mathbf{x}-\boldsymbol{\mu})/2}
$$

Untuk kasus bivariat, p = 2, sehingga (2π)^(p/2) = 2π, dan |Σ|^(1/2) = √1,5 ≈ 1,2247.

Maka bagian konstanta (di luar eksponen) menjadi:

$$
\frac{1}{(2\pi)\sqrt{1{,}5}} = \frac{1}{2\pi(1{,}2247)} \approx \frac{1}{7{,}6953} \approx 0{,}1300
$$

Bagian eksponen menggunakan hasil jarak tergeneralisasi kuadrat yang akan diturunkan lengkap di bagian (b). Dengan mensubstitusikan hasil tersebut, fungsi kepadatan normal bivariatnya adalah:

$$
f(x_1, x_2) = \frac{1}{2\pi\sqrt{1{,}5}} \, \exp\left[-\frac{1}{2}\left(\frac{1}{0{,}75}\right)\left(\frac{x_1^2}{2} - x_1(x_2-2) + (x_2-2)^2\right)\right]
$$

atau ditulis dalam bentuk desimal langsung menggunakan Σ⁻¹ dari Langkah 4:

$$
f(x_1, x_2) = 0{,}1300 \; \exp\left[-\frac{1}{2}\Big(0{,}6667\,x_1^2 - 0{,}9428\,x_1(x_2-2) + 1{,}3333\,(x_2-2)^2\Big)\right]
$$

*(Penurunan detail bagian eksponen ini ditunjukkan langkah demi langkah pada bagian (b) di bawah.)*

---

## Bagian (b): Jarak Tergeneralisasi Kuadrat (x − μ)′Σ⁻¹(x − μ)

### Langkah 1: Menyusun Vektor (x − μ)

$$
\mathbf{x} - \boldsymbol{\mu} = \begin{bmatrix} x_1 - \mu_1 \\ x_2 - \mu_2 \end{bmatrix} = \begin{bmatrix} x_1 - 0 \\ x_2 - 2 \end{bmatrix} = \begin{bmatrix} x_1 \\ x_2 - 2 \end{bmatrix}
$$

### Langkah 2: Mengalikan Σ⁻¹ dengan (x − μ)

$$
\boldsymbol{\Sigma}^{-1}(\mathbf{x}-\boldsymbol{\mu}) = \begin{bmatrix} 0{,}6667 & -0{,}4714 \\ -0{,}4714 & 1{,}3333 \end{bmatrix}\begin{bmatrix} x_1 \\ x_2 - 2 \end{bmatrix}
$$

Kalikan baris demi baris:

Baris 1:
$$
0{,}6667\,x_1 + (-0{,}4714)(x_2-2) = 0{,}6667\,x_1 - 0{,}4714(x_2-2)
$$

Baris 2:
$$
-0{,}4714\,x_1 + 1{,}3333(x_2-2)
$$

Sehingga:

$$
\boldsymbol{\Sigma}^{-1}(\mathbf{x}-\boldsymbol{\mu}) = \begin{bmatrix} 0{,}6667\,x_1 - 0{,}4714(x_2-2) \\ -0{,}4714\,x_1 + 1{,}3333(x_2-2) \end{bmatrix}
$$

### Langkah 3: Mengalikan (x − μ)′ dengan Hasil Langkah 2

$$
(\mathbf{x}-\boldsymbol{\mu})'\boldsymbol{\Sigma}^{-1}(\mathbf{x}-\boldsymbol{\mu}) = \begin{bmatrix} x_1 & x_2-2 \end{bmatrix}\begin{bmatrix} 0{,}6667\,x_1 - 0{,}4714(x_2-2) \\ -0{,}4714\,x_1 + 1{,}3333(x_2-2) \end{bmatrix}
$$

Hasil perkalian (dot product):

$$
= x_1\big[0{,}6667\,x_1 - 0{,}4714(x_2-2)\big] + (x_2-2)\big[-0{,}4714\,x_1 + 1{,}3333(x_2-2)\big]
$$

Jabarkan setiap suku:

$$
= 0{,}6667\,x_1^2 - 0{,}4714\,x_1(x_2-2) - 0{,}4714\,x_1(x_2-2) + 1{,}3333(x_2-2)^2
$$

Gabungkan dua suku tengah yang sama (masing-masing muncul sekali dari perkalian silang):

$$
= 0{,}6667\,x_1^2 - 0{,}9428\,x_1(x_2-2) + 1{,}3333(x_2-2)^2
$$

### Hasil Akhir Bagian (b)

$$
\boxed{(\mathbf{x}-\boldsymbol{\mu})'\boldsymbol{\Sigma}^{-1}(\mathbf{x}-\boldsymbol{\mu}) = 0{,}6667\,x_1^2 - 0{,}9428\,x_1(x_2-2) + 1{,}3333(x_2-2)^2}
$$

### Verifikasi dengan Rumus Alternatif (Bentuk ρ)

Sebagai pengecekan, untuk kasus bivariat terdapat rumus pintas jarak tergeneralisasi dalam bentuk korelasi ρ:

$$
(\mathbf{x}-\boldsymbol{\mu})'\boldsymbol{\Sigma}^{-1}(\mathbf{x}-\boldsymbol{\mu}) = \frac{1}{1-\rho_{12}^2}\left[\frac{(x_1-\mu_1)^2}{\sigma_{11}} - \frac{2\rho_{12}(x_1-\mu_1)(x_2-\mu_2)}{\sqrt{\sigma_{11}\sigma_{22}}} + \frac{(x_2-\mu_2)^2}{\sigma_{22}}\right]
$$

Substitusi nilai (1 − ρ₁₂² = 1 − 0,25 = 0,75):

$$
= \frac{1}{0{,}75}\left[\frac{x_1^2}{2} - \frac{2(0{,}5)\,x_1(x_2-2)}{\sqrt{2}} + \frac{(x_2-2)^2}{1}\right]
$$

$$
= 1{,}3333\left[0{,}5\,x_1^2 - 0{,}7071\,x_1(x_2-2) + (x_2-2)^2\right]
$$

$$
= 0{,}6667\,x_1^2 - 0{,}9428\,x_1(x_2-2) + 1{,}3333(x_2-2)^2
$$

Hasil ini **sama persis** dengan hasil pada Langkah 3, sehingga jawaban terverifikasi benar.

---

## Ringkasan Jawaban

| Bagian | Hasil |
|---|---|
| Σ (matriks kovarians) | $\begin{bmatrix} 2 & 0{,}7071 \\ 0{,}7071 & 1 \end{bmatrix}$ |
| \|Σ\| | 1,5 |
| Σ⁻¹ | $\begin{bmatrix} 0{,}6667 & -0{,}4714 \\ -0{,}4714 & 1{,}3333 \end{bmatrix}$ |
| **(a)** Fungsi kepadatan | $f(x_1,x_2) = \dfrac{1}{2\pi\sqrt{1{,}5}}\exp\left[-\dfrac{1}{2}\big(0{,}6667x_1^2 - 0{,}9428x_1(x_2-2) + 1{,}3333(x_2-2)^2\big)\right]$ |
| **(b)** Jarak tergeneralisasi kuadrat | $0{,}6667\,x_1^2 - 0{,}9428\,x_1(x_2-2) + 1{,}3333(x_2-2)^2$ |
