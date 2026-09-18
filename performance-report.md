# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-18 15:26 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260907.300.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/35352062068)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 14m 4s | 9.26s | 11.70s | 1.264× | 1.51% | **PASS** |
| textpk | 69 | 55 | 1h 37m 2s | 11.02s | 11.28s | 1.023× | 1.63% | **PASS** |
| blobpk | 69 | 55 | 1h 27m 24s | 9.25s | 11.13s | 1.203× | 1.02% | **PASS** |
| compositepk | 69 | 55 | 1h 21m 37s | 9.21s | 12.59s | 1.367× | 1.98% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.30ms | 29.91ms | 1.231× | 1.28% | PASS |
| mem_reads | `oltp_range_select` | 10.23ms | 13.22ms | 1.292× | 1.74% | PASS |
| mem_reads | `oltp_sum_range` | 9.64ms | 12.36ms | 1.282× | 1.11% | PASS |
| mem_reads | `oltp_order_range` | 2.59ms | 3.00ms | 1.159× | 1.28% | PASS |
| mem_reads | `oltp_distinct_range` | 3.64ms | 4.10ms | 1.127× | 1.02% | PASS |
| mem_reads | `oltp_index_scan` | 4.05ms | 5.69ms | 1.405× | 2.32% | PASS |
| mem_reads | `select_random_points` | 10.79ms | 11.59ms | 1.073× | 1.88% | PASS |
| mem_reads | `select_random_ranges` | 3.12ms | 4.08ms | 1.309× | 1.28% | PASS |
| mem_reads | `covering_index_scan` | 4.34ms | 4.61ms | 1.062× | 1.48% | PASS |
| mem_reads | `groupby_scan` | 30.20ms | 33.15ms | 1.098× | 0.80% | PASS |
| mem_reads | `index_join` | 6.09ms | 8.55ms | 1.404× | 2.05% | PASS |
| mem_reads | `index_join_scan` | 3.52ms | 4.66ms | 1.326× | 1.76% | PASS |
| mem_reads | `types_table_scan` | 1.12s | 1.36s | 1.211× | 0.93% | PASS |
| mem_reads | `table_scan` | 1.29s | 1.44s | 1.118× | 0.64% | PASS |
| mem_reads | `oltp_read_only` | 109.27ms | 127.31ms | 1.165× | 2.22% | PASS |
| mem_writes | `oltp_bulk_insert` | 182.32ms | 254.37ms | 1.395× | 1.13% | PASS |
| mem_writes | `oltp_insert` | 15.70ms | 28.95ms | 1.843× | 0.93% | PASS |
| mem_writes | `oltp_update_index` | 52.86ms | 109.63ms | 2.074× | 1.94% | PASS |
| mem_writes | `oltp_update_non_index` | 35.50ms | 61.73ms | 1.739× | 2.59% | PASS |
| mem_writes | `oltp_delete_insert` | 46.84ms | 81.23ms | 1.734× | 1.59% | PASS |
| mem_writes | `oltp_write_only` | 22.33ms | 45.86ms | 2.054× | 1.51% | PASS |
| mem_writes | `types_delete_insert` | 25.30ms | 41.31ms | 1.633× | 1.30% | PASS |
| mem_writes | `oltp_read_write` | 70.50ms | 114.55ms | 1.625× | 2.12% | PASS |
| file_reads | `oltp_point_select` | 99.30ms | 55.84ms | 0.562× | 0.80% | PASS |
| file_reads | `oltp_range_select` | 18.77ms | 16.10ms | 0.858× | 1.50% | PASS |
| file_reads | `oltp_sum_range` | 17.81ms | 15.37ms | 0.863× | 1.67% | PASS |
| file_reads | `oltp_order_range` | 3.48ms | 3.39ms | 0.973× | 1.76% | PASS |
| file_reads | `oltp_distinct_range` | 4.59ms | 4.51ms | 0.981× | 0.95% | PASS |
| file_reads | `oltp_index_scan` | 11.96ms | 8.47ms | 0.709× | 1.33% | PASS |
| file_reads | `select_random_points` | 19.15ms | 14.53ms | 0.759× | 1.44% | PASS |
| file_reads | `select_random_ranges` | 10.68ms | 6.69ms | 0.626× | 0.85% | PASS |
| file_reads | `covering_index_scan` | 12.28ms | 7.25ms | 0.591× | 1.28% | PASS |
| file_reads | `groupby_scan` | 31.08ms | 33.59ms | 1.081× | 1.18% | PASS |
| file_reads | `index_join` | 10.71ms | 10.54ms | 0.984× | 2.01% | PASS |
| file_reads | `index_join_scan` | 4.54ms | 5.12ms | 1.129× | 2.33% | PASS |
| file_reads | `types_table_scan` | 1.10s | 1.35s | 1.223× | 1.93% | PASS |
| file_reads | `table_scan` | 1.26s | 1.42s | 1.125× | 1.84% | PASS |
| file_reads | `oltp_read_only` | 219.34ms | 164.86ms | 0.752× | 1.12% | PASS |
| file_writes | `oltp_bulk_insert` | 195.91ms | 273.45ms | 1.396× | 1.48% | PASS |
| file_writes | `oltp_insert` | 22.94ms | 36.50ms | 1.591× | 1.50% | PASS |
| file_writes | `oltp_update_index` | 83.01ms | 136.84ms | 1.649× | 2.18% | PASS |
| file_writes | `oltp_update_non_index` | 61.10ms | 84.69ms | 1.386× | 1.87% | PASS |
| file_writes | `oltp_delete_insert` | 71.81ms | 103.13ms | 1.436× | 1.70% | PASS |
| file_writes | `oltp_write_only` | 47.02ms | 66.36ms | 1.411× | 2.13% | PASS |
| file_writes | `types_delete_insert` | 41.61ms | 55.32ms | 1.330× | 1.23% | PASS |
| file_writes | `oltp_read_write` | 99.84ms | 136.62ms | 1.368× | 1.75% | PASS |
| ac_reads | `oltp_point_select` | 49.09ms | 55.59ms | 1.132× | 1.22% | PASS |
| ac_reads | `oltp_range_select` | 13.36ms | 15.93ms | 1.192× | 1.98% | PASS |
| ac_reads | `oltp_sum_range` | 12.78ms | 15.32ms | 1.198× | 1.37% | PASS |
| ac_reads | `oltp_order_range` | 3.05ms | 3.38ms | 1.107× | 1.85% | PASS |
| ac_reads | `oltp_distinct_range` | 4.05ms | 4.49ms | 1.108× | 1.12% | PASS |
| ac_reads | `oltp_index_scan` | 6.88ms | 8.42ms | 1.224× | 1.25% | PASS |
| ac_reads | `select_random_points` | 13.52ms | 14.23ms | 1.053× | 1.74% | PASS |
| ac_reads | `select_random_ranges` | 5.63ms | 6.65ms | 1.181× | 1.50% | PASS |
| ac_reads | `covering_index_scan` | 7.16ms | 7.17ms | 1.001× | 1.06% | PASS |
| ac_reads | `groupby_scan` | 30.42ms | 33.48ms | 1.100× | 0.86% | PASS |
| ac_reads | `index_join` | 7.85ms | 10.35ms | 1.319× | 1.40% | PASS |
| ac_reads | `index_join_scan` | 3.95ms | 4.98ms | 1.262× | 1.69% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.33s | 1.279× | 0.84% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.40s | 1.187× | 1.76% | PASS |
| ac_reads | `oltp_read_only` | 140.95ms | 162.85ms | 1.155× | 1.18% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.91ms | 84.03ms | 3.669× | 5.32% | PASS |
| ac_writes | `oltp_insert_ac` | 25.16ms | 101.98ms | 4.053× | 6.32% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.93ms | 116.32ms | 4.165× | 5.56% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.26ms | 95.63ms | 3.785× | 5.79% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.71ms | 109.93ms | 4.276× | 5.05% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.80ms | 106.61ms | 3.979× | 6.30% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.91ms | 97.53ms | 3.915× | 5.58% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.66ms | 113.62ms | 3.706× | 4.23% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.40ms | 28.50ms | 1.122× | 2.29% | PASS |
| mem_reads | `oltp_range_select` | 12.67ms | 11.25ms | 0.888× | 3.25% | PASS |
| mem_reads | `oltp_sum_range` | 10.89ms | 11.00ms | 1.010× | 2.81% | PASS |
| mem_reads | `oltp_order_range` | 2.63ms | 2.57ms | 0.979× | 1.42% | PASS |
| mem_reads | `oltp_distinct_range` | 3.46ms | 3.33ms | 0.965× | 1.11% | PASS |
| mem_reads | `oltp_index_scan` | 3.74ms | 4.70ms | 1.259× | 2.19% | PASS |
| mem_reads | `select_random_points` | 14.48ms | 16.14ms | 1.115× | 1.43% | PASS |
| mem_reads | `select_random_ranges` | 3.47ms | 4.21ms | 1.213× | 2.54% | PASS |
| mem_reads | `covering_index_scan` | 3.92ms | 3.69ms | 0.943× | 3.32% | PASS |
| mem_reads | `groupby_scan` | 27.35ms | 27.94ms | 1.022× | 0.89% | PASS |
| mem_reads | `index_join` | 5.68ms | 6.94ms | 1.220× | 1.55% | PASS |
| mem_reads | `index_join_scan` | 4.50ms | 4.96ms | 1.102× | 5.41% | PASS |
| mem_reads | `types_table_scan` | 1.15s | 1.06s | 0.920× | 1.61% | PASS |
| mem_reads | `table_scan` | 1.42s | 1.14s | 0.803× | 1.77% | PASS |
| mem_reads | `oltp_read_only` | 108.54ms | 105.65ms | 0.973× | 2.10% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.65ms | 265.36ms | 1.461× | 1.14% | PASS |
| mem_writes | `oltp_insert` | 17.83ms | 29.96ms | 1.680× | 1.57% | PASS |
| mem_writes | `oltp_update_index` | 59.97ms | 105.63ms | 1.761× | 1.72% | PASS |
| mem_writes | `oltp_update_non_index` | 40.94ms | 67.55ms | 1.650× | 1.72% | PASS |
| mem_writes | `oltp_delete_insert` | 42.21ms | 80.49ms | 1.907× | 0.81% | PASS |
| mem_writes | `oltp_write_only` | 23.94ms | 47.63ms | 1.990× | 0.93% | PASS |
| mem_writes | `types_delete_insert` | 27.60ms | 41.10ms | 1.489× | 3.00% | PASS |
| mem_writes | `oltp_read_write` | 75.00ms | 108.75ms | 1.450× | 1.62% | PASS |
| file_reads | `oltp_point_select` | 99.07ms | 52.48ms | 0.530× | 0.78% | PASS |
| file_reads | `oltp_range_select` | 19.93ms | 13.36ms | 0.671× | 1.56% | PASS |
| file_reads | `oltp_sum_range` | 18.41ms | 13.26ms | 0.720× | 1.33% | PASS |
| file_reads | `oltp_order_range` | 3.52ms | 2.84ms | 0.807× | 1.07% | PASS |
| file_reads | `oltp_distinct_range` | 4.34ms | 3.62ms | 0.833× | 1.17% | PASS |
| file_reads | `oltp_index_scan` | 11.78ms | 7.65ms | 0.649× | 0.97% | PASS |
| file_reads | `select_random_points` | 22.94ms | 18.88ms | 0.823× | 0.86% | PASS |
| file_reads | `select_random_ranges` | 11.13ms | 6.72ms | 0.604× | 1.00% | PASS |
| file_reads | `covering_index_scan` | 12.84ms | 6.44ms | 0.502× | 1.42% | PASS |
| file_reads | `groupby_scan` | 28.37ms | 28.13ms | 0.992× | 1.03% | PASS |
| file_reads | `index_join` | 10.80ms | 9.33ms | 0.864× | 1.62% | PASS |
| file_reads | `index_join_scan` | 5.01ms | 5.14ms | 1.026× | 1.34% | PASS |
| file_reads | `types_table_scan` | 924.32ms | 979.00ms | 1.059× | 2.28% | PASS |
| file_reads | `table_scan` | 1.34s | 1.10s | 0.826× | 5.60% | PASS |
| file_reads | `oltp_read_only` | 212.37ms | 138.32ms | 0.651× | 1.41% | PASS |
| file_writes | `oltp_bulk_insert` | 258.23ms | 355.02ms | 1.375× | 9.39% | PASS |
| file_writes | `oltp_insert` | 94.22ms | 70.52ms | 0.748× | 48.82% | PASS |
| file_writes | `oltp_update_index` | 225.82ms | 215.17ms | 0.953× | 13.50% | PASS |
| file_writes | `oltp_update_non_index` | 257.63ms | 154.17ms | 0.598× | 28.68% | PASS |
| file_writes | `oltp_delete_insert` | 245.29ms | 173.40ms | 0.707× | 21.19% | PASS |
| file_writes | `oltp_write_only` | 152.38ms | 120.17ms | 0.789× | 15.60% | PASS |
| file_writes | `types_delete_insert` | 110.37ms | 105.90ms | 0.959× | 13.92% | PASS |
| file_writes | `oltp_read_write` | 237.30ms | 178.01ms | 0.750× | 21.51% | PASS |
| ac_reads | `oltp_point_select` | 49.93ms | 53.26ms | 1.067× | 1.80% | PASS |
| ac_reads | `oltp_range_select` | 15.39ms | 13.56ms | 0.881× | 2.68% | PASS |
| ac_reads | `oltp_sum_range` | 14.07ms | 13.62ms | 0.968× | 2.07% | PASS |
| ac_reads | `oltp_order_range` | 3.07ms | 2.84ms | 0.927× | 1.38% | PASS |
| ac_reads | `oltp_distinct_range` | 3.82ms | 3.61ms | 0.945× | 0.83% | PASS |
| ac_reads | `oltp_index_scan` | 6.89ms | 7.63ms | 1.107× | 0.68% | PASS |
| ac_reads | `select_random_points` | 18.84ms | 19.60ms | 1.040× | 1.61% | PASS |
| ac_reads | `select_random_ranges` | 6.31ms | 6.78ms | 1.075× | 1.03% | PASS |
| ac_reads | `covering_index_scan` | 7.78ms | 6.43ms | 0.827× | 1.11% | PASS |
| ac_reads | `groupby_scan` | 27.51ms | 28.11ms | 1.022× | 0.71% | PASS |
| ac_reads | `index_join` | 8.17ms | 9.21ms | 1.126× | 0.99% | PASS |
| ac_reads | `index_join_scan` | 4.67ms | 5.26ms | 1.126× | 1.63% | PASS |
| ac_reads | `types_table_scan` | 1.16s | 1.05s | 0.903× | 1.69% | PASS |
| ac_reads | `table_scan` | 1.49s | 1.16s | 0.777× | 0.65% | PASS |
| ac_reads | `oltp_read_only` | 149.10ms | 144.24ms | 0.967× | 1.04% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 52.23ms | 163.78ms | 3.135× | 40.14% | PASS |
| ac_writes | `oltp_insert_ac` | 103.23ms | 377.54ms | 3.657× | 75.25% | PASS |
| ac_writes | `oltp_update_index_ac` | 66.49ms | 286.10ms | 4.303× | 58.59% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 38.05ms | 142.66ms | 3.749× | 29.79% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 57.16ms | 289.85ms | 5.071× | 72.82% | PASS |
| ac_writes | `oltp_write_only_ac` | 52.28ms | 174.13ms | 3.331× | 44.83% | PASS |
| ac_writes | `types_delete_insert_ac` | 52.69ms | 196.98ms | 3.738× | 53.47% | PASS |
| ac_writes | `oltp_read_write_ac` | 64.18ms | 187.38ms | 2.920× | 54.02% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.06ms | 29.12ms | 1.117× | 0.85% | PASS |
| mem_reads | `oltp_range_select` | 11.74ms | 12.30ms | 1.048× | 0.86% | PASS |
| mem_reads | `oltp_sum_range` | 10.82ms | 11.38ms | 1.052× | 1.42% | PASS |
| mem_reads | `oltp_order_range` | 2.69ms | 2.82ms | 1.047× | 1.15% | PASS |
| mem_reads | `oltp_distinct_range` | 3.61ms | 3.73ms | 1.034× | 0.79% | PASS |
| mem_reads | `oltp_index_scan` | 3.99ms | 4.91ms | 1.230× | 0.84% | PASS |
| mem_reads | `select_random_points` | 16.12ms | 18.38ms | 1.141× | 1.13% | PASS |
| mem_reads | `select_random_ranges` | 3.34ms | 4.31ms | 1.290× | 1.26% | PASS |
| mem_reads | `covering_index_scan` | 3.60ms | 3.57ms | 0.992× | 1.18% | PASS |
| mem_reads | `groupby_scan` | 30.68ms | 31.12ms | 1.015× | 0.38% | PASS |
| mem_reads | `index_join` | 6.16ms | 7.85ms | 1.275× | 0.72% | PASS |
| mem_reads | `index_join_scan` | 3.57ms | 4.97ms | 1.394× | 2.47% | PASS |
| mem_reads | `types_table_scan` | 1.01s | 1.17s | 1.161× | 0.24% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.28s | 1.094× | 0.24% | PASS |
| mem_reads | `oltp_read_only` | 102.80ms | 114.25ms | 1.111× | 0.53% | PASS |
| mem_writes | `oltp_bulk_insert` | 188.13ms | 262.34ms | 1.394× | 0.39% | PASS |
| mem_writes | `oltp_insert` | 16.84ms | 30.71ms | 1.823× | 0.60% | PASS |
| mem_writes | `oltp_update_index` | 57.44ms | 106.54ms | 1.855× | 0.64% | PASS |
| mem_writes | `oltp_update_non_index` | 39.86ms | 67.58ms | 1.695× | 0.81% | PASS |
| mem_writes | `oltp_delete_insert` | 41.36ms | 84.22ms | 2.036× | 0.93% | PASS |
| mem_writes | `oltp_write_only` | 22.95ms | 50.43ms | 2.198× | 0.79% | PASS |
| mem_writes | `types_delete_insert` | 26.35ms | 43.71ms | 1.659× | 1.05% | PASS |
| mem_writes | `oltp_read_write` | 68.02ms | 111.66ms | 1.641× | 0.92% | PASS |
| file_reads | `oltp_point_select` | 57.24ms | 40.30ms | 0.704× | 0.61% | PASS |
| file_reads | `oltp_range_select` | 15.00ms | 13.88ms | 0.925× | 1.19% | PASS |
| file_reads | `oltp_sum_range` | 14.40ms | 13.04ms | 0.906× | 1.75% | PASS |
| file_reads | `oltp_order_range` | 3.09ms | 3.05ms | 0.986× | 0.93% | PASS |
| file_reads | `oltp_distinct_range` | 4.08ms | 3.98ms | 0.974× | 1.32% | PASS |
| file_reads | `oltp_index_scan` | 7.43ms | 6.46ms | 0.869× | 1.74% | PASS |
| file_reads | `select_random_points` | 19.97ms | 20.11ms | 1.007× | 0.95% | PASS |
| file_reads | `select_random_ranges` | 6.72ms | 5.62ms | 0.837× | 0.88% | PASS |
| file_reads | `covering_index_scan` | 7.32ms | 5.20ms | 0.711× | 1.40% | PASS |
| file_reads | `groupby_scan` | 31.53ms | 31.55ms | 1.001× | 0.42% | PASS |
| file_reads | `index_join` | 8.45ms | 9.74ms | 1.153× | 1.07% | PASS |
| file_reads | `index_join_scan` | 4.21ms | 5.49ms | 1.304× | 1.04% | PASS |
| file_reads | `types_table_scan` | 1.01s | 1.17s | 1.158× | 0.31% | PASS |
| file_reads | `table_scan` | 1.18s | 1.28s | 1.090× | 0.47% | PASS |
| file_reads | `oltp_read_only` | 152.49ms | 132.28ms | 0.867× | 0.94% | PASS |
| file_writes | `oltp_bulk_insert` | 254.02ms | 350.73ms | 1.381× | 1.92% | PASS |
| file_writes | `oltp_insert` | 46.50ms | 64.13ms | 1.379× | 2.02% | PASS |
| file_writes | `oltp_update_index` | 198.07ms | 209.82ms | 1.059× | 1.53% | PASS |
| file_writes | `oltp_update_non_index` | 153.27ms | 140.33ms | 0.916× | 1.25% | PASS |
| file_writes | `oltp_delete_insert` | 161.95ms | 168.61ms | 1.041× | 1.69% | PASS |
| file_writes | `oltp_write_only` | 120.09ms | 119.39ms | 0.994× | 1.64% | PASS |
| file_writes | `types_delete_insert` | 103.43ms | 98.91ms | 0.956× | 2.03% | PASS |
| file_writes | `oltp_read_write` | 168.44ms | 182.46ms | 1.083× | 1.88% | PASS |
| ac_reads | `oltp_point_select` | 35.68ms | 40.36ms | 1.131× | 0.38% | PASS |
| ac_reads | `oltp_range_select` | 13.17ms | 13.96ms | 1.060× | 0.62% | PASS |
| ac_reads | `oltp_sum_range` | 12.83ms | 13.17ms | 1.026× | 0.69% | PASS |
| ac_reads | `oltp_order_range` | 2.94ms | 3.07ms | 1.041× | 0.66% | PASS |
| ac_reads | `oltp_distinct_range` | 3.88ms | 3.97ms | 1.024× | 0.84% | PASS |
| ac_reads | `oltp_index_scan` | 5.39ms | 6.47ms | 1.200× | 1.48% | PASS |
| ac_reads | `select_random_points` | 17.81ms | 19.89ms | 1.117× | 1.25% | PASS |
| ac_reads | `select_random_ranges` | 4.62ms | 5.63ms | 1.220× | 1.67% | PASS |
| ac_reads | `covering_index_scan` | 5.17ms | 5.17ms | 1.000× | 0.89% | PASS |
| ac_reads | `groupby_scan` | 31.33ms | 31.55ms | 1.007× | 0.44% | PASS |
| ac_reads | `index_join` | 7.44ms | 9.72ms | 1.305× | 1.24% | PASS |
| ac_reads | `index_join_scan` | 4.09ms | 5.48ms | 1.341× | 1.02% | PASS |
| ac_reads | `types_table_scan` | 1.01s | 1.18s | 1.159× | 0.33% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.28s | 1.088× | 0.40% | PASS |
| ac_reads | `oltp_read_only` | 119.07ms | 131.87ms | 1.108× | 0.88% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.60ms | 88.09ms | 3.732× | 10.32% | PASS |
| ac_writes | `oltp_insert_ac` | 25.50ms | 104.80ms | 4.109× | 7.69% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.73ms | 116.40ms | 4.354× | 6.23% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.39ms | 103.52ms | 4.245× | 11.40% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.31ms | 113.53ms | 4.316× | 9.70% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.70ms | 106.51ms | 4.313× | 12.06% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.66ms | 101.47ms | 4.288× | 7.32% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.27ms | 114.62ms | 3.916× | 6.04% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 21.18ms | 24.77ms | 1.169× | 1.72% | PASS |
| mem_reads | `oltp_range_select` | 14.04ms | 13.56ms | 0.965× | 2.44% | PASS |
| mem_reads | `oltp_sum_range` | 13.66ms | 12.95ms | 0.948× | 2.12% | PASS |
| mem_reads | `oltp_order_range` | 2.64ms | 2.61ms | 0.991× | 2.37% | PASS |
| mem_reads | `oltp_distinct_range` | 3.29ms | 3.22ms | 0.978× | 1.72% | PASS |
| mem_reads | `oltp_index_scan` | 3.22ms | 4.21ms | 1.311× | 2.30% | PASS |
| mem_reads | `select_random_points` | 20.00ms | 22.13ms | 1.107× | 1.69% | PASS |
| mem_reads | `select_random_ranges` | 5.09ms | 5.87ms | 1.153× | 1.15% | PASS |
| mem_reads | `covering_index_scan` | 2.51ms | 2.70ms | 1.074× | 3.72% | PASS |
| mem_reads | `groupby_scan` | 23.34ms | 24.82ms | 1.063× | 0.91% | PASS |
| mem_reads | `index_join` | 5.64ms | 7.26ms | 1.287× | 1.47% | PASS |
| mem_reads | `index_join_scan` | 2.95ms | 4.49ms | 1.525× | 2.33% | PASS |
| mem_reads | `types_table_scan` | 758.63ms | 856.15ms | 1.129× | 1.18% | PASS |
| mem_reads | `table_scan` | 859.23ms | 915.09ms | 1.065× | 1.33% | PASS |
| mem_reads | `oltp_read_only` | 95.03ms | 102.08ms | 1.074× | 1.60% | PASS |
| mem_writes | `oltp_bulk_insert` | 140.14ms | 197.36ms | 1.408× | 1.36% | PASS |
| mem_writes | `oltp_insert` | 11.59ms | 21.34ms | 1.841× | 0.90% | PASS |
| mem_writes | `oltp_update_index` | 44.10ms | 75.68ms | 1.716× | 1.66% | PASS |
| mem_writes | `oltp_update_non_index` | 32.45ms | 52.17ms | 1.608× | 1.44% | PASS |
| mem_writes | `oltp_delete_insert` | 32.82ms | 60.78ms | 1.852× | 0.73% | PASS |
| mem_writes | `oltp_write_only` | 18.10ms | 36.49ms | 2.016× | 0.79% | PASS |
| mem_writes | `types_delete_insert` | 20.25ms | 33.05ms | 1.631× | 1.98% | PASS |
| mem_writes | `oltp_read_write` | 63.30ms | 92.16ms | 1.456× | 1.28% | PASS |
| file_reads | `oltp_point_select` | 43.69ms | 32.06ms | 0.734× | 1.55% | PASS |
| file_reads | `oltp_range_select` | 16.15ms | 14.34ms | 0.888× | 1.40% | PASS |
| file_reads | `oltp_sum_range` | 15.65ms | 13.62ms | 0.870× | 1.74% | PASS |
| file_reads | `oltp_order_range` | 2.79ms | 2.65ms | 0.950× | 1.21% | PASS |
| file_reads | `oltp_distinct_range` | 3.38ms | 3.22ms | 0.955× | 1.46% | PASS |
| file_reads | `oltp_index_scan` | 5.45ms | 4.88ms | 0.895× | 2.49% | PASS |
| file_reads | `select_random_points` | 22.22ms | 22.95ms | 1.033× | 2.22% | PASS |
| file_reads | `select_random_ranges` | 7.33ms | 6.54ms | 0.891× | 2.14% | PASS |
| file_reads | `covering_index_scan` | 4.80ms | 3.58ms | 0.745× | 5.50% | PASS |
| file_reads | `groupby_scan` | 23.09ms | 24.67ms | 1.069× | 0.70% | PASS |
| file_reads | `index_join` | 7.04ms | 8.36ms | 1.188× | 2.14% | PASS |
| file_reads | `index_join_scan` | 3.32ms | 4.72ms | 1.421× | 3.14% | PASS |
| file_reads | `types_table_scan` | 750.26ms | 848.31ms | 1.131× | 1.63% | PASS |
| file_reads | `table_scan` | 865.00ms | 916.37ms | 1.059× | 1.45% | PASS |
| file_reads | `oltp_read_only` | 125.58ms | 113.12ms | 0.901× | 1.59% | PASS |
| file_writes | `oltp_bulk_insert` | 268.29ms | 295.47ms | 1.101× | 26.42% | PASS |
| file_writes | `oltp_insert` | 53.20ms | 70.57ms | 1.327× | 72.83% | PASS |
| file_writes | `oltp_update_index` | 203.02ms | 166.34ms | 0.819× | 26.24% | PASS |
| file_writes | `oltp_update_non_index` | 149.24ms | 110.27ms | 0.739× | 23.06% | PASS |
| file_writes | `oltp_delete_insert` | 185.95ms | 125.55ms | 0.675× | 27.43% | PASS |
| file_writes | `oltp_write_only` | 227.90ms | 112.80ms | 0.495× | 44.33% | PASS |
| file_writes | `types_delete_insert` | 112.08ms | 99.14ms | 0.885× | 47.79% | PASS |
| file_writes | `oltp_read_write` | 199.31ms | 156.86ms | 0.787× | 39.22% | PASS |
| ac_reads | `oltp_point_select` | 27.61ms | 31.56ms | 1.143× | 1.39% | PASS |
| ac_reads | `oltp_range_select` | 15.01ms | 14.36ms | 0.956× | 1.73% | PASS |
| ac_reads | `oltp_sum_range` | 14.63ms | 13.73ms | 0.938× | 1.61% | PASS |
| ac_reads | `oltp_order_range` | 2.69ms | 2.69ms | 1.001× | 2.59% | PASS |
| ac_reads | `oltp_distinct_range` | 3.42ms | 3.32ms | 0.970× | 2.62% | PASS |
| ac_reads | `oltp_index_scan` | 4.27ms | 5.26ms | 1.233× | 3.40% | PASS |
| ac_reads | `select_random_points` | 20.84ms | 23.21ms | 1.114× | 1.91% | PASS |
| ac_reads | `select_random_ranges` | 5.86ms | 6.54ms | 1.115× | 1.62% | PASS |
| ac_reads | `covering_index_scan` | 3.42ms | 3.59ms | 1.050× | 4.03% | PASS |
| ac_reads | `groupby_scan` | 22.98ms | 24.68ms | 1.074× | 0.90% | PASS |
| ac_reads | `index_join` | 6.26ms | 7.77ms | 1.242× | 4.15% | PASS |
| ac_reads | `index_join_scan` | 2.92ms | 4.41ms | 1.511× | 3.94% | PASS |
| ac_reads | `types_table_scan` | 748.63ms | 845.04ms | 1.129× | 0.52% | PASS |
| ac_reads | `table_scan` | 862.28ms | 914.85ms | 1.061× | 1.33% | PASS |
| ac_reads | `oltp_read_only` | 107.67ms | 113.52ms | 1.054× | 1.83% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 152.25ms | 381.24ms | 2.504× | 55.72% | PASS |
| ac_writes | `oltp_insert_ac` | 225.29ms | 514.14ms | 2.282× | 57.67% | PASS |
| ac_writes | `oltp_update_index_ac` | 184.41ms | 669.74ms | 3.632× | 54.81% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 128.07ms | 299.88ms | 2.342× | 48.79% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 201.37ms | 385.88ms | 1.916× | 60.49% | PASS |
| ac_writes | `oltp_write_only_ac` | 367.19ms | 947.49ms | 2.580× | 45.09% | PASS |
| ac_writes | `types_delete_insert_ac` | 353.30ms | 955.57ms | 2.705× | 56.64% | PASS |
| ac_writes | `oltp_read_write_ac` | 265.01ms | 696.10ms | 2.627× | 48.54% | PASS |

</details>

## Version-control latency

Wall time: 2m 20s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 84.46ms | 200.00ms | 42.2% | 0.39% | PASS |
| `status_dirty_many_tables` | 87.33ms | 200.00ms | 43.7% | 0.44% | PASS |
| `diff_regular_working_one_table` | 80.15ms | 150.00ms | 53.4% | 0.58% | PASS |
| `diff_regular_working_many_tables` | 93.87ms | 200.00ms | 46.9% | 0.60% | PASS |
| `diff_stat_working_many_tables` | 92.68ms | 200.00ms | 46.3% | 0.51% | PASS |
| `diff_schema_working_many_tables` | 92.57ms | 200.00ms | 46.3% | 0.52% | PASS |
| `branch_list_many_branches` | 22.58ms | 100.00ms | 22.6% | 0.98% | PASS |
| `branch_create_delete` | 25.02ms | 100.00ms | 25.0% | 1.49% | PASS |
| `checkout_branch_clean` | 55.05ms | 200.00ms | 27.5% | 0.96% | PASS |
| `merge_data_no_conflicts` | 28.89ms | 150.00ms | 19.3% | 1.18% | PASS |
| `merge_schema_no_conflicts` | 22.48ms | 100.00ms | 22.5% | 1.96% | PASS |
| `merge_data_conflicts` | 127.24ms | 250.00ms | 50.9% | 0.28% | PASS |
| `merge_data_conflicts_with_resolve` | 126.98ms | 250.00ms | 50.8% | 0.36% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
