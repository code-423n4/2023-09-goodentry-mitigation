# Good Entry - Mitigation Review details
- Total Prize Pool: $26,000 USDC 
- [Warden guidelines for C4 mitigation reviews](https://code4rena.notion.site/Guidelines-for-C4-mitigation-reviews-ed10fc5cfbf640bd8dcec66f38b343c4)
- Submit findings [using the C4 form](https://code4rena.com/contests/YYYY-MM-sponsorName-mitigation-review/submit)
- Starts TBD XXX XXX XX 20:00 UTC (ex. `Starts March 22, 2023 20:00 UTC`)
- Ends TBD XXX XXX XX 20:00 UTC (ex. `Ends March 30, 2023 20:00 UTC`)

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

[ ⭐️ SPONSORS ADD INFO HERE ]

## Overview of changes

Please provide context about the mitigations that were applied if applicable and identify any areas of specific concern.

## Mitigations to be reviewed

### Branch
[ ⭐️ SPONSORS ADD A LINK TO THE BRANCH IN YOUR REPO CONTAINING ALL PRS ]

### Individual PRs
[ ⭐️ SPONSORS ADD ALL RELEVANT PRs TO THE TABLE BELOW:]

Wherever possible, mitigations should be provided in separate pull requests, one per issue. If that is not possible (e.g. because several audit findings stem from the same core problem), then please link the PR to all relevant issues in your findings repo. 

| URL | Mitigation of | Purpose | 
| ----------- | ------------- | ----------- |
| https://github.com/your-repo/sample-contracts/pull/XXX | H-01 | This mitigation does XYZ | 

## Out of Scope

Please list any High and Medium issues that were judged as valid but you have chosen not to fix.
