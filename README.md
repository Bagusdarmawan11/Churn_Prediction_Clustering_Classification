# Proyek Analisis Prediksi Churn Pelanggan dengan Clustering dan Klasifikasi

## Ringkasan Proyek
Proyek ini merupakan bagian dari tugas akhir "Machine Learning Process" yang berfokus pada analisis perilaku pelanggan untuk memprediksi *churn* (pelanggan yang berhenti menggunakan layanan). Proyek ini dibagi menjadi dua tahapan utama: **Clustering** dan **Klasifikasi**.

Tahap pertama, **Clustering**, bertujuan untuk mengelompokkan pelanggan berdasarkan karakteristik mereka dari dataset `Churn Modeling.csv` yang tidak berlabel. Hasil dari clustering ini menghasilkan sebuah dataset baru bernama `hasil_cluster.csv` yang menambahkan kolom cluster sebagai label.

Tahap kedua, **Klasifikasi**, menggunakan dataset `hasil_cluster.csv` yang sudah memiliki label cluster untuk membangun model prediktif. Tujuan klasifikasi adalah memprediksi apakah seorang pelanggan akan *churn* atau tidak berdasarkan fitur-fitur yang ada, termasuk cluster pelanggan.

## Struktur Proyek
* `[Clustering]_Submission_Akhir_BMLP_Bagus_Darmawan_(Updated).ipynb`: Notebook Jupyter untuk tahap clustering pelanggan.
* `[Klasifikasi]_Submission_Akhir_BMLP_Bagus_Darmawan.ipynb`: Notebook Jupyter untuk tahap klasifikasi prediksi churn.
* `Churn Modeling.csv`: Dataset asli yang digunakan untuk clustering.
* `hasil_cluster.csv`: Dataset hasil dari proses clustering, yang kemudian digunakan untuk klasifikasi.

## Tahap 1: Clustering Pelanggan (`[Clustering]_Submission_Akhir_BMLP_Bagus_Darmawan_(Updated).ipynb`)

### Tujuan
Mengidentifikasi kelompok-kelompok (cluster) pelanggan yang berbeda dalam dataset `Churn Modeling.csv` berdasarkan karakteristik yang ada, tanpa adanya label target. Tujuan utamanya adalah untuk mendapatkan wawasan tentang segmen pelanggan yang berbeda dan menambahkan label cluster ini ke dataset untuk digunakan dalam tahap klasifikasi.

### Dataset
Dataset yang digunakan adalah `Churn Modeling.csv`.
* **Jumlah Baris**: 10000
* **Jumlah Kolom**: 14
* **Tipe Data**: Mengandung data kategorikal (misalnya `Geography`, `Gender`) dan numerikal (misalnya `CreditScore`, `Age`, `Balance`, `EstimatedSalary`).

### Pra-pemrosesan Data
1.  **Penanganan Kolom Tidak Relevan**: Kolom seperti `RowNumber`, `CustomerId`, dan `Surname` dihapus karena tidak relevan untuk analisis clustering.
2.  **Encoding Kategorikal**:
    * Kolom `Geography` di-encode menggunakan One-Hot Encoding.
    * Kolom `Gender` di-encode menggunakan Label Encoding (Male=1, Female=0).
3.  **Standardisasi Data**: Semua fitur numerikal diskalakan menggunakan `StandardScaler` untuk memastikan bahwa tidak ada fitur yang mendominasi proses clustering karena skala nilai yang besar.

### Algoritma Clustering
Digunakan algoritma **K-Means**.
* **Penentuan Jumlah Cluster Optimal**: Metode Elbow Method digunakan untuk menentukan jumlah cluster yang optimal, di mana nilai WCSS (Within-Cluster Sum of Squares) mulai melandai. Berdasarkan analisis elbow method, jumlah cluster optimal yang dipilih adalah **3**.

### Hasil Clustering
* Dataset diperkaya dengan kolom baru bernama `Cluster` yang menunjukkan segmentasi pelanggan (0, 1, 2).
* Dataset yang telah diberi label cluster (`hasil_cluster.csv`) disimpan dan diunduh untuk digunakan pada tahap klasifikasi.

## Tahap 2: Klasifikasi Prediksi Churn (`[Klasifikasi]_Submission_Akhir_BMLP_Bagus_Darmawan.ipynb`)

### Tujuan
Membangun model klasifikasi yang dapat memprediksi *churn* pelanggan (kolom `Exited`) berdasarkan fitur-fitur yang ada, termasuk informasi cluster dari tahap sebelumnya. Tujuan utamanya adalah untuk mengidentifikasi faktor-faktor yang berkontribusi terhadap *churn* dan memprediksi kemungkinan seorang pelanggan akan *churn*.

### Dataset
Dataset yang digunakan adalah `hasil_cluster.csv`, yang merupakan output dari tahap clustering.
* **Fitur**: Semua kolom kecuali `Exited` dan `Cluster`. (Namun, dari notebook, terlihat kolom `Cluster` **digunakan** sebagai fitur tambahan untuk klasifikasi).
* **Target**: Kolom `Exited` (1 = Churn, 0 = Tidak Churn).

### Pra-pemrosesan Data
1.  **Penanganan Kolom Tidak Relevan**: Kolom `Unnamed: 0` (indeks sisa dari penyimpanan CSV) dihapus.
2.  **Encoding Kategorikal**: Kolom `Geography` dan `Gender` (jika belum di-encode, namun seharusnya sudah dari tahap clustering) akan di-encode. Kolom `Cluster` juga akan diperlakukan sebagai fitur.
3.  **Pembagian Data**: Data dibagi menjadi training set (80%) dan testing set (20%) menggunakan `train_test_split`.
4.  **Standardisasi Data**: Fitur-fitur numerikal pada dataset klasifikasi diskalakan menggunakan `StandardScaler`.

### Algoritma Klasifikasi
Beberapa algoritma klasifikasi dieksplorasi dan dievaluasi:
1.  **Decision Tree Classifier**
2.  **Random Forest Classifier**
3.  **K-Nearest Neighbors (KNN) Classifier**

### Evaluasi Model
Model dievaluasi menggunakan metrik berikut:
* **Accuracy Score**
* **Confusion Matrix**
* **Classification Report** (Precision, Recall, F1-Score)

**Hasil (Contoh)**:
* **Decision Tree**: Menghasilkan akurasi yang sangat tinggi (mendekati 100%), yang mengindikasikan kemungkinan *overfitting*.
* **Random Forest**: Juga menunjukkan akurasi yang sangat tinggi, mengindikasikan kemungkinan *overfitting*.
* **KNN**: Menunjukkan akurasi yang tinggi, tetapi sedikit lebih rendah dibandingkan Decision Tree dan Random Forest, dan mungkin lebih umum.

### Kesimpulan dan Rekomendasi
* Model klasifikasi yang dibangun menunjukkan performa yang sangat baik, namun akurasi yang mendekati sempurna (100%) pada model seperti Decision Tree dan Random Forest mungkin mengindikasikan adanya *overfitting* atau dataset yang terlalu mudah untuk diklasifikasikan.
* **Rekomendasi Tindakan Lanjutan**:
    * Lakukan validasi tambahan menggunakan cross-validation (k-folds) untuk memastikan model tidak overfitting.
    * Evaluasi performa dengan dataset baru untuk melihat apakah hasil tetap konsisten.
    * Lakukan Feature Engineering atau Feature Selection jika ditemukan fitur yang sangat dominan dan menyebabkan model terlalu "sempurna".
    * Jika ingin eksplorasi lebih lanjut, coba pendekatan lain seperti SVM atau Neural Networks untuk membandingkan hasilnya.
    * Lakukan Hyperparameter Tuning (misalnya pruning untuk Decision Tree, menyesuaikan jumlah estimator untuk Random Forest, atau nilai k untuk KNN) untuk meningkatkan generalisasi model.

## Cara Menjalankan Proyek
1.  **Kloning Repositori:**
    ```bash
    git clone <https://github.com/Bagusdarmawan11/Churn_Prediction_Clustering_Classification>
    cd <NAMA_REPOSITORI>
    ```

2.  **Siapkan Lingkungan Python:**
    Disarankan untuk menggunakan `conda` atau `venv`.
    ```bash
    conda create -n churn_env python=3.9
    conda activate churn_env
    # atau
    python -m venv churn_env
    source churn_env/bin/activate # di Linux/macOS
    # atau
    .\churn_env\Scripts\activate # di Windows
    ```

3.  **Instal Dependensi:**
    ```bash
    pip install pandas numpy scikit-learn matplotlib seaborn
    ```

4.  **Jalankan Notebook:**
    Buka kedua file `.ipynb` di lingkungan Jupyter Lab atau Jupyter Notebook:
    ```bash
    jupyter notebook
    # atau
    jupyter lab
    ```
    Jalankan sel-sel di `[Clustering]_Submission_Akhir_BMLP_Bagus_Darmawan_(Updated).ipynb` terlebih dahulu untuk menghasilkan `hasil_cluster.csv`, kemudian lanjutkan dengan `[Klasifikasi]_Submission_Akhir_BMLP_Bagus_Darmawan.ipynb`.

## Author
* Bagus Darmawan

---
