# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-28 19:24 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260920.314.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/36460520245)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 53s | 9.24s | 11.57s | 1.253× | 1.83% | **PASS** |
| textpk | 69 | 55 | 1h 34m 19s | 10.60s | 11.95s | 1.127× | 2.08% | **PASS** |
| blobpk | 69 | 55 | 1h 31m 45s | 9.99s | 11.81s | 1.183× | 1.99% | **PASS** |
| compositepk | 69 | 55 | 1h 2m 1s | 7.11s | 7.87s | 1.107× | 2.33% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.14ms | 29.53ms | 1.175× | 2.43% | PASS |
| mem_reads | `oltp_range_select` | 10.89ms | 13.38ms | 1.229× | 3.36% | PASS |
| mem_reads | `oltp_sum_range` | 10.31ms | 12.93ms | 1.254× | 3.30% | PASS |
| mem_reads | `oltp_order_range` | 2.71ms | 3.10ms | 1.143× | 1.41% | PASS |
| mem_reads | `oltp_distinct_range` | 3.70ms | 4.09ms | 1.106× | 1.38% | PASS |
| mem_reads | `oltp_index_scan` | 4.02ms | 5.51ms | 1.371× | 2.40% | PASS |
| mem_reads | `select_random_points` | 11.71ms | 11.68ms | 0.997× | 3.99% | PASS |
| mem_reads | `select_random_ranges` | 3.15ms | 4.08ms | 1.296× | 1.79% | PASS |
| mem_reads | `covering_index_scan` | 4.27ms | 4.39ms | 1.028× | 2.35% | PASS |
| mem_reads | `groupby_scan` | 30.15ms | 33.59ms | 1.114× | 0.91% | PASS |
| mem_reads | `index_join` | 6.08ms | 8.46ms | 1.391× | 2.22% | PASS |
| mem_reads | `index_join_scan` | 3.62ms | 4.65ms | 1.283× | 3.12% | PASS |
| mem_reads | `types_table_scan` | 1.10s | 1.36s | 1.233× | 0.88% | PASS |
| mem_reads | `table_scan` | 1.27s | 1.43s | 1.123× | 0.65% | PASS |
| mem_reads | `oltp_read_only` | 109.50ms | 126.42ms | 1.155× | 1.23% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.42ms | 255.85ms | 1.410× | 0.91% | PASS |
| mem_writes | `oltp_insert` | 15.76ms | 29.07ms | 1.845× | 1.73% | PASS |
| mem_writes | `oltp_update_index` | 54.14ms | 112.98ms | 2.087× | 1.65% | PASS |
| mem_writes | `oltp_update_non_index` | 35.67ms | 61.65ms | 1.728× | 2.27% | PASS |
| mem_writes | `oltp_delete_insert` | 46.73ms | 81.45ms | 1.743× | 1.27% | PASS |
| mem_writes | `oltp_write_only` | 22.81ms | 46.60ms | 2.043× | 2.08% | PASS |
| mem_writes | `types_delete_insert` | 25.83ms | 41.43ms | 1.604× | 1.94% | PASS |
| mem_writes | `oltp_read_write` | 71.08ms | 113.46ms | 1.596× | 2.36% | PASS |
| file_reads | `oltp_point_select` | 100.06ms | 55.77ms | 0.557× | 0.61% | PASS |
| file_reads | `oltp_range_select` | 18.86ms | 15.99ms | 0.848× | 2.11% | PASS |
| file_reads | `oltp_sum_range` | 18.08ms | 15.30ms | 0.846× | 1.73% | PASS |
| file_reads | `oltp_order_range` | 3.51ms | 3.35ms | 0.955× | 2.30% | PASS |
| file_reads | `oltp_distinct_range` | 4.57ms | 4.42ms | 0.966× | 1.40% | PASS |
| file_reads | `oltp_index_scan` | 12.12ms | 8.60ms | 0.709× | 1.83% | PASS |
| file_reads | `select_random_points` | 19.52ms | 14.38ms | 0.736× | 2.16% | PASS |
| file_reads | `select_random_ranges` | 10.74ms | 6.71ms | 0.625× | 1.20% | PASS |
| file_reads | `covering_index_scan` | 12.06ms | 7.19ms | 0.597× | 1.73% | PASS |
| file_reads | `groupby_scan` | 31.31ms | 34.12ms | 1.090× | 1.05% | PASS |
| file_reads | `index_join` | 10.49ms | 10.44ms | 0.995× | 2.30% | PASS |
| file_reads | `index_join_scan` | 4.69ms | 5.24ms | 1.117× | 3.04% | PASS |
| file_reads | `types_table_scan` | 1.09s | 1.35s | 1.238× | 1.83% | PASS |
| file_reads | `table_scan` | 1.24s | 1.42s | 1.140× | 1.49% | PASS |
| file_reads | `oltp_read_only` | 216.84ms | 162.77ms | 0.751× | 0.95% | PASS |
| file_writes | `oltp_bulk_insert` | 193.83ms | 274.22ms | 1.415× | 1.46% | PASS |
| file_writes | `oltp_insert` | 22.29ms | 35.87ms | 1.609× | 1.88% | PASS |
| file_writes | `oltp_update_index` | 79.25ms | 130.33ms | 1.644× | 1.45% | PASS |
| file_writes | `oltp_update_non_index` | 59.46ms | 83.14ms | 1.398× | 1.84% | PASS |
| file_writes | `oltp_delete_insert` | 69.04ms | 100.83ms | 1.460× | 1.69% | PASS |
| file_writes | `oltp_write_only` | 45.01ms | 64.71ms | 1.438× | 1.95% | PASS |
| file_writes | `types_delete_insert` | 41.16ms | 54.03ms | 1.313× | 2.47% | PASS |
| file_writes | `oltp_read_write` | 95.67ms | 132.81ms | 1.388× | 2.04% | PASS |
| ac_reads | `oltp_point_select` | 50.31ms | 55.97ms | 1.112× | 1.09% | PASS |
| ac_reads | `oltp_range_select` | 13.77ms | 15.97ms | 1.160× | 1.82% | PASS |
| ac_reads | `oltp_sum_range` | 13.05ms | 15.35ms | 1.176× | 1.71% | PASS |
| ac_reads | `oltp_order_range` | 2.99ms | 3.34ms | 1.117× | 1.31% | PASS |
| ac_reads | `oltp_distinct_range` | 4.05ms | 4.42ms | 1.091× | 1.83% | PASS |
| ac_reads | `oltp_index_scan` | 6.91ms | 8.42ms | 1.219× | 1.40% | PASS |
| ac_reads | `select_random_points` | 14.14ms | 14.30ms | 1.011× | 3.56% | PASS |
| ac_reads | `select_random_ranges` | 5.71ms | 6.68ms | 1.170× | 1.05% | PASS |
| ac_reads | `covering_index_scan` | 7.25ms | 7.18ms | 0.991× | 1.34% | PASS |
| ac_reads | `groupby_scan` | 30.36ms | 33.98ms | 1.119× | 0.81% | PASS |
| ac_reads | `index_join` | 8.04ms | 10.45ms | 1.300× | 2.01% | PASS |
| ac_reads | `index_join_scan` | 4.17ms | 5.21ms | 1.248× | 3.42% | PASS |
| ac_reads | `types_table_scan` | 1.08s | 1.35s | 1.250× | 1.61% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.41s | 1.176× | 1.39% | PASS |
| ac_reads | `oltp_read_only` | 141.05ms | 161.92ms | 1.148× | 1.09% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.68ms | 68.95ms | 3.040× | 6.10% | PASS |
| ac_writes | `oltp_insert_ac` | 25.52ms | 86.17ms | 3.377× | 6.19% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.56ms | 101.76ms | 3.563× | 6.99% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.39ms | 80.09ms | 3.423× | 7.05% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.42ms | 89.65ms | 3.671× | 4.70% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.87ms | 89.23ms | 3.588× | 4.22% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.79ms | 80.44ms | 3.530× | 6.30% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.94ms | 95.90ms | 3.100× | 4.66% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.43ms | 38.79ms | 1.234× | 1.90% | PASS |
| mem_reads | `oltp_range_select` | 13.93ms | 14.24ms | 1.023× | 2.88% | PASS |
| mem_reads | `oltp_sum_range` | 12.72ms | 14.61ms | 1.148× | 2.38% | PASS |
| mem_reads | `oltp_order_range` | 3.04ms | 3.19ms | 1.049× | 1.63% | PASS |
| mem_reads | `oltp_distinct_range` | 4.10ms | 4.29ms | 1.044× | 1.08% | PASS |
| mem_reads | `oltp_index_scan` | 4.62ms | 6.44ms | 1.396× | 1.79% | PASS |
| mem_reads | `select_random_points` | 18.10ms | 21.23ms | 1.173× | 2.55% | PASS |
| mem_reads | `select_random_ranges` | 4.04ms | 5.24ms | 1.296× | 2.21% | PASS |
| mem_reads | `covering_index_scan` | 4.54ms | 4.51ms | 0.993× | 2.23% | PASS |
| mem_reads | `groupby_scan` | 31.94ms | 34.40ms | 1.077× | 1.14% | PASS |
| mem_reads | `index_join` | 6.92ms | 9.34ms | 1.349× | 1.76% | PASS |
| mem_reads | `index_join_scan` | 4.91ms | 5.49ms | 1.118× | 4.75% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.24s | 1.159× | 0.93% | PASS |
| mem_reads | `table_scan` | 1.32s | 1.39s | 1.051× | 2.73% | PASS |
| mem_reads | `oltp_read_only` | 121.16ms | 138.02ms | 1.139× | 1.78% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.95ms | 357.28ms | 1.514× | 0.75% | PASS |
| mem_writes | `oltp_insert` | 22.68ms | 40.11ms | 1.768× | 1.80% | PASS |
| mem_writes | `oltp_update_index` | 74.61ms | 136.64ms | 1.832× | 1.58% | PASS |
| mem_writes | `oltp_update_non_index` | 50.01ms | 88.09ms | 1.761× | 1.53% | PASS |
| mem_writes | `oltp_delete_insert` | 51.69ms | 104.77ms | 2.027× | 1.32% | PASS |
| mem_writes | `oltp_write_only` | 29.24ms | 61.98ms | 2.119× | 1.25% | PASS |
| mem_writes | `types_delete_insert` | 33.63ms | 55.35ms | 1.646× | 1.90% | PASS |
| mem_writes | `oltp_read_write` | 90.89ms | 142.97ms | 1.573× | 2.20% | PASS |
| file_reads | `oltp_point_select` | 106.32ms | 64.38ms | 0.606× | 1.16% | PASS |
| file_reads | `oltp_range_select` | 22.02ms | 16.96ms | 0.771× | 2.43% | PASS |
| file_reads | `oltp_sum_range` | 20.89ms | 17.35ms | 0.831× | 2.30% | PASS |
| file_reads | `oltp_order_range` | 3.90ms | 3.52ms | 0.902× | 1.86% | PASS |
| file_reads | `oltp_distinct_range` | 5.04ms | 4.63ms | 0.920× | 1.19% | PASS |
| file_reads | `oltp_index_scan` | 12.45ms | 9.33ms | 0.749× | 1.38% | PASS |
| file_reads | `select_random_points` | 27.74ms | 24.83ms | 0.895× | 2.17% | PASS |
| file_reads | `select_random_ranges` | 11.80ms | 7.95ms | 0.674× | 0.98% | PASS |
| file_reads | `covering_index_scan` | 13.24ms | 7.49ms | 0.566× | 2.73% | PASS |
| file_reads | `groupby_scan` | 33.51ms | 34.92ms | 1.042× | 1.56% | PASS |
| file_reads | `index_join` | 11.42ms | 11.52ms | 1.008× | 2.35% | PASS |
| file_reads | `index_join_scan` | 5.84ms | 5.98ms | 1.023× | 2.08% | PASS |
| file_reads | `types_table_scan` | 1.15s | 1.25s | 1.086× | 3.23% | PASS |
| file_reads | `table_scan` | 1.50s | 1.40s | 0.933× | 2.48% | PASS |
| file_reads | `oltp_read_only` | 231.54ms | 176.78ms | 0.763× | 1.42% | PASS |
| file_writes | `oltp_bulk_insert` | 256.18ms | 389.22ms | 1.519× | 0.89% | PASS |
| file_writes | `oltp_insert` | 48.55ms | 53.54ms | 1.103× | 18.28% | PASS |
| file_writes | `oltp_update_index` | 123.12ms | 176.82ms | 1.436× | 1.51% | PASS |
| file_writes | `oltp_update_non_index` | 116.83ms | 117.88ms | 1.009× | 7.55% | PASS |
| file_writes | `oltp_delete_insert` | 98.51ms | 140.06ms | 1.422× | 1.95% | PASS |
| file_writes | `oltp_write_only` | 81.16ms | 89.08ms | 1.098× | 11.37% | PASS |
| file_writes | `types_delete_insert` | 58.92ms | 77.72ms | 1.319× | 1.58% | PASS |
| file_writes | `oltp_read_write` | 166.27ms | 171.66ms | 1.032× | 5.75% | PASS |
| ac_reads | `oltp_point_select` | 58.21ms | 66.00ms | 1.134× | 0.90% | PASS |
| ac_reads | `oltp_range_select` | 18.32ms | 17.42ms | 0.951× | 2.33% | PASS |
| ac_reads | `oltp_sum_range` | 16.72ms | 17.81ms | 1.065× | 2.31% | PASS |
| ac_reads | `oltp_order_range` | 3.52ms | 3.57ms | 1.014× | 1.61% | PASS |
| ac_reads | `oltp_distinct_range` | 4.59ms | 4.71ms | 1.027× | 1.35% | PASS |
| ac_reads | `oltp_index_scan` | 7.78ms | 9.41ms | 1.210× | 1.15% | PASS |
| ac_reads | `select_random_points` | 22.87ms | 25.38ms | 1.110× | 2.69% | PASS |
| ac_reads | `select_random_ranges` | 6.92ms | 8.00ms | 1.157× | 2.26% | PASS |
| ac_reads | `covering_index_scan` | 8.12ms | 7.54ms | 0.929× | 4.49% | PASS |
| ac_reads | `groupby_scan` | 33.05ms | 35.16ms | 1.064× | 0.76% | PASS |
| ac_reads | `index_join` | 10.14ms | 12.01ms | 1.184× | 3.65% | PASS |
| ac_reads | `index_join_scan` | 5.58ms | 6.13ms | 1.099× | 4.52% | PASS |
| ac_reads | `types_table_scan` | 1.13s | 1.25s | 1.106× | 2.04% | PASS |
| ac_reads | `table_scan` | 1.53s | 1.42s | 0.927× | 1.31% | PASS |
| ac_reads | `oltp_read_only` | 168.38ms | 182.33ms | 1.083× | 0.97% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.14ms | 68.86ms | 2.976× | 4.17% | PASS |
| ac_writes | `oltp_insert_ac` | 28.38ms | 84.81ms | 2.989× | 5.42% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.35ms | 102.83ms | 3.504× | 6.10% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.69ms | 81.92ms | 3.318× | 6.22% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 27.17ms | 95.76ms | 3.525× | 8.05% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.25ms | 91.85ms | 3.371× | 8.08% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.82ms | 81.60ms | 3.426× | 5.35% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.74ms | 99.28ms | 2.943× | 5.80% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.87ms | 36.85ms | 1.234× | 1.58% | PASS |
| mem_reads | `oltp_range_select` | 12.52ms | 13.86ms | 1.107× | 2.61% | PASS |
| mem_reads | `oltp_sum_range` | 11.70ms | 13.79ms | 1.178× | 2.08% | PASS |
| mem_reads | `oltp_order_range` | 2.86ms | 3.12ms | 1.090× | 1.45% | PASS |
| mem_reads | `oltp_distinct_range` | 3.98ms | 4.21ms | 1.057× | 1.38% | PASS |
| mem_reads | `oltp_index_scan` | 4.59ms | 6.26ms | 1.365× | 2.40% | PASS |
| mem_reads | `select_random_points` | 17.83ms | 20.75ms | 1.163× | 2.11% | PASS |
| mem_reads | `select_random_ranges` | 4.26ms | 5.21ms | 1.224× | 1.99% | PASS |
| mem_reads | `covering_index_scan` | 4.44ms | 4.58ms | 1.033× | 2.42% | PASS |
| mem_reads | `groupby_scan` | 32.04ms | 34.54ms | 1.078× | 0.81% | PASS |
| mem_reads | `index_join` | 6.93ms | 9.50ms | 1.371× | 3.04% | PASS |
| mem_reads | `index_join_scan` | 4.43ms | 5.30ms | 1.196× | 4.18% | PASS |
| mem_reads | `types_table_scan` | 1.09s | 1.25s | 1.147× | 2.69% | PASS |
| mem_reads | `table_scan` | 1.33s | 1.40s | 1.047× | 5.78% | PASS |
| mem_reads | `oltp_read_only` | 116.65ms | 135.16ms | 1.159× | 1.25% | PASS |
| mem_writes | `oltp_bulk_insert` | 243.85ms | 352.41ms | 1.445× | 0.93% | PASS |
| mem_writes | `oltp_insert` | 19.99ms | 39.15ms | 1.959× | 0.73% | PASS |
| mem_writes | `oltp_update_index` | 69.31ms | 131.29ms | 1.894× | 1.69% | PASS |
| mem_writes | `oltp_update_non_index` | 50.21ms | 85.88ms | 1.710× | 1.52% | PASS |
| mem_writes | `oltp_delete_insert` | 51.77ms | 106.27ms | 2.053× | 1.37% | PASS |
| mem_writes | `oltp_write_only` | 28.45ms | 62.02ms | 2.180× | 1.77% | PASS |
| mem_writes | `types_delete_insert` | 32.92ms | 54.07ms | 1.643× | 1.85% | PASS |
| mem_writes | `oltp_read_write` | 82.90ms | 137.70ms | 1.661× | 1.66% | PASS |
| file_reads | `oltp_point_select` | 106.24ms | 63.65ms | 0.599× | 1.06% | PASS |
| file_reads | `oltp_range_select` | 20.57ms | 16.83ms | 0.818× | 2.81% | PASS |
| file_reads | `oltp_sum_range` | 20.04ms | 16.83ms | 0.840× | 1.76% | PASS |
| file_reads | `oltp_order_range` | 3.76ms | 3.51ms | 0.934× | 2.19% | PASS |
| file_reads | `oltp_distinct_range` | 4.89ms | 4.61ms | 0.941× | 1.55% | PASS |
| file_reads | `oltp_index_scan` | 12.49ms | 9.26ms | 0.741× | 1.54% | PASS |
| file_reads | `select_random_points` | 27.87ms | 24.51ms | 0.879× | 2.72% | PASS |
| file_reads | `select_random_ranges` | 11.99ms | 7.86ms | 0.656× | 1.33% | PASS |
| file_reads | `covering_index_scan` | 12.38ms | 7.36ms | 0.594× | 1.34% | PASS |
| file_reads | `groupby_scan` | 33.31ms | 35.01ms | 1.051× | 1.01% | PASS |
| file_reads | `index_join` | 11.54ms | 11.31ms | 0.980× | 2.30% | PASS |
| file_reads | `index_join_scan` | 5.39ms | 5.77ms | 1.069× | 3.06% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.23s | 1.156× | 1.46% | PASS |
| file_reads | `table_scan` | 1.28s | 1.38s | 1.079× | 3.00% | PASS |
| file_reads | `oltp_read_only` | 239.70ms | 176.68ms | 0.737× | 1.48% | PASS |
| file_writes | `oltp_bulk_insert` | 263.20ms | 380.07ms | 1.444× | 1.12% | PASS |
| file_writes | `oltp_insert` | 32.26ms | 52.43ms | 1.625× | 1.82% | PASS |
| file_writes | `oltp_update_index` | 106.56ms | 165.11ms | 1.549× | 2.00% | PASS |
| file_writes | `oltp_update_non_index` | 80.82ms | 109.56ms | 1.356× | 1.33% | PASS |
| file_writes | `oltp_delete_insert` | 82.92ms | 131.16ms | 1.582× | 1.18% | PASS |
| file_writes | `oltp_write_only` | 57.44ms | 85.05ms | 1.481× | 2.71% | PASS |
| file_writes | `types_delete_insert` | 53.56ms | 72.86ms | 1.360× | 2.15% | PASS |
| file_writes | `oltp_read_write` | 120.03ms | 165.03ms | 1.375× | 1.63% | PASS |
| ac_reads | `oltp_point_select` | 56.10ms | 63.19ms | 1.127× | 1.18% | PASS |
| ac_reads | `oltp_range_select` | 16.49ms | 16.93ms | 1.026× | 2.50% | PASS |
| ac_reads | `oltp_sum_range` | 15.63ms | 16.82ms | 1.076× | 2.35% | PASS |
| ac_reads | `oltp_order_range` | 3.35ms | 3.50ms | 1.044× | 1.52% | PASS |
| ac_reads | `oltp_distinct_range` | 4.49ms | 4.60ms | 1.024× | 1.86% | PASS |
| ac_reads | `oltp_index_scan` | 7.75ms | 9.24ms | 1.192× | 1.82% | PASS |
| ac_reads | `select_random_points` | 22.52ms | 24.63ms | 1.094× | 3.13% | PASS |
| ac_reads | `select_random_ranges` | 7.00ms | 7.83ms | 1.119× | 1.12% | PASS |
| ac_reads | `covering_index_scan` | 8.03ms | 7.35ms | 0.915× | 2.38% | PASS |
| ac_reads | `groupby_scan` | 32.63ms | 35.02ms | 1.073× | 0.71% | PASS |
| ac_reads | `index_join` | 9.55ms | 11.38ms | 1.192× | 2.02% | PASS |
| ac_reads | `index_join_scan` | 4.95ms | 5.75ms | 1.162× | 2.66% | PASS |
| ac_reads | `types_table_scan` | 1.12s | 1.25s | 1.110× | 2.16% | PASS |
| ac_reads | `table_scan` | 1.40s | 1.40s | 1.001× | 4.25% | PASS |
| ac_reads | `oltp_read_only` | 162.74ms | 177.96ms | 1.094× | 1.36% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.22ms | 69.57ms | 2.873× | 6.73% | PASS |
| ac_writes | `oltp_insert_ac` | 25.32ms | 93.26ms | 3.684× | 6.11% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.93ms | 105.06ms | 3.762× | 5.30% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.38ms | 80.95ms | 3.462× | 5.08% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.54ms | 93.13ms | 3.795× | 5.57% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.30ms | 92.41ms | 3.653× | 4.73% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.09ms | 83.97ms | 3.802× | 8.19% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.88ms | 100.19ms | 3.047× | 6.97% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.46ms | 23.19ms | 1.134× | 3.04% | PASS |
| mem_reads | `oltp_range_select` | 10.68ms | 11.24ms | 1.052× | 3.66% | PASS |
| mem_reads | `oltp_sum_range` | 10.00ms | 11.16ms | 1.116× | 2.71% | PASS |
| mem_reads | `oltp_order_range` | 2.26ms | 2.23ms | 0.985× | 2.39% | PASS |
| mem_reads | `oltp_distinct_range` | 2.57ms | 2.68ms | 1.043× | 2.05% | PASS |
| mem_reads | `oltp_index_scan` | 2.70ms | 3.56ms | 1.320× | 3.15% | PASS |
| mem_reads | `select_random_points` | 16.69ms | 19.29ms | 1.155× | 3.55% | PASS |
| mem_reads | `select_random_ranges` | 4.37ms | 5.11ms | 1.170× | 4.95% | PASS |
| mem_reads | `covering_index_scan` | 2.49ms | 2.53ms | 1.016× | 1.80% | PASS |
| mem_reads | `groupby_scan` | 20.51ms | 22.05ms | 1.075× | 1.14% | PASS |
| mem_reads | `index_join` | 4.93ms | 6.05ms | 1.226× | 0.77% | PASS |
| mem_reads | `index_join_scan` | 2.58ms | 4.19ms | 1.623× | 7.62% | PASS |
| mem_reads | `types_table_scan` | 720.83ms | 777.31ms | 1.078× | 0.90% | PASS |
| mem_reads | `table_scan` | 814.46ms | 862.08ms | 1.058× | 1.16% | PASS |
| mem_reads | `oltp_read_only` | 85.14ms | 89.58ms | 1.052× | 2.67% | PASS |
| mem_writes | `oltp_bulk_insert` | 149.60ms | 206.47ms | 1.380× | 1.02% | PASS |
| mem_writes | `oltp_insert` | 11.21ms | 22.02ms | 1.964× | 2.11% | PASS |
| mem_writes | `oltp_update_index` | 40.15ms | 73.61ms | 1.833× | 1.92% | PASS |
| mem_writes | `oltp_update_non_index` | 31.30ms | 53.82ms | 1.719× | 1.95% | PASS |
| mem_writes | `oltp_delete_insert` | 29.97ms | 59.60ms | 1.989× | 1.63% | PASS |
| mem_writes | `oltp_write_only` | 16.78ms | 38.30ms | 2.283× | 1.87% | PASS |
| mem_writes | `types_delete_insert` | 19.61ms | 34.27ms | 1.748× | 2.54% | PASS |
| mem_writes | `oltp_read_write` | 54.59ms | 85.37ms | 1.564× | 2.24% | PASS |
| file_reads | `oltp_point_select` | 77.39ms | 43.16ms | 0.558× | 1.47% | PASS |
| file_reads | `oltp_range_select` | 17.14ms | 13.40ms | 0.782× | 1.69% | PASS |
| file_reads | `oltp_sum_range` | 16.18ms | 13.15ms | 0.813× | 1.78% | PASS |
| file_reads | `oltp_order_range` | 2.89ms | 2.40ms | 0.831× | 1.67% | PASS |
| file_reads | `oltp_distinct_range` | 3.48ms | 2.96ms | 0.850× | 0.88% | PASS |
| file_reads | `oltp_index_scan` | 9.08ms | 5.81ms | 0.640× | 0.95% | PASS |
| file_reads | `select_random_points` | 23.47ms | 20.86ms | 0.889× | 1.38% | PASS |
| file_reads | `select_random_ranges` | 10.63ms | 7.32ms | 0.689× | 1.05% | PASS |
| file_reads | `covering_index_scan` | 8.82ms | 4.70ms | 0.532× | 1.32% | PASS |
| file_reads | `groupby_scan` | 21.78ms | 21.99ms | 1.010× | 1.09% | PASS |
| file_reads | `index_join` | 8.31ms | 7.48ms | 0.900× | 2.57% | PASS |
| file_reads | `index_join_scan` | 3.58ms | 4.40ms | 1.226× | 3.55% | PASS |
| file_reads | `types_table_scan` | 704.89ms | 761.11ms | 1.080× | 1.07% | PASS |
| file_reads | `table_scan` | 813.16ms | 851.78ms | 1.047× | 1.36% | PASS |
| file_reads | `oltp_read_only` | 167.00ms | 117.11ms | 0.701× | 2.60% | PASS |
| file_writes | `oltp_bulk_insert` | 200.02ms | 269.05ms | 1.345× | 4.31% | PASS |
| file_writes | `oltp_insert` | 20.20ms | 38.66ms | 1.913× | 16.39% | PASS |
| file_writes | `oltp_update_index` | 141.22ms | 148.82ms | 1.054× | 3.96% | PASS |
| file_writes | `oltp_update_non_index` | 121.01ms | 109.01ms | 0.901× | 1.55% | PASS |
| file_writes | `oltp_delete_insert` | 120.96ms | 119.05ms | 0.984× | 3.83% | PASS |
| file_writes | `oltp_write_only` | 83.74ms | 88.91ms | 1.062× | 5.17% | PASS |
| file_writes | `types_delete_insert` | 78.05ms | 71.39ms | 0.915× | 6.10% | PASS |
| file_writes | `oltp_read_write` | 130.60ms | 141.41ms | 1.083× | 4.54% | PASS |
| ac_reads | `oltp_point_select` | 39.06ms | 43.11ms | 1.104× | 2.23% | PASS |
| ac_reads | `oltp_range_select` | 13.77ms | 13.63ms | 0.990× | 2.51% | PASS |
| ac_reads | `oltp_sum_range` | 12.71ms | 13.39ms | 1.054× | 2.33% | PASS |
| ac_reads | `oltp_order_range` | 2.67ms | 2.49ms | 0.934× | 1.34% | PASS |
| ac_reads | `oltp_distinct_range` | 3.19ms | 2.98ms | 0.935× | 1.29% | PASS |
| ac_reads | `oltp_index_scan` | 5.38ms | 6.25ms | 1.163× | 2.97% | PASS |
| ac_reads | `select_random_points` | 17.90ms | 20.33ms | 1.136× | 2.41% | PASS |
| ac_reads | `select_random_ranges` | 6.33ms | 7.07ms | 1.116× | 1.45% | PASS |
| ac_reads | `covering_index_scan` | 4.28ms | 4.53ms | 1.059× | 2.00% | PASS |
| ac_reads | `groupby_scan` | 20.85ms | 21.66ms | 1.039× | 1.72% | PASS |
| ac_reads | `index_join` | 6.09ms | 7.51ms | 1.232× | 2.46% | PASS |
| ac_reads | `index_join_scan` | 3.01ms | 4.27ms | 1.418× | 1.93% | PASS |
| ac_reads | `types_table_scan` | 728.14ms | 767.42ms | 1.054× | 3.95% | PASS |
| ac_reads | `table_scan` | 1.07s | 926.63ms | 0.867× | 2.76% | PASS |
| ac_reads | `oltp_read_only` | 107.06ms | 115.42ms | 1.078× | 0.71% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.40ms | 58.28ms | 2.602× | 4.16% | PASS |
| ac_writes | `oltp_insert_ac` | 26.80ms | 75.20ms | 2.806× | 2.82% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.79ms | 83.77ms | 2.910× | 3.82% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.93ms | 68.54ms | 2.989× | 3.18% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.76ms | 87.30ms | 3.262× | 5.55% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.18ms | 86.18ms | 3.170× | 4.98% | PASS |
| ac_writes | `types_delete_insert_ac` | 27.60ms | 74.69ms | 2.706× | 12.27% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.52ms | 102.75ms | 3.065× | 5.23% | PASS |

</details>

## Version-control latency

Wall time: 2m 21s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.71ms | 200.00ms | 41.9% | 0.37% | PASS |
| `status_dirty_many_tables` | 87.00ms | 200.00ms | 43.5% | 0.51% | PASS |
| `diff_regular_working_one_table` | 79.45ms | 150.00ms | 53.0% | 0.45% | PASS |
| `diff_regular_working_many_tables` | 92.36ms | 200.00ms | 46.2% | 0.58% | PASS |
| `diff_stat_working_many_tables` | 92.80ms | 200.00ms | 46.4% | 0.58% | PASS |
| `diff_schema_working_many_tables` | 92.64ms | 200.00ms | 46.3% | 0.50% | PASS |
| `branch_list_many_branches` | 23.37ms | 100.00ms | 23.4% | 1.99% | PASS |
| `branch_create_delete` | 25.35ms | 100.00ms | 25.4% | 1.59% | PASS |
| `checkout_branch_clean` | 56.50ms | 200.00ms | 28.2% | 0.82% | PASS |
| `merge_data_no_conflicts` | 30.13ms | 150.00ms | 20.1% | 1.59% | PASS |
| `merge_schema_no_conflicts` | 22.96ms | 100.00ms | 23.0% | 1.62% | PASS |
| `merge_data_conflicts` | 127.73ms | 250.00ms | 51.1% | 0.33% | PASS |
| `merge_data_conflicts_with_resolve` | 127.63ms | 250.00ms | 51.1% | 0.37% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
