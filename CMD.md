# CMD
[Dockerfile reference for the CMD instruction](Dockerfile reference for the CMD instruction)

การใช้คำสั่ง `CMD` เพื่อ Run Software ที่มีอยู่ใน Image จะใช้ในลักษณะดังนี้

 - `CMD ["executable", "param1", "param2"…]`
 
 แต่ทว่าหาก Image ที่มีมีการใช้งาน Services ( Apache , Rails ) จะใช้ในลักษณะดังนี้

 - `CMD ["apache2","-DFOREGROUND"]`

ในกรณีข้างต้นยังเหมาะแก่การทำ Service-based-Image. เช่น Perl, Python Php 

โดยมีการใช้ในลักษณะดังนี้ 

 - `CMD ["perl", "-de0"], CMD ["python"], or CMD ["php", "-a"]`
 
 การทำงานข้างต้น จะมีการทำงานคล้ายกับ `docker run -it python` โดยจะถูกส่งลงไปใน Unsable Shell
 
## รูปแบบ CMD ที่ไม่ควรใช้ 
 
 - `CMD ["param", "param"]` เว้นแต่ว่าใช้ร่วมกับ [ENTRYPOINT](https://docs.docker.com/engine/reference/builder/#entrypoint) หากยังไม่มั่นใจในความรู้ด้านนี้อาจก่อผลเสียได้

