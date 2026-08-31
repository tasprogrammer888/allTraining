# [M05] การควบคุมมอเตอร์ไฟฟ้า (DOL / Star-Delta / VFD)
**Electric Motor Control (DOL / Star-Delta / VFD)**

| รายการ | รายละเอียด |
|---|---|
| Module ID | M05 |
| Level | Basic |
| ระยะเวลา | **32 ชั่วโมง** (ทฤษฎี ~14 + ปฏิบัติ ~18) |
| Prerequisites | M00, M01, M02 |
| ต่อยอดไป | M06 (PLC), M08 (Servo/Inverter ขั้นสูง), M12 (Capstone) |

> **หมายเหตุการปรับปรุง:** เพิ่มเวลาจาก 28 → **32 ชม.** เพราะร่างเดิมอัดเนื้อหา 8 บท + 6 lab ในเวลาที่ไม่พอให้ "ต่อเป็น เดินจริง และไล่ปัญหาเป็น" โดยเฉพาะ VFD external control และ troubleshooting ที่เดิมตั้งเป็น objective แต่ไม่มี lab รองรับ — จึงเพิ่ม **Lab 7 (external control)** และ **Lab 8 (fault-injection drill)** และขยายบท VFD ให้ครอบคลุม safety จริงหน้างาน

---

## 1. ภาพรวมและเหตุผลของโมดูล

หลังผู้เรียนออกแบบวงจรควบคุมด้วยรีเลย์เป็นแล้ว (M02) โมดูลนี้คือการเอา logic เหล่านั้นไป **ขับโหลดจริงคือมอเตอร์** ซึ่งเป็นหัวใจของเครื่องจักรเกือบทุกตัวในโรงงาน เป้าหมายคือจบแล้วต้อง:
- ต่อ DOL / Forward-Reverse / Star-Delta / VFD ได้จริง เดินได้ และปลอดภัย
- เลือกอุปกรณ์ป้องกันให้ตรงมอเตอร์โดยอ่าน datasheet เป็น
- ไล่หา fault อย่างเป็นระบบ ไม่ใช่เดาสุ่มเปลี่ยนของ

### ความต่อเนื่องกับโมดูลอื่น (ไม่ซ้ำ ไม่มีช่องว่าง)
| มาจาก | ใช้ความรู้ |
|---|---|
| M00 | 3-phase, Vline=√3·Vphase, มัลติมิเตอร์/คลิปแอมป์/megger, **LOTO** |
| M01 | อ่าน wiring diagram, เข้าหัวสาย, ขนาดสาย, จัดสายในตู้ |
| M02 | contactor, NO/NC, self-holding, **interlock** (M05 นำมาใช้กับ power circuit จริง ไม่สอนซ้ำหลักการ relay) |

| ส่งต่อไป | สิ่งที่ M05 วางรากให้ |
|---|---|
| M06 PLC | DOL/star-delta เขียนเป็น ladder ได้ → ผู้เรียนเห็นสะพานจาก hardwired สู่ PLC |
| M08 Servo/Inverter ขั้นสูง | parameter, control terminal, V/f → ต่อยอด vector/closed-loop |
| M12 Capstone | inverter ขับสายพาน + คุยกับ PLC (DI/analog/fault relay) |

---

## 2. Learning Objectives
เมื่อจบโมดูล ผู้เรียนสามารถ:
1. อธิบายหลักการ 3-phase induction motor และอ่าน nameplate ได้ครบทุกช่อง
2. ตัดสินใจต่อ Y/Δ ให้ถูกตามแรงดันระบบ + nameplate และเข้าใจกับดัก 400/690V
3. เลือก/ต่ออุปกรณ์ป้องกัน (MCCB, contactor AC-3, OLR class, ฟิวส์) ตาม FLC
4. ต่อ DOL starter ครบชุด + pre-energization checklist + LOTO
5. ต่อ Forward/Reverse พร้อม electrical **และ** mechanical interlock
6. ต่อ + ตั้งเวลา Star-Delta (open transition) เข้าใจ transient spike และ dead time
7. ติดตั้ง/ตั้งค่า VFD Mitsubishi FR จาก manual จริง + ปฏิบัติ safety (DC bus discharge)
8. ควบคุม inverter ด้วยสัญญาณภายนอก (STF/STR, multi-speed, analog, output)
9. วัด/เทียบ inrush ของ DOL vs Star-Delta vs VFD และสรุปเป็นตารางตัดสินใจ
10. Troubleshoot อาการเสียอย่างเป็นระบบ รวม inverter fault code

---

## 3. โครงสร้างเนื้อหา (Topics)

### บทที่ 1 — 3-phase Induction Motor และ Nameplate (4 ชม.)
- โครงสร้าง: stator, rotor (squirrel cage), bearing, terminal box
- Rotating magnetic field, **Ns = 120f/P**, slip `s = (Ns−N)/Ns`
- Torque-speed curve: starting / pull-up / breakdown / full-load
- อ่าน nameplate ครบ: kW/HP, V (230/400 หรือ 400/690), A, Hz, RPM, cos φ, IE class, IP, insulation class, duty S1/S3, connection Y/Δ
- คำนวณ FLC, LRC (~6–8×), starting torque code

**ตัวอย่างคำนวณ FLC:** มอเตอร์ 4 kW, 400V, cosφ 0.85, η 0.88
`FLC = P / (√3 · V · cosφ · η) = 4000 / (1.732 · 400 · 0.85 · 0.88) ≈ 7.7 A`
→ inrush ≈ 6–8 × 7.7 ≈ **46–62 A**

---

### บทที่ 2 — การต่อ Star (Y) / Delta (Δ) และ Decision Logic (4 ชม.)
ขั้ว 6 ขั้วใน terminal box (ลำดับมาตรฐานสลับแถวล่าง):
```
   U1   V1   W1      <- ต้นขด (ต่อ L1 L2 L3)
   W2   U2   V2      <- ปลายขด (วาง link ตรงนี้)
```
- **Star (Y):** ลัดปลายขด W2-U2-V2 เข้าด้วยกัน (link แนวนอนแถวล่าง) → `Vphase = Vline/√3`
- **Delta (Δ):** link แนวตั้ง U1-W2, V1-U2, W1-V2 → `Vphase = Vline`

#### ตารางตัดสินใจ (จุดพลาดที่เจอบ่อยหน้างาน)
| Nameplate | ระบบ 400V → ต่อแบบ | Star-Delta starter? |
|---|---|---|
| 230/400V (Δ/Y) | **Y** (รันปกติ) | ❌ ใช้ไม่ได้ (รันเป็น Y อยู่แล้ว) |
| 400/690V (Δ/Y) | **Δ** (รันปกติ) | ❌ ห้าม! ขณะ Y ได้แค่ 230V/phase = under-volt ออกตัวไม่ขึ้น |
| 230/400V บนระบบ 230V | **Δ** (รันปกติ) | ✅ ใช้ star-delta ได้ (Y สตาร์ท → Δ รัน) |

> **กฎทอง:** Star-Delta ใช้ได้เฉพาะมอเตอร์ที่ **รันปกติแบบ Δ ที่แรงดันระบบ** เท่านั้น

---

### บทที่ 3 — อุปกรณ์ป้องกัน + อ่าน Datasheet (4 ชม.)
- **MCCB:** short-circuit + overload; เลือก In, Icu/Ics
- **Contactor:** อ่าน **AC-3** rating (ไม่ใช่ AC-1) เลือกตามกำลังมอเตอร์
- **OLR:** ตั้ง = FLC, trip class 10/20/30, reset auto/manual, phase-loss sensitive
- **ฟิวส์:** aR (back-up semiconductor), gG (general)
- **Coordination type 1 vs type 2**
- เลือกขนาดสายตามกระแส + voltage drop

> **Lab เชื่อมโยง:** ผู้เรียนเปิด selection table จริงของผู้ผลิตเพื่อจับคู่ contactor+OLR กับมอเตอร์ใน Lab 3

---

### บทที่ 4 — DOL Starter (4 ชม.)
**Power:** `MCCB → main contactor → OLR → motor`

**Control (ladder เทียบเท่า):**
```
  L1 ----[ Stop NC ]----+----[ Start NO ]----+----( KM )---- N
                        |                    |
                        +----[ KM aux NO ]---+        (self-holding)
  L1 ----[ OLR NC ]------------------------------ (อนุกรมในสาย control)
```
- แปลง wiring ⇄ ladder (เตรียม M06)
- pre-energization checklist + LOTO
- ข้อจำกัด DOL: inrush 6–8×, voltage dip → เหมาะมอเตอร์เล็ก

---

### บทที่ 5 — Forward / Reverse + Interlock (3 ชม.)
- กลับทาง = **สลับ 2 เฟส** (L1↔L3) — ห้ามสลับเฟสเดียว
- 2 contactors KM-FWD / KM-REV
- **Electrical interlock:** NC aux ไขว้เข้า coil อีกตัว
- **Mechanical interlock:** ต้องมีด้วยเสมอ (กัน contactor หน้าสัมผัสติด/ค้าง)

```
 Control (interlock ไขว้):
  ...[Fwd NO]---[KM-REV NC]---( KM-FWD )
  ...[Rev NO]---[KM-FWD NC]---( KM-REV )
```
> ถ้า FWD+REV ดูดพร้อมกัน = **phase-to-phase short** → จึงต้อง interlock 2 ชั้น

---

### บทที่ 6 — Star-Delta Starter (4 ชม.)
- ขณะ Y: V/phase ลดเป็น 1/√3 → กระแส & torque เหลือ **~1/3**
- อุปกรณ์: KM-Main, KM-Star, KM-Delta + star-delta timer
- **OLR ตั้งที่ ~0.58×FLC** (วัด line current ในวง Δ) — จุดพลาดคลาสสิก
- ลำดับ **open transition:**
  1. KM-Main + KM-Star ON (Y สตาร์ท)
  2. หน่วงเวลา → KM-Star OFF
  3. **dead time** (กันยิงพร้อมกัน)
  4. KM-Delta ON (Δ run)
- **transient spike:** open transition ตัดไฟชั่วขณะ → กระแส/แรงบิดกระชากตอนสลับ ถ้า dead time สั้นไปหรือ Star+Delta ดูดพร้อมกัน = short
- ต่อสาย 6 เส้น U1V1W1 / U2V2W2 ให้ตรง — สลับผิด = ลัดวงจร/หมุนกลับ

---

### บทที่ 7 — VFD / Inverter (Mitsubishi FR) + Safety (4 ชม.)
**หลักการ:** `Rectifier → DC bus (cap) → IGBT inverter → PWM`, **V/f constant**; เหนือ base freq = field weakening (torque ลด)

**Terminal:** R/S/T เข้า, U/V/W ออกมอเตอร์, P/+ N/− (DC/brake), control block

**Parameter พื้นฐาน (อ่านจาก FR manual จริง):**
| Pr | ความหมาย |
|---|---|
| Pr.0 | Torque boost |
| Pr.1 / Pr.2 | Max / Min frequency |
| Pr.3 | Base frequency |
| Pr.7 / Pr.8 | Accel / Decel time |
| Pr.9 | Electronic thermal O/L = **motor FLC** |
| Pr.79 | Operation mode (1=PU, 2=EXT, 3/4=combined, 0=auto) |
| Pr.80 | Motor capacity (kW) |
| Pr.160 | Parameter display level |
| ALLC | All clear (factory default) |

> ⚠️ **Safety VFD ที่ต้องท่องให้ขึ้นใจ:**
> 1. รอ **charge lamp ดับ + ≥10 นาที** ก่อนแตะขั้ว (cap มีไฟค้าง)
> 2. **ห้าม megger/AC** ที่ขั้ว U/V/W → IGBT พัง
> 3. **ห้าม contactor ตัด-ต่อ** ระหว่าง inverter↔motor ขณะรัน
> 4. ground ให้ครบ, ใช้ shielded cable, แยกสาย power จากสัญญาณ, ดู carrier freq (Pr.72) vs ระยะสาย

---

### บทที่ 8 — External Control + Troubleshooting (5 ชม.)
**Control terminal:** STF/STR/SD (DI), RH/RM/RL (multi-speed), terminal 2 (0–10V), terminal 4 (4–20mA + AU), terminal 5 (common); output RUN/FU/SE, A/B/C (fault relay) — sink/source wiring

**Mapping สู่ PLC (สะพาน M06/M08):**
| สัญญาณ | ทิศทาง | ใช้ทำ |
|---|---|---|
| PLC DO → STF | OUT | สั่ง run forward |
| PLC AO 4–20mA → term 4 | OUT | สั่งความเร็ว |
| RUN / FU → PLC DI | IN | สถานะเดิน |
| A-C fault relay → PLC DI | IN | แจ้ง fault |

**Fault code และ root cause:**
| Code | ความหมาย | สาเหตุ/แก้ |
|---|---|---|
| E.OC1/2/3 | Overcurrent (accel/run/decel) | accel เร็วไป, โหลดติด, สั้นวงจร → เพิ่ม Pr.7/8, เช็คโหลด |
| E.OV | Overvoltage | decel เร็วเกิน/regeneration → เพิ่ม Pr.8, ใส่ brake resistor |
| E.THM | Motor thermal | Pr.9 ผิด/โหลดเกิน → ตั้ง Pr.9=FLC |
| E.THT | Inverter thermal | inverter เล็กไป/ร้อน → ตรวจ cooling |
| E.OLT | Stall/overload | โหลดเกิน → ลดโหลด/ปรับ stall prev |
| E.LF | Output phase loss | สายมอเตอร์หลุด → ขันขั้ว |
| E.UVT | Undervoltage | ไฟตก → ตรวจ supply |

---

## 4. Hands-on Labs (8 ห้องปฏิบัติ)

| # | Lab | Deliverable หลัก |
|---|---|---|
| 1 | อ่าน nameplate + incoming inspection (megger) | ใบงาน + go/no-go |
| 2 | ต่อ Y/Δ + พิสูจน์ decision logic | ใบตัดสินใจ + ตารางกระแส |
| 3 | DOL starter เดินจริง | วงจรทำงาน + checklist เซ็น + inrush |
| 4 | Forward/Reverse + interlock 2 ชั้น | หลักฐาน interlock + ผังที่วาดเอง |
| 5 | Star-Delta อัตโนมัติ + เทียบ inrush | กราฟ DOL vs Y vs transition vs Δ |
| 6 | VFD ตั้งค่า PU + อ่าน manual | ตาราง V/f + DC-bus safety checklist |
| 7 | VFD external control (analog + multi-speed) | mapping terminal + fault relay จุดไฟ |
| 8 | **Troubleshooting drill (fault injection)** | report ≥4 เคส (root cause + แก้) |

> **เพิ่มจากร่างเดิม:** Lab 7 และ Lab 8 — เดิมมีแค่ถึง Lab 6 ทำให้ objective "external control" และ "troubleshoot" ไม่มีภาคปฏิบัติรองรับ จึงสอนแล้วทำงานจริงไม่ได้

---

## 5. Pre-Energization Checklist (ใช้ทุก lab ก่อนจ่ายไฟ)
- [ ] ทำ LOTO ตลอดการต่อสาย และ **test-before-touch** ก่อนสัมผัส
- [ ] ขันทุกขั้วแน่นด้วย torque ที่ถูกต้อง (ไม่มีสายหลวม)
- [ ] Ground ต่อครบและถูกต้อง
- [ ] ไม่มีสายหลุด/เศษลวด/เครื่องมือค้างในตู้
- [ ] ตั้ง OLR = FLC (หรือ 0.58×FLC สำหรับ star-delta)
- [ ] ตรวจลำดับเฟส + ทิศหมุนคาดหวัง
- [ ] (VFD) charge lamp ดับ, R/S/T และ U/V/W ไม่สลับ, ไม่ megger ที่ output
- [ ] ผู้สอนเซ็นรับรองก่อนสับเบรกเกอร์

---

## 6. ข้อผิดพลาดที่เจอบ่อย & Safety (สรุปสำหรับผู้สอนย้ำ)
1. มอเตอร์ 400/690V บน 400V → ต้อง Δ, **ห้าม** star-delta
2. star-delta dead time สั้นไป / Star+Delta ดูดพร้อม = short → interlock 2 ชั้น
3. forward/reverse ขาด mechanical interlock = เสี่ยง contactor ค้าง
4. OLR ใน star-delta ตั้งผิด (ต้อง 0.58×FLC)
5. VFD: ไม่รอ DC bus คายประจุ = ไฟดูด
6. VFD: megger/AC ที่ U/V/W = IGBT พัง
7. VFD: contactor ตัดต่อ motor ขณะรัน = trip/พัง
8. single-phasing → มอเตอร์คราง ร้อน ไหม้ (ใช้ OLR phase-loss + ขันขั้วแน่น)
9. กลับทาง = สลับ 2 เฟส (ไม่ใช่เฟสเดียว)
10. ต่อ 6 เส้น star-delta ผิดขั้ว = ลัด/หมุนกลับ

---

## 7. การประเมินผล
| สัดส่วน | รายการ |
|---|---|
| 60% | ภาคปฏิบัติ — วงจรทำงานจริง + checklist (เน้น Lab 3,5,6,7,8) |
| 20% | ใบงาน/รายงาน — nameplate, decision logic, ตาราง inrush, troubleshooting report |
| 10% | สอบทฤษฎี — Y/Δ, FLC/slip/inrush, อุปกรณ์ป้องกัน, fault code |
| 10% | Safety & housekeeping — LOTO/test-before-touch, จัดสาย, VFD safety |

**เกณฑ์ผ่าน:** ทุก lab หลักเดินได้จริง + troubleshooting drill แก้ได้ ≥3/4 เคส ด้วยวิธีเป็นระบบ (ไม่เดาสุ่ม)

---

## 8. อุปกรณ์/ซอฟต์แวร์
มอเตอร์ 3 เฟส (ทั้ง 230/400V และ 400/690V), ตู้ฝึก contactor×3+OLR+timer, mechanical interlock kit, Mitsubishi FR-D700/E700/E800 + PU, potentiometer, loop calibrator 4–20mA, true-RMS multimeter, clamp meter (inrush), megohmmeter 500V, LOTO set, PPE, **FR series manual ตัวจริง** + selection table ผู้ผลิต, (option) GX Works3 โชว์ ladder เตรียม M06

---
*โมดูลนี้ปรับปรุงให้ "ต่อเป็น เดินได้ ปลอดภัย และไล่ปัญหาเป็น" โดยอุดช่องว่าง external-control + troubleshooting ที่ร่างเดิมขาด และเสริม decision logic / VFD safety ที่จำเป็นต่อการทำงานจริงหน้างาน*