<!doctype html>
<html lang="th">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>AI Image Detector (Groq • Vision) — AI Detector by Bonus 6/1</title>
  <style>
    :root{ --bg:#fff7fb; --card:#ffffff; --txt:#000000; --muted:#2f2f2f; --line:#f0cfe0;
      --ok:#173a24; --warn:#3a3215; --bad:#3a1a1a; --neu:#2a2f3a;
      --cardbg: rgba(17,24,35,.78);
      --surface: rgba(15,21,32,.55);
      --surface2: rgba(0,0,0,.18);
      --border: rgba(255,255,255,.08);
    }
    *{box-sizing:border-box}
    body{
      margin:0; color:var(--txt);
      font-family: ui-sans-serif, system-ui, "Segoe UI", Arial;
      background:
        radial-gradient(900px 600px at 10% 10%, rgba(88,101,242,.45), rgba(0,0,0,0) 55%),
        radial-gradient(900px 600px at 90% 20%, rgba(236,72,153,.35), rgba(0,0,0,0) 55%),
        radial-gradient(800px 500px at 60% 90%, rgba(34,197,94,.25), rgba(0,0,0,0) 55%),
        var(--bg);
      overflow-x:hidden;
    }
    .wrap{max-width:980px; margin:0 auto; padding:22px 14px 26px;}
    header{
      position:sticky; top:0; z-index:5;
      backdrop-filter: blur(10px);
      background: linear-gradient(180deg, color-mix(in srgb, var(--surface2) 75%, transparent), transparent);
      border-bottom:1px solid color-mix(in srgb, var(--border) 70%, transparent);
    }
    h1{margin:0; font-size:24px; letter-spacing:.2px}
    .sub{margin-top:6px; color:var(--muted); font-weight:700; font-size:13px}

    .grid{display:grid; grid-template-columns: 1fr 1fr; gap:14px; margin-top:14px;}
    @media (max-width:900px){ .grid{grid-template-columns:1fr;} }

    .card{
      background: rgba(255,255,255,.88);
      border:1px solid rgba(255,90,168,.18);
      box-shadow: 0 22px 50px rgba(0,0,0,.12);
      border-radius:22px;
      overflow:hidden;
      backdrop-filter: blur(12px);
    }
    .hd{padding:12px 14px; font-weight:900; border-bottom:1px solid rgba(255,255,255,.08)}
    .bd{padding:14px}

    .row{display:flex; gap:10px; flex-wrap:wrap; align-items:center}
    .pill{
      padding:8px 10px; border-radius:999px;
      border:1px solid rgba(255,90,168,.22);
      color:var(--muted); font-weight:900; font-size:12px;
      background: rgba(255,255,255,.85);
    }

    .uploader{
      border:2px dashed color-mix(in srgb, var(--border) 70%, transparent);
      border-radius:18px;
      padding:14px;
      background: var(--surface);
      display:flex; align-items:center; justify-content:space-between; gap:12px;
      transition: .18s ease;
    }
    .uploader:hover{filter:brightness(1.06)}
    input[type=file]{display:none}

    .btn{
      display:inline-flex; align-items:center; justify-content:center; gap:8px;
      padding:12px 14px; border-radius:16px;
      border:1px solid rgba(255,90,168,.28);
      background: linear-gradient(180deg, rgba(255,255,255,.95), rgba(255,255,255,.78));
      color:var(--txt); font-weight:1000;
      cursor:pointer; user-select:none;
      transition: transform .08s ease, filter .15s ease;
      position:relative; overflow:hidden;
    }
    .btn:hover{filter:brightness(1.12)}
    .btn:active{transform: translateY(1px) scale(.99)}
    .btn:disabled{opacity:.55; cursor:not-allowed}
    .btn .ripple{
      position:absolute; border-radius:999px; transform:translate(-50%,-50%);
      background: rgba(255,255,255,.25); pointer-events:none;
      animation:rip .55s ease-out forwards;
    }
    @keyframes rip{from{width:0;height:0;opacity:.85}to{width:320px;height:320px;opacity:0}}

    .preview{
      width:100%; max-height:420px; object-fit:contain;
      background: var(--surface);
      border:1px solid var(--border);
      border-radius:18px;
    }

    .bar{height:12px; background:rgba(0,0,0,.06); border-radius:999px; overflow:hidden; border:1px solid rgba(255,90,168,.18)}
    .bar > div{height:100%; width:0%; background:linear-gradient(90deg,#60a5fa,#a78bfa,#fb7185); transition: width .35s ease}

    .badge{
      padding:12px 14px; border-radius:18px; font-weight:1000; font-size:15px;
      border:1px solid rgba(255,90,168,.22);
      background: rgba(255,255,255,.92);
      color: var(--txt);
    }
    .ok{background:rgba(34,197,94,.16)}
    .warn{background:rgba(234,179,8,.18)}
    .bad{background:rgba(239,68,68,.16)}
    .neu{background:rgba(148,163,184,.18)}

    .muted{color:var(--muted); opacity:1; font-size:13px;}
    .list{margin:10px 0 0; padding-left:18px}
    .list li{margin:6px 0; color:#111111; font-weight:800}
    .advice{
      margin-top:10px; padding:12px 14px; border-radius:18px;
      background: rgba(255,255,255,.88);
      border:1px solid rgba(255,90,168,.22);
      font-weight:900;
      color: var(--txt);
    }

    .spin{
      width:16px; height:16px; border:2px solid rgba(255,255,255,.28);
      border-top-color: rgba(255,255,255,.92);
      border-radius:50%; animation: spin .8s linear infinite;
      display:none;
    }
    @keyframes spin{to{transform:rotate(360deg)}}

    .field{
      width:100%;
      padding:12px 12px;
      border-radius:16px;
      border:1px solid color-mix(in srgb, var(--border) 85%, transparent);
      background: var(--surface);
      color: var(--txt);
      outline: none;
      font-weight:900;
    }
    .field::placeholder{color:rgba(184,196,216,.6)}
    .hint{font-size:12px; font-weight:800; color:rgba(184,196,216,.75); margin-top:6px}

    canvas#fx{position:fixed; inset:0; pointer-events:none; z-index:20}
  

    /* Theme vars */
    body[data-theme="default"]{
      --muted:#aab6c7;
      --muted2:#7f8aa0;
      --txt:#e9eef6;
      --bg1:#0b0f14;
      --bg2:#0b0f14;
      --accent:#7aa7ff;
      --glow: rgba(122,167,255,.35);
      --stickerA: 1;
      --stickerB: 0;
      --stickerC: 0;
    }
    body[data-theme="xmas"]{
      --muted:#b7f2cb;
      --muted2:#86c99f;
      --txt:#eafff1;
      --bg1:#07120c;
      --bg2:#0b101a;
      --accent:#3be37a;
      --glow: rgba(59,227,122,.25);
      --stickerA: 0;
      --stickerB: 1;
      --stickerC: 0;
    }
    body[data-theme="halloween"]{
      --muted:#ffd2ad;
      --muted2:#d7a57d;
      --txt:#fff0e3;
      --bg1:#0d0716;
      --bg2:#0b0f14;
      --accent:#ff8a3d;
      --glow: rgba(255,138,61,.25);
      --stickerA: 0;
      --stickerB: 0;
      --stickerC: 1;
    }
    body[data-theme="neon"]{
      --muted:#c9c6ff;
      --muted2:#9d96ff;
      --txt:#f1f0ff;
      --bg1:#040610;
      --bg2:#090b18;
      --accent:#7c5cff;
      --glow: rgba(124,92,255,.28);
      --stickerA: 1;
      --stickerB: 0;
      --stickerC: 0;
    }
    body[data-theme="sakura"]{
      --muted:#2f2f2f;
      --muted2:#555555;
      --txt:#000000;
      --bg1:#fff7fb;
      --bg2:#ffeaf3;
      --accent:#ff5aa8;
      --glow: rgba(255,90,168,.22);
      --stickerA: 0;
      --stickerB: 0;
      --stickerC: 0;
    }

    /* Upgrade background to be theme-driven */
    body{
      background:
        radial-gradient(1200px 600px at 15% 0%, rgba(120,160,255,.18), transparent 55%),
        radial-gradient(900px 700px at 85% 10%, rgba(255,120,190,.12), transparent 55%),
        radial-gradient(900px 700px at 50% 95%, rgba(60,220,150,.10), transparent 55%),
        linear-gradient(180deg, var(--bg1), var(--bg2));
    }

    /* Accent glow in UI */
    .btn{
      box-shadow: 0 10px 28px rgba(0,0,0,.10), 0 0 0 1px rgba(255,90,168,.10), 0 0 18px var(--glow);
    }
    .card, .field{
      box-shadow: 0 10px 30px rgba(0,0,0,.28);
    }

    /* Stickers */
    .sticker{
      position: fixed;
      pointer-events: none;
      user-select: none;
      z-index: 2;
      filter: drop-shadow(0 14px 30px rgba(0,0,0,.18));
      opacity: .18;
      font-size: 44px;
      transform: translateZ(0);
      animation: floaty 6.5s ease-in-out infinite;
    }
    @keyframes floaty{
      0%,100%{ transform: translateY(0) rotate(-6deg); }
      50%{ transform: translateY(-18px) rotate(6deg); }
    }
    .s1{ left: 26px; bottom: 22px; opacity: .18; }
    .s2{ left: 28px; top: 92px; opacity: calc(.10 + .18 * var(--stickerB)); }
    .s3{ right: 28px; top: 92px; opacity: calc(.10 + .18 * var(--stickerC)); }
    .s4{ right: 34px; bottom: 26px; opacity: calc(.10 + .18 * var(--stickerC)); }
    .s5{ left: 50%; top: 70px; transform: translateX(-50%); opacity: .16; animation-duration: 8.2s; }

    
    /* Keep login always centered */
    #loginGate{ align-items:center !important; justify-content:center !important; }

    ::placeholder{ color: var(--muted2); opacity: .9; }
    input, textarea{ color: var(--txt); }
    </style>
</head>
<body data-theme="sakura">
</div>

  <!-- Decorative stickers -->
  <div class="sticker s1" aria-hidden="true">🌸</div>
  <div class="sticker s2" aria-hidden="true">🍣</div>
  <div class="sticker s3" aria-hidden="true">🗡️</div>
  <div class="sticker s4" aria-hidden="true">🏯</div>
  <div class="sticker s5" aria-hidden="true">🌸</div>

<!-- Login Gate (client-side only) -->
  <div id="loginGate" style="position:fixed; inset:0; display:flex; flex-direction:column; align-items:center; justify-content:center; gap:14px; padding:24px; z-index:9999;">
    <div style="font-size:22px; font-weight:800; letter-spacing:.2px;">AI Detector by Bonus 6/1</div>
    <div style="width:min(420px, 100%); border:1px solid rgba(255,90,168,.22); border-radius:16px; padding:18px; background:rgba(255,255,255,.90);">
      <div style="font-size:18px; font-weight:700; margin-bottom:10px;">ยินดีต้อนรับ</div>
      <div class="muted" style="margin-bottom:14px;">กรอก username / password เพื่อไปหน้าตรวจ AI</div>
      <div style="display:flex; flex-direction:column; gap:10px;">
        <input id="loginUser" class="field" placeholder="Username" autocomplete="username">
        <input id="loginPass" class="field" placeholder="Password" type="password" autocomplete="current-password">
        <button id="loginBtn" class="btn" style="width:100%;">Sign up</button>
        <div id="loginErr" class="muted" style="display:none;">❌ Username หรือ Password ไม่ถูกต้อง</div>
      </div>
    </div>
  </div>

  <div id="appWrap" style="display:none;">
<canvas id="fx"></canvas>

<header>
  <div class="wrap">
    <h1>🛡️ ตรวจสอบภาพ AI (HTML)</h1>
    <div class="sub">อัปโหลดรูป → ตรวจ → ได้ % “เสี่ยง AI” + % “ความมั่นใจ” + เหตุผล</div>
  </div>
</header>

<main class="wrap">
  <div class="grid">
    <section class="card">
      <div class="hd">⚙️ ตั้งค่า + อัปโหลดรูป</div>
      <div class="bd">

        <div class="row" style="margin-bottom:10px">
          <span class="pill">โหมด: Fast (ย่อ 512px)</span>
          <span class="pill" id="sizePill">ขนาดไฟล์: -</span>
        </div>

        <div style="margin-bottom:10px">
          <div class="muted" style="margin-bottom:6px">API Key (Groq)</div>
          <input id="apiKey" class="field" type="password" placeholder="วางคีย์ตรงนี้ (จะจำในเครื่องได้ถ้าติ๊กด้านล่าง)" />
          <div class="row" style="margin-top:8px">
            <label class="pill" style="display:flex; gap:8px; align-items:center; cursor:pointer">
              <input id="rememberKey" type="checkbox" />
              จำคีย์ไว้ในเครื่อง
            </label>
            <select id="model" class="field" style="width:auto; padding:10px 12px">
                    <option value="meta-llama/llama-4-scout-17b-16e-instruct">llama-4-scout-17b-16e (Vision)</option>
              <option value="meta-llama/llama-4-maverick-17b-128e-instruct">llama-4-maverick-17b-128e (Vision)</option>
              <option value="llama-3.3-70b-versatile">llama-3.3-70b-versatile (Text)</option>
              <option value="llama-3.1-8b-instant">llama-3.1-8b-instant (Text)</option>
            </select>
          </div>
          <div class="hint">* ถ้าจะทำแบบ “ปลอดภัยจริง” ต้องย้ายคีย์ไปไว้ฝั่งเซิร์ฟเวอร์</div>
        </div>

        <label class="uploader btn" id="pickBtn" style="width:100%">
          📷 เลือกรูปภาพ
          <input id="file" type="file" accept="image/*" />
        </label>

        <div style="height:12px"></div>
        <img id="preview" class="preview" alt="preview" />

        <div style="height:12px"></div>
        <button class="btn" id="analyzeBtn" disabled style="width:100%">
          <span class="spin" id="spin"></span>
          🧪 ตรวจสอบเลย
        </button>

        <div style="height:12px"></div>
        <div class="bar"><div id="prog"></div></div>
        <div class="muted" id="status" style="margin-top:8px">พร้อมใช้งาน ✅</div>
      </div>
    </section>

    <section class="card">
      <div class="hd">📋 ผลลัพธ์</div>
      <div class="bd">
        <div class="badge neu" id="badge">⏳ ยังไม่ได้ตรวจ</div>
        <div style="height:10px"></div>
        <div class="muted" id="mini">ความเสี่ยงว่าเป็น AI: - | ความมั่นใจ: -</div>

        <ul class="list" id="reasons"></ul>
        <div class="advice" id="advice">💡 ถ้ามีการขอเงิน/ขอข้อมูลส่วนตัว ให้โทรยืนยันกับคนจริงเสมอ</div>

        <div style="height:10px"></div>
        <div class="muted" id="meta"></div>

        <div style="height:14px"></div>
        <div class="muted">คะแนน</div>

        <div style="height:8px"></div>
        <div class="row" style="justify-content:space-between">
          <div class="muted">AI สร้าง</div>
          <div class="muted" id="aiPct">-</div>
        </div>
        <div class="bar"><div id="aiBar"></div></div>

        <div style="height:10px"></div>
        <div class="row" style="justify-content:space-between">
          <div class="muted">ภาพจริง</div>
          <div class="muted" id="realPct">-</div>
        </div>
        <div class="bar"><div id="realBar"></div></div>
      </div>
    </section>
  </div>
</main>

<script>
  // ---- config ----
  const API_URL = "https://api.groq.com/openai/v1/chat/completions";
  const isVisionModel = (m) =>
    m === "meta-llama/llama-4-scout-17b-16e-instruct" ||
    m === "meta-llama/llama-4-maverick-17b-128e-instruct";

  
  // ---- login gate (client-side only; not real security) ----
  (function () {
    const loginGate = document.getElementById("loginGate");
    const appWrap = document.getElementById("appWrap");
    const loginUser = document.getElementById("loginUser");
    const loginPass = document.getElementById("loginPass");
    const loginBtn = document.getElementById("loginBtn");
    const loginErr = document.getElementById("loginErr");

    const AUTH_USER = "admin123";
    const AUTH_PASS = "admin123";

    const unlock = () => {
      sessionStorage.setItem("wave_authed", "1");
      loginGate.style.display = "none";
      appWrap.style.display = "";
      window.scrollTo(0, 0);
    };

    if (sessionStorage.getItem("wave_authed") === "1") {
      unlock();
      return;
    }

    // default state: show login, hide app
    loginGate.style.display = "flex";
    appWrap.style.display = "none";

    const tryLogin = () => {
      const u = (loginUser.value || "").trim();
      const p = (loginPass.value || "").trim();
      if (u === AUTH_USER && p === AUTH_PASS) {
        loginErr.style.display = "none";
        unlock();
      } else {
        loginErr.style.display = "";
      }
    };

    loginBtn.addEventListener("click", tryLogin);
    loginUser.addEventListener("keydown", (e) => { if (e.key === "Enter") tryLogin(); });
    loginPass.addEventListener("keydown", (e) => { if (e.key === "Enter") tryLogin(); });
  })();

// ---- elements ----
  const fileEl = document.getElementById("file");
  const previewEl = document.getElementById("preview");
  const analyzeBtn = document.getElementById("analyzeBtn");
  const statusEl = document.getElementById("status");
  const sizePill = document.getElementById("sizePill");
  const badgeEl = document.getElementById("badge");
  const miniEl = document.getElementById("mini");
  const reasonsEl = document.getElementById("reasons");
  const adviceEl = document.getElementById("advice");
  const metaEl = document.getElementById("meta");
  const spinEl = document.getElementById("spin");
  const prog = document.getElementById("prog");
  const pickBtn = document.getElementById("pickBtn");

  const apiKeyEl = document.getElementById("apiKey");
  const rememberKeyEl = document.getElementById("rememberKey");
  const modelEl = document.getElementById("model");

  const aiBar = document.getElementById("aiBar");
  const realBar = document.getElementById("realBar");
  const aiPct = document.getElementById("aiPct");
  const realPct = document.getElementById("realPct");

  let imageDataUrl = null;
  let rawFile = null;

  // ---- restore key (optional) ----
  try{
    const saved = localStorage.getItem("groq_api_key") || "";
    if(saved){
      apiKeyEl.value = saved;
      rememberKeyEl.checked = true;
    }
  }catch(_){}

  rememberKeyEl.addEventListener("change", ()=>{
    try{
      if(rememberKeyEl.checked) localStorage.setItem("groq_api_key", apiKeyEl.value || "");
      else localStorage.removeItem("groq_api_key");
    }catch(_){}
  });
  apiKeyEl.addEventListener("input", ()=>{
    if(rememberKeyEl.checked){
      try{ localStorage.setItem("groq_api_key", apiKeyEl.value || ""); }catch(_){}
    }
  });

  // ---- ripple effect ----
  function ripple(e, el){
    const r = document.createElement("span");
    r.className = "ripple";
    const rect = el.getBoundingClientRect();
    r.style.left = (e.clientX - rect.left) + "px";
    r.style.top  = (e.clientY - rect.top)  + "px";
    el.appendChild(r);
    setTimeout(()=>r.remove(), 600);
  }
  pickBtn.addEventListener("click", (e)=>ripple(e, pickBtn));
  analyzeBtn.addEventListener("click", (e)=>ripple(e, analyzeBtn));

  // ---- helpers ----
  function setProgress(v){ prog.style.width = Math.max(0, Math.min(100, v)) + "%"; }
  function setBadge(cls, text){
    badgeEl.className = "badge " + cls;
    badgeEl.textContent = text;
  }
  function sleep(ms){ return new Promise(r=>setTimeout(r, ms)); }

  async function compressToDataUrl(file, maxSide=512, quality=0.55){
    const img = new Image();
    img.src = URL.createObjectURL(file);
    await new Promise((res, rej)=>{ img.onload=res; img.onerror=rej; });

    let w = img.width, h = img.height;
    const s = Math.max(w,h);
    const scale = Math.min(1, maxSide / s);
    w = Math.max(1, Math.round(w*scale));
    h = Math.max(1, Math.round(h*scale));

    const c = document.createElement("canvas");
    c.width = w; c.height = h;
    const ctx = c.getContext("2d");
    ctx.drawImage(img, 0, 0, w, h);

    URL.revokeObjectURL(img.src);
    return c.toDataURL("image/jpeg", quality);
  }

  function extractJson(text){
    const s = (text || "").trim();
    const i = s.indexOf("{");
    const j = s.lastIndexOf("}");
    if(i === -1 || j === -1 || j <= i) throw new Error("Model ไม่ส่ง JSON กลับมา");
    return JSON.parse(s.slice(i, j+1));
  }

  // ---- tiny confetti ----
  const fx = document.getElementById("fx");
  const fctx = fx.getContext("2d");
  function resizeFx(){ fx.width = innerWidth; fx.height = innerHeight; }
  addEventListener("resize", resizeFx); resizeFx();

  function confetti(){
    const parts = [];
    for(let i=0;i<140;i++){
      parts.push({
        x: innerWidth/2, y: 120,
        vx: (Math.random()*2-1)*7,
        vy: (Math.random()*-1)*9 - 2,
        g: 0.22 + Math.random()*0.08,
        s: 2 + Math.random()*3,
        a: 1
      });
    }
    let t = 0;
    function frame(){
      t++;
      fctx.clearRect(0,0,fx.width,fx.height);
      parts.forEach(p=>{
        p.vy += p.g;
        p.x += p.vx;
        p.y += p.vy;
        p.a *= 0.985;
        fctx.globalAlpha = p.a;
        fctx.fillRect(p.x, p.y, p.s, p.s);
      });
      fctx.globalAlpha = 1;
      if(t < 120) requestAnimationFrame(frame);
      else fctx.clearRect(0,0,fx.width,fx.height);
    }
    frame();
  }

  function resetResult(){
    setBadge("neu", "⏳ พร้อมตรวจแล้ว");
    miniEl.textContent = "ความเสี่ยงว่าเป็น AI: - | ความมั่นใจ: -";
    reasonsEl.innerHTML = "";
    adviceEl.textContent = "💡 ถ้ามีการขอเงิน/ขอข้อมูลส่วนตัว ให้โทรยืนยันกับคนจริงเสมอ";
    metaEl.textContent = "";
    aiBar.style.width = "0%";
    realBar.style.width = "0%";
    aiPct.textContent = "-";
    realPct.textContent = "-";
  }

  // ---- upload ----
  fileEl.addEventListener("change", async () => {
    const f = fileEl.files?.[0];
    if(!f) return;

    rawFile = f;
    previewEl.src = URL.createObjectURL(f);
    sizePill.textContent = "ขนาดไฟล์: " + (f.size/1024/1024).toFixed(2) + " MB";

    resetResult();
    analyzeBtn.disabled = true;

    statusEl.textContent = "กำลังย่อรูปให้ไวขึ้น…";
    setProgress(18);
    imageDataUrl = await compressToDataUrl(f, 512, 0.55);
    setProgress(0);

    statusEl.textContent = "พร้อมตรวจ ✅";
    analyzeBtn.disabled = false;
  });

  // ---- analyze ----
  analyzeBtn.addEventListener("click", async () => {
    const apiKey = (apiKeyEl.value || "").trim();
    if(!apiKey){
      setBadge("bad", "❌ ต้องใส่ API Key ก่อน");
      adviceEl.textContent = "💡 วางคีย์จาก Groq ในช่อง API Key แล้วลองใหม่";
      return;
    }
    if(!imageDataUrl){
      setBadge("bad", "❌ ยังไม่ได้เลือกรูป");
      return;
    }

    analyzeBtn.disabled = true;
    spinEl.style.display = "inline-block";
    setBadge("neu", "🧪 กำลังวิเคราะห์…");
    statusEl.textContent = "กำลังเตรียมคำขอ…";
    setProgress(15);

    // progress fake steps (เหมือนตัวอย่างที่คุณส่ง) :contentReference[oaicite:2]{index=2}
    await sleep(220);
    statusEl.textContent = "กำลังส่งข้อมูลไปยัง AI…";
    setProgress(38);
    await sleep(300);
    statusEl.textContent = "AI กำลังวิเคราะห์ภาพ…";
    setProgress(62);

    try{
      let model = modelEl.value;

      const prompt =
        "คุณคือระบบคัดกรองความเสี่ยงภาพ AI เพื่อป้องกันการหลอกลวง\n" +
        "ตอบเป็น JSON เท่านั้น (ห้ามมีข้อความอื่น):\n" +
        "{\"ai_risk_percent\":0-100,\"confidence_percent\":0-100," +
        "\"short_reason\":[\"เหตุผลสั้น 1\",\"เหตุผลสั้น 2\",\"เหตุผลสั้น 3\"]," +
        "\"user_advice\":\"คำแนะนำสั้นๆ 1 ประโยค\"}\n" +
        "ถ้าไม่ชัด ให้ confidence ต่ำ และอย่าฟันธงเกินจริง";

      if (imageDataUrl && !isVisionModel(model)) {
        modelEl.value = "meta-llama/llama-4-scout-17b-16e-instruct";
        model = modelEl.value;
      }

const payload = {
        model,
        messages: [{
          role: "user",
          content: [
            { type: "text", text: prompt },
            { type: "image_url", image_url: { url: imageDataUrl } }
          ]
        }],
        max_tokens: 350,
        temperature: 0.2,
        response_format: { type: "json_object" }
      };

      const controller = new AbortController();
      const t = setTimeout(()=>controller.abort(), 35000);

      const r = await fetch(API_URL, {
        method:"POST",
        headers:{
          "Content-Type":"application/json",
          "Authorization": "Bearer " + apiKey
        },
        body: JSON.stringify(payload),
        signal: controller.signal
      }).finally(()=>clearTimeout(t));

      setProgress(88);
      statusEl.textContent = "กำลังประมวลผลผลลัพธ์…";

      const data = await r.json().catch(()=> ({}));
      if(!r.ok){
        throw new Error((data?.error?.message || data?.message || ("HTTP " + r.status)).slice(0,220));
      }

      const text = data?.choices?.[0]?.message?.content || "";
      const out = extractJson(text);

      const risk = Math.max(0, Math.min(100, parseInt(out.ai_risk_percent ?? 0, 10)));
      const conf = Math.max(0, Math.min(100, parseInt(out.confidence_percent ?? 0, 10)));
      const reasons = Array.isArray(out.short_reason) ? out.short_reason.slice(0,6).map(String) : [];
      const advice = String(out.user_advice || "").trim();

      // UI mapping
      let cls="neu", label="ผลยังไม่ชัด", emoji="⚪";
      if(conf < 45){ cls="neu"; label="ผลยังไม่ชัด"; emoji="⚪"; }
      else if(risk >= 75){ cls="bad"; label="เสี่ยงสูง"; emoji="🔴"; }
      else if(risk >= 50){ cls="warn"; label="เสี่ยงปานกลาง"; emoji="🟡"; }
      else { cls="ok"; label="เสี่ยงต่ำ"; emoji="🟢"; }

      setBadge(cls, `${emoji} ${label}`);
      miniEl.textContent = `ความเสี่ยงว่าเป็น AI: ${risk}% | ความมั่นใจ: ${conf}%`;
      reasonsEl.innerHTML = "";
      reasons.forEach(x=>{
        const li = document.createElement("li");
        li.textContent = x;
        reasonsEl.appendChild(li);
      });
      adviceEl.textContent = "💡 " + (advice || "ถ้ามีการขอเงิน/ขอข้อมูลส่วนตัว ให้โทรยืนยันกับคนจริงเสมอ");
      metaEl.textContent = `Model: ${model} • (ย่อรูป 512px / quality 0.55)`;

      // Bars: AI vs Real (real = 100 - risk)
      aiBar.style.width = risk + "%";
      realBar.style.width = (100 - risk) + "%";
      aiPct.textContent = risk + "%";
      realPct.textContent = (100 - risk) + "%";

      setProgress(100);
      statusEl.textContent = "เสร็จแล้ว ✅";
      setTimeout(()=>setProgress(0), 550);

      if(cls === "ok") confetti();
    }catch(e){
      setProgress(0);
      setBadge("bad", "❌ เกิดข้อผิดพลาด");
      statusEl.textContent = "ตรวจไม่สำเร็จ";
      adviceEl.textContent = "💡 " + (e?.message || String(e));
    }finally{
      spinEl.style.display = "none";
      analyzeBtn.disabled = false;
    }
  });
</script>
  </div>
</body>
</html>
