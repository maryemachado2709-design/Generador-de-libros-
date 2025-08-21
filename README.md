<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>📚 Generador de Libros</title>
  <style>
    :root{
      --bg:#0f172a;         /* slate-900 */
      --panel:#111827ee;    /* gray-900 */
      --muted:#94a3b8;      /* slate-400 */
      --text:#e5e7eb;       /* gray-200 */
      --brand:#6366f1;      /* indigo-500 */
      --brand-2:#22d3ee;    /* cyan-400 */
      --accent:#10b981;     /* emerald-500 */
      --danger:#ef4444;     /* red-500 */
    }
    *{box-sizing:border-box}
    body{
      margin:0; padding:0; font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, Cantarell, 'Helvetica Neue', Arial;
      background: radial-gradient(1200px 800px at 20% -10%, #1f2937, transparent),
                  radial-gradient(900px 600px at 120% 10%, #0ea5e9, transparent),
                  linear-gradient(180deg, #0b1022, #0b1022 60%, #0a0f1f 100%);
      color:var(--text);
      min-height:100dvh;
    }
    header{
      position:sticky; top:0; z-index:10;
      backdrop-filter: blur(8px);
      background: #0b1022cc; border-bottom: 1px solid #1e293b;
    }
    .wrap{ max-width:1100px; margin:0 auto; padding:18px 20px; }
    h1{ margin:0; font-size: clamp(20px, 3vw, 28px); letter-spacing:.2px; display:flex; align-items:center; gap:.5rem; }
    h1 .badge{ padding:.22rem .5rem; border-radius:999px; background:linear-gradient(90deg,var(--brand),var(--brand-2)); font-size:.78rem; color:#0b1022; font-weight:700; }

    .grid{ display:grid; grid-template-columns: 1.2fr 2fr; gap:18px; align-items:start; }
    @media (max-width: 980px){ .grid{ grid-template-columns: 1fr; } }

    .card{
      background: linear-gradient(180deg, #0f172a, #0b1327);
      border:1px solid #1f2a44; border-radius:16px; padding:16px; box-shadow:0 10px 30px #00000040;
    }
    .card h2{ margin:4px 0 14px; font-size:1.05rem; color:#cbd5e1; }

    label{ display:block; color:#cbd5e1; margin:10px 0 6px; font-size:.9rem }
    input[type="text"], select, textarea{
      width:100%; padding:12px 14px; background:#0b1224; border:1px solid #1f2a44; border-radius:12px; color:var(--text);
      outline:none; transition:.2s border;
    }
    textarea{ resize:vertical; min-height:80px }
    input:focus, select:focus, textarea:focus{ border-color:#334155 }

    .row{ display:grid; grid-template-columns: 1fr 1fr; gap:10px }
    .row-3{ display:grid; grid-template-columns: repeat(3, 1fr); gap:10px }
    @media (max-width:760px){ .row, .row-3{ grid-template-columns:1fr } }

    .actions{ display:flex; flex-wrap:wrap; gap:10px; margin-top:14px }
    button{
      border:0; padding:10px 14px; border-radius:12px; cursor:pointer; font-weight:600; color:#0b1022;
      background:linear-gradient(90deg,var(--brand),var(--brand-2));
      transition:transform .05s ease-in-out, filter .2s;
    }
    button:hover{ filter:brightness(1.05) }
    button:active{ transform:translateY(1px) }
    button.ghost{ background:#0b1224; color:#cbd5e1; border:1px solid #1f2a44 }
    button.warn{ background: linear-gradient(90deg,#f59e0b,#f43f5e); color:#0b1022 }

    .result h2{ margin-top:0 }
    .muted{ color:var(--muted) }
    .tag{ display:inline-block; font-size:.75rem; padding:.25rem .5rem; border:1px solid #1f2a44; border-radius:999px; color:#cbd5e1; margin-right:.4rem }

    .chapters{ margin-top:12px }
    .chapter{ margin:14px 0; padding:12px; background:#0b1224; border:1px solid #1f2a44; border-radius:12px }
    .chapter h3{ margin:.2rem 0 .4rem; color:#e2e8f0 }
    .chapter p{ color:#cbd5e1; line-height:1.6 }
    .poem-line{ display:block; margin:3px 0 }
    .scene{ margin:6px 0; padding-left:8px; border-left:3px solid #334155 }

    .history{ display:flex; flex-direction:column; gap:10px }
    .item{ padding:10px; border:1px dashed #1f2a44; border-radius:10px; cursor:pointer }
    .item:hover{ background:#0b1224 }

    .footer{ text-align:center; padding:20px 0; color:#94a3b8; font-size:.85rem }
  </style>
</head>
<body>
  <header>
    <div class="wrap">
      <h1>📚 Generador de Libros <span class="badge">por palabra o frase</span></h1>
    </div>
  </header>

  <main class="wrap" style="margin-top:14px">
    <div class="grid">
      <!-- Panel de controles -->
      <section class="card" aria-label="Controles de generación">
        <h2>Configura tu libro</h2>
        <label for="prompt">Palabra o frase base</label>
        <input id="prompt" type="text" placeholder="Ej: soledad, inteligencia artificial, café en la lluvia..." />

        <div class="row">
          <div>
            <label for="genre">Género</label>
            <select id="genre">
              <option>Fantasía</option>
              <option>Ciencia ficción</option>
              <option>Misterio</option>
              <option>Romance</option>
              <option>Terror</option>
              <option>Aventura</option>
              <option>Histórico</option>
              <option>Realismo mágico</option>
              <option>Infantil</option>
              <option>No ficción</option>
            </select>
          </div>
          <div>
            <label for="type">Tipo de libro</label>
            <select id="type">
              <option>Novela</option>
              <option>Cuento</option>
              <option>Poemario</option>
              <option>Ensayo</option>
              <option>Guion</option>
              <option>Fábula</option>
            </select>
          </div>
        </div>

        <div class="row-3">
          <div>
            <label for="length">Extensión estimada</label>
            <select id="length">
              <option value="micro">Micro</option>
              <option value="breve">Breve</option>
              <option value="media">Media</option>
              <option value="larga">Larga</option>
            </select>
          </div>
          <div>
            <label for="tone">Tono</label>
            <select id="tone">
              <option>Neutral</option>
              <option>Épico</option>
              <option>Lírico</option>
              <option>Humor</option>
              <option>Oscuro</option>
              <option>Inspirador</option>
            </select>
          </div>
          <div>
            <label for="audience">Audiencia</label>
            <select id="audience">
              <option>General</option>
              <option>Infantil</option>
              <option>Juvenil</option>
              <option>Adulta</option>
            </select>
          </div>
        </div>

        <label for="notes">Notas (opcional)</label>
        <textarea id="notes" placeholder="Personajes, lugares, estilo, época, moral, etc."></textarea>

        <div class="actions">
          <button id="btn-generate">Generar libro</button>
          <button id="btn-clear" class="ghost">Limpiar</button>
          <button id="btn-download" class="ghost">Descargar .txt</button>
          <button id="btn-print" class="ghost">Imprimir</button>
        </div>

        <p class="muted" style="margin-top:8px">Sugerencia: la misma entrada siempre generará un resultado similar gracias a una semilla interna.</p>
      </section>

      <!-- Resultado -->
      <section class="card result" aria-live="polite" id="result" style="display:none">
        <div id="meta"></div>
        <div id="content"></div>
      </section>
    </div>

    <section class="card" style="margin-top:16px">
      <h2>Historial (local)</h2>
      <p class="muted">Se guardan tus últimos 8 libros en este navegador.</p>
      <div id="history" class="history"></div>
    </section>

    <p class="footer">Hecho con HTML, CSS y JavaScript — todo corre en tu navegador ✨</p>
  </main>

  <script>
    // ========= Utilidades =========
    function hashString(str){
      // djb2 simple
      let h=5381; for(let i=0;i<str.length;i++){ h=((h<<5)+h)+str.charCodeAt(i); h|=0 } return h>>>0;
    }
    function mulberry32(a){
      return function(){
        let t = a += 0x6D2B79F5;
        t = Math.imul(t ^ t >>> 15, t | 1);
        t ^= t + Math.imul(t ^ t >>> 7, t | 61);
        return ((t ^ t >>> 14) >>> 0) / 4294967296;
      }
    }
    function pick(rand, arr){ return arr[Math.floor(rand()*arr.length)] }
    function titleCase(str){ return (str||'').trim().replace(/\s+/g,' ').replace(/(^|[\.!?"\(\[]\s*)([a-záéíóúñ])/g,(m,p1,p2)=>p1+p2.toUpperCase()).replace(/\b(\w)/g,(m)=>m.toUpperCase()) }
    function clamp(n,min,max){ return Math.max(min, Math.min(max,n)) }

    // ========= Banco de ideas =========
    const BANK = {
      motivos:{
        'Fantasía': ['reinos perdidos','magia antigua','profecías','dragones','portales','gremios de magos','amuletos'],
        'Ciencia ficción': ['colonias espaciales','IA sensible','viajes en el tiempo','biotecnología','primera toma de contacto','ciberpunk','terraformación'],
        'Misterio': ['pistas falsas','diarios ocultos','crimen sin resolver','detectives aficionados','pueblos con secretos','cartas anónimas'],
        'Romance': ['amores imposibles','segundas oportunidades','cartas de amor','encuentros fortuitos','amistad que florece','promesas'],
        'Terror': ['susurros en la noche','casas abandonadas','rituales','sombras al borde de la cama','leyendas urbanas','presencias'],
        'Aventura': ['mapas incompletos','ríos peligrosos','aliados inesperados','tesoros','desiertos interminables','cordilleras'],
        'Histórico': ['cartógrafos','revoluciones','expediciones','imperios','artesanos','crónicas','conquistadores'],
        'Realismo mágico': ['pueblos que sueñan','lluvias que recuerdan','animales que hablan','tiempo circular','milagros cotidianos'],
        'Infantil': ['amigos imaginarios','bosques risueños','nubes de algodón','castillos de arena','gatitos viajeros'],
        'No ficción': ['análisis de fuentes','teoría y práctica','casos reales','marcos conceptuales','síntesis y conclusiones']
      },
      tonos:{
        'Neutral':['con equilibrio y claridad'],
        'Épico':['con grandeza y destino inevitable'],
        'Lírico':['con metáforas y ritmo suave'],
        'Humor':['con guiños y travesuras'],
        'Oscuro':['con tensión y silencios'],
        'Inspirador':['con esperanza y propósito']
      },
      estructuras:{
        'Novela':['Apertura','Incidente','Viaje','Revelación','Clímax','Cierre'],
        'Cuento':['Situación','Quiebre','Confrontación','Resolución'],
        'Poemario':['Poema I','Poema II','Poema III','Poema IV','Poema V','Poema VI'],
        'Ensayo':['Introducción','Marco teórico','Desarrollo','Estudio de caso','Discusión','Conclusiones'],
        'Guion':['Acto I','Acto II','Acto III','Epílogo'],
        'Fábula':['Planteamiento','Peripecia','Moraleja']
      }
    }

    const LENGTH_TO_CHAPTERS = { micro:[1,3], breve:[3,6], media:[6,10], larga:[10,16] };

    // ========= Generación =========
    function generate(){
      const prompt = document.getElementById('prompt').value.trim();
      const genre = document.getElementById('genre').value;
      const type = document.getElementById('type').value;
      const length = document.getElementById('length').value; // micro/breve/media/larga
      const tone = document.getElementById('tone').value;
      const audience = document.getElementById('audience').value;
      const notes = document.getElementById('notes').value.trim();

      if(!prompt){ alert('Por favor, escribe una palabra o frase.'); return }

      // PRNG sembrado para resultados reproducibles por la misma entrada
      const seed = hashString([prompt,genre,type,length,tone,audience,notes].join('|'));
      const rand = mulberry32(seed);

      // Título & autor ficticio
      const base = prompt.replace(/(^|\s)\S/g, s => s.toUpperCase());
      const titleTemplates = [
        `El misterio de "${base}"`,
        `${titleCase(base)} y otros destinos`,
        `Bajo el signo de ${base.toLowerCase()}`,
        `${titleCase(base)}: crónicas ${genre.toLowerCase()}`,
        `La teoría de ${base.toLowerCase()}`
      ];
      const title = pick(rand, titleTemplates);
      const author = pick(rand, ['A. Valencia','M. Cuéllar','I. Prado','L. Salvatierra','R. Gaitán','S. C. Ramos','E. Velasco']);

      // Sinopsis
      const motivo = pick(rand, BANK.motivos[genre]);
      const tono = pick(rand, BANK.tonos[tone]);
      const hookTemplates = [
        `En un mundo donde "${prompt}" es la clave, un personaje descubre ${motivo}.`,
        `Cuando todo parece girar en torno a "${prompt}", una decisión cambia el curso de los acontecimientos.`,
        `Una señal relacionada con "${prompt}" despierta ${motivo} que nadie se atreve a nombrar.`,
      ];
      const hook = pick(rand, hookTemplates);
      const syn = `${hook} Esta obra de ${type.toLowerCase()} ${tono}, pensada para público ${audience.toLowerCase()}, explora ${motivo} desde la óptica del ${genre.toLowerCase()}. ${notes?('Notas del autor: '+notes):''}`.trim();

      // Número de capítulos / secciones
      const [minC,maxC] = LENGTH_TO_CHAPTERS[length];
      const count = clamp(Math.floor(rand()* (maxC-minC+1)) + minC, minC, maxC);

      // Fabricar nombres de capítulos/poemas/escenas
      function chapterName(i){
        const starts = ['Sombras de','Mapa de','Teoría de','Canción de','Memoria de','Rumbo a','Cartas sobre','Hipótesis de','Puertas a','Ruta de'];
        const ends = [base, motivo, prompt.toLowerCase(), pick(rand, BANK.motivos[genre])];
        const core = `${pick(rand, starts)} ${pick(rand, ends)}`;
        const numerales = ['I','II','III','IV','V','VI','VII','VIII','IX','X'];
        return `${core} ${numerales[i]||''}`.trim();
      }

      // Párrafo generador (prosa)
      function para(){
        const textures = [
          `El aire parecía sostener ${prompt}, como si cada esquina guardara ${motivo}.`,
          `Nadie entendía por qué ${prompt} seguía apareciendo en los registros, pero todos lo intuían.`,
          `Había señales: ${pick(rand,['luces distantes','huellas','susurros','códigos antiguos','mapas quemados'])} que apuntaban al mismo sitio.`,
          `A cada paso, ${pick(rand,['la ciudad','el bosque','el corredor de la nave','el puerto'])} ofrecía un indicio más.`
        ];
        return pick(rand, textures);
      }

      // Verso generador (poesía)
      function verse(){
        const v = [
          `${titleCase(prompt)} que no cesa`,
          `${pick(rand,['lluvia','polvo','luz','sombra','viento'])} en mis bolsillos`,
          `el mapa se pliega // ${motivo}`,
          `y en el borde del día // te nombro`,
          `${pick(rand,['regreso','parto','sueño','canto'])} con las manos vacías`
        ];
        return v.map(s=>`<span class="poem-line">${s}</span>`).join('');
      }

      // Escena (guion)
      function scene(ix){
        const loc = pick(rand,['INT. BIBLIOTECA','EXT. ESTACIÓN ESPACIAL','INT. CASA ANTIGUA','EXT. MERCADO COSTERO','INT. LABORATORIO']);
        const act = pick(rand,['NOCHE','AMANECER','LLUVIA','ATARDECER','TORMENTA']);
        const line = pick(rand,[
          `(${loc} - ${act}) Un personaje observa: "${prompt}" aparece escrito en una pared.`,
          `(${loc} - ${act}) Ruido de pasos. Un mapa marca ${motivo}.`,
          `(${loc} - ${act}) Una voz en off pregunta si todo empezó con "${prompt}".`
        ]);
        return `<div class="scene">${line}</div>`
      }

      // Construcción del contenido según tipo
      const chapters = [];
      for(let i=0;i<count;i++){
        const name = chapterName(i);
        let body = '';
        if(type==='Poemario'){
          body = verse();
        } else if(type==='Guion'){
          body = scene(i) + scene(i+1) + scene(i+2);
        } else if(type==='Ensayo' || type==='No ficción'){
          body = `<p>${para()}</p><p>${para()}</p><p>${pick(rand,[`Se examinan implicaciones de "${prompt}" en relación con ${motivo}.`,`Se comparan marcos teóricos frente a "${prompt}".`])}</p>`;
        } else if(type==='Fábula'){
          body = `<p>${para()}</p><p>${para()}</p><p><em>Moraleja:</em> ${pick(rand,[`No todo lo que nombra "${prompt}" es lo que parece.`,`La paciencia guía a quien comprende ${motivo}.`,`El valor pesa menos que el miedo cuando se comparte.`])}</p>`;
        } else { // Novela / Cuento
          const paras = ['micro','breve'].includes(length) ? 2 : (length==='media'? 3 : 4);
          body = Array.from({length:paras},()=>`<p>${para()}</p>`).join('');
        }
        chapters.push({ name, body });
      }

      const meta = {
        title,
        author,
        genre,
        type,
        length,
        tone,
        audience,
        seed,
        created: new Date().toLocaleString()
      };

      render(meta, chapters, syn);
      persist({ meta, chapters, synopsis: syn, prompt, notes });
      return { meta, chapters, synopsis: syn };
    }

    // ========= Render =========
    function render(meta, chapters, synopsis){
      const result = document.getElementById('result');
      const metaBox = document.getElementById('meta');
      const content = document.getElementById('content');
      result.style.display='block';

      metaBox.innerHTML = `
        <h2>${meta.title}</h2>
        <div class="muted" style="margin:6px 0 10px">
          <span class="tag">Autor: ${meta.author}</span>
          <span class="tag">Género: ${meta.genre}</span>
          <span class="tag">Tipo: ${meta.type}</span>
          <span class="tag">Extensión: ${meta.length}</span>
          <span class="tag">Tono: ${meta.tone}</span>
          <span class="tag">Audiencia: ${meta.audience}</span>
        </div>
        <p><strong>Sinopsis:</strong> ${synopsis}</p>
      `;

      content.innerHTML = `
        <div class="chapters">
          ${chapters.map((c,i)=>`<article class="chapter"><h3>${meta.type==='Poemario'?'Poema':'Capítulo'} ${i+1}: ${c.name}</h3>${c.body}</article>`).join('')}
        </div>
      `;

      // Desplazar
      setTimeout(()=>{ result.scrollIntoView({behavior:'smooth', block:'start'}) }, 50);
    }

    // ========= Descarga / Impresión =========
    function downloadTxt(){
      const meta = document.getElementById('meta').innerText || '';
      const content = document.getElementById('content').innerText || '';
      const blob = new Blob([meta + '\n\n' + content], {type:'text/plain;charset=utf-8'});
      const a = document.createElement('a');
      a.href = URL.createObjectURL(blob);
      a.download = 'libro-generado.txt';
      document.body.appendChild(a); a.click(); a.remove();
      URL.revokeObjectURL(a.href);
    }

    // ========= Historial (LocalStorage) =========
    const KEY = 'genlib_history_v1';
    function persist(entry){
      try{
        const list = JSON.parse(localStorage.getItem(KEY)||'[]');
        list.unshift(entry); if(list.length>8) list.pop();
        localStorage.setItem(KEY, JSON.stringify(list));
        paintHistory();
      }catch(e){ console.warn('No se pudo guardar historial', e) }
    }
    function paintHistory(){
      const box = document.getElementById('history');
      const list = JSON.parse(localStorage.getItem(KEY)||'[]');
      if(!list.length){ box.innerHTML = '<span class="muted">Aún no hay libros generados.</span>'; return }
      box.innerHTML = '';
      list.forEach((it, idx)=>{
        const div = document.createElement('div'); div.className='item';
        div.innerHTML = `<strong>${it.meta.title}</strong><br><span class="muted">${it.meta.genre} · ${it.meta.type} · ${new Date(it.meta.created).toLocaleString()}</span>`;
        div.addEventListener('click', ()=> render(it.meta, it.chapters, it.synopsis));
        box.appendChild(div);
      })
    }

    // ========= Eventos =========
    document.getElementById('btn-generate').addEventListener('click', generate);
    document.getElementById('btn-clear').addEventListener('click', ()=>{
      ['prompt','notes'].forEach(id=>document.getElementById(id).value='');
      document.getElementById('result').style.display='none';
    });
    document.getElementById('btn-download').addEventListener('click', downloadTxt);
    document.getElementById('btn-print').addEventListener('click', ()=>window.print());

    // Enter para generar
    document.getElementById('prompt').addEventListener('keydown', (e)=>{ if(e.key==='Enter'){ e.preventDefault(); generate() } });

    // Pintar historial al cargar
    paintHistory();
  </script>
</body>
</html>
