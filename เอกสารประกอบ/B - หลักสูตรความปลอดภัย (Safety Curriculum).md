# หลักสูตรความปลอดภัย (Safety Curriculum)
## เอกสารแกนกลางความปลอดภัยที่แทรกตลอดทุกโมดูล (M00–M13)

> เอกสารนี้เป็น "เสาหลักความปลอดภัย" ที่ผูกร้อยทุกโมดูลของหลักสูตร ตั้งแต่ผู้เรียนที่ไม่มีพื้นฐาน จนถึง Capstone เครื่อง Pick & Place ครบระบบ
> หลักการสำคัญ: **ความปลอดภัยไม่ใช่บทเรียนหนึ่งครั้ง แต่เป็นวินัยที่ทำซ้ำทุกครั้งก่อนแตะอุปกรณ์** — ทุก Lab ต้องผ่าน Pre-work Safety Check ก่อนเสมอ
> เวอร์ชัน: 1.0 | วันที่: 2026-06-30 | ภาษาหลัก: ไทย (คงศัพท์เทคนิคภาษาอังกฤษ)

---

## สารบัญ
1. [ปรัชญาความปลอดภัยและ Hierarchy of Controls](#1-ปรัชญาความปลอดภัยและ-hierarchy-of-controls)
2. [Safety Competency Matrix — ความปลอดภัยฝังในแต่ละโมดูล](#2-safety-competency-matrix--ความปลอดภัยฝังในแต่ละโมดูล)
3. [อันตรายจากไฟฟ้า (Electrical Hazards: Shock, Arc Flash, Arc Blast)](#3-อันตรายจากไฟฟ้า-electrical-hazards)
4. [LOTO — Lock-Out Tag-Out](#4-loto--lock-out-tag-out)
5. [PPE — อุปกรณ์ป้องกันส่วนบุคคล](#5-ppe--อุปกรณ์ป้องกันส่วนบุคคล)
6. [Grounding / Earthing และระบบป้องกัน](#6-grounding--earthing-และระบบป้องกัน)
7. [ความปลอดภัยลมอัด (Pneumatic Safety)](#7-ความปลอดภัยลมอัด-pneumatic-safety)
8. [ความปลอดภัยเครื่องจักรเคลื่อนไหว (Motor / Servo / Inverter)](#8-ความปลอดภัยเครื่องจักรเคลื่อนไหว-machine-motion-safety)
9. [ความปลอดภัยหุ่นยนต์ KUKA (Robot Safety / ISO 10218)](#9-ความปลอดภัยหุ่นยนต์-kuka-robot-safety)
10. [ความปลอดภัยระบบควบคุมและ Functional Safety (ISO 13849-1 / IEC 60204-1)](#10-ความปลอดภัยระบบควบคุมและ-functional-safety)
11. [มาตรฐานที่เกี่ยวข้อง (Standards Reference)](#11-มาตรฐานที่เกี่ยวข้อง-standards-reference)
12. [Checklist ใช้หน้างานได้จริง (พิมพ์แจก)](#12-checklist-ใช้หน้างานได้จริง)
13. [Incident Response และปฐมพยาบาล](#13-incident-response-และปฐมพยาบาล)

---

## 1. ปรัชญาความปลอดภัยและ Hierarchy of Controls

### 1.1 หลักคิด 3 ข้อที่ต้องท่องได้
1. **Safety First, Always** — ถ้าไม่ปลอดภัย ให้หยุด ไม่ว่างานจะเร่งแค่ไหน "หยุดได้เสมอ" คือสิทธิและหน้าที่ของทุกคน
2. **Assume it is LIVE** — ถือว่าทุกวงจร "มีไฟ/มีแรงดัน/มีลม/มีพลังงานสะสม" จนกว่าจะพิสูจน์ด้วยตนเองว่าไม่มี
3. **Verify, Don't Trust** — อย่าเชื่อป้าย อย่าเชื่อคำบอก อย่าเชื่อสวิตช์ ให้ "วัด/ทดสอบ" ด้วยตัวเองทุกครั้ง

### 1.2 Hierarchy of Controls (ลำดับการควบคุมอันตราย — เลือกจากบนลงล่าง)
| ลำดับ | มาตรการ | ประสิทธิภาพ | ตัวอย่างในหลักสูตร |
|---|---|---|---|
| 1 | **Elimination** (กำจัด) | สูงสุด | ออกแบบให้ไม่ต้องเข้าใกล้จุดอันตราย ลดแรงดัน/ลม ก่อนทำงาน |
| 2 | **Substitution** (ทดแทน) | สูง | ใช้ control voltage 24VDC แทน 230VAC ในวงจรควบคุม |
| 3 | **Engineering Controls** (วิศวกรรม) | ปานกลาง-สูง | safety fence, light curtain, interlock, guard, STO |
| 4 | **Administrative Controls** (บริหารจัดการ) | ปานกลาง | LOTO procedure, work permit, checklist, training |
| 5 | **PPE** (อุปกรณ์ป้องกันส่วนบุคคล) | ต่ำสุด (ด่านสุดท้าย) | ถุงมือฉนวน, แว่นนิรภัย, arc-rated clothing |

> **กฎทอง:** PPE คือด่านสุดท้าย ไม่ใช่ด่านแรก ห้ามใช้ PPE มาทดแทน LOTO หรือ engineering control

---

## 2. Safety Competency Matrix — ความปลอดภัยฝังในแต่ละโมดูล

| Module | หัวข้อความปลอดภัยที่เป็น Gate ก่อนเริ่ม Lab | Checklist ที่ต้องใช้ |
|---|---|---|
| **M00** พื้นฐานไฟฟ้า+ความปลอดภัย | ทุกหัวข้อ (โมดูลนี้คือฐานราก): shock/arc, LOTO, PPE, live-dead-live, grounding, CPR/AED | A, B, C, J |
| **M01** Wiring & Panel | LOTO ก่อนต่อสาย, dead test, torque spec, first energization | A, B, D, H |
| **M02** Relay Control | แยก power/control circuit, pre-power-on, วัดวงจรมีไฟอย่างปลอดภัย | A, B, D, H |
| **M03** Pneumatics | depressurize-to-zero, hose whip prevention, lockout แหล่งลม, stored energy | A, E, H |
| **M04** Sensors & Actuators | bench test ก่อนต่อ PLC, flyback spike, 24VDC fusing | A, B |
| **M05** Motor Control (DOL/Y-Δ/VFD) | LOTO, megger test, inrush, residual DC bus voltage ใน VFD | A, B, D, F, H |
| **M06** PLC Programming | fail-safe wiring, E-stop ต้อง hardwired, "PLC ≠ safety device" | A, B, G |
| **M07** HMI / GOT | **HMI ไม่ใช่อุปกรณ์ safety** — E-stop/interlock ต้อง hardwired | A, G |
| **M08** Servo & Inverter ขั้นสูง | STO (CN8), residual DC bus, shielding/grounding, pre-power-on | A, B, D, F, H |
| **M09** KUKA Robot | T1/T2/AUT modes, enabling switch, E-stop, fencing, safe speed 250mm/s, ISO 10218 | A, I, G |
| **M10** Communication | RS-485 wiring ปลอดภัย, ตรวจสายก่อนจ่ายไฟ, แยก power/signal | A, B |
| **M11** Advanced Programming | machine-level interlock, safety handshake, fail-safe state | A, G, I |
| **M12** Capstone Pick & Place | บูรณาการทั้งหมด: PLr selection, section-by-section power-up, ครูเซ็นอนุมัติ | A–J ทั้งหมด |
| **M13** Industry 4.0 / SCADA | OT cybersecurity, network segmentation, ความปลอดภัยขณะวัดวงจรมีไฟ | A, B, G |

---

## 3. อันตรายจากไฟฟ้า (Electrical Hazards)

### 3.1 กลไกอันตราย 4 อย่าง
| อันตราย | คืออะไร | ผลต่อร่างกาย/ทรัพย์สิน |
|---|---|---|
| **Electric Shock** | กระแสไหลผ่านร่างกาย | กล้ามเนื้อเกร็ง, หยุดหายใจ, ventricular fibrillation, เสียชีวิต |
| **Arc Flash** | ความร้อน/แสงจาก arc (อุณหภูมิสูงถึง ~19,000 °C) | ผิวไหม้รุนแรง, ตาบอด, ไฟไหม้เสื้อผ้า |
| **Arc Blast** | คลื่นแรงดัน + โลหะหลอมเหลวกระเด็น | แก้วหูฉีก, กระดูกหัก, สะเก็ดโลหะ |
| **ไฟไหม้ (Fire)** | overload, short circuit, จุดต่อหลวมร้อน | เพลิงไหม้, ควันพิษ |

### 3.2 เกณฑ์กระแสไฟฟ้าต่อร่างกาย (AC 50/60 Hz ผ่านร่างกาย — ใช้จำเชิงลำดับ)
| กระแส (mA) | ผลต่อร่างกาย |
|---|---|
| ~0.5–2 mA | เริ่มรู้สึก (perception threshold) |
| ~10–16 mA | **let-go threshold** — เกินนี้ปล่อยมือเองไม่ได้ กล้ามเนื้อเกร็งค้าง |
| ~30–50 mA | หายใจลำบาก, อันตรายถ้านาน |
| ~50–100 mA ขึ้นไป | **ventricular fibrillation** หัวใจห้องล่างสั่นพลิ้ว — เสี่ยงเสียชีวิต |
| > ~1–2 A | หัวใจหยุด, ไหม้เนื้อเยื่อ |

> **เหตุผลที่ RCD/ELCB ตั้ง 30 mA:** ตัดก่อนถึงระดับที่ทำให้ปล่อยมือไม่ได้และก่อน fibrillation

### 3.3 สูตรที่ต้องคำนวณได้
- **Ohm's Law:** V = I × R
- **Power:** P = V × I (และ P = I²R, P = V²/R)
- **Energy:** kWh = (kW × ชั่วโมง)
- **3-phase:** V(line-line) = √3 × V(line-neutral) ; P(3φ) = √3 × V(LL) × I × cos φ

### 3.4 ✅ Checklist อันตรายไฟฟ้าก่อนทำงาน (ดูฉบับพิมพ์ในหมวด 12 — Checklist A/B)
- [ ] ระบุระดับแรงดันของงาน (ELV ≤50VAC, LV ≤1000VAC) และความเสี่ยง arc flash
- [ ] ประเมินว่า "งานนี้ต้องมีไฟอยู่หรือไม่" — ถ้าไม่จำเป็น → LOTO ก่อนเสมอ
- [ ] กำหนดขอบเขตทำงาน (work boundary) และกันบุคคลที่ไม่เกี่ยวข้องออก

---

## 4. LOTO — Lock-Out Tag-Out

### 4.1 ขั้นตอน LOTO 6+1 ขั้น (ท่องและทำตามลำดับเสมอ)
1. **Prepare / Notify** — แจ้งผู้เกี่ยวข้อง ระบุแหล่งพลังงานทุกชนิด (ไฟฟ้า, ลม, hydraulic, ความร้อน, สปริง, แรงโน้มถ่วง)
2. **Shut Down** — ปิดเครื่องตามขั้นตอนปกติ
3. **Isolate** — ตัดแยกแหล่งพลังงานทุกตัว (เปิดเบรกเกอร์/วาล์วลม/ถอด disconnect)
4. **Lock & Tag** — ใส่ padlock + hasp + tag ชื่อผู้ทำงาน (ใช้ hasp รองรับหลายคน → แต่ละคนล็อกของตัวเอง)
5. **Release Stored Energy** — คายพลังงานสะสม: ปล่อยลมให้เป็น 0, รอ DC bus capacitor คายประจุ (servo/VFD ≥ 5–15 นาที ตาม manual), ลดสปริง/น้ำหนักค้าง
6. **Verify Zero Energy (live-dead-live)** — พิสูจน์ไร้พลังงานด้วยตนเอง (ดู 4.2)
7. **(เมื่อจบงาน) Restore** — ตรวจคน/เครื่องมือออกจากพื้นที่ → ถอด lock เจ้าของล็อกเท่านั้น → แจ้ง → จ่ายไฟ

### 4.2 หลัก live-dead-live (สำคัญที่สุดของการ verify)
```
1. LIVE  → วัดเครื่องมือกับแหล่งที่ "รู้ว่ามีไฟ"   → ยืนยันมิเตอร์ทำงานปกติ
2. DEAD  → วัดที่จุดงาน                          → ต้องอ่านได้ 0 V (ทุกเฟส, เฟส-กราวด์, เฟส-นิวทรัล)
3. LIVE  → วัดกับแหล่งที่รู้ว่ามีไฟอีกครั้ง        → ยืนยันมิเตอร์ยังทำงาน (ไม่ได้พังระหว่างวัด)
```
> ถ้าข้าม step LIVE ตัวที่ 3 แล้วมิเตอร์เสียระหว่างทาง คุณจะ "อ่าน 0 V จากมิเตอร์ที่พัง" = อันตรายถึงชีวิต

### 4.3 ✅ Checklist LOTO หน้างาน
- [ ] ระบุแหล่งพลังงานครบทุกชนิด (ไม่ใช่แค่ไฟฟ้า)
- [ ] แต่ละคนที่ทำงานมี padlock + tag ของตัวเอง (one person, one lock, one key)
- [ ] กุญแจอยู่กับเจ้าของล็อกเท่านั้น ห้ามฝากกุญแจ
- [ ] คายพลังงานสะสมครบ (ลม = 0 บนเกจ, DC bus คายประจุครบเวลา)
- [ ] ทำ live-dead-live ครบ 3 step
- [ ] ก่อน restore: ตรวจคน/มือ/เครื่องมือออกหมด, guard ใส่กลับครบ

---

## 5. PPE — อุปกรณ์ป้องกันส่วนบุคคล

### 5.1 PPE ตามประเภทงาน
| งาน | PPE ขั้นต่ำ |
|---|---|
| งานไฟฟ้าทั่วไป (dead) | แว่นนิรภัย, รองเท้า safety, ถุงมือทำงาน |
| งานไฟฟ้ามีไฟ (live) | ถุงมือฉนวน (insulated glove) + leather protector, แว่น/face shield, เสื้อ arc-rated, ผ้ายางปูพื้น |
| งานลมอัด | แว่นนิรภัย (กันสะเก็ด/ลมพุ่ง), ป้องกันหู ถ้าเสียงดัง |
| งานเครื่องจักร/หุ่นยนต์ | รองเท้า safety, แว่น, ไม่สวมของหลวม/สร้อย/ผมยาวปล่อย |
| งาน crimp/ตัดสาย | แว่นนิรภัย, ถุงมือ |

### 5.2 ระดับถุงมือฉนวน (IEC 60903)
| Class | Max use voltage (AC) |
|---|---|
| Class 00 | 500 V |
| Class 0 | 1,000 V |
| Class 1 | 7,500 V |

### 5.3 ✅ Checklist PPE ก่อนใช้
- [ ] เลือก PPE ตรงระดับแรงดัน/ประเภทงาน
- [ ] ถุงมือฉนวน: ทำ **air test** (ม้วนดูรอยรั่ว) + visual test ทุกครั้งก่อนใช้
- [ ] ถุงมือฉนวนสวมคู่กับ leather protector เสมอ (กันบาด/ทะลุ)
- [ ] แว่น/face shield ไม่มีรอยร้าว, arc-rated clothing ไม่มีรูขาด
- [ ] ตรวจวันหมดอายุ/รอบทดสอบของ PPE ไฟฟ้า

---

## 6. Grounding / Earthing และระบบป้องกัน

### 6.1 หน้าที่ของ Grounding
- นำกระแสรั่ว/ฟอลต์ลงดินอย่างปลอดภัย → ให้อุปกรณ์ป้องกัน (CB/RCD) ตัดวงจร
- ลดแรงดันสัมผัส (touch voltage) บนตัวถังโลหะ
- อ้างอิงศักย์ไฟฟ้า (signal/shield ground สำหรับ servo/communication เพื่อลด noise)

### 6.2 อุปกรณ์ป้องกันหลัก
| อุปกรณ์ | ป้องกันอะไร |
|---|---|
| **MCB** (curve B/C) | overload + short circuit |
| **MCCB** | เหมือน MCB แต่กระแสสูงกว่า ปรับค่าได้ |
| **RCD/ELCB 30 mA** | กระแสรั่วลงดิน → ป้องกัน shock ต่อคน |
| **Fuse (gG/aR)** | short circuit (aR สำหรับ semiconductor/VFD) |
| **Thermal Overload Relay** | overload มอเตอร์ (ขั้ว 95/96/97/98) |
| **Surge/flyback diode, snubber** | spike จาก inductive load |

### 6.3 ✅ Checklist Grounding
- [ ] ตัวถังตู้/เครื่องจักร/มอเตอร์ ต่อ PE bar ครบ
- [ ] PE conductor (เขียว-เหลือง) ขนาดถูกต้อง, ขันแน่นตาม torque
- [ ] วัดความต่อเนื่อง (continuity) ระหว่างจุดโลหะกับ PE bar
- [ ] shield ของสาย servo/communication ต่อ ground จุดเดียว (กัน ground loop)
- [ ] RCD/ELCB ทดสอบปุ่ม TEST ผ่าน

---

## 7. ความปลอดภัยลมอัด (Pneumatic Safety)

### 7.1 อันตรายเฉพาะของลมอัด
- **Stored energy:** กระบอกสูบ/ถังลมยังมีแรงดันค้างแม้ปิดแหล่งจ่าย → cylinder อาจพุ่งเอง
- **Hose whip:** สายลมหลุดแล้วสะบัดด้วยแรงดันสูง → ฟาดหน้า/ตา
- **Air injection:** ลมอัดเข้าผิวหนัง/ตา = อันตรายร้ายแรง (ห้ามเป่าลมใส่ตัว)
- **Pinch point:** กระบอกสูบหนีบนิ้ว

### 7.2 ✅ Checklist ความปลอดภัยลมก่อนถอด/ต่อ
- [ ] ปิด shut-off valve แหล่งลม + ล็อก (lockout)
- [ ] **Depressurize to zero** — ปล่อยลมจนเกจอ่าน 0 bar (ยืนยันด้วยเกจ ไม่ใช่เดา)
- [ ] ตรวจ stored energy ในกระบอก/accumulator คายหมด
- [ ] จับ/ยึดปลายสายลมก่อนปลด (กัน hose whip), ใช้ push-in fitting ถูกวิธี
- [ ] ตรวจ cylinder ไม่มีน้ำหนัก/สปริงค้างที่จะดีดเมื่อปลดลม
- [ ] meter-out สำหรับงานยกของ (ควบคุมความเร็วทิศทางโหลด ป้องกัน load ตก/พุ่ง)
- [ ] ไม่เป่าลมอัดใส่ร่างกาย/เสื้อผ้า

---

## 8. ความปลอดภัยเครื่องจักรเคลื่อนไหว (Machine Motion Safety)

### 8.1 Motor / DOL / Star-Delta
- **Auto-restart hazard:** หลัง overload trip หรือไฟดับ-มา มอเตอร์ต้องไม่สตาร์ตเอง → ออกแบบ seal-in/latching ให้ต้องกด start ใหม่
- **Phase-to-phase short:** วงจร forward/reverse ต้องมีทั้ง **electrical interlock + mechanical interlock**
- **Y/Δ ผิด:** มอเตอร์ 400/690V บนระบบ 400V ห้ามใช้ star-delta (overvoltage บนขดลวด)

### 8.2 Inverter (VFD) / Servo
- **Residual DC bus voltage:** หลังตัดไฟ DC bus capacitor ยังมีแรงดันอันตราย → **รอตามเวลาใน manual (เช่น 5–15 นาที)** ก่อนแตะขั้ว
- **STO (Safe Torque Off):** ฟังก์ชัน safety ตัด torque โดยไม่ต้องตัด main power — ต่อ CN8 (servo) ให้ถูกต้อง, ห้ามถอด short connector ทิ้งโดยไม่เข้าใจ
- **Unexpected motion:** servo/positioning อาจเคลื่อนเร็ว/แรง → ตั้ง soft limit + hardware limit switch + near-point dog, กันมือออกจากแนวการเคลื่อนที่
- แยกเดินสาย power/signal กัน noise ที่อาจทำให้ทำงานผิดพลาด

### 8.3 ✅ Checklist Motion Safety
- [ ] วงจรไม่ auto-restart หลังไฟดับ-มา (มี seal-in + ต้องกด start ใหม่)
- [ ] forward/reverse มี electrical + mechanical interlock
- [ ] รอ DC bus คายประจุครบเวลาก่อนแตะ servo/inverter
- [ ] hardware limit switch + soft limit ตั้งครบก่อน enable axis
- [ ] ทดสอบการเคลื่อนที่ครั้งแรกด้วย override ต่ำ/ความเร็วต่ำ และมือพร้อมที่ E-stop
- [ ] กันบุคคลออกจากแนวการเคลื่อนที่ของ axis/slide

---

## 9. ความปลอดภัยหุ่นยนต์ KUKA (Robot Safety / ISO 10218)

### 9.1 Operating Modes (KUKA)
| Mode | ความเร็ว / เงื่อนไข | การใช้งาน |
|---|---|---|
| **T1** (Manual Reduced) | **TCP จำกัด ≤ 250 mm/s** | teach/jog ปลอดภัยที่สุด — เข้าใกล้หุ่นได้ภายใต้ enabling switch |
| **T2** (Manual High) | ความเร็วเต็ม, ไม่จำกัด | ทดสอบโปรแกรมความเร็วจริง (ผู้ชำนาญเท่านั้น) |
| **AUT** | อัตโนมัติ | รันโปรแกรม ต้องอยู่หลัง fence/guard |
| **AUT EXT** | อัตโนมัติ สั่งจาก PLC ภายนอก | production จริง |

### 9.2 อุปกรณ์ Safety ของหุ่นยนต์
- **E-stop:** บน smartPAD + external (ต้อง hardwired เข้า safety circuit)
- **Enabling switch 3 ตำแหน่ง:** ปล่อย=หยุด, กดกลาง=เคลื่อนได้, กดสุด(panic)=หยุด
- **Safety fence / light curtain / safety mat:** กันคนเข้าโซนขณะ AUT
- **Safe Operational Stop / Safe Zone monitoring:** จำกัด workspace
- **Mastering/Referencing:** ต้อง master ก่อนเคลื่อนแม่นยำ (master หายเมื่อ battery loss/เปลี่ยนมอเตอร์)

### 9.3 ✅ Checklist Robot Safety (ก่อนเข้า cell / ก่อนรัน)
- [ ] รู้ตำแหน่ง E-stop ทุกตัว (smartPAD + external) และทดสอบแล้ว
- [ ] เลือก mode ให้ถูกงาน — เข้าใกล้หุ่น = **T1 เท่านั้น** (≤250 mm/s)
- [ ] enabling switch ทำงาน 3 ตำแหน่งถูกต้อง
- [ ] fence/light curtain ปิดครบ ก่อนสลับเข้า AUT/AUT EXT
- [ ] ไม่มีคนอื่นอยู่ใน robot envelope ก่อนเคลื่อน
- [ ] master/reference ครบ, ตรวจ payload/tool ตรงกับโปรแกรม
- [ ] ทดสอบโปรแกรมใหม่ครั้งแรกที่ override ต่ำ (POV ต่ำ) ใน T1 ก่อน
- [ ] handshake interlock กับ PLC ถูกต้อง (robot/PLC ไม่สั่งเคลื่อนชนกัน)

---

## 10. ความปลอดภัยระบบควบคุมและ Functional Safety

### 10.1 หลักการสำคัญ (สอนซ้ำตั้งแต่ M06–M12)
- **PLC ไม่ใช่ safety device** และ **HMI ไม่ใช่ safety device** → E-stop, safety interlock, light curtain ต้อง **hardwired ผ่าน safety relay / safety PLC** เท่านั้น
- **Fail-safe principle:** ออกแบบให้เมื่อสายขาด/อุปกรณ์เสีย ระบบเข้าสู่สถานะปลอดภัย (เช่น E-stop และ limit ใช้หน้าสัมผัส **NC + positive opening**)
- **De-energize to stop:** สัญญาณ stop/safety ควรทำงานแบบตัดไฟเพื่อหยุด

### 10.2 Functional Safety Workflow (ใช้ใน Capstone M12)
1. Risk Assessment → ระบุอันตรายแต่ละจุด
2. กำหนด **Required Performance Level (PLr)** ตาม ISO 13849-1 (จาก S/F/P: Severity, Frequency, Possibility of avoidance)
3. เลือก safety component (safety relay, light curtain, safety category) ให้ PL ที่ได้ ≥ PLr
4. Validate ด้วยการทดสอบ (functional test ทุก safety function)

### 10.3 ✅ Checklist Control Safety
- [ ] E-stop/interlock/light curtain เป็น hardwired ผ่าน safety relay (ไม่ผ่าน PLC/HMI logic)
- [ ] E-stop และ limit switch ใช้ NC + positive opening
- [ ] กำหนด PLr แต่ละ safety function และเลือก component ให้ PL ≥ PLr
- [ ] ทดสอบ safety function ทุกตัวจริง (กด E-stop แล้วทุก actuator หยุด, ตัด light curtain แล้วหยุด)
- [ ] reset ต้องเป็น manual deliberate action (ไม่ auto-reset)

---

## 11. มาตรฐานที่เกี่ยวข้อง (Standards Reference)

| มาตรฐาน | ขอบเขต | ใช้ในโมดูล |
|---|---|---|
| **IEC 60204-1** | Safety of machinery — Electrical equipment of machines | M01, M02, M05, M11, M12 |
| **ISO 13849-1** | Safety-related parts of control systems (PL/PLr) | M11, M12 |
| **ISO 12100** | Safety of machinery — Risk assessment & risk reduction (กรอบใหญ่) | M11, M12 |
| **ISO 10218-1/-2** | Robots & robotic devices — Safety (industrial robot + integration) | M09, M12 |
| **ISO/TS 15066** | Collaborative robots safety (อ้างอิงเสริม) | M09 |
| **IEC 61131-3** | PLC programming languages (LD/FBD/ST/SFC) | M06, M11 |
| **IEC 60617** | สัญลักษณ์ในแผนภาพไฟฟ้า | M01, M02 |
| **ISO 1219** | สัญลักษณ์ระบบ fluid power (pneumatic diagram) | M03 |
| **IEC 60364 / มาตรฐานติดตั้งไฟฟ้า** | การติดตั้งไฟฟ้าในอาคาร/grounding | M00, M01, M06 |
| **IEC 60903 / IEC 60900** | ถุงมือฉนวน / insulated tools | M00–M12 |
| **NFPA 70E (อ้างอิง)** | Electrical safety in the workplace (arc flash) | M00 |
| **กฎหมายไทย** | กฎกระทรวง/มาตรฐาน จป. ด้านความปลอดภัยไฟฟ้าและเครื่องจักร, มอก. ที่เกี่ยวข้อง | ทุกโมดูล |
| **มาตรฐานฝีมือแรงงานแห่งชาติ (DSD)** | สาขา "ช่างควบคุมด้วยระบบโปรแกรมเมเบิ้ลลอจิกคอนโทรลเลอร์ (PLC)" กรมพัฒนาฝีมือแรงงาน — ครอบคลุมความปลอดภัยไฟฟ้าพื้นฐาน + wiring + PLC ระดับ 1 | M00–M02, M06 |

> หมายเหตุ: ใช้ฉบับล่าสุดที่บังคับใช้เสมอ และอ้างอิงคู่กับกฎหมายความปลอดภัยฯ ของไทยและนโยบายโรงงาน
> **การใช้มาตรฐาน DSD:** ผู้เรียนที่ผ่าน L1–L3 (เอกสาร E) ควรมีความรู้/ทักษะเพียงพอสำหรับเข้าสอบมาตรฐานฝีมือแรงงานแห่งชาติ สาขาช่างควบคุม PLC ระดับ 1 ของกรมพัฒนาฝีมือแรงงานได้โดยตรง — เป็นวุฒิที่ตลาดแรงงานไทยรู้จักและใช้เทียบคู่กับใบรับรองภายในของหลักสูตร (ดูเอกสาร E §5)

---

## 12. Checklist ใช้หน้างานได้จริง (พิมพ์แจก)

> ใช้รูปแบบ "ติ๊ก + เซ็นชื่อ + เวลา" ทุก Lab ที่มีพลังงาน ครูเซ็นอนุมัติก่อนจ่ายไฟครั้งแรก

### Checklist A — Pre-Work Universal Safety (ใช้ทุกโมดูล)
- [ ] เข้าใจขอบเขตงานและอันตรายของงานวันนี้ (toolbox talk)
- [ ] ระบุแหล่งพลังงานทุกชนิด (ไฟฟ้า/ลม/แรงกล/ความร้อน)
- [ ] สวม PPE ตรงประเภทงาน และตรวจสภาพแล้ว
- [ ] พื้นที่ทำงานสะอาด ไม่มีน้ำ/สิ่งกีดขวาง รู้ตำแหน่ง E-stop และทางหนีไฟ
- [ ] มีเพื่อนร่วมงาน/ผู้ดูแลรับทราบ (ไม่ทำงานไฟฟ้าคนเดียวในงานเสี่ยง)

**ผู้ปฏิบัติ:____________ เวลา:______ ครูอนุมัติ:____________**

### Checklist B — Electrical Dead-Work / LOTO + Live-Dead-Live
- [ ] LOTO ครบ 6 ขั้น (isolate → lock+tag → release stored energy)
- [ ] live-dead-live: ทดสอบมิเตอร์กับแหล่งมีไฟ → วัดจุดงาน = 0V → ทดสอบมิเตอร์อีกครั้ง
- [ ] วัดครบทุกคู่: เฟส-เฟส, เฟส-นิวทรัล, เฟส-กราวด์
- [ ] (servo/VFD) รอ DC bus คายประจุครบเวลาตาม manual

### Checklist C — Electrical Theory/Bench (M00)
- [ ] แหล่งจ่าย DC ปรับค่า ตั้งถูกต้อง, fuse ติดตั้ง
- [ ] ไม่ลัดวงจร bench supply, ต่อขั้วถูกขั้ว

### Checklist D — Panel Wiring & First Energization (M01/M02/M05/M08)
- [ ] ขันขั้วตาม torque spec, pull test หางปลา/ferrule ผ่าน
- [ ] ตรวจ wire numbering/tag ตรงแบบ, ไม่มีสายหลวม/สายเปลือยโผล่
- [ ] dead test: continuity + ไม่มี short ระหว่างเฟส/กับกราวด์
- [ ] (ถ้ามีมอเตอร์) megger test ผ่านเกณฑ์
- [ ] guard/ฝาตู้พร้อม, แจ้งคนรอบข้างก่อนจ่ายไฟ
- [ ] first energization: จ่ายทีละ section, สังเกตเสียง/กลิ่น/ความร้อนผิดปกติ

### Checklist E — Pneumatic (M03)
- [ ] ปิด+ล็อกแหล่งลม, depressurize จนเกจ = 0 bar
- [ ] ยึดปลายสายก่อนปลด (กัน hose whip), fitting แน่น
- [ ] meter-out สำหรับงานยก, ตรวจ pinch point ก่อนจ่ายลม

### Checklist F — Servo / Inverter Power-Up (M05/M08)
- [ ] pre-power-on: ตรวจ L1/L2/L3, U/V/W, encoder(CN2), STO(CN8) ถูกต้อง
- [ ] shielding/grounding ครบ, แยก power/signal
- [ ] limit switch + soft limit ตั้งแล้ว, ทดสอบเคลื่อนที่ override ต่ำ มือพร้อม E-stop

### Checklist G — Control/PLC/HMI Safety (M06/M07/M11/M13)
- [ ] E-stop/interlock/light curtain เป็น hardwired ผ่าน safety relay (ไม่ผ่าน PLC/HMI)
- [ ] fail-safe: E-stop/limit เป็น NC + positive opening
- [ ] ทดสอบ E-stop จริง: กดแล้ว actuator ทุกตัวหยุด
- [ ] ไม่ auto-restart หลังไฟดับ-มา / หลัง reset

### Checklist H — Measurement Safety
- [ ] มิเตอร์ CAT rating เหมาะกับงาน, สายวัดสภาพดีไม่มีรอยแตก
- [ ] เลือกย่านวัดถูกต้องก่อนแตะจุดงาน, จับที่ probe หลัง guard

### Checklist I — Robot Cell (M09/M11/M12)
- [ ] mode = T1 (≤250 mm/s) เมื่อเข้าใกล้หุ่น
- [ ] enabling switch ทำงาน 3 ตำแหน่ง, E-stop ทุกตัวพร้อม
- [ ] fence/light curtain ปิดครบก่อน AUT, ไม่มีคนใน envelope
- [ ] master/reference ครบ, override ต่ำเมื่อรันโปรแกรมใหม่

### Checklist J — Emergency Readiness
- [ ] รู้ตำแหน่ง main breaker, ถังดับเพลิง (CO2/ผงเคมีสำหรับไฟฟ้า), ชุดปฐมพยาบาล, AED
- [ ] รู้เบอร์ฉุกเฉินและขั้นตอนแจ้งเหตุ

---

## 13. Incident Response และปฐมพยาบาล

### 13.1 ขั้นตอนเมื่อมีคนถูกไฟดูด
1. **อย่าแตะตัวผู้บาดเจ็บโดยตรง** ถ้ายังมีไฟ
2. **ตัดไฟทันที** (main breaker / disconnect) หรือใช้วัสดุฉนวนแยกผู้ป่วยออกจากแหล่งไฟ
3. แจ้งขอความช่วยเหลือ / โทรฉุกเฉิน
4. ตรวจการหายใจ-ชีพจร → **ทำ CPR ถ้าจำเป็น** → ใช้ **AED**
5. ดูแลแผลไหม้ และส่งโรงพยาบาลทุกกรณี (อันตรายภายในอาจไม่แสดงทันที)

### 13.2 ✅ Checklist ความพร้อมตอบสนองเหตุ
- [ ] ผู้สอน/ผู้เรียนรู้ตำแหน่ง main breaker และวิธีตัดไฟฉุกเฉิน
- [ ] มี CPR manikin/AED trainer สำหรับฝึก (M00) และมี AED จริงในพื้นที่
- [ ] ถังดับเพลิงเหมาะกับไฟฟ้า (CO2 / ผงเคมีแห้ง) อยู่ในระยะเข้าถึง
- [ ] มีแผนแจ้งเหตุและเบอร์ฉุกเฉินติดประกาศชัดเจน
- [ ] บันทึก incident/near-miss ทุกครั้งเพื่อปรับปรุง (continuous improvement)

---

> **สรุปวินัยความปลอดภัย 1 บรรทัด:** *Assume it's live → LOTO → Release stored energy → Live-Dead-Live → ทดสอบ safety function → จ่ายไฟทีละ section → ครูเซ็นอนุมัติ*
>
> เอกสารนี้ต้องถูกอ้างอิงและติ๊ก checklist จริงในทุก Lab ที่มีพลังงาน ไม่ใช่อ่านครั้งเดียวแล้วเก็บ