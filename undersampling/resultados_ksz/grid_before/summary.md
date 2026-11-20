# Results: full_ksz / grid_before

Mode **grid**, r applied **before**. r values: 0.8, 0.6, 0.4, 0.3, 0.2, 0.15, 0.1, 0.07, 0.05, 0.04, 0.03, 0.025, 0.02, 0.015, 0.01.

Machine: Windows-11-10.0.26200-SP0, 12 cores, 15.2 GB RAM. Peak memory: 6.07 GB.

## 1. Choosing r (validation month, 10 seeds)

every r judged on its own (95.0% interval): least time among the r not worse on validation profit; ties within 5% of baseline time -> largest r.

| r | trials with r | Share contacted | Profit | Diff vs r=1 | 95% interval | Passes | Time used to choose | Winner |
|---|---|---|---|---|---|---|---|---|
| 1 | 50 | 3.0% | 205.67M | 0.00M | [0.00M, 0.00M] | — | 2.96 h |  |
| 0.8 | 50 | 3.2% | 207.54M | 1.87M | [-1.32M, 5.50M] | yes | 2.91 h |  |
| 0.6 | 50 | 3.1% | 207.21M | 1.54M | [-0.66M, 4.07M] | yes | 1.69 h |  |
| 0.4 | 50 | 3.3% | 208.86M | 3.19M | [0.33M, 5.94M] | yes | 1.61 h |  |
| 0.3 | 50 | 3.1% | 204.46M | -1.21M | [-3.63M, 1.21M] | no | 2,026.6 s |  |
| 0.2 | 50 | 3.0% | 209.52M | 3.85M | [1.54M, 6.27M] | yes | 2,260.0 s |  |
| 0.15 | 50 | 3.5% | 209.44M | 3.77M | [0.14M, 6.85M] | yes | 2,528.2 s |  |
| 0.1 | 50 | 3.4% | 208.23M | 2.56M | [-0.08M, 5.09M] | yes | 1,159.0 s | **yes** |
| 0.07 | 50 | 3.2% | 206.11M | 0.44M | [-3.19M, 3.74M] | no | 1,391.5 s |  |
| 0.05 | 50 | 2.9% | 206.99M | 1.32M | [-2.64M, 5.28M] | no | 1,407.3 s |  |
| 0.04 | 50 | 2.9% | 207.76M | 2.09M | [-1.10M, 4.95M] | yes | 1,474.0 s |  |
| 0.03 | 50 | 3.3% | 204.13M | -1.54M | [-4.62M, 1.32M] | no | 1,370.4 s |  |
| 0.025 | 50 | 3.3% | 202.15M | -3.52M | [-6.82M, -0.11M] | no | 1,898.5 s |  |
| 0.02 | 50 | 2.9% | 198.85M | -6.82M | [-9.13M, -4.73M] | no | 768.7 s |  |
| 0.015 | 50 | 2.7% | 202.12M | -3.55M | [-6.41M, -0.47M] | no | 790.0 s |  |
| 0.01 | 50 | 3.3% | 195.55M | -10.12M | [-13.86M, -6.05M] | no | 870.7 s |  |

Winner: **r = 0.1**.

## 2. Winner (r = 0.1) vs baseline on the test month

Top 3 configurations × 20 seeds each side, paired by rank and seed. Margin δ = 1% of the baseline mean. Decisive: profit (at each model's share, fixed on validation) and AUC.

| Metric | Baseline | Winner | Mean diff | 95% interval | Wilcoxon p | Not worse than −δ |
|---|---|---|---|---|---|---|
| profit | 280.97M | 281.31M | 0.34M | [-0.96M, 1.68M] | 0.724 | yes |
| auc | 0.93665 | 0.93651 | -0.00014 | [-0.00032, 0.00006] | 0.193 | yes |
| profit_peak | 287.42M | 285.98M | -1.44M | [-2.51M, -0.39M] | 0.0164 | yes |

| Time | Baseline | Winner |
|---|---|---|
| prep_seconds | 192.8 s | 192.8 s |
| search_seconds | 2.90 h | 1,131.8 s |
| final_train_seconds | 211.8 s | 27.2 s |
| predict_seconds | 2.5 s | 1.7 s |
| total_seconds | 3.01 h | 1,353.5 s |
| seconds_per_tree (search) | 0.4307 | 0.0318 |

Total time reduction: **87.5%** (required 30%); search only 89.2%; final training 87.1%.

## 3. r curve (test month, best configuration of each r)

| r | Share | Profit | Diff vs r=1 | 95% interval | AUC | Not worse | Time | Train s |
|---|---|---|---|---|---|---|---|---|
| 1 | 3.0% | 279.48M | 0.00M | [0.00M, 0.00M] | 0.93688 | — | 2.96 h | 211.8 |
| 0.8 | 3.2% | 281.38M | 1.90M | [-0.96M, 4.76M] | 0.93607 | yes | 2.91 h | 174.7 |
| 0.6 | 3.1% | 280.97M | 1.49M | [-1.10M, 3.96M] | 0.93670 | yes | 1.69 h | 52.2 |
| 0.4 | 3.3% | 280.88M | 1.40M | [-1.02M, 3.93M] | 0.93540 | yes | 1.61 h | 103.4 |
| 0.3 | 3.1% | 279.32M | -0.17M | [-3.25M, 2.69M] | 0.93667 | no | 2,026.5 s | 62.5 |
| 0.2 | 3.0% | 280.86M | 1.38M | [-1.04M, 3.69M] | 0.93567 | yes | 2,259.8 s | 64.8 |
| 0.15 | 3.5% | 280.42M | 0.94M | [-1.98M, 4.01M] | 0.93597 | yes | 2,528.3 s | 38.1 |
| 0.1 | 3.4% | 282.10M | 2.61M | [-0.19M, 5.25M] | 0.93622 | yes | 1,159.0 s | 27.2 |
| 0.07 | 3.2% | 280.00M | 0.52M | [-2.01M, 2.83M] | 0.93597 | yes | 1,391.6 s | 18.9 |
| 0.05 | 2.9% | 279.76M | 0.28M | [-2.37M, 2.64M] | 0.93609 | yes | 1,407.2 s | 31.1 |
| 0.04 | 2.9% | 279.98M | 0.49M | [-2.37M, 3.13M] | 0.93594 | yes | 1,474.0 s | 32.5 |
| 0.03 | 3.3% | 278.90M | -0.58M | [-3.49M, 2.28M] | 0.93590 | no | 1,370.6 s | 39.7 |
| 0.025 | 3.3% | 280.50M | 1.02M | [-1.73M, 3.82M] | 0.93552 | yes | 1,898.5 s | 41.3 |
| 0.02 | 2.9% | 272.77M | -6.71M | [-9.57M, -4.07M] | 0.93552 | no | 768.6 s | 13.0 |
| 0.015 | 2.7% | 274.39M | -5.09M | [-7.62M, -2.39M] | 0.93506 | no | 790.0 s | 4.8 |
| 0.01 | 3.3% | 271.48M | -8.00M | [-11.58M, -4.59M] | 0.93428 | no | 870.7 s | 8.6 |

r that keep profit and AUC: 0.8, 0.6, 0.4, 0.2, 0.15, 0.1, 0.07, 0.05, 0.04, 0.025. r_flat (time flattens at): 0.1. Useful zone: [0.025, 0.1].

Whole exploration of this experiment: 21.73 h (searches 13.90 h, 400 final trainings 7.83 h).

## Verdict

- r_passed_validation: **True**
- about_the_same_auc_and_profit: **True**
- clearly_less_time: **True**
- time_flattens: **True**
- useful_zone: **True**
- hypothesis_confirmed: **True**
