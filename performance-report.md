# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-06 18:29 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu26 20261101.186.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/42256464615)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 9s | 8.69s | 11.44s | 1.316× | 1.46% | **PASS** |
| textpk | 69 | 55 | 1h 33m 19s | 10.06s | 11.79s | 1.172× | 2.09% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 15s | 10.04s | 11.35s | 1.130× | 1.22% | **PASS** |
| compositepk | 69 | 55 | 1h 2m 3s | 6.82s | 7.69s | 1.127× | 2.48% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.46ms | 28.95ms | 1.234× | 1.33% | PASS |
| mem_reads | `oltp_range_select` | 10.01ms | 12.11ms | 1.210× | 1.24% | PASS |
| mem_reads | `oltp_sum_range` | 9.31ms | 12.03ms | 1.292× | 1.16% | PASS |
| mem_reads | `oltp_order_range` | 2.64ms | 3.02ms | 1.144× | 1.00% | PASS |
| mem_reads | `oltp_distinct_range` | 3.63ms | 4.15ms | 1.144× | 0.96% | PASS |
| mem_reads | `oltp_index_scan` | 3.92ms | 5.17ms | 1.320× | 1.46% | PASS |
| mem_reads | `select_random_points` | 10.18ms | 10.95ms | 1.075× | 3.84% | PASS |
| mem_reads | `select_random_ranges` | 2.99ms | 3.95ms | 1.320× | 2.88% | PASS |
| mem_reads | `covering_index_scan` | 4.16ms | 4.12ms | 0.990× | 1.33% | PASS |
| mem_reads | `groupby_scan` | 31.37ms | 34.95ms | 1.114× | 2.29% | PASS |
| mem_reads | `index_join` | 6.03ms | 7.89ms | 1.309× | 1.29% | PASS |
| mem_reads | `index_join_scan` | 3.38ms | 4.38ms | 1.294× | 1.40% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.30s | 1.268× | 0.40% | PASS |
| mem_reads | `table_scan` | 1.14s | 1.43s | 1.251× | 0.49% | PASS |
| mem_reads | `oltp_read_only` | 106.28ms | 124.59ms | 1.172× | 0.86% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.37ms | 253.66ms | 1.414× | 1.24% | PASS |
| mem_writes | `oltp_insert` | 15.25ms | 28.15ms | 1.846× | 0.68% | PASS |
| mem_writes | `oltp_update_index` | 49.94ms | 106.29ms | 2.129× | 1.18% | PASS |
| mem_writes | `oltp_update_non_index` | 34.13ms | 61.64ms | 1.806× | 1.12% | PASS |
| mem_writes | `oltp_delete_insert` | 44.09ms | 81.00ms | 1.837× | 1.07% | PASS |
| mem_writes | `oltp_write_only` | 21.45ms | 46.72ms | 2.178× | 1.42% | PASS |
| mem_writes | `types_delete_insert` | 24.69ms | 42.56ms | 1.723× | 1.51% | PASS |
| mem_writes | `oltp_read_write` | 66.09ms | 109.80ms | 1.662× | 1.16% | PASS |
| file_reads | `oltp_point_select` | 93.37ms | 52.74ms | 0.565× | 0.90% | PASS |
| file_reads | `oltp_range_select` | 17.24ms | 14.68ms | 0.851× | 2.99% | PASS |
| file_reads | `oltp_sum_range` | 16.82ms | 14.66ms | 0.872× | 2.09% | PASS |
| file_reads | `oltp_order_range` | 3.58ms | 3.33ms | 0.932× | 3.04% | PASS |
| file_reads | `oltp_distinct_range` | 4.45ms | 4.45ms | 1.001× | 1.96% | PASS |
| file_reads | `oltp_index_scan` | 11.11ms | 7.79ms | 0.701× | 1.94% | PASS |
| file_reads | `select_random_points` | 18.30ms | 13.70ms | 0.749× | 3.27% | PASS |
| file_reads | `select_random_ranges` | 10.20ms | 6.47ms | 0.635× | 1.48% | PASS |
| file_reads | `covering_index_scan` | 11.21ms | 6.85ms | 0.611× | 1.55% | PASS |
| file_reads | `groupby_scan` | 32.39ms | 36.40ms | 1.124× | 1.81% | PASS |
| file_reads | `index_join` | 10.08ms | 9.94ms | 0.987× | 1.15% | PASS |
| file_reads | `index_join_scan` | 4.38ms | 4.83ms | 1.103× | 2.45% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.34s | 1.297× | 0.48% | PASS |
| file_reads | `table_scan` | 1.14s | 1.43s | 1.251× | 0.76% | PASS |
| file_reads | `oltp_read_only` | 204.88ms | 159.49ms | 0.778× | 0.78% | PASS |
| file_writes | `oltp_bulk_insert` | 192.19ms | 272.30ms | 1.417× | 0.91% | PASS |
| file_writes | `oltp_insert` | 21.87ms | 34.48ms | 1.576× | 1.54% | PASS |
| file_writes | `oltp_update_index` | 76.85ms | 131.66ms | 1.713× | 1.28% | PASS |
| file_writes | `oltp_update_non_index` | 58.18ms | 84.75ms | 1.457× | 1.64% | PASS |
| file_writes | `oltp_delete_insert` | 68.45ms | 104.51ms | 1.527× | 1.63% | PASS |
| file_writes | `oltp_write_only` | 43.70ms | 63.82ms | 1.461× | 1.74% | PASS |
| file_writes | `types_delete_insert` | 39.38ms | 52.23ms | 1.326× | 2.52% | PASS |
| file_writes | `oltp_read_write` | 92.16ms | 127.77ms | 1.386× | 1.46% | PASS |
| ac_reads | `oltp_point_select` | 46.03ms | 53.11ms | 1.154× | 1.08% | PASS |
| ac_reads | `oltp_range_select` | 13.06ms | 14.77ms | 1.130× | 2.02% | PASS |
| ac_reads | `oltp_sum_range` | 12.38ms | 14.81ms | 1.196× | 1.82% | PASS |
| ac_reads | `oltp_order_range` | 3.02ms | 3.34ms | 1.105× | 1.19% | PASS |
| ac_reads | `oltp_distinct_range` | 4.04ms | 4.48ms | 1.107× | 1.33% | PASS |
| ac_reads | `oltp_index_scan` | 6.53ms | 8.01ms | 1.226× | 2.02% | PASS |
| ac_reads | `select_random_points` | 13.54ms | 13.77ms | 1.017× | 1.35% | PASS |
| ac_reads | `select_random_ranges` | 5.51ms | 6.46ms | 1.171× | 1.63% | PASS |
| ac_reads | `covering_index_scan` | 6.80ms | 6.92ms | 1.017× | 1.52% | PASS |
| ac_reads | `groupby_scan` | 31.83ms | 36.42ms | 1.144× | 2.31% | PASS |
| ac_reads | `index_join` | 7.73ms | 10.00ms | 1.293× | 1.21% | PASS |
| ac_reads | `index_join_scan` | 3.89ms | 4.90ms | 1.260× | 1.94% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.34s | 1.299× | 0.51% | PASS |
| ac_reads | `table_scan` | 1.15s | 1.43s | 1.239× | 0.89% | PASS |
| ac_reads | `oltp_read_only` | 139.12ms | 160.50ms | 1.154× | 1.14% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.34ms | 62.51ms | 3.073× | 5.55% | PASS |
| ac_writes | `oltp_insert_ac` | 23.22ms | 78.95ms | 3.400× | 5.93% | PASS |
| ac_writes | `oltp_update_index_ac` | 24.28ms | 92.81ms | 3.823× | 2.85% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.84ms | 70.81ms | 3.397× | 5.14% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.43ms | 83.20ms | 3.710× | 7.93% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.89ms | 83.30ms | 3.639× | 7.19% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.02ms | 72.87ms | 3.467× | 7.71% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.10ms | 89.93ms | 3.200× | 5.04% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.73ms | 37.72ms | 1.227× | 1.96% | PASS |
| mem_reads | `oltp_range_select` | 14.83ms | 14.59ms | 0.984× | 3.11% | PASS |
| mem_reads | `oltp_sum_range` | 12.82ms | 14.09ms | 1.099× | 1.97% | PASS |
| mem_reads | `oltp_order_range` | 3.13ms | 3.26ms | 1.040× | 0.94% | PASS |
| mem_reads | `oltp_distinct_range` | 4.08ms | 4.43ms | 1.084× | 1.05% | PASS |
| mem_reads | `oltp_index_scan` | 4.58ms | 6.29ms | 1.374× | 2.27% | PASS |
| mem_reads | `select_random_points` | 18.68ms | 20.77ms | 1.112× | 2.20% | PASS |
| mem_reads | `select_random_ranges` | 4.08ms | 5.24ms | 1.282× | 1.15% | PASS |
| mem_reads | `covering_index_scan` | 4.56ms | 4.59ms | 1.007× | 3.28% | PASS |
| mem_reads | `groupby_scan` | 33.53ms | 37.51ms | 1.119× | 2.18% | PASS |
| mem_reads | `index_join` | 7.03ms | 9.28ms | 1.319× | 2.48% | PASS |
| mem_reads | `index_join_scan` | 4.97ms | 5.35ms | 1.078× | 3.19% | PASS |
| mem_reads | `types_table_scan` | 1.09s | 1.25s | 1.140× | 1.21% | PASS |
| mem_reads | `table_scan` | 1.30s | 1.35s | 1.040× | 1.78% | PASS |
| mem_reads | `oltp_read_only` | 126.47ms | 140.47ms | 1.111× | 1.54% | PASS |
| mem_writes | `oltp_bulk_insert` | 231.69ms | 360.12ms | 1.554× | 0.72% | PASS |
| mem_writes | `oltp_insert` | 21.65ms | 40.78ms | 1.883× | 1.58% | PASS |
| mem_writes | `oltp_update_index` | 73.70ms | 141.48ms | 1.920× | 1.46% | PASS |
| mem_writes | `oltp_update_non_index` | 50.58ms | 91.32ms | 1.805× | 1.68% | PASS |
| mem_writes | `oltp_delete_insert` | 52.66ms | 109.32ms | 2.076× | 1.90% | PASS |
| mem_writes | `oltp_write_only` | 29.86ms | 65.73ms | 2.202× | 2.49% | PASS |
| mem_writes | `types_delete_insert` | 34.00ms | 58.77ms | 1.728× | 1.15% | PASS |
| mem_writes | `oltp_read_write` | 91.72ms | 145.37ms | 1.585× | 1.92% | PASS |
| file_reads | `oltp_point_select` | 101.26ms | 61.69ms | 0.609× | 0.99% | PASS |
| file_reads | `oltp_range_select` | 22.36ms | 17.37ms | 0.777× | 2.20% | PASS |
| file_reads | `oltp_sum_range` | 20.60ms | 17.01ms | 0.826× | 2.15% | PASS |
| file_reads | `oltp_order_range` | 4.12ms | 3.62ms | 0.879× | 2.00% | PASS |
| file_reads | `oltp_distinct_range` | 5.07ms | 4.79ms | 0.945× | 1.81% | PASS |
| file_reads | `oltp_index_scan` | 11.94ms | 8.98ms | 0.752× | 1.74% | PASS |
| file_reads | `select_random_points` | 26.78ms | 24.21ms | 0.904× | 2.37% | PASS |
| file_reads | `select_random_ranges` | 11.24ms | 7.72ms | 0.686× | 1.32% | PASS |
| file_reads | `covering_index_scan` | 12.27ms | 7.32ms | 0.597× | 3.89% | PASS |
| file_reads | `groupby_scan` | 34.94ms | 38.22ms | 1.094× | 2.06% | PASS |
| file_reads | `index_join` | 11.57ms | 11.38ms | 0.984× | 3.62% | PASS |
| file_reads | `index_join_scan` | 6.00ms | 5.92ms | 0.987× | 4.37% | PASS |
| file_reads | `types_table_scan` | 1.10s | 1.25s | 1.137× | 1.38% | PASS |
| file_reads | `table_scan` | 1.32s | 1.35s | 1.022× | 2.22% | PASS |
| file_reads | `oltp_read_only` | 232.77ms | 177.46ms | 0.762× | 1.43% | PASS |
| file_writes | `oltp_bulk_insert` | 249.39ms | 392.71ms | 1.575× | 0.91% | PASS |
| file_writes | `oltp_insert` | 50.32ms | 54.45ms | 1.082× | 16.40% | PASS |
| file_writes | `oltp_update_index` | 117.66ms | 178.49ms | 1.517× | 2.09% | PASS |
| file_writes | `oltp_update_non_index` | 98.62ms | 119.56ms | 1.212× | 11.12% | PASS |
| file_writes | `oltp_delete_insert` | 93.32ms | 140.56ms | 1.506× | 2.12% | PASS |
| file_writes | `oltp_write_only` | 88.87ms | 90.78ms | 1.022× | 10.02% | PASS |
| file_writes | `types_delete_insert` | 57.41ms | 77.98ms | 1.358× | 1.84% | PASS |
| file_writes | `oltp_read_write` | 144.77ms | 171.46ms | 1.184× | 7.85% | PASS |
| ac_reads | `oltp_point_select` | 54.06ms | 61.73ms | 1.142× | 1.26% | PASS |
| ac_reads | `oltp_range_select` | 18.17ms | 17.29ms | 0.952× | 2.37% | PASS |
| ac_reads | `oltp_sum_range` | 16.00ms | 17.01ms | 1.063× | 2.22% | PASS |
| ac_reads | `oltp_order_range` | 3.59ms | 3.58ms | 0.997× | 1.38% | PASS |
| ac_reads | `oltp_distinct_range` | 4.62ms | 4.80ms | 1.040× | 1.51% | PASS |
| ac_reads | `oltp_index_scan` | 7.50ms | 8.93ms | 1.190× | 1.65% | PASS |
| ac_reads | `select_random_points` | 21.96ms | 24.42ms | 1.112× | 1.67% | PASS |
| ac_reads | `select_random_ranges` | 6.59ms | 7.73ms | 1.172× | 1.64% | PASS |
| ac_reads | `covering_index_scan` | 8.23ms | 7.31ms | 0.888× | 3.32% | PASS |
| ac_reads | `groupby_scan` | 34.32ms | 37.90ms | 1.104× | 1.81% | PASS |
| ac_reads | `index_join` | 9.63ms | 11.53ms | 1.197× | 2.98% | PASS |
| ac_reads | `index_join_scan` | 5.53ms | 6.17ms | 1.116× | 4.35% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.24s | 1.124× | 1.53% | PASS |
| ac_reads | `table_scan` | 1.34s | 1.35s | 1.009× | 1.90% | PASS |
| ac_reads | `oltp_read_only` | 161.89ms | 176.51ms | 1.090× | 2.12% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.99ms | 67.10ms | 2.919× | 7.45% | PASS |
| ac_writes | `oltp_insert_ac` | 25.74ms | 83.48ms | 3.243× | 11.14% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.41ms | 102.65ms | 3.613× | 8.02% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.22ms | 79.17ms | 3.563× | 10.07% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.47ms | 92.76ms | 3.641× | 8.64% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.57ms | 91.21ms | 3.433× | 6.84% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.75ms | 81.97ms | 3.604× | 10.91% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.90ms | 97.35ms | 3.052× | 5.12% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.15ms | 30.54ms | 1.168× | 0.69% | PASS |
| mem_reads | `oltp_range_select` | 11.88ms | 12.73ms | 1.072× | 1.23% | PASS |
| mem_reads | `oltp_sum_range` | 11.65ms | 12.07ms | 1.036× | 1.36% | PASS |
| mem_reads | `oltp_order_range` | 2.74ms | 2.96ms | 1.083× | 1.45% | PASS |
| mem_reads | `oltp_distinct_range` | 3.56ms | 3.86ms | 1.086× | 1.55% | PASS |
| mem_reads | `oltp_index_scan` | 4.03ms | 5.32ms | 1.321× | 1.10% | PASS |
| mem_reads | `select_random_points` | 16.66ms | 19.25ms | 1.156× | 0.72% | PASS |
| mem_reads | `select_random_ranges` | 3.38ms | 4.59ms | 1.356× | 1.59% | PASS |
| mem_reads | `covering_index_scan` | 3.58ms | 3.95ms | 1.103× | 1.68% | PASS |
| mem_reads | `groupby_scan` | 32.71ms | 33.34ms | 1.020× | 1.02% | PASS |
| mem_reads | `index_join` | 6.29ms | 8.76ms | 1.393× | 1.23% | PASS |
| mem_reads | `index_join_scan` | 3.73ms | 5.20ms | 1.393× | 1.33% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.17s | 1.131× | 0.46% | PASS |
| mem_reads | `table_scan` | 1.25s | 1.27s | 1.019× | 2.35% | PASS |
| mem_reads | `oltp_read_only` | 106.14ms | 118.89ms | 1.120× | 0.84% | PASS |
| mem_writes | `oltp_bulk_insert` | 182.66ms | 271.96ms | 1.489× | 0.51% | PASS |
| mem_writes | `oltp_insert` | 16.61ms | 33.56ms | 2.020× | 0.70% | PASS |
| mem_writes | `oltp_update_index` | 59.69ms | 122.46ms | 2.052× | 0.71% | PASS |
| mem_writes | `oltp_update_non_index` | 41.41ms | 75.32ms | 1.819× | 0.86% | PASS |
| mem_writes | `oltp_delete_insert` | 41.97ms | 93.35ms | 2.224× | 0.61% | PASS |
| mem_writes | `oltp_write_only` | 23.47ms | 56.18ms | 2.393× | 0.94% | PASS |
| mem_writes | `types_delete_insert` | 26.80ms | 48.66ms | 1.816× | 0.76% | PASS |
| mem_writes | `oltp_read_write` | 72.61ms | 122.90ms | 1.693× | 0.81% | PASS |
| file_reads | `oltp_point_select` | 56.66ms | 40.72ms | 0.719× | 0.82% | PASS |
| file_reads | `oltp_range_select` | 15.81ms | 14.06ms | 0.889× | 1.16% | PASS |
| file_reads | `oltp_sum_range` | 15.68ms | 13.43ms | 0.857× | 1.27% | PASS |
| file_reads | `oltp_order_range` | 3.24ms | 3.16ms | 0.975× | 1.38% | PASS |
| file_reads | `oltp_distinct_range` | 4.12ms | 4.11ms | 0.998× | 0.79% | PASS |
| file_reads | `oltp_index_scan` | 7.64ms | 6.65ms | 0.870× | 0.87% | PASS |
| file_reads | `select_random_points` | 20.99ms | 21.01ms | 1.001× | 0.73% | PASS |
| file_reads | `select_random_ranges` | 6.69ms | 5.66ms | 0.846× | 0.86% | PASS |
| file_reads | `covering_index_scan` | 7.31ms | 5.14ms | 0.703× | 0.70% | PASS |
| file_reads | `groupby_scan` | 33.85ms | 34.01ms | 1.005× | 0.52% | PASS |
| file_reads | `index_join` | 8.71ms | 9.90ms | 1.137× | 0.96% | PASS |
| file_reads | `index_join_scan` | 4.35ms | 5.55ms | 1.274× | 1.57% | PASS |
| file_reads | `types_table_scan` | 1.16s | 1.23s | 1.064× | 1.29% | PASS |
| file_reads | `table_scan` | 1.43s | 1.36s | 0.946× | 0.39% | PASS |
| file_reads | `oltp_read_only` | 159.03ms | 139.08ms | 0.875× | 0.64% | PASS |
| file_writes | `oltp_bulk_insert` | 253.56ms | 364.54ms | 1.438× | 3.14% | PASS |
| file_writes | `oltp_insert` | 46.60ms | 67.04ms | 1.439× | 1.21% | PASS |
| file_writes | `oltp_update_index` | 203.09ms | 227.09ms | 1.118× | 1.35% | PASS |
| file_writes | `oltp_update_non_index` | 156.30ms | 150.23ms | 0.961× | 1.25% | PASS |
| file_writes | `oltp_delete_insert` | 165.03ms | 178.32ms | 1.081× | 1.80% | PASS |
| file_writes | `oltp_write_only` | 120.97ms | 124.85ms | 1.032× | 0.91% | PASS |
| file_writes | `types_delete_insert` | 104.51ms | 100.54ms | 0.962× | 5.22% | PASS |
| file_writes | `oltp_read_write` | 170.84ms | 190.84ms | 1.117× | 1.48% | PASS |
| ac_reads | `oltp_point_select` | 35.71ms | 40.04ms | 1.121× | 1.01% | PASS |
| ac_reads | `oltp_range_select` | 13.49ms | 13.93ms | 1.032× | 1.22% | PASS |
| ac_reads | `oltp_sum_range` | 13.56ms | 13.32ms | 0.983× | 1.59% | PASS |
| ac_reads | `oltp_order_range` | 3.04ms | 3.15ms | 1.037× | 1.52% | PASS |
| ac_reads | `oltp_distinct_range` | 3.92ms | 4.10ms | 1.044× | 2.08% | PASS |
| ac_reads | `oltp_index_scan` | 5.66ms | 6.64ms | 1.173× | 1.22% | PASS |
| ac_reads | `select_random_points` | 18.69ms | 20.68ms | 1.106× | 1.15% | PASS |
| ac_reads | `select_random_ranges` | 4.76ms | 5.73ms | 1.205× | 1.91% | PASS |
| ac_reads | `covering_index_scan` | 5.26ms | 5.15ms | 0.979× | 1.27% | PASS |
| ac_reads | `groupby_scan` | 33.29ms | 33.48ms | 1.006× | 0.65% | PASS |
| ac_reads | `index_join` | 7.70ms | 9.89ms | 1.284× | 1.11% | PASS |
| ac_reads | `index_join_scan` | 4.18ms | 5.52ms | 1.320× | 1.38% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.17s | 1.132× | 0.48% | PASS |
| ac_reads | `table_scan` | 1.35s | 1.31s | 0.966× | 2.07% | PASS |
| ac_reads | `oltp_read_only` | 124.04ms | 134.60ms | 1.085× | 0.86% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 28.22ms | 75.32ms | 2.669× | 3.37% | PASS |
| ac_writes | `oltp_insert_ac` | 30.10ms | 93.22ms | 3.097× | 4.29% | PASS |
| ac_writes | `oltp_update_index_ac` | 33.72ms | 107.22ms | 3.180× | 3.84% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 28.12ms | 86.47ms | 3.075× | 3.34% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 29.62ms | 97.82ms | 3.303× | 4.98% | PASS |
| ac_writes | `oltp_write_only_ac` | 30.16ms | 96.95ms | 3.214× | 4.10% | PASS |
| ac_writes | `types_delete_insert_ac` | 27.95ms | 91.32ms | 3.267× | 5.20% | PASS |
| ac_writes | `oltp_read_write_ac` | 34.39ms | 104.05ms | 3.025× | 3.66% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.39ms | 23.19ms | 1.137× | 2.32% | PASS |
| mem_reads | `oltp_range_select` | 11.42ms | 11.53ms | 1.009× | 2.27% | PASS |
| mem_reads | `oltp_sum_range` | 11.32ms | 11.91ms | 1.052× | 1.99% | PASS |
| mem_reads | `oltp_order_range` | 2.34ms | 2.30ms | 0.985× | 2.41% | PASS |
| mem_reads | `oltp_distinct_range` | 2.80ms | 2.74ms | 0.980× | 2.01% | PASS |
| mem_reads | `oltp_index_scan` | 2.96ms | 3.93ms | 1.326× | 2.82% | PASS |
| mem_reads | `select_random_points` | 16.95ms | 19.37ms | 1.143× | 2.86% | PASS |
| mem_reads | `select_random_ranges` | 4.30ms | 4.91ms | 1.141× | 2.53% | PASS |
| mem_reads | `covering_index_scan` | 2.26ms | 2.39ms | 1.057× | 5.38% | PASS |
| mem_reads | `groupby_scan` | 21.82ms | 22.86ms | 1.048× | 1.85% | PASS |
| mem_reads | `index_join` | 4.91ms | 6.12ms | 1.247× | 3.72% | PASS |
| mem_reads | `index_join_scan` | 2.52ms | 4.27ms | 1.693× | 7.10% | PASS |
| mem_reads | `types_table_scan` | 702.14ms | 756.68ms | 1.078× | 1.65% | PASS |
| mem_reads | `table_scan` | 815.77ms | 856.70ms | 1.050× | 2.74% | PASS |
| mem_reads | `oltp_read_only` | 84.01ms | 89.28ms | 1.063× | 1.63% | PASS |
| mem_writes | `oltp_bulk_insert` | 149.17ms | 213.54ms | 1.432× | 1.17% | PASS |
| mem_writes | `oltp_insert` | 11.76ms | 24.18ms | 2.057× | 1.73% | PASS |
| mem_writes | `oltp_update_index` | 41.70ms | 79.36ms | 1.903× | 2.11% | PASS |
| mem_writes | `oltp_update_non_index` | 33.98ms | 60.37ms | 1.777× | 2.43% | PASS |
| mem_writes | `oltp_delete_insert` | 33.30ms | 71.26ms | 2.140× | 2.79% | PASS |
| mem_writes | `oltp_write_only` | 18.15ms | 44.48ms | 2.450× | 1.89% | PASS |
| mem_writes | `types_delete_insert` | 21.38ms | 38.03ms | 1.779× | 1.78% | PASS |
| mem_writes | `oltp_read_write` | 55.64ms | 89.85ms | 1.615× | 1.43% | PASS |
| file_reads | `oltp_point_select` | 74.83ms | 42.23ms | 0.564× | 1.40% | PASS |
| file_reads | `oltp_range_select` | 17.39ms | 13.87ms | 0.798× | 1.27% | PASS |
| file_reads | `oltp_sum_range` | 16.61ms | 13.69ms | 0.825× | 0.97% | PASS |
| file_reads | `oltp_order_range` | 3.01ms | 2.60ms | 0.865× | 1.46% | PASS |
| file_reads | `oltp_distinct_range` | 3.56ms | 3.14ms | 0.883× | 1.25% | PASS |
| file_reads | `oltp_index_scan` | 8.98ms | 6.01ms | 0.670× | 4.13% | PASS |
| file_reads | `select_random_points` | 22.62ms | 20.73ms | 0.916× | 2.08% | PASS |
| file_reads | `select_random_ranges` | 10.44ms | 7.20ms | 0.690× | 1.37% | PASS |
| file_reads | `covering_index_scan` | 8.48ms | 4.54ms | 0.535× | 1.75% | PASS |
| file_reads | `groupby_scan` | 23.71ms | 24.12ms | 1.017× | 1.01% | PASS |
| file_reads | `index_join` | 8.41ms | 7.45ms | 0.886× | 1.61% | PASS |
| file_reads | `index_join_scan` | 3.48ms | 4.27ms | 1.228× | 4.31% | PASS |
| file_reads | `types_table_scan` | 690.62ms | 736.16ms | 1.066× | 1.89% | PASS |
| file_reads | `table_scan` | 793.85ms | 829.85ms | 1.045× | 1.78% | PASS |
| file_reads | `oltp_read_only` | 164.04ms | 117.15ms | 0.714× | 2.84% | PASS |
| file_writes | `oltp_bulk_insert` | 199.57ms | 278.15ms | 1.394× | 3.80% | PASS |
| file_writes | `oltp_insert` | 19.52ms | 38.07ms | 1.951× | 2.60% | PASS |
| file_writes | `oltp_update_index` | 141.12ms | 149.73ms | 1.061× | 1.72% | PASS |
| file_writes | `oltp_update_non_index` | 120.70ms | 113.60ms | 0.941× | 1.84% | PASS |
| file_writes | `oltp_delete_insert` | 117.64ms | 129.63ms | 1.102× | 4.26% | PASS |
| file_writes | `oltp_write_only` | 80.77ms | 95.37ms | 1.181× | 3.92% | PASS |
| file_writes | `types_delete_insert` | 78.62ms | 69.47ms | 0.884× | 5.74% | PASS |
| file_writes | `oltp_read_write` | 125.44ms | 147.61ms | 1.177× | 2.90% | PASS |
| ac_reads | `oltp_point_select` | 37.29ms | 41.25ms | 1.106× | 3.35% | PASS |
| ac_reads | `oltp_range_select` | 13.20ms | 13.41ms | 1.015× | 3.97% | PASS |
| ac_reads | `oltp_sum_range` | 12.63ms | 13.19ms | 1.044× | 2.59% | PASS |
| ac_reads | `oltp_order_range` | 2.57ms | 2.48ms | 0.965× | 2.01% | PASS |
| ac_reads | `oltp_distinct_range` | 3.22ms | 3.12ms | 0.967× | 2.70% | PASS |
| ac_reads | `oltp_index_scan` | 5.09ms | 5.99ms | 1.177× | 4.30% | PASS |
| ac_reads | `select_random_points` | 18.41ms | 20.77ms | 1.128× | 2.48% | PASS |
| ac_reads | `select_random_ranges` | 6.37ms | 7.00ms | 1.099× | 1.63% | PASS |
| ac_reads | `covering_index_scan` | 4.09ms | 4.45ms | 1.086× | 3.05% | PASS |
| ac_reads | `groupby_scan` | 22.22ms | 23.22ms | 1.045× | 1.90% | PASS |
| ac_reads | `index_join` | 6.06ms | 7.27ms | 1.198× | 3.65% | PASS |
| ac_reads | `index_join_scan` | 3.15ms | 4.35ms | 1.378× | 3.96% | PASS |
| ac_reads | `types_table_scan` | 701.22ms | 754.08ms | 1.075× | 1.41% | PASS |
| ac_reads | `table_scan` | 890.61ms | 853.42ms | 0.958× | 4.65% | PASS |
| ac_reads | `oltp_read_only` | 114.10ms | 115.42ms | 1.012× | 2.26% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 19.94ms | 52.22ms | 2.620× | 5.82% | PASS |
| ac_writes | `oltp_insert_ac` | 23.28ms | 67.43ms | 2.897× | 4.36% | PASS |
| ac_writes | `oltp_update_index_ac` | 24.77ms | 75.22ms | 3.037× | 4.25% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.03ms | 61.25ms | 3.058× | 5.44% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 21.98ms | 67.43ms | 3.068× | 3.29% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.28ms | 68.32ms | 3.066× | 4.79% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.63ms | 59.45ms | 2.748× | 6.53% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.53ms | 75.74ms | 3.088× | 4.54% | PASS |

</details>

## Version-control latency

Wall time: 1m 52s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 62.31ms | 200.00ms | 31.2% | 0.76% | PASS |
| `status_dirty_many_tables` | 64.55ms | 200.00ms | 32.3% | 0.70% | PASS |
| `diff_regular_working_one_table` | 57.72ms | 150.00ms | 38.5% | 0.63% | PASS |
| `diff_regular_working_many_tables` | 68.69ms | 200.00ms | 34.3% | 0.73% | PASS |
| `diff_stat_working_many_tables` | 69.28ms | 200.00ms | 34.6% | 0.67% | PASS |
| `diff_schema_working_many_tables` | 69.47ms | 200.00ms | 34.7% | 0.68% | PASS |
| `branch_list_many_branches` | 16.75ms | 100.00ms | 16.7% | 1.39% | PASS |
| `branch_create_delete` | 19.36ms | 100.00ms | 19.4% | 1.74% | PASS |
| `checkout_branch_clean` | 92.79ms | 200.00ms | 46.4% | 7.02% | PASS |
| `merge_data_no_conflicts` | 31.80ms | 150.00ms | 21.2% | 1.78% | PASS |
| `merge_schema_no_conflicts` | 17.37ms | 100.00ms | 17.4% | 1.70% | PASS |
| `merge_data_conflicts` | 73.57ms | 250.00ms | 29.4% | 0.69% | PASS |
| `merge_data_conflicts_with_resolve` | 74.39ms | 250.00ms | 29.8% | 0.59% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
