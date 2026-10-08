# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-08 18:26 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261004.327.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/37811731878)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 43s | 8.92s | 11.45s | 1.283× | 1.60% | **PASS** |
| textpk | 69 | 55 | 1h 34m 27s | 10.46s | 11.91s | 1.139× | 2.13% | **PASS** |
| blobpk | 69 | 55 | 1h 27m 4s | 9.38s | 11.06s | 1.179× | 0.94% | **PASS** |
| compositepk | 69 | 55 | 1h 2m 44s | 7.12s | 7.74s | 1.087× | 2.65% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.56ms | 29.76ms | 1.211× | 1.94% | PASS |
| mem_reads | `oltp_range_select` | 10.33ms | 13.21ms | 1.278× | 1.20% | PASS |
| mem_reads | `oltp_sum_range` | 9.80ms | 12.40ms | 1.265× | 1.70% | PASS |
| mem_reads | `oltp_order_range` | 2.57ms | 2.99ms | 1.162× | 0.68% | PASS |
| mem_reads | `oltp_distinct_range` | 3.65ms | 4.04ms | 1.106× | 1.39% | PASS |
| mem_reads | `oltp_index_scan` | 3.99ms | 5.44ms | 1.364× | 1.86% | PASS |
| mem_reads | `select_random_points` | 11.13ms | 11.50ms | 1.033× | 3.60% | PASS |
| mem_reads | `select_random_ranges` | 3.08ms | 4.01ms | 1.299× | 1.47% | PASS |
| mem_reads | `covering_index_scan` | 4.24ms | 4.25ms | 1.004× | 1.55% | PASS |
| mem_reads | `groupby_scan` | 30.01ms | 33.39ms | 1.113× | 1.18% | PASS |
| mem_reads | `index_join` | 6.05ms | 8.24ms | 1.361× | 1.65% | PASS |
| mem_reads | `index_join_scan` | 3.47ms | 4.54ms | 1.309× | 2.13% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.35s | 1.263× | 1.86% | PASS |
| mem_reads | `table_scan` | 1.19s | 1.40s | 1.180× | 1.96% | PASS |
| mem_reads | `oltp_read_only` | 103.03ms | 123.57ms | 1.199× | 1.60% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.74ms | 251.72ms | 1.393× | 1.13% | PASS |
| mem_writes | `oltp_insert` | 15.46ms | 28.40ms | 1.837× | 0.99% | PASS |
| mem_writes | `oltp_update_index` | 50.19ms | 104.82ms | 2.088× | 0.86% | PASS |
| mem_writes | `oltp_update_non_index` | 33.35ms | 58.59ms | 1.757× | 1.48% | PASS |
| mem_writes | `oltp_delete_insert` | 44.23ms | 78.18ms | 1.767× | 1.18% | PASS |
| mem_writes | `oltp_write_only` | 21.86ms | 44.84ms | 2.052× | 1.20% | PASS |
| mem_writes | `types_delete_insert` | 24.24ms | 39.93ms | 1.647× | 1.33% | PASS |
| mem_writes | `oltp_read_write` | 65.59ms | 109.17ms | 1.664× | 1.31% | PASS |
| file_reads | `oltp_point_select` | 97.39ms | 54.70ms | 0.562× | 0.75% | PASS |
| file_reads | `oltp_range_select` | 17.50ms | 15.79ms | 0.902× | 1.53% | PASS |
| file_reads | `oltp_sum_range` | 17.28ms | 15.21ms | 0.880× | 2.04% | PASS |
| file_reads | `oltp_order_range` | 3.43ms | 3.37ms | 0.984× | 2.82% | PASS |
| file_reads | `oltp_distinct_range` | 4.51ms | 4.42ms | 0.980× | 2.12% | PASS |
| file_reads | `oltp_index_scan` | 11.54ms | 8.10ms | 0.701× | 2.07% | PASS |
| file_reads | `select_random_points` | 17.09ms | 13.76ms | 0.805× | 2.33% | PASS |
| file_reads | `select_random_ranges` | 10.37ms | 6.56ms | 0.633× | 1.42% | PASS |
| file_reads | `covering_index_scan` | 11.83ms | 6.82ms | 0.576× | 2.17% | PASS |
| file_reads | `groupby_scan` | 30.95ms | 33.93ms | 1.096× | 1.08% | PASS |
| file_reads | `index_join` | 10.29ms | 10.18ms | 0.989× | 1.35% | PASS |
| file_reads | `index_join_scan` | 4.50ms | 5.00ms | 1.112× | 2.05% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.33s | 1.283× | 1.05% | PASS |
| file_reads | `table_scan` | 1.19s | 1.40s | 1.176× | 2.03% | PASS |
| file_reads | `oltp_read_only` | 212.72ms | 161.57ms | 0.760× | 0.79% | PASS |
| file_writes | `oltp_bulk_insert` | 194.29ms | 270.82ms | 1.394× | 1.47% | PASS |
| file_writes | `oltp_insert` | 22.10ms | 35.41ms | 1.602× | 1.71% | PASS |
| file_writes | `oltp_update_index` | 77.15ms | 128.48ms | 1.665× | 1.44% | PASS |
| file_writes | `oltp_update_non_index` | 58.43ms | 81.45ms | 1.394× | 1.61% | PASS |
| file_writes | `oltp_delete_insert` | 67.95ms | 98.68ms | 1.452× | 1.66% | PASS |
| file_writes | `oltp_write_only` | 44.40ms | 64.20ms | 1.446× | 3.96% | PASS |
| file_writes | `types_delete_insert` | 40.13ms | 53.62ms | 1.336× | 2.07% | PASS |
| file_writes | `oltp_read_write` | 91.17ms | 129.07ms | 1.416× | 1.49% | PASS |
| ac_reads | `oltp_point_select` | 48.73ms | 55.22ms | 1.133× | 0.80% | PASS |
| ac_reads | `oltp_range_select` | 13.10ms | 15.88ms | 1.212× | 1.29% | PASS |
| ac_reads | `oltp_sum_range` | 12.72ms | 15.14ms | 1.191× | 1.35% | PASS |
| ac_reads | `oltp_order_range` | 3.05ms | 3.41ms | 1.118× | 2.65% | PASS |
| ac_reads | `oltp_distinct_range` | 4.08ms | 4.45ms | 1.090× | 2.19% | PASS |
| ac_reads | `oltp_index_scan` | 6.83ms | 8.29ms | 1.214× | 1.00% | PASS |
| ac_reads | `select_random_points` | 13.38ms | 14.01ms | 1.047× | 2.08% | PASS |
| ac_reads | `select_random_ranges` | 5.61ms | 6.61ms | 1.179× | 1.08% | PASS |
| ac_reads | `covering_index_scan` | 7.10ms | 7.09ms | 0.999× | 1.16% | PASS |
| ac_reads | `groupby_scan` | 30.17ms | 33.85ms | 1.122× | 1.07% | PASS |
| ac_reads | `index_join` | 7.68ms | 10.11ms | 1.316× | 1.60% | PASS |
| ac_reads | `index_join_scan` | 3.88ms | 4.94ms | 1.271× | 1.84% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.32s | 1.281× | 0.58% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.41s | 1.173× | 1.78% | PASS |
| ac_reads | `oltp_read_only` | 139.24ms | 161.10ms | 1.157× | 1.14% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.40ms | 72.21ms | 3.086× | 15.39% | PASS |
| ac_writes | `oltp_insert_ac` | 28.02ms | 91.15ms | 3.253× | 5.84% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.54ms | 105.04ms | 3.556× | 8.08% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.94ms | 80.67ms | 3.110× | 15.34% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 27.54ms | 95.27ms | 3.459× | 9.22% | PASS |
| ac_writes | `oltp_write_only_ac` | 28.58ms | 93.56ms | 3.274× | 9.31% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.74ms | 88.14ms | 3.713× | 14.64% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.47ms | 103.33ms | 3.182× | 8.08% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.47ms | 37.98ms | 1.246× | 1.65% | PASS |
| mem_reads | `oltp_range_select` | 14.10ms | 14.26ms | 1.012× | 2.91% | PASS |
| mem_reads | `oltp_sum_range` | 13.21ms | 14.58ms | 1.104× | 3.54% | PASS |
| mem_reads | `oltp_order_range` | 3.07ms | 3.20ms | 1.042× | 1.54% | PASS |
| mem_reads | `oltp_distinct_range` | 4.15ms | 4.29ms | 1.033× | 1.00% | PASS |
| mem_reads | `oltp_index_scan` | 4.77ms | 6.54ms | 1.371× | 1.52% | PASS |
| mem_reads | `select_random_points` | 19.53ms | 21.97ms | 1.125× | 2.16% | PASS |
| mem_reads | `select_random_ranges` | 4.33ms | 5.31ms | 1.225× | 1.76% | PASS |
| mem_reads | `covering_index_scan` | 5.58ms | 4.91ms | 0.880× | 3.67% | PASS |
| mem_reads | `groupby_scan` | 32.68ms | 34.63ms | 1.060× | 0.91% | PASS |
| mem_reads | `index_join` | 7.45ms | 9.84ms | 1.322× | 2.07% | PASS |
| mem_reads | `index_join_scan` | 4.85ms | 5.55ms | 1.145× | 3.41% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.24s | 1.159× | 2.13% | PASS |
| mem_reads | `table_scan` | 1.47s | 1.40s | 0.954× | 4.25% | PASS |
| mem_reads | `oltp_read_only` | 129.87ms | 140.88ms | 1.085× | 1.92% | PASS |
| mem_writes | `oltp_bulk_insert` | 234.91ms | 363.25ms | 1.546× | 0.78% | PASS |
| mem_writes | `oltp_insert` | 22.55ms | 40.26ms | 1.785× | 1.81% | PASS |
| mem_writes | `oltp_update_index` | 74.41ms | 137.03ms | 1.842× | 1.65% | PASS |
| mem_writes | `oltp_update_non_index` | 50.29ms | 88.55ms | 1.761× | 1.28% | PASS |
| mem_writes | `oltp_delete_insert` | 53.56ms | 106.14ms | 1.982× | 1.68% | PASS |
| mem_writes | `oltp_write_only` | 29.97ms | 62.47ms | 2.085× | 1.13% | PASS |
| mem_writes | `types_delete_insert` | 33.86ms | 56.16ms | 1.659× | 1.26% | PASS |
| mem_writes | `oltp_read_write` | 90.81ms | 142.34ms | 1.567× | 1.49% | PASS |
| file_reads | `oltp_point_select` | 107.48ms | 64.52ms | 0.600× | 0.85% | PASS |
| file_reads | `oltp_range_select` | 23.21ms | 17.27ms | 0.744× | 2.56% | PASS |
| file_reads | `oltp_sum_range` | 21.71ms | 17.63ms | 0.812× | 2.08% | PASS |
| file_reads | `oltp_order_range` | 4.24ms | 3.64ms | 0.858× | 1.49% | PASS |
| file_reads | `oltp_distinct_range` | 5.35ms | 4.72ms | 0.884× | 2.13% | PASS |
| file_reads | `oltp_index_scan` | 12.90ms | 9.33ms | 0.723× | 1.31% | PASS |
| file_reads | `select_random_points` | 29.10ms | 25.21ms | 0.867× | 2.20% | PASS |
| file_reads | `select_random_ranges` | 12.08ms | 7.96ms | 0.659× | 1.30% | PASS |
| file_reads | `covering_index_scan` | 13.60ms | 7.48ms | 0.550× | 3.16% | PASS |
| file_reads | `groupby_scan` | 33.84ms | 35.11ms | 1.038× | 1.22% | PASS |
| file_reads | `index_join` | 12.00ms | 11.41ms | 0.951× | 3.01% | PASS |
| file_reads | `index_join_scan` | 5.93ms | 5.97ms | 1.006× | 4.08% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.22s | 1.163× | 1.20% | PASS |
| file_reads | `table_scan` | 1.25s | 1.38s | 1.101× | 3.14% | PASS |
| file_reads | `oltp_read_only` | 229.02ms | 175.05ms | 0.764× | 0.91% | PASS |
| file_writes | `oltp_bulk_insert` | 255.04ms | 387.11ms | 1.518× | 1.20% | PASS |
| file_writes | `oltp_insert` | 48.10ms | 53.02ms | 1.102× | 17.52% | PASS |
| file_writes | `oltp_update_index` | 116.54ms | 171.51ms | 1.472× | 2.17% | PASS |
| file_writes | `oltp_update_non_index` | 109.21ms | 116.01ms | 1.062× | 14.14% | PASS |
| file_writes | `oltp_delete_insert` | 96.57ms | 138.18ms | 1.431× | 2.24% | PASS |
| file_writes | `oltp_write_only` | 86.29ms | 88.10ms | 1.021× | 12.07% | PASS |
| file_writes | `types_delete_insert` | 57.33ms | 76.11ms | 1.328× | 2.77% | PASS |
| file_writes | `oltp_read_write` | 137.55ms | 165.27ms | 1.202× | 3.31% | PASS |
| ac_reads | `oltp_point_select` | 56.73ms | 63.90ms | 1.126× | 0.87% | PASS |
| ac_reads | `oltp_range_select` | 17.86ms | 17.09ms | 0.957× | 1.33% | PASS |
| ac_reads | `oltp_sum_range` | 16.32ms | 17.29ms | 1.060× | 1.90% | PASS |
| ac_reads | `oltp_order_range` | 3.53ms | 3.57ms | 1.011× | 1.50% | PASS |
| ac_reads | `oltp_distinct_range` | 4.58ms | 4.70ms | 1.026× | 1.60% | PASS |
| ac_reads | `oltp_index_scan` | 7.77ms | 9.26ms | 1.192× | 1.84% | PASS |
| ac_reads | `select_random_points` | 22.20ms | 24.59ms | 1.107× | 2.56% | PASS |
| ac_reads | `select_random_ranges` | 6.85ms | 7.91ms | 1.154× | 1.78% | PASS |
| ac_reads | `covering_index_scan` | 8.40ms | 7.48ms | 0.890× | 4.85% | PASS |
| ac_reads | `groupby_scan` | 33.27ms | 35.18ms | 1.057× | 1.27% | PASS |
| ac_reads | `index_join` | 9.83ms | 11.37ms | 1.156× | 2.92% | PASS |
| ac_reads | `index_join_scan` | 5.50ms | 5.96ms | 1.084× | 3.32% | PASS |
| ac_reads | `types_table_scan` | 1.24s | 1.27s | 1.023× | 4.25% | PASS |
| ac_reads | `table_scan` | 1.52s | 1.41s | 0.928× | 2.31% | PASS |
| ac_reads | `oltp_read_only` | 163.51ms | 178.31ms | 1.091× | 2.46% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.17ms | 68.63ms | 2.839× | 8.89% | PASS |
| ac_writes | `oltp_insert_ac` | 29.06ms | 90.13ms | 3.102× | 7.76% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.12ms | 102.23ms | 3.511× | 5.77% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.57ms | 80.39ms | 3.562× | 7.27% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.10ms | 93.67ms | 3.589× | 6.91% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.55ms | 95.39ms | 3.462× | 6.03% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.80ms | 83.60ms | 3.667× | 8.52% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.51ms | 105.59ms | 3.151× | 7.64% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.20ms | 29.25ms | 1.116× | 0.67% | PASS |
| mem_reads | `oltp_range_select` | 11.85ms | 12.31ms | 1.039× | 0.80% | PASS |
| mem_reads | `oltp_sum_range` | 11.17ms | 11.62ms | 1.040× | 0.66% | PASS |
| mem_reads | `oltp_order_range` | 2.73ms | 2.86ms | 1.049× | 0.80% | PASS |
| mem_reads | `oltp_distinct_range` | 3.67ms | 3.78ms | 1.028× | 0.72% | PASS |
| mem_reads | `oltp_index_scan` | 4.03ms | 5.08ms | 1.259× | 1.15% | PASS |
| mem_reads | `select_random_points` | 16.30ms | 18.61ms | 1.141× | 0.81% | PASS |
| mem_reads | `select_random_ranges` | 3.40ms | 4.43ms | 1.304× | 0.96% | PASS |
| mem_reads | `covering_index_scan` | 3.61ms | 3.74ms | 1.037× | 1.47% | PASS |
| mem_reads | `groupby_scan` | 31.07ms | 31.27ms | 1.007× | 0.37% | PASS |
| mem_reads | `index_join` | 6.24ms | 8.38ms | 1.343× | 1.53% | PASS |
| mem_reads | `index_join_scan` | 3.67ms | 5.09ms | 1.388× | 1.10% | PASS |
| mem_reads | `types_table_scan` | 1.01s | 1.17s | 1.159× | 0.30% | PASS |
| mem_reads | `table_scan` | 1.20s | 1.29s | 1.075× | 1.31% | PASS |
| mem_reads | `oltp_read_only` | 108.13ms | 117.27ms | 1.085× | 0.63% | PASS |
| mem_writes | `oltp_bulk_insert` | 189.18ms | 265.54ms | 1.404× | 0.36% | PASS |
| mem_writes | `oltp_insert` | 16.95ms | 31.41ms | 1.852× | 0.71% | PASS |
| mem_writes | `oltp_update_index` | 59.17ms | 113.04ms | 1.910× | 1.05% | PASS |
| mem_writes | `oltp_update_non_index` | 40.56ms | 68.86ms | 1.698× | 0.64% | PASS |
| mem_writes | `oltp_delete_insert` | 41.81ms | 86.53ms | 2.070× | 0.93% | PASS |
| mem_writes | `oltp_write_only` | 23.00ms | 50.79ms | 2.208× | 0.77% | PASS |
| mem_writes | `types_delete_insert` | 26.30ms | 43.84ms | 1.667× | 0.74% | PASS |
| mem_writes | `oltp_read_write` | 69.32ms | 112.96ms | 1.630× | 0.80% | PASS |
| file_reads | `oltp_point_select` | 57.87ms | 40.57ms | 0.701× | 0.81% | PASS |
| file_reads | `oltp_range_select` | 15.60ms | 13.85ms | 0.888× | 1.00% | PASS |
| file_reads | `oltp_sum_range` | 15.05ms | 13.20ms | 0.877× | 0.62% | PASS |
| file_reads | `oltp_order_range` | 3.15ms | 3.07ms | 0.974× | 1.13% | PASS |
| file_reads | `oltp_distinct_range` | 4.07ms | 3.98ms | 0.976× | 1.08% | PASS |
| file_reads | `oltp_index_scan` | 7.38ms | 6.45ms | 0.873× | 1.27% | PASS |
| file_reads | `select_random_points` | 19.90ms | 20.12ms | 1.011× | 0.62% | PASS |
| file_reads | `select_random_ranges` | 6.81ms | 5.68ms | 0.834× | 1.18% | PASS |
| file_reads | `covering_index_scan` | 7.33ms | 5.23ms | 0.714× | 1.58% | PASS |
| file_reads | `groupby_scan` | 31.55ms | 31.55ms | 1.000× | 0.44% | PASS |
| file_reads | `index_join` | 8.39ms | 9.72ms | 1.158× | 0.95% | PASS |
| file_reads | `index_join_scan` | 4.22ms | 5.47ms | 1.297× | 1.23% | PASS |
| file_reads | `types_table_scan` | 1.02s | 1.17s | 1.157× | 0.37% | PASS |
| file_reads | `table_scan` | 1.18s | 1.28s | 1.085× | 0.48% | PASS |
| file_reads | `oltp_read_only` | 151.68ms | 131.78ms | 0.869× | 0.75% | PASS |
| file_writes | `oltp_bulk_insert` | 251.96ms | 342.55ms | 1.360× | 3.10% | PASS |
| file_writes | `oltp_insert` | 59.82ms | 60.20ms | 1.006× | 1.87% | PASS |
| file_writes | `oltp_update_index` | 187.06ms | 204.68ms | 1.094× | 4.35% | PASS |
| file_writes | `oltp_update_non_index` | 146.56ms | 133.44ms | 0.910× | 0.71% | PASS |
| file_writes | `oltp_delete_insert` | 149.87ms | 163.76ms | 1.093× | 0.74% | PASS |
| file_writes | `oltp_write_only` | 110.39ms | 115.00ms | 1.042× | 0.94% | PASS |
| file_writes | `types_delete_insert` | 91.55ms | 96.21ms | 1.051× | 4.69% | PASS |
| file_writes | `oltp_read_write` | 165.99ms | 177.39ms | 1.069× | 1.18% | PASS |
| ac_reads | `oltp_point_select` | 35.76ms | 40.45ms | 1.131× | 0.68% | PASS |
| ac_reads | `oltp_range_select` | 13.20ms | 13.78ms | 1.044× | 0.87% | PASS |
| ac_reads | `oltp_sum_range` | 12.84ms | 13.20ms | 1.028× | 0.89% | PASS |
| ac_reads | `oltp_order_range` | 2.94ms | 3.05ms | 1.039× | 1.09% | PASS |
| ac_reads | `oltp_distinct_range` | 3.89ms | 3.97ms | 1.022× | 0.62% | PASS |
| ac_reads | `oltp_index_scan` | 5.42ms | 6.49ms | 1.198× | 1.15% | PASS |
| ac_reads | `select_random_points` | 18.20ms | 20.23ms | 1.112× | 0.91% | PASS |
| ac_reads | `select_random_ranges` | 4.70ms | 5.69ms | 1.211× | 0.54% | PASS |
| ac_reads | `covering_index_scan` | 5.15ms | 5.18ms | 1.005× | 1.58% | PASS |
| ac_reads | `groupby_scan` | 31.44ms | 31.52ms | 1.003× | 0.50% | PASS |
| ac_reads | `index_join` | 7.50ms | 9.79ms | 1.305× | 0.76% | PASS |
| ac_reads | `index_join_scan` | 4.09ms | 5.49ms | 1.343× | 1.06% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.19s | 1.148× | 0.95% | PASS |
| ac_reads | `table_scan` | 1.24s | 1.30s | 1.050× | 2.25% | PASS |
| ac_reads | `oltp_read_only` | 120.21ms | 132.44ms | 1.102× | 0.69% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 28.51ms | 75.45ms | 2.646× | 3.32% | PASS |
| ac_writes | `oltp_insert_ac` | 30.35ms | 94.08ms | 3.100× | 2.04% | PASS |
| ac_writes | `oltp_update_index_ac` | 31.99ms | 106.38ms | 3.326× | 3.59% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 28.49ms | 87.15ms | 3.059× | 3.13% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 29.80ms | 98.95ms | 3.321× | 4.13% | PASS |
| ac_writes | `oltp_write_only_ac` | 30.36ms | 98.36ms | 3.240× | 4.10% | PASS |
| ac_writes | `types_delete_insert_ac` | 28.18ms | 86.62ms | 3.074× | 6.18% | PASS |
| ac_writes | `oltp_read_write_ac` | 34.45ms | 109.55ms | 3.180× | 5.00% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.90ms | 26.05ms | 1.138× | 2.11% | PASS |
| mem_reads | `oltp_range_select` | 11.99ms | 12.24ms | 1.021× | 2.10% | PASS |
| mem_reads | `oltp_sum_range` | 10.82ms | 11.69ms | 1.081× | 2.31% | PASS |
| mem_reads | `oltp_order_range` | 2.41ms | 2.33ms | 0.967× | 1.49% | PASS |
| mem_reads | `oltp_distinct_range` | 3.03ms | 2.91ms | 0.961× | 1.51% | PASS |
| mem_reads | `oltp_index_scan` | 3.22ms | 4.52ms | 1.403× | 2.50% | PASS |
| mem_reads | `select_random_points` | 18.43ms | 20.84ms | 1.131× | 3.78% | PASS |
| mem_reads | `select_random_ranges` | 4.63ms | 5.25ms | 1.132× | 2.84% | PASS |
| mem_reads | `covering_index_scan` | 2.45ms | 2.77ms | 1.132× | 4.01% | PASS |
| mem_reads | `groupby_scan` | 20.75ms | 21.53ms | 1.038× | 1.63% | PASS |
| mem_reads | `index_join` | 4.98ms | 6.28ms | 1.262× | 5.37% | PASS |
| mem_reads | `index_join_scan` | 2.75ms | 4.14ms | 1.505× | 5.83% | PASS |
| mem_reads | `types_table_scan` | 705.98ms | 753.83ms | 1.068× | 2.09% | PASS |
| mem_reads | `table_scan` | 917.77ms | 857.85ms | 0.935× | 3.75% | PASS |
| mem_reads | `oltp_read_only` | 86.85ms | 90.23ms | 1.039× | 3.15% | PASS |
| mem_writes | `oltp_bulk_insert` | 148.33ms | 205.90ms | 1.388× | 2.18% | PASS |
| mem_writes | `oltp_insert` | 11.40ms | 22.55ms | 1.979× | 1.90% | PASS |
| mem_writes | `oltp_update_index` | 44.07ms | 81.98ms | 1.860× | 1.84% | PASS |
| mem_writes | `oltp_update_non_index` | 33.39ms | 55.85ms | 1.673× | 2.95% | PASS |
| mem_writes | `oltp_delete_insert` | 30.96ms | 60.33ms | 1.949× | 1.82% | PASS |
| mem_writes | `oltp_write_only` | 17.04ms | 38.18ms | 2.240× | 2.20% | PASS |
| mem_writes | `types_delete_insert` | 20.68ms | 35.44ms | 1.714× | 3.44% | PASS |
| mem_writes | `oltp_read_write` | 57.99ms | 89.65ms | 1.546× | 2.32% | PASS |
| file_reads | `oltp_point_select` | 77.28ms | 42.90ms | 0.555× | 2.58% | PASS |
| file_reads | `oltp_range_select` | 17.17ms | 13.46ms | 0.784× | 1.79% | PASS |
| file_reads | `oltp_sum_range` | 16.52ms | 13.46ms | 0.815× | 2.08% | PASS |
| file_reads | `oltp_order_range` | 2.96ms | 2.46ms | 0.830× | 2.00% | PASS |
| file_reads | `oltp_distinct_range` | 3.51ms | 2.99ms | 0.850× | 1.67% | PASS |
| file_reads | `oltp_index_scan` | 9.21ms | 6.28ms | 0.682× | 1.82% | PASS |
| file_reads | `select_random_points` | 23.32ms | 21.01ms | 0.901× | 2.57% | PASS |
| file_reads | `select_random_ranges` | 10.48ms | 7.16ms | 0.684× | 1.81% | PASS |
| file_reads | `covering_index_scan` | 8.73ms | 4.71ms | 0.539× | 3.69% | PASS |
| file_reads | `groupby_scan` | 21.85ms | 22.03ms | 1.008× | 1.58% | PASS |
| file_reads | `index_join` | 8.43ms | 8.20ms | 0.973× | 3.55% | PASS |
| file_reads | `index_join_scan` | 3.62ms | 4.39ms | 1.215× | 2.52% | PASS |
| file_reads | `types_table_scan` | 736.30ms | 771.76ms | 1.048× | 3.80% | PASS |
| file_reads | `table_scan` | 830.42ms | 848.69ms | 1.022× | 1.72% | PASS |
| file_reads | `oltp_read_only` | 166.66ms | 115.16ms | 0.691× | 2.42% | PASS |
| file_writes | `oltp_bulk_insert` | 199.72ms | 259.13ms | 1.297× | 4.66% | PASS |
| file_writes | `oltp_insert` | 19.53ms | 38.80ms | 1.987× | 4.49% | PASS |
| file_writes | `oltp_update_index` | 144.12ms | 149.61ms | 1.038× | 4.27% | PASS |
| file_writes | `oltp_update_non_index` | 124.55ms | 109.73ms | 0.881× | 4.61% | PASS |
| file_writes | `oltp_delete_insert` | 120.96ms | 120.04ms | 0.992× | 3.40% | PASS |
| file_writes | `oltp_write_only` | 90.36ms | 92.80ms | 1.027× | 5.21% | PASS |
| file_writes | `types_delete_insert` | 76.30ms | 72.04ms | 0.944× | 5.35% | PASS |
| file_writes | `oltp_read_write` | 130.41ms | 140.02ms | 1.074× | 6.87% | PASS |
| ac_reads | `oltp_point_select` | 39.90ms | 43.50ms | 1.090× | 1.18% | PASS |
| ac_reads | `oltp_range_select` | 13.27ms | 13.51ms | 1.018× | 3.84% | PASS |
| ac_reads | `oltp_sum_range` | 13.10ms | 13.57ms | 1.036× | 2.22% | PASS |
| ac_reads | `oltp_order_range` | 2.62ms | 2.46ms | 0.941× | 2.71% | PASS |
| ac_reads | `oltp_distinct_range` | 3.17ms | 3.01ms | 0.948× | 1.48% | PASS |
| ac_reads | `oltp_index_scan` | 5.26ms | 6.22ms | 1.182× | 2.33% | PASS |
| ac_reads | `select_random_points` | 19.75ms | 21.38ms | 1.082× | 3.16% | PASS |
| ac_reads | `select_random_ranges` | 6.73ms | 7.17ms | 1.065× | 1.82% | PASS |
| ac_reads | `covering_index_scan` | 4.73ms | 5.07ms | 1.073× | 3.38% | PASS |
| ac_reads | `groupby_scan` | 20.75ms | 21.41ms | 1.032× | 1.81% | PASS |
| ac_reads | `index_join` | 6.66ms | 8.45ms | 1.269× | 2.63% | PASS |
| ac_reads | `index_join_scan` | 3.24ms | 4.36ms | 1.347× | 2.47% | PASS |
| ac_reads | `types_table_scan` | 727.83ms | 757.23ms | 1.040× | 4.51% | PASS |
| ac_reads | `table_scan` | 904.82ms | 856.07ms | 0.946× | 3.52% | PASS |
| ac_reads | `oltp_read_only` | 120.01ms | 119.99ms | 1.000× | 2.65% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.29ms | 58.62ms | 2.630× | 6.61% | PASS |
| ac_writes | `oltp_insert_ac` | 26.31ms | 74.94ms | 2.848× | 4.79% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.32ms | 84.81ms | 2.994× | 6.15% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.05ms | 66.09ms | 2.997× | 3.94% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.65ms | 75.07ms | 2.927× | 4.72% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.78ms | 75.38ms | 2.924× | 4.84% | PASS |
| ac_writes | `types_delete_insert_ac` | 26.58ms | 65.83ms | 2.477× | 8.40% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.44ms | 81.05ms | 2.850× | 3.25% | PASS |

</details>

## Version-control latency

Wall time: 1m 45s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 57.93ms | 200.00ms | 29.0% | 1.42% | PASS |
| `status_dirty_many_tables` | 58.98ms | 200.00ms | 29.5% | 1.21% | PASS |
| `diff_regular_working_one_table` | 53.90ms | 150.00ms | 35.9% | 0.89% | PASS |
| `diff_regular_working_many_tables` | 61.20ms | 200.00ms | 30.6% | 1.47% | PASS |
| `diff_stat_working_many_tables` | 59.78ms | 200.00ms | 29.9% | 1.68% | PASS |
| `diff_schema_working_many_tables` | 61.66ms | 200.00ms | 30.8% | 0.99% | PASS |
| `branch_list_many_branches` | 17.93ms | 100.00ms | 17.9% | 2.19% | PASS |
| `branch_create_delete` | 19.43ms | 100.00ms | 19.4% | 3.01% | PASS |
| `checkout_branch_clean` | 78.47ms | 200.00ms | 39.2% | 1.13% | PASS |
| `merge_data_no_conflicts` | 30.41ms | 150.00ms | 20.3% | 5.69% | PASS |
| `merge_schema_no_conflicts` | 16.95ms | 100.00ms | 17.0% | 2.18% | PASS |
| `merge_data_conflicts` | 76.27ms | 250.00ms | 30.5% | 1.57% | PASS |
| `merge_data_conflicts_with_resolve` | 74.81ms | 250.00ms | 29.9% | 1.37% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
