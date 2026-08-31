# [M09] พื้นฐานหุ่นยนต์ KUKA (KRL / smartPAD)

**ระดับ:** Intermediate | **ระยะเวลา:** 40 ชั่วโมง | **ต้องผ่านก่อน:** M08 (Servo/Inverter + advanced wiring), M06 (PLC), M00 (ความปลอดภัย)

> โมดูลนี้เน้น "ทำงานได้จริงหน้างาน" ไม่ใช่แค่กดปุ่มตามคู่มือ ผู้เรียนต้อง calibrate ได้แม่น เขียน KRL ที่ interlock กับ PLC ได้ และ troubleshoot ได้อย่างเป็นระบบ

---

## ภาพรวมและความต่อเนื่องกับหลักสูตร

| มาจาก | ใช้อะไร | ไปต่อที่ |
|---|---|---|
| M08 | servo/encoder, การต่อ power/signal แยก, ground/shield | แต่ละแกนหุ่นยนต์ = servo + brake + encoder |
| M06 | ladder, digital I/O, handshake | PLC คุม sequence สั่งหุ่นยนต์ผ่าน I/O |
| M00/M01/M04 | LOTO, อ่านแบบ, PNP/NPN sink-source | wiring gripper/interface และความปลอดภัย |
| **M09 (นี่)** | calibration, KRL, robot interlock | **M10** (เปลี่ยน I/O เป็น fieldbus), **M11/M12** (Capstone Pick & Place) |

**ไม่ซ้ำซ้อน:** fieldbus เจาะลึกใน M10 — ที่นี่เน้น hard-wired I/O (พื้นฐานที่ต้องแน่นก่อน) และให้ภาพรวม fieldbus เท่านั้น

---

## วัตถุประสงค์การเรียนรู้

เมื่อจบโมดูล ผู้เรียนสามารถ:
1. อธิบายโครงสร้างระบบ KUKA และอ่าน datasheet จริง (payload/reach/repeatability/axis range)
2. สาธิตระบบความปลอดภัย: operating modes, E-stop, enabling switch, zone monitoring ตาม ISO 10218
3. Power up/shutdown และ mastering/referencing แกนได้ พร้อมรู้ว่าเมื่อใด master หาย
4. Jog axis-specific และ Cartesian (World/Base/Tool) อย่างปลอดภัย
5. แยกแยะ frame: World/Robroot/Base/Flange/Tool และเลือกใช้ได้
6. ทำ TCP (XYZ 4-point + ABC) และ Base (3-point) calibration พร้อมตรวจ error < 1mm
7. Teach point + PTP/LIN/CIRC + approximation และเข้าใจ Status & Turn
8. เขียน KRL เบื้องต้น (.src/.dat, motion, I/O, flow, subprogram, BCO run)
9. ต่อ I/O และทำ handshake interlock KUKA ↔ Mitsubishi PLC
10. ออกแบบ/เขียน pick & place เต็มรอบพร้อม interlock
11. Troubleshoot ปัญหาพื้นฐานอย่างเป็นระบบ

---

## บทที่ 1 — ภาพรวมระบบ, อ่าน datasheet/manual และความปลอดภัย (6 ชม.)

### ส่วนประกอบระบบ
```
[Manipulator 6-axis]──motor/data cable──[KR C4/C5 Controller]──cable──[smartPAD]
   A1..A6 (servo+brake+encoder)            CCU, KPP, KSP drive
                                           X11 / X121 = peripheral / safety interface
```
- แต่ละแกน = servo motor + holding brake + resolver/encoder (เชื่อมความรู้ M08)
- **X11** = standard peripheral I/O, **X121** = safety interface (E-stop, enabling วงจร safety)

### อ่าน datasheet จริง (ทักษะหน้างาน)
| ค่า | ความหมาย | ใช้ทำอะไร |
|---|---|---|
| Payload | น้ำหนักสูงสุดที่ปลายแขน | เลือก gripper + ชิ้นงานไม่เกิน |
| Reach | ระยะเอื้อมสูงสุด | วาง layout fixture |
| Repeatability (±0.03–0.05mm) | กลับจุดเดิมแม่นแค่ไหน | ประเมินงาน precision |
| Axis range/speed A1–A6 | ขอบเขต/ความเร็วแต่ละแกน | หลีกเลี่ยง limit/singularity |
| Mounting position | floor/ceiling/wall | ตั้ง $WORLD ถูก |

### Operating Modes (จุดพลาดยอดฮิต)
| Mode | ความเร็ว | ใช้เมื่อ |
|---|---|---|
| **T1** | TCP **จำกัด 250 mm/s** | teach, ทดสอบ, อยู่ในรั้วได้ (ถือ enabling) |
| **T2** | manual full speed | ทดสอบความเร็วจริง (เสี่ยงสูง ใช้เฉพาะกรณีพิเศษ) |
| **AUT** | full speed | รันอัตโนมัติ start จาก smartPAD |
| **AUT EXT** | full speed | รันอัตโนมัติ start จาก PLC (โหมดสายการผลิตจริง) |

> ⚠️ **เข้าใจผิดบ่อย:** T1 ไม่ได้จำกัด axis speed — มันจำกัด **ความเร็ว TCP 250 mm/s** ปลายแขนยังทำให้เจ็บได้

### Enabling switch 3 ตำแหน่ง
```
ปล่อยมือ (release) ─→ STOP
กดค้างกลาง (center)─→ ENABLE  ← ตำแหน่งเดียวที่หุ่นยนต์ขยับได้
กดสุด (panic)      ─→ STOP
```

### Safety Checklist ก่อนเข้าพื้นที่
- [ ] LOTO บริเวณ / ยืนยันกับทีม
- [ ] mode = T1
- [ ] enabling switch อยู่ในมือ
- [ ] ตรวจ fence / light curtain ทำงาน
- [ ] payload + tool ตรง datasheet
- [ ] รู้ตำแหน่ง E-stop ที่ใกล้ที่สุด

มาตรฐาน: **ISO 10218-1/2**, cobot อ้างอิง **ISO/TS 15066**

---

## บทที่ 2 — smartPAD, Power up/Mastering, Jog (5 ชม.)

### Mastering — ทำไมและเมื่อใด
Mastering = บอกหุ่นยนต์ว่า "zero position" ของแต่ละแกนอยู่ตรงไหน (เทียบ encoder กับกลไกจริง)

**Master หายเมื่อ:** battery encoder หมด, ถอด motor/encoder, ชนแรง, เปลี่ยน controller
**อาการ:** error "mastering lost" — หุ่นยนต์ไม่ยอม jog Cartesian / รันโปรแกรม
**วิธีทำ:** EMD (Electronic Mastering Device) หรือ comparison/dial method ที่ตำแหน่ง notch ของแต่ละแกน
> ⚠️ ถอด EMD ออกทุกครั้งหลัง master ไม่งั้นชนตอนเคลื่อน

### Jog
- **Axis-specific:** ขยับทีละแกน A1–A6 (ปลอดภัยตอนหุ่นยนต์ใกล้ limit/singularity)
- **Cartesian:** ขยับ X/Y/Z/A/B/C อิงตาม frame ที่เลือก
  - World frame → ทิศคงที่ตามฐาน
  - Tool frame → ทิศตามปลาย tool (มือเอียงไป ทิศก็เอียงตาม)

---

## บทที่ 3 — Coordinate Systems & Calibration (6 ชม.)

### Chain ของ frame
```
World → Robroot → Base ─┐
            └→ Flange → Tool (TCP)
```

### TCP Calibration — XYZ 4-Point
jog ปลาย tool แตะ **reference tip คงที่** จาก 4 ทิศที่ต่างกันมาก ระบบคำนวณตำแหน่ง TCP

### TCP Orientation — ABC 2-Point / ABC World
กำหนดทิศของ tool (Z ชี้ไปไหน)

### Base Calibration — 3-Point
1. **Origin** ของระนาบงาน
2. จุดบนแกน **+X**
3. จุดในระนาบ **+XY**
> เลือกจุดให้ห่างและตั้งฉาก ไม่งั้นทิศเพี้ยน

### ตรวจสอบ (บังคับ)
เลือก Tool frame → re-orient หมุน A/B/C รอบ TCP → **ปลาย tool ต้องนิ่ง** ถ้าขยับ > 1mm = TCP ผิด ทำใหม่

---

## บทที่ 4 — Teach Point & Motion Types (6 ชม.)

| Motion | แนวการเคลื่อน | ใช้เมื่อ |
|---|---|---|
| **PTP** | ไม่ควบคุมแนวปลาย (เร็วสุด) | move ในที่ว่าง |
| **LIN** | เส้นตรงใน Cartesian | approach/pick/place |
| **CIRC** | ส่วนโค้ง (ผ่านจุดเสริม) | งานโค้ง |

### Approximation (CONT)
ทำให้ผ่านจุดแบบ blend ไม่หยุด → ลด cycle time, นุ่มขึ้น
- `C_PTP` (blend PTP), `C_DIS`/`C_VEL`/`C_ORI` (เกณฑ์ blend ของ LIN/CIRC)
> ระวัง: blend ตัดมุม อาจชนถ้า offset ไม่พอ

### Status & Turn (S&T) — จุดที่หลายคนพลาด
PTP ตำแหน่ง XYZ เดียวกัน **แขนวางได้หลายแบบ** (elbow up/down ฯลฯ) ถ้าไม่ล็อก S&T โปรแกรมอาจขยับแขนไม่เหมือนเดิม → ชน ต้องกำหนด S&T ในจุดแรก

### Singularity
**Wrist singularity** เกิดเมื่อ A5 ≈ 0 (แกน A4 กับ A6 เรียงแนวเดียว) → LIN ผ่านจุดนี้ทำให้ A4/A6 กระชาก/error
**แก้:** เปลี่ยนแนวเข้า, ใช้ PTP แทน LIN ช่วงนั้น

---

## บทที่ 5 — การเขียนโปรแกรม KRL เบื้องต้น (7 ชม.)

### โครงสร้างไฟล์
- `.src` = logic, `.dat` = point/data
- โครงสร้าง: `DEF name() … INI … END`, ใช้ **fold** พับโค้ดบน smartPAD

### ชนิดข้อมูลตำแหน่ง
- `E6POS` = X Y Z A B C + S(status) + T(turn) → Cartesian
- `E6AXIS` = A1..A6 → axis
- `FRAME` = X Y Z A B C

### ตัวอย่าง KRL — gripper subprogram + main
```krl
DEF pickplace()
  ; ----- INI -----
  DECL BOOL bGripOK
  BAS(#INITMOV,0)            ; init ความเร็ว/acc พื้นฐาน

  $TOOL = TOOL_DATA[1]       ; เรียก TCP ที่ calibrate
  $BASE = BASE_DATA[1]       ; เรียก Base

  PTP HOME Vel=100% DEFAULT

  WAIT FOR $IN[1]            ; รอ PLC: part_ready
  $OUT[10] = TRUE            ; แจ้ง PLC: busy

  PTP P_APPROACH             ; เหนือชิ้นงาน
  LIN P_PICK                 ; ลงแตะ (เส้นตรง)
  gripper_close()
  IF NOT bGripOK THEN
     $OUT[12] = TRUE         ; error -> PLC
     HALT
  ENDIF
  LIN P_RETRACT              ; ยกขึ้น

  PTP P_PLACE_APP
  LIN P_PLACE
  gripper_open()
  LIN P_RETRACT2

  PTP HOME
  $OUT[10] = FALSE           ; busy off
  $OUT[11] = TRUE            ; done -> PLC
END

DEF gripper_close()
  $OUT[5] = TRUE             ; solenoid close
  WAIT FOR ($IN[3] OR $IN[4])   ; gripped feedback (เพิ่ม logic timeout จริงได้)
  bGripOK = $IN[3]
END

DEF gripper_open()
  $OUT[5] = FALSE
  WAIT SEC 0.3
END
```

> **BCO run:** หลัง select/edit ต้องกด run ค้างจน "Block COincidence" — หุ่นยนต์เคลื่อนเข้าหา point แรกก่อน แล้วถึงรันต่อ **อย่าตกใจตอนมันขยับเข้าจุดแรก**

---

## บทที่ 6 — Gripper Control & PLC Interlock (Hard-wired I/O) (5 ชม.)

### Signal Map (ตัวอย่าง — หัวใจของ interlock)
| สัญญาณ | ทิศทาง | KUKA | PLC (FX5U) |
|---|---|---|---|
| part_ready | PLC→KUKA | $IN[1] | Y0 |
| start cycle | PLC→KUKA | $IN[2] | Y1 |
| ready | KUKA→PLC | $OUT[9] | X0 |
| busy | KUKA→PLC | $OUT[10] | X1 |
| done | KUKA→PLC | $OUT[11] | X2 |
| error | KUKA→PLC | $OUT[12] | X3 |

### Wiring (ผ่าน relay interface)
```
KUKA $OUT[10] (24V) ──→ [Relay coil + flyback diode] ──→ PLC X1
PLC Y0 (24V)        ──→ [Relay coil + flyback diode] ──→ KUKA $IN[1]
```
> ⚠️ ต้องมี **free-wheeling (flyback) diode** คร่อม relay coil กัน back-EMF เผา output card
> ⚠️ **แยก signal กับ power**, แยก shield ground (หลัก M08)

### Ladder ฝั่ง PLC (แนวคิด GX Works3)
```
X0(robot ready) AND part_present  ──( Y0 part_ready )
Y0              ── delay ──        ──( Y1 start )
X2(done)                          ──( reset / next part )
X3(error)                         ──( alarm )
```

### ⚠️ Safety แยกต่างหาก
E-stop / enabling **ห้าม** ต่อผ่าน standard I/O หรือ PLC ธรรมดา — ต้องใช้วงจร **safety-rated** ผ่าน X121 เท่านั้น

### Fieldbus (ภาพรวม → เจาะลึก M10)
PROFINET / EtherNet-IP / CC-Link IE = แทน hard-wired ด้วยสายเดียว แต่ต้องเข้าใจ hard-wired ก่อน

---

## บทที่ 7 — Pick & Place เต็มรอบ + Troubleshooting (5 ชม.)

### Sequence
```
HOME → wait part_ready(PLC) → busy=1 → approach → LIN pick → close+confirm
     → retract → move → LIN place → open → retract → HOME → done=1
```

### Approach/Retract offset
teach จุด pick จริง แล้วทำ approach = pick + 50mm (LIN ลง/ขึ้น) กันชน

### ขั้นตอนขึ้น AUT อย่างปลอดภัย
1. step mode override ต่ำ (ทีละบรรทัด)
2. continuous T1
3. **ออกนอกรั้ว** → AUT
4. AUT EXT (สั่งจาก PLC)

### Troubleshooting Matrix (อ่าน message log บน smartPAD เป็นจุดเริ่มเสมอ)
| อาการ | สาเหตุที่พบบ่อย | วิธีแก้ |
|---|---|---|
| ไม่ขยับหลัง select | ยังไม่ BCO run | กด run ค้างจน BCO |
| "mastering lost" | battery/ชน | re-master ด้วย EMD |
| "software limit" ขณะ jog | แกนเกินขอบเขต | jog axis ออกจาก limit |
| กระชาก/error ขณะ LIN | singularity A5≈0 | เปลี่ยนแนว / ใช้ PTP |
| gripper ไม่จับ | $OUT→relay→valve→air→sensor | ไล่ทีละจุดด้วย multimeter |
| start ไม่เข้า | I/O mismatch | เทียบ signal map กับ I/O monitor |
| "drives not ready" | หลัง E-stop | reset + recover สู่ home |

---

## Hands-on Labs (9 labs)

| Lab | เนื้อหา |
|---|---|
| 1 | ทัวร์ระบบ + อ่าน datasheet/manual จริง + safety check + E-stop test |
| 2 | Power up, Mastering, Jog (axis + Cartesian) |
| 3 | TCP Calibration (XYZ 4-Point + ABC) ตรวจ <1mm |
| 4 | Base Calibration (3-Point) + ตรวจทิศ |
| 5 | Teach PTP/LIN/CIRC + จัดการ Status/Turn |
| 6 | เขียน KRL + ควบคุม gripper ($OUT/$IN) + BCO run |
| 7 | Wiring + signal map handshake KUKA↔PLC + ladder + troubleshoot สายถอด |
| 8 | Pick & Place เต็มรอบ + interlock PLC + วัด cycle time |
| 9 | **Troubleshooting Drill** (timed) 7 กรณี จาก message log |

---

## การประเมินผล

| ภาค | สัดส่วน | เกณฑ์ |
|---|---|---|
| ทฤษฎี | 30% | ≥70% |
| ปฏิบัติ (calibration + KRL + pick&place) | 50% | ≥70%, TCP <1mm |
| Troubleshooting (จำลอง ≥4/7 กรณี) | 20% | ≥70% |
| **Safety gate** | — | unsafe act = ตกทันที |

---

## ข้อผิดพลาดที่พบบ่อย & ความปลอดภัย (สำคัญมาก)
- T1 จำกัด **TCP 250 mm/s** ไม่ใช่ axis speed — ยังอันตราย
- Enabling switch: ปล่อย/กดสุด = หยุด, **กลางเท่านั้น = enable**
- เข้ารั้วต้อง LOTO + ถือ enabling ตลอด
- ห้าม run AUT ขณะยืนในระยะแขน / ออกนอกรั้วก่อน
- ลืม BCO run → งงว่าไม่ขยับ
- ไม่ตรวจ TCP (re-orient) → LIN/CIRC ผิดแนว ชน
- payload/load data ผิด → overload, แม่นลด, brake สึก
- Base 3-point ใกล้/ไม่ตั้งฉาก → ทิศเพี้ยน
- safety signal ห้ามปนกับ standard I/O (ใช้ X121 safety เท่านั้น)
- ลืม flyback diode คร่อม relay coil → output card เสีย
- jog LIN ผ่าน singularity (A5≈0) → กระชาก/error
- ลืมถอด EMD หลัง master → ชน

---

## เชื่อมโยงสู่ Capstone
M09 → **M11** (System Integration) → **M12** (Capstone Pick & Place ทั้งระบบ)
หุ่นยนต์รับงานจาก PLC (M06) ผ่าน handshake (Lab 7-8); **M10** ยกระดับจาก hard-wired I/O เป็น fieldbus (PROFINET/CC-Link IE) ที่กล่าวในบทที่ 6 — ทักษะ calibration, KRL, interlock, troubleshooting ที่นี่คือแกนของสถานี robot ใน Capstone