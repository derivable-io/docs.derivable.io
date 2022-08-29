# Liquidation

The liquidation transaction can be executed by anyone when the LR is broken (CR < LR). C is liquified so D can be bought from the market and burned until the LR is met again (CR ≥ LR). Each liquidation transaction must guarantee this condition: $$CR < {CR'} \le LR + \frac {LR - CR} 2$$.

An external DEX call is required to trade C to D from the open market. The default DEX is the contract configured by each pool that provides a convenient way to execute the liquidation transaction directly from a wallet, an explorer or a simple frontend dapp.

The liquidation transaction can also be executed with custom DEX contracts, allowing external actors to utilize advanced trade strategies to increase the return token amount. To guard against griefing attacks that intentionally do a bad trade, the default DEX in this case is used to make sure that:

$$
\tag {E0} \Delta_D \ge \delta_D
$$

$$\Delta_D$$the amount of D token returned from the custom contract passed to the transaction

$$\delta_D$$ the amount of D token returned from the default DEX contract

## Liquidation Fee

The liquidation fee provides the incentive for external actors to spend gas to execute the liquidation transaction. The amount returned by the default DEX has a lower rate (DLFR) to incentivize liquidators to aggregate the trade transaction for the most profit output.

$$
\tag {E1} Liquidation \thinspace \thinspace Fee = \delta_D*DLFR + (\delta_D-\Delta_D)*LFR
$$

## **Liquidation Debt**

After all of C in the pool is liquidated, the pool is shut down and no further interaction can be performed on it. If the supply of D minted by this pool still exists, it will be converted to DEPT of token D. DEPT of all pools of D is accumulated in the D contract itself, and introduces collateral damage to all other running pools.

$$
\tag {E2} \sum DEBT=sum \space of \space all \space DEBT \space for \space each \space liquidated \space pool \space of \space D
$$

## **Collateral Damage**

When the token D has ∑DEBT > 0, all other pools of D take the collateral damage pro-rata. The effective R’DC will be increased a portion of the debt over the total supply:

$$
\tag{E4} R’_{DC} = min(R_C, S_D* P_{V/C} * (1 + \frac {∑DEBT} {∑S_D}))
$$

## Distribution Rate Limit

Total D minted by a pool is limited to a portion of the total supply of D from all pools:

$$
\tag{E5} S_D\le \sum S_D * DRL
$$

DRL can be set to 1.0 to provide unlimited distribution. DRL is designed to manage the risk of each pool and limit the Collateral Damage inflicted to the market of a derivative D.

<figure><img src="https://lh3.googleusercontent.com/YAhRIaDUcjfU0R_3UQqbF-jQro2cuQLdKxCf-vrXd9_E6wwZJqQbUQQy36HKGAZYHXPFP8UDATDBnFob9KruhZdLWnzz--kZQnmCGa2v3ClxSrjt-ASlmLoWazlgXLuZHtD0xr1xk0yZtSFOclr4CIU" alt=""><figcaption></figcaption></figure>
