### Pipe Dockerfile ผ่าน `stdin`

Docker มีความสามารถในการสร้าง image ผ่าน `stdin` ซึ่งทำให้การสร้างไม่ต้องเขัยน Dockerfile ลง disk หรือใช้ในตอนที่ `Dockerfile` สร้างแล้วไม่ต้องเก็บไฟล์ไว้

> #### ตัวอย่างต่อไปนี้คือตัวอย่างแบบง่ายแต่สามารถใช้ method ที่มากับ `stdin` ได้
> 
> ตัวอย่าง (ทั้งสองคำสั่งเหมือนกัน)
> 
> ```
> echo -e 'FROM busybox\nRUN echo "hello world"' | docker build -
> ```
> 
> ```
> docker build -<<EOF
> FROM busybox
> RUN echo "hello world"
> EOF
> ```
> 
> สามารถแทนที่ตัวอย่างนี้ด้วยคำสั่งที่เหมาะสมกับการใช้งานได้
