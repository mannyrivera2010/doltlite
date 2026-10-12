# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-12 20:52 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261004.327.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/38296368123)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 28s | 8.82s | 11.36s | 1.287× | 1.44% | **PASS** |
| textpk | 69 | 55 | 1h 33m 57s | 10.70s | 12.07s | 1.128× | 1.77% | **PASS** |
| blobpk | 69 | 55 | 1h 28m 34s | 9.26s | 10.90s | 1.177× | 1.80% | **PASS** |
| compositepk | 69 | 55 | 1h 3m 58s | 7.57s | 8.04s | 1.062× | 3.13% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.54ms | 28.98ms | 1.286× | 1.13% | PASS |
| mem_reads | `oltp_range_select` | 9.01ms | 13.04ms | 1.447× | 1.09% | PASS |
| mem_reads | `oltp_sum_range` | 8.75ms | 12.28ms | 1.404× | 0.70% | PASS |
| mem_reads | `oltp_order_range` | 2.49ms | 2.98ms | 1.196× | 1.82% | PASS |
| mem_reads | `oltp_distinct_range` | 3.46ms | 4.03ms | 1.163× | 1.17% | PASS |
| mem_reads | `oltp_index_scan` | 3.65ms | 5.14ms | 1.410× | 1.40% | PASS |
| mem_reads | `select_random_points` | 10.06ms | 11.18ms | 1.111× | 1.44% | PASS |
| mem_reads | `select_random_ranges` | 2.96ms | 3.98ms | 1.344× | 1.93% | PASS |
| mem_reads | `covering_index_scan` | 4.22ms | 4.18ms | 0.989× | 1.52% | PASS |
| mem_reads | `groupby_scan` | 29.77ms | 33.36ms | 1.120× | 0.57% | PASS |
| mem_reads | `index_join` | 6.02ms | 8.31ms | 1.381× | 2.04% | PASS |
| mem_reads | `index_join_scan` | 3.45ms | 4.67ms | 1.352× | 1.31% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.33s | 1.282× | 1.03% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.40s | 1.186× | 1.19% | PASS |
| mem_reads | `oltp_read_only` | 100.57ms | 123.72ms | 1.230× | 0.79% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.05ms | 253.55ms | 1.408× | 0.98% | PASS |
| mem_writes | `oltp_insert` | 15.42ms | 28.03ms | 1.817× | 0.72% | PASS |
| mem_writes | `oltp_update_index` | 49.48ms | 103.20ms | 2.086× | 1.17% | PASS |
| mem_writes | `oltp_update_non_index` | 33.38ms | 59.04ms | 1.769× | 1.14% | PASS |
| mem_writes | `oltp_delete_insert` | 45.07ms | 78.55ms | 1.743× | 1.26% | PASS |
| mem_writes | `oltp_write_only` | 21.59ms | 44.37ms | 2.055× | 1.27% | PASS |
| mem_writes | `types_delete_insert` | 24.43ms | 40.62ms | 1.663× | 0.96% | PASS |
| mem_writes | `oltp_read_write` | 65.98ms | 109.82ms | 1.664× | 1.46% | PASS |
| file_reads | `oltp_point_select` | 97.86ms | 55.37ms | 0.566× | 0.76% | PASS |
| file_reads | `oltp_range_select` | 17.99ms | 16.19ms | 0.900× | 1.67% | PASS |
| file_reads | `oltp_sum_range` | 17.30ms | 15.46ms | 0.893× | 1.48% | PASS |
| file_reads | `oltp_order_range` | 3.67ms | 3.67ms | 0.999× | 3.11% | PASS |
| file_reads | `oltp_distinct_range` | 4.66ms | 4.67ms | 1.001× | 2.90% | PASS |
| file_reads | `oltp_index_scan` | 11.71ms | 8.48ms | 0.724× | 1.22% | PASS |
| file_reads | `select_random_points` | 18.02ms | 14.12ms | 0.783× | 1.29% | PASS |
| file_reads | `select_random_ranges` | 10.48ms | 6.60ms | 0.630× | 0.91% | PASS |
| file_reads | `covering_index_scan` | 12.03ms | 7.13ms | 0.593× | 0.76% | PASS |
| file_reads | `groupby_scan` | 30.90ms | 33.96ms | 1.099× | 0.65% | PASS |
| file_reads | `index_join` | 10.30ms | 10.41ms | 1.011× | 1.83% | PASS |
| file_reads | `index_join_scan` | 4.48ms | 5.00ms | 1.117× | 1.88% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.33s | 1.278× | 0.50% | PASS |
| file_reads | `table_scan` | 1.18s | 1.40s | 1.186× | 0.88% | PASS |
| file_reads | `oltp_read_only` | 211.55ms | 161.54ms | 0.764× | 0.79% | PASS |
| file_writes | `oltp_bulk_insert` | 194.75ms | 271.81ms | 1.396× | 0.98% | PASS |
| file_writes | `oltp_insert` | 22.19ms | 35.63ms | 1.606× | 1.63% | PASS |
| file_writes | `oltp_update_index` | 76.96ms | 128.14ms | 1.665× | 1.08% | PASS |
| file_writes | `oltp_update_non_index` | 57.03ms | 80.75ms | 1.416× | 1.79% | PASS |
| file_writes | `oltp_delete_insert` | 67.40ms | 98.58ms | 1.463× | 2.40% | PASS |
| file_writes | `oltp_write_only` | 45.71ms | 65.62ms | 1.436× | 2.33% | PASS |
| file_writes | `types_delete_insert` | 43.60ms | 57.30ms | 1.314× | 2.15% | PASS |
| file_writes | `oltp_read_write` | 99.30ms | 134.78ms | 1.357× | 2.03% | PASS |
| ac_reads | `oltp_point_select` | 49.76ms | 55.66ms | 1.119× | 1.46% | PASS |
| ac_reads | `oltp_range_select` | 13.90ms | 16.32ms | 1.174× | 2.22% | PASS |
| ac_reads | `oltp_sum_range` | 12.67ms | 15.51ms | 1.224× | 2.36% | PASS |
| ac_reads | `oltp_order_range` | 3.33ms | 3.67ms | 1.104× | 3.24% | PASS |
| ac_reads | `oltp_distinct_range` | 4.24ms | 4.64ms | 1.095× | 2.37% | PASS |
| ac_reads | `oltp_index_scan` | 6.79ms | 8.36ms | 1.232× | 1.58% | PASS |
| ac_reads | `select_random_points` | 13.15ms | 14.04ms | 1.068× | 2.45% | PASS |
| ac_reads | `select_random_ranges` | 5.64ms | 6.66ms | 1.182× | 1.06% | PASS |
| ac_reads | `covering_index_scan` | 7.26ms | 7.17ms | 0.988× | 2.12% | PASS |
| ac_reads | `groupby_scan` | 30.83ms | 34.56ms | 1.121× | 1.29% | PASS |
| ac_reads | `index_join` | 7.77ms | 10.21ms | 1.314× | 1.71% | PASS |
| ac_reads | `index_join_scan` | 3.97ms | 5.03ms | 1.267× | 1.23% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.33s | 1.281× | 0.79% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.39s | 1.194× | 0.99% | PASS |
| ac_reads | `oltp_read_only` | 135.81ms | 160.56ms | 1.182× | 0.68% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.84ms | 64.65ms | 3.103× | 5.01% | PASS |
| ac_writes | `oltp_insert_ac` | 22.52ms | 80.02ms | 3.553× | 4.02% | PASS |
| ac_writes | `oltp_update_index_ac` | 24.93ms | 94.94ms | 3.808× | 4.15% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.56ms | 72.82ms | 3.377× | 6.96% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.04ms | 84.53ms | 3.669× | 3.95% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.62ms | 84.85ms | 3.591× | 3.58% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.64ms | 74.24ms | 3.598× | 5.09% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.66ms | 90.76ms | 3.167× | 4.05% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.52ms | 37.73ms | 1.236× | 1.83% | PASS |
| mem_reads | `oltp_range_select` | 13.72ms | 14.25ms | 1.039× | 2.48% | PASS |
| mem_reads | `oltp_sum_range` | 12.48ms | 14.34ms | 1.149× | 1.67% | PASS |
| mem_reads | `oltp_order_range` | 3.03ms | 3.17ms | 1.045× | 1.21% | PASS |
| mem_reads | `oltp_distinct_range` | 4.10ms | 4.25ms | 1.037× | 1.13% | PASS |
| mem_reads | `oltp_index_scan` | 4.64ms | 6.45ms | 1.388× | 1.59% | PASS |
| mem_reads | `select_random_points` | 18.39ms | 21.10ms | 1.148× | 1.83% | PASS |
| mem_reads | `select_random_ranges` | 4.04ms | 5.18ms | 1.283× | 1.24% | PASS |
| mem_reads | `covering_index_scan` | 4.88ms | 4.72ms | 0.967× | 3.48% | PASS |
| mem_reads | `groupby_scan` | 32.38ms | 34.46ms | 1.064× | 1.09% | PASS |
| mem_reads | `index_join` | 7.21ms | 9.64ms | 1.338× | 2.94% | PASS |
| mem_reads | `index_join_scan` | 4.70ms | 5.46ms | 1.162× | 2.93% | PASS |
| mem_reads | `types_table_scan` | 1.17s | 1.27s | 1.087× | 1.03% | PASS |
| mem_reads | `table_scan` | 1.44s | 1.41s | 0.978× | 1.00% | PASS |
| mem_reads | `oltp_read_only` | 127.59ms | 140.60ms | 1.102× | 1.66% | PASS |
| mem_writes | `oltp_bulk_insert` | 232.37ms | 355.56ms | 1.530× | 0.70% | PASS |
| mem_writes | `oltp_insert` | 22.32ms | 40.17ms | 1.800× | 1.55% | PASS |
| mem_writes | `oltp_update_index` | 74.99ms | 137.91ms | 1.839× | 2.34% | PASS |
| mem_writes | `oltp_update_non_index` | 49.67ms | 88.78ms | 1.787× | 1.28% | PASS |
| mem_writes | `oltp_delete_insert` | 52.93ms | 106.29ms | 2.008× | 1.62% | PASS |
| mem_writes | `oltp_write_only` | 29.95ms | 62.75ms | 2.095× | 1.63% | PASS |
| mem_writes | `types_delete_insert` | 33.89ms | 56.30ms | 1.661× | 1.52% | PASS |
| mem_writes | `oltp_read_write` | 90.56ms | 143.07ms | 1.580× | 2.01% | PASS |
| file_reads | `oltp_point_select` | 105.09ms | 64.09ms | 0.610× | 0.97% | PASS |
| file_reads | `oltp_range_select` | 22.04ms | 17.11ms | 0.776× | 2.49% | PASS |
| file_reads | `oltp_sum_range` | 20.70ms | 17.26ms | 0.834× | 1.96% | PASS |
| file_reads | `oltp_order_range` | 3.93ms | 3.50ms | 0.890× | 1.68% | PASS |
| file_reads | `oltp_distinct_range` | 5.09ms | 4.63ms | 0.909× | 1.36% | PASS |
| file_reads | `oltp_index_scan` | 12.58ms | 9.23ms | 0.734× | 1.84% | PASS |
| file_reads | `select_random_points` | 27.86ms | 24.65ms | 0.885× | 2.50% | PASS |
| file_reads | `select_random_ranges` | 11.71ms | 7.92ms | 0.676× | 1.32% | PASS |
| file_reads | `covering_index_scan` | 13.32ms | 7.43ms | 0.558× | 2.01% | PASS |
| file_reads | `groupby_scan` | 33.72ms | 35.09ms | 1.040× | 0.96% | PASS |
| file_reads | `index_join` | 12.30ms | 11.48ms | 0.933× | 2.44% | PASS |
| file_reads | `index_join_scan` | 6.01ms | 6.11ms | 1.017× | 2.98% | PASS |
| file_reads | `types_table_scan` | 1.16s | 1.26s | 1.083× | 0.95% | PASS |
| file_reads | `table_scan` | 1.45s | 1.41s | 0.970× | 0.84% | PASS |
| file_reads | `oltp_read_only` | 237.69ms | 178.22ms | 0.750× | 0.99% | PASS |
| file_writes | `oltp_bulk_insert` | 253.06ms | 386.88ms | 1.529× | 1.11% | PASS |
| file_writes | `oltp_insert` | 50.10ms | 52.59ms | 1.050× | 7.06% | PASS |
| file_writes | `oltp_update_index` | 120.36ms | 172.02ms | 1.429× | 1.78% | PASS |
| file_writes | `oltp_update_non_index` | 104.51ms | 114.38ms | 1.094× | 10.90% | PASS |
| file_writes | `oltp_delete_insert` | 98.78ms | 135.05ms | 1.367× | 1.24% | PASS |
| file_writes | `oltp_write_only` | 86.05ms | 86.20ms | 1.002× | 13.46% | PASS |
| file_writes | `types_delete_insert` | 60.02ms | 75.67ms | 1.261× | 1.77% | PASS |
| file_writes | `oltp_read_write` | 142.05ms | 166.69ms | 1.173× | 4.95% | PASS |
| ac_reads | `oltp_point_select` | 55.66ms | 64.37ms | 1.156× | 1.03% | PASS |
| ac_reads | `oltp_range_select` | 17.33ms | 17.14ms | 0.989× | 2.57% | PASS |
| ac_reads | `oltp_sum_range` | 15.71ms | 17.27ms | 1.099× | 1.93% | PASS |
| ac_reads | `oltp_order_range` | 3.41ms | 3.49ms | 1.025× | 1.19% | PASS |
| ac_reads | `oltp_distinct_range` | 4.53ms | 4.61ms | 1.017× | 1.32% | PASS |
| ac_reads | `oltp_index_scan` | 7.67ms | 9.27ms | 1.209× | 1.38% | PASS |
| ac_reads | `select_random_points` | 22.10ms | 25.06ms | 1.134× | 2.81% | PASS |
| ac_reads | `select_random_ranges` | 6.82ms | 7.90ms | 1.159× | 1.43% | PASS |
| ac_reads | `covering_index_scan` | 8.36ms | 7.42ms | 0.889× | 2.16% | PASS |
| ac_reads | `groupby_scan` | 32.94ms | 35.24ms | 1.070× | 1.10% | PASS |
| ac_reads | `index_join` | 9.60ms | 11.44ms | 1.192× | 1.93% | PASS |
| ac_reads | `index_join_scan` | 5.41ms | 6.00ms | 1.109× | 4.02% | PASS |
| ac_reads | `types_table_scan` | 1.16s | 1.26s | 1.087× | 1.15% | PASS |
| ac_reads | `table_scan` | 1.44s | 1.41s | 0.976× | 0.85% | PASS |
| ac_reads | `oltp_read_only` | 163.37ms | 178.82ms | 1.095× | 1.28% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 27.99ms | 79.85ms | 2.853× | 7.73% | PASS |
| ac_writes | `oltp_insert_ac` | 29.98ms | 96.34ms | 3.213× | 7.75% | PASS |
| ac_writes | `oltp_update_index_ac` | 33.54ms | 115.16ms | 3.433× | 7.69% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 27.40ms | 91.84ms | 3.352× | 9.21% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 30.29ms | 105.68ms | 3.489× | 13.63% | PASS |
| ac_writes | `oltp_write_only_ac` | 30.70ms | 102.81ms | 3.349× | 7.60% | PASS |
| ac_writes | `types_delete_insert_ac` | 27.05ms | 93.80ms | 3.468× | 6.97% | PASS |
| ac_writes | `oltp_read_write_ac` | 36.40ms | 112.06ms | 3.079× | 7.63% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.34ms | 27.71ms | 1.139× | 2.45% | PASS |
| mem_reads | `oltp_range_select` | 10.94ms | 10.90ms | 0.997× | 3.15% | PASS |
| mem_reads | `oltp_sum_range` | 9.91ms | 10.54ms | 1.064× | 3.29% | PASS |
| mem_reads | `oltp_order_range` | 2.42ms | 2.52ms | 1.039× | 1.80% | PASS |
| mem_reads | `oltp_distinct_range` | 3.38ms | 3.31ms | 0.978× | 1.20% | PASS |
| mem_reads | `oltp_index_scan` | 3.75ms | 4.86ms | 1.297× | 2.52% | PASS |
| mem_reads | `select_random_points` | 14.48ms | 16.28ms | 1.125× | 3.05% | PASS |
| mem_reads | `select_random_ranges` | 3.57ms | 4.29ms | 1.203× | 2.76% | PASS |
| mem_reads | `covering_index_scan` | 3.61ms | 3.80ms | 1.053× | 2.81% | PASS |
| mem_reads | `groupby_scan` | 26.92ms | 27.87ms | 1.035× | 0.93% | PASS |
| mem_reads | `index_join` | 5.66ms | 7.70ms | 1.361× | 4.03% | PASS |
| mem_reads | `index_join_scan` | 3.70ms | 4.77ms | 1.287× | 2.59% | PASS |
| mem_reads | `types_table_scan` | 885.75ms | 975.31ms | 1.101× | 0.95% | PASS |
| mem_reads | `table_scan` | 1.11s | 1.07s | 0.964× | 6.04% | PASS |
| mem_reads | `oltp_read_only` | 92.70ms | 99.84ms | 1.077× | 1.12% | PASS |
| mem_writes | `oltp_bulk_insert` | 183.22ms | 254.29ms | 1.388× | 0.84% | PASS |
| mem_writes | `oltp_insert` | 15.91ms | 29.52ms | 1.856× | 1.20% | PASS |
| mem_writes | `oltp_update_index` | 54.68ms | 100.98ms | 1.847× | 1.18% | PASS |
| mem_writes | `oltp_update_non_index` | 39.48ms | 64.99ms | 1.646× | 1.21% | PASS |
| mem_writes | `oltp_delete_insert` | 40.15ms | 80.74ms | 2.011× | 2.10% | PASS |
| mem_writes | `oltp_write_only` | 22.78ms | 47.84ms | 2.100× | 1.57% | PASS |
| mem_writes | `types_delete_insert` | 25.62ms | 39.79ms | 1.553× | 1.75% | PASS |
| mem_writes | `oltp_read_write` | 66.02ms | 104.75ms | 1.587× | 2.42% | PASS |
| file_reads | `oltp_point_select` | 99.59ms | 52.78ms | 0.530× | 0.98% | PASS |
| file_reads | `oltp_range_select` | 19.41ms | 13.56ms | 0.699× | 0.92% | PASS |
| file_reads | `oltp_sum_range` | 18.01ms | 13.23ms | 0.735× | 0.89% | PASS |
| file_reads | `oltp_order_range` | 3.39ms | 2.85ms | 0.841× | 1.24% | PASS |
| file_reads | `oltp_distinct_range` | 4.26ms | 3.62ms | 0.849× | 1.01% | PASS |
| file_reads | `oltp_index_scan` | 11.73ms | 7.60ms | 0.648× | 0.47% | PASS |
| file_reads | `select_random_points` | 22.78ms | 19.08ms | 0.838× | 1.86% | PASS |
| file_reads | `select_random_ranges` | 11.22ms | 6.76ms | 0.603× | 0.76% | PASS |
| file_reads | `covering_index_scan` | 12.14ms | 6.41ms | 0.528× | 0.83% | PASS |
| file_reads | `groupby_scan` | 27.81ms | 28.16ms | 1.012× | 0.94% | PASS |
| file_reads | `index_join` | 10.40ms | 9.44ms | 0.907× | 1.34% | PASS |
| file_reads | `index_join_scan` | 4.72ms | 5.21ms | 1.104× | 2.02% | PASS |
| file_reads | `types_table_scan` | 906.67ms | 977.52ms | 1.078× | 2.06% | PASS |
| file_reads | `table_scan` | 1.12s | 1.08s | 0.965× | 4.90% | PASS |
| file_reads | `oltp_read_only` | 209.57ms | 139.09ms | 0.664× | 1.26% | PASS |
| file_writes | `oltp_bulk_insert` | 255.92ms | 343.47ms | 1.342× | 10.54% | PASS |
| file_writes | `oltp_insert` | 67.66ms | 68.66ms | 1.015× | 31.43% | PASS |
| file_writes | `oltp_update_index` | 197.52ms | 202.70ms | 1.026× | 14.54% | PASS |
| file_writes | `oltp_update_non_index` | 189.32ms | 130.06ms | 0.687× | 23.39% | PASS |
| file_writes | `oltp_delete_insert` | 180.93ms | 158.54ms | 0.876× | 21.03% | PASS |
| file_writes | `oltp_write_only` | 143.11ms | 109.19ms | 0.763× | 23.39% | PASS |
| file_writes | `types_delete_insert` | 100.81ms | 95.27ms | 0.945× | 19.10% | PASS |
| file_writes | `oltp_read_write` | 215.20ms | 174.97ms | 0.813× | 21.99% | PASS |
| ac_reads | `oltp_point_select` | 49.31ms | 53.04ms | 1.075× | 1.40% | PASS |
| ac_reads | `oltp_range_select` | 14.21ms | 13.52ms | 0.952× | 1.40% | PASS |
| ac_reads | `oltp_sum_range` | 13.00ms | 13.23ms | 1.018× | 1.24% | PASS |
| ac_reads | `oltp_order_range` | 2.93ms | 2.85ms | 0.972× | 0.97% | PASS |
| ac_reads | `oltp_distinct_range` | 3.77ms | 3.60ms | 0.955× | 0.73% | PASS |
| ac_reads | `oltp_index_scan` | 6.82ms | 7.61ms | 1.116× | 1.00% | PASS |
| ac_reads | `select_random_points` | 17.35ms | 18.84ms | 1.086× | 1.32% | PASS |
| ac_reads | `select_random_ranges` | 6.26ms | 6.73ms | 1.076× | 1.18% | PASS |
| ac_reads | `covering_index_scan` | 7.11ms | 6.39ms | 0.899× | 1.12% | PASS |
| ac_reads | `groupby_scan` | 27.25ms | 28.22ms | 1.036× | 0.82% | PASS |
| ac_reads | `index_join` | 7.76ms | 9.29ms | 1.197× | 1.52% | PASS |
| ac_reads | `index_join_scan` | 4.20ms | 5.08ms | 1.209× | 1.40% | PASS |
| ac_reads | `types_table_scan` | 870.45ms | 965.41ms | 1.109× | 0.47% | PASS |
| ac_reads | `table_scan` | 1.04s | 1.06s | 1.020× | 1.97% | PASS |
| ac_reads | `oltp_read_only` | 132.96ms | 136.88ms | 1.030× | 1.92% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 51.01ms | 135.85ms | 2.663× | 68.19% | PASS |
| ac_writes | `oltp_insert_ac` | 69.20ms | 183.86ms | 2.657× | 60.89% | PASS |
| ac_writes | `oltp_update_index_ac` | 65.54ms | 307.54ms | 4.692× | 59.39% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 51.23ms | 188.07ms | 3.672× | 59.37% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 61.80ms | 344.93ms | 5.581× | 69.57% | PASS |
| ac_writes | `oltp_write_only_ac` | 73.61ms | 249.65ms | 3.391× | 53.04% | PASS |
| ac_writes | `types_delete_insert_ac` | 56.73ms | 224.05ms | 3.949× | 71.70% | PASS |
| ac_writes | `oltp_read_write_ac` | 133.02ms | 279.87ms | 2.104× | 62.01% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.92ms | 23.94ms | 1.145× | 2.45% | PASS |
| mem_reads | `oltp_range_select` | 11.57ms | 11.62ms | 1.004× | 3.32% | PASS |
| mem_reads | `oltp_sum_range` | 10.68ms | 11.45ms | 1.072× | 2.37% | PASS |
| mem_reads | `oltp_order_range` | 2.25ms | 2.23ms | 0.988× | 2.90% | PASS |
| mem_reads | `oltp_distinct_range` | 2.65ms | 2.73ms | 1.033× | 4.13% | PASS |
| mem_reads | `oltp_index_scan` | 3.04ms | 4.00ms | 1.315× | 2.63% | PASS |
| mem_reads | `select_random_points` | 18.52ms | 20.52ms | 1.108× | 2.99% | PASS |
| mem_reads | `select_random_ranges` | 4.85ms | 5.30ms | 1.094× | 1.73% | PASS |
| mem_reads | `covering_index_scan` | 2.53ms | 2.93ms | 1.159× | 5.13% | PASS |
| mem_reads | `groupby_scan` | 21.45ms | 22.16ms | 1.033× | 1.47% | PASS |
| mem_reads | `index_join` | 5.09ms | 6.38ms | 1.254× | 3.37% | PASS |
| mem_reads | `index_join_scan` | 2.97ms | 4.48ms | 1.509× | 3.97% | PASS |
| mem_reads | `types_table_scan` | 724.31ms | 771.33ms | 1.065× | 1.93% | PASS |
| mem_reads | `table_scan` | 866.64ms | 862.85ms | 0.996× | 7.00% | PASS |
| mem_reads | `oltp_read_only` | 98.11ms | 96.30ms | 0.981× | 1.83% | PASS |
| mem_writes | `oltp_bulk_insert` | 150.25ms | 214.44ms | 1.427× | 1.26% | PASS |
| mem_writes | `oltp_insert` | 11.94ms | 23.88ms | 2.000× | 1.34% | PASS |
| mem_writes | `oltp_update_index` | 48.02ms | 90.70ms | 1.889× | 2.98% | PASS |
| mem_writes | `oltp_update_non_index` | 35.24ms | 59.25ms | 1.681× | 2.33% | PASS |
| mem_writes | `oltp_delete_insert` | 33.61ms | 69.03ms | 2.054× | 2.48% | PASS |
| mem_writes | `oltp_write_only` | 18.27ms | 41.78ms | 2.286× | 2.52% | PASS |
| mem_writes | `types_delete_insert` | 22.07ms | 37.77ms | 1.712× | 1.86% | PASS |
| mem_writes | `oltp_read_write` | 65.63ms | 96.48ms | 1.470× | 3.41% | PASS |
| file_reads | `oltp_point_select` | 80.06ms | 45.21ms | 0.565× | 2.10% | PASS |
| file_reads | `oltp_range_select` | 18.85ms | 14.27ms | 0.757× | 2.21% | PASS |
| file_reads | `oltp_sum_range` | 17.39ms | 13.84ms | 0.796× | 1.94% | PASS |
| file_reads | `oltp_order_range` | 2.98ms | 2.48ms | 0.831× | 1.99% | PASS |
| file_reads | `oltp_distinct_range` | 3.55ms | 3.00ms | 0.843× | 1.37% | PASS |
| file_reads | `oltp_index_scan` | 9.33ms | 6.40ms | 0.685× | 1.79% | PASS |
| file_reads | `select_random_points` | 25.38ms | 22.11ms | 0.871× | 3.26% | PASS |
| file_reads | `select_random_ranges` | 10.69ms | 7.34ms | 0.687× | 1.33% | PASS |
| file_reads | `covering_index_scan` | 8.72ms | 5.17ms | 0.594× | 2.32% | PASS |
| file_reads | `groupby_scan` | 22.53ms | 22.51ms | 0.999× | 1.58% | PASS |
| file_reads | `index_join` | 8.76ms | 8.68ms | 0.991× | 3.98% | PASS |
| file_reads | `index_join_scan` | 3.73ms | 4.54ms | 1.218× | 6.00% | PASS |
| file_reads | `types_table_scan` | 794.50ms | 784.95ms | 0.988× | 2.83% | PASS |
| file_reads | `table_scan` | 970.52ms | 870.60ms | 0.897× | 2.89% | PASS |
| file_reads | `oltp_read_only` | 172.32ms | 118.97ms | 0.690× | 2.64% | PASS |
| file_writes | `oltp_bulk_insert` | 197.88ms | 267.38ms | 1.351× | 3.27% | PASS |
| file_writes | `oltp_insert` | 19.51ms | 38.87ms | 1.992× | 14.72% | PASS |
| file_writes | `oltp_update_index` | 148.17ms | 150.83ms | 1.018× | 3.13% | PASS |
| file_writes | `oltp_update_non_index` | 123.15ms | 109.67ms | 0.890× | 3.68% | PASS |
| file_writes | `oltp_delete_insert` | 116.03ms | 120.18ms | 1.036× | 5.44% | PASS |
| file_writes | `oltp_write_only` | 89.95ms | 91.89ms | 1.022× | 6.88% | PASS |
| file_writes | `types_delete_insert` | 75.62ms | 70.89ms | 0.937× | 12.26% | PASS |
| file_writes | `oltp_read_write` | 130.55ms | 145.26ms | 1.113× | 3.85% | PASS |
| ac_reads | `oltp_point_select` | 43.87ms | 47.33ms | 1.079× | 2.94% | PASS |
| ac_reads | `oltp_range_select` | 14.99ms | 14.48ms | 0.966× | 3.33% | PASS |
| ac_reads | `oltp_sum_range` | 13.58ms | 13.82ms | 1.017× | 3.23% | PASS |
| ac_reads | `oltp_order_range` | 2.70ms | 2.50ms | 0.926× | 1.64% | PASS |
| ac_reads | `oltp_distinct_range` | 3.29ms | 3.07ms | 0.935× | 2.61% | PASS |
| ac_reads | `oltp_index_scan` | 5.69ms | 6.59ms | 1.157× | 3.08% | PASS |
| ac_reads | `select_random_points` | 21.71ms | 23.26ms | 1.071× | 4.60% | PASS |
| ac_reads | `select_random_ranges` | 6.92ms | 7.37ms | 1.066× | 2.27% | PASS |
| ac_reads | `covering_index_scan` | 4.87ms | 5.07ms | 1.042× | 5.73% | PASS |
| ac_reads | `groupby_scan` | 20.94ms | 21.32ms | 1.018× | 1.95% | PASS |
| ac_reads | `index_join` | 6.57ms | 8.27ms | 1.258× | 3.65% | PASS |
| ac_reads | `index_join_scan` | 3.56ms | 5.12ms | 1.439× | 12.86% | PASS |
| ac_reads | `types_table_scan` | 841.57ms | 815.55ms | 0.969× | 4.39% | PASS |
| ac_reads | `table_scan` | 1.04s | 916.60ms | 0.883× | 4.81% | PASS |
| ac_reads | `oltp_read_only` | 115.57ms | 119.58ms | 1.035× | 3.85% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.57ms | 59.99ms | 2.781× | 6.57% | PASS |
| ac_writes | `oltp_insert_ac` | 25.81ms | 86.46ms | 3.350× | 7.17% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.77ms | 92.36ms | 3.450× | 5.41% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.33ms | 72.16ms | 3.231× | 6.16% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.12ms | 74.72ms | 3.232× | 5.00% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.92ms | 80.08ms | 3.347× | 7.88% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.84ms | 72.11ms | 3.024× | 12.16% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.35ms | 86.53ms | 3.052× | 5.95% | PASS |

</details>

## Version-control latency

Wall time: 1m 59s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 64.56ms | 200.00ms | 32.3% | 0.26% | PASS |
| `status_dirty_many_tables` | 66.72ms | 200.00ms | 33.4% | 0.26% | PASS |
| `diff_regular_working_one_table` | 60.88ms | 150.00ms | 40.6% | 0.35% | PASS |
| `diff_regular_working_many_tables` | 71.32ms | 200.00ms | 35.7% | 0.19% | PASS |
| `diff_stat_working_many_tables` | 71.31ms | 200.00ms | 35.7% | 0.25% | PASS |
| `diff_schema_working_many_tables` | 71.66ms | 200.00ms | 35.8% | 0.22% | PASS |
| `branch_list_many_branches` | 18.18ms | 100.00ms | 18.2% | 0.89% | PASS |
| `branch_create_delete` | 24.61ms | 100.00ms | 24.6% | 3.01% | PASS |
| `checkout_branch_clean` | 99.12ms | 200.00ms | 49.6% | 10.01% | PASS |
| `merge_data_no_conflicts` | 29.78ms | 150.00ms | 19.9% | 1.44% | PASS |
| `merge_schema_no_conflicts` | 17.68ms | 100.00ms | 17.7% | 1.76% | PASS |
| `merge_data_conflicts` | 99.34ms | 250.00ms | 39.7% | 0.12% | PASS |
| `merge_data_conflicts_with_resolve` | 99.15ms | 250.00ms | 39.7% | 0.15% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
