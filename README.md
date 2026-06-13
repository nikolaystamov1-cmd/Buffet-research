<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
  <title>🍽️ Buffet-style stock research | No API key</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    body {
      background: #f5f2eb;
      font-family: 'Segoe UI', Georgia, 'Times New Roman', serif;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      margin: 20px;
    }
    .buffet-card {
      max-width: 750px;
      width: 100%;
      background: #fffbf2;
      border: 2px solid #c7a86b;
      border-radius: 40px 40px 30px 30px;
      box-shadow: 0 20px 30px rgba(0,0,0,0.1);
      overflow: hidden;
    }
    .header {
      background: #4a3622;
      background-image: linear-gradient(135deg, #5e4529 0%, #3e2c1c 100%);
      color: #fbe9c3;
      padding: 25px 25px 15px;
      text-align: center;
      border-bottom: 4px solid #b49450;
    }
    .header h1 {
      font-size: 2.4rem;
      letter-spacing: 2px;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 12px;
      font-weight: 500;
    }
    .sub {
      font-style: italic;
      color: #ddc7a2;
      margin-top: 6px;
      font-size: 1rem;
    }
    .content {
      padding: 25px 25px 30px;
    }
    .ticker-section {
      display: flex;
      gap: 12px;
      align-items: center;
      flex-wrap: wrap;
      margin-bottom: 25px;
      background: #f3ede3;
      padding: 18px;
      border-radius: 50px;
    }
    .ticker-section label {
      font-weight: bold;
      color: #4a3622;
      font-size: 1.1rem;
    }
    .ticker-section input {
      flex: 2;
      padding: 12px 18px;
      border: 2px solid #c7a86b;
      border-radius: 40px;
      font-size: 1.2rem;
      background: white;
      outline: none;
      text-transform: uppercase;
      font-weight: 500;
      letter-spacing: 0.5px;
    }
    .ticker-section button {
      background: #b49450;
      border: none;
      color: white;
      font-weight: bold;
      padding: 12px 25px;
      border-radius: 40px;
      font-size: 1rem;
      cursor: pointer;
      transition: 0.2s;
      background: #8b6f3c;
      letter-spacing: 0.5px;
      display: flex;
      align-items: center;
      gap: 6px;
    }
    .ticker-section button:hover {
      background: #6b532b;
    }
    .buffet-dishes {
      background: #fcf9f2;
      border: 2px dashed #c7a86b;
      border-radius: 28px;
      padding: 18px 20px;
      margin-bottom: 20px;
    }
    .dishes-title {
      font-weight: 700;
      color: #4a3622;
      font-size: 1.2rem;
      margin-bottom: 14px;
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .checkbox-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
      gap: 12px;
    }
    .checkbox-item {
      display: flex;
      align-items: center;
      gap: 8px;
      background: white;
      padding: 8px 12px;
      border-radius: 30px;
      border: 1px solid #dac292;
      cursor: pointer;
      transition: 0.1s;
      font-size: 0.9rem;
    }
    .checkbox-item:hover {
      background: #f7f0e0;
    }
    .checkbox-item input[type="checkbox"] {
      accent-color: #8b6f3c;
      width: 17px;
      height: 17px;
      margin-right: 4px;
    }
    .loading, .error {
      text-align: center;
      padding: 18px;
      border-radius: 30px;
      margin: 15px 0;
    }
    .loading {
      background: #f3ede3;
      color: #5e4529;
    }
    .error {
      background: #ffe6e6;
      color: #a13030;
      border: 1px solid #d99494;
    }
    .results {
      margin-top: 20px;
      background: #ffffff;
      border-radius: 30px;
      padding: 20px;
      border: 1px solid #dac292;
    }
    .company-title {
      font-size: 1.5rem;
      font-weight: 600;
      color: #2d2416;
      margin-bottom: 15px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
    .metric-row {
      display: flex;
      justify-content: space-between;
      padding: 12px 10px;
      border-bottom: 1px solid #e7ddcc;
    }
    .metric-row:last-child {
      border-bottom: none;
    }
    .metric-name {
      font-weight: 500;
      color: #4f3b28;
    }
    .metric-value {
      font-weight: 600;
      color: #1f1a11;
    }
    .buffett-verdict {
      background: #f3ede3;
      border-radius: 25px;
      padding: 15px 20px;
      margin-top: 20px;
      font-style: italic;
      border-left: 6px solid #b49450;
    }
    .footer-note {
      font-size: 0.8rem;
      color: #8b775a;
      text-align: center;
      margin-top: 18px;
    }
  </style>
</head>
<body>
<div class="buffet-card">
  <div class="header">
    <h1>
      <span>🍽️</span> Buffett·et <span>📊</span>
    </h1>
    <div class="sub">pick your metrics • taste the fundamentals</div>
  </div>
  <div class="content">
    <div class="ticker-section">
      <label>📈 Ticker</label>
      <input type="text" id="tickerInput" placeholder="e.g. AAPL, MSFT, KO" value="AAPL" autofocus>
      <button id="fetchBtn"><span>🔍</span> Load buffet</button>
    </div>

    <div class="buffet-dishes">
      <div class="dishes-title"><span>🍲</span> Choose your dishes (metrics)</div>
      <div class="checkbox-grid" id="checkboxContainer"></div>
      <div style="margin-top:12px; display:flex; gap:8px; flex-wrap:wrap;">
        <button id="selectAllBtn" style="background:none; border:1px solid #b49450; color:#4a3622; padding:6px 16px; border-radius:20px; cursor:pointer; font-size:0.8rem;">Select all</button>
        <button id="clearAllBtn" style="background:none; border:1px solid #b49450; color:#4a3622; padding:6px 16px; border-radius:20px; cursor:pointer; font-size:0.8rem;">Clear</button>
      </div>
    </div>

    <div id="statusArea"></div>
    <div id="resultsArea"></div>
    <div class="footer-note">
      ⚠️ Data from Yahoo Finance (no API key needed). For educational buffet experience.
    </div>
  </div>
</div>

<script>
  (function() {
    const METRICS_CATALOG = [
      { id: "pe", label: "🥩 P/E Ratio", key: "trailingPE" },
      { id: "pb", label: "📖 P/B Ratio", key: "priceToBook" },
      { id: "roe", label: "🏦 ROE (%)", key: "returnOnEquity" },
      { id: "roa", label: "📊 ROA (%)", key: "returnOnAssets" },
      { id: "debtEquity", label: "⚖️ Debt/Equity", key: "debtToEquity" },
      { id: "currentRatio", label: "💧 Current Ratio", key: "currentRatio" },
      { id: "grossMargin", label: "🧀 Gross Margin (%)", key: "grossMargins" },
      { id: "netMargin", label: "💰 Net Margin (%)", key: "profitMargins" },
      { id: "revenueGrowth", label: "📈 Revenue Growth (YoY %)", key: "revenueGrowth" },
      { id: "eps", label: "🍬 EPS", key: "trailingEps" },
      { id: "dividendYield", label: "🍫 Dividend Yield (%)", key: "dividendYield" },
      { id: "payoutRatio", label: "🔄 Payout Ratio (%)", key: "payoutRatio" }
    ];

    const tickerInput = document.getElementById('tickerInput');
    const fetchBtn = document.getElementById('fetchBtn');
    const checkboxContainer = document.getElementById('checkboxContainer');
    const selectAllBtn = document.getElementById('selectAllBtn');
    const clearAllBtn = document.getElementById('clearAllBtn');
    const statusArea = document.getElementById('statusArea');
    const resultsArea = document.getElementById('resultsArea');

    let currentFinancialData = null;

    function renderCheckboxes() {
      checkboxContainer.innerHTML = '';
      METRICS_CATALOG.forEach(metric => {
        const label = document.createElement('label');
        label.className = 'checkbox-item';
        const checkbox = document.createElement('input');
        checkbox.type = 'checkbox';
        checkbox.value = metric.id;
        checkbox.dataset.key = metric.key;
        if (['pe','pb','roe','debtEquity','netMargin','revenueGrowth'].includes(metric.id)) {
          checkbox.checked = true;
        }
        label.appendChild(checkbox);
        label.appendChild(document.createTextNode(metric.label));
        checkboxContainer.appendChild(label);
      });
    }

    function getSelectedMetrics() {
      const checkboxes = document.querySelectorAll('#checkboxContainer input[type="checkbox"]:checked');
      return Array.from(checkboxes).map(cb => ({
        id: cb.value,
        key: cb.dataset.key,
        label: METRICS_CATALOG.find(m => m.id === cb.value)?.label || cb.value
      }));
    }

    selectAllBtn.addEventListener('click', () => {
      document.querySelectorAll('#checkboxContainer input[type="checkbox"]').forEach(cb => cb.checked = true);
    });
    clearAllBtn.addEventListener('click', () => {
      document.querySelectorAll('#checkboxContainer input[type="checkbox"]').forEach(cb => cb.checked = false);
    });

    // Yahoo Finance API (no key needed)
    async function fetchYahooData(symbol) {
      const url = `https://query1.finance.yahoo.com/v10/finance/quoteSummary/${symbol}?modules=price,defaultKeyStatistics,financialData,summaryDetail`;
      
      const res = await fetch(url);
      if (!res.ok) throw new Error(`Yahoo Finance fetch failed (${res.status})`);
      
      const data = await res.json();
      const result = data?.quoteSummary?.result?.[0];
      if (!result) throw new Error('Company not found or no data available.');
      
      const price = result.price || {};
      const stats = result.defaultKeyStatistics || {};
      const financial = result.financialData || {};
      const summary = result.summaryDetail || {};
      
      // Extract and merge all relevant fields
      const combined = {
        companyName: price.longName || price.shortName || symbol,
        symbol: price.symbol || symbol,
        price: price.regularMarketPrice?.raw || null,
        
        // Valuation
        trailingPE: summary.trailingPE?.raw || stats.trailingPE?.raw || null,
        priceToBook: stats.priceToBook?.raw || null,
        
        // Profitability
        returnOnEquity: financial.returnOnEquity?.raw ? financial.returnOnEquity.raw * 100 : null,
        returnOnAssets: financial.returnOnAssets?.raw ? financial.returnOnAssets.raw * 100 : null,
        
        // Leverage & Liquidity
        debtToEquity: financial.debtToEquity?.raw || null,
        currentRatio: financial.currentRatio?.raw || null,
        
        // Margins
        grossMargins: financial.grossMargins?.raw ? financial.grossMargins.raw * 100 : null,
        profitMargins: financial.profitMargins?.raw ? financial.profitMargins.raw * 100 : null,
        
        // Growth
        revenueGrowth: financial.revenueGrowth?.raw ? financial.revenueGrowth.raw * 100 : null,
        
        // Per share
        trailingEps: stats.trailingEps?.raw || null,
        
        // Dividends
        dividendYield: summary.dividendYield?.raw ? summary.dividendYield.raw * 100 : null,
        payoutRatio: summary.payoutRatio?.raw ? summary.payoutRatio.raw * 100 : null,
      };
      
      return combined;
    }

    function formatValue(value, decimals = 2) {
      if (value === null || value === undefined || isNaN(value)) return '—';
      if (Math.abs(value) > 1e9) return (value / 1e9).toFixed(2) + 'B';
      if (Math.abs(value) > 1e6) return (value / 1e6).toFixed(2) + 'M';
      return Number(value).toFixed(decimals);
    }

    function renderResults(data, selectedMetrics) {
      if (!data) return;

      const priceStr = data.price ? `$${Number(data.price).toFixed(2)}` : '—';
      let html = `
        <div class="results">
          <div class="company-title">
            <span>🏷️ ${data.companyName || data.symbol} (${data.symbol})</span>
            <span style="font-size:1.2rem;">${priceStr}</span>
          </div>
      `;

      selectedMetrics.forEach(metric => {
        let rawValue = data[metric.key];
        let displayValue = '—';

        if (rawValue !== null && rawValue !== undefined) {
          // These are already in percent
          if (['returnOnEquity','returnOnAssets','grossMargins','profitMargins','revenueGrowth','dividendYield','payoutRatio'].includes(metric.key)) {
            displayValue = formatValue(rawValue, 2) + '%';
          } else {
            displayValue = formatValue(rawValue, 2);
          }
        }

        html += `
          <div class="metric-row">
            <span class="metric-name">${metric.label}</span>
            <span class="metric-value">${displayValue}</span>
          </div>
        `;
      });

      const pe = data.trailingPE;
      const roe = data.returnOnEquity;
      const debtEq = data.debtToEquity;
      let verdict = '';
      if (pe !== null && roe !== null && debtEq !== null) {
        if (pe < 20 && roe > 15 && debtEq < 80) {
          verdict = '🍏 Looks like a wonderful company at a fair price? Classic Buffett characteristics.';
        } else if (pe < 25 && roe > 10 && debtEq < 120) {
          verdict = '🥩 Decent fundamentals. Might be worth a deeper dive.';
        } else if (pe > 30 || debtEq > 200) {
          verdict = '⚠️ Caution: high valuation or leverage. Not typical Buffett territory.';
        } else {
          verdict = '📋 Mixed signals – taste depends on your circle of competence.';
        }
      } else {
        verdict = '📭 Not enough data for a quick buffet verdict.';
      }

      html += `
        <div class="buffett-verdict">
          <strong>📜 Buffett's quick glance:</strong> ${verdict}
        </div>
      </div>
      `;

      resultsArea.innerHTML = html;
    }

    async function onFetch() {
      const symbol = tickerInput.value.trim().toUpperCase();
      if (!symbol) {
        statusArea.innerHTML = '<div class="error">Please enter a ticker symbol.</div>';
        return;
      }

      statusArea.innerHTML = '<div class="loading">⏳ Loading fresh ingredients from the market kitchen...</div>';
      resultsArea.innerHTML = '';

      try {
        const data = await fetchYahooData(symbol);
        currentFinancialData = data;
        statusArea.innerHTML = '';
        const selected = getSelectedMetrics();
        if (selected.length === 0) {
          resultsArea.innerHTML = '<div style="padding:20px; text-align:center; color:#4a3622;">🍽️ Please select at least one metric from the buffet.</div>';
          return;
        }
        renderResults(data, selected);
      } catch (error) {
        console.error(error);
        statusArea.innerHTML = `<div class="error">❌ ${error.message}</div>`;
        resultsArea.innerHTML = '';
        currentFinancialData = null;
      }
    }

    checkboxContainer.addEventListener('change', (e) => {
      if (e.target.type === 'checkbox' && currentFinancialData) {
        const selected = getSelectedMetrics();
        if (selected.length > 0) {
          renderResults(currentFinancialData, selected);
        } else {
          resultsArea.innerHTML = '<div style="padding:20px; text-align:center; color:#4a3622;">🍽️ Select at least one metric.</div>';
        }
      }
    });

    fetchBtn.addEventListener('click', onFetch);
    tickerInput.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') onFetch();
    });

    renderCheckboxes();

    // Auto-load AAPL on start
    window.addEventListener('load', () => {
      setTimeout(() => onFetch(), 500);
    });
  })();
</script>
</body>
</html>
