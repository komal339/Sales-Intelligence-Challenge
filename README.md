# Sales Insight & Alert System

## Approach
This project tackles the problem of declining win rates despite a healthy pipeline.  
We chose **Option B: Win Rate Driver Analysis** as the foundation for building a lightweight decision engine.

- **Modeling Choice:**  
  - Logistic regression was selected for its simplicity and interpretability.  
  - Features included: industry, region, product type, lead source, deal stage, sales cycle length, and deal amount.  
  - Target variable: deal outcome (Won = 1, Lost = 0).  
  - Categorical features were one‑hot encoded; numeric features were passed through unchanged.

- **Outputs:**  
  - Coefficients show which factors help or hurt win probability.  
  - Positive drivers (e.g., FinTech industry, Demo stage, Referral leads) highlight strengths.  
  - Negative drivers (e.g., Qualified stage, Partner leads, APAC/HealthTech industries) highlight weaknesses.

- **Business Usefulness:**  
  - It provides actionable insights for CROs and sales leaders to prioritize deals, adjust strategy, and improve qualification.

---

## How to Run the Project
1. **Environment Setup:**
   - Python 3.9+  
   - Install dependencies:  
     ```bash
     pip install pandas matplotlib scikit-learn
     ```

2. **Data Preparation:**
   - Export data from excel.  
   - Ensure columns include: `industry`, `region`, `product_type`, `lead_source`, `deal_stage`, `sales_cycle_days`, `deal_amount`, `outcome`.

3. **Run the Model:**
   - Train logistic regression on historical data.  
   - Save coefficients into a DataFrame called `importance`.

4. **Visualize Drivers:**
   - Use the provided plotting script:  
     ```python
     importance_sorted = importance.sort_values(by='coefficient', ascending=True)
     colors = ['orange' if val < 0 else 'steelblue' for val in importance_sorted['coefficient']]
     plt.barh(importance_sorted['feature'], importance_sorted['coefficient'], color=colors)
     plt.xlabel('Coefficient')
     plt.title('Win Rate Drivers')
     plt.show()
     ```

5. **Interpret Results:**
   - Positive coefficients = factors that increase win probability.  
   - Negative coefficients = factors that decrease win probability.  
   - Use these insights to generate alerts and recommendations.

---

## Key Decisions
- **Model Choice:** Logistic regression was chosen over complex ML models because interpretability is more valuable than marginal accuracy gains.  
- **Focus on Business Drivers:** We prioritized actionable insights (e.g., Qualified stage is a bottleneck) over predictive metrics.  
- **Visualization:** Horizontal bar chart with color coding (blue = positive, orange = negative) makes drivers easy to interpret.  
- **Alert System Design:** Instead of building a full production system, we designed a lightweight architecture (Part 4) that could be productized.  
- **Reflection:** We acknowledged limitations (data quality, model drift, alert fatigue) and proposed next steps (qualification scoring, dashboard, feedback loop).

---

## Example Output

### Sample Importance DataFrame
```text
                 feature     coefficient
20  cat__deal_stage_Qualified   -0.165953
14     cat__lead_source_Partner -0.080635
3       cat__industry_HealthTech -0.051940
5             cat__region_APAC   -0.051277
1          cat__industry_EdTech  -0.044727
4           cat__industry_SaaS   -0.033975
9       cat__product_type_Core   -0.027545
11       cat__product_type_Pro   -0.025725
6             cat__region_Europe -0.022885
10  cat__product_type_Enterprise -0.014955
...
2       cat__industry_FinTech    +0.069473
17     cat__deal_stage_Demo      +0.037886
16     cat__deal_stage_Closed    +0.031087
15     cat__lead_source_Referral +0.030168
18     cat__deal_stage_Negotiation +0.026413
```

### Sample Visualization
A horizontal bar chart titled **Win Rate Drivers**:
- **Blue bars (positive):** FinTech industry, Demo stage, Referral leads, Negotiation stage.  
- **Orange bars (negative):** Qualified stage, Partner leads, APAC/HealthTech/EdTech industries, Core/Pro/Enterprise products.  
- Sorted from most negative to most positive coefficients.


This project delivers a **decision engine + system design** that helps sales leaders understand why win rates are dropping and what actions to take. It emphasizes **clarity, interpretability, and business usefulness** over technical optimization.
