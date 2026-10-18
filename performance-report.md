# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-18 15:00 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261012.334.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/39131635542)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 10s | 9.03s | 11.43s | 1.266× | 1.50% | **PASS** |
| textpk | 69 | 55 | 1h 30m 2s | 9.64s | 10.07s | 1.044× | 1.55% | **PASS** |
| blobpk | 69 | 55 | 1h 6m 10s | 6.93s | 7.63s | 1.102× | 2.92% | **PASS** |
| compositepk | 69 | 55 | 1h 29m 38s | 10.44s | 12.26s | 1.175× | 1.87% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.63ms | 29.68ms | 1.205× | 1.77% | PASS |
| mem_reads | `oltp_range_select` | 10.72ms | 13.21ms | 1.232× | 1.67% | PASS |
| mem_reads | `oltp_sum_range` | 9.94ms | 12.40ms | 1.247× | 1.73% | PASS |
| mem_reads | `oltp_order_range` | 2.57ms | 3.00ms | 1.166× | 1.02% | PASS |
| mem_reads | `oltp_distinct_range` | 3.67ms | 4.05ms | 1.103× | 1.34% | PASS |
| mem_reads | `oltp_index_scan` | 3.98ms | 5.42ms | 1.364× | 2.09% | PASS |
| mem_reads | `select_random_points` | 10.46ms | 11.18ms | 1.069× | 3.61% | PASS |
| mem_reads | `select_random_ranges` | 3.06ms | 4.00ms | 1.306× | 1.51% | PASS |
| mem_reads | `covering_index_scan` | 4.24ms | 4.29ms | 1.013× | 1.99% | PASS |
| mem_reads | `groupby_scan` | 29.98ms | 33.36ms | 1.113× | 0.79% | PASS |
| mem_reads | `index_join` | 6.12ms | 8.27ms | 1.351× | 1.49% | PASS |
| mem_reads | `index_join_scan` | 3.65ms | 4.55ms | 1.246× | 2.27% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.34s | 1.267× | 0.67% | PASS |
| mem_reads | `table_scan` | 1.22s | 1.40s | 1.153× | 0.91% | PASS |
| mem_reads | `oltp_read_only` | 105.26ms | 124.25ms | 1.180× | 1.36% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.57ms | 254.68ms | 1.410× | 1.26% | PASS |
| mem_writes | `oltp_insert` | 15.54ms | 28.58ms | 1.839× | 0.77% | PASS |
| mem_writes | `oltp_update_index` | 52.23ms | 107.13ms | 2.051× | 1.38% | PASS |
| mem_writes | `oltp_update_non_index` | 35.31ms | 60.67ms | 1.718× | 2.27% | PASS |
| mem_writes | `oltp_delete_insert` | 46.58ms | 80.15ms | 1.721× | 1.08% | PASS |
| mem_writes | `oltp_write_only` | 22.49ms | 45.86ms | 2.039× | 1.36% | PASS |
| mem_writes | `types_delete_insert` | 25.36ms | 40.67ms | 1.604× | 1.93% | PASS |
| mem_writes | `oltp_read_write` | 69.59ms | 112.69ms | 1.619× | 1.59% | PASS |
| file_reads | `oltp_point_select` | 99.76ms | 55.64ms | 0.558× | 0.94% | PASS |
| file_reads | `oltp_range_select` | 18.53ms | 15.94ms | 0.860× | 1.41% | PASS |
| file_reads | `oltp_sum_range` | 17.73ms | 15.15ms | 0.855× | 1.28% | PASS |
| file_reads | `oltp_order_range` | 3.43ms | 3.32ms | 0.968× | 1.75% | PASS |
| file_reads | `oltp_distinct_range` | 4.50ms | 4.39ms | 0.974× | 1.19% | PASS |
| file_reads | `oltp_index_scan` | 11.70ms | 8.34ms | 0.713× | 1.44% | PASS |
| file_reads | `select_random_points` | 18.97ms | 13.99ms | 0.738× | 2.15% | PASS |
| file_reads | `select_random_ranges` | 10.57ms | 6.62ms | 0.626× | 1.28% | PASS |
| file_reads | `covering_index_scan` | 11.91ms | 7.12ms | 0.598× | 0.87% | PASS |
| file_reads | `groupby_scan` | 30.91ms | 33.79ms | 1.093× | 0.76% | PASS |
| file_reads | `index_join` | 10.46ms | 10.21ms | 0.976× | 1.30% | PASS |
| file_reads | `index_join_scan` | 4.69ms | 4.96ms | 1.058× | 3.06% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.34s | 1.265× | 0.92% | PASS |
| file_reads | `table_scan` | 1.24s | 1.41s | 1.136× | 1.22% | PASS |
| file_reads | `oltp_read_only` | 218.64ms | 162.87ms | 0.745× | 1.07% | PASS |
| file_writes | `oltp_bulk_insert` | 195.47ms | 272.21ms | 1.393× | 1.35% | PASS |
| file_writes | `oltp_insert` | 22.36ms | 35.85ms | 1.603× | 1.50% | PASS |
| file_writes | `oltp_update_index` | 80.07ms | 130.54ms | 1.630× | 1.91% | PASS |
| file_writes | `oltp_update_non_index` | 59.99ms | 82.76ms | 1.380× | 2.46% | PASS |
| file_writes | `oltp_delete_insert` | 69.97ms | 101.00ms | 1.444× | 2.24% | PASS |
| file_writes | `oltp_write_only` | 46.48ms | 65.39ms | 1.407× | 2.13% | PASS |
| file_writes | `types_delete_insert` | 41.59ms | 54.12ms | 1.301× | 2.22% | PASS |
| file_writes | `oltp_read_write` | 94.76ms | 131.86ms | 1.392× | 2.06% | PASS |
| ac_reads | `oltp_point_select` | 50.39ms | 55.95ms | 1.110× | 0.88% | PASS |
| ac_reads | `oltp_range_select` | 14.18ms | 16.24ms | 1.145× | 1.47% | PASS |
| ac_reads | `oltp_sum_range` | 13.39ms | 15.51ms | 1.158× | 1.53% | PASS |
| ac_reads | `oltp_order_range` | 3.00ms | 3.34ms | 1.115× | 1.80% | PASS |
| ac_reads | `oltp_distinct_range` | 4.06ms | 4.42ms | 1.089× | 1.22% | PASS |
| ac_reads | `oltp_index_scan` | 7.14ms | 8.59ms | 1.203× | 1.28% | PASS |
| ac_reads | `select_random_points` | 14.79ms | 14.60ms | 0.987× | 3.01% | PASS |
| ac_reads | `select_random_ranges` | 5.74ms | 6.70ms | 1.166× | 1.03% | PASS |
| ac_reads | `covering_index_scan` | 7.45ms | 7.30ms | 0.981× | 1.95% | PASS |
| ac_reads | `groupby_scan` | 30.57ms | 34.11ms | 1.116× | 0.67% | PASS |
| ac_reads | `index_join` | 8.35ms | 10.55ms | 1.263× | 2.03% | PASS |
| ac_reads | `index_join_scan` | 4.04ms | 4.92ms | 1.218× | 2.16% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.32s | 1.274× | 0.62% | PASS |
| ac_reads | `table_scan` | 1.19s | 1.40s | 1.176× | 0.90% | PASS |
| ac_reads | `oltp_read_only` | 144.10ms | 163.91ms | 1.138× | 1.31% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.14ms | 68.30ms | 3.231× | 7.10% | PASS |
| ac_writes | `oltp_insert_ac` | 25.53ms | 84.53ms | 3.311× | 7.94% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.49ms | 98.58ms | 3.722× | 4.28% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.88ms | 76.53ms | 3.498× | 4.70% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.78ms | 88.69ms | 3.578× | 6.98% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.73ms | 87.08ms | 3.670× | 6.03% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.24ms | 79.79ms | 3.756× | 7.15% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.14ms | 94.73ms | 3.142× | 5.05% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.15ms | 27.71ms | 1.147× | 1.60% | PASS |
| mem_reads | `oltp_range_select` | 11.57ms | 10.81ms | 0.934× | 1.87% | PASS |
| mem_reads | `oltp_sum_range` | 10.16ms | 10.61ms | 1.044× | 2.17% | PASS |
| mem_reads | `oltp_order_range` | 2.52ms | 2.53ms | 1.000× | 1.58% | PASS |
| mem_reads | `oltp_distinct_range` | 3.40ms | 3.31ms | 0.972× | 1.34% | PASS |
| mem_reads | `oltp_index_scan` | 3.79ms | 4.79ms | 1.265× | 3.12% | PASS |
| mem_reads | `select_random_points` | 15.36ms | 16.43ms | 1.070× | 2.63% | PASS |
| mem_reads | `select_random_ranges` | 3.53ms | 4.23ms | 1.197× | 2.46% | PASS |
| mem_reads | `covering_index_scan` | 3.87ms | 3.71ms | 0.958× | 5.01% | PASS |
| mem_reads | `groupby_scan` | 27.12ms | 28.19ms | 1.039× | 1.23% | PASS |
| mem_reads | `index_join` | 5.87ms | 7.40ms | 1.260× | 3.00% | PASS |
| mem_reads | `index_join_scan` | 4.72ms | 4.82ms | 1.022× | 3.64% | PASS |
| mem_reads | `types_table_scan` | 940.55ms | 990.90ms | 1.054× | 3.64% | PASS |
| mem_reads | `table_scan` | 1.07s | 1.07s | 0.997× | 1.48% | PASS |
| mem_reads | `oltp_read_only` | 100.07ms | 101.64ms | 1.016× | 2.95% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.69ms | 261.05ms | 1.437× | 1.10% | PASS |
| mem_writes | `oltp_insert` | 18.32ms | 30.55ms | 1.667× | 2.04% | PASS |
| mem_writes | `oltp_update_index` | 59.98ms | 105.39ms | 1.757× | 1.46% | PASS |
| mem_writes | `oltp_update_non_index` | 41.22ms | 67.45ms | 1.636× | 0.96% | PASS |
| mem_writes | `oltp_delete_insert` | 43.60ms | 81.42ms | 1.867× | 1.53% | PASS |
| mem_writes | `oltp_write_only` | 24.90ms | 48.53ms | 1.949× | 1.30% | PASS |
| mem_writes | `types_delete_insert` | 27.09ms | 40.73ms | 1.504× | 1.35% | PASS |
| mem_writes | `oltp_read_write` | 68.57ms | 103.76ms | 1.513× | 1.02% | PASS |
| file_reads | `oltp_point_select` | 99.00ms | 52.45ms | 0.530× | 0.75% | PASS |
| file_reads | `oltp_range_select` | 20.17ms | 13.43ms | 0.666× | 1.51% | PASS |
| file_reads | `oltp_sum_range` | 18.63ms | 13.36ms | 0.717× | 1.32% | PASS |
| file_reads | `oltp_order_range` | 3.50ms | 2.83ms | 0.807× | 1.40% | PASS |
| file_reads | `oltp_distinct_range` | 4.31ms | 3.61ms | 0.839× | 1.24% | PASS |
| file_reads | `oltp_index_scan` | 11.72ms | 7.59ms | 0.648× | 0.90% | PASS |
| file_reads | `select_random_points` | 23.07ms | 19.02ms | 0.824× | 1.49% | PASS |
| file_reads | `select_random_ranges` | 11.15ms | 6.75ms | 0.605× | 0.65% | PASS |
| file_reads | `covering_index_scan` | 12.76ms | 6.44ms | 0.505× | 0.70% | PASS |
| file_reads | `groupby_scan` | 28.42ms | 28.54ms | 1.004× | 0.83% | PASS |
| file_reads | `index_join` | 10.68ms | 9.26ms | 0.867× | 1.25% | PASS |
| file_reads | `index_join_scan` | 5.03ms | 5.15ms | 1.023× | 1.26% | PASS |
| file_reads | `types_table_scan` | 909.59ms | 972.45ms | 1.069× | 1.55% | PASS |
| file_reads | `table_scan` | 1.12s | 1.07s | 0.957× | 3.70% | PASS |
| file_reads | `oltp_read_only` | 208.25ms | 137.04ms | 0.658× | 1.24% | PASS |
| file_writes | `oltp_bulk_insert` | 270.64ms | 347.58ms | 1.284× | 10.01% | PASS |
| file_writes | `oltp_insert` | 69.51ms | 59.39ms | 0.854× | 24.01% | PASS |
| file_writes | `oltp_update_index` | 231.65ms | 211.10ms | 0.911× | 9.92% | PASS |
| file_writes | `oltp_update_non_index` | 170.54ms | 139.48ms | 0.818× | 9.60% | PASS |
| file_writes | `oltp_delete_insert` | 184.12ms | 168.62ms | 0.916× | 9.62% | PASS |
| file_writes | `oltp_write_only` | 146.53ms | 116.58ms | 0.796× | 21.47% | PASS |
| file_writes | `types_delete_insert` | 103.42ms | 99.38ms | 0.961× | 25.27% | PASS |
| file_writes | `oltp_read_write` | 191.37ms | 172.29ms | 0.900× | 10.77% | PASS |
| ac_reads | `oltp_point_select` | 48.82ms | 52.30ms | 1.071× | 0.76% | PASS |
| ac_reads | `oltp_range_select` | 15.20ms | 13.44ms | 0.885× | 1.37% | PASS |
| ac_reads | `oltp_sum_range` | 13.59ms | 13.32ms | 0.980× | 0.92% | PASS |
| ac_reads | `oltp_order_range` | 3.08ms | 2.83ms | 0.921× | 0.75% | PASS |
| ac_reads | `oltp_distinct_range` | 3.85ms | 3.61ms | 0.937× | 0.74% | PASS |
| ac_reads | `oltp_index_scan` | 6.89ms | 7.68ms | 1.115× | 0.74% | PASS |
| ac_reads | `select_random_points` | 18.53ms | 19.35ms | 1.044× | 2.26% | PASS |
| ac_reads | `select_random_ranges` | 6.25ms | 6.78ms | 1.085× | 0.98% | PASS |
| ac_reads | `covering_index_scan` | 7.88ms | 6.45ms | 0.820× | 1.22% | PASS |
| ac_reads | `groupby_scan` | 28.23ms | 28.74ms | 1.018× | 0.99% | PASS |
| ac_reads | `index_join` | 8.62ms | 9.69ms | 1.124× | 2.16% | PASS |
| ac_reads | `index_join_scan` | 4.79ms | 5.39ms | 1.125× | 1.64% | PASS |
| ac_reads | `types_table_scan` | 1.10s | 1.03s | 0.933× | 2.58% | PASS |
| ac_reads | `table_scan` | 1.47s | 1.15s | 0.782× | 0.72% | PASS |
| ac_reads | `oltp_read_only` | 145.37ms | 143.22ms | 0.985× | 1.44% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.00ms | 67.06ms | 3.049× | 25.36% | PASS |
| ac_writes | `oltp_insert_ac` | 28.15ms | 148.04ms | 5.260× | 55.97% | PASS |
| ac_writes | `oltp_update_index_ac` | 32.62ms | 142.97ms | 4.383× | 54.37% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.85ms | 111.02ms | 4.468× | 68.49% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.86ms | 115.08ms | 4.450× | 42.62% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.82ms | 95.88ms | 3.864× | 20.07% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.84ms | 92.79ms | 4.063× | 37.71% | PASS |
| ac_writes | `oltp_read_write_ac` | 36.35ms | 123.11ms | 3.387× | 53.23% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.06ms | 22.11ms | 1.102× | 2.03% | PASS |
| mem_reads | `oltp_range_select` | 8.77ms | 8.61ms | 0.982× | 2.84% | PASS |
| mem_reads | `oltp_sum_range` | 9.07ms | 8.74ms | 0.964× | 4.87% | PASS |
| mem_reads | `oltp_order_range` | 2.00ms | 1.94ms | 0.968× | 2.43% | PASS |
| mem_reads | `oltp_distinct_range` | 2.40ms | 2.39ms | 0.992× | 3.67% | PASS |
| mem_reads | `oltp_index_scan` | 2.86ms | 3.89ms | 1.361× | 4.04% | PASS |
| mem_reads | `select_random_points` | 11.71ms | 13.14ms | 1.122× | 4.84% | PASS |
| mem_reads | `select_random_ranges` | 3.02ms | 3.49ms | 1.155× | 2.49% | PASS |
| mem_reads | `covering_index_scan` | 2.56ms | 3.28ms | 1.284× | 5.12% | PASS |
| mem_reads | `groupby_scan` | 19.87ms | 19.58ms | 0.985× | 2.15% | PASS |
| mem_reads | `index_join` | 4.91ms | 7.62ms | 1.553× | 3.72% | PASS |
| mem_reads | `index_join_scan` | 3.56ms | 4.80ms | 1.347× | 3.53% | PASS |
| mem_reads | `types_table_scan` | 683.61ms | 732.71ms | 1.072× | 1.60% | PASS |
| mem_reads | `table_scan` | 885.99ms | 849.15ms | 0.958× | 7.60% | PASS |
| mem_reads | `oltp_read_only` | 73.92ms | 74.26ms | 1.005× | 4.30% | PASS |
| mem_writes | `oltp_bulk_insert` | 137.44ms | 200.48ms | 1.459× | 2.06% | PASS |
| mem_writes | `oltp_insert` | 11.67ms | 24.36ms | 2.087× | 1.95% | PASS |
| mem_writes | `oltp_update_index` | 42.71ms | 86.57ms | 2.027× | 3.02% | PASS |
| mem_writes | `oltp_update_non_index` | 31.74ms | 56.84ms | 1.791× | 2.95% | PASS |
| mem_writes | `oltp_delete_insert` | 31.70ms | 69.65ms | 2.197× | 2.66% | PASS |
| mem_writes | `oltp_write_only` | 17.72ms | 42.88ms | 2.420× | 2.44% | PASS |
| mem_writes | `types_delete_insert` | 21.31ms | 37.04ms | 1.738× | 3.34% | PASS |
| mem_writes | `oltp_read_write` | 61.15ms | 90.44ms | 1.479× | 3.09% | PASS |
| file_reads | `oltp_point_select` | 76.89ms | 42.19ms | 0.549× | 1.81% | PASS |
| file_reads | `oltp_range_select` | 14.74ms | 10.39ms | 0.705× | 2.11% | PASS |
| file_reads | `oltp_sum_range` | 14.75ms | 10.47ms | 0.710× | 2.80% | PASS |
| file_reads | `oltp_order_range` | 2.72ms | 2.22ms | 0.817× | 1.71% | PASS |
| file_reads | `oltp_distinct_range` | 3.29ms | 2.73ms | 0.829× | 1.02% | PASS |
| file_reads | `oltp_index_scan` | 9.24ms | 6.16ms | 0.666× | 1.83% | PASS |
| file_reads | `select_random_points` | 18.36ms | 15.10ms | 0.823× | 3.04% | PASS |
| file_reads | `select_random_ranges` | 8.60ms | 5.39ms | 0.626× | 2.01% | PASS |
| file_reads | `covering_index_scan` | 9.12ms | 5.07ms | 0.556× | 1.99% | PASS |
| file_reads | `groupby_scan` | 19.67ms | 18.81ms | 0.956× | 2.31% | PASS |
| file_reads | `index_join` | 8.03ms | 7.37ms | 0.917× | 1.98% | PASS |
| file_reads | `index_join_scan` | 3.82ms | 4.40ms | 1.151× | 2.34% | PASS |
| file_reads | `types_table_scan` | 695.89ms | 745.20ms | 1.071× | 1.98% | PASS |
| file_reads | `table_scan` | 928.98ms | 859.80ms | 0.926× | 3.46% | PASS |
| file_reads | `oltp_read_only` | 158.44ms | 102.63ms | 0.648× | 2.49% | PASS |
| file_writes | `oltp_bulk_insert` | 190.19ms | 270.23ms | 1.421× | 3.80% | PASS |
| file_writes | `oltp_insert` | 34.70ms | 50.03ms | 1.442× | 5.60% | PASS |
| file_writes | `oltp_update_index` | 168.61ms | 179.34ms | 1.064× | 7.01% | PASS |
| file_writes | `oltp_update_non_index` | 131.08ms | 121.40ms | 0.926× | 3.36% | PASS |
| file_writes | `oltp_delete_insert` | 131.87ms | 133.85ms | 1.015× | 2.65% | PASS |
| file_writes | `oltp_write_only` | 98.37ms | 98.81ms | 1.004× | 7.80% | PASS |
| file_writes | `types_delete_insert` | 90.37ms | 80.17ms | 0.887× | 9.17% | PASS |
| file_writes | `oltp_read_write` | 131.63ms | 139.82ms | 1.062× | 7.15% | PASS |
| ac_reads | `oltp_point_select` | 40.01ms | 42.60ms | 1.065× | 2.43% | PASS |
| ac_reads | `oltp_range_select` | 11.32ms | 10.61ms | 0.937× | 2.92% | PASS |
| ac_reads | `oltp_sum_range` | 11.36ms | 10.66ms | 0.938× | 3.43% | PASS |
| ac_reads | `oltp_order_range` | 2.46ms | 2.24ms | 0.911× | 1.30% | PASS |
| ac_reads | `oltp_distinct_range` | 2.95ms | 2.71ms | 0.920× | 1.66% | PASS |
| ac_reads | `oltp_index_scan` | 5.65ms | 6.22ms | 1.100× | 2.18% | PASS |
| ac_reads | `select_random_points` | 15.01ms | 14.86ms | 0.990× | 5.14% | PASS |
| ac_reads | `select_random_ranges` | 5.01ms | 5.37ms | 1.071× | 1.96% | PASS |
| ac_reads | `covering_index_scan` | 5.63ms | 5.10ms | 0.906× | 2.02% | PASS |
| ac_reads | `groupby_scan` | 19.36ms | 19.09ms | 0.986× | 2.08% | PASS |
| ac_reads | `index_join` | 6.35ms | 7.43ms | 1.169× | 3.51% | PASS |
| ac_reads | `index_join_scan` | 3.53ms | 4.40ms | 1.247× | 3.39% | PASS |
| ac_reads | `types_table_scan` | 681.70ms | 732.64ms | 1.075× | 1.69% | PASS |
| ac_reads | `table_scan` | 786.61ms | 827.00ms | 1.051× | 2.20% | PASS |
| ac_reads | `oltp_read_only` | 90.22ms | 96.67ms | 1.071× | 1.40% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.51ms | 54.95ms | 2.441× | 4.33% | PASS |
| ac_writes | `oltp_insert_ac` | 26.62ms | 72.06ms | 2.708× | 3.14% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.69ms | 79.95ms | 2.887× | 5.85% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.17ms | 66.04ms | 2.979× | 3.15% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.99ms | 74.05ms | 2.963× | 4.77% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.53ms | 74.46ms | 2.917× | 5.35% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.92ms | 69.98ms | 3.345× | 7.03% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.52ms | 79.87ms | 2.800× | 5.11% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.82ms | 40.75ms | 1.205× | 1.62% | PASS |
| mem_reads | `oltp_range_select` | 19.15ms | 21.24ms | 1.109× | 1.34% | PASS |
| mem_reads | `oltp_sum_range` | 18.41ms | 21.21ms | 1.152× | 1.16% | PASS |
| mem_reads | `oltp_order_range` | 3.61ms | 3.92ms | 1.086× | 1.15% | PASS |
| mem_reads | `oltp_distinct_range` | 4.78ms | 5.03ms | 1.051× | 1.48% | PASS |
| mem_reads | `oltp_index_scan` | 4.76ms | 6.46ms | 1.356× | 2.36% | PASS |
| mem_reads | `select_random_points` | 29.49ms | 33.11ms | 1.123× | 1.92% | PASS |
| mem_reads | `select_random_ranges` | 8.06ms | 9.33ms | 1.156× | 1.05% | PASS |
| mem_reads | `covering_index_scan` | 4.36ms | 4.77ms | 1.095× | 3.13% | PASS |
| mem_reads | `groupby_scan` | 36.74ms | 39.29ms | 1.069× | 0.88% | PASS |
| mem_reads | `index_join` | 8.38ms | 11.00ms | 1.312× | 2.53% | PASS |
| mem_reads | `index_join_scan` | 4.33ms | 5.72ms | 1.321× | 3.68% | PASS |
| mem_reads | `types_table_scan` | 1.15s | 1.27s | 1.109× | 0.50% | PASS |
| mem_reads | `table_scan` | 1.38s | 1.43s | 1.037× | 1.37% | PASS |
| mem_reads | `oltp_read_only` | 162.36ms | 177.08ms | 1.091× | 1.58% | PASS |
| mem_writes | `oltp_bulk_insert` | 250.15ms | 365.50ms | 1.461× | 0.85% | PASS |
| mem_writes | `oltp_insert` | 20.06ms | 37.82ms | 1.886× | 1.40% | PASS |
| mem_writes | `oltp_update_index` | 72.68ms | 126.88ms | 1.746× | 2.67% | PASS |
| mem_writes | `oltp_update_non_index` | 53.08ms | 86.48ms | 1.629× | 1.42% | PASS |
| mem_writes | `oltp_delete_insert` | 52.07ms | 100.48ms | 1.930× | 1.44% | PASS |
| mem_writes | `oltp_write_only` | 29.00ms | 60.72ms | 2.094× | 2.31% | PASS |
| mem_writes | `types_delete_insert` | 34.19ms | 57.37ms | 1.678× | 1.86% | PASS |
| mem_writes | `oltp_read_write` | 109.53ms | 162.00ms | 1.479× | 1.87% | PASS |
| file_reads | `oltp_point_select` | 111.81ms | 69.17ms | 0.619× | 1.55% | PASS |
| file_reads | `oltp_range_select` | 28.48ms | 25.17ms | 0.884× | 2.41% | PASS |
| file_reads | `oltp_sum_range` | 26.96ms | 25.01ms | 0.928× | 1.87% | PASS |
| file_reads | `oltp_order_range` | 4.90ms | 4.60ms | 0.939× | 2.81% | PASS |
| file_reads | `oltp_distinct_range` | 5.91ms | 5.62ms | 0.952× | 2.03% | PASS |
| file_reads | `oltp_index_scan` | 12.63ms | 9.46ms | 0.748× | 2.03% | PASS |
| file_reads | `select_random_points` | 38.93ms | 37.01ms | 0.951× | 1.74% | PASS |
| file_reads | `select_random_ranges` | 16.06ms | 12.50ms | 0.778× | 1.12% | PASS |
| file_reads | `covering_index_scan` | 12.70ms | 7.37ms | 0.581× | 1.68% | PASS |
| file_reads | `groupby_scan` | 38.54ms | 40.52ms | 1.051× | 1.00% | PASS |
| file_reads | `index_join` | 13.54ms | 13.79ms | 1.019× | 3.13% | PASS |
| file_reads | `index_join_scan` | 5.66ms | 6.43ms | 1.138× | 3.74% | PASS |
| file_reads | `types_table_scan` | 1.15s | 1.26s | 1.100× | 1.89% | PASS |
| file_reads | `table_scan` | 1.36s | 1.42s | 1.048× | 3.23% | PASS |
| file_reads | `oltp_read_only` | 271.85ms | 218.30ms | 0.803× | 1.08% | PASS |
| file_writes | `oltp_bulk_insert` | 266.01ms | 387.95ms | 1.458× | 1.33% | PASS |
| file_writes | `oltp_insert` | 26.85ms | 47.02ms | 1.751× | 1.49% | PASS |
| file_writes | `oltp_update_index` | 101.84ms | 147.74ms | 1.451× | 2.15% | PASS |
| file_writes | `oltp_update_non_index` | 82.66ms | 108.75ms | 1.316× | 2.64% | PASS |
| file_writes | `oltp_delete_insert` | 86.73ms | 129.47ms | 1.493× | 1.87% | PASS |
| file_writes | `oltp_write_only` | 55.06ms | 81.47ms | 1.480× | 2.03% | PASS |
| file_writes | `types_delete_insert` | 53.87ms | 73.28ms | 1.360× | 1.82% | PASS |
| file_writes | `oltp_read_write` | 146.39ms | 192.07ms | 1.312× | 1.58% | PASS |
| ac_reads | `oltp_point_select` | 61.44ms | 71.21ms | 1.159× | 1.46% | PASS |
| ac_reads | `oltp_range_select` | 23.30ms | 25.17ms | 1.080× | 2.34% | PASS |
| ac_reads | `oltp_sum_range` | 21.61ms | 24.86ms | 1.150× | 1.79% | PASS |
| ac_reads | `oltp_order_range` | 4.14ms | 4.42ms | 1.067× | 2.07% | PASS |
| ac_reads | `oltp_distinct_range` | 5.25ms | 5.51ms | 1.050× | 1.62% | PASS |
| ac_reads | `oltp_index_scan` | 7.65ms | 9.46ms | 1.237× | 1.89% | PASS |
| ac_reads | `select_random_points` | 32.26ms | 37.17ms | 1.152× | 1.91% | PASS |
| ac_reads | `select_random_ranges` | 10.66ms | 12.39ms | 1.162× | 1.29% | PASS |
| ac_reads | `covering_index_scan` | 7.27ms | 7.37ms | 1.014× | 1.51% | PASS |
| ac_reads | `groupby_scan` | 37.44ms | 40.28ms | 1.076× | 0.90% | PASS |
| ac_reads | `index_join` | 10.30ms | 13.30ms | 1.292× | 1.94% | PASS |
| ac_reads | `index_join_scan` | 4.81ms | 6.07ms | 1.261× | 2.55% | PASS |
| ac_reads | `types_table_scan` | 1.10s | 1.25s | 1.134× | 2.20% | PASS |
| ac_reads | `table_scan` | 1.30s | 1.41s | 1.082× | 2.29% | PASS |
| ac_reads | `oltp_read_only` | 198.00ms | 218.74ms | 1.105× | 1.61% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.32ms | 70.13ms | 3.007× | 6.99% | PASS |
| ac_writes | `oltp_insert_ac` | 25.54ms | 91.49ms | 3.582× | 6.58% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.84ms | 102.67ms | 3.688× | 4.47% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.62ms | 82.05ms | 3.474× | 6.30% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.56ms | 96.26ms | 3.624× | 6.98% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.85ms | 95.12ms | 3.680× | 7.00% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.77ms | 83.35ms | 3.661× | 7.69% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.89ms | 101.56ms | 3.088× | 4.43% | PASS |

</details>

## Version-control latency

Wall time: 2m 2s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 67.48ms | 200.00ms | 33.7% | 0.52% | PASS |
| `status_dirty_many_tables` | 69.92ms | 200.00ms | 35.0% | 0.46% | PASS |
| `diff_regular_working_one_table` | 63.79ms | 150.00ms | 42.5% | 0.66% | PASS |
| `diff_regular_working_many_tables` | 75.69ms | 200.00ms | 37.8% | 0.48% | PASS |
| `diff_stat_working_many_tables` | 75.45ms | 200.00ms | 37.7% | 0.53% | PASS |
| `diff_schema_working_many_tables` | 75.77ms | 200.00ms | 37.9% | 0.57% | PASS |
| `branch_list_many_branches` | 21.49ms | 100.00ms | 21.5% | 1.71% | PASS |
| `branch_create_delete` | 23.86ms | 100.00ms | 23.9% | 1.69% | PASS |
| `checkout_branch_clean` | 94.12ms | 200.00ms | 47.1% | 0.49% | PASS |
| `merge_data_no_conflicts` | 28.23ms | 150.00ms | 18.8% | 1.82% | PASS |
| `merge_schema_no_conflicts` | 21.69ms | 100.00ms | 21.7% | 1.44% | PASS |
| `merge_data_conflicts` | 78.82ms | 250.00ms | 31.5% | 0.93% | PASS |
| `merge_data_conflicts_with_resolve` | 78.02ms | 250.00ms | 31.2% | 0.55% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
