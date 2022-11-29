# Decentralized Derivatives Liquidity

DDL is the Decentralized Derivatives Liquidity protocol for the creation of derivatives tokens protected by over-collateralization of external assets. DDL eliminates the necessity of orderbook and positions in derivatives exchanges by utilizing a split-collateral pool that balances between a fungible token with a target derivative value and a shared collateral pool.

DDL is the world's first derivatives protocol suitable for the cost and scarceness of blockchain resources.

<figure><img src="https://lh4.googleusercontent.com/b0Dk7w1SUXaqH2_0SqqXoXjLdc3N1jIGNgomQlsvCJlzzVioLfQGK0dtBO-8qRv8rQHmZ4JdPC8ZwLG4D1g_HbJFCcz1lkzfN5a8kUlHvOSA6FMMYf5_0DTp_gVUtYoQSjWUzNItv3HljB0XAhsMnj8" alt=""><figcaption></figcaption></figure>

## _Target Value (V)_

Target Value (**V**) is any value that is calculable on a smart-contract platform:

* asset price fed from off-chain oracles: USD, EUR, Nasdaq, GME, etc.
* asset price fed from on-chain DEXes: BTC, ETH, BNB, etc.
* derivatives: $$\frac 1 {BTC}$$, $$\log_2 {ETH}$$, $${BNB}^2$$, $$max(50k-BTC,0)$$, etc. log



<figure><img src="https://lh3.googleusercontent.com/JbvSi1zDdXVItP7wQkSIszckLZrvcN9yJtF9ospZhDtVDDhapZtVufGxrs4EzoS2op2EOnoFk7i9NNSUaHqJ-x2-BExXzez568FFck35pou-jNs6ymu_0XImQVMAl93SaF00AvkdWj7Q9x9ZlLvEihU" alt=""><figcaption></figcaption></figure>

## _Derivative Token (D)_

Derivative Token (**D**) is a fungible token that tracks a target value **V** and is over-collateralized by on-chain assets. While the implementation of token D is open entirely to the community, in order to use the DDL protocol, its contract must define:

* a function to calculate the market price $$P_{v/c}$$
* a set of collateral pools and their Distribution Rate Limits

The **DDL** protocol ensures that **D** can be burnt by anyone, anytime to redeem the exact value of collateral token **C** from its pools.

## _Collateral Asset (C)_

Collateral Asset **** (**C**) **** is an on-chain crypto asset used as collateral for Derivative tokens. **C** is locked in collateral pools to make sure the **D** can be burnt to redeem the same value of **C**. Collateral token can be:

* Crypto-asset: BTC, ETH, etc.
* Single asset yield-bearing token: stETH
* Pair asset yield-bearing token: Uniswap LPs
* Multiple assets yield-bearing tokens
* Any asset that can be liquidized on-chain with a contract call.

<figure><img src="https://lh6.googleusercontent.com/MkNd5ATszlk0xeil6WI-iHRWJ-xIezS14R9VmDnCMrBT0dOsLzx62NJVVdgDvudfGUA47r7Gx1YuszfztkuVD6PdnmqhvepAFA_tUVEdmbiuejh9hO9DJbksfN-AJslh-F_qODX1gT2XQfcYwihugY8" alt=""><figcaption></figcaption></figure>
