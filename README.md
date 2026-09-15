# Riyadh Grocery Demand — A Backtested Forecasting Report

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Abdulwahab-Alnassar/riyadh-grocery-forecasting-capstone/blob/main/Riyadh_Grocery_Forecasting_Capstone.ipynb)

**Author:** Abdulwahab Mohammed Ali Alnassar  
**Training programme:** SDAIA Academy — Time Series Forecasting for AI Systems  
**Cohort dates:** Not supplied; enter the actual start and end dates before submission.  
**Project description:** A reproducible, single-notebook comparison of seasonal-naive,
Holt-Winters, and LightGBM forecasts for daily grocery demand, with chronological
model selection, rolling-origin calibration, and a separate final test.

## Run in Google Colab

1. Click **Open in Colab** above to open this repository's notebook.
2. Select a **CPU** runtime and choose **Runtime → Run all**.
3. The first code cell installs the required packages. The data snapshot is
   embedded in the notebook: no Drive mount, API key, or additional upload is needed.
4. The final cells create `capstone_results.zip`; set `DOWNLOAD_RESULTS = True`
   in the optional download cell if you want Colab to download it.
5. Alternatively, download the `.ipynb` and use **File → Upload notebook** in Colab.

## Dataset and scope

The project uses only **Riyadh / Grocery** from the course's
[`data/retail_demand.csv`](https://github.com/MohammadYusif/time-series-forecasting-ai-systems/blob/0acb110628250c5efec9dd2b3d5c0b46401ebdee/data/retail_demand.csv).
It contains 1,096 daily observations from 2023-01-01 to 2025-12-31.
The course data are **synthetic**, including their trend, weekly/yearly patterns,
and demand shocks. They do not describe an actual Riyadh retailer.
This dense daily series provides enough history for both statistical and tree models.

Only observed demand and calendar information are used. No hidden generator
variables, future observations, or assumed promotion schedules enter the forecasts.
The final forecast is for the 28 days following the dataset's end, not a live
forecast as of the date you open the notebook.

## Evaluation design

- 28-day operational horizon; weekly seasonal period of 7.
- Five expanding-window folds for model comparison, with 760 days in the first fit.
- Six later, disjoint forecast windows for interval calibration.
- The final 28 observations are reserved for an untouched final evaluation.
- MAE, RMSE, WAPE, and fold-specific seasonal MASE are reported.
- Nominal 90% intervals are evaluated using both coverage and mean width.
- Model selection is locked before calibration and final evaluation.

All seven capstone sections, their code, interpretation, and captured outputs are
inside the **one** `.ipynb` file. The supporting documents are for the separate
GitHub documentation requirement; they are not additional analysis notebooks.

## Documentation

- [Technical documentation](docs/TECHNICAL_DOCUMENTATION.md)
- [Rubric mapping](docs/RUBRIC_CHECKLIST.md)
- [Arabic running and presentation guide](docs/ARABIC_GUIDE.md)
- [Submission and GitHub steps](docs/SUBMISSION.md)
- [Dataset provenance](data/PROVENANCE.md)

## Programme and acknowledgements

Built against the [course capstone brief](https://mohammadyusif.github.io/time-series-forecasting-ai-systems/capstone.html).
Dataset and course materials: Mohammad Yusif's course repository, pinned to the
commit cited above. The notebook's implementation and evaluation design are
project-specific; the course's published benchmark scores are not reused.

[SDAIA Academy on GitHub](https://github.com/SDAIAAcademy).
The course labels its scoring weights and pass mark as a draft rubric. This project
maps to the seven published sections; no grade or approval is asserted.

## Reproducibility and Git

The delivered notebook includes actual saved outputs. An actual local IPython execution
report and package versions accompany the completed project. Outputs are reproducible
up to numerical-library and runtime variation; timing is specific to the machine.

Use meaningful commits for subsequent work. Never commit credentials, `.env`,
temporary outputs, or private data. Keep subsequent changes in meaningful commits.
The executed analysis and supporting files are published in this repository.


## Actual saved results

| Model | Selection WAPE | Final-test WAPE | Final coverage (nominal 90%) | Mean interval width |
|---|---:|---:|---:|---:|
| Seasonal Naive | 26.13% | 9.21% | 96.43% | 344.00 units |
| Holt-Winters | 28.11% | 8.66% | 92.86% | 387.13 units |
| LightGBM | 28.03% | 6.91% | 92.86% | 351.93 units |

The prespecified selection procedure chose **Seasonal Naive**. LightGBM won the
later December holdout, but the model was not reselected using the test. The
notebook discusses this difference and the large errors around validation fold 3.
These are synthetic-data results, not a production claim or a universal model ranking.

- Open `Forecasting_Report.html` for a standalone report without code.
- See `results/` for CSV tables, JSON configuration, and PNG figures.
- See [verification details](docs/VERIFICATION.md) for actual execution scope.
