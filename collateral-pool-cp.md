---
description: >-
  Collateral Pool is a smart contract that holds the collateral token C in
  reserve and is allowed by D to mint on its behalf
---

# Collateral Pool (CP)

## _**Split-Collateral**_

The **C** reserve is split into 2 virtual pools: Derivative Collateral and Counter Collateral. The **DC** pool always covers the same market value of the Derivative token supply mint by this pool. The **CC** pool takes the rest of the **C** reserve to absorb all the market volatility of **V** against **C**, providing the over-collateral property for the Derivative asset.

<figure><img src="https://lh5.googleusercontent.com/lGd6FBXYNREKIkZO1TbWugjAKLTabE3MXxexp-7hnUJNCBQgrQ4vL95-oEGmEOGWUIutJvDZKbn6sklyUciG22NjVBlz9O_TQAgyLyPylzbahgOOrWmbdchSXWOidD9D5MtvjT3j-nyRJWxUxXe2dpI" alt=""><figcaption></figcaption></figure>

With:

* $$R_c$$ is the total reserve of C in the pool
* $$S_D$$ is the supply of D minted by this pool
* $$P_{v/c}$$ is the current market price of V/C

We have the reserve of C for Derivative Collateral:

$$
\tag {E0}   R_{DC}=min(R_C,S_D*P_{v/c})
$$

And the reserve of C for Counter Collateral:

$$
\tag {E1} R_{CC}= R_C-R_{DC}
$$

### Market Movement

The market volatility of **V** against **C** will shift the balance between DC and CC as the total reserve of C in the pool is unchanged.

<figure><img src="https://lh5.googleusercontent.com/4glp5x5XTON2Fr-PPLNzW8vmTVSxKSRD8aZ3u1jpoDayroC0X1ilSlRkJQl4j96fxCHLDwnRPNb5nmlCEwocRP8-yqICrMSx319FJBPYnzlv49XBNvzP7BHxmh6jIuWTN5-c7PJJbcH6pqB8bj-iH7w" alt=""><figcaption></figcaption></figure>

The Counter Collateral is contracted and expanded to absorb all market fluctuations of V/C, giving Collateral Provider an elevated exposure to the market of C/V.

## _Pool Interaction_

Collateral Pools are smart contracts created by **DDL** protocol using a pre-defined PoolFactory contract. The next part is the first version of PoolFactory.

Collateral Pool allows anyone to:

* lock **C** to mint **D** (if the Distribution Rate Limit is not broken)
* burn **D** to unlock **C** (if the Target Collateral Ratio is not broken)
* lock **C** to mint **CP** (collateral provider token)
* burn **CP** and unlock **C** (if the Collateral Ratio is not broken)
* liquidize **C** if the Liquidation Ratio is violated

<figure><img src="https://lh3.googleusercontent.com/8EEYyzko_usYd9qm-YHIg_2caKeA1bKyljFvknntwfb3H8w4qH0RbX-61psZEYLgUjvxmdjxMGuHzywcxUXhXQxD1ZSQg0JCSuQcP5Tp_tNYvt42xu_ZBUpM_pDSidAe_wLlWEOwVicT3Y91WPsJeyU" alt=""><figcaption></figcaption></figure>

### Collateral Actions

#### Create Pool

A pool is created for the first time an amount of **C (𝚫C)** is added. The pool is initialized with:

&#x20;                                                           $$\tag {E2} \Delta_{CP}=S_{CP}=R_{CC}=\Delta_C$$​

$$\Delta_{CP}$$ is the amount of **CP** token received by the pool creator, (100% share of the pool).

$$S_{CP}$$ is the supply of the **CP** token of this pool.

$$R_{CC}$$ is the initial reserve of the Counter Collateral pool.

New pools can be created by anyone, but to mint the D token, pool addresses must be approved by the D token contract. Pool configuration and governance (or the lack thereof) is entirely open to the implementation of the derivative token contract.

#### Add Collateral

Locking $$\Delta_C$$ collateral to the pool will mint $$\Delta_{CP}$$ of CP token.

$$
\tag{E3} \Delta_{CP}=S_{CP} * \frac {\Delta_C} {\Delta_{CC}}
$$

#### Remove Collateral

Burning $$\Delta_{CP}$$ amount of **CP** token allows users to unlock $$\Delta_C$$ from the pool.

$$
\tag {E4} \Delta_C=R_{CC}*\frac {\Delta_{CP}} {S_{CP}}
$$

### **Derivative Actions**

#### Mint Derivative

Derivative token D can be minted by locking the same market value of C as Derivative Collateral.

$$
\tag {E5} \Delta_D=\Delta_C/P_V/C
$$

#### Burn Derivative

Collateral can be unlocked from Derivative Collateral by burning the same market value of D.

$$
\tag {E6} \Delta_C=\Delta_D*P_V/C
$$

#### Conversion Fee

Minting and burning D both take a conversion fee in C. This fee is left in the Counter Collateral pool for the CP as the service fee. The conversion fee rate is defined by each pool and contains FeeBase and FeeRate.

$$
\tag {E7} Fee=FeeBase + FeeRate * \Delta_C
$$

## **Pool Configs**

Pool configurations are immutable for each collateral pool, and are set on pool creation:

* Target Collateral Ratio (1 < TCR)
* Liquidation Ratio (1 < LR < TCR): the minimum collateral ratio before the liquidation (default = √TCR)
* Default DEX
* Default Liquidation Fee Rate (DLFR), Liquidation Fee Rate (LFR)
* FeeBase, FeeRate

The pool address is deterministically generated using:

* PoolFactory contract
* Derivative Token address
* Collateral Token address
* Packed encode of pool configs



## Collateral Ratio

At any point in time, the pool has an active Collateral Ratio of:

$$
\tag {E8} CR=R_C/R_{DC}
$$

CR is a crucial value to the DDL protocol, many of the user interactions are open, restricted, incentivized, or disincentivized depending on the current value of CR.

* D cannot be minted to make CR < TCR
* C cannot be unlocked to make CR < TCR
* Mint Fee is discounted when CR > TCR
* Burn Fee is discounted when CR < TCR
* When the CR < LR, the Pool Liquidation is available for everyone

## Fee Discount

**Conversion Fee Discount** is an incentive for the market to move back to the stabilized state, where the Collateral Ratio (**CR**) is close to Target Collateral Ratio (**TCR**).

* Stabilization Ratio: x = CR/TCR
* Fee Rate Factor: z = (x-TCR) / (TCR-LR)
* Minting Fee Rate = 1/(1+z) for {x >= TCR}, otherwise = ∞
* Burning Fee Rate = 1/(1-z) for {x <= TCR}, otherwise = 1

<figure><img src="https://lh4.googleusercontent.com/sNbGjkh8U69pGFHXtQRMfLEd0MlJ67J1p_7IsHGewLZaRCl2HZKvd26eIGNh1A-5jwINEkbHkQQ2HboAUnszH-pGvWFhDFwjsPGknUxOTciNpzNmXX6dN90qhi98cr_yh3_xPaC-TxnqJRdnx5Of058" alt=""><figcaption></figcaption></figure>
