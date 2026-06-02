## 🧠 Pengertian & Teori Fungsi Aktivasi

Fungsi aktivasi adalah komponen kritis dalam jaringan saraf tiruan (MLP) yang menentukan apakah suatu neuron harus aktif (*firing*) atau tidak, berdasarkan bobot inputnya. Tanpa fungsi aktivasi, MLP hanya akan bertindak sebagai model regresi linear biasa, tidak peduli seberapa banyak lapisan (*hidden layer*) yang ditambahkan.

Berikut adalah penjelasan mendalam mengenai tiga fungsi aktivasi yang diuji dalam proyek ini:

<table border="1" cellpadding="8" style="border-collapse: collapse; width: 100%; font-family: Arial, sans-serif;">
  <thead>
    <tr style="background-color: #f2f2f2; text-align: left;">
      <th style="font-weight: bold; width: 25%;">Fungsi Aktivasi</th>
      <th style="font-weight: bold; width: 25%;">Persamaan Matematis</th>
      <th style="font-weight: bold; width: 25%;">Rentang Output (Range)</th>
      <th style="font-weight: bold; width: 25%;">Karakteristik Utama</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="font-weight: bold; color: #1f4e79;">Sigmoid (Logistic)</td>
      <td><code>f(x) = 1 / (1 + e^-x)</code></td>
      <td>0 hingga 1</td>
      <td>Kurva berbentuk S, non-linear, rentan terhadap <i>Vanishing Gradient</i>.</td>
    </tr>
    <tr>
      <td style="font-weight: bold; color: #2e75b6;">Tanh (Hyperbolic Tangent)</td>
      <td><code>f(x) = (e^x - e^-x) / (e^x + e^-x)</code></td>
      <td>-1 hingga 1</td>
      <td>Berpusat di nol (<i>Zero-centered</i>), gradien lebih kuat dibanding Sigmoid.</td>
    </tr>
    <tr>
      <td style="font-weight: bold; color: #d99694;">ReLU (Rectified Linear Unit)</td>
      <td><code>f(x) = max(0, x)</code></td>
      <td>0 hingga Tak Hingga (∞)</td>
      <td>Sangat cepat, mengaktifkan neuron secara selektif (<i>Sparsity</i>).</td>
    </tr>
  </tbody>
</table>

<br>

### 1. Sigmoid (Logistic Activation)
Fungsi Sigmoid memetakan nilai input bernilai riil apa pun ke dalam rentang interval antara **0 dan 1**. 

* **Cara Kerja:** Jika input bernilai positif besar, outputnya akan mendekati 1. Sebaliknya, jika input bernilai negatif besar, outputnya akan mendekati 0.
* **Kelemahan pada Proyek:** Ketika nilai input sangat tinggi atau rendah, gradiennya menjadi sangat kecil (mendekati 0). Selama proses *backpropagation*, perkalian gradien yang kecil ini membuat pembaruan bobot melambat drastis. Hal ini menjelaskan mengapa nilai *loss* pada kurva eksperimen Sigmoid di proyek ini melandai paling lambat dan menyisakan error tertinggi (**0.124954**).

### 2. Tanh (Hyperbolic Tangent)
Fungsi Tanh memiliki bentuk kurva "S" yang mirip dengan Sigmoid, tetapi memetakan nilai input ke dalam rentang **-1 hingga 1**.

* **Cara Kerja:** Fungsi ini memiliki keunggulan karena sifatnya yang **zero-centered** (berpusat di angka nol). Artinya, input yang bernilai negatif akan dipetakan secara negatif, dan input positif dipetakan secara positif.
* **Dampak pada Proyek:** Sifat *zero-centered* ini membantu mempercepat konvergensi di awal latihan. Terbukti pada grafik *Training Loss Curve* proyek, kurva Tanh (oranye) langsung menukik tajam pada *epoch* 0-2. Namun, ia tetap memiliki kelemahan saturasi gradien di ujung kurvanya, yang menyebabkannya mentok di akurasi **90.00%**.

### 3. ReLU (Rectified Linear Unit)
ReLU adalah fungsi aktivasi yang paling populer dan menjadi standar *default* dalam pengembangan *deep learning* modern saat ini.

* **Cara Kerja:** Fungsi ini bekerja dengan sangat sederhana: jika input berharga negatif, maka output akan dipaksa menjadi **0**. Jika input berharga positif, maka output adalah **nilai input itu sendiri** (`x`).
* **Mengapa Terbaik di Proyek Ini?:** 
  * **Bebas Vanishing Gradient:** Karena turunannya selalu bernilai 1 untuk setiap input positif, gradien mengalir penuh tanpa menyusut, memungkinkan jaringan MLP mengekstrak fitur dataset Iris dengan optimal hingga mencapai akurasi **93.33%**.
  * **Komputasi Efisien:** Tidak melibatkan operasi eksponensial (seperti `e^-x`), sehingga komputasi internal berjalan jauh lebih ringan dan menghasilkan kestabilan *loss* akhir terkecil (**0.038180**).
