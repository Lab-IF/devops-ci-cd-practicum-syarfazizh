# Task 1 - Docker Basic

## Perintah yang dijalankan

docker pull nginx:alpine
docker run -d --name web-praktikum -p 8080:80 nginx:alpine
docker ps
docker logs web-praktikum
docker stop web-praktikum
docker rm web-praktikum

## Penjelasan

Task ini bertujuan untuk memahami lifecycle container mulai dari pull image, menjalankan container, melihat status, hingga menghentikan dan menghapus container.