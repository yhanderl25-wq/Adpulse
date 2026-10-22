<!DOCTYPE html>
<html lang="pt-BR" class="dark">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>AdPulse | Intelligence & Performance Dashboard</title>

<!-- PWA Meta Tags -->
<meta name="theme-color" content="#030712">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="AdPulse">
<link rel="apple-touch-icon" href="https://cdn-icons-png.flaticon.com/512/1055/1055687.png">
<link rel="manifest" href='data:application/manifest+json,{"name":"AdPulse Performance","short_name":"AdPulse","start_url":".","display":"standalone","background_color":"#030712","theme_color":"#030712","icons":[{"src":"https://cdn-icons-png.flaticon.com/512/1055/1055687.png","sizes":"512x512","type":"image/png"}]}'>

<!-- Fonts & Frameworks -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<!-- Meta Ads SDK -->
<script async defer crossorigin="anonymous" src="https://connect.facebook.net/pt_BR/sdk.js"></script>

<script>
  tailwind.config = {
    darkMode: 'class',
    theme: {
      extend: {
        fontFamily: { sans: ['Plus Jakarta Sans', 'sans-serif'] },
        colors: {
          brand: { 500: '#6366f1', 600: '#4f46e5', 700: '#4338ca' },
          dark: { 950: '#030712', 900: '#0b0f19', 800: '#151c2c', 700: '#222f46' }
        }
      }
    }
  }
</script>

<style>
  body {
    -webkit-tap-highlight-color: transparent;
    user-select: none;
    background-color: #030712;
    background-image: 
      radial-gradient(circle at 10% 0%, rgba(99, 102, 241, 0.12) 0%, transparent 35%),
      radial-gradient(circle at 90% 100%, rgba(139, 92, 246, 0.08) 0%, transparent 40%);
    background-attachment: fixed;
  }
  
  .hide-scrollbar::-webkit-scrollbar { display: none; }
  .hide-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
  
  .glass-card {
    background: rgba(15, 23, 42, 0.65);
    backdrop-filter: blur(16px);
    -webkit-backdrop-filter: blur(16px);
    border: 1px solid rgba(255, 255, 255, 0.07);
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
  }

  .glass-card-hover {
    transition: all 0.25s cubic-bezier(0.16, 1, 0.3, 1);
  }
  .glass-card-hover:hover {
    border-color: rgba(99, 102, 241, 0.4);
    transform: translateY(-2px);
    box-shadow: 0 12px 40px 0 rgba(99, 102, 241, 0.15);
  }

  .btn-primary {
    background: linear-gradient(135deg, #6366f1 0%, #4f46e5 100%);
    box-shadow: 0 4px 20px -2px rgba(99, 102, 241, 0.45);
    transition: all 0.2s cubic-bezier(0.16, 1, 0.3, 1);
  }
  .btn-primary:active { transform: scale(0.97); }

  .btn-danger {
    background: linear-gradient(135deg, #f43f5e 0%, #e11d48 100%);
    box-shadow: 0 4px 18px -2px rgba(244, 63, 94, 0.35);
    transition: all 0.2s cubic-bezier(0.16, 1, 0.3, 1);
  }
  .btn-danger:active { transform: scale(0.97); }

  .fade-in { animation: fadeIn 0.35s cubic-bezier(0.16, 1, 0.3, 1) both; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: none; } }

  .badge-chip {
    font-size: 10px;
    font-weight: 700;
    padding: 3px 9px;
    border-radius: 9999px;
    border: 1px solid;
    display: inline-flex;
    align-items: center;
    gap: 5px;
    letter-spacing: 0.02em;
  }
</style>
</head>
<body class="text-slate-100 font-sans antialiased min-h-screen flex flex-col pb-24">

<!-- HEADER PRINCIPAL -->
<header class="bg-dark-900/70 backdrop-blur-xl border-b border-white/5 sticky top-0 z-40 px-4 py-3.5 flex items-center justify-between">
  <div class="flex items-center gap-3">
    <div class="w-10 h-10 rounded-2xl bg-gradient-to-tr from-indigo-500 via-indigo-600 to-purple-600 flex items-center justify-center text-white shadow-lg shadow-indigo-500/30 border border-white/10">
      <i class="fa-solid fa-chart-line text-base"></i>
    </div>
    <div>
      <h1 class="font-extrabold text-sm tracking-tight text-white flex items-center gap-2">
        AdPulse <span class="text-[9px] font-black uppercase px-2 py-0.5 rounded-full bg-gradient-to-r from-indigo-500/20 to-purple-500/20 text-indigo-300 border border-indigo-500/30">PRO</span>
      </h1>
      <p id="account-status-badge" class="text-[10px] text-slate-400 font-medium flex items-center gap-1.5 mt-0.5">
        <span class="w-2 h-2 rounded-full bg-slate-600"></span> Nenhuma conta vinculada
      </p>
    </div>
  </div>
  <div class="flex items-center gap-2">
    <button onclick="shareReport()" class="p-2.5 rounded-2xl bg-dark-800/80 border border-white/10 text-emerald-400 hover:bg-dark-700 transition active:scale-95 shadow-md" title="Exportar Relatório via WhatsApp">
      <i class="fa-brands fa-whatsapp text-base"></i>
    </button>
    <button onclick="installPWA()" class="px-3.5 py-2 rounded-2xl bg-indigo-500/10 border border-indigo-500/30 text-indigo-300 text-xs font-bold flex items-center gap-1.5 hover:bg-indigo-500/20 transition active:scale-95 shadow-md">
      <i class="fa-solid fa-download text-[11px]"></i> App
    </button>
  </div>
</header>

<main class="max-w-md mx-auto w-full px-4 pt-4 flex-1">

  <!-- FILTRO DE JANELA TEMPORAL -->
  <div class="flex justify-between items-center mb-4 bg-dark-900/80 backdrop-blur-md p-1.5 rounded-2xl border border-white/5 shadow-inner">
    <span class="text-[10px] font-extrabold text-slate-400 pl-3 uppercase tracking-wider">Janela de Análise</span>
    <div class="flex gap-1">
      <button onclick="setTimeframe('7d')" id="tf-7d" class="text-[11px] font-bold px-3.5 py-1.5 rounded-xl transition bg-brand-500 text-white shadow-lg shadow-indigo-500/25">Últimos 7d</button>
      <button onclick="setTimeframe('30d')" id="tf-30d" class="text-[11px] font-bold px-3.5 py-1.5 rounded-xl transition text-slate-400 hover:text-slate-200">Últimos 30d</button>
    </div>
  </div>

  <!-- RAIO-X DE PERFORMANCE -->
  <div id="raiox" class="glass-card rounded-3xl p-4 mb-5 fade-in relative overflow-hidden"></div>

  <!-- ABA: CAMPANHAS (INÍCIO) -->
  <section id="sec-home" class="space-y-3.5">
    <div class="flex items-center justify-between px-0.5">
      <h2 class="text-xs font-bold text-slate-300 uppercase tracking-wider flex items-center gap-2">
        <i class="fa-solid fa-layer-group text-brand-500"></i> Campanhas em Monitoramento
      </h2>
      <span class="text-[10px] text-slate-500 font-medium" id="last-sync">—</span>
    </div>
    <div id="campaign-list" class="space-y-3.5"></div>
  </section>

  <!-- ABA: CRIATIVOS -->
  <section id="sec-creatives" class="hidden space-y-3.5">
    <div class="flex items-center justify-between px-0.5">
      <h2 class="text-xs font-bold text-slate-300 uppercase tracking-wider flex items-center gap-2">
        <i class="fa-solid fa-photo-film text-brand-500"></i> Performance por Criativo
      </h2>
    </div>
    <div id="filters" class="flex gap-2 overflow-x-auto hide-scrollbar pb-1"></div>
    <div id="creative-grid" class="grid grid-cols-2 gap-3"></div>
  </section>

  <!-- ABA: DIAGNÓSTICO -->
  <section id="sec-recommendations" class="hidden space-y-3.5">
    <h2 class="text-xs font-bold text-slate-300 uppercase tracking-wider flex items-center gap-2 px-0.5">
      <i class="fa-solid fa-sliders text-amber-400"></i> Métrica & Parâmetros de Eficiência
    </h2>
    <div id="criteria" class="space-y-2.5"></div>
  </section>

  <!-- ABA: CONEXÕES -->
  <section id="sec-connect" class="hidden space-y-3.5">
    <div class="glass-card rounded-3xl p-6 text-center">
      <div class="w-14 h-14 rounded-2xl bg-indigo-500/10 border border-indigo-500/30 flex items-center justify-center text-brand-500 mx-auto mb-3 text-2xl shadow-lg shadow-indigo-500/10">
        <i class="fa-solid fa-link"></i>
      </div>
      <h3 class="text-base font-bold text-slate-100">Integração de Contas de Anúncios</h3>
      <p class="text-xs text-slate-400 mt-1.5 leading-relaxed">Conecte sua conta do Meta Ads para sincronização automática de métricas via SDK Oficial.</p>
    </div>
    <div id="platform-cards" class="space-y-3.5"></div>
  </section>

  <!-- ABA: ASSINATURA -->
  <section id="sec-plan" class="hidden space-y-3.5">
    <div class="glass-card rounded-3xl p-6 text-center border-indigo-500/30 relative overflow-hidden">
      <div class="absolute -right-12 -top-12 w-36 h-36 bg-indigo-500/20 rounded-full blur-3xl"></div>
      <span class="bg-gradient-to-r from-indigo-500 to-purple-500 text-white text-[10px] font-black px-3.5 py-1 rounded-full uppercase tracking-wider shadow-lg shadow-indigo-500/30">
        Plano Gestor Profissional
      </span>
      <h3 class="text-3xl font-black text-white mt-5">R$ 180 <span class="text-xs font-semibold text-slate-400">/ trimestre</span></h3>
      <p class="text-xs text-slate-400 mt-1">Diagnóstico inteligente contínuo para evitar desperdício de verba.</p>
      <ul class="text-left text-xs text-slate-300 mt-6 space-y-3 border-t border-white/5 pt-5">
        <li class="flex items-center gap-3"><i class="fa-solid fa-circle-check text-emerald-400 text-sm"></i> Monitoramento unificado Meta Ads</li>
        <li class="flex items-center gap-3"><i class="fa-solid fa-circle-check text-emerald-400 text-sm"></i> Detecção automática de Fadiga de Anúncio</li>
        <li class="flex items-center gap-3"><i class="fa-solid fa-circle-check text-emerald-400 text-sm"></i> Interrupção de campanhas em 1 clique</li>
      </ul>
      <button onclick="toast('Redirecionando para o checkout seguro...')" class="btn-primary w-full text-white font-bold py-3.5 rounded-2xl text-xs mt-6">
        Ativar Licença de Gestão
      </button>
    </div>
  </section>
</main>

<!-- MODAL CONFIRMAÇÃO -->
<div id="modal" class="hidden fixed inset-0 z-[60] bg-black/80 backdrop-blur-md flex items-end sm:items-center justify-center p-4">
  <div class="glass-card rounded-3xl p-6 w-full max-w-md fade-in border-white/10">
    <h4 id="modal-title" class="font-bold text-base text-slate-100"></h4>
    <p id="modal-text" class="text-xs text-slate-400 mt-2 leading-relaxed"></p>
    <div class="grid grid-cols-2 gap-3 mt-6">
      <button onclick="closeModal()" class="py-3 rounded-2xl bg-dark-800 text-slate-300 font-bold text-xs hover:bg-dark-700 transition active:scale-95">Cancelar</button>
      <button id="modal-confirm" class="btn-danger text-white font-bold py-3 rounded-2xl text-xs">Confirmar Interrupção</button>
    </div>
  </div>
</div>

<!-- TOAST NOTIFICATION -->
<div id="toast" class="hidden fixed top-5 left-1/2 -translate-x-1/2 z-[70] bg-dark-900 border border-indigo-500/40 text-xs text-slate-100 px-4 py-3 rounded-2xl shadow-2xl fade-in flex items-center gap-2.5 backdrop-blur-xl">
  <i class="fa-solid fa-circle-info text-indigo-400 text-sm"></i> <span id="toast-msg"></span>
</div>

<!-- NAVEGAÇÃO INFERIOR PWA -->
<nav class="fixed bottom-0 left-0 right-0 bg-dark-900/80 backdrop-blur-xl border-t border-white/5 z-50">
  <div class="max-w-md mx-auto flex justify-around items-center h-16 px-2">
    <button onclick="switchTab('home')" data-tab="home" class="nav-btn flex flex-col items-center gap-1 w-full py-1 text-brand-500 transition"><i class="fa-solid fa-chart-simple text-sm"></i><span class="text-[9px] font-bold">Painel</span></button>
    <button onclick="switchTab('creatives')" data-tab="creatives" class="nav-btn flex flex-col items-center gap-1 w-full py-1 text-slate-500 transition"><i class="fa-solid fa-photo-film text-sm"></i><span class="text-[9px] font-bold">Criativos</span></button>
    <button onclick="switchTab('recommendations')" data-tab="recommendations" class="nav-btn flex flex-col items-center gap-1 w-full py-1 text-slate-500 transition"><i class="fa-solid fa-sliders text-sm"></i><span class="text-[9px] font-bold">Métricas</span></button>
    <button onclick="switchTab('connect')" data-tab="connect" class="nav-btn flex flex-col items-center gap-1 w-full py-1 text-slate-500 transition"><i class="fa-solid fa-plug text-sm"></i><span class="text-[9px] font-bold">Conexão</span></button>
    <button onclick="switchTab('plan')" data-tab="plan" class="nav-btn flex flex-col items-center gap-1 w-full py-1 text-slate-500 transition"><i class="fa-solid fa-gem text-sm"></i><span class="text-[9px] font-bold">Plano</span></button>
  </div>
</nav>

<script>
// CONFIGURAÇÃO DO META DEVELOPERS
const META_APP_ID = "1768169631131383";

window.fbAsyncInit = function() {
  FB.init({
    appId      : META_APP_ID,
    cookie     : true,
    xfbml      : true,
    version    : 'v18.0'
  });
};

const fmtBRL = v => v.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
const fmtPct = v => (v).toLocaleString('pt-BR', { minimumFractionDigits: 1, maximumFractionDigits: 1 }) + '%';
const fmtX   = v => v.toLocaleString('pt-BR', { minimumFractionDigits: 1, maximumFractionDigits: 1 }) + 'x';

const RULES = {
  ctr:  { good: 2.0, bad: 1.0 },
  cpc:  { good: 1.0, bad: 2.5 },
  freq: { good: 2.5, bad: 3.5 },
  roas: { good: 3.0, bad: 1.0 }
};

const TONE = {
  good: { text: 'text-emerald-400', border: 'border-emerald-500/30', bg: 'bg-emerald-500/10' },
  warn: { text: 'text-amber-400',   border: 'border-amber-500/30',   bg: 'bg-amber-500/10' },
  bad:  { text: 'text-rose-400',    border: 'border-rose-500/30',    bg: 'bg-rose-500/10' }
};

function diagCTR(v)  { return v > RULES.ctr.good ? { label: 'CTR Excelente', tone: 'good' } : v < RULES.ctr.bad  ? { label: 'CTR Baixo', tone: 'bad' } : { label: 'CTR Regular', tone: 'warn' }; }
function diagCPC(v)  { return v < RULES.cpc.good ? { label: 'CPC Otimizado', tone: 'good' } : v > RULES.cpc.bad  ? { label: 'CPC Elevado', tone: 'bad' } : { label: 'CPC Médio', tone: 'warn' }; }
function diagFreq(v) { return v > RULES.freq.bad  ? { label: 'Fadiga Alta', tone: 'bad' } : v > RULES.freq.good ? { label: 'Atenção Frequência', tone: 'warn' } : { label: 'Frequência Ideal', tone: 'good' }; }
function diagROAS(v){ return v >= RULES.roas.good ? { label: 'Escala Recomendada', tone: 'good' } : v < RULES.roas.bad  ? { label: 'Incompatível / Fuga', tone: 'bad' } : { label: 'Em Análise', tone: 'warn' }; }

function verdict(c) {
  const r = diagROAS(c.roas);
  if (r.tone === 'bad') return { kind: 'fuga', label: 'Desperdício de Verba', icon: 'fa-triangle-exclamation', tone: 'bad' };
  if (diagFreq(c.frequency).tone === 'bad') return { kind: 'fadiga', label: 'Fadiga de Criativo', icon: 'fa-rotate', tone: 'warn' };
  if (r.tone === 'good') return { kind: 'campea', label: 'Campanha de Alta Performance', icon: 'fa-circle-check', tone: 'good' };
  return { kind: 'ajuste', label: 'Desempenho Moderado', icon: 'fa-sliders', tone: 'warn' };
}

const STORE_KEY = 'adpulse_state_v3';
const state = loadState();
let currentTF = '7d';

function loadState() {
  try {
    const s = JSON.parse(localStorage.getItem(STORE_KEY));
    if (s) return s;
  } catch (e) {}
  return { paused: [], connected: { meta: false }, metaToken: null };
}
function saveState() { localStorage.setItem(STORE_KEY, JSON.stringify(state)); }

const API = {
  async fetchCampaigns() {
    if (state.connected.meta && state.metaToken) {
      try {
        return await fetchRealMetaCampaigns(state.metaToken);
      } catch(e) { console.error("Erro ao carregar dados do Meta:", e); }
    }
    const mult = currentTF === '30d' ? 3.5 : 1.0;
    return MOCK.campaigns
      .filter(c => state.connected[c.platform])
      .map(c => ({ ...c, spend: Math.round(c.spend * mult) }));
  },
  async fetchCreatives() {
    return MOCK.creatives.filter(c => state.connected[c.platform]);
  },
  async pauseCampaign(id) {
    if (state.metaToken) {
      await fetch(`https://graph.facebook.com/v18.0/${id}?status=PAUSED&access_token=${state.metaToken}`, { method: 'POST' });
    }
    return { ok: true, id };
  }
};

async function fetchRealMetaCampaigns(token) {
  const meRes = await fetch(`https://graph.facebook.com/v18.0/me/adaccounts?fields=name,account_id&access_token=${token}`);
  const meData = await meRes.json();
  if (!meData.data || !meData.data.length) return [];

  const adAccountId = meData.data[0].id;
  const url = `https://graph.facebook.com/v18.0/${adAccountId}/campaigns?fields=name,objective,status,insights{spend,clicks,cpc,ctr,frequency,purchase_roas}&access_token=${token}`;
  
  const res = await fetch(url);
  const data = await res.json();

  return (data.data || []).map(c => {
    const insights = (c.insights && c.insights.data[0]) || {};
    const spend = parseFloat(insights.spend || 0);
    const ctr = parseFloat(insights.ctr || 0);
    const cpc = parseFloat(insights.cpc || 0);
    const frequency = parseFloat(insights.frequency || 1.0);
    let roas = (insights.purchase_roas && insights.purchase_roas[0]) ? parseFloat(insights.purchase_roas[0].value || 0) : 0;

    return {
      id: c.id,
      platform: 'meta',
      name: c.name,
      objective: c.objective || 'Mensagens',
      spend: Math.round(spend),
      roas: parseFloat(roas.toFixed(1)),
      ctr: parseFloat(ctr.toFixed(1)),
      cpc: parseFloat(cpc.toFixed(2)),
      frequency: parseFloat(frequency.toFixed(1))
    };
  });
}

const MOCK = {
  campaigns: [
    { id: 'mc1', platform: 'meta', name: 'Oferta Especial - Raio 10km', objective: 'Mensagens / WhatsApp', spend: 890, roas: 4.6, ctr: 3.2, cpc: 0.62, frequency: 1.7 },
    { id: 'mc2', platform: 'meta', name: 'Tráfego Aberto - Institucional', objective: 'Tráfego', spend: 380, roas: 0.5, ctr: 0.7, cpc: 2.90, frequency: 3.8 }
  ],
  creatives: [
    { id: 'cr1', platform: 'meta', format: 'reel', title: 'Vídeo Demonstrativo Produto', ctr: 3.6, roas: 4.6, frequency: 1.7 },
    { id: 'cr2', platform: 'meta', format: 'carrossel', title: 'Carrossel Desconto Exclusivo', ctr: 1.1, roas: 1.6, frequency: 3.8 }
  ]
};

const FORMATS = { all: 'Todos', reel: 'Reels / Vídeo', carrossel: 'Carrossel', imagem: 'Imagem Estática' };
const FORMAT_STYLE = { reel: 'text-purple-400 bg-purple-500/10 border-purple-500/20', carrossel: 'text-indigo-400 bg-indigo-500/10 border-indigo-500/20', imagem: 'text-sky-400 bg-sky-500/10 border-sky-500/20' };
const PLATFORM = { meta: { name: 'Meta Ads', icon: 'fa-brands fa-facebook', color: 'text-blue-400', btn: 'Conectar Meta Ads' } };

function toast(msg) {
  const t = document.getElementById('toast');
  document.getElementById('toast-msg').textContent = msg;
  t.classList.remove('hidden');
  clearTimeout(toast._t);
  toast._t = setTimeout(() => t.classList.add('hidden'), 2800);
}

function kpi(label, value, diag) {
  return `<div class="bg-dark-950/90 p-2.5 rounded-2xl border border-white/5 text-center">
    <span class="text-[9px] text-slate-400 font-bold block uppercase tracking-wider">${label}</span>
    <span class="text-xs font-black ${TONE[diag.tone].text} mt-0.5 block">${value}</span>
    <span class="text-[8px] font-semibold ${TONE[diag.tone].text} mt-0.5 block">${diag.label}</span>
  </div>`;
}

function setTimeframe(tf) {
  currentTF = tf;
  document.getElementById('tf-7d').className = `text-[11px] font-bold px-3.5 py-1.5 rounded-xl transition ${tf === '7d' ? 'bg-brand-500 text-white shadow-lg shadow-indigo-500/25' : 'text-slate-400 hover:text-slate-200'}`;
  document.getElementById('tf-30d').className = `text-[11px] font-bold px-3.5 py-1.5 rounded-xl transition ${tf === '30d' ? 'bg-brand-500 text-white shadow-lg shadow-indigo-500/25' : 'text-slate-400 hover:text-slate-200'}`;
  refresh();
}

function renderRaioX(campaigns) {
  const activeCamp = campaigns.filter(c => !state.paused.includes(c.id));
  const spend = activeCamp.reduce((s, c) => s + c.spend, 0);
  const roas = activeCamp.length ? activeCamp.reduce((s, c) => s + c.roas * c.spend, 0) / (spend || 1) : 0;
  
  const pausedCamp = campaigns.filter(c => state.paused.includes(c.id));
  const savedSpend = pausedCamp.reduce((s, c) => s + c.spend, 0);

  let healthScore = 100;
  campaigns.forEach(c => {
    if (!state.paused.includes(c.id)) {
      if (verdict(c).kind === 'fuga') healthScore -= 30;
      if (verdict(c).kind === 'fadiga') healthScore -= 15;
    }
  });
  healthScore = Math.max(20, healthScore);
  const scoreTone = healthScore >= 80 ? 'good' : healthScore >= 50 ? 'warn' : 'bad';

  document.getElementById('raiox').innerHTML = `
    <div class="flex justify-between items-center mb-3">
      <span class="text-[11px] font-extrabold uppercase tracking-wider text-slate-300 flex items-center gap-2">
        <i class="fa-solid fa-wallet text-brand-500"></i> Visão Geral de Investimento
      </span>
      <span class="${TONE[scoreTone].bg} ${TONE[scoreTone].text} border ${TONE[scoreTone].border} text-[10px] px-2.5 py-0.5 rounded-full font-bold">
        Índice de Saúde: ${healthScore}/100
      </span>
    </div>
    <div class="grid grid-cols-2 gap-2.5 bg-dark-950/80 p-3.5 rounded-2xl border border-white/5">
      <div>
        <span class="text-[10px] text-slate-400 font-semibold block uppercase tracking-wider">Verba Empregada</span>
        <span class="text-lg font-black text-white mt-0.5 block tracking-tight">${fmtBRL(spend)}</span>
        <span class="text-[9px] text-slate-400 mt-0.5 block font-medium">${activeCamp.length} campanha(s) ativas</span>
      </div>
      <div>
        <span class="text-[10px] text-slate-400 font-semibold block uppercase tracking-wider">ROAS Consolidado</span>
        <span class="text-lg font-black ${TONE[diagROAS(roas).tone].text} mt-0.5 block tracking-tight">${activeCamp.length ? fmtX(roas) : '—'}</span>
        <span class="text-[9px] ${TONE[diagROAS(roas).tone].text} font-bold mt-0.5 block">${diagROAS(roas).label}</span>
      </div>
    </div>
    ${savedSpend > 0 ? `
    <div class="mt-3 bg-emerald-500/10 border border-emerald-500/20 p-2.5 rounded-2xl text-center flex items-center justify-center gap-2">
      <i class="fa-solid fa-shield-halved text-emerald-400 text-xs"></i>
      <span class="text-[11px] text-emerald-300 font-semibold">Economia Preservada: <strong>${fmtBRL(savedSpend)}</strong> retidos.</span>
    </div>` : ''}`;
}

function renderCampaigns(campaigns) {
  const list = document.getElementById('campaign-list');
  if (!campaigns.length) {
    list.innerHTML = emptyState('Conecte sua conta do Meta Ads para sincronizar suas campanhas.');
    return;
  }
  list.innerHTML = campaigns.map(c => {
    const v = verdict(c);
    const paused = state.paused.includes(c.id);
    const plat = PLATFORM[c.platform];
    const btn = paused
      ? `<button disabled class="w-full mt-3 bg-dark-800/80 border border-white/5 text-emerald-400 font-bold py-3 rounded-2xl text-xs flex items-center justify-center gap-2">
           <i class="fa-solid fa-circle-check"></i> Campanha Pausada (Economia de ${fmtBRL(c.spend)})
         </button>`
      : `<button onclick="askPause('${c.id}')" class="btn-danger w-full mt-3 text-white font-bold py-3 rounded-2xl text-xs">
           Pausar Veiculação e Preservar Verba
         </button>`;
    return `
    <div class="glass-card glass-card-hover rounded-3xl p-4.5 fade-in ${paused ? 'opacity-60' : ''}">
      <div class="flex justify-between items-start gap-2">
        <div class="min-w-0">
          <div class="flex flex-wrap gap-1.5">
            <span class="badge-chip ${TONE[v.tone].text} ${TONE[v.tone].bg} ${TONE[v.tone].border}"><i class="fa-solid ${v.icon}"></i> ${v.label}</span>
          </div>
          <h3 class="font-bold text-xs text-slate-100 mt-2 tracking-tight">${c.name}</h3>
          <p class="text-[10px] text-slate-400 mt-0.5"><i class="${plat.icon} ${plat.color}"></i> ${plat.name} · ${c.objective}</p>
        </div>
        <span class="text-xs font-black ${TONE[diagROAS(c.roas).tone].text} whitespace-nowrap">ROAS ${fmtX(c.roas)}</span>
      </div>
      <div class="grid grid-cols-4 gap-1.5 mt-3 pt-3 border-t border-white/5">
        ${kpi('CTR', fmtPct(c.ctr), diagCTR(c.ctr))}
        ${kpi('CPC', fmtBRL(c.cpc), diagCPC(c.cpc))}
        ${kpi('Freq.', fmtX(c.frequency), diagFreq(c.frequency))}
        ${kpi('Gasto', fmtBRL(c.spend), diagROAS(c.roas))}
      </div>
      ${v.kind === 'fuga' || v.kind === 'fadiga' ? btn : ''}
    </div>`;
  }).join('');
}

function renderFilters(active) {
  document.getElementById('filters').innerHTML = Object.entries(FORMATS).map(([k, label]) =>
    `<button onclick="setFilter('${k}')" class="text-[10px] font-bold px-3.5 py-1.5 rounded-xl whitespace-nowrap border transition
      ${active === k ? 'bg-brand-500 border-indigo-400 text-white shadow-lg shadow-indigo-500/25' : 'bg-dark-900 border-white/5 text-slate-400 hover:text-slate-200'}">${label}</button>`
  ).join('');
}

let currentFilter = 'all';
let creativesCache = [];
function setFilter(f) { currentFilter = f; renderCreatives(); }

function renderCreatives() {
  renderFilters(currentFilter);
  const grid = document.getElementById('creative-grid');
  const items = creativesCache.filter(c => currentFilter === 'all' || c.format === currentFilter);
  if (!creativesCache.length) { grid.innerHTML = emptyState('Nenhum anúncio identificado.', true); return; }
  if (!items.length) { grid.innerHTML = emptyState('Nenhum criativo neste formato.', true); return; }

  const icon = { reel: 'fa-play', carrossel: 'fa-images', imagem: 'fa-image' };
  grid.innerHTML = items.map(c => {
    const fatigue = diagFreq(c.frequency);
    return `
    <div class="glass-card rounded-3xl p-3.5 fade-in flex flex-col justify-between">
      <div>
        <div class="w-full h-24 bg-dark-950 rounded-2xl flex items-center justify-center text-brand-500 text-2xl border border-white/5 mb-2.5 shadow-inner">
          <i class="fa-solid ${icon[c.format]}"></i>
        </div>
        <span class="text-[9px] px-2 py-0.5 rounded-full font-bold border ${FORMAT_STYLE[c.format]}">${FORMATS[c.format]}</span>
        <h4 class="font-bold text-xs text-slate-100 mt-2 leading-snug tracking-tight">${c.title}</h4>
      </div>
      <div class="mt-3 pt-2.5 border-t border-white/5 text-[10px] space-y-1.5">
        <div class="flex justify-between"><span class="text-slate-400">CTR</span><strong class="${TONE[diagCTR(c.ctr).tone].text}">${fmtPct(c.ctr)}</strong></div>
        <div class="flex justify-between"><span class="text-slate-400">ROAS</span><strong class="${TONE[diagROAS(c.roas).tone].text}">${fmtX(c.roas)}</strong></div>
        <div class="flex justify-between"><span class="text-slate-400">Frequência</span><strong class="${TONE[fatigue.tone].text}">${fmtX(c.frequency)}</strong></div>
        <div class="mt-2"><span class="badge-chip ${TONE[fatigue.tone].text} ${TONE[fatigue.tone].bg} ${TONE[fatigue.tone].border} w-full justify-center">${fatigue.label}</span></div>
      </div>
    </div>`;
  }).join('');
}

function renderCriteria() {
  const rows = [
    { t: 'CTR (Click-Through Rate)', tag: 'Atratividade', d: 'Mede o interesse inicial do público pelo criativo.', g: '> 2,0%: anúncio de alto engajamento.', b: '< 1,0%: baixa atração, recomenda-se novo criativo.' },
    { t: 'CPC (Custo por Clique)', tag: 'Eficiência', d: 'Valor médio cobrado pelo tráfego gerado.', g: '< R$ 1,00: tráfego altamente rentável.', b: '> R$ 2,50: custo elevado para a conversão.' },
    { t: 'Frequência de Exposição', tag: 'Saturação', d: 'Média de visualizações por usuário único.', g: '1,0x – 2,5x: alcance saudável.', b: '> 3,5x: fadiga de público, recomenda-se renovar a lista.' },
    { t: 'ROAS (Retorno sobre Anúncio)', tag: 'Rentabilidade', d: 'Múltiplo de faturamento retornado sobre o investimento.', g: '≥ 3,0x: excelente taxa de retorno.', b: '< 1,0x: operação em prejuízo técnico.' }
  ];
  document.getElementById('criteria').innerHTML = rows.map(r => `
    <div class="glass-card rounded-3xl p-4.5">
      <div class="flex justify-between items-center mb-1.5">
        <strong class="text-indigo-300 text-xs font-bold">${r.t}</strong>
        <span class="text-[9px] font-bold bg-indigo-500/10 text-indigo-400 px-2.5 py-0.5 rounded-full border border-indigo-500/20">${r.tag}</span>
      </div>
      <p class="text-slate-300 text-[11px] leading-relaxed mt-1">
        ${r.d}
        <br><span class="text-emerald-400 font-semibold block mt-1.5">• ${r.g}</span>
        <span class="text-rose-400 font-semibold block mt-0.5">• ${r.b}</span>
      </p>
    </div>`).join('');
}

function renderPlatforms() {
  document.getElementById('platform-cards').innerHTML = Object.entries(PLATFORM).map(([key, p]) => {
    const on = state.connected[key];
    const action = on
      ? `<div class="p-3 bg-emerald-500/10 border border-emerald-500/20 rounded-2xl text-emerald-300 text-xs font-semibold flex items-center justify-between">
           <span><i class="fa-solid fa-circle-check mr-2"></i> Conexão Ativa</span>
           <button onclick="disconnect('${key}')" class="text-[10px] text-slate-400 hover:text-rose-400 underline transition">Desconectar</button>
         </div>`
      : `<button onclick="connect('${key}')" class="btn-primary w-full text-white font-bold py-3.5 rounded-2xl text-xs flex items-center justify-center gap-2">
           <i class="${p.icon} text-base"></i> ${p.btn}
         </button>`;
    return `
    <div class="glass-card rounded-3xl p-4.5">
      <div class="flex items-center gap-3.5 mb-4">
        <div class="w-11 h-11 rounded-2xl bg-dark-950 border border-white/5 flex items-center justify-center ${p.color} text-xl shadow-inner"><i class="${p.icon}"></i></div>
        <div>
          <h3 class="font-bold text-sm text-white">${p.name}</h3>
          <p class="text-[10px] text-slate-400 mt-0.5">${on ? 'Sincronizado via Graph API' : 'Pendente de autenticação'}</p>
        </div>
      </div>
      ${action}
    </div>`;
  }).join('');
}

function emptyState(text, compact) {
  return `<div class="glass-card rounded-3xl border-dashed ${compact ? 'p-4 col-span-2' : 'p-6'} text-center text-xs text-slate-400">
    <i class="fa-solid fa-plug-circle-exclamation text-indigo-400 text-2xl mb-2 block"></i>${text}
  </div>`;
}

function renderHeaderStatus() {
  const badge = document.getElementById('account-status-badge');
  const names = Object.keys(state.connected).filter(k => state.connected[k]).map(k => PLATFORM[k].name);
  badge.innerHTML = names.length
    ? `<span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span> ${names.join(' + ')} Ativo`
    : `<span class="w-2 h-2 rounded-full bg-slate-600"></span> Nenhuma conta vinculada`;
}

async function refresh() {
  const [campaigns, creatives] = await Promise.all([API.fetchCampaigns(), API.fetchCreatives()]);
  creativesCache = creatives;
  renderHeaderStatus();
  renderRaioX(campaigns);
  renderCampaigns(campaigns);
  renderCreatives();
  renderCriteria();
  renderPlatforms();
  document.getElementById('last-sync').textContent = 'Sincronizado às ' + new Date().toLocaleTimeString('pt-BR', { hour: '2-digit', minute: '2-digit' });
}

let pendingPause = null;
function askPause(id) {
  const c = MOCK.campaigns.find(x => x.id === id);
  pendingPause = id;
  document.getElementById('modal-title').textContent = 'Confirmar Interrupção de Veiculação';
  document.getElementById('modal-text').textContent =
    `A campanha "${c.name}" apresenta consumo de ${fmtBRL(c.spend)} com retorno abaixo do parâmetro (ROAS ${fmtX(c.roas)}). Ao interromper, o consumo de verba é pausado imediatamente.`;
  document.getElementById('modal-confirm').onclick = confirmPause;
  document.getElementById('modal').classList.remove('hidden');
}
function closeModal() { document.getElementById('modal').classList.add('hidden'); pendingPause = null; }

async function confirmPause() {
  const id = pendingPause;
  closeModal();
  if (!id) return;
  await API.pauseCampaign(id);
  if (!state.paused.includes(id)) state.paused.push(id);
  saveState();
  toast('Campanha pausada no Meta Ads!');
  refresh();
}

function connect(platform) {
  if (platform === 'meta') {
    if (typeof FB === 'undefined') {
      toast('Carregando SDK da Meta...');
      return;
    }
    FB.login(function(response) {
      if (response.authResponse) {
        state.connected.meta = true;
        state.metaToken = response.authResponse.accessToken;
        saveState();
        toast('Meta Ads vinculado com sucesso!');
        refresh();
      } else {
        toast('Autenticação cancelada.');
      }
    }, {scope: 'ads_read,ads_management'});
  }
}

function disconnect(platform) {
  state.connected[platform] = false;
  if (platform === 'meta') state.metaToken = null;
  saveState();
  toast(PLATFORM[platform].name + ' desconectado');
  refresh();
}

async function shareReport() {
  const campaigns = await API.fetchCampaigns();
  const activeCamp = campaigns.filter(c => !state.paused.includes(c.id));
  const spend = activeCamp.reduce((s, c) => s + c.spend, 0);
  const roas = activeCamp.length ? activeCamp.reduce((s, c) => s + c.roas * c.spend, 0) / (spend || 1) : 0;
  
  const text = `📈 *Relatório Executivo AdPulse* (${currentTF === '7d' ? '7 dias' : '30 dias'}):
• *Investimento Ativo:* ${fmtBRL(spend)}
• *ROAS Médio:* ${fmtX(roas)}
• *Campanhas Ativas:* ${activeCamp.length}
• *Economia Preservada:* ${state.paused.length} campanha(s) pausada(s)

_Gerado via AdPulse Performance Intelligence_`;

  if (navigator.share) {
    navigator.share({ title: 'Relatório AdPulse', text: text });
  } else {
    navigator.clipboard.writeText(text);
    toast('Relatório copiado para a área de transferência!');
  }
}

function switchTab(tab) {
  ['home','creatives','recommendations','connect','plan'].forEach(t => {
    document.getElementById('sec-' + t).classList.toggle('hidden', t !== tab);
  });
  document.querySelectorAll('.nav-btn').forEach(b => {
    const active = b.dataset.tab === tab;
    b.className = `nav-btn flex flex-col items-center gap-1 w-full py-1 transition ${active ? 'text-brand-500' : 'text-slate-500'}`;
  });
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

function installPWA() {
  toast("Toque em 'Compartilhar' e selecione 'Adicionar à Tela Inicial'");
}

document.getElementById('modal').addEventListener('click', e => { if (e.target.id === 'modal') closeModal(); });

switchTab('home');
refresh();
</script>
</body>
</html>
