# KiokdeFezes.github.io
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width,initial-scale=1.0" />
<title>Quiz — Trabalho e Exploração na América Espanhola</title>
<style>
  :root{
    --bg:#f5f7fb;
    --card:#ffffff;
    --accent:#4a90e2;
    --correct:#4caf50;
    --wrong:#e74c3c;
    --text:#333333;
  }
  body{
    font-family: Arial, sans-serif;
    background:var(--bg);
    display:flex;
    justify-content:center;
    align-items:center;
    min-height:100vh;
    margin:0;
    padding:20px;
  }
  .quiz-card{
    background:var(--card);
    padding:20px;
    border-radius:12px;
    box-shadow:0 4px 10px rgba(0,0,0,0.1);
    width:100%;
    max-width:700px;
  }
  h1{
    text-align:center;
    color:var(--text);
  }
  .question{
    font-size:18px;
    margin-bottom:15px;
  }
  .options button{
    display:block;
    width:100%;
    margin:8px 0;
    padding:10px;
    border:none;
    border-radius:8px;
    background:#f0f0f0;
    cursor:pointer;
    font-size:16px;
    transition:0.3s;
  }
  .options button:hover{background:#e0e0e0;}
  .options button.correct{background:var(--correct);color:#fff;}
  .options button.wrong{background:var(--wrong);color:#fff;}
  .explanation{
    margin-top:15px;
    font-size:15px;
    color:#555;
    padding:10px;
    background:#f9f9f9;
    border-left:4px solid var(--accent);
  }
  .next-btn{
    margin-top:15px;
    padding:10px 20px;
    border:none;
    border-radius:8px;
    background:var(--accent);
    color:#fff;
    font-size:16px;
    cursor:pointer;
  }
  .progress-container{
    width:100%;
    background:#ddd;
    border-radius:8px;
    margin:20px 0;
    height:20px;
    overflow:hidden;
  }
  .progress-bar{
    height:100%;
    width:0;
    background:var(--accent);
    transition:width 0.3s ease;
  }
</style>
</head>
<body>
  <div class="quiz-card">
    <h1>Quiz — Trabalho e Exploração</h1>
    <div class="progress-container"><div class="progress-bar"></div></div>
    <div class="question" id="question"></div>
    <div class="options" id="options"></div>
    <div class="explanation" id="explanation" style="display:none;"></div>
    <button class="next-btn" id="nextBtn" style="display:none;">Próxima</button>
  </div>
<script>
const QUESTIONS = [
  {
    type: "mcq",
    question: "Na hierarquia social da América espanhola, os chapetones ocupavam o topo. Quem eles eram?",
    options: [
      "Filhos de espanhóis nascidos na colônia, com prestígio limitado.",
      "Espanhóis vindos da metrópole, controlando cargos e comércio.",
      "Mestiços com acesso ao clero e à administração.",
      "Africanos libertos que ascenderam na sociedade."
    ],
    correctIndex: 1,
    explanation: "Chapetones eram peninsulares, nascidos na Espanha, enviados à América para ocupar os cargos mais altos."
  },
  {
    type: "mcq",
    question: "Os criollos se diferenciavam dos chapetones principalmente por:",
    options: [
      "Defenderem o monopólio da metrópole.",
      "Serem descendentes de espanhóis nascidos na colônia, excluídos de altos cargos.",
      "Estarem sempre em condições iguais às elites da metrópole.",
      "Serem indígenas aliados da Coroa espanhola."
    ],
    correctIndex: 1,
    explanation: "Criollos eram descendentes de espanhóis nascidos na colônia; ricos, mas com menos prestígio que os chapetones."
  },
  {
    type: "tf",
    question: "Indígenas e africanos compunham as camadas mais exploradas da sociedade colonial.",
    correct: true,
    explanation: "Esses grupos eram submetidos a trabalho compulsório, tanto em minas como em haciendas."
  }
  // ... (restante das perguntas que você já tinha)
];

let currentQuestion = 0;
const questionEl = document.getElementById("question");
const optionsEl = document.getElementById("options");
const explanationEl = document.getElementById("explanation");
const nextBtn = document.getElementById("nextBtn");
const progressBar = document.querySelector(".progress-bar");

// Função para embaralhar array
function shuffleArray(array){
  for(let i = array.length -1; i > 0; i--){
    const j = Math.floor(Math.random() * (i+1));
    [array[i], array[j]] = [array[j], array[i]];
  }
  return array;
}

function loadQuestion(){
  const q = QUESTIONS[currentQuestion];
  questionEl.textContent = q.question;
  optionsEl.innerHTML = "";
  explanationEl.style.display = "none";
  nextBtn.style.display = "none";

  if(q.type === "mcq"){
    let opts = q.options.map((opt,i)=>({text: opt, correct: i===q.correctIndex}));
    opts = shuffleArray(opts);
    opts.forEach(opt=>{
      const btn = document.createElement("button");
      btn.textContent = opt.text;
      btn.onclick = ()=> selectOption(btn,opt.correct,opts);
      optionsEl.appendChild(btn);
    });
  } else if(q.type === "tf"){
    ["Verdadeiro","Falso"].forEach((opt,i)=>{
      const btn = document.createElement("button");
      btn.textContent = opt;
      btn.onclick = ()=> selectOption(btn,(i===0)===q.correct,[{btn:btn, correct:(i===0)===q.correct}]);
      optionsEl.appendChild(btn);
    });
  }
  updateProgress();
}

function selectOption(button,isCorrect,opts){
  const buttons = optionsEl.querySelectorAll("button");
  buttons.forEach(b=>b.disabled=true);

  if(isCorrect){
    button.classList.add("correct");
  } else {
    button.classList.add("wrong");
    // Marca a correta
    const correctBtn = Array.from(buttons).find((b,i)=>opts[i].correct);
    if(correctBtn) correctBtn.classList.add("correct");
  }

  explanationEl.textContent = QUESTIONS[currentQuestion].explanation;
  explanationEl.style.display = "block";
  nextBtn.style.display = "inline-block";
}

nextBtn.addEventListener("click",()=>{
  currentQuestion++;
  if(currentQuestion < QUESTIONS.length){
    loadQuestion();
  } else {
    questionEl.textContent = "Fim do quiz!";
    optionsEl.innerHTML = "";
    explanationEl.style.display = "none";
    nextBtn.style.display = "none";
    progressBar.style.width = "100%";
  }
});

function updateProgress(){
  const progress = ((currentQuestion)/QUESTIONS.length)*100;
  progressBar.style.width = progress+"%";
}

loadQuestion();
</script>
</body>
</html>
