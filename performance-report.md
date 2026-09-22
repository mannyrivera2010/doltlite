# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-22 15:46 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260920.314.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/35738130734)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 54s | 8.93s | 11.56s | 1.296× | 1.54% | **PASS** |
| textpk | 69 | 55 | 1h 36m 14s | 12.23s | 12.28s | 1.004× | 1.68% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 54s | 9.88s | 13.71s | 1.388× | 2.76% | **PASS** |
| compositepk | 69 | 55 | 1h 21m 51s | 8.90s | 10.23s | 1.149× | 1.46% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.12ms | 29.40ms | 1.272× | 1.33% | PASS |
| mem_reads | `oltp_range_select` | 10.24ms | 13.14ms | 1.284× | 1.78% | PASS |
| mem_reads | `oltp_sum_range` | 9.60ms | 12.40ms | 1.292× | 1.66% | PASS |
| mem_reads | `oltp_order_range` | 2.53ms | 2.99ms | 1.180× | 1.69% | PASS |
| mem_reads | `oltp_distinct_range` | 3.56ms | 4.00ms | 1.123× | 1.63% | PASS |
| mem_reads | `oltp_index_scan` | 3.79ms | 5.18ms | 1.367× | 1.16% | PASS |
| mem_reads | `select_random_points` | 9.36ms | 10.98ms | 1.173× | 2.16% | PASS |
| mem_reads | `select_random_ranges` | 2.96ms | 3.99ms | 1.349× | 1.64% | PASS |
| mem_reads | `covering_index_scan` | 4.25ms | 4.39ms | 1.034× | 2.06% | PASS |
| mem_reads | `groupby_scan` | 30.20ms | 32.98ms | 1.092× | 0.64% | PASS |
| mem_reads | `index_join` | 6.01ms | 8.03ms | 1.335× | 1.25% | PASS |
| mem_reads | `index_join_scan` | 3.44ms | 4.54ms | 1.319× | 1.62% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.34s | 1.283× | 0.83% | PASS |
| mem_reads | `table_scan` | 1.22s | 1.42s | 1.159× | 1.25% | PASS |
| mem_reads | `oltp_read_only` | 102.06ms | 123.24ms | 1.208× | 1.63% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.09ms | 251.75ms | 1.390× | 1.17% | PASS |
| mem_writes | `oltp_insert` | 15.46ms | 28.75ms | 1.859× | 0.89% | PASS |
| mem_writes | `oltp_update_index` | 51.22ms | 108.64ms | 2.121× | 1.48% | PASS |
| mem_writes | `oltp_update_non_index` | 33.67ms | 59.48ms | 1.767× | 1.46% | PASS |
| mem_writes | `oltp_delete_insert` | 44.76ms | 78.87ms | 1.762× | 1.39% | PASS |
| mem_writes | `oltp_write_only` | 22.02ms | 45.47ms | 2.065× | 1.57% | PASS |
| mem_writes | `types_delete_insert` | 24.76ms | 40.92ms | 1.653× | 1.21% | PASS |
| mem_writes | `oltp_read_write` | 68.85ms | 112.40ms | 1.632× | 1.54% | PASS |
| file_reads | `oltp_point_select` | 98.74ms | 55.22ms | 0.559× | 0.74% | PASS |
| file_reads | `oltp_range_select` | 18.13ms | 15.96ms | 0.880× | 1.54% | PASS |
| file_reads | `oltp_sum_range` | 17.40ms | 15.24ms | 0.876× | 1.16% | PASS |
| file_reads | `oltp_order_range` | 3.45ms | 3.34ms | 0.970× | 1.77% | PASS |
| file_reads | `oltp_distinct_range` | 4.52ms | 4.40ms | 0.974× | 0.81% | PASS |
| file_reads | `oltp_index_scan` | 11.65ms | 8.26ms | 0.709× | 1.43% | PASS |
| file_reads | `select_random_points` | 18.09ms | 13.96ms | 0.772× | 1.53% | PASS |
| file_reads | `select_random_ranges` | 10.54ms | 6.63ms | 0.629× | 0.98% | PASS |
| file_reads | `covering_index_scan` | 12.11ms | 7.14ms | 0.589× | 1.15% | PASS |
| file_reads | `groupby_scan` | 31.02ms | 33.28ms | 1.073× | 1.03% | PASS |
| file_reads | `index_join` | 10.29ms | 10.19ms | 0.990× | 1.68% | PASS |
| file_reads | `index_join_scan` | 4.41ms | 4.93ms | 1.119× | 1.17% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.33s | 1.279× | 0.65% | PASS |
| file_reads | `table_scan` | 1.19s | 1.40s | 1.178× | 1.05% | PASS |
| file_reads | `oltp_read_only` | 212.83ms | 161.93ms | 0.761× | 1.05% | PASS |
| file_writes | `oltp_bulk_insert` | 194.76ms | 271.59ms | 1.394× | 1.64% | PASS |
| file_writes | `oltp_insert` | 22.04ms | 35.87ms | 1.628× | 1.55% | PASS |
| file_writes | `oltp_update_index` | 77.41ms | 128.63ms | 1.662× | 1.76% | PASS |
| file_writes | `oltp_update_non_index` | 57.89ms | 81.52ms | 1.408× | 1.92% | PASS |
| file_writes | `oltp_delete_insert` | 67.85ms | 99.71ms | 1.470× | 1.69% | PASS |
| file_writes | `oltp_write_only` | 44.23ms | 64.49ms | 1.458× | 2.28% | PASS |
| file_writes | `types_delete_insert` | 40.55ms | 54.30ms | 1.339× | 1.82% | PASS |
| file_writes | `oltp_read_write` | 98.45ms | 134.41ms | 1.365× | 2.00% | PASS |
| ac_reads | `oltp_point_select` | 49.78ms | 56.15ms | 1.128× | 1.19% | PASS |
| ac_reads | `oltp_range_select` | 13.79ms | 16.11ms | 1.169× | 1.60% | PASS |
| ac_reads | `oltp_sum_range` | 12.62ms | 15.20ms | 1.205× | 1.58% | PASS |
| ac_reads | `oltp_order_range` | 2.96ms | 3.32ms | 1.121× | 1.65% | PASS |
| ac_reads | `oltp_distinct_range` | 4.06ms | 4.43ms | 1.090× | 1.19% | PASS |
| ac_reads | `oltp_index_scan` | 6.76ms | 8.26ms | 1.222× | 0.98% | PASS |
| ac_reads | `select_random_points` | 13.02ms | 13.89ms | 1.067× | 1.56% | PASS |
| ac_reads | `select_random_ranges` | 5.60ms | 6.60ms | 1.178× | 1.02% | PASS |
| ac_reads | `covering_index_scan` | 7.07ms | 7.05ms | 0.997× | 1.62% | PASS |
| ac_reads | `groupby_scan` | 30.46ms | 33.33ms | 1.094× | 1.16% | PASS |
| ac_reads | `index_join` | 7.76ms | 10.12ms | 1.303× | 1.56% | PASS |
| ac_reads | `index_join_scan` | 3.92ms | 4.96ms | 1.265× | 1.09% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.33s | 1.278× | 0.45% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.41s | 1.189× | 0.69% | PASS |
| ac_reads | `oltp_read_only` | 138.77ms | 161.35ms | 1.163× | 0.92% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.45ms | 84.25ms | 3.593× | 5.63% | PASS |
| ac_writes | `oltp_insert_ac` | 26.14ms | 100.00ms | 3.825× | 5.74% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.68ms | 114.55ms | 4.139× | 6.91% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.23ms | 93.59ms | 4.029× | 7.13% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.30ms | 106.27ms | 4.200× | 6.11% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.91ms | 106.26ms | 4.101× | 7.63% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.63ms | 96.60ms | 4.088× | 8.27% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.50ms | 110.76ms | 3.631× | 6.30% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.94ms | 36.53ms | 1.144× | 1.84% | PASS |
| mem_reads | `oltp_range_select` | 16.85ms | 14.65ms | 0.870× | 1.70% | PASS |
| mem_reads | `oltp_sum_range` | 14.72ms | 14.34ms | 0.974× | 1.75% | PASS |
| mem_reads | `oltp_order_range` | 3.36ms | 3.27ms | 0.973× | 1.17% | PASS |
| mem_reads | `oltp_distinct_range` | 4.45ms | 4.27ms | 0.959× | 1.06% | PASS |
| mem_reads | `oltp_index_scan` | 4.98ms | 6.38ms | 1.283× | 1.94% | PASS |
| mem_reads | `select_random_points` | 19.50ms | 21.23ms | 1.089× | 0.89% | PASS |
| mem_reads | `select_random_ranges` | 4.39ms | 5.37ms | 1.224× | 1.17% | PASS |
| mem_reads | `covering_index_scan` | 5.71ms | 4.89ms | 0.856× | 2.36% | PASS |
| mem_reads | `groupby_scan` | 35.02ms | 35.93ms | 1.026× | 0.85% | PASS |
| mem_reads | `index_join` | 7.81ms | 10.19ms | 1.305× | 2.27% | PASS |
| mem_reads | `index_join_scan` | 5.10ms | 5.99ms | 1.174× | 1.83% | PASS |
| mem_reads | `types_table_scan` | 1.35s | 1.33s | 0.985× | 1.76% | PASS |
| mem_reads | `table_scan` | 1.70s | 1.43s | 0.842× | 1.11% | PASS |
| mem_reads | `oltp_read_only` | 127.96ms | 132.64ms | 1.037× | 1.91% | PASS |
| mem_writes | `oltp_bulk_insert` | 232.31ms | 336.75ms | 1.450× | 0.76% | PASS |
| mem_writes | `oltp_insert` | 23.15ms | 40.08ms | 1.731× | 1.95% | PASS |
| mem_writes | `oltp_update_index` | 82.45ms | 147.24ms | 1.786× | 1.66% | PASS |
| mem_writes | `oltp_update_non_index` | 53.61ms | 93.30ms | 1.740× | 1.20% | PASS |
| mem_writes | `oltp_delete_insert` | 57.26ms | 112.61ms | 1.967× | 1.91% | PASS |
| mem_writes | `oltp_write_only` | 33.01ms | 67.87ms | 2.056× | 1.84% | PASS |
| mem_writes | `types_delete_insert` | 35.43ms | 57.56ms | 1.625× | 1.50% | PASS |
| mem_writes | `oltp_read_write` | 99.72ms | 147.81ms | 1.482× | 2.06% | PASS |
| file_reads | `oltp_point_select` | 130.34ms | 69.36ms | 0.532× | 0.87% | PASS |
| file_reads | `oltp_range_select` | 26.08ms | 17.48ms | 0.670× | 1.98% | PASS |
| file_reads | `oltp_sum_range` | 23.63ms | 17.17ms | 0.726× | 2.10% | PASS |
| file_reads | `oltp_order_range` | 4.42ms | 3.62ms | 0.820× | 2.20% | PASS |
| file_reads | `oltp_distinct_range` | 5.42ms | 4.63ms | 0.854× | 1.20% | PASS |
| file_reads | `oltp_index_scan` | 14.87ms | 9.62ms | 0.647× | 1.03% | PASS |
| file_reads | `select_random_points` | 29.53ms | 24.15ms | 0.818× | 1.40% | PASS |
| file_reads | `select_random_ranges` | 14.17ms | 8.62ms | 0.608× | 0.86% | PASS |
| file_reads | `covering_index_scan` | 16.09ms | 8.11ms | 0.504× | 1.14% | PASS |
| file_reads | `groupby_scan` | 35.72ms | 36.06ms | 1.009× | 0.74% | PASS |
| file_reads | `index_join` | 13.60ms | 12.04ms | 0.885× | 2.43% | PASS |
| file_reads | `index_join_scan` | 6.21ms | 6.44ms | 1.036× | 1.64% | PASS |
| file_reads | `types_table_scan` | 1.38s | 1.31s | 0.944× | 1.77% | PASS |
| file_reads | `table_scan` | 1.75s | 1.45s | 0.830× | 0.62% | PASS |
| file_reads | `oltp_read_only` | 280.83ms | 186.15ms | 0.663× | 0.57% | PASS |
| file_writes | `oltp_bulk_insert` | 255.02ms | 374.70ms | 1.469× | 1.11% | PASS |
| file_writes | `oltp_insert` | 65.28ms | 54.66ms | 0.837× | 19.31% | PASS |
| file_writes | `oltp_update_index` | 126.99ms | 185.96ms | 1.464× | 1.40% | PASS |
| file_writes | `oltp_update_non_index` | 116.14ms | 122.52ms | 1.055× | 9.10% | PASS |
| file_writes | `oltp_delete_insert` | 98.37ms | 143.81ms | 1.462× | 1.75% | PASS |
| file_writes | `oltp_write_only` | 87.00ms | 91.00ms | 1.046× | 10.76% | PASS |
| file_writes | `types_delete_insert` | 58.09ms | 77.09ms | 1.327× | 1.71% | PASS |
| file_writes | `oltp_read_write` | 135.82ms | 164.41ms | 1.211× | 5.94% | PASS |
| ac_reads | `oltp_point_select` | 63.44ms | 68.32ms | 1.077× | 1.08% | PASS |
| ac_reads | `oltp_range_select` | 19.14ms | 17.39ms | 0.909× | 1.68% | PASS |
| ac_reads | `oltp_sum_range` | 17.05ms | 17.08ms | 1.002× | 1.11% | PASS |
| ac_reads | `oltp_order_range` | 3.82ms | 3.62ms | 0.947× | 0.90% | PASS |
| ac_reads | `oltp_distinct_range` | 4.86ms | 4.62ms | 0.952× | 0.61% | PASS |
| ac_reads | `oltp_index_scan` | 8.41ms | 9.57ms | 1.138× | 1.23% | PASS |
| ac_reads | `select_random_points` | 22.77ms | 24.12ms | 1.059× | 1.61% | PASS |
| ac_reads | `select_random_ranges` | 7.69ms | 8.59ms | 1.116× | 0.97% | PASS |
| ac_reads | `covering_index_scan` | 9.45ms | 8.10ms | 0.857× | 1.25% | PASS |
| ac_reads | `groupby_scan` | 34.75ms | 36.04ms | 1.037× | 0.56% | PASS |
| ac_reads | `index_join` | 10.16ms | 11.74ms | 1.155× | 1.16% | PASS |
| ac_reads | `index_join_scan` | 5.62ms | 6.41ms | 1.141× | 2.38% | PASS |
| ac_reads | `types_table_scan` | 1.34s | 1.31s | 0.976× | 2.10% | PASS |
| ac_reads | `table_scan` | 1.71s | 1.44s | 0.839× | 1.19% | PASS |
| ac_reads | `oltp_read_only` | 182.43ms | 184.31ms | 1.010× | 1.36% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.91ms | 67.37ms | 3.762× | 4.56% | PASS |
| ac_writes | `oltp_insert_ac` | 21.16ms | 85.83ms | 4.056× | 5.82% | PASS |
| ac_writes | `oltp_update_index_ac` | 22.47ms | 103.03ms | 4.586× | 5.91% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.38ms | 79.49ms | 4.573× | 5.46% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 20.14ms | 91.09ms | 4.523× | 5.68% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.43ms | 89.11ms | 4.587× | 4.18% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.93ms | 81.72ms | 4.827× | 5.13% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.69ms | 100.71ms | 3.637× | 4.33% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.00ms | 22.72ms | 1.136× | 1.75% | PASS |
| mem_reads | `oltp_range_select` | 10.41ms | 9.69ms | 0.930× | 2.71% | PASS |
| mem_reads | `oltp_sum_range` | 10.04ms | 9.25ms | 0.921× | 2.44% | PASS |
| mem_reads | `oltp_order_range` | 2.25ms | 2.22ms | 0.984× | 1.97% | PASS |
| mem_reads | `oltp_distinct_range` | 2.93ms | 2.83ms | 0.967× | 2.60% | PASS |
| mem_reads | `oltp_index_scan` | 3.14ms | 3.73ms | 1.190× | 2.25% | PASS |
| mem_reads | `select_random_points` | 13.73ms | 15.44ms | 1.125× | 3.39% | PASS |
| mem_reads | `select_random_ranges` | 2.85ms | 3.36ms | 1.176× | 2.49% | PASS |
| mem_reads | `covering_index_scan` | 2.71ms | 2.70ms | 0.996× | 2.56% | PASS |
| mem_reads | `groupby_scan` | 21.12ms | 22.40ms | 1.060× | 1.76% | PASS |
| mem_reads | `index_join` | 5.25ms | 6.88ms | 1.310× | 2.85% | PASS |
| mem_reads | `index_join_scan` | 3.44ms | 4.99ms | 1.448× | 3.60% | PASS |
| mem_reads | `types_table_scan` | 770.72ms | 855.81ms | 1.110× | 1.05% | PASS |
| mem_reads | `table_scan` | 886.81ms | 933.37ms | 1.053× | 3.59% | PASS |
| mem_reads | `oltp_read_only` | 78.00ms | 83.86ms | 1.075× | 1.39% | PASS |
| mem_writes | `oltp_bulk_insert` | 135.11ms | 194.52ms | 1.440× | 1.70% | PASS |
| mem_writes | `oltp_insert` | 12.73ms | 23.75ms | 1.866× | 1.66% | PASS |
| mem_writes | `oltp_update_index` | 46.72ms | 86.11ms | 1.843× | 1.85% | PASS |
| mem_writes | `oltp_update_non_index` | 30.38ms | 51.27ms | 1.688× | 2.55% | PASS |
| mem_writes | `oltp_delete_insert` | 32.69ms | 64.18ms | 1.963× | 1.99% | PASS |
| mem_writes | `oltp_write_only` | 18.72ms | 38.54ms | 2.059× | 1.66% | PASS |
| mem_writes | `types_delete_insert` | 20.19ms | 32.75ms | 1.622× | 2.76% | PASS |
| mem_writes | `oltp_read_write` | 53.69ms | 86.56ms | 1.612× | 1.82% | PASS |
| file_reads | `oltp_point_select` | 42.24ms | 30.32ms | 0.718× | 1.65% | PASS |
| file_reads | `oltp_range_select` | 13.16ms | 10.63ms | 0.808× | 2.60% | PASS |
| file_reads | `oltp_sum_range` | 12.49ms | 10.21ms | 0.817× | 3.17% | PASS |
| file_reads | `oltp_order_range` | 2.50ms | 2.36ms | 0.943× | 3.04% | PASS |
| file_reads | `oltp_distinct_range` | 3.19ms | 2.95ms | 0.923× | 2.51% | PASS |
| file_reads | `oltp_index_scan` | 5.85ms | 5.32ms | 0.909× | 2.35% | PASS |
| file_reads | `select_random_points` | 16.92ms | 17.27ms | 1.021× | 2.41% | PASS |
| file_reads | `select_random_ranges` | 5.18ms | 4.38ms | 0.844× | 2.42% | PASS |
| file_reads | `covering_index_scan` | 5.52ms | 4.05ms | 0.733× | 3.80% | PASS |
| file_reads | `groupby_scan` | 20.93ms | 22.22ms | 1.062× | 1.07% | PASS |
| file_reads | `index_join` | 6.36ms | 7.25ms | 1.140× | 4.09% | PASS |
| file_reads | `index_join_scan` | 3.32ms | 4.72ms | 1.422× | 6.14% | PASS |
| file_reads | `types_table_scan` | 754.96ms | 835.29ms | 1.106× | 0.59% | PASS |
| file_reads | `table_scan` | 882.97ms | 915.06ms | 1.036× | 2.51% | PASS |
| file_reads | `oltp_read_only` | 106.03ms | 92.83ms | 0.875× | 0.88% | PASS |
| file_writes | `oltp_bulk_insert` | 284.91ms | 315.46ms | 1.107× | 23.86% | PASS |
| file_writes | `oltp_insert` | 119.61ms | 66.25ms | 0.554× | 71.49% | PASS |
| file_writes | `oltp_update_index` | 230.26ms | 190.02ms | 0.825× | 26.49% | PASS |
| file_writes | `oltp_update_non_index` | 241.41ms | 146.52ms | 0.607× | 38.05% | PASS |
| file_writes | `oltp_delete_insert` | 333.19ms | 154.52ms | 0.464× | 42.15% | PASS |
| file_writes | `oltp_write_only` | 211.34ms | 116.50ms | 0.551× | 38.57% | PASS |
| file_writes | `types_delete_insert` | 145.28ms | 83.91ms | 0.578× | 38.90% | PASS |
| file_writes | `oltp_read_write` | 251.54ms | 162.36ms | 0.645× | 32.57% | PASS |
| ac_reads | `oltp_point_select` | 26.73ms | 29.85ms | 1.117× | 2.71% | PASS |
| ac_reads | `oltp_range_select` | 11.38ms | 10.45ms | 0.919× | 3.60% | PASS |
| ac_reads | `oltp_sum_range` | 10.69ms | 9.85ms | 0.922× | 4.20% | PASS |
| ac_reads | `oltp_order_range` | 2.42ms | 2.31ms | 0.957× | 3.92% | PASS |
| ac_reads | `oltp_distinct_range` | 3.04ms | 2.93ms | 0.964× | 2.92% | PASS |
| ac_reads | `oltp_index_scan` | 4.47ms | 5.32ms | 1.190× | 2.92% | PASS |
| ac_reads | `select_random_points` | 15.53ms | 16.95ms | 1.092× | 1.85% | PASS |
| ac_reads | `select_random_ranges` | 3.88ms | 4.37ms | 1.127× | 3.18% | PASS |
| ac_reads | `covering_index_scan` | 4.17ms | 4.03ms | 0.968× | 3.21% | PASS |
| ac_reads | `groupby_scan` | 20.83ms | 22.19ms | 1.065× | 0.90% | PASS |
| ac_reads | `index_join` | 6.47ms | 8.22ms | 1.271× | 3.36% | PASS |
| ac_reads | `index_join_scan` | 3.74ms | 5.11ms | 1.367× | 2.43% | PASS |
| ac_reads | `types_table_scan` | 755.51ms | 835.45ms | 1.106× | 0.91% | PASS |
| ac_reads | `table_scan` | 884.68ms | 914.63ms | 1.034× | 3.24% | PASS |
| ac_reads | `oltp_read_only` | 87.88ms | 94.57ms | 1.076× | 1.47% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 235.07ms | 694.95ms | 2.956× | 71.22% | PASS |
| ac_writes | `oltp_insert_ac` | 202.40ms | 662.90ms | 3.275× | 66.82% | PASS |
| ac_writes | `oltp_update_index_ac` | 229.76ms | 768.13ms | 3.343× | 62.37% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 322.51ms | 540.31ms | 1.675× | 55.21% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 334.34ms | 794.52ms | 2.376× | 49.07% | PASS |
| ac_writes | `oltp_write_only_ac` | 196.93ms | 561.38ms | 2.851× | 53.94% | PASS |
| ac_writes | `types_delete_insert_ac` | 277.59ms | 862.10ms | 3.106× | 69.54% | PASS |
| ac_writes | `oltp_read_write_ac` | 355.05ms | 1.10s | 3.099× | 59.88% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.63ms | 30.29ms | 1.138× | 2.12% | PASS |
| mem_reads | `oltp_range_select` | 16.03ms | 15.89ms | 0.991× | 1.70% | PASS |
| mem_reads | `oltp_sum_range` | 14.50ms | 15.17ms | 1.046× | 1.05% | PASS |
| mem_reads | `oltp_order_range` | 2.97ms | 2.96ms | 0.997× | 1.19% | PASS |
| mem_reads | `oltp_distinct_range` | 3.84ms | 3.80ms | 0.989× | 1.19% | PASS |
| mem_reads | `oltp_index_scan` | 3.75ms | 4.60ms | 1.226× | 2.22% | PASS |
| mem_reads | `select_random_points` | 21.85ms | 24.22ms | 1.108× | 1.17% | PASS |
| mem_reads | `select_random_ranges` | 6.06ms | 6.54ms | 1.079× | 1.09% | PASS |
| mem_reads | `covering_index_scan` | 3.40ms | 3.22ms | 0.946× | 1.63% | PASS |
| mem_reads | `groupby_scan` | 30.14ms | 30.94ms | 1.027× | 0.72% | PASS |
| mem_reads | `index_join` | 6.48ms | 7.93ms | 1.223× | 1.52% | PASS |
| mem_reads | `index_join_scan` | 3.53ms | 4.77ms | 1.351× | 1.79% | PASS |
| mem_reads | `types_table_scan` | 901.11ms | 988.84ms | 1.097× | 2.07% | PASS |
| mem_reads | `table_scan` | 1.08s | 1.08s | 0.999× | 3.63% | PASS |
| mem_reads | `oltp_read_only` | 120.42ms | 124.75ms | 1.036× | 1.07% | PASS |
| mem_writes | `oltp_bulk_insert` | 191.17ms | 258.41ms | 1.352× | 0.77% | PASS |
| mem_writes | `oltp_insert` | 15.23ms | 27.25ms | 1.789× | 0.84% | PASS |
| mem_writes | `oltp_update_index` | 54.63ms | 92.69ms | 1.697× | 1.46% | PASS |
| mem_writes | `oltp_update_non_index` | 41.49ms | 64.88ms | 1.564× | 0.91% | PASS |
| mem_writes | `oltp_delete_insert` | 42.36ms | 78.82ms | 1.861× | 1.40% | PASS |
| mem_writes | `oltp_write_only` | 24.53ms | 49.31ms | 2.010× | 1.71% | PASS |
| mem_writes | `types_delete_insert` | 28.23ms | 43.89ms | 1.555× | 1.21% | PASS |
| mem_writes | `oltp_read_write` | 91.59ms | 125.72ms | 1.373× | 1.61% | PASS |
| file_reads | `oltp_point_select` | 104.96ms | 57.69ms | 0.550× | 1.02% | PASS |
| file_reads | `oltp_range_select` | 24.96ms | 18.79ms | 0.753× | 1.91% | PASS |
| file_reads | `oltp_sum_range` | 22.50ms | 17.88ms | 0.795× | 1.06% | PASS |
| file_reads | `oltp_order_range` | 3.92ms | 3.31ms | 0.844× | 1.08% | PASS |
| file_reads | `oltp_distinct_range` | 4.75ms | 4.11ms | 0.866× | 1.02% | PASS |
| file_reads | `oltp_index_scan` | 11.79ms | 7.71ms | 0.654× | 0.80% | PASS |
| file_reads | `select_random_points` | 31.60ms | 27.79ms | 0.879× | 2.23% | PASS |
| file_reads | `select_random_ranges` | 14.04ms | 9.42ms | 0.671× | 0.91% | PASS |
| file_reads | `covering_index_scan` | 11.36ms | 6.29ms | 0.554× | 0.75% | PASS |
| file_reads | `groupby_scan` | 31.47ms | 31.60ms | 1.004× | 0.72% | PASS |
| file_reads | `index_join` | 10.89ms | 10.36ms | 0.951× | 1.27% | PASS |
| file_reads | `index_join_scan` | 4.42ms | 5.14ms | 1.161× | 1.47% | PASS |
| file_reads | `types_table_scan` | 888.70ms | 981.06ms | 1.104× | 1.69% | PASS |
| file_reads | `table_scan` | 1.06s | 1.07s | 1.008× | 4.00% | PASS |
| file_reads | `oltp_read_only` | 233.47ms | 163.28ms | 0.699× | 0.96% | PASS |
| file_writes | `oltp_bulk_insert` | 259.69ms | 337.19ms | 1.298× | 11.23% | PASS |
| file_writes | `oltp_insert` | 28.66ms | 51.03ms | 1.780× | 28.54% | PASS |
| file_writes | `oltp_update_index` | 176.39ms | 164.83ms | 0.934× | 11.79% | PASS |
| file_writes | `oltp_update_non_index` | 150.02ms | 131.44ms | 0.876× | 26.68% | PASS |
| file_writes | `oltp_delete_insert` | 140.08ms | 136.85ms | 0.977× | 9.21% | PASS |
| file_writes | `oltp_write_only` | 99.85ms | 102.13ms | 1.023× | 14.15% | PASS |
| file_writes | `types_delete_insert` | 83.33ms | 88.99ms | 1.068× | 17.68% | PASS |
| file_writes | `oltp_read_write` | 164.75ms | 179.57ms | 1.090× | 18.65% | PASS |
| ac_reads | `oltp_point_select` | 51.64ms | 55.33ms | 1.072× | 1.37% | PASS |
| ac_reads | `oltp_range_select` | 19.06ms | 18.46ms | 0.968× | 1.97% | PASS |
| ac_reads | `oltp_sum_range` | 17.69ms | 17.90ms | 1.012× | 1.10% | PASS |
| ac_reads | `oltp_order_range` | 3.49ms | 3.29ms | 0.944× | 1.25% | PASS |
| ac_reads | `oltp_distinct_range` | 4.29ms | 4.11ms | 0.957× | 1.12% | PASS |
| ac_reads | `oltp_index_scan` | 6.78ms | 7.66ms | 1.130× | 1.18% | PASS |
| ac_reads | `select_random_points` | 25.28ms | 27.07ms | 1.071× | 0.77% | PASS |
| ac_reads | `select_random_ranges` | 8.91ms | 9.34ms | 1.048× | 1.14% | PASS |
| ac_reads | `covering_index_scan` | 6.34ms | 6.22ms | 0.981× | 1.21% | PASS |
| ac_reads | `groupby_scan` | 30.82ms | 31.58ms | 1.025× | 0.71% | PASS |
| ac_reads | `index_join` | 8.37ms | 10.33ms | 1.235× | 1.21% | PASS |
| ac_reads | `index_join_scan` | 3.94ms | 5.10ms | 1.294× | 1.52% | PASS |
| ac_reads | `types_table_scan` | 870.99ms | 967.63ms | 1.111× | 0.70% | PASS |
| ac_reads | `table_scan` | 1.05s | 1.07s | 1.023× | 3.17% | PASS |
| ac_reads | `oltp_read_only` | 166.79ms | 166.68ms | 0.999× | 0.77% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.05ms | 82.14ms | 3.564× | 47.12% | PASS |
| ac_writes | `oltp_insert_ac` | 28.83ms | 108.31ms | 3.757× | 33.09% | PASS |
| ac_writes | `oltp_update_index_ac` | 95.00ms | 221.96ms | 2.336× | 66.18% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 43.12ms | 125.90ms | 2.920× | 65.75% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 29.61ms | 100.49ms | 3.394× | 38.41% | PASS |
| ac_writes | `oltp_write_only_ac` | 28.26ms | 142.59ms | 5.045× | 48.11% | PASS |
| ac_writes | `types_delete_insert_ac` | 41.74ms | 181.23ms | 4.342× | 71.16% | PASS |
| ac_writes | `oltp_read_write_ac` | 43.40ms | 170.88ms | 3.938× | 59.48% | PASS |

</details>

## Version-control latency

Wall time: 2m 22s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 82.46ms | 200.00ms | 41.2% | 0.61% | PASS |
| `status_dirty_many_tables` | 86.23ms | 200.00ms | 43.1% | 1.08% | PASS |
| `diff_regular_working_one_table` | 78.93ms | 150.00ms | 52.6% | 0.83% | PASS |
| `diff_regular_working_many_tables` | 92.40ms | 200.00ms | 46.2% | 0.68% | PASS |
| `diff_stat_working_many_tables` | 92.09ms | 200.00ms | 46.0% | 0.44% | PASS |
| `diff_schema_working_many_tables` | 92.84ms | 200.00ms | 46.4% | 0.59% | PASS |
| `branch_list_many_branches` | 23.20ms | 100.00ms | 23.2% | 1.54% | PASS |
| `branch_create_delete` | 26.17ms | 100.00ms | 26.2% | 3.68% | PASS |
| `checkout_branch_clean` | 57.47ms | 200.00ms | 28.7% | 0.88% | PASS |
| `merge_data_no_conflicts` | 30.86ms | 150.00ms | 20.6% | 0.91% | PASS |
| `merge_schema_no_conflicts` | 24.05ms | 100.00ms | 24.0% | 1.05% | PASS |
| `merge_data_conflicts` | 129.88ms | 250.00ms | 52.0% | 0.39% | PASS |
| `merge_data_conflicts_with_resolve` | 129.40ms | 250.00ms | 51.8% | 0.53% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
