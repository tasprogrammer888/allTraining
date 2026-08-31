# [M02] การออกแบบวงจรควบคุมด้วยรีเลย์ (Relay Control Logic)

**Relay Control Circuit Design** | Level: Foundation | ระยะเวลา: 32 ชั่วโมง
**Prerequisite:** M00, M01

---

## 1. ภาพรวมและตำแหน่งในหลักสูตร

โมดูลนี้คือ "หัวใจของตรรกะการควบคุม" ที่ทุกอย่างต่อยอดออกไป ผู้เรียนจะเปลี่ยนจากคนที่ "ต่อสายตามแบบ" (M01) มาเป็นคนที่ "ออกแบบ logic เองได้" แนวคิด seal-in, interlock, NO/NC contact, coil และ timer ที่เรียนที่นี่ จะถูกนำไปใช้ซ้ำตลอดในรูป **ladder logic ของ PLC (M06)**, การควบคุมมอเตอร์ (M05) และระบบนิวเมติกส์ (M03)

> หลักคิดสำคัญ: **PLC ก็คือ relay logic ที่ทำด้วยซอฟต์แวร์** ใครเข้าใจ relay logic แน่น จะเขียน ladder ได้เร็วและ debug เป็น

**เชื่อมต่อกับโมดูลอื่นอย่างไร**

| ก่อนหน้า | โมดูลนี้ | ส่งต่อไป |
|---|---|---|
| M00 ความปลอดภัย/มัลติมิเตอร์ | ใช้ LOTO + วัดไฟในวงจรจริง | M05 ใช้ DOL/interlock ต่อยอด Star-Delta/VFD |
| M01 wiring/อ่านแบบ | ต่อยอดเป็นออกแบบ + เขียนแบบเอง | M06 relay logic -> ladder (XIC/XIO/OTE) |
| | | M03/M04 ใช้แนวคิด interlock/sequence |

**ขอบเขตที่ "ไม่" สอนในโมดูลนี้ (กันซ้ำซ้อน):**
- การ build Star-Delta เต็มวงจร และ VFD/soft starter -> เป็นของ **M05** (ที่นี่สอนแค่ concept + demo)
- การเขียนโปรแกรม PLC จริง -> เป็นของ **M06** (ที่นี่แค่ทำสะพาน paper-mapping)

---

## 2. Learning Objectives

เมื่อจบโมดูล ผู้เรียนจะสามารถ:
1. อธิบายโครงสร้าง/หน้าที่ของ contactor, relay, timer, overload, MCB/MCCB และเลขขั้วมาตรฐานได้
2. อ่าน datasheet จริงเพื่อเลือกขนาดและตีความ rating (AC-1/AC-3, coil V, trip class) ได้
3. อ่าน/เขียน ladder & line diagram ตาม IEC พร้อม wire numbering และ cross-reference
4. ทำงานกับวงจรควบคุมอย่างปลอดภัย (test-before-touch, pre-power-on check)
5. ออกแบบ/ต่อ start-stop seal-in, interlock, jogging, reversing, sequential control
6. troubleshoot ด้วยมัลติมิเตอร์แบบเป็นระบบ
7. เชื่อม relay logic เข้ากับ ladder ของ PLC เพื่อปูทาง M06
8. สร้าง DOL starter เต็มตู้ที่ทำงานและปลอดภัยจริง

---

## 3. โครงสร้างเนื้อหา (8 บท / 32 ชม.)

| บท | หัวข้อ | ชม. |
|---|---|---|
| 1 | ความปลอดภัยในงานวงจรควบคุม + อุปกรณ์หัวใจ | 5 |
| 2 | การอ่าน datasheet และเลือกอุปกรณ์ป้องกัน | 4 |
| 3 | การอ่านและเขียน control circuit diagram | 4 |
| 4 | วงจรพื้นฐาน start/stop และ seal-in | 5 |
| 5 | interlock, jogging และ reversing | 4 |
| 6 | sequential control และการออกแบบจากโจทย์ | 4 |
| 7 | การ troubleshoot อย่างปลอดภัย | 3 |
| 8 | Capstone — DOL motor starter เต็มตู้ | 3 |

> หมายเหตุการจัดเวลา: lab แทรกในแต่ละบท ชั่วโมงข้างต้นคือเวลารวมบรรยาย+ปฏิบัติของบทนั้น Lab 0 (safety) ทำในบท 1 และเป็น gate บังคับก่อนทุก lab ที่มีไฟ

---

## 4. เลขขั้วมาตรฐานที่ต้องท่องให้ขึ้นใจ

| ขั้ว | ความหมาย |
|---|---|
| A1 / A2 | ขั้ว coil (จ่ายไฟ control เข้าที่นี่) |
| 1/2, 3/4, 5/6 | main contacts (power, 3 เฟส) |
| 13 / 14 | auxiliary **NO** (เลขลงท้าย 3-4 = NO) |
| 21 / 22 | auxiliary **NC** (เลขลงท้าย 1-2 = NC) |
| 95 / 96 | overload contact **NC** (ใช้ตัด control เมื่อ trip) |
| 97 / 98 | overload contact **NO** (ใช้ส่งสัญญาณ alarm) |

**เคล็ดจำ:** ขั้ว aux ลงท้าย **3-4 = NO**, ลงท้าย **1-2 = NC**

---

## 5. หลักการสำคัญ + ตัวอย่างวงจร

### 5.1 Power circuit vs Control circuit
- **Power circuit**: นำกระแสไปขับโหลด (มอเตอร์), ผ่าน main contacts, ปกติ 3 เฟส 400V
- **Control circuit**: สั่งให้ coil ทำงาน, กระแสต่ำ, ปกติ 24VDC หรือ 230VAC
- แยกกันเพื่อ: ความปลอดภัย, แรงดันคอนโทรลต่ำ, ออกแบบ logic ง่าย

### 5.2 Start/Stop พร้อม Seal-in (Latching)

```
   L (control)
    |
   [Stop NC]            <- สายขาด = หยุด (fail-safe)
    |
    +------[Start NO]---+----[ Coil KM1 (A1/A2) ]----  N/0V
    |                   |
    +--[KM1 aux NO 13/14]+   <- seal-in คร่อมปุ่ม start
```

**ทำไมค้างหลังปล่อยปุ่ม:** กด start -> KM1 ทำงาน -> aux NO (13/14) ปิด -> จ่ายไฟเลี้ยง coil แทนปุ่ม start -> ปล่อยปุ่มยังค้าง กด stop (ตัด NC) -> coil ดับ -> ปลด seal-in

**กฎเหล็ก:** stop และ E-stop **ต้องเป็น NC** เสมอ เพื่อให้ "สายขาด = เครื่องหยุด"

### 5.3 Reversing พร้อม Interlock

```
FWD coil: ...[Stop]--[FWD]--[KM_REV aux NC 21/22]--[KM_FWD]...
REV coil: ...[Stop]--[REV]--[KM_FWD aux NC 21/22]--[KM_REV]...
```
- **Electrical interlock**: NC aux ของอีกตัวอนุกรมใน coil -> ตัวหนึ่งทำงาน อีกตัวดึงไม่ได้
- **Mechanical interlock**: คานกลไกบน contactor กันดึงพร้อมกันแม้สายผิด
- ต้องมี **ทั้งสองชั้น** เพราะ reversing = สลับเฟส ถ้าดึงพร้อมกัน = **ลัดเฟสต่อเฟส** อันตรายร้ายแรง

### 5.4 สะพานสู่ PLC (ปูทาง M06)

| Relay logic (hardware) | Ladder ของ PLC |
|---|---|
| ปุ่ม start (NO) ต่อ input | `XIC` |
| ปุ่ม stop **สายจริงเป็น NC** | `XIC` (เพราะ input ปกติ ON) |
| coil contactor | `OTE` |
| aux NO seal-in | `XIC` ของ output คร่อม start |
| on-delay timer | `TON` |
| NC contact (ตัดการทำงาน) | `XIO` |

> **ประเด็นสำคัญที่ PLC แทนไม่ได้:** **E-stop ต้อง hardwired** ตัด control ทางกายภาพเสมอ ห้ามพึ่ง logic ใน PLC

---

## 6. การเลือกอุปกรณ์ (อ่าน datasheet จริง)

**ลำดับ coordination:** `MCCB/breaker -> Contactor -> Overload -> Motor`

Checklist เลือกอุปกรณ์:
- [ ] Coil voltage ตรงกับ control voltage ที่ใช้ (24VDC? 230VAC?)
- [ ] Contactor utilization category = **AC-3** สำหรับมอเตอร์ (ไม่ใช่ AC-1)
- [ ] AC-3 rating (kW/A) >= ขนาดมอเตอร์ ที่แรงดันใช้งาน
- [ ] Overload ช่วงตั้งกระแสครอบ **FLA** และตั้งให้ตรง FLA
- [ ] Trip class เหมาะกับโหลด (class 10 ทั่วไป, class 20 โหลดสตาร์ทนาน)
- [ ] MCB/MCCB breaking capacity >= ระดับ fault ของจุดติดตั้ง

---

## 7. Hands-on Labs

| Lab | ชื่อ | จุดเน้น |
|---|---|---|
| **0** | Safety drill (gate บังคับ) | test-before-touch, วัดไฟปลอดภัย |
| 1 | รู้จักอุปกรณ์ + อ่าน datasheet | ระบุขั้ว, ตีความ rating |
| 2 | Start/Stop + seal-in | latching, fail-safe stop |
| 3 | Forward/Reverse + interlock | กันลัดเฟส |
| 4 | Sequential ด้วย timer | ออกแบบจากโจทย์ |
| 5 | Troubleshoot (ฝัง fault) | half-split, fault report |
| 6 | แปลง relay -> ladder (paper) | สะพานสู่ M06 |
| **7** | Capstone: DOL starter เต็มตู้ | บูรณาการทั้งหมด |

> รายละเอียดแต่ละ lab ดูใน field `hands_on_labs`

### Pre-Power-On Inspection Checklist (ใช้ก่อนจ่ายไฟทุก lab)

- [ ] ทำ LOTO และพิสูจน์สภาวะไม่มีไฟแล้วระหว่างต่อสาย
- [ ] ทุกขั้วต่อตรงตาม diagram (ไล่ทีละ wire number)
- [ ] ไม่มีสายลัด/แตะกันที่ไม่ตั้งใจ, ปลายสายเข้า ferrule แน่น (pull-test)
- [ ] Coil voltage ที่จะจ่าย ตรงกับสเปก coil
- [ ] Overload ตั้งกระแสตรง FLA
- [ ] ต่อกราวด์ครบ (ตู้, มอเตอร์)
- [ ] stop/E-stop เป็น NC และทดสอบกลไกได้
- [ ] ผู้สอนตรวจและเซ็นก่อนจ่ายไฟ

---

## 8. ข้อผิดพลาดที่พบบ่อย & ความปลอดภัย

ดูรายการเต็มใน field `common_mistakes_safety` — ประเด็นบังคับท่อง:
1. **test-before-touch เสมอ**
2. **stop/E-stop = NC, fail-safe** ไม่มีข้อยกเว้น
3. **reversing ต้องมี interlock 2 ชั้น**
4. **coil voltage ผิด = coil ไหม้ทันที**
5. **continuity ตัดไฟ / วัดแรงดันมีไฟ** อย่าสลับ
6. **E-stop ต้อง hardwired**

---

## 9. การประเมินผล

| ส่วน | น้ำหนัก | รายละเอียด |
|---|---|---|
| ภาคปฏิบัติ (labs + Capstone) | 60% | ทุก lab ต้องผ่าน, Lab 0 เป็น gate, Lab 7 ถ่วงน้ำหนักสูงสุด, ใช้ rubric |
| ออกแบบ/เขียนแบบ | 25% | แปลงโจทย์เป็น diagram, wire numbering, relay->ladder |
| ทฤษฎี/datasheet | 15% | quiz เลือกขนาดและตีความ rating |

**เกณฑ์ผ่าน:** ทุกหมวด >= 70% **และ** ต้องผ่าน safety gate (Lab 0)

---

## 10. ส่งต่อสู่โมดูลถัดไป

- **M05 (Motor Control):** ต่อยอด DOL -> Star-Delta build เต็ม, soft starter, VFD
- **M06 (PLC):** เปลี่ยน relay logic เป็น ladder จริงบน FX5U/GX Works3
- **M03/M04:** ใช้ interlock/sequence กับวาล์วนิวเมติกส์และเซนเซอร์