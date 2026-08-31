# โครงงาน Capstone แบบไล่ระดับ (Progressive Capstone System)
## หลักสูตรช่างไฟฟ้า–ซ่อมบำรุงระบบอัตโนมัติ (PLC / HMI / Servo / Robot / Communication)

---

## ปรัชญาการออกแบบ: "ไต่บันไดสู่เครื่องจริง"

โครงงานทั้งหมดถูกออกแบบให้ **ทุกชิ้นเป็นชิ้นส่วนของเครื่อง Pick & Place เดียวกัน** ผู้เรียนจะค่อยๆ สร้างทักษะและชิ้นงานสะสมไปเรื่อยๆ จนถึง M12 จึงนำทุกอย่างมาประกอบเป็นเครื่องเดียวที่ทำงานได้จริง

```
M00-M02  →  Tier 1: ฐานราก (Foundation Build)        → ตู้ DOL Motor Starter
M03-M05  →  Tier 2: actuator & motor (Actuation)     → สถานี Electro-pneumatic + VFD conveyor
M06-M08  →  Tier 3: สมอง & การเคลื่อนที่ (Control)    → สถานี Servo positioning + HMI
M09-M11  →  Tier 4: บูรณาการย่อย (Integration)        → Robot cell + Comm + Sequence
M12      →  GRAND CAPSTONE: เครื่อง Pick & Place เต็มระบบ
M13      →  Stretch Tier: Digital Factory (Elective)
```

> **กฎทอง (Golden Rule):** ทุกโครงงานต้องผ่าน **Pre-Power-On Checklist + ครูเซ็นอนุมัติ** ก่อนจ่ายไฟครั้งแรกเสมอ — ไม่มีข้อยกเว้น ความปลอดภัยคือเกณฑ์ Pass/Fail ขั้นต้นที่ override คะแนนอื่นทั้งหมด

---

# TIER 1 — ฐานราก (Foundation Build)
### Mini-Project A: "ตู้ควบคุมมอเตอร์ DOL ที่ปลอดภัยและสวยงาม"
**โมดูลที่เกี่ยวข้อง:** M00 + M01 + M02 | **เวลาแนะนำ:** 12–16 ชม.

### โจทย์ / Spec
ออกแบบ ประกอบ และทดสอบตู้ควบคุมมอเตอร์ 3-phase แบบ Direct-On-Line (DOL) ที่มีระบบ start/stop พร้อม seal-in, overload protection และไฟแสดงสถานะ โดยทำเอกสารแบบครบชุดและเดินสายตามหลัก workmanship ของโรงงาน

| รายการ | Spec ที่กำหนด |
|---|---|
| มอเตอร์ | 3-phase induction 0.37 kW 400V (ใช้ของ M05/M01) |
| Control voltage | 24VDC จาก SMPS แยกจาก power circuit |
| Protection | MCCB + magnetic contactor (AC-3) + thermal overload relay class 10 |
| Operator interface | ปุ่ม Start (เขียว NO), Stop (แดง NC), E-stop (mushroom, positive opening), pilot lamp RUN/TRIP |
| Wiring standard | ferrule ทุกปลายสาย, wire numbering + cross-reference, จัดสายใน wire duct, ขัน torque ตาม spec |

### Sequence การทำงาน
1. จ่ายไฟ → MCCB ON → pilot lamp "POWER" ติด
2. กด **Start** → contactor coil (A1/A2) ทำงาน → มอเตอร์หมุน → lamp "RUN" ติด → seal-in ผ่าน aux contact NO 13/14 ค้างสถานะ
3. กด **Stop** หรือ **E-stop** → ตัด coil → มอเตอร์หยุด
4. Overload เกิน FLA → OLR trip (95/96) → ตัด control circuit → lamp "TRIP" ติด → ต้อง reset ด้วยมือ

### สิ่งที่ต้องส่ง (Deliverables)
- [ ] **เอกสาร:** Power schematic + Control ladder diagram (IEC 60617) พร้อม wire numbering ครบ
- [ ] **เอกสาร:** Panel layout + Bill of Material (BOM) + อ่าน datasheet ระบุ utilization category, coil voltage, ช่วงตั้ง OLR
- [ ] **ชิ้นงาน:** ตู้จริงที่ประกอบเสร็จ เดินสายเรียบร้อย
- [ ] **เอกสาร:** Dead-test checklist + Energization checklist (กรอกจริง)
- [ ] **เอกสาร:** ใบ LOTO + บันทึก live-dead-live verification
- [ ] **สาธิต:** การทำ LOTO และพิสูจน์ไร้พลังงานต่อหน้าครู

### Rubric (100 คะแนน)

| หมวด | เกณฑ์ | คะแนน |
|---|---|---|
| **Safety (GATE)** | LOTO + live-dead-live + PPE + pre-power check ครบ — **ผิดพลาดด้านความปลอดภัย = สอบตกทันที** | Pass/Fail |
| เอกสารแบบไฟฟ้า | schematic ถูกสัญลักษณ์ IEC, wire numbering + cross-ref ครบและตรงกับของจริง | 20 |
| Workmanship การเดินสาย | ferrule ทุกปลาย, จัดสายในราง, ความเรียบร้อย, torque ถูกต้อง, ผ่าน pull test | 25 |
| ความถูกต้องของวงจร | start/stop/seal-in/E-stop/OLR ทำงานครบทุกฟังก์ชัน | 25 |
| การเลือกอุปกรณ์ | ขนาด CB/สาย/OLR coordination กับ FLA ถูกต้อง อ่าน datasheet เป็น | 15 |
| Troubleshooting | ครูแอบใส่ fault 1 จุด → หาเจอและอธิบายวิธีคิดอย่างเป็นระบบ | 15 |

### Stretch Goal
- เพิ่ม **Forward/Reverse** พร้อม electrical + mechanical interlock
- เพิ่ม **remote/local selector switch** และ run-hour counter (timer)
- ทำ as-built drawing หลังแก้ไขจุดที่ต่างจากแบบเดิม

---

# TIER 2 — actuator และ motor (Actuation Layer)
### Mini-Project B: "สถานีคัดแยกชิ้นงาน Electro-Pneumatic + สายพาน VFD"
**โมดูลที่เกี่ยวข้อง:** M03 + M04 + M05 | **เวลาแนะนำ:** 16–20 ชม.

### โจทย์ / Spec
สร้างสถานีที่ใช้ลมอัดดันชิ้นงานและสายพานลำเลียงควบคุมความเร็วด้วย inverter โดยใช้ relay logic (ยังไม่ใช้ PLC) เพื่อให้ผู้เรียนเข้าใจ "hard logic" ก่อนเปลี่ยนเป็น "soft logic" ใน Tier 3

| รายการ | Spec |
|---|---|
| Actuator | double-acting cylinder (push/eject) ควบคุมด้วย double solenoid 5/2 |
| ความเร็ว cylinder | ปรับด้วย one-way flow control แบบ meter-out |
| Sensor | reed switch ตรวจ home/end ของกระบอก + inductive proximity ตรวจชิ้นโลหะ + photoelectric ตรวจชิ้นบนสายพาน |
| Conveyor | มอเตอร์ 3-phase + inverter FR (multi-speed 2 ระดับ: เดินช้า/เร็ว) |
| Logic | relay + timer (ยังไม่มี PLC) |

### Sequence การทำงาน
1. กด Start → conveyor เดิน (ความเร็วต่ำ via inverter multi-speed terminal)
2. photoelectric ตรวจพบชิ้นงานเข้าตำแหน่ง → conveyor หยุด (timer delay 0.5s)
3. inductive proximity ตรวจ: ถ้าเป็น **โลหะ** → cylinder ดันออก (extend) → ตรวจด้วย reed switch ปลายทาง → retract กลับ home
4. ถ้าเป็น **อโลหะ** → conveyor เดินต่อ (ความเร็วสูง) ผ่านไป
5. E-stop → ทุกอย่างหยุดทันที + cylinder อยู่ในตำแหน่งปลอดภัย

### สิ่งที่ต้องส่ง
- [ ] **เอกสาร:** Pneumatic circuit diagram (ISO 1219) ระบุ valve type, port P/A/B/R
- [ ] **เอกสาร:** Sensor wiring diagram ระบุ NPN/PNP, brown/blue/black, sink/source
- [ ] **เอกสาร:** Relay control ladder + inverter parameter list (multi-speed setup)
- [ ] **ชิ้นงาน:** สถานีที่ต่อวงจรลม + ไฟฟ้า + ทดสอบเดินได้จริง
- [ ] **เอกสาร:** บันทึกการ bench-test sensor (วัด output ด้วยมัลติมิเตอร์ก่อนต่อ)
- [ ] **สาธิต:** การ depressurize + verify zero pressure ก่อนถอดสายลม

### Rubric (100 คะแนน)

| หมวด | เกณฑ์ | คะแนน |
|---|---|---|
| **Safety (GATE)** | depressurize + hose whip prevention + electrical LOTO + E-stop ทำงาน | Pass/Fail |
| Pneumatic | วงจรลมถูกต้อง, meter-out เลือกถูก, ไม่มีรั่ว, cylinder เคลื่อนตามสั่ง | 20 |
| Sensor wiring | ต่อ NPN/PNP ถูก, bench-test ก่อนใช้, LED indicator ตรวจถูก | 20 |
| Inverter setup | multi-speed parameter ถูก, มอเตอร์เดิน 2 ความเร็วตามสั่ง, อ่าน manual เป็น | 20 |
| Logic การทำงาน | sequence คัดแยกถูกต้องครบทุก case (โลหะ/อโลหะ/E-stop) | 25 |
| Troubleshooting | แก้ fault sensor/ลม/inverter ที่ครูใส่ได้ | 15 |

### Stretch Goal
- เพิ่ม **counter** นับชิ้นงานแต่ละประเภท แสดงผ่าน digital counter relay
- ปรับ conveyor เป็น **analog speed control** ด้วย potentiometer (0–10V)
- เพิ่ม quick-exhaust valve เพื่อเพิ่มความเร็ว retract และวัด cycle time เปรียบเทียบ

---

# TIER 3 — สมองและการเคลื่อนที่แม่นยำ (Control & Motion)
### Mini-Project C: "สถานี Servo Positioning ควบคุมด้วย PLC + HMI"
**โมดูลที่เกี่ยวข้อง:** M06 + M07 + M08 | **เวลาแนะนำ:** 20–24 ชม.

### โจทย์ / Spec
แปลง relay logic ของ Tier 2 ให้เป็นโปรแกรม PLC FX5U พร้อมเพิ่มแกน servo สำหรับ positioning และหน้าจอ HMI GOT สำหรับสั่งงาน/มอนิเตอร์ นี่คือก้าวสำคัญจาก "ช่างไฟ" สู่ "ช่างออโตเมชัน"

| รายการ | Spec |
|---|---|
| PLC | Mitsubishi FX5U, โปรแกรม ladder บน GX Works3 |
| Servo | MELSERVO MR-JE-A + ballscrew slide 1 แกน |
| Positioning | Homing + 3 ตำแหน่งงาน (P1/P2/P3) ด้วย DRVA/DRVI |
| HMI | GOT2000, เชื่อม Ethernet กับ FX5U |
| Electronic gear | คำนวณ CMX/CDV ให้ 1 mm = จำนวน pulse ที่ถูกต้องตาม ballscrew lead |

### Sequence การทำงาน
1. Power on → HMI แสดงหน้า "NOT HOMED" → กดปุ่ม Home บน HMI → servo วิ่งหา near-point dog → ตั้ง home position
2. HMI แสดงสถานะ READY → operator เลือกตำแหน่งเป้าหมาย (P1/P2/P3) หรือใส่ค่าตำแหน่ง (mm) ผ่าน numerical input
3. กด Move → servo เคลื่อนไปตำแหน่งด้วย DRVA → HMI แสดงตำแหน่งจริง (current position) real-time
4. แสดง alarm บน HMI เมื่อ servo error / over-travel limit / not homed
5. E-stop (hardwired) → servo stop + STO → HMI แสดง alarm (HMI ไม่ใช่ safety device)

### สิ่งที่ต้องส่ง
- [ ] **เอกสาร:** I/O list + Device mapping table (PLC X/Y/M/D ↔ HMI object)
- [ ] **เอกสาร:** การคำนวณ electronic gear (mm/rev → pulse) แสดงวิธีทำ
- [ ] **โปรแกรม:** GX Works3 project (ladder มี comment + structured)
- [ ] **โปรแกรม:** GT Designer3 project (หน้าจอ control + status + alarm)
- [ ] **เอกสาร:** Servo wiring (CN1/CN2/CN8) + MR Configurator2 parameter list
- [ ] **สาธิต:** วัด positioning accuracy ด้วย dial gauge เทียบกับค่าสั่ง
- [ ] **สาธิต:** พิสูจน์ว่า E-stop ตัด servo ได้แม้ HMI ค้าง

### Rubric (100 คะแนน)

| หมวด | เกณฑ์ | คะแนน |
|---|---|---|
| **Safety (GATE)** | E-stop hardwired ทำงาน, STO ต่อถูก, pre-power check servo | Pass/Fail |
| PLC programming | ladder ถูก logic, มี comment/documentation, ไม่มี double-coil, scan-safe | 20 |
| Servo positioning | homing ทำงาน, accuracy ผ่านเกณฑ์ (±0.1mm), electronic gear คำนวณถูก | 25 |
| HMI design | device mapping ถูก, หน้าจอใช้งานได้จริง, status/alarm สื่อสารชัด | 20 |
| Communication | GOT–FX5U Ethernet ติดจริง, IP/connection setting ถูกทั้งสองฝั่ง | 10 |
| Integration | ทั้ง 3 ส่วนทำงานร่วมกันลื่นไหล + UX สำหรับ operator ดี | 10 |
| Troubleshooting | แก้ comm fail / servo alarm / wrong gear ratio ได้ | 15 |

### Stretch Goal
- เพิ่ม **Point Table** mode ทำ multi-position sequence อัตโนมัติ
- เพิ่ม **recipe** บน HMI เก็บชุดตำแหน่งหลาย job + data logging cycle time
- เพิ่ม trend graph แสดง position/speed real-time

---

# TIER 4 — บูรณาการย่อย (Sub-System Integration)
### Mini-Project D: "Robot Cell + เครือข่ายสื่อสาร + Step Sequence"
**โมดูลที่เกี่ยวข้อง:** M09 + M10 + M11 | **เวลาแนะนำ:** 24–28 ชม.

### โจทย์ / Spec
สร้าง robot cell ที่หุ่นยนต์ KUKA หยิบ-วางชิ้นงาน โดย PLC เป็น "ตัวกำกับวง" (master coordinator) สั่งงานหุ่นยนต์ผ่าน hardwired I/O handshake และอ่าน/เขียน inverter ผ่าน Modbus พร้อมเขียนโปรแกรม PLC แบบ state machine

| รายการ | Spec |
|---|---|
| Robot | KUKA 6-axis + KRL program (pick → place) |
| Coordination | PLC ↔ Robot ผ่าน hardwired I/O handshake (request/busy/done) |
| Communication | FX5U เป็น Modbus RTU master ควบคุม inverter FR (run/stop/set freq/read speed) |
| Program structure | PLC เขียนแบบ step sequence (SFC หรือ step register) + Function Block |
| Gripper | pneumatic gripper + sensor feedback (part present) |

### Sequence การทำงาน
1. PLC state HOME → ตรวจทุก device พร้อม (robot home, conveyor ready, gripper open)
2. PLC สั่ง conveyor (via Modbus → inverter run) → ชิ้นงานมาถึง part-present sensor → inverter stop
3. PLC ส่ง "PICK_REQUEST" (hardwired output) → robot เห็น input → robot ตอบ "BUSY"
4. Robot เคลื่อนไป pick → ปิด gripper → ตรวจ part-present → ยกขึ้น → ส่ง "PICK_DONE"
5. PLC สั่ง "PLACE_REQUEST" → robot วาง → เปิด gripper → กลับ home → "READY"
6. วน loop + นับ cycle count; alarm management ทุก step (timeout, no-part, robot fault)
7. E-stop รวม (safety relay) → หยุดทั้ง cell

### สิ่งที่ต้องส่ง
- [ ] **เอกสาร:** Sequence chart / state transition diagram ครบทุก step + transition condition
- [ ] **เอกสาร:** I/O handshake table (PLC ↔ Robot signal mapping)
- [ ] **เอกสาร:** Modbus register map (inverter FR) — 4xxxx → protocol address
- [ ] **โปรแกรม:** PLC step sequence (SFC/step register) + FB_Cylinder / FB_Gripper
- [ ] **โปรแกรม:** KRL robot program (teach points + motion + I/O handshake)
- [ ] **เอกสาร:** Alarm list + error handling design
- [ ] **สาธิต:** Robot safety (T1 mode jog, mastering, E-stop, enabling switch)

### Rubric (100 คะแนน)

| หมวด | เกณฑ์ | คะแนน |
|---|---|---|
| **Safety (GATE)** | robot T1/safety zone, enabling switch, safety relay E-stop, LOTO | Pass/Fail |
| State machine design | sequence chart ครบ, ไม่มี spaghetti logic, step reset/home ปลอดภัย | 20 |
| Modbus comm | RS-485 ต่อถูก (termination 120Ω), register map ถูก, run/stop/read สำเร็จ | 20 |
| Robot programming | teach point แม่น, motion type เหมาะสม, KRL อ่านเข้าใจ | 20 |
| Handshaking | PLC↔Robot handshake ทำงานไม่ค้าง, มี timeout protection | 15 |
| Modular code (FB) | ใช้ FB/label เป็น, reusable, ทีมอื่นอ่านได้ | 10 |
| Troubleshooting | แก้ comm timeout / handshake deadlock / robot fault ได้ | 15 |

### Stretch Goal
- เปลี่ยน inverter control จาก Modbus RTU เป็น **Modbus TCP** หรือเพิ่ม **CC-Link IE Field** remote I/O
- เพิ่ม robot vision-less sorting (วาง 2 ตำแหน่งตาม part type จาก sensor)
- เพิ่ม Wireshark capture วิเคราะห์ traffic + เอกสารส่งมอบ comm

---

# GRAND CAPSTONE (M12)
### "เครื่อง Pick & Place เต็มระบบ — Build, Program & Commission"
**เวลา:** 40 ชม. | **รูปแบบ:** กลุ่ม 2–3 คน หรือเดี่ยวตามทรัพยากร

> นี่คือการรวม **ทุก Tier** เข้าด้วยกัน: ตู้ไฟ (Tier 1) + actuator/conveyor/VFD (Tier 2) + servo/HMI (Tier 3) + robot/Modbus/sequence (Tier 4) เป็นเครื่องเดียวที่ทำงานครบวงจรและ commission ได้ตามมาตรฐาน

### โจทย์ / Spec ของเครื่อง
เครื่อง Pick & Place ที่รับชิ้นงานเข้า → คัดแยก → หุ่นยนต์/servo หยิบไปจัดเรียงตามตำแหน่ง โดยควบคุมด้วย PLC กลาง, สั่งงานผ่าน HMI, มี servo axis + conveyor inverter + robot, สื่อสารกันด้วย Modbus/Ethernet และมี safety system ตามมาตรฐาน

| ระบบย่อย | องค์ประกอบ |
|---|---|
| **Power & Control panel** | MCCB, contactor, safety relay, 24VDC, servo amp, inverter, comm cable — grounding/shielding ถูก |
| **PLC** | FX5U เป็น master, step sequence, FB modular, recipe, alarm |
| **HMI** | GOT2000 — operation screen, status, alarm history, recipe, manual/auto mode |
| **Servo** | MR-J4/J5 + axis สำหรับ positioning |
| **Inverter** | FR — conveyor, ควบคุมผ่าน Modbus |
| **Robot** | KUKA — pick & place ด้วย handshake |
| **Communication** | Modbus (inverter) + Ethernet (HMI/SLMP) + I/O handshake (robot) |
| **Safety** | E-stop + light curtain + safety relay ตาม IEC 60204-1 / ISO 13849-1 (กำหนด PLr) |

### Sequence of Operation (SOO) — ระดับเครื่อง
1. **Power-up & Init:** ตรวจ device ทั้งหมด, robot/servo home, แสดงสถานะบน HMI
2. **Mode select (HMI):** Manual (jog แต่ละ axis/device) / Auto (run cycle) / Maintenance
3. **Auto cycle:**
   - Conveyor (Modbus→inverter) ป้อนชิ้นงาน → sensor part-present → stop
   - คัดแยกประเภท (sensor) → ตัดสินใจปลายทาง
   - Servo axis เคลื่อนไปตำแหน่ง pick / robot pick (handshake)
   - วางชิ้นงานตาม recipe/ตำแหน่งที่เลือก
   - นับ production count, cycle time → แสดง + log บน HMI
4. **Alarm/Fault:** ทุก step มี timeout + alarm priority; HMI แสดง alarm history; recovery ปลอดภัย
5. **Safety:** E-stop/light curtain → controlled stop ทั้งเครื่อง (hardwired ผ่าน safety relay)

### สิ่งที่ต้องส่ง (Documentation Package เหมือนงานจริง)
- [ ] **System Architecture document** — network topology, I/O list, SOO
- [ ] **Safety concept** — อ้างอิง IEC 60204-1 / ISO 13849-1, กำหนด required PLr + เลือก safety component
- [ ] **แบบไฟฟ้าครบชุด (as-built):** power schematic, control, panel layout, pneumatic diagram, network diagram
- [ ] **การเลือกขนาด:** breaker/สาย/fuse จาก datasheet servo/inverter/safety relay
- [ ] **โปรแกรมทั้งหมด:** GX Works3 (PLC), GT Designer3 (HMI), KRL (robot), MR Configurator2, FR Configurator2
- [ ] **Test records:** continuity test, insulation (megger) test ผ่านเกณฑ์
- [ ] **Commissioning report:** pre-power checklist (ครูเซ็น), first power-up section-by-section, functional test
- [ ] **O&M หน้าเดียว:** วิธีใช้ + alarm troubleshooting guide สำหรับ operator/maintenance
- [ ] **การนำเสนอ (presentation):** สาธิตเครื่องทำงาน + อธิบาย design decision + รับมือ live troubleshooting

### Rubric ใหญ่ (200 คะแนน)

| หมวด | เกณฑ์ | คะแนน |
|---|---|---|
| **SAFETY (GATE — บังคับผ่าน)** | Pre-power checklist ครู approve, LOTO ทุกครั้ง, safety concept สมเหตุผล, E-stop/light curtain ทำงานจริง — **ผิดพลาดร้ายแรง = ไม่ผ่าน Capstone** | Pass/Fail |
| **เอกสารวิศวกรรม** | แบบครบ, as-built ตรงของจริง, BOM/sizing ถูก, SOO ชัด | 30 |
| **Panel build & wiring** | workmanship, grounding/shielding, ferrule, ผ่าน continuity + megger test | 30 |
| **PLC program** | step sequence สะอาด, FB modular, recipe, ไม่ค้าง, documentation ดี | 30 |
| **HMI** | operation/alarm/recipe ครบ, UX สำหรับ operator ดี, mode management | 20 |
| **Servo + Inverter** | positioning แม่น, Modbus comm เสถียร, parameter เหมาะสม | 20 |
| **Robot integration** | pick&place ทำงาน, handshake ไม่ deadlock, teach point แม่น | 20 |
| **System integration** | ทุกระบบทำงานร่วมกัน, full auto cycle สำเร็จต่อเนื่อง, cycle time สมเหตุผล | 20 |
| **Commissioning discipline** | first power-up ทีละ section, test record ครบ | 10 |
| **Troubleshooting (live)** | ครูใส่ fault ข้ามระบบ (comm/wiring/logic) → diagnose เป็นระบบ + แก้ได้ | 20 |
| **การนำเสนอ & O&M** | สื่อสาร design decision ชัด, เอกสารส่งมอบมืออาชีพ | 10 |

### เกณฑ์ระดับผลงาน (Performance Levels)

| ระดับ | คะแนน | ความหมาย |
|---|---|---|
| **Expert (ทำงานจริงได้)** | 170–200 | commission เครื่องเองได้, เอกสารระดับส่งมอบลูกค้า, troubleshoot ข้ามระบบคล่อง |
| **Competent** | 140–169 | ทำงานได้ภายใต้คำแนะนำเล็กน้อย, เอกสารครบแต่ต้องขัดเกลา |
| **Developing** | 110–139 | ทำได้บางส่วน, ต้องมีพี่เลี้ยง, ยังพลาดจุด integration |
| **Not yet** | < 110 หรือ ติด Safety GATE | ต้องทำซ้ำ |

### Stretch Goal ระดับ Capstone
- เพิ่ม **vision/measurement** เลือกตำแหน่งวางแบบ dynamic
- ทำ **changeover** สลับ recipe โดยไม่หยุดเครื่องนาน + วัด OEE เบื้องต้น
- เพิ่ม **predictive alarm** จากการอ่าน servo/inverter health (current/torque/load)
- ออกแบบ **fault injection self-test** ให้เครื่องวินิจฉัยตัวเองได้

---

# STRETCH TIER (Elective M13)
### Mini-Capstone: "Digital Factory — ต่อยอด Pick & Place ขึ้น SCADA/Cloud"
**เวลา:** 32 ชม. | สำหรับผู้เรียนที่ผ่าน Grand Capstone และต้องการก้าวสู่ Industry 4.0

### โจทย์ / Spec
นำเครื่อง Pick & Place จาก M12 เชื่อมขึ้นระดับ supervisory: ดึงข้อมูลจาก FX5U ขึ้น SCADA + cloud dashboard + ระบบ predictive maintenance เบื้องต้น พร้อม OT cybersecurity

### Sequence / สถาปัตยกรรม
1. FX5U เปิดบริการ SLMP/MC Protocol + Modbus TCP → ตรวจด้วย ping/port test
2. OPC UA server (KEPServerEX) ดึงค่า D/M จาก FX5U → SCADA (Ignition)
3. สร้าง tag database + real-time/historical trend + alarm (priority/deadband/ack)
4. ส่งข้อมูล production/health ผ่าน MQTT → Node-RED → InfluxDB → Grafana dashboard
5. Predictive: monitor motor current/servo load → threshold alert
6. Cybersecurity: VLAN segmentation OT/IT, firewall rule, OPC UA security policy/certificate

### สิ่งที่ต้องส่ง
- [ ] IP plan + network segmentation diagram (OT/IT)
- [ ] SCADA project (tag DB + trend + alarm)
- [ ] OPC UA + MQTT config + dashboard ที่แสดงข้อมูลจริงจากเครื่อง
- [ ] Predictive maintenance logic + ตัวอย่าง alert
- [ ] Cybersecurity assessment เบื้องต้น
- [ ] นำเสนอ Digital Factory demo

### Rubric (100 คะแนน)
| หมวด | คะแนน |
|---|---|
| Network/IP plan + เชื่อม FX5U สำเร็จ (ping/port ผ่าน) | 20 |
| SCADA tag/trend/alarm ทำงานจริง | 25 |
| OPC UA + MQTT + dashboard end-to-end | 25 |
| Predictive maintenance concept + demo | 15 |
| OT Cybersecurity (segmentation/security policy) | 15 |

### Stretch Goal
- เชื่อม cloud จริง (Azure/AWS IoT free tier) + remote dashboard
- คำนวณ OEE real-time + downtime analysis
- ทำ Unified Namespace (UNS) structure ด้วย MQTT topic hierarchy

---

# ตารางสรุปการไหลของโครงงาน (Master Map)

| Tier | Mini-Project | โมดูล | ผลลัพธ์สะสมสู่ Capstone |
|---|---|---|---|
| 1 | A: ตู้ DOL Starter | M00–M02 | ตู้ไฟ + control circuit + ทักษะ wiring/safety |
| 2 | B: Electro-pneumatic + VFD | M03–M05 | actuator + conveyor + sensor + inverter |
| 3 | C: Servo + PLC + HMI | M06–M08 | สมอง PLC + motion + หน้าจอควบคุม |
| 4 | D: Robot + Comm + Sequence | M09–M11 | หุ่นยนต์ + เครือข่าย + state machine |
| **GRAND** | **M12: Full Pick & Place** | รวมทุกโมดูล | **เครื่องจริงที่ commission ได้** |
| Stretch | M13: Digital Factory | M13 | ต่อยอด SCADA/IIoT |

---

# หลักการให้คะแนนข้ามทุก Tier (Cross-Cutting Principles)

1. **Safety เป็น GATE เสมอ** — ผิดพลาดด้านความปลอดภัย (ไม่ทำ LOTO, ไม่ verify zero energy, จ่ายไฟก่อนครู approve) = สอบตกโครงงานนั้นทันที ไม่ว่าคะแนนอื่นจะดีแค่ไหน
2. **เอกสารต้องตรงกับของจริง (as-built)** — แบบที่ไม่ตรงเครื่อง = หักคะแนนหนัก เพราะหน้างานจริงเอกสารผิดทำให้ช่างคนถัดไปอันตราย
3. **Troubleshooting คือหัวใจ** — ทุก Tier มีคะแนน troubleshooting 15–20% เพราะนี่คือทักษะที่แยก "คนทำงานจริงได้" ออกจาก "คนทำตามใบงาน"
4. **โค้ดต้องให้คนอื่นอ่านได้** — comment, FB modular, naming convention — เพราะทีมบำรุงรักษาต้องดูแลต่อ
5. **ไล่ระดับความเป็นเจ้าของ** — Tier ต้นครูกำหนด spec ชัด; Capstone ผู้เรียนออกแบบเองมากขึ้น (design decision เป็นคะแนน)

---

**หมายเหตุการนำไปใช้:** แต่ละ mini-project ควรมี Lab ย่อยใน module นำมาประกอบ ดังนั้นชั่วโมงโครงงานที่ระบุเป็น "ชั่วโมงบูรณาการเพิ่มเติม" ที่ดึงจาก capstone-block ของแต่ละโมดูล (เช่น M02 บทที่ 8, M06 บทที่ 10, M11 บทที่ 7) ไม่ใช่เวลาเพิ่มนอกหลักสูตร