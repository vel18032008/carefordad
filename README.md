<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CareForDad — A Smart Health Companion</title>
<meta name="description" content="CareForDad: medicine reminders, escalation alerts, family dashboard, and wearable health monitoring for parents with diabetes, BP, and chronic conditions.">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@3.31.0/dist/tabler-icons.min.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<style>
:root{
  --bg-tertiary:#f4f3f1; --bg-primary:#ffffff; --bg-secondary:#f7f6f4;
  --text-primary:#1a1a1a; --text-secondary:#6b6b6b;
  --border-tertiary:#ececec; --border-secondary:#dcdcdc;
  --radius-lg:14px; --radius-md:10px;
  --pink:#e74c7c; --pink-dark:#c0395e;
}
@media (prefers-color-scheme: dark){
  :root{
    --bg-tertiary:#0f0f10; --bg-primary:#1a1a1b; --bg-secondary:#222224;
    --text-primary:#f0f0f0; --text-secondary:#9a9a9a;
    --border-tertiary:#2c2c2e; --border-secondary:#3a3a3c;
  }
}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,sans-serif;background:var(--bg-tertiary);color:var(--text-primary);min-height:100vh}
.app{max-width:780px;margin:0 auto;padding:0 0 80px}
.topbar{background:var(--bg-primary);border-bottom:0.5px solid var(--border-tertiary);padding:12px 20px;display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:10}
.logo{display:flex;align-items:center;gap:8px;font-size:16px;font-weight:600}
.logo-icon{width:32px;height:32px;background:var(--pink);border-radius:8px;display:flex;align-items:center;justify-content:center;color:#fff;font-size:16px}
.role-pill{display:flex;gap:4px;background:var(--bg-secondary);border-radius:20px;padding:3px}
.role-btn{padding:4px 14px;border-radius:16px;border:none;background:transparent;font-size:12px;cursor:pointer;color:var(--text-secondary);font-family:inherit}
.role-btn.active{background:var(--bg-primary);color:var(--text-primary);font-weight:600;border:0.5px solid var(--border-secondary)}
.nav{display:flex;background:var(--bg-primary);border-top:0.5px solid var(--border-tertiary);position:fixed;bottom:0;left:0;right:0;z-index:10}
.nav-item{flex:1;display:flex;flex-direction:column;align-items:center;gap:3px;padding:10px 4px 8px;border:none;background:transparent;cursor:pointer;font-family:inherit;font-size:10px;color:var(--text-secondary)}
.nav-item.active{color:var(--pink)}
.nav-item i{font-size:22px}
.screen{padding:16px}
.section-label{font-size:11px;font-weight:600;color:var(--text-secondary);letter-spacing:0.06em;text-transform:uppercase;margin:20px 0 8px}
.card{background:var(--bg-primary);border:0.5px solid var(--border-tertiary);border-radius:var(--radius-lg);padding:16px 18px;margin-bottom:10px}
.metric-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:10px}
.metric{background:var(--bg-secondary);border-radius:var(--radius-md);padding:12px 14px}
.metric-label{font-size:11px;color:var(--text-secondary);margin-bottom:4px}
.metric-val{font-size:22px;font-weight:600}
.metric-val.good{color:#27ae60}
.metric-val.warn{color:#e67e22}
.metric-val.bad{color:#e74c7c}
.badge{display:inline-flex;align-items:center;gap:4px;font-size:11px;padding:3px 9px;border-radius:20px;font-weight:600}
.badge.taken{background:#e8f8ee;color:#1a7a3c}
.badge.missed{background:#fde8ee;color:#b0193c}
.badge.pending{background:#fff8e1;color:#8a6100}
.btn{display:inline-flex;align-items:center;gap:6px;padding:8px 16px;border-radius:var(--radius-md);border:0.5px solid var(--border-secondary);background:transparent;cursor:pointer;font-family:inherit;font-size:13px;color:var(--text-primary);font-weight:600}
.btn:hover{background:var(--bg-secondary)}
.btn-primary{background:var(--pink);border-color:var(--pink);color:#fff}
.btn-primary:hover{background:var(--pink-dark);border-color:var(--pink-dark)}
.btn-success{background:#27ae60;border-color:#27ae60;color:#fff}
.btn-success:hover{background:#1e8449}
.btn-sm{padding:5px 12px;font-size:12px}
.inp{width:100%;padding:9px 12px;border:0.5px solid var(--border-secondary);border-radius:var(--radius-md);background:var(--bg-primary);color:var(--text-primary);font-family:inherit;font-size:14px;margin-bottom:10px}
.inp:focus{outline:none;border-color:var(--pink)}
select.inp{cursor:pointer}
.med-row{display:flex;align-items:center;gap:10px;padding:12px 0;border-bottom:0.5px solid var(--border-tertiary)}
.med-row:last-child{border-bottom:none}
.med-icon{width:36px;height:36px;border-radius:8px;background:#fde8ee;display:flex;align-items:center;justify-content:center;color:var(--pink);font-size:18px;flex-shrink:0}
.med-info{flex:1}
.med-name{font-size:14px;font-weight:600}
.med-time{font-size:12px;color:var(--text-secondary);margin-top:2px}
.stock-bar{height:4px;border-radius:2px;background:var(--bg-secondary);margin-top:6px;overflow:hidden}
.stock-fill{height:100%;border-radius:2px;background:#27ae60;transition:width .3s}
.stock-fill.low{background:var(--pink)}
.alert-item{display:flex;align-items:flex-start;gap:10px;padding:10px 0;border-bottom:0.5px solid var(--border-tertiary)}
.alert-item:last-child{border-bottom:none}
.alert-dot{width:8px;height:8px;border-radius:50%;flex-shrink:0;margin-top:5px}
.alert-dot.warn{background:#e67e22}
.alert-dot.bad{background:var(--pink)}
.alert-dot.good{background:#27ae60}
.chart-wrap{position:relative;width:100%;height:160px;margin-top:12px}
.bp-entry{display:flex;gap:8px;align-items:center;margin-bottom:8px}
.tag{display:inline-flex;align-items:center;gap:3px;font-size:11px;padding:2px 8px;border-radius:12px;background:var(--bg-secondary);color:var(--text-secondary)}
.ai-box{background:linear-gradient(135deg,#fde8f3,#fae8f8);border:0.5px solid #e8b4d0;border-radius:var(--radius-lg);padding:16px 18px;margin-top:6px}
.ai-box p{font-size:14px;line-height:1.6;color:#6b2040}
.spinner{display:inline-block;width:16px;height:16px;border:2px solid #e8b4d0;border-top-color:var(--pink);border-radius:50%;animation:spin .8s linear infinite}
@keyframes spin{to{transform:rotate(360deg)}}
.wearable-card{text-align:center;padding:20px}
.hr-display{font-size:52px;font-weight:600;color:var(--pink);margin:10px 0}
.hr-pulse{display:inline-block;animation:pulse 1.2s ease-in-out infinite}
@keyframes pulse{0%,100%{transform:scale(1)}50%{transform:scale(1.08)}}
.timeline{position:relative;padding-left:20px}
.tl-item{position:relative;padding:8px 0 8px 16px;border-left:1.5px solid var(--border-tertiary)}
.tl-dot{position:absolute;left:-5px;top:12px;width:8px;height:8px;border-radius:50%;background:var(--border-secondary)}
.tl-dot.done{background:#27ae60}
.tl-dot.alert{background:var(--pink)}
.tl-time{font-size:11px;color:var(--text-secondary)}
.tl-text{font-size:13px;margin-top:2px}
.footer-note{text-align:center;font-size:11px;color:var(--text-secondary);padding:18px 16px 0}
input[type=number]{-moz-appearance:textfield}
input[type=number]::-webkit-outer-spin-button,input[type=number]::-webkit-inner-spin-button{-webkit-appearance:none}
</style>
</head>
<body>
<div class="app">
<div class="topbar">
  <div class="logo">
    <div class="logo-icon"><i class="ti ti-heart" aria-hidden="true"></i></div>
    CareForDad
  </div>
  <div class="role-pill">
    <button class="role-btn active" id="rb-parent" onclick="setRole('parent')">Parent</button>
    <button class="role-btn" id="rb-guardian" onclick="setRole('guardian')">Guardian</button>
  </div>
</div>

<div class="screen" id="screen-home">
<div class="section-label">Today's overview</div>
<div class="metric-grid">
  <div class="metric"><div class="metric-label">Medicines</div><div class="metric-val good" id="met-med">2/3</div></div>
  <div class="metric"><div class="metric-label">Adherence</div><div class="metric-val good" id="met-adh">94%</div></div>
  <div class="metric"><div class="metric-label">BP today</div><div class="metric-val warn" id="met-bp">138/88</div></div>
</div>

<div class="section-label">Medicines due today</div>
<div class="card" id="med-list-home"></div>

<div class="section-label">Smart escalation</div>
<div class="card">
  <div class="timeline" id="timeline"></div>
</div>

<div class="section-label">Quick log health</div>
<div class="card">
  <div style="display:flex;gap:8px;flex-wrap:wrap">
    <button class="btn btn-sm" onclick="showTab('health')"><i class="ti ti-activity" aria-hidden="true"></i> Log BP</button>
    <button class="btn btn-sm" onclick="showTab('health')"><i class="ti ti-droplet" aria-hidden="true"></i> Log Sugar</button>
    <button class="btn btn-sm btn-primary" onclick="showTab('wearable')"><i class="ti ti-watch" aria-hidden="true"></i> CareBand</button>
  </div>
</div>
</div>

<div class="screen" id="screen-meds" style="display:none">
<div class="section-label">My medicines</div>
<div class="card" id="med-full-list"></div>
<button class="btn btn-primary" style="width:100%;justify-content:center;margin-top:8px" onclick="toggleAddMed()"><i class="ti ti-plus" aria-hidden="true"></i> Add medicine</button>
<div id="add-med-form" style="display:none;margin-top:10px">
  <div class="card">
    <div class="section-label" style="margin-top:0">New medicine</div>
    <input class="inp" id="new-med-name" placeholder="Medicine name (e.g. Metformin)">
    <div style="display:flex;gap:8px">
      <input class="inp" type="time" id="new-med-time" value="08:00" style="flex:1">
      <input class="inp" type="number" id="new-med-stock" placeholder="Qty (tablets)" style="flex:1">
    </div>
    <select class="inp" id="new-med-freq">
      <option>Daily</option><option>Twice daily</option><option>Weekly</option>
    </select>
    <button class="btn btn-primary" style="width:100%;justify-content:center" onclick="addMedicine()"><i class="ti ti-check" aria-hidden="true"></i> Save medicine</button>
  </div>
</div>
</div>

<div class="screen" id="screen-health" style="display:none">
<div class="section-label">Log blood pressure</div>
<div class="card">
  <div class="bp-entry">
    <input class="inp" type="number" id="bp-sys" placeholder="Systolic" style="flex:1;margin:0">
    <span style="color:var(--text-secondary);font-size:18px">/</span>
    <input class="inp" type="number" id="bp-dia" placeholder="Diastolic" style="flex:1;margin:0">
    <span style="font-size:12px;color:var(--text-secondary)">mmHg</span>
  </div>
  <button class="btn btn-primary" style="width:100%;justify-content:center;margin-top:8px" onclick="logBP()"><i class="ti ti-plus" aria-hidden="true"></i> Log BP</button>
</div>

<div class="section-label">Log blood sugar</div>
<div class="card">
  <div class="bp-entry">
    <input class="inp" type="number" id="sugar-val" placeholder="Sugar level" style="flex:1;margin:0">
    <span style="font-size:12px;color:var(--text-secondary)">mg/dL</span>
  </div>
  <button class="btn btn-primary" style="width:100%;justify-content:center;margin-top:8px" onclick="logSugar()"><i class="ti ti-plus" aria-hidden="true"></i> Log Sugar</button>
</div>

<div class="section-label">Recent BP readings</div>
<div class="card"><div class="chart-wrap"><canvas id="bp-chart" role="img" aria-label="BP trend line chart showing systolic and diastolic readings over recent days"></canvas></div></div>

<div class="section-label">Recent sugar readings</div>
<div class="card"><div class="chart-wrap"><canvas id="sugar-chart" role="img" aria-label="Blood sugar trend line chart"></canvas></div></div>
</div>

<div class="screen" id="screen-family" style="display:none">
<div id="family-parent-view"></div>
<div id="family-guardian-view"></div>
</div>

<div class="screen" id="screen-wearable" style="display:none">
<div class="section-label">CareBand — live monitor</div>
<div class="card wearable-card">
  <i class="ti ti-watch" aria-hidden="true" style="font-size:36px;color:var(--text-secondary)"></i>
  <p style="font-size:12px;color:var(--text-secondary);margin-top:4px">Heart rate</p>
  <div class="hr-display"><span class="hr-pulse" id="hr-val">72</span> <span style="font-size:20px">bpm</span></div>
  <div id="hr-status" class="badge taken"><i class="ti ti-heart" aria-hidden="true"></i> Normal</div>
  <div style="margin-top:16px;display:flex;gap:8px;justify-content:center;flex-wrap:wrap">
    <button class="btn btn-sm btn-success" onclick="setHR(72,'normal')"><i class="ti ti-heart" aria-hidden="true"></i> Normal (72 bpm)</button>
    <button class="btn btn-sm btn-primary" onclick="setHR(125,'alert')"><i class="ti ti-alert-triangle" aria-hidden="true"></i> Alert (125 bpm)</button>
  </div>
  <p style="font-size:11px;color:var(--text-secondary);margin-top:10px">Simulated CareBand readings — demonstrates wearable integration concept</p>
</div>
<div class="section-label">Recent heart rate</div>
<div class="card"><div class="chart-wrap"><canvas id="hr-chart" role="img" aria-label="Heart rate trend over time"></canvas></div></div>
<div class="section-label">Activity today</div>
<div class="card">
  <div class="metric-grid">
    <div class="metric"><div class="metric-label">Steps</div><div class="metric-val good">4,280</div></div>
    <div class="metric"><div class="metric-label">Sleep</div><div class="metric-val good">7.2h</div></div>
    <div class="metric"><div class="metric-label">Active min</div><div class="metric-val warn">28</div></div>
  </div>
</div>
</div>

<div class="footer-note">CareForDad — built for the NIAT #BuildForDad Father's Day Challenge ❤️</div>
</div>

<nav class="nav">
  <button class="nav-item active" id="nb-home" onclick="showTab('home')"><i class="ti ti-home" aria-hidden="true"></i>Home</button>
  <button class="nav-item" id="nb-meds" onclick="showTab('meds')"><i class="ti ti-pill" aria-hidden="true"></i>Medicines</button>
  <button class="nav-item" id="nb-health" onclick="showTab('health')"><i class="ti ti-activity" aria-hidden="true"></i>Health</button>
  <button class="nav-item" id="nb-family" onclick="showTab('family')"><i class="ti ti-users" aria-hidden="true"></i>Family</button>
  <button class="nav-item" id="nb-wearable" onclick="showTab('wearable')"><i class="ti ti-watch" aria-hidden="true"></i>CareBand</button>
</nav>

<script>
let role = 'parent';
let activeTab = 'home';
let bpChart, sugarChart, hrChart;

const medicines = [
  {name:'Metformin 500mg', time:'08:00', stock:28, total:30, freq:'Daily', status:'taken'},
  {name:'Amlodipine 5mg', time:'13:00', stock:14, total:30, freq:'Daily', status:'pending'},
  {name:'Atorvastatin 10mg', time:'21:00', stock:5, total:30, freq:'Daily', status:'pending'},
];

const bpData = [
  {date:'Jun 12',sys:128,dia:82},{date:'Jun 13',sys:132,dia:85},{date:'Jun 14',sys:138,dia:88},
  {date:'Jun 15',sys:130,dia:84},{date:'Jun 16',sys:126,dia:80},{date:'Jun 17',sys:134,dia:86},{date:'Jun 18',sys:138,dia:88}
];
const sugarData = [
  {date:'Jun 12',val:118},{date:'Jun 13',val:142},{date:'Jun 14',val:156},{date:'Jun 15',val:135},
  {date:'Jun 16',val:128},{date:'Jun 17',val:145},{date:'Jun 18',val:138}
];
const hrData = [72,68,75,70,72,80,72];
const hrLabels = ['8am','10am','12pm','2pm','4pm','6pm','Now'];
const alerts = [
  {type:'warn',text:'Amlodipine not yet confirmed today (1:00 PM reminder sent)'},
  {type:'bad',text:'Sugar reading high on Jun 14 — 156 mg/dL'},
  {type:'warn',text:'Atorvastatin stock low — only 5 tablets remaining'},
  {type:'good',text:'Metformin taken on time this week — 7/7 days'},
];

// Pre-written AI-style summaries used for the live demo (no external API key required).
// In production this is generated dynamically via the Anthropic/OpenAI API, as described
// in the project design doc, using the patient's live adherence, BP, and sugar data.
const aiSummaries = [
  "Appa, this week your medicine adherence was strong at 94% — almost every dose taken on time. Your blood pressure has stayed fairly steady around 132/85, though it crept up slightly on Jun 14 and Jun 18. Your average sugar reading was 137 mg/dL, with one higher reading on Jun 14. One gentle suggestion: try to log Amlodipine right after lunch so it's never missed.",
];

function setRole(r){
  role=r;
  document.getElementById('rb-parent').classList.toggle('active',r==='parent');
  document.getElementById('rb-guardian').classList.toggle('active',r==='guardian');
  renderFamily();
}

function showTab(t){
  activeTab=t;
  ['home','meds','health','family','wearable'].forEach(s=>{
    document.getElementById('screen-'+s).style.display=s===t?'block':'none';
    document.getElementById('nb-'+s).classList.toggle('active',s===t);
  });
  if(t==='health') setTimeout(initCharts,50);
  if(t==='wearable') setTimeout(initHRChart,50);
  if(t==='family') renderFamily();
}

function renderHome(){
  const el = document.getElementById('med-list-home');
  el.innerHTML = medicines.map((m,i)=>`
    <div class="med-row">
      <div class="med-icon"><i class="ti ti-pill" aria-hidden="true"></i></div>
      <div class="med-info">
        <div class="med-name">${m.name}</div>
        <div class="med-time"><i class="ti ti-clock" style="font-size:12px;vertical-align:-1px"></i> ${m.time} &nbsp; <span class="tag">${m.freq}</span></div>
        <div class="stock-bar"><div class="stock-fill ${m.stock/m.total<0.2?'low':''}" style="width:${Math.round(m.stock/m.total*100)}%"></div></div>
      </div>
      <div>${m.status==='taken'?`<span class="badge taken"><i class="ti ti-check"></i> Taken</span>`:
        m.status==='pending'?`<button class="btn btn-sm btn-success" onclick="confirmMed(${i})"><i class="ti ti-check" aria-hidden="true"></i> Confirm</button>`:`<span class="badge missed">Missed</span>`}</div>
    </div>`).join('');

  const tl = document.getElementById('timeline');
  const med = medicines.find(m=>m.status==='pending');
  const mname = med?med.name:'Amlodipine';
  tl.innerHTML = `
    <div class="tl-item"><span class="tl-dot done"></span><div class="tl-time">1:00 PM</div><div class="tl-text">Reminder sent: <strong>${mname}</strong></div></div>
    <div class="tl-item"><span class="tl-dot done"></span><div class="tl-time">1:15 PM</div><div class="tl-text">Reminder again — no response</div></div>
    <div class="tl-item"><span class="tl-dot alert"></span><div class="tl-time">1:30 PM</div><div class="tl-text" style="color:var(--pink)">Guardian notified — has not confirmed medicine</div></div>
    <div class="tl-item"><span class="tl-dot" style="background:#ccc"></span><div class="tl-time">2:00 PM</div><div class="tl-text" style="color:var(--text-secondary)">Final reminder pending</div></div>`;
}

function renderMeds(){
  const el = document.getElementById('med-full-list');
  el.innerHTML = medicines.map((m,i)=>`
    <div class="med-row">
      <div class="med-icon"><i class="ti ti-pill" aria-hidden="true"></i></div>
      <div class="med-info">
        <div class="med-name">${m.name}</div>
        <div class="med-time">${m.time} · ${m.freq}</div>
        <div style="font-size:11px;color:${m.stock<=5?'var(--pink)':'var(--text-secondary)'};margin-top:3px">
          <i class="ti ti-package" style="font-size:11px;vertical-align:-1px"></i> ${m.stock} tablets left ${m.stock<=5?'⚠️ Running low':''}
        </div>
      </div>
      <div style="display:flex;gap:6px">
        <button class="btn btn-sm" onclick="deleteMed(${i})" style="color:var(--text-secondary)"><i class="ti ti-trash" aria-hidden="true"></i></button>
      </div>
    </div>`).join('');
}

function toggleAddMed(){
  const f=document.getElementById('add-med-form');
  f.style.display=f.style.display==='none'?'block':'none';
}

function addMedicine(){
  const name=document.getElementById('new-med-name').value.trim();
  const time=document.getElementById('new-med-time').value;
  const stock=parseInt(document.getElementById('new-med-stock').value)||30;
  const freq=document.getElementById('new-med-freq').value;
  if(!name){alert('Please enter a medicine name');return;}
  medicines.push({name,time,stock,total:stock,freq,status:'pending'});
  document.getElementById('add-med-form').style.display='none';
  document.getElementById('new-med-name').value='';
  renderMeds();
}

function deleteMed(i){medicines.splice(i,1);renderMeds();}

function confirmMed(i){
  medicines[i].status='taken';
  medicines[i].stock=Math.max(0,medicines[i].stock-1);
  renderHome();
  renderMeds();
  const taken=medicines.filter(m=>m.status==='taken').length;
  document.getElementById('met-med').textContent=taken+'/'+medicines.length;
}

function logBP(){
  const sys=parseInt(document.getElementById('bp-sys').value);
  const dia=parseInt(document.getElementById('bp-dia').value);
  if(!sys||!dia){alert('Enter both systolic and diastolic values');return;}
  bpData.push({date:'Now',sys,dia});
  if(bpData.length>10)bpData.shift();
  document.getElementById('bp-sys').value='';
  document.getElementById('bp-dia').value='';
  document.getElementById('met-bp').textContent=sys+'/'+dia;
  document.getElementById('met-bp').className='metric-val '+(sys>140||dia>90?'bad':sys>130||dia>85?'warn':'good');
  if(bpChart){
    bpChart.data.labels=bpData.map(d=>d.date);
    bpChart.data.datasets[0].data=bpData.map(d=>d.sys);
    bpChart.data.datasets[1].data=bpData.map(d=>d.dia);
    bpChart.update();
  }
}

function logSugar(){
  const val=parseInt(document.getElementById('sugar-val').value);
  if(!val){alert('Enter blood sugar value');return;}
  sugarData.push({date:'Now',val});
  if(sugarData.length>10)sugarData.shift();
  document.getElementById('sugar-val').value='';
  if(sugarChart){
    sugarChart.data.labels=sugarData.map(d=>d.date);
    sugarChart.data.datasets[0].data=sugarData.map(d=>d.val);
    sugarChart.update();
  }
}

let chartsInited=false;
function initCharts(){
  if(chartsInited)return;chartsInited=true;
  const isDark=matchMedia('(prefers-color-scheme:dark)').matches;
  const gridColor=isDark?'rgba(255,255,255,0.08)':'rgba(0,0,0,0.06)';
  const textColor=isDark?'#aaa':'#888';
  const opts={responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:gridColor},ticks:{color:textColor,font:{size:10}}},y:{grid:{color:gridColor},ticks:{color:textColor,font:{size:10}}}}};
  bpChart=new Chart(document.getElementById('bp-chart'),{type:'line',data:{labels:bpData.map(d=>d.date),datasets:[
    {label:'Systolic',data:bpData.map(d=>d.sys),borderColor:'#e74c7c',backgroundColor:'rgba(231,76,124,0.1)',tension:0.4,fill:true,pointRadius:3},
    {label:'Diastolic',data:bpData.map(d=>d.dia),borderColor:'#9b59b6',backgroundColor:'rgba(155,89,182,0.08)',tension:0.4,fill:true,pointRadius:3}
  ]},options:{...opts,plugins:{legend:{display:true,labels:{color:textColor,font:{size:11},usePointStyle:true,pointStyle:'circle'}}}}});
  sugarChart=new Chart(document.getElementById('sugar-chart'),{type:'line',data:{labels:sugarData.map(d=>d.date),datasets:[
    {label:'Sugar mg/dL',data:sugarData.map(d=>d.val),borderColor:'#e67e22',backgroundColor:'rgba(230,126,34,0.1)',tension:0.4,fill:true,pointRadius:3}
  ]},options:opts});
}

let hrChartInited=false;
function initHRChart(){
  if(hrChartInited)return;hrChartInited=true;
  const isDark=matchMedia('(prefers-color-scheme:dark)').matches;
  const gridColor=isDark?'rgba(255,255,255,0.08)':'rgba(0,0,0,0.06)';
  const textColor=isDark?'#aaa':'#888';
  hrChart=new Chart(document.getElementById('hr-chart'),{type:'line',data:{labels:hrLabels,datasets:[
    {label:'Heart rate',data:[...hrData],borderColor:'#e74c7c',backgroundColor:'rgba(231,76,124,0.1)',tension:0.4,fill:true,pointRadius:3}
  ]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:gridColor},ticks:{color:textColor,font:{size:10}}},y:{grid:{color:gridColor},ticks:{color:textColor,font:{size:10}},min:50,max:150}}}});
}

function setHR(bpm, state){
  document.getElementById('hr-val').textContent=bpm;
  const el=document.getElementById('hr-status');
  if(state==='alert'){
    el.className='badge missed';
    el.innerHTML='<i class="ti ti-alert-triangle" aria-hidden="true"></i> High — Guardian notified';
    alerts.unshift({type:'bad',text:'CareBand alert: Heart rate '+bpm+' bpm detected — guardian notified'});
  } else {
    el.className='badge taken';
    el.innerHTML='<i class="ti ti-heart" aria-hidden="true"></i> Normal';
  }
  hrData.push(bpm);if(hrData.length>7)hrData.shift();
  hrLabels.push('Now');if(hrLabels.length>7)hrLabels.shift();
  if(hrChart){hrChart.data.datasets[0].data=[...hrData];hrChart.data.labels=[...hrLabels];hrChart.update();}
}

function renderFamily(){
  const pv=document.getElementById('family-parent-view');
  const gv=document.getElementById('family-guardian-view');
  if(role==='parent'){
    pv.style.display='block';gv.style.display='none';
    pv.innerHTML=`
      <div class="section-label">Weekly AI summary</div>
      <div class="card">
        <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:10px">
          <span style="font-size:14px;font-weight:600">Health report</span>
          <button class="btn btn-sm btn-primary" onclick="getAISummary()"><i class="ti ti-sparkles" aria-hidden="true"></i> Generate summary</button>
        </div>
        <div id="ai-output"><p style="font-size:13px;color:var(--text-secondary)">Click "Generate summary" to get an AI-powered weekly health insight.</p></div>
      </div>
      <div class="section-label">Doctor report</div>
      <div class="card">
        <p style="font-size:13px;color:var(--text-secondary);margin-bottom:12px">Generate a PDF report to show your doctor at your next appointment.</p>
        <div class="metric-grid" style="margin-bottom:12px">
          <div class="metric"><div class="metric-label">Adherence</div><div class="metric-val good">94%</div></div>
          <div class="metric"><div class="metric-label">Avg BP</div><div class="metric-val warn">132/85</div></div>
          <div class="metric"><div class="metric-label">Avg Sugar</div><div class="metric-val warn">137</div></div>
        </div>
        <button class="btn btn-primary" style="width:100%;justify-content:center" onclick="generateReport()"><i class="ti ti-file-download" aria-hidden="true"></i> Download PDF report</button>
      </div>`;
  } else {
    pv.style.display='none';gv.style.display='block';
    const taken=medicines.filter(m=>m.status==='taken').length;
    gv.innerHTML=`
      <div class="section-label">Dad's status today</div>
      <div class="metric-grid">
        <div class="metric"><div class="metric-label">Medicines</div><div class="metric-val ${taken===medicines.length?'good':'warn'}">${taken}/${medicines.length}</div></div>
        <div class="metric"><div class="metric-label">BP logged</div><div class="metric-val good">Yes</div></div>
        <div class="metric"><div class="metric-label">Last active</div><div class="metric-val good">2h ago</div></div>
      </div>
      <div class="section-label">Alerts</div>
      <div class="card">
        ${alerts.map(a=>`
          <div class="alert-item">
            <div class="alert-dot ${a.type}"></div>
            <div style="flex:1"><div style="font-size:13px">${a.text}</div></div>
          </div>`).join('')}
      </div>
      <div class="section-label">Recent health trend</div>
      <div class="card">
        <div style="margin-bottom:8px;display:flex;gap:10px;flex-wrap:wrap">
          <span class="badge taken"><i class="ti ti-trending-up"></i> BP stable this week</span>
          <span class="badge pending"><i class="ti ti-alert-triangle"></i> Sugar above target twice</span>
        </div>
        <p style="font-size:13px;color:var(--text-secondary)">Medicine adherence this week: <strong>6/7 days</strong>. Overall trend is stable. One missed Amlodipine dose on Jun 16.</p>
      </div>`;
  }
}

function getAISummary(){
  const out=document.getElementById('ai-output');
  out.innerHTML='<div style="display:flex;align-items:center;gap:8px;color:var(--text-secondary);font-size:13px"><div class="spinner"></div> Generating AI health summary...</div>';
  const bpAvgSys=Math.round(bpData.reduce((s,d)=>s+d.sys,0)/bpData.length);
  const bpAvgDia=Math.round(bpData.reduce((s,d)=>s+d.dia,0)/bpData.length);
  const sugarAvg=Math.round(sugarData.reduce((s,d)=>s+d.val,0)/sugarData.length);
  setTimeout(()=>{
    const text=aiSummaries[0];
    out.innerHTML=`<div class="ai-box"><p>${text}</p></div>
      <div style="margin-top:10px;display:flex;gap:8px;flex-wrap:wrap">
        <span class="tag"><i class="ti ti-pill" style="font-size:11px"></i> Adherence: 94%</span>
        <span class="tag"><i class="ti ti-activity" style="font-size:11px"></i> BP: ${bpAvgSys}/${bpAvgDia}</span>
        <span class="tag"><i class="ti ti-droplet" style="font-size:11px"></i> Sugar avg: ${sugarAvg}</span>
      </div>`;
  }, 900);
}

function generateReport(){
  alert('PDF report feature: In the full app, this generates a doctor-ready PDF with BP trends, sugar trends, medicine adherence, and missed doses, using jsPDF as outlined in the project design doc.');
}

renderHome();
renderMeds();
renderFamily();
</script>
</body>
</html>
