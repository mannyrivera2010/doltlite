# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-30 17:50 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261026.354.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/41087352338)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 7m 58s | 7.82s | 9.18s | 1.173× | 1.63% | **PASS** |
| textpk | 69 | 55 | 1h 38m 4s | 10.96s | 12.22s | 1.115× | 1.67% | **PASS** |
| blobpk | 69 | 55 | 1h 35m 3s | 10.23s | 11.99s | 1.172× | 1.94% | **PASS** |
| compositepk | 69 | 55 | 1h 26m 34s | 10.43s | 10.91s | 1.046× | 7.93% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.14ms | 21.97ms | 1.091× | 1.63% | PASS |
| mem_reads | `oltp_range_select` | 9.10ms | 9.54ms | 1.049× | 3.36% | PASS |
| mem_reads | `oltp_sum_range` | 8.42ms | 9.09ms | 1.080× | 3.02% | PASS |
| mem_reads | `oltp_order_range` | 2.20ms | 2.28ms | 1.033× | 0.94% | PASS |
| mem_reads | `oltp_distinct_range` | 3.02ms | 3.09ms | 1.021× | 0.89% | PASS |
| mem_reads | `oltp_index_scan` | 3.25ms | 3.98ms | 1.225× | 2.86% | PASS |
| mem_reads | `select_random_points` | 9.51ms | 9.36ms | 0.985× | 4.24% | PASS |
| mem_reads | `select_random_ranges` | 2.61ms | 3.23ms | 1.236× | 1.56% | PASS |
| mem_reads | `covering_index_scan` | 3.40ms | 3.35ms | 0.987× | 3.50% | PASS |
| mem_reads | `groupby_scan` | 25.30ms | 27.27ms | 1.078× | 0.65% | PASS |
| mem_reads | `index_join` | 4.74ms | 6.18ms | 1.305× | 2.00% | PASS |
| mem_reads | `index_join_scan` | 2.96ms | 4.03ms | 1.361× | 2.62% | PASS |
| mem_reads | `types_table_scan` | 875.66ms | 993.88ms | 1.135× | 1.03% | PASS |
| mem_reads | `table_scan` | 1.02s | 1.08s | 1.061× | 1.62% | PASS |
| mem_reads | `oltp_read_only` | 84.12ms | 90.32ms | 1.074× | 2.23% | PASS |
| mem_writes | `oltp_bulk_insert` | 140.02ms | 195.28ms | 1.395× | 0.93% | PASS |
| mem_writes | `oltp_insert` | 12.45ms | 22.73ms | 1.825× | 0.82% | PASS |
| mem_writes | `oltp_update_index` | 42.82ms | 89.08ms | 2.080× | 1.88% | PASS |
| mem_writes | `oltp_update_non_index` | 30.28ms | 50.24ms | 1.659× | 1.85% | PASS |
| mem_writes | `oltp_delete_insert` | 37.96ms | 66.57ms | 1.754× | 1.60% | PASS |
| mem_writes | `oltp_write_only` | 18.99ms | 38.77ms | 2.042× | 1.67% | PASS |
| mem_writes | `types_delete_insert` | 21.07ms | 34.11ms | 1.619× | 1.97% | PASS |
| mem_writes | `oltp_read_write` | 55.41ms | 86.25ms | 1.557× | 2.63% | PASS |
| file_reads | `oltp_point_select` | 88.44ms | 44.77ms | 0.506× | 0.97% | PASS |
| file_reads | `oltp_range_select` | 16.20ms | 11.85ms | 0.732× | 2.79% | PASS |
| file_reads | `oltp_sum_range` | 15.08ms | 11.53ms | 0.765× | 1.57% | PASS |
| file_reads | `oltp_order_range` | 2.97ms | 2.55ms | 0.860× | 1.36% | PASS |
| file_reads | `oltp_distinct_range` | 3.77ms | 3.33ms | 0.883× | 1.54% | PASS |
| file_reads | `oltp_index_scan` | 10.40ms | 6.74ms | 0.648× | 1.21% | PASS |
| file_reads | `select_random_points` | 15.96ms | 11.75ms | 0.736× | 2.25% | PASS |
| file_reads | `select_random_ranges` | 9.46ms | 5.55ms | 0.586× | 1.25% | PASS |
| file_reads | `covering_index_scan` | 10.57ms | 5.94ms | 0.562× | 1.42% | PASS |
| file_reads | `groupby_scan` | 25.84ms | 27.49ms | 1.064× | 0.72% | PASS |
| file_reads | `index_join` | 8.71ms | 8.21ms | 0.942× | 1.87% | PASS |
| file_reads | `index_join_scan` | 3.73ms | 4.35ms | 1.165× | 2.24% | PASS |
| file_reads | `types_table_scan` | 861.03ms | 985.71ms | 1.145× | 0.69% | PASS |
| file_reads | `table_scan` | 999.19ms | 1.07s | 1.075× | 1.10% | PASS |
| file_reads | `oltp_read_only` | 181.90ms | 122.60ms | 0.674× | 1.08% | PASS |
| file_writes | `oltp_bulk_insert` | 180.34ms | 248.91ms | 1.380× | 4.15% | PASS |
| file_writes | `oltp_insert` | 23.18ms | 37.48ms | 1.617× | 13.64% | PASS |
| file_writes | `oltp_update_index` | 135.81ms | 149.89ms | 1.104× | 3.29% | PASS |
| file_writes | `oltp_update_non_index` | 107.47ms | 103.75ms | 0.965× | 4.25% | PASS |
| file_writes | `oltp_delete_insert` | 110.64ms | 124.00ms | 1.121× | 3.43% | PASS |
| file_writes | `oltp_write_only` | 82.41ms | 78.25ms | 0.950× | 2.46% | PASS |
| file_writes | `types_delete_insert` | 64.27ms | 60.70ms | 0.944× | 9.92% | PASS |
| file_writes | `oltp_read_write` | 125.32ms | 128.02ms | 1.022× | 5.08% | PASS |
| ac_reads | `oltp_point_select` | 42.86ms | 44.73ms | 1.044× | 1.12% | PASS |
| ac_reads | `oltp_range_select` | 11.68ms | 11.81ms | 1.012× | 1.17% | PASS |
| ac_reads | `oltp_sum_range` | 10.76ms | 11.52ms | 1.070× | 1.25% | PASS |
| ac_reads | `oltp_order_range` | 2.58ms | 2.56ms | 0.995× | 1.06% | PASS |
| ac_reads | `oltp_distinct_range` | 3.36ms | 3.33ms | 0.993× | 0.90% | PASS |
| ac_reads | `oltp_index_scan` | 5.94ms | 6.69ms | 1.127× | 0.86% | PASS |
| ac_reads | `select_random_points` | 11.62ms | 11.77ms | 1.013× | 1.51% | PASS |
| ac_reads | `select_random_ranges` | 5.02ms | 5.54ms | 1.104× | 0.76% | PASS |
| ac_reads | `covering_index_scan` | 6.20ms | 5.95ms | 0.960× | 1.10% | PASS |
| ac_reads | `groupby_scan` | 25.47ms | 27.50ms | 1.080× | 0.83% | PASS |
| ac_reads | `index_join` | 6.46ms | 8.19ms | 1.268× | 1.45% | PASS |
| ac_reads | `index_join_scan` | 3.31ms | 4.33ms | 1.311× | 2.61% | PASS |
| ac_reads | `types_table_scan` | 860.01ms | 988.66ms | 1.150× | 0.82% | PASS |
| ac_reads | `table_scan` | 1.00s | 1.07s | 1.073× | 0.79% | PASS |
| ac_reads | `oltp_read_only` | 116.87ms | 123.34ms | 1.055× | 1.03% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 28.11ms | 76.66ms | 2.727× | 42.22% | PASS |
| ac_writes | `oltp_insert_ac` | 22.57ms | 88.55ms | 3.923× | 25.88% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.81ms | 107.64ms | 3.736× | 34.36% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 19.30ms | 77.53ms | 4.016× | 22.01% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.41ms | 92.61ms | 4.132× | 28.25% | PASS |
| ac_writes | `oltp_write_only_ac` | 21.82ms | 90.92ms | 4.166× | 27.53% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.61ms | 96.79ms | 4.478× | 34.03% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.40ms | 108.31ms | 3.684× | 33.94% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.09ms | 37.68ms | 1.252× | 1.69% | PASS |
| mem_reads | `oltp_range_select` | 13.33ms | 14.26ms | 1.070× | 1.53% | PASS |
| mem_reads | `oltp_sum_range` | 12.12ms | 14.34ms | 1.183× | 1.78% | PASS |
| mem_reads | `oltp_order_range` | 3.03ms | 3.19ms | 1.054× | 1.26% | PASS |
| mem_reads | `oltp_distinct_range` | 4.09ms | 4.27ms | 1.044× | 1.12% | PASS |
| mem_reads | `oltp_index_scan` | 4.60ms | 6.40ms | 1.391× | 1.56% | PASS |
| mem_reads | `select_random_points` | 18.07ms | 21.01ms | 1.162× | 1.68% | PASS |
| mem_reads | `select_random_ranges` | 4.07ms | 5.22ms | 1.285× | 1.55% | PASS |
| mem_reads | `covering_index_scan` | 4.69ms | 4.69ms | 1.000× | 2.98% | PASS |
| mem_reads | `groupby_scan` | 32.26ms | 34.48ms | 1.069× | 0.97% | PASS |
| mem_reads | `index_join` | 7.12ms | 9.77ms | 1.373× | 2.17% | PASS |
| mem_reads | `index_join_scan` | 4.70ms | 5.45ms | 1.159× | 2.25% | PASS |
| mem_reads | `types_table_scan` | 1.11s | 1.26s | 1.131× | 2.48% | PASS |
| mem_reads | `table_scan` | 1.39s | 1.40s | 1.008× | 3.89% | PASS |
| mem_reads | `oltp_read_only` | 122.70ms | 139.10ms | 1.134× | 1.94% | PASS |
| mem_writes | `oltp_bulk_insert` | 237.28ms | 371.79ms | 1.567× | 0.87% | PASS |
| mem_writes | `oltp_insert` | 22.15ms | 42.66ms | 1.926× | 0.94% | PASS |
| mem_writes | `oltp_update_index` | 74.14ms | 146.88ms | 1.981× | 1.80% | PASS |
| mem_writes | `oltp_update_non_index` | 50.39ms | 95.26ms | 1.890× | 1.08% | PASS |
| mem_writes | `oltp_delete_insert` | 53.20ms | 113.73ms | 2.138× | 1.36% | PASS |
| mem_writes | `oltp_write_only` | 30.22ms | 68.84ms | 2.278× | 1.30% | PASS |
| mem_writes | `types_delete_insert` | 33.90ms | 61.59ms | 1.817× | 1.59% | PASS |
| mem_writes | `oltp_read_write` | 89.83ms | 148.72ms | 1.656× | 1.56% | PASS |
| file_reads | `oltp_point_select` | 100.91ms | 62.22ms | 0.617× | 1.04% | PASS |
| file_reads | `oltp_range_select` | 21.12ms | 16.95ms | 0.803× | 2.04% | PASS |
| file_reads | `oltp_sum_range` | 19.99ms | 16.95ms | 0.848× | 1.41% | PASS |
| file_reads | `oltp_order_range` | 3.93ms | 3.52ms | 0.896× | 1.55% | PASS |
| file_reads | `oltp_distinct_range` | 5.03ms | 4.59ms | 0.912× | 1.67% | PASS |
| file_reads | `oltp_index_scan` | 12.00ms | 9.04ms | 0.754× | 1.30% | PASS |
| file_reads | `select_random_points` | 26.71ms | 24.27ms | 0.909× | 1.38% | PASS |
| file_reads | `select_random_ranges` | 11.20ms | 7.68ms | 0.686× | 1.26% | PASS |
| file_reads | `covering_index_scan` | 12.49ms | 7.22ms | 0.578× | 1.94% | PASS |
| file_reads | `groupby_scan` | 33.29ms | 34.88ms | 1.048× | 1.27% | PASS |
| file_reads | `index_join` | 11.69ms | 11.52ms | 0.985× | 2.40% | PASS |
| file_reads | `index_join_scan` | 5.69ms | 5.88ms | 1.034× | 1.45% | PASS |
| file_reads | `types_table_scan` | 1.09s | 1.25s | 1.151× | 2.05% | PASS |
| file_reads | `table_scan` | 1.58s | 1.43s | 0.906× | 1.90% | PASS |
| file_reads | `oltp_read_only` | 243.07ms | 183.73ms | 0.756× | 0.88% | PASS |
| file_writes | `oltp_bulk_insert` | 259.54ms | 413.56ms | 1.593× | 0.96% | PASS |
| file_writes | `oltp_insert` | 57.85ms | 58.41ms | 1.010× | 15.64% | PASS |
| file_writes | `oltp_update_index` | 130.07ms | 195.65ms | 1.504× | 1.59% | PASS |
| file_writes | `oltp_update_non_index` | 108.35ms | 129.34ms | 1.194× | 7.10% | PASS |
| file_writes | `oltp_delete_insert` | 100.34ms | 152.03ms | 1.515× | 1.87% | PASS |
| file_writes | `oltp_write_only` | 96.08ms | 98.31ms | 1.023× | 10.72% | PASS |
| file_writes | `types_delete_insert` | 59.34ms | 82.27ms | 1.386× | 1.85% | PASS |
| file_writes | `oltp_read_write` | 156.44ms | 184.63ms | 1.180× | 3.61% | PASS |
| ac_reads | `oltp_point_select` | 56.70ms | 64.73ms | 1.142× | 1.21% | PASS |
| ac_reads | `oltp_range_select` | 17.97ms | 17.41ms | 0.969× | 2.75% | PASS |
| ac_reads | `oltp_sum_range` | 15.47ms | 17.02ms | 1.100× | 1.55% | PASS |
| ac_reads | `oltp_order_range` | 3.53ms | 3.51ms | 0.996× | 1.55% | PASS |
| ac_reads | `oltp_distinct_range` | 4.61ms | 4.63ms | 1.005× | 1.47% | PASS |
| ac_reads | `oltp_index_scan` | 7.64ms | 9.14ms | 1.196× | 0.95% | PASS |
| ac_reads | `select_random_points` | 23.20ms | 25.75ms | 1.110× | 2.76% | PASS |
| ac_reads | `select_random_ranges` | 6.97ms | 7.82ms | 1.121× | 1.66% | PASS |
| ac_reads | `covering_index_scan` | 8.88ms | 7.40ms | 0.833× | 2.11% | PASS |
| ac_reads | `groupby_scan` | 33.40ms | 35.25ms | 1.056× | 0.83% | PASS |
| ac_reads | `index_join` | 9.70ms | 11.68ms | 1.205× | 1.87% | PASS |
| ac_reads | `index_join_scan` | 5.49ms | 6.21ms | 1.132× | 3.73% | PASS |
| ac_reads | `types_table_scan` | 1.30s | 1.30s | 1.001× | 0.87% | PASS |
| ac_reads | `table_scan` | 1.61s | 1.45s | 0.902× | 0.78% | PASS |
| ac_reads | `oltp_read_only` | 172.44ms | 183.71ms | 1.065× | 1.08% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.29ms | 65.16ms | 3.060× | 8.19% | PASS |
| ac_writes | `oltp_insert_ac` | 25.25ms | 81.07ms | 3.211× | 5.41% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.55ms | 100.29ms | 3.640× | 4.66% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.98ms | 79.76ms | 3.629× | 6.21% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.80ms | 89.60ms | 3.764× | 6.63% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.26ms | 89.53ms | 3.544× | 7.01% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.89ms | 83.70ms | 3.823× | 9.77% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.16ms | 97.89ms | 3.142× | 5.58% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.03ms | 37.84ms | 1.219× | 1.74% | PASS |
| mem_reads | `oltp_range_select` | 13.27ms | 14.11ms | 1.063× | 3.10% | PASS |
| mem_reads | `oltp_sum_range` | 12.51ms | 14.14ms | 1.131× | 1.84% | PASS |
| mem_reads | `oltp_order_range` | 2.96ms | 3.18ms | 1.074× | 1.07% | PASS |
| mem_reads | `oltp_distinct_range` | 4.04ms | 4.29ms | 1.062× | 1.21% | PASS |
| mem_reads | `oltp_index_scan` | 4.78ms | 6.53ms | 1.366× | 1.69% | PASS |
| mem_reads | `select_random_points` | 18.50ms | 20.95ms | 1.133× | 2.79% | PASS |
| mem_reads | `select_random_ranges` | 4.23ms | 5.20ms | 1.230× | 2.06% | PASS |
| mem_reads | `covering_index_scan` | 4.47ms | 4.69ms | 1.049× | 2.94% | PASS |
| mem_reads | `groupby_scan` | 32.00ms | 34.48ms | 1.078× | 0.80% | PASS |
| mem_reads | `index_join` | 6.99ms | 9.71ms | 1.388× | 2.17% | PASS |
| mem_reads | `index_join_scan` | 4.52ms | 5.50ms | 1.216× | 3.20% | PASS |
| mem_reads | `types_table_scan` | 1.09s | 1.25s | 1.138× | 1.61% | PASS |
| mem_reads | `table_scan` | 1.41s | 1.41s | 0.999× | 0.71% | PASS |
| mem_reads | `oltp_read_only` | 127.16ms | 141.32ms | 1.111× | 1.35% | PASS |
| mem_writes | `oltp_bulk_insert` | 243.16ms | 368.46ms | 1.515× | 0.90% | PASS |
| mem_writes | `oltp_insert` | 20.56ms | 42.82ms | 2.083× | 1.89% | PASS |
| mem_writes | `oltp_update_index` | 71.14ms | 143.29ms | 2.014× | 2.49% | PASS |
| mem_writes | `oltp_update_non_index` | 50.62ms | 92.14ms | 1.820× | 1.51% | PASS |
| mem_writes | `oltp_delete_insert` | 50.95ms | 112.99ms | 2.218× | 1.57% | PASS |
| mem_writes | `oltp_write_only` | 29.51ms | 68.78ms | 2.331× | 1.78% | PASS |
| mem_writes | `types_delete_insert` | 33.93ms | 60.42ms | 1.781× | 1.94% | PASS |
| mem_writes | `oltp_read_write` | 89.05ms | 149.30ms | 1.677× | 2.40% | PASS |
| file_reads | `oltp_point_select` | 102.62ms | 61.93ms | 0.603× | 1.05% | PASS |
| file_reads | `oltp_range_select` | 21.66ms | 16.76ms | 0.774× | 1.74% | PASS |
| file_reads | `oltp_sum_range` | 20.22ms | 16.74ms | 0.828× | 1.55% | PASS |
| file_reads | `oltp_order_range` | 3.94ms | 3.52ms | 0.895× | 1.86% | PASS |
| file_reads | `oltp_distinct_range` | 5.05ms | 4.60ms | 0.910× | 1.84% | PASS |
| file_reads | `oltp_index_scan` | 12.40ms | 9.07ms | 0.732× | 1.95% | PASS |
| file_reads | `select_random_points` | 28.36ms | 25.08ms | 0.884× | 2.72% | PASS |
| file_reads | `select_random_ranges` | 11.60ms | 7.71ms | 0.665× | 1.34% | PASS |
| file_reads | `covering_index_scan` | 12.64ms | 7.27ms | 0.575× | 2.96% | PASS |
| file_reads | `groupby_scan` | 33.57ms | 35.07ms | 1.045× | 0.84% | PASS |
| file_reads | `index_join` | 11.75ms | 11.44ms | 0.973× | 2.79% | PASS |
| file_reads | `index_join_scan` | 5.60ms | 5.98ms | 1.067× | 3.09% | PASS |
| file_reads | `types_table_scan` | 1.11s | 1.25s | 1.125× | 2.79% | PASS |
| file_reads | `table_scan` | 1.34s | 1.40s | 1.042× | 2.83% | PASS |
| file_reads | `oltp_read_only` | 228.44ms | 174.56ms | 0.764× | 1.21% | PASS |
| file_writes | `oltp_bulk_insert` | 263.17ms | 396.48ms | 1.507× | 0.89% | PASS |
| file_writes | `oltp_insert` | 33.30ms | 56.03ms | 1.683× | 1.91% | PASS |
| file_writes | `oltp_update_index` | 109.75ms | 179.47ms | 1.635× | 2.15% | PASS |
| file_writes | `oltp_update_non_index` | 82.14ms | 118.46ms | 1.442× | 1.61% | PASS |
| file_writes | `oltp_delete_insert` | 86.18ms | 143.16ms | 1.661× | 2.08% | PASS |
| file_writes | `oltp_write_only` | 58.60ms | 93.06ms | 1.588× | 1.96% | PASS |
| file_writes | `types_delete_insert` | 54.75ms | 78.01ms | 1.425× | 2.30% | PASS |
| file_writes | `oltp_read_write` | 124.19ms | 176.17ms | 1.419× | 2.04% | PASS |
| ac_reads | `oltp_point_select` | 56.30ms | 62.30ms | 1.107× | 2.01% | PASS |
| ac_reads | `oltp_range_select` | 17.11ms | 16.76ms | 0.980× | 1.63% | PASS |
| ac_reads | `oltp_sum_range` | 16.39ms | 17.09ms | 1.042× | 2.06% | PASS |
| ac_reads | `oltp_order_range` | 3.53ms | 3.55ms | 1.006× | 1.87% | PASS |
| ac_reads | `oltp_distinct_range` | 4.54ms | 4.63ms | 1.020× | 1.70% | PASS |
| ac_reads | `oltp_index_scan` | 7.78ms | 9.09ms | 1.169× | 2.54% | PASS |
| ac_reads | `select_random_points` | 22.99ms | 24.94ms | 1.085× | 1.88% | PASS |
| ac_reads | `select_random_ranges` | 7.02ms | 7.68ms | 1.094× | 1.40% | PASS |
| ac_reads | `covering_index_scan` | 8.11ms | 7.24ms | 0.893× | 2.41% | PASS |
| ac_reads | `groupby_scan` | 32.81ms | 35.08ms | 1.069× | 0.83% | PASS |
| ac_reads | `index_join` | 9.48ms | 11.48ms | 1.211× | 2.92% | PASS |
| ac_reads | `index_join_scan` | 5.15ms | 6.09ms | 1.183× | 2.98% | PASS |
| ac_reads | `types_table_scan` | 1.14s | 1.26s | 1.108× | 1.90% | PASS |
| ac_reads | `table_scan` | 1.42s | 1.42s | 0.998× | 1.76% | PASS |
| ac_reads | `oltp_read_only` | 164.21ms | 177.54ms | 1.081× | 1.27% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.10ms | 60.80ms | 3.025× | 6.59% | PASS |
| ac_writes | `oltp_insert_ac` | 23.07ms | 84.35ms | 3.657× | 7.26% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.87ms | 95.76ms | 3.564× | 6.99% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.49ms | 73.28ms | 3.576× | 3.86% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.40ms | 86.02ms | 3.676× | 6.54% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.60ms | 83.87ms | 3.711× | 4.41% | PASS |
| ac_writes | `types_delete_insert_ac` | 19.87ms | 77.32ms | 3.892× | 7.18% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.46ms | 92.25ms | 3.241× | 4.82% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.52ms | 29.89ms | 1.127× | 2.17% | PASS |
| mem_reads | `oltp_range_select` | 15.70ms | 15.71ms | 1.001× | 1.54% | PASS |
| mem_reads | `oltp_sum_range` | 14.33ms | 15.12ms | 1.055× | 0.89% | PASS |
| mem_reads | `oltp_order_range` | 2.93ms | 2.93ms | 1.000× | 0.85% | PASS |
| mem_reads | `oltp_distinct_range` | 3.75ms | 3.75ms | 1.001× | 0.80% | PASS |
| mem_reads | `oltp_index_scan` | 3.73ms | 4.58ms | 1.226× | 1.73% | PASS |
| mem_reads | `select_random_points` | 28.54ms | 30.86ms | 1.081× | 10.37% | PASS |
| mem_reads | `select_random_ranges` | 7.33ms | 7.84ms | 1.070× | 11.62% | PASS |
| mem_reads | `covering_index_scan` | 4.29ms | 4.45ms | 1.037× | 17.31% | PASS |
| mem_reads | `groupby_scan` | 35.97ms | 39.97ms | 1.111× | 14.86% | PASS |
| mem_reads | `index_join` | 8.41ms | 10.98ms | 1.306× | 9.91% | PASS |
| mem_reads | `index_join_scan` | 4.47ms | 5.58ms | 1.249× | 20.35% | PASS |
| mem_reads | `types_table_scan` | 1.34s | 1.25s | 0.938× | 3.29% | PASS |
| mem_reads | `table_scan` | 1.30s | 1.16s | 0.890× | 12.50% | PASS |
| mem_reads | `oltp_read_only` | 120.25ms | 124.49ms | 1.035× | 1.51% | PASS |
| mem_writes | `oltp_bulk_insert` | 193.82ms | 284.14ms | 1.466× | 2.88% | PASS |
| mem_writes | `oltp_insert` | 17.49ms | 34.09ms | 1.949× | 14.52% | PASS |
| mem_writes | `oltp_update_index` | 69.40ms | 128.00ms | 1.844× | 19.07% | PASS |
| mem_writes | `oltp_update_non_index` | 51.26ms | 82.20ms | 1.604× | 12.65% | PASS |
| mem_writes | `oltp_delete_insert` | 62.84ms | 102.78ms | 1.635× | 11.22% | PASS |
| mem_writes | `oltp_write_only` | 31.19ms | 61.20ms | 1.962× | 16.14% | PASS |
| mem_writes | `types_delete_insert` | 35.17ms | 52.63ms | 1.497× | 13.95% | PASS |
| mem_writes | `oltp_read_write` | 116.17ms | 169.57ms | 1.460× | 18.58% | PASS |
| file_reads | `oltp_point_select` | 100.33ms | 55.78ms | 0.556× | 3.31% | PASS |
| file_reads | `oltp_range_select` | 29.81ms | 20.82ms | 0.699× | 19.65% | PASS |
| file_reads | `oltp_sum_range` | 26.37ms | 22.69ms | 0.860× | 18.25% | PASS |
| file_reads | `oltp_order_range` | 4.33ms | 3.54ms | 0.818× | 9.85% | PASS |
| file_reads | `oltp_distinct_range` | 4.69ms | 4.14ms | 0.883× | 1.97% | PASS |
| file_reads | `oltp_index_scan` | 11.01ms | 7.42ms | 0.675× | 0.86% | PASS |
| file_reads | `select_random_points` | 29.43ms | 26.80ms | 0.911× | 1.39% | PASS |
| file_reads | `select_random_ranges` | 13.78ms | 9.38ms | 0.681× | 2.22% | PASS |
| file_reads | `covering_index_scan` | 11.61ms | 6.57ms | 0.566× | 4.47% | PASS |
| file_reads | `groupby_scan` | 34.07ms | 35.28ms | 1.036× | 8.24% | PASS |
| file_reads | `index_join` | 11.14ms | 10.48ms | 0.941× | 2.59% | PASS |
| file_reads | `index_join_scan` | 5.07ms | 6.55ms | 1.293× | 11.91% | PASS |
| file_reads | `types_table_scan` | 1.21s | 1.15s | 0.955× | 3.34% | PASS |
| file_reads | `table_scan` | 1.01s | 1.06s | 1.047× | 0.82% | PASS |
| file_reads | `oltp_read_only` | 217.64ms | 157.55ms | 0.724× | 0.73% | PASS |
| file_writes | `oltp_bulk_insert` | 257.08ms | 387.97ms | 1.509× | 3.95% | PASS |
| file_writes | `oltp_insert` | 39.56ms | 61.52ms | 1.555× | 10.67% | PASS |
| file_writes | `oltp_update_index` | 198.70ms | 219.05ms | 1.102× | 10.79% | PASS |
| file_writes | `oltp_update_non_index` | 158.52ms | 157.84ms | 0.996× | 6.43% | PASS |
| file_writes | `oltp_delete_insert` | 162.31ms | 176.18ms | 1.085× | 9.80% | PASS |
| file_writes | `oltp_write_only` | 108.89ms | 124.80ms | 1.146× | 6.20% | PASS |
| file_writes | `types_delete_insert` | 96.31ms | 103.23ms | 1.072× | 7.93% | PASS |
| file_writes | `oltp_read_write` | 200.54ms | 233.91ms | 1.166× | 9.09% | PASS |
| ac_reads | `oltp_point_select` | 50.24ms | 53.83ms | 1.071× | 1.79% | PASS |
| ac_reads | `oltp_range_select` | 18.70ms | 18.22ms | 0.974× | 1.78% | PASS |
| ac_reads | `oltp_sum_range` | 17.09ms | 17.72ms | 1.037× | 1.07% | PASS |
| ac_reads | `oltp_order_range` | 3.78ms | 3.43ms | 0.907× | 11.42% | PASS |
| ac_reads | `oltp_distinct_range` | 4.29ms | 4.16ms | 0.969× | 2.71% | PASS |
| ac_reads | `oltp_index_scan` | 6.95ms | 7.93ms | 1.141× | 6.92% | PASS |
| ac_reads | `select_random_points` | 31.21ms | 32.85ms | 1.053× | 16.19% | PASS |
| ac_reads | `select_random_ranges` | 9.59ms | 10.08ms | 1.051× | 12.62% | PASS |
| ac_reads | `covering_index_scan` | 6.84ms | 6.90ms | 1.009× | 14.39% | PASS |
| ac_reads | `groupby_scan` | 33.05ms | 34.74ms | 1.051× | 13.22% | PASS |
| ac_reads | `index_join` | 9.85ms | 12.49ms | 1.268× | 8.13% | PASS |
| ac_reads | `index_join_scan` | 4.88ms | 6.67ms | 1.368× | 12.99% | PASS |
| ac_reads | `types_table_scan` | 892.77ms | 984.49ms | 1.103× | 3.90% | PASS |
| ac_reads | `table_scan` | 1.56s | 1.26s | 0.806× | 2.06% | PASS |
| ac_reads | `oltp_read_only` | 191.75ms | 196.81ms | 1.026× | 6.40% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.62ms | 55.27ms | 3.137× | 6.07% | PASS |
| ac_writes | `oltp_insert_ac` | 20.31ms | 78.37ms | 3.858× | 9.23% | PASS |
| ac_writes | `oltp_update_index_ac` | 22.39ms | 88.65ms | 3.959× | 8.32% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 18.62ms | 66.90ms | 3.594× | 6.04% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 20.62ms | 80.00ms | 3.880× | 7.68% | PASS |
| ac_writes | `oltp_write_only_ac` | 20.35ms | 75.17ms | 3.694× | 8.51% | PASS |
| ac_writes | `types_delete_insert_ac` | 18.02ms | 66.31ms | 3.680× | 7.16% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.23ms | 89.67ms | 3.293× | 9.92% | PASS |

</details>

## Version-control latency

Wall time: 2m 24s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 84.88ms | 200.00ms | 42.4% | 0.45% | PASS |
| `status_dirty_many_tables` | 87.79ms | 200.00ms | 43.9% | 0.48% | PASS |
| `diff_regular_working_one_table` | 80.60ms | 150.00ms | 53.7% | 0.41% | PASS |
| `diff_regular_working_many_tables` | 93.68ms | 200.00ms | 46.8% | 0.44% | PASS |
| `diff_stat_working_many_tables` | 93.91ms | 200.00ms | 47.0% | 0.70% | PASS |
| `diff_schema_working_many_tables` | 94.09ms | 200.00ms | 47.0% | 0.63% | PASS |
| `branch_list_many_branches` | 24.20ms | 100.00ms | 24.2% | 2.08% | PASS |
| `branch_create_delete` | 26.36ms | 100.00ms | 26.4% | 1.15% | PASS |
| `checkout_branch_clean` | 57.33ms | 200.00ms | 28.7% | 0.90% | PASS |
| `merge_data_no_conflicts` | 30.86ms | 150.00ms | 20.6% | 1.68% | PASS |
| `merge_schema_no_conflicts` | 23.74ms | 100.00ms | 23.7% | 2.27% | PASS |
| `merge_data_conflicts` | 129.09ms | 250.00ms | 51.6% | 0.32% | PASS |
| `merge_data_conflicts_with_resolve` | 128.94ms | 250.00ms | 51.6% | 0.38% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
