<!doctype html>
<html lang="ru">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Мини-игра: Лови квадрат</title>
  <style>
    :root{
      --bg:#0f1220; --card:#171a2b; --text:#e9ecff; --muted:#aab0d6;
      --accent:#6ea8fe; --danger:#ff6b6b; --good:#51cf66;
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0; font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif;
      color:var(--text);
      background:radial-gradient(1200px 700px at 20% 10%, #1b1f39 0%, var(--bg) 60%);
    }
    .page{min-height:100%; display:grid; place-items:center; padding:24px}
    .card{
      width:min(720px, 94vw);
      background:rgba(23,26,43,.92);
      border:1px solid rgba(255,255,255,.08);
      border-radius:18px;
      padding:18px;
      box-shadow:0 18px 60px rgba(0,0,0,.35);
    }
    h1{margin:0 0 10px; font-size:28px}
    p{margin:10px 0; color:var(--muted)}
    .row{display:flex; gap:10px; flex-wrap:wrap; align-items:center}
    .btn{
      appearance:none; border:0; border-radius:12px; padding:12px 16px;
      cursor:pointer; color:#0b1020; background:var(--accent);
      font-weight:800; font-size:16px;
      transition:transform .08s ease, filter .12s ease;
    }
    .btn:hover{filter:brightness(1.05)}
    .btn:active{transform:translateY(1px) scale(.99)}
    .btn-ghost{
      background:transparent; border:1px solid rgba(255,255,255,.18);
      color:var(--text);
    }
    .hint{font-size:13px; color:rgba(233,236,255,.72)}
    .topbar{
      display:flex; align-items:center; justify-content:space-between; gap:12px;
      padding:12px 14px; border-radius:14px;
      border:1px solid rgba(255,255,255,.08);
      background:rgba(255,255,255,.03);
      margin-top:14px;
    }
    .stats{display:flex; gap:16px; flex-wrap:wrap; color:var(--muted)}
    .stats b{color:var(--text)}
    .arena-wrap{display:grid; place-items:center; gap:10px; padding-top:14px}
    .arena{
      width:min(900px, 92vw);
      height:min(560px, 68vh);
      border-radius:18px;
      position:relative;
      background:rgba(255,255,255,.04);
      border:1px solid rgba(255,255,255,.08);
      overflow:hidden;
      user-select:none;
    }
    .target{
      width:56px; height:56px; position:absolute;
      left:50%; top:50%; transform:translate(-50%,-50%);
      border-radius:14px;
      background:linear-gradient(180deg, rgba(255,255,255,.18), rgba(255,255,255,.05));
      border:1px solid rgba(255,255,255,.22);
      box-shadow:0 10px 30px rgba(0,0,0,.35);
      cursor:pointer;
    }
    .target.good{outline:2px solid rgba(81,207,102,.6)}
    .target.bad{outline:2px solid rgba(255,107,107,.65)}
    .split{display:grid; grid-template-columns:1fr; gap:14px}
    @media (min-width: 780px){
      .split{grid-template-columns: 1fr 1fr}
    }
    .mini{
      background:rgba(255,255,255,.03);
      border:1px solid rgba(255,255,255,.08);
      border-radius:14px;
      padding:14px;
    }
    input[type="number"]{
      width:110px;
      padding:10px 12px;
      border-radius:12px;
      border:1px solid rgba(255,255,255,.18);
      background:rgba(0,0,0,.18);
      color:var(--text);
      outline:none;
    }
    label{color:var(--muted); font-size:14px}
    code{color:#d7dbff}
  </style>
</head>
<body>
  <div class="page">
    <main class="card">
      <h1>Мини-игра: «Лови квадрат»</h1>
      <p>Кликни по цели, набирай очки. Промах по полю минус очко. И да, открывается <b>в отдельном окне</b>.</p>

      <div class="split">
        <section class="mini">
          <div class="row" style="justify-content:space-between">
            <div>
              <label for="seconds">Время (сек):</label><br />
              <input id="seconds" type="number" min="5" max="180" value="30" />
            </div>
            <div class="row">
              <button class="btn" id="openGame">Открыть игру</button>
              <button class="btn btn-ghost" id="resetBest">Сбросить рекорд</button>
            </div>
          </div>
          <p class="hint">Если браузер блокирует всплывающие окна, он попросит разрешение.</p>
          <p class="hint">GitHub Pages: просто положи этот файл как <code>index.html</code> в репозиторий.</p>
        </section>

        <section class="mini">
          <p><b>Как включить GitHub Pages</b> (для тех, кто любит квесты):</p>
          <ol class="hint" style="margin:8px 0 0 18px">
            <li>Repo → <b>Settings</b> → <b>Pages</b></li>
            <li>Source: <b>Deploy from a branch</b></li>
            <li>Branch: <b>main</b>, folder: <b>/(root)</b></li>
            <li>Открой ссылку вида: <code>https://USERNAME.github.io/REPO/</code></li>
          </ol>
        </section>
      </div>

      <div class="topbar" aria-label="Панель статистики">
        <div class="stats">
          <div><b>Рекорд:</b> <span id="best">0</span></div>
        </div>
        <div class="hint">Рекорд хранится в браузере (LocalStorage).</div>
      </div>
    </main>
  </div>

  <script>
    "use strict";

    const BEST_KEY = "catch_square_best_v1";

    const secondsInput = document.getElementById("seconds");
    const openBtn = document.getElementById("openGame");
    const resetBestBtn = document.getElementById("resetBest");
    const bestEl = document.getElementById("best");

    function getBest() {
      const raw = localStorage.getItem(BEST_KEY);
      const n = raw ? Number(raw) : 0;
      return Number.isFinite(n) ? n : 0;
    }
    function setBest(n) { localStorage.setItem(BEST_KEY, String(n)); }
    function updateBestUI(){ bestEl.textContent = String(getBest()); }

    function clamp(n, min, max) { return Math.max(min, Math.min(max, n)); }

    // Открываем отдельное окно и рисуем игру "на лету"
    openBtn.addEventListener("click", () => {
      const totalSeconds = clamp(Number(secondsInput.value || 30), 5, 180);

      const w = window.open("", "_blank", "noopener,noreferrer,width=900,height=650");
      if (!w) return;

      // Вставляем полный HTML игры прямо в новое окно
      w.document.open();
      w.document.write(`<!doctype html>
<html lang="ru">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Лови квадрат — игра</title>
  <style>
    :root{
      --bg:#0f1220; --card:#171a2b; --text:#e9ecff; --muted:#aab0d6;
      --accent:#6ea8fe; --danger:#ff6b6b; --good:#51cf66;
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0; font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif;
      color:var(--text);
      background:radial-gradient(1200px 700px at 20% 10%, #1b1f39 0%, var(--bg) 60%);
      display:grid; grid-template-rows:auto 1fr;
    }
    .topbar{
      display:flex; align-items:center; justify-content:space-between; gap:12px;
      padding:14px 16px; border-bottom:1px solid rgba(255,255,255,.08);
      background:rgba(23,26,43,.75); backdrop-filter:blur(10px);
    }
    .stats{display:flex; gap:18px; flex-wrap:wrap; color:var(--muted)}
    .stats b{color:var(--text)}
    .actions{display:flex; gap:10px}
    .btn{
      appearance:none; border:0; border-radius:12px; padding:10px 14px;
      cursor:pointer; color:#0b1020; background:var(--accent);
      font-weight:800; font-size:15px;
      transition:transform .08s ease, filter .12s ease;
    }
    .btn:hover{filter:brightness(1.05)}
    .btn:active{transform:translateY(1px) scale(.99)}
    .btn-ghost{
      background:transparent; border:1px solid rgba(255,255,255,.18);
      color:var(--text);
    }
    .arena-wrap{display:grid; place-items:center; padding:18px; gap:10px}
    .arena{
      width:min(900px, 92vw);
      height:min(560px, 70vh);
      border-radius:18px;
      position:relative;
      background:rgba(255,255,255,.04);
      border:1px solid rgba(255,255,255,.08);
      overflow:hidden;
      user-select:none;
    }
    .target{
      width:56px; height:56px; position:absolute;
      left:50%; top:50%; transform:translate(-50%,-50%);
      border-radius:14px;
      background:linear-gradient(180deg, rgba(255,255,255,.18), rgba(255,255,255,.05));
      border:1px solid rgba(255,255,255,.22);
      box-shadow:0 10px 30px rgba(0,0,0,.35);
      cursor:pointer;
    }
    .target.good{outline:2px solid rgba(81,207,102,.6)}
    .target.bad{outline:2px solid rgba(255,107,107,.65)}
    .hint{font-size:13px; color:rgba(233,236,255,.72); margin:0}
  </style>
</head>
<body>
  <header class="topbar">
    <div class="stats">
      <div><b>Очки:</b> <span id="score">0</span></div>
      <div><b>Время:</b> <span id="time">${totalSeconds}</span>с</div>
      <div><b>Рекорд:</b> <span id="best">0</span></div>
    </div>
    <div class="actions">
      <button id="start" class="btn">Старт</button>
      <button id="reset" class="btn btn-ghost">Сброс</button>
    </div>
  </header>

  <main class="arena-wrap">
    <div id="arena" class="arena" aria-label="Игровое поле">
      <div id="target" class="target" role="button" aria-label="Цель"></div>
    </div>
    <p class="hint">Кликай по квадрату. Промах по полю: −1 очко.</p>
  </main>

  <script>
    "use strict";
    const BEST_KEY = "${BEST_KEY}";
    const arena = document.getElementById("arena");
    const target = document.getElementById("target");
    const scoreEl = document.getElementById("score");
    const timeEl  = document.getElementById("time");
    const bestEl  = document.getElementById("best");
    const startBtn = document.getElementById("start");
    const resetBtn = document.getElementById("reset");

    let score = 0;
    let timeLeft = ${totalSeconds};
    let running = false;
    let tickTimer = null;
    let moveTimer = null;

    function clamp(n, min, max){ return Math.max(min, Math.min(max, n)); }

    function getBest(){
      const raw = localStorage.getItem(BEST_KEY);
      const n = raw ? Number(raw) : 0;
      return Number.isFinite(n) ? n : 0;
    }
    function setBest(n){ localStorage.setItem(BEST_KEY, String(n)); }

    function updateUI(){
      scoreEl.textContent = String(score);
      timeEl.textContent = String(timeLeft);
      bestEl.textContent = String(getBest());
    }

    function setTargetPos(x,y){
      target.style.left = x + "px";
      target.style.top  = y + "px";
      target.style.transform = "translate(-50%, -50%)";
    }

    function randomizeTarget(){
      const rect = arena.getBoundingClientRect();
      const padding = 40;
      const x = Math.random() * (rect.width  - padding*2) + padding;
      const y = Math.random() * (rect.height - padding*2) + padding;
      setTargetPos(x,y);
    }

    function flash(cls){
      target.classList.remove("good","bad");
      target.classList.add(cls);
      setTimeout(() => target.classList.remove(cls), 120);
    }

    function stopGame(){
      running = false;
      clearInterval(tickTimer);
      clearInterval(moveTimer);
      tickTimer = null; moveTimer = null;

      const best = getBest();
      if (score > best) setBest(score);

      startBtn.textContent = "Старт";
      updateUI();
    }

    function startGame(){
      if (tickTimer || moveTimer) stopGame();
      score = 0;
      timeLeft = ${totalSeconds};
      running = true;

      startBtn.textContent = "Пауза";
      updateUI();
      randomizeTarget();

      tickTimer = setInterval(() => {
        if (!running) return;
        timeLeft = clamp(timeLeft - 1, 0, 999);
        updateUI();
        if (timeLeft <= 0) stopGame();
      }, 1000);

      moveTimer = setInterval(() => {
        if (!running) return;
        randomizeTarget();
      }, 700);
    }

    function togglePause(){
      if (!tickTimer || !moveTimer) return;
      running = !running;
      startBtn.textContent = running ? "Пауза" : "Продолжить";
    }

    target.addEventListener("click", (e) => {
      e.stopPropagation();
      if (!running) return;
      score += 1;
      flash("good");
      randomizeTarget();
      updateUI();
    });

    arena.addEventListener("click", () => {
      if (!running) return;
      score -= 1;
      flash("bad");
      updateUI();
    });

    startBtn.addEventListener("click", () => {
      if (!tickTimer && !moveTimer) startGame();
      else togglePause();
    });

    resetBtn.addEventListener("click", () => {
      stopGame();
      score = 0;
      timeLeft = ${totalSeconds};
      updateUI();
      randomizeTarget();
    });

    updateUI();
    randomizeTarget();
  <\/script>
</body>
</html>`);
      w.document.close();
    });

    resetBestBtn.addEventListener("click", () => {
      localStorage.removeItem(BEST_KEY);
      updateBestUI();
    });

    updateBestUI();
  </script>
</body>
</html>
