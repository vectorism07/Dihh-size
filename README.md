<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Baddie Finder </title>
  <style>
    :root {
      --bg: #0b0f14;
      --card: #131a22;
      --text: #e6edf3;
      --muted: #9aa6b2;
      --accent: #67e8f9;
      --accent-2: #a78bfa;
      --success: #10b981;
      --warn: #f59e0b;
      --danger: #ef4444;
      --shadow: 0 10px 30px rgba(0,0,0,0.35);
      --radius: 20px;
    }
    * { box-sizing: border-box; }
    html, body { height: 100%; }
    body {
      margin: 0;
      font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, "Helvetica Neue", Arial, "Apple Color Emoji", "Segoe UI Emoji";
      color: var(--text);
      background:
        radial-gradient(1000px 400px at -10% -10%, #1a1037 0%, transparent 60%),
        radial-gradient(900px 500px at 110% -10%, #0b3550 0%, transparent 60%),
        radial-gradient(900px 500px at 50% 115%, #0c4a3a 0%, transparent 55%),
        var(--bg);
      display: grid;
      place-items: center;
      padding: 24px;
    }

    .app {
      width: 100%;
      max-width: 560px;
    }

    .card {
      background: linear-gradient(180deg, rgba(255,255,255,0.03), transparent 30%) , var(--card);
      border: 1px solid rgba(255,255,255,0.06);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      padding: 24px;
      backdrop-filter: blur(6px);
    }

    .title {
      display: flex;
      align-items: center;
      gap: 12px;
      font-weight: 800;
      font-size: 1.5rem;
      letter-spacing: 0.2px;
      margin: 0 0 6px;
    }
    .title .badge {
      font-size: 0.7rem;
      padding: 4px 8px;
      border-radius: 999px;
      background: linear-gradient(90deg, var(--accent), var(--accent-2));
      color: #0b0f14;
      font-weight: 800;
      letter-spacing: 0.5px;
      text-transform: uppercase;
    }

    .subtitle {
      margin: 0 0 18px;
      color: var(--muted);
      font-size: 0.98rem;
      line-height: 1.45;
    }

    .row {
      display: grid;
      grid-template-columns: 1fr auto;
      gap: 12px;
      margin-top: 6px;
    }

    .input {
      width: 100%;
      background: #0f141b;
      border: 1px solid rgba(255,255,255,0.08);
      color: var(--text);
      padding: 14px 16px;
      border-radius: 14px;
      font-size: 1rem;
      outline: none;
      transition: border .2s, box-shadow .2s;
    }
    .input:focus {
      border-color: var(--accent);
      box-shadow: 0 0 0 6px rgba(103,232,249,0.12);
    }

    .btn {
      padding: 14px 18px;
      border-radius: 14px;
      border: 0;
      font-weight: 800;
      letter-spacing: 0.3px;
      cursor: pointer;
      background: linear-gradient(90deg, var(--accent), var(--accent-2));
      color: #0b0f14;
      transition: transform .05s ease-in-out, filter .2s;
    }
    .btn:active { transform: translateY(1px) }
    .btn:hover { filter: brightness(1.05) }

    .status {
      margin-top: 16px;
      background: #0f141b;
      border: 1px dashed rgba(255,255,255,0.12);
      border-radius: 14px;
      padding: 14px 16px;
      display: none;
      align-items: center;
      gap: 10px;
      font-size: 0.95rem;
      color: var(--muted);
    }

    .dot {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: currentColor;
      opacity: .6;
      animation: bounce 1.4s infinite ease-in-out both;
    }
    .dot:nth-child(1){ animation-delay: -0.32s }
    .dot:nth-child(2){ animation-delay: -0.16s }
    @keyframes bounce {
      0%, 80%, 100% { transform: scale(0) }
      40% { transform: scale(1) }
    }

    .result {
      display: none;
      margin-top: 18px;
      border-radius: 16px;
      padding: 18px;
      background: linear-gradient(180deg, rgba(16,185,129,0.08), rgba(103,232,249,0.05));
      border: 1px solid rgba(255,255,255,0.08);
    }

    .score {
      display: flex;
      align-items: baseline;
      gap: 10px;
      font-weight: 900;
      font-size: 2.4rem;
      line-height: 1;
      margin: 0;
    }
    .score small {
      font-weight: 700;
      color: var(--muted);
      font-size: 0.9rem;
      letter-spacing: 0.3px;
    }

    .remark {
      margin-top: 10px;
      font-size: 1.02rem;
    }

    .bar {
      margin-top: 14px;
      height: 12px;
      background: #0b0f14;
      border: 1px solid rgba(255,255,255,0.08);
      border-radius: 999px;
      overflow: hidden;
    }
    .bar > span {
      display: block;
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, var(--accent), var(--accent-2));
      transition: width .6s ease;
    }

    .footer {
      text-align: center;
      margin-top: 16px;
      color: var(--muted);
      font-size: 0.9rem;
    }
    .sig {
      margin-top: 6px;
      font-weight: 800;
      font-size: 0.95rem;
      color: transparent;
      background: linear-gradient(90deg, var(--accent), var(--accent-2));
      -webkit-background-clip: text;
              background-clip: text;
    }

    .err {
      border-color: var(--danger) !important;
      box-shadow: 0 0 0 6px rgba(239,68,68,0.12) !important;
    }

    @media (max-width: 420px) {
      .title { font-size: 1.3rem; }
      .score { font-size: 2rem; }
    }
  </style>
</head>
<body>
  <div class="app">
    <div class="card">
      <h1 class="title">
        Baddie Finder
        <span class="badge">sybau</span>
      </h1>
      <p class="subtitle">
        Enter your <b>Veiny ahh dihh length</b> (in inches). We’ll rate your chances 🍌
      </p>

      <div class="row">
        <input id="length" class="input" type="number" inputmode="decimal" step="0.1" min="0" placeholder="e.g., 5.5" />
        <button id="btn" class="btn">Find</button>
      </div>

      <div id="status" class="status">
        <div class="dot"></div><div class="dot"></div><div class="dot"></div>
        <span id="statusText">Calculating the baddie you'll get with that tiny dihh…</span>
      </div>

      <div id="result" class="result">
        <p class="score">
          <span id="scoreVal">10/10</span>
          <small id="tierText">baddie potential</small>
        </p>
        <div class="bar"><span id="barFill"></span></div>
        <p id="remark" class="remark"></p>
      </div>

      <div class="footer">
        Made by <span class="sig">“Burchatta Bhadwa”</span>
      </div>
    </div>
  </div>

  <script>
    const input = document.getElementById('length');
    const btn = document.getElementById('btn');
    const statusBox = document.getElementById('status');
    const resultBox = document.getElementById('result');
    const scoreVal = document.getElementById('scoreVal');
    const tierText = document.getElementById('tierText');
    const remark = document.getElementById('remark');
    const barFill = document.getElementById('barFill');

    function clamp(n, min, max){ return Math.max(min, Math.min(n, max)); }

    function computeScore(x){
      const idealMin = 5, idealMax = 6;
      if (x >= idealMin && x <= idealMax) return 10;
      const distance = x < idealMin ? (idealMin - x) : (x - idealMax);
      const raw = 10 - distance * 2.5; // each inch away drops ~2.5 points
      return clamp(Math.round(raw), 1, 10);
    }

    function makeRemark(x, score){
      if (!isFinite(x) || x <= 0) return "Enter a real number greater than 0 🙂";
      if (x < 2) return "your dihh doesn't exist lmao 🍌";
      if (x < 5) return "kinda small — confidence > size 😎";
      if (x <= 6) return "perfectly balanced. chef’s kiss ✨";
      if (x <= 8) return "a bit much — handle with care 🧤";
      if (x <= 12) return "bro that’s a plantain fr 💀";
      return "too big to be from this planet 👽";
    }

    function tierFromScore(s){
      if (s >= 9) return "peak baddie energy";
      if (s >= 7) return "strong baddie vibes";
      if (s >= 5) return "decent odds";
      if (s >= 3) return "iffy chances";
      return "hey, personality clutch time";
    }

    function randomDelay(minMs=2000, maxMs=3000){
      return Math.floor(Math.random() * (maxMs - minMs + 1)) + minMs;
    }

    function startLoading(){
      statusBox.style.display = 'flex';
      resultBox.style.display = 'none';
    }
    function stopLoading(){
      statusBox.style.display = 'none';
    }

    function showResult(score, x){
      scoreVal.textContent = `${score}/10`;
      tierText.textContent = tierFromScore(score);
      remark.textContent = makeRemark(x, score);
      barFill.style.width = `${score * 10}%`;
      resultBox.style.display = 'block';
    }

    function validate(){
      const val = parseFloat(input.value);
      if (!isFinite(val) || val <= 0) {
        input.classList.add('err');
        input.focus();
        return null;
      }
      input.classList.remove('err');
      return val;
    }

    btn.addEventListener('click', () => {
      const val = validate();
      if (val === null) return;

      startLoading();
      const wait = randomDelay(2000, 3000);
      setTimeout(() => {
        stopLoading();
        const score = computeScore(val);
        showResult(score, val);
      }, wait);
    });

    // Allow Enter key to submit
    input.addEventListener('keydown', (e) => {
      if (e.key === 'Enter') btn.click();
    });
  </script>
</body>
</html>
