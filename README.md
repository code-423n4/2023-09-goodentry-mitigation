# Good Entry - Mitigation Review details
- Total Prize Pool: $26,000 USDC 
- [Warden guidelines for C4 mitigation reviews](https://code4rena.notion.site/Guidelines-for-C4-mitigation-reviews-ed10fc5cfbf640bd8dcec66f38b343c4)
- Submit findings [using the C4 form](https://code4rena.com/contests/2023-09-good-entry-mitigation-review/submit)
- Starts September 06, 2023 20:00 UTC 
- Ends September 11, 2023 20:00 UTC 

## Important note 

Each warden must submit a mitigation review for *every High and Medium finding* from the parent audit that is listed as in-scope for the mitigation review. **Incomplete mitigation reviews will not be eligible for awards.**

## Findings being mitigated

Mitigations of all High and Medium issues will be considered in-scope and listed here.

- [H-01: When price is within within position's range, deposit at TokenisableRange can cause loss of fund](https://github.com/code-423n4/2023-08-goodentry-findings/issues/373)
- [H-02: Unused funds are not returned and not counted in GeVault](https://github.com/code-423n4/2023-08-goodentry-findings/issues/325)
- [H-03: Overflow can still happened when calculating priceX8 inside poolMatchesOracle operation](https://github.com/code-423n4/2023-08-goodentry-findings/issues/140)
- [H-04: TokenisableRange's incorrect accounting of non-reinvested fees in "deposit" exposes the fees to a flash-loan attack](https://github.com/code-423n4/2023-08-goodentry-findings/issues/85)
- [H-05: V3Proxy swapTokensForExactETH does not send back to the caller the unused input tokens](https://github.com/code-423n4/2023-08-goodentry-findings/issues/64)
- [H-06: Incorrect Solidity version in FullMath.sol can cause permanent freezing of assets for arithmetic underflow-induced revert](https://github.com/code-423n4/2023-08-goodentry-findings/issues/58)
- [M-01: V3 Proxy does not send funds to the recipient, instead it sends to the msg.sender](https://github.com/code-423n4/2023-08-goodentry-findings/issues/463)
- [M-02: Incorrect parameters passed to UniV3 may cause funds stuck in the vault](https://github.com/code-423n4/2023-08-goodentry-findings/issues/397)
- [M-03: Incorrect boundaries check in GeVault's "getActiveTickIndex" can temporarily freeze assets due to Index out of bounds error](https://github.com/code-423n4/2023-08-goodentry-findings/issues/379)
- [M-04: First depositor can break minting of liquidity shares in GeVault](https://github.com/code-423n4/2023-08-goodentry-findings/issues/367)
- [M-05: addDust does not achieve the goal correctly and may overflow revert](https://github.com/code-423n4/2023-08-goodentry-findings/issues/358)
- [M-06: User can steal refunded underlying tokens from initRange operation inside RangeManager](https://github.com/code-423n4/2023-08-goodentry-findings/issues/254)
- [M-07: Incorrect calculations in deposit() function in TokenisableRange.sol can make the users suffer from immediate loss](https://github.com/code-423n4/2023-08-goodentry-findings/issues/202)
- [M-08: Return value of low level call not checked.](https://github.com/code-423n4/2023-08-goodentry-findings/issues/83)

## Overview of changes

Simple errors like the sqrt version were corrected.
The main change is to the fee system in TokenizsableRange. Because repaying a TR debt exactly is tricky and introduces several problems (addDust, fee clawing on deposit...), the system has been changed.
TokenisableRange fees aren't compounded anymore directly in TR, but are sent to the corresponding GeVault. The Gevault address is queried from a list (new addition to RoeRouter) (or to treasury is no such vault exists).
We added a depositExactly function to TR, which takes an additional expectedAmount parameter. When depositing in TR, if because of rounding the difference between the expected liquidity and the actually minted liquidity is dust (as defined by: value is 0, or lower than 1 unit of the underlying token), then mint the expected liquidity.

Another set of changes is for the GeVaults: the activeIndex system has been changed so that the index point to the first tick above current price. If 2 ticks below or above exist, it tries to deposit assets in them (and gracefully ignores errors so as to prevent revert, eg when price is inside a tick).
Instead of depositing half of the assets into each of the 2 ticks above and below, this has been parameterized, allowing to change asset distribution in case of high volatility.


## Mitigations to be reviewed

### Branch

All PR can be seen [here](https://github.com/GoodEntry-io/ge/pulls?q=), and have been linked in each issue's comments.

### Individual PRs

Wherever possible, mitigations should be provided in separate pull requests, one per issue. If that is not possible (e.g. because several audit findings stem from the same core problem), then please link the PR to all relevant issues in your findings repo. 

| URL | Mitigation of | Purpose | 
| ----------- | ------------- | ----------- |
| https://github.com/GoodEntry-io/ge/pull/4 | H-01, H-04 | Remove complex fee clawing strategy | 
| https://github.com/GoodEntry-io/ge/commit/a8ba6492b19154c72596086f5531f6821b4a46a2 | H-02 | Take unused funds into account for TVL | 
| https://github.com/GoodEntry-io/ge/pull/3 | H-03 | Scale down sqrtPriceX96 to prevent overflow | 
| https://github.com/GoodEntry-io/ge/pull/2 | H-05 | Send back unused funds to user | 
| https://github.com/GoodEntry-io/ge/commit/8b0feaec0005937c8e6c7ef9bf039a0c2498529a | H-06 | Use correct Uniswap for sol ^0.8 libs | 
| https://github.com/GoodEntry-io/ge/pull/10 | M-01 | Added explicit require msg.sender == to | 
| https://github.com/GoodEntry-io/ge/commit/bbbac57c110223f45851494971a34f57c55922c7 | M-02 | Prevent collect from reverting by adding a check that it doesnt try to collect 0 | 
| https://github.com/GoodEntry-io/ge/pull/11 | M-03 | Reworked activeTickIndex as per desc above | 
| https://github.com/GoodEntry-io/ge/pull/8 | M-05, M-07 | Removed addDust mechanism, replaced by depositExactly in TR | 
| https://github.com/GoodEntry-io/ge/pull/3 | M-06 | Added return value check | 

## Out of Scope

M-04 is not really a problem as the team deploys the contract and can deposit a very small initial amount. The attack would then steal a negligible amount (likely less than the gas cost)
