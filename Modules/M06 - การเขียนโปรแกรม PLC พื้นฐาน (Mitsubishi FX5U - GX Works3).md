# [M06] การเขียนโปรแกรม PLC พื้นฐาน (Mitsubishi FX5U / GX Works3)

**Level:** Basic | **Duration:** 40 ชั่วโมง | **Prerequisites:** M02, M04, M00

> โมดูลนี้คือสะพานจาก "relay logic ที่จับต้องได้" (M02) ไปสู่ "logic ที่อยู่ในโปรแกรม" — เป้าหมายคือจบแล้ว **เขียนโปรแกรมที่รันบน PLC จริงได้ ต่อ I/O เป็น ไล่ปัญหาเป็น และทำงานปลอดภัย** ไม่ใช่แค่รันบน simulator

---

## ภาพรวมและความต่อเนื่องของหลักสูตร

| มาจาก | ใช้ทักษะอะไร | ส่งต่อไปที่ |
|---|---|---|
| M02 Relay logic | self-hold, interlock, sequence | Lab 6 แปลงเป็น ladder |
| M04 Sensor/Actuator | NPN/PNP, sink/source | Lab 2 ต่อ I/O |
| M00 Safety/ไฟฟ้า | 24V, มัลติมิเตอร์, LOTO | safety check ทุกแล็บ |
| **M06 (โมดูลนี้)** | device, MOV, compare | **M07 HMI** |
| | high-speed counter เบื้องต้น | **M08 Servo** |
| | structured + diagnostics | **M10/M11/M12** |

**สิ่งที่แก้ไขจากร่างเดิม (จุดที่ "สอนแล้วทำงานจริงไม่ได้"):**
1. **TON/TOF/CTU/CTD เป็นชื่อ IEC ไม่ใช่ device จริงบน FX5U classic ladder** — บน FX5U เขียน `OUT T0 K10` (on-delay), retentive ใช้ `ST`, counter ใช้ `OUT C0 K10`. **FX5U ไม่มี TOF device** ต้องทำ off-delay ด้วย logic หรือ Timer FB — ร่างเดิมทำให้ผู้เรียนหา TOF ที่คีย์บอร์ดไม่เจอ
2. **เพิ่มบท Safety/Fail-safe (บทที่ 9)** — ร่างเดิมไม่มีเลย ทั้งที่เป็นหัวใจงานหน้างาน (E-stop ต้อง hardwired)
3. **เพิ่ม Troubleshooting drill เป็นแล็บเต็ม (Lab 8)** — ฝัง bug จริงให้ไล่ ไม่ใช่แค่ "รู้จัก monitor"
4. **เพิ่มการอ่าน datasheet/manual จริง** ใน Lab 2 (หา sink/source จาก hardware manual)
5. **ย้ำ octal addressing, double coil, edge บน arithmetic** — กับดักที่ผู้เรียนเจอแน่นอน

---

## Learning Objectives
จบโมดูลผู้เรียนสามารถ:
1. อธิบาย architecture + scan cycle และประมาณ scan time / response delay
2. แยกแยะ device memory FX5U (X/Y octal, M, L, D, T, ST, C/LC, SM/SD)
3. เปรียบเทียบภาษา IEC 61131-3 และเลือกใช้ LD/ST/SFC ให้เหมาะ
4. ใช้ GX Works3 ครบ: project, parameter, Ethernet, download, monitor, online edit, Simulator
5. ต่อ I/O sink/source ถูกต้อง + pre-power-on safety check
6. เขียน ladder ด้วยคำสั่งพื้นฐานบน hardware จริง
7. แปลง relay logic (M02) เป็น ladder
8. ออกแบบ structured program + documentation + backup
9. troubleshoot อย่างเป็นระบบ
10. ออกแบบ fail-safe wiring + E-stop hardwired

---

## โครงสร้างเนื้อหา (10 บท / 40 ชม.)

### บทที่ 1 — PLC คืออะไร, Architecture, Scan Cycle (4 ชม.)
- PLC vs relay panel: เมื่อไหร่คุ้ม, เมื่อไหร่ไม่คุ้ม
- Hardware: CPU / power supply / I/O circuit / extension
- MELSEC iQ-F + FX5U-32M/64M, built-in Ethernet + RS-485
- **Scan cycle 4 ขั้น:** `Input refresh → Program execution → Output refresh → END/housekeeping`
- Scan time → response delay; input สั้นกว่า scan time อาจหลุด (→ ปูทาง pulse catch/edge)

```
        ┌──────────────────────────────────────────┐
        │  1. INPUT REFRESH  (อ่าน X เข้า image)     │
        │  2. PROGRAM EXEC   (รัน ladder ทีละ rung)  │
        │  3. OUTPUT REFRESH (เขียน image ออก Y)     │
        │  4. END PROCESSING (watchdog, comm, diag)  │
        └────────────────────┬─────────────────────┘
                             └──► วนซ้ำ (1 scan ≈ ไม่กี่ ms)
```

### บทที่ 2 — Device Memory & I/O Addressing (4 ชม.)
**ตารางสรุป device หลักของ FX5U**

| Device | ชนิด | ความหมาย | ฐานเลข | หมายเหตุ |
|---|---|---|---|---|
| X | bit | input relay | **octal** | X0-X7, **X10**-X17 (ไม่มี X8/X9) |
| Y | bit | output relay | **octal** | เหมือน X |
| M | bit | internal relay | decimal | งานทั่วไป |
| L | bit | latch relay | decimal | ค้างค่าเมื่อ power off (ตาม latch range) |
| D | word(16) | data register | decimal | คู่กัน = 32-bit (DMOV) |
| T | - | low-speed timer | decimal | default 100ms |
| ST | - | retentive timer | decimal | ค้างค่า ต้อง RST ล้าง |
| C / LC | - | counter / long(32-bit) | decimal | reset ด้วย RST |
| SM / SD | bit/word | special | decimal | SM400=always ON, SM412=1s clock |

> **กับดักที่ 1 (octal):** หลัง X7 คือ X10 — ผู้เรียนมักนับ decimal แล้วต่อสายผิด terminal

### บทที่ 3 — IEC 61131-3 & Ladder (3 ชม.)
- 4 ภาษาใน GX Works3: **LD** (logic), **FBD**, **ST** (คำนวณ/ลูป), **SFC** (sequence)
- ทำไมช่างไฟใช้ LD: อ่านเหมือน relay diagram จาก M02
- องค์ประกอบ: rung, rail, contact NO/NC, coil, power flow (ซ้าย→ขวา, บน→ล่าง)
- **NO/NC ในโปรแกรม vs wiring จริง:** ปุ่ม stop ต่อ NC → สัญญาณ ON ตอนปกติ → ในโปรแกรมใช้ contact NO ของ X นั้น (กดแล้ว X = OFF → ตัด)
- **double coil** = bug ยอดฮิต (อธิบายเต็มในบท 10)

### บทที่ 4 — GX Works3 ตั้งแต่ติดตั้งถึง Download (5 ชม.)
ขั้นตอน connect จริง:
1. New Project → MELSEC iQ-F → FX5U → Ladder
2. Module/CPU parameter → Ethernet Port → ตั้ง IP (เช่น `192.168.3.250`)
3. ตั้ง IP ของ PC ให้ subnet เดียวกัน (เช่น `192.168.3.1`) → **ping ทดสอบ**
4. เขียน rung → **Convert (F4)**
5. Connection Destination → Write to PLC
6. RUN → Online Monitor

```
rung แรก:
  |  X0          Y0  |
  |--| |---------( )--|     // X0 ON → Y0 ON
```

### บทที่ 5 — Contact/Coil/Latch/Edge (5 ชม.)
- LD/LDI, AND/ANI, OR/ORI, OUT, ANB/ORB
- **Self-hold (start/stop):**
```
  |  X0   X1          Y0  |
  |--| |--|/|----+------( )--|   X0=Start, X1=Stop(NC ทาง wiring)
  |  Y0          |           |
  |--| |---------+           |   self-hold
```
- SET/RST (latch ค้างแม้ rung false) vs self-hold
- PLS/PLF: ทำงาน 1 scan
- **Interlock forward/reverse:**
```
  |  X0   Y1          Y0  |   Fwd: ต้องไม่มี Rev
  |--| |--|/|---------( )--|
  |  X2   Y0          Y1  |   Rev: ต้องไม่มี Fwd
  |--| |--|/|---------( )--|
```

### บทที่ 6 — Timer & Counter (ตามจริงบน FX5U) (4 ชม.)
- **On-delay:** `OUT T0 K10` → 1.0 วินาที (T0 base 100ms); reset = rung false
- **Retentive:** ใช้ `ST` → ค้างค่าเมื่อ rung false ต้อง `RST ST0`
- **ชื่อ IEC TON/TOF อยู่ใน Function Block** ไม่ใช่ device classic
- **off-delay (FX5U ไม่มี TOF device):**
```
  // Y1 ดับช้ากว่า X0 = 3s
  |  X0                 Y1  |
  |--| |----------------( )--|       X0 ON → Y1 ON ทันที
  |  X0                      |
  |--|/|------[OUT T0 K30]---|       X0 OFF → เริ่มจับเวลา
  // ใช้ T0 ไปตัด self-hold ของ Y1
```
- Counter: `OUT C0 K10` + `RST C0`
- ประยุกต์: ไฟกระพริบ (timer 2 ตัว หรือ `SM412`), นับชิ้นงาน

### บทที่ 7 — Compare / MOV / Arithmetic (4 ชม.)
- Compare contact: `LD= D0 K100`, `AND> D0 D1`, ...
- `MOV K100 D0`, `DMOV` (32-bit), `BMOV`, `FMOV`
- Arithmetic: `+ - * /`; 16-bit overflow → ใช้ `D+` / `D*`
- **กับดักที่ 2 (edge):** `INC D0` / `MOV` ทำงาน **ทุก scan** → ค่าพุ่ง ต้องคุมด้วย `PLS` หรือ P-instruction (`INCP`)
```
  |  X0             |
  |--| |----[PLS M0]|
  |  M0             |
  |--| |----[INC D0]|   เพิ่มทีละ 1 ต่อการกด 1 ครั้ง
```

### บทที่ 8 — Relay→Ladder, Structured, Documentation (4 ชม.)
- เทคนิคแปลง: input→contact, output→coil, holding→self-hold
- **สิ่งที่ relay ไม่มีแต่ ladder มี:** scan order, double coil, edge, last-state-wins
- Step sequence ด้วย M-coil (state machine) หรือ SFC
- แบ่ง block: **Main / Auto / Manual / Safety**
- Device comment + label + naming convention + backup `.gx3`

### บทที่ 9 — Safety, Fail-safe Wiring, ข้อจำกัด PLC (3 ชม.) ⚠️ *บทใหม่*
- **E-stop ต้อง hardwired** ตัด output/มอเตอร์โดยตรง — ห้ามพึ่ง software logic
- **Fail-safe:** stop/limit/safety = **NC** (สายขาด → หยุด); start = NO
- PLC ทั่วไป **ไม่ใช่ safety-rated** → รู้จัก safety relay/safety PLC
- Output protection: flyback diode (inductive load), fuse, แยก field power
- Start-up behavior: latch อาจทำให้ output ON ทันทีตอน power-on
- เชื่อม M05: PLC สั่ง contactor/VFD อย่างปลอดภัย

```
E-STOP (hardwired) — ถูกต้อง:
  L+ ──[E-STOP NC]──[Safety Relay]── ไปเลี้ยง output/contactor
  PLC สั่งได้เฉพาะเมื่อ safety chain ปิดอยู่เท่านั้น
```

### บทที่ 10 — Monitoring, Troubleshooting, Capstone ย่อย (4 ชม.)
- Online monitor (power flow สด), device batch monitor, watch window, **force (อันตรายบนเครื่องจริง)**
- CPU module diagnostics / error code / event history
- **Debug อย่างเป็นระบบ:** แยก hardware↔software → ไล่ `X → logic → Y` → ตรวจ double coil/scan order
- **double coil:** เขียน OUT Y0 สองที่ → ผลเป็นค่า rung สุดท้ายเสมอ (rung แรกถูกทับ)
- Online edit ขณะ RUN (และเมื่อใดไม่ควรทำ)
- Capstone ย่อย: conveyor + sorting (Auto/Manual/Safety)

---

## Hands-on Labs (9 แล็บ — ทุกแล็บลง hardware จริง)

| # | ชื่อ | Deliverable หลัก |
|---|---|---|
| 1 | ทัวร์ GX Works3 + Simulator | .gx3 build ผ่าน + Y0 ติดเมื่อ force X0 |
| 2 | ต่อ I/O sink/source + อ่าน datasheet + safety check | safety checklist + ระบุ sink/source จาก manual |
| 3 | Self-hold / SET-RST / Interlock / Fail-safe | ทดสอบถอดสาย stop แล้วหยุดจริง |
| 4 | ไฟกระพริบ + off-delay (ไม่มี TOF) | timing diagram + วิธีทำ off-delay |
| 5 | Counter + Compare + MOV | total ใน D100 + ใช้ edge ใน INC |
| 6 | แปลง Relay (M02) → Ladder | mapping table + กับดักที่เจอ |
| 7 | Structured Sequence (Pick&Place จำลอง) | Auto/Manual/Safety + state diagram |
| 8 | **Troubleshooting Drill** (bug ฝัง 5-6 จุด) | troubleshooting log + project ที่แก้แล้ว |
| 9 | **Capstone: Conveyor Sorting** | ทำงานจริง + E-stop hardwired + สาธิต debug สด |

---

## ⚠️ Common Mistakes & Safety (เน้นย้ำหน้างาน)
- [ ] **X/Y octal** — หลัง X7 คือ X10 (ไม่มี X8/X9)
- [ ] **double coil** — OUT Y เดียวกันสองที่ = bug
- [ ] **FX5U ไม่มี TOF device** — off-delay ทำด้วย logic/FB
- [ ] **arithmetic/INC/MOV ทุก scan** — ต้องใส่ edge (PLS/P-instruction)
- [ ] **E-stop ต้อง hardwired** — ห้ามพึ่ง software
- [ ] **fail-safe NC** สำหรับ stop/limit/safety
- [ ] **วัดด้วยมัลติมิเตอร์ก่อนจ่ายไฟทุกครั้ง** (short/polarity/S-S)
- [ ] **flyback diode** สำหรับ inductive output
- [ ] **force บนเครื่องจริงอันตราย** — ตรวจพื้นที่ก่อน
- [ ] **online edit** — ห้ามทำขณะมีคนอยู่ใกล้เครื่อง

---

## Equipment & Software
PC (Win10/11, Ethernet) · GX Works3 + GX Simulator3 · FX5U-32M/64M · DC 24V supply · push button NO/NC · selector · limit switch · sensor NPN+PNP · pilot lamp · relay 24V + flyback diode · solenoid จำลอง · E-stop (NC mushroom) · มัลติมิเตอร์ · conveyor/sorting board · **คู่มือจริง: FX5U User's Manual (Hardware), MELSEC iQ-F Programming Manual, GX Works3 Operating Manual**

## Assessment
Quiz ทฤษฎี 20% · Lab deliverables 40% · Troubleshooting drill 15% · Capstone ย่อย 25%
**เกณฑ์ผ่าน:** โปรแกรมรันบน PLC จริง (ไม่ใช่แค่ simulator), wiring ผ่าน safety check, E-stop ทำงาน, documentation อ่านเข้าใจ

> **เทียบมาตรฐานภายนอก:** เนื้อหา M06 (ร่วมกับ M00–M02) ครอบคลุมเกณฑ์สอบ **มาตรฐานฝีมือแรงงานแห่งชาติ สาขาช่างควบคุมด้วยระบบ PLC ระดับ 1** (ดูเอกสาร B §11, เอกสาร E §5.1) ผู้ที่ผ่าน Lab 1–9 ครบควรพร้อมสอบเทียบวุฒิภายนอกนี้ได้โดยไม่ต้องเตรียมเพิ่มมาก

## เชื่อมสู่โมดูลถัดไป
device/MOV/compare → **M07 HMI** · high-speed counter เบื้องต้น → **M08 Servo** · structured + diagnostics → **M10/M11** · Lab 7+9 เป็นเวอร์ชันย่อของ → **M12 Capstone Pick&Place**
