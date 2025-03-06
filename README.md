# labExam02

## 1. Create instance (Virtual Machine) 
ใน Aws และ connect ใน terminal **อย่าลืมใส่ security group SSH HTTP Custom TCP (8080 - 8090)
<img width="901" alt="Screenshot 2568-03-06 at 23 20 02" src="https://github.com/user-attachments/assets/f4e0b663-20b3-41a6-bafe-6e4264dc176f" />

## 2.Connect Vm และ Install Nginx และแสดงหน้า Nginx 
```
sudo apt update
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```
- `http://<IP-ADDRESS>` เพื่อดูหน้าเว็บของ Nginx
<img width="1122" alt="Screenshot 2568-03-06 at 23 22 30" src="https://github.com/user-attachments/assets/ce711df9-8a58-427e-a114-ebb7d954de44" />

## 3. แก้ไขไฟล์ HTML เพื่อเปลี่ยนหน้าเว็บ
```
sudo nano /var/www/html/index.

<h1>labExam</h1>
```
- แก้ไขเนื้อหา HTML แล้วบันทึก รีweb

## 4. Install Docker และ Run "hello-world"
```bash
sudo apt update
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
- `http://<IP-ADDRESS:8080>`

## 6. สร้าง Docker image ใหม่ที่แสดงไฟล์ HTML

- เปิดไฟล์ `.html`
- สร้าง `Dockerfile`:
```Dockerfile
FROM nginx:latest
COPY ./html /usr/share/nginx/html
EXPOSE 80
```

```
- build docker and push run
```bash

docker build -t mycustomnginx .

docker tag mycustomnginx thanasmp/mycustomnginx
docker push thanasmp/mycustomnginx
```

docker run --name customnginx -d -p 8081:80 mycustomnginx
- เข้าไปที่ `http://<IP-ADDRESS>:8081`

## 7. อัปโหลด Image ไปยัง Docker Hub
```bash
docker login -u thanasmp

dckr_pat_H327C1sEMcgM4QQu7jdAXkyHjXA

docker tag mycustomnginx thanasmp/mycustomnginx
docker push thanasmp/mycustomnginx
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
