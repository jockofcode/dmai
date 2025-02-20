# Setting Up Your Local LLM

---

## Why DeepSeek-R1?

- 🧠 Excellent reasoning capabilities
- 📚 Strong coding knowledge
- 💻 Reasonable resource requirements
- 🔧 Great for development tasks

---

## Installing Ollama

```bash
# MacOS
curl -fsSL https://ollama.com/install.sh | sh

# Linux
curl -fsSL https://ollama.com/install.sh | sh

# Create systemd service for Ollama
sudo tee /etc/systemd/system/ollama.service << 'EOF'
[Unit]
Description=Ollama Service
After=network-online.target

[Service]
ExecStart=/usr/local/bin/ollama serve
Environment="OLLAMA_HOST=0.0.0.0"
User=YOUR_USERNAME
Restart=always

[Install]
WantedBy=multi-user.target
EOF
```

---

## Starting Ollama Service

```bash
# Enable and start the service
sudo systemctl daemon-reload
sudo systemctl enable ollama
sudo systemctl start ollama

# Check status
sudo systemctl status ollama
```

---

## Pulling DeepSeek-R1

```bash
# Pull the model
ollama pull deepseek-r1/deepseek-r1

# Test the model
curl -X POST http://localhost:11434/api/generate -d '{
  "model": "deepseek-r1",
  "prompt": "Write a short text adventure intro"
}'
```

---

## Model Configuration

### Key Settings
- Temperature: 0.7 (balanced creativity)
- Context Length: 8192 tokens
- Top-P: 0.9

```bash
# Create a custom model file
ollama create deepseek-adventure -f ./Modelfile
```

```dockerfile
FROM deepseek-r1

# Set parameters
PARAMETER temperature 0.7
PARAMETER top_p 0.9

# Add system prompt for adventure context
SYSTEM You are a text adventure game engine that creates immersive, interactive experiences.
```

---

## Testing Your Setup

```bash
# Test adventure generation
curl -X POST http://localhost:11434/api/generate -d '{
  "model": "deepseek-adventure",
  "prompt": "Create a dungeon entrance description"
}'
```

Expected Response:
```json
{
  "response": "Before you stands an ancient stone archway...",
  "done": true
}
```

---

## Hardware Considerations

- 🖥️ Minimum Requirements:
  - 16GB RAM
  - 8GB VRAM (GPU)
  - 30GB Storage
- 🚀 Recommended:
  - 32GB RAM
  - 12GB+ VRAM
  - SSD Storage

---

## Troubleshooting Tips

- Check logs: `journalctl -u ollama`
- Verify network: `netstat -tulpn | grep 11434`
- Monitor resources: `nvidia-smi` (GPU) or `top` (CPU)
