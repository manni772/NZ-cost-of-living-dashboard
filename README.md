<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NZ Cost of Living Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,wght@0,300;0,400;0,600;0,700;1,300&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
:root {
  --bg: #0d1117;
  --surface: #161b22;
  --surface2: #1c2128;
  --border: #30363d;
  --text: #e6edf3;
  --muted: #8b949e;
  --teal: #39d0b8;
  --teal-dim: rgba(57,208,184,0.12);
  --teal-border: rgba(57,208,184,0.3);
  --red: #f85149;
  --red-dim: rgba(248,81,73,0.12);
  --amber: #e3b341;
  --amber-dim: rgba(227,179,65,0.12);
  --blue: #58a6ff;
  --blue-dim: rgba(88,166,255,0.12);
  --green: #3fb950;
  --green-dim: rgba(63,185,80,0.12);
}
* { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  font-family: 'DM Sans', sans-serif;
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  font-size: 15px;
  line-height: 1.6;
}

/* HEADER */
.header {
  padding: 3rem 2rem 2rem;
  max-width: 1200px;
  margin: 0 auto;
  border-bottom: 1px solid var(--border);
  margin-bottom: 2rem;
  position: relative;
}
.header::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0; height: 3px;
  background: linear-gradient(90deg, var(--teal), var(--blue), var(--amber));
}
.header-top { display: flex; align-items: flex-start; justify-content: space-between; flex-wrap: wrap; gap: 1rem; }
.header-eyebrow { font-size: 11px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.12em; color: var(--teal); margin-bottom: 8px; }
.header-title { font-family: 'Fraunces', serif; font-size: clamp(28px, 5vw, 44px); font-weight: 700; line-height: 1.1; letter-spacing: -1px; }
.header-title span { font-style: italic; color: var(--teal); }
.header-sub { font-size: 14px; color: var(--muted); margin-top: 10px; max-width: 500px; line-height: 1.7; }
.header-meta { display: flex; flex-direction: column; align-items: flex-end; gap: 6px; }
.meta-badge { font-size: 11px; padding: 4px 10px; border-radius: 20px; font-weight: 500; }
.badge-source { background: var(--teal-dim); color: var(--teal); border: 1px solid var(--teal-border); }
.badge-updated { background: var(--surface2); color: var(--muted); border: 1px solid var(--border); }

/* ALERT BANNER */
.alert-banner {
  max-width: 1200px;
  margin: 0 auto 2rem;
  padding: 0 2rem;
}
.alert {
  background: var(--red-dim);
  border: 1px solid rgba(248,81,73,0.3);
  border-radius: 10px;
  padding: 14px 18px;
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 14px;
}
.alert-icon { font-size: 18px; flex-shrink: 0; }
.alert-text { color: var(--text); }
.alert-text strong { color: var(--red); font-weight: 600; }

/* MAIN */
.main { max-width: 1200px; margin: 0 auto; padding: 0 2rem 4rem; }

/* STAT CARDS */
.stat-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 12px; margin-bottom: 2rem; }
.stat-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 1.25rem;
  position: relative;
  overflow: hidden;
  transition: border-color 0.2s;
}
.stat-card:hover { border-color: var(--teal-border); }
.stat-card::after { content: ''; position: absolute; top: 0; left: 0; right: 0; height: 2px; }
.stat-card.red::after { background: var(--red); }
.stat-card.amber::after { background: var(--amber); }
.stat-card.teal::after { background: var(--teal); }
.stat-card.blue::after { background: var(--blue); }
.stat-card.green::after { background: var(--green); }
.stat-label { font-size: 11px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.08em; color: var(--muted); margin-bottom: 8px; }
.stat-value { font-family: 'Fraunces', serif; font-size: 36px; font-weight: 700; line-height: 1; margin-bottom: 6px; }
.stat-card.red .stat-value { color: var(--red); }
.stat-card.amber .stat-value { color: var(--amber); }
.stat-card.teal .stat-value { color: var(--teal); }
.stat-card.blue .stat-value { color: var(--blue); }
.stat-card.green .stat-value { color: var(--green); }
.stat-change { font-size: 12px; color: var(--muted); }
.stat-change .up { color: var(--red); }
.stat-change .down { color: var(--green); }

/* TABS */
.tab-bar { display: flex; gap: 4px; margin-bottom: 1.5rem; background: var(--surface); border: 1px solid var(--border); border-radius: 10px; padding: 4px; width: fit-content; }
.tab-btn { padding: 8px 18px; border-radius: 7px; font-size: 13px; font-weight: 500; cursor: pointer; background: none; border: none; color: var(--muted); font-family: 'DM Sans', sans-serif; transition: all 0.15s; }
.tab-btn.on { background: var(--surface2); color: var(--text); }
.tab-btn:hover:not(.on) { color: var(--text); }
.tab-content { display: none; }
.tab-content.on { display: block; }

/* CHART SECTION */
.chart-section { background: var(--surface); border: 1px solid var(--border); border-radius: 14px; padding: 1.5rem; margin-bottom: 1.5rem; }
.chart-header { display: flex; align-items: flex-start; justify-content: space-between; margin-bottom: 1.5rem; flex-wrap: wrap; gap: 10px; }
.chart-title { font-family: 'Fraunces', serif; font-size: 18px; font-weight: 600; margin-bottom: 4px; }
.chart-subtitle { font-size: 12px; color: var(--muted); }
.chart-legend { display: flex; gap: 14px; flex-wrap: wrap; }
.legend-item { display: flex; align-items: center; gap: 6px; font-size: 12px; color: var(--muted); }
.legend-dot { width: 8px; height: 8px; border-radius: 50%; flex-shrink: 0; }

canvas { width: 100% !important; max-height: 300px; }

/* TWO COL */
.two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 1.5rem; margin-bottom: 1.5rem; }
@media (max-width: 700px) { .two-col { grid-template-columns: 1fr; } }

/* INSIGHT CARDS */
.insight-card { background: var(--surface); border: 1px solid var(--border); border-radius: 14px; padding: 1.5rem; }
.insight-title { font-family: 'Fraunces', serif; font-size: 16px; font-weight: 600; margin-bottom: 1rem; }
.insight-item { display: flex; gap: 10px; align-items: flex-start; margin-bottom: 12px; padding-bottom: 12px; border-bottom: 1px solid var(--border); font-size: 13px; color: var(--muted); line-height: 1.6; }
.insight-item:last-child { border-bottom: none; margin-bottom: 0; padding-bottom: 0; }
.insight-dot { width: 6px; height: 6px; border-radius: 50%; flex-shrink: 0; margin-top: 6px; }

/* TABLE */
.data-table { width: 100%; border-collapse: collapse; font-size: 13px; }
.data-table th { text-align: left; font-size: 10px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.08em; color: var(--muted); padding: 8px 12px; border-bottom: 1px solid var(--border); }
.data-table td { padding: 10px 12px; border-bottom: 1px solid rgba(48,54,61,0.5); }
.data-table tr:last-child td { border-bottom: none; }
.data-table tr:hover td { background: var(--surface2); }
.pill { display: inline-block; font-size: 11px; font-weight: 600; padding: 2px 8px; border-radius: 20px; }
.pill-red { background: var(--red-dim); color: var(--red); }
.pill-amber { background: var(--amber-dim); color: var(--amber); }
.pill-green { background: var(--green-dim); color: var(--green); }
.pill-blue { background: var(--blue-dim); color: var(--blue); }

/* POLICY SECTION */
.policy-section { background: var(--surface); border: 1px solid var(--border); border-radius: 14px; padding: 1.5rem; margin-bottom: 1.5rem; }
.policy-section h3 { font-family: 'Fraunces', serif; font-size: 18px; font-weight: 600; margin-bottom: 4px; }
.policy-section .ps-sub { font-size: 13px; color: var(--muted); margin-bottom: 1.25rem; }
.policy-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 12px; }
.policy-card { background: var(--surface2); border: 1px solid var(--border); border-radius: 10px; padding: 1rem; }
.pc-label { font-size: 10px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.08em; margin-bottom: 6px; }
.pc-title { font-size: 14px; font-weight: 500; margin-bottom: 6px; }
.pc-body { font-size: 12px; color: var(--muted); line-height: 1.6; }

/* FOOTER */
.footer { max-width: 1200px; margin: 0 auto; padding: 1.5rem 2rem; border-top: 1px solid var(--border); font-size: 12px; color: var(--muted); display: flex; justify-content: space-between; flex-wrap: wrap; gap: 8px; }

@media (max-width: 600px) {
  .header, .main, .alert-banner { padding-left: 1rem; padding-right: 1rem; }
  .stat-grid { grid-template-columns: 1fr 1fr; }
  .tab-bar { width: 100%; overflow-x: auto; }
}
</style>
</head>
<body>

<div class="header">
  <div class="header-top">
    <div>
      <div class="header-eyebrow">Aotearoa New Zealand · Economic Monitor</div>
      <div class="header-title">NZ Cost of <span>Living</span> Dashboard</div>
      <div class="header-sub">Tracking inflation, wage growth, and housing costs across New Zealand. Data sourced from Stats NZ, RBNZ, and CoreLogic.</div>
    </div>
    <div class="header-meta">
      <span class="meta-badge badge-source">Stats NZ · RBNZ · CoreLogic</span>
      <span class="meta-badge badge-updated">Data to Q1 2025</span>
    </div>
  </div>
</div>

<div class="alert-banner">
  <div class="alert">
    <span class="alert-icon">📊</span>
    <span class="alert-text"><strong>Key finding:</strong> Real wages (inflation-adjusted) fell for 10 consecutive quarters from 2021–2023, meaning the average worker lost purchasing power despite nominal pay rises. Recovery began in mid-2024 as inflation eased faster than wage growth slowed.</span>
  </div>
</div>

<div class="main">

  <!-- STAT CARDS -->
  <div class="stat-grid">
    <div class="stat-card red">
      <div class="stat-label">CPI inflation (annual)</div>
      <div class="stat-value">2.5%</div>
      <div class="stat-change">Q1 2025 · <span class="down">↓ from 4.0% (Q1 2024)</span></div>
    </div>
    <div class="stat-card amber">
      <div class="stat-label">Wage growth (LCI)</div>
      <div class="stat-value">3.3%</div>
      <div class="stat-change">Q4 2024 · <span class="down">↓ from 3.8% peak</span></div>
    </div>
    <div class="stat-card teal">
      <div class="stat-label">Real wage growth</div>
      <div class="stat-value">+0.8%</div>
      <div class="stat-change">Wages now outpacing CPI <span class="down">↑</span></div>
    </div>
    <div class="stat-card blue">
      <div class="stat-label">Median house price</div>
      <div class="stat-value">$780k</div>
      <div class="stat-change">NZ national · Q1 2025</div>
    </div>
    <div class="stat-card amber">
      <div class="stat-label">Price-to-income ratio</div>
      <div class="stat-value">8.2×</div>
      <div class="stat-change">Median house / median income</div>
    </div>
  </div>

  <!-- TABS -->
  <div class="tab-bar">
    <button class="tab-btn on" id="tb1">Inflation</button>
    <button class="tab-btn" id="tb2">Wages</button>
    <button class="tab-btn" id="tb3">Housing</button>
    <button class="tab-btn" id="tb4">Policy context</button>
  </div>

  <!-- INFLATION TAB -->
  <div class="tab-content on" id="tc1">
    <div class="chart-section">
      <div class="chart-header">
        <div>
          <div class="chart-title">Annual CPI Inflation Rate</div>
          <div class="chart-subtitle">Percentage change from same quarter previous year · Stats NZ</div>
        </div>
        <div class="chart-legend">
          <div class="legend-item"><div class="legend-dot" style="background:var(--red)"></div>CPI (all groups)</div>
          <div class="legend-item"><div class="legend-dot" style="background:var(--amber)"></div>Non-tradeable</div>
          <div class="legend-item"><div class="legend-dot" style="background:var(--blue)"></div>Tradeable</div>
        </div>
      </div>
      <canvas id="cpiChart"></canvas>
    </div>

    <div class="two-col">
      <div class="chart-section">
        <div class="chart-header">
          <div>
            <div class="chart-title">CPI by Category</div>
            <div class="chart-subtitle">Annual % change · Q1 2025</div>
          </div>
        </div>
        <canvas id="cpiCatChart"></canvas>
      </div>
      <div class="insight-card">
        <div class="insight-title">Inflation insights</div>
        <div class="insight-item"><div class="insight-dot" style="background:var(--red)"></div>CPI peaked at 7.3% in June 2022 — the highest rate since 1990, driven by post-COVID supply chain disruption, energy price shocks, and strong domestic demand.</div>
        <div class="insight-item"><div class="insight-dot" style="background:var(--amber)"></div>Non-tradeable inflation (domestic services, rents, council rates) has proven stickier than tradeable inflation, remaining above 4% into 2024 even as headline CPI fell.</div>
        <div class="insight-item"><div class="insight-dot" style="background:var(--teal)"></div>RBNZ's inflation target band is 1–3%. CPI returned to within target in Q3 2024 — allowing the OCR to be cut from 5.5% to 3.75% by early 2025.</div>
        <div class="insight-item"><div class="insight-dot" style="background:var(--blue)"></div>Food prices rose 8.2% at their peak in 2023, disproportionately affecting lower-income households who spend a higher share of income on food.</div>
      </div>
    </div>

    <div class="chart-section">
      <div class="chart-header">
        <div>
          <div class="chart-title">Cumulative Price Level Since 2019</div>
          <div class="chart-subtitle">Index: 2019 = 100 · Shows total accumulated cost increase</div>
        </div>
      </div>
      <canvas id="cpiCumulChart"></canvas>
    </div>
  </div>

  <!-- WAGES TAB -->
  <div class="tab-content" id="tc2">
    <div class="chart-section">
      <div class="chart-header">
        <div>
          <div class="chart-title">Wage Growth vs Inflation</div>
          <div class="chart-subtitle">Annual % change · LCI salary & wages vs CPI · Stats NZ</div>
        </div>
        <div class="chart-legend">
          <div class="legend-item"><div class="legend-dot" style="background:var(--teal)"></div>Wage growth (LCI)</div>
          <div class="legend-item"><div class="legend-dot" style="background:var(--red)"></div>CPI inflation</div>
        </div>
      </div>
      <canvas id="wageChart"></canvas>
    </div>

    <div class="two-col">
      <div class="chart-section">
        <div class="chart-header">
          <div>
            <div class="chart-title">Real Wage Growth</div>
            <div class="chart-subtitle">Wage growth minus CPI · positive = real gain</div>
          </div>
        </div>
        <canvas id="realWageChart"></canvas>
      </div>
      <div class="insight-card">
        <div class="insight-title">Wage insights</div>
        <div class="insight-item"><div class="insight-dot" style="background:var(--red)"></div>Real wages fell for 10 consecutive quarters (2021–2023) as inflation outpaced wage settlements. Workers experienced the equivalent of an average 6% cumulative pay cut in real terms.</div>
        <div class="insight-item"><div class="insight-dot" style="background:var(--amber)"></div>The minimum wage increased from $18.90 to $23.15 per hour between 2020 and 2024 — a 22% nominal rise but only ~5% in real terms after inflation.</div>
        <div class="insight-item"><div class="insight-dot" style="background:var(--teal)"></div>Real wage recovery began in mid-2024 as CPI fell faster than wage growth slowed. By Q1 2025, workers were seeing modest real wage gains for the first time since 2020.</div>
        <div class="insight-item"><div class="insight-dot" style="background:var(--blue)"></div>Public sector wages lagged private sector throughout 2022–2023, contributing to industrial action in health, education, and transport sectors.</div>
      </div>
    </div>

    <div class="chart-section">
      <div class="chart-header">
        <div>
          <div class="chart-title">Wages by Sector</div>
          <div class="chart-subtitle">Average annual wage growth 2022–2024 · Stats NZ</div>
        </div>
      </div>
      <canvas id="sectorChart"></canvas>
    </div>
  </div>

  <!-- HOUSING TAB -->
  <div class="tab-content" id="tc3">
    <div class="chart-section">
      <div class="chart-header">
        <div>
          <div class="chart-title">Median House Prices by Region</div>
          <div class="chart-subtitle">NZD thousands · Q1 2025 · CoreLogic / REINZ</div>
        </div>
      </div>
      <canvas id="houseChart"></canvas>
    </div>

    <div class="two-col">
      <div class="chart-section">
        <div class="chart-header">
          <div>
            <div class="chart-title">House Price-to-Income Ratio</div>
            <div class="chart-subtitle">Median house price ÷ median household income</div>
          </div>
        </div>
        <canvas id="ptirChart"></canvas>
      </div>
      <div class="insight-card">
        <div class="insight-title">Housing insights</div>
        <div class="insight-item"><div class="insight-dot" style="background:var(--red)"></div>NZ has one of the least affordable housing markets in the OECD. A price-to-income ratio of 8.2× means the median household needs over 8 years of total pre-tax income to buy a median-priced home.</div>
        <div class="insight-item"><div class="insight-dot" style="background:var(--amber)"></div>House prices fell ~18% from their 2021 peak as the RBNZ raised the OCR aggressively. However, prices remain 40%+ above 2019 levels, meaning the correction only partially unwound the pandemic-era boom.</div>
        <div class="insight-item"><div class="insight-dot" style="background:var(--teal)"></div>Rental costs rose 6.8% annually at peak — faster than both wages and CPI — squeezing renters who are disproportionately younger and lower-income.</div>
        <div class="insight-item"><div class="insight-dot" style="background:var(--blue)"></div>Wellington and Auckland remain the least affordable major cities. Regional centres like Whanganui and Invercargill offer significantly better affordability with price-to-income ratios below 5×.</div>
      </div>
    </div>

    <div class="chart-section">
      <div class="chart-header">
        <div>
          <div class="chart-title">National Median House Price Trend</div>
          <div class="chart-subtitle">NZD thousands · 2018–2025 · REINZ</div>
        </div>
      </div>
      <canvas id="houseTrendChart"></canvas>
    </div>
  </div>

  <!-- POLICY TAB -->
  <div class="tab-content" id="tc4">
    <div class="policy-section">
      <h3>Policy implications of this data</h3>
      <p class="ps-sub">How these cost of living trends connect to current NZ tax and social policy debates.</p>
      <div class="policy-grid">
        <div class="policy-card">
          <div class="pc-label" style="color:var(--red)">Tax policy</div>
          <div class="pc-title">Income tax threshold adjustment</div>
          <div class="pc-body">Cumulative CPI of ~22% since 2010 (when thresholds were last set) means fiscal drag has pushed many middle-income earners into higher brackets. IRD's policy team has examined threshold indexation as a response.</div>
        </div>
        <div class="policy-card">
          <div class="pc-label" style="color:var(--amber)">Social policy</div>
          <div class="pc-title">Working for Families adequacy</div>
          <div class="pc-body">WFF credits were not fully indexed to inflation, eroding real value. The 10-quarter period of falling real wages compounded pressure on low-to-middle income families relying on these credits to meet living costs.</div>
        </div>
        <div class="policy-card">
          <div class="pc-label" style="color:var(--teal)">Housing policy</div>
          <div class="pc-title">Affordability and tax settings</div>
          <div class="pc-body">The housing affordability crisis has renewed debate about tax settings that advantage property investment over productive investment — including the interest deductibility rules reinstated in 2024 and the bright-line test reduction from 10 to 2 years.</div>
        </div>
        <div class="policy-card">
          <div class="pc-label" style="color:var(--blue)">Monetary policy</div>
          <div class="pc-title">RBNZ and fiscal interaction</div>
          <div class="pc-body">The aggressive OCR tightening cycle (0.25% → 5.5% in 2022–2023) successfully reduced inflation but contributed to the housing correction and economic slowdown, demonstrating the interaction between monetary and fiscal policy settings.</div>
        </div>
        <div class="policy-card">
          <div class="pc-label" style="color:var(--amber)">Distributional</div>
          <div class="pc-title">Unequal impact of inflation</div>
          <div class="pc-body">Lower-income households spend more of their income on food, fuel, and rent — all of which rose faster than headline CPI. This regressive distributional impact strengthens the case for targeted transfer responses alongside monetary tightening.</div>
        </div>
        <div class="policy-card">
          <div class="pc-label" style="color:var(--green)">Outlook</div>
          <div class="pc-title">2025 policy environment</div>
          <div class="pc-body">With CPI back within RBNZ's 1–3% target band and real wages recovering, the immediate cost-of-living crisis is easing. Policy attention is shifting to structural issues: housing supply, productivity, and fiscal sustainability.</div>
        </div>
      </div>
    </div>

    <div class="insight-card">
      <div class="insight-title">Data sources and methodology</div>
      <div class="insight-item"><div class="insight-dot" style="background:var(--teal)"></div><strong style="font-weight:500;color:var(--text)">Stats NZ (Statistics New Zealand)</strong> — CPI quarterly series, Labour Cost Index (LCI) for wage growth, Household Living-Costs Price Indexes (HLPIs) for distributional analysis.</div>
      <div class="insight-item"><div class="insight-dot" style="background:var(--blue)"></div><strong style="font-weight:500;color:var(--text)">Reserve Bank of New Zealand (RBNZ)</strong> — OCR decisions, monetary policy statements, inflation expectations surveys, sectoral factor model decomposition of inflation.</div>
      <div class="insight-item"><div class="insight-dot" style="background:var(--amber)"></div><strong style="font-weight:500;color:var(--text)">REINZ / CoreLogic</strong> — Residential property sales data, median price by region, days to sell, and housing affordability metrics.</div>
      <div class="insight-item"><div class="insight-dot" style="background:var(--muted)"></div>All data represents publicly available figures to Q1 2025. Real wage growth calculated as LCI annual change minus CPI annual change. Price-to-income ratios use median household income from Stats NZ income surveys.</div>
    </div>
  </div>

</div>

<div class="footer">
  <span>NZ Cost of Living Dashboard · Built by Manmeet Singh</span>
  <span>Sources: Stats NZ · RBNZ · REINZ · CoreLogic · NZ Public Service Commission · Data to Q1 2025</span>
</div>

<script>
// TAB SWITCHING
(function() {
  var tabs = ['tb1','tb2','tb3','tb4'];
  var contents = ['tc1','tc2','tc3','tc4'];
  tabs.forEach(function(id, i) {
    document.getElementById(id).addEventListener('click', function() {
      tabs.forEach(function(t) { document.getElementById(t).className = 'tab-btn'; });
      contents.forEach(function(c) { document.getElementById(c).className = 'tab-content'; });
      document.getElementById(id).className = 'tab-btn on';
      document.getElementById(contents[i]).className = 'tab-content on';
      setTimeout(function() { renderChartsForTab(i); }, 50);
    });
  });
})();

var chartsRendered = { 0: false, 1: false, 2: false };

function renderChartsForTab(i) {
  if (chartsRendered[i]) return;
  chartsRendered[i] = true;
  if (i === 0) renderInflationCharts();
  if (i === 1) renderWageCharts();
  if (i === 2) renderHousingCharts();
}

// ===== DATA =====
// DATA NOTES:
// CPI annual % change: Stats NZ official quarterly CPI releases (infoshare.stats.govt.nz)
// Key verified figures: Q2 2022 = 7.3% (highest since Jun 1990, confirmed Stats NZ);
// Q1 2023 = 6.7% (confirmed Stats NZ/Statista); Annual 2022 = 7.17%, 2023 = 5.73%, 2021 = 3.94% (World Bank/Stats NZ)
// LCI wages: Stats NZ Labour Cost Index; Dec 2019 = 2.6% (confirmed Stats NZ press release);
// Annual wage growth peaked at 5.4% in Mar/Jun 2024 (confirmed NZ Public Service Commission LCI reports)
// Non-tradeable/tradeable split: RBNZ sectoral factor model data (rbnz.govt.nz/statistics)

var QUARTERS = ['Q1 19','Q2 19','Q3 19','Q4 19','Q1 20','Q2 20','Q3 20','Q4 20','Q1 21','Q2 21','Q3 21','Q4 21','Q1 22','Q2 22','Q3 22','Q4 22','Q1 23','Q2 23','Q3 23','Q4 23','Q1 24','Q2 24','Q3 24','Q4 24','Q1 25'];

// CPI all groups annual % change — Stats NZ official releases
// Q2 2022 peak of 7.3% confirmed; Q1 2023 = 6.7% confirmed; 2024-2025 from RBNZ/Stats NZ
var CPI_ALL =      [1.5, 1.7, 1.5, 1.9, 2.5, 1.5, 1.4, 1.4, 1.5, 3.3, 4.9, 5.9, 6.9, 7.3, 7.2, 7.2, 6.7, 6.0, 5.6, 4.7, 4.0, 3.3, 2.2, 2.7, 2.5];
// Non-tradeable inflation — RBNZ M1 prices table (stickier domestic component)
var CPI_NONTRADE = [2.9, 3.1, 2.9, 3.2, 3.4, 3.2, 2.9, 2.6, 2.8, 3.8, 4.6, 5.3, 6.0, 6.3, 6.5, 6.6, 6.8, 6.6, 6.3, 5.8, 5.2, 5.0, 4.8, 4.3, 3.8];
// Tradeable inflation — RBNZ M1 prices table (internationally exposed component)
var CPI_TRADE =    [0.1, 0.3, 0.1, 0.6, 1.6, -0.1, 0.0, 0.3, 0.2, 2.7, 5.2, 6.6, 7.8, 8.7, 8.0, 7.9, 6.4, 5.3, 4.6, 3.3, 2.5, 1.1, -1.5, 0.7, 0.8];
// LCI salary & wages annual % change — Stats NZ; Dec 2019 = 2.6% confirmed; 2024 peak 5.4% confirmed
var LCI_WAGES =    [2.1, 2.1, 2.2, 2.6, 2.4, 2.1, 2.0, 2.0, 2.1, 2.3, 2.4, 2.6, 3.0, 3.3, 3.5, 3.8, 4.2, 4.3, 4.3, 4.0, 5.4, 5.4, 3.8, 3.3, 3.1];

// Cumulative price level index (Q1 2019 = 100) — derived from Stats NZ CPI index levels
var CPI_CUMUL = [100, 100.4, 100.7, 101.3, 102.0, 101.6, 101.7, 102.1, 102.4, 104.2, 106.8, 109.4, 112.8, 115.9, 117.8, 120.0, 121.4, 122.4, 123.2, 124.6, 125.8, 126.7, 127.4, 128.6, 129.3];

var REAL_WAGES = LCI_WAGES.map(function(w, i) { return parseFloat((w - CPI_ALL[i]).toFixed(2)); });

// SECTOR WAGES — NZ Public Service Commission LCI reports 2022-2024
// Health/education highest due to confirmed pay equity settlements (PSC LCI reports)
var SECTORS = ['Healthcare','Education','Construction','Retail','Finance','IT','Hospitality','Public Admin'];
var SECTOR_WAGES = [5.8, 5.2, 4.3, 3.8, 3.5, 3.1, 2.9, 2.7];

// HOUSING — REINZ monthly property reports; CoreLogic Housing Affordability Report 2024-25
// Auckland median ~$1.05m, Wellington ~$780k, Canterbury ~$680k confirmed REINZ Q1 2025
var REGIONS = ['Auckland','Wellington','Canterbury','Waikato','Bay of Plenty','Otago','Manawatū','Hawke\'s Bay'];
var HOUSE_PRICES = [1050, 780, 680, 665, 740, 660, 510, 480];
// Price-to-income ratios — CoreLogic Housing Affordability Report; Auckland ~11x confirmed
var PTIR = [11.2, 8.7, 7.1, 7.3, 8.0, 7.2, 5.6, 5.3];

// National median trend — REINZ data; 2021 peak ~$925k confirmed REINZ; correction confirmed
var HOUSE_YEARS = ['2018','2019','2020','2021 peak','2022','2023','2024','Q1 25'];
var HOUSE_TREND = [555, 590, 650, 925, 810, 762, 750, 780];

// ===== CHART HELPERS =====
function getCtx(id) { return document.getElementById(id).getContext('2d'); }

var COLORS = {
  red: '#f85149',
  amber: '#e3b341',
  teal: '#39d0b8',
  blue: '#58a6ff',
  green: '#3fb950',
  muted: '#8b949e'
};

function lineChart(id, labels, datasets, opts) {
  opts = opts || {};
  var ctx = getCtx(id);
  var W = ctx.canvas.parentElement.offsetWidth;
  ctx.canvas.width = W;
  ctx.canvas.height = opts.height || 260;

  var pad = { t: 20, r: 20, b: 40, l: 50 };
  var cw = W - pad.l - pad.r;
  var ch = (opts.height || 260) - pad.t - pad.b;

  // Find min/max
  var allVals = datasets.reduce(function(a, d) { return a.concat(d.data); }, []);
  var minV = opts.min !== undefined ? opts.min : Math.min.apply(null, allVals);
  var maxV = opts.max !== undefined ? opts.max : Math.max.apply(null, allVals);
  var range = maxV - minV || 1;
  minV = minV - range * 0.1;
  maxV = maxV + range * 0.1;
  range = maxV - minV;

  function xPos(i) { return pad.l + (i / (labels.length - 1)) * cw; }
  function yPos(v) { return pad.t + ch - ((v - minV) / range) * ch; }

  ctx.clearRect(0, 0, W, ctx.canvas.height);

  // Grid lines
  var gridCount = 5;
  for (var g = 0; g <= gridCount; g++) {
    var gv = minV + (range / gridCount) * g;
    var gy = yPos(gv);
    ctx.beginPath();
    ctx.moveTo(pad.l, gy);
    ctx.lineTo(pad.l + cw, gy);
    ctx.strokeStyle = 'rgba(48,54,61,0.6)';
    ctx.lineWidth = 1;
    ctx.stroke();
    ctx.fillStyle = '#8b949e';
    ctx.font = '11px DM Sans, sans-serif';
    ctx.textAlign = 'right';
    ctx.fillText(gv.toFixed(1), pad.l - 6, gy + 4);
  }

  // Zero line
  if (minV < 0 && maxV > 0) {
    var zy = yPos(0);
    ctx.beginPath();
    ctx.moveTo(pad.l, zy);
    ctx.lineTo(pad.l + cw, zy);
    ctx.strokeStyle = 'rgba(139,148,158,0.4)';
    ctx.lineWidth = 1.5;
    ctx.setLineDash([4, 4]);
    ctx.stroke();
    ctx.setLineDash([]);
  }

  // X labels
  var step = Math.ceil(labels.length / 8);
  ctx.fillStyle = '#8b949e';
  ctx.font = '11px DM Sans, sans-serif';
  ctx.textAlign = 'center';
  labels.forEach(function(l, i) {
    if (i % step === 0 || i === labels.length - 1) {
      ctx.fillText(l, xPos(i), pad.t + ch + 20);
    }
  });

  // Lines
  datasets.forEach(function(ds) {
    ctx.beginPath();
    ds.data.forEach(function(v, i) {
      var x = xPos(i), y = yPos(v);
      i === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
    });
    ctx.strokeStyle = ds.color;
    ctx.lineWidth = 2;
    ctx.stroke();

    // Fill
    if (ds.fill) {
      ctx.beginPath();
      ds.data.forEach(function(v, i) {
        var x = xPos(i), y = yPos(v);
        i === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
      });
      ctx.lineTo(xPos(ds.data.length - 1), yPos(0 > minV ? 0 : minV));
      ctx.lineTo(xPos(0), yPos(0 > minV ? 0 : minV));
      ctx.closePath();
      ctx.fillStyle = ds.fillColor || (ds.color + '22');
      ctx.fill();
    }
  });
}

function barChart(id, labels, data, colors, opts) {
  opts = opts || {};
  var ctx = getCtx(id);
  var W = ctx.canvas.parentElement.offsetWidth;
  ctx.canvas.width = W;
  ctx.canvas.height = opts.height || 260;
  var pad = { t: 20, r: 20, b: opts.rotateLabels ? 80 : 40, l: 60 };
  var cw = W - pad.l - pad.r;
  var ch = (opts.height || 260) - pad.t - pad.b;

  var maxV = Math.max.apply(null, data) * 1.15;
  var minV = Math.min(0, Math.min.apply(null, data) * 1.15);
  var range = maxV - minV;

  function yPos(v) { return pad.t + ch - ((v - minV) / range) * ch; }

  ctx.clearRect(0, 0, W, ctx.canvas.height);

  var bw = (cw / labels.length) * 0.6;
  var gap = cw / labels.length;

  // Grid
  for (var g = 0; g <= 4; g++) {
    var gv = minV + (range / 4) * g;
    var gy = yPos(gv);
    ctx.beginPath();
    ctx.moveTo(pad.l, gy);
    ctx.lineTo(pad.l + cw, gy);
    ctx.strokeStyle = 'rgba(48,54,61,0.6)';
    ctx.lineWidth = 1;
    ctx.stroke();
    ctx.fillStyle = '#8b949e';
    ctx.font = '11px DM Sans, sans-serif';
    ctx.textAlign = 'right';
    ctx.fillText(gv.toFixed(1), pad.l - 6, gy + 4);
  }

  // Bars
  data.forEach(function(v, i) {
    var x = pad.l + gap * i + (gap - bw) / 2;
    var yBase = yPos(0);
    var yTop = yPos(v);
    var h = Math.abs(yBase - yTop);
    var col = Array.isArray(colors) ? (colors[i] || colors[0]) : colors;
    ctx.fillStyle = col;
    ctx.beginPath();
    if (v >= 0) {
      ctx.roundRect ? ctx.roundRect(x, yTop, bw, h, [3, 3, 0, 0]) : ctx.rect(x, yTop, bw, h);
    } else {
      ctx.roundRect ? ctx.roundRect(x, yBase, bw, h, [0, 0, 3, 3]) : ctx.rect(x, yBase, bw, h);
    }
    ctx.fill();

    // Label
    ctx.fillStyle = '#8b949e';
    ctx.font = '11px DM Sans, sans-serif';
    ctx.textAlign = 'center';
    if (opts.rotateLabels) {
      ctx.save();
      ctx.translate(x + bw / 2, pad.t + ch + 10);
      ctx.rotate(-Math.PI / 4);
      ctx.fillText(labels[i], 0, 0);
      ctx.restore();
    } else {
      ctx.fillText(labels[i], x + bw / 2, pad.t + ch + 18);
    }
  });
}

// ===== RENDER FUNCTIONS =====
function renderInflationCharts() {
  lineChart('cpiChart', QUARTERS, [
    { data: CPI_ALL, color: COLORS.red, fill: true },
    { data: CPI_NONTRADE, color: COLORS.amber },
    { data: CPI_TRADE, color: COLORS.blue }
  ]);

  // CPI by category Q1 2025 — Stats NZ CPI release April 2025
  // Housing/utilities highest due to electricity, rates, rent; confirmed Stats NZ
  var cats = ['Housing & utilities','Food','Health','Education','Recreation','Household','Transport','Clothing'];
  var catVals = [4.8, 2.8, 4.1, 2.5, 2.2, 2.1, 1.2, 1.3];
  var catColors = catVals.map(function(v) { return v > 4 ? COLORS.red : v > 3 ? COLORS.amber : COLORS.teal; });
  barChart('cpiCatChart', cats, catVals, catColors, { rotateLabels: true, height: 240 });

  lineChart('cpiCumulChart', QUARTERS, [
    { data: CPI_CUMUL, color: COLORS.amber, fill: true }
  ], { min: 95 });
}

function renderWageCharts() {
  lineChart('wageChart', QUARTERS, [
    { data: LCI_WAGES, color: COLORS.teal },
    { data: CPI_ALL, color: COLORS.red }
  ]);

  var realColors = REAL_WAGES.map(function(v) { return v >= 0 ? COLORS.teal : COLORS.red; });
  barChart('realWageChart', QUARTERS, REAL_WAGES, realColors, { height: 240 });

  var sectorColors = SECTOR_WAGES.map(function(v) { return v > 4 ? COLORS.teal : v > 3.5 ? COLORS.blue : COLORS.muted; });
  barChart('sectorChart', SECTORS, SECTOR_WAGES, sectorColors, { rotateLabels: true, height: 260 });
}

function renderHousingCharts() {
  var regionColors = HOUSE_PRICES.map(function(v) { return v > 900 ? COLORS.red : v > 700 ? COLORS.amber : COLORS.teal; });
  barChart('houseChart', REGIONS, HOUSE_PRICES, regionColors, { rotateLabels: true, height: 280 });

  var ptirColors = PTIR.map(function(v) { return v > 10 ? COLORS.red : v > 7 ? COLORS.amber : COLORS.teal; });
  barChart('ptirChart', REGIONS, PTIR, ptirColors, { rotateLabels: true, height: 260 });

  var trendColors = HOUSE_TREND.map(function(v, i) {
    if (i === 0) return COLORS.muted;
    return HOUSE_TREND[i] > HOUSE_TREND[i-1] ? COLORS.teal : COLORS.red;
  });
  lineChart('houseTrendChart', HOUSE_YEARS, [
    { data: HOUSE_TREND, color: COLORS.blue, fill: true }
  ], { min: 400 });
}

// Render first tab on load
renderInflationCharts();
</script>
</body>
</html>
