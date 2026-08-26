---
description: Tokenized stocks and eligible collateral backing one GREEN position.
---

# Tokenized Stocks & Collateral

Ripe is built around a simple idea: supported tokenized stocks should be able to unlock liquidity without a protocol sale at origination. When configured with borrowing power, they can work alongside other eligible collateral to back a single GREEN position with weighted, asset-specific terms. Stability-vault positions remain excluded from those borrowing terms.

## Why Ripe's Approach is Different

### The Problem with Traditional Lending

Most DeFi protocols force you into one of two suboptimal models:

**Isolated Markets**: Each asset creates a separate loan position

* Deposit one Stock Token → Manage one position
* Deposit another eligible asset → Manage another position
* One liquidation doesn't help the others
* Complex management across multiple positions

**Pooled Lending**: Shared risk limits asset acceptance

* Only blue-chip assets allowed
* Your deposits back everyone's loans
* Bad actors affect all depositors
* Innovation stifled by conservative parameters

### Ripe's Solution: Best of Both Worlds

Ripe combines portfolio efficiency with individual risk isolation:

The portfolio below is a hypothetical example, not a live asset or parameter list.

```
Stock-Led Portfolio                 Ripe
┌───────────────────────┐    ┌─────────────────────────┐
│ Stock Token A         │    │ One GREEN debt position │
│ Stock Token B         │ ─> │ Weighted debt terms     │
│ Other eligible asset  │    │ Account-level health    │
└───────────────────────┘    └─────────────────────────┘

Each asset contributes according to its own configured terms.
```

This architecture combines eligible collateral without flattening its risks or configuration. Each asset keeps its own terms while contributing to one account-level position.

## Stock Tokens as Collateral

These docs use **stock tokens** and **tokenized stocks** as broad category terms. Issuers use different product names and structures, including [Robinhood Stock Tokens](https://docs.robinhood.com/chain/stock-tokens/) and [Coinbase Tokenized Stocks](https://www.coinbase.com/tokenize). Ripe's collateral mechanics do not make those products legally identical or replace the applicable issuer's terms.

### Robinhood Stock Tokens

Robinhood Stock Tokens are standard ERC-20 tokenised debt securities issued by Robinhood Assets (Jersey) Limited (RHJ). They provide economic exposure to a referenced equity or ETF, but they are not the underlying shares or fund interests and do not confer legal or beneficial rights in them. See Robinhood's [Stock Token documentation](https://docs.robinhood.com/chain/stock-tokens/) for the product description.

Ripe is a separate lending protocol and is not the issuer. Its contracts can custody and transfer supported Stock Tokens through permitted protocol routes, but Ripe support and protocol permissions do not determine who is eligible under RHJ's terms to acquire, hold, transfer, or use them. Canonical token identity, Ripe support, and the absence of a protocol whitelist do not establish investor eligibility. Review RHJ's current [Base Prospectus, supplements, and applicable Final Terms](https://docs.robinhood.com/rhj) before interacting with a Stock Token. Other security-linked products can have different issuer terms and rights.

RHJ describes Stock Tokens as high-risk products that are not appropriate for every investor and warns that an investor can lose some or all of the investment. Ripe support does not remove issuer, product-term, market, liquidity, oracle, transfer, or collateral-liquidation risk.

When a compatible Stock Token is supported, it can use Ripe's configured nominal ERC-20 vault path. Depositing transfers the token into protocol custody and credits the user's vault balance. Borrowing GREEN does not sell that token at origination. You retain token-level economic exposure while you hold the Stock Token directly or retain a credited, custody-backed vault balance. A successful permitted ordinary withdrawal returns the token to the owner; an authorized deleverage, eligible redemption, or liquidation can reduce the credited balance and transfer some or all of the token away.

### Nominal Custody and Backing

A nominal vault records token units rather than a rebasing or share-based claim. Ripe therefore requires custody and accounting to agree:

* A deposit is credited only when the exact accepted amount reaches the configured destination custody and the vault records that same amount.
* A withdrawal or settlement must deliver the amount that its accounting credits. If token controls or transfer behavior prevent exact delivery, the operation fails closed rather than completing with a short receipt.
* A recorded balance whose backing cannot be established provides no usable collateral value or borrowing power.

These checks keep a nominal token balance from becoming borrowing power or paid settlement when the corresponding Stock Tokens are not actually available.

### Independent Terms, Permissions, and Routes

Support for one Stock Token does not configure every other Stock Token. Vault assignment, borrowing and risk terms, price sources, permissions, and settlement behavior are selected per asset.

Stock Tokens are not intrinsically permissioned or assigned to a particular liquidation route. An asset can have issuer- or token-level transfer restrictions, a protocol whitelist, recipient checks, or no additional restriction at a given layer. Redemption, Stability settlement, auction, and other authorized routes are also independent choices; documentation for one route does not imply that every Stock Token uses it.

### Pricing and Corporate Actions

Ripe values a tokenized stock through the first usable source in its configured price-source order. For a multiplier-based stock token, the configured source must return the token-level USD price with the applicable adjustment already incorporated. Ripe consumes that price once without independently reading or applying `uiMultiplier()`. The nominal token units credited by Ripe remain unchanged. Where a product exposes `uiMultiplier()`, that value expresses the share-equivalent units represented by those tokens for display purposes; Ripe does not use it in valuation.

Reference-market closures do not create a separate pricing mode inside Ripe. See [Stock-Market Hours and Price Gaps](06-price-oracles.md#stock-market-hours-and-price-gaps) for how freshness, fallback, and reopening gaps interact.

## Asset Categories

Ripe's extensible architecture can support a vast and growing universe of tokenized value. The categories and assets below are illustrative; see [RIPE Params](https://params.ripe.finance) for current asset support and configuration.

**1. Tokenized Stocks and Other Tokenized Real-World Assets** - Market exposure as collateral

* **Stock Tokens**: ERC-20 products providing public equity exposure, subject to the custody, pricing, and asset-specific controls described above
* **Other tokenized assets**: Bonds, commodities, real estate interests, and other issuer-defined products when supported
* Eligibility, transfer restrictions, and protocol routes remain specific to each token and configuration

**2. Stablecoins** - The foundation of stability

* **USDC, USDT**: Major centralized stablecoins with deep liquidity
* **USDS**: Decentralized stablecoin from Sky Protocol
* **Yield-bearing stables**: Interest-earning stable assets
* Illustrative LTVs can be higher than more volatile collateral, subject to configuration

**3. Blue-Chip Crypto** - Established digital assets

* **WETH**: Wrapped Ethereum, the DeFi standard
* **WBTC/cbBTC**: Bitcoin representations on Ethereum
* **Major DeFi tokens**: AAVE, UNI, CRV, and other protocol tokens
* **Layer 1 tokens**: SOL, AVAX, XRP, HYPE (when bridged)
* Provide strong borrowing power with proven track records

**4. Yield-Bearing Assets** - Earn while you borrow

* **Liquid staking**: stETH, rETH, cbETH continue earning staking rewards
* **LP tokens**: Uniswap, Curve, Balancer positions keep earning fees
* **Vault tokens**: Yield-generating vault positions
* Share-based accounting preserves all accumulated yields

**[Underscore](https://underscore.finance/) Vaults** - The premier yield-bearing collateral

Underscore's AI-powered vaults (ERC4626) are the recommended way to earn yield while using assets as collateral:

* **Always optimizing**: AI agents continuously rebalance strategies even while your vault tokens are locked as collateral on Ripe
* **Multiple asset types**: USD, ETH, BTC, and other strategies available
* **Continuous yield**: Vaults keep earning while serving as your collateral

Supported Underscore vault shares can remain yield-bearing while used as Ripe collateral, subject to the vault's own product terms and Ripe's collateral configuration. Depositing a vault share does not itself qualify an ordinary borrower for Earn-vault rate treatment. That treatment applies only when the borrower address itself is registered as an Underscore Earn vault; see [Underscore Earn Vault Integration](02-borrowing.md#underscore-earn-vault-integration).

**5. NFTs & Unique Assets** - Beyond fungible tokens

* **Blue-chip collections**: Punks, Apes, Penguins as collateral
* **Art NFTs**: Generative and 1/1 pieces
* **Gaming items**: Weapons, land, characters
* **Music/Media**: Royalty-bearing NFTs
* Lower LTVs (30-50%) but still productive capital

**6. Emerging Digital Assets** - The new frontier

* **Prediction shares**: Tokenized prediction market positions
* **Meme coins**: PEPE, SHIB, and community tokens
* **Social tokens**: Creator coins and DAO tokens
* **AI tokens**: Emerging AI protocol tokens
* Conservative parameters reflect higher volatility

## How Deposits Work

### Vault Types Explained

Each supported asset is assigned to a configured vault whose accounting matches the asset's behavior:

**Simple ERC-20 Vaults** - Nominal fungible tokens, including configured Stock Tokens

* Direct 1:1 balance tracking
* Simple deposit/withdraw mechanics
* Most common vault type

**Rebase Erc20 Vaults** - Yield-bearing assets (stETH, aTokens)

* Share-based accounting preserves yields
* Compound earnings while deposited
* Underlying economics can continue when supported, subject to the asset's vault, custody, and market risks

Deposits and withdrawals must deliver the amount the vault accounts for. An underbacked position provides no usable collateral value or borrowing power.

**Special Purpose Vaults**

* [**Ripe Gov Vault**](../governance-and-economics/02-governance.md): Lock RIPE tokens for governance power
* [**Stability Pools**](../earning-and-rewards/02-stability-pools.md): Participate in configured liquidation settlement with sGREEN or eligible LP tokens
* **Future Vaults**: Non-fungible and other assets requiring specialized custody or accounting

Positions in any vault classified as a Stability vault are excluded from borrowing collateral terms.

### The Power of Extensibility

Ripe's vault system is designed to be infinitely extensible. As new asset types emerge or special requirements arise, the protocol can deploy new vault implementations without disrupting existing operations:

* **Custom Logic**: Each vault type can implement specific behaviors for its assets
* **Future-Proof**: Support for assets that don't exist yet
* **Seamless Integration**: New vaults plug into the existing ecosystem
* **Innovation Ready**: From NFT fractionalization to real-world asset settlements

This extensibility ensures Ripe can adapt to any tokenized value the future brings — whether it's gaming assets requiring special metadata, regulated securities needing compliance hooks, or entirely new token standards we haven't imagined yet.

The asset's protocol configuration determines which vault handles a deposit.

### Deposit Limits and Controls

Each asset has configured absolute token-balance limits for ordinary deposits. These limits can indirectly bound concentration, but they do not dynamically control an asset's percentage of GREEN backing.

**Why Limits Matter**

Deposit limits bound how much of an asset can be credited to one user and in aggregate. They are expressed in token units rather than as percentages of portfolio value or GREEN backing.

**Per-User Limits**

* An absolute maximum credited token balance for one user in the target vault
* The available amount is the configured limit minus that user's existing balance

**Global Limits**

* An absolute maximum aggregate token balance for the asset in the target vault
* The available amount is the configured limit minus the vault's existing total balance

**Minimum Balances**

* A configured minimum balance can apply when opening or leaving a position
* Deposits and withdrawals must leave the position at zero or at least that minimum, subject to the applicable path

Governance can update the configured absolute limits and minimum balance without changing this mechanism.

Current limits and eligibility are published through [RIPE Params](https://params.ripe.finance).

## Making Withdrawals

### Withdrawal Mechanics

Withdrawals respect your overall position health:

1. **Free Collateral**: Withdraw assets above borrowing needs
2. **Health Check**: Ensure position remains safe
3. **Transaction Processing**: General deposits have no contractual maturity queue, but withdrawals still require a successful transaction and all applicable controls
4. **Partial or Full**: Take what you need, leave the rest

### Understanding Available Withdrawals

Your withdrawal capacity depends on:

* **Unused collateral** not backing loans
* **Asset-specific LTVs** determining borrowing power
* **Current debt levels** and interest accrued
* **Overall health factor** maintaining safety

Example:

```
Deposited: $10,000 ETH
Borrowed: $5,000 GREEN (at 80% LTV)
Required: $6,250 collateral
Available to withdraw: $3,750 worth of ETH
```

## Earning While Deposited

### Conditional Reward Accumulation

Eligible deposits can accrue RIPE rewards through the protocol's points system only while points, the applicable asset and category allocations, emissions, and the rewards allowance are active:

```
Points = Deposit Value × Blocks Held
Share = Your Points / Total Points
Rewards = Your Share × Emissions
```

Time and size both affect points, but points do not guarantee a reward payout.

### Reward Categories

**General Depositors** - Eligible configured deposits can receive general-depositor rewards

* USD-weighted points when the asset uses the general-depositor category
* Rewards remain conditional on the category allocation, emissions, allowance, usable valuation, and custody-backed balance

**Vote Depositors** - Community-selected bonus rewards

* Configured voter points for selected assets
* Rewards remain conditional on the applicable voter allocation and global reward availability

**Special Rewards** - Enhanced earnings in specific vaults

* [Stability pool](../earning-and-rewards/02-stability-pools.md) positions can combine deposited-asset economics, liquidation settlement, and RIPE rewards when each path is configured
* [Governance Vault](../governance-and-economics/02-governance.md) positions can receive configured point multipliers and rewards

For a detailed exploration of the RIPE rewards system, including emission schedules, point calculations, and maximization strategies, see [RIPE Block Rewards](../earning-and-rewards/03-ripe-rewards.md).

## Advanced Features

### Delegation System

Third-party deposit, repayment, and bond permissions are disabled by default. Ripe separates those account-wide permissions from address-specific delegation:

* **Account-wide rights**: `canAnyoneDeposit`, `canAnyoneRepayDebt`, and `canAnyoneBondForUser`
* **Delegate rights**: `canWithdraw`, `canBorrow`, `canClaimFromStabPool`, and `canClaimLoot`
* **Permission Management**: Update or revoke permissions through Teller while that route is available and unpaused
* **Smart Wallet Compatible**: Works with Underscore smart wallets, including those used by Hightop for agent-managed accounts

Use cases:

* Team treasury management
* Automated strategy execution
* Family account structures
* Protocol integrations

### Asset-Specific Permissions

Some assets can require special access or recipient eligibility, while others have no additional restriction at the protocol layer:

* **Token-level controls**: Issuer-enforced allowlists, blocklists, pauses, or transfer restrictions
* **Protocol controls**: An optional asset whitelist or recipient check
* **Route controls**: Independent eligibility for deposit, withdrawal, redemption, Stability settlement, auction, or another authorized operation

The applicable controls come from the specific token and protocol configuration; the tokenized-stock category alone does not determine them.

## Why Deposit in Ripe?

### Immediate Benefits

* **Earn RIPE rewards** on eligible configured deposits
* **No protocol maturity lock** on general deposits, subject to health, pause, custody, token, and pricing controls
* **Productive collateral when supported** - eligible yield-bearing assets can retain the economics implemented by their configured vault path
* **Portfolio borrowing power** - eligible assets can support one position, while every deposited asset remains exposed to account-level settlement risk

### Long-term Value

* **Early participant advantages** in growing protocol
* **Governance participation** shapes the future
* **Network effects** as more assets join
* **Innovation pipeline** supporting new asset types

### Capital Efficiency

* **One position** instead of many to manage
* **Cross-collateralization** maximizes borrowing power
* **Diversification can reduce concentration risk**, while every asset remains exposed to account-level settlement
* **Optimized parameters** for each asset type

## The Power of True Portfolio Lending

Forget everything you know about DeFi borrowing. No more juggling ten different positions. No more leaving half your assets idle because they're "not supported." No more choosing between earning yield or accessing liquidity.

With Ripe, supported tokenized stocks can unlock GREEN liquidity without a protocol sale at origination, while other eligible assets can contribute to the same position. One loan, weighted terms, and asset-specific controls working together.

This isn't just another lending protocol — it's how DeFi lending should have worked from day one.

***

_For technical implementation details, see the_ [_Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core-lending/teller)
