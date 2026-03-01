# 🐳 Laporan Praktikum Pertemuan 02
## Docker Fundamentals

---

## 👤 Identitas Mahasiswa

| Item | Keterangan |
|------|------------|
| **Nama** | SYRIFA AZIZAH. M |
| **NIM** | 105841106423 |
| **Kelas** | 5A |
| **Tanggal** | 2026-02-25 |

---

## 📚 Pemahaman Docker

### Apa itu Docker?

Docker adalah platform yang digunakan untuk menjalankan aplikasi di dalam sebuah lingkungan terisolasi yang disebut container. Container memungkinkan aplikasi beserta seluruh dependensi, konfigurasi, dan library yang dibutuhkan dikemas menjadi satu kesatuan sehingga dapat dijalankan secara konsisten di berbagai lingkungan, baik di komputer pengembang maupun di server produksi. 

### Komponen Utama Docker

Image adalah template atau cetakan yang digunakan untuk membuat container. Image bersifat read-only dan berisi sistem dasar, dependensi, serta aplikasi yang akan dijalankan. Container adalah hasil instansiasi dari image. Container bersifat aktif dan dapat dijalankan, dihentikan, atau dihapus sesuai kebutuhan. Container bekerja secara terisolasi tetapi tetap berbagi kernel dengan sistem host sehingga lebih ringan dibandingkan mesin virtual. Registry adalah tempat penyimpanan image Docker. Registry yang paling umum digunakan adalah Docker Hub.

### Perbedaan Docker vs Virtual Machine

Virtual machine menjalankan sistem operasi lengkap di atas hypervisor, sehingga setiap mesin virtual memiliki sistem operasi sendiri. Hal ini membuat virtual machine lebih berat karena membutuhkan sumber daya yang lebih besar dan waktu boot yang lebih lama. Sebaliknya, container tidak menjalankan sistem operasi terpisah, melainkan berbagi kernel dengan sistem host. Karena itu, container lebih ringan, lebih cepat dijalankan, dan lebih efisien dalam penggunaan sumber daya.

---

## 🔧 Praktik Docker Commands

### Output docker ps -a

```
CONTAINER ID   IMAGE                  COMMAND                  CREATED          STATUS          PORTS                                         NAMES
a5ebdc00dadf   nginx:alpine           "/docker-entrypoint.…"   14 seconds ago   Up 12 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp       web-praktikum
```

### Output docker images

```
IMAGE                               ID             DISK USAGE   CONTENT SIZE   EXTRA
app-golang:1.0.0                    9c6429cc3bbe        492MB          120MB
confluentinc/cp-kafka:7.5.0         fbbb6fa11b25       1.33GB          439MB    U 
confluentinc/cp-zookeeper:7.5.0     02f6c042bb9a       1.33GB          439MB    U 
hello-world:latest                  ef54e839ef54       25.9kB         9.52kB    U 
mongo-express:latest                1b23d7976f02        287MB         59.8MB    U 
mongo:latest                        7f5bbdafebde       1.26GB          330MB    U 
nginx:alpine                        1d13701a5f9f       93.4MB         26.9MB    U 
nginx:latest                        7272239bd214        240MB         65.7MB    U   
praktikum-docker:v1                 4980d45c731b       92.5MB           26MB
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

FROM python:3.11-slim berarti image dasar yang digunakan adalah Python versi 3.11 dengan ukuran ringan. 
WORKDIR /app menetapkan direktori kerja di dalam container sehingga semua perintah berikutnya dijalankan di folder tersebut. 
COPY requirements.txt . menyalin file requirements dari komputer host ke dalam container. 
RUN pip install --no-cache-dir -r requirements.txt menjalankan perintah instalasi dependensi Python sesuai daftar pada file requirements. 
COPY app.py . menyalin file aplikasi ke dalam container. 
EXPOSE 5000 mendokumentasikan bahwa aplikasi akan berjalan pada port 5000. 
CMD ["python", "app.py"] menentukan perintah default yang dijalankan ketika container dimulai, yaitu menjalankan aplikasi Python.

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

Pada bagian version: '3.8' ditentukan versi format compose yang digunakan. Bagian services berisi daftar layanan atau container yang akan dijalankan, misalnya web, api, dan db. Setiap service dapat memiliki konfigurasi seperti image untuk menentukan image yang digunakan, build untuk membangun dari Dockerfile, ports untuk menghubungkan port host dengan port container, volumes untuk menyimpan data secara persisten, serta environment untuk mendefinisikan variabel lingkungan. Bagian depends_on menunjukkan ketergantungan antar service. Selain itu terdapat bagian networks untuk mengatur jaringan antar container dan volumes untuk mendefinisikan penyimpanan data yang digunakan bersama.

### Output Docker Compose Up

```
 Building 16.1s (12/12) FINISHED
 => [internal] load local bake definitions                                                                                                                                         0.0s 
 => => reading from stdin 632B                                                                                                                                                     0.0s 
 => [internal] load build definition from Dockerfile                                                                                                                               1.1s 
 => => transferring dockerfile: 215B                                                                                                                                               0.9s 
 => [internal] load metadata for docker.io/library/python:3.11-slim                                                                                                                2.7s 
 => [internal] load .dockerignore                                                                                                                                                  0.4s 
 => => transferring context: 2B                                                                                                                                                    0.0s 
 => [1/5] FROM docker.io/library/python:3.11-slim@sha256:fba6f3b73795df99960f4269b297420bdbe01a8631fc31ea3f121f2486d332d0                                                          0.3s 
 => => resolve docker.io/library/python:3.11-slim@sha256:fba6f3b73795df99960f4269b297420bdbe01a8631fc31ea3f121f2486d332d0                                                          0.3s 
 => [internal] load build context                                                                                                                                                  0.3s 
 => => transferring context: 285B                                                                                                                                                  0.1s 
 => CACHED [2/5] WORKDIR /app                                                                                                                                                      0.0s 
 => CACHED [3/5] COPY requirements.txt .                                                                                                                                           0.0s 
 => CACHED [4/5] RUN pip install --no-cache-dir -r requirements.txt                                                                                                                0.0s 
 => CACHED [5/5] COPY app.py .                                                                                                                                                     0.0s 
 => exporting to image                                                                                                                                                             1.0s 
 => => exporting layers                                                                                                                                                            0.0s 
 => => exporting manifest sha256:7cf0b02f4425281b00fe9ccb3587f4f7eb4799ed88162ae5dc1f4b9aaa4d8be9                                                                                  0.2s 
 => => exporting config sha256:fdd9a6adb827a9e7fb6e8015746d83fbb635aff39940ce6d630fb6fe7fdd60e3                                                                                    0.1s 
 => => exporting attestation manifest sha256:879df740087c120ca7438a994412bd9a63d04a69dbc6aab6046e85d8882b707a                                                                      0.2s
 => => exporting manifest list sha256:01c731f38131763fc9471237c8692dc4c8f54a64d949670ee107892e14097ae4                                                                             0.1s 
 => => naming to docker.io/library/task3-compose-web:latest                                                                                                                        0.0s
 => => unpacking to docker.io/library/task3-compose-web:latest                                                                                                                     0.1s 
 => resolving provenance for metadata file                                                                                                                                         1.0s 
[+] Running 5/5
 ✔ task3-compose-web              Built                                                                                                                                            0.0s 
 ✔ Network task3-compose_default  Created                                                                                                                                          0.5s 
 ✔ Volume task3-compose_db_data   Created                                                                                                                                          0.7s 
 ✔ Container task3-compose-db-1   Started                                                                                                                                         11.6s 
 ✔ Container task3-compose-web-1  Started        
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

Dari praktikum Docker ini, saya mempelajari bagaimana cara menjalankan aplikasi dalam container, membuat image sendiri menggunakan Dockerfile, serta mengelola beberapa container sekaligus dengan Docker Compose. Saya memahami bahwa Docker membantu menciptakan lingkungan yang konsisten sehingga aplikasi dapat berjalan tanpa perbedaan konfigurasi. Insight utama yang saya dapatkan adalah pentingnya standarisasi environment dalam pengembangan perangkat lunak agar proses deployment menjadi lebih mudah dan terkontrol.

### Manfaat Docker

Docker sangat membantu dalam pengembangan software karena memungkinkan aplikasi dikemas bersama dependensinya dalam satu paket yang portabel. Hal ini mempercepat proses pengujian, deployment, serta mempermudah kolaborasi antar tim.

### Tantangan dan Solusi

Tantangan yang saya hadapi selama praktikum adalah masalah jaringan dan keterbatasan ruang penyimpanan. Masalah jaringan terjadi saat proses mengunduh image dari Docker Hub, terutama jika koneksi internet tidak stabil. Solusinya adalah memastikan koneksi internet stabil dan menghindari penggunaan jaringan yang lambat saat proses pull atau build image. Tantangan kedua adalah ruang penyimpanan yang cepat berkurang karena ukuran image Docker yang cukup besar. Solusinya adalah menghapus image atau container yang tidak digunakan

---

## ✅ Checklist

- [x] Berhasil membuat Dockerfile yang valid
- [x] Berhasil build Docker image
- [x] Container berjalan dan aplikasi bisa diakses
- [x] Docker Compose berhasil dijalankan
- [x] Semua screenshot lengkap dan jelas
- [x] Penjelasan ditulis dengan bahasa sendiri

---

*Laporan ini dibuat pada Rabu, 25 Februari 2026*
