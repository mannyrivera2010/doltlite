# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-09 18:05 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261004.327.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/37959361677)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 17s | 8.99s | 11.48s | 1.277× | 1.76% | **PASS** |
| textpk | 69 | 55 | 1h 33m 39s | 10.43s | 11.90s | 1.141× | 1.56% | **PASS** |
| blobpk | 69 | 55 | 1h 5m 36s | 6.97s | 8.10s | 1.162× | 3.08% | **PASS** |
| compositepk | 69 | 55 | 1h 2m 38s | 7.06s | 8.30s | 1.176× | 1.81% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.87ms | 30.07ms | 1.209× | 2.16% | PASS |
| mem_reads | `oltp_range_select` | 10.90ms | 13.41ms | 1.229× | 3.09% | PASS |
| mem_reads | `oltp_sum_range` | 10.00ms | 12.57ms | 1.258× | 2.32% | PASS |
| mem_reads | `oltp_order_range` | 2.65ms | 3.06ms | 1.158× | 1.48% | PASS |
| mem_reads | `oltp_distinct_range` | 3.72ms | 4.10ms | 1.102× | 1.06% | PASS |
| mem_reads | `oltp_index_scan` | 4.04ms | 5.57ms | 1.378× | 1.68% | PASS |
| mem_reads | `select_random_points` | 10.72ms | 11.47ms | 1.070× | 2.63% | PASS |
| mem_reads | `select_random_ranges` | 3.08ms | 4.05ms | 1.314× | 2.07% | PASS |
| mem_reads | `covering_index_scan` | 4.24ms | 4.29ms | 1.010× | 1.87% | PASS |
| mem_reads | `groupby_scan` | 30.02ms | 33.42ms | 1.113× | 0.78% | PASS |
| mem_reads | `index_join` | 6.13ms | 8.40ms | 1.370× | 2.25% | PASS |
| mem_reads | `index_join_scan` | 3.56ms | 4.71ms | 1.322× | 2.43% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.35s | 1.268× | 1.46% | PASS |
| mem_reads | `table_scan` | 1.22s | 1.41s | 1.157× | 2.17% | PASS |
| mem_reads | `oltp_read_only` | 107.04ms | 125.31ms | 1.171× | 1.59% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.01ms | 251.59ms | 1.390× | 1.02% | PASS |
| mem_writes | `oltp_insert` | 15.52ms | 28.34ms | 1.826× | 0.97% | PASS |
| mem_writes | `oltp_update_index` | 50.47ms | 105.14ms | 2.083× | 0.79% | PASS |
| mem_writes | `oltp_update_non_index` | 34.47ms | 59.68ms | 1.731× | 1.82% | PASS |
| mem_writes | `oltp_delete_insert` | 45.68ms | 79.50ms | 1.740× | 1.43% | PASS |
| mem_writes | `oltp_write_only` | 22.36ms | 45.30ms | 2.026× | 1.16% | PASS |
| mem_writes | `types_delete_insert` | 24.71ms | 40.54ms | 1.640× | 1.49% | PASS |
| mem_writes | `oltp_read_write` | 69.28ms | 112.39ms | 1.622× | 2.07% | PASS |
| file_reads | `oltp_point_select` | 99.67ms | 55.52ms | 0.557× | 0.97% | PASS |
| file_reads | `oltp_range_select` | 18.52ms | 15.97ms | 0.862× | 2.23% | PASS |
| file_reads | `oltp_sum_range` | 17.50ms | 15.25ms | 0.871× | 1.72% | PASS |
| file_reads | `oltp_order_range` | 3.50ms | 3.39ms | 0.968× | 1.90% | PASS |
| file_reads | `oltp_distinct_range` | 4.54ms | 4.44ms | 0.978× | 1.48% | PASS |
| file_reads | `oltp_index_scan` | 11.69ms | 8.32ms | 0.712× | 1.23% | PASS |
| file_reads | `select_random_points` | 18.75ms | 14.04ms | 0.749× | 2.98% | PASS |
| file_reads | `select_random_ranges` | 10.56ms | 6.64ms | 0.629× | 0.71% | PASS |
| file_reads | `covering_index_scan` | 11.98ms | 7.11ms | 0.593× | 1.14% | PASS |
| file_reads | `groupby_scan` | 30.76ms | 33.88ms | 1.102× | 1.07% | PASS |
| file_reads | `index_join` | 10.40ms | 10.28ms | 0.988× | 1.59% | PASS |
| file_reads | `index_join_scan` | 4.61ms | 5.08ms | 1.103× | 2.63% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.33s | 1.280× | 0.47% | PASS |
| file_reads | `table_scan` | 1.19s | 1.40s | 1.182× | 1.47% | PASS |
| file_reads | `oltp_read_only` | 213.96ms | 161.99ms | 0.757× | 1.09% | PASS |
| file_writes | `oltp_bulk_insert` | 194.03ms | 270.99ms | 1.397× | 0.98% | PASS |
| file_writes | `oltp_insert` | 22.05ms | 35.60ms | 1.614× | 1.23% | PASS |
| file_writes | `oltp_update_index` | 79.17ms | 129.62ms | 1.637× | 1.79% | PASS |
| file_writes | `oltp_update_non_index` | 60.52ms | 82.73ms | 1.367× | 1.50% | PASS |
| file_writes | `oltp_delete_insert` | 69.77ms | 100.53ms | 1.441× | 1.62% | PASS |
| file_writes | `oltp_write_only` | 45.67ms | 64.97ms | 1.423× | 1.43% | PASS |
| file_writes | `types_delete_insert` | 40.95ms | 53.81ms | 1.314× | 2.03% | PASS |
| file_writes | `oltp_read_write` | 94.27ms | 131.52ms | 1.395× | 1.90% | PASS |
| ac_reads | `oltp_point_select` | 49.39ms | 55.36ms | 1.121× | 1.35% | PASS |
| ac_reads | `oltp_range_select` | 14.07ms | 16.03ms | 1.140× | 2.25% | PASS |
| ac_reads | `oltp_sum_range` | 12.97ms | 15.41ms | 1.188× | 1.95% | PASS |
| ac_reads | `oltp_order_range` | 3.03ms | 3.38ms | 1.114× | 2.13% | PASS |
| ac_reads | `oltp_distinct_range` | 4.17ms | 4.52ms | 1.084× | 1.87% | PASS |
| ac_reads | `oltp_index_scan` | 6.82ms | 8.35ms | 1.223× | 2.35% | PASS |
| ac_reads | `select_random_points` | 13.59ms | 14.04ms | 1.033× | 2.69% | PASS |
| ac_reads | `select_random_ranges` | 5.63ms | 6.64ms | 1.178× | 0.99% | PASS |
| ac_reads | `covering_index_scan` | 7.20ms | 7.15ms | 0.993× | 1.76% | PASS |
| ac_reads | `groupby_scan` | 30.46ms | 33.96ms | 1.115× | 0.76% | PASS |
| ac_reads | `index_join` | 8.00ms | 10.35ms | 1.294× | 1.84% | PASS |
| ac_reads | `index_join_scan` | 4.24ms | 5.18ms | 1.224× | 3.60% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.33s | 1.270× | 1.12% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.41s | 1.171× | 1.26% | PASS |
| ac_reads | `oltp_read_only` | 144.63ms | 163.73ms | 1.132× | 1.43% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.58ms | 72.80ms | 3.087× | 6.67% | PASS |
| ac_writes | `oltp_insert_ac` | 26.38ms | 92.58ms | 3.510× | 7.28% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.77ms | 105.66ms | 3.672× | 7.89% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.70ms | 81.39ms | 3.435× | 6.88% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.37ms | 94.89ms | 3.741× | 6.42% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.50ms | 91.53ms | 3.589× | 4.53% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.43ms | 81.99ms | 3.655× | 5.81% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.61ms | 99.71ms | 3.154× | 5.41% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.66ms | 38.18ms | 1.245× | 1.69% | PASS |
| mem_reads | `oltp_range_select` | 13.67ms | 14.29ms | 1.045× | 2.01% | PASS |
| mem_reads | `oltp_sum_range` | 12.56ms | 14.36ms | 1.144× | 1.75% | PASS |
| mem_reads | `oltp_order_range` | 3.00ms | 3.16ms | 1.055× | 1.52% | PASS |
| mem_reads | `oltp_distinct_range` | 4.04ms | 4.25ms | 1.052× | 0.89% | PASS |
| mem_reads | `oltp_index_scan` | 4.58ms | 6.41ms | 1.397× | 2.37% | PASS |
| mem_reads | `select_random_points` | 18.09ms | 20.99ms | 1.160× | 2.25% | PASS |
| mem_reads | `select_random_ranges` | 4.01ms | 5.19ms | 1.294× | 1.56% | PASS |
| mem_reads | `covering_index_scan` | 4.66ms | 4.63ms | 0.992× | 3.05% | PASS |
| mem_reads | `groupby_scan` | 32.21ms | 34.52ms | 1.072× | 1.09% | PASS |
| mem_reads | `index_join` | 6.96ms | 9.45ms | 1.358× | 2.49% | PASS |
| mem_reads | `index_join_scan` | 4.51ms | 5.37ms | 1.192× | 1.66% | PASS |
| mem_reads | `types_table_scan` | 1.16s | 1.26s | 1.090× | 4.37% | PASS |
| mem_reads | `table_scan` | 1.38s | 1.40s | 1.008× | 3.17% | PASS |
| mem_reads | `oltp_read_only` | 121.94ms | 138.30ms | 1.134× | 1.56% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.84ms | 359.14ms | 1.523× | 0.98% | PASS |
| mem_writes | `oltp_insert` | 22.24ms | 40.04ms | 1.801× | 1.47% | PASS |
| mem_writes | `oltp_update_index` | 73.93ms | 137.76ms | 1.863× | 1.44% | PASS |
| mem_writes | `oltp_update_non_index` | 50.30ms | 89.37ms | 1.777× | 1.21% | PASS |
| mem_writes | `oltp_delete_insert` | 55.02ms | 108.22ms | 1.967× | 1.10% | PASS |
| mem_writes | `oltp_write_only` | 30.70ms | 63.95ms | 2.083× | 1.04% | PASS |
| mem_writes | `types_delete_insert` | 33.75ms | 56.84ms | 1.684× | 0.87% | PASS |
| mem_writes | `oltp_read_write` | 93.83ms | 146.22ms | 1.558× | 1.09% | PASS |
| file_reads | `oltp_point_select` | 107.09ms | 64.28ms | 0.600× | 1.16% | PASS |
| file_reads | `oltp_range_select` | 21.65ms | 16.93ms | 0.782× | 1.61% | PASS |
| file_reads | `oltp_sum_range` | 20.60ms | 17.11ms | 0.831× | 1.23% | PASS |
| file_reads | `oltp_order_range` | 3.89ms | 3.48ms | 0.894× | 1.04% | PASS |
| file_reads | `oltp_distinct_range` | 4.96ms | 4.58ms | 0.924× | 1.35% | PASS |
| file_reads | `oltp_index_scan` | 12.67ms | 9.18ms | 0.725× | 1.35% | PASS |
| file_reads | `select_random_points` | 27.64ms | 24.93ms | 0.902× | 1.47% | PASS |
| file_reads | `select_random_ranges` | 11.80ms | 7.88ms | 0.668× | 0.71% | PASS |
| file_reads | `covering_index_scan` | 13.39ms | 7.46ms | 0.557× | 1.09% | PASS |
| file_reads | `groupby_scan` | 33.43ms | 35.02ms | 1.047× | 1.06% | PASS |
| file_reads | `index_join` | 12.15ms | 11.40ms | 0.938× | 2.10% | PASS |
| file_reads | `index_join_scan` | 5.70ms | 5.98ms | 1.051× | 1.54% | PASS |
| file_reads | `types_table_scan` | 1.15s | 1.25s | 1.087× | 3.50% | PASS |
| file_reads | `table_scan` | 1.41s | 1.40s | 0.990× | 3.12% | PASS |
| file_reads | `oltp_read_only` | 234.82ms | 178.31ms | 0.759× | 0.97% | PASS |
| file_writes | `oltp_bulk_insert` | 256.03ms | 387.16ms | 1.512× | 0.94% | PASS |
| file_writes | `oltp_insert` | 50.39ms | 52.61ms | 1.044× | 24.76% | PASS |
| file_writes | `oltp_update_index` | 119.44ms | 175.02ms | 1.465× | 2.12% | PASS |
| file_writes | `oltp_update_non_index` | 96.40ms | 113.83ms | 1.181× | 7.30% | PASS |
| file_writes | `oltp_delete_insert` | 92.26ms | 135.53ms | 1.469× | 1.63% | PASS |
| file_writes | `oltp_write_only` | 89.28ms | 86.18ms | 0.965× | 12.44% | PASS |
| file_writes | `types_delete_insert` | 57.00ms | 76.34ms | 1.339× | 1.78% | PASS |
| file_writes | `oltp_read_write` | 139.91ms | 168.30ms | 1.203× | 5.68% | PASS |
| ac_reads | `oltp_point_select` | 56.19ms | 64.24ms | 1.143× | 1.28% | PASS |
| ac_reads | `oltp_range_select` | 16.52ms | 16.99ms | 1.029× | 1.47% | PASS |
| ac_reads | `oltp_sum_range` | 15.46ms | 17.23ms | 1.114× | 1.10% | PASS |
| ac_reads | `oltp_order_range` | 3.38ms | 3.49ms | 1.031× | 1.45% | PASS |
| ac_reads | `oltp_distinct_range` | 4.41ms | 4.57ms | 1.037× | 0.83% | PASS |
| ac_reads | `oltp_index_scan` | 7.60ms | 9.16ms | 1.206× | 1.03% | PASS |
| ac_reads | `select_random_points` | 21.56ms | 24.72ms | 1.147× | 1.44% | PASS |
| ac_reads | `select_random_ranges` | 6.76ms | 7.87ms | 1.164× | 0.77% | PASS |
| ac_reads | `covering_index_scan` | 8.16ms | 7.43ms | 0.911× | 1.89% | PASS |
| ac_reads | `groupby_scan` | 33.00ms | 35.15ms | 1.065× | 0.79% | PASS |
| ac_reads | `index_join` | 9.60ms | 11.53ms | 1.201× | 1.57% | PASS |
| ac_reads | `index_join_scan` | 5.18ms | 6.04ms | 1.166× | 1.56% | PASS |
| ac_reads | `types_table_scan` | 1.16s | 1.25s | 1.080× | 1.55% | PASS |
| ac_reads | `table_scan` | 1.35s | 1.39s | 1.029× | 6.43% | PASS |
| ac_reads | `oltp_read_only` | 157.31ms | 176.42ms | 1.121× | 1.20% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.85ms | 66.80ms | 2.801× | 5.65% | PASS |
| ac_writes | `oltp_insert_ac` | 25.45ms | 82.14ms | 3.227× | 4.81% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.89ms | 99.93ms | 3.584× | 7.58% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.96ms | 79.52ms | 3.319× | 5.80% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.38ms | 89.95ms | 3.409× | 6.92% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.89ms | 90.49ms | 3.495× | 4.35% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.52ms | 80.54ms | 3.425× | 6.14% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.39ms | 97.39ms | 3.102× | 3.62% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 17.30ms | 20.16ms | 1.165× | 2.87% | PASS |
| mem_reads | `oltp_range_select` | 8.26ms | 8.76ms | 1.061× | 7.53% | PASS |
| mem_reads | `oltp_sum_range` | 8.20ms | 8.44ms | 1.029× | 8.88% | PASS |
| mem_reads | `oltp_order_range` | 1.97ms | 2.03ms | 1.033× | 4.14% | PASS |
| mem_reads | `oltp_distinct_range` | 2.63ms | 2.66ms | 1.010× | 2.11% | PASS |
| mem_reads | `oltp_index_scan` | 2.84ms | 3.49ms | 1.226× | 3.78% | PASS |
| mem_reads | `select_random_points` | 11.28ms | 13.60ms | 1.206× | 4.57% | PASS |
| mem_reads | `select_random_ranges` | 2.31ms | 3.10ms | 1.338× | 4.46% | PASS |
| mem_reads | `covering_index_scan` | 2.65ms | 2.52ms | 0.952× | 1.46% | PASS |
| mem_reads | `groupby_scan` | 19.75ms | 21.69ms | 1.098× | 0.84% | PASS |
| mem_reads | `index_join` | 4.97ms | 6.28ms | 1.262× | 1.86% | PASS |
| mem_reads | `index_join_scan` | 2.54ms | 3.88ms | 1.525× | 5.59% | PASS |
| mem_reads | `types_table_scan` | 749.56ms | 823.25ms | 1.098× | 0.90% | PASS |
| mem_reads | `table_scan` | 887.22ms | 909.42ms | 1.025× | 2.75% | PASS |
| mem_reads | `oltp_read_only` | 74.78ms | 81.46ms | 1.089× | 0.49% | PASS |
| mem_writes | `oltp_bulk_insert` | 132.73ms | 188.56ms | 1.421× | 0.36% | PASS |
| mem_writes | `oltp_insert` | 12.46ms | 23.00ms | 1.846× | 0.95% | PASS |
| mem_writes | `oltp_update_index` | 44.27ms | 81.32ms | 1.837× | 1.84% | PASS |
| mem_writes | `oltp_update_non_index` | 29.95ms | 50.35ms | 1.681× | 2.87% | PASS |
| mem_writes | `oltp_delete_insert` | 30.73ms | 62.02ms | 2.018× | 3.25% | PASS |
| mem_writes | `oltp_write_only` | 17.47ms | 36.77ms | 2.105× | 4.30% | PASS |
| mem_writes | `types_delete_insert` | 20.25ms | 32.86ms | 1.622× | 1.88% | PASS |
| mem_writes | `oltp_read_write` | 52.91ms | 84.41ms | 1.595× | 1.54% | PASS |
| file_reads | `oltp_point_select` | 40.59ms | 28.91ms | 0.712× | 1.47% | PASS |
| file_reads | `oltp_range_select` | 12.17ms | 10.25ms | 0.842× | 3.46% | PASS |
| file_reads | `oltp_sum_range` | 10.65ms | 9.55ms | 0.897× | 5.14% | PASS |
| file_reads | `oltp_order_range` | 2.15ms | 2.11ms | 0.981× | 2.66% | PASS |
| file_reads | `oltp_distinct_range` | 2.76ms | 2.73ms | 0.989× | 2.52% | PASS |
| file_reads | `oltp_index_scan` | 5.04ms | 4.30ms | 0.854× | 1.95% | PASS |
| file_reads | `select_random_points` | 14.26ms | 15.06ms | 1.057× | 3.01% | PASS |
| file_reads | `select_random_ranges` | 4.71ms | 3.98ms | 0.843× | 2.83% | PASS |
| file_reads | `covering_index_scan` | 4.88ms | 3.27ms | 0.669× | 1.11% | PASS |
| file_reads | `groupby_scan` | 19.92ms | 21.71ms | 1.090× | 0.82% | PASS |
| file_reads | `index_join` | 5.83ms | 6.57ms | 1.126× | 1.36% | PASS |
| file_reads | `index_join_scan` | 2.75ms | 4.04ms | 1.470× | 4.04% | PASS |
| file_reads | `types_table_scan` | 749.89ms | 824.31ms | 1.099× | 0.86% | PASS |
| file_reads | `table_scan` | 879.35ms | 909.91ms | 1.035× | 3.02% | PASS |
| file_reads | `oltp_read_only` | 104.55ms | 92.19ms | 0.882× | 1.51% | PASS |
| file_writes | `oltp_bulk_insert` | 181.74ms | 249.43ms | 1.372× | 4.44% | PASS |
| file_writes | `oltp_insert` | 30.24ms | 48.41ms | 1.601× | 7.07% | PASS |
| file_writes | `oltp_update_index` | 150.72ms | 154.28ms | 1.024× | 7.90% | PASS |
| file_writes | `oltp_update_non_index` | 110.83ms | 109.06ms | 0.984× | 2.13% | PASS |
| file_writes | `oltp_delete_insert` | 125.85ms | 124.69ms | 0.991× | 6.76% | PASS |
| file_writes | `oltp_write_only` | 90.63ms | 84.54ms | 0.933× | 5.94% | PASS |
| file_writes | `types_delete_insert` | 78.56ms | 69.97ms | 0.891× | 11.01% | PASS |
| file_writes | `oltp_read_write` | 121.13ms | 129.25ms | 1.067× | 3.08% | PASS |
| ac_reads | `oltp_point_select` | 24.98ms | 28.41ms | 1.137× | 1.48% | PASS |
| ac_reads | `oltp_range_select` | 9.82ms | 10.03ms | 1.021× | 6.01% | PASS |
| ac_reads | `oltp_sum_range` | 8.73ms | 9.35ms | 1.072× | 4.88% | PASS |
| ac_reads | `oltp_order_range` | 2.06ms | 2.14ms | 1.042× | 3.89% | PASS |
| ac_reads | `oltp_distinct_range` | 2.71ms | 2.81ms | 1.035× | 2.93% | PASS |
| ac_reads | `oltp_index_scan` | 3.70ms | 4.50ms | 1.214× | 3.19% | PASS |
| ac_reads | `select_random_points` | 14.10ms | 15.54ms | 1.102× | 4.53% | PASS |
| ac_reads | `select_random_ranges` | 3.48ms | 4.07ms | 1.172× | 3.29% | PASS |
| ac_reads | `covering_index_scan` | 3.51ms | 3.60ms | 1.025× | 4.09% | PASS |
| ac_reads | `groupby_scan` | 20.28ms | 22.09ms | 1.089× | 0.95% | PASS |
| ac_reads | `index_join` | 5.67ms | 7.24ms | 1.277× | 3.08% | PASS |
| ac_reads | `index_join_scan` | 2.90ms | 4.33ms | 1.489× | 5.91% | PASS |
| ac_reads | `types_table_scan` | 751.70ms | 823.56ms | 1.096× | 0.58% | PASS |
| ac_reads | `table_scan` | 878.14ms | 909.68ms | 1.036× | 2.52% | PASS |
| ac_reads | `oltp_read_only` | 86.78ms | 93.72ms | 1.080× | 0.99% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 32.14ms | 84.07ms | 2.616× | 4.32% | PASS |
| ac_writes | `oltp_insert_ac` | 33.40ms | 98.12ms | 2.938× | 5.13% | PASS |
| ac_writes | `oltp_update_index_ac` | 35.22ms | 104.02ms | 2.953× | 3.66% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 31.95ms | 91.94ms | 2.877× | 4.92% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 33.58ms | 100.42ms | 2.991× | 4.69% | PASS |
| ac_writes | `oltp_write_only_ac` | 33.88ms | 100.62ms | 2.970× | 5.15% | PASS |
| ac_writes | `types_delete_insert_ac` | 32.31ms | 96.17ms | 2.976× | 9.86% | PASS |
| ac_writes | `oltp_read_write_ac` | 36.40ms | 105.21ms | 2.891× | 5.89% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.31ms | 23.72ms | 1.168× | 1.92% | PASS |
| mem_reads | `oltp_range_select` | 13.75ms | 13.41ms | 0.976× | 1.16% | PASS |
| mem_reads | `oltp_sum_range` | 13.25ms | 12.60ms | 0.951× | 1.99% | PASS |
| mem_reads | `oltp_order_range` | 2.53ms | 2.53ms | 0.998× | 1.83% | PASS |
| mem_reads | `oltp_distinct_range` | 3.18ms | 3.15ms | 0.991× | 1.29% | PASS |
| mem_reads | `oltp_index_scan` | 3.19ms | 3.96ms | 1.243× | 2.36% | PASS |
| mem_reads | `select_random_points` | 20.06ms | 22.27ms | 1.110× | 1.38% | PASS |
| mem_reads | `select_random_ranges` | 5.04ms | 5.71ms | 1.133× | 2.13% | PASS |
| mem_reads | `covering_index_scan` | 2.48ms | 2.60ms | 1.050× | 1.98% | PASS |
| mem_reads | `groupby_scan` | 23.02ms | 24.79ms | 1.077× | 0.69% | PASS |
| mem_reads | `index_join` | 5.66ms | 7.24ms | 1.279× | 1.64% | PASS |
| mem_reads | `index_join_scan` | 2.96ms | 4.48ms | 1.515× | 2.28% | PASS |
| mem_reads | `types_table_scan` | 758.87ms | 842.38ms | 1.110× | 1.62% | PASS |
| mem_reads | `table_scan` | 868.62ms | 915.86ms | 1.054× | 2.36% | PASS |
| mem_reads | `oltp_read_only` | 97.15ms | 102.51ms | 1.055× | 1.24% | PASS |
| mem_writes | `oltp_bulk_insert` | 138.69ms | 198.16ms | 1.429× | 0.84% | PASS |
| mem_writes | `oltp_insert` | 11.61ms | 21.55ms | 1.856× | 0.76% | PASS |
| mem_writes | `oltp_update_index` | 45.55ms | 79.07ms | 1.736× | 1.81% | PASS |
| mem_writes | `oltp_update_non_index` | 32.65ms | 53.83ms | 1.649× | 1.46% | PASS |
| mem_writes | `oltp_delete_insert` | 33.67ms | 62.52ms | 1.857× | 1.78% | PASS |
| mem_writes | `oltp_write_only` | 18.57ms | 37.56ms | 2.023× | 1.48% | PASS |
| mem_writes | `types_delete_insert` | 21.11ms | 35.23ms | 1.669× | 1.68% | PASS |
| mem_writes | `oltp_read_write` | 66.42ms | 96.86ms | 1.458× | 2.06% | PASS |
| file_reads | `oltp_point_select` | 43.98ms | 32.43ms | 0.737× | 0.88% | PASS |
| file_reads | `oltp_range_select` | 16.83ms | 14.69ms | 0.873× | 1.34% | PASS |
| file_reads | `oltp_sum_range` | 16.46ms | 13.92ms | 0.846× | 0.96% | PASS |
| file_reads | `oltp_order_range` | 2.83ms | 2.72ms | 0.961× | 1.95% | PASS |
| file_reads | `oltp_distinct_range` | 3.53ms | 3.32ms | 0.941× | 1.13% | PASS |
| file_reads | `oltp_index_scan` | 5.63ms | 5.29ms | 0.938× | 2.18% | PASS |
| file_reads | `select_random_points` | 22.67ms | 23.58ms | 1.040× | 1.09% | PASS |
| file_reads | `select_random_ranges` | 7.44ms | 6.66ms | 0.895× | 1.69% | PASS |
| file_reads | `covering_index_scan` | 4.95ms | 3.86ms | 0.780× | 1.49% | PASS |
| file_reads | `groupby_scan` | 23.46ms | 25.14ms | 1.072× | 0.45% | PASS |
| file_reads | `index_join` | 7.16ms | 8.53ms | 1.192× | 1.72% | PASS |
| file_reads | `index_join_scan` | 3.22ms | 4.63ms | 1.437× | 2.75% | PASS |
| file_reads | `types_table_scan` | 755.42ms | 840.28ms | 1.112× | 2.48% | PASS |
| file_reads | `table_scan` | 863.58ms | 911.36ms | 1.055× | 1.91% | PASS |
| file_reads | `oltp_read_only` | 123.42ms | 112.45ms | 0.911× | 1.07% | PASS |
| file_writes | `oltp_bulk_insert` | 189.36ms | 250.75ms | 1.324× | 2.80% | PASS |
| file_writes | `oltp_insert` | 19.85ms | 38.22ms | 1.925× | 15.94% | PASS |
| file_writes | `oltp_update_index` | 139.82ms | 139.84ms | 1.000× | 4.00% | PASS |
| file_writes | `oltp_update_non_index` | 110.60ms | 99.88ms | 0.903× | 3.06% | PASS |
| file_writes | `oltp_delete_insert` | 106.83ms | 113.14ms | 1.059× | 3.57% | PASS |
| file_writes | `oltp_write_only` | 81.43ms | 85.32ms | 1.048× | 5.82% | PASS |
| file_writes | `types_delete_insert` | 70.38ms | 60.51ms | 0.860× | 10.00% | PASS |
| file_writes | `oltp_read_write` | 124.28ms | 140.32ms | 1.129× | 3.65% | PASS |
| ac_reads | `oltp_point_select` | 27.89ms | 32.04ms | 1.149× | 1.17% | PASS |
| ac_reads | `oltp_range_select` | 15.21ms | 14.57ms | 0.958× | 1.60% | PASS |
| ac_reads | `oltp_sum_range` | 14.56ms | 13.81ms | 0.948× | 1.34% | PASS |
| ac_reads | `oltp_order_range` | 2.72ms | 2.70ms | 0.991× | 2.10% | PASS |
| ac_reads | `oltp_distinct_range` | 3.37ms | 3.32ms | 0.986× | 1.55% | PASS |
| ac_reads | `oltp_index_scan` | 4.22ms | 5.25ms | 1.245× | 1.50% | PASS |
| ac_reads | `select_random_points` | 20.95ms | 23.36ms | 1.115× | 0.97% | PASS |
| ac_reads | `select_random_ranges` | 5.87ms | 6.59ms | 1.124× | 1.12% | PASS |
| ac_reads | `covering_index_scan` | 3.44ms | 3.77ms | 1.096× | 2.89% | PASS |
| ac_reads | `groupby_scan` | 23.29ms | 25.09ms | 1.078× | 1.19% | PASS |
| ac_reads | `index_join` | 6.32ms | 8.15ms | 1.290× | 3.42% | PASS |
| ac_reads | `index_join_scan` | 3.15ms | 4.61ms | 1.463× | 5.26% | PASS |
| ac_reads | `types_table_scan` | 752.11ms | 838.51ms | 1.115× | 1.62% | PASS |
| ac_reads | `table_scan` | 859.40ms | 911.44ms | 1.061× | 1.49% | PASS |
| ac_reads | `oltp_read_only` | 99.28ms | 110.39ms | 1.112× | 0.88% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 31.48ms | 86.09ms | 2.735× | 4.77% | PASS |
| ac_writes | `oltp_insert_ac` | 33.06ms | 95.24ms | 2.881× | 4.69% | PASS |
| ac_writes | `oltp_update_index_ac` | 34.33ms | 106.12ms | 3.091× | 4.81% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 31.86ms | 95.40ms | 2.994× | 3.87% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 33.47ms | 103.15ms | 3.082× | 2.17% | PASS |
| ac_writes | `oltp_write_only_ac` | 33.28ms | 101.15ms | 3.039× | 5.02% | PASS |
| ac_writes | `types_delete_insert_ac` | 31.63ms | 94.70ms | 2.994× | 8.55% | PASS |
| ac_writes | `oltp_read_write_ac` | 37.82ms | 108.13ms | 2.859× | 5.76% | PASS |

</details>

## Version-control latency

Wall time: 1m 40s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 53.73ms | 200.00ms | 26.9% | 1.10% | PASS |
| `status_dirty_many_tables` | 56.29ms | 200.00ms | 28.1% | 2.00% | PASS |
| `diff_regular_working_one_table` | 50.83ms | 150.00ms | 33.9% | 2.23% | PASS |
| `diff_regular_working_many_tables` | 58.15ms | 200.00ms | 29.1% | 1.88% | PASS |
| `diff_stat_working_many_tables` | 59.75ms | 200.00ms | 29.9% | 1.53% | PASS |
| `diff_schema_working_many_tables` | 60.30ms | 200.00ms | 30.2% | 1.21% | PASS |
| `branch_list_many_branches` | 16.26ms | 100.00ms | 16.3% | 2.36% | PASS |
| `branch_create_delete` | 17.78ms | 100.00ms | 17.8% | 3.35% | PASS |
| `checkout_branch_clean` | 78.16ms | 200.00ms | 39.1% | 9.66% | PASS |
| `merge_data_no_conflicts` | 26.83ms | 150.00ms | 17.9% | 10.36% | PASS |
| `merge_schema_no_conflicts` | 14.86ms | 100.00ms | 14.9% | 2.46% | PASS |
| `merge_data_conflicts` | 73.41ms | 250.00ms | 29.4% | 1.55% | PASS |
| `merge_data_conflicts_with_resolve` | 73.45ms | 250.00ms | 29.4% | 1.29% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
