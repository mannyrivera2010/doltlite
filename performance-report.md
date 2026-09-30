# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-30 17:44 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260920.314.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/36741245933)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 26s | 9.22s | 11.54s | 1.252× | 1.75% | **PASS** |
| textpk | 69 | 55 | 1h 38m 49s | 11.51s | 12.33s | 1.072× | 2.55% | **PASS** |
| blobpk | 69 | 55 | 1h 8m 53s | 6.90s | 8.13s | 1.179× | 2.08% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 54s | 9.90s | 12.06s | 1.218× | 1.50% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.44ms | 29.50ms | 1.207× | 1.18% | PASS |
| mem_reads | `oltp_range_select` | 10.57ms | 13.18ms | 1.247× | 1.83% | PASS |
| mem_reads | `oltp_sum_range` | 9.76ms | 12.46ms | 1.276× | 2.05% | PASS |
| mem_reads | `oltp_order_range` | 2.57ms | 3.01ms | 1.171× | 1.10% | PASS |
| mem_reads | `oltp_distinct_range` | 3.70ms | 4.05ms | 1.093× | 1.13% | PASS |
| mem_reads | `oltp_index_scan` | 4.00ms | 5.44ms | 1.360× | 1.95% | PASS |
| mem_reads | `select_random_points` | 10.41ms | 11.35ms | 1.090× | 2.58% | PASS |
| mem_reads | `select_random_ranges` | 3.04ms | 4.02ms | 1.320× | 1.89% | PASS |
| mem_reads | `covering_index_scan` | 4.25ms | 4.26ms | 1.002× | 2.10% | PASS |
| mem_reads | `groupby_scan` | 30.33ms | 33.48ms | 1.104× | 0.80% | PASS |
| mem_reads | `index_join` | 6.04ms | 8.29ms | 1.371× | 2.31% | PASS |
| mem_reads | `index_join_scan` | 3.57ms | 4.68ms | 1.311× | 2.98% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.34s | 1.271× | 1.19% | PASS |
| mem_reads | `table_scan` | 1.25s | 1.42s | 1.131× | 1.03% | PASS |
| mem_reads | `oltp_read_only` | 107.51ms | 125.22ms | 1.165× | 1.69% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.01ms | 254.81ms | 1.408× | 1.16% | PASS |
| mem_writes | `oltp_insert` | 15.71ms | 28.71ms | 1.828× | 0.94% | PASS |
| mem_writes | `oltp_update_index` | 52.89ms | 109.39ms | 2.068× | 2.32% | PASS |
| mem_writes | `oltp_update_non_index` | 35.79ms | 61.57ms | 1.720× | 2.42% | PASS |
| mem_writes | `oltp_delete_insert` | 47.04ms | 81.41ms | 1.731× | 1.69% | PASS |
| mem_writes | `oltp_write_only` | 22.89ms | 46.07ms | 2.012× | 1.93% | PASS |
| mem_writes | `types_delete_insert` | 25.32ms | 40.93ms | 1.616× | 1.76% | PASS |
| mem_writes | `oltp_read_write` | 69.88ms | 112.68ms | 1.613× | 2.49% | PASS |
| file_reads | `oltp_point_select` | 100.00ms | 55.70ms | 0.557× | 1.01% | PASS |
| file_reads | `oltp_range_select` | 18.74ms | 15.92ms | 0.849× | 1.84% | PASS |
| file_reads | `oltp_sum_range` | 18.07ms | 15.20ms | 0.841× | 1.39% | PASS |
| file_reads | `oltp_order_range` | 3.51ms | 3.33ms | 0.950× | 1.47% | PASS |
| file_reads | `oltp_distinct_range` | 4.59ms | 4.40ms | 0.959× | 1.08% | PASS |
| file_reads | `oltp_index_scan` | 11.87ms | 8.37ms | 0.705× | 1.43% | PASS |
| file_reads | `select_random_points` | 18.86ms | 14.13ms | 0.750× | 2.80% | PASS |
| file_reads | `select_random_ranges` | 10.61ms | 6.64ms | 0.627× | 0.97% | PASS |
| file_reads | `covering_index_scan` | 12.15ms | 7.11ms | 0.585× | 1.92% | PASS |
| file_reads | `groupby_scan` | 31.12ms | 33.75ms | 1.085× | 0.72% | PASS |
| file_reads | `index_join` | 10.54ms | 10.39ms | 0.986× | 1.93% | PASS |
| file_reads | `index_join_scan` | 4.59ms | 5.09ms | 1.109× | 1.13% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.34s | 1.263× | 1.05% | PASS |
| file_reads | `table_scan` | 1.27s | 1.42s | 1.120× | 0.69% | PASS |
| file_reads | `oltp_read_only` | 217.78ms | 163.66ms | 0.751× | 0.89% | PASS |
| file_writes | `oltp_bulk_insert` | 195.10ms | 272.76ms | 1.398× | 1.09% | PASS |
| file_writes | `oltp_insert` | 22.82ms | 35.88ms | 1.572× | 1.47% | PASS |
| file_writes | `oltp_update_index` | 79.95ms | 130.94ms | 1.638× | 2.26% | PASS |
| file_writes | `oltp_update_non_index` | 60.13ms | 82.28ms | 1.368× | 2.40% | PASS |
| file_writes | `oltp_delete_insert` | 69.77ms | 100.06ms | 1.434× | 1.75% | PASS |
| file_writes | `oltp_write_only` | 46.13ms | 64.70ms | 1.403× | 2.44% | PASS |
| file_writes | `types_delete_insert` | 41.44ms | 54.00ms | 1.303× | 2.21% | PASS |
| file_writes | `oltp_read_write` | 95.16ms | 131.04ms | 1.377× | 1.71% | PASS |
| ac_reads | `oltp_point_select` | 49.24ms | 55.14ms | 1.120× | 1.19% | PASS |
| ac_reads | `oltp_range_select` | 13.42ms | 15.90ms | 1.185× | 1.79% | PASS |
| ac_reads | `oltp_sum_range` | 12.82ms | 15.20ms | 1.186× | 1.68% | PASS |
| ac_reads | `oltp_order_range` | 2.99ms | 3.35ms | 1.120× | 1.88% | PASS |
| ac_reads | `oltp_distinct_range` | 4.04ms | 4.41ms | 1.091× | 1.52% | PASS |
| ac_reads | `oltp_index_scan` | 6.90ms | 8.42ms | 1.221× | 1.50% | PASS |
| ac_reads | `select_random_points` | 13.78ms | 14.15ms | 1.027× | 2.77% | PASS |
| ac_reads | `select_random_ranges` | 5.68ms | 6.66ms | 1.172× | 1.13% | PASS |
| ac_reads | `covering_index_scan` | 7.32ms | 7.20ms | 0.983× | 1.66% | PASS |
| ac_reads | `groupby_scan` | 30.55ms | 34.05ms | 1.114× | 0.71% | PASS |
| ac_reads | `index_join` | 7.96ms | 10.46ms | 1.315× | 2.20% | PASS |
| ac_reads | `index_join_scan` | 4.05ms | 5.13ms | 1.268× | 2.31% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.34s | 1.262× | 1.18% | PASS |
| ac_reads | `table_scan` | 1.26s | 1.42s | 1.125× | 1.47% | PASS |
| ac_reads | `oltp_read_only` | 141.75ms | 162.85ms | 1.149× | 1.38% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.08ms | 70.17ms | 3.040× | 7.64% | PASS |
| ac_writes | `oltp_insert_ac` | 26.39ms | 87.16ms | 3.303× | 7.83% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.08ms | 101.30ms | 3.607× | 6.29% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.24ms | 81.17ms | 3.216× | 6.67% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.61ms | 92.94ms | 3.629× | 7.31% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.87ms | 92.18ms | 3.430× | 8.15% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.86ms | 84.38ms | 3.537× | 8.49% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.92ms | 98.57ms | 3.088× | 8.70% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.19ms | 38.73ms | 1.203× | 2.44% | PASS |
| mem_reads | `oltp_range_select` | 15.21ms | 14.44ms | 0.950× | 2.70% | PASS |
| mem_reads | `oltp_sum_range` | 13.65ms | 14.56ms | 1.067× | 2.93% | PASS |
| mem_reads | `oltp_order_range` | 3.15ms | 3.22ms | 1.020× | 1.74% | PASS |
| mem_reads | `oltp_distinct_range` | 4.15ms | 4.30ms | 1.036× | 1.52% | PASS |
| mem_reads | `oltp_index_scan` | 4.98ms | 6.71ms | 1.347× | 4.30% | PASS |
| mem_reads | `select_random_points` | 20.62ms | 22.02ms | 1.068× | 3.60% | PASS |
| mem_reads | `select_random_ranges` | 4.42ms | 5.35ms | 1.212× | 3.25% | PASS |
| mem_reads | `covering_index_scan` | 5.52ms | 4.86ms | 0.879× | 7.17% | PASS |
| mem_reads | `groupby_scan` | 33.12ms | 35.11ms | 1.060× | 1.06% | PASS |
| mem_reads | `index_join` | 8.01ms | 10.13ms | 1.265× | 3.93% | PASS |
| mem_reads | `index_join_scan` | 5.68ms | 5.73ms | 1.009× | 4.61% | PASS |
| mem_reads | `types_table_scan` | 1.26s | 1.29s | 1.023× | 1.46% | PASS |
| mem_reads | `table_scan` | 1.49s | 1.42s | 0.948× | 3.60% | PASS |
| mem_reads | `oltp_read_only` | 131.91ms | 141.39ms | 1.072× | 2.60% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.08ms | 366.86ms | 1.554× | 1.18% | PASS |
| mem_writes | `oltp_insert` | 23.85ms | 40.61ms | 1.703× | 3.48% | PASS |
| mem_writes | `oltp_update_index` | 83.10ms | 144.67ms | 1.741× | 2.65% | PASS |
| mem_writes | `oltp_update_non_index` | 52.45ms | 91.25ms | 1.740× | 2.31% | PASS |
| mem_writes | `oltp_delete_insert` | 58.98ms | 111.09ms | 1.884× | 3.00% | PASS |
| mem_writes | `oltp_write_only` | 33.07ms | 65.14ms | 1.969× | 2.45% | PASS |
| mem_writes | `types_delete_insert` | 35.75ms | 57.83ms | 1.617× | 1.42% | PASS |
| mem_writes | `oltp_read_write` | 95.36ms | 147.28ms | 1.544× | 2.39% | PASS |
| file_reads | `oltp_point_select` | 108.39ms | 65.44ms | 0.604× | 1.28% | PASS |
| file_reads | `oltp_range_select` | 23.70ms | 17.59ms | 0.742× | 2.55% | PASS |
| file_reads | `oltp_sum_range` | 22.14ms | 17.85ms | 0.806× | 2.19% | PASS |
| file_reads | `oltp_order_range` | 4.20ms | 3.64ms | 0.866× | 3.11% | PASS |
| file_reads | `oltp_distinct_range` | 5.31ms | 4.77ms | 0.899× | 1.75% | PASS |
| file_reads | `oltp_index_scan` | 13.38ms | 9.70ms | 0.725× | 2.04% | PASS |
| file_reads | `select_random_points` | 30.55ms | 26.69ms | 0.873× | 2.17% | PASS |
| file_reads | `select_random_ranges` | 12.30ms | 8.10ms | 0.658× | 1.70% | PASS |
| file_reads | `covering_index_scan` | 14.44ms | 7.68ms | 0.532× | 1.68% | PASS |
| file_reads | `groupby_scan` | 34.93ms | 35.85ms | 1.027× | 0.82% | PASS |
| file_reads | `index_join` | 13.33ms | 12.35ms | 0.927× | 2.53% | PASS |
| file_reads | `index_join_scan` | 6.59ms | 6.42ms | 0.973× | 4.82% | PASS |
| file_reads | `types_table_scan` | 1.30s | 1.30s | 0.999× | 0.75% | PASS |
| file_reads | `table_scan` | 1.58s | 1.44s | 0.916× | 1.06% | PASS |
| file_reads | `oltp_read_only` | 250.73ms | 186.87ms | 0.745× | 0.83% | PASS |
| file_writes | `oltp_bulk_insert` | 257.07ms | 402.12ms | 1.564× | 0.80% | PASS |
| file_writes | `oltp_insert` | 57.57ms | 54.71ms | 0.950× | 16.96% | PASS |
| file_writes | `oltp_update_index` | 131.12ms | 184.84ms | 1.410× | 2.79% | PASS |
| file_writes | `oltp_update_non_index` | 110.82ms | 120.17ms | 1.084× | 9.68% | PASS |
| file_writes | `oltp_delete_insert` | 99.70ms | 142.94ms | 1.434× | 2.04% | PASS |
| file_writes | `oltp_write_only` | 91.52ms | 90.11ms | 0.985× | 16.47% | PASS |
| file_writes | `types_delete_insert` | 61.34ms | 79.75ms | 1.300× | 2.12% | PASS |
| file_writes | `oltp_read_write` | 158.64ms | 178.42ms | 1.125× | 6.03% | PASS |
| ac_reads | `oltp_point_select` | 59.28ms | 67.42ms | 1.137× | 1.87% | PASS |
| ac_reads | `oltp_range_select` | 19.07ms | 18.09ms | 0.949× | 3.37% | PASS |
| ac_reads | `oltp_sum_range` | 16.76ms | 17.96ms | 1.072× | 1.98% | PASS |
| ac_reads | `oltp_order_range` | 3.71ms | 3.72ms | 1.003× | 2.11% | PASS |
| ac_reads | `oltp_distinct_range` | 4.89ms | 4.93ms | 1.008× | 2.65% | PASS |
| ac_reads | `oltp_index_scan` | 8.13ms | 9.77ms | 1.202× | 1.58% | PASS |
| ac_reads | `select_random_points` | 23.98ms | 26.89ms | 1.121× | 2.60% | PASS |
| ac_reads | `select_random_ranges` | 7.15ms | 8.13ms | 1.137× | 1.40% | PASS |
| ac_reads | `covering_index_scan` | 9.11ms | 7.67ms | 0.842× | 2.60% | PASS |
| ac_reads | `groupby_scan` | 33.66ms | 35.62ms | 1.058× | 1.12% | PASS |
| ac_reads | `index_join` | 10.46ms | 12.01ms | 1.148× | 4.74% | PASS |
| ac_reads | `index_join_scan` | 5.96ms | 6.35ms | 1.066× | 4.92% | PASS |
| ac_reads | `types_table_scan` | 1.29s | 1.30s | 1.006× | 0.83% | PASS |
| ac_reads | `table_scan` | 1.57s | 1.43s | 0.912× | 1.75% | PASS |
| ac_reads | `oltp_read_only` | 169.63ms | 183.21ms | 1.080× | 1.81% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 26.88ms | 76.29ms | 2.838× | 9.65% | PASS |
| ac_writes | `oltp_insert_ac` | 29.69ms | 91.01ms | 3.065× | 6.13% | PASS |
| ac_writes | `oltp_update_index_ac` | 31.92ms | 108.73ms | 3.406× | 6.98% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 26.39ms | 89.25ms | 3.382× | 10.04% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 28.32ms | 102.49ms | 3.619× | 10.01% | PASS |
| ac_writes | `oltp_write_only_ac` | 29.61ms | 98.12ms | 3.313× | 7.55% | PASS |
| ac_writes | `types_delete_insert_ac` | 26.21ms | 92.13ms | 3.515× | 10.63% | PASS |
| ac_writes | `oltp_read_write_ac` | 35.51ms | 109.64ms | 3.088× | 8.20% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 19.60ms | 21.97ms | 1.121× | 1.37% | PASS |
| mem_reads | `oltp_range_select` | 8.77ms | 8.55ms | 0.975× | 2.76% | PASS |
| mem_reads | `oltp_sum_range` | 9.00ms | 8.60ms | 0.956× | 2.06% | PASS |
| mem_reads | `oltp_order_range` | 1.99ms | 1.93ms | 0.970× | 1.88% | PASS |
| mem_reads | `oltp_distinct_range` | 2.52ms | 2.39ms | 0.948× | 1.45% | PASS |
| mem_reads | `oltp_index_scan` | 2.97ms | 4.16ms | 1.400× | 1.90% | PASS |
| mem_reads | `select_random_points` | 12.94ms | 13.88ms | 1.072× | 2.62% | PASS |
| mem_reads | `select_random_ranges` | 2.98ms | 3.42ms | 1.145× | 2.06% | PASS |
| mem_reads | `covering_index_scan` | 2.52ms | 3.18ms | 1.258× | 3.47% | PASS |
| mem_reads | `groupby_scan` | 19.51ms | 18.98ms | 0.973× | 1.52% | PASS |
| mem_reads | `index_join` | 4.38ms | 6.52ms | 1.488× | 3.72% | PASS |
| mem_reads | `index_join_scan` | 3.45ms | 4.74ms | 1.373× | 2.39% | PASS |
| mem_reads | `types_table_scan` | 695.76ms | 737.13ms | 1.059× | 1.43% | PASS |
| mem_reads | `table_scan` | 864.92ms | 844.13ms | 0.976× | 2.96% | PASS |
| mem_reads | `oltp_read_only` | 73.53ms | 73.83ms | 1.004× | 2.38% | PASS |
| mem_writes | `oltp_bulk_insert` | 137.70ms | 200.13ms | 1.453× | 1.41% | PASS |
| mem_writes | `oltp_insert` | 11.57ms | 24.41ms | 2.109× | 1.51% | PASS |
| mem_writes | `oltp_update_index` | 43.53ms | 87.94ms | 2.020× | 1.69% | PASS |
| mem_writes | `oltp_update_non_index` | 32.64ms | 56.97ms | 1.745× | 1.86% | PASS |
| mem_writes | `oltp_delete_insert` | 31.19ms | 68.88ms | 2.208× | 2.44% | PASS |
| mem_writes | `oltp_write_only` | 18.32ms | 42.89ms | 2.341× | 2.39% | PASS |
| mem_writes | `types_delete_insert` | 20.95ms | 36.36ms | 1.735× | 1.67% | PASS |
| mem_writes | `oltp_read_write` | 53.09ms | 84.12ms | 1.584× | 2.12% | PASS |
| file_reads | `oltp_point_select` | 74.56ms | 40.83ms | 0.548× | 1.67% | PASS |
| file_reads | `oltp_range_select` | 14.46ms | 10.17ms | 0.704× | 2.08% | PASS |
| file_reads | `oltp_sum_range` | 14.62ms | 10.10ms | 0.690× | 1.73% | PASS |
| file_reads | `oltp_order_range` | 2.66ms | 2.14ms | 0.805× | 1.20% | PASS |
| file_reads | `oltp_distinct_range` | 3.17ms | 2.60ms | 0.819× | 1.13% | PASS |
| file_reads | `oltp_index_scan` | 9.04ms | 6.10ms | 0.674× | 1.35% | PASS |
| file_reads | `select_random_points` | 18.46ms | 14.93ms | 0.809× | 2.70% | PASS |
| file_reads | `select_random_ranges` | 8.54ms | 5.33ms | 0.624× | 1.47% | PASS |
| file_reads | `covering_index_scan` | 9.13ms | 5.09ms | 0.557× | 1.89% | PASS |
| file_reads | `groupby_scan` | 19.93ms | 18.84ms | 0.945× | 0.89% | PASS |
| file_reads | `index_join` | 7.97ms | 7.33ms | 0.919× | 2.42% | PASS |
| file_reads | `index_join_scan` | 3.74ms | 4.33ms | 1.157× | 2.57% | PASS |
| file_reads | `types_table_scan` | 670.00ms | 722.41ms | 1.078× | 1.37% | PASS |
| file_reads | `table_scan` | 778.84ms | 807.94ms | 1.037× | 1.71% | PASS |
| file_reads | `oltp_read_only` | 142.73ms | 96.51ms | 0.676× | 1.09% | PASS |
| file_writes | `oltp_bulk_insert` | 206.56ms | 268.83ms | 1.301× | 13.07% | PASS |
| file_writes | `oltp_insert` | 37.63ms | 48.86ms | 1.298× | 27.51% | PASS |
| file_writes | `oltp_update_index` | 164.77ms | 178.48ms | 1.083× | 5.20% | PASS |
| file_writes | `oltp_update_non_index` | 130.08ms | 118.95ms | 0.914× | 10.57% | PASS |
| file_writes | `oltp_delete_insert` | 140.38ms | 135.46ms | 0.965× | 14.74% | PASS |
| file_writes | `oltp_write_only` | 98.63ms | 98.14ms | 0.995× | 10.38% | PASS |
| file_writes | `types_delete_insert` | 85.90ms | 83.42ms | 0.971× | 13.71% | PASS |
| file_writes | `oltp_read_write` | 129.88ms | 133.39ms | 1.027× | 9.83% | PASS |
| ac_reads | `oltp_point_select` | 36.38ms | 40.18ms | 1.104× | 1.43% | PASS |
| ac_reads | `oltp_range_select` | 10.85ms | 10.04ms | 0.926× | 2.85% | PASS |
| ac_reads | `oltp_sum_range` | 10.42ms | 10.01ms | 0.961× | 1.82% | PASS |
| ac_reads | `oltp_order_range` | 2.23ms | 2.15ms | 0.962× | 2.33% | PASS |
| ac_reads | `oltp_distinct_range` | 2.73ms | 2.59ms | 0.949× | 1.68% | PASS |
| ac_reads | `oltp_index_scan` | 5.04ms | 5.99ms | 1.187× | 2.05% | PASS |
| ac_reads | `select_random_points` | 13.88ms | 14.41ms | 1.038× | 2.47% | PASS |
| ac_reads | `select_random_ranges` | 4.76ms | 5.29ms | 1.110× | 2.03% | PASS |
| ac_reads | `covering_index_scan` | 5.05ms | 4.95ms | 0.981× | 3.82% | PASS |
| ac_reads | `groupby_scan` | 19.12ms | 18.78ms | 0.983× | 0.95% | PASS |
| ac_reads | `index_join` | 5.92ms | 7.28ms | 1.228× | 2.50% | PASS |
| ac_reads | `index_join_scan` | 3.42ms | 4.34ms | 1.267× | 1.82% | PASS |
| ac_reads | `types_table_scan` | 670.08ms | 719.61ms | 1.074× | 0.91% | PASS |
| ac_reads | `table_scan` | 774.57ms | 811.12ms | 1.047× | 1.25% | PASS |
| ac_reads | `oltp_read_only` | 91.99ms | 96.98ms | 1.054× | 1.87% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 37.96ms | 110.77ms | 2.918× | 43.39% | PASS |
| ac_writes | `oltp_insert_ac` | 48.96ms | 139.98ms | 2.859× | 44.88% | PASS |
| ac_writes | `oltp_update_index_ac` | 45.85ms | 127.37ms | 2.778× | 47.44% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 50.89ms | 152.16ms | 2.990× | 52.64% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 47.90ms | 143.20ms | 2.989× | 46.82% | PASS |
| ac_writes | `oltp_write_only_ac` | 55.51ms | 129.40ms | 2.331× | 51.01% | PASS |
| ac_writes | `types_delete_insert_ac` | 61.46ms | 175.79ms | 2.860× | 76.16% | PASS |
| ac_writes | `oltp_read_write_ac` | 45.73ms | 221.50ms | 4.844× | 67.48% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.17ms | 40.90ms | 1.197× | 1.50% | PASS |
| mem_reads | `oltp_range_select` | 19.75ms | 21.25ms | 1.076× | 1.90% | PASS |
| mem_reads | `oltp_sum_range` | 18.59ms | 20.97ms | 1.128× | 1.15% | PASS |
| mem_reads | `oltp_order_range` | 3.61ms | 3.88ms | 1.074× | 0.89% | PASS |
| mem_reads | `oltp_distinct_range` | 4.74ms | 4.93ms | 1.040× | 1.30% | PASS |
| mem_reads | `oltp_index_scan` | 4.75ms | 6.39ms | 1.345× | 2.46% | PASS |
| mem_reads | `select_random_points` | 29.02ms | 32.20ms | 1.109× | 1.79% | PASS |
| mem_reads | `select_random_ranges` | 7.97ms | 9.14ms | 1.147× | 1.36% | PASS |
| mem_reads | `covering_index_scan` | 4.26ms | 4.40ms | 1.032× | 2.26% | PASS |
| mem_reads | `groupby_scan` | 36.84ms | 39.31ms | 1.067× | 0.72% | PASS |
| mem_reads | `index_join` | 8.32ms | 10.64ms | 1.278× | 2.09% | PASS |
| mem_reads | `index_join_scan` | 4.35ms | 5.54ms | 1.274× | 2.03% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.25s | 1.165× | 1.30% | PASS |
| mem_reads | `table_scan` | 1.27s | 1.41s | 1.109× | 1.93% | PASS |
| mem_reads | `oltp_read_only` | 156.07ms | 170.30ms | 1.091× | 1.74% | PASS |
| mem_writes | `oltp_bulk_insert` | 248.67ms | 357.74ms | 1.439× | 1.09% | PASS |
| mem_writes | `oltp_insert` | 19.42ms | 36.68ms | 1.889× | 0.85% | PASS |
| mem_writes | `oltp_update_index` | 70.79ms | 120.85ms | 1.707× | 1.49% | PASS |
| mem_writes | `oltp_update_non_index` | 52.97ms | 85.36ms | 1.611× | 1.90% | PASS |
| mem_writes | `oltp_delete_insert` | 51.61ms | 98.15ms | 1.902× | 1.78% | PASS |
| mem_writes | `oltp_write_only` | 28.16ms | 58.51ms | 2.077× | 1.64% | PASS |
| mem_writes | `types_delete_insert` | 33.65ms | 55.36ms | 1.645× | 1.14% | PASS |
| mem_writes | `oltp_read_write` | 106.28ms | 157.61ms | 1.483× | 1.76% | PASS |
| file_reads | `oltp_point_select` | 109.73ms | 67.34ms | 0.614× | 1.36% | PASS |
| file_reads | `oltp_range_select` | 28.23ms | 24.51ms | 0.868× | 2.11% | PASS |
| file_reads | `oltp_sum_range` | 26.96ms | 24.33ms | 0.902× | 1.49% | PASS |
| file_reads | `oltp_order_range` | 4.63ms | 4.32ms | 0.934× | 1.86% | PASS |
| file_reads | `oltp_distinct_range` | 5.74ms | 5.39ms | 0.938× | 1.18% | PASS |
| file_reads | `oltp_index_scan` | 12.44ms | 9.23ms | 0.742× | 1.23% | PASS |
| file_reads | `select_random_points` | 38.73ms | 36.48ms | 0.942× | 1.46% | PASS |
| file_reads | `select_random_ranges` | 15.87ms | 12.12ms | 0.764× | 1.33% | PASS |
| file_reads | `covering_index_scan` | 11.96ms | 7.23ms | 0.605× | 1.12% | PASS |
| file_reads | `groupby_scan` | 37.81ms | 40.11ms | 1.061× | 1.26% | PASS |
| file_reads | `index_join` | 12.83ms | 12.92ms | 1.007× | 1.92% | PASS |
| file_reads | `index_join_scan` | 5.38ms | 5.98ms | 1.113× | 2.49% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.24s | 1.179× | 1.21% | PASS |
| file_reads | `table_scan` | 1.26s | 1.41s | 1.116× | 1.94% | PASS |
| file_reads | `oltp_read_only` | 268.70ms | 211.37ms | 0.787× | 0.91% | PASS |
| file_writes | `oltp_bulk_insert` | 264.41ms | 381.90ms | 1.444× | 0.89% | PASS |
| file_writes | `oltp_insert` | 26.44ms | 46.42ms | 1.756× | 1.78% | PASS |
| file_writes | `oltp_update_index` | 99.90ms | 145.71ms | 1.459× | 1.38% | PASS |
| file_writes | `oltp_update_non_index` | 78.51ms | 105.33ms | 1.342× | 1.33% | PASS |
| file_writes | `oltp_delete_insert` | 78.61ms | 121.30ms | 1.543× | 1.13% | PASS |
| file_writes | `oltp_write_only` | 52.09ms | 78.31ms | 1.503× | 1.92% | PASS |
| file_writes | `types_delete_insert` | 50.64ms | 69.51ms | 1.373× | 1.15% | PASS |
| file_writes | `oltp_read_write` | 133.57ms | 179.01ms | 1.340× | 1.71% | PASS |
| ac_reads | `oltp_point_select` | 59.62ms | 67.46ms | 1.132× | 1.45% | PASS |
| ac_reads | `oltp_range_select` | 23.26ms | 24.30ms | 1.045× | 1.66% | PASS |
| ac_reads | `oltp_sum_range` | 21.87ms | 24.31ms | 1.112× | 1.40% | PASS |
| ac_reads | `oltp_order_range` | 4.12ms | 4.29ms | 1.043× | 1.49% | PASS |
| ac_reads | `oltp_distinct_range` | 5.20ms | 5.36ms | 1.032× | 1.31% | PASS |
| ac_reads | `oltp_index_scan` | 7.66ms | 9.25ms | 1.207× | 1.62% | PASS |
| ac_reads | `select_random_points` | 32.52ms | 36.19ms | 1.113× | 2.05% | PASS |
| ac_reads | `select_random_ranges` | 10.65ms | 12.06ms | 1.132× | 1.32% | PASS |
| ac_reads | `covering_index_scan` | 7.25ms | 7.26ms | 1.001× | 1.48% | PASS |
| ac_reads | `groupby_scan` | 37.30ms | 40.14ms | 1.076× | 1.08% | PASS |
| ac_reads | `index_join` | 10.22ms | 12.91ms | 1.263× | 2.45% | PASS |
| ac_reads | `index_join_scan` | 4.85ms | 6.02ms | 1.240× | 1.87% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.25s | 1.176× | 1.32% | PASS |
| ac_reads | `table_scan` | 1.25s | 1.41s | 1.133× | 2.17% | PASS |
| ac_reads | `oltp_read_only` | 195.05ms | 211.43ms | 1.084× | 1.29% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.67ms | 68.25ms | 2.883× | 5.24% | PASS |
| ac_writes | `oltp_insert_ac` | 26.39ms | 89.25ms | 3.382× | 6.77% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.43ms | 101.30ms | 3.693× | 3.50% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.72ms | 79.13ms | 3.336× | 6.91% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.96ms | 91.00ms | 3.646× | 4.11% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.21ms | 92.07ms | 3.513× | 4.14% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.76ms | 80.30ms | 3.380× | 5.76% | PASS |
| ac_writes | `oltp_read_write_ac` | 34.26ms | 100.47ms | 2.933× | 6.20% | PASS |

</details>

## Version-control latency

Wall time: 1m 43s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 55.81ms | 200.00ms | 27.9% | 1.05% | PASS |
| `status_dirty_many_tables` | 57.74ms | 200.00ms | 28.9% | 0.85% | PASS |
| `diff_regular_working_one_table` | 53.34ms | 150.00ms | 35.6% | 0.83% | PASS |
| `diff_regular_working_many_tables` | 60.12ms | 200.00ms | 30.1% | 0.96% | PASS |
| `diff_stat_working_many_tables` | 59.27ms | 200.00ms | 29.6% | 1.65% | PASS |
| `diff_schema_working_many_tables` | 60.01ms | 200.00ms | 30.0% | 1.85% | PASS |
| `branch_list_many_branches` | 16.03ms | 100.00ms | 16.0% | 2.49% | PASS |
| `branch_create_delete` | 17.74ms | 100.00ms | 17.7% | 2.18% | PASS |
| `checkout_branch_clean` | 70.89ms | 200.00ms | 35.4% | 1.49% | PASS |
| `merge_data_no_conflicts` | 23.51ms | 150.00ms | 15.7% | 1.34% | PASS |
| `merge_schema_no_conflicts` | 15.35ms | 100.00ms | 15.4% | 0.77% | PASS |
| `merge_data_conflicts` | 75.76ms | 250.00ms | 30.3% | 0.65% | PASS |
| `merge_data_conflicts_with_resolve` | 75.33ms | 250.00ms | 30.1% | 0.55% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
