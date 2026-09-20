# Proteomics Theory Foundations

**Project:** LC-MS/MS Proteomics Analysis  
**Purpose:** Core theoretical reference before starting practical FragPipe/MSFragger analysis  
**Recommended repository location:** `docs/proteomics_theory_foundations.md`

---

## 1. Overview of Bottom-Up Proteomics

Bottom-up proteomics studies proteins by first digesting them into smaller peptides and then analyzing those peptides using liquid chromatography coupled to tandem mass spectrometry (LC-MS/MS).

A simplified workflow is:

```text
Protein sample
    ↓
Protein digestion
    ↓
Peptide mixture
    ↓
Liquid chromatography (LC)
    ↓
Ionization
    ↓
Mass spectrometry
    ↓
MS1 precursor measurement
    ↓
Precursor isolation
    ↓
Fragmentation
    ↓
MS2 fragment measurement
    ↓
Peptide identification
    ↓
Protein inference
    ↓
Quantification
    ↓
Statistical and biological analysis
```

The important principle is that a mass spectrometer does not directly read protein names or peptide sequences. It measures properties of ions, primarily **mass-to-charge ratio (m/z)** and signal intensity. Computational analysis converts these measurements into peptide and protein identifications.

---

# 2. Mass Spectrometry Fundamentals

## 2.1 Why molecules must be ionized

Mass spectrometers manipulate charged particles using electric and/or magnetic fields. Neutral molecules therefore need to be converted into ions.

A peptide can gain one or more protons:

\[
M + H^+ \rightarrow [M+H]^+
\]

or:

\[
M + 2H^+ \rightarrow [M+2H]^{2+}
\]

The instrument measures:

\[
m/z
\]

where:

- **m** = ion mass
- **z** = ion charge

For an ion carrying multiple charges:

\[
m/z = \frac{M + zH}{z}
\]

where \(H\) is approximately the mass of a proton.

For quick reasoning, proton mass can sometimes be ignored:

\[
m/z \approx \frac{M}{z}
\]

Example:

A peptide with neutral mass ≈ 2000 Da:

```text
z = +1 → m/z ≈ 2000
z = +2 → m/z ≈ 1000
z = +4 → m/z ≈ 500
```

Multiple charging allows relatively large peptides and proteins to appear within an accessible m/z range.

---

# 3. Ionization Methods

## 3.1 Electrospray Ionization (ESI)

ESI converts analytes in liquid solution into gas-phase ions.

Simplified process:

```text
LC eluate
   ↓
high-voltage capillary
   ↓
charged droplets
   ↓
solvent evaporation
   ↓
smaller droplets
   ↓
gas-phase peptide ions
```

### Important properties of ESI

- Works directly with molecules in solution.
- Couples naturally to LC.
- Commonly produces **multiply charged ions**.
- Is a **soft ionization** method, meaning peptides can often be transferred into the gas phase without extensive fragmentation.

This makes ESI extremely important for LC-MS/MS proteomics.

---

## 3.2 Matrix-Assisted Laser Desorption/Ionization (MALDI)

MALDI uses:

```text
analyte + matrix
       ↓
dried crystal on target plate
       ↓
laser irradiation
       ↓
desorption and ionization
```

The matrix absorbs laser energy and assists analyte desorption and ionization.

### Important properties of MALDI

- Sample is normally mixed with a matrix and dried.
- Uses laser pulses.
- Often produces mainly **singly charged ions**.
- Is commonly paired with TOF mass analyzers.

A useful memory aid:

```text
ESI   = electricity + solution
MALDI = matrix + laser
```

---

# 4. Mass Analyzers

Ionization produces ions. A **mass analyzer** then distinguishes ions according to their m/z.

---

## 4.1 Quadrupole

A quadrupole contains four rods with oscillating electrical fields.

Its most useful conceptual role is:

> **m/z filter**

Example:

```text
Ions enter:
400  500  600  700  800 m/z

Quadrupole selects:
600 m/z
```

In tandem MS instruments, a quadrupole is often used to isolate a specific precursor ion before fragmentation.

---

## 4.2 Time-of-Flight (TOF)

TOF determines m/z by measuring how long accelerated ions take to travel through a defined flight region.

Conceptually:

```text
Ion acceleration
      ↓
flight tube
      ↓
detector
```

Under comparable acceleration conditions:

```text
lower m/z → generally reaches detector sooner
higher m/z → generally reaches detector later
```

MALDI and TOF work naturally together because the MALDI laser provides a defined starting time for each ion packet.

---

## 4.3 Orbitrap

In an Orbitrap, ions are trapped in an electrostatic field and oscillate around a central electrode.

Different ions have different oscillation frequencies.

The instrument measures an electrical image current and determines ion frequencies. A Fourier transform is then used to convert those frequency signals into an m/z spectrum.

Conceptually:

```text
Ion oscillation
     ↓
frequency signal
     ↓
Fourier transform
     ↓
m/z spectrum
```

Orbitrap instruments are widely used in proteomics because of their high mass accuracy and high resolving power.

---

# 5. Mass Accuracy and Resolution

These terms are related but are not identical.

## 5.1 Mass accuracy

Mass accuracy describes how close the measured m/z is to the true/theoretical m/z.

Example:

```text
Theoretical = 500.2500
Measured    = 500.2504
```

The measurement is very close to the expected value.

---

## 5.2 Resolution

Resolution describes the ability to distinguish two ions with very similar m/z values.

For example:

```text
500.200
500.205
```

A low-resolution instrument may see these as one broad peak.

A high-resolution instrument may resolve two distinct peaks.

---

# 6. Tandem Mass Spectrometry (MS/MS)

MS1 alone measures intact precursor ions, but precursor mass is often insufficient to uniquely identify a peptide.

Tandem mass spectrometry solves this by isolating a precursor ion and fragmenting it.

```text
Peptide mixture
      ↓
MS1
      ↓
select precursor
      ↓
fragment precursor
      ↓
MS2
      ↓
measure fragment ions
```

---

## 6.1 MS1

MS1 primarily provides information such as:

- precursor m/z
- ion intensity
- charge state
- isotopic pattern

Think:

> **MS1 finds precursor ions.**

---

## 6.2 MS2

MS2 measures ions produced after fragmentation of a selected precursor.

MS2 provides sequence-related information because peptide backbone fragmentation generates predictable fragment-ion series.

Think:

> **MS2 helps identify the peptide.**

---

# 7. Precursor and Product Ions

The intact peptide ion selected for fragmentation is the:

> **precursor ion**

After fragmentation, the resulting ions are:

> **product ions** or **fragment ions**

Example:

```text
precursor peptide
      ↓
fragmentation
      ↓
b1, b2, b3...
y1, y2, y3...
```

---

# 8. Peptide Fragmentation

Common collision-based fragmentation methods include:

- CID — Collision-Induced Dissociation
- HCD — Higher-energy Collisional Dissociation

The conceptual mechanism is:

```text
selected peptide ion
       ↓
collisions / energy deposition
       ↓
peptide backbone cleavage
       ↓
fragment ions
```

HCD is especially common in modern Orbitrap-based proteomics.

---

# 9. b-Ions and y-Ions

Peptides are written from:

```text
N-terminus → C-terminus
```

For a peptide:

```text
A-G-L-K
```

## 9.1 b-ion series

b-ions retain the N-terminal side:

```text
b1 = A
b2 = A-G
b3 = A-G-L
```

Memory aid:

> **b = beginning**

---

## 9.2 y-ion series

y-ions retain the C-terminal side:

```text
y1 = K
y2 = L-K
y3 = G-L-K
```

---

## 9.3 Why neighboring fragment masses matter

Differences between neighboring fragment-ion masses can correspond to amino-acid residue masses.

Example:

```text
b1 = 100 Da
b2 = 171 Da
```

Difference:

\[
171 - 100 = 71 \text{ Da}
\]

A residue mass around 71 Da corresponds to alanine.

Thus, fragment-ion spacing can provide peptide sequence information.

---

# 10. Protein Digestion Versus Peptide Fragmentation

These must not be confused.

## Protein digestion

Occurs before MS analysis.

Example:

```text
Protein
 ↓ trypsin
Peptides
```

## Peptide fragmentation

Occurs inside the mass spectrometer.

Example:

```text
Peptide ion
 ↓ HCD/CID
Fragment ions
```

So:

> Trypsin breaks proteins into peptides.

> HCD/CID breaks peptide ions into fragment ions.

---

# 11. Trypsin

Trypsin is one of the most commonly used enzymes in bottom-up proteomics.

It generally cleaves after:

- lysine (K)
- arginine (R)

with exceptions depending on local sequence context.

Example:

```text
ABCDEKFGHIRLMN
```

may generate approximately:

```text
ABCDEK
FGHIR
LMN
```

Tryptic peptides are well suited to positive-mode ESI because their basic residues often support formation of positively charged ions.

---

# 12. From Spectrum to Peptide Identification

An MS2 spectrum contains fragment-ion m/z values and intensities, but not a peptide name.

A database search therefore compares the observed spectrum to theoretical spectra derived from candidate peptide sequences.

Simplified:

```text
Experimental MS2 spectrum
        ↓
candidate peptide sequences
        ↓
theoretical fragment ions
        ↓
spectrum comparison
        ↓
match score
```

---

# 13. FASTA Protein Databases

A FASTA file contains protein sequences.

Example:

```text
>sp|P04637|P53_HUMAN Cellular tumor antigen p53
MEEPQSDPSVEPPLSQETFSDLWKLLPEN...
```

The header provides protein metadata, while the sequence contains amino acids.

A search engine uses these protein sequences to generate candidate peptides.

The database should reflect the biology of the sample. For a human experiment, a human protein database is normally appropriate.

Using an unnecessarily broad database increases the search space and can reduce sensitivity.

---

# 14. In Silico Digestion

The real experiment physically digests proteins with an enzyme.

The search engine reproduces this step computationally.

Example:

```text
Protein FASTA sequence
        ↓
trypsin cleavage rules
        ↓
theoretical peptides
```

This process is called:

> **in silico digestion**

---

# 15. Peptide-Spectrum Match (PSM)

A **PSM** is a proposed match between:

```text
one experimental MS2 spectrum
+
one candidate peptide sequence
```

Example:

```text
Spectrum #1254
     ↓
candidate peptide = AGLFK
     ↓
PSM
```

Multiple spectra can identify the same peptide.

Therefore:

```text
5 spectra matching 2 peptide sequences
=
5 PSMs
2 unique peptides
```

PSM count and peptide count are not interchangeable.

---

# 16. Search Scores

Search engines assign scores describing how well experimental and theoretical spectra match.

Examples of search/scoring systems include:

- MSFragger scores
- SEQUEST XCorr
- Mascot scores
- Andromeda scores
- Comet scores

Different engines use different scoring models.

A stronger score generally indicates better spectral agreement, but score interpretation is search-engine specific.

A high score alone does not guarantee correctness because some matches can occur by chance.

---

# 17. Mass Tolerance

Search engines need to know how closely an observed mass must match a theoretical mass.

## 17.1 Precursor tolerance

Applied to the precursor-ion mass or m/z.

## 17.2 Fragment tolerance

Applied to MS2 fragment-ion matching.

High-resolution data often use narrow tolerances.

---

# 18. ppm Mass Error

Mass error is often expressed in parts per million:

\[
ppm\ error =
\frac{measured-theoretical}{theoretical}
\times 10^6
\]

Example:

```text
Theoretical m/z = 500.000
Measured m/z    = 500.002
Difference      = 0.002
```

\[
ppm =
\frac{0.002}{500}
\times10^6
= 4
\]

So the mass error is:

> **4 ppm**

Another example:

```text
Theoretical = 800.000
Measured    = 800.004
```

\[
\frac{0.004}{800}\times10^6 = 5\ ppm
\]

---

# 19. Missed Cleavages

Trypsin does not always cleave every expected site.

Example:

```text
AAAAKBBBBR
```

Expected digestion:

```text
AAAAK
BBBBR
```

If the K site is not cleaved:

```text
AAAAKBBBBR
```

contains one missed cleavage.

Search software must therefore be told how many missed cleavage sites are allowed.

A common search might allow:

```text
Missed cleavages = 2
```

Allowing too many missed cleavages unnecessarily expands the search space.

---

# 20. Fixed and Variable Modifications

## 20.1 Fixed modification

A fixed modification is assumed to be present at every specified residue.

Example:

```text
Carbamidomethylation of cysteine
```

Approximate mass shift:

\[
+57.0215\ Da
\]

This is commonly associated with cysteine alkylation using iodoacetamide.

---

## 20.2 Variable modification

A variable modification is optional.

The software considers both modified and unmodified forms.

Example:

```text
Methionine oxidation
```

Approximate mass shift:

\[
+15.9949\ Da
\]

Thus a methionine-containing peptide can be considered in both states.

---

## 20.3 Why too many variable modifications are problematic

Every variable modification increases the number of theoretical peptide possibilities.

This can cause:

- a much larger search space
- longer computation
- more ambiguous candidate matches
- increased statistical burden

Search parameters should therefore reflect the actual experiment rather than every conceivable modification.

---

# 21. Target-Decoy Searching

Incorrect peptide matches can occur by chance.

To estimate the frequency of false matches, database searches commonly use:

```text
Target sequences
+
Decoy sequences
```

Target sequences represent real candidate proteins.

Decoys are artificial sequences designed to provide an estimate of chance matching.

A strong match to a decoy is treated as evidence of the rate at which incorrect matches can pass the scoring threshold.

---

# 22. False Discovery Rate (FDR)

FDR estimates the proportion of false discoveries among accepted identifications.

Conceptually:

\[
FDR \approx
\frac{\text{estimated false identifications}}
{\text{accepted identifications}}
\]

A common threshold is:

```text
1% FDR
```

This means that, under the chosen estimation procedure, approximately 1% of the accepted identifications are expected to be false.

It does **not** mean that every individual identification has exactly a 99% probability of being correct.

---

# 23. PEP and q-Value

## Posterior Error Probability (PEP)

PEP concerns an individual identification.

Conceptually:

> estimated probability that a specific identification is incorrect

Smaller is better.

---

## q-value

A q-value connects an identification threshold to false-discovery control.

A q-value of approximately:

```text
≤ 0.01
```

is often associated with a 1% FDR threshold.

---

# 24. PSM, Peptide, and Protein Levels

These are separate levels of evidence.

```text
MS2 spectrum
    ↓
PSM
    ↓
Peptide
    ↓
Protein / protein group
```

Example:

```text
Spectrum 1 → PEPTIDE-A
Spectrum 2 → PEPTIDE-A
Spectrum 3 → PEPTIDE-B
```

This represents:

```text
3 PSMs
2 unique peptides
```

If both peptides originate from the same protein, they may support one protein identification.

---

# 25. Protein Inference

Peptides can map to more than one protein.

Example:

```text
PEPTIDE-X
   ↓
Protein A
Protein B
```

Such a peptide is a **shared peptide**.

A peptide found only in one candidate protein provides more specific evidence and is often described as a **unique peptide** in that inference context.

Because shared peptides can make exact protein assignment ambiguous, proteomics software may report:

> **protein groups**

rather than confidently claiming one individual protein.

---

# 26. Contaminants

Proteomics samples can contain proteins that are not part of the biological sample.

Common examples include:

- keratins
- trypsin
- laboratory contaminants

Search workflows often include known contaminant sequences so that these proteins can be recognized rather than interpreted as biological findings.

---

# 27. What FragPipe Is

FragPipe is a proteomics workflow platform that coordinates multiple software components.

It is best understood as a:

> **workflow manager and analysis environment**

rather than a single search algorithm.

Simplified:

```text
LC-MS/MS data
      ↓
FragPipe workflow
      ↓
identification
validation
protein inference
quantification
report generation
```

---

# 28. FragPipe Versus MSFragger

## FragPipe

FragPipe organizes the complete workflow.

It provides:

- graphical user interface
- workflow configuration
- input management
- database management
- search parameter setup
- validation
- quantification
- result generation

## MSFragger

MSFragger is the core database search engine commonly used inside FragPipe.

Its main job is:

```text
Experimental spectrum
      ↓
candidate peptide search
      ↓
PSM candidates
```

A useful distinction is:

```text
FragPipe = workflow manager
MSFragger = search engine
```

---

# 29. Important FragPipe Components

A FragPipe workflow may coordinate several tools.

Not every workflow uses all of them.

---

## 29.1 MSFragger

Main role:

> peptide identification / database searching

Input:

```text
MS/MS spectra
+
FASTA database
+
search parameters
```

Output:

```text
candidate peptide-spectrum matches
```

---

## 29.2 MSBooster

MSBooster can use predicted peptide properties to improve scoring.

These can include:

- predicted fragment-ion intensities
- retention time
- peptide detectability
- other peptide features depending on workflow

Conceptually:

```text
candidate PSM
    +
predicted peptide behavior
    ↓
improved discrimination
```

---

## 29.3 Percolator

Percolator rescoring helps separate likely correct and incorrect PSMs using multiple features.

Conceptually:

```text
candidate PSMs
     ↓
rescoring
     ↓
better ranking / discrimination
```

---

## 29.4 Philosopher

Philosopher supports important downstream processing such as:

- target-decoy processing
- FDR-controlled reporting
- peptide/protein reporting
- workflow-level validation tasks

The exact role depends on the selected workflow.

---

## 29.5 ProteinProphet

ProteinProphet helps combine peptide evidence into protein-level inference.

Conceptually:

```text
PSMs
 ↓
peptides
 ↓
ProteinProphet
 ↓
proteins / protein groups
```

---

## 29.6 IonQuant

IonQuant performs quantitative analysis, particularly label-free quantification.

It extracts peptide-ion abundance information that can be summarized into protein-level quantitative values.

This changes the question from:

> What peptide is this?

to:

> How much of this peptide/protein is present?

---

# 30. Label-Free Quantification (LFQ)

LFQ compares abundance across separately analyzed samples without chemical labeling.

Example experimental design:

```text
Control_1
Control_2
Control_3

Treatment_1
Treatment_2
Treatment_3
```

Quantitative output may look conceptually like:

| Protein | Control_1 | Control_2 | Control_3 | Treatment_1 | Treatment_2 | Treatment_3 |
|---|---:|---:|---:|---:|---:|---:|
| Protein A | 1.2e8 | 1.3e8 | 1.1e8 | 4.8e8 | 4.6e8 | 5.0e8 |
| Protein B | 8.4e7 | 8.1e7 | 8.6e7 | 7.9e7 | 8.2e7 | 8.0e7 |

These values can later be analyzed using Python or R.

---

# 31. Match Between Runs (MBR)

A peptide may be confidently identified by MS2 in one LC-MS run but not fragmented in another.

MBR can attempt to transfer identification between runs using properties such as:

```text
accurate m/z
+
retention time
```

Conceptually:

```text
Run 1:
peptide identified by MS2

Run 2:
matching MS1 feature
but no corresponding MS2 identification

        ↓
possible MBR transfer
```

MBR can reduce missing quantitative values, but it does not eliminate all missing-data problems and should not be treated as direct MS2 evidence.

---

# 32. DDA — Data-Dependent Acquisition

In DDA, the instrument performs an MS1 survey scan and then selects a subset of precursor ions for fragmentation.

Conceptually:

```text
MS1
 ↓
detect precursor ions
 ↓
select several precursors
 ↓
fragment
 ↓
MS2
```

For a first practical proteomics project, DDA is easier to learn because it maps directly onto the precursor-selection concepts introduced earlier.

---

# 33. DIA — Data-Independent Acquisition

In DIA, the instrument systematically fragments precursor ions across predefined m/z windows.

Example:

```text
400–425
425–450
450–475
475–500
...
```

Many ions can be fragmented together, producing more complex MS2 spectra.

DIA is extremely important in modern proteomics, but DDA is a better starting point for learning fundamental identification and quantification workflows.

---

# 34. Closed Search Versus Open Search

## Closed search

A closed search uses relatively narrow precursor tolerances and a defined set of expected modifications.

Example:

```text
Fixed:
Carbamidomethyl C

Variable:
Oxidation M
```

This is appropriate for conventional proteomics workflows.

---

## Open search

An open search allows a much broader precursor-mass difference.

This can reveal unexpected mass shifts.

Conceptually:

```text
observed peptide mass
-
expected peptide mass
=
delta mass
```

An open search is valuable for discovery but increases interpretation complexity.

For a first practical project, a standard closed search is preferable.

---

# 35. Typical FragPipe Inputs

A standard FragPipe analysis needs several types of information.

## 35.1 Mass spectrometry files

Common formats include:

```text
.raw
.mzML
.mgf
Bruker .d
```

For a Linux-based beginner project, **mzML** is a convenient open format.

---

## 35.2 FASTA database

Contains candidate protein sequences.

For a human experiment:

```text
Human reference proteome FASTA
```

The database may also include:

```text
contaminants
decoy sequences
```

---

## 35.3 Search parameters

Typical parameters include:

```text
enzyme
missed cleavages
precursor tolerance
fragment tolerance
fixed modifications
variable modifications
peptide length limits
peptide mass limits
FDR thresholds
quantification settings
```

These should be determined by the actual experimental design and instrument.

---

# 36. Typical Search Configuration Example

For a human tryptic digest analyzed using high-resolution LC-MS/MS:

```text
Database:
Human reference proteome
+ contaminants
+ decoys

Enzyme:
Trypsin

Missed cleavages:
2

Fixed modification:
Carbamidomethyl C

Variable modification:
Oxidation M

Precursor tolerance:
appropriate narrow high-resolution setting

Fragment tolerance:
appropriate to acquisition method

FDR:
1%
```

This is only an example. Parameters should not be copied blindly into unrelated experiments.

---

# 37. Simplified FragPipe Workflow

A modern identification/quantification workflow can be viewed conceptually as:

```text
LC-MS/MS data
      ↓
MSFragger
database search
      ↓
candidate PSMs
      ↓
MSBooster / rescoring support
      ↓
Percolator
      ↓
ProteinProphet
protein inference
      ↓
Philosopher
FDR-controlled reporting
      ↓
IonQuant
quantification
      ↓
result tables
      ↓
Python / R
      ↓
QC + statistics + biology
```

The exact components depend on the selected workflow.

---

# 38. Common FragPipe Output Levels

Typical result reports can include:

```text
psm.tsv
peptide.tsv
protein.tsv
ion.tsv
```

Exact filenames vary with workflow and FragPipe version.

---

## 38.1 PSM-level output

Contains evidence closest to individual MS2 spectra.

Possible columns include:

- spectrum identifier
- peptide sequence
- charge
- protein
- mass error
- modification
- score
- probability/q-value-related fields

---

## 38.2 Peptide-level output

Collapses evidence toward peptide sequences.

Example:

```text
3 PSMs
    ↓
1 identified peptide
```

---

## 38.3 Protein-level output

Combines peptide evidence into proteins or protein groups.

This is where the protein-inference problem becomes visible.

---

# 39. What FragPipe Does Not Do for You

FragPipe can identify and quantify peptides/proteins, but it does not automatically turn software output into a valid biological conclusion.

Important downstream steps still include:

```text
quality control
data cleaning
replicate assessment
missing-value assessment
normalization
transformation
statistical testing
multiple-testing correction
visualization
biological interpretation
```

These tasks will be performed mainly with Python/R in the practical phase.

---

# 40. Why Experimental Design Still Matters

Good software cannot rescue poor study design.

For example:

```text
1 control sample
1 treatment sample
```

may produce thousands of quantified proteins, but this does not provide strong biological replication.

Proteomics software generates measurements and statistical evidence. Biological conclusions still depend on sound experimental design.

---

# 41. Practical Data-Analysis Workflow After FragPipe

The planned downstream workflow is:

```text
FragPipe protein/peptide results
        ↓
Python / pandas
        ↓
inspect columns and dimensions
        ↓
quality control
        ↓
filter contaminants / decoys as appropriate
        ↓
assess missing values
        ↓
log2 transformation
        ↓
normalization where appropriate
        ↓
replicate correlation
        ↓
PCA
        ↓
statistical comparison
        ↓
multiple-testing correction
        ↓
volcano plot
        ↓
heatmap
        ↓
biological interpretation
```

---

# 42. Planned First Practical Project

The recommended first hands-on workflow is:

> **DDA label-free LC-MS/MS proteomics**

A suitable small public dataset would ideally contain:

```text
2 biological conditions
3 replicates per condition
small mzML files
known organism
clear experimental design
```

Planned pipeline:

```text
Public mzML dataset
        ↓
species-specific FASTA
        ↓
FragPipe LFQ workflow
        ↓
MSFragger identification
        ↓
FDR-controlled validation
        ↓
IonQuant quantification
        ↓
PSM / peptide / protein QC
        ↓
Python analysis
        ↓
figures
        ↓
biological interpretation
        ↓
GitHub documentation
```

---

# 43. GitHub Reproducibility Strategy

The repository should contain:

```text
README.md
environment.yml
workflow files
sample metadata
analysis notebooks
scripts
figures
documentation
small processed results where appropriate
```

Large raw mass-spectrometry files should normally not be stored directly in GitHub.

Instead:

```text
data/raw/README.md
```

should document:

- dataset repository
- accession
- exact files used
- download source
- download date
- file format

---

# 44. FragPipe Documentation to Record

For reproducibility, record:

```text
FragPipe version
MSFragger version
workflow name
FASTA source
FASTA release/version
FASTA download date
organism
enzyme
missed cleavages
fixed modifications
variable modifications
precursor tolerance
fragment tolerance
FDR thresholds
quantification settings
MBR enabled/disabled
input-file list
sample annotations
```

Writing only:

> "The data were analyzed using FragPipe"

is not sufficient for reproducible computational proteomics.

---

# 45. Core Concepts to Remember

The essential conceptual chain is:

```text
Protein
 ↓
Trypsin
 ↓
Peptide
 ↓
LC
 ↓
ESI
 ↓
MS1 precursor
 ↓
Quadrupole isolation
 ↓
HCD/CID
 ↓
MS2 fragments
 ↓
b/y ions
 ↓
database search
 ↓
PSM
 ↓
FDR validation
 ↓
Peptide
 ↓
Protein / protein group
 ↓
Quantification
 ↓
Statistical analysis
 ↓
Biological interpretation
```

And for FragPipe specifically:

```text
FragPipe
   │
   ├── identification
   │      ↓
   │   MSFragger
   │      ↓
   │     PSMs
   │      ↓
   │   validation / FDR
   │      ↓
   │   peptides
   │      ↓
   │   proteins
   │
   └── quantification
          ↓
       IonQuant
          ↓
       intensity matrix
          ↓
       Python / R
```

---

# 46. Theory Completed Before Practical Work

The core theoretical foundation covered so far is:

### Day 1
Mass-spectrometry and proteomics fundamentals.

### Day 2
ESI, MALDI, quadrupole, TOF, Orbitrap, charge states, mass accuracy, and resolution.

### Day 3
MS1/MS2, precursor ions, peptide fragmentation, HCD/CID, b-ions, and y-ions.

### Day 4
Peptide-spectrum matching, in silico digestion, search scores, target-decoy searching, FDR, and protein inference.

### Day 5
FASTA databases, search parameters, missed cleavages, modifications, mass tolerances, and interpretation of search-engine results.

### Practical bridge
FragPipe, MSFragger, validation tools, IonQuant, DDA/LFQ, workflow files, and reproducible analysis design.

This theoretical foundation is sufficient to begin the first guided FragPipe analysis while learning more advanced theory alongside real data.

---

## Next Step

Proceed to:

> **Practical Day 1 — FragPipe installation, first launch, configuration check, and selection of a small public DDA dataset.**
