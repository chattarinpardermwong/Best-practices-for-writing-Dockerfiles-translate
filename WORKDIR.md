### WORKDIR

[คำสั่ง WORKDIR อ้างอิงจาก Dockerfile](https://docs.docker.com/engine/reference/builder/#workdir)

เพื่อความชัดเจนและน่าเชื่อถือ ควรใช้ absolute paths สำหรับ `WORKDIR` เสมอ นอกจากนี้ควรใช้ `WORKDIR` แทน proliferating instructions เช่น `RUN cd … && do-something` ซึ่งยากต่อการอ่าน แก้ไข และดูแล
