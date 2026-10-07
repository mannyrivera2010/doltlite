# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-07 18:24 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260927.320.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/37655017470)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 9m 28s | 8.68s | 10.63s | 1.225× | 1.00% | **PASS** |
| textpk | 69 | 55 | 1h 31m 51s | 9.57s | 11.69s | 1.221× | 1.47% | **PASS** |
| blobpk | 69 | 55 | 1h 8m 56s | 7.14s | 8.53s | 1.194× | 2.85% | **PASS** |
| compositepk | 69 | 55 | 1h 29m 20s | 10.34s | 12.23s | 1.183× | 1.57% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 21.52ms | 24.13ms | 1.121× | 1.01% | PASS |
| mem_reads | `oltp_range_select` | 9.70ms | 11.47ms | 1.183× | 1.06% | PASS |
| mem_reads | `oltp_sum_range` | 8.86ms | 10.71ms | 1.209× | 1.34% | PASS |
| mem_reads | `oltp_order_range` | 2.56ms | 2.83ms | 1.105× | 0.91% | PASS |
| mem_reads | `oltp_distinct_range` | 3.48ms | 3.76ms | 1.081× | 1.33% | PASS |
| mem_reads | `oltp_index_scan` | 3.54ms | 4.34ms | 1.224× | 1.83% | PASS |
| mem_reads | `select_random_points` | 9.90ms | 10.71ms | 1.081× | 1.34% | PASS |
| mem_reads | `select_random_ranges` | 2.66ms | 3.40ms | 1.277× | 1.41% | PASS |
| mem_reads | `covering_index_scan` | 3.43ms | 3.40ms | 0.991× | 2.51% | PASS |
| mem_reads | `groupby_scan` | 29.15ms | 30.86ms | 1.059× | 1.38% | PASS |
| mem_reads | `index_join` | 5.23ms | 7.05ms | 1.349× | 1.93% | PASS |
| mem_reads | `index_join_scan` | 2.88ms | 4.12ms | 1.430× | 1.57% | PASS |
| mem_reads | `types_table_scan` | 980.63ms | 1.18s | 1.208× | 0.44% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.35s | 1.155× | 0.35% | PASS |
| mem_reads | `oltp_read_only` | 96.16ms | 109.85ms | 1.142× | 0.64% | PASS |
| mem_writes | `oltp_bulk_insert` | 145.30ms | 196.24ms | 1.351× | 0.48% | PASS |
| mem_writes | `oltp_insert` | 13.12ms | 23.08ms | 1.759× | 0.53% | PASS |
| mem_writes | `oltp_update_index` | 44.66ms | 91.86ms | 2.057× | 0.95% | PASS |
| mem_writes | `oltp_update_non_index` | 29.20ms | 50.14ms | 1.717× | 1.01% | PASS |
| mem_writes | `oltp_delete_insert` | 39.88ms | 65.77ms | 1.649× | 0.86% | PASS |
| mem_writes | `oltp_write_only` | 18.56ms | 37.03ms | 1.995× | 1.09% | PASS |
| mem_writes | `types_delete_insert` | 21.02ms | 33.71ms | 1.603× | 0.85% | PASS |
| mem_writes | `oltp_read_write` | 56.47ms | 91.93ms | 1.628× | 1.14% | PASS |
| file_reads | `oltp_point_select` | 52.28ms | 34.06ms | 0.651× | 0.60% | PASS |
| file_reads | `oltp_range_select` | 13.17ms | 12.97ms | 0.985× | 0.55% | PASS |
| file_reads | `oltp_sum_range` | 12.84ms | 12.27ms | 0.956× | 1.18% | PASS |
| file_reads | `oltp_order_range` | 2.97ms | 3.02ms | 1.017× | 1.00% | PASS |
| file_reads | `oltp_distinct_range` | 3.93ms | 3.95ms | 1.004× | 0.61% | PASS |
| file_reads | `oltp_index_scan` | 6.98ms | 5.88ms | 0.841× | 0.53% | PASS |
| file_reads | `select_random_points` | 14.03ms | 12.43ms | 0.886× | 1.05% | PASS |
| file_reads | `select_random_ranges` | 5.96ms | 4.57ms | 0.768× | 0.78% | PASS |
| file_reads | `covering_index_scan` | 6.94ms | 4.96ms | 0.714× | 0.80% | PASS |
| file_reads | `groupby_scan` | 30.61ms | 32.16ms | 1.051× | 0.35% | PASS |
| file_reads | `index_join` | 7.33ms | 8.62ms | 1.176× | 0.75% | PASS |
| file_reads | `index_join_scan` | 3.41ms | 4.50ms | 1.319× | 1.01% | PASS |
| file_reads | `types_table_scan` | 1.02s | 1.23s | 1.209× | 0.52% | PASS |
| file_reads | `table_scan` | 1.14s | 1.32s | 1.159× | 0.32% | PASS |
| file_reads | `oltp_read_only` | 137.89ms | 122.18ms | 0.886× | 0.56% | PASS |
| file_writes | `oltp_bulk_insert` | 190.15ms | 249.80ms | 1.314× | 1.45% | PASS |
| file_writes | `oltp_insert` | 25.71ms | 39.06ms | 1.520× | 2.54% | PASS |
| file_writes | `oltp_update_index` | 139.12ms | 157.07ms | 1.129× | 1.96% | PASS |
| file_writes | `oltp_update_non_index` | 111.54ms | 103.19ms | 0.925× | 1.38% | PASS |
| file_writes | `oltp_delete_insert` | 119.52ms | 122.31ms | 1.023× | 0.82% | PASS |
| file_writes | `oltp_write_only` | 87.60ms | 85.17ms | 0.972× | 1.23% | PASS |
| file_writes | `types_delete_insert` | 75.69ms | 67.92ms | 0.897× | 2.06% | PASS |
| file_writes | `oltp_read_write` | 126.29ms | 140.93ms | 1.116× | 0.85% | PASS |
| ac_reads | `oltp_point_select` | 31.95ms | 34.36ms | 1.075× | 0.50% | PASS |
| ac_reads | `oltp_range_select` | 11.08ms | 13.03ms | 1.176× | 0.51% | PASS |
| ac_reads | `oltp_sum_range` | 10.67ms | 12.28ms | 1.151× | 0.45% | PASS |
| ac_reads | `oltp_order_range` | 2.74ms | 3.01ms | 1.099× | 0.63% | PASS |
| ac_reads | `oltp_distinct_range` | 3.72ms | 3.95ms | 1.062× | 0.73% | PASS |
| ac_reads | `oltp_index_scan` | 4.98ms | 5.87ms | 1.179× | 0.97% | PASS |
| ac_reads | `select_random_points` | 11.77ms | 12.24ms | 1.040× | 1.22% | PASS |
| ac_reads | `select_random_ranges` | 3.90ms | 4.57ms | 1.170× | 0.77% | PASS |
| ac_reads | `covering_index_scan` | 4.79ms | 4.93ms | 1.029× | 0.59% | PASS |
| ac_reads | `groupby_scan` | 30.09ms | 32.01ms | 1.064× | 0.47% | PASS |
| ac_reads | `index_join` | 6.32ms | 8.56ms | 1.354× | 1.17% | PASS |
| ac_reads | `index_join_scan` | 3.21ms | 4.47ms | 1.391× | 1.00% | PASS |
| ac_reads | `types_table_scan` | 1.01s | 1.22s | 1.211× | 0.39% | PASS |
| ac_reads | `table_scan` | 1.16s | 1.35s | 1.162× | 0.64% | PASS |
| ac_reads | `oltp_read_only` | 110.21ms | 124.94ms | 1.134× | 0.49% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 25.65ms | 69.10ms | 2.694× | 4.99% | PASS |
| ac_writes | `oltp_insert_ac` | 26.85ms | 80.35ms | 2.993× | 5.75% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.79ms | 92.56ms | 3.215× | 4.08% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 26.95ms | 78.95ms | 2.930× | 5.42% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 28.19ms | 87.08ms | 3.089× | 3.20% | PASS |
| ac_writes | `oltp_write_only_ac` | 28.16ms | 87.31ms | 3.100× | 2.95% | PASS |
| ac_writes | `types_delete_insert_ac` | 26.69ms | 78.03ms | 2.923× | 4.72% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.94ms | 91.88ms | 2.877× | 4.06% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.25ms | 37.31ms | 1.276× | 1.49% | PASS |
| mem_reads | `oltp_range_select` | 13.01ms | 14.10ms | 1.083× | 2.16% | PASS |
| mem_reads | `oltp_sum_range` | 11.75ms | 14.06ms | 1.197× | 1.98% | PASS |
| mem_reads | `oltp_order_range` | 2.92ms | 3.14ms | 1.077× | 1.68% | PASS |
| mem_reads | `oltp_distinct_range` | 3.96ms | 4.24ms | 1.071× | 1.31% | PASS |
| mem_reads | `oltp_index_scan` | 4.41ms | 6.19ms | 1.402× | 1.67% | PASS |
| mem_reads | `select_random_points` | 17.33ms | 20.62ms | 1.190× | 1.38% | PASS |
| mem_reads | `select_random_ranges` | 3.92ms | 5.12ms | 1.309× | 1.78% | PASS |
| mem_reads | `covering_index_scan` | 4.50ms | 4.42ms | 0.983× | 1.14% | PASS |
| mem_reads | `groupby_scan` | 31.52ms | 34.18ms | 1.084× | 1.25% | PASS |
| mem_reads | `index_join` | 6.84ms | 8.94ms | 1.308× | 1.53% | PASS |
| mem_reads | `index_join_scan` | 4.49ms | 5.28ms | 1.176× | 2.09% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.23s | 1.170× | 0.58% | PASS |
| mem_reads | `table_scan` | 1.22s | 1.37s | 1.119× | 0.67% | PASS |
| mem_reads | `oltp_read_only` | 117.18ms | 136.45ms | 1.165× | 1.11% | PASS |
| mem_writes | `oltp_bulk_insert` | 233.92ms | 357.18ms | 1.527× | 1.03% | PASS |
| mem_writes | `oltp_insert` | 21.45ms | 39.59ms | 1.846× | 1.22% | PASS |
| mem_writes | `oltp_update_index` | 69.59ms | 132.24ms | 1.900× | 0.73% | PASS |
| mem_writes | `oltp_update_non_index` | 47.81ms | 86.47ms | 1.809× | 1.12% | PASS |
| mem_writes | `oltp_delete_insert` | 50.41ms | 103.31ms | 2.049× | 0.78% | PASS |
| mem_writes | `oltp_write_only` | 28.56ms | 61.06ms | 2.138× | 0.80% | PASS |
| mem_writes | `types_delete_insert` | 32.87ms | 54.93ms | 1.671× | 1.47% | PASS |
| mem_writes | `oltp_read_write` | 83.69ms | 138.71ms | 1.657× | 1.00% | PASS |
| file_reads | `oltp_point_select` | 104.86ms | 63.32ms | 0.604× | 1.03% | PASS |
| file_reads | `oltp_range_select` | 20.93ms | 16.93ms | 0.809× | 2.84% | PASS |
| file_reads | `oltp_sum_range` | 19.65ms | 17.01ms | 0.866× | 1.89% | PASS |
| file_reads | `oltp_order_range` | 3.79ms | 3.50ms | 0.924× | 2.22% | PASS |
| file_reads | `oltp_distinct_range` | 4.91ms | 4.62ms | 0.942× | 1.28% | PASS |
| file_reads | `oltp_index_scan` | 12.13ms | 9.16ms | 0.755× | 1.37% | PASS |
| file_reads | `select_random_points` | 26.30ms | 24.07ms | 0.915× | 1.77% | PASS |
| file_reads | `select_random_ranges` | 11.51ms | 7.84ms | 0.682× | 1.25% | PASS |
| file_reads | `covering_index_scan` | 12.35ms | 7.40ms | 0.599× | 1.23% | PASS |
| file_reads | `groupby_scan` | 32.69ms | 34.80ms | 1.065× | 1.17% | PASS |
| file_reads | `index_join` | 11.27ms | 11.24ms | 0.997× | 1.30% | PASS |
| file_reads | `index_join_scan` | 5.59ms | 5.78ms | 1.034× | 1.66% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.22s | 1.167× | 0.58% | PASS |
| file_reads | `table_scan` | 1.22s | 1.36s | 1.115× | 0.86% | PASS |
| file_reads | `oltp_read_only` | 227.60ms | 176.67ms | 0.776× | 0.91% | PASS |
| file_writes | `oltp_bulk_insert` | 254.87ms | 385.86ms | 1.514× | 0.70% | PASS |
| file_writes | `oltp_insert` | 50.87ms | 52.58ms | 1.034× | 24.13% | PASS |
| file_writes | `oltp_update_index` | 112.87ms | 169.58ms | 1.502× | 1.57% | PASS |
| file_writes | `oltp_update_non_index` | 97.14ms | 113.25ms | 1.166× | 7.24% | PASS |
| file_writes | `oltp_delete_insert` | 89.47ms | 133.49ms | 1.492× | 1.43% | PASS |
| file_writes | `oltp_write_only` | 81.75ms | 85.19ms | 1.042× | 14.84% | PASS |
| file_writes | `types_delete_insert` | 56.01ms | 75.16ms | 1.342× | 1.59% | PASS |
| file_writes | `oltp_read_write` | 146.41ms | 162.91ms | 1.113× | 3.53% | PASS |
| ac_reads | `oltp_point_select` | 55.23ms | 63.79ms | 1.155× | 1.06% | PASS |
| ac_reads | `oltp_range_select` | 16.40ms | 16.94ms | 1.033× | 1.77% | PASS |
| ac_reads | `oltp_sum_range` | 15.18ms | 17.06ms | 1.124× | 1.51% | PASS |
| ac_reads | `oltp_order_range` | 3.40ms | 3.54ms | 1.040× | 1.66% | PASS |
| ac_reads | `oltp_distinct_range` | 4.46ms | 4.63ms | 1.037× | 1.33% | PASS |
| ac_reads | `oltp_index_scan` | 7.54ms | 9.20ms | 1.220× | 1.09% | PASS |
| ac_reads | `select_random_points` | 21.20ms | 24.09ms | 1.136× | 1.94% | PASS |
| ac_reads | `select_random_ranges` | 6.78ms | 7.86ms | 1.159× | 0.98% | PASS |
| ac_reads | `covering_index_scan` | 8.25ms | 7.41ms | 0.898× | 1.90% | PASS |
| ac_reads | `groupby_scan` | 32.32ms | 34.77ms | 1.076× | 1.04% | PASS |
| ac_reads | `index_join` | 9.24ms | 11.28ms | 1.221× | 2.45% | PASS |
| ac_reads | `index_join_scan` | 5.09ms | 5.76ms | 1.130× | 2.06% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.22s | 1.165× | 0.64% | PASS |
| ac_reads | `table_scan` | 1.23s | 1.37s | 1.113× | 0.73% | PASS |
| ac_reads | `oltp_read_only` | 154.72ms | 175.97ms | 1.137× | 1.02% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.91ms | 69.86ms | 2.804× | 8.04% | PASS |
| ac_writes | `oltp_insert_ac` | 28.04ms | 85.46ms | 3.047× | 6.22% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.11ms | 103.06ms | 3.541× | 6.75% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.80ms | 80.91ms | 3.549× | 3.64% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.25ms | 94.02ms | 3.724× | 4.89% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.97ms | 91.78ms | 3.534× | 6.17% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.49ms | 83.56ms | 3.715× | 7.08% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.73ms | 101.07ms | 3.087× | 4.42% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 21.61ms | 24.13ms | 1.117× | 2.18% | PASS |
| mem_reads | `oltp_range_select` | 9.70ms | 10.10ms | 1.041× | 4.09% | PASS |
| mem_reads | `oltp_sum_range` | 10.72ms | 10.54ms | 0.983× | 2.26% | PASS |
| mem_reads | `oltp_order_range` | 2.25ms | 2.25ms | 0.996× | 1.69% | PASS |
| mem_reads | `oltp_distinct_range` | 2.76ms | 2.70ms | 0.978× | 1.61% | PASS |
| mem_reads | `oltp_index_scan` | 3.51ms | 4.72ms | 1.345× | 2.02% | PASS |
| mem_reads | `select_random_points` | 14.60ms | 15.68ms | 1.074× | 2.99% | PASS |
| mem_reads | `select_random_ranges` | 3.13ms | 3.58ms | 1.143× | 1.98% | PASS |
| mem_reads | `covering_index_scan` | 2.54ms | 2.88ms | 1.137× | 4.78% | PASS |
| mem_reads | `groupby_scan` | 18.82ms | 19.51ms | 1.037× | 1.86% | PASS |
| mem_reads | `index_join` | 4.23ms | 5.41ms | 1.279× | 1.60% | PASS |
| mem_reads | `index_join_scan` | 2.35ms | 4.09ms | 1.743× | 4.43% | PASS |
| mem_reads | `types_table_scan` | 683.55ms | 834.02ms | 1.220× | 1.10% | PASS |
| mem_reads | `table_scan` | 841.45ms | 961.33ms | 1.142× | 3.07% | PASS |
| mem_reads | `oltp_read_only` | 70.32ms | 78.12ms | 1.111× | 1.97% | PASS |
| mem_writes | `oltp_bulk_insert` | 145.64ms | 208.84ms | 1.434× | 0.99% | PASS |
| mem_writes | `oltp_insert` | 12.51ms | 25.98ms | 2.077× | 2.31% | PASS |
| mem_writes | `oltp_update_index` | 48.94ms | 97.11ms | 1.984× | 2.83% | PASS |
| mem_writes | `oltp_update_non_index` | 35.40ms | 61.55ms | 1.738× | 1.98% | PASS |
| mem_writes | `oltp_delete_insert` | 34.23ms | 74.81ms | 2.186× | 2.35% | PASS |
| mem_writes | `oltp_write_only` | 20.17ms | 46.51ms | 2.305× | 2.79% | PASS |
| mem_writes | `types_delete_insert` | 23.21ms | 40.00ms | 1.723× | 1.68% | PASS |
| mem_writes | `oltp_read_write` | 62.72ms | 95.66ms | 1.525× | 2.92% | PASS |
| file_reads | `oltp_point_select` | 81.89ms | 45.17ms | 0.552× | 2.12% | PASS |
| file_reads | `oltp_range_select` | 16.59ms | 12.13ms | 0.731× | 3.11% | PASS |
| file_reads | `oltp_sum_range` | 16.91ms | 12.25ms | 0.724× | 4.06% | PASS |
| file_reads | `oltp_order_range` | 2.90ms | 2.43ms | 0.835× | 2.47% | PASS |
| file_reads | `oltp_distinct_range` | 3.43ms | 2.90ms | 0.844× | 1.59% | PASS |
| file_reads | `oltp_index_scan` | 9.50ms | 6.49ms | 0.683× | 2.67% | PASS |
| file_reads | `select_random_points` | 18.97ms | 15.89ms | 0.838× | 3.19% | PASS |
| file_reads | `select_random_ranges` | 8.88ms | 5.59ms | 0.630× | 1.39% | PASS |
| file_reads | `covering_index_scan` | 9.72ms | 5.39ms | 0.555× | 2.13% | PASS |
| file_reads | `groupby_scan` | 21.56ms | 21.17ms | 0.982× | 1.58% | PASS |
| file_reads | `index_join` | 8.89ms | 8.19ms | 0.920× | 3.58% | PASS |
| file_reads | `index_join_scan` | 4.24ms | 4.65ms | 1.096× | 3.98% | PASS |
| file_reads | `types_table_scan` | 758.76ms | 903.87ms | 1.191× | 3.39% | PASS |
| file_reads | `table_scan` | 1.04s | 1.02s | 0.977× | 5.78% | PASS |
| file_reads | `oltp_read_only` | 169.36ms | 114.67ms | 0.677× | 2.36% | PASS |
| file_writes | `oltp_bulk_insert` | 195.68ms | 279.60ms | 1.429× | 3.69% | PASS |
| file_writes | `oltp_insert` | 36.98ms | 54.22ms | 1.466× | 12.07% | PASS |
| file_writes | `oltp_update_index` | 169.10ms | 180.22ms | 1.066× | 4.93% | PASS |
| file_writes | `oltp_update_non_index` | 121.61ms | 119.23ms | 0.980× | 1.97% | PASS |
| file_writes | `oltp_delete_insert` | 133.71ms | 139.79ms | 1.046× | 3.23% | PASS |
| file_writes | `oltp_write_only` | 100.32ms | 99.42ms | 0.991× | 9.72% | PASS |
| file_writes | `types_delete_insert` | 88.53ms | 84.02ms | 0.949× | 7.93% | PASS |
| file_writes | `oltp_read_write` | 131.30ms | 139.84ms | 1.065× | 5.64% | PASS |
| ac_reads | `oltp_point_select` | 41.52ms | 44.74ms | 1.078× | 1.79% | PASS |
| ac_reads | `oltp_range_select` | 12.47ms | 12.04ms | 0.966× | 2.28% | PASS |
| ac_reads | `oltp_sum_range` | 12.54ms | 11.93ms | 0.951× | 2.77% | PASS |
| ac_reads | `oltp_order_range` | 2.42ms | 2.34ms | 0.964× | 1.57% | PASS |
| ac_reads | `oltp_distinct_range` | 2.96ms | 2.83ms | 0.954× | 1.76% | PASS |
| ac_reads | `oltp_index_scan` | 5.05ms | 6.15ms | 1.217× | 3.02% | PASS |
| ac_reads | `select_random_points` | 14.46ms | 15.16ms | 1.048× | 2.98% | PASS |
| ac_reads | `select_random_ranges` | 5.11ms | 5.58ms | 1.091× | 2.25% | PASS |
| ac_reads | `covering_index_scan` | 5.95ms | 5.43ms | 0.912× | 3.57% | PASS |
| ac_reads | `groupby_scan` | 20.92ms | 20.94ms | 1.001× | 1.57% | PASS |
| ac_reads | `index_join` | 7.13ms | 8.23ms | 1.153× | 5.44% | PASS |
| ac_reads | `index_join_scan` | 3.94ms | 4.64ms | 1.177× | 7.39% | PASS |
| ac_reads | `types_table_scan` | 701.38ms | 861.30ms | 1.228× | 1.50% | PASS |
| ac_reads | `table_scan` | 800.79ms | 940.63ms | 1.175× | 2.85% | PASS |
| ac_reads | `oltp_read_only` | 94.69ms | 103.80ms | 1.096× | 3.51% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.64ms | 58.30ms | 2.693× | 5.35% | PASS |
| ac_writes | `oltp_insert_ac` | 24.31ms | 74.08ms | 3.047× | 4.08% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.73ms | 84.11ms | 3.147× | 5.28% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.65ms | 64.35ms | 3.117× | 5.36% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.55ms | 74.66ms | 3.312× | 6.16% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.55ms | 79.92ms | 3.393× | 10.60% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.13ms | 67.14ms | 3.177× | 6.42% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.70ms | 76.27ms | 2.968× | 4.71% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 35.65ms | 42.40ms | 1.189× | 1.70% | PASS |
| mem_reads | `oltp_range_select` | 20.67ms | 22.03ms | 1.066× | 1.67% | PASS |
| mem_reads | `oltp_sum_range` | 18.90ms | 21.55ms | 1.140× | 1.48% | PASS |
| mem_reads | `oltp_order_range` | 3.65ms | 3.94ms | 1.082× | 1.50% | PASS |
| mem_reads | `oltp_distinct_range` | 4.79ms | 5.03ms | 1.048× | 0.86% | PASS |
| mem_reads | `oltp_index_scan` | 4.76ms | 6.64ms | 1.396× | 1.57% | PASS |
| mem_reads | `select_random_points` | 29.22ms | 33.04ms | 1.131× | 1.47% | PASS |
| mem_reads | `select_random_ranges` | 7.97ms | 9.31ms | 1.168× | 1.19% | PASS |
| mem_reads | `covering_index_scan` | 4.28ms | 4.68ms | 1.094× | 2.58% | PASS |
| mem_reads | `groupby_scan` | 37.02ms | 39.58ms | 1.069× | 0.76% | PASS |
| mem_reads | `index_join` | 8.40ms | 11.35ms | 1.352× | 1.83% | PASS |
| mem_reads | `index_join_scan` | 4.44ms | 6.01ms | 1.354× | 2.65% | PASS |
| mem_reads | `types_table_scan` | 1.15s | 1.28s | 1.113× | 1.14% | PASS |
| mem_reads | `table_scan` | 1.42s | 1.45s | 1.019× | 0.67% | PASS |
| mem_reads | `oltp_read_only` | 162.50ms | 177.02ms | 1.089× | 1.08% | PASS |
| mem_writes | `oltp_bulk_insert` | 250.97ms | 369.38ms | 1.472× | 0.87% | PASS |
| mem_writes | `oltp_insert` | 20.04ms | 38.12ms | 1.902× | 1.14% | PASS |
| mem_writes | `oltp_update_index` | 75.27ms | 130.82ms | 1.738× | 1.58% | PASS |
| mem_writes | `oltp_update_non_index` | 54.50ms | 89.27ms | 1.638× | 1.63% | PASS |
| mem_writes | `oltp_delete_insert` | 53.99ms | 103.24ms | 1.912× | 1.50% | PASS |
| mem_writes | `oltp_write_only` | 29.02ms | 61.05ms | 2.104× | 1.46% | PASS |
| mem_writes | `types_delete_insert` | 34.86ms | 57.90ms | 1.661× | 1.69% | PASS |
| mem_writes | `oltp_read_write` | 114.06ms | 166.66ms | 1.461× | 1.85% | PASS |
| file_reads | `oltp_point_select` | 111.84ms | 68.44ms | 0.612× | 1.22% | PASS |
| file_reads | `oltp_range_select` | 27.38ms | 24.54ms | 0.896× | 1.72% | PASS |
| file_reads | `oltp_sum_range` | 26.21ms | 24.37ms | 0.930× | 1.74% | PASS |
| file_reads | `oltp_order_range` | 4.62ms | 4.37ms | 0.946× | 1.23% | PASS |
| file_reads | `oltp_distinct_range` | 5.87ms | 5.46ms | 0.930× | 1.57% | PASS |
| file_reads | `oltp_index_scan` | 13.10ms | 9.66ms | 0.737× | 1.78% | PASS |
| file_reads | `select_random_points` | 39.17ms | 37.31ms | 0.953× | 1.69% | PASS |
| file_reads | `select_random_ranges` | 15.98ms | 12.36ms | 0.774× | 1.16% | PASS |
| file_reads | `covering_index_scan` | 12.20ms | 7.36ms | 0.604× | 1.69% | PASS |
| file_reads | `groupby_scan` | 38.01ms | 40.24ms | 1.059× | 1.05% | PASS |
| file_reads | `index_join` | 12.92ms | 13.27ms | 1.027× | 1.93% | PASS |
| file_reads | `index_join_scan` | 5.39ms | 6.28ms | 1.164× | 2.35% | PASS |
| file_reads | `types_table_scan` | 1.14s | 1.26s | 1.106× | 1.08% | PASS |
| file_reads | `table_scan` | 1.41s | 1.43s | 1.016× | 0.69% | PASS |
| file_reads | `oltp_read_only` | 277.09ms | 217.63ms | 0.785× | 0.88% | PASS |
| file_writes | `oltp_bulk_insert` | 266.02ms | 382.82ms | 1.439× | 1.12% | PASS |
| file_writes | `oltp_insert` | 26.99ms | 47.29ms | 1.752× | 1.96% | PASS |
| file_writes | `oltp_update_index` | 101.96ms | 149.88ms | 1.470× | 1.30% | PASS |
| file_writes | `oltp_update_non_index` | 79.71ms | 107.67ms | 1.351× | 1.79% | PASS |
| file_writes | `oltp_delete_insert` | 79.49ms | 123.80ms | 1.557× | 1.77% | PASS |
| file_writes | `oltp_write_only` | 53.56ms | 81.22ms | 1.516× | 1.93% | PASS |
| file_writes | `types_delete_insert` | 52.62ms | 71.81ms | 1.365× | 1.73% | PASS |
| file_writes | `oltp_read_write` | 140.98ms | 186.70ms | 1.324× | 1.83% | PASS |
| ac_reads | `oltp_point_select` | 60.12ms | 68.79ms | 1.144× | 1.09% | PASS |
| ac_reads | `oltp_range_select` | 23.01ms | 24.81ms | 1.079× | 1.96% | PASS |
| ac_reads | `oltp_sum_range` | 21.45ms | 24.44ms | 1.139× | 1.29% | PASS |
| ac_reads | `oltp_order_range` | 3.98ms | 4.34ms | 1.091× | 1.71% | PASS |
| ac_reads | `oltp_distinct_range` | 5.15ms | 5.40ms | 1.049× | 0.98% | PASS |
| ac_reads | `oltp_index_scan` | 7.58ms | 9.40ms | 1.240× | 1.38% | PASS |
| ac_reads | `select_random_points` | 31.94ms | 36.62ms | 1.146× | 1.24% | PASS |
| ac_reads | `select_random_ranges` | 10.46ms | 12.29ms | 1.174× | 1.16% | PASS |
| ac_reads | `covering_index_scan` | 7.02ms | 7.31ms | 1.041× | 1.84% | PASS |
| ac_reads | `groupby_scan` | 37.21ms | 40.01ms | 1.075× | 0.70% | PASS |
| ac_reads | `index_join` | 9.99ms | 12.97ms | 1.298× | 1.55% | PASS |
| ac_reads | `index_join_scan` | 4.72ms | 6.01ms | 1.272× | 1.97% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.24s | 1.183× | 1.01% | PASS |
| ac_reads | `table_scan` | 1.19s | 1.39s | 1.167× | 0.82% | PASS |
| ac_reads | `oltp_read_only` | 187.64ms | 211.43ms | 1.127× | 0.97% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.73ms | 66.98ms | 2.947× | 6.94% | PASS |
| ac_writes | `oltp_insert_ac` | 25.23ms | 89.03ms | 3.529× | 6.07% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.88ms | 101.58ms | 3.644× | 5.68% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.12ms | 79.55ms | 3.167× | 7.23% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.86ms | 90.48ms | 3.639× | 6.99% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.49ms | 89.05ms | 3.494× | 5.37% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.86ms | 80.75ms | 3.533× | 8.19% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.29ms | 98.77ms | 3.059× | 5.99% | PASS |

</details>

## Version-control latency

Wall time: 2m 24s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 85.02ms | 200.00ms | 42.5% | 0.58% | PASS |
| `status_dirty_many_tables` | 87.53ms | 200.00ms | 43.8% | 0.50% | PASS |
| `diff_regular_working_one_table` | 79.65ms | 150.00ms | 53.1% | 0.43% | PASS |
| `diff_regular_working_many_tables` | 93.58ms | 200.00ms | 46.8% | 0.68% | PASS |
| `diff_stat_working_many_tables` | 93.54ms | 200.00ms | 46.8% | 0.70% | PASS |
| `diff_schema_working_many_tables` | 93.72ms | 200.00ms | 46.9% | 0.71% | PASS |
| `branch_list_many_branches` | 24.00ms | 100.00ms | 24.0% | 1.50% | PASS |
| `branch_create_delete` | 26.20ms | 100.00ms | 26.2% | 1.56% | PASS |
| `checkout_branch_clean` | 56.97ms | 200.00ms | 28.5% | 0.89% | PASS |
| `merge_data_no_conflicts` | 30.12ms | 150.00ms | 20.1% | 1.02% | PASS |
| `merge_schema_no_conflicts` | 23.12ms | 100.00ms | 23.1% | 1.63% | PASS |
| `merge_data_conflicts` | 128.43ms | 250.00ms | 51.4% | 0.29% | PASS |
| `merge_data_conflicts_with_resolve` | 128.13ms | 250.00ms | 51.3% | 0.33% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
