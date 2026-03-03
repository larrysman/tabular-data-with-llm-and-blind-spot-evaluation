# TABULAR DATA EVALUATION WITH LLM
This project leverage on the textual model to evaluate a tabular dataset on Hugging Face and created a set of tabular inputs for model fine-tuning and push the datasets to Hugging Face.

**Blind Spots of Qwen/Qwen3.5-4B on Tabular Dataset**

## Overview
This tabular dataset contains the model inputs, model outputs, expected results for the systematic failure cases of the base model **Qwen/Qwen3.5-4B**.
The model has 4-billion parameters pretrained language model released within the last six months and fall between the 0.6-6B model requirements for the project.

The model were developed to performs well on general text tasks, it was not trained for *structured or tabular data* and for this reason making it a strong choice for probing the *blind spots* in reasoning over rows, columns, numerical values, categorical values, and mixed-type inputs.

The dataset contains diversed examples where the model's predictions are incorrect, imcomplete, and logically inconsistent when interpreting them with small tabular information. Each entry in the tabular data includes:
- **input**: this is the tabular prompt given to the model
- **model_output**: this is the actual output produced by the model **Qwen/Qwen3.5-4B**.
- **expected_output**: this is the correct and intended result of the prompt.

The goal is to highlight areas where small frontier models struggle with the structured or tabular data especially in this era where large langugae models are trained on textual data but limited training on structured data. Also, to provide seed data for future training, fine-tuning and evaluation.

## Model Evaluated
**Model**: `Qwen/Qwen3.5-4B`

**Type**: Base model (pre-trained, post-trained but not instruction tuned)

**Parameters**: 4B

**Release window**: Within the last six (6) months

**Modality**: Text but tested on tabular inputs.

## How the Qwen/Qwen3.5-4B model was loaded
To successfully load the model, it is important to follow the steps below:

- **Step 1**: Install and Import all necessary libraries and dependencies:

![ss1](https://cdn-uploads.huggingface.co/production/uploads/69a5b703a671119c7ba1453e/wstYRoxgOkFlt1yvLfOZw.png)


![ss2](https://cdn-uploads.huggingface.co/production/uploads/69a5b703a671119c7ba1453e/975p2_mn0SI5PHgWideJG.png)

- **Step 2**: Logging inot the Hugging Face Platform:

![ss3](https://cdn-uploads.huggingface.co/production/uploads/69a5b703a671119c7ba1453e/0d2jEaTpQ9Ht0Ug0zTuYv.png)

- **Step 3**: Load the Qwen/Qwen3.5-4B base model:

![ss4](https://cdn-uploads.huggingface.co/production/uploads/69a5b703a671119c7ba1453e/mMfP6jMt_zkLV037Pld_I.png)

- **Step 4**: Write a Python function for laoding the model:

![ss5](https://cdn-uploads.huggingface.co/production/uploads/69a5b703a671119c7ba1453e/uTcNYJIKf5xWd6hb-gIRI.png)

- **Step 5**: Run a single-shot tabular inputs on the model:

![image](https://cdn-uploads.huggingface.co/production/uploads/69a5b703a671119c7ba1453e/uz9DGCMkyzU04QyR3CB4O.png)

- **Step 6**: Examine the output of the model on the single-shot inputs:

![image](https://cdn-uploads.huggingface.co/production/uploads/69a5b703a671119c7ba1453e/-f4qfKT92SaLli04k3_BN.png)

With this simple single-shot, it is clearly that the blind spot in this model is that it failed to perform simple calculations on tabular data, failed to extract row entries from the tabular data and hence could not produce the accurate expected results.
These codes were executed in a Google Colab notebook using a free T4 GPU runtime.

## Dataset Creation
In order to test the model on structured or tabular datasets, I constructed a set of diverse tabular prompts with central focus on:
- Numerical Reeasoing
- Row and Column Comparisons
- Missing Label Handling
- Categorical Intepretation
- Robustness to formatting variations (pipe-delimited and inconsistent spacing)
- Arithmetic Computation Over Table Values
- Mixed Cases of Numerical and Categorical-Textual Fields.

For each prompt to the model, I recorded `model_input`, `model_output` and the `expected_result` and these were collected to create this dataset.

## Model Blind Spots Observability
After careful runnings of the model on the curated tabular dataset the following areas were the points were the model failed on the tabular data:

- **Numerical Reasoning**: The model frequently wrong calculations involving `sums`, `comparisons`, and `totals (price * qty)` etc.
- **Column and Row Interpretation**: The model confuses which columns or rows to compare, misreads row boundaries, ignores missing values and treat text as numbers or viceversa.
- **Mixed Type Reasoning**: The model struggles when some values are numeric and others are text ("ten" vs 10), categorical labels require counting.
- **Formatting Sensitivity**: The model performances degrades when the different formatting are present in the tabular datasets such as Pipe (|), allowing inconsistent spacing, missing headers etc.

## How to Improve The Model Blind Spots
I would strongly recommended a targeted fine‑tuning and the dataset should include:

- synthetic and real tables (CSV, TSV, pipe‑delimited, messy formats)

- row‑level reasoning tasks (maximum, minimum, filtering, comparisons)

 - column‑level reasoning tasks (sums, averages, missing values)

- arithmetic tasks grounded in table values

- mixed categorical and numerical fields

- explanations or chain‑of‑thought

## How to Assemble The Dataset For Improving the Model Blind Spots

- Convert open datasets from platforms such as UCI, Kaggle, OpenML into text‑table prompts

- Generate synthetic tables programmatically using Python pandas

- Add noise such as typos, inconsistent spacing, missing values to datasets

- Use templated question generation to ensure coverage.

## Dataset Size Recommendations
- *Pilot fine‑tune*: 5k–20k examples

- *Full fine‑tune*: 50k–200k examples

- *High‑quality curated examples* matter more than raw size.

## Intended Use Case
This dataset is intended for evaluating tabular reasoning in small frontier models, red‑teaming and robustness analysis, fine‑tuning experiments on structured data, research on multimodal or mixed‑modality LLM capabilities. Note that, it is not intended for production use or safety‑critical applications.

## Visit My Hugging Face:

**Datasets**: https://huggingface.co/datasets/olalytics/qwen-tabular-data-blindspots

**Email**: Larrysman2004@yahoo.com

**HuggingFace Username**: olalytics
