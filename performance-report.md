# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-07 12:03 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260720.247.2
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/31170141820)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 15s | 9.37s | 11.07s | 1.182× | 1.05% | **PASS** |
| textpk | 69 | 55 | 1h 32m 16s | 9.71s | 11.82s | 1.217× | 1.60% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 36s | 9.40s | 11.74s | 1.249× | 1.70% | **PASS** |
| compositepk | 69 | 55 | 1h 26m 36s | 9.40s | 12.42s | 1.321× | 1.24% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.47ms | 28.00ms | 1.144× | 1.60% | PASS |
| mem_reads | `oltp_range_select` | 10.72ms | 12.10ms | 1.129× | 1.44% | PASS |
| mem_reads | `oltp_sum_range` | 9.78ms | 11.48ms | 1.174× | 1.35% | PASS |
| mem_reads | `oltp_order_range` | 2.69ms | 2.91ms | 1.081× | 1.25% | PASS |
| mem_reads | `oltp_distinct_range` | 3.77ms | 3.90ms | 1.034× | 1.00% | PASS |
| mem_reads | `oltp_index_scan` | 3.98ms | 4.97ms | 1.251× | 0.89% | PASS |
| mem_reads | `select_random_points` | 10.66ms | 11.37ms | 1.066× | 1.91% | PASS |
| mem_reads | `select_random_ranges` | 3.13ms | 4.03ms | 1.288× | 1.08% | PASS |
| mem_reads | `covering_index_scan` | 4.34ms | 4.08ms | 0.940× | 1.50% | PASS |
| mem_reads | `groupby_scan` | 32.12ms | 34.37ms | 1.070× | 0.59% | PASS |
| mem_reads | `index_join` | 5.96ms | 7.85ms | 1.319× | 1.15% | PASS |
| mem_reads | `index_join_scan` | 3.50ms | 4.76ms | 1.361× | 1.57% | PASS |
| mem_reads | `types_table_scan` | 1.11s | 1.27s | 1.138× | 0.53% | PASS |
| mem_reads | `table_scan` | 1.27s | 1.37s | 1.079× | 0.63% | PASS |
| mem_reads | `oltp_read_only` | 103.24ms | 115.70ms | 1.121× | 0.79% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.95ms | 243.70ms | 1.347× | 1.15% | PASS |
| mem_writes | `oltp_insert` | 15.83ms | 28.29ms | 1.787× | 0.96% | PASS |
| mem_writes | `oltp_update_index` | 51.34ms | 105.85ms | 2.062× | 0.84% | PASS |
| mem_writes | `oltp_update_non_index` | 35.28ms | 58.70ms | 1.664× | 1.09% | PASS |
| mem_writes | `oltp_delete_insert` | 44.94ms | 78.49ms | 1.747× | 0.73% | PASS |
| mem_writes | `oltp_write_only` | 22.09ms | 45.26ms | 2.049× | 1.05% | PASS |
| mem_writes | `types_delete_insert` | 24.91ms | 40.09ms | 1.609× | 0.91% | PASS |
| mem_writes | `oltp_read_write` | 65.35ms | 105.43ms | 1.613× | 0.70% | PASS |
| file_reads | `oltp_point_select` | 119.64ms | 59.35ms | 0.496× | 0.82% | PASS |
| file_reads | `oltp_range_select` | 19.74ms | 15.26ms | 0.773× | 1.85% | PASS |
| file_reads | `oltp_sum_range` | 19.89ms | 14.86ms | 0.747× | 2.32% | PASS |
| file_reads | `oltp_order_range` | 3.60ms | 3.28ms | 0.911× | 1.55% | PASS |
| file_reads | `oltp_distinct_range` | 4.65ms | 4.28ms | 0.920× | 0.97% | PASS |
| file_reads | `oltp_index_scan` | 13.40ms | 8.56ms | 0.639× | 0.97% | PASS |
| file_reads | `select_random_points` | 19.73ms | 14.74ms | 0.747× | 1.47% | PASS |
| file_reads | `select_random_ranges` | 12.42ms | 7.22ms | 0.581× | 0.85% | PASS |
| file_reads | `covering_index_scan` | 13.95ms | 7.56ms | 0.542× | 0.99% | PASS |
| file_reads | `groupby_scan` | 32.96ms | 34.81ms | 1.056× | 0.76% | PASS |
| file_reads | `index_join` | 11.14ms | 10.27ms | 0.922× | 1.28% | PASS |
| file_reads | `index_join_scan` | 4.64ms | 5.28ms | 1.138× | 1.31% | PASS |
| file_reads | `types_table_scan` | 1.10s | 1.26s | 1.145× | 0.51% | PASS |
| file_reads | `table_scan` | 1.27s | 1.37s | 1.085× | 0.64% | PASS |
| file_reads | `oltp_read_only` | 249.38ms | 164.48ms | 0.660× | 0.91% | PASS |
| file_writes | `oltp_bulk_insert` | 195.76ms | 262.14ms | 1.339× | 0.86% | PASS |
| file_writes | `oltp_insert` | 22.26ms | 36.26ms | 1.629× | 1.20% | PASS |
| file_writes | `oltp_update_index` | 79.28ms | 131.11ms | 1.654× | 0.92% | PASS |
| file_writes | `oltp_update_non_index` | 59.64ms | 82.75ms | 1.388× | 1.57% | PASS |
| file_writes | `oltp_delete_insert` | 67.89ms | 99.81ms | 1.470× | 1.22% | PASS |
| file_writes | `oltp_write_only` | 43.95ms | 66.58ms | 1.515× | 1.44% | PASS |
| file_writes | `types_delete_insert` | 40.42ms | 54.05ms | 1.337× | 0.77% | PASS |
| file_writes | `oltp_read_write` | 88.78ms | 126.83ms | 1.429× | 1.28% | PASS |
| ac_reads | `oltp_point_select` | 55.93ms | 59.43ms | 1.063× | 0.72% | PASS |
| ac_reads | `oltp_range_select` | 14.21ms | 15.32ms | 1.078× | 0.89% | PASS |
| ac_reads | `oltp_sum_range` | 13.18ms | 14.79ms | 1.122× | 0.96% | PASS |
| ac_reads | `oltp_order_range` | 3.17ms | 3.28ms | 1.036× | 1.02% | PASS |
| ac_reads | `oltp_distinct_range` | 4.19ms | 4.29ms | 1.023× | 0.83% | PASS |
| ac_reads | `oltp_index_scan` | 7.42ms | 8.57ms | 1.155× | 1.29% | PASS |
| ac_reads | `select_random_points` | 14.18ms | 14.73ms | 1.039× | 1.13% | PASS |
| ac_reads | `select_random_ranges` | 6.35ms | 7.22ms | 1.137× | 0.82% | PASS |
| ac_reads | `covering_index_scan` | 7.80ms | 7.62ms | 0.977× | 1.32% | PASS |
| ac_reads | `groupby_scan` | 32.60ms | 34.86ms | 1.069× | 0.65% | PASS |
| ac_reads | `index_join` | 7.97ms | 10.14ms | 1.272× | 1.13% | PASS |
| ac_reads | `index_join_scan` | 4.06ms | 5.20ms | 1.281× | 1.02% | PASS |
| ac_reads | `types_table_scan` | 1.10s | 1.26s | 1.144× | 0.40% | PASS |
| ac_reads | `table_scan` | 1.26s | 1.37s | 1.088× | 0.38% | PASS |
| ac_reads | `oltp_read_only` | 149.08ms | 160.48ms | 1.076× | 0.74% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.70ms | 63.84ms | 4.067× | 3.42% | PASS |
| ac_writes | `oltp_insert_ac` | 17.90ms | 81.40ms | 4.547× | 3.30% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.47ms | 96.47ms | 4.713× | 3.90% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.39ms | 73.42ms | 4.478× | 6.05% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.82ms | 87.15ms | 4.890× | 2.80% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.90ms | 85.21ms | 4.509× | 4.41% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.09ms | 75.43ms | 4.687× | 5.33% | PASS |
| ac_writes | `oltp_read_write_ac` | 23.57ms | 94.02ms | 3.989× | 3.60% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.19ms | 38.01ms | 1.259× | 1.58% | PASS |
| mem_reads | `oltp_range_select` | 13.07ms | 14.40ms | 1.102× | 2.63% | PASS |
| mem_reads | `oltp_sum_range` | 11.56ms | 14.29ms | 1.236× | 1.02% | PASS |
| mem_reads | `oltp_order_range` | 2.90ms | 3.19ms | 1.100× | 2.88% | PASS |
| mem_reads | `oltp_distinct_range` | 4.06ms | 4.30ms | 1.059× | 1.23% | PASS |
| mem_reads | `oltp_index_scan` | 4.70ms | 6.63ms | 1.412× | 1.59% | PASS |
| mem_reads | `select_random_points` | 18.29ms | 21.63ms | 1.183× | 1.66% | PASS |
| mem_reads | `select_random_ranges` | 4.09ms | 5.32ms | 1.300× | 1.59% | PASS |
| mem_reads | `covering_index_scan` | 5.07ms | 4.85ms | 0.957× | 3.28% | PASS |
| mem_reads | `groupby_scan` | 32.52ms | 35.08ms | 1.079× | 0.69% | PASS |
| mem_reads | `index_join` | 6.93ms | 9.51ms | 1.371× | 1.50% | PASS |
| mem_reads | `index_join_scan` | 4.57ms | 5.40ms | 1.180× | 1.77% | PASS |
| mem_reads | `types_table_scan` | 1.12s | 1.24s | 1.114× | 1.60% | PASS |
| mem_reads | `table_scan` | 1.24s | 1.37s | 1.111× | 2.27% | PASS |
| mem_reads | `oltp_read_only` | 114.34ms | 137.28ms | 1.201× | 0.90% | PASS |
| mem_writes | `oltp_bulk_insert` | 234.70ms | 356.30ms | 1.518× | 0.60% | PASS |
| mem_writes | `oltp_insert` | 21.54ms | 39.68ms | 1.842× | 0.64% | PASS |
| mem_writes | `oltp_update_index` | 69.10ms | 132.24ms | 1.914× | 0.66% | PASS |
| mem_writes | `oltp_update_non_index` | 47.51ms | 86.57ms | 1.822× | 0.99% | PASS |
| mem_writes | `oltp_delete_insert` | 49.90ms | 103.30ms | 2.070× | 0.88% | PASS |
| mem_writes | `oltp_write_only` | 28.03ms | 61.16ms | 2.182× | 0.64% | PASS |
| mem_writes | `types_delete_insert` | 32.06ms | 54.56ms | 1.702× | 0.72% | PASS |
| mem_writes | `oltp_read_write` | 82.05ms | 138.82ms | 1.692× | 0.86% | PASS |
| file_reads | `oltp_point_select` | 103.10ms | 64.44ms | 0.625× | 1.05% | PASS |
| file_reads | `oltp_range_select` | 20.09ms | 17.21ms | 0.856× | 1.79% | PASS |
| file_reads | `oltp_sum_range` | 19.36ms | 17.33ms | 0.895× | 1.59% | PASS |
| file_reads | `oltp_order_range` | 3.71ms | 3.56ms | 0.960× | 2.18% | PASS |
| file_reads | `oltp_distinct_range` | 4.76ms | 4.63ms | 0.972× | 1.41% | PASS |
| file_reads | `oltp_index_scan` | 11.89ms | 9.36ms | 0.787× | 1.44% | PASS |
| file_reads | `select_random_points` | 25.72ms | 25.06ms | 0.974× | 1.72% | PASS |
| file_reads | `select_random_ranges` | 11.29ms | 8.05ms | 0.713× | 1.24% | PASS |
| file_reads | `covering_index_scan` | 12.31ms | 7.52ms | 0.611× | 1.42% | PASS |
| file_reads | `groupby_scan` | 32.36ms | 35.40ms | 1.094× | 1.43% | PASS |
| file_reads | `index_join` | 11.09ms | 11.46ms | 1.033× | 1.46% | PASS |
| file_reads | `index_join_scan` | 5.59ms | 5.93ms | 1.060× | 2.01% | PASS |
| file_reads | `types_table_scan` | 1.07s | 1.22s | 1.143× | 0.48% | PASS |
| file_reads | `table_scan` | 1.22s | 1.36s | 1.114× | 0.57% | PASS |
| file_reads | `oltp_read_only` | 226.61ms | 178.04ms | 0.786× | 1.23% | PASS |
| file_writes | `oltp_bulk_insert` | 255.99ms | 388.86ms | 1.519× | 0.93% | PASS |
| file_writes | `oltp_insert` | 51.41ms | 53.13ms | 1.033× | 22.95% | PASS |
| file_writes | `oltp_update_index` | 111.30ms | 169.83ms | 1.526× | 1.65% | PASS |
| file_writes | `oltp_update_non_index` | 98.26ms | 113.18ms | 1.152× | 8.97% | PASS |
| file_writes | `oltp_delete_insert` | 88.03ms | 133.32ms | 1.514× | 1.49% | PASS |
| file_writes | `oltp_write_only` | 83.84ms | 85.40ms | 1.019× | 9.03% | PASS |
| file_writes | `types_delete_insert` | 54.48ms | 74.41ms | 1.366× | 1.63% | PASS |
| file_writes | `oltp_read_write` | 133.19ms | 163.62ms | 1.228× | 7.71% | PASS |
| ac_reads | `oltp_point_select` | 54.53ms | 64.95ms | 1.191× | 1.37% | PASS |
| ac_reads | `oltp_range_select` | 16.31ms | 17.23ms | 1.056× | 1.80% | PASS |
| ac_reads | `oltp_sum_range` | 15.14ms | 17.29ms | 1.142× | 1.60% | PASS |
| ac_reads | `oltp_order_range` | 3.30ms | 3.55ms | 1.078× | 2.66% | PASS |
| ac_reads | `oltp_distinct_range` | 4.38ms | 4.62ms | 1.054× | 1.10% | PASS |
| ac_reads | `oltp_index_scan` | 7.34ms | 9.45ms | 1.287× | 1.78% | PASS |
| ac_reads | `select_random_points` | 20.99ms | 24.95ms | 1.189× | 1.69% | PASS |
| ac_reads | `select_random_ranges` | 6.71ms | 8.02ms | 1.195× | 1.98% | PASS |
| ac_reads | `covering_index_scan` | 8.02ms | 7.58ms | 0.946× | 2.45% | PASS |
| ac_reads | `groupby_scan` | 32.31ms | 35.23ms | 1.090× | 0.78% | PASS |
| ac_reads | `index_join` | 9.05ms | 11.49ms | 1.269× | 3.43% | PASS |
| ac_reads | `index_join_scan` | 5.24ms | 6.01ms | 1.147× | 3.16% | PASS |
| ac_reads | `types_table_scan` | 1.07s | 1.22s | 1.143× | 0.52% | PASS |
| ac_reads | `table_scan` | 1.28s | 1.38s | 1.083× | 3.94% | PASS |
| ac_reads | `oltp_read_only` | 153.43ms | 176.83ms | 1.153× | 0.83% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.62ms | 79.14ms | 3.661× | 4.35% | PASS |
| ac_writes | `oltp_insert_ac` | 24.97ms | 93.58ms | 3.748× | 3.73% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.85ms | 110.01ms | 4.097× | 3.21% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.07ms | 90.49ms | 3.923× | 5.02% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.63ms | 103.02ms | 4.020× | 6.11% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.60ms | 105.06ms | 3.950× | 7.99% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.83ms | 93.84ms | 4.298× | 5.05% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.82ms | 110.78ms | 3.481× | 4.59% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.29ms | 37.36ms | 1.233× | 1.70% | PASS |
| mem_reads | `oltp_range_select` | 13.53ms | 14.43ms | 1.066× | 2.60% | PASS |
| mem_reads | `oltp_sum_range` | 12.36ms | 14.22ms | 1.151× | 1.99% | PASS |
| mem_reads | `oltp_order_range` | 2.98ms | 3.18ms | 1.068× | 1.75% | PASS |
| mem_reads | `oltp_distinct_range` | 4.04ms | 4.26ms | 1.054× | 0.93% | PASS |
| mem_reads | `oltp_index_scan` | 4.61ms | 6.48ms | 1.406× | 1.21% | PASS |
| mem_reads | `select_random_points` | 18.05ms | 21.20ms | 1.174× | 1.91% | PASS |
| mem_reads | `select_random_ranges` | 4.22ms | 5.27ms | 1.249× | 1.32% | PASS |
| mem_reads | `covering_index_scan` | 4.54ms | 4.79ms | 1.055× | 2.22% | PASS |
| mem_reads | `groupby_scan` | 32.30ms | 34.39ms | 1.065× | 1.11% | PASS |
| mem_reads | `index_join` | 6.94ms | 9.89ms | 1.426× | 1.97% | PASS |
| mem_reads | `index_join_scan` | 4.24ms | 5.55ms | 1.311× | 1.70% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.23s | 1.172× | 1.26% | PASS |
| mem_reads | `table_scan` | 1.30s | 1.38s | 1.060× | 5.45% | PASS |
| mem_reads | `oltp_read_only` | 115.37ms | 134.72ms | 1.168× | 1.33% | PASS |
| mem_writes | `oltp_bulk_insert` | 242.37ms | 353.39ms | 1.458× | 1.06% | PASS |
| mem_writes | `oltp_insert` | 20.07ms | 39.42ms | 1.964× | 0.60% | PASS |
| mem_writes | `oltp_update_index` | 69.86ms | 133.93ms | 1.917× | 1.79% | PASS |
| mem_writes | `oltp_update_non_index` | 49.89ms | 85.87ms | 1.721× | 1.55% | PASS |
| mem_writes | `oltp_delete_insert` | 47.68ms | 100.57ms | 2.109× | 1.06% | PASS |
| mem_writes | `oltp_write_only` | 27.42ms | 60.81ms | 2.218× | 1.31% | PASS |
| mem_writes | `types_delete_insert` | 31.27ms | 52.82ms | 1.689× | 0.60% | PASS |
| mem_writes | `oltp_read_write` | 90.10ms | 144.40ms | 1.603× | 2.71% | PASS |
| file_reads | `oltp_point_select` | 106.92ms | 64.69ms | 0.605× | 1.42% | PASS |
| file_reads | `oltp_range_select` | 21.75ms | 17.40ms | 0.800× | 3.31% | PASS |
| file_reads | `oltp_sum_range` | 20.72ms | 17.55ms | 0.847× | 1.59% | PASS |
| file_reads | `oltp_order_range` | 4.43ms | 4.15ms | 0.936× | 4.09% | PASS |
| file_reads | `oltp_distinct_range` | 5.43ms | 5.27ms | 0.971× | 3.24% | PASS |
| file_reads | `oltp_index_scan` | 12.22ms | 9.44ms | 0.772× | 2.11% | PASS |
| file_reads | `select_random_points` | 27.31ms | 24.80ms | 0.908× | 2.70% | PASS |
| file_reads | `select_random_ranges` | 11.67ms | 7.94ms | 0.680× | 2.41% | PASS |
| file_reads | `covering_index_scan` | 12.89ms | 7.46ms | 0.579× | 2.62% | PASS |
| file_reads | `groupby_scan` | 32.44ms | 34.71ms | 1.070× | 1.25% | PASS |
| file_reads | `index_join` | 11.04ms | 11.41ms | 1.033× | 2.54% | PASS |
| file_reads | `index_join_scan` | 5.15ms | 6.01ms | 1.168× | 2.14% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.22s | 1.179× | 0.36% | PASS |
| file_reads | `table_scan` | 1.17s | 1.36s | 1.157× | 0.49% | PASS |
| file_reads | `oltp_read_only` | 231.00ms | 174.72ms | 0.756× | 1.08% | PASS |
| file_writes | `oltp_bulk_insert` | 261.50ms | 377.93ms | 1.445× | 0.93% | PASS |
| file_writes | `oltp_insert` | 31.48ms | 51.44ms | 1.634× | 1.65% | PASS |
| file_writes | `oltp_update_index` | 101.40ms | 162.30ms | 1.600× | 1.15% | PASS |
| file_writes | `oltp_update_non_index` | 77.23ms | 107.36ms | 1.390× | 1.33% | PASS |
| file_writes | `oltp_delete_insert` | 79.91ms | 128.79ms | 1.612× | 1.72% | PASS |
| file_writes | `oltp_write_only` | 55.28ms | 83.82ms | 1.516× | 2.41% | PASS |
| file_writes | `types_delete_insert` | 51.11ms | 71.23ms | 1.394× | 2.46% | PASS |
| file_writes | `oltp_read_write` | 112.81ms | 160.25ms | 1.420× | 1.54% | PASS |
| ac_reads | `oltp_point_select` | 54.84ms | 63.20ms | 1.152× | 0.97% | PASS |
| ac_reads | `oltp_range_select` | 16.13ms | 17.08ms | 1.059× | 1.33% | PASS |
| ac_reads | `oltp_sum_range` | 14.93ms | 16.92ms | 1.133× | 1.62% | PASS |
| ac_reads | `oltp_order_range` | 3.48ms | 3.66ms | 1.052× | 2.03% | PASS |
| ac_reads | `oltp_distinct_range` | 4.62ms | 4.86ms | 1.053× | 2.59% | PASS |
| ac_reads | `oltp_index_scan` | 7.50ms | 9.29ms | 1.238× | 1.97% | PASS |
| ac_reads | `select_random_points` | 21.43ms | 24.97ms | 1.165× | 1.47% | PASS |
| ac_reads | `select_random_ranges` | 6.99ms | 8.10ms | 1.159× | 1.41% | PASS |
| ac_reads | `covering_index_scan` | 7.71ms | 7.53ms | 0.977× | 2.80% | PASS |
| ac_reads | `groupby_scan` | 32.05ms | 34.83ms | 1.086× | 1.15% | PASS |
| ac_reads | `index_join` | 8.88ms | 11.51ms | 1.296× | 1.78% | PASS |
| ac_reads | `index_join_scan` | 4.76ms | 6.03ms | 1.267× | 1.65% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.22s | 1.181× | 0.62% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.36s | 1.158× | 0.54% | PASS |
| ac_reads | `oltp_read_only` | 151.84ms | 173.78ms | 1.145× | 0.60% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.17ms | 80.39ms | 3.626× | 3.87% | PASS |
| ac_writes | `oltp_insert_ac` | 24.76ms | 101.87ms | 4.115× | 4.08% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.95ms | 114.95ms | 4.264× | 4.08% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.57ms | 93.81ms | 4.156× | 3.07% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.91ms | 104.06ms | 4.353× | 4.30% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.40ms | 103.40ms | 4.070× | 5.59% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.76ms | 93.44ms | 4.295× | 5.80% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.62ms | 110.79ms | 3.618× | 3.54% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.55ms | 41.29ms | 1.309× | 1.39% | PASS |
| mem_reads | `oltp_range_select` | 17.58ms | 21.12ms | 1.201× | 1.36% | PASS |
| mem_reads | `oltp_sum_range` | 17.05ms | 21.02ms | 1.233× | 1.07% | PASS |
| mem_reads | `oltp_order_range` | 3.33ms | 3.87ms | 1.163× | 0.93% | PASS |
| mem_reads | `oltp_distinct_range` | 4.38ms | 4.96ms | 1.132× | 0.93% | PASS |
| mem_reads | `oltp_index_scan` | 4.28ms | 6.13ms | 1.433× | 1.24% | PASS |
| mem_reads | `select_random_points` | 26.20ms | 32.05ms | 1.223× | 1.51% | PASS |
| mem_reads | `select_random_ranges` | 7.17ms | 9.15ms | 1.275× | 1.28% | PASS |
| mem_reads | `covering_index_scan` | 4.22ms | 4.24ms | 1.005× | 1.38% | PASS |
| mem_reads | `groupby_scan` | 35.75ms | 38.99ms | 1.091× | 0.78% | PASS |
| mem_reads | `index_join` | 8.00ms | 10.39ms | 1.299× | 1.20% | PASS |
| mem_reads | `index_join_scan` | 3.72ms | 5.38ms | 1.445× | 1.69% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.31s | 1.257× | 0.53% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.46s | 1.251× | 0.38% | PASS |
| mem_reads | `oltp_read_only` | 145.27ms | 172.19ms | 1.185× | 0.64% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.44ms | 354.08ms | 1.420× | 0.65% | PASS |
| mem_writes | `oltp_insert` | 19.21ms | 36.22ms | 1.886× | 0.59% | PASS |
| mem_writes | `oltp_update_index` | 65.61ms | 115.01ms | 1.753× | 0.83% | PASS |
| mem_writes | `oltp_update_non_index` | 48.56ms | 82.91ms | 1.707× | 1.19% | PASS |
| mem_writes | `oltp_delete_insert` | 48.15ms | 94.47ms | 1.962× | 0.88% | PASS |
| mem_writes | `oltp_write_only` | 26.05ms | 56.80ms | 2.180× | 0.59% | PASS |
| mem_writes | `types_delete_insert` | 31.71ms | 53.77ms | 1.695× | 1.59% | PASS |
| mem_writes | `oltp_read_write` | 98.01ms | 155.80ms | 1.590× | 0.56% | PASS |
| file_reads | `oltp_point_select` | 105.99ms | 68.77ms | 0.649× | 1.05% | PASS |
| file_reads | `oltp_range_select` | 25.47ms | 24.77ms | 0.972× | 1.14% | PASS |
| file_reads | `oltp_sum_range` | 24.93ms | 24.46ms | 0.981× | 1.29% | PASS |
| file_reads | `oltp_order_range` | 4.19ms | 4.32ms | 1.030× | 0.93% | PASS |
| file_reads | `oltp_distinct_range` | 5.32ms | 5.41ms | 1.016× | 1.48% | PASS |
| file_reads | `oltp_index_scan` | 11.84ms | 9.29ms | 0.784× | 1.47% | PASS |
| file_reads | `select_random_points` | 35.77ms | 36.55ms | 1.022× | 0.88% | PASS |
| file_reads | `select_random_ranges` | 15.04ms | 12.43ms | 0.826× | 1.40% | PASS |
| file_reads | `covering_index_scan` | 11.68ms | 7.01ms | 0.600× | 1.10% | PASS |
| file_reads | `groupby_scan` | 36.63ms | 39.87ms | 1.088× | 0.81% | PASS |
| file_reads | `index_join` | 12.27ms | 12.90ms | 1.051× | 1.34% | PASS |
| file_reads | `index_join_scan` | 4.93ms | 6.09ms | 1.234× | 1.81% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.30s | 1.257× | 0.51% | PASS |
| file_reads | `table_scan` | 1.17s | 1.46s | 1.250× | 0.65% | PASS |
| file_reads | `oltp_read_only` | 255.03ms | 214.33ms | 0.840× | 0.78% | PASS |
| file_writes | `oltp_bulk_insert` | 264.00ms | 378.65ms | 1.434× | 1.37% | PASS |
| file_writes | `oltp_insert` | 26.02ms | 46.14ms | 1.773× | 1.33% | PASS |
| file_writes | `oltp_update_index` | 94.63ms | 141.84ms | 1.499× | 1.28% | PASS |
| file_writes | `oltp_update_non_index` | 73.92ms | 104.23ms | 1.410× | 1.16% | PASS |
| file_writes | `oltp_delete_insert` | 74.77ms | 118.52ms | 1.585× | 1.26% | PASS |
| file_writes | `oltp_write_only` | 49.57ms | 76.71ms | 1.547× | 1.53% | PASS |
| file_writes | `types_delete_insert` | 48.07ms | 67.64ms | 1.407× | 1.02% | PASS |
| file_writes | `oltp_read_write` | 123.50ms | 176.18ms | 1.427× | 0.98% | PASS |
| ac_reads | `oltp_point_select` | 57.29ms | 68.52ms | 1.196× | 1.19% | PASS |
| ac_reads | `oltp_range_select` | 21.02ms | 24.67ms | 1.173× | 1.94% | PASS |
| ac_reads | `oltp_sum_range` | 20.16ms | 24.47ms | 1.214× | 1.08% | PASS |
| ac_reads | `oltp_order_range` | 3.75ms | 4.30ms | 1.148× | 1.52% | PASS |
| ac_reads | `oltp_distinct_range` | 4.83ms | 5.38ms | 1.114× | 1.50% | PASS |
| ac_reads | `oltp_index_scan` | 7.05ms | 9.27ms | 1.315× | 1.81% | PASS |
| ac_reads | `select_random_points` | 30.27ms | 36.73ms | 1.213× | 1.25% | PASS |
| ac_reads | `select_random_ranges` | 10.10ms | 12.40ms | 1.227× | 1.85% | PASS |
| ac_reads | `covering_index_scan` | 6.82ms | 7.01ms | 1.028× | 1.65% | PASS |
| ac_reads | `groupby_scan` | 36.14ms | 39.71ms | 1.099× | 0.73% | PASS |
| ac_reads | `index_join` | 9.73ms | 12.95ms | 1.331× | 0.97% | PASS |
| ac_reads | `index_join_scan` | 4.53ms | 5.99ms | 1.321× | 2.99% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.30s | 1.260× | 0.61% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.46s | 1.248× | 0.31% | PASS |
| ac_reads | `oltp_read_only` | 184.22ms | 214.66ms | 1.165× | 0.87% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.87ms | 76.94ms | 3.518× | 5.16% | PASS |
| ac_writes | `oltp_insert_ac` | 24.21ms | 96.86ms | 4.000× | 3.42% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.80ms | 109.03ms | 4.226× | 3.52% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.64ms | 88.12ms | 3.727× | 4.43% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.00ms | 99.98ms | 4.167× | 4.73% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.07ms | 98.28ms | 4.082× | 4.10% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.66ms | 88.89ms | 4.105× | 6.23% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.30ms | 108.93ms | 3.480× | 3.42% | PASS |

</details>

## Version-control latency

Wall time: 2m 28s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 93.57ms | 200.00ms | 46.8% | 0.23% | PASS |
| `status_dirty_many_tables` | 97.08ms | 200.00ms | 48.5% | 0.23% | PASS |
| `diff_regular_working_one_table` | 89.29ms | 150.00ms | 59.5% | 0.21% | PASS |
| `diff_regular_working_many_tables` | 102.56ms | 200.00ms | 51.3% | 0.29% | PASS |
| `diff_stat_working_many_tables` | 102.65ms | 200.00ms | 51.3% | 0.21% | PASS |
| `diff_schema_working_many_tables` | 103.28ms | 200.00ms | 51.6% | 0.16% | PASS |
| `branch_list_many_branches` | 24.05ms | 100.00ms | 24.1% | 0.69% | PASS |
| `branch_create_delete` | 25.86ms | 100.00ms | 25.9% | 0.78% | PASS |
| `checkout_branch_clean` | 59.56ms | 200.00ms | 29.8% | 0.92% | PASS |
| `merge_data_no_conflicts` | 30.56ms | 150.00ms | 20.4% | 0.86% | PASS |
| `merge_schema_no_conflicts` | 22.62ms | 100.00ms | 22.6% | 0.62% | PASS |
| `merge_data_conflicts` | 129.11ms | 250.00ms | 51.6% | 0.14% | PASS |
| `merge_data_conflicts_with_resolve` | 129.10ms | 250.00ms | 51.6% | 0.22% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
