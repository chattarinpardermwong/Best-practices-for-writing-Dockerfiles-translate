# Sort muiti-line arguments 

- ถ้าเป็นไปได้ ที่เราจะสามารถเรียงลำดับของ คำสั่งเป็นหลายบรรทัดลงมาเพื่อช่วยหลีกเลี่ยงการใช้ packages 
  ที่ซ้ำซ้อนและง่ายต่อการอัพเดรต อ่านได้ง่ายการเขียนแต่ล่ะบรรทัดจะถูกขั้นด้วย backslash(\) เช่น
  
  
    
    
   
      RUN apt-get update && apt-get install -y \
      bzr \
      cvs \
      git \
      mercurial \
      subversion



 
