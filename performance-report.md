# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-01 17:52 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261026.354.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/41325349161)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 37s | 8.74s | 11.38s | 1.302× | 1.31% | **PASS** |
| textpk | 69 | 55 | 1h 11m 22s | 7.38s | 8.41s | 1.139× | 2.84% | **PASS** |
| blobpk | 69 | 55 | 1h 34m 15s | 9.92s | 11.89s | 1.199× | 1.75% | **PASS** |
| compositepk | 69 | 55 | 1h 0m 22s | 6.49s | 7.41s | 1.142× | 1.98% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.43ms | 29.28ms | 1.249× | 1.36% | PASS |
| mem_reads | `oltp_range_select` | 9.74ms | 13.06ms | 1.342× | 1.81% | PASS |
| mem_reads | `oltp_sum_range` | 9.32ms | 12.28ms | 1.318× | 1.23% | PASS |
| mem_reads | `oltp_order_range` | 2.49ms | 2.95ms | 1.186× | 1.10% | PASS |
| mem_reads | `oltp_distinct_range` | 3.55ms | 4.04ms | 1.139× | 1.57% | PASS |
| mem_reads | `oltp_index_scan` | 3.82ms | 5.22ms | 1.366× | 1.69% | PASS |
| mem_reads | `select_random_points` | 9.35ms | 10.95ms | 1.171× | 2.30% | PASS |
| mem_reads | `select_random_ranges` | 2.87ms | 3.96ms | 1.377× | 2.10% | PASS |
| mem_reads | `covering_index_scan` | 4.22ms | 4.13ms | 0.979× | 1.48% | PASS |
| mem_reads | `groupby_scan` | 29.67ms | 33.21ms | 1.119× | 1.29% | PASS |
| mem_reads | `index_join` | 5.97ms | 8.03ms | 1.345× | 1.30% | PASS |
| mem_reads | `index_join_scan` | 3.42ms | 4.54ms | 1.327× | 1.96% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.33s | 1.286× | 0.44% | PASS |
| mem_reads | `table_scan` | 1.16s | 1.39s | 1.197× | 0.53% | PASS |
| mem_reads | `oltp_read_only` | 101.23ms | 123.44ms | 1.219× | 1.30% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.57ms | 260.26ms | 1.433× | 0.92% | PASS |
| mem_writes | `oltp_insert` | 15.58ms | 29.41ms | 1.887× | 0.80% | PASS |
| mem_writes | `oltp_update_index` | 50.64ms | 110.53ms | 2.182× | 1.15% | PASS |
| mem_writes | `oltp_update_non_index` | 33.97ms | 63.59ms | 1.872× | 1.43% | PASS |
| mem_writes | `oltp_delete_insert` | 45.17ms | 83.59ms | 1.851× | 1.43% | PASS |
| mem_writes | `oltp_write_only` | 22.04ms | 48.54ms | 2.202× | 1.60% | PASS |
| mem_writes | `types_delete_insert` | 24.57ms | 43.93ms | 1.788× | 1.64% | PASS |
| mem_writes | `oltp_read_write` | 65.64ms | 113.59ms | 1.730× | 0.94% | PASS |
| file_reads | `oltp_point_select` | 92.64ms | 53.10ms | 0.573× | 0.64% | PASS |
| file_reads | `oltp_range_select` | 16.77ms | 15.57ms | 0.929× | 1.08% | PASS |
| file_reads | `oltp_sum_range` | 16.74ms | 14.88ms | 0.889× | 1.47% | PASS |
| file_reads | `oltp_order_range` | 3.28ms | 3.28ms | 1.000× | 1.42% | PASS |
| file_reads | `oltp_distinct_range` | 4.38ms | 4.37ms | 0.998× | 1.15% | PASS |
| file_reads | `oltp_index_scan` | 11.00ms | 8.04ms | 0.731× | 1.31% | PASS |
| file_reads | `select_random_points` | 16.82ms | 13.70ms | 0.814× | 1.66% | PASS |
| file_reads | `select_random_ranges` | 9.84ms | 6.42ms | 0.653× | 1.23% | PASS |
| file_reads | `covering_index_scan` | 11.31ms | 6.89ms | 0.609× | 1.13% | PASS |
| file_reads | `groupby_scan` | 30.54ms | 33.81ms | 1.107× | 1.00% | PASS |
| file_reads | `index_join` | 10.03ms | 9.94ms | 0.992× | 1.27% | PASS |
| file_reads | `index_join_scan` | 4.39ms | 4.91ms | 1.118× | 1.58% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.32s | 1.277× | 0.40% | PASS |
| file_reads | `table_scan` | 1.16s | 1.39s | 1.200× | 0.55% | PASS |
| file_reads | `oltp_read_only` | 202.31ms | 158.15ms | 0.782× | 1.05% | PASS |
| file_writes | `oltp_bulk_insert` | 195.80ms | 280.59ms | 1.433× | 1.02% | PASS |
| file_writes | `oltp_insert` | 22.10ms | 35.82ms | 1.621× | 1.19% | PASS |
| file_writes | `oltp_update_index` | 79.26ms | 137.39ms | 1.733× | 1.08% | PASS |
| file_writes | `oltp_update_non_index` | 57.78ms | 83.66ms | 1.448× | 1.27% | PASS |
| file_writes | `oltp_delete_insert` | 68.73ms | 108.70ms | 1.582× | 1.49% | PASS |
| file_writes | `oltp_write_only` | 45.22ms | 67.38ms | 1.490× | 1.49% | PASS |
| file_writes | `types_delete_insert` | 40.53ms | 53.85ms | 1.329× | 2.00% | PASS |
| file_writes | `oltp_read_write` | 91.21ms | 132.56ms | 1.453× | 1.43% | PASS |
| ac_reads | `oltp_point_select` | 46.69ms | 53.11ms | 1.137× | 0.89% | PASS |
| ac_reads | `oltp_range_select` | 12.60ms | 15.57ms | 1.235× | 1.37% | PASS |
| ac_reads | `oltp_sum_range` | 12.05ms | 14.88ms | 1.235× | 1.35% | PASS |
| ac_reads | `oltp_order_range` | 2.88ms | 3.28ms | 1.138× | 1.88% | PASS |
| ac_reads | `oltp_distinct_range` | 3.93ms | 4.36ms | 1.110× | 1.46% | PASS |
| ac_reads | `oltp_index_scan` | 6.43ms | 8.04ms | 1.251× | 1.53% | PASS |
| ac_reads | `select_random_points` | 12.52ms | 13.66ms | 1.091× | 1.88% | PASS |
| ac_reads | `select_random_ranges` | 5.36ms | 6.43ms | 1.201× | 0.97% | PASS |
| ac_reads | `covering_index_scan` | 6.78ms | 6.88ms | 1.015× | 1.12% | PASS |
| ac_reads | `groupby_scan` | 29.98ms | 33.74ms | 1.125× | 0.45% | PASS |
| ac_reads | `index_join` | 7.52ms | 9.97ms | 1.326× | 1.23% | PASS |
| ac_reads | `index_join_scan` | 3.88ms | 4.93ms | 1.274× | 1.30% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.33s | 1.282× | 0.47% | PASS |
| ac_reads | `table_scan` | 1.16s | 1.39s | 1.197× | 0.43% | PASS |
| ac_reads | `oltp_read_only` | 135.16ms | 158.64ms | 1.174× | 0.65% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.74ms | 63.25ms | 3.050× | 6.64% | PASS |
| ac_writes | `oltp_insert_ac` | 23.02ms | 79.87ms | 3.469× | 4.62% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.82ms | 97.43ms | 3.773× | 4.40% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.08ms | 74.56ms | 3.377× | 7.68% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.53ms | 84.11ms | 3.733× | 4.53% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.68ms | 83.77ms | 3.694× | 4.37% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.53ms | 73.50ms | 3.579× | 6.47% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.33ms | 92.55ms | 3.267× | 4.79% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 17.82ms | 21.68ms | 1.217× | 2.80% | PASS |
| mem_reads | `oltp_range_select` | 9.41ms | 9.27ms | 0.985× | 8.18% | PASS |
| mem_reads | `oltp_sum_range` | 8.32ms | 8.92ms | 1.072× | 8.60% | PASS |
| mem_reads | `oltp_order_range` | 1.81ms | 2.03ms | 1.123× | 3.11% | PASS |
| mem_reads | `oltp_distinct_range` | 2.46ms | 2.70ms | 1.099× | 2.84% | PASS |
| mem_reads | `oltp_index_scan` | 2.92ms | 3.74ms | 1.279× | 4.14% | PASS |
| mem_reads | `select_random_points` | 12.66ms | 14.94ms | 1.180× | 4.00% | PASS |
| mem_reads | `select_random_ranges` | 2.58ms | 3.27ms | 1.265× | 3.70% | PASS |
| mem_reads | `covering_index_scan` | 2.88ms | 2.69ms | 0.931× | 1.50% | PASS |
| mem_reads | `groupby_scan` | 20.23ms | 21.91ms | 1.083× | 1.40% | PASS |
| mem_reads | `index_join` | 5.15ms | 6.48ms | 1.259× | 1.62% | PASS |
| mem_reads | `index_join_scan` | 3.04ms | 4.38ms | 1.442× | 5.53% | PASS |
| mem_reads | `types_table_scan` | 762.52ms | 831.65ms | 1.091× | 1.79% | PASS |
| mem_reads | `table_scan` | 904.18ms | 913.55ms | 1.010× | 3.49% | PASS |
| mem_reads | `oltp_read_only` | 76.84ms | 84.16ms | 1.095× | 1.41% | PASS |
| mem_writes | `oltp_bulk_insert` | 131.22ms | 201.56ms | 1.536× | 0.56% | PASS |
| mem_writes | `oltp_insert` | 14.20ms | 24.72ms | 1.741× | 0.97% | PASS |
| mem_writes | `oltp_update_index` | 48.22ms | 91.48ms | 1.897× | 2.16% | PASS |
| mem_writes | `oltp_update_non_index` | 28.70ms | 55.24ms | 1.925× | 1.72% | PASS |
| mem_writes | `oltp_delete_insert` | 32.63ms | 67.83ms | 2.079× | 1.50% | PASS |
| mem_writes | `oltp_write_only` | 19.64ms | 41.48ms | 2.112× | 1.49% | PASS |
| mem_writes | `types_delete_insert` | 21.32ms | 37.58ms | 1.763× | 1.69% | PASS |
| mem_writes | `oltp_read_write` | 56.06ms | 90.59ms | 1.616× | 1.54% | PASS |
| file_reads | `oltp_point_select` | 40.76ms | 30.08ms | 0.738× | 1.61% | PASS |
| file_reads | `oltp_range_select` | 13.13ms | 10.71ms | 0.816× | 2.54% | PASS |
| file_reads | `oltp_sum_range` | 12.85ms | 10.41ms | 0.810× | 3.22% | PASS |
| file_reads | `oltp_order_range` | 2.55ms | 2.34ms | 0.917× | 2.52% | PASS |
| file_reads | `oltp_distinct_range` | 3.04ms | 2.90ms | 0.956× | 3.96% | PASS |
| file_reads | `oltp_index_scan` | 5.74ms | 5.25ms | 0.915× | 3.67% | PASS |
| file_reads | `select_random_points` | 16.56ms | 17.31ms | 1.045× | 2.62% | PASS |
| file_reads | `select_random_ranges` | 5.06ms | 4.26ms | 0.842× | 2.44% | PASS |
| file_reads | `covering_index_scan` | 5.49ms | 3.95ms | 0.719× | 5.82% | PASS |
| file_reads | `groupby_scan` | 21.32ms | 22.52ms | 1.056× | 0.64% | PASS |
| file_reads | `index_join` | 7.39ms | 8.10ms | 1.096× | 4.00% | PASS |
| file_reads | `index_join_scan` | 4.42ms | 5.02ms | 1.137× | 4.07% | PASS |
| file_reads | `types_table_scan` | 763.99ms | 832.56ms | 1.090× | 2.42% | PASS |
| file_reads | `table_scan` | 900.08ms | 914.65ms | 1.016× | 1.92% | PASS |
| file_reads | `oltp_read_only` | 104.08ms | 92.68ms | 0.890× | 0.76% | PASS |
| file_writes | `oltp_bulk_insert` | 196.45ms | 280.31ms | 1.427× | 1.73% | PASS |
| file_writes | `oltp_insert` | 57.92ms | 56.01ms | 0.967× | 3.17% | PASS |
| file_writes | `oltp_update_index` | 201.09ms | 192.64ms | 0.958× | 2.06% | PASS |
| file_writes | `oltp_update_non_index` | 145.72ms | 130.54ms | 0.896× | 1.32% | PASS |
| file_writes | `oltp_delete_insert` | 159.69ms | 144.47ms | 0.905× | 1.68% | PASS |
| file_writes | `oltp_write_only` | 121.29ms | 102.90ms | 0.848× | 1.78% | PASS |
| file_writes | `types_delete_insert` | 97.34ms | 81.40ms | 0.836× | 3.40% | PASS |
| file_writes | `oltp_read_write` | 155.76ms | 149.29ms | 0.958× | 2.32% | PASS |
| ac_reads | `oltp_point_select` | 24.80ms | 28.48ms | 1.148× | 1.55% | PASS |
| ac_reads | `oltp_range_select` | 11.82ms | 10.71ms | 0.906× | 3.97% | PASS |
| ac_reads | `oltp_sum_range` | 9.95ms | 9.96ms | 1.002× | 7.08% | PASS |
| ac_reads | `oltp_order_range` | 2.13ms | 2.17ms | 1.018× | 3.10% | PASS |
| ac_reads | `oltp_distinct_range` | 2.85ms | 2.88ms | 1.010× | 3.10% | PASS |
| ac_reads | `oltp_index_scan` | 3.73ms | 4.67ms | 1.251× | 3.80% | PASS |
| ac_reads | `select_random_points` | 13.85ms | 15.84ms | 1.143× | 3.58% | PASS |
| ac_reads | `select_random_ranges` | 3.17ms | 3.89ms | 1.228× | 3.25% | PASS |
| ac_reads | `covering_index_scan` | 3.71ms | 3.40ms | 0.918× | 2.55% | PASS |
| ac_reads | `groupby_scan` | 20.19ms | 21.95ms | 1.087× | 0.91% | PASS |
| ac_reads | `index_join` | 5.73ms | 7.05ms | 1.231× | 3.02% | PASS |
| ac_reads | `index_join_scan` | 3.12ms | 4.24ms | 1.359× | 5.45% | PASS |
| ac_reads | `types_table_scan` | 770.11ms | 834.28ms | 1.083× | 2.77% | PASS |
| ac_reads | `table_scan` | 903.18ms | 915.66ms | 1.014× | 4.14% | PASS |
| ac_reads | `oltp_read_only` | 86.87ms | 93.63ms | 1.078× | 1.23% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 34.56ms | 88.83ms | 2.570× | 3.06% | PASS |
| ac_writes | `oltp_insert_ac` | 36.47ms | 99.67ms | 2.733× | 5.08% | PASS |
| ac_writes | `oltp_update_index_ac` | 36.48ms | 108.64ms | 2.978× | 5.00% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 34.12ms | 96.73ms | 2.835× | 3.52% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 35.97ms | 104.56ms | 2.907× | 3.19% | PASS |
| ac_writes | `oltp_write_only_ac` | 36.24ms | 102.99ms | 2.842× | 3.92% | PASS |
| ac_writes | `types_delete_insert_ac` | 33.69ms | 97.07ms | 2.881× | 3.36% | PASS |
| ac_writes | `oltp_read_write_ac` | 38.82ms | 107.78ms | 2.776× | 4.41% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.69ms | 37.27ms | 1.215× | 1.82% | PASS |
| mem_reads | `oltp_range_select` | 13.52ms | 14.09ms | 1.042× | 2.78% | PASS |
| mem_reads | `oltp_sum_range` | 12.19ms | 13.95ms | 1.144× | 2.48% | PASS |
| mem_reads | `oltp_order_range` | 2.96ms | 3.17ms | 1.072× | 1.43% | PASS |
| mem_reads | `oltp_distinct_range` | 4.05ms | 4.24ms | 1.049× | 1.26% | PASS |
| mem_reads | `oltp_index_scan` | 4.61ms | 6.36ms | 1.380× | 2.32% | PASS |
| mem_reads | `select_random_points` | 18.11ms | 20.80ms | 1.148× | 2.04% | PASS |
| mem_reads | `select_random_ranges` | 4.11ms | 5.20ms | 1.265× | 1.76% | PASS |
| mem_reads | `covering_index_scan` | 4.41ms | 4.65ms | 1.056× | 2.31% | PASS |
| mem_reads | `groupby_scan` | 32.06ms | 34.23ms | 1.068× | 1.01% | PASS |
| mem_reads | `index_join` | 6.90ms | 9.50ms | 1.378× | 3.09% | PASS |
| mem_reads | `index_join_scan` | 4.40ms | 5.38ms | 1.221× | 3.97% | PASS |
| mem_reads | `types_table_scan` | 1.09s | 1.25s | 1.147× | 0.75% | PASS |
| mem_reads | `table_scan` | 1.31s | 1.39s | 1.060× | 1.26% | PASS |
| mem_reads | `oltp_read_only` | 120.94ms | 137.20ms | 1.134× | 1.53% | PASS |
| mem_writes | `oltp_bulk_insert` | 243.49ms | 368.57ms | 1.514× | 1.18% | PASS |
| mem_writes | `oltp_insert` | 20.24ms | 42.71ms | 2.110× | 1.15% | PASS |
| mem_writes | `oltp_update_index` | 70.66ms | 142.13ms | 2.012× | 1.91% | PASS |
| mem_writes | `oltp_update_non_index` | 50.16ms | 91.64ms | 1.827× | 1.75% | PASS |
| mem_writes | `oltp_delete_insert` | 50.80ms | 112.39ms | 2.212× | 2.26% | PASS |
| mem_writes | `oltp_write_only` | 28.89ms | 68.01ms | 2.355× | 1.77% | PASS |
| mem_writes | `types_delete_insert` | 33.56ms | 60.08ms | 1.790× | 1.72% | PASS |
| mem_writes | `oltp_read_write` | 87.93ms | 147.81ms | 1.681× | 2.43% | PASS |
| file_reads | `oltp_point_select` | 103.43ms | 61.74ms | 0.597× | 1.21% | PASS |
| file_reads | `oltp_range_select` | 21.72ms | 16.73ms | 0.770× | 2.71% | PASS |
| file_reads | `oltp_sum_range` | 20.66ms | 16.82ms | 0.814× | 1.67% | PASS |
| file_reads | `oltp_order_range` | 3.83ms | 3.48ms | 0.909× | 2.07% | PASS |
| file_reads | `oltp_distinct_range` | 4.96ms | 4.56ms | 0.921× | 1.25% | PASS |
| file_reads | `oltp_index_scan` | 12.50ms | 9.11ms | 0.729× | 2.49% | PASS |
| file_reads | `select_random_points` | 27.71ms | 24.52ms | 0.885× | 2.64% | PASS |
| file_reads | `select_random_ranges` | 11.66ms | 7.72ms | 0.662× | 1.75% | PASS |
| file_reads | `covering_index_scan` | 12.24ms | 7.24ms | 0.592× | 3.20% | PASS |
| file_reads | `groupby_scan` | 33.39ms | 34.95ms | 1.047× | 1.24% | PASS |
| file_reads | `index_join` | 11.56ms | 11.44ms | 0.989× | 4.35% | PASS |
| file_reads | `index_join_scan` | 5.56ms | 5.89ms | 1.060× | 3.89% | PASS |
| file_reads | `types_table_scan` | 1.10s | 1.25s | 1.138× | 1.27% | PASS |
| file_reads | `table_scan` | 1.33s | 1.39s | 1.046× | 1.21% | PASS |
| file_reads | `oltp_read_only` | 233.19ms | 174.30ms | 0.747× | 0.79% | PASS |
| file_writes | `oltp_bulk_insert` | 263.40ms | 396.39ms | 1.505× | 0.91% | PASS |
| file_writes | `oltp_insert` | 32.58ms | 55.46ms | 1.702× | 3.13% | PASS |
| file_writes | `oltp_update_index` | 107.11ms | 177.49ms | 1.657× | 1.47% | PASS |
| file_writes | `oltp_update_non_index` | 81.92ms | 117.46ms | 1.434× | 1.71% | PASS |
| file_writes | `oltp_delete_insert` | 85.41ms | 141.47ms | 1.656× | 1.34% | PASS |
| file_writes | `oltp_write_only` | 57.28ms | 92.23ms | 1.610× | 1.68% | PASS |
| file_writes | `types_delete_insert` | 54.29ms | 77.06ms | 1.419× | 1.27% | PASS |
| file_writes | `oltp_read_write` | 121.78ms | 171.60ms | 1.409× | 1.87% | PASS |
| ac_reads | `oltp_point_select` | 55.81ms | 61.80ms | 1.107× | 1.30% | PASS |
| ac_reads | `oltp_range_select` | 17.12ms | 16.73ms | 0.977× | 1.32% | PASS |
| ac_reads | `oltp_sum_range` | 15.87ms | 16.73ms | 1.054× | 1.45% | PASS |
| ac_reads | `oltp_order_range` | 3.39ms | 3.49ms | 1.029× | 1.28% | PASS |
| ac_reads | `oltp_distinct_range` | 4.49ms | 4.57ms | 1.019× | 0.85% | PASS |
| ac_reads | `oltp_index_scan` | 7.56ms | 8.98ms | 1.188× | 1.49% | PASS |
| ac_reads | `select_random_points` | 22.18ms | 24.12ms | 1.088× | 2.66% | PASS |
| ac_reads | `select_random_ranges` | 6.89ms | 7.72ms | 1.121× | 1.45% | PASS |
| ac_reads | `covering_index_scan` | 7.81ms | 7.21ms | 0.924× | 2.07% | PASS |
| ac_reads | `groupby_scan` | 32.69ms | 34.87ms | 1.067× | 1.10% | PASS |
| ac_reads | `index_join` | 9.34ms | 11.42ms | 1.223× | 3.28% | PASS |
| ac_reads | `index_join_scan` | 5.12ms | 5.94ms | 1.159× | 2.78% | PASS |
| ac_reads | `types_table_scan` | 1.09s | 1.24s | 1.139× | 0.97% | PASS |
| ac_reads | `table_scan` | 1.31s | 1.39s | 1.064× | 1.19% | PASS |
| ac_reads | `oltp_read_only` | 161.40ms | 174.43ms | 1.081× | 1.58% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.59ms | 64.15ms | 2.972× | 6.62% | PASS |
| ac_writes | `oltp_insert_ac` | 23.98ms | 86.73ms | 3.617× | 4.94% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.68ms | 94.86ms | 3.693× | 6.90% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.75ms | 76.08ms | 3.498× | 5.73% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.59ms | 86.07ms | 3.648× | 6.70% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.20ms | 87.33ms | 3.609× | 5.31% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.98ms | 76.24ms | 3.635× | 6.96% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.32ms | 93.58ms | 3.087× | 6.71% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 19.99ms | 22.90ms | 1.146× | 2.56% | PASS |
| mem_reads | `oltp_range_select` | 10.47ms | 10.77ms | 1.029× | 2.40% | PASS |
| mem_reads | `oltp_sum_range` | 9.79ms | 10.83ms | 1.106× | 1.99% | PASS |
| mem_reads | `oltp_order_range` | 2.10ms | 2.10ms | 0.998× | 2.15% | PASS |
| mem_reads | `oltp_distinct_range` | 2.58ms | 2.60ms | 1.006× | 2.03% | PASS |
| mem_reads | `oltp_index_scan` | 2.80ms | 3.40ms | 1.217× | 3.03% | PASS |
| mem_reads | `select_random_points` | 16.16ms | 18.16ms | 1.124× | 3.25% | PASS |
| mem_reads | `select_random_ranges` | 4.23ms | 4.76ms | 1.125× | 2.80% | PASS |
| mem_reads | `covering_index_scan` | 2.31ms | 2.34ms | 1.013× | 1.80% | PASS |
| mem_reads | `groupby_scan` | 19.72ms | 20.55ms | 1.042× | 1.54% | PASS |
| mem_reads | `index_join` | 4.80ms | 5.79ms | 1.205× | 1.09% | PASS |
| mem_reads | `index_join_scan` | 2.30ms | 3.92ms | 1.705× | 4.57% | PASS |
| mem_reads | `types_table_scan` | 673.94ms | 725.30ms | 1.076× | 0.90% | PASS |
| mem_reads | `table_scan` | 764.62ms | 810.99ms | 1.061× | 0.96% | PASS |
| mem_reads | `oltp_read_only` | 77.85ms | 83.30ms | 1.070× | 0.91% | PASS |
| mem_writes | `oltp_bulk_insert` | 141.31ms | 202.31ms | 1.432× | 0.89% | PASS |
| mem_writes | `oltp_insert` | 11.03ms | 22.75ms | 2.064× | 0.82% | PASS |
| mem_writes | `oltp_update_index` | 40.02ms | 77.34ms | 1.933× | 1.59% | PASS |
| mem_writes | `oltp_update_non_index` | 31.38ms | 56.89ms | 1.813× | 2.01% | PASS |
| mem_writes | `oltp_delete_insert` | 29.56ms | 61.96ms | 2.096× | 1.27% | PASS |
| mem_writes | `oltp_write_only` | 16.62ms | 40.41ms | 2.432× | 2.15% | PASS |
| mem_writes | `types_delete_insert` | 20.45ms | 35.81ms | 1.751× | 1.90% | PASS |
| mem_writes | `oltp_read_write` | 52.83ms | 87.67ms | 1.659× | 1.64% | PASS |
| file_reads | `oltp_point_select` | 71.28ms | 40.57ms | 0.569× | 1.16% | PASS |
| file_reads | `oltp_range_select` | 16.33ms | 12.74ms | 0.780× | 1.49% | PASS |
| file_reads | `oltp_sum_range` | 15.37ms | 12.60ms | 0.820× | 0.87% | PASS |
| file_reads | `oltp_order_range` | 2.76ms | 2.35ms | 0.849× | 1.52% | PASS |
| file_reads | `oltp_distinct_range` | 3.31ms | 2.84ms | 0.859× | 1.27% | PASS |
| file_reads | `oltp_index_scan` | 8.41ms | 5.64ms | 0.671× | 2.81% | PASS |
| file_reads | `select_random_points` | 22.31ms | 20.34ms | 0.911× | 2.40% | PASS |
| file_reads | `select_random_ranges` | 9.81ms | 6.92ms | 0.706× | 1.40% | PASS |
| file_reads | `covering_index_scan` | 7.99ms | 4.32ms | 0.540× | 2.37% | PASS |
| file_reads | `groupby_scan` | 20.69ms | 20.87ms | 1.009× | 1.03% | PASS |
| file_reads | `index_join` | 7.98ms | 7.28ms | 0.911× | 3.97% | PASS |
| file_reads | `index_join_scan` | 3.38ms | 4.11ms | 1.216× | 3.14% | PASS |
| file_reads | `types_table_scan` | 681.07ms | 727.28ms | 1.068× | 1.32% | PASS |
| file_reads | `table_scan` | 765.39ms | 808.59ms | 1.056× | 1.09% | PASS |
| file_reads | `oltp_read_only` | 152.70ms | 110.68ms | 0.725× | 2.00% | PASS |
| file_writes | `oltp_bulk_insert` | 190.88ms | 259.94ms | 1.362× | 2.06% | PASS |
| file_writes | `oltp_insert` | 19.64ms | 38.17ms | 1.943× | 4.10% | PASS |
| file_writes | `oltp_update_index` | 141.40ms | 147.39ms | 1.042× | 3.57% | PASS |
| file_writes | `oltp_update_non_index` | 120.82ms | 109.14ms | 0.903× | 1.50% | PASS |
| file_writes | `oltp_delete_insert` | 120.97ms | 119.59ms | 0.989× | 1.08% | PASS |
| file_writes | `oltp_write_only` | 80.82ms | 89.01ms | 1.101× | 1.00% | PASS |
| file_writes | `types_delete_insert` | 70.47ms | 69.34ms | 0.984× | 1.23% | PASS |
| file_writes | `oltp_read_write` | 121.42ms | 142.67ms | 1.175× | 3.22% | PASS |
| ac_reads | `oltp_point_select` | 37.81ms | 41.43ms | 1.096× | 1.58% | PASS |
| ac_reads | `oltp_range_select` | 13.12ms | 13.15ms | 1.002× | 1.48% | PASS |
| ac_reads | `oltp_sum_range` | 12.21ms | 12.90ms | 1.056× | 1.85% | PASS |
| ac_reads | `oltp_order_range` | 2.42ms | 2.34ms | 0.970× | 1.98% | PASS |
| ac_reads | `oltp_distinct_range` | 2.94ms | 2.84ms | 0.966× | 1.86% | PASS |
| ac_reads | `oltp_index_scan` | 4.71ms | 5.61ms | 1.190× | 3.49% | PASS |
| ac_reads | `select_random_points` | 18.28ms | 20.09ms | 1.099× | 2.10% | PASS |
| ac_reads | `select_random_ranges` | 6.28ms | 6.82ms | 1.086× | 1.70% | PASS |
| ac_reads | `covering_index_scan` | 4.24ms | 4.39ms | 1.036× | 2.41% | PASS |
| ac_reads | `groupby_scan` | 20.94ms | 21.21ms | 1.013× | 1.20% | PASS |
| ac_reads | `index_join` | 6.14ms | 7.35ms | 1.198× | 4.15% | PASS |
| ac_reads | `index_join_scan` | 3.03ms | 4.13ms | 1.361× | 3.32% | PASS |
| ac_reads | `types_table_scan` | 675.39ms | 727.88ms | 1.078× | 1.19% | PASS |
| ac_reads | `table_scan` | 766.56ms | 809.32ms | 1.056× | 0.78% | PASS |
| ac_reads | `oltp_read_only` | 103.70ms | 109.42ms | 1.055× | 1.20% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.63ms | 53.16ms | 2.457× | 3.95% | PASS |
| ac_writes | `oltp_insert_ac` | 25.61ms | 71.12ms | 2.777× | 3.96% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.72ms | 78.73ms | 2.947× | 6.01% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.27ms | 62.55ms | 2.940× | 3.08% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.32ms | 70.70ms | 2.907× | 3.71% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.69ms | 69.53ms | 2.817× | 2.52% | PASS |
| ac_writes | `types_delete_insert_ac` | 25.41ms | 61.21ms | 2.409× | 10.29% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.28ms | 76.84ms | 2.817× | 4.67% | PASS |

</details>

## Version-control latency

Wall time: 2m 27s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 86.14ms | 200.00ms | 43.1% | 0.71% | PASS |
| `status_dirty_many_tables` | 89.38ms | 200.00ms | 44.7% | 0.81% | PASS |
| `diff_regular_working_one_table` | 81.93ms | 150.00ms | 54.6% | 0.59% | PASS |
| `diff_regular_working_many_tables` | 95.35ms | 200.00ms | 47.7% | 0.56% | PASS |
| `diff_stat_working_many_tables` | 94.60ms | 200.00ms | 47.3% | 0.92% | PASS |
| `diff_schema_working_many_tables` | 95.28ms | 200.00ms | 47.6% | 0.77% | PASS |
| `branch_list_many_branches` | 25.29ms | 100.00ms | 25.3% | 1.39% | PASS |
| `branch_create_delete` | 27.26ms | 100.00ms | 27.3% | 1.61% | PASS |
| `checkout_branch_clean` | 58.32ms | 200.00ms | 29.2% | 1.17% | PASS |
| `merge_data_no_conflicts` | 31.46ms | 150.00ms | 21.0% | 1.49% | PASS |
| `merge_schema_no_conflicts` | 24.59ms | 100.00ms | 24.6% | 1.95% | PASS |
| `merge_data_conflicts` | 130.26ms | 250.00ms | 52.1% | 0.39% | PASS |
| `merge_data_conflicts_with_resolve` | 130.36ms | 250.00ms | 52.1% | 0.37% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
