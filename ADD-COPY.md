# ADD / COPY

[Dockerfile reference for the ADD instruction](https://docs.docker.com/engine/reference/builder/#add)

[Dockerfile reference for the COPY instruction](https://docs.docker.com/engine/reference/builder/#copy)

## ADD / COPY มีการหน้าที่การทำงานที่คล้ายคลึงกันอยู่ 

### COPY

- โดยปกติแล้วจะแนะนำให้ใช้ `COPY` เนื่องจาก `COPY` จะมีความตรงจุดมากกว่า `ADD`
เนื่องจาก `ADD` จะมีการทำงานแบบเดียวกันกับ `COPY` แต่ `ADD` จะสามารถแยกไฟล์ประเภท `.tar`
ได้จาก Local รวมไปถึง Remote URL support ด้วย 
ซึ่งการใช้งานที่เหมาะที่สุดของ ADD คือการใช้เพื่อแยกไฟล์ .tar ไปยัง Image ปลายทาง

* ยกตัวอย่างการใช้งาน
หากว่ามี Dockerfile ที่มีการทำงานแตกต่างกัน(มีหลายตัวต่างกัน)
ให้ใช้คำสั่ง `COPY` ไปทีละไฟล์ แทนที่จะใช้รวดเดียวแบบรวบทั้งหมด 

โดยการทำวิธีนี้จะช่วยให้ Cache จากการ Build นั้นมีความถูกต้องในแต่ละไฟล์ 

     COPY requirements.txt /tmp/

     RUN pip install --requirement /tmp/requirements.txt
  
     COPY . /tmp/

- ผลลัพธ์ได้จะได้รับคือ Cache จะถูกต้องมากกว่าการ `COPY . /tmp/` ไว้ที่บนสุดของตัวอย่างข้างต้น

### ADD 
- เนื่องจากขนาดของ Image มีความสำคัญ (memory) การใช้ คำสั่ง `ADD` เพื่อดึงข้อมูลpackage แบบ Remote URLs จึงไม่ควรทำอย่างยิ่ง
- โดยจะแนะนำให้ใช้คำสั่ง `curl / wget` เพื่อช่วยในการจัดการในส่วนของการทำงานในครั้งเดียว
(สามารถลบไฟล์ที่ไม่ต้องการหลังจากแตกไฟล์โดยไม่ต้องเพิ่ม Layers ของ Imageซ้ำ)


โดยจะยกตัวอย่างที่ไม่ควรทำให้คือ

     ADD http://example.com/big.tar.xz /usr/src/things/

     RUN tar -xJf /usr/src/things/big.tar.xz -C /usr/src/things

     RUN make -C /usr/src/things all

โดยควรทำดังนี้

    RUN mkdir -p /usr/src/things \

    && curl -SL http://example.com/big.tar.xz \
    
    | tar -xJC /usr/src/things \
    
    && make -C /usr/src/things all
    
สำหรับไฟล์อื่น `(files , directories )` ที่ไม่ต้องการความสามารถในการแยกไฟล์ของ `ADD (.tar)` จะเหมาะแก่การการใช้ `COPY` มากกว่า 
