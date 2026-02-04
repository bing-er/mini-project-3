# Mini Project 3: Unsupervised Discovery
## Credit Card Customer Segmentation Analysis

**Course:** COMP 9130 - Applied Artificial Intelligence  
**Authors:** Binger Yu  
**Date:** February 4, 2026

---

## 📋 Problem Description

### Business Problem
A bank seeks to segment its credit card customers based on their 6-month usage behavior to develop targeted product offerings and personalized marketing strategies. Currently, the bank treats all customers uniformly, missing opportunities to:
- Provide premium services to high-value customers
- Identify and assist customers showing signs of financial distress
- Re-engage inactive customers
- Detect fraudulent or unusual behavior patterns

### Why Unsupervised Learning?
This problem is ideal for unsupervised learning because:
1. **No Predefined Labels**: We don't have existing customer categories
2. **Pattern Discovery**: We want to discover natural groupings in customer behavior
3. **Multiple Objectives**: Clustering for segmentation, anomaly detection for outliers, dimensionality reduction for visualization
4. **Exploratory Nature**: We're discovering insights rather than predicting known outcomes

The goal is to identify meaningful customer segments, visualize complex behavioral patterns, and detect unusual cases that require special attention.

---

## 📊 Dataset Description

### Source
- **Dataset:** Credit Card Customer Segmentation  
- **Source:** [Kaggle - Credit Card Dataset](https://www.kaggle.com/datasets/arjunbhasin2013/ccdata)
- **Size:** 8,950 customers × 18 features
- **Time Period:** 6 months of credit card usage behavior

### Features

| Feature | Description | Type |
|---------|-------------|------|
| CUST_ID | Customer identification | Identifier |
| BALANCE | Account balance | Continuous |
| BALANCE_FREQUENCY | Frequency of balance updates (0-1) | Continuous |
| PURCHASES | Total purchase amount | Continuous |
| ONEOFF_PURCHASES | One-time purchase amount | Continuous |
| INSTALLMENTS_PURCHASES | Installment purchase amount | Continuous |
| CASH_ADVANCE | Cash advance amount | Continuous |
| PURCHASES_FREQUENCY | Purchase frequency (0-1) | Continuous |
| ONEOFF_PURCHASES_FREQUENCY | One-time purchase frequency | Continuous |
| PURCHASES_INSTALLMENTS_FREQUENCY | Installment purchase frequency | Continuous |
| CASH_ADVANCE_FREQUENCY | Cash advance frequency (0-1) | Continuous |
| CASH_ADVANCE_TRX | Number of cash advance transactions | Discrete |
| PURCHASES_TRX | Number of purchase transactions | Discrete |
| CREDIT_LIMIT | Credit card limit | Continuous |
| PAYMENTS | Total payments | Continuous |
| MINIMUM_PAYMENTS | Minimum payments made | Continuous |
| PRC_FULL_PAYMENT | Percentage of full payments | Continuous |
| TENURE | Duration of credit card service (months) | Discrete |

### Data Quality
- **Missing Values:** 
  - MINIMUM_PAYMENTS: 313 missing (3.5%)
  - CREDIT_LIMIT: 1 missing
- **Solution:** Imputed with median values (robust to outliers)

### Preprocessing Steps
1. **Handling Missing Data**: Median imputation for MINIMUM_PAYMENTS and CREDIT_LIMIT
2. **Feature Selection**: Removed CUST_ID (identifier only)
3. **Standardization**: Applied StandardScaler to all features (critical for K-means)
4. **No Outlier Removal**: Kept all data points; outliers identified through anomaly detection

---

## 🚀 Setup Instructions

### Prerequisites
- Python 3.8 or higher
- pip package manager
- Jupyter Notebook

### Installation

1. **Clone or Download the Repository**
   ```bash
   git clone https://github.com/bing-er/mini-project-3.git
   cd mini-project-3
   ```

2. **Create Virtual Environment (Recommended)**
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install Dependencies**
   
   **Option A: Use Setup Script (Recommended)**
   ```bash
   # On Mac/Linux:
   chmod +x setup.sh
   ./setup.sh
   
   # On Windows:
   setup.bat
   ```
   
   **Option B: Manual Installation**
   ```bash
   # Upgrade pip first
   python -m pip install --upgrade pip setuptools wheel
   
   # Install requirements
   pip install -r requirements.txt
   ```

4. **Troubleshooting**
   
   If you encounter installation issues (especially with UMAP), see the detailed [TROUBLESHOOTING.md](TROUBLESHOOTING.md) guide.
   
   **Quick fix for UMAP issues:**
   ```bash
   python -m pip install --upgrade pip setuptools wheel
   pip install numba llvmlite
   pip install umap-learn --no-cache-dir
   ```

### Data Setup

The dataset is already included in the `data/` directory. If you need to download it:
1. Visit [Kaggle Credit Card Dataset](https://www.kaggle.com/datasets/arjunbhasin2013/ccdata)
2. Download `CC_GENERAL.csv`
3. Place it in the `data/` directory

### Running the Analysis

1. **Start Jupyter Notebook**
   ```bash
   jupyter notebook
   ```

2. **Open the Analysis Notebook**
   - Navigate to `notebooks/analysis.ipynb`
   - Run all cells: `Run > Run All`

3. **Expected Runtime**
   - Full analysis: ~5-10 minutes
   - t-SNE is the slowest component (~2-3 minutes)

---

## 📈 Results Summary

### Optimal K Selection

**Chosen K: 3 clusters**

**Justification:**
1. **Elbow Method**: The inertia curve shows a clear elbow at K=3, with diminishing returns beyond this point (inertia drops from 127,784 at K=2 to 111,975 at K=3)
2. **Silhouette Score**: K=3 achieves the highest silhouette score of 0.2510, indicating the best-defined clusters among all K values tested
3. **Business Interpretability**: 3 segments provide clear, actionable business categories without over-segmentation
4. **Statistical Quality**: While the silhouette score is moderate (0.25), this is typical for customer behavioral data where segments naturally overlap along a continuum

| K | Inertia | Silhouette Score | Notes |
|---|---------|------------------|-------|
| 2 | 127,784.53 | 0.2100 | Too coarse - misses important patterns |
| **3** | **111,975.04** | **0.2510** | **Optimal - Highest silhouette score** |
| 4 | 99,061.94 | 0.1977 | Lower silhouette, unnecessary complexity |
| 5 | 91,490.50 | 0.1931 | Continued decline in quality |
| 6 | 84,826.59 | 0.2029 | Too granular for business use |

### Cluster Descriptions

#### Cluster 0: Premium Shoppers (14.2% of customers)
- **Characteristics**: High purchases, high credit limits, very frequent transactions
- **Avg Balance**: $2,182.35
- **Avg Purchases**: $4,187.02 (4.17× dataset mean - highest of all clusters)
- **Avg Cash Advance**: $449.75 (below average - not cash-dependent)
- **Avg Credit Limit**: $7,642.78 (1.70× dataset mean)
- **Purchase Frequency**: 0.95 (extremely active - use card regularly)
- **Risk Level**: Low
- **Value**: Highest-value customers requiring premium service and VIP treatment

**Business Profile**: Premium Shoppers are the bank's most valuable segment. They make frequent, high-value transactions and maintain strong payment histories. These customers use their cards as their primary payment method rather than for emergency liquidity.

---

#### Cluster 1: Low Engagement (68.3% of customers)
- **Characteristics**: Low activity across all metrics, largest segment
- **Avg Balance**: $807.72 (below average)
- **Avg Purchases**: $496.06 (0.49× dataset mean - less than half average!)
- **Avg Cash Advance**: $339.00 (low)
- **Avg Credit Limit**: $3,267.02 (0.73× dataset mean)
- **Purchase Frequency**: 0.46 (infrequent use - card is underutilized)
- **Risk Level**: Low
- **Opportunity**: Largest growth opportunity through re-engagement campaigns

**Business Profile**: Low Engagement customers represent dormant or underutilized accounts. These cardholders maintain their accounts but show minimal activity, suggesting either dissatisfaction, competitive card usage, or lack of awareness of benefits.

---

#### Cluster 2: Cash Advance Users (17.4% of customers)
- **Characteristics**: Very high cash advance usage, low purchase activity
- **Avg Balance**: $4,023.79 (2.57× dataset mean - carrying significant debt)
- **Avg Purchases**: $389.05 (0.39× dataset mean - very low shopping)
- **Avg Cash Advance**: $3,917.25 (4.00× dataset mean! - extreme reliance on cash)
- **Avg Credit Limit**: $6,729.47 (1.50× dataset mean)
- **Purchase Frequency**: 0.23 (rarely shop, mainly use for cash advances)
- **Risk Level**: High - potential financial distress
- **Concern**: Heavy cash advance reliance suggests liquidity problems

**Business Profile**: Cash Advance Users exhibit a concerning pattern of high cash withdrawals with minimal purchase activity. The elevated balances (2.57× mean) combined with heavy cash advance usage (4.00× mean) suggest these customers may be experiencing financial difficulties and using cash advances to meet liquidity needs.

### Anomaly Detection Results

**Total Anomalies Detected: 448 customers (5.01% of dataset)**

**Contamination Value: 0.05**
- **Justification**: Domain knowledge suggests ~5% unusual behavior is reasonable for credit card data
- **Balance**: Not too conservative (missing real anomalies) or too liberal (false positives)
- **Actionability**: Manageable number of cases to investigate thoroughly

#### Anomaly Types

| Anomaly Type | Count | Percentage | Business Action |
|--------------|-------|------------|-----------------|
| VIP High Spender | 91 | 20.3% | Dedicated account manager, premium rewards, exclusive benefits |
| Heavy Cash Advance User | 163 | 36.4% | Monitor for financial distress, offer debt consolidation, budgeting tools |
| High Balance Low Payment | 2 | 0.4% | Flag for credit risk review, send payment reminders, offer payment plans |
| Inactive High Limit | 3 | 0.7% | Re-engagement campaign, targeted offers, consider limit adjustment |
| Other Unusual Pattern | 189 | 42.2% | Manual review for data quality or fraud detection |

#### Cluster Distribution of Anomalies

**Where anomalies are concentrated:**
- **Premium Shoppers (Cluster 0)**: 282 anomalies - **22.1% of this cluster**
- **Cash Advance Users (Cluster 2)**: 158 anomalies - **10.1% of this cluster**
- **Low Engagement (Cluster 1)**: 8 anomalies - **0.1% of this cluster**

**Key Insight**: Anomalies are heavily concentrated in Premium Shoppers (22.1%) and Cash Advance Users (10.1%), while Low Engagement has minimal anomalies (0.1%). This pattern makes business sense: extreme behaviors (very high spending or high cash dependence) are more likely to be anomalous. The high anomaly rate in Premium Shoppers suggests this cluster contains both typical high-spenders and exceptional VIPs needing differentiated service levels.

### Key Business Insights

1. **Market Segmentation Opportunity**: Clear differentiation between 3 distinct customer types enables highly targeted marketing strategies and service levels

2. **VIP Identification**: 91 high-value customers identified for premium service programs and dedicated account management (20.3% of anomalies)

3. **Risk Management**: 163 customers showing heavy cash advance usage (4.00× dataset mean) require immediate financial wellness interventions and debt consolidation offers

4. **Re-engagement Potential**: 68.3% of customers are low-engagement, representing the largest growth opportunity. If even 10% can be activated, that's 611 additional active customers

5. **Premium Focus**: 14.2% of customers (Premium Shoppers) drive disproportionate revenue with 4.17× average purchase activity. These customers deserve premium treatment to ensure retention

6. **Anomaly Concentration**: 22.1% of Premium Shoppers are anomalous, indicating this segment contains both typical high-spenders and exceptional VIPs. This suggests a potential sub-segmentation opportunity within Premium Shoppers

7. **Balanced Portfolio**: Despite the large Low Engagement segment, the portfolio maintains a healthy mix of premium customers (14.2%) and moderate-risk cash users (17.4%)

---

## 👥 Team Contributions

### Binger Yu (solely)
- Data exploration and preprocessing
- Clustering analysis (elbow method, silhouette scores, K-means implementation)
- Dimensionality reduction (PCA, t-SNE)
- README documentation
- Anomaly detection implementation and analysis
- UMAP implementation and comparison
- Integrated analysis and business recommendations
- GitHub repository setup and organization
- 100% of technical report

---

## 📚 Key Techniques Used

### 1. Clustering
- **Algorithm**: K-means
- **Optimization**: Tested K=2 to K=10
- **Metrics**: Elbow method, Silhouette scores
- **Result**: K=3 clusters (silhouette score: 0.2510 - highest among all K values tested)

### 2. Dimensionality Reduction
- **PCA**: Explained 47.6% of variance with 2 components (PC1: 27.3%, PC2: 20.3%)
- **t-SNE**: Revealed clear local cluster separation and non-linear patterns
- **UMAP**: Balanced global and local structure, best for business presentations
- **Finding**: Data has predominantly non-linear structure with moderately well-separated clusters

### 3. Anomaly Detection
- **Algorithm**: Isolation Forest
- **Contamination**: 0.05 (5% of dataset)
- **Categories**: 5 distinct anomaly types identified
- **Integration**: Anomalies visualized across all dimensionality reduction plots

---

## 🔍 Limitations and Future Work

### Limitations
1. **Temporal Dynamics**: Analysis based on 6-month snapshot; doesn't capture seasonal patterns
2. **Feature Engineering**: Could benefit from derived features (e.g., debt-to-limit ratio)
3. **External Data**: Lacks demographic information that could enhance segmentation
4. **Static Clustering**: Customers may migrate between segments over time

### Future Improvements
1. **Hierarchical Clustering**: Compare with K-means to validate segment structure
2. **DBSCAN**: Test for handling non-spherical clusters
3. **Time-Series Analysis**: Track cluster membership changes over time
4. **Predictive Modeling**: Build classifiers to predict cluster membership for new customers
5. **A/B Testing**: Validate business recommendations through controlled experiments

---

## 📖 References

1. **Dataset Source**: Arjun Bhasin (2018). Credit Card Dataset for Clustering. Kaggle. https://www.kaggle.com/datasets/arjunbhasin2013/ccdata

2. **Scikit-learn Documentation**:
   - Clustering: https://scikit-learn.org/stable/modules/clustering.html
   - Decomposition: https://scikit-learn.org/stable/modules/decomposition.html
   - Ensemble Methods: https://scikit-learn.org/stable/modules/ensemble.html

3. **UMAP Documentation**: McInnes, L., Healy, J., & Melville, J. (2018). UMAP: Uniform Manifold Approximation and Projection for Dimension Reduction. https://umap-learn.readthedocs.io/

4. **Course Materials**: COMP 9130 Week 4 lecture notes and starter notebooks

---

## 📄 License

This project is submitted as coursework for COMP 9130. All rights reserved.
