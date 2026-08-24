# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-24 11:47 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260816.277.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/32715543356)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 40s | 8.84s | 11.50s | 1.302× | 1.47% | **PASS** |
| textpk | 69 | 55 | 1h 32m 25s | 10.61s | 11.76s | 1.108× | 1.32% | **PASS** |
| blobpk | 69 | 55 | 1h 31m 45s | 9.87s | 11.96s | 1.212× | 1.70% | **PASS** |
| compositepk | 69 | 55 | 1h 19m 33s | 9.17s | 13.14s | 1.433× | 1.87% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.62ms | 30.77ms | 1.201× | 2.16% | PASS |
| mem_reads | `oltp_range_select` | 11.36ms | 13.80ms | 1.215× | 2.16% | PASS |
| mem_reads | `oltp_sum_range` | 10.63ms | 13.01ms | 1.224× | 1.83% | PASS |
| mem_reads | `oltp_order_range` | 2.54ms | 3.01ms | 1.185× | 1.44% | PASS |
| mem_reads | `oltp_distinct_range` | 3.61ms | 4.03ms | 1.115× | 0.98% | PASS |
| mem_reads | `oltp_index_scan` | 3.90ms | 5.29ms | 1.357× | 1.62% | PASS |
| mem_reads | `select_random_points` | 9.58ms | 11.04ms | 1.152× | 1.89% | PASS |
| mem_reads | `select_random_ranges` | 2.90ms | 3.96ms | 1.365× | 1.54% | PASS |
| mem_reads | `covering_index_scan` | 4.22ms | 4.11ms | 0.975× | 0.94% | PASS |
| mem_reads | `groupby_scan` | 29.54ms | 32.71ms | 1.107× | 0.74% | PASS |
| mem_reads | `index_join` | 6.08ms | 8.12ms | 1.335× | 1.40% | PASS |
| mem_reads | `index_join_scan` | 3.47ms | 4.70ms | 1.353× | 2.99% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.34s | 1.281× | 1.49% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.40s | 1.193× | 0.86% | PASS |
| mem_reads | `oltp_read_only` | 111.23ms | 128.16ms | 1.152× | 1.44% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.24ms | 253.45ms | 1.406× | 0.99% | PASS |
| mem_writes | `oltp_insert` | 15.48ms | 28.57ms | 1.846× | 0.81% | PASS |
| mem_writes | `oltp_update_index` | 49.93ms | 104.55ms | 2.094× | 1.00% | PASS |
| mem_writes | `oltp_update_non_index` | 34.14ms | 59.77ms | 1.751× | 1.57% | PASS |
| mem_writes | `oltp_delete_insert` | 44.20ms | 78.39ms | 1.774× | 1.15% | PASS |
| mem_writes | `oltp_write_only` | 21.43ms | 44.79ms | 2.090× | 1.07% | PASS |
| mem_writes | `types_delete_insert` | 24.45ms | 40.33ms | 1.649× | 1.35% | PASS |
| mem_writes | `oltp_read_write` | 66.75ms | 111.25ms | 1.667× | 1.30% | PASS |
| file_reads | `oltp_point_select` | 97.72ms | 55.41ms | 0.567× | 0.74% | PASS |
| file_reads | `oltp_range_select` | 17.93ms | 15.96ms | 0.890× | 1.94% | PASS |
| file_reads | `oltp_sum_range` | 17.36ms | 15.28ms | 0.881× | 2.03% | PASS |
| file_reads | `oltp_order_range` | 3.44ms | 3.34ms | 0.971× | 2.55% | PASS |
| file_reads | `oltp_distinct_range` | 4.52ms | 4.44ms | 0.983× | 2.27% | PASS |
| file_reads | `oltp_index_scan` | 11.58ms | 8.23ms | 0.711× | 1.23% | PASS |
| file_reads | `select_random_points` | 17.77ms | 13.95ms | 0.785× | 3.13% | PASS |
| file_reads | `select_random_ranges` | 10.50ms | 6.62ms | 0.631× | 1.28% | PASS |
| file_reads | `covering_index_scan` | 11.94ms | 6.95ms | 0.582× | 0.96% | PASS |
| file_reads | `groupby_scan` | 30.63ms | 33.27ms | 1.086× | 0.99% | PASS |
| file_reads | `index_join` | 10.34ms | 10.02ms | 0.969× | 1.79% | PASS |
| file_reads | `index_join_scan` | 4.43ms | 5.04ms | 1.138× | 2.41% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.32s | 1.279× | 0.44% | PASS |
| file_reads | `table_scan` | 1.18s | 1.39s | 1.184× | 1.15% | PASS |
| file_reads | `oltp_read_only` | 210.30ms | 161.18ms | 0.766× | 0.86% | PASS |
| file_writes | `oltp_bulk_insert` | 194.44ms | 272.55ms | 1.402× | 1.42% | PASS |
| file_writes | `oltp_insert` | 21.94ms | 35.80ms | 1.632× | 1.47% | PASS |
| file_writes | `oltp_update_index` | 76.51ms | 127.90ms | 1.672× | 1.50% | PASS |
| file_writes | `oltp_update_non_index` | 57.75ms | 81.38ms | 1.409× | 1.76% | PASS |
| file_writes | `oltp_delete_insert` | 69.86ms | 102.50ms | 1.467× | 1.67% | PASS |
| file_writes | `oltp_write_only` | 46.82ms | 67.24ms | 1.436× | 2.23% | PASS |
| file_writes | `types_delete_insert` | 40.28ms | 53.75ms | 1.335× | 1.57% | PASS |
| file_writes | `oltp_read_write` | 91.95ms | 131.03ms | 1.425× | 1.34% | PASS |
| ac_reads | `oltp_point_select` | 48.40ms | 55.61ms | 1.149× | 1.42% | PASS |
| ac_reads | `oltp_range_select` | 12.99ms | 15.92ms | 1.225× | 1.14% | PASS |
| ac_reads | `oltp_sum_range` | 12.33ms | 15.15ms | 1.229× | 1.03% | PASS |
| ac_reads | `oltp_order_range` | 2.97ms | 3.36ms | 1.129× | 2.67% | PASS |
| ac_reads | `oltp_distinct_range` | 4.00ms | 4.46ms | 1.115× | 1.48% | PASS |
| ac_reads | `oltp_index_scan` | 6.62ms | 8.26ms | 1.248× | 1.05% | PASS |
| ac_reads | `select_random_points` | 12.97ms | 13.99ms | 1.079× | 1.48% | PASS |
| ac_reads | `select_random_ranges` | 5.56ms | 6.63ms | 1.194× | 1.10% | PASS |
| ac_reads | `covering_index_scan` | 7.06ms | 7.02ms | 0.994× | 2.17% | PASS |
| ac_reads | `groupby_scan` | 30.17ms | 33.34ms | 1.105× | 1.06% | PASS |
| ac_reads | `index_join` | 7.66ms | 10.02ms | 1.308× | 1.33% | PASS |
| ac_reads | `index_join_scan` | 3.90ms | 5.04ms | 1.294× | 2.05% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.33s | 1.277× | 0.60% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.39s | 1.191× | 0.86% | PASS |
| ac_reads | `oltp_read_only` | 139.17ms | 161.46ms | 1.160× | 0.96% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.89ms | 80.48ms | 3.676× | 3.64% | PASS |
| ac_writes | `oltp_insert_ac` | 24.53ms | 98.28ms | 4.007× | 4.68% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.76ms | 113.09ms | 4.226× | 5.17% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.94ms | 91.11ms | 3.972× | 5.26% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.74ms | 104.00ms | 4.203× | 5.28% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.70ms | 103.41ms | 4.024× | 5.71% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.41ms | 93.89ms | 4.189× | 7.22% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.59ms | 110.84ms | 3.745× | 3.51% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.09ms | 35.04ms | 1.164× | 1.77% | PASS |
| mem_reads | `oltp_range_select` | 14.63ms | 13.75ms | 0.940× | 2.72% | PASS |
| mem_reads | `oltp_sum_range` | 12.82ms | 13.57ms | 1.058× | 1.33% | PASS |
| mem_reads | `oltp_order_range` | 3.23ms | 3.19ms | 0.986× | 1.13% | PASS |
| mem_reads | `oltp_distinct_range` | 4.24ms | 4.19ms | 0.988× | 0.69% | PASS |
| mem_reads | `oltp_index_scan` | 4.76ms | 6.16ms | 1.295× | 1.33% | PASS |
| mem_reads | `select_random_points` | 18.66ms | 20.55ms | 1.102× | 1.64% | PASS |
| mem_reads | `select_random_ranges` | 4.28ms | 5.27ms | 1.231× | 1.02% | PASS |
| mem_reads | `covering_index_scan` | 5.20ms | 4.74ms | 0.911× | 2.81% | PASS |
| mem_reads | `groupby_scan` | 34.40ms | 35.69ms | 1.037× | 1.08% | PASS |
| mem_reads | `index_join` | 7.04ms | 8.93ms | 1.267× | 2.33% | PASS |
| mem_reads | `index_join_scan` | 4.75ms | 5.45ms | 1.148× | 1.14% | PASS |
| mem_reads | `types_table_scan` | 1.15s | 1.25s | 1.089× | 0.61% | PASS |
| mem_reads | `table_scan` | 1.35s | 1.36s | 1.009× | 0.83% | PASS |
| mem_reads | `oltp_read_only` | 122.65ms | 129.31ms | 1.054× | 2.09% | PASS |
| mem_writes | `oltp_bulk_insert` | 231.72ms | 337.31ms | 1.456× | 0.96% | PASS |
| mem_writes | `oltp_insert` | 22.62ms | 39.66ms | 1.753× | 1.26% | PASS |
| mem_writes | `oltp_update_index` | 75.59ms | 138.75ms | 1.836× | 1.80% | PASS |
| mem_writes | `oltp_update_non_index` | 51.91ms | 89.95ms | 1.733× | 1.41% | PASS |
| mem_writes | `oltp_delete_insert` | 53.48ms | 106.76ms | 1.996× | 1.25% | PASS |
| mem_writes | `oltp_write_only` | 29.77ms | 63.28ms | 2.126× | 0.93% | PASS |
| mem_writes | `types_delete_insert` | 33.20ms | 53.91ms | 1.624× | 0.89% | PASS |
| mem_writes | `oltp_read_write` | 83.80ms | 135.00ms | 1.611× | 0.81% | PASS |
| file_reads | `oltp_point_select` | 125.85ms | 67.19ms | 0.534× | 0.71% | PASS |
| file_reads | `oltp_range_select` | 23.78ms | 16.93ms | 0.712× | 2.25% | PASS |
| file_reads | `oltp_sum_range` | 23.10ms | 17.02ms | 0.736× | 1.38% | PASS |
| file_reads | `oltp_order_range` | 4.27ms | 3.55ms | 0.832× | 1.02% | PASS |
| file_reads | `oltp_distinct_range` | 5.32ms | 4.57ms | 0.859× | 1.01% | PASS |
| file_reads | `oltp_index_scan` | 14.80ms | 9.49ms | 0.641× | 1.06% | PASS |
| file_reads | `select_random_points` | 29.50ms | 24.06ms | 0.816× | 2.33% | PASS |
| file_reads | `select_random_ranges` | 13.83ms | 8.48ms | 0.614× | 1.28% | PASS |
| file_reads | `covering_index_scan` | 15.39ms | 8.01ms | 0.521× | 2.28% | PASS |
| file_reads | `groupby_scan` | 35.00ms | 35.91ms | 1.026× | 1.08% | PASS |
| file_reads | `index_join` | 12.38ms | 11.39ms | 0.920× | 1.28% | PASS |
| file_reads | `index_join_scan` | 5.99ms | 6.09ms | 1.017× | 1.80% | PASS |
| file_reads | `types_table_scan` | 1.21s | 1.26s | 1.047× | 2.53% | PASS |
| file_reads | `table_scan` | 1.56s | 1.40s | 0.901× | 1.71% | PASS |
| file_reads | `oltp_read_only` | 270.36ms | 177.87ms | 0.658× | 1.08% | PASS |
| file_writes | `oltp_bulk_insert` | 254.25ms | 367.41ms | 1.445× | 0.89% | PASS |
| file_writes | `oltp_insert` | 46.61ms | 53.21ms | 1.141× | 18.55% | PASS |
| file_writes | `oltp_update_index` | 122.45ms | 177.44ms | 1.449× | 1.32% | PASS |
| file_writes | `oltp_update_non_index` | 102.08ms | 117.11ms | 1.147× | 13.80% | PASS |
| file_writes | `oltp_delete_insert` | 93.19ms | 138.41ms | 1.485× | 1.36% | PASS |
| file_writes | `oltp_write_only` | 86.77ms | 89.17ms | 1.028× | 11.76% | PASS |
| file_writes | `types_delete_insert` | 56.41ms | 75.31ms | 1.335× | 1.56% | PASS |
| file_writes | `oltp_read_write` | 140.88ms | 160.35ms | 1.138× | 4.50% | PASS |
| ac_reads | `oltp_point_select` | 61.82ms | 67.22ms | 1.087× | 1.15% | PASS |
| ac_reads | `oltp_range_select` | 18.17ms | 16.98ms | 0.935× | 1.76% | PASS |
| ac_reads | `oltp_sum_range` | 16.24ms | 16.68ms | 1.027× | 1.41% | PASS |
| ac_reads | `oltp_order_range` | 3.66ms | 3.54ms | 0.969× | 1.23% | PASS |
| ac_reads | `oltp_distinct_range` | 4.65ms | 4.54ms | 0.976× | 0.92% | PASS |
| ac_reads | `oltp_index_scan` | 8.28ms | 9.41ms | 1.137× | 0.80% | PASS |
| ac_reads | `select_random_points` | 22.00ms | 23.34ms | 1.061× | 1.10% | PASS |
| ac_reads | `select_random_ranges` | 7.47ms | 8.46ms | 1.132× | 0.70% | PASS |
| ac_reads | `covering_index_scan` | 9.23ms | 7.98ms | 0.865× | 0.80% | PASS |
| ac_reads | `groupby_scan` | 34.33ms | 35.78ms | 1.042× | 0.50% | PASS |
| ac_reads | `index_join` | 9.62ms | 11.41ms | 1.186× | 1.59% | PASS |
| ac_reads | `index_join_scan` | 5.49ms | 6.08ms | 1.107× | 1.31% | PASS |
| ac_reads | `types_table_scan` | 1.15s | 1.24s | 1.080× | 0.56% | PASS |
| ac_reads | `table_scan` | 1.35s | 1.36s | 1.002× | 0.69% | PASS |
| ac_reads | `oltp_read_only` | 167.20ms | 174.85ms | 1.046× | 0.81% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.91ms | 65.66ms | 3.884× | 4.94% | PASS |
| ac_writes | `oltp_insert_ac` | 20.32ms | 83.26ms | 4.097× | 5.88% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.56ms | 100.59ms | 4.665× | 3.58% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.69ms | 78.95ms | 4.731× | 3.83% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 19.00ms | 90.82ms | 4.781× | 4.38% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.14ms | 88.37ms | 4.617× | 4.55% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.97ms | 79.82ms | 4.999× | 5.62% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.84ms | 97.74ms | 3.782× | 3.35% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.72ms | 36.86ms | 1.200× | 1.47% | PASS |
| mem_reads | `oltp_range_select` | 13.57ms | 14.02ms | 1.034× | 2.87% | PASS |
| mem_reads | `oltp_sum_range` | 12.28ms | 13.94ms | 1.135× | 2.59% | PASS |
| mem_reads | `oltp_order_range` | 2.91ms | 3.14ms | 1.079× | 1.13% | PASS |
| mem_reads | `oltp_distinct_range` | 4.00ms | 4.22ms | 1.055× | 0.93% | PASS |
| mem_reads | `oltp_index_scan` | 4.59ms | 6.29ms | 1.370× | 2.00% | PASS |
| mem_reads | `select_random_points` | 18.10ms | 20.73ms | 1.145× | 2.72% | PASS |
| mem_reads | `select_random_ranges` | 4.16ms | 5.18ms | 1.245× | 1.11% | PASS |
| mem_reads | `covering_index_scan` | 4.46ms | 4.59ms | 1.030× | 1.52% | PASS |
| mem_reads | `groupby_scan` | 31.89ms | 33.72ms | 1.058× | 0.93% | PASS |
| mem_reads | `index_join` | 6.89ms | 9.35ms | 1.358× | 1.45% | PASS |
| mem_reads | `index_join_scan` | 4.41ms | 5.30ms | 1.203× | 2.27% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.25s | 1.158× | 2.88% | PASS |
| mem_reads | `table_scan` | 1.30s | 1.39s | 1.068× | 2.20% | PASS |
| mem_reads | `oltp_read_only` | 122.07ms | 137.51ms | 1.126× | 1.72% | PASS |
| mem_writes | `oltp_bulk_insert` | 241.59ms | 355.77ms | 1.473× | 0.95% | PASS |
| mem_writes | `oltp_insert` | 20.45ms | 40.38ms | 1.975× | 1.47% | PASS |
| mem_writes | `oltp_update_index` | 71.31ms | 134.81ms | 1.891× | 1.73% | PASS |
| mem_writes | `oltp_update_non_index` | 49.33ms | 85.62ms | 1.736× | 1.08% | PASS |
| mem_writes | `oltp_delete_insert` | 50.09ms | 105.38ms | 2.104× | 1.70% | PASS |
| mem_writes | `oltp_write_only` | 28.23ms | 62.69ms | 2.220× | 1.22% | PASS |
| mem_writes | `types_delete_insert` | 33.12ms | 54.98ms | 1.660× | 1.50% | PASS |
| mem_writes | `oltp_read_write` | 85.42ms | 140.24ms | 1.642× | 1.63% | PASS |
| file_reads | `oltp_point_select` | 106.24ms | 62.90ms | 0.592× | 0.99% | PASS |
| file_reads | `oltp_range_select` | 21.25ms | 16.76ms | 0.789× | 2.40% | PASS |
| file_reads | `oltp_sum_range` | 20.46ms | 16.75ms | 0.819× | 1.94% | PASS |
| file_reads | `oltp_order_range` | 3.82ms | 3.49ms | 0.913× | 1.39% | PASS |
| file_reads | `oltp_distinct_range` | 4.94ms | 4.61ms | 0.934× | 1.13% | PASS |
| file_reads | `oltp_index_scan` | 12.67ms | 9.17ms | 0.724× | 1.36% | PASS |
| file_reads | `select_random_points` | 27.92ms | 24.79ms | 0.888× | 1.98% | PASS |
| file_reads | `select_random_ranges` | 11.91ms | 7.83ms | 0.657× | 0.97% | PASS |
| file_reads | `covering_index_scan` | 12.84ms | 7.33ms | 0.571× | 1.99% | PASS |
| file_reads | `groupby_scan` | 33.21ms | 34.38ms | 1.035× | 0.73% | PASS |
| file_reads | `index_join` | 11.97ms | 11.36ms | 0.949× | 2.24% | PASS |
| file_reads | `index_join_scan` | 5.46ms | 5.78ms | 1.058× | 2.55% | PASS |
| file_reads | `types_table_scan` | 1.08s | 1.24s | 1.151× | 2.09% | PASS |
| file_reads | `table_scan` | 1.29s | 1.39s | 1.085× | 2.63% | PASS |
| file_reads | `oltp_read_only` | 232.04ms | 175.16ms | 0.755× | 0.77% | PASS |
| file_writes | `oltp_bulk_insert` | 261.69ms | 380.21ms | 1.453× | 0.80% | PASS |
| file_writes | `oltp_insert` | 32.62ms | 52.83ms | 1.620× | 2.43% | PASS |
| file_writes | `oltp_update_index` | 107.57ms | 165.61ms | 1.540× | 1.29% | PASS |
| file_writes | `oltp_update_non_index` | 80.40ms | 109.27ms | 1.359× | 1.52% | PASS |
| file_writes | `oltp_delete_insert` | 83.37ms | 132.01ms | 1.583× | 1.19% | PASS |
| file_writes | `oltp_write_only` | 57.67ms | 85.59ms | 1.484× | 1.22% | PASS |
| file_writes | `types_delete_insert` | 53.94ms | 74.00ms | 1.372× | 1.50% | PASS |
| file_writes | `oltp_read_write` | 119.98ms | 163.98ms | 1.367× | 1.35% | PASS |
| ac_reads | `oltp_point_select` | 56.56ms | 63.38ms | 1.121× | 0.86% | PASS |
| ac_reads | `oltp_range_select` | 16.79ms | 16.85ms | 1.004× | 1.73% | PASS |
| ac_reads | `oltp_sum_range` | 15.64ms | 16.80ms | 1.074× | 1.46% | PASS |
| ac_reads | `oltp_order_range` | 3.33ms | 3.50ms | 1.052× | 1.54% | PASS |
| ac_reads | `oltp_distinct_range` | 4.43ms | 4.61ms | 1.041× | 0.95% | PASS |
| ac_reads | `oltp_index_scan` | 7.62ms | 9.13ms | 1.198× | 1.91% | PASS |
| ac_reads | `select_random_points` | 22.39ms | 24.40ms | 1.090× | 2.05% | PASS |
| ac_reads | `select_random_ranges` | 6.95ms | 7.81ms | 1.123× | 1.35% | PASS |
| ac_reads | `covering_index_scan` | 7.83ms | 7.33ms | 0.936× | 1.79% | PASS |
| ac_reads | `groupby_scan` | 32.30ms | 34.28ms | 1.061× | 0.80% | PASS |
| ac_reads | `index_join` | 9.25ms | 11.24ms | 1.215× | 2.73% | PASS |
| ac_reads | `index_join_scan` | 4.92ms | 5.80ms | 1.179× | 1.94% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.25s | 1.122× | 2.70% | PASS |
| ac_reads | `table_scan` | 1.31s | 1.40s | 1.067× | 3.45% | PASS |
| ac_reads | `oltp_read_only` | 160.55ms | 175.79ms | 1.095× | 1.33% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.30ms | 84.70ms | 3.485× | 6.12% | PASS |
| ac_writes | `oltp_insert_ac` | 25.66ms | 107.93ms | 4.206× | 5.98% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.08ms | 119.07ms | 4.241× | 6.85% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.58ms | 97.82ms | 3.979× | 6.30% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.18ms | 107.59ms | 4.272× | 4.22% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.29ms | 106.65ms | 3.908× | 5.26% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.35ms | 102.45ms | 4.388× | 8.05% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.13ms | 116.23ms | 3.508× | 5.33% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.16ms | 23.27ms | 1.154× | 1.23% | PASS |
| mem_reads | `oltp_range_select` | 13.25ms | 13.20ms | 0.997× | 3.05% | PASS |
| mem_reads | `oltp_sum_range` | 13.05ms | 12.47ms | 0.955× | 1.77% | PASS |
| mem_reads | `oltp_order_range` | 2.42ms | 2.47ms | 1.021× | 2.08% | PASS |
| mem_reads | `oltp_distinct_range` | 3.14ms | 3.11ms | 0.988× | 1.25% | PASS |
| mem_reads | `oltp_index_scan` | 2.98ms | 3.75ms | 1.259× | 1.81% | PASS |
| mem_reads | `select_random_points` | 19.73ms | 21.60ms | 1.095× | 1.28% | PASS |
| mem_reads | `select_random_ranges` | 4.96ms | 5.64ms | 1.137× | 0.90% | PASS |
| mem_reads | `covering_index_scan` | 2.48ms | 2.57ms | 1.035× | 1.59% | PASS |
| mem_reads | `groupby_scan` | 23.07ms | 24.64ms | 1.068× | 0.58% | PASS |
| mem_reads | `index_join` | 5.66ms | 7.18ms | 1.269× | 1.63% | PASS |
| mem_reads | `index_join_scan` | 2.90ms | 4.39ms | 1.516× | 2.61% | PASS |
| mem_reads | `types_table_scan` | 752.65ms | 846.17ms | 1.124× | 0.67% | PASS |
| mem_reads | `table_scan` | 868.61ms | 916.80ms | 1.055× | 2.11% | PASS |
| mem_reads | `oltp_read_only` | 96.24ms | 101.86ms | 1.058× | 0.96% | PASS |
| mem_writes | `oltp_bulk_insert` | 139.43ms | 198.47ms | 1.423× | 1.06% | PASS |
| mem_writes | `oltp_insert` | 11.64ms | 21.44ms | 1.842× | 0.90% | PASS |
| mem_writes | `oltp_update_index` | 44.30ms | 75.53ms | 1.705× | 1.16% | PASS |
| mem_writes | `oltp_update_non_index` | 31.58ms | 50.84ms | 1.610× | 1.71% | PASS |
| mem_writes | `oltp_delete_insert` | 32.40ms | 59.39ms | 1.833× | 1.22% | PASS |
| mem_writes | `oltp_write_only` | 17.80ms | 35.52ms | 1.996× | 2.38% | PASS |
| mem_writes | `types_delete_insert` | 20.20ms | 32.66ms | 1.617× | 1.71% | PASS |
| mem_writes | `oltp_read_write` | 61.78ms | 90.28ms | 1.461× | 0.76% | PASS |
| file_reads | `oltp_point_select` | 42.28ms | 30.96ms | 0.732× | 0.87% | PASS |
| file_reads | `oltp_range_select` | 16.36ms | 14.41ms | 0.881× | 2.26% | PASS |
| file_reads | `oltp_sum_range` | 15.89ms | 13.76ms | 0.866× | 1.87% | PASS |
| file_reads | `oltp_order_range` | 2.82ms | 2.68ms | 0.950× | 2.01% | PASS |
| file_reads | `oltp_distinct_range` | 3.37ms | 3.25ms | 0.965× | 3.24% | PASS |
| file_reads | `oltp_index_scan` | 5.18ms | 4.42ms | 0.852× | 1.59% | PASS |
| file_reads | `select_random_points` | 21.25ms | 21.76ms | 1.024× | 1.23% | PASS |
| file_reads | `select_random_ranges` | 6.96ms | 6.07ms | 0.872× | 2.08% | PASS |
| file_reads | `covering_index_scan` | 4.70ms | 3.25ms | 0.690× | 0.88% | PASS |
| file_reads | `groupby_scan` | 22.99ms | 24.63ms | 1.071× | 0.60% | PASS |
| file_reads | `index_join` | 6.59ms | 7.36ms | 1.116× | 1.21% | PASS |
| file_reads | `index_join_scan` | 2.82ms | 3.81ms | 1.352× | 4.99% | PASS |
| file_reads | `types_table_scan` | 749.03ms | 843.55ms | 1.126× | 0.37% | PASS |
| file_reads | `table_scan` | 857.92ms | 912.11ms | 1.063× | 1.10% | PASS |
| file_reads | `oltp_read_only` | 121.45ms | 110.53ms | 0.910× | 0.57% | PASS |
| file_writes | `oltp_bulk_insert` | 272.10ms | 286.19ms | 1.052× | 28.99% | PASS |
| file_writes | `oltp_insert` | 22.09ms | 43.99ms | 1.991× | 42.44% | PASS |
| file_writes | `oltp_update_index` | 155.73ms | 156.49ms | 1.005× | 20.67% | PASS |
| file_writes | `oltp_update_non_index` | 122.08ms | 115.17ms | 0.943× | 44.83% | PASS |
| file_writes | `oltp_delete_insert` | 199.50ms | 125.68ms | 0.630× | 37.09% | PASS |
| file_writes | `oltp_write_only` | 193.28ms | 101.19ms | 0.524× | 39.07% | PASS |
| file_writes | `types_delete_insert` | 108.73ms | 72.70ms | 0.669× | 38.21% | PASS |
| file_writes | `oltp_read_write` | 198.88ms | 146.53ms | 0.737× | 44.47% | PASS |
| ac_reads | `oltp_point_select` | 26.80ms | 30.48ms | 1.137× | 1.23% | PASS |
| ac_reads | `oltp_range_select` | 13.41ms | 13.88ms | 1.035× | 3.28% | PASS |
| ac_reads | `oltp_sum_range` | 12.73ms | 13.09ms | 1.028× | 3.58% | PASS |
| ac_reads | `oltp_order_range` | 2.49ms | 2.52ms | 1.014× | 3.51% | PASS |
| ac_reads | `oltp_distinct_range` | 3.01ms | 3.12ms | 1.037× | 3.18% | PASS |
| ac_reads | `oltp_index_scan` | 3.48ms | 4.24ms | 1.217× | 2.63% | PASS |
| ac_reads | `select_random_points` | 18.81ms | 20.75ms | 1.103× | 1.66% | PASS |
| ac_reads | `select_random_ranges` | 5.31ms | 6.02ms | 1.132× | 1.96% | PASS |
| ac_reads | `covering_index_scan` | 3.19ms | 3.24ms | 1.018× | 1.12% | PASS |
| ac_reads | `groupby_scan` | 22.82ms | 24.64ms | 1.080× | 0.68% | PASS |
| ac_reads | `index_join` | 6.10ms | 7.65ms | 1.254× | 2.60% | PASS |
| ac_reads | `index_join_scan` | 2.89ms | 4.36ms | 1.508× | 5.01% | PASS |
| ac_reads | `types_table_scan` | 748.60ms | 844.84ms | 1.129× | 0.54% | PASS |
| ac_reads | `table_scan` | 860.90ms | 913.44ms | 1.061× | 1.32% | PASS |
| ac_reads | `oltp_read_only` | 102.29ms | 111.19ms | 1.087× | 0.97% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 236.97ms | 577.51ms | 2.437× | 58.67% | PASS |
| ac_writes | `oltp_insert_ac` | 293.84ms | 702.91ms | 2.392× | 62.77% | PASS |
| ac_writes | `oltp_update_index_ac` | 235.91ms | 705.97ms | 2.993× | 49.74% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 208.66ms | 680.55ms | 3.261× | 61.14% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 278.58ms | 869.23ms | 3.120× | 65.89% | PASS |
| ac_writes | `oltp_write_only_ac` | 331.67ms | 917.65ms | 2.767× | 46.49% | PASS |
| ac_writes | `types_delete_insert_ac` | 174.49ms | 549.08ms | 3.147× | 61.90% | PASS |
| ac_writes | `oltp_read_write_ac` | 236.80ms | 534.23ms | 2.256× | 43.42% | PASS |

</details>

## Version-control latency

Wall time: 2m 15s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 81.33ms | 200.00ms | 40.7% | 0.47% | PASS |
| `status_dirty_many_tables` | 84.42ms | 200.00ms | 42.2% | 0.88% | PASS |
| `diff_regular_working_one_table` | 77.17ms | 150.00ms | 51.4% | 0.48% | PASS |
| `diff_regular_working_many_tables` | 89.67ms | 200.00ms | 44.8% | 0.55% | PASS |
| `diff_stat_working_many_tables` | 89.77ms | 200.00ms | 44.9% | 0.58% | PASS |
| `diff_schema_working_many_tables` | 90.32ms | 200.00ms | 45.2% | 0.58% | PASS |
| `branch_list_many_branches` | 21.97ms | 100.00ms | 22.0% | 1.32% | PASS |
| `branch_create_delete` | 24.28ms | 100.00ms | 24.3% | 0.72% | PASS |
| `checkout_branch_clean` | 54.05ms | 200.00ms | 27.0% | 0.61% | PASS |
| `merge_data_no_conflicts` | 28.04ms | 150.00ms | 18.7% | 0.91% | PASS |
| `merge_schema_no_conflicts` | 21.54ms | 100.00ms | 21.5% | 1.04% | PASS |
| `merge_data_conflicts` | 125.33ms | 250.00ms | 50.1% | 0.20% | PASS |
| `merge_data_conflicts_with_resolve` | 125.19ms | 250.00ms | 50.1% | 0.19% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
