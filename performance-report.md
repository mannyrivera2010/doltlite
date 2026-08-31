# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-31 18:49 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260823.283.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/33418216862)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 24s | 8.87s | 11.47s | 1.294× | 1.48% | **PASS** |
| textpk | 69 | 55 | 1h 34m 39s | 11.03s | 11.94s | 1.082× | 2.48% | **PASS** |
| blobpk | 69 | 55 | 1h 32m 38s | 11.14s | 12.06s | 1.083× | 1.73% | **PASS** |
| compositepk | 69 | 55 | 1h 22m 4s | 8.81s | 11.09s | 1.258× | 2.81% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.69ms | 29.30ms | 1.237× | 1.54% | PASS |
| mem_reads | `oltp_range_select` | 10.07ms | 13.06ms | 1.297× | 4.08% | PASS |
| mem_reads | `oltp_sum_range` | 9.21ms | 12.29ms | 1.334× | 1.62% | PASS |
| mem_reads | `oltp_order_range` | 2.46ms | 2.94ms | 1.198× | 1.32% | PASS |
| mem_reads | `oltp_distinct_range` | 3.57ms | 4.01ms | 1.123× | 1.27% | PASS |
| mem_reads | `oltp_index_scan` | 3.79ms | 5.22ms | 1.376× | 1.65% | PASS |
| mem_reads | `select_random_points` | 9.70ms | 11.01ms | 1.135× | 4.85% | PASS |
| mem_reads | `select_random_ranges` | 2.89ms | 3.92ms | 1.359× | 2.44% | PASS |
| mem_reads | `covering_index_scan` | 4.24ms | 4.12ms | 0.973× | 1.26% | PASS |
| mem_reads | `groupby_scan` | 29.67ms | 33.03ms | 1.113× | 0.76% | PASS |
| mem_reads | `index_join` | 6.09ms | 8.03ms | 1.318× | 1.25% | PASS |
| mem_reads | `index_join_scan` | 3.57ms | 4.55ms | 1.275× | 2.08% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.33s | 1.285× | 0.58% | PASS |
| mem_reads | `table_scan` | 1.16s | 1.39s | 1.198× | 0.53% | PASS |
| mem_reads | `oltp_read_only` | 101.24ms | 122.68ms | 1.212× | 0.99% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.43ms | 250.83ms | 1.383× | 1.11% | PASS |
| mem_writes | `oltp_insert` | 15.44ms | 28.19ms | 1.825× | 0.73% | PASS |
| mem_writes | `oltp_update_index` | 48.73ms | 102.09ms | 2.095× | 0.77% | PASS |
| mem_writes | `oltp_update_non_index` | 32.63ms | 58.06ms | 1.780× | 0.89% | PASS |
| mem_writes | `oltp_delete_insert` | 43.49ms | 77.14ms | 1.774× | 0.84% | PASS |
| mem_writes | `oltp_write_only` | 20.98ms | 44.01ms | 2.097× | 0.70% | PASS |
| mem_writes | `types_delete_insert` | 24.59ms | 39.78ms | 1.618× | 1.17% | PASS |
| mem_writes | `oltp_read_write` | 66.40ms | 109.11ms | 1.643× | 1.63% | PASS |
| file_reads | `oltp_point_select` | 98.19ms | 55.24ms | 0.563× | 0.76% | PASS |
| file_reads | `oltp_range_select` | 17.38ms | 15.95ms | 0.918× | 1.84% | PASS |
| file_reads | `oltp_sum_range` | 16.84ms | 15.29ms | 0.908× | 1.34% | PASS |
| file_reads | `oltp_order_range` | 3.35ms | 3.36ms | 1.002× | 1.56% | PASS |
| file_reads | `oltp_distinct_range` | 4.41ms | 4.41ms | 0.998× | 1.81% | PASS |
| file_reads | `oltp_index_scan` | 11.40ms | 8.11ms | 0.711× | 1.09% | PASS |
| file_reads | `select_random_points` | 17.62ms | 13.94ms | 0.791× | 2.29% | PASS |
| file_reads | `select_random_ranges` | 10.40ms | 6.61ms | 0.636× | 1.28% | PASS |
| file_reads | `covering_index_scan` | 11.81ms | 7.05ms | 0.597× | 1.05% | PASS |
| file_reads | `groupby_scan` | 30.92ms | 33.61ms | 1.087× | 0.93% | PASS |
| file_reads | `index_join` | 10.32ms | 10.23ms | 0.991× | 1.48% | PASS |
| file_reads | `index_join_scan` | 4.56ms | 4.96ms | 1.089× | 1.80% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.33s | 1.257× | 1.84% | PASS |
| file_reads | `table_scan` | 1.20s | 1.40s | 1.163× | 2.16% | PASS |
| file_reads | `oltp_read_only` | 213.12ms | 162.22ms | 0.761× | 0.97% | PASS |
| file_writes | `oltp_bulk_insert` | 195.31ms | 270.88ms | 1.387× | 0.95% | PASS |
| file_writes | `oltp_insert` | 22.08ms | 35.63ms | 1.613× | 1.46% | PASS |
| file_writes | `oltp_update_index` | 76.57ms | 127.11ms | 1.660× | 1.38% | PASS |
| file_writes | `oltp_update_non_index` | 57.21ms | 80.84ms | 1.413× | 1.47% | PASS |
| file_writes | `oltp_delete_insert` | 67.62ms | 99.11ms | 1.466× | 1.50% | PASS |
| file_writes | `oltp_write_only` | 44.13ms | 64.41ms | 1.460× | 1.39% | PASS |
| file_writes | `types_delete_insert` | 39.98ms | 53.17ms | 1.330× | 1.52% | PASS |
| file_writes | `oltp_read_write` | 89.67ms | 128.96ms | 1.438× | 1.64% | PASS |
| ac_reads | `oltp_point_select` | 47.58ms | 54.96ms | 1.155× | 0.81% | PASS |
| ac_reads | `oltp_range_select` | 12.81ms | 15.86ms | 1.238× | 4.37% | PASS |
| ac_reads | `oltp_sum_range` | 12.96ms | 15.29ms | 1.180× | 1.49% | PASS |
| ac_reads | `oltp_order_range` | 2.90ms | 3.37ms | 1.160× | 2.13% | PASS |
| ac_reads | `oltp_distinct_range` | 3.99ms | 4.41ms | 1.106× | 1.69% | PASS |
| ac_reads | `oltp_index_scan` | 6.46ms | 8.19ms | 1.269× | 1.72% | PASS |
| ac_reads | `select_random_points` | 12.69ms | 13.91ms | 1.096× | 3.28% | PASS |
| ac_reads | `select_random_ranges` | 5.39ms | 6.58ms | 1.221× | 1.58% | PASS |
| ac_reads | `covering_index_scan` | 6.84ms | 6.97ms | 1.020× | 1.48% | PASS |
| ac_reads | `groupby_scan` | 30.06ms | 33.61ms | 1.118× | 0.94% | PASS |
| ac_reads | `index_join` | 7.58ms | 10.05ms | 1.326× | 1.34% | PASS |
| ac_reads | `index_join_scan` | 3.84ms | 4.93ms | 1.285× | 1.69% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.32s | 1.275× | 0.62% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.40s | 1.169× | 0.84% | PASS |
| ac_reads | `oltp_read_only` | 141.52ms | 161.76ms | 1.143× | 1.33% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.87ms | 81.91ms | 3.745× | 4.91% | PASS |
| ac_writes | `oltp_insert_ac` | 24.61ms | 99.39ms | 4.039× | 4.77% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.65ms | 115.30ms | 4.170× | 6.33% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.61ms | 92.75ms | 4.103× | 4.67% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.38ms | 104.54ms | 4.288× | 4.75% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.94ms | 103.74ms | 4.159× | 5.35% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.27ms | 92.78ms | 4.166× | 5.29% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.06ms | 110.75ms | 3.685× | 6.23% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.50ms | 36.10ms | 1.146× | 2.76% | PASS |
| mem_reads | `oltp_range_select` | 14.88ms | 13.96ms | 0.938× | 3.28% | PASS |
| mem_reads | `oltp_sum_range` | 12.72ms | 13.38ms | 1.052× | 2.86% | PASS |
| mem_reads | `oltp_order_range` | 3.19ms | 3.18ms | 0.995× | 2.03% | PASS |
| mem_reads | `oltp_distinct_range` | 4.22ms | 4.15ms | 0.983× | 0.90% | PASS |
| mem_reads | `oltp_index_scan` | 4.72ms | 6.12ms | 1.296× | 2.37% | PASS |
| mem_reads | `select_random_points` | 18.64ms | 20.86ms | 1.119× | 2.91% | PASS |
| mem_reads | `select_random_ranges` | 4.18ms | 5.28ms | 1.262× | 2.47% | PASS |
| mem_reads | `covering_index_scan` | 6.29ms | 4.88ms | 0.775× | 14.95% | PASS |
| mem_reads | `groupby_scan` | 34.55ms | 35.62ms | 1.031× | 1.06% | PASS |
| mem_reads | `index_join` | 7.08ms | 9.04ms | 1.278× | 2.59% | PASS |
| mem_reads | `index_join_scan` | 4.89ms | 5.60ms | 1.146× | 3.05% | PASS |
| mem_reads | `types_table_scan` | 1.19s | 1.28s | 1.070× | 3.16% | PASS |
| mem_reads | `table_scan` | 1.60s | 1.42s | 0.889× | 5.03% | PASS |
| mem_reads | `oltp_read_only` | 129.49ms | 132.28ms | 1.022× | 1.77% | PASS |
| mem_writes | `oltp_bulk_insert` | 232.13ms | 340.04ms | 1.465× | 1.32% | PASS |
| mem_writes | `oltp_insert` | 22.55ms | 39.66ms | 1.759× | 1.23% | PASS |
| mem_writes | `oltp_update_index` | 74.96ms | 139.03ms | 1.855× | 1.83% | PASS |
| mem_writes | `oltp_update_non_index` | 52.10ms | 90.00ms | 1.727× | 2.32% | PASS |
| mem_writes | `oltp_delete_insert` | 59.44ms | 112.73ms | 1.896× | 5.08% | PASS |
| mem_writes | `oltp_write_only` | 31.87ms | 66.01ms | 2.071× | 1.97% | PASS |
| mem_writes | `types_delete_insert` | 35.29ms | 57.13ms | 1.619× | 4.10% | PASS |
| mem_writes | `oltp_read_write` | 95.95ms | 140.47ms | 1.464× | 4.61% | PASS |
| file_reads | `oltp_point_select` | 127.36ms | 67.50ms | 0.530× | 1.04% | PASS |
| file_reads | `oltp_range_select` | 24.85ms | 16.92ms | 0.681× | 2.46% | PASS |
| file_reads | `oltp_sum_range` | 23.49ms | 16.99ms | 0.723× | 2.27% | PASS |
| file_reads | `oltp_order_range` | 4.32ms | 3.57ms | 0.826× | 2.48% | PASS |
| file_reads | `oltp_distinct_range` | 5.35ms | 4.54ms | 0.849× | 1.56% | PASS |
| file_reads | `oltp_index_scan` | 14.74ms | 9.47ms | 0.642× | 1.29% | PASS |
| file_reads | `select_random_points` | 28.32ms | 24.00ms | 0.847× | 1.79% | PASS |
| file_reads | `select_random_ranges` | 13.91ms | 8.49ms | 0.610× | 0.99% | PASS |
| file_reads | `covering_index_scan` | 15.80ms | 8.01ms | 0.507× | 2.75% | PASS |
| file_reads | `groupby_scan` | 35.35ms | 35.76ms | 1.011× | 0.87% | PASS |
| file_reads | `index_join` | 13.20ms | 11.66ms | 0.883× | 2.81% | PASS |
| file_reads | `index_join_scan` | 6.28ms | 6.22ms | 0.990× | 3.95% | PASS |
| file_reads | `types_table_scan` | 1.18s | 1.27s | 1.078× | 2.64% | PASS |
| file_reads | `table_scan` | 1.42s | 1.38s | 0.967× | 3.41% | PASS |
| file_reads | `oltp_read_only` | 262.36ms | 175.50ms | 0.669× | 1.11% | PASS |
| file_writes | `oltp_bulk_insert` | 253.90ms | 372.13ms | 1.466× | 1.75% | PASS |
| file_writes | `oltp_insert` | 51.49ms | 52.88ms | 1.027× | 20.35% | PASS |
| file_writes | `oltp_update_index` | 133.89ms | 187.74ms | 1.402× | 4.12% | PASS |
| file_writes | `oltp_update_non_index` | 119.15ms | 121.16ms | 1.017× | 9.82% | PASS |
| file_writes | `oltp_delete_insert` | 94.17ms | 139.03ms | 1.476× | 1.52% | PASS |
| file_writes | `oltp_write_only` | 86.06ms | 89.25ms | 1.037× | 8.86% | PASS |
| file_writes | `types_delete_insert` | 58.13ms | 76.65ms | 1.318× | 2.13% | PASS |
| file_writes | `oltp_read_write` | 136.65ms | 161.49ms | 1.182× | 3.73% | PASS |
| ac_reads | `oltp_point_select` | 62.83ms | 67.59ms | 1.076× | 1.38% | PASS |
| ac_reads | `oltp_range_select` | 18.75ms | 17.13ms | 0.914× | 3.21% | PASS |
| ac_reads | `oltp_sum_range` | 16.95ms | 16.88ms | 0.995× | 3.13% | PASS |
| ac_reads | `oltp_order_range` | 3.71ms | 3.55ms | 0.957× | 1.14% | PASS |
| ac_reads | `oltp_distinct_range` | 4.77ms | 4.57ms | 0.958× | 1.36% | PASS |
| ac_reads | `oltp_index_scan` | 8.47ms | 9.58ms | 1.130× | 1.65% | PASS |
| ac_reads | `select_random_points` | 22.53ms | 24.45ms | 1.085× | 2.09% | PASS |
| ac_reads | `select_random_ranges` | 7.67ms | 8.55ms | 1.114× | 0.83% | PASS |
| ac_reads | `covering_index_scan` | 9.63ms | 8.10ms | 0.841× | 1.28% | PASS |
| ac_reads | `groupby_scan` | 35.80ms | 36.42ms | 1.017× | 1.40% | PASS |
| ac_reads | `index_join` | 10.31ms | 11.81ms | 1.145× | 2.28% | PASS |
| ac_reads | `index_join_scan` | 5.68ms | 6.25ms | 1.099× | 3.46% | PASS |
| ac_reads | `types_table_scan` | 1.17s | 1.26s | 1.071× | 1.95% | PASS |
| ac_reads | `table_scan` | 1.54s | 1.41s | 0.916× | 5.31% | PASS |
| ac_reads | `oltp_read_only` | 172.87ms | 176.82ms | 1.023× | 1.78% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.61ms | 67.65ms | 3.841× | 5.46% | PASS |
| ac_writes | `oltp_insert_ac` | 19.76ms | 82.21ms | 4.161× | 4.81% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.34ms | 99.96ms | 4.685× | 4.79% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.17ms | 77.14ms | 4.491× | 6.55% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 19.17ms | 87.98ms | 4.590× | 4.28% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.35ms | 87.42ms | 4.518× | 4.29% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.01ms | 76.69ms | 4.789× | 5.36% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.72ms | 94.76ms | 3.685× | 3.90% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.08ms | 35.90ms | 1.119× | 1.88% | PASS |
| mem_reads | `oltp_range_select` | 14.68ms | 13.89ms | 0.946× | 2.88% | PASS |
| mem_reads | `oltp_sum_range` | 12.86ms | 13.40ms | 1.042× | 2.24% | PASS |
| mem_reads | `oltp_order_range` | 3.19ms | 3.19ms | 1.001× | 0.92% | PASS |
| mem_reads | `oltp_distinct_range` | 4.30ms | 4.18ms | 0.971× | 0.71% | PASS |
| mem_reads | `oltp_index_scan` | 4.88ms | 6.23ms | 1.277× | 2.91% | PASS |
| mem_reads | `select_random_points` | 19.26ms | 20.88ms | 1.084× | 2.33% | PASS |
| mem_reads | `select_random_ranges` | 4.40ms | 5.33ms | 1.211× | 1.60% | PASS |
| mem_reads | `covering_index_scan` | 4.83ms | 4.85ms | 1.003× | 2.94% | PASS |
| mem_reads | `groupby_scan` | 34.49ms | 35.69ms | 1.035× | 0.72% | PASS |
| mem_reads | `index_join` | 7.23ms | 9.92ms | 1.373× | 3.41% | PASS |
| mem_reads | `index_join_scan` | 4.86ms | 5.97ms | 1.230× | 4.91% | PASS |
| mem_reads | `types_table_scan` | 1.26s | 1.31s | 1.045× | 1.42% | PASS |
| mem_reads | `table_scan` | 1.55s | 1.43s | 0.921× | 1.48% | PASS |
| mem_reads | `oltp_read_only` | 129.79ms | 132.19ms | 1.018× | 1.80% | PASS |
| mem_writes | `oltp_bulk_insert` | 237.62ms | 335.22ms | 1.411× | 0.65% | PASS |
| mem_writes | `oltp_insert` | 20.91ms | 39.74ms | 1.901× | 1.20% | PASS |
| mem_writes | `oltp_update_index` | 74.00ms | 137.86ms | 1.863× | 2.10% | PASS |
| mem_writes | `oltp_update_non_index` | 52.31ms | 86.84ms | 1.660× | 1.69% | PASS |
| mem_writes | `oltp_delete_insert` | 53.76ms | 108.55ms | 2.019× | 2.10% | PASS |
| mem_writes | `oltp_write_only` | 30.11ms | 65.58ms | 2.178× | 1.89% | PASS |
| mem_writes | `types_delete_insert` | 33.51ms | 54.86ms | 1.637× | 0.99% | PASS |
| mem_writes | `oltp_read_write` | 88.90ms | 139.53ms | 1.569× | 1.73% | PASS |
| file_reads | `oltp_point_select` | 129.15ms | 68.27ms | 0.529× | 0.86% | PASS |
| file_reads | `oltp_range_select` | 24.54ms | 17.08ms | 0.696× | 1.82% | PASS |
| file_reads | `oltp_sum_range` | 23.08ms | 16.64ms | 0.721× | 1.73% | PASS |
| file_reads | `oltp_order_range` | 4.21ms | 3.55ms | 0.844× | 1.58% | PASS |
| file_reads | `oltp_distinct_range` | 5.31ms | 4.56ms | 0.859× | 0.93% | PASS |
| file_reads | `oltp_index_scan` | 14.62ms | 9.54ms | 0.652× | 1.38% | PASS |
| file_reads | `select_random_points` | 29.36ms | 24.14ms | 0.822× | 2.39% | PASS |
| file_reads | `select_random_ranges` | 14.31ms | 8.66ms | 0.606× | 0.90% | PASS |
| file_reads | `covering_index_scan` | 15.10ms | 8.08ms | 0.535× | 1.65% | PASS |
| file_reads | `groupby_scan` | 35.44ms | 35.92ms | 1.014× | 1.11% | PASS |
| file_reads | `index_join` | 12.84ms | 11.65ms | 0.908× | 2.41% | PASS |
| file_reads | `index_join_scan` | 5.85ms | 6.30ms | 1.077× | 4.41% | PASS |
| file_reads | `types_table_scan` | 1.23s | 1.28s | 1.047× | 2.99% | PASS |
| file_reads | `table_scan` | 1.57s | 1.43s | 0.912× | 1.61% | PASS |
| file_reads | `oltp_read_only` | 275.60ms | 182.39ms | 0.662× | 0.67% | PASS |
| file_writes | `oltp_bulk_insert` | 260.19ms | 362.58ms | 1.394× | 0.93% | PASS |
| file_writes | `oltp_insert` | 33.17ms | 53.59ms | 1.616× | 1.55% | PASS |
| file_writes | `oltp_update_index` | 115.95ms | 176.33ms | 1.521× | 2.11% | PASS |
| file_writes | `oltp_update_non_index` | 83.90ms | 112.84ms | 1.345× | 1.59% | PASS |
| file_writes | `oltp_delete_insert` | 85.51ms | 135.90ms | 1.589× | 1.62% | PASS |
| file_writes | `oltp_write_only` | 57.89ms | 88.60ms | 1.531× | 1.65% | PASS |
| file_writes | `types_delete_insert` | 53.94ms | 73.35ms | 1.360× | 1.43% | PASS |
| file_writes | `oltp_read_write` | 117.94ms | 161.62ms | 1.370× | 2.30% | PASS |
| ac_reads | `oltp_point_select` | 63.03ms | 67.25ms | 1.067× | 1.15% | PASS |
| ac_reads | `oltp_range_select` | 18.37ms | 16.99ms | 0.925× | 2.41% | PASS |
| ac_reads | `oltp_sum_range` | 16.68ms | 16.67ms | 0.999× | 1.80% | PASS |
| ac_reads | `oltp_order_range` | 3.66ms | 3.54ms | 0.968× | 0.72% | PASS |
| ac_reads | `oltp_distinct_range` | 4.71ms | 4.57ms | 0.970× | 0.77% | PASS |
| ac_reads | `oltp_index_scan` | 8.54ms | 9.54ms | 1.117× | 1.50% | PASS |
| ac_reads | `select_random_points` | 22.90ms | 23.86ms | 1.042× | 2.40% | PASS |
| ac_reads | `select_random_ranges` | 7.83ms | 8.56ms | 1.093× | 1.10% | PASS |
| ac_reads | `covering_index_scan` | 8.89ms | 8.07ms | 0.907× | 1.26% | PASS |
| ac_reads | `groupby_scan` | 34.79ms | 35.97ms | 1.034× | 0.56% | PASS |
| ac_reads | `index_join` | 9.98ms | 11.80ms | 1.183× | 2.71% | PASS |
| ac_reads | `index_join_scan` | 5.33ms | 6.33ms | 1.188× | 3.46% | PASS |
| ac_reads | `types_table_scan` | 1.20s | 1.28s | 1.074× | 2.59% | PASS |
| ac_reads | `table_scan` | 1.53s | 1.42s | 0.924× | 1.50% | PASS |
| ac_reads | `oltp_read_only` | 174.48ms | 177.42ms | 1.017× | 1.48% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.77ms | 66.60ms | 3.748× | 6.68% | PASS |
| ac_writes | `oltp_insert_ac` | 19.33ms | 89.04ms | 4.607× | 6.27% | PASS |
| ac_writes | `oltp_update_index_ac` | 22.11ms | 99.95ms | 4.521× | 4.31% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.12ms | 76.02ms | 4.441× | 5.61% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.93ms | 89.93ms | 4.750× | 5.14% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.63ms | 92.52ms | 4.712× | 4.55% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.81ms | 82.12ms | 4.886× | 6.35% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.88ms | 97.47ms | 3.766× | 5.77% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.15ms | 28.43ms | 1.130× | 2.53% | PASS |
| mem_reads | `oltp_range_select` | 16.32ms | 17.47ms | 1.070× | 3.02% | PASS |
| mem_reads | `oltp_sum_range` | 15.30ms | 16.11ms | 1.053× | 2.44% | PASS |
| mem_reads | `oltp_order_range` | 3.02ms | 3.14ms | 1.040× | 3.95% | PASS |
| mem_reads | `oltp_distinct_range` | 3.95ms | 3.99ms | 1.011× | 2.59% | PASS |
| mem_reads | `oltp_index_scan` | 3.86ms | 4.74ms | 1.229× | 3.69% | PASS |
| mem_reads | `select_random_points` | 24.24ms | 27.06ms | 1.116× | 2.08% | PASS |
| mem_reads | `select_random_ranges` | 6.18ms | 6.98ms | 1.129× | 3.03% | PASS |
| mem_reads | `covering_index_scan` | 3.16ms | 3.29ms | 1.039× | 4.13% | PASS |
| mem_reads | `groupby_scan` | 31.70ms | 32.15ms | 1.014× | 3.06% | PASS |
| mem_reads | `index_join` | 6.62ms | 8.30ms | 1.254× | 2.65% | PASS |
| mem_reads | `index_join_scan` | 3.34ms | 4.63ms | 1.386× | 3.98% | PASS |
| mem_reads | `types_table_scan` | 939.34ms | 1.10s | 1.173× | 1.10% | PASS |
| mem_reads | `table_scan` | 1.05s | 1.18s | 1.121× | 1.01% | PASS |
| mem_reads | `oltp_read_only` | 120.65ms | 132.12ms | 1.095× | 1.67% | PASS |
| mem_writes | `oltp_bulk_insert` | 184.95ms | 252.44ms | 1.365× | 1.62% | PASS |
| mem_writes | `oltp_insert` | 15.55ms | 27.75ms | 1.785× | 1.96% | PASS |
| mem_writes | `oltp_update_index` | 54.19ms | 92.64ms | 1.710× | 1.79% | PASS |
| mem_writes | `oltp_update_non_index` | 39.22ms | 63.34ms | 1.615× | 1.60% | PASS |
| mem_writes | `oltp_delete_insert` | 40.03ms | 74.03ms | 1.849× | 1.78% | PASS |
| mem_writes | `oltp_write_only` | 21.36ms | 43.42ms | 2.033× | 2.03% | PASS |
| mem_writes | `types_delete_insert` | 25.26ms | 41.37ms | 1.638× | 1.68% | PASS |
| mem_writes | `oltp_read_write` | 77.98ms | 115.72ms | 1.484× | 1.48% | PASS |
| file_reads | `oltp_point_select` | 54.44ms | 39.52ms | 0.726× | 1.98% | PASS |
| file_reads | `oltp_range_select` | 19.51ms | 18.68ms | 0.958× | 2.81% | PASS |
| file_reads | `oltp_sum_range` | 18.34ms | 17.48ms | 0.953× | 2.34% | PASS |
| file_reads | `oltp_order_range` | 3.62ms | 3.54ms | 0.977× | 4.26% | PASS |
| file_reads | `oltp_distinct_range` | 4.57ms | 4.46ms | 0.976× | 1.67% | PASS |
| file_reads | `oltp_index_scan` | 6.97ms | 6.16ms | 0.883× | 3.39% | PASS |
| file_reads | `select_random_points` | 27.52ms | 28.20ms | 1.025× | 2.63% | PASS |
| file_reads | `select_random_ranges` | 8.90ms | 7.84ms | 0.881× | 2.43% | PASS |
| file_reads | `covering_index_scan` | 6.02ms | 4.37ms | 0.726× | 4.36% | PASS |
| file_reads | `groupby_scan` | 32.23ms | 32.74ms | 1.016× | 2.27% | PASS |
| file_reads | `index_join` | 8.75ms | 10.04ms | 1.147× | 3.05% | PASS |
| file_reads | `index_join_scan` | 3.79ms | 5.02ms | 1.326× | 3.41% | PASS |
| file_reads | `types_table_scan` | 945.52ms | 1.12s | 1.182× | 1.23% | PASS |
| file_reads | `table_scan` | 1.07s | 1.20s | 1.116× | 1.23% | PASS |
| file_reads | `oltp_read_only` | 164.80ms | 148.12ms | 0.899× | 1.73% | PASS |
| file_writes | `oltp_bulk_insert` | 276.06ms | 361.21ms | 1.308× | 26.15% | PASS |
| file_writes | `oltp_insert` | 28.70ms | 48.25ms | 1.681× | 30.13% | PASS |
| file_writes | `oltp_update_index` | 161.59ms | 169.25ms | 1.047× | 13.92% | PASS |
| file_writes | `oltp_update_non_index` | 124.73ms | 121.03ms | 0.970× | 11.92% | PASS |
| file_writes | `oltp_delete_insert` | 138.86ms | 133.86ms | 0.964× | 15.24% | PASS |
| file_writes | `oltp_write_only` | 90.53ms | 98.46ms | 1.088× | 20.84% | PASS |
| file_writes | `types_delete_insert` | 79.03ms | 85.66ms | 1.084× | 14.96% | PASS |
| file_writes | `oltp_read_write` | 144.84ms | 169.57ms | 1.171× | 6.22% | PASS |
| ac_reads | `oltp_point_select` | 35.44ms | 39.62ms | 1.118× | 1.99% | PASS |
| ac_reads | `oltp_range_select` | 17.56ms | 18.82ms | 1.072× | 1.98% | PASS |
| ac_reads | `oltp_sum_range` | 16.83ms | 17.88ms | 1.062× | 1.91% | PASS |
| ac_reads | `oltp_order_range` | 3.42ms | 3.52ms | 1.030× | 4.79% | PASS |
| ac_reads | `oltp_distinct_range` | 4.31ms | 4.33ms | 1.004× | 3.11% | PASS |
| ac_reads | `oltp_index_scan` | 4.95ms | 5.82ms | 1.177× | 4.99% | PASS |
| ac_reads | `select_random_points` | 25.37ms | 28.01ms | 1.104× | 2.60% | PASS |
| ac_reads | `select_random_ranges` | 7.39ms | 8.10ms | 1.096× | 2.98% | PASS |
| ac_reads | `covering_index_scan` | 4.25ms | 4.55ms | 1.071× | 3.71% | PASS |
| ac_reads | `groupby_scan` | 31.70ms | 32.24ms | 1.017× | 2.03% | PASS |
| ac_reads | `index_join` | 7.88ms | 10.02ms | 1.272× | 3.29% | PASS |
| ac_reads | `index_join_scan` | 3.70ms | 5.13ms | 1.384× | 2.47% | PASS |
| ac_reads | `types_table_scan` | 940.29ms | 1.11s | 1.179× | 0.96% | PASS |
| ac_reads | `table_scan` | 1.07s | 1.20s | 1.121× | 1.06% | PASS |
| ac_reads | `oltp_read_only` | 131.87ms | 143.76ms | 1.090× | 1.50% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 34.75ms | 95.69ms | 2.754× | 14.21% | PASS |
| ac_writes | `oltp_insert_ac` | 41.24ms | 149.09ms | 3.615× | 33.66% | PASS |
| ac_writes | `oltp_update_index_ac` | 42.52ms | 186.22ms | 4.380× | 38.96% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 57.52ms | 173.06ms | 3.009× | 34.47% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 40.32ms | 194.05ms | 4.812× | 47.36% | PASS |
| ac_writes | `oltp_write_only_ac` | 52.07ms | 159.50ms | 3.063× | 43.61% | PASS |
| ac_writes | `types_delete_insert_ac` | 42.95ms | 144.85ms | 3.373× | 36.76% | PASS |
| ac_writes | `oltp_read_write_ac` | 47.48ms | 234.78ms | 4.944× | 43.98% | PASS |

</details>

## Version-control latency

Wall time: 2m 22s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.56ms | 200.00ms | 41.8% | 0.62% | PASS |
| `status_dirty_many_tables` | 86.80ms | 200.00ms | 43.4% | 0.63% | PASS |
| `diff_regular_working_one_table` | 79.17ms | 150.00ms | 52.8% | 0.50% | PASS |
| `diff_regular_working_many_tables` | 93.33ms | 200.00ms | 46.7% | 1.11% | PASS |
| `diff_stat_working_many_tables` | 93.11ms | 200.00ms | 46.6% | 1.07% | PASS |
| `diff_schema_working_many_tables` | 92.77ms | 200.00ms | 46.4% | 0.83% | PASS |
| `branch_list_many_branches` | 24.08ms | 100.00ms | 24.1% | 1.51% | PASS |
| `branch_create_delete` | 26.16ms | 100.00ms | 26.2% | 2.27% | PASS |
| `checkout_branch_clean` | 56.85ms | 200.00ms | 28.4% | 1.09% | PASS |
| `merge_data_no_conflicts` | 30.20ms | 150.00ms | 20.1% | 2.27% | PASS |
| `merge_schema_no_conflicts` | 22.37ms | 100.00ms | 22.4% | 1.36% | PASS |
| `merge_data_conflicts` | 127.69ms | 250.00ms | 51.1% | 0.34% | PASS |
| `merge_data_conflicts_with_resolve` | 127.70ms | 250.00ms | 51.1% | 0.35% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
