# LC-MS Proteomics Analysis: FragPipe DDA-LFQ Workflow

## Project overview

This project demonstrates an end-to-end label-free quantitative
proteomics workflow using DDA LC-MS/MS data.

The analysis starts from mzML files and proceeds through peptide
identification, protein inference, label-free quantification,
quality control, multivariate analysis, and differential protein
abundance analysis.

## Dataset

The analysis uses six human DDA LC-MS/MS runs from the FragPipe
LFQ tutorial dataset.

Experimental design:

| Group | Samples |
|------|---------|
| IDHwt | sample03, sample06, sample09 |
| IDHmut | sample11, sample12, sample13 |

Each condition contains three biological samples.

## Software

- FragPipe 24
- MSFragger
- Philosopher
- Percolator
- ProteinProphet
- IonQuant
- Python
- pandas
- NumPy
- matplotlib
- scipy
- scikit-learn
- statsmodels
- JupyterLab

## Proteomics workflow

mzML files  
→ MSFragger database search  
→ peptide-spectrum matches  
→ Percolator rescoring  
→ protein inference and FDR filtering  
→ IonQuant label-free quantification  
→ match-between-runs  
→ MaxLFQ protein abundance  
→ Python downstream analysis

## Protein database

A reviewed human UniProt FASTA database was used.

The search database contained:

- Human reviewed proteins
- Common contaminants
- Reversed decoy sequences

The original human FASTA contained 20,416 entries.

## Data processing

FragPipe produced 8,181 protein groups.

After contaminant removal:

8,175 protein groups remained.

Proteins detected in at least 2 of 3 replicates in either IDHwt
or IDHmut samples were retained.

This resulted in:

6,233 proteins for downstream analysis.

Zeros in MaxLFQ measurements were treated as missing values and
converted to NaN.

Protein intensities were log2 transformed.

For complete-case PCA and differential-abundance analysis,
3,518 proteins quantified in all six samples were used.

## Quality control

Protein coverage across individual samples ranged from approximately
60–67%.

MaxLFQ intensity distributions were highly comparable across runs.

Pearson correlations:

IDHwt replicates: 0.920–0.933

IDHmut replicates: 0.952–0.957

Between-condition correlations were lower: approximately 0.77–0.84.

These results indicated good replicate consistency.

## Principal component analysis

PCA was performed using 3,518 proteins quantified in all six samples.

PC1 explained 66.4% of variance.

PC2 explained 11.6% of variance.

Together, PC1 and PC2 explained 78.0% of total proteomic variance.

IDHwt and IDHmut samples separated strongly along PC1.

## Differential abundance analysis

Protein abundance differences were tested using Welch's t-test.

P-values were corrected using the Benjamini-Hochberg false discovery
rate procedure.

Significance criteria:

q-value < 0.05

and

|log2 fold change| >= 1

Three proteins satisfied both criteria:

| Gene | log2FC | Direction |
|------|-------:|-----------|
| GDI1 | +1.34 | Higher in IDHmut |
| RAB18 | +1.17 | Higher in IDHmut |
| MANF | -1.23 | Higher in IDHwt |

## Interpretation

PCA demonstrated strong global proteomic separation between IDHwt
and IDHmut samples.

However, only three individual proteins remained significant after
multiple-testing correction.

This illustrates the distinction between multivariate separation and
univariate statistical significance. Coordinated differences across
many proteins can strongly influence PCA even when individual proteins
do not reach FDR significance.

The small sample size (n=3 per condition) limits statistical power,
and differential-abundance results should therefore be interpreted
as exploratory.

## Repository structure

```text
data/
results/
notebooks/
docs/
README.md
environment.yml
