tcae-geriatria-app/
├─ index.html
├─ manifest.json
├─ sw.js
├─ README.md
├─ assets/
│  ├─ icons/
│  │  ├─ icon-192.png
│  │  └─ icon-512.png
│  └─ styles/
│     └─ main.css
├─ src/
│  ├─ app.js
│  ├─ router.js
│  ├─ ui/
│  │  ├─ nav.js
│  │  ├─ flashcards.js
│  │  ├─ testRunner.js
│  │  └─ simulacros.js
│  └─ utils/
│     ├─ store.js
│     └─ fetchJSON.js
├─ data/
│  ├─ summaries/
│  │  ├─ comunes.md
│  │  ├─ tcae_sas.md
│  │  └─ geriatria.md
│  ├─ flashcards/
│  │  ├─ comunes.json
│  │  ├─ tcae_sas.json
│  │  └─ geriatria.json
│  └─ tests/
│     ├─ comunes/
│     │  ├─ simulacro1.json
│     │  ├─ simulacro2.json
│     │  ├─ simulacro3.json
│     │  ├─ mini1.json
│     │  ├─ mini2.json
│     │  └─ mini3.json
│     ├─ tcae_sas/
│     │  ├─ simulacro1.json
│     │  ├─ simulacro2.json
│     │  ├─ simulacro3.json
│     │  ├─ mini1.json
│     │  ├─ mini2.json
│     │  └─ mini3.json
│     └─ geriatria/
│        ├─ simulacro1.json
│        ├─ simulacro2.json
│        ├─ simulacro3.json
│        ├─ mini1.json
│        ├─ mini2.json
│        └─ mini3.json
└─ .github/
   └─ workflows/
      └─ pages.yml
      <!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>TCAE & Geriatría – Estudio</title>
  <link rel="stylesheet" href="./assets/styles/main.css" />
  <link rel="manifest" href="./manifest.json" />
</head>
<body>
  <header>
    <h1>TCAE & Aux. Geriatría</h1>
    <nav id="nav"></nav>
  </header>

  <main id="app" role="main" aria-live="polite"></main>

  <footer>
    <small>Hecho con 💙 por Lourdes</small>
  </footer>

  <script type="module">
    import { initNav } from './src/ui/nav.js';
    import { router, navigate } from './src/router.js';
    import './src/app.js';

    initNav([
      { path: '#/resumenes', label: 'Resúmenes' },
      { path: '#/flashcards', label: 'Tarjetas' },
      { path: '#/mini-tests', label: 'Mini‑tests' },
      { path: '#/simulacros', label: 'Simulacros' }
    ]);

    window.addEventListener('hashchange', () => router());
    window.addEventListener('load', () => {
      router();
      if ('serviceWorker' in navigator) {
        navigator.serviceWorker.register('./sw.js');
      }
    });
  </script>
</body>
</html>
:root {
  --bg: #0f1320;
  --panel: #161b2e;
  --text: #e8ecff;
  --accent: #6ea8fe;
  --good: #59d189;
  --bad: #ff6b6b;
}
* { box-sizing: border-box; }
body { margin: 0; font-family: system-ui, sans-serif; background: var(--bg); color: var(--text); }
header, footer { padding: 1rem; background: var(--panel); }
h1 { margin: 0 0 .5rem 0; font-size: 1.25rem; }
nav { display: flex; gap: .5rem; flex-wrap: wrap; }
nav a { color: var(--text); text-decoration: none; padding: .5rem .75rem; border: 1px solid #2a3357; border-radius: .5rem; }
nav a.active { background: var(--accent); color: #0b0d18; }
main { padding: 1rem; max-width: 900px; margin: 0 auto; }
.card { background: var(--panel); border: 1px solid #2a3357; border-radius: .75rem; padding: 1rem; margin-bottom: 1rem; }
.button { cursor: pointer; background: var(--accent); color: #0b0d18; border: none; border-radius: .5rem; padding: .5rem .75rem; }
.grid { display: grid; gap: .75rem; grid-template-columns: repeat(auto-fit,minmax(240px,1fr)); }
.tag { display: inline-block; padding: .25rem .5rem; border-radius: .5rem; background: #2a3357; margin-right: .5rem; font-size: .85rem; }
input, select { width: 100%; padding: .5rem; border-radius: .5rem; border: 1px solid #2a3357; background: #0f1426; color: var(--text); }
// src/router.js
import { renderHome } from './app.js';
import { renderFlashcards } from './ui/flashcards.js';
import { renderTests } from './ui/testRunner.js';
import { renderSimulacros } from './ui/simulacros.js';

export function router() {
  const app = document.getElementById('app');
  const route = location.hash || '#/resumenes';

  if (route.startsWith('#/resumenes')) return renderHome(app);
  if (route.startsWith('#/flashcards')) return renderFlashcards(app);
  if (route.startsWith('#/mini-tests')) return renderTests(app, 'mini');
  if (route.startsWith('#/simulacros')) return renderSimulacros(app);
}

export function navigate(path) {
  location.hash = path;
}
// src/ui/nav.js
export function initNav(items) {
  const nav = document.getElementById('nav');
  const draw = () => {
    const hash = location.hash || items[0].path;
    nav.innerHTML = items.map(i =>
      `<a href="${i.path}" class="${hash.startsWith(i.path) ? 'active' : ''}">${i.label}</a>`
    ).join('');
  };
  draw();
  window.addEventListener('hashchange', draw);
}
// src/ui/nav.js
export function initNav(items) {
  const nav = document.getElementById('nav');
  const draw = () => {
    const hash = location.hash || items[0].path;
    nav.innerHTML = items.map(i =>
      `<a href="${i.path}" class="${hash.startsWith(i.path) ? 'active' : ''}">${i.label}</a>`
    ).join('');
  };
  draw();
  window.addEventListener('hashchange', draw);
}
// src/app.js
import { getText } from './utils/fetchJSON.js';

export async function renderHome(el) {
  el.innerHTML = `
    <section class="card">
      <h2>Resúmenes</h2>
      <p>Selecciona bloque para visualizar contenido en Markdown.</p>
      <div class="grid">
        ${['comunes','tcae_sas','geriatria'].map(b =>
          `<button class="button" data-b="${b}">${b.toUpperCase()}</button>`
        ).join('')}
      </div>
      <article id="md" class="card"></article>
    </section>
  `;
  const mdEl = document.getElementById('md');
  document.querySelectorAll('[data-b]').forEach(btn => {
    btn.addEventListener('click', async () => {
      const b = btn.dataset.b;
      const md = await getText(`./data/summaries/${b}.md`);
      mdEl.innerHTML = markdownToHTML(md);
    });
  });
}

function markdownToHTML(md) {
  // ultra-simple MD → HTML (titles + bold + lists)
  return md
    .replace(/^### (.*$)/gim, '<h3>$1</h3>')
    .replace(/^## (.*$)/gim, '<h2>$1</h2>')
    .replace(/^# (.*$)/gim, '<h1>$1</h1>')
    .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
    .replace(/^\- (.*$)/gim, '<li>$1</li>')
    .replace(/\n/g, '<br/>');
}
// src/ui/flashcards.js
import { getJSON } from '../utils/fetchJSON.js';

export async function renderFlashcards(el) {
  el.innerHTML = `
    <section class="card">
      <h2>Tarjetas</h2>
      <label>Bloque</label>
      <select id="deck">
        <option value="comunes">Comunes</option>
        <option value="tcae_sas">TCAE SAS</option>
        <option value="geriatria">Geriatría</option>
      </select>
      <div id="cards" class="grid"></div>
    </section>
  `;
  const deckSel = document.getElementById('deck');
  deckSel.addEventListener('change', () => loadDeck(deckSel.value));
  loadDeck(deckSel.value);

  async function loadDeck(name) {
    const data = await getJSON(`./data/flashcards/${name}.json`);
    const cards = document.getElementById('cards');
    cards.innerHTML = data.map(c => `
      <div class="card">
        <div class="tag">${name}</div>
        <p><strong>Pregunta:</strong> ${c.front}</p>
        <details>
          <summary>Ver respuesta</summary>
          <p>${c.back}</p>
        </details>
      </div>
    `).join('');
  }
}
// src/ui/testRunner.js
import { getJSON } from '../utils/fetchJSON.js';

export async function renderTests(el, type = 'mini') {
  el.innerHTML = `
    <section class="card">
      <h2>${type === 'mini' ? 'Mini‑tests' : 'Tests'}</h2>
      <label>Bloque</label>
      <select id="block">
        <option value="comunes">Comunes</option>
        <option value="tcae_sas">TCAE SAS</option>
        <option value="geriatria">Geriatría</option>
      </select>
      <label>Examen</label>
      <select id="exam"></select>
      <div id="quiz" class="card"></div>
    </section>
  `;
  const block = document.getElementById('block');
  const exam = document.getElementById('exam');
  block.addEventListener('change', () => fillExams());
  fillExams();

  async function fillExams() {
    const b = block.value;
    const list = ['mini1','mini2','mini3'];
    exam.innerHTML = list.map((n,i)=>`<option value="${n}">Mini‑test ${i+1}</option>`).join('');
    if (type !== 'mini') {
      const sims = ['simulacro1','simulacro2','simulacro3'];
      exam.innerHTML = sims.map((n,i)=>`<option value="${n}">Simulacro ${i+1}</option>`).join('');
    }
    loadExam(b, exam.value);
  }

  exam.addEventListener('change', () => loadExam(block.value, exam.value));

  async function loadExam(b, name) {
    const data = await getJSON(`./data/tests/${b}/${name}.json`);
    startQuiz(data);
  }

  function startQuiz(test) {
    const qEl = document.getElementById('quiz');
    let score = 0, idx = 0, answers = [];
    render();

    function render() {
      const q = test.items[idx];
      qEl.innerHTML = `
        <div class="tag">${test.meta.title}</div>
        <p><strong>Pregunta ${idx+1}/${test.items.length}:</strong> ${q.prompt}</p>
        ${q.options.map((opt,i)=>`
          <label><input type="radio" name="opt" value="${i}"> ${opt}</label><br/>
        `).join('')}
        <button class="button" id="next">Responder</button>
      `;
      document.getElementById('next').onclick = () => {
        const sel = qEl.querySelector('input[name="opt"]:checked');
        if (!sel) return;
        const chosen = Number(sel.value);
        answers.push({ id: q.id, chosen, correct: q.answerIndex });
        if (chosen === q.answerIndex) score++;
        idx++;
        if (idx < test.items.length) render();
        else finish();
      };
    }

    function finish() {
      const wrong = answers.filter(a => a.chosen !== a.correct);
      qEl.innerHTML = `
        <h3>Resultado: ${score} / ${test.items.length}</h3>
        <details open>
          <summary>Revisar explicaciones</summary>
          ${wrong.map(w => {
            const q = test.items.find(i => i.id === w.id);
            return `<div class="card">
              <p><strong>${q.prompt}</strong></p>
              <p><strong>Tu respuesta:</strong> ${q.options[w.chosen]}</p>
              <p><strong>Correcta:</strong> ${q.options[w.correct]}</p>
              <p>${q.explanation || ''}</p>
            </div>`;
          }).join('') || '<p>¡Perfecto! Todas correctas.</p>'}
        </details>
        <button class="button" onclick="location.reload()">Repetir</button>
      `;
    }
  }
}
// src/ui/simulacros.js
import { renderTests } from './testRunner.js';
export async function renderSimulacros(el) {
  return renderTests(el, 'sim');
}
// src/utils/fetchJSON.js
export async function getJSON(path) {
  const res = await fetch(path);
  if (!res.ok) throw new Error(`No se pudo cargar ${path}`);
  return res.json();
}
export async function getText(path) {
  const res = await fetch(path);
  if (!res.ok) throw new Error(`No se pudo cargar ${path}`);
  return res.text();
}
{
  "name": "TCAE & Aux. Geriatría – Estudio",
  "short_name": "TCAE Geriatría",
  "start_url": "./index.html",
  "display": "standalone",
  "background_color": "#0f1320",
  "theme_color": "#6ea8fe",
  "icons": [
    { "src": "./assets/icons/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "./assets/icons/icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
// sw.js
const CACHE = 'tcae-geriatria-v1';
const ASSETS = [
  './',
  './index.html',
  './assets/styles/main.css',
  './src/app.js',
  './src/router.js',
  './src/ui/nav.js',
  './src/ui/flashcards.js',
  './src/ui/testRunner.js',
  './src/ui/simulacros.js',
  './src/utils/fetchJSON.js',
  './manifest.json'
];
self.addEventListener('install', e => {
  e.waitUntil(caches.open(CACHE).then(c => c.addAll(ASSETS)));
});
self.addEventListener('fetch', e => {
  e.respondWith(
    caches.match(e.request).then(r => r || fetch(e.request).then(resp => {
      const copy = resp.clone();
      caches.open(CACHE).then(c => c.put(e.request, copy)).catch(()=>{});
      return resp;
    })).catch(()=>caches.match('./index.html'))
  );
});
# TCAE & Auxiliar de Geriatría – App de estudio

App web y APK ligera para preparar oposiciones (bloques comunes, TCAE SAS y geriatría).

## Funciones
- Resúmenes en Markdown
- Tarjetas flash (JSON)
- Mini‑tests (20 preguntas)
- Simulacros (50 preguntas)
- PWA offline

## Estructura
- `data/` → contenido (summaries, flashcards, tests)
- `src/` → componentes UI y router
- `assets/` → estilos e iconos

## Cómo ejecutar
- Abrir `index.html` localmente o publicar en GitHub Pages.

## Publicar en GitHub Pages
1. Subir todo el repo.
2. En Settings → Pages → Source: `Deploy from a branch`.
3. Branch: `main` / `gh-pages` y carpeta `/root`.
4. Espera el build y abre la URL pública.

## Añadir contenido
- Resúmenes: `data/summaries/*.md`
- Tarjetas: `data/flashcards/*.json`
- Tests: `data/tests/{comunes|tcae_sas|geriatria}/(mini|simulacro).json`

## Créditos
Hecho por Lourdes. Licencia MIT.
# .github/workflows/pages.yml
name: Deploy Pages
on:
  push:
    branches: [ main ]
permissions:
  contents: read
  pages: write
  id-token: write
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Pages
        uses: actions/configure-pages@v4
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: .
      - name: Deploy to GitHub Pages
        uses: actions/deploy-pages@v4
        <!-- data/summaries/comunes.md -->
# Bloque común
## Constitución
- **Soberanía en el pueblo español (art. 1.2)**
- **Derecho a la salud (art. 43)**
## Estatuto de Andalucía
- **Instituciones y competencias**
## Igualdad, Dependencia, PRL, Datos, Biobancos, Residuos
- **Puntos clave en listas y definiciones**
- // data/flashcards/comunes.json
[
  { "id": "constitucion_soberania", "front": "¿Dónde reside la soberanía?", "back": "**En el pueblo español.**" }

]
// data/tests/comunes/mini1.json
{
  "meta": { "title": "Mini-Test Común 1", "target": "comunes", "questions": 20, "timeMinutes": 20 },
  "items": [
    {
      "id": "q1",
      "prompt": "¿Dónde reside la soberanía nacional según la Constitución?",
      "options": ["Gobierno", "Tribunales", "Pueblo español", "Parlamento"],
      "answerIndex": 2,
      "explanation": "Art. 1.2."
    }
  ]
}
tcae-geriatria-app/
├─ index.html
├─ manifest.json
├─ sw.js
├─ README.md
├─ assets/
│  ├─ styles/main.css
│  └─ icons/icon-192.png, icon-512.png
├─ src/
│  ├─ app.js
│  ├─ router.js
│  ├─ ui/nav.js
│  ├─ ui/flashcards.js
│  ├─ ui/testRunner.js
│  ├─ ui/simulacros.js
│  └─ utils/fetchJSON.js
├─ data/
│  ├─ summaries/comunes.md, tcae_sas.md, geriatria.md
│  ├─ flashcards/comunes.json, tcae_sas.json, geriatria.json
│  └─ tests/{comunes,tcae_sas,geriatria}/mini1.json, simulacro1.json...
└─ .github/workflows/pages.yml
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>TCAE & Geriatría – Estudio</title>
  <link rel="stylesheet" href="./assets/styles/main.css" />
  <link rel="manifest" href="./manifest.json" />
</head>
<body>
  <header>
    <h1>TCAE & Aux. Geriatría</h1>
    <nav id="nav"></nav>
  </header>
  <main id="app"></main>
  <footer><small>Hecho con 💙 por Lourdes</small></footer>
  <script type="module" src="./src/app.js"></script>
</body>
</html>
body { margin:0; font-family:sans-serif; background:#0f1320; color:#e8ecff; }
header,footer { background:#161b2e; padding:1rem; }
nav a { margin:.25rem; padding:.5rem; border-radius:.5rem; text-decoration:none; color:#e8ecff; }
nav a.active { background:#6ea8fe; color:#0b0d18; }
.card { background:#161b2e; padding:1rem; margin:.5rem 0; border-radius:.5rem; }
.button { background:#6ea8fe; color:#0b0d18; border:none; padding:.5rem 1rem; border-radius:.5rem; cursor:pointer; }
import { initNav } from './ui/nav.js';
import { router } from './router.js';

initNav([
  { path: '#/resumenes', label: 'Resúmenes' },
  { path: '#/flashcards', label: 'Tarjetas' },
  { path: '#/mini-tests', label: 'Mini‑tests' },
  { path: '#/simulacros', label: 'Simulacros' }
]);

window.addEventListener('hashchange', router);
window.addEventListener('load', router);
import { renderHome } from './ui/flashcards.js';
import { renderFlashcards } from './ui/flashcards.js';
import { renderTests } from './ui/testRunner.js';
import { renderSimulacros } from './ui/simulacros.js';

export function router() {
  const app = document.getElementById('app');
  const route = location.hash || '#/resumenes';
  if (route.startsWith('#/resumenes')) return renderHome(app);
  if (route.startsWith('#/flashcards')) return renderFlashcards(app);
  if (route.startsWith('#/mini-tests')) return renderTests(app,'mini');
  if (route.startsWith('#/simulacros')) return renderSimulacros(app);
}
import { getJSON } from '../utils/fetchJSON.js';

export async function renderFlashcards(el) {
  el.innerHTML = `<h2>Tarjetas</h2><select id="deck">
    <option value="comunes">Comunes</option>
    <option value="tcae_sas">TCAE SAS</option>
    <option value="geriatria">Geriatría</option>
  </select><div id="cards"></div>`;
  const deckSel = document.getElementById('deck');
  deckSel.addEventListener('change',()=>loadDeck(deckSel.value));
  loadDeck(deckSel.value);

  async function loadDeck(name){
    const data = await getJSON(`./data/flashcards/${name}.json`);
    document.getElementById('cards').innerHTML = data.map(c=>`
      <div class="card"><p><strong>${c.front}</strong></p><p>${c.back}</p></div>
    `).join('');
  }
}
import { getJSON } from '../utils/fetchJSON.js';

export async function renderTests(el,type='mini'){
  el.innerHTML = `<h2>${type==='mini'?'Mini‑tests':'Simulacros'}</h2>
    <select id="exam"></select><div id="quiz"></div>`;
  const exam=document.getElementById('exam');
  exam.innerHTML=['1','2','3'].map(n=>`<option value="${type}${n}">${type} ${n}</option>`).join('');
  exam.addEventListener('change',()=>loadExam(exam.value));
  loadExam(exam.value);

  async function loadExam(name){
    const data=await getJSON(`./data/tests/comunes/${name}.json`);
    startQuiz(data);
  }
  function startQuiz(test){
    let idx=0,score=0;
    const quiz=document.getElementById('quiz');
    render();
    function render(){
      const q=test.items[idx];
      quiz.innerHTML=`<p>${q.prompt}</p>${q.options.map((o,i)=>`<label><input type="radio" name="opt" value="${i}">${o}</label>`).join('')}<button id="next">Responder</button>`;
      document.getElementById('next').onclick=()=>{
        const sel=quiz.querySelector('input[name="opt"]:checked');
        if(!sel)return;
        if(Number(sel.value)===q.answerIndex)score++;
        idx++;
        if(idx<test.items.length)render();else quiz.innerHTML=`<h3>Resultado: ${score}/${test.items.length}</h3>`;
      };
    }
  }
}
{
  "name": "TCAE & Aux. Geriatría",
  "short_name": "TCAE Geriatría",
  "start_url": "./index.html",
  "display": "standalone",
  "background_color": "#0f1320",
  "theme_color": "#6ea8fe",
  "icons": [
    { "src": "./assets/icons/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "./assets/icons/icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
const CACHE='tcae-geriatria-v1';
const ASSETS=['./','./index.html','./assets/styles/main.css','./src/app.js'];
self.addEventListener('install',e=>e.waitUntil(caches.open(CACHE).then(c=>c.addAll(ASSETS))));
self.addEventListener('fetch',e=>e.respondWith(caches.match(e.request).then(r=>r||fetch(e.request))));
# TCAE & Auxiliar de Geriatría – App de estudio

App web ligera para preparar oposiciones.

## Funciones
- Resúmenes en Markdown
- Tarjetas flash (JSON)
- Mini‑tests (20 preguntas)
- Simulacros (50 preguntas)
- PWA offline

## Publicar en GitHub Pages
1. Subir repo.
2. Settings → Pages → Deploy from branch → main.
3. Abrir URL pública.
   name: Deploy Pages
on:
  push:
    branches: [ main ]
permissions:
  contents: read
  pages: write
  id-token: write
jobs:
  deploy:
    runs-on: ubuntu-latest
   
    steps:
      - uses: actions/checkout@v4
      - uses: actions/configure-pages@v4
      - uses: actions/upload-pages-artifact@v3
        with: { path: . }
      - uses: actions/deploy-pages@v4
https://doc.termux.com
