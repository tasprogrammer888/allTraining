# [M13] (Elective ขั้นสูง) Industry 4.0, SCADA และ IIoT
### (Advanced Elective) Industry 4.0, SCADA & IIoT

| | |
|---|---|
| **ระดับ** | Advanced (Elective) |
| **ระยะเวลา** | **32 ชั่วโมง** (ปรับจาก 24 — ดูหมายเหตุด้านล่าง) |
| **Prerequisite** | M10, M11 และต้องมีเครื่อง Pick & Place ที่รันได้จาก M12 |

> **หมายเหตุผู้สอน (สำคัญ):** ร่างเดิมตั้ง 24 ชม. แต่เนื้อหามี 6 หัวข้อใหญ่ + 6 Lab ที่แต่ละ Lab ต้องติดตั้งซอฟต์แวร์และ debug การเชื่อมต่อจริง ในหน้างานการ "ติดตั้ง stack + แก้ปัญหาเชื่อมต่อ" กินเวลามากกว่าตัวเนื้อหา 2-3 เท่า การยัด 24 ชม. จะทำให้ผู้เรียน "ดูครู demo" แทนที่จะ "ทำเองได้" จึงปรับเป็น **32 ชม.** เพิ่ม **Lab 0 (bring-up)** และ **troubleshooting drill** ซึ่งเป็นทักษะหน้างานที่ขาดไม่ได้ หากถูกบังคับ 24 ชม. จริง ให้ตัด cloud จริงออก (ใช้ broker local อย่างเดียว) และยุบ Lab 7 เป็นการบ้าน

---

## ปรัชญาของโมดูลนี้
โมดูลนี้คือ "การต่อยอดเครื่องจักรที่นักเรียนสร้างเองใน M12 ให้กลายเป็น digital factory" ไม่ใช่การเรียนทฤษฎี Industry 4.0 ลอย ๆ **ทุก Lab ใช้ข้อมูลจริงจากเครื่อง Pick & Place ของผู้เรียน** และเน้นหนักไปที่สิ่งที่หน้างานพังจริง คือ **การเชื่อมต่อและการ troubleshoot** ไม่ใช่แค่ happy path

จุดที่หน้างานจริงมักพลาด และโมดูลนี้แก้ให้:
1. นักเรียนเชื่อม SCADA ไม่ติด แล้วไม่รู้จะไล่จากตรงไหน → สอน **layered troubleshooting** (physical → ping → port → protocol → mapping)
2. ติดตั้ง stack ไม่สำเร็จ เสียเวลาทั้งวัน → มี **Lab 0 bring-up + smoke test** ก่อนเข้าเนื้อ
3. ทำ dashboard สวยแต่ข้อมูลไม่จริง → บังคับใช้ค่า live จาก PLC จริงทุก Lab

---

## Learning Objectives
เมื่อจบโมดูล ผู้เรียนจะสามารถ:
1. อธิบายเสาหลัก Industry 4.0, OT vs IT, Automation Pyramid และ Unified Namespace และระบุตำแหน่งเครื่องตนเองในภาพรวมได้
2. อธิบายสถาปัตยกรรม SCADA และความต่างจาก HMI ระดับเครื่องได้
3. วาง IP plan และตั้ง FX5U ให้เปิด SLMP/MC Protocol และ Modbus TCP พร้อมตรวจสอบก่อนเชื่อม
4. สร้าง tag database, trend, alarm บน SCADA ที่เชื่อม FX5U จริง
5. ตั้ง OPC UA server/client พร้อมจัดการ certificate/security
6. ออกแบบ topic และ publish ข้อมูลขึ้น MQTT เป็น JSON
7. สร้าง pipeline Node-RED + InfluxDB + Grafana แสดง KPI/OEE และ alert
8. ทำ predictive maintenance เบื้องต้น (baseline, control limit, แจ้งเตือนล่วงหน้า)
9. ออกแบบ OT cybersecurity (segmentation, firewall, IEC 62443 zone & conduit)
10. **Troubleshoot ทั้ง chain อย่างเป็นระบบ**
11. ออกแบบและนำเสนอ digital factory ของเครื่อง Pick & Place ตนเอง

---

## โครงสร้างเนื้อหา (32 ชม.)

| บท | หัวข้อ | ชม. |
|---|---|---|
| 0 | เตรียมสภาพแวดล้อม, IP plan, เครื่องมือ troubleshoot | 3 |
| 1 | แนวคิด Industry 4.0 และภาพรวม OT/IT | 3 |
| 2 | ระบบ SCADA — สถาปัตยกรรม, tag, trend, alarm | 5 |
| 3 | เชื่อมข้อมูลขึ้น cloud/IIoT — OPC UA และ MQTT | 6 |
| 4 | Data Logging และ Dashboard (Edge-to-Dashboard) | 5 |
| 5 | Predictive Maintenance เบื้องต้น | 3 |
| 6 | Cybersecurity สำหรับระบบ OT | 4 |
| 7 | บูรณาการและนำเสนอ Digital Factory (Mini-Capstone) | 3 |

---

## บทที่ 0 — Bring-up, IP Plan และเครื่องมือ Troubleshoot

นี่คือบทที่ร่างเดิมขาดไป และเป็นสาเหตุหลักที่ Lab ล้ม **ก่อนแตะ SCADA/MQTT ต้องมี network ที่เชื่อมกันได้ก่อน**

### IP Plan (ทำก่อนเสียบสายเสมอ)
ตัวอย่างตารางจอง IP (subnet `192.168.3.0/24`, gateway `192.168.3.1`):

| อุปกรณ์ | IP | Port ที่ใช้ | หมายเหตุ |
|---|---|---|---|
| FX5U PLC | 192.168.3.10 | 502 (Modbus TCP), SLMP/MC (ดู manual) | static |
| PC ผู้เรียน (SCADA/dev) | 192.168.3.20 | — | static |
| Gateway / Raspberry Pi | 192.168.3.30 | 1880 (Node-RED), 1883 (MQTT), 8086 (InfluxDB), 3000 (Grafana), 4840 (OPC UA) | static |
| Managed switch | 192.168.3.2 | 443 (mgmt) | — |

> SLMP/MC port ของ FX5U กำหนดใน External Device Configuration ของ GX Works3 — **ให้เปิด manual จริงตรวจ ไม่ต้องจำ** เพราะต่างรุ่น/เฟิร์มแวร์อาจต่างกัน นี่คือทักษะ "อ่าน datasheet" ที่ต้องฝึก

### Smoke test checklist (Lab 0)
```text
[ ] ping 192.168.3.10  → ตอบกลับ (network layer ผ่าน)
[ ] Test-NetConnection 192.168.3.10 -Port 502   → TcpTestSucceeded: True
[ ] Test-NetConnection 192.168.3.30 -Port 1883  → MQTT broker เปิด
[ ] เปิด http://192.168.3.30:1880  → Node-RED ขึ้น
[ ] เปิด http://192.168.3.30:3000  → Grafana login ขึ้น
[ ] เปิด http://192.168.3.30:8086  → InfluxDB ขึ้น
```
PowerShell บน Windows ใช้ `Test-NetConnection <ip> -Port <port>` แทน telnet ได้

### กฎ Change Management (พูดเรื่องนี้ตั้งแต่ต้น)
- การแก้ Ethernet parameter ของ FX5U ต้อง **STOP แล้ว Write to PLC แล้วรีสตาร์ท** → **ไลน์หยุด**
- ห้ามทำกับเครื่องที่กำลังผลิตโดยไม่มี backup โปรเจกต์ + แผนถอยกลับ
- Backup โปรเจกต์ GX Works3 ก่อนแก้เสมอ

---

## บทที่ 1 — Industry 4.0 และ OT/IT

### Automation Pyramid → เครื่องของเราอยู่ตรงไหน
```text
ERP        (วางแผนทั้งองค์กร)            IT
MES        (จัดการการผลิต)               IT/OT
SCADA      (ภาพรวมโรงงาน) ← M13 อยู่ตรงนี้  OT
Control    (PLC FX5U)     ← เครื่อง M12     OT
Field      (sensor/servo/inverter)         OT
```

### OT vs IT (ตารางที่ต้องเข้าใจให้ขึ้นใจ)
| | OT (เครื่องจักร) | IT (สำนักงาน) |
|---|---|---|
| เป้าหมายหลัก | **Availability/ความปลอดภัย** | Confidentiality/ข้อมูล |
| Uptime | 24/7 หยุดไม่ได้ | patch/reboot ได้ |
| อายุอุปกรณ์ | 15-20 ปี | 3-5 ปี |
| Patch | ยาก/เสี่ยงหยุดไลน์ | ทำประจำ |

> ประเด็นหน้างาน: ทีม IT มัก patch/scan network แล้วทำ PLC หลุด การ converge ต้องคุยกฎร่วมกัน ไม่ใช่ IT คุมฝ่ายเดียว

**Unified Namespace (UNS):** แทนที่จะต่อ point-to-point (N×N การเชื่อม) ให้ทุกระบบ pub/sub ผ่าน broker กลางด้วย topic ที่มีโครงสร้าง → scale ง่าย เป็นที่มาของการออกแบบ topic ในบทที่ 3

---

## บทที่ 2 — SCADA

### องค์ประกอบ
```text
FX5U ──(SLMP/Modbus TCP)── [Comm Driver] ── [SCADA Server: tag engine] ──┬── HMI/Client
                                                                          └── Historian (DB)
```

### HMI (GOT) vs SCADA
| | HMI/GOT | SCADA |
|---|---|---|
| ขอบเขต | 1 เครื่อง | ทั้งไลน์/โรงงาน |
| จำนวน tag | ร้อย | พัน-หมื่น |
| Client | จอเดียว | หลาย client |
| Historian | จำกัด | เต็มรูปแบบ |

### Tag naming convention (ตัวอย่าง)
```text
<Area>_<Line>_<Asset>_<Signal>
L1_PnP_Servo1_ActPos        (REAL, mm)
L1_PnP_Cyc_Counter          (DINT)
L1_PnP_Status_Running       (BOOL)
L1_PnP_Vac_Pressure         (REAL, bar)
```

### Address mapping FX5U → tag
| FX5U device | ความหมาย | Modbus addr (ตัวอย่าง) | SCADA tag |
|---|---|---|---|
| D100 | cycle counter | 4x register | L1_PnP_Cyc_Counter |
| D200 | servo position | 4x register | L1_PnP_Servo1_ActPos |
| M50 | running flag | 0x coil | L1_PnP_Status_Running |

> Modbus address ของ FX5U ขึ้นกับการ map ใน Modbus TCP slave parameter — **ดู manual จริง** การ map D/M ↔ register/coil คือจุดที่ผิดบ่อย

### Alarm ที่ดีต้องมี deadband
```text
HighAlarm: Vac_Pressure > 6.0 bar, deadband 0.2
→ เคลียร์เมื่อ < 5.8 bar (กัน chattering รอบ 6.0)
Priority: Critical / High / Warning
ต้อง acknowledge ได้ + เก็บ alarm log
```

---

## บทที่ 3 — OPC UA และ MQTT

### OPC UA — ทำไม และจุดที่พังบ่อย
- **information model + address space:** browse เห็นโครงสร้างได้ ไม่ใช่แค่ register ดิบ
- **security:** endpoint + security policy (None ↔ Basic256Sha256)
- **จุดพังอันดับ 1 (เน้นใน Lab 2):** client เชื่อมไม่ได้เพราะ **certificate ยังไม่ trust** ต้อง trust ทั้งฝั่ง server และ client

### MQTT — โครงสร้างที่ scale ได้
**Topic hierarchy (สอดคล้อง UNS):**
```text
factory/bkk/line1/pickplace/cycle/count
factory/bkk/line1/pickplace/servo1/position
factory/bkk/line1/pickplace/status        (retained + LWT)
```

**Payload JSON (ตัวอย่าง):**
```json
{
  "ts": "2026-06-30T10:15:23+07:00",
  "asset": "L1_PnP",
  "cycle_count": 18452,
  "servo1_pos_mm": 124.7,
  "vac_pressure_bar": 5.9,
  "running": true
}
```

**ค่าที่ต้องตั้งให้ถูก:**
| ค่า | ใช้เมื่อ |
|---|---|
| QoS 0 | ค่า telemetry ถี่ ๆ หายได้ |
| QoS 1 | event/alarm สำคัญ |
| retained | ค่าล่าสุด/status ให้ client ใหม่เห็นทันที |
| LWT | ให้ broker ประกาศ `offline` เมื่อ publisher หลุด |

> **Sparkplug B (เบื้องต้น):** มี birth/death message ทำให้รู้สถานะ asset อัตโนมัติ เหมาะกับ IIoT จริง สอนแนวคิด ไม่บังคับ implement

---

## บทที่ 4 — Data Logging + Dashboard

### Pipeline
```text
FX5U ─MQTT─→ Node-RED ─→ InfluxDB ─→ Grafana
              (parse)     (time-series) (dashboard)
```

### Node-RED function node (parse → influx)
```javascript
// flow: mqtt in → function → influxdb out
const d = JSON.parse(msg.payload);   // payload จาก MQTT
msg.payload = [
  {                                   // fields
    cycle: d.cycle_count,
    pos:   d.servo1_pos_mm,
    vac:   d.vac_pressure_bar
  },
  {                                   // tags
    asset: d.asset,
    line:  "line1"
  }
];
return msg;
```
> เคสพังบ่อย: payload ไม่ใช่ JSON ที่ถูก → `JSON.parse` พัง ให้คั่นด้วย **debug node** ดู payload ก่อนเสมอ

### OEE — สูตรและตัวอย่างคำนวณ
```text
OEE = Availability × Performance × Quality

Availability = Run time / Planned time
Performance  = (Ideal cycle × Total count) / Run time
Quality      = Good count / Total count
```
**ตัวอย่าง:** กะ 8 ชม. = 480 นาที, downtime 60 นาที, ideal cycle 5 วินาที/ชิ้น, ผลิต 4,200 ชิ้น, เสีย 100 ชิ้น
```text
Availability = 420/480              = 87.5%
Performance  = (5s × 4200)/(420×60s) = 21000/25200 = 83.3%
Quality      = 4100/4200            = 97.6%
OEE          = 0.875 × 0.833 × 0.976 ≈ 71.1%
```

### Alert (LINE/Telegram/email)
- ตั้ง threshold + **de-dup** (ส่งครั้งเดียวต่อ event, ไม่ส่งทุกวินาที)
- ใส่ context ในข้อความ: asset, ค่า, เวลา

---

## บทที่ 5 — Predictive Maintenance

### 3 ระดับ
| แบบ | กลยุทธ์ | ข้อเสีย |
|---|---|---|
| Reactive | เสียแล้วซ่อม | downtime สูง |
| Preventive | ซ่อมตามเวลา | ซ่อมเกินจำเป็น |
| Predictive | ซ่อมตามอาการจริง | ต้องมีข้อมูล/เซนเซอร์ |

### สัญญาณ PdM ของ Pick & Place
- **motor current** (clamp meter หรือ inverter monitor) — bearing สึก/โหลดเพิ่ม → current สูงขึ้น
- **cycle time** — ช้าลงเรื่อย ๆ = อาการเตือน
- **servo torque/load** จาก MR-J5 (อ่านผ่าน comms)
- **vacuum pressure** — ตก = หัวดูดรั่ว

### Control limit อย่างง่าย
```text
เก็บ baseline ตอนปกติ → คำนวณ mean (μ) และ std (σ)
UCL = μ + 3σ,  LCL = μ − 3σ
moving average กรอง noise
แจ้งเตือนเมื่อค่า > UCL ต่อเนื่อง N รอบ (กัน false alarm จุดเดียว)
```

> **ความปลอดภัยไฟฟ้า (Lab 5):** การวัด motor current ทำกับวงจร 3 เฟส **มีไฟ** — clamp รอบสายเดียวเท่านั้น, สวม PPE, ถ้าต้องต่อสายให้ **lockout/tagout** ก่อน อย่าถือว่า IIoT = งานเบา

---

## บทที่ 6 — OT Cybersecurity

### ทำไม OT เปราะ
legacy PLC/HMI patch ไม่ได้, protocol เก่าไม่มี auth, เครื่องอยู่ 20 ปี เคส Stuxnet / ransomware หยุดไลน์จริง

### Purdue / IEC 62443 zone & conduit
```text
Level 4-5  Enterprise (ERP)        ── IT zone
         ┌─── DMZ (historian, patch server) ───┐  ← conduit ควบคุม
Level 3   SCADA/MES                ── OT zone
Level 2   HMI/SCADA control
Level 1   PLC (FX5U)
Level 0   Sensor/Actuator
```
กฎ: ห้าม office ต่อตรงถึง PLC ต้องผ่าน DMZ/firewall

### Hardening checklist (Lab 6)
```text
[ ] เปลี่ยน default password ทุกอุปกรณ์ (PLC/HMI/Gateway/broker/Grafana)
[ ] แยก VLAN: OT แยกจาก office
[ ] Firewall rule: allow เฉพาะ port ที่จำเป็น (502/4840/1883...) deny ที่เหลือ
[ ] MQTT broker เปิด auth (ห้าม anonymous)
[ ] OPC UA ใช้ secure endpoint
[ ] Remote access ผ่าน VPN/jump host เท่านั้น
[ ] Dashboard/cloud = read-only ห้ามสั่ง actuator ตรง
[ ] Backup โปรเจกต์ PLC/SCADA + เก็บ offline
```

---

## บทที่ 7 — Mini-Capstone

รวมทุก Lab เป็นระบบเดียวของเครื่องตนเอง + end-to-end troubleshooting test (ผู้สอนถอด 1 จุด ผู้เรียนต้องหาเจอ) + นำเสนอ 10 นาที ส่งเอกสารสถาปัตยกรรม (diagram + IP plan + tag/topic spec + security checklist)

---

## Hands-on Labs (สรุป)

| Lab | ชื่อ | Deliverable หลัก |
|---|---|---|
| 0 | Bring-up + IP plan + smoke test | IP plan + smoke test checklist ผ่านครบ |
| 1 | SCADA ↔ FX5U: tag/trend/alarm | โปรเจกต์ SCADA live ≥8 tags + trend + alarm 3 ตัว |
| 1B | **Troubleshooting drill** | ฟอร์มวินิจฉัย 3 เคส พร้อมหลักฐาน |
| 2 | OPC UA server + แก้ certificate | secure endpoint อ่าน/เขียน D/M ได้ |
| 3 | Publish → MQTT (มี auth) | flow publish JSON + LWT + topic spec |
| 4 | Node-RED + InfluxDB + Grafana | dashboard ≥4 panel + OEE + alert จริง |
| 5 | PdM baseline + แจ้งเตือนล่วงหน้า | dashboard PdM + log จับ anomaly + รายงาน |
| 6 | Hardening เครือข่าย OT | network diagram + firewall rule + หลักฐาน block |
| 7 | Mini-Capstone ครบ chain | ระบบเดิน end-to-end + เอกสาร + นำเสนอ |

### Lab 1B — Troubleshooting drill (ตัวอย่าง fault ที่ผู้สอนตั้ง)
| Fault ที่ซ่อน | อาการที่ผู้เรียนเห็น | จุดที่ควรไล่เจอ |
|---|---|---|
| ปิด SLMP | ping ผ่าน แต่ SCADA timeout | port test 502/SLMP fail → enable + write PLC |
| subnet ผิด | ping ไม่ผ่าน | ipconfig เทียบ subnet |
| IP conflict | หลุด ๆ ติด ๆ | arp/Wireshark เห็น MAC ซ้ำ |
| ลืม Write to PLC | parameter ถูกแต่ไม่เชื่อม | ตรวจว่า write + restart แล้วหรือยัง |

แบบฟอร์ม: **อาการ → สมมติฐาน → วิธีตรวจ → สาเหตุจริง → วิธีแก้**

---

## Common Mistakes & Safety (สรุป)
- IP conflict / subnet ผิด → ทำ IP plan + ping/port test ก่อน
- ลืม enable SLMP/Modbus + Write to PLC + restart
- ping ผ่าน ≠ ใช้งานได้ → ต้อง test port
- OPC UA cert ไม่ trust สองฝั่ง (เสียเวลาที่สุด)
- Mosquitto เปิด anonymous = ช่องโหว่ร้ายแรง
- scan/log rate ถี่เกิน → DB/network ตัน
- alarm ไม่มี deadband → flood
- JSON ผิด format → Node-RED parse พัง
- **ไฟฟ้า:** วัด current วงจรมีไฟ → clamp สายเดียว + PPE; ต่อสาย → lockout/tagout
- **Change mgmt:** ห้ามแก้ network/PLC ไลน์ที่รันโดยไม่มี backup + แผนถอย
- **Safety logic:** IIoT/cloud อ่านอย่างเดียว ห้ามสั่ง actuator โดยไม่มี interlock
- แยก OT จาก office/internet ตั้งแต่ออกแบบ (กัน ransomware)

---

## ความต่อเนื่องกับโมดูลอื่น (ตรวจแล้ว)
- **รับจาก M10:** ใช้ Ethernet/Modbus/comms เป็นฐาน M13 ไม่สอนซ้ำพื้นฐาน comms แต่ต่อยอดเป็น SLMP/SCADA/OPC UA/MQTT — **ไม่ซ้ำซ้อน**
- **รับจาก M11:** ใช้ data structure/tag/label ที่เรียนมา map เป็น SCADA tag/MQTT payload
- **รับจาก M12:** ใช้เครื่อง Pick & Place เป็น data source จริงทุก Lab — **ปิดช่องว่าง "ข้อมูลไม่จริง"**
- **รับจาก M07:** เทียบ HMI/GOT (ระดับเครื่อง) กับ SCADA (ระดับโรงงาน) ชัดเจน ไม่สับสนบทบาท
- **รับจาก M08:** ดึงค่าสุขภาพจาก servo MR-J5 / inverter FR-E800 ใน PdM
- **ช่องว่างที่อุดเพิ่ม:** Lab 0 (bring-up) และ Lab 1B (troubleshooting) ที่ทั้งหลักสูตรยังไม่มี และเป็นทักษะหน้างานสำคัญที่สุดของงาน integration