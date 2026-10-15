# Customer Segmentation using Particle Swarm Optimization

**Problem 21:** Group synthetic customer records based on purchase frequency, spending, and product preferences using an unsupervised clustering algorithm.

**Course:** Bio-Inspired Computing

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1hmDmavdC1OHRwQplPeGMVw5wAvpP78ts?usp=sharing)

## Team

| Name | Register Number |
|------|-----------------|
| Kejal Jain | RA2311003010127 |
| Harsh Jain | RA2311003011135 |

## Overview

This project segments 3,000 synthetic customers into meaningful groups using **Particle Swarm Optimization (PSO)**, a bio-inspired algorithm based on how birds flock and fish school. A swarm of particles searches for the best cluster centres. The result is compared with K-Means as a baseline, and each segment is profiled and named for targeted marketing.

## Dataset

The dataset is generated in code with a fixed random seed (`42`), so results are reproducible.

| Feature | Description |
|---------|-------------|
| `purchase_frequency` | Number of orders per year |
| `total_spend` | Total money spent (₹) |
| `Electronics`, `Fashion`, `Grocery`, `Home` | Share of purchases in each category (sums to 1) |

- 3,000 customers, 6 features, no missing values
- Generated from 4 hidden customer groups using Poisson (frequency), lognormal (spend) and Dirichlet (category mix) distributions
- Hidden group labels are never given to the model

## PSO Design

| Component | Description |
|-----------|-------------|
| Representation | Each particle holds `k` cluster centres (4 × 6 = 24 values) |
| Objective | Minimize SSE: squared distance from each customer to its nearest centre |
| Constraints | Centres stay within the data range, velocity is clipped, and clusters with fewer than 20 customers are penalized |
| Strategy | 40 particles, 150 iterations, inertia weight decreasing from 0.9 to 0.4, c1 = c2 = 1.5 |

Update rule:

```
v = w·v + c1·r1·(pbest − x) + c2·r2·(gbest − x)
x = x + v
```

## Methodology

1. Generate the synthetic customer data
2. Exploratory data analysis (distributions and correlation heatmap)
3. Preprocessing: log transform on `total_spend`, then `StandardScaler` on all features
4. Run PSO for k = 2 to 8 and choose k using the SSE curve and silhouette score
5. Run the final PSO clustering with k = 4 and plot its convergence
6. Visualize clusters with PCA
7. Compare PSO with K-Means
8. Profile and name each segment

## Results

- Best number of clusters: **4**
- Silhouette score: **0.544**

| Metric | PSO | K-Means |
|--------|-----|---------|
| SSE (lower is better) | 4,384.9 | 4,384.6 |
| Silhouette (higher is better) | 0.544 | 0.544 |
| Davies-Bouldin (lower is better) | 0.744 | 0.744 |
| Calinski-Harabasz (higher is better) | 3,101 | 3,101 |
| Agreement between clusterings (ARI) | 1.0 | |

PSO reaches the same quality as K-Means and finds identical segments. K-Means is faster, while PSO adds a flexible, constraint-aware search.

| Segment | Customers | Avg Orders / Year | Avg Spend (₹) | Main Preference | Revenue Share |
|---------|-----------|-------------------|---------------|-----------------|---------------|
| Frequent Grocery Buyers | 901 | 44.87 | 27,630 | Grocery (0.67) | 17.1% |
| Low-Activity Customers | 779 | 5.98 | 9,304 | Balanced | 5.0% |
| High-Value Electronics Buyers | 600 | 39.67 | 1,45,548 | Electronics (0.58) | 59.9% |
| Fashion-Focused Customers | 720 | 19.55 | 36,409 | Fashion (0.76) | 18.0% |

**Key insight:** High-Value Electronics Buyers are only 20% of customers but generate about 60% of revenue.

## Business Recommendations

- **High-Value Electronics Buyers:** VIP loyalty tier, early access to new gadgets, premium bundles
- **Frequent Grocery Buyers:** subscribe-and-save, delivery pass, basket-size upsell
- **Fashion-Focused Customers:** new-arrival alerts, seasonal sales, style-based recommendations
- **Low-Activity Customers:** re-engagement offers, reminders, free-shipping threshold

## How to Run

1. Click the **Open In Colab** badge above
2. Go to **Runtime → Run all**
3. All outputs and charts appear inside the notebook. No files need to be uploaded.

## Tech Stack

Python, NumPy, Pandas, Matplotlib, Seaborn, scikit-learn (StandardScaler, KMeans, PCA, evaluation metrics)

## Links

- GitHub: https://github.com/harshjn9/harshjn9
- Colab Notebook: https://colab.research.google.com/drive/1hmDmavdC1OHRwQplPeGMVw5wAvpP78ts?usp=sharing
