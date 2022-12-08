# Deleverage

When the pool is under-collateralized, there are three choices for a Derivative pool to go:

* (A) keep running and letting a bank run ensures, whoever withdraws first gets their fund back until the pool is empty, whoever hasn't withdrawn loses everything.
* (B) stop working and split the C reserve with the D holders pro-rata, CPs lose everything. Users will need to move their funds to other new pools to gain Derivative exposure again.
* (C) deleverage the Derivatives and keep the pool running.

(A) can be used if there’s a certainty that the pool will NEVER be under-collateralized, either via some centralized multi-organization contracts, or some insurance funds can cover the risk.

(B) is a simple and fair solution, but not the most optimal for user experience.

(C) is a technical improvement solution of (B), and is used by Power Perpetual.

<figure><img src="https://lh3.googleusercontent.com/QMOiBi9THlxH_S2CfmmWGq_CNOKiSEFH54kPOSnubXV9rhdY2Mpgia7viXyA6I9vcUvPgF8aoGGwl9Fi1EzDZwySOXsx7HyyVkIWr6wNG_ffjDABrdS6OlKfbEKwtaCL7Us381sWZ_JdhSfjsAgllwZhw2i8OKHDtIp8hUwpPbDP6TCTmzihg2BrLDINpQ" alt=""><figcaption></figcaption></figure>

Deleveraging allows users to keep holding their Derivative tokens without the risk of bank runs, or the cost of moving funds from pool to pool. In the event of Deleverage, the pool state will be rolled back to the time market state where the Derivative token's capitalization crosses the collateral reserve.

<figure><img src="https://lh5.googleusercontent.com/wto6I3r-3RGOBuSYZnYHCNBnpmepIckaURhl_locLmv3FUucMq8EaYzc8G2zQn5lW97LHeVV_p_j4lei0huRGdOOvCVpswZgcvIHvFJeFBxQPF-DCyiakKSHe0j_kYYapmToJzprAmn1OrNrkCzw5z2SWJNA7-qPj_kiApuPg1-viOG4B6nJ4fmItYGe3Q" alt=""><figcaption></figcaption></figure>
