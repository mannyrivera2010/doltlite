# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-08 17:49 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu26 20261101.186.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/42498755910)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 9s | 8.99s | 11.56s | 1.286× | 1.70% | **PASS** |
| textpk | 69 | 55 | 1h 32m 26s | 9.75s | 11.71s | 1.201× | 2.18% | **PASS** |
| blobpk | 69 | 55 | 1h 31m 25s | 10.07s | 11.75s | 1.167× | 1.95% | **PASS** |
| compositepk | 69 | 55 | 1h 25m 8s | 9.95s | 10.77s | 1.083× | 3.21% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.20ms | 29.27ms | 1.210× | 2.24% | PASS |
| mem_reads | `oltp_range_select` | 10.84ms | 12.23ms | 1.128× | 1.57% | PASS |
| mem_reads | `oltp_sum_range` | 9.55ms | 12.05ms | 1.262× | 2.42% | PASS |
| mem_reads | `oltp_order_range` | 2.62ms | 3.03ms | 1.156× | 1.12% | PASS |
| mem_reads | `oltp_distinct_range` | 3.71ms | 4.19ms | 1.130× | 1.55% | PASS |
| mem_reads | `oltp_index_scan` | 4.03ms | 5.25ms | 1.303× | 2.04% | PASS |
| mem_reads | `select_random_points` | 10.77ms | 11.00ms | 1.021× | 4.94% | PASS |
| mem_reads | `select_random_ranges` | 3.10ms | 3.96ms | 1.279× | 3.03% | PASS |
| mem_reads | `covering_index_scan` | 4.14ms | 4.14ms | 0.999× | 1.81% | PASS |
| mem_reads | `groupby_scan` | 31.48ms | 34.67ms | 1.101× | 1.82% | PASS |
| mem_reads | `index_join` | 6.05ms | 7.91ms | 1.309× | 1.55% | PASS |
| mem_reads | `index_join_scan` | 3.44ms | 4.46ms | 1.296× | 2.23% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.31s | 1.249× | 0.98% | PASS |
| mem_reads | `table_scan` | 1.22s | 1.46s | 1.191× | 1.58% | PASS |
| mem_reads | `oltp_read_only` | 111.73ms | 126.36ms | 1.131× | 1.99% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.25ms | 252.76ms | 1.402× | 1.03% | PASS |
| mem_writes | `oltp_insert` | 15.31ms | 28.40ms | 1.855× | 1.02% | PASS |
| mem_writes | `oltp_update_index` | 51.97ms | 109.71ms | 2.111× | 1.60% | PASS |
| mem_writes | `oltp_update_non_index` | 35.88ms | 63.23ms | 1.762× | 1.70% | PASS |
| mem_writes | `oltp_delete_insert` | 45.39ms | 82.67ms | 1.822× | 1.44% | PASS |
| mem_writes | `oltp_write_only` | 22.31ms | 47.94ms | 2.148× | 0.88% | PASS |
| mem_writes | `types_delete_insert` | 24.56ms | 42.47ms | 1.729× | 1.96% | PASS |
| mem_writes | `oltp_read_write` | 65.94ms | 110.37ms | 1.674× | 1.21% | PASS |
| file_reads | `oltp_point_select` | 93.33ms | 52.70ms | 0.565× | 0.93% | PASS |
| file_reads | `oltp_range_select` | 17.43ms | 14.70ms | 0.843× | 1.82% | PASS |
| file_reads | `oltp_sum_range` | 16.88ms | 14.59ms | 0.865× | 2.28% | PASS |
| file_reads | `oltp_order_range` | 3.47ms | 3.36ms | 0.969× | 1.65% | PASS |
| file_reads | `oltp_distinct_range` | 4.48ms | 4.49ms | 1.002× | 1.06% | PASS |
| file_reads | `oltp_index_scan` | 11.18ms | 7.86ms | 0.703× | 1.37% | PASS |
| file_reads | `select_random_points` | 17.30ms | 13.54ms | 0.783× | 2.87% | PASS |
| file_reads | `select_random_ranges` | 9.83ms | 6.37ms | 0.647× | 1.50% | PASS |
| file_reads | `covering_index_scan` | 11.20ms | 6.69ms | 0.598× | 1.64% | PASS |
| file_reads | `groupby_scan` | 32.59ms | 35.33ms | 1.084× | 2.24% | PASS |
| file_reads | `index_join` | 10.12ms | 9.86ms | 0.974× | 1.60% | PASS |
| file_reads | `index_join_scan` | 4.40ms | 4.94ms | 1.123× | 2.13% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.34s | 1.286× | 0.52% | PASS |
| file_reads | `table_scan` | 1.15s | 1.43s | 1.239× | 0.78% | PASS |
| file_reads | `oltp_read_only` | 209.73ms | 159.84ms | 0.762× | 0.80% | PASS |
| file_writes | `oltp_bulk_insert` | 192.51ms | 272.89ms | 1.417× | 1.28% | PASS |
| file_writes | `oltp_insert` | 21.90ms | 34.63ms | 1.582× | 1.13% | PASS |
| file_writes | `oltp_update_index` | 76.67ms | 131.91ms | 1.720× | 1.18% | PASS |
| file_writes | `oltp_update_non_index` | 56.57ms | 84.38ms | 1.492× | 1.46% | PASS |
| file_writes | `oltp_delete_insert` | 66.83ms | 104.16ms | 1.559× | 1.27% | PASS |
| file_writes | `oltp_write_only` | 44.45ms | 64.73ms | 1.456× | 1.58% | PASS |
| file_writes | `types_delete_insert` | 40.94ms | 53.31ms | 1.302× | 2.26% | PASS |
| file_writes | `oltp_read_write` | 94.30ms | 130.11ms | 1.380× | 1.70% | PASS |
| ac_reads | `oltp_point_select` | 48.54ms | 52.99ms | 1.092× | 1.29% | PASS |
| ac_reads | `oltp_range_select` | 14.87ms | 15.17ms | 1.020× | 2.05% | PASS |
| ac_reads | `oltp_sum_range` | 13.39ms | 15.02ms | 1.122× | 1.90% | PASS |
| ac_reads | `oltp_order_range` | 3.25ms | 3.43ms | 1.055× | 1.27% | PASS |
| ac_reads | `oltp_distinct_range` | 4.17ms | 4.52ms | 1.084× | 1.24% | PASS |
| ac_reads | `oltp_index_scan` | 6.99ms | 8.06ms | 1.154× | 1.99% | PASS |
| ac_reads | `select_random_points` | 14.70ms | 13.78ms | 0.937× | 5.01% | PASS |
| ac_reads | `select_random_ranges` | 5.71ms | 6.40ms | 1.120× | 1.62% | PASS |
| ac_reads | `covering_index_scan` | 6.88ms | 6.91ms | 1.004× | 2.29% | PASS |
| ac_reads | `groupby_scan` | 32.34ms | 35.50ms | 1.098× | 2.34% | PASS |
| ac_reads | `index_join` | 7.96ms | 10.10ms | 1.270× | 2.51% | PASS |
| ac_reads | `index_join_scan` | 4.11ms | 5.21ms | 1.270× | 3.14% | PASS |
| ac_reads | `types_table_scan` | 1.08s | 1.36s | 1.260× | 1.80% | PASS |
| ac_reads | `table_scan` | 1.23s | 1.46s | 1.188× | 1.70% | PASS |
| ac_reads | `oltp_read_only` | 154.78ms | 167.74ms | 1.084× | 0.94% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.26ms | 64.18ms | 3.019× | 6.03% | PASS |
| ac_writes | `oltp_insert_ac` | 24.45ms | 82.78ms | 3.385× | 6.46% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.75ms | 94.72ms | 3.679× | 5.50% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.51ms | 72.21ms | 3.357× | 5.08% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.12ms | 84.62ms | 3.660× | 6.72% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.33ms | 83.21ms | 3.727× | 4.75% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.80ms | 72.43ms | 3.482× | 4.61% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.64ms | 89.59ms | 3.241× | 3.50% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.34ms | 37.12ms | 1.265× | 1.70% | PASS |
| mem_reads | `oltp_range_select` | 13.86ms | 14.36ms | 1.036× | 2.25% | PASS |
| mem_reads | `oltp_sum_range` | 11.86ms | 13.93ms | 1.175× | 2.52% | PASS |
| mem_reads | `oltp_order_range` | 3.04ms | 3.24ms | 1.064× | 2.04% | PASS |
| mem_reads | `oltp_distinct_range` | 4.04ms | 4.37ms | 1.082× | 1.54% | PASS |
| mem_reads | `oltp_index_scan` | 4.37ms | 6.00ms | 1.372× | 1.94% | PASS |
| mem_reads | `select_random_points` | 16.98ms | 20.42ms | 1.203× | 1.56% | PASS |
| mem_reads | `select_random_ranges` | 3.76ms | 5.13ms | 1.364× | 2.18% | PASS |
| mem_reads | `covering_index_scan` | 4.44ms | 4.34ms | 0.977× | 1.45% | PASS |
| mem_reads | `groupby_scan` | 32.87ms | 36.77ms | 1.119× | 1.51% | PASS |
| mem_reads | `index_join` | 6.82ms | 8.66ms | 1.270× | 1.46% | PASS |
| mem_reads | `index_join_scan` | 4.54ms | 5.41ms | 1.191× | 2.46% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.23s | 1.169× | 0.42% | PASS |
| mem_reads | `table_scan` | 1.19s | 1.34s | 1.123× | 0.88% | PASS |
| mem_reads | `oltp_read_only` | 119.42ms | 137.00ms | 1.147× | 1.17% | PASS |
| mem_writes | `oltp_bulk_insert` | 232.80ms | 359.64ms | 1.545× | 1.01% | PASS |
| mem_writes | `oltp_insert` | 21.43ms | 40.12ms | 1.872× | 0.81% | PASS |
| mem_writes | `oltp_update_index` | 70.18ms | 136.32ms | 1.942× | 1.44% | PASS |
| mem_writes | `oltp_update_non_index` | 48.23ms | 89.17ms | 1.849× | 0.83% | PASS |
| mem_writes | `oltp_delete_insert` | 51.14ms | 107.70ms | 2.106× | 1.98% | PASS |
| mem_writes | `oltp_write_only` | 29.91ms | 65.54ms | 2.191× | 1.31% | PASS |
| mem_writes | `types_delete_insert` | 32.55ms | 57.55ms | 1.768× | 1.20% | PASS |
| mem_writes | `oltp_read_write` | 85.60ms | 142.71ms | 1.667× | 1.37% | PASS |
| file_reads | `oltp_point_select` | 98.99ms | 61.93ms | 0.626× | 1.28% | PASS |
| file_reads | `oltp_range_select` | 21.61ms | 17.15ms | 0.793× | 2.55% | PASS |
| file_reads | `oltp_sum_range` | 19.64ms | 16.61ms | 0.846× | 2.52% | PASS |
| file_reads | `oltp_order_range` | 3.95ms | 3.60ms | 0.912× | 3.59% | PASS |
| file_reads | `oltp_distinct_range` | 5.09ms | 4.78ms | 0.940× | 2.04% | PASS |
| file_reads | `oltp_index_scan` | 11.80ms | 8.94ms | 0.757× | 1.40% | PASS |
| file_reads | `select_random_points` | 25.93ms | 24.00ms | 0.926× | 1.73% | PASS |
| file_reads | `select_random_ranges` | 11.05ms | 7.68ms | 0.696× | 1.68% | PASS |
| file_reads | `covering_index_scan` | 12.45ms | 7.23ms | 0.580× | 2.57% | PASS |
| file_reads | `groupby_scan` | 34.87ms | 36.41ms | 1.044× | 2.14% | PASS |
| file_reads | `index_join` | 10.97ms | 11.19ms | 1.020× | 3.32% | PASS |
| file_reads | `index_join_scan` | 5.50ms | 5.87ms | 1.066× | 2.66% | PASS |
| file_reads | `types_table_scan` | 1.10s | 1.26s | 1.138× | 5.23% | PASS |
| file_reads | `table_scan` | 1.29s | 1.34s | 1.043× | 5.00% | PASS |
| file_reads | `oltp_read_only` | 231.52ms | 176.60ms | 0.763× | 1.69% | PASS |
| file_writes | `oltp_bulk_insert` | 249.75ms | 391.31ms | 1.567× | 0.96% | PASS |
| file_writes | `oltp_insert` | 42.16ms | 53.75ms | 1.275× | 7.19% | PASS |
| file_writes | `oltp_update_index` | 115.64ms | 176.41ms | 1.525× | 2.23% | PASS |
| file_writes | `oltp_update_non_index` | 95.94ms | 118.97ms | 1.240× | 11.20% | PASS |
| file_writes | `oltp_delete_insert` | 95.44ms | 142.62ms | 1.494× | 2.08% | PASS |
| file_writes | `oltp_write_only` | 85.20ms | 91.77ms | 1.077× | 9.10% | PASS |
| file_writes | `types_delete_insert` | 56.88ms | 78.21ms | 1.375× | 1.97% | PASS |
| file_writes | `oltp_read_write` | 145.09ms | 172.76ms | 1.191× | 6.84% | PASS |
| ac_reads | `oltp_point_select` | 53.19ms | 62.00ms | 1.166× | 1.28% | PASS |
| ac_reads | `oltp_range_select` | 17.81ms | 17.14ms | 0.962× | 2.67% | PASS |
| ac_reads | `oltp_sum_range` | 15.96ms | 17.19ms | 1.077× | 2.76% | PASS |
| ac_reads | `oltp_order_range` | 3.91ms | 3.85ms | 0.982× | 3.81% | PASS |
| ac_reads | `oltp_distinct_range` | 4.82ms | 4.88ms | 1.014× | 3.47% | PASS |
| ac_reads | `oltp_index_scan` | 7.26ms | 8.89ms | 1.225× | 2.12% | PASS |
| ac_reads | `select_random_points` | 21.24ms | 24.14ms | 1.136× | 1.89% | PASS |
| ac_reads | `select_random_ranges` | 6.51ms | 7.72ms | 1.186× | 1.89% | PASS |
| ac_reads | `covering_index_scan` | 8.15ms | 7.28ms | 0.893× | 3.49% | PASS |
| ac_reads | `groupby_scan` | 34.36ms | 36.13ms | 1.052× | 3.04% | PASS |
| ac_reads | `index_join` | 9.08ms | 11.23ms | 1.237× | 3.25% | PASS |
| ac_reads | `index_join_scan` | 5.28ms | 5.95ms | 1.126× | 2.82% | PASS |
| ac_reads | `types_table_scan` | 1.09s | 1.25s | 1.142× | 2.64% | PASS |
| ac_reads | `table_scan` | 1.27s | 1.35s | 1.061× | 5.42% | PASS |
| ac_reads | `oltp_read_only` | 156.08ms | 173.80ms | 1.114× | 0.99% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.09ms | 64.12ms | 3.041× | 7.04% | PASS |
| ac_writes | `oltp_insert_ac` | 24.84ms | 78.83ms | 3.173× | 6.16% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.28ms | 96.03ms | 3.654× | 5.44% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.70ms | 74.02ms | 3.411× | 6.37% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.54ms | 85.64ms | 3.639× | 6.23% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.39ms | 84.86ms | 3.627× | 6.33% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.17ms | 78.69ms | 3.717× | 9.33% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.23ms | 92.43ms | 3.058× | 5.85% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.88ms | 38.34ms | 1.203× | 1.47% | PASS |
| mem_reads | `oltp_range_select` | 14.81ms | 14.72ms | 0.993× | 2.74% | PASS |
| mem_reads | `oltp_sum_range` | 12.13ms | 13.99ms | 1.153× | 2.35% | PASS |
| mem_reads | `oltp_order_range` | 3.05ms | 3.23ms | 1.056× | 1.51% | PASS |
| mem_reads | `oltp_distinct_range` | 4.02ms | 4.36ms | 1.084× | 1.16% | PASS |
| mem_reads | `oltp_index_scan` | 4.55ms | 6.20ms | 1.362× | 3.10% | PASS |
| mem_reads | `select_random_points` | 18.23ms | 20.67ms | 1.134× | 3.25% | PASS |
| mem_reads | `select_random_ranges` | 4.01ms | 5.22ms | 1.301× | 1.40% | PASS |
| mem_reads | `covering_index_scan` | 4.42ms | 4.67ms | 1.057× | 4.37% | PASS |
| mem_reads | `groupby_scan` | 33.30ms | 35.97ms | 1.080× | 2.21% | PASS |
| mem_reads | `index_join` | 6.81ms | 9.12ms | 1.341× | 2.59% | PASS |
| mem_reads | `index_join_scan` | 4.20ms | 5.42ms | 1.291× | 3.65% | PASS |
| mem_reads | `types_table_scan` | 1.17s | 1.27s | 1.080× | 1.98% | PASS |
| mem_reads | `table_scan` | 1.36s | 1.37s | 1.006× | 1.95% | PASS |
| mem_reads | `oltp_read_only` | 131.92ms | 142.58ms | 1.081× | 1.22% | PASS |
| mem_writes | `oltp_bulk_insert` | 233.96ms | 356.40ms | 1.523× | 0.98% | PASS |
| mem_writes | `oltp_insert` | 19.59ms | 40.66ms | 2.076× | 1.46% | PASS |
| mem_writes | `oltp_update_index` | 69.73ms | 136.36ms | 1.955× | 2.44% | PASS |
| mem_writes | `oltp_update_non_index` | 50.15ms | 89.25ms | 1.780× | 1.45% | PASS |
| mem_writes | `oltp_delete_insert` | 52.74ms | 110.79ms | 2.101× | 2.03% | PASS |
| mem_writes | `oltp_write_only` | 29.13ms | 66.02ms | 2.266× | 2.30% | PASS |
| mem_writes | `types_delete_insert` | 32.96ms | 58.16ms | 1.765× | 1.27% | PASS |
| mem_writes | `oltp_read_write` | 88.77ms | 143.78ms | 1.620× | 2.07% | PASS |
| file_reads | `oltp_point_select` | 98.53ms | 60.46ms | 0.614× | 1.00% | PASS |
| file_reads | `oltp_range_select` | 22.19ms | 17.06ms | 0.769× | 3.11% | PASS |
| file_reads | `oltp_sum_range` | 20.74ms | 16.90ms | 0.815× | 1.73% | PASS |
| file_reads | `oltp_order_range` | 3.99ms | 3.52ms | 0.882× | 1.30% | PASS |
| file_reads | `oltp_distinct_range` | 4.98ms | 4.66ms | 0.936× | 0.99% | PASS |
| file_reads | `oltp_index_scan` | 12.10ms | 8.93ms | 0.738× | 1.42% | PASS |
| file_reads | `select_random_points` | 27.77ms | 24.53ms | 0.883× | 2.04% | PASS |
| file_reads | `select_random_ranges` | 11.13ms | 7.58ms | 0.681× | 1.17% | PASS |
| file_reads | `covering_index_scan` | 12.28ms | 7.09ms | 0.577× | 1.64% | PASS |
| file_reads | `groupby_scan` | 34.44ms | 36.47ms | 1.059× | 2.70% | PASS |
| file_reads | `index_join` | 11.08ms | 11.18ms | 1.009× | 2.36% | PASS |
| file_reads | `index_join_scan` | 5.17ms | 5.87ms | 1.134× | 2.81% | PASS |
| file_reads | `types_table_scan` | 1.07s | 1.24s | 1.158× | 1.11% | PASS |
| file_reads | `table_scan` | 1.34s | 1.35s | 1.014× | 3.49% | PASS |
| file_reads | `oltp_read_only` | 231.24ms | 175.62ms | 0.759× | 1.55% | PASS |
| file_writes | `oltp_bulk_insert` | 247.84ms | 380.10ms | 1.534× | 1.23% | PASS |
| file_writes | `oltp_insert` | 31.27ms | 52.97ms | 1.694× | 2.57% | PASS |
| file_writes | `oltp_update_index` | 101.06ms | 165.26ms | 1.635× | 1.82% | PASS |
| file_writes | `oltp_update_non_index` | 79.06ms | 112.40ms | 1.422× | 1.70% | PASS |
| file_writes | `oltp_delete_insert` | 81.44ms | 134.84ms | 1.656× | 2.16% | PASS |
| file_writes | `oltp_write_only` | 56.08ms | 88.08ms | 1.571× | 1.73% | PASS |
| file_writes | `types_delete_insert` | 52.80ms | 73.68ms | 1.395× | 1.53% | PASS |
| file_writes | `oltp_read_write` | 122.97ms | 169.11ms | 1.375× | 1.53% | PASS |
| ac_reads | `oltp_point_select` | 54.05ms | 61.74ms | 1.142× | 1.63% | PASS |
| ac_reads | `oltp_range_select` | 17.56ms | 17.11ms | 0.975× | 2.54% | PASS |
| ac_reads | `oltp_sum_range` | 15.05ms | 16.47ms | 1.094× | 2.01% | PASS |
| ac_reads | `oltp_order_range` | 3.53ms | 3.54ms | 1.004× | 1.56% | PASS |
| ac_reads | `oltp_distinct_range` | 4.50ms | 4.71ms | 1.047× | 1.21% | PASS |
| ac_reads | `oltp_index_scan` | 7.47ms | 8.92ms | 1.194× | 1.19% | PASS |
| ac_reads | `select_random_points` | 22.21ms | 24.35ms | 1.096× | 1.50% | PASS |
| ac_reads | `select_random_ranges` | 6.59ms | 7.66ms | 1.163× | 1.01% | PASS |
| ac_reads | `covering_index_scan` | 7.76ms | 7.14ms | 0.920× | 1.60% | PASS |
| ac_reads | `groupby_scan` | 33.92ms | 37.32ms | 1.100× | 2.58% | PASS |
| ac_reads | `index_join` | 8.99ms | 11.31ms | 1.259× | 2.74% | PASS |
| ac_reads | `index_join_scan` | 4.84ms | 5.96ms | 1.232× | 2.86% | PASS |
| ac_reads | `types_table_scan` | 1.13s | 1.26s | 1.112× | 1.48% | PASS |
| ac_reads | `table_scan` | 1.34s | 1.36s | 1.008× | 1.14% | PASS |
| ac_reads | `oltp_read_only` | 160.44ms | 175.61ms | 1.095× | 0.87% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.02ms | 63.00ms | 2.997× | 7.55% | PASS |
| ac_writes | `oltp_insert_ac` | 24.04ms | 84.52ms | 3.516× | 8.11% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.76ms | 94.38ms | 3.527× | 6.64% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.72ms | 75.76ms | 3.488× | 8.02% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.50ms | 95.70ms | 3.906× | 8.35% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.63ms | 84.99ms | 3.597× | 6.89% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.25ms | 75.28ms | 3.718× | 8.25% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.39ms | 92.62ms | 3.152× | 6.27% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.12ms | 33.67ms | 1.118× | 3.08% | PASS |
| mem_reads | `oltp_range_select` | 17.23ms | 16.40ms | 0.952× | 2.40% | PASS |
| mem_reads | `oltp_sum_range` | 15.20ms | 16.10ms | 1.059× | 2.33% | PASS |
| mem_reads | `oltp_order_range` | 3.28ms | 3.30ms | 1.004× | 2.65% | PASS |
| mem_reads | `oltp_distinct_range` | 4.35ms | 4.33ms | 0.996× | 4.07% | PASS |
| mem_reads | `oltp_index_scan` | 4.16ms | 5.45ms | 1.310× | 3.90% | PASS |
| mem_reads | `select_random_points` | 24.59ms | 26.57ms | 1.081× | 2.74% | PASS |
| mem_reads | `select_random_ranges` | 6.51ms | 7.29ms | 1.119× | 2.54% | PASS |
| mem_reads | `covering_index_scan` | 3.54ms | 3.88ms | 1.097× | 2.64% | PASS |
| mem_reads | `groupby_scan` | 32.36ms | 33.30ms | 1.029× | 2.71% | PASS |
| mem_reads | `index_join` | 6.67ms | 8.36ms | 1.254× | 2.89% | PASS |
| mem_reads | `index_join_scan` | 3.57ms | 5.13ms | 1.436× | 4.16% | PASS |
| mem_reads | `types_table_scan` | 940.09ms | 1.04s | 1.110× | 2.81% | PASS |
| mem_reads | `table_scan` | 1.34s | 1.20s | 0.900× | 2.29% | PASS |
| mem_reads | `oltp_read_only` | 140.38ms | 141.38ms | 1.007× | 2.03% | PASS |
| mem_writes | `oltp_bulk_insert` | 193.40ms | 279.24ms | 1.444× | 2.42% | PASS |
| mem_writes | `oltp_insert` | 15.85ms | 31.28ms | 1.973× | 3.16% | PASS |
| mem_writes | `oltp_update_index` | 65.39ms | 118.53ms | 1.813× | 2.96% | PASS |
| mem_writes | `oltp_update_non_index` | 48.70ms | 80.69ms | 1.657× | 2.68% | PASS |
| mem_writes | `oltp_delete_insert` | 47.54ms | 91.42ms | 1.923× | 2.49% | PASS |
| mem_writes | `oltp_write_only` | 25.16ms | 54.17ms | 2.153× | 2.79% | PASS |
| mem_writes | `types_delete_insert` | 29.88ms | 48.22ms | 1.614× | 2.14% | PASS |
| mem_writes | `oltp_read_write` | 95.97ms | 136.51ms | 1.422× | 2.83% | PASS |
| file_reads | `oltp_point_select` | 103.75ms | 56.86ms | 0.548× | 1.68% | PASS |
| file_reads | `oltp_range_select` | 26.15ms | 20.06ms | 0.767× | 2.96% | PASS |
| file_reads | `oltp_sum_range` | 23.90ms | 19.33ms | 0.809× | 2.48% | PASS |
| file_reads | `oltp_order_range` | 3.96ms | 3.39ms | 0.856× | 3.13% | PASS |
| file_reads | `oltp_distinct_range` | 4.89ms | 4.28ms | 0.875× | 3.21% | PASS |
| file_reads | `oltp_index_scan` | 12.47ms | 8.28ms | 0.664× | 2.49% | PASS |
| file_reads | `select_random_points` | 34.23ms | 30.79ms | 0.900× | 3.32% | PASS |
| file_reads | `select_random_ranges` | 14.43ms | 9.96ms | 0.691× | 3.03% | PASS |
| file_reads | `covering_index_scan` | 12.06ms | 6.87ms | 0.569× | 3.63% | PASS |
| file_reads | `groupby_scan` | 33.81ms | 33.82ms | 1.000× | 2.47% | PASS |
| file_reads | `index_join` | 11.65ms | 11.29ms | 0.969× | 4.68% | PASS |
| file_reads | `index_join_scan` | 4.80ms | 5.81ms | 1.211× | 4.59% | PASS |
| file_reads | `types_table_scan` | 1.09s | 1.13s | 1.033× | 2.08% | PASS |
| file_reads | `table_scan` | 1.38s | 1.21s | 0.872× | 2.46% | PASS |
| file_reads | `oltp_read_only` | 244.94ms | 175.17ms | 0.715× | 1.48% | PASS |
| file_writes | `oltp_bulk_insert` | 258.82ms | 366.87ms | 1.417× | 4.50% | PASS |
| file_writes | `oltp_insert` | 29.12ms | 47.44ms | 1.629× | 3.22% | PASS |
| file_writes | `oltp_update_index` | 170.59ms | 198.34ms | 1.163× | 4.63% | PASS |
| file_writes | `oltp_update_non_index` | 149.13ms | 147.47ms | 0.989× | 4.81% | PASS |
| file_writes | `oltp_delete_insert` | 148.82ms | 158.40ms | 1.064× | 4.65% | PASS |
| file_writes | `oltp_write_only` | 99.99ms | 116.45ms | 1.165× | 11.64% | PASS |
| file_writes | `types_delete_insert` | 89.84ms | 95.73ms | 1.066× | 10.53% | PASS |
| file_writes | `oltp_read_write` | 175.77ms | 201.03ms | 1.144× | 3.37% | PASS |
| ac_reads | `oltp_point_select` | 56.12ms | 60.89ms | 1.085× | 2.70% | PASS |
| ac_reads | `oltp_range_select` | 21.40ms | 20.33ms | 0.950× | 4.45% | PASS |
| ac_reads | `oltp_sum_range` | 18.78ms | 19.78ms | 1.053× | 3.50% | PASS |
| ac_reads | `oltp_order_range` | 3.68ms | 3.60ms | 0.978× | 3.70% | PASS |
| ac_reads | `oltp_distinct_range` | 4.62ms | 4.61ms | 0.997× | 3.82% | PASS |
| ac_reads | `oltp_index_scan` | 7.28ms | 8.52ms | 1.170× | 4.15% | PASS |
| ac_reads | `select_random_points` | 27.72ms | 30.33ms | 1.094× | 3.16% | PASS |
| ac_reads | `select_random_ranges` | 9.77ms | 10.28ms | 1.052× | 2.32% | PASS |
| ac_reads | `covering_index_scan` | 6.79ms | 6.83ms | 1.006× | 5.30% | PASS |
| ac_reads | `groupby_scan` | 32.98ms | 34.86ms | 1.057× | 3.35% | PASS |
| ac_reads | `index_join` | 9.64ms | 12.06ms | 1.252× | 4.56% | PASS |
| ac_reads | `index_join_scan` | 4.44ms | 5.99ms | 1.347× | 5.10% | PASS |
| ac_reads | `types_table_scan` | 1.00s | 1.08s | 1.081× | 3.86% | PASS |
| ac_reads | `table_scan` | 1.16s | 1.16s | 1.002× | 6.52% | PASS |
| ac_reads | `oltp_read_only` | 165.61ms | 174.46ms | 1.053× | 3.18% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.57ms | 60.01ms | 2.782× | 14.81% | PASS |
| ac_writes | `oltp_insert_ac` | 25.85ms | 120.45ms | 4.660× | 29.12% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.59ms | 89.78ms | 3.377× | 10.49% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.93ms | 75.34ms | 3.600× | 15.16% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.55ms | 89.10ms | 3.629× | 14.65% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.49ms | 87.47ms | 3.571× | 17.14% | PASS |
| ac_writes | `types_delete_insert_ac` | 25.06ms | 73.28ms | 2.924× | 22.88% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.14ms | 92.62ms | 3.179× | 9.13% | PASS |

</details>

## Version-control latency

Wall time: 1m 57s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 61.00ms | 200.00ms | 30.5% | 0.31% | PASS |
| `status_dirty_many_tables` | 63.12ms | 200.00ms | 31.6% | 0.26% | PASS |
| `diff_regular_working_one_table` | 57.47ms | 150.00ms | 38.3% | 0.37% | PASS |
| `diff_regular_working_many_tables` | 67.33ms | 200.00ms | 33.7% | 0.38% | PASS |
| `diff_stat_working_many_tables` | 67.40ms | 200.00ms | 33.7% | 0.29% | PASS |
| `diff_schema_working_many_tables` | 67.70ms | 200.00ms | 33.8% | 0.32% | PASS |
| `branch_list_many_branches` | 15.47ms | 100.00ms | 15.5% | 0.83% | PASS |
| `branch_create_delete` | 22.70ms | 100.00ms | 22.7% | 7.40% | PASS |
| `checkout_branch_clean` | 101.06ms | 200.00ms | 50.5% | 14.01% | PASS |
| `merge_data_no_conflicts` | 28.99ms | 150.00ms | 19.3% | 4.57% | PASS |
| `merge_schema_no_conflicts` | 15.23ms | 100.00ms | 15.2% | 3.09% | PASS |
| `merge_data_conflicts` | 93.95ms | 250.00ms | 37.6% | 0.21% | PASS |
| `merge_data_conflicts_with_resolve` | 94.09ms | 250.00ms | 37.6% | 0.22% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
