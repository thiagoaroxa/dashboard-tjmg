<!DOCTYPE html>

<html lang="pt-BR">
<head>
  <meta charset="UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Dashboard de Produtividade — Gabinete TJMG</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: 'Segoe UI', 'Helvetica Neue', Arial, sans-serif; background: #F0F4F9; color: #1A2B45; min-height: 100vh; }
    ::-webkit-scrollbar { width: 5px; } ::-webkit-scrollbar-thumb { background: #3182CE55; border-radius: 4px; }

```
/* Header */
#header { background: linear-gradient(135deg, #0F2F55 0%, #2B6CB0 100%); padding: 0 32px; display: flex; align-items: center; gap: 16px; box-shadow: 0 2px 12px rgba(15,47,85,0.25); position: sticky; top: 0; z-index: 100; min-height: 64px; }
#header-title { color: #fff; font-weight: 700; font-size: 16px; letter-spacing: 0.2px; }
#header-sub { color: #BEE3F8; font-size: 11px; margin-top: 2px; letter-spacing: 0.4px; }
#header-divider { border-left: 1px solid rgba(255,255,255,0.25); padding-left: 16px; }
#header-tags { margin-left: auto; display: flex; gap: 8px; align-items: center; }
.htag { display: inline-flex; align-items: center; border-radius: 6px; padding: 4px 12px; font-size: 12px; font-weight: 600; }
#btn-reset { background: rgba(255,255,255,0.12); border: 1px solid rgba(255,255,255,0.3); color: #fff; border-radius: 7px; padding: 7px 16px; cursor: pointer; font-family: inherit; font-size: 12px; font-weight: 500; transition: background .2s; display: none; }
#btn-reset:hover { background: rgba(255,255,255,0.22); }

/* Upload */
#upload-screen { display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: calc(100vh - 64px); padding: 32px; gap: 24px; }
#upload-title { text-align: center; }
#upload-title h2 { font-size: 22px; font-weight: 700; color: #0F2F55; margin-top: 14px; }
#upload-title p { color: #718096; font-size: 14px; margin-top: 4px; }
#drop-zone { border: 2px dashed #CBD5E0; border-radius: 16px; padding: 56px 52px; text-align: center; cursor: pointer; transition: all .25s; max-width: 480px; width: 100%; background: #fff; }
#drop-zone:hover, #drop-zone.drag { border-color: #2B6CB0; background: #EBF8FF; }
#drop-zone .dz-icon { font-size: 44px; margin-bottom: 14px; }
#drop-zone h3 { font-size: 18px; font-weight: 700; color: #0F2F55; margin-bottom: 8px; }
#drop-zone p { color: #718096; font-size: 13.5px; margin-bottom: 22px; line-height: 1.6; }
#btn-upload { background: linear-gradient(135deg, #2B6CB0, #3182CE); color: #fff; border: none; border-radius: 8px; padding: 12px 30px; font-family: inherit; font-weight: 600; font-size: 14px; cursor: pointer; box-shadow: 0 4px 12px rgba(43,108,176,0.3); }
#file-input { display: none; }

/* Main */
#main-screen { display: none; padding: 24px 32px; max-width: 1400px; margin: 0 auto; }

/* Periodo */
#periodo { display: flex; gap: 8px; margin-bottom: 20px; flex-wrap: wrap; align-items: center; }
#periodo label { font-size: 12px; color: #718096; font-weight: 600; text-transform: uppercase; letter-spacing: 1px; margin-right: 4px; }
.mes-btn { background: transparent; border: 1px solid #CBD5E0; color: #718096; border-radius: 6px; padding: 5px 13px; cursor: pointer; font-family: inherit; font-size: 12px; font-weight: 500; transition: all .2s; }
.mes-btn:hover { border-color: #2B6CB0; color: #2B6CB0; background: #EBF4FF; }
.mes-btn.active { background: #2B6CB0; border-color: #2B6CB0; color: #fff; font-weight: 600; }

/* KPIs */
#kpis { display: grid; grid-template-columns: repeat(5,1fr); gap: 14px; margin-bottom: 22px; }
.kpi-card { background: #fff; border: 1px solid #E2E8F0; border-radius: 12px; box-shadow: 0 1px 4px rgba(0,0,0,0.07); padding: 20px; transition: all .2s; }
.kpi-card:hover { box-shadow: 0 6px 20px rgba(43,108,176,0.13); transform: translateY(-2px); }
.kpi-icon { font-size: 22px; margin-bottom: 8px; }
.kpi-value { font-size: 32px; font-weight: 800; letter-spacing: -1px; line-height: 1; }
.kpi-label { font-size: 12.5px; color: #4A5568; font-weight: 500; margin-top: 6px; }
.kpi-badge { display: inline-block; border-radius: 5px; padding: 2px 9px; font-size: 11px; font-weight: 700; float: right; margin-top: 2px; }

/* Tabs */
#tabs-wrap { background: #fff; border: 1px solid #E2E8F0; border-radius: 12px; box-shadow: 0 1px 4px rgba(0,0,0,0.07); margin-bottom: 22px; }
#tabs-header { display: flex; border-bottom: 1px solid #E2E8F0; padding: 0 8px; }
.tab-btn { background: transparent; border: none; border-bottom: 3px solid transparent; color: #718096; padding: 13px 22px; cursor: pointer; font-family: inherit; font-size: 13.5px; font-weight: 500; transition: all .2s; margin-bottom: -1px; }
.tab-btn:hover { color: #2B6CB0; }
.tab-btn.active { color: #2B6CB0; border-bottom-color: #2B6CB0; font-weight: 600; }
#tabs-content { padding: 22px 16px; }
.tab-pane { display: none; }
.tab-pane.active { display: block; }

/* Cards grid */
.grid-2 { display: grid; grid-template-columns: 2fr 1fr; gap: 18px; }
.grid-2-eq { display: grid; grid-template-columns: 1fr 1fr; gap: 18px; }
.grid-13 { display: grid; grid-template-columns: 1.3fr 1fr; gap: 18px; }
.col-full { grid-column: 1/-1; }
.inner-card { background: #fff; border: 1px solid #E2E8F0; border-radius: 12px; padding: 22px; margin-bottom: 0; }
.card-title { font-size: 14px; font-weight: 600; color: #0F2F55; margin-bottom: 16px; }

/* Charts */
.chart-wrap { position: relative; }

/* Ranking */
#ranking-list { display: flex; flex-direction: column; gap: 18px; }
.rank-item { }
.rank-top { display: flex; justify-content: space-between; align-items: center; }
.rank-left { display: flex; align-items: center; gap: 12px; }
.rank-pos { font-size: 18px; width: 28px; text-align: center; }
.rank-name { font-size: 14px; font-weight: 600; color: #1A2B45; }
.rank-sub { font-size: 11.5px; color: #718096; margin-top: 2px; }
.rank-num { font-size: 28px; font-weight: 800; letter-spacing: -1px; }
.rank-bar-wrap { height: 6px; background: #E2E8F0; border-radius: 10px; overflow: hidden; margin-top: 8px; }
.rank-bar-fill { height: 100%; border-radius: 10px; transition: width .7s; }

/* Tables */
.section-header { display: flex; align-items: center; gap: 10px; margin-bottom: 14px; }
.section-bar { width: 4px; height: 22px; border-radius: 2px; }
.section-title { font-size: 14px; font-weight: 700; color: #0F2F55; }
.section-badge { border-radius: 5px; padding: 2px 10px; font-size: 11px; font-weight: 600; }
.tbl-wrap { overflow-x: auto; margin-bottom: 8px; }
table { width: 100%; border-collapse: collapse; font-size: 13px; }
th { padding: 10px 14px; text-align: left; font-weight: 600; color: #0F2F55; font-size: 12px; white-space: nowrap; }
td { padding: 10px 14px; }
.td-mono { font-family: monospace; font-size: 11.5px; color: #4A5568; }
.status-badge { border-radius: 5px; padding: 3px 10px; font-size: 11.5px; font-weight: 600; white-space: nowrap; }
tr.urgent-row { background: #FFF8F8 !important; }

/* Donut center */
.donut-center { text-align: center; margin-top: -12px; }
.donut-pct { font-size: 36px; font-weight: 800; letter-spacing: -1px; }
.donut-sub { font-size: 12px; color: #718096; }

/* Footer */
#footer { text-align: center; padding: 14px 0 22px; color: #718096; font-size: 11.5px; display: flex; align-items: center; justify-content: center; gap: 10px; }

@keyframes fadeUp { from{opacity:0;transform:translateY(12px)} to{opacity:1;transform:translateY(0)} }
.fade-up { animation: fadeUp .4s ease forwards; }
```

  </style>
</head>
<body>

<!-- HEADER -->

<div id="header">
  <svg width="40" height="40" viewBox="0 0 100 100" fill="none">
    <rect width="100" height="100" rx="12" fill="#2B6CB0"/>
    <polygon points="50,10 88,75 12,75" stroke="white" stroke-width="5" fill="none"/>
    <polygon points="50,22 76,68 24,68" stroke="white" stroke-width="3" fill="none"/>
    <path d="M18,78 Q50,88 82,78" stroke="white" stroke-width="3" fill="none" stroke-linecap="round"/>
    <text x="50" y="96" text-anchor="middle" fill="white" font-size="18" font-weight="bold" font-family="Arial">TJMG</text>
  </svg>
  <div id="header-divider">
    <div id="header-title">Tribunal de Justiça de Minas Gerais</div>
    <div id="header-sub">GABINETE · Dashboard de Produtividade</div>
  </div>
  <div id="header-tags">
    <span id="tag-minutas" class="htag" style="display:none;background:rgba(255,255,255,0.15);color:#fff;border:1px solid rgba(255,255,255,0.25)"></span>
    <span id="tag-meses" class="htag" style="display:none;background:rgba(255,255,255,0.10);color:#BEE3F8;border:1px solid rgba(255,255,255,0.18)"></span>
    <button id="btn-reset" onclick="resetApp()">↑ Novo arquivo</button>
  </div>
</div>

<!-- UPLOAD -->

<div id="upload-screen">
  <div id="upload-title">
    <svg width="72" height="72" viewBox="0 0 100 100" fill="none" style="display:block;margin:0 auto">
      <rect width="100" height="100" rx="12" fill="#2B6CB0"/>
      <polygon points="50,10 88,75 12,75" stroke="white" stroke-width="5" fill="none"/>
      <polygon points="50,22 76,68 24,68" stroke="white" stroke-width="3" fill="none"/>
      <path d="M18,78 Q50,88 82,78" stroke="white" stroke-width="3" fill="none" stroke-linecap="round"/>
      <text x="50" y="96" text-anchor="middle" fill="white" font-size="18" font-weight="bold" font-family="Arial">TJMG</text>
    </svg>
    <h2>Controle de Produtividade</h2>
    <p>Gabinete do Magistrado</p>
  </div>
  <div id="drop-zone" onclick="document.getElementById('file-input').click()"
       ondragover="event.preventDefault();this.classList.add('drag')"
       ondragleave="this.classList.remove('drag')"
       ondrop="handleDrop(event)">
    <div class="dz-icon">📂</div>
    <h3>Carregar Planilha de Minutas</h3>
    <p>Arraste o arquivo .xlsx aqui<br>ou clique para selecionar</p>
    <button id="btn-upload">Selecionar Arquivo</button>
    <input type="file" id="file-input" accept=".xlsx,.xls" onchange="handleFile(this.files[0])"/>
  </div>
</div>

<!-- MAIN -->

<div id="main-screen">
  <!-- Período -->
  <div id="periodo"><label>Período:</label></div>

  <!-- KPIs -->

  <div id="kpis"></div>

  <!-- Tabs -->

  <div id="tabs-wrap">
    <div id="tabs-header">
      <button class="tab-btn active" onclick="switchTab('visao',this)">📈 Visão Geral</button>
      <button class="tab-btn" onclick="switchTab('ranking',this)">🏆 Ranking da Equipe</button>
      <button class="tab-btn" onclick="switchTab('tipos',this)">📂 Tipos & Áreas</button>
      <button class="tab-btn" onclick="switchTab('prioridades',this)">⚠️ Prioridades</button>
    </div>
    <div id="tabs-content">

```
  <!-- Visão Geral -->
  <div id="tab-visao" class="tab-pane active">
    <div class="grid-2">
      <div class="inner-card">
        <div class="card-title">Minutas por Mês</div>
        <div class="chart-wrap"><canvas id="chart-mes" height="200"></canvas></div>
      </div>
      <div class="inner-card" style="display:flex;flex-direction:column;align-items:center">
        <div class="card-title" style="align-self:flex-start">Taxa de Revisão</div>
        <canvas id="chart-donut" height="180" style="max-width:220px"></canvas>
        <div class="donut-center">
          <div class="donut-pct" id="donut-pct" style="color:#2C7A7B"></div>
          <div class="donut-sub" id="donut-sub"></div>
        </div>
      </div>
      <div class="inner-card col-full" id="dia-card">
        <div class="card-title">Produtividade Diária — <span id="dia-label" style="color:#3182CE;font-weight:400"></span></div>
        <div class="chart-wrap"><canvas id="chart-dia" height="130"></canvas></div>
      </div>
    </div>
  </div>

  <!-- Ranking -->
  <div id="tab-ranking" class="tab-pane">
    <div class="grid-13">
      <div class="inner-card">
        <div class="card-title">Ranking da Equipe — <span id="ranking-label" style="color:#3182CE;font-weight:400"></span></div>
        <div id="ranking-list"></div>
      </div>
      <div class="inner-card">
        <div class="card-title">Volume por Membro</div>
        <div class="chart-wrap"><canvas id="chart-ranking" height="300"></canvas></div>
      </div>
    </div>
  </div>

  <!-- Tipos & Áreas -->
  <div id="tab-tipos" class="tab-pane">
    <div class="grid-2-eq">
      <div class="inner-card">
        <div class="card-title">Distribuição por Tipo de Minuta</div>
        <div class="chart-wrap"><canvas id="chart-tipo" height="220"></canvas></div>
      </div>
      <div class="inner-card">
        <div class="card-title">Por Área da Justiça</div>
        <div class="chart-wrap"><canvas id="chart-justica" height="220"></canvas></div>
      </div>
      <div class="inner-card col-full">
        <div class="card-title">Total vs. Revisadas por Membro</div>
        <div class="chart-wrap"><canvas id="chart-membros" height="180"></canvas></div>
      </div>
    </div>
  </div>

  <!-- Prioridades -->
  <div id="tab-prioridades" class="tab-pane">
    <div style="margin-bottom:28px">
      <div class="section-header">
        <div class="section-bar" style="background:#2B6CB0"></div>
        <div class="section-title">Pedidos OAB</div>
        <span id="oab-badge" class="section-badge" style="background:#EBF4FF;color:#2B6CB0;border:1px solid #63B3ED"></span>
      </div>
      <div class="tbl-wrap">
        <table id="tbl-oab">
          <thead>
            <tr style="background:#EBF4FF">
              <th style="border-bottom:2px solid #2B6CB0">Advogado</th>
              <th style="border-bottom:2px solid #2B6CB0">Processo</th>
              <th style="border-bottom:2px solid #2B6CB0">Área</th>
              <th style="border-bottom:2px solid #2B6CB0">Assunto</th>
              <th style="border-bottom:2px solid #2B6CB0">Status</th>
              <th style="border-bottom:2px solid #2B6CB0">Providência</th>
              <th style="border-bottom:2px solid #2B6CB0">Próxima Providência</th>
            </tr>
          </thead>
          <tbody id="tbl-oab-body"></tbody>
        </table>
      </div>
    </div>
    <div>
      <div class="section-header">
        <div class="section-bar" style="background:#C53030"></div>
        <div class="section-title">Prioritários — Minutar</div>
        <span id="prior-badge" class="section-badge" style="background:#FFF5F5;color:#C53030;border:1px solid #FEB2B2"></span>
      </div>
      <div class="tbl-wrap">
        <table id="tbl-prior">
          <thead>
            <tr style="background:#FFF5F5">
              <th style="border-bottom:2px solid #C53030">Magistrado</th>
              <th style="border-bottom:2px solid #C53030">Responsável</th>
              <th style="border-bottom:2px solid #C53030">Processo</th>
              <th style="border-bottom:2px solid #C53030">Incluído em</th>
              <th style="border-bottom:2px solid #C53030">Prazo</th>
              <th style="border-bottom:2px solid #C53030">Sistema</th>
              <th style="border-bottom:2px solid #C53030">Área</th>
              <th style="border-bottom:2px solid #C53030">Status</th>
              <th style="border-bottom:2px solid #C53030">Observações</th>
            </tr>
          </thead>
          <tbody id="tbl-prior-body"></tbody>
        </table>
      </div>
    </div>
  </div>

</div>
```

  </div>

  <!-- Footer -->

  <div id="footer">
    <svg width="20" height="20" viewBox="0 0 100 100" fill="none">
      <rect width="100" height="100" rx="12" fill="#2B6CB0"/>
      <polygon points="50,10 88,75 12,75" stroke="white" stroke-width="5" fill="none"/>
      <polygon points="50,22 76,68 24,68" stroke="white" stroke-width="3" fill="none"/>
      <path d="M18,78 Q50,88 82,78" stroke="white" stroke-width="3" fill="none" stroke-linecap="round"/>
    </svg>
    Tribunal de Justiça de Minas Gerais · Sistema de Controle de Minutas do Gabinete
  </div>
</div>

<script>
// ── Estado global ─────────────────────────────────────────────────────────────
let allData = [], oabData = [], priorData = [], meses = [], activeMes = null;
let charts = {};

const CC = {
  tjBlue:'#2B6CB0', accent:'#63B3ED', gold:'#D4A017', teal:'#2C7A7B',
  violet:'#805AD5', orange:'#DD6B20', green:'#2F855A',
  border:'#E2E8F0', muted:'#718096', subtle:'#4A5568',
  rose:'#C53030', roseLight:'#FEB2B2', success:'#276749',
  goldLight:'#F6D860', tjBlueDark:'#1A4A80'
};
const CHART_COLORS = [CC.tjBlue, CC.accent, CC.gold, CC.teal, CC.violet, CC.orange, CC.green];

// ── Helpers ───────────────────────────────────────────────────────────────────
function fmtDate(v) {
  if (!v) return '—';
  if (v instanceof Date) return v.toLocaleDateString('pt-BR');
  if (typeof v === 'number') return new Date(Math.round((v-25569)*86400*1000)).toLocaleDateString('pt-BR');
  return String(v);
}
function groupBy(arr, key) {
  return arr.reduce((a,i)=>{ const k=i[key]||'—'; a[k]=(a[k]||0)+1; return a; }, {});
}
function normalize(n) { return n.replace(/\./g,'').replace(/\s+/g,' ').trim(); }
function destroyChart(id) { if(charts[id]) { charts[id].destroy(); delete charts[id]; } }

// ── Parsers ───────────────────────────────────────────────────────────────────
const SKIP = ['PEDIDOS OAB','PRIORITÁRIOS - MINUTAR'];

function parseMinutas(wb) {
  const rows = [];
  for (const name of wb.SheetNames) {
    if (SKIP.includes(name)) continue;
    const ws = wb.Sheets[name];
    const raw = XLSX.utils.sheet_to_json(ws, {header:1, defval:null});
    if (!raw.length) continue;
    let hIdx=-1, cm={};
    for (let i=0; i<Math.min(raw.length,10); i++) {
      const r = raw[i];
      const ri = r.findIndex(c=>typeof c==='string'&&c.toLowerCase().includes('responsáv'));
      if (ri!==-1) {
        hIdx=i;
        r.forEach((cell,idx)=>{
          if (!cell) return;
          const k=String(cell).toLowerCase().trim();
          if (k.includes('responsáv')||k.includes('responsav')) cm.resp=idx;
          else if (k==='data'&&cm.data===undefined) cm.data=idx;
          else if (k.includes('tipo de minuta')||k.includes('tipo de documento')) cm.tipo=idx;
          else if (k.includes('revisado')) cm.rev=idx;
          else if (k.includes('uso de ia')||k.includes('uso ia')) cm.ia=idx;
          else if (k.includes('justiça')||k.includes('justica')) cm.jus=idx;
        });
        break;
      }
    }
    if (hIdx===-1||cm.resp===undefined) continue;
    for (let i=hIdx+1; i<raw.length; i++) {
      const r=raw[i];
      if (!r||r.every(c=>c===null)) continue;
      const resp=r[cm.resp];
      if (!resp||typeof resp!=='string'||resp.trim()===''||resp.toLowerCase().includes('responsáv')||resp.toLowerCase()==='arquivo') continue;
      let dd=null;
      const dv=cm.data!==undefined?r[cm.data]:null;
      if (dv instanceof Date) dd=dv;
      else if (typeof dv==='number') dd=new Date(Math.round((dv-25569)*86400*1000));
      rows.push({ mes:name, responsavel:resp.trim(), tipo:(cm.tipo!==undefined?r[cm.tipo]:null)||'—',
        data:dd, revisado:cm.rev!==undefined?r[cm.rev]:null, ia:cm.ia!==undefined?r[cm.ia]:null,
        justica:cm.jus!==undefined?r[cm.jus]:null });
    }
  }
  return rows;
}

function parseOAB(wb) {
  const ws=wb.Sheets['PEDIDOS OAB']; if(!ws) return [];
  const raw=XLSX.utils.sheet_to_json(ws,{header:1,defval:null});
  const rows=[]; let lastAdv=null;
  for (let i=1;i<raw.length;i++) {
    const r=raw[i]; if(!r||r.every(c=>c===null)) continue;
    if (r[0]) lastAdv=String(r[0]).trim();
    if (!r[1]) continue;
    rows.push({ advogado:lastAdv||'—', processo:String(r[1]||'').trim(), justica:String(r[2]||'').trim(),
      assunto:String(r[3]||'').trim(), status:String(r[4]||'').trim(),
      prov:String(r[5]||'').trim(), prox:r[6]?String(r[6]).trim():null });
  }
  return rows;
}

function parsePrior(wb) {
  const ws=wb.Sheets['PRIORITÁRIOS - MINUTAR']; if(!ws) return [];
  const raw=XLSX.utils.sheet_to_json(ws,{header:1,defval:null});
  const rows=[];
  for (let i=1;i<raw.length;i++) {
    const r=raw[i]; if(!r||r.every(c=>c===null)||!r[2]) continue;
    rows.push({ magistrado:String(r[0]||'').trim(), responsavel:String(r[1]||'').trim(),
      processo:String(r[2]||'').trim(), incluido:fmtDate(r[3]), prazo:fmtDate(r[4]),
      sistema:String(r[5]||'').trim()||'—', justica:String(r[6]||'').trim()||'—',
      status:String(r[7]||'').trim()||'Pendente', obs:String(r[8]||'').trim() });
  }
  return rows;
}

// ── File handling ─────────────────────────────────────────────────────────────
function handleDrop(e) {
  e.preventDefault(); document.getElementById('drop-zone').classList.remove('drag');
  handleFile(e.dataTransfer.files[0]);
}
function handleFile(file) {
  if (!file) return;
  const reader=new FileReader();
  reader.onload=e=>{
    try {
      const wb=XLSX.read(e.target.result,{type:'array',cellDates:true});
      allData=parseMinutas(wb);
      if (!allData.length) { alert('Nenhum dado encontrado.'); return; }
      oabData=parseOAB(wb);
      priorData=parsePrior(wb);
      meses=[...new Set(allData.map(r=>r.mes))].filter(m=>allData.some(r=>r.mes===m));
      activeMes=null;
      showMain();
    } catch(err) { alert('Erro: '+err.message); }
  };
  reader.readAsArrayBuffer(file);
}

// ── Show/hide ─────────────────────────────────────────────────────────────────
function showMain() {
  document.getElementById('upload-screen').style.display='none';
  document.getElementById('main-screen').style.display='block';
  document.getElementById('main-screen').classList.add('fade-up');
  document.getElementById('btn-reset').style.display='block';
  buildPeriodo(); render();
}
function resetApp() {
  allData=[]; oabData=[]; priorData=[]; meses=[]; activeMes=null;
  Object.keys(charts).forEach(k=>{ charts[k].destroy(); delete charts[k]; });
  document.getElementById('main-screen').style.display='none';
  document.getElementById('upload-screen').style.display='flex';
  document.getElementById('btn-reset').style.display='none';
  document.getElementById('tag-minutas').style.display='none';
  document.getElementById('tag-meses').style.display='none';
}

// ── Período buttons ───────────────────────────────────────────────────────────
function buildPeriodo() {
  const p=document.getElementById('periodo');
  p.innerHTML='<label>Período:</label>';
  const btn=document.createElement('button');
  btn.className='mes-btn active'; btn.textContent='Todos'; btn.onclick=()=>setMes(null,btn);
  p.appendChild(btn);
  meses.forEach(m=>{
    const b=document.createElement('button');
    b.className='mes-btn'; b.textContent=m; b.onclick=()=>setMes(m,b);
    p.appendChild(b);
  });
}
function setMes(m, btn) {
  activeMes=m;
  document.querySelectorAll('.mes-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  render();
}

// ── Tab switching ─────────────────────────────────────────────────────────────
function switchTab(id, btn) {
  document.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('active'));
  document.querySelectorAll('.tab-pane').forEach(p=>p.classList.remove('active'));
  btn.classList.add('active');
  document.getElementById('tab-'+id).classList.add('active');
}

// ── Render ────────────────────────────────────────────────────────────────────
function render() {
  const fd = activeMes ? allData.filter(r=>r.mes===activeMes) : allData;
  const total=fd.length, rev=fd.filter(r=>r.revisado==='Sim').length,
        ia=fd.filter(r=>r.ia==='Sim').length, pend=total-rev,
        txRev=total?Math.round(rev/total*100):0, txIA=total?Math.round(ia/total*100):0;

  // Header tags
  document.getElementById('tag-minutas').style.display='inline-flex';
  document.getElementById('tag-minutas').textContent='📋 '+allData.length.toLocaleString()+' minutas';
  document.getElementById('tag-meses').style.display='inline-flex';
  document.getElementById('tag-meses').textContent='📅 '+meses.length+' meses';

  // KPIs
  const kpiDefs=[
    {label:'Total de Minutas',val:total,icon:'📋',color:CC.tjBlue,bg:'#EBF4FF',border:'#3182CE'},
    {label:'Revisadas',val:rev,icon:'✅',color:CC.success,bg:'#F0FFF4',border:'#9AE6B4'},
    {label:'Taxa de Revisão',val:txRev+'%',icon:'📊',color:'#3182CE',bg:'#EBF4FF',border:CC.accent},
    {label:'Uso de IA',val:txIA+'%',icon:'🤖',color:CC.violet,bg:'#FAF5FF',border:'#D6BCFA'},
    {label:'Não Revisadas',val:pend,icon:'⏳',color:CC.rose,bg:'#FFF5F5',border:CC.roseLight},
  ];
  document.getElementById('kpis').innerHTML=kpiDefs.map(k=>`
    <div class="kpi-card" style="border-top:3px solid ${k.color}">
      <div class="kpi-icon">${k.icon}
        <span class="kpi-badge" style="background:${k.bg};color:${k.color};border:1px solid ${k.border}">${k.val}</span>
      </div>
      <div class="kpi-value" style="color:${k.color}">${k.val}</div>
      <div class="kpi-label">${k.label}</div>
    </div>`).join('');

  // Ranking
  const rankingRaw=Object.entries(groupBy(fd,'responsavel'))
    .map(([nome,tot])=>{ const rows=fd.filter(r=>r.responsavel===nome); const r2=rows.filter(r=>r.revisado==='Sim').length; return {nome:normalize(nome),total:tot,revisadas:r2,taxa:Math.round(r2/tot*100)}; })
    .sort((a,b)=>b.total-a.total);

  document.getElementById('ranking-label').textContent=activeMes||'Todos os Meses';
  const MEDAL=['🥇','🥈','🥉'];
  const rankColors=['linear-gradient(90deg,#D4A017,#F6D860)','linear-gradient(90deg,#2B6CB0,#63B3ED)','linear-gradient(90deg,#C05621,#ED8936)'];
  document.getElementById('ranking-list').innerHTML=rankingRaw.map((r,i)=>`
    <div class="rank-item">
      <div class="rank-top">
        <div class="rank-left">
          <span class="rank-pos">${i<3?MEDAL[i]:'<span style="color:#718096;font-size:12px;font-weight:700">#'+(i+1)+'</span>'}</span>
          <div>
            <div class="rank-name">${r.nome}</div>
            <div class="rank-sub">${r.revisadas}/${r.total} revisadas · ${r.taxa}%</div>
          </div>
        </div>
        <div class="rank-num" style="color:${i===0?CC.gold:i===1?'#718096':i===2?'#C05621':CC.border}">${r.total}</div>
      </div>
      <div class="rank-bar-wrap"><div class="rank-bar-fill" style="width:${(r.total/rankingRaw[0].total*100).toFixed(1)}%;background:${i<3?rankColors[i]:CC.border}"></div></div>
    </div>`).join('');

  // By mes
  const byMes=meses.map(m=>({mes:m.slice(0,3),total:allData.filter(r=>r.mes===m).length}));

  // By dia
  const byDia={};
  fd.forEach(r=>{ if(r.data&&!isNaN(r.data)){ const k=r.data.getDate(); byDia[k]=(byDia[k]||0)+1; }});
  const diaData=Object.entries(byDia).sort((a,b)=>+a[0]-+b[0]).map(([d,t])=>({dia:d,total:t}));
  document.getElementById('dia-card').style.display=diaData.length?'block':'none';
  document.getElementById('dia-label').textContent=activeMes||'todos os meses';

  // By tipo & justica
  const byTipo=Object.entries(groupBy(fd,'tipo')).map(([n,v])=>({name:n,value:v})).sort((a,b)=>b.value-a.value);
  const byJus=Object.entries(groupBy(fd,'justica')).map(([n,v])=>({name:n,value:v})).sort((a,b)=>b.value-a.value);

  // Charts
  renderAreaChart('chart-mes', byMes.map(d=>d.mes), byMes.map(d=>d.total), 'Minutas', CC.tjBlue);
  renderDonut('chart-donut', ['Revisadas','Pendentes'], [rev,pend], [CC.teal,'#E2E8F0']);
  document.getElementById('donut-pct').textContent=txRev+'%';
  document.getElementById('donut-sub').textContent=rev+' de '+total+' minutas revisadas';
  if (diaData.length) renderAreaChart('chart-dia', diaData.map(d=>d.dia), diaData.map(d=>d.total), 'Minutas', '#3182CE');
  renderPie('chart-tipo', byTipo.map(d=>d.name), byTipo.map(d=>d.value));
  renderHBar('chart-justica', byJus.map(d=>d.name), byJus.map(d=>d.value));
  renderGroupedBar('chart-ranking', rankingRaw.map(r=>r.nome), rankingRaw.map(r=>r.total), rankingRaw.map(r=>r.revisadas));
  renderGroupedBar('chart-membros', rankingRaw.map(r=>r.nome), rankingRaw.map(r=>r.total), rankingRaw.map(r=>r.revisadas));

  // OAB table
  document.getElementById('oab-badge').textContent=oabData.length+' processos';
  const oabBody=document.getElementById('tbl-oab-body');
  oabBody.innerHTML=oabData.map((row,i)=>{
    const sc=row.status==='Assinado'?{bg:'#F0FFF4',color:'#276749',border:'#9AE6B4'}
      :row.status==='Minutar'?{bg:'#EBF4FF',color:CC.tjBlue,border:CC.accent}
      :{bg:'#FFFFF0',color:'#744210',border:'#F6E05E'};
    return `<tr style="border-bottom:1px solid #E2E8F0;background:${i%2===0?'#fff':'#FAFBFD'}">
      <td style="font-weight:600;color:#0F2F55;font-size:12.5px">${row.advogado}</td>
      <td class="td-mono">${row.processo}</td>
      <td>${row.justica}</td>
      <td>${row.assunto}</td>
      <td><span class="status-badge" style="background:${sc.bg};color:${sc.color};border:1px solid ${sc.border}">${row.status}</span></td>
      <td style="color:#4A5568;font-size:12px">${row.prov}</td>
      <td style="color:${row.prox?CC.tjBlue:'#718096'};font-weight:${row.prox?500:400};font-size:12px">${row.prox||'—'}</td>
    </tr>`;
  }).join('');

  // Prior table
  document.getElementById('prior-badge').textContent=priorData.length+' processos';
  const priorBody=document.getElementById('tbl-prior-body');
  priorBody.innerHTML=priorData.map((row,i)=>{
    const sc=row.status==='Concluído'?{bg:'#F0FFF4',color:'#276749',border:'#9AE6B4'}
      :row.status==='Urgente'?{bg:'#FFF5F5',color:CC.rose,border:CC.roseLight}
      :{bg:'#FFFFF0',color:'#744210',border:'#F6E05E'};
    const isUrg=row.status==='Urgente';
    return `<tr style="border-bottom:1px solid #E2E8F0;background:${isUrg?'#FFF8F8':i%2===0?'#fff':'#FAFBFD'}" ${isUrg?'class="urgent-row"':''}>
      <td style="font-weight:600;color:#0F2F55;font-size:12px">${row.magistrado||'—'}</td>
      <td style="font-size:12px">${row.responsavel||'—'}</td>
      <td class="td-mono" style="font-size:11px">${row.processo}</td>
      <td style="color:#718096;font-size:12px">${row.incluido}</td>
      <td style="font-weight:600;color:${isUrg?CC.rose:'#4A5568'};font-size:12px">${row.prazo}</td>
      <td style="font-size:12px">${row.sistema}</td>
      <td style="font-size:12px">${row.justica}</td>
      <td><span class="status-badge" style="background:${sc.bg};color:${sc.color};border:1px solid ${sc.border}">${row.status}</span></td>
      <td style="color:#4A5568;font-size:12px;max-width:260px">${row.obs||'—'}</td>
    </tr>`;
  }).join('');
}

// ── Chart helpers ─────────────────────────────────────────────────────────────
function renderAreaChart(id, labels, data, label, color) {
  destroyChart(id);
  const ctx=document.getElementById(id).getContext('2d');
  charts[id]=new Chart(ctx,{type:'line',data:{labels,datasets:[{label,data,borderColor:color,borderWidth:2.5,
    backgroundColor:color+'33',fill:true,tension:0.4,pointRadius:4,pointBackgroundColor:color}]},
    options:{responsive:true,plugins:{legend:{display:false},tooltip:{backgroundColor:'#0F2F55',titleColor:'#BEE3F8',bodyColor:'#fff',borderColor:'#3182CE',borderWidth:1}},
    scales:{x:{grid:{color:'#E2E8F0'},ticks:{color:'#718096',font:{size:11}}},y:{grid:{color:'#E2E8F0'},ticks:{color:'#718096',font:{size:11}}}}}});
}

function renderDonut(id, labels, data, colors) {
  destroyChart(id);
  const ctx=document.getElementById(id).getContext('2d');
  charts[id]=new Chart(ctx,{type:'doughnut',data:{labels,datasets:[{data,backgroundColor:colors,borderWidth:0,hoverOffset:4}]},
    options:{responsive:true,cutout:'65%',plugins:{legend:{display:false},tooltip:{backgroundColor:'#0F2F55',titleColor:'#BEE3F8',bodyColor:'#fff'}}}});
}

function renderPie(id, labels, data) {
  destroyChart(id);
  const ctx=document.getElementById(id).getContext('2d');
  charts[id]=new Chart(ctx,{type:'doughnut',data:{labels,datasets:[{data,backgroundColor:CHART_COLORS,borderWidth:2,borderColor:'#fff'}]},
    options:{responsive:true,cutout:'30%',plugins:{legend:{position:'bottom',labels:{font:{size:11},color:'#4A5568',padding:10}},
    tooltip:{backgroundColor:'#0F2F55',titleColor:'#BEE3F8',bodyColor:'#fff'}}}});
}

function renderHBar(id, labels, data) {
  destroyChart(id);
  const ctx=document.getElementById(id).getContext('2d');
  charts[id]=new Chart(ctx,{type:'bar',data:{labels,datasets:[{data,backgroundColor:CHART_COLORS,borderRadius:5}]},
    options:{indexAxis:'y',responsive:true,plugins:{legend:{display:false},tooltip:{backgroundColor:'#0F2F55',titleColor:'#BEE3F8',bodyColor:'#fff'}},
    scales:{x:{grid:{color:'#E2E8F0'},ticks:{color:'#718096',font:{size:10}}},y:{grid:{display:false},ticks:{color:'#4A5568',font:{size:10}}}}}});
}

function renderGroupedBar(id, labels, total, revisadas) {
  destroyChart(id);
  const ctx=document.getElementById(id).getContext('2d');
  charts[id]=new Chart(ctx,{type:'bar',data:{labels,datasets:[
    {label:'Total',data:total,backgroundColor:CC.tjBlue+'CC',borderRadius:5},
    {label:'Revisadas',data:revisadas,backgroundColor:CC.teal+'CC',borderRadius:5}]},
    options:{responsive:true,plugins:{legend:{labels:{color:'#4A5568',font:{size:11}}},tooltip:{backgroundColor:'#0F2F55',titleColor:'#BEE3F8',bodyColor:'#fff'}},
    scales:{x:{grid:{color:'#E2E8F0'},ticks:{color:'#718096',font:{size:11}}},y:{grid:{color:'#E2E8F0'},ticks:{color:'#718096',font:{size:11}}}}}});
}
</script>

</body>
</html>
