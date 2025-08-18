# ผลลัพธ์
## ดูขนาด Binary 
<img width="801" height="418" alt="Screenshot 2025-08-19 000716" src="https://github.com/user-attachments/assets/296527f9-3b8e-4bab-a3dd-de9a7c332bdc" />




## คำถามทบทวน

1. **Docker vs Native Setup** : อธิบายข้อดีของการใช้ Docker เปรียบเทียบกับการติดตั้ง ESP-IDF บน host system
   
   **ตอบ**
     - Docker: ติดตั้งง่าย ไม่ต้องยุ่งกับ dependency ของระบบหลัก แยกสภาพแวดล้อม ไม่ทำเครื่องรก ใช้งานข้ามเครื่อง/OS ได้เหมือนกันทุกที่

     - Native: ยืดหยุ่นกว่า เข้าถึง resource ได้เต็ม แต่ต้องติดตั้ง package เอง มีโอกาส conflict และย้ายเครื่องยาก
  
2.  **Build Process** : อธิบายขั้นตอนการ build ของ ESP-IDF ใน Docker container ตั้งแต่ source code จนได้ binary

    **ตอบ**
    - Mount Volume  โฟลเดอร์โปรเจกต์จาก host ถูกแชร์เข้า container

    - Execute Build Command  รัน idf.py build ใน container

    - CMake Configuration  CMake อ่าน CMakeLists.txt สร้าง build script + ตรวจ dependencies

    - Compilation  GCC คอมไพล์ source code → object files

    - Linking  รวม object files + libraries → binary/firmware

    - Output ไฟล์ .bin / .elf อยู่ในโฟลเดอร์ build/ และ sync กลับไปที่ host


3. **CMake Files** : บทบาทของไฟล์ CMakeLists.txt แต่ละไฟล์คืออะไร และทำงานอย่างไรใน Docker environment?

   **ตอบ**
   CMakeLists.txt (root): กำหนด project หลัก (ชื่อ, chip target ฯลฯ) CMakeLists.txt (component): บอกว่าโค้ดใน component นี้มีไฟล์อะไร ต้อง link อะไร
   
   Docker env: ใช้ CMake/Ninja ใน container จัดการ build โดยอ่าน CMakeLists.txt ทุกระดับ แล้วรวม workflow เดียวกัน
   
   
4. **Git Ignore** : ไฟล์ .gitignore มีความสำคัญอย่างไรสำหรับ ESP32 project development?
   **ตอบ**
   ป้องกันไม่ให้ไฟล์ที่ไม่จำเป็น (เช่น build/, .vscode/, binary, log) เข้าไปใน Git ทำให้ repo สะอาด sync กับทีมง่ายขึ้น

   
5. **Container Persistence** : ข้อมูลใดบ้างที่จะหายไปเมื่อ restart container และข้อมูลใดที่จะอยู่ต่อ?
   **ตอบ**
    หายไปเพราะ ไฟล์ที่แก้ใน container แต่ไม่ได้ mount (เช่น build output, ไฟล์ใน /tmp)

    อยู่ต่อไฟล์ใน volume/host bind (projects/, source code) เพราะ map มาจากเครื่องหลัก
 

7. **Development Workflow** : เปรียบเทียบ workflow การพัฒนาระหว่างการใช้ Docker กับการทำงานบน native system
   **ตอบ**
   Docker: สภาพแวดล้อมเหมือนกันทุกที่ ลดปัญหา dependency ต้องเข้า container ตลอดเวลา

   Native: สะดวก ใช้งานตรงบนเครื่อง เสี่ยง dependency พัง ย้ายเครื่องยาก

