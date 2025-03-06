# labExam02

## 1. Create instance (Virtual Machine) 
ใน Aws และ connect ใน terminal **อย่าลืมใส่ security group SSH HTTP Custom TCP (8080 - 8090)

## 2. Install Nginx และแสดงหน้า Nginx
```
sudo apt update
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```
- เปิดเบราว์เซอร์ไปที่ `http://<IP-ADDRESS>` เพื่อดูหน้าเว็บของ Nginx

## 3. แก้ไขไฟล์ HTML เพื่อเปลี่ยนหน้าเว็บ
```
sudo nano /var/www/html/index.html
```
- แก้ไขเนื้อหา HTML แล้วบันทึก

## 4. Install Docker และ Run "hello-world"
```bash
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo docker run hello-world
```

## 5. Run Docker image ของ Nginx และแสดงหน้าเว็บ
```bash
sudo docker rm -f mynginx
sudo docker run --name mynginx -d -p 8080:80 nginx
```
- เปิดเบราว์เซอร์ไปที่ `http://<IP-ADDRESS:8080>`

## 6. สร้าง Docker image ใหม่ที่แสดงไฟล์ HTML
```bash
mkdir mynginx
cd mynginx
nano index.html
```
- สร้าง `Dockerfile`:
```Dockerfile
nano Dockerfile
FROM nginx
COPY index.html /usr/share/nginx/html/index.html
```
- สร้างและรัน Docker image:
```bash

docker build -t mycustomnginx .
docker run --name customnginx -d -p 8081:80 mycustomnginx
```
- เข้าไปที่ `http://<IP-ADDRESS>:8081`

## 7. อัปโหลด Image ไปยัง Docker Hub
```bash
docker login
docker tag mycustomnginx <DOCKER_USERNAME>/mycustomnginx
docker push <DOCKER_USERNAME>/mycustomnginx
```

## 8. ใช้ Volume เพื่อแสดงหน้าเว็บจากข้อ 3
```bash
docker run --name nginx-volume -d -p 8082:80 -v /var/www/html/index.html:/usr/share/nginx/html/index.html nginx
```
- เข้าไปที่ `http://<IP-ADDRESS>:8082`

## 9. รัน `docker-compose.yml` ที่ได้รับจากผู้คุมสอบ
```bash
docker-compose up -d
```
- เปิดเบราว์เซอร์และดูผลลัพธ์

## 10. สร้าง HTML ใหม่และรันด้วย `docker-compose.yml`
```bash
nano /home/user/new_index.html
```
- เขียน `docker-compose.yml`:
```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8083:80"
    volumes:
      - /home/user/new_index.html:/usr/share/nginx/html/index.html
```
- รัน:
```bash
docker-compose up -d
```
- เข้าไปที่ `http://<IP-ADDRESS>:8083` เพื่อดูผลลัพธ์
