# KiokdeFezes.github.io
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Quiz Interativo - Geografia Econômica</title>
<style>
    body {
        font-family: Arial, sans-serif;
        background-color: #f2f2f2;
        display: flex;
        justify-content: center;
        padding: 50px 0;
    }
    .quiz-container {
        background-color: #fff;
        padding: 30px;
        border-radius: 12px;
        box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        width: 600px;
        max-width: 90%;
    }
    h2 {
        text-align: center;
    }
    .progress-container {
        background-color: #e0e0e0;
        border-radius: 8px;
        overflow: hidden;
        margin-bottom: 20px;
    }
    .progress-bar {
        height: 20px;
        width: 0%;
        background-color: #3498db;
        text-align: center;
        color: white;
        line-height: 20px;
        transition: width 0.3s;
    }
    .question {
        margin-bottom: 20px;
    }
    .answers {
        display: flex;
        flex-direction: column;
    }
    button.answer-btn {
        background-color: #3498db;
        color: white;
        border: none;
        padding: 10px;
        margin: 5px 0;
        border-radius: 6px;
        cursor: pointer;
        transition: background-color 0.3s;
    }
    button.answer-btn.correct {
        background-color: #2ecc71 !important;
    }
    button.answer-btn.wrong {
        background-color: #e74c3c !important;
    }
    button#next-btn {
        margin-top: 20px;
        padding: 10px 20px;
        border: none;
        border-radius: 6px;
        background-color: #f39c12;
        color: white;
        cursor: pointer;
        display: none;
    }
    .explanation {
        margin-top: 10px;
        font-style: italic;
    }
    .summary {
        margin-top: 20px;
    }
    .summary-item {
        margin: 5px 0;
    }
</style>
</head>
<body>
<div class="quiz-container">
    <h2>Quiz Interativo - Geografia Econômica</h2>

    <div class="progress-container">
        <div id="progress-bar" class="progress-bar">0%</div>
    </div>

    <div id="quiz"></div>
    <button id="next-btn" onclick="nextQuestion()">Próxima</button>
    <div id="summary" class="summary"></div>
</div>

<script>
const quizData = [
    {question:"O que é economia em uma sociedade?", type:"mc", answers:["Organização para produção e distribuição de bens e serviços","Estudo das plantas e animais","Conjunto de regras de trânsito","Apenas a fabricação de máquinas"], correct:0, explanation:"Economia se refere à forma como uma sociedade organiza produção e distribuição de bens e serviços."},
    {question:"Bens e serviços podem ser classificados como essenciais e não essenciais.", type:"tf", answers:["Verdadeiro","Falso"], correct:0, explanation:"Bens e serviços podem ser essenciais (alimento, saúde) ou não essenciais (lazer), dependendo da necessidade da sociedade."},
    {question:"O setor primário da economia envolve:", type:"mc", answers:["Indústrias de transformação","Comércio e serviços","Produção de matérias-primas","Educação e saúde"], correct:2, explanation:"O setor primário se ocupa da extração e produção de matérias-primas como agricultura, pesca e mineração."},
    {question:"O setor secundário é responsável por transformar matérias-primas em produtos.", type:"tf", answers:["Verdadeiro","Falso"], correct:0, explanation:"O setor secundário compreende a indústria e transformação de matérias-primas em bens de consumo ou equipamentos."},
    {question:"Os fatores de produção incluem terra, trabalho e capital.", type:"mc", answers:["Verdadeiro","Falso","Apenas terra e capital","Apenas trabalho e terra"], correct:0, explanation:"Os fatores de produção são terra (recursos naturais), trabalho (esforço humano) e capital (máquinas, equipamentos)."},
    {question:"Escassez significa que há recursos suficientes para todas as necessidades.", type:"tf", answers:["Verdadeiro","Falso"], correct:1, explanation:"Escassez indica que os recursos são limitados e não atendem todas as necessidades da sociedade."},
    {question:"O capital inclui máquinas, estradas e usinas.", type:"mc", answers:["Verdadeiro","Falso","Apenas máquinas","Apenas recursos naturais"], correct:0, explanation:"Capital engloba bens produzidos pelo trabalho para gerar novos bens e serviços."},
    {question:"O espaço geográfico é formado pela interação de terra, trabalho e capital.", type:"tf", answers:["Verdadeiro","Falso"], correct:0, explanation:"O espaço geográfico resulta da combinação desses fatores e das transformações humanas sobre eles."},
    {question:"O meio técnico refere-se a:", type:"mc", answers:["Conjunto de objetos técnicos que formam infraestrutura","A quantidade de capital financeiro","O trabalho humano não especializado","A natureza sem intervenção humana"], correct:0, explanation:"Meio técnico é o conjunto de objetos e técnicas que facilitam a produção e organização do espaço."},
    {question:"O homem se diferencia por produzir e aprimorar ferramentas.", type:"tf", answers:["Verdadeiro","Falso"], correct:0, explanation:"A capacidade de criar, modificar e aprimorar ferramentas é uma característica distintiva da humanidade."},
    {question:"O capitalismo busca lucro e envolve propriedade privada.", type:"mc", answers:["Verdadeiro","Falso","Busca apenas produção","Só existe no setor primário"], correct:0, explanation:"O capitalismo é caracterizado por produção visando lucro e propriedade privada dos meios de produção."},
    {question:"O capitalismo industrial surgiu entre 1750 e 1950.", type:"tf", answers:["Verdadeiro","Falso"], correct:0, explanation:"O capitalismo industrial ocorreu com a Revolução Industrial, mecanização e uso de mão de obra assalariada."},
    {question:"O capitalismo financeiro integra capital industrial e financeiro.", type:"mc", answers:["Verdadeiro","Falso","Apenas comércio","Apenas agricultura"], correct:0, explanation:"Na fase financeira, empresas usam capital industrial e financeiro (ações, títulos) de forma integrada."},
    {question:"A Revolução Industrial provocou êxodo rural.", type:"tf", answers:["Verdadeiro","Falso"], correct:0, explanation:"O crescimento das indústrias nas cidades atraiu pessoas do campo, caracterizando o êxodo rural."},
    {question:"Indústrias de bens de capital produzem máquinas e equipamentos para outras indústrias.", type:"mc", answers:["Verdadeiro","Falso","Produzem alimentos","Fornecem serviços"], correct:0, explanation:"Bens de capital são usados para produzir outros bens e serviços dentro do sistema industrial."},
    {question:"Indústrias de bens de consumo duráveis são usadas por anos.", type:"tf", answers:["Verdadeiro","Falso"], correct:0, explanation:"Bens duráveis, como carros e eletrodomésticos, têm vida útil prolongada."},
    {question:"Indústrias de bens de consumo não duráveis incluem alimentos e roupas.", type:"mc", answers:["Verdadeiro","Falso","Apenas máquinas","Apenas veículos"], correct:0, explanation:"Bens não duráveis são consumidos rapidamente, como alimentos e roupas."},
    {question:"A técnica permite modificar o meio e aumentar produtividade.", type:"tf", answers:["Verdadeiro","Falso"], correct:0, explanation:"A aplicação de conhecimento técnico permite transformar o meio natural e produzir mais."},
    {question:"O espaço geográfico combina sistemas de objetos e de ação.", type:"mc", answers:["Verdadeiro","Falso","Apenas objetos","Apenas ações"], correct:0, explanation:"Espaço geográfico é a combinação de objetos técnicos e das ações humanas sobre eles."},
    {question:"A industrialização altera comportamento e organização do espaço.", type:"tf", answers:["Verdadeiro","Falso"], correct:0, explanation:"A urbanização, divisão do trabalho e consumo mudam a forma de vida e ocupação do território."}
];

let currentQuestion = 0;
let score = 0;
let userAnswers = [];

function loadQuestion() {
    const quiz = document.getElementById('quiz');
    quiz.innerHTML = '';
    if(currentQuestion < quizData.length){
        const q = quizData[currentQuestion];

        // Embaralhar respostas mantendo referência ao índice original
        const shuffledAnswers = q.answers.map((text, index) => ({text, index}))
                                         .sort(() => Math.random() - 0.5);
        q.shuffledAnswers = shuffledAnswers;
        q.shuffledCorrect = shuffledAnswers.findIndex(a => a.index === q.correct);

        const questionEl = document.createElement('div');
        questionEl.classList.add('question');
        questionEl.innerHTML = `<strong>Pergunta ${currentQuestion + 1}:</strong> ${q.question}`;
        quiz.appendChild(questionEl);

        const answersEl = document.createElement('div');
        answersEl.classList.add('answers');

        shuffledAnswers.forEach((ans, index) => {
            const btn = document.createElement('button');
            btn.classList.add('answer-btn');
            btn.innerText = ans.text;
            btn.onclick = () => selectAnswer(index, btn);
            answersEl.appendChild(btn);
        });

        quiz.appendChild(answersEl);
        updateProgress();
    } else {
        showSummary();
    }
    document.getElementById('next-btn').style.display = 'none';
}

function selectAnswer(index, btn) {
    const q = quizData[currentQuestion];
    const buttons = document.querySelectorAll('.answer-btn');

    buttons.forEach(b => b.disabled = true);

    if(index === q.shuffledCorrect){
        btn.classList.add('correct');
        score++;
        userAnswers.push(true);
    } else {
        btn.classList.add('wrong');
        buttons[q.shuffledCorrect].classList.add('correct');
        userAnswers.push(false);
    }

    const explanationEl = document.createElement('div');
    explanationEl.classList.add('explanation');
    explanationEl.innerText = q.explanation;
    document.getElementById('quiz').appendChild(explanationEl);

    document.getElementById('next-btn').style.display = 'inline-block';
}

function nextQuestion() {
    currentQuestion++;
    loadQuestion();
}

function showSummary() {
    const quiz = document.getElementById('quiz');
    quiz.innerHTML = `<h3>Quiz concluído!</h3><p>Sua pontuação: ${score} / ${quizData.length}</p>`;
    document.getElementById('progress-bar').style.width = '100%';
    document.getElementById('progress-bar').innerText = '100%';

    const summaryEl = document.getElementById('summary');
    summaryEl.innerHTML = '<h4>Resumo:</h4>';
    quizData.forEach((q, index) => {
        const item = document.createElement('div');
        item.classList.add('summary-item');
        item.innerHTML = `Pergunta ${index+1}: ${userAnswers[index] ? '✔' : '✖'} - ${q.question}<br><em>${q.explanation}</em>`;
        summaryEl.appendChild(item);
    });
}

function updateProgress() {
    const progress = (currentQuestion / quizData.length) * 100;
    const progressBar = document.getElementById('progress-bar');
    progressBar.style.width = progress + '%';
    progressBar.innerText = Math.round(progress) + '%';
}

window.onload = loadQuestion;
</script>
</body>
</html>

