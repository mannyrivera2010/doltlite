# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-25 15:24 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261023.334.3
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/40199511502)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 8s | 8.76s | 11.33s | 1.293× | 1.28% | **PASS** |
| textpk | 69 | 55 | 1h 30m 40s | 9.49s | 10.26s | 1.082× | 1.88% | **PASS** |
| blobpk | 69 | 55 | 1h 17m 34s | 8.25s | 9.77s | 1.185× | 3.70% | **PASS** |
| compositepk | 69 | 55 | 1h 4m 45s | 7.40s | 7.94s | 1.074× | 2.75% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.66ms | 29.15ms | 1.232× | 1.26% | PASS |
| mem_reads | `oltp_range_select` | 9.92ms | 13.04ms | 1.315× | 2.53% | PASS |
| mem_reads | `oltp_sum_range` | 9.46ms | 12.26ms | 1.295× | 1.76% | PASS |
| mem_reads | `oltp_order_range` | 2.52ms | 2.96ms | 1.175× | 1.42% | PASS |
| mem_reads | `oltp_distinct_range` | 3.61ms | 4.02ms | 1.112× | 1.45% | PASS |
| mem_reads | `oltp_index_scan` | 3.87ms | 5.20ms | 1.342× | 1.51% | PASS |
| mem_reads | `select_random_points` | 9.68ms | 10.99ms | 1.135× | 2.90% | PASS |
| mem_reads | `select_random_ranges` | 2.93ms | 3.95ms | 1.349× | 1.71% | PASS |
| mem_reads | `covering_index_scan` | 4.22ms | 4.15ms | 0.982× | 1.40% | PASS |
| mem_reads | `groupby_scan` | 29.60ms | 33.20ms | 1.122× | 0.88% | PASS |
| mem_reads | `index_join` | 6.04ms | 8.01ms | 1.327× | 1.30% | PASS |
| mem_reads | `index_join_scan` | 3.45ms | 4.50ms | 1.304× | 1.18% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.33s | 1.286× | 0.63% | PASS |
| mem_reads | `table_scan` | 1.16s | 1.39s | 1.198× | 0.56% | PASS |
| mem_reads | `oltp_read_only` | 101.06ms | 122.81ms | 1.215× | 1.22% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.86ms | 252.74ms | 1.390× | 1.00% | PASS |
| mem_writes | `oltp_insert` | 15.56ms | 28.23ms | 1.814× | 0.97% | PASS |
| mem_writes | `oltp_update_index` | 49.85ms | 103.05ms | 2.067× | 0.85% | PASS |
| mem_writes | `oltp_update_non_index` | 33.51ms | 58.79ms | 1.754× | 1.26% | PASS |
| mem_writes | `oltp_delete_insert` | 44.77ms | 77.96ms | 1.741× | 1.38% | PASS |
| mem_writes | `oltp_write_only` | 21.84ms | 44.12ms | 2.021× | 1.27% | PASS |
| mem_writes | `types_delete_insert` | 24.40ms | 40.13ms | 1.645× | 0.83% | PASS |
| mem_writes | `oltp_read_write` | 65.67ms | 109.45ms | 1.667× | 1.18% | PASS |
| file_reads | `oltp_point_select` | 97.95ms | 55.20ms | 0.564× | 0.84% | PASS |
| file_reads | `oltp_range_select` | 17.49ms | 15.81ms | 0.904× | 1.86% | PASS |
| file_reads | `oltp_sum_range` | 16.91ms | 15.19ms | 0.899× | 1.16% | PASS |
| file_reads | `oltp_order_range` | 3.31ms | 3.31ms | 1.000× | 1.46% | PASS |
| file_reads | `oltp_distinct_range` | 4.45ms | 4.40ms | 0.989× | 1.35% | PASS |
| file_reads | `oltp_index_scan` | 11.45ms | 8.26ms | 0.721× | 1.20% | PASS |
| file_reads | `select_random_points` | 17.10ms | 13.87ms | 0.811× | 1.68% | PASS |
| file_reads | `select_random_ranges` | 10.36ms | 6.59ms | 0.636× | 1.03% | PASS |
| file_reads | `covering_index_scan` | 11.85ms | 7.02ms | 0.593× | 1.27% | PASS |
| file_reads | `groupby_scan` | 30.43ms | 33.68ms | 1.107× | 0.94% | PASS |
| file_reads | `index_join` | 10.16ms | 10.06ms | 0.990× | 1.29% | PASS |
| file_reads | `index_join_scan` | 4.41ms | 4.92ms | 1.115× | 1.63% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.32s | 1.279× | 0.65% | PASS |
| file_reads | `table_scan` | 1.16s | 1.39s | 1.196× | 0.79% | PASS |
| file_reads | `oltp_read_only` | 210.47ms | 160.40ms | 0.762× | 0.88% | PASS |
| file_writes | `oltp_bulk_insert` | 194.50ms | 270.63ms | 1.391× | 0.97% | PASS |
| file_writes | `oltp_insert` | 21.87ms | 35.46ms | 1.622× | 1.56% | PASS |
| file_writes | `oltp_update_index` | 75.86ms | 126.65ms | 1.670× | 0.95% | PASS |
| file_writes | `oltp_update_non_index` | 56.91ms | 80.12ms | 1.408× | 1.86% | PASS |
| file_writes | `oltp_delete_insert` | 66.97ms | 98.16ms | 1.466× | 1.77% | PASS |
| file_writes | `oltp_write_only` | 43.61ms | 63.53ms | 1.457× | 1.84% | PASS |
| file_writes | `types_delete_insert` | 39.56ms | 53.07ms | 1.342× | 1.72% | PASS |
| file_writes | `oltp_read_write` | 90.19ms | 129.16ms | 1.432× | 1.43% | PASS |
| ac_reads | `oltp_point_select` | 48.36ms | 54.95ms | 1.136× | 0.83% | PASS |
| ac_reads | `oltp_range_select` | 12.99ms | 15.77ms | 1.214× | 1.22% | PASS |
| ac_reads | `oltp_sum_range` | 12.28ms | 15.15ms | 1.234× | 0.92% | PASS |
| ac_reads | `oltp_order_range` | 2.94ms | 3.30ms | 1.120× | 1.42% | PASS |
| ac_reads | `oltp_distinct_range` | 4.02ms | 4.39ms | 1.092× | 0.97% | PASS |
| ac_reads | `oltp_index_scan` | 6.73ms | 8.32ms | 1.236× | 1.61% | PASS |
| ac_reads | `select_random_points` | 12.80ms | 13.88ms | 1.084× | 2.13% | PASS |
| ac_reads | `select_random_ranges` | 5.54ms | 6.59ms | 1.191× | 1.03% | PASS |
| ac_reads | `covering_index_scan` | 6.90ms | 7.05ms | 1.022× | 1.28% | PASS |
| ac_reads | `groupby_scan` | 29.98ms | 33.71ms | 1.125× | 0.86% | PASS |
| ac_reads | `index_join` | 7.60ms | 10.07ms | 1.326× | 1.90% | PASS |
| ac_reads | `index_join_scan` | 3.91ms | 4.90ms | 1.252× | 1.00% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.33s | 1.283× | 0.67% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.39s | 1.195× | 0.66% | PASS |
| ac_reads | `oltp_read_only` | 138.00ms | 160.53ms | 1.163× | 1.17% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.96ms | 66.23ms | 3.015× | 5.92% | PASS |
| ac_writes | `oltp_insert_ac` | 24.45ms | 82.98ms | 3.393× | 4.97% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.07ms | 101.93ms | 3.631× | 6.70% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.58ms | 79.21ms | 3.508× | 5.74% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.28ms | 89.48ms | 3.685× | 7.36% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.00ms | 86.61ms | 3.464× | 5.40% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.77ms | 78.61ms | 3.612× | 5.01% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.09ms | 93.61ms | 3.217× | 5.10% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.28ms | 28.43ms | 1.125× | 2.10% | PASS |
| mem_reads | `oltp_range_select` | 13.00ms | 11.11ms | 0.854× | 2.58% | PASS |
| mem_reads | `oltp_sum_range` | 11.25ms | 10.97ms | 0.975× | 2.69% | PASS |
| mem_reads | `oltp_order_range` | 2.69ms | 2.56ms | 0.953× | 2.07% | PASS |
| mem_reads | `oltp_distinct_range` | 3.51ms | 3.34ms | 0.952× | 1.14% | PASS |
| mem_reads | `oltp_index_scan` | 3.71ms | 4.77ms | 1.287× | 1.91% | PASS |
| mem_reads | `select_random_points` | 15.01ms | 16.36ms | 1.090× | 3.18% | PASS |
| mem_reads | `select_random_ranges` | 3.59ms | 4.25ms | 1.185× | 1.54% | PASS |
| mem_reads | `covering_index_scan` | 3.83ms | 3.56ms | 0.932× | 2.40% | PASS |
| mem_reads | `groupby_scan` | 27.19ms | 27.99ms | 1.030× | 1.03% | PASS |
| mem_reads | `index_join` | 5.86ms | 7.25ms | 1.238× | 2.91% | PASS |
| mem_reads | `index_join_scan` | 4.23ms | 4.75ms | 1.125× | 2.54% | PASS |
| mem_reads | `types_table_scan` | 913.55ms | 983.01ms | 1.076× | 1.88% | PASS |
| mem_reads | `table_scan` | 1.07s | 1.07s | 0.995× | 1.28% | PASS |
| mem_reads | `oltp_read_only` | 98.89ms | 101.86ms | 1.030× | 2.45% | PASS |
| mem_writes | `oltp_bulk_insert` | 182.49ms | 261.96ms | 1.435× | 1.21% | PASS |
| mem_writes | `oltp_insert` | 17.78ms | 29.91ms | 1.682× | 1.10% | PASS |
| mem_writes | `oltp_update_index` | 60.09ms | 105.76ms | 1.760× | 1.52% | PASS |
| mem_writes | `oltp_update_non_index` | 41.81ms | 68.48ms | 1.638× | 2.05% | PASS |
| mem_writes | `oltp_delete_insert` | 44.10ms | 84.06ms | 1.906× | 1.67% | PASS |
| mem_writes | `oltp_write_only` | 26.55ms | 51.17ms | 1.927× | 2.76% | PASS |
| mem_writes | `types_delete_insert` | 28.02ms | 42.77ms | 1.526× | 1.48% | PASS |
| mem_writes | `oltp_read_write` | 74.28ms | 108.03ms | 1.454× | 2.40% | PASS |
| file_reads | `oltp_point_select` | 99.98ms | 53.14ms | 0.531× | 0.66% | PASS |
| file_reads | `oltp_range_select` | 21.00ms | 13.64ms | 0.650× | 1.03% | PASS |
| file_reads | `oltp_sum_range` | 19.51ms | 13.61ms | 0.698× | 0.98% | PASS |
| file_reads | `oltp_order_range` | 3.60ms | 2.85ms | 0.790× | 0.74% | PASS |
| file_reads | `oltp_distinct_range` | 4.39ms | 3.62ms | 0.824× | 0.69% | PASS |
| file_reads | `oltp_index_scan` | 11.77ms | 7.63ms | 0.648× | 0.76% | PASS |
| file_reads | `select_random_points` | 23.33ms | 19.13ms | 0.820× | 1.19% | PASS |
| file_reads | `select_random_ranges` | 11.24ms | 6.78ms | 0.603× | 0.88% | PASS |
| file_reads | `covering_index_scan` | 12.80ms | 6.43ms | 0.503× | 0.62% | PASS |
| file_reads | `groupby_scan` | 28.10ms | 28.19ms | 1.003× | 0.69% | PASS |
| file_reads | `index_join` | 10.71ms | 9.26ms | 0.865× | 1.00% | PASS |
| file_reads | `index_join_scan` | 5.04ms | 5.17ms | 1.024× | 0.97% | PASS |
| file_reads | `types_table_scan` | 957.58ms | 987.70ms | 1.031× | 3.45% | PASS |
| file_reads | `table_scan` | 1.09s | 1.07s | 0.974× | 2.22% | PASS |
| file_reads | `oltp_read_only` | 214.17ms | 139.64ms | 0.652× | 1.44% | PASS |
| file_writes | `oltp_bulk_insert` | 254.86ms | 352.08ms | 1.381× | 6.42% | PASS |
| file_writes | `oltp_insert` | 70.67ms | 58.94ms | 0.834× | 14.32% | PASS |
| file_writes | `oltp_update_index` | 222.70ms | 210.46ms | 0.945× | 14.00% | PASS |
| file_writes | `oltp_update_non_index` | 160.92ms | 141.39ms | 0.879× | 5.31% | PASS |
| file_writes | `oltp_delete_insert` | 192.24ms | 181.00ms | 0.942× | 13.63% | PASS |
| file_writes | `oltp_write_only` | 131.98ms | 118.45ms | 0.897× | 15.55% | PASS |
| file_writes | `types_delete_insert` | 107.44ms | 104.68ms | 0.974× | 14.80% | PASS |
| file_writes | `oltp_read_write` | 203.36ms | 181.05ms | 0.890× | 18.41% | PASS |
| ac_reads | `oltp_point_select` | 49.27ms | 53.03ms | 1.076× | 0.88% | PASS |
| ac_reads | `oltp_range_select` | 16.16ms | 13.72ms | 0.849× | 2.36% | PASS |
| ac_reads | `oltp_sum_range` | 14.62ms | 13.71ms | 0.938× | 1.79% | PASS |
| ac_reads | `oltp_order_range` | 3.16ms | 2.85ms | 0.902× | 0.94% | PASS |
| ac_reads | `oltp_distinct_range` | 3.91ms | 3.63ms | 0.930× | 0.71% | PASS |
| ac_reads | `oltp_index_scan` | 6.93ms | 7.68ms | 1.107× | 1.28% | PASS |
| ac_reads | `select_random_points` | 18.50ms | 19.21ms | 1.038× | 1.22% | PASS |
| ac_reads | `select_random_ranges` | 6.29ms | 6.77ms | 1.077× | 0.71% | PASS |
| ac_reads | `covering_index_scan` | 7.82ms | 6.43ms | 0.822× | 0.58% | PASS |
| ac_reads | `groupby_scan` | 27.83ms | 28.33ms | 1.018× | 0.83% | PASS |
| ac_reads | `index_join` | 8.47ms | 9.56ms | 1.129× | 1.62% | PASS |
| ac_reads | `index_join_scan` | 4.67ms | 5.22ms | 1.119× | 0.88% | PASS |
| ac_reads | `types_table_scan` | 1.01s | 1.01s | 1.000× | 4.40% | PASS |
| ac_reads | `table_scan` | 1.33s | 1.11s | 0.832× | 4.79% | PASS |
| ac_reads | `oltp_read_only` | 140.72ms | 139.68ms | 0.993× | 0.99% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 53.75ms | 205.67ms | 3.826× | 67.69% | PASS |
| ac_writes | `oltp_insert_ac` | 34.71ms | 108.90ms | 3.137× | 41.36% | PASS |
| ac_writes | `oltp_update_index_ac` | 37.37ms | 173.57ms | 4.645× | 49.96% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 37.73ms | 139.13ms | 3.687× | 54.73% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.64ms | 103.50ms | 4.200× | 31.59% | PASS |
| ac_writes | `oltp_write_only_ac` | 48.12ms | 139.57ms | 2.900× | 52.91% | PASS |
| ac_writes | `types_delete_insert_ac` | 27.11ms | 109.83ms | 4.051× | 47.83% | PASS |
| ac_writes | `oltp_read_write_ac` | 38.08ms | 119.72ms | 3.144× | 37.71% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.45ms | 24.93ms | 1.110× | 5.20% | PASS |
| mem_reads | `oltp_range_select` | 10.62ms | 10.91ms | 1.027× | 3.82% | PASS |
| mem_reads | `oltp_sum_range` | 9.42ms | 9.99ms | 1.061× | 5.18% | PASS |
| mem_reads | `oltp_order_range` | 2.36ms | 2.52ms | 1.070× | 3.28% | PASS |
| mem_reads | `oltp_distinct_range` | 3.14ms | 3.30ms | 1.049× | 3.51% | PASS |
| mem_reads | `oltp_index_scan` | 3.36ms | 4.13ms | 1.231× | 4.62% | PASS |
| mem_reads | `select_random_points` | 13.30ms | 15.43ms | 1.161× | 3.82% | PASS |
| mem_reads | `select_random_ranges` | 2.85ms | 3.73ms | 1.310× | 4.18% | PASS |
| mem_reads | `covering_index_scan` | 3.12ms | 3.08ms | 0.985× | 2.52% | PASS |
| mem_reads | `groupby_scan` | 26.26ms | 27.30ms | 1.040× | 3.19% | PASS |
| mem_reads | `index_join` | 5.41ms | 6.96ms | 1.287× | 5.79% | PASS |
| mem_reads | `index_join_scan` | 3.27ms | 4.56ms | 1.397× | 5.65% | PASS |
| mem_reads | `types_table_scan` | 897.22ms | 1.04s | 1.162× | 0.68% | PASS |
| mem_reads | `table_scan` | 1.04s | 1.13s | 1.090× | 0.56% | PASS |
| mem_reads | `oltp_read_only` | 92.13ms | 102.69ms | 1.115× | 1.45% | PASS |
| mem_writes | `oltp_bulk_insert` | 166.89ms | 233.41ms | 1.399× | 1.33% | PASS |
| mem_writes | `oltp_insert` | 14.88ms | 27.12ms | 1.823× | 3.29% | PASS |
| mem_writes | `oltp_update_index` | 51.56ms | 94.30ms | 1.829× | 1.95% | PASS |
| mem_writes | `oltp_update_non_index` | 35.36ms | 59.55ms | 1.684× | 1.64% | PASS |
| mem_writes | `oltp_delete_insert` | 37.03ms | 74.09ms | 2.001× | 2.05% | PASS |
| mem_writes | `oltp_write_only` | 20.29ms | 43.39ms | 2.138× | 2.75% | PASS |
| mem_writes | `types_delete_insert` | 23.48ms | 37.80ms | 1.610× | 3.87% | PASS |
| mem_writes | `oltp_read_write` | 60.14ms | 97.98ms | 1.629× | 1.46% | PASS |
| file_reads | `oltp_point_select` | 49.86ms | 34.92ms | 0.700× | 1.40% | PASS |
| file_reads | `oltp_range_select` | 13.01ms | 12.24ms | 0.941× | 4.62% | PASS |
| file_reads | `oltp_sum_range` | 12.12ms | 11.14ms | 0.919× | 3.53% | PASS |
| file_reads | `oltp_order_range` | 2.70ms | 2.68ms | 0.993× | 3.74% | PASS |
| file_reads | `oltp_distinct_range` | 3.64ms | 3.53ms | 0.971× | 6.32% | PASS |
| file_reads | `oltp_index_scan` | 6.27ms | 5.45ms | 0.868× | 3.39% | PASS |
| file_reads | `select_random_points` | 17.77ms | 17.84ms | 1.004× | 3.73% | PASS |
| file_reads | `select_random_ranges` | 5.56ms | 4.64ms | 0.835× | 3.49% | PASS |
| file_reads | `covering_index_scan` | 5.97ms | 4.02ms | 0.673× | 4.44% | PASS |
| file_reads | `groupby_scan` | 26.70ms | 27.76ms | 1.039× | 3.04% | PASS |
| file_reads | `index_join` | 7.00ms | 7.66ms | 1.095× | 4.44% | PASS |
| file_reads | `index_join_scan` | 3.39ms | 4.69ms | 1.383× | 4.74% | PASS |
| file_reads | `types_table_scan` | 898.46ms | 1.04s | 1.159× | 0.59% | PASS |
| file_reads | `table_scan` | 1.04s | 1.14s | 1.095× | 0.48% | PASS |
| file_reads | `oltp_read_only` | 130.75ms | 115.43ms | 0.883× | 1.88% | PASS |
| file_writes | `oltp_bulk_insert` | 226.60ms | 307.35ms | 1.356× | 2.31% | PASS |
| file_writes | `oltp_insert` | 39.94ms | 56.26ms | 1.409× | 5.32% | PASS |
| file_writes | `oltp_update_index` | 175.62ms | 184.60ms | 1.051× | 2.91% | PASS |
| file_writes | `oltp_update_non_index` | 136.31ms | 127.14ms | 0.933× | 11.17% | PASS |
| file_writes | `oltp_delete_insert` | 144.63ms | 151.35ms | 1.046× | 9.36% | PASS |
| file_writes | `oltp_write_only` | 105.75ms | 103.53ms | 0.979× | 1.54% | PASS |
| file_writes | `types_delete_insert` | 90.60ms | 83.41ms | 0.921× | 2.45% | PASS |
| file_writes | `oltp_read_write` | 146.32ms | 160.55ms | 1.097× | 2.28% | PASS |
| ac_reads | `oltp_point_select` | 32.48ms | 35.86ms | 1.104× | 3.07% | PASS |
| ac_reads | `oltp_range_select` | 11.93ms | 12.24ms | 1.026× | 3.75% | PASS |
| ac_reads | `oltp_sum_range` | 11.81ms | 11.70ms | 0.990× | 4.58% | PASS |
| ac_reads | `oltp_order_range` | 2.62ms | 2.72ms | 1.038× | 4.29% | PASS |
| ac_reads | `oltp_distinct_range` | 3.46ms | 3.52ms | 1.017× | 4.56% | PASS |
| ac_reads | `oltp_index_scan` | 4.64ms | 5.45ms | 1.174× | 5.65% | PASS |
| ac_reads | `select_random_points` | 16.06ms | 17.91ms | 1.115× | 3.34% | PASS |
| ac_reads | `select_random_ranges` | 4.13ms | 4.88ms | 1.182× | 2.63% | PASS |
| ac_reads | `covering_index_scan` | 4.19ms | 4.10ms | 0.979× | 4.48% | PASS |
| ac_reads | `groupby_scan` | 27.98ms | 28.30ms | 1.011× | 3.99% | PASS |
| ac_reads | `index_join` | 6.42ms | 8.36ms | 1.303× | 3.72% | PASS |
| ac_reads | `index_join_scan` | 3.68ms | 4.98ms | 1.354× | 3.81% | PASS |
| ac_reads | `types_table_scan` | 914.20ms | 1.06s | 1.162× | 0.71% | PASS |
| ac_reads | `table_scan` | 1.05s | 1.15s | 1.093× | 0.71% | PASS |
| ac_reads | `oltp_read_only` | 106.00ms | 116.84ms | 1.102× | 1.80% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.84ms | 66.44ms | 2.674× | 3.51% | PASS |
| ac_writes | `oltp_insert_ac` | 26.64ms | 78.96ms | 2.964× | 3.70% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.58ms | 86.63ms | 3.141× | 6.31% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.50ms | 73.96ms | 2.900× | 5.41% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.38ms | 82.09ms | 3.112× | 5.53% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.57ms | 82.80ms | 3.117× | 7.83% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.97ms | 74.95ms | 3.002× | 5.71% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.00ms | 86.81ms | 2.894× | 5.28% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.16ms | 25.72ms | 1.111× | 2.48% | PASS |
| mem_reads | `oltp_range_select` | 12.55ms | 12.35ms | 0.984× | 2.24% | PASS |
| mem_reads | `oltp_sum_range` | 11.38ms | 12.05ms | 1.058× | 1.78% | PASS |
| mem_reads | `oltp_order_range` | 2.41ms | 2.27ms | 0.942× | 1.60% | PASS |
| mem_reads | `oltp_distinct_range` | 2.93ms | 2.79ms | 0.953× | 1.32% | PASS |
| mem_reads | `oltp_index_scan` | 3.19ms | 4.37ms | 1.370× | 2.13% | PASS |
| mem_reads | `select_random_points` | 19.49ms | 21.61ms | 1.109× | 2.75% | PASS |
| mem_reads | `select_random_ranges` | 5.02ms | 5.48ms | 1.091× | 1.30% | PASS |
| mem_reads | `covering_index_scan` | 2.52ms | 3.17ms | 1.257× | 3.91% | PASS |
| mem_reads | `groupby_scan` | 21.99ms | 22.34ms | 1.016× | 1.56% | PASS |
| mem_reads | `index_join` | 5.15ms | 7.44ms | 1.445× | 4.76% | PASS |
| mem_reads | `index_join_scan` | 3.09ms | 4.80ms | 1.555× | 2.76% | PASS |
| mem_reads | `types_table_scan` | 738.08ms | 784.18ms | 1.062× | 1.98% | PASS |
| mem_reads | `table_scan` | 852.76ms | 869.93ms | 1.020× | 2.83% | PASS |
| mem_reads | `oltp_read_only` | 85.14ms | 89.32ms | 1.049× | 2.67% | PASS |
| mem_writes | `oltp_bulk_insert` | 149.42ms | 206.84ms | 1.384× | 0.78% | PASS |
| mem_writes | `oltp_insert` | 11.50ms | 22.29ms | 1.938× | 1.15% | PASS |
| mem_writes | `oltp_update_index` | 41.62ms | 76.00ms | 1.826× | 1.34% | PASS |
| mem_writes | `oltp_update_non_index` | 32.26ms | 54.99ms | 1.705× | 1.83% | PASS |
| mem_writes | `oltp_delete_insert` | 30.16ms | 60.38ms | 2.002× | 1.29% | PASS |
| mem_writes | `oltp_write_only` | 16.86ms | 38.66ms | 2.294× | 1.89% | PASS |
| mem_writes | `types_delete_insert` | 20.25ms | 34.29ms | 1.693× | 3.90% | PASS |
| mem_writes | `oltp_read_write` | 54.52ms | 85.98ms | 1.577× | 1.50% | PASS |
| file_reads | `oltp_point_select` | 79.94ms | 44.29ms | 0.554× | 1.50% | PASS |
| file_reads | `oltp_range_select` | 17.45ms | 13.70ms | 0.785× | 1.32% | PASS |
| file_reads | `oltp_sum_range` | 16.57ms | 13.49ms | 0.814× | 1.30% | PASS |
| file_reads | `oltp_order_range` | 3.00ms | 2.47ms | 0.824× | 1.20% | PASS |
| file_reads | `oltp_distinct_range` | 3.54ms | 2.99ms | 0.843× | 1.13% | PASS |
| file_reads | `oltp_index_scan` | 9.24ms | 5.87ms | 0.635× | 1.26% | PASS |
| file_reads | `select_random_points` | 23.47ms | 21.22ms | 0.904× | 1.48% | PASS |
| file_reads | `select_random_ranges` | 10.60ms | 7.27ms | 0.686× | 1.12% | PASS |
| file_reads | `covering_index_scan` | 8.74ms | 4.60ms | 0.527× | 1.14% | PASS |
| file_reads | `groupby_scan` | 21.73ms | 21.92ms | 1.009× | 0.93% | PASS |
| file_reads | `index_join` | 8.33ms | 7.57ms | 0.909× | 1.72% | PASS |
| file_reads | `index_join_scan` | 3.44ms | 4.30ms | 1.250× | 2.20% | PASS |
| file_reads | `types_table_scan` | 697.37ms | 750.32ms | 1.076× | 1.65% | PASS |
| file_reads | `table_scan` | 864.53ms | 855.25ms | 0.989× | 4.46% | PASS |
| file_reads | `oltp_read_only` | 163.87ms | 115.94ms | 0.708× | 2.08% | PASS |
| file_writes | `oltp_bulk_insert` | 197.22ms | 260.13ms | 1.319× | 3.22% | PASS |
| file_writes | `oltp_insert` | 19.39ms | 38.53ms | 1.987× | 5.99% | PASS |
| file_writes | `oltp_update_index` | 150.39ms | 153.48ms | 1.021× | 3.73% | PASS |
| file_writes | `oltp_update_non_index` | 126.64ms | 110.92ms | 0.876× | 3.78% | PASS |
| file_writes | `oltp_delete_insert` | 129.88ms | 124.06ms | 0.955× | 3.24% | PASS |
| file_writes | `oltp_write_only` | 89.97ms | 94.29ms | 1.048× | 4.98% | PASS |
| file_writes | `types_delete_insert` | 79.06ms | 77.80ms | 0.984× | 7.51% | PASS |
| file_writes | `oltp_read_write` | 134.53ms | 149.13ms | 1.108× | 6.51% | PASS |
| ac_reads | `oltp_point_select` | 43.10ms | 46.80ms | 1.086× | 2.03% | PASS |
| ac_reads | `oltp_range_select` | 15.13ms | 14.63ms | 0.967× | 2.76% | PASS |
| ac_reads | `oltp_sum_range` | 14.58ms | 14.84ms | 1.018× | 4.34% | PASS |
| ac_reads | `oltp_order_range` | 2.86ms | 2.74ms | 0.958× | 5.45% | PASS |
| ac_reads | `oltp_distinct_range` | 3.51ms | 3.36ms | 0.958× | 4.41% | PASS |
| ac_reads | `oltp_index_scan` | 6.31ms | 7.58ms | 1.202× | 7.01% | PASS |
| ac_reads | `select_random_points` | 25.14ms | 26.93ms | 1.071× | 4.58% | PASS |
| ac_reads | `select_random_ranges` | 7.79ms | 8.50ms | 1.091× | 7.46% | PASS |
| ac_reads | `covering_index_scan` | 5.89ms | 6.11ms | 1.038× | 10.11% | PASS |
| ac_reads | `groupby_scan` | 24.28ms | 24.00ms | 0.989× | 3.75% | PASS |
| ac_reads | `index_join` | 7.01ms | 8.60ms | 1.226× | 6.31% | PASS |
| ac_reads | `index_join_scan` | 3.26ms | 4.38ms | 1.346× | 3.66% | PASS |
| ac_reads | `types_table_scan` | 854.81ms | 821.85ms | 0.961× | 4.71% | PASS |
| ac_reads | `table_scan` | 1.05s | 910.01ms | 0.870× | 1.96% | PASS |
| ac_reads | `oltp_read_only` | 124.94ms | 124.43ms | 0.996× | 2.39% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.03ms | 57.99ms | 2.632× | 3.40% | PASS |
| ac_writes | `oltp_insert_ac` | 27.29ms | 77.96ms | 2.857× | 6.20% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.07ms | 84.42ms | 2.904× | 4.39% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.43ms | 68.40ms | 3.050× | 3.09% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.45ms | 79.94ms | 3.022× | 6.15% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.33ms | 77.67ms | 2.950× | 4.91% | PASS |
| ac_writes | `types_delete_insert_ac` | 26.61ms | 66.60ms | 2.503× | 9.89% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.66ms | 79.25ms | 2.765× | 4.00% | PASS |

</details>

## Version-control latency

Wall time: 2m 3s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 67.29ms | 200.00ms | 33.6% | 0.50% | PASS |
| `status_dirty_many_tables` | 70.35ms | 200.00ms | 35.2% | 0.62% | PASS |
| `diff_regular_working_one_table` | 62.84ms | 150.00ms | 41.9% | 0.52% | PASS |
| `diff_regular_working_many_tables` | 74.82ms | 200.00ms | 37.4% | 0.52% | PASS |
| `diff_stat_working_many_tables` | 74.60ms | 200.00ms | 37.3% | 0.42% | PASS |
| `diff_schema_working_many_tables` | 74.98ms | 200.00ms | 37.5% | 0.80% | PASS |
| `branch_list_many_branches` | 20.34ms | 100.00ms | 20.3% | 1.64% | PASS |
| `branch_create_delete` | 30.28ms | 100.00ms | 30.3% | 1.64% | PASS |
| `checkout_branch_clean` | 97.85ms | 200.00ms | 48.9% | 0.32% | PASS |
| `merge_data_no_conflicts` | 37.32ms | 150.00ms | 24.9% | 1.12% | PASS |
| `merge_schema_no_conflicts` | 21.19ms | 100.00ms | 21.2% | 1.49% | PASS |
| `merge_data_conflicts` | 78.32ms | 250.00ms | 31.3% | 0.88% | PASS |
| `merge_data_conflicts_with_resolve` | 77.73ms | 250.00ms | 31.1% | 0.62% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
