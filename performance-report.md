# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-19 14:54 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260907.300.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/35445376446)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 37s | 8.84s | 11.49s | 1.300× | 1.26% | **PASS** |
| textpk | 69 | 55 | 1h 32m 18s | 9.67s | 11.85s | 1.225× | 1.60% | **PASS** |
| blobpk | 69 | 55 | 1h 31m 53s | 9.88s | 11.93s | 1.207× | 2.10% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 9s | 9.69s | 12.05s | 1.244× | 1.34% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.89ms | 29.42ms | 1.232× | 1.48% | PASS |
| mem_reads | `oltp_range_select` | 10.04ms | 13.13ms | 1.308× | 1.52% | PASS |
| mem_reads | `oltp_sum_range` | 9.50ms | 12.42ms | 1.308× | 0.61% | PASS |
| mem_reads | `oltp_order_range` | 2.53ms | 2.99ms | 1.179× | 0.99% | PASS |
| mem_reads | `oltp_distinct_range` | 3.63ms | 4.06ms | 1.120× | 1.07% | PASS |
| mem_reads | `oltp_index_scan` | 3.92ms | 5.30ms | 1.354× | 1.63% | PASS |
| mem_reads | `select_random_points` | 9.99ms | 11.17ms | 1.118× | 1.99% | PASS |
| mem_reads | `select_random_ranges` | 2.96ms | 3.98ms | 1.343× | 1.19% | PASS |
| mem_reads | `covering_index_scan` | 4.24ms | 4.23ms | 0.997× | 1.07% | PASS |
| mem_reads | `groupby_scan` | 29.86ms | 32.91ms | 1.102× | 0.67% | PASS |
| mem_reads | `index_join` | 6.02ms | 8.12ms | 1.350× | 1.59% | PASS |
| mem_reads | `index_join_scan` | 3.50ms | 4.71ms | 1.344× | 1.96% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.34s | 1.288× | 0.77% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.40s | 1.190× | 0.88% | PASS |
| mem_reads | `oltp_read_only` | 102.80ms | 123.53ms | 1.202× | 0.98% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.47ms | 252.30ms | 1.390× | 1.24% | PASS |
| mem_writes | `oltp_insert` | 15.54ms | 28.63ms | 1.842× | 0.98% | PASS |
| mem_writes | `oltp_update_index` | 49.85ms | 105.16ms | 2.109× | 1.05% | PASS |
| mem_writes | `oltp_update_non_index` | 33.79ms | 59.80ms | 1.770× | 1.26% | PASS |
| mem_writes | `oltp_delete_insert` | 44.90ms | 79.07ms | 1.761× | 1.23% | PASS |
| mem_writes | `oltp_write_only` | 21.72ms | 45.11ms | 2.077× | 1.25% | PASS |
| mem_writes | `types_delete_insert` | 24.50ms | 40.66ms | 1.660× | 1.15% | PASS |
| mem_writes | `oltp_read_write` | 66.64ms | 110.89ms | 1.664× | 1.43% | PASS |
| file_reads | `oltp_point_select` | 98.63ms | 55.15ms | 0.559× | 0.95% | PASS |
| file_reads | `oltp_range_select` | 17.84ms | 15.80ms | 0.885× | 2.35% | PASS |
| file_reads | `oltp_sum_range` | 17.19ms | 15.09ms | 0.878× | 1.16% | PASS |
| file_reads | `oltp_order_range` | 3.36ms | 3.31ms | 0.986× | 1.40% | PASS |
| file_reads | `oltp_distinct_range` | 4.48ms | 4.39ms | 0.979× | 1.46% | PASS |
| file_reads | `oltp_index_scan` | 11.61ms | 8.26ms | 0.711× | 1.07% | PASS |
| file_reads | `select_random_points` | 17.63ms | 13.81ms | 0.784× | 1.84% | PASS |
| file_reads | `select_random_ranges` | 10.44ms | 6.59ms | 0.632× | 1.24% | PASS |
| file_reads | `covering_index_scan` | 11.82ms | 7.10ms | 0.601× | 0.99% | PASS |
| file_reads | `groupby_scan` | 30.69ms | 33.42ms | 1.089× | 0.90% | PASS |
| file_reads | `index_join` | 10.28ms | 10.19ms | 0.991× | 1.30% | PASS |
| file_reads | `index_join_scan` | 4.45ms | 5.01ms | 1.124× | 2.11% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.33s | 1.280× | 0.49% | PASS |
| file_reads | `table_scan` | 1.18s | 1.40s | 1.183× | 0.64% | PASS |
| file_reads | `oltp_read_only` | 214.37ms | 161.96ms | 0.756× | 0.89% | PASS |
| file_writes | `oltp_bulk_insert` | 194.54ms | 272.83ms | 1.402× | 1.47% | PASS |
| file_writes | `oltp_insert` | 22.11ms | 36.01ms | 1.629× | 1.39% | PASS |
| file_writes | `oltp_update_index` | 77.56ms | 128.45ms | 1.656× | 1.10% | PASS |
| file_writes | `oltp_update_non_index` | 57.71ms | 81.45ms | 1.411× | 1.91% | PASS |
| file_writes | `oltp_delete_insert` | 68.10ms | 99.84ms | 1.466× | 1.21% | PASS |
| file_writes | `oltp_write_only` | 44.42ms | 65.02ms | 1.464× | 1.78% | PASS |
| file_writes | `types_delete_insert` | 40.12ms | 53.64ms | 1.337× | 1.26% | PASS |
| file_writes | `oltp_read_write` | 91.29ms | 130.25ms | 1.427× | 1.21% | PASS |
| ac_reads | `oltp_point_select` | 48.75ms | 55.30ms | 1.134× | 1.17% | PASS |
| ac_reads | `oltp_range_select` | 12.95ms | 15.75ms | 1.217× | 1.60% | PASS |
| ac_reads | `oltp_sum_range` | 12.47ms | 15.16ms | 1.215× | 1.34% | PASS |
| ac_reads | `oltp_order_range` | 2.89ms | 3.30ms | 1.142× | 1.34% | PASS |
| ac_reads | `oltp_distinct_range` | 3.96ms | 4.38ms | 1.105× | 1.04% | PASS |
| ac_reads | `oltp_index_scan` | 6.70ms | 8.29ms | 1.238× | 1.72% | PASS |
| ac_reads | `select_random_points` | 12.98ms | 13.89ms | 1.070× | 1.66% | PASS |
| ac_reads | `select_random_ranges` | 5.54ms | 6.57ms | 1.186× | 0.76% | PASS |
| ac_reads | `covering_index_scan` | 7.00ms | 7.08ms | 1.011× | 1.26% | PASS |
| ac_reads | `groupby_scan` | 30.12ms | 33.45ms | 1.111× | 0.61% | PASS |
| ac_reads | `index_join` | 7.79ms | 10.33ms | 1.326× | 1.30% | PASS |
| ac_reads | `index_join_scan` | 3.90ms | 5.00ms | 1.279× | 2.29% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.33s | 1.283× | 0.67% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.40s | 1.188× | 0.74% | PASS |
| ac_reads | `oltp_read_only` | 140.03ms | 161.90ms | 1.156× | 1.32% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.16ms | 78.01ms | 3.687× | 2.78% | PASS |
| ac_writes | `oltp_insert_ac` | 24.07ms | 93.73ms | 3.893× | 5.66% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.53ms | 108.21ms | 4.239× | 3.56% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.69ms | 88.29ms | 4.070× | 3.23% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.40ms | 100.59ms | 4.299× | 3.73% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.75ms | 98.91ms | 4.164× | 3.16% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.75ms | 89.97ms | 3.955× | 6.27% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.12ms | 105.55ms | 3.625× | 3.76% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.21ms | 37.30ms | 1.235× | 1.60% | PASS |
| mem_reads | `oltp_range_select` | 13.37ms | 14.14ms | 1.058× | 2.64% | PASS |
| mem_reads | `oltp_sum_range` | 12.24ms | 14.09ms | 1.151× | 1.44% | PASS |
| mem_reads | `oltp_order_range` | 2.93ms | 3.15ms | 1.075× | 1.38% | PASS |
| mem_reads | `oltp_distinct_range` | 3.99ms | 4.21ms | 1.053× | 1.01% | PASS |
| mem_reads | `oltp_index_scan` | 4.49ms | 6.26ms | 1.395× | 1.38% | PASS |
| mem_reads | `select_random_points` | 18.10ms | 20.75ms | 1.147× | 1.91% | PASS |
| mem_reads | `select_random_ranges` | 4.01ms | 5.17ms | 1.289× | 1.61% | PASS |
| mem_reads | `covering_index_scan` | 4.52ms | 4.49ms | 0.993× | 1.60% | PASS |
| mem_reads | `groupby_scan` | 31.80ms | 33.66ms | 1.059× | 1.09% | PASS |
| mem_reads | `index_join` | 6.89ms | 9.02ms | 1.310× | 2.38% | PASS |
| mem_reads | `index_join_scan` | 4.80ms | 5.27ms | 1.099× | 2.52% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.23s | 1.174× | 0.82% | PASS |
| mem_reads | `table_scan` | 1.24s | 1.37s | 1.111× | 1.63% | PASS |
| mem_reads | `oltp_read_only` | 119.45ms | 136.02ms | 1.139× | 1.75% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.01ms | 356.72ms | 1.511× | 0.72% | PASS |
| mem_writes | `oltp_insert` | 21.71ms | 40.11ms | 1.847× | 1.35% | PASS |
| mem_writes | `oltp_update_index` | 71.46ms | 133.38ms | 1.866× | 1.06% | PASS |
| mem_writes | `oltp_update_non_index` | 49.15ms | 86.44ms | 1.759× | 1.32% | PASS |
| mem_writes | `oltp_delete_insert` | 51.23ms | 103.51ms | 2.020× | 1.07% | PASS |
| mem_writes | `oltp_write_only` | 28.98ms | 62.09ms | 2.142× | 1.04% | PASS |
| mem_writes | `types_delete_insert` | 33.52ms | 54.81ms | 1.635× | 1.24% | PASS |
| mem_writes | `oltp_read_write` | 87.90ms | 140.71ms | 1.601× | 2.43% | PASS |
| file_reads | `oltp_point_select` | 105.81ms | 63.98ms | 0.605× | 1.21% | PASS |
| file_reads | `oltp_range_select` | 21.46ms | 17.06ms | 0.795× | 2.05% | PASS |
| file_reads | `oltp_sum_range` | 20.46ms | 17.24ms | 0.843× | 1.37% | PASS |
| file_reads | `oltp_order_range` | 3.88ms | 3.53ms | 0.910× | 1.50% | PASS |
| file_reads | `oltp_distinct_range` | 4.99ms | 4.62ms | 0.926× | 2.05% | PASS |
| file_reads | `oltp_index_scan` | 12.20ms | 9.28ms | 0.761× | 1.72% | PASS |
| file_reads | `select_random_points` | 26.96ms | 24.46ms | 0.907× | 1.48% | PASS |
| file_reads | `select_random_ranges` | 11.61ms | 7.88ms | 0.679× | 1.18% | PASS |
| file_reads | `covering_index_scan` | 12.22ms | 7.42ms | 0.607× | 1.52% | PASS |
| file_reads | `groupby_scan` | 32.95ms | 34.20ms | 1.038× | 1.31% | PASS |
| file_reads | `index_join` | 11.29ms | 11.34ms | 1.004× | 1.73% | PASS |
| file_reads | `index_join_scan` | 5.83ms | 5.82ms | 0.997× | 1.20% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.23s | 1.164× | 0.85% | PASS |
| file_reads | `table_scan` | 1.24s | 1.37s | 1.106× | 1.17% | PASS |
| file_reads | `oltp_read_only` | 232.75ms | 177.67ms | 0.763× | 1.30% | PASS |
| file_writes | `oltp_bulk_insert` | 256.98ms | 388.30ms | 1.511× | 1.18% | PASS |
| file_writes | `oltp_insert` | 46.80ms | 52.95ms | 1.131× | 21.33% | PASS |
| file_writes | `oltp_update_index` | 113.91ms | 170.61ms | 1.498× | 1.97% | PASS |
| file_writes | `oltp_update_non_index` | 96.97ms | 112.61ms | 1.161× | 7.93% | PASS |
| file_writes | `oltp_delete_insert` | 91.04ms | 134.88ms | 1.482× | 2.05% | PASS |
| file_writes | `oltp_write_only` | 91.06ms | 86.01ms | 0.945× | 12.92% | PASS |
| file_writes | `types_delete_insert` | 56.00ms | 74.65ms | 1.333× | 1.82% | PASS |
| file_writes | `oltp_read_write` | 140.86ms | 164.30ms | 1.166× | 5.43% | PASS |
| ac_reads | `oltp_point_select` | 55.42ms | 64.11ms | 1.157× | 1.16% | PASS |
| ac_reads | `oltp_range_select` | 17.57ms | 17.12ms | 0.974× | 2.05% | PASS |
| ac_reads | `oltp_sum_range` | 16.07ms | 17.34ms | 1.079× | 1.49% | PASS |
| ac_reads | `oltp_order_range` | 3.43ms | 3.58ms | 1.043× | 1.95% | PASS |
| ac_reads | `oltp_distinct_range` | 4.50ms | 4.64ms | 1.031× | 1.05% | PASS |
| ac_reads | `oltp_index_scan` | 7.45ms | 9.32ms | 1.252× | 2.42% | PASS |
| ac_reads | `select_random_points` | 21.77ms | 24.68ms | 1.133× | 1.48% | PASS |
| ac_reads | `select_random_ranges` | 6.75ms | 7.89ms | 1.170× | 0.83% | PASS |
| ac_reads | `covering_index_scan` | 8.20ms | 7.42ms | 0.905× | 3.95% | PASS |
| ac_reads | `groupby_scan` | 32.52ms | 34.25ms | 1.053× | 0.85% | PASS |
| ac_reads | `index_join` | 9.26ms | 11.34ms | 1.224× | 3.16% | PASS |
| ac_reads | `index_join_scan` | 5.34ms | 5.84ms | 1.092× | 2.04% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.23s | 1.164× | 0.76% | PASS |
| ac_reads | `table_scan` | 1.24s | 1.38s | 1.106× | 1.16% | PASS |
| ac_reads | `oltp_read_only` | 159.20ms | 177.53ms | 1.115× | 1.29% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.02ms | 82.74ms | 3.594× | 7.25% | PASS |
| ac_writes | `oltp_insert_ac` | 26.11ms | 98.48ms | 3.772× | 5.98% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.29ms | 114.76ms | 4.056× | 5.41% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.12ms | 96.82ms | 4.187× | 6.00% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.99ms | 105.47ms | 4.220× | 5.88% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.85ms | 103.43ms | 3.852× | 7.41% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.41ms | 95.26ms | 4.251× | 5.56% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.13ms | 113.23ms | 3.417× | 6.71% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.46ms | 36.67ms | 1.204× | 1.28% | PASS |
| mem_reads | `oltp_range_select` | 13.19ms | 13.97ms | 1.059× | 3.58% | PASS |
| mem_reads | `oltp_sum_range` | 12.35ms | 13.95ms | 1.130× | 3.19% | PASS |
| mem_reads | `oltp_order_range` | 2.97ms | 3.14ms | 1.057× | 1.69% | PASS |
| mem_reads | `oltp_distinct_range` | 4.02ms | 4.23ms | 1.052× | 1.34% | PASS |
| mem_reads | `oltp_index_scan` | 4.61ms | 6.30ms | 1.366× | 2.76% | PASS |
| mem_reads | `select_random_points` | 18.13ms | 20.89ms | 1.152× | 2.52% | PASS |
| mem_reads | `select_random_ranges` | 4.08ms | 5.14ms | 1.261× | 1.64% | PASS |
| mem_reads | `covering_index_scan` | 4.45ms | 4.60ms | 1.034× | 2.28% | PASS |
| mem_reads | `groupby_scan` | 32.07ms | 33.78ms | 1.053× | 1.16% | PASS |
| mem_reads | `index_join` | 7.01ms | 9.53ms | 1.359× | 4.17% | PASS |
| mem_reads | `index_join_scan` | 4.43ms | 5.34ms | 1.205× | 3.47% | PASS |
| mem_reads | `types_table_scan` | 1.08s | 1.24s | 1.145× | 2.05% | PASS |
| mem_reads | `table_scan` | 1.30s | 1.39s | 1.069× | 1.56% | PASS |
| mem_reads | `oltp_read_only` | 124.24ms | 137.29ms | 1.105× | 1.85% | PASS |
| mem_writes | `oltp_bulk_insert` | 242.31ms | 353.54ms | 1.459× | 0.82% | PASS |
| mem_writes | `oltp_insert` | 20.12ms | 39.56ms | 1.967× | 0.75% | PASS |
| mem_writes | `oltp_update_index` | 69.44ms | 132.59ms | 1.910× | 2.50% | PASS |
| mem_writes | `oltp_update_non_index` | 50.12ms | 85.89ms | 1.714× | 2.33% | PASS |
| mem_writes | `oltp_delete_insert` | 50.42ms | 105.07ms | 2.084× | 2.83% | PASS |
| mem_writes | `oltp_write_only` | 28.34ms | 63.02ms | 2.224× | 3.69% | PASS |
| mem_writes | `types_delete_insert` | 33.68ms | 54.90ms | 1.630× | 1.52% | PASS |
| mem_writes | `oltp_read_write` | 87.91ms | 141.89ms | 1.614× | 2.37% | PASS |
| file_reads | `oltp_point_select` | 108.12ms | 63.66ms | 0.589× | 1.37% | PASS |
| file_reads | `oltp_range_select` | 21.66ms | 16.85ms | 0.778× | 2.10% | PASS |
| file_reads | `oltp_sum_range` | 21.19ms | 16.84ms | 0.795× | 1.86% | PASS |
| file_reads | `oltp_order_range` | 3.90ms | 3.49ms | 0.894× | 1.67% | PASS |
| file_reads | `oltp_distinct_range` | 5.00ms | 4.58ms | 0.916× | 1.93% | PASS |
| file_reads | `oltp_index_scan` | 12.49ms | 9.07ms | 0.726× | 2.03% | PASS |
| file_reads | `select_random_points` | 28.28ms | 24.49ms | 0.866× | 2.64% | PASS |
| file_reads | `select_random_ranges` | 11.99ms | 7.81ms | 0.651× | 1.54% | PASS |
| file_reads | `covering_index_scan` | 12.62ms | 7.36ms | 0.583× | 3.44% | PASS |
| file_reads | `groupby_scan` | 33.19ms | 34.30ms | 1.033× | 1.20% | PASS |
| file_reads | `index_join` | 11.71ms | 11.45ms | 0.978× | 3.18% | PASS |
| file_reads | `index_join_scan` | 5.50ms | 5.83ms | 1.060× | 3.58% | PASS |
| file_reads | `types_table_scan` | 1.09s | 1.24s | 1.141× | 1.37% | PASS |
| file_reads | `table_scan` | 1.31s | 1.39s | 1.058× | 1.40% | PASS |
| file_reads | `oltp_read_only` | 240.72ms | 178.69ms | 0.742× | 1.02% | PASS |
| file_writes | `oltp_bulk_insert` | 261.61ms | 382.69ms | 1.463× | 0.92% | PASS |
| file_writes | `oltp_insert` | 32.53ms | 52.68ms | 1.619× | 2.55% | PASS |
| file_writes | `oltp_update_index` | 106.31ms | 164.94ms | 1.552× | 1.87% | PASS |
| file_writes | `oltp_update_non_index` | 79.85ms | 108.94ms | 1.364× | 1.61% | PASS |
| file_writes | `oltp_delete_insert` | 82.34ms | 131.60ms | 1.598× | 2.39% | PASS |
| file_writes | `oltp_write_only` | 57.15ms | 85.06ms | 1.488× | 2.53% | PASS |
| file_writes | `types_delete_insert` | 53.68ms | 72.85ms | 1.357× | 2.19% | PASS |
| file_writes | `oltp_read_write` | 121.62ms | 164.75ms | 1.355× | 2.40% | PASS |
| ac_reads | `oltp_point_select` | 57.34ms | 63.96ms | 1.116× | 1.12% | PASS |
| ac_reads | `oltp_range_select` | 17.06ms | 16.89ms | 0.990× | 1.93% | PASS |
| ac_reads | `oltp_sum_range` | 15.73ms | 16.90ms | 1.074× | 2.06% | PASS |
| ac_reads | `oltp_order_range` | 3.37ms | 3.49ms | 1.037× | 1.96% | PASS |
| ac_reads | `oltp_distinct_range` | 4.42ms | 4.61ms | 1.042× | 1.30% | PASS |
| ac_reads | `oltp_index_scan` | 7.60ms | 9.14ms | 1.203× | 2.49% | PASS |
| ac_reads | `select_random_points` | 22.56ms | 24.40ms | 1.082× | 3.22% | PASS |
| ac_reads | `select_random_ranges` | 6.94ms | 7.82ms | 1.127× | 1.47% | PASS |
| ac_reads | `covering_index_scan` | 7.89ms | 7.34ms | 0.930× | 4.40% | PASS |
| ac_reads | `groupby_scan` | 32.52ms | 34.28ms | 1.054× | 1.13% | PASS |
| ac_reads | `index_join` | 9.23ms | 11.39ms | 1.234× | 3.79% | PASS |
| ac_reads | `index_join_scan` | 4.99ms | 5.85ms | 1.173× | 3.54% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.23s | 1.157× | 1.56% | PASS |
| ac_reads | `table_scan` | 1.31s | 1.39s | 1.064× | 1.56% | PASS |
| ac_reads | `oltp_read_only` | 164.32ms | 177.51ms | 1.080× | 1.90% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.07ms | 82.80ms | 3.589× | 7.81% | PASS |
| ac_writes | `oltp_insert_ac` | 27.02ms | 106.18ms | 3.930× | 4.34% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.97ms | 121.28ms | 4.047× | 8.27% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.99ms | 97.94ms | 4.082× | 8.27% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.43ms | 107.47ms | 4.067× | 5.34% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.22ms | 109.58ms | 4.179× | 7.57% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.79ms | 103.84ms | 4.365× | 9.57% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.04ms | 114.84ms | 3.476× | 7.10% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.78ms | 40.49ms | 1.199× | 1.84% | PASS |
| mem_reads | `oltp_range_select` | 20.11ms | 21.12ms | 1.050× | 1.60% | PASS |
| mem_reads | `oltp_sum_range` | 18.41ms | 20.98ms | 1.140× | 0.97% | PASS |
| mem_reads | `oltp_order_range` | 3.56ms | 3.84ms | 1.078× | 1.02% | PASS |
| mem_reads | `oltp_distinct_range` | 4.69ms | 4.90ms | 1.045× | 1.16% | PASS |
| mem_reads | `oltp_index_scan` | 4.58ms | 6.16ms | 1.345× | 1.53% | PASS |
| mem_reads | `select_random_points` | 27.76ms | 32.00ms | 1.152× | 1.37% | PASS |
| mem_reads | `select_random_ranges` | 7.74ms | 9.01ms | 1.165× | 1.19% | PASS |
| mem_reads | `covering_index_scan` | 4.26ms | 4.19ms | 0.983× | 1.70% | PASS |
| mem_reads | `groupby_scan` | 36.32ms | 38.65ms | 1.064× | 0.79% | PASS |
| mem_reads | `index_join` | 8.26ms | 10.37ms | 1.255× | 1.56% | PASS |
| mem_reads | `index_join_scan` | 4.24ms | 5.40ms | 1.273× | 2.08% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.24s | 1.188× | 0.77% | PASS |
| mem_reads | `table_scan` | 1.24s | 1.40s | 1.127× | 2.12% | PASS |
| mem_reads | `oltp_read_only` | 156.57ms | 169.95ms | 1.085× | 0.92% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.39ms | 357.38ms | 1.433× | 0.73% | PASS |
| mem_writes | `oltp_insert` | 19.34ms | 36.70ms | 1.898× | 0.80% | PASS |
| mem_writes | `oltp_update_index` | 69.81ms | 119.22ms | 1.708× | 1.24% | PASS |
| mem_writes | `oltp_update_non_index` | 51.51ms | 83.54ms | 1.622× | 1.24% | PASS |
| mem_writes | `oltp_delete_insert` | 50.91ms | 96.84ms | 1.902× | 1.25% | PASS |
| mem_writes | `oltp_write_only` | 27.76ms | 58.06ms | 2.092× | 1.15% | PASS |
| mem_writes | `types_delete_insert` | 33.40ms | 55.26ms | 1.654× | 1.60% | PASS |
| mem_writes | `oltp_read_write` | 105.93ms | 156.60ms | 1.478× | 1.29% | PASS |
| file_reads | `oltp_point_select` | 110.14ms | 66.54ms | 0.604× | 0.84% | PASS |
| file_reads | `oltp_range_select` | 27.27ms | 24.20ms | 0.887× | 1.84% | PASS |
| file_reads | `oltp_sum_range` | 26.22ms | 24.11ms | 0.919× | 1.29% | PASS |
| file_reads | `oltp_order_range` | 4.50ms | 4.25ms | 0.945× | 2.41% | PASS |
| file_reads | `oltp_distinct_range` | 5.66ms | 5.32ms | 0.941× | 1.52% | PASS |
| file_reads | `oltp_index_scan` | 12.37ms | 9.20ms | 0.744× | 1.22% | PASS |
| file_reads | `select_random_points` | 38.34ms | 36.22ms | 0.945× | 1.84% | PASS |
| file_reads | `select_random_ranges` | 15.80ms | 12.04ms | 0.762× | 1.19% | PASS |
| file_reads | `covering_index_scan` | 11.98ms | 7.20ms | 0.601× | 1.62% | PASS |
| file_reads | `groupby_scan` | 37.44ms | 39.37ms | 1.051× | 1.30% | PASS |
| file_reads | `index_join` | 12.69ms | 12.86ms | 1.014× | 1.39% | PASS |
| file_reads | `index_join_scan` | 5.27ms | 5.94ms | 1.128× | 2.29% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.23s | 1.185× | 0.57% | PASS |
| file_reads | `table_scan` | 1.22s | 1.39s | 1.141× | 0.91% | PASS |
| file_reads | `oltp_read_only` | 267.13ms | 210.24ms | 0.787× | 1.20% | PASS |
| file_writes | `oltp_bulk_insert` | 264.03ms | 380.11ms | 1.440× | 1.15% | PASS |
| file_writes | `oltp_insert` | 26.30ms | 46.61ms | 1.772× | 1.46% | PASS |
| file_writes | `oltp_update_index` | 98.49ms | 143.57ms | 1.458× | 1.28% | PASS |
| file_writes | `oltp_update_non_index` | 77.84ms | 105.23ms | 1.352× | 1.43% | PASS |
| file_writes | `oltp_delete_insert` | 78.42ms | 120.28ms | 1.534× | 1.55% | PASS |
| file_writes | `oltp_write_only` | 52.10ms | 78.16ms | 1.500× | 2.36% | PASS |
| file_writes | `types_delete_insert` | 50.82ms | 69.38ms | 1.365× | 2.09% | PASS |
| file_writes | `oltp_read_write` | 132.03ms | 176.92ms | 1.340× | 1.21% | PASS |
| ac_reads | `oltp_point_select` | 58.79ms | 66.96ms | 1.139× | 1.15% | PASS |
| ac_reads | `oltp_range_select` | 22.51ms | 24.19ms | 1.075× | 1.74% | PASS |
| ac_reads | `oltp_sum_range` | 21.03ms | 24.07ms | 1.145× | 1.17% | PASS |
| ac_reads | `oltp_order_range` | 3.99ms | 4.25ms | 1.065× | 1.10% | PASS |
| ac_reads | `oltp_distinct_range` | 5.11ms | 5.34ms | 1.044× | 1.11% | PASS |
| ac_reads | `oltp_index_scan` | 7.43ms | 9.18ms | 1.235× | 1.81% | PASS |
| ac_reads | `select_random_points` | 31.96ms | 36.29ms | 1.135× | 1.41% | PASS |
| ac_reads | `select_random_ranges` | 10.60ms | 12.03ms | 1.135× | 1.27% | PASS |
| ac_reads | `covering_index_scan` | 7.06ms | 7.22ms | 1.022× | 1.85% | PASS |
| ac_reads | `groupby_scan` | 36.84ms | 39.34ms | 1.068× | 0.83% | PASS |
| ac_reads | `index_join` | 10.06ms | 12.84ms | 1.276× | 1.73% | PASS |
| ac_reads | `index_join_scan` | 4.74ms | 5.94ms | 1.254× | 1.34% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.23s | 1.180× | 0.82% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.39s | 1.160× | 0.79% | PASS |
| ac_reads | `oltp_read_only` | 191.13ms | 209.63ms | 1.097× | 1.10% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.02ms | 80.38ms | 3.651× | 4.68% | PASS |
| ac_writes | `oltp_insert_ac` | 23.99ms | 100.30ms | 4.181× | 5.99% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.97ms | 113.89ms | 4.071× | 6.35% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.31ms | 94.17ms | 4.040× | 5.74% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.93ms | 105.91ms | 4.249× | 5.95% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.94ms | 103.54ms | 4.152× | 5.91% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.15ms | 96.23ms | 4.345× | 5.34% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.02ms | 116.21ms | 3.520× | 4.62% | PASS |

</details>

## Version-control latency

Wall time: 1m 43s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 48.58ms | 200.00ms | 24.3% | 3.89% | PASS |
| `status_dirty_many_tables` | 48.82ms | 200.00ms | 24.4% | 0.63% | PASS |
| `diff_regular_working_one_table` | 44.33ms | 150.00ms | 29.6% | 0.76% | PASS |
| `diff_regular_working_many_tables` | 52.19ms | 200.00ms | 26.1% | 0.79% | PASS |
| `diff_stat_working_many_tables` | 52.40ms | 200.00ms | 26.2% | 0.57% | PASS |
| `diff_schema_working_many_tables` | 52.20ms | 200.00ms | 26.1% | 0.50% | PASS |
| `branch_list_many_branches` | 14.38ms | 100.00ms | 14.4% | 0.83% | PASS |
| `branch_create_delete` | 29.63ms | 100.00ms | 29.6% | 43.95% | PASS |
| `checkout_branch_clean` | 88.78ms | 200.00ms | 44.4% | 22.90% | PASS |
| `merge_data_no_conflicts` | 26.50ms | 150.00ms | 17.7% | 10.29% | PASS |
| `merge_schema_no_conflicts` | 15.77ms | 100.00ms | 15.8% | 3.83% | PASS |
| `merge_data_conflicts` | 63.35ms | 250.00ms | 25.3% | 1.43% | PASS |
| `merge_data_conflicts_with_resolve` | 55.28ms | 250.00ms | 22.1% | 1.09% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
