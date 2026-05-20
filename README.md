<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Movimentações de Colaboradores</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@latest/tabler-icons.min.css">
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    :root {
      --bg: #ffffff; --bg2: #f7f7f5; --bg3: #f0efe8;
      --bg-info: #e6f1fb; --bg-success: #eaf3de; --bg-danger: #fcebeb;
      --text: #1a1a18; --text2: #6b6b66; --text3: #9c9a92;
      --text-info: #0c447c; --text-success: #27500a; --text-danger: #791f1f;
      --text-warn: #633806;
      --border: rgba(0,0,0,0.11); --border2: rgba(0,0,0,0.20);
      --radius: 8px; --radius-lg: 12px;
      --adm-bg: #eaf3de; --adm-text: #27500a;
      --des-bg: #fcebeb; --des-text: #791f1f;
    }
    @media (prefers-color-scheme: dark) {
      :root {
        --bg: #1e1e1c; --bg2: #2a2a28; --bg3: #242422;
        --bg-info: #042c53; --bg-success: #173404; --bg-danger: #501313;
        --text: #e8e6de; --text2: #a8a69e; --text3: #6e6c65;
        --text-info: #85b7eb; --text-success: #97c459; --text-danger: #f09595;
        --text-warn: #ef9f27;
        --border: rgba(255,255,255,0.10); --border2: rgba(255,255,255,0.20);
        --adm-bg: #173404; --adm-text: #97c459;
        --des-bg: #501313; --des-text: #f09595;
      }
    }
    body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; font-size: 14px; color: var(--text); background: var(--bg3); min-height: 100vh; }
    header { background: var(--bg); border-bottom: 0.5px solid var(--border); padding: .9rem 1.5rem; display: flex; align-items: center; gap: 12px; }
    .logo { width: 36px; height: 36px; background: var(--bg-info); border-radius: var(--radius); display: flex; align-items: center; justify-content: center; color: var(--text-info); font-size: 19px; flex-shrink: 0; }
    header h1 { font-size: 15px; font-weight: 500; }
    header p { font-size: 12px; color: var(--text2); margin-top: 1px; }
    main { max-width: 1060px; margin: 0 auto; padding: 1.25rem 1.25rem 3rem; }
    .card { background: var(--bg); border: 0.5px solid var(--border); border-radius: var(--radius-lg); padding: 1.25rem; margin-bottom: 1rem; }
    .section-label { font-size: 11px; font-weight: 500; color: var(--text2); text-transform: uppercase; letter-spacing: .06em; margin-bottom: .75rem; display: flex; align-items: center; gap: 5px; }
    .drop-zone { border: 1.5px dashed var(--border2); border-radius: var(--radius-lg); padding: 2rem 1rem; text-align: center; cursor: pointer; transition: background .12s, border-color .12s; }
    .drop-zone:hover, .drop-zone.over { background: var(--bg2); }
    .drop-zone.done { border-color: #639922; border-style: solid; }
    .drop-zone i { font-size: 30px; color: var(--text3); display: block; margin-bottom: .6rem; }
    .drop-zone .dz-main { font-size: 13px; font-weight: 500; color: var(--text2); margin-bottom: .25rem; }
    .drop-zone .dz-sub { font-size: 12px; color: var(--text3); }
    input[type=file] { display: none; }
    .file-status { font-size: 12px; margin-top: .6rem; color: var(--text3); display: flex; align-items: center; gap: 5px; min-height: 18px; }
    .file-status.ok { color: var(--text-success); }
    .file-status.err { color: var(--text-danger); }
    .alert { font-size: 13px; padding: .7rem 1rem; border-radius: var(--radius); margin-bottom: 1rem; display: flex; align-items: flex-start; gap: 8px; }
    .alert.success { background: var(--bg-success); color: var(--text-success); }
    .alert.error   { background: var(--bg-danger);  color: var(--text-danger); }
    .alert.info    { background: var(--bg-info);    color: var(--text-info); }
    .metrics { display: grid; grid-template-columns: repeat(4, minmax(0, 1fr)); gap: 10px; margin-bottom: 1rem; }
    .metric { background: var(--bg2); border-radius: var(--radius); padding: .85rem 1rem; }
    .metric-label { font-size: 12px; color: var(--text2); margin-bottom: 4px; }
    .metric-value { font-size: 26px; font-weight: 500; line-height: 1; }
    .metric-value.blue  { color: var(--text-info); }
    .metric-value.green { color: var(--text-success); }
    .metric-value.red   { color: var(--text-danger); }
    .metric-value.muted { color: var(--text2); }
    .filters { display: flex; align-items: center; gap: 8px; flex-wrap: wrap; margin-bottom: 1rem; }
    .filters label { font-size: 12px; color: var(--text2); }
    select { font-size: 13px; padding: 5px 10px; border-radius: var(--radius); border: 0.5px solid var(--border2); background: var(--bg); color: var(--text); cursor: pointer; }
    select:focus { outline: none; box-shadow: 0 0 0 2px var(--bg-info); }
    .view-tabs { display: flex; gap: 4px; margin-bottom: 1rem; flex-wrap: wrap; align-items: center; }
    .vtab { font-size: 13px; padding: 5px 12px; border-radius: var(--radius); cursor: pointer; border: 0.5px solid var(--border); background: transparent; color: var(--text2); transition: background .1s; }
    .vtab:hover { background: var(--bg2); }
    .vtab.active { background: var(--bg2); color: var(--text); font-weight: 500; border-color: var(--border2); }
    .badge { display: inline-block; font-size: 11px; background: var(--bg3); color: var(--text2); border-radius: 10px; padding: 1px 7px; margin-left: 4px; }
    .btn { font-size: 13px; padding: 6px 13px; border-radius: var(--radius); cursor: pointer; border: 0.5px solid var(--border2); background: transparent; color: var(--text); display: inline-flex; align-items: center; gap: 5px; transition: background .1s; }
    .btn:hover { background: var(--bg2); }
    .btn:active { transform: scale(.98); }
    .view-content { display: none; }
    .view-content.active { display: block; }
    .scroll-wrap { overflow-x: auto; }
    table { width: 100%; border-collapse: collapse; font-size: 13px; table-layout: fixed; }
    th { text-align: left; font-weight: 500; color: var(--text2); font-size: 12px; padding: 7px 10px; border-bottom: 0.5px solid var(--border); white-space: nowrap; }
    th.sort { cursor: pointer; user-select: none; }
    th.sort:hover { color: var(--text); }
    td { padding: 7px 10px; border-bottom: 0.5px solid var(--border); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
    tr:last-child td { border-bottom: none; }
    tbody tr:hover td { background: var(--bg2); }
    .col-nome  { width: 24%; }
    .col-email { width: 27%; }
    .col-emp   { width: 16%; }
    .col-mes   { width: 13%; }
    .col-data  { width: 12%; }
    .col-tipo  { width: 8%; }
    .tag { display: inline-block; font-size: 11px; font-weight: 500; padding: 2px 8px; border-radius: 4px; }
    .tag.adm { background: var(--adm-bg); color: var(--adm-text); }
    .tag.des { background: var(--des-bg); color: var(--des-text); }
    .timeline { display: flex; flex-direction: column; gap: 1.5rem; }
    .tl-group-title { font-size: 14px; font-weight: 500; color: var(--text); margin-bottom: .75rem; display: flex; align-items: center; gap: 8px; }
    .tl-group-title .tl-dot { width: 8px; height: 8px; border-radius: 50%; background: var(--text2); flex-shrink: 0; }
    .tl-item { display: flex; align-items: flex-start; gap: 12px; padding: .6rem 0; border-bottom: 0.5px solid var(--border); }
    .tl-item:last-child { border-bottom: none; }
    .tl-icon { width: 30px; height: 30px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 15px; flex-shrink: 0; margin-top: 1px; }
    .tl-icon.adm { background: var(--adm-bg); color: var(--adm-text); }
    .tl-icon.des { background: var(--des-bg); color: var(--des-text); }
    .tl-info { flex: 1; min-width: 0; }
    .tl-name { font-size: 13px; font-weight: 500; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
    .tl-meta { font-size: 12px; color: var(--text2); margin-top: 2px; }
    .tl-date { font-size: 12px; color: var(--text3); white-space: nowrap; margin-left: auto; padding-left: 12px; }
    .company-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 1rem; }
    .company-card { background: var(--bg); border: 0.5px solid var(--border); border-radius: var(--radius-lg); padding: 1rem 1.25rem; }
    .company-name { font-size: 14px; font-weight: 500; margin-bottom: .75rem; display: flex; align-items: center; justify-content: space-between; }
    .co-stats { display: flex; gap: 8px; }
    .co-stat { font-size: 12px; padding: 2px 8px; border-radius: 4px; font-weight: 500; }
    .co-stat.adm { background: var(--adm-bg); color: var(--adm-text); }
    .co-stat.des { background: var(--des-bg); color: var(--des-text); }
    .co-list { margin-top: .6rem; }
    .co-row { font-size: 12px; padding: 4px 0; border-bottom: 0.5px solid var(--border); display: flex; align-items: center; gap: 6px; color: var(--text2); }
    .co-row:last-child { border-bottom: none; }
    .co-row .co-tag { font-size: 10px; padding: 1px 6px; border-radius: 3px; flex-shrink: 0; }
    .co-row .co-name { white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
    .co-row .co-date { margin-left: auto; color: var(--text3); flex-shrink: 0; font-size: 11px; }
    .empty { text-align: center; padding: 2.5rem; color: var(--text3); font-size: 13px; }
    .empty i { font-size: 28px; display: block; margin-bottom: .5rem; }
    footer { text-align: center; padding: 1.25rem; font-size: 12px; color: var(--text3); border-top: 0.5px solid var(--border); background: var(--bg); margin-top: 2rem; }
    @media (max-width: 640px) {
      .metrics { grid-template-columns: repeat(2, 1fr); }
      .col-email, .col-mes { display: none; }
    }
  </style>
</head>
<body>
<header>
  <div class="logo"><i class="ti ti-users-group" aria-hidden="true"></i></div>
  <div>
    <h1>Movimentações de colaboradores</h1>
    <p>Análise de admissões e desligamentos por mês e empresa</p>
  </div>
</header>
<main>
  <div class="card">
    <div class="section-label"><i class="ti ti-file-upload" aria-hidden="true"></i>Carregar planilha</div>
    <div class="drop-zone" id="drop-zone" onclick="document.getElementById('file-input').click()" role="button" tabindex="0">
      <i class="ti ti-table-import" aria-hidden="true"></i>
      <div class="dz-main">Clique ou arraste o arquivo aqui</div>
      <div class="dz-sub">Exportação ImpulseUP (.xlsx) — Nome, E-mail, Data de admissão, Empresa, Data de desligamento</div>
    </div>
    <input type="file" id="file-input" accept=".xlsx,.xls,.csv">
    <div class="file-status" id="file-status"></div>
  </div>
  <div id="alert-box"></div>
  <div class="metrics">
    <div class="metric"><div class="metric-label">Total de colaboradores</div><div class="metric-value blue" id="m-total">—</div></div>
    <div class="metric"><div class="metric-label">Admissões no período</div><div class="metric-value green" id="m-adm">—</div></div>
    <div class="metric"><div class="metric-label">Desligamentos no período</div><div class="metric-value red" id="m-des">—</div></div>
    <div class="metric"><div class="metric-label">Empresas</div><div class="metric-value muted" id="m-emp">—</div></div>
  </div>
  <div class="card">
    <div class="filters">
      <label for="sel-mes"><i class="ti ti-calendar" aria-hidden="true" style="font-size:14px;vertical-align:-2px;margin-right:2px"></i>Mês/ano</label>
      <select id="sel-mes"><option value="">Todos os períodos</option></select>
      <label for="sel-emp" style="margin-left:8px"><i class="ti ti-building" aria-hidden="true" style="font-size:14px;vertical-align:-2px;margin-right:2px"></i>Empresa</label>
      <select id="sel-emp"><option value="">Todas as empresas</option></select>
      <label for="sel-tipo" style="margin-left:8px"><i class="ti ti-filter" aria-hidden="true" style="font-size:14px;vertical-align:-2px;margin-right:2px"></i>Tipo</label>
      <select id="sel-tipo">
        <option value="">Admissões e desligamentos</option>
        <option value="adm">Somente admissões</option>
        <option value="des">Somente desligamentos</option>
      </select>
      <div style="flex:1"></div>
      <button class="btn" id="btn-export" style="display:none" onclick="exportCSV()"><i class="ti ti-download" aria-hidden="true"></i>Exportar CSV</button>
    </div>
    <div class="view-tabs">
      <button class="vtab active" data-view="tabela"><i class="ti ti-table" aria-hidden="true" style="font-size:14px;vertical-align:-2px;margin-right:4px"></i>Tabela <span class="badge" id="cnt-tabela">0</span></button>
      <button class="vtab" data-view="timeline"><i class="ti ti-timeline" aria-hidden="true" style="font-size:14px;vertical-align:-2px;margin-right:4px"></i>Por mês <span class="badge" id="cnt-timeline">0</span></button>
      <button class="vtab" data-view="empresa"><i class="ti ti-building" aria-hidden="true" style="font-size:14px;vertical-align:-2px;margin-right:4px"></i>Por empresa <span class="badge" id="cnt-empresa">0</span></button>
    </div>
    <div class="view-content active" id="view-tabela">
      <div class="scroll-wrap">
        <table>
          <thead><tr>
            <th class="col-nome sort" data-col="nome">Nome <i class="ti ti-arrows-sort" style="font-size:11px"></i></th>
            <th class="col-email">E-mail</th>
            <th class="col-emp sort" data-col="empresa">Empresa <i class="ti ti-arrows-sort" style="font-size:11px"></i></th>
            <th class="col-mes sort" data-col="mes">Mês/ano <i class="ti ti-arrows-sort" style="font-size:11px"></i></th>
            <th class="col-data">Data</th>
            <th class="col-tipo">Tipo</th>
          </tr></thead>
          <tbody id="body-tabela"><tr><td colspan="6" class="empty"><i class="ti ti-upload" aria-hidden="true"></i>Carregue a planilha para ver as movimentações</td></tr></tbody>
        </table>
      </div>
    </div>
    <div class="view-content" id="view-timeline">
      <div id="container-timeline" class="empty"><i class="ti ti-upload" aria-hidden="true"></i>Carregue a planilha para ver o histórico por mês</div>
    </div>
    <div class="view-content" id="view-empresa">
      <div id="container-empresa" class="empty"><i class="ti ti-upload" aria-hidden="true"></i>Carregue a planilha para ver o resumo por empresa</div>
    </div>
  </div>
</main>
<footer>Processamento 100% local — nenhum dado é enviado para servidores externos.</footer>
<script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js"></script>
<script>
let todosRegistros = [];
let sortCol = 'mes', sortDir = 1;
function showAlert(msg, type) {
  const el = document.getElementById('alert-box');
  const icon = type === 'success' ? 'check' : type === 'error' ? 'alert-circle' : 'info-circle';
  el.innerHTML = msg ? `<div class="alert ${type}"><i class="ti ti-${icon}" aria-hidden="true"></i><span>${msg}</span></div>` : '';
}
function parseDate(v) {
  if (!v) return null;
  if (v instanceof Date) return isNaN(v) ? null : v;
  if (typeof v === 'number') { const d = XLSX.SSF.parse_date_code(v); if (d) return new Date(d.y, d.m - 1, d.d); }
  if (typeof v === 'string') {
    const s = v.trim();
    const br = s.match(/^(\d{1,2})\/(\d{1,2})\/(\d{4})$/);
    if (br) return new Date(+br[3], +br[2] - 1, +br[1]);
    const iso = s.match(/^(\d{4})-(\d{2})-(\d{2})/);
    if (iso) return new Date(+iso[1], +iso[2] - 1, +iso[3]);
  }
  return null;
}
function fmtDate(d) { if (!d) return ''; return d.toLocaleDateString('pt-BR'); }
function mesAno(d) { if (!d) return ''; return d.toLocaleDateString('pt-BR', { month: '2-digit', year: 'numeric' }).replace(' de ', '/').replace(' ', '/'); }
function mesAnoSort(d) { if (!d) return ''; return `${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,'0')}`; }
function findCol(row, candidatos) {
  const keys = Object.keys(row);
  for (const c of candidatos) { const k = keys.find(k => k.trim().toLowerCase() === c.toLowerCase()); if (k) return k; }
  return null;
}
function lerArquivo(file, callback) {
  const reader = new FileReader();
  reader.onload = e => {
    try {
      const wb = XLSX.read(e.target.result, { type: 'array', cellDates: false });
      const rows = XLSX.utils.sheet_to_json(wb.Sheets[wb.SheetNames[0]], { defval: '' });
      callback(null, rows);
    } catch (err) { callback('Erro ao ler o arquivo: ' + err.message); }
  };
  reader.readAsArrayBuffer(file);
}
function processarPlanilha(rows) {
  if (!rows.length) return [];
  const r0 = rows[0];
  const cols = {
    nome:  findCol(r0, ['Nome completo','Nome','name']),
    email: findCol(r0, ['Email','E-mail','email']),
    adm:   findCol(r0, ['Data de admissão','Data admissão','admissao','hire date']),
    emp:   findCol(r0, ['Empresa','empresa','company']),
    des:   findCol(r0, ['Data de desligamento','Data desligamento','desligamento','termination date']),
  };
  if (!cols.email) return null;
  const eventos = [];
  rows.forEach(row => {
    const email = String(row[cols.email] || '').trim().toLowerCase();
    if (!email || !email.includes('@')) return;
    const nome    = String(row[cols.nome]  || '').trim();
    const empresa = String(row[cols.emp]   || '').trim();
    const dtAdm   = parseDate(row[cols.adm]);
    const dtDes   = cols.des ? parseDate(row[cols.des]) : null;
    if (dtAdm) eventos.push({ nome, email, empresa, tipo:'adm', data:dtAdm, mesAnoSort:mesAnoSort(dtAdm), mesAnoLabel:mesAno(dtAdm), dataFmt:fmtDate(dtAdm) });
    if (dtDes) eventos.push({ nome, email, empresa, tipo:'des', data:dtDes, mesAnoSort:mesAnoSort(dtDes), mesAnoLabel:mesAno(dtDes), dataFmt:fmtDate(dtDes) });
  });
  return eventos;
}
function popularFiltros(eventos) {
  const meses = [...new Set(eventos.map(e => e.mesAnoSort))].sort();
  const emps  = [...new Set(eventos.map(e => e.empresa))].filter(Boolean).sort();
  const selMes = document.getElementById('sel-mes');
  const selEmp = document.getElementById('sel-emp');
  selMes.innerHTML = '<option value="">Todos os períodos</option>';
  selEmp.innerHTML = '<option value="">Todas as empresas</option>';
  meses.forEach(m => { const ev = eventos.find(e => e.mesAnoSort === m); const opt = document.createElement('option'); opt.value = m; opt.textContent = ev.mesAnoLabel; selMes.appendChild(opt); });
  emps.forEach(emp => { const opt = document.createElement('option'); opt.value = emp; opt.textContent = emp; selEmp.appendChild(opt); });
}
function filtrar() {
  const mes = document.getElementById('sel-mes').value;
  const emp = document.getElementById('sel-emp').value;
  const tipo = document.getElementById('sel-tipo').value;
  return todosRegistros.filter(e => {
    if (mes  && e.mesAnoSort !== mes)  return false;
    if (emp  && e.empresa    !== emp)  return false;
    if (tipo && e.tipo       !== tipo) return false;
    return true;
  });
}
function renderTudo() {
  const dados = filtrar().slice().sort((a, b) => {
    let va = sortCol === 'mes' ? a.mesAnoSort : (a[sortCol] || '');
    let vb = sortCol === 'mes' ? b.mesAnoSort : (b[sortCol] || '');
    return va < vb ? -sortDir : va > vb ? sortDir : 0;
  });
  document.getElementById('cnt-tabela').textContent   = dados.length;
  document.getElementById('cnt-timeline').textContent = dados.length;
  document.getElementById('cnt-empresa').textContent  = dados.length;
  renderTabela(dados); renderTimeline(dados); renderEmpresas(dados);
}
function renderTabela(dados) {
  const tbody = document.getElementById('body-tabela');
  if (!dados.length) { tbody.innerHTML = '<tr><td colspan="6" class="empty"><i class="ti ti-search" aria-hidden="true"></i>Nenhum resultado para os filtros selecionados</td></tr>'; return; }
  tbody.innerHTML = dados.map(e => `<tr><td class="col-nome" title="${e.nome}">${e.nome}</td><td class="col-email" title="${e.email}">${e.email}</td><td class="col-emp" title="${e.empresa}">${e.empresa}</td><td class="col-mes">${e.mesAnoLabel}</td><td class="col-data">${e.dataFmt}</td><td class="col-tipo"><span class="tag ${e.tipo}">${e.tipo==='adm'?'Admissão':'Desligamento'}</span></td></tr>`).join('');
}
function renderTimeline(dados) {
  const container = document.getElementById('container-timeline');
  if (!dados.length) { container.className='empty'; container.innerHTML='<i class="ti ti-search" aria-hidden="true"></i>Nenhum resultado para os filtros selecionados'; return; }
  container.className = 'timeline';
  const porMes = {};
  dados.forEach(e => { if (!porMes[e.mesAnoSort]) porMes[e.mesAnoSort] = { label: e.mesAnoLabel, itens: [] }; porMes[e.mesAnoSort].itens.push(e); });
  container.innerHTML = Object.keys(porMes).sort().reverse().map(k => {
    const g = porMes[k];
    const adm = g.itens.filter(i => i.tipo==='adm').length;
    const des = g.itens.filter(i => i.tipo==='des').length;
    return `<div class="card" style="margin-bottom:0"><div class="tl-group-title"><span class="tl-dot"></span>${g.label}${adm?`<span class="tag adm" style="font-size:11px">${adm} admissão${adm>1?'ões':''}</span>`:''}${des?`<span class="tag des" style="font-size:11px">${des} desligamento${des>1?'s':''}</span>`:''}</div><div class="co-list">${g.itens.map(e=>`<div class="tl-item"><div class="tl-icon ${e.tipo}"><i class="ti ti-user-${e.tipo==='adm'?'plus':'minus'}" aria-hidden="true"></i></div><div class="tl-info"><div class="tl-name">${e.nome}</div><div class="tl-meta">${e.empresa}${e.email?' · '+e.email:''}</div></div><div class="tl-date">${e.dataFmt}</div></div>`).join('')}</div></div>`;
  }).join('');
}
function renderEmpresas(dados) {
  const container = document.getElementById('container-empresa');
  if (!dados.length) { container.className='empty'; container.innerHTML='<i class="ti ti-search" aria-hidden="true"></i>Nenhum resultado para os filtros selecionados'; return; }
  container.className = 'company-grid';
  const porEmp = {};
  dados.forEach(e => { if (!porEmp[e.empresa]) porEmp[e.empresa]=[]; porEmp[e.empresa].push(e); });
  container.innerHTML = Object.keys(porEmp).sort().map(emp => {
    const itens = porEmp[emp].slice().sort((a,b) => a.mesAnoSort < b.mesAnoSort ? 1 : -1);
    const adm = itens.filter(i=>i.tipo==='adm').length;
    const des = itens.filter(i=>i.tipo==='des').length;
    return `<div class="company-card"><div class="company-name">${emp||'(sem empresa)'}<div class="co-stats">${adm?`<span class="co-stat adm">+${adm}</span>`:''}${des?`<span class="co-stat des">-${des}</span>`:''}</div></div><div class="co-list">${itens.map(e=>`<div class="co-row"><span class="co-tag tag ${e.tipo}">${e.tipo==='adm'?'Adm':'Des'}</span><span class="co-name" title="${e.nome}">${e.nome}</span><span class="co-date">${e.mesAnoLabel}</span></div>`).join('')}</div></div>`;
  }).join('');
}
function exportCSV() {
  const dados = filtrar();
  if (!dados.length) return;
  const rows = dados.map(e => [e.nome,e.email,e.empresa,e.mesAnoLabel,e.dataFmt,e.tipo==='adm'?'Admissão':'Desligamento'].map(v=>`"${String(v).replace(/"/g,'""')}"`).join(',')).join('\n');
  const blob = new Blob(['\uFEFF' + 'Nome,Email,Empresa,Mês/Ano,Data,Tipo\n' + rows], { type:'text/csv;charset=utf-8' });
  const a = document.createElement('a'); a.href = URL.createObjectURL(blob); a.download = 'movimentacoes_' + new Date().toISOString().slice(0,7) + '.csv'; a.click();
}
function handle(file) {
  const status = document.getElementById('file-status');
  status.className = 'file-status'; status.textContent = 'Lendo ' + file.name + '…';
  lerArquivo(file, (err, rows) => {
    if (err) { status.className='file-status err'; status.innerHTML=`<i class="ti ti-alert-circle"></i>${err}`; showAlert(err,'error'); return; }
    const eventos = processarPlanilha(rows);
    if (!eventos) { showAlert('Coluna de e-mail não encontrada.','error'); return; }
    todosRegistros = eventos;
    document.getElementById('m-total').textContent = rows.length;
    document.getElementById('m-adm').textContent   = eventos.filter(e=>e.tipo==='adm').length;
    document.getElementById('m-des').textContent   = eventos.filter(e=>e.tipo==='des').length;
    document.getElementById('m-emp').textContent   = new Set(eventos.map(e=>e.empresa)).size;
    document.getElementById('drop-zone').classList.add('done');
    status.className='file-status ok'; status.innerHTML=`<i class="ti ti-check"></i>${file.name} — ${rows.length} colaboradores, ${eventos.length} eventos processados`;
    popularFiltros(eventos); renderTudo();
    document.getElementById('btn-export').style.display='';
    showAlert(`Planilha carregada: <strong>${eventos.filter(e=>e.tipo==='adm').length}</strong> admissão(ões) e <strong>${eventos.filter(e=>e.tipo==='des').length}</strong> desligamento(s) em <strong>${new Set(eventos.map(e=>e.empresa)).size}</strong> empresa(s).`,'success');
  });
}
const zone = document.getElementById('drop-zone'), input = document.getElementById('file-input');
input.onchange = () => { if (input.files[0]) handle(input.files[0]); };
zone.addEventListener('dragover', e => { e.preventDefault(); zone.classList.add('over'); });
zone.addEventListener('dragleave', () => zone.classList.remove('over'));
zone.addEventListener('drop', e => { e.preventDefault(); zone.classList.remove('over'); if (e.dataTransfer.files[0]) handle(e.dataTransfer.files[0]); });
zone.addEventListener('keydown', e => { if (e.key==='Enter'||e.key===' ') input.click(); });
document.querySelectorAll('.vtab').forEach(btn => btn.addEventListener('click', () => {
  document.querySelectorAll('.vtab').forEach(t=>t.classList.remove('active'));
  document.querySelectorAll('.view-content').forEach(t=>t.classList.remove('active'));
  btn.classList.add('active'); document.getElementById('view-'+btn.dataset.view).classList.add('active');
}));
document.querySelectorAll('th.sort').forEach(th => th.addEventListener('click', () => {
  const col = th.dataset.col;
  if (sortCol===col) sortDir*=-1; else { sortCol=col; sortDir=1; }
  renderTudo();
}));
['sel-mes','sel-emp','sel-tipo'].forEach(id => document.getElementById(id).addEventListener('change', renderTudo));
</script>
</body>
</html>
