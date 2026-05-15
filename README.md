<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Love Letter ✉️</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;1,400;1,600&family=Cormorant+Garamond:ital,wght@0,300;0,400;1,300;1,400&family=Great+Vibes&display=swap" rel="stylesheet">
<style>
  :root {
    --rose: #c0394b;
    --blush: #f7e0e4;
    --petal: #f0c0cc;
    --ink: #2a1520;
    --cream: #fdf6f0;
    --gold: #b8860b;
    --shadow: rgba(192,57,75,0.18);
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'Cormorant Garamond', serif;
    background: var(--cream);
    color: var(--ink);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ---- PETALS BG ---- */
  .petal-bg {
    position: fixed; inset: 0; pointer-events: none; z-index: 0;
    overflow: hidden;
  }
  .petal {
    position: absolute;
    width: 18px; height: 22px;
    background: radial-gradient(ellipse at 40% 40%, #f7c0cc, #e8809a88);
    border-radius: 60% 40% 60% 40% / 60% 60% 40% 40%;
    opacity: 0;
    animation: fall linear infinite;
  }
  @keyframes fall {
    0%   { transform: translateY(-60px) rotate(0deg) scale(0.8); opacity: 0; }
    10%  { opacity: 0.7; }
    90%  { opacity: 0.5; }
    100% { transform: translateY(110vh) rotate(360deg) scale(1); opacity: 0; }
  }

  /* ---- TABS ---- */
  #app { position: relative; z-index: 1; }
  .tabs {
    display: flex;
    justify-content: center;
    gap: 0;
    padding: 28px 20px 0;
  }
  .tab-btn {
    font-family: 'Playfair Display', serif;
    font-size: 1rem;
    font-weight: 700;
    letter-spacing: 0.08em;
    padding: 12px 32px;
    border: 2px solid var(--rose);
    background: transparent;
    color: var(--rose);
    cursor: pointer;
    transition: all 0.25s;
    position: relative;
  }
  .tab-btn:first-child { border-radius: 30px 0 0 30px; }
  .tab-btn:last-child  { border-radius: 0 30px 30px 0; }
  .tab-btn.active {
    background: var(--rose);
    color: #fff;
  }
  .tab-btn:hover:not(.active) { background: var(--blush); }

  /* ---- EDITOR PANEL ---- */
  #editor, #preview-panel { display: none; max-width: 760px; margin: 32px auto; padding: 0 20px 60px; }
  #editor.active, #preview-panel.active { display: block; }

  .section-title {
    font-family: 'Great Vibes', cursive;
    font-size: 2.6rem;
    color: var(--rose);
    text-align: center;
    margin-bottom: 28px;
  }

  .card {
    background: #fff;
    border-radius: 20px;
    box-shadow: 0 8px 40px var(--shadow);
    padding: 36px 40px;
    margin-bottom: 24px;
  }

  label {
    display: block;
    font-family: 'Playfair Display', serif;
    font-size: 0.85rem;
    font-weight: 700;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--rose);
    margin-bottom: 8px;
    margin-top: 20px;
  }
  label:first-child { margin-top: 0; }

  input[type=text], textarea, select {
    width: 100%;
    font-family: 'Cormorant Garamond', serif;
    font-size: 1.1rem;
    padding: 12px 16px;
    border: 2px solid var(--petal);
    border-radius: 12px;
    background: var(--blush);
    color: var(--ink);
    outline: none;
    transition: border-color 0.2s;
    resize: vertical;
  }
  input[type=text]:focus, textarea:focus, select:focus { border-color: var(--rose); }
  textarea { min-height: 140px; }

  .upload-zone {
    border: 2px dashed var(--petal);
    border-radius: 16px;
    background: var(--blush);
    text-align: center;
    padding: 30px 20px;
    cursor: pointer;
    transition: border-color 0.2s, background 0.2s;
    position: relative;
  }
  .upload-zone:hover { border-color: var(--rose); background: #f9eaed; }
  .upload-zone input[type=file] { position: absolute; inset: 0; opacity: 0; cursor: pointer; width: 100%; height: 100%; }
  .upload-zone p { font-size: 1rem; color: #b07080; }
  .upload-zone .upload-icon { font-size: 2.5rem; margin-bottom: 8px; }
  .upload-preview {
    max-width: 100%;
    max-height: 200px;
    border-radius: 12px;
    margin-top: 14px;
    display: none;
    object-fit: cover;
    width: 100%;
  }

  .sound-list {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
    gap: 12px;
    margin-top: 8px;
  }
  .sound-opt {
    border: 2px solid var(--petal);
    border-radius: 12px;
    padding: 12px 10px;
    text-align: center;
    cursor: pointer;
    font-family: 'Playfair Display', serif;
    font-size: 0.88rem;
    font-weight: 700;
    color: var(--rose);
    background: var(--blush);
    transition: all 0.2s;
    position: relative;
  }
  .sound-opt.selected { background: var(--rose); color: #fff; border-color: var(--rose); }
  .sound-opt:hover:not(.selected) { background: #f7d0d8; }
  .sound-opt .sound-icon { font-size: 1.6rem; display: block; margin-bottom: 5px; }

  .color-row {
    display: flex; gap: 14px; flex-wrap: wrap; margin-top: 8px;
  }
  .color-swatch {
    width: 40px; height: 40px; border-radius: 50%;
    cursor: pointer; border: 3px solid transparent;
    transition: border-color 0.2s, transform 0.2s;
  }
  .color-swatch.selected, .color-swatch:hover { border-color: var(--ink); transform: scale(1.15); }

  /* CTA buttons */
  .btn {
    display: inline-block;
    font-family: 'Playfair Display', serif;
    font-size: 1rem;
    font-weight: 700;
    letter-spacing: 0.08em;
    padding: 14px 40px;
    border-radius: 50px;
    border: none;
    cursor: pointer;
    transition: all 0.25s;
  }
  .btn-rose { background: var(--rose); color: #fff; box-shadow: 0 6px 28px var(--shadow); }
  .btn-rose:hover { background: #a02a3c; transform: translateY(-2px); }
  .btn-outline { background: transparent; border: 2px solid var(--rose); color: var(--rose); }
  .btn-outline:hover { background: var(--blush); }
  .btn-row { display: flex; gap: 14px; justify-content: center; margin-top: 10px; flex-wrap: wrap; }

  /* ---- LETTER PREVIEW ---- */
  .letter-wrap {
    position: relative;
    border-radius: 24px;
    overflow: hidden;
    box-shadow: 0 20px 80px rgba(192,57,75,0.22), 0 4px 20px rgba(0,0,0,0.08);
    min-height: 520px;
  }
  .letter-bg {
    position: absolute; inset: 0;
    background: var(--letter-bg, var(--cream));
    transition: background 0.4s;
    z-index: 0;
  }
  .letter-noise {
    position: absolute; inset: 0; z-index: 1; opacity: 0.04;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='1'/%3E%3C/svg%3E");
  }
  .letter-content {
    position: relative; z-index: 2;
    padding: 50px 52px 52px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 28px;
  }

  .letter-photo {
    width: 200px; height: 200px;
    border-radius: 50%;
    object-fit: cover;
    border: 5px solid #fff;
    box-shadow: 0 6px 32px rgba(192,57,75,0.25);
    display: block;
    transition: all 0.3s;
  }
  .letter-photo.hidden { display: none; }

  .letter-photo-placeholder {
    width: 200px; height: 200px; border-radius: 50%;
    background: var(--blush);
    display: flex; align-items: center; justify-content: center;
    font-size: 4rem;
    border: 4px dashed var(--petal);
  }

  .letter-to {
    font-family: 'Great Vibes', cursive;
    font-size: 2.1rem;
    color: var(--letter-accent, var(--rose));
    text-align: center;
  }
  .letter-body {
    font-family: 'Cormorant Garamond', serif;
    font-size: 1.25rem;
    line-height: 1.85;
    color: var(--ink);
    text-align: center;
    max-width: 520px;
    white-space: pre-wrap;
  }
  .letter-from {
    font-family: 'Great Vibes', cursive;
    font-size: 1.8rem;
    color: var(--letter-accent, var(--rose));
    text-align: right;
    align-self: flex-end;
    margin-right: 20px;
  }
  .letter-hearts {
    font-size: 1.6rem;
    letter-spacing: 8px;
    animation: pulse 2s ease-in-out infinite;
  }
  @keyframes pulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.12); }
  }

  .letter-divider {
    width: 80px; height: 2px;
    background: linear-gradient(to right, transparent, var(--letter-accent, var(--rose)), transparent);
    border: none; margin: 0 auto;
  }

  /* ---- SHARE ---- */
  .share-box {
    background: var(--blush);
    border-radius: 16px;
    padding: 24px 28px;
    margin-top: 24px;
    text-align: center;
  }
  .share-box h3 {
    font-family: 'Playfair Display', serif;
    font-size: 1.1rem;
    font-weight: 700;
    color: var(--rose);
    margin-bottom: 14px;
  }
  .share-link-row {
    display: flex; gap: 10px; align-items: center;
  }
  .share-link-row input {
    flex: 1;
    font-size: 0.88rem;
    padding: 10px 14px;
    border-radius: 10px;
    border: 2px solid var(--petal);
    background: #fff;
    color: var(--ink);
    font-family: monospace;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .btn-copy {
    padding: 10px 20px;
    border-radius: 10px;
    background: var(--rose);
    color: #fff;
    border: none;
    font-family: 'Playfair Display', serif;
    font-weight: 700;
    cursor: pointer;
    white-space: nowrap;
    transition: background 0.2s;
  }
  .btn-copy:hover { background: #a02a3c; }
  .copy-msg {
    font-size: 0.88rem;
    color: var(--rose);
    margin-top: 8px;
    height: 18px;
    transition: opacity 0.3s;
  }

  /* ---- MUSIC PLAYER ---- */
  .music-indicator {
    position: fixed;
    bottom: 28px; right: 28px;
    background: var(--rose);
    color: #fff;
    border-radius: 50%;
    width: 56px; height: 56px;
    display: flex; align-items: center; justify-content: center;
    font-size: 1.6rem;
    box-shadow: 0 6px 24px var(--shadow);
    cursor: pointer;
    z-index: 100;
    border: none;
    transition: transform 0.2s;
  }
  .music-indicator:hover { transform: scale(1.12); }
  .music-indicator.spinning span {
    display: inline-block;
    animation: spin 2s linear infinite;
  }
  @keyframes spin { 100% { transform: rotate(360deg); } }

  /* ---- RESPONSIVE ---- */
  @media (max-width: 600px) {
    .card { padding: 24px 20px; }
    .letter-content { padding: 34px 24px 40px; }
    .letter-body { font-size: 1.1rem; }
    .tabs { padding: 18px 10px 0; }
    .tab-btn { padding: 10px 20px; font-size: 0.9rem; }
  }

  /* ---- NOTIFICATION TOAST ---- */
  .toast {
    position: fixed; bottom: 100px; left: 50%; transform: translateX(-50%);
    background: var(--rose); color: #fff;
    padding: 12px 28px; border-radius: 40px;
    font-family: 'Playfair Display', serif; font-weight: 700;
    box-shadow: 0 6px 28px var(--shadow);
    z-index: 200; opacity: 0;
    transition: opacity 0.3s;
    pointer-events: none;
    white-space: nowrap;
  }
  .toast.show { opacity: 1; }

  /* ---- ANIMATE IN ---- */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(28px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  .card { animation: fadeUp 0.5s ease both; }
  .card:nth-child(2) { animation-delay: 0.08s; }
  .card:nth-child(3) { animation-delay: 0.16s; }
  .card:nth-child(4) { animation-delay: 0.24s; }
</style>
</head>
<body>

<div class="petal-bg" id="petalBg"></div>

<div id="app">
  <div class="tabs">
    <button class="tab-btn active" onclick="showTab('editor')">✏️ Create</button>
    <button class="tab-btn" onclick="showTab('preview-panel')">💌 Preview & Share</button>
  </div>

  <!-- EDITOR -->
  <div id="editor" class="active">
    <div class="section-title">Write Your Love Letter</div>

    <div class="card">
      <label>To (recipient's name)</label>
      <input type="text" id="toName" placeholder="My Dearest..." oninput="syncPreview()">

      <label>From (your name)</label>
      <input type="text" id="fromName" placeholder="Forever Yours..." oninput="syncPreview()">

      <label>Your Message</label>
      <textarea id="letterBody" placeholder="Every moment with you feels like a dream I never want to wake from..." oninput="syncPreview()"></textarea>
    </div>

    <div class="card">
      <label>📷 Your Photo (optional)</label>
      <div class="upload-zone" id="photoZone">
        <input type="file" accept="image/*" id="photoInput" onchange="handlePhoto(event)">
        <div class="upload-icon">🌹</div>
        <p>Click or drag a photo here</p>
        <img id="uploadPreview" class="upload-preview" alt="preview">
      </div>
    </div>

    <div class="card">
      <label>🎵 Background Music</label>
      <div class="sound-list" id="soundList">
        <div class="sound-opt selected" data-sound="none" onclick="selectSound(this)">
          <span class="sound-icon">🔇</span>No Music
        </div>
        <div class="sound-opt" data-sound="piano" onclick="selectSound(this)">
          <span class="sound-icon">🎹</span>Piano
        </div>
        <div class="sound-opt" data-sound="violin" onclick="selectSound(this)">
          <span class="sound-icon">🎻</span>Strings
        </div>
        <div class="sound-opt" data-sound="guitar" onclick="selectSound(this)">
          <span class="sound-icon">🎸</span>Guitar
        </div>
        <div class="sound-opt" data-sound="harp" onclick="selectSound(this)">
          <span class="sound-icon">✨</span>Harp
        </div>
        <div class="sound-opt" data-sound="custom" onclick="triggerCustomAudio()">
          <span class="sound-icon">📁</span>Upload
        </div>
      </div>
      <input type="file" id="audioInput" accept="audio/*" style="display:none" onchange="handleCustomAudio(event)">
      <p id="audioFileName" style="margin-top:10px;font-size:0.9rem;color:#b07080;"></p>
    </div>

    <div class="card">
      <label>🎨 Letter Theme Color</label>
      <div class="color-row" id="colorRow">
        <div class="color-swatch selected" data-color="#c0394b" style="background:#c0394b" onclick="selectColor(this)" title="Rose"></div>
        <div class="color-swatch" data-color="#a0522d" style="background:#a0522d" onclick="selectColor(this)" title="Cinnamon"></div>
        <div class="color-swatch" data-color="#b56f8c" style="background:#b56f8c" onclick="selectColor(this)" title="Dusty Mauve"></div>
        <div class="color-swatch" data-color="#4a7fa5" style="background:#4a7fa5" onclick="selectColor(this)" title="Sapphire"></div>
        <div class="color-swatch" data-color="#5a8a6a" style="background:#5a8a6a" onclick="selectColor(this)" title="Forest"></div>
        <div class="color-swatch" data-color="#b8860b" style="background:#b8860b" onclick="selectColor(this)" title="Gold"></div>
        <div class="color-swatch" data-color="#7e57a0" style="background:#7e57a0" onclick="selectColor(this)" title="Violet"></div>
        <div class="color-swatch" data-color="#2a1520" style="background:#2a1520" onclick="selectColor(this)" title="Midnight"></div>
      </div>
      <label style="margin-top:20px">🖼 Background Style</label>
      <div class="color-row" id="bgRow">
        <div class="color-swatch selected" data-bg="#fdf6f0" style="background:#fdf6f0;border:2px solid #e0c0b0" onclick="selectBg(this)" title="Cream"></div>
        <div class="color-swatch" data-bg="#fff0f3" style="background:#fff0f3;border:2px solid #f0c0cc" onclick="selectBg(this)" title="Blush"></div>
        <div class="color-swatch" data-bg="#fffff0" style="background:#fffff0;border:2px solid #d4c080" onclick="selectBg(this)" title="Ivory"></div>
        <div class="color-swatch" data-bg="#1a1020" style="background:#1a1020" onclick="selectBg(this)" title="Midnight"></div>
        <div class="color-swatch" data-bg="#0d1f2d" style="background:#0d1f2d" onclick="selectBg(this)" title="Deep Sea"></div>
        <div class="color-swatch" data-bg="#1a2410" style="background:#1a2410" onclick="selectBg(this)" title="Emerald Night"></div>
      </div>
    </div>

    <div class="btn-row">
      <button class="btn btn-rose" onclick="showTab('preview-panel')">Preview My Letter 💌</button>
    </div>
  </div>

  <!-- PREVIEW -->
  <div id="preview-panel">
    <div class="section-title">Your Love Letter</div>

    <div class="letter-wrap">
      <div class="letter-bg" id="letterBg"></div>
      <div class="letter-noise"></div>
      <div class="letter-content">
        <div class="letter-hearts" id="previewHearts">♥ ♥ ♥</div>
        <div id="photoArea">
          <div class="letter-photo-placeholder" id="photoPlaceholder">🌹</div>
          <img id="previewPhoto" class="letter-photo hidden" alt="Your photo">
        </div>
        <div class="letter-to" id="previewTo">My Dearest...</div>
        <hr class="letter-divider" id="divider1">
        <div class="letter-body" id="previewBody">Your heartfelt words will appear here...</div>
        <hr class="letter-divider" id="divider2">
        <div class="letter-from" id="previewFrom">— Forever Yours</div>
        <div class="letter-hearts" id="previewHearts2">♥ ♥ ♥</div>
      </div>
    </div>

    <div class="share-box">
      <h3>💌 Share This Letter</h3>
      <div class="share-link-row">
        <input type="text" id="shareUrl" readonly placeholder="Generate your share link...">
        <button class="btn-copy" onclick="copyLink()">Copy</button>
      </div>
      <div class="copy-msg" id="copyMsg"></div>
      <div class="btn-row" style="margin-top:18px">
        <button class="btn btn-rose" onclick="generateShareLink()">✨ Generate Share Link</button>
        <button class="btn btn-outline" onclick="showTab('editor')">← Edit Letter</button>
      </div>
    </div>
  </div>
</div>

<button class="music-indicator" id="musicBtn" onclick="toggleMusic()" title="Toggle music" style="display:none">
  <span>🎵</span>
</button>

<div class="toast" id="toast"></div>

<audio id="bgAudio" loop></audio>

<script>
  // ---- PETALS ----
  const petalBg = document.getElementById('petalBg');
  for (let i = 0; i < 18; i++) {
    const p = document.createElement('div');
    p.className = 'petal';
    p.style.cssText = `
      left: ${Math.random()*100}%;
      animation-duration: ${6+Math.random()*10}s;
      animation-delay: ${-Math.random()*14}s;
      width: ${12+Math.random()*16}px;
      height: ${14+Math.random()*18}px;
      transform: rotate(${Math.random()*360}deg);
    `;
    petalBg.appendChild(p);
  }

  // ---- STATE ----
  let state = {
    to: '', from: '', body: '',
    photoDataUrl: null,
    sound: 'none',
    customAudioUrl: null,
    color: '#c0394b',
    bg: '#fdf6f0'
  };

  // ---- TABS ----
  function showTab(id) {
    document.querySelectorAll('.tab-btn').forEach((b,i) => {
      b.classList.toggle('active', (i===0 && id==='editor') || (i===1 && id==='preview-panel'));
    });
    document.getElementById('editor').classList.toggle('active', id==='editor');
    document.getElementById('preview-panel').classList.toggle('active', id==='preview-panel');
    if (id==='preview-panel') { syncPreview(); generateShareLink(); }
  }

  // ---- SYNC PREVIEW ----
  function syncPreview() {
    state.to   = document.getElementById('toName').value.trim();
    state.from = document.getElementById('fromName').value.trim();
    state.body = document.getElementById('letterBody').value.trim();

    document.getElementById('previewTo').textContent  = state.to   ? 'My Dearest ' + state.to + ',' : 'My Dearest...';
    document.getElementById('previewFrom').textContent = state.from ? '— ' + state.from : '— Forever Yours';
    document.getElementById('previewBody').textContent = state.body || 'Your heartfelt words will appear here...';

    // accent color
    const accent = state.color;
    document.getElementById('previewTo').style.color   = accent;
    document.getElementById('previewFrom').style.color = accent;
    document.getElementById('divider1').style.backgroundImage = `linear-gradient(to right, transparent, ${accent}, transparent)`;
    document.getElementById('divider2').style.backgroundImage = `linear-gradient(to right, transparent, ${accent}, transparent)`;

    // bg
    document.getElementById('letterBg').style.background = state.bg;
    const isDark = isDarkColor(state.bg);
    document.getElementById('previewBody').style.color = isDark ? '#f0d8e0' : '#2a1520';

    // photo
    if (state.photoDataUrl) {
      document.getElementById('previewPhoto').src = state.photoDataUrl;
      document.getElementById('previewPhoto').classList.remove('hidden');
      document.getElementById('photoPlaceholder').style.display = 'none';
    } else {
      document.getElementById('previewPhoto').classList.add('hidden');
      document.getElementById('photoPlaceholder').style.display = 'flex';
    }
  }

  function isDarkColor(hex) {
    if (!hex.startsWith('#')) return false;
    const r = parseInt(hex.slice(1,3),16), g = parseInt(hex.slice(3,5),16), b = parseInt(hex.slice(5,7),16);
    return (r*299 + g*587 + b*114) / 1000 < 128;
  }

  // ---- PHOTO ----
  function handlePhoto(e) {
    const file = e.target.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = ev => {
      state.photoDataUrl = ev.target.result;
      const prev = document.getElementById('uploadPreview');
      prev.src = ev.target.result;
      prev.style.display = 'block';
      syncPreview();
    };
    reader.readAsDataURL(file);
  }

  // ---- SOUND ----
  const soundFreqs = {
    piano:  [261.63, 293.66, 329.63, 349.23, 392.00, 440.00, 493.88, 523.25],
    violin: [196, 220, 246.94, 261.63, 293.66, 329.63],
    guitar: [82.41, 110, 146.83, 196, 246.94, 329.63],
    harp:   [523.25, 587.33, 659.25, 698.46, 783.99, 880]
  };

  let audioCtx = null, gainNode = null, activeOscillators = [];

  function selectSound(el) {
    document.querySelectorAll('.sound-opt').forEach(s => s.classList.remove('selected'));
    el.classList.add('selected');
    state.sound = el.dataset.sound;
    stopSynth();
    if (state.sound !== 'none' && state.sound !== 'custom') {
      document.getElementById('musicBtn').style.display = 'flex';
      startSynth(state.sound);
    } else if (state.sound === 'none') {
      document.getElementById('musicBtn').style.display = 'none';
    }
  }

  function startSynth(type) {
    stopSynth();
    audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    gainNode = audioCtx.createGain();
    gainNode.gain.setValueAtTime(0.08, audioCtx.currentTime);
    gainNode.connect(audioCtx.destination);
    const freqs = soundFreqs[type] || soundFreqs.piano;
    function playNote(freq, time, dur) {
      const osc = audioCtx.createOscillator();
      const g = audioCtx.createGain();
      osc.connect(g); g.connect(gainNode);
      osc.type = type==='violin' ? 'sawtooth' : type==='guitar' ? 'triangle' : 'sine';
      osc.frequency.setValueAtTime(freq, time);
      g.gain.setValueAtTime(0, time);
      g.gain.linearRampToValueAtTime(0.6, time + 0.1);
      g.gain.linearRampToValueAtTime(0, time + dur);
      osc.start(time); osc.stop(time + dur + 0.1);
      activeOscillators.push(osc);
    }
    let t = audioCtx.currentTime;
    const pattern = [0,2,4,5,4,2,0,5,7,5,4,2];
    function scheduleLoop() {
      pattern.forEach((fi, i) => {
        const f = freqs[fi % freqs.length];
        playNote(f, t + i*0.55, 0.5);
      });
      t += pattern.length * 0.55;
      if (t < audioCtx.currentTime + 20) scheduleLoop();
      else setTimeout(scheduleLoop, (t - audioCtx.currentTime - 15) * 1000);
    }
    scheduleLoop();
    document.getElementById('musicBtn').style.display = 'flex';
    document.getElementById('musicBtn').classList.add('spinning');
    showToast('🎵 Music playing...');
  }

  function stopSynth() {
    if (audioCtx) { audioCtx.close(); audioCtx = null; }
    activeOscillators = [];
    document.getElementById('musicBtn').classList.remove('spinning');
  }

  let musicPlaying = false;
  function toggleMusic() {
    musicPlaying = !musicPlaying;
    if (musicPlaying) {
      startSynth(state.sound !== 'none' && state.sound !== 'custom' ? state.sound : 'piano');
    } else {
      stopSynth();
      showToast('🔇 Music paused');
    }
  }

  function triggerCustomAudio() {
    document.getElementById('audioInput').click();
  }
  function handleCustomAudio(e) {
    const file = e.target.files[0]; if (!file) return;
    stopSynth();
    document.querySelectorAll('.sound-opt').forEach(s=>s.classList.remove('selected'));
    document.querySelector('[data-sound=custom]').classList.add('selected');
    state.sound = 'custom';
    const url = URL.createObjectURL(file);
    state.customAudioUrl = url;
    const audio = document.getElementById('bgAudio');
    audio.src = url;
    audio.play();
    musicPlaying = true;
    document.getElementById('musicBtn').style.display = 'flex';
    document.getElementById('musicBtn').classList.add('spinning');
    document.getElementById('audioFileName').textContent = '🎵 ' + file.name;
    showToast('🎵 Your music is playing!');
    audio.addEventListener('ended', () => { document.getElementById('musicBtn').classList.remove('spinning'); });
  }

  // ---- COLOR ----
  function selectColor(el) {
    document.querySelectorAll('#colorRow .color-swatch').forEach(s=>s.classList.remove('selected'));
    el.classList.add('selected');
    state.color = el.dataset.color;
    syncPreview();
  }
  function selectBg(el) {
    document.querySelectorAll('#bgRow .color-swatch').forEach(s=>s.classList.remove('selected'));
    el.classList.add('selected');
    state.bg = el.dataset.bg;
    syncPreview();
  }

  // ---- SHARE LINK ----
  function generateShareLink() {
    syncPreview();
    const data = {
      to: state.to, from: state.from, body: state.body,
      color: state.color, bg: state.bg,
      photo: state.photoDataUrl ? 'included' : null
    };
    const encoded = btoa(unescape(encodeURIComponent(JSON.stringify(data))));
    const url = window.location.href.split('?')[0] + '?letter=' + encoded;
    document.getElementById('shareUrl').value = url;
  }

  function copyLink() {
    const url = document.getElementById('shareUrl').value;
    if (!url || url.includes('Generate')) return;
    navigator.clipboard.writeText(url).then(() => {
      const msg = document.getElementById('copyMsg');
      msg.textContent = '✓ Copied to clipboard!';
      showToast('💌 Link copied! Share it with love.');
      setTimeout(()=>{ msg.textContent=''; }, 3000);
    });
  }

  // ---- LOAD FROM URL ----
  function loadFromUrl() {
    const params = new URLSearchParams(window.location.search);
    const encoded = params.get('letter');
    if (!encoded) return;
    try {
      const data = JSON.parse(decodeURIComponent(escape(atob(encoded))));
      document.getElementById('toName').value = data.to || '';
      document.getElementById('fromName').value = data.from || '';
      document.getElementById('letterBody').value = data.body || '';
      if (data.color) {
        state.color = data.color;
        document.querySelectorAll('#colorRow .color-swatch').forEach(s=>{
          s.classList.toggle('selected', s.dataset.color===data.color);
        });
      }
      if (data.bg) {
        state.bg = data.bg;
        document.querySelectorAll('#bgRow .color-swatch').forEach(s=>{
          s.classList.toggle('selected', s.dataset.bg===data.bg);
        });
      }
      syncPreview();
      showTab('preview-panel');
    } catch(e) {}
  }

  // ---- TOAST ----
  function showToast(msg) {
    const t = document.getElementById('toast');
    t.textContent = msg; t.classList.add('show');
    setTimeout(()=>t.classList.remove('show'), 2600);
  }

  // ---- INIT ----
  loadFromUrl();
  syncPreview();
</script>
</body>
</html>
