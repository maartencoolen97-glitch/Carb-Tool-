<!doctype html>
<html lang="nl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Carbonara Style Recipe Tool - Leeg</title>
  <script src="https://unpkg.com/docx@8.5.0/build/index.umd.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/file-saver@2.0.5/dist/FileSaver.min.js"></script>
  <style>
    :root{
      --bg:#111111; --panel:#1b1b1b; --muted:#b7b7b7; --text:#f4f1ea; --line:#2c2c2c;
      --accent:#d4b483;
    }
    *{box-sizing:border-box}
    body{margin:0; font-family:Georgia, "Times New Roman", serif; background:var(--bg); color:var(--text);}
    .wrap{max-width:1200px; margin:0 auto; padding:24px;}
    .hero{display:grid; grid-template-columns:1.1fr .9fr; gap:24px; align-items:start;}
    .card{
      background:var(--panel); border:1px solid var(--line); border-radius:18px; padding:20px;
      box-shadow:0 10px 30px rgba(0,0,0,.25);
    }
    h1{margin:0 0 8px; font-size:34px; line-height:1.1}
    h2{margin:0 0 14px; font-size:20px}
    .sub{color:var(--muted); font-size:16px; line-height:1.5}
    .grid2{display:grid; grid-template-columns:1fr 1fr; gap:12px}
    label{display:block; font-size:13px; text-transform:uppercase; letter-spacing:.08em; color:var(--accent); margin:14px 0 6px}
    input, textarea{
      width:100%; background:#121212; color:var(--text); border:1px solid #343434; border-radius:12px;
      padding:12px 14px; font:inherit;
    }
    textarea{min-height:96px; resize:vertical}
    .small{min-height:68px}
    .actions{display:flex; gap:12px; flex-wrap:wrap; margin-top:18px}
    button{border:0; border-radius:999px; padding:12px 18px; cursor:pointer; font:inherit; font-weight:bold;}
    .primary{background:var(--accent); color:#151515}
    .secondary{background:#262626; color:var(--text); border:1px solid #3b3b3b}
    .preview{
      background:#f8f5ef; color:#202020; border-radius:14px; padding:34px; min-height:700px;
      box-shadow: inset 0 0 0 1px rgba(0,0,0,.06);
    }
    .recipe-title{font-size:38px; text-align:center; margin:0 0 20px; font-weight:700}
    .intro{font-style:italic; margin:0 0 18px; line-height:1.6}
    .meta{font-weight:700; margin:0 0 20px}
    .section-title{font-size:22px; margin:24px 0 10px; font-weight:700}
    ul,ol{margin:0 0 18px 22px; padding:0}
    li{margin:0 0 8px; line-height:1.5}
    .closing{text-align:center; font-weight:700; margin-top:24px}
    .footer-note{margin-top:14px; font-size:13px; color:var(--muted)}
    @media (max-width: 900px){
      .hero{grid-template-columns:1fr}
      .preview{min-height:auto}
    }
  </style>
</head>
<body>
  <div class="wrap">
    <div class="hero">
      <div class="card">
        <h1>Carbonara Style Recipe Tool</h1>
        <p class="sub">Lege versie. Vul je recept vanaf nul in en genereer direct een Wordbestand in jouw Carbonara-template.</p>

        <label for="title">Titel</label>
        <input id="title" value="" placeholder="Bijv. Carbonara" />

        <label for="intro">Intro</label>
        <textarea id="intro" placeholder="Korte intro / persoonlijke tekst"></textarea>

        <div class="grid2">
          <div>
            <label for="serves">Serves</label>
            <input id="serves" value="" placeholder="Bijv. 2" />
          </div>
          <div>
            <label for="time">Time</label>
            <input id="time" value="" placeholder="Bijv. 20 min" />
          </div>
        </div>

        <label for="difficulty">Difficulty</label>
        <input id="difficulty" value="" placeholder="Bijv. Easy / Medium / Hard" />

        <label for="ingredients">Ingredients (één per regel)</label>
        <textarea id="ingredients" placeholder="Eerste ingrediënt&#10;Tweede ingrediënt&#10;Derde ingrediënt"></textarea>

        <label for="preparation">Preparation (één stap per regel)</label>
        <textarea id="preparation" placeholder="Stap 1&#10;Stap 2&#10;Stap 3"></textarea>

        <label for="notesTitle">Notes titel</label>
        <input id="notesTitle" value="" placeholder="Bijv. Notes / Tips / Meal Prep" />

        <label for="notes">Notes (één per regel, optioneel)</label>
        <textarea id="notes" class="small" placeholder="Optionele notitie 1&#10;Optionele notitie 2"></textarea>

        <label for="closing">Afsluitzin</label>
        <input id="closing" value="" placeholder="Bijv. En-fucking-joy." />

        <div class="actions">
          <button class="primary" id="downloadBtn">Genereer Wordbestand</button>
          <button class="secondary" id="clearBtn">Maak alles leeg</button>
        </div>
        <div class="footer-note">Bestandsnaam wordt automatisch afgeleid van de titel.</div>
      </div>

      <div class="card">
        <h2>Live preview</h2>
        <div class="preview" id="preview"></div>
      </div>
    </div>
  </div>

<script>
const { Document, Packer, Paragraph, TextRun, AlignmentType } = docx;

const els = {
  title: document.getElementById('title'),
  intro: document.getElementById('intro'),
  serves: document.getElementById('serves'),
  time: document.getElementById('time'),
  difficulty: document.getElementById('difficulty'),
  ingredients: document.getElementById('ingredients'),
  preparation: document.getElementById('preparation'),
  notesTitle: document.getElementById('notesTitle'),
  notes: document.getElementById('notes'),
  closing: document.getElementById('closing'),
  preview: document.getElementById('preview'),
  downloadBtn: document.getElementById('downloadBtn'),
  clearBtn: document.getElementById('clearBtn')
};

function splitLines(value){
  return value.split('\n').map(s => s.trim()).filter(Boolean);
}

function dataFromForm(){
  return {
    title: els.title.value.trim(),
    intro: els.intro.value.trim(),
    serves: els.serves.value.trim(),
    time: els.time.value.trim(),
    difficulty: els.difficulty.value.trim(),
    ingredients: splitLines(els.ingredients.value),
    preparation: splitLines(els.preparation.value),
    notesTitle: els.notesTitle.value.trim() || 'Notes',
    notes: splitLines(els.notes.value),
    closing: els.closing.value.trim()
  };
}

function esc(s){
  return s.replace(/[&<>\
