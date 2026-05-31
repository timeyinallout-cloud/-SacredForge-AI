# SacredForge AI · Quran & Bible Studio

A single-file, zero-dependency web app for generating Islamic and Christian scripture video scripts, animated canvas scenes, TTS narration, and SRT subtitle exports.

---

## Quick Start

1. Download `index.html`
2. Open it in any modern browser — no server, no install, no build step
3. Start chatting or click a suggestion tile on the welcome screen

---

## API Keys — Where to Enter Them

Both keys are entered directly in the **sidebar**, under the **AI Engine** section. Keys are held in memory only and are never stored or sent anywhere other than their respective APIs.

### Free Tier — Pollinations AI

1. Click the **✦ FREE** tab in the AI Engine section
2. You will see a **"Pollinations key (optional)"** password field
3. Paste your Pollinations API key there
4. The badge next to the Pollinations label will change from **FREE → KEY SET** to confirm
5. Use the 👁 eye button to reveal/hide the key as you type

> The app works without a key at all — adding one unlocks higher rate limits on the Pollinations side.
> Get a key at: **https://auth.pollinations.ai**

### Pro Tier — Claude Sonnet 4 (Anthropic)

1. Click the **★ PRO** tab in the AI Engine section
2. Paste your Anthropic API key (`sk-ant-…`) into the **"sk-ant-… API key"** field
3. Use the 👁 eye button to reveal/hide
4. The app will switch to Claude Sonnet 4 for all chat and script generation

> Get an Anthropic API key at: **https://console.anthropic.com**

---

## Features

| Feature | Description |
|---|---|
| 💬 AI Chat | Conversational scripture assistant — tafsir, theology, devotional writing |
| 🎬 Video Script Generator | Structured scene scripts for any Surah or Bible passage |
| 🎨 Animation Studio | Canvas-based animated backgrounds: stars, desert, ocean, temple, mountain, garden |
| 🔊 TTS Voice Studio | Browser Web Speech API with adjustable rate and pitch |
| 📄 SRT Export | Download subtitle files ready for any video editor |

---

## AI Engines

| | Free (Pollinations) | Pro (Claude) |
|---|---|---|
| **Provider** | pollinations.ai | Anthropic |
| **Model** | OpenAI-compatible default | claude-sonnet-4-20250514 |
| **API key needed?** | No (optional for higher limits) | Yes |
| **Key field location** | Sidebar → ✦ FREE tab | Sidebar → ★ PRO tab |
| **Endpoint** | `https://text.pollinations.ai/openai` | `https://api.anthropic.com/v1/messages` |

Switch between tiers at any time using the **FREE / PRO** toggle in the sidebar — takes effect on the very next message.

---

## Project Structure

Everything lives in one `index.html` file:

```
index.html
├── <style>         Dark theme, layout, splash screen, sidebar, modals (~350 lines)
├── <div id="app">  HTML — sidebar, welcome splash, chat, studio canvas, modals
└── <script>
    ├── State        conversationHistory, currentTier, pollinationsApiKey, claudeApiKey
    ├── Tier UI      setTier(), savePollinationsKey(), saveApiKey(), key visibility toggles
    ├── API calls    callAI() → callPollinations() or callClaude()
    ├── Chat         sendMessage(), addMessage(), formatResponse(), splashPrompt()
    ├── Studio       openStudio(), ssGenerate(), ssRender(), canvas animation loop
    ├── TTS          ttsBrowserSpeak(), ttsStop(), loadVoices()
    └── SRT Export   ssExportSRT()
```

---

## Welcome Screen

On first load a clean splash screen is shown (similar to Claude's home screen) with:

- SacredForge logo and tagline
- 6 suggestion tiles — click any to instantly start a chat
- Disappears automatically when the first message is sent
- Restored by clicking **+ New** in the top bar

---

## Scripture Data (Built-in)

The app ships with built-in verse data so it works even without an API call for basic display:

- **Quran** — 15 key Surahs with English translations (Sahih International)
- **Bible (KJV)** — selected Psalms and a default verse pool covering both Testaments

The AI chat can reference and expand on any scripture beyond what is built in.

---

## Security Notes

- Keys are stored **in JavaScript memory only** — they are cleared when you close or refresh the tab
- No keys are written to `localStorage`, cookies, or any server
- For a shared or public deployment, proxy API calls through a backend so keys are never in the browser at all
- Never paste API keys into chat messages — share them only via the sidebar key fields

---

## Browser Requirements

Chrome 90+ · Edge 90+ · Firefox 90+ · Safari 15+

Requires: `fetch`, `async/await`, `<canvas>`, Web Speech API (`speechSynthesis`), CSS custom properties, `dvh` units.

---

## Customisation

- **Change the Pollinations model** — edit the `model` field in `callPollinations()` (e.g. `'mistral'`, `'claude'`)
- **Add Surahs** — extend the `QURAN_SURAHS` and `QURAN_VERSES` objects
- **Add Bible books** — extend `BIBLE_CHAPTERS_COUNT` and `BIBLE_VERSES`
- **Adjust TTS defaults** — change `value` on the rate/pitch sliders in the TTS panel HTML
- **Add animation backgrounds** — extend the `drawBackground()` switch cases in the studio script
