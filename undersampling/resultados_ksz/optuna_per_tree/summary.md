# Results: full_ksz / optuna_per_tree

Mode **optuna**, r applied **per_tree**. r values: 0.8, 0.6, 0.4, 0.3, 0.2, 0.15, 0.1, 0.07, 0.05, 0.04, 0.03, 0.025, 0.02, 0.015, 0.01.

Machine: Windows-11-10.0.26200-SP0, 12 cores, 15.2 GB RAM. Peak memory: 5.46 GB.

## 1. Choosing r (validation month, 10 seeds)

every r judged on its own (95.0% interval): least time among the r not worse on validation profit; ties within 5% of baseline time -> largest r.

| r | trials with r | Share contacted | Profit | Diff vs r=1 | 95% interval | Passes | Time used to choose | Winner |
|---|---|---|---|---|---|---|---|---|
| 1 | 50 | 3.0% | 205.67M | 0.00M | [0.00M, 0.00M] | — | 212.3 s |  |
| 0.8 | 2 | 3.3% | 201.38M | -4.29M | [-6.82M, -1.21M] | no | 142.8 s |  |
| 0.6 | 1 | 3.3% | 196.32M | -9.35M | [-12.21M, -6.82M] | no | 18.6 s |  |
| 0.4 | 1 | 3.0% | 189.83M | -15.84M | [-18.37M, -13.42M] | no | 17.7 s |  |
| 0.3 | 5 | 3.3% | 203.36M | -2.31M | [-4.51M, 0.00M] | no | 52.6 s |  |
| 0.2 | 1 | 3.1% | 184.66M | -21.01M | [-25.19M, -17.05M] | no | 14.6 s |  |
| 0.15 | 1 | 2.8% | 198.38M | -7.29M | [-11.03M, -3.33M] | no | 17.3 s |  |
| 0.1 | 29 | 3.1% | 207.43M | 1.76M | [-0.88M, 4.51M] | yes | 45.3 s | **yes** |
| 0.07 | 1 | 3.4% | 188.98M | -16.69M | [-20.76M, -12.40M] | no | 10.7 s |  |
| 0.05 | 1 | 2.6% | 193.54M | -12.13M | [-14.77M, -9.16M] | no | 41.6 s |  |
| 0.04 | 1 | 3.6% | 182.05M | -23.62M | [-28.02M, -19.55M] | no | 20.1 s |  |
| 0.03 | 1 | 3.5% | 137.61M | -68.06M | [-73.45M, -62.45M] | no | 16.7 s |  |
| 0.025 | 1 | 3.0% | 204.79M | -0.88M | [-3.19M, 1.65M] | no | 28.7 s |  |
| 0.02 | 1 | 3.2% | 200.83M | -4.84M | [-9.57M, -0.22M] | no | 73.0 s |  |
| 0.015 | 3 | 3.3% | 207.43M | 1.76M | [-1.54M, 5.06M] | yes | 51.3 s |  |
| 0.01 | 1 | 2.9% | 190.82M | -14.85M | [-18.15M, -11.77M] | no | 82.3 s |  |

Winner: **r = 0.1**.

## 2. Winner (r = 0.1) vs baseline on the test month

Top 3 configurations × 20 seeds each side, paired by rank and seed. Margin δ = 1% of the baseline mean. Decisive: profit (at each model's share, fixed on validation) and AUC.

| Metric | Baseline | Winner | Mean diff | 95% interval | Wilcoxon p | Not worse than −δ |
|---|---|---|---|---|---|---|
| profit | 280.97M | 282.09M | 1.12M | [-0.24M, 2.48M] | 0.101 | yes |
| auc | 0.93665 | 0.93715 | 0.00050 | [0.00026, 0.00075] | 0.000292 | yes |
| profit_peak | 287.42M | 287.91M | 0.49M | [-0.67M, 1.60M] | 0.359 | yes |

| Time | Baseline | Winner |
|---|---|---|
| prep_seconds | 195.1 s | 195.1 s |
| search_seconds | 2.90 h | 3,253.0 s |
| final_train_seconds | 211.8 s | 45.6 s |
| predict_seconds | 2.5 s | 1.3 s |
| total_seconds | 3.01 h | 3,495.0 s |
| seconds_per_tree (search) | 0.4307 | 0.1127 |

Total time reduction: **67.8%** (required 30%); search only 68.8%; final training 78.5%.

## 3. r curve (test month, best configuration of each r)

| r | Share | Profit | Diff vs r=1 | 95% interval | AUC | Not worse | Time | Train s |
|---|---|---|---|---|---|---|---|---|
| 1 | 3.0% | 279.48M | 0.00M | [0.00M, 0.00M] | 0.93688 | — | 211.8 s | 211.8 |
| 0.8 | 3.3% | 278.57M | -0.91M | [-3.22M, 1.18M] | 0.93548 | no | 142.8 s | 142.8 |
| 0.6 | 3.3% | 274.89M | -4.59M | [-7.18M, -2.34M] | 0.93335 | no | 18.7 s | 18.7 |
| 0.4 | 3.0% | 266.78M | -12.71M | [-15.84M, -9.68M] | 0.93436 | no | 17.7 s | 17.7 |
| 0.3 | 3.3% | 280.39M | 0.91M | [-1.35M, 3.22M] | 0.93703 | yes | 52.8 s | 52.8 |
| 0.2 | 3.1% | 262.16M | -17.32M | [-21.23M, -13.09M] | 0.93171 | no | 14.6 s | 14.6 |
| 0.15 | 2.8% | 264.61M | -14.88M | [-17.57M, -12.29M] | 0.93407 | no | 17.3 s | 17.3 |
| 0.1 | 3.1% | 282.51M | 3.02M | [0.11M, 5.88M] | 0.93632 | yes | 45.6 s | 45.6 |
| 0.07 | 3.4% | 267.96M | -11.52M | [-14.44M, -8.44M] | 0.93111 | no | 10.7 s | 10.7 |
| 0.05 | 2.6% | 265.10M | -14.38M | [-17.68M, -11.74M] | 0.93245 | no | 41.4 s | 41.4 |
| 0.04 | 3.6% | 263.81M | -15.68M | [-19.75M, -11.71M] | 0.92814 | no | 20.1 s | 20.1 |
| 0.03 | 3.5% | 217.00M | -62.48M | [-67.32M, -57.64M] | 0.89969 | no | 16.7 s | 16.7 |
| 0.025 | 3.0% | 282.67M | 3.19M | [0.49M, 5.94M] | 0.93529 | yes | 28.8 s | 28.8 |
| 0.02 | 3.2% | 278.74M | -0.74M | [-3.27M, 1.79M] | 0.93349 | no | 73.2 s | 73.2 |
| 0.015 | 3.3% | 283.14M | 3.66M | [1.57M, 5.64M] | 0.93650 | yes | 51.8 s | 51.8 |
| 0.01 | 2.9% | 260.73M | -18.75M | [-21.50M, -16.17M] | 0.93033 | no | 82.1 s | 82.1 |

r that keep profit and AUC: 0.3, 0.1, 0.025, 0.015. r_flat (time flattens at): 0.6. Useful zone: [0.015, 0.3].

Whole exploration of this experiment: 11.67 h (searches 3.80 h, 400 final trainings 7.87 h).

## Verdict

- r_passed_validation: **True**
- about_the_same_auc_and_profit: **True**
- clearly_less_time: **True**
- time_flattens: **True**
- useful_zone: **True**
- hypothesis_confirmed: **True**
