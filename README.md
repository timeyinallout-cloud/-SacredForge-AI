# SacredForge AI · Quran & Bible Studio

A single-file, client-side web application for generating Islamic and Christian scripture video scripts, animated canvases, TTS narration, and SRT subtitle exports — powered by the **Anthropic Claude API**.

---

## ⚠️ API Notice — Not Pollinations

> **This file does NOT use the Pollinations API.**
> It calls the **Anthropic Claude API** directly:
>
> ```
> POST https://api.anthropic.com/v1/messages
> Model: claude-sonnet-4-20250514
> ```
>
> The sidebar also displays **"Claude Sonnet 4 · FREE"** as the AI model label.
> If you intended to use the Pollinations API (`https://text.pollinations.ai` or `https://image.pollinations.ai`), the `callAI()` function (line ~759) will need to be updated.

---

## Features

| Feature | Description |
|---|---|
| 💬 **AI Chat** | Conversational scripture assistant — tafsir, theology, devotional writing |
| 🎬 **Video Script Generator** | Structured scene scripts for Quran Surahs or Bible passages |
| 🎨 **Animation Studio** | Canvas-based animated backgrounds (stars, desert, ocean, temple, mountain, garden) |
| 🔊 **TTS Voice Studio** | Browser Web Speech API with rate/pitch controls |
| 📄 **SRT Export** | Download subtitle files for use in video editors |

---

## How to Use

1. **Open `index.html`** in any modern browser (Chrome, Edge, Firefox, Safari). No server or build step needed.
2. **Set your API key.** The app calls `https://api.anthropic.com/v1/messages` directly from the browser. You must either:
   - Add your Anthropic API key in the `callAI()` function headers: `'x-api-key': 'YOUR_KEY_HERE'`
   - Or proxy requests through a backend to keep the key secure (recommended for production).
3. **Choose a scripture source** from the sidebar: Holy Quran, Holy Bible (KJV), or Both.
4. **Chat freely** or use the Quick Action buttons to open the Video Generator modal.
5. **Open Studio** to animate and preview generated scenes on the canvas.
6. **Export SRT** subtitles from the studio toolbar when your scene list is ready.

---

## Project Structure

Everything is self-contained in a single `index.html` file:

```
index.html
├── <style>          CSS — dark theme, layout, components (lines 9–350)
├── <div id="app">   HTML — sidebar, chat, modals, studio canvas
└── <script>
    ├── Data         QURAN_SURAHS, QURAN_VERSES, BIBLE_VERSES constants
    ├── callAI()     Anthropic API call (single function, line ~759)
    ├── Chat         sendMessage(), addMessage(), formatResponse()
    ├── Studio       openStudio(), ssGenerate(), ssRender(), animation loop
    ├── TTS          ttsBrowserSpeak(), ttsStop(), loadVoices()
    └── SRT Export   ssExportSRT(), toSRTTime()
```

---

## AI / API Details

```js
// callAI() — lines ~759–777
fetch('https://api.anthropic.com/v1/messages', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    model: 'claude-sonnet-4-20250514',
    max_tokens: 4000,
    system: getSystemPrompt(),
    messages,   // full conversation history
  }),
});
```

The system prompt (`getSystemPrompt()`) instructs the model to act as a reverent scripture assistant and to output structured `[SCENE N: ...]` blocks when generating video scripts.

---

## Security Warning

**Never expose your Anthropic API key in a public-facing HTML file.** The current implementation makes direct browser-to-API calls with no key obfuscation. For any deployment beyond local personal use, route API calls through a server-side proxy.

---

## Fonts & External Dependencies

| Resource | Source |
|---|---|
| Cinzel, Lora, JetBrains Mono | Google Fonts CDN |
| Web Speech API (TTS) | Native browser API — no external dependency |
| Canvas animation | Native `<canvas>` — no library |
| Anthropic API | `https://api.anthropic.com` |

No npm packages, no build tools, no frameworks. Pure HTML/CSS/JS.

---

## Customisation Tips

- **Add more Surahs** — extend the `QURAN_SURAHS` and `QURAN_VERSES` objects.
- **Add more Bible books** — extend `BIBLE_CHAPTERS_COUNT` and `BIBLE_VERSES`.
- **Change the AI model** — swap `claude-sonnet-4-20250514` in `callAI()` for another Anthropic model string.
- **Switch to Pollinations** — replace the `callAI()` fetch target with `https://text.pollinations.ai/openai` and adjust the request body to match the Pollinations OpenAI-compatible schema.
- **Add music** — the studio canvas loop is a good injection point for a Web Audio API background track.

---

## Browser Compatibility

Requires a modern browser with support for: `fetch`, `async/await`, `<canvas>`, Web Speech API (`speechSynthesis`), CSS custom properties, and `dvh` units.
Chrome 90+ / Edge 90+ / Firefox 90+ / Safari 15+ recommended.
