# Figure: Net MP Partner Health Effect Across Interaction Models

**Paper:** *For Better or for Health? Medical Knowledge Spillovers and Health Inequality in European Partnerships*  
**Author:** Michaela Bunakova, Department of Sociology, McGill University  
**Data:** European Social Survey (ESS), pooled rounds 2002–2023  

---

## What this figure shows

A 2×2 panel figure displaying the **net association between having a medical practitioner (MP) partner and subjective health** (measured on a 1–5 scale) across four interaction models. Each panel corresponds to one moderating variable:

| Panel | Model | Moderator |
|-------|-------|-----------|
| Top-left | Model 5 | Welfare regime |
| Top-right | Model 6 | Age group |
| Bottom-left | Model 7 | Household income quintile |
| Bottom-right | Model 8 | GDP per capita tertile |

Each panel contains:
- A **horizontal bar chart** showing the net MP partner β at each category level, with 95% CI whiskers
- A **dashed zero line** (null effect)
- A **dashed red benchmark line** at the additive (non-interaction) MP partner effect (β = 0.074, Model 3)
- A **summary table** below each chart showing β in scale points, as a % of the sample mean (4.12), and significance markers

---

## How net effects are computed

All models estimate an interaction between the binary treatment indicator (`1.treat`, i.e. having an MP partner) and a moderating variable. The **net effect** at each moderator category is:

```
net_effect = β_treat (main effect at reference) + β_treat#moderator
```

For the **reference category**, the net effect equals the main effect coefficient directly.

### Model-specific reference effects

| Model | Reference category | β_treat (main effect) |
|-------|-------------------|----------------------|
| Model 5 | Post-Communist regime | 0.168 |
| Model 6 | Age 40–49 | 0.047 |
| Model 7 | Highest income quintile | 0.127 |
| Model 8 | Q1 (lowest GDP tertile) | 0.164 |

---

## Standard errors

Standard errors come directly from Stata's `e(V)` variance-covariance matrix after each interaction model. The SE used for each CI bar is the SE of the **interaction term** (for non-reference categories) or an approximated SE of 0.060 for reference categories.

```
95% CI = net_effect ± 1.96 × SE
```

### Stata commands used to extract SEs

```stata
* Run interaction model (example: Model 8)
mixed health i.treat##i.gdp_3cat [covariates] || country: , reml

* Extract variance-covariance matrix
matrix V = e(V)
matrix list V

* Or use estat vce for a formatted table
estat vce, covariance
```

The relevant entries are of the form `Cov(1.treat#2.gdp_3cat, 1.treat#2.gdp_3cat)` — the diagonal elements of the VCV matrix for each interaction term.

---

## A note on significance markers

The **star (*)** next to a category denotes that the *interaction term itself* is significantly different from zero (p < 0.05) — i.e., that category differs significantly from the reference.

This is **not** the same as the net effect being significantly different from zero. A category can have a significant interaction term (meaningfully different from reference) while its net effect CI still crosses zero. This occurs when the reference-category effect is large but the interaction pulls the estimate back toward zero. The GDP panel is the clearest example: Q2 and Q3 are significantly *lower* than Q1, but their net effects (≈ 0.032) are small enough that the CI includes zero.

---

## Data values

### Model 5 — Welfare regime (ref: Post-Communist, β = 0.168)

| Category | Interaction β | Net effect | SE | p < 0.05 |
|----------|--------------|------------|----|----------|
| Post-Communist | — (ref) | +0.168 | 0.060* | — |
| Social Democratic | −0.140 | +0.028 | 0.0847588 | No |
| Conservative | −0.106 | +0.062 | 0.0669493 | No |
| Liberal | −0.111 | +0.057 | 0.0854667 | No |
| Mediterranean | −0.203 | −0.035 | 0.0969082 | Yes |

### Model 6 — Age group (ref: 40–49, β = 0.047)

| Category | Interaction β | Net effect | SE | p < 0.05 |
|----------|--------------|------------|----|----------|
| 20–29 | +0.164 | +0.211 | 0.0883355 | No |
| 30–39 | +0.023 | +0.070 | 0.0608685 | No |
| 40–49 | — (ref) | +0.047 | 0.060* | — |
| 50–59 | +0.024 | +0.071 | 0.0796367 | No |
| 60–69 | +0.024 | +0.071 | 0.0964974 | No |

### Model 7 — Household income (ref: Highest, β = 0.127)

| Category | Interaction β | Net effect | SE | p < 0.05 |
|----------|--------------|------------|----|----------|
| Lowest | −0.129 | −0.002 | 0.3338671 | No |
| Low-middle | +0.131 | +0.258 | 0.1560928 | No |
| Middle | −0.127 | +0.000 | 0.1386543 | No |
| Upper-middle | −0.171 | −0.044 | 0.0690386 | Yes |
| Highest | — (ref) | +0.127 | 0.060* | — |

> **Note:** The very wide CI for the "Lowest" income category (SE = 0.334) reflects sparse representation of MP-partner households at the bottom of the income distribution in this elite-restricted analytical sample.

### Model 8 — GDP per capita tertile (ref: Q1 lowest, β = 0.164)

| Category | Interaction β | Net effect | SE | p < 0.05 |
|----------|--------------|------------|----|----------|
| Q1 — low | — (ref) | +0.164 | 0.060* | — |
| Q2 — middle | −0.132 | +0.032 | 0.0695443 | Yes |
| Q3 — high | −0.132 | +0.032 | 0.0638913 | Yes |

> **GDP source:** World Bank National Accounts Data, indicator `NY.GDP.MKTP.CD` (https://data.worldbank.org/indicator/NY.GDP.MKTP.CD). GDP per capita in current USD was retrieved for each country-year combination matching ESS survey rounds, log-transformed to address right skew, then categorised into three approximately equal tertiles using the individual-level distribution of the log-transformed variable.

*\* SE for reference categories is approximated at 0.060 (consistent with model-level SE estimates for the main treatment effect).*

---

## How to reproduce the figure

The figure is built in plain HTML + JavaScript using [Chart.js 4.4.1](https://www.chartjs.org/). No build step or server is required — open the `.html` file in any modern browser.

### Dependencies

| Library | Version | Source |
|---------|---------|--------|
| Chart.js | 4.4.1 | `https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js` |

### File

Save the code block below as `mp_interaction_panels.html` and open in a browser.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>MP Partner Interaction Effects</title>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Arial, sans-serif; background: #f5f4f0; padding: 32px 24px; }
  .panel-wrap { display: grid; grid-template-columns: repeat(2, minmax(0,1fr)); gap: 16px; max-width: 960px; margin-bottom: 16px; }
  .panel { background: #fff; border: 0.5px solid #d3d1c7; border-radius: 12px; padding: 16px 16px 14px; }
  .panel-title { font-size: 13px; font-weight: 600; color: #2c2c2a; margin-bottom: 2px; }
  .panel-sub { font-size: 10.5px; color: #888780; margin-bottom: 10px; }
  .panel-chart { position: relative; width: 100%; }
  .panel-table { width: 100%; border-collapse: collapse; margin-top: 10px; font-size: 11px; }
  .panel-table td { padding: 3px 6px; color: #5f5e5a; }
  .panel-table td:first-child { color: #2c2c2a; font-weight: 600; width: 38%; }
  .panel-table td:nth-child(2) { text-align: right; width: 20%; font-variant-numeric: tabular-nums; }
  .panel-table td:nth-child(3) { text-align: right; width: 24%; font-variant-numeric: tabular-nums; color: #888780; }
  .panel-table td:nth-child(4) { text-align: center; width: 18%; }
  .panel-table tr:nth-child(even) td { background: #f8f7f4; }
  .panel-table .hdr td { font-weight: 600; font-size: 10px; color: #888780; background: transparent !important; border-bottom: 0.5px solid #d3d1c7; padding-bottom: 4px; }
  .dot { display: inline-block; width: 8px; height: 8px; border-radius: 2px; vertical-align: middle; margin-right: 5px; }
  .note-row { display: flex; gap: 14px; flex-wrap: wrap; max-width: 960px; padding-top: 10px; border-top: 0.5px solid #d3d1c7; font-size: 10.5px; color: #888780; align-items: center; }
  .legend-item { display: flex; align-items: center; gap: 5px; white-space: nowrap; }
  .swatch { width: 10px; height: 10px; border-radius: 2px; flex-shrink: 0; }
  .swatch-dash { width: 18px; height: 0; flex-shrink: 0; border-top: 2px dashed; }
</style>
</head>
<body>

<div class="panel-wrap">
  <div class="panel" id="p5"></div>
  <div class="panel" id="p6"></div>
  <div class="panel" id="p7"></div>
  <div class="panel" id="p8"></div>
</div>

<div class="note-row">
  <div class="legend-item"><div class="swatch" style="background:rgba(24,95,165,0.82);border:1px solid #185FA5;"></div><span>Sig. different from ref. (p &lt; 0.05)</span></div>
  <div class="legend-item"><div class="swatch" style="background:rgba(136,135,128,0.42);border:1px solid #888780;"></div><span>Non-significant</span></div>
  <div class="legend-item"><div class="swatch" style="background:rgba(29,158,117,0.45);border:1px solid #1D9E75;"></div><span>Reference category</span></div>
  <div class="legend-item"><div class="swatch-dash" style="border-color:#E24B4A;"></div><span>Additive MP effect (β = 0.074)</span></div>
  <span style="margin-left:auto;">Scale: 1–5. Mean = 4.12. Bars = net effect ± 95% CI. * sig. different from ref. † reference.</span>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<script>
const MEAN = 4.12, BENCH = 0.074;
const C = {
  sig:'#185FA5', sigBg:'rgba(24,95,165,0.82)',
  ns:'#888780',  nsBg:'rgba(136,135,128,0.42)',
  ref:'#1D9E75', refBg:'rgba(29,158,117,0.45)',
  zero:'rgba(0,0,0,0.18)', bench:'#E24B4A',
  grid:'rgba(0,0,0,0.06)', label:'#444441', tick:'#73726c',
};

const panels = [
  {
    id:'p5', title:'Model 5 — welfare regime',
    sub:'ref: Post-Communist  ·  scale: 1–5  ·  mean = 4.12',
    yMin:-0.34, yMax:0.38,
    data:[
      {label:'Post-Communist', net:0.168,         se:0.060,     ref:true,  sig:false},
      {label:'Social Dem.',    net:0.168-0.140,   se:0.0847588, ref:false, sig:false},
      {label:'Conservative',  net:0.168-0.106,   se:0.0669493, ref:false, sig:false},
      {label:'Liberal',       net:0.168-0.111,   se:0.0854667, ref:false, sig:false},
      {label:'Mediterranean', net:0.168-0.203,   se:0.0969082, ref:false, sig:true },
    ],
  },
  {
    id:'p6', title:'Model 6 — age group',
    sub:'ref: 40–49  ·  scale: 1–5  ·  mean = 4.12',
    yMin:-0.24, yMax:0.42,
    data:[
      {label:'20–29', net:0.047+0.164, se:0.0883355, ref:false, sig:false},
      {label:'30–39', net:0.047+0.023, se:0.0608685, ref:false, sig:false},
      {label:'40–49', net:0.047,       se:0.060,     ref:true,  sig:false},
      {label:'50–59', net:0.047+0.024, se:0.0796367, ref:false, sig:false},
      {label:'60–69', net:0.047+0.024, se:0.0964974, ref:false, sig:false},
    ],
  },
  {
    id:'p7', title:'Model 7 — household income',
    sub:'ref: Highest income  ·  scale: 1–5  ·  mean = 4.12',
    yMin:-0.80, yMax:0.62,
    data:[
      {label:'Lowest',     net:0.127-0.129, se:0.3338671, ref:false, sig:false},
      {label:'Low-middle', net:0.127+0.131, se:0.1560928, ref:false, sig:false},
      {label:'Middle',     net:0.127-0.127, se:0.1386543, ref:false, sig:false},
      {label:'Upper-mid',  net:0.127-0.171, se:0.0690386, ref:false, sig:true },
      {label:'Highest',    net:0.127,       se:0.060,     ref:true,  sig:false},
    ],
  },
  {
    id:'p8', title:'Model 8 — GDP per capita tertile',
    sub:'ref: Q1 lowest GDP  ·  scale: 1–5  ·  mean = 4.12',
    yMin:-0.18, yMax:0.32,
    data:[
      {label:'Q1 — low',    net:0.164,        se:0.060,     ref:true,  sig:false},
      {label:'Q2 — middle', net:0.164-0.132,  se:0.0695443, ref:false, sig:true },
      {label:'Q3 — high',   net:0.164-0.132,  se:0.0638913, ref:false, sig:true },
    ],
  },
];

function makeZeroPlugin() {
  return { id:'z_'+Math.random().toString(36).slice(2), beforeDraw(c) {
    const{ctx,scales:{x}}=c; if(!x)return; const px=x.getPixelForValue(0);
    ctx.save(); ctx.strokeStyle=C.zero; ctx.lineWidth=1; ctx.setLineDash([4,3]);
    ctx.beginPath(); ctx.moveTo(px,c.chartArea.top); ctx.lineTo(px,c.chartArea.bottom); ctx.stroke();
    ctx.setLineDash([]); ctx.restore();
  }};
}

function makeBenchPlugin() {
  return { id:'b_'+Math.random().toString(36).slice(2), beforeDraw(c) {
    const{ctx,scales:{x}}=c; if(!x)return; const px=x.getPixelForValue(BENCH);
    ctx.save(); ctx.strokeStyle=C.bench; ctx.lineWidth=1; ctx.globalAlpha=0.65; ctx.setLineDash([5,4]);
    ctx.beginPath(); ctx.moveTo(px,c.chartArea.top); ctx.lineTo(px,c.chartArea.bottom); ctx.stroke();
    ctx.setLineDash([]); ctx.globalAlpha=1; ctx.restore();
  }};
}

function makeCiPlugin(nets, ses, pdata) {
  return { id:'ci_'+Math.random().toString(36).slice(2), afterDatasetsDraw(c) {
    const{ctx,scales:{x,y}}=c; if(!x||!y)return; ctx.save();
    for(let i=0;i<pdata.length;i++){
      const net=nets[i],se=ses[i];
      const xLo=x.getPixelForValue(net-1.96*se), xHi=x.getPixelForValue(net+1.96*se);
      const yPx=y.getPixelForValue(i);
      const col=pdata[i].ref?C.ref:(pdata[i].sig?C.sig:C.ns);
      ctx.strokeStyle=col; ctx.lineWidth=1.5;
      ctx.beginPath(); ctx.moveTo(xLo,yPx); ctx.lineTo(xHi,yPx); ctx.stroke();
      [xLo,xHi].forEach(t=>{ctx.beginPath();ctx.moveTo(t,yPx-4);ctx.lineTo(t,yPx+4);ctx.stroke();});
    }
    ctx.restore();
  }};
}

panels.forEach(p => {
  const container = document.getElementById(p.id);
  const titleEl = document.createElement('div'); titleEl.className='panel-title'; titleEl.textContent=p.title; container.appendChild(titleEl);
  const subEl   = document.createElement('div'); subEl.className='panel-sub';   subEl.textContent=p.sub;   container.appendChild(subEl);
  const wrap = document.createElement('div'); wrap.className='panel-chart'; wrap.style.height=(Math.max(100,p.data.length*32+50))+'px'; container.appendChild(wrap);
  const canvas = document.createElement('canvas'); wrap.appendChild(canvas);

  const nets=p.data.map(d=>+d.net.toFixed(4)), ses=p.data.map(d=>+d.se.toFixed(7));
  const bgC=p.data.map(d=>d.ref?C.refBg:(d.sig?C.sigBg:C.nsBg));
  const bdC=p.data.map(d=>d.ref?C.ref:(d.sig?C.sig:C.ns));

  new Chart(canvas,{
    type:'bar',
    data:{labels:p.data.map(d=>d.label),datasets:[{data:nets,backgroundColor:bgC,borderColor:bdC,borderWidth:1,borderRadius:3,barThickness:15}]},
    options:{
      indexAxis:'y', responsive:true, maintainAspectRatio:false, animation:false,
      layout:{padding:{right:10,top:4,bottom:4,left:2}},
      scales:{
        x:{min:p.yMin,max:p.yMax,grid:{color:C.grid},border:{display:false},
           ticks:{color:C.tick,font:{size:10},maxTicksLimit:6,callback(v){return Math.abs(v)<0.0001?'0':(v>0?'+':'')+v.toFixed(2);}},
           title:{display:true,text:'Change in subjective health (1–5 scale)',color:C.tick,font:{size:10}}},
        y:{grid:{display:false},border:{display:false},ticks:{color:C.label,font:{size:11}}}
      },
      plugins:{legend:{display:false},tooltip:{
        callbacks:{
          title:items=>{const i=items[0].dataIndex;return p.data[i].label+(p.data[i].sig?' *':'')+(p.data[i].ref?' (ref)':'');},
          label:items=>{const i=items[0].dataIndex,net=nets[i],se=ses[i],sign=net>=0?'+':'';
            return['Change: '+sign+net.toFixed(3)+' pts','Predicted: '+(MEAN+net).toFixed(3)+' / 5',
                   '% of mean: '+sign+((net/MEAN)*100).toFixed(2)+'%',
                   '95% CI: ['+(net-1.96*se).toFixed(3)+', '+(net+1.96*se).toFixed(3)+']',
                   'SE = '+se.toFixed(4)];}
        },
        backgroundColor:'#fff',borderColor:'rgba(0,0,0,0.12)',borderWidth:0.5,
        titleColor:C.label,bodyColor:C.tick,padding:10,cornerRadius:8,
      }}
    },
    plugins:[makeZeroPlugin(), makeBenchPlugin(), makeCiPlugin(nets,ses,p.data)]
  });

  const div=document.createElement('div'); div.style.cssText='border-top:0.5px solid #d3d1c7;margin-top:10px;padding-top:8px;';
  const tbl=document.createElement('table'); tbl.className='panel-table';
  const hdr=document.createElement('tr'); hdr.className='hdr';
  ['Category','β (pts)','% of mean',''].forEach(h=>{const td=document.createElement('td');td.textContent=h;hdr.appendChild(td);});
  tbl.appendChild(hdr);

  p.data.forEach((d,i)=>{
    const net=nets[i],pct=((net/MEAN)*100).toFixed(1),sign=net>=0?'+':'';
    const col=d.ref?C.ref:(d.sig?C.sig:C.ns), bg=d.ref?C.refBg:(d.sig?C.sigBg:C.nsBg);
    const tr=document.createElement('tr');
    const tdN=document.createElement('td'); tdN.innerHTML='<span class="dot" style="background:'+bg+';border:1px solid '+col+';"></span>'+d.label; tr.appendChild(tdN);
    const tdB=document.createElement('td'); tdB.style.cssText='color:'+col+';font-weight:600;'; tdB.textContent=sign+net.toFixed(3); tr.appendChild(tdB);
    const tdP=document.createElement('td'); tdP.textContent=sign+pct+'%'; tr.appendChild(tdP);
    const tdS=document.createElement('td'); tdS.style.cssText='color:'+col+';font-weight:600;text-align:center;'; tdS.textContent=d.ref?'†':(d.sig?'*':''); tr.appendChild(tdS);
    tbl.appendChild(tr);
  });
  div.appendChild(tbl); container.appendChild(div);
});
</script>
</body>
</html>
```

---

## Citation

If you use or adapt this figure, please cite:

> Bunakova, M. (*forthcoming*). For Better or for Health? Medical Knowledge Spillovers and Health Inequality in European Partnerships. McGill University.
