# แผนพัฒนาหลักสูตร — Gap Analysis (42 ประเด็น)

จากการตรวจ 3 มุมมอง: ทำงานจริงหน้างาน / pedagogy คนไม่มีพื้นฐาน / ความครบถ้วนเทียบมาตรฐาน Mechatronics


## ระดับความสำคัญ: สูง

### Commissioning & Start-up จริงของเครื่อง (ลำดับการเปิดเครื่องครั้งแรก / Power-on sequence)
- **ทำไมสำคัญ:** หลักสูตรสอน 'การประกอบ' และ 'การเขียนโปรแกรม' แต่ไม่มีหัวข้อ commissioning อย่างเป็นระบบ ซึ่งเป็นทักษะที่แยกคน 'เขียนโปรแกรมเป็น' ออกจากคน 'ทำเครื่องให้เดินได้จริง'. หน้างานจริง ขั้นตอนอันตรายและพังบ่อยที่สุดคือการจ่ายไฟครั้งแรก: ลืม megger ก่อนจ่ายไฟ, จ่าย power servo ก่อนตรวจ wiring, สั่ง jog แกนโดยไม่เช็ค limit/E-stop, มอเตอร์หมุนกลับทาง, phase sequence ผิด. คนที่ไม่เคย commission จะทำ servo/robot ชนหรือมอเตอร์ไหม้ตั้งแต่วันแรก
- **ข้อเสนอแนะ:** เพิ่มหัวข้อ 'Commissioning Procedure' เป็นเนื้อหาบังคับ พร้อม checklist มาตรฐานที่ใช้ซ้ำได้ทุกเครื่อง:

**Pre-power-on checklist**
- [ ] ตรวจ insulation resistance (megger) ก่อนจ่ายไฟ
- [ ] ตรวจ tightness ของ terminal (torque check)
- [ ] verify wiring ตาม drawing ทีละจุด (ring out ด้วยมิเตอร์)
- [ ] ตรวจ phase sequence ก่อนต่อมอเตอร์ 3 เฟส
- [ ] เช็ค E-stop / safety circuit ก่อนจ่าย control power

**Power-on sequence แบบเป็นขั้น**
1. จ่าย control power เปล่า ดู indicator/PLC RUN
2. ตรวจ I/O ทีละจุดด้วย forced/monitor (input ก่อน output)
3. จ่าย power โดย actuator ปลด/แยกโหลด
4. jog แกน servo/robot ด้วย low speed ทีละแกน ตรวจทิศทางและ limit
5. dry run ทั้ง sequence โดยไม่มีชิ้นงาน
6. run จริงด้วยความเร็วลดทอน (override) แล้วค่อยเพิ่ม

ให้ผู้เรียนทำ commissioning เครื่องจริงตั้งแต่ศูนย์อย่างน้อย 1 ครั้งภายใต้ขั้นตอนนี้
- **เพิ่มที่ไหน:** แทรกเป็นหัวข้อหลักใน M12 (Capstone) และสอดแทรก mini-commissioning ตั้งแต่ M08 (servo/inverter) และ M09 (robot)

### Backup / Restore โปรแกรมและพารามิเตอร์ทั้งระบบ (PLC + HMI + Servo + Robot)
- **ทำไมสำคัญ:** นี่คือทักษะ 'ขั้นต่ำของช่างซ่อมบำรุง' ที่หลักสูตรไม่ได้ระบุเลย. หน้างานจริง: PLC พัง/battery หมด, HMI เสียต้องเปลี่ยนเครื่อง, servo amplifier เปลี่ยนตัวต้อง re-load parameter, robot ต้อง restore mastering. ถ้าไม่มี backup และไม่รู้วิธี restore โรงงานหยุดเป็นวัน
- **ข้อเสนอแนะ:** สอน workflow backup/restore ข้ามอุปกรณ์:
- GX Works3: read/write/verify PLC, backup project+parameter+device comment, การจัดการ PLC battery และ retentive data
- GT Designer3: backup screen data, transfer ผ่าน SD/USB
- MELSERVO (MR Configurator2): backup/restore servo parameter, copy parameter ตอนเปลี่ยน amplifier
- KUKA: archive/restore ผ่าน smartPAD/WorkVisual, ความสำคัญของ mastering data
- นโยบาย backup: ตั้งชื่อไฟล์/version/เก็บที่ไหน
- scenario: 'อุปกรณ์พัง เปลี่ยนใหม่ ทำให้เครื่องกลับมาเดินได้'
- **เพิ่มที่ไหน:** หัวข้อย่อยใน M06 (PLC), M07 (HMI), M08 (servo), M09 (robot) และสรุปรวมเป็น 'System Backup & Recovery Drill' ใน M11/M12

### การอ่าน manual / datasheet จริงและการหาคำตอบด้วยตัวเอง
- **ทำไมสำคัญ:** เป้าหมายคือทำงานจริงเทียบเท่าจบ Mechatronics แต่ทักษะที่ทำให้คนโตในงานได้คืออ่าน manual ภาษาอังกฤษเป็นและหาคำตอบเองได้ ซึ่งหลักสูตรไม่มีหัวข้อนี้ชัดเจน. หน้างานไม่มีใครจำ instruction/parameter/error code ได้หมด ต้องเปิด FX5U Programming Manual, MELSERVO parameter list, KUKA system manual, Modbus register map. คนอ่าน manual ไม่เป็นจะติดทุกครั้งที่เจอของใหม่
- **ข้อเสนอแนะ:** เพิ่มทักษะ navigate documentation:
- โครงสร้าง manual ของ Mitsubishi (Hardware/Programming/Instruction/Parameter manual ต่างกันยังไง)
- อ่าน parameter table ของ servo/inverter แปลงเป็นค่าจริง
- อ่าน register/address map สำหรับ Modbus/CC-Link
- ใช้ error code list ของแต่ละ vendor เพื่อ debug
- ศัพท์เทคนิคอังกฤษที่จำเป็น (sourcing/sinking, latch, interlock, homing)
- แบบฝึก: โยนปัญหาที่ไม่ได้สอนในคลาส ให้ไปหาคำตอบจาก manual เอง
- **เพิ่มที่ไหน:** แทรกตั้งแต่ M01 และทำเป็น recurring habit ทุกโมดูล โดยเฉพาะ M06, M08, M09, M10

### Troubleshooting อย่างเป็นระบบ + การใช้เครื่องมือวัด
- **ทำไมสำคัญ:** M12 มีคำว่า troubleshooting แต่อยู่ท้ายสุดและไม่มีการสอนวิธีคิดอย่างเป็นระบบมาก่อน. ในงานซ่อมบำรุงจริง 70-80% ของเวลาคือการหาว่าปัญหาอยู่ที่ไหน (electrical/mechanical/logic/communication). ถ้าไม่มี methodology ผู้เรียนจะเดาสุ่มและเปลี่ยนของมั่ว. ทักษะใช้ multimeter/clamp meter/oscilloscope และอ่าน PLC diagnostics เป็นหัวใจ
- **ข้อเสนอแนะ:** สร้างหัวข้อ troubleshooting ที่สอนกระบวนการ ไม่ใช่แค่เคส:
- divide-and-conquer / half-split, แยกชั้น (field device->wiring->I/O->logic->comm)
- symptom -> hypothesis -> test -> confirm
- ใช้ multimeter/clamp meter/megger/oscilloscope จริง
- ใช้ PLC online monitor / forced I/O / diagnostics ของ GX Works3 ไล่ปัญหา
- อ่าน servo/inverter/robot error code หาสาเหตุ
- ฝึก fault-insertion: ครูแอบสร้าง fault ให้หาเจอภายในเวลาจำกัด
- **เพิ่มที่ไหน:** เริ่มสอน method ตั้งแต่ M05/M06 และทำ fault-finding lab เข้มข้นใน M11; M12 ใช้ประเมินจริง

### Alarm / Fault handling design (ออกแบบระบบแจ้งเตือนและกู้คืน)
- **ทำไมสำคัญ:** โจทย์ระบุชัดว่าต้องการการจัดการ alarm แต่แผนปัจจุบันไม่มีหัวข้อ alarm management ที่เป็นเรื่องเป็นราว. เครื่องจริงทุกเครื่องต้องมี alarm ที่บอกว่าเกิดอะไร ที่ไหน แก้ยังไง. alarm logic, fault latching, reset/acknowledge, recovery sequence และการแสดงผลบน HMI ทำให้เครื่อง maintain ได้จริง
- **ข้อเสนอแนะ:** เพิ่ม alarm & fault handling เป็นหัวข้อบังคับ:
- alarm logic ใน PLC (latch fault, first-fault detection, severity levels)
- แยก warning vs fault vs E-stop และพฤติกรรมเครื่องในแต่ละระดับ
- recovery/reset/acknowledge และ safe-state
- alarm list/history บน GOT (พร้อม comment บอกวิธีแก้)
- map fault จาก servo/inverter/robot/comm มาแสดงรวมที่ HMI
- Pick & Place ต้องมี alarm ครบ (overtravel, servo alarm, air pressure low, robot fault, comm timeout)
- **เพิ่มที่ไหน:** alarm บน HMI ใน M07, alarm logic ใน M06/M11 และบังคับใช้จริงใน M12

### Homing / Origin return, soft limit และ axis safety สำหรับ servo และ robot
- **ทำไมสำคัญ:** M08 และ M09 มี แต่ homing/origin return และการตั้ง soft limit เป็นจุดที่มือใหม่พลาดบ่อยและทำให้แกนชนได้จริง. การกำหนด home position, absolute vs incremental, software over-travel limit และ robot mastering/calibration เป็นพื้นฐานที่ถ้าขาดจะ commission positioning machine ไม่ได้ และ Pick & Place พึ่งความแม่นยำตำแหน่งนี้
- **ข้อเสนอแนะ:** ระบุให้ชัดในเนื้อหา servo/robot:
- homing methods (dog+Z-phase, stopper, absolute encoder) เลือกใช้เมื่อไหร่
- ตั้ง electronic gear / unit conversion ให้ตำแหน่งจริงตรงค่าโปรแกรม
- soft limit / hard limit / E-stop layering
- robot mastering, tool/base calibration, ผลของ mastering หายต่อความปลอดภัย
- positioning accuracy/repeatability ต่อการ pick ชิ้นงาน
- **เพิ่มที่ไหน:** หัวข้อย่อยใน M08 (servo homing/limit) และ M09 (robot mastering/calibration)

### Communication troubleshooting และ network commissioning จริง
- **ทำไมสำคัญ:** M10 สอน Modbus/Ethernet/CC-Link แต่งานจริงปัญหาใหญ่คือ commission แล้วไม่คุย หรือคุยแล้วหลุดเป็นช่วง. ทักษะที่ขาดคือ IP/subnet planning, station/node addressing, ไล่ปัญหา comm (cable, termination resistor RS485, word/byte order, timeout, CRC) และใช้เครื่องมือ (ping, comm monitor, register watch). Pick & Place บูรณาการหลายอุปกรณ์ ถ้า comm ล่มทั้งเครื่องหยุด
- **ข้อเสนอแนะ:** เสริม M10 ด้วยภาคปฏิบัติ commissioning + troubleshooting:
- IP/subnet/gateway planning และ address map รวมทุก node ในตารางเดียว
- วางสาย, termination resistor, shielding/grounding สำหรับ RS485/CC-Link
- Modbus จริง: function code, register/coil mapping, data type, word/byte order, scaling
- debug: ping, comm status bit, error/timeout handling ใน ladder, register monitor
- design comm-loss handling (timeout->safe-state, reconnect)
- lab: ทำให้ PLC<->HMI<->servo<->robot คุยครบ แล้วครูตัด/ทำ fault
- **เพิ่มที่ไหน:** ขยายภาคปฏิบัติใน M10 และผูก comm-loss handling เข้า M11/M12

### Functional safety จริงของเครื่อง (E-stop, safety relay/light curtain, guard, LOTO)
- **ทำไมสำคัญ:** M00 มีความปลอดภัยแต่เน้นส่วนบุคคล/ไฟฟ้าทั่วไป. สิ่งที่ขาดคือ machine safety เชิงออกแบบ: safety circuit ที่แยกจาก PLC logic, E-stop category, safety relay, light curtain/door switch, dual-channel และ LOTO ตอนซ่อม. เครื่องที่มี servo/robot/นิวเมติกส์เคลื่อนไหวแรง ถ้าออกแบบ safety ผิดคืออันตรายถึงชีวิตและผิดข้อกำหนดโรงงาน
- **ข้อเสนอแนะ:** เพิ่มหัวข้อ machine functional safety:
- safety แยกชั้นจาก control logic (อย่าพึ่ง PLC ปกติเป็นตัวหยุดฉุกเฉิน)
- E-stop categories, safety relay, dual-channel monitoring, manual reset
- light curtain / safety door switch / guard interlock การต่อใช้งานจริง
- wiring safety circuit ให้ตัด power servo/robot ได้จริง
- LOTO procedure ก่อนเข้าซ่อม (สำคัญมากสำหรับช่างซ่อมบำรุง)
- Pick & Place ต้องมี safety circuit ที่ผ่านการทดสอบ
- **เพิ่มที่ไหน:** ขยาย M00 (LOTO + machine safety concept), เพิ่มการ wiring safety circuit จริงใน M02/M08 และบังคับใน M12

### ขาดสะพานเชื่อม Digital Logic / ระบบเลขฐาน ก่อนเข้า PLC (M06)
- **ทำไมสำคัญ:** M06 (PLC FX5U/GX Works3) มี prereq แค่ M02 (relay) และ M04 (sensor) แต่การเขียน ladder จริงต้องเข้าใจ bit/word, ระบบเลขฐาน 2/10/16 (binary/decimal/hex), Boolean logic (AND/OR/NOT/XOR), data type (BOOL/INT/WORD/DINT/REAL), และการ addressing (X/Y/M/D, K/H). คนไม่มีพื้นฐานที่ข้ามจาก relay กายภาพไปสู่ logic ในซอฟต์แวร์ 'ทันที' จะงงกับ data register, การ MOV/CMP, timer/counter preset value และ analog scaling ตั้งแต่สัปดาห์แรก ทำให้ M06 40 ชม.ต้องเสียเวลาสอนพื้นฐานเหล่านี้แทรกแบบไม่เป็นระบบ
- **ข้อเสนอแนะ:** แทรกหน่วยย่อย 'Digital Logic & Number Systems for Automation' 8-12 ชม. ก่อน M06: ครอบคลุม binary/hex/BCD, การแปลงฐาน, Boolean algebra + truth table, การ map relay logic เป็น ladder, แนวคิด bit vs word vs register, data type พื้นฐาน, และ analog-to-digital scaling เบื้องต้น ผูกกับของจริงโดยให้แปลงวงจร relay จาก M02 เป็น truth table แล้วค่อยเป็น ladder
- **เพิ่มที่ไหน:** เพิ่มเป็นโมดูลใหม่ M05.5 (หรือหน่วยนำใน M06) คั่นระหว่าง M04/M05 กับ M06 และเพิ่มเป็น prereq ของ M06

### M08 กระโดดสู่ Servo/Inverter ขั้นสูง โดยขาดพื้นฐาน Motion Control & Closed-loop
- **ทำไมสำคัญ:** M08 ชื่อ 'ขั้นสูง' แต่ prereq มีแค่ M06 (PLC) และ M05 (motor DOL/Star-Delta/VFD) ทำให้ผู้เรียนเจอแนวคิดหนักพร้อมกันแบบไม่มีบันได: encoder/resolver feedback, closed-loop vs open-loop, PID/gain tuning, electronic gear ratio, การเลือกโหมด position/speed/torque, pulse train output (FX5U built-in, ต้องใช้ transistor/sink output, MR-J4-A/MR-JE-A) เทียบกับ SSCNET III/H (FX5-40SSC-S กับ MR-J4-B) ผู้ไม่มีพื้นฐานจะไม่เข้าใจว่าทำไมต้องมี home return, ทำไม pulse=ระยะทาง, gain สูงไปเกิด oscillation อย่างไร M05 สอนแค่ on/off + VFD พื้นฐานซึ่งห่างชั้นจาก servo positioning มาก
- **ข้อเสนอแนะ:** เพิ่มหน่วย 'Motion Control Fundamentals' 8-12 ชม. เป็นบันไดก่อน M08: แนวคิด open vs closed loop, encoder/feedback, ความสัมพันธ์ pulse-ระยะทาง-ความเร็ว, จุดประสงค์ของ home return/limit switch, PID เชิงสัญชาตญาณ (P, I, D ทำอะไร) และ 'เลือกสถาปัตยกรรม servo': แยกชัดว่าหลักสูตรจะใช้ pulse train (MR-J4-A/JE-A, FX5U sink output) หรือ SSCNET (FX5-40SSC-S, MR-J4-B) เพราะ wiring และ programming ต่างกันสิ้นเชิง ควรระบุรุ่นให้ตรงกับชุดฝึกที่มีจริง
- **เพิ่มที่ไหน:** แตก M08 เป็นสองส่วน: เพิ่มหน่วยนำ 'Motion Control Fundamentals' ต้นโมดูล หรือสร้าง M07.5 ใหม่; และเพิ่มหมายเหตุเลือกสถาปัตยกรรม servo ใน M08

### ขาดโมดูล Sequential Control / State Machine (GRAFCET/SFC) ก่อน Advanced Programming (M11)
- **ทำไมสำคัญ:** M11 (บูรณาการ) และ M12 (Pick & Place เต็มระบบ) ต้องเขียนลำดับการทำงานของเครื่องที่มีหลายสถานะ (home > pick > move > place > return) พร้อม interlock, mode (auto/manual/step), alarm handling และ recovery แต่ไม่มีโมดูลใดสอน 'วิธีคิดเชิงลำดับ' อย่างเป็นระบบ เช่น state machine, sequence step, GRAFCET/SFC, หรือ structured design ผู้ไม่มีพื้นฐานจะเขียน ladder แบบ spaghetti ที่ debug ไม่ได้ และพังทันทีเมื่อเจอเครื่องจริงที่ต้องมี manual jog, E-stop recovery และ resume from step นี่คือช่องว่างที่ทำให้ 'เขียน PLC ได้' กับ 'เขียนเครื่องจักรได้' แตกต่างกัน
- **ข้อเสนอแนะ:** เพิ่มโมดูล 'Sequential Control & Machine State Design' 24-28 ชม. สอน: state machine/step sequencer pattern บน ladder, SFC ใน GX Works3, การออกแบบ mode auto/manual/single-step, interlock matrix, E-stop & safety state, alarm/fault handling และ recovery/resume ใช้แบบฝึกที่เป็น sequence จริง (เช่น conveyor + cylinder sorting) ก่อนถึง integration
- **เพิ่มที่ไหน:** เพิ่มโมดูลใหม่ระหว่าง M07/M08 กับ M11 (เช่น M10.5) และตั้งเป็น prereq ของ M11

### M09 KUKA Robot ขาดพื้นฐาน Robot Safety, Coordinate Frames และ Jog ก่อน KRL
- **ทำไมสำคัญ:** M09 (40 ชม.) prereq แค่ M08 แล้วเข้า KRL/smartPAD เลย แต่ผู้ไม่มีพื้นฐานหุ่นยนต์ต้องผ่านบันไดสำคัญก่อนเขียนโปรแกรม: ความปลอดภัยหุ่นยนต์ (safety zone, T1/T2/AUT/AUT EXT mode, enabling switch/deadman, E-stop), ระบบพิกัด (World/Base/Tool/TCP, joint vs cartesian), การ jog ด้วย smartPAD, การ teach point, payload/tool calibration การโดดเข้า KRL ก่อนเข้าใจ frame และ jogging ทำให้สอน inline form (PTP/LIN/CIRC) แล้วผู้เรียนไม่เข้าใจว่า point อ้างอิงอะไร และเสี่ยงอันตรายจริงกับแขนกล
- **ข้อเสนอแนะ:** จัดโครงสร้าง M09 ให้มีลำดับชัด: (1) Robot safety & operating modes + smartPAD orientation (2) Coordinate frames & TCP/Base calibration (3) Manual jogging & teaching points (4) Inline form motion (PTP/LIN/CIRC) (5) จากนั้นค่อย KRL programming ควรกันชั่วโมงด้านความปลอดภัยและ frame อย่างน้อย 8-10 ชม. ก่อนแตะโค้ด และเพิ่ม checklist ความปลอดภัยที่ผู้เรียนต้องผ่านก่อนใช้แขนกล
- **เพิ่มที่ไหน:** ปรับโครงภายใน M09 (เพิ่มหน่วยนำ safety/frame/jog) หรือแยกเป็น M09a (พื้นฐาน/ความปลอดภัย) และ M09b (KRL)

### ขาดโมดูล/วิธีการ Troubleshooting & Maintenance ที่เป็นระบบ (ถูกยัดไว้แค่ใน M12)
- **ทำไมสำคัญ:** เป้าหมายคือ 'ช่างซ่อมบำรุงที่ทำงานจริงได้' แต่ troubleshooting ปรากฏแค่ท้าย M12 ปนกับงานสร้างเครื่อง ทำให้ได้เวลาฝึกน้อยและไม่เป็นระบบ การแก้ปัญหาเป็นทักษะที่ต้องสอนวิธีคิดและฝึกซ้ำหลายรอบ: การอ่าน wiring/electrical drawing เพื่อไล่จุดเสีย, การใช้ multimeter/oscilloscope วัดสัญญาณ, การอ่าน PLC diagnostic/error code (GX Works3), servo/inverter alarm code (MELSERVO/FR), robot fault (KUKA), การแยกแยะปัญหา hardware vs software vs communication, และ preventive maintenance ทักษะนี้คือหัวใจของช่างซ่อมบำรุง แต่กลับเป็นช่องว่างที่ใหญ่ที่สุด
- **ข้อเสนอแนะ:** เพิ่มโมดูล 'Systematic Troubleshooting & Maintenance' 24-32 ชม.: methodology (สังเกต-ตั้งสมมติฐาน-แยกส่วน-ทดสอบ), การใช้ diagnostic tool ของแต่ละแพลตฟอร์ม, การอ่าน alarm/error code, fault injection lab (ครูตั้งจุดเสียให้ผู้เรียนหา), การไล่สายจากแบบ, log/SOP การซ่อม, preventive maintenance & spare parts ควรสอนต้นทาง (เช่น troubleshooting วงจร relay/wiring) แล้ว spiral เพิ่มความซับซ้อนทุกโมดูล
- **เพิ่มที่ไหน:** เพิ่มโมดูลใหม่ก่อน M12 และฝังกิจกรรม fault-finding เล็กๆ ในทุกโมดูลตั้งแต่ M01

### Functional Safety / Safety PLC ของเครื่องจักร (ISO 13849-1, PLr, Safety Relay/Safety PLC) ขาดทั้งหมด
- **ทำไมสำคัญ:** M00 สอนแค่ความปลอดภัยส่วนบุคคล (อันตรายไฟฟ้า, PPE, LOTO) แต่ไม่มี 'machine functional safety' ซึ่งเป็นแกนของ Mechatronics สากลและเป็นข้อบังคับตามกฎหมาย/มาตรฐานเครื่องจักร โดยเฉพาะเมื่อหลักสูตรมี servo, inverter และหุ่นยนต์ KUKA ที่เคลื่อนที่เร็วและอันตรายถึงชีวิต ผู้เรียนต้องออกแบบวงจร E-stop ที่ถูก category, เลือก safety relay/Safety PLC, ประเมิน Performance Level (PLr/PLd/PLe) ด้วย SISTEMA, ต่อ light curtain/safety door/two-hand control และตั้ง Safe Zone/SafeOperation ของหุ่นยนต์ ขาดส่วนนี้ = ผู้เรียนสร้างเครื่องที่ผิดมาตรฐานความปลอดภัยและทำงานจริงในโรงงานไม่ได้
- **ข้อเสนอแนะ:** เพิ่มโมดูลใหม่ 'M-Safety: Machine Functional Safety & Safety PLC' (~32 ชม.) ครอบคลุม: ISO 13849-1 (PL/PLr, category B/1/2/3/4), ISO 12100 risk assessment, การคำนวณ SISTEMA, safety relay (เช่น MELSEC-Safety / FX5-SF / WS0 series), การต่อ light curtain/safety mat/door switch แบบ dual channel, STO/SS1 ของ MELSERVO, KUKA SafeOperation/safe zone บน smartPAD พร้อม Lab E-stop category 3/4 จริง วางหลัง M08 (มี servo แล้ว) ก่อน M09 (robot) เป็น prereq บังคับของ M09 และ M12
- **เพิ่มที่ไหน:** เพิ่มโมดูลใหม่ระหว่าง M08 และ M09 (เสนอรหัส M08.5) และกำหนดเป็น prereq บังคับของ M09, M11, M12

### Predictive/Preventive Maintenance, Condition Monitoring และระเบียบวิธี Troubleshooting เชิงระบบ
- **ทำไมสำคัญ:** กลุ่มเป้าหมายคือ 'ช่างซ่อมบำรุง' โดยตรง แต่ทั้งหลักสูตรไม่มีโมดูลบำรุงรักษาเลย Troubleshooting ถูกยัดรวมไว้แค่ใน M12 (Capstone) ตอนปลายทาง ทำให้ผู้เรียนไม่ได้ฝึกทักษะหากินหลักของอาชีพอย่างเป็นระบบ ขาด PM schedule, FMEA, root-cause analysis (5-Why/fishbone), การอ่าน alarm/diagnostics ของ GX Works3/MELSERVO/KUKA, vibration/thermal/insulation monitoring และแนวคิด CMMS/MTBF/MTTR ซึ่งเป็นเนื้อหามาตรฐานของวิศวกรรมซ่อมบำรุง
- **ข้อเสนอแนะ:** เพิ่มโมดูล 'M-Maint: Predictive/Preventive Maintenance & Systematic Troubleshooting' (~32 ชม.): PM/PdM strategy, FMEA & RCA, condition monitoring (vibration analysis เบื้องต้น, thermography, insulation/Megger test, bearing/coupling), การใช้ diagnostic/alarm history ของ FX5U/GOT/MELSERVO/inverter/KUKA, MTBF/MTTR/OEE, ระเบียบวิธี troubleshoot 6 ขั้น และ work order/CMMS เพิ่ม Lab 'จงใจสร้าง fault แล้วให้ผู้เรียนไล่หา' บนชุดฝึก
- **เพิ่มที่ไหน:** เพิ่มโมดูลใหม่ (เสนอ M11.5) วางหลัง M11 ก่อน M12 และนำ mini-troubleshooting lab แทรกย่อยตั้งแต่ M02, M06, M08 ด้วย

### Machine Vision / ระบบมองเห็นด้วยกล้องสำหรับ Pick & Place
- **ทำไมสำคัญ:** Capstone เป็นเครื่อง Pick & Place ซึ่งในอุตสาหกรรมจริงมัก 'vision-guided' (หากล้องระบุตำแหน่ง/มุม/คุณภาพชิ้นงานก่อนหยิบ) หลักสูตร Mechatronics สมัยใหม่ถือ vision เป็นวิชาแกน แต่ที่นี่ขาดทั้งหมด ทำให้ผู้เรียนทำได้แค่ pick ตำแหน่งตายตัว (fixed position) ไม่สามารถจัดการชิ้นงานวางสุ่ม, inspection (วัด/ตรวจตำหนิ/อ่าน barcode/QR) หรือทำ robot-vision calibration ได้
- **ข้อเสนอแนะ:** เพิ่มโมดูล 'M-Vision: Industrial Machine Vision & Inspection' (~28 ชม.): หลักการแสง/เลนส์/กล้อง, smart camera (เช่น Cognex In-Sight หรือ Keyence) หรือ vision sensor, pattern matching/blob/edge/measurement, barcode/OCR, การส่งผลผ่าน Ethernet/Modbus เข้า PLC, vision-to-robot coordinate calibration (hand-eye) ของ KUKA แล้วผูกเป็น option ของ Capstone (vision-guided pick)
- **เพิ่มที่ไหน:** เพิ่มโมดูลใหม่หลัง M10 (สื่อสารแล้ว) ก่อน M11 (เสนอ M10.5) เป็น prereq เสริมของ M12; ถ้างบจำกัดให้เริ่มเป็น Elective คู่กับ M13

### การเขียนโปรแกรมแบบ IEC 61131-3 ขั้นโครงสร้าง (ST, FB, SFC/GRAFCET) และสถาปัตยกรรมโค้ดที่ใช้ซ้ำได้
- **ทำไมสำคัญ:** M06 'Basic PLC' กับ M11 'Advanced Programming' มีแนวโน้มสอน ladder เป็นหลัก แต่ 'ผู้เชี่ยวชาญเทียบ Mechatronics' ต้องเขียนแบบมีโครงสร้าง: Structured Text (ST), Function Block (FB)/FUN ที่นำกลับมาใช้ใหม่, SFC/GRAFCET สำหรับ sequence ของเครื่อง, structured/label programming และ state machine GX Works3 รองรับ FB/ST/SFC เต็มรูปแบบ ขาดส่วนนี้ทำให้โค้ด Capstone เป็น ladder ก้อนใหญ่ดูแลไม่ได้ และไม่ตรงแนวปฏิบัติ OEM จริง
- **ข้อเสนอแนะ:** ขยาย M06 ให้ครอบคลุม IEC 61131-3 (LD/ST/FBD/SFC) และเพิ่มหน่วยใน M11 ว่าด้วย: การออกแบบ state machine/GRAFCET, FB library ที่ reuse ได้, structured project & label/global label, naming convention, commenting, และ version control/backup ของโปรเจกต์ พร้อมมาตรฐาน code review ก่อนนำขึ้น Capstone
- **เพิ่มที่ไหน:** ขยายเนื้อหาใน M06 (เพิ่ม ~8 ชม.) และเพิ่มหน่วย 'software architecture' ใน M11


## ระดับความสำคัญ: กลาง

### Preventive / Predictive Maintenance และ machine lifecycle (มุมช่างซ่อมบำรุง)
- **ทำไมสำคัญ:** กลุ่มเป้าหมายครึ่งหนึ่งคือช่างซ่อมบำรุง แต่หลักสูตรเน้น build & program เกือบทั้งหมด แทบไม่มี maintenance routine. งานจริงคือ PM ตามแผน, เปลี่ยน battery PLC, check torque/insulation ตามรอบ, ดูแล servo brake/encoder, จัดการ spare part, ทำ machine log. ขาดส่วนนี้ผู้เรียนทำงาน maintenance day-to-day ไม่เป็น
- **ข้อเสนอแนะ:** เพิ่มหัวข้อ maintenance practice:
- วาง PM schedule และ checklist ตามอุปกรณ์
- PLC battery / retentive memory, เปลี่ยนโดยไม่เสียโปรแกรม
- ดูแล servo/inverter (cooling fan, capacitor aging, encoder)
- spare part management, MTBF/MTTR เบื้องต้น
- machine documentation / maintenance log / as-built update
- (เชื่อม M13) condition monitoring เบื้องต้น
- **เพิ่มที่ไหน:** เพิ่มเป็นหัวข้อใน M11 หรือสร้างโมดูลใหม่ 'M11.5 Maintenance & Reliability Practice' (~16-24 ชม.)

### Documentation / As-built & handover (ส่งมอบงานแบบมืออาชีพ)
- **ทำไมสำคัญ:** การอ่านแบบมีใน M01 แต่การทำเอกสารและส่งมอบงานไม่มี. งานจริง การแก้ wiring/program แล้วไม่ update drawing = คนถัดไปซ่อมไม่ได้. ทักษะทำ as-built drawing, I/O list, parameter sheet, sequence description, operation manual และ handover แยกช่างมืออาชีพออกจากช่างทั่วไป
- **ข้อเสนอแนะ:** เพิ่มทักษะ documentation เป็นผลงานบังคับ:
- update as-built electrical/pneumatic drawing หลังแก้งาน
- I/O list, tag naming convention, parameter sheet, comm address map
- sequence of operation / state description ของเครื่อง
- operation manual + alarm troubleshooting guide สำหรับ operator
- handover checklist
- บังคับ Capstone ต้องส่ง documentation package ครบ ไม่ใช่แค่เครื่องเดิน
- **เพิ่มที่ไหน:** เริ่มสอน naming/I/O list ใน M06, drawing update ใน M01/M02 และบังคับเป็น deliverable ใน M12

### โครงสร้างโปรแกรม PLC ที่ดูแลรักษาได้ (architecture, state machine, naming, comment)
- **ทำไมสำคัญ:** M06 สอน basic PLC programming และ M11 advanced/integration แต่ไม่ระบุการออกแบบโครงสร้างโปรแกรมให้คนอื่นอ่านและแก้ได้. มือใหม่มักเขียน ladder ก้อนเดียวยาว ไม่มี comment ไม่มี structure ทำให้ troubleshoot และต่อยอดไม่ได้. งานจริงต้องการ modular program, state machine, interlock ชัด, naming/comment อ่านรู้เรื่อง
- **ข้อเสนอแนะ:** เพิ่มหัวข้อ programming best practice:
- program organization ใน GX Works3 (program blocks, FB/FUN, structured ladder/ST เบื้องต้น)
- ออกแบบ sequence ด้วย state machine / step sequencer (เหมาะกับ Pick & Place)
- mode handling (auto/manual/jog/home), interlock matrix
- naming convention, device comment, label ที่ทำให้โปรแกรม self-documenting
- ออกแบบให้ reuse และ scale ได้, code review กันในคลาส
- **เพิ่มที่ไหน:** หลักการใน M06 (naming/comment/step) และเน้นเต็มที่ใน M11 ก่อนเข้า Capstone

### การเลือกและคำนวณอุปกรณ์เบื้องต้น (sizing: breaker/cable/contactor/servo/cylinder)
- **ทำไมสำคัญ:** ผู้เรียนติดตั้งและ wiring เป็น แต่ถ้าไม่รู้ทำไมเลือกขนาดนี้ จะออกแบบ/แก้ปัญหาเองไม่ได้และเลือกของผิดขนาด. sizing breaker/fuse, cable ampacity, contactor/overload, เลือก servo จาก load/inertia, เลือกขนาด cylinder/valve จากแรงและ flow เป็นความรู้ที่ทำให้เป็นผู้เชี่ยวชาญ ไม่ใช่แค่คนต่อสายตามแบบ
- **ข้อเสนอแนะ:** เพิ่มภาค sizing เชิงปฏิบัติ (เน้นสูตรใช้งานจริง):
- breaker/fuse/cable/contactor/overload sizing สำหรับมอเตอร์และวงจรควบคุม
- voltage drop และ short-circuit เบื้องต้น
- servo sizing (load inertia ratio, torque/rpm, duty)
- inverter sizing ตาม motor และ load type
- pneumatic sizing: cylinder force, air consumption, valve Cv, FRL
- ใช้ vendor selection tool/แคตตาล็อกจริง
- **เพิ่มที่ไหน:** power sizing ใน M05, servo/inverter sizing ใน M08, pneumatic sizing ใน M03, control wiring sizing ใน M01/M02

### Grounding, shielding และ EMC/noise management ในงานจริง
- **ทำไมสำคัญ:** ไม่มีหัวข้อ grounding/EMC แยกชัดเจน ทั้งที่เป็นสาเหตุอันดับต้นๆ ของปัญหา 'ผีๆ' ในเครื่องที่มี VFD/servo: sensor อ่านผิด, comm หลุด, analog แกว่ง, PLC reset เอง. คนไม่เข้าใจ grounding/shielding จะ troubleshoot อาการ noise ไม่เจอและวางสายผิดทำให้เครื่อง unreliable
- **ข้อเสนอแนะ:** เพิ่มเนื้อหา grounding & noise:
- single-point grounding, PE/FG แยกอย่างไร, shield termination ที่ถูกต้อง
- วางสาย power/signal แยกราง, การข้ามสายแบบลด coupling
- ผลของ VFD/servo ต่อ noise และวิธีลด (ferrite, line reactor, EMC filter, สาย shielded)
- noise บน analog/encoder/comm และการแก้
- ผูกเข้ากับ panel building และ servo/inverter wiring
- **เพิ่มที่ไหน:** หลักการใน M01 (panel/grounding) และเจาะลึกผลกระทบ+วิธีแก้ใน M08

### การจัดการสัญญาณ analog และ scaling (4-20mA/0-10V, analog I/O)
- **ทำไมสำคัญ:** M04 มี sensor และ M06 มี PLC แต่ไม่ชัดว่าครอบคลุม analog ซึ่งงานจริงเจอตลอด: pressure/temperature/flow/level transmitter, load cell, scale ค่า raw->engineering unit, filter, ตั้ง analog module ของ FX5U. มือใหม่ที่ทำแต่ digital I/O จะ stuck ทันทีเมื่อเจองาน analog
- **ข้อเสนอแนะ:** เพิ่มหัวข้อ analog:
- ชนิดสัญญาณ 4-20mA / 0-10V / RTD / thermocouple และการต่อ
- ตั้ง analog input/output module ของ FX5U/iQ-F
- scaling raw count->engineering unit, offset/gain, filtering
- แสดง/บันทึก analog บน HMI (trend, data logging)
- analog troubleshooting (noise, open circuit, range ผิด)
- **เพิ่มที่ไหน:** M04 (sensor analog) และ M06 (analog module + scaling ใน GX Works3); แสดงผลใน M07

### Servo tuning จริง (gain, inertia ratio, vibration/overshoot)
- **ทำไมสำคัญ:** M08 ครอบคลุม servo control แต่ถ้าไม่เน้น tuning ผู้เรียนจะตั้ง servo ให้หมุนได้ แต่เครื่องสั่น/overshoot/ตำแหน่งไม่นิ่ง ทำให้ Pick & Place หยิบพลาดหรือชน. tuning (auto-tuning, manual gain, inertia ratio, แก้ vibration/resonance) แยกคนใช้ servo เป็นกับคนที่ทำ servo ให้ทำงานดีจริง
- **ข้อเสนอแนะ:** ระบุ servo tuning เป็นหัวข้อย่อยใน M08:
- auto-tuning vs manual tuning, อ่าน response
- inertia ratio estimation และผลต่อ gain
- แก้ vibration/resonance (notch filter, machine resonance)
- ใช้ MR Configurator2 graph/analyzer
- ประเมิน positioning settling time/accuracy
- ฝึก tune แกนจริงจนนิ่งพอสำหรับ pick & place
- **เพิ่มที่ไหน:** หัวข้อย่อยใน M08 (servo)

### การประเมินผลแบบ competency-based และ logbook ทักษะหน้างาน
- **ทำไมสำคัญ:** หลักสูตรมีโมดูล/ชั่วโมง แต่ไม่มีกลไกพิสูจน์ว่าผู้เรียนทำได้จริง. เป้าหมายคือสร้างผู้เชี่ยวชาญที่ทำงานจริงได้ ต้องวัดเป็น competency (ทำงานสำเร็จภายใต้เวลา/มาตรฐาน) ไม่ใช่สอบความรู้. ถ้าไม่มีการวัด hands-on จะไม่รู้ว่าใครพร้อมหน้างาน
- **ข้อเสนอแนะ:** ออกแบบระบบประเมินเชิงสมรรถนะ:
- skill checklist ต่อโมดูล (ทำได้/ทำไม่ได้) ประเมินโดยลงมือจริง
- timed practical test (เช่น commission แกนนี้ภายใน X นาที, หา fault ที่ซ่อนไว้ภายใน Y นาที)
- logbook บันทึกชั่วโมงปฏิบัติและงานที่ทำสำเร็จ
- Capstone rubric: เครื่องเดิน + safety + alarm + documentation + troubleshoot ได้
- mapping competency เทียบ job task จริงในโรงงาน
- **เพิ่มที่ไหน:** โครงสร้างประเมินครอบทุกโมดูล และ rubric เข้มข้นใน M12 (Capstone)

### ขาดโมดูล Networking Fundamentals ก่อน Industrial Communication (M10)
- **ทำไมสำคัญ:** M10 รวม Modbus/Ethernet/CC-Link แต่ prereq มีแค่ M06/M08 ไม่มีจุดใดปูพื้นฐานเครือข่ายเลย ผู้ไม่มีพื้นฐานจะตั้งค่า Ethernet/IP ไม่เป็นถ้าไม่เข้าใจ IP address, subnet mask, gateway, port, แนวคิด master/slave, register map (Modbus 4xxxx/3xxxx, coil/discrete), byte order/endianness, RS-485 vs Ethernet physical layer และ termination/biasing การข้ามไปตั้งค่า CC-Link/Modbus โดยไม่รู้พื้นฐานเหล่านี้ทำให้ debug ปัญหาสื่อสาร (timeout, no response, garbage data) ไม่ได้เลย
- **ข้อเสนอแนะ:** เพิ่มหน่วย 'Networking & Fieldbus Fundamentals' 8 ชม. นำใน M10: IP/subnet/gateway, OSI แบบย่อ, master-slave/client-server, Modbus register model & function codes, RS-485 wiring (A/B, termination, bias), การใช้เครื่องมือ scan/ping และ Modbus poll tester ก่อนเชื่อมอุปกรณ์จริง
- **เพิ่มที่ไหน:** เพิ่มหน่วยนำใน M10 (เพิ่มชั่วโมงเป็น ~40) หรือแยกหน่วยพื้นฐานออกมาก่อน M10

### ขาด Electro-Pneumatics เป็นสะพานเชื่อม Pneumatics (M03) กับการควบคุมด้วย PLC
- **ทำไมสำคัญ:** M03 สอนนิวเมติกส์และควบคุมเบื้องต้น (น่าจะเป็น manual/mechanical valve) แต่ Pick & Place ใน Capstone ต้องให้ PLC สั่ง solenoid valve และอ่าน reed switch/pressure sensor ไม่มีโมดูลใดเชื่อม 'นิวเมติกส์' กับ 'การควบคุมด้วยไฟฟ้า/PLC' อย่างชัดเจน เช่น 5/2, 5/3 solenoid valve, single vs double solenoid, การ wiring solenoid 24VDC ผ่าน PLC output, vacuum gripper/ejector สำหรับงานหยิบจับ ผู้เรียนจึงอาจไปเจอ electro-pneumatics ครั้งแรกตอน Capstone ซึ่งสายเกินไป
- **ข้อเสนอแนะ:** เพิ่มหน่วย 'Electro-Pneumatics' 8-12 ชม.: solenoid valve types (5/2, 5/3, single/double), การควบคุมด้วยรีเลย์แล้วยกระดับเป็น PLC output, reed/pressure sensor เป็น PLC input, vacuum gripper & ejector, การออกแบบวงจร electro-pneumatic ของลำดับ cylinder ผูกกับ sensor feedback
- **เพิ่มที่ไหน:** ขยาย M03 ให้รวม electro-pneumatics (เพิ่มชั่วโมง) หรือเพิ่มหน่วยต่อยอดหลัง M04/M06

### M12 Capstone Pick & Place เพียง 40 ชม. ตื้นเกินไปสำหรับการบูรณาการทั้งระบบ
- **ทำไมสำคัญ:** M12 ต้องประกอบ + เขียนโปรแกรม + บูรณาการ PLC+HMI+Servo+Robot+Communication + troubleshooting ทั้งหมดใน 40 ชม. ซึ่งน้อยมากเมื่อเทียบกับความซับซ้อน (mechanical assembly, wiring ตู้, ตั้งค่า servo home, สอน point หุ่นยนต์, จูน comm ระหว่าง PLC-robot, ออกแบบ HMI, sequence + alarm + recovery, commissioning) สำหรับคนไม่มีพื้นฐาน การให้เวลาน้อยจะทำให้ครูต้อง 'ทำให้ดู' แทนที่ผู้เรียนได้ลงมือจนชำนาญ ขัดกับเป้าหมาย 'ทำงานจริงได้'
- **ข้อเสนอแนะ:** เพิ่มเวลา M12 เป็น 60-80 ชม. และแบ่งเฟสชัดเจน: (1) mechanical & panel build (2) power-on & I/O checkout (3) subsystem commissioning (servo home, robot teach, comm link) (4) sequence programming + HMI (5) integration & dry run (6) fault injection & troubleshooting (7) acceptance test ตาม checklist กำหนด milestone/gate ที่ต้องผ่านก่อนไปเฟสถัดไป
- **เพิ่มที่ไหน:** ขยายชั่วโมงและปรับโครงสร้าง M12 ให้เป็นเฟส พร้อม acceptance checklist

### ขาด Structured Programming (FB/FC, Structured Text, การจัดระเบียบโค้ด) ก่อน M11
- **ทำไมสำคัญ:** M06 เป็น PLC พื้นฐาน (ladder) แต่ M11/M12 ต้องเขียนโปรแกรมเครื่องจักรขนาดใหญ่ที่ maintain ได้ ซึ่งต้องใช้ Function Block (FB), Function (FC), label/structured data, อาจรวม Structured Text สำหรับงานคำนวณ และแนวทางตั้งชื่อ/comment/จัดเลเยอร์โปรแกรม GX Works3 รองรับสิ่งเหล่านี้ แต่หลักสูตรไม่มีจุดสอน ทำให้ผู้เรียนเขียนโปรแกรมใหญ่แบบก้อนเดียวที่ reuse และ debug ไม่ได้ เป็นช่องว่างระหว่าง 'เขียน ladder เป็น' กับ 'ออกแบบโปรแกรมระดับเครื่องจักร'
- **ข้อเสนอแนะ:** เพิ่มหน่วย 'Structured PLC Programming' 12-16 ชม.: FB/FC และการ reuse, label & structured data type, เกริ่น Structured Text สำหรับงานคำนวณ/loop, coding standard (naming, comment, modularization), การแบ่งโปรแกรมเป็น program block ตามฟังก์ชัน ใช้เป็นบันไดสู่ M11
- **เพิ่มที่ไหน:** เพิ่มหน่วยต่อยอดหลัง M06 หรือเป็นส่วนต้นของ M11 (และเพิ่มชั่วโมง M11)

### ขาดการฝึกซ้ำ/Spiral Review — ทักษะช่วงต้น (wiring, drawing, relay) ไม่ถูกนำกลับมาใช้ซ้ำเป็นระบบ
- **ทำไมสำคัญ:** หลักสูตรเป็นเส้นตรง (linear) แต่ละทักษะเรียนครั้งเดียวแล้วไป ผู้ไม่มีพื้นฐานต้องการ retrieval practice และ spacing เพื่อคงทักษะ เช่น ทักษะอ่านแบบไฟฟ้า/wiring จาก M01 ถ้าไม่ได้ใช้ซ้ำในหลายโมดูลจะลืม จนถึง M08/M12 ที่ต้อง wiring ตู้จริงก็ทำไม่ได้ การไม่มีจุด review/integration ระหว่างทาง ทำให้ช่องว่างความรู้สะสมและไประเบิดที่ Capstone
- **ข้อเสนอแนะ:** แทรก 'integration checkpoint' สั้นๆ (4-8 ชม.) ทุก 3-4 โมดูล ที่ผู้เรียนต้องนำทักษะสะสมมาทำมินิโปรเจกต์ (เช่น หลัง M06: relay+sensor+pneumatic+PLC คุม conveyor; หลัง M08: เพิ่ม servo positioning) และกำหนดให้ทุกโมดูลตั้งแต่ M06 ต้อง 'อ่านแบบและไล่สาย' ของชุดฝึกจริงทุกครั้ง เพื่อ spiral ทักษะ M01 ไว้ตลอด
- **เพิ่มที่ไหน:** เพิ่ม mini-integration checkpoint หลัง M06, หลัง M08, หลัง M10 และฝังการอ่านแบบ/ไล่สายในทุกโมดูลปฏิบัติ

### M00 Foundation: ต้องยืนยันว่ามีพื้นฐาน DC/AC, การวัดด้วยเครื่องมือ และคณิตช่างครบสำหรับ 'คนไม่มีความรู้เลย'
- **ทำไมสำคัญ:** เป้าหมายคือคน 'ไม่มีความรู้เลย' M00 24 ชม. รวมทั้งความปลอดภัย ซึ่งอาจไม่พอครอบคลุมพื้นฐานที่จำเป็นต่อทุกโมดูลถัดไป: กฎของโอห์ม, อนุกรม/ขนาน, ความต่าง DC vs AC (single/3-phase), การใช้ multimeter/clamp meter อย่างถูกต้องและปลอดภัย, การอ่านค่าและตีความ, หน่วย SI, คณิตช่างพื้นฐาน (เปอร์เซ็นต์, อัตราส่วน—สำคัญต่อ gear ratio/scaling ภายหลัง) ถ้า M00 ปูไม่แน่น ช่องว่างจะลามไปทุกโมดูล
- **ข้อเสนอแนะ:** ตรวจให้ M00 ครอบคลุมและฝึกมือจริง: Ohm's law + series/parallel, DC vs 1ph/3ph AC, การใช้ multimeter/clamp meter (วัด V/I/R/continuity) อย่างปลอดภัย, การอ่าน nameplate, หน่วยและคณิตช่างพื้นฐาน ถ้าเนื้อหาแน่นเกิน 24 ชม. ให้แยกความปลอดภัย (LOTO, arc flash, PPE) กับพื้นฐานไฟฟ้า/การวัด ออกเป็นสองหน่วยและเพิ่มชั่วโมง
- **เพิ่มที่ไหน:** ทบทวน/ขยาย M00 (อาจแยกเป็น M00a ความปลอดภัย + M00b พื้นฐานไฟฟ้าและการวัด) เพิ่มชั่วโมงรวมเป็น ~32

### PID / Closed-loop Control และ Advanced Motion (electronic cam/gear, interpolation, synchronization)
- **ทำไมสำคัญ:** M08 ครอบคลุม servo/inverter แต่ทักษะควบคุมแกนหลักของ Mechatronics คือ closed-loop: การจูน PID (ตำแหน่ง/ความเร็ว/แรงบิด), การควบคุมกระบวนการ (อุณหภูมิ/แรงดัน/flow ด้วย analog I/O + PID instruction ของ FX5U), และ motion ขั้นสูง เช่น electronic cam/gear, multi-axis interpolation, synchronized motion ซึ่งจำเป็นกับ pick & place ที่ลื่นไหล หากขาดผู้เรียนจะปรับ servo ได้แค่ point-to-point หยาบๆ และจูนระบบ analog ไม่เป็น
- **ข้อเสนอแนะ:** เพิ่มหน่วยใน M08: ทฤษฎี closed-loop + PID tuning (ทั้ง motion และ process), analog I/O scaling, auto-tuning ของ MELSERVO; และเพิ่มหน่วย advanced motion ใน M11: positioning table, electronic cam/gear, multi-axis interpolation/sync ผ่าน Simple Motion / FX5U positioning
- **เพิ่มที่ไหน:** เพิ่มเนื้อหาใน M08 (PID/process control) และ M11 (advanced multi-axis motion)

### EMC, Grounding/Bonding, Shielding และ Power Quality/Harmonics ของระบบ servo/inverter/comm
- **ทำไมสำคัญ:** ระบบที่มี VFD, servo และ bus สื่อสาร (Modbus/Ethernet/CC-Link) ล้มเหลวบ่อยจากปัญหา noise/EMC และ grounding ที่ผิด ซึ่งเป็นความรู้ที่ช่างมือโปรต้องมี แต่หลักสูตรไม่ระบุชัด: single-point ground, shield termination, แยกสายกำลัง/สายสัญญาณ, ferrite/EMC filter, สาย shielded สำหรับ encoder/bus, และ harmonics/power factor ที่ VFD สร้าง (รวมถึง reactor/filter) ขาดส่วนนี้ระบบ Capstone จะมี intermittent fault ที่ไล่ไม่จบ
- **ข้อเสนอแนะ:** แทรกหน่วย 'EMC & Grounding best practice' ใน M08 (advanced wiring) ครอบคลุม shield termination, cable routing/segregation, EMC filter, equipotential bonding; และเพิ่มหัวข้อ power quality/harmonics/PF correction/line reactor ใน M05 (motor/VFD)
- **เพิ่มที่ไหน:** แทรกใน M08 (EMC/grounding/shielding) และ M05 (harmonics/power quality)

### OT/ICS Cybersecurity (ความมั่นคงปลอดภัยของระบบควบคุม)
- **ทำไมสำคัญ:** เมื่อหลักสูตรพา PLC/HMI ขึ้น Ethernet, SCADA และ IIoT (M10, M13) ความเสี่ยงด้านไซเบอร์กลายเป็นประเด็นมาตรฐานสมัยใหม่ (IEC 62443) แต่หลักสูตรไม่กล่าวถึงเลย ผู้เรียนควรรู้ network segmentation/zone-conduit, firewall/DMZ ระหว่าง OT-IT, การตั้งรหัส/ปิดพอร์ตที่ไม่ใช้ของ GOT/FX5U, secure remote access และ backup/recovery เพื่อไม่ให้สร้างระบบที่เปิดช่องโจมตี
- **ข้อเสนอแนะ:** เพิ่มหน่วย OT security พื้นฐานตาม IEC 62443 ใน M10 (network design: VLAN/segmentation, IT/OT boundary) และขยายใน M13 (secure SCADA/IIoT, cloud connectivity ที่ปลอดภัย, backup/DR)
- **เพิ่มที่ไหน:** แทรกใน M10 (network segmentation) และขยายใน M13 (secure IIoT/SCADA)

### เครื่องมือวัด/ทดสอบทางไฟฟ้าและการสอบเทียบ (Megger, oscilloscope, loop/analog calibration)
- **ทำไมสำคัญ:** ทักษะมือพื้นฐานของช่างคือการใช้เครื่องมือวัดอย่างถูกต้องและตีความผล แต่หลักสูตรระบุแค่การเดินสาย/อ่านแบบ ไม่ระบุการใช้ insulation tester (Megger), clamp meter, earth ground tester, oscilloscope (ดู PWM/encoder/communication signal) และการ calibrate/verify สัญญาณ 4-20mA / 0-10V กับ analog I/O ซึ่งจำเป็นต่อทั้งงานติดตั้งและ troubleshoot
- **ข้อเสนอแนะ:** เพิ่มหน่วย 'เครื่องมือวัดและการสอบเทียบ' ใน M01 (insulation/earth/clamp test, การอ่านค่า) และเพิ่มการใช้ oscilloscope + loop/analog calibration ใน M04 (sensors/analog) และ M08 (servo/encoder signal)
- **เพิ่มที่ไหน:** แทรกใน M01 (insulation/earth/clamp + safety measurement), M04 (analog 4-20mA/0-10V calibration), M08 (oscilloscope/encoder)

### Electrical CAD/Documentation มาตรฐาน (EPLAN/AutoCAD Electrical, IEC symbol, wire numbering, as-built)
- **ทำไมสำคัญ:** M01 สอน 'อ่านแบบ' แต่ผู้เชี่ยวชาญต้อง 'เขียน/แก้แบบ' ด้วยเครื่องมือ CAD ไฟฟ้ามาตรฐาน (EPLAN หรือ AutoCAD Electrical), ใช้สัญลักษณ์ IEC 60617, ระบบ wire/terminal numbering, cross-reference, BOM และจัดทำเอกสาร as-built/manual ส่งมอบ ซึ่งเป็น deliverable จริงของทุกโครงการ ขาดส่วนนี้ผู้เรียนสร้างเครื่องได้แต่ส่งมอบเอกสารวิชาชีพไม่ได้
- **ข้อเสนอแนะ:** ขยาย M01 ให้รวมการเขียนแบบด้วย electrical CAD (อย่างน้อย AutoCAD Electrical หรือ EPLAN Education), มาตรฐานสัญลักษณ์/numbering/cross-reference, การทำ BOM และกำหนดให้ Capstone (M12) ต้องส่ง as-built drawing + O&M manual เป็นเกณฑ์ประเมิน
- **เพิ่มที่ไหน:** ขยาย M01 (เพิ่มการเขียนแบบด้วย CAD) และเพิ่มเกณฑ์เอกสารส่งมอบใน M12


## ระดับความสำคัญ: ต่ำ

### ทักษะ operator interface & UX ของ HMI
- **ทำไมสำคัญ:** M07 สอนสร้าง HMI ได้ แต่การออกแบบหน้าจอให้ operator ใช้งานจริงปลอดภัยมักถูกมองข้าม. หน้างาน HMI ที่ออกแบบไม่ดี = operator กดผิด, ไม่เห็น alarm, ไม่รู้สถานะเครื่อง. screen navigation, สถานะชัดเจน, ป้องกันการกดผิด, user level/password ทำให้เครื่องใช้งานได้จริงในไลน์ผลิต
- **ข้อเสนอแนะ:** เสริม M07 ด้วยหลักการออกแบบ:
- screen hierarchy/navigation (overview/manual/alarm/setting)
- แสดงสถานะเครื่องและ feedback ชัดเจน (สี/สถานะมาตรฐาน)
- ป้องกัน mis-operation, confirmation, interlock บนหน้าจอ
- user level / password / security
- recipe / parameter setting หน้างาน, data logging, trend
- ออกแบบ HMI ของ Pick & Place ให้ operator จริงใช้ได้
- **เพิ่มที่ไหน:** ขยายเนื้อหา M07 และนำไปใช้จริงใน M12

### Mechanical & assembly literacy พื้นฐานสำหรับงานประกอบเครื่อง
- **ทำไมสำคัญ:** Capstone ให้ประกอบเครื่อง Pick & Place ที่มีส่วนกล แต่หลักสูตรเป็นสายไฟฟ้า/ควบคุมล้วน ไม่มีพื้นฐานกลเลย เช่น อ่าน mechanical drawing เบื้องต้น, coupling/alignment, ติดตั้ง linear guide/ball screw/belt, align มอเตอร์-โหลด, backlash. ปัญหา positioning หลายอย่างเป็นปัญหากล ถ้าแยก electrical/mechanical ไม่ออกจะ troubleshoot ผิดทาง
- **ข้อเสนอแนะ:** เพิ่มพื้นฐานกลเท่าที่จำเป็นต่องาน mechatronics:
- อ่าน mechanical/assembly drawing เบื้องต้น
- ติดตั้ง/align มอเตอร์-coupling-load, ball screw/belt, linear guide
- backlash, rigidity, ผลต่อ positioning และ servo tuning
- แยกแยะปัญหากล vs ปัญหาไฟฟ้า/โปรแกรม
- fastening/torque พื้นฐาน
- **เพิ่มที่ไหน:** mini-module ก่อน Capstone (ต้น M08 หรือ M11) หรือเพิ่มหัวข้อ mechanical literacy ~12-16 ชม.

### ขาด Drive Sizing / การเลือกและคำนวณอุปกรณ์ และมาตรฐานการออกแบบตู้
- **ทำไมสำคัญ:** ผู้เรียนสร้างเครื่องได้แต่ไม่รู้ 'ทำไมเลือกอุปกรณ์ตัวนี้' เช่น การเลือกขนาด breaker/contactor/overload, สายไฟตามกระแส, การเลือก servo/motor ตาม inertia & torque, การเลือก inverter ตามโหลด, มาตรฐานการจัดวางตู้ (IEC/มอก., wire color code, labeling, การแยก power/signal, grounding/shielding ป้องกัน noise ใน servo/encoder) ช่องว่างนี้ทำให้ได้ 'ช่างประกอบตามแบบ' มากกว่า 'ผู้เชี่ยวชาญที่ออกแบบได้' ตามเป้าหมายเทียบ Mechatronics
- **ข้อเสนอแนะ:** เพิ่มหน่วย 'Component Selection & Panel Design Standards' 8-12 ชม.: การคำนวณ/เลือก breaker-contactor-overload-สายไฟ, drive/motor sizing เชิงปฏิบัติ, มาตรฐานตู้และ labeling, grounding/shielding & noise mitigation (สำคัญมากกับ servo/encoder/comm) สอดแทรกหลัง M05/M08
- **เพิ่มที่ไหน:** เพิ่มหน่วยใน M08 (advanced wiring) หรือต่อยอดจาก M01/M05; เพิ่มหัวข้อ grounding/shielding ใน M08 และ M10

### Energy Management & Sustainability (พลังงาน, OEE, การวัดและลดการใช้พลังงาน)
- **ทำไมสำคัญ:** อุตสาหกรรมสมัยใหม่ (และ Industry 4.0) ให้ความสำคัญกับ energy monitoring, ESG และประสิทธิภาพการผลิต แต่หลักสูตรไม่มี energy management เลย ผู้เรียนควรรู้การวัดพลังงาน (power meter, CC-Link/Modbus energy data), การคำนวณ OEE/หน่วยพลังงานต่อชิ้น, และเทคนิคประหยัด เช่น VFD energy saving, sleep mode, regenerative braking ของ servo
- **ข้อเสนอแนะ:** เพิ่มหน่วย energy monitoring/OEE ใน M13 (Industry 4.0): การเก็บ energy data, dashboard OEE/พลังงาน, และแทรกแนวคิด energy-saving ของ VFD/servo (regen) ใน M05/M08
- **เพิ่มที่ไหน:** เพิ่มใน M13 (energy/OEE dashboard) และแทรกแนวคิด energy-saving ใน M05, M08

### End-of-Arm Tooling/Gripper, Robot-PLC Integration และ Offline Programming/Simulation
- **ทำไมสำคัญ:** M09 สอน KRL/smartPAD แต่งานหุ่นยนต์จริงต้องเข้าใจการเลือก/ออกแบบ gripper (vacuum/pneumatic/EOAT), payload/reach/cycle-time, การ handshake robot-PLC (program select, I/O interface), palletizing/pattern และ offline programming/simulation (เช่น KUKA.Sim) เพื่อลดเวลา downtime หน้าเครื่อง ส่วนเหล่านี้กระจายอยู่บางส่วนแต่ไม่ครบ
- **ข้อเสนอแนะ:** เพิ่มหน่วยใน M09/M11: การเลือกและต่อ EOAT/gripper, robot-PLC handshake & I/O mapping, base/tool calibration, palletizing pattern และแนะนำ offline simulation (KUKA.Sim/WorkVisual) เพื่อ verify path ก่อนรันจริง
- **เพิ่มที่ไหน:** เพิ่มเนื้อหาใน M09 (EOAT/calibration) และ M11 (robot-PLC integration, offline sim)

### การเชื่อมต่อข้อมูล MES/Database/Traceability (SQL, OPC UA, data logging)
- **ทำไมสำคัญ:** M13 ระบุ SCADA/IIoT แต่ไม่ชัดเรื่องการต่อระบบบนสุด: OPC UA (มาตรฐานกลางของ Industry 4.0), การ log ข้อมูลผลิตลง database (SQL), traceability ราย lot/serial และ MES integration ซึ่งเป็นสิ่งที่โรงงานสมัยใหม่ต้องการจากวิศวกรอัตโนมัติ
- **ข้อเสนอแนะ:** เพิ่มหน่วยใน M13: OPC UA server/client บน FX5U/GOT, data logging ลง SQL/CSV, traceability และแนวคิด MES/ERP integration พร้อม mini-lab ส่งข้อมูล Capstone ขึ้น dashboard/ฐานข้อมูล
- **เพิ่มที่ไหน:** ขยาย M13 (OPC UA, SQL logging, traceability/MES)

