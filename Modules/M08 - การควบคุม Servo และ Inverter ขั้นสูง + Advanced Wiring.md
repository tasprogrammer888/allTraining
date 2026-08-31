# [M08] การควบคุม Servo และ Inverter ขั้นสูง + Advanced Wiring
### Advanced Servo & Inverter Control + Advanced Wiring

| | |
|---|---|
| **Level** | Intermediate |
| **ระยะเวลา** | 40 ชั่วโมง |
| **Prerequisite** | M06 (PLC FX5U), M05 (Motor/VFD), M04 (Sensor), M01/M02 (Wiring), M00 (Safety) |
| **ต่อยอดไปยัง** | M09 (KUKA), M10 (Communication), M11 (Integration), M12 (Capstone) |

---

## 1. ภาพรวมและจุดยืนของโมดูล

โมดูลนี้คือ "สะพาน" ระหว่างการควบคุมมอเตอร์แบบความเร็ว (M05) กับการควบคุมตำแหน่งที่แม่นยำสำหรับงานอัตโนมัติจริง ผู้เรียนจะเปลี่ยนจาก "สั่งมอเตอร์ให้หมุน" เป็น "สั่งแกนให้ไปหยุดตรงตำแหน่งที่ต้องการแบบ closed-loop" ซึ่งเป็นหัวใจของเครื่องจักรอัตโนมัติทุกชนิด

> **หลักการสอนของโมดูลนี้:** ทุก Lab ต้อง "จ่ายไฟแล้วทำงานจริง" ไม่ใช่แค่ทฤษฎี และทุกครั้งที่ต่อ power ต้องผ่าน *Pre-Power-On Check* + ครูเซ็นรับรองก่อน

### สิ่งที่ปรับปรุงจากร่างเดิม (สำหรับผู้ออกแบบหลักสูตร)
- **เพิ่มบทที่ 2 ใหม่ (Positioning Math):** ร่างเดิมพูดถึง electronic gear แต่ไม่มีชั่วโมงฝึกคำนวณจริง ทำให้ผู้เรียน "ตั้งค่าได้แต่ไม่เข้าใจ" — เป็นช่องว่างที่ทำให้ Lab positioning ล้มเหลวบ่อยที่สุดหน้างาน
- **เพิ่ม Pre-Power-On Check ด้วย multimeter:** ร่างเดิมมีแค่ "ครูตรวจก่อนจ่ายไฟ" ซึ่งคลุมเครือ — เปลี่ยนเป็น checklist วัดจริง (continuity U/V/W, short L-PE)
- **เพิ่ม Lab 9 Troubleshooting Drill แบบ instructor-seeded faults:** ร่างเดิมสอน troubleshooting เป็นทฤษฎีในบทท้าย แต่ไม่มี lab ฝึกหาของพังจริง — ซึ่งเป็นทักษะที่หน้างานต้องการมากที่สุด
- **เพิ่มเรื่องอันตราย DC bus residual voltage และ STO short connector:** สองจุดนี้คือสาเหตุอุบัติเหตุ/อุปกรณ์พัง/งงเปิดไม่ติด อันดับต้นๆ ของมือใหม่
- **ปรับ Lab 7 inverter ให้ลงมือ Modbus จริง:** เชื่อมต่อตรงกับ M10 ไม่ให้เป็นแค่ทฤษฎี
- **ขยายเป็น 8 บท** เพื่อกระจายชั่วโมงให้ positioning (บทที่ 5) และ troubleshooting (บทที่ 8) ได้เวลาเพียงพอ

---

## 2. Learning Objectives

เมื่อจบโมดูล ผู้เรียนจะสามารถ:
1. เลือกใช้ **servo / inverter / stepper** ให้เหมาะกับโจทย์งานจริง และอธิบาย closed-loop vs open-loop ได้
2. **คำนวณ electronic gear (CMX/CDV)** แปลง pulse ↔ ระยะจริง (mm) จากกลไก ballscrew/pulley/gearbox ได้
3. **ต่อสาย MELSERVO** (main circuit, encoder, CN1, STO) + shielding/grounding ถูกต้องและปลอดภัย
4. ทำ **Pre-Power-On Check** ด้วย multimeter ก่อนจ่ายไฟทุกครั้ง
5. ตั้งค่า + **tuning servo** ด้วย MR Configurator2 ให้แกนนิ่ง ไม่สั่น ไม่ overshoot
6. เขียน **PLC FX5U positioning** (JOG / Homing DSZR / DRVA / DRVI) พร้อมจัดการ limit
7. ทำ **point table multi-position sequence**
8. ควบคุม **inverter ขั้นสูง** (multi-speed / PID / Modbus RTU)
9. **Troubleshoot** alarm ของ servo/inverter อย่างเป็นระบบ หา root cause ได้
10. บูรณาการ **HMI → PLC → Servo** ครบวงจร

---

## 3. โครงสร้างเนื้อหา (8 บท / 40 ชม.)

| บท | หัวข้อ | ชม. |
|---|---|---|
| 1 | ระบบ Servo, Closed-Loop และการเลือก Servo/Inverter/Stepper | 4 |
| 2 | คณิตศาสตร์ Positioning — Electronic Gear & Resolution | 3 |
| 3 | Advanced Wiring MELSERVO + Pre-Power-On Check | 5 |
| 4 | Parameter Setup & Tuning (MR Configurator2) | 5 |
| 5 | Positioning Control จาก FX5U (JOG/Homing/DRVA/DRVI) | 7 |
| 6 | Point Table & Multi-Position Sequence | 4 |
| 7 | Inverter ขั้นสูง (Multi-speed / PID / Modbus) | 5 |
| 8 | บูรณาการ Servo + HMI + Troubleshooting | 7 |
| | **รวม** | **40** |

---

## 4. รายละเอียดบทเรียน (เลือกจุดสำคัญ)

### บทที่ 1 — ทำไมต้อง Servo
ตารางเลือกใช้ที่ใช้สอนได้จริง:

| โจทย์งาน | เลือกใช้ | เหตุผล |
|---|---|---|
| Pick & place หยุดตรงตำแหน่ง ±0.05mm | **Servo** | closed-loop, dynamic, แม่น |
| Conveyor / pump / fan ปรับความเร็ว | **Inverter + มอเตอร์** | ไม่ต้องการตำแหน่ง ถูกกว่า |
| งานเบา ราคาประหยัด ไม่มีโหลดกระชาก | **Stepper** | open-loop ถูก แต่ลื่น (lose step) ได้ |

### บทที่ 2 — Electronic Gear (หัวใจที่มือใหม่พลาด)
สูตรพื้นฐาน:

```
CMX (Pr.PA06)     Encoder resolution (pulse/rev)
---------- =  --------------------------------------------------
CDV (Pr.PA07)   จำนวน command pulse ที่ต้องการต่อ 1 รอบมอเตอร์
```

**ตัวอย่าง:** ballscrew lead = 10 mm/rev, ต้องการ 1 command pulse = 1 micron (0.001 mm)
- ต้องการ pulse ต่อรอบ = 10 mm ÷ 0.001 mm = **10,000 pulse/rev**
- encoder MR-JE = 131072 pulse/rev
- ⇒ CMX/CDV = 131072 / 10000 → ลดทอนเป็นค่าจำนวนเต็ม

> ตรวจสอบเสมอ: สั่ง 10,000 pulse แล้วแกนต้องเลื่อน 10 mm พอดี — ถ้าไม่ตรง = electronic gear ผิด

### บทที่ 3 — Wiring & Pre-Power-On Check

**ตัวอย่าง terminal หลัก MR-JE-A (1-phase 200V):**

```
[L1][L2]  ── จากเบรกเกอร์ (power)
[P][C]    ── regenerative resistor (ถ้าใช้)
[U][V][W] ── ไป servo motor  (ห้ามสลับเฟส!)
[PE]      ── ground
CN1       ── control I/O (SON, EM2, ALM, LSP/LSN, INP, RD ...)
CN2       ── encoder (shield!)
CN8       ── STO (ต้องเสียบ short connector ถ้าไม่ใช้)
```

**Pre-Power-On Checklist (วัดด้วย multimeter ก่อนจ่ายไฟ):**

- [ ] LOTO ติดตั้ง + ยืนยันไฟดับ + รอ DC bus discharge ครบเวลาตามคู่มือ
- [ ] Continuity U(amp)→U(motor), V→V, W→W ครบ ไม่สลับ
- [ ] วัด L1/L2 → PE = ไม่ short (ค่าความต้านทานสูง/OL)
- [ ] CN8 มี short connector (กรณีไม่ใช้ STO)
- [ ] PE ต่อแน่น, shield encoder ลง ground ปลายเดียว
- [ ] สาย encoder/signal ไม่เดินคู่กับสาย power
- [ ] **ครูเซ็นรับรอง** ก่อนจ่ายไฟ ✍️

**EMC & Noise-Aware Layout (ทำไม servo/inverter ตำแหน่งเพี้ยน/สื่อสารหลุดโดยไม่มี fault ทางไฟฟ้า):**
- **แยกโซนในตู้:** จัดกลุ่มอุปกรณ์กำลัง (inverter, contactor, transformer) แยกจากกลุ่ม signal/analog (PLC, module analog, terminal สาย encoder) — เว้นระยะหรือกั้นด้วยแผ่นโลหะต่อกราวด์ (partition)
- **แยกเส้นทางเดินสาย:** สาย power (U/V/W, สายมอเตอร์) กับสาย signal (encoder, analog, comm) ต้องเดินคนละราง/ท่อ ถ้าจำเป็นต้องตัดผ่านให้ตัดตั้งฉาก 90° เท่านั้น (ห้ามเดินขนานแนบกัน)
- **Shield ต่อกราวด์ปลายเดียว** (ทำที่ฝั่ง drive/controller) กันเกิด ground loop — ย้ำจากที่กล่าวไว้ข้างบน
- **Ferrite core / EMI filter** ที่สายเข้า inverter ลดสัญญาณรบกวนย้อนเข้าระบบไฟ
- **อาการเมื่อ Layout/EMC ผิด:** ตำแหน่ง servo เพี้ยนแบบสุ่ม (ไม่ repeat), analog input กระตุก/ค่าสั่น, Modbus/comm timeout เป็นพัก ๆ โดยหา hardware fault ไม่เจอ — เป็นกับดักที่มือใหม่วินิจฉัยผิดว่าเป็นปัญหาโปรแกรม
- เชื่อมกับ **M10** (comm timeout อาจมีสาเหตุจาก EMC ไม่ใช่แค่ config ผิด) และ **M12** (ตรวจ layout ตู้ Capstone ก่อน commission)

### บทที่ 5 — ตัวอย่าง Ladder (FX5U positioning)

```
// JOG forward ขณะกดปุ่ม X0
|--[ X0 ]--[/ M_LimitP ]----------[ JOG+  axis1 ]--|

// Homing เมื่อกด X2 (ใช้ DSZR กับ near-point dog X10 + zero signal)
|--[ X2 ]------------------------[ DSZR axis1 ... ]--|

// Absolute move ไปตำแหน่งใน D100 (จาก HMI) เมื่อกด Start X3
|--[ X3 ]--[/ Busy ]--------------[ DRVA  D100 (speed) axis1 ]--|

// Incremental move +10mm
|--[ X4 ]--[/ Busy ]--------------[ DRVI  +10000 (speed) axis1 ]--|
```

> ลำดับความปลอดภัย: **hardware limit (LSP/LSN) > software stroke limit > โปรแกรม** — limit switch จริงต้องหยุดแกนได้แม้โปรแกรมพัง

### บทที่ 7 — Inverter: จุดที่ติดบ่อยที่สุดคือ Pr.79

| Pr.79 | ความหมาย | ใช้เมื่อ |
|---|---|---|
| 0 | external/PU สลับได้ | ทั่วไป |
| 1 | PU เท่านั้น | สั่งจากหน้าจอ inverter |
| 2 | external เท่านั้น | สั่งจาก terminal |
| 3/4 | combined | ความเร็วจาก analog + start จาก terminal |
| 6/(NET) | communication | **สั่งจาก PLC ผ่าน Modbus** |

> เมื่อ "สั่งไม่ได้" ให้เช็ค **Pr.79 เป็นอันดับแรก**

**Fault code ที่ต้องจำ:** `E.OC` overcurrent · `E.OV` overvoltage · `E.THM/E.THT` thermal · `E.LF` output phase loss · `E.GF` ground fault

---

## 5. Hands-On Labs (9 Labs)

| Lab | ชื่อ | Deliverable หลัก |
|---|---|---|
| 1 | Model code + datasheet + label ขั้วต่อ | ใบงาน model code + ภาพ label |
| 2 | คำนวณ electronic gear 3 เคส | worksheet + ค่า CMX/CDV |
| 3 | ต่อสาย servo + Pre-Power-On Check | ระบบไม่มี alarm + checklist ครูเซ็น |
| 4 | Setup + Tuning (MR Configurator2) | param file + JOG นิ่งไม่สั่น |
| 5 | JOG + Homing จาก FX5U | homing สำเร็จ + limit หยุดได้ |
| 6 | Absolute/Incremental + Point Table | วิ่งตรงตำแหน่ง + sequence อัตโนมัติ |
| 7 | Inverter Multi-speed + Modbus | 3 speed + สั่ง/อ่านผ่าน Modbus |
| 8 | บูรณาการ HMI→PLC→Servo | ระบบครบวงจรสาธิตสด |
| 9 | **Troubleshooting Drill (seeded faults)** | ใบบันทึก root cause ทุกเคส |

### Lab 9 — Instructor-Seeded Faults (ทักษะหน้างานจริง)
ครูแอบใส่ความผิดพลาดลงในระบบที่ทำงานได้แล้ว ผู้เรียนต้องหาด้วย framework: **อาการ → สมมติฐาน → จุดวัด/monitor → root cause → แก้**

| # | Fault ที่ใส่ | อาการที่ผู้เรียนจะเจอ |
|---|---|---|
| 1 | สลับเฟส U-V | มอเตอร์ run-away / oscillation / alarm |
| 2 | encoder CN2 หลวม | servo alarm / ไม่มี feedback |
| 3 | electronic gear ผิด | ระยะไม่ถึง/เลย ทั้งที่โปรแกรมถูก |
| 4 | Pr.79 inverter ผิด mode | สั่งจาก PLC ไม่ได้ |
| 5 | limit switch ค้าง | servo ไม่ยอมเดินทิศนั้น |
| 6 | STO short connector หลุด | servo-on ไม่ได้ |

---

## 6. ⚠️ Safety & Common Mistakes

| ความเสี่ยง / ข้อผิดพลาด | ผลที่ตามมา | การป้องกัน |
|---|---|---|
| แตะ DC bus (P/N) หลังตัดไฟทันที | ไฟดูด (residual voltage) | LOTO + รอ discharge + วัดยืนยัน |
| ต่อ U/V/W สลับเฟส | run-away / alarm | เช็ค continuity ก่อนจ่ายไฟ |
| ลืม STO short connector | servo-on ไม่ได้ | ตรวจ CN8 ทุกครั้ง |
| encoder เดินคู่สาย power | noise / ตำแหน่งเพี้ยน | แยก duct + ground shield |
| ใช้ relay output ทำ pulse | ความถี่ไม่พอ/relay พัง | ใช้ transistor output |
| พึ่ง E-stop บนจอ HMI | ไม่ safety-rated | ต้องมี hardwired E-stop + STO |
| จ่ายไฟไม่ทำ pre-check | amplifier พัง | checklist + ครูเซ็น |
| เดินสาย power/signal ขนานแนบกัน | ตำแหน่งเพี้ยน/analog กระตุก/comm timeout แบบหา fault ไม่เจอ | แยกราง, ตัดผ่านตั้งฉาก 90°, shield ปลายเดียว, ferrite core |

---

## 7. การประเมินผล (Competency-Based)

| ส่วน | สัดส่วน | เกณฑ์ |
|---|---|---|
| Practical demonstration (Lab 5/6/8) | 50% | สั่ง servo จาก PLC+HMI ไปตำแหน่งแม่นยำ ครบลูป |
| Wiring & Safety (Lab 3) | 20% | ผ่าน pre-power-on checklist (ครูเซ็น) |
| Troubleshooting (Lab 9) | 20% | หา root cause ≥ 4/6 เคส พร้อมเหตุผล |
| Knowledge / Calc (Lab 2 + quiz) | 10% | electronic gear ถูก + quiz ผ่าน |

**เกณฑ์ผ่าน:** ทุกส่วน ≥ 60% และ practical ต้องสาธิตผ่านจริง

---

## 8. ความต่อเนื่องกับโมดูลอื่น (ไม่ซ้ำ ไม่มีช่องว่าง)

- **รับจาก M05:** หลักการ inverter/VFD พื้นฐาน → M08 ต่อยอดเป็น advanced (multi-speed/PID/Modbus) ไม่สอนซ้ำพื้นฐาน 3-phase
- **รับจาก M06:** ladder + index addressing → M08 ใช้ทำ positioning + point table
- **รับจาก M07:** การเขียน HMI พื้นฐาน → M08 บทที่ 8 ใช้ HMI สั่ง servo (ไม่สอนการสร้างจอใหม่ตั้งแต่ต้น)
- **ส่งต่อ M09 (KUKA):** servo/conveyor ที่คุมได้จะเชื่อมเข้าหุ่นยนต์
- **ส่งต่อ M10 (Communication):** Modbus ที่เริ่มใน Lab 7 → ต่อยอด CC-Link/Ethernet
- **ส่งต่อ M11/M12:** positioning หลายแกน + troubleshooting framework คือพื้นฐานที่ Capstone ถือว่า "ทำได้แล้ว"

---

## 9. อุปกรณ์และซอฟต์แวร์ที่ต้องเตรียม

**ฮาร์ดแวร์:** MR-JE-A/MR-J4-A + HG motor, ballscrew slide 1 แกน + dog + limit ×2, FX5U (transistor output), GOT, FR-D700/E700 + มอเตอร์ 3-phase, RS-485 converter, regen resistor + short connector สำรอง

**ซอฟต์แวร์:** GX Works3, GT Designer3, MR Configurator2

**เครื่องมือ/Safety:** multimeter, crimp/ferrule tool, LOTO kit, insulated tools, dial gauge/ไม้บรรทัดเหล็ก, คู่มือ/datasheet จริงทุกตัว
