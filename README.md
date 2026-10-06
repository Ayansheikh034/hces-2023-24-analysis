# Household Consumption, Inequality and Welfare Coverage in India
### Evidence from the HCES 2023–24 Unit-Level Data

**Third Prize**, Data Visualization Hackathon *"Innovate with GoIStats"* (2025), organised by the National Statistics Office (NSO), MoSPI, with MyGov (25 February – 31 March 2025).

**Author:** Ayan Bashir Sheikh, M.Sc. Statistics, Department of Statistics, Savitribai Phule Pune University, Pune

## About

This project analyses the unit-level data of the Household Consumption Expenditure Survey (HCES) 2023–24. It asks:

- How does Monthly Per Capita Expenditure (MPCE) differ between rural and urban India, by State/UT?
- How unequal is spending across fractile classes and social groups?
- How do States cluster by the structure of their food spending?
- How far are households covered by the Public Distribution System (PDS), PMGKY, electricity and clean cooking fuel?

All estimates are weighted using the survey multipliers.

## Key findings

- Urban MPCE (Rs. 6,996) is about 1.7 times rural MPCE (Rs. 4,122). The ratio ranges from 1.00 (Lakshadweep) to 2.04 (Meghalaya).
- The top 5% spend about 6 times (rural) and 8.5 times (urban) as much per head as the bottom 5%. A grouped-data Gini is about 0.24 (rural) and 0.28 (urban).
- Food is 47.0% of rural and 39.7% of urban MPCE.
- Ration-card gaps are largest in Chandigarh, Delhi and the urban sectors of several larger States.
- Electrification is nearly complete, but rural LPG shares are only 15–21% in five States.

## Repository contents

```
.
├── README.md
├── paper/
│   ├── main.tex              # 10-page summary paper (LaTeX)
│   └── figs/                 # charts used by the paper (Figure_19, 20, 21, 47, 53, 56)
├── report/
│   └── HCES_full_report.pdf  # full 105-page report
├── code/                     # analysis scripts / notebooks
├── tables/                   # output tables behind the report
└── figures/                  # all charts from the full report
```

## Reproducing the paper

1. Upload `paper/main.tex` and the `figs/` folder to Overleaf (or compile locally with `pdflatex`).
2. Set `\showfigstrue` in `main.tex` to include the charts. The default builds a 10-page version without them.
3. Replace the `\repo` placeholder with this repository's URL.

## Data

The HCES 2023–24 unit-level data are published by MoSPI and are not redistributed here:
<https://microdata.gov.in/NADA/index.php/catalog/226>

Download the data from MoSPI and place it in a local `data/` folder (ignored by git) before running the code.

## Limitations

All results are descriptive. MPCE measures reported spending, not income or wealth. Small State/UT cells rest on few sampled first-stage units, and the Gini coefficient is a grouped-data approximation. See Section "Limitations and Next Steps" in the paper.

## Citation

> Sheikh, A. B. (2025). *Household Consumption, Inequality and Welfare Coverage in India: Evidence from the HCES 2023–24 Unit-Level Data.*

## Licence

Code: MIT. Report text and figures: CC BY 4.0. (Change these if you prefer a different licence.)

## Contact

Ayan Bashir Sheikh – add your email or LinkedIn here.
