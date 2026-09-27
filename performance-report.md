# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-27 16:26 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260920.314.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/36327206817)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 27s | 8.91s | 11.41s | 1.281× | 1.43% | **PASS** |
| textpk | 69 | 55 | 1h 35m 56s | 10.81s | 12.09s | 1.118× | 1.88% | **PASS** |
| blobpk | 69 | 55 | 1h 7m 55s | 7.41s | 7.86s | 1.061× | 2.69% | **PASS** |
| compositepk | 69 | 55 | 1h 28m 45s | 10.13s | 12.13s | 1.198× | 1.50% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.09ms | 29.45ms | 1.223× | 1.96% | PASS |
| mem_reads | `oltp_range_select` | 10.29ms | 13.13ms | 1.276× | 1.60% | PASS |
| mem_reads | `oltp_sum_range` | 9.53ms | 12.34ms | 1.295× | 1.43% | PASS |
| mem_reads | `oltp_order_range` | 2.51ms | 2.97ms | 1.184× | 0.85% | PASS |
| mem_reads | `oltp_distinct_range` | 3.60ms | 4.03ms | 1.118× | 1.14% | PASS |
| mem_reads | `oltp_index_scan` | 3.89ms | 5.26ms | 1.352× | 1.00% | PASS |
| mem_reads | `select_random_points` | 9.81ms | 10.97ms | 1.118× | 1.72% | PASS |
| mem_reads | `select_random_ranges` | 2.97ms | 3.98ms | 1.340× | 1.93% | PASS |
| mem_reads | `covering_index_scan` | 4.26ms | 4.29ms | 1.007× | 2.41% | PASS |
| mem_reads | `groupby_scan` | 30.19ms | 33.43ms | 1.107× | 0.64% | PASS |
| mem_reads | `index_join` | 6.12ms | 8.46ms | 1.383× | 2.23% | PASS |
| mem_reads | `index_join_scan` | 3.56ms | 4.62ms | 1.298× | 3.27% | PASS |
| mem_reads | `types_table_scan` | 1.10s | 1.36s | 1.228× | 0.89% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.40s | 1.187× | 1.06% | PASS |
| mem_reads | `oltp_read_only` | 102.31ms | 123.33ms | 1.206× | 1.16% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.56ms | 252.93ms | 1.401× | 1.07% | PASS |
| mem_writes | `oltp_insert` | 15.48ms | 28.11ms | 1.816× | 1.19% | PASS |
| mem_writes | `oltp_update_index` | 50.55ms | 103.35ms | 2.044× | 1.17% | PASS |
| mem_writes | `oltp_update_non_index` | 35.61ms | 60.73ms | 1.705× | 1.90% | PASS |
| mem_writes | `oltp_delete_insert` | 46.95ms | 80.85ms | 1.722× | 1.51% | PASS |
| mem_writes | `oltp_write_only` | 22.18ms | 45.23ms | 2.039× | 1.87% | PASS |
| mem_writes | `types_delete_insert` | 25.40ms | 40.64ms | 1.600× | 1.28% | PASS |
| mem_writes | `oltp_read_write` | 69.69ms | 111.41ms | 1.599× | 2.33% | PASS |
| file_reads | `oltp_point_select` | 99.13ms | 55.18ms | 0.557× | 0.83% | PASS |
| file_reads | `oltp_range_select` | 18.07ms | 15.76ms | 0.872× | 1.60% | PASS |
| file_reads | `oltp_sum_range` | 17.55ms | 15.15ms | 0.863× | 1.27% | PASS |
| file_reads | `oltp_order_range` | 3.46ms | 3.32ms | 0.958× | 1.56% | PASS |
| file_reads | `oltp_distinct_range` | 4.48ms | 4.38ms | 0.978× | 1.72% | PASS |
| file_reads | `oltp_index_scan` | 11.65ms | 8.28ms | 0.711× | 1.32% | PASS |
| file_reads | `select_random_points` | 17.92ms | 13.76ms | 0.768× | 3.15% | PASS |
| file_reads | `select_random_ranges` | 10.48ms | 6.59ms | 0.629× | 1.01% | PASS |
| file_reads | `covering_index_scan` | 11.80ms | 7.04ms | 0.597× | 1.16% | PASS |
| file_reads | `groupby_scan` | 30.74ms | 33.73ms | 1.097× | 0.90% | PASS |
| file_reads | `index_join` | 10.21ms | 10.12ms | 0.992× | 1.37% | PASS |
| file_reads | `index_join_scan` | 4.43ms | 5.00ms | 1.127× | 1.57% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.32s | 1.281× | 0.40% | PASS |
| file_reads | `table_scan` | 1.17s | 1.39s | 1.188× | 0.62% | PASS |
| file_reads | `oltp_read_only` | 211.50ms | 161.57ms | 0.764× | 0.74% | PASS |
| file_writes | `oltp_bulk_insert` | 194.40ms | 271.38ms | 1.396× | 1.58% | PASS |
| file_writes | `oltp_insert` | 21.99ms | 35.54ms | 1.616× | 1.84% | PASS |
| file_writes | `oltp_update_index` | 77.23ms | 126.59ms | 1.639× | 1.45% | PASS |
| file_writes | `oltp_update_non_index` | 57.78ms | 80.92ms | 1.400× | 1.86% | PASS |
| file_writes | `oltp_delete_insert` | 67.69ms | 98.03ms | 1.448× | 1.31% | PASS |
| file_writes | `oltp_write_only` | 44.38ms | 64.08ms | 1.444× | 1.94% | PASS |
| file_writes | `types_delete_insert` | 40.46ms | 53.38ms | 1.319× | 1.88% | PASS |
| file_writes | `oltp_read_write` | 91.30ms | 129.18ms | 1.415× | 1.42% | PASS |
| ac_reads | `oltp_point_select` | 48.89ms | 55.30ms | 1.131× | 0.94% | PASS |
| ac_reads | `oltp_range_select` | 13.32ms | 15.76ms | 1.183× | 1.87% | PASS |
| ac_reads | `oltp_sum_range` | 12.40ms | 15.12ms | 1.219× | 1.23% | PASS |
| ac_reads | `oltp_order_range` | 2.90ms | 3.31ms | 1.142× | 1.27% | PASS |
| ac_reads | `oltp_distinct_range` | 3.96ms | 4.38ms | 1.105× | 1.17% | PASS |
| ac_reads | `oltp_index_scan` | 6.59ms | 8.28ms | 1.256× | 1.42% | PASS |
| ac_reads | `select_random_points` | 12.97ms | 13.79ms | 1.064× | 1.38% | PASS |
| ac_reads | `select_random_ranges` | 5.53ms | 6.60ms | 1.192× | 1.11% | PASS |
| ac_reads | `covering_index_scan` | 6.93ms | 7.07ms | 1.019× | 1.96% | PASS |
| ac_reads | `groupby_scan` | 30.04ms | 33.66ms | 1.120× | 0.73% | PASS |
| ac_reads | `index_join` | 7.64ms | 10.13ms | 1.325× | 1.40% | PASS |
| ac_reads | `index_join_scan` | 3.90ms | 4.94ms | 1.266× | 1.74% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.33s | 1.284× | 0.58% | PASS |
| ac_reads | `table_scan` | 1.19s | 1.40s | 1.174× | 1.91% | PASS |
| ac_reads | `oltp_read_only` | 141.04ms | 161.64ms | 1.146× | 0.97% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.38ms | 66.17ms | 3.095× | 5.49% | PASS |
| ac_writes | `oltp_insert_ac` | 24.13ms | 83.67ms | 3.467× | 7.14% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.71ms | 100.70ms | 3.769× | 5.38% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.33ms | 78.48ms | 3.515× | 6.47% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.34ms | 93.28ms | 3.832× | 7.22% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.86ms | 89.61ms | 3.605× | 8.64% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.43ms | 84.63ms | 3.948× | 8.81% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.10ms | 97.66ms | 3.356× | 5.69% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.10ms | 37.73ms | 1.253× | 1.55% | PASS |
| mem_reads | `oltp_range_select` | 13.64ms | 14.14ms | 1.037× | 2.70% | PASS |
| mem_reads | `oltp_sum_range` | 12.02ms | 14.15ms | 1.178× | 1.51% | PASS |
| mem_reads | `oltp_order_range` | 2.99ms | 3.17ms | 1.059× | 1.48% | PASS |
| mem_reads | `oltp_distinct_range` | 4.06ms | 4.23ms | 1.043× | 1.01% | PASS |
| mem_reads | `oltp_index_scan` | 4.49ms | 6.32ms | 1.408× | 1.18% | PASS |
| mem_reads | `select_random_points` | 17.85ms | 21.00ms | 1.177× | 2.24% | PASS |
| mem_reads | `select_random_ranges` | 3.99ms | 5.18ms | 1.301× | 1.47% | PASS |
| mem_reads | `covering_index_scan` | 4.54ms | 4.55ms | 1.002× | 1.85% | PASS |
| mem_reads | `groupby_scan` | 31.97ms | 34.38ms | 1.075× | 1.24% | PASS |
| mem_reads | `index_join` | 6.95ms | 9.37ms | 1.348× | 2.90% | PASS |
| mem_reads | `index_join_scan` | 4.75ms | 5.46ms | 1.149× | 2.85% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.24s | 1.158× | 1.13% | PASS |
| mem_reads | `table_scan` | 1.27s | 1.38s | 1.083× | 1.75% | PASS |
| mem_reads | `oltp_read_only` | 121.84ms | 137.32ms | 1.127× | 1.85% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.92ms | 356.88ms | 1.513× | 0.68% | PASS |
| mem_writes | `oltp_insert` | 21.85ms | 39.73ms | 1.818× | 1.08% | PASS |
| mem_writes | `oltp_update_index` | 72.50ms | 135.04ms | 1.863× | 1.31% | PASS |
| mem_writes | `oltp_update_non_index` | 50.06ms | 87.56ms | 1.749× | 1.60% | PASS |
| mem_writes | `oltp_delete_insert` | 52.18ms | 104.87ms | 2.010× | 0.92% | PASS |
| mem_writes | `oltp_write_only` | 29.57ms | 62.14ms | 2.102× | 1.37% | PASS |
| mem_writes | `types_delete_insert` | 33.73ms | 55.66ms | 1.650× | 1.64% | PASS |
| mem_writes | `oltp_read_write` | 87.94ms | 141.20ms | 1.606× | 1.41% | PASS |
| file_reads | `oltp_point_select` | 107.29ms | 64.62ms | 0.602× | 1.20% | PASS |
| file_reads | `oltp_range_select` | 22.69ms | 17.12ms | 0.754× | 2.44% | PASS |
| file_reads | `oltp_sum_range` | 21.11ms | 17.11ms | 0.811× | 2.33% | PASS |
| file_reads | `oltp_order_range` | 4.02ms | 3.57ms | 0.888× | 2.09% | PASS |
| file_reads | `oltp_distinct_range` | 5.16ms | 4.72ms | 0.916× | 1.66% | PASS |
| file_reads | `oltp_index_scan` | 12.77ms | 9.52ms | 0.746× | 1.85% | PASS |
| file_reads | `select_random_points` | 27.81ms | 24.67ms | 0.887× | 2.46% | PASS |
| file_reads | `select_random_ranges` | 11.74ms | 7.95ms | 0.677× | 1.32% | PASS |
| file_reads | `covering_index_scan` | 12.58ms | 7.50ms | 0.596× | 1.70% | PASS |
| file_reads | `groupby_scan` | 33.55ms | 35.27ms | 1.051× | 1.16% | PASS |
| file_reads | `index_join` | 12.27ms | 11.89ms | 0.969× | 3.58% | PASS |
| file_reads | `index_join_scan` | 6.12ms | 6.19ms | 1.011× | 3.74% | PASS |
| file_reads | `types_table_scan` | 1.10s | 1.25s | 1.131× | 4.12% | PASS |
| file_reads | `table_scan` | 1.56s | 1.43s | 0.913× | 2.37% | PASS |
| file_reads | `oltp_read_only` | 256.12ms | 185.98ms | 0.726× | 1.26% | PASS |
| file_writes | `oltp_bulk_insert` | 257.31ms | 400.98ms | 1.558× | 1.14% | PASS |
| file_writes | `oltp_insert` | 55.28ms | 55.27ms | 1.000× | 17.52% | PASS |
| file_writes | `oltp_update_index` | 133.85ms | 188.57ms | 1.409× | 3.27% | PASS |
| file_writes | `oltp_update_non_index` | 103.45ms | 122.65ms | 1.186× | 11.11% | PASS |
| file_writes | `oltp_delete_insert` | 102.90ms | 146.88ms | 1.427× | 1.88% | PASS |
| file_writes | `oltp_write_only` | 89.80ms | 94.17ms | 1.049× | 11.49% | PASS |
| file_writes | `types_delete_insert` | 62.52ms | 81.69ms | 1.307× | 2.27% | PASS |
| file_writes | `oltp_read_write` | 164.59ms | 183.29ms | 1.114× | 8.70% | PASS |
| ac_reads | `oltp_point_select` | 59.77ms | 67.44ms | 1.128× | 1.50% | PASS |
| ac_reads | `oltp_range_select` | 19.71ms | 18.09ms | 0.917× | 1.93% | PASS |
| ac_reads | `oltp_sum_range` | 17.58ms | 18.32ms | 1.042× | 2.39% | PASS |
| ac_reads | `oltp_order_range` | 3.73ms | 3.69ms | 0.989× | 1.83% | PASS |
| ac_reads | `oltp_distinct_range` | 4.79ms | 4.76ms | 0.994× | 1.57% | PASS |
| ac_reads | `oltp_index_scan` | 8.12ms | 9.78ms | 1.204× | 2.52% | PASS |
| ac_reads | `select_random_points` | 24.77ms | 27.01ms | 1.091× | 2.03% | PASS |
| ac_reads | `select_random_ranges` | 7.30ms | 8.12ms | 1.113× | 2.02% | PASS |
| ac_reads | `covering_index_scan` | 9.66ms | 7.74ms | 0.801× | 2.19% | PASS |
| ac_reads | `groupby_scan` | 33.86ms | 35.59ms | 1.051× | 1.19% | PASS |
| ac_reads | `index_join` | 10.42ms | 11.96ms | 1.148× | 3.91% | PASS |
| ac_reads | `index_join_scan` | 5.97ms | 6.24ms | 1.044× | 5.10% | PASS |
| ac_reads | `types_table_scan` | 1.30s | 1.30s | 1.002× | 0.88% | PASS |
| ac_reads | `table_scan` | 1.56s | 1.43s | 0.912× | 0.80% | PASS |
| ac_reads | `oltp_read_only` | 174.49ms | 184.56ms | 1.058× | 1.48% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.12ms | 70.14ms | 3.034× | 8.43% | PASS |
| ac_writes | `oltp_insert_ac` | 25.96ms | 85.94ms | 3.311× | 9.32% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.56ms | 103.77ms | 3.633× | 6.49% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.01ms | 81.01ms | 3.680× | 4.76% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.50ms | 92.31ms | 3.767× | 5.67% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.96ms | 92.41ms | 3.428× | 6.73% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.02ms | 83.13ms | 3.774× | 9.73% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.26ms | 98.66ms | 3.156× | 5.21% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 19.70ms | 22.43ms | 1.139× | 2.14% | PASS |
| mem_reads | `oltp_range_select` | 8.59ms | 8.71ms | 1.014× | 2.48% | PASS |
| mem_reads | `oltp_sum_range` | 9.51ms | 9.03ms | 0.950× | 3.70% | PASS |
| mem_reads | `oltp_order_range` | 2.12ms | 2.00ms | 0.943× | 1.81% | PASS |
| mem_reads | `oltp_distinct_range` | 2.61ms | 2.47ms | 0.945× | 1.08% | PASS |
| mem_reads | `oltp_index_scan` | 3.16ms | 4.40ms | 1.394× | 2.73% | PASS |
| mem_reads | `select_random_points` | 13.84ms | 14.69ms | 1.062× | 1.78% | PASS |
| mem_reads | `select_random_ranges` | 3.12ms | 3.55ms | 1.138× | 1.50% | PASS |
| mem_reads | `covering_index_scan` | 2.78ms | 3.38ms | 1.215× | 4.05% | PASS |
| mem_reads | `groupby_scan` | 20.29ms | 19.66ms | 0.969× | 1.93% | PASS |
| mem_reads | `index_join` | 4.77ms | 7.42ms | 1.557× | 2.61% | PASS |
| mem_reads | `index_join_scan` | 3.66ms | 4.93ms | 1.347× | 4.33% | PASS |
| mem_reads | `types_table_scan` | 762.09ms | 782.38ms | 1.027× | 2.25% | PASS |
| mem_reads | `table_scan` | 952.30ms | 891.15ms | 0.936× | 2.28% | PASS |
| mem_reads | `oltp_read_only` | 74.58ms | 76.00ms | 1.019× | 3.18% | PASS |
| mem_writes | `oltp_bulk_insert` | 141.91ms | 206.54ms | 1.455× | 1.84% | PASS |
| mem_writes | `oltp_insert` | 12.11ms | 25.44ms | 2.101× | 1.97% | PASS |
| mem_writes | `oltp_update_index` | 45.39ms | 93.11ms | 2.051× | 2.47% | PASS |
| mem_writes | `oltp_update_non_index` | 33.40ms | 59.21ms | 1.773× | 1.49% | PASS |
| mem_writes | `oltp_delete_insert` | 32.23ms | 71.39ms | 2.215× | 2.22% | PASS |
| mem_writes | `oltp_write_only` | 18.76ms | 44.60ms | 2.377× | 2.23% | PASS |
| mem_writes | `types_delete_insert` | 21.46ms | 37.22ms | 1.734× | 2.75% | PASS |
| mem_writes | `oltp_read_write` | 54.10ms | 85.44ms | 1.579× | 2.99% | PASS |
| file_reads | `oltp_point_select` | 74.45ms | 41.08ms | 0.552× | 1.98% | PASS |
| file_reads | `oltp_range_select` | 14.60ms | 10.37ms | 0.711× | 2.47% | PASS |
| file_reads | `oltp_sum_range` | 14.98ms | 10.56ms | 0.705× | 2.69% | PASS |
| file_reads | `oltp_order_range` | 2.69ms | 2.18ms | 0.809× | 0.92% | PASS |
| file_reads | `oltp_distinct_range` | 3.18ms | 2.65ms | 0.834× | 1.14% | PASS |
| file_reads | `oltp_index_scan` | 9.27ms | 6.30ms | 0.679× | 1.58% | PASS |
| file_reads | `select_random_points` | 18.71ms | 15.22ms | 0.813× | 2.66% | PASS |
| file_reads | `select_random_ranges` | 8.60ms | 5.37ms | 0.624× | 1.80% | PASS |
| file_reads | `covering_index_scan` | 9.55ms | 5.29ms | 0.555× | 2.57% | PASS |
| file_reads | `groupby_scan` | 21.18ms | 20.13ms | 0.950× | 0.96% | PASS |
| file_reads | `index_join` | 8.29ms | 7.67ms | 0.926× | 2.74% | PASS |
| file_reads | `index_join_scan` | 3.88ms | 4.39ms | 1.133× | 2.15% | PASS |
| file_reads | `types_table_scan` | 746.88ms | 758.93ms | 1.016× | 3.11% | PASS |
| file_reads | `table_scan` | 977.72ms | 885.94ms | 0.906× | 3.23% | PASS |
| file_reads | `oltp_read_only` | 163.06ms | 106.08ms | 0.651× | 3.54% | PASS |
| file_writes | `oltp_bulk_insert` | 193.79ms | 279.33ms | 1.441× | 4.56% | PASS |
| file_writes | `oltp_insert` | 30.57ms | 48.93ms | 1.600× | 5.10% | PASS |
| file_writes | `oltp_update_index` | 171.32ms | 179.51ms | 1.048× | 3.80% | PASS |
| file_writes | `oltp_update_non_index` | 133.78ms | 126.21ms | 0.943× | 4.46% | PASS |
| file_writes | `oltp_delete_insert` | 137.05ms | 139.83ms | 1.020× | 3.69% | PASS |
| file_writes | `oltp_write_only` | 99.87ms | 99.39ms | 0.995× | 8.38% | PASS |
| file_writes | `types_delete_insert` | 90.02ms | 79.96ms | 0.888× | 10.32% | PASS |
| file_writes | `oltp_read_write` | 131.06ms | 139.44ms | 1.064× | 5.48% | PASS |
| ac_reads | `oltp_point_select` | 39.61ms | 42.69ms | 1.078× | 2.33% | PASS |
| ac_reads | `oltp_range_select` | 11.88ms | 11.15ms | 0.938× | 2.73% | PASS |
| ac_reads | `oltp_sum_range` | 12.21ms | 11.27ms | 0.923× | 3.37% | PASS |
| ac_reads | `oltp_order_range` | 2.37ms | 2.21ms | 0.930× | 1.74% | PASS |
| ac_reads | `oltp_distinct_range` | 2.89ms | 2.68ms | 0.926× | 1.83% | PASS |
| ac_reads | `oltp_index_scan` | 5.51ms | 6.22ms | 1.129× | 1.43% | PASS |
| ac_reads | `select_random_points` | 15.33ms | 15.72ms | 1.025× | 2.46% | PASS |
| ac_reads | `select_random_ranges` | 5.04ms | 5.37ms | 1.064× | 1.46% | PASS |
| ac_reads | `covering_index_scan` | 5.64ms | 5.20ms | 0.923× | 2.91% | PASS |
| ac_reads | `groupby_scan` | 19.98ms | 19.18ms | 0.960× | 1.56% | PASS |
| ac_reads | `index_join` | 6.51ms | 7.57ms | 1.163× | 3.31% | PASS |
| ac_reads | `index_join_scan` | 3.61ms | 4.44ms | 1.231× | 3.14% | PASS |
| ac_reads | `types_table_scan` | 719.68ms | 743.50ms | 1.033× | 1.78% | PASS |
| ac_reads | `table_scan` | 948.43ms | 865.69ms | 0.913× | 3.09% | PASS |
| ac_reads | `oltp_read_only` | 104.10ms | 103.40ms | 0.993× | 3.77% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.49ms | 56.92ms | 2.531× | 4.78% | PASS |
| ac_writes | `oltp_insert_ac` | 26.45ms | 71.85ms | 2.717× | 5.36% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.23ms | 81.91ms | 2.902× | 3.61% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.89ms | 64.74ms | 2.957× | 2.81% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.46ms | 75.40ms | 2.961× | 3.64% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.45ms | 74.31ms | 2.919× | 6.03% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.56ms | 64.73ms | 3.149× | 6.20% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.42ms | 77.85ms | 2.740× | 5.11% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.48ms | 40.91ms | 1.186× | 2.29% | PASS |
| mem_reads | `oltp_range_select` | 19.71ms | 21.41ms | 1.086× | 2.53% | PASS |
| mem_reads | `oltp_sum_range` | 18.63ms | 21.14ms | 1.135× | 1.49% | PASS |
| mem_reads | `oltp_order_range` | 3.64ms | 3.87ms | 1.063× | 1.20% | PASS |
| mem_reads | `oltp_distinct_range` | 4.74ms | 4.95ms | 1.043× | 0.82% | PASS |
| mem_reads | `oltp_index_scan` | 4.72ms | 6.46ms | 1.370× | 1.58% | PASS |
| mem_reads | `select_random_points` | 28.97ms | 32.89ms | 1.135× | 2.01% | PASS |
| mem_reads | `select_random_ranges` | 7.91ms | 9.18ms | 1.160× | 1.37% | PASS |
| mem_reads | `covering_index_scan` | 4.29ms | 4.51ms | 1.052× | 1.91% | PASS |
| mem_reads | `groupby_scan` | 36.82ms | 39.50ms | 1.073× | 1.13% | PASS |
| mem_reads | `index_join` | 8.34ms | 10.96ms | 1.314× | 2.55% | PASS |
| mem_reads | `index_join_scan` | 4.42ms | 5.65ms | 1.279× | 2.12% | PASS |
| mem_reads | `types_table_scan` | 1.08s | 1.26s | 1.162× | 1.07% | PASS |
| mem_reads | `table_scan` | 1.31s | 1.41s | 1.079× | 1.33% | PASS |
| mem_reads | `oltp_read_only` | 157.25ms | 172.91ms | 1.100× | 1.13% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.38ms | 359.62ms | 1.442× | 0.90% | PASS |
| mem_writes | `oltp_insert` | 19.47ms | 36.98ms | 1.900× | 0.63% | PASS |
| mem_writes | `oltp_update_index` | 70.21ms | 120.94ms | 1.723× | 1.07% | PASS |
| mem_writes | `oltp_update_non_index` | 52.36ms | 85.39ms | 1.631× | 1.24% | PASS |
| mem_writes | `oltp_delete_insert` | 51.45ms | 99.50ms | 1.934× | 1.18% | PASS |
| mem_writes | `oltp_write_only` | 28.27ms | 59.33ms | 2.098× | 1.48% | PASS |
| mem_writes | `types_delete_insert` | 34.04ms | 56.15ms | 1.650× | 1.39% | PASS |
| mem_writes | `oltp_read_write` | 107.48ms | 160.11ms | 1.490× | 1.29% | PASS |
| file_reads | `oltp_point_select` | 112.03ms | 67.55ms | 0.603× | 0.91% | PASS |
| file_reads | `oltp_range_select` | 28.47ms | 24.58ms | 0.863× | 1.30% | PASS |
| file_reads | `oltp_sum_range` | 27.16ms | 24.32ms | 0.895× | 1.40% | PASS |
| file_reads | `oltp_order_range` | 4.56ms | 4.26ms | 0.934× | 1.30% | PASS |
| file_reads | `oltp_distinct_range` | 5.75ms | 5.35ms | 0.930× | 1.06% | PASS |
| file_reads | `oltp_index_scan` | 12.77ms | 9.37ms | 0.734× | 2.00% | PASS |
| file_reads | `select_random_points` | 39.32ms | 37.31ms | 0.949× | 1.60% | PASS |
| file_reads | `select_random_ranges` | 16.15ms | 12.18ms | 0.754× | 1.02% | PASS |
| file_reads | `covering_index_scan` | 12.37ms | 7.32ms | 0.592× | 1.62% | PASS |
| file_reads | `groupby_scan` | 38.04ms | 40.17ms | 1.056× | 0.92% | PASS |
| file_reads | `index_join` | 13.11ms | 13.28ms | 1.013× | 1.86% | PASS |
| file_reads | `index_join_scan` | 5.54ms | 6.24ms | 1.127× | 2.64% | PASS |
| file_reads | `types_table_scan` | 1.07s | 1.25s | 1.160× | 1.23% | PASS |
| file_reads | `table_scan` | 1.31s | 1.41s | 1.076× | 1.48% | PASS |
| file_reads | `oltp_read_only` | 272.48ms | 214.83ms | 0.788× | 1.07% | PASS |
| file_writes | `oltp_bulk_insert` | 264.45ms | 383.09ms | 1.449× | 0.85% | PASS |
| file_writes | `oltp_insert` | 27.07ms | 46.80ms | 1.729× | 1.60% | PASS |
| file_writes | `oltp_update_index` | 102.21ms | 148.66ms | 1.455× | 1.56% | PASS |
| file_writes | `oltp_update_non_index` | 80.36ms | 107.32ms | 1.336× | 1.52% | PASS |
| file_writes | `oltp_delete_insert` | 81.88ms | 123.54ms | 1.509× | 1.55% | PASS |
| file_writes | `oltp_write_only` | 53.81ms | 79.70ms | 1.481× | 1.72% | PASS |
| file_writes | `types_delete_insert` | 52.17ms | 70.37ms | 1.349× | 1.67% | PASS |
| file_writes | `oltp_read_write` | 136.72ms | 180.41ms | 1.320× | 1.40% | PASS |
| ac_reads | `oltp_point_select` | 59.96ms | 67.48ms | 1.125× | 1.50% | PASS |
| ac_reads | `oltp_range_select` | 22.89ms | 24.48ms | 1.070× | 2.34% | PASS |
| ac_reads | `oltp_sum_range` | 21.69ms | 24.25ms | 1.118× | 1.13% | PASS |
| ac_reads | `oltp_order_range` | 4.02ms | 4.26ms | 1.061× | 0.96% | PASS |
| ac_reads | `oltp_distinct_range` | 5.16ms | 5.34ms | 1.034× | 1.00% | PASS |
| ac_reads | `oltp_index_scan` | 7.71ms | 9.33ms | 1.210× | 1.97% | PASS |
| ac_reads | `select_random_points` | 33.01ms | 37.43ms | 1.134× | 1.93% | PASS |
| ac_reads | `select_random_ranges` | 10.83ms | 12.18ms | 1.125× | 1.59% | PASS |
| ac_reads | `covering_index_scan` | 7.32ms | 7.33ms | 1.002× | 1.21% | PASS |
| ac_reads | `groupby_scan` | 37.23ms | 40.15ms | 1.078× | 0.94% | PASS |
| ac_reads | `index_join` | 10.42ms | 13.26ms | 1.273× | 1.75% | PASS |
| ac_reads | `index_join_scan` | 4.94ms | 6.19ms | 1.252× | 2.78% | PASS |
| ac_reads | `types_table_scan` | 1.08s | 1.25s | 1.159× | 2.06% | PASS |
| ac_reads | `table_scan` | 1.31s | 1.41s | 1.073× | 1.50% | PASS |
| ac_reads | `oltp_read_only` | 196.79ms | 216.16ms | 1.098× | 1.27% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.24ms | 69.31ms | 2.860× | 9.96% | PASS |
| ac_writes | `oltp_insert_ac` | 25.28ms | 89.86ms | 3.555× | 6.12% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.43ms | 102.58ms | 3.739× | 5.78% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.96ms | 83.31ms | 3.477× | 6.44% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.53ms | 94.95ms | 3.579× | 6.56% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.64ms | 94.00ms | 3.529× | 4.69% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.30ms | 86.09ms | 3.860× | 6.15% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.51ms | 103.23ms | 3.080× | 6.32% | PASS |

</details>

## Version-control latency

Wall time: 1m 35s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 52.40ms | 200.00ms | 26.2% | 1.82% | PASS |
| `status_dirty_many_tables` | 53.48ms | 200.00ms | 26.7% | 0.84% | PASS |
| `diff_regular_working_one_table` | 49.18ms | 150.00ms | 32.8% | 0.79% | PASS |
| `diff_regular_working_many_tables` | 56.39ms | 200.00ms | 28.2% | 1.12% | PASS |
| `diff_stat_working_many_tables` | 57.19ms | 200.00ms | 28.6% | 1.78% | PASS |
| `diff_schema_working_many_tables` | 57.94ms | 200.00ms | 29.0% | 1.46% | PASS |
| `branch_list_many_branches` | 15.52ms | 100.00ms | 15.5% | 1.37% | PASS |
| `branch_create_delete` | 16.98ms | 100.00ms | 17.0% | 1.48% | PASS |
| `checkout_branch_clean` | 74.93ms | 200.00ms | 37.5% | 4.14% | PASS |
| `merge_data_no_conflicts` | 27.11ms | 150.00ms | 18.1% | 9.39% | PASS |
| `merge_schema_no_conflicts` | 14.89ms | 100.00ms | 14.9% | 2.63% | PASS |
| `merge_data_conflicts` | 72.75ms | 250.00ms | 29.1% | 0.85% | PASS |
| `merge_data_conflicts_with_resolve` | 73.09ms | 250.00ms | 29.2% | 1.05% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
