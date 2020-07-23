### การสร้าง ephemeral container

Image ที่สร้างมาจาก `Dockerfile` ควรสร้าง containers ให้เป็น ephemeral เท่าที่สามารถเป็นไปได้ ซึ่ง ephemeral หมายถึง container ที่สามรถหยุด ทำลายตัวเองแล้วสร้างขึ้นมาใหม่แทนที่เก่า ด้วยชุดคำสั่งที่น้อยที่สุด 

อ้างอิงถึง [Processes](https://12factor.net/processes) ภายใต้กฎของ *The Twelve-factor App* การ run containers ควรเป็นแบบ stateless fashion.
