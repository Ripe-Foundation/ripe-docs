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
│  WETH   USDC    WBTC    PEPE    stETH     │
│  $10k   $5k     $15k    $100    $50k      │
│  80%    90%     80%     50%     85%       │ <- Individual LTVs
│  ↓      ↓       ↓       ↓       ↓         │
│  ═══════════════════════════════════════  │
│           COMBINED COLLATERAL             │
│           Total Value: $80,100            │
│           Borrowing Power: $67,050        │
│                     ↓                     │
│         SINGLE GREEN LOAN POSITION        │
│         Up to $67,050 GREEN               │
│                                           │
│  • One loan, one interest rate            │
│  • One set of account thresholds          │
│  • Configured collateral contributes      │
│  • Account-level collateral and debt      │
└───────────────────────────────────────────┘
```

This architecture enables broad asset support while maintaining account-level debt and collateral accounting. Global debt limits, rates, oracle and liquidity conditions, and bad debt remain shared protocol risks. Listing still requires governance configuration, a compatible vault, and a usable price path.

## Asset Types the Architecture Can Support

The following categories illustrate what compatible custody, pricing, and routing modules can represent. They are not a live supported-asset list. The ordinary Teller intake path is ERC-20-based, so native assets and other token standards require a compatible wrapper or broader Teller/routing support; see [RIPE Params](https://params.ripe.finance) for current configuration.

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
* Debt terms remain specific to each configured asset and deployment

**3. Yield-Bearing Assets** - Earn while you borrow

* **Liquid staking**: Compatible receipt tokens can retain their external staking economics
* **LP tokens**: Compatible AMM position tokens can retain their external fee economics
* **Vault tokens**: Yield-generating vault positions
* Ripe share accounting can reflect external balance or exchange-rate changes when the selected vault supports that token's behavior

**[Underscore](https://underscore.finance/) Vaults** - Optional strategy-managed collateral

When an Underscore registry and compatible vault shares are configured, those ERC-4626 positions can serve as yield-bearing collateral:

* **Strategy-managed**: The external vault can continue operating its strategy while its shares are deposited as collateral
* **Multiple asset types**: USD, ETH, BTC, and other strategies available
* **External strategy economics**: The vault can continue its own accounting while its shares are deposited
* **Borrower-specific terms**: A borrower address recognized as an Underscore Earn vault can receive [discounted rates](02-borrowing.md#underscore-earn-vault-integration); depositing an Underscore share does not discount an ordinary user's loan

The Ripe position holds the vault shares while the external vault continues its own accounting. Availability and strategy behavior depend on the configured integration.

**4. Tokenized Real-World Assets** - Bridging traditional finance

* **Securities**: ERC-20-compatible representations of tokenized stocks, bonds, or ETFs
* **Commodities**: Compatible tokenized representations of gold, silver, or other commodities
* **Real estate**: Compatible property-backed token representations
* **Carbon credits**: Compatible environmental-asset representations
* Listing still requires custody, pricing, permissions, and debt terms; protocol hooks do not establish legal or regulatory compliance

**5. Wrapped or Fractionalized Unique Assets** - Future architecture

* An ERC-20-compatible wrapper or fractionalized representation could use the ordinary fungible intake path if the full custody, pricing, and risk stack is configured
* Raw ERC-721 and ERC-1155 assets are not supported merely by registering a new vault; they require Teller and routing changes in addition to asset-specific custody and liquidation logic

**6. Emerging Digital Assets** - The new frontier

* **Prediction shares**: Compatible fungible prediction-market positions
* **Community and social tokens**: ERC-20-compatible assets when configured
* **Other standards**: Require compatible wrappers or protocol changes beyond a vault implementation
* Debt terms can reflect governance's risk assessment without implying current support

## How Deposits Work

### Vault Types Explained

Ripe routes deposits according to the configured vault and asset relationship:

**Simple Erc20 Vaults** - Standard fungible tokens (WETH, USDC, and other compatible ERC-20 assets)

* Direct 1:1 balance tracking
* Simple deposit/withdraw mechanics
* Most common vault type

**Rebase Erc20 Vaults** - Yield-bearing assets (stETH, aTokens)

* Share-based accounting preserves yields
* Compound earnings while deposited
* Yield treatment depends on the configured asset and compatible share-vault behavior

Listing alone is insufficient: the asset also needs a vault compatible with its transfer and accounting behavior. Ordinary Teller deposits measure custody through ERC-20 `balanceOf` and move funds through ERC-20 `transferFrom`; simple vaults require full backing and exact delivery, while share vaults issue and burn shares against measured asset movement. Native ETH, raw NFTs, and other standards therefore need an ERC-20-compatible wrapper or changes to Teller and its routing as well as a compatible vault.

**Special Purpose Vaults**

* [**Ripe Gov Vault**](../governance-and-economics/02-governance.md): Lock RIPE tokens for governance-point and reward accounting
* [**Stability Pools**](../earning-and-rewards/02-stability-pools.md): Earn from liquidations with sGREEN/LP tokens
* **Future Vaults and Routes**: Additional fungible accounting models, and broader protocol changes for non-ERC-20 standards

### The Power of Extensibility

Ripe can add governed vault implementations for asset behaviors that fit the existing Teller and registry interfaces. Broader token standards may also require intake, routing, pricing, and liquidation changes:

* **Custom Logic**: Each vault type can implement specific behaviors for its assets
* **Registry Integration**: Governance can register compatible vaults and asset routes
* **Wrapper Support**: ERC-20-compatible representations can reuse the fungible intake path when the rest of the stack is configured
* **Protocol Extensions**: Native or non-fungible standards require more than a vault implementation

This architecture provides extension points; it does not make every token or token standard depositable by default.

The protocol uses the vault selected by its governed registry and routing configuration.

### Deposit Limits and Controls

Each asset has configurable parameters that protect the stability of GREEN, our stablecoin:

**Why Limits Matter**

Deposit limits bound the token amount one user or one vault can accept under an asset's configuration. Governance can use those absolute caps as one risk-management tool, but the contracts do not enforce a target portfolio percentage or guarantee diversification.

**Per-User Limits**

* Action-time token-amount cap for one user's asset position in the selected vault
* The remaining allowance is calculated from that user's existing balance when an ordinary deposit is submitted
* A protocol department can use a separately authorized path that skips these ordinary depositor limits

**Aggregate Vault Limits**

* Action-time token-amount cap for the asset's total balance in the selected vault
* The remaining allowance is calculated from that vault's existing asset balance when an ordinary deposit is submitted
* This is called the global deposit limit in configuration, but it is not a percentage of all protocol collateral

**Topology Limits**

* An ordinary deposit can be blocked when the user already participates in the configured maximum number of vaults
* Adding a new asset position can be blocked when that vault already contains the configured maximum number of assets for the user

**Minimum Balance Check**

* Applied when an ordinary deposit creates or increases a position and after a partial withdrawal
* A full withdrawal can deplete the position rather than leaving the minimum
* This is an action-time check, not a promise that external balance changes can never leave dust

Ordinary deposits also run whole-account debt housekeeping. With debt outstanding, a deposit can revert if another debt-bearing asset has configured pricing but no usable result. Governance can change these asset-and-vault controls over time; see [RIPE Params](https://params.ripe.finance) for the current configuration.

## Making Withdrawals

### Withdrawal Mechanics

Withdrawals respect your overall position health:

1. **Free Collateral**: Withdraw assets above borrowing needs
2. **Health Check**: Keep debt within max-borrow capacity with the protocol's additional withdrawal buffer
3. **Onchain Processing**: An eligible withdrawal completes in its transaction after health, custody, pause, quarantine, and other protocol checks pass
4. **Partial or Full**: Take what you need, leave the rest

### Understanding Available Withdrawals

Your withdrawal capacity depends on:

* **Unused collateral** not backing loans
* **Asset-specific LTVs** determining borrowing power
* **Current debt levels** and interest accrued
* **Current max-borrow capacity** after applying the protocol's withdrawal buffer

Ordinary withdrawals are blocked while the account is `inLiquidation` or already above its current max-borrow capacity. Strict whole-account housekeeping can also revert a withdrawal—including a zero-LTV withdrawal—while another debt-bearing asset has configured pricing but no usable result.

Example:

```
Deposited: $10,000 of hypothetical ERC-20 collateral
Borrowed: $5,000 GREEN (at 80% LTV)
Buffered debt for withdrawal sizing: $5,050 (1% above debt)
Required: $6,312.50 collateral
Available to withdraw: $3,687.50 worth of the example collateral
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

* USD-weighted allocation under the configured reward terms
* An asset contributes general-depositor USD points only when its staker-points allocation is zero
* No special lock requirement
* Allocation can differ by asset and vault

**Vote Depositors** - Community-selected bonus rewards

* Higher allocations for chosen assets
* Governance participation benefits
* Strategic deposit opportunities

**Special Rewards** - Enhanced earnings in specific vaults

* [Stability pool](../earning-and-rewards/02-stability-pools.md) deposits with multiple configured return sources and liquidation exposure
* [Governance Vault](../governance-and-economics/02-governance.md) staking with multipliers
* Future special purpose incentives

For a detailed exploration of the RIPE rewards system, including emission schedules, point calculations, and maximization strategies, see [RIPE Block Rewards](../earning-and-rewards/03-ripe-rewards.md).

## Advanced Features

### Delegation System

Grant any combination of the four address-specific action permissions:

* **`canWithdraw`**: Submit eligible withdrawals for the account
* **`canBorrow`**: Submit eligible borrows and use routes that rely on borrowing authority
* **`canClaimFromStabPool`**: Claim eligible Stability-vault assets
* **`canClaimLoot`**: Claim eligible RIPE rewards

Deposit-for-user permission is separate: the account-wide `canAnyoneDeposit` setting permits any caller to deposit for that account. Valid Ripe departments and a recognized Underscore wallet owner can also use their authorized paths. Delegation and account-config changes require Teller to be unpaused; they are not guaranteed to be changeable during a pause.

Use cases:

* Team treasury management
* Automated strategy execution
* Family account structures
* Protocol integrations

### Whitelisted Assets

An asset can use a configured allowlist hook:

* The relevant account or recipient must pass the configured eligibility check for the action
* Deposit, withdrawal, redemption, Stability, and auction paths can apply their own configured permission surfaces
* The onchain hook enforces the configured result; it does not itself establish KYC, investor status, or legal compliance

Interfaces should surface these restrictions, but execution is determined by the onchain checks.

## Why Deposit in Ripe?

### Immediate Benefits

* **Earn RIPE rewards** when the deposited asset is configured for incentives
* **No time lock** on general deposits; withdrawal remains subject to health, custody, pause, quarantine, and other ordinary controls
* **Compatible yield-bearing collateral** can retain its external share or exchange-rate economics while deposited
* **Portfolio accounting** lets supported assets contribute together; the resulting risk depends on composition and does not necessarily fall

### Additional Protocol Uses

* **Governance-vault positions** can accumulate configured points under their own lock and checkpoint rules
* **Configured incentives** can add RIPE reward eligibility to particular asset-and-vault positions
* **Future asset additions** can extend the portfolio model only after the required custody, pricing, routing, and governance setup

### Capital Efficiency

* **One debt position** instead of a separate loan for each supported collateral type
* **Cross-collateralization** can increase usable capacity relative to separately managed positions under the example assumptions
* **Composition-dependent risk** can improve or worsen with concentration and correlation
* **Asset-specific parameters** apply to each configured collateral type

## The Power of True Portfolio Lending

Portfolio accounting avoids a separate debt position for each supported collateral type. Assets without compatible custody, pricing, and configuration remain outside that position.

With Ripe, configured yield-bearing shares can keep their external economics while contributing to one weighted loan. Additional collateral types can join the same model after compatible vault, pricing, and governance setup.

This isn't just another lending protocol — it's how DeFi lending should have worked from day one.

***

_For technical implementation details, see the_ [_Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core-lending/teller)
