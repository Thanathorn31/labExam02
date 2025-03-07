# labExam02

## 1. Create instance (Virtual Machine) 



## 2.Connect Vm และ Install Nginx และแสดงหน้า Nginx 
```
sudo apt update
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```
- `http://<IP-ADDRESS>` เพื่อดูหน้าเว็บของ Nginx


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


- build docker and push run
```
docker build --platform linux/amd64 -t mycustomnginx .
docker tag mycustomnginx thanasmp/mycustomnginx
docker push thanasmp/mycustomnginx
```

- เข้าไปที่ `http://<IP-ADDRESS>:8081`

## 7. อัปโหลด Image ไปยัง Docker Hub
```
in vm 
docker login -u thanasmp
dckr_pat_H327C1sEMcgM4QQu7jdAXkyHjXA

docker stop customnginx
docker rm customnginx

docker pull thanasmp/mycustomnginx
docker run --name customnginx -d -p 8081:80 thanasmp/mycustomnginx
```

## 8. ใช้ Volume เพื่อแสดงหน้าเว็บจากข้อ 3
```bash
docker run --name mapping -p 8082:80 -v /var/www/html:/usr/share/nginx/html -d nginx

```
- เข้าไปที่ `http://<IP-ADDRESS>:8082`

## 9. รัน `docker-compose.yml` ที่ได้รับจากผู้คุมสอบ
```bash
docker-compose up -d
```
- เปิดเบราว์เซอร์และดูผลลัพธ์

## 10. สร้าง HTML ใหม่และรันด้วย `docker-compose.yml`

back to root folder
```
cd ~ 
```


create new file html
```
touch newindex.html
```


edit file html and write something
```
nano newindex.html

<h1>hii stamp</h1>
```

check file html

```
 cat newindex.html
```
create docker-compose.html

```
touch docker-compose.yml
```
edit file docker-compose
```
nano docker-compose.yml
```

put this 
```
version: '3.8'

services:
  web:
    image: nginx:latest
    container_name: my-nginx
    ports:
      - "8083:80"
    volumes:
      - ./newindex.html:/usr/share/nginx/html/index.html
    restart: always

```
then run
```
sudo apt update
sudo apt install docker-compose -y

docker compose up -d
```
stop
```
docker-compose down
```

- เข้าไปที่ `http://<IP-ADDRESS>:8083` เพื่อดูผลลัพธ์
