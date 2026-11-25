# Delineating Neighborhoods in Brooklyn

Official neighborhood boundaries are drawn by convention, not by who lives where or what was built there. If you let the data draw them instead, what neighborhoods emerge?

This project groups Brooklyn's census tracts into neighborhoods by resident demographics and housing stock. It compares ordinary clustering with **regionalization**, a spatially constrained clustering where every neighborhood must be contiguous.

Sean Andrew Chen and Lina Moe · Final project for *Applied Multivariate Methods*, Rutgers Bloustein School, Fall 2022. Full paper: [`SeanChen_LinaMoe_AMM_FinalPaper.pdf`](SeanChen_LinaMoe_AMM_FinalPaper.pdf).

<p align="center">
  <img src="images/maps_kmeans.jpg" width="32%" alt="K-means clusters">
  <img src="images/maps_azp.jpg" width="32%" alt="AZP regionalization">
  <img src="images/maps_maxp.jpg" width="32%" alt="Max-p regionalization">
</p>
<p align="center"><em>The same tracts grouped three ways: k-means (unconstrained), AZP and max-p (contiguous regions). Rows use kernel PCA, PCA and standardized inputs.</em></p>

## Data

- **Source:** American Community Survey 2019 5-year estimates, via the Census API.
- **Coverage:** 752 populated Brooklyn census tracts, 85 variables from 13 tables.
- **Variables:**
  - race, age, citizenship, nativity, education
  - income, rent, poverty, tenure
  - building typology and housing age
- **Geography:** TIGER/Line 2019 tract boundaries.

## Methods

- **Dimension reduction:** standardized variables, PCA and kernel PCA.
- **Clustering:** k-means; agglomerative hierarchical (Ward).
- **Regionalization**, with rook contiguity weights:
  - Automatic Zoning Procedure (AZP)
  - max-p regions
  - spatial agglomerative hierarchical clustering (Ward)
  - SKATER
- **Choosing k:** silhouette, Calinski-Harabasz and Davies-Bouldin scores.
- **Compactness:** isoperimetric quotient.

## Findings

- **Fit favors few, unconstrained clusters.** Most fit metrics pointed to two or three clusters; k-means with three clusters fit best. Davies-Bouldin was the exception and often suggested 30 or more.
- **Contiguity costs fit but buys compactness.** Every regionalization method fit worse than unconstrained clustering, but produced far more compact neighborhoods. Spatial agglomerative clustering at 30 regions was the most compact, and SKATER with 13 regions also ranked well. AZP tended to produce elongated regions.

## Files

- [`SeanChen_LinaMoe_AMM_FinalProject_JupyterNotebook.ipynb`](SeanChen_LinaMoe_AMM_FinalProject_JupyterNotebook.ipynb): the full analysis. Outputs are cleared to keep the file small; every figure is in the paper.
- `images/`: selected maps.

Running the notebook needs a free [Census API key](https://api.census.gov/data/key_signup.html) saved as `census_api_key.txt`; that file is not in the repo.

## Stack

Python · pandas · GeoPandas · scikit-learn · PySAL (libpysal, spopt) · SciPy · mapclassify · seaborn
