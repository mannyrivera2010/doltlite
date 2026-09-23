# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-23 15:55 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260920.314.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/35873391617)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 33s | 9.41s | 11.36s | 1.207× | 1.28% | **PASS** |
| textpk | 69 | 55 | 1h 32m 47s | 10.06s | 11.88s | 1.181× | 1.58% | **PASS** |
| blobpk | 69 | 55 | 1h 31m 37s | 9.29s | 10.34s | 1.113× | 2.06% | **PASS** |
| compositepk | 69 | 55 | 1h 15m 52s | 9.11s | 10.51s | 1.153× | 1.16% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.89ms | 27.63ms | 1.110× | 1.09% | PASS |
| mem_reads | `oltp_range_select` | 10.61ms | 11.87ms | 1.119× | 1.14% | PASS |
| mem_reads | `oltp_sum_range` | 9.77ms | 11.31ms | 1.158× | 0.97% | PASS |
| mem_reads | `oltp_order_range` | 2.67ms | 2.85ms | 1.066× | 0.94% | PASS |
| mem_reads | `oltp_distinct_range` | 3.76ms | 3.87ms | 1.030× | 0.74% | PASS |
| mem_reads | `oltp_index_scan` | 3.99ms | 4.85ms | 1.217× | 0.98% | PASS |
| mem_reads | `select_random_points` | 10.49ms | 10.98ms | 1.047× | 1.15% | PASS |
| mem_reads | `select_random_ranges` | 3.10ms | 3.95ms | 1.271× | 1.12% | PASS |
| mem_reads | `covering_index_scan` | 4.37ms | 4.04ms | 0.926× | 1.15% | PASS |
| mem_reads | `groupby_scan` | 31.79ms | 34.20ms | 1.076× | 0.62% | PASS |
| mem_reads | `index_join` | 5.96ms | 7.72ms | 1.296× | 0.82% | PASS |
| mem_reads | `index_join_scan` | 3.55ms | 4.83ms | 1.361× | 1.28% | PASS |
| mem_reads | `types_table_scan` | 1.11s | 1.30s | 1.171× | 0.27% | PASS |
| mem_reads | `table_scan` | 1.26s | 1.42s | 1.128× | 0.38% | PASS |
| mem_reads | `oltp_read_only` | 103.12ms | 114.35ms | 1.109× | 0.78% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.09ms | 241.82ms | 1.350× | 0.95% | PASS |
| mem_writes | `oltp_insert` | 15.77ms | 28.14ms | 1.785× | 0.63% | PASS |
| mem_writes | `oltp_update_index` | 52.80ms | 106.60ms | 2.019× | 1.82% | PASS |
| mem_writes | `oltp_update_non_index` | 37.63ms | 60.95ms | 1.620× | 2.16% | PASS |
| mem_writes | `oltp_delete_insert` | 48.48ms | 82.81ms | 1.708× | 1.69% | PASS |
| mem_writes | `oltp_write_only` | 23.26ms | 46.95ms | 2.018× | 1.89% | PASS |
| mem_writes | `types_delete_insert` | 25.98ms | 40.59ms | 1.563× | 1.71% | PASS |
| mem_writes | `oltp_read_write` | 69.15ms | 106.36ms | 1.538× | 2.91% | PASS |
| file_reads | `oltp_point_select` | 121.85ms | 59.79ms | 0.491× | 0.70% | PASS |
| file_reads | `oltp_range_select` | 20.70ms | 15.22ms | 0.735× | 2.38% | PASS |
| file_reads | `oltp_sum_range` | 19.84ms | 14.78ms | 0.745× | 1.60% | PASS |
| file_reads | `oltp_order_range` | 3.76ms | 3.29ms | 0.877× | 2.09% | PASS |
| file_reads | `oltp_distinct_range` | 4.82ms | 4.29ms | 0.890× | 1.85% | PASS |
| file_reads | `oltp_index_scan` | 13.67ms | 8.68ms | 0.635× | 1.24% | PASS |
| file_reads | `select_random_points` | 21.15ms | 14.91ms | 0.705× | 2.29% | PASS |
| file_reads | `select_random_ranges` | 12.85ms | 7.25ms | 0.565× | 0.87% | PASS |
| file_reads | `covering_index_scan` | 14.28ms | 7.71ms | 0.540× | 1.29% | PASS |
| file_reads | `groupby_scan` | 33.01ms | 34.81ms | 1.055× | 0.82% | PASS |
| file_reads | `index_join` | 11.26ms | 10.08ms | 0.895× | 1.72% | PASS |
| file_reads | `index_join_scan` | 4.81ms | 5.20ms | 1.080× | 2.28% | PASS |
| file_reads | `types_table_scan` | 1.11s | 1.31s | 1.178× | 0.95% | PASS |
| file_reads | `table_scan` | 1.30s | 1.44s | 1.114× | 1.91% | PASS |
| file_reads | `oltp_read_only` | 249.20ms | 163.40ms | 0.656× | 0.75% | PASS |
| file_writes | `oltp_bulk_insert` | 195.09ms | 262.27ms | 1.344× | 0.70% | PASS |
| file_writes | `oltp_insert` | 22.28ms | 36.45ms | 1.636× | 1.26% | PASS |
| file_writes | `oltp_update_index` | 82.36ms | 133.26ms | 1.618× | 1.47% | PASS |
| file_writes | `oltp_update_non_index` | 61.65ms | 83.82ms | 1.360× | 2.73% | PASS |
| file_writes | `oltp_delete_insert` | 69.96ms | 101.61ms | 1.452× | 1.73% | PASS |
| file_writes | `oltp_write_only` | 45.27ms | 67.26ms | 1.486× | 2.31% | PASS |
| file_writes | `types_delete_insert` | 41.55ms | 54.79ms | 1.319× | 1.87% | PASS |
| file_writes | `oltp_read_write` | 90.17ms | 126.67ms | 1.405× | 1.86% | PASS |
| ac_reads | `oltp_point_select` | 56.10ms | 59.32ms | 1.057× | 0.76% | PASS |
| ac_reads | `oltp_range_select` | 14.81ms | 15.29ms | 1.033× | 1.06% | PASS |
| ac_reads | `oltp_sum_range` | 13.81ms | 14.85ms | 1.075× | 1.60% | PASS |
| ac_reads | `oltp_order_range` | 3.19ms | 3.30ms | 1.033× | 1.72% | PASS |
| ac_reads | `oltp_distinct_range` | 4.24ms | 4.29ms | 1.013× | 1.10% | PASS |
| ac_reads | `oltp_index_scan` | 7.63ms | 8.71ms | 1.142× | 1.08% | PASS |
| ac_reads | `select_random_points` | 15.02ms | 15.02ms | 1.000× | 3.56% | PASS |
| ac_reads | `select_random_ranges` | 6.52ms | 7.26ms | 1.114× | 1.00% | PASS |
| ac_reads | `covering_index_scan` | 8.03ms | 7.77ms | 0.967× | 1.13% | PASS |
| ac_reads | `groupby_scan` | 32.66ms | 34.83ms | 1.066× | 0.99% | PASS |
| ac_reads | `index_join` | 8.38ms | 10.45ms | 1.248× | 1.85% | PASS |
| ac_reads | `index_join_scan` | 4.22ms | 5.30ms | 1.256× | 2.95% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.31s | 1.178× | 0.92% | PASS |
| ac_reads | `table_scan` | 1.25s | 1.42s | 1.136× | 0.55% | PASS |
| ac_reads | `oltp_read_only` | 147.89ms | 159.99ms | 1.082× | 0.67% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.10ms | 63.42ms | 3.938× | 3.94% | PASS |
| ac_writes | `oltp_insert_ac` | 17.93ms | 79.44ms | 4.430× | 4.22% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.50ms | 94.60ms | 4.851× | 1.91% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.60ms | 73.16ms | 4.408× | 3.05% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.52ms | 85.23ms | 4.864× | 2.20% | PASS |
| ac_writes | `oltp_write_only_ac` | 17.97ms | 83.68ms | 4.656× | 4.78% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.06ms | 73.59ms | 4.581× | 4.54% | PASS |
| ac_writes | `oltp_read_write_ac` | 23.16ms | 91.14ms | 3.936× | 2.84% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.70ms | 37.48ms | 1.221× | 1.14% | PASS |
| mem_reads | `oltp_range_select` | 13.39ms | 14.04ms | 1.048× | 1.98% | PASS |
| mem_reads | `oltp_sum_range` | 12.38ms | 14.11ms | 1.140× | 1.78% | PASS |
| mem_reads | `oltp_order_range` | 2.93ms | 3.15ms | 1.076× | 1.25% | PASS |
| mem_reads | `oltp_distinct_range` | 4.05ms | 4.21ms | 1.038× | 0.75% | PASS |
| mem_reads | `oltp_index_scan` | 4.59ms | 6.29ms | 1.372× | 1.43% | PASS |
| mem_reads | `select_random_points` | 18.81ms | 20.69ms | 1.100× | 2.22% | PASS |
| mem_reads | `select_random_ranges` | 4.12ms | 5.17ms | 1.258× | 1.26% | PASS |
| mem_reads | `covering_index_scan` | 4.91ms | 4.68ms | 0.953× | 3.36% | PASS |
| mem_reads | `groupby_scan` | 32.35ms | 33.81ms | 1.045× | 1.03% | PASS |
| mem_reads | `index_join` | 7.34ms | 9.50ms | 1.295× | 2.23% | PASS |
| mem_reads | `index_join_scan` | 4.93ms | 5.34ms | 1.082× | 3.42% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.23s | 1.163× | 0.76% | PASS |
| mem_reads | `table_scan` | 1.25s | 1.37s | 1.095× | 1.14% | PASS |
| mem_reads | `oltp_read_only` | 121.70ms | 136.01ms | 1.118× | 1.45% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.15ms | 355.06ms | 1.504× | 0.82% | PASS |
| mem_writes | `oltp_insert` | 21.77ms | 40.15ms | 1.844× | 1.04% | PASS |
| mem_writes | `oltp_update_index` | 71.59ms | 133.24ms | 1.861× | 0.95% | PASS |
| mem_writes | `oltp_update_non_index` | 49.38ms | 86.56ms | 1.753× | 0.91% | PASS |
| mem_writes | `oltp_delete_insert` | 51.39ms | 104.02ms | 2.024× | 0.90% | PASS |
| mem_writes | `oltp_write_only` | 28.88ms | 62.06ms | 2.149× | 1.20% | PASS |
| mem_writes | `types_delete_insert` | 33.68ms | 55.14ms | 1.637× | 1.10% | PASS |
| mem_writes | `oltp_read_write` | 88.82ms | 139.77ms | 1.574× | 1.22% | PASS |
| file_reads | `oltp_point_select` | 106.67ms | 63.78ms | 0.598× | 0.88% | PASS |
| file_reads | `oltp_range_select` | 21.69ms | 16.80ms | 0.775× | 1.68% | PASS |
| file_reads | `oltp_sum_range` | 20.76ms | 16.98ms | 0.818× | 2.19% | PASS |
| file_reads | `oltp_order_range` | 3.85ms | 3.48ms | 0.903× | 1.52% | PASS |
| file_reads | `oltp_distinct_range` | 4.99ms | 4.55ms | 0.913× | 1.62% | PASS |
| file_reads | `oltp_index_scan` | 12.48ms | 9.12ms | 0.731× | 1.70% | PASS |
| file_reads | `select_random_points` | 27.50ms | 24.20ms | 0.880× | 1.37% | PASS |
| file_reads | `select_random_ranges` | 11.81ms | 7.82ms | 0.662× | 1.19% | PASS |
| file_reads | `covering_index_scan` | 12.66ms | 7.41ms | 0.585× | 3.38% | PASS |
| file_reads | `groupby_scan` | 33.35ms | 34.24ms | 1.027× | 1.20% | PASS |
| file_reads | `index_join` | 11.83ms | 11.29ms | 0.954× | 3.45% | PASS |
| file_reads | `index_join_scan` | 6.08ms | 5.92ms | 0.974× | 1.88% | PASS |
| file_reads | `types_table_scan` | 1.14s | 1.25s | 1.097× | 0.84% | PASS |
| file_reads | `table_scan` | 1.44s | 1.40s | 0.974× | 1.55% | PASS |
| file_reads | `oltp_read_only` | 237.92ms | 176.68ms | 0.743× | 0.96% | PASS |
| file_writes | `oltp_bulk_insert` | 255.35ms | 386.80ms | 1.515× | 0.98% | PASS |
| file_writes | `oltp_insert` | 50.88ms | 53.31ms | 1.048× | 25.74% | PASS |
| file_writes | `oltp_update_index` | 116.03ms | 170.83ms | 1.472× | 1.87% | PASS |
| file_writes | `oltp_update_non_index` | 100.95ms | 112.73ms | 1.117× | 12.05% | PASS |
| file_writes | `oltp_delete_insert` | 92.01ms | 134.71ms | 1.464× | 1.94% | PASS |
| file_writes | `oltp_write_only` | 90.92ms | 86.08ms | 0.947× | 12.77% | PASS |
| file_writes | `types_delete_insert` | 56.83ms | 75.39ms | 1.327× | 1.49% | PASS |
| file_writes | `oltp_read_write` | 140.26ms | 165.00ms | 1.176× | 6.45% | PASS |
| ac_reads | `oltp_point_select` | 56.25ms | 63.51ms | 1.129× | 0.87% | PASS |
| ac_reads | `oltp_range_select` | 17.15ms | 16.93ms | 0.987× | 1.15% | PASS |
| ac_reads | `oltp_sum_range` | 15.86ms | 17.07ms | 1.077× | 2.08% | PASS |
| ac_reads | `oltp_order_range` | 3.34ms | 3.47ms | 1.039× | 1.58% | PASS |
| ac_reads | `oltp_distinct_range` | 4.46ms | 4.56ms | 1.024× | 1.29% | PASS |
| ac_reads | `oltp_index_scan` | 7.61ms | 9.20ms | 1.208× | 2.27% | PASS |
| ac_reads | `select_random_points` | 22.09ms | 24.39ms | 1.104× | 1.68% | PASS |
| ac_reads | `select_random_ranges` | 6.82ms | 7.84ms | 1.151× | 1.37% | PASS |
| ac_reads | `covering_index_scan` | 8.25ms | 7.43ms | 0.900× | 2.92% | PASS |
| ac_reads | `groupby_scan` | 32.53ms | 34.22ms | 1.052× | 0.99% | PASS |
| ac_reads | `index_join` | 9.14ms | 11.26ms | 1.231× | 3.00% | PASS |
| ac_reads | `index_join_scan` | 5.37ms | 5.86ms | 1.090× | 2.03% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.23s | 1.159× | 0.92% | PASS |
| ac_reads | `table_scan` | 1.29s | 1.38s | 1.067× | 2.32% | PASS |
| ac_reads | `oltp_read_only` | 161.62ms | 175.99ms | 1.089× | 1.21% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.45ms | 81.78ms | 3.487× | 5.37% | PASS |
| ac_writes | `oltp_insert_ac` | 27.13ms | 95.88ms | 3.534× | 7.75% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.22ms | 111.59ms | 4.100× | 3.99% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.04ms | 96.30ms | 4.006× | 6.74% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.93ms | 100.84ms | 4.214× | 4.18% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.31ms | 108.10ms | 4.108× | 5.69% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.64ms | 95.57ms | 4.220× | 7.06% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.41ms | 108.84ms | 3.465× | 4.66% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.57ms | 27.91ms | 1.136× | 2.78% | PASS |
| mem_reads | `oltp_range_select` | 11.42ms | 10.88ms | 0.952× | 3.91% | PASS |
| mem_reads | `oltp_sum_range` | 10.40ms | 10.60ms | 1.019× | 3.19% | PASS |
| mem_reads | `oltp_order_range` | 2.56ms | 2.54ms | 0.993× | 1.27% | PASS |
| mem_reads | `oltp_distinct_range` | 3.35ms | 3.32ms | 0.990× | 0.99% | PASS |
| mem_reads | `oltp_index_scan` | 3.77ms | 4.98ms | 1.320× | 1.83% | PASS |
| mem_reads | `select_random_points` | 14.77ms | 16.33ms | 1.106× | 3.22% | PASS |
| mem_reads | `select_random_ranges` | 3.57ms | 4.24ms | 1.189× | 2.73% | PASS |
| mem_reads | `covering_index_scan` | 3.64ms | 3.89ms | 1.069× | 2.06% | PASS |
| mem_reads | `groupby_scan` | 27.09ms | 27.90ms | 1.030× | 1.23% | PASS |
| mem_reads | `index_join` | 5.67ms | 7.95ms | 1.401× | 3.14% | PASS |
| mem_reads | `index_join_scan` | 3.76ms | 4.82ms | 1.284× | 2.06% | PASS |
| mem_reads | `types_table_scan` | 1.01s | 1.03s | 1.022× | 2.22% | PASS |
| mem_reads | `table_scan` | 1.13s | 1.09s | 0.965× | 4.47% | PASS |
| mem_reads | `oltp_read_only` | 101.79ms | 103.43ms | 1.016× | 2.87% | PASS |
| mem_writes | `oltp_bulk_insert` | 183.57ms | 258.10ms | 1.406× | 1.05% | PASS |
| mem_writes | `oltp_insert` | 15.99ms | 30.02ms | 1.878× | 1.32% | PASS |
| mem_writes | `oltp_update_index` | 55.21ms | 101.20ms | 1.833× | 1.49% | PASS |
| mem_writes | `oltp_update_non_index` | 40.03ms | 66.26ms | 1.655× | 1.87% | PASS |
| mem_writes | `oltp_delete_insert` | 40.85ms | 80.86ms | 1.979× | 1.90% | PASS |
| mem_writes | `oltp_write_only` | 23.64ms | 49.05ms | 2.075× | 2.19% | PASS |
| mem_writes | `types_delete_insert` | 26.64ms | 41.65ms | 1.564× | 1.78% | PASS |
| mem_writes | `oltp_read_write` | 70.17ms | 106.84ms | 1.523× | 2.17% | PASS |
| file_reads | `oltp_point_select` | 99.82ms | 53.01ms | 0.531× | 1.02% | PASS |
| file_reads | `oltp_range_select` | 19.49ms | 13.50ms | 0.693× | 1.63% | PASS |
| file_reads | `oltp_sum_range` | 18.97ms | 13.51ms | 0.712× | 2.11% | PASS |
| file_reads | `oltp_order_range` | 3.44ms | 2.83ms | 0.823× | 0.89% | PASS |
| file_reads | `oltp_distinct_range` | 4.25ms | 3.58ms | 0.843× | 1.10% | PASS |
| file_reads | `oltp_index_scan` | 11.73ms | 7.59ms | 0.647× | 1.31% | PASS |
| file_reads | `select_random_points` | 22.76ms | 19.12ms | 0.840× | 2.25% | PASS |
| file_reads | `select_random_ranges` | 11.18ms | 6.73ms | 0.602× | 0.46% | PASS |
| file_reads | `covering_index_scan` | 12.32ms | 6.39ms | 0.519× | 1.03% | PASS |
| file_reads | `groupby_scan` | 28.00ms | 28.29ms | 1.010× | 0.66% | PASS |
| file_reads | `index_join` | 10.36ms | 9.35ms | 0.903× | 1.10% | PASS |
| file_reads | `index_join_scan` | 4.65ms | 5.17ms | 1.113× | 2.01% | PASS |
| file_reads | `types_table_scan` | 923.35ms | 995.88ms | 1.079× | 2.88% | PASS |
| file_reads | `table_scan` | 1.18s | 1.10s | 0.928× | 5.05% | PASS |
| file_reads | `oltp_read_only` | 210.30ms | 139.44ms | 0.663× | 1.21% | PASS |
| file_writes | `oltp_bulk_insert` | 251.48ms | 347.35ms | 1.381× | 14.26% | PASS |
| file_writes | `oltp_insert` | 55.50ms | 61.05ms | 1.100× | 25.40% | PASS |
| file_writes | `oltp_update_index` | 197.52ms | 221.95ms | 1.124× | 16.80% | PASS |
| file_writes | `oltp_update_non_index` | 166.49ms | 137.18ms | 0.824× | 22.83% | PASS |
| file_writes | `oltp_delete_insert` | 159.04ms | 167.46ms | 1.053× | 15.28% | PASS |
| file_writes | `oltp_write_only` | 160.49ms | 116.89ms | 0.728× | 31.71% | PASS |
| file_writes | `types_delete_insert` | 121.91ms | 100.78ms | 0.827× | 25.26% | PASS |
| file_writes | `oltp_read_write` | 224.39ms | 187.84ms | 0.837× | 21.95% | PASS |
| ac_reads | `oltp_point_select` | 48.94ms | 52.51ms | 1.073× | 0.95% | PASS |
| ac_reads | `oltp_range_select` | 14.24ms | 13.41ms | 0.941× | 1.86% | PASS |
| ac_reads | `oltp_sum_range` | 13.02ms | 13.02ms | 1.000× | 1.00% | PASS |
| ac_reads | `oltp_order_range` | 2.97ms | 2.80ms | 0.943× | 0.82% | PASS |
| ac_reads | `oltp_distinct_range` | 3.74ms | 3.58ms | 0.959× | 0.81% | PASS |
| ac_reads | `oltp_index_scan` | 6.75ms | 7.54ms | 1.118× | 1.23% | PASS |
| ac_reads | `select_random_points` | 17.69ms | 19.02ms | 1.075× | 1.61% | PASS |
| ac_reads | `select_random_ranges` | 6.24ms | 6.72ms | 1.077× | 0.68% | PASS |
| ac_reads | `covering_index_scan` | 7.11ms | 6.36ms | 0.895× | 0.73% | PASS |
| ac_reads | `groupby_scan` | 27.38ms | 28.10ms | 1.026× | 0.91% | PASS |
| ac_reads | `index_join` | 7.90ms | 9.69ms | 1.227× | 2.06% | PASS |
| ac_reads | `index_join_scan` | 4.20ms | 5.12ms | 1.220× | 1.61% | PASS |
| ac_reads | `types_table_scan` | 904.61ms | 974.51ms | 1.077× | 3.24% | PASS |
| ac_reads | `table_scan` | 1.14s | 1.09s | 0.959× | 5.50% | PASS |
| ac_reads | `oltp_read_only` | 136.44ms | 138.27ms | 1.013× | 1.57% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.07ms | 75.21ms | 3.125× | 33.84% | PASS |
| ac_writes | `oltp_insert_ac` | 41.97ms | 177.01ms | 4.218× | 75.30% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.81ms | 153.55ms | 5.331× | 60.46% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 28.42ms | 134.20ms | 4.722× | 57.81% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 40.65ms | 230.84ms | 5.678× | 67.37% | PASS |
| ac_writes | `oltp_write_only_ac` | 28.44ms | 153.46ms | 5.397× | 68.10% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.13ms | 99.13ms | 4.286× | 62.73% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.82ms | 120.94ms | 3.924× | 58.13% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.58ms | 34.76ms | 1.175× | 0.99% | PASS |
| mem_reads | `oltp_range_select` | 16.69ms | 17.56ms | 1.052× | 0.90% | PASS |
| mem_reads | `oltp_sum_range` | 15.84ms | 17.19ms | 1.085× | 0.87% | PASS |
| mem_reads | `oltp_order_range` | 3.16ms | 3.34ms | 1.056× | 0.72% | PASS |
| mem_reads | `oltp_distinct_range` | 3.95ms | 4.16ms | 1.053× | 0.64% | PASS |
| mem_reads | `oltp_index_scan` | 4.08ms | 5.55ms | 1.360× | 1.74% | PASS |
| mem_reads | `select_random_points` | 25.90ms | 29.99ms | 1.158× | 1.05% | PASS |
| mem_reads | `select_random_ranges` | 6.66ms | 7.96ms | 1.195× | 0.72% | PASS |
| mem_reads | `covering_index_scan` | 3.66ms | 3.90ms | 1.066× | 2.35% | PASS |
| mem_reads | `groupby_scan` | 33.05ms | 33.76ms | 1.022× | 0.51% | PASS |
| mem_reads | `index_join` | 7.15ms | 9.84ms | 1.376× | 1.46% | PASS |
| mem_reads | `index_join_scan` | 3.44ms | 5.05ms | 1.467× | 1.18% | PASS |
| mem_reads | `types_table_scan` | 1.01s | 1.11s | 1.091× | 1.51% | PASS |
| mem_reads | `table_scan` | 1.22s | 1.23s | 1.006× | 0.69% | PASS |
| mem_reads | `oltp_read_only` | 131.42ms | 143.29ms | 1.090× | 0.67% | PASS |
| mem_writes | `oltp_bulk_insert` | 214.86ms | 299.55ms | 1.394× | 0.88% | PASS |
| mem_writes | `oltp_insert` | 16.79ms | 30.78ms | 1.833× | 0.76% | PASS |
| mem_writes | `oltp_update_index` | 60.91ms | 106.15ms | 1.743× | 1.19% | PASS |
| mem_writes | `oltp_update_non_index` | 43.66ms | 70.27ms | 1.610× | 0.92% | PASS |
| mem_writes | `oltp_delete_insert` | 43.82ms | 82.59ms | 1.885× | 0.94% | PASS |
| mem_writes | `oltp_write_only` | 23.08ms | 48.57ms | 2.105× | 1.25% | PASS |
| mem_writes | `types_delete_insert` | 27.90ms | 46.33ms | 1.661× | 0.82% | PASS |
| mem_writes | `oltp_read_write` | 88.48ms | 131.70ms | 1.489× | 1.16% | PASS |
| file_reads | `oltp_point_select` | 62.53ms | 46.83ms | 0.749× | 1.12% | PASS |
| file_reads | `oltp_range_select` | 20.23ms | 19.19ms | 0.948× | 0.94% | PASS |
| file_reads | `oltp_sum_range` | 19.51ms | 18.87ms | 0.967× | 0.94% | PASS |
| file_reads | `oltp_order_range` | 3.57ms | 3.52ms | 0.987× | 1.40% | PASS |
| file_reads | `oltp_distinct_range` | 4.39ms | 4.40ms | 1.002× | 1.35% | PASS |
| file_reads | `oltp_index_scan` | 7.56ms | 6.91ms | 0.914× | 1.41% | PASS |
| file_reads | `select_random_points` | 29.79ms | 31.47ms | 1.056× | 1.41% | PASS |
| file_reads | `select_random_ranges` | 10.32ms | 9.47ms | 0.918× | 1.09% | PASS |
| file_reads | `covering_index_scan` | 7.01ms | 5.23ms | 0.747× | 1.61% | PASS |
| file_reads | `groupby_scan` | 33.68ms | 34.08ms | 1.012× | 0.97% | PASS |
| file_reads | `index_join` | 9.31ms | 11.06ms | 1.188× | 1.33% | PASS |
| file_reads | `index_join_scan` | 3.99ms | 5.43ms | 1.361× | 1.59% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.12s | 1.074× | 0.74% | PASS |
| file_reads | `table_scan` | 1.26s | 1.25s | 0.992× | 0.61% | PASS |
| file_reads | `oltp_read_only` | 186.21ms | 166.25ms | 0.893× | 1.08% | PASS |
| file_writes | `oltp_bulk_insert` | 228.07ms | 318.12ms | 1.395× | 0.92% | PASS |
| file_writes | `oltp_insert` | 21.84ms | 38.22ms | 1.750× | 1.69% | PASS |
| file_writes | `oltp_update_index` | 80.51ms | 125.12ms | 1.554× | 1.34% | PASS |
| file_writes | `oltp_update_non_index` | 60.56ms | 87.06ms | 1.438× | 1.16% | PASS |
| file_writes | `oltp_delete_insert` | 60.84ms | 100.72ms | 1.656× | 1.44% | PASS |
| file_writes | `oltp_write_only` | 37.92ms | 63.71ms | 1.680× | 1.99% | PASS |
| file_writes | `types_delete_insert` | 39.02ms | 56.82ms | 1.456× | 1.77% | PASS |
| file_writes | `oltp_read_write` | 106.77ms | 148.93ms | 1.395× | 1.54% | PASS |
| ac_reads | `oltp_point_select` | 40.35ms | 47.55ms | 1.178× | 1.97% | PASS |
| ac_reads | `oltp_range_select` | 18.13ms | 19.29ms | 1.064× | 0.88% | PASS |
| ac_reads | `oltp_sum_range` | 17.36ms | 19.02ms | 1.095× | 0.99% | PASS |
| ac_reads | `oltp_order_range` | 3.35ms | 3.59ms | 1.071× | 1.91% | PASS |
| ac_reads | `oltp_distinct_range` | 4.13ms | 4.38ms | 1.061× | 1.11% | PASS |
| ac_reads | `oltp_index_scan` | 5.38ms | 6.96ms | 1.294× | 1.18% | PASS |
| ac_reads | `select_random_points` | 26.70ms | 31.41ms | 1.176× | 1.07% | PASS |
| ac_reads | `select_random_ranges` | 7.95ms | 9.49ms | 1.194× | 1.32% | PASS |
| ac_reads | `covering_index_scan` | 4.87ms | 5.27ms | 1.082× | 1.26% | PASS |
| ac_reads | `groupby_scan` | 33.44ms | 34.12ms | 1.020× | 0.63% | PASS |
| ac_reads | `index_join` | 8.07ms | 10.95ms | 1.358× | 1.00% | PASS |
| ac_reads | `index_join_scan` | 3.69ms | 5.29ms | 1.433× | 1.14% | PASS |
| ac_reads | `types_table_scan` | 1.01s | 1.11s | 1.099× | 1.45% | PASS |
| ac_reads | `table_scan` | 1.25s | 1.24s | 0.997× | 0.83% | PASS |
| ac_reads | `oltp_read_only` | 148.09ms | 163.40ms | 1.103× | 1.01% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 18.45ms | 67.02ms | 3.633× | 7.26% | PASS |
| ac_writes | `oltp_insert_ac` | 20.16ms | 84.06ms | 4.171× | 6.33% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.77ms | 93.41ms | 4.291× | 5.73% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 18.16ms | 76.33ms | 4.203× | 7.17% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 20.11ms | 85.89ms | 4.271× | 6.13% | PASS |
| ac_writes | `oltp_write_only_ac` | 20.15ms | 85.27ms | 4.231× | 8.34% | PASS |
| ac_writes | `types_delete_insert_ac` | 18.21ms | 75.31ms | 4.136× | 5.96% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.23ms | 92.35ms | 3.660× | 7.21% | PASS |

</details>

## Version-control latency

Wall time: 2m 18s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 82.39ms | 200.00ms | 41.2% | 0.37% | PASS |
| `status_dirty_many_tables` | 85.47ms | 200.00ms | 42.7% | 0.26% | PASS |
| `diff_regular_working_one_table` | 78.04ms | 150.00ms | 52.0% | 0.38% | PASS |
| `diff_regular_working_many_tables` | 90.97ms | 200.00ms | 45.5% | 0.27% | PASS |
| `diff_stat_working_many_tables` | 91.01ms | 200.00ms | 45.5% | 0.28% | PASS |
| `diff_schema_working_many_tables` | 91.39ms | 200.00ms | 45.7% | 0.37% | PASS |
| `branch_list_many_branches` | 22.54ms | 100.00ms | 22.5% | 0.98% | PASS |
| `branch_create_delete` | 24.71ms | 100.00ms | 24.7% | 0.91% | PASS |
| `checkout_branch_clean` | 54.73ms | 200.00ms | 27.4% | 0.80% | PASS |
| `merge_data_no_conflicts` | 28.56ms | 150.00ms | 19.0% | 0.99% | PASS |
| `merge_schema_no_conflicts` | 22.01ms | 100.00ms | 22.0% | 0.96% | PASS |
| `merge_data_conflicts` | 126.58ms | 250.00ms | 50.6% | 0.27% | PASS |
| `merge_data_conflicts_with_resolve` | 126.63ms | 250.00ms | 50.7% | 0.24% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
