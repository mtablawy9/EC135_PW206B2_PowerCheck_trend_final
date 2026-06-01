<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>EC135 PW206B2 Inflight Power Check + Trend</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/modern-normalize/1.1.0/modern-normalize.min.css">
<style>
  :root{
    --ink:#1c2430; --muted:#5b6776; --line:#d9dee6; --bg:#f4f6f9;
    --card:#ffffff; --accent:#0b5d8a; --warn:#b3261e; --ok:#1c7a3e;
    --warnbg:#fdecea; --okbg:#e8f5ec;
  }
  *{box-sizing:border-box;}
  body{font-family:"Segoe UI",Tahoma,Arial,sans-serif;color:var(--ink);
       background:var(--bg);max-width:920px;margin:24px auto;padding:0 16px;line-height:1.5;}
  h1{font-size:1.35rem;letter-spacing:.2px;margin:.2rem 0;}
  h2{font-size:1.02rem;margin:0 0 .2rem;}
  .sub{color:var(--muted);font-size:.9rem;margin-top:0;}
  .card{background:var(--card);border:1px solid var(--line);border-radius:10px;
        padding:16px 18px;margin-top:16px;box-shadow:0 1px 2px rgba(0,0,0,.04);}
  .banner{background:var(--warnbg);border:1px solid #f3b9b4;border-left:4px solid var(--warn);
          color:#7a1812;border-radius:8px;padding:10px 14px;font-size:.9rem;}
  label{display:block;font-size:.9rem;font-weight:600;margin-top:12px;}
  label small{font-weight:400;color:var(--muted);}
  input[type="number"],input[type="text"],input[type="date"]{
       width:200px;padding:7px 9px;margin-top:4px;border:1px solid var(--line);
       border-radius:6px;font-size:.95rem;}
  select{width:280px;padding:7px 9px;margin-top:4px;border:1px solid var(--line);
         border-radius:6px;font-size:.95rem;background:#fff;}
  .checks label{font-weight:500;display:flex;align-items:center;gap:8px;}
  .checks input{width:auto;margin-top:0;}
  .err{color:var(--warn);font-size:.82rem;margin-top:3px;min-height:1em;}
  .row{display:flex;flex-wrap:wrap;gap:10px;margin-top:16px;align-items:center;}
  .inline-fields{display:flex;flex-wrap:wrap;gap:18px;}
  button{padding:9px 16px;border:1px solid var(--accent);background:var(--accent);color:#fff;
         border-radius:6px;font-size:.92rem;cursor:pointer;}
  button.secondary{background:#fff;color:var(--accent);}
  button.danger{background:#fff;color:var(--warn);border-color:var(--warn);}
  button:disabled{opacity:.5;cursor:not-allowed;}
  .kv{display:flex;justify-content:space-between;border-bottom:1px dashed var(--line);
      padding:8px 0;font-size:.95rem;}
  .kv:last-child{border-bottom:none;}
  .kv b{font-variant-numeric:tabular-nums;}
  .verdict{margin-top:10px;padding:10px 12px;border-radius:8px;font-size:.92rem;}
  .verdict.ok{background:var(--okbg);border:1px solid #b6e0c2;color:var(--ok);}
  .verdict.warn{background:var(--warnbg);border:1px solid #f3b9b4;color:var(--warn);}
  table{width:100%;border-collapse:collapse;margin-top:10px;font-size:.85rem;}
  th,td{text-align:right;padding:6px 8px;border-bottom:1px solid var(--line);
        font-variant-numeric:tabular-nums;white-space:nowrap;}
  th{color:var(--muted);font-weight:600;background:#fafbfc;}
  td:first-child,th:first-child{text-align:left;}
  .delbtn{padding:2px 9px;font-size:.78rem;}
  .stats{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:10px;margin-top:12px;}
  .stat{border:1px solid var(--line);border-radius:8px;padding:10px 12px;background:#fafbfc;}
  .stat .lbl{font-size:.76rem;color:var(--muted);}
  .stat .val{font-size:1.08rem;font-weight:700;font-variant-numeric:tabular-nums;margin-top:2px;}
  .ok-text{color:var(--ok);} .warn-text{color:var(--warn);}
</style>
</head>
<body>
  <h1>EC135 PW206B2 Inflight Power Check</h1>
  <p class="sub">60% Torque, Bleed OFF — TOT margin estimate + per-engine deterioration trend.</p>
 
  <!-- ENGINE IDENTIFICATION -->
  <div class="card">
    <h2>Engine identification</h2>
    <div class="inline-fields">
      <label>Engine Serial Number — S/N <small>key for history</small>
        <input id="sn" type="text" placeholder="e.g. PCE-CB1234" />
        <div class="err" id="snErr"></div>
      </label>
      <label>Aircraft Reg <small>optional</small>
        <input id="reg" type="text" placeholder="e.g. SU-XXX" />
      </label>
      <label>Date of check
        <input id="cdate" type="date" />
      </label>
      <label>Engine hours — EHRS <small>optional → °C/100h</small>
        <input id="ehrs" type="number" step="0.1" placeholder="e.g. 2450.5" />
      </label>
    </div>
  </div>
 
  <!-- POWER CHECK INPUTS -->
  <div class="card">
    <h2>Power check inputs</h2>
    <label>Pressure Altitude — PA (ft) <small>height by pressure</small>
      <input id="pa" type="number" value="0" step="100" />
      <div class="err" id="paErr"></div>
    </label>
 
    <label>Outside Air Temperature — OAT (°C)
      <input id="oat" type="number" value="15" step="0.1" />
      <div class="err" id="oatErr"></div>
    </label>
 
    <label>Measured TOT from engine (°C) <small>raw reading, before any filter correction</small>
      <input id="meas" type="number" step="0.1" placeholder="e.g. 700" />
      <div class="err" id="measErr"></div>
    </label>
 
    <label>Inlet filter installed <small>applies a TOT correction</small>
      <select id="filterSelect">
        <option value="none" selected>None (no correction)</option>
        <option value="sand">Sand Filter (−25 °C)</option>
        <option value="ibf">IBF — Inlet Barrier Filter (−12 °C)</option>
      </select>
    </label>
 
    <div class="checks" style="margin-top:12px;">
      <label><input type="checkbox" id="doplot" checked> Show chart</label>
    </div>
 
    <div class="row">
      <button id="calculate">Calculate</button>
      <button id="logBtn" class="secondary" disabled>Log reading to history</button>
      <button id="viewHistBtn" class="secondary">View / refresh engine history</button>
      <button id="downloadPdf" class="secondary" disabled>Download PDF report</button>
      <button id="reset" class="secondary">Reset</button>
    </div>
  </div>
 
  <!-- RESULTS -->
  <div id="results" class="card" style="display:none">
    <div class="kv"><span>Expected TOT (model)</span><b id="expVal"></b></div>
    <div class="kv"><span>Measured TOT (after correction)</span><b id="measVal"></b></div>
    <div class="kv"><span>ΔTOT (Expected − Measured)</span><b id="deltaVal"></b></div>
    <div class="kv"><span>Margin to MCP limit (Limit − Measured)</span><b id="marginVal"></b></div>
    <div id="verdict" class="verdict"></div>
  </div>
 
  <!-- CURRENT CALCULATION CHART -->
  <div id="plot" class="card" style="display:none">
    <div id="chart" style="width:100%;height:520px;"></div>
  </div>
 
  <!-- ENGINE HISTORY -->
  <div id="histCard" class="card" style="display:none">
    <h2>Engine history</h2>
    <p class="sub" id="histSummary"></p>
    <table>
      <thead>
        <tr>
          <th>Date</th><th>EHRS</th><th>PA</th><th>OAT</th>
          <th>TOT corr</th><th>ΔTOT</th><th>Margin</th><th></th>
        </tr>
      </thead>
      <tbody id="histTableBody"></tbody>
    </table>
    <div class="row">
      <button id="exportBtn" class="secondary">Export JSON (backup)</button>
      <label class="secondary" style="font-weight:500;margin:0;">
        <button id="importBtn" class="secondary" type="button">Import JSON</button>
        <input id="importFile" type="file" accept="application/json" style="display:none" />
      </label>
      <button id="demoBtn" class="secondary">Load demo data</button>
      <button id="clearEngineBtn" class="danger">Clear this engine</button>
    </div>
    <p class="sub" id="storageNote" style="margin-top:10px;"></p>
  </div>
 
  <!-- TREND -->
  <div id="trendCard" class="card" style="display:none">
    <h2>Deterioration trend</h2>
    <label style="font-weight:600;">Trend metric
      <select id="metric">
        <option value="margin">Margin to MCP limit (as requested)</option>
        <option value="delta">ΔTOT — condition-normalized (recommended)</option>
      </select>
    </label>
    <p class="sub" style="margin:6px 0 0;">
      Margin changes with OAT/PA, so on hot days it drops even on a healthy engine.
      ΔTOT compares each reading to the model at that day's conditions, so it isolates real
      deterioration better. Best fit = least-squares line.
    </p>
    <div class="stats" id="trendStats"></div>
    <p class="sub" id="trendNote" style="margin-top:10px;"></p>
    <div id="trendChart" style="width:100%;height:440px;margin-top:6px;"></div>
  </div>
 
<script src="https://cdnjs.cloudflare.com/ajax/libs/plotly.js/2.24.1/plotly.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script>
(() => {
  "use strict";
 
  /* ============================================================
     PLACEHOLDER MODEL — REPLACE WITH CERTIFIED CHART DATA.
     TOT = A0 + c_intercept*PA + (B0 + c_slope*PA)*OAT
     ============================================================ */
  const MODEL = {
    A0: 660, B0: 2.50, c_intercept: 0.00775, c_slope: 0.000075,
    MCP_LIMIT: 835, SAND_CORR: 25, IBF_CORR: 12
  };
 
  const BOUNDS = { pa:[-2000,25000], oat:[-60,60], meas:[200,1200] };
  const LS_KEY = 'ec135_pw206b2_history_v1';
 
  const el = id => document.getElementById(id);
  const totFn = (pa, oat) => MODEL.A0 + MODEL.c_intercept*pa + (MODEL.B0 + MODEL.c_slope*pa)*oat;
  const dayNum = ds => Date.parse(ds)/86400000;
 
  let lastResult = null;
  let _mem = [];
 
  /* ---------- storage ---------- */
  function loadHistory(){
    try { const s = localStorage.getItem(LS_KEY); return s ? JSON.parse(s) : []; }
    catch(e){ return _mem; }
  }
  function saveHistory(arr){
    _mem = arr;
    try { localStorage.setItem(LS_KEY, JSON.stringify(arr)); } catch(e){ /* session-only */ }
  }
  function getRecords(sn){
    return loadHistory().filter(r => r.sn === sn)
      .sort((a,b) => a.date < b.date ? -1 : a.date > b.date ? 1 : (a.id < b.id ? -1 : 1));
  }
 
  /* ---------- math ---------- */
  function linregress(pts){
    const n = pts.length;
    if (n < 2) return null;
    let sx=0, sy=0, sxx=0, sxy=0, syy=0;
    for (const p of pts){ sx+=p.x; sy+=p.y; sxx+=p.x*p.x; sxy+=p.x*p.y; syy+=p.y*p.y; }
    const denom = n*sxx - sx*sx;
    if (denom === 0) return null;
    const slope = (n*sxy - sx*sy)/denom;
    const intercept = (sy - slope*sx)/n;
    const ssTot = syy - sy*sy/n;
    let ssRes = 0;
    for (const p of pts){ const yp = slope*p.x + intercept; ssRes += (p.y-yp)*(p.y-yp); }
    const r2 = ssTot === 0 ? 1 : 1 - ssRes/ssTot;
    return { slope, intercept, r2, n };
  }
 
  function computeTrend(recs, metric){
    const pts = recs.map(r => ({ x: dayNum(r.date), y: r[metric], date: r.date }))
                    .filter(p => Number.isFinite(p.x) && Number.isFinite(p.y));
    const out = { n: pts.length };
    if (pts.length){ out.first = pts[0]; out.last = pts[pts.length-1]; }
    const lr = linregress(pts);
    out.lr = lr;
    if (lr){
      out.perDay = lr.slope;
      out.perMonth = lr.slope*30.4375;
      out.perYear = lr.slope*365.25;
      out.r2 = lr.r2;
      if (metric === 'margin' && lr.slope < 0){
        const x0 = -lr.intercept/lr.slope;
        if (Number.isFinite(x0)){
          out.zeroX = x0;
          out.daysToZero = x0 - out.last.x;
          const dz = new Date(x0*86400000);
          if (!isNaN(dz.getTime())) out.zeroDate = dz.toISOString().slice(0,10);
        }
      }
    }
    const hpts = recs.filter(r => Number.isFinite(r.ehrs)).map(r => ({ x:r.ehrs, y:r[metric] }));
    if (hpts.length === recs.length && hpts.length >= 2){
      const lrh = linregress(hpts);
      if (lrh){ out.per100h = lrh.slope*100; out.r2h = lrh.r2; }
    }
    return out;
  }
 
  /* ---------- core power check (unchanged logic) ---------- */
  function readField(id, errId, range){
    const raw = el(id).value.trim();
    el(errId).textContent = "";
    if (raw === ""){ el(errId).textContent = "Required."; return NaN; }
    const v = Number(raw);
    if (!Number.isFinite(v)){ el(errId).textContent = "Enter a number."; return NaN; }
    if (v < range[0] || v > range[1]){
      el(errId).textContent = `Out of range (${range[0]} to ${range[1]}).`; return NaN;
    }
    return v;
  }
 
  function calculateAndShow(){
    const PA  = readField('pa','paErr',BOUNDS.pa);
    const OAT = readField('oat','oatErr',BOUNDS.oat);
    const RAW = readField('meas','measErr',BOUNDS.meas);
    if ([PA,OAT,RAW].some(v => !Number.isFinite(v))){
      el('results').style.display='none'; el('plot').style.display='none';
      el('downloadPdf').disabled = true; el('logBtn').disabled = true; return;
    }
 
    const filter = el('filterSelect').value;
    let measured = RAW;
    let adj = "no correction";
    if (filter === 'sand'){ measured -= MODEL.SAND_CORR; adj = `-${MODEL.SAND_CORR} C (Sand Filter)`; }
    else if (filter === 'ibf'){ measured -= MODEL.IBF_CORR; adj = `-${MODEL.IBF_CORR} C (IBF)`; }
 
    const expected = totFn(PA, OAT);
    const delta = expected - measured;
    const margin = MODEL.MCP_LIMIT - measured;
 
    lastResult = { PA, OAT, RAW, measured, adj, expected, delta, margin };
 
    el('expVal').textContent   = `${expected.toFixed(1)} °C`;
    el('measVal').textContent  = `${measured.toFixed(1)} °C (${adj})`;
    el('deltaVal').textContent = `${delta.toFixed(1)} °C`;
    el('marginVal').textContent= `${margin.toFixed(1)} °C`;
 
    const v = el('verdict');
    if (measured >= MODEL.MCP_LIMIT){
      v.className = "verdict warn";
      v.innerHTML = `Measured TOT is at or above the MCP limit (${MODEL.MCP_LIMIT} °C). Investigate before continued operation.`;
    } else if (delta < 0){
      v.className = "verdict warn";
      v.innerHTML = `Engine is running <b>hotter</b> than the model predicts by ${Math.abs(delta).toFixed(1)} °C. Lower margin — compare against the official RFM power-check chart.`;
    } else {
      v.className = "verdict ok";
      v.innerHTML = `Engine is running <b>at/below</b> the predicted TOT (margin ${delta.toFixed(1)} °C vs model). Confirm against the RFM chart — this is an estimate, not a pass result.`;
    }
 
    el('results').style.display = 'block';
    el('downloadPdf').disabled = false;
    el('logBtn').disabled = false;
 
    if (el('doplot').checked){ el('plot').style.display='block'; drawPlot(); }
    else { el('plot').style.display='none'; Plotly.purge('chart'); }
  }
 
  function drawPlot(){
    const { PA, OAT, expected, measured } = lastResult;
    const lo = Math.min(-20, Math.floor(OAT) - 5);
    const hi = Math.max(55,  Math.ceil(OAT)  + 5);
    const OAT_vec = [];
    for (let t = lo; t <= hi; t++) OAT_vec.push(t);
 
    const PA_list = [0,2000,4000,6000,8000,10000];
    const traces = PA_list.map(pa => ({
      x: OAT_vec, y: OAT_vec.map(o => totFn(pa,o)),
      mode:'lines', name:`PA = ${pa} ft`, line:{width:2}
    }));
    traces.push({ x:[OAT], y:[expected], mode:'markers', name:'Expected',
                  marker:{color:'#111', size:10, symbol:'circle'} });
    traces.push({ x:[OAT], y:[measured], mode:'markers', name:'Measured',
                  marker:{color:'#b3261e', size:13, symbol:'x'} });
    traces.push({ x:[lo,hi], y:[MODEL.MCP_LIMIT,MODEL.MCP_LIMIT], mode:'lines',
                  name:`MCP limit (${MODEL.MCP_LIMIT} °C)`, line:{dash:'dash',color:'#6b21a8',width:2} });
 
    const layout = {
      title:'EC135 PW206B2 — TOT vs OAT (60% Torque, Bleed OFF)',
      xaxis:{title:'OAT (°C)'}, yaxis:{title:'TOT (°C)'},
      showlegend:true, margin:{t:50},
      annotations:[{
        x: OAT, y: (expected+measured)/2, xshift:8, xanchor:'left',
        text:`ΔTOT = ${(expected-measured).toFixed(1)} °C`,
        showarrow:false, font:{color:'#0b5d8a', size:12}
      }]
    };
    Plotly.react('chart', traces, layout, {responsive:true});
  }
 
  /* ---------- history logging ---------- */
  function logReading(){
    if (!lastResult){ alert('Calculate a valid power check first.'); return; }
    const sn = (el('sn').value || '').trim().toUpperCase();
    if (!sn){ el('snErr').textContent = 'Enter Engine S/N to log this reading.'; return; }
    el('snErr').textContent = '';
 
    const date = el('cdate').value || new Date().toISOString().slice(0,10);
    const reg  = (el('reg').value || '').trim().toUpperCase();
    const ehrsRaw = (el('ehrs').value || '').trim();
    const ehrs = ehrsRaw === '' ? null : Number(ehrsRaw);
 
    const dup = getRecords(sn).some(r => r.date === date);
    if (dup && !confirm(`A reading for ${sn} on ${date} already exists. Add another anyway?`)) return;
 
    const rec = {
      id: Date.now().toString(36) + Math.random().toString(36).slice(2,7),
      sn, reg, date,
      ehrs: Number.isFinite(ehrs) ? ehrs : null,
      pa: lastResult.PA, oat: lastResult.OAT,
      rawTot: +lastResult.RAW.toFixed(1),
      corr: lastResult.adj,
      measured: +lastResult.measured.toFixed(1),
      expected: +lastResult.expected.toFixed(1),
      delta: +lastResult.delta.toFixed(1),
      margin: +lastResult.margin.toFixed(1)
    };
    const all = loadHistory();
    all.push(rec);
    saveHistory(all);
    renderHistory(sn);
    el('histCard').scrollIntoView({ behavior:'smooth', block:'start' });
  }
 
  /* ---------- rendering ---------- */
  function stat(lbl, val, cls=''){
    return `<div class="stat"><div class="lbl">${lbl}</div><div class="val ${cls}">${val}</div></div>`;
  }
 
  function renderStats(t, metric, recs){
    const cards = [];
    cards.push(stat('Readings', t.n));
    cards.push(stat('Period (days)', t.lr ? Math.round(t.last.x - t.first.x) : '—'));
    const curMargin = recs[recs.length-1].margin;
    cards.push(stat('Current margin', curMargin.toFixed(1)+' °C', curMargin > 0 ? 'ok-text' : 'warn-text'));
 
    if (t.lr){
      const pm = t.perMonth;
      const arrow = pm < 0 ? '▼' : pm > 0 ? '▲' : '■';
      const cls = pm < 0 ? 'warn-text' : pm > 0 ? 'ok-text' : '';
      cards.push(stat(`Trend (${metric==='margin'?'margin':'ΔTOT'})`, `${arrow} ${pm.toFixed(2)} °C/mo`, cls));
      cards.push(stat('R² (fit)', t.r2.toFixed(3)));
      if (t.per100h != null){
        const a = t.per100h < 0 ? '▼' : t.per100h > 0 ? '▲' : '■';
        cards.push(stat('Per 100 EHRS', `${a} ${t.per100h.toFixed(2)} °C`, t.per100h < 0 ? 'warn-text' : 'ok-text'));
      }
      if (metric === 'margin'){
        let proj = 'not declining', pc = '';
        if (t.zeroDate){
          const yr = +t.zeroDate.slice(0,4);
          if (t.daysToZero <= 0){ proj = 'at/below limit now'; pc = 'warn-text'; }
          else if (yr > new Date().getFullYear()+50){ proj = '>50 yr (negligible)'; }
          else { proj = `${t.zeroDate} (~${Math.round(t.daysToZero)} d)`; pc = 'warn-text'; }
        }
        cards.push(stat('Proj. margin → 0', proj, pc));
      }
    } else {
      cards.push(stat('Trend', 'need ≥2 readings'));
    }
    el('trendStats').innerHTML = cards.join('');
  }
 
  function interpText(t, metric){
    if (!t.lr) return 'Add at least two readings on different dates to compute a deterioration rate. Estimate only — placeholder model.';
    const pm = t.perMonth;
    const word = metric === 'margin' ? 'Margin' : 'ΔTOT';
    let s;
    if (pm < -0.1){
      s = `${word} is decreasing ≈ ${Math.abs(pm).toFixed(2)} °C/month — engine running progressively hotter (deterioration).`;
      if (metric === 'margin' && t.zeroDate && t.daysToZero > 0)
        s += ` At this rate it is projected to reach the MCP limit around ${t.zeroDate}.`;
    } else if (pm > 0.1){
      s = `${word} is increasing ≈ ${pm.toFixed(2)} °C/month — no deterioration trend (post-maintenance, technique, or data scatter).`;
    } else {
      s = `${word} is essentially stable (≈ ${pm.toFixed(2)} °C/month).`;
    }
    return s + ' Estimate only — placeholder model and consistent technique assumed.';
  }
 
  function drawTrend(recs, metric){
    const t = computeTrend(recs, metric);
    const dates = recs.map(r => r.date);
    const yvals = recs.map(r => r[metric]);
    const yTitle = metric === 'margin' ? 'Margin to MCP limit (°C)' : 'ΔTOT = Expected − Measured (°C)';
 
    const traces = [{
      x: dates, y: yvals, mode:'lines+markers', name:'Readings',
      line:{width:1.5,color:'#0b5d8a'}, marker:{size:8,color:'#0b5d8a'}
    }];
 
    if (t.lr){
      const yStart = t.lr.slope*t.first.x + t.lr.intercept;
      const yEnd   = t.lr.slope*t.last.x  + t.lr.intercept;
      traces.push({ x:[t.first.date, t.last.date], y:[yStart, yEnd], mode:'lines',
                    name:'Best fit', line:{width:2,color:'#b3261e'} });
      if (t.zeroDate && t.daysToZero > 0){
        traces.push({ x:[t.last.date, t.zeroDate], y:[yEnd, 0], mode:'lines',
                      name:'Projection → 0', line:{width:2,dash:'dot',color:'#b3261e'} });
      }
    }
    if (metric === 'margin'){
      const xEnd = (t.zeroDate && t.daysToZero > 0) ? t.zeroDate : dates[dates.length-1];
      traces.push({ x:[dates[0], xEnd], y:[0,0], mode:'lines',
                    name:'Margin = 0 (MCP limit)', line:{width:1.5,dash:'dash',color:'#6b21a8'} });
    }
 
    const layout = {
      title:`Engine ${recs[0].sn} — ${metric==='margin'?'Margin':'ΔTOT'} trend vs date`,
      xaxis:{title:'Date'}, yaxis:{title:yTitle}, showlegend:true, margin:{t:50}
    };
    Plotly.react('trendChart', traces, layout, {responsive:true});
  }
 
  function renderHistory(snRaw){
    const sn = (snRaw || '').trim().toUpperCase();
    if (!sn){ el('snErr').textContent = 'Enter Engine S/N.'; return; }
    el('snErr').textContent = '';
    el('sn').value = sn;
 
    const recs = getRecords(sn);
    el('histCard').style.display = 'block';
 
    if (recs.length === 0){
      el('histSummary').textContent = `No readings logged for S/N ${sn} yet. Do a power check and click "Log reading to history".`;
      el('histTableBody').innerHTML = '';
      el('trendStats').innerHTML = '';
      el('trendNote').textContent = '';
      el('trendCard').style.display = 'none';
      Plotly.purge('trendChart');
      return;
    }
 
    const reg = (recs.find(r => r.reg) || {}).reg || '—';
    el('histSummary').textContent =
      `${recs.length} reading(s) — S/N ${sn} — Reg ${reg} — ${recs[0].date} → ${recs[recs.length-1].date}`;
 
    el('histTableBody').innerHTML = recs.map(r => `
      <tr>
        <td>${r.date}</td>
        <td>${r.ehrs == null ? '—' : r.ehrs}</td>
        <td>${r.pa}</td>
        <td>${r.oat}</td>
        <td>${r.measured.toFixed(1)}</td>
        <td>${r.delta.toFixed(1)}</td>
        <td>${r.margin.toFixed(1)}</td>
        <td><button class="secondary delbtn" data-id="${r.id}">✕</button></td>
      </tr>`).join('');
 
    const metric = el('metric').value;
    const t = computeTrend(recs, metric);
    renderStats(t, metric, recs);
    el('trendNote').textContent = interpText(t, metric);
    el('trendCard').style.display = 'block';
    drawTrend(recs, metric);
  }
 
  /* ---------- data management ---------- */
  function exportJSON(){
    const data = loadHistory();
    const blob = new Blob([JSON.stringify(data, null, 2)], { type:'application/json' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `EC135_PW206B2_history_${new Date().toISOString().slice(0,10)}.json`;
    document.body.appendChild(a); a.click(); a.remove();
    URL.revokeObjectURL(url);
  }
 
  function importJSON(file){
    const r = new FileReader();
    r.onload = () => {
      try {
        const incoming = JSON.parse(r.result);
        if (!Array.isArray(incoming)) throw new Error('file is not a record array');
        const cur = loadHistory();
        const ids = new Set(cur.map(x => x.id));
        let added = 0;
        for (const rec of incoming){
          if (rec && rec.id && rec.sn && rec.date && !ids.has(rec.id)){
            cur.push(rec); ids.add(rec.id); added++;
          }
        }
        saveHistory(cur);
        alert(`Imported ${added} new record(s).`);
        if (el('sn').value.trim()) renderHistory(el('sn').value);
      } catch(e){ alert('Import failed: ' + e.message); }
    };
    r.readAsText(file);
  }
 
  function clearEngine(){
    const sn = (el('sn').value || '').trim().toUpperCase();
    if (!sn){ alert('Enter the S/N to clear.'); return; }
    if (!confirm(`Delete ALL readings for S/N ${sn}? This cannot be undone (export first to keep a backup).`)) return;
    saveHistory(loadHistory().filter(r => r.sn !== sn));
    renderHistory(sn);
  }
 
  function loadDemo(){
    const base = loadHistory();
    if (!base.some(r => r.sn === 'DEMO-0001')){
      const start = new Date(); start.setMonth(start.getMonth() - 5);
      const margins = [168, 162, 159, 151, 147, 140]; // gently declining
      for (let i = 0; i < 6; i++){
        const d = new Date(start); d.setMonth(start.getMonth() + i);
        const date = d.toISOString().slice(0,10);
        const margin = margins[i];
        const measured = MODEL.MCP_LIMIT - margin;
        const expected = totFn(0, 15);
        base.push({
          id: 'demo' + i, sn: 'DEMO-0001', reg: 'DEMO', date,
          ehrs: 2000 + i*45, pa: 0, oat: 15, rawTot: measured, corr: 'no correction',
          measured, expected: +expected.toFixed(1),
          delta: +(expected - measured).toFixed(1), margin
        });
      }
      saveHistory(base);
    }
    renderHistory('DEMO-0001');
  }
 
  /* ---------- PDF (now includes engine ID) ---------- */
  function downloadPDF(){
    if (!lastResult) return;
    const { jsPDF } = window.jspdf;
    const d = lastResult;
    const doc = new jsPDF();
    doc.setFontSize(14);
    doc.text('EC135 PW206B2 Inflight Power Check Report', 12, 20);
    doc.setFontSize(9);
    doc.text('Illustrative model - not certified RFM data. Not for airworthiness decisions.', 12, 27);
    doc.setFontSize(11);
    const lines = [
      `Date: ${el('cdate').value || new Date().toLocaleDateString()}`,
      `Engine S/N: ${(el('sn').value||'-').toUpperCase()}    Aircraft Reg: ${(el('reg').value||'-').toUpperCase()}`,
      `Engine hours (EHRS): ${el('ehrs').value || '-'}`,
      `Pressure Altitude (ft): ${d.PA}`,
      `OAT (degC): ${d.OAT}`,
      `Measured TOT raw (degC): ${d.RAW.toFixed(1)}`,
      `Filter correction: ${d.adj}`,
      `Measured TOT corrected (degC): ${d.measured.toFixed(1)}`,
      `Expected TOT model (degC): ${d.expected.toFixed(1)}`,
      `Delta TOT (Expected - Measured) (degC): ${d.delta.toFixed(1)}`,
      `Margin to MCP limit (degC): ${d.margin.toFixed(1)}`
    ];
    lines.forEach((t,i) => doc.text(t, 12, 40 + i*8));
    doc.save('EC135_TOT_Report.pdf');
  }
 
  /* ---------- wiring ---------- */
  el('calculate').addEventListener('click', calculateAndShow);
  el('logBtn').addEventListener('click', logReading);
  el('viewHistBtn').addEventListener('click', () => renderHistory(el('sn').value));
  el('downloadPdf').addEventListener('click', downloadPDF);
  el('metric').addEventListener('change', () => { const sn = el('sn').value.trim(); if (sn) renderHistory(sn); });
  el('exportBtn').addEventListener('click', exportJSON);
  el('importBtn').addEventListener('click', () => el('importFile').click());
  el('importFile').addEventListener('change', e => { const f = e.target.files[0]; if (f) importJSON(f); e.target.value=''; });
  el('clearEngineBtn').addEventListener('click', clearEngine);
  el('demoBtn').addEventListener('click', loadDemo);
 
  el('histTableBody').addEventListener('click', e => {
    const b = e.target.closest('.delbtn'); if (!b) return;
    if (!confirm('Delete this reading?')) return;
    saveHistory(loadHistory().filter(r => r.id !== b.dataset.id));
    renderHistory(el('sn').value);
  });
 
  el('reset').addEventListener('click', () => {
    el('pa').value=0; el('oat').value=15; el('meas').value='';
    el('filterSelect').value='none';
    el('doplot').checked=true;
    ['paErr','oatErr','measErr'].forEach(id => el(id).textContent='');
    el('results').style.display='none'; el('plot').style.display='none';
    el('downloadPdf').disabled=true; el('logBtn').disabled=true;
    lastResult=null; Plotly.purge('chart');
  });
 
  /* ---------- init ---------- */
  el('cdate').value = new Date().toISOString().slice(0,10);
  (function(){
    try { localStorage.setItem('__t','1'); localStorage.removeItem('__t');
      el('storageNote').textContent = 'History is saved in this browser (localStorage). Use Export to keep a permanent backup before clearing browser data.';
    } catch(e){
      el('storageNote').textContent = 'Browser storage is unavailable here, so history is session-only. Use Export/Import to keep records between sessions.';
    }
  })();
})();
</script>
</body>
</html>
