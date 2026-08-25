# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-25 11:36 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260816.277.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/32835119282)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 52s | 9.19s | 11.68s | 1.270× | 1.49% | **PASS** |
| textpk | 69 | 55 | 1h 29m 52s | 9.05s | 10.06s | 1.112× | 1.40% | **PASS** |
| blobpk | 69 | 55 | 1h 31m 25s | 9.54s | 11.87s | 1.245× | 1.67% | **PASS** |
| compositepk | 69 | 55 | 1h 24m 5s | 8.96s | 12.74s | 1.422× | 1.70% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.18ms | 30.32ms | 1.204× | 1.82% | PASS |
| mem_reads | `oltp_range_select` | 10.55ms | 13.31ms | 1.262× | 1.52% | PASS |
| mem_reads | `oltp_sum_range` | 10.02ms | 12.53ms | 1.251× | 1.52% | PASS |
| mem_reads | `oltp_order_range` | 2.62ms | 3.01ms | 1.148× | 1.12% | PASS |
| mem_reads | `oltp_distinct_range` | 3.68ms | 4.08ms | 1.108× | 0.86% | PASS |
| mem_reads | `oltp_index_scan` | 4.02ms | 5.59ms | 1.390× | 1.11% | PASS |
| mem_reads | `select_random_points` | 10.52ms | 11.38ms | 1.082× | 1.35% | PASS |
| mem_reads | `select_random_ranges` | 3.10ms | 4.04ms | 1.303× | 1.17% | PASS |
| mem_reads | `covering_index_scan` | 4.26ms | 4.41ms | 1.036× | 1.95% | PASS |
| mem_reads | `groupby_scan` | 30.20ms | 33.11ms | 1.096× | 0.56% | PASS |
| mem_reads | `index_join` | 6.11ms | 8.73ms | 1.429× | 1.33% | PASS |
| mem_reads | `index_join_scan` | 3.54ms | 4.79ms | 1.351× | 2.53% | PASS |
| mem_reads | `types_table_scan` | 1.08s | 1.35s | 1.247× | 1.59% | PASS |
| mem_reads | `table_scan` | 1.22s | 1.41s | 1.155× | 1.05% | PASS |
| mem_reads | `oltp_read_only` | 106.39ms | 125.93ms | 1.184× | 1.15% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.99ms | 254.80ms | 1.400× | 1.00% | PASS |
| mem_writes | `oltp_insert` | 15.62ms | 28.94ms | 1.852× | 1.13% | PASS |
| mem_writes | `oltp_update_index` | 51.12ms | 108.11ms | 2.115× | 1.21% | PASS |
| mem_writes | `oltp_update_non_index` | 35.11ms | 61.74ms | 1.759× | 1.54% | PASS |
| mem_writes | `oltp_delete_insert` | 46.64ms | 82.07ms | 1.760× | 1.15% | PASS |
| mem_writes | `oltp_write_only` | 22.63ms | 46.72ms | 2.065× | 1.41% | PASS |
| mem_writes | `types_delete_insert` | 24.79ms | 41.03ms | 1.655× | 1.68% | PASS |
| mem_writes | `oltp_read_write` | 67.97ms | 112.28ms | 1.652× | 1.40% | PASS |
| file_reads | `oltp_point_select` | 99.79ms | 55.52ms | 0.556× | 1.03% | PASS |
| file_reads | `oltp_range_select` | 18.46ms | 15.91ms | 0.862× | 1.61% | PASS |
| file_reads | `oltp_sum_range` | 18.49ms | 15.47ms | 0.837× | 1.50% | PASS |
| file_reads | `oltp_order_range` | 3.51ms | 3.35ms | 0.955× | 1.29% | PASS |
| file_reads | `oltp_distinct_range` | 4.61ms | 4.44ms | 0.963× | 1.63% | PASS |
| file_reads | `oltp_index_scan` | 11.77ms | 8.31ms | 0.706× | 1.84% | PASS |
| file_reads | `select_random_points` | 17.33ms | 13.90ms | 0.802× | 3.28% | PASS |
| file_reads | `select_random_ranges` | 10.45ms | 6.58ms | 0.630× | 1.49% | PASS |
| file_reads | `covering_index_scan` | 12.00ms | 6.92ms | 0.577× | 1.82% | PASS |
| file_reads | `groupby_scan` | 30.79ms | 33.34ms | 1.083× | 1.02% | PASS |
| file_reads | `index_join` | 10.39ms | 10.10ms | 0.972× | 1.89% | PASS |
| file_reads | `index_join_scan` | 4.46ms | 4.96ms | 1.112× | 1.21% | PASS |
| file_reads | `types_table_scan` | 1.09s | 1.34s | 1.235× | 1.44% | PASS |
| file_reads | `table_scan` | 1.27s | 1.42s | 1.119× | 1.32% | PASS |
| file_reads | `oltp_read_only` | 219.12ms | 164.80ms | 0.752× | 0.93% | PASS |
| file_writes | `oltp_bulk_insert` | 196.12ms | 274.71ms | 1.401× | 1.34% | PASS |
| file_writes | `oltp_insert` | 22.72ms | 36.45ms | 1.605× | 1.91% | PASS |
| file_writes | `oltp_update_index` | 82.88ms | 136.43ms | 1.646× | 1.70% | PASS |
| file_writes | `oltp_update_non_index` | 60.89ms | 85.48ms | 1.404× | 1.22% | PASS |
| file_writes | `oltp_delete_insert` | 72.68ms | 105.58ms | 1.453× | 2.26% | PASS |
| file_writes | `oltp_write_only` | 47.34ms | 66.87ms | 1.412× | 1.80% | PASS |
| file_writes | `types_delete_insert` | 41.55ms | 55.14ms | 1.327× | 2.01% | PASS |
| file_writes | `oltp_read_write` | 95.75ms | 132.42ms | 1.383× | 2.86% | PASS |
| ac_reads | `oltp_point_select` | 48.80ms | 55.19ms | 1.131× | 1.45% | PASS |
| ac_reads | `oltp_range_select` | 13.17ms | 15.87ms | 1.205× | 1.43% | PASS |
| ac_reads | `oltp_sum_range` | 12.71ms | 15.24ms | 1.199× | 1.63% | PASS |
| ac_reads | `oltp_order_range` | 3.00ms | 3.37ms | 1.123× | 1.61% | PASS |
| ac_reads | `oltp_distinct_range` | 4.05ms | 4.42ms | 1.092× | 1.12% | PASS |
| ac_reads | `oltp_index_scan` | 6.81ms | 8.31ms | 1.220× | 1.01% | PASS |
| ac_reads | `select_random_points` | 13.24ms | 14.05ms | 1.061× | 1.18% | PASS |
| ac_reads | `select_random_ranges` | 5.71ms | 6.70ms | 1.173× | 1.05% | PASS |
| ac_reads | `covering_index_scan` | 7.25ms | 7.26ms | 1.002× | 1.31% | PASS |
| ac_reads | `groupby_scan` | 30.48ms | 33.74ms | 1.107× | 1.20% | PASS |
| ac_reads | `index_join` | 7.96ms | 10.54ms | 1.326× | 1.73% | PASS |
| ac_reads | `index_join_scan` | 3.96ms | 5.03ms | 1.270× | 1.60% | PASS |
| ac_reads | `types_table_scan` | 1.07s | 1.34s | 1.250× | 1.65% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.41s | 1.171× | 1.46% | PASS |
| ac_reads | `oltp_read_only` | 144.06ms | 164.22ms | 1.140× | 1.34% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.79ms | 84.23ms | 3.541× | 8.92% | PASS |
| ac_writes | `oltp_insert_ac` | 27.07ms | 104.78ms | 3.870× | 7.37% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.51ms | 122.56ms | 4.153× | 6.35% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.79ms | 95.23ms | 3.842× | 8.52% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.13ms | 106.57ms | 4.241× | 5.13% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.30ms | 105.72ms | 4.020× | 6.47% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.32ms | 97.00ms | 3.989× | 8.26% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.39ms | 114.23ms | 3.640× | 6.50% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.90ms | 27.57ms | 1.154× | 1.68% | PASS |
| mem_reads | `oltp_range_select` | 11.45ms | 10.73ms | 0.937× | 1.95% | PASS |
| mem_reads | `oltp_sum_range` | 10.49ms | 10.57ms | 1.008× | 3.72% | PASS |
| mem_reads | `oltp_order_range` | 2.53ms | 2.52ms | 0.994× | 1.85% | PASS |
| mem_reads | `oltp_distinct_range` | 3.38ms | 3.29ms | 0.976× | 1.17% | PASS |
| mem_reads | `oltp_index_scan` | 3.82ms | 4.92ms | 1.290× | 2.23% | PASS |
| mem_reads | `select_random_points` | 15.52ms | 16.52ms | 1.065× | 3.77% | PASS |
| mem_reads | `select_random_ranges` | 3.32ms | 4.16ms | 1.254× | 1.11% | PASS |
| mem_reads | `covering_index_scan` | 3.75ms | 3.45ms | 0.920× | 1.59% | PASS |
| mem_reads | `groupby_scan` | 26.61ms | 27.52ms | 1.034× | 0.71% | PASS |
| mem_reads | `index_join` | 5.65ms | 7.03ms | 1.243× | 1.39% | PASS |
| mem_reads | `index_join_scan` | 4.15ms | 4.62ms | 1.113× | 2.03% | PASS |
| mem_reads | `types_table_scan` | 900.70ms | 976.70ms | 1.084× | 0.96% | PASS |
| mem_reads | `table_scan` | 1.07s | 1.06s | 0.991× | 1.15% | PASS |
| mem_reads | `oltp_read_only` | 94.60ms | 100.19ms | 1.059× | 0.95% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.44ms | 259.78ms | 1.432× | 0.69% | PASS |
| mem_writes | `oltp_insert` | 17.71ms | 29.90ms | 1.688× | 1.11% | PASS |
| mem_writes | `oltp_update_index` | 60.81ms | 105.49ms | 1.735× | 2.50% | PASS |
| mem_writes | `oltp_update_non_index` | 41.27ms | 67.48ms | 1.635× | 1.47% | PASS |
| mem_writes | `oltp_delete_insert` | 43.42ms | 81.79ms | 1.884× | 1.26% | PASS |
| mem_writes | `oltp_write_only` | 26.21ms | 49.68ms | 1.895× | 1.55% | PASS |
| mem_writes | `types_delete_insert` | 27.95ms | 42.22ms | 1.510× | 1.36% | PASS |
| mem_writes | `oltp_read_write` | 73.75ms | 106.06ms | 1.438× | 3.76% | PASS |
| file_reads | `oltp_point_select` | 100.29ms | 53.14ms | 0.530× | 0.70% | PASS |
| file_reads | `oltp_range_select` | 20.06ms | 13.44ms | 0.670× | 1.40% | PASS |
| file_reads | `oltp_sum_range` | 18.94ms | 13.54ms | 0.715× | 1.91% | PASS |
| file_reads | `oltp_order_range` | 3.51ms | 2.83ms | 0.806× | 1.09% | PASS |
| file_reads | `oltp_distinct_range` | 4.29ms | 3.59ms | 0.837× | 0.82% | PASS |
| file_reads | `oltp_index_scan` | 11.79ms | 7.62ms | 0.646× | 0.78% | PASS |
| file_reads | `select_random_points` | 23.85ms | 19.62ms | 0.823× | 1.46% | PASS |
| file_reads | `select_random_ranges` | 11.17ms | 6.78ms | 0.607× | 0.61% | PASS |
| file_reads | `covering_index_scan` | 12.59ms | 6.42ms | 0.510× | 0.43% | PASS |
| file_reads | `groupby_scan` | 27.79ms | 27.91ms | 1.005× | 0.73% | PASS |
| file_reads | `index_join` | 10.57ms | 9.24ms | 0.874× | 0.97% | PASS |
| file_reads | `index_join_scan` | 5.03ms | 5.10ms | 1.015× | 1.69% | PASS |
| file_reads | `types_table_scan` | 916.73ms | 981.02ms | 1.070× | 1.48% | PASS |
| file_reads | `table_scan` | 1.06s | 1.06s | 0.995× | 1.55% | PASS |
| file_reads | `oltp_read_only` | 203.68ms | 136.11ms | 0.668× | 0.62% | PASS |
| file_writes | `oltp_bulk_insert` | 260.74ms | 406.83ms | 1.560× | 27.64% | PASS |
| file_writes | `oltp_insert` | 70.68ms | 61.36ms | 0.868× | 33.49% | PASS |
| file_writes | `oltp_update_index` | 272.17ms | 213.23ms | 0.783× | 16.34% | PASS |
| file_writes | `oltp_update_non_index` | 180.19ms | 139.55ms | 0.774× | 25.19% | PASS |
| file_writes | `oltp_delete_insert` | 172.29ms | 165.87ms | 0.963× | 14.01% | PASS |
| file_writes | `oltp_write_only` | 131.46ms | 118.11ms | 0.898× | 10.04% | PASS |
| file_writes | `types_delete_insert` | 107.52ms | 98.63ms | 0.917× | 13.28% | PASS |
| file_writes | `oltp_read_write` | 178.72ms | 172.95ms | 0.968× | 6.33% | PASS |
| ac_reads | `oltp_point_select` | 49.59ms | 52.95ms | 1.068× | 1.21% | PASS |
| ac_reads | `oltp_range_select` | 15.04ms | 13.43ms | 0.893× | 1.09% | PASS |
| ac_reads | `oltp_sum_range` | 13.61ms | 13.41ms | 0.985× | 0.88% | PASS |
| ac_reads | `oltp_order_range` | 3.03ms | 2.82ms | 0.931× | 0.61% | PASS |
| ac_reads | `oltp_distinct_range` | 3.82ms | 3.60ms | 0.942× | 1.08% | PASS |
| ac_reads | `oltp_index_scan` | 6.86ms | 7.64ms | 1.114× | 0.75% | PASS |
| ac_reads | `select_random_points` | 18.22ms | 19.24ms | 1.056× | 0.79% | PASS |
| ac_reads | `select_random_ranges` | 6.18ms | 6.75ms | 1.093× | 0.81% | PASS |
| ac_reads | `covering_index_scan` | 7.73ms | 6.43ms | 0.832× | 0.88% | PASS |
| ac_reads | `groupby_scan` | 27.59ms | 28.02ms | 1.015× | 0.76% | PASS |
| ac_reads | `index_join` | 8.16ms | 9.29ms | 1.138× | 0.79% | PASS |
| ac_reads | `index_join_scan` | 4.58ms | 5.13ms | 1.120× | 1.10% | PASS |
| ac_reads | `types_table_scan` | 926.09ms | 979.76ms | 1.058× | 2.38% | PASS |
| ac_reads | `table_scan` | 1.07s | 1.06s | 0.992× | 1.04% | PASS |
| ac_reads | `oltp_read_only` | 131.19ms | 136.53ms | 1.041× | 0.80% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 58.59ms | 120.04ms | 2.049× | 58.64% | PASS |
| ac_writes | `oltp_insert_ac` | 34.51ms | 97.96ms | 2.839× | 23.23% | PASS |
| ac_writes | `oltp_update_index_ac` | 35.82ms | 148.56ms | 4.147× | 41.22% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 32.08ms | 121.78ms | 3.796× | 49.77% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 35.53ms | 121.98ms | 3.433× | 36.82% | PASS |
| ac_writes | `oltp_write_only_ac` | 36.64ms | 154.59ms | 4.220× | 57.10% | PASS |
| ac_writes | `types_delete_insert_ac` | 30.97ms | 107.74ms | 3.479× | 55.24% | PASS |
| ac_writes | `oltp_read_write_ac` | 41.03ms | 125.31ms | 3.054× | 38.31% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.19ms | 37.41ms | 1.199× | 1.37% | PASS |
| mem_reads | `oltp_range_select` | 13.66ms | 14.14ms | 1.036× | 2.04% | PASS |
| mem_reads | `oltp_sum_range` | 12.49ms | 14.04ms | 1.124× | 1.66% | PASS |
| mem_reads | `oltp_order_range` | 2.94ms | 3.16ms | 1.075× | 1.46% | PASS |
| mem_reads | `oltp_distinct_range` | 3.99ms | 4.22ms | 1.058× | 1.18% | PASS |
| mem_reads | `oltp_index_scan` | 4.60ms | 6.33ms | 1.376× | 2.15% | PASS |
| mem_reads | `select_random_points` | 18.59ms | 21.15ms | 1.138× | 1.61% | PASS |
| mem_reads | `select_random_ranges` | 4.14ms | 5.20ms | 1.256× | 1.91% | PASS |
| mem_reads | `covering_index_scan` | 4.41ms | 4.59ms | 1.041× | 1.92% | PASS |
| mem_reads | `groupby_scan` | 31.92ms | 33.74ms | 1.057× | 0.99% | PASS |
| mem_reads | `index_join` | 6.89ms | 9.23ms | 1.339× | 2.09% | PASS |
| mem_reads | `index_join_scan` | 4.53ms | 5.47ms | 1.209× | 2.19% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.24s | 1.175× | 1.18% | PASS |
| mem_reads | `table_scan` | 1.22s | 1.38s | 1.130× | 0.86% | PASS |
| mem_reads | `oltp_read_only` | 120.86ms | 136.20ms | 1.127× | 1.55% | PASS |
| mem_writes | `oltp_bulk_insert` | 241.01ms | 354.75ms | 1.472× | 1.10% | PASS |
| mem_writes | `oltp_insert` | 20.05ms | 39.97ms | 1.993× | 1.14% | PASS |
| mem_writes | `oltp_update_index` | 69.75ms | 132.31ms | 1.897× | 1.48% | PASS |
| mem_writes | `oltp_update_non_index` | 50.52ms | 85.95ms | 1.701× | 1.13% | PASS |
| mem_writes | `oltp_delete_insert` | 49.53ms | 103.66ms | 2.093× | 1.53% | PASS |
| mem_writes | `oltp_write_only` | 28.38ms | 62.83ms | 2.214× | 1.11% | PASS |
| mem_writes | `types_delete_insert` | 33.71ms | 55.11ms | 1.635× | 1.15% | PASS |
| mem_writes | `oltp_read_write` | 86.51ms | 140.42ms | 1.623× | 1.43% | PASS |
| file_reads | `oltp_point_select` | 106.77ms | 63.89ms | 0.598× | 1.06% | PASS |
| file_reads | `oltp_range_select` | 21.66ms | 17.12ms | 0.791× | 2.55% | PASS |
| file_reads | `oltp_sum_range` | 20.42ms | 17.12ms | 0.839× | 1.70% | PASS |
| file_reads | `oltp_order_range` | 3.92ms | 3.59ms | 0.915× | 2.33% | PASS |
| file_reads | `oltp_distinct_range` | 4.96ms | 4.67ms | 0.940× | 1.71% | PASS |
| file_reads | `oltp_index_scan` | 12.44ms | 9.31ms | 0.748× | 1.34% | PASS |
| file_reads | `select_random_points` | 27.59ms | 25.13ms | 0.911× | 2.19% | PASS |
| file_reads | `select_random_ranges` | 11.83ms | 7.92ms | 0.670× | 1.22% | PASS |
| file_reads | `covering_index_scan` | 12.19ms | 7.42ms | 0.609× | 1.77% | PASS |
| file_reads | `groupby_scan` | 33.36ms | 34.37ms | 1.030× | 1.06% | PASS |
| file_reads | `index_join` | 11.29ms | 11.48ms | 1.017× | 2.62% | PASS |
| file_reads | `index_join_scan` | 5.52ms | 5.98ms | 1.083× | 2.70% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.23s | 1.168× | 1.04% | PASS |
| file_reads | `table_scan` | 1.23s | 1.38s | 1.119× | 1.45% | PASS |
| file_reads | `oltp_read_only` | 233.08ms | 176.12ms | 0.756× | 1.11% | PASS |
| file_writes | `oltp_bulk_insert` | 261.23ms | 380.11ms | 1.455× | 0.92% | PASS |
| file_writes | `oltp_insert` | 31.70ms | 53.01ms | 1.672× | 1.61% | PASS |
| file_writes | `oltp_update_index` | 104.73ms | 166.05ms | 1.586× | 1.89% | PASS |
| file_writes | `oltp_update_non_index` | 82.51ms | 111.71ms | 1.354× | 1.83% | PASS |
| file_writes | `oltp_delete_insert` | 82.64ms | 132.62ms | 1.605× | 1.67% | PASS |
| file_writes | `oltp_write_only` | 57.30ms | 86.06ms | 1.502× | 1.67% | PASS |
| file_writes | `types_delete_insert` | 53.96ms | 73.84ms | 1.368× | 1.61% | PASS |
| file_writes | `oltp_read_write` | 119.87ms | 164.61ms | 1.373× | 2.01% | PASS |
| ac_reads | `oltp_point_select` | 56.66ms | 63.60ms | 1.123× | 1.21% | PASS |
| ac_reads | `oltp_range_select` | 16.56ms | 17.05ms | 1.030× | 2.11% | PASS |
| ac_reads | `oltp_sum_range` | 15.87ms | 17.07ms | 1.076× | 1.72% | PASS |
| ac_reads | `oltp_order_range` | 3.36ms | 3.55ms | 1.059× | 2.42% | PASS |
| ac_reads | `oltp_distinct_range` | 4.45ms | 4.64ms | 1.043× | 1.67% | PASS |
| ac_reads | `oltp_index_scan` | 7.49ms | 9.28ms | 1.239× | 2.29% | PASS |
| ac_reads | `select_random_points` | 22.23ms | 24.90ms | 1.120× | 1.75% | PASS |
| ac_reads | `select_random_ranges` | 6.97ms | 7.89ms | 1.132× | 1.50% | PASS |
| ac_reads | `covering_index_scan` | 7.67ms | 7.40ms | 0.964× | 3.42% | PASS |
| ac_reads | `groupby_scan` | 32.56ms | 34.33ms | 1.054× | 0.90% | PASS |
| ac_reads | `index_join` | 9.00ms | 11.63ms | 1.292× | 3.26% | PASS |
| ac_reads | `index_join_scan` | 5.06ms | 6.07ms | 1.199× | 2.73% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.23s | 1.176× | 0.65% | PASS |
| ac_reads | `table_scan` | 1.23s | 1.38s | 1.121× | 1.63% | PASS |
| ac_reads | `oltp_read_only` | 160.93ms | 176.02ms | 1.094× | 1.34% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.30ms | 83.30ms | 3.427× | 7.23% | PASS |
| ac_writes | `oltp_insert_ac` | 27.29ms | 106.65ms | 3.908× | 7.08% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.39ms | 121.35ms | 4.128× | 4.65% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.47ms | 95.00ms | 3.882× | 7.03% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.39ms | 106.89ms | 4.210× | 4.97% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.14ms | 105.29ms | 4.028× | 5.95% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.62ms | 97.20ms | 4.115× | 7.49% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.28ms | 113.08ms | 3.503× | 3.95% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.05ms | 23.35ms | 1.165× | 1.13% | PASS |
| mem_reads | `oltp_range_select` | 13.52ms | 13.24ms | 0.979× | 1.97% | PASS |
| mem_reads | `oltp_sum_range` | 13.15ms | 12.52ms | 0.952× | 1.80% | PASS |
| mem_reads | `oltp_order_range` | 2.52ms | 2.51ms | 0.995× | 1.28% | PASS |
| mem_reads | `oltp_distinct_range` | 3.19ms | 3.13ms | 0.981× | 1.22% | PASS |
| mem_reads | `oltp_index_scan` | 3.13ms | 3.97ms | 1.270× | 1.95% | PASS |
| mem_reads | `select_random_points` | 19.50ms | 21.67ms | 1.111× | 1.00% | PASS |
| mem_reads | `select_random_ranges` | 4.92ms | 5.55ms | 1.127× | 1.09% | PASS |
| mem_reads | `covering_index_scan` | 2.46ms | 2.52ms | 1.024× | 1.42% | PASS |
| mem_reads | `groupby_scan` | 22.98ms | 24.60ms | 1.070× | 0.70% | PASS |
| mem_reads | `index_join` | 5.63ms | 7.17ms | 1.275× | 1.40% | PASS |
| mem_reads | `index_join_scan` | 2.91ms | 4.41ms | 1.517× | 2.31% | PASS |
| mem_reads | `types_table_scan` | 759.33ms | 849.88ms | 1.119× | 0.89% | PASS |
| mem_reads | `table_scan` | 860.88ms | 914.40ms | 1.062× | 2.46% | PASS |
| mem_reads | `oltp_read_only` | 93.41ms | 100.55ms | 1.076× | 0.97% | PASS |
| mem_writes | `oltp_bulk_insert` | 138.74ms | 197.25ms | 1.422× | 0.75% | PASS |
| mem_writes | `oltp_insert` | 11.58ms | 21.39ms | 1.847× | 0.79% | PASS |
| mem_writes | `oltp_update_index` | 43.95ms | 76.38ms | 1.738× | 1.57% | PASS |
| mem_writes | `oltp_update_non_index` | 31.65ms | 51.99ms | 1.643× | 1.70% | PASS |
| mem_writes | `oltp_delete_insert` | 32.34ms | 59.39ms | 1.837× | 1.63% | PASS |
| mem_writes | `oltp_write_only` | 17.66ms | 35.46ms | 2.008× | 1.45% | PASS |
| mem_writes | `types_delete_insert` | 20.15ms | 32.92ms | 1.634× | 1.12% | PASS |
| mem_writes | `oltp_read_write` | 63.41ms | 92.64ms | 1.461× | 1.05% | PASS |
| file_reads | `oltp_point_select` | 43.57ms | 31.90ms | 0.732× | 1.44% | PASS |
| file_reads | `oltp_range_select` | 16.54ms | 14.50ms | 0.876× | 1.06% | PASS |
| file_reads | `oltp_sum_range` | 16.28ms | 13.75ms | 0.845× | 1.44% | PASS |
| file_reads | `oltp_order_range` | 2.84ms | 2.69ms | 0.949× | 1.71% | PASS |
| file_reads | `oltp_distinct_range` | 3.54ms | 3.31ms | 0.934× | 1.45% | PASS |
| file_reads | `oltp_index_scan` | 5.67ms | 5.34ms | 0.941× | 2.14% | PASS |
| file_reads | `select_random_points` | 22.67ms | 23.45ms | 1.035× | 1.28% | PASS |
| file_reads | `select_random_ranges` | 7.44ms | 6.58ms | 0.885× | 0.94% | PASS |
| file_reads | `covering_index_scan` | 4.97ms | 3.82ms | 0.769× | 1.84% | PASS |
| file_reads | `groupby_scan` | 23.36ms | 24.83ms | 1.063× | 0.50% | PASS |
| file_reads | `index_join` | 7.12ms | 8.50ms | 1.195× | 1.86% | PASS |
| file_reads | `index_join_scan` | 3.28ms | 4.73ms | 1.441× | 3.16% | PASS |
| file_reads | `types_table_scan` | 754.72ms | 849.65ms | 1.126× | 0.94% | PASS |
| file_reads | `table_scan` | 868.50ms | 916.06ms | 1.055× | 1.84% | PASS |
| file_reads | `oltp_read_only` | 122.91ms | 111.71ms | 0.909× | 0.52% | PASS |
| file_writes | `oltp_bulk_insert` | 212.19ms | 258.96ms | 1.220× | 22.93% | PASS |
| file_writes | `oltp_insert` | 107.13ms | 76.57ms | 0.715× | 70.07% | PASS |
| file_writes | `oltp_update_index` | 219.63ms | 194.70ms | 0.887× | 29.07% | PASS |
| file_writes | `oltp_update_non_index` | 219.66ms | 146.76ms | 0.668× | 31.95% | PASS |
| file_writes | `oltp_delete_insert` | 229.85ms | 134.09ms | 0.583× | 39.00% | PASS |
| file_writes | `oltp_write_only` | 160.29ms | 107.76ms | 0.672× | 35.77% | PASS |
| file_writes | `types_delete_insert` | 123.16ms | 91.98ms | 0.747× | 31.56% | PASS |
| file_writes | `oltp_read_write` | 200.86ms | 197.53ms | 0.983× | 34.44% | PASS |
| ac_reads | `oltp_point_select` | 27.75ms | 31.82ms | 1.147× | 0.88% | PASS |
| ac_reads | `oltp_range_select` | 15.18ms | 14.48ms | 0.954× | 1.10% | PASS |
| ac_reads | `oltp_sum_range` | 14.70ms | 13.73ms | 0.934× | 1.44% | PASS |
| ac_reads | `oltp_order_range` | 2.69ms | 2.67ms | 0.996× | 1.87% | PASS |
| ac_reads | `oltp_distinct_range` | 3.44ms | 3.30ms | 0.961× | 2.34% | PASS |
| ac_reads | `oltp_index_scan` | 4.30ms | 5.28ms | 1.227× | 2.64% | PASS |
| ac_reads | `select_random_points` | 20.91ms | 23.27ms | 1.113× | 0.85% | PASS |
| ac_reads | `select_random_ranges` | 5.90ms | 6.59ms | 1.117× | 1.19% | PASS |
| ac_reads | `covering_index_scan` | 3.36ms | 3.50ms | 1.044× | 2.04% | PASS |
| ac_reads | `groupby_scan` | 22.86ms | 24.58ms | 1.075× | 0.51% | PASS |
| ac_reads | `index_join` | 6.31ms | 8.20ms | 1.299× | 2.81% | PASS |
| ac_reads | `index_join_scan` | 3.20ms | 4.68ms | 1.459× | 2.56% | PASS |
| ac_reads | `types_table_scan` | 756.04ms | 847.34ms | 1.121× | 0.96% | PASS |
| ac_reads | `table_scan` | 866.93ms | 913.51ms | 1.054× | 1.96% | PASS |
| ac_reads | `oltp_read_only` | 105.41ms | 112.58ms | 1.068× | 1.19% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 234.18ms | 613.46ms | 2.620× | 57.47% | PASS |
| ac_writes | `oltp_insert_ac` | 52.38ms | 175.56ms | 3.351× | 47.23% | PASS |
| ac_writes | `oltp_update_index_ac` | 183.68ms | 589.10ms | 3.207× | 71.31% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 287.70ms | 883.11ms | 3.069× | 72.88% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 220.35ms | 648.45ms | 2.943× | 69.32% | PASS |
| ac_writes | `oltp_write_only_ac` | 177.16ms | 784.65ms | 4.429× | 64.03% | PASS |
| ac_writes | `types_delete_insert_ac` | 223.71ms | 745.76ms | 3.334× | 56.21% | PASS |
| ac_writes | `oltp_read_write_ac` | 162.39ms | 502.14ms | 3.092× | 55.28% | PASS |

</details>

## Version-control latency

Wall time: 2m 18s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 81.90ms | 200.00ms | 40.9% | 0.68% | PASS |
| `status_dirty_many_tables` | 85.81ms | 200.00ms | 42.9% | 0.73% | PASS |
| `diff_regular_working_one_table` | 78.83ms | 150.00ms | 52.6% | 0.53% | PASS |
| `diff_regular_working_many_tables` | 91.06ms | 200.00ms | 45.5% | 0.59% | PASS |
| `diff_stat_working_many_tables` | 91.07ms | 200.00ms | 45.5% | 0.52% | PASS |
| `diff_schema_working_many_tables` | 91.11ms | 200.00ms | 45.6% | 0.44% | PASS |
| `branch_list_many_branches` | 22.56ms | 100.00ms | 22.6% | 0.86% | PASS |
| `branch_create_delete` | 24.66ms | 100.00ms | 24.7% | 0.99% | PASS |
| `checkout_branch_clean` | 56.09ms | 200.00ms | 28.0% | 2.15% | PASS |
| `merge_data_no_conflicts` | 28.59ms | 150.00ms | 19.1% | 1.07% | PASS |
| `merge_schema_no_conflicts` | 21.98ms | 100.00ms | 22.0% | 1.12% | PASS |
| `merge_data_conflicts` | 126.95ms | 250.00ms | 50.8% | 0.36% | PASS |
| `merge_data_conflicts_with_resolve` | 127.07ms | 250.00ms | 50.8% | 0.41% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
