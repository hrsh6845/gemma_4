# Gemma4 — Local AI Setup

A self-hosted AI chat setup using Google's Gemma models.
- **Ollama** runs natively on your Mac (uses Apple Metal GPU — fast)
- **Open WebUI** runs in Docker (browser chat interface)

---

## First-Time Setup

### Step 1 — Install Ollama on your Mac

```bash
brew install ollama
```

Then start the Ollama service (it runs in the background):

```bash
ollama serve
```

> You only need to do this once. After that, Ollama starts automatically at login.

---

### Step 2 — Pull the Gemma model

This downloads the model to your Mac (~1.5 GB for the 2B):

```bash
ollama pull gemma3:2b
```

Grab a coffee — first download takes a few minutes.

---

### Step 3 — Make sure Docker Desktop is running

Open Docker Desktop from your Applications folder if it's not already running.

---

### Step 4 — Start the stack

From this project folder:

```bash
docker compose up -d
```

The `-d` means "detached" — it runs in the background and gives your terminal back.

---

### Step 5 — Open the chat UI

Go to **http://localhost:3000** in your browser.

Select `gemma3:2b` from the model dropdown and start chatting!

---

## Daily Use

| What you want | Command |
|---|---|
| Start everything | `docker compose up -d` |
| Stop everything | `docker compose down` |
| Check if it's running | `docker compose ps` |
| See logs | `docker compose logs -f` |
| Chat UI | http://localhost:3000 |

---

## Changing the Model

Edit `.env` and change `OLLAMA_MODEL` to any model you want, then:

```bash
# Pull the new model first
ollama pull gemma3:9b

# Restart the stack
docker compose down && docker compose up -d
```

---

## Checkpoints — How to Save & Roll Back

Every time you make a meaningful change, save a checkpoint:

```bash
# Save current state
git add -A
git commit -m "checkpoint: describe what you changed"
git tag checkpoint-1-initial   # give it a friendly name
```

To go back to a previous checkpoint:

```bash
# See all your checkpoints
git tag

# Roll back to one
git checkout checkpoint-1-initial
docker compose down && docker compose up -d
```

---

## Project Structure

```
.
├── docker-compose.yml      ← Main stack definition
├── .env                    ← Config: model, ports, memory limits
├── .gitignore              ← Keeps model weights out of git
├── config/
│   └── open-webui/         ← Reserved for future UI config
├── data/
│   └── open-webui/         ← Chat history & settings (not committed)
└── README.md               ← You are here
```

---

## Checkpoints Log

| Tag | Description |
|---|---|
| `checkpoint-1-initial` | Ollama native + Open WebUI in Docker, Gemma 3 2B |
