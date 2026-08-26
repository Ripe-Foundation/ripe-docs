---
description: The stablecoin that works harder.
---

# GREEN: The Stablecoin That Works Harder

Your USDC sits there doing nothing. Your DAI needs a new vault for every asset. Your LUSD only takes ETH.

GREEN is built different. Borrow it against your whole portfolio — stock tokens, ETH, stablecoins, yield positions — in one loan. Hold it as [sGREEN](../earning-and-rewards/01-sgreen.md) and it earns protocol revenue. Put it in a [Stability Pool](../earning-and-rewards/02-stability-pools.md) and it buys liquidated collateral below market. And six mechanisms pull it back toward $1.

This is what happens when you stop asking "how do we make another stablecoin?" and start asking "how should a stablecoin actually work?"

> **Live terms live onchain.** Assets, LTVs, rates, fees, and which optional features are switched on vary by deployment and change over time. Every number on this page is an example. [Params](https://params.ripe.finance) is the source of truth.

## Why GREEN Exists

### The Problem with Current Stablecoin Borrowing

**Isolated Positions (MakerDAO/Liquity)**: One vault per asset. Five assets, five loans, five liquidation risks.

**Pooled Risk (Aave/Compound)**: "Safe" assets only. Your collateral backs everyone's loans, and their bad debt is your problem.

**Isolated Money Markets (Morpho/Euler)**: One market per pair, each needing its own lenders. Same fragmentation, thinner liquidity.

### Ripe's Solution: Unified Multi-Collateral Borrowing

```
Traditional:  ETH → DAI Position 1
              WBTC → DAI Position 2
              USDC → Can't use

Ripe:         Stock tokens + ETH + Stablecoins + Yield positions
              ↓ (all combined)
              One GREEN Loan at Weighted Terms
```

Every supported asset you deposit backs the same GREEN loan, each on its own terms. Your risk stays yours: nobody else's blowup touches your position.

### Complementary, Not Competitive

Other protocols are great at yield, and Ripe turns their yield-bearing tokens into collateral. Where a token is supported, it keeps earning while it backs your loan: lending-market tokens keep accruing interest, vault shares keep compounding. Earn there, borrow here.

### Built for the Entire Ecosystem

```
                        GREEN ECOSYSTEM
┌─────────────────────────────────────────────────────────┐
│                                                         │
│  BORROWING          YIELD              STABILITY        │
│  ┌─────────┐       ┌──────────┐       ┌──────────┐      │
│  │ Mint    │       │ sGREEN   │       │ Stability│      │
│  │ GREEN   │  ───> │ Auto-    │  ───> │ Pools    │      │
│  │ Against │       │ Compound │       │ Earn     │      │
│  │ Assets  │       │ Yield    │       │ Discounts│      │
│  └─────────┘       └──────────┘       └──────────┘      │
│       │                  │                   │          │
│       └──────────────────┴───────────────────┘          │
│                          │                              │
│                    ┌─────▼──────┐                       │
│                    │  TREASURY  │                       │
│                    │  Endaoment │                       │
│                    │  Stabilizer│                       │
│                    └────────────┘                       │
│                                                         │
│  Every GREEN serves multiple purposes simultaneously    │
└─────────────────────────────────────────────────────────┘
```

## How GREEN Works

### Creation Through Borrowing

Borrowing is the main way GREEN comes into existence:

1. **Deposit Collateral**: Any [supported asset](03-collateral-assets.md) — [stock tokens](08-stock-tokens.md), WETH, stablecoins, yield positions
2. **Borrow GREEN**: New GREEN is minted against your collateral, up to each asset's LTV
3. **Pay Interest**: At your position's weighted rate, plus a [dynamic boost](02-borrowing.md#dynamic-interest-rates) if GREEN needs defending
4. **Origination Fee (Daowry)**: A one-time fee taken from the borrow. It goes to sGREEN holders; governance can split part of it toward a RIPE buyback allocation, and the default sends all of it to sGREEN

**Example:**

```
Your Unified Portfolio:
- A stock token worth $10,000 (60% LTV = $6,000 borrowing power)
- WETH worth $10,000 (70% LTV = $7,000 borrowing power)
- A stablecoin worth $10,000 (80% LTV = $8,000 borrowing power)
- A yield position worth $4,000 (70% LTV = $2,800 borrowing power)

Total Borrowing Power: $23,800 GREEN
One loan, one rate, one number to watch
```

Borrowing isn't the only mint. GREEN is also created when:

* interest that has accrued across the protocol is realized — it's minted alongside the next borrow and sent to sGREEN (and the buyback allocation, if one is set);
* someone mints through the [PSM](#6-peg-stability-module-psm) against its reserve stablecoin;
* the treasury's stabilizer adds GREEN liquidity to the reference pool;
* the treasury mints GREEN against a partner's assets for a liquidity partnership;
* a keeper is paid for a liquidation (see below).

### Destruction Through Repayment

Repay and the GREEN is burned, so supply expands and contracts with real borrowing demand:

1. **Send GREEN**: Return borrowed amount plus interest. Overpay and the extra comes back to you
2. **Burn Forever**: The GREEN applied to your debt is destroyed
3. **Free Your Collateral**: Less debt, more room to withdraw

GREEN also burns when it's redeemed for collateral, when a Stability Pool's GREEN or sGREEN settles a liquidation, when your own sGREEN pays down debt in a deleverage, when it's redeemed through the PSM, and when the treasury pulls GREEN out of the reference pool.

### Cross-Chain Token Movement

GREEN moves between chains by burn-and-mint: the amount you send is burned on the source chain and minted to you on the destination. Each chain's deployment sets which bridge pool may mint there.

## The Six Pillars of Stability

GREEN targets $1 through six mechanisms with different triggers. Several are redemption or conversion paths, so when GREEN drifts, arbitrageurs have more than one way to push it back.

### 1. Overcollateralization Foundation

* **Every loan overcollateralized**: Each asset's LTV caps what it can borrow, and safer assets get higher LTVs
* **Portfolio effect**: Stock tokens, ETH, stablecoins, and yield positions don't all fall together
* **Priced at every action**: Each borrow, withdrawal, or liquidation re-values your whole position with live [oracle prices](06-price-oracles.md)
* **Warning zones first**: Redemption and deleverage come before [liquidation](04-liquidations.md)

### 2. Dynamic Interest Rate Response

When GREEN is weak, borrowing gets more expensive, so borrowers buy GREEN to repay — and that buying supports the peg. Ripe reads the signal from a GREEN reference pool. Every time someone uses the protocol, it records a snapshot of the pool's GREEN share; the rate signal is the time-weighted average of those snapshots, using the lower reading of each consecutive pair so a single spike can't raise anyone's rate. Above the danger trigger, a boost is added to base rates, and it grows the longer the pool stays there — up to a hard cap. The danger clock only resets once the pool has been safe for a sustained window, not on the first good reading. If the history is stale or gappy (a quiet protocol records no snapshots), there's no signal and everyone pays their base rate.

Full mechanics: [Dynamic Interest Rates](02-borrowing.md#dynamic-interest-rates).

### 3. Direct Redemption Mechanism

Once a position falls into the [redemption zone](04-liquidations.md#the-redemption-buffer), any GREEN holder can redeem against it: pay GREEN at $1, receive collateral at oracle price. No discount, no penalty; the borrower's debt drops by the same amount. Amounts round down.

```
GREEN trading at $0.97? Here's what happens:

1. Arbitrageurs spot the 3% discount
2. Buy 10,000 GREEN for $9,700 on the market
3. Redeem it for $10,000 of collateral from positions in the redemption zone
4. Gross spread: $300, before fees and slippage
5. That buying pressure pushes GREEN back toward $1
```

Someone has to submit the transaction, and it only works while positions sit in the redemption zone — in calm markets there may be nothing to redeem. It switches on when stress pushes positions toward liquidation, which is when GREEN needs it most.

### 4. Stability Pool Redemption Mechanism

A second redemption path runs through [Stability Pools](../earning-and-rewards/02-stability-pools.md):

* Pools hold the collateral they bought in liquidations
* If governance has enabled it, GREEN holders can redeem 1 GREEN for $1 of that collateral at oracle price
* Only while a pool holds collateral to claim — no recent liquidations, no pool redemptions

```
GREEN at $0.96? Two redemption options:
1. Direct redemption (if positions are in the redemption zone)
2. Pool redemption (if pools hold liquidated collateral)
```

Both paths fill up during market stress: more liquidations mean more collateral in the pools and more GREEN demand, right when it's needed.

### 5. Endaoment Treasury Operations

The [Endaoment](07-endaoment.md) is the protocol-owned treasury. Payments for bonds and Reserve Engine allocations land there, and governance deploys them for yield, liquidity, and peg defense.

**The Stabilizer**

```
GREEN below peg (more GREEN than paired asset in the pool)?
→ Pull GREEN liquidity out of the pool
→ Burn it, up to the pool debt on record
→ Pool rebalances; price drifts back toward $1

GREEN above peg (less GREEN than paired asset)?
→ Add GREEN liquidity, treasury GREEN first
→ Mint the rest, recorded as pool debt, up to a ceiling
→ Deeper pool; price drifts back toward $1
```

Governance submits each stabilizer move; it isn't on a timer. Each move applies a fraction of the imbalance rather than forcing an exact 50/50 in one shot, and only executes if the treasury's net position doesn't get worse. Treasury GREEN is used before any is minted, and minting stops at the pool-debt ceiling.

**Liquidity Management**

* **Any supported venue**: Swaps, liquidity adds and removes, and yield deposits through registered integrations
* **Partner programs**: GREEN minted against a partner's assets and paired with them for deeper markets. This minting sits outside the stabilizer ceiling; any GREEN the pool doesn't take is burned
* **Self-funding**: Fees and yield from these operations stay in the treasury

### 6. Peg Stability Module (PSM)

Forget complex arbitrage strategies. The PSM swaps GREEN for its reserve stablecoin (one stablecoin, fixed at deployment) at $1, in one transaction, in whichever direction governance has turned on.

* **Mint GREEN**: Deposit the reserve stablecoin, receive GREEN 1:1 (minus a fee, if one is set). Ask for sGREEN and anything over 1 GREEN is wrapped for you
* **Redeem GREEN**: Burn GREEN or sGREEN, receive the reserve stablecoin 1:1 (minus a fee, if set)
* **No zone required**: Unlike redemptions, the PSM doesn't need positions in the redemption zone or collateral in a pool — just reserves

```
GREEN trading at $0.97?
1. Buy 10,000 GREEN on a DEX for $9,700
2. Redeem via the PSM for 10,000 of the reserve stablecoin
3. Gross spread: $300 before fees
4. Your buying pressure restores the peg

GREEN trading at $1.03?
1. Mint 10,000 GREEN via the PSM for 10,000 of the reserve stablecoin
2. Sell on a DEX for $10,300
3. Gross spread: $300 before fees
4. Your selling pressure restores the peg
```

**The Fine Print**

Quotes use the reserve stablecoin's oracle price or 1:1, whichever gives you less. Per-interval limits cap how much can be minted or redeemed, and governance can require an allowlist during rollout. Recognized Underscore vaults skip the limits, the allowlist, and the fee, and get the better quote on redemption. If governance has set a yield vault, the PSM parks its whole idle reserve balance there and pulls it back when a redemption needs it. Redemptions are capped by total reserves, idle plus in the vault.

Bottom line: when GREEN trades off-peg, the PSM is the shortest path to closing the gap.

## Additional GREEN Ecosystem Features

### Bad Debt Resolution

If extreme conditions leave bad debt on the books, [bond](../governance-and-economics/03-bonds.md) payments clear it, and the RIPE for that portion is accounted separately from the bond allocation. Governance administers RIPE's 1 billion cap as protocol-wide policy across all chains, and bad-debt-recovery RIPE counts toward it. Detail: [RIPE Tokenomics](../governance-and-economics/01-ripe-tokenomics.md).

### Protocol-Wide Integration

* GREEN is the only borrowable asset in Ripe Protocol
* Liquidation debt is settled in GREEN: auctions pay GREEN, and Stability Pools pay from their GREEN and sGREEN or hand non-GREEN assets to the treasury
* [Deleverage](05-deleverage.md) is a separate way debt comes down, spending your sGREEN and stablecoin positions

### Keeper Network Protection

* Anyone can trigger a liquidation once a position is past its threshold; bots watch around the clock
* The keeper's reward is minted as sGREEN (the keeper can ask for GREEN; dust is paid as GREEN) and added to the borrower's debt
* Liquidation fees are charged once per liquidation episode, not on every retry
* Fast liquidation keeps bad debt small

## GREEN Throughout Ripe Protocol

### Transform to sGREEN for Automatic Yield

[sGREEN](../earning-and-rewards/01-sgreen.md) is GREEN's yield-bearing twin:

* **Set and forget**: Deposit GREEN, receive sGREEN
* **Exchange-rate growth**: Each sGREEN redeems for more GREEN as revenue arrives; no rebasing
* **Protocol revenue**: Origination fees and borrower interest
* **Instant liquidity**: Redeem for GREEN anytime
* **Stackable**: Stability Pools accept sGREEN

### Participate in Stability Pools

Deposit sGREEN or GREEN LP tokens in [Stability Pools](../earning-and-rewards/02-stability-pools.md):

* **Three income sources**: sGREEN yield, liquidation proceeds, RIPE rewards
* **Below-market collateral**: Pools buy liquidated collateral at the asset's liquidation spread
* **Support the protocol**: Your deposit is what makes orderly liquidations possible
* **No lockups**: Withdraw whenever the pool holds what you're owed

### Enable Borrowing Options

When you borrow, choose how the GREEN arrives:

* **Direct GREEN**: For any use
* **Auto-sGREEN**: Start earning immediately
* **Stability Pool entry**: Straight into a pool, earning from day one

### Earn RIPE Block Rewards

Multiple ways to earn [RIPE](../earning-and-rewards/03-ripe-rewards.md) with GREEN:

* **Borrowing GREEN**: Larger, longer loans earn more
* **Stability Pool deposits**: sGREEN and GREEN LP deposits earn RIPE
* **Time-weighted**: Points accumulate on position size × time
* **Offset costs**: Rewards cut your effective borrowing rate or boost pool returns

## GREEN vs The Competition: A Clear Winner

**vs Centralized Stables (USDC/USDT)**: You can't borrow them — you buy them. GREEN is minted against what you already own, and sGREEN earns while USDC sits idle.

**vs Traditional Crypto Stables (DAI/LUSD)**: DAI wants a vault per collateral type. LUSD takes ETH only. GREEN takes your whole portfolio in one loan.

**vs Algorithmic Experiments (UST)**: No real backing, inevitable collapse. Every GREEN loan is overcollateralized, and there's no lender to run on.

**The key difference**: GREEN is built for borrowers who want their whole portfolio working — one loan, one rate, one number to watch.

## Why This Matters

Every other stablecoin makes you choose: safety or efficiency, yield or liquidity, simplicity or power.

GREEN breaks the tradeoffs. One position backed by everything you own. Yield that compounds while you sleep. Liquidation proceeds when others get rekt. A peg defended by six mechanisms, not faith.

Stop settling for stablecoins designed for 2020. This is how money works in DeFi now.

***

_For technical implementation details, see the_ [_GreenToken Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/tokens/greentoken)_._
