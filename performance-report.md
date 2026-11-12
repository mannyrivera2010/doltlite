# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-12 19:17 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu26 20261101.186.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/43272171022)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 55m 46s | 6.54s | 7.48s | 1.143× | 2.46% | **PASS** |
| textpk | 69 | 55 | 1h 14m 50s | 8.26s | 8.40s | 1.018× | 3.78% | **PASS** |
| blobpk | 69 | 55 | 1h 32m 33s | 10.79s | 11.79s | 1.093× | 1.12% | **PASS** |
| compositepk | 69 | 55 | 1h 31m 25s | 11.13s | 12.54s | 1.127× | 1.50% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 18.91ms | 19.41ms | 1.026× | 2.79% | PASS |
| mem_reads | `oltp_range_select` | 6.89ms | 7.54ms | 1.094× | 2.04% | PASS |
| mem_reads | `oltp_sum_range` | 6.36ms | 7.53ms | 1.184× | 2.34% | PASS |
| mem_reads | `oltp_order_range` | 1.77ms | 1.85ms | 1.045× | 3.05% | PASS |
| mem_reads | `oltp_distinct_range` | 2.29ms | 2.38ms | 1.037× | 1.78% | PASS |
| mem_reads | `oltp_index_scan` | 2.76ms | 3.34ms | 1.209× | 4.82% | PASS |
| mem_reads | `select_random_points` | 10.63ms | 9.53ms | 0.896× | 5.37% | PASS |
| mem_reads | `select_random_ranges` | 2.34ms | 2.66ms | 1.136× | 2.25% | PASS |
| mem_reads | `covering_index_scan` | 2.39ms | 2.58ms | 1.080× | 2.74% | PASS |
| mem_reads | `groupby_scan` | 19.49ms | 20.57ms | 1.056× | 1.27% | PASS |
| mem_reads | `index_join` | 3.84ms | 5.37ms | 1.398× | 4.97% | PASS |
| mem_reads | `index_join_scan` | 2.58ms | 3.76ms | 1.457× | 3.56% | PASS |
| mem_reads | `types_table_scan` | 716.37ms | 785.63ms | 1.097× | 1.78% | PASS |
| mem_reads | `table_scan` | 826.35ms | 889.27ms | 1.076× | 2.61% | PASS |
| mem_reads | `oltp_read_only` | 66.37ms | 69.90ms | 1.053× | 1.81% | PASS |
| mem_writes | `oltp_bulk_insert` | 112.42ms | 157.47ms | 1.401× | 1.14% | PASS |
| mem_writes | `oltp_insert` | 9.67ms | 19.10ms | 1.974× | 1.16% | PASS |
| mem_writes | `oltp_update_index` | 36.33ms | 81.10ms | 2.232× | 2.33% | PASS |
| mem_writes | `oltp_update_non_index` | 26.18ms | 45.95ms | 1.755× | 2.25% | PASS |
| mem_writes | `oltp_delete_insert` | 30.85ms | 58.48ms | 1.895× | 2.06% | PASS |
| mem_writes | `oltp_write_only` | 15.46ms | 35.47ms | 2.295× | 1.91% | PASS |
| mem_writes | `types_delete_insert` | 18.54ms | 30.52ms | 1.647× | 2.74% | PASS |
| mem_writes | `oltp_read_write` | 44.91ms | 72.77ms | 1.620× | 3.14% | PASS |
| file_reads | `oltp_point_select` | 75.66ms | 39.84ms | 0.527× | 1.52% | PASS |
| file_reads | `oltp_range_select` | 13.01ms | 9.58ms | 0.736× | 2.34% | PASS |
| file_reads | `oltp_sum_range` | 12.44ms | 9.82ms | 0.789× | 2.32% | PASS |
| file_reads | `oltp_order_range` | 2.48ms | 2.15ms | 0.866× | 1.43% | PASS |
| file_reads | `oltp_distinct_range` | 2.98ms | 2.66ms | 0.893× | 1.64% | PASS |
| file_reads | `oltp_index_scan` | 8.90ms | 6.00ms | 0.674× | 1.40% | PASS |
| file_reads | `select_random_points` | 16.32ms | 11.35ms | 0.696× | 3.28% | PASS |
| file_reads | `select_random_ranges` | 7.94ms | 4.56ms | 0.575× | 1.23% | PASS |
| file_reads | `covering_index_scan` | 8.60ms | 4.57ms | 0.531× | 3.11% | PASS |
| file_reads | `groupby_scan` | 20.20ms | 20.94ms | 1.037× | 1.23% | PASS |
| file_reads | `index_join` | 7.20ms | 6.82ms | 0.946× | 5.79% | PASS |
| file_reads | `index_join_scan` | 3.24ms | 4.02ms | 1.239× | 4.06% | PASS |
| file_reads | `types_table_scan` | 749.21ms | 815.34ms | 1.088× | 1.97% | PASS |
| file_reads | `table_scan` | 831.03ms | 875.44ms | 1.053× | 3.54% | PASS |
| file_reads | `oltp_read_only` | 146.26ms | 97.74ms | 0.668× | 2.29% | PASS |
| file_writes | `oltp_bulk_insert` | 150.25ms | 207.91ms | 1.384× | 1.00% | PASS |
| file_writes | `oltp_insert` | 19.73ms | 27.74ms | 1.406× | 1.34% | PASS |
| file_writes | `oltp_update_index` | 124.08ms | 138.19ms | 1.114× | 4.76% | PASS |
| file_writes | `oltp_update_non_index` | 100.74ms | 94.91ms | 0.942× | 3.94% | PASS |
| file_writes | `oltp_delete_insert` | 104.09ms | 108.07ms | 1.038× | 4.07% | PASS |
| file_writes | `oltp_write_only` | 82.54ms | 78.26ms | 0.948× | 7.30% | PASS |
| file_writes | `types_delete_insert` | 61.34ms | 52.57ms | 0.857× | 4.23% | PASS |
| file_writes | `oltp_read_write` | 110.56ms | 108.55ms | 0.982× | 3.78% | PASS |
| ac_reads | `oltp_point_select` | 35.55ms | 38.32ms | 1.078× | 1.98% | PASS |
| ac_reads | `oltp_range_select` | 8.96ms | 9.45ms | 1.055× | 2.46% | PASS |
| ac_reads | `oltp_sum_range` | 8.37ms | 9.55ms | 1.141× | 2.05% | PASS |
| ac_reads | `oltp_order_range` | 2.01ms | 2.09ms | 1.039× | 2.76% | PASS |
| ac_reads | `oltp_distinct_range` | 2.50ms | 2.60ms | 1.038× | 1.48% | PASS |
| ac_reads | `oltp_index_scan` | 4.53ms | 5.40ms | 1.192× | 2.68% | PASS |
| ac_reads | `select_random_points` | 10.58ms | 10.25ms | 0.969× | 4.00% | PASS |
| ac_reads | `select_random_ranges` | 3.80ms | 4.37ms | 1.150× | 1.28% | PASS |
| ac_reads | `covering_index_scan` | 4.17ms | 4.37ms | 1.049× | 1.60% | PASS |
| ac_reads | `groupby_scan` | 18.78ms | 20.12ms | 1.071× | 1.64% | PASS |
| ac_reads | `index_join` | 4.75ms | 5.85ms | 1.232× | 2.41% | PASS |
| ac_reads | `index_join_scan` | 2.57ms | 3.84ms | 1.496× | 4.83% | PASS |
| ac_reads | `types_table_scan` | 711.60ms | 784.96ms | 1.103× | 1.70% | PASS |
| ac_reads | `table_scan` | 822.38ms | 877.06ms | 1.066× | 2.73% | PASS |
| ac_reads | `oltp_read_only` | 83.54ms | 93.62ms | 1.121× | 1.46% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.44ms | 53.48ms | 2.617× | 4.01% | PASS |
| ac_writes | `oltp_insert_ac` | 22.08ms | 67.09ms | 3.038× | 4.54% | PASS |
| ac_writes | `oltp_update_index_ac` | 23.07ms | 77.33ms | 3.351× | 3.25% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 19.80ms | 59.81ms | 3.021× | 2.50% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 21.98ms | 74.45ms | 3.387× | 8.51% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.05ms | 70.90ms | 3.216× | 4.04% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.10ms | 67.59ms | 3.363× | 5.46% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.55ms | 77.85ms | 3.046× | 5.05% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.11ms | 24.58ms | 1.112× | 2.72% | PASS |
| mem_reads | `oltp_range_select` | 11.18ms | 10.06ms | 0.900× | 3.80% | PASS |
| mem_reads | `oltp_sum_range` | 11.27ms | 10.64ms | 0.944× | 4.35% | PASS |
| mem_reads | `oltp_order_range` | 2.37ms | 2.20ms | 0.928× | 1.42% | PASS |
| mem_reads | `oltp_distinct_range` | 2.86ms | 2.65ms | 0.929× | 1.49% | PASS |
| mem_reads | `oltp_index_scan` | 3.09ms | 4.20ms | 1.362× | 3.58% | PASS |
| mem_reads | `select_random_points` | 13.79ms | 14.80ms | 1.073× | 3.36% | PASS |
| mem_reads | `select_random_ranges` | 3.03ms | 3.58ms | 1.184× | 2.47% | PASS |
| mem_reads | `covering_index_scan` | 2.65ms | 3.13ms | 1.178× | 6.07% | PASS |
| mem_reads | `groupby_scan` | 22.11ms | 21.54ms | 0.974× | 1.31% | PASS |
| mem_reads | `index_join` | 4.99ms | 7.01ms | 1.407× | 5.57% | PASS |
| mem_reads | `index_join_scan` | 4.36ms | 5.28ms | 1.211× | 6.40% | PASS |
| mem_reads | `types_table_scan` | 878.77ms | 829.12ms | 0.944× | 5.57% | PASS |
| mem_reads | `table_scan` | 1.09s | 928.58ms | 0.849× | 6.20% | PASS |
| mem_reads | `oltp_read_only` | 90.12ms | 83.70ms | 0.929× | 3.73% | PASS |
| mem_writes | `oltp_bulk_insert` | 138.41ms | 218.23ms | 1.577× | 1.74% | PASS |
| mem_writes | `oltp_insert` | 13.87ms | 27.14ms | 1.957× | 3.84% | PASS |
| mem_writes | `oltp_update_index` | 50.84ms | 101.90ms | 2.004× | 4.67% | PASS |
| mem_writes | `oltp_update_non_index` | 37.04ms | 68.80ms | 1.858× | 1.76% | PASS |
| mem_writes | `oltp_delete_insert` | 37.31ms | 80.91ms | 2.169× | 4.08% | PASS |
| mem_writes | `oltp_write_only` | 22.07ms | 50.43ms | 2.285× | 2.89% | PASS |
| mem_writes | `types_delete_insert` | 23.72ms | 43.14ms | 1.819× | 2.41% | PASS |
| mem_writes | `oltp_read_write` | 67.49ms | 100.79ms | 1.493× | 3.22% | PASS |
| file_reads | `oltp_point_select` | 76.97ms | 43.34ms | 0.563× | 1.43% | PASS |
| file_reads | `oltp_range_select` | 16.89ms | 11.43ms | 0.677× | 3.34% | PASS |
| file_reads | `oltp_sum_range` | 16.95ms | 11.70ms | 0.690× | 3.11% | PASS |
| file_reads | `oltp_order_range` | 3.07ms | 2.38ms | 0.776× | 1.62% | PASS |
| file_reads | `oltp_distinct_range` | 3.56ms | 2.89ms | 0.811× | 1.73% | PASS |
| file_reads | `oltp_index_scan` | 9.42ms | 6.50ms | 0.690× | 2.20% | PASS |
| file_reads | `select_random_points` | 20.42ms | 17.20ms | 0.843× | 4.96% | PASS |
| file_reads | `select_random_ranges` | 8.93ms | 5.61ms | 0.629× | 1.57% | PASS |
| file_reads | `covering_index_scan` | 10.31ms | 5.41ms | 0.524× | 2.13% | PASS |
| file_reads | `groupby_scan` | 23.68ms | 22.28ms | 0.941× | 1.59% | PASS |
| file_reads | `index_join` | 9.57ms | 8.69ms | 0.908× | 7.04% | PASS |
| file_reads | `index_join_scan` | 4.68ms | 5.00ms | 1.068× | 4.32% | PASS |
| file_reads | `types_table_scan` | 811.26ms | 800.85ms | 0.987× | 5.39% | PASS |
| file_reads | `table_scan` | 931.53ms | 894.58ms | 0.960× | 4.75% | PASS |
| file_reads | `oltp_read_only` | 162.33ms | 107.13ms | 0.660× | 3.88% | PASS |
| file_writes | `oltp_bulk_insert` | 201.42ms | 308.04ms | 1.529× | 3.43% | PASS |
| file_writes | `oltp_insert` | 51.09ms | 57.55ms | 1.126× | 6.63% | PASS |
| file_writes | `oltp_update_index` | 201.09ms | 199.07ms | 0.990× | 4.75% | PASS |
| file_writes | `oltp_update_non_index` | 136.95ms | 133.96ms | 0.978× | 4.85% | PASS |
| file_writes | `oltp_delete_insert` | 162.76ms | 159.47ms | 0.980× | 5.18% | PASS |
| file_writes | `oltp_write_only` | 125.22ms | 109.51ms | 0.875× | 5.04% | PASS |
| file_writes | `types_delete_insert` | 93.28ms | 89.30ms | 0.957× | 7.82% | PASS |
| file_writes | `oltp_read_write` | 168.00ms | 155.38ms | 0.925× | 3.78% | PASS |
| ac_reads | `oltp_point_select` | 40.92ms | 44.29ms | 1.082× | 1.78% | PASS |
| ac_reads | `oltp_range_select` | 13.44ms | 11.54ms | 0.858× | 3.04% | PASS |
| ac_reads | `oltp_sum_range` | 13.12ms | 11.78ms | 0.898× | 3.47% | PASS |
| ac_reads | `oltp_order_range` | 2.68ms | 2.38ms | 0.889× | 2.03% | PASS |
| ac_reads | `oltp_distinct_range` | 3.21ms | 2.90ms | 0.905× | 1.53% | PASS |
| ac_reads | `oltp_index_scan` | 5.89ms | 6.56ms | 1.114× | 1.89% | PASS |
| ac_reads | `select_random_points` | 17.12ms | 16.85ms | 0.984× | 5.21% | PASS |
| ac_reads | `select_random_ranges` | 5.18ms | 5.54ms | 1.070× | 1.24% | PASS |
| ac_reads | `covering_index_scan` | 6.68ms | 5.34ms | 0.799× | 3.06% | PASS |
| ac_reads | `groupby_scan` | 23.47ms | 22.39ms | 0.954× | 1.77% | PASS |
| ac_reads | `index_join` | 7.39ms | 8.40ms | 1.136× | 4.17% | PASS |
| ac_reads | `index_join_scan` | 4.48ms | 5.06ms | 1.129× | 5.04% | PASS |
| ac_reads | `types_table_scan` | 806.72ms | 816.85ms | 1.013× | 3.42% | PASS |
| ac_reads | `table_scan` | 1.15s | 954.11ms | 0.829× | 4.65% | PASS |
| ac_reads | `oltp_read_only` | 122.88ms | 117.96ms | 0.960× | 2.23% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 25.52ms | 54.10ms | 2.120× | 4.85% | PASS |
| ac_writes | `oltp_insert_ac` | 30.22ms | 68.02ms | 2.251× | 7.77% | PASS |
| ac_writes | `oltp_update_index_ac` | 31.79ms | 83.53ms | 2.628× | 6.46% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 26.30ms | 67.29ms | 2.559× | 8.27% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 29.41ms | 76.39ms | 2.597× | 7.20% | PASS |
| ac_writes | `oltp_write_only_ac` | 29.25ms | 74.04ms | 2.531× | 8.55% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.92ms | 68.50ms | 3.275× | 9.66% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.55ms | 77.98ms | 2.395× | 8.92% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 27.63ms | 31.87ms | 1.154× | 0.84% | PASS |
| mem_reads | `oltp_range_select` | 12.35ms | 12.94ms | 1.048× | 0.99% | PASS |
| mem_reads | `oltp_sum_range` | 12.13ms | 12.27ms | 1.011× | 1.31% | PASS |
| mem_reads | `oltp_order_range` | 2.80ms | 2.99ms | 1.066× | 0.76% | PASS |
| mem_reads | `oltp_distinct_range` | 3.69ms | 3.96ms | 1.074× | 0.84% | PASS |
| mem_reads | `oltp_index_scan` | 4.18ms | 5.58ms | 1.335× | 1.05% | PASS |
| mem_reads | `select_random_points` | 17.67ms | 20.06ms | 1.135× | 1.32% | PASS |
| mem_reads | `select_random_ranges` | 3.47ms | 4.70ms | 1.355× | 1.08% | PASS |
| mem_reads | `covering_index_scan` | 3.71ms | 4.17ms | 1.125× | 1.45% | PASS |
| mem_reads | `groupby_scan` | 33.74ms | 34.10ms | 1.011× | 0.38% | PASS |
| mem_reads | `index_join` | 6.44ms | 9.37ms | 1.454× | 1.07% | PASS |
| mem_reads | `index_join_scan` | 3.85ms | 5.38ms | 1.398× | 1.08% | PASS |
| mem_reads | `types_table_scan` | 1.19s | 1.24s | 1.043× | 0.63% | PASS |
| mem_reads | `table_scan` | 1.45s | 1.36s | 0.944× | 0.28% | PASS |
| mem_reads | `oltp_read_only` | 120.65ms | 126.89ms | 1.052× | 0.89% | PASS |
| mem_writes | `oltp_bulk_insert` | 187.71ms | 283.10ms | 1.508× | 0.52% | PASS |
| mem_writes | `oltp_insert` | 17.21ms | 34.86ms | 2.026× | 0.55% | PASS |
| mem_writes | `oltp_update_index` | 64.12ms | 132.85ms | 2.072× | 0.97% | PASS |
| mem_writes | `oltp_update_non_index` | 43.53ms | 79.26ms | 1.821× | 1.10% | PASS |
| mem_writes | `oltp_delete_insert` | 44.42ms | 98.10ms | 2.208× | 0.95% | PASS |
| mem_writes | `oltp_write_only` | 24.68ms | 58.69ms | 2.378× | 1.16% | PASS |
| mem_writes | `types_delete_insert` | 27.79ms | 50.60ms | 1.821× | 0.93% | PASS |
| mem_writes | `oltp_read_write` | 78.51ms | 128.57ms | 1.638× | 1.36% | PASS |
| file_reads | `oltp_point_select` | 58.00ms | 41.62ms | 0.718× | 0.52% | PASS |
| file_reads | `oltp_range_select` | 16.77ms | 14.26ms | 0.850× | 1.87% | PASS |
| file_reads | `oltp_sum_range` | 16.11ms | 13.48ms | 0.837× | 1.82% | PASS |
| file_reads | `oltp_order_range` | 3.26ms | 3.16ms | 0.968× | 1.14% | PASS |
| file_reads | `oltp_distinct_range` | 4.16ms | 4.14ms | 0.995× | 1.08% | PASS |
| file_reads | `oltp_index_scan` | 7.68ms | 6.69ms | 0.871× | 1.00% | PASS |
| file_reads | `select_random_points` | 21.62ms | 21.41ms | 0.990× | 0.74% | PASS |
| file_reads | `select_random_ranges` | 6.78ms | 5.71ms | 0.842× | 1.13% | PASS |
| file_reads | `covering_index_scan` | 7.39ms | 5.19ms | 0.702× | 1.13% | PASS |
| file_reads | `groupby_scan` | 34.26ms | 34.28ms | 1.001× | 0.42% | PASS |
| file_reads | `index_join` | 8.94ms | 10.20ms | 1.141× | 1.12% | PASS |
| file_reads | `index_join_scan` | 4.47ms | 5.74ms | 1.284× | 2.14% | PASS |
| file_reads | `types_table_scan` | 1.21s | 1.26s | 1.038× | 0.36% | PASS |
| file_reads | `table_scan` | 1.45s | 1.36s | 0.943× | 0.47% | PASS |
| file_reads | `oltp_read_only` | 160.68ms | 139.91ms | 0.871× | 0.68% | PASS |
| file_writes | `oltp_bulk_insert` | 251.01ms | 365.51ms | 1.456× | 3.32% | PASS |
| file_writes | `oltp_insert` | 65.32ms | 65.00ms | 0.995× | 3.47% | PASS |
| file_writes | `oltp_update_index` | 201.34ms | 231.06ms | 1.148× | 2.04% | PASS |
| file_writes | `oltp_update_non_index` | 155.10ms | 150.98ms | 0.973× | 2.03% | PASS |
| file_writes | `oltp_delete_insert` | 164.74ms | 181.31ms | 1.101× | 8.39% | PASS |
| file_writes | `oltp_write_only` | 127.27ms | 125.52ms | 0.986× | 2.87% | PASS |
| file_writes | `types_delete_insert` | 100.61ms | 105.16ms | 1.045× | 6.04% | PASS |
| file_writes | `oltp_read_write` | 182.59ms | 193.11ms | 1.058× | 1.31% | PASS |
| ac_reads | `oltp_point_select` | 37.03ms | 41.03ms | 1.108× | 0.71% | PASS |
| ac_reads | `oltp_range_select` | 13.93ms | 14.21ms | 1.020× | 0.94% | PASS |
| ac_reads | `oltp_sum_range` | 13.75ms | 13.54ms | 0.984× | 0.94% | PASS |
| ac_reads | `oltp_order_range` | 3.04ms | 3.15ms | 1.037× | 1.21% | PASS |
| ac_reads | `oltp_distinct_range` | 3.92ms | 4.12ms | 1.049× | 0.88% | PASS |
| ac_reads | `oltp_index_scan` | 5.70ms | 6.64ms | 1.165× | 1.23% | PASS |
| ac_reads | `select_random_points` | 19.88ms | 21.69ms | 1.091× | 1.32% | PASS |
| ac_reads | `select_random_ranges` | 4.83ms | 5.74ms | 1.188× | 1.72% | PASS |
| ac_reads | `covering_index_scan` | 5.36ms | 5.23ms | 0.976× | 1.85% | PASS |
| ac_reads | `groupby_scan` | 34.09ms | 34.32ms | 1.007× | 0.37% | PASS |
| ac_reads | `index_join` | 7.86ms | 10.15ms | 1.291× | 1.27% | PASS |
| ac_reads | `index_join_scan` | 4.28ms | 5.66ms | 1.324× | 1.21% | PASS |
| ac_reads | `types_table_scan` | 1.19s | 1.25s | 1.048× | 0.60% | PASS |
| ac_reads | `table_scan` | 1.43s | 1.36s | 0.949× | 0.45% | PASS |
| ac_reads | `oltp_read_only` | 130.71ms | 139.91ms | 1.070× | 1.05% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 29.14ms | 78.83ms | 2.705× | 5.72% | PASS |
| ac_writes | `oltp_insert_ac` | 31.04ms | 100.06ms | 3.223× | 6.08% | PASS |
| ac_writes | `oltp_update_index_ac` | 32.80ms | 108.65ms | 3.313× | 4.73% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 29.55ms | 90.71ms | 3.069× | 5.75% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 31.36ms | 103.12ms | 3.288× | 5.94% | PASS |
| ac_writes | `oltp_write_only_ac` | 31.24ms | 102.23ms | 3.272× | 5.29% | PASS |
| ac_writes | `types_delete_insert_ac` | 28.38ms | 93.40ms | 3.291× | 8.34% | PASS |
| ac_writes | `oltp_read_write_ac` | 35.24ms | 107.81ms | 3.060× | 6.01% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.56ms | 37.19ms | 1.108× | 1.69% | PASS |
| mem_reads | `oltp_range_select` | 19.72ms | 20.26ms | 1.027× | 2.10% | PASS |
| mem_reads | `oltp_sum_range` | 17.32ms | 19.42ms | 1.121× | 1.39% | PASS |
| mem_reads | `oltp_order_range` | 3.67ms | 3.84ms | 1.047× | 0.97% | PASS |
| mem_reads | `oltp_distinct_range` | 4.69ms | 4.88ms | 1.041× | 0.92% | PASS |
| mem_reads | `oltp_index_scan` | 4.72ms | 5.88ms | 1.246× | 1.98% | PASS |
| mem_reads | `select_random_points` | 26.64ms | 29.77ms | 1.118× | 2.12% | PASS |
| mem_reads | `select_random_ranges` | 7.41ms | 8.20ms | 1.107× | 0.97% | PASS |
| mem_reads | `covering_index_scan` | 4.31ms | 4.38ms | 1.014× | 2.74% | PASS |
| mem_reads | `groupby_scan` | 38.52ms | 39.69ms | 1.030× | 0.65% | PASS |
| mem_reads | `index_join` | 8.00ms | 10.11ms | 1.265× | 1.84% | PASS |
| mem_reads | `index_join_scan` | 4.27ms | 5.63ms | 1.319× | 3.06% | PASS |
| mem_reads | `types_table_scan` | 1.22s | 1.29s | 1.064× | 1.72% | PASS |
| mem_reads | `table_scan` | 1.34s | 1.38s | 1.032× | 3.66% | PASS |
| mem_reads | `oltp_read_only` | 154.62ms | 163.75ms | 1.059× | 1.38% | PASS |
| mem_writes | `oltp_bulk_insert` | 242.02ms | 349.31ms | 1.443× | 0.94% | PASS |
| mem_writes | `oltp_insert` | 19.09ms | 38.14ms | 1.998× | 0.92% | PASS |
| mem_writes | `oltp_update_index` | 69.40ms | 126.77ms | 1.827× | 1.74% | PASS |
| mem_writes | `oltp_update_non_index` | 53.20ms | 90.01ms | 1.692× | 1.24% | PASS |
| mem_writes | `oltp_delete_insert` | 49.97ms | 101.69ms | 2.035× | 1.14% | PASS |
| mem_writes | `oltp_write_only` | 28.21ms | 65.64ms | 2.327× | 1.59% | PASS |
| mem_writes | `types_delete_insert` | 33.76ms | 57.91ms | 1.715× | 1.50% | PASS |
| mem_writes | `oltp_read_write` | 99.97ms | 155.40ms | 1.555× | 1.97% | PASS |
| file_reads | `oltp_point_select` | 121.01ms | 66.75ms | 0.552× | 0.69% | PASS |
| file_reads | `oltp_range_select` | 28.68ms | 23.28ms | 0.812× | 2.21% | PASS |
| file_reads | `oltp_sum_range` | 26.63ms | 22.77ms | 0.855× | 1.25% | PASS |
| file_reads | `oltp_order_range` | 4.63ms | 4.19ms | 0.905× | 1.64% | PASS |
| file_reads | `oltp_distinct_range` | 5.68ms | 5.28ms | 0.929× | 1.70% | PASS |
| file_reads | `oltp_index_scan` | 13.50ms | 9.04ms | 0.669× | 1.39% | PASS |
| file_reads | `select_random_points` | 35.67ms | 32.81ms | 0.920× | 1.65% | PASS |
| file_reads | `select_random_ranges` | 16.46ms | 11.33ms | 0.688× | 1.40% | PASS |
| file_reads | `covering_index_scan` | 14.70ms | 8.18ms | 0.556× | 1.26% | PASS |
| file_reads | `groupby_scan` | 42.95ms | 44.55ms | 1.037× | 0.77% | PASS |
| file_reads | `index_join` | 13.86ms | 13.22ms | 0.954× | 1.46% | PASS |
| file_reads | `index_join_scan` | 5.55ms | 6.35ms | 1.144× | 1.85% | PASS |
| file_reads | `types_table_scan` | 1.17s | 1.28s | 1.096× | 1.34% | PASS |
| file_reads | `table_scan` | 1.54s | 1.51s | 0.982× | 2.53% | PASS |
| file_reads | `oltp_read_only` | 307.05ms | 226.61ms | 0.738× | 0.80% | PASS |
| file_writes | `oltp_bulk_insert` | 279.69ms | 407.18ms | 1.456× | 0.75% | PASS |
| file_writes | `oltp_insert` | 26.47ms | 49.12ms | 1.856× | 1.61% | PASS |
| file_writes | `oltp_update_index` | 110.10ms | 168.98ms | 1.535× | 1.24% | PASS |
| file_writes | `oltp_update_non_index` | 86.12ms | 120.88ms | 1.404× | 2.08% | PASS |
| file_writes | `oltp_delete_insert` | 84.20ms | 138.59ms | 1.646× | 1.30% | PASS |
| file_writes | `oltp_write_only` | 57.13ms | 92.72ms | 1.623× | 2.06% | PASS |
| file_writes | `types_delete_insert` | 54.99ms | 76.90ms | 1.398× | 1.78% | PASS |
| file_writes | `oltp_read_write` | 131.65ms | 181.23ms | 1.377× | 1.31% | PASS |
| ac_reads | `oltp_point_select` | 65.51ms | 72.70ms | 1.110× | 1.07% | PASS |
| ac_reads | `oltp_range_select` | 24.40ms | 25.37ms | 1.040× | 1.37% | PASS |
| ac_reads | `oltp_sum_range` | 22.12ms | 24.61ms | 1.112× | 1.32% | PASS |
| ac_reads | `oltp_order_range` | 4.39ms | 4.54ms | 1.035× | 0.91% | PASS |
| ac_reads | `oltp_distinct_range` | 5.53ms | 5.72ms | 1.034× | 1.23% | PASS |
| ac_reads | `oltp_index_scan` | 8.44ms | 9.77ms | 1.157× | 1.22% | PASS |
| ac_reads | `select_random_points` | 31.66ms | 35.27ms | 1.114× | 1.23% | PASS |
| ac_reads | `select_random_ranges` | 10.37ms | 11.33ms | 1.092× | 0.79% | PASS |
| ac_reads | `covering_index_scan` | 8.24ms | 8.21ms | 0.996× | 0.99% | PASS |
| ac_reads | `groupby_scan` | 42.61ms | 44.95ms | 1.055× | 1.09% | PASS |
| ac_reads | `index_join` | 10.92ms | 13.50ms | 1.237× | 1.79% | PASS |
| ac_reads | `index_join_scan` | 5.03ms | 6.45ms | 1.283× | 2.21% | PASS |
| ac_reads | `types_table_scan` | 1.26s | 1.38s | 1.099× | 1.50% | PASS |
| ac_reads | `table_scan` | 1.60s | 1.52s | 0.949× | 2.37% | PASS |
| ac_reads | `oltp_read_only` | 209.50ms | 224.70ms | 1.073× | 0.94% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.04ms | 55.06ms | 3.231× | 5.27% | PASS |
| ac_writes | `oltp_insert_ac` | 20.98ms | 82.76ms | 3.944× | 9.45% | PASS |
| ac_writes | `oltp_update_index_ac` | 22.38ms | 96.09ms | 4.293× | 4.31% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 18.14ms | 71.24ms | 3.928× | 8.75% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 20.56ms | 86.73ms | 4.218× | 6.08% | PASS |
| ac_writes | `oltp_write_only_ac` | 21.20ms | 85.70ms | 4.043× | 6.89% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.07ms | 71.42ms | 4.183× | 5.84% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.76ms | 88.41ms | 3.304× | 7.01% | PASS |

</details>

## Version-control latency

Wall time: 1m 52s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 61.02ms | 200.00ms | 30.5% | 0.49% | PASS |
| `status_dirty_many_tables` | 63.06ms | 200.00ms | 31.5% | 0.23% | PASS |
| `diff_regular_working_one_table` | 57.18ms | 150.00ms | 38.1% | 0.37% | PASS |
| `diff_regular_working_many_tables` | 67.09ms | 200.00ms | 33.5% | 0.23% | PASS |
| `diff_stat_working_many_tables` | 66.99ms | 200.00ms | 33.5% | 0.23% | PASS |
| `diff_schema_working_many_tables` | 67.44ms | 200.00ms | 33.7% | 0.26% | PASS |
| `branch_list_many_branches` | 15.39ms | 100.00ms | 15.4% | 1.04% | PASS |
| `branch_create_delete` | 22.52ms | 100.00ms | 22.5% | 7.11% | PASS |
| `checkout_branch_clean` | 96.48ms | 200.00ms | 48.2% | 12.54% | PASS |
| `merge_data_no_conflicts` | 28.07ms | 150.00ms | 18.7% | 3.59% | PASS |
| `merge_schema_no_conflicts` | 14.88ms | 100.00ms | 14.9% | 1.10% | PASS |
| `merge_data_conflicts` | 93.59ms | 250.00ms | 37.4% | 0.24% | PASS |
| `merge_data_conflicts_with_resolve` | 93.94ms | 250.00ms | 37.6% | 0.23% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
