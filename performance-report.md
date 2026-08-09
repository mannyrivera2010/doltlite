# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-09 11:40 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260720.247.2
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/31307514761)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 21s | 8.80s | 11.46s | 1.302× | 1.61% | **PASS** |
| textpk | 69 | 55 | 1h 23m 39s | 8.69s | 9.62s | 1.107× | 0.81% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 30s | 9.51s | 11.80s | 1.241× | 1.75% | **PASS** |
| compositepk | 69 | 55 | 1h 26m 24s | 10.24s | 11.81s | 1.153× | 0.94% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.72ms | 29.14ms | 1.282× | 1.14% | PASS |
| mem_reads | `oltp_range_select` | 9.27ms | 13.06ms | 1.408× | 1.37% | PASS |
| mem_reads | `oltp_sum_range` | 9.37ms | 12.29ms | 1.311× | 3.46% | PASS |
| mem_reads | `oltp_order_range` | 2.43ms | 2.97ms | 1.220× | 1.46% | PASS |
| mem_reads | `oltp_distinct_range` | 3.47ms | 4.00ms | 1.154× | 1.72% | PASS |
| mem_reads | `oltp_index_scan` | 3.75ms | 5.16ms | 1.376× | 1.51% | PASS |
| mem_reads | `select_random_points` | 9.14ms | 10.89ms | 1.191× | 2.18% | PASS |
| mem_reads | `select_random_ranges` | 2.81ms | 3.92ms | 1.394× | 2.10% | PASS |
| mem_reads | `covering_index_scan` | 4.23ms | 4.09ms | 0.968× | 1.32% | PASS |
| mem_reads | `groupby_scan` | 29.37ms | 32.95ms | 1.122× | 0.91% | PASS |
| mem_reads | `index_join` | 5.97ms | 7.97ms | 1.335× | 1.43% | PASS |
| mem_reads | `index_join_scan` | 3.23ms | 4.52ms | 1.399× | 3.27% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.33s | 1.285× | 1.00% | PASS |
| mem_reads | `table_scan` | 1.21s | 1.42s | 1.174× | 2.49% | PASS |
| mem_reads | `oltp_read_only` | 104.81ms | 124.39ms | 1.187× | 1.56% | PASS |
| mem_writes | `oltp_bulk_insert` | 182.09ms | 250.71ms | 1.377× | 1.26% | PASS |
| mem_writes | `oltp_insert` | 15.60ms | 28.45ms | 1.824× | 0.98% | PASS |
| mem_writes | `oltp_update_index` | 53.25ms | 110.75ms | 2.080× | 2.39% | PASS |
| mem_writes | `oltp_update_non_index` | 34.39ms | 59.12ms | 1.719× | 1.82% | PASS |
| mem_writes | `oltp_delete_insert` | 45.04ms | 77.68ms | 1.725× | 1.05% | PASS |
| mem_writes | `oltp_write_only` | 21.72ms | 44.36ms | 2.042× | 1.89% | PASS |
| mem_writes | `types_delete_insert` | 24.75ms | 40.01ms | 1.617× | 1.34% | PASS |
| mem_writes | `oltp_read_write` | 66.44ms | 109.54ms | 1.649× | 1.95% | PASS |
| file_reads | `oltp_point_select` | 97.34ms | 54.65ms | 0.561× | 0.91% | PASS |
| file_reads | `oltp_range_select` | 18.12ms | 15.90ms | 0.878× | 1.96% | PASS |
| file_reads | `oltp_sum_range` | 17.75ms | 15.17ms | 0.855× | 1.70% | PASS |
| file_reads | `oltp_order_range` | 3.43ms | 3.32ms | 0.968× | 1.55% | PASS |
| file_reads | `oltp_distinct_range` | 4.47ms | 4.39ms | 0.980× | 1.20% | PASS |
| file_reads | `oltp_index_scan` | 11.75ms | 8.19ms | 0.697× | 1.81% | PASS |
| file_reads | `select_random_points` | 18.45ms | 13.86ms | 0.751× | 3.50% | PASS |
| file_reads | `select_random_ranges` | 10.51ms | 6.57ms | 0.625× | 1.27% | PASS |
| file_reads | `covering_index_scan` | 12.07ms | 6.92ms | 0.573× | 1.80% | PASS |
| file_reads | `groupby_scan` | 30.54ms | 33.40ms | 1.094× | 1.12% | PASS |
| file_reads | `index_join` | 10.35ms | 9.97ms | 0.963× | 1.61% | PASS |
| file_reads | `index_join_scan` | 4.45ms | 4.90ms | 1.102× | 2.36% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.33s | 1.280× | 0.77% | PASS |
| file_reads | `table_scan` | 1.16s | 1.39s | 1.195× | 0.57% | PASS |
| file_reads | `oltp_read_only` | 209.49ms | 161.63ms | 0.772× | 1.06% | PASS |
| file_writes | `oltp_bulk_insert` | 195.90ms | 272.93ms | 1.393× | 1.63% | PASS |
| file_writes | `oltp_insert` | 22.28ms | 35.70ms | 1.602× | 1.82% | PASS |
| file_writes | `oltp_update_index` | 76.53ms | 126.40ms | 1.652× | 1.27% | PASS |
| file_writes | `oltp_update_non_index` | 57.16ms | 79.93ms | 1.398× | 2.01% | PASS |
| file_writes | `oltp_delete_insert` | 66.82ms | 97.61ms | 1.461× | 1.23% | PASS |
| file_writes | `oltp_write_only` | 43.72ms | 63.63ms | 1.455× | 1.68% | PASS |
| file_writes | `types_delete_insert` | 39.26ms | 52.73ms | 1.343× | 1.62% | PASS |
| file_writes | `oltp_read_write` | 89.01ms | 128.78ms | 1.447× | 1.46% | PASS |
| ac_reads | `oltp_point_select` | 47.40ms | 54.70ms | 1.154× | 0.79% | PASS |
| ac_reads | `oltp_range_select` | 12.28ms | 15.87ms | 1.292× | 1.61% | PASS |
| ac_reads | `oltp_sum_range` | 11.99ms | 15.17ms | 1.265× | 1.27% | PASS |
| ac_reads | `oltp_order_range` | 2.90ms | 3.39ms | 1.169× | 1.79% | PASS |
| ac_reads | `oltp_distinct_range` | 3.85ms | 4.40ms | 1.142× | 1.68% | PASS |
| ac_reads | `oltp_index_scan` | 6.41ms | 8.16ms | 1.273× | 1.56% | PASS |
| ac_reads | `select_random_points` | 12.80ms | 13.95ms | 1.090× | 2.39% | PASS |
| ac_reads | `select_random_ranges` | 5.42ms | 6.59ms | 1.216× | 0.94% | PASS |
| ac_reads | `covering_index_scan` | 6.97ms | 7.10ms | 1.018× | 2.28% | PASS |
| ac_reads | `groupby_scan` | 29.84ms | 33.51ms | 1.123× | 1.04% | PASS |
| ac_reads | `index_join` | 7.62ms | 10.13ms | 1.329× | 1.50% | PASS |
| ac_reads | `index_join_scan` | 3.85ms | 4.93ms | 1.281× | 2.17% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.32s | 1.280× | 0.50% | PASS |
| ac_reads | `table_scan` | 1.16s | 1.39s | 1.199× | 0.39% | PASS |
| ac_reads | `oltp_read_only` | 136.31ms | 160.94ms | 1.181× | 0.83% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.97ms | 77.97ms | 3.718× | 2.68% | PASS |
| ac_writes | `oltp_insert_ac` | 23.75ms | 95.50ms | 4.020× | 4.13% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.76ms | 109.43ms | 4.248× | 3.20% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.41ms | 89.79ms | 4.007× | 5.62% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.30ms | 101.16ms | 4.341× | 3.46% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.12ms | 100.43ms | 4.164× | 5.38% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.38ms | 92.45ms | 4.324× | 5.62% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.72ms | 106.63ms | 3.713× | 3.13% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.76ms | 27.46ms | 1.156× | 1.12% | PASS |
| mem_reads | `oltp_range_select` | 11.09ms | 10.84ms | 0.978× | 1.30% | PASS |
| mem_reads | `oltp_sum_range` | 9.74ms | 10.52ms | 1.080× | 1.05% | PASS |
| mem_reads | `oltp_order_range` | 2.52ms | 2.51ms | 0.998× | 1.14% | PASS |
| mem_reads | `oltp_distinct_range` | 3.37ms | 3.27ms | 0.973× | 0.89% | PASS |
| mem_reads | `oltp_index_scan` | 3.66ms | 4.64ms | 1.268× | 1.19% | PASS |
| mem_reads | `select_random_points` | 14.09ms | 15.95ms | 1.132× | 0.84% | PASS |
| mem_reads | `select_random_ranges` | 3.28ms | 4.13ms | 1.260× | 1.28% | PASS |
| mem_reads | `covering_index_scan` | 3.74ms | 3.42ms | 0.915× | 1.46% | PASS |
| mem_reads | `groupby_scan` | 26.81ms | 27.69ms | 1.033× | 0.50% | PASS |
| mem_reads | `index_join` | 5.64ms | 6.86ms | 1.215× | 1.40% | PASS |
| mem_reads | `index_join_scan` | 4.01ms | 4.62ms | 1.153× | 1.19% | PASS |
| mem_reads | `types_table_scan` | 887.75ms | 969.91ms | 1.093× | 0.50% | PASS |
| mem_reads | `table_scan` | 1.04s | 1.05s | 1.017× | 0.42% | PASS |
| mem_reads | `oltp_read_only` | 93.87ms | 100.43ms | 1.070× | 0.44% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.24ms | 259.21ms | 1.430× | 0.65% | PASS |
| mem_writes | `oltp_insert` | 17.58ms | 29.66ms | 1.688× | 0.57% | PASS |
| mem_writes | `oltp_update_index` | 57.32ms | 102.93ms | 1.796× | 0.69% | PASS |
| mem_writes | `oltp_update_non_index` | 39.93ms | 66.61ms | 1.668× | 0.65% | PASS |
| mem_writes | `oltp_delete_insert` | 41.49ms | 80.25ms | 1.934× | 0.75% | PASS |
| mem_writes | `oltp_write_only` | 23.74ms | 47.45ms | 1.999× | 1.03% | PASS |
| mem_writes | `types_delete_insert` | 26.33ms | 40.36ms | 1.533× | 0.81% | PASS |
| mem_writes | `oltp_read_write` | 66.19ms | 103.08ms | 1.557× | 0.59% | PASS |
| file_reads | `oltp_point_select` | 98.66ms | 52.40ms | 0.531× | 0.58% | PASS |
| file_reads | `oltp_range_select` | 20.12ms | 13.46ms | 0.669× | 0.55% | PASS |
| file_reads | `oltp_sum_range` | 18.36ms | 13.29ms | 0.724× | 0.67% | PASS |
| file_reads | `oltp_order_range` | 3.56ms | 2.84ms | 0.798× | 1.04% | PASS |
| file_reads | `oltp_distinct_range` | 4.32ms | 3.59ms | 0.830× | 0.91% | PASS |
| file_reads | `oltp_index_scan` | 11.70ms | 7.54ms | 0.645× | 0.77% | PASS |
| file_reads | `select_random_points` | 22.73ms | 18.82ms | 0.828× | 0.64% | PASS |
| file_reads | `select_random_ranges` | 11.08ms | 6.73ms | 0.607× | 0.52% | PASS |
| file_reads | `covering_index_scan` | 12.63ms | 6.39ms | 0.506× | 0.59% | PASS |
| file_reads | `groupby_scan` | 28.17ms | 27.99ms | 0.994× | 0.45% | PASS |
| file_reads | `index_join` | 10.60ms | 9.13ms | 0.861× | 0.94% | PASS |
| file_reads | `index_join_scan` | 4.94ms | 5.05ms | 1.023× | 0.82% | PASS |
| file_reads | `types_table_scan` | 893.47ms | 962.40ms | 1.077× | 0.60% | PASS |
| file_reads | `table_scan` | 1.04s | 1.05s | 1.006× | 0.45% | PASS |
| file_reads | `oltp_read_only` | 203.49ms | 136.26ms | 0.670× | 0.39% | PASS |
| file_writes | `oltp_bulk_insert` | 246.11ms | 347.20ms | 1.411× | 2.88% | PASS |
| file_writes | `oltp_insert` | 60.33ms | 58.61ms | 0.971× | 2.48% | PASS |
| file_writes | `oltp_update_index` | 205.05ms | 198.88ms | 0.970× | 1.71% | PASS |
| file_writes | `oltp_update_non_index` | 157.48ms | 135.88ms | 0.863× | 2.38% | PASS |
| file_writes | `oltp_delete_insert` | 168.25ms | 159.71ms | 0.949× | 2.10% | PASS |
| file_writes | `oltp_write_only` | 127.45ms | 115.58ms | 0.907× | 3.09% | PASS |
| file_writes | `types_delete_insert` | 100.22ms | 86.55ms | 0.864× | 7.85% | PASS |
| file_writes | `oltp_read_write` | 170.84ms | 169.10ms | 0.990× | 1.57% | PASS |
| ac_reads | `oltp_point_select` | 48.68ms | 52.47ms | 1.078× | 0.74% | PASS |
| ac_reads | `oltp_range_select` | 14.85ms | 13.45ms | 0.906× | 1.03% | PASS |
| ac_reads | `oltp_sum_range` | 13.27ms | 13.31ms | 1.003× | 0.70% | PASS |
| ac_reads | `oltp_order_range` | 3.06ms | 2.83ms | 0.927× | 0.77% | PASS |
| ac_reads | `oltp_distinct_range` | 3.81ms | 3.60ms | 0.943× | 0.73% | PASS |
| ac_reads | `oltp_index_scan` | 6.78ms | 7.54ms | 1.112× | 0.79% | PASS |
| ac_reads | `select_random_points` | 17.74ms | 18.86ms | 1.063× | 0.63% | PASS |
| ac_reads | `select_random_ranges` | 6.17ms | 6.73ms | 1.091× | 0.59% | PASS |
| ac_reads | `covering_index_scan` | 7.67ms | 6.40ms | 0.834× | 0.71% | PASS |
| ac_reads | `groupby_scan` | 27.55ms | 28.05ms | 1.018× | 0.59% | PASS |
| ac_reads | `index_join` | 8.10ms | 9.13ms | 1.127× | 0.79% | PASS |
| ac_reads | `index_join_scan` | 4.54ms | 5.05ms | 1.111× | 0.86% | PASS |
| ac_reads | `types_table_scan` | 893.28ms | 964.08ms | 1.079× | 0.43% | PASS |
| ac_reads | `table_scan` | 1.04s | 1.05s | 1.008× | 0.29% | PASS |
| ac_reads | `oltp_read_only` | 130.54ms | 136.21ms | 1.043× | 0.54% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 28.29ms | 76.38ms | 2.700× | 4.06% | PASS |
| ac_writes | `oltp_insert_ac` | 31.40ms | 88.69ms | 2.825× | 5.22% | PASS |
| ac_writes | `oltp_update_index_ac` | 32.71ms | 102.07ms | 3.120× | 4.69% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 29.59ms | 89.69ms | 3.031× | 8.55% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 31.41ms | 99.20ms | 3.158× | 5.74% | PASS |
| ac_writes | `oltp_write_only_ac` | 32.03ms | 94.21ms | 2.941× | 4.88% | PASS |
| ac_writes | `types_delete_insert_ac` | 28.12ms | 86.30ms | 3.069× | 3.93% | PASS |
| ac_writes | `oltp_read_write_ac` | 37.34ms | 101.79ms | 2.726× | 5.78% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 28.88ms | 36.64ms | 1.269× | 1.43% | PASS |
| mem_reads | `oltp_range_select` | 11.53ms | 13.95ms | 1.211× | 1.55% | PASS |
| mem_reads | `oltp_sum_range` | 11.03ms | 13.79ms | 1.250× | 1.82% | PASS |
| mem_reads | `oltp_order_range` | 2.84ms | 3.15ms | 1.110× | 1.86% | PASS |
| mem_reads | `oltp_distinct_range` | 3.87ms | 4.20ms | 1.085× | 1.24% | PASS |
| mem_reads | `oltp_index_scan` | 4.34ms | 6.09ms | 1.405× | 1.66% | PASS |
| mem_reads | `select_random_points` | 16.78ms | 20.37ms | 1.214× | 2.79% | PASS |
| mem_reads | `select_random_ranges` | 3.98ms | 5.14ms | 1.290× | 1.99% | PASS |
| mem_reads | `covering_index_scan` | 4.40ms | 4.50ms | 1.023× | 2.35% | PASS |
| mem_reads | `groupby_scan` | 31.72ms | 34.00ms | 1.072× | 1.21% | PASS |
| mem_reads | `index_join` | 6.75ms | 9.04ms | 1.340× | 2.21% | PASS |
| mem_reads | `index_join_scan` | 4.07ms | 5.29ms | 1.298× | 2.47% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.23s | 1.180× | 0.99% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.36s | 1.156× | 1.13% | PASS |
| mem_reads | `oltp_read_only` | 113.39ms | 134.62ms | 1.187× | 1.12% | PASS |
| mem_writes | `oltp_bulk_insert` | 243.56ms | 351.44ms | 1.443× | 0.81% | PASS |
| mem_writes | `oltp_insert` | 20.06ms | 39.01ms | 1.944× | 0.80% | PASS |
| mem_writes | `oltp_update_index` | 70.15ms | 132.10ms | 1.883× | 2.19% | PASS |
| mem_writes | `oltp_update_non_index` | 48.22ms | 83.88ms | 1.740× | 1.43% | PASS |
| mem_writes | `oltp_delete_insert` | 47.65ms | 101.21ms | 2.124× | 1.14% | PASS |
| mem_writes | `oltp_write_only` | 27.52ms | 61.31ms | 2.228× | 1.05% | PASS |
| mem_writes | `types_delete_insert` | 33.05ms | 54.90ms | 1.661× | 2.32% | PASS |
| mem_writes | `oltp_read_write` | 81.34ms | 138.28ms | 1.700× | 1.14% | PASS |
| file_reads | `oltp_point_select` | 104.37ms | 63.17ms | 0.605× | 1.33% | PASS |
| file_reads | `oltp_range_select` | 20.62ms | 16.79ms | 0.814× | 2.71% | PASS |
| file_reads | `oltp_sum_range` | 19.90ms | 16.70ms | 0.839× | 1.54% | PASS |
| file_reads | `oltp_order_range` | 3.68ms | 3.49ms | 0.949× | 2.57% | PASS |
| file_reads | `oltp_distinct_range` | 4.68ms | 4.55ms | 0.972× | 2.34% | PASS |
| file_reads | `oltp_index_scan` | 12.40ms | 9.00ms | 0.726× | 1.63% | PASS |
| file_reads | `select_random_points` | 26.81ms | 24.04ms | 0.896× | 2.20% | PASS |
| file_reads | `select_random_ranges` | 11.64ms | 7.83ms | 0.673× | 1.44% | PASS |
| file_reads | `covering_index_scan` | 12.52ms | 7.30ms | 0.584× | 1.98% | PASS |
| file_reads | `groupby_scan` | 32.30ms | 34.61ms | 1.071× | 1.06% | PASS |
| file_reads | `index_join` | 11.09ms | 11.23ms | 1.012× | 2.45% | PASS |
| file_reads | `index_join_scan` | 5.12ms | 5.83ms | 1.139× | 2.30% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.24s | 1.167× | 1.99% | PASS |
| file_reads | `table_scan` | 1.35s | 1.39s | 1.032× | 4.03% | PASS |
| file_reads | `oltp_read_only` | 229.84ms | 174.04ms | 0.757× | 1.10% | PASS |
| file_writes | `oltp_bulk_insert` | 262.79ms | 377.91ms | 1.438× | 0.79% | PASS |
| file_writes | `oltp_insert` | 31.42ms | 51.21ms | 1.630× | 1.75% | PASS |
| file_writes | `oltp_update_index` | 101.09ms | 160.73ms | 1.590× | 0.99% | PASS |
| file_writes | `oltp_update_non_index` | 77.20ms | 106.88ms | 1.385× | 1.45% | PASS |
| file_writes | `oltp_delete_insert` | 79.48ms | 128.69ms | 1.619× | 1.75% | PASS |
| file_writes | `oltp_write_only` | 54.69ms | 83.11ms | 1.520× | 1.70% | PASS |
| file_writes | `types_delete_insert` | 50.99ms | 70.35ms | 1.380× | 2.11% | PASS |
| file_writes | `oltp_read_write` | 111.10ms | 160.45ms | 1.444× | 1.47% | PASS |
| ac_reads | `oltp_point_select` | 55.61ms | 63.14ms | 1.135× | 1.40% | PASS |
| ac_reads | `oltp_range_select` | 16.28ms | 16.89ms | 1.038× | 1.76% | PASS |
| ac_reads | `oltp_sum_range` | 14.87ms | 16.85ms | 1.133× | 2.58% | PASS |
| ac_reads | `oltp_order_range` | 3.26ms | 3.50ms | 1.074× | 1.47% | PASS |
| ac_reads | `oltp_distinct_range` | 4.22ms | 4.56ms | 1.079× | 1.74% | PASS |
| ac_reads | `oltp_index_scan` | 7.11ms | 9.03ms | 1.271× | 1.64% | PASS |
| ac_reads | `select_random_points` | 20.62ms | 24.34ms | 1.180× | 1.59% | PASS |
| ac_reads | `select_random_ranges` | 6.52ms | 7.77ms | 1.192× | 1.47% | PASS |
| ac_reads | `covering_index_scan` | 7.16ms | 7.16ms | 0.999× | 1.92% | PASS |
| ac_reads | `groupby_scan` | 31.85ms | 34.48ms | 1.083× | 0.84% | PASS |
| ac_reads | `index_join` | 8.43ms | 11.27ms | 1.336× | 1.89% | PASS |
| ac_reads | `index_join_scan` | 4.58ms | 5.85ms | 1.277× | 2.18% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.22s | 1.181× | 0.63% | PASS |
| ac_reads | `table_scan` | 1.24s | 1.37s | 1.109× | 3.52% | PASS |
| ac_reads | `oltp_read_only` | 152.06ms | 173.53ms | 1.141× | 0.91% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.83ms | 80.46ms | 3.685× | 6.96% | PASS |
| ac_writes | `oltp_insert_ac` | 24.22ms | 101.30ms | 4.183× | 3.49% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.98ms | 111.70ms | 4.299× | 4.52% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.55ms | 92.09ms | 4.083× | 6.04% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.41ms | 107.00ms | 4.385× | 8.35% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.58ms | 114.17ms | 4.295× | 8.81% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.50ms | 106.67ms | 4.741× | 8.55% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.58ms | 126.28ms | 3.877× | 6.76% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.53ms | 37.74ms | 1.126× | 1.37% | PASS |
| mem_reads | `oltp_range_select` | 19.92ms | 20.04ms | 1.006× | 1.77% | PASS |
| mem_reads | `oltp_sum_range` | 18.38ms | 19.14ms | 1.041× | 1.22% | PASS |
| mem_reads | `oltp_order_range` | 3.74ms | 3.79ms | 1.012× | 0.91% | PASS |
| mem_reads | `oltp_distinct_range` | 4.83ms | 4.85ms | 1.005× | 0.74% | PASS |
| mem_reads | `oltp_index_scan` | 4.70ms | 5.90ms | 1.255× | 1.21% | PASS |
| mem_reads | `select_random_points` | 27.63ms | 30.63ms | 1.109× | 0.94% | PASS |
| mem_reads | `select_random_ranges` | 7.56ms | 8.23ms | 1.088× | 0.96% | PASS |
| mem_reads | `covering_index_scan` | 4.37ms | 4.23ms | 0.966× | 1.71% | PASS |
| mem_reads | `groupby_scan` | 38.65ms | 40.14ms | 1.038× | 0.57% | PASS |
| mem_reads | `index_join` | 8.14ms | 10.15ms | 1.246× | 1.02% | PASS |
| mem_reads | `index_join_scan` | 4.30ms | 5.66ms | 1.315× | 0.98% | PASS |
| mem_reads | `types_table_scan` | 1.13s | 1.25s | 1.107× | 0.52% | PASS |
| mem_reads | `table_scan` | 1.31s | 1.36s | 1.038× | 0.79% | PASS |
| mem_reads | `oltp_read_only` | 151.76ms | 159.00ms | 1.048× | 0.76% | PASS |
| mem_writes | `oltp_bulk_insert` | 246.65ms | 339.63ms | 1.377× | 0.63% | PASS |
| mem_writes | `oltp_insert` | 19.63ms | 36.27ms | 1.848× | 0.65% | PASS |
| mem_writes | `oltp_update_index` | 69.14ms | 119.18ms | 1.724× | 0.99% | PASS |
| mem_writes | `oltp_update_non_index` | 52.14ms | 84.40ms | 1.619× | 0.72% | PASS |
| mem_writes | `oltp_delete_insert` | 50.83ms | 96.81ms | 1.905× | 0.91% | PASS |
| mem_writes | `oltp_write_only` | 27.72ms | 59.53ms | 2.147× | 1.09% | PASS |
| mem_writes | `types_delete_insert` | 32.87ms | 53.95ms | 1.641× | 1.28% | PASS |
| mem_writes | `oltp_read_write` | 100.36ms | 148.09ms | 1.476× | 1.03% | PASS |
| file_reads | `oltp_point_select` | 129.72ms | 69.92ms | 0.539× | 1.05% | PASS |
| file_reads | `oltp_range_select` | 30.20ms | 23.43ms | 0.776× | 1.09% | PASS |
| file_reads | `oltp_sum_range` | 28.44ms | 22.54ms | 0.792× | 0.72% | PASS |
| file_reads | `oltp_order_range` | 4.81ms | 4.17ms | 0.868× | 0.82% | PASS |
| file_reads | `oltp_distinct_range` | 5.89ms | 5.26ms | 0.893× | 0.94% | PASS |
| file_reads | `oltp_index_scan` | 14.62ms | 9.44ms | 0.645× | 0.86% | PASS |
| file_reads | `select_random_points` | 37.93ms | 33.95ms | 0.895× | 0.93% | PASS |
| file_reads | `select_random_ranges` | 17.45ms | 11.71ms | 0.671× | 0.77% | PASS |
| file_reads | `covering_index_scan` | 14.43ms | 7.77ms | 0.539× | 0.79% | PASS |
| file_reads | `groupby_scan` | 39.88ms | 40.83ms | 1.024× | 0.59% | PASS |
| file_reads | `index_join` | 13.58ms | 12.64ms | 0.931× | 0.84% | PASS |
| file_reads | `index_join_scan` | 5.44ms | 6.12ms | 1.124× | 0.98% | PASS |
| file_reads | `types_table_scan` | 1.12s | 1.24s | 1.107× | 0.61% | PASS |
| file_reads | `table_scan` | 1.32s | 1.36s | 1.031× | 0.66% | PASS |
| file_reads | `oltp_read_only` | 291.31ms | 206.95ms | 0.710× | 0.48% | PASS |
| file_writes | `oltp_bulk_insert` | 263.97ms | 365.19ms | 1.383× | 1.01% | PASS |
| file_writes | `oltp_insert` | 26.76ms | 46.84ms | 1.750× | 1.14% | PASS |
| file_writes | `oltp_update_index` | 100.69ms | 147.42ms | 1.464× | 1.18% | PASS |
| file_writes | `oltp_update_non_index` | 80.07ms | 107.39ms | 1.341× | 1.08% | PASS |
| file_writes | `oltp_delete_insert` | 79.11ms | 122.10ms | 1.543× | 0.96% | PASS |
| file_writes | `oltp_write_only` | 52.16ms | 81.28ms | 1.558× | 1.62% | PASS |
| file_writes | `types_delete_insert` | 51.43ms | 68.01ms | 1.322× | 0.91% | PASS |
| file_writes | `oltp_read_write` | 125.92ms | 169.93ms | 1.349× | 0.83% | PASS |
| ac_reads | `oltp_point_select` | 64.97ms | 70.04ms | 1.078× | 1.07% | PASS |
| ac_reads | `oltp_range_select` | 23.51ms | 23.35ms | 0.993× | 0.85% | PASS |
| ac_reads | `oltp_sum_range` | 21.77ms | 22.58ms | 1.037× | 0.94% | PASS |
| ac_reads | `oltp_order_range` | 4.21ms | 4.17ms | 0.989× | 0.85% | PASS |
| ac_reads | `oltp_distinct_range` | 5.26ms | 5.26ms | 1.001× | 1.03% | PASS |
| ac_reads | `oltp_index_scan` | 8.12ms | 9.38ms | 1.155× | 0.73% | PASS |
| ac_reads | `select_random_points` | 31.19ms | 33.93ms | 1.088× | 0.96% | PASS |
| ac_reads | `select_random_ranges` | 10.97ms | 11.63ms | 1.061× | 0.75% | PASS |
| ac_reads | `covering_index_scan` | 7.81ms | 7.74ms | 0.991× | 0.87% | PASS |
| ac_reads | `groupby_scan` | 39.00ms | 40.81ms | 1.047× | 0.63% | PASS |
| ac_reads | `index_join` | 10.21ms | 12.62ms | 1.236× | 0.71% | PASS |
| ac_reads | `index_join_scan` | 4.78ms | 6.13ms | 1.281× | 0.94% | PASS |
| ac_reads | `types_table_scan` | 1.12s | 1.24s | 1.108× | 0.51% | PASS |
| ac_reads | `table_scan` | 1.31s | 1.36s | 1.035× | 0.50% | PASS |
| ac_reads | `oltp_read_only` | 198.44ms | 206.52ms | 1.041× | 0.93% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.13ms | 66.73ms | 3.895× | 3.84% | PASS |
| ac_writes | `oltp_insert_ac` | 19.98ms | 87.68ms | 4.389× | 3.82% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.82ms | 100.53ms | 4.607× | 4.13% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.70ms | 78.34ms | 4.426× | 3.30% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 20.04ms | 91.24ms | 4.553× | 3.51% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.34ms | 89.74ms | 4.639× | 2.76% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.87ms | 79.86ms | 4.468× | 4.86% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.65ms | 98.25ms | 3.687× | 2.82% | PASS |

</details>

## Version-control latency

Wall time: 2m 7s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 66.98ms | 200.00ms | 33.5% | 0.81% | PASS |
| `status_dirty_many_tables` | 68.77ms | 200.00ms | 34.4% | 0.38% | PASS |
| `diff_regular_working_one_table` | 62.97ms | 150.00ms | 42.0% | 0.44% | PASS |
| `diff_regular_working_many_tables` | 73.55ms | 200.00ms | 36.8% | 0.48% | PASS |
| `diff_stat_working_many_tables` | 73.45ms | 200.00ms | 36.7% | 0.59% | PASS |
| `diff_schema_working_many_tables` | 73.69ms | 200.00ms | 36.8% | 0.47% | PASS |
| `branch_list_many_branches` | 19.61ms | 100.00ms | 19.6% | 1.47% | PASS |
| `branch_create_delete` | 29.27ms | 100.00ms | 29.3% | 2.66% | PASS |
| `checkout_branch_clean` | 108.80ms | 200.00ms | 54.4% | 5.64% | PASS |
| `merge_data_no_conflicts` | 35.86ms | 150.00ms | 23.9% | 1.41% | PASS |
| `merge_schema_no_conflicts` | 19.76ms | 100.00ms | 19.8% | 1.69% | PASS |
| `merge_data_conflicts` | 100.83ms | 250.00ms | 40.3% | 0.24% | PASS |
| `merge_data_conflicts_with_resolve` | 101.11ms | 250.00ms | 40.4% | 0.35% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
