# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-11 12:04 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260720.247.2
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/31482205767)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 20m 0s | 9.02s | 12.20s | 1.353× | 2.05% | **PASS** |
| textpk | 69 | 55 | 1h 34m 52s | 10.80s | 12.16s | 1.126× | 1.98% | **PASS** |
| blobpk | 69 | 55 | 1h 14m 1s | 7.16s | 8.57s | 1.196× | 2.32% | **PASS** |
| compositepk | 69 | 55 | 1h 28m 36s | 10.12s | 12.29s | 1.215× | 1.79% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.31ms | 22.92ms | 1.128× | 1.65% | PASS |
| mem_reads | `oltp_range_select` | 9.28ms | 11.10ms | 1.196× | 2.34% | PASS |
| mem_reads | `oltp_sum_range` | 8.54ms | 10.37ms | 1.214× | 1.82% | PASS |
| mem_reads | `oltp_order_range` | 2.47ms | 2.75ms | 1.112× | 2.18% | PASS |
| mem_reads | `oltp_distinct_range` | 3.37ms | 3.61ms | 1.074× | 2.02% | PASS |
| mem_reads | `oltp_index_scan` | 3.50ms | 4.18ms | 1.195× | 1.90% | PASS |
| mem_reads | `select_random_points` | 9.86ms | 10.43ms | 1.057× | 1.65% | PASS |
| mem_reads | `select_random_ranges` | 2.60ms | 3.29ms | 1.265× | 1.77% | PASS |
| mem_reads | `covering_index_scan` | 3.34ms | 3.30ms | 0.987× | 2.13% | PASS |
| mem_reads | `groupby_scan` | 28.60ms | 30.78ms | 1.076× | 1.24% | PASS |
| mem_reads | `index_join` | 5.11ms | 6.96ms | 1.360× | 2.37% | PASS |
| mem_reads | `index_join_scan` | 2.79ms | 3.96ms | 1.418× | 2.28% | PASS |
| mem_reads | `types_table_scan` | 995.11ms | 1.20s | 1.201× | 0.85% | PASS |
| mem_reads | `table_scan` | 1.09s | 1.26s | 1.157× | 0.97% | PASS |
| mem_reads | `oltp_read_only` | 87.78ms | 100.99ms | 1.151× | 1.11% | PASS |
| mem_writes | `oltp_bulk_insert` | 137.46ms | 181.41ms | 1.320× | 1.18% | PASS |
| mem_writes | `oltp_insert` | 12.34ms | 21.32ms | 1.728× | 1.11% | PASS |
| mem_writes | `oltp_update_index` | 41.36ms | 83.06ms | 2.008× | 1.17% | PASS |
| mem_writes | `oltp_update_non_index` | 27.30ms | 47.02ms | 1.722× | 1.45% | PASS |
| mem_writes | `oltp_delete_insert` | 37.55ms | 61.53ms | 1.639× | 1.46% | PASS |
| mem_writes | `oltp_write_only` | 17.82ms | 35.68ms | 2.002× | 1.52% | PASS |
| mem_writes | `types_delete_insert` | 19.83ms | 31.38ms | 1.583× | 1.63% | PASS |
| mem_writes | `oltp_read_write` | 54.17ms | 86.58ms | 1.598× | 1.36% | PASS |
| file_reads | `oltp_point_select` | 49.58ms | 32.38ms | 0.653× | 2.04% | PASS |
| file_reads | `oltp_range_select` | 12.48ms | 12.39ms | 0.993× | 2.59% | PASS |
| file_reads | `oltp_sum_range` | 11.95ms | 11.82ms | 0.989× | 2.37% | PASS |
| file_reads | `oltp_order_range` | 2.84ms | 2.91ms | 1.024× | 2.05% | PASS |
| file_reads | `oltp_distinct_range` | 3.70ms | 3.74ms | 1.011× | 1.50% | PASS |
| file_reads | `oltp_index_scan` | 6.55ms | 5.46ms | 0.834× | 2.91% | PASS |
| file_reads | `select_random_points` | 13.05ms | 11.71ms | 0.897× | 1.46% | PASS |
| file_reads | `select_random_ranges` | 5.62ms | 4.34ms | 0.773× | 1.74% | PASS |
| file_reads | `covering_index_scan` | 6.49ms | 4.55ms | 0.701× | 2.39% | PASS |
| file_reads | `groupby_scan` | 28.65ms | 30.36ms | 1.059× | 1.63% | PASS |
| file_reads | `index_join` | 6.96ms | 7.87ms | 1.132× | 2.90% | PASS |
| file_reads | `index_join_scan` | 3.21ms | 4.18ms | 1.305× | 2.12% | PASS |
| file_reads | `types_table_scan` | 978.66ms | 1.18s | 1.201× | 1.15% | PASS |
| file_reads | `table_scan` | 1.10s | 1.27s | 1.157× | 0.89% | PASS |
| file_reads | `oltp_read_only` | 131.89ms | 118.51ms | 0.899× | 0.98% | PASS |
| file_writes | `oltp_bulk_insert` | 240.58ms | 259.18ms | 1.077× | 24.28% | PASS |
| file_writes | `oltp_insert` | 25.17ms | 37.42ms | 1.487× | 36.90% | PASS |
| file_writes | `oltp_update_index` | 189.45ms | 160.44ms | 0.847× | 22.82% | PASS |
| file_writes | `oltp_update_non_index` | 110.73ms | 103.52ms | 0.935× | 26.48% | PASS |
| file_writes | `oltp_delete_insert` | 114.01ms | 115.88ms | 1.016× | 14.20% | PASS |
| file_writes | `oltp_write_only` | 117.86ms | 78.66ms | 0.667× | 32.51% | PASS |
| file_writes | `types_delete_insert` | 66.38ms | 68.19ms | 1.027× | 26.66% | PASS |
| file_writes | `oltp_read_write` | 128.84ms | 175.69ms | 1.364× | 37.33% | PASS |
| ac_reads | `oltp_point_select` | 30.14ms | 32.71ms | 1.085× | 1.81% | PASS |
| ac_reads | `oltp_range_select` | 10.34ms | 12.43ms | 1.202× | 1.83% | PASS |
| ac_reads | `oltp_sum_range` | 9.84ms | 11.63ms | 1.182× | 2.24% | PASS |
| ac_reads | `oltp_order_range` | 2.59ms | 2.85ms | 1.100× | 2.04% | PASS |
| ac_reads | `oltp_distinct_range` | 3.52ms | 3.78ms | 1.073× | 2.25% | PASS |
| ac_reads | `oltp_index_scan` | 4.71ms | 5.44ms | 1.154× | 2.35% | PASS |
| ac_reads | `select_random_points` | 11.34ms | 11.72ms | 1.034× | 1.85% | PASS |
| ac_reads | `select_random_ranges` | 3.67ms | 4.27ms | 1.165× | 2.38% | PASS |
| ac_reads | `covering_index_scan` | 4.48ms | 4.52ms | 1.008× | 3.02% | PASS |
| ac_reads | `groupby_scan` | 28.45ms | 30.11ms | 1.058× | 1.31% | PASS |
| ac_reads | `index_join` | 6.00ms | 7.77ms | 1.296× | 3.24% | PASS |
| ac_reads | `index_join_scan` | 3.08ms | 4.32ms | 1.401× | 2.52% | PASS |
| ac_reads | `types_table_scan` | 979.06ms | 1.18s | 1.208× | 0.83% | PASS |
| ac_reads | `table_scan` | 1.11s | 1.29s | 1.159× | 0.79% | PASS |
| ac_reads | `oltp_read_only` | 104.11ms | 118.52ms | 1.138× | 1.15% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 100.98ms | 304.70ms | 3.017× | 62.31% | PASS |
| ac_writes | `oltp_insert_ac` | 139.09ms | 349.16ms | 2.510× | 70.66% | PASS |
| ac_writes | `oltp_update_index_ac` | 74.08ms | 244.96ms | 3.307× | 54.80% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 58.55ms | 309.14ms | 5.280× | 45.76% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 65.98ms | 344.07ms | 5.215× | 53.56% | PASS |
| ac_writes | `oltp_write_only_ac` | 67.34ms | 306.08ms | 4.545× | 38.90% | PASS |
| ac_writes | `types_delete_insert_ac` | 96.67ms | 255.99ms | 2.648× | 35.01% | PASS |
| ac_writes | `oltp_read_write_ac` | 127.33ms | 447.93ms | 3.518× | 66.44% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.66ms | 38.04ms | 1.241× | 1.58% | PASS |
| mem_reads | `oltp_range_select` | 13.68ms | 14.33ms | 1.047× | 2.37% | PASS |
| mem_reads | `oltp_sum_range` | 12.55ms | 14.43ms | 1.150× | 1.59% | PASS |
| mem_reads | `oltp_order_range` | 3.02ms | 3.20ms | 1.059× | 1.24% | PASS |
| mem_reads | `oltp_distinct_range` | 3.98ms | 4.25ms | 1.066× | 1.09% | PASS |
| mem_reads | `oltp_index_scan` | 4.60ms | 6.41ms | 1.394× | 1.90% | PASS |
| mem_reads | `select_random_points` | 18.22ms | 21.45ms | 1.177× | 2.60% | PASS |
| mem_reads | `select_random_ranges` | 4.06ms | 5.26ms | 1.293× | 1.78% | PASS |
| mem_reads | `covering_index_scan` | 5.12ms | 4.79ms | 0.935× | 4.50% | PASS |
| mem_reads | `groupby_scan` | 32.75ms | 34.62ms | 1.057× | 1.17% | PASS |
| mem_reads | `index_join` | 7.67ms | 9.87ms | 1.288× | 4.29% | PASS |
| mem_reads | `index_join_scan` | 4.70ms | 5.59ms | 1.190× | 3.32% | PASS |
| mem_reads | `types_table_scan` | 1.22s | 1.28s | 1.048× | 2.44% | PASS |
| mem_reads | `table_scan` | 1.53s | 1.43s | 0.937× | 1.22% | PASS |
| mem_reads | `oltp_read_only` | 125.90ms | 140.22ms | 1.114× | 1.98% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.81ms | 362.18ms | 1.529× | 0.98% | PASS |
| mem_writes | `oltp_insert` | 23.07ms | 40.93ms | 1.774× | 2.40% | PASS |
| mem_writes | `oltp_update_index` | 78.85ms | 142.26ms | 1.804× | 3.71% | PASS |
| mem_writes | `oltp_update_non_index` | 51.08ms | 90.96ms | 1.781× | 1.86% | PASS |
| mem_writes | `oltp_delete_insert` | 55.23ms | 108.53ms | 1.965× | 1.99% | PASS |
| mem_writes | `oltp_write_only` | 31.25ms | 64.78ms | 2.073× | 1.96% | PASS |
| mem_writes | `types_delete_insert` | 34.24ms | 57.16ms | 1.670× | 1.47% | PASS |
| mem_writes | `oltp_read_write` | 90.16ms | 143.89ms | 1.596× | 2.10% | PASS |
| file_reads | `oltp_point_select` | 106.11ms | 65.17ms | 0.614× | 1.25% | PASS |
| file_reads | `oltp_range_select` | 22.08ms | 17.29ms | 0.783× | 2.34% | PASS |
| file_reads | `oltp_sum_range` | 21.20ms | 17.47ms | 0.824× | 1.70% | PASS |
| file_reads | `oltp_order_range` | 3.92ms | 3.54ms | 0.904× | 1.22% | PASS |
| file_reads | `oltp_distinct_range` | 5.03ms | 4.64ms | 0.923× | 1.95% | PASS |
| file_reads | `oltp_index_scan` | 12.59ms | 9.25ms | 0.735× | 1.46% | PASS |
| file_reads | `select_random_points` | 27.60ms | 25.16ms | 0.912× | 2.64% | PASS |
| file_reads | `select_random_ranges` | 11.80ms | 7.97ms | 0.676× | 1.33% | PASS |
| file_reads | `covering_index_scan` | 13.30ms | 7.50ms | 0.564× | 1.35% | PASS |
| file_reads | `groupby_scan` | 33.55ms | 34.83ms | 1.038× | 1.28% | PASS |
| file_reads | `index_join` | 12.35ms | 11.47ms | 0.929× | 2.04% | PASS |
| file_reads | `index_join_scan` | 5.85ms | 5.97ms | 1.021× | 4.60% | PASS |
| file_reads | `types_table_scan` | 1.20s | 1.27s | 1.052× | 2.27% | PASS |
| file_reads | `table_scan` | 1.46s | 1.41s | 0.964× | 3.09% | PASS |
| file_reads | `oltp_read_only` | 237.79ms | 178.01ms | 0.749× | 0.95% | PASS |
| file_writes | `oltp_bulk_insert` | 256.59ms | 389.23ms | 1.517× | 0.82% | PASS |
| file_writes | `oltp_insert` | 58.96ms | 53.63ms | 0.910× | 21.47% | PASS |
| file_writes | `oltp_update_index` | 122.19ms | 175.70ms | 1.438× | 1.58% | PASS |
| file_writes | `oltp_update_non_index` | 98.75ms | 116.06ms | 1.175× | 11.06% | PASS |
| file_writes | `oltp_delete_insert` | 94.91ms | 137.00ms | 1.444× | 1.90% | PASS |
| file_writes | `oltp_write_only` | 89.03ms | 86.45ms | 0.971× | 7.50% | PASS |
| file_writes | `types_delete_insert` | 56.76ms | 75.82ms | 1.336× | 1.39% | PASS |
| file_writes | `oltp_read_write` | 138.90ms | 166.72ms | 1.200× | 7.14% | PASS |
| ac_reads | `oltp_point_select` | 55.81ms | 64.38ms | 1.154× | 1.27% | PASS |
| ac_reads | `oltp_range_select` | 17.47ms | 17.11ms | 0.979× | 2.59% | PASS |
| ac_reads | `oltp_sum_range` | 15.56ms | 17.15ms | 1.102× | 1.87% | PASS |
| ac_reads | `oltp_order_range` | 3.45ms | 3.54ms | 1.026× | 1.69% | PASS |
| ac_reads | `oltp_distinct_range` | 4.46ms | 4.60ms | 1.032× | 0.98% | PASS |
| ac_reads | `oltp_index_scan` | 7.64ms | 9.22ms | 1.206× | 1.11% | PASS |
| ac_reads | `select_random_points` | 22.24ms | 25.04ms | 1.126× | 1.81% | PASS |
| ac_reads | `select_random_ranges` | 6.93ms | 7.98ms | 1.152× | 1.74% | PASS |
| ac_reads | `covering_index_scan` | 8.45ms | 7.48ms | 0.884× | 2.04% | PASS |
| ac_reads | `groupby_scan` | 32.73ms | 34.82ms | 1.064× | 1.34% | PASS |
| ac_reads | `index_join` | 9.55ms | 11.44ms | 1.197× | 2.84% | PASS |
| ac_reads | `index_join_scan` | 5.39ms | 5.90ms | 1.093× | 4.61% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.25s | 1.126× | 2.19% | PASS |
| ac_reads | `table_scan` | 1.41s | 1.40s | 0.991× | 2.98% | PASS |
| ac_reads | `oltp_read_only` | 161.60ms | 178.68ms | 1.106× | 1.95% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.49ms | 82.09ms | 3.650× | 5.60% | PASS |
| ac_writes | `oltp_insert_ac` | 27.29ms | 101.90ms | 3.734× | 5.46% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.46ms | 117.41ms | 3.985× | 7.87% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.43ms | 97.68ms | 3.998× | 8.50% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.58ms | 109.81ms | 4.131× | 7.42% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.70ms | 109.81ms | 3.965× | 7.15% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.89ms | 101.92ms | 4.094× | 8.73% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.50ms | 116.12ms | 3.573× | 5.02% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 19.87ms | 22.39ms | 1.127× | 1.97% | PASS |
| mem_reads | `oltp_range_select` | 8.68ms | 8.47ms | 0.976× | 3.89% | PASS |
| mem_reads | `oltp_sum_range` | 8.77ms | 8.52ms | 0.972× | 4.09% | PASS |
| mem_reads | `oltp_order_range` | 1.98ms | 1.94ms | 0.978× | 2.24% | PASS |
| mem_reads | `oltp_distinct_range` | 2.49ms | 2.40ms | 0.963× | 1.69% | PASS |
| mem_reads | `oltp_index_scan` | 2.95ms | 4.15ms | 1.409× | 1.95% | PASS |
| mem_reads | `select_random_points` | 12.84ms | 14.04ms | 1.094× | 3.54% | PASS |
| mem_reads | `select_random_ranges` | 2.96ms | 3.46ms | 1.169× | 1.87% | PASS |
| mem_reads | `covering_index_scan` | 2.48ms | 3.17ms | 1.281× | 3.04% | PASS |
| mem_reads | `groupby_scan` | 19.58ms | 19.25ms | 0.983× | 2.32% | PASS |
| mem_reads | `index_join` | 4.44ms | 6.59ms | 1.484× | 6.13% | PASS |
| mem_reads | `index_join_scan` | 3.62ms | 4.99ms | 1.377× | 5.12% | PASS |
| mem_reads | `types_table_scan` | 696.46ms | 732.93ms | 1.052× | 2.07% | PASS |
| mem_reads | `table_scan` | 804.95ms | 822.04ms | 1.021× | 2.28% | PASS |
| mem_reads | `oltp_read_only` | 68.96ms | 71.97ms | 1.044× | 2.78% | PASS |
| mem_writes | `oltp_bulk_insert` | 135.38ms | 198.06ms | 1.463× | 0.96% | PASS |
| mem_writes | `oltp_insert` | 11.47ms | 24.29ms | 2.118× | 1.57% | PASS |
| mem_writes | `oltp_update_index` | 43.70ms | 89.11ms | 2.039× | 2.18% | PASS |
| mem_writes | `oltp_update_non_index` | 32.03ms | 57.11ms | 1.783× | 1.54% | PASS |
| mem_writes | `oltp_delete_insert` | 30.50ms | 67.98ms | 2.229× | 2.26% | PASS |
| mem_writes | `oltp_write_only` | 18.36ms | 43.62ms | 2.375× | 1.97% | PASS |
| mem_writes | `types_delete_insert` | 20.70ms | 36.03ms | 1.741× | 2.43% | PASS |
| mem_writes | `oltp_read_write` | 50.48ms | 82.30ms | 1.630× | 3.12% | PASS |
| file_reads | `oltp_point_select` | 73.75ms | 40.89ms | 0.554× | 1.27% | PASS |
| file_reads | `oltp_range_select` | 14.18ms | 10.15ms | 0.716× | 1.57% | PASS |
| file_reads | `oltp_sum_range` | 14.33ms | 10.14ms | 0.708× | 1.72% | PASS |
| file_reads | `oltp_order_range` | 2.66ms | 2.17ms | 0.816× | 1.53% | PASS |
| file_reads | `oltp_distinct_range` | 3.15ms | 2.63ms | 0.834× | 1.33% | PASS |
| file_reads | `oltp_index_scan` | 8.88ms | 6.13ms | 0.690× | 1.28% | PASS |
| file_reads | `select_random_points` | 17.71ms | 14.62ms | 0.825× | 2.42% | PASS |
| file_reads | `select_random_ranges` | 8.46ms | 5.34ms | 0.631× | 0.87% | PASS |
| file_reads | `covering_index_scan` | 9.02ms | 5.10ms | 0.566× | 1.78% | PASS |
| file_reads | `groupby_scan` | 19.97ms | 19.16ms | 0.960× | 1.83% | PASS |
| file_reads | `index_join` | 8.05ms | 7.53ms | 0.935× | 2.16% | PASS |
| file_reads | `index_join_scan` | 3.82ms | 4.31ms | 1.128× | 2.11% | PASS |
| file_reads | `types_table_scan` | 691.15ms | 729.32ms | 1.055× | 1.74% | PASS |
| file_reads | `table_scan` | 848.97ms | 831.34ms | 0.979× | 3.95% | PASS |
| file_reads | `oltp_read_only` | 152.72ms | 101.24ms | 0.663× | 2.48% | PASS |
| file_writes | `oltp_bulk_insert` | 200.10ms | 270.13ms | 1.350× | 9.85% | PASS |
| file_writes | `oltp_insert` | 35.76ms | 61.57ms | 1.722× | 23.83% | PASS |
| file_writes | `oltp_update_index` | 191.63ms | 204.57ms | 1.067× | 17.65% | PASS |
| file_writes | `oltp_update_non_index` | 183.63ms | 136.65ms | 0.744× | 31.25% | PASS |
| file_writes | `oltp_delete_insert` | 149.06ms | 143.01ms | 0.959× | 20.71% | PASS |
| file_writes | `oltp_write_only` | 140.06ms | 105.53ms | 0.754× | 25.66% | PASS |
| file_writes | `types_delete_insert` | 93.27ms | 81.59ms | 0.875× | 25.33% | PASS |
| file_writes | `oltp_read_write` | 137.95ms | 149.06ms | 1.081× | 15.98% | PASS |
| ac_reads | `oltp_point_select` | 38.49ms | 42.30ms | 1.099× | 2.63% | PASS |
| ac_reads | `oltp_range_select` | 10.75ms | 10.19ms | 0.947× | 2.50% | PASS |
| ac_reads | `oltp_sum_range` | 10.83ms | 10.22ms | 0.944× | 2.46% | PASS |
| ac_reads | `oltp_order_range` | 2.36ms | 2.19ms | 0.927× | 2.12% | PASS |
| ac_reads | `oltp_distinct_range` | 2.87ms | 2.64ms | 0.920× | 0.94% | PASS |
| ac_reads | `oltp_index_scan` | 5.42ms | 6.14ms | 1.134× | 1.46% | PASS |
| ac_reads | `select_random_points` | 14.56ms | 14.92ms | 1.025× | 3.08% | PASS |
| ac_reads | `select_random_ranges` | 4.98ms | 5.36ms | 1.075× | 1.44% | PASS |
| ac_reads | `covering_index_scan` | 5.48ms | 5.11ms | 0.932× | 2.14% | PASS |
| ac_reads | `groupby_scan` | 19.50ms | 19.11ms | 0.980× | 1.52% | PASS |
| ac_reads | `index_join` | 6.30ms | 7.53ms | 1.195× | 2.72% | PASS |
| ac_reads | `index_join_scan` | 3.61ms | 4.45ms | 1.232× | 2.07% | PASS |
| ac_reads | `types_table_scan` | 694.35ms | 734.67ms | 1.058× | 2.15% | PASS |
| ac_reads | `table_scan` | 813.29ms | 825.90ms | 1.016× | 2.34% | PASS |
| ac_reads | `oltp_read_only` | 95.38ms | 99.24ms | 1.040× | 2.30% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 34.41ms | 94.44ms | 2.744× | 25.91% | PASS |
| ac_writes | `oltp_insert_ac` | 41.31ms | 119.00ms | 2.880× | 35.58% | PASS |
| ac_writes | `oltp_update_index_ac` | 37.86ms | 148.06ms | 3.910× | 35.53% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 54.53ms | 267.95ms | 4.914× | 60.59% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 114.85ms | 295.76ms | 2.575× | 69.56% | PASS |
| ac_writes | `oltp_write_only_ac` | 42.04ms | 170.86ms | 4.064× | 67.10% | PASS |
| ac_writes | `types_delete_insert_ac` | 44.15ms | 176.12ms | 3.989× | 61.92% | PASS |
| ac_writes | `oltp_read_write_ac` | 54.04ms | 241.68ms | 4.472× | 55.36% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 36.03ms | 42.15ms | 1.170× | 1.79% | PASS |
| mem_reads | `oltp_range_select` | 20.47ms | 21.79ms | 1.064× | 2.47% | PASS |
| mem_reads | `oltp_sum_range` | 19.06ms | 21.40ms | 1.123× | 1.29% | PASS |
| mem_reads | `oltp_order_range` | 3.66ms | 3.97ms | 1.085× | 0.89% | PASS |
| mem_reads | `oltp_distinct_range` | 4.76ms | 5.03ms | 1.058× | 0.93% | PASS |
| mem_reads | `oltp_index_scan` | 4.91ms | 6.89ms | 1.404× | 2.35% | PASS |
| mem_reads | `select_random_points` | 30.61ms | 33.69ms | 1.101× | 1.39% | PASS |
| mem_reads | `select_random_ranges` | 8.02ms | 9.25ms | 1.153× | 1.09% | PASS |
| mem_reads | `covering_index_scan` | 4.31ms | 4.63ms | 1.076× | 2.20% | PASS |
| mem_reads | `groupby_scan` | 37.02ms | 39.70ms | 1.072× | 1.01% | PASS |
| mem_reads | `index_join` | 8.31ms | 10.85ms | 1.306× | 2.08% | PASS |
| mem_reads | `index_join_scan` | 4.33ms | 5.61ms | 1.296× | 1.76% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.24s | 1.181× | 1.52% | PASS |
| mem_reads | `table_scan` | 1.30s | 1.41s | 1.086× | 2.33% | PASS |
| mem_reads | `oltp_read_only` | 160.42ms | 175.59ms | 1.095× | 1.70% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.95ms | 364.27ms | 1.457× | 0.98% | PASS |
| mem_writes | `oltp_insert` | 19.44ms | 36.85ms | 1.896× | 0.86% | PASS |
| mem_writes | `oltp_update_index` | 70.29ms | 120.11ms | 1.709× | 2.13% | PASS |
| mem_writes | `oltp_update_non_index` | 52.76ms | 84.70ms | 1.606× | 1.64% | PASS |
| mem_writes | `oltp_delete_insert` | 50.88ms | 97.36ms | 1.914× | 1.55% | PASS |
| mem_writes | `oltp_write_only` | 27.62ms | 57.94ms | 2.098× | 1.69% | PASS |
| mem_writes | `types_delete_insert` | 33.50ms | 55.52ms | 1.657× | 1.66% | PASS |
| mem_writes | `oltp_read_write` | 104.23ms | 157.38ms | 1.510× | 1.99% | PASS |
| file_reads | `oltp_point_select` | 108.46ms | 69.05ms | 0.637× | 1.10% | PASS |
| file_reads | `oltp_range_select` | 27.81ms | 24.69ms | 0.888× | 1.80% | PASS |
| file_reads | `oltp_sum_range` | 26.57ms | 24.54ms | 0.924× | 1.85% | PASS |
| file_reads | `oltp_order_range` | 4.53ms | 4.39ms | 0.968× | 1.54% | PASS |
| file_reads | `oltp_distinct_range` | 5.72ms | 5.46ms | 0.954× | 1.28% | PASS |
| file_reads | `oltp_index_scan` | 12.49ms | 9.59ms | 0.768× | 1.64% | PASS |
| file_reads | `select_random_points` | 38.53ms | 37.30ms | 0.968× | 1.97% | PASS |
| file_reads | `select_random_ranges` | 15.76ms | 12.45ms | 0.790× | 1.40% | PASS |
| file_reads | `covering_index_scan` | 11.83ms | 7.41ms | 0.627× | 1.70% | PASS |
| file_reads | `groupby_scan` | 37.76ms | 40.22ms | 1.065× | 0.68% | PASS |
| file_reads | `index_join` | 12.68ms | 13.31ms | 1.049× | 2.36% | PASS |
| file_reads | `index_join_scan` | 5.38ms | 6.20ms | 1.152× | 2.25% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.24s | 1.182× | 1.31% | PASS |
| file_reads | `table_scan` | 1.39s | 1.44s | 1.033× | 1.64% | PASS |
| file_reads | `oltp_read_only` | 273.41ms | 218.50ms | 0.799× | 0.89% | PASS |
| file_writes | `oltp_bulk_insert` | 265.34ms | 387.32ms | 1.460× | 0.98% | PASS |
| file_writes | `oltp_insert` | 28.14ms | 47.69ms | 1.695× | 2.99% | PASS |
| file_writes | `oltp_update_index` | 103.90ms | 149.76ms | 1.441× | 2.36% | PASS |
| file_writes | `oltp_update_non_index` | 80.56ms | 107.06ms | 1.329× | 1.79% | PASS |
| file_writes | `oltp_delete_insert` | 80.97ms | 123.25ms | 1.522× | 1.97% | PASS |
| file_writes | `oltp_write_only` | 54.99ms | 80.53ms | 1.465× | 2.00% | PASS |
| file_writes | `types_delete_insert` | 53.43ms | 71.89ms | 1.345× | 2.05% | PASS |
| file_writes | `oltp_read_write` | 138.55ms | 183.85ms | 1.327× | 1.97% | PASS |
| ac_reads | `oltp_point_select` | 60.35ms | 69.34ms | 1.149× | 1.32% | PASS |
| ac_reads | `oltp_range_select` | 23.34ms | 24.78ms | 1.062× | 1.74% | PASS |
| ac_reads | `oltp_sum_range` | 21.25ms | 24.48ms | 1.152× | 1.56% | PASS |
| ac_reads | `oltp_order_range` | 4.01ms | 4.38ms | 1.091× | 2.14% | PASS |
| ac_reads | `oltp_distinct_range` | 5.15ms | 5.47ms | 1.062× | 1.23% | PASS |
| ac_reads | `oltp_index_scan` | 7.34ms | 9.46ms | 1.288× | 2.30% | PASS |
| ac_reads | `select_random_points` | 32.20ms | 36.94ms | 1.147× | 2.60% | PASS |
| ac_reads | `select_random_ranges` | 10.46ms | 12.28ms | 1.173× | 1.11% | PASS |
| ac_reads | `covering_index_scan` | 6.88ms | 7.24ms | 1.053× | 1.81% | PASS |
| ac_reads | `groupby_scan` | 36.86ms | 39.89ms | 1.082× | 1.00% | PASS |
| ac_reads | `index_join` | 9.82ms | 12.96ms | 1.320× | 1.74% | PASS |
| ac_reads | `index_join_scan` | 4.71ms | 6.12ms | 1.299× | 2.39% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.26s | 1.132× | 3.17% | PASS |
| ac_reads | `table_scan` | 1.25s | 1.41s | 1.127× | 2.47% | PASS |
| ac_reads | `oltp_read_only` | 192.90ms | 216.49ms | 1.122× | 1.70% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 25.20ms | 86.01ms | 3.413× | 6.99% | PASS |
| ac_writes | `oltp_insert_ac` | 26.55ms | 107.96ms | 4.066× | 7.56% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.43ms | 119.80ms | 4.214× | 7.20% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.26ms | 96.70ms | 4.158× | 5.11% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.09ms | 106.26ms | 4.235× | 4.85% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.27ms | 105.06ms | 4.158× | 5.67% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.90ms | 101.16ms | 4.418× | 8.04% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.22ms | 115.46ms | 3.584× | 3.32% | PASS |

</details>

## Version-control latency

Wall time: 1m 51s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 56.09ms | 200.00ms | 28.0% | 1.28% | PASS |
| `status_dirty_many_tables` | 58.30ms | 200.00ms | 29.1% | 1.22% | PASS |
| `diff_regular_working_one_table` | 52.44ms | 150.00ms | 35.0% | 1.06% | PASS |
| `diff_regular_working_many_tables` | 62.67ms | 200.00ms | 31.3% | 1.71% | PASS |
| `diff_stat_working_many_tables` | 62.73ms | 200.00ms | 31.4% | 1.31% | PASS |
| `diff_schema_working_many_tables` | 62.68ms | 200.00ms | 31.3% | 0.94% | PASS |
| `branch_list_many_branches` | 17.44ms | 100.00ms | 17.4% | 1.75% | PASS |
| `branch_create_delete` | 19.89ms | 100.00ms | 19.9% | 3.37% | PASS |
| `checkout_branch_clean` | 85.17ms | 200.00ms | 42.6% | 11.72% | PASS |
| `merge_data_no_conflicts` | 30.18ms | 150.00ms | 20.1% | 20.92% | PASS |
| `merge_schema_no_conflicts` | 20.71ms | 100.00ms | 20.7% | 13.45% | PASS |
| `merge_data_conflicts` | 66.15ms | 250.00ms | 26.5% | 1.47% | PASS |
| `merge_data_conflicts_with_resolve` | 65.66ms | 250.00ms | 26.3% | 1.04% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
