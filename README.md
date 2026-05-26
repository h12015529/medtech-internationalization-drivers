# Internationalisierung börsennotierter Medizintechnik-Unternehmen
### ExInt II: Research Designs in SME Research | WU Vienna | SS 2026

Autor: Thomas Lackner (h12015529)

## Research Question

> Welchen Einfluss haben Firmengröße und F&E-Intensität auf den Internationalisierungsgrad börsennotierter Medizintechnik-Unternehmen?

## Theoretischer Hintergrund

| Theorie | Kernaussage | Implikation für die Internationalisierung |
|---------|-------------|--------------------------------------------|
| Uppsala-Modell (Johanson & Vahlne 1977) | Internationalisierung verläuft als gradueller, ressourcenabhängiger Prozess | Größere Firmen verfügen über mehr Ressourcen, um diesen Prozess voranzutreiben |
| OLI-Paradigma (Dunning 1988) | Internationalisierung beruht auf Ownership-, Location- und Internalization-Vorteilen | F&E schafft firmenspezifische Ownership-Vorteile, die Auslandsexpansion begünstigen |
| Resource-based View (Barney 1991) | Wettbewerbsvorteile entstehen aus wertvollen, seltenen Ressourcen (VRIN) | Firmengröße und F&E-Bestand sind Ressourcen, die Internationalisierung ermöglichen |

## Hypothesen

- **H1:** Der Internationalisierungsgrad steht in einem positiven Zusammenhang mit der Firmengröße. *(Test: β(Größe) > 0)*
- **H2:** Eine höhere F&E-Intensität geht mit einem höheren Internationalisierungsgrad einher. *(Test: β(F&E-Intensität) > 0)*
- **H3:** Profitablere Medizintechnik-Unternehmen weisen einen höheren Internationalisierungsgrad auf als weniger profitable. *(Test: β(ROA) > 0)*

## Data

| Item | Detail |
|------|--------|
| Quelle | WRDS / Compustat Global |
| Heruntergeladen | 26.05.2026 |
| Lizenz | WRDS subscriber agreement |
| Sample | Börsennotierte Medizintechnik-Unternehmen (SIC 3841 & 3845) |
| Zeitraum | 2005–2025 |
| Analyseeinheit | Firm-year |

**Zentrale Variablen:**

| Variable | Compustat-Feld(er) | Beschreibung |
|----------|--------------------|--------------|
| DOI (Internationalisierungsgrad) | `pifo / sale` | Anteil des im Ausland erzielten Einkommens (abhängige Variable) |
| Firmengröße | `log(at)` | Logarithmierte Bilanzsumme (H1) |
| F&E-Intensität | `xrd / at` | F&E-Ausgaben relativ zur Bilanzsumme (H2) |
| ROA (Profitabilität) | `ib / at` | Return on Assets (H3) |
| Leverage (Kontrollvariable) | `dltt / at` | Langfristige Verbindlichkeiten / Bilanzsumme |
| Firmenalter (Kontrollvariable) | `fyear - inco` | Jahre seit Gründung |

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

## Literaturverzeichnis

Barney, J. (1991). Firm Resources and Sustained Competitive Advantage. *Journal of Management*, 17(1), 99–120.

Dunning, J. H. (1988). The Eclectic Paradigm of International Production: A Restatement and Some Possible Extensions. *Journal of International Business Studies*, 19(1), 1–31.

Johanson, J., & Vahlne, J.-E. (1977). The Internationalization Process of the Firm — A Model of Knowledge Development and Increasing Foreign Market Commitments. *Journal of International Business Studies*, 8(1), 23–32.

Lu, J. W., & Beamish, P. W. (2001). The internationalization and performance of SMEs. *Strategic Management Journal*, 22(6–7), 565–586.