# OWASP LLM Top 10 2025 — 3 ข้อที่สนใจ

## LLM01: Prompt Injection

การฝัง instruction ใน input เพื่อหลอกให้ LLM ทำตามคำสั่งที่ไม่ได้ตั้งใจ เช่น ให้เปิดเผยข้อมูลลับ หรือข้ามกฎที่ตั้งไว้

**เกี่ยวกับงานยังไง:** ถ้าองค์กรนำ LLM มาใช้เป็น chatbot ให้บริการลูกค้า ผู้โจมตีอาจใช้ prompt injection หลอกให้ bot เปิดเผยข้อมูลภายในระบบ หรือสั่งให้ทำ action ที่ไม่ควรทำได้ เช่น อนุมัติคำขอโดยไม่ผ่านการตรวจสอบ ซึ่งใน Workshop นี้จะได้ลองโจมตี LLMGoat ด้วย prompt injection จริงใน Day 2

## LLM02: Insecure Output Handling

output ที่ LLM สร้างขึ้นไม่ถูก sanitize หรือ validate ก่อนนำไปใช้งาน ทำให้อาจเกิด XSS, SQL injection, หรือ command injection ต่อระบบปลายทางได้

**เกี่ยวกับงานยังไง:** ในระบบที่ LLM สร้างโค้ดหรือ query แล้วรันอัตโนมัติ ถ้าไม่ตรวจสอบ output ก่อน ผู้โจมตีอาจใช้ prompt injection ให้ LLM สร้าง malicious code ที่ถูก execute บน server โดยตรง ดังนั้นทุกครั้งที่นำ output จาก LLM ไปใช้ต่อ ต้อง sanitize เสมอ

## LLM06: Sensitive Information Disclosure

LLM อาจเปิดเผยข้อมูลที่เป็นความลับ เช่น API key, ข้อมูลส่วนตัว, system prompt, หรือข้อมูลจาก training data โดยไม่ตั้งใจ

**เกี่ยวกับงานยังไง:** ถ้า LLM ถูก train หรือได้รับ context ที่มีข้อมูลลับขององค์กร (เช่น credentials, ข้อมูลลูกค้า, โครงสร้างระบบภายใน) ผู้โจมตีอาจใช้ prompt ที่ออกแบบมาเพื่อดึงข้อมูลเหล่านี้ออกมาได้ การป้องกันต้องทั้งจำกัด context ที่ให้ LLM และ filter output ก่อนส่งให้ผู้ใช้
