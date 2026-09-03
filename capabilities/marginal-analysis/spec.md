# Spec: Marginal Analysis

## Method

Rank every option by its marginal profit at the next unit, and allocate one unit at a time to whichever option currently has the highest marginal profit, continuing until either (a) a marginal profit turns negative for all remaining options, or (b) a shared constraint (land, labor, budget) is exhausted. This is the P = MC decision rule applied across multiple competing options simultaneously, instead of one at a time.

## Assumptions

- Each option's cost curve (and therefore its marginal cost) can be estimated independently of how much of the other options is produced.
- The resources being allocated (e.g., beds, acres, labor hours) are shared across all options and constrained in total.
- Diminishing returns apply within each option as more units of it are added.

## Inputs / Outputs

Inputs: a marginal cost (or marginal profit) schedule for each option, and the total available amount of each shared constraint. Outputs: the unit-by-unit allocation across options, the point at which each option stops being worth adding more of, and the resulting total profit. See the model file in this folder for the reproducible worksheet.
