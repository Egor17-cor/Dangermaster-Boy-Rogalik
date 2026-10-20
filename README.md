<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Данжермастер Бой — 50 уровней (Изометрия)</title>
  <style>
    * {
      box-sizing: border-box;
      user-select: none;
      -webkit-tap-highlight-color: transparent;
    }
    body {
      margin: 0;
      min-height: 100vh;
      background: #0b0e14;
      display: flex;
      justify-content: center;
      align-items: center;
      font-family: 'Segoe UI', Roboto, system-ui, sans-serif;
      padding: 8px;
    }
    .game-wrapper {
      background: #1e2530;
      padding: 12px 12px 16px;
      border-radius: 24px;
      box-shadow: 0 15px 30px rgba(0, 0, 0, 0.7), inset 0 1px 2px rgba(255, 255, 255, 0.06);
      border: 1px solid #3d4756;
      width: 100%;
      max-width: 750px;
      position: relative;
    }
    .canvas-container {
      display: flex;
      justify-content: center;
      background: #151b23;
      padding: 6px;
      border-radius: 16px;
      box-shadow: inset 0 4px 8px rgba(0,0,0,0.6);
    }
    canvas {
      display: block;
      width: 100%;
      height: auto;
      aspect-ratio: 1/1;
      max-width: 650px;
      image-rendering: pixelated;
      background: #1a2530;
      border-radius: 12px;
      box-shadow: 0 0 0 1px #3a4658;
      cursor: pointer;
      touch-action: none;
    }
    .info-panel {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      align-items: center;
      margin-top: 10px;
      color: #c8d2e0;
      text-shadow: 0 2px 3px black;
      font-weight: 500;
      padding: 6px 8px;
      background: #151b23;
      border-radius: 40px;
      border: 1px solid #334052;
      gap: 4px;
    }
    .stats {
      display: flex;
      flex-wrap: wrap;
      gap: 3px 2px;
    }
    .stats span {
      background: #232d3b;
      padding: 3px 8px;
      border-radius: 30px;
      border: 1px solid #3a4a5e;
      box-shadow: inset 0 1px 3px #0b0e12;
      font-size: 0.65rem;
      white-space: nowrap;
    }
    .weapon-badge {
      background: #2d3f3a;
      border-color: #5a8a7a;
      color: #b6e6d0;
    }
    .controls {
      display: flex;
      flex-wrap: wrap;
      gap: 4px;
    }
    .btn {
      background: #2f3c4d;
      border: none;
      color: #eaf0fa;
      font-weight: bold;
      padding: 4px 10px;
      border-radius: 30px;
      font-size: 0.7rem;
      cursor: pointer;
      box-shadow: 0 4px 0 #0b1016, 0 3px 8px rgba(0,0,0,0.5);
      transition: 0.07s ease;
      border: 1px solid #4b5b70;
      touch-action: manipulation;
      white-space: nowrap;
    }
    .btn:active {
      transform: translateY(4px);
      box-shadow: 0 1px 0 #0b1016;
    }
    .btn.reset { background: #4d3a3a; border-color: #7a5252; }
    .btn.down { background: #2d4a3a; border-color: #4a8a5a; }
    .btn.auto { background: #3a2d4a; border-color: #6a4a8a; }
    .btn.auto.active { background: #4a3a2d; border-color: #8a6a4a; box-shadow: 0 0 15px #ffaa44; }
    .btn.inv { background: #2d3a4a; border-color: #4a6a8a; }
    .btn.heal { background: #2d4a3a; border-color: #4a8a5a; color: #b6e6d0; }
    .btn.heal:active { background: #3a5a4a; }
    .btn.story { background: #3a2d4a; border-color: #6a4a8a; }
    .btn.menu { background: #3a3a2d; border-color: #6a6a4a; }
    .btn.music { background: #2d3a4a; border-color: #4a6a8a; }
    .btn.music.muted { background: #3a2d2d; border-color: #6a4a4a; opacity: 0.6; }
    .message {
      background: #1b232e;
      padding: 3px 10px;
      border-radius: 40px;
      border-left: 4px solid #d9a066;
      color: #d3deed;
      min-width: 80px;
      text-align: center;
      font-size: 0.7rem;
      flex: 1 1 100%;
    }

    /* ---- КНОПОЧНОЕ УПРАВЛЕНИЕ ДЛЯ ТЕЛЕФОНОВ ---- */
    .dpad {
      display: none;
      justify-content: center;
      margin-top: 10px;
      gap: 4px;
      padding: 6px;
      background: #151b23;
      border-radius: 20px;
      border: 1px solid #334052;
    }
    .dpad .btn-dpad {
      background: #2f3c4d;
      border: 1px solid #4b5b70;
      color: #eaf0fa;
      font-size: 1.4rem;
      padding: 8px 14px;
      border-radius: 12px;
      cursor: pointer;
      box-shadow: 0 4px 0 #0b1016, 0 3px 8px rgba(0,0,0,0.5);
      transition: 0.07s ease;
      touch-action: manipulation;
      min-width: 48px;
      text-align: center;
      font-weight: bold;
      user-select: none;
    }
    .dpad .btn-dpad:active {
      transform: translateY(4px);
      box-shadow: 0 1px 0 #0b1016;
    }
    .dpad .btn-dpad.center {
      background: #3a2d4a;
      border-color: #6a4a8a;
      font-size: 0.8rem;
      min-width: 48px;
    }
    .dpad .btn-dpad.action {
      background: #2d4a3a;
      border-color: #4a8a5a;
    }
    .dpad .btn-dpad.action-heal {
      background: #4a2d3a;
      border-color: #8a4a5a;
    }
    .dpad .btn-dpad.action-inv {
      background: #2d3a4a;
      border-color: #4a6a8a;
    }

    .dpad-row {
      display: flex;
      gap: 4px;
      justify-content: center;
    }

    /* Показываем D-Pad только на мобильных устройствах */
    @media (max-width: 768px) {
      .dpad { display: flex; flex-wrap: wrap; gap: 4px; }
      .dpad .btn-dpad { padding: 10px 16px; min-width: 52px; font-size: 1.6rem; }
      .stats span { font-size: 0.6rem; padding: 2px 6px; }
      .btn { font-size: 0.6rem; padding: 3px 8px; }
      .message { font-size: 0.6rem; min-width: 60px; }
    }
    @media (max-width: 420px) {
      .info-panel { flex-direction: column; align-items: stretch; gap: 3px; }
      .controls { justify-content: center; }
      .stats { justify-content: center; }
      .dpad .btn-dpad { padding: 8px 12px; min-width: 42px; font-size: 1.3rem; }
    }
    @media (max-width: 360px) {
      .dpad .btn-dpad { padding: 6px 8px; min-width: 36px; font-size: 1rem; }
    }

    .inventory-overlay, .menu-overlay, .story-overlay {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      justify-content: center;
      align-items: center;
      z-index: 1000;
      backdrop-filter: blur(4px);
      padding: 12px;
    }
    .inventory-overlay.active, .menu-overlay.active, .story-overlay.active { display: flex; }
    .inventory-overlay { background: rgba(0,0,0,0.7); z-index: 1000; }
    .menu-overlay { background: rgba(0,0,0,0.85); z-index: 3000; backdrop-filter: blur(8px); }
    .story-overlay { background: rgba(0,0,0,0.8); z-index: 2000; backdrop-filter: blur(6px); }

    .inventory-window {
      background: #1e2530;
      border: 2px solid #4a5a7a;
      border-radius: 24px;
      padding: 20px 18px 24px;
      max-width: 480px;
      width: 100%;
      max-height: 80vh;
      box-shadow: 0 20px 60px rgba(0,0,0,0.9);
      color: #c8d2e0;
      overflow-y: auto;
    }
    .menu-window {
      background: #1e2530;
      border: 2px solid #8a7a4a;
      border-radius: 28px;
      padding: 35px 40px 40px;
      max-width: 520px;
      width: 100%;
      max-height: 90vh;
      overflow-y: auto;
      box-shadow: 0 20px 80px rgba(0,0,0,0.9);
      color: #d3deed;
      text-align: center;
    }
    .story-window {
      background: #1e2530;
      border: 2px solid #6a4a8a;
      border-radius: 24px;
      padding: 30px 35px 35px;
      max-width: 550px;
      width: 100%;
      max-height: 85vh;
      overflow-y: auto;
      box-shadow: 0 20px 60px rgba(0,0,0,0.9);
      color: #d3deed;
    }
    .inv-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 14px;
      border-bottom: 1px solid #3a4a5e;
      padding-bottom: 10px;
    }
    .inv-header h2 { margin: 0; font-weight: 400; font-size: 1.3rem; color: #eaf0fa; }
    .inv-close {
      background: #4d3a3a;
      border: none;
      color: #f0e6d2;
      font-size: 1.4rem;
      padding: 0 14px;
      border-radius: 30px;
      cursor: pointer;
      border: 1px solid #7a5252;
      box-shadow: 0 3px 0 #2a1a1a;
      transition: 0.07s;
      line-height: 1.6;
    }
    .inv-close:active { transform: translateY(3px); box-shadow: 0 0px 0 #2a1a1a; }
    .inv-items {
      display: flex;
      flex-direction: column;
      gap: 8px;
      max-height: 50vh;
      overflow-y: auto;
      padding-right: 4px;
    }
    .inv-items::-webkit-scrollbar { width: 6px; }
    .inv-items::-webkit-scrollbar-track { background: #151b23; border-radius: 10px; }
    .inv-items::-webkit-scrollbar-thumb { background: #3a4a5e; border-radius: 10px; }
    .inv-item {
      background: #232d3b;
      padding: 10px 14px;
      border-radius: 12px;
      border-left: 4px solid #6a8a9a;
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      align-items: center;
      border: 1px solid #3a4a5e;
      gap: 6px;
    }
    .inv-item .item-name { font-weight: 500; font-size: 0.9rem; }
    .inv-item .item-type { font-size: 0.7rem; opacity: 0.7; margin-left: 6px; }
    .inv-item .item-bonus {
      background: #2d4a3a;
      padding: 2px 10px;
      border-radius: 20px;
      font-size: 0.75rem;
      border: 1px solid #4a8a5a;
    }
    .inv-empty { text-align: center; opacity: 0.6; padding: 30px 0; font-style: italic; }
    .inv-item .item-use {
      background: #2d4a3a;
      border: 1px solid #4a8a5a;
      color: #b6e6d0;
      padding: 4px 12px;
      border-radius: 20px;
      cursor: pointer;
      font-weight: bold;
      transition: 0.1s;
      font-size: 0.75rem;
      touch-action: manipulation;
    }
    .inv-item .item-use:active { transform: scale(0.95); }
    .inv-item .item-use.heal { background: #4a2d3a; border-color: #8a4a5a; color: #f0b6c8; }
    .inv-item .item-use.spell { background: #2d2d4a; border-color: #5a5a8a; color: #b6b6f0; }

    .menu-window h1 {
      font-size: 2.2rem;
      color: #d4af37;
      text-shadow: 0 0 30px rgba(180, 130, 50, 0.3);
      margin: 0 0 8px 0;
      letter-spacing: 2px;
    }
    .menu-window .subtitle { color: #8a9aaa; font-size: 0.95rem; margin-bottom: 25px; letter-spacing: 4px; }
    .menu-window .menu-divider {
      height: 2px;
      background: linear-gradient(90deg, transparent, #4a5a6a, transparent);
      margin: 15px 0 20px 0;
    }
    .menu-window .menu-stats {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 10px 20px;
      font-size: 0.95rem;
      margin: 10px 0 20px 0;
    }
    .menu-window .menu-stats span { color: #b6c8e0; }
    .menu-window .menu-stats .label { color: #8a9aaa; }
    .menu-btn {
      display: block;
      width: 100%;
      padding: 14px;
      margin: 8px 0;
      font-size: 1.05rem;
      font-weight: bold;
      border: none;
      border-radius: 16px;
      cursor: pointer;
      transition: 0.1s;
      touch-action: manipulation;
      color: #eaf0fa;
    }
    .menu-btn:active { transform: scale(0.97); }
    .menu-btn.primary { background: #4a6a3a; border: 2px solid #6a9a5a; box-shadow: 0 4px 0 #2a3a1a; }
    .menu-btn.primary:active { transform: translateY(4px); box-shadow: 0 0px 0 #2a3a1a; }
    .menu-btn.secondary { background: #3a4a6a; border: 2px solid #5a7a9a; box-shadow: 0 4px 0 #1a2a3a; }
    .menu-btn.secondary:active { transform: translateY(4px); box-shadow: 0 0px 0 #1a2a3a; }
    .menu-btn.danger { background: #6a3a3a; border: 2px solid #9a5a5a; box-shadow: 0 4px 0 #3a1a1a; }
    .menu-btn.danger:active { transform: translateY(4px); box-shadow: 0 0px 0 #3a1a1a; }
    .menu-btn.gold { background: #6a5a2a; border: 2px solid #9a8a4a; box-shadow: 0 4px 0 #3a2a0a; }
    .menu-btn.gold:active { transform: translateY(4px); box-shadow: 0 0px 0 #3a2a0a; }
    .menu-btn.purple { background: #4a2a6a; border: 2px solid #7a4a9a; box-shadow: 0 4px 0 #2a0a3a; }
    .menu-btn.purple:active { transform: translateY(4px); box-shadow: 0 0px 0 #2a0a3a; }
    .menu-btn .hotkey { font-size: 0.7rem; opacity: 0.5; margin-left: 8px; }
    .menu-window .version { margin-top: 15px; font-size: 0.7rem; color: #4a5a6a; }
    .menu-window::-webkit-scrollbar { width: 6px; }
    .menu-window::-webkit-scrollbar-track { background: #151b23; border-radius: 10px; }
    .menu-window::-webkit-scrollbar-thumb { background: #4a5a6a; border-radius: 10px; }

    .menu-story {
      text-align: left;
      background: #151b23;
      border-radius: 16px;
      padding: 16px 20px;
      margin: 10px 0 16px 0;
      border: 1px solid #3a4a5e;
      max-height: 180px;
      overflow-y: auto;
    }
    .menu-story p { font-size: 0.9rem; line-height: 1.6; margin: 6px 0; color: #b6c8e0; }
    .menu-story .highlight { color: #d4af37; font-weight: bold; }
    .menu-story .danger { color: #ff6b6b; }
    .menu-story .magic { color: #88ddff; }
    .menu-story::-webkit-scrollbar { width: 4px; }
    .menu-story::-webkit-scrollbar-thumb { background: #4a5a6a; border-radius: 10px; }

    .story-window h2 {
      text-align: center;
      color: #c8a0e0;
      font-size: 1.8rem;
      margin-top: 0;
      margin-bottom: 20px;
      text-shadow: 0 0 20px rgba(100,60,180,0.3);
    }
    .story-window p { line-height: 1.8; font-size: 1.05rem; margin: 12px 0; color: #c8d2e0; }
    .story-window .highlight { color: #d4af37; font-weight: bold; }
    .story-window .danger { color: #ff6b6b; }
    .story-window .magic { color: #88ddff; }
    .story-window .btn-start {
      display: block;
      margin: 25px auto 0;
      padding: 12px 40px;
      font-size: 1.1rem;
      background: #4a2d6a;
      border: 2px solid #8a6aaa;
      color: #eaf0fa;
      border-radius: 30px;
      cursor: pointer;
      font-weight: bold;
      box-shadow: 0 4px 0 #2a1a3a;
      transition: 0.1s;
    }
    .story-window .btn-start:active { transform: translateY(4px); box-shadow: 0 0px 0 #2a1a3a; }
    .story-window::-webkit-scrollbar { width: 6px; }
    .story-window::-webkit-scrollbar-track { background: #151b23; border-radius: 10px; }
    .story-window::-webkit-scrollbar-thumb { background: #4a2d6a; border-radius: 10px; }

    @media (max-width: 550px) {
      .game-wrapper { padding: 8px; }
      .menu-window { padding: 20px; }
      .menu-window h1 { font-size: 1.6rem; }
      .menu-btn { font-size: 0.9rem; padding: 12px; }
      .story-window { padding: 20px; }
      .story-window h2 { font-size: 1.4rem; }
      .story-window p { font-size: 0.95rem; }
    }
  </style>
</head>
<body>
<div class="game-wrapper">
  <div class="canvas-container">
    <canvas id="gameCanvas" width="650" height="650"></canvas>
  </div>
  <div class="info-panel">
    <div class="stats">
      <span>❤️ <span id="hpDisplay">100</span></span>
      <span>⚔️ <span id="atkDisplay">3</span></span>
      <span class="weapon-badge">🗡️ <span id="weaponDisplay">Меч</span></span>
      <span>📊 <span id="levelDisplay">1</span>/50</span>
      <span>💀 <span id="killsDisplay">0</span></span>
      <span>📦 <span id="invCountDisplay">0</span></span>
      <span>🪙 <span id="coinsDisplay">0</span></span>
    </div>
    <div class="message" id="msgDisplay">➡️ Исследуй данж</div>
    <div class="controls">
      <button class="btn menu" id="menuBtn">⚙️</button>
      <button class="btn story" id="storyBtn">📜</button>
      <button class="btn inv" id="invBtn">📦</button>
      <button class="btn auto" id="autoBtn">⚡</button>
      <button class="btn down" id="downBtn">⬇</button>
      <button class="btn reset" id="resetBtn">⟳</button>
      <button class="btn music" id="musicBtn">🎵</button>
    </div>
  </div>

  <!-- D-Pad для кнопочных телефонов -->
  <div class="dpad" id="dpad">
    <div class="dpad-row">
      <button class="btn-dpad" data-dir="up">▲</button>
    </div>
    <div class="dpad-row">
      <button class="btn-dpad" data-dir="left">◄</button>
      <button class="btn-dpad center" data-dir="center">⚔</button>
      <button class="btn-dpad" data-dir="right">►</button>
    </div>
    <div class="dpad-row">
      <button class="btn-dpad" data-dir="down">▼</button>
    </div>
    <div class="dpad-row" style="margin-top:4px; gap:6px;">
      <button class="btn-dpad action" data-dir="attack">⚔️</button>
      <button class="btn-dpad action-heal" data-dir="heal">🧪</button>
      <button class="btn-dpad action-inv" data-dir="inventory">📦</button>
    </div>
  </div>
</div>

<!-- Модальные окна -->
<div class="menu-overlay" id="menuOverlay">
  <div class="menu-window">
    <h1>⚔️ Данжермастер Бой</h1>
    <div class="subtitle">— 50 уровней подземелья —</div>
    <div class="menu-story">
      <p>Вы — <span class="highlight">Сэр Эдрик фон Шталь</span>, рыцарь Ордена Рассвета.</p>
      <p>Преследуя некроманта, вы попали в <span class="danger">проклятый лабиринт</span> <span class="highlight">«Глубины Забвения»</span>.</p>
      <p>Единственный путь — <span class="magic">пройти 50 уровней</span> и найти <span class="highlight">Кристалл Света</span>.</p>
    </div>
    <div class="menu-divider"></div>
    <div class="menu-stats">
      <span><span class="label">🏆 Уровень:</span> <span id="menuLevel">1</span>/50</span>
      <span><span class="label">💀 Убито:</span> <span id="menuKills">0</span></span>
      <span><span class="label">🪙 Монет:</span> <span id="menuCoins">0</span></span>
    </div>
    <button class="menu-btn primary" id="menuResumeBtn">▶ Продолжить</button>
    <button class="menu-btn gold" id="menuStoryBtn">📜 История</button>
    <button class="menu-btn secondary" id="menuInvBtn">📦 Инвентарь</button>
    <button class="menu-btn purple" id="menuAutoBtn">⚡ Авто-атака</button>
    <button class="menu-btn" id="menuMusicBtn" style="background:#2d3a4a;border:2px solid #4a6a8a;box-shadow:0 4px 0 #1a2a3a;">🎵 Музыка</button>
    <button class="menu-btn danger" id="menuResetBtn">⟳ Перезапустить</button>
    <div class="version">v3.0 — Живой изометрический данж</div>
  </div>
</div>

<div class="story-overlay" id="storyOverlay">
  <div class="story-window">
    <h2>⚔️ Легенда о Падшем Рыцаре</h2>
    <p>Вы — <span class="highlight">Сэр Эдрик фон Шталь</span>, рыцарь Ордена Рассвета.</p>
    <p>Преследуя таинственного некроманта, вы попали в <span class="danger">проклятый подземный лабиринт</span> — <span class="highlight">«Глубины Забвения»</span>.</p>
    <p>Магия некроманта захлопнула вход. Единственный путь на поверхность — <span class="magic">пройти через 50 уровней</span>, сражаясь с монстрами и собирая силу древних артефактов.</p>
    <p>Каждый уровень лабиринта имеет свой цвет — от огненно-красного до ледяного синего. Говорят, на самом дне находится <span class="highlight">Кристалл Света</span>.</p>
    <p>Удачи, рыцарь. <span class="danger">Монстры</span> ждут...</p>
    <button class="btn-start" id="storyStartBtn">⚔️ В бой!</button>
  </div>
</div>

<div class="inventory-overlay" id="invOverlay">
  <div class="inventory-window">
    <div class="inv-header">
      <h2>📦 Инвентарь</h2>
      <button class="inv-close" id="invCloseBtn">✕</button>
    </div>
    <div class="inv-items" id="invItemsContainer"></div>
  </div>
</div>

<script>
  (function() {
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');

    const SIZE = 10;
    const CANVAS_SIZE = 650;
    const TILE_W = 50;
    const TILE_H = 28;
    const OFFSET_X = CANVAS_SIZE / 2;
    const OFFSET_Y = 40;

    // ---- ЧАСТИЦЫ ДЛЯ ЖИВОСТИ ----
    let particles = [];
    let fireflies = [];

    function initParticles() {
      for (let i = 0; i < 30; i++) {
        particles.push({
          x: Math.random() * CANVAS_SIZE,
          y: Math.random() * CANVAS_SIZE,
          vx: (Math.random() - 0.5) * 0.3,
          vy: (Math.random() - 0.5) * 0.2 - 0.1,
          size: 2 + Math.random() * 6,
          alpha: 0.05 + Math.random() * 0.1,
          life: 100 + Math.random() * 200
        });
      }
      for (let i = 0; i < 8; i++) {
        fireflies.push({
          x: Math.random() * CANVAS_SIZE,
          y: Math.random() * CANVAS_SIZE,
          vx: (Math.random() - 0.5) * 0.5,
          vy: (Math.random() - 0.5) * 0.5,
          radius: 2 + Math.random() * 3,
          phase: Math.random() * Math.PI * 2,
          speed: 0.02 + Math.random() * 0.03,
          alpha: 0.3 + Math.random() * 0.5
        });
      }
    }

    function updateParticles() {
      for (const p of particles) {
        p.x += p.vx;
        p.y += p.vy;
        p.vy += 0.01;
        if (p.x < 0) p.x = CANVAS_SIZE;
        if (p.x > CANVAS_SIZE) p.x = 0;
        if (p.y < 0) p.y = CANVAS_SIZE;
        if (p.y > CANVAS_SIZE) p.y = 0;
        p.alpha = 0.03 + Math.sin(p.life * 0.02) * 0.03 + 0.03;
        p.life++;
      }
      for (const f of fireflies) {
        f.x += f.vx + Math.sin(f.phase) * 0.3;
        f.y += f.vy + Math.cos(f.phase * 0.7) * 0.3;
        f.phase += f.speed;
        if (f.x < 0) f.x = CANVAS_SIZE;
        if (f.x > CANVAS_SIZE) f.x = 0;
        if (f.y < 0) f.y = CANVAS_SIZE;
        if (f.y > CANVAS_SIZE) f.y = 0;
        f.alpha = 0.3 + Math.sin(f.phase) * 0.2 + 0.3;
      }
    }

    function drawParticles() {
      for (const p of particles) {
        ctx.beginPath();
        ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(180, 200, 220, ${p.alpha})`;
        ctx.fill();
      }
      for (const f of fireflies) {
        const glow = ctx.createRadialGradient(f.x, f.y, 0, f.x, f.y, f.radius * 4);
        glow.addColorStop(0, `rgba(255, 255, 200, ${f.alpha})`);
        glow.addColorStop(1, `rgba(255, 255, 200, 0)`);
        ctx.fillStyle = glow;
        ctx.beginPath();
        ctx.arc(f.x, f.y, f.radius * 4, 0, Math.PI * 2);
        ctx.fill();
        ctx.fillStyle = `rgba(255, 255, 200, ${f.alpha * 0.8})`;
        ctx.beginPath();
        ctx.arc(f.x, f.y, f.radius, 0, Math.PI * 2);
        ctx.fill();
      }
    }

    // ---- ОСНОВНЫЕ ФУНКЦИИ ИГРЫ ----
    function isoToScreen(gridX, gridY) {
      const x = (gridX - gridY) * TILE_W / 2;
      const y = (gridX + gridY) * TILE_H / 2;
      return { x: x + OFFSET_X, y: y + OFFSET_Y };
    }

    function screenToIso(screenX, screenY) {
      const dx = screenX - OFFSET_X;
      const dy = screenY - OFFSET_Y;
      const gridX = Math.round((dx / (TILE_W/2) + dy / (TILE_H/2)) / 2);
      const gridY = Math.round((dy / (TILE_H/2) - dx / (TILE_W/2)) / 2);
      return { x: gridX, y: gridY };
    }

    // Глобальные переменные
    let dungeon = [];
    let player = { x: 0, y: 0 };
    let hp = 100;
    let maxHp = 100;
    let baseAttack = 3;
    let attackBonus = 0;
    let weaponName = 'Меч';
    let gameOver = false;
    let victory = false;
    let monsterCount = 0;
    let currentLevel = 1;
    let kills = 0;
    let coins = 0;
    let autoMode = false;
    let autoInterval = null;
    let inventory = [];
    let isPaused = false;
    let frameCount = 0;
    let playerFrame = 0;
    let monsterFrame = 0;
    const ANIMATION_SPEED = 10;

    // Элементы UI
    const hpSpan = document.getElementById('hpDisplay');
    const atkSpan = document.getElementById('atkDisplay');
    const weaponSpan = document.getElementById('weaponDisplay');
    const levelSpan = document.getElementById('levelDisplay');
    const killsSpan = document.getElementById('killsDisplay');
    const invCountSpan = document.getElementById('invCountDisplay');
    const coinsSpan = document.getElementById('coinsDisplay');
    const msgDiv = document.getElementById('msgDisplay');
    const menuLevel = document.getElementById('menuLevel');
    const menuKills = document.getElementById('menuKills');
    const menuCoins = document.getElementById('menuCoins');

    // Цветовые схемы
    const colorSchemes = [
      { floor: '#3a4a5e', floorLight: '#4a5a6e', wall: '#2b384a', wallLight: '#3b485a', name: 'Каменный' },
      { floor: '#5a3a3a', floorLight: '#6a4a4a', wall: '#4a2a2a', wallLight: '#5a3a3a', name: 'Кровавый' },
      { floor: '#3a5a3a', floorLight: '#4a6a4a', wall: '#2a4a2a', wallLight: '#3a5a3a', name: 'Лесной' },
      { floor: '#3a4a6a', floorLight: '#4a5a7a', wall: '#2a3a5a', wallLight: '#3a4a6a', name: 'Ледяной' },
      { floor: '#6a4a3a', floorLight: '#7a5a4a', wall: '#5a3a2a', wallLight: '#6a4a3a', name: 'Глиняный' },
      { floor: '#4a3a6a', floorLight: '#5a4a7a', wall: '#3a2a5a', wallLight: '#4a3a6a', name: 'Тёмный' },
      { floor: '#3a5a5a', floorLight: '#4a6a6a', wall: '#2a4a4a', wallLight: '#3a5a5a', name: 'Болотный' },
      { floor: '#6a4a4a', floorLight: '#7a5a5a', wall: '#5a3a3a', wallLight: '#6a4a4a', name: 'Ржавый' },
      { floor: '#4a5a3a', floorLight: '#5a6a4a', wall: '#3a4a2a', wallLight: '#4a5a3a', name: 'Ядовитый' },
      { floor: '#5a4a6a', floorLight: '#6a5a7a', wall: '#4a3a5a', wallLight: '#5a4a6a', name: 'Мистический' }
    ];

    function getColors(level) {
      return colorSchemes[(level - 1) % colorSchemes.length];
    }

    // ---- МУЗЫКА ----
    let audioCtx = null;
    let musicNodes = [];
    let isMusicPlaying = false;
    let isMusicMuted = false;

    function createMusic() {
      try {
        audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        const notes = [
          { freq: 220, duration: 0.4 }, { freq: 277, duration: 0.4 },
          { freq: 330, duration: 0.4 }, { freq: 440, duration: 0.8 },
          { freq: 330, duration: 0.4 }, { freq: 277, duration: 0.4 },
          { freq: 220, duration: 0.8 }, { freq: 330, duration: 0.4 },
          { freq: 440, duration: 0.4 }, { freq: 554, duration: 0.8 },
          { freq: 440, duration: 0.4 }, { freq: 330, duration: 0.4 },
          { freq: 277, duration: 0.8 },
        ];
        let time = audioCtx.currentTime + 0.1;
        notes.forEach((note) => {
          const osc1 = audioCtx.createOscillator();
          const gain1 = audioCtx.createGain();
          osc1.type = 'square';
          osc1.frequency.value = note.freq;
          gain1.gain.value = 0.08;
          gain1.gain.exponentialRampToValueAtTime(0.001, time + note.duration);
          osc1.connect(gain1);
          gain1.connect(audioCtx.destination);
          osc1.start(time);
          osc1.stop(time + note.duration);
          musicNodes.push(osc1, gain1);
          const osc2 = audioCtx.createOscillator();
          const gain2 = audioCtx.createGain();
          osc2.type = 'sine';
          osc2.frequency.value = note.freq * 1.25;
          gain2.gain.value = 0.04;
          gain2.gain.exponentialRampToValueAtTime(0.001, time + note.duration);
          osc2.connect(gain2);
          gain2.connect(audioCtx.destination);
          osc2.start(time);
          osc2.stop(time + note.duration);
          musicNodes.push(osc2, gain2);
          const osc3 = audioCtx.createOscillator();
          const gain3 = audioCtx.createGain();
          osc3.type = 'sawtooth';
          osc3.frequency.value = note.freq * 0.5;
          gain3.gain.value = 0.06;
          gain3.gain.exponentialRampToValueAtTime(0.001, time + note.duration);
          osc3.connect(gain3);
          gain3.connect(audioCtx.destination);
          osc3.start(time);
          osc3.stop(time + note.duration);
          musicNodes.push(osc3, gain3);
          time += note.duration + 0.05;
        });
        const drone = audioCtx.createOscillator();
        const droneGain = audioCtx.createGain();
        drone.type = 'sawtooth';
        drone.frequency.value = 110;
        droneGain.gain.value = 0.03;
        droneGain.gain.exponentialRampToValueAtTime(0.001, time + 2);
        drone.connect(droneGain);
        droneGain.connect(audioCtx.destination);
        drone.start(audioCtx.currentTime + 0.1);
        drone.stop(audioCtx.currentTime + 8);
        musicNodes.push(drone, droneGain);
        isMusicPlaying = true;
        document.getElementById('musicBtn').textContent = '🎵';
        document.getElementById('musicBtn').classList.remove('muted');
        setTimeout(() => {
          if (isMusicPlaying && !isMusicMuted) createMusic();
        }, 8500);
      } catch(e) { console.log('Audio not supported'); }
    }

    function toggleMusic() {
      if (!audioCtx) { createMusic(); return; }
      if (isMusicMuted) {
        isMusicMuted = false;
        document.getElementById('musicBtn').textContent = '🎵';
        document.getElementById('musicBtn').classList.remove('muted');
        if (!isMusicPlaying) createMusic();
      } else {
        isMusicMuted = true;
        isMusicPlaying = false;
        document.getElementById('musicBtn').textContent = '🔇';
        document.getElementById('musicBtn').classList.add('muted');
        musicNodes.forEach(node => {
          try { node.stop && node.stop(); } catch(e) {}
          try { node.disconnect && node.disconnect(); } catch(e) {}
        });
        musicNodes = [];
        if (audioCtx) { try { audioCtx.close(); } catch(e) {} audioCtx = null; }
      }
    }

    // ---- ИГРОВЫЕ ФУНКЦИИ ----
    function rand(min, max) { return Math.floor(Math.random() * (max - min + 1)) + min; }

    function generateDungeon(level) {
      const grid = Array(SIZE).fill().map(() => Array(SIZE).fill(1));
      function carve(x, y) {
        const dirs = [[0, -2], [0, 2], [-2, 0], [2, 0]];
        for (let i = dirs.length - 1; i > 0; i--) {
          const j = rand(0, i);
          [dirs[i], dirs[j]] = [dirs[j], dirs[i]];
        }
        for (let [dx, dy] of dirs) {
          const nx = x + dx, ny = y + dy;
          if (nx >= 0 && nx < SIZE && ny >= 0 && ny < SIZE && grid[nx][ny] === 1) {
            grid[x + dx/2][y + dy/2] = 0;
            grid[nx][ny] = 0;
            carve(nx, ny);
          }
        }
      }
      grid[1][1] = 0;
      carve(1, 1);
      grid[0][0] = 0;
      if (grid[1][0] === 1) grid[1][0] = 0;
      if (grid[0][1] === 1) grid[0][1] = 0;

      const baseMonsters = 6;
      const extraMonsters = Math.min(level, 10);
      const numMonsters = rand(baseMonsters, baseMonsters + extraMonsters);
      let placed = 0, attempts = 0;
      while (placed < numMonsters && attempts < 800) {
        attempts++;
        const x = rand(0, SIZE-1), y = rand(0, SIZE-1);
        if (x === 0 && y === 0) continue;
        if (Math.abs(x - 0) + Math.abs(y - 0) <= 2) continue;
        if (grid[x][y] === 0) { grid[x][y] = 2; placed++; }
      }
      if (placed === 0) {
        for (let y = 0; y < SIZE; y++) {
          for (let x = 0; x < SIZE; x++) {
            if (grid[x][y] === 0 && !(x === 0 && y === 0)) {
              grid[x][y] = 2; placed++; break;
            }
          }
          if (placed) break;
        }
      }
      const chestCount = rand(2, Math.min(3 + Math.floor(level / 5), 6));
      let chestPlaced = 0, chestAttempts = 0;
      while (chestPlaced < chestCount && chestAttempts < 400) {
        chestAttempts++;
        const x = rand(0, SIZE-1), y = rand(0, SIZE-1);
        if (x === 0 && y === 0) continue;
        if (Math.abs(x - 0) + Math.abs(y - 0) <= 2) continue;
        if (grid[x][y] === 0) { grid[x][y] = 3; chestPlaced++; }
      }
      if (level < MAX_LEVEL) {
        let stairsPlaced = false, stairsAttempts = 0;
        while (!stairsPlaced && stairsAttempts < 300) {
          stairsAttempts++;
          const x = rand(0, SIZE-1), y = rand(0, SIZE-1);
          if (x === 0 && y === 0) continue;
          if (Math.abs(x - 0) + Math.abs(y - 0) <= 2) continue;
          if (grid[x][y] === 0) { grid[x][y] = 4; stairsPlaced = true; }
        }
        if (!stairsPlaced) {
          for (let y = 0; y < SIZE; y++) {
            for (let x = 0; x < SIZE; x++) {
              if (grid[x][y] === 0 && !(x === 0 && y === 0)) {
                grid[x][y] = 4; stairsPlaced = true; break;
              }
            }
            if (stairsPlaced) break;
          }
        }
      }
      return grid;
    }

    const MAX_LEVEL = 50;
    const MAX_INVENTORY = 20;

    function resetGame() {
      stopAutoMode();
      currentLevel = 1;
      dungeon = generateDungeon(currentLevel);
      player = { x: 0, y: 0 };
      hp = maxHp;
      baseAttack = 3;
      attackBonus = 0;
      weaponName = 'Меч';
      gameOver = false;
      victory = false;
      kills = 0;
      coins = 0;
      inventory = [];
      monsterCount = 0;
      for (let row of dungeon) for (let cell of row) if (cell === 2) monsterCount++;
      updateUI();
      msgDiv.textContent = '⚔️ Исследуй данж';
      render();
    }

    function updateUI() {
      hpSpan.textContent = hp;
      atkSpan.textContent = baseAttack + attackBonus;
      weaponSpan.textContent = weaponName;
      levelSpan.textContent = currentLevel;
      killsSpan.textContent = kills;
      invCountSpan.textContent = inventory.length;
      coinsSpan.textContent = coins;
      if (hp <= 0) hpSpan.textContent = '0';
      menuLevel.textContent = currentLevel;
      menuKills.textContent = kills;
      menuCoins.textContent = coins;
    }

    // ---- ОТРИСОВКА ----
    function drawIsometricTile(gridX, gridY, type, frame) {
      const pos = isoToScreen(gridX, gridY);
      const x = pos.x;
      const y = pos.y;
      const w = TILE_W;
      const h = TILE_H;
      const colors = getColors(currentLevel);

      ctx.shadowColor = 'rgba(0,0,0,0.3)';
      ctx.shadowBlur = 4;
      ctx.shadowOffsetX = 0;
      ctx.shadowOffsetY = 4;

      ctx.beginPath();
      ctx.moveTo(x, y);
      ctx.lineTo(x + w/2, y + h/2);
      ctx.lineTo(x, y + h);
      ctx.lineTo(x - w/2, y + h/2);
      ctx.closePath();
      
      if (type === 1) {
        ctx.shadowBlur = 0;
        ctx.fillStyle = colors.wall;
        ctx.fill();
        ctx.strokeStyle = colors.wallLight;
        ctx.lineWidth = 1;
        ctx.stroke();
        ctx.beginPath();
        ctx.moveTo(x, y - 10);
        ctx.lineTo(x + w/2, y + h/2 - 10);
        ctx.lineTo(x, y + h - 10);
        ctx.lineTo(x - w/2, y + h/2 - 10);
        ctx.closePath();
        const glow = 0.2 + Math.sin(frame * 0.03) * 0.1;
        ctx.fillStyle = `rgba(200, 220, 255, ${glow})`;
        ctx.fill();
        ctx.fillStyle = colors.wallLight;
        ctx.fill();
        ctx.strokeStyle = colors.wall;
        ctx.stroke();
        ctx.shadowColor = 'rgba(0,0,0,0.5)';
        ctx.shadowBlur = 6;
        ctx.beginPath();
        ctx.moveTo(x - w/2, y + h/2);
        ctx.lineTo(x - w/2, y + h/2 - 10);
        ctx.lineTo(x, y + h - 10);
        ctx.lineTo(x, y + h);
        ctx.closePath();
        ctx.fillStyle = colors.wall;
        ctx.fill();
        ctx.strokeStyle = colors.wall;
        ctx.stroke();
        ctx.beginPath();
        ctx.moveTo(x + w/2, y + h/2);
        ctx.lineTo(x + w/2, y + h/2 - 10);
        ctx.lineTo(x, y + h - 10);
        ctx.lineTo(x, y + h);
        ctx.closePath();
        ctx.fillStyle = colors.wall;
        ctx.fill();
        ctx.strokeStyle = colors.wall;
        ctx.stroke();
        ctx.shadowBlur = 0;
        if ((gridX + gridY) % 3 === 0) {
          const crystalGlow = 0.3 + Math.sin(frame * 0.05 + gridX + gridY) * 0.2;
          ctx.fillStyle = `rgba(100, 200, 255, ${crystalGlow * 0.3})`;
          ctx.beginPath();
          ctx.arc(x - 8 + Math.sin(frame * 0.02 + gridX) * 3, y + 6 + Math.sin(frame * 0.03 + gridY) * 3, 3, 0, Math.PI * 2);
          ctx.fill();
        }
      } else {
        ctx.shadowBlur = 2;
        ctx.shadowColor = 'rgba(0,0,0,0.2)';
        ctx.fillStyle = colors.floor;
        ctx.fill();
        ctx.shadowBlur = 0;
        ctx.strokeStyle = colors.floorLight;
        ctx.lineWidth = 1;
        ctx.stroke();
        const pulse = 0.1 + Math.sin(frame * 0.02 + gridX * 0.5 + gridY * 0.7) * 0.05;
        if ((gridX + gridY) % 2 === 0) {
          ctx.beginPath();
          ctx.moveTo(x - w/4, y + h/4);
          ctx.lineTo(x, y + h/4);
          ctx.lineTo(x + w/4, y + h/2);
          ctx.lineTo(x, y + h*3/4);
          ctx.lineTo(x - w/4, y + h/2);
          ctx.closePath();
          ctx.fillStyle = `rgba(255, 255, 255, ${pulse})`;
          ctx.fill();
        }
        ctx.shadowBlur = 0;
      }
      ctx.shadowBlur = 0;
      ctx.shadowOffsetY = 0;
      return pos;
    }

    function drawAnimatedPlayerIso(gridX, gridY, frame) {
      const pos = isoToScreen(gridX, gridY);
      const x = pos.x;
      const y = pos.y - 12;
      const bobOffset = Math.sin(frame * 0.3) * 2;
      
      ctx.shadowColor = 'rgba(0,0,0,0.5)';
      ctx.shadowBlur = 10;
      ctx.shadowOffsetY = 4;
      
      ctx.fillStyle = '#5a1a2a';
      ctx.fillRect(x - 14 + Math.sin(frame * 0.2) * 2, y + 2 + bobOffset, 8, 20);
      ctx.fillRect(x + 6 - Math.sin(frame * 0.2) * 2, y + 2 + bobOffset, 8, 20);
      ctx.fillStyle = '#4a6a8a';
      ctx.fillRect(x - 10, y + 4 + bobOffset, 20, 22);
      ctx.fillStyle = '#3a5a7a';
      ctx.fillRect(x - 6, y + 8 + bobOffset, 12, 16);
      
      const legOffset = Math.sin(frame * 0.4) * 3;
      ctx.fillStyle = '#2a3a4a';
      ctx.fillRect(x - 8 + legOffset, y + 22 + bobOffset, 6, 8);
      ctx.fillRect(x + 2 - legOffset, y + 22 + bobOffset, 6, 8);
      
      ctx.fillStyle = '#8a9aaa';
      ctx.fillRect(x - 8, y - 4 + bobOffset, 16, 12);
      ctx.fillStyle = '#6a7a8a';
      ctx.fillRect(x - 6, y - 2 + bobOffset, 12, 8);
      
      const glow = 0.7 + Math.sin(frame * 0.1) * 0.3;
      ctx.fillStyle = '#f0e6d2';
      ctx.fillRect(x - 4, y + 2 + bobOffset, 3, 3);
      ctx.fillRect(x + 1, y + 2 + bobOffset, 3, 3);
      ctx.fillStyle = `rgba(74, 240, 255, ${glow})`;
      ctx.fillRect(x - 3, y + 3 + bobOffset, 1, 1);
      ctx.fillRect(x + 2, y + 3 + bobOffset, 1, 1);
      
      ctx.fillStyle = '#c0c8d0';
      ctx.fillRect(x + 10, y - 2 + bobOffset, 3, 20);
      ctx.fillStyle = '#8a7a4a';
      ctx.fillRect(x + 6, y + 18 + bobOffset, 10, 3);
      
      ctx.fillStyle = '#5a6a7a';
      ctx.fillRect(x - 14, y + 2 + bobOffset, 8, 14);
      ctx.fillStyle = '#7a8a9a';
      ctx.fillRect(x - 12, y + 4 + bobOffset, 4, 10);
      
      ctx.shadowBlur = 0;
      ctx.shadowOffsetY = 0;
    }

    function drawAnimatedMonsterIso(gridX, gridY, frame) {
      const pos = isoToScreen(gridX, gridY);
      const x = pos.x;
      const y = pos.y - 8;
      const pulse = 1 + Math.sin(frame * 0.08) * 0.04;
      const floatOffset = Math.sin(frame * 0.06) * 2;
      const radius = 16 + Math.sin(frame * 0.05) * 2;
      
      ctx.save();
      ctx.translate(x, y + floatOffset);
      ctx.scale(pulse, pulse);
      
      ctx.shadowColor = '#b33a3a';
      ctx.shadowBlur = 20 + Math.sin(frame * 0.1) * 5;
      ctx.shadowOffsetY = 4;
      
      ctx.beginPath();
      ctx.arc(0, 0, radius, 0, 2 * Math.PI);
      ctx.fillStyle = '#b13a3a';
      ctx.fill();
      ctx.shadowBlur = 0;
      
      const innerGlow = 0.3 + Math.sin(frame * 0.1) * 0.15;
      ctx.beginPath();
      ctx.arc(-2, -2, radius * 0.5, 0, 2 * Math.PI);
      ctx.fillStyle = `rgba(200, 80, 80, ${innerGlow})`;
      ctx.fill();
      
      const eyeGlow = 0.6 + Math.sin(frame * 0.15) * 0.4;
      ctx.fillStyle = '#f0e6d2';
      ctx.beginPath();
      ctx.arc(-6, -4 + Math.sin(frame * 0.08) * 2, 4, 0, 2 * Math.PI);
      ctx.fill();
      ctx.beginPath();
      ctx.arc(6, -4 + Math.sin(frame * 0.08 + 1) * 2, 4, 0, 2 * Math.PI);
      ctx.fill();
      ctx.fillStyle = `rgba(255, 50, 50, ${eyeGlow})`;
      ctx.beginPath();
      ctx.arc(-6, -2 + Math.sin(frame * 0.08) * 2, 2, 0, 2 * Math.PI);
      ctx.fill();
      ctx.beginPath();
      ctx.arc(6, -2 + Math.sin(frame * 0.08 + 1) * 2, 2, 0, 2 * Math.PI);
      ctx.fill();
      ctx.fillStyle = '#1f1f2a';
      ctx.beginPath();
      ctx.arc(-6, -2 + Math.sin(frame * 0.08) * 2, 1, 0, 2 * Math.PI);
      ctx.fill();
      ctx.beginPath();
      ctx.arc(6, -2 + Math.sin(frame * 0.08 + 1) * 2, 1, 0, 2 * Math.PI);
      ctx.fill();
      
      ctx.fillStyle = '#a04040';
      ctx.beginPath();
      ctx.arc(0, 6 + Math.sin(frame * 0.05) * 2, 8 + Math.sin(frame * 0.07) * 2, 0, Math.PI, true);
      ctx.fill();
      
      ctx.fillStyle = '#8a2a2a';
      ctx.beginPath();
      ctx.moveTo(-10, -10 + Math.sin(frame * 0.03) * 3);
      ctx.lineTo(-6, -16 + Math.sin(frame * 0.04) * 4);
      ctx.lineTo(-3, -10 + Math.sin(frame * 0.03) * 3);
      ctx.fill();
      ctx.beginPath();
      ctx.moveTo(10, -10 + Math.sin(frame * 0.03 + 0.5) * 3);
      ctx.lineTo(6, -16 + Math.sin(frame * 0.04 + 0.5) * 4);
      ctx.lineTo(3, -10 + Math.sin(frame * 0.03 + 0.5) * 3);
      ctx.fill();
      
      const auraAlpha = 0.05 + Math.sin(frame * 0.05) * 0.03;
      ctx.beginPath();
      ctx.arc(0, 0, radius * 1.8, 0, 2 * Math.PI);
      ctx.fillStyle = `rgba(180, 30, 30, ${auraAlpha})`;
      ctx.fill();
      
      ctx.shadowOffsetY = 0;
      ctx.restore();
    }

    function drawLootAnimationsIso() {
      for (const l of lootAnimations) {
        const pos = isoToScreen(l.gridX, l.gridY);
        const x = pos.x;
        const y = pos.y - 20 - l.offsetY;
        ctx.save();
        ctx.globalAlpha = l.alpha;
        ctx.translate(x, y);
        ctx.scale(l.scale, l.scale);
        ctx.shadowColor = 'rgba(0,0,0,0.8)';
        ctx.shadowBlur = 15;
        ctx.shadowOffsetY = 4;
        ctx.font = '24px sans-serif';
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';
        ctx.fillStyle = l.color;
        ctx.fillText(l.icon, 0, 0);
        ctx.shadowBlur = 0;
        ctx.font = 'bold 12px sans-serif';
        ctx.fillStyle = '#fff';
        ctx.shadowColor = 'rgba(0,0,0,0.9)';
        ctx.shadowBlur = 8;
        ctx.fillText(l.text, 0, 24);
        ctx.restore();
      }
    }

    let lootAnimations = [];

    function spawnLoot(gridX, gridY, icon, text, color = '#ffdd44') {
      lootAnimations.push({
        gridX: gridX,
        gridY: gridY,
        offsetY: 0,
        vy: -3,
        gravity: 0.2,
        life: 60,
        maxLife: 60,
        icon: icon,
        text: text,
        color: color,
        scale: 1.2,
        alpha: 1
      });
    }

    function updateLootAnimations() {
      for (let i = lootAnimations.length - 1; i >= 0; i--) {
        const l = lootAnimations[i];
        l.offsetY += l.vy;
        l.vy += l.gravity;
        if (l.offsetY > 0) {
          l.offsetY = 0;
          l.vy = 0;
        }
        l.life--;
        l.alpha = Math.min(1, l.life / 20);
        l.scale = 1.2 - (1 - l.life / l.maxLife) * 0.4;
        if (l.life <= 0) {
          lootAnimations.splice(i, 1);
        }
      }
    }

    // ---- ИГРОВАЯ ЛОГИКА ----
    function quickHeal() {
      if (gameOver || victory) return;
      for (let i = 0; i < inventory.length; i++) {
        if (inventory[i].type === 'heal') {
          const healAmount = inventory[i].bonus;
          hp = Math.min(maxHp, hp + healAmount);
          inventory.splice(i, 1);
          msgDiv.textContent = `❤️ +${healAmount} здоровья!`;
          updateUI();
          render();
          updateInventoryUI();
          return;
        }
      }
      msgDiv.textContent = '❌ Нет зелья!';
      render();
    }

    function goDown() {
      if (gameOver || victory) return;
      if (currentLevel >= MAX_LEVEL) {
        msgDiv.textContent = '🏆 Победа!';
        victory = true;
        stopAutoMode();
        render();
        return;
      }
      const cell = dungeon[player.x][player.y];
      if (cell !== 4) {
        msgDiv.textContent = '🔍 Найдите лестницу!';
        render();
        return;
      }
      currentLevel++;
      dungeon = generateDungeon(currentLevel);
      player = { x: 0, y: 0 };
      hp = Math.min(maxHp, hp + 5);
      monsterCount = 0;
      for (let row of dungeon) for (let cell of row) if (cell === 2) monsterCount++;
      const colors = getColors(currentLevel);
      msgDiv.textContent = `⬇️ Уровень ${currentLevel} (${colors.name})`;
      updateUI();
      render();
    }

    function openChest(gridX, gridY) {
      dungeon[gridX][gridY] = 0;
      const lootType = rand(0, 4);
      let icon, text, color;
      if (lootType === 0) {
        const amount = rand(5, 20);
        coins += amount;
        icon = '🪙'; text = `+${amount}`; color = '#ffdd44';
        msgDiv.textContent = `🪙 +${amount} монет! (${coins})`;
      } else if (lootType === 1) {
        const amount = rand(1, 5);
        if (inventory.length < MAX_INVENTORY) {
          inventory.push({ name: `Кристалл (x${amount})`, type: 'crystal', bonus: amount, icon: '💎' });
          icon = '💎'; text = `+${amount}`; color = '#88ddff';
          msgDiv.textContent = `💎 +${amount} кристаллов!`;
        } else {
          msgDiv.textContent = '⚠️ Инвентарь полон!';
          icon = '⚠️'; text = '!'; color = '#ff6666';
        }
      } else if (lootType === 2) {
        const heal = rand(5, 15);
        if (inventory.length < MAX_INVENTORY) {
          inventory.push({ name: `Зелье (+${heal})`, type: 'heal', bonus: heal, icon: '🧪' });
          icon = '🧪'; text = `+${heal}`; color = '#66ff88';
          msgDiv.textContent = `🧪 Зелье (+${heal})!`;
        } else {
          msgDiv.textContent = '⚠️ Инвентарь полон!';
          icon = '⚠️'; text = '!'; color = '#ff6666';
        }
      } else if (lootType === 3) {
        const spellBonus = rand(2, 5);
        if (inventory.length < MAX_INVENTORY) {
          const spellNames = ['Огненный шар', 'Ледяная стрела', 'Молния', 'Тьма', 'Свет'];
          const spellName = spellNames[rand(0, spellNames.length-1)];
          inventory.push({ name: `Книга: ${spellName}`, type: 'spell', bonus: spellBonus, icon: '📕' });
          icon = '📕'; text = `+${spellBonus}`; color = '#ff88dd';
          msgDiv.textContent = `📕 "${spellName}" (+${spellBonus} атаки)!`;
        } else {
          msgDiv.textContent = '⚠️ Инвентарь полон!';
          icon = '⚠️'; text = '!'; color = '#ff6666';
        }
      } else if (lootType === 4) {
        const bonus = rand(1, 3);
        const names = ['Топор', 'Катана', 'Копьё', 'Посох', 'Длинный меч', 'Двуручник', 'Секира', 'Кинжал'];
        const newName = names[rand(0, names.length-1)];
        if (inventory.length < MAX_INVENTORY) {
          inventory.push({ name: newName, type: 'weapon', bonus: bonus, icon: '🗡️' });
          icon = '🗡️'; text = `+${bonus}`; color = '#ffaa66';
          msgDiv.textContent = `🗡️ ${newName} (+${bonus} атаки)!`;
        } else {
          msgDiv.textContent = '⚠️ Инвентарь полон!';
          icon = '⚠️'; text = '!'; color = '#ff6666';
        }
      }
      spawnLoot(gridX, gridY, icon, text, color);
      updateUI();
      render();
    }

    function tryMove(dx, dy) {
      if (gameOver || victory || isPaused) return;
      const newX = player.x + dx, newY = player.y + dy;
      if (newX < 0 || newX >= SIZE || newY < 0 || newY >= SIZE) {
        msgDiv.textContent = '🚫 Стена!';
        render();
        return;
      }
      const cell = dungeon[newX][newY];
      if (cell === 1) {
        msgDiv.textContent = '🚫 Стена!';
        render();
        return;
      }
      if (cell === 3) {
        openChest(newX, newY);
        player.x = newX; player.y = newY;
        return;
      }
      if (cell === 4) {
        player.x = newX; player.y = newY;
        msgDiv.textContent = '⬇️ Нажмите "Спуск"';
        updateUI();
        render();
        return;
      }
      if (cell === 2) {
        let totalAttack = baseAttack + attackBonus;
        if (window._tempSpellBonus) {
          totalAttack += window._tempSpellBonus;
          window._tempSpellBonus = null;
        }
        const playerDmg = totalAttack + rand(0, 2);
        const monsterDmg = rand(5, 12);
        msgDiv.textContent = `⚔️ Удар! (${playerDmg})`;
        dungeon[newX][newY] = 0;
        monsterCount--;
        kills++;
        hp -= monsterDmg;
        if (hp < 0) hp = 0;
        if (hp <= 0) {
          gameOver = true; hp = 0;
          stopAutoMode();
          updateUI();
          render();
          msgDiv.textContent = '💀 Вы погибли...';
          return;
        }
        if (monsterCount === 0 && currentLevel === MAX_LEVEL) {
          victory = true;
          stopAutoMode();
          updateUI();
          render();
          msgDiv.textContent = '🎉 Победа!';
          return;
        }
        player.x = newX; player.y = newY;
        updateUI();
        render();
        return;
      }
      player.x = newX; player.y = newY;
      msgDiv.textContent = '🚶 Шаг';
      updateUI();
      render();
    }

    // ---- АВТО ----
    function startAutoMode() {
      if (gameOver || victory || autoMode) return;
      autoMode = true;
      document.getElementById('autoBtn').classList.add('active');
      document.getElementById('autoBtn').textContent = '⏹';
      msgDiv.textContent = '⚡ Авто-атака';
      autoInterval = setInterval(() => {
        if (gameOver || victory) { stopAutoMode(); return; }
        if (!isPaused) autoAttack();
      }, 400);
    }
    function stopAutoMode() {
      autoMode = false;
      if (autoInterval) { clearInterval(autoInterval); autoInterval = null; }
      document.getElementById('autoBtn').classList.remove('active');
      document.getElementById('autoBtn').textContent = '⚡';
    }
    function toggleAutoMode() { if (autoMode) stopAutoMode(); else startAutoMode(); }

    function autoAttack() {
      if (gameOver || victory || monsterCount === 0) return;
      let nearestMonster = null, nearestDist = Infinity;
      for (let x = 0; x < SIZE; x++) {
        for (let y = 0; y < SIZE; y++) {
          if (dungeon[x][y] === 2) {
            const dist = Math.abs(x - player.x) + Math.abs(y - player.y);
            if (dist < nearestDist) { nearestDist = dist; nearestMonster = { x, y }; }
          }
        }
      }
      if (!nearestMonster) return;
      if (nearestDist === 1) {
        tryMove(nearestMonster.x - player.x, nearestMonster.y - player.y);
      } else if (nearestDist <= 5) {
        const dx = Math.sign(nearestMonster.x - player.x);
        const dy = Math.sign(nearestMonster.y - player.y);
        if (dx !== 0) {
          const nx = player.x + dx, ny = player.y;
          if (nx >= 0 && nx < SIZE && ny >= 0 && ny < SIZE && dungeon[nx][ny] === 0) {
            tryMove(dx, 0); return;
          }
        }
        if (dy !== 0) {
          const nx = player.x, ny = player.y + dy;
          if (nx >= 0 && nx < SIZE && ny >= 0 && ny < SIZE && dungeon[nx][ny] === 0) {
            tryMove(0, dy); return;
          }
        }
        const dirs = [[1,0], [-1,0], [0,1], [0,-1]];
        for (let [ddx, ddy] of dirs) {
          const nx = player.x + ddx, ny = player.y + ddy;
          if (nx >= 0 && nx < SIZE && ny >= 0 && ny < SIZE && dungeon[nx][ny] === 0) {
            tryMove(ddx, ddy); return;
          }
        }
      }
    }

    // ---- ИНВЕНТАРЬ ----
    function useItem(index) {
      if (index < 0 || index >= inventory.length) return;
      const item = inventory[index];
      if (item.type === 'heal') {
        hp = Math.min(maxHp, hp + item.bonus);
        inventory.splice(index, 1);
        msgDiv.textContent = `❤️ +${item.bonus} HP!`;
      } else if (item.type === 'spell') {
        const tempBonus = item.bonus;
        baseAttack += tempBonus;
        msgDiv.textContent = `📕 +${tempBonus} атаки (1 бой)`;
        inventory.splice(index, 1);
        window._tempSpellBonus = tempBonus;
      } else if (item.type === 'weapon') {
        attackBonus = item.bonus;
        weaponName = item.name;
        inventory.splice(index, 1);
        msgDiv.textContent = `🗡️ ${item.name} (+${item.bonus})`;
      } else if (item.type === 'crystal') {
        const coinValue = item.bonus * 3;
        coins += coinValue;
        inventory.splice(index, 1);
        msgDiv.textContent = `💎 +${coinValue} монет!`;
      }
      updateUI();
      render();
      updateInventoryUI();
    }

    function updateInventoryUI() {
      const container = document.getElementById('invItemsContainer');
      if (inventory.length === 0) {
        container.innerHTML = '<div class="inv-empty">Инвентарь пуст</div>';
        return;
      }
      let html = '';
      inventory.forEach((item, index) => {
        const icon = item.icon || '📦';
        const typeLabels = { 'weapon': '🗡️ Оружие', 'heal': '🧪 Зелье', 'spell': '📕 Заклинание', 'crystal': '💎 Кристалл' };
        const typeLabel = typeLabels[item.type] || '📦 Предмет';
        const bonusTexts = {
          'weapon': `+${item.bonus} атаки`,
          'heal': `+${item.bonus} HP`,
          'spell': `+${item.bonus} атаки (врем.)`,
          'crystal': `${item.bonus} шт. (продать за ${item.bonus*3} монет)`
        };
        const bonusText = bonusTexts[item.type] || `+${item.bonus}`;
        const useClasses = { 'weapon': 'item-use', 'heal': 'item-use heal', 'spell': 'item-use spell', 'crystal': 'item-use' };
        const useClass = useClasses[item.type] || 'item-use';
        const useTexts = { 'weapon': 'Экипировать', 'heal': 'Использовать', 'spell': 'Изучить', 'crystal': 'Продать' };
        const useText = useTexts[item.type] || 'Использовать';
        html += `
          <div class="inv-item">
            <div><span class="item-name">${icon} ${item.name}</span> <span class="item-type">${typeLabel}</span></div>
            <div style="display:flex; gap:6px; align-items:center; flex-wrap:wrap;">
              <span class="item-bonus">${bonusText}</span>
              <button class="${useClass}" data-index="${index}">${useText}</button>
            </div>
          </div>
        `;
      });
      container.innerHTML = html;
      container.querySelectorAll('.item-use').forEach(btn => {
        btn.addEventListener('click', (e) => {
          const index = parseInt(e.target.dataset.index);
          useItem(index);
        });
        btn.addEventListener('touchstart', (e) => {
          const index = parseInt(e.target.dataset.index);
          useItem(index);
        }, {passive: true});
      });
    }

    // ---- ОБРАБОТЧИК КЛИКОВ ----
    function handleCanvasClick(e) {
      e.preventDefault();
      if (gameOver || victory || isPaused) return;
      if (autoMode) stopAutoMode();
      
      const rect = canvas.getBoundingClientRect();
      const scaleX = canvas.width / rect.width;
      const scaleY = canvas.height / rect.height;
      let clientX, clientY;
      if (e.touches) {
        clientX = e.touches[0].clientX;
        clientY = e.touches[0].clientY;
      } else {
        clientX = e.clientX;
        clientY = e.clientY;
      }
      const mouseX = (clientX - rect.left) * scaleX;
      const mouseY = (clientY - rect.top) * scaleY;
      
      const grid = screenToIso(mouseX, mouseY);
      const gridX = grid.x;
      const gridY = grid.y;
      
      if (gridX < 0 || gridX >= SIZE || gridY < 0 || gridY >= SIZE) {
        msgDiv.textContent = '🚫 Вне карты!';
        render();
        return;
      }

      if (dungeon[gridX][gridY] === 2) {
        const dist = Math.abs(gridX - player.x) + Math.abs(gridY - player.y);
        if (dist === 1) {
          tryMove(gridX - player.x, gridY - player.y);
        } else {
          msgDiv.textContent = '🔴 Подойдите ближе!';
          render();
        }
        return;
      }
      
      const dist = Math.abs(gridX - player.x) + Math.abs(gridY - player.y);
      if (dist <= 3) {
        let curX = player.x, curY = player.y;
        while (curX !== gridX || curY !== gridY) {
          const dx = Math.sign(gridX - curX);
          const dy = Math.sign(gridY - curY);
          let moved = false;
          if (dx !== 0) {
            const nx = curX + dx;
            if (nx >= 0 && nx < SIZE && dungeon[nx][curY] === 0) {
              tryMove(dx, 0);
              moved = true;
              curX = nx;
            }
          }
          if (!moved && dy !== 0) {
            const ny = curY + dy;
            if (ny >= 0 && ny < SIZE && dungeon[curX][ny] === 0) {
              tryMove(0, dy);
              moved = true;
              curY = ny;
            }
          }
          if (!moved) break;
        }
      } else {
        msgDiv.textContent = '🔴 Слишком далеко!';
        render();
      }
    }

    // ---- ОСНОВНАЯ ОТРИСОВКА ----
    function render() {
      ctx.clearRect(0, 0, CANVAS_SIZE, CANVAS_SIZE);
      
      frameCount++;
      if (frameCount % ANIMATION_SPEED === 0) {
        playerFrame++;
        monsterFrame++;
      }
      
      drawParticles();
      
      for (let x = 0; x < SIZE; x++) {
        for (let y = 0; y < SIZE; y++) {
          drawIsometricTile(x, y, dungeon[x][y], frameCount);
        }
      }
      
      for (let x = 0; x < SIZE; x++) {
        for (let y = 0; y < SIZE; y++) {
          const cell = dungeon[x][y];
          const pos = isoToScreen(x, y);
          if (cell === 4) {
            const pulse = 0.7 + Math.sin(frameCount * 0.05) * 0.3;
            ctx.shadowColor = `rgba(212, 192, 128, ${pulse * 0.5})`;
            ctx.shadowBlur = 20 + Math.sin(frameCount * 0.05) * 10;
            ctx.font = '28px sans-serif';
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            ctx.fillStyle = `rgba(212, 192, 128, ${pulse})`;
            ctx.fillText('⬇', pos.x, pos.y - 6);
            ctx.shadowBlur = 0;
          } else if (cell === 3) {
            const chestBob = Math.sin(frameCount * 0.04 + x + y) * 2;
            ctx.shadowColor = 'rgba(0,0,0,0.4)';
            ctx.shadowBlur = 8;
            ctx.shadowOffsetY = 4;
            ctx.fillStyle = '#8b6f4c';
            ctx.fillRect(pos.x - 12, pos.y - 8 + chestBob, 24, 16);
            ctx.fillStyle = '#d4af37';
            ctx.fillRect(pos.x - 6, pos.y - 4 + chestBob, 12, 8);
            ctx.fillStyle = '#f5e6b0';
            ctx.fillRect(pos.x - 3, pos.y - 2 + chestBob, 6, 4);
            ctx.shadowBlur = 0;
            ctx.shadowOffsetY = 0;
            const glow = 0.1 + Math.sin(frameCount * 0.03 + x + y) * 0.05;
            ctx.fillStyle = `rgba(255, 215, 0, ${glow})`;
            ctx.beginPath();
            ctx.arc(pos.x, pos.y - 4 + chestBob, 20, 0, Math.PI * 2);
            ctx.fill();
          }
        }
      }
      
      for (let x = 0; x < SIZE; x++) {
        for (let y = 0; y < SIZE; y++) {
          if (dungeon[x][y] === 2) {
            drawAnimatedMonsterIso(x, y, monsterFrame);
          }
        }
      }
      
      drawAnimatedPlayerIso(player.x, player.y, playerFrame);
      drawLootAnimationsIso();
      
      if (gameOver || victory) {
        ctx.fillStyle = 'rgba(0,0,0,0.5)';
        ctx.fillRect(0, 0, CANVAS_SIZE, CANVAS_SIZE);
        ctx.font = 'bold 36px "Segoe UI", sans-serif';
        ctx.fillStyle = victory ? '#b3ffa0' : '#fca5a5';
        ctx.shadowColor = 'black';
        ctx.shadowBlur = 20;
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';
        ctx.fillText(victory ? '✨ ПОБЕДА ✨' : '💀 ПОРАЖЕНИЕ', CANVAS_SIZE/2, CANVAS_SIZE/2);
        ctx.shadowBlur = 0;
      }
    }

    // ---- УПРАВЛЕНИЕ МЕНЮ ----
    function toggleInventory() {
      const overlay = document.getElementById('invOverlay');
      if (overlay.classList.contains('active')) {
        overlay.classList.remove('active');
      } else {
        updateInventoryUI();
        overlay.classList.add('active');
      }
    }

    function toggleStory() {
      const overlay = document.getElementById('storyOverlay');
      overlay.classList.toggle('active');
    }

    function toggleMenu() {
      const overlay = document.getElementById('menuOverlay');
      const isActive = overlay.classList.contains('active');
      if (isActive) {
        overlay.classList.remove('active');
        isPaused = false;
      } else {
        updateUI();
        overlay.classList.add('active');
        isPaused = true;
      }
    }

    // ---- ОБРАБОТЧИК КНОПОК D-PAD ----
    function setupDpad() {
      const btns = document.querySelectorAll('.btn-dpad');
      btns.forEach(btn => {
        btn.addEventListener('click', (e) => {
          e.preventDefault();
          const dir = btn.dataset.dir;
          handleDpadAction(dir);
        });
        btn.addEventListener('touchstart', (e) => {
          e.preventDefault();
          const dir = btn.dataset.dir;
          handleDpadAction(dir);
        }, {passive: false});
      });
    }

    function handleDpadAction(dir) {
      if (gameOver || victory || isPaused) return;
      if (autoMode) stopAutoMode();
      
      switch(dir) {
        case 'up': tryMove(0, -1); break;
        case 'down': tryMove(0, 1); break;
        case 'left': tryMove(-1, 0); break;
        case 'right': tryMove(1, 0); break;
        case 'center': 
          // Атака ближайшего монстра
          if (monsterCount > 0) {
            let nearestMonster = null, nearestDist = Infinity;
            for (let x = 0; x < SIZE; x++) {
              for (let y = 0; y < SIZE; y++) {
                if (dungeon[x][y] === 2) {
                  const dist = Math.abs(x - player.x) + Math.abs(y - player.y);
                  if (dist < nearestDist) { nearestDist = dist; nearestMonster = { x, y }; }
                }
              }
            }
            if (nearestMonster && nearestDist === 1) {
              tryMove(nearestMonster.x - player.x, nearestMonster.y - player.y);
            } else {
              msgDiv.textContent = '🔴 Нет монстра рядом!';
              render();
            }
          } else {
            msgDiv.textContent = '💀 Монстров нет!';
            render();
          }
          break;
        case 'attack':
          if (monsterCount > 0) {
            let nearestMonster = null, nearestDist = Infinity;
            for (let x = 0; x < SIZE; x++) {
              for (let y = 0; y < SIZE; y++) {
                if (dungeon[x][y] === 2) {
                  const dist = Math.abs(x - player.x) + Math.abs(y - player.y);
                  if (dist < nearestDist) { nearestDist = dist; nearestMonster = { x, y }; }
                }
              }
            }
            if (nearestMonster && nearestDist === 1) {
              tryMove(nearestMonster.x - player.x, nearestMonster.y - player.y);
            } else if (nearestMonster && nearestDist > 1) {
              // Идем к монстру
              const dx = Math.sign(nearestMonster.x - player.x);
              const dy = Math.sign(nearestMonster.y - player.y);
              if (dx !== 0) {
                const nx = player.x + dx;
                if (nx >= 0 && nx < SIZE && dungeon[nx][player.y] === 0) {
                  tryMove(dx, 0); break;
                }
              }
              if (dy !== 0) {
                const ny = player.y + dy;
                if (ny >= 0 && ny < SIZE && dungeon[player.x][ny] === 0) {
                  tryMove(0, dy); break;
                }
              }
            } else {
              msgDiv.textContent = '🔴 Нет монстров!';
              render();
            }
          } else {
            msgDiv.textContent = '💀 Монстров нет!';
            render();
          }
          break;
        case 'heal': quickHeal(); break;
        case 'inventory': toggleInventory(); break;
      }
    }

    // ---- КЛАВИАТУРА (для поддержки) ----
    function handleKey(e) {
      const key = e.key;
      if (key === 'Escape') {
        e.preventDefault();
        toggleMenu();
        return;
      }
      if (isPaused) return;
      if (['ArrowUp', 'ArrowDown', 'ArrowLeft', 'ArrowRight'].includes(key)) {
        e.preventDefault();
        if (autoMode) stopAutoMode();
        switch (key) {
          case 'ArrowUp': tryMove(0, -1); break;
          case 'ArrowDown': tryMove(0, 1); break;
          case 'ArrowLeft': tryMove(-1, 0); break;
          case 'ArrowRight': tryMove(1, 0); break;
        }
      }
      if (key === 'a' || key === 'A') toggleAutoMode();
      if (key === 'i' || key === 'I') toggleInventory();
      if (key === 'h' || key === 'H') quickHeal();
      if (key === 'm' || key === 'M') toggleMenu();
    }

    // ---- ИГРОВОЙ ЦИКЛ ----
    function gameLoop() {
      updateLootAnimations();
      updateParticles();
      render();
      requestAnimationFrame(gameLoop);
    }

    // ---- ИНИЦИАЛИЗАЦИЯ ----
    function init() {
      initParticles();
      resetGame();
      
      document.addEventListener('click', function firstClick() {
        if (!audioCtx && !isMusicMuted) createMusic();
        document.removeEventListener('click', firstClick);
      }, { once: true });
      
      window.addEventListener('keydown', handleKey);
      canvas.addEventListener('click', handleCanvasClick);
      canvas.addEventListener('touchstart', handleCanvasClick, {passive: false});
      
      const healBtn = document.createElement('button');
      healBtn.className = 'btn heal';
      healBtn.id = 'healBtn';
      healBtn.textContent = '🧪';
      healBtn.addEventListener('click', quickHeal);
      document.querySelector('.controls').appendChild(healBtn);

      // Настройка D-Pad
      setupDpad();

      document.getElementById('musicBtn').addEventListener('click', toggleMusic);
      document.getElementById('menuBtn').addEventListener('click', toggleMenu);
      document.getElementById('menuResumeBtn').addEventListener('click', toggleMenu);
      document.getElementById('menuStoryBtn').addEventListener('click', () => { toggleMenu(); toggleStory(); });
      document.getElementById('menuInvBtn').addEventListener('click', () => { toggleMenu(); toggleInventory(); });
      document.getElementById('menuAutoBtn').addEventListener('click', () => { toggleMenu(); toggleAutoMode(); });
      document.getElementById('menuMusicBtn').addEventListener('click', () => { toggleMenu(); toggleMusic(); });
      document.getElementById('menuResetBtn').addEventListener('click', () => { toggleMenu(); stopAutoMode(); resetGame(); });

      document.getElementById('resetBtn').addEventListener('click', () => { stopAutoMode(); resetGame(); });
      document.getElementById('downBtn').addEventListener('click', goDown);
      document.getElementById('autoBtn').addEventListener('click', toggleAutoMode);
      document.getElementById('invBtn').addEventListener('click', toggleInventory);
      document.getElementById('invCloseBtn').addEventListener('click', toggleInventory);
      document.getElementById('storyBtn').addEventListener('click', toggleStory);
      document.getElementById('storyStartBtn').addEventListener('click', toggleStory);
      document.getElementById('invOverlay').addEventListener('click', (e) => {
        if (e.target === e.currentTarget) toggleInventory();
      });
      document.getElementById('storyOverlay').addEventListener('click', (e) => {
        if (e.target === e.currentTarget) toggleStory();
      });
      document.getElementById('menuOverlay').addEventListener('click', (e) => {
        if (e.target === e.currentTarget) toggleMenu();
      });
      
      canvas.setAttribute('tabindex', '0');
      canvas.focus();
      
      setTimeout(() => toggleMenu(), 300);
      gameLoop();
    }

    init();
  })();
</script>
</body>
</html>
