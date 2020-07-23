### USER

[คำสั่ง USER อ้างอิงจาก Dockerfile](https://docs.docker.com/engine/reference/builder/#user)

หาก service สามารถ run without privileges ได้ ให้ใช้ `USER` เพื่อเปลี่ยนเป็น non-root user ก่อน เริ่มด้วยการสร้าง user และ group ใน `Dockerfile` ตัวอย่างเช่น `RUN groupadd -r postgres && useradd --no-log-init -r -g postgres postgres`

> UID/GID ที่ชัดเจน
>
> Users และ groups ใน image ถูกกำหนดด้วย non-deterministic UID/GID ซึ่งจะมีการกำหนดค่าถัดไปของ UID/GID โดยไม่คำนึงถึงการ 
> rebuild ของ image ดังนั้นควรกำหนด UID/GID ให้ชัดเจน

> เนื่องจาก[จุดบกพร่องที่ยังไม่ได้รับการแก้](https://github.com/golang/go/issues/13548)ใน Go archive/tar package's 
> เกี่ยวกับการการจัดการ sparse files การพยายามสร้าง user ที่มีค่า UID ที่สูงอย่างมีนัยยะสำคัญใน Docker container 
> อาจทำให้ disk exhaustion ได้ เพราะ `/var/log/faillog` ใน container layer จะเต็มไปด้วย NULL (\0) characters 
> วิธีแก้คือให้ pass `--no-log-init` flag ที่ useradd แต่ใน Debian/Ubuntu คำสั่ง `adduser` จะไม่รองรับ flag นี้

หลีกเลี่ยงการติดตั้งหรือใช้ `sudo` เนื่องจาก unpredictable TTY และ signal-forwarding behavior ที่อาจทำให้เกิดปัญหา หากต้องการฟังก์ชั่นที่คล้ายกับ `sudo` เช่นการ initialize daemon ด้วยสิทธิ์ `root` แต่ run ด้วยสิทธิ์ non-`root` ให้ไปใช้ [“gosu”](https://github.com/tianon/gosu)

สุดท้ายเพื่อลด layers และความซับซ้อน หลีกเลี่ยงการสลับ `USER` ไปมาบ่อยๆ
