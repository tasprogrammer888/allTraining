# รายการอุปกรณ์และซอฟต์แวร์ทั้งหลักสูตร
## Equipment & Software Master List — หลักสูตรช่างไฟฟ้า/ซ่อมบำรุงสู่ผู้เชี่ยวชาญระบบอัตโนมัติ

> เอกสารนี้รวบรวมอุปกรณ์ทั้งหมดจาก M00–M13 จัดกลุ่มเป็น 4 หมวด: (ก) ฮาร์ดแวร์ฝึก/เทรนเนอร์คิท (ข) เครื่องมือช่าง (ค) ซอฟต์แวร์+เวอร์ชัน (ง) วัสดุสิ้นเปลือง
> สมมติฐานขนาดคลาส: **ผู้เรียน 12 คน / 1 รุ่น, จัดเป็น 6 กลุ่ม (กลุ่มละ 2 คน)** — เป็นฐานในการคำนวณ "จำนวนต่อผู้เรียน/ต่อกลุ่ม"
> ระดับการจัดซื้อแบ่งเป็น: **[ประหยัด]** = เปิดสอนได้แบบ lean, **[ครบ]** = อุปกรณ์เต็มรูปแบบเทียบ vocational lab จริง

---

## สารบัญ
1. [หลักการวางแผนจัดซื้อ](#หลักการ)
2. [ก. ฮาร์ดแวร์ฝึก / เทรนเนอร์คิท](#ก-ฮาร์ดแวร์)
3. [ข. เครื่องมือช่าง](#ข-เครื่องมือ)
4. [ค. ซอฟต์แวร์ + เวอร์ชัน](#ค-ซอฟต์แวร์)
5. [ง. วัสดุสิ้นเปลือง](#ง-วัสดุ)
6. [ทางเลือกราคาประหยัด vs ครบ (สรุปเชิงกลยุทธ์)](#ทางเลือก)
7. [ชุดเทรนเนอร์ขั้นต่ำเพื่อเปิดสอน (Minimum Viable Lab)](#ขั้นต่ำ)
8. [Checklist ก่อนเปิดรุ่นแรก](#checklist)
9. [จ. โครงสร้างพื้นฐาน Remote Lab (เรียนออนไลน์คุมเครื่องจริง)](#remote-lab)
10. [ฉ. ชุดอุปกรณ์เคลื่อนที่สำหรับสอนต่างจังหวัด (Mobile / On-site Kit)](#mobile-kit)

---

<a name="หลักการ"></a>
## 1. หลักการวางแผนจัดซื้อ

| หลักการ | คำอธิบาย |
|---|---|
| **1 platform ตลอดสาย** | ยึด Mitsubishi (FX5U / GOT2000 / MELSERVO / FR) + KUKA เป็นแกน เพื่อให้ skill โอนข้ามโมดูลได้ ไม่ต้องเรียนรู้ ecosystem ใหม่ |
| **ใช้ซ้ำข้ามโมดูล** | FX5U, GOT, multimeter, LOTO kit, ferrule tool ฯลฯ ซื้อครั้งเดียวใช้ตั้งแต่ M04 ถึง M13 — ลงทุนหนักครั้งเดียว |
| **Simulator-first** | GX Simulator3 / GT Simulator3 / FluidSIM / MR Configurator2 ลดจำนวน hardware rig ที่ต้องซื้อในช่วงเรียน logic |
| **ของจริงเฉพาะ "ทักษะมือ + commissioning"** | wiring, crimp, megger, power-up sequence ต้องของจริง — simulator แทนไม่ได้ |
| **Capstone ใช้ rig รวม** | M12 Pick & Place 1–2 ชุดต่อทั้งคลาส (rotate ใช้งาน) เพราะเป็น integration ไม่ใช่ทักษะเดี่ยว |
| **Remote Lab เพิ่ม utilization** | rig เครื่องจริง (PLC/HMI/servo/robot/P&P) ต่อกล้อง+remote access เปิดให้ **ผู้เรียนออนไลน์จองใช้นอกเวลา** ได้ — สินทรัพย์ก้อนใหญ่ 1 ชุดบริการทั้ง on-site (กลางวัน) + online (เย็น/สุดสัปดาห์) ดู §จ (Remote Lab) |

---

<a name="ก-ฮาร์ดแวร์"></a>
## 2. ก. ฮาร์ดแวร์ฝึก / เทรนเนอร์คิท

### 2.1 PLC / HMI (แกนหลักของหลักสูตร — ใช้ M04–M13)

| รายการ | รุ่นจริงที่แนะนำ | จำนวน | ใช้ในโมดูล | หมายเหตุ |
|---|---|---|---|---|
| PLC Mitsubishi FX5U | **FX5U-32MT/ES** (transistor output — รองรับ high-speed pulse สำหรับ servo) | **1 ชุด / กลุ่ม (รวม 6)** + 1 สำรอง | M04, M06–M13 | เลือก **MT (transistor)** ไม่ใช่ MR (relay) เพื่อให้ทำ positioning M08 ได้ |
| PLC FX5U (ทางเลือก I/O มาก) | FX5U-64MT/ES | optional 1–2 | M11, M12 | สำหรับ rig ที่ I/O เยอะ |
| HMI GOT2000 | **GT2107-WTBD** (7", built-in Ethernet) | **1 เครื่อง / กลุ่ม (รวม 6)** | M07, M08, M11–M13 | รุ่นใหญ่ขึ้น GT2310 ได้ถ้างบถึง |
| FX5 RS-485 communication BD | **FX5-485-BD** หรือ **FX5-485-ADP** | 1 / กลุ่ม (6) | M10, M13 | จำเป็นสำหรับ Modbus RTU master |
| Ethernet switch (unmanaged) | 5–8 port industrial/desktop | 2–3 / คลาส | M07, M10, M13 | |
| Ethernet switch (managed) | รองรับ VLAN (Moxa/Hirschmann หรือ TP-Link managed) | **1 / คลาส** | M10, M13 | สอน diagnostics/VLAN/cybersecurity |

### 2.2 Servo / Inverter (M05, M08, M10, M12)

| รายการ | รุ่นจริง | จำนวน | หมายเหตุ |
|---|---|---|---|
| Servo amplifier + motor | **MELSERVO MR-JE-A** + HG-KN servo motor (เริ่มต้น) หรือ **MR-J4-A** | **1 ชุด / กลุ่ม (3–6)** | MR-JE เพียงพอสำหรับสอน; MR-J4/J5 สำหรับ Capstone ขั้นสูง |
| Servo ขั้นสูง (Capstone) | **MR-J4-A** หรือ **MR-J5-A** + HG motor | 1–2 ชุด / คลาส | ใช้ใน M11–M12 |
| Inverter (พื้นฐาน) | **FR-D720S** / **FR-D700 series** | **1 / กลุ่ม (6)** | สอน multi-speed, analog command |
| Inverter (ขั้นสูง/comm) | **FR-E800** (รองรับ Modbus/CC-Link ในตัว) ≥2 ตัว | 2 / คลาส | M10 Modbus, M13 ดึงค่า health |
| มอเตอร์ 3-phase (สาธิต Y/Δ) | induction 0.37–2.2 kW, **มีทั้ง 230/400V และ 400/690V** | อย่างละ 1–2 | M05 decision logic ต้องมี nameplate 2 แบบ |
| Ballscrew slide / linear axis 1 แกน | linear actuator + near-point dog + limit switch ×2 | **1 / กลุ่ม (3) หรือ 2 / คลาส** | M08 positioning |
| Regenerative resistor + CN8 short connector สำรอง | ตาม spec servo | 1 ชุด / คลาส | M08 |

### 2.3 Robot (M09, M11, M12)

| รายการ | รุ่นจริง | จำนวน | หมายเหตุ |
|---|---|---|---|
| หุ่นยนต์ KUKA 6-axis | **KR 6 R900 sixx (KR AGILUS)** + **KR C4** หรือ **KR C5 compact** controller | **1 เซลล์ / คลาส** (ของแพง — แชร์ทั้งรุ่น) | งบสูงสุดในหลักสูตร |
| smartPAD / smartPAD2 | มากับ controller | 1 (+1 สำรองถ้าได้) | |
| EMD (Electronic Mastering Device) | KUKA EMD หรือ dial gauge สำหรับ comparison mastering | 1 / คลาส | M09 mastering |
| Safety fence / light curtain + E-stop ภายนอก | ตามมาตรฐาน ISO 10218 | 1 ชุด / เซลล์ | บังคับด้านความปลอดภัย |
| Robot fieldbus card | EtherNet/IP หรือ PROFINET (config ไว้แล้ว) | 1 | M10 บทที่ 8 |

### 2.4 เทรนเนอร์บอร์ด / ชุดฝึกตามโมดูล

| รายการ | รุ่น/สเปก | จำนวน | โมดูล |
|---|---|---|---|
| ตู้ฝึก LOTO | เบรกเกอร์จริง + multi-lock hasp + lockout station | **1 / กลุ่ม (6)** | M00 |
| ตู้ฝึกไฟฟ้า (มี MCB curve B/C, RCD/ELCB 30mA, fuse, PE bar) | รองรับการแอบใส่ fault | 6 | M00, M01 |
| แผงฝึกประกอบตู้ (back plate + DIN rail + wire duct) | 35mm DIN rail, wire duct หลายขนาด | **1 / คน หรือ 1 / กลุ่ม** | M01, M02, M05 |
| ชุด contactor + overload (DOL/reversing/star-delta) | **Mitsubishi MS-T/S-T series** + **TH-T overload** + mechanical interlock kit | 1 ชุด / กลุ่ม (6) | M02, M05 |
| ชุด relay/timer | ice cube relay 8/11-pin + base, **Omron H3CR / MY series** timer | ≥4 / กลุ่ม | M02, M03 |
| ชุดฝึกนิวเมติกส์ (training board) | cylinder single/double-acting, 3/2 & 5/2 valve (manual + solenoid 24VDC), FRL unit, manifold | **1 / กลุ่ม (6)** | M03 |
| Pneumatic gripper (สาธิต P&P) | gripper เล็ก + vacuum option | 2 / คลาส | M03, M09, M12 |
| ชุดเซนเซอร์ (sensor kit) | inductive/capacitive/photoelectric (NPN+PNP), reed, encoder (incremental+absolute), 4-20mA transmitter | **1 ชุด / กลุ่ม (6)** | M04 |
| ชุด actuator output | solenoid valve DC, interposing relay, contactor coil | 1 / กลุ่ม | M04 |
| Conveyor + sorting board (Capstone ย่อย) | สายพานเล็ก + sensor + inverter | 2–3 / คลาส | M06, M10, M12 |
| CC-Link IE Field remote I/O | **NZ2GF series** | 1–2 / คลาส | M10 |
| **ชุด trainer Pick & Place เต็มระบบ** | servo+ballscrew axis + conveyor+inverter + pneumatic gripper + sensors + safety relay + E-stop + light curtain | **1–2 ชุด / คลาส** | **M12 Capstone** |
| หุ่นฝึก CPR + AED trainer | CPR manikin + AED trainer | 1 ชุด / คลาส | M00 ปฐมพยาบาล |
| IoT Gateway / edge | **Raspberry Pi 4 (4–8GB)** หรือ Moxa/Advantech gateway | 1–2 / คลาส | M13 (elective) |

---

<a name="ข-เครื่องมือ"></a>
## 3. ข. เครื่องมือช่าง

### 3.1 เครื่องมือวัด/ทดสอบ

| รายการ | รุ่นจริงที่แนะนำ | จำนวน | โมดูล |
|---|---|---|---|
| Digital multimeter (CAT III 600V+) | **Fluke 117 / 115** (true-RMS) หรือ Hioki/Kyoritsu เทียบเท่า | **1 / คน (12)** | ทุกโมดูล |
| Clamp meter (มี inrush capture) | **Fluke 376 FC** / Hioki CM4373 | **1 / กลุ่ม (6)** | M00, M05, M13 |
| Insulation tester / megger | **Fluke 1507** / **Kyoritsu 3005A** (500/1000V) | **1 / กลุ่ม (6)** | M00, M01, M05, M12 |
| Two-pole voltage tester | **Fluke T6 / T150** | 1 / กลุ่ม (6) | M00, M02 (live-dead-live) |
| Non-contact voltage tester (NCV) | Fluke 1AC | 1 / กลุ่ม | M00 |
| Oscilloscope / peak detector | scope พกพา 1 ตัว (สาธิต flyback spike) | **1 / คลาส** | M04 |
| Bench DC power supply (ปรับค่าได้) | 0–30V/5A | 1 / กลุ่ม (6) | M00, M04 |
| Signal generator / loop calibrator 4-20mA | hand-held loop calibrator | 1–2 / คลาส | M04, M05 |
| Cable tester (RJ45) | LAN cable tester | 1–2 / คลาส | M10, M13 |
| Dial gauge / ไม้บรรทัดเหล็ก (วัด positioning) | dial indicator + magnetic base | 1–2 / คลาส | M08, M09 |
| Stopwatch (วัด cycle time) | นาฬิกาจับเวลา/แอป | ตามกลุ่ม | M09, M12 |

### 3.2 เครื่องมือเข้าหัวสาย / ประกอบตู้

| รายการ | สเปก | จำนวน | โมดูล |
|---|---|---|---|
| Wire stripper | ปรับขนาดได้ | **1 / คน** | M01+ |
| Ratchet crimping tool (หางปลา) | ring/fork lug crimper | 1 / กลุ่ม | M01, M08 |
| Ferrule crimper (bootlace) | self-adjusting ferrule crimper | **1 / กลุ่ม (6)** | M01–M12 (ทุกงาน wiring) |
| Torque screwdriver (ปรับค่าได้) | ตามค่า spec ขั้วต่อ | 1 / กลุ่ม | M01, M05, M08, M12 |
| ชุดไขควง insulated (1000V) | flat/Phillips/Torx | **1 ชุด / คน** | ทุกโมดูล |
| Cable cutter | สำหรับสาย control + power | 1 / กลุ่ม | M01+ |
| ตัวตัดสายลม (PU hose cutter) | | 1 / กลุ่ม | M03 |
| Ferrule printer / wire marker tool (optional) | | 1 / คลาส | M01 |

### 3.3 ความปลอดภัยส่วนบุคคล (PPE) + LOTO

| รายการ | สเปก | จำนวน |
|---|---|---|
| ถุงมือฉนวน (insulated gloves) class 00/0 + leather protector | ตามมาตรฐาน | 1 คู่ / คน |
| แว่นตานิรภัย + face shield | arc-rated | 1 / คน |
| รองเท้า safety + เสื้อ arc-rated | | 1 / คน |
| ชุด LOTO (padlock + hasp + tag) | multi-lock | **1 / คน** (padlock) + station กลาง |
| ผ้ายางปูพื้น (live test mat) | | 2–3 / คลาส |

---

<a name="ค-ซอฟต์แวร์"></a>
## 4. ค. ซอฟต์แวร์ + เวอร์ชัน

> **PC สเปก:** Windows 10/11 64-bit, RAM **≥8GB (แนะนำ 16GB สำหรับ M13)**, Ethernet port, USB — **1 เครื่อง / คน (12)** หรืออย่างน้อย 1 / กลุ่ม

### 4.1 ซอฟต์แวร์หลัก (Mitsubishi + KUKA) — ต้องมีลิขสิทธิ์

| ซอฟต์แวร์ | เวอร์ชัน/รุ่น | License | โมดูล | หมายเหตุ |
|---|---|---|---|---|
| **GX Works3** | เวอร์ชันรองรับ FX5U (≥1.x ล่าสุด) | มีลิขสิทธิ์ (education/site license) | M06–M13 | แกนหลัก PLC |
| **GX Simulator3** | มากับ GX Works3 | รวมในชุด | M06, M11, M13 | รัน ladder ไม่ต้องมี hardware |
| **GT Works3 / GT Designer3 Version1** | for GOT2000 | มีลิขสิทธิ์ | M07, M08, M11, M12 | HMI design |
| **GT Simulator3** | มากับ GT Works3 | รวมในชุด | M07, M11 | จำลอง GOT |
| **MR Configurator2** | เวอร์ชันรองรับ MR-JE/J4/J5 | มีลิขสิทธิ์ (มักฟรีให้ download) | M08, M11, M12 | tuning servo |
| **FR Configurator2** | สำหรับ FR series | ฟรี (Mitsubishi) | M05, M12 | parameter inverter |
| **KUKA System Software (KSS)** | **KSS 8.x** | มากับ controller | M09, M11, M12 | บน robot controller |
| **KUKA WorkVisual** | เวอร์ชันคู่กับ KSS | ฟรี (KUKA Xpert) | M09, M12 | config/programming offline |

### 4.2 ซอฟต์แวร์เสริม (ส่วนใหญ่ฟรี/trial)

| ซอฟต์แวร์ | เวอร์ชัน | License | โมดูล |
|---|---|---|---|
| **FluidSIM (Festo)** | ล่าสุด | trial/edu | M03 จำลองวงจรนิวเมติกส์ |
| **CAD ไฟฟ้า** — EPLAN Education / AutoCAD Electrical / **QElectroTech (ฟรี)** | ล่าสุด | edu/ฟรี | M01, M02 |
| **draw.io** | web/desktop | ฟรี | M02 เขียน schematic เบื้องต้น |
| **Modbus Poll / qModMaster** | qModMaster ฟรี / Modbus Poll trial | ฟรี/trial | M10 |
| **Wireshark** | ล่าสุด | ฟรี | M10, M13 packet analysis |
| **Ignition Maker Edition** | ล่าสุด | **ฟรีถาวร (เรียนรู้)** | M13 SCADA (แนะนำ) |
| **KEPServerEX** | trial | trial 2 ชม. reset | M13 OPC UA |
| **UaExpert** | ล่าสุด | ฟรี | M13 OPC UA client |
| **Mosquitto + MQTT Explorer** | ล่าสุด | ฟรี | M13 MQTT |
| **Node-RED + InfluxDB + Grafana** | ล่าสุด | ฟรี (open-source) | M13 dashboard stack |

---

<a name="ง-วัสดุ"></a>
## 5. ง. วัสดุสิ้นเปลือง (ซื้อซ้ำทุกรุ่น)

> ประมาณการต่อ **1 รุ่น (12 คน)** — ควรเผื่อ 20–30% สำหรับของเสีย/ฝึกซ้ำ

### 5.1 สายไฟ + ปลายสาย

| รายการ | สเปก | ปริมาณ/รุ่น (ประมาณ) |
|---|---|---|
| สาย THW | 1.5 / 2.5 / 4.0 mm² (หลายสี) | ม้วนละ ~100 m × หลายสี |
| สาย VAF / VCT / control cable multi-core / shielded | ตามแบบ | ตามจำนวน lab |
| สาย shielded twisted pair (RS-485) | สำหรับ Modbus | 1 ม้วน |
| สาย UTP Cat5e/Cat6 + RJ45 connector | crimp เอง | 1 ม้วน + RJ45 ×100 |
| หางปลา ring/fork | หลายขนาด | กล่องละ ≥500 |
| Bootlace ferrule (single/twin) + color code | หลายขนาด | กล่องละ ≥1000 |
| Pin terminal | | กล่อง |
| Termination resistor 120Ω (RS-485) | | ≥20 |

### 5.2 อุปกรณ์ติดตั้ง / จัดสาย (กึ่งสิ้นเปลือง)

| รายการ | ปริมาณ |
|---|---|
| Terminal block (screw + push-in), end stop, ground/fuse terminal | กล่อง |
| Cable tie, spiral wrap, wire marker / wire duct (ตัดใช้) | ตามงาน |
| Wire ferrule / wire tag (label) | กล่อง |

### 5.3 นิวเมติกส์ + อื่นๆ

| รายการ | ปริมาณ |
|---|---|
| สายลม PU + push-in fitting (หลายขนาด) | ม้วน + กล่อง fitting |
| Flyback / surge diode (1N4007), RC snubber | กล่อง |
| ฟิวส์ (0.5–2A) + fuse holder | กล่อง |
| ชุดถ่วงน้ำหนักจำลองโหลด (meter-out demo) | ใช้ซ้ำได้ |
| target วัสดุเซนเซอร์: เหล็ก/อะลู/สแตนเลส/พลาสติก/กระดาษ/ภาชนะน้ำ + แผ่นสะท้อน | ชุดสาธิต (ใช้ซ้ำ) |

### 5.4 สิ้นเปลืองห้องเรียน / เอกสาร

| รายการ |
|---|
| กระดาษ grid (เขียน schematic), ใบ checklist พิมพ์ (LOTO/dead test/energization/workmanship/fault report) |
| SD card / USB drive (data logging M07, M13) |
| datasheet/manual ฉบับพิมพ์ (contactor, overload, timer, MCB, sensor, FX5U, GOT, MELSERVO, FR, KUKA KSS) |
| โปสเตอร์: ขั้นตอน LOTO, รหัสสีสาย, IEC 60617, ตารางพิกัดกระแสสาย |

---

<a name="ทางเลือก"></a>
## 6. ทางเลือกราคาประหยัด vs ครบ (สรุปเชิงกลยุทธ์)

| หมวด | [ประหยัด] เปิดสอนได้ | [ครบ] เต็มรูปแบบ |
|---|---|---|
| **PLC** | FX5U-32MT × 3 (กลุ่มละ 2 คนใช้ร่วม 4 คน) + GX Simulator3 หนัก | FX5U × 6–7 (1/กลุ่ม + สำรอง) |
| **HMI** | GT Simulator3 บน PC + GOT2000 จริง 2 เครื่องหมุนเวียน | GT2107 × 6 (1/กลุ่ม) |
| **Servo** | MR-JE-A × 2–3 หมุนเวียน + simulator | MR-JE × 3 + MR-J4/J5 × 2 (Capstone) |
| **Inverter** | FR-D720S × 2 | FR-D700 × 6 + FR-E800 × 2 (comm) |
| **Robot** | KUKA 1 เซลล์ + WorkVisual offline (ทุกกลุ่มเขียน offline, รันจริงหมุนเวียน) | KUKA 1 เซลล์ + safety fence/light curtain เต็ม + EMD แท้ |
| **Pick & Place rig** | 1 ชุด หมุนเวียนทั้งรุ่น | 2 ชุด (ลดเวลารอคิว) |
| **เครื่องมือวัด** | Hioki/Kyoritsu/UNI-T (CAT III) แทน Fluke | Fluke 117/376/1507 |
| **CAD/SCADA** | QElectroTech + Ignition Maker (ฟรี) | EPLAN Education + Ignition/WinCC |
| **CC-Link / managed switch** | ข้าม (สอนเป็นทฤษฎี + วิดีโอ) ใน M10 | NZ2GF + managed switch จริง |
| **IoT (M13 elective)** | Raspberry Pi 4 + stack ฟรี | IoT gateway industrial + managed switch/firewall |

**หลักการตัดงบ:** ตัด "จำนวนชุด" ก่อน (หมุนเวียน + simulator) → ตัด "elective M13/CC-Link hardware" → **ห้ามตัด:** PPE, LOTO, megger, multimeter, ferrule tool, robot safety fence (ความปลอดภัยห้ามประนีประนอม)

---

<a name="ขั้นต่ำ"></a>
## 7. ชุดเทรนเนอร์ขั้นต่ำเพื่อเปิดสอน (Minimum Viable Lab)

> เป้าหมาย: เปิดสอนรุ่นแรก **12 คน** ครอบคลุม M00–M12 (เลื่อน M13 elective ออกไปก่อน) ด้วยงบต่ำสุดที่ยังให้ทักษะมือ + commissioning ของจริง

### 7.1 ขั้นต่ำที่ "ขาดไม่ได้"

| # | รายการ | จำนวนขั้นต่ำ | เหตุผล |
|---|---|---|---|
| 1 | FX5U-32MT/ES | **3** | กลุ่มละ 2 คน, 6 กลุ่มใช้หมุนเวียน 2 กลุ่ม/ตัว |
| 2 | GOT2000 (GT2107) | **2** | ใช้ของจริง + GT Simulator3 เสริม |
| 3 | MELSERVO MR-JE-A + motor + linear axis | **1** | M08/M11/M12 หมุนเวียน |
| 4 | Inverter FR-D720S + motor 3-phase | **2** | M05 + M10 Modbus (ต้อง ≥2 สำหรับ comm) |
| 5 | KUKA KR 6 R900 + KR C4/C5 + smartPAD + safety fence | **1 เซลล์** | งบก้อนใหญ่สุด — แชร์ทั้งรุ่น |
| 6 | ตู้ฝึกไฟฟ้า/LOTO + แผงประกอบตู้ | **6** (1/กลุ่ม) | ทักษะมือ — simulator แทนไม่ได้ |
| 7 | ชุด contactor+overload (DOL/reversing/star-delta) | **3** | M02/M05 หมุนเวียน |
| 8 | ชุดนิวเมติกส์ training board + sensor kit | **3** | M03/M04 หมุนเวียน |
| 9 | มอเตอร์ 3-phase 230/400V + 400/690V (สาธิต Y/Δ) | อย่างละ **1** | M05 decision logic |
| 10 | Pick & Place trainer rig | **1** | M12 Capstone หมุนเวียน |
| 11 | FX5-485-BD | **2** | M10 Modbus |
| 12 | Ethernet switch (unmanaged) | **2** | M07/M10 |

### 7.2 เครื่องมือ + PPE ขั้นต่ำ (ห้ามตัด)

- Multimeter CAT III: **6** (1/กลุ่ม)
- Megger/insulation tester: **2**
- Two-pole voltage tester: **2**, Clamp meter: **2**
- Ferrule crimper + crimping tool หางปลา: **3 ชุด**
- ชุดไขควง insulated: **6 ชุด**
- Torque screwdriver: **2**
- PPE (ถุงมือฉนวน/แว่น/รองเท้า): **1 ชุด/คน (12)**
- LOTO padlock: **1/คน (12)** + station กลาง
- CPR manikin + AED trainer: **1 ชุด** (M00)

### 7.3 ซอฟต์แวร์ขั้นต่ำ

- GX Works3 + GX Simulator3 (ลิขสิทธิ์)
- GT Works3/GT Designer3 + GT Simulator3 (ลิขสิทธิ์)
- MR Configurator2 + FR Configurator2 (ฟรี/รวมในชุด)
- KUKA KSS (มากับ controller) + WorkVisual (ฟรี)
- FluidSIM trial, QElectroTech (ฟรี), qModMaster + Wireshark (ฟรี)
- PC: **6 เครื่อง** (1/กลุ่ม, RAM 8GB+)

### 7.4 สิ่งที่ "เลื่อนได้" ในรุ่นแรก
M13 IoT stack ทั้งหมด · CC-Link NZ2GF · managed switch · MR-J4/J5 (ใช้ MR-JE แทนชั่วคราว) · FR-E800 (ใช้ FR-D720S 2 ตัวทำ Modbus ได้) · oscilloscope (สาธิตด้วยวิดีโอ/peak detector) · ferrule printer

---

<a name="checklist"></a>
## 8. Checklist ก่อนเปิดรุ่นแรก

- [ ] PLC FX5U-**MT** (transistor) ยืนยันแล้ว — ไม่ใช่ MR (relay) เพื่อรองรับ positioning M08
- [ ] ลิขสิทธิ์ GX Works3 + GT Works3 พร้อมใช้บนทุก PC
- [ ] FX5-485-BD/ADP สั่งครบ (M10 ขาดไม่ได้)
- [ ] Inverter อย่างน้อย 2 ตัว (Modbus master/slave demo)
- [ ] มอเตอร์ทั้ง 230/400V และ 400/690V ครบ (M05 decision logic)
- [ ] KUKA safety fence / light curtain / E-stop ภายนอก ติดตั้งครบตาม ISO 10218
- [ ] EMD หรือ dial gauge สำหรับ mastering (M09)
- [ ] PPE + LOTO ครบ 1 ชุด/คน — **ตรวจสภาพถุงมือฉนวน (air/visual test)**
- [ ] Megger + two-pole tester พร้อม (live-dead-live, insulation test รับงาน M12)
- [ ] Pick & Place rig ทดสอบ run จบ cycle ได้จริงก่อนเปิดสอน
- [ ] datasheet/manual ฉบับจริง (FX5U, GOT, MR-JE, FR, KUKA KSS, contactor/overload/timer/sensor) พิมพ์/PDF ครบ
- [ ] Simulator (GX/GT/FluidSIM/MR Config) ติดตั้งและทดสอบบนทุก PC แล้ว
- [ ] วัสดุสิ้นเปลือง (สาย/ferrule/หางปลา/สายลม/RJ45) เผื่อ 20–30%
- [ ] CPR manikin + AED trainer + ชุดปฐมพยาบาล (M00)

**เพิ่มเติมถ้าเปิดบริการออนไลน์ (Remote Lab) ตั้งแต่รุ่นแรก — ดู §จ:**
- [ ] Lab PC + กล้อง 2–3 มุม ต่อ rig ที่เปิดรีโมต (อย่างน้อย PLC+HMI + servo/P&P)
- [ ] Remote access (RustDesk/AnyDesk/RDP) + VPN + VLAN แยกโซน lab ทดสอบแล้ว
- [ ] **Safety คุมทางไกลครบ:** enclosure + remote/local E-stop + watchdog safe-state + speed limit + remote power cut
- [ ] ระบบจอง slot + แบบฟอร์ม log session พร้อม
- [ ] เน็ต upload พอ (≥30 Mbps) + UPS + เน็ตสำรอง 4G/5G ทดสอบ failover แล้ว
- [ ] ซ้อม dry-run: ผู้เรียนจำลองรีโมตเข้าเขียน PLC จริง + ดูกล้อง + กด E-stop ได้จริง

---

<a name="remote-lab"></a>
## 9. จ. โครงสร้างพื้นฐาน Remote Lab (เรียนออนไลน์คุมเครื่องจริง)

> **เป้าหมาย:** ให้ผู้เรียนออนไลน์ **จองเวลา → รีโมตเข้าเขียนโปรแกรม → download ลงเครื่องจริง → ดูผลผ่านกล้องสด** โดยปลอดภัย · ระบบนี้คือหัวใจที่ทำให้โหมด Online ในเอกสาร D §1.3 ทำงานได้จริง
> เปิดใช้กับ rig ที่เป็น "งานเขียนโปรแกรม/สั่งเดินเครื่อง" เป็นหลัก: **PLC+HMI station, servo/ballscrew, Pick&Place rig, KUKA cell, comm bench** (งานทักษะมือไม่เปิดรีโมต)

### 9.1 องค์ประกอบระบบ Remote Lab

| # | องค์ประกอบ | สเปก/รุ่นแนะนำ | [ประหยัด] เริ่มต้น | [ครบ] เต็มรูปแบบ | หมายเหตุ |
|---|---|---|---|---|---|
| 1 | **Lab PC / Gateway ต่อ rig** (รันซอฟต์แวร์จริง ให้ผู้เรียนรีโมตเข้ามาคุม) | Win10/11, i5+/16GB, Ethernet→PLC | 1 ต่อ rig ที่เปิดรีโมต (เริ่ม 1–2) | 1 ต่อทุก rig หลัก | ติดตั้ง GX Works3/GT Designer3/MR/FR/WorkVisual |
| 2 | **กล้อง IP ต่อ rig** | PoE IP cam 1080p, RTSP/WebRTC latency ต่ำ; มุมใกล้ใช้ได้ทั้ง fixed/PTZ | 2 มุม/rig (รวม + จุดเคลื่อนที่) | 3 มุม/rig (รวม + motion + จอ HMI/ไฟสถานะ) | เห็น axis/gripper/conveyor + จอ + indicator |
| 3 | **PoE switch + NVR (ถ้ามีหลายกล้อง)** | managed PoE switch | switch 8-port PoE 1 | + NVR/สตรีมเซิร์ฟเวอร์ | จ่ายไฟ+รวมสตรีมกล้อง |
| 4 | **ช่องทาง Remote Access** | Guacamole (บนเบราว์เซอร์) / AnyDesk / RustDesk / RDP-over-VPN | โซลูชันฟรี/ถูก (RustDesk self-host หรือ AnyDesk) | remote-lab platform + SSO + คิว | 1 ผู้เรียน/rig/รอบ |
| 5 | **VPN / Firewall / VLAN แยกโซน lab** | WireGuard/OpenVPN + managed switch (มีอยู่แล้วใน §2.1) | WireGuard + VLAN แยก | firewall จริง + segmentation | กัน lab ออกจากเน็ตออฟฟิศ, ปิด attack surface |
| 6 | **Safety layer คุมทางไกล** (ดู 9.2) | safety relay/PLC + watchdog + remote/local E-stop | ครบขั้นต่ำตาม 9.2 | dual-channel + safe-torque-off | **ห้ามประนีประนอม** |
| 7 | **ระบบจอง (booking/queue)** | ปฏิทินจอง slot rig | Google Calendar/ชีตจอง | ระบบจอง+auth รายบุคคล + logging | 60–90 นาที/คน/รอบ |
| 8 | **เน็ต upload + สำรอง + UPS** | fiber upload ≥30–50 Mbps + 4G/5G failover | เน็ตบ้าน/ธุรกิจ + UPS PC/เราเตอร์ | leased line + failover อัตโนมัติ | สตรีมกล้องหลายตัวกิน upload |
| 9 | **ไมโครโฟน/ลำโพงข้าง rig (optional)** | ฟังเสียงเครื่อง/คุย TA | – | 1/rig | ช่วย debug (ฟังเสียง contactor/valve) |

### 9.2 Safety layer สำหรับคุมเครื่องจริงทางไกล (บังคับ — สอดคล้อง D §1.6)

| มาตรการ | รายละเอียด |
|---|---|
| **Enclosure / กั้นพื้นที่** | rig ที่เปิดรีโมตอยู่ในกรง/รั้ว ไม่มีคนในระยะอันตราย มีเฉพาะ TA เข้าใกล้ได้ |
| **Remote E-stop + Local E-stop + HW E-stop** | ผู้เรียนกดหยุดได้บนหน้าจอ + TA หน้างานกดได้ + ปุ่ม hardware จริงที่ตู้ |
| **Software watchdog / heartbeat** | ขาดการเชื่อมต่อ/หมด heartbeat → เข้า safe-state (หยุดแกน ตัด enable) อัตโนมัติ |
| **Speed/torque limit** | servo/robot ล็อกความเร็ว-แรงบิดต่ำในโหมดรีโมต (robot คงโหมด T1 ≤250 mm/s) |
| **Remote power sequencing** | contactor สั่งตัด/จ่ายไฟได้จากระบบ; **TA ยืนยันก่อน energize** ทุกครั้ง |
| **Camera/interlock coupling** | กล้อง/สัญญาณหลุด → ระบบไม่รับคำสั่งเดินเครื่อง |
| **Session logging** | log ว่าใครสั่งอะไรเมื่อไร (โปรแกรม/คำสั่ง/เวลา) เพื่อสอบสวนเหตุ + ประเมินผล |
| **สิทธิ์รายบุคคล** | ผู้เรียนเข้าถึงเฉพาะ rig ที่จองในรอบของตน ครั้งละ 1 คน |

### 9.3 ชุด Remote Lab ขั้นต่ำเพื่อเปิดบริการออนไลน์

- **Lab PC** 1–2 เครื่องต่อ rig ที่เปิดรีโมต (เริ่มจาก PLC+HMI + servo/P&P)
- **กล้อง IP 2 มุม/rig** + PoE switch 1
- **RustDesk/AnyDesk** + **WireGuard VPN** + VLAN แยก lab
- **Safety ขั้นต่ำ:** enclosure + remote/local E-stop + watchdog safe-state + speed limit + remote power cut
- **ปฏิทินจอง** (เริ่มด้วย Google Calendar) + แบบฟอร์ม log session
- **UPS** สำหรับ PC/สวิตช์/กล้อง + เน็ตสำรอง 4G/5G
- งบประมาณเริ่มต้น ≈ **50,000–150,000 บาท** (สอดคล้อง F §5.1)

> **หลักตัดงบ Remote Lab:** เริ่มเปิดรีโมตเฉพาะ **PLC+HMI + servo/P&P** ก่อน (โมดูลโปรแกรมที่ออนไลน์ได้ 100%) → ค่อยเพิ่ม KUKA/หลาย rig ใน Phase 2 · **ห้ามตัด:** safety layer (watchdog, E-stop, enclosure, speed limit) และเน็ต/UPS สำรอง

---

<a name="mobile-kit"></a>
## 10. ฉ. ชุดอุปกรณ์เคลื่อนที่สำหรับสอนต่างจังหวัด (Mobile / On-site Kit)

> **เป้าหมาย:** ยกหลักสูตร (โดยเฉพาะทักษะมือ + โปรแกรมพื้นฐาน) ไปสอนถึงโรงงาน/สถาบันในต่างจังหวัด สิ่งที่ขนไม่ไหว (KUKA, P&P rig เต็ม) ใช้ **เชื่อมกลับมาที่ Remote Lab ของศูนย์** ผ่าน 4G/5G

### 10.1 รายการชุดเคลื่อนที่

| รายการ | ขนไปได้ | หมายเหตุ |
|---|---|---|
| PLC FX5U + GOT2000 ในกล่อง flight-case | ✅ | trainer พกพา 2–4 ชุด |
| ชุด relay/contactor/timer + แผงประกอบตู้ย่อย | ✅ | ทักษะมือ M02/M05 แบบพกพา |
| ชุด sensor kit + ชุดนิวเมติกส์ย่อย | ✅ | M03/M04 |
| Inverter FR-D720S + มอเตอร์เล็ก | ✅ | M05/M10 Modbus |
| เครื่องมือวัด (DMM/clamp/megger/two-pole) + PPE + LOTO | ✅ | **ห้ามขาด** — งานไฟต้องปลอดภัยเท่าที่ศูนย์ |
| โน้ตบุ๊กพร้อมซอฟต์แวร์ครบ (GX/GT/MR/FR/WorkVisual) | ✅ | 1/กลุ่ม |
| **Router 4G/5G + VPN กลับศูนย์** | ✅ | ให้ผู้เรียน on-site เข้า **Remote Lab** ของศูนย์เพื่อรัน servo/KUKA/P&P ที่ขนไม่ได้ |
| KUKA robot / P&P rig เต็มระบบ | ❌ (หนัก/เสี่ยง) | ใช้ WorkVisual offline + **รันจริงผ่าน Remote Lab กลับมาที่ศูนย์** |
| กล้อง + ขาตั้ง (ถ่ายบันทึก/ถ่ายทอด) | ✅ | บันทึกคลาส + ให้คนที่ศูนย์ช่วยดู |

### 10.2 โลจิสติกส์และ Checklist ก่อนออก on-site

- [ ] ล็อก **จำนวนผู้เรียน + จำนวนวัน + รายการอุปกรณ์ที่ขน** ก่อนรับงาน (ผูกกับราคาใน F §4.4)
- [ ] ตรวจไฟปลายทาง: มีไฟ 3 เฟส/ปลั๊กกราวด์/พื้นที่พอ หรือไม่ (สำรวจล่วงหน้า)
- [ ] **ประกันอุปกรณ์ระหว่างขนส่ง** + รายการนับของเข้า-ออก
- [ ] ทดสอบ Router 4G/5G + VPN เข้า Remote Lab จากจังหวัดปลายทาง (ก่อนวันสอน)
- [ ] PPE/LOTO/มิเตอร์ครบตามจำนวนผู้เรียน — มาตรฐานความปลอดภัยเท่าที่ศูนย์
- [ ] แผนสำรองถ้าเน็ตปลายทางไม่ดี: ใช้ simulator + เลื่อนงาน Remote Lab เป็นการบ้านหลังกลับ

---

### หมายเหตุการใช้งานเอกสาร
- ตัวเลขจำนวนอ้างอิงคลาส **12 คน / 6 กลุ่ม** — ปรับสัดส่วนตามขนาดรุ่นจริง (rule of thumb: hardware rig = 1/กลุ่ม, เครื่องมือวัด = 1/กลุ่ม, PPE/multimeter หลัก = 1/คน)
- รุ่น Mitsubishi/KUKA ที่ระบุเป็นรุ่นแนะนำ ณ ปัจจุบัน — ตรวจสอบ availability และรุ่นทดแทน (เช่น FX5U → iQ-F ใหม่, MR-JE → MR-J5) กับตัวแทนจำหน่ายก่อนสั่งซื้อ
- งบก้อนใหญ่เรียงลำดับ: **KUKA cell > Pick & Place rig > Servo sets > PLC/HMI/Inverter > เครื่องมือวัด** — วางแผน CAPEX ตามนี้