# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-13 15:28 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260907.300.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/34761040977)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 30s | 8.84s | 11.53s | 1.304× | 1.32% | **PASS** |
| textpk | 69 | 55 | 1h 33m 56s | 10.62s | 11.95s | 1.125× | 1.33% | **PASS** |
| blobpk | 69 | 55 | 1h 31m 22s | 9.76s | 11.89s | 1.219× | 1.79% | **PASS** |
| compositepk | 69 | 55 | 1h 25m 38s | 10.08s | 11.70s | 1.161× | 1.08% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.74ms | 29.52ms | 1.244× | 1.26% | PASS |
| mem_reads | `oltp_range_select` | 10.07ms | 13.12ms | 1.303× | 2.11% | PASS |
| mem_reads | `oltp_sum_range` | 9.63ms | 12.34ms | 1.282× | 1.68% | PASS |
| mem_reads | `oltp_order_range` | 2.54ms | 2.99ms | 1.177× | 1.29% | PASS |
| mem_reads | `oltp_distinct_range` | 3.62ms | 4.01ms | 1.107× | 0.90% | PASS |
| mem_reads | `oltp_index_scan` | 3.88ms | 5.28ms | 1.360× | 1.35% | PASS |
| mem_reads | `select_random_points` | 9.77ms | 11.10ms | 1.136× | 1.68% | PASS |
| mem_reads | `select_random_ranges` | 2.96ms | 3.98ms | 1.343× | 1.62% | PASS |
| mem_reads | `covering_index_scan` | 4.24ms | 4.16ms | 0.981× | 1.65% | PASS |
| mem_reads | `groupby_scan` | 30.32ms | 32.65ms | 1.077× | 0.79% | PASS |
| mem_reads | `index_join` | 6.10ms | 8.07ms | 1.324× | 1.18% | PASS |
| mem_reads | `index_join_scan` | 3.47ms | 4.60ms | 1.328× | 1.47% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.33s | 1.285× | 0.58% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.39s | 1.190× | 0.90% | PASS |
| mem_reads | `oltp_read_only` | 102.23ms | 123.21ms | 1.205× | 0.90% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.81ms | 250.80ms | 1.379× | 1.21% | PASS |
| mem_writes | `oltp_insert` | 15.48ms | 28.44ms | 1.837× | 1.21% | PASS |
| mem_writes | `oltp_update_index` | 49.83ms | 104.65ms | 2.100× | 0.97% | PASS |
| mem_writes | `oltp_update_non_index` | 33.67ms | 59.34ms | 1.762× | 1.49% | PASS |
| mem_writes | `oltp_delete_insert` | 44.42ms | 78.70ms | 1.772× | 1.28% | PASS |
| mem_writes | `oltp_write_only` | 21.73ms | 45.14ms | 2.077× | 1.09% | PASS |
| mem_writes | `types_delete_insert` | 24.43ms | 40.46ms | 1.656× | 1.04% | PASS |
| mem_writes | `oltp_read_write` | 67.05ms | 110.55ms | 1.649× | 0.87% | PASS |
| file_reads | `oltp_point_select` | 98.41ms | 55.14ms | 0.560× | 0.65% | PASS |
| file_reads | `oltp_range_select` | 17.97ms | 15.83ms | 0.881× | 1.78% | PASS |
| file_reads | `oltp_sum_range` | 17.56ms | 15.21ms | 0.866× | 1.32% | PASS |
| file_reads | `oltp_order_range` | 3.48ms | 3.36ms | 0.967× | 2.22% | PASS |
| file_reads | `oltp_distinct_range` | 4.54ms | 4.39ms | 0.966× | 1.60% | PASS |
| file_reads | `oltp_index_scan` | 11.88ms | 8.31ms | 0.700× | 1.01% | PASS |
| file_reads | `select_random_points` | 18.05ms | 13.98ms | 0.774× | 1.29% | PASS |
| file_reads | `select_random_ranges` | 10.60ms | 6.61ms | 0.624× | 0.81% | PASS |
| file_reads | `covering_index_scan` | 12.19ms | 7.07ms | 0.580× | 1.55% | PASS |
| file_reads | `groupby_scan` | 31.34ms | 33.42ms | 1.067× | 1.08% | PASS |
| file_reads | `index_join` | 10.43ms | 10.13ms | 0.972× | 1.70% | PASS |
| file_reads | `index_join_scan` | 4.46ms | 4.96ms | 1.112× | 1.01% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.33s | 1.274× | 0.65% | PASS |
| file_reads | `table_scan` | 1.18s | 1.40s | 1.182× | 0.92% | PASS |
| file_reads | `oltp_read_only` | 211.33ms | 161.43ms | 0.764× | 0.92% | PASS |
| file_writes | `oltp_bulk_insert` | 194.18ms | 269.95ms | 1.390× | 1.04% | PASS |
| file_writes | `oltp_insert` | 22.34ms | 36.04ms | 1.613× | 1.76% | PASS |
| file_writes | `oltp_update_index` | 77.33ms | 129.09ms | 1.669× | 1.67% | PASS |
| file_writes | `oltp_update_non_index` | 57.79ms | 81.19ms | 1.405× | 1.66% | PASS |
| file_writes | `oltp_delete_insert` | 68.21ms | 99.61ms | 1.460× | 1.49% | PASS |
| file_writes | `oltp_write_only` | 44.99ms | 64.73ms | 1.439× | 1.86% | PASS |
| file_writes | `types_delete_insert` | 40.32ms | 53.69ms | 1.332× | 1.62% | PASS |
| file_writes | `oltp_read_write` | 91.53ms | 130.32ms | 1.424× | 1.60% | PASS |
| ac_reads | `oltp_point_select` | 48.24ms | 55.24ms | 1.145× | 0.75% | PASS |
| ac_reads | `oltp_range_select` | 13.02ms | 15.83ms | 1.216× | 0.89% | PASS |
| ac_reads | `oltp_sum_range` | 12.31ms | 15.14ms | 1.230× | 1.07% | PASS |
| ac_reads | `oltp_order_range` | 2.97ms | 3.34ms | 1.124× | 2.54% | PASS |
| ac_reads | `oltp_distinct_range` | 4.02ms | 4.47ms | 1.113× | 1.36% | PASS |
| ac_reads | `oltp_index_scan` | 6.64ms | 8.30ms | 1.251× | 1.10% | PASS |
| ac_reads | `select_random_points` | 12.87ms | 13.98ms | 1.087× | 1.35% | PASS |
| ac_reads | `select_random_ranges` | 5.53ms | 6.61ms | 1.194× | 1.14% | PASS |
| ac_reads | `covering_index_scan` | 7.05ms | 7.03ms | 0.996× | 1.78% | PASS |
| ac_reads | `groupby_scan` | 30.05ms | 33.19ms | 1.104× | 0.98% | PASS |
| ac_reads | `index_join` | 7.75ms | 10.18ms | 1.313× | 1.67% | PASS |
| ac_reads | `index_join_scan` | 3.87ms | 4.98ms | 1.288× | 1.94% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.32s | 1.277× | 0.37% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.40s | 1.193× | 0.67% | PASS |
| ac_reads | `oltp_read_only` | 139.09ms | 161.59ms | 1.162× | 0.76% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.86ms | 86.44ms | 3.623× | 8.98% | PASS |
| ac_writes | `oltp_insert_ac` | 26.45ms | 102.90ms | 3.890× | 6.47% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.98ms | 121.74ms | 4.351× | 6.27% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.26ms | 98.73ms | 3.909× | 8.07% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.44ms | 110.95ms | 4.197× | 7.32% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.84ms | 108.08ms | 4.027× | 7.47% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.25ms | 101.30ms | 4.177× | 8.52% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.40ms | 115.14ms | 3.666× | 4.94% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 27.78ms | 31.43ms | 1.131× | 1.33% | PASS |
| mem_reads | `oltp_range_select` | 13.01ms | 13.11ms | 1.008× | 1.04% | PASS |
| mem_reads | `oltp_sum_range` | 12.23ms | 12.42ms | 1.016× | 1.20% | PASS |
| mem_reads | `oltp_order_range` | 2.90ms | 3.05ms | 1.050× | 0.98% | PASS |
| mem_reads | `oltp_distinct_range` | 3.82ms | 3.95ms | 1.033× | 0.82% | PASS |
| mem_reads | `oltp_index_scan` | 4.24ms | 5.49ms | 1.295× | 1.30% | PASS |
| mem_reads | `select_random_points` | 17.38ms | 20.07ms | 1.155× | 1.42% | PASS |
| mem_reads | `select_random_ranges` | 3.55ms | 4.72ms | 1.327× | 0.88% | PASS |
| mem_reads | `covering_index_scan` | 4.02ms | 4.20ms | 1.043× | 1.95% | PASS |
| mem_reads | `groupby_scan` | 33.02ms | 32.87ms | 0.995× | 0.46% | PASS |
| mem_reads | `index_join` | 6.82ms | 9.57ms | 1.404× | 1.93% | PASS |
| mem_reads | `index_join_scan` | 4.46ms | 5.47ms | 1.227× | 1.70% | PASS |
| mem_reads | `types_table_scan` | 1.17s | 1.27s | 1.082× | 2.11% | PASS |
| mem_reads | `table_scan` | 1.44s | 1.37s | 0.954× | 2.30% | PASS |
| mem_reads | `oltp_read_only` | 118.87ms | 125.40ms | 1.055× | 2.23% | PASS |
| mem_writes | `oltp_bulk_insert` | 188.92ms | 282.62ms | 1.496× | 0.55% | PASS |
| mem_writes | `oltp_insert` | 19.30ms | 33.62ms | 1.742× | 0.98% | PASS |
| mem_writes | `oltp_update_index` | 68.35ms | 130.97ms | 1.916× | 1.27% | PASS |
| mem_writes | `oltp_update_non_index` | 44.82ms | 76.77ms | 1.713× | 1.32% | PASS |
| mem_writes | `oltp_delete_insert` | 46.66ms | 94.47ms | 2.025× | 1.33% | PASS |
| mem_writes | `oltp_write_only` | 25.70ms | 55.24ms | 2.149× | 1.19% | PASS |
| mem_writes | `types_delete_insert` | 28.41ms | 47.34ms | 1.666× | 0.88% | PASS |
| mem_writes | `oltp_read_write` | 78.20ms | 123.88ms | 1.584× | 1.70% | PASS |
| file_reads | `oltp_point_select` | 61.10ms | 43.60ms | 0.714× | 0.80% | PASS |
| file_reads | `oltp_range_select` | 17.52ms | 14.80ms | 0.845× | 1.64% | PASS |
| file_reads | `oltp_sum_range` | 16.35ms | 14.03ms | 0.858× | 1.04% | PASS |
| file_reads | `oltp_order_range` | 3.43ms | 3.25ms | 0.947× | 1.67% | PASS |
| file_reads | `oltp_distinct_range` | 4.38ms | 4.17ms | 0.953× | 0.68% | PASS |
| file_reads | `oltp_index_scan` | 8.29ms | 7.05ms | 0.850× | 0.84% | PASS |
| file_reads | `select_random_points` | 22.26ms | 22.09ms | 0.992× | 1.63% | PASS |
| file_reads | `select_random_ranges` | 7.40ms | 6.03ms | 0.816× | 0.72% | PASS |
| file_reads | `covering_index_scan` | 8.88ms | 5.55ms | 0.625× | 1.41% | PASS |
| file_reads | `groupby_scan` | 33.43ms | 32.88ms | 0.983× | 0.34% | PASS |
| file_reads | `index_join` | 9.66ms | 10.59ms | 1.096× | 1.02% | PASS |
| file_reads | `index_join_scan` | 5.00ms | 5.80ms | 1.158× | 1.20% | PASS |
| file_reads | `types_table_scan` | 1.12s | 1.25s | 1.112× | 1.55% | PASS |
| file_reads | `table_scan` | 1.41s | 1.37s | 0.971× | 2.71% | PASS |
| file_reads | `oltp_read_only` | 166.73ms | 142.67ms | 0.856× | 1.06% | PASS |
| file_writes | `oltp_bulk_insert` | 261.66ms | 371.25ms | 1.419× | 1.85% | PASS |
| file_writes | `oltp_insert` | 80.20ms | 63.51ms | 0.792× | 5.87% | PASS |
| file_writes | `oltp_update_index` | 228.34ms | 232.98ms | 1.020× | 1.95% | PASS |
| file_writes | `oltp_update_non_index` | 170.62ms | 152.28ms | 0.893× | 1.75% | PASS |
| file_writes | `oltp_delete_insert` | 186.52ms | 184.19ms | 0.987× | 2.48% | PASS |
| file_writes | `oltp_write_only` | 143.63ms | 120.02ms | 0.836× | 3.03% | PASS |
| file_writes | `types_delete_insert` | 104.16ms | 103.06ms | 0.989× | 6.45% | PASS |
| file_writes | `oltp_read_write` | 195.38ms | 189.29ms | 0.969× | 1.91% | PASS |
| ac_reads | `oltp_point_select` | 37.80ms | 43.14ms | 1.141× | 1.37% | PASS |
| ac_reads | `oltp_range_select` | 14.44ms | 14.70ms | 1.018× | 1.01% | PASS |
| ac_reads | `oltp_sum_range` | 13.83ms | 13.95ms | 1.009× | 1.18% | PASS |
| ac_reads | `oltp_order_range` | 3.17ms | 3.24ms | 1.023× | 1.08% | PASS |
| ac_reads | `oltp_distinct_range` | 4.10ms | 4.15ms | 1.012× | 1.03% | PASS |
| ac_reads | `oltp_index_scan` | 6.00ms | 7.00ms | 1.167× | 1.00% | PASS |
| ac_reads | `select_random_points` | 19.67ms | 21.68ms | 1.102× | 1.21% | PASS |
| ac_reads | `select_random_ranges` | 5.12ms | 6.01ms | 1.173× | 0.91% | PASS |
| ac_reads | `covering_index_scan` | 6.55ms | 5.53ms | 0.845× | 1.41% | PASS |
| ac_reads | `groupby_scan` | 33.10ms | 32.88ms | 0.993× | 0.35% | PASS |
| ac_reads | `index_join` | 8.58ms | 10.57ms | 1.232× | 0.79% | PASS |
| ac_reads | `index_join_scan` | 4.82ms | 5.76ms | 1.194× | 1.61% | PASS |
| ac_reads | `types_table_scan` | 1.10s | 1.24s | 1.130× | 0.63% | PASS |
| ac_reads | `table_scan` | 1.33s | 1.36s | 1.020× | 1.17% | PASS |
| ac_reads | `oltp_read_only` | 129.89ms | 140.65ms | 1.083× | 0.78% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 29.61ms | 92.44ms | 3.122× | 6.67% | PASS |
| ac_writes | `oltp_insert_ac` | 35.45ms | 115.52ms | 3.259× | 11.15% | PASS |
| ac_writes | `oltp_update_index_ac` | 36.24ms | 131.60ms | 3.631× | 10.50% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 29.49ms | 104.67ms | 3.549× | 6.58% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 33.58ms | 117.39ms | 3.496× | 8.88% | PASS |
| ac_writes | `oltp_write_only_ac` | 34.38ms | 119.78ms | 3.484× | 8.48% | PASS |
| ac_writes | `types_delete_insert_ac` | 27.92ms | 120.30ms | 4.308× | 16.04% | PASS |
| ac_writes | `oltp_read_write_ac` | 38.94ms | 127.24ms | 3.268× | 8.08% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.06ms | 37.08ms | 1.194× | 2.82% | PASS |
| mem_reads | `oltp_range_select` | 13.41ms | 14.01ms | 1.045× | 3.01% | PASS |
| mem_reads | `oltp_sum_range` | 12.06ms | 13.91ms | 1.153× | 2.09% | PASS |
| mem_reads | `oltp_order_range` | 2.91ms | 3.27ms | 1.122× | 1.51% | PASS |
| mem_reads | `oltp_distinct_range` | 4.00ms | 4.33ms | 1.083× | 0.86% | PASS |
| mem_reads | `oltp_index_scan` | 4.61ms | 6.21ms | 1.348× | 1.79% | PASS |
| mem_reads | `select_random_points` | 18.36ms | 20.78ms | 1.132× | 3.09% | PASS |
| mem_reads | `select_random_ranges` | 4.03ms | 5.14ms | 1.277× | 1.19% | PASS |
| mem_reads | `covering_index_scan` | 4.51ms | 4.68ms | 1.038× | 1.67% | PASS |
| mem_reads | `groupby_scan` | 31.89ms | 33.76ms | 1.059× | 1.07% | PASS |
| mem_reads | `index_join` | 6.80ms | 9.19ms | 1.352× | 1.89% | PASS |
| mem_reads | `index_join_scan` | 4.38ms | 5.27ms | 1.204× | 3.14% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.24s | 1.166× | 1.90% | PASS |
| mem_reads | `table_scan` | 1.39s | 1.41s | 1.014× | 1.34% | PASS |
| mem_reads | `oltp_read_only` | 122.29ms | 139.31ms | 1.139× | 1.20% | PASS |
| mem_writes | `oltp_bulk_insert` | 242.25ms | 354.07ms | 1.462× | 0.87% | PASS |
| mem_writes | `oltp_insert` | 19.89ms | 39.57ms | 1.989× | 0.70% | PASS |
| mem_writes | `oltp_update_index` | 68.26ms | 130.69ms | 1.914× | 1.12% | PASS |
| mem_writes | `oltp_update_non_index` | 49.27ms | 85.21ms | 1.730× | 1.25% | PASS |
| mem_writes | `oltp_delete_insert` | 49.39ms | 104.11ms | 2.108× | 1.55% | PASS |
| mem_writes | `oltp_write_only` | 28.59ms | 62.98ms | 2.203× | 1.88% | PASS |
| mem_writes | `types_delete_insert` | 33.77ms | 55.74ms | 1.651× | 2.49% | PASS |
| mem_writes | `oltp_read_write` | 84.25ms | 139.69ms | 1.658× | 1.82% | PASS |
| file_reads | `oltp_point_select` | 107.72ms | 63.02ms | 0.585× | 1.15% | PASS |
| file_reads | `oltp_range_select` | 21.82ms | 16.83ms | 0.771× | 1.82% | PASS |
| file_reads | `oltp_sum_range` | 20.55ms | 16.89ms | 0.822× | 1.84% | PASS |
| file_reads | `oltp_order_range` | 3.84ms | 3.63ms | 0.944× | 2.41% | PASS |
| file_reads | `oltp_distinct_range` | 4.89ms | 4.69ms | 0.959× | 1.15% | PASS |
| file_reads | `oltp_index_scan` | 12.51ms | 9.13ms | 0.730× | 2.02% | PASS |
| file_reads | `select_random_points` | 27.63ms | 24.16ms | 0.875× | 1.82% | PASS |
| file_reads | `select_random_ranges` | 11.80ms | 7.82ms | 0.663× | 1.22% | PASS |
| file_reads | `covering_index_scan` | 12.48ms | 7.34ms | 0.589× | 2.45% | PASS |
| file_reads | `groupby_scan` | 33.05ms | 34.20ms | 1.035× | 1.14% | PASS |
| file_reads | `index_join` | 11.32ms | 11.21ms | 0.990× | 1.57% | PASS |
| file_reads | `index_join_scan` | 5.47ms | 5.77ms | 1.056× | 3.18% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.23s | 1.177× | 0.99% | PASS |
| file_reads | `table_scan` | 1.29s | 1.38s | 1.076× | 4.01% | PASS |
| file_reads | `oltp_read_only` | 236.21ms | 177.88ms | 0.753× | 1.30% | PASS |
| file_writes | `oltp_bulk_insert` | 261.00ms | 379.09ms | 1.452× | 1.24% | PASS |
| file_writes | `oltp_insert` | 32.25ms | 52.76ms | 1.636× | 2.10% | PASS |
| file_writes | `oltp_update_index` | 104.22ms | 163.56ms | 1.569× | 1.43% | PASS |
| file_writes | `oltp_update_non_index` | 81.38ms | 110.07ms | 1.353× | 1.48% | PASS |
| file_writes | `oltp_delete_insert` | 84.67ms | 133.53ms | 1.577× | 1.07% | PASS |
| file_writes | `oltp_write_only` | 58.38ms | 86.25ms | 1.477× | 1.69% | PASS |
| file_writes | `types_delete_insert` | 53.27ms | 73.32ms | 1.376× | 1.60% | PASS |
| file_writes | `oltp_read_write` | 116.72ms | 162.53ms | 1.392× | 1.11% | PASS |
| ac_reads | `oltp_point_select` | 56.49ms | 63.19ms | 1.119× | 1.11% | PASS |
| ac_reads | `oltp_range_select` | 16.72ms | 16.92ms | 1.012× | 2.39% | PASS |
| ac_reads | `oltp_sum_range` | 15.46ms | 16.89ms | 1.093× | 1.97% | PASS |
| ac_reads | `oltp_order_range` | 3.33ms | 3.61ms | 1.085× | 1.37% | PASS |
| ac_reads | `oltp_distinct_range` | 4.42ms | 4.70ms | 1.064× | 0.88% | PASS |
| ac_reads | `oltp_index_scan` | 7.60ms | 9.15ms | 1.204× | 1.13% | PASS |
| ac_reads | `select_random_points` | 21.71ms | 24.25ms | 1.117× | 2.40% | PASS |
| ac_reads | `select_random_ranges` | 6.90ms | 7.83ms | 1.135× | 1.19% | PASS |
| ac_reads | `covering_index_scan` | 7.75ms | 7.35ms | 0.948× | 2.74% | PASS |
| ac_reads | `groupby_scan` | 32.51ms | 34.22ms | 1.053× | 0.98% | PASS |
| ac_reads | `index_join` | 9.18ms | 11.22ms | 1.223× | 2.45% | PASS |
| ac_reads | `index_join_scan` | 4.91ms | 5.77ms | 1.175× | 2.59% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.23s | 1.175× | 1.11% | PASS |
| ac_reads | `table_scan` | 1.24s | 1.38s | 1.108× | 2.32% | PASS |
| ac_reads | `oltp_read_only` | 159.79ms | 178.07ms | 1.114× | 1.07% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.38ms | 83.76ms | 3.742× | 6.34% | PASS |
| ac_writes | `oltp_insert_ac` | 25.32ms | 106.08ms | 4.189× | 7.29% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.61ms | 116.23ms | 4.209× | 5.70% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.00ms | 95.78ms | 3.991× | 5.06% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.17ms | 109.55ms | 4.353× | 5.33% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.30ms | 109.45ms | 4.161× | 6.47% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.37ms | 101.99ms | 4.560× | 7.30% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.06ms | 116.06ms | 3.620× | 5.37% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.46ms | 37.43ms | 1.118× | 1.39% | PASS |
| mem_reads | `oltp_range_select` | 19.71ms | 19.96ms | 1.012× | 0.79% | PASS |
| mem_reads | `oltp_sum_range` | 18.29ms | 19.06ms | 1.042× | 1.00% | PASS |
| mem_reads | `oltp_order_range` | 3.73ms | 3.79ms | 1.015× | 0.71% | PASS |
| mem_reads | `oltp_distinct_range` | 4.77ms | 4.84ms | 1.014× | 0.88% | PASS |
| mem_reads | `oltp_index_scan` | 4.68ms | 5.74ms | 1.225× | 1.82% | PASS |
| mem_reads | `select_random_points` | 27.53ms | 30.18ms | 1.096× | 1.22% | PASS |
| mem_reads | `select_random_ranges` | 7.66ms | 8.24ms | 1.077× | 0.95% | PASS |
| mem_reads | `covering_index_scan` | 4.39ms | 4.21ms | 0.958× | 1.59% | PASS |
| mem_reads | `groupby_scan` | 38.72ms | 39.91ms | 1.031× | 0.69% | PASS |
| mem_reads | `index_join` | 8.15ms | 9.93ms | 1.219× | 0.81% | PASS |
| mem_reads | `index_join_scan` | 4.31ms | 5.61ms | 1.304× | 1.69% | PASS |
| mem_reads | `types_table_scan` | 1.12s | 1.25s | 1.111× | 0.56% | PASS |
| mem_reads | `table_scan` | 1.29s | 1.36s | 1.055× | 0.47% | PASS |
| mem_reads | `oltp_read_only` | 152.00ms | 159.31ms | 1.048× | 0.92% | PASS |
| mem_writes | `oltp_bulk_insert` | 246.15ms | 338.03ms | 1.373× | 0.83% | PASS |
| mem_writes | `oltp_insert` | 19.48ms | 35.86ms | 1.841× | 0.63% | PASS |
| mem_writes | `oltp_update_index` | 68.44ms | 118.93ms | 1.738× | 0.77% | PASS |
| mem_writes | `oltp_update_non_index` | 51.30ms | 83.44ms | 1.626× | 0.71% | PASS |
| mem_writes | `oltp_delete_insert` | 49.73ms | 95.56ms | 1.922× | 0.87% | PASS |
| mem_writes | `oltp_write_only` | 27.35ms | 59.65ms | 2.181× | 0.75% | PASS |
| mem_writes | `types_delete_insert` | 33.21ms | 54.51ms | 1.641× | 1.16% | PASS |
| mem_writes | `oltp_read_write` | 98.74ms | 147.38ms | 1.493× | 1.21% | PASS |
| file_reads | `oltp_point_select` | 128.69ms | 69.62ms | 0.541× | 1.05% | PASS |
| file_reads | `oltp_range_select` | 28.75ms | 23.30ms | 0.810× | 2.04% | PASS |
| file_reads | `oltp_sum_range` | 27.93ms | 22.55ms | 0.807× | 1.79% | PASS |
| file_reads | `oltp_order_range` | 4.51ms | 4.15ms | 0.921× | 1.42% | PASS |
| file_reads | `oltp_distinct_range` | 5.64ms | 5.23ms | 0.927× | 2.03% | PASS |
| file_reads | `oltp_index_scan` | 14.10ms | 9.29ms | 0.659× | 1.65% | PASS |
| file_reads | `select_random_points` | 36.73ms | 33.67ms | 0.917× | 2.41% | PASS |
| file_reads | `select_random_ranges` | 17.31ms | 11.60ms | 0.670× | 1.42% | PASS |
| file_reads | `covering_index_scan` | 14.03ms | 7.66ms | 0.546× | 1.67% | PASS |
| file_reads | `groupby_scan` | 39.58ms | 40.64ms | 1.027× | 0.79% | PASS |
| file_reads | `index_join` | 13.18ms | 12.53ms | 0.950× | 1.34% | PASS |
| file_reads | `index_join_scan` | 5.26ms | 6.21ms | 1.179× | 1.77% | PASS |
| file_reads | `types_table_scan` | 1.11s | 1.24s | 1.113× | 0.59% | PASS |
| file_reads | `table_scan` | 1.28s | 1.35s | 1.052× | 0.57% | PASS |
| file_reads | `oltp_read_only` | 287.97ms | 205.39ms | 0.713× | 0.56% | PASS |
| file_writes | `oltp_bulk_insert` | 262.65ms | 359.93ms | 1.370× | 1.01% | PASS |
| file_writes | `oltp_insert` | 25.82ms | 46.55ms | 1.803× | 1.07% | PASS |
| file_writes | `oltp_update_index` | 97.99ms | 145.62ms | 1.486× | 1.53% | PASS |
| file_writes | `oltp_update_non_index` | 77.59ms | 106.25ms | 1.369× | 1.76% | PASS |
| file_writes | `oltp_delete_insert` | 76.59ms | 120.62ms | 1.575× | 1.32% | PASS |
| file_writes | `oltp_write_only` | 49.96ms | 79.85ms | 1.598× | 2.03% | PASS |
| file_writes | `types_delete_insert` | 49.30ms | 67.06ms | 1.360× | 1.30% | PASS |
| file_writes | `oltp_read_write` | 121.95ms | 167.98ms | 1.377× | 1.08% | PASS |
| ac_reads | `oltp_point_select` | 64.22ms | 69.56ms | 1.083× | 1.21% | PASS |
| ac_reads | `oltp_range_select` | 23.16ms | 23.34ms | 1.008× | 0.94% | PASS |
| ac_reads | `oltp_sum_range` | 21.48ms | 22.53ms | 1.049× | 0.96% | PASS |
| ac_reads | `oltp_order_range` | 4.13ms | 4.16ms | 1.007× | 0.94% | PASS |
| ac_reads | `oltp_distinct_range` | 5.17ms | 5.25ms | 1.015× | 0.99% | PASS |
| ac_reads | `oltp_index_scan` | 8.05ms | 9.34ms | 1.160× | 1.14% | PASS |
| ac_reads | `select_random_points` | 30.96ms | 33.80ms | 1.092× | 0.93% | PASS |
| ac_reads | `select_random_ranges` | 10.84ms | 11.61ms | 1.072× | 1.15% | PASS |
| ac_reads | `covering_index_scan` | 7.80ms | 7.71ms | 0.988× | 1.02% | PASS |
| ac_reads | `groupby_scan` | 38.96ms | 40.64ms | 1.043× | 0.59% | PASS |
| ac_reads | `index_join` | 10.12ms | 12.58ms | 1.243× | 0.76% | PASS |
| ac_reads | `index_join_scan` | 4.77ms | 6.24ms | 1.307× | 1.45% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.24s | 1.114× | 0.40% | PASS |
| ac_reads | `table_scan` | 1.29s | 1.35s | 1.051× | 0.39% | PASS |
| ac_reads | `oltp_read_only` | 195.56ms | 205.50ms | 1.051× | 0.64% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.47ms | 60.52ms | 3.912× | 2.53% | PASS |
| ac_writes | `oltp_insert_ac` | 17.85ms | 82.73ms | 4.635× | 3.20% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.30ms | 93.51ms | 4.846× | 3.06% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 15.82ms | 72.12ms | 4.559× | 3.26% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.17ms | 83.88ms | 4.884× | 2.15% | PASS |
| ac_writes | `oltp_write_only_ac` | 17.62ms | 83.45ms | 4.735× | 3.42% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.33ms | 71.83ms | 4.686× | 3.28% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.78ms | 92.93ms | 3.750× | 1.97% | PASS |

</details>

## Version-control latency

Wall time: 2m 18s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 82.68ms | 200.00ms | 41.3% | 0.50% | PASS |
| `status_dirty_many_tables` | 85.87ms | 200.00ms | 42.9% | 0.62% | PASS |
| `diff_regular_working_one_table` | 78.49ms | 150.00ms | 52.3% | 0.55% | PASS |
| `diff_regular_working_many_tables` | 91.30ms | 200.00ms | 45.7% | 0.67% | PASS |
| `diff_stat_working_many_tables` | 91.16ms | 200.00ms | 45.6% | 0.53% | PASS |
| `diff_schema_working_many_tables` | 91.68ms | 200.00ms | 45.8% | 0.51% | PASS |
| `branch_list_many_branches` | 22.79ms | 100.00ms | 22.8% | 1.07% | PASS |
| `branch_create_delete` | 25.05ms | 100.00ms | 25.1% | 1.66% | PASS |
| `checkout_branch_clean` | 55.27ms | 200.00ms | 27.6% | 0.99% | PASS |
| `merge_data_no_conflicts` | 28.76ms | 150.00ms | 19.2% | 0.98% | PASS |
| `merge_schema_no_conflicts` | 22.30ms | 100.00ms | 22.3% | 1.70% | PASS |
| `merge_data_conflicts` | 126.84ms | 250.00ms | 50.7% | 0.42% | PASS |
| `merge_data_conflicts_with_resolve` | 126.88ms | 250.00ms | 50.8% | 0.53% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
