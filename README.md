# Credit-Score-Model-for-DeFi-

Objective->
To assign credit scores (0–1000) to DeFi wallets on the Aave V2 protocol based on behavioral patterns from on-chain transaction history. Higher scores indicate reliable and responsible usage; lower scores reflect risky, bot-like, or exploitative behavior.

Data & Processing->
/Input: A JSON dataset containing ~100K raw transaction records with fields like action, amount, assetSymbol, assetPriceUSD, timestamp, etc.
/Preprocessing:
1.Parsed JSON to a DataFrame.
2.Normalized the amount by token decimals and assetPriceUSD to compute USD values.
3.Extracted timestamp-based features like days_active_span, first_txn, etc. to understand the wallets better.
4.Aggregated user actions (total deposits, borrow, repayment, redeem, etc.) into wallet-level summaries.

Feature Engineering->

1.repay_to_borrow	= Total amount repaid / borrowed  => A high ratio here is a great sign as it means the wallet actively repays its debts, possibly even ahead of time. However, a low value is a red flag —> they borrowed, but barely repaid.

2.liquidation_ratio=	Liquidation count / total borrows => Being liquidated is like getting margin called; the wallet is too risky or mismanaged. thus, the higher this ratio, the more reckless or vulnerable the wallet seems.

3.total_borrow_usd	= Sum of all borrow actions in USD => It gives us an idea of how much risk the user is taking on.

4. n_total_txn= Number of transactions by the wallet => more transactions can mean active participation and maturity, while just a few one-off actions may help in signalling bots or testing wallets.

5. days_active_span=	Days between first and last transaction =>

6. Other calculated ratios & stats: How often the user deposited versus borrowed, How many times they redeemed collateral, Average asset values they interacted with, Any unusual spiking behaviors or low-activity patterns


 ML Model->
 
Algorithm: KMeans Clustering :- Clusters wallets based on behavioral patterns. StandardScaler applied to normalize features.

n_clusters = 100 to ensure granularity across score buckets.

Scoring Logic
After clustering, each cluster is evaluated by its mean repay_to_borrow and liquidation_ratio.
A score index is computed:- score_index = repay_to_borrow - liquidation_ratio
Clusters are ranked by this index.

Credit scores are linearly mapped from 1000 (best) to 0 (worst) and assigned to all wallets in each cluster.

Wallet Score Analysis
Distribution: Credit scores are binned into ranges (e.g., 0-100, 101-200, ..., 901-1000) and plotted.
-Most wallets fall in mid or low tiers with limited high scorers — consistent with DeFi's high-risk user base.

Behavior by Range
Metric	                  Lower Score Wallets	              Higher Score Wallets
repay_to_borrow	          Low (often < 1)	                  High (closer to 1 or >1)
liquidation_ratio          	High	                          Near zero
n_total_txn              	Fewer txns	                     More consistent history
days_active_span	      Short (e.g., a few days)            	 Long (e.g., >30 days)

=> Low Score Wallets: More likely bots, exploiters, or over-leveraged users.

=> High Score Wallets: Long-term participants, regular repayments, fewer liquidations.

Outputs->
wallet_scores.json: List of wallets and their final credit scores.



Visuals: Score distribution graph, behavioral boxplots.
