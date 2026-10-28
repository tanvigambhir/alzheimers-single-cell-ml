# Alzheimer's Disease Cell-State Classification from Single-Nucleus RNA-seq

A machine learning pipeline that classifies individual brain nuclei as Alzheimer's disease (AD) or healthy control from their gene expression, then examines which genes the model relied on most.

## Dataset

- **Source:** Grubman et al., 2019, *Nature Neuroscience*: "A single-cell atlas of entorhinal cortex from individuals with Alzheimer's disease reveals cell-type-specific gene expression regulation." GEO accession [GSE138852](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE138852).
- **Tissue:** Human entorhinal cortex, one of the earliest regions affected by Alzheimer's pathology.
- **Assay:** Single-nucleus RNA-seq (snRNA-seq), 10x Genomics.
- **Size:** 13,214 nuclei from 12 individuals (AD and controls), including oligodendrocytes, astrocytes, microglia, neurons, and other cell types.

## Pipeline

All analysis is in `neuro_ml_alzheimers_analysis.ipynb`.

1. **Quality control (Scanpy):** Removed computational doublets and unidentified cell types, and filtered out genes expressed in fewer than 10 nuclei.
2. **Normalization:** Total-count normalization to 10,000 counts per nucleus, followed by log1p transformation.
3. **Feature selection:** Retained the top 2,000 highly variable genes and scaled expression.
4. **Classification (scikit-learn):** Random Forest classifier with 100 trees, trained on an 80/20 train/test split of nuclei.
5. **Interpretation:** Ranked genes by Random Forest feature importance.

## Results

- **Accuracy:** 94.91% on held-out nuclei (see Limitations).
- **Top predictive genes:**
  1. **LINGO1:** a negative regulator of myelination and axon regeneration
  2. **NEAT1** and **LINC00486:** long non-coding RNAs; NEAT1 is a stress-responsive RNA that scaffolds nuclear paraspeckles
  3. **HSPA1A:** an HSP70-family chaperone involved in protein folding and the cellular stress response
  4. **XIST:** an X-inactivation lncRNA expressed only in female cells (see Limitations)

These genes are predictive features, not validated disease mechanisms.

## Limitations

- **Cell-level split:** The train/test split was performed on nuclei, not donors. With only 12 individuals, nuclei from the same donor appear in both training and test sets, so the model can learn donor-specific signals. The reported accuracy is therefore likely an overestimate.
- **Sex as a confound:** XIST, a female-specific gene, ranked among the top features. This suggests the model partly learned donor sex rather than disease state, which can occur if sex is unevenly distributed between AD and control groups.
- **Library confounding:** In this dataset, each 10x library pooled two individuals with the same diagnosis, so technical batch effects are aligned with disease status and may contribute to classification performance.
- **Single metric:** Only accuracy is reported. Class balance, precision/recall, and per-cell-type performance were not evaluated.

## Next Steps

- Evaluate with a **donor-level split** (e.g., leave-one-donor-out cross-validation) so no individual appears in both training and test sets.
- Exclude sex-linked genes (e.g., XIST, Y-chromosome genes) and compare performance.
- Report balanced accuracy, precision/recall, and per-cell-type results.
- Train separate classifiers per cell type to identify cell-type-specific disease signatures.

## Tech Stack

Python · Scanpy · AnnData · scikit-learn · Pandas · Matplotlib · Seaborn
