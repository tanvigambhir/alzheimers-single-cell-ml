# Predicting Alzheimer's Disease States from Single-Cell RNA-seq Data

## Project Overview
This project develops a machine learning pipeline to classify individual brain cells as either Alzheimer's Disease (AD) or Healthy Control based on their transcriptomic profiles. By applying a Random Forest classifier to single-cell RNA-sequencing data, the model not only achieves high classification accuracy but also extracts the underlying gene signatures driving the disease state.

## Biological Background
Alzheimer's disease is characterized by complex, cell-type-specific transcriptional changes. Analyzing bulk tissue often masks these intricate differences. This pipeline leverages single-cell resolution to identify the specific molecular markers of neurodegeneration, moving beyond simple classification to actual biological target discovery.

## Dataset & Preprocessing
* **Data Source:** Mathys et al., 2019 (GEO Accession: GSE138852).
* **Composition:** 13,214 cells across various types (Oligodendrocytes, Astrocytes, Microglia, Neurons, etc.).
* **Quality Control (Scanpy):** * Filtered out computational doublets and unidentified cell types.
  * Removed rare genes (genes expressed in < 10 cells) to reduce technical noise.
* **Normalization & Scaling:** Total count normalization (target sum 1e4), log1p transformation, and scaling applied to the top 2,000 highly variable genes.

## Machine Learning Modeling
* **Algorithm:** Random Forest Classifier (100 estimators).
* **Validation:** 80/20 Train-Test split.
* **Performance:** Achieved **94.91% accuracy** on unseen test cells, demonstrating that AD cells possess a fundamentally distinct and predictable transcriptomic state compared to healthy controls.

## Biological Interpretation & Feature Importance
The true value of this model lies in identifying which genes the algorithm relied on to classify Alzheimer's cells. The top predictive features closely align with documented neurodegenerative pathology:

1. **LINGO1:** A known negative regulator of myelination and neuronal survival. Its emergence as the top feature highlights severe myelin breakdown and points to a critical therapeutic target for neural repair.
2. **NEAT1 & LINC00486:** Long non-coding RNAs strongly associated with cellular stress responses and paraspeckle formation, heavily upregulated in neurodegenerative states.
3. **HSPA1A:** A major heat shock protein. Its high importance score indicates the cellular stress response attempting to clear misfolded protein aggregates (like amyloid-beta and tau).
4. **XIST:** Suggests potential sex-specific transcriptional divergence in the disease pathology.

## Tech Stack
* `Python`, `Scanpy`, `AnnData` (Biological preprocessing and UMAP dimensionality reduction)
* `Scikit-Learn` (Machine learning, Random Forest modeling, Feature extraction)
* `Pandas`, `Matplotlib`, `Seaborn` (Data manipulation and visualization)

---
*Created by Tanvi Gambhir as a demonstration of applying computational pipelines to complex neurobiological data.*
