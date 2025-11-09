[mision_saludable_juego_ods_3_index (1).html](https://github.com/user-attachments/files/23442934/mision_saludable_juego_ods_3_index.1.html)
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Misión Saludable — ODS 3</title>
  <style>
    :root{--bg:#f6fbff;--card:#ffffff;--accent:#2b9bf3;--muted:#6b7280}
    *{box-sizing:border-box;font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto,'Helvetica Neue',Arial}
    body{margin:0;background:linear-gradient(180deg,#e6f3ff 0%,var(--bg) 100%);min-height:100vh;display:flex;align-items:center;justify-content:center;padding:24px}
    .app{width:100%;max-width:980px;background:var(--card);border-radius:16px;box-shadow:0 8px 30px rgba(27,50,78,0.08);overflow:hidden}
    header{display:flex;align-items:center;gap:16px;padding:20px;border-bottom:1px solid #eef3fb}
    h1{margin:0;font-size:20px;color:#073063}
    .subtitle{color:var(--muted);font-size:13px}

    .board{display:grid;grid-template-columns:320px 1fr;gap:18px;padding:22px}
    .card{background:linear-gradient(180deg,#fff,#fbfdff);padding:16px;border-radius:12px;border:1px solid #eef6ff}
    .chars{display:flex;flex-direction:column;gap:12px}
    .char{display:flex;align-items:center;gap:12px;padding:10px;border-radius:10px;cursor:pointer;border:2px dashed transparent}
    .char:hover{background:#f0f9ff}
    .char.selected{border-color:var(--accent);box-shadow:0 6px 18px rgba(43,155,243,0.12)}
    .char .emoji{font-size:30px}
    .info{font-size:14px;color:#073063}

    .game-area{display:flex;flex-direction:column;gap:14px}
    .question{font-size:18px;font-weight:600;color:#073063}
    .answers{display:grid;grid-template-columns:1fr 1fr;gap:10px}
    button.answer{padding:12px;border-radius:10px;border:1px solid #d6e9ff;background:white;cursor:pointer;font-size:14px}
    button.answer:hover{transform:translateY(-2px)}
    .status{display:flex;justify-content:space-between;align-items:center}
    .stars{font-size:20px}
    .tip{background:#f1fbff;padding:10px;border-radius:8px;border:1px solid #e1f3ff;font-size:13px}
    .controls{display:flex;gap:8px}
    .btn{padding:8px 10px;border-radius:8px;border:none;cursor:pointer}
    .btn.primary{background:var(--accent);color:white}
    .btn.ghost{background:transparent;border:1px solid #dbeefa}
    footer{padding:12px 22px;border-top:1px solid #eef3fb;color:var(--muted);font-size:13px}

    .age-btn{padding:10px 14px;border-radius:8px;border:1px solid #dbeefa;background:white;cursor:pointer}
    .age-btn.selected{background:linear-gradient(90deg,#2b9bf3,#62c0ff);color:#fff;border-color:transparent}

    @media (max-width:900px){.board{grid-template-columns:1fr;}}
  </style>
</head>
<body>
  <div class="app" role="application" aria-label="Juego Misión Saludable">
    <header>
      <div style="font-size:34px">🩺</div>
      <div>
        <h1>Misión Saludable — ODS 3</h1>
        <div class="subtitle">Juego interactivo: responde preguntas y gana estrellas</div>
      </div>
    </header>

    <div class="board">
      <aside class="card">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">
          <strong>Elige un personaje</strong>
        </div>
        <div class="chars" id="chars">
          <div class="char" data-id="luna" tabindex="0">
            <div class="emoji">👧</div>
            <div>
              <div style="font-weight:700">Luna</div>
              <div style="font-size:12px;color:var(--muted)">Le encanta jugar y aprender sobre salud</div>
            </div>
          </div>

          <div class="char" data-id="tomas" tabindex="0">
            <div class="emoji">🧑‍🌾</div>
            <div>
              <div style="font-weight:700">Tomás</div>
              <div style="font-size:12px;color:var(--muted)">Curioso y valiente, siempre pregunta a los mayores</div>
            </div>
          </div>

          <div class="char" data-id="max" tabindex="0">
            <div class="emoji">🐶</div>
            <div>
              <div style="font-weight:700">Max</div>
              <div style="font-size:12px;color:var(--muted)">Un perrito muy saludable que necesita tu ayuda</div>
            </div>
          </div>
        </div>

        <hr style="margin:12px 0;border:none;border-top:1px solid #f0f7ff">
        <div style="font-weight:700;margin-bottom:6px">Progreso</div>
        <div id="progressText">No has comenzado — elige un personaje</div>
        <div style="height:8px;background:#eef7ff;border-radius:999px;margin-top:8px;overflow:hidden">
          <div id="progressBar" style="width:0%;height:100%;background:linear-gradient(90deg,#2b9bf3,#62c0ff)"></div>
        </div>

        <div style="margin-top:12px;font-size:13px;color:var(--muted)">Estrellas: <span id="starCount">0</span> ⭐</div>
        <div style="margin-top:12px;display:flex;gap:8px">
          <button class="btn ghost" id="restart">Reiniciar</button>
          <button class="btn primary" id="startBtn">Comenzar misión</button>
        </div>
      </aside>

      <main class="card game-area" aria-live="polite">
        <div class="status" style="align-items:center">
          <div style="display:flex;gap:12px;align-items:center">
            <div id="currentCharacter">Personaje: —</div>
            <div class="stars" id="starsInline">—</div>
          </div>
          <div id="questionNumber">Pregunta 0 / 0</div>
        </div>

        <!-- Age selector -->
        <div id="ageSelector" style="text-align:center;margin-top:16px">
          <h3>Elige tu edad</h3>
          <div style="display:flex;gap:10px;justify-content:center;margin-top:8px">
            <button class="age-btn" data-group="1-5">1-5 años</button>
            <button class="age-btn" data-group="6-10">6-10 años</button>
            <button class="age-btn" data-group="11-17">11-17 años</button>
          </div>
        </div>

        <!-- Quiz area -->
        <div id="quiz" style="margin-top:18px">
          <div class="question" id="questionText">Selecciona un personaje, elige tu edad y presiona "Comenzar misión" para empezar.</div>
          <div class="answers" id="answers"></div>
          <div class="tip" id="tipBox" style="display:none;margin-top:12px"></div>

          <div style="display:flex;justify-content:space-between;align-items:center;margin-top:12px">
            <div id="resultMsg" style="font-weight:700"></div>
            <div class="controls">
              <button class="btn ghost" id="nextBtn" disabled>Siguiente</button>
              <button class="btn primary" id="finishBtn" disabled>Terminar</button>
            </div>
          </div>
        </div>

        <div style="margin-top:12px;color:var(--muted);font-size:13px">Consejo: aprender sobre salud te ayuda a estar feliz. ¡Escoge bien!</div>
      </main>
    </div>

    <footer>
      Misión Saludable — Objetivo 3 (Salud y Bienestar). Diseñado para niños. Puedes reiniciar y jugar otra vez.
    </footer>

    <section style="padding:20px;border-top:1px solid #eef3fb;background:#f9fdff;font-size:14px;color:#073063">
      <strong>¿Cómo ayudar a cumplir la ODS 3?</strong>
      <ul style="margin-top:8px;line-height:1.5">
        <li>Lavarse las manos para evitar enfermedades.</li>
        <li>Vacunarse a tiempo para proteger la salud.</li>
        <li>Hacer ejercicio y comer saludable.</li>
        <li>Dormir bien para que el cuerpo descanse.</li>
        <li>Hablar con un adulto cuando te sientes triste o preocupado.</li>
      </ul>
    </section>
  </div>

  <script>
    // --- Data sets ---
    const ageQuestions = {
      '1-5': [
        {q: '¿Qué debes hacer antes de comer?', a: ['Jugar con la comida', 'Lavarte las manos', 'Soplar la mesa'], c: 1, tip: 'Lavarse las manos evita gérmenes.'},
        {q: '¿Qué alimento es saludable?', a: ['Dulces', 'Frutas', 'Chatarra'], c: 1, tip: 'Las frutas tienen vitaminas.'},
        {q: '¿Qué debes hacer cuando estornudas?', a: ['Tapar con el brazo', 'Gritar', 'Correr'], c: 0, tip: 'Tapar con el brazo evita contagios.'},
        {q: '¿Qué bebida es mejor para el cuerpo?', a: ['Refresco', 'Agua', 'Gaseosa'], c: 1, tip: 'El agua hidrata sin azúcares.'},
        {q: '¿Qué debes hacer antes de dormir?', a: ['Saltar sin parar', 'Cepillarte los dientes', 'Ver TV toda la noche'], c: 1, tip: 'Cepillar los dientes evita caries.'},
        {q: '¿Qué comida te hace fuerte?', a: ['Chocolates', 'Verduras', 'Nada'], c: 1, tip: 'Las verduras tienen nutrientes.'},
        {q: '¿Qué debes hacer si te raspas la rodilla?', a: ['Lavar y avisar', 'Ignorar', 'Comer'], c: 0, tip: 'Lava y avisa a un adulto.'},
        {q: '¿Cómo te cuidas del sol?', a: ['Sombrero', 'Mirar al sol', 'Nada'], c: 0, tip: 'Usa sombrero y protector solar.'},
        {q: '¿Qué hace un doctor?', a: ['Jugar fútbol', 'Cuidar la salud', 'Cocinar'], c: 1, tip: 'Los doctores ayudan a sanar.'},
        {q: '¿Qué debes hacer al toser?', a: ['Cubrir boca', 'Gritar', 'Nada'], c: 0, tip: 'Cubrir la boca evita contagios.'}
      ],
      '6-10': [
        {q: '¿Para qué sirven las vacunas?', a: ['Enfermarme', 'Protegerme', 'Dormir'], c: 1, tip: 'Vacunarse protege contra enfermedades.'},
        {q: '¿Qué debes hacer todos los días?', a: ['Ver TV todo el día', 'Cepillarme los dientes', 'No comer'], c: 1, tip: 'Cepillarse mantiene dientes sanos.'},
        {q: '¿Qué pasa si no dormimos bien?', a: ['Estamos cansados', 'Somos invisibles', 'Nada'], c: 0, tip: 'Dormir ayuda a recuperar energía.'},
        {q: '¿Qué es más saludable?', a: ['Frutas', 'Muchos dulces', 'Nada'], c: 0, tip: 'Frutas = vitaminas.'},
        {q: '¿Qué ayuda al corazón?', a: ['Ejercicio', 'No moverme', 'Gritar'], c: 0, tip: 'El ejercicio fortalece el corazón.'},
        {q: '¿Qué debes hacer cuando te enfermas?', a: ['Tomar medicina sin adulto', 'Decir a un adulto', 'Salir a jugar'], c: 1, tip: 'Dile a un adulto para recibir ayuda.'},
        {q: '¿Qué parte del cuerpo limpian los dientes?', a: ['Orejas', 'Boca', 'Pies'], c: 1, tip: 'Los dientes limpian la boca.'},
        {q: '¿Qué debes hacer después de jugar afuera?', a: ['Lavarte', 'No bañarte', 'Comer tierra'], c: 0, tip: 'Báñate o lávate las manos.'},
        {q: '¿Qué pasa si comes mucha comida chatarra?', a: ['Nada', 'Puede hacer daño', 'Vuelves superhéroe'], c: 1, tip: 'La comida chatarra puede afectar tu salud.'},
        {q: '¿Qué debes hacer si alguien está triste?', a: ['Reírte', 'Ayudar o hablar', 'Ignorar'], c: 1, tip: 'Escuchar y ayudar hace bien.'}
      ],
      '11-17': [
        {q: '¿Qué es la salud mental?', a: ['Cuidar lo que siento y pienso', 'Solo hacer deporte', 'Comer dulces'], c: 0, tip: 'Cuidar emociones es parte de la salud.'},
        {q: '¿Cuál es una forma de cuidar tu salud?', a: ['Fumar', 'Dormir bien', 'No desayunar'], c: 1, tip: 'Dormir ayuda al rendimiento.'},
        {q: '¿Qué causa el alcohol en adolescentes?', a: ['Mejora la salud', 'Hace daño a órganos', 'Te hace volar'], c: 1, tip: 'El alcohol puede dañar órganos.'},
        {q: '¿Qué hacer si tienes ansiedad o tristeza?', a: ['Hablar con alguien', 'Ignorar', 'Aislarse'], c: 0, tip: 'Hablar ayuda a encontrar soluciones.'},
        {q: '¿Qué alimento da energía saludable?', a: ['Frutas y verduras', 'Solo dulces', 'Nada'], c: 0, tip: 'Los alimentos balanceados dan energía.'},
        {q: '¿Qué ayuda a evitar enfermedades?', a: ['Vacunarse', 'Compartir botellas', 'No lavarse'], c: 0, tip: 'Vacunarse protege a la comunidad.'},
        {q: '¿Qué es higiene personal?', a: ['No bañarse', 'Cuidar limpieza del cuerpo', 'Solo peinarse'], c: 1, tip: 'La higiene previene infecciones.'},
        {q: '¿Qué pasa si no haces ejercicio?', a: ['Más salud', 'Menos energía', 'Superpoderes'], c: 1, tip: 'La inactividad reduce energía.'},
        {q: '¿Con quién debes hablar sobre tu salud?', a: ['Con adultos y médicos', 'Con nadie', 'Con desconocidos'], c: 0, tip: 'Busca apoyo en personas de confianza.'},
        {q: '¿Qué hacer si alguien se cae y sangra?', a: ['Ayudar y avisar', 'Reír', 'Ignorar'], c: 0, tip: 'Ayuda y busca a un adulto o médico.'}
      ]
    };

    // Otro conjunto de preguntas generales (nombre distinto para evitar colisiones)
    const generalQuestions = [
      {q: '¿Cuál es una buena forma de cuidar tu cuerpo?',
       a: ['Dormir poco','Comer frutas y verduras','Jugar videojuegos todo el día'],
       correct:1, tip:'¡Las frutas y verduras te dan vitaminas y energía.'},

      {q: '¿Qué debes hacer para evitar enfermarte?',
       a:['No lavarte las manos','Lavarte las manos antes de comer','Compartir tu cepillo de dientes'],
       correct:1, tip:'Lavarse las manos con agua y jabón elimina gérmenes.'},

      {q: '¿Qué ejercicio es bueno para ti?',
       a:['Dormir todo el día','Correr o montar bicicleta','Comer muchos dulces'],
       correct:1, tip:'Mover el cuerpo te hace más fuerte y feliz.'},

      {q: 'Si te sientes triste, ¿qué debes hacer?',
       a:['Callarte y no decir nada','Hablar con un adulto o doctor','Irte solo'],
       correct:1, tip:'Hablar ayuda a encontrar soluciones y sentirte mejor.'},

      {q: '¿Qué ayuda a que las vacunas funcionen?',
       a:['No ir al doctor','Vacunarse a tiempo','Comer solo dulces'],
       correct:1, tip:'Las vacunas ayudan a tu cuerpo a prevenir enfermedades.'},

      {q: '¿Por qué es importante dormir bien?',
       a:['Te ayuda a descansar y crecer','Te hace sentir triste','Te impide aprender'],
       correct:0, tip:'Dormir ayuda a que tu cerebro y cuerpo descansen.'},

      {q: '¿Qué debemos beber para mantenernos hidratados?',
       a:['Agua','Solo refrescos','Aceite'],
       correct:0, tip:'El agua es la mejor para hidratar el cuerpo.'},

      {q: '¿Qué es parte de un estilo de vida saludable?',
       a:['Comer balanceado, descansar y hacer ejercicio','No hacer nada','Comer solo dulces'],
       correct:0, tip:'Equilibrio en alimentación, sueño y actividad física.'}
    ];

    // --- UI elements ---
    const charsEl = document.getElementById('chars');
    const charEls = Array.from(document.querySelectorAll('.char'));
    const startBtn = document.getElementById('startBtn');
    const restartBtn = document.getElementById('restart');
    const currentCharacter = document.getElementById('currentCharacter');
    const questionText = document.getElementById('questionText');
    const answersEl = document.getElementById('answers');
    const tipBox = document.getElementById('tipBox');
    const nextBtn = document.getElementById('nextBtn');
    const finishBtn = document.getElementById('finishBtn');
    const resultMsg = document.getElementById('resultMsg');
    const progressBar = document.getElementById('progressBar');
    const progressText = document.getElementById('progressText');
    const starCountEl = document.getElementById('starCount');
    const starsInline = document.getElementById('starsInline');
    const questionNumber = document.getElementById('questionNumber');

    let selectedChar = null;
    let selectedAgeGroup = null;
    let score = 0;
    let qIndex = 0;
    let currentQuestionSet = [];

    // --- Helpers ---
    function updateProgress(){
      const pct = currentQuestionSet.length ? Math.round((qIndex / currentQuestionSet.length) * 100) : 0;
      progressBar.style.width = pct + '%';
      progressText.textContent = currentQuestionSet.length ? `Pregunta ${Math.min(qIndex,currentQuestionSet.length)} de ${currentQuestionSet.length}` : 'No has comenzado — elige un personaje y edad';
      questionNumber.textContent = currentQuestionSet.length ? `Pregunta ${Math.min(qIndex+1,currentQuestionSet.length)} / ${currentQuestionSet.length}` : 'Pregunta 0 / 0';
      starCountEl.textContent = score;
      let s = '';
      for(let i=0;i<Math.min(score,5);i++) s += '⭐';
      if(!s) s = '—';
      starsInline.textContent = s;
    }

    function capitalize(s){ return s ? s.charAt(0).toUpperCase()+s.slice(1) : '' }

    charEls.forEach(el=>{
      el.addEventListener('click',()=>selectChar(el.dataset.id));
      el.addEventListener('keydown',(e)=>{ if(e.key==='Enter') selectChar(el.dataset.id) });
    });

    function selectChar(id){
      selectedChar = id;
      charEls.forEach(c=>c.classList.toggle('selected', c.dataset.id===id));
      currentCharacter.textContent = `Personaje: ${capitalize(id)}`;
      resultMsg.textContent = '';
      updateProgress();
    }

    // Age selection
    document.querySelectorAll('.age-btn').forEach(b=>{
      b.addEventListener('click', ()=>{
        document.querySelectorAll('.age-btn').forEach(x=>x.classList.remove('selected'));
        b.classList.add('selected');
        selectedAgeGroup = b.dataset.group;
        resultMsg.textContent = '';
        updateProgress();
      });
    });

    // Start button
    startBtn.addEventListener('click', ()=>{
      if(!selectedChar){ alert('Por favor elige un personaje antes de comenzar.'); return }
      if(!selectedAgeGroup){ alert('Por favor elige tu grupo de edad.'); return }
      score = 0; qIndex = 0; resultMsg.textContent=''; tipBox.style.display='none';
      currentQuestionSet = ageQuestions[selectedAgeGroup] ? JSON.parse(JSON.stringify(ageQuestions[selectedAgeGroup])) : JSON.parse(JSON.stringify(generalQuestions));
      renderQuestion();
      updateProgress();
    });

    // Restart
    restartBtn.addEventListener('click', ()=>{
      score = 0; qIndex = 0; selectedChar = null; selectedAgeGroup = null; currentQuestionSet = [];
      charEls.forEach(c=>c.classList.remove('selected'));
      document.querySelectorAll('.age-btn').forEach(x=>x.classList.remove('selected'));
      currentCharacter.textContent = 'Personaje: —';
      questionText.textContent = 'Selecciona un personaje, elige tu edad y presiona "Comenzar misión" para ver la primera pregunta.';
      answersEl.innerHTML=''; tipBox.style.display='none'; resultMsg.textContent=''; progressBar.style.width='0%'; progressText.textContent='No has comenzado — elige un personaje'; starCountEl.textContent='0'; questionNumber.textContent='Pregunta 0 / 0'; starsInline.textContent='—';
    });

    // Render question
    function renderQuestion(){
      if(qIndex >= currentQuestionSet.length){ finishGame(); return; }
      const q = currentQuestionSet[qIndex];
      questionText.textContent = q.q;
      answersEl.innerHTML = '';
      q.a.forEach((opt,i)=>{
        const btn = document.createElement('button');
        btn.className='answer';
        btn.textContent = opt;
        btn.addEventListener('click', ()=> selectAnswer(i));
        answersEl.appendChild(btn);
      });
      nextBtn.disabled = true; finishBtn.disabled = false; tipBox.style.display='none'; resultMsg.textContent='';
      updateProgress();
    }

    function selectAnswer(i){
      const q = currentQuestionSet[qIndex];
      const buttons = Array.from(answersEl.children);
      buttons.forEach((b,idx)=>{ b.disabled=true; if(idx===q.c || idx===q.correct) b.style.borderColor='green'; });
      const correctIndex = (typeof q.c !== 'undefined') ? q.c : q.correct;
      if(i === correctIndex){
        score += 1; resultMsg.textContent = '¡Respuesta buena! ¡Correcto! +1 estrella';
        buttons[i].style.background = '#e6ffef';
      } else {
        score = Math.max(0, score - 1); resultMsg.textContent = 'Respuesta mala... -1 estrella';
        buttons[i].style.background = '#ffecec';
      }
      tipBox.style.display='block'; tipBox.textContent = q.tip || '';
      nextBtn.disabled = false; finishBtn.disabled = false;
      qIndex += 1;
      updateProgress();
    }

    nextBtn.addEventListener('click', ()=>{
      if(qIndex >= currentQuestionSet.length){ finishGame(); return }
      renderQuestion();
    });

    finishBtn.addEventListener('click', finishGame);

    function finishGame(){
      answersEl.innerHTML='';
      questionText.textContent = `¡Misión completada! Has obtenido ${score} estrella(s).`;
      tipBox.style.display='block'; tipBox.textContent = score >= 6 ? '¡Excelente trabajo! Sigue cuidando tu salud todos los días.' : '¡Buen intento! Repasa los consejos y vuelve a jugar.';
      resultMsg.textContent = '';
      nextBtn.disabled = true; finishBtn.disabled = true; updateProgress();
    }

    // Initial state
    updateProgress();

    // --- Extra: small automated tests in console to ensure datasets valid ---
    (function runBasicChecks(){
      console.group('Misión Saludable — checks');
      const groups = Object.keys(ageQuestions);
      console.log('Age groups found:', groups);
      groups.forEach(g=>{
        const arr = ageQuestions[g];
        console.assert(Array.isArray(arr) && arr.length===10, `El grupo ${g} debe tener 10 preguntas (tiene ${arr.length})`);
        arr.forEach((q,i)=>{
          console.assert(q.q && Array.isArray(q.a) && (typeof q.c === 'number'), `Pregunta ${i} del grupo ${g} mal formada`);
        });
      });
      console.assert(Array.isArray(generalQuestions) && generalQuestions.length>0, 'generalQuestions debe estar definido');
      console.groupEnd();
    })();

  </script>
</body>
</html>
