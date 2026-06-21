<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Help / @Cell — Gestão de OS</title>
<style>
/* ── Reset & base ── */
*{box-sizing:border-box;margin:0;padding:0}
:root{
  --bg:#f8f7f4;--surface:#fff;--surface2:#f2f0eb;
  --border:#e0ddd6;--border2:#ccc9c0;
  --text:#1a1917;--text2:#5f5e5a;--text3:#999;
  --green:#1D9E75;--green-bg:#EAF3DE;--green-txt:#3B6D11;
  --blue:#185FA5;--blue-bg:#E6F1FB;--blue-txt:#185FA5;
  --amber:#854F0B;--amber-bg:#FAEEDA;--amber-txt:#854F0B;
  --red:#A32D2D;--red-bg:#FCEBEB;--red-txt:#A32D2D;
  --gray-bg:#F1EFE8;--gray-txt:#5F5E5A;
  --radius:10px;--radius-sm:7px;
}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;background:var(--bg);color:var(--text);font-size:14px;min-height:100vh}
button{cursor:pointer;font-family:inherit;font-size:13px}
input,select,textarea{font-family:inherit;font-size:13px}

/* ── Login ── */
#login-screen{display:flex;flex-direction:column;align-items:center;justify-content:center;min-height:100vh;gap:24px}
.login-card{background:var(--surface);border:0.5px solid var(--border);border-radius:var(--radius);padding:2rem 2.5rem;text-align:center;max-width:380px;width:90%}
.login-card h1{font-size:20px;font-weight:600;margin-bottom:4px}
.login-card p{color:var(--text2);font-size:13px;margin-bottom:24px}
.perfil-btns{display:flex;flex-direction:column;gap:10px}
.perfil-btn{padding:14px;border-radius:var(--radius-sm);border:0.5px solid var(--border);background:var(--surface);font-size:15px;font-weight:500;transition:all .15s}
.perfil-btn:hover{background:var(--surface2);border-color:var(--border2)}
.perfil-btn.cell{border-color:#1D9E75;color:#1D9E75}
.perfil-btn.cell:hover{background:var(--green-bg)}
.perfil-btn.help{border-color:#185FA5;color:#185FA5}
.perfil-btn.help:hover{background:var(--blue-bg)}
.pin-form{margin-top:16px;display:none;flex-direction:column;gap:10px}
.pin-form input{padding:10px 12px;border:0.5px solid var(--border);border-radius:var(--radius-sm);font-size:16px;text-align:center;letter-spacing:6px;width:100%}
.pin-form button{padding:10px;border-radius:var(--radius-sm);border:none;font-size:14px;font-weight:500}
.pin-submit{background:var(--text);color:#fff}
.pin-back{background:transparent;color:var(--text2);border:0.5px solid var(--border)!important;border:none}
.err-msg{color:var(--red);font-size:12px}

/* ── App layout ── */
#app{display:none;flex-direction:column;min-height:100vh}
.topbar{background:var(--surface);border-bottom:0.5px solid var(--border);padding:12px 20px;display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:10}
.topbar-left{display:flex;align-items:center;gap:12px}
.topbar h1{font-size:16px;font-weight:600}
.perfil-badge{font-size:11px;padding:3px 10px;border-radius:20px;font-weight:500}
.perfil-badge.cell{background:var(--green-bg);color:var(--green-txt)}
.perfil-badge.help{background:var(--blue-bg);color:var(--blue-txt)}
.unidade-sel{font-size:13px;border:0.5px solid var(--border);border-radius:var(--radius-sm);padding:5px 10px;background:var(--surface)}
.sair-btn{font-size:12px;color:var(--text2);background:none;border:0.5px solid var(--border);padding:4px 10px;border-radius:var(--radius-sm)}

/* ── Nav tabs ── */
.navtabs{background:var(--surface);border-bottom:0.5px solid var(--border);padding:0 20px;display:flex;gap:4px;overflow-x:auto}
.navtab{padding:10px 16px;border-bottom:2px solid transparent;font-size:13px;font-weight:500;background:none;border-left:none;border-right:none;border-top:none;color:var(--text2);white-space:nowrap}
.navtab.active{border-bottom-color:var(--text);color:var(--text)}

/* ── Main content ── */
.main{padding:20px;max-width:1100px;margin:0 auto;width:100%}

/* ── Cards ── */
.cards-row{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:10px;margin-bottom:20px}
.mcard{background:var(--surface2);border-radius:var(--radius-sm);padding:12px 14px}
.mcard-label{font-size:11px;color:var(--text2);margin-bottom:4px;text-transform:uppercase;letter-spacing:.04em}
.mcard-val{font-size:22px;font-weight:600;color:var(--text)}
.mcard-sub{font-size:11px;color:var(--text3);margin-top:2px}
.mcard.green .mcard-val{color:var(--green)}
.mcard.blue  .mcard-val{color:var(--blue)}
.mcard.amber .mcard-val{color:var(--amber)}

/* ── Section ── */
.section{background:var(--surface);border:0.5px solid var(--border);border-radius:var(--radius);overflow:hidden;margin-bottom:16px}
.section-header{padding:12px 16px;display:flex;align-items:center;justify-content:space-between;border-bottom:0.5px solid var(--border);flex-wrap:wrap;gap:8px}
.section-title{font-size:13px;font-weight:600;color:var(--text)}
.btn{padding:7px 14px;border-radius:var(--radius-sm);border:0.5px solid var(--border);background:var(--surface);color:var(--text);font-size:12px;font-weight:500;transition:all .15s}
.btn:hover{background:var(--surface2)}
.btn-primary{background:var(--text);color:#fff;border-color:var(--text)}
.btn-primary:hover{opacity:.88}
.btn-green{background:var(--green-bg);color:var(--green-txt);border-color:#9FE1CB}
.btn-amber{background:var(--amber-bg);color:var(--amber-txt);border-color:#FAC775}
.btn-sm{padding:4px 10px;font-size:11px}

/* ── Table ── */
.tbl-wrap{overflow-x:auto}
table{width:100%;border-collapse:collapse;font-size:12px}
th{text-align:left;padding:8px 10px;font-weight:600;font-size:11px;color:var(--text2);background:var(--surface2);border-bottom:0.5px solid var(--border);white-space:nowrap}
td{padding:9px 10px;border-bottom:0.5px solid var(--border);vertical-align:middle;color:var(--text)}
tr:last-child td{border-bottom:none}
tr:hover td{background:var(--surface2)}
.tfoot-row td{background:var(--surface2);font-weight:600;border-top:1px solid var(--border);border-bottom:none}

/* ── Badges ── */
.badge{display:inline-flex;align-items:center;gap:3px;font-size:10px;padding:2px 8px;border-radius:20px;font-weight:500;white-space:nowrap}
.b-green{background:var(--green-bg);color:var(--green-txt)}
.b-blue{background:var(--blue-bg);color:var(--blue-txt)}
.b-amber{background:var(--amber-bg);color:var(--amber-txt)}
.b-gray{background:var(--gray-bg);color:var(--gray-txt)}
.b-red{background:var(--red-bg);color:var(--red-txt)}

/* ── Inline inputs ── */
.td-input{width:72px;padding:4px 6px;border:0.5px solid var(--border);border-radius:5px;background:var(--surface);color:var(--text);text-align:right}
.td-input:focus{outline:none;border-color:var(--border2)}
.td-select{font-size:11px;padding:3px 5px;border:0.5px solid var(--border);border-radius:5px;background:var(--surface);color:var(--text)}

/* ── Modal / Drawer ── */
.overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.4);z-index:100;align-items:flex-end;justify-content:center}
.overlay.active{display:flex}
.drawer{background:var(--surface);border-radius:16px 16px 0 0;padding:24px;width:100%;max-width:560px;max-height:90vh;overflow-y:auto}
.drawer h2{font-size:16px;font-weight:600;margin-bottom:18px}
.form-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px}
.form-grid.full{grid-template-columns:1fr}
.form-field{display:flex;flex-direction:column;gap:5px}
.form-field label{font-size:12px;font-weight:500;color:var(--text2)}
.form-field input,.form-field select,.form-field textarea{padding:9px 11px;border:0.5px solid var(--border);border-radius:var(--radius-sm);background:var(--surface);color:var(--text)}
.form-field input:focus,.form-field select:focus{outline:none;border-color:var(--border2)}
.drawer-actions{display:flex;gap:10px;margin-top:20px}
.drawer-actions button{flex:1;padding:11px}

/* ── Alertas ── */
.alerta{display:flex;align-items:center;gap:12px;padding:12px 14px;border-radius:var(--radius-sm);background:var(--amber-bg);border:0.5px solid #FAC775;margin-bottom:8px}
.alerta .al-info{flex:1}
.alerta .al-os{font-size:13px;font-weight:600;color:#633806}
.alerta .al-msg{font-size:12px;color:var(--amber-txt);margin-top:2px}

/* ── Divisão / Fechamento ── */
.div-box{background:var(--surface2);border-radius:var(--radius-sm);padding:14px 18px;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:16px;margin-bottom:16px}
.div-item{text-align:center}
.div-label{font-size:11px;color:var(--text2);text-transform:uppercase;letter-spacing:.04em}
.div-val{font-size:26px;font-weight:700;margin-top:2px}
.div-val.green{color:var(--green)}
.div-val.blue{color:var(--blue)}
.div-sep{font-size:28px;color:var(--border2)}

/* ── Toast ── */
#toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%);background:#1a1917;color:#fff;padding:10px 20px;border-radius:30px;font-size:13px;font-weight:500;display:none;z-index:999;white-space:nowrap;max-width:90vw}

/* ── Fechamento preview ── */
.fech-box{background:var(--surface2);border:0.5px solid var(--border);border-radius:var(--radius);padding:18px;font-size:13px}
.fech-row{display:flex;justify-content:space-between;padding:5px 0;border-bottom:0.5px solid var(--border)}
.fech-row:last-child{border-bottom:none;font-weight:700;font-size:14px}
.fech-row.total{border-top:1px solid var(--border2);margin-top:6px;padding-top:8px}

/* ── Loading ── */
.loading{display:flex;align-items:center;justify-content:center;padding:40px;color:var(--text3);font-size:13px;gap:8px}
.spinner{width:16px;height:16px;border:2px solid var(--border);border-top-color:var(--text2);border-radius:50%;animation:spin .7s linear infinite}
@keyframes spin{to{transform:rotate(360deg)}}
.empty{padding:40px;text-align:center;color:var(--text3);font-size:13px}
</style>
</head>
<body>

<!-- ── LOGIN ─────────────────────────────────────────────── -->
<div id="login-screen">
  <div class="login-card">
    <h1>Help / @Cell</h1>
    <p>Gestão de Ordens de Serviço</p>
    <div id="escolha-perfil" class="perfil-btns">
      <button class="perfil-btn cell" onclick="escolherPerfil('cell')">📱 @Cell — Prestador</button>
      <button class="perfil-btn help" onclick="escolherPerfil('help')">🏪 Help — Contratante</button>
    </div>
    <div class="pin-form" id="pin-form">
      <p id="pin-label" style="font-size:13px;color:var(--text2)"></p>
      <input type="password" inputmode="numeric" maxlength="6" id="pin-input" placeholder="••••" oninput="if(this.value.length===4)confirmarPin()">
      <button class="pin-submit" onclick="confirmarPin()">Entrar</button>
      <button class="pin-back btn" onclick="voltarLogin()">Voltar</button>
      <p class="err-msg" id="pin-err" style="display:none">PIN incorreto</p>
    </div>
    <p style="font-size:11px;color:var(--text3);margin-top:16px">Sistema exclusivo Help / @Cell Celular · Três Lagoas/MS</p>
  </div>
</div>

<!-- ── APP ───────────────────────────────────────────────── -->
<div id="app">
  <div class="topbar">
    <div class="topbar-left">
      <h1>Help / @Cell</h1>
      <span class="perfil-badge" id="perfil-badge"></span>
    </div>
    <div style="display:flex;align-items:center;gap:8px">
      <select class="unidade-sel" id="unidade-sel" onchange="trocarUnidade(this.value)">
        <option value="Centro">Help Centro</option>
        <option value="Shopping">Help Shopping</option>
        <option value="Consolidado">Consolidado</option>
      </select>
      <button class="sair-btn" onclick="sair()">Sair</button>
    </div>
  </div>

  <div class="navtabs" id="navtabs">
    <button class="navtab active" onclick="switchTab('dashboard')">Dashboard</button>
    <button class="navtab" onclick="switchTab('os')">Ordens de Serviço</button>
    <button class="navtab" onclick="switchTab('importar')">⬆ Importar XLS</button>
    <button class="navtab" id="tab-fechamento" onclick="switchTab('fechamento')">Fechamento</button>
    <button class="navtab" id="tab-capital" onclick="switchTab('capital')">Capital</button>
  </div>

  <div class="main">

    <!-- Dashboard -->
    <div id="tab-dashboard">
    <div class="cards-row" id="cards-row">
      <div class="mcard"><div class="mcard-label">OS em aberto</div><div class="mcard-val" id="c-abertas">—</div><div class="mcard-sub">aguardando / andamento</div></div>
      <div class="mcard amber"><div class="mcard-label">Prontas p/ retirar</div><div class="mcard-val" id="c-prontas">—</div><div class="mcard-sub">avisar cliente</div></div>
      <div class="mcard"><div class="mcard-label">Entregues</div><div class="mcard-val" id="c-entregues">—</div></div>
      <div class="mcard amber"><div class="mcard-label">Lib. cobrança</div><div class="mcard-val" id="c-liberadas">—</div><div class="mcard-sub">não cobradas</div></div>
      <div class="mcard"><div class="mcard-label">Faturado</div><div class="mcard-val" id="c-fat">—</div></div>
      <div class="mcard green"><div class="mcard-label">@Cell (50%)</div><div class="mcard-val" id="c-cell">—</div></div>
    </div>
      <div class="div-box" id="div-box">
        <div class="div-item"><div class="div-label">Em caixa</div><div class="div-val" id="dv-caixa">—</div></div>
        <div class="div-sep">÷</div>
        <div class="div-item"><div class="div-label">@Cell (50%)</div><div class="div-val green" id="dv-cell">—</div></div>
        <div class="div-sep">=</div>
        <div class="div-item"><div class="div-label">Help (50%)</div><div class="div-val blue" id="dv-help">—</div></div>
      </div>
      <div id="alertas-section"></div>
    </div>

    <!-- Tabela OS -->
    <div id="tab-os" style="display:none">
      <div class="section">
        <div class="section-header">
          <span class="section-title" id="os-titulo">Ordens de Serviço</span>
          <div style="display:flex;gap:8px;flex-wrap:wrap">
            <select class="td-select" id="filtro-sit" style="padding:6px 8px;font-size:12px" onchange="filtrarTabela()">
              <option value="">Todas as situações</option>
              <option value="Aguardando">Aguardando</option>
              <option value="andamento">Em andamento</option>
              <option value="Pronto">Pronto — avisar</option>
              <option value="entregue">Entregue</option>
            </select>
            <button class="btn btn-primary" id="btn-nova-os" onclick="abrirDrawerNovaOS()">+ Nova OS</button>
          </div>
        </div>
        <div class="tbl-wrap">
          <table id="os-table">
            <thead><tr>
              <th>O.S</th><th>Modelo</th><th>Situação</th>
              <th id="th-venda">Venda</th><th id="th-taxa">Taxa</th>
              <th>Peças</th><th>@Cell</th><th>Retirou?</th><th>Dias</th><th>Ações</th>
            </tr></thead>
            <tbody id="os-tbody"></tbody>
            <tfoot id="os-tfoot"></tfoot>
          </table>
        </div>
      </div>
    </div>

    <!-- Fechamento -->
    <div id="tab-fechamento" style="display:none">
      <div class="section">
        <div class="section-header">
          <span class="section-title">Fechar período</span>
          <button class="btn btn-green" onclick="gerarFechamento()">Gerar fechamento ↓</button>
        </div>
        <div style="padding:16px" id="fech-content">
          <p style="color:var(--text2);font-size:13px">OS liberadas para cobrança (cliente retirou ou 30+ dias): carregando...</p>
        </div>
      </div>
      <div class="section" id="fech-historico-sec" style="display:none">
        <div class="section-header"><span class="section-title">Histórico de fechamentos</span></div>
        <div class="tbl-wrap">
          <table>
            <thead><tr><th>Período</th><th>Data</th><th>Faturado</th><th>Em caixa</th><th>@Cell</th><th>Help</th></tr></thead>
            <tbody id="fech-tbody"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- Capital -->
    <div id="tab-capital" style="display:none">
      <div class="section">
        <div class="section-header">
          <span class="section-title">Capital disponibilizado pela Help</span>
          <button class="btn btn-primary" id="btn-capital" onclick="abrirDrawerCapital()">+ Lançar capital</button>
        </div>
        <div style="padding:16px" id="capital-content">
          <div class="loading"><span class="spinner"></span> Carregando...</div>
        </div>
      </div>
    </div>

    <!-- Importar XLS -->
    <div id="tab-importar" style="display:none">
      <div class="section">
        <div class="section-header">
          <span class="section-title">Importar arquivo do sistema (XLS/XLSX)</span>
        </div>
        <div style="padding:20px">
          <p style="color:var(--text2);font-size:13px;margin-bottom:16px">
            Exporte o relatório de OS do sistema SHARMAQ e importe aqui. O sistema identifica automaticamente as OS novas e atualiza situações das existentes.
          </p>
          <div id="drop-zone" style="border:2px dashed var(--border2);border-radius:var(--radius);padding:40px;text-align:center;cursor:pointer;transition:all .2s" 
               onclick="document.getElementById('xls-input').click()"
               ondragover="event.preventDefault();this.style.borderColor='var(--green)'"
               ondragleave="this.style.borderColor='var(--border2)'"
               ondrop="handleDrop(event)">
            <div style="font-size:32px;margin-bottom:8px">📂</div>
            <div style="font-size:14px;font-weight:500;color:var(--text)">Clique ou arraste o arquivo XLS aqui</div>
            <div style="font-size:12px;color:var(--text3);margin-top:4px">Exportação do SHARMAQ — Consulta de OS</div>
          </div>
          <input type="file" id="xls-input" accept=".xls,.xlsx" style="display:none" onchange="processarXLS(this.files[0])">
          <div id="import-preview" style="margin-top:20px;display:none">
            <div style="font-size:13px;font-weight:600;margin-bottom:10px" id="import-summary"></div>
            <div class="tbl-wrap" style="max-height:360px;overflow-y:auto">
              <table id="import-table">
                <thead><tr>
                  <th>O.S</th><th>Modelo</th><th>Defeito</th><th>Situação</th>
                  <th>Entrada</th><th>Atendente</th><th>Vlr Serv.</th><th>Status</th>
                </tr></thead>
                <tbody id="import-tbody"></tbody>
              </table>
            </div>
            <div style="display:flex;gap:10px;margin-top:16px">
              <button class="btn" onclick="cancelarImport()">Cancelar</button>
              <button class="btn btn-primary" onclick="confirmarImport()">Importar <span id="import-count"></span> OS</button>
            </div>
          </div>
          <div id="import-resultado" style="margin-top:16px;display:none"></div>
        </div>
      </div>
    </div>

  </div>
</div>

<!-- ── DRAWER NOVA OS ─────────────────────────────────────── -->
<div class="overlay" id="drawer-os">
  <div class="drawer">
    <h2 id="drawer-os-titulo">Nova OS</h2>
    <div class="form-grid">
      <div class="form-field">
        <label>Número da OS *</label>
        <input type="number" id="f-os" placeholder="74XXX">
      </div>
      <div class="form-field">
        <label>Modelo do aparelho *</label>
        <input type="text" id="f-modelo" placeholder="iPhone 15, A55...">
      </div>
      <div class="form-field full form-grid">
        <div class="form-field">
          <label>Defeito *</label>
          <input type="text" id="f-defeito" placeholder="Módulo completo, não liga...">
        </div>
        <div class="form-field">
          <label>Situação</label>
          <select id="f-situacao">
            <option>Aguardando autorização do orçamento</option>
            <option>Aguardando avaliação do técnico</option>
            <option selected>Autorizado, Reparo em andamento</option>
            <option>Pronto, avisar cliente</option>
            <option>Equipamento entregue reparado</option>
          </select>
        </div>
      </div>
      <div class="form-field" id="f-venda-wrap">
        <label>Valor de venda (R$)</label>
        <input type="number" id="f-venda" placeholder="0.00" step="0.01" oninput="calcPreview()">
      </div>
      <div class="form-field" id="f-taxa-wrap">
        <label>Forma de pagamento</label>
        <select id="f-forma" onchange="calcPreview()">
          <option value="0">Pix / Dinheiro — 0%</option>
          <option value="0.05">Débito / Crédito à vista — 5%</option>
          <option value="0.10">Crédito parcelado — 10%</option>
        </select>
      </div>
      <div class="form-field">
        <label>Peças — capital (R$)</label>
        <input type="number" id="f-pecas-cap" placeholder="0.00" step="0.01" oninput="calcPreview()">
      </div>
      <div class="form-field">
        <label>Peças — base cálculo (R$)</label>
        <input type="number" id="f-pecas-base" placeholder="= capital se vazio" step="0.01" oninput="calcPreview()">
      </div>
      <div class="form-field full">
        <label>Observação</label>
        <textarea id="f-obs" rows="2" placeholder="Ainda na loja, saiu..."></textarea>
      </div>
    </div>
    <!-- Preview cálculo -->
    <div id="calc-preview" style="background:var(--surface2);border-radius:var(--radius-sm);padding:12px;margin-top:12px;font-size:12px;display:none">
      <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px;text-align:center">
        <div><div style="color:var(--text2)">Recebido</div><div style="font-size:16px;font-weight:700" id="prev-rec">—</div></div>
        <div><div style="color:var(--text2)">Em caixa</div><div style="font-size:16px;font-weight:700" id="prev-caixa">—</div></div>
        <div><div style="color:var(--text2)">@Cell (50%)</div><div style="font-size:16px;font-weight:700;color:var(--green)" id="prev-cell">—</div></div>
      </div>
    </div>
    <div class="drawer-actions">
      <button class="btn" onclick="fecharDrawer('drawer-os')">Cancelar</button>
      <button class="btn btn-primary" onclick="salvarOS()">Salvar OS</button>
    </div>
  </div>
</div>

<!-- ── DRAWER EDITAR OS ───────────────────────────────────── -->
<div class="overlay" id="drawer-edit">
  <div class="drawer">
    <h2 id="edit-titulo">Editar OS</h2>
    <input type="hidden" id="edit-os-num">
    <div class="form-grid">
      <div class="form-field full">
        <label>Situação</label>
        <select id="edit-situacao">
          <option>Aguardando autorização do orçamento</option>
          <option>Aguardando avaliação do técnico</option>
          <option>Autorizado, Reparo em andamento</option>
          <option>Pronto, avisar cliente</option>
          <option>Equipamento entregue reparado</option>
        </select>
      </div>
      <div class="form-field">
        <label>Valor de venda (R$)</label>
        <input type="number" id="edit-venda" step="0.01" oninput="calcPreviewEdit()">
      </div>
      <div class="form-field">
        <label>Forma de pagamento</label>
        <select id="edit-forma" onchange="calcPreviewEdit()">
          <option value="0">Pix / Dinheiro — 0%</option>
          <option value="0.05">Débito / Crédito à vista — 5%</option>
          <option value="0.10">Crédito parcelado — 10%</option>
        </select>
      </div>
      <div class="form-field">
        <label>Peças capital (R$)</label>
        <input type="number" id="edit-pecas-cap" step="0.01" oninput="calcPreviewEdit()">
      </div>
      <div class="form-field">
        <label>Peças base cálculo (R$)</label>
        <input type="number" id="edit-pecas-base" step="0.01" oninput="calcPreviewEdit()">
      </div>
      <div class="form-field full">
        <label>Observação</label>
        <textarea id="edit-obs" rows="2"></textarea>
      </div>
    </div>
    <div id="calc-preview-edit" style="background:var(--surface2);border-radius:var(--radius-sm);padding:12px;margin-top:12px;font-size:12px;display:none">
      <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px;text-align:center">
        <div><div style="color:var(--text2)">Recebido</div><div style="font-size:16px;font-weight:700" id="prev-rec-e">—</div></div>
        <div><div style="color:var(--text2)">Em caixa</div><div style="font-size:16px;font-weight:700" id="prev-caixa-e">—</div></div>
        <div><div style="color:var(--text2)">@Cell (50%)</div><div style="font-size:16px;font-weight:700;color:var(--green)" id="prev-cell-e">—</div></div>
      </div>
    </div>
    <div class="drawer-actions">
      <button class="btn" onclick="fecharDrawer('drawer-edit')">Cancelar</button>
      <button class="btn btn-amber" id="btn-retirada-edit" onclick="marcarRetiradaEdit()" style="display:none">✓ Cliente retirou</button>
      <button class="btn btn-primary" onclick="salvarEdicao()">Atualizar</button>
    </div>
  </div>
</div>

<!-- ── DRAWER CAPITAL ─────────────────────────────────────── -->
<div class="overlay" id="drawer-capital">
  <div class="drawer">
    <h2>Lançar capital recebido</h2>
    <div class="form-grid">
      <div class="form-field">
        <label>Valor (R$) *</label>
        <input type="number" id="cap-valor" placeholder="0.00" step="0.01">
      </div>
      <div class="form-field">
        <label>Descrição</label>
        <input type="text" id="cap-desc" placeholder="Transferência 13/06...">
      </div>
    </div>
    <div class="drawer-actions">
      <button class="btn" onclick="fecharDrawer('drawer-capital')">Cancelar</button>
      <button class="btn btn-primary" onclick="salvarCapital()">Confirmar</button>
    </div>
  </div>
</div>

<div id="toast"></div>

<script>
// ══════════════════════════════════════════════════════════════
// CONFIGURAÇÃO — cole a URL do seu Apps Script aqui após publicar
// ══════════════════════════════════════════════════════════════
const SCRIPT_URL = 'https://script.google.com/macros/s/AKfycbym3Qqq_QDRbzRMVQ_M7O4qRUYMFvAuuqxyCHodcEDATjwB-VZMZ-bZFDJPg_holBPDBQ/exec';

// PINs de acesso (altere para os PINs reais)
const PINS = { cell: '1234', help: '5678' };

// ── Estado ────────────────────────────────────────────────────
let perfil = null;       // 'cell' ou 'help'
let unidade = 'Centro';  // 'Centro' | 'Shopping' | 'Consolidado'
let currentTab = 'dashboard';
let osCache = [];
let perfilSelecionado = null;

// ── Login ─────────────────────────────────────────────────────
function escolherPerfil(p) {
  perfilSelecionado = p;
  document.getElementById('escolha-perfil').style.display = 'none';
  const pf = document.getElementById('pin-form');
  pf.style.display = 'flex';
  document.getElementById('pin-label').textContent =
    p === 'cell' ? 'Digite o PIN da @Cell (prestador)' : 'Digite o PIN da Help (contratante)';
  document.getElementById('pin-input').focus();
}

function confirmarPin() {
  const pin = document.getElementById('pin-input').value;
  if (pin === PINS[perfilSelecionado]) {
    perfil = perfilSelecionado;
    iniciarApp();
  } else {
    document.getElementById('pin-err').style.display = 'block';
    document.getElementById('pin-input').value = '';
    setTimeout(()=>document.getElementById('pin-err').style.display='none', 2500);
  }
}

function voltarLogin() {
  perfilSelecionado = null;
  document.getElementById('pin-form').style.display = 'none';
  document.getElementById('pin-input').value = '';
  document.getElementById('escolha-perfil').style.display = 'flex';
}

// ── Iniciar app ───────────────────────────────────────────────
function iniciarApp() {
  document.getElementById('login-screen').style.display = 'none';
  document.getElementById('app').style.display = 'flex';

  const badge = document.getElementById('perfil-badge');
  badge.textContent = perfil === 'cell' ? '@Cell — Prestador' : 'Help — Contratante';
  badge.className = 'perfil-badge ' + perfil;

  // Acesso simétrico — ambos veem e editam tudo
  carregarDashboard();
}

function sair() {
  perfil = null;
  perfilSelecionado = null;
  document.getElementById('login-screen').style.display = 'flex';
  document.getElementById('app').style.display = 'none';
  document.getElementById('pin-form').style.display = 'none';
  document.getElementById('escolha-perfil').style.display = 'flex';
  document.getElementById('pin-input').value = '';
}

// ── Navegação ─────────────────────────────────────────────────
function switchTab(tab) {
  currentTab = tab;
  document.querySelectorAll('.navtab').forEach(t=>t.classList.remove('active'));
  event.target.classList.add('active');
  ['dashboard','os','importar','fechamento','capital'].forEach(t=>{
    document.getElementById('tab-'+t).style.display = t===tab?'block':'none';
  });
  if (tab==='dashboard') carregarDashboard();
  if (tab==='os') carregarOS();
  if (tab==='importar') initImportar();
  if (tab==='fechamento') carregarFechamento();
  if (tab==='capital') carregarCapital();
}

function trocarUnidade(u) {
  unidade = u;
  if (currentTab==='dashboard') carregarDashboard();
  if (currentTab==='os') carregarOS();
  if (currentTab==='importar') initImportar();
  if (currentTab==='fechamento') carregarFechamento();
  if (currentTab==='capital') carregarCapital();
}

// ── API calls ─────────────────────────────────────────────────
async function apiGet(params) {
  const url = SCRIPT_URL + '?' + new URLSearchParams(params);
  const r = await fetch(url);
  return r.json();
}
async function apiPost(body) {
  const r = await fetch(SCRIPT_URL, {
    method:'POST', body: JSON.stringify(body),
    headers:{'Content-Type':'application/json'}
  });
  return r.json();
}

// ── Dashboard ─────────────────────────────────────────────────
async function carregarDashboard() {
  document.getElementById('cards-row').innerHTML =
    '<div class="mcard"><div class="loading"><span class="spinner"></span></div></div>';

  const res = await apiGet({acao:'getResumo', unidade});
  if (!res.ok) { toast('Erro: '+res.erro); return; }

  const d = res;
  document.getElementById('c-abertas').textContent = d.qAberta;
  document.getElementById('c-prontas').textContent = d.qPronta;
  document.getElementById('c-entregues').textContent = d.qEntregue;
  document.getElementById('c-liberadas').textContent = d.qLiberada;
  document.getElementById('c-fat').textContent = fmt(d.totFat);
  document.getElementById('c-cell').textContent = fmt(d.totCell);

  document.getElementById('dv-caixa').textContent = fmt(d.totCaixa);
  document.getElementById('dv-cell').textContent = fmt(d.totCell);
  document.getElementById('dv-help').textContent = fmt(d.totCell);

  // Alertas
  const osRes = await apiGet({acao:'listarOS', unidade: unidade==='Consolidado'?'Centro':unidade});
  const alertas = (osRes.dados||[]).filter(r=>
    (r.situacao||'').includes('Pronto') && r.diasAberto > 3 ||
    r.cobradoPorPrazo
  );
  document.getElementById('alertas-section').innerHTML = alertas.length===0 ? '' :
    `<div class="section"><div class="section-header"><span class="section-title">⚠ Alertas</span></div><div style="padding:12px">${
      alertas.map(r=>`<div class="alerta">
        <div class="al-info">
          <div class="al-os">OS ${r.os} — ${r.modelo}</div>
          <div class="al-msg">${r.cobradoPorPrazo?'30+ dias sem retirada — liberado para cobrança':
            `Pronto há ${r.diasAberto} dias — cliente não foi avisado?`}</div>
        </div>
      </div>`).join('')
    }</div></div>`;
}

// ── Tabela OS ─────────────────────────────────────────────────
async function carregarOS() {
  document.getElementById('os-tbody').innerHTML =
    '<tr><td colspan="10" class="loading"><span class="spinner"></span> Carregando...</td></tr>';
  const u = unidade==='Consolidado'?'Centro':unidade;
  const res = await apiGet({acao:'listarOS', unidade:u});
  if (!res.ok) { toast('Erro: '+res.erro); return; }
  osCache = res.dados || [];
  renderTabela(osCache);
}

function filtrarTabela() {
  const f = document.getElementById('filtro-sit').value;
  const filtrado = f ? osCache.filter(r=>(r.situacao||'').includes(f)) : osCache;
  renderTabela(filtrado);
}

function renderTabela(dados) {
  let totV=0,totP=0,totCell=0;

  const rows = dados.map(r=>{
    totV+=r.venda||0; totP+=r.pecasBase||r.pecasCapital||0; totCell+=r.cell50||0;
    const sit = sitBadge(r.situacao||'');
    const dias = r.diasAberto!==null ? `<span style="color:${r.diasAberto>10?'var(--red)':r.diasAberto>5?'var(--amber)':'var(--text2)'}">${r.diasAberto}d</span>` : '—';
    const retirou = r.retirou
      ? `<span class="badge b-green">Sim</span>`
      : `<button class="btn btn-sm btn-amber" onclick="marcarRetirada(${r.os})">Marcar</button>`;
    const acoes = `<button class="btn btn-sm" onclick="abrirEditar(${r.os})">Editar</button>`;

    const vRow = `<td>${r.venda>0?fmt(r.venda):'—'}</td>
         <td>${r.taxaPct>0?(r.taxaPct*100)+'%':'0%'}</td>`;
    const cellRow = `<td style="color:var(--green);font-weight:600">${r.cell50>0?fmt(r.cell50):'—'}</td>`;

    return `<tr>
      <td style="font-size:11px;color:var(--text2);white-space:nowrap">${r.os}</td>
      <td><div style="font-weight:500;font-size:12px">${r.modelo}</div><div style="font-size:11px;color:var(--text3)">${r.defeito}</div></td>
      <td>${sit}</td>
      ${vRow}
      <td>${r.pecasBase||r.pecasCapital ? fmt(r.pecasBase||r.pecasCapital) : '—'}</td>
      ${cellRow}
      <td>${r.retirou?`<span class="badge b-green">Sim · ${r.dataRetirada||''}</span>`:`<span class="badge b-gray">Não</span>`}</td>
      <td>${dias}</td>
      <td>${acoes}</td>
    </tr>`;
  }).join('');

  document.getElementById('os-tbody').innerHTML = rows || '<tr><td colspan="10" class="empty">Nenhuma OS encontrada</td></tr>';
  document.getElementById('os-tfoot').innerHTML = `<tr class="tfoot-row">
    <td colspan="3">Totais</td>
    <td>${fmt(totV)}</td><td></td><td>${fmt(totP)}</td>
    <td style="color:var(--green)">${fmt(totCell)}</td>
    <td colspan="3"></td>
  </tr>`;
}

function sitBadge(s) {
  if (s.includes('Pronto')) return `<span class="badge b-green">Pronto — avisar</span>`;
  if (s.includes('andamento')) return `<span class="badge b-blue">Em andamento</span>`;
  if (s.includes('Aguardando')) return `<span class="badge b-amber">Aguardando</span>`;
  if (s.includes('entregue')) return `<span class="badge b-gray">Entregue</span>`;
  return `<span class="badge b-gray">${s}</span>`;
}

// ── Marcar retirada ───────────────────────────────────────────
async function marcarRetirada(osNum) {
  if (!confirm('Confirmar retirada do cliente para OS '+osNum+'?')) return;
  const res = await apiPost({acao:'marcarRetirada', unidade, os:osNum});
  toast(res.mensagem || res.erro);
  if (res.ok) carregarOS();
}

// ── Drawer nova OS ────────────────────────────────────────────
function abrirDrawerNovaOS() {
  document.getElementById('f-os').value='';
  document.getElementById('f-modelo').value='';
  document.getElementById('f-defeito').value='';
  document.getElementById('f-venda').value='';
  document.getElementById('f-pecas-cap').value='';
  document.getElementById('f-pecas-base').value='';
  document.getElementById('f-obs').value='';
  document.getElementById('calc-preview').style.display='none';
  document.getElementById('drawer-os').classList.add('active');
}

function calcPreview() {
  const v=parseFloat(document.getElementById('f-venda').value)||0;
  const t=parseFloat(document.getElementById('f-forma').value)||0;
  const p=parseFloat(document.getElementById('f-pecas-base').value)||parseFloat(document.getElementById('f-pecas-cap').value)||0;
  if (!v) { document.getElementById('calc-preview').style.display='none'; return; }
  const rec=v-(v*t), com=rec*0.03, caixa=rec-p-com, cell=caixa>0?caixa*0.5:0;
  document.getElementById('calc-preview').style.display='block';
  document.getElementById('prev-rec').textContent=fmt(rec);
  document.getElementById('prev-caixa').textContent=fmt(caixa);
  document.getElementById('prev-cell').textContent=fmt(cell);
}

async function salvarOS() {
  const os=document.getElementById('f-os').value;
  const modelo=document.getElementById('f-modelo').value;
  const defeito=document.getElementById('f-defeito').value;
  if (!os||!modelo||!defeito) { toast('Preencha OS, modelo e defeito'); return; }
  const res = await apiPost({
    acao:'lancarOS', unidade,
    os, modelo, defeito,
    situacao: document.getElementById('f-situacao').value,
    venda: parseFloat(document.getElementById('f-venda').value)||0,
    taxaPct: parseFloat(document.getElementById('f-forma').value)||0,
    pecasCapital: parseFloat(document.getElementById('f-pecas-cap').value)||0,
    pecasBase: parseFloat(document.getElementById('f-pecas-base').value)||0,
    obs: document.getElementById('f-obs').value
  });
  toast(res.mensagem||res.erro);
  if (res.ok) { fecharDrawer('drawer-os'); carregarOS(); }
}

// ── Drawer editar OS ──────────────────────────────────────────
function abrirEditar(osNum) {
  const r = osCache.find(x=>x.os==osNum);
  if (!r) return;
  document.getElementById('edit-os-num').value = osNum;
  document.getElementById('edit-titulo').textContent = 'Editar OS '+osNum+' — '+r.modelo;
  document.getElementById('edit-situacao').value = r.situacao||'';
  document.getElementById('edit-venda').value = r.venda||'';
  document.getElementById('edit-forma').value = r.taxaPct||0;
  document.getElementById('edit-pecas-cap').value = r.pecasCapital||'';
  document.getElementById('edit-pecas-base').value = r.pecasBase||'';
  document.getElementById('edit-obs').value = r.obs||'';
  // Mostrar botão de retirada só se ainda não retirou
  document.getElementById('btn-retirada-edit').style.display = r.retirou ? 'none' : 'block';
  calcPreviewEdit();
  document.getElementById('drawer-edit').classList.add('active');
}

async function marcarRetiradaEdit() {
  const osNum = document.getElementById('edit-os-num').value;
  if (!confirm('Confirmar retirada do cliente para OS '+osNum+'?')) return;
  const res = await apiPost({acao:'marcarRetirada', unidade, os:osNum});
  toast(res.mensagem||res.erro);
  if (res.ok) { fecharDrawer('drawer-edit'); carregarOS(); }
}

function calcPreviewEdit() {
  const v=parseFloat(document.getElementById('edit-venda').value)||0;
  const t=parseFloat(document.getElementById('edit-forma').value)||0;
  const p=parseFloat(document.getElementById('edit-pecas-base').value)||parseFloat(document.getElementById('edit-pecas-cap').value)||0;
  if (!v) { document.getElementById('calc-preview-edit').style.display='none'; return; }
  const rec=v-(v*t), com=rec*0.03, caixa=rec-p-com, cell=caixa>0?caixa*0.5:0;
  document.getElementById('calc-preview-edit').style.display='block';
  document.getElementById('prev-rec-e').textContent=fmt(rec);
  document.getElementById('prev-caixa-e').textContent=fmt(caixa);
  document.getElementById('prev-cell-e').textContent=fmt(cell);
}

async function salvarEdicao() {
  const osNum=document.getElementById('edit-os-num').value;
  const res = await apiPost({
    acao:'atualizarOS', unidade, os:osNum,
    situacao: document.getElementById('edit-situacao').value,
    venda: parseFloat(document.getElementById('edit-venda').value)||0,
    taxaPct: parseFloat(document.getElementById('edit-forma').value)||0,
    pecasCapital: parseFloat(document.getElementById('edit-pecas-cap').value)||0,
    pecasBase: parseFloat(document.getElementById('edit-pecas-base').value)||0,
    obs: document.getElementById('edit-obs').value
  });
  toast(res.mensagem||res.erro);
  if (res.ok) { fecharDrawer('drawer-edit'); carregarOS(); }
}

// ── Fechamento ────────────────────────────────────────────────
async function carregarFechamento() {
  const u = unidade==='Consolidado'?'Centro':unidade;
  const res = await apiGet({acao:'listarOS', unidade:u});
  const liberadas = (res.dados||[]).filter(r=>r.liberadoParaCobranca && !r.cobrado);

  let html='';
  if (liberadas.length===0) {
    html='<p style="color:var(--text2)">Nenhuma OS liberada para cobrança no momento.</p>';
  } else {
    let totV=0,totP=0,totCaixa=0,totCell=0;
    liberadas.forEach(r=>{totV+=r.venda;totP+=r.pecasBase||r.pecasCapital;totCaixa+=r.emCaixa;totCell+=r.cell50;});
    const rows = liberadas.map(r=>`<tr><td>OS ${r.os}</td><td>${r.modelo}</td>
      <td>${r.retirou?'Retirou':'30+ dias'}</td>
      <td>${fmt(r.venda)}</td><td>${fmt(r.cell50)}</td></tr>`).join('');

    html=`<div style="overflow-x:auto;margin-bottom:16px"><table>
      <thead><tr><th>OS</th><th>Modelo</th><th>Motivo</th><th>Venda</th><th>@Cell</th></tr></thead>
      <tbody>${rows}</tbody></table></div>
      <div class="fech-box">
        <div class="fech-row"><span>Faturado</span><span>${fmt(totV)}</span></div>
        <div class="fech-row"><span>Peças</span><span>− ${fmt(totP)}</span></div>
        <div class="fech-row"><span>Comissão (3%)</span><span>− ${fmt(totV*0.03)}</span></div>
        <div class="fech-row total"><span>@Cell recebe (50%)</span><span style="color:var(--green)">${fmt(totCell)}</span></div>
        <div class="fech-row"><span>Help recebe (50%)</span><span style="color:var(--blue)">${fmt(totCell)}</span></div>
      </div>`;
  }
  document.getElementById('fech-content').innerHTML=html;

  const fhRes = await apiGet({acao:'getFechamentos', unidade});
  if (fhRes.ok && fhRes.dados.length>0) {
    document.getElementById('fech-historico-sec').style.display='block';
    document.getElementById('fech-tbody').innerHTML = fhRes.dados.reverse().map(f=>`<tr>
      <td>${f.periodo}</td><td style="font-size:11px">${f.dataGerado}</td>
      <td>${fmt(f.fat)}</td><td>${fmt(f.caixa)}</td>
      <td style="color:var(--green);font-weight:600">${fmt(f.cell)}</td>
      <td style="color:var(--blue)">${fmt(f.help)}</td>
    </tr>`).join('');
  }
}

async function gerarFechamento() {
  if (!confirm('Gerar fechamento e marcar as OS como cobradas?')) return;
  const res = await apiPost({acao:'gerarFechamento', unidade});
  toast(res.mensagem||res.erro);
  if (res.ok) carregarFechamento();
}

// ── Capital ───────────────────────────────────────────────────
async function carregarCapital() {
  const u = unidade==='Consolidado'?'Centro':unidade;
  const res = await apiGet({acao:'getCapital', unidade:u});
  if (!res.ok) { document.getElementById('capital-content').innerHTML='<p>Erro ao carregar</p>'; return; }
  document.getElementById('capital-content').innerHTML=`
    <div class="cards-row" style="margin-bottom:16px">
      <div class="mcard"><div class="mcard-label">Total disponibilizado</div><div class="mcard-val">${fmt(res.total)}</div></div>
      <div class="mcard"><div class="mcard-label">Peças utilizadas</div><div class="mcard-val">${fmt(res.pecasUsadas)}</div></div>
      <div class="mcard green"><div class="mcard-label">Saldo disponível</div><div class="mcard-val">${fmt(res.saldo)}</div></div>
    </div>
    ${res.entradas.length>0?`<div class="tbl-wrap"><table>
      <thead><tr><th>Data</th><th>Valor</th><th>Descrição</th></tr></thead>
      <tbody>${res.entradas.map(e=>`<tr><td>${e.data}</td><td>${fmt(e.valor)}</td><td>${e.desc||'—'}</td></tr>`).join('')}</tbody>
    </table></div>`:'<p style="color:var(--text3);font-size:13px">Nenhum capital lançado ainda.</p>'}
  `;
}

function abrirDrawerCapital() {
  document.getElementById('cap-valor').value='';
  document.getElementById('cap-desc').value='';
  document.getElementById('drawer-capital').classList.add('active');
}

async function salvarCapital() {
  const v=parseFloat(document.getElementById('cap-valor').value);
  if (!v||v<=0){toast('Informe um valor válido');return;}
  const u=unidade==='Consolidado'?'Centro':unidade;
  const res=await apiPost({acao:'lancarCapital',unidade:u,valor:v,descricao:document.getElementById('cap-desc').value});
  toast(res.mensagem||res.erro);
  if(res.ok){fecharDrawer('drawer-capital');carregarCapital();}
}

// ── Helpers ───────────────────────────────────────────────────
function fecharDrawer(id) { document.getElementById(id).classList.remove('active'); }
document.querySelectorAll('.overlay').forEach(o=>o.addEventListener('click',e=>{if(e.target===o)fecharDrawer(o.id);}));

function fmt(n){return'R$ '+(n||0).toLocaleString('pt-BR',{minimumFractionDigits:2,maximumFractionDigits:2});}

function toast(msg,dur=3000){
  const t=document.getElementById('toast');
  t.textContent=msg;t.style.display='block';
  clearTimeout(t._t);t._t=setTimeout(()=>t.style.display='none',dur);
}


// ── Importar XLS ──────────────────────────────────────────────
let importData = [];

function initImportar() {
  document.getElementById('import-preview').style.display = 'none';
  document.getElementById('import-resultado').style.display = 'none';
}

function handleDrop(e) {
  e.preventDefault();
  document.getElementById('drop-zone').style.borderColor = 'var(--border2)';
  const file = e.dataTransfer.files[0];
  if (file) processarXLS(file);
}

function processarXLS(file) {
  if (!file) return;
  const reader = new FileReader();
  reader.onload = function(e) {
    try {
      const wb = XLSX.read(e.target.result, {type:'array', cellDates:true});
      const ws = wb.Sheets[wb.SheetNames[0]];
      const rows = XLSX.utils.sheet_to_json(ws, {defval:''});
      
      if (!rows.length) { toast('Arquivo vazio ou formato não reconhecido'); return; }
      
      // Detect header mapping (SHARMAQ export columns)
      const sample = rows[0];
      const keys = Object.keys(sample);
      console.log('Columns found:', keys);
      
      // Map SHARMAQ columns to our fields
      const mapCol = (row, candidates) => {
        for (const c of candidates) {
          if (row[c] !== undefined && row[c] !== '') return row[c];
        }
        return '';
      };

      importData = rows
        .filter(r => mapCol(r, ['OS Nº','OS','Os Nº','OS N','OS №']) !== '')
        .map(r => {
          const osNum   = mapCol(r, ['OS Nº','OS','Os Nº','OS N','OS №']);
          const modelo  = mapCol(r, ['MODELO','Modelo','modelo']);
          const defeito = mapCol(r, ['Defeito','DEFEITO','defeito']);
          const sit     = mapCol(r, ['Situação','Situacao','SITUAÇÃO','situação','Situacão']);
          const entrada = mapCol(r, ['Entrada','ENTRADA','entrada','Data entrada']);
          const atend   = mapCol(r, ['ATENDENTE','Atendente','atendente']);
          const tecnico = mapCol(r, ['Técnico Resp.','Tecnico Resp.','Técnico','tecnico']);
          const vlrServ = parseFloat(mapCol(r, ['Vlr Serv.','Total','vlr serv','Valor Serv'])) || 0;
          const vlrPecas= parseFloat(mapCol(r, ['Vlr Peças','Vlr Pecas','vlr pecas'])) || 0;
          const aparelho= mapCol(r, ['Aparelho','APARELHO','aparelho','Marca']);
          
          // Format date
          let dataEntrada = '';
          if (entrada) {
            try {
              const d = entrada instanceof Date ? entrada : new Date(entrada);
              if (!isNaN(d)) dataEntrada = d.toLocaleDateString('pt-BR');
              else dataEntrada = String(entrada).split(' ')[0];
            } catch(e) { dataEntrada = String(entrada); }
          }

          // Determine status vs existing OS
          const existing = osCache.find(x => String(x.os) === String(osNum));
          const status = existing ? 'atualizar' : 'novo';

          return {
            os: String(osNum),
            modelo: modelo || aparelho || '—',
            defeito: defeito || '—',
            situacao: sit || 'Autorizado, Reparo em andamento',
            dataEntrada,
            atendente: atend || tecnico || '',
            vlrServ, vlrPecas,
            status,
            existing
          };
        });

      if (!importData.length) { toast('Nenhuma OS encontrada no arquivo'); return; }

      // Show preview
      const novos = importData.filter(r=>r.status==='novo').length;
      const atualizar = importData.filter(r=>r.status==='atualizar').length;
      document.getElementById('import-summary').innerHTML = 
        `<span style="color:var(--green)">✓ ${novos} novas OS</span>&nbsp;&nbsp;` +
        `<span style="color:var(--blue)">↻ ${atualizar} para atualizar</span>&nbsp;&nbsp;` +
        `<span style="color:var(--text2)">Total: ${importData.length}</span>`;
      document.getElementById('import-count').textContent = importData.length;

      const rows_html = importData.map(r => {
        const badge = r.status==='novo' 
          ? '<span class="badge b-green">Nova</span>'
          : '<span class="badge b-blue">Atualizar</span>';
        return `<tr>
          <td style="font-size:11px;color:var(--text2)">${r.os}</td>
          <td style="font-size:12px">${r.modelo}</td>
          <td style="font-size:11px;color:var(--text3)">${r.defeito}</td>
          <td>${sitBadge(r.situacao)}</td>
          <td style="font-size:11px">${r.dataEntrada}</td>
          <td style="font-size:11px">${r.atendente}</td>
          <td style="font-size:11px">${r.vlrServ>0?fmt(r.vlrServ):'—'}</td>
          <td>${badge}</td>
        </tr>`;
      }).join('');

      document.getElementById('import-tbody').innerHTML = rows_html;
      document.getElementById('import-preview').style.display = 'block';
      document.getElementById('import-resultado').style.display = 'none';

    } catch(err) {
      console.error(err);
      toast('Erro ao ler arquivo: ' + err.message);
    }
  };
  reader.readAsArrayBuffer(file);
}

async function confirmarImport() {
  if (!importData.length) return;
  const btn = event.target;
  btn.disabled = true;
  btn.textContent = 'Importando...';

  let ok = 0, erros = 0;
  for (const r of importData) {
    try {
      let res;
      if (r.status === 'novo') {
        res = await apiPost({
          acao: 'lancarOS', unidade,
          os: r.os, modelo: r.modelo, defeito: r.defeito,
          situacao: r.situacao, venda: r.vlrServ,
          pecasCapital: r.vlrPecas, pecasBase: r.vlrPecas,
          obs: ''
        });
      } else {
        res = await apiPost({
          acao: 'atualizarOS', unidade,
          os: r.os, situacao: r.situacao,
          venda: r.vlrServ > 0 ? r.vlrServ : undefined,
          pecasCapital: r.vlrPecas > 0 ? r.vlrPecas : undefined,
          pecasBase: r.vlrPecas > 0 ? r.vlrPecas : undefined
        });
      }
      if (res.ok) ok++; else erros++;
    } catch(e) { erros++; }
  }

  btn.disabled = false;
  btn.textContent = 'Importar';

  const resultMsg = `<div style="padding:14px;border-radius:var(--radius-sm);background:${erros===0?'var(--green-bg)':'var(--amber-bg)'};border:0.5px solid ${erros===0?'#9FE1CB':'#FAC775'}">
    <div style="font-weight:600;color:${erros===0?'var(--green-txt)':'var(--amber-txt)'}">
      ${erros===0?'✓ Importação concluída!':'⚠ Importação com erros'}
    </div>
    <div style="font-size:12px;margin-top:4px;color:${erros===0?'var(--green-txt)':'var(--amber-txt)'}">
      ${ok} OS importadas com sucesso${erros>0?' · '+erros+' com erro':''}.
    </div>
  </div>`;

  document.getElementById('import-resultado').innerHTML = resultMsg;
  document.getElementById('import-resultado').style.display = 'block';
  document.getElementById('import-preview').style.display = 'none';
  importData = [];

  if (ok > 0) { await carregarOS(); }
}

function cancelarImport() {
  importData = [];
  document.getElementById('import-preview').style.display = 'none';
  document.getElementById('xls-input').value = '';
}

// ── Modo DEMO (sem SCRIPT_URL) ────────────────────────────────
if (SCRIPT_URL==='COLE_SUA_URL_AQUI') {
  window.apiGet = async(p)=>demoGet(p);
  window.apiPost = async(b)=>demoPost(b);
}

// Dados demo
const DEMO_CENTRO = [
  {row:2,os:74695,modelo:'G31 Azul',defeito:'Botões externos',situacao:'Autorizado, Reparo em andamento',dataEntrada:'13/06/2025',formaPagto:'Pix',venda:165,taxaPct:0,pecasCapital:28.31,pecasBase:28.31,comissao:4.95,emCaixa:131.74,cell50:65.87,help50:65.87,retirou:false,dataRetirada:'',obs:'Saiu',cobrado:false,diasAberto:8,cobradoPorPrazo:false,liberadoParaCobranca:false},
  {row:3,os:74697,modelo:'iPhone 11 Lilás',defeito:'Tampa traseira',situacao:'Autorizado, Reparo em andamento',dataEntrada:'13/06/2025',formaPagto:'Crédito',venda:395,taxaPct:0.10,pecasCapital:25,pecasBase:25,comissao:11.85,emCaixa:322.65,cell50:161.33,help50:161.33,retirou:true,dataRetirada:'15/06/2025',obs:'Saiu',cobrado:false,diasAberto:8,cobradoPorPrazo:false,liberadoParaCobranca:true},
  {row:4,os:73993,modelo:'Poco X5 5G Preto',defeito:'Módulo completo',situacao:'Pronto, avisar cliente',dataEntrada:'01/06/2025',formaPagto:'',venda:0,taxaPct:0,pecasCapital:200,pecasBase:200,comissao:0,emCaixa:0,cell50:0,help50:0,retirou:false,dataRetirada:'',obs:'Ainda na loja',cobrado:false,diasAberto:20,cobradoPorPrazo:false,liberadoParaCobranca:false},
  {row:5,os:73761,modelo:'Redmi A3 Preto',defeito:'Não liga',situacao:'Pronto, avisar cliente',dataEntrada:'28/05/2025',formaPagto:'',venda:0,taxaPct:0,pecasCapital:75,pecasBase:75,comissao:0,emCaixa:0,cell50:0,help50:0,retirou:false,dataRetirada:'',obs:'',cobrado:false,diasAberto:16,cobradoPorPrazo:false,liberadoParaCobranca:false},
  {row:6,os:74727,modelo:'G75 Azul Claro',defeito:'Microfone chiando',situacao:'Pronto, avisar cliente',dataEntrada:'10/06/2025',formaPagto:'',venda:0,taxaPct:0,pecasCapital:30,pecasBase:30,comissao:0,emCaixa:0,cell50:0,help50:0,retirou:false,dataRetirada:'',obs:'',cobrado:false,diasAberto:11,cobradoPorPrazo:false,liberadoParaCobranca:false},
];
const DEMO_SHOPPING = [
  {row:2,os:74724,modelo:'iPhone 16 Preto',defeito:'Módulo completo',situacao:'Equipamento entregue reparado',dataEntrada:'05/06/2025',formaPagto:'Crédito',venda:1995,taxaPct:0.10,pecasCapital:455,pecasBase:455,comissao:53.87,emCaixa:1286.63,cell50:643.32,help50:643.32,retirou:false,dataRetirada:'',obs:'Saiu',cobrado:false,diasAberto:16,cobradoPorPrazo:false,liberadoParaCobranca:false},
  {row:3,os:74670,modelo:'A06 Azul Escuro',defeito:'Módulo completo',situacao:'Pronto, avisar cliente',dataEntrada:'28/05/2025',formaPagto:'',venda:0,taxaPct:0,pecasCapital:110,pecasBase:110,comissao:0,emCaixa:0,cell50:0,help50:0,retirou:false,dataRetirada:'',obs:'',cobrado:false,diasAberto:24,cobradoPorPrazo:false,liberadoParaCobranca:false},
  {row:4,os:74701,modelo:'iPhone 11 Pro Max Verde',defeito:'Não liga',situacao:'Pronto, avisar cliente',dataEntrada:'02/06/2025',formaPagto:'',venda:0,taxaPct:0,pecasCapital:0,pecasBase:0,comissao:0,emCaixa:0,cell50:0,help50:0,retirou:false,dataRetirada:'',obs:'',cobrado:false,diasAberto:19,cobradoPorPrazo:false,liberadoParaCobranca:false},
];

function demoGet(p) {
  const u=p.unidade||'Centro', a=p.acao;
  const dados = u==='Shopping'?DEMO_SHOPPING:DEMO_CENTRO;
  if(a==='listarOS') return {ok:true,dados};
  if(a==='getCapital') return {ok:true,total:2112.48,pecasUsadas:358.31,saldo:1754.17,entradas:[{data:'26/02/2025',valor:1104,desc:'Transferência fev'},{data:'30/03/2025',valor:918.74,desc:'Transferência mar'},{data:'30/04/2025',valor:89.74,desc:'Transferência abr'}]};
  if(a==='getResumo'){
    let totFat=0,totPecas=0,totCom=0,totCaixa=0,totCell=0,qAberta=0,qPronta=0,qEntregue=0,qLiberada=0;
    dados.forEach(r=>{totFat+=r.venda;totPecas+=r.pecasBase;totCom+=r.comissao;totCaixa+=r.emCaixa;totCell+=r.cell50;
      if(r.situacao.includes('Pronto'))qPronta++;else if(r.situacao.includes('entregue'))qEntregue++;else qAberta++;
      if(r.liberadoParaCobranca&&!r.cobrado)qLiberada++;});
    return {ok:true,totFat,totPecas,totCom,totCaixa,totCell,qAberta,qPronta,qEntregue,qLiberada,capitalSaldo:1754.17};
  }
  if(a==='getFechamentos') return {ok:true,dados:[{periodo:'05/2025',dataGerado:'22/05/2025 18:00',fat:2100,pecas:380,com:51.6,caixa:1668.4,cell:834.2,help:834.2}]};
  return {ok:true};
}
function demoPost(b){
  if(b.acao==='marcarRetirada'){
    const d=b.unidade==='Shopping'?DEMO_SHOPPING:DEMO_CENTRO;
    const r=d.find(x=>x.os==b.os);
    if(r){r.retirou=true;r.dataRetirada=new Date().toLocaleDateString('pt-BR');r.situacao='Equipamento entregue reparado';r.liberadoParaCobranca=true;}
    return {ok:true,mensagem:'Retirada registrada para OS '+b.os};
  }
  if(b.acao==='lancarOS'){
    const d=b.unidade==='Shopping'?DEMO_SHOPPING:DEMO_CENTRO;
    const rec=b.venda-(b.venda*b.taxaPct),com=rec*0.03,caixa=rec-b.pecasBase-com;
    d.push({os:b.os,modelo:b.modelo,defeito:b.defeito,situacao:b.situacao,venda:b.venda,taxaPct:b.taxaPct,pecasCapital:b.pecasCapital,pecasBase:b.pecasBase,comissao:com,emCaixa:caixa,cell50:caixa*0.5,help50:caixa*0.5,retirou:false,diasAberto:0,obs:b.obs,cobrado:false,liberadoParaCobranca:false});
    return {ok:true,mensagem:'OS '+b.os+' lançada com sucesso.'};
  }
  if(b.acao==='atualizarOS'){
    const d=b.unidade==='Shopping'?DEMO_SHOPPING:DEMO_CENTRO;
    const r=d.find(x=>x.os==b.os);
    if(r){Object.assign(r,{venda:b.venda,taxaPct:b.taxaPct,pecasCapital:b.pecasCapital,pecasBase:b.pecasBase,situacao:b.situacao,obs:b.obs});
      const rec=b.venda-(b.venda*b.taxaPct),com=rec*0.03,caixa=rec-(b.pecasBase||b.pecasCapital)-com;
      r.comissao=com;r.emCaixa=caixa;r.cell50=caixa*0.5;r.help50=caixa*0.5;}
    return {ok:true,mensagem:'OS '+b.os+' atualizada.'};
  }
  if(b.acao==='gerarFechamento') return {ok:true,mensagem:'Fechamento gerado — demo.'};
  if(b.acao==='lancarCapital') return {ok:true,mensagem:'Capital R$ '+b.valor+' registrado.'};
  return {ok:true,mensagem:'OK (demo)'};
}
</script>
<script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js"></script>
</body>
</html>
