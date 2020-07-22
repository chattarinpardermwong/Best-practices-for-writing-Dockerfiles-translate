# EXPOSE
[Dockerfile reference for the EXPOSE instruction](https://docs.docker.com/engine/reference/builder/#expose)
คำสั่ง EXPOSE เป็นการระบุพอร์ต รับการเชื่อมต่อ

สำหรับ Container เราควรจะใช้ Port ทั่วไป(Common port)

ยกตัวอย่างเช่น Image ที่มี Apache จะใช้

 - `EXPOSE 80`

สำหรับ Apllication เราควรใช้ Traditional port

ยกตัวอย่างเช่น Image ที่มี MongoDB จะใช้

 - `EXPOSE 27017`

รวมไปถึงการเข้าถึงจาก Users ภายนอกผู้ใช้งานสามารถใช้คำสั่ง

 - `docker run` + Flag indicating 
 
 เพื่อเลือกระบุ port ของผู้ใช้ โดยการเชื่อมต่อกันระหว่าง Container และ Docker นั้น
 ได้มีการจัดเตรียม Environment Variables เพื่อทำการเชื่อมต่อจากทาง Container 
 ไปทางผู้รับที่ต้นทาง ได้โดยดังนี้ 
 - `MYSQL_PORT_3306_TCP`
