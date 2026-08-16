# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-16 11:29 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260810.271.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/31940191556)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 12s | 8.78s | 11.45s | 1.304× | 1.26% | **PASS** |
| textpk | 69 | 55 | 1h 33m 13s | 10.11s | 11.93s | 1.181× | 1.95% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 33s | 9.45s | 12.15s | 1.285× | 1.19% | **PASS** |
| compositepk | 69 | 55 | 1h 28m 28s | 9.94s | 12.26s | 1.233× | 1.64% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.64ms | 29.41ms | 1.244× | 1.61% | PASS |
| mem_reads | `oltp_range_select` | 10.07ms | 13.11ms | 1.301× | 1.97% | PASS |
| mem_reads | `oltp_sum_range` | 9.45ms | 12.32ms | 1.303× | 1.57% | PASS |
| mem_reads | `oltp_order_range` | 2.52ms | 3.02ms | 1.199× | 1.07% | PASS |
| mem_reads | `oltp_distinct_range` | 3.61ms | 4.07ms | 1.127× | 0.86% | PASS |
| mem_reads | `oltp_index_scan` | 3.90ms | 5.27ms | 1.353× | 1.20% | PASS |
| mem_reads | `select_random_points` | 9.98ms | 11.06ms | 1.108× | 2.46% | PASS |
| mem_reads | `select_random_ranges` | 2.96ms | 3.96ms | 1.338× | 1.67% | PASS |
| mem_reads | `covering_index_scan` | 4.23ms | 4.12ms | 0.973× | 0.93% | PASS |
| mem_reads | `groupby_scan` | 29.66ms | 32.75ms | 1.104× | 0.86% | PASS |
| mem_reads | `index_join` | 6.01ms | 8.05ms | 1.338× | 1.08% | PASS |
| mem_reads | `index_join_scan` | 3.50ms | 4.53ms | 1.296× | 1.90% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.33s | 1.286× | 0.63% | PASS |
| mem_reads | `table_scan` | 1.16s | 1.39s | 1.196× | 0.42% | PASS |
| mem_reads | `oltp_read_only` | 101.91ms | 123.99ms | 1.217× | 0.83% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.73ms | 251.05ms | 1.389× | 1.22% | PASS |
| mem_writes | `oltp_insert` | 15.50ms | 28.39ms | 1.832× | 0.84% | PASS |
| mem_writes | `oltp_update_index` | 50.31ms | 103.64ms | 2.060× | 1.08% | PASS |
| mem_writes | `oltp_update_non_index` | 34.17ms | 58.83ms | 1.721× | 1.24% | PASS |
| mem_writes | `oltp_delete_insert` | 45.23ms | 78.33ms | 1.732× | 0.90% | PASS |
| mem_writes | `oltp_write_only` | 21.80ms | 44.53ms | 2.043× | 1.35% | PASS |
| mem_writes | `types_delete_insert` | 24.83ms | 40.06ms | 1.614× | 1.25% | PASS |
| mem_writes | `oltp_read_write` | 66.58ms | 109.97ms | 1.652× | 1.14% | PASS |
| file_reads | `oltp_point_select` | 97.71ms | 55.06ms | 0.563× | 0.52% | PASS |
| file_reads | `oltp_range_select` | 17.74ms | 15.76ms | 0.888× | 1.37% | PASS |
| file_reads | `oltp_sum_range` | 17.18ms | 15.15ms | 0.882× | 1.80% | PASS |
| file_reads | `oltp_order_range` | 3.39ms | 3.34ms | 0.985× | 1.38% | PASS |
| file_reads | `oltp_distinct_range` | 4.47ms | 4.40ms | 0.984× | 1.31% | PASS |
| file_reads | `oltp_index_scan` | 11.50ms | 8.26ms | 0.718× | 1.51% | PASS |
| file_reads | `select_random_points` | 17.79ms | 13.84ms | 0.778× | 2.37% | PASS |
| file_reads | `select_random_ranges` | 10.44ms | 6.61ms | 0.633× | 1.03% | PASS |
| file_reads | `covering_index_scan` | 11.79ms | 7.05ms | 0.598× | 0.97% | PASS |
| file_reads | `groupby_scan` | 30.53ms | 33.15ms | 1.086× | 0.80% | PASS |
| file_reads | `index_join` | 10.22ms | 10.12ms | 0.990× | 1.78% | PASS |
| file_reads | `index_join_scan` | 4.51ms | 4.98ms | 1.102× | 2.25% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.33s | 1.284× | 0.42% | PASS |
| file_reads | `table_scan` | 1.16s | 1.39s | 1.196× | 0.46% | PASS |
| file_reads | `oltp_read_only` | 210.14ms | 161.66ms | 0.769× | 0.80% | PASS |
| file_writes | `oltp_bulk_insert` | 194.24ms | 269.89ms | 1.390× | 0.90% | PASS |
| file_writes | `oltp_insert` | 22.18ms | 35.73ms | 1.611× | 1.62% | PASS |
| file_writes | `oltp_update_index` | 77.12ms | 126.94ms | 1.646× | 1.28% | PASS |
| file_writes | `oltp_update_non_index` | 57.01ms | 80.75ms | 1.417× | 1.22% | PASS |
| file_writes | `oltp_delete_insert` | 68.18ms | 98.31ms | 1.442× | 1.38% | PASS |
| file_writes | `oltp_write_only` | 44.64ms | 63.94ms | 1.432× | 1.56% | PASS |
| file_writes | `types_delete_insert` | 39.82ms | 53.49ms | 1.343× | 1.59% | PASS |
| file_writes | `oltp_read_write` | 92.10ms | 129.61ms | 1.407× | 1.60% | PASS |
| ac_reads | `oltp_point_select` | 48.16ms | 55.13ms | 1.145× | 0.89% | PASS |
| ac_reads | `oltp_range_select` | 12.79ms | 15.81ms | 1.237× | 1.51% | PASS |
| ac_reads | `oltp_sum_range` | 12.28ms | 15.17ms | 1.236× | 1.08% | PASS |
| ac_reads | `oltp_order_range` | 2.87ms | 3.29ms | 1.148× | 1.26% | PASS |
| ac_reads | `oltp_distinct_range` | 3.96ms | 4.36ms | 1.101× | 1.08% | PASS |
| ac_reads | `oltp_index_scan` | 6.55ms | 8.29ms | 1.267× | 1.62% | PASS |
| ac_reads | `select_random_points` | 12.81ms | 13.90ms | 1.086× | 0.97% | PASS |
| ac_reads | `select_random_ranges` | 5.49ms | 6.60ms | 1.202× | 0.85% | PASS |
| ac_reads | `covering_index_scan` | 6.87ms | 7.03ms | 1.024× | 1.42% | PASS |
| ac_reads | `groupby_scan` | 30.02ms | 33.09ms | 1.102× | 1.21% | PASS |
| ac_reads | `index_join` | 7.58ms | 10.19ms | 1.345× | 1.69% | PASS |
| ac_reads | `index_join_scan` | 3.91ms | 5.00ms | 1.281× | 1.69% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.33s | 1.286× | 0.55% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.39s | 1.195× | 0.53% | PASS |
| ac_reads | `oltp_read_only` | 138.04ms | 161.12ms | 1.167× | 0.88% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.87ms | 80.71ms | 3.691× | 4.50% | PASS |
| ac_writes | `oltp_insert_ac` | 24.43ms | 96.63ms | 3.955× | 3.11% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.01ms | 109.89ms | 4.226× | 3.82% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.66ms | 89.85ms | 3.964× | 7.19% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.43ms | 101.98ms | 4.175× | 5.54% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.96ms | 99.68ms | 3.994× | 5.36% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.96ms | 90.43ms | 4.118× | 5.31% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.42ms | 106.52ms | 3.502× | 4.73% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.29ms | 38.72ms | 1.237× | 1.72% | PASS |
| mem_reads | `oltp_range_select` | 14.07ms | 14.54ms | 1.033× | 2.70% | PASS |
| mem_reads | `oltp_sum_range` | 12.71ms | 14.49ms | 1.141× | 2.31% | PASS |
| mem_reads | `oltp_order_range` | 3.03ms | 3.22ms | 1.061× | 1.41% | PASS |
| mem_reads | `oltp_distinct_range` | 4.05ms | 4.25ms | 1.051× | 1.12% | PASS |
| mem_reads | `oltp_index_scan` | 4.66ms | 6.65ms | 1.428× | 2.08% | PASS |
| mem_reads | `select_random_points` | 18.44ms | 21.78ms | 1.181× | 2.26% | PASS |
| mem_reads | `select_random_ranges` | 4.15ms | 5.29ms | 1.273× | 2.41% | PASS |
| mem_reads | `covering_index_scan` | 4.81ms | 4.80ms | 0.999× | 1.96% | PASS |
| mem_reads | `groupby_scan` | 32.68ms | 34.27ms | 1.049× | 1.00% | PASS |
| mem_reads | `index_join` | 7.10ms | 9.99ms | 1.407× | 2.85% | PASS |
| mem_reads | `index_join_scan` | 4.68ms | 5.67ms | 1.210× | 2.56% | PASS |
| mem_reads | `types_table_scan` | 1.15s | 1.26s | 1.092× | 1.50% | PASS |
| mem_reads | `table_scan` | 1.48s | 1.41s | 0.951× | 2.22% | PASS |
| mem_reads | `oltp_read_only` | 120.84ms | 138.44ms | 1.146× | 1.58% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.06ms | 359.34ms | 1.529× | 1.08% | PASS |
| mem_writes | `oltp_insert` | 21.50ms | 40.16ms | 1.868× | 0.81% | PASS |
| mem_writes | `oltp_update_index` | 69.77ms | 133.27ms | 1.910× | 1.23% | PASS |
| mem_writes | `oltp_update_non_index` | 48.21ms | 87.39ms | 1.813× | 1.21% | PASS |
| mem_writes | `oltp_delete_insert` | 52.32ms | 106.49ms | 2.035× | 2.14% | PASS |
| mem_writes | `oltp_write_only` | 29.93ms | 63.50ms | 2.122× | 2.34% | PASS |
| mem_writes | `types_delete_insert` | 33.14ms | 55.94ms | 1.688× | 1.56% | PASS |
| mem_writes | `oltp_read_write` | 86.82ms | 144.31ms | 1.662× | 1.95% | PASS |
| file_reads | `oltp_point_select` | 105.29ms | 63.90ms | 0.607× | 1.18% | PASS |
| file_reads | `oltp_range_select` | 20.55ms | 17.04ms | 0.829× | 2.24% | PASS |
| file_reads | `oltp_sum_range` | 19.65ms | 17.37ms | 0.884× | 1.72% | PASS |
| file_reads | `oltp_order_range` | 3.71ms | 3.50ms | 0.945× | 1.78% | PASS |
| file_reads | `oltp_distinct_range` | 4.77ms | 4.58ms | 0.960× | 1.08% | PASS |
| file_reads | `oltp_index_scan` | 12.24ms | 9.24ms | 0.755× | 1.77% | PASS |
| file_reads | `select_random_points` | 26.36ms | 24.72ms | 0.938× | 1.40% | PASS |
| file_reads | `select_random_ranges` | 11.51ms | 7.88ms | 0.685× | 1.46% | PASS |
| file_reads | `covering_index_scan` | 12.34ms | 7.36ms | 0.596× | 2.14% | PASS |
| file_reads | `groupby_scan` | 32.65ms | 34.41ms | 1.054× | 1.15% | PASS |
| file_reads | `index_join` | 11.29ms | 11.40ms | 1.009× | 1.54% | PASS |
| file_reads | `index_join_scan` | 5.68ms | 5.85ms | 1.030× | 3.06% | PASS |
| file_reads | `types_table_scan` | 1.07s | 1.23s | 1.156× | 1.64% | PASS |
| file_reads | `table_scan` | 1.36s | 1.39s | 1.023× | 4.84% | PASS |
| file_reads | `oltp_read_only` | 235.39ms | 179.15ms | 0.761× | 1.26% | PASS |
| file_writes | `oltp_bulk_insert` | 255.17ms | 389.21ms | 1.525× | 1.15% | PASS |
| file_writes | `oltp_insert` | 49.71ms | 53.34ms | 1.073× | 20.63% | PASS |
| file_writes | `oltp_update_index` | 115.52ms | 171.87ms | 1.488× | 1.42% | PASS |
| file_writes | `oltp_update_non_index` | 95.59ms | 113.49ms | 1.187× | 9.02% | PASS |
| file_writes | `oltp_delete_insert` | 87.74ms | 134.20ms | 1.529× | 0.94% | PASS |
| file_writes | `oltp_write_only` | 85.43ms | 86.32ms | 1.010× | 11.93% | PASS |
| file_writes | `types_delete_insert` | 54.98ms | 74.74ms | 1.360× | 1.73% | PASS |
| file_writes | `oltp_read_write` | 141.43ms | 167.33ms | 1.183× | 6.29% | PASS |
| ac_reads | `oltp_point_select` | 55.44ms | 63.64ms | 1.148× | 0.84% | PASS |
| ac_reads | `oltp_range_select` | 16.95ms | 17.01ms | 1.004× | 2.36% | PASS |
| ac_reads | `oltp_sum_range` | 15.99ms | 17.18ms | 1.074× | 2.32% | PASS |
| ac_reads | `oltp_order_range` | 3.37ms | 3.51ms | 1.041× | 2.55% | PASS |
| ac_reads | `oltp_distinct_range` | 4.41ms | 4.58ms | 1.037× | 1.08% | PASS |
| ac_reads | `oltp_index_scan` | 7.73ms | 9.30ms | 1.203× | 1.48% | PASS |
| ac_reads | `select_random_points` | 21.56ms | 24.86ms | 1.153× | 2.37% | PASS |
| ac_reads | `select_random_ranges` | 6.64ms | 7.89ms | 1.188× | 1.69% | PASS |
| ac_reads | `covering_index_scan` | 8.44ms | 7.42ms | 0.880× | 1.81% | PASS |
| ac_reads | `groupby_scan` | 32.68ms | 34.44ms | 1.054× | 0.87% | PASS |
| ac_reads | `index_join` | 9.35ms | 11.33ms | 1.212× | 3.21% | PASS |
| ac_reads | `index_join_scan` | 5.15ms | 5.90ms | 1.147× | 3.30% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.22s | 1.168× | 0.85% | PASS |
| ac_reads | `table_scan` | 1.23s | 1.37s | 1.118× | 2.14% | PASS |
| ac_reads | `oltp_read_only` | 154.13ms | 176.54ms | 1.145× | 1.13% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.88ms | 79.91ms | 3.493× | 7.24% | PASS |
| ac_writes | `oltp_insert_ac` | 23.91ms | 96.07ms | 4.019× | 4.96% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.91ms | 112.07ms | 4.015× | 5.47% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.30ms | 91.30ms | 3.918× | 5.20% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.52ms | 103.30ms | 4.214× | 6.10% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.12ms | 102.91ms | 4.097× | 5.93% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.79ms | 95.45ms | 4.381× | 8.65% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.23ms | 110.38ms | 3.425× | 4.88% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.29ms | 36.89ms | 1.259× | 1.64% | PASS |
| mem_reads | `oltp_range_select` | 12.07ms | 13.99ms | 1.159× | 1.55% | PASS |
| mem_reads | `oltp_sum_range` | 11.31ms | 14.00ms | 1.238× | 1.72% | PASS |
| mem_reads | `oltp_order_range` | 2.83ms | 3.13ms | 1.106× | 1.14% | PASS |
| mem_reads | `oltp_distinct_range` | 3.87ms | 4.21ms | 1.088× | 1.06% | PASS |
| mem_reads | `oltp_index_scan` | 4.34ms | 6.07ms | 1.398× | 1.76% | PASS |
| mem_reads | `select_random_points` | 17.15ms | 20.64ms | 1.204× | 2.22% | PASS |
| mem_reads | `select_random_ranges` | 3.81ms | 5.13ms | 1.347× | 1.93% | PASS |
| mem_reads | `covering_index_scan` | 4.38ms | 4.33ms | 0.987× | 1.19% | PASS |
| mem_reads | `groupby_scan` | 31.12ms | 33.71ms | 1.083× | 0.77% | PASS |
| mem_reads | `index_join` | 6.73ms | 8.88ms | 1.319× | 1.18% | PASS |
| mem_reads | `index_join_scan` | 3.94ms | 5.37ms | 1.362× | 3.45% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.22s | 1.172× | 0.66% | PASS |
| mem_reads | `table_scan` | 1.19s | 1.36s | 1.149× | 0.57% | PASS |
| mem_reads | `oltp_read_only` | 114.43ms | 134.60ms | 1.176× | 0.97% | PASS |
| mem_writes | `oltp_bulk_insert` | 242.13ms | 351.76ms | 1.453× | 0.84% | PASS |
| mem_writes | `oltp_insert` | 19.84ms | 39.56ms | 1.994× | 0.63% | PASS |
| mem_writes | `oltp_update_index` | 66.19ms | 128.78ms | 1.946× | 1.01% | PASS |
| mem_writes | `oltp_update_non_index` | 47.21ms | 83.91ms | 1.777× | 1.28% | PASS |
| mem_writes | `oltp_delete_insert` | 47.60ms | 101.98ms | 2.142× | 0.95% | PASS |
| mem_writes | `oltp_write_only` | 27.01ms | 61.30ms | 2.270× | 0.51% | PASS |
| mem_writes | `types_delete_insert` | 31.77ms | 53.37ms | 1.680× | 1.92% | PASS |
| mem_writes | `oltp_read_write` | 80.68ms | 137.60ms | 1.706× | 1.37% | PASS |
| file_reads | `oltp_point_select` | 105.76ms | 63.01ms | 0.596× | 0.63% | PASS |
| file_reads | `oltp_range_select` | 21.28ms | 16.81ms | 0.790× | 2.10% | PASS |
| file_reads | `oltp_sum_range` | 20.25ms | 16.90ms | 0.835× | 1.53% | PASS |
| file_reads | `oltp_order_range` | 3.87ms | 3.48ms | 0.898× | 1.05% | PASS |
| file_reads | `oltp_distinct_range` | 4.96ms | 4.57ms | 0.921× | 0.94% | PASS |
| file_reads | `oltp_index_scan` | 12.59ms | 9.03ms | 0.717× | 0.93% | PASS |
| file_reads | `select_random_points` | 27.35ms | 24.21ms | 0.885× | 2.05% | PASS |
| file_reads | `select_random_ranges` | 11.86ms | 7.83ms | 0.660× | 0.67% | PASS |
| file_reads | `covering_index_scan` | 12.84ms | 7.22ms | 0.562× | 1.19% | PASS |
| file_reads | `groupby_scan` | 32.74ms | 34.16ms | 1.043× | 0.85% | PASS |
| file_reads | `index_join` | 11.82ms | 11.10ms | 0.939× | 1.22% | PASS |
| file_reads | `index_join_scan` | 5.32ms | 5.89ms | 1.107× | 2.29% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.22s | 1.178× | 0.51% | PASS |
| file_reads | `table_scan` | 1.18s | 1.36s | 1.157× | 0.49% | PASS |
| file_reads | `oltp_read_only` | 227.38ms | 173.56ms | 0.763× | 0.82% | PASS |
| file_writes | `oltp_bulk_insert` | 262.88ms | 378.01ms | 1.438× | 0.89% | PASS |
| file_writes | `oltp_insert` | 37.56ms | 52.07ms | 1.386× | 2.14% | PASS |
| file_writes | `oltp_update_index` | 104.42ms | 161.87ms | 1.550× | 0.92% | PASS |
| file_writes | `oltp_update_non_index` | 84.46ms | 107.61ms | 1.274× | 1.37% | PASS |
| file_writes | `oltp_delete_insert` | 85.33ms | 129.42ms | 1.517× | 1.11% | PASS |
| file_writes | `oltp_write_only` | 62.81ms | 83.62ms | 1.331× | 1.76% | PASS |
| file_writes | `types_delete_insert` | 54.58ms | 71.29ms | 1.306× | 1.04% | PASS |
| file_writes | `oltp_read_write` | 120.59ms | 159.93ms | 1.326× | 1.44% | PASS |
| ac_reads | `oltp_point_select` | 54.76ms | 63.17ms | 1.154× | 0.81% | PASS |
| ac_reads | `oltp_range_select` | 15.45ms | 16.89ms | 1.093× | 1.21% | PASS |
| ac_reads | `oltp_sum_range` | 14.76ms | 16.90ms | 1.145× | 1.21% | PASS |
| ac_reads | `oltp_order_range` | 3.21ms | 3.47ms | 1.079× | 1.30% | PASS |
| ac_reads | `oltp_distinct_range` | 4.30ms | 4.57ms | 1.064× | 1.24% | PASS |
| ac_reads | `oltp_index_scan` | 7.15ms | 9.03ms | 1.262× | 1.00% | PASS |
| ac_reads | `select_random_points` | 20.95ms | 24.16ms | 1.153× | 1.28% | PASS |
| ac_reads | `select_random_ranges` | 6.67ms | 7.83ms | 1.174× | 0.93% | PASS |
| ac_reads | `covering_index_scan` | 7.13ms | 7.26ms | 1.018× | 1.30% | PASS |
| ac_reads | `groupby_scan` | 31.75ms | 34.20ms | 1.077× | 0.87% | PASS |
| ac_reads | `index_join` | 8.54ms | 11.21ms | 1.312× | 0.88% | PASS |
| ac_reads | `index_join_scan` | 4.68ms | 5.92ms | 1.265× | 1.30% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.22s | 1.181× | 0.46% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.36s | 1.159× | 0.51% | PASS |
| ac_reads | `oltp_read_only` | 152.94ms | 172.99ms | 1.131× | 0.58% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 47.11ms | 151.30ms | 3.212× | 16.56% | PASS |
| ac_writes | `oltp_insert_ac` | 43.12ms | 156.87ms | 3.638× | 12.96% | PASS |
| ac_writes | `oltp_update_index_ac` | 44.98ms | 163.70ms | 3.639× | 14.41% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 41.76ms | 146.66ms | 3.512× | 17.23% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 42.20ms | 163.81ms | 3.881× | 17.42% | PASS |
| ac_writes | `oltp_write_only_ac` | 44.47ms | 158.21ms | 3.558× | 19.27% | PASS |
| ac_writes | `types_delete_insert_ac` | 39.93ms | 153.00ms | 3.832× | 14.52% | PASS |
| ac_writes | `oltp_read_write_ac` | 50.05ms | 160.87ms | 3.214× | 15.55% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.20ms | 40.91ms | 1.196× | 1.60% | PASS |
| mem_reads | `oltp_range_select` | 19.44ms | 21.81ms | 1.122× | 2.65% | PASS |
| mem_reads | `oltp_sum_range` | 18.24ms | 20.96ms | 1.149× | 1.38% | PASS |
| mem_reads | `oltp_order_range` | 3.57ms | 3.99ms | 1.120× | 1.20% | PASS |
| mem_reads | `oltp_distinct_range` | 4.66ms | 5.05ms | 1.083× | 1.13% | PASS |
| mem_reads | `oltp_index_scan` | 4.61ms | 6.19ms | 1.342× | 3.11% | PASS |
| mem_reads | `select_random_points` | 28.14ms | 32.03ms | 1.138× | 2.72% | PASS |
| mem_reads | `select_random_ranges` | 7.85ms | 9.20ms | 1.172× | 1.12% | PASS |
| mem_reads | `covering_index_scan` | 4.38ms | 4.78ms | 1.090× | 3.04% | PASS |
| mem_reads | `groupby_scan` | 37.25ms | 38.98ms | 1.047× | 0.89% | PASS |
| mem_reads | `index_join` | 8.26ms | 10.54ms | 1.277× | 1.83% | PASS |
| mem_reads | `index_join_scan` | 4.24ms | 5.42ms | 1.278× | 2.34% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.25s | 1.193× | 1.43% | PASS |
| mem_reads | `table_scan` | 1.19s | 1.39s | 1.172× | 1.79% | PASS |
| mem_reads | `oltp_read_only` | 152.79ms | 172.80ms | 1.131× | 1.61% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.37ms | 362.07ms | 1.452× | 1.41% | PASS |
| mem_writes | `oltp_insert` | 19.63ms | 37.46ms | 1.908× | 1.40% | PASS |
| mem_writes | `oltp_update_index` | 68.56ms | 116.62ms | 1.701× | 1.33% | PASS |
| mem_writes | `oltp_update_non_index` | 51.12ms | 82.73ms | 1.618× | 1.46% | PASS |
| mem_writes | `oltp_delete_insert` | 50.25ms | 95.89ms | 1.908× | 0.88% | PASS |
| mem_writes | `oltp_write_only` | 27.66ms | 58.42ms | 2.112× | 1.72% | PASS |
| mem_writes | `types_delete_insert` | 33.57ms | 56.07ms | 1.670× | 1.49% | PASS |
| mem_writes | `oltp_read_write` | 106.54ms | 158.41ms | 1.487× | 1.28% | PASS |
| file_reads | `oltp_point_select` | 110.84ms | 67.19ms | 0.606× | 1.13% | PASS |
| file_reads | `oltp_range_select` | 28.12ms | 25.06ms | 0.891× | 2.36% | PASS |
| file_reads | `oltp_sum_range` | 26.61ms | 24.21ms | 0.910× | 1.19% | PASS |
| file_reads | `oltp_order_range` | 4.53ms | 4.37ms | 0.964× | 1.42% | PASS |
| file_reads | `oltp_distinct_range` | 5.70ms | 5.45ms | 0.956× | 0.94% | PASS |
| file_reads | `oltp_index_scan` | 12.58ms | 9.18ms | 0.729× | 1.48% | PASS |
| file_reads | `select_random_points` | 39.37ms | 36.85ms | 0.936× | 1.50% | PASS |
| file_reads | `select_random_ranges` | 15.92ms | 12.13ms | 0.762× | 1.22% | PASS |
| file_reads | `covering_index_scan` | 11.89ms | 7.27ms | 0.611× | 1.26% | PASS |
| file_reads | `groupby_scan` | 38.11ms | 39.58ms | 1.039× | 0.87% | PASS |
| file_reads | `index_join` | 12.86ms | 12.92ms | 1.005× | 1.78% | PASS |
| file_reads | `index_join_scan` | 5.47ms | 6.08ms | 1.111× | 2.52% | PASS |
| file_reads | `types_table_scan` | 1.13s | 1.26s | 1.118× | 1.19% | PASS |
| file_reads | `table_scan` | 1.34s | 1.42s | 1.055× | 2.32% | PASS |
| file_reads | `oltp_read_only` | 274.38ms | 216.54ms | 0.789× | 1.01% | PASS |
| file_writes | `oltp_bulk_insert` | 264.61ms | 384.68ms | 1.454× | 1.16% | PASS |
| file_writes | `oltp_insert` | 26.63ms | 46.98ms | 1.764× | 2.08% | PASS |
| file_writes | `oltp_update_index` | 99.32ms | 145.24ms | 1.462× | 1.40% | PASS |
| file_writes | `oltp_update_non_index` | 77.08ms | 104.40ms | 1.354× | 2.07% | PASS |
| file_writes | `oltp_delete_insert` | 78.85ms | 121.03ms | 1.535× | 1.84% | PASS |
| file_writes | `oltp_write_only` | 51.55ms | 78.15ms | 1.516× | 1.98% | PASS |
| file_writes | `types_delete_insert` | 51.19ms | 69.99ms | 1.367× | 1.97% | PASS |
| file_writes | `oltp_read_write` | 128.83ms | 176.65ms | 1.371× | 2.49% | PASS |
| ac_reads | `oltp_point_select` | 57.48ms | 66.74ms | 1.161× | 0.94% | PASS |
| ac_reads | `oltp_range_select` | 21.92ms | 25.00ms | 1.141× | 2.24% | PASS |
| ac_reads | `oltp_sum_range` | 20.73ms | 24.27ms | 1.171× | 1.88% | PASS |
| ac_reads | `oltp_order_range` | 4.11ms | 4.43ms | 1.079× | 1.64% | PASS |
| ac_reads | `oltp_distinct_range` | 5.26ms | 5.50ms | 1.045× | 1.64% | PASS |
| ac_reads | `oltp_index_scan` | 8.02ms | 9.45ms | 1.178× | 2.12% | PASS |
| ac_reads | `select_random_points` | 34.88ms | 38.41ms | 1.101× | 1.52% | PASS |
| ac_reads | `select_random_ranges` | 10.94ms | 12.33ms | 1.127× | 1.31% | PASS |
| ac_reads | `covering_index_scan` | 7.25ms | 7.33ms | 1.012× | 1.47% | PASS |
| ac_reads | `groupby_scan` | 38.15ms | 40.04ms | 1.050× | 1.06% | PASS |
| ac_reads | `index_join` | 10.79ms | 13.79ms | 1.277× | 3.70% | PASS |
| ac_reads | `index_join_scan` | 4.99ms | 6.19ms | 1.242× | 4.81% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.24s | 1.183× | 0.71% | PASS |
| ac_reads | `table_scan` | 1.25s | 1.40s | 1.120× | 2.26% | PASS |
| ac_reads | `oltp_read_only` | 196.28ms | 214.79ms | 1.094× | 1.66% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 25.55ms | 94.96ms | 3.717× | 6.85% | PASS |
| ac_writes | `oltp_insert_ac` | 27.97ms | 121.50ms | 4.344× | 7.37% | PASS |
| ac_writes | `oltp_update_index_ac` | 31.18ms | 133.09ms | 4.268× | 5.33% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.32ms | 108.33ms | 4.278× | 6.94% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.86ms | 113.30ms | 4.218× | 9.11% | PASS |
| ac_writes | `oltp_write_only_ac` | 28.04ms | 112.58ms | 4.015× | 7.37% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.06ms | 102.85ms | 4.274× | 6.47% | PASS |
| ac_writes | `oltp_read_write_ac` | 34.77ms | 117.33ms | 3.375× | 6.65% | PASS |

</details>

## Version-control latency

Wall time: 1m 38s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 48.35ms | 200.00ms | 24.2% | 0.92% | PASS |
| `status_dirty_many_tables` | 49.71ms | 200.00ms | 24.9% | 0.63% | PASS |
| `diff_regular_working_one_table` | 45.11ms | 150.00ms | 30.1% | 0.69% | PASS |
| `diff_regular_working_many_tables` | 53.35ms | 200.00ms | 26.7% | 0.96% | PASS |
| `diff_stat_working_many_tables` | 53.68ms | 200.00ms | 26.8% | 0.87% | PASS |
| `diff_schema_working_many_tables` | 53.90ms | 200.00ms | 27.0% | 0.79% | PASS |
| `branch_list_many_branches` | 14.54ms | 100.00ms | 14.5% | 0.99% | PASS |
| `branch_create_delete` | 18.41ms | 100.00ms | 18.4% | 5.04% | PASS |
| `checkout_branch_clean` | 74.36ms | 200.00ms | 37.2% | 4.97% | PASS |
| `merge_data_no_conflicts` | 30.17ms | 150.00ms | 20.1% | 19.53% | PASS |
| `merge_schema_no_conflicts` | 16.97ms | 100.00ms | 17.0% | 5.52% | PASS |
| `merge_data_conflicts` | 55.26ms | 250.00ms | 22.1% | 0.86% | PASS |
| `merge_data_conflicts_with_resolve` | 55.02ms | 250.00ms | 22.0% | 0.67% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
