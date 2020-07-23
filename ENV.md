# ENV

[Dockerfile reference for the ENV instruction](https://docs.docker.com/engine/reference/builder/#env)

เพื่อจัดการให้ซอฟต์แวร์ใหม่ให้ทำงานได้สะดวกมากขึ้น เราจะสามารถใช้ `ENV`

เพื่อที่จะอัปเดต evironment variable สำหรับ software ที่ติดตั้งอยู่ใน container

โดยมีตัวอย่างดังนี้

- `ENV PATH /usr/local/nginx/bin:$PATH` โดยจะต้องตรวจสอบก่อนว่าคำสั่ง `CMD ["nginx"]` สามารถทำงานได้

คำสั่ง `ENV` มีประโยชร์สำหรับการจัดการ Environment variables และ ยังสามารถใช้เพื่อตั้งค่า Version เพื่อให้ง่ายแก่การจัดการ

- `ENV PG_MAJOR 9.3`
- `ENV PG_VERSION 9.3.4`
- `RUN curl -SL http://example.com/postgres-$PG_VERSION.tar.xz | tar -xJC /usr/src/postgress && …`
- `ENV PATH /usr/local/postgres-$PG_MAJOR/bin:$PATH`

วิธีการใช้คำสั่งดังนี้ช่วยให้เราสามารถใช้ `ENV` เพื่อให้สามารถรวม software ใน Container

โดยแต่ละบรรทัดจะสร้าง Layer เหมือนกับคำสั่ง `RUN` ยกตัวอย่างเช่นหากคุณยกเลิกการตั้งค่า environment variables

ในงานหน้าก็จะยังมีมันอยู่ใน Layer ต่อไปและไม่สามารถทิ้งได้

โดยมีคำสั่งดังนี้

-   `FROM alpine`
-   `ENV ADMIN_USER="mark"`
-   `RUN echo $ADMIN_USER > ./mark`
-   `RUN unset ADMIN_USER`
-   `$ docker run --rm test sh -c 'echo $ADMIN_USER'`

-   `mark` << ผลลัพธ์ที่ได้

เพื่อป้องกันสิ่งเหล้านั้น หากเราต้องการยกเลิก environment variables จะใช้คำสั่ง `RUN`

ควบคู่ไปกับ Command ของ Shell เพื่อตั้งค่าใช้งานและยกเลิก variables ทั้งหมดได้ภายใน layer เดียว

โดยการแยะคำสั่งคือ `;` หรือ `&&` 

ซึ่งถ้าเลือก `&&` ในการใช้งานหากมีคำสั่งที่ทำงานผิดพลาด `docker build `

จะทำให้ไม่สามารถใช้งานได้ตามปกติ จึงควรมีการใช้ `\` เพื่อให้ง่ายต่อการอ่าน

โดยจะมีตัวอย่างดังนี้

-   `FROM alpine`
-   `RUN export ADMIN_USER="mark" \`
-       && echo $ADMIN_USER > ./mark \
-       && unset ADMIN_USER
-   `CMD sh`
-   `$ docker run --rm test sh -c 'echo $ADMIN_USER'`


