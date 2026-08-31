# รายงานสังเคราะห์: สิ่งที่ต้องปรับปรุงก่อนเปิดสอน
**หลักสูตร Industrial Mechatronics Practitioner (FX5U + GOT + MELSERVO + KUKA + Modbus/CC-Link)**
สังเคราะห์จาก audit 4 มุมมอง (teach-ready / consistency / gap-priority / launch-blockers) — รวมข้อซ้ำจาก ~35 ประเด็นเหลือ 18 ข้อ

---

## ภาพรวม 1 ย่อหน้า

ชั้น **curriculum/syllabus แข็งแรงมากแล้ว** (objectives, โครงสร้างชั่วโมง, rubric, safety gate, equipment list, instructor guide) — ช่องว่างกระจุกอยู่ 3 จุด: (1) **teaching material ที่ใช้สอนจริง ≈ ศูนย์** (ไม่มีใบงาน ~100 labs, ไม่มีไฟล์ .gx3/GT/KRL, ไม่มีแบบไฟฟ้า CAD, ไม่มีข้อสอบ, ไม่มี fault library), (2) **เอกสารขัดแย้งกันเอง** (ชั่วโมงรวม 5 ค่าใน 5 ไฟล์, prerequisite ไม่ตรงกัน 6 โมดูล, rubric Capstone 3 ระบบ), (3) **ตัวเลขธุรกิจยังไม่เป็นจริง** (ไม่มี BOM ราคาจริง, break-even, สถานที่, ตัวผู้สอน, กฎหมาย/ประกัน) หลักการแก้: **ไม่ต้องรอครบ 14 โมดูลค่อยเปิด** — ทำของสัปดาห์ 1–4 ให้ครบ แล้วผลิตล่วงหน้า 2–3 สัปดาห์ระหว่างรุ่นแรกเดิน

> **มิติใหม่ (โมเดลส่งมอบ): Online เป็นแกน + Remote Lab + Offline พรีเมียม + On-site ต่างจังหวัด** — เพิ่ม 2 launch-blocker: (ก) **Remote Lab ต้องพร้อมและปลอดภัยก่อนเปิดโหมด online** (ข้อ 1.8 ใหม่) และ (ข) การตัดสินใจ scope ต้องเลือก **โหมดของรุ่นแรก** ด้วย (ข้อ 1.1 ขยาย) · ข้อดีเชิงกลยุทธ์: **รุ่นแรกแบบ Online programming (M06+M07) + Remote Lab เล็ก ๆ ใช้ทุนต่ำกว่าและเปิดได้เร็วกว่า** คลาส on-site เต็มรูป จึงเป็นตัวเลือกเปิดตัวที่ควรพิจารณา

---

## กลุ่มที่ 1: [ต้องทำก่อนเปิดรุ่นแรก]

### 1.1 ตัดสินใจ scope + โหมด + ราคา + ปฏิทินรุ่นแรก (การตัดสินใจ ไม่ใช่งานเขียน)
- **ปัญหา:** ยังไม่ได้เลือกว่ารุ่นแรกขายอะไร (คอร์สสั้น M00–M08 vs Full Bootcamp ถึง M12 — ต่างกัน ~1.8–4.8M vs 700k–1.5M), ราคาเป็นช่วงกว้าง 2 เท่า (Bootcamp 80k–180k), ไม่มีวันเปิด/รูปแบบเรียน (กลุ่มเป้าหมาย "ช่างมีงานแล้ว" เรียนได้แค่เสาร์–อาทิตย์ แต่ตารางหลักเป็น full-time 16 สัปดาห์) และตาราง D สัปดาห์ 16 อัด 46 ชม. ยังไม่ resolve · **เพิ่มมิติใหม่: ต้องเลือกโหมดของรุ่นแรกด้วย (Online / Offline / Hybrid / On-site)** — แต่ละโหมดต่างกันที่ทุน, ความพร้อม Remote Lab, และตลาด
- **วิธีแก้:** เลือก product + **โหมด** แรก มี 2 เส้นทางแนะนำ:
  - **เส้นทาง A (ทุนต่ำ/เร็ว): Online PLC+HMI (M06+M07) + Remote Lab** — ต้องมีแค่ rig รีโมต 1–2 ชุด + ชุด Remote Lab (50k–150k) ไม่ต้องรอ 6 station ครบ, เปิดรับได้ทั่วประเทศ, fix ราคา online 2–3 SKU (เช่น 9,900 / early bird 7,900 + Safety online ฟรี)
  - **เส้นทาง B (พรีเมียม): Offline PLC+HMI 10 วัน เสาร์–อาทิตย์ 5 สัปดาห์** (พร้อมสุดใน D §4.4) → ราคา offline (เช่น 18,900 / early bird 14,900) + Safety lead magnet 990
  - หรือ **Hybrid** = ขาย A ก่อน แล้ว upsell block ทักษะมือ · ทุกเส้นทาง: เงื่อนไขผ่อน/มัดจำ/refund 1 หน้า + ระบุ PPE/Remote Lab รวมหรือไม่ + กำหนดวันเปิด/deadline + เพิ่มบัฟเฟอร์ตาราง bootcamp เป็น 17–18 สัปดาห์
- **Effort:** 2–3 วัน

### 1.2 แก้ความขัดแย้งในเอกสารทั้งหมด (single source of truth)
- **ปัญหา (รวม 8 ประเด็น consistency):** ① ชั่วโมงรวม 5 ค่าใน 5 ไฟล์ (396/424/428/444/456/464/496) ทั้งที่ผลบวกจริงจากไฟล์โมดูล = **core 424 / รวม M13 456** ② สารบัญระบุ M05=28, M13=24 (ที่ถูกคือ 32/32) ③ prereq M10 ขัดกัน (M06,M07 vs M06,M08) → **ทำเส้นทางใบรับรอง L3 ใน E §5.1 เดินไม่ได้** ④ prereq M06/M08/M09/M11/M13 ไม่ตรงกันระหว่าง Master/สารบัญ/ไฟล์โมดูล ⑤ M13 อ้าง MR-J5/FR-E800 "จาก M08" แต่ M08 ใช้ MR-JE/J4 + FR-D700 ⑥ Capstone มี rubric 3 ระบบขัดกัน (C: 140/200=70% ผ่าน vs E: ≥75% vs M12: gate-based) — คนได้ 70–74% ผ่านตาม C แต่ตกตาม E ⑦ FR-E700 อ้างใน M05/M08 แต่ไม่มีใน A ⑧ dangling reference "field hands_on_labs" ใน M02
- **วิธีแก้:** ยึดไฟล์โมดูลเป็นฐาน: ชั่วโมง = 424/456 แก้ทุกไฟล์ (~10 จุด) · M10 prereq = **M06, M07** (Modbus คุม FR ไม่ต้องผ่าน servo ก่อน — เพื่อรักษา L3) · M13 = Elective, prereq M12 · ทำตาราง prerequisite ฉบับเดียว sync กลับ Master/สารบัญ · เลือก rubric Capstone ฉบับ 200 คะแนนของ C, ปรับ Competent = 150/200 (75%) และระบุใน E ว่า M12 ใช้ gate-based แทนสูตร 30/50/20 · ตัด E700, แก้ M13 ให้รับจาก M12/M10
- **Effort:** 3–4 วัน

### 1.3 BOM ราคาจริง + license quote + break-even + สถานที่ (ชุดตัวเลขการเงิน)
- **ปัญหา:** งบลงทุนยังเป็นช่วงประมาณการ (700k–1.5M กว้าง 2 เท่า), นิยาม Minimum Lab ขัดกันระหว่าง A §7 (รวม KUKA+rig) กับ F §5.1 (เลื่อนไป Phase 2), ไม่มีราคา license GX Works3/GT Works3 ต่อ seat + FluidSIM ยังพึ่ง trial, **ไม่มี break-even model เลย** (ไม่มี fixed cost/เดือน, contribution margin ต่อ seat), และ**ไม่มี facility requirement** (พื้นที่ ตร.ม., ไฟ 3-phase 380V, compressor, พื้นที่ปลอดภัย KUKA ตาม ISO 10218, ค่าเช่า — recurring cost ก้อนใหญ่สุดที่หายจากแผน)
- **วิธีแก้:** ทำ BOM เดียวตรงกับ scope ข้อ 1.1 → ขอใบเสนอราคาจริงจาก Mitsubishi distributor (hardware + license ถาม education/training-center license พร้อมกัน, seat = 6 PC) → **เพิ่มบรรทัด Remote Lab ใน BOM** (กล้อง IP, Lab PC/gateway, remote access, safety คุมทางไกล, เน็ต/UPS ≈ 50k–150k ตาม A §จ / F §5.1) → ทำ Facility Requirement Sheet (~150–250 ตร.ม. / 6 station + robot cell + **โซน rig รีโมตที่กั้นแยก**) + สำรวจทำเล 2–3 แห่ง → ทำ break-even sheet 3 scenario (**online ล้วน / ผสม / corporate+on-site นำ** — online มี contribution margin ต่อ seat สูงเพราะไม่มีต้นทุนวัสดุ) เพิ่มเป็น section ใหม่ใน F และ sync A §7 ↔ F §5.1
- **Effort:** 2–3 วันงานเขียน + รอ quote/สำรวจ 1–2 สัปดาห์ (คู่ขนาน)

### 1.4 Instructor plan + กฎหมาย/ประกัน
- **ปัญหา:** มี ratio (Lead 1 + TA 1 : 12 คน) แต่ไม่มีตัวบุคคล, คุณสมบัติขั้นต่ำ, ค่าตอบแทน — คนเก่งทั้ง Mitsubishi+KUKA+Modbus หายากมาก และ F ระบุความเสี่ยง "วิทยากร single point" เองแต่ไม่มีแผน · grep ทั้งโปรเจกต์**ไม่พบคำว่า ประกัน/จดทะเบียน/ใบอนุญาต** ทั้งที่ lab มีไฟ 380V + หุ่นยนต์ = ความเสี่ยงถึงชีวิตและความรับผิดผู้สอน
- **วิธีแก้:** instructor plan 1 หน้า (แยกคนสอน robot ออกจากคนสอน PLC ได้, อัตรา ~5,000–15,000 บาท/วัน ใส่ fixed cost, สัญญากันลาออกกลางรุ่น, งบวิทยากร CPR ภายนอกสำหรับ M00) · ปรึกษาบัญชี/ทนาย 1 ครั้ง: โครงสร้างนิติบุคคล + ประกันอุบัติเหตุกลุ่ม + public liability + ศึกษาขึ้นทะเบียนกรมพัฒนาฝีมือแรงงาน (จุดขาย B2B — ลูกค้าเบิกลดหย่อน 200%)
- **Effort:** เขียน 2–3 วัน / recruit + ดำเนินการจริง 2–4 สัปดาห์ (คู่ขนาน)

### 1.5 สร้างสถานีฝึก prototype + ล็อกการตัดสินใจทางเทคนิคที่ผูกกับจัดซื้อ
- **ปัญหา:** มี equipment list แต่ไม่มีแบบ build (panel layout, DIN rail, terminal assignment) และ**ยังไม่ได้ตัดสินใจ servo architecture** (pulse train MR-JE-A/J4-A vs SSCNET FX5-40SSC-S+MR-J4-B) ซึ่ง wiring/programming ต่างกันสิ้นเชิง — เขียน lab sheet M08 ไม่ได้จนกว่าจะล็อก
- **วิธีแก้:** ล็อกสเปก servo architecture ก่อนสั่งซื้อ → สร้าง prototype สถานีฝึกมาตรฐาน 1 สถานี (+ P&P rig ถ้า scope ถึง M12) พร้อมทำ drawing/BOM as-built — as-built นี้กลายเป็น "แบบจริง" ให้ผู้เรียนใช้ใน M01/M12 ไปในตัว
- **Effort:** ตัดสินใจ 1 วัน / build 3–4 สัปดาห์หลังของมาถึง (คู่ขนาน)

### 1.6 Teaching material ขั้นต่ำสำหรับสัปดาห์ 1–4 (M00–M02)
- **ปัญหา:** ถ้าเปิดพรุ่งนี้ครู improvise ~100% — ไม่มีใบงาน, slide, แบบไฟฟ้า, ข้อสอบ, ฟอร์ม แม้แต่ชิ้นเดียว ทั้งที่ D §6.2 บังคับเองว่า "ทุก lab มีใบงาน 1 หน้า"
- **วิธีแก้ (เรียงตาม quick-win):**
  1. **ฟอร์ม sign-off พิมพ์ได้** ~10–12 แบบ (Safety Gate, Skill Passport, Pre-Power-On, competency checklist, เปิด-ปิดแล็บ) — เนื้อหาเสร็จแล้ว แค่แปลงเป็น docx/PDF + เลขเวอร์ชัน → **1–2 วัน**
  2. **Manuals ตรงรุ่น** — PDF ที่มีผิดรุ่น (FX legacy + Q/L) ดาวน์โหลดฟรีจาก Mitsubishi FA: FX5U Hardware, iQ-F Programming, GX Works3, GT Designer3, MR-JE/J4, FR-D700 + KUKA KSS จัดโฟลเดอร์ Manuals/ → **ครึ่งวัน**
  3. **ใบงาน lab M00–M02** จาก template มาตรฐานเดียว เริ่ม safety-critical: M00 Lab 1 (LOTO+live-dead-live), M02 Lab 0/2/3 + ลบ dangling ref ใน M02 → **~15–20 ใบ × 2–4 ชม. ≈ 8–10 วัน**
  4. **แบบไฟฟ้า CAD ชุดแรก** (QElectroTech ฟรีได้): DOL+Reversing M02 + แบบสถานีฝึกจาก 1.5 + เวอร์ชันฝัง error สำหรับ drill → **3–4 วัน**
  5. **Slide M00** (arc flash, PPE, CPR ต้องใช้ภาพจริง) + โปสเตอร์ LOTO/การ์ด NPN-PNP/glossary → **3–4 วัน**
  6. **ข้อสอบ M00 + pre-assessment** (เนื้อหาต้นทางละเอียดพอ แปลงได้ทันที) → **2 วัน**
- **Effort รวม:** ~18–22 วัน (แบ่งทีมทำคู่ขนานได้)

### 1.7 หน่วยความปลอดภัย/commissioning ที่เป็น prerequisite ของการจ่ายไฟครั้งแรก
- **ปัญหา:** จุดเสี่ยงพัง/อันตรายสุดของรุ่นแรก: จ่ายไฟโดยไม่ megger, jog โดยไม่เช็ค E-stop/limit — commissioning มี hook ใน M10–M12 แต่ไม่มี checklist กลาง และ machine safety ภาคปฏิบัติ (E-stop ตัด power จริง, dual-channel, LOTO ก่อนซ่อม) ยังไม่เป็น lab บังคับ
- **วิธีแก้:** Commissioning Checklist กลาง 1 ชุด (pre-power-on + power-on 6 ขั้น) ใช้ซ้ำทุกโมดูล + ขยาย M00 (LOTO + machine safety concept) + lab wiring safety circuit จริงใน M02
- **Effort:** 4–5 วัน

### 1.8 Remote Lab พร้อม + ปลอดภัย (prerequisite ของการเปิดโหมด Online) — ใหม่
- **ปัญหา:** โมเดลใหม่ให้ **Online เป็นแกน** โดยพึ่ง Remote Lab (ผู้เรียนคุมเครื่องจริงทางไกล) แต่ยังไม่มีระบบจริง — ถ้าเปิด online โดยไม่มี Remote Lab ก็เหลือแค่ simulator/วิดีโอ (เสีย USP) และถ้าเปิดโดย **safety คุมทางไกลไม่ครบ = เสี่ยงอุบัติเหตุกับเครื่องจริง** (servo/robot วิ่งโดยไม่มีคนคุมหน้างาน)
- **วิธีแก้:** ก่อนเปิด online รุ่นแรก ต้องมี **ชุด Remote Lab ขั้นต่ำ** (A §9.3): Lab PC + กล้อง 2 มุม/rig + remote access + VPN/VLAN + **safety layer ครบ** (enclosure, remote/local E-stop, watchdog safe-state, speed limit, remote power cut, session log — A §9.2 / D §1.6) + ระบบจอง slot → **ซ้อม dry-run** ให้ผู้ทดสอบรีโมตเข้าเขียน PLC จริง ดูกล้อง และกด E-stop ได้จริง · กำหนด SLA/แผน fallback เมื่อเน็ต/กล้องหลุด
- **Effort:** ตั้งค่า+ทดสอบ ~4–6 วัน (หลังของถึง) + ตัดสินใจสถาปัตยกรรม remote access 1 วัน

**รวมกลุ่ม 1: ~40–52 วันงาน (คนเดียว, รวม Remote Lab ถ้าเปิด online) → บีบเหลือ 6–8 สัปดาห์ปฏิทินถ้าทำคู่ขนาน 2–3 คน**

---

## กลุ่มที่ 2: [ทำระหว่างสอนรุ่นแรก] — ผลิตล่วงหน้าโมดูลละ 2–3 สัปดาห์

### 2.1 ใบงาน + ไฟล์โปรแกรม + แบบไฟฟ้า รายโมดูล (M03→M12 ตามลำดับสอน)
- **ปัญหา:** labs ที่เหลือ ~75–85 ใบ ไม่มีใบงาน · โค้ดทั้งหมดเป็น snippet ใน markdown — ไม่มี starter .gx3, เฉลยครู, เวอร์ชันฝัง bug (แกนประเมิน M06 Lab 8 / M07 Lab 7), KRL .src/.dat, parameter export MR/FR · แบบไฟฟ้า M05/M06/M08/M09 ยังเป็น ASCII art — กิจกรรม M12 บทที่ 2 "หา error ในแบบ" รันไม่ได้เลย
- **วิธีแก้:** ต่อ lab สร้าง 3 ไฟล์ (starter / เฉลย / ฝัง bug + บันทึกตำแหน่ง bug) verify บน hardware จริง, เก็บ repo ตาม naming convention ที่ M06 บทที่ 8 สอนเอง · แบบ CAD เพิ่ม: Star-Delta (M05), I/O FX5U (M06), CN1/CN2/CN8 (M08), robot interface (M09), full set P&P rig (M12) รวม ~15–25 แผ่น
- **Effort:** ต่อเนื่อง ~2–3 วัน/โมดูล × 10 โมดูล ≈ 25–30 วัน

### 2.2 Fault Library + คลังข้อสอบรายโมดูล (ฝั่งครู)
- **ปัญหา:** fault-injection เป็นกลไกประเมินหลัก (M06 "bug 5–6 จุด", M07 6 เคส, M09 7 กรณี timed, M12 2 รอบ) แต่ไม่มีเอกสารครูว่า inject อย่างไรให้ปลอดภัย/ย้อนกลับได้, expected symptom, root cause, เกณฑ์เวลา — ครูใหม่/TA รัน drill ไม่ได้ มาตรฐานระหว่างรุ่นไม่เท่ากัน · ข้อสอบทฤษฎี (น้ำหนัก 30% ทุกโมดูล) ยังไม่มีสักชุด
- **วิธีแก้:** การ์ด fault 1 หน้า/ตัว (~40–60 การ์ด) เริ่มจากที่ไฟล์ระบุแล้ว (M12: encoder หลวม/IP ผิด subnet/parameter ผิด) · question bank 30–50 ข้อ/โมดูล + exit ticket รายวัน — ทยอยทำนำหน้าการสอน
- **Effort:** fault library ~8–10 วัน + ข้อสอบ ~1 วัน/โมดูล

### 2.3 เติม curriculum gaps เชิงเนื้อหา (ทันสอนถ้าเริ่มตอนรุ่นแรกอยู่ช่วง M03–M05)
- **ปัญหา/วิธีแก้ (5 หน่วย เรียงตาม deadline ในตาราง):**
  | หน่วย | สอนก่อนถึง | เนื้อหา | Effort |
  |---|---|---|---|
  | Digital Logic bridge (M05.5) | M06 | binary/hex, Boolean, X/Y/M/D — ไม่งั้น M06 40 ชม. เสียเวลาสอนแทรก | 2 วัน |
  | Troubleshooting methodology | M05/M06 | half-split, symptom→hypothesis→test, multimeter/megger + fault card ทุกโมดูล M02–M11 | 6–7 วัน |
  | Motion fundamentals | M08 | open/closed loop, encoder, pulse=ระยะทาง, PID เชิงสัญชาตญาณ | 2–3 วัน |
  | **จัดโครง M09 ใหม่** | M09 | safety/T1-T2-AUT/enabling switch → frames/TCP → jog/teach → inline form → ค่อย KRL + pre-robot safety checklist (ความเสี่ยงถึงชีวิต, effort ต่ำสุดในกลุ่ม — แค่เรียงใหม่+เพิ่ม 8–10 ชม.) | 2 วัน |
  | Networking fundamentals + comm-loss lab | M10 | IP/subnet, Modbus register model, RS-485 termination, timeout→safe-state | 3–4 วัน |

### 2.4 ยกระดับ M11/M12: Sequential Control + Alarm Design + Backup/Restore + Phase-gate Capstone
- **ปัญหา:** ถ้าไม่ทำ Capstone จะเป็น spaghetti ladder ที่ debug ไม่ได้ และรุ่นแรกจะจบแบบครู "ทำให้ดู" · backup/restore เป็นข้อ impact/effort คุ้มสุดในลิสต์ (procedural ล้วน ไม่ต้องซื้ออะไร)
- **วิธีแก้:** หน่วย Sequential Control & Machine State Design ต้น M11 (step sequencer/SFC, mode auto-manual-step, recovery/resume) + FB/naming ท้าย M06 (5–6 วัน) · alarm design pattern 1 ชุด (first-fault, warning/fault/E-stop, alarm history บน GOT) + alarm list ขั้นต่ำเป็น acceptance ของ Capstone (3 วัน) · backup/restore ย่อยใน M06–M09 + recovery drill "อุปกรณ์พังเปลี่ยนใหม่" (2–3 วัน) · ปรับ M12 เป็น 7 phase-gate + acceptance checklist + ตัดสินใจขยายเป็น 60–80 ชม. ซึ่งกระทบตาราง/ราคา (3–4 วัน)
- **Effort รวม:** ~13–16 วัน

### 2.5 ถ่ายวิดีโอระหว่างสอน + LMS + marketing ต่อเนื่อง
- **ปัญหา:** **โมเดลใหม่ให้ online เป็นแกน → LMS + คลังวิดีโอเลื่อนความสำคัญขึ้น (ไม่ใช่แค่ hybrid เสริม)** ยังไม่มีวิดีโอ/LMS/ระบบส่งงานออนไลน์ · marketing มีแค่ funnel concept ไม่มีงบ/timeline/เป้า lead และคลิป demo ติดปัญหาไก่-ไข่ (ยังไม่มี rig) — แต่ **คลิป "คุมเครื่องจริงทางไกล" คือ demo ที่ทรงพลังสุด** ควรถ่ายทันทีที่ Remote Lab พร้อม
- **วิธีแก้:** รุ่นแรกสอนสด + **ถ่ายระหว่างสอน**เป็นคลัง เริ่ม screen-capture GX Works3/GT Designer3 (ต้นทุนต่ำสุด) + คลิป LOTO/CPR สำหรับ pre-course · launch plan 8 สัปดาห์ก่อนเปิด: ads 15,000–30,000/เดือน, เป้า 100–150 lead → 12 ที่นั่ง, demo ใช้ mini-rig ที่มีจริงแทน Capstone เต็ม, lead magnet Safety ฟรีเก็บรายชื่อ
- **Effort:** วางแผน 1 สัปดาห์ + ทำต่อเนื่อง

---

## กลุ่มที่ 3: [Phase 2 ค่อยทำ]

| ข้อ | เหตุผลที่เลื่อนได้ | Effort |
|---|---|---|
| ชุดวิดีโอ flipped classroom เต็มรูป + LMS | รุ่นแรกสอนสด ใช้ฟุตเทจที่ถ่ายสะสมจาก 2.5 | สูง (ต่อเนื่อง) |
| โมดูล Functional Safety เต็ม ~32 ชม. (ISO 13849 PL/PLr, SISTEMA, Safety PLC, STO/SS1, SafeOperation) | ขั้นปฏิบัติครอบคลุมแล้วใน 1.7 | สูง |
| PdM เต็มรูป (vibration/thermography/CMMS/FMEA) + PM practice | ทักษะ troubleshooting หลักครอบคลุมใน 2.3 แล้ว | สูง |
| Structured Text เชิงลึก (IEC 61131-3) | FB/SFC พื้นฐานเข้าไปใน M06/M11 แล้ว | กลาง |
| Logbook/competency tracking เต็มรูป | รุ่นแรกใช้ skill checklist lightweight | กลาง |
| ขึ้นทะเบียนกรมพัฒนาฝีมือแรงงาน | เริ่มศึกษาขั้นตอนตั้งแต่ 1.4 แต่ดำเนินการจริงหลังมีผลรุ่นแรก | 2–4 สัปดาห์ |
| M13 (SCADA/IIoT) ปรับ hand-off + KEPServerEX license จริง | เป็น elective, trial 2 ชม. พอสำหรับ demo | ต่ำ–กลาง |

---

## Roadmap 90 วัน

สมมติฐาน: ทีม 2–3 คน (ผู้สอนหลัก + คนทำสื่อ/แอดมิน), เปิดรุ่นแรก = คอร์ส PLC+HMI (M06+M07) เริ่ม ~วัน 61 · **เลือกโหมดตามข้อ 1.1:** เส้นทาง A = **Online + Remote Lab** (ทุนต่ำ/เร็ว) หรือ เส้นทาง B = **Offline 10 วัน เสาร์–อาทิตย์** — roadmap ด้านล่างรองรับทั้งสอง (คอลัมน์ฮาร์ดแวร์รวมงาน Remote Lab สำหรับเส้นทาง A)

| ช่วง | เอกสาร/หลักสูตร | การเงิน/ธุรกิจ | ฮาร์ดแวร์/สถานที่ | สื่อการสอน | การตลาด |
|---|---|---|---|---|---|
| **วัน 1–14** | แก้ consistency ทั้งหมด (1.2) · ตัดสินใจ scope+ราคา+ปฏิทิน (1.1) · ล็อก servo architecture (1.5) | ทำ BOM → ส่งขอ quote hardware+license · เริ่มปรึกษาทนาย/ประกัน (1.4) | สำรวจทำเล 2–3 แห่ง + Facility Requirement Sheet | ฟอร์ม sign-off ทั้งชุด (1–2 วัน) · โหลด manuals ตรงรุ่น (ครึ่งวัน) · template ใบงานมาตรฐาน | เปิด lead magnet Safety/LOTO ออนไลน์เก็บรายชื่อ |
| **วัน 15–30** | Commissioning Checklist กลาง + ขยาย M00 safety (1.7) · instructor plan + เริ่ม recruit (1.4) | รับ quote → break-even sheet 3 scenario → เคาะราคาสุดท้าย · เซ็นสัญญาเช่า + ซื้อประกัน | **สั่งซื้ออุปกรณ์** (lead time!) · เริ่มปรับปรุงสถานที่ · **สั่งชุด Remote Lab (กล้อง/PC/remote access) ถ้าเปิด online** | ใบงาน M00–M01 + slide M00 + ข้อสอบ M00/pre-assessment + โปสเตอร์ LOTO | เปิดตัวรุ่น 1 + early bird · เริ่ม ads (15–30k/เดือน) |
| **วัน 31–60** | หน่วย Digital Logic bridge + troubleshooting methodology (2.3) · แก้ M12 เป็น phase-gate บนกระดาษ (2.4) | ติดตาม conversion เป้า 100–150 lead → 12 ที่นั่ง · จ้างผู้สอน/TA เซ็นสัญญา | ของทยอยมา → **build สถานีฝึก 6 สถานี** + as-built drawing · ทดสอบ safety circuit ทุกสถานี · **ประกอบ+ทดสอบ Remote Lab + dry-run รีโมต/E-stop (1.8)** | ใบงาน M02 + แบบ CAD ชุดแรก (DOL/Reversing + ฝัง error) · starter/เฉลย/bug .gx3 ของ M06 ชุดแรก · fault card M02/M06 | คอนเทนต์ต่อเนื่อง (คลิป build สถานีฝึก + **คลิปคุมเครื่องทางไกล** = คอนเทนต์ฟรี) · ปิดรับสมัคร ~วัน 55 |
| **วัน 61–90** | **เปิดสอนรุ่น 1** · รัน checklist A §8 ก่อนวันแรก · เก็บ feedback รายสัปดาห์ (exit ticket) | ทบทวน actual cost vs break-even หลังรุ่นเริ่ม · วางแผน SKU ถัดไป (track M06–M08) | บำรุงรักษา + จัดหาอุปกรณ์เฟสถัดไป (servo/GOT เพิ่ม) | ผลิตนำหน้า 2–3 สัปดาห์: ใบงาน+ไฟล์+ข้อสอบ M06–M07 · **ถ่าย screen-capture ระหว่างสอนทุกคาบ** · fault library M07 | เปิด waitlist รุ่น 2 · ใช้ภาพ/รีวิวรุ่น 1 ทำ social proof |

**Milestone ตรวจรับ:** วัน 14 = เอกสารไม่ขัดแย้งกัน + ราคา/**โหมด**/ปฏิทิน fix แล้ว · วัน 30 = สั่งซื้อแล้ว + break-even รู้ตัวเลข + สื่อ M00 ครบ · วัน 60 = สถานี 6 ตัวผ่าน safety test + **Remote Lab ผ่าน dry-run รีโมต/E-stop (ถ้าเปิด online)** + สื่อ M00–M02 ครบ + ที่นั่งเต็ม ≥80% · วัน 90 = รุ่น 1 ผ่านครึ่งทาง + สื่อ M06–M07 พร้อม + คลังวิดีโอเริ่มสะสม

---

*หมายเหตุ: ตัวเลขชั่วโมงอ้างอิง core M00–M12 = 424 ชม. / รวม M13 = 456 ชม. (ผลบวกจริงจากไฟล์โมดูล) — ใช้ค่านี้เป็น single source of truth ในการแก้ทุกไฟล์ตามข้อ 1.2*