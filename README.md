<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>POFO OS 2</title>
<style>
* { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
body {
  font-family: -apple-system, 'Segoe UI', Roboto, Arial, sans-serif;
  background: #1a1a2e;
  color: #fff;
  height: 100vh;
  overflow: hidden;
  user-select: none;
}
#os {
  display: flex;
  flex-direction: column;
  height: 100vh;
  transition: background 0.3s;
}
#statusbar {
  height: 28px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 10px;
  font-size: 12px;
  background: rgba(0,0,0,0.2);
  flex-shrink: 0;
}
#statusbar .left { color: #e94560; font-weight: bold; }
#statusbar .right { color: #fff; }
#desktop {
  flex: 1;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(7, 1fr);
  gap: 6px;
  padding: 8px;
  overflow-y: auto;
}
.app-btn {
  background: #0f3460;
  border: none;
  border-radius: 0;
  color: #fff;
  font-size: 13px;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 4px;
  transition: transform 0.1s, background 0.2s;
  overflow: hidden;
  position: relative;
}
.app-btn:active { transform: scale(0.95); }
.app-btn img { width: 40px; height: 40px; object-fit: cover; border-radius: 0; }
.app-btn .label { margin-top: 4px; font-size: 11px; text-align: center; line-height: 1.1; }

#bottombar {
  height: 54px;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
  background: rgba(0,0,0,0.25);
  flex-shrink: 0;
  padding: 6px;
}
.nav-btn {
  flex: 1;
  height: 44px;
  border-radius: 0;
  border: none;
  font-size: 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.1s;
}
.nav-btn:active { transform: scale(0.95); }
.nav-btn.back { background: #16213e; color: #fff; }
.nav-btn.exit { background: #e94560; color: #fff; }
.nav-btn:disabled { opacity: 0.4; cursor: not-allowed; }

/* Экран приложения */
#app-screen {
  position: absolute;
  top: 0; left: 0; right: 0; bottom: 0;
  background: inherit;
  display: none;
  flex-direction: column;
  z-index: 100;
}
#app-screen.active { display: flex; }
#app-header {
  height: 40px;
  display: flex;
  align-items: center;
  padding: 0 12px;
  background: rgba(0,0,0,0.3);
  color: #e94560;
  font-weight: bold;
  font-size: 16px;
  flex-shrink: 0;
}
#app-content {
  flex: 1;
  overflow-y: auto;
  padding: 10px;
}
#app-footer {
  height: 54px;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
  background: rgba(0,0,0,0.25);
  flex-shrink: 0;
  padding: 6px;
}

/* Общие стили */
.btn {
  background: #e94560;
  color: #fff;
  border: none;
  border-radius: 0;
  padding: 10px 16px;
  font-size: 15px;
  cursor: pointer;
  margin: 4px 0;
  width: 100%;
  transition: transform 0.1s;
}
.btn:active { transform: scale(0.97); }
.btn.secondary { background: #16213e; }
.btn.danger { background: #e74c3c; }
.btn.success { background: #2ecc71; }
.btn:disabled { opacity: 0.5; cursor: not-allowed; }
.input {
  width: 100%;
  padding: 10px;
  border-radius: 0;
  border: none;
  background: #16213e;
  color: #fff;
  font-size: 15px;
  margin: 4px 0;
}
.label { color: #aaa; font-size: 13px; margin: 8px 0 4px; }
.title { color: #e94560; font-size: 22px; text-align: center; margin: 10px 0; }
.divider { height: 1px; background: #444; margin: 12px 0; }
.row { display: flex; align-items: center; gap: 8px; margin: 6px 0; }
.row > * { flex-shrink: 0; }

/* Кликер */
#clicker-area { text-align: center; padding: 20px; }
#clicker-count { font-size: 48px; margin: 10px; }
#clicker-btn {
  width: 140px; height: 140px; border-radius: 0;
  background: #ff6b6b; color: #fff;
  font-size: 60px; font-weight: bold;
  border: none; cursor: pointer; margin: 20px auto;
  display: block; transition: transform 0.1s;
}
#clicker-btn:active { transform: scale(0.92); }

/* Чат */
#chat-messages { flex: 1; overflow-y: auto; padding: 8px; }
.msg { margin: 6px 0; padding: 8px 12px; border-radius: 0; max-width: 85%; }
.msg.bot { background: #16213e; align-self: flex-start; }
.msg.user { background: #2ecc71; align-self: flex-end; margin-left: auto; }
.msg .sender { font-size: 11px; color: #e94560; margin-bottom: 2px; }
.msg.user .sender { color: #fff; }
#chat-input-row { display: flex; gap: 6px; padding: 8px; }
#chat-input { flex: 1; }

/* Галерея */
.gallery-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 6px;
}
.gallery-grid img {
  width: 100%; height: 100px; object-fit: cover;
  border-radius: 0; cursor: pointer;
}

/* Обои */
.wallpaper-row {
  display: flex; align-items: center; gap: 10px;
  margin: 8px 0; padding: 6px; background: #16213e;
  border-radius: 0;
}
.wallpaper-swatch {
  width: 40px; height: 40px; border-radius: 0;
  flex-shrink: 0;
}

/* Админ */
.user-row {
  display: flex; align-items: center; justify-content: space-between;
  padding: 8px; background: #16213e; border-radius: 0;
  margin: 4px 0; font-size: 14px;
}
.user-row .ban-btn {
  padding: 6px 12px; border: none; border-radius: 0;
  font-size: 12px; cursor: pointer;
}

/* Модалка */
.modal-overlay {
  position: fixed; top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(0,0,0,0.7);
  display: none; align-items: center; justify-content: center;
  z-index: 1000;
}
.modal-overlay.active { display: flex; }
.modal {
  background: #16213e; border-radius: 0;
  padding: 20px; width: 85%; max-width: 400px;
  max-height: 80vh; overflow-y: auto;
}
.modal h3 { color: #e94560; margin-bottom: 12px; text-align: center; }

/* Паника */
.panic-overlay {
  position: fixed; top: 0; left: 0; right: 0; bottom: 0;
  background: #ff0000;
  display: none; flex-direction: column;
  align-items: center; justify-content: center;
  z-index: 2000; color: #fff; text-align: center; padding: 20px;
}
.panic-overlay.active { display: flex; }
.panic-overlay h2 { font-size: 28px; margin-bottom: 20px; }
</style>
</head>
<body>

<div id="os">
  <div id="statusbar">
    <span class="left" id="os-name">POFO OS 2.0</span>
    <span class="right" id="clock">--:--:--</span>
  </div>
  <div id="desktop"></div>
  <div id="bottombar">
    <button class="nav-btn back" id="nav-back" disabled>◀</button>
    <button class="nav-btn exit" id="nav-exit" disabled>⏺</button>
  </div>
</div>

<div id="app-screen">
  <div id="app-header"></div>
  <div id="app-content"></div>
  <div id="app-footer">
    <button class="nav-btn back" id="app-back">◀</button>
    <button class="nav-btn exit" id="app-exit">⏺</button>
  </div>
</div>

<div class="modal-overlay" id="modal-overlay">
  <div class="modal" id="modal-content"></div>
</div>

<div class="panic-overlay" id="panic-overlay">
  <h2>🚨 СИСТЕМА В ПАНИКЕ 🚨</h2>
  <p>Нажмите кнопку ниже для выхода</p>
  <button class="btn danger" style="max-width:200px;margin-top:20px;" onclick="exitPanic()">🚪 ВЫЙТИ ИЗ СИСТЕМЫ</button>
</div>

<script>
// ============ ДАННЫЕ ============
const COLORS = {
  bg: '#1a1a2e', card: '#16213e', accent: '#e94560',
  text: '#fff', textSec: '#aaa', appIcon: '#0f3460',
};
const WALLPAPERS = [
  {name:'Тёмный', color:'#1a1a2e', id:'dark', text:'#fff'},
  {name:'Синий', color:'#0c2461', id:'blue', text:'#fff'},
  {name:'Фиолетовый', color:'#2c1a4d', id:'purple', text:'#fff'},
  {name:'Зелёный', color:'#0a3d2a', id:'green', text:'#fff'},
  {name:'Красный', color:'#4a0a0a', id:'red', text:'#fff'},
  {name:'Оранжевый', color:'#4a2a0a', id:'orange', text:'#fff'},
  {name:'Серый', color:'#2a2a2a', id:'gray', text:'#fff'},
  {name:'Бирюзовый', color:'#0a3d3d', id:'teal', text:'#fff'},
  {name:'Белый', color:'#f0f0f0', id:'white', text:'#333'},
  {name:'Коричневый', color:'#5d4037', id:'brown', text:'#fff'},
  {name:'Голубой', color:'#4fc3f7', id:'lightblue', text:'#333'},
  {name:'Розовый', color:'#f06292', id:'pink', text:'#333'},
  {name:'Паника', color:'#ff0000', id:'panic', text:'#fff'},
];
const USERS = {
  danpro: {name:'Данил Г.', status:'Владелец'},
  koshak_12: {name:'Артём К.', status:'Админ'},
  POFO_official82783: {name:'Тимофей У.', status:'Админ'},
  user: {name:'Обычный', status:'Участник'},
};
const BASE_APPS = [
  {name:'POFO Calc', id:'calc'},
  {name:'POFO Notes', id:'notes'},
  {name:'POFO System', id:'sysinfo'},
  {name:'POFO Play', id:'play'},
  {name:'POFO Settings', id:'settings'},
];
const STORE_APPS = [
  {name:'POFO AI Chat', id:'chat'},
  {name:'POFO Camera', id:'camera'},
  {name:'POFO Music', id:'music'},
  {name:'POFO TV', id:'tv'},
  {name:'POFO Browser', id:'browser'},
  {name:'POFO Maps', id:'maps'},
  {name:'POFO Gallery', id:'gallery'},
  {name:'POFO Weather', id:'weather'},
  {name:'POFO Removal', id:'removal'},
  {name:'POFO Wallpaper', id:'wallpaper'},
  {name:'POFO Clicker', id:'clicker'},
];
const ROOT_APPS = [{name:'POFO Icon', id:'icon'}];

// ============ СОСТОЯНИЕ ============
const state = {
  currentUser: null,
  currentUserName: '',
  currentUserStatus: '',
  theme: 'dark',
  wallpaper: 'dark',
  customWallpaper: null,
  installedApps: [],
  appNames: {},
  appIcons: {},
  bannedUsers: [],
  customChannels: [],
  clickerCount: 0,
  clickerDate: new Date().toDateString(),
  panicMode: false,
  backupApps: [],
  notesText: '',
  currentApp: null,
  appHistory: [],
};

// ============ ХРАНИЛИЩЕ ============
function save(key, val) {
  try { localStorage.setItem('pofo_'+key, JSON.stringify(val)); } catch(e){}
}
function load(key, def) {
  try {
    const v = localStorage.getItem('pofo_'+key);
    return v ? JSON.parse(v) : def;
  } catch(e){ return def; }
}
function saveState() {
  save('user', state.currentUser);
  save('theme', state.theme);
  save('wallpaper', state.wallpaper);
  save('customWallpaper', state.customWallpaper);
  save('installedApps', state.installedApps);
  save('appNames', state.appNames);
  save('appIcons', state.appIcons);
  save('bannedUsers', state.bannedUsers);
  save('customChannels', state.customChannels);
  save('notesText', state.notesText);
  save('clickerCount', state.clickerCount);
  save('clickerDate', state.clickerDate);
}
function loadState() {
  state.currentUser = load('user', null);
  state.theme = load('theme', 'dark');
  state.wallpaper = load('wallpaper', 'dark');
  state.customWallpaper = load('customWallpaper', null);
  state.installedApps = load('installedApps', []);
  state.appNames = load('appNames', {});
  state.appIcons = load('appIcons', {});
  state.bannedUsers = load('bannedUsers', []);
  state.customChannels = load('customChannels', []);
  state.notesText = load('notesText', '');
  state.clickerCount = load('clickerCount', 0);
  state.clickerDate = load('clickerDate', new Date().toDateString());
  
  if (state.currentUser && USERS[state.currentUser]) {
    const u = USERS[state.currentUser];
    state.currentUserName = u.name;
    state.currentUserStatus = u.status;
    if (state.bannedUsers.includes(state.currentUser)) {
      state.currentUser = null;
      state.currentUserName = '';
      state.currentUserStatus = '';
    }
  } else {
    state.currentUser = null;
  }
}

// ============ ХЕЛПЕРЫ ============
function getTextColor() {
  if (state.theme === 'light') return '#000';
  if (state.customWallpaper) return '#fff';
  const wp = WALLPAPERS.find(w => w.id === state.wallpaper);
  return wp ? wp.text : '#fff';
}
function getWallpaperColor() {
  if (state.theme === 'light') return '#fff';
  if (state.customWallpaper) return null;
  const wp = WALLPAPERS.find(w => w.id === state.wallpaper);
  return wp ? wp.color : '#1a1a2e';
}
function getAllApps() {
  if (state.panicMode) return [{name:'EXIT', id:'exit'}];
  const apps = [];
  BASE_APPS.forEach(a => {
    const c = {...a};
    if (state.appNames[c.id]) c.name = state.appNames[c.id];
    c.icon = state.appIcons[c.id];
    apps.push(c);
  });
  state.installedApps.forEach(a => {
    const c = {...a};
    if (state.appNames[c.id]) c.name = state.appNames[c.id];
    c.icon = state.appIcons[c.id];
    apps.push(c);
  });
  if (state.currentUser && ['Админ','Владелец'].includes(state.currentUserStatus)) {
    const admin = {name:'Admin Panel', id:'admin'};
    if (state.appNames['admin']) admin.name = state.appNames['admin'];
    admin.icon = state.appIcons['admin'];
    apps.push(admin);
  }
  return apps;
}
function updateClock() {
  const now = new Date();
  document.getElementById('clock').textContent =
    String(now.getHours()).padStart(2,'0') + ':' +
    String(now.getMinutes()).padStart(2,'0') + ':' +
    String(now.getSeconds()).padStart(2,'0');
}

// ============ РЕНДЕР РАБОЧЕГО СТОЛА ============
function renderDesktop() {
  const os = document.getElementById('os');
  const wallpaperColor = getWallpaperColor();
  if (state.customWallpaper) {
    os.style.background = `url('${state.customWallpaper}') center/cover no-repeat`;
  } else {
    os.style.background = wallpaperColor;
  }
  
  document.getElementById('os-name').textContent = 'POFO OS 2.0' +
    (state.currentUser ? ` | ${state.currentUserName} (${state.currentUserStatus})` : '');
  
  const desk = document.getElementById('desktop');
  desk.innerHTML = '';
  const apps = getAllApps();
  const totalCells = 28;
  for (let i = 0; i < totalCells; i++) {
    if (i < apps.length) {
      const app = apps[i];
      const btn = document.createElement('button');
      btn.className = 'app-btn';
      btn.onclick = () => launchApp(app);
      if (app.icon) {
        const img = document.createElement('img');
        img.src = app.icon;
        btn.appendChild(img);
        const lbl = document.createElement('div');
        lbl.className = 'label';
        lbl.textContent = app.name;
        btn.appendChild(lbl);
      } else {
        btn.textContent = app.name;
        btn.style.fontSize = '12px';
      }
      desk.appendChild(btn);
    } else {
      const empty = document.createElement('div');
      desk.appendChild(empty);
    }
  }
  
  document.getElementById('nav-back').disabled = true;
  document.getElementById('nav-exit').disabled = true;
}

// ============ ЗАПУСК ПРИЛОЖЕНИЙ ============
function launchApp(app) {
  state.currentApp = app.id;
  state.appHistory = [];
  const appScreen = document.getElementById('app-screen');
  appScreen.classList.add('active');
  if (state.customWallpaper) {
    appScreen.style.background = `url('${state.customWallpaper}') center/cover no-repeat`;
  } else {
    appScreen.style.background = getWallpaperColor() || '#1a1a2e';
  }
  document.getElementById('app-header').textContent = app.name;
  document.getElementById('app-back').disabled = false;
  document.getElementById('app-exit').disabled = false;
  
  const content = document.getElementById('app-content');
  content.innerHTML = '';
  
  switch(app.id) {
    case 'calc': renderCalc(content); break;
    case 'notes': renderNotes(content); break;
    case 'sysinfo': renderSysinfo(content); break;
    case 'play': renderPlay(content); break;
    case 'settings': renderSettings(content); break;
    case 'chat': renderChat(content); break;
    case 'removal': renderRemoval(content); break;
    case 'wallpaper': renderWallpaper(content); break;
    case 'icon': renderIcon(content); break;
    case 'clicker': renderClicker(content); break;
    case 'admin': renderAdmin(content); break;
    case 'exit': renderExit(content); break;
    case 'gallery': renderGallery(content); break;
    case 'tv': renderTV(content); break;
    case 'browser': renderBrowser(content); break;
    default:
      content.innerHTML = `<div style="text-align:center;padding:40px;color:#aaa;">
        <div style="font-size:40px;margin-bottom:20px;">📱</div>
        <div>${app.name}</div><div style="margin-top:10px;">Приложение установлено!</div>
      </div>`;
  }
}

function closeApp() {
  document.getElementById('app-screen').classList.remove('active');
  state.currentApp = null;
  state.appHistory = [];
  renderDesktop();
}

// ============ СТРАНИЦЫ ПРИЛОЖЕНИЙ ============
function renderCalc(c) {
  c.innerHTML = `
    <div style="display:flex;flex-direction:column;height:100%;gap:8px;">
      <input class="input" id="calc-display" readonly value="0" style="font-size:28px;text-align:right;">
      <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:6px;flex:1;">
        ${['7','8','9','/','4','5','6','*','1','2','3','-','C','0','=','+'].map(k =>
          `<button class="btn secondary" style="font-size:20px;" onclick="calcPress('${k}')">${k}</button>`
        ).join('')}
      </div>
    </div>`;
}
function calcPress(k) {
  const d = document.getElementById('calc-display');
  if (k === 'C') { d.value = '0'; return; }
  if (k === '=') {
    try { d.value = eval(d.value); } catch(e) { d.value = 'Ошибка'; }
    return;
  }
  if (d.value === '0' && /\d/.test(k)) d.value = k;
  else d.value += k;
}

function renderNotes(c) {
  c.innerHTML = `
    <textarea class="input" style="height:70vh;" placeholder="Введите заметку..."
      oninput="state.notesText=this.value;saveState()">${state.notesText}</textarea>
    <button class="btn danger" onclick="state.notesText='';document.querySelector('textarea').value='';saveState()">🗑 Очистить</button>`;
}

function renderSysinfo(c) {
  const acc = state.currentUser
    ? `👤 ${state.currentUserName} (${state.currentUserStatus})\n🔑 ${state.currentUser}`
    : '🔒 Не авторизован';
  c.innerHTML = `
    <div class="title">POFO OS 2.0</div>
    <div style="text-align:center;white-space:pre-line;margin:10px 0;">${acc}</div>
    <button class="btn ${state.currentUser ? 'danger' : 'success'}"
      onclick="${state.currentUser ? 'logout()' : 'showLogin()'}">
      ${state.currentUser ? '🚪 Выйти' : '🔐 Войти'}</button>
    <div class="divider"></div>
    <div class="row"><span class="label">Система:</span> Android 14</div>
    <div class="row"><span class="label">Версия ОС:</span> POFO OS 2.0</div>
    <div class="row"><span class="label">Устройство:</span> POFO R50 Pro</div>
    <div class="row"><span class="label">Процессор:</span> Dimensity 8500-Ultra</div>
    <div class="row"><span class="label">Ядро:</span> Linux 5.15</div>
    <div class="row"><span class="label">Архитектура:</span> ARM64 (aarch64)</div>
    <div class="row"><span class="label">Root:</span> ${window.rootEnabled ? '✅ Включён' : '❌ Выключен'}</div>`;
}

function renderPlay(c) {
  let html = `<div class="title">POFO Play</div>
    <div style="text-align:center;color:#aaa;margin-bottom:10px;">Магазин приложений</div>`;
  STORE_APPS.forEach(app => {
    const installed = state.installedApps.find(a => a.id === app.id);
    html += `<button class="btn ${installed ? 'success' : 'secondary'}"
      ${installed ? 'disabled' : ''}
      onclick="installApp('${app.id}','${app.name}')">
      ${app.name} ${installed ? '✅' : '📥'}</button>`;
  });
  if (window.rootEnabled) {
    html += `<div class="divider"></div><div style="text-align:center;color:#2ecc71;">═══ ROOT-приложения ═══</div>`;
    ROOT_APPS.forEach(app => {
      const installed = state.installedApps.find(a => a.id === app.id);
      html += `<button class="btn ${installed ? 'success' : 'danger'}"
        ${installed ? 'disabled' : ''}
        onclick="installApp('${app.id}','${app.name}')">
        🔓 ${app.name} ${installed ? '✅' : '📥'}</button>`;
    });
  }
  c.innerHTML = html;
}
function installApp(id, name) {
  if (state.installedApps.find(a => a.id === id)) return;
  state.installedApps.push({id, name});
  saveState();
  closeApp();
  alert(`${name} установлен!`);
}

function renderSettings(c) {
  let html = `<div class="title">⚙️ POFO Settings</div>
    <div style="text-align:center;color:#e94560;font-size:22px;margin:10px 0;">POFO OS 2.0</div>
    <div class="divider"></div>
    <div class="row"><span class="label">Root права:</span> ${window.rootEnabled ? '✅ Включены' : '❌ Выключены'}</div>
    <button class="btn ${window.rootEnabled ? 'success' : 'secondary'}"
      ${window.rootEnabled ? 'disabled' : ''} onclick="showRootPassword()">
      ${window.rootEnabled ? '🔒 Root уже включён' : '🔓 Включить Root права'}</button>
    <div class="divider"></div>`;
  if (window.rootEnabled) {
    html += `
      <div class="label">📝 Изменить название ОС</div>
      <input class="input" id="os-name-input" value="POFO OS 2.0">
      <button class="btn secondary" onclick="changeOSName()">💾 Сохранить название</button>
      <button class="btn danger" onclick="resetOSName()">↺ Сбросить на POFO OS 2.0</button>
      <div class="divider"></div>
      <div class="row"><span class="label">Тема:</span> ${state.theme === 'light' ? 'Светлая' : 'Тёмная'}</div>
      <button class="btn secondary" onclick="toggleTheme()">🔄 Переключить тему</button>
      <div class="divider"></div>`;
  }
  html += `
    <button class="btn secondary" onclick="checkUpdate()">📡 Проверить обновление</button>
    <button class="btn danger" onclick="factoryReset()">🔄 Сбросить до заводских</button>
    <div style="text-align:center;color:#aaa;font-size:12px;margin-top:10px;">
      Установлено приложений: ${state.installedApps.length}</div>`;
  c.innerHTML = html;
}

function renderChat(c) {
  c.innerHTML = `
    <div id="chat-messages" style="height:55vh;overflow-y:auto;background:#0d1421;border-radius:0;padding:8px;"></div>
    <div id="chat-input-row">
      <input class="input" id="chat-input" placeholder="Введите сообщение...">
      <button class="btn" onclick="sendChat()">📤</button>
    </div>
    <div style="display:grid;grid-template-columns:repeat(10,1fr);gap:2px;margin-top:8px;">
      ${'1234567890qwertyuiopasdfghjklzxcvbnm'.split('').map(k =>
        `<button class="btn secondary" style="padding:4px;font-size:12px;"
          onclick="chatKey('${k}')">${k}</button>`
      ).join('')}
      <button class="btn secondary" style="padding:4px;font-size:12px;" onclick="chatKey(' ')">␣</button>
      <button class="btn danger" style="padding:4px;font-size:12px;" onclick="chatKey('⌫')">⌫</button>
    </div>`;
  addChatMessage('🤖 POFO AI', 'Привет! Я POFO AI. Задай мне вопрос.', true);
}
function chatKey(k) {
  const inp = document.getElementById('chat-input');
  if (k === '⌫') inp.value = inp.value.slice(0,-1);
  else inp.value += k;
}
function addChatMessage(sender, text, isBot) {
  const box = document.getElementById('chat-messages');
  if (!box) return;
  const div = document.createElement('div');
  div.className = 'msg ' + (isBot ? 'bot' : 'user');
  div.innerHTML = `<div class="sender">${sender}</div><div>${text}</div>`;
  box.appendChild(div);
  box.scrollTop = box.scrollHeight;
}
function sendChat() {
  const inp = document.getElementById('chat-input');
  const msg = inp.value.trim();
  if (!msg) return;
  addChatMessage('👤 Вы', msg, false);
  inp.value = '';
  setTimeout(() => aiRespond(msg), 500);
}
function aiRespond(msg) {
  const m = msg.toLowerCase();
  let r;
  if (/привет|здравствуй/.test(m)) r = 'Привет! Рад тебя видеть!';
  else if (/как дела|как ты/.test(m)) r = 'У меня всё отлично! А у тебя?';
  else if (/имя|зовут/.test(m)) r = 'Меня зовут POFO AI!';
  else if (/погода/.test(m)) r = 'Сегодня отличная погода! ☀️';
  else if (/спасибо/.test(m)) r = 'Пожалуйста! 😊';
  else if (/пока|до свидания/.test(m)) r = 'До встречи! 👋';
  else r = `Интересный вопрос! Я запомнил: '${msg}'.`;
  addChatMessage('🤖 POFO AI', r, true);
}

function renderRemoval(c) {
  if (state.installedApps.length === 0) {
    c.innerHTML = '<div style="text-align:center;color:#aaa;padding:40px;">Нет установленных приложений</div>';
    return;
  }
  let html = '<div class="title">🗑️ POFO Removal</div>';
  state.installedApps.forEach(app => {
    html += `<div class="user-row">
      <span>${app.name}</span>
      <button class="ban-btn danger" onclick="uninstallApp('${app.id}')">🗑️</button>
    </div>`;
  });
  c.innerHTML = html;
}
function uninstallApp(id) {
  if (!confirm('Удалить приложение?')) return;
  state.installedApps = state.installedApps.filter(a => a.id !== id);
  saveState();
  closeApp();
}

function renderWallpaper(c) {
  let html = `<div class="title">🖼️ POFO Wallpaper</div>
    <div style="text-align:center;color:#aaa;margin-bottom:10px;">Выберите обои</div>
    <button class="btn" onclick="pickWallpaperImage()">🖼️ Выбрать из галереи</button>`;
  if (state.customWallpaper) {
    html += `<button class="btn danger" onclick="resetCustomWallpaper()">↺ Сбросить пользовательские обои</button>`;
  }
  html += '<div class="divider"></div>';
  WALLPAPERS.forEach(wp => {
    const active = state.wallpaper === wp.id && !state.customWallpaper;
    html += `<div class="wallpaper-row">
      <div class="wallpaper-swatch" style="background:${wp.color};"></div>
      <span style="flex:1;">${wp.name}</span>
      <button class="btn ${active ? 'success' : 'secondary'}" style="width:auto;padding:6px 12px;font-size:13px;"
        ${active ? 'disabled' : ''} onclick="setWallpaper('${wp.id}')">
        ${active ? '✅' : 'Применить'}</button>
    </div>`;
  });
  c.innerHTML = html;
}
function setWallpaper(id) {
  state.wallpaper = id;
  state.customWallpaper = null;
  saveState();
  closeApp();
}
function resetCustomWallpaper() {
  state.customWallpaper = null;
  state.wallpaper = 'dark';
  saveState();
  closeApp();
}
function pickWallpaperImage() {
  const inp = document.createElement('input');
  inp.type = 'file';
  inp.accept = 'image/*';
  inp.onchange = e => {
    const file = e.target.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = ev => {
      state.customWallpaper = ev.target.result;
      saveState();
      closeApp();
    };
    reader.readAsDataURL(file);
  };
  inp.click();
}

function renderIcon(c) {
  let html = `<div class="title">🎨 POFO Icon</div>
    <div style="text-align:center;color:#aaa;margin-bottom:10px;">Названия и иконки приложений</div>
    <button class="btn danger" onclick="resetAllIcons()">🗑️ Сбросить все иконки</button>
    <div class="divider"></div>`;
  const all = [...BASE_APPS, ...state.installedApps];
  all.forEach(app => {
    const name = state.appNames[app.id] || app.name;
    const icon = state.appIcons[app.id];
    html += `<div style="background:#16213e;border-radius:0;padding:10px;margin:6px 0;">
      <div class="row">
        <div style="width:40px;height:40px;background:#0f3460;border-radius:0;display:flex;align-items:center;justify-content:center;overflow:hidden;">
          ${icon ? `<img src="${icon}" style="width:100%;height:100%;object-fit:cover;">` : '📁'}
        </div>
        <input class="input" id="name-${app.id}" value="${name}" style="flex:1;">
      </div>
      <div class="row" style="margin-top:6px;">
        <button class="btn secondary" style="font-size:12px;" onclick="pickIcon('${app.id}')">🖼️ Иконка</button>
        <button class="btn secondary" style="font-size:12px;" onclick="applyName('${app.id}')">💾 Имя</button>
        <button class="btn danger" style="font-size:12px;" onclick="resetName('${app.id}')">↺</button>
        ${icon ? `<button class="btn danger" style="font-size:12px;" onclick="resetIcon('${app.id}')">🗑</button>` : ''}
      </div>
    </div>`;
  });
  c.innerHTML = html;
}
function pickIcon(appId) {
  const inp = document.createElement('input');
  inp.type = 'file';
  inp.accept = 'image/*';
  inp.onchange = e => {
    const file = e.target.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = ev => {
      state.appIcons[appId] = ev.target.result;
      saveState();
      closeApp();
      setTimeout(() => launchApp({id:'icon',name:'POFO Icon'}), 100);
    };
    reader.readAsDataURL(file);
  };
  inp.click();
}
function applyName(appId) {
  const val = document.getElementById('name-'+appId).value.trim();
  if (!val) return;
  state.appNames[appId] = val;
  saveState();
  closeApp();
  setTimeout(() => launchApp({id:'icon',name:'POFO Icon'}), 100);
}
function resetName(appId) {
  delete state.appNames[appId];
  saveState();
  closeApp();
  setTimeout(() => launchApp({id:'icon',name:'POFO Icon'}), 100);
}
function resetIcon(appId) {
  delete state.appIcons[appId];
  saveState();
  closeApp();
  setTimeout(() => launchApp({id:'icon',name:'POFO Icon'}), 100);
}
function resetAllIcons() {
  if (!confirm('Сбросить все иконки?')) return;
  state.appIcons = {};
  saveState();
  closeApp();
  setTimeout(() => launchApp({id:'icon',name:'POFO Icon'}), 100);
}

function renderClicker(c) {
  if (new Date().toDateString() !== state.clickerDate) {
    state.clickerCount = 0;
    state.clickerDate = new Date().toDateString();
    saveState();
  }
  const rem = 100 - state.clickerCount;
  c.innerHTML = `<div id="clicker-area">
    <div style="color:#ff6b6b;font-size:22px;font-weight:bold;">🖱️ POFO Clicker</div>
    <div id="clicker-count">${state.clickerCount}</div>
    <div style="color:#aaa;">Осталось: ${rem} / 100</div>
    <button id="clicker-btn" onclick="clickerPress()">O</button>
    <div style="color:#aaa;font-size:12px;">Лимит: 100 нажатий в день</div>
  </div>`;
}
function clickerPress() {
  if (new Date().toDateString() !== state.clickerDate) {
    state.clickerCount = 0;
    state.clickerDate = new Date().toDateString();
  }
  if (state.clickerCount >= 100) {
    alert('Лимит на сегодня исчерпан (100 нажатий)');
    return;
  }
  state.clickerCount++;
  saveState();
  const cnt = document.getElementById('clicker-count');
  if (cnt) cnt.textContent = state.clickerCount;
  const rem = 100 - state.clickerCount;
  const remEl = document.querySelector('#clicker-area div[style*="color:#aaa"]');
  if (remEl && remEl.textContent.includes('Осталось')) remEl.textContent = `Осталось: ${rem} / 100`;
}

function renderAdmin(c) {
  let html = `<div class="title">🛠️ Admin Panel</div>
    <div style="text-align:center;margin-bottom:10px;">
      ${state.currentUserName} (${state.currentUserStatus})
    </div>
    <div class="divider"></div>
    <div class="row"><span class="label">Тема:</span> ${state.theme==='light'?'Светлая':'Тёмная'}</div>
    <button class="btn secondary" onclick="toggleTheme()">🔄 Переключить тему</button>
    <div class="divider"></div>
    <button class="btn danger" onclick="adminClearApps()">🗑️ Очистить все приложения</button>
    <button class="btn secondary" onclick="adminResetSettings()">🔄 Сбросить настройки</button>
    <button class="btn secondary" onclick="state.notesText='';saveState();alert('Заметки очищены')">📝 Очистить заметки</button>
    <button class="btn secondary" onclick="alert('Система перезагружается... (демо)')">🔄 Перезагрузить (демо)</button>
    <button class="btn danger" onclick="triggerPanic()">📥 Получить новую версию 2.1</button>
    <div class="divider"></div>
    <div class="label">👥 Управление пользователями (только участники):</div>`;
  
  Object.entries(USERS).forEach(([login, data]) => {
    if (['Админ','Владелец'].includes(data.status)) return;
    const banned = state.bannedUsers.includes(login);
    html += `<div class="user-row">
      <span>${login} — ${data.name} (${data.status}) ${banned?'⛔':'✅'}</span>
      <button class="ban-btn ${banned ? 'success' : 'danger'}"
        onclick="${banned ? `unbanUser('${login}')` : `banUser('${login}')`}">
        ${banned ? '🔓 Разбан' : '🚫 Бан'}
      </button>
    </div>`;
  });
  if (!Object.values(USERS).some(u => u.status === 'Участник')) {
    html += '<div style="color:#aaa;text-align:center;padding:10px;">Нет участников</div>';
  }
  c.innerHTML = html;
}
function banUser(login) {
  if (!state.bannedUsers.includes(login)) {
    state.bannedUsers.push(login);
    saveState();
    closeApp();
    setTimeout(() => launchApp({id:'admin',name:'Admin Panel'}), 100);
  }
}
function unbanUser(login) {
  state.bannedUsers = state.bannedUsers.filter(u => u !== login);
  saveState();
  closeApp();
  setTimeout(() => launchApp({id:'admin',name:'Admin Panel'}), 100);
}
function adminClearApps() {
  if (!confirm('Удалить все установленные приложения?')) return;
  state.installedApps = [];
  saveState();
  closeApp();
}
function adminResetSettings() {
  state.wallpaper = 'dark';
  state.customWallpaper = null;
  state.appNames = {};
  if (state.theme === 'light') { state.theme = 'dark'; }
  saveState();
  closeApp();
}

function renderExit(c) {
  c.innerHTML = `<div style="text-align:center;padding:40px;">
    <div style="font-size:48px;">🚨</div>
    <div style="font-size:22px;color:#e74c3c;margin:20px 0;">СИСТЕМА В ПАНИКЕ</div>
    <button class="btn danger" style="max-width:200px;margin:20px auto;"
      onclick="exitPanic()">🚪 ВЫЙТИ ИЗ СИСТЕМЫ</button>
  </div>`;
}

function renderGallery(c) {
  c.innerHTML = `<div class="title">🖼️ POFO Gallery</div>
    <div style="text-align:center;color:#aaa;margin-bottom:10px;">Выберите фото из устройства</div>
    <button class="btn" onclick="pickGalleryImage()">📂 Выбрать фото</button>
    <div id="gallery-preview" style="margin-top:10px;"></div>`;
}
function pickGalleryImage() {
  const inp = document.createElement('input');
  inp.type = 'file';
  inp.accept = 'image/*';
  inp.multiple = true;
  inp.onchange = e => {
    const preview = document.getElementById('gallery-preview');
    if (!preview) return;
    preview.innerHTML = '<div class="gallery-grid"></div>';
    const grid = preview.querySelector('.gallery-grid');
    Array.from(e.target.files).forEach(file => {
      const reader = new FileReader();
      reader.onload = ev => {
        const img = document.createElement('img');
        img.src = ev.target.result;
        img.onclick = () => showFullImage(ev.target.result);
        grid.appendChild(img);
      };
      reader.readAsDataURL(file);
    });
  };
  inp.click();
}
function showFullImage(src) {
  showModal(`<img src="${src}" style="width:100%;border-radius:0;">`, 'Просмотр', [
    {text:'Закрыть', cls:'secondary', action: closeModal}
  ]);
}

function renderTV(c) {
  c.innerHTML = `<div class="title">📺 POFO TV</div>
    <div style="text-align:center;color:#aaa;margin-bottom:10px;">Добавьте свой канал (m3u8)</div>
    <button class="btn" onclick="addChannelDialog()">➕ Добавить канал</button>
    <div class="divider"></div>
    <div id="channels-list"></div>`;
  renderChannels();
}
function renderChannels() {
  const list = document.getElementById('channels-list');
  if (!list) return;
  if (state.customChannels.length === 0) {
    list.innerHTML = '<div style="text-align:center;color:#aaa;padding:20px;">Нет каналов. Добавьте свой!</div>';
    return;
  }
  list.innerHTML = state.customChannels.map((ch, i) =>
    `<div class="user-row">
      <span>📺 ${ch.name}</span>
      <div>
        <button class="ban-btn success" onclick="playChannel(${i})">▶</button>
        <button class="ban-btn danger" onclick="removeChannel(${i})">✕</button>
      </div>
    </div>`
  ).join('');
}
function addChannelDialog() {
  showModal(`
    <input class="input" id="ch-name" placeholder="Название канала">
    <input class="input" id="ch-url" placeholder="Ссылка m3u8">
  `, 'Добавить канал', [
    {text:'Отмена', cls:'secondary', action: closeModal},
    {text:'Добавить', cls:'success', action: () => {
      const name = document.getElementById('ch-name').value.trim();
      const url = document.getElementById('ch-url').value.trim();
      if (!name || !url) return alert('Заполните оба поля!');
      state.customChannels.push({name, url});
      saveState();
      closeModal();
      renderChannels();
    }}
  ]);
}
function removeChannel(i) {
  state.customChannels.splice(i,1);
  saveState();
  renderChannels();
}
function playChannel(i) {
  const ch = state.customChannels[i];
  showModal(`<div style="text-align:center;">
    <div style="font-size:30px;margin:10px;">📺</div>
    <div style="color:#e94560;font-weight:bold;">${ch.name}</div>
    <video src="${ch.url}" controls autoplay style="width:100%;margin-top:10px;border-radius:0;"></video>
  </div>`, ch.name, [{text:'Закрыть', cls:'danger', action: closeModal}]);
}

function renderBrowser(c) {
  c.innerHTML = `<div class="title">🌐 POFO Browser</div>
    <input class="input" id="browser-url" placeholder="URL или поисковый запрос">
    <div class="row">
      <button class="btn" onclick="browse()">Перейти</button>
      <button class="btn secondary" onclick="browserBack()">◀ Назад</button>
    </div>
    <div id="browser-frame" style="margin-top:10px;height:60vh;background:#000;border-radius:0;display:flex;align-items:center;justify-content:center;color:#aaa;text-align:center;padding:20px;">
      Введите URL. Из-за ограничений браузера некоторые сайты могут не загрузиться.
    </div>`;
}
function browse() {
  const url = document.getElementById('browser-url').value.trim();
  if (!url) return;
  const frame = document.getElementById('browser-frame');
  let target = url;
  if (!/^https?:\/\//.test(url)) {
    target = 'https://www.google.com/search?q=' + encodeURIComponent(url);
  }
  frame.innerHTML = `<iframe src="${target}" style="width:100%;height:100%;border:none;border-radius:0;"></iframe>`;
}
function browserBack() {
  const frame = document.getElementById('browser-frame');
  try { frame.querySelector('iframe').contentWindow.history.back(); } catch(e) { alert('Нет истории'); }
}

// ============ ЛОГИН ============
function showLogin() {
  showModal(`
    <input class="input" id="login-input" placeholder="Логин">
  `, '🔐 Вход в аккаунт', [
    {text:'Отмена', cls:'secondary', action: closeModal},
    {text:'Войти', cls:'success', action: () => {
      const login = document.getElementById('login-input').value.trim();
      if (state.bannedUsers.includes(login)) return alert('❌ Вы забанены!');
      if (USERS[login]) {
        state.currentUser = login;
        state.currentUserName = USERS[login].name;
        state.currentUserStatus = USERS[login].status;
        saveState();
        closeModal();
        closeApp();
        alert(`Добро пожаловать, ${state.currentUserName}!`);
      } else {
        alert('Неверный логин!');
      }
    }}
  ]);
}
function logout() {
  state.currentUser = null;
  state.currentUserName = '';
  state.currentUserStatus = '';
  saveState();
  closeApp();
}

// ============ МОДАЛКА ============
function showModal(html, title = '', buttons = []) {
  const ov = document.getElementById('modal-overlay');
  const mc = document.getElementById('modal-content');
  mc.innerHTML = `<h3>${title}</h3>${html}<div id="modal-buttons" style="margin-top:15px;"></div>`;
  const bc = document.getElementById('modal-buttons');
  buttons.forEach(b => {
    const btn = document.createElement('button');
    btn.className = 'btn ' + (b.cls || '');
    btn.textContent = b.text;
    btn.onclick = b.action;
    bc.appendChild(btn);
  });
  ov.classList.add('active');
}
function closeModal() {
  document.getElementById('modal-overlay').classList.remove('active');
}

// ============ ROOT, ТЕМА, ПАНИКА ============
window.rootEnabled = false;

function showRootPassword() {
  showModal(`
    <input class="input" id="root-pass" type="password" placeholder="Пароль">
  `, '🔓 Root права', [
    {text:'Отмена', cls:'secondary', action: closeModal},
    {text:'Подтвердить', cls:'success', action: () => {
      const p = document.getElementById('root-pass').value;
      if (p === 'POFO_root') {
        window.rootEnabled = true;
        closeModal();
        closeApp();
        alert('Root права включены!');
      } else {
        alert('Неверный пароль!');
      }
    }}
  ]);
}
function changeOSName() {
  const n = document.getElementById('os-name-input').value.trim();
  if (!n) return;
  document.getElementById('os-name').textContent = n;
  alert('Название изменено: ' + n);
}
function resetOSName() {
  document.getElementById('os-name').textContent = 'POFO OS 2.0';
  alert('Название сброшено');
}
function toggleTheme() {
  state.theme = state.theme === 'light' ? 'dark' : 'light';
  saveState();
  document.body.style.background = state.theme === 'light' ? '#fff' : '#1a1a2e';
  document.body.style.color = state.theme === 'light' ? '#000' : '#fff';
  closeApp();
}
function checkUpdate() {
  alert('✅ Обновлений не найдено\nPOFO OS 2.0 — последняя версия');
}
function factoryReset() {
  if (!confirm('Сбросить всё до заводских настроек?')) return;
  state.installedApps = [];
  state.appNames = {};
  state.appIcons = {};
  state.bannedUsers = [];
  state.customChannels = [];
  state.notesText = '';
  state.wallpaper = 'dark';
  state.customWallpaper = null;
  state.theme = 'dark';
  state.clickerCount = 0;
  state.clickerDate = new Date().toDateString();
  state.currentUser = null;
  state.currentUserName = '';
  state.currentUserStatus = '';
  state.panicMode = false;
  state.backupApps = [];
  window.rootEnabled = false;
  saveState();
  closeApp();
  alert('Сброс выполнен');
}
function triggerPanic() {
  state.panicMode = true;
  state.backupApps = [...state.installedApps];
  state.installedApps = [];
  state.wallpaper = 'panic';
  state.customWallpaper = null;
  saveState();
  closeApp();
  document.getElementById('panic-overlay').classList.add('active');
}
function exitPanic() {
  state.panicMode = false;
  state.installedApps = state.backupApps;
  state.backupApps = [];
  state.wallpaper = 'dark';
  saveState();
  document.getElementById('panic-overlay').classList.remove('active');
  renderDesktop();
}

// ============ НАВИГАЦИЯ ============
document.getElementById('app-back').onclick = () => closeApp();
document.getElementById('app-exit').onclick = () => closeApp();
document.getElementById('nav-back').onclick = () => {};
document.getElementById('nav-exit').onclick = () => {};

// ============ ИНИЦИАЛИЗАЦИЯ ============
loadState();
if (state.theme === 'light') {
  document.body.style.background = '#fff';
  document.body.style.color = '#000';
}
renderDesktop();
setInterval(updateClock, 1000);
updateClock();
</script>
</body>
</html>
