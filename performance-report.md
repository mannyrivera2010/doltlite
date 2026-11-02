# DoltLite Performance Report

> Nightly result: **FAIL**
>
> Generated: 2026-11-02 21:14 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261026.354.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/41512263510)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 14m 13s | 8.80s | 11.54s | 1.311× | 1.45% | **PASS** |
| textpk | 69 | 55 | 1h 29m 26s | 9.78s | 9.96s | 1.019× | 1.35% | **PASS** |
| blobpk | 69 | 55 | 1h 9m 31s | 6.94s | 7.96s | 1.146× | 2.87% | **FAIL** |
| compositepk | 69 | 55 | 1h 2m 17s | 6.72s | 7.65s | 1.138× | 2.64% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.93ms | 30.04ms | 1.255× | 1.47% | PASS |
| mem_reads | `oltp_range_select` | 10.18ms | 13.29ms | 1.305× | 2.43% | PASS |
| mem_reads | `oltp_sum_range` | 9.73ms | 12.33ms | 1.267× | 1.45% | PASS |
| mem_reads | `oltp_order_range` | 2.58ms | 3.02ms | 1.172× | 1.54% | PASS |
| mem_reads | `oltp_distinct_range` | 3.62ms | 4.13ms | 1.141× | 1.15% | PASS |
| mem_reads | `oltp_index_scan` | 3.92ms | 5.35ms | 1.364× | 1.95% | PASS |
| mem_reads | `select_random_points` | 10.60ms | 11.24ms | 1.061× | 3.04% | PASS |
| mem_reads | `select_random_ranges` | 3.07ms | 4.01ms | 1.306× | 2.19% | PASS |
| mem_reads | `covering_index_scan` | 4.23ms | 4.26ms | 1.007× | 2.15% | PASS |
| mem_reads | `groupby_scan` | 29.84ms | 33.42ms | 1.120× | 0.83% | PASS |
| mem_reads | `index_join` | 6.11ms | 8.19ms | 1.340× | 2.30% | PASS |
| mem_reads | `index_join_scan` | 3.62ms | 4.59ms | 1.267× | 1.83% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.35s | 1.304× | 0.46% | PASS |
| mem_reads | `table_scan` | 1.16s | 1.41s | 1.216× | 0.49% | PASS |
| mem_reads | `oltp_read_only` | 102.80ms | 124.84ms | 1.214× | 0.80% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.77ms | 261.93ms | 1.441× | 1.19% | PASS |
| mem_writes | `oltp_insert` | 15.55ms | 29.39ms | 1.890× | 0.84% | PASS |
| mem_writes | `oltp_update_index` | 51.06ms | 111.42ms | 2.182× | 0.99% | PASS |
| mem_writes | `oltp_update_non_index` | 34.95ms | 64.02ms | 1.832× | 1.45% | PASS |
| mem_writes | `oltp_delete_insert` | 45.79ms | 83.61ms | 1.826× | 0.99% | PASS |
| mem_writes | `oltp_write_only` | 22.70ms | 49.15ms | 2.166× | 0.71% | PASS |
| mem_writes | `types_delete_insert` | 26.14ms | 44.97ms | 1.720× | 1.08% | PASS |
| mem_writes | `oltp_read_write` | 72.34ms | 116.88ms | 1.616× | 1.96% | PASS |
| file_reads | `oltp_point_select` | 93.75ms | 53.16ms | 0.567× | 0.86% | PASS |
| file_reads | `oltp_range_select` | 17.77ms | 15.80ms | 0.889× | 1.79% | PASS |
| file_reads | `oltp_sum_range` | 17.18ms | 15.09ms | 0.878× | 1.39% | PASS |
| file_reads | `oltp_order_range` | 3.41ms | 3.33ms | 0.978× | 1.28% | PASS |
| file_reads | `oltp_distinct_range` | 4.47ms | 4.46ms | 0.996× | 0.91% | PASS |
| file_reads | `oltp_index_scan` | 11.14ms | 8.14ms | 0.731× | 1.22% | PASS |
| file_reads | `select_random_points` | 17.83ms | 13.79ms | 0.773× | 1.59% | PASS |
| file_reads | `select_random_ranges` | 10.01ms | 6.45ms | 0.645× | 1.06% | PASS |
| file_reads | `covering_index_scan` | 11.24ms | 6.89ms | 0.613× | 0.81% | PASS |
| file_reads | `groupby_scan` | 30.79ms | 33.75ms | 1.096× | 1.03% | PASS |
| file_reads | `index_join` | 10.03ms | 10.01ms | 0.997× | 1.74% | PASS |
| file_reads | `index_join_scan` | 4.62ms | 5.01ms | 1.085× | 1.97% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.35s | 1.302× | 0.78% | PASS |
| file_reads | `table_scan` | 1.17s | 1.42s | 1.208× | 1.19% | PASS |
| file_reads | `oltp_read_only` | 206.26ms | 159.73ms | 0.774× | 1.08% | PASS |
| file_writes | `oltp_bulk_insert` | 194.89ms | 280.73ms | 1.440× | 1.14% | PASS |
| file_writes | `oltp_insert` | 22.10ms | 35.99ms | 1.628× | 1.27% | PASS |
| file_writes | `oltp_update_index` | 78.39ms | 138.14ms | 1.762× | 1.65% | PASS |
| file_writes | `oltp_update_non_index` | 59.05ms | 83.84ms | 1.420× | 1.87% | PASS |
| file_writes | `oltp_delete_insert` | 68.72ms | 108.45ms | 1.578× | 1.36% | PASS |
| file_writes | `oltp_write_only` | 45.27ms | 66.96ms | 1.479× | 1.80% | PASS |
| file_writes | `types_delete_insert` | 41.17ms | 53.80ms | 1.307× | 2.04% | PASS |
| file_writes | `oltp_read_write` | 93.44ms | 133.37ms | 1.427× | 1.01% | PASS |
| ac_reads | `oltp_point_select` | 47.04ms | 53.16ms | 1.130× | 0.94% | PASS |
| ac_reads | `oltp_range_select` | 13.34ms | 15.81ms | 1.185× | 2.43% | PASS |
| ac_reads | `oltp_sum_range` | 12.93ms | 15.05ms | 1.164× | 2.26% | PASS |
| ac_reads | `oltp_order_range` | 2.95ms | 3.31ms | 1.123× | 1.64% | PASS |
| ac_reads | `oltp_distinct_range` | 3.96ms | 4.45ms | 1.122× | 1.48% | PASS |
| ac_reads | `oltp_index_scan` | 6.54ms | 8.15ms | 1.246× | 2.25% | PASS |
| ac_reads | `select_random_points` | 13.75ms | 13.82ms | 1.005× | 2.50% | PASS |
| ac_reads | `select_random_ranges` | 5.53ms | 6.46ms | 1.167× | 1.32% | PASS |
| ac_reads | `covering_index_scan` | 6.77ms | 6.91ms | 1.021× | 1.55% | PASS |
| ac_reads | `groupby_scan` | 30.19ms | 33.86ms | 1.122× | 1.02% | PASS |
| ac_reads | `index_join` | 7.55ms | 10.08ms | 1.336× | 1.18% | PASS |
| ac_reads | `index_join_scan` | 4.03ms | 5.01ms | 1.242× | 1.27% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.35s | 1.302× | 0.90% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.42s | 1.211× | 0.88% | PASS |
| ac_reads | `oltp_read_only` | 139.86ms | 160.63ms | 1.149× | 1.38% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.37ms | 62.82ms | 3.084× | 6.83% | PASS |
| ac_writes | `oltp_insert_ac` | 22.70ms | 80.27ms | 3.536× | 5.01% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.65ms | 94.85ms | 3.698× | 6.50% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.93ms | 73.09ms | 3.493× | 7.28% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.06ms | 84.75ms | 3.675× | 7.22% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.26ms | 83.04ms | 3.730× | 4.77% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.43ms | 73.73ms | 3.609× | 8.26% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.10ms | 90.71ms | 3.347× | 3.20% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.12ms | 28.41ms | 1.131× | 1.90% | PASS |
| mem_reads | `oltp_range_select` | 12.10ms | 11.16ms | 0.923× | 2.30% | PASS |
| mem_reads | `oltp_sum_range` | 10.39ms | 10.88ms | 1.046× | 1.46% | PASS |
| mem_reads | `oltp_order_range` | 2.67ms | 2.55ms | 0.954× | 1.35% | PASS |
| mem_reads | `oltp_distinct_range` | 3.46ms | 3.31ms | 0.958× | 0.82% | PASS |
| mem_reads | `oltp_index_scan` | 3.85ms | 4.93ms | 1.283× | 1.60% | PASS |
| mem_reads | `select_random_points` | 14.29ms | 16.10ms | 1.127× | 1.48% | PASS |
| mem_reads | `select_random_ranges` | 3.33ms | 4.14ms | 1.246× | 1.09% | PASS |
| mem_reads | `covering_index_scan` | 3.81ms | 3.49ms | 0.915× | 2.46% | PASS |
| mem_reads | `groupby_scan` | 26.94ms | 27.66ms | 1.027× | 0.96% | PASS |
| mem_reads | `index_join` | 5.83ms | 7.52ms | 1.291× | 4.30% | PASS |
| mem_reads | `index_join_scan` | 4.25ms | 4.81ms | 1.133× | 2.22% | PASS |
| mem_reads | `types_table_scan` | 1.11s | 1.05s | 0.949× | 1.62% | PASS |
| mem_reads | `table_scan` | 1.43s | 1.14s | 0.800× | 0.67% | PASS |
| mem_reads | `oltp_read_only` | 110.67ms | 107.72ms | 0.973× | 1.39% | PASS |
| mem_writes | `oltp_bulk_insert` | 182.62ms | 277.91ms | 1.522× | 0.92% | PASS |
| mem_writes | `oltp_insert` | 18.27ms | 32.96ms | 1.804× | 1.16% | PASS |
| mem_writes | `oltp_update_index` | 62.05ms | 118.77ms | 1.914× | 1.64% | PASS |
| mem_writes | `oltp_update_non_index` | 42.53ms | 76.51ms | 1.799× | 1.01% | PASS |
| mem_writes | `oltp_delete_insert` | 44.36ms | 91.39ms | 2.060× | 1.31% | PASS |
| mem_writes | `oltp_write_only` | 25.22ms | 55.12ms | 2.185× | 1.40% | PASS |
| mem_writes | `types_delete_insert` | 27.85ms | 48.08ms | 1.726× | 1.04% | PASS |
| mem_writes | `oltp_read_write` | 75.73ms | 116.52ms | 1.539× | 2.44% | PASS |
| file_reads | `oltp_point_select` | 94.97ms | 52.25ms | 0.550× | 1.21% | PASS |
| file_reads | `oltp_range_select` | 19.10ms | 13.27ms | 0.695× | 0.84% | PASS |
| file_reads | `oltp_sum_range` | 17.68ms | 13.15ms | 0.744× | 0.77% | PASS |
| file_reads | `oltp_order_range` | 3.51ms | 2.81ms | 0.800× | 0.92% | PASS |
| file_reads | `oltp_distinct_range` | 4.31ms | 3.58ms | 0.832× | 0.88% | PASS |
| file_reads | `oltp_index_scan` | 11.20ms | 7.45ms | 0.666× | 0.91% | PASS |
| file_reads | `select_random_points` | 22.94ms | 19.12ms | 0.833× | 1.45% | PASS |
| file_reads | `select_random_ranges` | 10.50ms | 6.61ms | 0.629× | 0.63% | PASS |
| file_reads | `covering_index_scan` | 12.00ms | 6.26ms | 0.521× | 0.76% | PASS |
| file_reads | `groupby_scan` | 28.18ms | 28.12ms | 0.998× | 0.94% | PASS |
| file_reads | `index_join` | 10.35ms | 9.40ms | 0.908× | 1.13% | PASS |
| file_reads | `index_join_scan` | 4.95ms | 5.22ms | 1.055× | 1.67% | PASS |
| file_reads | `types_table_scan` | 1.01s | 1.02s | 1.009× | 4.51% | PASS |
| file_reads | `table_scan` | 1.41s | 1.13s | 0.802× | 2.02% | PASS |
| file_reads | `oltp_read_only` | 202.89ms | 137.06ms | 0.676× | 0.54% | PASS |
| file_writes | `oltp_bulk_insert` | 248.25ms | 374.82ms | 1.510× | 4.68% | PASS |
| file_writes | `oltp_insert` | 60.88ms | 57.46ms | 0.944× | 1.78% | PASS |
| file_writes | `oltp_update_index` | 212.72ms | 220.89ms | 1.038× | 1.26% | PASS |
| file_writes | `oltp_update_non_index` | 159.52ms | 149.31ms | 0.936× | 3.28% | PASS |
| file_writes | `oltp_delete_insert` | 167.51ms | 174.34ms | 1.041× | 1.16% | PASS |
| file_writes | `oltp_write_only` | 124.69ms | 122.30ms | 0.981× | 2.71% | PASS |
| file_writes | `types_delete_insert` | 103.66ms | 92.22ms | 0.890× | 5.88% | PASS |
| file_writes | `oltp_read_write` | 174.59ms | 178.39ms | 1.022× | 4.03% | PASS |
| ac_reads | `oltp_point_select` | 47.81ms | 51.56ms | 1.078× | 1.14% | PASS |
| ac_reads | `oltp_range_select` | 14.82ms | 13.40ms | 0.904× | 1.75% | PASS |
| ac_reads | `oltp_sum_range` | 13.33ms | 13.30ms | 0.998× | 1.32% | PASS |
| ac_reads | `oltp_order_range` | 3.11ms | 2.82ms | 0.907× | 0.83% | PASS |
| ac_reads | `oltp_distinct_range` | 3.83ms | 3.58ms | 0.934× | 0.62% | PASS |
| ac_reads | `oltp_index_scan` | 6.70ms | 7.42ms | 1.108× | 0.74% | PASS |
| ac_reads | `select_random_points` | 18.13ms | 19.11ms | 1.054× | 1.30% | PASS |
| ac_reads | `select_random_ranges` | 6.03ms | 6.62ms | 1.098× | 0.72% | PASS |
| ac_reads | `covering_index_scan` | 7.63ms | 6.29ms | 0.824× | 0.76% | PASS |
| ac_reads | `groupby_scan` | 27.78ms | 28.12ms | 1.012× | 0.85% | PASS |
| ac_reads | `index_join` | 8.07ms | 9.32ms | 1.154× | 1.30% | PASS |
| ac_reads | `index_join_scan` | 4.54ms | 5.11ms | 1.125× | 1.47% | PASS |
| ac_reads | `types_table_scan` | 914.80ms | 976.70ms | 1.068× | 1.61% | PASS |
| ac_reads | `table_scan` | 1.07s | 1.06s | 0.993× | 1.15% | PASS |
| ac_reads | `oltp_read_only` | 129.93ms | 134.31ms | 1.034× | 0.49% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.09ms | 69.62ms | 3.465× | 45.48% | PASS |
| ac_writes | `oltp_insert_ac` | 20.98ms | 67.60ms | 3.221× | 10.19% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.56ms | 79.35ms | 3.681× | 10.66% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.30ms | 64.11ms | 3.707× | 16.70% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.98ms | 70.81ms | 3.730× | 8.71% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.79ms | 74.51ms | 3.766× | 9.12% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.06ms | 64.08ms | 3.756× | 11.70% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.70ms | 77.90ms | 3.154× | 8.36% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 19.55ms | 22.66ms | 1.159× | 2.44% | PASS |
| mem_reads | `oltp_range_select` | 9.21ms | 9.11ms | 0.988× | 3.09% | PASS |
| mem_reads | `oltp_sum_range` | 8.18ms | 8.62ms | 1.054× | 6.34% | PASS |
| mem_reads | `oltp_order_range` | 1.99ms | 2.03ms | 1.017× | 3.45% | PASS |
| mem_reads | `oltp_distinct_range` | 2.55ms | 2.53ms | 0.993× | 2.85% | PASS |
| mem_reads | `oltp_index_scan` | 2.88ms | 3.97ms | 1.378× | 4.10% | PASS |
| mem_reads | `select_random_points` | 11.65ms | 13.96ms | 1.199× | 4.33% | PASS |
| mem_reads | `select_random_ranges` | 2.97ms | 3.49ms | 1.176× | 2.97% | PASS |
| mem_reads | `covering_index_scan` | 2.59ms | 2.98ms | 1.150× | 5.51% | PASS |
| mem_reads | `groupby_scan` | 19.58ms | 19.78ms | 1.010× | 2.19% | PASS |
| mem_reads | `index_join` | 4.49ms | 5.91ms | 1.316× | 2.69% | PASS |
| mem_reads | `index_join_scan` | 3.27ms | 4.62ms | 1.414× | 6.33% | PASS |
| mem_reads | `types_table_scan` | 718.00ms | 764.13ms | 1.064× | 2.38% | PASS |
| mem_reads | `table_scan` | 829.37ms | 872.36ms | 1.052× | 1.72% | PASS |
| mem_reads | `oltp_read_only` | 72.67ms | 77.45ms | 1.066× | 3.81% | PASS |
| mem_writes | `oltp_bulk_insert` | 150.80ms | 228.14ms | 1.513× | 1.55% | PASS |
| mem_writes | `oltp_insert` | 12.96ms | 28.88ms | 2.228× | 1.42% | PASS |
| mem_writes | `oltp_update_index` | 45.39ms | 98.20ms | 2.163× | 2.36% | PASS |
| mem_writes | `oltp_update_non_index` | 32.79ms | 63.42ms | 1.934× | 2.18% | PASS |
| mem_writes | `oltp_delete_insert` | 32.16ms | 74.62ms | 2.320× | 1.61% | PASS |
| mem_writes | `oltp_write_only` | 18.45ms | 46.99ms | 2.547× | 1.73% | FAIL |
| mem_writes | `types_delete_insert` | 22.00ms | 39.96ms | 1.816× | 2.03% | PASS |
| mem_writes | `oltp_read_write` | 49.55ms | 89.31ms | 1.802× | 1.39% | PASS |
| file_reads | `oltp_point_select` | 75.69ms | 42.29ms | 0.559× | 1.72% | PASS |
| file_reads | `oltp_range_select` | 14.86ms | 10.73ms | 0.722× | 2.60% | PASS |
| file_reads | `oltp_sum_range` | 14.90ms | 10.79ms | 0.724× | 1.87% | PASS |
| file_reads | `oltp_order_range` | 2.82ms | 2.31ms | 0.820× | 2.11% | PASS |
| file_reads | `oltp_distinct_range` | 3.38ms | 2.84ms | 0.840× | 1.74% | PASS |
| file_reads | `oltp_index_scan` | 9.16ms | 6.33ms | 0.691× | 2.13% | PASS |
| file_reads | `select_random_points` | 19.20ms | 16.24ms | 0.846× | 3.41% | PASS |
| file_reads | `select_random_ranges` | 8.71ms | 5.55ms | 0.636× | 1.93% | PASS |
| file_reads | `covering_index_scan` | 9.41ms | 5.24ms | 0.557× | 2.96% | PASS |
| file_reads | `groupby_scan` | 21.01ms | 20.26ms | 0.964× | 2.29% | PASS |
| file_reads | `index_join` | 8.37ms | 7.61ms | 0.909× | 3.18% | PASS |
| file_reads | `index_join_scan` | 4.08ms | 4.68ms | 1.146× | 3.37% | PASS |
| file_reads | `types_table_scan` | 711.95ms | 771.51ms | 1.084× | 2.19% | PASS |
| file_reads | `table_scan` | 832.10ms | 862.41ms | 1.036× | 2.73% | PASS |
| file_reads | `oltp_read_only` | 145.00ms | 100.99ms | 0.696× | 1.77% | PASS |
| file_writes | `oltp_bulk_insert` | 199.53ms | 289.08ms | 1.449× | 3.62% | PASS |
| file_writes | `oltp_insert` | 29.73ms | 56.73ms | 1.908× | 4.34% | PASS |
| file_writes | `oltp_update_index` | 171.09ms | 190.42ms | 1.113× | 4.21% | PASS |
| file_writes | `oltp_update_non_index` | 130.54ms | 128.61ms | 0.985× | 0.72% | PASS |
| file_writes | `oltp_delete_insert` | 132.86ms | 149.35ms | 1.124× | 5.82% | PASS |
| file_writes | `oltp_write_only` | 99.85ms | 104.72ms | 1.049× | 6.62% | PASS |
| file_writes | `types_delete_insert` | 89.98ms | 79.40ms | 0.882× | 10.57% | PASS |
| file_writes | `oltp_read_write` | 130.61ms | 143.39ms | 1.098× | 3.36% | PASS |
| ac_reads | `oltp_point_select` | 37.31ms | 41.31ms | 1.107× | 1.48% | PASS |
| ac_reads | `oltp_range_select` | 10.42ms | 10.58ms | 1.015× | 2.77% | PASS |
| ac_reads | `oltp_sum_range` | 10.39ms | 10.51ms | 1.012× | 3.53% | PASS |
| ac_reads | `oltp_order_range` | 2.48ms | 2.31ms | 0.934× | 2.21% | PASS |
| ac_reads | `oltp_distinct_range` | 2.96ms | 2.82ms | 0.953× | 2.82% | PASS |
| ac_reads | `oltp_index_scan` | 5.34ms | 6.34ms | 1.186× | 3.22% | PASS |
| ac_reads | `select_random_points` | 14.52ms | 15.76ms | 1.086× | 2.87% | PASS |
| ac_reads | `select_random_ranges` | 4.86ms | 5.51ms | 1.135× | 2.14% | PASS |
| ac_reads | `covering_index_scan` | 5.25ms | 5.25ms | 1.000× | 6.19% | PASS |
| ac_reads | `groupby_scan` | 21.12ms | 20.91ms | 0.990× | 1.64% | PASS |
| ac_reads | `index_join` | 6.10ms | 7.41ms | 1.215× | 3.50% | PASS |
| ac_reads | `index_join_scan` | 3.69ms | 4.70ms | 1.272× | 3.52% | PASS |
| ac_reads | `types_table_scan` | 747.78ms | 792.75ms | 1.060× | 2.93% | PASS |
| ac_reads | `table_scan` | 836.43ms | 872.81ms | 1.043× | 1.93% | PASS |
| ac_reads | `oltp_read_only` | 98.90ms | 103.31ms | 1.044× | 2.74% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.57ms | 54.30ms | 2.406× | 4.99% | PASS |
| ac_writes | `oltp_insert_ac` | 26.88ms | 74.91ms | 2.787× | 7.59% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.60ms | 81.81ms | 2.860× | 5.47% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.92ms | 63.35ms | 2.890× | 4.18% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.14ms | 72.00ms | 2.865× | 5.26% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.21ms | 70.76ms | 2.807× | 5.16% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.30ms | 63.45ms | 3.126× | 6.00% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.43ms | 77.79ms | 2.736× | 6.27% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.02ms | 24.33ms | 1.105× | 2.94% | PASS |
| mem_reads | `oltp_range_select` | 11.65ms | 11.66ms | 1.001× | 2.20% | PASS |
| mem_reads | `oltp_sum_range` | 10.56ms | 11.47ms | 1.086× | 3.50% | PASS |
| mem_reads | `oltp_order_range` | 2.25ms | 2.14ms | 0.953× | 2.89% | PASS |
| mem_reads | `oltp_distinct_range` | 2.67ms | 2.64ms | 0.989× | 1.52% | PASS |
| mem_reads | `oltp_index_scan` | 2.86ms | 3.64ms | 1.273× | 3.01% | PASS |
| mem_reads | `select_random_points` | 17.25ms | 19.41ms | 1.125× | 3.58% | PASS |
| mem_reads | `select_random_ranges` | 4.56ms | 5.07ms | 1.112× | 2.19% | PASS |
| mem_reads | `covering_index_scan` | 2.32ms | 2.78ms | 1.197× | 3.90% | PASS |
| mem_reads | `groupby_scan` | 20.61ms | 21.32ms | 1.035× | 1.86% | PASS |
| mem_reads | `index_join` | 4.86ms | 5.97ms | 1.229× | 4.93% | PASS |
| mem_reads | `index_join_scan` | 2.76ms | 4.07ms | 1.473× | 7.75% | PASS |
| mem_reads | `types_table_scan` | 692.69ms | 744.05ms | 1.074× | 1.59% | PASS |
| mem_reads | `table_scan` | 854.55ms | 860.90ms | 1.007× | 5.39% | PASS |
| mem_reads | `oltp_read_only` | 85.28ms | 89.57ms | 1.050× | 2.22% | PASS |
| mem_writes | `oltp_bulk_insert` | 147.73ms | 212.29ms | 1.437× | 1.08% | PASS |
| mem_writes | `oltp_insert` | 12.24ms | 25.40ms | 2.075× | 1.78% | PASS |
| mem_writes | `oltp_update_index` | 44.32ms | 83.86ms | 1.892× | 3.00% | PASS |
| mem_writes | `oltp_update_non_index` | 33.13ms | 59.55ms | 1.798× | 1.96% | PASS |
| mem_writes | `oltp_delete_insert` | 31.78ms | 66.21ms | 2.083× | 2.45% | PASS |
| mem_writes | `oltp_write_only` | 17.60ms | 42.45ms | 2.412× | 1.72% | PASS |
| mem_writes | `types_delete_insert` | 21.70ms | 37.69ms | 1.737× | 3.12% | PASS |
| mem_writes | `oltp_read_write` | 58.45ms | 92.97ms | 1.591× | 3.61% | PASS |
| file_reads | `oltp_point_select` | 73.37ms | 41.43ms | 0.565× | 2.12% | PASS |
| file_reads | `oltp_range_select` | 16.51ms | 12.86ms | 0.779× | 1.92% | PASS |
| file_reads | `oltp_sum_range` | 15.15ms | 12.47ms | 0.823× | 1.45% | PASS |
| file_reads | `oltp_order_range` | 2.76ms | 2.28ms | 0.825× | 2.16% | PASS |
| file_reads | `oltp_distinct_range` | 3.28ms | 2.80ms | 0.854× | 1.57% | PASS |
| file_reads | `oltp_index_scan` | 8.26ms | 5.39ms | 0.653× | 1.71% | PASS |
| file_reads | `select_random_points` | 22.14ms | 20.14ms | 0.910× | 1.48% | PASS |
| file_reads | `select_random_ranges` | 9.65ms | 6.68ms | 0.692× | 1.53% | PASS |
| file_reads | `covering_index_scan` | 7.82ms | 4.17ms | 0.533× | 2.34% | PASS |
| file_reads | `groupby_scan` | 20.50ms | 20.69ms | 1.009× | 1.19% | PASS |
| file_reads | `index_join` | 7.77ms | 7.03ms | 0.905× | 2.99% | PASS |
| file_reads | `index_join_scan` | 3.42ms | 4.31ms | 1.259× | 3.13% | PASS |
| file_reads | `types_table_scan` | 674.66ms | 730.82ms | 1.083× | 1.59% | PASS |
| file_reads | `table_scan` | 771.05ms | 808.32ms | 1.048× | 1.17% | PASS |
| file_reads | `oltp_read_only` | 155.09ms | 111.30ms | 0.718× | 2.14% | PASS |
| file_writes | `oltp_bulk_insert` | 199.37ms | 268.85ms | 1.349× | 3.25% | PASS |
| file_writes | `oltp_insert` | 19.54ms | 38.57ms | 1.975× | 6.68% | PASS |
| file_writes | `oltp_update_index` | 149.37ms | 160.26ms | 1.073× | 4.41% | PASS |
| file_writes | `oltp_update_non_index` | 124.04ms | 118.47ms | 0.955× | 4.39% | PASS |
| file_writes | `oltp_delete_insert` | 115.03ms | 129.72ms | 1.128× | 4.49% | PASS |
| file_writes | `oltp_write_only` | 89.35ms | 98.33ms | 1.100× | 7.69% | PASS |
| file_writes | `types_delete_insert` | 74.26ms | 65.61ms | 0.884× | 10.55% | PASS |
| file_writes | `oltp_read_write` | 130.38ms | 148.39ms | 1.138× | 2.48% | PASS |
| ac_reads | `oltp_point_select` | 37.73ms | 40.74ms | 1.080× | 1.84% | PASS |
| ac_reads | `oltp_range_select` | 12.30ms | 12.82ms | 1.042× | 1.99% | PASS |
| ac_reads | `oltp_sum_range` | 11.64ms | 12.67ms | 1.088× | 1.88% | PASS |
| ac_reads | `oltp_order_range` | 2.41ms | 2.33ms | 0.969× | 2.18% | PASS |
| ac_reads | `oltp_distinct_range` | 2.98ms | 2.86ms | 0.958× | 2.81% | PASS |
| ac_reads | `oltp_index_scan` | 5.20ms | 5.95ms | 1.145× | 2.69% | PASS |
| ac_reads | `select_random_points` | 19.59ms | 20.97ms | 1.071× | 3.38% | PASS |
| ac_reads | `select_random_ranges` | 6.45ms | 6.92ms | 1.072× | 2.64% | PASS |
| ac_reads | `covering_index_scan` | 4.79ms | 4.93ms | 1.030× | 1.53% | PASS |
| ac_reads | `groupby_scan` | 20.86ms | 21.15ms | 1.014× | 2.23% | PASS |
| ac_reads | `index_join` | 6.36ms | 8.09ms | 1.271× | 3.25% | PASS |
| ac_reads | `index_join_scan` | 3.01ms | 4.25ms | 1.414× | 4.37% | PASS |
| ac_reads | `types_table_scan` | 694.07ms | 745.93ms | 1.075× | 1.68% | PASS |
| ac_reads | `table_scan` | 796.81ms | 842.98ms | 1.058× | 1.99% | PASS |
| ac_reads | `oltp_read_only` | 115.37ms | 116.37ms | 1.009× | 1.91% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 19.41ms | 51.23ms | 2.639× | 5.45% | PASS |
| ac_writes | `oltp_insert_ac` | 23.46ms | 68.36ms | 2.913× | 3.77% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.38ms | 79.55ms | 3.134× | 3.39% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.77ms | 64.77ms | 3.118× | 4.03% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.55ms | 71.26ms | 3.160× | 4.20% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.74ms | 72.85ms | 3.204× | 4.25% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.59ms | 63.07ms | 2.565× | 9.95% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.57ms | 80.38ms | 3.026× | 4.33% | PASS |

</details>

## Version-control latency

Wall time: 2m 23s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.87ms | 200.00ms | 41.9% | 0.76% | PASS |
| `status_dirty_many_tables` | 87.48ms | 200.00ms | 43.7% | 0.65% | PASS |
| `diff_regular_working_one_table` | 80.60ms | 150.00ms | 53.7% | 0.99% | PASS |
| `diff_regular_working_many_tables` | 94.03ms | 200.00ms | 47.0% | 1.72% | PASS |
| `diff_stat_working_many_tables` | 92.50ms | 200.00ms | 46.3% | 0.48% | PASS |
| `diff_schema_working_many_tables` | 93.05ms | 200.00ms | 46.5% | 0.50% | PASS |
| `branch_list_many_branches` | 23.50ms | 100.00ms | 23.5% | 1.58% | PASS |
| `branch_create_delete` | 25.36ms | 100.00ms | 25.4% | 1.40% | PASS |
| `checkout_branch_clean` | 56.73ms | 200.00ms | 28.4% | 1.18% | PASS |
| `merge_data_no_conflicts` | 29.84ms | 150.00ms | 19.9% | 1.57% | PASS |
| `merge_schema_no_conflicts` | 23.19ms | 100.00ms | 23.2% | 1.75% | PASS |
| `merge_data_conflicts` | 128.82ms | 250.00ms | 51.5% | 0.35% | PASS |
| `merge_data_conflicts_with_resolve` | 128.68ms | 250.00ms | 51.5% | 0.37% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
