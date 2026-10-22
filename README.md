<!DOCTYPE html>
<html lang="pt-BR" class="dark">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>AdPulse | Intelligence & Performance Dashboard</title>

<!-- PWA Meta Tags -->
<meta name="theme-color" content="#05070a">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="AdPulse">
<link rel="apple-touch-icon" href="https://cdn-icons-png.flaticon.com/512/1055/1055687.png">
<link rel="manifest" href='data:application/manifest+json,{"name":"AdPulse Performance","short_name":"AdPulse","start_url":".","display":"standalone","background_color":"#05070a","theme_color":"#05070a","icons":[{"src":"https://cdn-icons-png.flaticon.com/512/1055/1055687.png","sizes":"512x512","type":"image/png"}]}'>

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
          brand: { 500: '#06b6d4', 600: '#0891b2', 700: '#0e7490' },
          violet: { 500: '#6366f1', 600: '#4f46e5' },
          dark: { 950: '#05070a', 900: '#0a0e17', 800: '#121824', 700: '#1e293b' }
        }
      }
    }
  }
</script>

<style>
  body {
    -webkit-tap-highlight-color: transparent;
    user-select: none;
    background-color: #05070a;
    background-image: 
      radial-gradient(circle at 0% 0%, rgba(6, 182, 212, 0.15) 0%, transparent 40%),
      radial-gradient(circle at 100% 100%, rgba(99, 102, 241, 0.12) 0%, transparent 45%);
    background-attachment: fixed;
  }
  
  .hide-scrollbar::-webkit-scrollbar { display: none; }
  .hide-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
  
  .glass-card {
    background: rgba(10, 14, 23, 0.7);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    border: 1px solid rgba(255, 255, 255, 0.08);
    box-shadow: 0 10px 30px -5px rgba(0, 0, 0, 0.5);
  }

  .glass-card-hover {
    transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);
  }
  .glass-card-hover:hover {
    border-color: rgba(6, 182, 212, 0.4);
    transform: translateY(-2px);
    box-shadow: 0 15px 35px -5px rgba(6, 182, 212, 0.15);
  }

  .btn-primary {
    background: linear-gradient(135deg, #06b6d4 0%, #3b82f6 50%, #6366f1 100%);
    box-shadow: 0 4px 25px -2px rgba(6, 182, 212, 0.4);
    transition: all 0.2s cubic-bezier(0.16, 1, 0.3, 1);
  }
  .btn-primary:active { transform: scale(0.97); }

  .btn-danger {
    background: linear-gradient(135deg, #f43f5e 0%, #e11d48 100%);
    box-shadow: 0 4px 20px -2px rgba(244, 63, 94, 0.4);
    transition: all 0.2s cubic-bezier(0.16, 1, 0.3, 1);
  }
  .btn-danger:active { transform: scale(0.97); }

  .fade-in { animation: fadeIn 0.35s cubic-bezier(0.16, 1, 0.3, 1) both; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: none; } }

  .badge-chip {
    font-size: 10px;
    font-weight: 700;
    padding: 3px 10px;
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
<header class="bg-dark-900/80 backdrop-blur-xl border-b border-white/10 sticky top-0 z-40 px-4 py-3.5 flex items-center justify-between">
  <div class="flex items-center gap-3">
    <div class="w-10 h-10 rounded-2xl bg-gradient-to-tr from-cyan-500 via-blue-600 to-indigo-600 flex items-center justify-center text-white shadow-lg shadow-cyan-500/25 border border-white/20">
      <i class="fa-solid fa-chart-line text-base"></i>
    </div>
    <div>
      <h1 class="font-extrabold text-sm tracking-tight text-white flex items-center gap-2">
        AdPulse <span class="text-[9px] font-black uppercase px-2 py-0.5 rounded-full bg-cyan-500/10 text-cyan-400 border border-cyan-500/30">PRO</span>
      </h1>
      <p id="account-status-badge" class="text-[10px] text-slate-400 font-medium flex items-center gap-1.5 mt-0.5">
        <span class="w-2 h-2 rounded-full bg-slate-600"></span> Nenhuma conta vinculada
      </p>
    </div>
  </div>
  <div class="flex items-center gap-2">
    <button onclick="shareReport()" class="p-2.5 rounded-2xl bg-dark-800 border border-white/10 text-emerald-400 hover:bg-dark-700 transition active:scale-95 shadow-md" title="Exportar Relatório via WhatsApp">
      <i class="fa-brands fa-whatsapp text-base"></i>
    </button>
    <button onclick="installPWA()" class="px-3.5 py-2 rounded-2xl bg-cyan-500/10 border border-cyan-500/30 text-cyan-300 text-xs font-bold flex items-center gap-1.5 hover:bg-cyan-500/20 transition active:scale-95 shadow-md">
      <i class="fa-solid fa-download text-[11px]"></i> App
    </button>
  </div>
</header>

<main class="max-w-md mx-auto w-full px-4 pt-4 flex-1">

  <!-- FILTRO DE JANELA TEMPORAL -->
  <div class="flex justify-between items-center mb-4 bg-dark-900/80 backdrop-blur-md p-1.5 rounded-2xl border border-white/10 shadow-inner">
    <span class="text-[10px] font-extrabold text-slate-400 pl-3 uppercase tracking-wider">Janela de Análise</span>
    <div class="flex gap-1">
      <button onclick="setTimeframe('7d')" id="tf-7d" class="text-[11px] font-bold px-3.5 py-1.5 rounded-xl transition bg-brand-500 text-slate-950 shadow-lg shadow-cyan-500/25">Últimos 7d</button>
      <button onclick="setTimeframe('30d')" id="tf-30d" class="text-[11px] font-bold px-3.5 py-1.5 rounded-xl transition text-slate-400 hover:text-slate-200">Últimos 30d</button>
    </div>
  </div>

  <!-- RAIO-X DE PERFORMANCE -->
  <div id="raiox" class="glass-card rounded-3xl p-4 mb-5 fade-in relative overflow-hidden"></div>

  <!-- ABA: CAMPANHAS (INÍCIO) -->
  <section id="sec-home" class="space-y-3.5">
    <div class="flex items-center justify-between px-0.5">
      <h2 class="text-xs font-bold text-slate-300 uppercase tracking-wider flex items-center gap-2">
        <i class="fa-solid fa-layer-group text-cyan-400"></i> Campanhas em Monitoramento
      </h2>
      <span class="text-[10px] text-slate-500 font-medium" id="last-sync">—</span>
    </div>
    <div id="campaign-list" class="space-y-3.5"></div>
  </section>

  <!-- ABA: CRIATIVOS -->
  <section id="sec-creatives" class="hidden space-y-3.5">
    <div class="flex items-center justify-between px-0.5">
      <h2 class="text-xs font-bold text-slate-300 uppercase tracking-wider flex items-center gap-2">
        <i class="fa-solid fa-photo-film text-cyan-400"></i> Performance por Criativo
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
      <div class="w-14 h-14 rounded-2xl bg-cyan-500/10 border border-cyan-500/30 flex items-center justify-center text-cyan-400 mx-auto mb-3 text-2xl shadow-lg shadow-cyan-500/10">
        <i class="fa-solid fa-link"></i>
      </div>
      <h3 class="text-base font-bold text-slate-100">Integração de Contas de Anúncios</h3>
      <p class="text-xs text-slate-400 mt-1.5 leading-relaxed">Conecte sua conta do Meta Ads para sincronização automática de métricas via SDK Oficial.</p>
    </div>
    <div id="platform-cards" class="space-y-3.5"></div>
  </section>

  <!-- ABA: ASSINATURA -->
  <section id="sec-plan" class="hidden space-y-3.5">
    <div class="glass-card rounded-3xl p-6 text-center border-cyan-500/30 relative overflow-hidden">
      <div class="absolute -right-12 -top-12 w-36 h-36 bg-cyan-500/20 rounded-full blur-3xl"></div>
      <span class="bg-gradient-to-r from-cyan-400 via-blue-500 to-indigo-500 text-slate-950 text-[10px] font-black px-3.5 py-1 rounded-full uppercase tracking-wider shadow-lg shadow-cyan-500/20">
        Plano Gestor Profissional
      </span>
      <h3 class="text-3xl font-black text-white mt-5">R$ 180 <span class="text-xs font-semibold text-slate-400">/ trimestre</span></h3>
      <p class="text-xs text-slate-400 mt-1">Diagnóstico inteligente contínuo para evitar desperdício de verba.</p>
      <ul class="text-left text-xs text-slate-300 mt-6 space-y-3 border-t border-white/10 pt-5">
        <li class="flex items-center gap-3"><i class="fa-solid fa-circle-check text-emerald-400 text-sm"></i> Monitoramento unificado Meta Ads</li>
        <li class="flex items-center gap-3"><i class="fa-solid fa-circle-check text-emerald-400 text-sm"></i> Detecção automática de Fadiga de Anúncio</li>
        <li class="flex items-center gap-3"><i class="fa-solid fa-circle-check text-emerald-400 text-sm"></i> Interrupção de campanhas em 1 clique</li>
      </ul>
      <button onclick="toast('Redirecionando para o checkout seguro...')" class="btn-primary w-full text-slate-950 font-black py-3.5 rounded-2xl text-xs mt-6">
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
<div id="toast" class="hidden fixed top-5 left-1/2 -translate-x-1/2 z-[70] bg-dark-900 border border-cyan-500/40 text-xs text-slate-100 px-4 py-3 rounded-2xl shadow-2xl fade-in flex items-center gap-2.5 backdrop-blur-xl">
  <i class="fa-solid fa-circle-info text-cyan-400 text-sm"></i> <span id="toast-msg"></span>
</div>

<!-- NAVEGAÇÃO INFERIOR PWA -->
<nav class="fixed bottom-0 left-0 right-0 bg-dark-900/90 backdrop-blur-xl border-t border-white/10 z-50">
  <div class="max-w-md mx-auto flex justify-around items-center h-16 px-2">
    <button onclick="switchTab('home')" data-tab="home" class="nav-btn flex flex-col items-center gap-1 w-full py-1 text-cyan-400 transition"><i class="fa-solid fa-chart-simple text-sm"></i><span class="text-[9px] font-bold">Painel</span></button>
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
      frequency: parseFloat(frequency
