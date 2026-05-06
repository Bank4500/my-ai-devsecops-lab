# Day 1 Troubleshooting — ปัญหาที่เจอ + วิธีแก้

## ปัญหาที่ 1: LLMGoat เชื่อมต่อ Ollama ไม่ได้ (Connection refused)

**อาการ:**
```
[ERROR] Failed to list Ollama models at http://host.docker.internal:11434:
  Max retries exceeded ... [Errno 111] Connection refused
```

Docker container (LLMGoat) พยายามเชื่อมต่อ Ollama ผ่าน `host.docker.internal:11434` แต่ถูกปฏิเสธการเชื่อมต่อ

**สาเหตุ:**
Ollama ติดตั้งไว้บน Windows แต่ Docker รันผ่าน WSL2 ทำให้ network ซ้อนกันหลายชั้น — Docker container เข้าถึง Ollama บน Windows ไม่ได้โดยตรง

**วิธีแก้:**
ติดตั้ง Ollama ใน WSL Ubuntu แทน แล้วตั้งค่าให้ listen ทุก interface:
```bash
# ติดตั้ง Ollama ใน WSL
sudo apt-get install -y zstd
curl -fsSL https://ollama.com/install.sh | sh

# ตั้งค่าให้ listen 0.0.0.0 แทน 127.0.0.1
sudo systemctl stop ollama
OLLAMA_HOST=0.0.0.0 ollama serve &

# Pull model ใหม่
OLLAMA_HOST=0.0.0.0 ollama pull qwen2.5-coder:3b

# Restart LLMGoat
docker compose -f compose.local.yaml down --remove-orphans
docker compose -f compose.local.yaml up -d
```

## ปัญหาที่ 2: Ollama ใน WSL ฟังแค่ localhost — Docker container ยังเข้าไม่ได้

**อาการ:**
```
[ERROR] No models available on Ollama at http://host.docker.internal:11434
```

ติดตั้ง Ollama ใน WSL แล้ว แต่ Docker container ยังเชื่อมไม่ได้ หรือเชื่อมได้แต่ไม่เห็น model

**สาเหตุ:**
Ollama default listen เฉพาะ `127.0.0.1` แต่ Docker container เข้าผ่าน IP อื่น (`host.docker.internal` ไม่ใช่ 127.0.0.1) นอกจากนี้ model ที่ pull ไว้ตอน Ollama รันเป็น systemd service (user: ollama) จะอยู่คนละ directory กับตอนรัน `OLLAMA_HOST=0.0.0.0 ollama serve` (user: root)

**วิธีแก้:**
ต้องทำ 2 อย่าง:
1. ตั้ง `OLLAMA_HOST=0.0.0.0` ก่อนรัน `ollama serve`
2. Pull model ใหม่ภายใต้ environment เดียวกัน: `OLLAMA_HOST=0.0.0.0 ollama pull qwen2.5-coder:3b`

วิธีตั้งถาวรผ่าน systemd:
```bash
sudo systemctl edit ollama
# เพิ่ม:
# [Service]
# Environment="OLLAMA_HOST=0.0.0.0"
sudo systemctl restart ollama
```

## ปัญหาที่ 3: ติดตั้ง Ollama ใน WSL ขึ้น error "requires zstd"

**อาการ:**
```
ERROR: This version requires zstd for extraction. Please install zstd and try again
```

**สาเหตุ:**
Ubuntu ใน WSL ไม่ได้ติดตั้ง package `zstd` มาให้ ซึ่ง Ollama installer ต้องใช้สำหรับแตกไฟล์

**วิธีแก้:**
```bash
sudo apt-get install -y zstd
curl -fsSL https://ollama.com/install.sh | sh
```
