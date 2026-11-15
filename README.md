# 🛡️ GoHard — Threat Detection & Incident Decision Support

> **ระบบตรวจจับภัยคุกคามทางไซเบอร์และสนับสนุนการตัดสินใจสำหรับทีม Security Operations (SOC)**  
> ผสานการวิเคราะห์พฤติกรรมผิดปกติ (Behavioral Anomaly Detection) เข้ากับการจำลองเส้นทางการโจมตีตามกรอบ MITRE ATT&CK พร้อมโหมดทดสอบ **Passive Mode** และการอนุมัติระงับเหตุแบบ **Human-in-the-Loop (HIL)**

[![React](https://img.shields.io/badge/Frontend-React%2018-61DAFB?style=flat-square&logo=react)](https://react.dev/)
[![Vite](https://img.shields.io/badge/Bundler-Vite-646CFF?style=flat-square&logo=vite)](https://vitejs.dev/)
[![Tailwind CSS](https://img.shields.io/badge/Styles-Tailwind%20CSS-38B2AC?style=flat-square&logo=tailwind-css)](https://tailwindcss.com/)
[![Framework](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-E02424?style=flat-square)](https://attack.mitre.org/)
[![Compliance](https://img.shields.io/badge/Compliance-PDPA%20Ready-10B981?style=flat-square)](#audit--governance)
[![GitHub Repo](https://img.shields.io/badge/Repository-GitHub-181717?style=flat-square&logo=github)](https://github.com/Name-Thinking/GoHard)

---

## 📌 สารบัญ (Table of Contents)
1. [ภาพรวมโครงการ (Project Overview)](#-ภาพรวมโครงการ-project-overview)
2. [ปัญหาและความท้าทายของทีม SOC (Problem Statement)](#-ปัญหาและความท้าทายของทีม-soc-problem-statement)
3. [ตารางเปรียบเทียบแนวทางดั้งเดิม vs GoHard](#-ตารางเปรียบเทียบแนวทางดั้งเดิม-vs-gohard)
4. [สถาปัตยกรรมระบบ 5 ขั้นตอน (System Architecture & Pipeline)](#-สถาปัตยกรรมระบบ-5-ขั้นตอน-system-architecture--pipeline)
5. [ฟีเจอร์เด่นบน Web Application (Key Features)](#-ฟีเจอร์เด่นบน-web-application-key-features)
6. [โครงสร้างทีมพัฒนา (Team & Organization)](#-โครงสร้างทีมพัฒนา-team--organization)
7. [โครงสร้างไฟล์โปรเจกต์ (Project Structure)](#-โครงสร้างไฟล์โปรเจกต์-project-structure)
8. [วิธีติดตั้งและรันระบบ (Getting Started)](#-วิธีติดตั้งและรันระบบ-getting-started)
9. [การปฏิบัติตามมาตรฐาน (Audit & Governance)](#-การปฏิบัติตามมาตรฐาน-audit--governance)

---

## 🎯 ภาพรวมโครงการ (Project Overview)

ในสภาพแวดล้อมระบบไอทียุคปัจจุบัน ปริมาณ Traffic และจำนวน Log ที่เกิดขึ้นในแต่ละวันมีมหาศาล ระบบตรวจจับภัยคุกคามแบบเดิมมักส่งผลให้เกิด **Alert Fatigue (ความเหนื่อยล้าจากการแจ้งเตือน)** หรือการแจ้งเตือนที่ผิดพลาด (False Positive) จำนวนมาก ในขณะที่การโจมตีสมัยใหม่เกิดขึ้นรวดเร็วในระดับวินาที

**GoHard** ถูกออกแบบมาเพื่อทำหน้าที่เป็น **ผู้ช่วยวิเคราะห์และสนับสนุนการตัดสินใจ (Decision Support Copilot)** ของทีม Security Operations:
- คัดกรองและจัดกลุ่ม Alert ที่กระจัดกระจายให้กลายเป็นภาพรวมเหตุการณ์เดียว
- เชื่อมโยงร่องรอยการโจมตีข้ามระบบ (Cross-System) ตามกรอบ **MITRE ATT&CK**
- เสนอแนวทางระงับเหตุ (Playbook Action) พร้อมประเมินผลกระทบทางธุรกิจ (Blast Radius)
- รองรับโหมดสังเกตการณ์ **Passive Mode (14 วัน)** โดยไม่ส่งผลกระทบต่อ Production

---

## 🚨 ปัญหาและความท้าทายของทีม SOC (Problem Statement)

| รหัส | ปัญหาหลัก | ผลกระทบต่อองค์กร |
| :---: | :--- | :--- |
| **01** | **Alert Fatigue & Noise** | การแจ้งเตือน False Positive หลักพันรายการต่อวัน ทำให้ทีม SOC ใช้เวลาหมดไปกับการคัดกรอง และอาจมองข้ามภัยคุกคามจริง |
| **02** | **Automated High-Speed Attacks** | ผู้โจมตีใช้สคริปต์อัตโนมัติขยายผลในระดับวินาที ซึ่งเร็วเกินกว่าการเปิดดู Log ด้วยมือแบบเดิม |
| **03** | **Cross-System Attack Chains** | ร่องรอยการบุกรุกกระจายตัวระหว่าง Cloud, On-Premise, Network และ Endpoint ทำให้ปะติดปะต่อเส้นทางการโจมตีได้ยาก |
| **04** | **Lack of Actionable Context** | ขาดข้อมูลสรุปความเสี่ยงที่เข้าใจง่าย ผู้บริหารหรือ CISO เกิดความลังเลในการกดบล็อกเพราะกังวลว่าระบบธุรกิจจะหยุดชะงัก |

---

## ⚖️ ตารางเปรียบเทียบแนวทางดั้งเดิม vs GoHard

| มิติการทำงาน | ระบบตั้งรับแบบดั้งเดิม (Traditional SIEM / Rule-based) | แนวทางของ GoHard (AI-Driven Decision Support) |
| :--- | :--- | :--- |
| **รูปแบบการตรวจจับ** | พึ่งพากฎตายตัว (Static Rules) หรือ Signature ตรวจจับได้เฉพาะภัยที่เคยรู้จักมาก่อน | วิเคราะห์ความผิดปกติของพฤติกรรม (Behavioral Anomaly) ตรวจจับเทคนิคใหม่ได้ |
| **การวิเคราะห์และคัดกรอง** | วิศวกรความปลอดภัยต้องเปิดอ่าน Log แยกรายเครื่องเพื่อหาจุดเชื่อมโยง | เชื่อมโยง Event เข้าด้วยกันเป็น Attack Chain พร้อมคะแนนความเสี่ยงอัตโนมัติ |
| **การตัดสินใจยับยั้งเหตุ** | เกิดความลังเล กลัวระบบงานล่ม หรือเปิด Auto-Block จนกระทบผู้ใช้งานปกติ | มีโหมด Passive ทดสอบความแม่นยำ และโหมด Human-in-the-Loop ให้ทีมอนุมัติอย่างมั่นใจ |

---

## ⚙️ สถาปัตยกรรมระบบ 5 ขั้นตอน (System Architecture & Pipeline)

```
[ STEP 01 ] ──▶ [ STEP 02 ] ──▶ [ STEP 03 ] ──▶ [ STEP 04 ] ──▶ [ STEP 05 ]
Log & Traffic    Behavioral       Attack Chain     Risk Scoring     Human-in-the-Loop
  Ingestion      Anomaly Det.    Reconstruction     & Impact        Approval & Action
```

1. **STEP 01: Log & Traffic Ingestion**
   - รวบรวมข้อมูล Syslog, Windows Event Logs, API Gateway Access Logs และ Network Flow จากอุปกรณ์เครือข่ายและเครื่องลูกข่ายแบบไม่กระทบประสิทธิภาพ
   - *ตัวอย่างข้อมูล:* `syslog://fw-core-01: DENY TCP 185.220.101.42:443 -> 10.24.8.112:445 (bytes=1420)`
2. **STEP 02: Behavioral Anomaly Detection**
   - ประมวลผลด้วยโมเดล Machine Learning เพื่อตรวจจับพฤติกรรมที่เบี่ยงเบนไปจาก Baseline การทำงานปกติขององค์กร
   - *ตัวอย่างข้อมูล:* `anomaly_score=0.94 | baseline_dev=+4.2σ | feature=unusual_child_process(powershell.exe)`
3. **STEP 03: Attack Chain Reconstruction**
   - เชื่อมโยงร่องรอยการโจมตีที่เกิดขึ้นในแต่ละเครื่องเข้าเป็นลำดับขั้นตอนตามกรอบ MITRE ATT&CK
   - *ตัวอย่างข้อมูล:* `Chain #402: Initial Access (T1190) -> Execution (T1059) -> Lateral Movement (T1021)`
4. **STEP 04: Risk Scoring & Impact Analysis**
   - คำนวณคะแนนความรุนแรงและประเมินผลกระทบ (Blast Radius) ต่อระบบงานหากสั่งแยกเครื่องหรือบล็อกพอร์ต
   - *ตัวอย่างข้อมูล:* `asset_criticality=HIGH (Finance DB) | blast_radius=1 host | recommended_playbook=PB-ISOLATE-02`
5. **STEP 05: Human-in-the-Loop Approval**
   - นำเสนอสรุปแนวทางยับยั้งบนแดชบอร์ดเพื่อให้ CISO หรือทีม SOC กดอนุมัติสั่งการได้ทันทีในคลิกเดียว
   - *ตัวอย่างข้อมูล:* `status=APPROVED_BY_SOC | action=isolate_host(10.24.8.112) | execution_time=180ms`

---

## 🚀 ฟีเจอร์เด่นบน Web Application (Key Features)

- **Interactive SOC Incident Console:**
  - เรดาร์จำลองการสแกนตรวจจับภัยคุกคามแบบ Real-time (Radar Sweep)
  - แสดงตำแหน่งและระดับความรุนแรงของภัยคุกคาม (`MITRE T1059`, `T1021`, `T1190`, `T1110`)
  - รองรับการสลับโหมดการทำงานระหว่าง **Passive Mode** และ **Human-in-the-Loop**
  - คลิกเลือกแต่ละเหตุการณ์เพื่อดูรายละเอียดเป้าหมาย คำแนะนำจาก AI และกดปุ่มจำลองการอนุมัติระงับเหตุได้จริง
- **Interactive Pipeline Inspector:**
  - คลิกเลือกดูผลลัพธ์จำลองของแต่ละขั้นตอนประมวลผล (Step 01 - 05)
- **Request Demo & 14-Day Evaluation Modal:**
  - ฟอร์มขอทดสอบระบบ Passive Mode ฟรี 14 วัน พร้อมระบบบันทึกข้อมูลและตรวจสอบความยินยอมตามมาตรฐาน PDPA
- **Fluid & Accessible Typography:**
  - ออกแบบฟอนต์ภาษาไทย (`Anuphan` และ `Prompt`) ป้องกันปัญหาคำตกหล่น (No word breaking/dropping)
  - รองรับสเกลหน้าจอ 125% Visual Scaling อย่างคมชัด
  - แอนิเมชันเปิดตัวเนื้อหาแบบ **Scroll Reveal Animation** ที่ลื่นไหลและประหยัดทรัพยากร GPU

---

## 👥 โครงสร้างทีมพัฒนา (Team & Organization)

| รูปถ่าย | รายชื่อ - นามสกุล | รหัสนักศึกษา | ตำแหน่ง (Role) | ฝ่ายงาน & ขอบเขตความรับผิดชอบ |
| :---: | :--- | :---: | :--- | :--- |
| <img src="./assets/frank.jpg" width="80" height="80" style="border-radius: 8px; object-fit: cover;" alt="CEO"> | **แฟร้งค์ ลอเรนซ์ เมอร์คาโด** | `6752300771` | **CEO (Chief Executive Officer)** | **Executive & Leadership**<br>กำหนดวิสัยทัศน์ ทิศทางกลยุทธ์ของบริษัท บริหารภาพรวมโปรเจกต์ และดูแลการเติบโตทางธุรกิจ |
| <img src="./assets/supachok.jpg" width="80" height="80" style="border-radius: 8px; object-fit: cover;" alt="AI Engineer"> | **ศุภโชค ชมภูพื้น** | `6752300852` | **AI Engineer** | **AI & Data Engineering**<br>ออกแบบ พัฒนา และปรับแต่งโมเดล AI/ML (Behavioral Anomaly Detection & Context Engine) เพื่อตรวจจับภัยคุกคาม |
| <img src="./assets/sila_avatar.jpg" width="80" height="80" style="border-radius: 8px; object-fit: cover;" alt="MLOps Engineer"> | **ศิลา บำรุงพันธ์** | `6752300372` | **MLOps Engineer** | **AI & Data Engineering**<br>ดูแลระบบ Data Pipeline จัดการการนำเข้า Log ประมวลผลข้อมูล Real-time และ Deploy โมเดล AI ลง Production |

---

## 📁 โครงสร้างไฟล์โปรเจกต์ (Project Structure)

```text
e:\Gohart\
├── assets\
│   ├── frank.jpg                 # รูปโปรไฟล์ CEO (แฟร้งค์ ลอเรนซ์ เมอร์คาโด)
│   ├── supachok.jpg              # รูปโปรไฟล์ AI Engineer (ศุภโชค ชมภูพื้น)
│   ├── sila_avatar.jpg           # รูปโปรไฟล์ MLOps Engineer (ศิลา บำรุงพันธ์)
│   ├── index-B48hGD3H.js         # แอปพลิเคชัน React + Lucide Icons (Production Bundle)
│   └── index-Bu8emfw2.css        # สไตล์ชีต Cyber Theme (288+ Utility Classes, 0 Syntax Errors)
├── index.html                    # หน้าหลัก HTML พร้อม Favicon SVG และฟอนต์ภาษาไทย
└── README.md                     # เอกสารแนะนำโครงการ GoHard
```

---

## 💻 วิธีติดตั้งและรันระบบ (Getting Started)

### ความต้องการของระบบ (Prerequisites)
- **Python 3.9+** (สำหรับ Static HTTP Server) หรือ
- **Node.js 18+** (หากต้องการทดสอบด้วย Vite หรือ Node tooling)

### ขั้นตอนการรันเว็บแอปพลิเคชัน

1. **โคลนคลังโค้ด (Clone Repository):**
   ```bash
   git clone https://github.com/Name-Thinking/GoHard.git
   cd GoHard
   ```

2. **เปิดให้บริการเซิร์ฟเวอร์ Static Server:**
   - *วิธีที่ 1 (ผ่าน Python built-in server):*
     ```bash
     python -m http.server 5173
     ```
   - *วิธีที่ 2 (ผ่าน Node npx serve):*
     ```bash
     npx serve -l 5173 .
     ```

3. **เข้าชมผ่านเว็บเบราว์เซอร์:**
   - เปิดเบราว์เซอร์แล้วไปที่: **`http://localhost:5173/`**

---

## 🔒 การปฏิบัติตามมาตรฐาน (Audit & Governance)

- **PDPA Compliant:** แบบฟอร์มขอทดสอบระบบระบุวัตถุประสงค์การจัดเก็บข้อมูลการติดต่ออย่างชัดเจน และมี Checkbox ให้ผู้ใช้กดยืนยันความยินยอมก่อนส่งข้อมูล
- **MITRE ATT&CK Framework:** อ้างอิงรหัสเทคนิคการโจมตีตามมาตรฐานสากล (T1059: Command and Scripting Interpreter, T1021: Remote Services, T1190: Exploit Public-Facing Application, T1110: Brute Force)
- **Zero Inconsequential Slop:** ปราศจากเอฟเฟกต์แสงฟุ้งที่รบกวนสายตา โดยเน้นการจัดวางเลย์เอาต์ที่ชัดเจนและสนับสนุนการตัดสินใจของทีมความปลอดภัยเป็นสำคัญ

---

## 📄 ลิขสิทธิ์และการติดต่อ (License & Contact)

- **โครงการ:** GoHard Security Project © 2026
- **GitHub Repository:** [https://github.com/Name-Thinking/GoHard](https://github.com/Name-Thinking/GoHard)
