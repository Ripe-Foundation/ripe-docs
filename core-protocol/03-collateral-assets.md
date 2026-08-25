---
description: One loan. Supported collateral. Portfolio efficiency.
---

# Collateral Assets

Ripe treats supported collateral like a portfolio. Deposits with borrowing terms work together to back a single GREEN loan, with each asset contributing its own weight. The vault architecture can extend this model to additional asset and accounting types when governance supplies compatible custody and pricing.

## Why Ripe's Approach is Different

### The Problem with Traditional Lending

Most DeFi protocols force you into one of two suboptimal models:

**Isolated Markets**: Each asset creates a separate loan position

* Deposit ETH → Manage one position
* Deposit cbBTC → Manage another position
* One liquidation doesn't help the others
* Complex management across multiple positions

**Pooled Lending**: Shared risk limits asset acceptance

* Only blue-chip assets allowed
* Your deposits back everyone's loans
* Bad actors affect all depositors
* Innovation stifled by conservative parameters

### Ripe's Solution: Portfolio Accounting

Ripe combines portfolio efficiency with account-level collateral and debt accounting. The values below are illustrative rather than current asset terms:

```
Your Multi-Asset Portfolio = One GREEN Loan
┌───────────────────────────────────────────┐
│  ETH    USDC    WBTC    PEPE    stETH     │
│  $10k   $5k     $15k    $100    $50k      │
│  80%    90%     80%     50%     85%       │ <- Individual LTVs
│  ↓      ↓       ↓       ↓       ↓         │
│  ═══════════════════════════════════════  │
│           COMBINED COLLATERAL             │
│           Total Value: $80,100            │
│           Borrowing Power: $67,585        │
│                     ↓                     │
│         SINGLE GREEN LOAN POSITION        │
│         Up to $67,585 GREEN               │
│                                           │
│  • One loan, one interest rate            │
│  • One health factor to monitor           │
│  • Configured collateral contributes      │
│  • Account-level collateral and debt      │
└───────────────────────────────────────────┘
```

This architecture enables broad asset support while maintaining account-level debt and collateral accounting. Global debt limits, rates, oracle and liquidity conditions, and bad debt remain shared protocol risks. Listing still requires governance configuration, a compatible vault, and a usable price path.

## Asset Types the Architecture Can Support

The following categories illustrate what compatible vault and pricing modules can represent. They are not a live supported-asset list; see [RIPE Params](https://params.ripe.finance) for current configuration.

**1. Stablecoins** - The foundation of stability

* **USDC, USDT**: Major centralized stablecoins with deep liquidity
* **USDS**: Decentralized stablecoin from Sky Protocol
* **Yield-bearing stables**: Interest-earning stable assets
* Can receive comparatively conservative or permissive terms according to governance's risk assessment

**2. Blue-Chip Crypto** - Established digital assets

* **WETH**: Wrapped Ethereum, the DeFi standard
* **WBTC/cbBTC**: Bitcoin representations on Ethereum
* **Major DeFi tokens**: AAVE, UNI, CRV, and other protocol tokens
* **Layer 1 tokens**: SOL, AVAX, XRP, HYPE (when bridged)
* Provide strong borrowing power with proven track records

**3. Yield-Bearing Assets** - Earn while you borrow

* **Liquid staking**: stETH, rETH, cbETH continue earning staking rewards
* **LP tokens**: Uniswap, Curve, Balancer positions keep earning fees
* **Vault tokens**: Yield-generating vault positions
* Share-based accounting preserves all accumulated yields

**[Underscore](https://underscore.finance/) Vaults** - Optional strategy-managed collateral

When an Underscore registry and compatible vault shares are configured, those ERC-4626 positions can serve as yield-bearing collateral:

* **Strategy-managed**: The external vault can continue operating its strategy while its shares are deposited as collateral
* **Multiple asset types**: USD, ETH, BTC, and other strategies available
* **Continuous yield**: Vaults keep earning while serving as your collateral
* **Preferential borrowing**: Recognized Underscore vaults can receive [discounted rates](02-borrowing.md#underscore-earn-vault-integration) when borrowing GREEN

The Ripe position holds the vault shares while the external vault continues its own accounting. Availability and strategy behavior depend on the configured integration.

**4. Tokenized Real-World Assets** - Bridging traditional finance

* **Securities**: Tokenized stocks, bonds, ETFs
* **Commodities**: Gold, silver, oil representations
* **Real estate**: Property-backed tokens
* **Carbon credits**: Environmental assets
* Special handling for regulatory compliance

**5. NFTs & Unique Assets** - Beyond fungible tokens

* **Blue-chip collections**: Punks, Apes, Penguins as collateral
* **Art NFTs**: Generative and 1/1 pieces
* **Gaming items**: Weapons, land, characters
* **Music/Media**: Royalty-bearing NFTs
* Can receive more conservative terms while remaining productive capital

**6. Emerging Digital Assets** - The new frontier

* **Prediction shares**: Tokenized prediction market positions
* **Meme coins**: PEPE, SHIB, and community tokens
* **Social tokens**: Creator coins and DAO tokens
* **AI tokens**: Emerging AI protocol tokens
* Conservative parameters reflect higher volatility

## How Deposits Work

### Vault Types Explained

Ripe routes deposits according to the configured vault and asset relationship:

**Simple Erc20 Vaults** - Standard tokens (ETH, USDC, most assets)

* Direct 1:1 balance tracking
* Simple deposit/withdraw mechanics
* Most common vault type

**Rebase Erc20 Vaults** - Yield-bearing assets (stETH, aTokens)

* Share-based accounting preserves yields
* Compound earnings while deposited
* Yield treatment depends on the configured asset and compatible share-vault behavior

Listing alone is insufficient: the asset also needs a vault compatible with its transfer and accounting behavior. Teller credits the exact custody increase it measures; simple vaults require full backing and exact delivery, while share vaults issue and burn shares against measured asset movement. Non-standard tokens therefore require compatible vault support.

**Special Purpose Vaults**

* [**Ripe Gov Vault**](../governance-and-economics/02-governance.md): Lock RIPE tokens for governance power
* [**Stability Pools**](../earning-and-rewards/02-stability-pools.md): Earn from liquidations with sGREEN/LP tokens
* **Future Vaults**: NFTs, RWAs, and emerging asset types

### The Power of Extensibility

Ripe's vault system is designed to be infinitely extensible. As new asset types emerge or special requirements arise, the protocol can deploy new vault implementations without disrupting existing operations:

* **Custom Logic**: Each vault type can implement specific behaviors for its assets
* **Future-Proof**: Support for assets that don't exist yet
* **Seamless Integration**: New vaults plug into the existing ecosystem
* **Innovation Ready**: From NFT fractionalization to real-world asset settlements

This extensibility ensures Ripe can adapt to any tokenized value the future brings — whether it's gaming assets requiring special metadata, regulated securities needing compliance hooks, or entirely new token standards we haven't imagined yet.

The protocol uses the vault selected by its governed registry and routing configuration.

### Deposit Limits and Controls

Each asset has configurable parameters that protect the stability of GREEN, our stablecoin:

**Why Limits Matter**

Deposit limits bound the token amount one user or one vault can accept under an asset's configuration. Governance can use those absolute caps as one risk-management tool, but the contracts do not enforce a target portfolio percentage or guarantee diversification.

**Per-User Limits**

* Absolute token-amount cap for one user's asset position in the selected vault
* The remaining allowance is calculated from that user's existing balance
* A protocol department can use a separately authorized path that skips these ordinary depositor limits

**Aggregate Vault Limits**

* Absolute token-amount cap for the asset's total balance in the selected vault
* The remaining allowance is calculated from that vault's existing asset balance
* This is called the global deposit limit in configuration, but it is not a percentage of all protocol collateral

**Minimum Balances**

* Small position requirements
* Prevents dust accumulation
* Ensures meaningful participation
* Reduces computational overhead

Governance can change these asset-and-vault controls over time. See [RIPE Params](https://params.ripe.finance) for the current configuration.

## Making Withdrawals

### Withdrawal Mechanics

Withdrawals respect your overall position health:

1. **Free Collateral**: Withdraw assets above borrowing needs
2. **Health Check**: Ensure position remains safe
3. **Onchain Processing**: An eligible withdrawal completes in its transaction after health, custody, pause, quarantine, and other protocol checks pass
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

### Reward Accumulation

When a deposit asset and vault are configured for RIPE incentives, its reward entitlement accumulates through the protocol's points system:

```
Rewards = Category Rewards × Asset Category-Point Share × User Balance-Point Share
```

Global category points, asset-category points, and user balance points accrue separately. User points use the position's normalized balance or shares; general-depositor asset points use eligible USD value; and staker or voter asset points use configured allocation rates. See [RIPE Rewards](../earning-and-rewards/03-ripe-rewards.md#understanding-your-share) for the full flow.

### Reward Categories

**General Depositors** - Configured deposits can earn base rewards

* USD-weighted fair distribution
* No special lock requirement
* Allocation can differ by asset and vault

**Vote Depositors** - Community-selected bonus rewards

* Higher allocations for chosen assets
* Governance participation benefits
* Strategic deposit opportunities

**Special Rewards** - Enhanced earnings in specific vaults

* [Stability pool](../earning-and-rewards/02-stability-pools.md) deposits earning dual yields
* [Governance Vault](../governance-and-economics/02-governance.md) staking with multipliers
* Future special purpose incentives

For a detailed exploration of the RIPE rewards system, including emission schedules, point calculations, and maximization strategies, see [RIPE Block Rewards](../earning-and-rewards/03-ripe-rewards.md).

## Advanced Features

### Delegation System

Grant specific permissions to other addresses:

* **Deposit Rights**: Allow others to add collateral
* **Withdrawal Rights**: Delegate withdrawal capabilities
* **Full Flexibility**: Revoke permissions anytime
* **Smart Wallet Compatible**: Works with Underscore smart wallets, including those used by Hightop for agent-managed accounts

Use cases:

* Team treasury management
* Automated strategy execution
* Family account structures
* Protocol integrations

### Whitelisted Assets

Some assets require special access:

* **Tokenized Securities**: KYC/AML verification
* **Institutional Assets**: Accredited investor status
* **Beta Features**: Early access programs
* **Regulated Tokens**: Compliance requirements

The protocol handles permissions transparently — you'll know if an asset requires approval.

## Why Deposit in Ripe?

### Immediate Benefits

* **Earn RIPE rewards** when the deposited asset is configured for incentives
* **No time lock** on general deposits; withdrawal remains subject to health, custody, pause, quarantine, and other ordinary controls
* **Compatible yield-bearing collateral** can retain its external share or exchange-rate economics while deposited
* **Portfolio approach** reduces liquidation risk

### Long-term Value

* **Early participant advantages** in growing protocol
* **Governance participation** shapes the future
* **Network effects** as more assets join
* **Innovation pipeline** supporting new asset types

### Capital Efficiency

* **One position** instead of many to manage
* **Cross-collateralization** maximizes borrowing power
* **Lower liquidation risk** through diversification
* **Optimized parameters** for each asset type

## The Power of True Portfolio Lending

Forget everything you know about DeFi borrowing. No more juggling ten different positions. No more leaving half your assets idle because they're "not supported." No more choosing between earning yield or accessing liquidity.

With Ripe, configured yield-bearing shares can keep their external economics while contributing to one weighted loan. Additional collateral types can join the same model after compatible vault, pricing, and governance setup.

This isn't just another lending protocol — it's how DeFi lending should have worked from day one.

***

_For technical implementation details, see the_ [_Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core-lending/teller)
