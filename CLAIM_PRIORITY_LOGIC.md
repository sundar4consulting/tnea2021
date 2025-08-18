# Claims Priority Decision Logic

This document describes the logic used to assign a priority ranking to insurance claims in the claims table UI.

## Priority Scoring Criteria
Each claim is assigned a **priority score** based on the following attributes:

- **Claim Amount**: Higher claim amounts are prioritized.
- **Date Filed**: More recent claims are prioritized.
- **Status**: Approved claims are prioritized over denied claims.

## Scoring Formula
The score for each claim is calculated as follows:

- **Amount**: Each $1,000 in the claim amount adds 10 points to the score.
- **Date**: The number of days since 2025-01-01 is added to the score (more recent = higher score).
- **Status**: If the claim is `Approved`, add 20 points. If `Denied`, add 0 points.

**Formula:**

```
score = (floor(amount / 1000) * 10)
      + (days since 2025-01-01)
      + (status == 'Approved' ? 20 : 0)
```

## Example Calculation
| Claim ID | Status   | Date Filed  | Amount | Score Calculation                      | Score |
|----------|----------|-------------|--------|----------------------------------------|-------|
| CLM001   | Approved | 2025-08-01  | 2000   | (20) + (212) + (20)                   | 252   |
| CLM002   | Denied   | 2025-07-15  | 500    | (0) + (195) + (0)                     | 195   |
| CLM003   | Approved | 2025-08-10  | 3500   | (30) + (221) + (20)                   | 271   |

## Priority Ranking
After calculating the score for each claim, claims are sorted in descending order of score. The highest score receives priority rank 1, the next highest rank 2, and so on.

## Customization
You can adjust the weights or add more attributes to the scoring logic as needed for your business requirements.
