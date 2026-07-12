# 🪖 ProjectCNN_Helmetman

**ระบบตรวจจับการสวมหมวกกันน็อคด้วย Deep Learning (CNN)**

โปรเจกต์นี้พัฒนาโมเดล Computer Vision เพื่อจำแนกภาพผู้ขับขี่ตามพฤติกรรมการสวมหมวกกันน็อค โดยมีเป้าหมายเพื่อสนับสนุนการรวบรวมสถิติความปลอดภัยในมหาวิทยาลัย และเป็นต้นแบบระบบตรวจสอบความปลอดภัยบนท้องถนน

---

## 📌 วัตถุประสงค์

- รวบรวมข้อมูลสถิติที่แม่นยำเกี่ยวกับการสวมหมวกกันน็อคภายในมหาวิทยาลัย เพื่อส่งเสริมความปลอดภัยของผู้ขับขี่
- พัฒนาโมเดล AI (CNN) ที่สามารถตรวจจับและจำแนกการสวมหมวกกันน็อคจากภาพถ่ายได้อย่างแม่นยำ

---

## 🗂️ ชุดข้อมูล (Dataset)

### แหล่งที่มา

| แหล่งข้อมูล | รายละเอียด |
|---|---|
| จัดเก็บเอง | ลงพื้นที่เก็บข้อมูล ณ มหาวิทยาลัยเทคโนโลยีพระจอมเกล้าพระนครเหนือ วิทยาเขตปราจีนบุรี |
| แหล่งอ้างอิงเพิ่มเติม | [Kaggle: Helmet Dataset (Classification)](https://www.kaggle.com/datasets/abuzarkhaaan/helmet-dataset-cls/data) |

### จำนวนภาพแยกตามคลาส

| Class | คำอธิบาย | จัดเก็บเอง | จาก Kaggle | รวม |
|---|---|---|---|---|
| 0 | สวมหมวกกันน็อคครบทุกคน (All wearing helmet) | 300 | 155 | 455 |
| 1 | ไม่สวมหมวกกันน็อคเลยสักคน (No helmet) | 300 | 155 | 455 |
| 2 | สวมหมวกกันน็อคบางคน (Partial use) | 455 | – | 455 |

### การแบ่งชุดข้อมูล

| ชุดข้อมูล | สัดส่วน |
|---|---|
| Train set | 80% |
| Validation set | 10% |
| Test set | 10% |

---

## ⚙️ ขั้นตอนการเตรียมข้อมูล (Data Preparation Pipeline)

1. **จัดหาข้อมูล**
   - ถ่ายภาพเอง: นัดกลุ่มตัวอย่างถ่ายภาพที่ มจพ. วิทยาเขตปราจีนบุรี
   - รวบรวมข้อมูลเพิ่มเติมจากชุดข้อมูลสาธารณะที่มีลักษณะใกล้เคียงกัน

2. **จัดแบ่งข้อมูล**
   - จำแนกภาพออกเป็น 3 คลาส และจัดเก็บใน Folder แยกตามคลาส
   - แบ่งข้อมูลเป็น Train / Validation / Test set

3. **Data Augmentation**
   - Rescale
   - Rotation range
   - Width shift range
   - Height shift range
   - Shear range
   - Zoom range
   - Horizontal flip
   - Brightness range

4. **Resize & Normalize**
   - ปรับขนาดภาพเป็น **224 × 224 × 3**

5. **แบ่งชุดข้อมูลสำหรับ Train / Validation / Test**
   - Train: 80% | Validation: 10% | Test: 10%

6. **จัดเก็บ Label Map**

   | Class | Label |
   |---|---|
   | 0 | All wearing helmet |
   | 1 | No helmet |
   | 2 | Partial use |

---

## 📊 ผลการทดลองและเปรียบเทียบโมเดล

| Model | Test Accuracy | F1 (Weighted) | Recall (Weighted) | Precision (Weighted) | Size (MB) |
|---|---|---|---|---|---|
| **Base CNN** | **92.59%** | **0.9250** | **0.9259** | **0.9260** | 76.3 |
| GAP + KR | 86.67% | 0.8591 | 0.8667 | 0.8984 | 6.08 |
| VGG + GAP | 85.19% | 0.8514 | 0.8519 | 0.8560 | 74.1 |

### 🏆 โมเดลที่ดีที่สุด (Base CNN)

- **Test Accuracy:** 92.59%
- **Loss:** 0.2133

---

## 🔍 สรุปผลการทดลอง

ผลการทดลองแสดงให้เห็นว่าโมเดล **Base CNN** สามารถจำแนกภาพได้อย่างแม่นยำสูง โดยมี **Test Accuracy ประมาณ 92–93%** และ **Weighted F1-score ประมาณ 0.92–0.93**

- Recall ของคลาสสำคัญ **"No Helmet"** อยู่ที่ **0.84** สะท้อนให้เห็นว่าโมเดลสามารถตรวจจับผู้ไม่สวมหมวกกันน็อคได้ดี แม้ในสภาวะที่ dataset มีขนาดค่อนข้างเล็ก
- การใช้ **Dropout** ช่วยลดปัญหา Overfitting และทำให้โมเดล generalize กับภาพที่ไม่เคยเห็นมาก่อน (unseen data) ได้ดีขึ้น
- โมเดลมีความพร้อมสำหรับการนำไปต่อยอดใช้งานจริงในระบบตรวจสอบความปลอดภัยของผู้ขับขี่บนท้องถนน
- แนวทางการพัฒนาต่อ: เพิ่มปริมาณ dataset เพื่อเสริมความแม่นยำและความหลากหลายของโมเดลในสถานการณ์จริง

---

## 🔗 ข้อมูลเพิ่มเติม

- Presentation / Slide: [ดูรายละเอียดเพิ่มเติม](https://canva.link/so44dtbrik7t5h5)
- Dataset อ้างอิง: [Kaggle Helmet Dataset](https://www.kaggle.com/datasets/abuzarkhaaan/helmet-dataset-cls/data)

---

## 🧰 Skills ที่แสดงในโปรเจกต์นี้

`Data Collection` · `Data Cleaning & Labeling` · `Data Augmentation` · `Image Preprocessing (Resize/Normalize)` · `Model Evaluation & Comparison (Accuracy, F1, Recall, Precision)` · `Deep Learning (CNN)` · `Model Optimization (Dropout, Overfitting Reduction)`
