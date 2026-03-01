# 🐳 Laporan Praktikum Pertemuan 02
## Docker Fundamentals

---

## 👤 Identitas Mahasiswa

| Item | Keterangan |
|------|------------|
| **Nama** | Syarifah Azizah M |
| **NIM** | 105841106423 |
| **Kelas** | 5A |
| **Tanggal** | 2026-03-01 |

---

## 📚 Pemahaman Docker

### Apa itu Docker?

Docker adalah sebuah platform perangkat lunak yang berfungsi sebagai wadah atau kontainer (containerization) untuk menjalankan aplikasi secara terisolasi. Secara fundamental, Docker memungkinkan pengembang untuk membungkus kode aplikasi beserta seluruh dependensinya seperti pustaka (libraries), konfigurasi sistem, dan runtime ke dalam satu paket tunggal yang disebut Image. Teknologi ini memberikan efisiensi tinggi karena setiap kontainer berbagi kernel sistem operasi yang sama namun tetap memiliki ruang kerja privat. Hal ini menjamin portabilitas aplikasi agar dapat berjalan secara konsisten di berbagai infrastruktur tanpa mengalami kendala perbedaan konfigurasi sistem. Penggunaan Docker mengoptimalkan penggunaan sumber daya perangkat keras dan mempercepat siklus distribusi perangkat lunak melalui standardisasi lingkungan yang mutlak.

### Komponen Utama Docker

Arsitektur Docker didasarkan pada tiga komponen inti yang bekerja secara berkesinambungan untuk menjamin efisiensi kontainerisasi. Komponen pertama adalah Docker Images, yang merupakan templat statis bersifat read-only dan berfungsi sebagai cetak biru (blueprint) aplikasi. Image ini menyimpan seluruh konfigurasi, pustaka, dan instruksi yang diperlukan agar aplikasi dapat berjalan secara konsisten di berbagai lingkungan tanpa adanya perubahan data. Selanjutnya terdapat Docker Containers, yang didefinisikan sebagai unit eksekusi aktif atau perwujudan nyata dari sebuah Image. Jika Image dianggap sebagai rencana pembangunan, maka Kontainer adalah aplikasi yang sudah berjalan di atas sistem operasi. Kontainer menyediakan ruang isolasi yang aman bagi aplikasi, sehingga satu proses tidak akan mengintervensi proses lainnya meskipun berjalan pada infrastruktur fisik yang sama.
Komponen terakhir adalah Docker Registry, yang berperan sebagai infrastruktur penyimpanan dan distribusi Image. Registry bertindak sebagai repositori pusat yang memungkinkan pengembang untuk mengunggah hasil pekerjaan mereka atau mengunduh berbagai Image publik yang tersedia secara global. Melalui Docker Hub sebagai registry terbesar, kolaborasi dan sinkronisasi antar tim pengembang dapat dilakukan dengan cepat karena standar Image yang digunakan tetap terjaga keutuhannya.

### Perbedaan Docker vs Virtual Machine

Perbedaan fundamental antara Docker Container dan Virtual Machine (VM) terletak pada tingkat abstraksi perangkat keras dan penggunaan sumber daya sistem. Virtual Machine bekerja dengan mengabstraksi perangkat keras fisik melalui lapisan hypervisor, di mana setiap unitnya memerlukan sistem operasi tamu (Guest OS) yang lengkap untuk dapat berjalan. Hal ini menyebabkan penggunaan ruang penyimpanan yang besar, konsumsi memori yang tinggi, serta waktu pemuatan (boot time) yang relatif lambat karena harus menginisialisasi seluruh kernel sistem operasi dari awal.
Sebaliknya, Docker Container bekerja dengan melakukan abstraksi pada tingkat aplikasi di atas kernel sistem operasi inang (Host OS). Kontainer tidak memerlukan sistem operasi tamu secara mandiri, melainkan berbagi kernel yang sama dengan komputer induk namun tetap menjaga isolasi proses secara ketat. Pendekatan ini membuat Docker jauh lebih ringan, efisien dalam penggunaan CPU dan RAM, serta mampu dijalankan dalam waktu hitungan detik. Meskipun VM menawarkan isolasi keamanan yang lebih kuat karena pemisahan kernel secara total, Docker memberikan keunggulan mutlak dalam hal portabilitas dan kecepatan distribusi aplikasi di lingkungan pengembangan modern.

---

## 🔧 Praktik Docker Commands

### Output docker ps -a

```
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS          PORTS
                 NAMES
0a2fb426c2b3   nginx:alpine           "/docker-entrypoint.…"   7 seconds ago   Up 6 seconds    0.0.0.0:8080->80/tcp, [::]:8080->80/tcp       web-praktikum
```

### Output docker images

```
IMAGE                               ID             DISK USAGE   CONTENT SIZE   EXTRA
app-golang:1.0.0                    9c6429cc3bbe        492MB          120MB
confluentinc/cp-kafka:7.5.0         fbbb6fa11b25       1.33GB          439MB    U 
confluentinc/cp-zookeeper:7.5.0     02f6c042bb9a       1.33GB          439MB    U 
flask-praktikum:v1                  876708500c89        210MB         51.1MB
hello-world:latest                  ef54e839ef54       25.9kB         9.52kB    U 
mongo-express:latest                1b23d7976f02        287MB         59.8MB    U 
mongo:latest                        7f5bbdafebde       1.26GB          330MB    U 
nginx:alpine                        1d13701a5f9f       93.4MB         26.9MB
nginx:latest                        7272239bd214        240MB         65.7MB    U 
postgres:15-alpine                  5fe8ca7fc662        392MB          110MB
praktikum-docker:v1                 7818eced23d7        210MB         51.1MB
```

### Docker Run Command

```bash
docker run -d -p 5000:5000 --name flask-web flask-praktikum:v1
```

---

## 📄 Dockerfile

### Isi Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

EXPOSE 5000

CMD ["python", "app.py"]
```

### Penjelasan Dockerfile

Baris FROM python:3.11-slim menunjukkan bahwa proses pembuatan image akan menggunakan Python versi 3.11 sebagai fondasi utama. Versi slim dipilih karena ukurannya lebih kecil sehingga lebih hemat ruang penyimpanan dan lebih cepat saat proses pengunduhan.

Perintah WORKDIR /app berfungsi untuk menetapkan folder kerja di dalam container. Artinya, semua instruksi berikutnya seperti menyalin file atau menjalankan perintah akan dilakukan di dalam direktori tersebut sehingga struktur aplikasi menjadi lebih terorganisasi.

Instruksi COPY requirements.txt . digunakan untuk memindahkan file requirements.txt dari komputer lokal ke dalam container. File ini berisi daftar pustaka atau dependensi yang dibutuhkan agar aplikasi dapat berjalan dengan baik.

Perintah RUN pip install --no-cache-dir -r requirements.txt dijalankan saat proses build untuk menginstal seluruh dependensi yang tercantum di dalam file requirements.txt. Opsi no-cache-dir digunakan agar file sementara hasil instalasi tidak disimpan, sehingga ukuran image tetap lebih kecil.

Baris COPY app.py . berfungsi untuk menyalin file utama aplikasi ke dalam container. Dengan demikian, kode program yang telah dibuat dapat dijalankan di dalam lingkungan container.

Instruksi EXPOSE 5000 memberikan informasi bahwa aplikasi di dalam container menggunakan port 5000. Meskipun tidak langsung membuka port tersebut, perintah ini berfungsi sebagai dokumentasi agar pengguna mengetahui port yang digunakan oleh aplikasi.

Terakhir, CMD ["python", "app.py"] menentukan perintah yang otomatis dijalankan ketika container mulai berjalan. Dalam hal ini, container akan langsung mengeksekusi file app.py menggunakan interpreter Python sehingga aplikasi dapat aktif saat container dijalankan.

---

## 🐙 Docker Compose

### Isi docker-compose.yml

```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - db

  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: praktikum
      POSTGRES_PASSWORD: devops123
      POSTGRES_DB: praktikum_db
    ports:
      - "5432:5432"
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:
```

### Penjelasan Docker Compose

Baris version: '3.8' menunjukkan bahwa file docker-compose ini menggunakan format versi 3.8. Versi ini menentukan aturan dan fitur yang dapat digunakan dalam konfigurasi Docker Compose tersebut.

Bagian services berisi daftar layanan atau container yang akan dijalankan secara bersamaan. Dalam konfigurasi ini terdapat dua layanan, yaitu web dan db, yang saling terhubung dalam satu aplikasi.

Pada service web, baris build: . berarti Docker akan membangun image menggunakan Dockerfile yang berada di folder yang sama dengan file docker-compose.yml. Kemudian bagian ports dengan nilai "5000:5000" menghubungkan port 5000 pada komputer host dengan port 5000 di dalam container sehingga aplikasi dapat diakses melalui browser. Bagian depends_on menunjukkan bahwa service web akan dijalankan setelah service db aktif terlebih dahulu, karena web kemungkinan membutuhkan database untuk bekerja dengan baik.

Pada service db, baris image: postgres:15-alpine menandakan bahwa container database menggunakan image PostgreSQL versi 15 dengan varian alpine yang lebih ringan. Bagian environment digunakan untuk menetapkan variabel lingkungan seperti nama pengguna database, kata sandi, dan nama database yang akan dibuat saat container pertama kali dijalankan. Selanjutnya, bagian ports dengan nilai "5432:5432" menghubungkan port database di dalam container ke port pada komputer host agar dapat diakses dari luar container jika diperlukan. Bagian volumes dengan nilai db_data:/var/lib/postgresql/data digunakan untuk menyimpan data database secara permanen sehingga data tidak hilang meskipun container dihentikan atau dihapus.

Terakhir, bagian volumes di bagian bawah mendefinisikan volume bernama db_data. Volume ini dibuat oleh Docker untuk menyimpan data database secara terpisah dari container, sehingga data tetap aman dan dapat digunakan kembali saat container dijalankan ulang.

### Output Docker Compose Up

```
time="2026-03-02T00:22:56+07:00" level=warning msg="C:\\Users\\T430S\\devops-ci-cd-practicum-syarfazizh\\pertemuan-02\\docker-compose\\docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion"
[+] Building 9.8s (13/13) FINISHED
 => [internal] load local bake definitions                                                    0.0s 
 => => reading from stdin 618B                                                                0.0s 
 => [internal] load build definition from Dockerfile                                          0.6s 
 => => transferring dockerfile: 215B                                                          0.2s 
 => [internal] load metadata for docker.io/library/python:3.11-slim                           4.1s 
 => [auth] library/python:pull token for registry-1.docker.io                                 0.0s 
 => [internal] load .dockerignore                                                             0.1s 
 => => transferring context: 2B                                                               0.0s 
 => [1/5] FROM docker.io/library/python:3.11-slim@sha256:c8271b1f627d0068857dce5b53e14a95586  0.2s 
 => => resolve docker.io/library/python:3.11-slim@sha256:c8271b1f627d0068857dce5b53e14a95586  0.2s 
 => [internal] load build context                                                             0.4s 
 => => transferring context: 285B                                                             0.1s 
 => CACHED [2/5] WORKDIR /app                                                                 0.0s 
 => CACHED [3/5] COPY requirements.txt .                                                      0.0s 
 => CACHED [4/5] RUN pip install --no-cache-dir -r requirements.txt                           0.0s 
 => CACHED [5/5] COPY app.py .                                                                0.0s 
 => exporting to image                                                                        1.1s 
 => => exporting layers                                                                       0.0s 
 => => exporting manifest sha256:3f095e7d160421dfe7665b7b2aa31e5e11d84a899b8bde6fdb6f81417ba  0.1s 
 => => exporting config sha256:300e028eaaef18e7375932c6b2e7151a345bf788c080b9de2e1a160bb0220  0.0s 
 => => exporting attestation manifest sha256:3221345d96bdcb78847cc164d41c519d1296cf0a95e1cac  0.4s
 => => exporting manifest list sha256:b499424cd182556d6786750fa74586d8a0d4448c65696da5bbeb4f  0.1s 
 => => naming to docker.io/library/docker-compose-web:latest                                  0.0s 
 => => unpacking to docker.io/library/docker-compose-web:latest                               0.1s 
 => resolving provenance for metadata file                                                    0.2s 
[+] Running 5/5
 ✔ docker-compose-web              Built                                                      0.0s 
 ✔ Network docker-compose_default  Created                                                    0.5s 
 ✔ Volume docker-compose_db_data   Created                                                    0.2s 
 ✔ Container docker-compose-db-1   Started                                                   12.2s 
 ✔ Container docker-compose-web-1  Started  
```

---

## 📸 Screenshots

| No | Screenshot | Keterangan |
|----|------------|------------|
| 1 | ![Container Running](screenshots/01-container-running.png) | Container yang sedang berjalan |
| 2 | ![Docker Images](screenshots/02-docker-images.png) | Daftar Docker images |
| 3 | ![Dockerfile](screenshots/03-dockerfile-content.png) | Isi file Dockerfile |
| 4 | ![Docker Build](screenshots/04-docker-build.png) | Proses docker build |
| 5 | ![App Browser](screenshots/05-app-browser.png) | Aplikasi berjalan di browser |
| 6 | ![Compose Up](screenshots/06-compose-up.png) | Docker Compose up |

---

## 💭 Refleksi & Kesimpulan

### Yang Dipelajari

Melalui praktikum ini, pemahaman mendalam mengenai efisiensi alur kerja pengembangan perangkat lunak modern dapat dicapai melalui penerapan teknologi kontainerisasi. Pembelajaran utama mencakup kemampuan untuk mengisolasi aplikasi beserta seluruh dependensinya ke dalam satu unit yang konsisten, sehingga meminimalisir konflik konfigurasi antar lingkungan pengembangan. Praktikum ini juga memberikan penguasaan teknis dalam manajemen siklus hidup aplikasi, mulai dari pembuatan Docker Image yang terstandarisasi hingga pengoperasian Docker Container yang ringan dan responsif. praktikum ini juga menekankan pentingnya kolaborasi global melalui penggunaan Docker Registry atau Docker Hub. Kemampuan untuk mengunggah dan mendistribusikan Image ke repositori publik memastikan bahwa aplikasi dapat diakses dan dijalankan oleh tim lain dengan spesifikasi yang identik.

### Manfaat Docker

Docker mentransformasi metodologi pengembangan perangkat lunak dengan menghadirkan standarisasi lingkungan yang mutlak melalui konsep kontainerisasi. Teknologi ini mengeliminasi permasalahan klasik mengenai perbedaan konfigurasi antar perangkat keras maupun sistem operasi, sehingga pengembang dapat menjamin bahwa aplikasi yang berjalan di lingkungan lokal akan berfungsi secara identik saat diimplementasikan pada server pengujian maupun produksi. Dengan membungkus seluruh dependensi, pustaka, dan konfigurasi ke dalam sebuah Image, Docker menciptakan konsistensi yang mempercepat proses debugging dan meminimalisir risiko kesalahan teknis yang disebabkan oleh perbedaan infrastruktur.

### Tantangan dan Solusi

Tantangan utama yang dihadapi selama praktikum ini terletak pada konfigurasi awal lingkungan kerja dan sinkronisasi antara perangkat lokal dengan repositori jarak jauh. Kendala teknis seperti perbedaan versi sistem operasi dan ketidakcocokan dependensi seringkali menyebabkan kegagalan saat menjalankan instruksi dalam Docker Image. Selain itu, pemahaman mengenai manajemen penyimpanan dan jaringan antar kontainer memerlukan ketelitian ekstra agar setiap layanan dapat berkomunikasi secara optimal tanpa mengganggu stabilitas sistem inang. 
Solusi strategis yang diterapkan untuk mengatasi kendala tersebut adalah dengan melakukan standardisasi melalui penggunaan Dockerfile yang presisi dan pemanfaatan perintah eksekusi yang sesuai dengan dokumentasi resmi. Kendala pada proses sinkronisasi data ke GitHub diatasi dengan melakukan pembersihan riwayat commit dan penggunaan perintah paksa (force push) untuk memulihkan struktur folder yang sempat tidak terdeteksi oleh sistem kontrol versi. Melalui pendekatan pemecahan masalah yang sistematis dan referensi pada komunitas pengembang global, setiap hambatan teknis dapat diselesaikan dengan mengoptimalkan konfigurasi sistem dan memastikan seluruh komponen pendukung aplikasi terintegrasi dengan benar.

---

## ✅ Checklist

- [x] Berhasil membuat Dockerfile yang valid
- [x] Berhasil build Docker image
- [x] Container berjalan dan aplikasi bisa diakses
- [x] Docker Compose berhasil dijalankan
- [x] Semua screenshot lengkap dan jelas
- [x] Penjelasan ditulis dengan bahasa sendiri

---

*Laporan ini dibuat pada Senin, 2 Maret 2026*
