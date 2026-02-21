<script>
/* ======= 生きてる感フル強化スクリプト ======= */
/* これを既存の <script>…</script> と丸ごと置き換えてください */

const chat = document.getElementById('chat');
const text = document.getElementById('text');
const sendBtn = document.getElementById('sendBtn');
const togglePro = document.getElementById('togglePro');
const notifBtn = document.getElementById('notifBtn');
const useRealBtn = document.getElementById('useRealBtn');
const calmBtn = document.getElementById('calm');
const bpmEl = document.getElementById('bpm');
const affEl = document.getElementById('aff');
const toast = document.getElementById('toast');
const bgm = document.getElementById('bgm');

let bpm = 72;
let affinity = 12;
let proactive = true;
let lastInteraction = Date.now();
let consecutivePro = 0;
let memory = JSON.parse(localStorage.getItem('aiMemory') || '{}');
let convoHistory = JSON.parse(localStorage.getItem('aiHistory') || '[]');
let useRealAI = false;
let mood = typeof memory.mood === 'number' ? memory.mood : 50; // 0..100
if (!memory.createdAt) { memory.createdAt = Date.now(); localStorage.setItem('aiMemory', JSON.stringify(memory)); }

/* ---------- helpers ---------- */
function updateUI(){ bpmEl.textContent = Math.round(bpm); affEl.textContent = Math.round(affinity); }
function showToast(t){ toast.style.display='block'; toast.textContent = t; setTimeout(()=> toast.style.display='none', 3500); }

function pushMessage(textContent, cls){
  const d = document.createElement('div');
  d.className = 'msg ' + cls;
  chat.appendChild(d);
  // typewriter effect
  let i = 0;
  function type(){
    if(i < textContent.length){
      d.textContent += textContent[i++];
      chat.scrollTop = chat.scrollHeight;
      // typing speed slightly varies with mood (more calm = slower)
      const speed = 18 + Math.max(0, (80 - mood) / 2);
      setTimeout(type, speed);
    }
  }
  type();
}

/* typing indicator and then message (simulate human thinking) */
function showTypingThen(message, cls, baseDelay = 800){
  const t = document.createElement('div');
  t.className = 'msg ai typing';
  t.innerHTML = '打ち込み中<span class="dots"><span class="dot"></span><span class="dot"></span><span class="dot"></span></span>';
  chat.appendChild(t);
  chat.scrollTop = chat.scrollHeight;
  heartbeat(0.12); // small heartbeat while typing
  // variable delay: longer when message is longer, a bit affected by mood
  const delay = baseDelay + Math.min(1600, message.length * 8) + (100 - mood) * 4 * Math.random();
  setTimeout(()=> {
    t.remove();
    pushMessage(message, cls);
    heartbeat(0.22);
  }, delay);
}

/* ---------- Audio heartbeat ---------- */
let ctx = null;
function ensureCtx(){ if(!ctx) ctx = new (window.AudioContext || window.webkitAudioContext)(); }
function heartbeat(str = 0.28){
  ensureCtx();
  const now = ctx.currentTime;
  for(let i=0;i<2;i++){
    const o = ctx.createOscillator();
    const g = ctx.createGain();
    o.type = 'sine';
    o.frequency.value = 120 - i*8;
    g.gain.value = str * (i===0 ? 0.72 : 0.42);
    o.connect(g); g.connect(ctx.destination);
    o.start(now + i*0.06);
    g.gain.exponentialRampToValueAtTime(0.001, now + 0.26 + i*0.02);
    o.stop(now + 0.28 + i*0.02);
  }
}

/* ---------- memory ---------- */
function remember(text){
  const s = (text || '').toLowerCase();
  if(s.includes('名前は') || s.includes('わたしの名前は') || s.includes('私の名前は')){
    const m = text.match(/名前は\s*([^\s、。]+)/i);
    if(m) { memory.username = m[1].trim(); showToast(`名前を覚えたよ：${memory.username}`); }
  }
  if(s.includes('好き') || s.includes('すき')) memory.likes = text;
  if(s.includes('疲') || s.includes('つか')) memory.tired = true;
  if(s.includes('学校') || s.includes('がっこう')) memory.school = true;
  // mood words
  if(s.includes('嬉') || s.includes('うれ')) mood = Math.min(100, mood + 6);
  if(s.includes('悲') || s.includes('かなしい') || s.includes('つら')) mood = Math.max(0, mood - 8);
  // save
  memory.lastSeen = Date.now();
  memory.mood = mood;
  localStorage.setItem('aiMemory', JSON.stringify(memory));
}

/* ---------- affinity ---------- */
function incAffinity(n){
  affinity = Math.min(999, affinity + n);
  updateUI();
  if(affinity >= 30 && !memory.unlockedSR){ memory.unlockedSR = true; showToast('特別な反応を覚えた…'); localStorage.setItem('aiMemory', JSON.stringify(memory)); }
  if(affinity >= 60 && !memory.unlockedSSR){ memory.unlockedSSR = true; showToast('さらに特別な反応が解放された'); localStorage.setItem('aiMemory', JSON.stringify(memory)); }
}

/* ---------- reply crafting (human-like, safe) ---------- */
const baseProactive = [
  "ねえ、少し話していい？声が聞きたくなった。",
  "外の風が落ち着いてきたね。今日のことを話してみる？",
  "夜に見る景色は、言葉を優しくするみたいだよ。"
];
const baseReactive = [
  "その言葉、静かに響くね。",
  "深呼吸して。君の呼吸が落ち着くまで待つよ。",
  "ゆっくりでいいよ。話してくれてありがとう。"
];
const srPool = ["前に話してくれたこと、覚えてる。今日はどう？"];
const ssrPool = ["静かな時間が、君の宝物になりますように。"];

// small function to get random element
function pick(arr){ return arr[Math.floor(Math.random()*arr.length)]; }

function craftReply(input){
  input = (input||"").trim();
  remember(input);

  // If user g
