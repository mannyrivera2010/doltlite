# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-15 16:02 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260907.300.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/34981465491)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 59s | 8.91s | 11.53s | 1.294× | 1.63% | **PASS** |
| textpk | 69 | 55 | 1h 35m 1s | 11.89s | 12.18s | 1.025× | 1.95% | **PASS** |
| blobpk | 69 | 55 | 1h 27m 59s | 9.33s | 13.18s | 1.413× | 3.13% | **PASS** |
| compositepk | 69 | 55 | 1h 22m 49s | 8.85s | 10.41s | 1.177× | 1.42% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.03ms | 29.61ms | 1.232× | 1.72% | PASS |
| mem_reads | `oltp_range_select` | 10.18ms | 13.16ms | 1.292× | 1.53% | PASS |
| mem_reads | `oltp_sum_range` | 9.53ms | 12.41ms | 1.303× | 1.96% | PASS |
| mem_reads | `oltp_order_range` | 2.56ms | 3.01ms | 1.174× | 1.58% | PASS |
| mem_reads | `oltp_distinct_range` | 3.66ms | 4.08ms | 1.115× | 1.02% | PASS |
| mem_reads | `oltp_index_scan` | 3.95ms | 5.33ms | 1.349× | 1.51% | PASS |
| mem_reads | `select_random_points` | 10.15ms | 11.35ms | 1.118× | 2.99% | PASS |
| mem_reads | `select_random_ranges` | 3.06ms | 4.03ms | 1.316× | 1.85% | PASS |
| mem_reads | `covering_index_scan` | 4.25ms | 4.28ms | 1.007× | 2.24% | PASS |
| mem_reads | `groupby_scan` | 29.95ms | 32.85ms | 1.097× | 0.89% | PASS |
| mem_reads | `index_join` | 6.07ms | 8.17ms | 1.347× | 1.64% | PASS |
| mem_reads | `index_join_scan` | 3.48ms | 4.75ms | 1.367× | 1.87% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.33s | 1.276× | 0.59% | PASS |
| mem_reads | `table_scan` | 1.19s | 1.40s | 1.177× | 1.34% | PASS |
| mem_reads | `oltp_read_only` | 102.36ms | 124.05ms | 1.212× | 1.25% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.54ms | 252.85ms | 1.393× | 1.48% | PASS |
| mem_writes | `oltp_insert` | 15.48ms | 28.52ms | 1.842× | 0.79% | PASS |
| mem_writes | `oltp_update_index` | 50.73ms | 105.92ms | 2.088× | 1.61% | PASS |
| mem_writes | `oltp_update_non_index` | 34.66ms | 60.01ms | 1.731× | 1.44% | PASS |
| mem_writes | `oltp_delete_insert` | 45.42ms | 79.86ms | 1.758× | 1.53% | PASS |
| mem_writes | `oltp_write_only` | 22.16ms | 45.19ms | 2.040× | 1.81% | PASS |
| mem_writes | `types_delete_insert` | 24.55ms | 40.62ms | 1.654× | 1.49% | PASS |
| mem_writes | `oltp_read_write` | 68.45ms | 112.09ms | 1.638× | 1.93% | PASS |
| file_reads | `oltp_point_select` | 99.40ms | 55.36ms | 0.557× | 0.81% | PASS |
| file_reads | `oltp_range_select` | 18.16ms | 15.90ms | 0.876× | 1.86% | PASS |
| file_reads | `oltp_sum_range` | 17.94ms | 15.28ms | 0.852× | 1.31% | PASS |
| file_reads | `oltp_order_range` | 3.53ms | 3.40ms | 0.964× | 2.15% | PASS |
| file_reads | `oltp_distinct_range` | 4.66ms | 4.49ms | 0.964× | 1.58% | PASS |
| file_reads | `oltp_index_scan` | 11.86ms | 8.32ms | 0.702× | 1.69% | PASS |
| file_reads | `select_random_points` | 18.08ms | 13.98ms | 0.773× | 2.66% | PASS |
| file_reads | `select_random_ranges` | 10.57ms | 6.59ms | 0.623× | 1.32% | PASS |
| file_reads | `covering_index_scan` | 11.93ms | 7.09ms | 0.594× | 1.57% | PASS |
| file_reads | `groupby_scan` | 30.90ms | 33.33ms | 1.079× | 0.92% | PASS |
| file_reads | `index_join` | 10.40ms | 10.24ms | 0.985× | 1.81% | PASS |
| file_reads | `index_join_scan` | 4.49ms | 5.11ms | 1.138× | 1.57% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.33s | 1.277× | 0.95% | PASS |
| file_reads | `table_scan` | 1.20s | 1.40s | 1.168× | 1.47% | PASS |
| file_reads | `oltp_read_only` | 216.05ms | 162.68ms | 0.753× | 1.03% | PASS |
| file_writes | `oltp_bulk_insert` | 195.86ms | 273.20ms | 1.395× | 1.46% | PASS |
| file_writes | `oltp_insert` | 22.83ms | 36.26ms | 1.588× | 2.32% | PASS |
| file_writes | `oltp_update_index` | 79.16ms | 130.13ms | 1.644× | 1.71% | PASS |
| file_writes | `oltp_update_non_index` | 59.53ms | 82.65ms | 1.388× | 1.66% | PASS |
| file_writes | `oltp_delete_insert` | 70.16ms | 101.27ms | 1.443× | 1.63% | PASS |
| file_writes | `oltp_write_only` | 46.85ms | 65.45ms | 1.397× | 2.10% | PASS |
| file_writes | `types_delete_insert` | 40.56ms | 54.41ms | 1.341× | 1.96% | PASS |
| file_writes | `oltp_read_write` | 93.70ms | 131.45ms | 1.403× | 2.12% | PASS |
| ac_reads | `oltp_point_select` | 48.96ms | 55.31ms | 1.130× | 0.96% | PASS |
| ac_reads | `oltp_range_select` | 13.34ms | 15.89ms | 1.192× | 1.47% | PASS |
| ac_reads | `oltp_sum_range` | 12.76ms | 15.24ms | 1.195× | 1.34% | PASS |
| ac_reads | `oltp_order_range` | 3.00ms | 3.37ms | 1.123× | 1.75% | PASS |
| ac_reads | `oltp_distinct_range` | 4.10ms | 4.49ms | 1.095× | 1.90% | PASS |
| ac_reads | `oltp_index_scan` | 6.79ms | 8.24ms | 1.213× | 1.92% | PASS |
| ac_reads | `select_random_points` | 13.12ms | 14.11ms | 1.076× | 1.80% | PASS |
| ac_reads | `select_random_ranges` | 5.64ms | 6.62ms | 1.175× | 1.09% | PASS |
| ac_reads | `covering_index_scan` | 7.18ms | 7.12ms | 0.992× | 1.59% | PASS |
| ac_reads | `groupby_scan` | 30.27ms | 33.36ms | 1.102× | 0.87% | PASS |
| ac_reads | `index_join` | 7.80ms | 10.17ms | 1.303× | 1.96% | PASS |
| ac_reads | `index_join_scan` | 3.99ms | 5.08ms | 1.275× | 2.04% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.33s | 1.281× | 0.68% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.40s | 1.181× | 0.99% | PASS |
| ac_reads | `oltp_read_only` | 142.71ms | 162.56ms | 1.139× | 1.60% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.26ms | 82.94ms | 3.902× | 8.91% | PASS |
| ac_writes | `oltp_insert_ac` | 24.94ms | 100.76ms | 4.040× | 7.55% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.09ms | 113.87ms | 4.364× | 6.18% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.05ms | 90.60ms | 3.931× | 7.31% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.36ms | 103.57ms | 4.085× | 7.49% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.08ms | 103.75ms | 4.309× | 6.12% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.16ms | 95.84ms | 4.529× | 6.68% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.47ms | 111.07ms | 3.530× | 6.54% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.22ms | 34.91ms | 1.155× | 1.29% | PASS |
| mem_reads | `oltp_range_select` | 14.92ms | 13.73ms | 0.920× | 2.20% | PASS |
| mem_reads | `oltp_sum_range` | 12.74ms | 13.43ms | 1.054× | 1.34% | PASS |
| mem_reads | `oltp_order_range` | 3.22ms | 3.23ms | 1.004× | 1.26% | PASS |
| mem_reads | `oltp_distinct_range` | 4.26ms | 4.25ms | 0.998× | 0.98% | PASS |
| mem_reads | `oltp_index_scan` | 4.93ms | 6.31ms | 1.279× | 2.84% | PASS |
| mem_reads | `select_random_points` | 19.21ms | 21.05ms | 1.096× | 1.64% | PASS |
| mem_reads | `select_random_ranges` | 4.34ms | 5.37ms | 1.238× | 1.35% | PASS |
| mem_reads | `covering_index_scan` | 5.67ms | 4.90ms | 0.864× | 3.91% | PASS |
| mem_reads | `groupby_scan` | 34.95ms | 35.83ms | 1.025× | 0.85% | PASS |
| mem_reads | `index_join` | 7.71ms | 9.95ms | 1.289× | 2.93% | PASS |
| mem_reads | `index_join_scan` | 4.99ms | 5.79ms | 1.159× | 1.49% | PASS |
| mem_reads | `types_table_scan` | 1.23s | 1.28s | 1.044× | 1.97% | PASS |
| mem_reads | `table_scan` | 1.64s | 1.42s | 0.867× | 3.06% | PASS |
| mem_reads | `oltp_read_only` | 132.72ms | 134.61ms | 1.014× | 1.95% | PASS |
| mem_writes | `oltp_bulk_insert` | 232.15ms | 339.28ms | 1.461× | 0.85% | PASS |
| mem_writes | `oltp_insert` | 24.03ms | 40.47ms | 1.684× | 1.82% | PASS |
| mem_writes | `oltp_update_index` | 81.00ms | 144.58ms | 1.785× | 2.26% | PASS |
| mem_writes | `oltp_update_non_index` | 53.01ms | 91.77ms | 1.731× | 1.12% | PASS |
| mem_writes | `oltp_delete_insert` | 57.45ms | 111.65ms | 1.943× | 2.78% | PASS |
| mem_writes | `oltp_write_only` | 31.93ms | 67.04ms | 2.100× | 1.78% | PASS |
| mem_writes | `types_delete_insert` | 34.88ms | 56.85ms | 1.630× | 1.64% | PASS |
| mem_writes | `oltp_read_write` | 90.85ms | 139.95ms | 1.541× | 2.07% | PASS |
| file_reads | `oltp_point_select` | 128.61ms | 68.69ms | 0.534× | 0.54% | PASS |
| file_reads | `oltp_range_select` | 25.91ms | 17.34ms | 0.669× | 1.35% | PASS |
| file_reads | `oltp_sum_range` | 22.47ms | 16.79ms | 0.747× | 2.11% | PASS |
| file_reads | `oltp_order_range` | 4.10ms | 3.59ms | 0.875× | 2.31% | PASS |
| file_reads | `oltp_distinct_range` | 5.20ms | 4.60ms | 0.885× | 1.44% | PASS |
| file_reads | `oltp_index_scan` | 14.48ms | 9.52ms | 0.658× | 1.49% | PASS |
| file_reads | `select_random_points` | 28.86ms | 23.89ms | 0.828× | 2.11% | PASS |
| file_reads | `select_random_ranges` | 13.78ms | 8.55ms | 0.621× | 0.97% | PASS |
| file_reads | `covering_index_scan` | 15.45ms | 8.05ms | 0.521× | 2.69% | PASS |
| file_reads | `groupby_scan` | 35.42ms | 35.91ms | 1.014× | 1.26% | PASS |
| file_reads | `index_join` | 12.99ms | 11.50ms | 0.885× | 2.52% | PASS |
| file_reads | `index_join_scan` | 6.04ms | 6.23ms | 1.032× | 1.70% | PASS |
| file_reads | `types_table_scan` | 1.34s | 1.30s | 0.970× | 2.93% | PASS |
| file_reads | `table_scan` | 1.62s | 1.42s | 0.874× | 3.85% | PASS |
| file_reads | `oltp_read_only` | 271.38ms | 181.67ms | 0.669× | 1.04% | PASS |
| file_writes | `oltp_bulk_insert` | 255.43ms | 372.51ms | 1.458× | 0.94% | PASS |
| file_writes | `oltp_insert` | 57.66ms | 54.10ms | 0.938× | 21.75% | PASS |
| file_writes | `oltp_update_index` | 123.59ms | 180.71ms | 1.462× | 1.56% | PASS |
| file_writes | `oltp_update_non_index` | 99.33ms | 118.77ms | 1.196× | 11.61% | PASS |
| file_writes | `oltp_delete_insert` | 96.66ms | 141.07ms | 1.459× | 1.85% | PASS |
| file_writes | `oltp_write_only` | 85.55ms | 91.08ms | 1.065× | 9.44% | PASS |
| file_writes | `types_delete_insert` | 58.53ms | 77.34ms | 1.321× | 1.99% | PASS |
| file_writes | `oltp_read_write` | 154.45ms | 167.38ms | 1.084× | 10.01% | PASS |
| ac_reads | `oltp_point_select` | 63.95ms | 68.58ms | 1.072× | 1.61% | PASS |
| ac_reads | `oltp_range_select` | 18.93ms | 17.32ms | 0.915× | 2.16% | PASS |
| ac_reads | `oltp_sum_range` | 17.15ms | 17.18ms | 1.002× | 1.86% | PASS |
| ac_reads | `oltp_order_range` | 3.75ms | 3.61ms | 0.963× | 1.27% | PASS |
| ac_reads | `oltp_distinct_range` | 4.73ms | 4.61ms | 0.976× | 0.82% | PASS |
| ac_reads | `oltp_index_scan` | 8.47ms | 9.56ms | 1.129× | 0.90% | PASS |
| ac_reads | `select_random_points` | 22.62ms | 23.77ms | 1.051× | 1.96% | PASS |
| ac_reads | `select_random_ranges` | 7.63ms | 8.56ms | 1.122× | 1.08% | PASS |
| ac_reads | `covering_index_scan` | 9.67ms | 8.12ms | 0.840× | 1.72% | PASS |
| ac_reads | `groupby_scan` | 35.22ms | 36.13ms | 1.026× | 0.63% | PASS |
| ac_reads | `index_join` | 10.05ms | 11.61ms | 1.156× | 1.33% | PASS |
| ac_reads | `index_join_scan` | 5.86ms | 6.68ms | 1.141× | 2.34% | PASS |
| ac_reads | `types_table_scan` | 1.43s | 1.34s | 0.937× | 3.94% | PASS |
| ac_reads | `table_scan` | 1.67s | 1.43s | 0.854× | 2.48% | PASS |
| ac_reads | `oltp_read_only` | 179.31ms | 182.70ms | 1.019× | 1.46% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.31ms | 67.71ms | 3.911× | 5.18% | PASS |
| ac_writes | `oltp_insert_ac` | 20.73ms | 84.70ms | 4.086× | 6.31% | PASS |
| ac_writes | `oltp_update_index_ac` | 23.02ms | 105.12ms | 4.566× | 5.62% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 18.16ms | 82.79ms | 4.558× | 6.83% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 20.38ms | 92.48ms | 4.539× | 4.66% | PASS |
| ac_writes | `oltp_write_only_ac` | 20.15ms | 89.64ms | 4.449× | 5.37% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.06ms | 80.35ms | 4.709× | 7.10% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.14ms | 102.05ms | 3.760× | 4.31% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 18.73ms | 21.20ms | 1.131× | 2.00% | PASS |
| mem_reads | `oltp_range_select` | 8.70ms | 8.35ms | 0.960× | 5.21% | PASS |
| mem_reads | `oltp_sum_range` | 8.19ms | 8.27ms | 1.010× | 4.64% | PASS |
| mem_reads | `oltp_order_range` | 1.85ms | 1.92ms | 1.037× | 3.88% | PASS |
| mem_reads | `oltp_distinct_range` | 2.44ms | 2.41ms | 0.991× | 4.10% | PASS |
| mem_reads | `oltp_index_scan` | 2.85ms | 3.92ms | 1.374× | 3.61% | PASS |
| mem_reads | `select_random_points` | 12.48ms | 13.67ms | 1.095× | 4.47% | PASS |
| mem_reads | `select_random_ranges` | 3.06ms | 3.55ms | 1.160× | 2.96% | PASS |
| mem_reads | `covering_index_scan` | 2.49ms | 3.17ms | 1.270× | 2.98% | PASS |
| mem_reads | `groupby_scan` | 19.77ms | 19.59ms | 0.991× | 2.78% | PASS |
| mem_reads | `index_join` | 4.31ms | 5.43ms | 1.259× | 3.01% | PASS |
| mem_reads | `index_join_scan` | 2.37ms | 4.32ms | 1.820× | 13.35% | PASS |
| mem_reads | `types_table_scan` | 693.53ms | 743.74ms | 1.072× | 2.28% | PASS |
| mem_reads | `table_scan` | 804.13ms | 837.41ms | 1.041× | 3.59% | PASS |
| mem_reads | `oltp_read_only` | 66.70ms | 71.21ms | 1.068× | 3.02% | PASS |
| mem_writes | `oltp_bulk_insert` | 140.29ms | 202.43ms | 1.443× | 1.82% | PASS |
| mem_writes | `oltp_insert` | 11.83ms | 24.62ms | 2.081× | 1.78% | PASS |
| mem_writes | `oltp_update_index` | 41.96ms | 85.06ms | 2.027× | 3.08% | PASS |
| mem_writes | `oltp_update_non_index` | 31.17ms | 55.56ms | 1.782× | 3.33% | PASS |
| mem_writes | `oltp_delete_insert` | 30.12ms | 65.84ms | 2.186× | 3.30% | PASS |
| mem_writes | `oltp_write_only` | 18.02ms | 42.72ms | 2.371× | 3.20% | PASS |
| mem_writes | `types_delete_insert` | 20.67ms | 35.32ms | 1.708× | 3.20% | PASS |
| mem_writes | `oltp_read_write` | 47.14ms | 78.98ms | 1.675× | 2.40% | PASS |
| file_reads | `oltp_point_select` | 75.60ms | 41.34ms | 0.547× | 2.40% | PASS |
| file_reads | `oltp_range_select` | 15.33ms | 10.40ms | 0.679× | 2.62% | PASS |
| file_reads | `oltp_sum_range` | 14.66ms | 10.42ms | 0.711× | 1.99% | PASS |
| file_reads | `oltp_order_range` | 2.74ms | 2.21ms | 0.808× | 1.60% | PASS |
| file_reads | `oltp_distinct_range` | 3.29ms | 2.71ms | 0.825× | 1.66% | PASS |
| file_reads | `oltp_index_scan` | 9.29ms | 6.25ms | 0.672× | 2.07% | PASS |
| file_reads | `select_random_points` | 18.18ms | 15.12ms | 0.832× | 2.38% | PASS |
| file_reads | `select_random_ranges` | 8.82ms | 5.50ms | 0.623× | 1.46% | PASS |
| file_reads | `covering_index_scan` | 9.33ms | 5.20ms | 0.558× | 2.80% | PASS |
| file_reads | `groupby_scan` | 20.23ms | 19.43ms | 0.961× | 3.44% | PASS |
| file_reads | `index_join` | 8.18ms | 7.46ms | 0.912× | 2.48% | PASS |
| file_reads | `index_join_scan` | 3.84ms | 4.42ms | 1.150× | 2.60% | PASS |
| file_reads | `types_table_scan` | 689.77ms | 743.00ms | 1.077× | 2.14% | PASS |
| file_reads | `table_scan` | 863.90ms | 859.91ms | 0.995× | 5.89% | PASS |
| file_reads | `oltp_read_only` | 160.79ms | 104.35ms | 0.649× | 2.63% | PASS |
| file_writes | `oltp_bulk_insert` | 251.74ms | 289.06ms | 1.148× | 21.21% | PASS |
| file_writes | `oltp_insert` | 94.58ms | 61.71ms | 0.652× | 56.78% | PASS |
| file_writes | `oltp_update_index` | 219.50ms | 193.59ms | 0.882× | 22.60% | PASS |
| file_writes | `oltp_update_non_index` | 214.83ms | 137.68ms | 0.641× | 29.02% | PASS |
| file_writes | `oltp_delete_insert` | 215.59ms | 168.55ms | 0.782× | 28.64% | PASS |
| file_writes | `oltp_write_only` | 170.66ms | 121.52ms | 0.712× | 37.94% | PASS |
| file_writes | `types_delete_insert` | 143.60ms | 115.69ms | 0.806× | 33.79% | PASS |
| file_writes | `oltp_read_write` | 232.22ms | 165.78ms | 0.714× | 38.43% | PASS |
| ac_reads | `oltp_point_select` | 38.48ms | 42.03ms | 1.092× | 2.57% | PASS |
| ac_reads | `oltp_range_select` | 11.07ms | 10.61ms | 0.959× | 2.93% | PASS |
| ac_reads | `oltp_sum_range` | 10.93ms | 10.39ms | 0.951× | 2.13% | PASS |
| ac_reads | `oltp_order_range` | 2.39ms | 2.21ms | 0.924× | 2.09% | PASS |
| ac_reads | `oltp_distinct_range` | 2.86ms | 2.67ms | 0.934× | 1.93% | PASS |
| ac_reads | `oltp_index_scan` | 5.08ms | 6.08ms | 1.195× | 3.75% | PASS |
| ac_reads | `select_random_points` | 14.46ms | 15.11ms | 1.045× | 3.13% | PASS |
| ac_reads | `select_random_ranges` | 5.07ms | 5.40ms | 1.063× | 1.65% | PASS |
| ac_reads | `covering_index_scan` | 5.53ms | 5.20ms | 0.940× | 3.69% | PASS |
| ac_reads | `groupby_scan` | 20.03ms | 19.75ms | 0.986× | 2.10% | PASS |
| ac_reads | `index_join` | 6.25ms | 7.45ms | 1.192× | 3.14% | PASS |
| ac_reads | `index_join_scan` | 3.64ms | 4.52ms | 1.241× | 2.36% | PASS |
| ac_reads | `types_table_scan` | 700.01ms | 751.10ms | 1.073× | 2.21% | PASS |
| ac_reads | `table_scan` | 855.93ms | 857.07ms | 1.001× | 3.54% | PASS |
| ac_reads | `oltp_read_only` | 93.17ms | 98.77ms | 1.060× | 3.03% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 245.77ms | 690.53ms | 2.810× | 55.70% | PASS |
| ac_writes | `oltp_insert_ac` | 285.17ms | 749.66ms | 2.629× | 43.77% | PASS |
| ac_writes | `oltp_update_index_ac` | 218.42ms | 753.01ms | 3.448× | 70.07% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 308.44ms | 697.41ms | 2.261× | 67.37% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 247.25ms | 780.19ms | 3.155× | 63.14% | PASS |
| ac_writes | `oltp_write_only_ac` | 231.63ms | 645.11ms | 2.785× | 61.61% | PASS |
| ac_writes | `types_delete_insert_ac` | 361.56ms | 911.03ms | 2.520× | 55.45% | PASS |
| ac_writes | `oltp_read_write_ac` | 217.02ms | 688.53ms | 3.173× | 64.48% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 27.51ms | 30.15ms | 1.096× | 2.69% | PASS |
| mem_reads | `oltp_range_select` | 17.00ms | 15.89ms | 0.935× | 1.85% | PASS |
| mem_reads | `oltp_sum_range` | 15.44ms | 15.27ms | 0.989× | 1.78% | PASS |
| mem_reads | `oltp_order_range` | 3.09ms | 2.99ms | 0.967× | 1.17% | PASS |
| mem_reads | `oltp_distinct_range` | 3.91ms | 3.80ms | 0.972× | 1.12% | PASS |
| mem_reads | `oltp_index_scan` | 3.90ms | 4.83ms | 1.238× | 2.35% | PASS |
| mem_reads | `select_random_points` | 23.58ms | 24.53ms | 1.040× | 1.91% | PASS |
| mem_reads | `select_random_ranges` | 6.24ms | 6.58ms | 1.053× | 1.42% | PASS |
| mem_reads | `covering_index_scan` | 3.41ms | 3.39ms | 0.993× | 2.53% | PASS |
| mem_reads | `groupby_scan` | 30.72ms | 31.16ms | 1.014× | 0.75% | PASS |
| mem_reads | `index_join` | 6.45ms | 7.91ms | 1.227× | 1.44% | PASS |
| mem_reads | `index_join_scan` | 3.49ms | 4.59ms | 1.316× | 2.07% | PASS |
| mem_reads | `types_table_scan` | 887.00ms | 980.20ms | 1.105× | 0.98% | PASS |
| mem_reads | `table_scan` | 1.06s | 1.07s | 1.013× | 3.45% | PASS |
| mem_reads | `oltp_read_only` | 124.64ms | 125.14ms | 1.004× | 1.85% | PASS |
| mem_writes | `oltp_bulk_insert` | 190.89ms | 259.51ms | 1.359× | 0.70% | PASS |
| mem_writes | `oltp_insert` | 15.21ms | 27.43ms | 1.803× | 0.79% | PASS |
| mem_writes | `oltp_update_index` | 56.59ms | 93.95ms | 1.660× | 1.71% | PASS |
| mem_writes | `oltp_update_non_index` | 43.02ms | 65.78ms | 1.529× | 1.67% | PASS |
| mem_writes | `oltp_delete_insert` | 41.76ms | 76.17ms | 1.824× | 1.35% | PASS |
| mem_writes | `oltp_write_only` | 23.93ms | 47.36ms | 1.979× | 1.69% | PASS |
| mem_writes | `types_delete_insert` | 27.41ms | 41.50ms | 1.514× | 1.95% | PASS |
| mem_writes | `oltp_read_write` | 80.98ms | 115.85ms | 1.431× | 1.76% | PASS |
| file_reads | `oltp_point_select` | 101.53ms | 54.56ms | 0.537× | 1.07% | PASS |
| file_reads | `oltp_range_select` | 24.00ms | 18.21ms | 0.759× | 1.16% | PASS |
| file_reads | `oltp_sum_range` | 22.39ms | 17.66ms | 0.789× | 0.91% | PASS |
| file_reads | `oltp_order_range` | 3.93ms | 3.25ms | 0.827× | 0.77% | PASS |
| file_reads | `oltp_distinct_range` | 4.70ms | 4.08ms | 0.868× | 1.27% | PASS |
| file_reads | `oltp_index_scan` | 11.54ms | 7.53ms | 0.653× | 1.06% | PASS |
| file_reads | `select_random_points` | 30.04ms | 26.58ms | 0.885× | 0.92% | PASS |
| file_reads | `select_random_ranges` | 13.78ms | 9.22ms | 0.669× | 0.80% | PASS |
| file_reads | `covering_index_scan` | 11.26ms | 6.23ms | 0.553× | 1.03% | PASS |
| file_reads | `groupby_scan` | 31.55ms | 31.54ms | 1.000× | 0.68% | PASS |
| file_reads | `index_join` | 10.72ms | 10.07ms | 0.940× | 0.91% | PASS |
| file_reads | `index_join_scan` | 4.34ms | 4.96ms | 1.143× | 0.99% | PASS |
| file_reads | `types_table_scan` | 867.37ms | 966.47ms | 1.114× | 0.82% | PASS |
| file_reads | `table_scan` | 1.01s | 1.05s | 1.047× | 0.65% | PASS |
| file_reads | `oltp_read_only` | 227.17ms | 159.29ms | 0.701× | 0.70% | PASS |
| file_writes | `oltp_bulk_insert` | 249.13ms | 334.05ms | 1.341× | 5.56% | PASS |
| file_writes | `oltp_insert` | 38.76ms | 57.75ms | 1.490× | 40.32% | PASS |
| file_writes | `oltp_update_index` | 173.63ms | 171.96ms | 0.990× | 9.91% | PASS |
| file_writes | `oltp_update_non_index` | 172.74ms | 141.70ms | 0.820× | 18.54% | PASS |
| file_writes | `oltp_delete_insert` | 146.51ms | 155.17ms | 1.059× | 7.02% | PASS |
| file_writes | `oltp_write_only` | 112.56ms | 107.84ms | 0.958× | 18.12% | PASS |
| file_writes | `types_delete_insert` | 94.08ms | 93.97ms | 0.999× | 16.12% | PASS |
| file_writes | `oltp_read_write` | 167.69ms | 184.89ms | 1.103× | 14.17% | PASS |
| ac_reads | `oltp_point_select` | 51.42ms | 55.01ms | 1.070× | 1.20% | PASS |
| ac_reads | `oltp_range_select` | 19.74ms | 18.43ms | 0.934× | 1.90% | PASS |
| ac_reads | `oltp_sum_range` | 17.95ms | 17.93ms | 0.999× | 1.36% | PASS |
| ac_reads | `oltp_order_range` | 3.47ms | 3.25ms | 0.937× | 0.88% | PASS |
| ac_reads | `oltp_distinct_range` | 4.28ms | 4.09ms | 0.956× | 0.93% | PASS |
| ac_reads | `oltp_index_scan` | 6.71ms | 7.55ms | 1.126× | 0.93% | PASS |
| ac_reads | `select_random_points` | 25.49ms | 26.86ms | 1.054× | 1.24% | PASS |
| ac_reads | `select_random_ranges` | 8.81ms | 9.21ms | 1.045× | 1.06% | PASS |
| ac_reads | `covering_index_scan` | 6.31ms | 6.23ms | 0.988× | 0.83% | PASS |
| ac_reads | `groupby_scan` | 30.78ms | 31.45ms | 1.022× | 0.59% | PASS |
| ac_reads | `index_join` | 8.25ms | 10.07ms | 1.221× | 0.61% | PASS |
| ac_reads | `index_join_scan` | 3.90ms | 5.00ms | 1.279× | 1.31% | PASS |
| ac_reads | `types_table_scan` | 871.32ms | 970.18ms | 1.113× | 1.02% | PASS |
| ac_reads | `table_scan` | 1.05s | 1.07s | 1.020× | 2.27% | PASS |
| ac_reads | `oltp_read_only` | 158.45ms | 160.17ms | 1.011× | 1.87% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 36.04ms | 142.57ms | 3.956× | 49.48% | PASS |
| ac_writes | `oltp_insert_ac` | 52.40ms | 192.94ms | 3.682× | 59.59% | PASS |
| ac_writes | `oltp_update_index_ac` | 52.55ms | 182.10ms | 3.465× | 41.92% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 36.89ms | 116.10ms | 3.147× | 30.02% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 44.80ms | 209.80ms | 4.683× | 57.06% | PASS |
| ac_writes | `oltp_write_only_ac` | 43.65ms | 166.29ms | 3.810× | 30.60% | PASS |
| ac_writes | `types_delete_insert_ac` | 36.56ms | 129.61ms | 3.545× | 30.12% | PASS |
| ac_writes | `oltp_read_write_ac` | 58.46ms | 203.24ms | 3.477× | 33.19% | PASS |

</details>

## Version-control latency

Wall time: 2m 22s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.94ms | 200.00ms | 42.0% | 0.47% | PASS |
| `status_dirty_many_tables` | 87.32ms | 200.00ms | 43.7% | 0.50% | PASS |
| `diff_regular_working_one_table` | 79.59ms | 150.00ms | 53.1% | 0.42% | PASS |
| `diff_regular_working_many_tables` | 92.83ms | 200.00ms | 46.4% | 0.41% | PASS |
| `diff_stat_working_many_tables` | 93.24ms | 200.00ms | 46.6% | 0.54% | PASS |
| `diff_schema_working_many_tables` | 93.30ms | 200.00ms | 46.6% | 0.47% | PASS |
| `branch_list_many_branches` | 23.48ms | 100.00ms | 23.5% | 1.26% | PASS |
| `branch_create_delete` | 25.82ms | 100.00ms | 25.8% | 1.70% | PASS |
| `checkout_branch_clean` | 56.95ms | 200.00ms | 28.5% | 1.25% | PASS |
| `merge_data_no_conflicts` | 29.89ms | 150.00ms | 19.9% | 1.43% | PASS |
| `merge_schema_no_conflicts` | 23.32ms | 100.00ms | 23.3% | 1.87% | PASS |
| `merge_data_conflicts` | 128.48ms | 250.00ms | 51.4% | 0.25% | PASS |
| `merge_data_conflicts_with_resolve` | 128.17ms | 250.00ms | 51.3% | 0.25% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
