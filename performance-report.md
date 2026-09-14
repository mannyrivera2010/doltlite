# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-14 17:39 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260907.300.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/34865605141)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 4s | 9.71s | 11.24s | 1.158× | 1.76% | **PASS** |
| textpk | 69 | 55 | 1h 37m 40s | 10.61s | 11.07s | 1.044× | 2.15% | **PASS** |
| blobpk | 69 | 55 | 1h 33m 40s | 10.60s | 12.21s | 1.152× | 1.87% | **PASS** |
| compositepk | 69 | 55 | 1h 28m 44s | 10.25s | 12.23s | 1.193× | 1.52% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.44ms | 28.54ms | 1.079× | 2.65% | PASS |
| mem_reads | `oltp_range_select` | 11.84ms | 12.38ms | 1.046× | 2.87% | PASS |
| mem_reads | `oltp_sum_range` | 10.68ms | 11.74ms | 1.100× | 2.32% | PASS |
| mem_reads | `oltp_order_range` | 2.75ms | 2.91ms | 1.057× | 1.11% | PASS |
| mem_reads | `oltp_distinct_range` | 3.83ms | 3.91ms | 1.022× | 0.86% | PASS |
| mem_reads | `oltp_index_scan` | 4.16ms | 5.37ms | 1.290× | 1.66% | PASS |
| mem_reads | `select_random_points` | 11.68ms | 11.55ms | 0.989× | 4.77% | PASS |
| mem_reads | `select_random_ranges` | 3.21ms | 4.07ms | 1.268× | 1.64% | PASS |
| mem_reads | `covering_index_scan` | 4.38ms | 4.23ms | 0.965× | 2.19% | PASS |
| mem_reads | `groupby_scan` | 32.39ms | 34.41ms | 1.062× | 0.60% | PASS |
| mem_reads | `index_join` | 5.97ms | 7.82ms | 1.310× | 1.81% | PASS |
| mem_reads | `index_join_scan` | 3.69ms | 4.74ms | 1.284× | 2.16% | PASS |
| mem_reads | `types_table_scan` | 1.15s | 1.28s | 1.116× | 1.45% | PASS |
| mem_reads | `table_scan` | 1.34s | 1.39s | 1.044× | 1.16% | PASS |
| mem_reads | `oltp_read_only` | 108.64ms | 115.60ms | 1.064× | 2.54% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.31ms | 240.38ms | 1.341× | 0.88% | PASS |
| mem_writes | `oltp_insert` | 15.94ms | 28.49ms | 1.787× | 1.20% | PASS |
| mem_writes | `oltp_update_index` | 53.29ms | 107.72ms | 2.022× | 2.51% | PASS |
| mem_writes | `oltp_update_non_index` | 37.55ms | 59.85ms | 1.594× | 2.29% | PASS |
| mem_writes | `oltp_delete_insert` | 47.36ms | 80.16ms | 1.693× | 1.92% | PASS |
| mem_writes | `oltp_write_only` | 23.35ms | 45.89ms | 1.966× | 2.62% | PASS |
| mem_writes | `types_delete_insert` | 25.16ms | 39.97ms | 1.589× | 2.39% | PASS |
| mem_writes | `oltp_read_write` | 65.69ms | 104.21ms | 1.586× | 1.27% | PASS |
| file_reads | `oltp_point_select` | 119.69ms | 59.01ms | 0.493× | 0.92% | PASS |
| file_reads | `oltp_range_select` | 20.50ms | 15.13ms | 0.738× | 1.48% | PASS |
| file_reads | `oltp_sum_range` | 19.50ms | 14.77ms | 0.757× | 1.45% | PASS |
| file_reads | `oltp_order_range` | 3.75ms | 3.23ms | 0.862× | 1.14% | PASS |
| file_reads | `oltp_distinct_range` | 4.78ms | 4.22ms | 0.884× | 1.02% | PASS |
| file_reads | `oltp_index_scan` | 13.67ms | 8.38ms | 0.613× | 2.62% | PASS |
| file_reads | `select_random_points` | 20.77ms | 14.52ms | 0.699× | 1.95% | PASS |
| file_reads | `select_random_ranges` | 12.74ms | 7.20ms | 0.565× | 1.06% | PASS |
| file_reads | `covering_index_scan` | 14.21ms | 7.50ms | 0.528× | 1.55% | PASS |
| file_reads | `groupby_scan` | 33.27ms | 34.84ms | 1.047× | 0.89% | PASS |
| file_reads | `index_join` | 11.42ms | 10.23ms | 0.896× | 1.73% | PASS |
| file_reads | `index_join_scan` | 4.76ms | 5.27ms | 1.106× | 2.32% | PASS |
| file_reads | `types_table_scan` | 1.14s | 1.28s | 1.126× | 1.92% | PASS |
| file_reads | `table_scan` | 1.27s | 1.38s | 1.086× | 1.07% | PASS |
| file_reads | `oltp_read_only` | 240.58ms | 160.12ms | 0.666× | 0.84% | PASS |
| file_writes | `oltp_bulk_insert` | 194.51ms | 258.85ms | 1.331× | 1.08% | PASS |
| file_writes | `oltp_insert` | 22.69ms | 36.67ms | 1.617× | 1.64% | PASS |
| file_writes | `oltp_update_index` | 82.76ms | 133.66ms | 1.615× | 1.89% | PASS |
| file_writes | `oltp_update_non_index` | 63.25ms | 84.39ms | 1.334× | 1.89% | PASS |
| file_writes | `oltp_delete_insert` | 71.75ms | 101.88ms | 1.420× | 1.93% | PASS |
| file_writes | `oltp_write_only` | 46.43ms | 67.70ms | 1.458× | 1.85% | PASS |
| file_writes | `types_delete_insert` | 41.90ms | 54.35ms | 1.297× | 1.31% | PASS |
| file_writes | `oltp_read_write` | 97.09ms | 130.44ms | 1.344× | 1.85% | PASS |
| ac_reads | `oltp_point_select` | 57.53ms | 59.78ms | 1.039× | 0.93% | PASS |
| ac_reads | `oltp_range_select` | 15.03ms | 15.46ms | 1.029× | 2.00% | PASS |
| ac_reads | `oltp_sum_range` | 13.85ms | 15.05ms | 1.087× | 1.78% | PASS |
| ac_reads | `oltp_order_range` | 3.17ms | 3.25ms | 1.023× | 1.26% | PASS |
| ac_reads | `oltp_distinct_range` | 4.15ms | 4.24ms | 1.021× | 0.83% | PASS |
| ac_reads | `oltp_index_scan` | 7.28ms | 8.43ms | 1.158× | 1.41% | PASS |
| ac_reads | `select_random_points` | 14.59ms | 14.60ms | 1.001× | 2.82% | PASS |
| ac_reads | `select_random_ranges` | 6.33ms | 7.22ms | 1.141× | 1.28% | PASS |
| ac_reads | `covering_index_scan` | 7.86ms | 7.69ms | 0.978× | 1.32% | PASS |
| ac_reads | `groupby_scan` | 32.95ms | 34.94ms | 1.061× | 0.59% | PASS |
| ac_reads | `index_join` | 8.00ms | 10.09ms | 1.261× | 1.76% | PASS |
| ac_reads | `index_join_scan` | 4.07ms | 5.13ms | 1.262× | 2.01% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.27s | 1.142× | 0.91% | PASS |
| ac_reads | `table_scan` | 1.40s | 1.43s | 1.018× | 1.23% | PASS |
| ac_reads | `oltp_read_only` | 161.82ms | 168.19ms | 1.039× | 1.22% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.80ms | 76.67ms | 4.306× | 10.10% | PASS |
| ac_writes | `oltp_insert_ac` | 19.18ms | 88.16ms | 4.598× | 6.28% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.70ms | 103.02ms | 4.977× | 7.10% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.27ms | 81.39ms | 4.713× | 7.89% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.87ms | 87.87ms | 4.658× | 8.43% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.88ms | 84.14ms | 4.457× | 4.54% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.85ms | 74.00ms | 4.669× | 4.47% | PASS |
| ac_writes | `oltp_read_write_ac` | 23.84ms | 92.67ms | 3.886× | 3.50% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.52ms | 29.71ms | 1.120× | 2.15% | PASS |
| mem_reads | `oltp_range_select` | 14.10ms | 11.46ms | 0.813× | 2.53% | PASS |
| mem_reads | `oltp_sum_range` | 11.73ms | 11.36ms | 0.968× | 1.73% | PASS |
| mem_reads | `oltp_order_range` | 2.79ms | 2.60ms | 0.931× | 1.07% | PASS |
| mem_reads | `oltp_distinct_range` | 3.58ms | 3.39ms | 0.946× | 1.34% | PASS |
| mem_reads | `oltp_index_scan` | 4.04ms | 5.21ms | 1.290× | 2.39% | PASS |
| mem_reads | `select_random_points` | 16.05ms | 17.71ms | 1.104× | 2.50% | PASS |
| mem_reads | `select_random_ranges` | 3.76ms | 4.39ms | 1.169× | 1.57% | PASS |
| mem_reads | `covering_index_scan` | 4.70ms | 4.02ms | 0.855× | 4.82% | PASS |
| mem_reads | `groupby_scan` | 28.15ms | 28.22ms | 1.003× | 0.98% | PASS |
| mem_reads | `index_join` | 6.76ms | 8.54ms | 1.264× | 4.12% | PASS |
| mem_reads | `index_join_scan` | 4.53ms | 5.23ms | 1.155× | 2.14% | PASS |
| mem_reads | `types_table_scan` | 1.15s | 1.06s | 0.923× | 1.57% | PASS |
| mem_reads | `table_scan` | 1.47s | 1.16s | 0.790× | 0.62% | PASS |
| mem_reads | `oltp_read_only` | 115.45ms | 110.19ms | 0.954× | 2.30% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.86ms | 265.17ms | 1.458× | 0.77% | PASS |
| mem_writes | `oltp_insert` | 19.21ms | 30.94ms | 1.610× | 2.48% | PASS |
| mem_writes | `oltp_update_index` | 65.11ms | 113.75ms | 1.747× | 2.92% | PASS |
| mem_writes | `oltp_update_non_index` | 44.79ms | 72.75ms | 1.624× | 1.37% | PASS |
| mem_writes | `oltp_delete_insert` | 46.14ms | 87.89ms | 1.905× | 2.84% | PASS |
| mem_writes | `oltp_write_only` | 25.97ms | 51.40ms | 1.979× | 3.01% | PASS |
| mem_writes | `types_delete_insert` | 28.36ms | 43.69ms | 1.541× | 2.05% | PASS |
| mem_writes | `oltp_read_write` | 78.03ms | 110.21ms | 1.412× | 2.19% | PASS |
| file_reads | `oltp_point_select` | 100.83ms | 53.74ms | 0.533× | 0.96% | PASS |
| file_reads | `oltp_range_select` | 21.30ms | 13.80ms | 0.648× | 1.67% | PASS |
| file_reads | `oltp_sum_range` | 19.39ms | 13.77ms | 0.710× | 1.88% | PASS |
| file_reads | `oltp_order_range` | 3.58ms | 2.85ms | 0.796× | 0.96% | PASS |
| file_reads | `oltp_distinct_range` | 4.38ms | 3.62ms | 0.827× | 1.06% | PASS |
| file_reads | `oltp_index_scan` | 11.90ms | 7.76ms | 0.652× | 1.02% | PASS |
| file_reads | `select_random_points` | 24.42ms | 19.85ms | 0.813× | 2.26% | PASS |
| file_reads | `select_random_ranges` | 11.25ms | 6.78ms | 0.602× | 0.67% | PASS |
| file_reads | `covering_index_scan` | 12.87ms | 6.53ms | 0.508× | 0.98% | PASS |
| file_reads | `groupby_scan` | 28.72ms | 28.32ms | 0.986× | 0.61% | PASS |
| file_reads | `index_join` | 10.89ms | 9.48ms | 0.871× | 1.60% | PASS |
| file_reads | `index_join_scan` | 5.08ms | 5.17ms | 1.018× | 1.80% | PASS |
| file_reads | `types_table_scan` | 1.00s | 1.01s | 1.004× | 5.67% | PASS |
| file_reads | `table_scan` | 1.17s | 1.08s | 0.924× | 6.76% | PASS |
| file_reads | `oltp_read_only` | 209.48ms | 139.16ms | 0.664× | 1.44% | PASS |
| file_writes | `oltp_bulk_insert` | 256.93ms | 357.58ms | 1.392× | 6.86% | PASS |
| file_writes | `oltp_insert` | 103.99ms | 69.20ms | 0.665× | 24.65% | PASS |
| file_writes | `oltp_update_index` | 290.61ms | 220.13ms | 0.757× | 17.43% | PASS |
| file_writes | `oltp_update_non_index` | 203.98ms | 167.86ms | 0.823× | 19.46% | PASS |
| file_writes | `oltp_delete_insert` | 212.41ms | 188.33ms | 0.887× | 17.10% | PASS |
| file_writes | `oltp_write_only` | 149.32ms | 122.81ms | 0.822× | 24.84% | PASS |
| file_writes | `types_delete_insert` | 119.19ms | 105.37ms | 0.884× | 13.28% | PASS |
| file_writes | `oltp_read_write` | 254.16ms | 184.86ms | 0.727× | 28.21% | PASS |
| ac_reads | `oltp_point_select` | 51.75ms | 55.13ms | 1.065× | 0.81% | PASS |
| ac_reads | `oltp_range_select` | 17.20ms | 14.15ms | 0.823× | 2.33% | PASS |
| ac_reads | `oltp_sum_range` | 15.02ms | 14.19ms | 0.945× | 1.33% | PASS |
| ac_reads | `oltp_order_range` | 3.19ms | 2.86ms | 0.897× | 1.41% | PASS |
| ac_reads | `oltp_distinct_range` | 3.92ms | 3.62ms | 0.923× | 1.17% | PASS |
| ac_reads | `oltp_index_scan` | 7.07ms | 7.85ms | 1.111× | 1.35% | PASS |
| ac_reads | `select_random_points` | 19.60ms | 20.35ms | 1.038× | 2.06% | PASS |
| ac_reads | `select_random_ranges` | 6.32ms | 6.83ms | 1.081× | 0.81% | PASS |
| ac_reads | `covering_index_scan` | 7.91ms | 6.48ms | 0.819× | 1.72% | PASS |
| ac_reads | `groupby_scan` | 28.13ms | 28.39ms | 1.009× | 1.25% | PASS |
| ac_reads | `index_join` | 8.35ms | 9.53ms | 1.142× | 1.45% | PASS |
| ac_reads | `index_join_scan` | 4.65ms | 5.22ms | 1.124× | 2.64% | PASS |
| ac_reads | `types_table_scan` | 956.70ms | 986.62ms | 1.031× | 6.12% | PASS |
| ac_reads | `table_scan` | 1.22s | 1.10s | 0.902× | 5.54% | PASS |
| ac_reads | `oltp_read_only` | 141.25ms | 140.34ms | 0.994× | 1.68% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 47.64ms | 169.50ms | 3.558× | 69.73% | PASS |
| ac_writes | `oltp_insert_ac` | 36.29ms | 157.38ms | 4.337× | 68.16% | PASS |
| ac_writes | `oltp_update_index_ac` | 125.28ms | 283.77ms | 2.265× | 66.46% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 58.70ms | 265.90ms | 4.530× | 65.79% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 86.97ms | 249.19ms | 2.865× | 50.76% | PASS |
| ac_writes | `oltp_write_only_ac` | 40.37ms | 148.73ms | 3.684× | 47.59% | PASS |
| ac_writes | `types_delete_insert_ac` | 43.94ms | 127.92ms | 2.911× | 52.73% | PASS |
| ac_writes | `oltp_read_write_ac` | 100.74ms | 212.98ms | 2.114× | 56.84% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.51ms | 38.25ms | 1.214× | 2.46% | PASS |
| mem_reads | `oltp_range_select` | 14.06ms | 14.50ms | 1.032× | 3.34% | PASS |
| mem_reads | `oltp_sum_range` | 12.71ms | 14.45ms | 1.137× | 2.05% | PASS |
| mem_reads | `oltp_order_range` | 3.01ms | 3.20ms | 1.064× | 1.28% | PASS |
| mem_reads | `oltp_distinct_range` | 4.09ms | 4.27ms | 1.046× | 1.02% | PASS |
| mem_reads | `oltp_index_scan` | 4.79ms | 6.58ms | 1.373× | 2.13% | PASS |
| mem_reads | `select_random_points` | 19.05ms | 22.20ms | 1.165× | 2.73% | PASS |
| mem_reads | `select_random_ranges` | 4.16ms | 5.24ms | 1.261× | 1.86% | PASS |
| mem_reads | `covering_index_scan` | 4.67ms | 4.82ms | 1.033× | 3.64% | PASS |
| mem_reads | `groupby_scan` | 32.51ms | 34.03ms | 1.047× | 0.78% | PASS |
| mem_reads | `index_join` | 7.15ms | 10.17ms | 1.422× | 2.75% | PASS |
| mem_reads | `index_join_scan` | 4.39ms | 5.80ms | 1.321× | 3.82% | PASS |
| mem_reads | `types_table_scan` | 1.17s | 1.27s | 1.088× | 1.19% | PASS |
| mem_reads | `table_scan` | 1.44s | 1.43s | 0.990× | 0.84% | PASS |
| mem_reads | `oltp_read_only` | 129.86ms | 143.35ms | 1.104× | 1.80% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.51ms | 355.07ms | 1.501× | 0.85% | PASS |
| mem_writes | `oltp_insert` | 20.70ms | 40.66ms | 1.964× | 2.24% | PASS |
| mem_writes | `oltp_update_index` | 74.22ms | 140.03ms | 1.887× | 3.02% | PASS |
| mem_writes | `oltp_update_non_index` | 50.91ms | 88.12ms | 1.731× | 1.57% | PASS |
| mem_writes | `oltp_delete_insert` | 52.13ms | 109.50ms | 2.100× | 2.14% | PASS |
| mem_writes | `oltp_write_only` | 29.42ms | 64.56ms | 2.194× | 1.88% | PASS |
| mem_writes | `types_delete_insert` | 32.94ms | 55.99ms | 1.700× | 1.75% | PASS |
| mem_writes | `oltp_read_write` | 91.35ms | 147.21ms | 1.611× | 1.67% | PASS |
| file_reads | `oltp_point_select` | 106.76ms | 64.21ms | 0.601× | 1.09% | PASS |
| file_reads | `oltp_range_select` | 22.23ms | 17.19ms | 0.773× | 1.43% | PASS |
| file_reads | `oltp_sum_range` | 20.91ms | 17.07ms | 0.816× | 2.12% | PASS |
| file_reads | `oltp_order_range` | 3.91ms | 3.53ms | 0.903× | 2.04% | PASS |
| file_reads | `oltp_distinct_range` | 5.02ms | 4.61ms | 0.918× | 1.10% | PASS |
| file_reads | `oltp_index_scan` | 12.62ms | 9.32ms | 0.739× | 1.46% | PASS |
| file_reads | `select_random_points` | 28.50ms | 25.17ms | 0.883× | 3.19% | PASS |
| file_reads | `select_random_ranges` | 11.88ms | 7.87ms | 0.663× | 0.93% | PASS |
| file_reads | `covering_index_scan` | 12.98ms | 7.37ms | 0.568× | 2.09% | PASS |
| file_reads | `groupby_scan` | 33.35ms | 34.53ms | 1.035× | 0.87% | PASS |
| file_reads | `index_join` | 11.96ms | 11.75ms | 0.982× | 2.58% | PASS |
| file_reads | `index_join_scan` | 5.43ms | 6.05ms | 1.115× | 2.41% | PASS |
| file_reads | `types_table_scan` | 1.14s | 1.26s | 1.100× | 1.34% | PASS |
| file_reads | `table_scan` | 1.43s | 1.42s | 0.989× | 1.57% | PASS |
| file_reads | `oltp_read_only` | 241.57ms | 181.23ms | 0.750× | 0.87% | PASS |
| file_writes | `oltp_bulk_insert` | 256.61ms | 380.35ms | 1.482× | 0.85% | PASS |
| file_writes | `oltp_insert` | 32.82ms | 53.24ms | 1.622× | 1.98% | PASS |
| file_writes | `oltp_update_index` | 110.19ms | 171.87ms | 1.560× | 2.88% | PASS |
| file_writes | `oltp_update_non_index` | 83.48ms | 113.76ms | 1.363× | 1.38% | PASS |
| file_writes | `oltp_delete_insert` | 86.66ms | 137.54ms | 1.587× | 1.52% | PASS |
| file_writes | `oltp_write_only` | 59.93ms | 88.43ms | 1.476× | 1.87% | PASS |
| file_writes | `types_delete_insert` | 55.38ms | 76.75ms | 1.386× | 1.49% | PASS |
| file_writes | `oltp_read_write` | 129.79ms | 174.68ms | 1.346× | 1.82% | PASS |
| ac_reads | `oltp_point_select` | 57.45ms | 65.36ms | 1.138× | 1.29% | PASS |
| ac_reads | `oltp_range_select` | 17.10ms | 17.34ms | 1.014× | 2.04% | PASS |
| ac_reads | `oltp_sum_range` | 16.14ms | 17.37ms | 1.076× | 2.72% | PASS |
| ac_reads | `oltp_order_range` | 3.42ms | 3.55ms | 1.038× | 1.44% | PASS |
| ac_reads | `oltp_distinct_range` | 4.49ms | 4.63ms | 1.030× | 1.48% | PASS |
| ac_reads | `oltp_index_scan` | 7.73ms | 9.34ms | 1.207× | 1.73% | PASS |
| ac_reads | `select_random_points` | 22.60ms | 25.26ms | 1.118× | 2.23% | PASS |
| ac_reads | `select_random_ranges` | 7.01ms | 7.93ms | 1.131× | 1.71% | PASS |
| ac_reads | `covering_index_scan` | 8.11ms | 7.36ms | 0.908× | 2.94% | PASS |
| ac_reads | `groupby_scan` | 33.10ms | 34.73ms | 1.049× | 0.99% | PASS |
| ac_reads | `index_join` | 9.39ms | 11.86ms | 1.263× | 3.01% | PASS |
| ac_reads | `index_join_scan` | 4.95ms | 6.29ms | 1.271× | 4.10% | PASS |
| ac_reads | `types_table_scan` | 1.19s | 1.27s | 1.070× | 1.11% | PASS |
| ac_reads | `table_scan` | 1.45s | 1.43s | 0.984× | 0.61% | PASS |
| ac_reads | `oltp_read_only` | 169.70ms | 181.80ms | 1.071× | 1.35% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.32ms | 83.81ms | 3.593× | 5.80% | PASS |
| ac_writes | `oltp_insert_ac` | 25.40ms | 107.86ms | 4.247× | 5.30% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.99ms | 119.77ms | 4.279× | 4.12% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.63ms | 97.05ms | 4.107× | 5.48% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.88ms | 108.21ms | 4.349× | 6.18% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.97ms | 106.57ms | 3.951× | 5.04% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.70ms | 99.52ms | 4.385× | 5.73% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.31ms | 115.22ms | 3.565× | 5.25% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.78ms | 42.34ms | 1.217× | 1.50% | PASS |
| mem_reads | `oltp_range_select` | 19.75ms | 21.53ms | 1.090× | 1.66% | PASS |
| mem_reads | `oltp_sum_range` | 18.63ms | 21.43ms | 1.150× | 1.32% | PASS |
| mem_reads | `oltp_order_range` | 3.62ms | 3.87ms | 1.071× | 1.18% | PASS |
| mem_reads | `oltp_distinct_range` | 4.72ms | 4.97ms | 1.051× | 0.98% | PASS |
| mem_reads | `oltp_index_scan` | 4.73ms | 6.60ms | 1.396× | 2.26% | PASS |
| mem_reads | `select_random_points` | 29.22ms | 33.33ms | 1.140× | 1.77% | PASS |
| mem_reads | `select_random_ranges` | 7.93ms | 9.24ms | 1.165× | 1.15% | PASS |
| mem_reads | `covering_index_scan` | 4.29ms | 4.63ms | 1.080× | 2.30% | PASS |
| mem_reads | `groupby_scan` | 36.99ms | 39.25ms | 1.061× | 1.02% | PASS |
| mem_reads | `index_join` | 8.24ms | 10.94ms | 1.327× | 2.52% | PASS |
| mem_reads | `index_join_scan` | 4.26ms | 5.56ms | 1.303× | 2.68% | PASS |
| mem_reads | `types_table_scan` | 1.13s | 1.27s | 1.118× | 1.30% | PASS |
| mem_reads | `table_scan` | 1.38s | 1.43s | 1.041× | 1.36% | PASS |
| mem_reads | `oltp_read_only` | 156.84ms | 173.52ms | 1.106× | 1.61% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.13ms | 358.77ms | 1.440× | 1.25% | PASS |
| mem_writes | `oltp_insert` | 19.36ms | 36.97ms | 1.909× | 0.74% | PASS |
| mem_writes | `oltp_update_index` | 70.00ms | 120.75ms | 1.725× | 1.35% | PASS |
| mem_writes | `oltp_update_non_index` | 52.01ms | 85.01ms | 1.634× | 1.68% | PASS |
| mem_writes | `oltp_delete_insert` | 50.81ms | 98.84ms | 1.945× | 1.51% | PASS |
| mem_writes | `oltp_write_only` | 27.82ms | 59.36ms | 2.133× | 1.33% | PASS |
| mem_writes | `types_delete_insert` | 33.43ms | 56.56ms | 1.692× | 1.43% | PASS |
| mem_writes | `oltp_read_write` | 110.29ms | 162.96ms | 1.478× | 1.00% | PASS |
| file_reads | `oltp_point_select` | 111.28ms | 68.15ms | 0.612× | 0.78% | PASS |
| file_reads | `oltp_range_select` | 28.00ms | 24.61ms | 0.879× | 1.36% | PASS |
| file_reads | `oltp_sum_range` | 26.95ms | 24.21ms | 0.899× | 1.32% | PASS |
| file_reads | `oltp_order_range` | 4.52ms | 4.27ms | 0.944× | 1.44% | PASS |
| file_reads | `oltp_distinct_range` | 5.71ms | 5.39ms | 0.944× | 1.15% | PASS |
| file_reads | `oltp_index_scan` | 12.70ms | 9.24ms | 0.727× | 1.44% | PASS |
| file_reads | `select_random_points` | 39.41ms | 37.27ms | 0.945× | 1.52% | PASS |
| file_reads | `select_random_ranges` | 16.17ms | 12.07ms | 0.746× | 0.89% | PASS |
| file_reads | `covering_index_scan` | 12.25ms | 7.32ms | 0.598× | 1.78% | PASS |
| file_reads | `groupby_scan` | 37.83ms | 40.01ms | 1.058× | 0.81% | PASS |
| file_reads | `index_join` | 12.97ms | 12.97ms | 1.000× | 1.62% | PASS |
| file_reads | `index_join_scan` | 5.32ms | 6.08ms | 1.143× | 1.53% | PASS |
| file_reads | `types_table_scan` | 1.07s | 1.25s | 1.163× | 1.86% | PASS |
| file_reads | `table_scan` | 1.33s | 1.41s | 1.065× | 2.85% | PASS |
| file_reads | `oltp_read_only` | 275.35ms | 214.81ms | 0.780× | 1.11% | PASS |
| file_writes | `oltp_bulk_insert` | 264.30ms | 384.57ms | 1.455× | 1.00% | PASS |
| file_writes | `oltp_insert` | 26.74ms | 47.19ms | 1.765× | 1.68% | PASS |
| file_writes | `oltp_update_index` | 102.71ms | 148.01ms | 1.441× | 2.25% | PASS |
| file_writes | `oltp_update_non_index` | 79.26ms | 106.78ms | 1.347× | 1.70% | PASS |
| file_writes | `oltp_delete_insert` | 79.25ms | 122.64ms | 1.548× | 1.68% | PASS |
| file_writes | `oltp_write_only` | 52.57ms | 79.00ms | 1.503× | 1.48% | PASS |
| file_writes | `types_delete_insert` | 53.22ms | 72.15ms | 1.356× | 2.07% | PASS |
| file_writes | `oltp_read_write` | 142.28ms | 185.73ms | 1.305× | 1.36% | PASS |
| ac_reads | `oltp_point_select` | 59.70ms | 67.55ms | 1.132× | 1.14% | PASS |
| ac_reads | `oltp_range_select` | 23.08ms | 24.63ms | 1.067× | 1.79% | PASS |
| ac_reads | `oltp_sum_range` | 22.36ms | 24.67ms | 1.103× | 1.38% | PASS |
| ac_reads | `oltp_order_range` | 4.17ms | 4.36ms | 1.045× | 1.91% | PASS |
| ac_reads | `oltp_distinct_range` | 5.25ms | 5.45ms | 1.038× | 1.52% | PASS |
| ac_reads | `oltp_index_scan` | 7.57ms | 9.26ms | 1.224× | 1.85% | PASS |
| ac_reads | `select_random_points` | 32.52ms | 36.87ms | 1.134× | 1.46% | PASS |
| ac_reads | `select_random_ranges` | 10.67ms | 12.08ms | 1.132× | 1.25% | PASS |
| ac_reads | `covering_index_scan` | 7.24ms | 7.25ms | 1.002× | 2.08% | PASS |
| ac_reads | `groupby_scan` | 37.11ms | 39.84ms | 1.074× | 0.96% | PASS |
| ac_reads | `index_join` | 10.26ms | 13.14ms | 1.280× | 1.67% | PASS |
| ac_reads | `index_join_scan` | 4.83ms | 6.14ms | 1.272× | 2.40% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.24s | 1.173× | 1.30% | PASS |
| ac_reads | `table_scan` | 1.33s | 1.41s | 1.065× | 3.09% | PASS |
| ac_reads | `oltp_read_only` | 193.30ms | 211.77ms | 1.096× | 1.50% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.43ms | 78.52ms | 3.664× | 3.27% | PASS |
| ac_writes | `oltp_insert_ac` | 25.04ms | 98.56ms | 3.936× | 6.09% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.68ms | 111.25ms | 4.019× | 5.63% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.86ms | 90.78ms | 3.971× | 4.61% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.19ms | 101.10ms | 4.179× | 5.23% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.48ms | 101.17ms | 3.970× | 5.06% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.12ms | 89.71ms | 4.247× | 5.25% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.88ms | 109.06ms | 3.421× | 4.18% | PASS |

</details>

## Version-control latency

Wall time: 2m 6s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 67.06ms | 200.00ms | 33.5% | 0.52% | PASS |
| `status_dirty_many_tables` | 69.27ms | 200.00ms | 34.6% | 0.51% | PASS |
| `diff_regular_working_one_table` | 62.80ms | 150.00ms | 41.9% | 0.40% | PASS |
| `diff_regular_working_many_tables` | 73.86ms | 200.00ms | 36.9% | 0.64% | PASS |
| `diff_stat_working_many_tables` | 73.89ms | 200.00ms | 36.9% | 0.85% | PASS |
| `diff_schema_working_many_tables` | 73.52ms | 200.00ms | 36.8% | 0.43% | PASS |
| `branch_list_many_branches` | 19.67ms | 100.00ms | 19.7% | 1.39% | PASS |
| `branch_create_delete` | 26.87ms | 100.00ms | 26.9% | 2.64% | PASS |
| `checkout_branch_clean` | 102.71ms | 200.00ms | 51.4% | 13.11% | PASS |
| `merge_data_no_conflicts` | 33.14ms | 150.00ms | 22.1% | 3.55% | PASS |
| `merge_schema_no_conflicts` | 19.78ms | 100.00ms | 19.8% | 1.51% | PASS |
| `merge_data_conflicts` | 100.81ms | 250.00ms | 40.3% | 0.18% | PASS |
| `merge_data_conflicts_with_resolve` | 100.65ms | 250.00ms | 40.3% | 0.25% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
