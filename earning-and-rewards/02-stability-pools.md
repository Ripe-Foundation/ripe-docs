---
description: Provide liquidation liquidity and receive claimable collateral
---

# Stability Pools: Provide Liquidation Liquidity

Stability pools let deposited assets provide in-protocol liquidity for compatible liquidations. When a pool is eligible and has capacity, some of its settlement liquidity can be exchanged for the borrower's collateral at the configured liquidation spread.

This can create attractive outcomes, but it is not guaranteed arbitrage. The received collateral becomes claimable custody, its market price can move, and a pool can be skipped when it is incompatible, reserved, full, paused, unpriced, or short of spendable liquidity. Auction settlement handles eligible collateral that the Stability route cannot absorb.

## The Core Proposition

### Passive Liquidation Participation

Unlike liquidation bots, Stability-vault depositors do not need to submit each purchase themselves:

* **Passive Participation**: Deposit once and automatically participate in liquidations
* **Share Accounting**: Settlement liquidity and active claim assets are reflected in cohort NAV
* **No Technical Barriers**: No bots, no gas wars, no timing games
* **Conditional Priority**: A compatible Stability vault is checked before auction fallback for collateral configured to use it

When Stability settlement occurs, part of the cohort's spendable settlement asset is exchanged for claimable collateral. That changes the composition of the position; it does not instantly convert the claim into base-asset cash or guarantee a realized profit.

### Multiple Revenue Streams

Depending on the deposited asset and current configuration, a position may have several sources of return:

1. **Underlying Asset Return**: [sGREEN](01-sgreen.md) can continue reflecting its underlying vault economics; an AMM LP token continues reflecting its pool share and fees
2. **Liquidation Spread**: A cohort may receive more oracle-valued collateral than the settlement value it supplies
3. [**RIPE Rewards**](03-ripe-rewards.md): A deposited Stability-vault position may earn configured protocol rewards

These sources have different risks and are not fixed or guaranteed. In particular, collateral prices can move after a liquidation, and claimable collateral is not the same thing as immediately withdrawable settlement liquidity.

## How Stability Pools Work

### Pool, LP Token, and Stability Vault Are Different

These three objects are related but not interchangeable:

1. **External AMM pool**: The trading venue where users supply a token pair and swaps generate AMM fees
2. **LP token**: The token representing a proportional position in that external AMM pool
3. **Ripe Stability vault**: A Ripe vault that accepts configured settlement assets, including an eligible LP token, and issues internal shares used for Ripe accounting and rewards

An LP token in your wallet still represents its AMM position and can continue accruing the AMM's trading fees. It does **not** earn Ripe Stability-vault rewards until it is deposited into Ripe.

### The Deposit Process

Ripe can configure different assets as Stability-vault settlement assets. Common designs include:

**GREEN-pair LP Tokens**

* Continue representing the underlying AMM position and its fee economics
* Become eligible for Ripe rewards only after deposit into the Ripe Stability vault
* Can be transferred to [Endaoment](../core-protocol/07-endaoment.md) when consumed in liquidation settlement, if configured that way

[**sGREEN**](01-sgreen.md) (Savings GREEN)

* Continues earning base yield in the pool
* Can be redeemed and burned when used as liquidation settlement liquidity

Your deposit is converted to Stability-vault shares representing a proportional claim on that settlement-asset cohort's NAV.

### The Liquidation Flow

When a borrower's position needs liquidation:

1. **AuctionHouse checks eligibility**: The collateral must permit Stability settlement and the vault must accept it as a claim asset
2. **The vault checks capacity**: It needs unreserved settlement custody, a usable price, and room for the claim asset
3. **Settlement changes pool composition**: Settlement liquidity leaves or is burned; liquidated collateral enters custody as a claim for the cohort
4. **Debt receives credit**: Only value actually supplied by the Stability vault reduces the borrower's liquidation target
5. **Auction fallback remains**: Any configured auction-eligible remainder can proceed to a Dutch auction

Capacity can decline during liquidations as spendable settlement liquidity becomes claimable collateral, claim-asset slots fill, or custody becomes reserved. A later liquidation can therefore use less Stability liquidity—or skip the vault entirely—even if an earlier one was absorbed.

### USD Value-Based Accounting

Unlike simple token vaults, stability pools use sophisticated USD value-based share accounting:

* **Share Price = Cohort NAV / Total Shares**
* **Cohort NAV = Unreserved settlement-asset custody + Valued active claim assets**
* **Your Value = Your Shares × Current Share Price**

“Unreserved” matters: custody already owed as a claim to another cohort is not counted as spendable settlement liquidity. Active claim assets are included in NAV using current prices, so share value can rise or fall with those assets.

A claim asset can also be **dormant**: the vault retains its custody and liability, but it is not seated in the active claim list—for example, when a new balance is below the activation floor. Maintenance can activate or prune entries only under bounded conditions. Dormant balances are not silently treated as free settlement liquidity.

## The Economics of Liquidation Outcomes

### How Liquidation Spreads Affect Your Position

The configured liquidation spread determines how much collateral value the cohort receives relative to the settlement value it supplies. It is only one part of the depositor's eventual return:

* The collateral remains exposed to price changes after settlement
* Claiming the collateral realizes a token position, not a guaranteed base-asset profit
* Withdrawing the original Stability asset depends on how much unreserved custody remains

The spread can provide a cushion, but it does not guarantee profitability during volatile markets.

### Real-World Scenarios

**During Market Volatility**: Liquidations may increase as prices swing, creating more settlement opportunities but also increasing the price risk of claim assets.

**In Stable Markets**: Fewer liquidations may occur. Underlying asset returns and RIPE rewards, when configured, remain separate from liquidation outcomes.

**Portfolio Effect**: As liquidations occur across different collateral types, the cohort can accumulate multiple claim assets. That is changing exposure, not automatic diversification or profit.

## Advanced Features

### Claiming Liquidated Collateral

After liquidations, you can claim your proportional share of accumulated collateral:

* **Flexible Claims**: Choose which assets to claim and when
* **Share Settlement**: Claiming burns the shares corresponding to the value delivered
* **Auto-Deposit Option**: Claimed assets can automatically enter Ripe deposit vaults
* **Batch Claims**: Multiple claim assets can be requested in one transaction, each with its own maximum USD value
* **Configurable RIPE Rewards**: A claim may receive locked RIPE when the claim-reward configuration and reward budget allow it

**How to Claim:**

For each claim, you specify:
1. **Stability Pool Asset**: Which configured settlement-asset cohort supplies your shares (for example, sGREEN or an eligible GREEN-pair LP token)
2. **Claim Asset**: Which available liquidated collateral you want to receive
3. **Maximum USD Value**: Cap on how much to claim (or max for full claim)

The protocol caps each result by your shares, available claim custody, the requested maximum, and a usable price. Invalid or unavailable entries in a batch can be skipped; the batch succeeds only if at least one claim transfers value. The vault checkpoints the affected Stability cohorts after all mutations are complete.

Only an **active** claim asset appears in the normal claim/NAV list. A nonzero **dormant** claim balance remains tracked as a custody liability but may require maintenance before it can become an active claim. Dormant does not mean unowned or available for the vault to spend.

**Delegation**: Others can claim on your behalf if you've granted `canClaimFromStabPool` permission in your delegation settings. This enables automated claim strategies.

#### Claim Incentives

A successful claim can earn a RIPE reward under the protocol-wide claim configuration:

* **Value-Based Calculation**: The configured RIPE-per-dollar rate is applied to the total USD value successfully claimed
* **Budgeted Distribution**: The result is capped by the RIPE reward budget; a zero rate or exhausted budget produces no reward
* **Governance Deposit**: Awarded RIPE is minted and deposited into the current core [governance vault](../governance-and-economics/02-governance.md) with the configured claim-reward lock

The reward can encourage users to remove claim collateral, but it is not dynamically raised per asset when a pool needs rebalancing. A claim also does not replenish the original Stability asset; new deposits or other configured flows are still needed to rebuild settlement liquidity.

### GREEN Redemption Mechanism

Stability pools can also provide a GREEN redemption path. When redemption is enabled for the vault, asset, and recipient—and usable pricing and claim custody are available—an eligible caller can exchange GREEN for available claim collateral:

1. **GREEN is the debt-value input**: The vault treats GREEN as a $1 unit for the redemption calculation
2. **Configured collateral pricing determines output**: The claim amount is bounded by the requested GREEN, available GREEN, asset pricing, and claim custody
3. **Pool composition changes**: GREEN value replaces the redeemed claim collateral; an sGREEN cohort deposits the incoming GREEN into sGREEN, while other cohorts record it as GREEN claim custody

This conditional arbitrage path can support GREEN's peg, but it is not an unconditional right to redeem any amount or asset and does not guarantee depositor value.

### Multi-Asset Accumulation

Over time, a Stability cohort can accumulate several configured claim assets from compatible liquidations. GREEN received through pool redemptions changes custody in the opposite direction by replacing redeemed claim collateral; for an sGREEN cohort it is converted into sGREEN instead of becoming a GREEN claim. The actual asset mix is configuration- and activity-dependent, and each claim asset brings its own price and liquidity risk.

## Why Participate in Stability Pools?

### For Yield Seekers

* **Multiple potential return sources** from underlying economics, liquidation outcomes, and configured rewards
* **Passive settlement exposure** without submitting each liquidation transaction yourself
* **Liquidation exposure** through configured settlement spreads, with corresponding market risk

### For Risk-Conscious Users

* **Conditional settlement-route priority**: an eligible Stability vault is checked before ordinary auction fallback
* **Flexible claims and withdrawals**, subject to available custody and configuration
* **Protocol protection** role enhances system stability

### For GREEN Ecosystem Supporters

* **Strengthen the protocol** by providing liquidation liquidity
* **Potentially earn configured rewards** while supplying eligible settlement liquidity
* **Accumulate governance points** when claim rewards are deposited into the configured governance vault

## The Liquidation Game, Simplified

For collateral configured to use Stability settlement, an eligible vault is checked before auction fallback. Some events can be absorbed completely, some only partly, and some not at all.

When settlement occurs, your cohort exchanges liquid Stability assets for claimable collateral at the configured spread. Its NAV includes valued active claims, but that NAV is not a promise of immediate withdrawal in the original asset.

No bots. No gas wars. No coding required.

Deposit with the full position in mind: underlying asset economics, RIPE rewards when configured, liquidation-claim exposure, and the liquidity available when you later withdraw or claim.

***

_Provide liquidation liquidity, and understand what the vault may hold in return._

_For technical implementation details, see the_ [_StabilityPool Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core-lending/stabilitypool)_._
