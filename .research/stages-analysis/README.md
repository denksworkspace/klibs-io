MVP of the new stage system:

C - number of commits

P - number of merged pull requests

I - number of closed issues

EWMA is used to smooth outliers, so that a missed value does not strongly drop the result.

EWMA_cur = alpha * Y + (1 - alpha) * EWMA_prev (where alpha = 0.2 (estimate of the current week’s weight); Y - value (we calculate separately C; P; I) for three different EWMAs; EWMA_prev - value for the previous period). We calculate EWMA separately for the last 7 - 12 weeks (EWMA_target), and separately for 1 - 6 weeks (EWMA_actual), and take EWMA_actual / EWMA_target.

EWMA_1 = Y

Thus we will have parameters:

EWMA_Y_{target/actual}

EWMA_ratio_Y - if EWMA_Y_target is not equal to 0

Statuses:

For those that have insufficient data:

- archive - given to projects whose GitHub is archived
- recently created - given to projects that were created less than half a year ago
- on pause - projects where EWMA_C_target = 0 and EWMA_C_actual = 0 and EWMA_P_target = 0 and EWMA_P_actual = 0

T = 0.9 - threshold

- sporadic maintenance - given to projects where 0 < sum(C + P over the last 12 weeks) <= 12
- maintenance - (given to projects where EWMA_C_ratio >= T) or (EWMA_C_target = 0 and EWMA_C_actual > 0)
- active maintenance - (given to projects with the maintenance status) and (at least one of (EWMA_P_ratio >= T and EWMA_P_target == 0) or (EWMA_I_ratio >= T and EWMA_I_target == 0))
- stable/moderate active - in all other cases

Demonstration: [link](images/ewma-system-demonstration.pdf)

[YouTrack issue](https://youtrack.jetbrains.com/issue/KTL-4654/Research-and-evaluate-stages-of-development-for-a-project)
