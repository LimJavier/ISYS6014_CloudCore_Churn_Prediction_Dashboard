# ISYS6014_CloudCore_Churn_Prediction_Dashboard
# CloudCore Retention Review Queue

A Streamlit dashboard for the CloudCore Networks Assessment 3 project. It shows customers ranked by the churn probability from our Orange Logistic Regression model, so the support team can decide who to review each week. It is decision support: a person reviews every flag, and nothing contacts a customer automatically.

## What's in this repository

| File | Purpose |
|---|---|
| Group_Dashboard (`.ipynb`) | Runs the dashboard in Google Colab through ngrok |
| `README.md` | These instructions |
| `cloudcore_churn_predictions.csv` | The Orange Logistic Regression model prediction output to feed into the dashboard | 
| `cloudcore_lr_coefficients.csv` | The Logistic Regression coefficients, used for the "main factors" |

## The two data files

```
cloudcore_churn_predictions.csv
cloudcore_lr_coefficients.csv
```

These are the exact file names `app.py` reads, so keep it the same. If your copy has a different name (for example `customer_churn_prediction.csv`), rename it, or change the file name in `load_data()` in `app.py`.

Put the CSV in the same folder as `app.py`. It needs these columns: `customer_id`, `company_name`, `region`, `support_tier`, `tenure_months`, `monthly_recurring_revenue`, `product_count`, `churn_flag`, `Logistic Regression`, `Logistic Regression (0)` and `Logistic Regression (1)`. Orange adds two extra header rows under the column names, and the app skips them.
If the coefficients file is missing, the app still runs, but the factors panel says "approximate" and uses a replica of the model instead of Orange's coefficients.

## Run in Google Colab

You need your own free ngrok account and token. Do not use anyone else's.

1. On GitHub, open the `Group_Dashboard.ipynb`, and download with the "Download raw file" button (the download icon at the top right of the file view). Treat the repository as a template: run your own copy in your own Colab and don't change the master files in the repository unless the team agrees.
2. Open Google Colab and upload the notebook you downloaded (File > Upload notebook). Then upload cloudcore_churn_predictions.csv (from the team's shared folder) to the Colab when asked to. The notebook can only see files that are in the Colab session.
3. Get your own ngrok authtoken from your ngrok dashboard.
4. Run the cells in order: install the packages, start ngrok, then start Streamlit. Open the public link that ngrok prints.

The notebook should read the token with:

```python
from google.colab import userdata
ngrok.set_auth_token(userdata.get("NGROK_TOKEN"))
```

## Using the dashboard

- Set the weekly review capacity in the sidebar. The queue is the top customers by churn probability.
- Filter by region and support tier, or search by customer name or ID.
- Select a customer to see their details, the main factors behind their probability, and to record a review decision.
- Tick "Show actual churn (history)" in the sidebar to compare the ranking with what actually happened in the historical data.

## Things to know

- The probabilities are out-of-fold predictions from the Orange model on the historical extract. They are not live scoring of new customers.
- The "main factors" use the coefficients of the Orange Logistic Regression, which were fitted on all customers, so they differ slightly from the out-of-fold probabilities. They show association in this data, not what causes churn.
- The review log lasts only for the current session. Download it before closing the app.
