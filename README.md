# Profitabilität börsennotierter Medizintechnik-Unternehmen
### ExInt II: Research Designs in SME Research | WU Vienna | SS 2026

Autor: Thomas Lackner (h12015529)

## Research Question

> Welchen Einfluss hat der Verschuldungsgrad (Leverage)  auf die Profitabilität (ROA) börsennotierter Medizintechnik-Unternehmen und moderiert die Firmengröße diesen Zusammenhang?

## Theoretischer Hintergrund

| Theorie | Kernaussage | Implikation für die Hypothesen |
|---------|-------------|--------------------------------------------|
| Trade-off-Theorie (Kraus & Litzenberger 1973; Modigliani & Miller 1963) | Firmen wägen Steuervorteile von Fremdkapital gegen Kosten finanzieller Notlage ab | Jenseits eines Optimums senkt zusätzliche Verschuldung die Profitabilität (Zinslast, Distress) — Basis für H1 |
| Pecking-Order-Theorie (Myers & Majluf 1984) | Firmen finanzieren bevorzugt intern; hohe externe Verschuldung geht mit geringerer Profitabilität einher | Stützt den negativen Leverage–ROA-Zusammenhang (H1) |
| Resource-based View (Barney 1991) | Wettbewerbsvorteile entstehen aus wertvollen Ressourcen | Firmengröße ist eine Ressource (Debt Capacity, Skaleneffekte, geringeres Distress-Risiko), die den Leverage-Effekt abschwächt — Basis für H2 |


## Hypothesen

- **H1:** Ein höherer Verschuldungsgrad (gemessen als `dltt/at`) steht in einem
  negativen Zusammenhang mit der Profitabilität (ROA), da eine steigende
  Zinslast und ein höheres Financial-Distress-Risiko den laufenden
  Unternehmensgewinn reduzieren.
- **H2:** Die Firmengröße (`log(at)`) moderiert diesen Zusammenhang positiv:
  Bei größeren Unternehmen fällt der negative Leverage-Effekt schwächer aus,
  da sie über höhere Debt Capacity und ein geringeres Distress-Risiko
  verfügen (Interaktion: `leverage × ln_at`).


## Data

| Item | Detail |
|------|--------|
| Quelle | WRDS / Compustat Global |
| Heruntergeladen | 26.05.2026 |
| Lizenz | WRDS subscriber agreement |
| Sample | Börsennotierte Medizintechnik-Unternehmen (SIC 3841 & 3845) |
| Zeitraum | 2015–2025 |
| Analyseeinheit | Firm-year |
| Raw rows     | 365.259                             |
| Clean rows   | 3.108                               |
| Analysesample     | 2.713 Firm-years / 352 Firmen  |

**Hinweis zum Design (DOI):** Maße zur Internationalisierung (z. B. `pifo`,
foreign income) sind in diesem Compustat-Global-Pull nicht verfügbar, und
`rect/sale` erzeugt extreme Ausreißer. Die Fragestellung wurde daher auf
firmeninterne Profitabilitätstreiber fokussiert. Als zentrale unabhängige
Variable dient der Verschuldungsgrad — theoretisch gut fundiert in der
Kapitalstrukturforschung und mit hoher Datenabdeckung verfügbar.

**Zentrale Variablen:**

**Abhängige Variable (Y):**

| Variable | Compustat-Feld(er) | Formel | Beschreibung |
|----------|-------------------|--------|--------------|
| Profitabilität (ROA) | `ib, at` | `ib / at` | Return on Assets |

**Unabhängige Variablen (X):**

| Variable | Compustat-Feld(er) | Formel | Beschreibung |
|----------|-------------------|--------|--------------|
| Leverage | `dltt, at` | `dltt / at` | Haupt-X (H1) |
| Firmengröße | `at` | `log(at)` | Moderator + Kontrolle (H2) |
| Leverage × Größe | – | `leverage * ln_at` | H2-Interaktion |

**Kontrollvariablen:**

| Variable | Compustat-Feld(er) | Formel | Beschreibung |
|----------|-------------------|--------|--------------|
| F&E-Intensität | `xrd, at` | `xrd.fillna(0) / at` | Innovationsaufwand relativ zur Bilanzsumme |
| Kapitalintensität | `capx, at` | `capx / at` | Investitionsintensität |
| Liquidität | `che, at` | `che / at` | Cash-Bestand relativ zur Bilanzsumme |
| EBITDA-Marge | `ebitda, sale` | `ebitda / sale` | Operative Profitabilität |

## How to Reproduce

```bash
# 1. Repository klonen
git clone https://github.com/h12015529/medtech-internationalization-drivers
cd medtech-internationalization-drivers

# 2. Virtuelle Umgebung erstellen und aktivieren
python -m venv .venv
source .venv/bin/activate        # Mac / Linux
# .venv\Scripts\activate         # Windows

# 3. Abhängigkeiten installieren
pip install -r requirements.txt

# 4. WRDS-Zugangsdaten setzen
# .env-Datei anlegen und WRDS-Benutzernamen eintragen

# 5. Gesamte Pipeline ausführen
task all

# --- Oder einzelne Schritte ---
task pull          # 01_pull_data.py    -> data/raw/
task clean         # 02_clean.py        -> data/processed/
task descriptives  # 03_descriptives.py -> output/tables/, output/figures/
task regression    # 04_regression.py   -> output/tables/
```

## Project Structure

```
medtech-internationalization-drivers/
├── data/
│   ├── raw/                   <- von WRDS heruntergeladen (nicht in Git)
│   └── processed/             <- bereinigtes Panel (vom Code erzeugt)
├── code/
│   ├── 01_pull_data.py        <- WRDS Compustat Global Pull
│   ├── 02_clean.py            <- Branchenfilter, Variablenkonstruktion
│   ├── 03_descriptives.py     <- deskriptive Statistik, Abbildungen
│   └── 04_regression.py       <- Panel-Regressionen
├── output/
│   ├── tables/                <- Regressionstabellen
│   └── figures/               <- Abbildungen
├── references/
│   └── library.bib            <- aus Zotero exportiert (Better BibTeX)
├── Taskfile.yml               <- Pipeline-Automatisierung
├── pyproject.toml             <- Projektkonfiguration + ruff-Settings
├── requirements.txt           <- fixierte Paketversionen
├── .gitignore
└── README.md
```
## Ergebnisse (Kurzfassung)

- Höhere Verschuldung senkt die ROA signifikant (β = −0,26, p < 0,01, TWFE) —
  **H1 bestätigt** (Zinslast und Financial-Distress-Risiko).
- Die Firmengröße moderiert den Zusammenhang **nicht** signifikant
  (Interaktion β = 0,01, p = 0,60) — **H2 nicht bestätigt**.
- Der TWFE-Effekt ist ca. 25 % stärker als in der gepoolten OLS → Omitted-
  Variable-Bias; Firm-Fixed-Effects sind notwendig (Hausman: FE vor RE).

## Literaturverzeichnis

Barney, J. (1991). Firm Resources and Sustained Competitive Advantage. *Journal of Management*, 17(1), 99–120.

Kraus, A., & Litzenberger, R. H. (1973). A State-Preference Model of Optimal Financial Leverage. *Journal of Finance*, 28(4), 911–922.

Modigliani, F., & Miller, M. H. (1963). Corporate Income Taxes and the Cost of Capital: A Correction. *American Economic Review*, 53(3), 433–443.

Myers, S. C., & Majluf, N. S. (1984). Corporate Financing and Investment Decisions When Firms Have Information That Investors Do Not Have. *Journal of Financial Economics*, 13(2), 187–221.