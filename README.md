# Clicky Cross-Platform 🖥️

> **An AI companion that lives next to your cursor.**
> Ask it anything about your screen — it points, explains, and guides you step-by-step, like a real tutor sitting beside you.

Forked from [Bitshank-2338/clicky-windows](https://github.com/Bitshank-2338/clicky-windows) (originally a port of [farzaa/clicky](https://github.com/farzaa/clicky)).

This fork adds **Kimi (Moonshot AI) as a first-class provider** and makes the app **cross-platform** (Windows, macOS, Linux).

---

## What is this?

Clicky is a little AI buddy that **lives next to your cursor**. You hold a hotkey, ask it something about your screen, and it talks back — pointing at buttons, walking you through steps, drawing arrows on your screen. Think of it as having a patient tutor sitting beside you while you learn anything: video editing, coding, a new app, whatever.

**No more Alt-Tab to ChatGPT.** No more typing out descriptions of what's on your screen. Just hold **Ctrl + Alt + Space**, speak, and Clicky handles the rest.

Works **100% offline** with Ollama, or plug in your **Kimi API key** (or Claude / OpenAI / Gemini) for the full experience.

---

## Key Differences from Upstream

| Feature | Upstream (clicky-windows) | This Fork |
|---|---|---|
| **Primary LLM** | Claude (priority) | **Kimi / Moonshot AI** |
| **Platform** | Windows only | **Windows, macOS, Linux** |
| **Subscriptions needed** | 3+ (Claude, ElevenLabs, etc.) | **1** (Kimi only — everything else local) |
| **Local STT** | faster-whisper | faster-whisper |
| **Local TTS** | edge-tts | edge-tts |

---

## Quick Start

### 1. Prerequisites

- **Python 3.11+**
- **Git**

### 2. Clone & Install

```bash
git clone https://github.com/YOUR_USERNAME/clicky-crossplatform.git
cd clicky-crossplatform
python -m venv venv

# Windows:
venv\Scripts\activate
# macOS / Linux:
source venv/bin/activate

pip install -r requirements.txt
```

### 3. Configure

Copy the example env file and add your **Kimi API key**:

```bash
cp .env.example .env
```

Edit `.env`:

```env
KIMI_API_KEY=your-moonshot-api-key-here
KIMI_MODEL=kimi-k2-vision-preview
```

> Get your key at: https://platform.moonshot.ai/

**That's it.** Leave everything else empty — Clicky will use free local alternatives for STT (faster-whisper) and TTS (edge-tts).

### 4. Run

```bash
python main.py
```

---

## Fully Local Mode (Zero API Keys)

If you want to run **completely offline**, install [Ollama](https://ollama.com/) and pull a vision model:

```bash
ollama pull llama3.2-vision
```

Then leave `KIMI_API_KEY` empty in `.env`. Clicky will automatically fall back to Ollama.

---

## Platform Notes

| Platform | Notes |
|---|---|
| **Windows** | Fully supported. All features work out of the box. |
| **macOS** | Fully supported. Hotkeys work globally. May need microphone permissions on first run. |
| **Linux** | Supported. Requires a running desktop environment (X11 or Wayland). `xdg-open` used for file opening. |

---

## Architecture

```
┌─────────────────────────────────────────────┐
│  Your screen (browser, IDE, Premiere, etc.) │
│                                             │
│          🔵◂  ← Clicky blue buddy           │
│          (floats beside your real cursor)   │
│                                             │
│  ┌──────────────────────────┐               │
│  │  Clicky  [Kimi]      —   │  ← panel      │
│  │  ● Thinking…             │               │
│  │  "The search bar is      │               │
│  │   right here ↗"          │               │
│  │  Model: kimi-k2-vision…  │               │
│  └──────────────────────────┘               │
└─────────────────────────────────────────────┘
```

---

## Feature List

### 🎙️ Voice Activation
- Hold **Ctrl + Alt + Space** to push-to-talk
- Say **"Clicky"** for hands-free wake word
- Press **Esc** to stop any response or TTS mid-stream

### 👁️ Screen Aware
- Full multi-monitor screenshot on every query
- Describes only what it sees — never hallucinates
- Detects active window title for per-app memory

### 🎯 Pixel-Perfect Pointing
- **Two-stage grid locator** works with *any* vision LLM
- Bezier arc flight with configurable teacher pace
- Pulsing highlight ring + speech bubble label on the target

### 🔒 Privacy-First Defaults
- **STT:** local Whisper (free, no cloud) — default
- **TTS:** Microsoft Edge TTS (free, no API key) — default
- **Search:** DuckDuckGo (free, no key) — default
- **LLM:** Your choice — Kimi, Ollama (local), or legacy providers

---

## Supported LLM Providers

| Provider | API Key | Vision | Notes |
|---|---|---|---|
| **Kimi (Moonshot AI)** | `KIMI_API_KEY` | ✅ | **Recommended. Single subscription.** |
| Ollama | none | ✅ | Fully local, free |
| Claude | `ANTHROPIC_API_KEY` | ✅ | Legacy support |
| OpenAI | `OPENAI_API_KEY` | ✅ | Legacy support |
| Gemini | `GOOGLE_API_KEY` | ✅ | Legacy support |
| GitHub Copilot | OAuth | ✅ | Legacy support |

---

## Development

### Project Layout

```
clicky-crossplatform/
├── ai/               # LLM providers (Kimi, Claude, Ollama, …)
├── audio/            # Mic capture, STT, TTS
├── screen/           # Screenshot helpers
├── ui/               # Qt overlay, panel, tray
├── tutor_features/   # Code mode, journal, OCR, …
├── config.py         # Settings + .env loader
├── main.py           # Entry point
└── requirements.txt
```

### Adding a New Provider

1. Create `ai/your_provider.py` inheriting from `BaseLLMProvider`
2. Register it in `config.py` (`llm_provider()` and `available_llm_providers()`)
3. Hook it into `companion_manager.py` (`_get_llm()` and `set_active_provider()`)
4. (Optional) Add a model fetcher in `ai/model_registry.py`

---

## License

MIT — same as upstream. Go wild.

---

## Credits

- Original macOS concept: [farzaa/clicky](https://github.com/farzaa/clicky)
- Windows port + local-first stack: [Bitshank-2338/clicky-windows](https://github.com/Bitshank-2338/clicky-windows)
- Cross-platform + Kimi integration: this fork
