# Relay → Telegram (Openclaw)

A voice-first iOS Shortcut that transcribes what you say and sends it directly to your Openclaw chat on Telegram — no tapping Send, no screen reading, no reply context baked in. You control when the recording stops.

---

## How It Works

1. Press the **Action Button** (or Back Tap)
2. Speak your prompt — take as long as you need
3. Tap the mic button on screen to stop recording when you're done
4. Message is sent automatically to your Openclaw Telegram chat
5. Short notification confirms it was delivered

---

## What You Need Before Starting

- **Bot token** — from a Telegram bot you control (create one via [@BotFather](https://t.me/BotFather) if you don't have one). This bot is the sender; it must have already started a conversation with your Openclaw chat.
- **Chat ID** — the Telegram chat ID of your Openclaw conversation. The easiest way to find it: forward a message from that chat to [@userinfobot](https://t.me/userinfobot) and it will return the chat ID.

---

## Setup

### Build the Shortcut

Open the **Shortcuts app** and create a new shortcut named **Relay → Openclaw** with these steps in order:

| Step | Action | Settings |
|---|---|---|
| 1 | Dictate Text | Language: Default · Stop Listening: **On Tap**. Rename output → `VoiceInput` |
| 2 | Text | Paste the request body below. Rename output → `RequestBody` |
| 3 | Get Contents of URL | See API call settings below |
| 4 | Show Notification | Title: `Sent to Openclaw` · Body: `VoiceInput` variable from Step 1 |

---

### Request Body (paste into Step 2 Text action)

Replace `YOUR_CHAT_ID` with your Openclaw chat ID (keep the quotes).  
Replace `[VOICE INPUT]` with the `VoiceInput` variable from Step 1.

```json
{"chat_id":"YOUR_CHAT_ID","text":"[VOICE INPUT]"}
```

---

### API Call Settings (Step 3)

- **URL:** `https://api.telegram.org/botYOUR_BOT_TOKEN/sendMessage`  
  *(replace `YOUR_BOT_TOKEN` with your actual token — it goes right after the word `bot` in the URL, no spaces)*
- **Method:** POST
- **Headers:**

| Key | Value |
|---|---|
| `Content-Type` | `application/json` |

- **Request Body:** File → select `RequestBody` variable from Step 2

---

### Assign to Action Button

Settings → Action Button → Shortcut → select **Relay → Openclaw**

Or for **Back Tap**: Settings → Accessibility → Touch → Back Tap → Double/Triple Tap → select **Relay → Openclaw**

---

## How the Voice Recording Works

The **Stop Listening: On Tap** setting means iOS will keep recording until you tap the microphone button on screen — no automatic cutoff after a pause. You can speak in fragments, pause to think, and keep going. Tap when you're done.

---

## Tips

- If your message is long or conversational, Telegram has a 4096-character text limit. For typical voice prompts this won't be an issue.
- The notification shows your transcribed text so you can verify what was sent without opening Telegram.
- To add `parse_mode` (Markdown formatting) in Openclaw's replies, add `"parse_mode":"Markdown"` to the request body — not needed for sending, but useful if Openclaw responds with formatted text.
