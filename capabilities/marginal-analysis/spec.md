---
type: spec
capability: marginal-analysis
engagement: perfect-competition
date: 2026-09-03
status: Audited  
built_with: ""
---

# Marginal Analysis — model specification

## Purpose
The farmer is using a model for maximizing growing beds of tomatoes, carrots and mesclun.  The goal is to maximize season profit taking into consideration costs and constraints. 

## Inputs — the named contract
| Name | Value | Unit | Source |
|---|---|---|---|
| Season weeks| 36|weeks |case instructions |
|Farm acreage |1.5|acres|case instructions|
|Acres Per Bed| 0.02| acres/bed| Modeling assumption| 
|Total Bed Capacity|64| beds|case instructions|
|Fixed cost|20000|dollars/season|case instructions|
|Farmer hours available|720|hours/Sean|case instructions|
|Farmer Labor Rate|34.72|dollars/hour|case instructions|
|Temp Hours per Worker| 1440| hours/worker|case instructions|
|Temp Labor Rate|17.36| dollars/hour| case instructions
|Max Temp Workers|4| workers| Case instructions| 
|Tomato Max Beds| 20| beds|Case instructions|
|Tomato Price| 8800 |dollars/bed|Case instructions|
|Tomato Labor| 2.50|hours/week/bed | Case instructions|
|Tomato Fertilizer| 880| dollars/bed| Case instructions|
|Tomato Diminishing Return| 10| Percent/bed| Case instructions|
|Carrot Max Beds| 20 | beds|Case instructions|
|Carrot Price| 2094| dollars/bed| Case instructions| 
|Carrot Labor| 0.833| hours/week/bed| Case instructions| 
|Carrot fertilizer| 440| dollars/bed| Case instructions| 
|Carrot Diminishing Return| 2.5| percent/bed| Case instructions| 
|Mesclun Max Beds| 30| beds| Case instructions| 
|Mesclun Price| 2700| dollars/bed| Case instructions|
|Mesclun Labor| 1.25| hours/week/bed| Case instructions|
|Mesclun Fertilizer| 880| dollars/bed| Case instructions|
|Mesclun Diminishing Return| 1.25| percent/bed| Case instructions| 


## Structure
1. Inputs — contains all model assumptions and input values.
2. Marginal Analysis — calculates each crop’s revenue, variable costs, marginal cost, marginal profit, and standalone P≈MC behavior by bed.
3. Farm Model — combines tomatoes, carrots, and mesclun and calculates farm-wide labor, costs, revenue, and season profit.
4. Solver — contains the three crop-bed decision variables, objective profit, and constraints used for optimization.
5. Audit — contains the validation tests and checks used to verify that the model works correctly.

## Calculation logic
 1. Crop labor hours
    LABOR_HRS(q) = q × HRS_PER_BED × WEEKS × (1 + DIM_PCT)^q
2. Total farm labor
    TOTAL_LABOR_HRS = TOMATO_LABOR_HRS + CARROT_LABOR_HRS + MESCLUN_LABOR_HRS
3. Farmer hours used
    FARMER_HRS_USED = MIN(TOTAL_LABOR_HRS, FARMER_HRS_AVAILABLE)
4. Temporary labor hours
    TEMP_HRS = MAX(TOTAL_LABOR_HRS - FARMER_HRS_AVAILABLE, 0)
5. Temporary workers required
    TEMP_WORKERS = TEMP_HRS / TEMP_HRS_PER_WORKER
6. Total labor cost
    TOTAL_LABOR_COST = (FARMER_HRS_USED × FARMER_LABOR_RATE) + (TEMP_HRS × TEMP_LABOR_RATE)
7. Farm-wide blended labor rate
    BLENDED_LABOR_RATE = TOTAL_LABOR_COST / TOTAL_LABOR_HRS
8. Crop labor cost
    CROP_LABOR_COST = CROP_LABOR_HRS × BLENDED_LABOR_RATE
9. Crop revenue
    CROP_REVENUE = CROP_BEDS × CROP_PRICE
10. Crop fertilizer cost
    CROP_FERTILIZER_COST = CROP_BEDS × CROP_FERTILIZER_PER_BED
11. Total revenue
    TOTAL_REVENUE = TOMATO_REVENUE + CARROT_REVENUE + MESCLUN_REVENUE
12. Total fertilizer cost
    TOTAL_FERTILIZER_COST = TOMATO_FERTILIZER_COST + CARROT_FERTILIZER_COST + MESCLUN_FERTILIZER_COST
13. Total variable cost
    TOTAL_VARIABLE_COST = TOTAL_LABOR_COST + TOTAL_FERTILIZER_COST
14. Total cost
    TOTAL_COST = TOTAL_VARIABLE_COST + FIXED_COST
15. Season profit
    SEASON_PROFIT = TOTAL_REVENUE - TOTAL_COST
16. Marginal cost of bed q
    MC(q) = VARIABLE_COST(q) - VARIABLE_COST(q-1)
17. Marginal revenue
    MR = CROP_PRICE
18. Marginal profit of bed q
    MARGINAL_PROFIT(q) = MR - MC(q)
19. Cumulative contribution at q
    CROP_CONTRIBUTION(q) = REVENUE(q) - VARIABLE_COST(q)
20. Land used
    LAND_USED = TOTAL_BEDS × ACRES_PER_BED
21. Total beds
    TOTAL_BEDS = TOMATO_BEDS + CARROT_BEDS + MESCLUN_BEDS

## Conventions
 1. Farmer labor is used first, up to 720 hours for the season.
2. Temporary labor is used after the farmer’s 720 hours are exhausted.
3. Temporary workers may be fractional for this model, with 1,440 hours available per worker and a maximum of 4 workers.
4. Labor is treated as a shared farm-wide resource across tomatoes, carrots, and mesclun rather than separately available to each crop.
5. Farm-wide blended labor rate equals total labor dollars divided by total labor hours; crop labor cost is allocated using this blended rate.
6. Fixed cost is $20,000 per season and does not change with the number or type of beds planted.
7. Marginal analysis is performed separately for each crop to identify its standalone P≈MC behavior; the Farm Model and Solver determine the optimal combined crop mix.
8. Bed decisions are nonnegative whole numbers.
9. The 0.02 acre-per-bed assumption is retained, with total land use limited to 1.5 acres.
10. Solver maximizes final season profit after variable and fixed costs.
11. Solver uses GRG Nonlinear with integer decision to maximize season profit with tomato, carrot, and mesclun bed counts as the changing cells.

## Validation rules
 1. Tomato q=1 hand calculation:
    1 × 2.5 × 36 × 1.10 = 99 labor hours
    The workbook must return 99 hours.
2. Marginal-cost cross-check: Compare at least one intermediate marginal-cost result with the Farm Profit Lab.
3. Published optimal mix check: approximately 10 tomato beds, 20 carrot beds, and 30 mesclun beds.
4. Published season profit check: approximately $42,762.
5. Standalone P≈MC checks: approximately tomato 10 beds, carrot 10 beds, mesclun 6 beds.
6. Formula integrity: every calculated cell must contain a formula rather than a manually entered calculated value.
7. Excel error check: no #VALUE!, #DIV/0!, #REF!, #N/A, or other formula errors.
8. Constraint checks: tomato ≤20, carrot ≤20, mesclun ≤30, total beds ≤64, temp workers ≤4, land used ≤1.5 acres, and crop-bed decisions are nonnegative integers.
9. Solver starting-point test: run Solver from both 0/0/0 and 20/0/0 and record whether the solutions agree or show path dependence.
10. Tomato MC behavior: confirm the expected marginal-cost dip around tomato bed 6; do not automatically treat the dip as a formula error.


## Outputs
1. Optimal tomato beds, carrot beds, and mesclun beds.
2. Total beds planted.
3. Revenue by crop and total farm revenue.
4. Labor hours by crop and total farm labor hours.
5. Farmer hours used and temporary labor hours required.
6. Temporary workers required.
7. Total labor cost and farm-wide blended labor rate.
8. Fertilizer cost by crop and total fertilizer cost.
9. Total variable cost, total cost, and final season profit.
10. Marginal cost and marginal profit by bed for each crop.
11. Cumulative contribution by crop.
12. Standalone P≈MC crossing for each crop.
13. Land used.
14. Constraint status showing whether each constraint is binding or slack.
15. Solver’s optimized crop mix and maximum season profit.
16. Additional-bed decision flag indicating whether another bed is profitable or prevented by a constraint.

## Audit findings
1.  First Audit was test tomato q=1 calculation which resulted in 99 hours, which matched with the workbook calculation confirming that the labor hours formula is working correctly. 
2. Solver was run from two different starting points, 0/0/0 and 20/0/0. Both resulted in the optimal mix of 10/20/30, so the model did not show path dependence in these tests.
3.  The workbook session profit was $42775.16 compared with $42762 in the farm profit lab.  The optimal crop mix was the same, so the small profit difference appears to be due to rounding or input precision rather than a different model result.  
4. The small difference appears to be due to rounding or input precision. The marginal profit result from the model was consistent with the Farm Profit Lab estimate for adding +1 tomato bed.
5. The marginal  cost dip despite increased labor hours can be explained by the increased use of temporary labor( at 17.36/hour) after the farmer has exhausted the 720 hour allocation. With additional beds grown the marginal costs begin to rise again due to diminishing returns.  This is not a formula error. 
6.  The workbook showed that calculations were executed without error values after confirming with Excel's formula error check. 
7.  The formula integrity check with Excel's use of "show formula" verified that the results were generated by formulas rather than pasted/hard-coded calculated values. 

