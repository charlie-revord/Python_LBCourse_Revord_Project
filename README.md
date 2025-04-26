# ▶️ Introduction 

As part of [Luke Barousse's Python for Data Analytics Course](https://www.lukebarousse.com/python), I have been tasked with performing an analysis of webscraped job posting information from 2023 using Python. This dataset compiles thousands of data roles posted on popular job boards like LinkedIn, and offers ample opportunities for budding analysts like myself to practice Python data manipulation and visualization workflows.

The dataset is accessible from this link: https://huggingface.co/datasets/lukebarousse/data_jobs

Sample fields from the dataset include:

* Job Post Date
* Job Post Location
* Company Name
* Job Title
* Salary Information
* Desired Skills
* And more!

# 🏙️ Defining the Analysis

To tailor the analysis project to my interests, I have decided to analyze all Chicagoland area jobs referenced in the dataset. Though I recently accepted a full-time analyst role, I would like to know what skills are sought by employers as I plan future upskilling sessions. 

The code snippets included below handle initial library and dataset imports.

```python
# Library and module imports
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib.ticker import PercentFormatter
import numpy as np
import openpyxl as xl
from datetime import datetime
import ast
from adjustText import adjust_text
from datasets import load_dataset
import seaborn as sns
```

```python
# load in Luke Barousse's jobs dataset as dataframe
dataset = load_dataset('lukebarousse/data_jobs')
jobs_data = dataset['train'].to_pandas()

# Convert job_posted_date to datetime type, convert nested job skill list strings to lists, and output field info
jobs_data['job_posted_date'] = pd.to_datetime(jobs_data['job_posted_date'])
jobs_data['job_skills'] = jobs_data['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
jobs_data.info()
```

# ❔ Analysis Questions

Looking at the Chicagoland job market, my analysis questions are as follows: 

1. What are the Top 3 Data Roles posted within the Chicagoland area? For the Top 3 Data Roles, which companies routinely hire for these roles? 
2. What skills are most frequently mentioned in job posts for the Top 3 Data Roles? 
3. For the Top Role, how do top skills for this role trend over a year of job posts? 
4. How robust is the salary information for Chicagoland jobs in the dataset, and what trends can be extrapolated from it? 
5. Finally, for top role postings with salary information, which skills are most requested and which are most lucrative? Are there skills which balance high demand with good pay? 


# 🚖 Filtering for Chicago "Windy City" Data

The following code snippet isolates job posts within the U.S. that contain the string (any case) 'Chicago' in the job location field:

```python
# Creates chicago-based roles dataframe
chicago_roles_data = (
    jobs_data[(jobs_data['job_country'] == 'United States') & 
    (jobs_data['job_location'].str.lower().str.contains('chicago'))]
    )
```

# Analysis

The analysis of the Chicagoland dataset is split into sections for each question, respectively. The raw code can be referenced in the attached Jupyter Notebook called [data_workbook](data_workbook.ipynb). 