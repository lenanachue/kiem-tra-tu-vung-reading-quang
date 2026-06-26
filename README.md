<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Trạm Ôn Tập Từ Vựng - Bài Kiểm Tra Tổng Hợp</title>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    min-height: 100vh;
    background: linear-gradient(180deg, #eff6ff 0%, #dbeafe 40%, #eff6ff 100%);
    font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
    position: relative;
    overflow-x: hidden;
    color: #1e293b;
  }

  @keyframes floatUp {
    0%   { transform: translateY(0) rotate(0deg); opacity: 0; }
    10%  { opacity: 0.5; }
    85%  { opacity: 0.35; }
    100% { transform: translateY(-110vh) rotate(15deg); opacity: 0; }
  }
  @keyframes waveFloat {
    0%, 100% { transform: translateY(0px) rotate(0deg); }
    50%       { transform: translateY(-6px) rotate(6deg); }
  }
  @keyframes bloomPop {
    0%   { transform: scale(0.6); opacity: 0; }
    100% { transform: scale(1); opacity: 1; }
  }
  button:hover { filter: brightness(1.05); }

  /* Floating decor */
  #decor {
    position: fixed; top: 0; left: 0; right: 0; bottom: 0;
    pointer-events: none; overflow: hidden; z-index: 0;
  }
  .float-icon { position: absolute; font-size: 22px; opacity: 0; animation: floatUp linear infinite; }

  /* Header */
  #header {
    background: linear-gradient(135deg, #1d4ed8 0%, #3b82f6 50%, #60a5fa 100%);
    padding: 28px 20px 20px;
    text-align: center;
    position: relative;
    overflow: hidden;
    z-index: 1;
  }
  #header .wave-bottom { position: absolute; bottom: -2px; left: 0; right: 0; height: 20px; }
  #header .wave-bottom svg { width: 100%; height: 100%; display: block; }
  #header-icon { animation: waveFloat 3s ease-in-out infinite; font-size: 36px; margin-bottom: 4px; }
  #header h1 {
    color: #fff; font-size: 20px; font-weight: 800;
    letter-spacing: 0.5px; text-shadow: 0 2px 8px rgba(0,0,0,0.15);
    margin-bottom: 4px;
  }
  #header p { color: rgba(255,255,255,0.92); font-size: 13px; }

  /* Tabs */
  #tabs {
    display: flex; gap: 0; padding: 12px 16px 0;
    position: relative; z-index: 1; overflow-x: auto;
  }
  .tab-btn {
    flex: 1; min-width: 110px; padding: 10px 8px;
    border: none; border-bottom: 3px solid transparent;
    background: transparent; color: #2563eb; font-weight: 500;
    font-size: 12.5px; cursor: pointer;
    border-radius: 10px 10px 0 0; transition: all 0.2s ease;
    white-space: nowrap;
  }
  .tab-btn.active {
    border-bottom-color: #1d4ed8;
    background: rgba(59,130,246,0.14);
    color: #1e3a8a; font-weight: 700;
  }

  /* Content */
  #content { padding: 16px 16px 40px; max-width: 740px; margin: 0 auto; position: relative; z-index: 1; }
  .tab-panel { display: none; }
  .tab-panel.active { display: block; animation: bloomPop 0.35s ease; }

  h2.section-title { color: #1e3a8a; font-size: 18px; font-weight: 700; margin: 0 0 6px; }
  p.section-sub    { color: #2563eb; font-size: 13px; margin: 0 0 16px; line-height: 1.5; }

  .card-box {
    background: #fff; border-radius: 14px; padding: 16px 18px;
    margin-bottom: 12px; border: 1.5px solid #bfdbfe;
    box-shadow: 0 2px 8px rgba(37,99,235,0.08);
  }

  /* Part 1: flashcards */
  .vocab-grid { display: grid; gap: 14px; }
  .vcard {
    background: #fff; border: 1.5px solid #bfdbfe; border-radius: 16px;
    padding: 18px 20px; box-shadow: 0 3px 10px rgba(37,99,235,0.09);
    position: relative;
  }
  .vcard .vc-deco { position: absolute; top: 12px; right: 16px; font-size: 22px; opacity: 0.55; }
  .vc-num {
    display: inline-block; width: 24px; height: 24px; line-height: 24px; text-align: center;
    background: #1d4ed8; color: #fff; border-radius: 50%; font-size: 11.5px; font-weight: 800;
    margin-right: 8px;
  }
  .vc-word { font-size: 21px; font-weight: 800; color: #1e3a8a; display: inline-block; vertical-align: middle; }
  .vc-ipa  { font-size: 14px; color: #2563eb; font-style: italic; margin: 6px 0 8px 32px; }
  .vc-pos  {
    display: inline-block; background: #dbeafe; color: #1e40af; font-weight: 700;
    font-size: 11px; padding: 3px 10px; border-radius: 8px; margin-left: 32px;
  }
  .vc-reveal-btn {
    display: block; margin: 14px 0 0 32px; background: linear-gradient(135deg, #93c5fd, #60a5fa);
    color: #1e3a8a; border: none; border-radius: 10px; padding: 8px 16px;
    font-size: 12.5px; font-weight: 700; cursor: pointer;
  }
  .vc-meaning-box {
    display: none; margin: 12px 0 0 32px; padding: 12px 14px;
    background: linear-gradient(135deg, #eff6ff, #dbeafe); border-radius: 10px;
    border: 1px solid #bfdbfe;
  }
  .vc-meaning-box.show { display: block; animation: bloomPop 0.3s ease; }
  .vc-meaning { font-size: 14.5px; font-weight: 700; color: #1d4ed8; margin-bottom: 6px; }
  .vc-example { font-size: 12.5px; color: #475569; font-style: italic; margin-bottom: 3px; line-height: 1.5; }
  .vc-example-vi { font-size: 11.5px; color: #2563eb; line-height: 1.5; }

  /* Match rows (Part 2) */
  .match-row { display: flex; align-items: center; gap: 12px; flex-wrap: wrap; }
  .match-dir {
    display: inline-block; font-size: 9.5px; font-weight: 800; padding: 2px 8px;
    border-radius: 6px; margin-bottom: 5px; letter-spacing: 0.5px;
  }
  .match-dir.ev { background: #dbeafe; color: #1d4ed8; }
  .match-dir.ve { background: #e0f2fe; color: #0369a1; }
  .match-prompt { min-width: 150px; flex: 1; }
  .match-prompt .mp-text { font-size: 15px; font-weight: 800; color: #1e3a8a; }
  .match-prompt .mp-ipa  { font-size: 12px; color: #2563eb; font-style: italic; }
  .match-select { flex: 1.3; min-width: 200px; }

  select.field-select {
    width: 100%; background: #f0f7ff; border: 1.5px solid #bfdbfe; border-radius: 10px;
    padding: 9px 12px; font-size: 13.5px; color: #334155; cursor: pointer;
  }
  select.field-select:disabled { cursor: default; opacity: 0.95; }
  select.field-select.correct { border-color: #81c995; background: #e8fdf1; }
  select.field-select.wrong   { border-color: #e88e8e; background: #fde8e8; }
  .row-result-icon { font-size: 16px; width: 22px; text-align: center; flex-shrink: 0; }

  /* Part 3: fill-in sentences */
  .word-bank { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 16px; }
  .word-chip {
    background: #dbeafe; border: 1px solid #bfdbfe; color: #1e40af;
    font-size: 12.5px; font-weight: 700; padding: 5px 12px; border-radius: 999px;
  }
  .fill-sentence { font-size: 14px; color: #334155; line-height: 1.9; margin-bottom: 6px; }
  .fill-vi { font-size: 12.5px; color: #2563eb; font-style: italic; line-height: 1.5; }
  .inline-select {
    display: inline-block; background: #fff; color: #1e3a8a;
    font-weight: 700; font-size: 13px; padding: 2px 6px;
    border: 1.5px solid #3b82f6; border-radius: 6px; margin: 0 2px;
    cursor: pointer; max-width: 170px; vertical-align: middle;
  }
  .inline-select.correct { border-color: #81c995; background: #e8fdf1; color: #2c3e50; }
  .inline-select.wrong   { border-color: #e88e8e; background: #fde8e8; color: #2c3e50; }
  .inline-select:disabled { cursor: default; opacity: 0.95; }

  .explanation {
    margin-top: 8px; padding: 9px 13px; border-radius: 10px;
    font-size: 12.5px; color: #334155; line-height: 1.55;
  }
  .explanation.correct { background: rgba(129,201,149,0.15); }
  .explanation.wrong   { background: rgba(232,142,142,0.15); }

  /* Buttons */
  .btn-row { display: flex; justify-content: center; margin-top: 16px; gap: 10px; flex-wrap: wrap; }
  .btn-primary {
    background: linear-gradient(135deg, #3b82f6, #1d4ed8); color: #fff;
    border: none; border-radius: 12px; padding: 12px 36px;
    font-size: 15px; font-weight: 700; cursor: pointer;
    box-shadow: 0 4px 14px rgba(29,78,216,0.28);
  }
  .btn-reset {
    background: linear-gradient(135deg, #93c5fd, #60a5fa); color: #1e3a8a;
    border: none; border-radius: 12px; padding: 12px 36px;
    font-size: 15px; font-weight: 700; cursor: pointer;
    box-shadow: 0 4px 14px rgba(96,165,250,0.28);
  }
  .score-box {
    background: linear-gradient(135deg, #dbeafe 0%, #93c5fd 100%);
    border-radius: 16px; padding: 20px 24px; margin: 14px 0;
    text-align: center; border: 2px solid #bfdbfe;
  }
  .score-emoji { font-size: 30px; margin-bottom: 6px; }
  .score-pct   { font-size: 26px; font-weight: 800; color: #1e3a8a; margin-bottom: 6px; }
  .score-detail { display: flex; justify-content: center; gap: 22px; font-size: 13.5px; color: #334155; }

  .part-note {
    font-size: 12px; color: #1d4ed8; background: rgba(59,130,246,0.08);
    border-radius: 10px; padding: 8px 12px; margin-bottom: 12px;
  }
</style>
</head>
<body>

<div id="decor"></div>

<div id="header">
  <div class="wave-bottom">
    <svg viewBox="0 0 1200 40" preserveAspectRatio="none">
      <path d="M0,20 Q150,0 300,20 T600,20 T900,20 T1200,20 L1200,40 L0,40 Z" fill="#eff6ff"/>
    </svg>
  </div>
  <div id="header-icon">📘</div>
  <h1>Trạm Ôn Tập Từ Vựng</h1>
  <p>Đọc kỹ - Nhớ chắc - Áp dụng đúng!</p>
</div>

<div id="tabs">
  <button class="tab-btn active" data-tab="read">📖 Đọc từ vựng</button>
  <button class="tab-btn" data-tab="twoway">🔄 Kiểm tra 2 chiều</button>
  <button class="tab-btn" data-tab="apply">✍️ Bài tập áp dụng</button>
</div>

<div id="content">

  <!-- PHẦN 1: ĐỌC TỪ VỰNG -->
  <div id="tab-read" class="tab-panel active">
    <h2 class="section-title">📖 Phần 1 - Đọc từ vựng</h2>
    <p class="section-sub">Nhìn phiên âm (IPA) và tự đọc to từng từ trước. Cố nhớ lại nghĩa trong đầu, sau đó mới bấm "Xem nghĩa" để kiểm tra.</p>
    <div class="vocab-grid" id="vocab-grid"></div>
  </div>

  <!-- PHẦN 2: KIỂM TRA 2 CHIỀU -->
  <div id="tab-twoway" class="tab-panel">
    <h2 class="section-title">🔄 Phần 2 - Kiểm tra 2 chiều Anh - Việt</h2>
    <p class="section-sub">Mỗi từ được hỏi theo cả 2 chiều: nhìn từ tiếng Anh đoán nghĩa, và nhìn nghĩa tiếng Việt đoán từ tiếng Anh - để chắc chắn em nhớ cả 2 mặt.</p>
    <div id="twoway-container"></div>
  </div>

  <!-- PHẦN 3: BÀI TẬP ÁP DỤNG -->
  <div id="tab-apply" class="tab-panel">
    <h2 class="section-title">✍️ Phần 3 - Bài tập áp dụng</h2>
    <p class="section-sub">Điền từ đúng vào chỗ trống để hoàn thành câu - đây là lúc kiểm tra em có dùng được từ trong ngữ cảnh thật không.</p>
    <div id="apply-container"></div>
  </div>

</div>

<script>
// ─── DATA: 10 TỪ VỰNG ──────────────────────────────────────────────────────

const VOCAB = [
  { word:"occasion", ipa:"/əˈkeɪ.ʒən/", pos:"danh từ (n)",
    meaning:"dịp, sự kiện đặc biệt",
    example:`They bought new clothes for the occasion.`,
    exampleVi:`Họ mua quần áo mới cho dịp đặc biệt này.` },
  { word:"reunion", ipa:"/riːˈjuː.ni.ən/", pos:"danh từ (n)",
    meaning:"buổi sum họp, đoàn tụ",
    example:`The whole family had a reunion dinner on New Year's Eve.`,
    exampleVi:`Cả gia đình có một bữa cơm sum họp vào đêm giao thừa.` },
  { word:"traditional", ipa:"/trəˈdɪʃ.ən.əl/", pos:"tính từ (adj)",
    meaning:"(thuộc) truyền thống",
    example:`Banh chung is a traditional Vietnamese cake.`,
    exampleVi:`Bánh chưng là một loại bánh truyền thống của Việt Nam.` },
  { word:"elder", ipa:"/ˈel.dər/", pos:"danh từ (n)",
    meaning:"người lớn tuổi, người cao niên",
    example:`Children pay their respects to the elders during Tet.`,
    exampleVi:`Trẻ em thể hiện sự tôn trọng với người lớn tuổi trong ngày Tết.` },
  { word:"wrap", ipa:"/ræp/", pos:"động từ (v)",
    meaning:"gói, bọc",
    example:`Lucky money is wrapped in red envelopes.`,
    exampleVi:`Tiền lì xì được gói trong những phong bì đỏ.` },
  { word:"addiction", ipa:"/əˈdɪk.ʃən/", pos:"danh từ (n)",
    meaning:"sự nghiện, chứng nghiện",
    example:`Spending too much time online can lead to Internet addiction.`,
    exampleVi:`Dành quá nhiều thời gian trên mạng có thể dẫn đến nghiện Internet.` },
  { word:"addict", ipa:"/ˈæd.ɪkt/", pos:"danh từ (n)",
    meaning:"người nghiện (cái gì)",
    example:`Internet addicts often forget about their real lives.`,
    exampleVi:`Người nghiện Internet thường quên đi cuộc sống thực của mình.` },
  { word:"instead", ipa:"/ɪnˈsted/", pos:"trạng từ (adv)",
    meaning:"thay vào đó",
    example:`They don't talk to friends; instead, they chat online all day.`,
    exampleVi:`Họ không nói chuyện với bạn bè; thay vào đó, họ trò chuyện trực tuyến cả ngày.` },
  { word:"stress", ipa:"/stres/", pos:"danh từ (n)",
    meaning:"sự căng thẳng, áp lực",
    example:`Some people use the Internet to escape from stress.`,
    exampleVi:`Một số người dùng Internet để thoát khỏi căng thẳng.` },
  { word:"escape", ipa:"/ɪˈskeɪp/", pos:"động từ (v)",
    meaning:"trốn thoát, thoát khỏi",
    example:`She wanted to escape from her problems for a while.`,
    exampleVi:`Cô ấy muốn thoát khỏi các vấn đề của mình trong một thời gian.` }
];

// ─── HELPERS ───────────────────────────────────────────────────────────────

function shuffle(arr) {
  const a = arr.slice();
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}

// ─── DECOR (icon học tập trôi nổi) ─────────────────────────────────────────

function renderDecor() {
  const ICONS = ["📚","✏️","🎓","📝","📌","🔖","📐","🧮","📏","✒️"];
  const container = document.getElementById("decor");
  for (let i = 0; i < 14; i++) {
    const el = document.createElement("span");
    el.className = "float-icon";
    el.textContent = ICONS[i % ICONS.length];
    el.style.left = `${Math.random() * 92}%`;
    el.style.bottom = `-${20 + Math.random() * 40}px`;
    el.style.fontSize = `${16 + Math.random() * 14}px`;
    el.style.animationDuration = `${10 + Math.random() * 8}s`;
    el.style.animationDelay = `${Math.random() * 8}s`;
    container.appendChild(el);
  }
}

// ─── PHẦN 1: ĐỌC TỪ VỰNG (FLASHCARD ẨN NGHĨA) ──────────────────────────────

function renderVocabRead() {
  const grid = document.getElementById("vocab-grid");
  grid.innerHTML = VOCAB.map((v, i) => `
    <div class="vcard">
      <span class="vc-deco">📘</span>
      <span class="vc-num">${i + 1}</span><span class="vc-word">${v.word}</span>
      <div class="vc-ipa">${v.ipa}</div>
      <span class="vc-pos">${v.pos}</span>
      <button class="vc-reveal-btn" onclick="toggleReveal(${i})" id="btn-${i}">👁️ Xem nghĩa</button>
      <div class="vc-meaning-box" id="meaning-${i}">
        <div class="vc-meaning">🇻🇳 ${v.meaning}</div>
        <div class="vc-example">"${v.example}"</div>
        <div class="vc-example-vi">→ ${v.exampleVi}</div>
      </div>
    </div>
  `).join("");
}

function toggleReveal(i) {
  const box = document.getElementById("meaning-" + i);
  const btn = document.getElementById("btn-" + i);
  const showing = box.classList.toggle("show");
  btn.textContent = showing ? "🙈 Ẩn nghĩa" : "👁️ Xem nghĩa";
}

// ─── PHẦN 2: KIỂM TRA 2 CHIỀU ───────────────────────────────────────────────

let twowayState;

function buildTwoway() {
  const qs = [];
  VOCAB.forEach((v, i) => {
    const others = VOCAB.filter((_, j) => j !== i);
    // Chiều EV: cho từ tiếng Anh, đoán nghĩa tiếng Việt
    const evDistractors = shuffle(others).slice(0, 3).map(o => o.meaning);
    qs.push({
      dir: "ev", wordIdx: i,
      options: shuffle([v.meaning, ...evDistractors]),
      correct: v.meaning
    });
    // Chiều VE: cho nghĩa tiếng Việt, đoán từ tiếng Anh
    const veDistractors = shuffle(others).slice(0, 3).map(o => o.word);
    qs.push({
      dir: "ve", wordIdx: i,
      options: shuffle([v.word, ...veDistractors]),
      correct: v.word
    });
  });
  return { questions: shuffle(qs), selected: {}, submitted: false };
}

function renderTwoway() {
  const container = document.getElementById("twoway-container");
  const s = twowayState;
  const submitted = s.submitted;

  let rows = s.questions.map((q, qi) => {
    const v = VOCAB[q.wordIdx];
    const sel = s.selected[qi];
    const isCorrect = submitted && sel === q.correct;
    let cls = "field-select";
    if (submitted) cls += isCorrect ? " correct" : " wrong";

    let opts = `<option value="" disabled ${sel === undefined ? "selected" : ""}>-- chọn đáp án --</option>`;
    q.options.forEach(o => {
      opts += `<option value="${o}" ${sel === o ? "selected" : ""}>${o}</option>`;
    });

    const icon = !submitted ? "" : (isCorrect ? "✅" : "❌");
    const dirLabel = q.dir === "ev"
      ? `<span class="match-dir ev">ANH → VIỆT</span>`
      : `<span class="match-dir ve">VIỆT → ANH</span>`;
    const promptHtml = q.dir === "ev"
      ? `<div class="mp-text">${v.word}</div><div class="mp-ipa">${v.ipa}</div>`
      : `<div class="mp-text" style="font-size:13.5px;">${v.meaning}</div>`;

    return `
      <div class="card-box match-row">
        <div class="match-prompt">${dirLabel}<br/>${promptHtml}</div>
        <div class="match-select">
          <select class="${cls}" ${submitted ? "disabled" : ""} onchange="selectTwoway(${qi}, this.value)">${opts}</select>
          ${submitted && !isCorrect ? `<div class="explanation wrong">Đáp án đúng: <strong>${q.correct}</strong></div>` : ""}
        </div>
        <div class="row-result-icon">${icon}</div>
      </div>
    `;
  }).join("");

  let html = `<div class="part-note">Tổng cộng ${s.questions.length} câu hỏi (10 từ × 2 chiều), đã trộn ngẫu nhiên.</div>` + rows;

  if (submitted) {
    const correct = s.questions.filter((q, qi) => s.selected[qi] === q.correct).length;
    const total = s.questions.length;
    const pct = Math.round((correct / total) * 100);
    const e = pct >= 80 ? "🎉" : pct >= 50 ? "👍" : "💪";
    html += `
      <div class="score-box">
        <div class="score-emoji">${e}</div>
        <div class="score-pct">${pct}%</div>
        <div class="score-detail">
          <span>✅ Đúng: <strong style="color:#27ae60">${correct}</strong></span>
          <span>❌ Sai: <strong style="color:#e74c3c">${total - correct}</strong></span>
        </div>
      </div>
    `;
  }

  html += `
    <div class="btn-row">
      ${!submitted
        ? `<button class="btn-primary" onclick="submitTwoway()">📘 Nộp bài Phần 2</button>`
        : `<button class="btn-reset" onclick="resetTwoway()">🔄 Làm lại Phần 2</button>`
      }
    </div>
  `;

  container.innerHTML = html;
}

function selectTwoway(qi, value) {
  if (twowayState.submitted) return;
  twowayState.selected[qi] = value;
  renderTwoway();
}

function submitTwoway() {
  if (Object.keys(twowayState.selected).length < twowayState.questions.length) {
    alert("Em hãy hoàn thành tất cả câu hỏi trước khi nộp bài nhé!");
    return;
  }
  twowayState.submitted = true;
  renderTwoway();
}

function resetTwoway() {
  twowayState = buildTwoway();
  renderTwoway();
}

// ─── PHẦN 3: BÀI TẬP ÁP DỤNG (ĐIỀN TỪ VÀO CÂU) ──────────────────────────────

const FILL = [
  { sentence:`They bought new clothes for the ___.`, answer:"occasion",
    vi:`Họ mua quần áo mới cho dịp đặc biệt này.` },
  { sentence:`The whole family had a ___ dinner on New Year's Eve.`, answer:"reunion",
    vi:`Cả gia đình có một bữa cơm sum họp vào đêm giao thừa.` },
  { sentence:`Banh chung is a ___ Vietnamese cake.`, answer:"traditional",
    vi:`Bánh chưng là một loại bánh truyền thống của Việt Nam.` },
  { sentence:`Tet is a time to show deep respect to every ___ in the family.`, answer:"elder",
    vi:`Tết là thời điểm để thể hiện sự tôn trọng sâu sắc với mỗi người lớn tuổi trong gia đình.` },
  { sentence:`Lucky money is ___ped in red envelopes.`, answer:"wrap",
    vi:`Tiền lì xì được gói trong những phong bì đỏ.` },
  { sentence:`Spending too much time online can lead to Internet ___.`, answer:"addiction",
    vi:`Dành quá nhiều thời gian trên mạng có thể dẫn đến nghiện Internet.` },
  { sentence:`Internet ___s often forget about their real lives.`, answer:"addict",
    vi:`Người nghiện Internet thường quên đi cuộc sống thực của mình.` },
  { sentence:`They don't talk to friends; ___, they chat online all day.`, answer:"instead",
    vi:`Họ không nói chuyện với bạn bè; thay vào đó, họ trò chuyện trực tuyến cả ngày.` },
  { sentence:`Some people use the Internet to escape from ___.`, answer:"stress",
    vi:`Một số người dùng Internet để thoát khỏi căng thẳng.` },
  { sentence:`She wanted to ___ from her problems for a while.`, answer:"escape",
    vi:`Cô ấy muốn thoát khỏi các vấn đề của mình trong một thời gian.` }
];

let fillState;

function buildFill() {
  return { order: shuffle(FILL.map((_, i) => i)), selected: {}, submitted: false };
}

function renderFill() {
  const container = document.getElementById("apply-container");
  const s = fillState;
  const submitted = s.submitted;

  const wordBank = `
    <div class="word-bank">
      ${shuffle(VOCAB).map(v => `<span class="word-chip">${v.word}</span>`).join("")}
    </div>
  `;

  let sentences = s.order.map((idx, pos) => {
    const f = FILL[idx];
    const sel = s.selected[idx];
    const isCorrect = submitted && sel === f.answer;
    let cls = "inline-select";
    if (submitted) cls += isCorrect ? " correct" : " wrong";

    let opts = `<option value="" disabled ${sel === undefined ? "selected" : ""}>...</option>`;
    VOCAB.forEach(v => {
      opts += `<option value="${v.word}" ${sel === v.word ? "selected" : ""}>${v.word}</option>`;
    });

    const select = `<select class="${cls}" ${submitted ? "disabled" : ""} onchange="selectFill(${idx}, this.value)">${opts}</select>`;
    const sentenceHtml = f.sentence.replace("___", select);
    const icon = !submitted ? "" : (isCorrect ? " ✅" : " ❌");

    return `
      <div class="card-box">
        <div class="fill-sentence"><strong>${pos + 1}.</strong> ${sentenceHtml}${icon}</div>
        <div class="fill-vi">📝 Nghĩa: ${f.vi}</div>
        ${submitted && !isCorrect ? `<div class="explanation wrong">Đáp án đúng: <strong>${f.answer}</strong></div>` : ""}
      </div>
    `;
  }).join("");

  let html = wordBank + sentences;

  if (submitted) {
    const correct = FILL.filter((f, i) => s.selected[i] === f.answer).length;
    const total = FILL.length;
    const pct = Math.round((correct / total) * 100);
    const e = pct >= 80 ? "🎉" : pct >= 50 ? "👍" : "💪";
    html += `
      <div class="score-box">
        <div class="score-emoji">${e}</div>
        <div class="score-pct">${pct}%</div>
        <div class="score-detail">
          <span>✅ Đúng: <strong style="color:#27ae60">${correct}</strong></span>
          <span>❌ Sai: <strong style="color:#e74c3c">${total - correct}</strong></span>
        </div>
      </div>
    `;
  }

  html += `
    <div class="btn-row">
      ${!submitted
        ? `<button class="btn-primary" onclick="submitFill()">📘 Nộp bài Phần 3</button>`
        : `<button class="btn-reset" onclick="resetFill()">🔄 Làm lại Phần 3</button>`
      }
    </div>
  `;

  container.innerHTML = html;
}

function selectFill(idx, value) {
  if (fillState.submitted) return;
  fillState.selected[idx] = value;
  renderFill();
}

function submitFill() {
  if (Object.keys(fillState.selected).length < FILL.length) {
    alert("Em hãy điền đủ từ vào tất cả các câu trước khi nộp bài nhé!");
    return;
  }
  fillState.submitted = true;
  renderFill();
}

function resetFill() {
  fillState = buildFill();
  renderFill();
}

// ─── TABS ────────────────────────────────────────────────────────────────

document.querySelectorAll(".tab-btn").forEach(btn => {
  btn.addEventListener("click", () => {
    const key = btn.dataset.tab;
    document.querySelectorAll(".tab-btn").forEach(b => b.classList.remove("active"));
    document.querySelectorAll(".tab-panel").forEach(p => p.classList.remove("active"));
    btn.classList.add("active");
    document.getElementById("tab-" + key).classList.add("active");
  });
});

// ─── INIT ──────────────────────────────────────────────────────────────────

renderDecor();
renderVocabRead();
twowayState = buildTwoway();
renderTwoway();
fillState = buildFill();
renderFill();
</script>
</body>
</html>
