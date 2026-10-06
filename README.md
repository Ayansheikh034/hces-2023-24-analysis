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
3. The `\repo` line in `main.tex` already points to this repository: <https://github.com/Ayansheikh034/hces-2023-24-analysis>

## Data

The HCES 2023–24 unit-level data are published by MoSPI and are not redistributed here:
<https://microdata.gov.in/NADA/index.php/catalog/226>

Download the data from MoSPI and place it in a local `data/` folder (ignored by git) before running the code.

## Understanding the multiplier (survey weight)

Every household in the HCES unit-level file carries a **multiplier**. It is the survey weight, and every estimate in this project uses it. This section explains what it is and how to use it. The explanation follows the training slides *"Calculating and using multipliers"* by Soumendra Chattopadhyay (Sampling and Official Statistics Unit, Indian Statistical Institute, Kolkata).

### What is a multiplier?

A multiplier is a **blow-up factor**. It turns values from the sample households into estimates for the whole population. Put simply, each sample household *represents* a number of households in the population, and that number is its multiplier.

- If 4 households are drawn from a population of 100, each sample household represents 100/4 = **25** households, so each gets a multiplier of 25.
- If the 100 households are split into two strata of 20 and 80, and 2 households are drawn from each, the two households from the first stratum each get a multiplier of 10, and the two from the second each get 40. The second pair counts for more because together they represent more households.

The estimate of a population total is always the sum of (multiplier × sample value):

```
Estimated total of Y  =  Σ Mᵢ · yᵢ        (sum over all sample households)
```

Here `Mᵢ` is the multiplier (the "multiplier part") and `yᵢ` is the household's observed value (the "data part").

### How the multiplier is built

It depends only on **how the sample was drawn**, never on the answers a household gave.

| Design | Multiplier for a sample household |
|---|---|
| One stage, no strata | N / n |
| Stratified | H / h, where h households were drawn from a stratum of H |
| Two-stage (villages, then households) | (N / n) × (Hⱼ / hⱼ), where n of N villages were drawn and hⱼ of Hⱼ households in village j were drawn |

Because it depends only on the design:
- it can differ from household to household;
- it is the **same for every variable**. A household has one multiplier whether you estimate total expenditure, total population or electricity use.

HCES 2023–24 uses a stratified, multi-stage design with first-stage units (villages or urban blocks) drawn with probability proportional to size. The final multiplier combines all these stages, and MoSPI calculates it and supplies it in the data file. You do not need to calculate it yourself.

### Four useful facts

1. **The sum of all multipliers equals the number of households in the population.** In this project the weights add up to about 291.7 million households (193.8 million rural and 97.9 million urban). If the sampled units are persons, the multipliers sum to the number of persons.
2. **Counting households with a condition.** For any yes/no question (owns a TV, has electricity, has no ration card), the estimated number of "yes" households is the sum of the multipliers of the "yes" households. The estimated share is that sum divided by the sum of all multipliers.
3. **Order does not matter.** Rows can be in any order.
4. **Using multipliers is a computing tool.** They let one weight serve many variables. The same estimates can be obtained in other ways.

### How to use it

**Totals**

```
Total households        = Σ M
Total persons           = Σ M · (household size)
Total expenditure       = Σ M · (household monthly expenditure)
```

**Ratios** (estimate the top and the bottom separately, then divide)

```
Ratio R = Y / X   is estimated as   Σ Mᵢ·yᵢ  /  Σ Mᵢ·xᵢ
```

| Quantity | y (numerator) | x (denominator) |
|---|---|---|
| Monthly per capita expenditure (MPCE) | household monthly expenditure | household size |
| Share of households with a TV | 1 if the household owns one, else 0 | 1 |
| Adult literacy rate | literate adults in household | adults in household |
| Per capita floor area | floor area of dwelling | household size |

This is the formula used for MPCE in this project:

```
MPCE = Σ M · X  /  Σ M · n      (X = household expenditure, n = household size)
```

**Subgroups.** To estimate for a State, a sector (rural or urban) or a social group, sum only over the households in that group.

### Example in Python

The column names below are placeholders. Check the exact names in the data layout file that comes with the MoSPI download.

```python
import pandas as pd

df = pd.read_csv("data/hces_household_level.csv")

# Weight = multiplier. Check the MoSPI documentation for the exact rule
# (NSS files usually store it multiplied by 100, so divide by 100).
df["w"] = df["multiplier"] / 100

# Total households in the population
total_hh = df["w"].sum()

# MPCE for each State and sector
grp = df.groupby(["state", "sector"])
mpce = grp.apply(lambda g: (g["w"] * g["hh_expenditure"]).sum()
                           / (g["w"] * g["hh_size"]).sum())

# Share of households with no ration card (indicator is 1 or 0)
no_card = grp.apply(lambda g: (g["w"] * g["no_card"]).sum() / g["w"].sum())
```

### Things to watch

- **Always weight.** An unweighted average describes the sample, not the population, and can be badly misleading because households do not all represent the same number of households.
- **Persons need an extra step.** The multiplier is attached to the household. For person-level estimates, multiply it by the household size (as in the MPCE formula).
- **Scaling.** The weight is stored differently in different NSS files. Confirm the scaling rule in the survey's documentation before computing totals. A quick check is that the weights should sum to about 291.7 million households.
- **Small cells.** Cells with few sampled households give unstable estimates, even when weighted.

## Limitations

All results are descriptive. MPCE measures reported spending, not income or wealth. Small State/UT cells rest on few sampled first-stage units, and the Gini coefficient is a grouped-data approximation. See Section "Limitations and Next Steps" in the paper.

## Citation

> Sheikh, A. B. (2025). *Household Consumption, Inequality and Welfare Coverage in India: Evidence from the HCES 2023–24 Unit-Level Data.*

## Licence

Code: MIT. Report text and figures: CC BY 4.0.

## Contact

Ayan Bashir Sheikh
- Email: [ayansheikh034@gmail.com](mailto:ayansheikh034@gmail.com)
- LinkedIn: [linkedin.com/in/ayan-sheikh-900255196](https://www.linkedin.com/in/ayan-sheikh-900255196)
