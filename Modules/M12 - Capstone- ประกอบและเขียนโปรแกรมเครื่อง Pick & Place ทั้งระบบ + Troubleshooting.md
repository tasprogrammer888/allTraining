# [M12] Capstone: ประกอบและเขียนโปรแกรมเครื่อง Pick & Place ทั้งระบบ + Troubleshooting
**Full Pick & Place Machine Build, Programming & Troubleshooting**

| | |
|---|---|
| **Level** | Advanced (capstone ปิดหลักสูตร) |
| **Duration** | 40 ชั่วโมง (แนะนำจัดเป็น block ต่อเนื่อง 1 สัปดาห์เต็ม หรือ 5 วัน) |
| **Prerequisite หลัก** | M11 (ผ่านแล้ว) + M06–M10 + พื้นฐาน M00–M05 + ใบรับรองความปลอดภัย |

> **ปรัชญาของโมดูล:** นี่ไม่ใช่ "เรียนเรื่องใหม่" แต่คือการ *เอาทุกอย่างที่เรียนมาทั้งหลักสูตรมาประกอบเป็นเครื่องจักรจริงหนึ่งเครื่อง* แล้วทำให้มันเดินได้ พังเป็น แก้เป็น และส่งมอบเป็นเอกสารได้ — เหมือนงาน commissioning หน้างานจริง

---

## 1. ความต่อเนื่องกับโมดูลอื่น (ป้องกันการสอนซ้ำ/ช่องว่าง)

| มาจาก | สิ่งที่ต้องมีติดตัวเข้า M12 | M12 ทำอะไรเพิ่ม (ไม่ซ้ำ) |
|---|---|---|
| **M11** Advanced Programming & Integration | structured program, FB/FC, integration บน **bench** | เอา logic ลง **เครื่องจริง** + ผูกกับ I/O/ field device จริง |
| **M08** Servo/Inverter | ต่อ + tune servo บนชุดเดี่ยว | commission servo **ในระบบรวม** + electronic gear จาก ballscrew จริง |
| **M09** KUKA | jog/teach/KRL พื้นฐาน | handshake robot↔PLC + interlock E-stop ในไลน์จริง |
| **M10** Communication | config link, map register | verify data integrity ของทั้ง 4 อุปกรณ์พร้อมกัน |
| **M01/M02** Wiring/Relay | เดินสาย, safety circuit | machine wiring **ทั้งระบบ** + safety relay ตาม PLr |

> **เส้นแบ่งสำคัญ:** M11 = ทำให้ logic ถูกบน bench • **M12 = ทำให้ของจริงเดิน + พังเป็น + แก้เป็น + ส่งมอบเป็น**
> ต่อไปยัง **M13**: นำเครื่องนี้ขึ้น SCADA/IIoT

---

## 2. Learning Objectives (สรุป)

จบโมดูลผู้เรียนต้อง: ออกแบบ system architecture + safety concept (พร้อม PLr) → เลือกอุปกรณ์จาก datasheet → wire ทั้งระบบผ่าน megger → first power-up อย่างปลอดภัย → commission servo/inverter/gripper/sensor → เขียน PLC/HMI/KRL/comm → เดิน auto cycle จริง → validate safety → troubleshoot อย่างเป็นระบบ → ทำ PM/spare → ส่งมอบเอกสาร → defend โครงงาน

---

## 3. โครงสร้างเวลา 40 ชั่วโมง (พร้อม buffer)

| บท | หัวข้อ | ชม. |
|---|---|---|
| 1 | System Design & Planning (+ Design Review #1 gate) | 5 |
| 2 | Panel Building & Machine Wiring (+ datasheet selection) | 9 |
| 3 | First Power-Up & Field Device Setup | 5 |
| 4 | Integrated Programming (PLC/HMI/Robot/Comm) | 8 |
| 5 | System Commissioning & Tuning (+ Safety Validation gate) | 4 |
| 6 | Troubleshooting & Maintenance Methodology (+ fault drill) | 5 |
| 7 | Documentation, Handover & Project Defense | 4 |
| | **รวม** | **40** |

> **หมายเหตุจากหน้างาน:** เวลา commissioning จริงมักบานปลาย ครูควรเผื่อ buffer ในบท 4–5 และยอมตัด "ความสวยงาม" ของ HMI ก่อนถ้าจำเป็น แต่ **ห้ามตัด pre-power-on test และ safety validation** เด็ดขาด

---

## 4. รายละเอียดแต่ละบท

### บทที่ 1 — System Design & Planning (5 ชม.)
- เข้าใจ scope: `conveyor (inverter) → หยิบด้วย servo axis/robot + gripper → วางลงถาด`
- เขียน **Sequence of Operation (SOO)** เป็นภาษาคน ทีละ step รวม manual + recovery
- ทำ **I/O list** + tag naming convention (ใช้ร่วมทั้ง PLC/HMI/robot)
- **Network topology** และ **Safety concept** (กำหนด stop category + required PLr → เลือก safety relay/light curtain ของจริง)
- **Design Review #1 (gate)** — ไม่ผ่านห้ามเบิกของ

**ตัวอย่าง I/O list (ย่อ):**

| Tag | Address | Type | อุปกรณ์ | หมายเหตุ |
|---|---|---|---|---|
| `X0` | X0 | DI | Part-present sensor | PNP, NO |
| `X1` | X1 | DI | Pick position sensor | |
| `X2` | X2 | DI | E-stop status (จาก safety relay) | monitor only |
| `Y10` | Y10 | DO | Gripper close solenoid | |
| `Y11` | Y11 | DO | Conveyor run (ถ้าไม่ใช้ comm) | |
| `M_ReqPick` | M100 | comm | Request pick → robot | handshake |
| `M_PickDone` | M101 | comm | Pick done ← robot | handshake |

---

### บทที่ 2 — Panel Building & Machine Wiring (9 ชม.)

- **ตรวจนับอุปกรณ์ตาม BOQ (Bill of Quantities)** ก่อนเบิก/เริ่มประกอบ — เทียบของจริงกับรายการในแบบทีละชิ้น (รุ่น/สเปก/จำนวน) บันทึกส่วนขาด/ผิดสเปกก่อนลงมือ **ห้ามเริ่มประกอบถ้าของไม่ครบ** (ทักษะหน้างานจริงที่แยก "ประกอบเป็น" ออกจาก "ทำโปรเจกต์ได้จริง" — ของขาด/ผิดรุ่นกลางงานคือสาเหตุเสียเวลาอันดับต้น ๆ ของงาน SI จริง)
- อ่าน/แก้แบบ + **หา error ในแบบที่ครูแกล้งใส่มา**
- **อ่าน datasheet จริง** (MR-J4/J5, FR, safety relay) → เลือก breaker/สาย/fuse + torque
- Layout (EMC zoning, ระยะระบายความร้อน), wiring power/control/comm, ferrule + label สองปลาย
- ต่อ field device, grounding/shielding สาย encoder (single-point)
- **Pre-power-on inspection** → checklist + megger

**ตัวอย่าง Pre-Power-On Checklist (บางส่วน):**

- [ ] Continuity: ทุก terminal ตรง schematic
- [ ] Insulation (megger 500VDC): control circuit **≥ 1 MΩ** → ค่าจริง: ______ MΩ
- [ ] **ปลด PLC/servo/inverter ออกก่อน megger** (กันพัง)
- [ ] Polarity 24V (+/–) ถูกต้องทุกจุด
- [ ] Ground continuity จาก enclosure → main earth: ______ Ω
- [ ] Servo U/V/W และ encoder A/B ไม่สลับ
- [ ] ครูตรวจและลงนามอนุมัติจ่ายไฟ: __________

> **กับดักหน้างาน:** ลืมปลดอุปกรณ์อิเล็กทรอนิกส์ก่อน megger = เผา PLC/servo ทันที เป็นความผิดพลาดที่แพงที่สุดและพบบ่อยที่สุดของช่างใหม่

---

### บทที่ 3 — First Power-Up & Field Device Setup (5 ชม.)

- **First power-up ทีละ section** ภายใต้ LOTO: 24V control ก่อน → วัดทุกจุด → power circuit เมื่อผ่าน
- Servo commissioning: **electronic gear** จากสูตร

```
Electronic gear = (encoder resolution × gear ratio) / (ballscrew lead / command unit)
เช่น encoder 4,194,304 pulse/rev, lead 10 mm, command unit 0.001 mm/pulse
→ 1 rev = 10 mm = 10,000 command pulse
→ CMX/CDV = 4,194,304 / 10,000  (ลดทอนเศษส่วนตาม parameter MR-J)
```

- JOG test → home/origin return → ตั้ง software limit
- Inverter parameter + run ผ่าน Modbus/CC-Link
- Gripper (regulator + flow control), sensor calibration
- **I/O verification point-to-point ด้วย force I/O** เทียบ I/O list (ปิด wiring error ก่อนเขียนโปรแกรม)

---

### บทที่ 4 — Integrated Programming (8 ชม.)

**โครงสร้าง PLC แบ่ง block:** `init / mode / auto / alarm / comm`

**ตัวอย่าง step sequence (auto cycle) — pseudo-ladder/SFC:**

```
Step 10  Home all axis        → DONE & no_alarm     → Step 20
Step 20  Conveyor run         → Part_present (X0)    → Step 30
Step 30  Axis move to PICK    → Pos_done            → Step 40
Step 40  Gripper close (Y10)  → Grip_confirm + T#0.5s→ Step 50
Step 50  Req pick → robot     → Pick_done (M101)    → Step 60
Step 60  Axis move to PLACE   → Pos_done            → Step 70
Step 70  Gripper open         → T#0.3s              → Step 10 (cycle++)

ทุก step มี: STEP_TIMER → ถ้าเกิน step_timeout → set ALARM + latch + first-fault capture
```

**ตัวอย่าง ladder (first-fault capture):**

```
| Step40_active  Grip_confirm  STEP_TIMER>T#2s |          (ALM_Grip) |
|------]/[----------]/[--------------] [--------|----------( )------|
|                                                                    |
| (ALM_Grip) (ALM_FirstFault_unset)                                 |
|----] [----------] [-----------------------------------(SET D_FirstFault = K40)|
```

**ตัวอย่าง KRL (robot handshake):**

```krl
DEF pickplace()
  $BASE = BASE_DATA[1]
  $TOOL = TOOL_DATA[1]
  PTP HOME
  LOOP
     WAIT FOR $IN[10]        ; M_ReqPick จาก PLC
     PTP P_pick_app          ; approach
     LIN P_pick
     $OUT[20] = TRUE         ; gripper (หรือให้ PLC คุม)
     WAIT SEC 0.3
     LIN P_pick_app
     $OUT[11] = TRUE         ; M_PickDone → PLC
     WAIT FOR $IN[11]        ; ack
     $OUT[11] = FALSE
  ENDLOOP
END
```

**Communication:** config Modbus TCP/Ethernet/CC-Link → **verify data integrity** (write register แล้ว read-back ให้ตรง)

---

### บทที่ 5 — System Commissioning & Tuning (4 ชม.)

ลำดับบังคับ: **`I/O check → single device → dry run → step mode → auto cycle`**

- Tune servo gain + acc/dec (ดู overshoot/vibration จาก amplifier monitor)
- จูน handshake timing ลด cycle time โดยไม่ชน
- **Safety Validation (gate):** ทดสอบ E-stop ทุกตัว, guard, light curtain → บันทึก record
- Continuous run + เก็บ cycle time / jam rate

> **กฎเหล็กหน้างาน:** ห้ามกระโดดข้าม dry run → auto โดยตรง การชนของ axis/robot 90% มาจากการข้ามขั้นนี้

---

### บทที่ 6 — Troubleshooting & Maintenance (5 ชม.)

**ระเบียบวิธี:** `symptom → สังเกต/วัด → สมมติฐาน → ทดสอบ → root cause` (5-Why + fault tree)
**แยกชั้น:** `power → wiring → field device → communication → program`

**Diagnostic จริงที่ต้องใช้เป็น:**

| อุปกรณ์ | เครื่องมือ | ตัวอย่าง code |
|---|---|---|
| PLC | GX Works3 monitor / error code / device monitor | module error, watchdog |
| Servo | MELSERVO alarm code | AL.16 (encoder), AL.32 (overcurrent), AL.50 (overload) |
| Robot | KUKA message log | handshake timeout, drives off |

**Fault-injection drill (2 รอบ):** ครู inject เช่น ถอด sensor / ตั้ง gear ผิด / ดึงสาย comm / สลับ encoder phase / **เดินสาย signal ขนานแนบสาย power (EMC)** → ผู้เรียนหา root cause + แก้ในเวลา + เขียน fault report

> **เคส EMC พิเศษกว่าเคสอื่น:** อาการ (ตำแหน่งเพี้ยนแบบสุ่ม/comm timeout เป็นพัก ๆ) ไม่ใช่ wiring ผิดแบบ continuity เจอด้วย DMM — ผู้เรียนต้องนึกถึง layout/EMC (ที่เรียนใน M08) เป็นสมมติฐาน ไม่ใช่ไล่แค่ระดับ electrical fault ตรง ๆ เป็นแบบฝึกแยก "fault ทางไฟฟ้า" ออกจาก "fault ทาง layout/สัญญาณรบกวน" ซึ่งมือใหม่มักข้ามไปเพราะเครื่องมือวัดปกติไม่ฟ้อง

**PM plan & spare list:** จุดตรวจ + ความถี่ (daily/weekly/monthly) + critical spare (servo, sensor, valve, fuse, comm module) + lead time

---

### บทที่ 7 — Documentation, Handover & Project Defense (4 ชม.)

- **As-built** (red-line → as-built), **O&M manual**, **alarm list + วิธีแก้**
- **Backup** PLC/GOT/robot/servo/inverter พร้อม **version**
- **FAT/SAT แบบย่อ** run ต่อหน้ากรรมการ
- นำเสนอ design decision + ปัญหาที่แก้ + สาธิต → **oral defense**

---

## 5. Hands-On Labs (11 labs)

| Lab | ชื่อ | Deliverable หลัก |
|---|---|---|
| 12.1 | Design Package | SOO + I/O list + topology + safety (PLr) ผ่าน DR#1 |
| 12.2 | Datasheet → เลือก breaker/สาย/fuse | component selection sheet (อ้างหน้า datasheet) |
| 12.3 | BOQ check + ประกอบตู้ + machine wiring | BOQ verification sheet (ครบ/ขาด/ผิดสเปก) + ตู้/wiring + label ครบ ผ่าน workmanship |
| 12.4 | Pre-Power-On + First Power-Up | checklist + ค่า megger + ครูเซ็น (**gate**) |
| 12.5 | Commission servo/inverter/gripper/sensor | commissioning sheet + I/O verification |
| 12.6 | PLC Auto Cycle (state machine) | PLC project + first-fault capture ผ่าน review |
| 12.7 | HMI + KRL + Communication | HMI ครบหน้า + robot handshake + register map verify |
| 12.8 | System Commissioning + Safety Validation | auto cycle จริง + Safety Validation Record (**gate**) |
| 12.9 | Fault-Injection Drill | fault report ต่อเคส (root cause + เวลา) |
| 12.10 | PM/Spare + Handover package | as-built/O&M/PM/spare/backup |
| 12.11 | FAT/SAT + Project Defense | acceptance result + นำเสนอ + defense |

---

## 6. การประเมิน (gate-based)

| รายการ | น้ำหนัก |
|---|---|
| Design Review #1 | 10% |
| Datasheet component selection | 5% |
| Workmanship ตู้/wiring/label | 15% |
| **Pre-power-on + megger + อนุมัติ** | **gate บังคับ** |
| Commissioning + I/O verification | 10% |
| PLC/HMI/Robot/Comm + code review | 20% |
| **Auto cycle + Safety Validation** | **gate บังคับ** (15%) |
| Fault-injection drill | 10% |
| Handover package | 5% |
| FAT/SAT + Defense | 10% |

> Gate บังคับ (pre-power-on, safety validation) **ไม่ผ่าน = หยุด ไปต่อไม่ได้** — สะท้อนวินัยความปลอดภัยหน้างานจริง

---

## 7. ข้อผิดพลาดที่พบบ่อย & ความปลอดภัย (ครูต้องย้ำ)

1. **ห้ามจ่ายไฟก่อนผ่าน checklist + ครูเซ็น** (gate)
2. **LOTO ทุกครั้ง** ก่อนแก้ wiring / เข้าส่วน live
3. **megger ต้องปลด PLC/servo/inverter ก่อน** — ไม่งั้นเผาบอร์ด
4. สาย encoder **shield + single-point ground** — กัน ground loop → servo drift/alarm
5. **อย่าสลับ U/V/W หรือ A/B encoder** — runaway/alarm พบบ่อย
6. **E-stop ต้อง hardwired ตาม PLr** — ผ่าน PLC อย่างเดียวไม่ปลอดภัย
7. robot handshake ต้อง **interlock E-stop + timeout ทุก step**
8. ตั้ง **software limit + home ก่อน abs move** — กัน axis ชนปลายราง
9. **ปล่อย air + LOTO pneumatic** ก่อนเข้าใกล้ gripper
10. **ห้ามข้าม dry run → auto** — สาเหตุการชนอันดับ 1

---

## 8. อุปกรณ์ & ซอฟต์แวร์

**Software:** GX Works3 • GT Designer3 • WorkVisual + smartPAD • MR Configurator2 • FR Configurator2
**Hardware:** trainer Pick & Place (servo+ballscrew, conveyor+inverter, gripper, sensor, safety relay+E-stop+light curtain)
**เครื่องมือวัด:** multimeter • megger 500VDC • clamp meter • torque screwdriver • crimping tool + ferrule
**เอกสารอ้างอิงจริง:** MR-J4/J5 manual • FR inverter manual • FX5U/MELSEC programming manual • safety relay manual

---

## 9. สรุปสิ่งที่ปรับปรุงจากร่างเดิม

- แก้ **เลขบทซ้ำ** (เดิมมี "บทที่ 3" สองอัน) → จัดใหม่เป็น 7 บทเรียงต่อเนื่อง
- แก้ **ความขัดแย้งเวลา** (เดิมอ้าง 5 สัปดาห์แต่ 40 ชม.) → ชัดเจนว่า 1 block + เพิ่มหมายเหตุ buffer
- ยกระดับ **safety จาก "ระดับเข้าใจ" → ทำได้จริง**: กำหนด PLr, เลือก safety component, เพิ่ม **2 gate บังคับ** (pre-power-on, safety validation)
- เพิ่ม **datasheet lab (12.2)** — เลือก breaker/สาย/fuse จาก manual จริง
- เพิ่ม **fault-injection drill 2 รอบ + fault report** (เดิมมีแค่ผ่านๆ)
- เติม **ตัวอย่างจริง**: I/O table, electronic gear formula, step sequence, ladder first-fault, KRL handshake, megger criteria
- ขยาย labs จาก ~1 ที่ truncate → **11 labs ครบ มี deliverable ชัด**
- เพิ่ม **assessment แบบ gate-based** สะท้อนวินัยหน้างาน