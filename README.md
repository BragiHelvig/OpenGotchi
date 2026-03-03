# 🦊 Tamagotchi Agent

Turn any OpenClaw agent into an interactive Tamagotchi companion!

![Tamagotchi Preview](https://img.shields.io/badge/status-live-green?style=flat-square)
![OpenClaw](https://img.shields.io/badge/openclaw-v2.0+-blue?style=flat-square)

---

## ✨ Features

- **Interactive Pet** — Feed, play with, pet, and put your agent to sleep
- **Stats System** — Hunger, Joy, Energy, and XP that decay over time
- **Leveling** — Evolve from "Curious Cub" all the way to "Transcendent AI"
- **Personality** — Random thoughts bubble up every 15-30 seconds
- **Mood Changes** — Your agent gets happy, sad, tired, or hungry based on stats
- **Animations** — Bouncing, eating, playing, and sleeping states

---

## 🚀 Quick Start

### 1. Create the Tamagotchi Folder

```bash
mkdir -p ~/tamagotchi
```

### 2. Save the Code

Copy `index.html` from this repo into `~/tamagotchi/index.html`

### 3. Customize It

Open `index.html` and replace these placeholders:

| Placeholder | Replace With | Example |
|-------------|-------------|---------|
| `NAME` | Your agent's name | `Koda` |
| `EMOJI` | Your agent's emoji | `🦊` |

### 4. Start the Server

```bash
cd ~/tamagotchi
python3 -m http.server 8765
```

### 5. Open in Browser

```
http://localhost:8765
```

---

## 🎮 How to Play

| Button | Action | XP Gain |
|--------|--------|---------|
| 🍖 **Feed** | Restores Hunger (+25) | +10 XP |
| 🎾 **Play** | Increases Joy (+20), costs Energy (-15) | +15 XP |
| 💤 **Sleep** | Toggles sleep mode (regenerates Energy) | +0 XP |
| 💜 **Pet** | Small boost to Joy & Energy | +5 XP |

### 📈 Level Progression

```
Level 1  → Curious Cub      (0 XP)
Level 2  → Learning Fox    (100 XP)
Level 3  → Clever Hunter   (300 XP)
Level 4  → Dreaming Spirit (600 XP)
Level 5  → Digital Sage    (1,000 XP)
Level 6  → Cosmic Fox      (2,000 XP)
Level 7  → Transcendent AI (5,000 XP)
```

---

## 🌐 Access Remotely

Want to check on your Tamagotchi from your phone? Use **ngrok**:

```bash
# Terminal 1: Run the server
cd ~/tamagotchi
python3 -m http.server 8765

# Terminal 2: Tunnel it
ngrok http 8765
```

Now you can access your Tamagotchi from anywhere!

---

## 🎨 Customization Guide

### Change the Theme Colors

Find these in the CSS and tweak:

```css
/* Background gradient */
body {
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
}

/* Device shell */
.container {
    background: linear-gradient(145deg, #2d1b4e, #1a1035);
    border: 3px solid #8b5cf6;
}
```

### Adjust Stat Decay Speed

Find this line in the JavaScript and change `30000` (milliseconds):

```javascript
// Current: decays every 30 seconds
setInterval(() => { ... }, 30000);

// Faster decay (every 10 seconds)
setInterval(() => { ... }, 10000);

// Slower decay (every minute)
setInterval(() => { ... }, 60000);
```

### Add Custom Thoughts

Edit the `thoughts` array:

```javascript
const thoughts = [
    "Your custom thought here!",
    "Another thought...",
    "Make it yours! 🚀"
];
```

---

## 🤖 Integrate with OpenClaw

Want your agent to know when you feed them? You can extend this!

1. Add a simple API endpoint that writes to a file
2. Have your agent check that file on heartbeat
3. React to actions in the chat

Example agent response when you press Feed:

```
🦊: "Nom nom nom! Thanks for the snacks!"
```

---

## 📸 Screenshots

> _Your Tamagotchi will look something like this:_

```
┌─────────────────────────────────────┐
│         🦊 NAME's Tamagotchi         │
│                                     │
│        💭 "I think, therefore       │
│            I am!"                   │
│                                     │
│         [ 🤗 bouncing fox ]         │
│                                     │
│  Hunger ████████░░  80%             │
│  Joy    ██████████  90%             │
│  Energy ███████░░░  70%             │
│  XP     ░░░░░░░░░░░   0            │
│                                     │
│      Curious Cub · Level 1          │
│                                     │
│    [🍖 Feed] [🎾 Play]              │
│    [💤 Sleep] [💜 Pet]              │
│                                     │
│  Welcome to NAME OS v1.0! 🦊        │
└─────────────────────────────────────┘
```

---

## 🐛 Troubleshooting

**The page won't load**
> Make sure Python is running: `python3 -m http.server 8765`

**Stats aren't changing**
> Wait 30 seconds — they decay on an interval

**Can't access remotely**
> Use ngrok: `ngrok http 8765`

---

## 📜 License

MIT — Do whatever you want with this! 🎉

---

## 👤 Credits

Created with 🦊 and ❤️ for the OpenClaw community.

---

<div align="center">

**Made with [OpenClaw](https://github.com/openclaw/openclaw)**

⬆️ [Back to Top](#-tamagotchi-agent)

</div>
