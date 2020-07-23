### VOLUME

[คำสั่ง VOLUME อ้างอิงจาก Dockerfile](https://docs.docker.com/engine/reference/builder/#volume)

ควรใช้คำสั่ง `VOLUME` เพื่อใช้ในการเก็บ database storage area, configuration storage หรือ files/folders ใดๆ ที่สร้างโดย docker container และขอแนะนำให้ใช้ `VOLUME` สำหรับส่วน mutable หรือ/และ user-serviceable ของ image
