# ผลลัพธ์
## ดูขนาด Binary 
<img width="801" height="418" alt="Screenshot 2025-08-19 000716" src="https://github.com/user-attachments/assets/296527f9-3b8e-4bab-a3dd-de9a7c332bdc" />




## คำถามทบทวน

1. **Docker vs Native Setup** : อธิบายข้อดีของการใช้ Docker เปรียบเทียบกับการติดตั้ง ESP-IDF บน host system
   
   **ตอบ**
     - Docker: ติดตั้งง่าย ไม่ต้องยุ่งกับ dependency ของระบบหลัก แยกสภาพแวดล้อม ไม่ทำเครื่องรก ใช้งานข้ามเครื่อง/OS ได้เหมือนกันทุกที่

     - Native: ยืดหยุ่นกว่า เข้าถึง resource ได้เต็ม แต่ต้องติดตั้ง package เอง มีโอกาส conflict และย้ายเครื่องยาก
  
3.  **Build Process** : อธิบายขั้นตอนการ build ของ ESP-IDF ใน Docker container ตั้งแต่ source code จนได้ binary

    **ตอบ**
    1. Mount Volume  โฟลเดอร์โปรเจกต์จาก host ถูกแชร์เข้า container

    2. Execute Build Command  รัน idf.py build ใน container

    3. CMake Configuration  CMake อ่าน CMakeLists.txt สร้าง build script + ตรวจ dependencies

    4. Compilation  GCC คอมไพล์ source code → object files

    5. Linking  รวม object files + libraries → binary/firmware

    6. Output ไฟล์ .bin / .elf อยู่ในโฟลเดอร์ build/ และ sync กลับไปที่ host

