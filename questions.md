# คำถามค้างใจสำหรับ Day 2

## คำถามที่ 1: Prompt Injection ป้องกันได้ 100% ไหม?
จาก Day 1 เราเรียนรู้ว่า Prompt Injection คือการฝัง instruction หลอก LLM — ในทางปฏิบัติจริง มีวิธีป้องกันที่สมบูรณ์แบบไหม? หรือเป็นแค่การลดความเสี่ยง? แล้ว LLMGoat ที่เราจะโจมตีใน Day 2 มี defense อะไรบ้างที่ต้องเจาะผ่าน?

## คำถามที่ 2: Aider ใช้โจมตี LLM ยังไงใน Red Team scenario?
ใน Day 1 เราใช้ Aider แค่สร้างไฟล์ hello world — ใน Day 2 จะใช้ Aider สร้าง attack payload อัตโนมัติยังไง? สั่งให้ Aider เขียน script ที่ทำ prompt injection หลายๆ แบบแล้วทดสอบกับ LLMGoat ได้เลยไหม?

## คำถามที่ 3: ถ้า model เล็ก (3B) โจมตียากกว่า model ใหญ่ไหม?
เราใช้ qwen2.5-coder:3b ซึ่งเป็น model ขนาดเล็ก — ในการโจมตี LLM จริง model ขนาดเล็กจะมีพฤติกรรมต่างจาก model ใหญ่ (เช่น GPT-4, Claude) ไหม? model เล็กจะถูกหลอกง่ายกว่าหรือยากกว่า?
