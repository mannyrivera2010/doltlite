# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-13 12:15 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260720.247.2
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/31691932678)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 30s | 9.39s | 11.08s | 1.180× | 1.42% | **PASS** |
| textpk | 69 | 55 | 1h 34m 11s | 11.66s | 12.02s | 1.030× | 1.56% | **PASS** |
| blobpk | 69 | 55 | 1h 32m 7s | 10.10s | 12.01s | 1.189× | 2.16% | **PASS** |
| compositepk | 69 | 55 | 1h 25m 57s | 10.14s | 11.74s | 1.158× | 1.09% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.82ms | 27.63ms | 1.113× | 1.32% | PASS |
| mem_reads | `oltp_range_select` | 10.72ms | 11.97ms | 1.116× | 1.57% | PASS |
| mem_reads | `oltp_sum_range` | 9.85ms | 11.44ms | 1.161× | 1.26% | PASS |
| mem_reads | `oltp_order_range` | 2.72ms | 2.89ms | 1.065× | 1.15% | PASS |
| mem_reads | `oltp_distinct_range` | 3.75ms | 3.88ms | 1.034× | 1.09% | PASS |
| mem_reads | `oltp_index_scan` | 4.00ms | 4.91ms | 1.226× | 1.80% | PASS |
| mem_reads | `select_random_points` | 10.65ms | 11.31ms | 1.062× | 0.96% | PASS |
| mem_reads | `select_random_ranges` | 3.18ms | 4.03ms | 1.266× | 2.21% | PASS |
| mem_reads | `covering_index_scan` | 4.38ms | 4.36ms | 0.996× | 2.31% | PASS |
| mem_reads | `groupby_scan` | 32.36ms | 34.60ms | 1.069× | 1.00% | PASS |
| mem_reads | `index_join` | 6.11ms | 8.39ms | 1.375× | 1.47% | PASS |
| mem_reads | `index_join_scan` | 3.63ms | 4.89ms | 1.346× | 1.79% | PASS |
| mem_reads | `types_table_scan` | 1.13s | 1.27s | 1.122× | 1.14% | PASS |
| mem_reads | `table_scan` | 1.26s | 1.37s | 1.089× | 0.58% | PASS |
| mem_reads | `oltp_read_only` | 104.14ms | 115.15ms | 1.106× | 0.87% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.77ms | 242.59ms | 1.335× | 1.27% | PASS |
| mem_writes | `oltp_insert` | 15.89ms | 28.27ms | 1.779× | 0.92% | PASS |
| mem_writes | `oltp_update_index` | 51.73ms | 106.56ms | 2.060× | 1.10% | PASS |
| mem_writes | `oltp_update_non_index` | 36.01ms | 59.38ms | 1.649× | 1.28% | PASS |
| mem_writes | `oltp_delete_insert` | 45.55ms | 79.10ms | 1.736× | 1.10% | PASS |
| mem_writes | `oltp_write_only` | 22.43ms | 45.41ms | 2.025× | 1.34% | PASS |
| mem_writes | `types_delete_insert` | 25.08ms | 39.89ms | 1.591× | 1.03% | PASS |
| mem_writes | `oltp_read_write` | 66.08ms | 104.83ms | 1.586× | 1.49% | PASS |
| file_reads | `oltp_point_select` | 119.59ms | 59.20ms | 0.495× | 0.58% | PASS |
| file_reads | `oltp_range_select` | 19.89ms | 15.24ms | 0.766× | 1.81% | PASS |
| file_reads | `oltp_sum_range` | 19.26ms | 14.76ms | 0.766× | 1.75% | PASS |
| file_reads | `oltp_order_range` | 3.59ms | 3.26ms | 0.909× | 1.77% | PASS |
| file_reads | `oltp_distinct_range` | 4.63ms | 4.26ms | 0.919× | 1.37% | PASS |
| file_reads | `oltp_index_scan` | 13.75ms | 8.50ms | 0.618× | 2.11% | PASS |
| file_reads | `select_random_points` | 20.31ms | 14.90ms | 0.734× | 2.46% | PASS |
| file_reads | `select_random_ranges` | 12.70ms | 7.22ms | 0.568× | 1.00% | PASS |
| file_reads | `covering_index_scan` | 14.04ms | 7.62ms | 0.543× | 1.46% | PASS |
| file_reads | `groupby_scan` | 33.09ms | 35.00ms | 1.058× | 0.78% | PASS |
| file_reads | `index_join` | 11.11ms | 10.28ms | 0.926× | 1.62% | PASS |
| file_reads | `index_join_scan` | 4.71ms | 5.32ms | 1.129× | 2.05% | PASS |
| file_reads | `types_table_scan` | 1.11s | 1.26s | 1.139× | 0.44% | PASS |
| file_reads | `table_scan` | 1.26s | 1.38s | 1.087× | 0.86% | PASS |
| file_reads | `oltp_read_only` | 240.52ms | 160.58ms | 0.668× | 0.63% | PASS |
| file_writes | `oltp_bulk_insert` | 196.56ms | 262.23ms | 1.334× | 1.05% | PASS |
| file_writes | `oltp_insert` | 22.28ms | 36.17ms | 1.624× | 1.37% | PASS |
| file_writes | `oltp_update_index` | 81.26ms | 133.17ms | 1.639× | 1.56% | PASS |
| file_writes | `oltp_update_non_index` | 61.31ms | 83.78ms | 1.366× | 1.76% | PASS |
| file_writes | `oltp_delete_insert` | 68.57ms | 99.80ms | 1.455× | 1.43% | PASS |
| file_writes | `oltp_write_only` | 45.43ms | 66.85ms | 1.471× | 1.84% | PASS |
| file_writes | `types_delete_insert` | 41.64ms | 54.18ms | 1.301× | 1.84% | PASS |
| file_writes | `oltp_read_write` | 92.37ms | 127.82ms | 1.384× | 1.85% | PASS |
| ac_reads | `oltp_point_select` | 55.93ms | 59.21ms | 1.059× | 0.74% | PASS |
| ac_reads | `oltp_range_select` | 14.56ms | 15.29ms | 1.050× | 1.70% | PASS |
| ac_reads | `oltp_sum_range` | 13.25ms | 14.82ms | 1.119× | 1.27% | PASS |
| ac_reads | `oltp_order_range` | 3.17ms | 3.27ms | 1.033× | 1.49% | PASS |
| ac_reads | `oltp_distinct_range` | 4.17ms | 4.26ms | 1.024× | 1.24% | PASS |
| ac_reads | `oltp_index_scan` | 7.39ms | 8.49ms | 1.148× | 1.42% | PASS |
| ac_reads | `select_random_points` | 14.30ms | 14.88ms | 1.041× | 1.44% | PASS |
| ac_reads | `select_random_ranges` | 6.37ms | 7.26ms | 1.140× | 0.98% | PASS |
| ac_reads | `covering_index_scan` | 7.78ms | 7.60ms | 0.978× | 1.16% | PASS |
| ac_reads | `groupby_scan` | 32.52ms | 34.94ms | 1.074× | 0.73% | PASS |
| ac_reads | `index_join` | 8.11ms | 10.36ms | 1.278× | 1.60% | PASS |
| ac_reads | `index_join_scan` | 4.20ms | 5.43ms | 1.293× | 2.47% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.26s | 1.139× | 0.68% | PASS |
| ac_reads | `table_scan` | 1.26s | 1.37s | 1.090× | 0.54% | PASS |
| ac_reads | `oltp_read_only` | 149.96ms | 160.72ms | 1.072× | 0.72% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.74ms | 66.34ms | 3.962× | 5.53% | PASS |
| ac_writes | `oltp_insert_ac` | 18.81ms | 82.62ms | 4.392× | 4.56% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.14ms | 97.47ms | 4.839× | 5.05% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.47ms | 74.53ms | 4.524× | 4.82% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.16ms | 87.13ms | 4.799× | 4.02% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.40ms | 86.28ms | 4.688× | 4.97% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.01ms | 75.95ms | 4.743× | 4.56% | PASS |
| ac_writes | `oltp_read_write_ac` | 23.95ms | 93.58ms | 3.908× | 2.70% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.71ms | 35.16ms | 1.145× | 1.12% | PASS |
| mem_reads | `oltp_range_select` | 13.75ms | 13.71ms | 0.997× | 1.67% | PASS |
| mem_reads | `oltp_sum_range` | 12.54ms | 13.22ms | 1.054× | 1.79% | PASS |
| mem_reads | `oltp_order_range` | 3.12ms | 3.15ms | 1.010× | 1.38% | PASS |
| mem_reads | `oltp_distinct_range` | 4.21ms | 4.19ms | 0.995× | 0.82% | PASS |
| mem_reads | `oltp_index_scan` | 4.61ms | 5.84ms | 1.269× | 1.73% | PASS |
| mem_reads | `select_random_points` | 17.76ms | 19.95ms | 1.123× | 1.05% | PASS |
| mem_reads | `select_random_ranges` | 4.09ms | 5.23ms | 1.279× | 1.56% | PASS |
| mem_reads | `covering_index_scan` | 4.87ms | 4.49ms | 0.922× | 2.02% | PASS |
| mem_reads | `groupby_scan` | 34.01ms | 35.55ms | 1.045× | 0.54% | PASS |
| mem_reads | `index_join` | 7.14ms | 9.20ms | 1.287× | 1.70% | PASS |
| mem_reads | `index_join_scan` | 4.88ms | 5.56ms | 1.138× | 2.55% | PASS |
| mem_reads | `types_table_scan` | 1.17s | 1.26s | 1.076× | 1.60% | PASS |
| mem_reads | `table_scan` | 1.68s | 1.42s | 0.847× | 2.04% | PASS |
| mem_reads | `oltp_read_only` | 133.58ms | 133.84ms | 1.002× | 1.15% | PASS |
| mem_writes | `oltp_bulk_insert` | 233.12ms | 336.43ms | 1.443× | 1.09% | PASS |
| mem_writes | `oltp_insert` | 23.45ms | 40.05ms | 1.707× | 1.47% | PASS |
| mem_writes | `oltp_update_index` | 80.05ms | 143.20ms | 1.789× | 2.16% | PASS |
| mem_writes | `oltp_update_non_index` | 52.06ms | 88.57ms | 1.701× | 1.75% | PASS |
| mem_writes | `oltp_delete_insert` | 54.11ms | 107.35ms | 1.984× | 1.35% | PASS |
| mem_writes | `oltp_write_only` | 31.11ms | 65.05ms | 2.091× | 1.15% | PASS |
| mem_writes | `types_delete_insert` | 33.90ms | 54.53ms | 1.609× | 1.69% | PASS |
| mem_writes | `oltp_read_write` | 85.42ms | 135.19ms | 1.583× | 1.04% | PASS |
| file_reads | `oltp_point_select` | 127.01ms | 67.42ms | 0.531× | 0.90% | PASS |
| file_reads | `oltp_range_select` | 24.69ms | 17.02ms | 0.689× | 2.75% | PASS |
| file_reads | `oltp_sum_range` | 22.73ms | 16.68ms | 0.734× | 1.67% | PASS |
| file_reads | `oltp_order_range` | 4.24ms | 3.54ms | 0.834× | 1.48% | PASS |
| file_reads | `oltp_distinct_range` | 5.32ms | 4.54ms | 0.854× | 1.06% | PASS |
| file_reads | `oltp_index_scan` | 14.60ms | 9.42ms | 0.645× | 1.18% | PASS |
| file_reads | `select_random_points` | 28.47ms | 23.45ms | 0.824× | 1.54% | PASS |
| file_reads | `select_random_ranges` | 13.99ms | 8.55ms | 0.611× | 0.68% | PASS |
| file_reads | `covering_index_scan` | 15.89ms | 8.06ms | 0.507× | 1.05% | PASS |
| file_reads | `groupby_scan` | 35.50ms | 36.17ms | 1.019× | 0.77% | PASS |
| file_reads | `index_join` | 13.24ms | 11.54ms | 0.872× | 1.69% | PASS |
| file_reads | `index_join_scan` | 6.03ms | 6.23ms | 1.033× | 1.90% | PASS |
| file_reads | `types_table_scan` | 1.26s | 1.29s | 1.019× | 3.86% | PASS |
| file_reads | `table_scan` | 1.64s | 1.41s | 0.863× | 1.78% | PASS |
| file_reads | `oltp_read_only` | 272.74ms | 180.96ms | 0.663× | 1.25% | PASS |
| file_writes | `oltp_bulk_insert` | 255.16ms | 369.46ms | 1.448× | 1.06% | PASS |
| file_writes | `oltp_insert` | 66.43ms | 53.73ms | 0.809× | 13.70% | PASS |
| file_writes | `oltp_update_index` | 124.63ms | 181.42ms | 1.456× | 1.16% | PASS |
| file_writes | `oltp_update_non_index` | 116.93ms | 119.08ms | 1.018× | 11.65% | PASS |
| file_writes | `oltp_delete_insert` | 98.72ms | 141.93ms | 1.438× | 2.15% | PASS |
| file_writes | `oltp_write_only` | 85.25ms | 91.37ms | 1.072× | 9.50% | PASS |
| file_writes | `types_delete_insert` | 59.00ms | 77.21ms | 1.309× | 1.96% | PASS |
| file_writes | `oltp_read_write` | 137.00ms | 162.67ms | 1.187× | 5.01% | PASS |
| ac_reads | `oltp_point_select` | 63.16ms | 67.47ms | 1.068× | 1.43% | PASS |
| ac_reads | `oltp_range_select` | 18.52ms | 17.06ms | 0.921× | 1.55% | PASS |
| ac_reads | `oltp_sum_range` | 16.89ms | 17.00ms | 1.006× | 1.06% | PASS |
| ac_reads | `oltp_order_range` | 3.74ms | 3.55ms | 0.947× | 0.98% | PASS |
| ac_reads | `oltp_distinct_range` | 4.73ms | 4.55ms | 0.963× | 0.81% | PASS |
| ac_reads | `oltp_index_scan` | 8.49ms | 9.53ms | 1.122× | 1.03% | PASS |
| ac_reads | `select_random_points` | 22.96ms | 23.87ms | 1.040× | 2.23% | PASS |
| ac_reads | `select_random_ranges` | 7.56ms | 8.50ms | 1.125× | 0.86% | PASS |
| ac_reads | `covering_index_scan` | 9.53ms | 8.04ms | 0.843× | 1.16% | PASS |
| ac_reads | `groupby_scan` | 34.73ms | 36.01ms | 1.037× | 0.62% | PASS |
| ac_reads | `index_join` | 10.08ms | 11.51ms | 1.141× | 1.37% | PASS |
| ac_reads | `index_join_scan` | 5.50ms | 6.16ms | 1.121× | 2.08% | PASS |
| ac_reads | `types_table_scan` | 1.31s | 1.29s | 0.990× | 1.67% | PASS |
| ac_reads | `table_scan` | 1.67s | 1.42s | 0.854× | 1.51% | PASS |
| ac_reads | `oltp_read_only` | 177.17ms | 178.98ms | 1.010× | 1.51% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.12ms | 63.29ms | 3.926× | 3.61% | PASS |
| ac_writes | `oltp_insert_ac` | 19.55ms | 80.70ms | 4.128× | 4.76% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.39ms | 99.20ms | 4.639× | 2.55% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.22ms | 76.57ms | 4.722× | 6.06% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.71ms | 87.37ms | 4.669× | 4.07% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.41ms | 88.47ms | 4.557× | 5.19% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.84ms | 77.22ms | 4.876× | 5.73% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.30ms | 94.65ms | 3.741× | 3.32% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.28ms | 37.75ms | 1.207× | 2.14% | PASS |
| mem_reads | `oltp_range_select` | 14.05ms | 14.19ms | 1.010× | 4.67% | PASS |
| mem_reads | `oltp_sum_range` | 12.63ms | 14.22ms | 1.126× | 2.94% | PASS |
| mem_reads | `oltp_order_range` | 3.06ms | 3.20ms | 1.045× | 1.73% | PASS |
| mem_reads | `oltp_distinct_range` | 4.06ms | 4.26ms | 1.050× | 1.05% | PASS |
| mem_reads | `oltp_index_scan` | 4.80ms | 6.60ms | 1.373× | 2.23% | PASS |
| mem_reads | `select_random_points` | 18.80ms | 21.21ms | 1.128× | 2.40% | PASS |
| mem_reads | `select_random_ranges` | 4.30ms | 5.27ms | 1.224× | 1.70% | PASS |
| mem_reads | `covering_index_scan` | 4.61ms | 4.72ms | 1.024× | 2.79% | PASS |
| mem_reads | `groupby_scan` | 32.39ms | 34.30ms | 1.059× | 1.11% | PASS |
| mem_reads | `index_join` | 7.16ms | 9.76ms | 1.362× | 2.77% | PASS |
| mem_reads | `index_join_scan` | 4.48ms | 5.51ms | 1.231× | 4.42% | PASS |
| mem_reads | `types_table_scan` | 1.09s | 1.25s | 1.147× | 2.46% | PASS |
| mem_reads | `table_scan` | 1.30s | 1.39s | 1.072× | 4.73% | PASS |
| mem_reads | `oltp_read_only` | 121.54ms | 136.11ms | 1.120× | 2.23% | PASS |
| mem_writes | `oltp_bulk_insert` | 237.59ms | 353.51ms | 1.488× | 0.93% | PASS |
| mem_writes | `oltp_insert` | 19.87ms | 40.11ms | 2.019× | 0.95% | PASS |
| mem_writes | `oltp_update_index` | 68.65ms | 133.55ms | 1.945× | 1.69% | PASS |
| mem_writes | `oltp_update_non_index` | 49.14ms | 86.67ms | 1.764× | 1.52% | PASS |
| mem_writes | `oltp_delete_insert` | 49.34ms | 105.35ms | 2.135× | 1.68% | PASS |
| mem_writes | `oltp_write_only` | 28.77ms | 63.80ms | 2.217× | 1.24% | PASS |
| mem_writes | `types_delete_insert` | 32.52ms | 54.10ms | 1.664× | 2.14% | PASS |
| mem_writes | `oltp_read_write` | 84.15ms | 140.59ms | 1.671× | 2.46% | PASS |
| file_reads | `oltp_point_select` | 105.69ms | 63.08ms | 0.597× | 0.95% | PASS |
| file_reads | `oltp_range_select` | 22.79ms | 17.23ms | 0.756× | 1.63% | PASS |
| file_reads | `oltp_sum_range` | 21.00ms | 17.08ms | 0.813× | 2.01% | PASS |
| file_reads | `oltp_order_range` | 4.26ms | 3.80ms | 0.892× | 2.29% | PASS |
| file_reads | `oltp_distinct_range` | 5.39ms | 4.96ms | 0.920× | 2.84% | PASS |
| file_reads | `oltp_index_scan` | 12.60ms | 9.14ms | 0.726× | 1.23% | PASS |
| file_reads | `select_random_points` | 27.63ms | 24.43ms | 0.884× | 2.33% | PASS |
| file_reads | `select_random_ranges` | 11.86ms | 7.83ms | 0.660× | 0.87% | PASS |
| file_reads | `covering_index_scan` | 13.20ms | 7.31ms | 0.554× | 1.60% | PASS |
| file_reads | `groupby_scan` | 34.10ms | 35.28ms | 1.034× | 1.19% | PASS |
| file_reads | `index_join` | 12.03ms | 11.31ms | 0.940× | 2.25% | PASS |
| file_reads | `index_join_scan` | 5.61ms | 5.99ms | 1.067× | 3.37% | PASS |
| file_reads | `types_table_scan` | 1.19s | 1.27s | 1.073× | 1.54% | PASS |
| file_reads | `table_scan` | 1.45s | 1.43s | 0.983× | 1.69% | PASS |
| file_reads | `oltp_read_only` | 239.57ms | 177.99ms | 0.743× | 1.35% | PASS |
| file_writes | `oltp_bulk_insert` | 256.23ms | 377.55ms | 1.473× | 1.19% | PASS |
| file_writes | `oltp_insert` | 37.27ms | 52.49ms | 1.408× | 2.05% | PASS |
| file_writes | `oltp_update_index` | 106.23ms | 164.38ms | 1.547× | 1.90% | PASS |
| file_writes | `oltp_update_non_index` | 85.47ms | 108.67ms | 1.271× | 1.72% | PASS |
| file_writes | `oltp_delete_insert` | 88.70ms | 133.00ms | 1.499× | 1.89% | PASS |
| file_writes | `oltp_write_only` | 63.70ms | 84.45ms | 1.326× | 1.51% | PASS |
| file_writes | `types_delete_insert` | 55.07ms | 72.85ms | 1.323× | 1.49% | PASS |
| file_writes | `oltp_read_write` | 133.06ms | 168.36ms | 1.265× | 1.69% | PASS |
| ac_reads | `oltp_point_select` | 56.46ms | 63.77ms | 1.129× | 1.20% | PASS |
| ac_reads | `oltp_range_select` | 17.57ms | 17.59ms | 1.002× | 4.05% | PASS |
| ac_reads | `oltp_sum_range` | 16.17ms | 17.20ms | 1.064× | 3.02% | PASS |
| ac_reads | `oltp_order_range` | 3.83ms | 3.92ms | 1.025× | 2.45% | PASS |
| ac_reads | `oltp_distinct_range` | 4.90ms | 4.95ms | 1.010× | 3.21% | PASS |
| ac_reads | `oltp_index_scan` | 7.60ms | 9.25ms | 1.217× | 2.00% | PASS |
| ac_reads | `select_random_points` | 23.14ms | 25.49ms | 1.101× | 4.62% | PASS |
| ac_reads | `select_random_ranges` | 7.05ms | 7.87ms | 1.115× | 1.48% | PASS |
| ac_reads | `covering_index_scan` | 8.38ms | 7.39ms | 0.882× | 3.23% | PASS |
| ac_reads | `groupby_scan` | 33.30ms | 35.23ms | 1.058× | 1.05% | PASS |
| ac_reads | `index_join` | 9.78ms | 11.55ms | 1.181× | 4.34% | PASS |
| ac_reads | `index_join_scan` | 5.10ms | 6.02ms | 1.181× | 3.51% | PASS |
| ac_reads | `types_table_scan` | 1.09s | 1.25s | 1.145× | 2.31% | PASS |
| ac_reads | `table_scan` | 1.25s | 1.38s | 1.105× | 3.10% | PASS |
| ac_reads | `oltp_read_only` | 164.94ms | 179.76ms | 1.090× | 2.16% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.61ms | 80.22ms | 3.713× | 4.57% | PASS |
| ac_writes | `oltp_insert_ac` | 24.63ms | 104.39ms | 4.238× | 5.36% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.58ms | 116.31ms | 4.375× | 4.91% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.26ms | 93.73ms | 4.210× | 4.43% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.35ms | 105.62ms | 4.524× | 4.98% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.93ms | 108.38ms | 4.346× | 7.50% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.71ms | 99.71ms | 4.593× | 6.39% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.71ms | 112.11ms | 3.650× | 4.25% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.03ms | 36.90ms | 1.117× | 1.61% | PASS |
| mem_reads | `oltp_range_select` | 19.26ms | 19.78ms | 1.027× | 0.90% | PASS |
| mem_reads | `oltp_sum_range` | 18.12ms | 19.05ms | 1.051× | 1.05% | PASS |
| mem_reads | `oltp_order_range` | 3.70ms | 3.73ms | 1.010× | 1.11% | PASS |
| mem_reads | `oltp_distinct_range` | 4.79ms | 4.80ms | 1.002× | 0.90% | PASS |
| mem_reads | `oltp_index_scan` | 4.66ms | 5.80ms | 1.244× | 2.10% | PASS |
| mem_reads | `select_random_points` | 29.30ms | 31.26ms | 1.067× | 4.29% | PASS |
| mem_reads | `select_random_ranges` | 7.69ms | 8.29ms | 1.078× | 1.50% | PASS |
| mem_reads | `covering_index_scan` | 4.42ms | 4.37ms | 0.989× | 2.34% | PASS |
| mem_reads | `groupby_scan` | 38.67ms | 40.11ms | 1.037× | 0.78% | PASS |
| mem_reads | `index_join` | 8.21ms | 10.29ms | 1.253× | 1.88% | PASS |
| mem_reads | `index_join_scan` | 4.44ms | 5.59ms | 1.261× | 2.21% | PASS |
| mem_reads | `types_table_scan` | 1.13s | 1.25s | 1.111× | 0.76% | PASS |
| mem_reads | `table_scan` | 1.30s | 1.36s | 1.047× | 1.37% | PASS |
| mem_reads | `oltp_read_only` | 154.45ms | 159.24ms | 1.031× | 1.93% | PASS |
| mem_writes | `oltp_bulk_insert` | 245.83ms | 338.51ms | 1.377× | 1.14% | PASS |
| mem_writes | `oltp_insert` | 19.43ms | 36.00ms | 1.853× | 0.43% | PASS |
| mem_writes | `oltp_update_index` | 67.43ms | 117.41ms | 1.741× | 1.07% | PASS |
| mem_writes | `oltp_update_non_index` | 52.52ms | 85.27ms | 1.624× | 2.04% | PASS |
| mem_writes | `oltp_delete_insert` | 49.83ms | 96.67ms | 1.940× | 0.94% | PASS |
| mem_writes | `oltp_write_only` | 27.12ms | 59.39ms | 2.190× | 0.89% | PASS |
| mem_writes | `types_delete_insert` | 32.59ms | 53.66ms | 1.647× | 1.63% | PASS |
| mem_writes | `oltp_read_write` | 104.25ms | 149.65ms | 1.435× | 2.71% | PASS |
| file_reads | `oltp_point_select` | 130.46ms | 70.42ms | 0.540× | 1.43% | PASS |
| file_reads | `oltp_range_select` | 30.23ms | 23.42ms | 0.775× | 2.70% | PASS |
| file_reads | `oltp_sum_range` | 28.14ms | 22.55ms | 0.801× | 1.52% | PASS |
| file_reads | `oltp_order_range` | 4.74ms | 4.12ms | 0.870× | 0.82% | PASS |
| file_reads | `oltp_distinct_range` | 5.83ms | 5.21ms | 0.893× | 1.04% | PASS |
| file_reads | `oltp_index_scan` | 14.55ms | 9.25ms | 0.636× | 1.50% | PASS |
| file_reads | `select_random_points` | 37.63ms | 33.87ms | 0.900× | 1.27% | PASS |
| file_reads | `select_random_ranges` | 17.29ms | 11.59ms | 0.670× | 1.61% | PASS |
| file_reads | `covering_index_scan` | 14.36ms | 7.55ms | 0.526× | 0.95% | PASS |
| file_reads | `groupby_scan` | 39.65ms | 40.68ms | 1.026× | 0.67% | PASS |
| file_reads | `index_join` | 13.42ms | 12.27ms | 0.914× | 1.23% | PASS |
| file_reads | `index_join_scan` | 5.38ms | 6.06ms | 1.126× | 1.08% | PASS |
| file_reads | `types_table_scan` | 1.12s | 1.24s | 1.109× | 0.65% | PASS |
| file_reads | `table_scan` | 1.29s | 1.35s | 1.044× | 0.39% | PASS |
| file_reads | `oltp_read_only` | 287.70ms | 204.91ms | 0.712× | 0.63% | PASS |
| file_writes | `oltp_bulk_insert` | 261.36ms | 359.58ms | 1.376× | 0.94% | PASS |
| file_writes | `oltp_insert` | 26.29ms | 46.65ms | 1.775× | 1.20% | PASS |
| file_writes | `oltp_update_index` | 99.40ms | 145.70ms | 1.466× | 1.12% | PASS |
| file_writes | `oltp_update_non_index` | 78.32ms | 105.86ms | 1.352× | 1.39% | PASS |
| file_writes | `oltp_delete_insert` | 77.53ms | 121.33ms | 1.565× | 1.27% | PASS |
| file_writes | `oltp_write_only` | 50.75ms | 79.76ms | 1.572× | 1.33% | PASS |
| file_writes | `types_delete_insert` | 49.73ms | 67.23ms | 1.352× | 1.07% | PASS |
| file_writes | `oltp_read_write` | 122.25ms | 168.66ms | 1.380× | 0.83% | PASS |
| ac_reads | `oltp_point_select` | 64.09ms | 68.79ms | 1.073× | 1.09% | PASS |
| ac_reads | `oltp_range_select` | 22.62ms | 23.17ms | 1.024× | 0.79% | PASS |
| ac_reads | `oltp_sum_range` | 21.22ms | 22.51ms | 1.061× | 0.90% | PASS |
| ac_reads | `oltp_order_range` | 4.07ms | 4.13ms | 1.015× | 1.07% | PASS |
| ac_reads | `oltp_distinct_range` | 5.13ms | 5.19ms | 1.011× | 0.80% | PASS |
| ac_reads | `oltp_index_scan` | 7.96ms | 9.12ms | 1.145× | 1.07% | PASS |
| ac_reads | `select_random_points` | 30.63ms | 33.70ms | 1.100× | 0.84% | PASS |
| ac_reads | `select_random_ranges` | 10.73ms | 11.59ms | 1.081× | 0.91% | PASS |
| ac_reads | `covering_index_scan` | 7.70ms | 7.52ms | 0.977× | 1.00% | PASS |
| ac_reads | `groupby_scan` | 38.59ms | 40.74ms | 1.056× | 0.61% | PASS |
| ac_reads | `index_join` | 10.03ms | 12.35ms | 1.231× | 0.85% | PASS |
| ac_reads | `index_join_scan` | 4.70ms | 6.12ms | 1.302× | 1.02% | PASS |
| ac_reads | `types_table_scan` | 1.12s | 1.24s | 1.113× | 0.39% | PASS |
| ac_reads | `table_scan` | 1.29s | 1.35s | 1.047× | 0.40% | PASS |
| ac_reads | `oltp_read_only` | 195.40ms | 204.93ms | 1.049× | 0.63% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.10ms | 63.43ms | 3.939× | 4.13% | PASS |
| ac_writes | `oltp_insert_ac` | 18.22ms | 84.08ms | 4.616× | 2.53% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.83ms | 97.06ms | 4.894× | 2.77% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.83ms | 75.01ms | 4.206× | 3.63% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.82ms | 87.15ms | 4.891× | 3.49% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.36ms | 87.27ms | 4.753× | 3.10% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.89ms | 76.21ms | 4.797× | 5.29% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.19ms | 97.47ms | 3.869× | 2.72% | PASS |

</details>

## Version-control latency

Wall time: 2m 22s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.23ms | 200.00ms | 41.6% | 0.57% | PASS |
| `status_dirty_many_tables` | 87.04ms | 200.00ms | 43.5% | 0.65% | PASS |
| `diff_regular_working_one_table` | 79.76ms | 150.00ms | 53.2% | 0.65% | PASS |
| `diff_regular_working_many_tables` | 92.57ms | 200.00ms | 46.3% | 0.84% | PASS |
| `diff_stat_working_many_tables` | 92.14ms | 200.00ms | 46.1% | 0.63% | PASS |
| `diff_schema_working_many_tables` | 93.24ms | 200.00ms | 46.6% | 0.62% | PASS |
| `branch_list_many_branches` | 23.43ms | 100.00ms | 23.4% | 1.05% | PASS |
| `branch_create_delete` | 26.02ms | 100.00ms | 26.0% | 1.65% | PASS |
| `checkout_branch_clean` | 55.92ms | 200.00ms | 28.0% | 1.09% | PASS |
| `merge_data_no_conflicts` | 30.13ms | 150.00ms | 20.1% | 1.96% | PASS |
| `merge_schema_no_conflicts` | 23.59ms | 100.00ms | 23.6% | 1.92% | PASS |
| `merge_data_conflicts` | 128.08ms | 250.00ms | 51.2% | 0.47% | PASS |
| `merge_data_conflicts_with_resolve` | 127.89ms | 250.00ms | 51.2% | 0.51% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
