# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-01 15:51 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260823.283.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/33518333173)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 20s | 9.85s | 11.24s | 1.141× | 1.47% | **PASS** |
| textpk | 69 | 55 | 1h 30m 12s | 9.62s | 11.21s | 1.165× | 1.02% | **PASS** |
| blobpk | 69 | 55 | 1h 32m 54s | 10.12s | 12.05s | 1.191× | 2.18% | **PASS** |
| compositepk | 69 | 55 | 1h 26m 53s | 9.49s | 12.01s | 1.266× | 1.33% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.69ms | 27.59ms | 1.117× | 1.72% | PASS |
| mem_reads | `oltp_range_select` | 10.52ms | 11.83ms | 1.124× | 1.79% | PASS |
| mem_reads | `oltp_sum_range` | 9.77ms | 11.32ms | 1.159× | 1.63% | PASS |
| mem_reads | `oltp_order_range` | 2.66ms | 2.84ms | 1.069× | 1.38% | PASS |
| mem_reads | `oltp_distinct_range` | 3.76ms | 3.88ms | 1.031× | 0.77% | PASS |
| mem_reads | `oltp_index_scan` | 4.00ms | 4.91ms | 1.229× | 1.60% | PASS |
| mem_reads | `select_random_points` | 10.41ms | 11.03ms | 1.060× | 2.08% | PASS |
| mem_reads | `select_random_ranges` | 3.07ms | 3.96ms | 1.289× | 1.47% | PASS |
| mem_reads | `covering_index_scan` | 4.36ms | 4.04ms | 0.927× | 1.11% | PASS |
| mem_reads | `groupby_scan` | 31.62ms | 34.16ms | 1.080× | 0.52% | PASS |
| mem_reads | `index_join` | 5.92ms | 7.63ms | 1.288× | 0.98% | PASS |
| mem_reads | `index_join_scan` | 3.44ms | 4.59ms | 1.337× | 0.96% | PASS |
| mem_reads | `types_table_scan` | 1.11s | 1.26s | 1.133× | 0.50% | PASS |
| mem_reads | `table_scan` | 1.26s | 1.37s | 1.090× | 0.59% | PASS |
| mem_reads | `oltp_read_only` | 102.45ms | 113.71ms | 1.110× | 0.74% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.70ms | 241.15ms | 1.342× | 1.07% | PASS |
| mem_writes | `oltp_insert` | 15.86ms | 28.36ms | 1.788× | 0.97% | PASS |
| mem_writes | `oltp_update_index` | 51.38ms | 105.07ms | 2.045× | 0.95% | PASS |
| mem_writes | `oltp_update_non_index` | 35.46ms | 58.54ms | 1.651× | 1.27% | PASS |
| mem_writes | `oltp_delete_insert` | 47.93ms | 81.81ms | 1.707× | 1.80% | PASS |
| mem_writes | `oltp_write_only` | 22.49ms | 45.60ms | 2.028× | 1.95% | PASS |
| mem_writes | `types_delete_insert` | 25.00ms | 39.96ms | 1.599× | 1.63% | PASS |
| mem_writes | `oltp_read_write` | 66.67ms | 104.79ms | 1.572× | 1.65% | PASS |
| file_reads | `oltp_point_select` | 120.82ms | 59.38ms | 0.491× | 0.97% | PASS |
| file_reads | `oltp_range_select` | 20.85ms | 15.16ms | 0.727× | 1.76% | PASS |
| file_reads | `oltp_sum_range` | 19.66ms | 14.74ms | 0.750× | 1.51% | PASS |
| file_reads | `oltp_order_range` | 3.70ms | 3.24ms | 0.875× | 1.15% | PASS |
| file_reads | `oltp_distinct_range` | 4.75ms | 4.23ms | 0.891× | 1.14% | PASS |
| file_reads | `oltp_index_scan` | 13.37ms | 8.35ms | 0.625× | 1.22% | PASS |
| file_reads | `select_random_points` | 19.84ms | 14.53ms | 0.732× | 2.32% | PASS |
| file_reads | `select_random_ranges` | 12.62ms | 7.19ms | 0.569× | 0.98% | PASS |
| file_reads | `covering_index_scan` | 14.02ms | 7.60ms | 0.542× | 0.72% | PASS |
| file_reads | `groupby_scan` | 32.64ms | 34.66ms | 1.062× | 1.03% | PASS |
| file_reads | `index_join` | 11.20ms | 10.07ms | 0.899× | 1.65% | PASS |
| file_reads | `index_join_scan` | 4.79ms | 5.14ms | 1.073× | 2.00% | PASS |
| file_reads | `types_table_scan` | 1.20s | 1.30s | 1.080× | 1.67% | PASS |
| file_reads | `table_scan` | 1.37s | 1.41s | 1.025× | 1.64% | PASS |
| file_reads | `oltp_read_only` | 247.41ms | 162.78ms | 0.658× | 0.82% | PASS |
| file_writes | `oltp_bulk_insert` | 195.42ms | 265.08ms | 1.356× | 0.74% | PASS |
| file_writes | `oltp_insert` | 23.11ms | 36.95ms | 1.599× | 1.27% | PASS |
| file_writes | `oltp_update_index` | 84.83ms | 138.81ms | 1.636× | 2.07% | PASS |
| file_writes | `oltp_update_non_index` | 64.99ms | 86.47ms | 1.331× | 1.86% | PASS |
| file_writes | `oltp_delete_insert` | 74.62ms | 106.59ms | 1.428× | 2.23% | PASS |
| file_writes | `oltp_write_only` | 48.50ms | 69.13ms | 1.425× | 2.14% | PASS |
| file_writes | `types_delete_insert` | 43.65ms | 56.16ms | 1.287× | 1.32% | PASS |
| file_writes | `oltp_read_write` | 98.34ms | 132.82ms | 1.351× | 2.62% | PASS |
| ac_reads | `oltp_point_select` | 58.20ms | 60.55ms | 1.040× | 0.93% | PASS |
| ac_reads | `oltp_range_select` | 15.61ms | 15.75ms | 1.009× | 1.47% | PASS |
| ac_reads | `oltp_sum_range` | 14.44ms | 15.48ms | 1.072× | 1.39% | PASS |
| ac_reads | `oltp_order_range` | 3.23ms | 3.28ms | 1.016× | 1.03% | PASS |
| ac_reads | `oltp_distinct_range` | 4.23ms | 4.26ms | 1.007× | 0.81% | PASS |
| ac_reads | `oltp_index_scan` | 7.88ms | 8.89ms | 1.129× | 1.58% | PASS |
| ac_reads | `select_random_points` | 16.59ms | 15.81ms | 0.953× | 2.62% | PASS |
| ac_reads | `select_random_ranges` | 6.54ms | 7.28ms | 1.112× | 0.82% | PASS |
| ac_reads | `covering_index_scan` | 8.14ms | 7.77ms | 0.954× | 1.43% | PASS |
| ac_reads | `groupby_scan` | 32.67ms | 34.85ms | 1.067× | 0.62% | PASS |
| ac_reads | `index_join` | 8.33ms | 10.26ms | 1.231× | 1.99% | PASS |
| ac_reads | `index_join_scan` | 4.30ms | 5.19ms | 1.207× | 3.38% | PASS |
| ac_reads | `types_table_scan` | 1.21s | 1.30s | 1.072× | 1.28% | PASS |
| ac_reads | `table_scan` | 1.39s | 1.41s | 1.017× | 0.69% | PASS |
| ac_reads | `oltp_read_only` | 157.08ms | 163.85ms | 1.043× | 1.35% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.73ms | 62.68ms | 3.985× | 4.86% | PASS |
| ac_writes | `oltp_insert_ac` | 17.74ms | 80.50ms | 4.538× | 4.34% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.87ms | 95.79ms | 4.821× | 4.08% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.45ms | 72.78ms | 4.425× | 5.75% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.05ms | 87.38ms | 4.842× | 5.21% | PASS |
| ac_writes | `oltp_write_only_ac` | 17.65ms | 83.51ms | 4.730× | 3.83% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.18ms | 74.64ms | 4.614× | 5.85% | PASS |
| ac_writes | `oltp_read_write_ac` | 23.59ms | 92.76ms | 3.932× | 4.26% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.81ms | 30.50ms | 1.138× | 0.70% | PASS |
| mem_reads | `oltp_range_select` | 12.76ms | 12.74ms | 0.999× | 0.99% | PASS |
| mem_reads | `oltp_sum_range` | 12.20ms | 12.12ms | 0.993× | 0.90% | PASS |
| mem_reads | `oltp_order_range` | 2.82ms | 2.96ms | 1.048× | 0.63% | PASS |
| mem_reads | `oltp_distinct_range` | 3.71ms | 3.84ms | 1.034× | 0.68% | PASS |
| mem_reads | `oltp_index_scan` | 4.11ms | 5.34ms | 1.301× | 0.89% | PASS |
| mem_reads | `select_random_points` | 16.94ms | 19.59ms | 1.157× | 0.96% | PASS |
| mem_reads | `select_random_ranges` | 3.46ms | 4.54ms | 1.312× | 1.00% | PASS |
| mem_reads | `covering_index_scan` | 3.92ms | 3.98ms | 1.016× | 1.13% | PASS |
| mem_reads | `groupby_scan` | 31.78ms | 31.65ms | 0.996× | 0.50% | PASS |
| mem_reads | `index_join` | 6.54ms | 8.92ms | 1.363× | 1.02% | PASS |
| mem_reads | `index_join_scan` | 4.25ms | 5.22ms | 1.228× | 0.92% | PASS |
| mem_reads | `types_table_scan` | 1.10s | 1.22s | 1.100× | 1.91% | PASS |
| mem_reads | `table_scan` | 1.26s | 1.30s | 1.032× | 2.94% | PASS |
| mem_reads | `oltp_read_only` | 104.38ms | 115.87ms | 1.110× | 0.91% | PASS |
| mem_writes | `oltp_bulk_insert` | 182.41ms | 267.87ms | 1.468× | 0.51% | PASS |
| mem_writes | `oltp_insert` | 18.36ms | 31.45ms | 1.713× | 0.85% | PASS |
| mem_writes | `oltp_update_index` | 61.06ms | 112.71ms | 1.846× | 1.18% | PASS |
| mem_writes | `oltp_update_non_index` | 40.20ms | 68.82ms | 1.712× | 0.61% | PASS |
| mem_writes | `oltp_delete_insert` | 42.58ms | 84.11ms | 1.975× | 0.61% | PASS |
| mem_writes | `oltp_write_only` | 23.95ms | 50.04ms | 2.090× | 0.66% | PASS |
| mem_writes | `types_delete_insert` | 26.96ms | 44.17ms | 1.639× | 0.66% | PASS |
| mem_writes | `oltp_read_write` | 70.29ms | 113.36ms | 1.613× | 0.78% | PASS |
| file_reads | `oltp_point_select` | 57.56ms | 41.33ms | 0.718× | 0.60% | PASS |
| file_reads | `oltp_range_select` | 15.84ms | 14.15ms | 0.893× | 0.87% | PASS |
| file_reads | `oltp_sum_range` | 15.11ms | 13.38ms | 0.886× | 0.91% | PASS |
| file_reads | `oltp_order_range` | 3.21ms | 3.12ms | 0.973× | 1.27% | PASS |
| file_reads | `oltp_distinct_range` | 4.15ms | 4.00ms | 0.963× | 0.96% | PASS |
| file_reads | `oltp_index_scan` | 7.62ms | 6.64ms | 0.872× | 1.07% | PASS |
| file_reads | `select_random_points` | 20.04ms | 20.45ms | 1.021× | 0.89% | PASS |
| file_reads | `select_random_ranges` | 6.77ms | 5.69ms | 0.840× | 1.07% | PASS |
| file_reads | `covering_index_scan` | 7.57ms | 5.22ms | 0.689× | 2.80% | PASS |
| file_reads | `groupby_scan` | 31.70ms | 31.68ms | 1.000× | 0.76% | PASS |
| file_reads | `index_join` | 8.51ms | 9.61ms | 1.129× | 2.05% | PASS |
| file_reads | `index_join_scan` | 4.56ms | 5.39ms | 1.182× | 2.39% | PASS |
| file_reads | `types_table_scan` | 1.02s | 1.18s | 1.152× | 0.22% | PASS |
| file_reads | `table_scan` | 1.19s | 1.28s | 1.078× | 0.39% | PASS |
| file_reads | `oltp_read_only` | 148.91ms | 131.92ms | 0.886× | 0.69% | PASS |
| file_writes | `oltp_bulk_insert` | 255.19ms | 357.70ms | 1.402× | 4.14% | PASS |
| file_writes | `oltp_insert` | 63.70ms | 64.85ms | 1.018× | 3.58% | PASS |
| file_writes | `oltp_update_index` | 228.00ms | 219.18ms | 0.961× | 1.01% | PASS |
| file_writes | `oltp_update_non_index` | 164.68ms | 147.54ms | 0.896× | 0.82% | PASS |
| file_writes | `oltp_delete_insert` | 186.62ms | 170.91ms | 0.916× | 1.16% | PASS |
| file_writes | `oltp_write_only` | 139.55ms | 119.27ms | 0.855× | 1.75% | PASS |
| file_writes | `types_delete_insert` | 112.98ms | 100.94ms | 0.893× | 2.38% | PASS |
| file_writes | `oltp_read_write` | 186.24ms | 183.19ms | 0.984× | 1.29% | PASS |
| ac_reads | `oltp_point_select` | 34.95ms | 40.72ms | 1.165× | 0.82% | PASS |
| ac_reads | `oltp_range_select` | 13.01ms | 13.94ms | 1.071× | 1.75% | PASS |
| ac_reads | `oltp_sum_range` | 12.30ms | 13.15ms | 1.070× | 1.63% | PASS |
| ac_reads | `oltp_order_range` | 2.87ms | 3.04ms | 1.059× | 1.19% | PASS |
| ac_reads | `oltp_distinct_range` | 3.78ms | 3.94ms | 1.043× | 1.08% | PASS |
| ac_reads | `oltp_index_scan` | 5.10ms | 6.23ms | 1.222× | 2.19% | PASS |
| ac_reads | `select_random_points` | 17.11ms | 19.79ms | 1.157× | 1.15% | PASS |
| ac_reads | `select_random_ranges` | 4.55ms | 5.59ms | 1.230× | 1.88% | PASS |
| ac_reads | `covering_index_scan` | 5.14ms | 4.88ms | 0.949× | 2.51% | PASS |
| ac_reads | `groupby_scan` | 31.23ms | 31.56ms | 1.010× | 0.66% | PASS |
| ac_reads | `index_join` | 7.61ms | 9.32ms | 1.226× | 1.46% | PASS |
| ac_reads | `index_join_scan` | 4.57ms | 5.44ms | 1.191× | 1.72% | PASS |
| ac_reads | `types_table_scan` | 1.02s | 1.18s | 1.153× | 0.25% | PASS |
| ac_reads | `table_scan` | 1.19s | 1.28s | 1.078× | 0.32% | PASS |
| ac_reads | `oltp_read_only` | 119.30ms | 132.28ms | 1.109× | 0.72% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.87ms | 86.01ms | 3.761× | 4.86% | PASS |
| ac_writes | `oltp_insert_ac` | 25.32ms | 97.36ms | 3.845× | 5.78% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.55ms | 112.16ms | 4.224× | 6.10% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.50ms | 91.80ms | 4.081× | 5.03% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.30ms | 101.05ms | 4.159× | 3.93% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.87ms | 100.13ms | 4.026× | 4.25% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.29ms | 96.25ms | 4.318× | 8.07% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.11ms | 109.55ms | 3.764× | 4.01% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.41ms | 37.57ms | 1.196× | 1.29% | PASS |
| mem_reads | `oltp_range_select` | 13.24ms | 14.11ms | 1.065× | 2.04% | PASS |
| mem_reads | `oltp_sum_range` | 12.19ms | 14.06ms | 1.153× | 2.86% | PASS |
| mem_reads | `oltp_order_range` | 2.95ms | 3.15ms | 1.066× | 1.78% | PASS |
| mem_reads | `oltp_distinct_range` | 3.99ms | 4.21ms | 1.057× | 1.81% | PASS |
| mem_reads | `oltp_index_scan` | 4.50ms | 6.22ms | 1.382× | 1.79% | PASS |
| mem_reads | `select_random_points` | 17.82ms | 20.90ms | 1.173× | 2.37% | PASS |
| mem_reads | `select_random_ranges` | 4.26ms | 5.24ms | 1.228× | 1.56% | PASS |
| mem_reads | `covering_index_scan` | 4.42ms | 4.66ms | 1.054× | 1.62% | PASS |
| mem_reads | `groupby_scan` | 31.67ms | 33.66ms | 1.063× | 0.86% | PASS |
| mem_reads | `index_join` | 6.77ms | 8.96ms | 1.325× | 2.50% | PASS |
| mem_reads | `index_join_scan` | 4.22ms | 5.40ms | 1.279× | 2.38% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.24s | 1.170× | 1.78% | PASS |
| mem_reads | `table_scan` | 1.42s | 1.41s | 0.994× | 2.28% | PASS |
| mem_reads | `oltp_read_only` | 122.50ms | 138.41ms | 1.130× | 3.40% | PASS |
| mem_writes | `oltp_bulk_insert` | 242.12ms | 357.09ms | 1.475× | 1.00% | PASS |
| mem_writes | `oltp_insert` | 20.19ms | 40.48ms | 2.005× | 1.56% | PASS |
| mem_writes | `oltp_update_index` | 71.17ms | 135.24ms | 1.900× | 4.10% | PASS |
| mem_writes | `oltp_update_non_index` | 49.62ms | 86.44ms | 1.742× | 1.54% | PASS |
| mem_writes | `oltp_delete_insert` | 50.73ms | 105.34ms | 2.076× | 2.60% | PASS |
| mem_writes | `oltp_write_only` | 28.98ms | 64.28ms | 2.218× | 2.19% | PASS |
| mem_writes | `types_delete_insert` | 33.52ms | 56.22ms | 1.677× | 1.80% | PASS |
| mem_writes | `oltp_read_write` | 89.88ms | 145.08ms | 1.614× | 2.95% | PASS |
| file_reads | `oltp_point_select` | 107.14ms | 64.28ms | 0.600× | 1.41% | PASS |
| file_reads | `oltp_range_select` | 21.98ms | 17.01ms | 0.774× | 2.91% | PASS |
| file_reads | `oltp_sum_range` | 21.42ms | 17.41ms | 0.813× | 1.75% | PASS |
| file_reads | `oltp_order_range` | 4.08ms | 3.66ms | 0.898× | 2.61% | PASS |
| file_reads | `oltp_distinct_range` | 5.17ms | 4.77ms | 0.922× | 1.64% | PASS |
| file_reads | `oltp_index_scan` | 12.84ms | 9.31ms | 0.725× | 1.48% | PASS |
| file_reads | `select_random_points` | 28.01ms | 25.14ms | 0.897× | 2.39% | PASS |
| file_reads | `select_random_ranges` | 11.99ms | 7.96ms | 0.664× | 2.34% | PASS |
| file_reads | `covering_index_scan` | 12.79ms | 7.42ms | 0.580× | 1.55% | PASS |
| file_reads | `groupby_scan` | 33.55ms | 34.73ms | 1.035× | 1.15% | PASS |
| file_reads | `index_join` | 11.95ms | 11.46ms | 0.960× | 2.97% | PASS |
| file_reads | `index_join_scan` | 5.45ms | 6.06ms | 1.113× | 2.77% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.24s | 1.164× | 1.96% | PASS |
| file_reads | `table_scan` | 1.28s | 1.39s | 1.079× | 3.46% | PASS |
| file_reads | `oltp_read_only` | 234.68ms | 176.06ms | 0.750× | 1.74% | PASS |
| file_writes | `oltp_bulk_insert` | 261.45ms | 380.62ms | 1.456× | 0.86% | PASS |
| file_writes | `oltp_insert` | 31.47ms | 52.62ms | 1.672× | 2.36% | PASS |
| file_writes | `oltp_update_index` | 103.02ms | 164.45ms | 1.596× | 1.73% | PASS |
| file_writes | `oltp_update_non_index` | 79.26ms | 108.85ms | 1.373× | 2.18% | PASS |
| file_writes | `oltp_delete_insert` | 81.39ms | 131.13ms | 1.611× | 2.07% | PASS |
| file_writes | `oltp_write_only` | 55.93ms | 84.83ms | 1.517× | 2.41% | PASS |
| file_writes | `types_delete_insert` | 52.36ms | 73.48ms | 1.403× | 2.01% | PASS |
| file_writes | `oltp_read_write` | 115.85ms | 163.37ms | 1.410× | 1.79% | PASS |
| ac_reads | `oltp_point_select` | 55.95ms | 63.48ms | 1.135× | 1.24% | PASS |
| ac_reads | `oltp_range_select` | 16.66ms | 16.86ms | 1.012× | 2.82% | PASS |
| ac_reads | `oltp_sum_range` | 15.52ms | 16.79ms | 1.082× | 2.37% | PASS |
| ac_reads | `oltp_order_range` | 3.50ms | 3.59ms | 1.025× | 1.77% | PASS |
| ac_reads | `oltp_distinct_range` | 4.56ms | 4.73ms | 1.037× | 1.74% | PASS |
| ac_reads | `oltp_index_scan` | 7.53ms | 9.18ms | 1.219× | 1.91% | PASS |
| ac_reads | `select_random_points` | 21.81ms | 24.33ms | 1.115× | 3.34% | PASS |
| ac_reads | `select_random_ranges` | 6.88ms | 7.89ms | 1.147× | 1.64% | PASS |
| ac_reads | `covering_index_scan` | 7.77ms | 7.37ms | 0.949× | 3.31% | PASS |
| ac_reads | `groupby_scan` | 32.32ms | 34.37ms | 1.064× | 0.91% | PASS |
| ac_reads | `index_join` | 9.19ms | 11.33ms | 1.233× | 2.86% | PASS |
| ac_reads | `index_join_scan` | 5.11ms | 6.24ms | 1.221× | 4.19% | PASS |
| ac_reads | `types_table_scan` | 1.17s | 1.27s | 1.087× | 1.99% | PASS |
| ac_reads | `table_scan` | 1.42s | 1.41s | 0.996× | 2.44% | PASS |
| ac_reads | `oltp_read_only` | 165.56ms | 178.70ms | 1.079× | 2.01% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.73ms | 87.31ms | 3.678× | 6.80% | PASS |
| ac_writes | `oltp_insert_ac` | 28.55ms | 114.85ms | 4.023× | 7.36% | PASS |
| ac_writes | `oltp_update_index_ac` | 31.17ms | 124.72ms | 4.001× | 6.79% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.87ms | 101.74ms | 3.933× | 8.38% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 27.46ms | 113.83ms | 4.145× | 7.14% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.98ms | 113.16ms | 4.045× | 7.52% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.24ms | 107.19ms | 4.422× | 7.73% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.26ms | 117.79ms | 3.651× | 5.32% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.53ms | 41.33ms | 1.271× | 1.03% | PASS |
| mem_reads | `oltp_range_select` | 18.61ms | 21.22ms | 1.140× | 1.35% | PASS |
| mem_reads | `oltp_sum_range` | 17.72ms | 21.02ms | 1.187× | 1.16% | PASS |
| mem_reads | `oltp_order_range` | 3.48ms | 3.85ms | 1.104× | 1.68% | PASS |
| mem_reads | `oltp_distinct_range` | 4.62ms | 4.93ms | 1.066× | 1.33% | PASS |
| mem_reads | `oltp_index_scan` | 4.46ms | 6.22ms | 1.395× | 2.20% | PASS |
| mem_reads | `select_random_points` | 27.16ms | 32.62ms | 1.201× | 1.81% | PASS |
| mem_reads | `select_random_ranges` | 7.61ms | 9.22ms | 1.212× | 1.42% | PASS |
| mem_reads | `covering_index_scan` | 4.25ms | 4.23ms | 0.995× | 1.57% | PASS |
| mem_reads | `groupby_scan` | 36.13ms | 38.73ms | 1.072× | 1.12% | PASS |
| mem_reads | `index_join` | 8.24ms | 10.42ms | 1.265× | 1.09% | PASS |
| mem_reads | `index_join_scan` | 4.08ms | 5.58ms | 1.366× | 1.74% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.24s | 1.191× | 0.63% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.39s | 1.176× | 0.59% | PASS |
| mem_reads | `oltp_read_only` | 148.69ms | 170.19ms | 1.145× | 0.66% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.16ms | 357.28ms | 1.434× | 0.83% | PASS |
| mem_writes | `oltp_insert` | 19.25ms | 36.60ms | 1.902× | 0.81% | PASS |
| mem_writes | `oltp_update_index` | 67.62ms | 116.63ms | 1.725× | 1.05% | PASS |
| mem_writes | `oltp_update_non_index` | 50.02ms | 82.88ms | 1.657× | 1.17% | PASS |
| mem_writes | `oltp_delete_insert` | 49.55ms | 95.92ms | 1.936× | 1.03% | PASS |
| mem_writes | `oltp_write_only` | 26.67ms | 57.45ms | 2.154× | 1.09% | PASS |
| mem_writes | `types_delete_insert` | 32.18ms | 54.72ms | 1.700× | 0.81% | PASS |
| mem_writes | `oltp_read_write` | 100.85ms | 156.15ms | 1.548× | 0.68% | PASS |
| file_reads | `oltp_point_select` | 108.15ms | 66.80ms | 0.618× | 0.99% | PASS |
| file_reads | `oltp_range_select` | 25.95ms | 24.30ms | 0.936× | 1.66% | PASS |
| file_reads | `oltp_sum_range` | 25.61ms | 24.20ms | 0.945× | 1.58% | PASS |
| file_reads | `oltp_order_range` | 4.37ms | 4.26ms | 0.975× | 2.49% | PASS |
| file_reads | `oltp_distinct_range` | 5.56ms | 5.37ms | 0.965× | 1.48% | PASS |
| file_reads | `oltp_index_scan` | 12.14ms | 9.23ms | 0.760× | 1.40% | PASS |
| file_reads | `select_random_points` | 36.61ms | 36.40ms | 0.994× | 0.97% | PASS |
| file_reads | `select_random_ranges` | 15.43ms | 12.09ms | 0.784× | 1.15% | PASS |
| file_reads | `covering_index_scan` | 11.85ms | 7.05ms | 0.595× | 1.95% | PASS |
| file_reads | `groupby_scan` | 37.13ms | 39.49ms | 1.063× | 1.07% | PASS |
| file_reads | `index_join` | 12.44ms | 12.68ms | 1.019× | 1.37% | PASS |
| file_reads | `index_join_scan` | 5.14ms | 6.10ms | 1.187× | 2.13% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.23s | 1.192× | 0.61% | PASS |
| file_reads | `table_scan` | 1.18s | 1.38s | 1.173× | 0.53% | PASS |
| file_reads | `oltp_read_only` | 260.79ms | 210.34ms | 0.807× | 1.12% | PASS |
| file_writes | `oltp_bulk_insert` | 264.57ms | 382.11ms | 1.444× | 1.24% | PASS |
| file_writes | `oltp_insert` | 26.13ms | 46.59ms | 1.783× | 1.24% | PASS |
| file_writes | `oltp_update_index` | 96.45ms | 143.75ms | 1.490× | 1.56% | PASS |
| file_writes | `oltp_update_non_index` | 76.32ms | 105.09ms | 1.377× | 1.61% | PASS |
| file_writes | `oltp_delete_insert` | 76.55ms | 120.52ms | 1.574× | 2.04% | PASS |
| file_writes | `oltp_write_only` | 50.68ms | 77.77ms | 1.535× | 1.87% | PASS |
| file_writes | `types_delete_insert` | 49.07ms | 68.75ms | 1.401× | 1.42% | PASS |
| file_writes | `oltp_read_write` | 125.50ms | 176.06ms | 1.403× | 1.41% | PASS |
| ac_reads | `oltp_point_select` | 57.79ms | 67.06ms | 1.160× | 0.90% | PASS |
| ac_reads | `oltp_range_select` | 21.69ms | 24.33ms | 1.122× | 0.96% | PASS |
| ac_reads | `oltp_sum_range` | 20.80ms | 24.27ms | 1.167× | 1.32% | PASS |
| ac_reads | `oltp_order_range` | 3.92ms | 4.27ms | 1.089× | 1.43% | PASS |
| ac_reads | `oltp_distinct_range` | 5.08ms | 5.38ms | 1.058× | 1.12% | PASS |
| ac_reads | `oltp_index_scan` | 7.28ms | 9.10ms | 1.251× | 1.71% | PASS |
| ac_reads | `select_random_points` | 31.12ms | 36.19ms | 1.163× | 0.88% | PASS |
| ac_reads | `select_random_ranges` | 10.38ms | 12.08ms | 1.164× | 0.95% | PASS |
| ac_reads | `covering_index_scan` | 7.02ms | 7.08ms | 1.010× | 1.38% | PASS |
| ac_reads | `groupby_scan` | 36.68ms | 39.36ms | 1.073× | 0.79% | PASS |
| ac_reads | `index_join` | 9.84ms | 12.70ms | 1.290× | 2.01% | PASS |
| ac_reads | `index_join_scan` | 4.65ms | 6.07ms | 1.306× | 1.34% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.23s | 1.189× | 0.48% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.39s | 1.171× | 0.63% | PASS |
| ac_reads | `oltp_read_only` | 186.92ms | 209.68ms | 1.122× | 0.93% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.91ms | 81.09ms | 3.539× | 7.13% | PASS |
| ac_writes | `oltp_insert_ac` | 26.19ms | 103.36ms | 3.946× | 6.64% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.43ms | 112.74ms | 4.110× | 5.80% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.99ms | 92.17ms | 4.010× | 6.63% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.45ms | 102.56ms | 4.030× | 8.33% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.91ms | 101.33ms | 3.910× | 5.67% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.55ms | 95.32ms | 4.226× | 5.89% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.46ms | 113.02ms | 3.378× | 5.36% | PASS |

</details>

## Version-control latency

Wall time: 2m 29s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 93.60ms | 200.00ms | 46.8% | 0.34% | PASS |
| `status_dirty_many_tables` | 97.00ms | 200.00ms | 48.5% | 0.33% | PASS |
| `diff_regular_working_one_table` | 89.25ms | 150.00ms | 59.5% | 0.34% | PASS |
| `diff_regular_working_many_tables` | 102.61ms | 200.00ms | 51.3% | 0.26% | PASS |
| `diff_stat_working_many_tables` | 102.43ms | 200.00ms | 51.2% | 0.23% | PASS |
| `diff_schema_working_many_tables` | 103.16ms | 200.00ms | 51.6% | 0.27% | PASS |
| `branch_list_many_branches` | 24.40ms | 100.00ms | 24.4% | 1.27% | PASS |
| `branch_create_delete` | 26.37ms | 100.00ms | 26.4% | 0.93% | PASS |
| `checkout_branch_clean` | 60.32ms | 200.00ms | 30.2% | 0.72% | PASS |
| `merge_data_no_conflicts` | 31.34ms | 150.00ms | 20.9% | 0.88% | PASS |
| `merge_schema_no_conflicts` | 23.48ms | 100.00ms | 23.5% | 1.21% | PASS |
| `merge_data_conflicts` | 129.87ms | 250.00ms | 51.9% | 0.22% | PASS |
| `merge_data_conflicts_with_resolve` | 129.59ms | 250.00ms | 51.8% | 0.17% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
