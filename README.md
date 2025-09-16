# Grisssowm.github.io
<!doctype html>
<html lang="pt-BR">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Quiz — Revoluções Industriais e Meio Técnico-Científico-Informacional</title>
<style>
  :root{
    --bg: #f6f8fb;
    --card: #ffffff;
    --accent: #2563eb;
    --success: #16a34a;
    --danger: #dc2626;
    --muted: #6b7280;
    --btn-padding: 12px 18px;
    font-family: Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
  }
  *{box-sizing:border-box}
  body{
    margin:0;
    min-height:100vh;
    display:flex;
    align-items:center;
    justify-content:center;
    background:var(--bg);
    padding:24px;
    color:#0b1220;
  }
  .wrap{
    width:100%;
    max-width:980px;
  }
  .card{
    background:var(--card);
    border-radius:14px;
    padding:26px;
    box-shadow: 0 10px 30px rgba(16,24,40,0.08);
  }
  header{
    display:flex;
    gap:14px;
    align-items:center;
    margin-bottom:10px;
  }
  header h1{ margin:0; font-size:20px; }
  header p{ margin:0; color:var(--muted); font-size:14px; }
  .progress-wrap{ margin-top:12px; display:flex; align-items:center; gap:12px; }
  .progress{
    flex:1;
    height:12px;
    background:#e6eefc;
    border-radius:999px;
    overflow:hidden;
  }
  .progress > i{
    display:block;
    height:100%;
    width:0%;
    background:linear-gradient(90deg,var(--accent),#60a5fa);
    transition:width .35s ease;
  }
  .progress-label{ min-width:68px; text-align:right; font-size:13px; color:var(--muted); }
  .question-area{ margin-top:18px; }
  .q-number{ color:var(--muted); font-size:13px; margin-bottom:8px; }
  .q-text{ font-size:18px; margin-bottom:18px; line-height:1.45; }
  .choices{
    display:flex;
    flex-direction:column;
    gap:12px;
    align-items:center;
  }
  .choice-btn{
    width:100%;
    max-width:820px;
    padding:var(--btn-padding);
    border-radius:12px;
    border:1px solid rgba(2,6,23,0.06);
    background:#fff;
    cursor:pointer;
    font-size:15px;
    text-align:center;
    transition: transform .06s ease, box-shadow .06s ease;
    outline:none;
  }
  .choice-btn:hover{ transform:translateY(-3px); box-shadow:0 8px 20px rgba(2,6,23,0.06); }
  .choice-btn[disabled]{ cursor:default; opacity:0.96; transform:none; box-shadow:none; }
  .choice-correct{
    background:var(--success) !important;
    color:#fff !important;
    border-color: rgba(16,185,129,0.9) !important;
    box-shadow:0 8px 20px rgba(16,185,129,0.12);
  }
  .choice-wrong{
    background:var(--danger) !important;
    color:#fff !important;
    border-color: rgba(220,38,38,0.9) !important;
    box-shadow:0 8px 20px rgba(220,38,38,0.12);
  }
  .btn-row{ display:flex; justify-content:center; margin-top:18px; gap:12px; }
  .btn-next{
    padding:10px 18px;
    border-radius:10px;
    background:var(--accent);
    color:white;
    border:0;
    cursor:pointer;
    font-weight:600;
    display:inline-block;
  }
  .btn-next.hidden{ display:none; }
  .meta{ margin-top:12px; color:var(--muted); font-size:14px; text-align:center; }
  .summary{
    margin-top:18px;
  }
  .final-card{
    background:#fff;
    border-radius:12px;
    padding:18px;
    border:1px solid rgba(2,6,23,0.04);
  }
  .summary-list{ list-style:none; margin:12px 0 0 0; padding:0; display:grid; gap:12px; }
  .summary-item{ padding:12px; border-radius:10px; border:1px solid rgba(2,6,23,0.04); background:#fcfeff; }
  .summary-item strong{ display:block; margin-bottom:6px; }
  .explain{ color:var(--muted); font-size:14px; margin-top:8px; }
  .small{ font-size:13px; color:var(--muted); }
  footer{ margin-top:18px; text-align:center; color:var(--muted); font-size:13px; }
  @media (max-width:720px){
    .q-text{ font-size:16px; }
    .choice-btn{ font-size:15px; }
    header{ flex-direction:column; align-items:flex-start; gap:6px; }
  }
</style>
</head>
<body>
  <div class="wrap">
    <div class="card" id="card">
      <header>
        <div>
          <h1>Quiz — Revoluções Industriais & Meio Técnico-Científico-Informacional</h1>
          <p>20 questões (múltipla escolha + verdadeiro/falso). Responda e veja feedback imediato. Alternativas embaralhadas.</p>
        </div>
      </header>

      <div class="progress-wrap" aria-hidden="false">
        <div class="progress" aria-valuemin="0" aria-valuemax="100" aria-valuenow="0">
          <i id="progress-bar"></i>
        </div>
        <div class="progress-label" id="progress-label">0%</div>
      </div>

      <div class="question-area" id="question-area"></div>
      <div class="meta" id="meta">Questão <span id="current">1</span> de <span id="total">20</span></div>

      <div class="summary" id="summary-container" style="display:none;">
        <div class="final-card">
          <h2>Resumo final</h2>
          <p id="score" style="font-weight:700; margin:6px 0;"></p>
          <ul class="summary-list" id="summary-list"></ul>
          <div style="margin-top:12px; text-align:center;">
            <button id="restart" class="btn-next" style="background:#374151">Refazer quiz</button>
          </div>
        </div>
      </div>

      <footer>
        Fonte: texto fornecido sobre Segunda Revolução Industrial, imperialismo, Terceira Revolução Industrial, tecnociência, eletrônica e meio técnico-científico-informacional.
      </footer>
    </div>
  </div>

<script>
/*
Quiz gerado a partir do texto fornecido.
- 20 perguntas (mix MCQ e TF)
- Alternativas embaralhadas a cada render
- Próxima aparece só após responder
- Barra de progresso (porcentagem)
- Resumo final com ✔ / ✖ e explicação
*/

const questions = [
  {
    text: "Qual invenção e uso impulsionaram a Segunda Revolução Industrial, conforme o texto?",
    type: "mcq",
    choices: ["Máquina a vapor e carvão", "Eletricidade e motor a combustão interna", "Energia nuclear", "Internet"],
    correct: "Eletricidade e motor a combustão interna",
    explanation: "O texto destaca a utilização da eletricidade e do motor a combustão interna como marca da Segunda Revolução Industrial."
  },
  {
    text: "Durante a Segunda Revolução Industrial, qual material tornou-se central para a indústria?",
    type: "mcq",
    choices: ["Plástico", "Aço", "Madeira", "Cobre"],
    correct: "Aço",
    explanation: "O desenvolvimento de novos processos para a produção de aço o tornou um dos materiais mais importantes na indústria."
  },
  {
    text: "O surgimento do taylorismo visava principalmente:",
    type: "mcq",
    choices: ["Aumentar a produtividade organizando o trabalho em tarefas simples", "Diminuir o controle sobre os operários", "Eliminar máquinas das fábricas", "Promover férias mais longas"],
    correct: "Aumentar a produtividade organizando o trabalho em tarefas simples",
    explanation: "O método de Taylor dividia o processo em etapas simples para aumentar eficiência e produtividade."
  },
  {
    text: "O taylorismo procurava reduzir a força de trabalho através da eficiência.",
    type: "tf",
    choices: ["Verdadeiro","Falso"],
    correct: "Verdadeiro",
    explanation: "Taylor buscava produzir mais com menos tempo, energia e matéria-prima, reduzindo a necessidade de mão de obra."
  },
  {
    text: "Uma consequência econômica da Segunda Revolução Industrial, apontada no texto, foi:",
    type: "mcq",
    choices: ["Menor necessidade de matérias-primas", "A ampliação do mercado consumidor para absorver maior produção", "Fim do comércio internacional", "Redução da produção industrial"],
    correct: "A ampliação do mercado consumidor para absorver maior produção",
    explanation: "Com o aumento da produção havia a necessidade de expandir mercados consumidores para comprar os produtos."
  },
  {
    text: "Qual país foi o primeiro a reunir condições para iniciar a industrialização, segundo o texto?",
    type: "mcq",
    choices: ["Estados Unidos", "França", "Reino Unido", "Alemanha"],
    correct: "Reino Unido",
    explanation: "No final do século XVIII, o Reino Unido era o único país que reunia condições para iniciar a industrialização."
  },
  {
    text: "O imperialismo (neocolonialismo) é descrito como:",
    type: "mcq",
    choices: ["Uma expansão para garantir matérias-primas e mercados", "Uma forma de comércio igualitário entre países", "Uma política de isolamento cultural", "A distribuição de tecnologia gratuitamente"],
    correct: "Uma expansão para garantir matérias-primas e mercados",
    explanation: "As potências industriais expandiram-se à África e Ásia para garantir matérias-primas e mercados, dando origem ao imperialismo."
  },
  {
    text: "As potências imperialistas citadas incluem Inglaterra, França, Alemanha e Japão.",
    type: "tf",
    choices: ["Verdadeiro","Falso"],
    correct: "Verdadeiro",
    explanation: "O texto cita Inglaterra, França, Alemanha, Bélgica, Japão e Estados Unidos como principais potências imperialistas."
  },
  {
    text: "O texto relaciona o imperialismo com práticas violentas e genocídios em algumas regiões.",
    type: "tf",
    choices: ["Verdadeiro","Falso"],
    correct: "Verdadeiro",
    explanation: "Relatos no texto mencionam violência na África Subsaariana e registros de genocídios na história contemporânea."
  },
  {
    text: "A Terceira Revolução Industrial é caracterizada, segundo o texto, por:",
    type: "mcq",
    choices: ["A invenção da máquina a vapor", "A junção entre técnica e ciência e o avanço da eletrônica e indústria química", "O fim da produção industrial", "A adoção do motor a combustão interna"],
    correct: "A junção entre técnica e ciência e o avanço da eletrônica e indústria química",
    explanation: "O texto define a Terceira Revolução como o período em que ciência e técnica se unem, com avanços em eletrônica e indústria química."
  },
  {
    text: "A energia nuclear foi usada no final da Segunda Guerra Mundial e depois aplicada para geração elétrica.",
    type: "tf",
    choices: ["Verdadeiro","Falso"],
    correct: "Verdadeiro",
    explanation: "O texto menciona as bombas atômicas lançadas em agosto de 1945 e o posterior uso da energia nuclear para eletricidade."
  },
  {
    text: "O termo 'tecnociência' refere-se a:",
    type: "mcq",
    choices: ["Estudos científicos sem financiamento", "A pesquisa científica financiada e orientada para produzir tecnologias", "A técnica manual sem ciência", "Um tipo de indústria agrícola"],
    correct: "A pesquisa científica financiada e orientada para produzir tecnologias",
    explanation: "Tecnociência descreve estudos científicos com financiamento de empresas/Estado e infraestrutura, com intenção de criar tecnologias."
  },
  {
    text: "As tecnologias, diferentemente dos saberes tradicionais, costumam ser propriedade registrada por patentes.",
    type: "tf",
    choices: ["Verdadeiro","Falso"],
    correct: "Verdadeiro",
    explanation: "O texto explica que tecnologias são tratadas como propriedades e registradas por meio de patentes."
  },
  {
    text: "O crescimento de registros de patentes no século XIX ocorreu por causa de:",
    type: "mcq",
    choices: ["Menor investimento em pesquisa", "Aumento de investimentos científicos e ampliação de universidades", "Redução das invenções", "Proibição de indústria química"],
    correct: "Aumento de investimentos científicos e ampliação de universidades",
    explanation: "Mais investimentos em ciência e mais centros de pesquisa levaram ao aumento de registros de patentes."
  },
  {
    text: "Segundo o texto, a eletrônica permitiu evoluções como rádio, televisão e computadores.",
    type: "tf",
    choices: ["Verdadeiro","Falso"],
    correct: "Verdadeiro",
    explanation: "A eletrônica expandiu o uso da eletricidade e possibilitou invenções como rádio, TV e computadores."
  },
  {
    text: "O primeiro computador eletrônico surgiu em 1946, conforme o texto.",
    type: "tf",
    choices: ["Verdadeiro","Falso"],
    correct: "Verdadeiro",
    explanation: "O texto indica que, nesse contexto, em 1946 surgiu o primeiro computador eletrônico."
  },
  {
    text: "A biotecnologia, mencionada no texto, está associada a avanços em:",
    type: "mcq",
    choices: ["Televisão e rádio", "Engenharia genética aplicada à medicina e agropecuária", "Motor a combustão", "Máquina a vapor"],
    correct: "Engenharia genética aplicada à medicina e agropecuária",
    explanation: "O texto relaciona a biotecnologia a aplicações da engenharia genética, como OGMs e clonagem."
  },
  {
    text: "A informação torna-se variável-chave do período porque empresas e governos precisam organizar e processar grandes volumes de dados.",
    type: "tf",
    choices: ["Verdadeiro","Falso"],
    correct: "Verdadeiro",
    explanation: "O texto destaca a necessidade de organizar estoques, contabilidade e gestão de grandes volumes de informação."
  },
  {
    text: "IBM foi citada como exemplo de empresa que surgiu para organizar processamento de dados.",
    type: "mcq",
    choices: ["Apple", "IBM", "Microsoft", "Oracle"],
    correct: "IBM",
    explanation: "O texto cita a IBM, criada em 1924 pela junção de empresas que produziam relógios de ponto, balanças e máquinas de contabilizar."
  },
  {
    text: "As tecnologias do meio técnico-científico-informacional funcionam de forma interdependente — se uma falhar, o sistema pode paralisar.",
    type: "tf",
    choices: ["Verdadeiro","Falso"],
    correct: "Verdadeiro",
    explanation: "O texto exemplifica que tecnologias dependem umas das outras (ex.: celular depende de antenas e eletricidade)."
  }
];

// --- estado ---
const total = questions.length;
document.getElementById('total').innerText = total;
let currentIndex = 0;
let score = 0;
const userAnswers = []; // {questionIndex, selected, correct, isCorrect}

// util shuffle
function shuffleArray(arr){
  for(let i = arr.length -1; i>0; i--){
    const j = Math.floor(Math.random() * (i+1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
}

// render
function renderQuestion(){
  const q = questions[currentIndex];
  const area = document.getElementById('question-area');
  area.innerHTML = '';

  document.getElementById('current').innerText = currentIndex + 1;
  updateProgress();

  const qnum = document.createElement('div');
  qnum.className = 'q-number';
  qnum.innerText = (q.type === 'tf' ? 'Verdadeiro ou Falso' : 'Múltipla escolha');
  area.appendChild(qnum);

  const qtext = document.createElement('div');
  qtext.className = 'q-text';
  qtext.innerText = q.text;
  area.appendChild(qtext);

  // prepare choices copy and shuffle (ensure correct included)
  const choices = q.choices ? q.choices.slice() : (q.type==='tf' ? ['Verdadeiro','Falso'] : []);
  if(!choices.includes(q.correct)) choices.push(q.correct);
  shuffleArray(choices);

  const choicesDiv = document.createElement('div');
  choicesDiv.className = 'choices';

  choices.forEach(choiceText => {
    const btn = document.createElement('button');
    btn.className = 'choice-btn';
    btn.type = 'button';
    btn.innerText = choiceText;
    btn.dataset.choice = choiceText;
    btn.addEventListener('click', onChoiceClick);
    choicesDiv.appendChild(btn);
  });

  area.appendChild(choicesDiv);

  // next button (hidden by default)
  const btnRow = document.createElement('div');
  btnRow.className = 'btn-row';
  const nextBtn = document.createElement('button');
  nextBtn.id = 'nextBtn';
  nextBtn.className = 'btn-next hidden';
  nextBtn.innerText = (currentIndex === total -1 ? 'Finalizar' : 'Próxima');
  nextBtn.addEventListener('click', onNext);
  btnRow.appendChild(nextBtn);
  area.appendChild(btnRow);
}

// choice click
function onChoiceClick(e){
  const btn = e.currentTarget;
  const chosen = btn.dataset.choice;
  const q = questions[currentIndex];

  // disable buttons
  const allBtns = Array.from(document.querySelectorAll('.choice-btn'));
  allBtns.forEach(b => b.disabled = true);

  // mark correct (green) and the clicked wrong (red) if applicable
  allBtns.forEach(b => {
    if(b.dataset.choice === q.correct){
      b.classList.add('choice-correct');
    }
  });

  if(chosen === q.correct){
    // correct
    score++;
    userAnswers.push({questionIndex: currentIndex, selected: chosen, correct: q.correct, isCorrect: true});
  } else {
    // wrong
    btn.classList.add('choice-wrong');
    userAnswers.push({questionIndex: currentIndex, selected: chosen, correct: q.correct, isCorrect: false});
  }

  // reveal next button
  const nextBtn = document.getElementById('nextBtn');
  nextBtn.classList.remove('hidden');
}

// next or finish
function onNext(){
  currentIndex++;
  if(currentIndex >= total){
    showSummary();
  } else {
    renderQuestion();
  }
}

// progress update
function updateProgress(){
  const percent = Math.round((currentIndex / total) * 100);
  const bar = document.getElementById('progress-bar');
  const label = document.getElementById('progress-label');
  bar.style.width = percent + '%';
  bar.setAttribute('aria-valuenow', percent);
  label.innerText = percent + '%';
}

// summary
function showSummary(){
  document.getElementById('question-area').style.display = 'none';
  document.getElementById('meta').style.display = 'none';
  const cont = document.getElementById('summary-container');
  cont.style.display = 'block';
  document.getElementById('score').innerText = `Você acertou ${score} de ${total} perguntas.`;

  const list = document.getElementById('summary-list');
  list.innerHTML = '';

  for(let i=0;i<questions.length;i++){
    const q = questions[i];
    const rec = userAnswers.find(r => r.questionIndex === i);
    const isCorrect = rec ? rec.isCorrect : false;

    const li = document.createElement('li');
    li.className = 'summary-item';

    const header = document.createElement('strong');
    header.innerText = `Q${i+1}: ${q.text}`;
    li.appendChild(header);

    const markLine = document.createElement('div');
    markLine.className = 'small';
    markLine.innerHTML = (isCorrect ? `<span style="color:${getComputedStyle(document.documentElement).getPropertyValue('--success') || '#16a34a'}">✔ Acertou</span>` : `<span style="color:${getComputedStyle(document.documentElement).getPropertyValue('--danger') || '#dc2626'}">✖ Errou</span>`);
    li.appendChild(markLine);

    const ansLine = document.createElement('div');
    ansLine.className = 'small';
    ansLine.innerHTML = `<strong>Sua resposta:</strong> ${rec ? rec.selected : '<em>Sem resposta</em>'} &nbsp; — &nbsp; <strong>Resposta certa:</strong> ${q.correct}`;
    li.appendChild(ansLine);

    const expl = document.createElement('div');
    expl.className = 'explain';
    expl.innerText = q.explanation;
    li.appendChild(expl);

    list.appendChild(li);
  }

  // ensure progress shows 100%
  document.getElementById('progress-bar').style.width = '100%';
  document.getElementById('progress-label').innerText = '100%';

  // restart
  document.getElementById('restart').addEventListener('click', () => {
    currentIndex = 0;
    score = 0;
    userAnswers.length = 0;
    document.getElementById('summary-container').style.display = 'none';
    document.getElementById('question-area').style.display = '';
    document.getElementById('meta').style.display = '';
    renderQuestion();
  });
}

// initial render
renderQuestion();
updateProgress();

</script>
</body>
</html>
