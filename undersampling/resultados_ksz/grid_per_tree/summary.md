# Results: full_ksz / grid_per_tree

Mode **grid**, r applied **per_tree**. r values: 0.8, 0.6, 0.4, 0.3, 0.2, 0.15, 0.1, 0.07, 0.05, 0.04, 0.03, 0.025, 0.02, 0.015, 0.01.

Machine: Windows-11-10.0.26200-SP0, 12 cores, 15.2 GB RAM. Peak memory: 5.98 GB.

## 1. Choosing r (validation month, 10 seeds)

every r judged on its own (95.0% interval): least time among the r not worse on validation profit; ties within 5% of baseline time -> largest r.

| r | trials with r | Share contacted | Profit | Diff vs r=1 | 95% interval | Passes | Time used to choose | Winner |
|---|---|---|---|---|---|---|---|---|
| 1 | 50 | 3.0% | 205.67M | 0.00M | [0.00M, 0.00M] | — | 2.96 h |  |
| 0.8 | 50 | 3.0% | 208.09M | 2.42M | [0.00M, 5.17M] | yes | 2.26 h |  |
| 0.6 | 50 | 3.3% | 210.40M | 4.73M | [1.32M, 8.36M] | yes | 1.62 h |  |
| 0.4 | 50 | 3.1% | 210.07M | 4.40M | [1.21M, 7.81M] | yes | 1.89 h |  |
| 0.3 | 50 | 3.1% | 210.07M | 4.40M | [1.87M, 6.82M] | yes | 1.42 h |  |
| 0.2 | 50 | 3.1% | 202.15M | -3.52M | [-6.38M, -0.33M] | no | 3,417.9 s |  |
| 0.15 | 50 | 3.7% | 205.59M | -0.08M | [-2.06M, 1.79M] | no | 1.01 h |  |
| 0.1 | 50 | 3.1% | 210.29M | 4.62M | [2.86M, 6.49M] | yes | 3,359.0 s |  |
| 0.07 | 50 | 3.2% | 210.84M | 5.17M | [2.97M, 8.03M] | yes | 3,445.4 s |  |
| 0.05 | 50 | 3.3% | 207.32M | 1.65M | [-1.98M, 4.62M] | yes | 3,167.6 s |  |
| 0.04 | 50 | 3.0% | 208.75M | 3.08M | [0.22M, 5.94M] | yes | 2,280.5 s | **yes** |
| 0.03 | 50 | 3.2% | 202.48M | -3.19M | [-7.26M, 1.32M] | no | 3,048.5 s |  |
| 0.025 | 50 | 3.4% | 204.82M | -0.85M | [-4.92M, 3.55M] | no | 3,036.0 s |  |
| 0.02 | 50 | 2.9% | 203.91M | -1.76M | [-4.29M, 0.66M] | no | 3,268.8 s |  |
| 0.015 | 50 | 3.7% | 203.17M | -2.50M | [-5.14M, 0.36M] | no | 3,052.7 s |  |
| 0.01 | 50 | 3.4% | 206.25M | 0.58M | [-1.40M, 2.67M] | yes | 3,583.0 s |  |

Winner: **r = 0.04**.

## 2. Winner (r = 0.04) vs baseline on the test month

Top 3 configurations × 20 seeds each side, paired by rank and seed. Margin δ = 1% of the baseline mean. Decisive: profit (at each model's share, fixed on validation) and AUC.

| Metric | Baseline | Winner | Mean diff | 95% interval | Wilcoxon p | Not worse than −δ |
|---|---|---|---|---|---|---|
| profit | 280.97M | 281.11M | 0.14M | [-1.40M, 1.67M] | 0.994 | yes |
| auc | 0.93665 | 0.93732 | 0.00067 | [0.00050, 0.00083] | 1.57e-08 | yes |
| profit_peak | 287.42M | 287.09M | -0.33M | [-1.58M, 0.90M] | 0.721 | yes |

| Time | Baseline | Winner |
|---|---|---|
| prep_seconds | 193.3 s | 193.3 s |
| search_seconds | 2.90 h | 2,234.0 s |
| final_train_seconds | 211.8 s | 46.5 s |
| predict_seconds | 2.5 s | 2.6 s |
| total_seconds | 3.01 h | 2,476.5 s |
| seconds_per_tree (search) | 0.4307 | 0.0547 |

Total time reduction: **77.2%** (required 30%); search only 78.6%; final training 78.0%.

## 3. r curve (test month, best configuration of each r)

| r | Share | Profit | Diff vs r=1 | 95% interval | AUC | Not worse | Time | Train s |
|---|---|---|---|---|---|---|---|---|
| 1 | 3.0% | 279.48M | 0.00M | [0.00M, 0.00M] | 0.93688 | — | 2.96 h | 211.8 |
| 0.8 | 3.0% | 278.60M | -0.88M | [-3.41M, 1.49M] | 0.93609 | no | 2.26 h | 281.1 |
| 0.6 | 3.3% | 281.76M | 2.28M | [-0.08M, 4.70M] | 0.93725 | yes | 1.62 h | 235.4 |
| 0.4 | 3.1% | 281.46M | 1.98M | [-0.44M, 4.62M] | 0.93672 | yes | 1.89 h | 222.9 |
| 0.3 | 3.1% | 283.00M | 3.52M | [1.04M, 6.00M] | 0.93737 | yes | 1.42 h | 123.3 |
| 0.2 | 3.1% | 278.66M | -0.82M | [-4.29M, 2.53M] | 0.93608 | no | 3,417.9 s | 89.1 |
| 0.15 | 3.7% | 280.56M | 1.07M | [-1.24M, 3.38M] | 0.93744 | yes | 1.01 h | 89.9 |
| 0.1 | 3.1% | 283.61M | 4.12M | [1.65M, 6.66M] | 0.93698 | yes | 3,359.3 s | 97.0 |
| 0.07 | 3.2% | 284.19M | 4.70M | [1.84M, 7.45M] | 0.93710 | yes | 3,445.4 s | 111.6 |
| 0.05 | 3.3% | 283.86M | 4.37M | [1.29M, 7.78M] | 0.93615 | yes | 3,167.6 s | 54.5 |
| 0.04 | 3.0% | 283.06M | 3.58M | [0.88M, 6.11M] | 0.93753 | yes | 2,280.5 s | 46.5 |
| 0.03 | 3.2% | 286.00M | 6.52M | [2.94M, 9.71M] | 0.93703 | yes | 3,048.7 s | 56.6 |
| 0.025 | 3.4% | 283.52M | 4.04M | [1.18M, 6.96M] | 0.93607 | yes | 3,036.3 s | 58.7 |
| 0.02 | 2.9% | 279.54M | 0.06M | [-2.64M, 2.64M] | 0.93736 | yes | 3,268.7 s | 47.0 |
| 0.015 | 3.7% | 283.31M | 3.82M | [0.58M, 6.85M] | 0.93682 | yes | 3,053.5 s | 117.4 |
| 0.01 | 3.4% | 284.35M | 4.87M | [2.06M, 7.67M] | 0.93690 | yes | 3,583.5 s | 106.3 |

r that keep profit and AUC: 0.6, 0.4, 0.3, 0.15, 0.1, 0.07, 0.05, 0.04, 0.03, 0.025, 0.02, 0.015, 0.01. r_flat (time flattens at): 0.04. Useful zone: [0.01, 0.04].

Whole exploration of this experiment: 33.19 h (searches 19.41 h, 400 final trainings 13.79 h).

## Verdict

- r_passed_validation: **True**
- about_the_same_auc_and_profit: **True**
- clearly_less_time: **True**
- time_flattens: **True**
- useful_zone: **True**
- hypothesis_confirmed: **True**
