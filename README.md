# Social Homophily and Post-Disaster Mobility after the Marshall Fire

This repository contains the analysis workflow for studying how social homophily relates to evacuation destination choice, long-term displacement, and return behavior after the Marshall Fire in Colorado.

The project is based on longitudinal anonymized mobility data and social connectedness measures, and focuses on three main questions:

1. What social, geographic, and behavioral factors predict evacuation behavior and evacuation distance?
2. Do evacuees choose destinations that are more socially similar or socially connected than expected under a distance-based null model?
3. Is the social homophily of evacuation destinations associated with long-term return and displacement outcomes?

The accompanying paper frames the project around the Marshall Fire in Colorado and shows that pre-disaster mobility behavior, social similarity, and social connectedness are important for explaining evacuation destination choice and longer-term recovery trajectories. The paper also notes that the underlying Cuebiq mobility data are not publicly shareable, while code from aggregated data can be made public.

## Data access and confidentiality

**Raw mobility data are not included in this repository.**

This repo is structured for a public or semi-public release **without restricted data**. The paper states that the mobility data were obtained through Cuebiq/Spectus under restricted access and are therefore not publicly available. Only non-sensitive derived outputs, code, documentation, and optionally small metadata tables or schemas should be committed.


## Repository structure

```text
social-homophily-marshall-fire/
├── README.md
├── .gitignore
├── requirements.txt
├── environment.yml
├── LICENSE.md
├── notebooks/
│   ├── 01_evacuation_distance_analysis.ipynb
│   ├── 02_social_homophily_computation.ipynb
│   ├── 03_evacuation_regression_analysis.ipynb
│   ├── 04_displacement_return_analysis.ipynb
│   ├── 05a_null_model_simulation_no_population.ipynb
│   ├── 05b_null_model_simulation_final.ipynb
│   ├── 05c_null_model_simulation_trial.ipynb
│   ├── 05d_null_model_simulation_supplementary.ipynb
│   └── 06_simulation_regression_analysis.ipynb
├── src/
│   └── social_homophily/
│       ├── __init__.py
│       ├── paths.py
│       └── utils.py
├── data/
│   ├── processed/
│   └── README.md
├── outputs/
│   ├── figures/
│   └── tables/
├── docs/
│   └── NOTEBOOK_GUIDE.md
└── config/
    └── paths.example.env
```

## Notebook workflow

The uploaded notebooks already form a clear analysis pipeline:

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

This is the approximate dependency chain inferred from the notebooks:

- `evacuees.csv` -> `evacuess_for_homophily_final.csv`
- `evacuess_for_homophily_final.csv` -> `evacuees_for_regression_final.csv`
- `evacuees_for_regression_final.csv` + `colorado_SCI_ZIP_CBG.csv` -> `evacuees_newSCI_simulation.csv`
- `evacuees_for_regression_final.csv` + `displaced_for_regression_final.csv` -> displacement/return analysis outputs
- `evacuees_newSCI_simulation.csv` + pairwise origin-destination tables -> simulated destinations and actual-vs-simulated comparison tables

Because the data are restricted, the public version of the repo should treat these files as **expected inputs** rather than committed data assets unless they are safe, aggregated, and explicitly shareable.

## Suggested cleanup before pushing to GitHub

1. Clear notebook outputs.
2. Replace absolute local paths with relative paths or environment variables.
3. Move repeated helper code into `src/social_homophily/`.
4. Add a short markdown intro cell at the top of each notebook:
   - purpose
   - required inputs
   - generated outputs
   - expected runtime
5. Rename inconsistent files where possible. Example: `evacuess_for_regs.csv` should become something like `evacuees_for_simulation.csv`.
6. Keep only the final simulation notebook in the main workflow and mark trial notebooks as exploratory.

## Reproducibility notes

The paper states that the analysis was conducted in Python and that the code for reproducing the main results from aggregated data is intended to be public. In practice, complete reproduction will depend on restricted mobility data access and on derived intermediate datasets that may need to be regenerated internally.

For a public-facing repository, the cleanest approach is:

- document the full pipeline honestly
- provide notebook logic and reusable code
- provide data dictionaries and expected schemas
- include only safe derived outputs
- explain which parts cannot be rerun without restricted access

## Citation

If you use this repository, cite the associated paper and include the project title, authors, and repository URL once finalized.

## Status

This repository template was reconstructed from the uploaded notebooks and accompanying manuscript. It is a strong starting point, but it still needs one final pass of file renaming, path cleanup, and notebook annotation before public release.
