---
layout: post
title: Monitoring Conduct Risk via FCA's Consumer Duty Using Python
image: "/posts/consumer-duty-title-img.png"
tags: [Conduct Risk, Consumer Duty, Python, Compliance Analytics]
---

This project uses Python to simulate and analyse a bank's compliance with the FCA's Consumer Duty outcomes. We use dummy data to mimic real-world challenges and apply analytical techniques to highlight areas of concern, particularly for vulnerable customers.

# Table of contents

- [00. Project Overview](#overview-main)
    - [Context](#overview-context)
    - [Actions](#overview-actions)
    - [Results & Discussion](#overview-results)
- [01. Concept Overview and Column Descriptions](#concept-overview)
- [02. Data Overview & Preparation](#data-overview)
- [03. Conduct Risk Analysis](#analysis)
- [04. Analysing The Results](#results)
- [05. Discussion](#discussion)

___

# Project Overview  <a name="overview-main"></a>

### Context <a name="overview-context"></a>

The FCA's **Consumer Duty** regulation requires UK financial institutions to ensure fair treatment of customers across four key outcomes:
* Products and Services
* Price and Value
* Consumer Understanding
* Customer Support

Banks must especially ensure vulnerable customers are not disadvantaged. Our task is to simulate and analyse a dataset to uncover possible conduct risk issues using python!

<br>
<br>
### Actions <a name="overview-actions"></a>

To monitor compliance, we:

* Generated dummy data with key conduct risk features.
* Focused on vulnerable customers and complaint rates.
* Compared communication clarity and support metrics.
* Used grouping to highlight risks.

<br>
<br>

### Results & Discussion <a name="overview-results"></a>

Key findings include:

* Vulnerable customers reported slightly higher communication clarity scores (3.04) than non-vulnerable customers (2.98)
* Despite higher clarity, vulnerable customers filed more complaints (~14.6% vs ~10%).
* Support response time was similar for both groups:
	* ~24.8 hours for customers who complained,
	* ~24.2 hours for those who didn’t.
	* Suggests that wait time alone does not explain complaints.
* Product-level analysis revealed that vulnerable loan customers had the highest complaint rate (20%), combined with slightly lower clarity scores.
* Vulnerable credit card customers had the highest clarity scores (3.17) and a lower complaint rate (12.2%) than their non-vulnerable peers (13.7%).
* Mortgage customers across both groups showed low complaint rates and solid clarity scores, suggesting a generally well-managed product experience.


These findings help simulate how a bank could monitor Consumer Duty outcomes using internal data.


<br>
<br>

___

# Concept Overview and Column Descriptions <a name="concept-overview"></a>

<br>
#### Conduct Risk

Conduct Risk refers to the risk that a firm's behaviour causes unfair outcomes for customers or undermines market integrity. It includes things like mis-selling, misleading information, or failing to act in the customer's best interest.

<br>
#### Consumer Duty

Consumer Duty, introduced by the FCA in July 2023, is a specific regulation that aims to tackle conduct risk by holding firms to a higher standard. It mandates that firms must deliver "good outcomes" for customers in four key areas:

* Products and Services
* Price and Value
* Consumer Understanding
* Customer Support

**In simple terms:**

>**The Consumer Duty is a formal framework introduced by the FCA to reduce conduct risk.**

This project uses simulated data inspired by Consumer Duty outcomes to analyse possible conduct risk exposures - especially for vulnerable customers. The columns in our dataset described below have been generated to help carry out this analysis:

<br>
**Customer ID**

A unique ID for each customer

<br>
**Product Type**

The type of financial product held by the customer - "loan", "credit_card", or "mortgage". THis helps us assess fairness across different offerings.

<br>
**Communication Clarity Score**

A score from 1 to 5 indicating how clearly the bank communicated with the customer. Lower scores reflect poor understanding, linking directly to the *Consumer Understanding* outcome.

<br>
**Support Response Time**

The time (in hours) it took for the bank’s customer support to respond. Delays here reflect potential failures in the *Customer Support* outcome.

<br>
**Complaint Flag**

A binary flag where `1` means the customer submitted a formal complaint. This is a critical indicator of dissatisfaction and conduct risk.

<br>
**Vulnerability flag**

A flag where `1` means the customer is considered vulnerable — due to age, disability, or financial hardship. Under Consumer Duty, firms are expected to pay special attention to these customers.

<br>
**Price vs Benchmark**

Shows how much more or less (%) the customer paid compared to an industry benchmark. Positive values mean the customer paid more — helping us analyze fairness under the *Price and Value* outcome.

By analysing these columns, we aim to identify patterns of potential unfairness or harm, particularly for vulnerable customers — fulfilling the spirit of both **conduct risk monitoring** and **Consumer Duty compliance**.



<br>
# Data Overview & Preparation  <a name="data-overview"></a>

To analyze how banks can monitor compliance with the FCA’s Consumer Duty, we need a dataset that reflects various aspects of customer experience. Since real customer data is confidential, we simulate a realistic dataset of **1,000 customers**.

The goal is to create data that allows us to explore:

* Communication clarity and support response times
* Complaint behavior across different customer types
* Whether vulnerable customers are treated fairly
* How prices compare to industry benchmarks

We'll use Python to randomly generate this data using NumPy and organize it into a pandas DataFrame — a powerful tabular structure ideal for analysis.

The following code performs these steps:
* Imports the necessary libraries (`pandas` for data handling, `numpy` for random generation)
* Sets a seed to ensure reproducible results
* Defines the number of customers to simulate (`n = 1000`)
* Randomly generates values for customer attributes such as product type, communication clarity, support response time, complaint flag, vulnerability status, and price deviation

We then preview the first few rows to verify the structure of our dataset.

<br>
```python

# install the required python libraries
import pandas as pd
import numpy as np

# Step 2: Set a random seed so that results are reproducible every time you run the code
np.random.seed(42)

# Step 3: Set the number of customers to simulate
n = 1000

# Step 4: Generate the dataset using pandas and NumPy
data = pd.DataFrame({
    "customer_id": np.arange(1, n+1),
    "product_type": np.random.choice(["loan", "credit_card", "mortgage"], n),
    "communication_clarity_score": np.random.randint(1, 6, n),
    "support_response_time": np.random.normal(24, 10, n).clip(1).round(2),
    "complaint_flag": np.random.choice([0, 1], n, p=[0.9, 0.1]),
    "vulnerability_flag": np.random.choice([0, 1], n, p=[0.85, 0.15]),
    "price_vs_benchmark": np.random.normal(0, 10, n).round(2)
})

# Step 5: Display the first 10 rows of the dataset to verify its structure
data.head(10)

```
<br>
A sample of this data (the first 10 rows) can be seen below:
<br>
<br>

**customer\_id**|**product\_type**|**communication\_clarity\_score**|**support\_response\_time**|**complaint\_flag**|**vulnerability\_flag**|**price\_vs\_benchmark**
:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:
1|mortgage|3|27.42|1|0|-7.03
2|loan|3|12.88|0|1|2.09
3|mortgage|5|32.02|1|0|16.62
4|mortgage|4|20.02|0|0|-3.32
5|loan|2|21.38|0|0|9.61
6|loan|4|18.92|0|0|5.69
7|mortgage|4|27.02|1|0|3.11
8|credit\_card|3|16.15|0|0|4.3
9|mortgage|4|33.01|0|0|-0.95
10|mortgage|1|22.61|0|1|0.35

___

<br>
# Conduct Risk Analysis <a name="Analysis"></a>

Now that we've prepared our dataset, we begin analyzing three key risk areas:

*Communication Clarity – Do vulnerable customers receive less clear communication?
*Complaint Rate – Are vulnerable customers more likely to file complaints?
*Customer Support Response Time – Do those who complain experience longer wait times?

We'll also **segment our findings by product type** to see if these risks are concentrated in specific products like loans, credit cards, or mortgages.

```python

# Average communication clarity score by vulnerability
clarity_by_group = consumer_duty_data.groupby("vulnerability_flag")["communication_clarity_score"].mean()

# Complaint rate by vulnerability group
complaint_rate = consumer_duty_data.groupby("vulnerability_flag")["complaint_flag"].mean()

# Average support time for those who complained vs those who did not
support_by_complaint = consumer_duty_data.groupby("complaint_flag")["support_response_time"].mean()

# Display the metrics
print("Average Communication Clarity by Vulnerability:\n", clarity_by_group)
print("\nComplaint Rate by Vulnerability:\n", complaint_rate)
print("\nSupport Response Time by Complaint Flag:\n", support_by_complaint)

```

<br>
#### Segmenting by Product Type

```python

# Complaint rate by product type and vulnerability
product_complaints = consumer_duty_data.groupby(["product_type", "vulnerability_flag"])["complaint_flag"].mean().unstack()

# Clarity score by product type and vulnerability
product_clarity = consumer_duty_data.groupby(["product_type", "vulnerability_flag"])["communication_clarity_score"].mean().unstack()

# Display results
print("Complaint Rate by Product and Vulnerability:\n", product_complaints)
print("\nClarity Score by Product and Vulnerability:\n", product_clarity)

```
___

<br>
# Analysing The Results <a name="results"></a>

Based on the metrics from our simulated data, we can draw the following insights:

### Communication clarity by Vulnerability

* Vulnerable customers have a slightly higher average clarity score (**3.04**) than non-vulnerable customers (**2.98**).
* This suggests that, in this simulation, **vulnerable customers are not receiving poorer communication**, and may even perceive it as slightly clearer. However, the difference is marginal and may not be statistically significant without further testing. 

### Complaint Rate by Vulnerability 

* Vulnerable customers have a complaint rate of **~14.6%**, compared to **~10%** for non-vulnerable customers.
* Despite having similar clarity scores, **vulnerable customers are more likely to file complaints**, indicating that other factors (e.g., pricing, support, complexity) may be contributing to dissatisfaction.

### Support Response Time by Complaint Flag

* Customers who **filed complaints** experienced an average support delay of **~24.8 hours**.
* Those who **did not complain** still had a support response time of **~24.2 hours**.
* This small difference suggests that **longer wait times may not be the main driver of complaints**, but may still contribute in specific cases.

### Complaint Rate by Product and Vulnerability 
<br>
<br>

**Product**|**Non-Vulnerable (%)**|**Vulnerable (%) **
:-----:|:-----:|:-----:
Credit Card|13.7%|12.2%
Loan |9.0%|20.0%
Mortgage|7.2%|9.8%

<br>
* Vulnerable customers with **loans** show the **highest complaint rate** (20%) — more than double their non-vulnerable peers.
* For **credit cards**, interestingly, **non-vulnerable customers complain more** than vulnerable ones.
* **Mortgage customers** show the lowest complaint rates overall.

**Insight**: The high complaint rate among vulnerable loan customers could reflect financial pressure, product complexity, or repayment struggles — a potential conduct risk area.

### Clarity Score by Product and Vulnerability 
<br>
<br>

**Product**|**Non-Vulnerable (%)**|**Vulnerable (%) **
:-----:|:-----:|:-----:
Credit Card|2.9%|3.2%
Loan |3.0%|2.9%
Mortgage|3.1%|3.1%

<br>
* Vulnerable customers with **credit cards** actually report **higher clarity** than non-vulnerable customers.
* In contrast, vulnerable **loan holders** report slightly **lower clarity**, which might correlate with their higher complaint rate.
* Mortgage customers report relatively high clarity scores across both groups.

### Summary:

* Vulnerability alone does not explain poor clarity — but may correlate with **higher complaint sensitivity**, especially in loan products.
* The Bank should pay closer attention to **vulnerable customers in the loan segment**, both in clarity and complaint patterns.

These insights show how targeted data analysis can help financial firms align with the FCA's **Consumer Duty** and proactively identify **Conduct Risk** hotspots.

___

<br>
# Discussion <a name="discussion"></a>

The analysis of our simulated customer data provides several insights into potential conduct risk areas that banks and other financial firms should be aware of under the FCA's Consumer Duty regulation:

### ***Clarity does not always predict complaints***

Vulnerable customers reported a **slightly higher average communication clarity score** (3.04) than non-vulnerable customers (2.98). This suggests that in this simulation, vulnerability did not correspond with lower perceived clarity. However, despite this, vulnerable customers still had a **higher complaint rate** (~14.6% vs ~10%).

**Interpretation:** Communication clarity alone does not explain complaint behaviour. Other factors like financial stress, product complexity, or perceived fairness may influence vulnerable customers more strongly.

### ***Support Time Differences are Marginal***

The difference in **support response times** between those who complained (~24.8 hours) and those who didn’t (~24.2 hours) was small.

**Interpretation:** Longer wait times may contribute to dissatisfaction, but they may not be the primary driver of complaints in this dataset. The root cause of complaints likely lies elsewhere — such as product experience or pricing.

### ***Risk is Concentrated in Vulnerable Loan Customers***

Segmenting by **product type** revealed that:

* Vulnerable **loan customers** had a very high complaint rate of **20%**
* They also had a slightly **lower clarity score** (2.9) compared to non-vulnerable loan holders (3.00)

**Interpretation:** This combination is a red flag. It suggests that vulnerable customers in the loan segment may be experiencing **communication gaps and dissatisfaction**, posing a **higher conduct risk** for the bank.

### ***Product Experience Varies by Customer Group***

* Vulnerable **credit card customers** surprisingly had **higher clarity scores** and **fewer complaints** than their non-vulnerable counterparts.
* **Mortgage customers**, regardless of vulnerability, had lower complaint rates and generally high clarity scores — indicating a more stable experience.

**Interpretation:** This shows that conduct risk is **not evenly distributed**. It's crucial for firms to monitor performance **by both customer type and product type**, not in isolation.


### ***Strategic Takeaways for Compliance & Risk Teams***

* **Proactively monitor** high-risk combinations (e.g., vulnerable + loan) using complaint and clarity metrics.
* Investigate beyond surface-level satisfaction — clarity doesn't always equal contentment.
* Consider **targeted interventions** for products or teams where complaint rates cross critical thresholds.

These kinds of insights — even when simulated — show the power of data-driven approaches in aligning with the spirit and letter of the FCA's **Consumer Duty** and in reducing **real-world conduct risk**.