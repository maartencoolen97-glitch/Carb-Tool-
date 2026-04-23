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

function splitLines(value) {
  return value.split('\n').map(s => s.trim()).filter(Boolean);
}

function dataFromForm() {
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

function esc(s) {
  return String(s).replace(/[&<>"']/g, function (match) {
    const map = {
      '&': '&amp;',
      '<': '&lt;',
      '>': '&gt;',
      '"': '&quot;',
      "'": '&#39;'
    };
    return map[match];
  });
}

function renderPreview() {
  const data = dataFromForm();

  const metaParts = [];
  if (data.serves) metaParts.push(`Serves: ${esc(data.serves)}`);
  if (data.time) metaParts.push(`Time: ${esc(data.time)}`);
  if (data.difficulty) metaParts.push(`Difficulty: ${esc(data.difficulty)}`);

  els.preview.innerHTML = `
    <h1 class="recipe-title">${esc(data.title || 'Jouw recept')}</h1>
    ${data.intro ? `<p class="intro">${esc(data.intro)}</p>` : ''}
    ${metaParts.length ? `<p class="meta">${metaParts.join(' · ')}</p>` : ''}

    <div>
      <div class="section-title">Ingredients</div>
      ${
        data.ingredients.length
          ? `<ul>${data.ingredients.map(item => `<li>${esc(item)}</li>`).join('')}</ul>`
          : `<p>Geen ingrediënten ingevuld.</p>`
      }
    </div>

    <div>
      <div class="section-title">Preparation</div>
      ${
        data.preparation.length
          ? `<ol>${data.preparation.map(step => `<li>${esc(step)}</li>`).join('')}</ol>`
          : `<p>Geen bereidingsstappen ingevuld.</p>`
      }
    </div>

    ${
      data.notes.length
        ? `
          <div>
            <div class="section-title">${esc(data.notesTitle)}</div>
            <ul>${data.notes.map(note => `<li>${esc(note)}</li>`).join('')}</ul>
          </div>
        `
        : ''
    }

    ${data.closing ? `<p class="closing">${esc(data.closing)}</p>` : ''}
  `;
}

async function generateWord() {
  const data = dataFromForm();

  const children = [];

  children.push(
    new Paragraph({
      alignment: AlignmentType.CENTER,
      children: [
        new TextRun({
          text: data.title || 'Jouw recept',
          bold: true,
          size: 32
        })
      ]
    })
  );

  if (data.intro) {
    children.push(
      new Paragraph({
        children: [new TextRun({ text: data.intro, italics: true })]
      })
    );
  }

  const meta = [];
  if (data.serves) meta.push(`Serves: ${data.serves}`);
  if (data.time) meta.push(`Time: ${data.time}`);
  if (data.difficulty) meta.push(`Difficulty: ${data.difficulty}`);

  if (meta.length) {
    children.push(
      new Paragraph({
        children: [new TextRun({ text: meta.join(' · '), bold: true })]
      })
    );
  }

  children.push(
    new Paragraph({
      children: [new TextRun({ text: 'Ingredients', bold: true, size: 28 })]
    })
  );

  if (data.ingredients.length) {
    data.ingredients.forEach(item => {
      children.push(
        new Paragraph({
          text: item,
          bullet: { level: 0 }
        })
      );
    });
  } else {
    children.push(new Paragraph('Geen ingrediënten ingevuld.'));
  }

  children.push(
    new Paragraph({
      children: [new TextRun({ text: 'Preparation', bold: true, size: 28 })]
    })
  );

  if (data.preparation.length) {
    data.preparation.forEach(step => {
      children.push(
        new Paragraph({
          text: step,
          numbering: {
            reference: "steps-numbering",
            level: 0
          }
        })
      );
    });
  } else {
    children.push(new Paragraph('Geen bereidingsstappen ingevuld.'));
  }

  if (data.notes.length) {
    children.push(
      new Paragraph({
        children: [new TextRun({ text: data.notesTitle, bold: true, size: 28 })]
      })
    );

    data.notes.forEach(note => {
      children.push(
        new Paragraph({
          text: note,
          bullet: { level: 0 }
        })
      );
    });
  }

  if (data.closing) {
    children.push(
      new Paragraph({
        alignment: AlignmentType.CENTER,
        children: [new TextRun({ text: data.closing, bold: true })]
      })
    );
  }

  const doc = new Document({
    numbering: {
      config: [
        {
          reference: "steps-numbering",
          levels: [
            {
              level: 0,
              format: "decimal",
              text: "%1.",
              alignment: AlignmentType.START
            }
          ]
        }
      ]
    },
    sections: [
      {
        properties: {},
        children
      }
    ]
  });

  const blob = await Packer.toBlob(doc);
  const filename = (data.title || 'recept')
    .toLowerCase()
    .replace(/[^a-z0-9à-ž]+/gi, '-')
    .replace(/^-+|-+$/g, '') + '.docx';

  saveAs(blob, filename);
}

function clearForm() {
  els.title.value = '';
  els.intro.value = '';
  els.serves.value = '';
  els.time.value = '';
  els.difficulty.value = '';
  els.ingredients.value = '';
  els.preparation.value = '';
  els.notesTitle.value = '';
  els.notes.value = '';
  els.closing.value = '';
  renderPreview();
}

[
  els.title,
  els.intro,
  els.serves,
  els.time,
  els.difficulty,
  els.ingredients,
  els.preparation,
  els.notesTitle,
  els.notes,
  els.closing
].forEach(el => {
  el.addEventListener('input', renderPreview);
});

els.downloadBtn.addEventListener('click', generateWord);
els.clearBtn.addEventListener('click', clearForm);

renderPreview();
</script>
