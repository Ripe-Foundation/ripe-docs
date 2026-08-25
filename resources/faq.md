---
description: Quick answers to what actually matters.
---

# Frequently Asked Questions

## Getting Started

### What is Ripe Protocol?

One loan backed by a portfolio of supported collateral. While other protocols may require separate debt positions, Ripe combines configured assets into ONE position with weighted terms and lets you borrow GREEN against their combined borrowing power.

### What makes Ripe different from other lending protocols?

**One loan, many supported assets.** Ripe lets configured collateral assets work together to back a single GREEN [borrowing position](../core-protocol/02-borrowing.md). The architecture can add more asset types through compatible vault and pricing modules; [RIPE Params](https://params.ripe.finance) shows what is currently configured.

### What is GREEN?

[GREEN](../core-protocol/01-green-stablecoin.md) is Ripe's overcollateralized stablecoin, designed to target $1. You create GREEN by borrowing against supported collateral under configured debt terms. A standard GREEN or sGREEN repayment burns the GREEN applied to debt; separate deleverage and Stability-settlement routes can instead transfer a configured non-GREEN asset for debt credit.

### Is my money safe?

Your [deposits](../core-protocol/03-collateral-assets.md) back your own loan rather than funding a shared borrower pool. The protocol uses separate redemption, deleverage, Stability-settlement, and [liquidation](../core-protocol/04-liquidations.md) paths to manage unsafe debt. Liquidation targets restored health, but it can consume all eligible collateral when the shortfall requires it. Smart-contract, oracle, market, liquidity, and bad-debt risks still remain.

## Borrowing Basics

### How much can I borrow?

Each supported collateral asset has configured Loan-to-Value (LTV) terms. Your total [borrowing power](../core-protocol/02-borrowing.md) combines the value of your deposits using those individual terms. See [RIPE Params](https://params.ripe.finance) for the current assets and LTVs on each network.

### How do interest rates work?

You pay a single weighted rate based on your [collateral mix](../core-protocol/03-collateral-assets.md), plus any applicable dynamic adjustment. The debt stores its terms between updates; protocol actions that refresh those terms can apply the latest collateral weighting and sustained GREEN-pool signal.

### What are dynamic rates?

Dynamic rates are a protective mechanism for sustained GREEN imbalance. The protocol builds a duration-weighted ratio from chronological reference-pool snapshots, and each interval uses the lower of two consecutive readings so one spike cannot activate the mechanism by itself. Above the configured danger threshold, a ratio boost and a sustained-danger boost are added to the weighted base rate, subject to a maximum-rate cap. Danger history clears only after consecutive safe observations cover the same configured freshness interval used by the snapshot mechanism. See [Dynamic Interest Rates](../core-protocol/02-borrowing.md#dynamic-interest-rates) for details.

### Can I repay early?

There is no maturity date or prepayment penalty. Partial and full repayment are supported, subject to Teller and Ledger availability, account locks, token controls, and the ordinary protocol checks described below.

## Managing Risk

### When do I get liquidated?

A position becomes eligible for [liquidation](../core-protocol/04-liquidations.md) when its collateral value reaches or falls below the required minimum for its debt. A permissionless caller must then submit an eligible liquidation transaction. There are three key thresholds to monitor (values vary by asset risk):

1. **Max LTV**: Your borrowing limit; you cannot increase debt beyond it
2. **Redemption threshold**: The zone in which GREEN holders may redeem against eligible collateral
3. **Liquidation threshold**: The point at which a liquidation episode may begin

**Illustrative example**: With $6,000 debt and an assumed 90% liquidation threshold, the position needs more than approximately $6,667 of collateral ($6,000 ÷ 0.90) to remain outside liquidation eligibility under the example terms. At or below the exact threshold, it is eligible.

Monitor your position and add collateral or repay debt before reaching these zones. For a visual guide showing all risk zones, see [How Thresholds Work Together](../core-protocol/02-borrowing.md#how-thresholds-work-together-a-visual-guide).

### What happens if one of my collateral assets cannot be priced?

If debt-bearing collateral has no usable price or backing amount, Ripe places the account in a valuation quarantine. The unavailable asset contributes no borrowing power; new borrowing and debt-supporting withdrawals are blocked, while new liquidation, redemption, and deleveraging passes are withheld because they cannot be priced safely. Repayment remains the dependable recovery action, subject to its normal controls. Ordinary Teller deposits and withdrawals also run strict debt housekeeping and can revert while the debt-bearing asset remains unpriced, so adding collateral is not a promised outage-recovery path. Quarantine is not an automatic liquidation or an insolvency determination. Normal health checks resume after pricing or backing recovers.

### What's the difference between redemption and liquidation?

**Redemption** (a separate permissionless debt-reduction path):

* No penalty or discount
* GREEN is treated as a $1 debt-value input and exchanged for a conservatively calculated, vault-credited amount of eligible collateral
* Can remain available below the liquidation threshold until a liquidation episode actually begins
* Is not an automatic phase inside a liquidation transaction

**Liquidation** (available at the liquidation threshold):

* Incurs configured liquidation fees
* Tries an eligible Stability Pool before starting a Dutch auction for remaining eligible collateral
* Targets restored health and may be partial, but can use all eligible collateral when the shortfall requires it

Borrower addresses registered as Underscore Earn vaults are excluded from ordinary credit redemption and AuctionHouse liquidation; merely depositing an Underscore share token does not give an ordinary borrower that exception.

### How does partial liquidation work?

Ripe calculates a target repayment intended to restore the account to a safer LTV. For example, if a hypothetical account needs only a 20% debt reduction and enough eligible collateral can settle it, the remaining collateral stays in the account. Severe undercollateralization can make the target equal to all debt and can consume all eligible collateral, so partial liquidation is a design goal rather than a guarantee.

## Earning with Ripe

### What is sGREEN?

[sGREEN](../earning-and-rewards/01-sgreen.md) is yield-bearing GREEN that captures the configured share of borrower revenue sent to the savings vault. Your sGREEN balance is non-rebasing; its GREEN conversion value reflects the vault's backing per share. That backing can increase through:

* Borrower interest payments
* Origination fees from new loans
* Protocol revenue distributions

No separate revenue claim is needed. Returns still depend on borrower activity, the configured revenue split, GREEN backing, and the token and vault controls.

### How do stability pools work?

[Stability pools](../earning-and-rewards/02-stability-pools.md) can hold configured settlement assets such as sGREEN or a GREEN-pair LP token. When a compatible liquidation uses the vault, settlement liquidity is exchanged for claimable collateral at the configured spread. As a depositor, you can:

* Retain the underlying economics of the configured settlement asset—for example, sGREEN backing growth or an LP token's AMM exposure
* Gain exposure to claimable liquidation collateral
* Earn RIPE rewards when the position is configured for them
* Withdraw settlement liquidity when sufficient unreserved custody is available

It's like being a liquidator without running any bots.

### How do I earn RIPE rewards?

[RIPE rewards](../earning-and-rewards/03-ripe-rewards.md) accrue from a configured, allowance-capped RIPE-per-block rate. Authorized governance paths administer the category and asset weights stored in Mission Control, allocating entitlement among borrowers, stakers, general depositors, and vote-selected deposits. General-depositor USD points accrue for an asset only when that asset has no staker-points allocation. Your share is based on your points relative to the applicable pool.

RIPE is minted when a claim consumes the accrued entitlement, not on every block. A configured portion of a normal claim can be deposited into the current core governance vault with a configured reward lock; **Stake All** deposits the entire claim. See [RIPE Params](https://params.ripe.finance) for live reward settings.

## GREEN Stability

### How does GREEN maintain its $1 peg?

Six mechanisms work together:

1. **Overcollateralization**: Borrowing is limited by the configured terms of deposited collateral
2. **Dynamic rates**: Borrowing costs can increase when sustained, corroborated reference-pool observations cross the configured danger threshold
3. **Direct redemption**: Eligible GREEN can be exchanged for oracle-valued collateral when a position and all redemption checks qualify
4. **Stability pool redemption**: A separately configured path through available claim collateral
5. [**Endaoment**](../core-protocol/07-endaoment.md) **operations**: Authorized, bounded treasury actions can adjust configured liquidity positions
6. **Peg Stability Module (PSM)**: Optional reserve-backed conversions with independent mint and redemption controls

### What happens if GREEN trades below $1?

Available mechanisms may create an arbitrage path:

1. Buying GREEN cheap on a DEX (e.g., $0.97)
2. Using direct redemption when an eligible debt position exists, or the PSM when its redemption direction and reserve capacity are available
3. Comparing the resulting oracle/par quote, fees, and execution costs with the market price

Profitable execution creates buying pressure that can support GREEN's return toward its target. The PSM is separate from debt-position redemption, but it remains subject to its configured controls and reserves.

### Can GREEN lose its peg permanently?

GREEN targets $1, but no smart-contract mechanism can guarantee a market price or recovery time. Overcollateralization, dynamic rates, eligible redemptions, Stability pools, authorized Endaoment operations, and the optional PSM can create corrective incentives. Their effectiveness depends on collateral value, usable pricing, liquidity, reserve capacity, configuration, caller activity, and market demand.

### What is the PSM and how do I use it?

When enabled, the Peg Stability Module (PSM) converts between GREEN and one immutable, six-decimal reserve token around a $1 reference. Ordinary-user quotes use the more conservative of the reserve asset's oracle value and par, then apply any configured fee:

* **Mint GREEN**: Send the reserve stablecoin and receive GREEN, or request delivery through sGREEN; if the calculated output is not greater than one GREEN, the PSM delivers GREEN instead of wrapping it
* **Redeem GREEN**: Pay with GREEN or sGREEN and receive the reserve stablecoin

**Why use it?**

* Arbitrage when GREEN trades off-peg
* Get GREEN without borrowing
* Exit GREEN to the configured reserve asset without a DEX trade

**Ordinary and recognized-vault paths:** Minting and redemption can be enabled independently. An ordinary path can be subject to its direction-specific allowlist when enforcement is enabled; the configured fee and interval capacity apply. When the **recipient** is a recognized Underscore vault, those ordinary allowlist, fee, and interval controls are bypassed. Recognized-recipient minting is still bounded by the submitted input and conservative lower-of-oracle-and-par sizing, but not by PSM reserve inventory; recognized-recipient redemption uses the higher of the oracle-derived amount and par and still requires sufficient reserve liquidity. See [RIPE Params](https://params.ripe.finance) for current onchain availability and terms.

## Technical Details

### Why did my transaction fail?

When the last-touch guard is enabled, routed user actions update that user's last-touch marker. A higher-risk action checks that the user has not already been touched in the current action block, so it can fail after either a lower-risk or another higher-risk action in that same action block. Lower-risk actions do not perform the uniqueness check themselves, and approved protocol integrations may be exempt. Wait for the next action block before retrying a protected action.

The action-block identity is a Ledger deployment choice: it can use the chain's normal `block.number` or a configured chain-specific source such as an Arbitrum child-block counter. This is separate from the protocol's economic block clock. Snapshot durations, borrowing intervals, auctions, and reward accounting continue to use the EVM `block.number`, so their timing must not be inferred from the action-block cadence.

This rate limiting:
* Restricts protected same-user action sequences within one action block
* Makes the relevant ordering rule explicit and deterministic
* Protects protocol state consistency without changing the economic block clock

### What is account locking?

Account locking is an emergency security feature enforced by Teller-routed user actions for a specific wallet address. If your account is locked, those routes cannot deposit, withdraw, borrow, or repay for the account.

Direct functions outside Teller have their own permission and pause boundaries. For example, direct PSM conversions and Reserve Engine acquisitions are not governed by the Teller account-lock check merely because the same wallet calls them.

Account locking is therefore a scoped Teller control, not a universal wallet freeze across every Ripe contract.

### Can the protocol be paused?

Ripe has component-level pause and availability controls rather than one universal switch that halts every operation:

* **Per-contract pausing**: Specific functions (deposits, borrows, liquidations) can be paused independently
* **Separate boundaries**: Pausing acquisitions, claims, Teller actions, or a price source can have different effects; a price-source pause freezes its configuration and update operations but does not by itself remove the source from reads
* **Mint circuit breaker**: RipeHQ's protocol-wide mint flag can stop authorized RIPE or GREEN minting, but it is not a global pause for transfers, repayments, or every other action
* **Administration**: Configured governors act through the relevant contract, Switchboard, and timelock paths

Always identify which component and direction a pause controls; do not infer that every protocol route shares the same state.

## Advanced Features

### Can I use yield-bearing tokens as collateral?

When a yield-bearing token is supported through a compatible vault and price path, its external share or exchange-rate economics can continue while it is deposited. Support is asset-specific rather than automatic for every yield token.

### Can I use tokenized real-world assets?

Ripe's vault architecture can support tokenized real-world assets when governance configures compatible custody, pricing, permissions, and debt terms. Being tokenized or having market value is not enough by itself; check [RIPE Params](https://params.ripe.finance) for current support.

### What's the delegation system?

You can grant granular permissions to other addresses for automated position management:

**Per-Address Delegations:**
* **canWithdraw**: Allow delegate to withdraw collateral on your behalf
* **canBorrow**: Enable borrowing and [deleveraging](../core-protocol/05-deleverage.md) operations
* **canClaimFromStabPool**: Allow claiming liquidated collateral from stability pools
* **canClaimLoot**: Allow claiming RIPE rewards on your behalf

**Global User Settings:**
* **canAnyoneDeposit**: Let any address add collateral for you
* **canAnyoneRepayDebt**: Allow anyone to repay your debt (useful for rescue scenarios)
* **canAnyoneBondForUser**: Let others purchase bonds on your behalf

**Limits and controls:**
* Routed withdrawal proceeds go to the original owner, but a delegate with withdrawal or borrowing authority can still change exposure, debt health, and liquidation risk and can cause material loss
* Each permission is independent — granting one doesn't grant others
* Per-address delegation has no `canDeposit` flag; third-party deposits use the account-wide `canAnyoneDeposit` setting or a trusted protocol path
* Changing or revoking these settings requires the Teller configuration route to be available and unpaused

**Use Cases:**
* Automated yield strategies via third-party protocols
* Team-managed treasury positions
* Bot-assisted position management
* Emergency rescue by trusted parties

### Can I use my borrowed GREEN to earn yield?

Absolutely! When borrowing, you can:

* Receive GREEN directly for any use
* Choose Savings GREEN to convert the borrowed amount into sGREEN
* When configured, send that newly created sGREEN directly to an eligible Stability vault

This can create positive carry when the realized sGREEN return exceeds the GREEN borrowing cost, but neither side of that spread is guaranteed.

## Governance & RIPE Token

### What is RIPE?

RIPE is the protocol's [governance](../governance-and-economics/02-governance.md) token. Lock it in the governance vault to:

* Accumulate governance points that a compatible governance interface can use as voting weight
* Earn rewards from the staker allocation when the asset and category are configured
* Increase the point rate when the configured lock terms provide a duration boost

### How does governance participation work?

Governance-vault positions record points lazily from normalized shares and elapsed protocol blocks. At a checkpoint, the then-current remaining-lock multiplier is applied across the whole uncheckpointed interval rather than declining continuously block by block. A compatible governance interface can use those points as voting weight; the points do not by themselves authorize administrative calls.

### What's the RIPE token distribution?

The Ripe Foundation's published 1 billion RIPE base-allocation policy is divided as follows; it is not an aggregate cap enforced by the token contract:

* **25%** Community incentives (only allocation unlocking at TGE)
* **22.2%** Ripe Foundation treasury
* **20.6%** Core contributors (4-year linear vesting, with position-transfer eligibility after year one)
* **17.2%** Early backers ($550k seed at $0.02)
* **15%** Distribution partner (Hightop)

Contributor schedules and lifecycle actions are recorded by onchain vesting contracts. See full details in [RIPE Tokenomics](../governance-and-economics/01-ripe-tokenomics.md).

### What are Ripe Bonds?

[Bonds](../governance-and-economics/03-bonds.md) let you exchange a configured payment asset for RIPE at an epoch price. A qualifying lock can add a configured bonus, and the accepted payment goes to the [Endaoment](../core-protocol/07-endaoment.md).

### What is the RIPE Reserve Engine?

The [RIPE Reserve Engine](../governance-and-economics/04-reserve-engine.md) is a separate acquisition-and-vesting mechanism. It accepts a configured payment token, sends the payment to EndaomentFunds, and reserves a base-plus-duration-bonus RIPE allocation from its own Vesting budget. Nothing is minted at acquisition: RIPE is minted only as the beneficiary claims vested amounts, either directly or through an optional deposit into the current core governance vault.

### What are Bond Boosters?

Bond Boosters can add a configured bonus for eligible users. A booster tracks how many whole payment-token units a user can apply, consumes those units during a qualifying purchase, and can require a minimum lock or expire at a configured block. The booster and lock bonuses are calculated separately from the base RIPE payout.

### What's the Endaoment?

The [Endaoment](../core-protocol/07-endaoment.md) is Ripe's governed onchain treasury system. It:

* Holds treasury assets received from bonds, Reserve Engine acquisitions, and other authorized flows
* Can deploy configured yield positions, including compatible [Underscore](https://underscore.finance/) integrations
* Supports GREEN through authorized, bounded market operations
* Holds and deploys protocol-owned assets under its permission model

Accepted bond and Reserve Engine payments become protocol treasury assets in EndaomentFunds.

## Safety & Security

### What are the main risks?

* **Smart contract risk**: Bugs could affect funds
* **Liquidation risk**: Collateral value dropping too fast
* **Oracle risk**: Incorrect or unavailable price feeds; ordered fallback improves availability but is not cross-source consensus
* **Interest rate risk**: Dynamic rates during market stress

### Is Ripe audited?

ChainSecurity reviewed an earlier Ripe Finance smart-contract architecture, and Anatomist reviewed a later Ripe Protocol revision. Each review covers only the specific code revisions and scope identified in its report; later changes and deployments may not be covered. See [Audits](audits.md) for the published reports and scope details.

### How does Ripe price assets accurately?

Ripe's [Price Desk](../core-protocol/06-price-oracles.md) checks configured priority sources first and then other registered sources, returning the first usable nonzero price. Each source call is isolated, so one revert, malformed response, stale feed, or unsupported asset does not stop later sources from being checked. If no source can establish a usable price, strict valuation fails closed; the protocol does not substitute a last-known cached price. Debt-bearing collateral with unavailable pricing enters the valuation quarantine described above.

### How does Ripe handle bad debt?

The protocol has multiple defenses:

1. Conservative collateral ratios
2. Separate redemption and deleverage mechanisms that may reduce unsafe debt before [liquidation](../core-protocol/04-liquidations.md)
3. [Stability pools](../earning-and-rewards/02-stability-pools.md) that can absorb compatible liquidation collateral when capacity is available
4. Permissionless keepers can submit eligible liquidations, although monitoring and transaction timing are not guaranteed
5. Qualifying [bond](../governance-and-economics/03-bonds.md) purchases can credit payment value against outstanding bad debt

If bad debt exists during a [bond](../governance-and-economics/03-bonds.md) purchase, the oracle-valued payment can clear bad debt up to that value. The corresponding portion of the RIPE payout is recorded outside the ordinary bond budget, so bad-debt recovery can expand supply beyond that ordinary budget and dilute holders.

### What happens in a market crash?

During extreme volatility:

* Redemption and deleverage remain separate paths for reducing eligible debt
* Stability pools provide liquidation liquidity only while compatible and sufficiently funded
* Liquidation targets restored health but can reach the full debt
* Dynamic rates can increase borrowing cost under sustained configured danger conditions
* Ordered oracle sources provide failover; when none is usable, valuation fails closed and debt-bearing accounts can enter quarantine

## Getting Help

### Where can I learn more?

* [**Documentation**](../): Detailed guides in our docs
* [**Discord**](https://discord.gg/hightop): Active community and team support
* [**Twitter**](https://x.com/ripe_dao): Protocol updates and announcements
* [**GitHub**](https://github.com/Ripe-Foundation/ripe-protocol): Open source code and development
* [**Homepage**](https://www.ripe.finance/): Ripe Homepage

### How do I report bugs or issues?

Security issues: Please report privately to ripefinance@proton.me General bugs: Open an issue on GitHub or report in Discord

***

_This FAQ covers common questions. For detailed technical information, see our comprehensive documentation._
