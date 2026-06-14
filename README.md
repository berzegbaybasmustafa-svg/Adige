# Adige
Tünelmak otomasyonu
<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>TÜNEL BETON ROBOTU – SCADA v2.0</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Exo+2:wght@300;400;600;700;900&display=swap');

  :root {
    --bg:        #040810;
    --bg2:       #070e1a;
    --bg3:       #0b1628;
    --panel:     #0d1e35;
    --border:    #0f3060;
    --cyan:      #00e5ff;
    --cyan2:     #00b8d4;
    --green:     #00ff88;
    --green2:    #00c96a;
    --orange:    #ff6d00;
    --red:       #ff1744;
    --yellow:    #ffd600;
    --purple:    #d500f9;
    --text:      #cce8ff;
    --text-dim:  #4a7099;
    --mono:      'Share Tech Mono', monospace;
    --sans:      'Exo 2', sans-serif;
  }

  * { margin:0; padding:0; box-sizing:border-box; -webkit-tap-highlight-color:transparent; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--sans);
    font-size: 13px;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ── TOP BAR ── */
  #topbar {
    background: linear-gradient(90deg, #020814 0%, #081528 40%, #040d1c 100%);
    border-bottom: 1px solid var(--cyan);
    box-shadow: 0 0 20px rgba(0,229,255,.2);
    padding: 8px 12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: sticky; top:0; z-index:99;
  }
  #topbar .logo {
    font-family: var(--mono);
    font-size: 11px;
    color: var(--cyan);
    text-shadow: 0 0 10px var(--cyan);
    line-height: 1.3;
  }
  #topbar .logo span { color: var(--green); text-shadow: 0 0 8px var(--green); }
  #topbar .status-row { display:flex; gap:6px; align-items:center; }
  .badge {
    font-family: var(--mono);
    font-size: 10px;
    padding: 2px 7px;
    border-radius: 3px;
    border: 1px solid;
    text-transform:uppercase;
  }
  .badge-online  { color:var(--green);  border-color:var(--green);  background:rgba(0,255,136,.08); box-shadow:0 0 8px rgba(0,255,136,.3); }
  .badge-sys     { color:var(--cyan);   border-color:var(--cyan);   background:rgba(0,229,255,.08); }
  #clock { font-family:var(--mono); font-size:11px; color:var(--yellow); text-shadow:0 0 8px rgba(255,214,0,.5); }

  /* ── NAV TABS ── */
  #navtabs {
    display: flex;
    overflow-x: auto;
    scrollbar-width:none;
    background: var(--bg2);
    border-bottom: 1px solid var(--border);
    gap:0;
  }
  #navtabs::-webkit-scrollbar { display:none; }
  .tab {
    flex-shrink:0;
    padding: 9px 14px;
    font-size: 11px;
    font-weight: 600;
    font-family: var(--mono);
    color: var(--text-dim);
    cursor: pointer;
    border-bottom: 2px solid transparent;
    white-space:nowrap;
    transition: all .2s;
    letter-spacing:.5px;
  }
  .tab.active { color:var(--cyan); border-bottom-color:var(--cyan); background:rgba(0,229,255,.05); text-shadow:0 0 8px rgba(0,229,255,.5); }
  .tab:hover:not(.active) { color:var(--text); background:rgba(255,255,255,.03); }

  /* ── SECTIONS ── */
  .section { display:none; padding:10px; animation: fadeIn .25s ease; }
  .section.active { display:block; }
  @keyframes fadeIn { from{opacity:0;transform:translateY(4px)} to{opacity:1;transform:none} }

  /* ── GRID ── */
  .grid-2 { display:grid; grid-template-columns:1fr 1fr; gap:8px; margin-bottom:8px; }
  .grid-3 { display:grid; grid-template-columns:1fr 1fr 1fr; gap:8px; margin-bottom:8px; }
  .grid-1 { margin-bottom:8px; }

  /* ── PANEL ── */
  .panel {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 10px;
    position: relative;
    overflow: hidden;
  }
  .panel::before {
    content:'';
    position:absolute; top:0; left:0; right:0; height:1px;
    background: linear-gradient(90deg, transparent, var(--cyan), transparent);
    opacity:.4;
  }
  .panel.green-accent::before { background: linear-gradient(90deg, transparent, var(--green), transparent); }
  .panel.orange-accent::before { background: linear-gradient(90deg, transparent, var(--orange), transparent); }
  .panel.purple-accent::before { background: linear-gradient(90deg, transparent, var(--purple), transparent); }

  .panel-title {
    font-family: var(--mono);
    font-size: 10px;
    color: var(--text-dim);
    text-transform: uppercase;
    letter-spacing: 1.5px;
    margin-bottom: 8px;
    display:flex;
    align-items:center;
    gap:5px;
  }
  .panel-title .dot {
    width:5px; height:5px; border-radius:50%;
    background:var(--cyan);
    box-shadow:0 0 6px var(--cyan);
    animation: pulse 2s infinite;
  }
  .panel-title .dot.green  { background:var(--green);  box-shadow:0 0 6px var(--green); }
  .panel-title .dot.orange { background:var(--orange); box-shadow:0 0 6px var(--orange); }
  .panel-title .dot.purple { background:var(--purple); box-shadow:0 0 6px var(--purple); }
  @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:.3} }

  /* ── BIG VALUE ── */
  .big-val {
    font-family: var(--mono);
    font-size: 26px;
    font-weight: 700;
    color: var(--cyan);
    text-shadow: 0 0 15px rgba(0,229,255,.6);
    line-height:1;
  }
  .big-val.green  { color:var(--green);  text-shadow:0 0 15px rgba(0,255,136,.6); }
  .big-val.orange { color:var(--orange); text-shadow:0 0 15px rgba(255,109,0,.6); }
  .big-val.yellow { color:var(--yellow); text-shadow:0 0 15px rgba(255,214,0,.6); }
  .big-val.red    { color:var(--red);    text-shadow:0 0 15px rgba(255,23,68,.6); }
  .val-unit { font-size:11px; color:var(--text-dim); margin-left:3px; }
  .val-label { font-size:10px; color:var(--text-dim); margin-top:3px; text-transform:uppercase; letter-spacing:1px; }

  /* ── GAUGE BAR ── */
  .gauge-wrap { margin:6px 0; }
  .gauge-label { display:flex; justify-content:space-between; font-size:10px; color:var(--text-dim); font-family:var(--mono); margin-bottom:3px; }
  .gauge-track { height:6px; background:rgba(255,255,255,.07); border-radius:4px; overflow:hidden; }
  .gauge-fill  { height:100%; border-radius:4px; transition: width .8s cubic-bezier(.4,0,.2,1); position:relative; }
  .gauge-fill.cyan   { background: linear-gradient(90deg, var(--cyan2), var(--cyan)); box-shadow:0 0 8px rgba(0,229,255,.4); }
  .gauge-fill.green  { background: linear-gradient(90deg, var(--green2),var(--green)); box-shadow:0 0 8px rgba(0,255,136,.4); }
  .gauge-fill.orange { background: linear-gradient(90deg, #e65100, var(--orange)); box-shadow:0 0 8px rgba(255,109,0,.4); }
  .gauge-fill.red    { background: linear-gradient(90deg, #b71c1c, var(--red)); box-shadow:0 0 8px rgba(255,23,68,.4); }
  .gauge-fill.yellow { background: linear-gradient(90deg, #f57f17, var(--yellow)); box-shadow:0 0 8px rgba(255,214,0,.4); }
  .gauge-fill.purple { background: linear-gradient(90deg, #7b1fa2, var(--purple)); box-shadow:0 0 8px rgba(213,0,249,.4); }

  /* ── TOGGLE SWITCH ── */
  .ctrl-row { display:flex; align-items:center; justify-content:space-between; margin:5px 0; }
  .ctrl-label { font-size:11px; color:var(--text); }
  .toggle { position:relative; width:38px; height:20px; }
  .toggle input { display:none; }
  .toggle-slider {
    position:absolute; inset:0;
    background:rgba(255,255,255,.1);
    border-radius:10px;
    border:1px solid var(--border);
    cursor:pointer;
    transition:.3s;
  }
  .toggle-slider::after {
    content:'';
    position:absolute;
    left:3px; top:2px;
    width:14px; height:14px;
    border-radius:50%;
    background:var(--text-dim);
    transition:.3s;
  }
  .toggle input:checked + .toggle-slider { background:rgba(0,255,136,.2); border-color:var(--green); }
  .toggle input:checked + .toggle-slider::after { transform:translateX(18px); background:var(--green); box-shadow:0 0 6px var(--green); }

  /* ── ALARM LIST ── */
  .alarm-item {
    display:flex; align-items:center; gap:8px;
    padding:6px 8px; margin:4px 0;
    border-radius:4px;
    font-size:11px;
    font-family:var(--mono);
    border: 1px solid;
  }
  .alarm-item.alarm-high   { background:rgba(255,23,68,.1);   border-color:rgba(255,23,68,.3);   color:var(--red); }
  .alarm-item.alarm-mid    { background:rgba(255,109,0,.1);  border-color:rgba(255,109,0,.3);  color:var(--orange); }
  .alarm-item.alarm-low    { background:rgba(255,214,0,.1);  border-color:rgba(255,214,0,.3);  color:var(--yellow); }
  .alarm-item.alarm-ok     { background:rgba(0,255,136,.07); border-color:rgba(0,255,136,.2);  color:var(--green); }
  .alarm-icon { font-size:14px; }
  .alarm-time { color:var(--text-dim); font-size:10px; }

  /* ── TUNNEL SVG SECTION ── */
  #tunnel-viz {
    width:100%; aspect-ratio:2/1;
    background: radial-gradient(ellipse at 50% 30%, #071828 0%, #020810 70%);
    border-radius:6px;
    border:1px solid var(--border);
    overflow:hidden;
    position:relative;
    margin-bottom:8px;
  }
  #tunnel-viz svg { width:100%; height:100%; }

  /* ── ARM ANIMATION ── */
  .arm-seg { transition: all 1s ease; }

  /* ── SPRAY PARTICLE ── */
  @keyframes spray {
    0%  { opacity:.9; transform:translate(0,0)         scale(1); }
    100%{ opacity:0;  transform:translate(var(--dx),var(--dy)) scale(0.3); }
  }
  .spray-dot {
    position:absolute;
    border-radius:50%;
    background:var(--cyan);
    box-shadow:0 0 4px var(--cyan);
    animation: spray var(--dur) linear infinite;
    animation-delay: var(--del);
  }

  /* ── PUMP ANIMATION ── */
  @keyframes pump {
    0%,100% { transform:scaleX(1); }
    50%     { transform:scaleX(.85); }
  }

  /* ── DATA TABLE ── */
  .data-table { width:100%; border-collapse:collapse; }
  .data-table tr { border-bottom:1px solid rgba(15,48,96,.6); }
  .data-table tr:last-child { border:none; }
  .data-table td { padding:5px 3px; font-size:11px; }
  .data-table td:first-child { color:var(--text-dim); font-family:var(--mono); font-size:10px; text-transform:uppercase; }
  .data-table td:last-child  { color:var(--cyan); font-family:var(--mono); text-align:right; }

  /* ── CIRCLE GAUGE ── */
  .circle-gauge { display:flex; flex-direction:column; align-items:center; }
  .cg-svg { width:72px; height:72px; }
  .cg-track { fill:none; stroke:rgba(255,255,255,.07); stroke-width:6; }
  .cg-fill  { fill:none; stroke-width:6; stroke-linecap:round;
    stroke-dasharray:175.9; stroke-dashoffset:175.9;
    transform:rotate(-90deg); transform-origin:50% 50%;
    transition: stroke-dashoffset 1s cubic-bezier(.4,0,.2,1);
  }
  .cg-text { font-family:var(--mono); font-size:12px; dominant-baseline:middle; text-anchor:middle; }
  .cg-sub  { font-family:var(--mono); font-size:8px;  dominant-baseline:middle; text-anchor:middle; fill:var(--text-dim); }
  .cg-label { font-size:10px; color:var(--text-dim); margin-top:3px; text-transform:uppercase; letter-spacing:1px; text-align:center; }

  /* ── BTN ── */
  .btn {
    padding:8px 14px; border-radius:4px; border:1px solid;
    font-family:var(--mono); font-size:11px; cursor:pointer;
    text-transform:uppercase; letter-spacing:1px;
    transition:all .2s; background:transparent;
  }
  .btn-cyan   { color:var(--cyan);   border-color:var(--cyan);   }
  .btn-cyan:hover   { background:rgba(0,229,255,.15); box-shadow:0 0 12px rgba(0,229,255,.3); }
  .btn-green  { color:var(--green);  border-color:var(--green);  }
  .btn-green:hover  { background:rgba(0,255,136,.15); box-shadow:0 0 12px rgba(0,255,136,.3); }
  .btn-red    { color:var(--red);    border-color:var(--red);    }
  .btn-red:hover    { background:rgba(255,23,68,.15);  box-shadow:0 0 12px rgba(255,23,68,.3); }
  .btn-orange { color:var(--orange); border-color:var(--orange); }
  .btn-orange:hover { background:rgba(255,109,0,.15);  box-shadow:0 0 12px rgba(255,109,0,.3); }
  .btn-row { display:flex; gap:6px; flex-wrap:wrap; margin-top:8px; }

  /* ── MODE SELECTOR ── */
  .mode-btn {
    flex:1; padding:8px 4px;
    border-radius:4px; border:1px solid var(--border);
    background:transparent; color:var(--text-dim);
    font-family:var(--mono); font-size:10px; cursor:pointer;
    text-align:center; transition:all .2s; text-transform:uppercase;
  }
  .mode-btn.active { border-color:var(--cyan); color:var(--cyan); background:rgba(0,229,255,.1); box-shadow:0 0 10px rgba(0,229,255,.2); }

  /* ── SEPARATOR ── */
  .sep { border:none; border-top:1px solid var(--border); margin:8px 0; }

  /* scrollbar */
  ::-webkit-scrollbar { width:4px; height:4px; }
  ::-webkit-scrollbar-track { background:var(--bg); }
  ::-webkit-scrollbar-thumb { background:var(--border); border-radius:2px; }
  ::-webkit-scrollbar-thumb:hover { background:var(--cyan2); }

  /* ── PROGRESS RING inside panel ── */
  .mini-ring-row { display:grid; grid-template-columns:repeat(3,1fr); gap:6px; }

  /* safety LED */
  .led-row { display:flex; flex-wrap:wrap; gap:6px; margin:6px 0; }
  .led {
    display:flex; align-items:center; gap:4px;
    font-size:10px; font-family:var(--mono);
    padding:3px 7px; border-radius:3px;
    border:1px solid rgba(255,255,255,.08);
    background:rgba(0,0,0,.3);
  }
  .led-dot { width:7px; height:7px; border-radius:50%; }
  .led-on-green  { background:var(--green);  box-shadow:0 0 6px var(--green); }
  .led-on-red    { background:var(--red);    box-shadow:0 0 6px var(--red); }
  .led-on-yellow { background:var(--yellow); box-shadow:0 0 6px var(--yellow); }
  .led-on-cyan   { background:var(--cyan);   box-shadow:0 0 6px var(--cyan); }
  .led-off       { background:rgba(255,255,255,.1); }

  /* logbook */
  .log-entry {
    border-left:2px solid var(--border);
    padding:4px 8px;
    margin-bottom:5px;
    font-family:var(--mono);
    font-size:10px;
  }
  .log-entry.cyan   { border-color:var(--cyan); }
  .log-entry.green  { border-color:var(--green); }
  .log-entry.orange { border-color:var(--orange); }
  .log-entry.red    { border-color:var(--red); }
  .log-time { color:var(--text-dim); }
  .log-msg  { color:var(--text); }
</style>
</head>
<body>

<!-- TOP BAR -->
<div id="topbar">
  <div class="logo">
    ANKA GLC EX<br>
    <span>TÜNELMAk / TİTAN IS26</span>
  </div>
  <div class="status-row">
    <div class="badge badge-online">● ÇEVRİMİÇİ</div>
    <div class="badge badge-sys">SYS OK</div>
  </div>
  <div id="clock">--:--:--</div>
</div>

<!-- NAV TABS -->
<div id="navtabs">
  <div class="tab active" onclick="switchTab('overview')">GENEL</div>
  <div class="tab" onclick="switchTab('manipulator')">MANİPÜLATÖR</div>
  <div class="tab" onclick="switchTab('pump')">POMPA</div>
  <div class="tab" onclick="switchTab('hydraulic')">HİDROLİK</div>
  <div class="tab" onclick="switchTab('safety')">GÜVENLİK</div>
  <div class="tab" onclick="switchTab('log')">LOG</div>
</div>

<!-- ═══════════════════════════════════ GENEL ════════════════════════════════════ -->
<div class="section active" id="sec-overview">

  <!-- Tünel Görselleştirme -->
  <div id="tunnel-viz">
    <svg viewBox="0 0 400 200" xmlns="http://www.w3.org/2000/svg">
      <defs>
        <radialGradient id="tunnelGrad" cx="50%" cy="50%" r="55%">
          <stop offset="0%" stop-color="#0d2a4a"/>
          <stop offset="100%" stop-color="#020810"/>
        </radialGradient>
        <filter id="glow">
          <feGaussianBlur stdDeviation="2.5" result="blur"/>
          <feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge>
        </filter>
        <filter id="glow2">
          <feGaussianBlur stdDeviation="4" result="blur"/>
          <feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge>
        </filter>
        <!-- spray gradient -->
        <radialGradient id="sprayGrad" cx="50%" cy="50%" r="50%">
          <stop offset="0%" stop-color="#00e5ff" stop-opacity="0.9"/>
          <stop offset="100%" stop-color="#00e5ff" stop-opacity="0"/>
        </radialGradient>
      </defs>

      <!-- Tunnel background -->
      <ellipse cx="200" cy="100" rx="190" ry="90" fill="url(#tunnelGrad)"/>

      <!-- Tunnel arch outline -->
      <path d="M 15 180 Q 15 20 200 20 Q 385 20 385 180" fill="none" stroke="#1a4a7a" stroke-width="2.5" opacity="0.7"/>
      <path d="M 25 180 Q 25 30 200 30 Q 375 30 375 180" fill="none" stroke="#0f3060" stroke-width="1" opacity="0.5"/>

      <!-- Depth lines -->
      <line x1="15" y1="180" x2="385" y2="180" stroke="#0f3060" stroke-width="1"/>
      <line x1="15" y1="185" x2="385" y2="185" stroke="#0f3060" stroke-width=".5" opacity="0.4"/>

      <!-- Concrete layer on arch (animated) -->
      <path id="concreteLayer" d="M 20 180 Q 20 25 200 25 Q 380 25 380 180" fill="none" stroke="#00e5ff" stroke-width="4" opacity="0.15" stroke-dasharray="10 5">
        <animate attributeName="stroke-dashoffset" from="0" to="-300" dur="3s" repeatCount="indefinite"/>
      </path>

      <!-- Chassis / machine body -->
      <rect x="120" y="150" width="110" height="28" rx="4" fill="#0d2240" stroke="#1a4a7a" stroke-width="1.5"/>
      <rect x="128" y="142" width="55" height="14" rx="3" fill="#0b1e38" stroke="#1a3a5c"/>
      <!-- wheels -->
      <circle cx="140" cy="180" r="9" fill="#0a1828" stroke="#1a4a7a" stroke-width="2"/>
      <circle cx="140" cy="180" r="4" fill="#0f3060"/>
      <circle cx="210" cy="180" r="9" fill="#0a1828" stroke="#1a4a7a" stroke-width="2"/>
      <circle cx="210" cy="180" r="4" fill="#0f3060"/>

      <!-- Stabilizers -->
      <rect x="110" y="172" width="12" height="8" rx="1" fill="#0d2240" stroke="#1a4a7a"/>
      <rect x="228" y="172" width="12" height="8" rx="1" fill="#0d2240" stroke="#1a4a7a"/>

      <!-- Manipulator arm - animated -->
      <g id="manipArm" filter="url(#glow)">
        <!-- Base joint -->
        <circle cx="175" cy="148" r="5" fill="#00e5ff" opacity="0.8"/>
        <!-- Arm segment 1 -->
        <line id="armSeg1" x1="175" y1="148" x2="155" y2="95" stroke="#00e5ff" stroke-width="3" stroke-linecap="round" opacity="0.9">
          <animateTransform attributeName="transform" type="rotate" values="0 175 148;3 175 148;-3 175 148;0 175 148" dur="4s" repeatCount="indefinite"/>
        </line>
        <circle cx="155" cy="95" r="4" fill="#00e5ff" opacity="0.7">
          <animateTransform attributeName="transform" type="rotate" values="0 175 148;3 175 148;-3 175 148;0 175 148" dur="4s" repeatCount="indefinite"/>
        </circle>
        <!-- Arm segment 2 -->
        <line id="armSeg2" x1="155" y1="95" x2="125" y2="58" stroke="#00b8d4" stroke-width="2.5" stroke-linecap="round" opacity="0.85">
          <animateTransform attributeName="transform" type="rotate" values="0 175 148;3 175 148;-3 175 148;0 175 148" dur="4s" repeatCount="indefinite"/>
        </line>
        <circle cx="125" cy="58" r="3" fill="#00b8d4" opacity="0.7">
          <animateTransform attributeName="transform" type="rotate" values="0 175 148;3 175 148;-3 175 148;0 175 148" dur="4s" repeatCount="indefinite"/>
        </circle>
        <!-- Nozzle -->
        <line x1="125" y1="58" x2="112" y2="42" stroke="#00ff88" stroke-width="2" stroke-linecap="round" opacity="0.9">
          <animateTransform attributeName="transform" type="rotate" values="0 175 148;3 175 148;-3 175 148;0 175 148" dur="4s" repeatCount="indefinite"/>
        </line>
        <circle cx="112" cy="42" r="4" fill="#00ff88" opacity="0.9" filter="url(#glow2)">
          <animateTransform attributeName="transform" type="rotate" values="0 175 148;3 175 148;-3 175 148;0 175 148" dur="4s" repeatCount="indefinite"/>
        </circle>
      </g>

      <!-- Spray cone -->
      <g id="sprayCone" opacity="0.7">
        <path d="M 112 42 L 78 22 L 92 35 Z" fill="url(#sprayGrad)">
          <animate attributeName="opacity" values="0.7;1;0.7" dur="0.4s" repeatCount="indefinite"/>
        </path>
        <!-- spray particles -->
        <circle r="1.5" fill="#00e5ff">
          <animateMotion path="M112,42 Q90,30 72,18" dur="0.5s" repeatCount="indefinite"/>
          <animate attributeName="opacity" values="1;0" dur="0.5s" repeatCount="indefinite"/>
        </circle>
        <circle r="1" fill="#00ff88">
          <animateMotion path="M112,42 Q88,28 70,22" dur="0.6s" repeatCount="indefinite" begin="0.2s"/>
          <animate attributeName="opacity" values="1;0" dur="0.6s" repeatCount="indefinite" begin="0.2s"/>
        </circle>
        <circle r="1.5" fill="#00e5ff">
          <animateMotion path="M112,42 Q95,33 78,28" dur="0.55s" repeatCount="indefinite" begin="0.3s"/>
          <animate attributeName="opacity" values="1;0" dur="0.55s" repeatCount="indefinite" begin="0.3s"/>
        </circle>
      </g>

      <!-- Labels -->
      <text x="175" y="130" fill="#4a7099" font-size="7" font-family="monospace" text-anchor="middle">TITAN IS26</text>
      <text x="112" y="72" fill="#00e5ff" font-size="6.5" font-family="monospace" filter="url(#glow)">NOZZLE</text>
      <text x="340" y="108" fill="#4a7099" font-size="7" font-family="monospace" text-anchor="middle">TÜNEL KESİTİ</text>
      <text x="340" y="118" fill="#4a7099" font-size="6" font-family="monospace" text-anchor="middle">16–130 m²</text>

      <!-- Distance line -->
      <line x1="112" y1="42" x2="35" y2="42" stroke="#ffd600" stroke-width=".8" stroke-dasharray="3 2" opacity="0.6"/>
      <text x="70" y="38" fill="#ffd600" font-size="6.5" font-family="monospace" opacity="0.8">1.4m</text>

      <!-- Concrete thickness indicator -->
      <rect x="28" y="55" width="6" height="18" rx="1" fill="#00e5ff" opacity="0.4"/>
      <text x="36" y="65" fill="#00e5ff" font-size="6" font-family="monospace" opacity="0.7">25cm</text>
    </svg>
  </div>

  <!-- 3 big KPI -->
  <div class="grid-3">
    <div class="panel green-accent">
      <div class="panel-title"><div class="dot green"></div>DEBI</div>
      <div class="big-val green" id="flowRate">22.4<span class="val-unit">m³/h</span></div>
      <div class="val-label">Beton Akışı</div>
    </div>
    <div class="panel">
      <div class="panel-title"><div class="dot"></div>HİD. BASINÇ</div>
      <div class="big-val" id="hydPressure">187<span class="val-unit">bar</span></div>
      <div class="val-label">Hidrolik</div>
    </div>
    <div class="panel orange-accent">
      <div class="panel-title"><div class="dot orange"></div>HIZLANDIR.</div>
      <div class="big-val orange" id="accelDose">5.2<span class="val-unit">%</span></div>
      <div class="val-label">Katkı Oranı</div>
    </div>
  </div>

  <!-- İşlem modu -->
  <div class="panel" style="margin-bottom:8px;">
    <div class="panel-title"><div class="dot"></div>ÇALIŞMA MODU</div>
    <div style="display:flex; gap:6px;">
      <button class="mode-btn active" id="mode-manuel" onclick="setMode('manuel')">MANUEL</button>
      <button class="mode-btn" id="mode-yari" onclick="setMode('yari')">YARI OTOMATİK</button>
      <button class="mode-btn" id="mode-oto" onclick="setMode('oto')">OTOMATİK</button>
    </div>
  </div>

  <!-- Gauges row -->
  <div class="grid-2">
    <div class="panel">
      <div class="panel-title"><div class="dot"></div>KOMPRESÖR</div>
      <div class="gauge-wrap">
        <div class="gauge-label"><span>Hava Akışı</span><span id="airFlow">8.7 m³/dak</span></div>
        <div class="gauge-track"><div class="gauge-fill cyan" id="airBar" style="width:85%"></div></div>
      </div>
      <div class="gauge-wrap">
        <div class="gauge-label"><span>Basınç</span><span id="airPres">5.4 bar</span></div>
        <div class="gauge-track"><div class="gauge-fill green" id="airPresBar" style="width:90%"></div></div>
      </div>
    </div>
    <div class="panel">
      <div class="panel-title"><div class="dot green"></div>KATKIMADDE</div>
      <div class="big-val green" style="font-size:20px;" id="addTank">730<span class="val-unit">L</span></div>
      <div class="val-label">Tank – 1000L kapasiteli</div>
      <div class="gauge-wrap" style="margin-top:6px;">
        <div class="gauge-label"><span>Doluluk</span><span>73%</span></div>
        <div class="gauge-track"><div class="gauge-fill green" style="width:73%"></div></div>
      </div>
    </div>
  </div>

  <!-- Beton kalınlık ve ilerleme -->
  <div class="panel green-accent" style="margin-bottom:8px;">
    <div class="panel-title"><div class="dot green"></div>BETON TABAKASI – MEVCUT KESİT</div>
    <div class="grid-2">
      <div>
        <div class="gauge-wrap">
          <div class="gauge-label"><span>Tavan</span><span id="thickTop">23 cm</span></div>
          <div class="gauge-track"><div class="gauge-fill cyan" id="thickTopBar" style="width:77%"></div></div>
        </div>
        <div class="gauge-wrap">
          <div class="gauge-label"><span>Sol Yan</span><span id="thickLeft">25 cm</span></div>
          <div class="gauge-track"><div class="gauge-fill green" id="thickLeftBar" style="width:83%"></div></div>
        </div>
      </div>
      <div>
        <div class="gauge-wrap">
          <div class="gauge-label"><span>Sağ Yan</span><span id="thickRight">22 cm</span></div>
          <div class="gauge-track"><div class="gauge-fill cyan" id="thickRightBar" style="width:73%"></div></div>
        </div>
        <div class="gauge-wrap">
          <div class="gauge-label"><span>Hedef</span><span>30 cm</span></div>
          <div class="gauge-track"><div class="gauge-fill yellow" style="width:100%"></div></div>
        </div>
      </div>
    </div>
  </div>

  <!-- Acil Stop -->
  <div style="display:flex; gap:8px; margin-bottom:8px;">
    <button class="btn btn-red" style="flex:1; padding:12px; font-size:13px;" onclick="emergencyStop()">⬛ ACİL DURDUR</button>
    <button class="btn btn-green" style="flex:1; padding:12px; font-size:13px;" onclick="resumeOp()">▶ DEVAM</button>
  </div>

</div>

<!-- ═══════════════════════════════ MANİPÜLATÖR ═════════════════════════════════ -->
<div class="section" id="sec-manipulator">

  <div class="panel" style="margin-bottom:8px;">
    <div class="panel-title"><div class="dot"></div>MANİPÜLATÖR POZİSYONU</div>
    <table class="data-table" style="width:100%">
      <tr><td>Dikey Uzanım</td><td id="vReach">9.2 <span style="color:var(--text-dim)">m</span></td></tr>
      <tr><td>Yatay Uzanım</td><td id="hReach">7.0 <span style="color:var(--text-dim)">m</span></td></tr>
      <tr><td>Eklem 1 (Omuz)</td><td id="joint1">42.3 °</td></tr>
      <tr><td>Eklem 2 (Dirsek)</td><td id="joint2">67.8 °</td></tr>
      <tr><td>Eklem 3 (Bilek)</td><td id="joint3">-12.1 °</td></tr>
      <tr><td>Nozzle Rotasyonu</td><td id="nozzleRot">128 °</td></tr>
      <tr><td>Nozzle Tilt</td><td id="nozzleTilt">+8 °</td></tr>
      <tr><td>Salınım</td><td id="oscillation">4.5 °</td></tr>
    </table>
  </div>

  <!-- Eklem gauge'ları -->
  <div class="panel purple-accent" style="margin-bottom:8px;">
    <div class="panel-title"><div class="dot purple"></div>EKLEM YÜKLEME (% Maks. Moment)</div>
    <div class="gauge-wrap">
      <div class="gauge-label"><span>Omuz Eklemi</span><span id="j1Load">58%</span></div>
      <div class="gauge-track"><div class="gauge-fill purple" id="j1Bar" style="width:58%"></div></div>
    </div>
    <div class="gauge-wrap">
      <div class="gauge-label"><span>Dirsek Eklemi</span><span id="j2Load">42%</span></div>
      <div class="gauge-track"><div class="gauge-fill cyan" id="j2Bar" style="width:42%"></div></div>
    </div>
    <div class="gauge-wrap">
      <div class="gauge-label"><span>Bilek Eklemi</span><span id="j3Load">31%</span></div>
      <div class="gauge-track"><div class="gauge-fill green" id="j3Bar" style="width:31%"></div></div>
    </div>
  </div>

  <!-- Nozzle durum -->
  <div class="grid-2">
    <div class="panel">
      <div class="panel-title"><div class="dot green"></div>NOZZLE MESAFESİ</div>
      <div class="big-val green" id="nozzleDist">1.4<span class="val-unit">m</span></div>
      <div class="val-label">Duvara mesafe</div>
      <div class="gauge-wrap" style="margin-top:6px;">
        <div class="gauge-label"><span>1.0–2.0m</span><span>OPT.</span></div>
        <div class="gauge-track"><div class="gauge-fill green" style="width:40%"></div></div>
      </div>
    </div>
    <div class="panel">
      <div class="panel-title"><div class="dot"></div>PÜSKÜRTME AÇISI</div>
      <div class="big-val" id="sprayAngle">88°<span class="val-unit"></span></div>
      <div class="val-label">Dik açı (≈90° ideal)</div>
      <div class="gauge-wrap" style="margin-top:6px;">
        <div class="gauge-label"><span>Geri Sıçrama</span><span id="reboundRate">12%</span></div>
        <div class="gauge-track"><div class="gauge-fill orange" id="reboundBar" style="width:12%"></div></div>
      </div>
    </div>
  </div>

  <div class="panel" style="margin-top:8px;">
    <div class="panel-title"><div class="dot"></div>MANİPÜLATÖR KONTROL</div>
    <div class="grid-2">
      <div>
        <div class="ctrl-row"><span class="ctrl-label">Oto. Yatay Tarama</span>
          <label class="toggle"><input type="checkbox" id="autoScan" checked onchange="toggleCtrl(this,'autoScan')"><div class="toggle-slider"></div></label>
        </div>
        <div class="ctrl-row"><span class="ctrl-label">Açı Koruma</span>
          <label class="toggle"><input type="checkbox" id="angleGuard" checked onchange="toggleCtrl(this,'angleGuard')"><div class="toggle-slider"></div></label>
        </div>
      </div>
      <div>
        <div class="ctrl-row"><span class="ctrl-label">Mesafe Sensörü</span>
          <label class="toggle"><input type="checkbox" id="distSensor" checked onchange="toggleCtrl(this,'distSensor')"><div class="toggle-slider"></div></label>
        </div>
        <div class="ctrl-row"><span class="ctrl-label">Salınım Aktif</span>
          <label class="toggle"><input type="checkbox" id="oscillActive" onchange="toggleCtrl(this,'oscillActive')"><div class="toggle-slider"></div></label>
        </div>
      </div>
    </div>
    <div class="btn-row">
      <button class="btn btn-cyan" onclick="homePos()">🏠 HOME POZ.</button>
      <button class="btn btn-orange" onclick="parkPos()">⬇ PARK</button>
    </div>
  </div>
</div>

<!-- ═══════════════════════════════════ POMPA ═══════════════════════════════════ -->
<div class="section" id="sec-pump">

  <div class="panel green-accent" style="margin-bottom:8px;">
    <div class="panel-title"><div class="dot green"></div>TITAN HİD. ÇİFT PİSTON POMPASI</div>
    <!-- animated pump -->
    <div style="display:flex;align-items:center;justify-content:center; gap:10px; padding:10px 0;">
      <svg width="200" height="70" viewBox="0 0 200 70">
        <defs>
          <linearGradient id="pumpBodyGrad" x1="0" x2="1" y1="0" y2="0">
            <stop offset="0%" stop-color="#0d2a4a"/>
            <stop offset="100%" stop-color="#071828"/>
          </linearGradient>
        </defs>
        <!-- pump body -->
        <rect x="40" y="20" width="120" height="30" rx="4" fill="url(#pumpBodyGrad)" stroke="#1a4a7a" stroke-width="1.5"/>
        <!-- piston 1 -->
        <rect x="48" y="26" width="40" height="18" rx="2" fill="#0f3060" stroke="#00e5ff" stroke-width="1">
          <animate attributeName="x" values="48;38;48;58;48" dur="1s" repeatCount="indefinite"/>
        </rect>
        <text x="68" y="39" fill="#00e5ff" font-size="8" font-family="monospace" text-anchor="middle">P1</text>
        <!-- piston 2 -->
        <rect x="112" y="26" width="40" height="18" rx="2" fill="#0f3060" stroke="#00ff88" stroke-width="1">
          <animate attributeName="x" values="112;122;112;102;112" dur="1s" repeatCount="indefinite"/>
        </rect>
        <text x="132" y="39" fill="#00ff88" font-size="8" font-family="monospace" text-anchor="middle">P2</text>
        <!-- inlet/outlet lines -->
        <line x1="40" y1="35" x2="10" y2="35" stroke="#1a4a7a" stroke-width="3"/>
        <rect x="5" y="28" width="10" height="14" rx="2" fill="#0d2240" stroke="#1a4a7a"/>
        <text x="10" y="58" fill="#4a7099" font-size="7" font-family="monospace" text-anchor="middle">GİRİŞ</text>
        <line x1="160" y1="35" x2="190" y2="35" stroke="#00e5ff" stroke-width="3">
          <animate attributeName="stroke" values="#00e5ff;#00ff88;#00e5ff" dur="1s" repeatCount="indefinite"/>
        </line>
        <text x="190" y="58" fill="#4a7099" font-size="7" font-family="monospace" text-anchor="middle">ÇIKIŞ</text>
      </svg>
    </div>
    <table class="data-table">
      <tr><td>Max. Kapasite</td><td>34 m³/h</td></tr>
      <tr><td>Mevcut Debi</td><td id="pumpFlow">22.4 m³/h</td></tr>
      <tr><td>Beton Silindiri</td><td>180 × 1000 mm</td></tr>
      <tr><td>Max. Beton Basıncı</td><td>50 bar</td></tr>
      <tr><td>Mevcut Beton Basıncı</td><td id="concPressure">38 bar</td></tr>
      <tr><td>Max. Hid. Basınç</td><td>210 bar</td></tr>
      <tr><td>Mevcut Hid. Basınç</td><td id="pumpHidPres">187 bar</td></tr>
      <tr><td>Huni Hacmi</td><td>250 L</td></tr>
      <tr><td>Hat Çapı</td><td>5" → 4" → 3¼"</td></tr>
    </table>
  </div>

  <div class="grid-2">
    <div class="panel">
      <div class="panel-title"><div class="dot green"></div>POMPA HIZI</div>
      <div class="big-val green" id="pumpSpeed">66<span class="val-unit">%</span></div>
      <div class="gauge-wrap" style="margin-top:6px;">
        <div class="gauge-track"><div class="gauge-fill green" id="pumpSpeedBar" style="width:66%"></div></div>
      </div>
    </div>
    <div class="panel orange-accent">
      <div class="panel-title"><div class="dot orange"></div>HUNI SEVİYESİ</div>
      <div class="big-val orange" id="hopperLevel">68<span class="val-unit">%</span></div>
      <div class="gauge-wrap" style="margin-top:6px;">
        <div class="gauge-track"><div class="gauge-fill orange" id="hopperBar" style="width:68%"></div></div>
      </div>
    </div>
  </div>

  <div class="panel" style="margin-top:8px;">
    <div class="panel-title"><div class="dot"></div>POMPA KONTROL</div>
    <div class="ctrl-row"><span class="ctrl-label">Huni Vibrasyonu</span>
      <label class="toggle"><input type="checkbox" id="hopperVib" checked><div class="toggle-slider"></div></label>
    </div>
    <div class="ctrl-row"><span class="ctrl-label">Otomatik Hız Kontrolü</span>
      <label class="toggle"><input type="checkbox" id="autoSpeed" checked><div class="toggle-slider"></div></label>
    </div>
    <div class="ctrl-row"><span class="ctrl-label">Geri Yıkama</span>
      <label class="toggle"><input type="checkbox" id="backFlush"><div class="toggle-slider"></div></label>
    </div>
    <div class="btn-row">
      <button class="btn btn-green" onclick="pumpStart()">▶ POMPALA</button>
      <button class="btn btn-orange" onclick="pumpStop()">⏸ DURDUR</button>
      <button class="btn btn-cyan" onclick="pumpFlush()">💧 YIKA</button>
    </div>
  </div>
</div>

<!-- ═══════════════════════════════════ HİDROLİK ════════════════════════════════ -->
<div class="section" id="sec-hydraulic">

  <div class="mini-ring-row" style="margin-bottom:8px;">
    <!-- Circle gauges -->
    <div class="circle-gauge">
      <svg class="cg-svg" viewBox="0 0 72 72">
        <circle class="cg-track" cx="36" cy="36" r="28"/>
        <circle class="cg-fill" cx="36" cy="36" r="28" stroke="#00e5ff" id="cgHidPres" style="stroke-dashoffset:44.8"/>
        <text class="cg-text" x="36" y="34" fill="#00e5ff" id="cgHidPresVal">187</text>
        <text class="cg-sub" x="36" y="45">bar</text>
      </svg>
      <div class="cg-label">Hid. Basınç</div>
    </div>
    <div class="circle-gauge">
      <svg class="cg-svg" viewBox="0 0 72 72">
        <circle class="cg-track" cx="36" cy="36" r="28"/>
        <circle class="cg-fill" cx="36" cy="36" r="28" stroke="#00ff88" id="cgOilTemp" style="stroke-dashoffset:105.5"/>
        <text class="cg-text" x="36" y="34" fill="#00ff88" id="cgOilTempVal">52</text>
        <text class="cg-sub" x="36" y="45">°C</text>
      </svg>
      <div class="cg-label">Yağ Sıcaklığı</div>
    </div>
    <div class="circle-gauge">
      <svg class="cg-svg" viewBox="0 0 72 72">
        <circle class="cg-track" cx="36" cy="36" r="28"/>
        <circle class="cg-fill" cx="36" cy="36" r="28" stroke="#ff6d00" id="cgFlow" style="stroke-dashoffset:70.3"/>
        <text class="cg-text" x="36" y="34" fill="#ff6d00" id="cgFlowVal">200</text>
        <text class="cg-sub" x="36" y="45">L/dak</text>
      </svg>
      <div class="cg-label">Akış Hızı</div>
    </div>
  </div>

  <div class="panel" style="margin-bottom:8px;">
    <div class="panel-title"><div class="dot"></div>HİDROLİK SİSTEM PARAMETRELERİ</div>
    <table class="data-table">
      <tr><td>Pompa Tipi</td><td>Sabit + Değişken Deplasman</td></tr>
      <tr><td>Max. Basınç</td><td>210 bar</td></tr>
      <tr><td>Mevcut Basınç</td><td id="hidPresLive">187 bar</td></tr>
      <tr><td>Akış Hızı (1450rpm)</td><td>290 L/dak</td></tr>
      <tr><td>Mevcut Akış</td><td id="hidFlowLive">200 L/dak</td></tr>
      <tr><td>Yağ Tankı</td><td>325 L</td></tr>
      <tr><td>Dönüş Filtresi</td><td>20 µm</td></tr>
      <tr><td>Yağ Sıcaklığı</td><td id="hidTempLive">52 °C</td></tr>
      <tr><td>Yağ Seviyesi</td><td id="hidLevel">87 %</td></tr>
    </table>
  </div>

  <div class="grid-2">
    <div class="panel">
      <div class="panel-title"><div class="dot green"></div>FİLTRE DURUMU</div>
      <div class="gauge-wrap">
        <div class="gauge-label"><span>Dönüş Filtresi</span><span>82%</span></div>
        <div class="gauge-track"><div class="gauge-fill green" style="width:82%"></div></div>
      </div>
      <div class="gauge-wrap">
        <div class="gauge-label"><span>Emiş Filtresi</span><span>74%</span></div>
        <div class="gauge-track"><div class="gauge-fill cyan" style="width:74%"></div></div>
      </div>
    </div>
    <div class="panel orange-accent">
      <div class="panel-title"><div class="dot orange"></div>SOĞUTMA</div>
      <div class="big-val orange" id="coolerTemp">52<span class="val-unit">°C</span></div>
      <div class="val-label">Yağ sıcaklığı</div>
      <div style="margin-top:5px; font-size:10px; color:var(--green); font-family:var(--mono)">Max: 80°C ✓ NORMAL</div>
    </div>
  </div>

  <div class="panel" style="margin-top:8px;">
    <div class="panel-title"><div class="dot"></div>KONTROL VALFLERİ</div>
    <div class="ctrl-row"><span class="ctrl-label">Manipülatör Valf</span>
      <label class="toggle"><input type="checkbox" checked><div class="toggle-slider"></div></label>
    </div>
    <div class="ctrl-row"><span class="ctrl-label">Pompa Valf</span>
      <label class="toggle"><input type="checkbox" checked><div class="toggle-slider"></div></label>
    </div>
    <div class="ctrl-row"><span class="ctrl-label">Stabilizatör Valfleri</span>
      <label class="toggle"><input type="checkbox" checked><div class="toggle-slider"></div></label>
    </div>
    <div class="ctrl-row"><span class="ctrl-label">Soğutucu Fan</span>
      <label class="toggle"><input type="checkbox" checked><div class="toggle-slider"></div></label>
    </div>
  </div>
</div>

<!-- ═══════════════════════════════════ GÜVENLİK ═══════════════════════════════ -->
<div class="section" id="sec-safety">

  <div class="panel" style="margin-bottom:8px;">
    <div class="panel-title"><div class="dot green"></div>GÜVENLİK SİSTEMLERİ DURUMU</div>
    <div class="led-row">
      <div class="led"><div class="led-dot led-on-green"></div>E-STOP HAZIR</div>
      <div class="led"><div class="led-dot led-on-green"></div>ROPS/FOPS</div>
      <div class="led"><div class="led-dot led-on-green"></div>ATEX UYUMLU</div>
      <div class="led"><div class="led-dot led-on-cyan"></div>UZAK KUMANDA</div>
      <div class="led"><div class="led-dot led-on-green"></div>FREN SİSTEMİ</div>
      <div class="led"><div class="led-dot led-on-yellow"></div>ARKA KAMERA</div>
      <div class="led"><div class="led-dot led-on-green"></div>TOZ SENSÖRÜ</div>
      <div class="led"><div class="led-dot led-on-green"></div>EĞİM SENSÖRÜ</div>
    </div>
  </div>

  <!-- Aktif Alarmlar -->
  <div class="panel orange-accent" style="margin-bottom:8px;">
    <div class="panel-title"><div class="dot orange"></div>AKTİF ALARMLAR</div>
    <div class="alarm-item alarm-low alarm-icon">
      <span class="alarm-icon">⚠</span>
      <div style="flex:1">
        <div>Huni beton seviyesi düşük</div>
        <div class="alarm-time">09:42:18</div>
      </div>
      <span style="color:var(--yellow); font-size:10px;">DÜŞÜK</span>
    </div>
    <div class="alarm-item alarm-ok">
      <span class="alarm-icon">✓</span>
      <div style="flex:1">
        <div>Tüm sistem parametreleri normal</div>
        <div class="alarm-time">09:38:05</div>
      </div>
      <span style="font-size:10px;">OK</span>
    </div>
  </div>

  <!-- Fren & Direksiyon -->
  <div class="panel" style="margin-bottom:8px;">
    <div class="panel-title"><div class="dot"></div>ARAÇ KONTROL SİSTEMLERİ</div>
    <table class="data-table">
      <tr><td>Şanzıman</td><td>JCB PS760 Powershift</td></tr>
      <tr><td>İleri Vites</td><td>4 Otomatik / Manuel</td></tr>
      <tr><td>Geri Vites</td><td>3 Manuel</td></tr>
      <tr><td>Maks. Eğim</td><td>40%</td></tr>
      <tr><td>Maks. Hız</td><td>24.5 km/h</td></tr>
      <tr><td>Servis Freni</td><td>Çift devreli yaş disk</td></tr>
      <tr><td>Park Freni</td><td>SAHR (Fail-safe)</td></tr>
    </table>

    <hr class="sep">
    <div class="panel-title" style="margin-bottom:6px;"><div class="dot"></div>DİREKSİYON MODU</div>
    <div style="display:flex; gap:5px;">
      <button class="mode-btn active" id="steer-4ws" onclick="setSteering('4ws')">4 TEKL.</button>
      <button class="mode-btn" id="steer-fws" onclick="setSteering('fws')">ÖN TEKL.</button>
      <button class="mode-btn" id="steer-crab" onclick="setSteering('crab')">YENGEÇ</button>
    </div>
  </div>

  <!-- Çevre -->
  <div class="grid-2">
    <div class="panel">
      <div class="panel-title"><div class="dot green"></div>ÇEVRE MONİTÖR</div>
      <div class="gauge-wrap">
        <div class="gauge-label"><span>Toz (mg/m³)</span><span id="dustLevel">3.2</span></div>
        <div class="gauge-track"><div class="gauge-fill green" id="dustBar" style="width:16%"></div></div>
      </div>
      <div class="gauge-wrap">
        <div class="gauge-label"><span>CO (ppm)</span><span id="coLevel">12</span></div>
        <div class="gauge-track"><div class="gauge-fill cyan" id="coBar" style="width:6%"></div></div>
      </div>
      <div class="gauge-wrap">
        <div class="gauge-label"><span>Nem (%)</span><span id="humidity">68</span></div>
        <div class="gauge-track"><div class="gauge-fill yellow" style="width:68%"></div></div>
      </div>
    </div>
    <div class="panel">
      <div class="panel-title"><div class="dot"></div>ELEKTRİK SİSTEMİ</div>
      <table class="data-table">
        <tr><td>Akü</td><td>12V / 150Ah</td></tr>
        <tr><td>Akü Şarjı</td><td id="battLevel">87%</td></tr>
        <tr><td>Güç</td><td>400V / 50Hz</td></tr>
        <tr><td>IP Koruma</td><td>IP65</td></tr>
        <tr><td>Kablo Makarası</td><td>50m</td></tr>
      </table>
    </div>
  </div>
</div>

<!-- ═══════════════════════════════════ LOG ════════════════════════════════════ -->
<div class="section" id="sec-log">
  <div class="panel" style="margin-bottom:8px;">
    <div class="panel-title"><div class="dot"></div>SİSTEM KAYIT DEFTERİ</div>
    <div id="logContainer">
      <!-- filled by JS -->
    </div>
    <div class="btn-row" style="margin-top:8px;">
      <button class="btn btn-cyan" onclick="clearLog()">🗑 LOGU TEMİZLE</button>
      <button class="btn btn-green" onclick="exportLog()">⬇ DIŞA AKTAR</button>
    </div>
  </div>
</div>

<script>
// ── CLOCK ──
function updateClock() {
  const now = new Date();
  document.getElementById('clock').textContent =
    String(now.getHours()).padStart(2,'0') + ':' +
    String(now.getMinutes()).padStart(2,'0') + ':' +
    String(now.getSeconds()).padStart(2,'0');
}
setInterval(updateClock, 1000);
updateClock();

// ── TAB SWITCH ──
function switchTab(id) {
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
  event.target.classList.add('active');
  document.getElementById('sec-' + id).classList.add('active');
}

// ── MODE ──
function setMode(m) {
  ['manuel','yari','oto'].forEach(k => document.getElementById('mode-'+k).classList.remove('active'));
  document.getElementById('mode-'+m).classList.add('active');
  addLog('cyan', 'Çalışma modu değiştirildi: ' + m.toUpperCase());
}

// ── STEERING ──
function setSteering(m) {
  ['4ws','fws','crab'].forEach(k => document.getElementById('steer-'+k).classList.remove('active'));
  document.getElementById('steer-'+m).classList.add('active');
  addLog('cyan', 'Direksiyon modu: ' + m.toUpperCase());
}

// ── BUTTONS ──
function emergencyStop() {
  addLog('red','⬛ ACİL DURDURMA AKTIVE EDİLDİ!');
  document.querySelector('#topbar .badge-online').textContent = '● DURDURULDU';
  document.querySelector('#topbar .badge-online').style.color = 'var(--red)';
  document.querySelector('#topbar .badge-online').style.borderColor = 'var(--red)';
}
function resumeOp() {
  addLog('green','▶ Operasyon devam ettirildi.');
  document.querySelector('#topbar .badge-online').textContent = '● ÇEVRİMİÇİ';
  document.querySelector('#topbar .badge-online').style.color = 'var(--green)';
  document.querySelector('#topbar .badge-online').style.borderColor = 'var(--green)';
}
function homePos()  { addLog('cyan','Manipülatör HOME pozisyonuna gidiyor...'); }
function parkPos()  { addLog('orange','Manipülatör PARK pozisyonuna iniyor...'); }
function pumpStart(){ addLog('green','Beton pompası başlatıldı.'); }
function pumpStop() { addLog('orange','Beton pompası durduruldu.'); }
function pumpFlush(){ addLog('cyan','Pompa yıkama döngüsü başlatıldı.'); }
function clearLog() { document.getElementById('logContainer').innerHTML=''; }
function exportLog() { addLog('cyan','Log dosyası dışa aktarılıyor...'); }
function toggleCtrl(el, name) {
  addLog(el.checked?'green':'orange', name + (el.checked?' aktive edildi':' deaktive edildi') + '.');
}

// ── LOG ──
const logEntries = [
  {cls:'green',  msg:'Sistem başlatıldı – Titan IS26 bağlantısı kuruldu.'},
  {cls:'cyan',   msg:'Manipülatör kalibrasyonu tamamlandı.'},
  {cls:'cyan',   msg:'Uzak kumanda sinyal kalitesi: %94'},
  {cls:'green',  msg:'Beton pompası devreye alındı. Debi: 22.4 m³/h'},
  {cls:'orange', msg:'Huni beton seviyesi %68 – dolum gerekli.'},
  {cls:'cyan',   msg:'Otomatik yatay tarama aktif – nozzle salınıyor.'},
  {cls:'green',  msg:'Beton kalınlığı hedef aralığında: 22-25cm / 30cm hedef.'},
];
function addLog(cls, msg) {
  const t = new Date();
  const time = String(t.getHours()).padStart(2,'0')+':'+String(t.getMinutes()).padStart(2,'0')+':'+String(t.getSeconds()).padStart(2,'0');
  const div = document.createElement('div');
  div.className = 'log-entry ' + cls;
  div.innerHTML = `<span class="log-time">${time}</span> <span class="log-msg">${msg}</span>`;
  const c = document.getElementById('logContainer');
  c.insertBefore(div, c.firstChild);
}
logEntries.forEach(e => addLog(e.cls, e.msg));

// ── LIVE DATA SIMULATION ──
function rnd(base, variance) { return (base + (Math.random()*2-1)*variance).toFixed(1); }
function rndInt(base, variance) { return Math.round(base + (Math.random()*2-1)*variance); }

function updateLiveData() {
  // Genel
  const fr = parseFloat(rnd(22.4, 1.5));
  document.getElementById('flowRate').innerHTML = fr + '<span class="val-unit">m³/h</span>';
  document.getElementById('pumpFlow').textContent = fr + ' m³/h';

  const hp = rndInt(187, 8);
  document.getElementById('hydPressure').innerHTML = hp + '<span class="val-unit">bar</span>';
  document.getElementById('pumpHidPres').textContent = hp + ' bar';
  document.getElementById('hidPresLive').textContent = hp + ' bar';
  document.getElementById('cgHidPresVal').textContent = hp;
  const hpOff = 175.9 * (1 - hp/210);
  document.getElementById('cgHidPres').style.strokeDashoffset = hpOff;

  const acc = parseFloat(rnd(5.2, 0.3));
  document.getElementById('accelDose').innerHTML = acc + '<span class="val-unit">%</span>';

  const af = parseFloat(rnd(8.7, .4));
  document.getElementById('airFlow').textContent = af + ' m³/dak';
  document.getElementById('airBar').style.width = Math.min(100, af/10.2*100).toFixed(0) + '%';

  const addTk = rndInt(730, 5);
  document.getElementById('addTank').innerHTML = addTk + '<span class="val-unit">L</span>';

  // Thickness
  const tt = rndInt(23, 1);
  document.getElementById('thickTop').textContent = tt + ' cm';
  document.getElementById('thickTopBar').style.width = (tt/30*100).toFixed(0)+'%';
  const tl = rndInt(25, 1);
  document.getElementById('thickLeft').textContent = tl + ' cm';
  document.getElementById('thickLeftBar').style.width = (tl/30*100).toFixed(0)+'%';
  const tr2 = rndInt(22, 1);
  document.getElementById('thickRight').textContent = tr2 + ' cm';
  document.getElementById('thickRightBar').style.width = (tr2/30*100).toFixed(0)+'%';

  // Manipulatör
  const nd = parseFloat(rnd(1.4, .1));
  document.getElementById('nozzleDist').innerHTML = nd + '<span class="val-unit">m</span>';
  const sa = rndInt(88, 3);
  document.getElementById('sprayAngle').innerHTML = sa + '°';
  const rb = rndInt(12, 2);
  document.getElementById('reboundRate').textContent = rb + '%';
  document.getElementById('reboundBar').style.width = rb + '%';
  document.getElementById('joint1').textContent = rnd(42.3, 2) + ' °';
  document.getElementById('joint2').textContent = rnd(67.8, 2) + ' °';
  document.getElementById('joint3').textContent = rnd(-12.1, 1) + ' °';
  document.getElementById('nozzleRot').textContent = rndInt(128, 3) + ' °';

  const j1l = rndInt(58, 3);
  document.getElementById('j1Load').textContent = j1l + '%';
  document.getElementById('j1Bar').style.width = j1l + '%';
  const j2l = rndInt(42, 3);
  document.getElementById('j2Load').textContent = j2l + '%';
  document.getElementById('j2Bar').style.width = j2l + '%';

  // Pompa
  const ps = rndInt(66, 3);
  document.getElementById('pumpSpeed').innerHTML = ps + '<span class="val-unit">%</span>';
  document.getElementById('pumpSpeedBar').style.width = ps + '%';
  const hl = rndInt(68, 4);
  document.getElementById('hopperLevel').innerHTML = hl + '<span class="val-unit">%</span>';
  document.getElementById('hopperBar').style.width = hl + '%';
  const cp = rndInt(38, 3);
  document.getElementById('concPressure').textContent = cp + ' bar';

  // Hidrolik
  const ot = rndInt(52, 2);
  document.getElementById('cgOilTempVal').textContent = ot;
  document.getElementById('hidTempLive').textContent = ot + ' °C';
  document.getElementById('coolerTemp').innerHTML = ot + '<span class="val-unit">°C</span>';
  const otOff = 175.9 * (1 - ot/80);
  document.getElementById('cgOilTemp').style.strokeDashoffset = Math.max(0, otOff);

  const hfl = rndInt(200, 10);
  document.getElementById('cgFlowVal').textContent = hfl;
  document.getElementById('hidFlowLive').textContent = hfl + ' L/dak';
  const hflOff = 175.9 * (1 - hfl/290);
  document.getElementById('cgFlow').style.strokeDashoffset = Math.max(0, hflOff);

  // Safety env
  document.getElementById('dustLevel').textContent = rnd(3.2, 0.5);
  document.getElementById('coLevel').textContent = rndInt(12, 2);
  document.getElementById('humidity').textContent = rndInt(68, 3);
}

setInterval(updateLiveData, 1500);
updateLiveData();
</script>
</body>
</html>
