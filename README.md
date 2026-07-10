# ProjectCNN_Helmetman
#วัตถุประสงค์
เพื่อรวบรวมข้อมูลสถิติที่แม่นยำเกี่ยวกับการสวมหมวกกันน็อคในมหาวิทยาลัยเพื่อส่งเสริมความปลอดภัย
เพื่อพัฒนาโมเดล AI ที่สามารถตรวจจับการสวมหมวกกันน็อคและการไม่สวมหมวกกันน็อคจากภาพได้

#รายละเอียดชุดข้อมูลที่นำมาใช้ 
1.จัดหาเอง ชุดข้อมูลนี้ได้จากการลงพื้นที่เก็บข้อมูล ณ มหาวิทยาลัยเทคโนโลยีพระจอมเกล้าพระนครเหนือ วิทยาเขตปราจีนบุรี
2.รับข้อมูลมา ได้เอาข้อมูลมาจากแหล่งอ้างอิงเพิ่มเติมอีก https://www.kaggle.com/datasets/abuzarkhaaan/helmet-dataset-cls/data 

#การแบ่งชุดข้อมูล (Train, Test, Validate)
trainset 80% 
testset 10% 
validate 10% 

Class 1 สวมครบทุกคน
หาเอง 300 
ข้อมูลจาก kaggle 155 รูป

Class 2 ไม่สวมเลยสักคน
หาเอง 300 
ข้อมูลจาก kaggle 155 รูป

Class 3 ไม่สวมบางคน
หาเอง 455

#การจัดเตรียมข้อมูล
1. จัดหาข้อมูล
-ถ่ายเอง โดยจะนัดกลุ่มตัวอย่างออกมาถ่าย ที่ มจพ ปราจีนบุรี เพื่อรวบรวมข้อมูลนำไปใช้  train model 
-รับข้อมูลมา โดยเราจะหาข้อมูล ที่เคยมีคนทำคล้ายกัน คัดข้อมูลของเขามาใช้ train model

2. จัดแบ่งข้อมูล
     โดยเราจะนำข้อมูลที่หามาได้ นำมาจำแนก แบ่งออกเป็นแต่ละ
     คลาสโดยจะจัดทำ Folder แยกออกจากกัน และทำการแบ่ง 
     ชุดข้อมูลออกเป็น 3 ชุด train set , validation set , test set

3. ทำ data augment ที่ใช้ก็จะมี
  rescale
  rotation_range
  width_shift_range
  height_shift_range
  shear_range
  zoom_range
  horizontal_flip
  brightness_range

4.  ทำการ Resize และ Normalize
     โดยขนาด ที่ปรับ 224 * 224 * 3

5.  แบ่งข้อมูลเป็น Train / Validation / Test set
Train 80 %
Validation 10 %
Test 10 %

6. เก็บ Label Map ไว้ใช้ตอนทำนายTrain 80 %
class 0  All wearing helmet
class 1    No helmet
class 2   Patial use

#เปรียบเทียบข้อมูล
Model	Test Acc	 F1 (Weight)	recall(Weight)	Precision(Weight)	Size (MB)
Base CNN	92.59%	0.9250	0.9259	0.9260	76.3
GAP+KR	86.67%	0.8591	08667	0.8984	6.08
VGG+GAP	85.19%	08514	0.8519	0.8560	74.1

*Best Model test set*
accuracy   92.59%
loss       0.2133

#สรุปผลการทดลอง
ผลการทดลองแสดงให้เห็นว่าโมเดลสามารถจำแนกภาพได้แม่นยำสูง โดย Test Accuracy อยู่ที่ประมาณ 92–93% และ Weighted F1-score ประมาณ 0.92–0.93 
ซึ่ง Recall ของคลาสสำคัญ “No_helmet” อยู่ที่ 0.84 แสดงให้เห็นว่าโมเดลสามารถจับผู้ไม่ใส่หมวกได้ดี แม้ dataset จะมีขนาดเล็ก การใช้ Dropout ช่วยลด overfitting 
และทำให้โมเดล generalize กับภาพ unseen ได้ดี สรุปได้ว่าโมเดลมีความพร้อมสำหรับการนำไปใช้ตรวจสอบความปลอดภัยผู้ขับขี่บนถนนจริง และสามารถปรับปรุงเพิ่มเติมโดยเพิ่ม dataset

#รายละเอียดเพิ่มเติม https://canva.link/so44dtbrik7t5h5
