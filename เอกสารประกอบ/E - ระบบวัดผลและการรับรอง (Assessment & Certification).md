# ระบบวัดผลและเส้นทางการรับรอง (Assessment & Certification Path)
## หลักสูตรช่างไฟฟ้า–ระบบอัตโนมัติอุตสาหกรรม (PLC / HMI / Servo / Robot / Communication)

> เอกสารนี้ออกแบบให้สอดคล้องกับ 14 โมดูล (M00–M13) และ Capstone หลัก "เครื่อง Pick & Place"
> ปรัชญาการวัดผล: **เน้น competency-based และ skill-based** — ผ่านได้ต้อง "ทำงานจริงได้อย่างปลอดภัย" ไม่ใช่แค่ "ตอบข้อสอบถูก"

---

## สารบัญ
1. [หลักการและน้ำหนักการวัดผล](#1-หลักการและน้ำหนักการวัดผล)
2. [เกณฑ์ผ่านแต่ละโมดูล (ทฤษฎี + ปฏิบัติ)](#2-เกณฑ์ผ่านแต่ละโมดูล-ทฤษฎี--ปฏิบัติ)
3. [Safety Gate — ประตูบังคับที่ตัดเกรดทันที](#3-safety-gate--ประตูบังคับที่ตัดเกรดทันที)
4. [Competency Checklist แบบ Skill-Based (รายโมดูล)](#4-competency-checklist-แบบ-skill-based-รายโมดูล)
5. [ระดับใบรับรอง (Foundation / Operator / Technician / Specialist)](#5-ระดับใบรับรอง-foundation--operator--technician--specialist)
6. [การ Map กับงานจริง (Job Description)](#6-การ-map-กับงานจริง-job-description)
7. [ตัวอย่างข้อสอบปฏิบัติ (Practical Exams)](#7-ตัวอย่างข้อสอบปฏิบัติ-practical-exams)
8. [แบบฟอร์มและเครื่องมือประเมิน](#8-แบบฟอร์มและเครื่องมือประเมิน)

---

## 1. หลักการและน้ำหนักการวัดผล

### 1.1 องค์ประกอบคะแนนมาตรฐานของทุกโมดูล

| องค์ประกอบ | น้ำหนัก | รูปแบบ | เกณฑ์ผ่านขั้นต่ำ |
|---|---|---|---|
| **ทฤษฎี (Theory)** | 30% | ข้อเขียน/ออนไลน์, อ่าน datasheet, คำนวณ | ≥ 70% |
| **ปฏิบัติ (Practical / Hands-on)** | 50% | ทำงานจริงบนชุดฝึก ประเมินด้วย rubric | ≥ 75% |
| **Safety & Workmanship** | 20% | สังเกตพฤติกรรมตลอดโมดูล + checklist | ≥ 80% |
| **Safety Gate (บังคับผ่าน)** | Pass/Fail | ข้อปฏิบัติด้านความปลอดภัยที่ห้ามพลาด | ต้อง Pass 100% |

> **กฎเหล็ก:** หาก Safety Gate = Fail → ผลรวมทั้งโมดูล = ไม่ผ่าน (ไม่ว่าคะแนนอื่นจะสูงเท่าใด)
> ต้องเข้ารับการ remediation และสอบ Safety Gate ใหม่ก่อนทำส่วนอื่นต่อ

### 1.2 ระดับการประเมินทักษะ (Skill Rating Scale)

ใช้สเกล 0–4 ในทุก competency checklist:

| ระดับ | ชื่อ | ความหมาย |
|:---:|---|---|
| **0** | ทำไม่ได้ | ทำไม่ได้/ทำผิดจนเกิดอันตราย |
| **1** | ต้องช่วยเต็มที่ | ทำได้เมื่อมีครูชี้นำทุกขั้นตอน |
| **2** | ต้องช่วยบางส่วน | ทำได้แต่ต้องเตือน/แก้ไขเป็นระยะ |
| **3** | ทำได้เอง (เกณฑ์ผ่าน) | ทำได้ด้วยตนเองถูกต้อง ปลอดภัย ตามเวลา |
| **4** | สอนคนอื่นได้ | ทำได้คล่อง อธิบาย/แก้ปัญหาเองได้ เป็น mentor |

> **เกณฑ์ผ่าน competency:** ทุกข้อต้องได้ ≥ 3 และข้อที่ติด `[Critical]` ต้องได้ 3–4 เท่านั้น

### 1.3 เกณฑ์เกรดและการตัดสิน

| เกรด | ช่วงคะแนนรวม | ความหมาย |
|:---:|:---:|---|
| **PASS-D (Distinction)** | ≥ 90% | ผ่านดีเยี่ยม มีศักยภาพเป็น mentor |
| **PASS-M (Merit)** | 80–89% | ผ่านระดับดี ทำงานจริงได้มั่นใจ |
| **PASS (Competent)** | เกณฑ์ขั้นต่ำ–79% | ผ่าน ทำงานได้ภายใต้การกำกับเบื้องต้น |
| **REFER (ซ่อม)** | ต่ำกว่าเกณฑ์ ≤ 1 ส่วน | ซ่อมเฉพาะส่วน + สอบใหม่ภายใน 2 สัปดาห์ |
| **FAIL** | ต่ำกว่าเกณฑ์หลายส่วน / Safety Fail | เรียนซ้ำทั้งโมดูล |

### 1.4 Placement Test ก่อนเข้าเรียน (Skills Test / Test-out)

> เป้าหมาย: คัดแยกผู้เรียนตามพื้นฐานจริงตั้งแต่วันแรก ป้องกันทั้ง "มือใหม่ถูกจับไปเรียนเร็วเกินจนตามไม่ทัน" และ "คนมีพื้นฐานต้องนั่งเรียนซ้ำสิ่งที่ทำเป็นแล้ว" — ใช้ผลสอบตัดสินจุดเริ่มเรียน ไม่ใช่แค่ถามปากเปล่า (ต่อยอดจาก pre-assessment ในเอกสาร D §7.1 ให้เป็นเครื่องมือมาตรฐานที่ให้คะแนนได้)

**โครงสร้างข้อสอบ (คะแนนเต็ม 600, ใช้เวลา ~8 ชั่วโมง/1 วัน):**

| ส่วน | สัดส่วนคะแนน | เนื้อหา | อ้างอิงโมดูล |
|---|:---:|---|---|
| ทฤษฎีไฟฟ้าพื้นฐาน | 150 | Ohm's law, power, AC/DC, 1φ/3φ, อ่าน nameplate | M00 |
| อ่านแบบไฟฟ้า | 100 | สัญลักษณ์ IEC 60617, trace schematic, wire numbering | M01 |
| ปฏิบัติ: ต่อวงจร Star-Delta | 250 | ย้ำหางปลา (pull test) + ต่อวงจร DOL/Star-Delta ทำงานจริงภายในเวลาที่กำหนด | M01, M02, M05 |
| Safety Gate (LOTO + live-dead-live) | 100 | Pass/Fail — คะแนนอื่นไม่นับถ้าข้อนี้ Fail | M00 |

**เกณฑ์การจัดระดับตามคะแนน:**

| คะแนนรวม | ผลการจัดระดับ |
|---|---|
| < 300 หรือ Safety Gate Fail | เริ่มเรียนที่ **M00** ตามปกติ (ไม่มีพื้นฐานเพียงพอให้ข้าม) |
| 300–449 | เริ่มที่ **M00** แต่ได้ fast-track (ลดชั่วโมงลงมือซ้ำ ~30% ใน M00–M01) |
| 450–549 | **Test-out M00–M01** ได้ → เริ่มเรียนจริงที่ **M02** |
| ≥ 550 | **Test-out M00–M02** ได้ → เริ่มเรียนจริงที่ **M03/M04/M05** (ตรง Persona B ในภาพรวมหลักสูตร §2.2) |

> **กฎเหล็ก:** ไม่ว่าคะแนนสูงแค่ไหน ผู้เรียนที่ Safety Gate = Fail **ต้องเริ่มที่ M00** เสมอ — ทักษะปลอดภัยข้ามไม่ได้ไม่ว่าจะมีประสบการณ์มากี่ปี
> ผู้เรียนที่ไม่ผ่านเกณฑ์ขั้นต่ำสามารถสมัครสอบ Placement Test ซ้ำได้หลังเว้นระยะ ≥ 2 สัปดาห์

---

## 2. เกณฑ์ผ่านแต่ละโมดูล (ทฤษฎี + ปฏิบัติ)

ตารางนี้แสดง "ภาระงานวัดผลหลัก (key assessment)" และ "เกณฑ์ผ่านเฉพาะ (must-pass criteria)" ของแต่ละโมดูล

| ID | โมดูล (ชม.) | ทฤษฎี (30%) | ปฏิบัติหลัก (50%) | เกณฑ์ Critical ที่ต้องผ่าน |
|:---:|---|---|---|---|
| **M00** | ความปลอดภัย & พื้นฐานไฟฟ้า (24) | ทดสอบเกณฑ์กระแส mA, Ohm/Power, √3, color code | สาธิต **LOTO ครบสเต็ป** + **live-dead-live** | ทำ LOTO + verify zero energy ถูก 100% |
| **M01** | Wiring & Panel & อ่านแบบ (32) | อ่าน schematic/IEC 60617, เลือกขนาดสาย+CB | ประกอบตู้ DOL ย้ำหางปลา/ferrule + **pull test ผ่าน** + torque | ย้ำผ่าน pull test, first power-up ปลอดภัย |
| **M02** | Relay Control Logic (32) | อ่าน datasheet contactor/OLR, เลขขั้ว A1/A2/95-98 | ต่อวงจร **start/stop + seal-in + reversing interlock** ทำงานจริง | interlock กัน short ทำงาน 100% |
| **M03** | Pneumatics (28) | อ่าน ISO 1219, meter-in vs meter-out | ต่อวงจร electro-pneumatic + ปรับความเร็ว + **depressurize ก่อนถอด** | depressurize/verify ก่อนถอดสายลมทุกครั้ง |
| **M04** | Sensors & Actuators (24) | อ่าน datasheet, NPN vs PNP, sink/source | พิสูจน์ชนิด sensor ด้วย DMM + ต่อ 3-wire เข้า FX5U **bench test ก่อน** | ตั้ง S/S ถูก, ต่อ sensor ติดและอ่านค่าได้ |
| **M05** | Motor Control DOL/Y-Δ/VFD (32) | อ่าน nameplate, decision Y/Δ, coordination type | ต่อ **Star-Delta** + Forward/Reverse + set inverter FR | เลือก Y/Δ ถูกตามแรงดัน, interlock ครบ |
| **M06** | Basic PLC (FX5U/GX Works3) (40) | scan cycle, device memory X/Y/M/D/T/C, IEC 61131-3 | เขียน ladder + Timer/Counter + **download จริง + online monitor** | โปรแกรมทำงานตาม spec, fail-safe wiring |
| **M07** | HMI (GOT/GT Designer3) (28) | บทบาท HMI, **HMI ไม่ใช่ safety** | ตั้ง comm GOT–FX5U ติด + สร้างหน้าจอ map device | E-stop ยัง hardwired, comm ติดจริง |
| **M08** | Servo & Inverter ขั้นสูง (40) | electronic gear CMX/CDV, คำนวณ pulse/ระยะ | wiring MELSERVO (CN1/CN2/CN8) + tuning + positioning จาก PLC | **pre-power-on check ผ่านก่อนจ่ายไฟ**, ตำแหน่งแม่นตาม spec |
| **M09** | KUKA Robot (KRL/smartPAD) (40) | datasheet, modes T1/T2/AUT, coordinate systems | mastering + jog + teach point + เขียน KRL + handshake PLC | ทำงานใน T1 (250 mm/s), ใช้ enabling switch ถูก |
| **M10** | Communication (Modbus/Eth/CC-Link) (32) | IP/subnet, Modbus register map, 4xxxx→protocol | RS-485 + termination 120Ω + **FX5U เป็น Modbus master คุม FR** | สื่อสารอ่าน/เขียนได้จริง, จัดการ timeout |
| **M11** | Advanced Programming (32) | state machine, SFC vs step register | เขียน step sequence + **FB modular** + recipe + alarm | sequence/home ทำงานปลอดภัย, FB reuse ได้ |
| **M12** | **Capstone Pick & Place** (40) | system architecture, SOO, safety concept (ISO 13849) | **ประกอบตู้ทั้งระบบ + megger test + โปรแกรมทั้งระบบทำงาน** | first power-up ทีละ section + ครูเซ็นอนุมัติ |
| **M13** | Industry 4.0 / SCADA / IIoT (Elective) (32) | OT vs IT, automation pyramid, OPC UA | SCADA tag + trend + alarm เชื่อม FX5U + dashboard | (Elective — ไม่บังคับเพื่อ core cert) |

### 2.1 เงื่อนไขผ่านโมดูล (ครบทุกข้อ)
- [ ] คะแนนทฤษฎี ≥ 70%
- [ ] คะแนนปฏิบัติ ≥ 75%
- [ ] Safety & Workmanship ≥ 80%
- [ ] **Safety Gate = Pass**
- [ ] Competency checklist ทุกข้อ ≥ 3 (ข้อ `[Critical]` ≥ 3)
- [ ] เข้าเรียน ≥ 80% ของชั่วโมงโมดูล (สำหรับงานปฏิบัติที่พลาดไม่ได้)

---

## 3. Safety Gate — ประตูบังคับที่ตัดเกรดทันที

ทุกโมดูลที่ทำงานกับไฟ/ลม/หุ่นยนต์ มี **Safety Gate** ที่ผู้เรียนต้องผ่าน **100%** สังเกตได้จากพฤติกรรมจริงในแล็บ

### Safety Gate รายกลุ่มงาน

| กลุ่มงาน | โมดูลที่บังคับ | พฤติกรรมที่ต้องเห็น (ห้ามพลาดแม้ครั้งเดียว) |
|---|---|---|
| **งานไฟฟ้า** | M00–M02, M05, M06, M08, M12 | LOTO ครบสเต็ป → live-dead-live → สวม PPE/ตรวจถุงมือฉนวน → ไม่แตะตัวนำมีไฟโดยไม่วัดก่อน |
| **งานลมอัด** | M03 | depressurize ให้เป็น 0 + ยืนยันด้วยเกจ + จับสายกัน hose whip ก่อนถอด |
| **งานหุ่นยนต์** | M09, M12 | อยู่นอก workspace ตอน AUT, ใช้ T1 ตอน teach, มือบน enabling switch, รู้ตำแหน่ง E-stop |
| **งานสื่อสาร/ตู้มีไฟ** | M10, M12 | ตรวจสายด้วย DMM ก่อนจ่ายไฟ, แยก power/signal, ไม่ถอดขั้วขณะมีไฟ |

> **บันทึก:** ใช้ใบ `SAFETY-GATE-OBSERVATION` (ดูหมวด 8) ครูเซ็นกำกับทุกครั้งที่จ่ายไฟ/ลม/รันหุ่นยนต์ครั้งแรก

---

## 4. Competency Checklist แบบ Skill-Based (รายโมดูล)

> ให้คะแนนแต่ละข้อตามสเกล 0–4 (หมวด 1.2) — ข้อ `[Critical]` ต้องได้ ≥ 3 จึงผ่านโมดูล

### M00 — ความปลอดภัย & พื้นฐานไฟฟ้า
- [ ] `[Critical]` ทำ LOTO ครบทุกสเต็ป (แจ้ง → ปิด → ล็อก → tag → verify) ด้วยตนเอง
- [ ] `[Critical]` พิสูจน์ไร้พลังงานด้วย live-dead-live ถูกต้อง
- [ ] เลือก/ตรวจสภาพ PPE และถุงมือฉนวน (air/visual test) ก่อนใช้
- [ ] คำนวณ V=IR, P=VI, kWh จากโจทย์โรงงานได้
- [ ] แยก AC/DC, 1-phase/3-phase, อธิบาย ×√3 และ star/delta เบื้องต้น
- [ ] วัดด้วย DMM/clamp/megger เลือกย่านและ CAT rating ถูกต้อง

### M01 — Wiring, Panel & อ่านแบบ
- [ ] เลือกชนิด/ขนาดสายและสีให้สอดคล้องพิกัด CB
- [ ] `[Critical]` ย้ำหางปลา/ครอบ ferrule ผ่าน **pull test**
- [ ] ขันขั้วด้วย torque ตาม spec
- [ ] อ่านสัญลักษณ์ IEC 60617 + trace วงจรในแบบ
- [ ] ทำ wire numbering / cross-reference ครบ
- [ ] `[Critical]` ประกอบตู้ + first energization ปลอดภัยตาม checklist

### M02 — Relay Control Logic
- [ ] อธิบายหน้าที่/เลขขั้ว contactor, relay, timer, OLR (A1/A2, 13/14, 21/22, 95-98)
- [ ] อ่าน datasheet เลือก AC-3, coil voltage, ช่วงตั้ง overload
- [ ] อ่าน/เขียน ladder line diagram + cross-reference
- [ ] `[Critical]` ต่อ start/stop + seal-in ทำงานจริง
- [ ] `[Critical]` ต่อ reversing + electrical & mechanical interlock กัน short
- [ ] troubleshoot วงจรควบคุมอย่างปลอดภัย (วัดแบบ live ถูกวิธี)

### M03 — Pneumatics
- [ ] อธิบาย/ไล่เส้นทางลมจากปั๊มถึง actuator + exhaust
- [ ] `[Critical]` depressurize + verify ด้วยเกจ ก่อนถอด/ต่อทุกครั้ง
- [ ] อ่าน ISO 1219, ระบุชนิดวาล์ว 3/2, 5/2, port P/A/B/R
- [ ] ต่อวงจร single/double-acting cylinder ทำงานจริง
- [ ] เลือก meter-in vs meter-out ถูกตามทิศโหลด (งานยก = meter-out)
- [ ] ต่อ electro-pneumatic + solenoid + sensor feedback

### M04 — Sensors & Actuators
- [ ] เลือกชนิด sensor ตามวัสดุ/ระยะ/สภาพหน้างาน
- [ ] อ่าน datasheet: output type, sensing distance, IP, response time, สีสาย
- [ ] `[Critical]` แยก NPN/PNP + พิสูจน์ชนิดด้วย DMM
- [ ] `[Critical]` ต่อ 3-wire เข้า FX5U ตั้ง S/S sink/source ถูก
- [ ] bench test sensor (LED/output) ก่อนต่อเข้า PLC
- [ ] ขับ output load + ใส่ flyback diode/snubber ถูกต้อง

### M05 — Motor Control (DOL / Star-Delta / VFD)
- [ ] อ่าน nameplate ครบทุกช่อง + อธิบาย torque-speed/slip
- [ ] `[Critical]` ตัดสินใจ Y/Δ ถูกตามแรงดันระบบ + ต่อ link terminal box ถูก
- [ ] เลือกขนาด MCCB/contactor/OLR/fuse ตาม FLC + coordination
- [ ] `[Critical]` ต่อ DOL power+control + pre-energization checklist
- [ ] ต่อ Forward/Reverse + interlock ครบ
- [ ] ต่อ Star-Delta + set/run inverter FR ด้วย PU และ analog

### M06 — Basic PLC (FX5U / GX Works3)
- [ ] อธิบาย scan cycle + ประเมิน scan time + ปัญหา double-coil
- [ ] ใช้ device memory X/Y (octal)/M/L/D/T/ST/C/LC/SM/SD ถูก
- [ ] `[Critical]` ใช้ GX Works3: project → parameter → IP → convert → write/verify → online monitor
- [ ] เขียน contact/coil/latch/edge, Timer/Counter, Compare/MOV/arithmetic
- [ ] `[Critical]` ต่อ I/O (sink/source) + pre-power-on check ด้วย DMM
- [ ] แปลง relay logic → ladder มี documentation

### M07 — HMI (GOT / GT Designer3)
- [ ] `[Critical]` ระบุชัดว่า **HMI ไม่ใช่ safety** — E-stop/interlock ต้อง hardwired
- [ ] ทำตาราง device mapping (X/Y/M/D + GB/GD)
- [ ] เดินไฟ GOT (24VDC, FG) + เลือกสาย Ethernet ถูก
- [ ] `[Critical]` ตั้ง comm 2 ฝั่ง (FX5U + GOT) จนติดจริง
- [ ] สร้าง switch/lamp/numerical/text + screen switching map device
- [ ] ทำ alarm + lamp animation สื่อสถานะ

### M08 — Servo & Inverter ขั้นสูง
- [ ] อธิบาย closed-loop vs open-loop + เลือก servo/inverter/stepper
- [ ] `[Critical]` คำนวณ electronic gear (CMX/CDV) + pulse↔ระยะจริง (ballscrew/gear)
- [ ] อ่าน manual MELSERVO + ต่อ CN1/CN2/CN8 + shielding/grounding
- [ ] `[Critical]` pre-power-on check ด้วย DMM ก่อนจ่ายไฟ servo
- [ ] tuning ด้วย MR Configurator2 + positioning จาก FX5U (JOG/Homing/DRVA/DRVI)
- [ ] wiring + set inverter FR multi-speed/analog แยกสาย power/signal

### M09 — KUKA Robot
- [ ] อ่าน datasheet (payload/reach/repeatability)
- [ ] `[Critical]` อธิบาย/ใช้ modes T1/T2/AUT/AUT EXT + enabling switch 3 ตำแหน่ง
- [ ] `[Critical]` mastering/referencing แกน + อธิบายเมื่อ master หาย
- [ ] jog axis + Cartesian (World/Base/Tool) + จัดการ override
- [ ] teach point + motion types + เขียน KRL เบื้องต้น
- [ ] คุม gripper + handshake I/O กับ PLC

### M10 — Communication
- [ ] แยก serial (232/422/485) vs Ethernet + เลือกใช้
- [ ] `[Critical]` กำหนด IP/subnet/gateway ให้อยู่ subnet เดียว + วินิจฉัย ping fail
- [ ] ต่อ RS-485 daisy-chain (A/B/SG) + termination 120Ω + ตรวจสายด้วย DMM
- [ ] อ่าน register map Modbus + แปลง 4xxxx → protocol address
- [ ] `[Critical]` FX5U เป็น Modbus RTU master คุม FR (run/stop/freq/อ่านความเร็ว) + จัดการ timeout
- [ ] Modbus TCP / CC-Link IE Field เบื้องต้น

### M11 — Advanced Programming
- [ ] อธิบาย state machine + ปัญหา spaghetti logic
- [ ] `[Critical]` ออกแบบ sequence chart/state transition + I/O list ก่อนเขียน
- [ ] เขียน step control ทั้ง step register(D)+compare และ SFC + home/reset ปลอดภัย
- [ ] `[Critical]` สร้าง/instance FB ด้วย label (modular) ทีมอ่านโค้ดได้
- [ ] data handling: R/file register, index Z, scaling analog, recipe + validate
- [ ] error handling + alarm management

### M12 — Capstone Pick & Place
- [ ] `[Critical]` ส่งมอบเอกสาร: I/O list, network topology, SOO, safety concept (PLr/ISO 13849)
- [ ] อ่าน/แก้ schematic, GA, pneumatic, network ให้ตรง as-built
- [ ] เลือกสาย/breaker/fuse จาก datasheet servo/inverter/safety relay
- [ ] `[Critical]` ต่อตู้ทั้งระบบ + grounding/shielding + ผ่าน continuity/megger test
- [ ] `[Critical]` pre-power-on + first power-up ทีละ section + ครูเซ็นอนุมัติ
- [ ] integrate PLC+HMI+Servo+Robot+Comm ทำงานครบ cycle + troubleshoot

---

## 5. ระดับใบรับรอง (Foundation / Operator / Technician / Specialist)

ใบรับรองเป็นระบบ **สะสมและต่อยอด (stackable)** — ผ่านครบโมดูลในระดับใด ได้ใบรับรองระดับนั้น

### 5.1 โครงสร้างระดับใบรับรอง

| ระดับ | ชื่อใบรับรอง | โมดูลบังคับ | Capstone/สอบรวบยอด | ความสามารถสรุป |
|:---:|---|---|---|---|
| **L1** | **Foundation Certificate**<br>(พื้นฐานไฟฟ้า–ความปลอดภัย) | M00, M01, M02 | สอบปฏิบัติ DOL panel | เดินสาย ประกอบตู้พื้นฐาน อ่านแบบ ทำงานปลอดภัย LOTO |
| **L2** | **Operator / Automation Wiring Certificate** | + M03, M04, M05 | สอบปฏิบัติ electro-pneumatic + motor control | ต่อระบบลม/sensor/มอเตอร์/inverter ดูแลเครื่องระดับ operator |
| **L3** | **Technician Certificate**<br>(PLC–HMI Automation) | + M06, M07, M10 | Mini-capstone: PLC+HMI+Modbus คุม conveyor | เขียน PLC/HMI พื้นฐาน ตั้ง comm ติดตั้ง/ดูแลเครื่องอัตโนมัติ |
| **L4** | **Specialist Certificate**<br>(System Integration) | + M08, M09, M11, **M12** | **Capstone Pick & Place เต็มระบบ** | บูรณาการ servo+robot+comm ออกแบบ/commission เครื่องทั้งระบบ |
| **L4+** | **Specialist + Industry 4.0 (Elective)** | + M13 | Mini-capstone Digital Factory | ต่อยอด SCADA/IIoT/OPC UA สู่ smart factory |

> **เทียบมาตรฐานภายนอก:** ผู้ที่ผ่าน L1–L3 มีความรู้/ทักษะครอบคลุมเกณฑ์สอบ **มาตรฐานฝีมือแรงงานแห่งชาติ สาขาช่างควบคุมด้วยระบบ PLC ระดับ 1** ของกรมพัฒนาฝีมือแรงงาน (ดูเอกสาร B §11) — ศูนย์ฝึกควรพิจารณาขึ้นทะเบียนเป็นสถานที่ทดสอบมาตรฐานฝีมือแรงงานที่ได้รับการรับรอง เพื่อให้ผู้เรียนสอบเทียบวุฒิภายนอกได้ในที่เดียว เป็นทั้งจุดขายและ ancillary revenue (ดูเอกสาร F §7)

### 5.2 เงื่อนไขการได้รับใบรับรองแต่ละระดับ
- ผ่านทุกโมดูลในระดับนั้น **และทุกระดับก่อนหน้า** (stackable)
- ผ่าน **Safety Gate ทุกโมดูล** (ไม่มีข้อยกเว้น)
- ผ่านการสอบปฏิบัติรวบยอด (capstone/integrated exam) ของระดับ ≥ 75%
- ใบรับรองระบุ: ระดับ, รายการ competency ที่ผ่าน, เกรด (PASS/Merit/Distinction), วันหมดอายุ

### 5.3 การต่ออายุ (Recertification)
| ระดับ | อายุใบรับรอง | เงื่อนไขต่ออายุ |
|:---:|:---:|---|
| L1–L2 | 3 ปี | refresher ความปลอดภัย (LOTO) + ประเมินทักษะ |
| L3–L4 | 2 ปี | refresher + สอบปฏิบัติ troubleshooting ใหม่ |

### 5.4 ใบรับรองเสริมเฉพาะทาง (Add-on Certificate) — Machinery Safety Assessor

นอกเหนือจากสาย L1–L4 (สร้าง/บูรณาการระบบ) หลักสูตรควรเปิดใบรับรองเสริมสำหรับผู้ที่อยากต่อยอดสาย **ประเมิน/ออกแบบความปลอดภัยเครื่องจักร** โดยเฉพาะ — เป็นสายอาชีพจริงที่ตลาดต้องการ (เช่น safety auditor ของโรงงาน, ผู้ตรวจรับรองเครื่องจักรใหม่ก่อนใช้งาน) และต่อยอดจากฐานความรู้ safety ที่หลักสูตรมีอยู่แล้วอย่างแข็งแรง (เอกสาร B)

| ใบรับรองเสริม | เงื่อนไขเข้าเรียน | เนื้อหาเพิ่มจากฐานเดิม | ผลลัพธ์ |
|---|---|---|---|
| **Machinery Safety Assessor (เสริม)** | มี L2 ขึ้นไป (ผ่าน M00–M05) | ขยายจาก เอกสาร B §1 (Hierarchy of Controls) และ §10 (Functional Safety) เป็นหลักสูตรเต็ม: Risk Assessment เชิงลึกตาม ISO 12100, การกำหนด PLr/PL ตาม ISO 13849-1 แบบคำนวณจริง (ไม่ใช่แค่ concept), การตรวจรับรองเครื่องจักรก่อนใช้งาน, การเขียนรายงานประเมินความเสี่ยง | ประเมิน/ตรวจรับรองความปลอดภัยเครื่องจักรในโรงงานได้ ไม่ใช่แค่ทำงานปลอดภัยตามที่กำหนด |

> ใบรับรองนี้เป็น **ทางเลือกคู่ขนาน ไม่ใช่ prerequisite ของ L3–L4** — เหมาะกับผู้เรียนที่ต้องการเป็น safety officer/auditor มากกว่าสาย technician/integration สามารถขายเป็นคอร์สสั้นแยก (ดูเอกสาร F §3.1) โดยใช้เนื้อหา M00 + เอกสาร B เป็นฐาน แล้วเสริมความลึกเฉพาะด้าน risk assessment/PL calculation

---

## 6. การ Map กับงานจริง (Job Description)

ตารางเชื่อมแต่ละระดับใบรับรองกับตำแหน่งงานจริงในโรงงาน เพื่อใช้รับสมัคร/เลื่อนตำแหน่ง

### 6.1 Certificate → ตำแหน่งงาน

| ระดับ | ตำแหน่งงานที่รองรับ | ระดับเงิน/อาวุโส (อ้างอิง) | งานหลักที่ทำได้ |
|:---:|---|---|---|
| **L1 Foundation** | ช่างเดินสาย/Panel Wirer, ผู้ช่วยช่างไฟฟ้า | Entry / Junior | เดินสายตามแบบ, ประกอบตู้, ย้ำหางปลา, LOTO, อ่าน schematic |
| **L2 Operator** | Machine Operator (เครื่องอัตโนมัติ), ช่างซ่อมบำรุงระดับต้น | Junior | เปลี่ยน sensor/วาล์ว/มอเตอร์, ตั้ง inverter, ดูแลระบบลม, แก้ปัญหาเบื้องต้น |
| **L3 Technician** | Automation Technician, Maintenance Technician, PLC Service | Mid-level | แก้ไข/ปรับ ladder, ออกแบบหน้า HMI, ตั้ง Modbus/Ethernet, commission เครื่อง |
| **L4 Specialist** | Automation Engineer (Junior), System Integrator, Commissioning Engineer | Senior Tech / Junior Eng | ออกแบบระบบ Pick & Place, integrate servo+robot+comm, เขียน safety concept, นำทีม build & commission |
| **L4+ Industry 4.0** | OT/SCADA Engineer, IIoT Integrator | Specialist | เชื่อมข้อมูลขึ้น SCADA/cloud, predictive maintenance, OT cybersecurity |

### 6.2 ตัวอย่าง JD แบบละเอียด — Automation Technician (ต้องมี L3)

```
ตำแหน่ง: Automation Technician
ใบรับรองที่ต้องมี: Technician Certificate (L3) ขึ้นไป

หน้าที่รับผิดชอบ (Key Responsibilities):
  • วินิจฉัยและแก้ไขปัญหาเครื่องจักรอัตโนมัติ (PLC/HMI/sensor/motor) ลด downtime
  • ปรับแก้โปรแกรม ladder บน GX Works3 และหน้าจอ HMI บน GT Designer3
  • ตั้งค่าและแก้ปัญหาการสื่อสาร Modbus/Ethernet ระหว่างอุปกรณ์
  • ทำ preventive maintenance และจัดทำเอกสาร as-built

Competency ที่ต้องพิสูจน์ได้ (จาก checklist):
  ✓ M06: download/online monitor + แก้ ladder ได้ (≥ระดับ 3)
  ✓ M07: ตั้ง comm GOT–FX5U + แก้หน้าจอ (≥ระดับ 3)
  ✓ M10: FX5U Modbus master + diagnose ping/timeout (≥ระดับ 3)
  ✓ Safety: LOTO + live-dead-live (ระดับ 4 บังคับ)
```

### 6.3 ตัวอย่าง JD แบบละเอียด — Commissioning / Integration Engineer (ต้องมี L4)

```
ตำแหน่ง: Commissioning Engineer (System Integration)
ใบรับรองที่ต้องมี: Specialist Certificate (L4)

หน้าที่รับผิดชอบ:
  • ออกแบบ system architecture (I/O list, network topology, SOO, safety concept)
  • build ตู้ไฟทั้งระบบ + commission เครื่อง multi-discipline (PLC+HMI+servo+robot+comm)
  • คำนวณ electronic gear / positioning, tuning servo, set robot handshake
  • รับผิดชอบ safety เครื่อง (ISO 13849-1 PLr) และ sign-off first power-up

Competency ที่ต้องพิสูจน์ได้:
  ✓ M08: positioning math + pre-power-on check servo (≥ระดับ 3)
  ✓ M09: robot T1 teach + handshake PLC (≥ระดับ 3)
  ✓ M11: FB modular + SFC sequence (≥ระดับ 3)
  ✓ M12: Capstone ทั้งระบบ ผ่าน megger + first power-up + ครูเซ็น (≥ระดับ 3)
```

---

## 7. ตัวอย่างข้อสอบปฏิบัติ (Practical Exams)

แต่ละข้อสอบมี: โจทย์, เงื่อนไข/เวลา, เกณฑ์ rubric, Safety Gate, และ "fault ที่ครูแอบใส่" สำหรับวัด troubleshooting

---

### 7.1 [L1 / M00–M02] สอบปฏิบัติ "DOL Motor Starter + Reversing"

**โจทย์:** ประกอบและเดินสายตู้ DOL พร้อม forward/reverse ตามแบบ schematic ที่ให้ มอเตอร์ 3-phase สั่ง start/stop ได้ มี seal-in, OLR trip, และ interlock กันการสั่งสองทิศพร้อมกัน

**เงื่อนไข:** เวลา 180 นาที | ทำงานคนเดียว | ใช้ datasheet จริงประกอบ

**Safety Gate (Pass/Fail):**
- [ ] ทำ LOTO ก่อนเข้าทำงานในตู้
- [ ] verify zero energy ด้วย live-dead-live ก่อนต่อสาย
- [ ] ขออนุญาตครู + checklist ก่อน first power-up

**Rubric (100 คะแนน):**

| หัวข้อ | คะแนน | เกณฑ์ผ่าน |
|---|:---:|---|
| อ่านแบบ + trace วงจรถูกต้อง | 15 | ระบุจุดต่อตรงแบบ |
| คุณภาพงานย้ำสาย (pull test + ferrule) | 20 | สุ่ม 5 จุด ผ่านทุกจุด |
| Torque + workmanship จัดสายในราง | 15 | เป็นระเบียบ ไม่หลวม |
| วงจร power ถูกต้อง | 15 | มอเตอร์หมุนถูกทิศ |
| seal-in + OLR + interlock ทำงาน | 25 | `[Critical]` interlock กัน short ได้ 100% |
| Safety & LOTO discipline | 10 | ปฏิบัติครบ |

**Troubleshooting (โบนัส/บังคับ L1+):** ครูแอบสลับสาย control 1 เส้น → ผู้เรียนต้องหา fault ด้วย DMM ภายใน 20 นาทีและอธิบายวิธีไล่

---

### 7.2 [L2 / M03–M05] สอบปฏิบัติ "Electro-Pneumatic Sequence + Inverter"

**โจทย์:** ต่อวงจร electro-pneumatic ให้ double-acting cylinder ทำลำดับ A+ → (delay) → A− ควบคุมด้วย solenoid + relay มี reed switch ยืนยันตำแหน่ง และต่อ inverter FR คุมมอเตอร์ conveyor 2 ความเร็วผ่าน multi-speed terminal

**เงื่อนไข:** เวลา 150 นาที

**Safety Gate (Pass/Fail):**
- [ ] depressurize + verify เกจ = 0 ก่อนต่อ/แก้สายลม
- [ ] LOTO ก่อนต่อ inverter, pre-power-on check

**Rubric (100 คะแนน):**

| หัวข้อ | คะแนน | เกณฑ์ |
|---|:---:|---|
| อ่าน ISO 1219 + ต่อวงจรลมถูก | 20 | cylinder เคลื่อนถูกทิศ |
| ปรับความเร็ว meter-out ถูกหลัก | 10 | `[Critical]` เลือก meter-out สำหรับยกโหลด |
| sensor reed wiring + อ่านค่าเข้า relay | 15 | สถานะถูกต้อง |
| sequence A+/delay/A− ทำงานครบ | 20 | ครบ cycle |
| ต่อ + set inverter FR multi-speed | 25 | 2 ความเร็วถูกต้อง |
| Safety (ลม+ไฟ) | 10 | ครบ |

**Troubleshooting:** ครูถอด termination/ตั้ง parameter inverter ผิด 1 จุด → หาและแก้

---

### 7.3 [L3 / M06–M07] สอบปฏิบัติ "PLC + HMI Conveyor Sorting"

**โจทย์:** เขียนโปรแกรม FX5U + หน้าจอ GOT สำหรับสายพานคัดแยกชิ้นงาน:
- ปุ่ม Start/Stop (มี seal-in ใน ladder), E-stop (hardwired)
- sensor นับชิ้นงาน, เมื่อครบ 5 ชิ้น → กระบอกผลักทำงาน
- HMI แสดง: สถานะ run/stop (lamp animation), จำนวนนับ (numerical), ปุ่ม reset
- comm GOT–FX5U ผ่าน Ethernet ต้องติดจริง

**เงื่อนไข:** เวลา 210 นาที | ส่ง ladder + screen + device mapping table

**Safety Gate (Pass/Fail):**
- [ ] E-stop ต่อ hardwired ไม่ผ่าน HMI/PLC logic อย่างเดียว
- [ ] pre-power-on check I/O ด้วย DMM (sink/source ถูก)

**Rubric (100 คะแนน):**

| หัวข้อ | คะแนน | เกณฑ์ |
|---|:---:|---|
| ladder ทำงานตาม spec (นับ/ผลัก/seal-in) | 30 | ครบทุกเงื่อนไข |
| ตั้ง comm GOT–FX5U ติดจริง | 20 | `[Critical]` |
| HMI: switch/lamp/numerical + animation | 20 | map device ถูก |
| device mapping table + documentation | 10 | ครบถ้วน อ่านง่าย |
| online monitor + แก้ bug สด | 10 | แก้ได้ |
| Safety (E-stop hardwired + I/O check) | 10 | `[Critical]` |

**Troubleshooting:** ตั้ง IP คนละ subnet หรือ double-coil → วินิจฉัยและแก้

---

### 7.4 [L4 / M08–M09] สอบปฏิบัติ "Servo Positioning + Robot Pick"

**โจทย์ส่วน A (Servo):** คำนวณ electronic gear ให้ ballscrew lead 10 mm เคลื่อน 0.01 mm/pulse แล้ว set MELSERVO + เขียน FX5U สั่ง homing + DRVA ไป 3 ตำแหน่ง วัดความแม่นด้วย dial gauge (±0.1 mm)

**โจทย์ส่วน B (Robot):** teach KUKA pick จากตำแหน่ง A → place ที่ B ใน T1, handshake กับ PLC (PLC สั่ง start → robot ทำงาน → robot ส่ง done กลับ)

**เงื่อนไข:** เวลา 240 นาที

**Safety Gate (Pass/Fail):**
- [ ] pre-power-on check servo wiring (CN1/CN2/CN8) ก่อนจ่ายไฟ
- [ ] teach ใน T1 (250 mm/s), มือบน enabling switch, อยู่ตำแหน่งปลอดภัย

**Rubric (100 คะแนน):**

| หัวข้อ | คะแนน | เกณฑ์ |
|---|:---:|---|
| คำนวณ CMX/CDV + pulse/ระยะ ถูก | 15 | `[Critical]` ตรงโจทย์ |
| servo wiring + pre-power check | 15 | ผ่าน checklist |
| homing + DRVA ตำแหน่งแม่น ±0.1 mm | 20 | วัด dial gauge ผ่าน |
| robot mastering + teach point | 15 | ตำแหน่งถูก |
| handshake PLC↔robot ทำงานครบ | 25 | `[Critical]` cycle ครบ |
| Safety (servo + robot T1) | 10 | `[Critical]` |

---

### 7.5 [L4 Capstone / M12] สอบรวบยอด "Full Pick & Place Build & Commission"

**โจทย์:** จากแบบที่ให้ ทีม (2–3 คน) ต้องส่งมอบเครื่อง Pick & Place ที่ทำงานครบ cycle:
conveyor (inverter) นำชิ้นงานเข้า → sensor part-present → servo axis/robot หยิบ → place ลงตำแหน่ง → HMI ควบคุม/แสดงสถานะ → safety relay + E-stop + light curtain ทำงาน

**ส่งมอบ (Deliverables):**
1. เอกสาร: I/O list, network topology, Sequence of Operation (SOO), safety concept (PLr + ISO 13849-1)
2. ตู้ไฟต่อครบ ผ่าน continuity + insulation (megger 500VDC) test
3. โปรแกรม PLC (step/SFC + FB), HMI, robot KRL, parameter servo/inverter
4. รายงาน first power-up ทีละ section (มีลายเซ็นครูอนุมัติทุก section)

**เงื่อนไข:** 3–5 วันทำการ | ประเมินทั้งกระบวนการ + ผลงาน + presentation

**Safety Gate (Pass/Fail — บังคับสูงสุด):**
- [ ] LOTO ตลอดงาน build
- [ ] **ครูเซ็นอนุมัติก่อน first power-up ทุก section**
- [ ] safety circuit (E-stop/light curtain) ทดสอบผ่านก่อนเดินเครื่อง full
- [ ] robot AUT เฉพาะเมื่อ guard ปิดและคนอยู่นอก zone

**Rubric (100 คะแนน):**

| หมวด | คะแนน | เกณฑ์ผ่าน |
|---|:---:|---|
| เอกสารออกแบบ (I/O, topology, SOO, safety concept) | 20 | ครบ ตรวจสอบได้ ตรง as-built |
| คุณภาพ wiring + grounding/shielding | 15 | ผ่าน continuity + megger |
| โปรแกรม PLC (sequence/FB) ทำงานครบ cycle | 20 | ทุก step + home/reset ปลอดภัย |
| Integration PLC+HMI+Servo+Robot+Comm | 20 | `[Critical]` handshake ทุกตัวติด |
| Commissioning + first power-up discipline | 10 | section-by-section + sign-off |
| Troubleshooting (fault ที่ครูใส่) | 10 | หา+แก้อย่างเป็นระบบ |
| Presentation + ส่งมอบเอกสาร | 5 | ครบถ้วน |

**Troubleshooting (บังคับ):** ครูแอบใส่ 3 fault ข้าม discipline เช่น (1) สาย encoder หลวม, (2) IP comm ผิด subnet, (3) parameter inverter ผิด → ทีมต้องวินิจฉัยเป็นระบบและบันทึก root cause

---

## 8. แบบฟอร์มและเครื่องมือประเมิน

### 8.1 ใบ `SAFETY-GATE-OBSERVATION` (ใช้ทุกครั้งที่จ่ายไฟ/ลม/รันหุ่นยนต์)

```
ชื่อผู้เรียน: __________  โมดูล: ____  วันที่: ____
[ ] แจ้ง/ขออนุญาตก่อนทำงาน
[ ] LOTO ครบสเต็ป (ไฟ) / depressurize+verify (ลม)
[ ] live-dead-live ก่อนต่อสาย
[ ] PPE ครบ + ตรวจถุงมือฉนวน
[ ] robot: T1 + enabling switch + อยู่นอก zone (ถ้ามี)
[ ] pre-power-on check ด้วย DMM ก่อนจ่ายไฟครั้งแรก
ผล: ☐ PASS  ☐ FAIL (ระบุ): __________
ลายเซ็นครู: __________
```

### 8.2 ใบสรุปผลโมดูล `MODULE-RESULT`

```
โมดูล: ____  ผู้เรียน: __________
ทฤษฎี (30%): ____%   ปฏิบัติ (50%): ____%   Safety/Workmanship (20%): ____%
Safety Gate: ☐ PASS ☐ FAIL
Competency checklist: ทุกข้อ ≥3 ☐  ข้อ Critical ≥3 ☐
เกรด: ☐ Distinction ☐ Merit ☐ Pass ☐ Refer ☐ Fail
หมายเหตุ/แผน remediation: __________
```

### 8.3 Passport ทักษะ (Skill Passport) — สะสมสู่ใบรับรอง

ตารางติดตามความก้าวหน้ารายโมดูล (ใช้กับ portfolio ของผู้เรียน):

| โมดูล | ผ่านเมื่อ | เกรด | Safety Gate | สะสมสู่ระดับ |
|:---:|:---:|:---:|:---:|:---:|
| M00–M02 | | | | → L1 |
| M03–M05 | | | | → L2 |
| M06,M07,M10 | | | | → L3 |
| M08,M09,M11,M12 | | | | → L4 |
| M13 | | | | → L4+ |

---

### หมายเหตุการนำไปใช้
- ปรับช่วงเวลาสอบ/จำนวน fault ตามขนาดกลุ่มและจำนวนชุดฝึกได้
- ควรเก็บ **ภาพถ่าย/วิดีโองานปฏิบัติ** เข้า portfolio เป็นหลักฐาน competency โดยเฉพาะ L3–L4
- แนะนำให้ **ครู 2 คนประเมินอิสระ** สำหรับสอบ Capstone (M12) เพื่อลด bias และให้คะแนน rubric ตรงกัน (inter-rater)
- ทุกระดับใบรับรองยึดหลักเดียวกัน: **"ปลอดภัยก่อน → ทำได้จริง → อธิบาย/แก้ปัญหาเองได้"**