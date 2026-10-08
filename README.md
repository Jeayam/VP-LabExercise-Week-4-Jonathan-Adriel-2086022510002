### 1. A Text inside a Row overflows. Which part of "constraints go down, sizes go up, parent sets position" was violated, and by which widget?
Bagian "constraints go down" dilanggar oleh widget Row yang memberikan unbounded width constraint (lebar tanpa batas, $0 \le width \le \infty$) kepada anak-anaknya. Akibatnya, widget Text mengukur ukurannya berdasarkan panjang teks dan melebihi lebar layar yang tersedia.


### 2. Why is adding width: 150 to the text the wrong fix, even if the stripes disappear?
Menambahkan width: 150 adalah solusi hardcoded kaku yang hanya berfungsi untuk satu ukuran layar atau ukuran font tertentu. Perbaikan layout harus bersifat responsif dan relasional (seperti menggunakan Expanded atau Flexible) agar dapat beradaptasi di berbagai perangkat.



### 3. You fixed the landscape overflow by wrapping everything in a SingleChildScrollView and setting shrinkWrap: true on the list. Which test fails, and why does it matter once the data comes from an API?
**Tes 7 500 items — built lazily akan gagal karena shrinkWrap: true memaksa list merender seluruh 500 item di memori sekaligus untuk menghitung total tingginya. Hal ini menghancurkan *view virtualization* (lazy loading) dan dapat menyebabkan lag atau crash ketika data berukuran besar diambil dari API.


### 4. Why does the tablet layout use LayoutBuilder rather than MediaQuery.sizeOf(context)?
LayoutBuilder mengukur batasan (constraints) dari parent widget tempat layout dirender, sehingga adaptif baik saat window berubah ukuran maupun saat diletakkan di dalam multi-pane split view. Sebaliknya, MediaQuery.sizeOf(context) hanya mengukur total ukuran fisik layar perangkat.


### 5. The empty-data crash was not a layout error. Why does it belong in a layout lab anyway?
Penanganan data kosong membutuhkan perancangan antarmuka (UI layout) kondisional yang tepat (menampilkan widget empty state khusus ber-Key('empty-state') alih-alih list kosong atau mengalami crash). Layout yang tangguh (unbreakable) harus mampu menangani semua kondisi data secara visual.
