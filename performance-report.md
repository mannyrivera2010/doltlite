# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-21 11:42 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260816.277.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/32470716513)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 37s | 8.82s | 11.50s | 1.303× | 1.48% | **PASS** |
| textpk | 69 | 55 | 1h 37m 40s | 10.75s | 11.69s | 1.087× | 2.12% | **PASS** |
| blobpk | 69 | 55 | 1h 31m 3s | 9.62s | 11.80s | 1.226× | 1.80% | **PASS** |
| compositepk | 69 | 55 | 1h 28m 32s | 10.35s | 12.31s | 1.188× | 1.54% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.72ms | 30.64ms | 1.191× | 2.04% | PASS |
| mem_reads | `oltp_range_select` | 11.69ms | 14.02ms | 1.200× | 2.49% | PASS |
| mem_reads | `oltp_sum_range` | 10.35ms | 13.11ms | 1.266× | 1.97% | PASS |
| mem_reads | `oltp_order_range` | 2.64ms | 3.11ms | 1.181× | 1.61% | PASS |
| mem_reads | `oltp_distinct_range` | 3.75ms | 4.28ms | 1.141× | 1.63% | PASS |
| mem_reads | `oltp_index_scan` | 4.13ms | 6.00ms | 1.452× | 2.71% | PASS |
| mem_reads | `select_random_points` | 11.21ms | 12.06ms | 1.076× | 1.70% | PASS |
| mem_reads | `select_random_ranges` | 3.18ms | 4.18ms | 1.315× | 1.62% | PASS |
| mem_reads | `covering_index_scan` | 4.32ms | 4.67ms | 1.081× | 2.59% | PASS |
| mem_reads | `groupby_scan` | 30.20ms | 33.10ms | 1.096× | 0.66% | PASS |
| mem_reads | `index_join` | 6.14ms | 8.71ms | 1.419× | 2.17% | PASS |
| mem_reads | `index_join_scan` | 3.51ms | 4.73ms | 1.348× | 1.57% | PASS |
| mem_reads | `types_table_scan` | 1.08s | 1.35s | 1.252× | 1.44% | PASS |
| mem_reads | `table_scan` | 1.15s | 1.40s | 1.211× | 0.52% | PASS |
| mem_reads | `oltp_read_only` | 99.44ms | 123.33ms | 1.240× | 0.93% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.06ms | 251.45ms | 1.389× | 1.24% | PASS |
| mem_writes | `oltp_insert` | 15.40ms | 28.49ms | 1.849× | 0.93% | PASS |
| mem_writes | `oltp_update_index` | 48.52ms | 103.57ms | 2.135× | 0.97% | PASS |
| mem_writes | `oltp_update_non_index` | 32.58ms | 58.62ms | 1.799× | 0.82% | PASS |
| mem_writes | `oltp_delete_insert` | 43.20ms | 78.14ms | 1.809× | 1.08% | PASS |
| mem_writes | `oltp_write_only` | 20.82ms | 44.22ms | 2.124× | 0.98% | PASS |
| mem_writes | `types_delete_insert` | 24.06ms | 39.99ms | 1.662× | 1.44% | PASS |
| mem_writes | `oltp_read_write` | 64.29ms | 109.03ms | 1.696× | 0.93% | PASS |
| file_reads | `oltp_point_select` | 98.98ms | 55.69ms | 0.563× | 0.78% | PASS |
| file_reads | `oltp_range_select` | 18.56ms | 16.04ms | 0.864× | 1.77% | PASS |
| file_reads | `oltp_sum_range` | 17.69ms | 15.33ms | 0.867× | 1.48% | PASS |
| file_reads | `oltp_order_range` | 3.70ms | 3.57ms | 0.966× | 3.85% | PASS |
| file_reads | `oltp_distinct_range` | 4.84ms | 4.78ms | 0.987× | 4.11% | PASS |
| file_reads | `oltp_index_scan` | 11.89ms | 8.24ms | 0.693× | 2.28% | PASS |
| file_reads | `select_random_points` | 18.32ms | 13.96ms | 0.762× | 1.65% | PASS |
| file_reads | `select_random_ranges` | 10.57ms | 6.61ms | 0.626× | 0.92% | PASS |
| file_reads | `covering_index_scan` | 12.38ms | 6.85ms | 0.554× | 1.34% | PASS |
| file_reads | `groupby_scan` | 30.93ms | 33.42ms | 1.080× | 0.86% | PASS |
| file_reads | `index_join` | 10.46ms | 9.92ms | 0.948× | 1.82% | PASS |
| file_reads | `index_join_scan` | 4.40ms | 4.93ms | 1.121× | 0.83% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.32s | 1.279× | 0.40% | PASS |
| file_reads | `table_scan` | 1.19s | 1.41s | 1.188× | 1.97% | PASS |
| file_reads | `oltp_read_only` | 211.47ms | 162.16ms | 0.767× | 0.97% | PASS |
| file_writes | `oltp_bulk_insert` | 194.60ms | 271.33ms | 1.394× | 1.43% | PASS |
| file_writes | `oltp_insert` | 22.02ms | 35.65ms | 1.618× | 1.11% | PASS |
| file_writes | `oltp_update_index` | 75.81ms | 124.99ms | 1.649× | 0.97% | PASS |
| file_writes | `oltp_update_non_index` | 57.05ms | 80.02ms | 1.403× | 1.08% | PASS |
| file_writes | `oltp_delete_insert` | 67.90ms | 98.85ms | 1.456× | 1.46% | PASS |
| file_writes | `oltp_write_only` | 44.75ms | 63.77ms | 1.425× | 2.02% | PASS |
| file_writes | `types_delete_insert` | 39.90ms | 52.97ms | 1.328× | 1.13% | PASS |
| file_writes | `oltp_read_write` | 88.98ms | 128.24ms | 1.441× | 0.71% | PASS |
| ac_reads | `oltp_point_select` | 47.77ms | 55.41ms | 1.160× | 0.69% | PASS |
| ac_reads | `oltp_range_select` | 13.35ms | 16.09ms | 1.205× | 1.95% | PASS |
| ac_reads | `oltp_sum_range` | 12.43ms | 15.33ms | 1.233× | 1.64% | PASS |
| ac_reads | `oltp_order_range` | 3.10ms | 3.51ms | 1.133× | 3.20% | PASS |
| ac_reads | `oltp_distinct_range` | 4.18ms | 4.55ms | 1.088× | 2.48% | PASS |
| ac_reads | `oltp_index_scan` | 6.59ms | 8.22ms | 1.249× | 1.94% | PASS |
| ac_reads | `select_random_points` | 12.54ms | 13.88ms | 1.106× | 1.72% | PASS |
| ac_reads | `select_random_ranges` | 5.38ms | 6.59ms | 1.224× | 0.94% | PASS |
| ac_reads | `covering_index_scan` | 6.92ms | 6.82ms | 0.985× | 1.36% | PASS |
| ac_reads | `groupby_scan` | 29.93ms | 33.16ms | 1.108× | 0.84% | PASS |
| ac_reads | `index_join` | 7.54ms | 9.99ms | 1.325× | 1.38% | PASS |
| ac_reads | `index_join_scan` | 3.88ms | 5.03ms | 1.296× | 1.81% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.33s | 1.283× | 0.52% | PASS |
| ac_reads | `table_scan` | 1.16s | 1.39s | 1.201× | 0.33% | PASS |
| ac_reads | `oltp_read_only` | 138.41ms | 163.30ms | 1.180× | 0.90% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.42ms | 80.29ms | 3.582× | 4.90% | PASS |
| ac_writes | `oltp_insert_ac` | 24.70ms | 99.92ms | 4.045× | 9.41% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.06ms | 112.04ms | 4.141× | 5.86% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.16ms | 89.02ms | 4.017× | 7.31% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.84ms | 100.66ms | 4.053× | 6.72% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.14ms | 101.96ms | 4.056× | 6.51% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.14ms | 95.62ms | 4.524× | 6.20% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.40ms | 107.20ms | 3.774× | 2.90% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.19ms | 29.60ms | 1.130× | 2.06% | PASS |
| mem_reads | `oltp_range_select` | 12.90ms | 11.28ms | 0.874× | 2.69% | PASS |
| mem_reads | `oltp_sum_range` | 11.19ms | 11.05ms | 0.988× | 2.46% | PASS |
| mem_reads | `oltp_order_range` | 2.73ms | 2.60ms | 0.953× | 1.21% | PASS |
| mem_reads | `oltp_distinct_range` | 3.48ms | 3.34ms | 0.960× | 0.96% | PASS |
| mem_reads | `oltp_index_scan` | 3.93ms | 5.07ms | 1.290× | 2.55% | PASS |
| mem_reads | `select_random_points` | 15.40ms | 16.73ms | 1.086× | 2.62% | PASS |
| mem_reads | `select_random_ranges` | 3.39ms | 4.19ms | 1.235× | 1.58% | PASS |
| mem_reads | `covering_index_scan` | 3.79ms | 3.57ms | 0.942× | 2.87% | PASS |
| mem_reads | `groupby_scan` | 27.15ms | 27.90ms | 1.028× | 1.13% | PASS |
| mem_reads | `index_join` | 5.84ms | 7.51ms | 1.286× | 4.46% | PASS |
| mem_reads | `index_join_scan` | 4.36ms | 5.01ms | 1.150× | 3.86% | PASS |
| mem_reads | `types_table_scan` | 1.12s | 1.05s | 0.936× | 2.05% | PASS |
| mem_reads | `table_scan` | 1.43s | 1.15s | 0.802× | 0.95% | PASS |
| mem_reads | `oltp_read_only` | 106.88ms | 104.85ms | 0.981× | 2.34% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.81ms | 266.38ms | 1.465× | 0.80% | PASS |
| mem_writes | `oltp_insert` | 18.62ms | 30.55ms | 1.640× | 1.49% | PASS |
| mem_writes | `oltp_update_index` | 63.39ms | 110.53ms | 1.744× | 2.76% | PASS |
| mem_writes | `oltp_update_non_index` | 42.62ms | 69.96ms | 1.641× | 2.12% | PASS |
| mem_writes | `oltp_delete_insert` | 44.49ms | 84.60ms | 1.902× | 2.10% | PASS |
| mem_writes | `oltp_write_only` | 25.33ms | 50.29ms | 1.985× | 2.31% | PASS |
| mem_writes | `types_delete_insert` | 27.69ms | 42.06ms | 1.519× | 2.31% | PASS |
| mem_writes | `oltp_read_write` | 72.80ms | 107.25ms | 1.473× | 3.37% | PASS |
| file_reads | `oltp_point_select` | 100.34ms | 53.98ms | 0.538× | 1.08% | PASS |
| file_reads | `oltp_range_select` | 21.02ms | 13.78ms | 0.656× | 2.13% | PASS |
| file_reads | `oltp_sum_range` | 19.63ms | 13.78ms | 0.702× | 1.49% | PASS |
| file_reads | `oltp_order_range` | 3.53ms | 2.83ms | 0.804× | 0.87% | PASS |
| file_reads | `oltp_distinct_range` | 4.32ms | 3.61ms | 0.834× | 0.66% | PASS |
| file_reads | `oltp_index_scan` | 11.82ms | 7.67ms | 0.649× | 0.70% | PASS |
| file_reads | `select_random_points` | 23.61ms | 19.56ms | 0.828× | 1.65% | PASS |
| file_reads | `select_random_ranges` | 11.24ms | 6.79ms | 0.604× | 0.56% | PASS |
| file_reads | `covering_index_scan` | 12.95ms | 6.48ms | 0.501× | 1.21% | PASS |
| file_reads | `groupby_scan` | 28.64ms | 28.34ms | 0.989× | 0.95% | PASS |
| file_reads | `index_join` | 10.82ms | 9.38ms | 0.867× | 1.44% | PASS |
| file_reads | `index_join_scan` | 5.09ms | 5.15ms | 1.011× | 1.19% | PASS |
| file_reads | `types_table_scan` | 996.91ms | 993.34ms | 0.996× | 4.59% | PASS |
| file_reads | `table_scan` | 1.21s | 1.09s | 0.899× | 4.58% | PASS |
| file_reads | `oltp_read_only` | 210.89ms | 138.65ms | 0.657× | 0.91% | PASS |
| file_writes | `oltp_bulk_insert` | 270.76ms | 371.86ms | 1.373× | 12.16% | PASS |
| file_writes | `oltp_insert` | 77.25ms | 67.88ms | 0.879× | 21.21% | PASS |
| file_writes | `oltp_update_index` | 276.27ms | 219.44ms | 0.794× | 19.63% | PASS |
| file_writes | `oltp_update_non_index` | 219.07ms | 142.91ms | 0.652× | 17.83% | PASS |
| file_writes | `oltp_delete_insert` | 258.18ms | 177.08ms | 0.686× | 27.30% | PASS |
| file_writes | `oltp_write_only` | 221.59ms | 134.20ms | 0.606× | 30.40% | PASS |
| file_writes | `types_delete_insert` | 130.00ms | 109.66ms | 0.844× | 37.19% | PASS |
| file_writes | `oltp_read_write` | 193.64ms | 179.58ms | 0.927× | 14.20% | PASS |
| ac_reads | `oltp_point_select` | 50.17ms | 53.31ms | 1.063× | 0.95% | PASS |
| ac_reads | `oltp_range_select` | 15.88ms | 13.71ms | 0.863× | 2.49% | PASS |
| ac_reads | `oltp_sum_range` | 14.20ms | 13.65ms | 0.961× | 1.75% | PASS |
| ac_reads | `oltp_order_range` | 3.10ms | 2.85ms | 0.918× | 1.08% | PASS |
| ac_reads | `oltp_distinct_range` | 3.89ms | 3.62ms | 0.932× | 1.28% | PASS |
| ac_reads | `oltp_index_scan` | 6.98ms | 7.74ms | 1.108× | 1.20% | PASS |
| ac_reads | `select_random_points` | 18.93ms | 19.84ms | 1.048× | 1.66% | PASS |
| ac_reads | `select_random_ranges` | 6.27ms | 6.79ms | 1.084× | 0.87% | PASS |
| ac_reads | `covering_index_scan` | 7.75ms | 6.44ms | 0.831× | 0.78% | PASS |
| ac_reads | `groupby_scan` | 27.71ms | 28.20ms | 1.018× | 0.90% | PASS |
| ac_reads | `index_join` | 8.50ms | 9.50ms | 1.118× | 1.69% | PASS |
| ac_reads | `index_join_scan` | 4.64ms | 5.17ms | 1.115× | 1.16% | PASS |
| ac_reads | `types_table_scan` | 932.75ms | 982.34ms | 1.053× | 3.15% | PASS |
| ac_reads | `table_scan` | 1.25s | 1.09s | 0.877× | 5.44% | PASS |
| ac_reads | `oltp_read_only` | 134.72ms | 137.56ms | 1.021× | 2.02% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 70.26ms | 170.57ms | 2.428× | 57.42% | PASS |
| ac_writes | `oltp_insert_ac` | 73.50ms | 256.60ms | 3.491× | 72.13% | PASS |
| ac_writes | `oltp_update_index_ac` | 51.59ms | 290.09ms | 5.623× | 61.83% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 110.19ms | 228.65ms | 2.075× | 63.22% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 58.07ms | 251.92ms | 4.338× | 64.72% | PASS |
| ac_writes | `oltp_write_only_ac` | 126.78ms | 470.83ms | 3.714× | 65.69% | PASS |
| ac_writes | `types_delete_insert_ac` | 83.75ms | 239.85ms | 2.864× | 55.15% | PASS |
| ac_writes | `oltp_read_write_ac` | 120.12ms | 408.25ms | 3.399× | 55.47% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.16ms | 37.03ms | 1.228× | 1.67% | PASS |
| mem_reads | `oltp_range_select` | 12.61ms | 13.95ms | 1.106× | 1.61% | PASS |
| mem_reads | `oltp_sum_range` | 11.64ms | 13.87ms | 1.191× | 2.48% | PASS |
| mem_reads | `oltp_order_range` | 2.83ms | 3.12ms | 1.102× | 1.32% | PASS |
| mem_reads | `oltp_distinct_range` | 3.94ms | 4.19ms | 1.066× | 1.32% | PASS |
| mem_reads | `oltp_index_scan` | 4.37ms | 6.08ms | 1.392× | 1.34% | PASS |
| mem_reads | `select_random_points` | 16.66ms | 20.50ms | 1.231× | 1.66% | PASS |
| mem_reads | `select_random_ranges` | 3.86ms | 5.13ms | 1.328× | 1.77% | PASS |
| mem_reads | `covering_index_scan` | 4.39ms | 4.38ms | 0.997× | 1.33% | PASS |
| mem_reads | `groupby_scan` | 31.27ms | 33.57ms | 1.073× | 1.05% | PASS |
| mem_reads | `index_join` | 6.73ms | 8.97ms | 1.333× | 2.05% | PASS |
| mem_reads | `index_join_scan` | 4.00ms | 5.19ms | 1.298× | 2.09% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.22s | 1.178× | 0.57% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.36s | 1.151× | 0.58% | PASS |
| mem_reads | `oltp_read_only` | 114.84ms | 133.99ms | 1.167× | 0.81% | PASS |
| mem_writes | `oltp_bulk_insert` | 242.32ms | 355.68ms | 1.468× | 1.01% | PASS |
| mem_writes | `oltp_insert` | 19.97ms | 40.02ms | 2.004× | 0.93% | PASS |
| mem_writes | `oltp_update_index` | 70.73ms | 133.67ms | 1.890× | 2.31% | PASS |
| mem_writes | `oltp_update_non_index` | 49.57ms | 86.31ms | 1.741× | 2.18% | PASS |
| mem_writes | `oltp_delete_insert` | 50.05ms | 105.44ms | 2.107× | 2.92% | PASS |
| mem_writes | `oltp_write_only` | 28.43ms | 63.19ms | 2.223× | 2.30% | PASS |
| mem_writes | `types_delete_insert` | 32.81ms | 55.33ms | 1.686× | 1.51% | PASS |
| mem_writes | `oltp_read_write` | 88.16ms | 143.01ms | 1.622× | 2.98% | PASS |
| file_reads | `oltp_point_select` | 107.39ms | 63.81ms | 0.594× | 1.08% | PASS |
| file_reads | `oltp_range_select` | 21.37ms | 16.81ms | 0.787× | 2.40% | PASS |
| file_reads | `oltp_sum_range` | 20.60ms | 16.84ms | 0.817× | 2.53% | PASS |
| file_reads | `oltp_order_range` | 3.92ms | 3.51ms | 0.894× | 1.80% | PASS |
| file_reads | `oltp_distinct_range` | 5.07ms | 4.60ms | 0.908× | 2.16% | PASS |
| file_reads | `oltp_index_scan` | 12.68ms | 9.27ms | 0.731× | 1.69% | PASS |
| file_reads | `select_random_points` | 27.39ms | 24.55ms | 0.896× | 3.16% | PASS |
| file_reads | `select_random_ranges` | 11.86ms | 7.88ms | 0.664× | 0.96% | PASS |
| file_reads | `covering_index_scan` | 12.46ms | 7.37ms | 0.591× | 2.15% | PASS |
| file_reads | `groupby_scan` | 33.12ms | 34.35ms | 1.037× | 1.01% | PASS |
| file_reads | `index_join` | 11.59ms | 11.24ms | 0.970× | 2.44% | PASS |
| file_reads | `index_join_scan` | 5.24ms | 5.78ms | 1.104× | 2.71% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.23s | 1.162× | 1.25% | PASS |
| file_reads | `table_scan` | 1.22s | 1.37s | 1.119× | 2.69% | PASS |
| file_reads | `oltp_read_only` | 229.32ms | 173.35ms | 0.756× | 1.04% | PASS |
| file_writes | `oltp_bulk_insert` | 261.00ms | 379.79ms | 1.455× | 0.83% | PASS |
| file_writes | `oltp_insert` | 31.43ms | 52.12ms | 1.659× | 1.83% | PASS |
| file_writes | `oltp_update_index` | 102.95ms | 162.67ms | 1.580× | 1.48% | PASS |
| file_writes | `oltp_update_non_index` | 78.02ms | 107.99ms | 1.384× | 1.84% | PASS |
| file_writes | `oltp_delete_insert` | 81.20ms | 130.94ms | 1.613× | 1.59% | PASS |
| file_writes | `oltp_write_only` | 56.09ms | 85.00ms | 1.516× | 2.17% | PASS |
| file_writes | `types_delete_insert` | 51.53ms | 71.95ms | 1.396× | 1.32% | PASS |
| file_writes | `oltp_read_write` | 114.56ms | 160.91ms | 1.405× | 1.79% | PASS |
| ac_reads | `oltp_point_select` | 55.90ms | 63.08ms | 1.128× | 1.18% | PASS |
| ac_reads | `oltp_range_select` | 16.18ms | 16.67ms | 1.030× | 2.03% | PASS |
| ac_reads | `oltp_sum_range` | 15.22ms | 16.67ms | 1.096× | 1.78% | PASS |
| ac_reads | `oltp_order_range` | 3.42ms | 3.50ms | 1.024× | 2.32% | PASS |
| ac_reads | `oltp_distinct_range` | 4.46ms | 4.57ms | 1.025× | 1.90% | PASS |
| ac_reads | `oltp_index_scan` | 7.56ms | 9.11ms | 1.205× | 1.41% | PASS |
| ac_reads | `select_random_points` | 21.49ms | 24.11ms | 1.122× | 2.38% | PASS |
| ac_reads | `select_random_ranges` | 6.89ms | 7.84ms | 1.138× | 1.01% | PASS |
| ac_reads | `covering_index_scan` | 7.84ms | 7.34ms | 0.936× | 1.06% | PASS |
| ac_reads | `groupby_scan` | 32.22ms | 34.29ms | 1.064× | 0.75% | PASS |
| ac_reads | `index_join` | 8.89ms | 11.14ms | 1.253× | 2.46% | PASS |
| ac_reads | `index_join_scan` | 4.70ms | 5.73ms | 1.218× | 1.53% | PASS |
| ac_reads | `types_table_scan` | 1.07s | 1.24s | 1.157× | 2.72% | PASS |
| ac_reads | `table_scan` | 1.40s | 1.39s | 0.998× | 3.79% | PASS |
| ac_reads | `oltp_read_only` | 158.07ms | 174.80ms | 1.106× | 1.35% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.21ms | 79.99ms | 3.602× | 6.32% | PASS |
| ac_writes | `oltp_insert_ac` | 25.95ms | 103.11ms | 3.974× | 5.36% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.65ms | 112.36ms | 4.216× | 3.91% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.47ms | 90.06ms | 4.008× | 5.56% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.46ms | 103.18ms | 4.219× | 6.43% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.55ms | 101.26ms | 4.125× | 5.36% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.51ms | 94.76ms | 4.405× | 5.08% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.11ms | 113.14ms | 3.524× | 4.48% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.93ms | 41.41ms | 1.221× | 1.59% | PASS |
| mem_reads | `oltp_range_select` | 19.55ms | 21.41ms | 1.095× | 1.67% | PASS |
| mem_reads | `oltp_sum_range` | 18.61ms | 21.29ms | 1.144× | 1.27% | PASS |
| mem_reads | `oltp_order_range` | 3.67ms | 3.87ms | 1.056× | 0.96% | PASS |
| mem_reads | `oltp_distinct_range` | 4.78ms | 4.99ms | 1.045× | 1.22% | PASS |
| mem_reads | `oltp_index_scan` | 4.70ms | 6.52ms | 1.385× | 1.99% | PASS |
| mem_reads | `select_random_points` | 29.07ms | 32.57ms | 1.121× | 1.88% | PASS |
| mem_reads | `select_random_ranges` | 7.83ms | 9.16ms | 1.169× | 1.06% | PASS |
| mem_reads | `covering_index_scan` | 4.23ms | 4.36ms | 1.030× | 2.69% | PASS |
| mem_reads | `groupby_scan` | 36.66ms | 38.73ms | 1.056× | 0.99% | PASS |
| mem_reads | `index_join` | 8.31ms | 10.77ms | 1.295× | 1.94% | PASS |
| mem_reads | `index_join_scan` | 4.26ms | 5.55ms | 1.300× | 2.23% | PASS |
| mem_reads | `types_table_scan` | 1.10s | 1.26s | 1.144× | 1.56% | PASS |
| mem_reads | `table_scan` | 1.30s | 1.41s | 1.083× | 1.97% | PASS |
| mem_reads | `oltp_read_only` | 159.47ms | 173.44ms | 1.088× | 1.90% | PASS |
| mem_writes | `oltp_bulk_insert` | 245.08ms | 360.65ms | 1.472× | 1.19% | PASS |
| mem_writes | `oltp_insert` | 19.36ms | 37.09ms | 1.916× | 1.05% | PASS |
| mem_writes | `oltp_update_index` | 71.40ms | 124.12ms | 1.738× | 1.54% | PASS |
| mem_writes | `oltp_update_non_index` | 53.52ms | 86.36ms | 1.614× | 1.27% | PASS |
| mem_writes | `oltp_delete_insert` | 53.15ms | 103.32ms | 1.944× | 1.41% | PASS |
| mem_writes | `oltp_write_only` | 29.16ms | 61.46ms | 2.108× | 1.28% | PASS |
| mem_writes | `types_delete_insert` | 33.43ms | 56.10ms | 1.678× | 2.04% | PASS |
| mem_writes | `oltp_read_write` | 107.57ms | 161.32ms | 1.500× | 1.62% | PASS |
| file_reads | `oltp_point_select` | 109.04ms | 67.96ms | 0.623× | 0.98% | PASS |
| file_reads | `oltp_range_select` | 27.54ms | 24.28ms | 0.882× | 1.38% | PASS |
| file_reads | `oltp_sum_range` | 26.45ms | 24.12ms | 0.912× | 1.56% | PASS |
| file_reads | `oltp_order_range` | 4.56ms | 4.29ms | 0.940× | 1.87% | PASS |
| file_reads | `oltp_distinct_range` | 5.75ms | 5.43ms | 0.945× | 1.40% | PASS |
| file_reads | `oltp_index_scan` | 12.45ms | 9.24ms | 0.742× | 1.74% | PASS |
| file_reads | `select_random_points` | 39.18ms | 37.23ms | 0.950× | 2.04% | PASS |
| file_reads | `select_random_ranges` | 15.71ms | 12.05ms | 0.767× | 1.13% | PASS |
| file_reads | `covering_index_scan` | 12.09ms | 7.22ms | 0.597× | 1.34% | PASS |
| file_reads | `groupby_scan` | 37.93ms | 39.54ms | 1.042× | 0.95% | PASS |
| file_reads | `index_join` | 12.75ms | 13.06ms | 1.024× | 2.07% | PASS |
| file_reads | `index_join_scan` | 5.32ms | 6.01ms | 1.131× | 1.93% | PASS |
| file_reads | `types_table_scan` | 1.07s | 1.24s | 1.160× | 1.29% | PASS |
| file_reads | `table_scan` | 1.37s | 1.42s | 1.035× | 1.96% | PASS |
| file_reads | `oltp_read_only` | 272.94ms | 216.89ms | 0.795× | 1.18% | PASS |
| file_writes | `oltp_bulk_insert` | 260.52ms | 385.28ms | 1.479× | 1.02% | PASS |
| file_writes | `oltp_insert` | 27.00ms | 47.48ms | 1.758× | 1.25% | PASS |
| file_writes | `oltp_update_index` | 101.02ms | 148.98ms | 1.475× | 1.44% | PASS |
| file_writes | `oltp_update_non_index` | 79.64ms | 107.08ms | 1.345× | 1.69% | PASS |
| file_writes | `oltp_delete_insert` | 81.79ms | 127.21ms | 1.555× | 1.96% | PASS |
| file_writes | `oltp_write_only` | 53.35ms | 80.12ms | 1.502× | 2.41% | PASS |
| file_writes | `types_delete_insert` | 51.11ms | 70.50ms | 1.380× | 1.89% | PASS |
| file_writes | `oltp_read_write` | 133.41ms | 179.53ms | 1.346× | 1.60% | PASS |
| ac_reads | `oltp_point_select` | 58.72ms | 67.69ms | 1.153× | 1.09% | PASS |
| ac_reads | `oltp_range_select` | 22.54ms | 24.27ms | 1.077× | 1.33% | PASS |
| ac_reads | `oltp_sum_range` | 21.45ms | 24.16ms | 1.127× | 1.42% | PASS |
| ac_reads | `oltp_order_range` | 4.01ms | 4.25ms | 1.061× | 1.32% | PASS |
| ac_reads | `oltp_distinct_range` | 5.16ms | 5.36ms | 1.039× | 0.98% | PASS |
| ac_reads | `oltp_index_scan` | 7.50ms | 9.30ms | 1.240× | 1.25% | PASS |
| ac_reads | `select_random_points` | 33.19ms | 37.26ms | 1.123× | 1.70% | PASS |
| ac_reads | `select_random_ranges` | 10.66ms | 12.05ms | 1.131× | 1.46% | PASS |
| ac_reads | `covering_index_scan` | 7.25ms | 7.28ms | 1.005× | 1.04% | PASS |
| ac_reads | `groupby_scan` | 37.38ms | 39.64ms | 1.060× | 0.72% | PASS |
| ac_reads | `index_join` | 10.32ms | 13.35ms | 1.293× | 1.51% | PASS |
| ac_reads | `index_join_scan` | 4.88ms | 6.14ms | 1.259× | 2.23% | PASS |
| ac_reads | `types_table_scan` | 1.14s | 1.26s | 1.107× | 0.85% | PASS |
| ac_reads | `table_scan` | 1.42s | 1.43s | 1.010× | 0.75% | PASS |
| ac_reads | `oltp_read_only` | 198.35ms | 217.56ms | 1.097× | 0.96% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.49ms | 84.72ms | 3.606× | 6.62% | PASS |
| ac_writes | `oltp_insert_ac` | 27.22ms | 106.24ms | 3.903× | 9.43% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.78ms | 118.26ms | 4.257× | 5.16% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.71ms | 97.84ms | 4.127× | 5.54% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.28ms | 106.52ms | 4.387× | 6.99% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.54ms | 103.02ms | 3.881× | 5.55% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.01ms | 96.55ms | 4.386× | 7.32% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.30ms | 115.09ms | 3.563× | 4.64% | PASS |

</details>

## Version-control latency

Wall time: 2m 25s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 84.63ms | 200.00ms | 42.3% | 0.84% | PASS |
| `status_dirty_many_tables` | 88.25ms | 200.00ms | 44.1% | 0.76% | PASS |
| `diff_regular_working_one_table` | 80.07ms | 150.00ms | 53.4% | 0.58% | PASS |
| `diff_regular_working_many_tables` | 94.00ms | 200.00ms | 47.0% | 0.62% | PASS |
| `diff_stat_working_many_tables` | 93.82ms | 200.00ms | 46.9% | 0.65% | PASS |
| `diff_schema_working_many_tables` | 94.77ms | 200.00ms | 47.4% | 0.66% | PASS |
| `branch_list_many_branches` | 24.32ms | 100.00ms | 24.3% | 1.81% | PASS |
| `branch_create_delete` | 27.29ms | 100.00ms | 27.3% | 1.81% | PASS |
| `checkout_branch_clean` | 57.68ms | 200.00ms | 28.8% | 1.17% | PASS |
| `merge_data_no_conflicts` | 31.24ms | 150.00ms | 20.8% | 2.01% | PASS |
| `merge_schema_no_conflicts` | 24.48ms | 100.00ms | 24.5% | 2.14% | PASS |
| `merge_data_conflicts` | 128.59ms | 250.00ms | 51.4% | 0.32% | PASS |
| `merge_data_conflicts_with_resolve` | 128.88ms | 250.00ms | 51.6% | 0.36% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
