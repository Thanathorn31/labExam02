# labExam02

## 1. (Virtual Machine) 
ใน Aws และ connect ใน terminal **อย่าลืมใส่ security group SSH HTTP Custom TCP (8080 - 8090)

## 2. Install Nginx และแสดงหน้า Nginx
```
sudo apt update
sudo apt install nginx -y
sudo systemctl status nginx
```
- เปิดเบราว์เซอร์ไปที่ `http://<IP-ADDRESS>` เพื่อดูหน้าเว็บของ Nginx

## 3. แก้ไขไฟล์ HTML เพื่อเปลี่ยนหน้าเว็บ
```
sudo nano /var/www/html/index.html
```
- แก้ไขเนื้อหา HTML แล้วบันทึก
