# Grisssowm.github.io
<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Quiz: Ciências & Português</title>
  <style>
    :root{--bg:#f5f7fb;--card:#ffffff;--primary:#4f46e5;--success:#16a34a;--danger:#dc2626;--muted:#6b7280}
    body{font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,Helvetica,Arial;background:var(--bg);margin:0;padding:32px;display:flex;align-items:center;justify-content:center;min-height:100vh}
    .container{width:100%;max-width:900px;background:var(--card);border-radius:12px;box-shadow:0 6px 24px rgba(15,23,42,0.08);padding:28px}
    h1{margin:0 0 8px;font-size:20px}
    .meta{color:var(--muted);margin-bottom:18px}
    .progress{height:14px;background:#eef2ff;border-radius:999px;overflow:hidden;margin-bottom:18px}
    .progress > .bar{height:100%;width:0%;background:linear-gradient(90deg,var(--primary),#06b6d4);transition:width .35s}
    .progress-label{display:flex;justify-content:space-between;font-size:13px;margin-bottom:8px}
    .card{padding:18px;border-radius:10px;border:1px solid #eef2ff}
    .question{font-weight:600;margin-bottom:12px}
    .answers{display:flex;flex-direction:column;gap:10px}
    .answer-btn{padding:12px;border-radius:8px;border:1px solid #e6e9fb;background:#fff;cursor:pointer;text-align:left;font-size:15px}
    .answer-btn:hover{transform:translateY(-1px)}
    .answer-btn.correct{background:rgba(22,163,74,0.12);border-color:rgba(22,163,74,0.25)}
    .answer-btn.incorrect{background:rgba(220,38,38,0.08);border-color:rgba(220,38,38,0.18)}
    .controls{display:flex;justify-content:space-between;align-items:center;margin-top:16px}
    .next-btn{padding:10px 16px;border-radius:8px;border:none;background:var(--primary);color:#fff;cursor:pointer;display:none}
    .next-btn:disabled{opacity:.5;cursor:not-allowed}
    .summary{margin-top:20px}
    .summary-list{margin-top:12px;border-top:1px dashed #eef2ff;padding-top:12px}
    .summary-item{padding:10px;border-radius:8px;background:#fafafa;margin-bottom:8px}
    .explanation{font-size:14px;color:var(--muted);margin-top:8px}
    .score{font-weight:700;margin-top:12px}
    .small{font-size:13px;color:var(--muted)}
    footer{margin-top:14px;font-size:13px;color:var(--muted)}
    .menu{display:flex;gap:12px;justify-content:center;margin-bottom:18px}
    .menu button{padding:10px 16px;border-radius:8px;border:none;background:#111827;color:#fff;cursor:pointer}
    @media (max-width:640px){.container{padding:18px}}
  </style>
</head>
<body>
  <div class="container" id="app">
    <h1>Quiz — Ciências & Português</h1>
    <div class="meta">Escolha um tema para começar</div>

    <div class="menu" id="menu">
      <button id="btnCiencias">Ciências (50)</button>
      <button id="btnPortugues">Português (50)</button>
    </div>

    <!-- Progress area (updated per-theme) -->
    <div class="progress-label" id="progressLabel" style="display:none"><div id="qcount">Pergunta 1 / 50</div><div id="pct">0%</div></div>
    <div class="progress" id="progressBarWrap" style="display:none" aria-hidden><div class="bar" id="bar"></div></div>

    <div class="card" id="card" style="display:none">
      <div class="question" id="question">Carregando...</div>
      <div class="answers" id="answers"></div>
      <div class="explanation" id="explanation" style="display:none"></div>
      <div class="controls">
        <div class="small" id="hint">Fonte: documentos fornecidos</div>
        <div style="margin-left:auto">
          <button id="nextBtn" class="next-btn">Próxima</button>
        </div>
      </div>
    </div>

    <div class="summary" id="summary" style="display:none">
      <div class="score" id="score"></div>
      <div class="summary-list" id="summaryList"></div>
      <div style="margin-top:12px;display:flex;gap:8px;flex-wrap:wrap">
        <button id="retrySame" class="menuBtn">Jogar esse tema novamente</button>
        <button id="playOther" class="menuBtn">Jogar o outro tema</button>
        <button id="backMenu" class="menuBtn">Voltar à seleção</button>
      </div>
      <footer>Feito a partir dos documentos que você enviou — perguntas geradas automaticamente.</footer>
    </div>
  </div>

<script>
// --- QUESTIONS: split into two arrays (50 each) ---
const portugues = [
  {q: 'O que caracteriza um período simples?', choices: ['Possui apenas um verbo','Possui mais de um verbo','Possui apenas sujeito oculto','É sempre uma frase nominal'], a:0, explanation:'Período simples contém apenas um verbo ou uma locução verbal que funcione como um único verbo.'},
  {q: 'Quando um período é composto?', choices: ['Quando possui mais de um verbo','Quando tem apenas sujeito composto','Quando é uma frase nominal','Quando é curto demais'], a:0, explanation:'Período composto tem mais de um verbo, cada verbo representa uma oração.'},
  {q: 'Qual termo descreve orações que mantêm estrutura completa e podem ser unidas por conjunção?', choices:['Orações coordenadas','Orações subordinadas','Orações reduzidas','Orações absolutas'], a:0, explanation:'Orações coordenadas têm estrutura sintática independente.'},
  {q: 'O que é uma oração subordinada?', choices:['Oração que depende da principal','Oração com verbo no infinitivo','Oração sem sujeito','Oração exclamativa'], a:0, explanation:'Oração subordinada depende sintaticamente de outra, a principal.'},
  {q: 'Qual conjunção indica sentido adversativo?', choices:['mas','e','logo','pois'], a:0, explanation:'"Mas" indica oposição entre as ideias.'},
  {q: 'Qual conjunção costuma indicar conclusão?', choices:['logo','mas','e','ou'], a:0, explanation:'"Logo" e similares expressam conclusão ou consequência.'},
  {q: 'Assinale a opção que é exemplo de coordenação assindética:', choices:['Li o mito, não entendi seu significado.','Li o mito, mas não entendi.','Se gostou, diga.','Que li o livro...'], a:0, explanation:'Assindética = sem conjunção; as orações são apenas separadas por vírgula.'},
  {q: 'Qual das alternativas apresenta conjunção explicativa?', choices:['pois','ou','mas','e'], a:0, explanation:'"Pois" justifica ou explica a oração anterior.'},
  {q: 'Quando a vírgula é proibida entre orações com "e"?', choices:['Quando as orações têm o mesmo sujeito','Quando há polissíndeto','Quando são alternadas','Quando emprega conjunção adversativa'], a:0, explanation:'Com sujeito idêntico e conjunção aditiva "e", não se usa vírgula entre os verbos.'},
  {q: 'O que é polissíndeto?', choices:['Repetição da conjunção entre várias orações','Ausência de conjunção','Conjunção explicativa','Conjunção conclusiva'], a:0, explanation:'Polissíndeto = repetição de conjunções (ex.: e... e... e...).'},
  {q: 'Qual exemplo corresponde a orações coordenadas alternativas?', choices:['Ou vence o cão, ou vence a raposa.','A raposa fugiu e voltou.','Ele estudou, logo passou.','Estava triste, mas sorriu.'], a:0, explanation:'Alternativas indicam escolha entre possibilidades (ou...ou).'},
  {q: 'O pronome relativo "que" pode introduzir qual tipo de oração?', choices:['Adjetiva','Adverbial','Substantiva','Coordenada'], a:0, explanation:'"Que" introduz orações adjetivas que caracterizam um termo do substantivo.'},
  {q: 'Uma oração que funciona como objeto direto é chamada de:', choices:['Substantiva','Adjetiva','Adverbial','Coordenada'], a:0, explanation:'Orações substantivas podem exercer função de objeto direto, sujeito, etc.'},
  {q: 'Qual conjunção expressa sentido aditivo?', choices:['e','porém','logo','ou'], a:0, explanation:'Conjunções aditivas (e, nem) exprimem soma ou adição de ideias.'},
  {q: 'Marque a alternativa com conjunção que indica explicação:', choices:['porque','ou','e','mas'], a:0, explanation:'"Porque" conecta uma justificativa/explicação à oração anterior.'},
  {q: 'Qual o efeito sintático de uma oração subordinada adverbial?', choices:['Amplia o sentido do verbo principal','Substitui o sujeito','Serve como vocativo','Sempre tem pronome relativo'], a:0, explanation:'Adverbial amplia circunstâncias como tempo, causa, condição do verbo da principal.'},
  {q: 'Em "Eu trocarei o livro assim que chegar à biblioteca.", que tipo de subordinada é "assim que chegar à biblioteca"?', choices:['Adverbial temporal','Substantiva','Adjetiva','Coordenada'], a:0, explanation:'"Assim que" indica tempo — é subordinada adverbial temporal.'},
  {q: 'Em "O livro de mitologia que li me surpreendeu.", a oração "que li" é:', choices:['Adjetiva restritiva','Substantiva','Adverbial','Coordenada'], a:0, explanation:'"Que li" caracteriza "o livro" — é oração adjetiva.'},
  {q: 'Quando usamos vírgula entre coordenadas introduzidas por "e"?', choices:['Quando os sujeitos são diferentes (opcional)','Nunca','Sempre','Quando há polissíndeto apenas'], a:0, explanation:'Com sujeitos diferentes a vírgula é opcional; com polissíndeto é obrigatória.'},
  {q: 'Qual conjunção pertence ao grupo conclusivo?', choices:['portanto','e','mas','ou'], a:0, explanation:'"Portanto" indica conclusão; pertence às conclusivas.'},
  {q: 'O que diferencia uma oração coordenada sindética de uma assindética?', choices:['A presença de conjunção','O número de sujeitos','O tempo verbal','A pontuação somente'], a:0, explanation:'Sindética tem conjunção que liga orações; assindética não tem.'},
  {q: 'Exemplo de conjunção adversativa além de "mas":', choices:['porém','e','ou','logo'], a:0, explanation:'"Porém" expressa adversidade/contraste, como "mas".'},
  {q: 'Qual conjunção pode ter sentido explicativo e também conclusivo dependendo do contexto?', choices:['pois','e','ou','mas'], a:0, explanation:'"Pois" pode ter sentido explicativo; em posição pós-verbal costuma explicar.'},
  {q: 'Se uma oração não possui sentido completo sozinha e depende de outra, é:', choices:['Subordinada','Coordenada','Assindética','Sindética'], a:0, explanation:'Falta de sentido independente caracteriza oração subordinada.'},
  {q: 'As orações que aparecem justapostas sem conjunção são chamadas de:', choices:['Assindéticas','Sindéticas','Subordinadas','Relativas'], a:0, explanation:'Justapostas sem conjunção = assindéticas.'},
  {q: 'Qual é a função típica de uma oração substantiva?', choices:['Substituir um termo substantivo (ex.: atuar como objeto direto)','Caracterizar um substantivo','Indicar circunstância temporal','Unir orações coordenadas'], a:0, explanation:'Orações substantivas desempenham funções típicas de substantivo, como sujeito ou objeto.'},
  {q: 'Em "Ela pesquisou o mito e não encontrou nada.", qual sentido a conjunção "e" pode assumir?', choices:['Oposição (mas) ou adição dependendo do contexto','Aditivo sempre','Conclusivo sempre','Explicativo sempre'], a:0, explanation:'A conjunção "e" pode assumir vários sentidos conforme contexto (oposição, conclusão, etc.).'},
  {q: 'Qual estrutura demonstra que cada oração tem sujeito e predicado próprios?', choices:['Coordenação','Subordinação','Orações reduzidas','Vocativo'], a:0, explanation:'Coordenação = cada oração tem estrutura completa com sujeito e predicado.'},
  {q: 'Em "Li o mito, não entendi seu significado.", o sujeito de ambas as orações é:', choices:['Oculto (eu)','Li o mito','Você','Ele'], a:0, explanation:'Sujeito oculto é "eu" nas duas orações do exemplo.'},
  {q: 'Conjunções coordenativas morfologicamente são:', choices:['Classe invariável','Verbos auxiliares','Pronomes','Advérbios'], a:0, explanation:'Conjunções são palavras invariáveis que ligam orações.'},
  {q: 'Qual conectivo é usado para justificar algo e pode ser classificado como explicativo?', choices:['pois','ou','e','mas'], a:0, explanation:'"Pois" justifica a oração anterior: é explicativa.'},
  {q: 'Quando dizemos que uma oração é autônoma no sentido apresentado na tabela, ela é:', choices:['Adversativa','Subordinada','Reduzida','Substantiva'], a:0, explanation:'No contexto da tabela, "autônoma" relaciona-se às adversativas que apresentam independência sem relação de dependência sintática.'},
  {q: 'O que significa que as orações "se associam em um único período preservando sua estrutura completa"?', choices:['São coordenadas','São reduzidas','São subordinadas','São elipses'], a:0, explanation:'Preservar estrutura completa = orações coordenadas.'},
  {q: 'No período composto, cada verbo (quando não locução) indica:', choices:['Uma oração','Um sujeito','Uma vírgula','Um advérbio'], a:0, explanation:'Cada verbo marca uma oração no período.'},
  {q: 'Qual alternativa indica corretamente que as orações coordenadas podem expressar conclusão?', choices:['Conclusivas','Aditivas','Alternativas','Adversativas'], a:0, explanation:'Conclusivas exprimem consequência ou conclusão.'},
  {q: 'O conector "porquanto" pertence a qual grupo?', choices:['Explicativas','Aditivas','Alternativas','Adversativas'], a:0, explanation:'"Porquanto" é uma conjunção explicativa/causal.'},
  {q: 'As orações coordenadas que se unem sem conjunção são organizadas por:', choices:['Vírgulas','Ponto e vírgula','Dois pontos','Travessão'], a:0, explanation:'Assindéticas geralmente são separadas por vírgulas.'},
  {q: 'Qual é a indicação típica de uma oração coordenada conclusiva?', choices:['Expressar consequência da oração anterior','Unir sujeitos distintos','Explicar uma ideia','Ser reduzida'], a:0, explanation:'Conclusiva exprime a conclusão ou consequência da oração precedente.'}
];

const ciencias = [
  {q: 'Qual a porcentagem aproximada do plasma no volume sanguíneo?', choices: ['55%','41%','30%','70%'], a:0, explanation:'O plasma compõe aproximadamente 55% do sangue, sendo ~90% água.'},
  {q: 'Qual é a principal função da hemoglobina nas hemácias?', choices: ['Transportar oxigênio','Coagular sangue','Produzir anticorpos','Regulador térmico'], a:0, explanation:'Hemoglobina liga-se ao oxigênio e o transporta dos pulmões para as células.'},
  {q: 'Cerca de quantos litros de sangue circulam no corpo de um adulto?', choices: ['5 a 6 litros','1 a 2 litros','10 a 12 litros','200 a 300 ml'], a:0, explanation:'Um adulto tem em média 5 a 6 litros de sangue.'},
  {q: 'Onde são produzidos principalmente os elementos figurados do sangue?', choices: ['Medula óssea','Fígado','Baço','Pulmões'], a:0, explanation:'Medula óssea produz hemácias, leucócitos e plaquetas.'},
  {q: 'Qual célula sanguínea é responsável pela defesa do organismo?', choices: ['Leucócitos','Hemácias','Plaquetas','Plasma'], a:0, explanation:'Leucócitos (glóbulos brancos) atuam na defesa contra agentes estranhos.'},
  {q: 'Que componente do sangue tem cor amarelada e contém 90% água?', choices: ['Plasma','Hemácias','Plaquetas','Fibrina'], a:0, explanation:'Plasma é o componente líquido, de coloração amarelada.'},
  {q: 'Qual a função principal das plaquetas?', choices: ['Promover a coagulação','Transportar oxigênio','Combater infecções','Filtrar sangue'], a:0, explanation:'Plaquetas liberam substâncias que iniciam a coagulação para estancar sangramentos.'},
  {q: 'Quantas plaquetas aproximadamente por mm³ um adulto possui?', choices: ['~300.000','~50.000','~1.000.000','~10.000'], a:0, explanation:'Um adulto tem cerca de 300 mil plaquetas por milímetro cúbico.'},
  {q: 'A hemácia vive, em média, quanto tempo?', choices: ['90 a 120 dias','5 a 9 dias','Horas a semanas','365 dias'], a:0, explanation:'Vida útil das hemácias é de 90 a 120 dias; já as plaquetas 5-9 dias.'},
  {q: 'O que ocorre na centrifugação do sangue?', choices: ['Separação em plasma e elementos figurados','Mistura completa','Formação de coágulos','Destruição das hemácias'], a:0, explanation:'Centrifugação permite visualizar plasma e elementos figurados separadamente.'},
  {q: 'Qual é a principal função do sistema circulatório?', choices: ['Distribuir nutrientes, gases e regular temperatura','Produzir hormônios','Digestionar alimentos','Controlar movimentos'], a:0, explanation:'Sistema circulatório distribui substâncias e regula temperatura.'},
  {q: 'Qual vaso sanguíneo sai do coração levando sangue com maior pressão?', choices: ['Artéria','Veia','Capilar','Vênula'], a:0, explanation:'Artérias saem do coração e suportam alta pressão.'},
  {q: 'Quais vasos possuem válvulas para impedir refluxo?', choices: ['Veias','Artérias','Capilares','Arteríolas'], a:0, explanation:'Veias têm válvulas que auxiliam o retorno venoso contra a gravidade.'},
  {q: 'Onde ocorrem as trocas gasosas entre sangue e ar?', choices: ['Capilares (em contato com alvéolos)','Veias','Artérias','Coração'], a:0, explanation:'Capilares pulmonares em contato com alvéolos permitem trocas gasosas.'},
  {q: 'Qual tecido reveste externamente o coração?', choices: ['Pericárdio','Endocárdio','Miocárdio','Epicárdio'], a:0, explanation:'Pericárdio reveste externamente o coração e facilita seus movimentos.'},
  {q: 'Qual cavidade do coração bombeia sangue para o corpo (circuito sistêmico)?', choices: ['Ventrículo esquerdo','Ventrículo direito','Átrio direito','Átrio esquerdo'], a:0, explanation:'Ventrículo esquerdo impulsiona sangue para o corpo com alta pressão.'},
  {q: 'O que é sístole?', choices: ['Contração do músculo cardíaco','Relaxamento','Batida fraca','Um tipo de válvula'], a:0, explanation:'Sístole = contração; diástole = relaxamento.'},
  {q: 'Em repouso, a frequência cardíaca média é de:', choices: ['60 a 70 batimentos por minuto','30 a 40','100 a 120','200'], a:0, explanation:'Coração em repouso bate cerca de 60-70 vezes por minuto.'},
  {q: 'Qual componente do sangue contém aglutininas (anticorpos) do sistema ABO?', choices: ['Plasma','Hemácias','Plaquetas','Fibrina'], a:0, explanation:'As aglutininas estão no plasma e reagem contra aglutinogênios estranhos.'},
  {q: 'Qual tipo sanguíneo é considerado doador universal?', choices: ['O-','AB+','A+','B-'], a:0, explanation:'O- não possui aglutinogênios e pode ser dado a todos os tipos.'},
  {q: 'Por que um receptor Rh- pode reagir mal a sangue Rh+?', choices: ['Porque produz anticorpos anti-Rh','Porque tem mais hemácias','Porque tem mais plasma','Porque Rh- tem trombose'], a:0, explanation:'Receptor Rh- pode formar anticorpos anti-Rh que aglutinam hemácias Rh+.'},
  {q: 'Quem pode ser doador de sangue segundo o documento?', choices: ['Pessoas entre 16 e 69 anos (com regras)','Qualquer pessoa sem restrições','Apenas maiores de 50','Apenas profissionais de saúde'], a:0, explanation:'Doadores geralmente entre 16 e 69 anos; menores de 18 precisam de autorização.'},
  {q: 'Qual requisito mínimo de peso para ser doador?', choices: ['50 kg','40 kg','60 kg','70 kg'], a:0, explanation:'Peso mínimo citado: 50 kg.'},
  {q: 'Quanto tempo homens devem aguardar entre doações?', choices: ['60 dias','30 dias','90 dias','120 dias'], a:0, explanation:'Homens: pelo menos 60 dias entre doações; mulheres 90 dias.'},
  {q: 'Durante a pandemia, o que aconteceu com os bancos de sangue?', choices: ['Redução significativa nas doações','Aumento recorde de doações','Nenhuma mudança','Foram extintos'], a:0, explanation:'Houve queda nas doações, deixando os bancos em situação crítica.'},
  {q: 'Qual doença está associada a produção descontrolada de células sanguíneas na medula?', choices: ['Leucemia','Anemia','Hemofilia','Trombose'], a:0, explanation:'Leucemia é câncer da medula, produz células imaturas e descontroladas.'},
  {q: 'Anemia caracteriza-se por:', choices: ['Redução de hemácias ou hemoglobina','Excesso de plaquetas','Infecção bacteriana','Coagulação anormal'], a:0, explanation:'Anemia = baixa contagem de hemácias ou hemoglobina, reduz transporte de O2.'},
  {q: 'Trombose é:', choices: ['Formação de coágulo que obstrui vaso','Falta de plaquetas','Tipo de leucemia','Infecção viral'], a:0, explanation:'Trombo = coágulo que pode obstruir vasos e causar sintomas locais.'},
  {q: 'Qual órgão remove hemácias antigas do sangue?', choices: ['Baço e fígado','Rins','Coração','Pulmões'], a:0, explanation:'Baço e fígado removem hemácias envelhecidas.'},
  {q: 'Qual pigmento contém ferro e transporta oxigênio?', choices: ['Hemoglobina','Mioglobina','Fibrina','Albumina'], a:0, explanation:'Hemoglobina é pigmento rico em ferro presente nas hemácias.'},
  {q: 'Qual é a cor natural do plasma?', choices: ['Amarelada','Vermelha','Incolor','Verde'], a:0, explanation:'Plasma tem tom amarelado devido a substâncias dissolvidas.'},
  {q: 'As hemácias têm núcleo?', choices: ['Não (anucleadas)','Sim, grande núcleo','Apenas em mulheres','Apenas durante infecções'], a:0, explanation:'Hemácias humanas maduras são anucleadas (sem núcleo).'},
  {q: 'Onde o sangue circula obrigatoriamente nos humanos?', choices: ['Dentro dos vasos sanguíneos','No espaço intersticial','Fora do corpo','Dentro dos ossos'], a:0, explanation:'Sangue humano circula dentro de vasos: artérias, veias e capilares.'},
  {q: 'Qual é o papel dos capilares?', choices: ['Permitir trocas de substâncias entre sangue e células','Bombear sangue','Produzir sangue','Filtrar urina'], a:0, explanation:'Capilares, com paredes finas, permitem trocas gasosas e de nutrientes.'},
  {q: 'O que acontece quando aglutininas do plasma encontram aglutinogênios incompatíveis?', choices: ['Aglutinação das hemácias e risco de obstrução vascular','Aumento de plaquetas','Redução de plasma','Cura imediata'], a:0, explanation:'Aglutinação leva ao entupimento de vasos e pode ser fatal.'},
  {q: 'Qual das opções é impedimento definitivo para doação segundo o texto?', choices: ['Soropositividade para HIV','Ter colírio no olho','Usar óculos','Alergia sazonal'], a:0, explanation:'Ser soropositivo para HIV é implicação permanente que impede a doação.'},
  {q: 'O que é fibrina?', choices: ['Proteína que forma rede no coágulo','Tipo de hemácia','Hormônio','Anticorpo'], a:0, explanation:'Fibrina forma a malha que, junto com plaquetas, constitui o coágulo.'},
  {q: 'Qual procedimento garante que o sangue coletado seja seguro para transfusão?', choices: ['Conhecer tipos sanguíneos do doador e receptor','Dar plasma a qualquer pessoa','Misturar todos os tipos','Não testar'], a:0, explanation:'Teste de compatibilidade (ABO e Rh) evita reações transfusionais.'},
  {q: 'Qual elemento ajuda a distribuir calor pelo corpo?', choices: ['Sangue','Plaquetas','Hemácias sozinhas','Glândulas sebáceas'], a:0, explanation:'Sangue participa do controle térmico distribuindo calor.'},
  {q: 'Qual é o efeito da COVID-19 mencionado em relação ao sangue?', choices: ['Pode favorecer trombose','Aumenta o número de plaquetas permanentemente','Cura anemia','Diminui plasma a zero'], a:0, explanation:'COVID-19 pode aumentar risco de trombose em alguns casos.'},
  {q: 'Qual das afirmativas é verdadeira sobre hemácias?', choices: ['São a célula em maior quantidade no sangue','São as menos numerosas','Têm núcleo grande','Vivem anos'], a:0, explanation:'Hemácias são as mais numerosas; vivem cerca de 90-120 dias.'},
  {q: 'O que significa "elementos figurados" do sangue?', choices: ['Células e fragmentos celulares (hemácias, leucócitos, plaquetas)','Só o plasma','Somente proteínas','Fibrina apenas'], a:0, explanation:'Elementos figurados = células e fragmentos suspensos no plasma.'},
  {q: 'Em caso de transfusão, por que O- é considerado doador universal mas receptor restrito?', choices: ['Porque não possui aglutinogênios, mas possui aglutininas anti-A/anti-B/anti-Rh','Porque é raro','Porque tem mais plasma','Porque é sempre positivo'], a:0, explanation:'O- não tem antígenos na hemácia, mas seu plasma contém anticorpos que atacam outros tipos.'}
];

// Utility functions
function shuffle(array){for(let i=array.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[array[i],array[j]]=[array[j],array[i]]}return array}

// App state per theme
let state = {
  theme: null, // 'ciencias' or 'portugues'
  questions: [],
  order: [],
  idx: 0,
  userAnswers: []
};

// DOM refs
const menuEl = document.getElementById('menu');
const cardEl = document.getElementById('card');
const questionEl = document.getElementById('question');
const answersEl = document.getElementById('answers');
const explanationEl = document.getElementById('explanation');
const nextBtn = document.getElementById('nextBtn');
const progressLabel = document.getElementById('progressLabel');
const qcount = document.getElementById('qcount');
const bar = document.getElementById('bar');
const pct = document.getElementById('pct');
const progressBarWrap = document.getElementById('progressBarWrap');
const summaryEl = document.getElementById('summary');
const scoreEl = document.getElementById('score');
const summaryList = document.getElementById('summaryList');
const retrySame = document.getElementById('retrySame');
const playOther = document.getElementById('playOther');
const backMenu = document.getElementById('backMenu');

// Menu buttons
document.getElementById('btnCiencias').addEventListener('click', ()=>startTheme('ciencias'));
document.getElementById('btnPortugues').addEventListener('click', ()=>startTheme('portugues'));

// Next button
nextBtn.addEventListener('click', ()=>{
  state.idx++;
  if(state.idx >= state.order.length) showSummary();
  else renderQuestion();
});

retrySame.addEventListener('click', ()=> startTheme(state.theme));
playOther.addEventListener('click', ()=> startTheme(state.theme === 'ciencias' ? 'portugues' : 'ciencias'));
backMenu.addEventListener('click', ()=>{
  summaryEl.style.display = 'none';
  menuEl.style.display = 'flex';
  progressLabel.style.display = 'none';
  progressBarWrap.style.display = 'none';
});

function startTheme(theme){
  state.theme = theme;
  state.questions = theme === 'ciencias' ? JSON.parse(JSON.stringify(ciencias)) : JSON.parse(JSON.stringify(portugues));
  state.order = shuffle(state.questions.map((_,i)=>i));
  state.idx = 0;
  state.userAnswers = [];

  // UI
  menuEl.style.display = 'none';
  summaryEl.style.display = 'none';
  cardEl.style.display = 'block';
  progressLabel.style.display = 'flex';
  progressBarWrap.style.display = 'block';

  renderQuestion();
}

function renderQuestion(){
  const qIndex = state.order[state.idx];
  const current = state.questions[qIndex];
  questionEl.textContent = `${state.idx+1}. ${current.q}`;
  answersEl.innerHTML = '';
  explanationEl.style.display = 'none';
  nextBtn.style.display = 'none'; nextBtn.disabled = false;

  // shuffle choices
  const choices = current.choices.map((c,i)=>({text:c,orig:i}));
  shuffle(choices);

  choices.forEach(ch => {
    const btn = document.createElement('button');
    btn.className = 'answer-btn';
    btn.innerText = ch.text;
    btn.addEventListener('click', ()=>handleSelect(btn, ch.orig, current));
    answersEl.appendChild(btn);
  });

  // progress
  const total = state.order.length;
  qcount.textContent = `Pergunta ${state.idx+1} / ${total}`;
  const percentage = Math.round(((state.idx)/total)*100);
  bar.style.width = percentage + '%';
  pct.textContent = percentage + '%';
}

function handleSelect(btn, origIndex, current){
  if(nextBtn.style.display === 'block') return; // already answered
  const btns = answersEl.querySelectorAll('.answer-btn');
  btns.forEach(b=>b.disabled=true);

  const correctIndex = current.a;
  const isCorrect = (origIndex === correctIndex);
  if(isCorrect) btn.classList.add('correct');
  else {
    btn.classList.add('incorrect');
    // highlight correct
    btns.forEach(b=>{ if(b.innerText === current.choices[correctIndex]) b.classList.add('correct'); });
  }

  explanationEl.style.display = 'block';
  explanationEl.textContent = current.explanation || '';

  // store
  state.userAnswers[state.idx] = {selected: origIndex, correct: correctIndex, isCorrect, question: current.q};

  // show next
  nextBtn.style.display = 'block';
}

function showSummary(){
  cardEl.style.display = 'none';
  progressLabel.style.display = 'none';
  progressBarWrap.style.display = 'none';
  summaryEl.style.display = 'block';

  const total = state.order.length;
  const correctCount = state.userAnswers.filter(a=>a && a.isCorrect).length;
  scoreEl.textContent = `Você acertou ${correctCount} de ${total} perguntas.`;

  summaryList.innerHTML = '';
  state.order.forEach((qIdx,i)=>{
    const q = state.questions[qIdx];
    const ua = state.userAnswers[i];
    const ok = ua && ua.isCorrect;
    const userText = ua ? (q.choices[ua.selected] || '—') : '—';
    const item = document.createElement('div');
    item.className = 'summary-item';
    const mark = ok ? '✔' : '✖';
    item.innerHTML = `<strong>${i+1}. ${q.q}</strong><div style="margin-top:6px">${mark} Sua resposta: <em>${userText}</em></div><div style="margin-top:6px;color:#374151">Resposta correta: <strong>${q.choices[q.a]}</strong></div><div class="explanation">Explicação: ${q.explanation || ''}</div>`;
    summaryList.appendChild(item);
  });
}

</script>
</body>
</html>
