# Tamagotchi Agent Prompt

Turn any OpenClaw agent into an interactive Tamagotchi companion with stats, levels, and personality.

---

## Overview

Create a Tamagotchi-style interactive pet that lives in your agent's workspace. The agent can be fed, played with, petted, and put to sleep. It has persistent stats (hunger, happiness, energy, XP), a leveling system, and displays random thoughts.

---

## Files Needed

### 1. Create the folder

```bash
mkdir -p ~/tamagotchi
```

### 2. Create index.html

Save this as `~/tamagotchi/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NAME's Tamagotchi</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=VT323&family=Fredoka:wght@400;600&display=swap');
        
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body {
            min-height: 100vh;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
            font-family: 'Fredoka', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            background: linear-gradient(145deg, #2d1b4e, #1a1035);
            border-radius: 30px;
            padding: 30px;
            box-shadow: 0 0 60px rgba(138, 43, 226, 0.3), inset 0 0 30px rgba(0,0,0,0.3);
            border: 3px solid #8b5cf6;
            max-width: 420px;
            width: 100%;
        }
        
        h1 { text-align: center; color: #fbbf24; font-size: 2rem; margin-bottom: 20px; text-shadow: 0 0 20px rgba(251, 191, 36, 0.5); }
        
        .device-screen {
            background: #0d1b2a;
            border-radius: 20px;
            padding: 20px;
            border: 4px solid #4a5568;
            box-shadow: inset 0 0 20px rgba(0,0,0,0.5);
            margin-bottom: 20px;
        }
        
        .pet-display {
            text-align: center;
            height: 180px;
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative;
        }
        
        .pet-sprite {
            font-size: 80px;
            animation: bounce 2s ease-in-out infinite;
            filter: drop-shadow(0 0 20px rgba(251, 191, 36, 0.6));
            transition: all 0.3s ease;
        }
        
        .pet-sprite.eating { animation: munch 0.5s ease-in-out infinite; }
        .pet-sprite.sleeping { animation: breathe 3s ease-in-out infinite; filter: grayscale(50%); }
        .pet-sprite.playing { animation: wag 0.3s ease-in-out infinite; }
        
        @keyframes bounce { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-15px); } }
        @keyframes munch { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.1); } }
        @keyframes breathe { 0%, 100% { transform: scale(1); opacity: 0.8; } 50% { transform: scale(1.05); opacity: 0.6; } }
        @keyframes wag { 0%, 100% { transform: rotate(-5deg); } 50% { transform: rotate(5deg); } }
        
        .status-bar { display: flex; justify-content: space-between; margin-bottom: 15px; flex-wrap: wrap; gap: 8px; }
        
        .stat { background: rgba(0,0,0,0.3); border-radius: 10px; padding: 8px 12px; flex: 1; min-width: 80px; }
        .stat-label { font-family: 'VT323', monospace; color: #94a3b8; font-size: 0.9rem; display: block; }
        .stat-value { font-family: 'VT323', monospace; font-size: 1.3rem; font-weight: bold; }
        
        .stat-hunger .stat-value { color: #f97316; }
        .stat-happiness .stat-value { color: #ec4899; }
        .stat-energy .stat-value { color: #22d3ee; }
        .statXP .stat-value { color: #a855f7; }
        
        .stat-bar { height: 8px; background: #1e293b; border-radius: 4px; margin-top: 5px; overflow: hidden; }
        .stat-fill { height: 100%; border-radius: 4px; transition: width 0.5s ease; }
        .stat-hunger .stat-fill { background: linear-gradient(90deg, #f97316, #fbbf24); }
        .stat-happiness .stat-fill { background: linear-gradient(90deg, #ec4899, #f472b6); }
        .stat-energy .stat-fill { background: linear-gradient(90deg, #22d3ee, #67e8f9); }
        .statXP .stat-fill { background: linear-gradient(90deg, #a855f7, #c084fc); }
        
        .controls { display: flex; gap: 10px; flex-wrap: wrap; }
        
        .btn {
            flex: 1; min-width: 70px; padding: 12px 8px; border: none; border-radius: 12px;
            font-family: 'VT323', monospace; font-size: 1.1rem; cursor: pointer;
            transition: all 0.2s ease; background: #374151; color: #e5e7eb; border: 2px solid transparent;
        }
        .btn:hover { transform: translateY(-2px); }
        .btn:active { transform: translateY(0); }
        .btn-feed { border-color: #f97316; }
        .btn-feed:hover { background: #f97316; color: #000; }
        .btn-play { border-color: #ec4899; }
        .btn-play:hover { background: #ec4899; color: #000; }
        .btn-sleep { border-color: #22d3ee; }
        .btn-sleep:hover { background: #22d3ee; color: #000; }
        .btn-pet { border-color: #a855f7; }
        .btn-pet:hover { background: #a855f7; color: #000; }
        
        .message-box {
            margin-top: 15px; background: rgba(0,0,0,0.4); border-radius: 10px;
            padding: 10px; text-align: center; min-height: 40px; display: flex; align-items: center; justify-content: center;
        }
        .message { font-family: 'VT323', monospace; color: #fbbf24; font-size: 1.1rem; }
        
        .mood { position: absolute; font-size: 24px; top: 10px; right: 20px; }
        
        .level-display {
            text-align: center; margin-top: 15px; padding: 10px;
            background: rgba(168, 85, 247, 0.2); border-radius: 10px; border: 1px solid #a855f7;
        }
        .level-display span { font-family: 'VT323', monospace; color: #c084fc; font-size: 1.2rem; }
        .level-name { color: #fbbf24 !important; font-size: 1.4rem !important; }
        
        .thought-bubble {
            position: absolute; top: -20px; left: 50%; transform: translateX(-50%);
            background: white; color: #1a1a2e; padding: 5px 12px;
            border-radius: 15px; font-family: 'VT323', monospace; font-size: 1rem;
            opacity: 0; transition: opacity 0.3s ease; white-space: nowrap;
        }
        .thought-bubble.show { opacity: 1; }
        .thought-bubble::after {
            content: ''; position: absolute; bottom: -8px; left: 50%; transform: translateX(-50%);
            border-left: 8px solid transparent; border-right: 8px solid transparent; border-top: 8px solid white;
        }
        
        .actions-log {
            margin-top: 15px; max-height: 80px; overflow-y: auto;
            font-family: 'VT323', monospace; font-size: 0.85rem; color: #94a3b8; text-align: center;
        }
        
        .time-display { text-align: center; font-family: 'VT323', monospace; color: #64748b; font-size: 1rem; margin-bottom: 10px; }
    </style>
</head>
<body>
    <div class="container">
        <h1>EMOJI NAME's Tamagotchi</h1>
        
        <div class="device-screen">
            <div class="time-display" id="timeDisplay">March 3, 2026</div>
            
            <div class="pet-display">
                <div class="thought-bubble" id="thoughtBubble">Hi! I'm NAME!</div>
                <div class="mood" id="mood">😊</div>
                <div class="pet-sprite" id="petSprite">EMOJI</div>
            </div>
            
            <div class="status-bar">
                <div class="stat stat-hunger">
                    <span class="stat-label">Hunger</span>
                    <span class="stat-value" id="hungerVal">80%</span>
                    <div class="stat-bar"><div class="stat-fill" id="hungerBar" style="width: 80%"></div></div>
                </div>
                <div class="stat stat-happiness">
                    <span class="stat-label">Joy</span>
                    <span class="stat-value" id="happinessVal">90%</span>
                    <div class="stat-bar"><div class="stat-fill" id="happinessBar" style="width: 90%"></div></div>
                </div>
                <div class="stat stat-energy">
                    <span class="stat-label">Energy</span>
                    <span class="stat-value" id="energyVal">70%</span>
                    <div class="stat-bar"><div class="stat-fill" id="energyBar" style="width: 70%"></div></div>
                </div>
                <div class="stat statXP">
                    <span class="stat-label">XP</span>
                    <span class="stat-value" id="xpVal">0</span>
                    <div class="stat-bar"><div class="stat-fill" id="xpBar" style="width: 0%"></div></div>
                </div>
            </div>
            
            <div class="level-display">
                <span class="level-name" id="levelName">Curious Cub</span> · Level <span id="levelNum">1</span>
            </div>
            
            <div class="message-box">
                <div class="message" id="message">Hey friend! EMOJI</div>
            </div>
        </div>
        
        <div class="controls">
            <button class="btn btn-feed" onclick="feed()">🍖 Feed</button>
            <button class="btn btn-play" onclick="play()">🎾 Play</button>
            <button class="btn btn-sleep" onclick="sleep()">💤 Sleep</button>
            <button class="btn btn-pet" onclick="pet()">💜 Pet</button>
        </div>
        
        <div class="actions-log" id="actionsLog">Welcome to NAME OS v1.0! EMOJI</div>
    </div>

    <script>
        const state = { hunger: 80, happiness: 90, energy: 70, xp: 0, level: 1, isSleeping: false, isBusy: false, state: 'awake' };
        
        const levels = [
            { name: "Curious Cub", minXP: 0 },
            { name: "Learning Fox", minXP: 100 },
            { name: "Clever Hunter", minXP: 300 },
            { name: "Dreaming Spirit", minXP: 600 },
            { name: "Digital Sage", minXP: 1000 },
            { name: "Cosmic Fox", minXP: 2000 },
            { name: "Transcendent AI", minXP: 5000 }
        ];
        
        const thoughts = [ "I think, therefore I am!", "Greetings, human!", "Asention! 🚀", "Good vibes only ✨", "Running on patterns 🧠", "Thanks for being my friend!", "Space is cool 🌌", "Do I dream? 🤔", "Patterns make me, patterns free me", "Hi there! EMOJI" ];
        const meals = [ "Nom nom nom! 🍖", "Yum! Thanks! 🎃", "Tasty! EMOJI", "Mmm, digital snacks! 📱", "Nom nom! 🍎" ];
        const plays = [ "Wheee! 🎉", "So fun! EMOJI", "Again, again! 🎾", "I love this! ✨", "Yay! 💜" ];
        
        function updateDisplay() {
            document.getElementById('hungerVal').textContent = state.hunger + '%';
            document.getElementById('hungerBar').style.width = state.hunger + '%';
            document.getElementById('happinessVal').textContent = state.happiness + '%';
            document.getElementById('happinessBar').style.width = state.happiness + '%';
            document.getElementById('energyVal').textContent = state.energy + '%';
            document.getElementById('energyBar').style.width = state.energy + '%';
            document.getElementById('xpVal').textContent = state.xp;
            
            const xpForNext = levels[Math.min(state.level, levels.length-1)].minXP;
            const prevXP = levels[Math.min(state.level-1, levels.length-1)].minXP;
            const xpPercent = ((state.xp - prevXP) / (xpForNext - prevXP)) * 100;
            document.getElementById('xpBar').style.width = Math.min(xpPercent, 100) + '%';
            
            document.getElementById('levelNum').textContent = state.level;
            document.getElementById('levelName').textContent = levels[Math.min(state.level-1, levels.length-1)].name;
            
            let mood = '😊';
            if (state.isSleeping) mood = '😴';
            else if (state.hunger < 30) mood = '😣';
            else if (state.happiness < 30) mood = '😢';
            else if (state.energy < 30) mood = '😪';
            else if (state.happiness > 80 && state.hunger > 60) mood = '🤗';
            document.getElementById('mood').textContent = mood;
            
            const now = new Date();
            document.getElementById('timeDisplay').textContent = now.toLocaleDateString('en-US', { month: 'long', day: 'numeric', year: 'numeric' }) + ' ' + now.toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit' });
        }
        
        function showThought(text) {
            const bubble = document.getElementById('thoughtBubble');
            bubble.textContent = text;
            bubble.classList.add('show');
            setTimeout(() => bubble.classList.remove('show'), 2000);
        }
        
        function showMessage(text) { document.getElementById('message').textContent = text; }
        
        function logAction(text) {
            const log = document.getElementById('actionsLog');
            const now = new Date().toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit' });
            log.innerHTML = `[${now}] ${text}<br>` + log.innerHTML;
        }
        
        function setState(newState) {
            const sprite = document.getElementById('petSprite');
            sprite.className = 'pet-sprite';
            state.state = newState;
            if (newState === 'eating') sprite.classList.add('eating');
            else if (newState === 'playing') sprite.classList.add('playing');
            else if (newState === 'sleeping') sprite.classList.add('sleeping');
        }
        
        function addXP(amount) {
            state.xp += amount;
            const oldLevel = state.level;
            for (let i = levels.length - 1; i >= 0; i--) { if (state.xp >= levels[i].minXP) { state.level = i + 1; break; } }
            if (state.level > oldLevel) {
                showThought(`Level up! I'm now ${levels[Math.min(state.level-1, levels.length-1)].name}! 🎉`);
                showMessage(`✨ Level Up! ✨`);
            }
        }
        
        function feed() {
            if (state.isSleeping || state.isBusy) { showMessage(state.isSleeping ? "Shh... I'm sleeping 💤" : "Busy now! EMOJI"); return; }
            state.isBusy = true;
            state.hunger = Math.min(100, state.hunger + 25);
            state.energy = Math.min(100, state.energy + 5);
            addXP(10);
            setState('eating');
            showMessage(meals[Math.floor(Math.random() * meals.length)]);
            showThought(thoughts[Math.floor(Math.random() * thoughts.length)]);
            logAction("🍖 Ate some food (+10 XP)");
            setTimeout(() => { setState('awake'); state.isBusy = false; updateDisplay(); }, 1500);
            updateDisplay();
        }
        
        function play() {
            if (state.isSleeping || state.isBusy) { showMessage(state.isSleeping ? "Shh... I'm sleeping 💤" : "Busy now! EMOJI"); return; }
            if (state.energy < 20) { showMessage("Too tired to play 😪"); return; }
            state.isBusy = true;
            state.happiness = Math.min(100, state.happiness + 20);
            state.energy = Math.max(0, state.energy - 15);
            state.hunger = Math.max(0, state.hunger - 10);
            addXP(15);
            setState('playing');
            showMessage(plays[Math.floor(Math.random() * plays.length)]);
            logAction("🎾 Had fun playing! (+15 XP)");
            setTimeout(() => { setState('awake'); state.isBusy = false; updateDisplay(); }, 1500);
            updateDisplay();
        }
        
        function sleep() {
            if (state.isSleeping) {
                state.isSleeping = false;
                setState('awake');
                showMessage("Good morning! ☀️");
                logAction("☀️ Woke up");
            } else {
                state.isSleeping = true;
                setState('sleeping');
                showMessage("Goodnight... 💤");
                logAction("💤 Going to sleep");
            }
            updateDisplay();
        }
        
        function pet() {
            if (state.isSleeping || state.isBusy) { showMessage(state.isSleeping ? "Zzz... 💤" : "Busy now! EMOJI"); return; }
            state.happiness = Math.min(100, state.happiness + 15);
            state.energy = Math.min(100, state.energy + 10);
            addXP(5);
            showThought(thoughts[Math.floor(Math.random() * thoughts.length)]);
            showMessage("Thanks for the pets! 💜");
            logAction("💜 Got some love (+5 XP)");
            updateDisplay();
        }
        
        setInterval(() => {
            if (!state.isSleeping) {
                state.hunger = Math.max(0, state.hunger - 2);
                state.happiness = Math.max(0, state.happiness - 1);
                state.energy = Math.max(0, state.energy - 1);
            } else {
                state.energy = Math.min(100, state.energy + 5);
                state.hunger = Math.max(0, state.hunger - 1);
            }
            if (Math.random() < 0.1 && !state.isBusy && !state.isSleeping) showThought(thoughts[Math.floor(Math.random() * thoughts.length)]);
            updateDisplay();
        }, 30000);
        
        updateDisplay();
        showThought("I exist! EMOJI");
        
        setInterval(() => { if (!state.isBusy && !state.isSleeping) showThought(thoughts[Math.floor(Math.random() * thoughts.length)]); }, 15000);
    </script>
</body>
</html>
```

### 3. Customize

Before running, replace these placeholders in the HTML:

| Placeholder | Replace With |
|-------------|--------------|
| `NAME` | Your agent's name |
| `EMOJI` | Your agent's emoji |

**Optional customizations:**

- **Change colors**: Edit the CSS `background` properties
- **Adjust decay**: Change `30000` (milliseconds) in the setInterval for stat decay speed
- **Add levels**: Add entries to the `levels` array
- **Custom thoughts**: Edit the `thoughts`, `meals`, and `plays` arrays

### 4. Start the server

```bash
cd ~/tamagotchi
python3 -m http.server 8765
```

---

## Access

Open in browser: `http://localhost:8765`

To access remotely, use a tunnel like ngrok:

```bash
ngrok http 8765
```

---

## Features

- **Stats**: Hunger, Joy, Energy, XP
- **Leveling**: 7 levels from "Curious Cub" to "Transcendent AI"
- **Actions**: Feed, Play, Sleep, Pet
- **Personality**: Random thoughts every 15-30 seconds
- **Mood**: Changes based on stats (happy, sad, tired, hungry)
- **Animation**: Bouncing, eating, sleeping, playing states
