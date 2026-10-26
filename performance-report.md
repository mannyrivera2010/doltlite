# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-26 17:51 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261023.334.3
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/40357436003)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 20s | 9.82s | 11.20s | 1.140× | 1.79% | **PASS** |
| textpk | 69 | 55 | 1h 33m 10s | 10.31s | 12.00s | 1.164× | 1.74% | **PASS** |
| blobpk | 69 | 55 | 1h 31m 37s | 9.89s | 11.75s | 1.188× | 1.88% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 0s | 9.63s | 11.94s | 1.240× | 1.27% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.57ms | 27.93ms | 1.092× | 1.87% | PASS |
| mem_reads | `oltp_range_select` | 11.03ms | 12.04ms | 1.092× | 4.02% | PASS |
| mem_reads | `oltp_sum_range` | 9.97ms | 11.55ms | 1.159× | 1.59% | PASS |
| mem_reads | `oltp_order_range` | 2.72ms | 2.89ms | 1.064× | 1.18% | PASS |
| mem_reads | `oltp_distinct_range` | 3.80ms | 3.92ms | 1.030× | 1.04% | PASS |
| mem_reads | `oltp_index_scan` | 4.15ms | 5.38ms | 1.296× | 2.25% | PASS |
| mem_reads | `select_random_points` | 11.72ms | 11.82ms | 1.009× | 3.86% | PASS |
| mem_reads | `select_random_ranges` | 3.18ms | 4.08ms | 1.284× | 1.89% | PASS |
| mem_reads | `covering_index_scan` | 4.41ms | 4.45ms | 1.009× | 3.87% | PASS |
| mem_reads | `groupby_scan` | 32.13ms | 34.37ms | 1.070× | 0.89% | PASS |
| mem_reads | `index_join` | 5.99ms | 8.09ms | 1.349× | 2.98% | PASS |
| mem_reads | `index_join_scan` | 3.58ms | 4.77ms | 1.332× | 1.79% | PASS |
| mem_reads | `types_table_scan` | 1.16s | 1.29s | 1.114× | 2.18% | PASS |
| mem_reads | `table_scan` | 1.35s | 1.40s | 1.043× | 2.48% | PASS |
| mem_reads | `oltp_read_only` | 105.37ms | 115.42ms | 1.095× | 1.67% | PASS |
| mem_writes | `oltp_bulk_insert` | 183.13ms | 242.34ms | 1.323× | 0.99% | PASS |
| mem_writes | `oltp_insert` | 15.96ms | 28.23ms | 1.769× | 0.82% | PASS |
| mem_writes | `oltp_update_index` | 54.59ms | 110.97ms | 2.033× | 2.07% | PASS |
| mem_writes | `oltp_update_non_index` | 35.98ms | 58.87ms | 1.636× | 1.29% | PASS |
| mem_writes | `oltp_delete_insert` | 46.82ms | 80.47ms | 1.719× | 1.60% | PASS |
| mem_writes | `oltp_write_only` | 22.54ms | 45.84ms | 2.033× | 2.08% | PASS |
| mem_writes | `types_delete_insert` | 25.63ms | 40.31ms | 1.573× | 1.92% | PASS |
| mem_writes | `oltp_read_write` | 71.65ms | 109.42ms | 1.527× | 1.29% | PASS |
| file_reads | `oltp_point_select` | 121.81ms | 59.90ms | 0.492× | 0.90% | PASS |
| file_reads | `oltp_range_select` | 20.71ms | 15.30ms | 0.739× | 2.60% | PASS |
| file_reads | `oltp_sum_range` | 19.96ms | 14.90ms | 0.746× | 1.79% | PASS |
| file_reads | `oltp_order_range` | 3.67ms | 3.25ms | 0.886× | 1.66% | PASS |
| file_reads | `oltp_distinct_range` | 4.80ms | 4.27ms | 0.889× | 1.21% | PASS |
| file_reads | `oltp_index_scan` | 13.61ms | 8.57ms | 0.630× | 1.78% | PASS |
| file_reads | `select_random_points` | 20.77ms | 14.78ms | 0.712× | 3.39% | PASS |
| file_reads | `select_random_ranges` | 12.83ms | 7.28ms | 0.567× | 0.66% | PASS |
| file_reads | `covering_index_scan` | 14.38ms | 7.75ms | 0.539× | 1.68% | PASS |
| file_reads | `groupby_scan` | 33.36ms | 34.86ms | 1.045× | 1.03% | PASS |
| file_reads | `index_join` | 11.44ms | 10.40ms | 0.910× | 1.92% | PASS |
| file_reads | `index_join_scan` | 4.64ms | 5.16ms | 1.112× | 1.77% | PASS |
| file_reads | `types_table_scan` | 1.18s | 1.29s | 1.101× | 1.56% | PASS |
| file_reads | `table_scan` | 1.35s | 1.41s | 1.045× | 2.35% | PASS |
| file_reads | `oltp_read_only` | 244.62ms | 161.06ms | 0.658× | 1.43% | PASS |
| file_writes | `oltp_bulk_insert` | 199.05ms | 262.74ms | 1.320× | 1.40% | PASS |
| file_writes | `oltp_insert` | 22.43ms | 36.31ms | 1.619× | 1.85% | PASS |
| file_writes | `oltp_update_index` | 81.52ms | 134.19ms | 1.646× | 2.57% | PASS |
| file_writes | `oltp_update_non_index` | 61.95ms | 83.82ms | 1.353× | 1.69% | PASS |
| file_writes | `oltp_delete_insert` | 69.73ms | 101.97ms | 1.462× | 2.39% | PASS |
| file_writes | `oltp_write_only` | 45.60ms | 67.52ms | 1.481× | 2.14% | PASS |
| file_writes | `types_delete_insert` | 41.85ms | 54.67ms | 1.306× | 2.17% | PASS |
| file_writes | `oltp_read_write` | 92.09ms | 128.86ms | 1.399× | 3.30% | PASS |
| ac_reads | `oltp_point_select` | 57.14ms | 60.27ms | 1.055× | 1.31% | PASS |
| ac_reads | `oltp_range_select` | 14.82ms | 15.36ms | 1.037× | 2.14% | PASS |
| ac_reads | `oltp_sum_range` | 13.65ms | 15.00ms | 1.099× | 2.00% | PASS |
| ac_reads | `oltp_order_range` | 3.16ms | 3.26ms | 1.031× | 1.17% | PASS |
| ac_reads | `oltp_distinct_range` | 4.15ms | 4.26ms | 1.026× | 1.04% | PASS |
| ac_reads | `oltp_index_scan` | 7.45ms | 8.53ms | 1.145× | 1.17% | PASS |
| ac_reads | `select_random_points` | 14.46ms | 14.75ms | 1.020× | 1.38% | PASS |
| ac_reads | `select_random_ranges` | 6.32ms | 7.23ms | 1.144× | 1.38% | PASS |
| ac_reads | `covering_index_scan` | 7.91ms | 7.69ms | 0.972× | 1.45% | PASS |
| ac_reads | `groupby_scan` | 32.85ms | 34.91ms | 1.062× | 0.94% | PASS |
| ac_reads | `index_join` | 8.06ms | 10.29ms | 1.278× | 2.49% | PASS |
| ac_reads | `index_join_scan` | 4.08ms | 5.20ms | 1.275× | 1.87% | PASS |
| ac_reads | `types_table_scan` | 1.12s | 1.28s | 1.139× | 1.61% | PASS |
| ac_reads | `table_scan` | 1.37s | 1.40s | 1.024× | 1.73% | PASS |
| ac_reads | `oltp_read_only` | 156.12ms | 163.72ms | 1.049× | 1.12% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.39ms | 55.60ms | 3.392× | 4.89% | PASS |
| ac_writes | `oltp_insert_ac` | 18.62ms | 71.87ms | 3.860× | 4.72% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.51ms | 88.09ms | 4.296× | 5.01% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.16ms | 65.65ms | 3.826× | 6.61% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.29ms | 78.45ms | 4.288× | 3.96% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.84ms | 76.73ms | 4.072× | 4.34% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.37ms | 66.24ms | 4.046× | 6.03% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.19ms | 84.00ms | 3.472× | 3.27% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.35ms | 38.28ms | 1.261× | 1.71% | PASS |
| mem_reads | `oltp_range_select` | 14.17ms | 14.42ms | 1.018× | 3.05% | PASS |
| mem_reads | `oltp_sum_range` | 12.24ms | 14.16ms | 1.157× | 2.41% | PASS |
| mem_reads | `oltp_order_range` | 3.01ms | 3.17ms | 1.051× | 1.46% | PASS |
| mem_reads | `oltp_distinct_range` | 4.08ms | 4.24ms | 1.039× | 1.23% | PASS |
| mem_reads | `oltp_index_scan` | 4.59ms | 6.39ms | 1.392× | 2.54% | PASS |
| mem_reads | `select_random_points` | 17.51ms | 20.96ms | 1.197× | 2.10% | PASS |
| mem_reads | `select_random_ranges` | 4.07ms | 5.22ms | 1.282× | 1.63% | PASS |
| mem_reads | `covering_index_scan` | 5.19ms | 4.81ms | 0.926× | 4.13% | PASS |
| mem_reads | `groupby_scan` | 32.89ms | 35.04ms | 1.065× | 0.94% | PASS |
| mem_reads | `index_join` | 7.52ms | 9.97ms | 1.327× | 2.47% | PASS |
| mem_reads | `index_join_scan` | 4.81ms | 5.60ms | 1.165× | 2.81% | PASS |
| mem_reads | `types_table_scan` | 1.25s | 1.28s | 1.026× | 1.86% | PASS |
| mem_reads | `table_scan` | 1.33s | 1.40s | 1.050× | 3.42% | PASS |
| mem_reads | `oltp_read_only` | 124.25ms | 139.16ms | 1.120× | 1.37% | PASS |
| mem_writes | `oltp_bulk_insert` | 230.17ms | 355.30ms | 1.544× | 0.87% | PASS |
| mem_writes | `oltp_insert` | 22.04ms | 39.92ms | 1.811× | 1.24% | PASS |
| mem_writes | `oltp_update_index` | 73.97ms | 137.35ms | 1.857× | 1.56% | PASS |
| mem_writes | `oltp_update_non_index` | 49.74ms | 89.01ms | 1.789× | 1.63% | PASS |
| mem_writes | `oltp_delete_insert` | 52.67ms | 105.44ms | 2.002× | 1.78% | PASS |
| mem_writes | `oltp_write_only` | 28.77ms | 61.51ms | 2.138× | 1.47% | PASS |
| mem_writes | `types_delete_insert` | 32.70ms | 55.58ms | 1.700× | 1.06% | PASS |
| mem_writes | `oltp_read_write` | 87.68ms | 141.53ms | 1.614× | 1.74% | PASS |
| file_reads | `oltp_point_select` | 103.85ms | 63.32ms | 0.610× | 0.98% | PASS |
| file_reads | `oltp_range_select` | 21.25ms | 16.88ms | 0.795× | 2.08% | PASS |
| file_reads | `oltp_sum_range` | 20.58ms | 17.09ms | 0.831× | 1.65% | PASS |
| file_reads | `oltp_order_range` | 4.05ms | 3.59ms | 0.886× | 1.94% | PASS |
| file_reads | `oltp_distinct_range` | 5.02ms | 4.60ms | 0.917× | 1.53% | PASS |
| file_reads | `oltp_index_scan` | 12.18ms | 9.19ms | 0.755× | 1.23% | PASS |
| file_reads | `select_random_points` | 26.57ms | 24.61ms | 0.927× | 1.54% | PASS |
| file_reads | `select_random_ranges` | 11.84ms | 7.92ms | 0.669× | 1.23% | PASS |
| file_reads | `covering_index_scan` | 13.59ms | 7.57ms | 0.557× | 3.37% | PASS |
| file_reads | `groupby_scan` | 34.05ms | 35.67ms | 1.048× | 1.50% | PASS |
| file_reads | `index_join` | 12.75ms | 12.04ms | 0.944× | 2.63% | PASS |
| file_reads | `index_join_scan` | 5.77ms | 6.02ms | 1.043× | 1.83% | PASS |
| file_reads | `types_table_scan` | 1.09s | 1.25s | 1.144× | 2.37% | PASS |
| file_reads | `table_scan` | 1.29s | 1.39s | 1.075× | 3.15% | PASS |
| file_reads | `oltp_read_only` | 227.20ms | 174.65ms | 0.769× | 1.18% | PASS |
| file_writes | `oltp_bulk_insert` | 248.28ms | 381.05ms | 1.535× | 0.91% | PASS |
| file_writes | `oltp_insert` | 47.99ms | 51.81ms | 1.080× | 4.90% | PASS |
| file_writes | `oltp_update_index` | 115.20ms | 168.15ms | 1.460× | 1.23% | PASS |
| file_writes | `oltp_update_non_index` | 101.53ms | 111.68ms | 1.100× | 10.11% | PASS |
| file_writes | `oltp_delete_insert` | 96.73ms | 134.04ms | 1.386× | 1.62% | PASS |
| file_writes | `oltp_write_only` | 86.97ms | 84.40ms | 0.970× | 11.10% | PASS |
| file_writes | `types_delete_insert` | 57.84ms | 73.89ms | 1.278× | 1.29% | PASS |
| file_writes | `oltp_read_write` | 137.79ms | 161.83ms | 1.174× | 6.08% | PASS |
| ac_reads | `oltp_point_select` | 54.22ms | 63.09ms | 1.164× | 1.22% | PASS |
| ac_reads | `oltp_range_select` | 16.88ms | 16.93ms | 1.003× | 3.13% | PASS |
| ac_reads | `oltp_sum_range` | 15.82ms | 17.10ms | 1.081× | 2.34% | PASS |
| ac_reads | `oltp_order_range` | 3.44ms | 3.52ms | 1.023× | 1.28% | PASS |
| ac_reads | `oltp_distinct_range` | 4.53ms | 4.61ms | 1.019× | 1.56% | PASS |
| ac_reads | `oltp_index_scan` | 7.65ms | 9.37ms | 1.225× | 0.98% | PASS |
| ac_reads | `select_random_points` | 22.13ms | 25.45ms | 1.150× | 2.88% | PASS |
| ac_reads | `select_random_ranges` | 6.76ms | 7.82ms | 1.157× | 1.01% | PASS |
| ac_reads | `covering_index_scan` | 8.36ms | 7.38ms | 0.883× | 1.79% | PASS |
| ac_reads | `groupby_scan` | 32.67ms | 35.08ms | 1.074× | 1.13% | PASS |
| ac_reads | `index_join` | 9.42ms | 11.31ms | 1.200× | 1.67% | PASS |
| ac_reads | `index_join_scan` | 5.23ms | 5.99ms | 1.145× | 1.32% | PASS |
| ac_reads | `types_table_scan` | 1.24s | 1.28s | 1.025× | 1.10% | PASS |
| ac_reads | `table_scan` | 1.27s | 1.38s | 1.091× | 3.13% | PASS |
| ac_reads | `oltp_read_only` | 153.53ms | 175.03ms | 1.140× | 1.06% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 31.05ms | 79.65ms | 2.566× | 7.16% | PASS |
| ac_writes | `oltp_insert_ac` | 32.29ms | 97.22ms | 3.011× | 8.40% | PASS |
| ac_writes | `oltp_update_index_ac` | 34.82ms | 111.79ms | 3.211× | 8.26% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 31.66ms | 95.09ms | 3.003× | 8.09% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 33.05ms | 108.40ms | 3.280× | 8.23% | PASS |
| ac_writes | `oltp_write_only_ac` | 34.31ms | 109.24ms | 3.184× | 8.63% | PASS |
| ac_writes | `types_delete_insert_ac` | 28.58ms | 98.58ms | 3.450× | 10.41% | PASS |
| ac_writes | `oltp_read_write_ac` | 42.68ms | 117.63ms | 2.756× | 7.23% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.02ms | 36.88ms | 1.189× | 1.43% | PASS |
| mem_reads | `oltp_range_select` | 13.82ms | 14.03ms | 1.015× | 2.04% | PASS |
| mem_reads | `oltp_sum_range` | 13.02ms | 14.10ms | 1.083× | 2.11% | PASS |
| mem_reads | `oltp_order_range` | 3.04ms | 3.17ms | 1.040× | 1.27% | PASS |
| mem_reads | `oltp_distinct_range` | 4.09ms | 4.22ms | 1.031× | 1.15% | PASS |
| mem_reads | `oltp_index_scan` | 4.63ms | 6.24ms | 1.348× | 1.81% | PASS |
| mem_reads | `select_random_points` | 18.50ms | 20.64ms | 1.116× | 1.71% | PASS |
| mem_reads | `select_random_ranges` | 4.21ms | 5.14ms | 1.221× | 1.98% | PASS |
| mem_reads | `covering_index_scan` | 4.41ms | 4.54ms | 1.028× | 1.92% | PASS |
| mem_reads | `groupby_scan` | 32.03ms | 34.12ms | 1.065× | 0.77% | PASS |
| mem_reads | `index_join` | 6.85ms | 9.18ms | 1.341× | 2.18% | PASS |
| mem_reads | `index_join_scan` | 4.43ms | 5.24ms | 1.182× | 2.09% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.24s | 1.153× | 2.04% | PASS |
| mem_reads | `table_scan` | 1.29s | 1.39s | 1.077× | 2.79% | PASS |
| mem_reads | `oltp_read_only` | 123.51ms | 135.78ms | 1.099× | 1.70% | PASS |
| mem_writes | `oltp_bulk_insert` | 242.67ms | 354.11ms | 1.459× | 1.02% | PASS |
| mem_writes | `oltp_insert` | 20.05ms | 39.31ms | 1.961× | 1.13% | PASS |
| mem_writes | `oltp_update_index` | 69.37ms | 129.88ms | 1.872× | 2.04% | PASS |
| mem_writes | `oltp_update_non_index` | 49.70ms | 84.39ms | 1.698× | 1.32% | PASS |
| mem_writes | `oltp_delete_insert` | 49.37ms | 102.45ms | 2.075× | 1.53% | PASS |
| mem_writes | `oltp_write_only` | 28.31ms | 61.38ms | 2.168× | 1.04% | PASS |
| mem_writes | `types_delete_insert` | 33.22ms | 54.47ms | 1.640× | 1.60% | PASS |
| mem_writes | `oltp_read_write` | 88.42ms | 140.16ms | 1.585× | 2.87% | PASS |
| file_reads | `oltp_point_select` | 106.84ms | 63.07ms | 0.590× | 0.77% | PASS |
| file_reads | `oltp_range_select` | 21.34ms | 16.63ms | 0.779× | 1.28% | PASS |
| file_reads | `oltp_sum_range` | 20.78ms | 16.80ms | 0.808× | 2.03% | PASS |
| file_reads | `oltp_order_range` | 3.91ms | 3.50ms | 0.897× | 1.50% | PASS |
| file_reads | `oltp_distinct_range` | 5.00ms | 4.59ms | 0.917× | 1.12% | PASS |
| file_reads | `oltp_index_scan` | 12.59ms | 9.03ms | 0.718× | 1.65% | PASS |
| file_reads | `select_random_points` | 27.52ms | 24.27ms | 0.882× | 1.07% | PASS |
| file_reads | `select_random_ranges` | 11.87ms | 7.77ms | 0.654× | 1.19% | PASS |
| file_reads | `covering_index_scan` | 12.65ms | 7.29ms | 0.576× | 3.86% | PASS |
| file_reads | `groupby_scan` | 33.09ms | 34.80ms | 1.052× | 0.84% | PASS |
| file_reads | `index_join` | 11.60ms | 11.22ms | 0.968× | 2.90% | PASS |
| file_reads | `index_join_scan` | 5.59ms | 5.82ms | 1.040× | 2.57% | PASS |
| file_reads | `types_table_scan` | 1.08s | 1.24s | 1.149× | 2.55% | PASS |
| file_reads | `table_scan` | 1.26s | 1.38s | 1.090× | 2.06% | PASS |
| file_reads | `oltp_read_only` | 237.10ms | 175.46ms | 0.740× | 1.51% | PASS |
| file_writes | `oltp_bulk_insert` | 260.92ms | 379.12ms | 1.453× | 1.07% | PASS |
| file_writes | `oltp_insert` | 32.22ms | 51.98ms | 1.613× | 3.27% | PASS |
| file_writes | `oltp_update_index` | 107.52ms | 164.31ms | 1.528× | 1.94% | PASS |
| file_writes | `oltp_update_non_index` | 80.26ms | 108.85ms | 1.356× | 1.56% | PASS |
| file_writes | `oltp_delete_insert` | 82.85ms | 131.24ms | 1.584× | 1.11% | PASS |
| file_writes | `oltp_write_only` | 56.09ms | 83.79ms | 1.494× | 1.63% | PASS |
| file_writes | `types_delete_insert` | 53.84ms | 72.50ms | 1.346× | 1.64% | PASS |
| file_writes | `oltp_read_write` | 122.65ms | 167.68ms | 1.367× | 2.49% | PASS |
| ac_reads | `oltp_point_select` | 57.07ms | 63.75ms | 1.117× | 1.22% | PASS |
| ac_reads | `oltp_range_select` | 16.56ms | 16.68ms | 1.007× | 2.30% | PASS |
| ac_reads | `oltp_sum_range` | 15.47ms | 16.73ms | 1.082× | 1.56% | PASS |
| ac_reads | `oltp_order_range` | 3.37ms | 3.49ms | 1.036× | 1.28% | PASS |
| ac_reads | `oltp_distinct_range` | 4.44ms | 4.59ms | 1.034× | 1.30% | PASS |
| ac_reads | `oltp_index_scan` | 7.58ms | 9.04ms | 1.194× | 1.88% | PASS |
| ac_reads | `select_random_points` | 22.25ms | 24.48ms | 1.101× | 2.75% | PASS |
| ac_reads | `select_random_ranges` | 6.87ms | 7.79ms | 1.134× | 1.29% | PASS |
| ac_reads | `covering_index_scan` | 7.93ms | 7.31ms | 0.923× | 3.74% | PASS |
| ac_reads | `groupby_scan` | 32.48ms | 34.81ms | 1.072× | 0.95% | PASS |
| ac_reads | `index_join` | 9.24ms | 11.20ms | 1.211× | 2.56% | PASS |
| ac_reads | `index_join_scan` | 4.96ms | 5.74ms | 1.156× | 1.96% | PASS |
| ac_reads | `types_table_scan` | 1.12s | 1.25s | 1.113× | 2.10% | PASS |
| ac_reads | `table_scan` | 1.36s | 1.39s | 1.025× | 3.08% | PASS |
| ac_reads | `oltp_read_only` | 164.59ms | 177.21ms | 1.077× | 1.40% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.96ms | 66.26ms | 3.017× | 5.62% | PASS |
| ac_writes | `oltp_insert_ac` | 25.86ms | 90.08ms | 3.484× | 7.17% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.38ms | 97.81ms | 3.572× | 5.16% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.14ms | 79.63ms | 3.298× | 6.79% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.87ms | 90.53ms | 3.640× | 4.71% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.17ms | 88.57ms | 3.519× | 5.62% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.20ms | 79.80ms | 3.440× | 7.00% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.80ms | 96.59ms | 3.037× | 5.03% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.91ms | 40.33ms | 1.226× | 1.36% | PASS |
| mem_reads | `oltp_range_select` | 19.08ms | 21.11ms | 1.106× | 1.36% | PASS |
| mem_reads | `oltp_sum_range` | 18.18ms | 20.99ms | 1.155× | 1.51% | PASS |
| mem_reads | `oltp_order_range` | 3.66ms | 3.89ms | 1.063× | 1.19% | PASS |
| mem_reads | `oltp_distinct_range` | 4.77ms | 4.95ms | 1.037× | 0.98% | PASS |
| mem_reads | `oltp_index_scan` | 4.82ms | 6.56ms | 1.362× | 1.94% | PASS |
| mem_reads | `select_random_points` | 28.61ms | 32.52ms | 1.137× | 2.02% | PASS |
| mem_reads | `select_random_ranges` | 7.85ms | 9.08ms | 1.157× | 1.66% | PASS |
| mem_reads | `covering_index_scan` | 4.25ms | 4.27ms | 1.004× | 2.36% | PASS |
| mem_reads | `groupby_scan` | 36.63ms | 39.34ms | 1.074× | 0.79% | PASS |
| mem_reads | `index_join` | 8.24ms | 10.40ms | 1.263× | 1.89% | PASS |
| mem_reads | `index_join_scan` | 4.19ms | 5.48ms | 1.307× | 1.34% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.24s | 1.186× | 0.68% | PASS |
| mem_reads | `table_scan` | 1.19s | 1.39s | 1.170× | 0.82% | PASS |
| mem_reads | `oltp_read_only` | 152.69ms | 170.08ms | 1.114× | 1.19% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.99ms | 357.45ms | 1.430× | 0.87% | PASS |
| mem_writes | `oltp_insert` | 19.30ms | 36.40ms | 1.885× | 0.71% | PASS |
| mem_writes | `oltp_update_index` | 67.97ms | 116.18ms | 1.709× | 1.30% | PASS |
| mem_writes | `oltp_update_non_index` | 50.93ms | 83.17ms | 1.633× | 1.12% | PASS |
| mem_writes | `oltp_delete_insert` | 49.91ms | 95.74ms | 1.918× | 1.14% | PASS |
| mem_writes | `oltp_write_only` | 27.20ms | 57.52ms | 2.115× | 0.90% | PASS |
| mem_writes | `types_delete_insert` | 32.84ms | 54.67ms | 1.665× | 1.01% | PASS |
| mem_writes | `oltp_read_write` | 102.00ms | 155.64ms | 1.526× | 1.17% | PASS |
| file_reads | `oltp_point_select` | 109.16ms | 66.89ms | 0.613× | 0.96% | PASS |
| file_reads | `oltp_range_select` | 27.22ms | 24.23ms | 0.890× | 1.30% | PASS |
| file_reads | `oltp_sum_range` | 26.33ms | 24.05ms | 0.913× | 1.22% | PASS |
| file_reads | `oltp_order_range` | 4.52ms | 4.22ms | 0.935× | 1.43% | PASS |
| file_reads | `oltp_distinct_range` | 5.69ms | 5.34ms | 0.939× | 1.33% | PASS |
| file_reads | `oltp_index_scan` | 12.66ms | 9.22ms | 0.728× | 1.56% | PASS |
| file_reads | `select_random_points` | 37.88ms | 36.28ms | 0.958× | 1.20% | PASS |
| file_reads | `select_random_ranges` | 15.88ms | 12.04ms | 0.758× | 1.07% | PASS |
| file_reads | `covering_index_scan` | 12.18ms | 7.18ms | 0.589× | 1.28% | PASS |
| file_reads | `groupby_scan` | 37.57ms | 39.95ms | 1.063× | 0.64% | PASS |
| file_reads | `index_join` | 12.79ms | 12.77ms | 0.999× | 1.62% | PASS |
| file_reads | `index_join_scan` | 5.24ms | 5.97ms | 1.140× | 1.14% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.23s | 1.190× | 0.70% | PASS |
| file_reads | `table_scan` | 1.22s | 1.39s | 1.143× | 0.96% | PASS |
| file_reads | `oltp_read_only` | 266.57ms | 211.21ms | 0.792× | 0.87% | PASS |
| file_writes | `oltp_bulk_insert` | 264.86ms | 381.09ms | 1.439× | 0.74% | PASS |
| file_writes | `oltp_insert` | 26.54ms | 46.57ms | 1.754× | 1.45% | PASS |
| file_writes | `oltp_update_index` | 100.13ms | 144.04ms | 1.439× | 1.11% | PASS |
| file_writes | `oltp_update_non_index` | 78.78ms | 105.79ms | 1.343× | 1.55% | PASS |
| file_writes | `oltp_delete_insert` | 78.77ms | 120.39ms | 1.528× | 1.16% | PASS |
| file_writes | `oltp_write_only` | 52.41ms | 78.37ms | 1.495× | 1.39% | PASS |
| file_writes | `types_delete_insert` | 51.04ms | 69.45ms | 1.361× | 1.32% | PASS |
| file_writes | `oltp_read_write` | 130.68ms | 176.96ms | 1.354× | 1.26% | PASS |
| ac_reads | `oltp_point_select` | 58.65ms | 66.89ms | 1.140× | 1.07% | PASS |
| ac_reads | `oltp_range_select` | 22.15ms | 24.18ms | 1.092× | 0.99% | PASS |
| ac_reads | `oltp_sum_range` | 21.07ms | 24.11ms | 1.144× | 1.34% | PASS |
| ac_reads | `oltp_order_range` | 3.97ms | 4.26ms | 1.074× | 1.40% | PASS |
| ac_reads | `oltp_distinct_range` | 5.14ms | 5.34ms | 1.039× | 1.94% | PASS |
| ac_reads | `oltp_index_scan` | 7.57ms | 9.22ms | 1.219× | 1.56% | PASS |
| ac_reads | `select_random_points` | 31.87ms | 36.41ms | 1.142× | 1.33% | PASS |
| ac_reads | `select_random_ranges` | 10.70ms | 12.10ms | 1.130× | 1.07% | PASS |
| ac_reads | `covering_index_scan` | 7.05ms | 7.24ms | 1.027× | 0.94% | PASS |
| ac_reads | `groupby_scan` | 36.86ms | 39.91ms | 1.083× | 0.82% | PASS |
| ac_reads | `index_join` | 9.99ms | 12.79ms | 1.281× | 1.27% | PASS |
| ac_reads | `index_join_scan` | 4.67ms | 5.97ms | 1.280× | 1.46% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.23s | 1.191× | 0.65% | PASS |
| ac_reads | `table_scan` | 1.21s | 1.39s | 1.150× | 1.00% | PASS |
| ac_reads | `oltp_read_only` | 190.54ms | 210.67ms | 1.106× | 1.04% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.16ms | 67.13ms | 2.778× | 5.21% | PASS |
| ac_writes | `oltp_insert_ac` | 25.37ms | 89.54ms | 3.529× | 4.46% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.44ms | 102.61ms | 3.608× | 3.88% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.67ms | 80.88ms | 3.150× | 5.78% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.00ms | 91.88ms | 3.534× | 5.96% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.82ms | 91.66ms | 3.549× | 5.45% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.00ms | 81.15ms | 3.529× | 8.05% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.92ms | 100.90ms | 3.065× | 4.92% | PASS |

</details>

## Version-control latency

Wall time: 1m 47s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 58.28ms | 200.00ms | 29.1% | 1.43% | PASS |
| `status_dirty_many_tables` | 58.83ms | 200.00ms | 29.4% | 1.71% | PASS |
| `diff_regular_working_one_table` | 54.67ms | 150.00ms | 36.4% | 1.17% | PASS |
| `diff_regular_working_many_tables` | 61.71ms | 200.00ms | 30.9% | 1.36% | PASS |
| `diff_stat_working_many_tables` | 63.31ms | 200.00ms | 31.7% | 1.00% | PASS |
| `diff_schema_working_many_tables` | 63.20ms | 200.00ms | 31.6% | 0.87% | PASS |
| `branch_list_many_branches` | 17.20ms | 100.00ms | 17.2% | 1.38% | PASS |
| `branch_create_delete` | 19.24ms | 100.00ms | 19.2% | 2.33% | PASS |
| `checkout_branch_clean` | 78.81ms | 200.00ms | 39.4% | 1.42% | PASS |
| `merge_data_no_conflicts` | 31.67ms | 150.00ms | 21.1% | 2.48% | PASS |
| `merge_schema_no_conflicts` | 17.57ms | 100.00ms | 17.6% | 2.65% | PASS |
| `merge_data_conflicts` | 79.89ms | 250.00ms | 32.0% | 0.98% | PASS |
| `merge_data_conflicts_with_resolve` | 78.68ms | 250.00ms | 31.5% | 0.87% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
