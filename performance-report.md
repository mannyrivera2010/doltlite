# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-10 12:20 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260720.247.2
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/31380452234)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 36s | 9.63s | 11.06s | 1.149× | 1.22% | **PASS** |
| textpk | 69 | 55 | 1h 32m 47s | 10.79s | 11.75s | 1.089× | 1.28% | **PASS** |
| blobpk | 69 | 55 | 1h 32m 31s | 10.45s | 11.82s | 1.131× | 1.64% | **PASS** |
| compositepk | 69 | 55 | 1h 14m 3s | 8.57s | 10.21s | 1.192× | 0.99% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.61ms | 27.51ms | 1.118× | 1.17% | PASS |
| mem_reads | `oltp_range_select` | 10.67ms | 11.94ms | 1.119× | 1.11% | PASS |
| mem_reads | `oltp_sum_range` | 9.71ms | 11.42ms | 1.176× | 1.56% | PASS |
| mem_reads | `oltp_order_range` | 2.68ms | 2.90ms | 1.082× | 1.35% | PASS |
| mem_reads | `oltp_distinct_range` | 3.74ms | 3.89ms | 1.041× | 0.78% | PASS |
| mem_reads | `oltp_index_scan` | 3.97ms | 4.94ms | 1.245× | 1.26% | PASS |
| mem_reads | `select_random_points` | 10.49ms | 11.25ms | 1.072× | 1.30% | PASS |
| mem_reads | `select_random_ranges` | 3.06ms | 3.99ms | 1.302× | 1.23% | PASS |
| mem_reads | `covering_index_scan` | 4.35ms | 4.09ms | 0.940× | 1.16% | PASS |
| mem_reads | `groupby_scan` | 31.70ms | 34.45ms | 1.087× | 0.74% | PASS |
| mem_reads | `index_join` | 5.95ms | 7.78ms | 1.307× | 1.31% | PASS |
| mem_reads | `index_join_scan` | 3.49ms | 4.66ms | 1.335× | 0.95% | PASS |
| mem_reads | `types_table_scan` | 1.14s | 1.26s | 1.103× | 0.51% | PASS |
| mem_reads | `table_scan` | 1.31s | 1.37s | 1.048× | 0.60% | PASS |
| mem_reads | `oltp_read_only` | 106.01ms | 115.75ms | 1.092× | 1.22% | PASS |
| mem_writes | `oltp_bulk_insert` | 183.06ms | 241.79ms | 1.321× | 1.12% | PASS |
| mem_writes | `oltp_insert` | 15.88ms | 28.34ms | 1.784× | 0.94% | PASS |
| mem_writes | `oltp_update_index` | 51.37ms | 106.03ms | 2.064× | 1.01% | PASS |
| mem_writes | `oltp_update_non_index` | 35.49ms | 59.35ms | 1.672× | 1.24% | PASS |
| mem_writes | `oltp_delete_insert` | 45.70ms | 79.67ms | 1.744× | 0.92% | PASS |
| mem_writes | `oltp_write_only` | 22.03ms | 45.37ms | 2.059× | 1.22% | PASS |
| mem_writes | `types_delete_insert` | 24.54ms | 39.80ms | 1.622× | 0.81% | PASS |
| mem_writes | `oltp_read_write` | 65.22ms | 104.32ms | 1.599× | 1.19% | PASS |
| file_reads | `oltp_point_select` | 119.95ms | 58.81ms | 0.490× | 0.80% | PASS |
| file_reads | `oltp_range_select` | 21.20ms | 15.31ms | 0.722× | 0.98% | PASS |
| file_reads | `oltp_sum_range` | 19.16ms | 14.76ms | 0.770× | 1.75% | PASS |
| file_reads | `oltp_order_range` | 3.61ms | 3.26ms | 0.902× | 1.35% | PASS |
| file_reads | `oltp_distinct_range` | 4.67ms | 4.23ms | 0.905× | 1.23% | PASS |
| file_reads | `oltp_index_scan` | 13.69ms | 8.52ms | 0.622× | 1.31% | PASS |
| file_reads | `select_random_points` | 21.33ms | 14.89ms | 0.698× | 1.23% | PASS |
| file_reads | `select_random_ranges` | 12.71ms | 7.18ms | 0.565× | 1.43% | PASS |
| file_reads | `covering_index_scan` | 13.94ms | 7.63ms | 0.548× | 1.04% | PASS |
| file_reads | `groupby_scan` | 32.72ms | 34.89ms | 1.066× | 0.65% | PASS |
| file_reads | `index_join` | 11.06ms | 9.98ms | 0.902× | 1.20% | PASS |
| file_reads | `index_join_scan` | 4.57ms | 5.07ms | 1.108× | 1.34% | PASS |
| file_reads | `types_table_scan` | 1.16s | 1.27s | 1.094× | 1.48% | PASS |
| file_reads | `table_scan` | 1.33s | 1.38s | 1.041× | 1.55% | PASS |
| file_reads | `oltp_read_only` | 241.16ms | 160.82ms | 0.667× | 2.10% | PASS |
| file_writes | `oltp_bulk_insert` | 197.01ms | 261.83ms | 1.329× | 1.05% | PASS |
| file_writes | `oltp_insert` | 22.24ms | 36.22ms | 1.628× | 1.42% | PASS |
| file_writes | `oltp_update_index` | 80.80ms | 132.31ms | 1.637× | 1.68% | PASS |
| file_writes | `oltp_update_non_index` | 60.80ms | 83.41ms | 1.372× | 1.00% | PASS |
| file_writes | `oltp_delete_insert` | 70.03ms | 100.84ms | 1.440× | 1.12% | PASS |
| file_writes | `oltp_write_only` | 45.14ms | 67.44ms | 1.494× | 1.48% | PASS |
| file_writes | `types_delete_insert` | 41.47ms | 54.20ms | 1.307× | 1.38% | PASS |
| file_writes | `oltp_read_write` | 88.69ms | 125.69ms | 1.417× | 1.42% | PASS |
| ac_reads | `oltp_point_select` | 55.62ms | 58.95ms | 1.060× | 1.38% | PASS |
| ac_reads | `oltp_range_select` | 14.36ms | 15.11ms | 1.052× | 1.06% | PASS |
| ac_reads | `oltp_sum_range` | 13.20ms | 14.71ms | 1.115× | 1.25% | PASS |
| ac_reads | `oltp_order_range` | 3.15ms | 3.25ms | 1.031× | 1.01% | PASS |
| ac_reads | `oltp_distinct_range` | 4.14ms | 4.23ms | 1.022× | 0.81% | PASS |
| ac_reads | `oltp_index_scan` | 7.41ms | 8.44ms | 1.139× | 1.21% | PASS |
| ac_reads | `select_random_points` | 14.00ms | 14.47ms | 1.034× | 0.95% | PASS |
| ac_reads | `select_random_ranges` | 6.33ms | 7.17ms | 1.133× | 1.08% | PASS |
| ac_reads | `covering_index_scan` | 7.81ms | 7.62ms | 0.975× | 1.38% | PASS |
| ac_reads | `groupby_scan` | 32.23ms | 34.92ms | 1.084× | 1.01% | PASS |
| ac_reads | `index_join` | 8.00ms | 10.02ms | 1.252× | 0.99% | PASS |
| ac_reads | `index_join_scan` | 4.06ms | 5.07ms | 1.249× | 1.00% | PASS |
| ac_reads | `types_table_scan` | 1.14s | 1.26s | 1.104× | 0.70% | PASS |
| ac_reads | `table_scan` | 1.29s | 1.37s | 1.062× | 0.61% | PASS |
| ac_reads | `oltp_read_only` | 148.72ms | 161.18ms | 1.084× | 1.34% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.30ms | 62.21ms | 4.065× | 4.33% | PASS |
| ac_writes | `oltp_insert_ac` | 18.07ms | 78.82ms | 4.361× | 5.75% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.29ms | 95.72ms | 4.962× | 4.18% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 15.65ms | 70.25ms | 4.490× | 5.16% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.73ms | 85.28ms | 4.811× | 3.75% | PASS |
| ac_writes | `oltp_write_only_ac` | 17.24ms | 82.21ms | 4.770× | 2.68% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.29ms | 73.26ms | 4.790× | 6.26% | PASS |
| ac_writes | `oltp_read_write_ac` | 23.18ms | 90.22ms | 3.892× | 4.06% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.21ms | 34.84ms | 1.153× | 1.24% | PASS |
| mem_reads | `oltp_range_select` | 13.83ms | 13.55ms | 0.980× | 0.83% | PASS |
| mem_reads | `oltp_sum_range` | 12.24ms | 13.20ms | 1.078× | 0.97% | PASS |
| mem_reads | `oltp_order_range` | 3.17ms | 3.18ms | 1.002× | 0.98% | PASS |
| mem_reads | `oltp_distinct_range` | 4.26ms | 4.17ms | 0.978× | 0.81% | PASS |
| mem_reads | `oltp_index_scan` | 4.63ms | 5.85ms | 1.265× | 1.14% | PASS |
| mem_reads | `select_random_points` | 17.79ms | 20.00ms | 1.124× | 0.96% | PASS |
| mem_reads | `select_random_ranges` | 4.10ms | 5.21ms | 1.269× | 1.04% | PASS |
| mem_reads | `covering_index_scan` | 4.83ms | 4.37ms | 0.904× | 1.10% | PASS |
| mem_reads | `groupby_scan` | 34.09ms | 35.23ms | 1.034× | 0.66% | PASS |
| mem_reads | `index_join` | 7.04ms | 8.72ms | 1.239× | 0.68% | PASS |
| mem_reads | `index_join_scan` | 4.79ms | 5.52ms | 1.152× | 0.65% | PASS |
| mem_reads | `types_table_scan` | 1.19s | 1.26s | 1.058× | 2.88% | PASS |
| mem_reads | `table_scan` | 1.72s | 1.43s | 0.832× | 0.87% | PASS |
| mem_reads | `oltp_read_only` | 134.05ms | 133.50ms | 0.996× | 3.60% | PASS |
| mem_writes | `oltp_bulk_insert` | 232.90ms | 337.20ms | 1.448× | 1.27% | PASS |
| mem_writes | `oltp_insert` | 22.57ms | 39.57ms | 1.753× | 0.91% | PASS |
| mem_writes | `oltp_update_index` | 72.84ms | 136.34ms | 1.872× | 0.88% | PASS |
| mem_writes | `oltp_update_non_index` | 50.37ms | 88.18ms | 1.751× | 0.57% | PASS |
| mem_writes | `oltp_delete_insert` | 53.02ms | 106.93ms | 2.017× | 0.70% | PASS |
| mem_writes | `oltp_write_only` | 30.26ms | 64.20ms | 2.121× | 0.73% | PASS |
| mem_writes | `types_delete_insert` | 34.24ms | 55.57ms | 1.623× | 1.33% | PASS |
| mem_writes | `oltp_read_write` | 85.72ms | 135.61ms | 1.582× | 1.46% | PASS |
| file_reads | `oltp_point_select` | 126.62ms | 66.71ms | 0.527× | 1.01% | PASS |
| file_reads | `oltp_range_select` | 24.28ms | 16.90ms | 0.696× | 1.53% | PASS |
| file_reads | `oltp_sum_range` | 22.60ms | 16.59ms | 0.734× | 0.82% | PASS |
| file_reads | `oltp_order_range` | 4.26ms | 3.55ms | 0.832× | 2.14% | PASS |
| file_reads | `oltp_distinct_range` | 5.33ms | 4.55ms | 0.853× | 1.25% | PASS |
| file_reads | `oltp_index_scan` | 14.68ms | 9.42ms | 0.641× | 1.62% | PASS |
| file_reads | `select_random_points` | 28.49ms | 23.51ms | 0.825× | 1.03% | PASS |
| file_reads | `select_random_ranges` | 13.99ms | 8.55ms | 0.611× | 1.01% | PASS |
| file_reads | `covering_index_scan` | 15.86ms | 8.05ms | 0.508× | 1.88% | PASS |
| file_reads | `groupby_scan` | 35.38ms | 35.74ms | 1.010× | 0.56% | PASS |
| file_reads | `index_join` | 13.02ms | 11.33ms | 0.870× | 1.65% | PASS |
| file_reads | `index_join_scan` | 5.94ms | 6.12ms | 1.030× | 1.58% | PASS |
| file_reads | `types_table_scan` | 1.15s | 1.24s | 1.081× | 0.45% | PASS |
| file_reads | `table_scan` | 1.35s | 1.35s | 1.004× | 0.32% | PASS |
| file_reads | `oltp_read_only` | 259.86ms | 174.80ms | 0.673× | 0.50% | PASS |
| file_writes | `oltp_bulk_insert` | 254.18ms | 368.51ms | 1.450× | 0.95% | PASS |
| file_writes | `oltp_insert` | 62.55ms | 52.60ms | 0.841× | 17.16% | PASS |
| file_writes | `oltp_update_index` | 117.18ms | 173.72ms | 1.483× | 1.48% | PASS |
| file_writes | `oltp_update_non_index` | 115.71ms | 116.01ms | 1.003× | 15.56% | PASS |
| file_writes | `oltp_delete_insert` | 93.48ms | 139.60ms | 1.493× | 1.54% | PASS |
| file_writes | `oltp_write_only` | 85.55ms | 90.08ms | 1.053× | 10.25% | PASS |
| file_writes | `types_delete_insert` | 58.02ms | 76.01ms | 1.310× | 2.53% | PASS |
| file_writes | `oltp_read_write` | 141.35ms | 164.60ms | 1.164× | 8.19% | PASS |
| ac_reads | `oltp_point_select` | 62.27ms | 67.58ms | 1.085× | 2.14% | PASS |
| ac_reads | `oltp_range_select` | 18.41ms | 17.03ms | 0.925× | 2.32% | PASS |
| ac_reads | `oltp_sum_range` | 16.52ms | 16.79ms | 1.016× | 2.05% | PASS |
| ac_reads | `oltp_order_range` | 3.79ms | 3.58ms | 0.945× | 1.39% | PASS |
| ac_reads | `oltp_distinct_range` | 4.74ms | 4.56ms | 0.961× | 1.28% | PASS |
| ac_reads | `oltp_index_scan` | 8.39ms | 9.48ms | 1.130× | 1.72% | PASS |
| ac_reads | `select_random_points` | 22.06ms | 23.48ms | 1.064× | 1.09% | PASS |
| ac_reads | `select_random_ranges` | 7.67ms | 8.57ms | 1.118× | 0.85% | PASS |
| ac_reads | `covering_index_scan` | 9.45ms | 8.10ms | 0.857× | 2.11% | PASS |
| ac_reads | `groupby_scan` | 35.01ms | 35.83ms | 1.023× | 2.12% | PASS |
| ac_reads | `index_join` | 9.89ms | 11.43ms | 1.155× | 1.99% | PASS |
| ac_reads | `index_join_scan` | 5.50ms | 6.20ms | 1.128× | 1.54% | PASS |
| ac_reads | `types_table_scan` | 1.16s | 1.25s | 1.076× | 0.98% | PASS |
| ac_reads | `table_scan` | 1.35s | 1.35s | 1.002× | 0.81% | PASS |
| ac_reads | `oltp_read_only` | 168.02ms | 175.12ms | 1.042× | 0.80% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.36ms | 64.78ms | 3.961× | 5.06% | PASS |
| ac_writes | `oltp_insert_ac` | 19.19ms | 81.08ms | 4.226× | 4.47% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.17ms | 100.58ms | 4.750× | 3.66% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.70ms | 76.17ms | 4.561× | 4.63% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.01ms | 86.93ms | 4.827× | 3.02% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.83ms | 85.73ms | 4.553× | 3.46% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.46ms | 74.83ms | 4.839× | 4.01% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.06ms | 94.01ms | 3.751× | 3.94% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.85ms | 36.02ms | 1.096× | 2.16% | PASS |
| mem_reads | `oltp_range_select` | 15.37ms | 14.28ms | 0.929× | 2.74% | PASS |
| mem_reads | `oltp_sum_range` | 13.76ms | 14.02ms | 1.019× | 1.38% | PASS |
| mem_reads | `oltp_order_range` | 3.38ms | 3.26ms | 0.963× | 1.16% | PASS |
| mem_reads | `oltp_distinct_range` | 4.35ms | 4.23ms | 0.971× | 0.97% | PASS |
| mem_reads | `oltp_index_scan` | 4.67ms | 5.93ms | 1.269× | 1.80% | PASS |
| mem_reads | `select_random_points` | 17.93ms | 20.10ms | 1.121× | 1.60% | PASS |
| mem_reads | `select_random_ranges` | 4.36ms | 5.36ms | 1.230× | 1.64% | PASS |
| mem_reads | `covering_index_scan` | 4.62ms | 4.69ms | 1.015× | 3.18% | PASS |
| mem_reads | `groupby_scan` | 34.18ms | 35.43ms | 1.036× | 0.91% | PASS |
| mem_reads | `index_join` | 7.14ms | 9.89ms | 1.385× | 2.65% | PASS |
| mem_reads | `index_join_scan` | 4.58ms | 5.93ms | 1.294× | 2.57% | PASS |
| mem_reads | `types_table_scan` | 1.18s | 1.27s | 1.079× | 3.76% | PASS |
| mem_reads | `table_scan` | 1.52s | 1.40s | 0.916× | 5.40% | PASS |
| mem_reads | `oltp_read_only` | 131.15ms | 133.73ms | 1.020× | 2.54% | PASS |
| mem_writes | `oltp_bulk_insert` | 237.97ms | 338.88ms | 1.424× | 1.10% | PASS |
| mem_writes | `oltp_insert` | 21.42ms | 40.84ms | 1.906× | 1.18% | PASS |
| mem_writes | `oltp_update_index` | 82.96ms | 149.68ms | 1.804× | 2.86% | PASS |
| mem_writes | `oltp_update_non_index` | 55.80ms | 92.72ms | 1.662× | 1.18% | PASS |
| mem_writes | `oltp_delete_insert` | 56.62ms | 113.19ms | 1.999× | 2.01% | PASS |
| mem_writes | `oltp_write_only` | 31.39ms | 67.63ms | 2.155× | 2.63% | PASS |
| mem_writes | `types_delete_insert` | 34.55ms | 56.48ms | 1.634× | 1.27% | PASS |
| mem_writes | `oltp_read_write` | 95.42ms | 143.72ms | 1.506× | 2.86% | PASS |
| file_reads | `oltp_point_select` | 130.33ms | 69.05ms | 0.530× | 0.76% | PASS |
| file_reads | `oltp_range_select` | 25.37ms | 17.28ms | 0.681× | 1.94% | PASS |
| file_reads | `oltp_sum_range` | 23.44ms | 16.93ms | 0.722× | 1.69% | PASS |
| file_reads | `oltp_order_range` | 4.38ms | 3.60ms | 0.822× | 1.07% | PASS |
| file_reads | `oltp_distinct_range` | 5.46ms | 4.59ms | 0.842× | 1.20% | PASS |
| file_reads | `oltp_index_scan` | 15.12ms | 9.73ms | 0.643× | 1.01% | PASS |
| file_reads | `select_random_points` | 31.25ms | 25.52ms | 0.817× | 2.65% | PASS |
| file_reads | `select_random_ranges` | 14.17ms | 8.63ms | 0.609× | 0.76% | PASS |
| file_reads | `covering_index_scan` | 15.61ms | 8.17ms | 0.523× | 0.92% | PASS |
| file_reads | `groupby_scan` | 35.76ms | 36.06ms | 1.008× | 0.74% | PASS |
| file_reads | `index_join` | 12.68ms | 11.40ms | 0.900× | 2.25% | PASS |
| file_reads | `index_join_scan` | 5.60ms | 6.27ms | 1.119× | 2.52% | PASS |
| file_reads | `types_table_scan` | 1.15s | 1.24s | 1.081× | 1.33% | PASS |
| file_reads | `table_scan` | 1.33s | 1.35s | 1.018× | 1.50% | PASS |
| file_reads | `oltp_read_only` | 258.43ms | 173.42ms | 0.671× | 0.93% | PASS |
| file_writes | `oltp_bulk_insert` | 261.58ms | 360.62ms | 1.379× | 0.83% | PASS |
| file_writes | `oltp_insert` | 33.53ms | 53.78ms | 1.604× | 2.30% | PASS |
| file_writes | `oltp_update_index` | 115.76ms | 177.96ms | 1.537× | 1.69% | PASS |
| file_writes | `oltp_update_non_index` | 87.57ms | 116.81ms | 1.334× | 1.23% | PASS |
| file_writes | `oltp_delete_insert` | 90.47ms | 142.22ms | 1.572× | 2.16% | PASS |
| file_writes | `oltp_write_only` | 59.24ms | 91.11ms | 1.538× | 2.07% | PASS |
| file_writes | `types_delete_insert` | 55.38ms | 75.21ms | 1.358× | 1.49% | PASS |
| file_writes | `oltp_read_write` | 126.21ms | 169.39ms | 1.342× | 1.27% | PASS |
| ac_reads | `oltp_point_select` | 64.38ms | 68.20ms | 1.059× | 1.04% | PASS |
| ac_reads | `oltp_range_select` | 18.68ms | 17.34ms | 0.928× | 2.74% | PASS |
| ac_reads | `oltp_sum_range` | 17.37ms | 17.22ms | 0.991× | 2.12% | PASS |
| ac_reads | `oltp_order_range` | 3.80ms | 3.58ms | 0.943× | 1.30% | PASS |
| ac_reads | `oltp_distinct_range` | 4.79ms | 4.56ms | 0.952× | 1.10% | PASS |
| ac_reads | `oltp_index_scan` | 8.56ms | 9.64ms | 1.126× | 1.03% | PASS |
| ac_reads | `select_random_points` | 23.15ms | 24.25ms | 1.048× | 2.81% | PASS |
| ac_reads | `select_random_ranges` | 7.67ms | 8.56ms | 1.116× | 1.71% | PASS |
| ac_reads | `covering_index_scan` | 8.54ms | 7.97ms | 0.933× | 1.37% | PASS |
| ac_reads | `groupby_scan` | 34.35ms | 35.72ms | 1.040× | 0.71% | PASS |
| ac_reads | `index_join` | 9.26ms | 11.34ms | 1.225× | 1.41% | PASS |
| ac_reads | `index_join_scan` | 5.01ms | 6.12ms | 1.223× | 1.32% | PASS |
| ac_reads | `types_table_scan` | 1.13s | 1.24s | 1.094× | 0.71% | PASS |
| ac_reads | `table_scan` | 1.30s | 1.35s | 1.037× | 0.64% | PASS |
| ac_reads | `oltp_read_only` | 169.28ms | 174.76ms | 1.032× | 0.85% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.35ms | 67.43ms | 3.887× | 4.34% | PASS |
| ac_writes | `oltp_insert_ac` | 20.31ms | 90.36ms | 4.449× | 4.00% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.42ms | 101.27ms | 4.727× | 3.18% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.40ms | 78.04ms | 4.484× | 3.68% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.99ms | 92.42ms | 4.867× | 4.35% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.71ms | 91.38ms | 4.637× | 5.07% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.82ms | 82.61ms | 4.637× | 4.85% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.73ms | 101.66ms | 3.665× | 5.33% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 28.68ms | 33.89ms | 1.182× | 0.95% | PASS |
| mem_reads | `oltp_range_select` | 16.46ms | 17.60ms | 1.070× | 0.68% | PASS |
| mem_reads | `oltp_sum_range` | 15.62ms | 17.13ms | 1.096× | 0.55% | PASS |
| mem_reads | `oltp_order_range` | 3.12ms | 3.33ms | 1.069× | 0.68% | PASS |
| mem_reads | `oltp_distinct_range` | 3.90ms | 4.19ms | 1.073× | 0.59% | PASS |
| mem_reads | `oltp_index_scan` | 4.03ms | 5.20ms | 1.290× | 1.36% | PASS |
| mem_reads | `select_random_points` | 24.90ms | 28.95ms | 1.163× | 0.94% | PASS |
| mem_reads | `select_random_ranges` | 6.58ms | 7.89ms | 1.198× | 0.75% | PASS |
| mem_reads | `covering_index_scan` | 3.65ms | 3.60ms | 0.987× | 1.08% | PASS |
| mem_reads | `groupby_scan` | 32.73ms | 33.68ms | 1.029× | 0.46% | PASS |
| mem_reads | `index_join` | 7.08ms | 9.35ms | 1.321× | 0.92% | PASS |
| mem_reads | `index_join_scan` | 3.41ms | 4.97ms | 1.460× | 0.87% | PASS |
| mem_reads | `types_table_scan` | 953.64ms | 1.08s | 1.137× | 0.58% | PASS |
| mem_reads | `table_scan` | 1.10s | 1.19s | 1.080× | 0.89% | PASS |
| mem_reads | `oltp_read_only` | 126.75ms | 141.88ms | 1.119× | 0.84% | PASS |
| mem_writes | `oltp_bulk_insert` | 214.17ms | 296.86ms | 1.386× | 1.27% | PASS |
| mem_writes | `oltp_insert` | 16.70ms | 30.39ms | 1.820× | 0.59% | PASS |
| mem_writes | `oltp_update_index` | 59.04ms | 100.09ms | 1.695× | 1.04% | PASS |
| mem_writes | `oltp_update_non_index` | 42.89ms | 68.73ms | 1.603× | 1.01% | PASS |
| mem_writes | `oltp_delete_insert` | 43.03ms | 80.44ms | 1.869× | 0.80% | PASS |
| mem_writes | `oltp_write_only` | 22.80ms | 47.72ms | 2.093× | 0.71% | PASS |
| mem_writes | `types_delete_insert` | 27.51ms | 45.83ms | 1.666× | 0.87% | PASS |
| mem_writes | `oltp_read_write` | 86.25ms | 129.30ms | 1.499× | 1.20% | PASS |
| file_reads | `oltp_point_select` | 61.62ms | 45.97ms | 0.746× | 1.04% | PASS |
| file_reads | `oltp_range_select` | 19.80ms | 19.17ms | 0.968× | 0.63% | PASS |
| file_reads | `oltp_sum_range` | 19.03ms | 18.69ms | 0.982× | 0.91% | PASS |
| file_reads | `oltp_order_range` | 3.48ms | 3.55ms | 1.020× | 0.99% | PASS |
| file_reads | `oltp_distinct_range` | 4.32ms | 4.41ms | 1.020× | 1.01% | PASS |
| file_reads | `oltp_index_scan` | 7.32ms | 6.81ms | 0.931× | 1.42% | PASS |
| file_reads | `select_random_points` | 29.07ms | 31.46ms | 1.082× | 1.25% | PASS |
| file_reads | `select_random_ranges` | 10.26ms | 9.44ms | 0.920× | 1.24% | PASS |
| file_reads | `covering_index_scan` | 6.90ms | 5.15ms | 0.747× | 0.91% | PASS |
| file_reads | `groupby_scan` | 33.26ms | 34.02ms | 1.023× | 0.58% | PASS |
| file_reads | `index_join` | 9.07ms | 10.68ms | 1.177× | 0.99% | PASS |
| file_reads | `index_join_scan` | 3.86ms | 5.25ms | 1.360× | 1.43% | PASS |
| file_reads | `types_table_scan` | 951.83ms | 1.09s | 1.147× | 0.74% | PASS |
| file_reads | `table_scan` | 1.10s | 1.19s | 1.080× | 0.91% | PASS |
| file_reads | `oltp_read_only` | 177.85ms | 161.59ms | 0.909× | 0.65% | PASS |
| file_writes | `oltp_bulk_insert` | 226.64ms | 312.74ms | 1.380× | 0.98% | PASS |
| file_writes | `oltp_insert` | 21.09ms | 37.62ms | 1.784× | 1.25% | PASS |
| file_writes | `oltp_update_index` | 75.21ms | 117.81ms | 1.566× | 1.49% | PASS |
| file_writes | `oltp_update_non_index` | 57.31ms | 83.49ms | 1.457× | 1.13% | PASS |
| file_writes | `oltp_delete_insert` | 57.72ms | 96.23ms | 1.667× | 1.05% | PASS |
| file_writes | `oltp_write_only` | 35.52ms | 61.15ms | 1.721× | 1.48% | PASS |
| file_writes | `types_delete_insert` | 37.18ms | 55.59ms | 1.495× | 1.45% | PASS |
| file_writes | `oltp_read_write` | 98.32ms | 141.64ms | 1.441× | 1.17% | PASS |
| ac_reads | `oltp_point_select` | 38.89ms | 45.85ms | 1.179× | 1.38% | PASS |
| ac_reads | `oltp_range_select` | 17.50ms | 19.12ms | 1.093× | 0.85% | PASS |
| ac_reads | `oltp_sum_range` | 16.75ms | 18.67ms | 1.114× | 1.03% | PASS |
| ac_reads | `oltp_order_range` | 3.29ms | 3.54ms | 1.078× | 1.25% | PASS |
| ac_reads | `oltp_distinct_range` | 4.05ms | 4.41ms | 1.087× | 0.91% | PASS |
| ac_reads | `oltp_index_scan` | 5.15ms | 6.75ms | 1.310× | 1.56% | PASS |
| ac_reads | `select_random_points` | 26.24ms | 30.87ms | 1.177× | 1.00% | PASS |
| ac_reads | `select_random_ranges` | 7.81ms | 9.38ms | 1.200× | 0.93% | PASS |
| ac_reads | `covering_index_scan` | 4.83ms | 5.15ms | 1.066× | 0.78% | PASS |
| ac_reads | `groupby_scan` | 33.24ms | 33.96ms | 1.022× | 0.50% | PASS |
| ac_reads | `index_join` | 7.89ms | 10.71ms | 1.357× | 1.24% | PASS |
| ac_reads | `index_join_scan` | 3.66ms | 5.21ms | 1.423× | 0.86% | PASS |
| ac_reads | `types_table_scan` | 980.81ms | 1.10s | 1.123× | 1.20% | PASS |
| ac_reads | `table_scan` | 1.24s | 1.23s | 0.997× | 0.77% | PASS |
| ac_reads | `oltp_read_only` | 145.63ms | 162.39ms | 1.115× | 0.98% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.87ms | 57.97ms | 3.653× | 5.12% | PASS |
| ac_writes | `oltp_insert_ac` | 17.91ms | 73.98ms | 4.131× | 4.39% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.65ms | 89.05ms | 4.312× | 6.53% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.14ms | 68.58ms | 4.250× | 8.42% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.29ms | 76.40ms | 4.418× | 4.93% | PASS |
| ac_writes | `oltp_write_only_ac` | 17.20ms | 75.67ms | 4.401× | 5.61% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.56ms | 67.46ms | 4.334× | 6.17% | PASS |
| ac_writes | `oltp_read_write_ac` | 22.37ms | 86.75ms | 3.878× | 6.51% | PASS |

</details>

## Version-control latency

Wall time: 1m 47s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 55.95ms | 200.00ms | 28.0% | 0.70% | PASS |
| `status_dirty_many_tables` | 57.76ms | 200.00ms | 28.9% | 0.74% | PASS |
| `diff_regular_working_one_table` | 52.99ms | 150.00ms | 35.3% | 0.86% | PASS |
| `diff_regular_working_many_tables` | 60.27ms | 200.00ms | 30.1% | 0.70% | PASS |
| `diff_stat_working_many_tables` | 60.57ms | 200.00ms | 30.3% | 0.88% | PASS |
| `diff_schema_working_many_tables` | 60.95ms | 200.00ms | 30.5% | 0.89% | PASS |
| `branch_list_many_branches` | 16.25ms | 100.00ms | 16.2% | 0.88% | PASS |
| `branch_create_delete` | 23.88ms | 100.00ms | 23.9% | 23.52% | PASS |
| `checkout_branch_clean` | 83.83ms | 200.00ms | 41.9% | 10.97% | PASS |
| `merge_data_no_conflicts` | 30.18ms | 150.00ms | 20.1% | 9.92% | PASS |
| `merge_schema_no_conflicts` | 16.54ms | 100.00ms | 16.5% | 2.68% | PASS |
| `merge_data_conflicts` | 76.08ms | 250.00ms | 30.4% | 1.00% | PASS |
| `merge_data_conflicts_with_resolve` | 75.80ms | 250.00ms | 30.3% | 0.64% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
