# SpaceX Landing Prediction | IBM Applied Data Science Capstone

This repository contains my completed capstone for the IBM Data Science Professional Certificate. It follows the IBM course workflow for collecting and analysing Falcon 9 launch data, exploring launch outcomes, creating interactive visualisations and comparing classification models. The original lab author credits and IBM notices remain in the notebooks.

## Workflow and files

1. `1-Data Collection.ipynb`: launch data collection through the SpaceX API and the course snapshot.
2. `2-Data Collection using Web Scraping.ipynb`: extraction of launch records from the course web page snapshot.
3. `3-Data Wrangling.ipynb`: preprocessing and landing outcome labels.
4. `4-EDA with SQL.ipynb`: SQL exploration of launch records.
5. `5-EDA with Visualization.ipynb`: visual analysis and feature encoding.
6. `6-Interactive Visual Analytics with Folium.ipynb`: maps and geographical exploration.
7. `7-Interactive Visual Analytics with Dashboard.py`: Dash application with launch site selection and payload filtering.
8. `8-Machine learning prediction.ipynb`: classification pipelines, hyperparameter tuning, model comparison and saved evaluation outputs.
9. `Applied Data Science Capstone presentaion.pdf`: presentation of the project, with evaluation pages updated to match the corrected notebook.

## Setup

Use Python 3.11 or newer. Open a terminal in the repository root, which contains `requirements.txt` and the `data` folder.

Create an environment:

```bash
python -m venv .venv
```

On Windows PowerShell, activate it with:

```powershell
.\.venv\Scripts\Activate.ps1
```

On macOS or Linux, activate it with:

```bash
source .venv/bin/activate
```

Install the dependencies:

```bash
python -m pip install -r requirements.txt
```

The core versions in `requirements.txt` match the verified dashboard and classification run. The original collection, scraping, SQL and map notebooks use additional packages listed in the same file and still depend on their original external course or SpaceX sources.

## Run the dashboard

```bash
python "7-Interactive Visual Analytics with Dashboard.py"
```

Open `http://127.0.0.1:8050` in your browser. The application reads `data/spacex_launch_dash.csv` relative to the script's location, so there is no personal desktop path to edit. The bundled dashboard snapshot contains 56 launch records. Keep the `data` folder beside the script, including when copying the application to another location.

## Rerun the classification notebook

Start Jupyter from the repository root:

```bash
python -m notebook
```

Open `8-Machine learning prediction.ipynb`, then restart the kernel and run all cells in order. This notebook reads the bundled `data/dataset_part_2.csv` and `data/dataset_part_3.csv` snapshots. It does not download model input data at runtime and can run without internet access after installing the dependencies. It writes the current results to `data/model_evaluation.csv` and `data/model_evaluation.json`.

## Evaluation correction

The supplied IBM lab sequence placed `StandardScaler.fit_transform(X)` before `train_test_split`. This portfolio revision splits the unscaled feature table first and puts `StandardScaler` inside each model's `Pipeline`. `GridSearchCV` therefore fits scaling separately within each training fold. Hyperparameters and model selection use the training set's cross validation scores, while the held out test set is used for reporting.

The split remains `test_size=0.2, random_state=2`, giving 72 training examples and 18 test examples. The decision tree also has a fixed random seed. The original duplicate SVM kernel option was removed without changing the unique kernels searched. All four classifiers use 10 fold cross validation and accuracy scoring.

## Current results

The corrected notebook was fully rerun with the bundled course data. Models are listed in descending cross validation score:

1. Decision tree: cross validation accuracy **0.8768**; test accuracy **0.8333**.
2. KNN: cross validation accuracy **0.8482**; test accuracy **0.8333**.
3. Logistic regression: cross validation accuracy **0.8464**; test accuracy **0.8333**.
4. SVM: cross validation accuracy **0.8214**; test accuracy **0.8333**.

The decision tree is selected using cross validation. Each model correctly predicts 15 of the 18 test examples in this run. This is a small educational comparison using a fixed, already preprocessed course dataset; it does not establish future launch forecasting accuracy or a validated launch cost model. The correction addresses scaling in the classification notebook rather than reworking the upstream preprocessing performed in the course dataset. Library versions or changing the split can also change the scores.

Presentation pages 10, 27, 28, 30 and 32 were updated to reflect the corrected method, scores and interpretation. The confusion matrices remain consistent with the rerun.

## Data sources and attribution

The bundled CSV files are snapshots downloaded from the IBM course assets. The dashboard dataset differs from the 90 example classification dataset and is used only for the dashboard.

1. Classification outcomes: https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/datasets/dataset_part_2.csv
2. Classification features: https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/datasets/dataset_part_3.csv
3. Dashboard records: https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/datasets/spacex_launch_dash.csv
4. Evaluation guidance: https://scikit-learn.org/stable/common_pitfalls.html#data-leakage

This is a course capstone with subsequent portfolio improvements. The original IBM templates, dataset attribution and author notices have been retained.
