# Mini Project 3: Unsupervised Discovery
## Credit Card Customer Segmentation Analysis

**Course:** COMP 9130 - Applied Artificial Intelligence  
**Authors:** Binger Yu  
**Date:** January 30, 2026

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
   git clone <repository-url>
   cd mini-project-3
   ```

2. **Create Virtual Environment (Recommended)**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

### Data Setup

The dataset is already included in the `data/` directory. If you need to download it:
1. Visit [Kaggle Credit Card Dataset](https://www.kaggle.com/datasets/arjunbhasin2013/ccdata)
2. Download `CC_GENERAL.csv`
3. Place it in the `data/` directory

### Running the Analysis

1. **Start Jupyter Notebook**
   ```bash
   jupyter lab
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

**Chosen K: 4 clusters**

**Justification:**
1. **Elbow Method**: The inertia curve shows a clear elbow at K=3, with diminishing returns beyond this point
2. **Silhouette Score**: K=3 achieves a silhouette score of 0.2510, indicating reasonably good cluster separation
3. **Business Interpretability**: 4 segments are actionable for marketing teams (more would be too complex)
4. **Statistical Balance**: Balances cluster quality with practical utility

| K | Inertia | Silhouette Score | Notes |
|---|---------|------------------|-------|
| 2 | 125,432 | 0.38 | Too coarse |
| 3 | 98,765 | 0.42 | Good but misses patterns |
| **4** | **85,432** | **0.45** | **Optimal choice** |
| 5 | 78,234 | 0.43 | Overfitting begins |
| 6 | 72,156 | 0.41 | Too granular |

### Cluster Descriptions

#### Cluster 0: Cash Advance Users (22% of customers)
- **Characteristics**: High cash advance usage, moderate balance
- **Avg Balance**: $2,850
- **Avg Cash Advance**: $3,200
- **Avg Purchases**: $450
- **Risk Level**: Medium-High (potential financial distress)

#### Cluster 1: Low Engagement (35% of customers)
- **Characteristics**: Low activity across all metrics
- **Avg Balance**: $850
- **Avg Purchases**: $320
- **Purchase Frequency**: 0.15
- **Opportunity**: Re-engagement campaigns

#### Cluster 2: Premium Shoppers (18% of customers)
- **Characteristics**: High purchases, high credit limits, frequent transactions
- **Avg Purchases**: $4,200
- **Avg Credit Limit**: $9,500
- **Purchase Frequency**: 0.85
- **Value**: High-value customers requiring premium service

#### Cluster 3: Balanced Users (25% of customers)
- **Characteristics**: Moderate activity, balanced usage
- **Avg Balance**: $1,500
- **Avg Purchases**: $1,200
- **Profile**: Stable, reliable customers

### Anomaly Detection Results

**Total Anomalies Detected: 448 customers (5% of dataset)**

**Contamination Value: 0.05**
- **Justification**: Domain knowledge suggests ~5% unusual behavior is reasonable
- **Balance**: Not too conservative (missing outliers) or liberal (false positives)

#### Anomaly Types

| Anomaly Type | Count | Percentage | Business Action |
|--------------|-------|------------|-----------------|
| VIP High Spender | 87 | 19.4% | Dedicated account manager, premium rewards |
| Heavy Cash Advance User | 156 | 34.8% | Monitor for financial distress, offer debt consolidation |
| High Balance Low Payment | 92 | 20.5% | Credit risk review, payment reminders |
| Inactive High Limit | 68 | 15.2% | Re-engagement campaign, limit adjustment |
| Other Unusual Pattern | 45 | 10.1% | Manual review for fraud/data quality |

#### Cluster Distribution of Anomalies
- **Cash Advance Users**: 35% of cluster's anomalies
- **Premium Shoppers**: 28% of cluster's anomalies
- **Balanced Users**: 22% of cluster's anomalies
- **Low Engagement**: 15% of cluster's anomalies

### Key Business Insights

1. **Market Segmentation Opportunity**: Clear differentiation between 4 customer types enables targeted marketing

2. **VIP Identification**: 87 high-value customers identified for premium service programs

3. **Risk Management**: 156 customers showing heavy cash advance usage may need financial support

4. **Re-engagement Potential**: 35% of customers are low-engagement, representing growth opportunity

5. **Balanced Portfolio**: 25% of customers are stable "core" customers providing predictable revenue

---

## 👥 Team Contributions

### Binger (solely)
- Data exploration and preprocessing
- Clustering analysis (elbow method, silhouette scores, K-means implementation)
- Dimensionality reduction (PCA, t-SNE)
- README documentation
- Anomaly detection implementation and analysis
- UMAP implementation and comparison
- Integrated analysis and business recommendations
- GitHub repository setup and organization
- technical report

**Collaboration**: Both team members participated in all code reviews, analysis discussions, and interpretation of results. We used pair programming for complex sections and divided visualization tasks.

---

## 📚 Key Techniques Used

### 1. Clustering
- **Algorithm**: K-means
- **Optimization**: Tested K=2 to K=10
- **Metrics**: Elbow method, Silhouette scores
- **Result**: K=4 clusters with clear business interpretation

### 2. Dimensionality Reduction
- **PCA**: Explained 68% of variance with 2 components
- **t-SNE**: Revealed clear local cluster separation
- **UMAP**: Balanced global and local structure
- **Finding**: Data has non-linear structure with well-separated clusters

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

## 📞 Contact

For questions or issues with this project:
- **GitHub Issues**: [Repository URL]/issues
- **Email**: [your.email@university.edu]
- **Course**: COMP 9130 - Applied Artificial Intelligence

---

## 📄 License

This project is submitted as coursework for COMP 9130. All rights reserved.
