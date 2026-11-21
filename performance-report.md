# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-21 18:26 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu26 20261116.88.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/44927094028)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 57m 38s | 7.09s | 7.81s | 1.102× | 2.70% | **PASS** |
| textpk | 69 | 55 | 1h 28m 45s | 9.48s | 10.92s | 1.152× | 1.33% | **PASS** |
| blobpk | 69 | 55 | 1h 28m 18s | 9.66s | 11.21s | 1.160× | 1.24% | **PASS** |
| compositepk | 69 | 55 | 1h 28m 31s | 10.31s | 12.01s | 1.165× | 1.66% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 18.05ms | 18.26ms | 1.011× | 2.70% | PASS |
| mem_reads | `oltp_range_select` | 6.64ms | 7.13ms | 1.073× | 2.94% | PASS |
| mem_reads | `oltp_sum_range` | 6.35ms | 7.43ms | 1.170× | 1.98% | PASS |
| mem_reads | `oltp_order_range` | 1.74ms | 1.80ms | 1.038× | 2.26% | PASS |
| mem_reads | `oltp_distinct_range` | 2.23ms | 2.29ms | 1.031× | 1.78% | PASS |
| mem_reads | `oltp_index_scan` | 2.62ms | 3.27ms | 1.245× | 2.89% | PASS |
| mem_reads | `select_random_points` | 10.41ms | 9.25ms | 0.889× | 2.31% | PASS |
| mem_reads | `select_random_ranges` | 2.25ms | 2.57ms | 1.141× | 3.24% | PASS |
| mem_reads | `covering_index_scan` | 2.32ms | 2.59ms | 1.120× | 6.81% | PASS |
| mem_reads | `groupby_scan` | 18.95ms | 19.98ms | 1.055× | 1.83% | PASS |
| mem_reads | `index_join` | 3.77ms | 5.86ms | 1.555× | 5.63% | PASS |
| mem_reads | `index_join_scan` | 2.64ms | 4.01ms | 1.519× | 4.17% | PASS |
| mem_reads | `types_table_scan` | 746.92ms | 802.57ms | 1.075× | 1.58% | PASS |
| mem_reads | `table_scan` | 907.07ms | 900.40ms | 0.993× | 2.45% | PASS |
| mem_reads | `oltp_read_only` | 76.83ms | 77.74ms | 1.012× | 1.67% | PASS |
| mem_writes | `oltp_bulk_insert` | 111.19ms | 162.04ms | 1.457× | 1.41% | PASS |
| mem_writes | `oltp_insert` | 9.76ms | 19.49ms | 1.997× | 1.80% | PASS |
| mem_writes | `oltp_update_index` | 40.63ms | 91.75ms | 2.258× | 3.53% | PASS |
| mem_writes | `oltp_update_non_index` | 29.63ms | 50.40ms | 1.701× | 2.24% | PASS |
| mem_writes | `oltp_delete_insert` | 36.35ms | 67.29ms | 1.851× | 3.06% | PASS |
| mem_writes | `oltp_write_only` | 17.72ms | 38.90ms | 2.194× | 2.46% | PASS |
| mem_writes | `types_delete_insert` | 19.69ms | 32.67ms | 1.659× | 2.04% | PASS |
| mem_writes | `oltp_read_write` | 54.98ms | 81.16ms | 1.476× | 2.83% | PASS |
| file_reads | `oltp_point_select` | 75.19ms | 38.72ms | 0.515× | 1.22% | PASS |
| file_reads | `oltp_range_select` | 13.31ms | 9.76ms | 0.733× | 2.25% | PASS |
| file_reads | `oltp_sum_range` | 12.80ms | 9.94ms | 0.776× | 1.81% | PASS |
| file_reads | `oltp_order_range` | 2.49ms | 2.12ms | 0.851× | 2.23% | PASS |
| file_reads | `oltp_distinct_range` | 3.27ms | 2.88ms | 0.880× | 1.47% | PASS |
| file_reads | `oltp_index_scan` | 9.88ms | 6.55ms | 0.664× | 2.13% | PASS |
| file_reads | `select_random_points` | 19.54ms | 13.33ms | 0.682× | 2.18% | PASS |
| file_reads | `select_random_ranges` | 8.96ms | 5.17ms | 0.577× | 0.97% | PASS |
| file_reads | `covering_index_scan` | 9.34ms | 5.53ms | 0.592× | 2.14% | PASS |
| file_reads | `groupby_scan` | 20.90ms | 21.43ms | 1.026× | 1.20% | PASS |
| file_reads | `index_join` | 7.96ms | 8.04ms | 1.011× | 5.25% | PASS |
| file_reads | `index_join_scan` | 3.55ms | 4.71ms | 1.326× | 10.21% | PASS |
| file_reads | `types_table_scan` | 769.99ms | 823.60ms | 1.070× | 1.92% | PASS |
| file_reads | `table_scan` | 922.46ms | 908.42ms | 0.985× | 1.04% | PASS |
| file_reads | `oltp_read_only` | 161.49ms | 107.49ms | 0.666× | 1.56% | PASS |
| file_writes | `oltp_bulk_insert` | 152.39ms | 218.83ms | 1.436× | 3.47% | PASS |
| file_writes | `oltp_insert` | 19.78ms | 30.79ms | 1.557× | 11.81% | PASS |
| file_writes | `oltp_update_index` | 135.55ms | 148.93ms | 1.099× | 3.47% | PASS |
| file_writes | `oltp_update_non_index` | 108.93ms | 106.77ms | 0.980× | 3.67% | PASS |
| file_writes | `oltp_delete_insert` | 111.94ms | 118.78ms | 1.061× | 2.75% | PASS |
| file_writes | `oltp_write_only` | 81.31ms | 78.68ms | 0.968× | 3.49% | PASS |
| file_writes | `types_delete_insert` | 64.74ms | 59.80ms | 0.924× | 8.78% | PASS |
| file_writes | `oltp_read_write` | 122.43ms | 118.71ms | 0.970× | 4.56% | PASS |
| ac_reads | `oltp_point_select` | 36.01ms | 37.03ms | 1.028× | 2.29% | PASS |
| ac_reads | `oltp_range_select` | 9.28ms | 9.47ms | 1.021× | 2.90% | PASS |
| ac_reads | `oltp_sum_range` | 8.91ms | 9.74ms | 1.093× | 1.85% | PASS |
| ac_reads | `oltp_order_range` | 2.08ms | 2.08ms | 1.000× | 2.88% | PASS |
| ac_reads | `oltp_distinct_range` | 2.64ms | 2.66ms | 1.009× | 2.49% | PASS |
| ac_reads | `oltp_index_scan` | 5.24ms | 5.92ms | 1.129× | 2.48% | PASS |
| ac_reads | `select_random_points` | 13.46ms | 11.70ms | 0.869× | 4.84% | PASS |
| ac_reads | `select_random_ranges` | 4.28ms | 4.59ms | 1.074× | 2.37% | PASS |
| ac_reads | `covering_index_scan` | 5.04ms | 5.13ms | 1.018× | 3.05% | PASS |
| ac_reads | `groupby_scan` | 19.79ms | 20.78ms | 1.050× | 2.51% | PASS |
| ac_reads | `index_join` | 5.45ms | 7.16ms | 1.314× | 4.04% | PASS |
| ac_reads | `index_join_scan` | 2.96ms | 4.20ms | 1.422× | 5.94% | PASS |
| ac_reads | `types_table_scan` | 783.02ms | 847.97ms | 1.083× | 2.79% | PASS |
| ac_reads | `table_scan` | 948.02ms | 935.58ms | 0.987× | 1.68% | PASS |
| ac_reads | `oltp_read_only` | 101.06ms | 103.74ms | 1.027× | 2.37% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.12ms | 53.63ms | 2.666× | 4.06% | PASS |
| ac_writes | `oltp_insert_ac` | 21.46ms | 66.32ms | 3.090× | 4.39% | PASS |
| ac_writes | `oltp_update_index_ac` | 23.24ms | 77.13ms | 3.319× | 3.63% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.50ms | 61.45ms | 2.998× | 5.97% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 21.72ms | 70.76ms | 3.257× | 5.04% | PASS |
| ac_writes | `oltp_write_only_ac` | 21.73ms | 72.71ms | 3.346× | 4.77% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.03ms | 68.56ms | 3.424× | 6.38% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.01ms | 76.39ms | 3.054× | 4.30% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.64ms | 29.82ms | 1.163× | 1.49% | PASS |
| mem_reads | `oltp_range_select` | 11.86ms | 12.30ms | 1.038× | 1.34% | PASS |
| mem_reads | `oltp_sum_range` | 11.64ms | 11.87ms | 1.020× | 1.16% | PASS |
| mem_reads | `oltp_order_range` | 2.71ms | 2.88ms | 1.062× | 1.10% | PASS |
| mem_reads | `oltp_distinct_range` | 3.56ms | 3.81ms | 1.069× | 0.71% | PASS |
| mem_reads | `oltp_index_scan` | 4.00ms | 5.06ms | 1.266× | 1.70% | PASS |
| mem_reads | `select_random_points` | 16.22ms | 18.69ms | 1.152× | 1.23% | PASS |
| mem_reads | `select_random_ranges` | 3.33ms | 4.44ms | 1.333× | 1.50% | PASS |
| mem_reads | `covering_index_scan` | 3.73ms | 3.68ms | 0.986× | 1.44% | PASS |
| mem_reads | `groupby_scan` | 32.28ms | 32.68ms | 1.012× | 0.54% | PASS |
| mem_reads | `index_join` | 6.45ms | 8.10ms | 1.257× | 1.15% | PASS |
| mem_reads | `index_join_scan` | 4.08ms | 4.96ms | 1.213× | 2.95% | PASS |
| mem_reads | `types_table_scan` | 1.02s | 1.15s | 1.128× | 0.28% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.24s | 1.051× | 0.30% | PASS |
| mem_reads | `oltp_read_only` | 102.89ms | 115.53ms | 1.123× | 0.67% | PASS |
| mem_writes | `oltp_bulk_insert` | 176.69ms | 272.56ms | 1.543× | 0.50% | PASS |
| mem_writes | `oltp_insert` | 18.09ms | 32.37ms | 1.789× | 0.64% | PASS |
| mem_writes | `oltp_update_index` | 60.52ms | 115.61ms | 1.910× | 1.12% | PASS |
| mem_writes | `oltp_update_non_index` | 39.88ms | 72.93ms | 1.829× | 0.92% | PASS |
| mem_writes | `oltp_delete_insert` | 41.99ms | 87.30ms | 2.079× | 0.80% | PASS |
| mem_writes | `oltp_write_only` | 23.42ms | 52.36ms | 2.236× | 1.18% | PASS |
| mem_writes | `types_delete_insert` | 26.33ms | 46.97ms | 1.784× | 1.14% | PASS |
| mem_writes | `oltp_read_write` | 68.50ms | 114.40ms | 1.670× | 0.67% | PASS |
| file_reads | `oltp_point_select` | 53.91ms | 39.05ms | 0.724× | 0.92% | PASS |
| file_reads | `oltp_range_select` | 15.02ms | 13.49ms | 0.899× | 1.88% | PASS |
| file_reads | `oltp_sum_range` | 14.44ms | 12.88ms | 0.892× | 2.15% | PASS |
| file_reads | `oltp_order_range` | 3.07ms | 3.04ms | 0.991× | 1.61% | PASS |
| file_reads | `oltp_distinct_range` | 3.95ms | 3.97ms | 1.005× | 1.07% | PASS |
| file_reads | `oltp_index_scan` | 7.09ms | 6.36ms | 0.897× | 2.07% | PASS |
| file_reads | `select_random_points` | 19.36ms | 19.91ms | 1.029× | 1.27% | PASS |
| file_reads | `select_random_ranges` | 6.29ms | 5.41ms | 0.859× | 1.55% | PASS |
| file_reads | `covering_index_scan` | 6.88ms | 4.82ms | 0.701× | 2.32% | PASS |
| file_reads | `groupby_scan` | 32.51ms | 32.85ms | 1.010× | 1.21% | PASS |
| file_reads | `index_join` | 8.55ms | 9.45ms | 1.105× | 3.04% | PASS |
| file_reads | `index_join_scan` | 4.75ms | 5.42ms | 1.143× | 1.49% | PASS |
| file_reads | `types_table_scan` | 1.02s | 1.15s | 1.131× | 0.26% | PASS |
| file_reads | `table_scan` | 1.19s | 1.25s | 1.050× | 0.46% | PASS |
| file_reads | `oltp_read_only` | 144.42ms | 128.99ms | 0.893× | 0.78% | PASS |
| file_writes | `oltp_bulk_insert` | 248.61ms | 363.70ms | 1.463× | 2.15% | PASS |
| file_writes | `oltp_insert` | 80.85ms | 61.73ms | 0.764× | 5.52% | PASS |
| file_writes | `oltp_update_index` | 220.37ms | 222.14ms | 1.008× | 3.54% | PASS |
| file_writes | `oltp_update_non_index` | 167.25ms | 151.80ms | 0.908× | 2.35% | PASS |
| file_writes | `oltp_delete_insert` | 180.87ms | 178.78ms | 0.988× | 4.82% | PASS |
| file_writes | `oltp_write_only` | 140.87ms | 119.11ms | 0.846× | 5.68% | PASS |
| file_writes | `types_delete_insert` | 101.84ms | 98.06ms | 0.963× | 4.58% | PASS |
| file_writes | `oltp_read_write` | 184.76ms | 180.16ms | 0.975× | 3.46% | PASS |
| ac_reads | `oltp_point_select` | 34.75ms | 39.16ms | 1.127× | 0.78% | PASS |
| ac_reads | `oltp_range_select` | 13.39ms | 13.56ms | 1.012× | 0.69% | PASS |
| ac_reads | `oltp_sum_range` | 13.05ms | 12.93ms | 0.990× | 1.02% | PASS |
| ac_reads | `oltp_order_range` | 2.97ms | 3.06ms | 1.030× | 1.55% | PASS |
| ac_reads | `oltp_distinct_range` | 3.80ms | 3.99ms | 1.049× | 1.03% | PASS |
| ac_reads | `oltp_index_scan` | 5.56ms | 6.50ms | 1.168× | 1.90% | PASS |
| ac_reads | `select_random_points` | 18.27ms | 20.39ms | 1.116× | 0.98% | PASS |
| ac_reads | `select_random_ranges` | 4.59ms | 5.53ms | 1.206× | 1.35% | PASS |
| ac_reads | `covering_index_scan` | 5.68ms | 4.99ms | 0.878× | 2.96% | PASS |
| ac_reads | `groupby_scan` | 32.97ms | 33.05ms | 1.002× | 0.62% | PASS |
| ac_reads | `index_join` | 8.04ms | 9.63ms | 1.198× | 0.99% | PASS |
| ac_reads | `index_join_scan` | 4.64ms | 5.40ms | 1.163× | 1.33% | PASS |
| ac_reads | `types_table_scan` | 1.02s | 1.15s | 1.132× | 0.29% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.25s | 1.054× | 0.43% | PASS |
| ac_reads | `oltp_read_only` | 116.31ms | 128.53ms | 1.105× | 0.53% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 31.82ms | 75.75ms | 2.381× | 7.22% | PASS |
| ac_writes | `oltp_insert_ac` | 36.27ms | 87.92ms | 2.424× | 6.83% | PASS |
| ac_writes | `oltp_update_index_ac` | 37.53ms | 102.48ms | 2.730× | 6.41% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 31.60ms | 85.32ms | 2.700× | 7.22% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 35.13ms | 95.06ms | 2.706× | 4.21% | PASS |
| ac_writes | `oltp_write_only_ac` | 35.03ms | 93.38ms | 2.666× | 4.31% | PASS |
| ac_writes | `types_delete_insert_ac` | 27.59ms | 87.31ms | 3.165× | 5.44% | PASS |
| ac_writes | `oltp_read_write_ac` | 39.07ms | 100.62ms | 2.576× | 4.47% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.74ms | 30.38ms | 1.180× | 1.24% | PASS |
| mem_reads | `oltp_range_select` | 11.63ms | 12.49ms | 1.074× | 1.29% | PASS |
| mem_reads | `oltp_sum_range` | 11.27ms | 11.74ms | 1.042× | 1.11% | PASS |
| mem_reads | `oltp_order_range` | 2.67ms | 2.87ms | 1.075× | 1.31% | PASS |
| mem_reads | `oltp_distinct_range` | 3.58ms | 3.86ms | 1.079× | 1.09% | PASS |
| mem_reads | `oltp_index_scan` | 3.99ms | 5.23ms | 1.312× | 1.32% | PASS |
| mem_reads | `select_random_points` | 16.32ms | 18.87ms | 1.156× | 1.05% | PASS |
| mem_reads | `select_random_ranges` | 3.30ms | 4.48ms | 1.358× | 1.18% | PASS |
| mem_reads | `covering_index_scan` | 3.53ms | 3.84ms | 1.088× | 1.83% | PASS |
| mem_reads | `groupby_scan` | 32.28ms | 32.95ms | 1.021× | 0.74% | PASS |
| mem_reads | `index_join` | 6.21ms | 8.42ms | 1.356× | 1.66% | PASS |
| mem_reads | `index_join_scan` | 3.71ms | 5.11ms | 1.377× | 1.49% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.19s | 1.131× | 0.69% | PASS |
| mem_reads | `table_scan` | 1.25s | 1.29s | 1.033× | 2.03% | PASS |
| mem_reads | `oltp_read_only` | 107.19ms | 120.40ms | 1.123× | 0.77% | PASS |
| mem_writes | `oltp_bulk_insert` | 186.99ms | 279.24ms | 1.493× | 0.57% | PASS |
| mem_writes | `oltp_insert` | 16.88ms | 34.01ms | 2.015× | 0.53% | PASS |
| mem_writes | `oltp_update_index` | 59.58ms | 121.05ms | 2.032× | 1.04% | PASS |
| mem_writes | `oltp_update_non_index` | 40.95ms | 75.33ms | 1.840× | 0.72% | PASS |
| mem_writes | `oltp_delete_insert` | 42.23ms | 93.79ms | 2.221× | 0.92% | PASS |
| mem_writes | `oltp_write_only` | 23.38ms | 55.51ms | 2.375× | 1.24% | PASS |
| mem_writes | `types_delete_insert` | 26.64ms | 48.63ms | 1.825× | 1.04% | PASS |
| mem_writes | `oltp_read_write` | 70.61ms | 120.86ms | 1.712× | 1.55% | PASS |
| file_reads | `oltp_point_select` | 55.82ms | 40.07ms | 0.718× | 0.80% | PASS |
| file_reads | `oltp_range_select` | 15.16ms | 13.82ms | 0.911× | 0.90% | PASS |
| file_reads | `oltp_sum_range` | 15.11ms | 13.14ms | 0.870× | 1.52% | PASS |
| file_reads | `oltp_order_range` | 3.20ms | 3.13ms | 0.980× | 1.59% | PASS |
| file_reads | `oltp_distinct_range` | 4.09ms | 4.09ms | 0.999× | 0.88% | PASS |
| file_reads | `oltp_index_scan` | 7.45ms | 6.54ms | 0.879× | 1.56% | PASS |
| file_reads | `select_random_points` | 20.49ms | 20.61ms | 1.006× | 0.78% | PASS |
| file_reads | `select_random_ranges` | 6.65ms | 5.65ms | 0.850× | 1.12% | PASS |
| file_reads | `covering_index_scan` | 7.25ms | 5.13ms | 0.707× | 1.21% | PASS |
| file_reads | `groupby_scan` | 33.46ms | 33.65ms | 1.006× | 0.52% | PASS |
| file_reads | `index_join` | 8.45ms | 9.76ms | 1.155× | 1.44% | PASS |
| file_reads | `index_join_scan` | 4.29ms | 5.49ms | 1.281× | 1.73% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.19s | 1.132× | 0.79% | PASS |
| file_reads | `table_scan` | 1.26s | 1.29s | 1.029× | 3.36% | PASS |
| file_reads | `oltp_read_only` | 153.18ms | 133.80ms | 0.873× | 1.32% | PASS |
| file_writes | `oltp_bulk_insert` | 240.25ms | 359.90ms | 1.498× | 3.13% | PASS |
| file_writes | `oltp_insert` | 59.68ms | 58.52ms | 0.981× | 5.97% | PASS |
| file_writes | `oltp_update_index` | 190.66ms | 209.51ms | 1.099× | 1.54% | PASS |
| file_writes | `oltp_update_non_index` | 149.73ms | 138.92ms | 0.928× | 3.33% | PASS |
| file_writes | `oltp_delete_insert` | 150.80ms | 169.04ms | 1.121× | 2.33% | PASS |
| file_writes | `oltp_write_only` | 119.94ms | 118.59ms | 0.989× | 0.33% | PASS |
| file_writes | `types_delete_insert` | 93.73ms | 95.22ms | 1.016× | 4.14% | PASS |
| file_writes | `oltp_read_write` | 170.36ms | 182.44ms | 1.071× | 3.94% | PASS |
| ac_reads | `oltp_point_select` | 35.43ms | 39.74ms | 1.121× | 1.07% | PASS |
| ac_reads | `oltp_range_select` | 13.02ms | 13.72ms | 1.054× | 1.05% | PASS |
| ac_reads | `oltp_sum_range` | 13.00ms | 13.10ms | 1.008× | 1.16% | PASS |
| ac_reads | `oltp_order_range` | 2.93ms | 3.06ms | 1.044× | 1.08% | PASS |
| ac_reads | `oltp_distinct_range` | 3.80ms | 4.00ms | 1.052× | 0.91% | PASS |
| ac_reads | `oltp_index_scan` | 5.45ms | 6.41ms | 1.177× | 1.11% | PASS |
| ac_reads | `select_random_points` | 18.13ms | 20.17ms | 1.113× | 0.99% | PASS |
| ac_reads | `select_random_ranges` | 4.58ms | 5.54ms | 1.208× | 1.05% | PASS |
| ac_reads | `covering_index_scan` | 5.17ms | 5.07ms | 0.981× | 1.61% | PASS |
| ac_reads | `groupby_scan` | 32.75ms | 33.21ms | 1.014× | 0.75% | PASS |
| ac_reads | `index_join` | 7.51ms | 9.68ms | 1.289× | 0.96% | PASS |
| ac_reads | `index_join_scan` | 4.14ms | 5.47ms | 1.320× | 1.31% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.19s | 1.125× | 1.00% | PASS |
| ac_reads | `table_scan` | 1.28s | 1.31s | 1.020× | 3.19% | PASS |
| ac_reads | `oltp_read_only` | 120.40ms | 133.07ms | 1.105× | 1.08% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 27.74ms | 74.84ms | 2.698× | 3.04% | PASS |
| ac_writes | `oltp_insert_ac` | 29.99ms | 94.90ms | 3.164× | 4.99% | PASS |
| ac_writes | `oltp_update_index_ac` | 31.62ms | 104.23ms | 3.297× | 3.80% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 27.94ms | 84.58ms | 3.028× | 3.43% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 29.36ms | 95.76ms | 3.262× | 3.84% | PASS |
| ac_writes | `oltp_write_only_ac` | 29.56ms | 95.77ms | 3.240× | 3.82% | PASS |
| ac_writes | `types_delete_insert_ac` | 27.15ms | 84.56ms | 3.115× | 4.44% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.30ms | 100.94ms | 3.031× | 3.20% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.53ms | 41.04ms | 1.189× | 2.46% | PASS |
| mem_reads | `oltp_range_select` | 20.37ms | 22.15ms | 1.087× | 1.62% | PASS |
| mem_reads | `oltp_sum_range` | 18.66ms | 21.07ms | 1.129× | 2.10% | PASS |
| mem_reads | `oltp_order_range` | 3.71ms | 3.99ms | 1.073× | 1.57% | PASS |
| mem_reads | `oltp_distinct_range` | 4.72ms | 5.11ms | 1.082× | 1.45% | PASS |
| mem_reads | `oltp_index_scan` | 4.62ms | 6.10ms | 1.322× | 1.27% | PASS |
| mem_reads | `select_random_points` | 27.36ms | 30.91ms | 1.129× | 2.69% | PASS |
| mem_reads | `select_random_ranges` | 7.92ms | 9.10ms | 1.149× | 1.40% | PASS |
| mem_reads | `covering_index_scan` | 4.24ms | 4.56ms | 1.075× | 2.38% | PASS |
| mem_reads | `groupby_scan` | 38.48ms | 42.29ms | 1.099× | 2.05% | PASS |
| mem_reads | `index_join` | 8.33ms | 11.27ms | 1.352× | 2.77% | PASS |
| mem_reads | `index_join_scan` | 4.27ms | 5.79ms | 1.355× | 3.11% | PASS |
| mem_reads | `types_table_scan` | 1.12s | 1.26s | 1.124× | 1.67% | PASS |
| mem_reads | `table_scan` | 1.35s | 1.37s | 1.011× | 1.62% | PASS |
| mem_reads | `oltp_read_only` | 162.18ms | 175.81ms | 1.084× | 1.37% | PASS |
| mem_writes | `oltp_bulk_insert` | 244.69ms | 367.09ms | 1.500× | 1.23% | PASS |
| mem_writes | `oltp_insert` | 19.05ms | 38.12ms | 2.001× | 1.20% | PASS |
| mem_writes | `oltp_update_index` | 69.83ms | 125.31ms | 1.794× | 1.91% | PASS |
| mem_writes | `oltp_update_non_index` | 52.23ms | 88.94ms | 1.703× | 1.41% | PASS |
| mem_writes | `oltp_delete_insert` | 51.13ms | 103.21ms | 2.019× | 1.25% | PASS |
| mem_writes | `oltp_write_only` | 28.38ms | 63.35ms | 2.232× | 1.68% | PASS |
| mem_writes | `types_delete_insert` | 34.01ms | 58.59ms | 1.723× | 1.54% | PASS |
| mem_writes | `oltp_read_write` | 108.23ms | 165.42ms | 1.528× | 1.52% | PASS |
| file_reads | `oltp_point_select` | 105.32ms | 65.26ms | 0.620× | 0.93% | PASS |
| file_reads | `oltp_range_select` | 28.60ms | 24.83ms | 0.868× | 1.55% | PASS |
| file_reads | `oltp_sum_range` | 26.57ms | 24.15ms | 0.909× | 1.62% | PASS |
| file_reads | `oltp_order_range` | 4.91ms | 4.50ms | 0.917× | 2.33% | PASS |
| file_reads | `oltp_distinct_range` | 5.99ms | 5.63ms | 0.940× | 1.38% | PASS |
| file_reads | `oltp_index_scan` | 12.19ms | 8.92ms | 0.732× | 0.92% | PASS |
| file_reads | `select_random_points` | 37.11ms | 34.74ms | 0.936× | 1.91% | PASS |
| file_reads | `select_random_ranges` | 15.66ms | 11.71ms | 0.748× | 1.47% | PASS |
| file_reads | `covering_index_scan` | 12.00ms | 7.13ms | 0.594× | 1.54% | PASS |
| file_reads | `groupby_scan` | 40.00ms | 43.15ms | 1.079× | 1.62% | PASS |
| file_reads | `index_join` | 12.74ms | 12.97ms | 1.018× | 1.55% | PASS |
| file_reads | `index_join_scan` | 5.21ms | 6.09ms | 1.170× | 1.87% | PASS |
| file_reads | `types_table_scan` | 1.12s | 1.26s | 1.125× | 1.10% | PASS |
| file_reads | `table_scan` | 1.36s | 1.37s | 1.009× | 1.96% | PASS |
| file_reads | `oltp_read_only` | 269.54ms | 213.45ms | 0.792× | 1.24% | PASS |
| file_writes | `oltp_bulk_insert` | 258.47ms | 386.98ms | 1.497× | 1.04% | PASS |
| file_writes | `oltp_insert` | 26.44ms | 48.55ms | 1.836× | 1.44% | PASS |
| file_writes | `oltp_update_index` | 101.67ms | 154.41ms | 1.519× | 2.01% | PASS |
| file_writes | `oltp_update_non_index` | 79.06ms | 110.20ms | 1.394× | 1.55% | PASS |
| file_writes | `oltp_delete_insert` | 79.39ms | 127.09ms | 1.601× | 1.48% | PASS |
| file_writes | `oltp_write_only` | 52.85ms | 82.85ms | 1.568× | 1.77% | PASS |
| file_writes | `types_delete_insert` | 50.93ms | 71.66ms | 1.407× | 1.70% | PASS |
| file_writes | `oltp_read_write` | 138.16ms | 184.79ms | 1.337× | 2.33% | PASS |
| ac_reads | `oltp_point_select` | 57.31ms | 64.83ms | 1.131× | 1.66% | PASS |
| ac_reads | `oltp_range_select` | 23.06ms | 24.72ms | 1.072× | 1.52% | PASS |
| ac_reads | `oltp_sum_range` | 21.25ms | 23.93ms | 1.126× | 1.43% | PASS |
| ac_reads | `oltp_order_range` | 4.24ms | 4.38ms | 1.034× | 1.51% | PASS |
| ac_reads | `oltp_distinct_range` | 5.34ms | 5.57ms | 1.043× | 1.78% | PASS |
| ac_reads | `oltp_index_scan` | 7.58ms | 8.95ms | 1.181× | 1.64% | PASS |
| ac_reads | `select_random_points` | 31.67ms | 35.01ms | 1.105× | 2.55% | PASS |
| ac_reads | `select_random_ranges` | 10.74ms | 11.79ms | 1.099× | 1.14% | PASS |
| ac_reads | `covering_index_scan` | 7.18ms | 7.14ms | 0.993× | 1.69% | PASS |
| ac_reads | `groupby_scan` | 38.93ms | 42.86ms | 1.101× | 2.16% | PASS |
| ac_reads | `index_join` | 10.23ms | 12.88ms | 1.260× | 2.02% | PASS |
| ac_reads | `index_join_scan` | 4.72ms | 6.05ms | 1.281× | 3.09% | PASS |
| ac_reads | `types_table_scan` | 1.10s | 1.25s | 1.136× | 2.56% | PASS |
| ac_reads | `table_scan` | 1.34s | 1.36s | 1.018× | 1.90% | PASS |
| ac_reads | `oltp_read_only` | 193.66ms | 211.10ms | 1.090× | 1.06% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.27ms | 67.85ms | 2.915× | 6.57% | PASS |
| ac_writes | `oltp_insert_ac` | 23.80ms | 83.68ms | 3.515× | 5.46% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.74ms | 94.68ms | 3.541× | 4.67% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.20ms | 74.55ms | 3.359× | 4.03% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.81ms | 84.98ms | 3.570× | 5.37% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.80ms | 85.93ms | 3.611× | 5.02% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.24ms | 76.23ms | 3.589× | 4.79% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.42ms | 95.13ms | 3.028× | 4.82% | PASS |

</details>

## Version-control latency

Wall time: 2m 6s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 78.90ms | 200.00ms | 39.4% | 0.37% | PASS |
| `status_dirty_many_tables` | 81.83ms | 200.00ms | 40.9% | 0.33% | PASS |
| `diff_regular_working_one_table` | 74.61ms | 150.00ms | 49.7% | 0.34% | PASS |
| `diff_regular_working_many_tables` | 87.36ms | 200.00ms | 43.7% | 0.32% | PASS |
| `diff_stat_working_many_tables` | 87.25ms | 200.00ms | 43.6% | 0.30% | PASS |
| `diff_schema_working_many_tables` | 87.61ms | 200.00ms | 43.8% | 0.32% | PASS |
| `branch_list_many_branches` | 18.86ms | 100.00ms | 18.9% | 1.57% | PASS |
| `branch_create_delete` | 20.78ms | 100.00ms | 20.8% | 1.57% | PASS |
| `checkout_branch_clean` | 50.86ms | 200.00ms | 25.4% | 0.70% | PASS |
| `merge_data_no_conflicts` | 25.05ms | 150.00ms | 16.7% | 1.20% | PASS |
| `merge_schema_no_conflicts` | 18.18ms | 100.00ms | 18.2% | 1.90% | PASS |
| `merge_data_conflicts` | 111.05ms | 250.00ms | 44.4% | 0.74% | PASS |
| `merge_data_conflicts_with_resolve` | 110.13ms | 250.00ms | 44.1% | 0.34% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
