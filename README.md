# Portfolio Biodiversity Impact Score (PBIS) for banks in Indonesia, Malaysia and Singapore

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24242785.svg)](https://doi.org/10.5281/zenodo.24242785)

Data and code for the paper:

> T. R. D. Cahyaningtyas and N. A. Rizal, "Portfolio Biodiversity Impact Assessment and Unsupervised Learning for Banks in Indonesia, Malaysia, and Singapore," in *Proc. 2026 6th International Conference on Intelligent Cybernetics Technology & Applications (ICICyTA)*, 2026.

PBIS combines ENCORE pressure ratings with the sector mix of each bank's business lending. It is a relative index of how much of a loan book sits in sectors that put material pressure on nature. It does not measure actual biodiversity loss or financial risk.

The sample covers 13 banks over 2023-2025: Mandiri, BRI, BNI and BCA (Indonesia); Maybank, CIMB, RHB, Public Bank, Hong Leong Bank and AmBank (Malaysia); DBS, OCBC and UOB (Singapore).

## Contents

```
PBIS_analysis.ipynb            notebook that reproduces all tables and figures
data/credit_sector.xlsx        gross loans by reported sector, mapped to ISIC sections
data/encore_raw.xlsx           ENCORE pressure ratings (October 2024 release, downloaded 20 Sep 2026)
data/env_lseg_template.xlsx    empty template for the LSEG Environmental Pillar Scores
requirements.txt
LICENSE
CITATION.cff
```

## Running the notebook

Locally:

```
pip install -r requirements.txt
jupyter notebook PBIS_analysis.ipynb
```

On Google Colab, upload the notebook and either upload the `data` folder next to it or upload the files when the notebook asks for them.

Results are written to `outputs/`: tables as CSV (`outputs/tables`), figures as PNG and PDF (`outputs/figures`), all tables in one workbook (`outputs/PBIS_results.xlsx`), and the numbers quoted in the paper (`outputs/paper_numbers.json`). A full run takes a few minutes because of the bootstrap (2,000) and permutation (10,000) loops. The random seed is fixed at 42.

## Data

**credit_sector.xlsx.** Sheet `credit` holds consolidated gross loans by economic sector for each bank and year, taken from annual reports. `Reported sector` keeps the bank's own category name; `Mapped ISIC section` gives the ISIC Rev. 4 section(s) used in the analysis. Amounts are in millions of local currency (`Unit`). Sheet `banks` lists full names and stock codes, and sheet `notes` describes the mapping choices. In short:

- a category that covers several ISIC sections is split equally across them;
- loans to individuals (mortgages, consumer loans, credit cards, private individuals) are mapped to `Household` and left out of PBIS;
- the Indonesian banks report no consumer category, so their `Others` line is treated as household lending;
- Singapore's `Building and construction` is split 50/50 between construction (F) and real estate (L).

The last two choices, and the equal split of combined categories, are varied in Section 2.5 of the notebook.

**encore_raw.xlsx.** Extract of the ENCORE file "09. Pressure materiality detail" (October 2024 release, downloaded on 20 September 2026): 271 activities by 13 pressures. In the baseline, N/A ratings are coded 0 and ND (no data) ratings are treated as missing. See the `notes` sheet for column definitions.

**LSEG Environmental Pillar Scores.** These scores are licensed and cannot be shared here. If you have access, fill in `data/env_lseg_template.xlsx` (one row per bank, one column per year) and save it as `data/env_lseg.xlsx`. Without this file the notebook still runs; only Section 3 (RQ2) and Fig. 3 are skipped. Because the bootstrap and permutation loops share one random generator, the clustering bootstrap ARI values (Section 4) can then differ from the paper in the second decimal; all other results are unchanged.

## Licence and sources

Code: MIT licence (see `LICENSE`). Loan data: compiled from the banks' public annual reports and shared under CC BY 4.0. ENCORE data: Global Canopy, UNEP FI and UNEP-WCMC, https://www.encorenature.org; please cite ENCORE and follow its terms of use.

## Contact

Titis Ruci Dewi Cahyaningtyas, School of Economics and Business, Telkom University, titisrucidewi@student.telkomuniversity.ac.id
