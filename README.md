# poke151
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-title" content="Pokédex Typing">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="theme-color" content="#A8232B">
<title>Pokédex Typing</title>
<style>
:root{
  --shell:#A8232B; --shell-dk:#6E1219; --lcd:#D3E0B8; --lcd-dk:#1F3320; --lcd-mid:#7E9770;
  --amber:#F2C230; --bad:#8C1C13;
  --mono: ui-monospace,"SF Mono",Menlo,Consolas,monospace;
}
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
html,body{height:100%;margin:0}
body{
  background:var(--shell-dk);color:var(--lcd-dk);font-family:var(--mono);
  padding:env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);
  overscroll-behavior:none;
}
.device{
  height:100%;max-width:520px;margin:0 auto;background:var(--shell);
  display:flex;flex-direction:column;gap:12px;padding:14px;
  border-left:3px solid var(--shell-dk);border-right:3px solid var(--shell-dk);
}
.top{display:flex;align-items:center;gap:10px;color:#fff}
.lens{width:34px;height:34px;border-radius:50%;background:radial-gradient(circle at 35% 35%,#9fe3ff,#1b78c9 60%,#0c3f78);border:3px solid #fff;flex:none}
.top h1{font-size:16px;margin:0;font-weight:700;letter-spacing:.02em;flex:1}
.stats{display:grid;grid-template-columns:repeat(3,1fr);gap:8px}
.stat{background:var(--shell-dk);color:#fff;border-radius:8px;padding:8px 10px}
.stat small{display:block;font-size:11px;opacity:.75}
.stat b{font-size:20px;font-variant-numeric:tabular-nums}
.screen{
  flex:1;min-height:0;background:var(--lcd);border:4px solid var(--shell-dk);border-radius:10px;
  padding:14px;display:flex;flex-direction:column;position:relative;overflow:hidden;
  box-shadow:inset 0 0 0 2px var(--lcd-mid);
}
.bar{height:8px;background:var(--lcd-mid);border-radius:4px;overflow:hidden}
.bar i{display:block;height:100%;width:0;background:var(--lcd-dk);transition:width .15s}
.prog{display:flex;justify-content:space-between;font-size:13px;margin:6px 0 0}
.target{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:6px}
.target .no{font-size:64px;font-weight:700;line-height:1;font-variant-numeric:tabular-nums}
.target .q{font-size:18px;letter-spacing:.3em;color:var(--lcd-mid)}
.pop{font-size:15px;height:20px;font-weight:700}
.recent{font-size:14px;line-height:1.5;min-height:84px;color:var(--lcd-dk)}
.recent div:nth-child(1){opacity:1}.recent div:nth-child(2){opacity:.65}.recent div:nth-child(3){opacity:.4}.recent div:nth-child(4){opacity:.2}
.typing{display:flex;gap:8px}
#inp{
  flex:1;min-width:0;font:700 20px var(--mono);padding:14px;border-radius:10px;border:3px solid var(--shell-dk);
  background:#fff;color:#111;outline:none;
}
#inp:focus{border-color:var(--amber)}
button{
  font:700 16px var(--mono);border:0;border-radius:10px;padding:14px 18px;background:var(--amber);color:#2a1d00;
  box-shadow:0 3px 0 #a07d0f;cursor:pointer;
}
button:active{transform:translateY(2px);box-shadow:0 1px 0 #a07d0f}
.overlay{position:absolute;inset:0;background:var(--lcd);padding:18px;display:none;flex-direction:column;gap:12px;overflow:auto}
.overlay.on{display:flex}
.overlay h2{margin:0;font-size:24px}
.overlay p{margin:0;font-size:15px;line-height:1.5}
.answer{background:var(--lcd-dk);color:var(--lcd);border-radius:8px;padding:12px}
.answer b{font-size:26px;display:block;margin-top:2px}
.rules li{margin-bottom:6px}
.rules{padding-left:18px;margin:0;font-size:14px;line-height:1.45}
.spacer{flex:1}
.shake{animation:sh .35s}
@keyframes sh{20%{transform:translateX(-6px)}40%{transform:translateX(6px)}60%{transform:translateX(-4px)}80%{transform:translateX(4px)}}
@media (prefers-reduced-motion:reduce){.shake{animation:none}}
</style>
</head>
<body>
<main class="device">
  <div class="top"><div class="lens"></div><h1>POKÉDEX TYPING · 151</h1></div>
  <div class="stats">
    <div class="stat"><small>Pontos</small><b id="score">0</b></div>
    <div class="stat"><small>Combo</small><b id="combo">x1.0</b></div>
    <div class="stat"><small>Tempo</small><b id="time">0:00</b></div>
  </div>

  <section class="screen" id="screen">
    <div>
      <div class="bar"><i id="barfill"></i></div>
      <div class="prog"><span id="count">0 / 151</span><span id="best">Recorde: 0</span></div>
    </div>
    <div class="target">
      <div class="q">DIGITE O POKÉMON</div>
      <div class="no" id="no">Nº 001</div>
      <div class="pop" id="pop"></div>
    </div>
    <div class="recent" id="recent"></div>

    <div class="overlay on" id="start">
      <h2>Treinador</h2>
      <p>Digite os 151 Pokémon da primeira geração na ordem da Pokédex, sem errar nenhum.</p>
      <ul class="rules">
        <li>Acertou o nome? Avança sozinho.</li>
        <li>Errou e apertou espaço ou Enter? Fim de jogo, e você vê qual era o próximo.</li>
        <li>Combo: cada acerto sem usar apagar aumenta o multiplicador (até x3). Apagar zera o combo.</li>
        <li>Bônus de velocidade: quanto mais rápido, mais pontos.</li>
        <li>Acentos e símbolos não importam. Para os Nidoran, digite “nidoranf” e “nidoranm”.</li>
      </ul>
      <div class="spacer"></div>
      <button id="go">Começar</button>
    </div>

    <div class="overlay" id="over">
      <h2 id="ovtitle">Fim de jogo</h2>
      <div class="answer" id="ovans"></div>
      <p id="ovsum"></p>
      <div class="spacer"></div>
      <button id="again">Jogar de novo</button>
    </div>
  </section>

  <div class="typing">
    <input id="inp" type="text" inputmode="text" enterkeyhint="go" autocomplete="off" autocorrect="off"
      autocapitalize="off" spellcheck="false" placeholder="nome do Pokémon" aria-label="Nome do Pokémon" disabled>
    <button id="ok" aria-label="Enviar">OK</button>
  </div>
</main>

<script>
const NAMES = ["Bulbasaur","Ivysaur","Venusaur","Charmander","Charmeleon","Charizard","Squirtle","Wartortle","Blastoise","Caterpie","Metapod","Butterfree","Weedle","Kakuna","Beedrill","Pidgey","Pidgeotto","Pidgeot","Rattata","Raticate","Spearow","Fearow","Ekans","Arbok","Pikachu","Raichu","Sandshrew","Sandslash","Nidoran♀","Nidorina","Nidoqueen","Nidoran♂","Nidorino","Nidoking","Clefairy","Clefable","Vulpix","Ninetales","Jigglypuff","Wigglytuff","Zubat","Golbat","Oddish","Gloom","Vileplume","Paras","Parasect","Venonat","Venomoth","Diglett","Dugtrio","Meowth","Persian","Psyduck","Golduck","Mankey","Primeape","Growlithe","Arcanine","Poliwag","Poliwhirl","Poliwrath","Abra","Kadabra","Alakazam","Machop","Machoke","Machamp","Bellsprout","Weepinbell","Victreebel","Tentacool","Tentacruel","Geodude","Graveler","Golem","Ponyta","Rapidash","Slowpoke","Slowbro","Magnemite","Magneton","Farfetch'd","Doduo","Dodrio","Seel","Dewgong","Grimer","Muk","Shellder","Cloyster","Gastly","Haunter","Gengar","Onix","Drowzee","Hypno","Krabby","Kingler","Voltorb","Electrode","Exeggcute","Exeggutor","Cubone","Marowak","Hitmonlee","Hitmonchan","Lickitung","Koffing","Weezing","Rhyhorn","Rhydon","Chansey","Tangela","Kangaskhan","Horsea","Seadra","Goldeen","Seaking","Staryu","Starmie","Mr. Mime","Scyther","Jynx","Electabuzz","Magmar","Pinsir","Tauros","Magikarp","Gyarados","Lapras","Ditto","Eevee","Vaporeon","Jolteon","Flareon","Porygon","Omanyte","Omastar","Kabuto","Kabutops","Aerodactyl","Snorlax","Articuno","Zapdos","Moltres","Dratini","Dragonair","Dragonite","Mewtwo","Mew"];
const TOTAL = NAMES.length;
const norm = s => s.toLowerCase().normalize("NFD").replace(/[\u0300-\u036f]/g,"")
  .replace(/♀/g,"f").replace(/♂/g,"m").replace(/[^a-z0-9]/g,"");
const KEYS = NAMES.map(norm);
const ALT = {28:["nidoranfemale","nidoranfemea"],31:["nidoranmale","nidoranmacho"]};
const matches = (i,v) => v===KEYS[i] || (ALT[i]||[]).includes(v);
const pad = n => String(n+1).padStart(3,"0");

const $ = id => document.getElementById(id);
const inp=$("inp"), el={score:$("score"),combo:$("combo"),time:$("time"),count:$("count"),best:$("best"),
  bar:$("barfill"),no:$("no"),pop:$("pop"),recent:$("recent"),start:$("start"),over:$("over"),screen:$("screen")};

let idx=0, score=0, combo=0, usedBackspace=false, startT=0, entryT=0, timer=null, running=false, recent=[];
let best=0;
try{ best = parseInt(localStorage.getItem("pokedex-typing-best")||"0",10)||0; }catch(e){}
el.best.textContent = "Recorde: "+best;

const mult = () => 1 + Math.min(combo,20)*0.1;
function fmt(ms){ const s=Math.floor(ms/1000); return Math.floor(s/60)+":"+String(s%60).padStart(2,"0"); }

function render(){
  el.score.textContent = score;
  el.combo.textContent = "x"+mult().toFixed(1);
  el.count.textContent = idx+" / "+TOTAL;
  el.bar.style.width = (idx/TOTAL*100)+"%";
  el.no.textContent = idx<TOTAL ? "Nº "+pad(idx) : "Nº ---";
  el.recent.innerHTML = recent.slice(-4).reverse().map(r=>"<div>✓ "+r+"</div>").join("");
}

function begin(){
  idx=0; score=0; combo=0; recent=[]; usedBackspace=false; running=true;
  startT=entryT=Date.now();
  el.start.classList.remove("on"); el.over.classList.remove("on");
  el.pop.textContent="";
  inp.disabled=false; inp.value=""; render();
  clearInterval(timer);
  timer=setInterval(()=>{ el.time.textContent = fmt(Date.now()-startT); },250);
  inp.focus();
}

function correct(){
  const ms = Date.now()-entryT;
  combo = usedBackspace ? 0 : combo+1;
  const speed = Math.max(0, Math.round(100 - ms/40));
  const gain = Math.round((100+speed)*mult());
  score += gain;
  recent.push(NAMES[idx]);
  el.pop.textContent = "+"+gain+(speed>0?"  (velocidade +"+speed+")":"");
  idx++; usedBackspace=false; entryT=Date.now(); inp.value="";
  render();
  if(idx>=TOTAL) finish(true);
}

function wrong(typed){
  finish(false, typed);
}

function finish(won, typed){
  running=false; clearInterval(timer);
  const total = Date.now()-startT;
  el.time.textContent = fmt(total);
  inp.disabled=true; inp.blur();
  let isBest=false;
  if(score>best){ best=score; isBest=true; try{localStorage.setItem("pokedex-typing-best",String(best));}catch(e){} }
  el.best.textContent="Recorde: "+best;
  $("ovtitle").textContent = won ? "Mestre Pokémon!" : "Fim de jogo";
  $("ovans").innerHTML = won
    ? "Você completou os 151 sem errar nenhum."
    : "O próximo era<b>#"+pad(idx)+" "+NAMES[idx]+"</b>"+(typed?"<small>Você digitou: “"+typed.replace(/</g,"&lt;")+"”</small>":"");
  $("ovsum").innerHTML = "Pontos: <b>"+score+"</b>"+(isBest?" (novo recorde!)":"")+"<br>Acertos: "+idx+" de "+TOTAL+"<br>Tempo: "+fmt(total);
  el.over.classList.add("on");
  if(!won){ el.screen.classList.remove("shake"); void el.screen.offsetWidth; el.screen.classList.add("shake"); }
}

function submit(){
  if(!running) return;
  const raw=inp.value, v=norm(raw);
  if(!v) return;
  if(matches(idx,v)) correct(); else wrong(raw.trim());
}

inp.addEventListener("input",()=>{
  if(!running) return;
  const raw=inp.value;
  const v=norm(raw);
  if(matches(idx,v)){ correct(); return; }
  if(/\s$/.test(raw)){
    // espaço envia, exceto em nomes com espaço (ex.: "Mr. Mime") enquanto o texto ainda é prefixo
    const name=NAMES[idx];
    if(/[ .]/.test(name) && v && KEYS[idx].startsWith(v)) return;
    if(!v){ inp.value=""; return; }
    wrong(raw.trim());
  }
});
inp.addEventListener("keydown",e=>{
  if(e.key==="Backspace") usedBackspace=true;
  if(e.key==="Enter"){ e.preventDefault(); submit(); }
});
$("ok").addEventListener("click",()=>{ running ? submit() : null; if(running) inp.focus(); });
$("go").addEventListener("click",begin);
$("again").addEventListener("click",begin);
render();
</script>
</body>
</html>
