# IT135IU – Introduction to Data Science (Course Project)

## Project Topic 12: Which product categories generate the most returns, and is it worth acting on?

**Team Members:**
- **DANG UYEN THU** (ITDSIU25044) - Product Owner
- **TRUONG THUAN TUAN** (ITDSIU25051) - Data Engineer
- **NGUYEN AN KHOI** (ITDSIU25016) - Data Analyst
- **NGUYEN THI KHANH LINH** (ITDSIU25019) - Visualisation Lead
- **NGUYEN CHI PHUC** (ITDSIU25028) - Reproducibility Lead
- **DANG LE TRUC LINH** (ITDSIU25018) - Ethics and Privacy Officer

---

# D3 – Data Analysis: Return Rates by Product Category

## 1. Overview
This notebook (`D3_analysis.ipynb`) contains the core data analysis for our project. The objective is to answer the decision question: **"Which product categories generate the most returns, and is it worth acting on?"** 

The analysis is performed by the **Data Analyst (Nguyen An Khoi)**. It covers the analytical pipeline from loading the pre-processed data, executing strict cleaning protocols (handling extreme outliers), calculating descriptive statistics (Return Rate by Line, Return Rate by Value, Excess Return Value), performing a Chi-square test of independence, and finally exporting the processed results for the Visualisation Lead.

## 2. Prerequisites & Environment Setup
To run this notebook successfully on your local machine, you must have Python 3 installed. 

**Step 2.1: Clone the Repository**
Open your terminal (Command Prompt / PowerShell / Terminal) and clone the repository:
```bash
git clone https://github.com/truongtuan1312/introds-project
cd introds-project
```

**Step 2.2: Install Required Libraries**
It is highly recommended to use a virtual environment. To install all required dependencies, run:
`pip install -r requirements.txt`

## 3. Step-by-Step Data Preparation (Crucial Step)
Why is this step necessary?
The raw datasets are intentionally excluded from this Git repository (via .gitignore) because the original Excel files exceed 100MB. Uploading them would violate GitHub's file size limits. Furthermore, Excel files are extremely slow to read. Therefore, we convert them to CSV format for fast, reproducible execution.

**Step 3.1: Download the Raw Data**
Download the two original datasets directly from the UCI Machine Learning Repository. You do not need to register; just click download.

Source 1: Online Retail (Dataset 352) -> Download the .xlsx file.

Source 2: Online Retail II (Dataset 502) -> Download the .xlsx file.

**Step 3.2: Convert Excel to CSV**
The notebook D3_analysis.ipynb reads CSV files, not Excel files. You need to convert them:

Open the downloaded Online Retail.xlsx file using Microsoft Excel (or Google Sheets).

Go to File -> Save As (or Export).

Choose the format: CSV (Comma delimited) (*.csv).

Save the file.

Repeat this exact process for the online_retail_II.xlsx file.

**Step 3.3: Rename and Move to the Correct Folder**
The notebook is programmed to look for specific filenames in a specific directory.

Rename the CSV files you just created exactly as follows (all lowercase, no spaces):

Rename the CSV of Source 1 to: retail1.csv

Rename the CSV of Source 2 to: retail2.csv

Create the folder structure: Inside the root folder of your project (introds-project), navigate to the data folder. If there is no raw folder inside data, create one.

Move the files: Place both retail1.csv and retail2.csv directly inside the data/raw/ directory.

Your directory structure MUST look exactly like this before proceeding:

text
introds-project/
├── data/
│   ├── raw/
│   │   ├── retail1.csv    <-- YOU MUST PLACE THIS HERE
│   │   └── retail2.csv    <-- YOU MUST PLACE THIS HERE
│   └── processed/         <-- Notebook will save outputs here automatically
├── notebooks/
│   └── D3_analysis.ipynb  <-- YOU WILL RUN THIS FILE
├── requirements.txt
└── README.md

## 4. How to Execute the Notebook
**Step 4.1: Open the Notebook**

Open the project folder in VS Code or launch Jupyter Notebook.

Navigate to notebooks/D3_analysis.ipynb and open it.

**Step 4.2: Select the Kernel**
If using VS Code, click on the "Select Kernel" button in the top right corner and choose your installed Python 3 environment.

**Step 4.3: Run All Cells**

Click the "Restart" button (circular arrow icon) to clear any previous memory.

Click the "Run All" button (double play icon).

Wait for all cells to execute sequentially. The data loading and cleaning process may take 10–20 seconds.

Reproducibility Note:
The notebook uses a fixed random seed (RANDOM_SEED = 42). Running this notebook on any machine will yield the exact same numerical results reported in our final D4 submission.

## 5. Methodology & Cleaning Decisions
To ensure our findings are accurate, the notebook applies strict logic that differs slightly from the raw D2 output:

Removing Non-Products: We drop rows where StockCode is not a 5-digit number (removes postage, manual adjustments, discounts).

Outlier Handling: We remove exactly 15 lines with an absolute Quantity >= 10,000. The raw data contains extreme data-entry errors (e.g., two bulk orders of 80,995 and 74,215 units that were cancelled immediately). If kept, these two lines alone would artificially inflate the return rate of their categories by over 10%. A sensitivity check confirms our top categories remain stable across different thresholds.

Cancellation Proxy: As the dataset lacks a literal "return" field, we flag InvoiceNo starting with 'C' as a cancellation/return proxy.

Primary Metric: We evaluate returns using Excess Return Value (the financial loss exceeding the system-wide average of 2.43%), which directly answers "is it worth acting on".

## 6. Expected Outputs
If the notebook runs successfully, it will automatically generate two files inside the data/processed/ folder. These files are the handover to the Visualisation Lead for chart generation:

category_summary.csv: Contains descriptive stats (sales, returns, return rates, excess return values).

category_return_rate_ci.csv: Contains the 95% Wilson Confidence Intervals for line return rates.

At the very bottom of the notebook, there is an Appendix cell that explicitly displays the 15 removed outlier rows for transparency.

7. Troubleshooting (Common Errors)
Error: FileNotFoundError: [Errno 2] No such file or directory: '../data/raw/retail1.csv'

Fix: You did not place retail1.csv and retail2.csv in the correct folder, or you named them incorrectly. Re-check Section 3.3.

Error: ModuleNotFoundError: No module named 'scipy' (or statsmodels)

Fix: You haven't installed the required libraries. Run pip install scipy statsmodels in your terminal.

Error: ParserError: Error tokenizing data

Fix: When saving the CSV from Excel, ensure you used the standard "CSV (Comma delimited)" format, not "CSV UTF-8" or "Macintosh".
