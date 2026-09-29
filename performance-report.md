# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-29 17:42 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260920.314.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/36595022916)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 7s | 9.14s | 11.49s | 1.257× | 1.47% | **PASS** |
| textpk | 69 | 55 | 1h 35m 4s | 10.55s | 11.98s | 1.136× | 4.02% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 27s | 9.63s | 11.75s | 1.220× | 1.93% | **PASS** |
| compositepk | 69 | 55 | 58m 49s | 6.42s | 7.30s | 1.136× | 1.58% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.93ms | 29.56ms | 1.236× | 1.54% | PASS |
| mem_reads | `oltp_range_select` | 10.16ms | 13.13ms | 1.292× | 1.47% | PASS |
| mem_reads | `oltp_sum_range` | 9.41ms | 12.33ms | 1.310× | 0.90% | PASS |
| mem_reads | `oltp_order_range` | 2.55ms | 2.99ms | 1.172× | 1.50% | PASS |
| mem_reads | `oltp_distinct_range` | 3.64ms | 4.05ms | 1.113× | 1.40% | PASS |
| mem_reads | `oltp_index_scan` | 3.95ms | 5.32ms | 1.347× | 1.46% | PASS |
| mem_reads | `select_random_points` | 10.11ms | 11.17ms | 1.105× | 2.02% | PASS |
| mem_reads | `select_random_ranges` | 2.96ms | 3.98ms | 1.345× | 1.61% | PASS |
| mem_reads | `covering_index_scan` | 4.24ms | 4.24ms | 1.001× | 1.40% | PASS |
| mem_reads | `groupby_scan` | 29.96ms | 33.33ms | 1.112× | 0.93% | PASS |
| mem_reads | `index_join` | 6.05ms | 8.56ms | 1.414× | 2.16% | PASS |
| mem_reads | `index_join_scan` | 3.47ms | 4.63ms | 1.335× | 2.14% | PASS |
| mem_reads | `types_table_scan` | 1.10s | 1.35s | 1.235× | 0.92% | PASS |
| mem_reads | `table_scan` | 1.22s | 1.41s | 1.159× | 2.21% | PASS |
| mem_reads | `oltp_read_only` | 104.21ms | 124.11ms | 1.191× | 1.18% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.91ms | 253.56ms | 1.402× | 1.32% | PASS |
| mem_writes | `oltp_insert` | 15.47ms | 28.34ms | 1.832× | 0.86% | PASS |
| mem_writes | `oltp_update_index` | 50.75ms | 106.25ms | 2.094× | 1.62% | PASS |
| mem_writes | `oltp_update_non_index` | 34.23ms | 60.15ms | 1.757× | 1.21% | PASS |
| mem_writes | `oltp_delete_insert` | 45.47ms | 80.02ms | 1.760× | 1.27% | PASS |
| mem_writes | `oltp_write_only` | 22.36ms | 46.21ms | 2.067× | 1.52% | PASS |
| mem_writes | `types_delete_insert` | 24.53ms | 40.81ms | 1.664× | 1.12% | PASS |
| mem_writes | `oltp_read_write` | 69.82ms | 112.47ms | 1.611× | 2.13% | PASS |
| file_reads | `oltp_point_select` | 99.05ms | 55.50ms | 0.560× | 0.52% | PASS |
| file_reads | `oltp_range_select` | 18.32ms | 15.93ms | 0.870× | 1.08% | PASS |
| file_reads | `oltp_sum_range` | 17.74ms | 15.31ms | 0.863× | 1.32% | PASS |
| file_reads | `oltp_order_range` | 3.45ms | 3.33ms | 0.964× | 1.75% | PASS |
| file_reads | `oltp_distinct_range` | 4.52ms | 4.38ms | 0.969× | 1.50% | PASS |
| file_reads | `oltp_index_scan` | 11.90ms | 8.46ms | 0.711× | 1.26% | PASS |
| file_reads | `select_random_points` | 18.39ms | 14.12ms | 0.768× | 2.24% | PASS |
| file_reads | `select_random_ranges` | 10.57ms | 6.66ms | 0.630× | 0.83% | PASS |
| file_reads | `covering_index_scan` | 12.04ms | 7.18ms | 0.596× | 0.90% | PASS |
| file_reads | `groupby_scan` | 30.95ms | 33.84ms | 1.093× | 0.92% | PASS |
| file_reads | `index_join` | 10.42ms | 10.46ms | 1.004× | 1.28% | PASS |
| file_reads | `index_join_scan` | 4.47ms | 5.12ms | 1.145× | 1.76% | PASS |
| file_reads | `types_table_scan` | 1.08s | 1.34s | 1.245× | 1.03% | PASS |
| file_reads | `table_scan` | 1.24s | 1.41s | 1.136× | 1.49% | PASS |
| file_reads | `oltp_read_only` | 214.65ms | 162.31ms | 0.756× | 1.10% | PASS |
| file_writes | `oltp_bulk_insert` | 195.35ms | 271.95ms | 1.392× | 1.08% | PASS |
| file_writes | `oltp_insert` | 22.40ms | 35.83ms | 1.599× | 1.83% | PASS |
| file_writes | `oltp_update_index` | 77.74ms | 128.97ms | 1.659× | 1.60% | PASS |
| file_writes | `oltp_update_non_index` | 57.77ms | 81.51ms | 1.411× | 1.73% | PASS |
| file_writes | `oltp_delete_insert` | 68.30ms | 99.75ms | 1.460× | 1.55% | PASS |
| file_writes | `oltp_write_only` | 45.33ms | 64.85ms | 1.431× | 1.78% | PASS |
| file_writes | `types_delete_insert` | 40.64ms | 54.08ms | 1.331× | 1.66% | PASS |
| file_writes | `oltp_read_write` | 94.11ms | 130.82ms | 1.390× | 1.96% | PASS |
| ac_reads | `oltp_point_select` | 48.61ms | 55.32ms | 1.138× | 0.64% | PASS |
| ac_reads | `oltp_range_select` | 12.97ms | 15.85ms | 1.222× | 1.34% | PASS |
| ac_reads | `oltp_sum_range` | 12.41ms | 15.18ms | 1.223× | 1.17% | PASS |
| ac_reads | `oltp_order_range` | 2.94ms | 3.32ms | 1.128× | 1.85% | PASS |
| ac_reads | `oltp_distinct_range` | 3.98ms | 4.37ms | 1.097× | 0.98% | PASS |
| ac_reads | `oltp_index_scan` | 6.79ms | 8.35ms | 1.229× | 1.28% | PASS |
| ac_reads | `select_random_points` | 12.98ms | 13.93ms | 1.074× | 1.57% | PASS |
| ac_reads | `select_random_ranges` | 5.58ms | 6.62ms | 1.186× | 0.85% | PASS |
| ac_reads | `covering_index_scan` | 7.08ms | 7.13ms | 1.006× | 0.88% | PASS |
| ac_reads | `groupby_scan` | 30.25ms | 33.77ms | 1.116× | 1.11% | PASS |
| ac_reads | `index_join` | 7.76ms | 10.31ms | 1.329× | 1.76% | PASS |
| ac_reads | `index_join_scan` | 3.89ms | 5.09ms | 1.307× | 1.47% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.34s | 1.260× | 1.45% | PASS |
| ac_reads | `table_scan` | 1.23s | 1.41s | 1.149× | 1.55% | PASS |
| ac_reads | `oltp_read_only` | 141.96ms | 162.88ms | 1.147× | 1.09% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.52ms | 69.32ms | 3.078× | 5.37% | PASS |
| ac_writes | `oltp_insert_ac` | 26.75ms | 86.73ms | 3.242× | 6.18% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.86ms | 101.87ms | 3.530× | 5.87% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.47ms | 79.49ms | 3.248× | 7.01% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.89ms | 92.44ms | 3.438× | 8.58% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.70ms | 89.34ms | 3.346× | 6.45% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.17ms | 80.07ms | 3.313× | 7.81% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.96ms | 96.33ms | 3.014× | 5.83% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.61ms | 39.00ms | 1.234× | 2.83% | PASS |
| mem_reads | `oltp_range_select` | 14.76ms | 14.29ms | 0.969× | 4.65% | PASS |
| mem_reads | `oltp_sum_range` | 14.41ms | 14.89ms | 1.033× | 3.32% | PASS |
| mem_reads | `oltp_order_range` | 3.22ms | 3.26ms | 1.011× | 2.54% | PASS |
| mem_reads | `oltp_distinct_range` | 4.26ms | 4.29ms | 1.006× | 1.44% | PASS |
| mem_reads | `oltp_index_scan` | 4.77ms | 6.50ms | 1.364× | 3.33% | PASS |
| mem_reads | `select_random_points` | 19.73ms | 21.37ms | 1.083× | 4.77% | PASS |
| mem_reads | `select_random_ranges` | 4.38ms | 5.33ms | 1.217× | 3.21% | PASS |
| mem_reads | `covering_index_scan` | 5.75ms | 4.86ms | 0.844× | 8.72% | PASS |
| mem_reads | `groupby_scan` | 33.23ms | 34.77ms | 1.046× | 1.69% | PASS |
| mem_reads | `index_join` | 7.40ms | 9.68ms | 1.309× | 4.98% | PASS |
| mem_reads | `index_join_scan` | 5.09ms | 5.44ms | 1.068× | 4.12% | PASS |
| mem_reads | `types_table_scan` | 1.19s | 1.27s | 1.070× | 5.21% | PASS |
| mem_reads | `table_scan` | 1.41s | 1.40s | 0.996× | 5.56% | PASS |
| mem_reads | `oltp_read_only` | 133.03ms | 142.78ms | 1.073× | 4.01% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.04ms | 360.98ms | 1.529× | 1.17% | PASS |
| mem_writes | `oltp_insert` | 22.68ms | 40.29ms | 1.776× | 4.10% | PASS |
| mem_writes | `oltp_update_index` | 73.34ms | 135.09ms | 1.842× | 1.88% | PASS |
| mem_writes | `oltp_update_non_index` | 51.38ms | 88.37ms | 1.720× | 3.01% | PASS |
| mem_writes | `oltp_delete_insert` | 54.17ms | 107.30ms | 1.981× | 4.07% | PASS |
| mem_writes | `oltp_write_only` | 30.64ms | 62.69ms | 2.046× | 3.75% | PASS |
| mem_writes | `types_delete_insert` | 34.69ms | 57.07ms | 1.645× | 2.33% | PASS |
| mem_writes | `oltp_read_write` | 98.75ms | 146.57ms | 1.484× | 5.07% | PASS |
| file_reads | `oltp_point_select` | 109.14ms | 66.00ms | 0.605× | 1.97% | PASS |
| file_reads | `oltp_range_select` | 23.89ms | 17.59ms | 0.737× | 4.45% | PASS |
| file_reads | `oltp_sum_range` | 21.52ms | 17.46ms | 0.811× | 3.49% | PASS |
| file_reads | `oltp_order_range` | 4.39ms | 3.90ms | 0.887× | 4.37% | PASS |
| file_reads | `oltp_distinct_range` | 5.43ms | 4.85ms | 0.892× | 3.36% | PASS |
| file_reads | `oltp_index_scan` | 12.50ms | 9.30ms | 0.745× | 2.33% | PASS |
| file_reads | `select_random_points` | 28.79ms | 25.42ms | 0.883× | 2.66% | PASS |
| file_reads | `select_random_ranges` | 11.83ms | 8.02ms | 0.678× | 1.45% | PASS |
| file_reads | `covering_index_scan` | 13.74ms | 7.57ms | 0.551× | 4.88% | PASS |
| file_reads | `groupby_scan` | 35.12ms | 35.93ms | 1.023× | 1.48% | PASS |
| file_reads | `index_join` | 13.57ms | 11.93ms | 0.879× | 4.30% | PASS |
| file_reads | `index_join_scan` | 6.58ms | 6.11ms | 0.929× | 5.82% | PASS |
| file_reads | `types_table_scan` | 1.14s | 1.26s | 1.110× | 4.31% | PASS |
| file_reads | `table_scan` | 1.42s | 1.39s | 0.982× | 8.89% | PASS |
| file_reads | `oltp_read_only` | 247.31ms | 181.86ms | 0.735× | 1.50% | PASS |
| file_writes | `oltp_bulk_insert` | 256.78ms | 391.07ms | 1.523× | 1.14% | PASS |
| file_writes | `oltp_insert` | 48.57ms | 52.90ms | 1.089× | 17.52% | PASS |
| file_writes | `oltp_update_index` | 121.08ms | 175.54ms | 1.450× | 3.17% | PASS |
| file_writes | `oltp_update_non_index` | 99.42ms | 115.32ms | 1.160× | 11.62% | PASS |
| file_writes | `oltp_delete_insert` | 92.10ms | 137.32ms | 1.491× | 3.25% | PASS |
| file_writes | `oltp_write_only` | 85.73ms | 85.72ms | 1.000× | 11.64% | PASS |
| file_writes | `types_delete_insert` | 57.96ms | 75.93ms | 1.310× | 2.21% | PASS |
| file_writes | `oltp_read_write` | 144.90ms | 164.70ms | 1.137× | 12.04% | PASS |
| ac_reads | `oltp_point_select` | 57.51ms | 65.69ms | 1.142× | 1.98% | PASS |
| ac_reads | `oltp_range_select` | 18.17ms | 17.52ms | 0.964× | 4.19% | PASS |
| ac_reads | `oltp_sum_range` | 16.26ms | 17.39ms | 1.070× | 2.99% | PASS |
| ac_reads | `oltp_order_range` | 3.82ms | 3.80ms | 0.994× | 5.20% | PASS |
| ac_reads | `oltp_distinct_range` | 4.75ms | 4.76ms | 1.003× | 2.00% | PASS |
| ac_reads | `oltp_index_scan` | 7.70ms | 9.30ms | 1.208× | 2.40% | PASS |
| ac_reads | `select_random_points` | 22.32ms | 24.92ms | 1.116× | 2.90% | PASS |
| ac_reads | `select_random_ranges` | 6.94ms | 7.92ms | 1.142× | 1.64% | PASS |
| ac_reads | `covering_index_scan` | 9.50ms | 7.67ms | 0.808× | 4.02% | PASS |
| ac_reads | `groupby_scan` | 33.89ms | 35.45ms | 1.046× | 0.77% | PASS |
| ac_reads | `index_join` | 9.64ms | 11.43ms | 1.185× | 3.93% | PASS |
| ac_reads | `index_join_scan` | 5.55ms | 5.96ms | 1.074× | 4.75% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.25s | 1.121× | 5.32% | PASS |
| ac_reads | `table_scan` | 1.39s | 1.41s | 1.013× | 9.04% | PASS |
| ac_reads | `oltp_read_only` | 164.25ms | 179.22ms | 1.091× | 3.21% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.24ms | 68.26ms | 3.069× | 5.63% | PASS |
| ac_writes | `oltp_insert_ac` | 25.81ms | 85.54ms | 3.314× | 5.60% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.91ms | 103.87ms | 3.722× | 4.89% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.34ms | 81.67ms | 3.499× | 6.16% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.12ms | 94.52ms | 3.619× | 5.93% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.52ms | 90.19ms | 3.534× | 4.74% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.94ms | 80.26ms | 3.658× | 5.47% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.12ms | 102.81ms | 3.201× | 6.09% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 28.69ms | 36.45ms | 1.270× | 1.27% | PASS |
| mem_reads | `oltp_range_select` | 11.77ms | 13.99ms | 1.189× | 1.82% | PASS |
| mem_reads | `oltp_sum_range` | 11.14ms | 13.92ms | 1.250× | 1.88% | PASS |
| mem_reads | `oltp_order_range` | 2.73ms | 3.13ms | 1.148× | 1.40% | PASS |
| mem_reads | `oltp_distinct_range` | 3.82ms | 4.20ms | 1.100× | 1.30% | PASS |
| mem_reads | `oltp_index_scan` | 4.27ms | 6.03ms | 1.412× | 2.03% | PASS |
| mem_reads | `select_random_points` | 16.55ms | 20.46ms | 1.237× | 1.88% | PASS |
| mem_reads | `select_random_ranges` | 3.72ms | 5.10ms | 1.372× | 1.96% | PASS |
| mem_reads | `covering_index_scan` | 4.34ms | 4.32ms | 0.994× | 1.11% | PASS |
| mem_reads | `groupby_scan` | 31.24ms | 34.19ms | 1.094× | 0.84% | PASS |
| mem_reads | `index_join` | 6.71ms | 8.89ms | 1.324× | 1.72% | PASS |
| mem_reads | `index_join_scan` | 3.97ms | 5.47ms | 1.378× | 3.48% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.22s | 1.186× | 0.59% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.36s | 1.166× | 0.45% | PASS |
| mem_reads | `oltp_read_only` | 114.12ms | 134.23ms | 1.176× | 1.38% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.69ms | 347.52ms | 1.468× | 0.73% | PASS |
| mem_writes | `oltp_insert` | 19.64ms | 38.82ms | 1.976× | 0.74% | PASS |
| mem_writes | `oltp_update_index` | 66.11ms | 128.09ms | 1.938× | 1.10% | PASS |
| mem_writes | `oltp_update_non_index` | 46.80ms | 83.21ms | 1.778× | 1.61% | PASS |
| mem_writes | `oltp_delete_insert` | 49.52ms | 104.02ms | 2.101× | 1.35% | PASS |
| mem_writes | `oltp_write_only` | 28.43ms | 62.79ms | 2.209× | 2.42% | PASS |
| mem_writes | `types_delete_insert` | 31.79ms | 53.04ms | 1.668× | 2.21% | PASS |
| mem_writes | `oltp_read_write` | 79.99ms | 137.29ms | 1.716× | 0.83% | PASS |
| file_reads | `oltp_point_select` | 104.53ms | 62.95ms | 0.602× | 1.17% | PASS |
| file_reads | `oltp_range_select` | 20.78ms | 17.09ms | 0.822× | 3.89% | PASS |
| file_reads | `oltp_sum_range` | 20.21ms | 17.21ms | 0.851× | 2.91% | PASS |
| file_reads | `oltp_order_range` | 3.97ms | 3.71ms | 0.933× | 5.04% | PASS |
| file_reads | `oltp_distinct_range` | 5.07ms | 4.79ms | 0.945× | 2.66% | PASS |
| file_reads | `oltp_index_scan` | 12.49ms | 9.12ms | 0.730× | 2.82% | PASS |
| file_reads | `select_random_points` | 30.48ms | 26.78ms | 0.879× | 3.14% | PASS |
| file_reads | `select_random_ranges` | 12.43ms | 8.10ms | 0.652× | 1.63% | PASS |
| file_reads | `covering_index_scan` | 13.29ms | 7.48ms | 0.563× | 2.57% | PASS |
| file_reads | `groupby_scan` | 34.27ms | 35.60ms | 1.039× | 1.02% | PASS |
| file_reads | `index_join` | 12.71ms | 12.19ms | 0.959× | 3.19% | PASS |
| file_reads | `index_join_scan` | 5.50ms | 6.14ms | 1.117× | 4.08% | PASS |
| file_reads | `types_table_scan` | 1.17s | 1.26s | 1.081× | 4.56% | PASS |
| file_reads | `table_scan` | 1.43s | 1.42s | 0.992× | 1.30% | PASS |
| file_reads | `oltp_read_only` | 239.12ms | 179.55ms | 0.751× | 1.89% | PASS |
| file_writes | `oltp_bulk_insert` | 256.17ms | 375.97ms | 1.468× | 0.98% | PASS |
| file_writes | `oltp_insert` | 32.70ms | 52.61ms | 1.609× | 3.58% | PASS |
| file_writes | `oltp_update_index` | 109.57ms | 170.37ms | 1.555× | 2.65% | PASS |
| file_writes | `oltp_update_non_index` | 81.28ms | 112.12ms | 1.379× | 1.69% | PASS |
| file_writes | `oltp_delete_insert` | 81.39ms | 131.84ms | 1.620× | 2.61% | PASS |
| file_writes | `oltp_write_only` | 54.30ms | 83.80ms | 1.543× | 2.10% | PASS |
| file_writes | `types_delete_insert` | 51.11ms | 71.30ms | 1.395× | 2.46% | PASS |
| file_writes | `oltp_read_write` | 110.81ms | 160.69ms | 1.450× | 1.72% | PASS |
| ac_reads | `oltp_point_select` | 54.02ms | 63.17ms | 1.170× | 1.13% | PASS |
| ac_reads | `oltp_range_select` | 15.35ms | 16.94ms | 1.104× | 1.85% | PASS |
| ac_reads | `oltp_sum_range` | 14.71ms | 17.11ms | 1.163× | 1.93% | PASS |
| ac_reads | `oltp_order_range` | 3.33ms | 3.65ms | 1.094× | 2.75% | PASS |
| ac_reads | `oltp_distinct_range` | 4.49ms | 4.81ms | 1.072× | 2.72% | PASS |
| ac_reads | `oltp_index_scan` | 7.12ms | 9.12ms | 1.281× | 2.11% | PASS |
| ac_reads | `select_random_points` | 20.76ms | 24.38ms | 1.174× | 1.53% | PASS |
| ac_reads | `select_random_ranges` | 6.57ms | 7.83ms | 1.192× | 1.38% | PASS |
| ac_reads | `covering_index_scan` | 7.26ms | 7.32ms | 1.009× | 2.53% | PASS |
| ac_reads | `groupby_scan` | 32.50ms | 35.30ms | 1.086× | 1.09% | PASS |
| ac_reads | `index_join` | 8.55ms | 11.63ms | 1.360× | 2.66% | PASS |
| ac_reads | `index_join_scan` | 4.79ms | 6.12ms | 1.279× | 2.84% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.22s | 1.187× | 0.53% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.36s | 1.163× | 0.55% | PASS |
| ac_reads | `oltp_read_only` | 152.41ms | 174.88ms | 1.147× | 1.15% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.79ms | 70.13ms | 2.948× | 7.14% | PASS |
| ac_writes | `oltp_insert_ac` | 27.89ms | 95.19ms | 3.414× | 9.03% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.31ms | 106.48ms | 3.633× | 7.40% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.85ms | 86.03ms | 3.607× | 8.82% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.32ms | 93.36ms | 3.548× | 5.79% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.23ms | 94.32ms | 3.596× | 8.82% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.32ms | 85.74ms | 3.677× | 6.91% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.78ms | 101.14ms | 3.183× | 5.07% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 18.49ms | 22.77ms | 1.232× | 2.95% | PASS |
| mem_reads | `oltp_range_select` | 9.92ms | 10.80ms | 1.089× | 4.25% | PASS |
| mem_reads | `oltp_sum_range` | 9.22ms | 10.69ms | 1.159× | 3.19% | PASS |
| mem_reads | `oltp_order_range` | 1.98ms | 2.06ms | 1.040× | 2.05% | PASS |
| mem_reads | `oltp_distinct_range` | 2.41ms | 2.56ms | 1.063× | 1.65% | PASS |
| mem_reads | `oltp_index_scan` | 2.57ms | 3.45ms | 1.345× | 2.22% | PASS |
| mem_reads | `select_random_points` | 14.85ms | 18.16ms | 1.223× | 4.36% | PASS |
| mem_reads | `select_random_ranges` | 3.96ms | 4.81ms | 1.214× | 2.47% | PASS |
| mem_reads | `covering_index_scan` | 2.29ms | 2.37ms | 1.034× | 1.27% | PASS |
| mem_reads | `groupby_scan` | 18.89ms | 20.41ms | 1.081× | 0.74% | PASS |
| mem_reads | `index_join` | 4.57ms | 5.68ms | 1.243× | 1.14% | PASS |
| mem_reads | `index_join_scan` | 2.12ms | 3.74ms | 1.763× | 2.02% | PASS |
| mem_reads | `types_table_scan` | 669.45ms | 722.47ms | 1.079× | 0.87% | PASS |
| mem_reads | `table_scan` | 758.72ms | 804.12ms | 1.060× | 0.81% | PASS |
| mem_reads | `oltp_read_only` | 75.93ms | 83.43ms | 1.099× | 0.88% | PASS |
| mem_writes | `oltp_bulk_insert` | 140.46ms | 192.55ms | 1.371× | 0.78% | PASS |
| mem_writes | `oltp_insert` | 10.90ms | 20.98ms | 1.925× | 0.58% | PASS |
| mem_writes | `oltp_update_index` | 38.38ms | 70.87ms | 1.847× | 1.58% | PASS |
| mem_writes | `oltp_update_non_index` | 30.74ms | 52.78ms | 1.717× | 2.14% | PASS |
| mem_writes | `oltp_delete_insert` | 29.56ms | 58.96ms | 1.995× | 1.51% | PASS |
| mem_writes | `oltp_write_only` | 16.71ms | 37.89ms | 2.267× | 2.31% | PASS |
| mem_writes | `types_delete_insert` | 19.46ms | 32.74ms | 1.682× | 3.74% | PASS |
| mem_writes | `oltp_read_write` | 51.21ms | 82.03ms | 1.602× | 1.86% | PASS |
| file_reads | `oltp_point_select` | 74.42ms | 41.61ms | 0.559× | 0.67% | PASS |
| file_reads | `oltp_range_select` | 16.30ms | 12.81ms | 0.786× | 1.23% | PASS |
| file_reads | `oltp_sum_range` | 15.43ms | 12.64ms | 0.819× | 0.87% | PASS |
| file_reads | `oltp_order_range` | 2.77ms | 2.30ms | 0.833× | 1.47% | PASS |
| file_reads | `oltp_distinct_range` | 3.29ms | 2.80ms | 0.851× | 1.03% | PASS |
| file_reads | `oltp_index_scan` | 8.61ms | 5.55ms | 0.645× | 0.85% | PASS |
| file_reads | `select_random_points` | 22.19ms | 19.98ms | 0.900× | 0.98% | PASS |
| file_reads | `select_random_ranges` | 9.99ms | 6.89ms | 0.690× | 0.82% | PASS |
| file_reads | `covering_index_scan` | 8.21ms | 4.39ms | 0.534× | 0.92% | PASS |
| file_reads | `groupby_scan` | 20.32ms | 20.63ms | 1.016× | 0.43% | PASS |
| file_reads | `index_join` | 7.87ms | 7.20ms | 0.915× | 1.90% | PASS |
| file_reads | `index_join_scan` | 3.24ms | 3.99ms | 1.233× | 0.91% | PASS |
| file_reads | `types_table_scan` | 665.39ms | 718.03ms | 1.079× | 0.61% | PASS |
| file_reads | `table_scan` | 756.65ms | 802.75ms | 1.061× | 0.70% | PASS |
| file_reads | `oltp_read_only` | 156.34ms | 111.00ms | 0.710× | 0.52% | PASS |
| file_writes | `oltp_bulk_insert` | 190.01ms | 249.12ms | 1.311× | 2.34% | PASS |
| file_writes | `oltp_insert` | 19.53ms | 38.58ms | 1.975× | 4.09% | PASS |
| file_writes | `oltp_update_index` | 141.20ms | 140.09ms | 0.992× | 3.91% | PASS |
| file_writes | `oltp_update_non_index` | 120.74ms | 105.24ms | 0.872× | 6.51% | PASS |
| file_writes | `oltp_delete_insert` | 120.53ms | 110.51ms | 0.917× | 5.67% | PASS |
| file_writes | `oltp_write_only` | 80.75ms | 88.87ms | 1.101× | 2.35% | PASS |
| file_writes | `types_delete_insert` | 72.94ms | 69.59ms | 0.954× | 6.48% | PASS |
| file_writes | `oltp_read_write` | 121.11ms | 132.90ms | 1.097× | 4.27% | PASS |
| ac_reads | `oltp_point_select` | 36.80ms | 41.43ms | 1.126× | 1.25% | PASS |
| ac_reads | `oltp_range_select` | 12.10ms | 12.71ms | 1.051× | 0.95% | PASS |
| ac_reads | `oltp_sum_range` | 11.48ms | 12.76ms | 1.112× | 1.32% | PASS |
| ac_reads | `oltp_order_range` | 2.32ms | 2.30ms | 0.991× | 1.73% | PASS |
| ac_reads | `oltp_distinct_range` | 2.83ms | 2.80ms | 0.990× | 0.81% | PASS |
| ac_reads | `oltp_index_scan` | 4.62ms | 5.58ms | 1.208× | 0.98% | PASS |
| ac_reads | `select_random_points` | 17.52ms | 19.96ms | 1.139× | 1.34% | PASS |
| ac_reads | `select_random_ranges` | 6.15ms | 6.92ms | 1.125× | 1.09% | PASS |
| ac_reads | `covering_index_scan` | 4.60ms | 4.79ms | 1.041× | 2.91% | PASS |
| ac_reads | `groupby_scan` | 20.38ms | 20.98ms | 1.029× | 1.12% | PASS |
| ac_reads | `index_join` | 6.66ms | 8.34ms | 1.252× | 4.18% | PASS |
| ac_reads | `index_join_scan` | 2.83ms | 4.02ms | 1.422× | 1.60% | PASS |
| ac_reads | `types_table_scan` | 666.40ms | 720.16ms | 1.081× | 0.84% | PASS |
| ac_reads | `table_scan` | 759.52ms | 803.21ms | 1.058× | 0.76% | PASS |
| ac_reads | `oltp_read_only` | 102.58ms | 110.76ms | 1.080× | 0.42% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.78ms | 54.28ms | 2.492× | 4.34% | PASS |
| ac_writes | `oltp_insert_ac` | 25.69ms | 74.59ms | 2.903× | 9.63% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.54ms | 76.85ms | 2.896× | 4.76% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.01ms | 62.03ms | 2.953× | 3.86% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.16ms | 70.99ms | 2.938× | 3.47% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.41ms | 72.00ms | 2.949× | 5.38% | PASS |
| ac_writes | `types_delete_insert_ac` | 25.16ms | 59.61ms | 2.369× | 6.34% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.86ms | 78.59ms | 2.926× | 4.65% | PASS |

</details>

## Version-control latency

Wall time: 2m 9s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 67.15ms | 200.00ms | 33.6% | 0.59% | PASS |
| `status_dirty_many_tables` | 69.81ms | 200.00ms | 34.9% | 0.54% | PASS |
| `diff_regular_working_one_table` | 63.48ms | 150.00ms | 42.3% | 0.72% | PASS |
| `diff_regular_working_many_tables` | 75.08ms | 200.00ms | 37.5% | 0.51% | PASS |
| `diff_stat_working_many_tables` | 75.08ms | 200.00ms | 37.5% | 0.40% | PASS |
| `diff_schema_working_many_tables` | 75.18ms | 200.00ms | 37.6% | 0.44% | PASS |
| `branch_list_many_branches` | 21.32ms | 100.00ms | 21.3% | 1.28% | PASS |
| `branch_create_delete` | 31.54ms | 100.00ms | 31.5% | 1.47% | PASS |
| `checkout_branch_clean` | 118.29ms | 200.00ms | 59.1% | 8.04% | PASS |
| `merge_data_no_conflicts` | 38.09ms | 150.00ms | 25.4% | 1.40% | PASS |
| `merge_schema_no_conflicts` | 21.67ms | 100.00ms | 21.7% | 1.31% | PASS |
| `merge_data_conflicts` | 78.59ms | 250.00ms | 31.4% | 0.30% | PASS |
| `merge_data_conflicts_with_resolve` | 78.03ms | 250.00ms | 31.2% | 0.64% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
