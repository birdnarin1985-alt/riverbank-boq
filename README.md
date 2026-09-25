# 📐 Riverbank Bio-Engineering Pro Suite — System Design & UI Rules
**Generated Date & Time:** 25 กันยายน 2026 เวลา 22:18 น. (GMT+7)
**Version:** 1.0 (Production Stable)

---

## 1. กฎสัดส่วนทองคำและการจัดลำดับสายตา (Golden Ratio & Fibonacci Scale)
* **สัดส่วนแบ่งคอลัมน์ ($61.8\% : 38.2\%$):**
  * หน้าจอแนวนอน: แบ่งพื้นที่แผงเอกสาร/แบบจำลองวิศวกรรมหลัก ($61.8\%$) และแผงควบคุม HUD ($38.2\%$)
  * ส่วนหัวเอกสาร: ข้อมูลผู้รับเหมา $61.8\%$ และข้อมูลโครงการ $38.2\%$
  * สรุปท้ายสัญญา: รายละเอียดเงื่อนไขชำระเงิน $61.8\%$ และยอดสุทธิ/ตัวหนังสือระบุยอดเงิน $38.2\%$
* **ระยะห่างฟีโบนัชชี (Fibonacci Spacing Hierarchy):**
  * คุม Padding และ Margin ด้วยค่าฟีโบนัชชี: `3px`, `5px`, `8px`, `13px`, `21px`, `34px`, `55px`
* **ลำดับขั้นตัวอักษร (Typography Hierarchy Scale):**
  * Micro / Sub-labels: 9px
  * Captions / Meta: 11px
  * Body Text: 13px
  * Sub-headings: 16px
  * Section Titles: 21px
  * Grand Total & Hero: 26px

---

## 2. กฎการพิมพ์มาตรฐาน 4 หน้ากระดาษ A4 (Strict 4-Page Print Rules)
ป้องกันปัญหาหน้าเกิน (หน้าที่ 5 และ 8) และหน้าว่างสีขาวบน WebKit / iOS Safari:
* **เลิกใช้ Flexbox ในโหมดพิมพ์:** ใน `@media print` กำหนดโครงสร้างหลัก `.page-sheet` เป็น `display: block !important;` เท่านั้น เพื่อป้องกันการคำนวณความสูงคลาดเคลื่อน
* **ห้ามฟิกซ์ความสูงคงที่ (No Fixed Height):** ห้ามใช้ `min-height: 270mm` หรือ `height: 275mm` เพราะเมื่อรวมกับระยะขอบ `@page { margin: 8mm 10mm; }` จะเกินพื้นที่พิมพ์จริง ($281\text{mm}$) ส่งผลให้เกิดการปัดเศษสร้างหน้าว่างต่อท้าย
* **การคุมการแบ่งหน้า (Page Breaks):**
  * `.page-sheet` ทุกหน้าต้องมี:
    * `page-break-inside: avoid !important; break-inside: avoid !important;`
    * `page-break-after: always !important; break-after: page !important;`
  * **ล็อกหน้าสุดท้าย:** หน้าที่ 4 (`#page-4` หรือ `.page-sheet:last-of-type`) ต้องระบุ:
    * `page-break-after: auto !important; break-after: auto !important;` เพื่อตัดหน้าว่างต่อท้ายอย่างเด็ดขาด
* **การจัดกึ่งกลางสายตาหน้า 4:** ใช้ระบบ `margin: 32mm auto 0 auto !important;` เพื่อให้กล่องยันต์ลอยอยู่กึ่งกลางแผ่น A4 อย่างสมดุลโดยไม่กระทบต่อความสูงรวมของหน้ากระดาษ

---

## 3. สถาปัตยกรรมแบบจำลองเวกเตอร์ (Parametric Vector Drawings)
* **หน้า 2 (แบบจำลองสถาปัตย์และรูปตัด):**
  * **ภาพทัศนียภาพ 3D ด้านบน:** ใช้มุมมองแบบกว้างด้านข้าง (Elevated Oblique Side Perspective) กางความยาวตลิ่งเกือบเต็มความกว้างเฟรม SVG ($430\text{px}$) พร้อมเส้นนำสายตาสู่จุด Horizon เพื่อเปิดมุมมองด้านข้างให้เห็นความยาวและชั้นระเบียงชัดเจน
  * **รูปตัดขวาง 2D ด้านล่าง:** แสดงความลาดชันและชั้นดินด้วยเส้น Hatching เวกเตอร์ และแสดงแนวเสาเข็มที่ตอกลึกใต้ฐานราก
  * **ประสิทธิภาพ:** เรนเดอร์ด้วย Pure SVG พาราเมตริกที่คำนวณผ่านพิกัดทางคณิตศาสตร์แบบเรียลไทม์ ปราศจากไลบรารี 3D ภายนอก

---

## 4. โครงสร้างระบบแบบไฟล์เดียว (Zero-Dependency Single File)
* **Single Source of Truth:** รวมโค้ด HTML, CSS และ JavaScript ไว้ในไฟล์ `index.html` เพียงไฟล์เดียว ใช้งานแบบ Offline-Ready ได้ทันที
* **ระบบภาษา (Bilingual Support):** รองรับการสลับภาษา TH / EN แบบไดนามิกผ่านฟังก์ชัน `toggleLanguage()` ซึ่งแปลงทั้งข้อความในตาราง BOQ, เอกสารสัญญา, สเปก CAD, สาส์นธรรมทาน และแปลงตัวเลขเงินเป็นตัวหนังสือทั้งสองภาษา
* **ระบบ Telemetry ภายในเครื่อง:** ดักจับข้อผิดพลาดผ่าน `window.onerror` และ `unhandledrejection` บันทึกลง `localStorage` สูงสุด 10 รายการ พร้อมปุ่มคัดลอกรายงานข้อผิดพลาดสำหรับส่งตรวจเช็ก


