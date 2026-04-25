# FIFA Men's World Cup — Historical Confederation Analysis

A statistical analysis of inter-confederation performance across all FIFA Men's World Cup tournaments, examining win rates, goal-scoring patterns, head-to-head records, knockout stage performance, and clustering of confederations by historical strength.

---

## Research Question

> Which confederations have historically dominated the FIFA Men's World Cup, and are the observed performance differences between them statistically significant?

---

## Key Findings

- **CONMEBOL and UEFA form a statistically distinct elite tier**, separated from all other confederations at p < 0.001 under both Bonferroni and Benjamini-Hochberg corrections
- **CONMEBOL and UEFA are not significantly different from each other** (p = 0.669) — making their head-to-head matchups the most competitive inter-confederation contests at the World Cup
- **CAF, CONCACAF, and AFC show no statistically significant differences between them**, forming a second tier with win rates between 18–23%
- The **3-tier structure is independently confirmed** by both hierarchical and k-means clustering
- **Mexico is the only nation outside UEFA and CONMEBOL** to have won 10 or more World Cup matches (17 wins)
- **Brazil leads all nations** by every metric: points (247), wins (76), goals scored (237), and goal difference (+129)
- Performance gaps **widen in knockout matches** — CONCACAF has never beaten CONMEBOL in a knockout (0 from 5), and UEFA's win rate against CAF rises from 52.5% to 78.6% in elimination rounds

---

## Dataset

**Source: Joshua Fjelstul's World Cup Dataset**
https://github.com/jfjelstul/worldcup

Two files are used from the dataset:

- `data/matches.csv` — match-level data for all Men's and Women's World Cup fixtures
- `data/teams.csv` — team reference file including confederation membership

| Attribute | Value |
|-----------|-------|
| Tournaments covered | 1930 – present |
| Total matches | All Men's World Cup fixtures |
| Confederations | AFC, CAF, CONCACAF, CONMEBOL, OFC, UEFA |

> Please credit Joshua Fjelstul when using this data. See the original repository for full license details: https://github.com/jfjelstul/worldcup

---

## Methodological Decisions

Several data decisions were made to ensure historical accuracy:

- **West Germany and Germany are treated as separate nations** — merging them would require arbitrary decisions about which modern nations inherit representation from dissolved political entities such as the Soviet Union (Russia, Ukraine, etc.) and Yugoslavia (Serbia, Croatia, Montenegro)
- **Australia** is assigned to OFC for tournaments up to and including 2006, and to AFC from 2010 onward, reflecting its actual confederation change
- **Israel** is assigned to AFC for the 1970 World Cup, its only participation under that confederation
- **Penalty shootout results** are treated as draws in regulation time, with a separate wins-including-penalties metric maintained in all head-to-head tables
- **All confederation analyses** are restricted to inter-confederation matches only, excluding intra-confederation group stage matches

---

## Project Structure

```
WorldCup/
│
├── README.md
├── wc_analysis.Rmd                       # Full analysis pipeline — knits to PDF
├── matches.csv                           # Match-level World Cup data
├── teams.csv                             # Team and confederation reference
├── wc_graphs.pdf                         # All charts compiled
├── wc_tables.pdf                         # All formatted tables compiled
└──  WorldCup_Confederation_Report.pdf    # Full written report

---

## Charts Produced

| Chart | Description |
|-------|-------------|
| Confederation Win Rate Heatmap | Win rates for each confederation against every other confederation across all inter-confederation matches |
| Win Rate vs Goals Per Game | Scatter plot showing each confederation's win rate and average goals scored per game |
| Hierarchical Clustering Dendrogram | Ward's method clustering on win rate, goals per game, and goals against — 3-cluster solution |
| K-Means Cluster Plot | K-means clustering (k=3) on win rate and goals per game confirming the 3-tier structure |

---

## Tables Produced

| Table | Description |
|-------|-------------|
| Country Table | All-time World Cup record for all 86 nations: matches, wins, draws, losses, goals for/against, goal difference, points, win rate |
| Confederation Table | Aggregated performance per confederation in inter-confederation matches |
| Head-to-Head (All Matches) | Full head-to-head record between every confederation pair including penalty shootout wins |
| Head-to-Head (Knockout Only) | Same as above restricted to knockout stage matches |
| Pairwise Proportion Tests | Two-proportion z-tests for all 15 confederation pairs with Bonferroni and BH corrections |

---

## Setup and Usage

### Prerequisites

- R 4.0+
- RStudio (recommended for knitting)

### Install required packages

```r
install.packages(c(
  "tidyverse", "scales", "tibble",
  "broom", "kableExtra"
))
```

### Update data paths

Open `wc_analysis.Rmd` and update the file paths to point to your local `data/` folder:

```r
wc_matches <- read.csv("data/matches.csv")
teams      <- read.csv("data/teams.csv")
```

### Knit the analysis

Open `wc_analysis.Rmd` in RStudio and click **Knit → Knit to PDF**, or run:

```r
rmarkdown::render("wc_analysis.Rmd", output_format = "pdf_document")
```

---

## Statistical Methods

| Method | Purpose |
|--------|---------|
| Proportion z-test (`prop.test`) | Test whether win rate differences between confederation pairs are statistically significant |
| Bonferroni correction | Conservative multiple comparison correction — controls family-wise error rate |
| Benjamini-Hochberg (BH) correction | Less conservative correction — controls false discovery rate, preferred for exploratory analysis |
| Chi-square test of homogeneity | Overall test confirming confederations are not all performing equally before pairwise comparisons |
| Hierarchical clustering | Ward's method on Euclidean distance using scaled win rate, goals per game, and goals against |
| K-means clustering | k=3 solution on scaled win rate and goals per game (seed = 123, 25 restarts) |

---

## Limitations

- OFC has participated in only 13 inter-confederation matches — all OFC statistics should be interpreted with caution
- The analysis treats all World Cup eras equally; the competitive gap between confederations may have narrowed or widened over time
- Win rate does not distinguish between close and emphatic victories
- Keeping historical nations separate from successor states means some nations have fewer data points than under a merged approach

---

## References

- Fjelstul, J. (2022). World Cup Dataset. GitHub. https://github.com/jfjelstul/worldcup
- FIFA Official Records: https://www.fifa.com/fifa-world-ranking

---

## License

Code in this repository is released under the MIT License. Data files are subject to the license terms of the original dataset — see https://github.com/jfjelstul/worldcup for full details.
