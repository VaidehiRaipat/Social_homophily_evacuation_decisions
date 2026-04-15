# Social Homophily and Post-Disaster Mobility after the Marshall Fire

This repository contains the analysis workflow for studying how social homophily relates to evacuation destination choice, long-term displacement, and return behavior after the Marshall Fire in Colorado.

The project is based on longitudinal anonymized mobility data and social connectedness measures, and focuses on three main questions:

1. What social, geographic, and behavioral factors predict evacuation behavior and evacuation distance?
2. Do evacuees choose destinations that are more socially similar or socially connected than expected under a distance-based null model?
3. Is the social homophily of evacuation destinations associated with long-term return and displacement outcomes?

The accompanying paper (https://arxiv.org/abs/2507.18270) frames the project around the Marshall Fire in Colorado and shows that pre-disaster mobility behavior, social similarity, and social connectedness are important for explaining evacuation destination choice and longer-term recovery trajectories. The paper also notes that the underlying Cuebiq mobility data are not publicly shareable, while code from aggregated data can be made public.

## Data access and confidentiality

**Raw mobility data are not included in this repository.**

This repo is structured for a public or semi-public release **without restricted data**. The mobility data were obtained through Cuebiq/Spectus under restricted access and are therefore not publicly available. 
Only non-sensitive derived outputs, code, documentation, and optionally small metadata tables or schemas should be committed.


## Repository structure

```text
social-homophily-marshall-fire/
├── Processing notebooks/
│   ├── 00_Determining_Evacuees.ipynb
├── Results notebooks/
│   ├── 01_evacuation_distance_analysis.ipynb
│   ├── 02_social_homophily_computation.ipynb
│   ├── 03_evacuation_regression_analysis.ipynb
│   ├── 04_displacement_return_analysis.ipynb
│   ├── 05a_null_model_simulation_no_population.ipynb
│   ├── 05b_null_model_simulation_final.ipynb
│   ├── 05c_null_model_simulation_trial.ipynb
│   ├── 05d_null_model_simulation_supplementary.ipynb
│   └── 06_simulation_regression_analysis.ipynb
│        ├── outputs/
│            ├── figures/
│            └── tables/
│
├── data/
    ├── Census
    └── Social Connectedness Index
```

## Notebook workflow

### Processing notebooks

0. **00_Determining_Evacuees.ipynb**
   Estimates user home locations from pre-disaster nighttime stay patterns at the census block group level, using all threshold definitions considered in the main and supplementary analyses. Based on these inferred home locations, it identifies evacuees and non-evacuees during the disaster period, constructs evacuation motifs from post-disaster nighttime location trajectories, and performs robustness checks to assess the sensitivity of these classifications to alternative threshold choices.
   
### Results notebooks

1. **01_evacuation_distance_analysis.ipynb**  
   Computes evacuation distances and prepares an evacuee-level file for downstream homophily analysis.

2. **02_social_homophily_computation.ipynb**  
   Computes social similarity / homophily measures between origin and destination locations and prepares the regression-ready dataset.

3. **03_evacuation_regression_analysis.ipynb**  
   Runs evacuation regressions and produces a simulation-ready enriched evacuee table.

4. **04_displacement_return_analysis.ipynb**  
   Examines long-term return and displacement outcomes.

5. **05a–05d_null_model_simulation_*.ipynb**  
   Builds and tests alternative null-model simulation pipelines for destination choice.

6. **06_simulation_regression_analysis.ipynb**  
   Compares actual versus simulated outcomes and runs post-simulation regressions.

## Known input/output chain from the notebooks

This is the dependency chain of datasetd created from the commited notebooks:

- `evacuees.csv` -> `evacuess_for_homophily_final.csv`
- `evacuess_for_homophily_final.csv` -> `evacuees_for_regression_final.csv`
- `evacuees_for_regression_final.csv` + `colorado_SCI_ZIP_CBG.csv` -> `evacuees_newSCI_simulation.csv`
- `evacuees_for_regression_final.csv` + `displaced_for_regression_final.csv` -> displacement/return analysis outputs
- `evacuees_newSCI_simulation.csv` + pairwise origin-destination tables -> simulated destinations and actual-vs-simulated comparison tables

Because the data are restricted, the public version of the repo should treat these files as **expected inputs** rather than committed data assets unless they are safe, aggregated, and explicitly shareable.

## Reproducibility notes

This analysis was conducted in Python and that the code for reproducing the main results from aggregated data is intended to be public. In practice, complete reproduction will depend on restricted mobility data access and on derived intermediate datasets that may need to be regenerated internally.

## Requirements

pandas
numpy
geopandas
shapely
geopy
scipy
scikit-learn
statsmodels
matplotlib
seaborn
jupyter
notebook
stargazer

