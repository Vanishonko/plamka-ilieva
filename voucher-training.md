---
title: Ваучерно обучение
description: Ваучерно обучение
---

<p>ЦПО към "ИТИ - Пламка Георгиева" - Монтана ще обучава заети и безработни лица с ваучери по лицензираните професии по Програма „Развитие на човешките ресурси“ 2021 – 2027 г., Националния план за възстановяване и устойчивост и Фонда за справедлив преход.
График за обучение с ваучери</p>
<h2>Таблица с обучения</h2>

<div id="csv-table" class="csv-table-wrapper">
  <p id="csv-loading">Зареждане на таблицата…</p>
</div>

<script>
// Robust CSV parser that handles quoted fields and "" escapes
function parseCSV(text) {
  // normalize newlines
  text = text.replace(/\r\n/g, "\n").replace(/\r/g, "\n");
  const rows = [];
  let cur = "";
  let row = [];
  let inQuotes = false;
  for (let i = 0; i < text.length; i++) {
    const ch = text[i];
    const next = text[i + 1];

    if (ch === '"') {
      // if double quote inside quoted field, consume one and add a quote
      if (inQuotes && next === '"') {
        cur += '"';
        i++;
      } else {
        inQuotes = !inQuotes;
      }
      continue;
    }

    if (ch === ',' && !inQuotes) {
      row.push(cur);
      cur = "";
      continue;
    }

    if (ch === '\n' && !inQuotes) {
      row.push(cur);
      rows.push(row);
      row = [];
      cur = "";
      continue;
    }

    cur += ch;
  }
  // push last cell/row if any
  if (cur !== "" || row.length) {
    row.push(cur);
    rows.push(row);
  }

  // Trim BOM from first cell if present and trim whitespace
  return rows.map(r => r.map(c => (c || "").replace(/^\ufeff/, "").trim()));
}

function renderCSVTable(text) {
  const rows = parseCSV(text);

  if (!rows || rows.length === 0) {
    throw new Error("CSV е празен или невалиден.");
  }

  // Find header row: first row that has at least 2 non-empty cells and at least 3 columns
  let headerIndex = rows.findIndex(r => {
    const nonEmpty = r.filter(c => c !== "");
    return nonEmpty.length >= 2 && r.length >= 3;
  });

  // fallback to first row if detection failed
  if (headerIndex === -1) headerIndex = 0;

  const headers = rows[headerIndex].map(h => h || "");
  const dataRows = rows.slice(headerIndex + 1);

  const colCount = headers.length;

  // Build table HTML ensuring every row has exactly colCount cells (pad with empty strings)
  let html = "<table class='csv-table'><thead><tr>";
  for (let i = 0; i < colCount; i++) {
    html += `<th>${headers[i] || ""}</th>`;
  }
  html += "</tr></thead><tbody>";

  dataRows.forEach(r => {
    // skip completely empty rows
    const allEmpty = r.every(cell => (cell || "").trim() === "");
    if (allEmpty) return;
    html += "<tr>";
    for (let i = 0; i < colCount; i++) {
      const cell = (r[i] || "").trim();
      // If the cell is long, we can keep it but allow wrapping
      html += `<td>${cell}</td>`;
    }
    html += "</tr>";
  });

  html += "</tbody></table>";
  return html;
}

// Fetch, parse and inject
fetch("/assets/tablica.csv")
  .then(response => {
    if (!response.ok) throw new Error("Не може да се зареди CSV файлът.");
    return response.text();
  })
  .then(text => {
    const wrapper = document.getElementById("csv-table");
    try {
      const html = renderCSVTable(text);
      wrapper.innerHTML = html;
    } catch (err) {
      wrapper.innerHTML = `<p style="color:red;">Грешка при обработка на CSV: ${err.message}</p>`;
      console.error(err);
    }
  })
  .catch(err => {
    const wrapper = document.getElementById("csv-table");
    wrapper.innerHTML = `<p style="color:red;">Грешка при зареждане на таблицата.</p>`;
    console.error(err);
  });
</script>

<style>
.csv-table-wrapper {
  overflow-x: auto;
  margin: 1.25em 0;
  /* ensure wrapper uses full available width (won't force layout outside your container) */
  width: 100%;
}

/* make table take full width of wrapper but allow horizontal scroll if it can't fit */
.csv-table {
  border-collapse: collapse;
  width: 100%;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial;
  font-size: 0.95rem;
  box-shadow: 0 2px 6px rgba(0,0,0,0.06);
}

/* cells */
.csv-table th, .csv-table td {
  border: 1px solid #e6e6e6;
  padding: 10px 12px;
  text-align: left;
  vertical-align: top;
  word-break: break-word;
}

/* header style */
.csv-table th {
  background-color: #004aad;
  color: #fff;
  font-weight: 600;
  position: sticky;
  top: 0;
  z-index: 2;
}

/* zebra + hover */
.csv-table tr:nth-child(even) td { background: #fafafa; }
.csv-table tr:hover td { background: #f1f7ff; }

/* nice small screens fallback */
@media (max-width: 700px) {
  .csv-table { min-width: 600px; font-size: 0.9rem; }
}
</style>
