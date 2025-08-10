# บันทึกผลการทดลอง


# คำถามทบทวน

# 1 Multiple Source Files: เหตุใดต้องแยก source code เป็นหลายไฟล์?
```c
เหตุผลที่แยก source code เป็นหลายไฟล์ เพราะ
- แบ่งโค้ดตามหน้าที่ (modular) → อ่านและแก้ไขง่าย
- ลดความซับซ้อนของไฟล์เดียวที่มีโค้ดยาวมาก
- สามารถ reuse โค้ดบางส่วนในโปรเจกต์อื่นได้
- ทำให้การคอมไพล์ incremental เร็วขึ้น (แก้เฉพาะไฟล์ที่เปลี่ยน)
```

# 2 CMakeLists.txt Management: การเพิ่มไฟล์ source ใหม่ต้องแก้ไขอะไรบ้าง?
```c
เมื่อเพิ่มไฟล์ .c ใหม่ ต้องเพิ่มชื่อไฟล์ใน SRCS หรือ add_executable() ใน CMakeLists.txt เพื่อให้ระบบ build รู้ว่าต้องคอมไพล์ไฟล์ใหม่ด้วย
```

# 3 Header Files: บทบาทของไฟล์ .h คืออะไร และทำไมต้องมี?
```c
บทบาท:
- ประกาศฟังก์ชัน, ตัวแปร, macro, struct เพื่อให้ไฟล์อื่นสามารถเรียกใช้ได้
- เป็น interface ระหว่างโมดูล
ทำไมต้องมี: เพื่อแยกส่วนประกาศและส่วน implement (.c) ออกจากกัน → ลดการซ้ำซ้อนและทำให้โค้ดเป็นระบบ
```

# 4 Include Directories: เหตุใด CMakeLists.txt ต้องระบุ INCLUDE_DIRS?
```c
เหตุผลที่ต้องระบุ INCLUDE_DIRS ใน CMakeLists.txt เพราะระบบ build ต้องรู้ว่าหาไฟล์ .h ได้จากโฟลเดอร์ไหน
ถ้าไม่ระบุ อาจเจอ error ว่า No such file or directory เวลาทำ #include "xxx.h"
```

# 5 Git Ignore: ไฟล์ .gitignore ช่วยอะไรในการจัดการ ESP32 project?
```c
- .gitignore ช่วยกำหนดว่าไฟล์/โฟลเดอร์ไหนไม่ต้อง track ใน Git เช่น
  - ไฟล์ binary (.bin, .elf)
  - โฟลเดอร์ build
  - ไฟล์ config ส่วนตัว
- ทำให้ repository สะอาดและไม่บวม
```

# 6 Task Management: การใช้ FreeRTOS task ในโมดูล LED ช่วยอะไร?
```c
การใช้ FreeRTOS task ในโมดูล LED ทำให้สามารถควบคุมการกระพริบ LED แบบ asynchronous
ข้อดี:
- LED ทำงานพร้อมกับ task อื่นได้
- สามารถตั้งความถี่/รูปแบบการกระพริบอิสระจาก main loop
```

# 7 Code Organization: ข้อดีของการแยกโมดูล sensor, display, led เป็นไฟล์แยกคืออะไร?
```c
ข้อดี:
- แต่ละโมดูลมีความรับผิดชอบชัดเจน (Single Responsibility Principle)
- ปรับปรุงหรือแก้ไขบางโมดูลได้โดยไม่กระทบส่วนอื่น
- ทำงานเป็นทีมง่ายขึ้น (คนละไฟล์ คนละส่วนงาน)
- เพิ่มโอกาส reuse code
```
