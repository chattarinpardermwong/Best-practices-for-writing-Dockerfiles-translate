### ONBUILD

[คำสั่ง ONBUILD อ้างอิงจาก Dockerfile](https://docs.docker.com/engine/reference/builder/#onbuild)

คำสั่ง `ONBUILD` จะทำงานหลังจากสร้าง `Dockerfile` ปัจจุบันเสร็จสมบูรณ์ คำสั่ง `ONBUILD` จะทำงานใน child image ใดๆ `FROM` image ปัจจุบัน ให้คิดว่า `ONBUILD` เป็นคำสั่งจาก parent `Dockerfile` ไปที่ child `Dockerfile`

Docker จะใช้งานคำสั่ง `ONBUILD` ก่อนคำสั่งใดๆ ก็ตามใน child `Dockerfile`

`ONBUILD` มีประโยชน์สำหรับ images ที่กำลังจะถูก build `FROM` image ที่กำหนด ตัวอย่างเช่นจะใช้ `ONBUILD` สำหรับ language stack image ที่สร้าง arbitrary user software ที่เขียนในภาษานั้นใน `Dockerfile` อย่างที่เห็นใน [Ruby’s `ONBUILD` variants](https://github.com/docker-library/ruby/blob/c43fef8a60cea31eb9e7d960a076d633cb62ba8d/2.4/jessie/onbuild/Dockerfile)

Images ที่สร้างขึ้นด้วย `ONBUILD` ควรมี tag ที่ต่างกัน ตัวอย่างเช่น `ruby:1.9-onbuild` หรือ `ruby:2.0-onbuild`

ระวังเมื่อใส่ `ADD` หรือ `COPY` ใน `ONBUILD` ไฟล์ "onbuild" image อาจเสียหายถ้าหาก new build’s context ไม่มี resource ที่เพิ่มเข้ามา การเพิ่ม tag ที่ต่างกันตามที่แนะนำข้างต้นช่วยลดปัญหานี้ได้โดยเป็นทางเลือกให้ผู้เขียน `Dockerfile`
