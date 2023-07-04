
# Deteksi bentuk gambar menggunakan contour

Program ini adalah implementasi deteksi kontur pada sebuah gambar helm menggunakan OpenCV dan visualisasi hasilnya menggunakan Matplotlib. Langkah-langkahnya meliputi konversi gambar menjadi skala keabuan, aplikasi threshold biner, pengaplikasian Gaussian blur untuk mengurangi noise, deteksi tepi dengan metode Canny, pencarian kontur, dan penggambaran kontur pada gambar asli serta pada latar belakang putih terpisah.

Hasilnya ditampilkan dalam subplot-subplot dengan gambar asli, gambar hasil dengan kontur pada latar belakang putih, dan gambar hasil dengan kontur pada gambar asli. Program ini memanfaatkan fungsionalitas OpenCV dan Matplotlib untuk memproses gambar dan memvisualisasikan hasil deteksi kontur dengan tujuan analisis atau pengolahan lebih lanjut.


## Deployment

To deploy this project run

```
import cv2
import numpy as np
import matplotlib.pyplot as plt

image = cv2.imread("helm.jpg")

gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

ret, thresh = cv2.threshold(gray, 150, 255, cv2.THRESH_BINARY)

# berfungis mengurangi noise
blurred = cv2.GaussianBlur(gray, (5, 5), 0)

# deteksi tepi menggunakan metode Canny pada citra yang sudah dikurangi noisenya
edges = cv2.Canny(blurred, 50, 150)

# mencari kontur pada citra tepi dan mengembalikan kontur
contours, _ = cv2.findContours(edges.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

# digunakan untuk membuat citra kosong dengan latar belakang putih.
result_with_bg = np.ones_like(image) * 255

# menjadikan warna kontur sebagai merah
cv2.drawContours(result_with_bg, contours, -1, (0, 0, 255), 2)

# membuat salinan citra asli yang akan digunakan untuk menggambar kontur
image_copy = image.copy()

# menggambar kontur pada citra image copy menggunakan fungsi cv2.drawContours()
cv2.drawContours(image=image_copy, contours=contours, contourIdx=-1, color=(0, 0, 255), thickness=2, lineType=cv2.LINE_AA)

# Mengonversi citra image_copy dari mode warna BGR ke RGB
image_copy_rgb = cv2.cvtColor(image_copy, cv2.COLOR_BGR2RGB)

# Membuat sebuah figure dengan 1 baris dan 3 kolom
fig, axes = plt.subplots(1, 3, figsize=(12, 4))

# Menampilkan citra asli image pada subplot pertama (indeks 0).
axes[0].imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
axes[0].set_title("Original Image")


# Menampilkan citra hasil dengan kontur yang terdeteksi 
axes[1].imshow(cv2.cvtColor(result_with_bg, cv2.COLOR_BGR2RGB))
axes[1].set_title("Image with contours")


# Menampilkan citra hasil dengan kontur yang terdeteksi pada citra asli
axes[2].imshow(image_copy_rgb)
axes[2].set_title(" ")

# membuat tampilan subplot lebih rapi
plt.tight_layout()

# menampilkan plot yang telah dibuat
plt.show()
```
## Penjelasan          

```
import cv2
import numpy as np
import matplotlib.pyplot as plt
```

cv2 (OpenCV): Digunakan untuk melakukan operasi pengolahan gambar, seperti mengubah gambar ke skala keabuan (grayscale), melakukan filter Gaussian untuk mengurangi noise, melakukan deteksi tepi menggunakan metode Canny, dan menemukan kontur pada gambar menggunakan fungsi findContours().

numpy (Numerical Python): Digunakan untuk bekerja dengan struktur data array multidimensi yang efisien. Dalam konteks deteksi kontur, numpy digunakan untuk membuat array kosong yang memiliki dimensi yang sama dengan gambar, yang nantinya akan diisi dengan kontur yang terdeteksi.

matplotlib.pyplot (Matplotlib): Digunakan untuk memvisualisasikan gambar dan kontur yang telah dideteksi. Melalui fungsi-fungsi yang disediakan, matplotlib.pyplot memungkinkan pengguna untuk membuat plot, grafik, dan subplot untuk menampilkan gambar asli, gambar dengan kontur, serta visualisasi lainnya secara interaktif atau dalam bentuk gambar statis.
```
image = cv2.imread("helm.jpg")
```
Fungsi cv2.imread("helm.jpg") digunakan untuk membaca sebuah gambar dari file dengan nama "helm.jpg" dan menyimpannya ke dalam variabel image. Fungsi ini merupakan bagian dari library OpenCV (cv2)
```
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

ret, thresh = cv2.threshold(gray, 150, 255, cv2.THRESH_BINARY)
```
Berfungsi untuk konversi gambar ke skala keabuan (grayscale) menggunakan fungsi cv2.cvtColor(image, cv2.COLOR_BGR2GRAY). Hal ini berguna untuk mengubah gambar yang semula memiliki tiga saluran warna (BGR) menjadi hanya memiliki satu saluran warna (grayscale) yang mewakili intensitas keabuan.

Selanjutnya, kita menggunakan fungsi cv2.threshold(gray, 150, 255, cv2.THRESH_BINARY) untuk melakukan thresholding pada gambar skala keabuan. Thresholding adalah proses mengubah piksel-piksel dalam gambar menjadi hitam atau putih berdasarkan nilai ambang tertentu. Dalam contoh ini, piksel dengan intensitas keabuan di atas 150 akan diubah menjadi warna putih (255) dan piksel di bawah 150 akan diubah menjadi warna hitam (0). Hasilnya akan disimpan dalam variabel thresh.
```
# berfungis mengurangi noise
blurred = cv2.GaussianBlur(gray, (5, 5), 0)

# deteksi tepi menggunakan metode Canny pada citra yang sudah dikurangi noisenya
edges = cv2.Canny(blurred, 50, 150)

# mencari kontur pada citra tepi dan mengembalikan kontur
contours, _ = cv2.findContours(edges.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

# digunakan untuk membuat citra kosong dengan latar belakang putih.
result_with_bg = np.ones_like(image) * 255

# menjadikan warna kontur sebagai merah
cv2.drawContours(result_with_bg, contours, -1, (0, 0, 255), 2)

# membuat salinan citra asli yang akan digunakan untuk menggambar kontur
image_copy = image.copy()

# menggambar kontur pada citra image copy menggunakan fungsi cv2.drawContours()
cv2.drawContours(image=image_copy, contours=contours, contourIdx=-1, color=(0, 0, 255), thickness=2, lineType=cv2.LINE_AA)

# Mengonversi citra image_copy dari mode warna BGR ke RGB
image_copy_rgb = cv2.cvtColor(image_copy, cv2.COLOR_BGR2RGB)
```
blurred = cv2.GaussianBlur(gray, (5, 5), 0): Menggunakan filter Gaussian untuk mengurangi noise pada gambar skala keabuan (gray). Filter ini bekerja dengan memuluskan gambar dan mengurangi varian intensitas piksel yang dapat dianggap sebagai noise.

edges = cv2.Canny(blurred, 50, 150): Menggunakan metode Canny untuk mendeteksi tepi dalam gambar yang telah dikurangi noise. Metode Canny memanfaatkan perbedaan intensitas piksel untuk menemukan tepi dalam gambar.

contours, _ = cv2.findContours(edges.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE): Mencari kontur dalam gambar tepi (edges). Fungsi cv2.findContours() mengembalikan daftar kontur yang ditemukan dalam gambar.

result_with_bg = np.ones_like(image) * 255: Membuat citra kosong dengan latar belakang putih. Citra ini akan digunakan untuk menampilkan kontur dengan latar belakang putih.

cv2.drawContours(result_with_bg, contours, -1, (0, 0, 255), 2): Menggambar kontur pada citra kosong result_with_bg dengan warna merah. Kontur yang ditemukan dari gambar tepi akan digambar dengan warna merah pada citra ini.

image_copy = image.copy(): Membuat salinan citra asli yang akan digunakan untuk menggambar kontur.

cv2.drawContours(image=image_copy, contours=contours, contourIdx=-1, color=(0, 0, 255), thickness=2, lineType=cv2.LINE_AA): Menggambar kontur pada citra salinan image_copy dengan warna merah menggunakan fungsi cv2.drawContours(). Kontur yang ditemukan dari gambar tepi akan digambar dengan warna merah pada citra ini.

image_copy_rgb = cv2.cvtColor(image_copy, cv2.COLOR_BGR2RGB): Mengonversi citra image_copy dari mode warna BGR ke RGB. Hal ini diperlukan agar citra dapat ditampilkan dengan benar menggunakan fungsi plt.imshow() yang mengharapkan citra dalam format RGB.
```
# Membuat sebuah figure dengan 1 baris dan 3 kolom
fig, axes = plt.subplots(1, 3, figsize=(12, 4))

# Menampilkan citra asli image pada subplot pertama (indeks 0).
axes[0].imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
axes[0].set_title("Original Image")


# Menampilkan citra hasil dengan kontur yang terdeteksi 
axes[1].imshow(cv2.cvtColor(result_with_bg, cv2.COLOR_BGR2RGB))
axes[1].set_title("Image with contours")


# Menampilkan citra hasil dengan kontur yang terdeteksi pada citra asli
axes[2].imshow(image_copy_rgb)
axes[2].set_title(" ")

# membuat tampilan subplot lebih rapi
plt.tight_layout()

# menampilkan plot yang telah dibuat
plt.show()
```
digunakan untuk membuat tiga subplots dalam satu baris. Pada subplot pertama (axes[0]), gambar asli (image) ditampilkan dengan menggunakan fungsi imshow() setelah dikonversi dari mode warna BGR ke RGB menggunakan cv2.cvtColor(). Subplot ini diberi judul "Original Image" menggunakan set_title().

Pada subplot kedua (axes[1]), gambar hasil dengan kontur (result_with_bg) ditampilkan. Citra tersebut dikonversi dari mode warna BGR ke RGB dan kemudian ditampilkan dengan menggunakan imshow(). Subplot ini diberi judul "Image with contours".

Pada subplot ketiga (axes[2]), gambar asli (image_copy_rgb) dengan kontur yang digambar di atasnya ditampilkan. Citra tersebut sudah dikonversi ke mode warna RGB dan kemudian ditampilkan menggunakan imshow(). Subplot ini tidak diberi judul.

Setelah itu, plt.tight_layout() digunakan untuk mengatur jarak antara subplot agar tampilan lebih rapi. Terakhir, plt.show() digunakan untuk menampilkan plot dengan tiga subplot yang telah dibuat.
## Screenshots

![alt text](https://raw.githubusercontent.com/RnkFry/PA-PC_202131126_ALIFADHITYA_A/main/Hasil.png?raw=true)


## Authors

Alif Adhitya (202131126) - RnkFry

