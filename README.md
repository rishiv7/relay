# Relay

An iOS Shortcut that acts as an AI agent living wherever you are on your phone. Press one button, speak your intent — it reads your screen and drops a draft straight to your clipboard.

No app switching. No re-explaining context. Just paste.

---

## How It Works

1. Press the **Action Button** (or trigger via Back Tap)
2. Shortcut takes a screenshot of your current screen
3. Speak your instructions (or stay silent to auto-draft)
4. Claude reads the screenshot and your voice input
5. Draft is copied to your clipboard
6. Paste directly into any text field

---

## Setup

### Prerequisites
- iPhone with Action Button (iPhone 15 Pro / 16 series) or any iPhone for Back Tap
- iOS 16+
- A Claude API key from [console.anthropic.com](https://console.anthropic.com)

### Steps

**1. Build the Shortcut**

Open the **Shortcuts app** on your iPhone and create a new shortcut with the following actions in order:

| Step | Action | Settings |
|---|---|---|
| 1 | Take Screenshot | — |
| 2 | Encode Media | Encoding: Base64, Line Breaks: None |
| 3 | Dictate Text | Stop Listening: After Pause. Rename output → `VoiceInstructions` |
| 4 | Text | Paste the request body below. Rename output → `RequestBody` |
| 5 | Get Contents of URL | See API call settings below |
| 6 | Get Dictionary from Input | Input: result of step 5 |
| 7 | Get Dictionary Value | Key: `content` |
| 8 | Get Item from List | First Item |
| 9 | Get Dictionary Value | Key: `text` |
| 10 | Copy to Clipboard | — |
| 11 | Show Notification | Title: `Reply ready`, Body: result of step 9 |

---

**2. Request Body (paste into Step 4 Text action)**

Replace `[ENCODED MEDIA]` with the Encoded Media variable from Step 2.
Replace `[VOICE INSTRUCTIONS]` with the VoiceInstructions variable from Step 3.

```json
{"model":"claude-haiku-4-5-20251001","max_tokens":4096,"messages":[{"role":"user","content":[{"type":"image","source":{"type":"base64","media_type":"image/png","data":"[ENCODED MEDIA]"}},{"type":"text","text":"You are an AI assistant helping a user reply to messages on their phone. Analyze the conversation in this screenshot. Draft a natural, concise reply that matches the tone. If the user provided instructions, follow them: [VOICE INSTRUCTIONS]. Return ONLY the reply text, no explanations."}]}]}
```

---

**3. API Call Settings (Step 5)**

- **URL:** `https://api.anthropic.com/v1/messages`
- **Method:** POST
- **Headers:**

| Key | Value |
|---|---|
| `x-api-key` | `YOUR_API_KEY_HERE` |
| `anthropic-version` | `2023-06-01` |
| `content-type` | `application/json` |

- **Request Body:** File → select `RequestBody` variable from Step 4

---

**4. Assign to Action Button**

Settings → Action Button → Shortcut → select your shortcut

Or for **Back Tap**: Settings → Accessibility → Touch → Back Tap → Double/Triple Tap → select your shortcut

---

## Customising the System Prompt

The system prompt is the text inside the `"text"` field in the request body. Edit it to change how the AI behaves — match your tone, language, or add context about yourself.

---

## Roadmap

- [ ] Backend API so the Claude key is never exposed
- [ ] Personalisation layer — learns your writing style over time
- [ ] Per-contact and per-app tone profiles
- [ ] Calendar and contacts context awareness
- [ ] Agent builder UI for non-technical users
- [ ] Multi-screenshot context capture via screen recording

---

## Contributing

Early days. If you try this and have feedback, open an issue or reach out on X.
