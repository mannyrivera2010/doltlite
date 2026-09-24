# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-24 15:59 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260920.314.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/36012101655)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 42s | 9.13s | 11.51s | 1.261× | 1.69% | **PASS** |
| textpk | 69 | 55 | 1h 34m 51s | 10.58s | 11.98s | 1.132× | 2.32% | **PASS** |
| blobpk | 69 | 55 | 1h 31m 23s | 10.00s | 11.85s | 1.184× | 1.66% | **PASS** |
| compositepk | 69 | 55 | 1h 25m 34s | 9.46s | 11.83s | 1.251× | 1.23% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.02ms | 29.83ms | 1.192× | 1.64% | PASS |
| mem_reads | `oltp_range_select` | 10.60ms | 13.24ms | 1.248× | 1.81% | PASS |
| mem_reads | `oltp_sum_range` | 10.09ms | 12.44ms | 1.233× | 2.13% | PASS |
| mem_reads | `oltp_order_range` | 2.62ms | 3.02ms | 1.153× | 1.44% | PASS |
| mem_reads | `oltp_distinct_range` | 3.66ms | 4.05ms | 1.106× | 1.23% | PASS |
| mem_reads | `oltp_index_scan` | 4.04ms | 5.42ms | 1.342× | 2.67% | PASS |
| mem_reads | `select_random_points` | 10.72ms | 11.33ms | 1.056× | 3.62% | PASS |
| mem_reads | `select_random_ranges` | 3.14ms | 4.08ms | 1.297× | 1.82% | PASS |
| mem_reads | `covering_index_scan` | 4.29ms | 4.42ms | 1.030× | 1.94% | PASS |
| mem_reads | `groupby_scan` | 30.22ms | 33.41ms | 1.106× | 0.88% | PASS |
| mem_reads | `index_join` | 6.16ms | 8.50ms | 1.381× | 2.53% | PASS |
| mem_reads | `index_join_scan` | 3.58ms | 4.65ms | 1.298× | 2.06% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.34s | 1.284× | 0.76% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.39s | 1.196× | 0.73% | PASS |
| mem_reads | `oltp_read_only` | 103.75ms | 123.67ms | 1.192× | 1.12% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.56ms | 252.65ms | 1.392× | 1.16% | PASS |
| mem_writes | `oltp_insert` | 15.53ms | 28.17ms | 1.814× | 1.01% | PASS |
| mem_writes | `oltp_update_index` | 51.10ms | 104.22ms | 2.040× | 1.24% | PASS |
| mem_writes | `oltp_update_non_index` | 35.48ms | 60.40ms | 1.702× | 1.56% | PASS |
| mem_writes | `oltp_delete_insert` | 46.51ms | 79.59ms | 1.711× | 1.70% | PASS |
| mem_writes | `oltp_write_only` | 22.87ms | 46.00ms | 2.011× | 1.72% | PASS |
| mem_writes | `types_delete_insert` | 25.51ms | 41.06ms | 1.609× | 1.94% | PASS |
| mem_writes | `oltp_read_write` | 72.86ms | 113.58ms | 1.559× | 1.54% | PASS |
| file_reads | `oltp_point_select` | 100.76ms | 56.08ms | 0.557× | 0.79% | PASS |
| file_reads | `oltp_range_select` | 19.25ms | 16.12ms | 0.837× | 1.17% | PASS |
| file_reads | `oltp_sum_range` | 18.30ms | 15.38ms | 0.840× | 1.35% | PASS |
| file_reads | `oltp_order_range` | 3.57ms | 3.40ms | 0.952× | 1.68% | PASS |
| file_reads | `oltp_distinct_range` | 4.66ms | 4.45ms | 0.955× | 1.25% | PASS |
| file_reads | `oltp_index_scan` | 11.99ms | 8.43ms | 0.703× | 1.28% | PASS |
| file_reads | `select_random_points` | 18.93ms | 14.11ms | 0.745× | 1.76% | PASS |
| file_reads | `select_random_ranges` | 10.71ms | 6.66ms | 0.621× | 1.01% | PASS |
| file_reads | `covering_index_scan` | 12.04ms | 7.15ms | 0.594× | 1.33% | PASS |
| file_reads | `groupby_scan` | 31.17ms | 33.90ms | 1.087× | 1.02% | PASS |
| file_reads | `index_join` | 10.56ms | 10.34ms | 0.980× | 2.07% | PASS |
| file_reads | `index_join_scan` | 4.83ms | 5.20ms | 1.075× | 3.62% | PASS |
| file_reads | `types_table_scan` | 1.09s | 1.35s | 1.231× | 1.45% | PASS |
| file_reads | `table_scan` | 1.22s | 1.41s | 1.150× | 1.20% | PASS |
| file_reads | `oltp_read_only` | 218.06ms | 163.17ms | 0.748× | 1.17% | PASS |
| file_writes | `oltp_bulk_insert` | 194.58ms | 273.69ms | 1.407× | 1.16% | PASS |
| file_writes | `oltp_insert` | 22.56ms | 35.85ms | 1.589× | 1.95% | PASS |
| file_writes | `oltp_update_index` | 79.65ms | 129.94ms | 1.631× | 1.17% | PASS |
| file_writes | `oltp_update_non_index` | 60.76ms | 83.83ms | 1.380× | 2.55% | PASS |
| file_writes | `oltp_delete_insert` | 73.31ms | 101.34ms | 1.382× | 1.91% | PASS |
| file_writes | `oltp_write_only` | 46.85ms | 65.46ms | 1.397× | 1.95% | PASS |
| file_writes | `types_delete_insert` | 42.48ms | 54.67ms | 1.287× | 2.09% | PASS |
| file_writes | `oltp_read_write` | 96.90ms | 132.35ms | 1.366× | 1.96% | PASS |
| ac_reads | `oltp_point_select` | 49.90ms | 55.69ms | 1.116× | 1.23% | PASS |
| ac_reads | `oltp_range_select` | 13.83ms | 16.06ms | 1.161× | 1.69% | PASS |
| ac_reads | `oltp_sum_range` | 13.21ms | 15.33ms | 1.161× | 1.92% | PASS |
| ac_reads | `oltp_order_range` | 3.03ms | 3.38ms | 1.114× | 1.47% | PASS |
| ac_reads | `oltp_distinct_range` | 4.11ms | 4.47ms | 1.088× | 1.71% | PASS |
| ac_reads | `oltp_index_scan` | 7.01ms | 8.48ms | 1.210× | 1.58% | PASS |
| ac_reads | `select_random_points` | 14.43ms | 14.39ms | 0.997× | 3.35% | PASS |
| ac_reads | `select_random_ranges` | 5.75ms | 6.67ms | 1.160× | 1.08% | PASS |
| ac_reads | `covering_index_scan` | 7.36ms | 7.25ms | 0.985× | 1.58% | PASS |
| ac_reads | `groupby_scan` | 30.74ms | 34.14ms | 1.111× | 0.87% | PASS |
| ac_reads | `index_join` | 8.26ms | 10.70ms | 1.297× | 2.06% | PASS |
| ac_reads | `index_join_scan` | 4.20ms | 5.16ms | 1.231× | 2.65% | PASS |
| ac_reads | `types_table_scan` | 1.10s | 1.35s | 1.224× | 1.69% | PASS |
| ac_reads | `table_scan` | 1.23s | 1.42s | 1.150× | 1.70% | PASS |
| ac_reads | `oltp_read_only` | 147.43ms | 164.81ms | 1.118× | 1.26% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.58ms | 69.85ms | 2.962× | 6.90% | PASS |
| ac_writes | `oltp_insert_ac` | 25.05ms | 85.90ms | 3.429× | 7.72% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.90ms | 103.77ms | 3.470× | 6.37% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.24ms | 80.04ms | 3.302× | 8.43% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.36ms | 93.59ms | 3.691× | 8.84% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.43ms | 92.36ms | 3.495× | 7.95% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.58ms | 81.68ms | 3.323× | 9.63% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.27ms | 99.81ms | 3.093× | 5.97% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.07ms | 38.33ms | 1.234× | 2.14% | PASS |
| mem_reads | `oltp_range_select` | 14.58ms | 14.79ms | 1.015× | 2.32% | PASS |
| mem_reads | `oltp_sum_range` | 13.10ms | 14.49ms | 1.106× | 2.75% | PASS |
| mem_reads | `oltp_order_range` | 3.07ms | 3.21ms | 1.047× | 1.38% | PASS |
| mem_reads | `oltp_distinct_range` | 4.10ms | 4.28ms | 1.044× | 1.16% | PASS |
| mem_reads | `oltp_index_scan` | 4.67ms | 6.50ms | 1.391× | 2.31% | PASS |
| mem_reads | `select_random_points` | 18.75ms | 21.40ms | 1.141× | 2.04% | PASS |
| mem_reads | `select_random_ranges` | 4.18ms | 5.27ms | 1.260× | 1.76% | PASS |
| mem_reads | `covering_index_scan` | 4.70ms | 4.71ms | 1.003× | 2.79% | PASS |
| mem_reads | `groupby_scan` | 32.41ms | 34.58ms | 1.067× | 1.24% | PASS |
| mem_reads | `index_join` | 7.58ms | 10.12ms | 1.336× | 3.43% | PASS |
| mem_reads | `index_join_scan` | 5.03ms | 5.62ms | 1.118× | 5.10% | PASS |
| mem_reads | `types_table_scan` | 1.19s | 1.27s | 1.066× | 5.29% | PASS |
| mem_reads | `table_scan` | 1.45s | 1.40s | 0.966× | 4.02% | PASS |
| mem_reads | `oltp_read_only` | 124.28ms | 139.05ms | 1.119× | 1.79% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.52ms | 360.19ms | 1.523× | 0.94% | PASS |
| mem_writes | `oltp_insert` | 22.20ms | 40.09ms | 1.806× | 1.07% | PASS |
| mem_writes | `oltp_update_index` | 76.81ms | 140.60ms | 1.830× | 2.12% | PASS |
| mem_writes | `oltp_update_non_index` | 52.70ms | 92.29ms | 1.751× | 1.30% | PASS |
| mem_writes | `oltp_delete_insert` | 58.09ms | 112.67ms | 1.939× | 1.66% | PASS |
| mem_writes | `oltp_write_only` | 32.32ms | 65.83ms | 2.037× | 2.49% | PASS |
| mem_writes | `types_delete_insert` | 34.78ms | 57.83ms | 1.663× | 1.73% | PASS |
| mem_writes | `oltp_read_write` | 87.44ms | 141.11ms | 1.614× | 2.01% | PASS |
| file_reads | `oltp_point_select` | 106.81ms | 64.86ms | 0.607× | 1.09% | PASS |
| file_reads | `oltp_range_select` | 21.30ms | 17.17ms | 0.806× | 2.53% | PASS |
| file_reads | `oltp_sum_range` | 21.50ms | 17.59ms | 0.818× | 1.86% | PASS |
| file_reads | `oltp_order_range` | 4.07ms | 3.60ms | 0.884× | 2.14% | PASS |
| file_reads | `oltp_distinct_range` | 5.17ms | 4.68ms | 0.906× | 1.09% | PASS |
| file_reads | `oltp_index_scan` | 13.11ms | 9.57ms | 0.730× | 1.66% | PASS |
| file_reads | `select_random_points` | 29.35ms | 26.31ms | 0.896× | 2.34% | PASS |
| file_reads | `select_random_ranges` | 12.23ms | 8.04ms | 0.658× | 1.07% | PASS |
| file_reads | `covering_index_scan` | 13.45ms | 7.53ms | 0.560× | 4.40% | PASS |
| file_reads | `groupby_scan` | 33.63ms | 35.03ms | 1.042× | 1.18% | PASS |
| file_reads | `index_join` | 12.04ms | 11.76ms | 0.977× | 3.47% | PASS |
| file_reads | `index_join_scan` | 6.22ms | 6.18ms | 0.993× | 4.14% | PASS |
| file_reads | `types_table_scan` | 1.22s | 1.27s | 1.039× | 3.31% | PASS |
| file_reads | `table_scan` | 1.38s | 1.40s | 1.018× | 4.35% | PASS |
| file_reads | `oltp_read_only` | 244.85ms | 180.66ms | 0.738× | 1.06% | PASS |
| file_writes | `oltp_bulk_insert` | 256.22ms | 391.06ms | 1.526× | 1.01% | PASS |
| file_writes | `oltp_insert` | 60.87ms | 53.66ms | 0.882× | 18.80% | PASS |
| file_writes | `oltp_update_index` | 118.92ms | 175.16ms | 1.473× | 1.87% | PASS |
| file_writes | `oltp_update_non_index` | 91.81ms | 114.65ms | 1.249× | 7.12% | PASS |
| file_writes | `oltp_delete_insert` | 94.16ms | 136.77ms | 1.453× | 2.49% | PASS |
| file_writes | `oltp_write_only` | 87.13ms | 87.84ms | 1.008× | 7.79% | PASS |
| file_writes | `types_delete_insert` | 58.35ms | 77.51ms | 1.329× | 2.10% | PASS |
| file_writes | `oltp_read_write` | 154.79ms | 173.39ms | 1.120× | 8.85% | PASS |
| ac_reads | `oltp_point_select` | 58.52ms | 66.17ms | 1.131× | 1.31% | PASS |
| ac_reads | `oltp_range_select` | 17.93ms | 17.48ms | 0.975× | 2.57% | PASS |
| ac_reads | `oltp_sum_range` | 16.56ms | 17.57ms | 1.061× | 2.23% | PASS |
| ac_reads | `oltp_order_range` | 3.57ms | 3.62ms | 1.016× | 1.75% | PASS |
| ac_reads | `oltp_distinct_range` | 4.62ms | 4.68ms | 1.014× | 1.86% | PASS |
| ac_reads | `oltp_index_scan` | 8.04ms | 9.57ms | 1.190× | 2.22% | PASS |
| ac_reads | `select_random_points` | 23.37ms | 25.94ms | 1.110× | 2.86% | PASS |
| ac_reads | `select_random_ranges` | 7.12ms | 8.05ms | 1.130× | 1.46% | PASS |
| ac_reads | `covering_index_scan` | 9.02ms | 7.59ms | 0.841× | 2.76% | PASS |
| ac_reads | `groupby_scan` | 33.40ms | 35.25ms | 1.055× | 1.08% | PASS |
| ac_reads | `index_join` | 9.94ms | 11.85ms | 1.192× | 2.61% | PASS |
| ac_reads | `index_join_scan` | 5.67ms | 6.15ms | 1.085× | 4.42% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.24s | 1.116× | 3.39% | PASS |
| ac_reads | `table_scan` | 1.35s | 1.39s | 1.030× | 4.11% | PASS |
| ac_reads | `oltp_read_only` | 164.94ms | 178.50ms | 1.082× | 1.73% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.42ms | 68.07ms | 3.036× | 5.63% | PASS |
| ac_writes | `oltp_insert_ac` | 26.96ms | 86.16ms | 3.195× | 6.24% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.96ms | 102.99ms | 3.683× | 6.47% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.92ms | 80.49ms | 3.512× | 6.00% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.74ms | 93.01ms | 3.614× | 7.21% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.90ms | 92.23ms | 3.561× | 5.57% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.08ms | 81.23ms | 3.679× | 7.71% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.58ms | 97.80ms | 3.002× | 5.26% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.75ms | 36.97ms | 1.242× | 1.41% | PASS |
| mem_reads | `oltp_range_select` | 12.69ms | 14.02ms | 1.105× | 2.31% | PASS |
| mem_reads | `oltp_sum_range` | 12.02ms | 13.94ms | 1.160× | 2.73% | PASS |
| mem_reads | `oltp_order_range` | 2.93ms | 3.16ms | 1.079× | 1.38% | PASS |
| mem_reads | `oltp_distinct_range` | 3.98ms | 4.22ms | 1.058× | 0.99% | PASS |
| mem_reads | `oltp_index_scan` | 4.55ms | 6.39ms | 1.406× | 1.94% | PASS |
| mem_reads | `select_random_points` | 17.60ms | 20.86ms | 1.186× | 2.06% | PASS |
| mem_reads | `select_random_ranges` | 4.12ms | 5.23ms | 1.270× | 1.61% | PASS |
| mem_reads | `covering_index_scan` | 4.41ms | 4.63ms | 1.052× | 2.08% | PASS |
| mem_reads | `groupby_scan` | 32.32ms | 34.51ms | 1.068× | 0.82% | PASS |
| mem_reads | `index_join` | 6.81ms | 9.51ms | 1.397× | 3.10% | PASS |
| mem_reads | `index_join_scan` | 4.29ms | 5.48ms | 1.278× | 2.49% | PASS |
| mem_reads | `types_table_scan` | 1.08s | 1.25s | 1.157× | 1.30% | PASS |
| mem_reads | `table_scan` | 1.37s | 1.41s | 1.026× | 2.03% | PASS |
| mem_reads | `oltp_read_only` | 120.14ms | 136.81ms | 1.139× | 1.27% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.28ms | 351.31ms | 1.487× | 0.93% | PASS |
| mem_writes | `oltp_insert` | 19.71ms | 39.28ms | 1.993× | 1.05% | PASS |
| mem_writes | `oltp_update_index` | 70.03ms | 134.21ms | 1.916× | 2.31% | PASS |
| mem_writes | `oltp_update_non_index` | 48.74ms | 84.79ms | 1.740× | 1.52% | PASS |
| mem_writes | `oltp_delete_insert` | 48.82ms | 103.78ms | 2.126× | 1.84% | PASS |
| mem_writes | `oltp_write_only` | 27.83ms | 61.91ms | 2.224× | 1.47% | PASS |
| mem_writes | `types_delete_insert` | 32.91ms | 55.48ms | 1.686× | 1.79% | PASS |
| mem_writes | `oltp_read_write` | 90.80ms | 145.39ms | 1.601× | 1.49% | PASS |
| file_reads | `oltp_point_select` | 106.18ms | 63.74ms | 0.600× | 1.08% | PASS |
| file_reads | `oltp_range_select` | 21.95ms | 17.16ms | 0.782× | 2.02% | PASS |
| file_reads | `oltp_sum_range` | 20.63ms | 17.19ms | 0.833× | 1.59% | PASS |
| file_reads | `oltp_order_range` | 3.92ms | 3.50ms | 0.894× | 1.32% | PASS |
| file_reads | `oltp_distinct_range` | 4.97ms | 4.58ms | 0.921× | 1.36% | PASS |
| file_reads | `oltp_index_scan` | 12.46ms | 9.26ms | 0.743× | 1.36% | PASS |
| file_reads | `select_random_points` | 28.18ms | 25.10ms | 0.891× | 2.55% | PASS |
| file_reads | `select_random_ranges` | 11.77ms | 7.88ms | 0.670× | 1.17% | PASS |
| file_reads | `covering_index_scan` | 12.70ms | 7.38ms | 0.581× | 2.21% | PASS |
| file_reads | `groupby_scan` | 33.42ms | 35.02ms | 1.048× | 0.81% | PASS |
| file_reads | `index_join` | 12.05ms | 11.73ms | 0.973× | 3.86% | PASS |
| file_reads | `index_join_scan` | 5.50ms | 6.09ms | 1.106× | 3.28% | PASS |
| file_reads | `types_table_scan` | 1.19s | 1.27s | 1.065× | 1.38% | PASS |
| file_reads | `table_scan` | 1.29s | 1.39s | 1.080× | 3.58% | PASS |
| file_reads | `oltp_read_only` | 236.85ms | 177.46ms | 0.749× | 1.01% | PASS |
| file_writes | `oltp_bulk_insert` | 255.30ms | 375.58ms | 1.471× | 0.72% | PASS |
| file_writes | `oltp_insert` | 31.78ms | 51.65ms | 1.625× | 1.66% | PASS |
| file_writes | `oltp_update_index` | 106.14ms | 166.37ms | 1.567× | 1.57% | PASS |
| file_writes | `oltp_update_non_index` | 80.61ms | 109.22ms | 1.355× | 1.03% | PASS |
| file_writes | `oltp_delete_insert` | 81.53ms | 130.91ms | 1.606× | 1.73% | PASS |
| file_writes | `oltp_write_only` | 55.96ms | 84.12ms | 1.503× | 1.36% | PASS |
| file_writes | `types_delete_insert` | 52.17ms | 72.22ms | 1.384× | 1.63% | PASS |
| file_writes | `oltp_read_write` | 117.14ms | 162.52ms | 1.387× | 1.34% | PASS |
| ac_reads | `oltp_point_select` | 55.12ms | 62.92ms | 1.141× | 1.05% | PASS |
| ac_reads | `oltp_range_select` | 16.43ms | 16.93ms | 1.031× | 2.37% | PASS |
| ac_reads | `oltp_sum_range` | 15.67ms | 17.04ms | 1.087× | 1.73% | PASS |
| ac_reads | `oltp_order_range` | 3.39ms | 3.50ms | 1.034× | 1.49% | PASS |
| ac_reads | `oltp_distinct_range` | 4.47ms | 4.59ms | 1.027× | 1.06% | PASS |
| ac_reads | `oltp_index_scan` | 7.67ms | 9.35ms | 1.220× | 1.82% | PASS |
| ac_reads | `select_random_points` | 22.79ms | 25.62ms | 1.124× | 1.66% | PASS |
| ac_reads | `select_random_ranges` | 6.95ms | 7.89ms | 1.135× | 1.37% | PASS |
| ac_reads | `covering_index_scan` | 7.98ms | 7.37ms | 0.923× | 1.99% | PASS |
| ac_reads | `groupby_scan` | 32.68ms | 34.98ms | 1.070× | 0.65% | PASS |
| ac_reads | `index_join` | 9.17ms | 11.48ms | 1.252× | 2.71% | PASS |
| ac_reads | `index_join_scan` | 4.93ms | 6.02ms | 1.221× | 3.28% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.24s | 1.124× | 1.49% | PASS |
| ac_reads | `table_scan` | 1.28s | 1.39s | 1.080× | 2.40% | PASS |
| ac_reads | `oltp_read_only` | 156.26ms | 174.82ms | 1.119× | 1.47% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.19ms | 68.28ms | 2.945× | 5.20% | PASS |
| ac_writes | `oltp_insert_ac` | 26.83ms | 91.80ms | 3.422× | 6.22% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.35ms | 101.25ms | 3.450× | 4.77% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.49ms | 81.83ms | 3.210× | 7.42% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.61ms | 91.47ms | 3.572× | 4.33% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.29ms | 91.37ms | 3.348× | 5.09% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.24ms | 81.61ms | 3.512× | 5.12% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.89ms | 97.98ms | 3.072× | 4.04% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.87ms | 40.21ms | 1.224× | 1.29% | PASS |
| mem_reads | `oltp_range_select` | 19.50ms | 21.42ms | 1.098× | 0.90% | PASS |
| mem_reads | `oltp_sum_range` | 18.31ms | 21.00ms | 1.147× | 1.23% | PASS |
| mem_reads | `oltp_order_range` | 3.58ms | 3.89ms | 1.085× | 1.13% | PASS |
| mem_reads | `oltp_distinct_range` | 4.69ms | 4.93ms | 1.052× | 1.17% | PASS |
| mem_reads | `oltp_index_scan` | 4.60ms | 6.26ms | 1.362× | 1.09% | PASS |
| mem_reads | `select_random_points` | 28.28ms | 32.05ms | 1.134× | 1.49% | PASS |
| mem_reads | `select_random_ranges` | 7.75ms | 8.96ms | 1.157× | 1.20% | PASS |
| mem_reads | `covering_index_scan` | 4.21ms | 4.27ms | 1.015× | 1.23% | PASS |
| mem_reads | `groupby_scan` | 36.17ms | 39.19ms | 1.083× | 0.99% | PASS |
| mem_reads | `index_join` | 8.01ms | 10.20ms | 1.273× | 1.13% | PASS |
| mem_reads | `index_join_scan` | 3.99ms | 5.28ms | 1.323× | 4.36% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.23s | 1.187× | 0.63% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.38s | 1.175× | 0.42% | PASS |
| mem_reads | `oltp_read_only` | 144.88ms | 167.93ms | 1.159× | 0.63% | PASS |
| mem_writes | `oltp_bulk_insert` | 243.94ms | 349.61ms | 1.433× | 0.83% | PASS |
| mem_writes | `oltp_insert` | 18.99ms | 35.90ms | 1.891× | 0.59% | PASS |
| mem_writes | `oltp_update_index` | 65.06ms | 113.37ms | 1.743× | 0.70% | PASS |
| mem_writes | `oltp_update_non_index` | 48.05ms | 80.50ms | 1.675× | 0.95% | PASS |
| mem_writes | `oltp_delete_insert` | 47.66ms | 93.59ms | 1.964× | 0.88% | PASS |
| mem_writes | `oltp_write_only` | 25.62ms | 56.10ms | 2.190× | 0.91% | PASS |
| mem_writes | `types_delete_insert` | 31.05ms | 53.27ms | 1.715× | 0.75% | PASS |
| mem_writes | `oltp_read_write` | 96.78ms | 151.97ms | 1.570× | 0.68% | PASS |
| file_reads | `oltp_point_select` | 105.34ms | 65.75ms | 0.624× | 0.89% | PASS |
| file_reads | `oltp_range_select` | 25.43ms | 24.19ms | 0.951× | 2.28% | PASS |
| file_reads | `oltp_sum_range` | 24.73ms | 23.88ms | 0.965× | 1.27% | PASS |
| file_reads | `oltp_order_range` | 4.22ms | 4.21ms | 0.996× | 2.55% | PASS |
| file_reads | `oltp_distinct_range` | 5.40ms | 5.30ms | 0.982× | 1.86% | PASS |
| file_reads | `oltp_index_scan` | 11.94ms | 9.05ms | 0.758× | 2.44% | PASS |
| file_reads | `select_random_points` | 35.75ms | 35.68ms | 0.998× | 1.94% | PASS |
| file_reads | `select_random_ranges` | 14.95ms | 11.89ms | 0.795× | 1.29% | PASS |
| file_reads | `covering_index_scan` | 11.69ms | 7.05ms | 0.603× | 1.51% | PASS |
| file_reads | `groupby_scan` | 37.04ms | 39.80ms | 1.075× | 1.16% | PASS |
| file_reads | `index_join` | 12.25ms | 12.52ms | 1.022× | 1.32% | PASS |
| file_reads | `index_join_scan` | 5.05ms | 5.81ms | 1.151× | 1.45% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.23s | 1.187× | 0.74% | PASS |
| file_reads | `table_scan` | 1.19s | 1.38s | 1.164× | 1.11% | PASS |
| file_reads | `oltp_read_only` | 263.20ms | 209.69ms | 0.797× | 1.15% | PASS |
| file_writes | `oltp_bulk_insert` | 258.52ms | 375.97ms | 1.454× | 0.86% | PASS |
| file_writes | `oltp_insert` | 26.11ms | 46.57ms | 1.783× | 1.36% | PASS |
| file_writes | `oltp_update_index` | 98.89ms | 145.29ms | 1.469× | 1.00% | PASS |
| file_writes | `oltp_update_non_index` | 77.61ms | 105.16ms | 1.355× | 1.67% | PASS |
| file_writes | `oltp_delete_insert` | 77.51ms | 120.55ms | 1.555× | 1.04% | PASS |
| file_writes | `oltp_write_only` | 51.50ms | 78.04ms | 1.515× | 1.77% | PASS |
| file_writes | `types_delete_insert` | 50.02ms | 68.95ms | 1.378× | 1.44% | PASS |
| file_writes | `oltp_read_write` | 127.08ms | 175.39ms | 1.380× | 1.96% | PASS |
| ac_reads | `oltp_point_select` | 56.41ms | 65.91ms | 1.168× | 1.29% | PASS |
| ac_reads | `oltp_range_select` | 21.41ms | 24.23ms | 1.131× | 1.60% | PASS |
| ac_reads | `oltp_sum_range` | 20.18ms | 23.87ms | 1.183× | 0.79% | PASS |
| ac_reads | `oltp_order_range` | 3.80ms | 4.21ms | 1.107× | 1.41% | PASS |
| ac_reads | `oltp_distinct_range` | 4.95ms | 5.26ms | 1.064× | 1.37% | PASS |
| ac_reads | `oltp_index_scan` | 7.01ms | 9.04ms | 1.291× | 1.30% | PASS |
| ac_reads | `select_random_points` | 30.43ms | 35.72ms | 1.174× | 1.65% | PASS |
| ac_reads | `select_random_ranges` | 10.02ms | 11.93ms | 1.191× | 1.07% | PASS |
| ac_reads | `covering_index_scan` | 6.75ms | 7.06ms | 1.046× | 1.07% | PASS |
| ac_reads | `groupby_scan` | 36.29ms | 39.77ms | 1.096× | 0.84% | PASS |
| ac_reads | `index_join` | 9.67ms | 12.57ms | 1.300× | 1.20% | PASS |
| ac_reads | `index_join_scan` | 4.53ms | 5.84ms | 1.288× | 1.47% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.23s | 1.189× | 0.32% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.38s | 1.172× | 0.82% | PASS |
| ac_reads | `oltp_read_only` | 182.34ms | 207.54ms | 1.138× | 0.79% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.71ms | 65.67ms | 2.891× | 4.42% | PASS |
| ac_writes | `oltp_insert_ac` | 25.26ms | 87.55ms | 3.466× | 6.40% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.83ms | 98.40ms | 3.667× | 4.91% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.13ms | 77.60ms | 3.355× | 4.30% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.67ms | 88.46ms | 3.585× | 4.24% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.15ms | 88.10ms | 3.503× | 4.57% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.33ms | 76.21ms | 3.267× | 7.03% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.25ms | 97.41ms | 3.117× | 4.34% | PASS |

</details>

## Version-control latency

Wall time: 2m 18s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 82.34ms | 200.00ms | 41.2% | 0.32% | PASS |
| `status_dirty_many_tables` | 85.17ms | 200.00ms | 42.6% | 0.38% | PASS |
| `diff_regular_working_one_table` | 77.75ms | 150.00ms | 51.8% | 0.45% | PASS |
| `diff_regular_working_many_tables` | 90.62ms | 200.00ms | 45.3% | 0.36% | PASS |
| `diff_stat_working_many_tables` | 90.55ms | 200.00ms | 45.3% | 0.34% | PASS |
| `diff_schema_working_many_tables` | 90.98ms | 200.00ms | 45.5% | 0.29% | PASS |
| `branch_list_many_branches` | 22.03ms | 100.00ms | 22.0% | 0.80% | PASS |
| `branch_create_delete` | 24.07ms | 100.00ms | 24.1% | 1.06% | PASS |
| `checkout_branch_clean` | 54.17ms | 200.00ms | 27.1% | 1.12% | PASS |
| `merge_data_no_conflicts` | 28.10ms | 150.00ms | 18.7% | 1.09% | PASS |
| `merge_schema_no_conflicts` | 21.68ms | 100.00ms | 21.7% | 1.38% | PASS |
| `merge_data_conflicts` | 126.18ms | 250.00ms | 50.5% | 0.27% | PASS |
| `merge_data_conflicts_with_resolve` | 126.08ms | 250.00ms | 50.4% | 0.29% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
