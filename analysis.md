HOW ARE WALLETS SCORED?
We looked at real usage patterns and not just raw numbers. Some of the signals we considered were:
->How consistently a wallet repays its borrowed funds.
->How often it gets liquidated (a proxy for financial distress).
->How active it is, eg many tiny, repeated transactions may indicate bots.
->Whether it's just borrowing for short-term profit (and vanishing).
->Whether it's supplying more than borrowing (i.e., acting like a lender).

These features were then standardized (to normalize their scales), and passed into a KMeans clustering algorithm. KMeans helped us find natural groupings of wallets based purely on how they behave without external labels .
Once clusters were created, we ranked them based on overall responsibility indicators (e.g., high repayment ratio, low liquidation ratio, consistent use). These cluster rankings were then mapped to score bands between 0 and 1000,
allowing us to assign a DeFi credit score to each wallet.


THE SCORES-
Here’s how the wallets are distributed across different score ranges:

Score Range	# of Wallets
Credit Score Distribution:
score_range
0-99         1621
100-199       215
200-299       249
300-399       177
400-499       202
500-599       561
600-699        79
700-799        82
800-899        41
900-999       269
1000-1099       1
Name: count, dtype: int64

# Most wallets scored between 500-599 which suggests a healthy middle group with moderate risk and responsible behavior.


WHAT DO THESE SCORES SAY ABOUT USERS?
Here’s a breakdown of what different score brackets usually mean based on their behavior:

# 0–300: Caution Zone (Bots or Exploiters)
These wallets tend to borrow and disappear without repaying.
They’re often liquidated soon after borrowing.
Likely bots, flash loan attackers, or exploit hunters.

# 300–500:  Risky but possibly new
Some repayments, but not consistent.
May be learning or testing the platform.
They’ve had liquidations, but not excessively.
Could be beginners or sloppy borrowers.

# 500–700: The average users of Aave
These users repay most of what they borrow.
They might make mistakes occasionally, but not the risky kind.
They use Aave regularly and interact in healthy ways.
Probably everyday users or small-scale DeFi participants.

# 700–1000: Power users of Aave
These are professional DeFi players.
They repay everything and avoid liquidations altogether.
Active across multiple time windows and not just one-off transactions.
Often deposit more than they borrow —> strong signal of trustworthiness.

FURTHER ENHANCEMENTS
This unsupervised credit scoring model is a step towards a fair, transparent, and data-driven financial identities in DeFi. Having said this there’s still a lot of potential for growth.
for example:=

-> Special handling for wallets that haven’t borrowes since not all users are borrowers. Our next model must avoid penalizing them unfairly. Instead, if a wallet only deposits or redeems, 
assign a score based on its frequency of deposits, withdrawal behaviour etc. We could also treat lenders and borrowers as two separate behavior segments, and cluster them independently before scoring.

-> Using timestamps for behavior profiling like Recency Score (how active the wallet has been in the last N days), Activity Frequency or Time between borrow and repays.

-> Handling outliers more robustly and experimenting with other clustering algos(eg: HDBSCAN)

->We can also improve this using supervised learning if labels like “known bot” or “exploit” accounts are available.



