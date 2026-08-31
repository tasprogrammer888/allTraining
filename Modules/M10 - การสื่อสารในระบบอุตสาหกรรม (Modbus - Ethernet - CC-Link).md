# [M10] การสื่อสารในระบบอุตสาหกรรม (Modbus / Ethernet / CC-Link)

**Industrial Communication (Modbus / Ethernet / CC-Link)**

| | |
|---|---|
| **Module ID** | M10 |
| **Level** | Intermediate |
| **Duration** | 32 ชั่วโมง (บรรยาย + ปฏิบัติ เน้นมือทำ ~65%) |
| **Prerequisite** | M06 (PLC FX5U/GX Works3), M08 (Servo/Inverter + advanced wiring), M00/M01 (safety + wiring) |
| **ต่อยอดสู่** | M11 (System Integration), M12 (Capstone), M13 (SCADA/IIoT) |

---

## 1. ภาพรวมและเหตุผลของโมดูล

ในโรงงานจริง "เครื่องเดินไม่ได้" ครึ่งหนึ่งของเคสมาจากการสื่อสาร ไม่ใช่ logic ผิด โมดูลนี้ทำให้ผู้เรียนเปลี่ยนจาก "ต่อ I/O เส้นต่อเส้น" ไปสู่การให้ PLC, HMI, inverter, servo และ robot **คุยกันผ่าน network** ได้จริง และที่สำคัญกว่าคือ **หาเหตุเมื่อมันไม่คุยกันให้เจอ**

> หัวใจของโมดูล: ต่อให้ติด + รู้ว่าทำไมไม่ติด + ทำเอกสารส่งมอบเป็น

### สิ่งที่ปรับปรุงจากร่างเดิม (สำหรับผู้สอน)
- **เพิ่ม Lab อ่าน manual จริง (Lab 3)** ก่อนลงมือ Modbus — เดิมพูดถึงแต่ไม่มี drill ทำให้ผู้เรียนลอกค่าครูแล้วทำงานเองไม่ได้
- **แยก safety check เข้า Lab RS-485 (Lab 4)** + เพิ่มหัวข้อ safety ชัดเจน (ห้าม hot-plug, ESD, แยกสายกำลัง)
- **ระบุคำสั่งจริงของ FX5U (ADPRW / predefined protocol)** และพารามิเตอร์ Pr. จริงของ FR แทนการพูดลอย ๆ ว่า "ตั้ง Modbus master"
- **แก้บท HMI ให้ใช้ SLMP/MC protocol (วิธี native ของ Mitsubishi)** เป็นหลัก ไม่ใช่ Modbus TCP ซึ่งไม่ตรงงานจริง GOT+FX5U
- **บท KUKA ปรับสมจริง**: ใช้ bus ที่ config ไว้แล้ว เพราะ config WorkVisual เต็มรูปแบบใน 2 ชม.เป็นไปไม่ได้ และเน้น state machine handshake + safety แยก
- **เพิ่ม Lab 9 fault-injection drill จับเวลา** — troubleshooting คือทักษะ #1 หน้างาน เดิมบางเกินไป
- **เพิ่ม Lab 10 mini-project** ส่งต่อ artifact เข้า M12 โดยตรง

---

## 2. Learning Objectives

เมื่อจบโมดูล ผู้เรียนสามารถ:

1. แยกแยะ serial (RS-232/422/485) vs Ethernet เลือกใช้ตามระยะ/จำนวน node/งานได้
2. คำนวณและตั้ง IP/subnet ให้อุปกรณ์อยู่ subnet เดียวกัน และวินิจฉัยเมื่อ ping ไม่ผ่าน
3. ต่อ RS-485 daisy-chain + termination 120 ohm + ตรวจสายก่อนจ่ายไฟ
4. อ่าน communication manual หา register map และแปลง 4xxxx -> protocol address
5. ทำให้ FX5U เป็น Modbus RTU master คุม inverter FR ได้จริง พร้อม error handling
6. เชื่อม PLC-HMI ผ่าน Ethernet (SLMP) และเข้าใจความต่างจาก Modbus TCP
7. ตั้ง CC-Link IE Field (master/remote, RX/RY/RWr/RWw, auto refresh)
8. ทำ handshake PLC<->KUKA และเข้าใจว่า safety ต้องแยกจาก process bus
9. troubleshoot แบบ 4 ชั้น (physical -> parameter -> protocol -> application)
10. ออกแบบ architecture + mapping table + commissioning doc ของเครื่อง 1 ระบบ
11. ปฏิบัติตาม safety: ไม่ hot-plug, กัน ESD, แยกสายสื่อสาร/กำลัง

---

## 3. โครงสร้างเนื้อหา (9 บท / 32 ชม.)

| บท | หัวข้อ | ชม. |
|---|---|---|
| 1 | พื้นฐานการสื่อสารข้อมูลและเครือข่ายในโรงงาน | 3.5 |
| 2 | IP Addressing และพื้นฐาน Ethernet | 3.5 |
| 3 | Modbus เชิงลึก (RTU/TCP) + อ่าน manual | 4.5 |
| 4 | การต่อสายและตั้งค่า RS-485 (+ safety) | 3 |
| 5 | FX5U เป็น Modbus RTU Master คุม Inverter FR | 5 |
| 6 | Modbus TCP และ PLC-HMI ผ่าน Ethernet (SLMP) | 3 |
| 7 | CC-Link IE Field | 4 |
| 8 | เชื่อม PLC กับ KUKA ผ่าน Fieldbus | 2.5 |
| 9 | Troubleshooting + เอกสารส่งมอบ | 3 |
| | **รวม** | **32** |

---

## 4. รายละเอียดบทเรียนสำคัญ (พร้อมตัวอย่างสอน)

### บทที่ 3: Modbus Data Model

| Object | Address range | อ่าน/เขียน | Function Code |
|---|---|---|---|
| Coil | 0xxxx | R/W | 01 (read), 05/15 (write) |
| Discrete Input | 1xxxx | R | 02 |
| Input Register | 3xxxx | R | 04 |
| Holding Register | 4xxxx | R/W | 03 (read), 06/16 (write) |

**กับดักที่คนพลาดบ่อย (สอนให้เน้น):**
- `40001` (เลขที่หน้า manual) = **protocol address 0** (off-by-one)
- ค่า 32-bit (เช่น output frequency บางรุ่น) ต้องดู **word order** ให้ตรง ไม่งั้นค่าเพี้ยนเป็นพันเท่า
- Modbus RTU มี **CRC16**, Modbus TCP ไม่มี CRC แต่มี **MBAP header**

### บทที่ 4: RS-485 wiring (ตัวอย่าง daisy-chain)

```
  FX5U (485-BD)        FR Inverter #1        FR Inverter #2
  A(D+)----------+------A(D+)---------+-------A(D+)
  B(D-)----------|---+--B(D-)------+--|-------B(D-)
  SG ------------|---|--SG --------|--|-------SG
                 |   |             |  |
            [120R termination ที่ "ปลายสายทั้งสองด้านเท่านั้น"]
   วัดรวมเมื่อมี 2 ตัว = ~60 ohm  (ใช้ multimeter ยืนยันก่อนจ่ายไฟ)
   - shield ต่อ ground "จุดเดียว"
   - ห้ามต่อแบบ star, ห้าม hot-plug
```

**Checklist ก่อนจ่ายไฟ RS-485:**
- [ ] A ตรง A, B ตรง B ทุก node (ไม่สลับ)
- [ ] SG เชื่อมถึงกัน
- [ ] termination 120 ohm ที่ปลายทั้งสอง (วัดได้ ~60 ohm)
- [ ] shield ground จุดเดียว
- [ ] baud/parity/stop/station ตั้งตรงกันทุก node และจดใน mapping table
- [ ] สายสื่อสารแยกจากสายกำลัง/VFD output

### บทที่ 5: พารามิเตอร์ FR สำหรับ Modbus RTU (ตัวอย่าง อ้างอิง manual จริงเสมอ)

| Pr. | ความหมาย | ค่าตัวอย่าง |
|---|---|---|
| Pr.549 | Protocol selection | 1 = Modbus RTU |
| Pr.331 | Station number | 1, 2, ... |
| Pr.332 | Communication speed (baud) | 96 = 9600 |
| Pr.334 | Parity | ตามที่ตั้งฝั่ง PLC |
| Pr.79 | Operation mode | ให้สั่งผ่าน network ได้ |

> ค่า Pr. ต่างกันตามรุ่น (FR-E800 vs FR-D700) — **ผู้เรียนต้องเปิด manual ของรุ่นที่ใช้จริง (Lab 3) ห้ามจำ**

**ตัวอย่าง concept ladder (FX5U, ADPRW) อ่าน output frequency (FC03):**
```
[trigger] -- ADPRW  K1  H03  H...  K1  D100 --   ; slave1, FC03, start addr, qty1, -> D100
            (อ่านค่าจาก holding register มาเก็บ D100)
[trigger] -- ADPRW  K1  H06  H...  K1  D200 --   ; FC06 เขียน set frequency จาก D200
ลำดับ: set freq -> run -> read actual -> stop  (trigger ทีละคำสั่ง อย่ายิงพร้อมกัน)
```

### บทที่ 7: CC-Link IE Field — Link Device

| Link device | ทิศทาง | ชนิด |
|---|---|---|
| RX | remote -> master | bit input |
| RY | master -> remote | bit output |
| RWr | remote -> master | word read |
| RWw | master -> remote | word write |

ตั้ง network parameter + **auto refresh** ใน GX Works3 เพื่อ map RX/RY -> device PLC (X/M, D) อัตโนมัติ แล้วใช้ diagnostics ดูว่าทุก station online

### บทที่ 8: Handshake PLC <-> KUKA (state machine)

```
PLC -> Robot : Program_Select (word), Start (bit), Reset (bit)
Robot -> PLC : Busy, Done, Error, At_Home (bits)

ลำดับ: PLC set Program_Select -> pulse Start
       -> Robot set Busy -> ทำงาน -> set Done (clear Busy)
       -> PLC อ่าน Done แล้ว clear Start
[Safety: E-stop / robot enable / safety gate เดินผ่าน safety relay หรือ safety bus แยก
 ห้ามผ่าน fieldbus process เด็ดขาด]
```

### บทที่ 9: Troubleshooting 4 ชั้น

| ชั้น | ตรวจอะไร | เครื่องมือ |
|---|---|---|
| Physical | สาย/ขั้ว A-B, termination, ไฟ, LED SD/RD | multimeter, ตา, cable tester |
| Parameter | baud/parity/station/IP/subnet | manual, module param |
| Protocol | FC, exception code, CRC, frame | Modbus poll, Wireshark |
| Application | logic, scaling, word order, timeout | GX Works3 monitor |

---

## 5. Hands-on Labs (10 แล็บ)

| # | Lab | Deliverable หลัก |
|---|---|---|
| 1 | จำแนกสาย + เข้าหัว Ethernet | สาย LAN ผ่าน tester + ตารางจำแนกสาย |
| 2 | IP plan + ping/arp (+ inject fault) | IP plan + ผล ping + บันทึก 2 เคสผิด |
| 3 | อ่าน manual FR หา register map | register map table ใช้จริง |
| 4 | RS-485 daisy-chain + safety check | บอร์ดต่อถูก + pre-power-on checklist เซ็นรับรอง |
| 5 | FX5U <-> FR ผ่าน Modbus RTU | โปรแกรมสั่ง/อ่านความเร็วจริง + error เมื่อตัดสาย |
| 6 | คุม Inverter ผ่าน HMI GOT (SLMP) | หน้าจอ HMI สั่ง+monitor ได้ + ไฟล์ GT Designer3 |
| 7 | CC-Link IE Field + remote I/O | โปรเจกต์ R/W remote I/O + mapping + diagnostics |
| 8 | Handshake PLC <-> KUKA | signal mapping + robot รัน 1 งานรับ done + ยืนยัน safety แยก |
| 9 | Fault-injection drill (จับเวลา) | troubleshooting log ครบทุกเคส + เวลาที่ใช้ |
| 10 | Mini-project: architecture เครื่อง 1 ระบบ | diagram + mapping table + commissioning checklist |

> **Critical labs (ต้องผ่าน):** 5, 7, 8, 9

---

## 6. ความปลอดภัยและข้อผิดพลาดที่พบบ่อย

**Safety (ย้ำทุกแล็บ):**
- ห้าม **hot-plug** สายสื่อสารขณะมีไฟ (พอร์ตพัง)
- กัน **ESD** ก่อนจับ board/connector
- แยกสายสื่อสารออกจาก **สายกำลัง/VFD output**, shield ground จุดเดียว
- **safety signal (E-stop/robot enable) ห้ามผ่าน fieldbus process** ต้องใช้ safety relay/safety bus แยก

**Top mistakes:**
1. สลับขั้ว A/B — สาเหตุอันดับ 1
2. termination ผิดตำแหน่ง/จำนวน
3. baud/parity/station/IP ไม่ตรงกัน
4. off-by-one ของ Modbus address + word order ผิด
5. IP/station conflict
6. ยิงคำสั่ง Modbus พร้อมกันบน master เดียว -> bus ชน/timeout
7. ไม่ตั้ง timeout/error handling -> scan ค้าง

---

## 7. การประเมินผล

| ส่วน | น้ำหนัก |
|---|---|
| Lab deliverables (เน้น 5,7,8) | 50% |
| Lab 9 fault-injection (root cause + log + เวลา) | 25% |
| Lab 10 mini-project architecture | 15% |
| แบบทดสอบความเข้าใจ (Modbus/IP/RS-485/safety) | 10% |

**เกณฑ์ผ่าน:** Critical labs (5,7,8,9) ผ่านทุกตัว และคะแนนรวม ≥ 70%

---

## 8. ความต่อเนื่องกับโมดูลอื่น (ไม่ซ้ำ ไม่มีช่องว่าง)

- **รับจาก M06:** ladder, device M/D/X/Y, monitor — โมดูลนี้ "ไม่สอนซ้ำ" แต่ใช้เป็นฐานในการเขียน comm logic
- **รับจาก M08:** การตั้ง inverter/servo ผ่าน I/O/parameter — โมดูลนี้ยกระดับเป็นการคุมผ่าน network
- **รับจาก M09 (ไม่บังคับ):** KUKA basics — บทที่ 8 ใช้ robot ที่ config bus ไว้แล้ว เพื่อไม่ทับ scope M09
- **ส่งให้ M11:** กลไก handshake และ data exchange เป็นกระดูกสันหลังของ system integration
- **ส่งให้ M12 (Capstone):** artifact จาก Lab 10 (architecture + mapping + checklist) ใช้ต่อโดยตรง + ทักษะ troubleshooting 4 ชั้นคือหัวใจ commissioning
- **ส่งให้ M13:** Modbus TCP/SLMP/network เป็นพื้นฐานก่อนต่อ OPC UA/MQTT/SCADA

---

## 9. อุปกรณ์และซอฟต์แวร์ที่ต้องเตรียม

- FX5U + RS-485 option (FX5-485-BD/ADP) + Ethernet
- GX Works3, GT Designer3, GOT2000
- Inverter FR ≥ 2 ตัว + มอเตอร์โหลดเบา
- CC-Link IE Field remote I/O ≥ 1 station
- KUKA + smartPAD (bus config แล้ว, ใช้ร่วม cell M09)
- Industrial Ethernet switch, สาย UTP/STP, crimp tool, cable tester, ferrule
- Multimeter, เครื่องมือเข้าขั้ว
- PC + Modbus Poll/qModMaster + Wireshark
- **communication manual จริงของ FR และ FX5U** (สำหรับ Lab 3)
