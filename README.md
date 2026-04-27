<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CyberAttack Defense - Tendencias en Ciberseguridad</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
 
  @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.4} }
  @keyframes shake { 0%,100%{transform:translateX(0)} 20%{transform:translateX(-8px)} 40%{transform:translateX(8px)} 60%{transform:translateX(-6px)} 80%{transform:translateX(6px)} }
  @keyframes pop { 0%{transform:scale(0.8);opacity:0} 60%{transform:scale(1.05)} 100%{transform:scale(1);opacity:1} }
  @keyframes scan { 0%{top:0%} 100%{top:100%} }
  @keyframes glitch { 0%,90%,100%{transform:none;opacity:1} 92%{transform:translateX(-3px);opacity:0.8} 94%{transform:translateX(3px);opacity:0.9} 96%{transform:translateX(-2px)} }
  @keyframes float { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-6px)} }
 
  body {
    background: #0a0e1a;
    color: #e2e8f0;
    font-family: 'Courier New', monospace;
    min-height: 100vh;
  }
 
  #gw {
    background: #0a0e1a;
    color: #e2e8f0;
    font-family: 'Courier New', monospace;
    min-height: 100vh;
    position: relative;
    overflow: hidden;
  }
 
  .grid-bg {
    position: fixed; inset: 0;
    background-image:
      linear-gradient(rgba(0,255,180,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,255,180,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
    z-index: 0;
  }
 
  .scan-line {
    position: fixed; left: 0; right: 0; height: 2px;
    background: linear-gradient(transparent, rgba(0,255,180,0.15), transparent);
    animation: scan 4s linear infinite;
    pointer-events: none;
    z-index: 1;
  }
 
  .inner {
    position: relative; z-index: 2;
    padding: 1.5rem 1rem;
    max-width: 700px;
    margin: 0 auto;
  }
 
  .header { text-align: center; margin-bottom: 1.5rem; }
 
  .logo {
    font-size: 11px; letter-spacing: 0.15em; color: #00ffb3;
    text-transform: uppercase; margin-bottom: 0.4rem; opacity: 0.7;
  }
 
  .title {
    font-size: clamp(20px, 5vw, 32px); font-weight: 700;
    color: #00ffb3; animation: glitch 6s ease infinite;
    text-shadow: 0 0 20px rgba(0,255,179,0.4);
    margin-bottom: 0.3rem;
  }
 
  .subtitle { font-size: 12px; color: #64748b; letter-spacing: 0.1em; }
 
  .hud {
    display: grid; grid-template-columns: 1fr auto 1fr;
    align-items: center; gap: 1rem; margin-bottom: 1.2rem;
    background: rgba(0,255,179,0.04);
    border: 0.5px solid rgba(0,255,179,0.15);
    border-radius: 8px; padding: 0.75rem 1rem;
  }
 
  .hud-stat { display: flex; flex-direction: column; gap: 2px; }
  .hud-stat.right { align-items: flex-end; }
 
  .hud-label {
    font-size: 10px; color: #475569;
    letter-spacing: 0.1em; text-transform: uppercase;
  }
 
  .hud-val {
    font-size: 18px; font-weight: 700; color: #00ffb3;
    font-family: 'Courier New', monospace;
  }
 
  .timer-wrap { position: relative; width: 56px; height: 56px; }
 
  .timer-svg { width: 56px; height: 56px; transform: rotate(-90deg); }
 
  .timer-bg { fill: none; stroke: rgba(0,255,179,0.1); stroke-width: 3; }
 
  .timer-arc {
    fill: none; stroke: #00ffb3; stroke-width: 3;
    stroke-dasharray: 138.2; stroke-dashoffset: 0;
    stroke-linecap: round;
    transition: stroke 0.3s, stroke-dashoffset 0.9s linear;
  }
 
  .timer-num {
    position: absolute; inset: 0; display: flex;
    align-items: center; justify-content: center;
    font-size: 16px; font-weight: 700; color: #00ffb3;
  }
 
  .lives { display: flex; gap: 4px; margin-top: 2px; }
 
  .heart {
    width: 14px; height: 14px; display: inline-block;
    background: #e24b4a;
    clip-path: polygon(50% 85%, 0% 35%, 15% 15%, 35% 15%, 50% 30%, 65% 15%, 85% 15%, 100% 35%);
    transition: all 0.3s;
  }
 
  .heart.lost {
    background: #1e293b;
    clip-path: polygon(50% 85%, 0% 35%, 15% 15%, 35% 15%, 50% 30%, 65% 15%, 85% 15%, 100% 35%);
  }
 
  .progress-track {
    height: 4px; background: rgba(0,255,179,0.1);
    border-radius: 2px; margin-bottom: 1.5rem; overflow: hidden;
  }
 
  .progress-fill {
    height: 100%; background: #00ffb3; border-radius: 2px;
    transition: width 0.5s ease; box-shadow: 0 0 8px rgba(0,255,179,0.5);
  }
 
  .q-card {
    background: rgba(15,20,35,0.9);
    border: 0.5px solid rgba(0,255,179,0.2);
    border-radius: 12px; padding: 1.5rem; margin-bottom: 1.2rem;
    animation: pop 0.3s ease;
  }
 
  .q-num { font-size: 10px; color: #475569; letter-spacing: 0.15em; margin-bottom: 0.5rem; }
 
  .q-text { font-size: clamp(14px, 2.5vw, 17px); line-height: 1.5; color: #e2e8f0; }
 
  .options { display: grid; gap: 0.6rem; }
 
  .opt-btn {
    background: rgba(15,20,35,0.8);
    border: 0.5px solid rgba(0,255,179,0.25);
    border-radius: 8px; padding: 0.85rem 1.1rem; color: #e2e8f0;
    font-family: 'Courier New', monospace;
    font-size: clamp(12px, 2vw, 14px);
    cursor: pointer; transition: all 0.2s; text-align: left;
    display: flex; align-items: center; gap: 10px;
    width: 100%;
  }
 
  .opt-btn:hover:not(:disabled) {
    background: rgba(0,255,179,0.08);
    border-color: rgba(0,255,179,0.5);
    color: #00ffb3; transform: translateX(4px);
  }
 
  .opt-btn:disabled { cursor: default; }
 
  .opt-btn.correct {
    background: rgba(16,185,129,0.15);
    border-color: #10b981; color: #10b981;
  }
 
  .opt-btn.wrong {
    background: rgba(239,68,68,0.12);
    border-color: #ef4444; color: #ef4444;
    animation: shake 0.4s ease;
  }
 
  .opt-key {
    width: 22px; height: 22px; border-radius: 4px;
    border: 0.5px solid rgba(0,255,179,0.3);
    display: flex; align-items: center; justify-content: center;
    font-size: 11px; color: #475569; flex-shrink: 0;
  }
 
  .feedback {
    border-radius: 8px; padding: 0.75rem 1rem;
    font-size: 13px; line-height: 1.5; margin-top: 0.8rem;
    animation: pop 0.3s ease; display: none;
  }
 
  .feedback.show { display: block; }
  .feedback.ok { background: rgba(16,185,129,0.1); border: 0.5px solid rgba(16,185,129,0.3); color: #10b981; }
  .feedback.fail { background: rgba(239,68,68,0.1); border: 0.5px solid rgba(239,68,68,0.3); color: #ef4444; }
 
  .combo-badge {
    text-align: center; font-size: 12px; color: #f59e0b;
    margin-bottom: 0.5rem; min-height: 20px; font-weight: 700;
    letter-spacing: 0.1em;
  }
 
  .end-card {
    text-align: center; animation: pop 0.5s ease;
    background: rgba(15,20,35,0.95);
    border: 0.5px solid rgba(0,255,179,0.3);
    border-radius: 16px; padding: 2rem 1.5rem;
  }
 
  .end-icon { font-size: 48px; margin-bottom: 0.8rem; line-height: 1; }
 
  .end-title { font-size: 22px; font-weight: 700; color: #00ffb3; margin-bottom: 0.5rem; }
 
  .end-sub { font-size: 13px; color: #64748b; margin-bottom: 1.5rem; }
 
  .score-ring {
    width: 100px; height: 100px; margin: 0 auto 1.5rem;
    position: relative; display: flex; align-items: center; justify-content: center;
  }
 
  .score-ring svg {
    position: absolute; inset: 0;
    width: 100%; height: 100%; transform: rotate(-90deg);
  }
 
  .score-num { font-size: 24px; font-weight: 700; color: #00ffb3; position: relative; z-index:1; }
 
  .stats-grid {
    display: grid; grid-template-columns: 1fr 1fr 1fr;
    gap: 0.5rem; margin-bottom: 1.5rem;
  }
 
  .stat-box {
    background: rgba(0,255,179,0.06);
    border: 0.5px solid rgba(0,255,179,0.15);
    border-radius: 8px; padding: 0.6rem;
  }
 
  .stat-box-val { font-size: 18px; font-weight: 700; color: #00ffb3; display: block; }
  .stat-box-lbl { font-size: 10px; color: #475569; letter-spacing: 0.08em; }
 
  .rank-badge {
    display: inline-block; padding: 4px 14px; border-radius: 20px;
    font-size: 12px; font-weight: 700; letter-spacing: 0.1em; margin-bottom: 1rem;
  }
 
  .action-row {
    display: flex; gap: 0.75rem;
    justify-content: center; flex-wrap: wrap;
  }
 
  .btn-primary {
    background: #00ffb3; color: #0a0e1a; border: none;
    border-radius: 8px; padding: 0.7rem 1.5rem; font-size: 14px;
    font-weight: 700; font-family: 'Courier New', monospace;
    cursor: pointer; letter-spacing: 0.05em; transition: all 0.2s;
  }
 
  .btn-primary:hover { background: #00e6a0; transform: scale(1.03); }
 
  .streak-bar { display: flex; gap: 3px; margin-top: 4px; }
 
  .streak-dot {
    width: 8px; height: 8px; border-radius: 50%;
    background: rgba(0,255,179,0.2); transition: all 0.3s;
  }
 
  .streak-dot.active { background: #f59e0b; box-shadow: 0 0 6px #f59e0b; }
 
  .particles { position: fixed; inset: 0; pointer-events: none; overflow: hidden; z-index: 99; }
 
  .particle {
    position: absolute; width: 4px; height: 4px;
    border-radius: 50%; background: #00ffb3; opacity: 0;
  }
 
  @media (max-width: 480px) {
    .inner { padding: 1rem 0.75rem; }
    .hud { padding: 0.6rem 0.75rem; gap: 0.5rem; }
    .stats-grid { grid-template-columns: 1fr 1fr 1fr; }
  }
</style>
</head>
<body>
<div id="gw">
  <div class="grid-bg"></div>
  <div class="scan-line"></div>
  <div class="particles" id="particles"></div>
 
  <div class="inner">
    <div class="header">
      <div class="logo">CyberSec Simulation v2.0</div>
      <div class="title">CYBERATTACK DEFENSE</div>
      <div class="subtitle">Tendencias Futuras en Ciberseguridad</div>
    </div>
 
    <div class="hud">
      <div class="hud-stat">
        <span class="hud-label">Score</span>
        <span class="hud-val" id="scoreVal">000</span>
        <div class="streak-bar" id="streakBar"></div>
      </div>
 
      <div class="timer-wrap">
        <svg class="timer-svg" viewBox="0 0 44 44">
          <circle class="timer-bg" cx="22" cy="22" r="18"/>
          <circle class="timer-arc" id="timerArc" cx="22" cy="22" r="18"/>
        </svg>
        <div class="timer-num" id="timerNum">15</div>
      </div>
 
      <div class="hud-stat right">
        <span class="hud-label">Vidas</span>
        <div class="lives" id="livesRow"></div>
        <span class="hud-label" style="margin-top:2px" id="levelLbl">Nivel 1</span>
      </div>
    </div>
 
    <div class="progress-track">
      <div class="progress-fill" id="progressFill" style="width:0%"></div>
    </div>
 
    <div id="mainArea"></div>
  </div>
</div>
 
<script>
const levels = [
  {
    topic: "IA en Seguridad", color: "#a78bfa",
    question: "Un sistema de IA detecta anomalías en el tráfico de red a las 3am. ¿Cuál es la respuesta correcta?",
    options: ["Ignorarlo, son falsos positivos comunes","Investigar y correlacionar con otros indicadores","Reiniciar el servidor afectado","Desconectar toda la red inmediatamente"],
    answer: 1,
    explanation: "La IA detecta patrones, pero siempre necesitas correlación humana. Los falsos positivos son reales, pero ignorar sin revisar es peligroso."
  },
  {
    topic: "Cloud Security", color: "#60a5fa",
    question: "Tu equipo sube una base de datos con datos de clientes a S3. ¿Cómo debe configurarse el bucket?",
    options: ["Público, para acceso fácil del equipo","Privado con IAM roles específicos","Privado con contraseña compartida","Acceso abierto solo dentro de la empresa"],
    answer: 1,
    explanation: "Principio de menor privilegio: solo quienes necesitan acceso lo tienen, usando IAM roles granulares. Nunca credenciales compartidas."
  },
  {
    topic: "DevSecOps", color: "#34d399",
    question: "¿En qué etapa del SDLC debe integrarse la seguridad según DevSecOps?",
    options: ["Solo en producción","Solo en testing","Desde el diseño hasta el despliegue","Solo cuando hay un incidente"],
    answer: 2,
    explanation: "Shift-left: integrar seguridad desde el inicio reduce el costo de corrección hasta 100x comparado con encontrar vulnerabilidades en producción."
  },
  {
    topic: "IA en Seguridad", color: "#a78bfa",
    question: "Un atacante usa IA generativa para crear deepfakes de voz del CEO. ¿Cuál es la mejor defensa?",
    options: ["Prohibir el uso de voz en reuniones","Implementar verificación multifactor y códigos de confirmación","Entrenar al CEO para sonar diferente","Usar solo comunicación por escrito"],
    answer: 1,
    explanation: "Los deepfakes de voz son muy convincentes. El protocolo de verificación fuera de banda (ej: código del día) es la defensa más práctica."
  },
  {
    topic: "Cloud Security", color: "#60a5fa",
    question: "Detectas que un contenedor Docker en producción tiene acceso root innecesario. ¿Qué haces?",
    options: ["Documentarlo para revisión futura","Ejecutar contenedores sin privilegios y aplicar seccomp","Solo cambia la contraseña root","Reinstalar el servidor completo"],
    answer: 1,
    explanation: "Los contenedores deben seguir el principio de menor privilegio. Nunca deben correr como root en producción, usando perfiles seccomp y AppArmor."
  },
  {
    topic: "DevSecOps", color: "#34d399",
    question: "Un desarrollador hardcodea una API key de AWS en el código y lo sube a GitHub. ¿Primer paso?",
    options: ["Hacer el repositorio privado","Rotar la credencial inmediatamente y auditar su uso","Eliminar el commit del historial","Notificar al desarrollador por email"],
    answer: 1,
    explanation: "Las credenciales expuestas deben rotarse en minutos. GitHub los indexa casi en tiempo real y bots automatizados las explotan en segundos."
  },
  {
    topic: "IA en Seguridad", color: "#a78bfa",
    question: "¿Qué riesgo representa un modelo LLM con acceso a herramientas internas de la empresa?",
    options: ["Ninguno si está en la nube privada","Prompt injection puede manipularlo para exfiltrar datos","Solo es riesgo si el modelo es de código abierto","El riesgo es solo de disponibilidad"],
    answer: 1,
    explanation: "Prompt injection es el OWASP #1 para LLMs. Un atacante puede inyectar instrucciones que hagan al modelo ignorar controles y filtrar información."
  },
  {
    topic: "Cloud Security", color: "#60a5fa",
    question: "Tu empresa adopta arquitectura Zero Trust en la nube. ¿Cuál es el principio central?",
    options: ["Confiar en usuarios internos por defecto","Nunca confiar, siempre verificar, sin importar el origen","Confiar en todo el tráfico cifrado","Solo verificar en el primer acceso"],
    answer: 1,
    explanation: "Zero Trust elimina el concepto de red confiable. Toda solicitud debe autenticarse, autorizarse y cifrarse, incluso si viene de dentro de la red."
  }
];
 
const LIVES = 3;
const STREAK_MAX = 4;
let state = { score:0, level:0, lives:LIVES, streak:0, maxStreak:0, correct:0, wrong:0, timer:null, timeLeft:15 };
 
function fmt(n) { return String(n).padStart(3,'0'); }
 
function renderLives() {
  const row = document.getElementById('livesRow');
  row.innerHTML = '';
  for(let i=0;i<LIVES;i++){
    const h = document.createElement('div');
    h.className = 'heart' + (i >= state.lives ? ' lost' : '');
    row.appendChild(h);
  }
}
 
function renderStreak() {
  const bar = document.getElementById('streakBar');
  bar.innerHTML = '';
  for(let i=0;i<STREAK_MAX;i++){
    const d = document.createElement('div');
    d.className = 'streak-dot' + (i < state.streak ? ' active' : '');
    bar.appendChild(d);
  }
}
 
function updateTimer(seconds) {
  const arc = document.getElementById('timerArc');
  const num = document.getElementById('timerNum');
  const circ = 2 * Math.PI * 18;
  arc.style.strokeDashoffset = circ * (1 - seconds/15);
  const color = seconds <= 5 ? '#ef4444' : seconds <= 8 ? '#f59e0b' : '#00ffb3';
  arc.style.stroke = color;
  num.style.color = color;
  num.textContent = seconds;
}
 
function startTimer() {
  clearInterval(state.timer);
  state.timeLeft = 15;
  updateTimer(15);
  state.timer = setInterval(() => {
    state.timeLeft--;
    updateTimer(state.timeLeft);
    if(state.timeLeft <= 0) { clearInterval(state.timer); timeOut(); }
  }, 1000);
}
 
function stopTimer() { clearInterval(state.timer); }
 
function timeOut() {
  state.lives--;
  renderLives();
  state.streak = 0;
  renderStreak();
  document.querySelectorAll('.opt-btn').forEach((b,i) => {
    b.disabled = true;
    if(i === levels[state.level].answer) b.classList.add('correct');
  });
  showFeedback(false, "Tiempo agotado. " + levels[state.level].explanation);
  if(state.lives <= 0) { setTimeout(gameOver, 1800); return; }
  setTimeout(() => { state.level++; showQuestion(); }, 2200);
}
 
function showQuestion() {
  if(state.level >= levels.length) { endGame(); return; }
  const q = levels[state.level];
  document.getElementById('progressFill').style.width = (state.level / levels.length * 100) + '%';
  document.getElementById('levelLbl').textContent = 'Nivel ' + (state.level + 1);
  document.getElementById('scoreVal').textContent = fmt(state.score);
  renderStreak();
 
  document.getElementById('mainArea').innerHTML = `
    <div style="display:flex;align-items:center;gap:8px;margin-bottom:1rem;">
      <div style="width:6px;height:6px;border-radius:50%;background:${q.color};animation:pulse 1s ease infinite"></div>
      <span style="font-size:11px;color:${q.color};letter-spacing:0.1em;text-transform:uppercase">${q.topic}</span>
      <span style="font-size:10px;color:#334155;margin-left:auto">${state.level+1} / ${levels.length}</span>
    </div>
    <div class="q-card">
      <div class="q-num">ESCENARIO ${String(state.level+1).padStart(2,'0')}</div>
      <div class="q-text">${q.question}</div>
    </div>
    <div class="combo-badge" id="comboBadge">${state.streak >= 2 ? '+ RACHA x' + state.streak + ' !' : ''}</div>
    <div class="options" id="optionsGrid">
      ${q.options.map((o,i) => `
        <button class="opt-btn" onclick="pick(${i})">
          <span class="opt-key">${String.fromCharCode(65+i)}</span>
          ${o}
        </button>`).join('')}
    </div>
    <div class="feedback" id="feedbackBox"></div>
  `;
  startTimer();
}
 
function pick(i) {
  stopTimer();
  const q = levels[state.level];
  const btns = document.querySelectorAll('.opt-btn');
  btns.forEach(b => b.disabled = true);
 
  if(i === q.answer) {
    btns[i].classList.add('correct');
    const bonus = state.streak >= 3 ? 25 : state.streak >= 2 ? 20 : 15;
    const timeBonus = Math.floor(state.timeLeft * 0.5);
    state.score += bonus + timeBonus;
    state.streak++;
    state.correct++;
    if(state.streak > state.maxStreak) state.maxStreak = state.streak;
    document.getElementById('scoreVal').textContent = fmt(state.score);
    renderStreak();
    spawnParticles();
    showFeedback(true, "Correcto +"+bonus+" pts (+" + timeBonus + " rapidez). " + q.explanation);
  } else {
    btns[i].classList.add('wrong');
    btns[q.answer].classList.add('correct');
    state.lives--;
    state.streak = 0;
    state.wrong++;
    renderLives();
    renderStreak();
    showFeedback(false, "Incorrecto. " + q.explanation);
  }
 
  if(state.lives <= 0) { setTimeout(gameOver, 1800); return; }
  setTimeout(() => { state.level++; showQuestion(); }, 2400);
}
 
function showFeedback(ok, text) {
  const fb = document.getElementById('feedbackBox');
  if(!fb) return;
  fb.className = 'feedback show ' + (ok ? 'ok' : 'fail');
  fb.innerHTML = (ok ? '✓ ' : '✗ ') + text;
}
 
function spawnParticles() {
  const p = document.getElementById('particles');
  for(let i=0;i<14;i++){
    const dot = document.createElement('div');
    dot.className = 'particle';
    dot.style.left = (20 + Math.random()*60) + '%';
    dot.style.top = (30 + Math.random()*40) + '%';
    dot.style.background = ['#00ffb3','#60a5fa','#a78bfa','#f59e0b'][Math.floor(Math.random()*4)];
    p.appendChild(dot);
    dot.animate([
      {opacity:1,transform:'translate(0,0) scale(1)'},
      {opacity:0,transform:`translate(${(Math.random()-0.5)*100}px,${-50-Math.random()*80}px) scale(0)`}
    ], {duration:700+Math.random()*400, easing:'ease-out'}).onfinish = () => dot.remove();
  }
}
 
function gameOver() {
  stopTimer();
  const pct = Math.round((state.correct / levels.length) * 100);
  document.getElementById('progressFill').style.width = (state.level / levels.length * 100) + '%';
  document.getElementById('mainArea').innerHTML = `
    <div class="end-card">
      <div class="end-icon">💀</div>
      <div class="end-title">SISTEMA COMPROMETIDO</div>
      <div class="end-sub">Las defensas fallaron. El atacante ganó acceso.</div>
      <div class="score-ring">
        <svg viewBox="0 0 100 100">
          <circle fill="none" stroke="rgba(239,68,68,0.15)" stroke-width="8" cx="50" cy="50" r="40"/>
          <circle fill="none" stroke="#ef4444" stroke-width="8" cx="50" cy="50" r="40"
            stroke-dasharray="251.2"
            stroke-dashoffset="${251.2*(1-pct/100)}"
            stroke-linecap="round"
            style="transform-origin:center;transform:rotate(-90deg)"/>
        </svg>
        <div class="score-num">${fmt(state.score)}</div>
      </div>
      <div class="stats-grid">
        <div class="stat-box"><span class="stat-box-val">${state.correct}</span><span class="stat-box-lbl">Correctas</span></div>
        <div class="stat-box"><span class="stat-box-val">${state.wrong}</span><span class="stat-box-lbl">Errores</span></div>
        <div class="stat-box"><span class="stat-box-val">x${state.maxStreak}</span><span class="stat-box-lbl">Racha</span></div>
      </div>
      <div class="action-row">
        <button class="btn-primary" onclick="restart()">Reintentar</button>
      </div>
    </div>`;
}
 
function endGame() {
  stopTimer();
  const pct = Math.round((state.correct / levels.length) * 100);
  const rank = pct === 100 ? ['ELITE DEFENDER','#00ffb3'] : pct >= 75 ? ['SECURITY PRO','#a78bfa'] : pct >= 50 ? ['ANALYST LVL 2','#60a5fa'] : ['TRAINEE','#f59e0b'];
  spawnParticles(); spawnParticles();
  document.getElementById('mainArea').innerHTML = `
    <div class="end-card">
      <div class="end-icon">🛡️</div>
      <div class="end-title">SISTEMA ASEGURADO</div>
      <div class="end-sub">Superaste todos los escenarios. ¡Excelente trabajo!</div>
      <div class="score-ring">
        <svg viewBox="0 0 100 100">
          <circle fill="none" stroke="rgba(0,255,179,0.1)" stroke-width="8" cx="50" cy="50" r="40"/>
          <circle fill="none" stroke="#00ffb3" stroke-width="8" cx="50" cy="50" r="40"
            stroke-dasharray="251.2"
            stroke-dashoffset="${251.2*(1-pct/100)}"
            stroke-linecap="round"
            style="transform-origin:center;transform:rotate(-90deg)"/>
        </svg>
        <div class="score-num">${fmt(state.score)}</div>
      </div>
      <div style="margin-bottom:1rem">
        <span class="rank-badge" style="background:rgba(0,255,179,0.1);color:${rank[1]};border:0.5px solid ${rank[1]}44">${rank[0]}</span>
      </div>
      <div class="stats-grid">
        <div class="stat-box"><span class="stat-box-val">${state.correct}</span><span class="stat-box-lbl">Correctas</span></div>
        <div class="stat-box"><span class="stat-box-val">${pct}%</span><span class="stat-box-lbl">Precisión</span></div>
        <div class="stat-box"><span class="stat-box-val">x${state.maxStreak}</span><span class="stat-box-lbl">Racha</span></div>
      </div>
      <div class="action-row">
        <button class="btn-primary" onclick="restart()">Jugar de nuevo</button>
      </div>
    </div>`;
}
 
function restart() {
  state = { score:0, level:0, lives:LIVES, streak:0, maxStreak:0, correct:0, wrong:0, timer:null, timeLeft:15 };
  document.getElementById('progressFill').style.width = '0%';
  renderLives();
  renderStreak();
  showQuestion();
}
 
renderLives();
renderStreak();
showQuestion();
</script>
</body>
</html>
