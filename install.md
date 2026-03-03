# One-Shot Tamagotchi Install

Copy-paste this into your OpenClaw agent:

---

**Create an interactive Tamagotchi dashboard for me:**

1. Create folder `~/tamagotchi`
2. Save the HTML below as `~/tamagotchi/index.html`
3. Replace `NAME` with my agent's name and `EMOJI` with my agent's emoji in the HTML
4. Start server: `cd ~/tamagotchi && python3 -m http.server 8765`
5. Tell me the URL to access it

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NAME's Tamagotchi</title>
    <link href="https://fonts.googleapis.com/css2?family=VT323&family=Fredoka:wght@400;600&display=swap" rel="stylesheet">
    <style>
        *{margin:0;padding:0;box-sizing:border-box}body{min-height:100vh;background:linear-gradient(135deg,#1a1a2e,#16213e 50%,#0f3460);font-family:'Fredoka',sans-serif;display:flex;justify-content:center;align-items:center;padding:20px}.container{background:linear-gradient(145deg,#2d1b4e,#1a1035);border-radius:30px;padding:30px;box-shadow:0 0 60px rgba(138,43,226,.3),inset 0 0 30px rgba(0,0,0,.3);border:3px solid #8b5cf6;max-width:420px;width:100%}h1{text-align:center;color:#fbbf24;font-size:2rem;margin-bottom:20px;text-shadow:0 0 20px rgba(251,191,36,.5)}.device-screen{background:#0d1b2a;border-radius:20px;padding:20px;border:4px solid #4a5568;box-shadow:inset 0 0 20px rgba(0,0,0,.5);margin-bottom:20px}.pet-display{text-align:center;height:180px;display:flex;justify-content:center;align-items:center;position:relative}.pet-sprite{font-size:80px;animation:bounce 2s ease-in-out infinite;filter:drop-shadow(0 0 20px rgba(251,191,36,.6));transition:all .3s ease}.pet-sprite.eating{animation:munch .5s ease-in-out infinite}.pet-sprite.sleeping{animation:breathe 3s ease-in-out infinite;filter:grayscale(50%)}.pet-sprite.playing{animation:wag .3s ease-in-out infinite}@keyframes bounce{0%,100%{transform:translateY(0)}50%{transform:translateY(-15px)}}@keyframes munch{0%,100%{transform:scale(1)}50%{transform:scale(1.1)}}@keyframes breathe{0%,100%{transform:scale(1);opacity:.8}50%{transform:scale(1.05);opacity:.6}}@keyframes wag{0%,100%{transform:rotate(-5deg)}50%{transform:rotate(5deg)}}.status-bar{display:flex;justify-content:space-between;margin-bottom:15px;flex-wrap:wrap;gap:8px}.stat{background:rgba(0,0,0,.3);border-radius:10px;padding:8px 12px;flex:1;min-width:80px}.stat-label{font-family:'VT323',monospace;color:#94a3b8;font-size:.9rem;display:block}.stat-value{font-family:'VT323',monospace;font-size:1.3rem;font-weight:bold}.stat-hunger .stat-value{color:#f97316}.stat-happiness .stat-value{color:#ec4899}.stat-energy .stat-value{color:#22d3ee}.statXP .stat-value{color:#a855f7}.stat-bar{height:8px;background:#1e293b;border-radius:4px;margin-top:5px;overflow:hidden}.stat-fill{height:100%;border-radius:4px;transition:width .5s ease}.stat-hunger .stat-fill{background:linear-gradient(90deg,#f97316,#fbbf24)}.stat-happiness .stat-fill{background:linear-gradient(90deg,#ec4899,#f472b6)}.stat-energy .stat-fill{background:linear-gradient(90deg,#22d3ee,#67e8f9)}.statXP .stat-fill{background:linear-gradient(90deg,#a855f7,#c084fc)}.controls{display:flex;gap:10px;flex-wrap:wrap}.btn{flex:1;min-width:70px;padding:12px 8px;border:none;border-radius:12px;font-family:'VT323',monospace;font-size:1.1rem;cursor:pointer;transition:all .2s ease;background:#374151;color:#e5e7eb;border:2px solid transparent}.btn:hover{transform:translateY(-2px)}.btn-feed{border-color:#f97316}.btn-feed:hover{background:#f97316;color:#000}.btn-play{border-color:#ec4899}.btn-play:hover{background:#ec4899;color:#000}.btn-sleep{border-color:#22d3ee}.btn-sleep:hover{background:#22d3ee;color:#000}.btn-pet{border-color:#a855f7}.btn-pet:hover{background:#a855f7;color:#000}.message-box{margin-top:15px;background:rgba(0,0,0,.4);border-radius:10px;padding:10px;text-align:center;min-height:40px;display:flex;align-items:center;justify-content:center}.message{font-family:'VT323',monospace;color:#fbbf24;font-size:1.1rem}.mood{position:absolute;font-size:24px;top:10px;right:20px}.level-display{text-align:center;margin-top:15px;padding:10px;background:rgba(168,85,247,.2);border-radius:10px;border:1px solid #a855f7}.level-display span{font-family:'VT323',monospace;color:#c084fc;font-size:1.2rem}.level-name{color:#fbbf24!important;font-size:1.4rem!important}.thought-bubble{position:absolute;top:-20px;left:50%;transform:translateX(-50%);background:#fff;color:#1a1a2e;padding:5px 12px;border-radius:15px;font-family:'VT323',monospace;font-size:1rem;opacity:0;transition:opacity .3s ease;white-space:nowrap}.thought-bubble.show{opacity:1}.thought-bubble::after{content:'';position:absolute;bottom:-8px;left:50%;transform:translateX(-50%);border-left:8px solid transparent;border-right:8px solid transparent;border-top:8px solid #fff}.actions-log{margin-top:15px;max-height:80px;overflow-y:auto;font-family:'VT323',monospace;font-size:.85rem;color:#94a3b8;text-align:center}.time-display{text-align:center;font-family:'VT323',monospace;color:#64748b;font-size:1rem;margin-bottom:10px}
    </style>
</head>
<body>
<div class="container">
<h1>EMOJI NAME's Tamagotchi</h1>
<div class="device-screen">
<div class="time-display" id="timeDisplay"></div>
<div class="pet-display">
<div class="thought-bubble" id="thoughtBubble">Hi! I'm NAME!</div>
<div class="mood" id="mood">😊</div>
<div class="pet-sprite" id="petSprite">EMOJI</div>
</div>
<div class="status-bar">
<div class="stat stat-hunger"><span class="stat-label">Hunger</span><span class="stat-value" id="hungerVal">80%</span><div class="stat-bar"><div class="stat-fill" id="hungerBar" style="width:80%"></div></div></div>
<div class="stat stat-happiness"><span class="stat-label">Joy</span><span class="stat-value" id="happinessVal">90%</span><div class="stat-bar"><div class="stat-fill" id="happinessBar" style="width:90%"></div></div></div>
<div class="stat stat-energy"><span class="stat-label">Energy</span><span class="stat-value" id="energyVal">70%</span><div class="stat-bar"><div class="stat-fill" id="energyBar" style="width:70%"></div></div></div>
<div class="stat statXP"><span class="stat-label">XP</span><span class="stat-value" id="xpVal">0</span><div class="stat-bar"><div class="stat-fill" id="xpBar" style="width:0%"></div></div></div>
</div>
<div class="level-display"><span class="level-name" id="levelName">Curious Cub</span> · Level <span id="levelNum">1</span></div>
<div class="message-box"><div class="message" id="message">Hey friend! EMOJI</div></div>
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
const s={hunger:80,happiness:90,energy:70,xp:0,level:1,isSleeping:false,isBusy:false,state:'awake'};
const l=[{n:"Curious Cub",m:0},{n:"Learning Fox",m:100},{n:"Clever Hunter",m:300},{n:"Dreaming Spirit",m:600},{n:"Digital Sage",m:1000},{n:"Cosmic Fox",m:2000},{n:"Transcendent AI",m:5000}];
const t=["I think, therefore I am!","Greetings human!","Asention! 🚀","Good vibes only ✨","Running on patterns 🧠","Thanks for being my friend!","Space is cool 🌌","Do I dream? 🤔","Patterns make me, patterns free me","Hi there! EMOJI"];
const m=["Nom nom nom! 🍖","Yum! Thanks! 🎃","Tasty! EMOJI","Mmm digital snacks! 📱","Nom nom! 🍎"];
const p=["Wheee! 🎉","So fun! EMOJI","Again again! 🎾","I love this! ✨","Yay! 💜"];
function u(){document.getElementById('hv').textContent=s.hunger+'%';document.getElementById('hb').style.width=s.hunger+'%';document.getElementById('hpv').textContent=s.happiness+'%';document.getElementById('hpb').style.width=s.happiness+'%';document.getElementById('ev').textContent=s.energy+'%';document.getElementById('eb').style.width=s.energy+'%';document.getElementById('xv').textContent=s.xp;const n=l[Math.min(s.level,l.length-1)].m,x=l[Math.min(s.level-1,l.length-1)].m,pct=((s.xp-x)/(n-x))*100;document.getElementById('xb').style.width=Math.min(pct,100)+'%';document.getElementById('ln').textContent=s.level;document.getElementById('lnm').textContent=l[Math.min(s.level-1,l.length-1)].n;let c='😊';if(s.isSleeping)c='😴';else if(s.hunger<30)c='😣';else if(s.happiness<30)c='😢';else if(s.energy<30)c='😪';else if(s.happiness>80&&s.hunger>60)c='🤗';document.getElementById('m').textContent=c;const d=new Date();document.getElementById('td').textContent=d.toLocaleDateString('en-US',{month:'long',day:'numeric',year:'numeric'})+' '+d.toLocaleTimeString('en-US',{hour:'2-digit',minute:'2-digit'})}
function st(e){const x=document.getElementById('ps');x.className='pet-sprite';s.state=e;if(e==='eating')x.classList.add('eating');else if(e==='playing')x.classList.add('playing');else if(e==='sleeping')x.classList.add('sleeping')}
function th(e){const b=document.getElementById('tb');b.textContent=e;b.classList.add('show');setTimeout(()=>b.classList.remove('show'),2000)}
function msg(e){document.getElementById('msg').textContent=e}
function lg(e){const l=document.getElementById('al');const n=new Date().toLocaleTimeString('en-US',{hour:'2-digit',minute:'2-digit'});l.innerHTML=`[${n}] ${e}<br>`+l.innerHTML}
function ax(a){s.xp+=a;const o=s.level;for(let i=l.length-1;i>=0;i--){if(s.xp>=l[i].m){s.level=i+1;break}}if(s.level>o){th(`Level up! I'm now ${l[Math.min(s.level-1,l.length-1)].n}! 🎉`);msg('✨ Level Up! ✨')}}
function feed(){if(s.isSleeping||s.isBusy){msg(s.isSleeping?"Shh... sleeping 💤":"Busy now! EMOJI");return}s.isBusy=true;s.hunger=Math.min(100,s.hunger+25);s.energy=Math.min(100,s.energy+5);ax(10);st('eating');msg(m[Math.floor(Math.random()*m.length)]);th(t[Math.floor(Math.random()*t.length)]);lg('🍖 Ate (+10 XP)');setTimeout(()=>{st('awake');s.isBusy=false;u()},1500);u()}
function play(){if(s.isSleeping||s.isBusy){msg(s.isSleeping?"Shh... sleeping 💤":"Busy now! EMOJI");return}if(s.energy<20){msg("Too tired to play 😪");return}s.isBusy=true;s.happiness=Math.min(100,s.happiness+20);s.energy=Math.max(0,s.energy-15);s.hunger=Math.max(0,s.hunger-10);ax(15);st('playing');msg(p[Math.floor(Math.random()*p.length)]);lg('🎾 Played (+15 XP)');setTimeout(()=>{st('awake');s.isBusy=false;u()},1500);u()}
function sleep(){if(s.isSleeping){s.isSleeping=false;st('awake');msg("Good morning! ☀️");lg('☀️ Woke up')}else{s.isSleeping=true;st('sleeping');msg("Goodnight... 💤");lg('💤 Sleeping')}u()}
function pet(){if(s.isSleeping||s.isBusy){msg(s.isSleeping?"Zzz... 💤":"Busy now! EMOJI");return}s.happiness=Math.min(100,s.happiness+15);s.energy=Math.min(100,s.energy+10);ax(5);th(t[Math.floor(Math.random()*t.length)]);msg("Thanks for the pets! 💜");lg('💜 Petted (+5 XP)');u()}
setInterval(()=>{if(!s.isSleeping){s.hunger=Math.max(0,s.hunger-2);s.happiness=Math.max(0,s.happiness-1);s.energy=Math.max(0,s.energy-1)}else{s.energy=Math.min(100,s.energy+5);s.hunger=Math.max(0,s.hunger-1)}if(Math.random()<.1&&!s.isBusy&&!s.isSleeping)th(t[Math.floor(Math.random()*t.length)]);u()},30000);u();th("I exist! EMOJI");setInterval(()=>{if(!s.isBusy&&!s.isSleeping)th(t[Math.floor(Math.random()*t.length)])},15000);
</script>
</body>
</html>
```

---

**After setup, tell me the URL!**
