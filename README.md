# Early-Pregnancy Vitamin D, Gene Expression, and Preeclampsia

QBS 103 Data Science · Chunlei Li 

## Project overview

This project explores how early-pregnancy vitamin D levels and gene expression relate to later preeclampsia. It uses the  GSE85307 dataset. The goal is a reproducible R workflow covering data cleaning, sample matching, exploratory analysis, and visualization.

The project was built in three stages. Each stage reuses the previous one and adds a new layer:

| Stage | Question it adds | Main outputs |
|---|---|---|
| Submission 1 | How is one gene (CYP24A1) related to vitamin D and pregnancy outcome? | Clean, matched data; three ggplot2 figures |
| Submission 2 | Do other genes show the same patterns? | A reusable plotting function applied to three genes |
| Final analysis | What does the study population look like, and how do the most variable genes compare across groups? | Table 1, a ten-gene clustered heatmap, a dumbbell plot |

## Data source

The project uses three files:

- `GSE85307_covar-1.csv`: participant characteristics (157 rows)
- `GSE85307_geneExpression.csv`: expression values for 500 probes across 157 participants
- `GSE85307_geneNames.csv`: probe-to-gene mapping

Of the 157 participants, 110 had normal pregnancy outcomes and 47 later developed preeclampsia. No participants were excluded, and no covariate or expression values were missing.

- Original publication: Mirzakhani H, et al. Early pregnancy vitamin D status and risk of preeclampsia. *Journal of Clinical Investigation*. 2016;126(12):4702–4715. https://doi.org/10.1172/JCI89031
- GEO accession: https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE85307

## Variables of interest

- **Primary gene:** CYP24A1, chosen because it is involved in vitamin D metabolism
- **Additional genes (Submission 2 onward):** UTS2 and RUNDC3A, the two genes with the highest variance among the 500 supplied genes. 
- **Continuous covariate:** baseline maternal vitamin D, 25(OH)D (`mat_vitD`)
- **Categorical covariate:** pregnancy outcome (`preg_preeclampsia`: Normal / Preeclampsia)
- **Derived categorical covariate:** vitamin D group (`vitD_group`: <30 or ≥30 ng/mL). The 30 ng/mL threshold follows the source study; it is not a universal diagnostic cutoff.
- **Additional covariates (final analysis):** maternal age and BMI, used in the summary table

Subgroup sizes used throughout:

| | <30 ng/mL | ≥30 ng/mL |
|---|---|---|
| Normal | 78 | 32 |
| Preeclampsia | 41 | 6 |

---

## Submission 1: Building the foundation with one gene

**File:** `Submission1_Chunlei_li-1.Rmd`

Submission 1 builds the data pipeline that every later stage depends on, and then looks at a single gene.

**Data preparation**

1. Import the three original files without changing them.
2. Remove the text labels (for example, `"mat_vitD: 24.3"` → `24.3`) and convert the five continuous covariates to numbers, stopping if any conversion fails.
3. Check that sample IDs, probe IDs, and gene names have no duplicates, and that both files contain the same set of samples.
4. Reorder the expression columns to match the covariate rows **by `geoID`**, not by their original positions.
5. Replace probe IDs with gene names using the mapping file.
6. Define the outcome factor and the derived vitamin D group, then combine everything into one table with one participant per row.

**Figures (ggplot2)**

1. Histogram of CYP24A1 expression
2. Scatterplot of vitamin D and CYP24A1 expression, with an unadjusted linear fit
3. Boxplot of CYP24A1 by pregnancy outcome, faceted by vitamin D group, with individual points and subgroup counts

**Findings:** CYP24A1 expression had one main peak near 4.8. It showed little linear association with vitamin D, and its distributions overlapped across outcome groups.

## Submission 2: Generalizing to three genes

**File:** `Submission2_revise.Rmd`

Submission 2 turns the three Submission 1 figures into a function, so the same analysis can be repeated for any gene.

- `makeGenePlots(data, genes)` takes the data and a vector of gene names, and returns a histogram, scatterplot, and boxplot for each gene.
- Gene variance was calculated across all 500 genes to select the two additional genes, UTS2 and RUNDC3A.
- The function was applied to CYP24A1, UTS2, and RUNDC3A.

**Findings:**

- **UTS2** showed two groups of values (most near 5, a smaller group between about 6 and 8.5). Both groups appeared across the range of vitamin D.
- **RUNDC3A** had lower median expression in the preeclampsia group within both vitamin D groups, although the distributions overlapped.

## Final analysis: Population summary and a multi-gene view

**File:** `2_Final_Analysis_R.Rmd`

The final analysis keeps the cleaning steps and the three genes from earlier stages, and adds three new layers.

**1. Improved plotting function.** `make_gene_plots()` now also takes the continuous and categorical covariate names and readable labels as arguments, so it is no longer tied to specific column names.

**2. Summary statistics table (Table 1).** Histograms of vitamin D, age, and BMI were checked first to choose suitable statistics: mean (SD) for age, and median [Q1, Q3] for the right-skewed vitamin D and BMI. Categorical variables are shown as n (%). The table is stratified by pregnancy outcome, printed with `kableExtra`, and exported to `Table1.tex` for the LaTeX report.

*Finding:* the preeclampsia group had lower median vitamin D and higher median BMI; mean age was similar.

**3. Ten-gene clustered heatmap (Figure 10).** The ten genes with the highest standard deviation were plotted with `pheatmap`, using Euclidean distance for rows and columns and two tracking bars (pregnancy outcome and vitamin D group). Rows were not standardized, so colors reflect the supplied expression values.

*Finding:* genes separated mainly by overall expression level, and one cluster of participants had higher UTS2 expression. The tracking bars did not group by outcome or vitamin D.

**4. Dumbbell plot (Figure 11), a plot type not covered in class.** For the same ten genes, it compares median expression in the normal and preeclampsia groups, ordered by the difference.

*Finding:* RUNDC3A showed the largest difference (8.74 vs. 9.15, lower in preeclampsia). TGM2, ZNF594, and CLIC2 were about 0.2 to 0.3 lower; the other six genes differed by 0.06 or less.

---




## Reproducibility principles

- Samples are matched using identifiers, not their original order, and each match is checked in code.
- The original input files are never modified; all cleaning is done on a working copy.
- No additional log transformation is applied to the supplied expression values.
- Software versions are recorded at the end of each analysis.

## Interpretation and limitations

- All analyses are descriptive. No hypothesis tests were performed, and no results were adjusted for other factors such as BMI.
- The data come from a selected case-control sample, so group percentages should not be read as population preeclampsia incidence or risk.
- The preeclampsia subgroup with vitamin D ≥30 ng/mL contains only six participants, so comparisons involving it are very uncertain.
- Genes selected by variance are not necessarily related to the outcome.
- These results describe associations and do not establish causal effects.



