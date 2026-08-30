# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-30 16:10 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260823.283.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/33317003058)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 11m 14s | 9.22s | 10.95s | 1.188× | 0.80% | **PASS** |
| textpk | 69 | 55 | 1h 37m 50s | 10.61s | 13.95s | 1.316× | 3.09% | **PASS** |
| blobpk | 69 | 55 | 1h 18m 27s | 9.02s | 10.37s | 1.149× | 1.32% | **PASS** |
| compositepk | 69 | 55 | 1h 23m 50s | 9.40s | 11.48s | 1.221× | 1.03% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.11ms | 27.34ms | 1.134× | 0.98% | PASS |
| mem_reads | `oltp_range_select` | 10.22ms | 11.77ms | 1.152× | 0.94% | PASS |
| mem_reads | `oltp_sum_range` | 9.46ms | 11.22ms | 1.185× | 0.94% | PASS |
| mem_reads | `oltp_order_range` | 2.60ms | 2.81ms | 1.085× | 0.79% | PASS |
| mem_reads | `oltp_distinct_range` | 3.68ms | 3.82ms | 1.037× | 0.69% | PASS |
| mem_reads | `oltp_index_scan` | 3.87ms | 4.73ms | 1.223× | 1.22% | PASS |
| mem_reads | `select_random_points` | 10.07ms | 10.83ms | 1.075× | 0.83% | PASS |
| mem_reads | `select_random_ranges` | 2.98ms | 3.90ms | 1.308× | 0.83% | PASS |
| mem_reads | `covering_index_scan` | 4.34ms | 4.02ms | 0.926× | 0.66% | PASS |
| mem_reads | `groupby_scan` | 31.43ms | 34.12ms | 1.086× | 0.42% | PASS |
| mem_reads | `index_join` | 5.86ms | 7.60ms | 1.297× | 0.67% | PASS |
| mem_reads | `index_join_scan` | 3.38ms | 4.62ms | 1.365× | 0.72% | PASS |
| mem_reads | `types_table_scan` | 1.11s | 1.26s | 1.138× | 0.45% | PASS |
| mem_reads | `table_scan` | 1.24s | 1.37s | 1.098× | 0.34% | PASS |
| mem_reads | `oltp_read_only` | 101.63ms | 113.81ms | 1.120× | 0.68% | PASS |
| mem_writes | `oltp_bulk_insert` | 178.96ms | 241.39ms | 1.349× | 1.15% | PASS |
| mem_writes | `oltp_insert` | 15.71ms | 27.97ms | 1.780× | 0.94% | PASS |
| mem_writes | `oltp_update_index` | 49.78ms | 103.78ms | 2.085× | 0.71% | PASS |
| mem_writes | `oltp_update_non_index` | 34.07ms | 57.84ms | 1.698× | 0.93% | PASS |
| mem_writes | `oltp_delete_insert` | 43.68ms | 77.18ms | 1.767× | 0.71% | PASS |
| mem_writes | `oltp_write_only` | 21.31ms | 44.58ms | 2.092× | 0.65% | PASS |
| mem_writes | `types_delete_insert` | 24.16ms | 39.47ms | 1.633× | 0.75% | PASS |
| mem_writes | `oltp_read_write` | 63.61ms | 103.34ms | 1.625× | 0.87% | PASS |
| file_reads | `oltp_point_select` | 119.76ms | 59.01ms | 0.493× | 0.56% | PASS |
| file_reads | `oltp_range_select` | 20.44ms | 15.08ms | 0.738× | 0.53% | PASS |
| file_reads | `oltp_sum_range` | 19.44ms | 14.58ms | 0.750× | 0.61% | PASS |
| file_reads | `oltp_order_range` | 3.71ms | 3.20ms | 0.861× | 0.82% | PASS |
| file_reads | `oltp_distinct_range` | 4.75ms | 4.20ms | 0.883× | 0.76% | PASS |
| file_reads | `oltp_index_scan` | 13.78ms | 8.21ms | 0.596× | 1.09% | PASS |
| file_reads | `select_random_points` | 19.75ms | 14.11ms | 0.714× | 1.54% | PASS |
| file_reads | `select_random_ranges` | 12.64ms | 7.12ms | 0.563× | 0.75% | PASS |
| file_reads | `covering_index_scan` | 14.29ms | 7.32ms | 0.512× | 0.64% | PASS |
| file_reads | `groupby_scan` | 32.75ms | 34.66ms | 1.058× | 0.52% | PASS |
| file_reads | `index_join` | 11.26ms | 9.77ms | 0.867× | 0.73% | PASS |
| file_reads | `index_join_scan` | 4.57ms | 5.10ms | 1.118× | 1.64% | PASS |
| file_reads | `types_table_scan` | 1.10s | 1.26s | 1.146× | 0.41% | PASS |
| file_reads | `table_scan` | 1.24s | 1.37s | 1.104× | 0.37% | PASS |
| file_reads | `oltp_read_only` | 237.23ms | 158.65ms | 0.669× | 0.56% | PASS |
| file_writes | `oltp_bulk_insert` | 193.49ms | 259.89ms | 1.343× | 0.94% | PASS |
| file_writes | `oltp_insert` | 22.38ms | 35.80ms | 1.600× | 1.20% | PASS |
| file_writes | `oltp_update_index` | 78.46ms | 128.13ms | 1.633× | 0.75% | PASS |
| file_writes | `oltp_update_non_index` | 58.52ms | 81.10ms | 1.386× | 0.80% | PASS |
| file_writes | `oltp_delete_insert` | 67.33ms | 97.34ms | 1.446× | 0.93% | PASS |
| file_writes | `oltp_write_only` | 43.94ms | 65.23ms | 1.485× | 1.52% | PASS |
| file_writes | `types_delete_insert` | 40.65ms | 53.00ms | 1.304× | 0.97% | PASS |
| file_writes | `oltp_read_write` | 86.98ms | 124.08ms | 1.427× | 1.07% | PASS |
| ac_reads | `oltp_point_select` | 54.90ms | 58.88ms | 1.072× | 0.66% | PASS |
| ac_reads | `oltp_range_select` | 13.55ms | 15.04ms | 1.110× | 0.73% | PASS |
| ac_reads | `oltp_sum_range` | 12.70ms | 14.63ms | 1.152× | 0.64% | PASS |
| ac_reads | `oltp_order_range` | 3.01ms | 3.19ms | 1.061× | 1.36% | PASS |
| ac_reads | `oltp_distinct_range` | 4.02ms | 4.20ms | 1.045× | 0.52% | PASS |
| ac_reads | `oltp_index_scan` | 7.09ms | 8.17ms | 1.153× | 0.81% | PASS |
| ac_reads | `select_random_points` | 13.54ms | 14.34ms | 1.059× | 0.64% | PASS |
| ac_reads | `select_random_ranges` | 6.14ms | 7.09ms | 1.154× | 1.09% | PASS |
| ac_reads | `covering_index_scan` | 7.50ms | 7.30ms | 0.973× | 0.83% | PASS |
| ac_reads | `groupby_scan` | 31.92ms | 34.57ms | 1.083× | 0.46% | PASS |
| ac_reads | `index_join` | 7.69ms | 9.81ms | 1.276× | 0.92% | PASS |
| ac_reads | `index_join_scan` | 3.92ms | 5.26ms | 1.343× | 1.01% | PASS |
| ac_reads | `types_table_scan` | 1.09s | 1.25s | 1.147× | 0.57% | PASS |
| ac_reads | `table_scan` | 1.24s | 1.37s | 1.104× | 0.53% | PASS |
| ac_reads | `oltp_read_only` | 145.61ms | 159.28ms | 1.094× | 0.62% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 14.83ms | 59.58ms | 4.017× | 3.04% | PASS |
| ac_writes | `oltp_insert_ac` | 17.01ms | 76.44ms | 4.494× | 2.89% | PASS |
| ac_writes | `oltp_update_index_ac` | 18.39ms | 91.29ms | 4.966× | 2.91% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 14.97ms | 68.39ms | 4.567× | 2.40% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 16.59ms | 81.89ms | 4.935× | 2.04% | PASS |
| ac_writes | `oltp_write_only_ac` | 16.81ms | 80.75ms | 4.803× | 2.65% | PASS |
| ac_writes | `types_delete_insert_ac` | 14.64ms | 70.09ms | 4.787× | 2.83% | PASS |
| ac_writes | `oltp_read_write_ac` | 21.91ms | 87.86ms | 4.010× | 2.17% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.49ms | 25.65ms | 1.092× | 2.11% | PASS |
| mem_reads | `oltp_range_select` | 11.16ms | 10.06ms | 0.901× | 2.81% | PASS |
| mem_reads | `oltp_sum_range` | 11.25ms | 10.33ms | 0.918× | 3.32% | PASS |
| mem_reads | `oltp_order_range` | 2.31ms | 2.12ms | 0.916× | 1.72% | PASS |
| mem_reads | `oltp_distinct_range` | 2.83ms | 2.63ms | 0.929× | 1.95% | PASS |
| mem_reads | `oltp_index_scan` | 3.39ms | 4.51ms | 1.332× | 3.85% | PASS |
| mem_reads | `select_random_points` | 15.84ms | 16.50ms | 1.041× | 4.37% | PASS |
| mem_reads | `select_random_ranges` | 3.27ms | 3.74ms | 1.142× | 1.66% | PASS |
| mem_reads | `covering_index_scan` | 3.61ms | 3.53ms | 0.980× | 7.01% | PASS |
| mem_reads | `groupby_scan` | 21.07ms | 20.31ms | 0.964× | 2.40% | PASS |
| mem_reads | `index_join` | 5.35ms | 7.55ms | 1.411× | 5.68% | PASS |
| mem_reads | `index_join_scan` | 4.29ms | 4.97ms | 1.160× | 3.67% | PASS |
| mem_reads | `types_table_scan` | 761.28ms | 789.60ms | 1.037× | 3.82% | PASS |
| mem_reads | `table_scan` | 1.10s | 938.99ms | 0.857× | 4.69% | PASS |
| mem_reads | `oltp_read_only` | 86.22ms | 82.86ms | 0.961× | 2.62% | PASS |
| mem_writes | `oltp_bulk_insert` | 143.21ms | 216.11ms | 1.509× | 1.47% | PASS |
| mem_writes | `oltp_insert` | 14.86ms | 26.76ms | 1.801× | 3.30% | PASS |
| mem_writes | `oltp_update_index` | 55.97ms | 103.53ms | 1.850× | 3.67% | PASS |
| mem_writes | `oltp_update_non_index` | 38.39ms | 67.61ms | 1.761× | 1.69% | PASS |
| mem_writes | `oltp_delete_insert` | 42.01ms | 80.92ms | 1.926× | 2.73% | PASS |
| mem_writes | `oltp_write_only` | 23.97ms | 49.59ms | 2.069× | 3.04% | PASS |
| mem_writes | `types_delete_insert` | 25.21ms | 42.23ms | 1.675× | 1.40% | PASS |
| mem_writes | `oltp_read_write` | 66.49ms | 96.87ms | 1.457× | 3.05% | PASS |
| file_reads | `oltp_point_select` | 83.87ms | 46.09ms | 0.550× | 1.36% | PASS |
| file_reads | `oltp_range_select` | 17.39ms | 11.78ms | 0.677× | 3.69% | PASS |
| file_reads | `oltp_sum_range` | 16.67ms | 11.47ms | 0.688× | 2.70% | PASS |
| file_reads | `oltp_order_range` | 3.02ms | 2.37ms | 0.783× | 2.83% | PASS |
| file_reads | `oltp_distinct_range` | 3.58ms | 2.88ms | 0.803× | 1.46% | PASS |
| file_reads | `oltp_index_scan` | 9.86ms | 6.67ms | 0.677× | 1.63% | PASS |
| file_reads | `select_random_points` | 22.21ms | 18.10ms | 0.815× | 3.25% | PASS |
| file_reads | `select_random_ranges` | 9.27ms | 5.72ms | 0.617× | 1.27% | PASS |
| file_reads | `covering_index_scan` | 11.01ms | 5.65ms | 0.514× | 1.37% | PASS |
| file_reads | `groupby_scan` | 22.46ms | 21.13ms | 0.941× | 1.76% | PASS |
| file_reads | `index_join` | 9.14ms | 8.06ms | 0.882× | 1.91% | PASS |
| file_reads | `index_join_scan` | 4.45ms | 4.66ms | 1.048× | 1.91% | PASS |
| file_reads | `types_table_scan` | 765.49ms | 807.82ms | 1.055× | 4.39% | PASS |
| file_reads | `table_scan` | 919.46ms | 882.62ms | 0.960× | 5.06% | PASS |
| file_reads | `oltp_read_only` | 172.34ms | 111.66ms | 0.648× | 3.36% | PASS |
| file_writes | `oltp_bulk_insert` | 265.20ms | 333.41ms | 1.257× | 31.13% | PASS |
| file_writes | `oltp_insert` | 168.15ms | 116.06ms | 0.690× | 53.34% | PASS |
| file_writes | `oltp_update_index` | 347.02ms | 216.13ms | 0.623× | 31.59% | PASS |
| file_writes | `oltp_update_non_index` | 257.67ms | 156.86ms | 0.609× | 32.48% | PASS |
| file_writes | `oltp_delete_insert` | 325.00ms | 182.33ms | 0.561× | 42.18% | PASS |
| file_writes | `oltp_write_only` | 210.58ms | 139.36ms | 0.662× | 40.89% | PASS |
| file_writes | `types_delete_insert` | 222.04ms | 119.21ms | 0.537× | 51.58% | PASS |
| file_writes | `oltp_read_write` | 299.04ms | 184.64ms | 0.617× | 31.79% | PASS |
| ac_reads | `oltp_point_select` | 41.19ms | 43.82ms | 1.064× | 2.41% | PASS |
| ac_reads | `oltp_range_select` | 12.34ms | 11.09ms | 0.898× | 4.00% | PASS |
| ac_reads | `oltp_sum_range` | 12.50ms | 11.39ms | 0.911× | 2.02% | PASS |
| ac_reads | `oltp_order_range` | 2.68ms | 2.35ms | 0.878× | 1.39% | PASS |
| ac_reads | `oltp_distinct_range` | 3.18ms | 2.87ms | 0.900× | 1.74% | PASS |
| ac_reads | `oltp_index_scan` | 5.83ms | 6.52ms | 1.119× | 1.80% | PASS |
| ac_reads | `select_random_points` | 16.40ms | 16.50ms | 1.006× | 3.09% | PASS |
| ac_reads | `select_random_ranges` | 5.33ms | 5.61ms | 1.053× | 1.33% | PASS |
| ac_reads | `covering_index_scan` | 6.46ms | 5.50ms | 0.850× | 1.65% | PASS |
| ac_reads | `groupby_scan` | 21.24ms | 20.50ms | 0.965× | 1.23% | PASS |
| ac_reads | `index_join` | 7.05ms | 7.91ms | 1.123× | 2.61% | PASS |
| ac_reads | `index_join_scan` | 4.04ms | 4.58ms | 1.135× | 2.56% | PASS |
| ac_reads | `types_table_scan` | 726.25ms | 770.23ms | 1.061× | 2.72% | PASS |
| ac_reads | `table_scan` | 869.58ms | 870.01ms | 1.000× | 3.18% | PASS |
| ac_reads | `oltp_read_only` | 108.57ms | 107.53ms | 0.990× | 3.60% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 246.00ms | 784.61ms | 3.189× | 77.89% | PASS |
| ac_writes | `oltp_insert_ac` | 375.46ms | 842.05ms | 2.243× | 56.58% | PASS |
| ac_writes | `oltp_update_index_ac` | 299.75ms | 980.48ms | 3.271× | 57.03% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 254.19ms | 704.62ms | 2.772× | 50.20% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 276.68ms | 812.76ms | 2.938× | 55.26% | PASS |
| ac_writes | `oltp_write_only_ac` | 196.34ms | 738.17ms | 3.760× | 71.30% | PASS |
| ac_writes | `types_delete_insert_ac` | 325.40ms | 732.38ms | 2.251× | 68.06% | PASS |
| ac_writes | `oltp_read_write_ac` | 164.52ms | 470.21ms | 2.858× | 52.73% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.27ms | 31.35ms | 1.193× | 1.23% | PASS |
| mem_reads | `oltp_range_select` | 11.17ms | 12.04ms | 1.078× | 0.95% | PASS |
| mem_reads | `oltp_sum_range` | 10.96ms | 12.08ms | 1.102× | 1.13% | PASS |
| mem_reads | `oltp_order_range` | 2.62ms | 2.82ms | 1.076× | 1.02% | PASS |
| mem_reads | `oltp_distinct_range` | 3.38ms | 3.62ms | 1.070× | 0.52% | PASS |
| mem_reads | `oltp_index_scan` | 3.94ms | 5.43ms | 1.378× | 0.99% | PASS |
| mem_reads | `select_random_points` | 16.41ms | 20.00ms | 1.219× | 1.67% | PASS |
| mem_reads | `select_random_ranges` | 3.39ms | 4.50ms | 1.329× | 0.94% | PASS |
| mem_reads | `covering_index_scan` | 3.86ms | 4.03ms | 1.043× | 1.86% | PASS |
| mem_reads | `groupby_scan` | 29.55ms | 30.39ms | 1.028× | 0.44% | PASS |
| mem_reads | `index_join` | 6.03ms | 8.85ms | 1.468× | 0.87% | PASS |
| mem_reads | `index_join_scan` | 3.52ms | 5.10ms | 1.447× | 0.72% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.13s | 1.082× | 0.54% | PASS |
| mem_reads | `table_scan` | 1.26s | 1.23s | 0.978× | 0.87% | PASS |
| mem_reads | `oltp_read_only` | 107.67ms | 120.92ms | 1.123× | 0.85% | PASS |
| mem_writes | `oltp_bulk_insert` | 204.99ms | 296.14ms | 1.445× | 1.97% | PASS |
| mem_writes | `oltp_insert` | 17.61ms | 33.77ms | 1.918× | 1.07% | PASS |
| mem_writes | `oltp_update_index` | 60.48ms | 116.79ms | 1.931× | 1.11% | PASS |
| mem_writes | `oltp_update_non_index` | 41.53ms | 70.60ms | 1.700× | 1.03% | PASS |
| mem_writes | `oltp_delete_insert` | 42.68ms | 90.36ms | 2.117× | 0.74% | PASS |
| mem_writes | `oltp_write_only` | 23.57ms | 52.84ms | 2.242× | 0.89% | PASS |
| mem_writes | `types_delete_insert` | 27.68ms | 47.03ms | 1.699× | 1.00% | PASS |
| mem_writes | `oltp_read_write` | 74.77ms | 122.90ms | 1.644× | 1.32% | PASS |
| file_reads | `oltp_point_select` | 60.43ms | 44.52ms | 0.737× | 1.22% | PASS |
| file_reads | `oltp_range_select` | 14.94ms | 13.57ms | 0.908× | 1.31% | PASS |
| file_reads | `oltp_sum_range` | 14.85ms | 13.64ms | 0.919× | 1.96% | PASS |
| file_reads | `oltp_order_range` | 3.04ms | 3.04ms | 1.000× | 2.41% | PASS |
| file_reads | `oltp_distinct_range` | 3.82ms | 3.84ms | 1.005× | 1.59% | PASS |
| file_reads | `oltp_index_scan` | 7.60ms | 6.88ms | 0.905× | 1.46% | PASS |
| file_reads | `select_random_points` | 20.13ms | 21.74ms | 1.080× | 1.44% | PASS |
| file_reads | `select_random_ranges` | 6.88ms | 5.84ms | 0.849× | 0.95% | PASS |
| file_reads | `covering_index_scan` | 7.51ms | 5.40ms | 0.719× | 1.62% | PASS |
| file_reads | `groupby_scan` | 30.11ms | 30.69ms | 1.019× | 0.58% | PASS |
| file_reads | `index_join` | 8.30ms | 9.79ms | 1.180× | 1.61% | PASS |
| file_reads | `index_join_scan` | 4.07ms | 5.40ms | 1.326× | 1.75% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.13s | 1.081× | 0.73% | PASS |
| file_reads | `table_scan` | 1.28s | 1.25s | 0.973× | 0.55% | PASS |
| file_reads | `oltp_read_only` | 158.14ms | 139.20ms | 0.880× | 0.83% | PASS |
| file_writes | `oltp_bulk_insert` | 218.37ms | 316.37ms | 1.449× | 1.73% | PASS |
| file_writes | `oltp_insert` | 24.58ms | 42.86ms | 1.743× | 1.66% | PASS |
| file_writes | `oltp_update_index` | 80.84ms | 138.77ms | 1.717× | 1.33% | PASS |
| file_writes | `oltp_update_non_index` | 58.65ms | 87.61ms | 1.494× | 1.51% | PASS |
| file_writes | `oltp_delete_insert` | 60.61ms | 107.55ms | 1.774× | 1.06% | PASS |
| file_writes | `oltp_write_only` | 39.00ms | 67.73ms | 1.737× | 2.20% | PASS |
| file_writes | `types_delete_insert` | 38.98ms | 58.96ms | 1.513× | 1.24% | PASS |
| file_writes | `oltp_read_write` | 88.60ms | 135.14ms | 1.525× | 1.40% | PASS |
| ac_reads | `oltp_point_select` | 36.10ms | 43.74ms | 1.212× | 0.93% | PASS |
| ac_reads | `oltp_range_select` | 12.40ms | 13.56ms | 1.093× | 1.38% | PASS |
| ac_reads | `oltp_sum_range` | 12.14ms | 13.57ms | 1.118× | 1.34% | PASS |
| ac_reads | `oltp_order_range` | 2.75ms | 3.03ms | 1.101× | 1.58% | PASS |
| ac_reads | `oltp_distinct_range` | 3.52ms | 3.83ms | 1.088× | 1.47% | PASS |
| ac_reads | `oltp_index_scan` | 5.17ms | 6.82ms | 1.319× | 1.30% | PASS |
| ac_reads | `select_random_points` | 17.25ms | 21.40ms | 1.240× | 1.32% | PASS |
| ac_reads | `select_random_ranges` | 4.58ms | 5.81ms | 1.269× | 1.60% | PASS |
| ac_reads | `covering_index_scan` | 5.36ms | 5.41ms | 1.010× | 1.46% | PASS |
| ac_reads | `groupby_scan` | 29.60ms | 30.58ms | 1.033× | 0.50% | PASS |
| ac_reads | `index_join` | 7.23ms | 9.82ms | 1.359× | 1.55% | PASS |
| ac_reads | `index_join_scan` | 3.95ms | 5.48ms | 1.386× | 2.18% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.14s | 1.073× | 0.68% | PASS |
| ac_reads | `table_scan` | 1.25s | 1.23s | 0.983× | 0.94% | PASS |
| ac_reads | `oltp_read_only` | 123.07ms | 139.47ms | 1.133× | 0.93% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 14.75ms | 57.95ms | 3.930× | 6.79% | PASS |
| ac_writes | `oltp_insert_ac` | 17.55ms | 75.42ms | 4.297× | 7.40% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.31ms | 87.07ms | 4.509× | 4.65% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.14ms | 68.64ms | 4.251× | 7.16% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 16.81ms | 77.67ms | 4.619× | 6.73% | PASS |
| ac_writes | `oltp_write_only_ac` | 17.01ms | 77.91ms | 4.581× | 5.98% | PASS |
| ac_writes | `types_delete_insert_ac` | 14.96ms | 71.69ms | 4.793× | 4.77% | PASS |
| ac_writes | `oltp_read_write_ac` | 21.26ms | 84.35ms | 3.967× | 4.62% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 27.66ms | 31.53ms | 1.140× | 0.61% | PASS |
| mem_reads | `oltp_range_select` | 17.34ms | 18.50ms | 1.067× | 0.52% | PASS |
| mem_reads | `oltp_sum_range` | 16.12ms | 17.20ms | 1.067× | 0.62% | PASS |
| mem_reads | `oltp_order_range` | 3.32ms | 3.45ms | 1.041× | 0.55% | PASS |
| mem_reads | `oltp_distinct_range` | 4.26ms | 4.39ms | 1.031× | 0.70% | PASS |
| mem_reads | `oltp_index_scan` | 4.06ms | 5.06ms | 1.248× | 1.56% | PASS |
| mem_reads | `select_random_points` | 25.96ms | 29.02ms | 1.118× | 0.75% | PASS |
| mem_reads | `select_random_ranges` | 6.48ms | 7.33ms | 1.132× | 1.13% | PASS |
| mem_reads | `covering_index_scan` | 3.45ms | 3.55ms | 1.028× | 1.99% | PASS |
| mem_reads | `groupby_scan` | 34.71ms | 35.38ms | 1.019× | 0.53% | PASS |
| mem_reads | `index_join` | 7.29ms | 9.39ms | 1.289× | 0.89% | PASS |
| mem_reads | `index_join_scan` | 3.56ms | 5.01ms | 1.409× | 1.33% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.20s | 1.169× | 0.59% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.29s | 1.108× | 0.68% | PASS |
| mem_reads | `oltp_read_only` | 129.98ms | 141.79ms | 1.091× | 0.62% | PASS |
| mem_writes | `oltp_bulk_insert` | 198.33ms | 271.87ms | 1.371× | 0.41% | PASS |
| mem_writes | `oltp_insert` | 16.19ms | 28.98ms | 1.790× | 0.77% | PASS |
| mem_writes | `oltp_update_index` | 57.22ms | 99.07ms | 1.732× | 0.88% | PASS |
| mem_writes | `oltp_update_non_index` | 41.28ms | 67.38ms | 1.632× | 0.71% | PASS |
| mem_writes | `oltp_delete_insert` | 41.95ms | 78.86ms | 1.880× | 0.74% | PASS |
| mem_writes | `oltp_write_only` | 22.48ms | 47.23ms | 2.101× | 0.94% | PASS |
| mem_writes | `types_delete_insert` | 26.49ms | 44.25ms | 1.670× | 0.68% | PASS |
| mem_writes | `oltp_read_write` | 82.42ms | 123.30ms | 1.496× | 0.97% | PASS |
| file_reads | `oltp_point_select` | 59.89ms | 43.16ms | 0.721× | 0.60% | PASS |
| file_reads | `oltp_range_select` | 21.39ms | 20.31ms | 0.950× | 1.03% | PASS |
| file_reads | `oltp_sum_range` | 20.29ms | 18.99ms | 0.936× | 1.12% | PASS |
| file_reads | `oltp_order_range` | 3.80ms | 3.69ms | 0.973× | 1.54% | PASS |
| file_reads | `oltp_distinct_range` | 4.78ms | 4.63ms | 0.970× | 1.34% | PASS |
| file_reads | `oltp_index_scan` | 7.61ms | 6.70ms | 0.881× | 1.41% | PASS |
| file_reads | `select_random_points` | 29.91ms | 30.62ms | 1.024× | 1.11% | PASS |
| file_reads | `select_random_ranges` | 10.07ms | 8.75ms | 0.868× | 0.91% | PASS |
| file_reads | `covering_index_scan` | 6.98ms | 5.10ms | 0.730× | 1.64% | PASS |
| file_reads | `groupby_scan` | 35.69ms | 36.10ms | 1.011× | 0.59% | PASS |
| file_reads | `index_join` | 9.37ms | 10.84ms | 1.157× | 1.31% | PASS |
| file_reads | `index_join_scan` | 4.02ms | 5.36ms | 1.334× | 1.50% | PASS |
| file_reads | `types_table_scan` | 1.02s | 1.20s | 1.175× | 0.65% | PASS |
| file_reads | `table_scan` | 1.16s | 1.29s | 1.116× | 0.53% | PASS |
| file_reads | `oltp_read_only` | 176.82ms | 158.99ms | 0.899× | 0.65% | PASS |
| file_writes | `oltp_bulk_insert` | 258.46ms | 345.35ms | 1.336× | 2.67% | PASS |
| file_writes | `oltp_insert` | 33.27ms | 53.42ms | 1.606× | 5.87% | PASS |
| file_writes | `oltp_update_index` | 173.99ms | 178.23ms | 1.024× | 1.32% | PASS |
| file_writes | `oltp_update_non_index` | 145.19ms | 131.00ms | 0.902× | 1.10% | PASS |
| file_writes | `oltp_delete_insert` | 143.92ms | 149.30ms | 1.037× | 1.56% | PASS |
| file_writes | `oltp_write_only` | 104.67ms | 106.10ms | 1.014× | 3.10% | PASS |
| file_writes | `types_delete_insert` | 93.73ms | 86.97ms | 0.928× | 7.03% | PASS |
| file_writes | `oltp_read_write` | 165.01ms | 182.04ms | 1.103× | 1.78% | PASS |
| ac_reads | `oltp_point_select` | 37.49ms | 42.80ms | 1.142× | 0.61% | PASS |
| ac_reads | `oltp_range_select` | 18.48ms | 19.88ms | 1.076× | 0.71% | PASS |
| ac_reads | `oltp_sum_range` | 17.48ms | 18.71ms | 1.070× | 0.69% | PASS |
| ac_reads | `oltp_order_range` | 3.53ms | 3.65ms | 1.033× | 1.07% | PASS |
| ac_reads | `oltp_distinct_range` | 4.49ms | 4.57ms | 1.019× | 0.82% | PASS |
| ac_reads | `oltp_index_scan` | 5.43ms | 6.65ms | 1.225× | 1.22% | PASS |
| ac_reads | `select_random_points` | 27.36ms | 30.25ms | 1.105× | 0.76% | PASS |
| ac_reads | `select_random_ranges` | 7.74ms | 8.66ms | 1.119× | 0.96% | PASS |
| ac_reads | `covering_index_scan` | 4.67ms | 5.08ms | 1.086× | 1.30% | PASS |
| ac_reads | `groupby_scan` | 34.87ms | 35.69ms | 1.023× | 0.45% | PASS |
| ac_reads | `index_join` | 8.19ms | 10.60ms | 1.294× | 1.21% | PASS |
| ac_reads | `index_join_scan` | 3.85ms | 5.32ms | 1.380× | 1.19% | PASS |
| ac_reads | `types_table_scan` | 1.02s | 1.21s | 1.178× | 1.20% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.30s | 1.105× | 0.61% | PASS |
| ac_reads | `oltp_read_only` | 146.67ms | 159.68ms | 1.089× | 0.78% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 28.76ms | 103.41ms | 3.595× | 15.41% | PASS |
| ac_writes | `oltp_insert_ac` | 29.15ms | 119.84ms | 4.111× | 15.53% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.74ms | 135.04ms | 4.699× | 13.65% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 29.52ms | 117.56ms | 3.983× | 15.03% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 27.22ms | 122.36ms | 4.495× | 12.85% | PASS |
| ac_writes | `oltp_write_only_ac` | 28.23ms | 123.84ms | 4.388× | 14.37% | PASS |
| ac_writes | `types_delete_insert_ac` | 26.75ms | 128.25ms | 4.795× | 18.38% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.14ms | 133.71ms | 4.035× | 14.74% | PASS |

</details>

## Version-control latency

Wall time: 1m 49s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 62.44ms | 200.00ms | 31.2% | 0.70% | PASS |
| `status_dirty_many_tables` | 65.07ms | 200.00ms | 32.5% | 0.78% | PASS |
| `diff_regular_working_one_table` | 58.66ms | 150.00ms | 39.1% | 0.71% | PASS |
| `diff_regular_working_many_tables` | 70.30ms | 200.00ms | 35.2% | 0.56% | PASS |
| `diff_stat_working_many_tables` | 70.05ms | 200.00ms | 35.0% | 0.55% | PASS |
| `diff_schema_working_many_tables` | 70.52ms | 200.00ms | 35.3% | 0.65% | PASS |
| `branch_list_many_branches` | 19.80ms | 100.00ms | 19.8% | 1.53% | PASS |
| `branch_create_delete` | 21.48ms | 100.00ms | 21.5% | 1.35% | PASS |
| `checkout_branch_clean` | 46.19ms | 200.00ms | 23.1% | 0.89% | PASS |
| `merge_data_no_conflicts` | 24.42ms | 150.00ms | 16.3% | 1.55% | PASS |
| `merge_schema_no_conflicts` | 19.78ms | 100.00ms | 19.8% | 1.44% | PASS |
| `merge_data_conflicts` | 75.86ms | 250.00ms | 30.3% | 0.49% | PASS |
| `merge_data_conflicts_with_resolve` | 75.74ms | 250.00ms | 30.3% | 0.85% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
