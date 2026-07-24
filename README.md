# DVC Demo: Telecom Customer Churn Prediction

This repository demonstrates how to use **Git** and **DVC (Data Version Control)** together in a Machine Learning project.  
The project uses a **Telecom Customer Churn dataset** to show how data versioning works in real-world ML workflows.

## Overview

In Machine Learning projects, both **code** and **data** change over time.  
Git is excellent for version-controlling source code, but it is not suitable for storing large datasets.  
That is where DVC comes in.

DVC helps us track dataset versions while storing the actual data in remote storage such as **AWS S3**.  
Git stores the code and `.dvc` pointer files, while DVC manages the real dataset.

## Why DVC?

DVC solves common problems in ML projects such as:

- Tracking multiple versions of the same dataset.
- Reproducing experiments reliably.
- Keeping large files out of Git.
- Helping teams collaborate on shared data.
- Restoring older dataset versions when needed.

## How It Works

1. The dataset is added using `dvc add`.
2. DVC creates a small `.dvc` file containing metadata and a unique hash.
3. The `.dvc` file is committed to Git.
4. The actual dataset is uploaded to remote storage using `dvc push`.
5. Other team members can clone the repo and use `dvc pull` to download the exact dataset version.

## Project Workflow

- **Git** stores:
  - Python scripts
  - configuration files
  - notebooks
  - `.dvc` metadata files

- **DVC** stores:
  - actual datasets
  - dataset versions
  - remote data pointers

## Dataset

This project uses a **Telecom Customer Churn** dataset.  
The goal is to demonstrate how dataset versioning works in a machine learning workflow.

## Example Commands

```bash
git init
dvc init
dvc add data/telco_churn.csv
dvc remote add -d churn-remote s3://your-bucket-name
dvc push
git add .
git commit -m "Track dataset with DVC"
```

To get the data on another machine:

```bash
git clone <repo-url>
dvc pull
```

## Project Structure

```bash
dvc-demo/
├── data/
│   ├── telco_churn.csv
│   └── telco_churn.csv.dvc
├── .dvc/
├── .dvcignore
├── .gitignore
├── requirements.txt
├── src/
│   └── train.py
└── README.md
```

## Learning Outcome

By the end of this project, you will understand:

- Why Git alone is not enough for ML datasets.
- How DVC helps version large data files.
- How to store datasets in AWS S3.
- How to reproduce the same dataset version across teams.
- How Git and DVC work together in a practical ML project.

## Final Takeaway

**Git stores the code and the pointers. DVC stores the actual data in S3.**  
This makes ML projects more reproducible, collaborative, and easy to manage.

## Next Step

In the next section, we will implement this workflow practically and see DVC in action.
