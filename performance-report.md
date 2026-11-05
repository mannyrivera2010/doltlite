# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-05 18:57 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261026.354.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/42075950798)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 54m 51s | 6.31s | 7.26s | 1.151× | 2.58% | **PASS** |
| textpk | 69 | 55 | 1h 19m 29s | 8.60s | 10.12s | 1.177× | 1.47% | **PASS** |
| blobpk | 69 | 55 | 1h 19m 40s | 9.03s | 10.38s | 1.150× | 1.24% | **PASS** |
| compositepk | 69 | 55 | 1h 4m 28s | 7.12s | 7.94s | 1.116× | 2.71% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 16.76ms | 18.73ms | 1.117× | 2.65% | PASS |
| mem_reads | `oltp_range_select` | 6.51ms | 8.02ms | 1.232× | 2.58% | PASS |
| mem_reads | `oltp_sum_range` | 6.26ms | 7.49ms | 1.197× | 2.81% | PASS |
| mem_reads | `oltp_order_range` | 1.74ms | 1.78ms | 1.021× | 1.74% | PASS |
| mem_reads | `oltp_distinct_range` | 2.21ms | 2.29ms | 1.032× | 1.21% | PASS |
| mem_reads | `oltp_index_scan` | 2.62ms | 3.41ms | 1.303× | 2.61% | PASS |
| mem_reads | `select_random_points` | 8.64ms | 9.20ms | 1.065× | 2.99% | PASS |
| mem_reads | `select_random_ranges` | 2.09ms | 2.63ms | 1.257× | 1.80% | PASS |
| mem_reads | `covering_index_scan` | 2.40ms | 2.56ms | 1.068× | 3.23% | PASS |
| mem_reads | `groupby_scan` | 17.64ms | 18.53ms | 1.051× | 1.34% | PASS |
| mem_reads | `index_join` | 3.67ms | 4.96ms | 1.351× | 2.09% | PASS |
| mem_reads | `index_join_scan` | 2.48ms | 3.72ms | 1.498× | 3.69% | PASS |
| mem_reads | `types_table_scan` | 695.13ms | 762.15ms | 1.096× | 1.65% | PASS |
| mem_reads | `table_scan` | 798.53ms | 855.12ms | 1.071× | 2.50% | PASS |
| mem_reads | `oltp_read_only` | 57.95ms | 65.11ms | 1.124× | 1.94% | PASS |
| mem_writes | `oltp_bulk_insert` | 111.45ms | 156.28ms | 1.402× | 1.27% | PASS |
| mem_writes | `oltp_insert` | 9.40ms | 18.36ms | 1.953× | 1.06% | PASS |
| mem_writes | `oltp_update_index` | 32.04ms | 74.68ms | 2.331× | 2.14% | PASS |
| mem_writes | `oltp_update_non_index` | 24.64ms | 46.34ms | 1.880× | 2.71% | PASS |
| mem_writes | `oltp_delete_insert` | 28.89ms | 56.59ms | 1.959× | 1.74% | PASS |
| mem_writes | `oltp_write_only` | 14.40ms | 33.36ms | 2.317× | 2.70% | PASS |
| mem_writes | `types_delete_insert` | 16.57ms | 29.45ms | 1.777× | 1.71% | PASS |
| mem_writes | `oltp_read_write` | 41.02ms | 71.22ms | 1.736× | 3.00% | PASS |
| file_reads | `oltp_point_select` | 71.35ms | 38.04ms | 0.533× | 1.22% | PASS |
| file_reads | `oltp_range_select` | 12.79ms | 10.36ms | 0.810× | 2.57% | PASS |
| file_reads | `oltp_sum_range` | 12.44ms | 9.79ms | 0.787× | 2.01% | PASS |
| file_reads | `oltp_order_range` | 2.40ms | 2.02ms | 0.841× | 1.59% | PASS |
| file_reads | `oltp_distinct_range` | 2.89ms | 2.51ms | 0.869× | 1.49% | PASS |
| file_reads | `oltp_index_scan` | 8.44ms | 5.81ms | 0.689× | 1.72% | PASS |
| file_reads | `select_random_points` | 14.50ms | 11.32ms | 0.781× | 3.42% | PASS |
| file_reads | `select_random_ranges` | 7.51ms | 4.57ms | 0.609× | 1.81% | PASS |
| file_reads | `covering_index_scan` | 8.49ms | 5.04ms | 0.593× | 1.78% | PASS |
| file_reads | `groupby_scan` | 18.27ms | 18.85ms | 1.032× | 1.78% | PASS |
| file_reads | `index_join` | 7.27ms | 7.60ms | 1.046× | 4.80% | PASS |
| file_reads | `index_join_scan` | 3.18ms | 4.08ms | 1.281× | 5.41% | PASS |
| file_reads | `types_table_scan` | 694.62ms | 759.41ms | 1.093× | 1.89% | PASS |
| file_reads | `table_scan` | 811.13ms | 855.17ms | 1.054× | 2.59% | PASS |
| file_reads | `oltp_read_only` | 138.10ms | 92.01ms | 0.666× | 1.99% | PASS |
| file_writes | `oltp_bulk_insert` | 150.54ms | 207.80ms | 1.380× | 3.35% | PASS |
| file_writes | `oltp_insert` | 19.83ms | 28.15ms | 1.420× | 7.19% | PASS |
| file_writes | `oltp_update_index` | 121.54ms | 135.05ms | 1.111× | 4.17% | PASS |
| file_writes | `oltp_update_non_index` | 101.16ms | 88.81ms | 0.878× | 2.45% | PASS |
| file_writes | `oltp_delete_insert` | 106.77ms | 117.78ms | 1.103× | 3.95% | PASS |
| file_writes | `oltp_write_only` | 72.21ms | 78.71ms | 1.090× | 2.59% | PASS |
| file_writes | `types_delete_insert` | 63.15ms | 59.74ms | 0.946× | 4.86% | PASS |
| file_writes | `oltp_read_write` | 104.07ms | 118.51ms | 1.139× | 5.42% | PASS |
| ac_reads | `oltp_point_select` | 34.09ms | 36.92ms | 1.083× | 2.70% | PASS |
| ac_reads | `oltp_range_select` | 8.85ms | 9.27ms | 1.047× | 2.13% | PASS |
| ac_reads | `oltp_sum_range` | 8.43ms | 9.41ms | 1.116× | 1.64% | PASS |
| ac_reads | `oltp_order_range` | 2.04ms | 2.01ms | 0.986× | 1.32% | PASS |
| ac_reads | `oltp_distinct_range` | 2.55ms | 2.52ms | 0.989× | 1.63% | PASS |
| ac_reads | `oltp_index_scan` | 4.95ms | 5.74ms | 1.161× | 2.82% | PASS |
| ac_reads | `select_random_points` | 10.67ms | 11.01ms | 1.031× | 2.99% | PASS |
| ac_reads | `select_random_ranges` | 4.05ms | 4.58ms | 1.130× | 1.05% | PASS |
| ac_reads | `covering_index_scan` | 4.98ms | 5.09ms | 1.022× | 2.53% | PASS |
| ac_reads | `groupby_scan` | 17.99ms | 18.99ms | 1.056× | 1.69% | PASS |
| ac_reads | `index_join` | 5.17ms | 6.92ms | 1.340× | 3.96% | PASS |
| ac_reads | `index_join_scan` | 2.74ms | 4.02ms | 1.471× | 5.04% | PASS |
| ac_reads | `types_table_scan` | 679.73ms | 747.07ms | 1.099× | 1.32% | PASS |
| ac_reads | `table_scan` | 804.15ms | 857.81ms | 1.067× | 1.72% | PASS |
| ac_reads | `oltp_read_only` | 87.09ms | 92.85ms | 1.066× | 2.87% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.02ms | 53.03ms | 2.649× | 3.53% | PASS |
| ac_writes | `oltp_insert_ac` | 22.04ms | 66.42ms | 3.013× | 3.91% | PASS |
| ac_writes | `oltp_update_index_ac` | 23.06ms | 77.08ms | 3.343× | 3.50% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.19ms | 60.36ms | 2.990× | 5.20% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.07ms | 70.18ms | 3.180× | 3.08% | PASS |
| ac_writes | `oltp_write_only_ac` | 21.99ms | 68.87ms | 3.132× | 3.77% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.38ms | 66.99ms | 3.286× | 5.63% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.48ms | 72.64ms | 2.967× | 3.21% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.77ms | 31.67ms | 1.229× | 1.14% | PASS |
| mem_reads | `oltp_range_select` | 11.43ms | 12.31ms | 1.077× | 1.02% | PASS |
| mem_reads | `oltp_sum_range` | 10.97ms | 12.18ms | 1.110× | 1.08% | PASS |
| mem_reads | `oltp_order_range` | 2.64ms | 2.85ms | 1.082× | 1.19% | PASS |
| mem_reads | `oltp_distinct_range` | 3.38ms | 3.69ms | 1.090× | 0.76% | PASS |
| mem_reads | `oltp_index_scan` | 3.94ms | 5.39ms | 1.366× | 1.40% | PASS |
| mem_reads | `select_random_points` | 15.81ms | 20.02ms | 1.266× | 1.43% | PASS |
| mem_reads | `select_random_ranges` | 3.40ms | 4.53ms | 1.332× | 1.16% | PASS |
| mem_reads | `covering_index_scan` | 4.04ms | 3.97ms | 0.984× | 1.83% | PASS |
| mem_reads | `groupby_scan` | 29.32ms | 30.47ms | 1.039× | 0.43% | PASS |
| mem_reads | `index_join` | 6.11ms | 8.36ms | 1.368× | 1.71% | PASS |
| mem_reads | `index_join_scan` | 3.84ms | 5.01ms | 1.303× | 1.12% | PASS |
| mem_reads | `types_table_scan` | 970.27ms | 1.10s | 1.133× | 0.74% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.20s | 1.021× | 1.73% | PASS |
| mem_reads | `oltp_read_only` | 104.45ms | 120.13ms | 1.150× | 1.60% | PASS |
| mem_writes | `oltp_bulk_insert` | 200.99ms | 301.19ms | 1.498× | 0.60% | PASS |
| mem_writes | `oltp_insert` | 18.97ms | 34.86ms | 1.837× | 0.63% | PASS |
| mem_writes | `oltp_update_index` | 62.23ms | 121.32ms | 1.950× | 1.70% | PASS |
| mem_writes | `oltp_update_non_index` | 42.15ms | 76.60ms | 1.818× | 1.06% | PASS |
| mem_writes | `oltp_delete_insert` | 43.80ms | 92.82ms | 2.119× | 1.05% | PASS |
| mem_writes | `oltp_write_only` | 24.35ms | 54.68ms | 2.246× | 1.41% | PASS |
| mem_writes | `types_delete_insert` | 27.69ms | 49.41ms | 1.784× | 1.46% | PASS |
| mem_writes | `oltp_read_write` | 72.95ms | 123.44ms | 1.692× | 1.81% | PASS |
| file_reads | `oltp_point_select` | 54.73ms | 41.90ms | 0.766× | 1.51% | PASS |
| file_reads | `oltp_range_select` | 14.42ms | 13.58ms | 0.942× | 1.34% | PASS |
| file_reads | `oltp_sum_range` | 14.01ms | 13.51ms | 0.964× | 1.77% | PASS |
| file_reads | `oltp_order_range` | 3.04ms | 3.03ms | 0.996× | 1.52% | PASS |
| file_reads | `oltp_distinct_range` | 3.85ms | 3.89ms | 1.010× | 1.87% | PASS |
| file_reads | `oltp_index_scan` | 7.35ms | 6.77ms | 0.921× | 1.65% | PASS |
| file_reads | `select_random_points` | 20.48ms | 22.42ms | 1.095× | 1.47% | PASS |
| file_reads | `select_random_ranges` | 6.53ms | 5.64ms | 0.864× | 1.17% | PASS |
| file_reads | `covering_index_scan` | 7.73ms | 5.22ms | 0.676× | 2.45% | PASS |
| file_reads | `groupby_scan` | 30.14ms | 30.88ms | 1.024× | 0.75% | PASS |
| file_reads | `index_join` | 8.49ms | 9.82ms | 1.157× | 1.85% | PASS |
| file_reads | `index_join_scan` | 4.41ms | 5.40ms | 1.225× | 0.86% | PASS |
| file_reads | `types_table_scan` | 973.69ms | 1.10s | 1.133× | 0.70% | PASS |
| file_reads | `table_scan` | 1.17s | 1.21s | 1.031× | 1.15% | PASS |
| file_reads | `oltp_read_only` | 147.55ms | 135.69ms | 0.920× | 1.54% | PASS |
| file_writes | `oltp_bulk_insert` | 216.68ms | 323.89ms | 1.495× | 0.95% | PASS |
| file_writes | `oltp_insert` | 44.65ms | 44.48ms | 0.996× | 15.94% | PASS |
| file_writes | `oltp_update_index` | 86.56ms | 146.26ms | 1.690× | 1.94% | PASS |
| file_writes | `oltp_update_non_index` | 88.92ms | 94.26ms | 1.060× | 8.07% | PASS |
| file_writes | `oltp_delete_insert` | 65.73ms | 113.47ms | 1.726× | 1.42% | PASS |
| file_writes | `oltp_write_only` | 68.02ms | 72.32ms | 1.063× | 19.47% | PASS |
| file_writes | `types_delete_insert` | 41.10ms | 61.90ms | 1.506× | 1.04% | PASS |
| file_writes | `oltp_read_write` | 110.19ms | 140.28ms | 1.273× | 13.77% | PASS |
| ac_reads | `oltp_point_select` | 35.75ms | 41.74ms | 1.168× | 1.17% | PASS |
| ac_reads | `oltp_range_select` | 12.65ms | 13.55ms | 1.071× | 1.59% | PASS |
| ac_reads | `oltp_sum_range` | 12.25ms | 13.47ms | 1.100× | 1.53% | PASS |
| ac_reads | `oltp_order_range` | 2.82ms | 3.02ms | 1.069× | 1.79% | PASS |
| ac_reads | `oltp_distinct_range` | 3.59ms | 3.86ms | 1.077× | 1.22% | PASS |
| ac_reads | `oltp_index_scan` | 5.26ms | 6.65ms | 1.264× | 2.06% | PASS |
| ac_reads | `select_random_points` | 17.26ms | 21.48ms | 1.244× | 1.20% | PASS |
| ac_reads | `select_random_ranges` | 4.54ms | 5.60ms | 1.234× | 1.32% | PASS |
| ac_reads | `covering_index_scan` | 5.64ms | 5.17ms | 0.917× | 2.21% | PASS |
| ac_reads | `groupby_scan` | 29.72ms | 30.71ms | 1.033× | 0.83% | PASS |
| ac_reads | `index_join` | 7.37ms | 9.54ms | 1.295× | 2.02% | PASS |
| ac_reads | `index_join_scan` | 4.25ms | 5.32ms | 1.252× | 1.25% | PASS |
| ac_reads | `types_table_scan` | 977.99ms | 1.10s | 1.128× | 0.86% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.21s | 1.033× | 1.51% | PASS |
| ac_reads | `oltp_read_only` | 119.77ms | 135.66ms | 1.133× | 1.09% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.40ms | 46.77ms | 3.037× | 8.40% | PASS |
| ac_writes | `oltp_insert_ac` | 17.48ms | 59.46ms | 3.401× | 3.78% | PASS |
| ac_writes | `oltp_update_index_ac` | 18.84ms | 74.04ms | 3.931× | 3.53% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 15.43ms | 55.94ms | 3.625× | 5.11% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.70ms | 66.78ms | 3.772× | 6.17% | PASS |
| ac_writes | `oltp_write_only_ac` | 17.54ms | 65.24ms | 3.720× | 5.26% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.24ms | 57.97ms | 3.804× | 6.08% | PASS |
| ac_writes | `oltp_read_write_ac` | 21.74ms | 71.03ms | 3.268× | 5.33% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.96ms | 31.98ms | 1.186× | 1.54% | PASS |
| mem_reads | `oltp_range_select` | 11.45ms | 12.23ms | 1.068× | 0.89% | PASS |
| mem_reads | `oltp_sum_range` | 11.04ms | 12.16ms | 1.101× | 1.27% | PASS |
| mem_reads | `oltp_order_range` | 2.61ms | 2.85ms | 1.092× | 1.05% | PASS |
| mem_reads | `oltp_distinct_range` | 3.38ms | 3.68ms | 1.089× | 0.86% | PASS |
| mem_reads | `oltp_index_scan` | 3.93ms | 5.48ms | 1.394× | 1.12% | PASS |
| mem_reads | `select_random_points` | 16.23ms | 19.94ms | 1.229× | 1.26% | PASS |
| mem_reads | `select_random_ranges` | 3.40ms | 4.53ms | 1.333× | 0.96% | PASS |
| mem_reads | `covering_index_scan` | 3.87ms | 4.13ms | 1.068× | 1.24% | PASS |
| mem_reads | `groupby_scan` | 29.27ms | 30.36ms | 1.037× | 0.57% | PASS |
| mem_reads | `index_join` | 5.99ms | 8.69ms | 1.452× | 1.69% | PASS |
| mem_reads | `index_join_scan` | 3.52ms | 5.10ms | 1.449× | 1.32% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.13s | 1.090× | 1.03% | PASS |
| mem_reads | `table_scan` | 1.26s | 1.24s | 0.984× | 0.74% | PASS |
| mem_reads | `oltp_read_only` | 107.18ms | 121.29ms | 1.132× | 0.88% | PASS |
| mem_writes | `oltp_bulk_insert` | 205.60ms | 303.80ms | 1.478× | 1.06% | PASS |
| mem_writes | `oltp_insert` | 17.75ms | 35.41ms | 1.995× | 0.78% | PASS |
| mem_writes | `oltp_update_index` | 62.34ms | 125.69ms | 2.016× | 1.25% | PASS |
| mem_writes | `oltp_update_non_index` | 42.76ms | 76.16ms | 1.781× | 1.02% | PASS |
| mem_writes | `oltp_delete_insert` | 43.25ms | 95.56ms | 2.209× | 0.68% | PASS |
| mem_writes | `oltp_write_only` | 23.94ms | 56.12ms | 2.344× | 1.32% | PASS |
| mem_writes | `types_delete_insert` | 27.92ms | 50.08ms | 1.794× | 0.82% | PASS |
| mem_writes | `oltp_read_write` | 76.62ms | 128.80ms | 1.681× | 1.02% | PASS |
| file_reads | `oltp_point_select` | 56.50ms | 42.43ms | 0.751× | 1.05% | PASS |
| file_reads | `oltp_range_select` | 14.79ms | 13.55ms | 0.916× | 1.59% | PASS |
| file_reads | `oltp_sum_range` | 14.46ms | 13.44ms | 0.930× | 1.33% | PASS |
| file_reads | `oltp_order_range` | 3.00ms | 3.00ms | 0.999× | 1.07% | PASS |
| file_reads | `oltp_distinct_range` | 3.79ms | 3.83ms | 1.009× | 0.75% | PASS |
| file_reads | `oltp_index_scan` | 7.15ms | 6.64ms | 0.929× | 1.44% | PASS |
| file_reads | `select_random_points` | 19.95ms | 21.64ms | 1.085× | 1.07% | PASS |
| file_reads | `select_random_ranges` | 6.48ms | 5.61ms | 0.866× | 0.95% | PASS |
| file_reads | `covering_index_scan` | 7.25ms | 5.21ms | 0.719× | 1.61% | PASS |
| file_reads | `groupby_scan` | 29.94ms | 30.77ms | 1.027× | 0.53% | PASS |
| file_reads | `index_join` | 8.12ms | 9.76ms | 1.202× | 1.40% | PASS |
| file_reads | `index_join_scan` | 4.03ms | 5.34ms | 1.323× | 1.37% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.14s | 1.080× | 0.62% | PASS |
| file_reads | `table_scan` | 1.29s | 1.25s | 0.971× | 0.71% | PASS |
| file_reads | `oltp_read_only` | 154.21ms | 139.71ms | 0.906× | 0.69% | PASS |
| file_writes | `oltp_bulk_insert` | 219.80ms | 320.97ms | 1.460× | 1.52% | PASS |
| file_writes | `oltp_insert` | 24.52ms | 43.99ms | 1.794× | 1.48% | PASS |
| file_writes | `oltp_update_index` | 82.58ms | 145.95ms | 1.767× | 1.24% | PASS |
| file_writes | `oltp_update_non_index` | 59.88ms | 93.03ms | 1.553× | 1.06% | PASS |
| file_writes | `oltp_delete_insert` | 64.63ms | 116.30ms | 1.799× | 1.32% | PASS |
| file_writes | `oltp_write_only` | 40.33ms | 73.31ms | 1.818× | 1.78% | PASS |
| file_writes | `types_delete_insert` | 40.08ms | 61.47ms | 1.534× | 1.27% | PASS |
| file_writes | `oltp_read_write` | 91.76ms | 142.09ms | 1.548× | 1.31% | PASS |
| ac_reads | `oltp_point_select` | 36.44ms | 41.99ms | 1.152× | 1.23% | PASS |
| ac_reads | `oltp_range_select` | 12.60ms | 13.52ms | 1.073× | 1.38% | PASS |
| ac_reads | `oltp_sum_range` | 12.33ms | 13.35ms | 1.083× | 1.55% | PASS |
| ac_reads | `oltp_order_range` | 2.77ms | 2.98ms | 1.078× | 1.34% | PASS |
| ac_reads | `oltp_distinct_range` | 3.55ms | 3.82ms | 1.077× | 1.04% | PASS |
| ac_reads | `oltp_index_scan` | 5.31ms | 6.61ms | 1.244× | 1.28% | PASS |
| ac_reads | `select_random_points` | 17.42ms | 21.22ms | 1.218× | 1.55% | PASS |
| ac_reads | `select_random_ranges` | 4.52ms | 5.58ms | 1.233× | 1.11% | PASS |
| ac_reads | `covering_index_scan` | 5.31ms | 5.18ms | 0.976× | 1.22% | PASS |
| ac_reads | `groupby_scan` | 29.57ms | 30.62ms | 1.036× | 0.60% | PASS |
| ac_reads | `index_join` | 7.15ms | 9.66ms | 1.351× | 1.48% | PASS |
| ac_reads | `index_join_scan` | 3.86ms | 5.30ms | 1.373× | 1.46% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.13s | 1.104× | 1.17% | PASS |
| ac_reads | `table_scan` | 1.27s | 1.24s | 0.977× | 0.55% | PASS |
| ac_reads | `oltp_read_only` | 124.77ms | 138.79ms | 1.112× | 0.83% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.27ms | 46.17ms | 3.023× | 5.17% | PASS |
| ac_writes | `oltp_insert_ac` | 18.15ms | 64.08ms | 3.531× | 3.90% | PASS |
| ac_writes | `oltp_update_index_ac` | 17.90ms | 75.07ms | 4.193× | 6.24% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 15.24ms | 56.59ms | 3.713× | 5.00% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 16.13ms | 66.52ms | 4.125× | 6.63% | PASS |
| ac_writes | `oltp_write_only_ac` | 16.74ms | 65.86ms | 3.935× | 6.65% | PASS |
| ac_writes | `types_delete_insert_ac` | 14.81ms | 58.74ms | 3.967× | 7.47% | PASS |
| ac_writes | `oltp_read_write_ac` | 21.18ms | 72.34ms | 3.416× | 5.36% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.51ms | 23.12ms | 1.127× | 2.65% | PASS |
| mem_reads | `oltp_range_select` | 11.52ms | 11.35ms | 0.985× | 2.83% | PASS |
| mem_reads | `oltp_sum_range` | 10.53ms | 11.37ms | 1.079× | 2.42% | PASS |
| mem_reads | `oltp_order_range` | 2.46ms | 2.26ms | 0.919× | 1.59% | PASS |
| mem_reads | `oltp_distinct_range` | 2.89ms | 2.73ms | 0.947× | 1.19% | PASS |
| mem_reads | `oltp_index_scan` | 3.09ms | 3.95ms | 1.277× | 3.61% | PASS |
| mem_reads | `select_random_points` | 18.23ms | 20.23ms | 1.110× | 2.63% | PASS |
| mem_reads | `select_random_ranges` | 4.89ms | 5.37ms | 1.096× | 1.59% | PASS |
| mem_reads | `covering_index_scan` | 2.50ms | 2.85ms | 1.143× | 8.91% | PASS |
| mem_reads | `groupby_scan` | 22.69ms | 21.96ms | 0.968× | 1.63% | PASS |
| mem_reads | `index_join` | 5.08ms | 6.85ms | 1.349× | 6.32% | PASS |
| mem_reads | `index_join_scan` | 3.12ms | 4.65ms | 1.493× | 3.51% | PASS |
| mem_reads | `types_table_scan` | 814.08ms | 803.19ms | 0.987× | 4.71% | PASS |
| mem_reads | `table_scan` | 885.62ms | 865.69ms | 0.977× | 5.50% | PASS |
| mem_reads | `oltp_read_only` | 94.89ms | 93.89ms | 0.990× | 1.73% | PASS |
| mem_writes | `oltp_bulk_insert` | 148.21ms | 216.42ms | 1.460× | 1.67% | PASS |
| mem_writes | `oltp_insert` | 11.71ms | 25.13ms | 2.147× | 2.16% | PASS |
| mem_writes | `oltp_update_index` | 46.08ms | 92.10ms | 1.999× | 2.71% | PASS |
| mem_writes | `oltp_update_non_index` | 36.31ms | 64.99ms | 1.790× | 2.41% | PASS |
| mem_writes | `oltp_delete_insert` | 34.13ms | 74.14ms | 2.172× | 2.81% | PASS |
| mem_writes | `oltp_write_only` | 18.45ms | 46.02ms | 2.495× | 3.04% | PASS |
| mem_writes | `types_delete_insert` | 22.58ms | 40.79ms | 1.807× | 2.63% | PASS |
| mem_writes | `oltp_read_write` | 58.47ms | 94.48ms | 1.616× | 3.43% | PASS |
| file_reads | `oltp_point_select` | 74.67ms | 42.91ms | 0.575× | 1.87% | PASS |
| file_reads | `oltp_range_select` | 17.41ms | 13.50ms | 0.775× | 1.88% | PASS |
| file_reads | `oltp_sum_range` | 16.29ms | 13.31ms | 0.817× | 2.01% | PASS |
| file_reads | `oltp_order_range` | 3.05ms | 2.47ms | 0.809× | 2.38% | PASS |
| file_reads | `oltp_distinct_range` | 3.63ms | 3.05ms | 0.839× | 1.49% | PASS |
| file_reads | `oltp_index_scan` | 9.10ms | 6.32ms | 0.694× | 1.89% | PASS |
| file_reads | `select_random_points` | 25.32ms | 23.04ms | 0.910× | 2.73% | PASS |
| file_reads | `select_random_ranges` | 10.52ms | 7.31ms | 0.696× | 1.51% | PASS |
| file_reads | `covering_index_scan` | 8.69ms | 5.14ms | 0.592× | 1.38% | PASS |
| file_reads | `groupby_scan` | 23.54ms | 22.21ms | 0.943× | 1.76% | PASS |
| file_reads | `index_join` | 8.64ms | 8.44ms | 0.977× | 3.16% | PASS |
| file_reads | `index_join_scan` | 3.83ms | 4.60ms | 1.202× | 6.71% | PASS |
| file_reads | `types_table_scan` | 734.41ms | 776.08ms | 1.057× | 2.31% | PASS |
| file_reads | `table_scan` | 830.04ms | 865.17ms | 1.042× | 1.72% | PASS |
| file_reads | `oltp_read_only` | 166.16ms | 117.22ms | 0.706× | 1.81% | PASS |
| file_writes | `oltp_bulk_insert` | 199.35ms | 270.81ms | 1.358× | 2.86% | PASS |
| file_writes | `oltp_insert` | 19.36ms | 38.88ms | 2.009× | 4.21% | PASS |
| file_writes | `oltp_update_index` | 145.05ms | 155.86ms | 1.074× | 4.02% | PASS |
| file_writes | `oltp_update_non_index` | 120.31ms | 118.10ms | 0.982× | 1.27% | PASS |
| file_writes | `oltp_delete_insert` | 120.31ms | 128.81ms | 1.071× | 6.22% | PASS |
| file_writes | `oltp_write_only` | 83.53ms | 98.61ms | 1.180× | 3.59% | PASS |
| file_writes | `types_delete_insert` | 75.75ms | 76.78ms | 1.014× | 8.99% | PASS |
| file_writes | `oltp_read_write` | 126.26ms | 147.85ms | 1.171× | 4.40% | PASS |
| ac_reads | `oltp_point_select` | 39.88ms | 42.63ms | 1.069× | 1.93% | PASS |
| ac_reads | `oltp_range_select` | 13.86ms | 13.74ms | 0.991× | 2.03% | PASS |
| ac_reads | `oltp_sum_range` | 12.84ms | 13.40ms | 1.044× | 2.53% | PASS |
| ac_reads | `oltp_order_range` | 2.80ms | 2.52ms | 0.900× | 1.95% | PASS |
| ac_reads | `oltp_distinct_range` | 3.32ms | 3.10ms | 0.932× | 1.74% | PASS |
| ac_reads | `oltp_index_scan` | 5.27ms | 6.10ms | 1.158× | 3.68% | PASS |
| ac_reads | `select_random_points` | 20.34ms | 21.88ms | 1.076× | 2.78% | PASS |
| ac_reads | `select_random_ranges` | 6.85ms | 7.27ms | 1.062× | 1.20% | PASS |
| ac_reads | `covering_index_scan` | 4.93ms | 5.07ms | 1.028× | 3.30% | PASS |
| ac_reads | `groupby_scan` | 23.05ms | 22.60ms | 0.980× | 1.88% | PASS |
| ac_reads | `index_join` | 6.62ms | 8.30ms | 1.254× | 4.24% | PASS |
| ac_reads | `index_join_scan` | 3.30ms | 4.46ms | 1.353× | 3.20% | PASS |
| ac_reads | `types_table_scan` | 719.88ms | 771.15ms | 1.071× | 2.24% | PASS |
| ac_reads | `table_scan` | 831.14ms | 855.12ms | 1.029× | 2.50% | PASS |
| ac_reads | `oltp_read_only` | 114.52ms | 114.73ms | 1.002× | 3.38% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.97ms | 57.52ms | 2.618× | 5.19% | PASS |
| ac_writes | `oltp_insert_ac` | 26.08ms | 72.83ms | 2.792× | 7.04% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.97ms | 81.06ms | 2.898× | 3.69% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.75ms | 66.18ms | 3.042× | 4.21% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.33ms | 73.81ms | 2.914× | 3.81% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.84ms | 73.86ms | 2.858× | 5.55% | PASS |
| ac_writes | `types_delete_insert_ac` | 26.27ms | 63.87ms | 2.432× | 7.73% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.25ms | 82.56ms | 2.922× | 4.87% | PASS |

</details>

## Version-control latency

Wall time: 2m 30s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 87.13ms | 200.00ms | 43.6% | 0.62% | PASS |
| `status_dirty_many_tables` | 90.34ms | 200.00ms | 45.2% | 0.54% | PASS |
| `diff_regular_working_one_table` | 82.00ms | 150.00ms | 54.7% | 0.50% | PASS |
| `diff_regular_working_many_tables` | 96.10ms | 200.00ms | 48.0% | 0.58% | PASS |
| `diff_stat_working_many_tables` | 96.35ms | 200.00ms | 48.2% | 0.45% | PASS |
| `diff_schema_working_many_tables` | 96.83ms | 200.00ms | 48.4% | 0.52% | PASS |
| `branch_list_many_branches` | 25.71ms | 100.00ms | 25.7% | 1.50% | PASS |
| `branch_create_delete` | 27.94ms | 100.00ms | 27.9% | 1.37% | PASS |
| `checkout_branch_clean` | 59.70ms | 200.00ms | 29.8% | 0.91% | PASS |
| `merge_data_no_conflicts` | 32.40ms | 150.00ms | 21.6% | 1.68% | PASS |
| `merge_schema_no_conflicts` | 25.25ms | 100.00ms | 25.2% | 1.46% | PASS |
| `merge_data_conflicts` | 130.62ms | 250.00ms | 52.2% | 0.28% | PASS |
| `merge_data_conflicts_with_resolve` | 130.66ms | 250.00ms | 52.3% | 0.25% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
