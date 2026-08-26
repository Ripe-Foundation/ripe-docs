---
description: Get RIPE, put it to work, and understand its locking rules
---

# Get RIPE and Lock It

RIPE is the protocol's token. Locking it boosts your rewards and builds the governance points that will be your voting weight when onchain [governance](../governance-and-economics/02-governance.md) goes live.

**Step 1.** Get RIPE: press **Get RIPE** on the RIPE page to swap for it, or **Bridge RIPE** if you hold it on another network. Two other ways to acquire it are [Ripe Bonds](../governance-and-economics/03-bonds.md) (pay stablecoins, get RIPE at the epoch price, optionally locked for a bonus) and the [RIPE Reserve Engine](../governance-and-economics/04-reserve-engine.md) (pay up front, get a larger allocation that vests over time). Both depend on what's enabled on your network.

Where RIPE rewards come from, so you know what you're joining: the protocol distributes RIPE every block, split across stakers (RIPE, RIPE LP, and Stability Pool depositors) and borrowers. The RIPE page shows the live split and the daily amount. The [RIPE Rewards guide](../earning-and-rewards/03-ripe-rewards.md) explains how those rewards are calculated.

**There are two ways to put RIPE to work**, shown side by side on the RIPE page. Deposit RIPE on its own, covered in the steps below. Or pair RIPE with the network's ETH to get a RIPE LP token and deposit that instead, using the **Get RIPE/WETH LP** link (Uniswap V2 in the screenshots). The LP route carries the usual liquidity-provider tradeoff: it earns more, and your position shifts between the two tokens as their prices move, so you don't come out holding the same amount of RIPE you put in. Both routes pay rewards in RIPE, so the claiming rules further down apply either way.

**Step 2.** Go to the **RIPE** page (or the **Earn** page), find the RIPE row, and press **Deposit**.

![The Earn deposit dialog, shown here for sGREEN; the RIPE version adds a lock-duration picker](../.gitbook/assets/user-guide-14-sgreen-deposit.png)

**Step 3.** Enter an amount using the 25% / 50% / 75% / MAX buttons or by typing it.

**Step 4.** Choose your lock duration. Depositing is the one place you set this yourself; the lock attached to claimed rewards is a protocol setting. You can extend a lock later, never shorten it directly.

Worth understanding before you pick: your RIPE position carries a **single blended lock duration**, not one per deposit. Every new deposit is weighted into that figure by size and duration. Deposit a large amount at a short lock and the blended duration across your whole position comes down. Deposit at a long lock and it goes up. Longer means a bigger rewards boost, and it applies to everything you hold, not just the new money.

One more thing the [governance guide](../governance-and-economics/02-governance.md) explains in detail: your lock bonus is banked each time your position is touched — a deposit, a claim, an extension. Touch it while the lock is still long and the boost is yours; leave it untouched until after it expires and that stretch earns no bonus.

**Step 5.** Press **Deposit** and confirm in your wallet. Your position and its rewards appear on the page.

## How Claiming Rewards Applies a Lock

This is the part of Ripe most likely to catch you out, so read it before you claim anything.

Open the claim dialog and it splits your claimable amount in two: an **Unlocked Amount** that goes to your wallet, and a **Stake Amount** that gets staked. The split is a protocol setting (25% liquid / 75% staked in the screenshot), so read the two numbers in the dialog rather than assuming the split you last saw.

The **Stake All** toggle only shifts the ratio further. Leave it off and you receive the unlocked portion. Turn it on and everything is staked, with nothing reaching your wallet. There is no setting that gets you the full amount liquid unless the protocol's auto-stake ratio is zero.

![The claim dialog. Stake All is switched off, and 75% of the claim is still being staked](../.gitbook/assets/user-guide-15-claim-dialog.png)

Then the part people don't expect:

**Staking rewards applies a lock to your entire deposited RIPE, not only the rewards being staked.** The dialog shows this as **Weighted Lock Duration**, blending your existing position and the newly staked rewards into one lock covering all of it. In the screenshot, rewards staked at a fresh 11-month-28-day lock produced a weighted lock of 7 months 3 days across the whole position, and a new rewards boost to match. The exact boost depends on your deployment's lock terms, so read the dialog rather than the screenshot.

So claiming is never a neutral act. Every claim that stakes something re-locks your full RIPE position for a recalculated duration.

**Read these four numbers before pressing Claim:**

* **Unlocked Amount**: what actually reaches your wallet.
* **Stake Amount**: what gets staked instead.
* **Weighted Lock Duration**: how long your whole position is committed once you confirm.
* **Final Rewards Boost**: what you earn in return.

## Leaving a Lock Early Has a Cost

If early exit is enabled on your network, leaving a lock before it ends costs a large share of your balance — the protocol setting, not a sliding scale (80% in the example on the governance page). Releasing the lock and withdrawing are two separate steps, and a bad-debt freeze can block both. The [governance guide](../governance-and-economics/02-governance.md#early-exit-the-nuclear-option) explains the full mechanism.

That number is the whole reason to take the lock duration seriously at deposit, and to check the Weighted Lock Duration before every claim. RIPE you deposit here is money set aside. If there's a chance you'll want it sooner, the honest move is to hold RIPE in your wallet unlocked and skip the boost.

Next: [See everything you can deposit on Earn](07-what-you-can-deposit-on-earn.md).
