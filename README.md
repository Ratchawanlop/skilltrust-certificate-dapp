# 🎓 SkillTrust Academy - Digital Certificate Verification System

ระบบออกและตรวจสอบใบรับรองดิจิทัลบนเครือข่าย Ethereum Blockchain เพื่อป้องกันการปลอมแปลงเอกสาร ลดขั้นตอนการตรวจสอบคุณวุฒิของผู้สมัครงาน และคุ้มครองข้อมูลส่วนบุคคล (PDPA) ด้วยการเก็บเฉพาะค่า Hash ของใบรับรองบนบล็อกเชน

🌐 **Live Demo (GitHub Pages):**  
👉 [https://ratchawanlop.github.io/skilltrust-certificate-dapp/](https://ratchawanlop.github.io/skilltrust-certificate-dapp/) *(หรือเข้าผ่าน `/SkillTrustCer.html`)*

---

## 🛠️ Tech Stack (เทคโนโลยีที่ใช้)

- **Frontend:** HTML5, CSS3 (Modern Responsive Grid & Custom Design System), Vanilla JavaScript (ES6+)
- **Blockchain Interaction:** [Ethers.js](https://docs.ethers.org/v5/) (`v5.7.2`) สำหรับเชื่อมต่อกับ Web3 Provider และเรียกใช้งาน Smart Contract
- **Smart Contract & Network:** Solidity บนเครือข่าย Ethereum / EVM-Compatible Testnet (เช่น Sepolia, Holesky, Ganache หรือ Remix VM/Injected Provider)
- **Wallet Provider:** MetaMask Browser Extension
- **Cryptography & Privacy:** `Keccak-256` (`ethers.utils.id`) สำหรับแปลงข้อมูลส่วนบุคคลและผลคะแนนให้เป็นรหัส Hash ก่อนบันทึกขึ้น Public Blockchain
- **Verification QR Code:** QR Server API & URL Query Parameters (`?certId=...`) สำหรับสแกนตรวจสอบใบรับรองได้ทันที

---

## 📋 สิ่งที่ต้องเตรียมก่อนใช้งาน (Prerequisites)

1. **เว็บเบราว์เซอร์** เช่น Google Chrome, Microsoft Edge หรือ Brave
2. **ส่วนขยายกระเป๋าเงินดิจิทัล [MetaMask](https://metamask.io/)** ติดตั้งลงในเบราว์เซอร์
3. **เหรียญสำหรับค่าธรรมเนียม (Testnet ETH)** ในกระเป๋า MetaMask บนเครือข่ายที่ Smart Contract ถูก Deploy ไว้ (เช่น Sepolia Testnet)
4. **Smart Contract Address**
   - ค่าเริ่มต้นในระบบถูกตั้งไว้ที่: `0x557ac564eB184482e08AfA6f3Df3c491D709C18D`
   - ผู้ใช้สามารถเปลี่ยนหรือกรอก **Contract Address** ของตนเองได้ที่ช่องตั้งค่าด้านบนสุดของหน้าเว็บได้ทันที

### โครงสร้าง Smart Contract ABI ที่รองรับ
```solidity
function createCourse(string memory _courseId, string memory _details) public
function issueCertificate(string memory _certId, string memory _courseId, address _recipient, string memory _certHash) public
function verifyCertificate(string memory _certId) public view returns (string memory courseId, address recipient, string memory certHash, uint256 issueDate, bool isValid)
function revokeCertificate(string memory _certId) public
function getMyCertificates() public view returns (string[] memory)
```

---

## 🧪 วิธีการทดสอบระบบ (How to Test)

### ขั้นตอนที่ 0: เชื่อมต่อกระเป๋าเงินและตั้งค่า Contract
1. เปิดหน้าเว็บไซต์ หรือเปิดไฟล์ `SkillTrustCer.html` บนเบราว์เซอร์
2. กดปุ่ม **"🔗 Connect MetaMask"** มุมขวาบน และกดยืนยันการเชื่อมต่อในหน้าต่าง MetaMask
3. *(ทางเลือก)* หากต้องการใช้ Smart Contract ที่คุณ Deploy เอง ให้วาง Address ลงในช่อง **"ตั้งค่า Smart Contract Address"** แล้วกดปุ่ม **"อัปเดต Contract"**

---

### ขั้นตอนที่ 1: ลงทะเบียนหลักสูตร (Create Training Course - สำหรับ Admin)
1. ไปที่กล่อง **"1. ลงทะเบียนหลักสูตร (Create Training Course)"**
2. กรอกข้อมูลทดสอบ:
   - **รหัสหลักสูตร (Course ID):** เช่น `AI-101`
   - **รายละเอียดการอบรม (Course Details):** เช่น `AI & Data Analytics Bootcamp`
3. กดปุ่ม **"➕ ลงทะเบียนหลักสูตร (Create Course)"** และกด Confirm Transaction บน MetaMask รอจนระบบแจ้งเตือนว่าสร้างสำเร็จ

---

### ขั้นตอนที่ 2 & 3: เตรียมรหัส Hash และออกใบรับรอง (Store Hash & Issue Certificate - สำหรับ Issuer)
1. ไปที่กล่อง **"2. ออกใบรับรอง (Issue Certificate)"** แล้วกรอกข้อมูลเบื้องต้น:
   - **รหัสอ้างอิงใบรับรอง (Cert ID):** เช่น `CERT-2026-001` *(ต้องไม่ซ้ำกับใบเดิม)*
   - **รหัสหลักสูตร (Course ID):** เช่น `AI-101` *(ต้องตรงกับหลักสูตรที่สร้างไว้)*
   - **Address ผู้รับใบรับรอง:** กรอก Wallet Address ของผู้เรียน (เช่น `0x...` สามารถใช้ Address ของตัวเองเพื่อทดสอบหน้า Dashboard ได้)
2. ไปที่กล่อง **"3. เตรียมรหัสหลักฐาน (Store Certificate Hash)"** เพื่อแปลงข้อมูลส่วนบุคคลเป็น Hash:
   - **ชื่อ-นามสกุลผู้รับ (Off-chain):** เช่น `สมชาย ใจดี`
   - **ผลคะแนน / ข้อมูลเอกสาร PDF:** เช่น `Pass (95%)`
   - กดปุ่ม **"🛡️ สร้างค่า Hash และส่งไปยังฟอร์มออกใบรับรอง"** ระบบจะคำนวณค่า `Keccak-256 Hash` และนำไปเติมในช่อง `Certificate Hash` ของกล่องที่ 2 ให้อัตโนมัติ
3. กลับมาที่กล่องที่ 2 แล้วกดปุ่ม **"✨ ออกใบรับรองและบันทึกบน Blockchain"** จากนั้นยืนยันธุรกรรมบน MetaMask

---

### ขั้นตอนที่ 4: ตรวจสอบความถูกต้องของใบรับรอง (Verify Certificate - สำหรับทั่วไป/ผู้ว่าจ้าง)
1. ไปที่กล่อง **"4. ตรวจสอบความถูกต้อง (Verify Certificate)"**
2. กรอกรหัสใบรับรอง เช่น `CERT-2026-001`
3. กดปุ่ม **"🔎 ตรวจสอบสถานะใบรับรอง & สร้าง QR Code"**
4. ระบบจะดึงข้อมูลจาก Blockchain มาแสดงผล:
   - สถานะ: `✅ VALID (มีผลใช้งาน)`
   - รหัสหลักสูตร, Address ผู้รับ, Certificate Hash, วันที่ออกใบรับรอง และ **QR Code** สำหรับสแกนตรวจสอบ

---

### ขั้นตอนที่ 5: แสดงประวัติใบรับรองของฉัน (Certificate Dashboard - สำหรับผู้เข้าอบรม)
1. เชื่อมต่อ MetaMask ด้วยบัญชีที่เป็นผู้รับใบรับรอง (`Recipient Wallet`)
2. ไปที่กล่อง **"6. แสดงประวัติใบรับรอง (Certificate Dashboard)"**
3. กดปุ่ม **"📂 โหลดใบรับรองของฉัน (My Certificates)"**
4. ระบบจะแสดงป้ายรายการรหัสใบรับรองทั้งหมดที่ผูกกับกระเป๋าเงินนี้ (สามารถคลิกที่ป้ายรหัสใบรับรองเพื่อส่งค่าไปตรวจสอบในกล่องที่ 4 ได้ทันที)

---

### ขั้นตอนที่ 6: เพิกถอนใบรับรอง (Revoke Certificate - สำหรับ Admin/Issuer)
1. ไปที่กล่อง **"5. เพิกถอนใบรับรอง (Revoke Certificate)"**
2. กรอกรหัสใบรับรองที่ต้องการยกเลิก เช่น `CERT-2026-001`
3. กดปุ่ม **"⚠️ เพิกถอนใบรับรอง (Revoke Certificate)"** และยืนยันธุรกรรมบน MetaMask
4. ทดลองกลับไปกดตรวจสอบรหัส `CERT-2026-001` ในกล่องที่ 4 อีกครั้ง สถานะจะเปลี่ยนเป็น `❌ REVOKED (ถูกเพิกถอน)` โดยที่ประวัติการออกใบรับรองยังคงอยู่ครบถ้วน
