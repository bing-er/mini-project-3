# Data Download Instructions

## Credit Card Customer Segmentation Dataset

### Option 1: Kaggle (Recommended)
1. Visit: https://www.kaggle.com/datasets/arjunbhasin2013/ccdata
2. Click "Download" button (requires free Kaggle account)
3. Extract `CC_GENERAL.csv` 
4. Place in the `data/` directory

### Option 2: Already Included
The dataset is already included in this repository in the `data/` folder.

### Dataset Information
- **Filename**: CC_GENERAL.csv
- **Size**: 902.88 KB
- **Rows**: 8,950 customers
- **Columns**: 18 (1 ID + 17 features)
- **Format**: CSV (comma-separated values)

### Verification
After downloading, verify the file:
```bash
# Check file exists
ls data/CC_GENERAL.csv

# Check number of rows (should be 8951 including header)
wc -l data/CC_GENERAL.csv

# View first few lines
head data/CC_GENERAL.csv
```

Expected output:
- Line count: 8951 (8950 data rows + 1 header)
- First line should be: CUST_ID,BALANCE,BALANCE_FREQUENCY,PURCHASES,...
