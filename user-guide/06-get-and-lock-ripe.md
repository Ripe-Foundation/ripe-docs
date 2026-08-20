---
description: Get RIPE, put it to work, and understand its locking rules
---

# Get RIPE and Lock It

RIPE is the protocol's token. Locking it boosts your rewards and is how you take part in [protocol governance](../governance-and-economics/02-governance.md).

**Step 1.** Get RIPE: press **Get RIPE** on the RIPE page to swap for it, or **Bridge RIPE** if you hold it on Base.

Where RIPE rewards come from, so you know what you're joining: the protocol distributes a fixed amount daily, split across RIPE LP stakers, RIPE stakers, the GREEN/USDG and sGREEN stability pools, and borrowers. The RIPE page shows the live split and the daily amount. The [RIPE Rewards guide](../earning-and-rewards/03-ripe-rewards.md) explains how those rewards are calculated.

**There are two ways to put RIPE to work**, shown side by side on the RIPE page. Deposit RIPE on its own, covered in the steps below. Or pair RIPE with WETH to get **RIPE/WETH LP** and deposit that instead, using the **Get RIPE/WETH LP** link which takes you to Uniswap V2. The LP route carries the usual liquidity-provider tradeoff: it earns more, and your position shifts between the two tokens as their prices move, so you don't come out holding the same amount of RIPE you put in. Both routes pay rewards in RIPE, so the claiming rules further down apply either way.

**Step 2.** Go to the **RIPE** page (or the **Earn** page), find the RIPE row, and press **Deposit**.

![Depositing into an Earn position](../.gitbook/assets/user-guide-14-sgreen-deposit.png)

**Step 3.** Enter an amount using the 25% / 50% / 75% / MAX buttons or by typing it.

**Step 4.** Choose your lock duration. Depositing is the one place you set this yourself; the duration attached to claimed rewards is fixed by the protocol and you don't get a say in it.

Worth understanding before you pick: your RIPE position carries a **single blended lock duration**, not one per deposit. Every new deposit is weighted into that figure by size and duration. Deposit a large amount at a short lock and the blended duration across your whole position comes down. Deposit at a long lock and it goes up. Longer means a bigger rewards boost, and it applies to everything you hold, not just the new money.

**Step 5.** Press **Deposit** and confirm in your wallet. Your position and its rewards appear on the page.

## Claiming Rewards Always Locks Something

This is the part of Ripe most likely to catch you out, so read it before you claim anything.

**You can't take RIPE rewards fully liquid.** Open the claim dialog and it splits your claimable amount in two: an **Unlocked Amount** that goes to your wallet, and a **Stake Amount** that gets staked whether you want it or not. The split is currently 25% to your wallet and 75% staked, so claiming 1.47 RIPE sends you 0.36 and stakes 1.10. That ratio is a protocol setting rather than a permanent rule, so read the two numbers in the dialog rather than assuming the split you last saw.

The **Stake All** toggle only shifts the ratio further. Leave it off and you receive the unlocked quarter. Turn it on and everything is staked, with nothing reaching your wallet. There is no setting that gets you the full amount liquid.

![The claim dialog. Stake All is switched off here, and 75% of the claim is still being staked](../.gitbook/assets/user-guide-15-claim-dialog.png)

Then the part people don't expect:

**Staking rewards applies a lock to your entire deposited RIPE, not only the rewards being staked.** The dialog shows this as **Weighted Lock Duration**, blending your existing position and the newly staked rewards into one lock covering all of it. In that same example, rewards staked at a fresh 11 month 28 day duration produced a weighted lock of 7 months 3 days across the whole position, with a 19.62% rewards boost to match.

So claiming is never a neutral act. Every claim re-locks your full RIPE position for a recalculated duration.

**Read these four numbers before pressing Claim:**

* **Unlocked Amount**: what actually reaches your wallet.
* **Stake Amount**: what gets staked instead.
* **Weighted Lock Duration**: how long your whole position is committed once you confirm.
* **Final Rewards Boost**: what you earn in return.

## Leaving a Lock Early Costs 80%

If you exit a lock before it ends, the penalty is **80% of your balance**. Not 80% of the rewards, not a sliding scale that softens near the end: four fifths of what you pull out. The [governance guide](../governance-and-economics/02-governance.md#early-exit-the-nuclear-option) explains the other consequences of an early exit.

That number is the whole reason to take the lock duration seriously at deposit, and to check the Weighted Lock Duration before every claim. RIPE you deposit here is money set aside for the better part of a year. If there's a chance you'll want it sooner, the honest move is to hold RIPE in your wallet unlocked and skip the boost.

Next: [See everything you can deposit on Earn](07-what-you-can-deposit-on-earn.md).
