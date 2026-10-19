# QBS 103 Data Science - project check in 1

## Project Overview

This project explores early-pregnancy vitamin D levels, gene expression,
and subsequent preeclampsia outcomes using the instructor-provided
GSE85307 dataset.

The goal is to develop a reproducible R workflow for data cleaning,
sample matching, exploratory analysis, and visualization.

## Data Source

The project uses three instructor-provided files:

- GSE85307_covar-1.csv: participant characteristics
- GSE85307_geneExpression.csv: gene expression measurements
- GSE85307_geneNames.csv: probe-to-gene mapping

The supplied expression data contain 500 probes across 157 participants:
110 with normal pregnancy outcomes and 47 who subsequently developed
preeclampsia.

Original publication:

Mirzakhani H, et al. Early pregnancy vitamin D status and risk of
preeclampsia. Journal of Clinical Investigation. 2016;126(12):4702–4715.

https://doi.org/10.1172/JCI89031

GEO accession:

https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE85307

## Variables of Interest

- Primary gene: CYP24A1
- Additional genes for Submission 2: PTX3 and CXCR2
- Continuous covariate: baseline maternal vitamin D level
- Categorical covariate: pregnancy outcome
- Derived categorical covariate: vitamin D <30 or ≥30 ng/mL
- Additional continuous covariates for the final report: maternal age and BMI

The vitamin D threshold follows the original study's grouping convention.

## Submission 1

The first submission focuses on:

1. Importing and cleaning the original data in R.
2. Matching expression measurements to participant characteristics by geoID.
3. Mapping probe identifiers to gene names.
4. Creating a histogram of CYP24A1 expression.
5. Creating a scatterplot of vitamin D and CYP24A1 expression.
6. Creating a boxplot grouped by pregnancy outcome and vitamin D category.

All plots are created using ggplot2.

## Planned Extensions

- Develop a reusable plotting function for three genes.
- Incorporate instructor feedback.
- Produce a stratified summary statistics table.
- Create a clustered ten-gene heatmap with two annotation tracks.
- Add an eligible visualization type not previously covered in class.
- Prepare a reproducible final report.

## Reproducibility

All data cleaning and analysis are performed in R.

Samples are matched using identifiers rather than their original order.
The original input files are preserved, and no additional log
transformation is applied to the supplied expression values.

Code, documentation, and results will be updated throughout the project.

## Interpretation

These exploratory analyses describe associations and do not establish
causal effects. The selected case-control sample should not be used to
estimate population preeclampsia incidence.

## Author

Chunlei Li
Dartmouth College

## Acknowledgments

ChatGPT/Codex was used to assist with draft code.I also watched numerous 
YouTube tutorials on GitHub workflows and project data processing to deepen my understanding.
Assistance will be disclosed in accordance with course requirements.
