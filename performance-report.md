# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-25 16:24 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260920.314.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/36149240455)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 41s | 9.11s | 11.51s | 1.264× | 1.83% | **PASS** |
| textpk | 69 | 55 | 1h 38m 18s | 11.56s | 12.33s | 1.067× | 1.91% | **PASS** |
| blobpk | 69 | 55 | 1h 29m 39s | 9.98s | 11.48s | 1.150× | 1.01% | **PASS** |
| compositepk | 69 | 55 | 1h 13m 12s | 8.12s | 9.72s | 1.196× | 1.06% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.48ms | 29.44ms | 1.203× | 2.31% | PASS |
| mem_reads | `oltp_range_select` | 10.68ms | 13.24ms | 1.240× | 2.58% | PASS |
| mem_reads | `oltp_sum_range` | 9.98ms | 12.45ms | 1.248× | 1.63% | PASS |
| mem_reads | `oltp_order_range` | 2.61ms | 3.04ms | 1.165× | 1.89% | PASS |
| mem_reads | `oltp_distinct_range` | 3.67ms | 4.07ms | 1.109× | 1.36% | PASS |
| mem_reads | `oltp_index_scan` | 3.92ms | 5.51ms | 1.406× | 1.95% | PASS |
| mem_reads | `select_random_points` | 10.68ms | 11.38ms | 1.065× | 3.30% | PASS |
| mem_reads | `select_random_ranges` | 3.00ms | 3.98ms | 1.324× | 1.78% | PASS |
| mem_reads | `covering_index_scan` | 4.25ms | 4.34ms | 1.020× | 2.53% | PASS |
| mem_reads | `groupby_scan` | 29.98ms | 33.41ms | 1.114× | 0.62% | PASS |
| mem_reads | `index_join` | 6.05ms | 8.35ms | 1.379× | 2.13% | PASS |
| mem_reads | `index_join_scan` | 3.59ms | 4.59ms | 1.278× | 2.77% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.34s | 1.272× | 1.42% | PASS |
| mem_reads | `table_scan` | 1.23s | 1.41s | 1.150× | 1.21% | PASS |
| mem_reads | `oltp_read_only` | 105.68ms | 124.64ms | 1.179× | 1.21% | PASS |
| mem_writes | `oltp_bulk_insert` | 176.83ms | 249.88ms | 1.413× | 0.98% | PASS |
| mem_writes | `oltp_insert` | 15.37ms | 28.49ms | 1.854× | 1.02% | PASS |
| mem_writes | `oltp_update_index` | 53.18ms | 111.01ms | 2.087× | 1.78% | PASS |
| mem_writes | `oltp_update_non_index` | 36.08ms | 62.14ms | 1.722× | 1.50% | PASS |
| mem_writes | `oltp_delete_insert` | 47.01ms | 81.91ms | 1.742× | 1.96% | PASS |
| mem_writes | `oltp_write_only` | 22.41ms | 45.55ms | 2.032× | 1.14% | PASS |
| mem_writes | `types_delete_insert` | 25.08ms | 40.64ms | 1.621× | 1.89% | PASS |
| mem_writes | `oltp_read_write` | 69.17ms | 112.13ms | 1.621× | 1.65% | PASS |
| file_reads | `oltp_point_select` | 97.86ms | 55.50ms | 0.567× | 1.15% | PASS |
| file_reads | `oltp_range_select` | 18.62ms | 15.97ms | 0.858× | 2.07% | PASS |
| file_reads | `oltp_sum_range` | 17.97ms | 15.32ms | 0.853× | 1.85% | PASS |
| file_reads | `oltp_order_range` | 3.62ms | 3.49ms | 0.965× | 5.73% | PASS |
| file_reads | `oltp_distinct_range` | 4.76ms | 4.48ms | 0.940× | 4.48% | PASS |
| file_reads | `oltp_index_scan` | 11.77ms | 8.42ms | 0.716× | 1.46% | PASS |
| file_reads | `select_random_points` | 18.40ms | 14.06ms | 0.764× | 1.65% | PASS |
| file_reads | `select_random_ranges` | 10.52ms | 6.59ms | 0.627× | 1.13% | PASS |
| file_reads | `covering_index_scan` | 12.31ms | 7.14ms | 0.580× | 2.28% | PASS |
| file_reads | `groupby_scan` | 31.26ms | 34.07ms | 1.090× | 1.45% | PASS |
| file_reads | `index_join` | 10.64ms | 10.40ms | 0.978× | 1.63% | PASS |
| file_reads | `index_join_scan` | 4.62ms | 5.05ms | 1.093× | 2.38% | PASS |
| file_reads | `types_table_scan` | 1.07s | 1.34s | 1.262× | 1.07% | PASS |
| file_reads | `table_scan` | 1.24s | 1.42s | 1.140× | 1.02% | PASS |
| file_reads | `oltp_read_only` | 216.75ms | 164.28ms | 0.758× | 0.93% | PASS |
| file_writes | `oltp_bulk_insert` | 190.35ms | 268.57ms | 1.411× | 1.28% | PASS |
| file_writes | `oltp_insert` | 22.34ms | 35.65ms | 1.596× | 2.25% | PASS |
| file_writes | `oltp_update_index` | 78.70ms | 129.13ms | 1.641× | 1.59% | PASS |
| file_writes | `oltp_update_non_index` | 59.09ms | 82.37ms | 1.394× | 1.55% | PASS |
| file_writes | `oltp_delete_insert` | 71.27ms | 101.37ms | 1.422× | 2.03% | PASS |
| file_writes | `oltp_write_only` | 47.11ms | 66.22ms | 1.406× | 1.91% | PASS |
| file_writes | `types_delete_insert` | 42.00ms | 55.22ms | 1.315× | 1.83% | PASS |
| file_writes | `oltp_read_write` | 98.26ms | 134.46ms | 1.368× | 1.54% | PASS |
| ac_reads | `oltp_point_select` | 49.50ms | 56.18ms | 1.135× | 1.24% | PASS |
| ac_reads | `oltp_range_select` | 14.13ms | 16.27ms | 1.152× | 2.83% | PASS |
| ac_reads | `oltp_sum_range` | 13.29ms | 15.59ms | 1.173× | 2.28% | PASS |
| ac_reads | `oltp_order_range` | 3.06ms | 3.46ms | 1.131× | 4.72% | PASS |
| ac_reads | `oltp_distinct_range` | 4.12ms | 4.52ms | 1.095× | 5.34% | PASS |
| ac_reads | `oltp_index_scan` | 6.61ms | 8.33ms | 1.260× | 1.44% | PASS |
| ac_reads | `select_random_points` | 13.24ms | 13.94ms | 1.053× | 3.42% | PASS |
| ac_reads | `select_random_ranges` | 5.54ms | 6.57ms | 1.187× | 0.97% | PASS |
| ac_reads | `covering_index_scan` | 7.06ms | 7.08ms | 1.003× | 2.41% | PASS |
| ac_reads | `groupby_scan` | 30.51ms | 34.16ms | 1.120× | 1.08% | PASS |
| ac_reads | `index_join` | 7.92ms | 10.45ms | 1.319× | 1.83% | PASS |
| ac_reads | `index_join_scan` | 3.98ms | 4.98ms | 1.250× | 2.28% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.34s | 1.268× | 1.10% | PASS |
| ac_reads | `table_scan` | 1.22s | 1.41s | 1.150× | 0.94% | PASS |
| ac_reads | `oltp_read_only` | 138.24ms | 161.52ms | 1.168× | 1.11% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.34ms | 69.55ms | 2.980× | 8.33% | PASS |
| ac_writes | `oltp_insert_ac` | 24.94ms | 86.42ms | 3.466× | 4.01% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.14ms | 102.52ms | 3.777× | 5.07% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.01ms | 78.29ms | 3.402× | 7.03% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.26ms | 91.61ms | 3.626× | 6.87% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.70ms | 89.03ms | 3.604× | 5.36% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.05ms | 80.61ms | 3.498× | 6.87% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.63ms | 96.17ms | 3.246× | 4.69% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.79ms | 38.90ms | 1.224× | 1.79% | PASS |
| mem_reads | `oltp_range_select` | 14.81ms | 14.65ms | 0.989× | 2.03% | PASS |
| mem_reads | `oltp_sum_range` | 12.88ms | 14.52ms | 1.127× | 2.49% | PASS |
| mem_reads | `oltp_order_range` | 3.03ms | 3.23ms | 1.067× | 1.21% | PASS |
| mem_reads | `oltp_distinct_range` | 4.08ms | 4.29ms | 1.050× | 1.15% | PASS |
| mem_reads | `oltp_index_scan` | 4.70ms | 6.61ms | 1.404× | 2.02% | PASS |
| mem_reads | `select_random_points` | 19.30ms | 22.09ms | 1.145× | 1.60% | PASS |
| mem_reads | `select_random_ranges` | 4.21ms | 5.34ms | 1.268× | 1.37% | PASS |
| mem_reads | `covering_index_scan` | 5.46ms | 4.87ms | 0.891× | 4.25% | PASS |
| mem_reads | `groupby_scan` | 32.87ms | 34.75ms | 1.057× | 0.95% | PASS |
| mem_reads | `index_join` | 7.65ms | 10.06ms | 1.316× | 3.35% | PASS |
| mem_reads | `index_join_scan` | 4.81ms | 5.68ms | 1.182× | 2.75% | PASS |
| mem_reads | `types_table_scan` | 1.27s | 1.28s | 1.012× | 1.67% | PASS |
| mem_reads | `table_scan` | 1.58s | 1.43s | 0.908× | 0.85% | PASS |
| mem_reads | `oltp_read_only` | 132.83ms | 145.13ms | 1.093× | 1.19% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.25ms | 364.38ms | 1.542× | 0.64% | PASS |
| mem_writes | `oltp_insert` | 23.82ms | 41.13ms | 1.726× | 1.76% | PASS |
| mem_writes | `oltp_update_index` | 80.10ms | 143.11ms | 1.787× | 2.18% | PASS |
| mem_writes | `oltp_update_non_index` | 51.64ms | 90.32ms | 1.749× | 1.22% | PASS |
| mem_writes | `oltp_delete_insert` | 56.94ms | 109.58ms | 1.924× | 2.08% | PASS |
| mem_writes | `oltp_write_only` | 32.16ms | 64.81ms | 2.015× | 2.26% | PASS |
| mem_writes | `types_delete_insert` | 35.34ms | 57.91ms | 1.638× | 1.47% | PASS |
| mem_writes | `oltp_read_write` | 96.01ms | 148.89ms | 1.551× | 1.95% | PASS |
| file_reads | `oltp_point_select` | 109.48ms | 66.12ms | 0.604× | 1.05% | PASS |
| file_reads | `oltp_range_select` | 22.94ms | 17.38ms | 0.758× | 1.91% | PASS |
| file_reads | `oltp_sum_range` | 21.73ms | 17.79ms | 0.818× | 2.04% | PASS |
| file_reads | `oltp_order_range` | 4.04ms | 3.60ms | 0.891× | 1.76% | PASS |
| file_reads | `oltp_distinct_range` | 5.13ms | 4.67ms | 0.910× | 1.51% | PASS |
| file_reads | `oltp_index_scan` | 12.92ms | 9.35ms | 0.724× | 1.46% | PASS |
| file_reads | `select_random_points` | 28.95ms | 25.61ms | 0.885× | 1.64% | PASS |
| file_reads | `select_random_ranges` | 11.99ms | 7.98ms | 0.665× | 1.38% | PASS |
| file_reads | `covering_index_scan` | 13.77ms | 7.54ms | 0.548× | 1.99% | PASS |
| file_reads | `groupby_scan` | 34.36ms | 35.58ms | 1.036× | 1.02% | PASS |
| file_reads | `index_join` | 13.28ms | 12.41ms | 0.934× | 2.65% | PASS |
| file_reads | `index_join_scan` | 6.49ms | 6.53ms | 1.007× | 5.70% | PASS |
| file_reads | `types_table_scan` | 1.28s | 1.29s | 1.005× | 1.19% | PASS |
| file_reads | `table_scan` | 1.58s | 1.43s | 0.909× | 0.68% | PASS |
| file_reads | `oltp_read_only` | 249.57ms | 185.34ms | 0.743× | 0.96% | PASS |
| file_writes | `oltp_bulk_insert` | 257.94ms | 403.40ms | 1.564× | 1.40% | PASS |
| file_writes | `oltp_insert` | 57.75ms | 55.14ms | 0.955× | 18.93% | PASS |
| file_writes | `oltp_update_index` | 133.21ms | 188.64ms | 1.416× | 2.33% | PASS |
| file_writes | `oltp_update_non_index` | 109.34ms | 122.39ms | 1.119× | 6.41% | PASS |
| file_writes | `oltp_delete_insert` | 100.85ms | 143.09ms | 1.419× | 2.06% | PASS |
| file_writes | `oltp_write_only` | 90.95ms | 91.03ms | 1.001× | 9.60% | PASS |
| file_writes | `types_delete_insert` | 60.41ms | 79.30ms | 1.313× | 1.66% | PASS |
| file_writes | `oltp_read_write` | 164.85ms | 178.39ms | 1.082× | 6.30% | PASS |
| ac_reads | `oltp_point_select` | 58.87ms | 67.34ms | 1.144× | 1.45% | PASS |
| ac_reads | `oltp_range_select` | 18.50ms | 17.91ms | 0.968× | 1.99% | PASS |
| ac_reads | `oltp_sum_range` | 16.77ms | 17.99ms | 1.072× | 2.12% | PASS |
| ac_reads | `oltp_order_range` | 3.54ms | 3.65ms | 1.032× | 1.82% | PASS |
| ac_reads | `oltp_distinct_range` | 4.61ms | 4.72ms | 1.024× | 1.36% | PASS |
| ac_reads | `oltp_index_scan` | 7.92ms | 9.54ms | 1.204× | 1.58% | PASS |
| ac_reads | `select_random_points` | 24.27ms | 27.31ms | 1.125× | 2.22% | PASS |
| ac_reads | `select_random_ranges` | 7.23ms | 8.17ms | 1.130× | 1.45% | PASS |
| ac_reads | `covering_index_scan` | 9.12ms | 7.70ms | 0.844× | 2.21% | PASS |
| ac_reads | `groupby_scan` | 33.66ms | 35.75ms | 1.062× | 0.84% | PASS |
| ac_reads | `index_join` | 10.66ms | 12.31ms | 1.154× | 5.59% | PASS |
| ac_reads | `index_join_scan` | 5.61ms | 6.39ms | 1.139× | 4.43% | PASS |
| ac_reads | `types_table_scan` | 1.28s | 1.29s | 1.004× | 1.01% | PASS |
| ac_reads | `table_scan` | 1.58s | 1.44s | 0.916× | 0.71% | PASS |
| ac_reads | `oltp_read_only` | 176.66ms | 189.31ms | 1.072× | 1.03% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.60ms | 76.15ms | 3.227× | 6.04% | PASS |
| ac_writes | `oltp_insert_ac` | 27.65ms | 92.61ms | 3.350× | 6.12% | PASS |
| ac_writes | `oltp_update_index_ac` | 32.46ms | 114.14ms | 3.517× | 8.02% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.28ms | 89.64ms | 3.546× | 7.46% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.01ms | 100.33ms | 3.857× | 7.87% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.62ms | 95.89ms | 3.472× | 7.83% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.15ms | 92.29ms | 3.987× | 11.12% | PASS |
| ac_writes | `oltp_read_write_ac` | 34.67ms | 105.34ms | 3.039× | 6.94% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.67ms | 34.91ms | 1.138× | 1.65% | PASS |
| mem_reads | `oltp_range_select` | 13.54ms | 13.57ms | 1.003× | 1.99% | PASS |
| mem_reads | `oltp_sum_range` | 12.20ms | 13.09ms | 1.073× | 1.72% | PASS |
| mem_reads | `oltp_order_range` | 3.05ms | 3.12ms | 1.023× | 1.37% | PASS |
| mem_reads | `oltp_distinct_range` | 4.12ms | 4.16ms | 1.009× | 0.99% | PASS |
| mem_reads | `oltp_index_scan` | 4.61ms | 5.75ms | 1.247× | 1.36% | PASS |
| mem_reads | `select_random_points` | 17.23ms | 19.58ms | 1.136× | 1.40% | PASS |
| mem_reads | `select_random_ranges` | 4.19ms | 5.17ms | 1.233× | 1.08% | PASS |
| mem_reads | `covering_index_scan` | 4.58ms | 4.35ms | 0.950× | 1.36% | PASS |
| mem_reads | `groupby_scan` | 33.66ms | 35.21ms | 1.046× | 0.84% | PASS |
| mem_reads | `index_join` | 6.94ms | 9.09ms | 1.311× | 2.49% | PASS |
| mem_reads | `index_join_scan` | 4.41ms | 5.55ms | 1.260× | 1.81% | PASS |
| mem_reads | `types_table_scan` | 1.12s | 1.24s | 1.104× | 0.73% | PASS |
| mem_reads | `table_scan` | 1.30s | 1.36s | 1.041× | 0.56% | PASS |
| mem_reads | `oltp_read_only` | 117.34ms | 127.34ms | 1.085× | 0.75% | PASS |
| mem_writes | `oltp_bulk_insert` | 238.75ms | 332.37ms | 1.392× | 0.66% | PASS |
| mem_writes | `oltp_insert` | 20.53ms | 39.00ms | 1.900× | 0.84% | PASS |
| mem_writes | `oltp_update_index` | 69.05ms | 130.41ms | 1.889× | 0.80% | PASS |
| mem_writes | `oltp_update_non_index` | 49.30ms | 84.78ms | 1.720× | 0.94% | PASS |
| mem_writes | `oltp_delete_insert` | 49.52ms | 103.79ms | 2.096× | 0.88% | PASS |
| mem_writes | `oltp_write_only` | 28.15ms | 62.99ms | 2.237× | 0.91% | PASS |
| mem_writes | `types_delete_insert` | 32.33ms | 52.58ms | 1.626× | 0.99% | PASS |
| mem_writes | `oltp_read_write` | 81.51ms | 133.75ms | 1.641× | 1.01% | PASS |
| file_reads | `oltp_point_select` | 126.70ms | 66.49ms | 0.525× | 0.84% | PASS |
| file_reads | `oltp_range_select` | 23.77ms | 16.81ms | 0.707× | 0.87% | PASS |
| file_reads | `oltp_sum_range` | 22.30ms | 16.32ms | 0.732× | 1.31% | PASS |
| file_reads | `oltp_order_range` | 4.15ms | 3.52ms | 0.849× | 0.91% | PASS |
| file_reads | `oltp_distinct_range` | 5.18ms | 4.52ms | 0.873× | 0.87% | PASS |
| file_reads | `oltp_index_scan` | 14.66ms | 9.36ms | 0.638× | 0.76% | PASS |
| file_reads | `select_random_points` | 28.20ms | 23.03ms | 0.817× | 1.05% | PASS |
| file_reads | `select_random_ranges` | 14.04ms | 8.49ms | 0.605× | 0.49% | PASS |
| file_reads | `covering_index_scan` | 15.12ms | 7.93ms | 0.525× | 0.80% | PASS |
| file_reads | `groupby_scan` | 34.70ms | 35.51ms | 1.023× | 0.44% | PASS |
| file_reads | `index_join` | 12.66ms | 11.37ms | 0.898× | 1.37% | PASS |
| file_reads | `index_join_scan` | 5.54ms | 6.08ms | 1.097× | 1.38% | PASS |
| file_reads | `types_table_scan` | 1.12s | 1.24s | 1.100× | 0.52% | PASS |
| file_reads | `table_scan` | 1.32s | 1.35s | 1.030× | 1.55% | PASS |
| file_reads | `oltp_read_only` | 258.23ms | 174.48ms | 0.676× | 0.87% | PASS |
| file_writes | `oltp_bulk_insert` | 261.17ms | 359.31ms | 1.376× | 0.73% | PASS |
| file_writes | `oltp_insert` | 31.77ms | 52.08ms | 1.639× | 1.52% | PASS |
| file_writes | `oltp_update_index` | 106.78ms | 165.63ms | 1.551× | 0.83% | PASS |
| file_writes | `oltp_update_non_index` | 80.67ms | 110.00ms | 1.364× | 1.22% | PASS |
| file_writes | `oltp_delete_insert` | 83.28ms | 132.65ms | 1.593× | 1.97% | PASS |
| file_writes | `oltp_write_only` | 57.85ms | 86.97ms | 1.503× | 1.70% | PASS |
| file_writes | `types_delete_insert` | 52.92ms | 71.45ms | 1.350× | 1.16% | PASS |
| file_writes | `oltp_read_write` | 111.49ms | 156.80ms | 1.406× | 0.90% | PASS |
| ac_reads | `oltp_point_select` | 61.81ms | 66.44ms | 1.075× | 0.94% | PASS |
| ac_reads | `oltp_range_select` | 16.95ms | 16.79ms | 0.990× | 1.38% | PASS |
| ac_reads | `oltp_sum_range` | 15.66ms | 16.31ms | 1.041× | 0.94% | PASS |
| ac_reads | `oltp_order_range` | 3.48ms | 3.50ms | 1.009× | 1.06% | PASS |
| ac_reads | `oltp_distinct_range` | 4.48ms | 4.52ms | 1.010× | 1.10% | PASS |
| ac_reads | `oltp_index_scan` | 8.13ms | 9.31ms | 1.146× | 0.88% | PASS |
| ac_reads | `select_random_points` | 21.43ms | 23.02ms | 1.074× | 0.77% | PASS |
| ac_reads | `select_random_ranges` | 7.53ms | 8.46ms | 1.123× | 0.87% | PASS |
| ac_reads | `covering_index_scan` | 8.47ms | 7.93ms | 0.936× | 0.94% | PASS |
| ac_reads | `groupby_scan` | 33.87ms | 35.57ms | 1.050× | 0.53% | PASS |
| ac_reads | `index_join` | 9.15ms | 11.21ms | 1.224× | 1.21% | PASS |
| ac_reads | `index_join_scan` | 4.90ms | 6.00ms | 1.224× | 1.04% | PASS |
| ac_reads | `types_table_scan` | 1.12s | 1.24s | 1.103× | 0.65% | PASS |
| ac_reads | `table_scan` | 1.31s | 1.35s | 1.033× | 1.04% | PASS |
| ac_reads | `oltp_read_only` | 164.84ms | 173.58ms | 1.053× | 0.70% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.45ms | 52.48ms | 3.190× | 3.99% | PASS |
| ac_writes | `oltp_insert_ac` | 17.94ms | 74.85ms | 4.173× | 4.26% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.57ms | 86.58ms | 4.423× | 3.36% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.38ms | 64.93ms | 3.963× | 3.56% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.45ms | 77.71ms | 4.212× | 4.66% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.93ms | 75.80ms | 4.003× | 4.79% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.68ms | 66.40ms | 4.235× | 3.93% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.21ms | 84.42ms | 3.487× | 3.55% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.30ms | 27.77ms | 1.143× | 0.90% | PASS |
| mem_reads | `oltp_range_select` | 15.46ms | 16.19ms | 1.048× | 0.78% | PASS |
| mem_reads | `oltp_sum_range` | 14.65ms | 15.19ms | 1.036× | 0.63% | PASS |
| mem_reads | `oltp_order_range` | 2.96ms | 3.11ms | 1.052× | 0.73% | PASS |
| mem_reads | `oltp_distinct_range` | 3.78ms | 3.89ms | 1.030× | 0.45% | PASS |
| mem_reads | `oltp_index_scan` | 3.60ms | 4.58ms | 1.272× | 0.93% | PASS |
| mem_reads | `select_random_points` | 22.61ms | 25.36ms | 1.122× | 0.80% | PASS |
| mem_reads | `select_random_ranges` | 5.69ms | 6.50ms | 1.144× | 0.81% | PASS |
| mem_reads | `covering_index_scan` | 2.96ms | 3.14ms | 1.061× | 1.69% | PASS |
| mem_reads | `groupby_scan` | 30.19ms | 30.54ms | 1.011× | 0.58% | PASS |
| mem_reads | `index_join` | 6.37ms | 8.14ms | 1.279× | 0.85% | PASS |
| mem_reads | `index_join_scan` | 3.14ms | 4.57ms | 1.455× | 0.83% | PASS |
| mem_reads | `types_table_scan` | 878.43ms | 1.03s | 1.170× | 0.72% | PASS |
| mem_reads | `table_scan` | 1.00s | 1.12s | 1.113× | 1.14% | PASS |
| mem_reads | `oltp_read_only` | 114.30ms | 123.78ms | 1.083× | 1.01% | PASS |
| mem_writes | `oltp_bulk_insert` | 170.76ms | 235.39ms | 1.378× | 0.80% | PASS |
| mem_writes | `oltp_insert` | 13.95ms | 25.24ms | 1.809× | 0.70% | PASS |
| mem_writes | `oltp_update_index` | 51.08ms | 89.57ms | 1.754× | 1.46% | PASS |
| mem_writes | `oltp_update_non_index` | 36.23ms | 59.35ms | 1.638× | 0.88% | PASS |
| mem_writes | `oltp_delete_insert` | 36.61ms | 68.16ms | 1.862× | 0.99% | PASS |
| mem_writes | `oltp_write_only` | 20.10ms | 41.33ms | 2.056× | 0.97% | PASS |
| mem_writes | `types_delete_insert` | 23.12ms | 38.13ms | 1.649× | 0.87% | PASS |
| mem_writes | `oltp_read_write` | 72.75ms | 108.62ms | 1.493× | 1.11% | PASS |
| file_reads | `oltp_point_select` | 50.71ms | 36.71ms | 0.724× | 0.66% | PASS |
| file_reads | `oltp_range_select` | 18.30ms | 17.31ms | 0.946× | 0.78% | PASS |
| file_reads | `oltp_sum_range` | 17.64ms | 16.35ms | 0.927× | 1.01% | PASS |
| file_reads | `oltp_order_range` | 3.39ms | 3.28ms | 0.967× | 0.88% | PASS |
| file_reads | `oltp_distinct_range` | 4.23ms | 4.07ms | 0.962× | 0.96% | PASS |
| file_reads | `oltp_index_scan` | 6.57ms | 5.92ms | 0.902× | 1.24% | PASS |
| file_reads | `select_random_points` | 25.73ms | 26.52ms | 1.031× | 0.65% | PASS |
| file_reads | `select_random_ranges` | 8.59ms | 7.51ms | 0.874× | 1.15% | PASS |
| file_reads | `covering_index_scan` | 5.83ms | 4.37ms | 0.749× | 1.90% | PASS |
| file_reads | `groupby_scan` | 30.39ms | 30.64ms | 1.008× | 0.55% | PASS |
| file_reads | `index_join` | 7.95ms | 9.36ms | 1.176× | 1.95% | PASS |
| file_reads | `index_join_scan` | 3.56ms | 4.85ms | 1.363× | 1.45% | PASS |
| file_reads | `types_table_scan` | 875.24ms | 1.03s | 1.178× | 0.58% | PASS |
| file_reads | `table_scan` | 1.00s | 1.12s | 1.115× | 1.03% | PASS |
| file_reads | `oltp_read_only` | 153.00ms | 136.71ms | 0.893× | 1.11% | PASS |
| file_writes | `oltp_bulk_insert` | 222.32ms | 301.25ms | 1.355× | 1.91% | PASS |
| file_writes | `oltp_insert` | 27.83ms | 46.38ms | 1.667× | 4.06% | PASS |
| file_writes | `oltp_update_index` | 153.96ms | 158.31ms | 1.028× | 2.04% | PASS |
| file_writes | `oltp_update_non_index` | 127.63ms | 115.45ms | 0.905× | 1.76% | PASS |
| file_writes | `oltp_delete_insert` | 126.96ms | 132.13ms | 1.041× | 1.71% | PASS |
| file_writes | `oltp_write_only` | 92.57ms | 94.05ms | 1.016× | 3.18% | PASS |
| file_writes | `types_delete_insert` | 83.36ms | 84.43ms | 1.013× | 7.07% | PASS |
| file_writes | `oltp_read_write` | 144.85ms | 162.64ms | 1.123× | 2.04% | PASS |
| ac_reads | `oltp_point_select` | 33.12ms | 36.79ms | 1.111× | 0.80% | PASS |
| ac_reads | `oltp_range_select` | 16.61ms | 17.38ms | 1.047× | 1.09% | PASS |
| ac_reads | `oltp_sum_range` | 15.77ms | 16.35ms | 1.037× | 1.06% | PASS |
| ac_reads | `oltp_order_range` | 3.22ms | 3.29ms | 1.021× | 1.19% | PASS |
| ac_reads | `oltp_distinct_range` | 4.04ms | 4.07ms | 1.008× | 1.16% | PASS |
| ac_reads | `oltp_index_scan` | 4.89ms | 5.90ms | 1.207× | 1.14% | PASS |
| ac_reads | `select_random_points` | 24.17ms | 26.65ms | 1.103× | 1.20% | PASS |
| ac_reads | `select_random_ranges` | 6.90ms | 7.60ms | 1.102× | 1.34% | PASS |
| ac_reads | `covering_index_scan` | 4.15ms | 4.45ms | 1.074× | 1.42% | PASS |
| ac_reads | `groupby_scan` | 30.73ms | 30.84ms | 1.004× | 0.71% | PASS |
| ac_reads | `index_join` | 7.25ms | 9.48ms | 1.308× | 1.28% | PASS |
| ac_reads | `index_join_scan` | 3.46ms | 4.86ms | 1.404× | 0.88% | PASS |
| ac_reads | `types_table_scan` | 879.82ms | 1.03s | 1.174× | 0.70% | PASS |
| ac_reads | `table_scan` | 997.20ms | 1.11s | 1.114× | 0.43% | PASS |
| ac_reads | `oltp_read_only` | 126.92ms | 136.94ms | 1.079× | 0.94% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 25.27ms | 65.31ms | 2.584× | 4.81% | PASS |
| ac_writes | `oltp_insert_ac` | 27.31ms | 79.13ms | 2.897× | 4.94% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.99ms | 89.80ms | 3.208× | 3.74% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.82ms | 74.33ms | 2.879× | 5.65% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.69ms | 82.83ms | 3.103× | 5.92% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.47ms | 83.94ms | 3.056× | 5.89% | PASS |
| ac_writes | `types_delete_insert_ac` | 25.50ms | 76.31ms | 2.992× | 7.67% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.44ms | 89.15ms | 2.836× | 3.97% | PASS |

</details>

## Version-control latency

Wall time: 1m 49s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 49.55ms | 200.00ms | 24.8% | 4.55% | PASS |
| `status_dirty_many_tables` | 50.21ms | 200.00ms | 25.1% | 0.78% | PASS |
| `diff_regular_working_one_table` | 45.88ms | 150.00ms | 30.6% | 1.16% | PASS |
| `diff_regular_working_many_tables` | 54.94ms | 200.00ms | 27.5% | 2.56% | PASS |
| `diff_stat_working_many_tables` | 56.05ms | 200.00ms | 28.0% | 1.93% | PASS |
| `diff_schema_working_many_tables` | 54.79ms | 200.00ms | 27.4% | 1.54% | PASS |
| `branch_list_many_branches` | 15.27ms | 100.00ms | 15.3% | 1.65% | PASS |
| `branch_create_delete` | 29.21ms | 100.00ms | 29.2% | 41.93% | PASS |
| `checkout_branch_clean` | 77.07ms | 200.00ms | 38.5% | 9.59% | PASS |
| `merge_data_no_conflicts` | 64.05ms | 150.00ms | 42.7% | 55.11% | PASS |
| `merge_schema_no_conflicts` | 17.45ms | 100.00ms | 17.5% | 12.30% | PASS |
| `merge_data_conflicts` | 57.50ms | 250.00ms | 23.0% | 1.33% | PASS |
| `merge_data_conflicts_with_resolve` | 57.83ms | 250.00ms | 23.1% | 1.69% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
