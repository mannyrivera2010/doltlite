# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-27 16:58 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261023.334.3
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/40531525800)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 0m 5s | 7.40s | 9.05s | 1.223× | 1.53% | **PASS** |
| textpk | 69 | 55 | 1h 11m 55s | 7.44s | 7.94s | 1.068× | 3.10% | **PASS** |
| blobpk | 69 | 55 | 1h 19m 32s | 9.20s | 10.45s | 1.136× | 1.60% | **PASS** |
| compositepk | 69 | 55 | 1h 25m 44s | 9.51s | 11.84s | 1.245× | 1.28% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 18.00ms | 20.24ms | 1.124× | 0.91% | PASS |
| mem_reads | `oltp_range_select` | 8.29ms | 9.74ms | 1.176× | 1.89% | PASS |
| mem_reads | `oltp_sum_range` | 7.46ms | 9.08ms | 1.218× | 3.52% | PASS |
| mem_reads | `oltp_order_range` | 2.15ms | 2.40ms | 1.115× | 2.08% | PASS |
| mem_reads | `oltp_distinct_range` | 2.97ms | 3.23ms | 1.087× | 1.90% | PASS |
| mem_reads | `oltp_index_scan` | 3.09ms | 3.77ms | 1.219× | 1.67% | PASS |
| mem_reads | `select_random_points` | 7.97ms | 8.87ms | 1.113× | 4.21% | PASS |
| mem_reads | `select_random_ranges` | 2.30ms | 2.90ms | 1.262× | 2.10% | PASS |
| mem_reads | `covering_index_scan` | 2.96ms | 2.96ms | 1.001× | 1.63% | PASS |
| mem_reads | `groupby_scan` | 25.08ms | 26.68ms | 1.064× | 0.47% | PASS |
| mem_reads | `index_join` | 4.58ms | 6.21ms | 1.357× | 1.11% | PASS |
| mem_reads | `index_join_scan` | 2.53ms | 3.71ms | 1.468× | 1.22% | PASS |
| mem_reads | `types_table_scan` | 853.75ms | 1.03s | 1.206× | 0.48% | PASS |
| mem_reads | `table_scan` | 970.24ms | 1.12s | 1.158× | 0.55% | PASS |
| mem_reads | `oltp_read_only` | 79.47ms | 91.15ms | 1.147× | 0.47% | PASS |
| mem_writes | `oltp_bulk_insert` | 121.24ms | 160.74ms | 1.326× | 0.71% | PASS |
| mem_writes | `oltp_insert` | 10.94ms | 18.88ms | 1.727× | 0.88% | PASS |
| mem_writes | `oltp_update_index` | 37.48ms | 75.11ms | 2.004× | 1.14% | PASS |
| mem_writes | `oltp_update_non_index` | 24.73ms | 41.96ms | 1.696× | 1.08% | PASS |
| mem_writes | `oltp_delete_insert` | 33.70ms | 55.03ms | 1.633× | 1.16% | PASS |
| mem_writes | `oltp_write_only` | 16.07ms | 31.74ms | 1.975× | 1.00% | PASS |
| mem_writes | `types_delete_insert` | 17.96ms | 28.28ms | 1.574× | 1.21% | PASS |
| mem_writes | `oltp_read_write` | 47.64ms | 76.13ms | 1.598× | 0.78% | PASS |
| file_reads | `oltp_point_select` | 43.82ms | 28.69ms | 0.655× | 0.86% | PASS |
| file_reads | `oltp_range_select` | 11.14ms | 10.92ms | 0.980× | 1.31% | PASS |
| file_reads | `oltp_sum_range` | 10.76ms | 10.50ms | 0.976× | 1.97% | PASS |
| file_reads | `oltp_order_range` | 2.60ms | 2.65ms | 1.018× | 1.45% | PASS |
| file_reads | `oltp_distinct_range` | 3.41ms | 3.41ms | 1.000× | 1.13% | PASS |
| file_reads | `oltp_index_scan` | 5.77ms | 4.75ms | 0.824× | 2.98% | PASS |
| file_reads | `select_random_points` | 11.25ms | 9.95ms | 0.885× | 2.71% | PASS |
| file_reads | `select_random_ranges` | 4.87ms | 3.73ms | 0.766× | 1.48% | PASS |
| file_reads | `covering_index_scan` | 5.70ms | 3.90ms | 0.684× | 1.58% | PASS |
| file_reads | `groupby_scan` | 25.19ms | 26.88ms | 1.067× | 1.10% | PASS |
| file_reads | `index_join` | 6.13ms | 6.88ms | 1.122× | 1.43% | PASS |
| file_reads | `index_join_scan` | 2.91ms | 3.91ms | 1.345× | 1.72% | PASS |
| file_reads | `types_table_scan` | 854.27ms | 1.04s | 1.213× | 0.38% | PASS |
| file_reads | `table_scan` | 970.29ms | 1.13s | 1.163× | 0.52% | PASS |
| file_reads | `oltp_read_only` | 117.98ms | 104.57ms | 0.886× | 0.52% | PASS |
| file_writes | `oltp_bulk_insert` | 166.01ms | 216.82ms | 1.306× | 2.69% | PASS |
| file_writes | `oltp_insert` | 22.88ms | 34.30ms | 1.499× | 1.80% | PASS |
| file_writes | `oltp_update_index` | 129.37ms | 136.49ms | 1.055× | 1.09% | PASS |
| file_writes | `oltp_update_non_index` | 104.30ms | 91.28ms | 0.875× | 2.41% | PASS |
| file_writes | `oltp_delete_insert` | 110.86ms | 107.38ms | 0.969× | 1.08% | PASS |
| file_writes | `oltp_write_only` | 82.32ms | 75.32ms | 0.915× | 1.49% | PASS |
| file_writes | `types_delete_insert` | 71.34ms | 60.51ms | 0.848× | 2.66% | PASS |
| file_writes | `oltp_read_write` | 114.41ms | 121.27ms | 1.060× | 1.74% | PASS |
| ac_reads | `oltp_point_select` | 26.25ms | 28.40ms | 1.082× | 0.92% | PASS |
| ac_reads | `oltp_range_select` | 9.00ms | 10.83ms | 1.204× | 3.09% | PASS |
| ac_reads | `oltp_sum_range` | 8.87ms | 10.24ms | 1.155× | 3.14% | PASS |
| ac_reads | `oltp_order_range` | 2.31ms | 2.54ms | 1.103× | 1.96% | PASS |
| ac_reads | `oltp_distinct_range` | 3.17ms | 3.36ms | 1.062× | 1.05% | PASS |
| ac_reads | `oltp_index_scan` | 4.33ms | 5.13ms | 1.185× | 1.53% | PASS |
| ac_reads | `select_random_points` | 9.73ms | 10.17ms | 1.046× | 2.46% | PASS |
| ac_reads | `select_random_ranges` | 3.28ms | 3.86ms | 1.177× | 1.87% | PASS |
| ac_reads | `covering_index_scan` | 3.99ms | 4.07ms | 1.022× | 2.11% | PASS |
| ac_reads | `groupby_scan` | 25.52ms | 26.96ms | 1.057× | 0.60% | PASS |
| ac_reads | `index_join` | 5.45ms | 7.31ms | 1.341× | 1.68% | PASS |
| ac_reads | `index_join_scan` | 2.77ms | 3.87ms | 1.396× | 1.82% | PASS |
| ac_reads | `types_table_scan` | 852.04ms | 1.04s | 1.216× | 0.59% | PASS |
| ac_reads | `table_scan` | 969.65ms | 1.13s | 1.165× | 0.69% | PASS |
| ac_reads | `oltp_read_only` | 91.76ms | 104.27ms | 1.136× | 0.75% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.92ms | 65.74ms | 2.638× | 5.20% | PASS |
| ac_writes | `oltp_insert_ac` | 26.25ms | 75.00ms | 2.857× | 3.83% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.14ms | 84.01ms | 3.214× | 4.09% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.12ms | 70.61ms | 2.927× | 3.63% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.85ms | 79.93ms | 3.092× | 4.36% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.09ms | 75.86ms | 3.023× | 4.86% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.55ms | 69.90ms | 2.968× | 6.25% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.63ms | 79.42ms | 2.874× | 3.69% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 18.62ms | 21.68ms | 1.164× | 2.56% | PASS |
| mem_reads | `oltp_range_select` | 8.56ms | 8.64ms | 1.010× | 4.44% | PASS |
| mem_reads | `oltp_sum_range` | 8.61ms | 8.68ms | 1.008× | 5.68% | PASS |
| mem_reads | `oltp_order_range` | 2.00ms | 1.92ms | 0.960× | 5.21% | PASS |
| mem_reads | `oltp_distinct_range` | 2.52ms | 2.41ms | 0.958× | 4.01% | PASS |
| mem_reads | `oltp_index_scan` | 2.86ms | 3.86ms | 1.349× | 3.78% | PASS |
| mem_reads | `select_random_points` | 12.73ms | 13.70ms | 1.076× | 4.76% | PASS |
| mem_reads | `select_random_ranges` | 2.85ms | 3.50ms | 1.227× | 5.34% | PASS |
| mem_reads | `covering_index_scan` | 3.10ms | 3.38ms | 1.092× | 5.39% | PASS |
| mem_reads | `groupby_scan` | 19.47ms | 19.71ms | 1.012× | 3.12% | PASS |
| mem_reads | `index_join` | 4.40ms | 5.54ms | 1.259× | 3.97% | PASS |
| mem_reads | `index_join_scan` | 3.07ms | 4.27ms | 1.390× | 16.82% | PASS |
| mem_reads | `types_table_scan` | 706.64ms | 756.12ms | 1.070× | 1.64% | PASS |
| mem_reads | `table_scan` | 916.10ms | 860.14ms | 0.939× | 6.32% | PASS |
| mem_reads | `oltp_read_only` | 68.22ms | 72.58ms | 1.064× | 1.93% | PASS |
| mem_writes | `oltp_bulk_insert` | 141.47ms | 216.35ms | 1.529× | 1.32% | PASS |
| mem_writes | `oltp_insert` | 13.30ms | 26.52ms | 1.993× | 2.27% | PASS |
| mem_writes | `oltp_update_index` | 46.59ms | 96.60ms | 2.074× | 2.31% | PASS |
| mem_writes | `oltp_update_non_index` | 33.57ms | 63.88ms | 1.903× | 2.15% | PASS |
| mem_writes | `oltp_delete_insert` | 33.40ms | 74.34ms | 2.225× | 2.38% | PASS |
| mem_writes | `oltp_write_only` | 19.55ms | 47.06ms | 2.408× | 1.68% | PASS |
| mem_writes | `types_delete_insert` | 23.72ms | 43.11ms | 1.817× | 1.79% | PASS |
| mem_writes | `oltp_read_write` | 60.91ms | 94.80ms | 1.557× | 2.46% | PASS |
| file_reads | `oltp_point_select` | 74.36ms | 41.60ms | 0.559× | 1.87% | PASS |
| file_reads | `oltp_range_select` | 15.63ms | 10.80ms | 0.691× | 2.50% | PASS |
| file_reads | `oltp_sum_range` | 15.26ms | 10.88ms | 0.713× | 2.05% | PASS |
| file_reads | `oltp_order_range` | 2.88ms | 2.25ms | 0.781× | 2.14% | PASS |
| file_reads | `oltp_distinct_range` | 3.39ms | 2.78ms | 0.820× | 1.12% | PASS |
| file_reads | `oltp_index_scan` | 9.24ms | 6.43ms | 0.695× | 1.64% | PASS |
| file_reads | `select_random_points` | 19.39ms | 15.81ms | 0.815× | 3.55% | PASS |
| file_reads | `select_random_ranges` | 8.65ms | 5.47ms | 0.633× | 1.48% | PASS |
| file_reads | `covering_index_scan` | 9.78ms | 5.20ms | 0.532× | 2.18% | PASS |
| file_reads | `groupby_scan` | 21.30ms | 20.18ms | 0.947× | 1.27% | PASS |
| file_reads | `index_join` | 8.72ms | 7.95ms | 0.911× | 3.05% | PASS |
| file_reads | `index_join_scan` | 4.41ms | 4.66ms | 1.056× | 3.17% | PASS |
| file_reads | `types_table_scan` | 729.98ms | 767.23ms | 1.051× | 2.16% | PASS |
| file_reads | `table_scan` | 1.04s | 900.23ms | 0.867× | 6.56% | PASS |
| file_reads | `oltp_read_only` | 146.74ms | 99.75ms | 0.680× | 3.30% | PASS |
| file_writes | `oltp_bulk_insert` | 199.19ms | 296.84ms | 1.490× | 3.41% | PASS |
| file_writes | `oltp_insert` | 47.32ms | 53.97ms | 1.141× | 4.97% | PASS |
| file_writes | `oltp_update_index` | 191.65ms | 189.10ms | 0.987× | 2.33% | PASS |
| file_writes | `oltp_update_non_index` | 143.49ms | 131.57ms | 0.917× | 5.21% | PASS |
| file_writes | `oltp_delete_insert` | 151.79ms | 148.78ms | 0.980× | 3.54% | PASS |
| file_writes | `oltp_write_only` | 121.08ms | 109.91ms | 0.908× | 3.19% | PASS |
| file_writes | `types_delete_insert` | 100.07ms | 79.09ms | 0.790× | 9.10% | PASS |
| file_writes | `oltp_read_write` | 155.45ms | 154.19ms | 0.992× | 2.24% | PASS |
| ac_reads | `oltp_point_select` | 37.85ms | 40.95ms | 1.082× | 1.44% | PASS |
| ac_reads | `oltp_range_select` | 11.20ms | 10.56ms | 0.944× | 2.84% | PASS |
| ac_reads | `oltp_sum_range` | 11.69ms | 10.84ms | 0.927× | 2.96% | PASS |
| ac_reads | `oltp_order_range` | 2.41ms | 2.20ms | 0.911× | 1.93% | PASS |
| ac_reads | `oltp_distinct_range` | 3.05ms | 2.75ms | 0.904× | 1.89% | PASS |
| ac_reads | `oltp_index_scan` | 5.62ms | 6.27ms | 1.117× | 1.77% | PASS |
| ac_reads | `select_random_points` | 14.92ms | 15.27ms | 1.024× | 3.10% | PASS |
| ac_reads | `select_random_ranges` | 4.96ms | 5.41ms | 1.089× | 1.75% | PASS |
| ac_reads | `covering_index_scan` | 5.71ms | 4.96ms | 0.869× | 4.67% | PASS |
| ac_reads | `groupby_scan` | 20.21ms | 19.69ms | 0.974× | 1.67% | PASS |
| ac_reads | `index_join` | 6.31ms | 7.19ms | 1.138× | 3.97% | PASS |
| ac_reads | `index_join_scan` | 3.94ms | 4.45ms | 1.131× | 1.91% | PASS |
| ac_reads | `types_table_scan` | 718.57ms | 768.46ms | 1.069× | 1.47% | PASS |
| ac_reads | `table_scan` | 940.72ms | 887.38ms | 0.943× | 4.29% | PASS |
| ac_reads | `oltp_read_only` | 97.69ms | 99.33ms | 1.017× | 3.44% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.47ms | 53.48ms | 2.613× | 4.80% | PASS |
| ac_writes | `oltp_insert_ac` | 23.29ms | 65.67ms | 2.819× | 4.89% | PASS |
| ac_writes | `oltp_update_index_ac` | 24.34ms | 80.00ms | 3.287× | 3.75% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.37ms | 60.36ms | 2.963× | 3.73% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.05ms | 72.15ms | 3.271× | 4.49% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.38ms | 71.05ms | 3.174× | 4.22% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.34ms | 62.49ms | 3.073× | 6.52% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.45ms | 76.55ms | 2.894× | 3.58% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 27.26ms | 32.40ms | 1.189× | 1.19% | PASS |
| mem_reads | `oltp_range_select` | 11.81ms | 12.40ms | 1.050× | 1.49% | PASS |
| mem_reads | `oltp_sum_range` | 11.37ms | 12.28ms | 1.080× | 1.49% | PASS |
| mem_reads | `oltp_order_range` | 2.64ms | 2.84ms | 1.077× | 0.81% | PASS |
| mem_reads | `oltp_distinct_range` | 3.42ms | 3.67ms | 1.074× | 0.76% | PASS |
| mem_reads | `oltp_index_scan` | 4.05ms | 5.59ms | 1.381× | 1.39% | PASS |
| mem_reads | `select_random_points` | 16.68ms | 20.53ms | 1.231× | 1.41% | PASS |
| mem_reads | `select_random_ranges` | 3.52ms | 4.58ms | 1.300× | 1.24% | PASS |
| mem_reads | `covering_index_scan` | 4.03ms | 4.19ms | 1.039× | 1.70% | PASS |
| mem_reads | `groupby_scan` | 29.95ms | 30.64ms | 1.023× | 0.79% | PASS |
| mem_reads | `index_join` | 6.18ms | 9.11ms | 1.473× | 1.81% | PASS |
| mem_reads | `index_join_scan` | 3.58ms | 5.30ms | 1.479× | 1.80% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.14s | 1.077× | 0.72% | PASS |
| mem_reads | `table_scan` | 1.30s | 1.26s | 0.969× | 0.51% | PASS |
| mem_reads | `oltp_read_only` | 109.47ms | 123.11ms | 1.125× | 1.05% | PASS |
| mem_writes | `oltp_bulk_insert` | 205.78ms | 296.33ms | 1.440× | 1.12% | PASS |
| mem_writes | `oltp_insert` | 17.58ms | 33.93ms | 1.930× | 0.87% | PASS |
| mem_writes | `oltp_update_index` | 62.19ms | 120.91ms | 1.944× | 1.21% | PASS |
| mem_writes | `oltp_update_non_index` | 42.97ms | 72.76ms | 1.694× | 0.95% | PASS |
| mem_writes | `oltp_delete_insert` | 43.13ms | 91.47ms | 2.120× | 1.74% | PASS |
| mem_writes | `oltp_write_only` | 23.76ms | 53.27ms | 2.242× | 1.60% | PASS |
| mem_writes | `types_delete_insert` | 27.98ms | 47.51ms | 1.698× | 1.03% | PASS |
| mem_writes | `oltp_read_write` | 75.51ms | 124.34ms | 1.647× | 1.69% | PASS |
| file_reads | `oltp_point_select` | 61.42ms | 45.23ms | 0.736× | 1.48% | PASS |
| file_reads | `oltp_range_select` | 15.44ms | 13.91ms | 0.901× | 1.67% | PASS |
| file_reads | `oltp_sum_range` | 15.18ms | 13.86ms | 0.913× | 1.75% | PASS |
| file_reads | `oltp_order_range` | 3.07ms | 3.07ms | 0.999× | 1.64% | PASS |
| file_reads | `oltp_distinct_range` | 3.86ms | 3.92ms | 1.015× | 2.37% | PASS |
| file_reads | `oltp_index_scan` | 7.78ms | 7.00ms | 0.900× | 2.06% | PASS |
| file_reads | `select_random_points` | 21.11ms | 22.56ms | 1.068× | 1.85% | PASS |
| file_reads | `select_random_ranges` | 7.02ms | 5.92ms | 0.843× | 1.66% | PASS |
| file_reads | `covering_index_scan` | 7.73ms | 5.50ms | 0.711× | 1.73% | PASS |
| file_reads | `groupby_scan` | 30.26ms | 30.95ms | 1.023× | 0.54% | PASS |
| file_reads | `index_join` | 8.43ms | 9.93ms | 1.177× | 1.97% | PASS |
| file_reads | `index_join_scan` | 4.12ms | 5.41ms | 1.313× | 1.62% | PASS |
| file_reads | `types_table_scan` | 1.07s | 1.15s | 1.070× | 0.58% | PASS |
| file_reads | `table_scan` | 1.29s | 1.26s | 0.971× | 0.69% | PASS |
| file_reads | `oltp_read_only` | 160.86ms | 142.67ms | 0.887× | 1.24% | PASS |
| file_writes | `oltp_bulk_insert` | 219.64ms | 314.89ms | 1.434× | 0.91% | PASS |
| file_writes | `oltp_insert` | 25.34ms | 42.84ms | 1.691× | 2.32% | PASS |
| file_writes | `oltp_update_index` | 83.48ms | 142.50ms | 1.707× | 1.77% | PASS |
| file_writes | `oltp_update_non_index` | 61.49ms | 89.81ms | 1.460× | 1.82% | PASS |
| file_writes | `oltp_delete_insert` | 63.45ms | 110.72ms | 1.745× | 2.59% | PASS |
| file_writes | `oltp_write_only` | 39.85ms | 68.76ms | 1.726× | 2.23% | PASS |
| file_writes | `types_delete_insert` | 40.45ms | 60.03ms | 1.484× | 3.09% | PASS |
| file_writes | `oltp_read_write` | 94.68ms | 140.64ms | 1.485× | 1.44% | PASS |
| ac_reads | `oltp_point_select` | 37.73ms | 45.26ms | 1.200× | 1.40% | PASS |
| ac_reads | `oltp_range_select` | 13.10ms | 13.99ms | 1.068× | 1.09% | PASS |
| ac_reads | `oltp_sum_range` | 13.00ms | 14.04ms | 1.080× | 2.54% | PASS |
| ac_reads | `oltp_order_range` | 2.82ms | 3.10ms | 1.098× | 4.24% | PASS |
| ac_reads | `oltp_distinct_range` | 3.60ms | 3.99ms | 1.107× | 2.69% | PASS |
| ac_reads | `oltp_index_scan` | 5.43ms | 6.97ms | 1.284× | 1.46% | PASS |
| ac_reads | `select_random_points` | 18.04ms | 22.25ms | 1.234× | 1.94% | PASS |
| ac_reads | `select_random_ranges` | 4.71ms | 5.94ms | 1.261× | 1.44% | PASS |
| ac_reads | `covering_index_scan` | 5.49ms | 5.49ms | 0.999× | 1.60% | PASS |
| ac_reads | `groupby_scan` | 29.88ms | 30.84ms | 1.032× | 0.56% | PASS |
| ac_reads | `index_join` | 7.28ms | 9.85ms | 1.352× | 1.37% | PASS |
| ac_reads | `index_join_scan` | 3.90ms | 5.37ms | 1.375× | 1.70% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.14s | 1.076× | 0.67% | PASS |
| ac_reads | `table_scan` | 1.29s | 1.25s | 0.972× | 0.59% | PASS |
| ac_reads | `oltp_read_only` | 126.56ms | 143.48ms | 1.134× | 0.92% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.89ms | 52.16ms | 3.088× | 10.74% | PASS |
| ac_writes | `oltp_insert_ac` | 19.73ms | 71.63ms | 3.631× | 8.07% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.14ms | 81.26ms | 3.844× | 9.01% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.51ms | 63.18ms | 3.608× | 11.35% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.82ms | 72.46ms | 4.066× | 10.88% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.74ms | 73.75ms | 3.935× | 10.83% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.21ms | 65.03ms | 3.777× | 11.36% | PASS |
| ac_writes | `oltp_read_write_ac` | 23.68ms | 79.94ms | 3.376× | 8.36% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.52ms | 39.96ms | 1.229× | 1.41% | PASS |
| mem_reads | `oltp_range_select` | 18.64ms | 21.13ms | 1.134× | 1.41% | PASS |
| mem_reads | `oltp_sum_range` | 17.83ms | 20.79ms | 1.166× | 0.92% | PASS |
| mem_reads | `oltp_order_range` | 3.51ms | 3.82ms | 1.086× | 1.56% | PASS |
| mem_reads | `oltp_distinct_range` | 4.68ms | 4.92ms | 1.051× | 0.95% | PASS |
| mem_reads | `oltp_index_scan` | 4.55ms | 6.13ms | 1.347× | 2.04% | PASS |
| mem_reads | `select_random_points` | 27.66ms | 31.84ms | 1.151× | 1.55% | PASS |
| mem_reads | `select_random_ranges` | 7.62ms | 8.98ms | 1.179× | 1.28% | PASS |
| mem_reads | `covering_index_scan` | 4.19ms | 4.19ms | 1.000× | 1.61% | PASS |
| mem_reads | `groupby_scan` | 36.18ms | 39.23ms | 1.085× | 0.78% | PASS |
| mem_reads | `index_join` | 8.21ms | 10.30ms | 1.256× | 1.25% | PASS |
| mem_reads | `index_join_scan` | 4.16ms | 5.53ms | 1.329× | 1.92% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.24s | 1.191× | 0.63% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.39s | 1.181× | 0.53% | PASS |
| mem_reads | `oltp_read_only` | 148.53ms | 168.12ms | 1.132× | 1.27% | PASS |
| mem_writes | `oltp_bulk_insert` | 243.13ms | 351.81ms | 1.447× | 0.98% | PASS |
| mem_writes | `oltp_insert` | 19.00ms | 36.08ms | 1.898× | 0.97% | PASS |
| mem_writes | `oltp_update_index` | 67.58ms | 115.36ms | 1.707× | 0.94% | PASS |
| mem_writes | `oltp_update_non_index` | 49.82ms | 81.81ms | 1.642× | 0.88% | PASS |
| mem_writes | `oltp_delete_insert` | 49.70ms | 94.92ms | 1.910× | 1.38% | PASS |
| mem_writes | `oltp_write_only` | 26.95ms | 56.83ms | 2.109× | 1.45% | PASS |
| mem_writes | `types_delete_insert` | 32.40ms | 54.08ms | 1.669× | 1.76% | PASS |
| mem_writes | `oltp_read_write` | 101.05ms | 153.85ms | 1.522× | 1.35% | PASS |
| file_reads | `oltp_point_select` | 107.34ms | 66.04ms | 0.615× | 0.82% | PASS |
| file_reads | `oltp_range_select` | 27.51ms | 24.34ms | 0.885× | 1.69% | PASS |
| file_reads | `oltp_sum_range` | 26.34ms | 24.07ms | 0.914× | 1.29% | PASS |
| file_reads | `oltp_order_range` | 4.62ms | 4.34ms | 0.940× | 1.86% | PASS |
| file_reads | `oltp_distinct_range` | 5.81ms | 5.47ms | 0.941× | 2.52% | PASS |
| file_reads | `oltp_index_scan` | 12.44ms | 8.97ms | 0.721× | 1.19% | PASS |
| file_reads | `select_random_points` | 37.94ms | 35.99ms | 0.948× | 1.04% | PASS |
| file_reads | `select_random_ranges` | 15.71ms | 12.03ms | 0.766× | 1.02% | PASS |
| file_reads | `covering_index_scan` | 12.27ms | 6.96ms | 0.568× | 1.99% | PASS |
| file_reads | `groupby_scan` | 37.84ms | 40.29ms | 1.065× | 0.89% | PASS |
| file_reads | `index_join` | 12.84ms | 12.77ms | 0.995× | 1.42% | PASS |
| file_reads | `index_join_scan` | 5.26ms | 6.08ms | 1.155× | 1.39% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.23s | 1.189× | 0.31% | PASS |
| file_reads | `table_scan` | 1.18s | 1.39s | 1.174× | 0.66% | PASS |
| file_reads | `oltp_read_only` | 262.56ms | 207.89ms | 0.792× | 1.01% | PASS |
| file_writes | `oltp_bulk_insert` | 258.01ms | 373.85ms | 1.449× | 0.96% | PASS |
| file_writes | `oltp_insert` | 29.11ms | 45.89ms | 1.577× | 1.11% | PASS |
| file_writes | `oltp_update_index` | 98.17ms | 140.87ms | 1.435× | 1.00% | PASS |
| file_writes | `oltp_update_non_index` | 80.31ms | 103.13ms | 1.284× | 1.02% | PASS |
| file_writes | `oltp_delete_insert` | 81.43ms | 118.36ms | 1.454× | 0.98% | PASS |
| file_writes | `oltp_write_only` | 57.83ms | 76.41ms | 1.321× | 1.69% | PASS |
| file_writes | `types_delete_insert` | 51.43ms | 67.86ms | 1.320× | 1.62% | PASS |
| file_writes | `oltp_read_write` | 135.35ms | 173.52ms | 1.282× | 0.98% | PASS |
| ac_reads | `oltp_point_select` | 57.11ms | 65.71ms | 1.150× | 0.97% | PASS |
| ac_reads | `oltp_range_select` | 21.79ms | 24.15ms | 1.108× | 1.67% | PASS |
| ac_reads | `oltp_sum_range` | 20.89ms | 23.96ms | 1.147× | 1.41% | PASS |
| ac_reads | `oltp_order_range` | 4.05ms | 4.32ms | 1.068× | 1.81% | PASS |
| ac_reads | `oltp_distinct_range` | 5.15ms | 5.45ms | 1.058× | 1.66% | PASS |
| ac_reads | `oltp_index_scan` | 7.22ms | 8.95ms | 1.240× | 1.25% | PASS |
| ac_reads | `select_random_points` | 31.41ms | 35.91ms | 1.143× | 1.00% | PASS |
| ac_reads | `select_random_ranges` | 10.35ms | 11.98ms | 1.158× | 1.01% | PASS |
| ac_reads | `covering_index_scan` | 6.98ms | 6.96ms | 0.997× | 1.99% | PASS |
| ac_reads | `groupby_scan` | 36.98ms | 40.17ms | 1.086× | 0.71% | PASS |
| ac_reads | `index_join` | 9.82ms | 12.63ms | 1.287× | 1.14% | PASS |
| ac_reads | `index_join_scan` | 4.67ms | 6.10ms | 1.306× | 1.34% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.23s | 1.192× | 0.45% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.39s | 1.177× | 0.53% | PASS |
| ac_reads | `oltp_read_only` | 186.57ms | 207.53ms | 1.112× | 1.00% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.00ms | 63.67ms | 2.769× | 5.25% | PASS |
| ac_writes | `oltp_insert_ac` | 23.65ms | 82.97ms | 3.509× | 3.43% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.70ms | 96.11ms | 3.599× | 5.93% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.59ms | 74.66ms | 3.305× | 3.90% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.74ms | 86.31ms | 3.636× | 4.70% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.22ms | 85.54ms | 3.532× | 5.83% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.81ms | 74.53ms | 3.417× | 8.85% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.20ms | 94.25ms | 3.121× | 3.79% | PASS |

</details>

## Version-control latency

Wall time: 1m 41s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 54.97ms | 200.00ms | 27.5% | 0.85% | PASS |
| `status_dirty_many_tables` | 57.11ms | 200.00ms | 28.6% | 1.48% | PASS |
| `diff_regular_working_one_table` | 52.02ms | 150.00ms | 34.7% | 0.47% | PASS |
| `diff_regular_working_many_tables` | 59.65ms | 200.00ms | 29.8% | 0.73% | PASS |
| `diff_stat_working_many_tables` | 60.04ms | 200.00ms | 30.0% | 1.18% | PASS |
| `diff_schema_working_many_tables` | 62.15ms | 200.00ms | 31.1% | 0.91% | PASS |
| `branch_list_many_branches` | 16.71ms | 100.00ms | 16.7% | 1.58% | PASS |
| `branch_create_delete` | 18.35ms | 100.00ms | 18.4% | 2.39% | PASS |
| `checkout_branch_clean` | 75.25ms | 200.00ms | 37.6% | 4.43% | PASS |
| `merge_data_no_conflicts` | 28.15ms | 150.00ms | 18.8% | 5.50% | PASS |
| `merge_schema_no_conflicts` | 15.86ms | 100.00ms | 15.9% | 2.27% | PASS |
| `merge_data_conflicts` | 76.27ms | 250.00ms | 30.5% | 1.07% | PASS |
| `merge_data_conflicts_with_resolve` | 77.02ms | 250.00ms | 30.8% | 1.11% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
