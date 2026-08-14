here<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Clavecin — Apprendre le piano</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,600;9..144,700&family=Inter:wght@400;500;600&family=IBM+Plex+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
:root {
  --ink: #14120f;
  --ivory: #f2ede4;
  --walnut: #3a2a1e;
  --walnut-light: #4d382745;
  --brass: #b08d57;
  --brass-bright: #d4ac6e;
  --off: #8c4a3d;
  --line: #ffffff1a;
  --font-display: 'Fraunces', serif;
  --font-body: 'Inter', sans-serif;
  --font-mono: 'IBM Plex Mono', monospace;
}
* { box-sizing: border-box; }
html, body {
  margin: 0; padding: 0;
  background: var(--ink); color: var(--ivory);
  font-family: var(--font-body); min-height: 100vh;
}
::selection { background: var(--brass); color: var(--ink); }
button { font-family: inherit; cursor: pointer; }
.hidden { display: none !important; }

.topbar {
  display: flex; justify-content: space-between; align-items: center;
  padding: 20px 32px; border-bottom: 1px solid var(--line);
}
.brand { display: flex; align-items: center; gap: 10px; }
.brand-mark { color: var(--brass); font-size: 14px; }
.brand-name { font-family: var(--font-display); font-weight: 600; font-size: 20px; letter-spacing: 0.02em; }
.source-status { display: flex; align-items: center; gap: 8px; font-family: var(--font-mono); font-size: 12px; color: #b5ab9c; }
.dot { width: 7px; height: 7px; border-radius: 50%; background: var(--off); }
.dot.active { background: var(--brass-bright); box-shadow: 0 0 8px var(--brass-bright); }

.setup-screen { min-height: calc(100vh - 73px); display: flex; align-items: center; justify-content: center; padding: 40px 24px; }
.setup-inner { max-width: 640px; width: 100%; text-align: center; }
.eyebrow { font-family: var(--font-mono); font-size: 12px; letter-spacing: 0.12em; text-transform: uppercase; color: var(--brass); margin: 0 0 12px; }
.setup-inner h1 { font-family: var(--font-display); font-size: clamp(32px, 5vw, 48px); font-weight: 600; margin: 0 0 16px; line-height: 1.1; }
.setup-sub { color: #c9beac; font-size: 15px; line-height: 1.6; max-width: 440px; margin: 0 auto 40px; }
.source-choices { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
@media (max-width: 560px) { .source-choices { grid-template-columns: 1fr; } }
.source-card {
  background: var(--walnut-light); border: 1px solid var(--line); border-radius: 4px;
  padding: 28px 20px; display: flex; flex-direction: column; align-items: flex-start; gap: 8px;
  text-align: left; transition: border-color 0.15s ease, transform 0.15s ease; color: var(--ivory);
}
.source-card:hover { border-color: var(--brass); transform: translateY(-2px); }
.source-card:focus-visible { outline: 2px solid var(--brass-bright); outline-offset: 2px; }
.source-icon { font-size: 22px; color: var(--brass); }
.source-title { font-family: var(--font-display); font-size: 18px; font-weight: 600; }
.source-desc { font-size: 13px; color: #b5ab9c; line-height: 1.5; }
.setup-note { margin-top: 24px; font-family: var(--font-mono); font-size: 12px; color: var(--off); min-height: 18px; }

.play-screen { max-width: 900px; margin: 0 auto; padding: 32px 24px 48px; }
.lesson-meta { display: flex; justify-content: space-between; align-items: flex-end; margin-bottom: 24px; flex-wrap: wrap; gap: 16px; }
.lesson-meta h2 { font-family: var(--font-display); font-size: 28px; font-weight: 600; margin: 0; }
.score-panel { display: flex; gap: 24px; }
.score-item { text-align: right; }
.score-value { display: block; font-family: var(--font-mono); font-size: 26px; color: var(--brass-bright); }
.score-label { font-size: 11px; text-transform: uppercase; letter-spacing: 0.08em; color: #b5ab9c; }

.note-highway {
  position: relative; height: 260px;
  background: linear-gradient(180deg, #1c1812 0%, #14120f 100%);
  border: 1px solid var(--line); border-radius: 4px; overflow: hidden; margin-bottom: 4px;
}
.strike-line { position: absolute; bottom: 0; left: 0; right: 0; height: 2px; background: var(--brass); box-shadow: 0 0 12px var(--brass); }
.lanes { position: absolute; inset: 0; }
.falling-note { position: absolute; height: 18px; border-radius: 2px; background: var(--brass); opacity: 0.9; }
.falling-note.hit { background: var(--brass-bright); }
.falling-note.missed { background: var(--off); }

.keyboard {
  position: relative; height: 120px; display: flex;
  background: var(--walnut); border: 1px solid var(--line); border-top: none;
  border-radius: 0 0 4px 4px; margin-bottom: 32px; padding: 6px 6px 0;
}
.key { position: relative; flex: 1; background: var(--ivory); border: 1px solid #00000033; border-radius: 0 0 3px 3px; transition: background 0.08s ease; }
.key.black { position: absolute; width: 5.5%; height: 62%; background: var(--ink); border-radius: 0 0 2px 2px; z-index: 2; }
.key.active { background: var(--brass-bright); }
.key.black.active { background: var(--brass); }

.controls { display: flex; gap: 12px; margin-bottom: 16px; }
.btn-primary { background: var(--brass); color: var(--ink); border: none; border-radius: 3px; padding: 12px 24px; font-size: 14px; font-weight: 600; letter-spacing: 0.01em; }
.btn-primary:hover { background: var(--brass-bright); }
.btn-ghost { background: transparent; color: var(--ivory); border: 1px solid var(--line); border-radius: 3px; padding: 12px 20px; font-size: 14px; }
.btn-ghost:hover { border-color: var(--brass); }
.live-readout { font-family: var(--font-mono); font-size: 13px; color: #b5ab9c; }

@media (prefers-reduced-motion: reduce) { * { transition: none !important; animation: none !important; } }
</style>
</head>
<body>

<header class="topbar">
  <div class="brand">
    <span class="brand-mark">◆</span>
    <span class="brand-name">Clavecin</span>
  </div>
  <div class="source-status">
    <span class="dot" id="sourceDot"></span>
    <span id="sourceLabel">Aucune source</span>
  </div>
</header>

<section id="levelScreen" class="setup-screen">
  <div class="setup-inner">
    <p class="eyebrow">Pour commencer</p>
    <h1>Quel est votre niveau actuel&nbsp;?</h1>
    <p class="setup-sub">Ça détermine comment les exercices progressent.</p>
    <div class="source-choices">
      <button class="source-card" data-level="debutant">
        <span class="source-icon">◠</span>
        <span class="source-title">Débutant</span>
        <span class="source-desc">La difficulté s'ajuste automatiquement selon vos performances.</span>
      </button>
      <button class="source-card" data-level="intermediaire">
        <span class="source-icon">▤</span>
        <span class="source-title">Intermédiaire</span>
        <span class="source-desc">Choisissez directement un niveau : 1, 2 ou 3.</span>
      </button>
    </div>
  </div>
</section>

<section id="lessonPickScreen" class="setup-screen hidden">
  <div class="setup-inner">
    <p class="eyebrow">Niveau intermédiaire</p>
    <h1>Quel niveau voulez-vous travailler&nbsp;?</h1>
    <div class="source-choices" style="grid-template-columns: repeat(3, 1fr);">
      <button class="source-card" data-fixed-level="1">
        <span class="source-title">Niveau 1</span>
        <span class="source-desc">Gamme simple, tempo modéré.</span>
      </button>
      <button class="source-card" data-fixed-level="2">
        <span class="source-title">Niveau 2</span>
        <span class="source-desc">Plus de notes, tempo plus rapide.</span>
      </button>
      <button class="source-card" data-fixed-level="3">
        <span class="source-title">Niveau 3</span>
        <span class="source-desc">Plage étendue, rythme plus exigeant.</span>
      </button>
    </div>
  </div>
</section>

<section id="setupScreen" class="setup-screen hidden">
  <div class="setup-inner">
    <p class="eyebrow">Avant de commencer</p>
    <h1>Comment allez-vous jouer&nbsp;?</h1>
    <p class="setup-sub">Choisissez votre source d'entrée. Vous pourrez en changer à tout moment.</p>
    <div class="source-choices">
      <button class="source-card" data-source="mic">
        <span class="source-icon">◠</span>
        <span class="source-title">Piano acoustique</span>
        <span class="source-desc">Détection par microphone. Une note à la fois.</span>
      </button>
      <button class="source-card" data-source="midi">
        <span class="source-icon">▤</span>
        <span class="source-title">Clavier MIDI</span>
        <span class="source-desc">Branché en USB. Notes exactes, accords compris.</span>
      </button>
    </div>
    <p class="setup-note" id="setupNote"></p>
  </div>
</section>

<main id="playScreen" class="play-screen hidden">
  <div class="lesson-meta">
    <div>
      <p class="eyebrow" id="levelLabel">Niveau adaptatif</p>
      <h2 id="lessonTitle">Chargement…</h2>
    </div>
    <div class="score-panel">
      <div class="score-item"><span class="score-value" id="pitchScore">—</span><span class="score-label">Justesse</span></div>
      <div class="score-item"><span class="score-value" id="timingScore">—</span><span class="score-label">Rythme</span></div>
    </div>
  </div>

  <div class="note-highway" id="noteHighway">
    <div class="strike-line"></div>
    <div class="lanes" id="lanes"></div>
  </div>

  <div class="keyboard" id="keyboard"></div>

  <div class="controls">
    <button class="btn-primary" id="startBtn">Démarrer l'exercice</button>
    <button class="btn-ghost" id="changeSourceBtn">Changer de source</button>
  </div>

  <p class="live-readout" id="liveReadout">En attente…</p>
</main>

<script>
// ============================================================
// 1. DÉTECTION DE PITCH (autocorrélation) — pour la source micro
// ============================================================
const RMS_THRESHOLD = 0.01;

function autoCorrelate(buffer, sampleRate) {
  const SIZE = buffer.length;
  let rms = 0;
  for (let i = 0; i < SIZE; i++) rms += buffer[i] * buffer[i];
  rms = Math.sqrt(rms / SIZE);
  if (rms < RMS_THRESHOLD) return null;

  let start = 0, end = SIZE - 1;
  const trimThreshold = rms * 0.2;
  while (start < SIZE / 2 && Math.abs(buffer[start]) < trimThreshold) start++;
  while (end > SIZE / 2 && Math.abs(buffer[end]) < trimThreshold) end--;

  const trimmed = buffer.slice(start, end);
  const n = trimmed.length;
  if (n < 512) return null;

  const correlations = new Float32Array(n);
  for (let lag = 0; lag < n; lag++) {
    let sum = 0;
    for (let i = 0; i < n - lag; i++) sum += trimmed[i] * trimmed[i + lag];
    correlations[lag] = sum;
  }

  let d = 0;
  while (d < n - 1 && correlations[d] > correlations[d + 1]) d++;

  let maxVal = -Infinity, maxPos = -1;
  for (let i = d; i < n; i++) {
    if (correlations[i] > maxVal) { maxVal = correlations[i]; maxPos = i; }
  }
  if (maxPos <= 0) return null;

  const x1 = correlations[maxPos - 1] || 0;
  const x2 = correlations[maxPos];
  const x3 = correlations[maxPos + 1] || 0;
  const a = (x1 + x3 - 2 * x2) / 2;
  const b = (x3 - x1) / 2;
  const shift = a ? -b / (2 * a) : 0;

  const period = maxPos + shift;
  if (period <= 0) return null;

  return sampleRate / period;
}

function frequencyToNote(frequency) {
  if (!frequency || frequency <= 0) return null;
  const NOTE_NAMES = ['C', 'C#', 'D', 'D#', 'E', 'F', 'F#', 'G', 'G#', 'A', 'A#', 'B'];
  const midi = 69 + 12 * Math.log2(frequency / 440);
  const rounded = Math.round(midi);
  const cents = Math.round((midi - rounded) * 100);
  const octave = Math.floor(rounded / 12) - 1;
  const name = NOTE_NAMES[((rounded % 12) + 12) % 12];
  return { note: `${name}${octave}`, midi: rounded, cents, frequency };
}

function midiToNoteName(midi) {
  const NOTE_NAMES = ['C', 'C#', 'D', 'D#', 'E', 'F', 'F#', 'G', 'G#', 'A', 'A#', 'B'];
  const octave = Math.floor(midi / 12) - 1;
  return `${NOTE_NAMES[midi % 12]}${octave}`;
}

// ============================================================
// 2. SOURCES AUDIO — même interface (événement 'note') pour les deux
// ============================================================
class MicSource extends EventTarget {
  constructor() { super(); this.audioContext = null; this.analyser = null; this.stream = null; this.running = false; this._lastNote = null; }

  async start() {
    this.audioContext = new (window.AudioContext || window.webkitAudioContext)();
    this.stream = await navigator.mediaDevices.getUserMedia({ audio: true });
    const src = this.audioContext.createMediaStreamSource(this.stream);
    this.analyser = this.audioContext.createAnalyser();
    this.analyser.fftSize = 2048;
    src.connect(this.analyser);
    this.running = true;
    this._loop();
  }

  _loop() {
    if (!this.running) return;
    const buffer = new Float32Array(this.analyser.fftSize);
    this.analyser.getFloatTimeDomainData(buffer);
    const freq = autoCorrelate(buffer, this.audioContext.sampleRate);
    if (freq) {
      const noteInfo = frequencyToNote(freq);
      if (noteInfo && noteInfo.note !== this._lastNote) {
        this._lastNote = noteInfo.note;
        this.dispatchEvent(new CustomEvent('note', {
          detail: { note: noteInfo.note, midi: noteInfo.midi, cents: noteInfo.cents, velocity: null, timestamp: performance.now() },
        }));
      }
    } else {
      this._lastNote = null;
    }
    requestAnimationFrame(() => this._loop());
  }

  stop() {
    this.running = false;
    if (this.stream) this.stream.getTracks().forEach((t) => t.stop());
    if (this.audioContext) this.audioContext.close();
  }
}

class MidiSource extends EventTarget {
  constructor() { super(); this.access = null; this.inputs = []; }

  async start() {
    if (!navigator.requestMIDIAccess) throw new Error('Web MIDI non supporté par ce navigateur');
    this.access = await navigator.requestMIDIAccess();
    this.inputs = Array.from(this.access.inputs.values());
    if (this.inputs.length === 0) throw new Error('Aucun périphérique MIDI détecté');

    this.inputs.forEach((input) => { input.onmidimessage = (msg) => this._handleMessage(msg); });
    this.access.onstatechange = () => {
      this.inputs = Array.from(this.access.inputs.values());
      this.inputs.forEach((input) => { input.onmidimessage = (msg) => this._handleMessage(msg); });
    };
  }

  _handleMessage(msg) {
    const [status, data1, data2] = msg.data;
    const command = status & 0xf0;
    if (command === 0x90 && data2 > 0) {
      this.dispatchEvent(new CustomEvent('note', {
        detail: { note: midiToNoteName(data1), midi: data1, velocity: data2, timestamp: performance.now() },
      }));
    }
  }

  stop() {
    this.inputs.forEach((input) => { input.onmidimessage = null; });
    if (this.access) this.access.onstatechange = null;
  }
}

// ============================================================
// 3. MOTEUR DE LEÇON — comparaison notes jouées / attendues + scoring
// ============================================================
const TIMING_TOLERANCE_MS = 300;

class LessonEngine {
  constructor(lesson) {
    this.lesson = lesson;
    this.expected = lesson.notes.map((n) => ({ ...n, hit: false }));
    this.startedAt = null;
    this.results = [];
  }

  start() {
    this.startedAt = performance.now();
    this.expected.forEach((n) => (n.hit = false));
    this.results = [];
  }

  registerPlayedNote(midi, timestamp) {
    if (this.startedAt === null) return null;
    const elapsed = timestamp - this.startedAt;

    let best = null, bestDelta = Infinity;
    for (const exp of this.expected) {
      if (exp.hit) continue;
      const delta = Math.abs(exp.startTime - elapsed);
      if (delta < bestDelta) { bestDelta = delta; best = exp; }
    }

    if (!best || bestDelta > TIMING_TOLERANCE_MS * 2) {
      this.results.push({ expectedNote: null, playedNote: midi, correct: false, timingErrorMs: null });
      return { correct: false, expected: null };
    }

    best.hit = true;
    const correct = best.midi === midi;
    this.results.push({ expectedNote: best.midi, playedNote: midi, correct, timingErrorMs: bestDelta });
    return { correct, expected: best, timingErrorMs: bestDelta };
  }

  getPitchScore() {
    if (this.results.length === 0) return 0;
    const correct = this.results.filter((r) => r.correct).length;
    return Math.round((correct / this.expected.length) * 100);
  }

  getTimingScore() {
    const withTiming = this.results.filter((r) => r.timingErrorMs !== null);
    if (withTiming.length === 0) return 0;
    const avgError = withTiming.reduce((s, r) => s + r.timingErrorMs, 0) / withTiming.length;
    const score = 100 - (avgError / (TIMING_TOLERANCE_MS * 2)) * 100;
    return Math.max(0, Math.round(score));
  }

  isComplete() { return this.expected.every((n) => n.hit); }
}

function nextDifficulty(currentLevel, pitchScore, timingScore) {
  const overall = (pitchScore + timingScore) / 2;
  if (overall >= 85) return Math.min(currentLevel + 1, 10);
  if (overall < 50) return Math.max(currentLevel - 1, 1);
  return currentLevel;
}

// ============================================================
// 4. LEÇONS — tout part d'une seule source de données
// ============================================================
// Toutes les leçons (fixes et générées) sont dérivées de cet objet unique.
// Pour ajouter/modifier une leçon fixe ou changer les réglages de génération
// adaptative, c'est ici et nulle part ailleurs.
const LESSON_LIBRARY = {
  scale: [
    { midi: 60, note: 'C4' },
    { midi: 62, note: 'D4' },
    { midi: 64, note: 'E4' },
    { midi: 65, note: 'F4' },
    { midi: 67, note: 'G4' },
    { midi: 69, note: 'A4' },
    { midi: 71, note: 'B4' },
    { midi: 72, note: 'C5' },
  ],
  fixed: {
    1: {
      title: 'Niveau 1 — Gamme de Do',
      stepMs: 700,
      noteDurationMs: 500,
      sequence: [60, 62, 64, 65, 67],
    },
    2: {
      title: 'Niveau 2 — Gamme complète',
      stepMs: 550,
      noteDurationMs: 400,
      sequence: [60, 62, 64, 65, 67, 69, 71],
    },
    3: {
      title: 'Niveau 3 — Aller-retour rapide',
      stepMs: 450,
      noteDurationMs: 350,
      sequence: [60, 64, 67, 72, 67, 64, 60],
    },
  },
};

// Construit une leçon exploitable par le moteur à partir d'une entrée
// de LESSON_LIBRARY.fixed (séquence de midi -> notes datées).
function buildFixedLesson(level) {
  const def = LESSON_LIBRARY.fixed[level];
  const notes = def.sequence.map((midi, i) => ({
    midi,
    note: LESSON_LIBRARY.scale.find((s) => s.midi === midi)?.note || '',
    startTime: i * def.stepMs,
    duration: def.noteDurationMs,
  }));
  return { id: `fixed-${level}`, title: def.title, level: `${level}`, notes };
}

// Génère une leçon adaptative à partir de la même bibliothèque : plus le
// niveau monte, plus il y a de notes, plus la plage est large, plus le tempo est rapide.
function generateAdaptiveLesson(level) {
  const scale = LESSON_LIBRARY.scale;
  const noteCount = Math.min(3 + level, 10);
  const rangeSize = Math.min(3 + Math.floor(level / 2), scale.length);
  const stepMs = Math.max(1000 - level * 70, 350);

  const notes = [];
  for (let i = 0; i < noteCount; i++) {
    const entry = scale[Math.floor(Math.random() * rangeSize)];
    notes.push({ midi: entry.midi, note: entry.note, startTime: i * stepMs, duration: Math.round(stepMs * 0.6) });
  }

  return { id: `adaptive-${level}`, title: `Exercice adaptatif — niveau ${level}`, level: `${level}`, isAdaptive: true, notes };
}

// ============================================================
// 5. UI / APPLICATION
// ============================================================
const KEYBOARD_START_MIDI = 60;
const KEYBOARD_OCTAVES = 1;
const WHITE_KEY_MIDI_OFFSETS = [0, 2, 4, 5, 7, 9, 11];
const BLACK_KEY_MIDI_OFFSETS = [1, 3, null, 6, 8, 10, null];
const HIGHWAY_DURATION_MS = 5000;

let source = null;
let engine = null;
let lesson = null;
let highwayStart = null;
let userLevel = null; // 'debutant' | 'intermediaire'
let adaptiveDifficulty = 1;

const el = {
  levelScreen: document.getElementById('levelScreen'),
  lessonPickScreen: document.getElementById('lessonPickScreen'),
  setupScreen: document.getElementById('setupScreen'),
  playScreen: document.getElementById('playScreen'),
  setupNote: document.getElementById('setupNote'),
  sourceDot: document.getElementById('sourceDot'),
  sourceLabel: document.getElementById('sourceLabel'),
  lessonTitle: document.getElementById('lessonTitle'),
  levelLabel: document.getElementById('levelLabel'),
  pitchScore: document.getElementById('pitchScore'),
  timingScore: document.getElementById('timingScore'),
  keyboard: document.getElementById('keyboard'),
  lanes: document.getElementById('lanes'),
  startBtn: document.getElementById('startBtn'),
  changeSourceBtn: document.getElementById('changeSourceBtn'),
  liveReadout: document.getElementById('liveReadout'),
};

// Étape 1 : niveau de l'utilisateur
document.querySelectorAll('#levelScreen .source-card').forEach((card) => {
  card.addEventListener('click', () => {
    userLevel = card.dataset.level;
    el.levelScreen.classList.add('hidden');
    if (userLevel === 'debutant') {
      adaptiveDifficulty = 1;
      el.setupScreen.classList.remove('hidden');
    } else {
      el.lessonPickScreen.classList.remove('hidden');
    }
  });
});

// Étape 1bis (intermédiaire uniquement) : choix du niveau figé 1/2/3
let pendingFixedLevel = null;
document.querySelectorAll('#lessonPickScreen .source-card').forEach((card) => {
  card.addEventListener('click', () => {
    pendingFixedLevel = Number(card.dataset.fixedLevel);
    el.lessonPickScreen.classList.add('hidden');
    el.setupScreen.classList.remove('hidden');
  });
});

// Étape 2 : source d'entrée (micro / MIDI)
document.querySelectorAll('#setupScreen .source-card').forEach((card) => {
  card.addEventListener('click', () => chooseSource(card.dataset.source));
});

el.changeSourceBtn.addEventListener('click', () => {
  if (source) source.stop();
  el.playScreen.classList.add('hidden');
  el.levelScreen.classList.remove('hidden');
  setSourceStatus(false, 'Aucune source');
});

async function chooseSource(type) {
  el.setupNote.textContent = '';
  try {
    source = type === 'mic' ? new MicSource() : new MidiSource();
    await source.start();
    source.addEventListener('note', onNotePlayed);

    setSourceStatus(true, type === 'mic' ? 'Micro actif' : 'MIDI connecté');
    el.setupScreen.classList.add('hidden');
    el.playScreen.classList.remove('hidden');

    const firstLesson = userLevel === 'debutant'
      ? generateAdaptiveLesson(adaptiveDifficulty)
      : buildFixedLesson(pendingFixedLevel);
    loadLesson(firstLesson);
  } catch (err) {
    el.setupNote.textContent = `Impossible de démarrer cette source : ${err.message}`;
  }
}

function setSourceStatus(active, label) {
  el.sourceDot.classList.toggle('active', active);
  el.sourceLabel.textContent = label;
}

function loadLesson(l) {
  lesson = l;
  engine = new LessonEngine(lesson);
  el.lessonTitle.textContent = lesson.title;
  el.levelLabel.textContent = userLevel === 'debutant' ? `Niveau adaptatif ${lesson.level}` : `Niveau ${lesson.level}`;
  el.startBtn.textContent = "Démarrer l'exercice";
  el.startBtn.disabled = false;
  el.pitchScore.textContent = '—';
  el.timingScore.textContent = '—';
  renderKeyboard();
  renderLanes();
}

function renderKeyboard() {
  el.keyboard.innerHTML = '';
  const totalWhite = 7 * KEYBOARD_OCTAVES;
  for (let i = 0; i < totalWhite; i++) {
    const key = document.createElement('div');
    key.className = 'key white';
    const octave = Math.floor(i / 7);
    const midi = KEYBOARD_START_MIDI + octave * 12 + WHITE_KEY_MIDI_OFFSETS[i % 7];
    key.dataset.midi = midi;
    el.keyboard.appendChild(key);

    const blackOffset = BLACK_KEY_MIDI_OFFSETS[i % 7];
    if (blackOffset !== null) {
      const blackKey = document.createElement('div');
      blackKey.className = 'key black';
      blackKey.dataset.midi = KEYBOARD_START_MIDI + octave * 12 + blackOffset;
      blackKey.style.left = `${((i + 1) / totalWhite) * 100 - (100 / totalWhite) * 0.35}%`;
      el.keyboard.appendChild(blackKey);
    }
  }
}

function flashKey(midi) {
  const key = el.keyboard.querySelector(`[data-midi="${midi}"]`);
  if (!key) return;
  key.classList.add('active');
  setTimeout(() => key.classList.remove('active'), 250);
}

function renderLanes() {
  el.lanes.innerHTML = '';
  const totalWhite = 7 * KEYBOARD_OCTAVES;
  lesson.notes.forEach((n) => {
    const div = document.createElement('div');
    div.className = 'falling-note';
    div.dataset.midi = n.midi;
    const laneIndex = n.midi - KEYBOARD_START_MIDI;
    div.style.left = `${(laneIndex / (totalWhite * 2)) * 100}%`;
    div.style.width = `${100 / (totalWhite * 2)}%`;
    div.style.setProperty('--start', n.startTime);
    el.lanes.appendChild(div);
  });
}

function animateHighway() {
  if (highwayStart === null) return;
  const elapsed = performance.now() - highwayStart;
  const highwayHeight = document.querySelector('.note-highway').clientHeight;

  el.lanes.querySelectorAll('.falling-note').forEach((noteEl) => {
    const start = Number(noteEl.style.getPropertyValue('--start'));
    const progress = (elapsed - (start - HIGHWAY_DURATION_MS)) / HIGHWAY_DURATION_MS;
    noteEl.style.bottom = `${progress * highwayHeight}px`;
  });

  if (elapsed < lesson.notes[lesson.notes.length - 1].startTime + HIGHWAY_DURATION_MS) {
    requestAnimationFrame(animateHighway);
  }
}

let awaitingNextLesson = false;

el.startBtn.addEventListener('click', () => {
  if (awaitingNextLesson) {
    awaitingNextLesson = false;
    loadLesson(generateAdaptiveLesson(adaptiveDifficulty));
    return;
  }
  engine.start();
  highwayStart = performance.now();
  animateHighway();
  el.liveReadout.textContent = "Exercice en cours…";
});

function onNotePlayed(e) {
  const { midi, note } = e.detail;

  if (!engine || !engine.startedAt) {
    el.liveReadout.textContent = `${note} détectée (exercice non démarré)`;
    return;
  }

  const result = engine.registerPlayedNote(midi, e.detail.timestamp);
  flashKey(midi);

  el.pitchScore.textContent = `${engine.getPitchScore()}`;
  el.timingScore.textContent = `${engine.getTimingScore()}`;
  el.liveReadout.textContent = result?.correct
    ? `${note} — correct ✓`
    : `${note} — attendu : ${result?.expected ? result.expected.note : '?'}`;

  if (engine.isComplete()) {
    const pitch = engine.getPitchScore();
    const timing = engine.getTimingScore();

    if (userLevel === 'debutant') {
      const next = nextDifficulty(adaptiveDifficulty, pitch, timing);
      const trend = next > adaptiveDifficulty ? 'ça monte d\'un cran ↑' : next < adaptiveDifficulty ? 'on redescend d\'un cran ↓' : 'on reste sur ce niveau';
      adaptiveDifficulty = next;
      el.liveReadout.textContent = `Terminé — Justesse ${pitch} / Rythme ${timing} — ${trend}`;
      el.startBtn.textContent = 'Leçon suivante';
      awaitingNextLesson = true;
    } else {
      el.liveReadout.textContent = `Exercice terminé — Justesse ${pitch} / Rythme ${timing}`;
    }
  }
}
</script>
</body>
</html>
