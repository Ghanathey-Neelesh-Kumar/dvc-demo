# DVC Demo — Telecom Customer Churn Dataset

This repository is the lightweight hands-on companion for the **DVC data-versioning material** in MLOps Zero to Hero.

It demonstrates how Git and DVC work together:

- Git versions the source code and small DVC pointer files.
- DVC tracks the exact contents of the dataset.
- An S3 remote stores the actual dataset objects outside Git.

> **Scope:** This is a DVC demonstration, not a model-training project. The `src/train.py` file is intentionally a placeholder and does not train a machine-learning model. The focus is on dataset tracking and reproducibility.

## Clone the repository

```bash
git clone https://github.com/Ghanathey-Neelesh-Kumar/dvc-demo.git
cd dvc-demo
```

## Prerequisites

- Git
- Python 3.10+
- DVC with S3 support
- AWS CLI, if you are using Amazon S3

Install DVC:

```bash
python -m pip install "dvc[s3]"
```

Verify:

```bash
git --version
dvc version
aws --version
```

## Understand the repository

```text
dvc-demo/
├── .dvc/
│   └── config
├── data/
│   └── telco_churn.csv.dvc
├── src/
│   └── train.py
├── .dvcignore
├── .gitignore
├── requirements.txt
└── README.md
```

The file `data/telco_churn.csv.dvc` is a small pointer file. It records the hash and path of the dataset; it is not the dataset itself.

The actual data is stored in a DVC remote. GitHub does not contain the original CSV, and students should not assume they can access the instructor's AWS account.

## Configure your own DVC remote

The repository is already initialised as a DVC project and contains a default remote named `churn-remote`. Do not run `dvc init` again.

Create an S3 bucket that you own, then update the existing remote:

```bash
dvc remote modify churn-remote url s3://<YOUR_DVC_BUCKET>/dvc-demo
dvc remote modify churn-remote region <AWS_REGION>
```

DVC can use the AWS credentials configured through the AWS CLI. Verify the identity before accessing S3:

```bash
aws sts get-caller-identity
```

Do not place access keys in the repository. If you need a named AWS CLI profile, configure it locally rather than committing credentials.

## Track a dataset

Place a CSV dataset at:

```text
data/telco_churn.csv
```

If you are using your own dataset, make sure the file exists at that path. Then track it:

```bash
dvc add data/telco_churn.csv
dvc status
```

DVC creates or updates:

```text
data/telco_churn.csv.dvc
```

This pointer contains metadata for the dataset version. Commit the pointer file to Git:

```bash
git add data/telco_churn.csv.dvc
git commit -m "Track telecom churn dataset with DVC"
```

Push the actual dataset content to your configured remote:

```bash
dvc push
```

## Restore a dataset version

On another machine, after cloning the repository and configuring access to the same DVC remote:

```bash
dvc pull
```

DVC downloads the dataset version identified by the pointer file.

If you do not have access to the remote used to create the original pointer, use your own CSV and run `dvc add` and `dvc push` as shown above. The pointer in Git alone cannot download data that is not available in a remote you can access.

## Try the placeholder script

```bash
python src/train.py
```

This only prints a message to represent where a training pipeline would run. It is not part of the DVC data-versioning workflow.

## Key takeaway

**Git versions the code and DVC pointer. DVC stores and restores the actual dataset.**

For the combined DVC and MLflow experiment-tracking workflow, use the separate [MLflow and DVC Customer Churn Demo](https://github.com/Ghanathey-Neelesh-Kumar/mlflow-dvc-customer-churn-demo).
