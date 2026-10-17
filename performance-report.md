# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-17 14:44 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261012.334.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/39035246034)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 18s | 8.84s | 11.36s | 1.285× | 1.30% | **PASS** |
| textpk | 69 | 55 | 1h 31m 1s | 9.26s | 10.17s | 1.097× | 1.41% | **PASS** |
| blobpk | 69 | 55 | 1h 34m 1s | 10.56s | 12.02s | 1.138× | 1.41% | **PASS** |
| compositepk | 69 | 55 | 1h 26m 56s | 9.54s | 11.92s | 1.250× | 1.35% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.78ms | 29.60ms | 1.245× | 1.40% | PASS |
| mem_reads | `oltp_range_select` | 10.13ms | 13.12ms | 1.295× | 1.48% | PASS |
| mem_reads | `oltp_sum_range` | 9.67ms | 12.36ms | 1.279× | 1.15% | PASS |
| mem_reads | `oltp_order_range` | 2.56ms | 2.99ms | 1.167× | 1.42% | PASS |
| mem_reads | `oltp_distinct_range` | 3.62ms | 4.03ms | 1.113× | 1.17% | PASS |
| mem_reads | `oltp_index_scan` | 3.93ms | 5.31ms | 1.351× | 1.48% | PASS |
| mem_reads | `select_random_points` | 9.92ms | 11.05ms | 1.114× | 1.25% | PASS |
| mem_reads | `select_random_ranges` | 2.95ms | 3.98ms | 1.348× | 1.97% | PASS |
| mem_reads | `covering_index_scan` | 4.24ms | 4.18ms | 0.985× | 1.80% | PASS |
| mem_reads | `groupby_scan` | 29.72ms | 33.27ms | 1.119× | 0.50% | PASS |
| mem_reads | `index_join` | 6.03ms | 8.10ms | 1.343× | 1.39% | PASS |
| mem_reads | `index_join_scan` | 3.46ms | 4.49ms | 1.298× | 1.34% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.33s | 1.282× | 0.49% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.40s | 1.184× | 0.86% | PASS |
| mem_reads | `oltp_read_only` | 102.89ms | 123.43ms | 1.200× | 1.07% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.07ms | 252.04ms | 1.392× | 1.03% | PASS |
| mem_writes | `oltp_insert` | 15.46ms | 28.27ms | 1.829× | 1.01% | PASS |
| mem_writes | `oltp_update_index` | 50.06ms | 104.98ms | 2.097× | 1.40% | PASS |
| mem_writes | `oltp_update_non_index` | 33.79ms | 59.67ms | 1.766× | 1.56% | PASS |
| mem_writes | `oltp_delete_insert` | 44.76ms | 78.81ms | 1.761× | 1.16% | PASS |
| mem_writes | `oltp_write_only` | 21.77ms | 44.77ms | 2.056× | 0.95% | PASS |
| mem_writes | `types_delete_insert` | 24.46ms | 40.35ms | 1.650× | 1.14% | PASS |
| mem_writes | `oltp_read_write` | 67.24ms | 110.42ms | 1.642× | 1.47% | PASS |
| file_reads | `oltp_point_select` | 98.50ms | 55.16ms | 0.560× | 0.79% | PASS |
| file_reads | `oltp_range_select` | 18.05ms | 15.84ms | 0.877× | 1.82% | PASS |
| file_reads | `oltp_sum_range` | 17.09ms | 15.03ms | 0.880× | 1.03% | PASS |
| file_reads | `oltp_order_range` | 3.38ms | 3.32ms | 0.981× | 1.50% | PASS |
| file_reads | `oltp_distinct_range` | 4.44ms | 4.36ms | 0.983× | 1.30% | PASS |
| file_reads | `oltp_index_scan` | 11.45ms | 8.19ms | 0.716× | 1.42% | PASS |
| file_reads | `select_random_points` | 17.45ms | 13.85ms | 0.793× | 2.15% | PASS |
| file_reads | `select_random_ranges` | 10.37ms | 6.59ms | 0.636× | 1.09% | PASS |
| file_reads | `covering_index_scan` | 11.82ms | 7.07ms | 0.598× | 0.87% | PASS |
| file_reads | `groupby_scan` | 30.64ms | 33.74ms | 1.101× | 0.90% | PASS |
| file_reads | `index_join` | 10.24ms | 10.10ms | 0.986× | 0.86% | PASS |
| file_reads | `index_join_scan` | 4.43ms | 4.89ms | 1.102× | 1.54% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.33s | 1.277× | 0.83% | PASS |
| file_reads | `table_scan` | 1.18s | 1.40s | 1.182× | 0.91% | PASS |
| file_reads | `oltp_read_only` | 212.17ms | 161.30ms | 0.760× | 1.06% | PASS |
| file_writes | `oltp_bulk_insert` | 194.10ms | 270.98ms | 1.396× | 1.08% | PASS |
| file_writes | `oltp_insert` | 21.94ms | 35.58ms | 1.622× | 1.17% | PASS |
| file_writes | `oltp_update_index` | 76.71ms | 127.50ms | 1.662× | 1.70% | PASS |
| file_writes | `oltp_update_non_index` | 57.40ms | 81.43ms | 1.419× | 1.33% | PASS |
| file_writes | `oltp_delete_insert` | 68.10ms | 99.11ms | 1.455× | 1.77% | PASS |
| file_writes | `oltp_write_only` | 44.22ms | 64.09ms | 1.449× | 1.72% | PASS |
| file_writes | `types_delete_insert` | 39.46ms | 53.59ms | 1.358× | 1.86% | PASS |
| file_writes | `oltp_read_write` | 91.67ms | 130.12ms | 1.420× | 1.86% | PASS |
| ac_reads | `oltp_point_select` | 48.71ms | 55.33ms | 1.136× | 0.80% | PASS |
| ac_reads | `oltp_range_select` | 13.13ms | 15.83ms | 1.206× | 1.17% | PASS |
| ac_reads | `oltp_sum_range` | 12.43ms | 15.05ms | 1.211× | 1.19% | PASS |
| ac_reads | `oltp_order_range` | 2.91ms | 3.28ms | 1.127× | 1.78% | PASS |
| ac_reads | `oltp_distinct_range` | 3.97ms | 4.35ms | 1.095× | 0.76% | PASS |
| ac_reads | `oltp_index_scan` | 6.67ms | 8.18ms | 1.227× | 1.60% | PASS |
| ac_reads | `select_random_points` | 12.95ms | 13.83ms | 1.068× | 1.59% | PASS |
| ac_reads | `select_random_ranges` | 5.53ms | 6.59ms | 1.192× | 0.96% | PASS |
| ac_reads | `covering_index_scan` | 7.05ms | 7.06ms | 1.002× | 1.20% | PASS |
| ac_reads | `groupby_scan` | 30.09ms | 33.67ms | 1.119× | 0.75% | PASS |
| ac_reads | `index_join` | 7.71ms | 10.12ms | 1.313× | 1.40% | PASS |
| ac_reads | `index_join_scan` | 3.92ms | 4.89ms | 1.248× | 1.10% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.33s | 1.279× | 0.62% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.39s | 1.178× | 1.17% | PASS |
| ac_reads | `oltp_read_only` | 139.96ms | 162.40ms | 1.160× | 0.91% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.73ms | 66.55ms | 3.062× | 3.99% | PASS |
| ac_writes | `oltp_insert_ac` | 26.26ms | 84.15ms | 3.204× | 5.97% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.32ms | 98.70ms | 3.750× | 4.09% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.32ms | 76.27ms | 3.418× | 5.23% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.05ms | 88.03ms | 3.660× | 3.50% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.23ms | 87.46ms | 3.466× | 5.93% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.72ms | 78.63ms | 3.620× | 4.17% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.57ms | 93.87ms | 3.175× | 3.23% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.73ms | 27.93ms | 1.129× | 2.54% | PASS |
| mem_reads | `oltp_range_select` | 11.49ms | 10.77ms | 0.937× | 1.01% | PASS |
| mem_reads | `oltp_sum_range` | 9.93ms | 10.46ms | 1.053× | 1.52% | PASS |
| mem_reads | `oltp_order_range` | 2.52ms | 2.51ms | 0.995× | 1.51% | PASS |
| mem_reads | `oltp_distinct_range` | 3.35ms | 3.29ms | 0.984× | 1.04% | PASS |
| mem_reads | `oltp_index_scan` | 3.63ms | 4.63ms | 1.273× | 1.28% | PASS |
| mem_reads | `select_random_points` | 14.11ms | 15.91ms | 1.127× | 1.04% | PASS |
| mem_reads | `select_random_ranges` | 3.30ms | 4.12ms | 1.248× | 1.28% | PASS |
| mem_reads | `covering_index_scan` | 3.76ms | 3.43ms | 0.910× | 1.73% | PASS |
| mem_reads | `groupby_scan` | 26.56ms | 27.73ms | 1.044× | 0.62% | PASS |
| mem_reads | `index_join` | 5.66ms | 6.94ms | 1.226× | 1.58% | PASS |
| mem_reads | `index_join_scan` | 4.09ms | 4.59ms | 1.121× | 1.19% | PASS |
| mem_reads | `types_table_scan` | 884.65ms | 967.27ms | 1.093× | 0.50% | PASS |
| mem_reads | `table_scan` | 1.08s | 1.06s | 0.983× | 3.12% | PASS |
| mem_reads | `oltp_read_only` | 103.41ms | 104.36ms | 1.009× | 2.35% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.02ms | 259.40ms | 1.433× | 0.93% | PASS |
| mem_writes | `oltp_insert` | 17.67ms | 29.88ms | 1.691× | 1.15% | PASS |
| mem_writes | `oltp_update_index` | 58.81ms | 105.09ms | 1.787× | 1.61% | PASS |
| mem_writes | `oltp_update_non_index` | 40.21ms | 66.73ms | 1.660× | 1.11% | PASS |
| mem_writes | `oltp_delete_insert` | 41.48ms | 80.43ms | 1.939× | 0.81% | PASS |
| mem_writes | `oltp_write_only` | 23.93ms | 47.99ms | 2.006× | 1.19% | PASS |
| mem_writes | `types_delete_insert` | 26.70ms | 40.97ms | 1.534× | 1.18% | PASS |
| mem_writes | `oltp_read_write` | 66.54ms | 103.61ms | 1.557× | 0.83% | PASS |
| file_reads | `oltp_point_select` | 98.68ms | 52.45ms | 0.532× | 0.75% | PASS |
| file_reads | `oltp_range_select` | 20.02ms | 13.38ms | 0.668× | 1.39% | PASS |
| file_reads | `oltp_sum_range` | 18.69ms | 13.38ms | 0.716× | 1.09% | PASS |
| file_reads | `oltp_order_range` | 3.54ms | 2.85ms | 0.806× | 1.41% | PASS |
| file_reads | `oltp_distinct_range` | 4.29ms | 3.60ms | 0.839× | 0.74% | PASS |
| file_reads | `oltp_index_scan` | 11.76ms | 7.60ms | 0.647× | 0.83% | PASS |
| file_reads | `select_random_points` | 23.04ms | 18.86ms | 0.819× | 0.89% | PASS |
| file_reads | `select_random_ranges` | 11.05ms | 6.72ms | 0.608× | 0.91% | PASS |
| file_reads | `covering_index_scan` | 12.60ms | 6.38ms | 0.506× | 0.75% | PASS |
| file_reads | `groupby_scan` | 27.78ms | 28.05ms | 1.010× | 0.63% | PASS |
| file_reads | `index_join` | 10.58ms | 9.24ms | 0.874× | 1.21% | PASS |
| file_reads | `index_join_scan` | 5.02ms | 5.06ms | 1.008× | 1.30% | PASS |
| file_reads | `types_table_scan` | 959.04ms | 985.92ms | 1.028× | 4.38% | PASS |
| file_reads | `table_scan` | 1.09s | 1.07s | 0.977× | 2.79% | PASS |
| file_reads | `oltp_read_only` | 206.03ms | 137.07ms | 0.665× | 1.37% | PASS |
| file_writes | `oltp_bulk_insert` | 265.74ms | 355.02ms | 1.336× | 9.52% | PASS |
| file_writes | `oltp_insert` | 120.40ms | 68.72ms | 0.571× | 37.06% | PASS |
| file_writes | `oltp_update_index` | 247.47ms | 220.22ms | 0.890× | 12.36% | PASS |
| file_writes | `oltp_update_non_index` | 189.16ms | 149.10ms | 0.788× | 16.39% | PASS |
| file_writes | `oltp_delete_insert` | 203.78ms | 175.14ms | 0.859× | 6.79% | PASS |
| file_writes | `oltp_write_only` | 173.30ms | 124.14ms | 0.716× | 21.17% | PASS |
| file_writes | `types_delete_insert` | 130.44ms | 110.45ms | 0.847× | 26.69% | PASS |
| file_writes | `oltp_read_write` | 209.95ms | 184.18ms | 0.877× | 18.78% | PASS |
| ac_reads | `oltp_point_select` | 50.01ms | 54.17ms | 1.083× | 1.29% | PASS |
| ac_reads | `oltp_range_select` | 16.33ms | 13.84ms | 0.848× | 1.80% | PASS |
| ac_reads | `oltp_sum_range` | 14.27ms | 13.73ms | 0.962× | 1.22% | PASS |
| ac_reads | `oltp_order_range` | 3.11ms | 2.87ms | 0.922× | 1.03% | PASS |
| ac_reads | `oltp_distinct_range` | 3.87ms | 3.63ms | 0.939× | 0.87% | PASS |
| ac_reads | `oltp_index_scan` | 6.86ms | 7.62ms | 1.111× | 1.46% | PASS |
| ac_reads | `select_random_points` | 19.20ms | 19.97ms | 1.040× | 2.42% | PASS |
| ac_reads | `select_random_ranges` | 6.25ms | 6.75ms | 1.080× | 1.23% | PASS |
| ac_reads | `covering_index_scan` | 7.78ms | 6.42ms | 0.826× | 1.66% | PASS |
| ac_reads | `groupby_scan` | 28.21ms | 28.59ms | 1.013× | 1.18% | PASS |
| ac_reads | `index_join` | 8.52ms | 9.48ms | 1.114× | 1.68% | PASS |
| ac_reads | `index_join_scan` | 4.70ms | 5.13ms | 1.091× | 1.22% | PASS |
| ac_reads | `types_table_scan` | 909.96ms | 971.88ms | 1.068× | 2.79% | PASS |
| ac_reads | `table_scan` | 1.10s | 1.07s | 0.976× | 3.78% | PASS |
| ac_reads | `oltp_read_only` | 139.52ms | 141.18ms | 1.012× | 2.58% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.58ms | 92.96ms | 4.307× | 65.38% | PASS |
| ac_writes | `oltp_insert_ac` | 24.71ms | 143.90ms | 5.823× | 65.01% | PASS |
| ac_writes | `oltp_update_index_ac` | 36.77ms | 147.08ms | 4.000× | 50.93% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 26.68ms | 113.22ms | 4.243× | 49.70% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 41.47ms | 186.01ms | 4.485× | 54.05% | PASS |
| ac_writes | `oltp_write_only_ac` | 36.73ms | 164.01ms | 4.466× | 50.28% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.78ms | 107.27ms | 4.511× | 66.23% | PASS |
| ac_writes | `oltp_read_write_ac` | 50.88ms | 117.83ms | 2.316× | 53.28% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.38ms | 38.20ms | 1.218× | 1.32% | PASS |
| mem_reads | `oltp_range_select` | 13.78ms | 14.38ms | 1.044× | 1.53% | PASS |
| mem_reads | `oltp_sum_range` | 13.11ms | 14.60ms | 1.114× | 1.55% | PASS |
| mem_reads | `oltp_order_range` | 3.03ms | 3.19ms | 1.051× | 0.94% | PASS |
| mem_reads | `oltp_distinct_range` | 4.08ms | 4.30ms | 1.053× | 0.69% | PASS |
| mem_reads | `oltp_index_scan` | 4.84ms | 6.64ms | 1.374× | 1.26% | PASS |
| mem_reads | `select_random_points` | 19.17ms | 21.98ms | 1.147× | 2.13% | PASS |
| mem_reads | `select_random_ranges` | 4.29ms | 5.27ms | 1.227× | 1.37% | PASS |
| mem_reads | `covering_index_scan` | 4.81ms | 4.86ms | 1.010× | 1.94% | PASS |
| mem_reads | `groupby_scan` | 32.50ms | 34.67ms | 1.067× | 0.96% | PASS |
| mem_reads | `index_join` | 7.35ms | 10.11ms | 1.375× | 1.64% | PASS |
| mem_reads | `index_join_scan` | 4.35ms | 5.64ms | 1.297× | 1.42% | PASS |
| mem_reads | `types_table_scan` | 1.17s | 1.27s | 1.087× | 1.41% | PASS |
| mem_reads | `table_scan` | 1.44s | 1.42s | 0.983× | 1.04% | PASS |
| mem_reads | `oltp_read_only` | 126.48ms | 140.38ms | 1.110× | 1.25% | PASS |
| mem_writes | `oltp_bulk_insert` | 243.65ms | 359.63ms | 1.476× | 1.17% | PASS |
| mem_writes | `oltp_insert` | 20.71ms | 40.48ms | 1.954× | 0.82% | PASS |
| mem_writes | `oltp_update_index` | 73.85ms | 138.86ms | 1.880× | 0.91% | PASS |
| mem_writes | `oltp_update_non_index` | 50.25ms | 86.64ms | 1.724× | 1.14% | PASS |
| mem_writes | `oltp_delete_insert` | 50.89ms | 105.79ms | 2.079× | 1.56% | PASS |
| mem_writes | `oltp_write_only` | 28.73ms | 62.93ms | 2.191× | 0.87% | PASS |
| mem_writes | `types_delete_insert` | 33.33ms | 55.59ms | 1.668× | 1.12% | PASS |
| mem_writes | `oltp_read_write` | 90.63ms | 144.69ms | 1.597× | 1.33% | PASS |
| file_reads | `oltp_point_select` | 108.17ms | 64.47ms | 0.596× | 0.91% | PASS |
| file_reads | `oltp_range_select` | 21.60ms | 17.17ms | 0.795× | 1.01% | PASS |
| file_reads | `oltp_sum_range` | 20.64ms | 17.09ms | 0.828× | 0.92% | PASS |
| file_reads | `oltp_order_range` | 3.88ms | 3.53ms | 0.910× | 1.81% | PASS |
| file_reads | `oltp_distinct_range` | 5.02ms | 4.71ms | 0.938× | 1.59% | PASS |
| file_reads | `oltp_index_scan` | 12.71ms | 9.33ms | 0.734× | 1.31% | PASS |
| file_reads | `select_random_points` | 28.32ms | 25.22ms | 0.891× | 2.02% | PASS |
| file_reads | `select_random_ranges` | 11.97ms | 7.91ms | 0.661× | 0.78% | PASS |
| file_reads | `covering_index_scan` | 12.97ms | 7.43ms | 0.573× | 1.35% | PASS |
| file_reads | `groupby_scan` | 33.37ms | 35.05ms | 1.050× | 0.76% | PASS |
| file_reads | `index_join` | 11.91ms | 11.63ms | 0.977× | 1.42% | PASS |
| file_reads | `index_join_scan` | 5.51ms | 6.17ms | 1.120× | 1.89% | PASS |
| file_reads | `types_table_scan` | 1.18s | 1.26s | 1.072× | 1.46% | PASS |
| file_reads | `table_scan` | 1.47s | 1.42s | 0.968× | 0.81% | PASS |
| file_reads | `oltp_read_only` | 244.35ms | 180.79ms | 0.740× | 1.10% | PASS |
| file_writes | `oltp_bulk_insert` | 263.74ms | 388.33ms | 1.472× | 0.85% | PASS |
| file_writes | `oltp_insert` | 33.99ms | 53.84ms | 1.584× | 1.86% | PASS |
| file_writes | `oltp_update_index` | 115.76ms | 175.56ms | 1.517× | 2.54% | PASS |
| file_writes | `oltp_update_non_index` | 84.62ms | 113.74ms | 1.344× | 1.73% | PASS |
| file_writes | `oltp_delete_insert` | 89.69ms | 138.50ms | 1.544× | 2.29% | PASS |
| file_writes | `oltp_write_only` | 60.39ms | 88.19ms | 1.460× | 2.33% | PASS |
| file_writes | `types_delete_insert` | 56.14ms | 76.04ms | 1.354× | 1.30% | PASS |
| file_writes | `oltp_read_write` | 123.04ms | 167.03ms | 1.358× | 1.62% | PASS |
| ac_reads | `oltp_point_select` | 57.41ms | 64.92ms | 1.131× | 1.28% | PASS |
| ac_reads | `oltp_range_select` | 17.11ms | 17.41ms | 1.017× | 1.41% | PASS |
| ac_reads | `oltp_sum_range` | 15.89ms | 17.40ms | 1.095× | 1.13% | PASS |
| ac_reads | `oltp_order_range` | 3.49ms | 3.59ms | 1.029× | 1.93% | PASS |
| ac_reads | `oltp_distinct_range` | 4.57ms | 4.71ms | 1.030× | 1.70% | PASS |
| ac_reads | `oltp_index_scan` | 7.80ms | 9.38ms | 1.203× | 1.17% | PASS |
| ac_reads | `select_random_points` | 23.12ms | 25.47ms | 1.102× | 2.21% | PASS |
| ac_reads | `select_random_ranges` | 7.00ms | 7.95ms | 1.136× | 1.06% | PASS |
| ac_reads | `covering_index_scan` | 8.00ms | 7.44ms | 0.930× | 0.91% | PASS |
| ac_reads | `groupby_scan` | 32.85ms | 35.18ms | 1.071× | 0.81% | PASS |
| ac_reads | `index_join` | 9.38ms | 11.74ms | 1.251× | 1.58% | PASS |
| ac_reads | `index_join_scan` | 4.97ms | 6.17ms | 1.239× | 1.33% | PASS |
| ac_reads | `types_table_scan` | 1.15s | 1.25s | 1.089× | 1.91% | PASS |
| ac_reads | `table_scan` | 1.38s | 1.39s | 1.012× | 1.81% | PASS |
| ac_reads | `oltp_read_only` | 165.48ms | 179.52ms | 1.085× | 1.18% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.79ms | 70.18ms | 2.951× | 5.61% | PASS |
| ac_writes | `oltp_insert_ac` | 26.21ms | 91.25ms | 3.481× | 5.23% | PASS |
| ac_writes | `oltp_update_index_ac` | 30.22ms | 104.48ms | 3.457× | 7.09% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.10ms | 83.61ms | 3.331× | 6.46% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.35ms | 93.85ms | 3.702× | 6.79% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.62ms | 93.39ms | 3.381× | 5.91% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.68ms | 82.95ms | 3.361× | 9.29% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.47ms | 99.95ms | 3.177× | 4.94% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.88ms | 40.56ms | 1.234× | 1.44% | PASS |
| mem_reads | `oltp_range_select` | 18.84ms | 21.13ms | 1.121× | 1.59% | PASS |
| mem_reads | `oltp_sum_range` | 17.94ms | 20.98ms | 1.169× | 1.26% | PASS |
| mem_reads | `oltp_order_range` | 3.52ms | 3.85ms | 1.095× | 1.61% | PASS |
| mem_reads | `oltp_distinct_range` | 4.63ms | 4.91ms | 1.060× | 1.12% | PASS |
| mem_reads | `oltp_index_scan` | 4.55ms | 6.18ms | 1.356× | 1.84% | PASS |
| mem_reads | `select_random_points` | 28.13ms | 32.24ms | 1.146× | 1.63% | PASS |
| mem_reads | `select_random_ranges` | 7.60ms | 9.08ms | 1.194× | 1.68% | PASS |
| mem_reads | `covering_index_scan` | 4.26ms | 4.25ms | 0.997× | 1.67% | PASS |
| mem_reads | `groupby_scan` | 36.33ms | 39.34ms | 1.083× | 1.15% | PASS |
| mem_reads | `index_join` | 8.25ms | 10.45ms | 1.266× | 1.37% | PASS |
| mem_reads | `index_join_scan` | 4.24ms | 5.57ms | 1.314× | 1.32% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.24s | 1.186× | 0.76% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.39s | 1.173× | 0.74% | PASS |
| mem_reads | `oltp_read_only` | 148.89ms | 169.18ms | 1.136× | 0.79% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.16ms | 356.84ms | 1.432× | 1.11% | PASS |
| mem_writes | `oltp_insert` | 19.28ms | 36.55ms | 1.896× | 0.73% | PASS |
| mem_writes | `oltp_update_index` | 68.69ms | 116.15ms | 1.691× | 1.45% | PASS |
| mem_writes | `oltp_update_non_index` | 50.99ms | 82.48ms | 1.618× | 1.06% | PASS |
| mem_writes | `oltp_delete_insert` | 49.79ms | 95.39ms | 1.916× | 0.94% | PASS |
| mem_writes | `oltp_write_only` | 27.12ms | 57.09ms | 2.105× | 1.13% | PASS |
| mem_writes | `types_delete_insert` | 32.41ms | 54.48ms | 1.681× | 0.91% | PASS |
| mem_writes | `oltp_read_write` | 100.86ms | 154.61ms | 1.533× | 1.16% | PASS |
| file_reads | `oltp_point_select` | 108.50ms | 66.90ms | 0.617× | 0.87% | PASS |
| file_reads | `oltp_range_select` | 26.45ms | 24.34ms | 0.920× | 1.48% | PASS |
| file_reads | `oltp_sum_range` | 25.78ms | 24.20ms | 0.939× | 1.36% | PASS |
| file_reads | `oltp_order_range` | 4.40ms | 4.33ms | 0.984× | 2.31% | PASS |
| file_reads | `oltp_distinct_range` | 5.60ms | 5.40ms | 0.964× | 1.77% | PASS |
| file_reads | `oltp_index_scan` | 12.13ms | 9.18ms | 0.757× | 1.34% | PASS |
| file_reads | `select_random_points` | 37.40ms | 36.23ms | 0.969× | 1.58% | PASS |
| file_reads | `select_random_ranges` | 15.52ms | 12.15ms | 0.783× | 1.35% | PASS |
| file_reads | `covering_index_scan` | 11.87ms | 7.24ms | 0.610× | 1.13% | PASS |
| file_reads | `groupby_scan` | 37.20ms | 40.06ms | 1.077× | 0.88% | PASS |
| file_reads | `index_join` | 12.66ms | 12.98ms | 1.025× | 1.03% | PASS |
| file_reads | `index_join_scan` | 5.27ms | 6.10ms | 1.158× | 1.86% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.23s | 1.187× | 0.61% | PASS |
| file_reads | `table_scan` | 1.19s | 1.39s | 1.166× | 1.10% | PASS |
| file_reads | `oltp_read_only` | 262.61ms | 210.06ms | 0.800× | 0.95% | PASS |
| file_writes | `oltp_bulk_insert` | 264.77ms | 380.05ms | 1.435× | 0.82% | PASS |
| file_writes | `oltp_insert` | 26.61ms | 46.46ms | 1.746× | 1.77% | PASS |
| file_writes | `oltp_update_index` | 97.72ms | 143.74ms | 1.471× | 1.29% | PASS |
| file_writes | `oltp_update_non_index` | 77.60ms | 104.52ms | 1.347× | 1.50% | PASS |
| file_writes | `oltp_delete_insert` | 77.94ms | 120.47ms | 1.546× | 1.24% | PASS |
| file_writes | `oltp_write_only` | 51.98ms | 78.61ms | 1.512× | 1.61% | PASS |
| file_writes | `types_delete_insert` | 49.98ms | 68.90ms | 1.379× | 1.68% | PASS |
| file_writes | `oltp_read_write` | 127.29ms | 175.74ms | 1.381× | 1.42% | PASS |
| ac_reads | `oltp_point_select` | 57.98ms | 66.47ms | 1.146× | 1.03% | PASS |
| ac_reads | `oltp_range_select` | 21.80ms | 24.30ms | 1.114× | 1.16% | PASS |
| ac_reads | `oltp_sum_range` | 20.95ms | 24.21ms | 1.156× | 1.47% | PASS |
| ac_reads | `oltp_order_range` | 4.03ms | 4.36ms | 1.082× | 1.28% | PASS |
| ac_reads | `oltp_distinct_range` | 5.11ms | 5.46ms | 1.068× | 1.30% | PASS |
| ac_reads | `oltp_index_scan` | 7.27ms | 9.22ms | 1.267× | 1.56% | PASS |
| ac_reads | `select_random_points` | 31.66ms | 36.42ms | 1.151× | 1.35% | PASS |
| ac_reads | `select_random_ranges` | 10.49ms | 12.12ms | 1.155× | 1.20% | PASS |
| ac_reads | `covering_index_scan` | 7.10ms | 7.25ms | 1.021× | 1.64% | PASS |
| ac_reads | `groupby_scan` | 36.86ms | 40.09ms | 1.088× | 0.91% | PASS |
| ac_reads | `index_join` | 10.00ms | 12.99ms | 1.298× | 1.90% | PASS |
| ac_reads | `index_join_scan` | 4.72ms | 6.10ms | 1.290× | 2.07% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.23s | 1.189× | 0.44% | PASS |
| ac_reads | `table_scan` | 1.19s | 1.39s | 1.169× | 0.69% | PASS |
| ac_reads | `oltp_read_only` | 186.72ms | 209.39ms | 1.121× | 0.86% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.20ms | 71.06ms | 3.064× | 5.19% | PASS |
| ac_writes | `oltp_insert_ac` | 25.09ms | 90.07ms | 3.591× | 4.60% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.89ms | 101.40ms | 3.771× | 4.95% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.76ms | 80.97ms | 3.408× | 8.30% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.12ms | 91.13ms | 3.628× | 9.04% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.31ms | 89.78ms | 3.548× | 5.38% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.73ms | 84.31ms | 3.553× | 7.76% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.45ms | 99.84ms | 2.984× | 5.58% | PASS |

</details>

## Version-control latency

Wall time: 2m 17s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 82.01ms | 200.00ms | 41.0% | 0.49% | PASS |
| `status_dirty_many_tables` | 85.21ms | 200.00ms | 42.6% | 0.38% | PASS |
| `diff_regular_working_one_table` | 77.83ms | 150.00ms | 51.9% | 0.41% | PASS |
| `diff_regular_working_many_tables` | 90.72ms | 200.00ms | 45.4% | 0.32% | PASS |
| `diff_stat_working_many_tables` | 90.80ms | 200.00ms | 45.4% | 0.34% | PASS |
| `diff_schema_working_many_tables` | 91.19ms | 200.00ms | 45.6% | 0.29% | PASS |
| `branch_list_many_branches` | 22.45ms | 100.00ms | 22.4% | 1.09% | PASS |
| `branch_create_delete` | 24.26ms | 100.00ms | 24.3% | 0.79% | PASS |
| `checkout_branch_clean` | 53.96ms | 200.00ms | 27.0% | 0.47% | PASS |
| `merge_data_no_conflicts` | 28.09ms | 150.00ms | 18.7% | 0.74% | PASS |
| `merge_schema_no_conflicts` | 21.59ms | 100.00ms | 21.6% | 1.29% | PASS |
| `merge_data_conflicts` | 126.01ms | 250.00ms | 50.4% | 0.27% | PASS |
| `merge_data_conflicts_with_resolve` | 126.33ms | 250.00ms | 50.5% | 0.36% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
