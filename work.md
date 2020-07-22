Leverage build cache
* ในตอนที่สร้าง image จะมี cache อยู่ในนั้น ถ้าหากว่าไม่ต้องการ cacheสามารถใช่คำสั่ง—no-cache = true บนคำสั่งdocker buildได้ แต่ถ้าหากต้องการใช้มันก็จำเป็นต้องรู้กฎดั้งนี้
ถ้าหากเรามีการสร้างimageหลักขึ้นมาแล้วมี cache เราจะสามารถนำcache ไปเปรียบเทียบกับ imageอื่นๆได้เลยว่ามีคำสั่งเดียวกันแน่นอน ไม่งั้น cacheจะไม่ถูกต้อง

Dockerfile instructions
* instruction พวกนี้มีไว้เพื่อช่วยเพิ่มประสิทธิภาพและพัฒนา

FROM
* หากเราใช้official imageเป็นพื้นฐาน imageของเรา ในรูบแบบนี้มีการแนะนำให้ใช้alpine imageเพราะว่ามีการควบคุมที่แน่นหนาและก็มีขนาดเล็กและยังสามารถใช้งานในlinuxได้แบบเต้มรูปแบบ

LABEL
* มีเอาไว้ให้เพื่อช่วยในการจัดระเบียน imageในโปรเจค
* สำหรับแต่ละlabelมีการเขียนขึ้นต้นไว้ว่า LABEL เช่น

`#Set one or more individual labels`

`LABEL com.example.version="0.0.1-beta"`

`LABEL vendor1="ACME Incorporated"`

`LABEL vendor2=ZENITH\ Incorporated`

`LABEL com.example.release-date="2015-02-12"`

`LABEL com.example.version.is-production=""`








	
