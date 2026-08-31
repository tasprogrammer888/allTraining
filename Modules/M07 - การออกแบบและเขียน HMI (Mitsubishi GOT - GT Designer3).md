# [M07] การออกแบบและเขียน HMI (Mitsubishi GOT / GT Designer3)

**ระดับ:** Basic | **ระยะเวลา:** 28 ชั่วโมง | **Prerequisite:** M06 (PLC พื้นฐาน FX5U / GX Works3)

> โมดูลนี้ต่อยอดจาก M06 โดยตรง: ผู้เรียนนำโปรแกรม PLC ที่เขียนไว้แล้วมาสร้างหน้าจอ HMI ควบคุมและ monitor เครื่องให้ทำงานได้จริงบน GOT2000 ของจริง พร้อมทักษะเดินสาย ตั้งค่า communication สองฝั่ง และ troubleshooting ที่ใช้ได้หน้างานจริง

---

## 1. ภาพรวมและปรัชญาของโมดูล

หลายหลักสูตรสอน HMI แบบ \"ลากออบเจกต์มาวาง\" แต่ในงานจริง จุดที่ผู้เรียนตกม้าตายคือ **communication ไม่ติด** และ **ความเข้าใจผิดว่า HMI ทำหน้าที่ safety ได้** โมดูลนี้จึงเริ่มจาก hardware/wiring + การตั้งค่าสองฝั่ง (PLC และ GOT) ก่อน แล้วจึงไล่ขึ้นไปถึงการออกแบบหน้าจอ UX และจบด้วย troubleshooting drill + capstone

### หลักความปลอดภัยที่ต้องย้ำตั้งแต่ชั่วโมงแรก
> **HMI ไม่ใช่ safety device** — Emergency Stop, motor overload, safety interlock ต้องเป็น **hardwired** เสมอ ปุ่มบนจอใช้สำหรับ operation ปกติเท่านั้น และต้องสมมุติเสมอว่า \"จอค้าง/comm หลุดได้ทุกเมื่อ\" ระบบหยุดฉุกเฉินต้องทำงานได้แม้จอตาย

---

## 2. Learning Objectives

เมื่อจบโมดูล ผู้เรียนจะสามารถ:
1. อธิบายสถาปัตยกรรม GOT–PLC, device link และข้อจำกัดด้าน safety ของ HMI
2. เดินสาย/จ่ายไฟ GOT2000 (24VDC, FG) และเลือก/ต่อสาย Ethernet ได้ถูกต้อง
3. ตั้งค่า communication **ครบสองฝั่ง** (FX5U ใน GX Works3 + GOT ใน GT Designer3) จน comm ติดจริง
4. สร้างหน้าจอด้วย switch/lamp/numerical/text/figure + screen switching และ map device ถูกต้อง
5. ออกแบบ lamp animation และสื่อสถานะด้วยภาพตามมาตรฐานสี
6. สร้างระบบ alarm, recipe, trend graph, data logging
7. ตั้ง security/operator level และออกแบบ UX/navigation/interlock บนจอ
8. อ่าน manual/help จริงเพื่อหาคำตอบและ device range
9. troubleshoot comm error / device ไม่ขยับ / หน้าจอไม่สลับ อย่างเป็นขั้นตอน
10. transfer ลง GOT จริงและส่งมอบ HMI ครบชุดสำหรับเครื่องจาก M06

---

## 3. โครงสร้างเนื้อหา (28 ชั่วโมง)

| บท | หัวข้อ | ชม. |
|----|--------|-----|
| 1 | หลักการ HMI, สถาปัตยกรรม GOT–PLC และความปลอดภัย | 3 |
| 2 | Hardware, การเดินสาย และตั้งค่าการสื่อสาร GOT–FX5U | 4 |
| 3 | ออบเจกต์พื้นฐาน — Switch, Lamp, Numerical/Text + Screen Switching | 4 |
| 4 | Lamp animation และการสื่อสถานะด้วยภาพ | 3 |
| 5 | ระบบ Alarm สำหรับ operation และ maintenance | 3 |
| 6 | Recipe, Trend Graph และ Data Logging | 3 |
| 7 | Security, Operator Level และหลัก UX | 3 |
| 8 | Simulation, Transfer, Troubleshooting และโปรเจกต์รวมจาก M06 | 5 |
| | **รวม** | **28** |

---

## 4. ความรู้เชิงเทคนิคที่ต้องเข้าใจ (เน้นจุดที่ทำให้ \"ใช้งานได้จริง\")

### 4.1 Device ที่ HMI อ้างอิง

| ประเภท | ตัวอย่าง | ใช้ทำอะไร |
|--------|----------|-----------|
| PLC bit device | X, Y, M | ปุ่ม, สถานะ ON/OFF, fault flag |
| PLC word device | D, (T/C ค่า) | ค่าตัวเลข, setpoint, parameter |
| GOT internal bit | GB | logic ภายในจอ (เช่น flag หน้าจอ) ไม่เปลือง memory PLC |
| GOT internal word | GD | screen switching device, ตัวแปรชั่วคราวบนจอ |

> **เคล็ดหน้างาน:** ใช้ `GD` เป็น screen switching device และใช้ `GB/GD` กับ logic ที่เป็นของจอล้วน ๆ จะไม่ไปกินพื้นที่ device ของ PLC และทำให้ PLC program สะอาดขึ้น

### 4.2 การตั้งค่า Communication — หัวใจของโมดูล (สาเหตุ comm error อันดับ 1)

ต้องตั้ง **สองฝั่ง** ถึงจะติด:

**ฝั่ง PLC (GX Works3) — มักลืม:**
```
Module Parameter > FX5U CPU > Ethernet Port
  - IP Address           : 192.168.1.10
  - Subnet Mask          : 255.255.255.0
  - External Device Config: เปิด MELSOFT Connection (และ/หรือ SLMP)
  → จากนั้น download ลง PLC
```

**ฝั่ง GOT (GT Designer3):**
```
Common > Controller Setting > CH1
  - Manufacturer   : MITSUBISHI ELECTRIC
  - Controller Type: MELSEC iQ-F (FX5)
  - I/F            : Ethernet
  - GOT IP Address : 192.168.1.18   (ห้ามชนกับ PLC/PC)
  - PLC IP Address : 192.168.1.10   (ตรงกับที่ตั้งใน GX Works3)
```

**Checklist ก่อนบอกว่า \"comm พัง\":**
- [ ] PLC กับ GOT อยู่ subnet เดียวกัน (เช่น 192.168.1.x)
- [ ] IP ไม่ชนกัน (PLC ≠ GOT ≠ PC)
- [ ] `ping 192.168.1.10` จาก PC ผ่าน
- [ ] ตั้ง IP ฝั่ง PLC แล้ว **download** ลง PLC จริง
- [ ] เปิด MELSOFT/SLMP connection ฝั่ง PLC
- [ ] สาย Ethernet link LED ติด (เลือกชนิดสายถูก)
- [ ] ไม่มี System Alarm เรื่อง comm บนจอ

### 4.3 การจ่ายไฟและกราวด์ GOT2000

```
   +24V ──► [ + ]
   0V   ──► [ - ]        GOT2000 Power Terminal
   PE   ──► [ FG ] ──► กราวด์ตู้ (ground bar)
```
- FG **ต้องต่อ** เพื่อกัน noise และ comm หลุดเป็นช่วง
- วัดแรงดันยืนยัน 24VDC และตรวจขั้วก่อนจ่ายไฟ (ต่อกลับขั้วจออาจเสียหาย)

### 4.4 ตัวอย่าง ladder ฝั่ง PLC ที่คู่กับปุ่มบนจอ

ปุ่ม Start/Stop บนจอควรใช้ **momentary** แล้วทำ latch ใน PLC (ปลอดภัยกว่า alternate เมื่อ comm หลุด):

```
// M0 = ปุ่ม Start (momentary จาก HMI)
// M1 = ปุ่ม Stop  (momentary จาก HMI)
// X0 = E-stop hardwired (NC), Y0 = มอเตอร์

|--[ M0 ]--+--[/ M1 ]--[/ X0 ]--( Y0 )|
|--[ Y0 ]--+

// E-stop (X0) เป็น NC hardwired: หลุดเมื่อใดก็ตัด Y0 ทันที ไม่พึ่ง HMI
```

> ถ้าใช้ Bit Switch แบบ **alternate** กับปุ่มเดินเครื่อง แล้ว comm หลุดตอนสถานะ ON เครื่องอาจค้างเดิน — จึงควร momentary + latch หรือใส่ watchdog/heartbeat ระหว่าง GOT กับ PLC

### 4.5 มาตรฐานสีสถานะ (ใช้ให้สม่ำเสมอทุกหน้า)

| สี | ความหมาย | หมายเหตุ |
|----|----------|----------|
| เขียว | Run / ปกติ | |
| แดง | Fault / Stop / อันตราย | + ไฟกระพริบเมื่อ active |
| เหลือง | Warning / รอ | |
| ขาว/เทา | Off / ไม่พร้อม | |

> รองรับผู้ตาบอดสี: อย่าพึ่งสีอย่างเดียว ให้มี **รูป/ไอคอน/ข้อความ** ประกอบเสมอ

---

## 5. Hands-on Labs

> ทุกแล็บต้องผ่าน safety check ก่อนจ่ายไฟ และทดสอบกับ GOT/PLC จริง (ใช้ simulator ช่วยตอน develop)

| Lab | ชื่อ | ผลลัพธ์หลัก |
|-----|------|-------------|
| 0 | เดินสาย/Power-up GOT + อ่านคู่มือจริง | GOT บูตปกติ, กราวด์ครบ, ใบงานค่าจากคู่มือ |
| 1 | ตั้ง Ethernet สองฝั่งให้ comm ติดจริง | comm กับ FX5U ติด, lamp ตาม M0 |
| 2 | Manual Control + Screen Switching | Start/Stop/อ่าน-ป้อนค่า/สลับหน้าได้จริง |
| 3 | Lamp Animation + Status Overview | อ่านสถานะจากภาพได้ทันที |
| 4 | Alarm + Alarm History | เตือน fault + log เวลา + acknowledge |
| 5 | Recipe + Trend Graph | โหลด recipe + กราฟ trend จริง |
| 6 | Security/Operator Level + Navigation + Interlock | login กันปุ่มอันตราย + navigation ครบ |
| 7 | **Troubleshooting Drill** | แก้เคสเสียจำลองได้ตามเวลา |
| 8 | **Capstone:** HMI ครบชุดสำหรับเครื่องจาก M06 | ควบคุม/monitor เครื่องจริงครบฟังก์ชัน |

### Lab 7 — Troubleshooting Drill (จุดเด่นของโมดูลฉบับปรับปรุง)
ผู้สอนจงใจสร้างจุดเสีย ผู้เรียนไล่แก้แบบมีระบบ:

| เคส | อาการ | ทักษะที่ฝึก |
|-----|-------|-------------|
| IP ผิด/ชน | comm error | ping, ตรวจ subnet, GOT Utility |
| สายหลุด/ผิดชนิด | link ไม่ขึ้น | ตรวจ LED, เลือกสาย |
| map device ผิด | lamp/ค่าไม่ขยับ | เทียบ device mapping, monitor GX Works3 |
| screen switch ผิด | หน้าจอไม่สลับ | ตรวจ switching device |
| data type ผิด | ค่าตัวเลขเพี้ยน | ตรวจ format/16-32bit/signed |
| recipe ผิดตำแหน่ง | ค่าเข้าผิด D | ตรวจ device list ของ recipe |

**ใบงาน:** อาการ → สมมุติฐาน → วิธีตรวจ → สาเหตุ → การแก้ (ครบทุกเคส)

---

## 6. ข้อผิดพลาดที่พบบ่อย & ความปลอดภัย (Checklist)

- [ ] **ห้าม** ใช้ปุ่ม Stop/E-stop บนจอเป็นทางหยุดฉุกเฉินเดียว — E-stop/overload/interlock ต้อง hardwired และตัดได้แม้จอค้าง
- [ ] ตั้ง IP + เปิด MELSOFT/SLMP ฝั่ง PLC แล้ว **download** (ตั้งฝั่ง GOT อย่างเดียวไม่ติด)
- [ ] ping ผ่านก่อนเสมอ, IP ไม่ชน, subnet เดียวกัน
- [ ] ต่อ FG/กราวด์ และวัดแรงดัน/ตรวจขั้วก่อนจ่ายไฟ
- [ ] เลือกสาย Ethernet ถูกชนิด (straight ผ่าน switch / crossover เมื่อ direct ที่ไม่ auto-MDIX)
- [ ] ตั้ง upper/lower limit ทุก Numerical Input
- [ ] ปุ่มเดินเครื่องใช้ momentary + latch ไม่ใช่ alternate
- [ ] สื่อสถานะด้วยสี + รูป/ข้อความ (รองรับตาบอดสี) และสม่ำเสมอทุกหน้า
- [ ] Lockout/Tagout ขณะต่อสาย ห้ามต่อสายขณะมีไฟ

---

## 7. การประเมินผล

- **ภาคปฏิบัติรายแล็บ (60%)** — Lab 0–7 ผ่าน deliverable: comm ติดจริง, ออบเจกต์ทำงานจริง, troubleshooting แก้ได้ตามเวลา
- **Capstone โมดูล (30%)** — Lab 8: ชุดหน้าจอครบ ทำงานบน GOT จริง + เอกสาร device mapping/navigation + ผลทดสอบ E-stop hardwired
- **ทฤษฎี/ปากเปล่า (10%)** — device link, ความปลอดภัย HMI, การเลือกชนิด switch, การอ่านคู่มือ

> **เกณฑ์ผ่าน:** ทุกแล็บผ่าน, capstone ใช้งานได้จริง, ตอบหลัก \"HMI ไม่ใช่ safety device\" ได้ถูกต้อง

---

## 8. ความต่อเนื่องกับโมดูลอื่น

- **มาจาก M06:** ใช้ทักษะ device (X/Y/M/D), ladder, การตั้ง Ethernet + download ของ FX5U โดยตรง; capstone ต่อยอดเครื่องจาก M06
- **ไปยัง M11 (System Integration, มี M07 เป็น prereq):** device mapping, alarm, recipe, trend, security, troubleshooting comm เป็นฐานของการบูรณาการ
- **ไปยัง M12 (Capstone Pick & Place):** ต้องมี HMI ควบคุม/monitor ทั้งเครื่อง
- **ไปยัง M13 (SCADA/IIoT):** ขยายแนวคิด operator interface สู่ระดับโรงงาน
- **ไม่ซ้ำซ้อน:** เนื้อหา ladder/PLC logic อยู่ใน M06; M07 เน้น \"หน้าจอ + การเชื่อมต่อ + UX + troubleshooting comm\" เท่านั้น

---

## 9. อุปกรณ์และซอฟต์แวร์

- PC + GT Works3 (GT Designer3 for GOT2000) + GX Works3 (รองรับ FX5U)
- GT Simulator3 + GX Simulator3
- GOT2000 จริง (แนะนำ built-in Ethernet) + FX5U จาก M06 พร้อม I/O ทดสอบ
- 24VDC supply, เบรกเกอร์/ฟิวส์, terminal block
- สาย Ethernet (straight + crossover) + switch เล็ก, สาย USB, SD/USB drive
- มัลติมิเตอร์ (safety check)
- E-stop + overload/safety relay จริง (สาธิต safety hardwired)
- คู่มือจริง: GOT2000 Connection Manual, GT Designer3 Help, FX5U User's Manual (Ethernet)