# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-26 11:46 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260823.283.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/32956566317)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 11m 53s | 9.31s | 11.03s | 1.185× | 0.98% | **PASS** |
| textpk | 69 | 55 | 1h 36m 7s | 11.09s | 12.40s | 1.118× | 2.46% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 39s | 9.80s | 11.87s | 1.210× | 1.72% | **PASS** |
| compositepk | 69 | 55 | 1h 26m 27s | 9.67s | 12.03s | 1.244× | 1.31% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.42ms | 27.39ms | 1.122× | 1.10% | PASS |
| mem_reads | `oltp_range_select` | 10.38ms | 11.89ms | 1.145× | 1.22% | PASS |
| mem_reads | `oltp_sum_range` | 9.59ms | 11.35ms | 1.184× | 0.91% | PASS |
| mem_reads | `oltp_order_range` | 2.63ms | 2.83ms | 1.077× | 0.98% | PASS |
| mem_reads | `oltp_distinct_range` | 3.77ms | 3.88ms | 1.031× | 0.80% | PASS |
| mem_reads | `oltp_index_scan` | 4.00ms | 4.97ms | 1.243× | 1.81% | PASS |
| mem_reads | `select_random_points` | 10.40ms | 10.98ms | 1.056× | 1.95% | PASS |
| mem_reads | `select_random_ranges` | 3.06ms | 3.93ms | 1.287× | 1.05% | PASS |
| mem_reads | `covering_index_scan` | 4.37ms | 4.04ms | 0.924× | 1.15% | PASS |
| mem_reads | `groupby_scan` | 31.66ms | 34.34ms | 1.085× | 0.49% | PASS |
| mem_reads | `index_join` | 5.90ms | 7.73ms | 1.310× | 0.86% | PASS |
| mem_reads | `index_join_scan` | 3.53ms | 4.71ms | 1.335× | 1.75% | PASS |
| mem_reads | `types_table_scan` | 1.11s | 1.26s | 1.137× | 0.71% | PASS |
| mem_reads | `table_scan` | 1.25s | 1.37s | 1.093× | 0.69% | PASS |
| mem_reads | `oltp_read_only` | 102.94ms | 114.12ms | 1.109× | 0.84% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.09ms | 240.33ms | 1.342× | 0.61% | PASS |
| mem_writes | `oltp_insert` | 15.77ms | 28.11ms | 1.783× | 0.73% | PASS |
| mem_writes | `oltp_update_index` | 50.41ms | 104.97ms | 2.082× | 0.93% | PASS |
| mem_writes | `oltp_update_non_index` | 34.51ms | 58.17ms | 1.686× | 0.80% | PASS |
| mem_writes | `oltp_delete_insert` | 44.05ms | 77.61ms | 1.762× | 0.64% | PASS |
| mem_writes | `oltp_write_only` | 21.59ms | 44.83ms | 2.076× | 0.92% | PASS |
| mem_writes | `types_delete_insert` | 24.55ms | 39.68ms | 1.617× | 1.36% | PASS |
| mem_writes | `oltp_read_write` | 64.68ms | 104.81ms | 1.620× | 0.93% | PASS |
| file_reads | `oltp_point_select` | 120.13ms | 59.05ms | 0.492× | 0.57% | PASS |
| file_reads | `oltp_range_select` | 20.40ms | 15.02ms | 0.736× | 1.21% | PASS |
| file_reads | `oltp_sum_range` | 19.41ms | 14.64ms | 0.754× | 1.16% | PASS |
| file_reads | `oltp_order_range` | 3.73ms | 3.22ms | 0.864× | 0.73% | PASS |
| file_reads | `oltp_distinct_range` | 4.82ms | 4.21ms | 0.874× | 0.78% | PASS |
| file_reads | `oltp_index_scan` | 13.81ms | 8.27ms | 0.599× | 0.98% | PASS |
| file_reads | `select_random_points` | 20.54ms | 14.33ms | 0.698× | 0.55% | PASS |
| file_reads | `select_random_ranges` | 12.70ms | 7.13ms | 0.561× | 0.71% | PASS |
| file_reads | `covering_index_scan` | 14.30ms | 7.38ms | 0.516× | 0.80% | PASS |
| file_reads | `groupby_scan` | 32.83ms | 34.81ms | 1.060× | 0.71% | PASS |
| file_reads | `index_join` | 11.32ms | 9.84ms | 0.869× | 0.84% | PASS |
| file_reads | `index_join_scan` | 4.59ms | 5.06ms | 1.103× | 1.08% | PASS |
| file_reads | `types_table_scan` | 1.10s | 1.26s | 1.144× | 0.56% | PASS |
| file_reads | `table_scan` | 1.25s | 1.37s | 1.096× | 0.46% | PASS |
| file_reads | `oltp_read_only` | 239.60ms | 160.15ms | 0.668× | 0.59% | PASS |
| file_writes | `oltp_bulk_insert` | 194.34ms | 259.54ms | 1.335× | 1.00% | PASS |
| file_writes | `oltp_insert` | 22.25ms | 36.32ms | 1.633× | 1.14% | PASS |
| file_writes | `oltp_update_index` | 81.27ms | 132.52ms | 1.631× | 0.90% | PASS |
| file_writes | `oltp_update_non_index` | 61.21ms | 83.06ms | 1.357× | 1.71% | PASS |
| file_writes | `oltp_delete_insert` | 69.54ms | 100.29ms | 1.442× | 1.45% | PASS |
| file_writes | `oltp_write_only` | 45.40ms | 66.68ms | 1.469× | 1.63% | PASS |
| file_writes | `types_delete_insert` | 41.80ms | 54.59ms | 1.306× | 1.37% | PASS |
| file_writes | `oltp_read_write` | 91.94ms | 128.27ms | 1.395× | 1.33% | PASS |
| ac_reads | `oltp_point_select` | 55.67ms | 59.10ms | 1.062× | 0.63% | PASS |
| ac_reads | `oltp_range_select` | 14.29ms | 15.20ms | 1.063× | 1.35% | PASS |
| ac_reads | `oltp_sum_range` | 13.26ms | 14.84ms | 1.119× | 0.89% | PASS |
| ac_reads | `oltp_order_range` | 3.13ms | 3.25ms | 1.035× | 1.34% | PASS |
| ac_reads | `oltp_distinct_range` | 4.18ms | 4.27ms | 1.020× | 1.14% | PASS |
| ac_reads | `oltp_index_scan` | 7.53ms | 8.54ms | 1.134× | 1.09% | PASS |
| ac_reads | `select_random_points` | 14.78ms | 14.83ms | 1.003× | 2.43% | PASS |
| ac_reads | `select_random_ranges` | 6.41ms | 7.21ms | 1.125× | 0.88% | PASS |
| ac_reads | `covering_index_scan` | 7.81ms | 7.62ms | 0.975× | 1.47% | PASS |
| ac_reads | `groupby_scan` | 32.28ms | 34.94ms | 1.083× | 0.64% | PASS |
| ac_reads | `index_join` | 7.86ms | 10.04ms | 1.278× | 1.65% | PASS |
| ac_reads | `index_join_scan` | 4.00ms | 5.06ms | 1.266× | 1.54% | PASS |
| ac_reads | `types_table_scan` | 1.10s | 1.26s | 1.146× | 0.56% | PASS |
| ac_reads | `table_scan` | 1.25s | 1.37s | 1.097× | 0.50% | PASS |
| ac_reads | `oltp_read_only` | 149.03ms | 160.41ms | 1.076× | 0.85% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.53ms | 62.76ms | 4.042× | 3.18% | PASS |
| ac_writes | `oltp_insert_ac` | 17.86ms | 80.39ms | 4.500× | 3.30% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.52ms | 95.86ms | 4.912× | 2.09% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.80ms | 73.93ms | 4.400× | 4.01% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.27ms | 85.95ms | 4.705× | 4.09% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.51ms | 84.27ms | 4.553× | 4.51% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.12ms | 75.55ms | 4.687× | 4.24% | PASS |
| ac_writes | `oltp_read_write_ac` | 23.71ms | 92.27ms | 3.892× | 4.63% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 28.93ms | 36.92ms | 1.276× | 1.08% | PASS |
| mem_reads | `oltp_range_select` | 12.46ms | 13.95ms | 1.120× | 1.68% | PASS |
| mem_reads | `oltp_sum_range` | 11.35ms | 14.01ms | 1.234× | 1.37% | PASS |
| mem_reads | `oltp_order_range` | 2.81ms | 3.13ms | 1.114× | 1.06% | PASS |
| mem_reads | `oltp_distinct_range` | 3.88ms | 4.20ms | 1.083× | 1.01% | PASS |
| mem_reads | `oltp_index_scan` | 4.30ms | 6.04ms | 1.406× | 1.54% | PASS |
| mem_reads | `select_random_points` | 17.73ms | 20.80ms | 1.173× | 1.96% | PASS |
| mem_reads | `select_random_ranges` | 4.00ms | 5.18ms | 1.294× | 2.18% | PASS |
| mem_reads | `covering_index_scan` | 4.53ms | 4.45ms | 0.982× | 1.49% | PASS |
| mem_reads | `groupby_scan` | 31.55ms | 33.69ms | 1.068× | 0.76% | PASS |
| mem_reads | `index_join` | 6.83ms | 9.02ms | 1.321× | 1.86% | PASS |
| mem_reads | `index_join_scan` | 4.50ms | 5.30ms | 1.177× | 1.96% | PASS |
| mem_reads | `types_table_scan` | 1.13s | 1.25s | 1.106× | 4.30% | PASS |
| mem_reads | `table_scan` | 1.43s | 1.40s | 0.983× | 2.76% | PASS |
| mem_reads | `oltp_read_only` | 130.89ms | 141.78ms | 1.083× | 1.87% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.34ms | 363.53ms | 1.545× | 1.17% | PASS |
| mem_writes | `oltp_insert` | 23.56ms | 41.77ms | 1.773× | 1.83% | PASS |
| mem_writes | `oltp_update_index` | 79.08ms | 142.75ms | 1.805× | 1.74% | PASS |
| mem_writes | `oltp_update_non_index` | 51.09ms | 90.60ms | 1.773× | 1.48% | PASS |
| mem_writes | `oltp_delete_insert` | 53.83ms | 108.35ms | 2.013× | 1.06% | PASS |
| mem_writes | `oltp_write_only` | 30.91ms | 64.73ms | 2.094× | 1.53% | PASS |
| mem_writes | `types_delete_insert` | 34.24ms | 57.75ms | 1.687× | 1.39% | PASS |
| mem_writes | `oltp_read_write` | 102.70ms | 155.49ms | 1.514× | 3.93% | PASS |
| file_reads | `oltp_point_select` | 109.44ms | 66.54ms | 0.608× | 1.78% | PASS |
| file_reads | `oltp_range_select` | 24.11ms | 17.69ms | 0.734× | 2.41% | PASS |
| file_reads | `oltp_sum_range` | 22.04ms | 17.72ms | 0.804× | 3.11% | PASS |
| file_reads | `oltp_order_range` | 4.04ms | 3.58ms | 0.887× | 2.44% | PASS |
| file_reads | `oltp_distinct_range` | 5.13ms | 4.69ms | 0.914× | 2.60% | PASS |
| file_reads | `oltp_index_scan` | 12.93ms | 9.45ms | 0.731× | 1.62% | PASS |
| file_reads | `select_random_points` | 28.49ms | 25.31ms | 0.888× | 2.46% | PASS |
| file_reads | `select_random_ranges` | 11.92ms | 7.97ms | 0.668× | 1.71% | PASS |
| file_reads | `covering_index_scan` | 13.88ms | 7.66ms | 0.552× | 3.13% | PASS |
| file_reads | `groupby_scan` | 33.84ms | 34.70ms | 1.025× | 1.19% | PASS |
| file_reads | `index_join` | 12.55ms | 11.64ms | 0.928× | 2.97% | PASS |
| file_reads | `index_join_scan` | 6.01ms | 6.11ms | 1.016× | 4.74% | PASS |
| file_reads | `types_table_scan` | 1.25s | 1.27s | 1.011× | 2.65% | PASS |
| file_reads | `table_scan` | 1.51s | 1.41s | 0.931× | 3.43% | PASS |
| file_reads | `oltp_read_only` | 250.46ms | 186.05ms | 0.743× | 3.16% | PASS |
| file_writes | `oltp_bulk_insert` | 257.16ms | 395.53ms | 1.538× | 1.56% | PASS |
| file_writes | `oltp_insert` | 51.38ms | 54.29ms | 1.057× | 22.23% | PASS |
| file_writes | `oltp_update_index` | 132.59ms | 184.15ms | 1.389× | 4.59% | PASS |
| file_writes | `oltp_update_non_index` | 111.41ms | 119.89ms | 1.076× | 10.92% | PASS |
| file_writes | `oltp_delete_insert` | 100.14ms | 143.39ms | 1.432× | 2.05% | PASS |
| file_writes | `oltp_write_only` | 78.96ms | 90.91ms | 1.151× | 12.91% | PASS |
| file_writes | `types_delete_insert` | 60.35ms | 78.98ms | 1.309× | 2.68% | PASS |
| file_writes | `oltp_read_write` | 163.91ms | 176.87ms | 1.079× | 7.50% | PASS |
| ac_reads | `oltp_point_select` | 57.25ms | 65.49ms | 1.144× | 1.77% | PASS |
| ac_reads | `oltp_range_select` | 18.23ms | 17.39ms | 0.954× | 3.09% | PASS |
| ac_reads | `oltp_sum_range` | 16.76ms | 17.58ms | 1.049× | 2.77% | PASS |
| ac_reads | `oltp_order_range` | 3.61ms | 3.61ms | 0.999× | 3.12% | PASS |
| ac_reads | `oltp_distinct_range` | 4.66ms | 4.71ms | 1.011× | 2.68% | PASS |
| ac_reads | `oltp_index_scan` | 7.67ms | 9.37ms | 1.221× | 1.67% | PASS |
| ac_reads | `select_random_points` | 21.91ms | 24.96ms | 1.139× | 1.67% | PASS |
| ac_reads | `select_random_ranges` | 6.98ms | 7.98ms | 1.143× | 1.99% | PASS |
| ac_reads | `covering_index_scan` | 8.70ms | 7.54ms | 0.867× | 3.16% | PASS |
| ac_reads | `groupby_scan` | 33.36ms | 34.70ms | 1.040× | 1.35% | PASS |
| ac_reads | `index_join` | 10.54ms | 12.13ms | 1.151× | 4.61% | PASS |
| ac_reads | `index_join_scan` | 5.78ms | 6.38ms | 1.104× | 6.53% | PASS |
| ac_reads | `types_table_scan` | 1.23s | 1.26s | 1.030× | 2.66% | PASS |
| ac_reads | `table_scan` | 1.56s | 1.43s | 0.911× | 1.40% | PASS |
| ac_reads | `oltp_read_only` | 161.06ms | 178.27ms | 1.107× | 4.48% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 27.96ms | 104.40ms | 3.734× | 10.20% | PASS |
| ac_writes | `oltp_insert_ac` | 32.04ms | 123.96ms | 3.869× | 6.72% | PASS |
| ac_writes | `oltp_update_index_ac` | 34.43ms | 145.48ms | 4.226× | 8.41% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 28.34ms | 120.55ms | 4.254× | 6.10% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 31.11ms | 131.35ms | 4.223× | 7.11% | PASS |
| ac_writes | `oltp_write_only_ac` | 30.14ms | 130.41ms | 4.328× | 7.56% | PASS |
| ac_writes | `types_delete_insert_ac` | 28.36ms | 122.69ms | 4.326× | 7.90% | PASS |
| ac_writes | `oltp_read_write_ac` | 38.78ms | 144.99ms | 3.739× | 8.08% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.83ms | 36.52ms | 1.224× | 2.16% | PASS |
| mem_reads | `oltp_range_select` | 12.67ms | 13.77ms | 1.087× | 2.07% | PASS |
| mem_reads | `oltp_sum_range` | 11.69ms | 13.82ms | 1.182× | 1.32% | PASS |
| mem_reads | `oltp_order_range` | 2.92ms | 3.12ms | 1.069× | 1.04% | PASS |
| mem_reads | `oltp_distinct_range` | 4.00ms | 4.20ms | 1.050× | 0.71% | PASS |
| mem_reads | `oltp_index_scan` | 4.50ms | 6.24ms | 1.386× | 1.83% | PASS |
| mem_reads | `select_random_points` | 17.59ms | 20.59ms | 1.171× | 1.99% | PASS |
| mem_reads | `select_random_ranges` | 3.98ms | 5.10ms | 1.280× | 1.27% | PASS |
| mem_reads | `covering_index_scan` | 4.40ms | 4.59ms | 1.042× | 2.37% | PASS |
| mem_reads | `groupby_scan` | 31.70ms | 33.61ms | 1.060× | 0.98% | PASS |
| mem_reads | `index_join` | 6.79ms | 9.34ms | 1.377× | 2.86% | PASS |
| mem_reads | `index_join_scan` | 4.12ms | 5.26ms | 1.277× | 1.70% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.24s | 1.152× | 1.26% | PASS |
| mem_reads | `table_scan` | 1.27s | 1.38s | 1.087× | 1.76% | PASS |
| mem_reads | `oltp_read_only` | 119.95ms | 136.01ms | 1.134× | 1.87% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.30ms | 349.53ms | 1.479× | 1.01% | PASS |
| mem_writes | `oltp_insert` | 19.70ms | 39.56ms | 2.009× | 0.97% | PASS |
| mem_writes | `oltp_update_index` | 68.35ms | 130.56ms | 1.910× | 1.75% | PASS |
| mem_writes | `oltp_update_non_index` | 47.78ms | 84.44ms | 1.767× | 1.47% | PASS |
| mem_writes | `oltp_delete_insert` | 48.69ms | 103.34ms | 2.122× | 1.28% | PASS |
| mem_writes | `oltp_write_only` | 27.59ms | 62.09ms | 2.251× | 1.51% | PASS |
| mem_writes | `types_delete_insert` | 32.03ms | 54.35ms | 1.697× | 1.72% | PASS |
| mem_writes | `oltp_read_write` | 84.32ms | 139.50ms | 1.654× | 1.94% | PASS |
| file_reads | `oltp_point_select` | 105.13ms | 63.60ms | 0.605× | 0.74% | PASS |
| file_reads | `oltp_range_select` | 20.95ms | 16.71ms | 0.798× | 2.05% | PASS |
| file_reads | `oltp_sum_range` | 20.16ms | 16.83ms | 0.835× | 1.41% | PASS |
| file_reads | `oltp_order_range` | 3.83ms | 3.49ms | 0.913× | 2.49% | PASS |
| file_reads | `oltp_distinct_range` | 4.95ms | 4.58ms | 0.925× | 1.52% | PASS |
| file_reads | `oltp_index_scan` | 12.34ms | 9.03ms | 0.732× | 1.35% | PASS |
| file_reads | `select_random_points` | 26.93ms | 24.24ms | 0.900× | 1.85% | PASS |
| file_reads | `select_random_ranges` | 11.58ms | 7.75ms | 0.670× | 1.11% | PASS |
| file_reads | `covering_index_scan` | 12.53ms | 7.31ms | 0.583× | 2.23% | PASS |
| file_reads | `groupby_scan` | 32.79ms | 34.22ms | 1.044× | 1.12% | PASS |
| file_reads | `index_join` | 11.59ms | 11.16ms | 0.963× | 2.09% | PASS |
| file_reads | `index_join_scan` | 5.21ms | 5.77ms | 1.106× | 1.75% | PASS |
| file_reads | `types_table_scan` | 1.08s | 1.24s | 1.141× | 1.13% | PASS |
| file_reads | `table_scan` | 1.30s | 1.38s | 1.059× | 1.52% | PASS |
| file_reads | `oltp_read_only` | 234.23ms | 176.31ms | 0.753× | 1.05% | PASS |
| file_writes | `oltp_bulk_insert` | 255.30ms | 375.95ms | 1.473× | 0.89% | PASS |
| file_writes | `oltp_insert` | 32.05ms | 52.54ms | 1.639× | 3.91% | PASS |
| file_writes | `oltp_update_index` | 105.49ms | 165.55ms | 1.569× | 1.84% | PASS |
| file_writes | `oltp_update_non_index` | 79.69ms | 109.03ms | 1.368× | 2.11% | PASS |
| file_writes | `oltp_delete_insert` | 83.00ms | 131.92ms | 1.590× | 1.99% | PASS |
| file_writes | `oltp_write_only` | 56.90ms | 85.03ms | 1.494× | 2.23% | PASS |
| file_writes | `types_delete_insert` | 52.95ms | 73.07ms | 1.380× | 1.70% | PASS |
| file_writes | `oltp_read_write` | 118.08ms | 162.79ms | 1.379× | 2.25% | PASS |
| ac_reads | `oltp_point_select` | 54.78ms | 63.49ms | 1.159× | 1.32% | PASS |
| ac_reads | `oltp_range_select` | 16.09ms | 16.77ms | 1.042× | 1.69% | PASS |
| ac_reads | `oltp_sum_range` | 15.21ms | 16.72ms | 1.099× | 1.76% | PASS |
| ac_reads | `oltp_order_range` | 3.36ms | 3.49ms | 1.038× | 1.50% | PASS |
| ac_reads | `oltp_distinct_range` | 4.43ms | 4.56ms | 1.029× | 1.48% | PASS |
| ac_reads | `oltp_index_scan` | 7.45ms | 9.11ms | 1.223× | 1.53% | PASS |
| ac_reads | `select_random_points` | 21.40ms | 24.19ms | 1.130× | 1.81% | PASS |
| ac_reads | `select_random_ranges` | 6.72ms | 7.78ms | 1.157× | 1.22% | PASS |
| ac_reads | `covering_index_scan` | 7.73ms | 7.32ms | 0.946× | 1.08% | PASS |
| ac_reads | `groupby_scan` | 32.21ms | 34.29ms | 1.064× | 0.84% | PASS |
| ac_reads | `index_join` | 9.02ms | 11.20ms | 1.241× | 1.62% | PASS |
| ac_reads | `index_join_scan` | 4.67ms | 5.81ms | 1.244× | 1.86% | PASS |
| ac_reads | `types_table_scan` | 1.08s | 1.24s | 1.140× | 1.22% | PASS |
| ac_reads | `table_scan` | 1.33s | 1.39s | 1.043× | 1.51% | PASS |
| ac_reads | `oltp_read_only` | 160.34ms | 176.15ms | 1.099× | 1.81% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.62ms | 83.58ms | 3.395× | 6.68% | PASS |
| ac_writes | `oltp_insert_ac` | 25.14ms | 106.26ms | 4.227× | 4.71% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.36ms | 116.83ms | 3.980× | 7.01% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.88ms | 96.41ms | 3.876× | 8.40% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.32ms | 107.76ms | 4.255× | 5.49% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.21ms | 106.72ms | 4.233× | 4.81% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.41ms | 99.39ms | 4.435× | 9.19% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.43ms | 114.78ms | 3.652× | 4.23% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.88ms | 40.17ms | 1.222× | 1.28% | PASS |
| mem_reads | `oltp_range_select` | 19.36ms | 21.18ms | 1.094× | 1.76% | PASS |
| mem_reads | `oltp_sum_range` | 18.20ms | 20.94ms | 1.151× | 1.07% | PASS |
| mem_reads | `oltp_order_range` | 3.56ms | 3.85ms | 1.081× | 0.92% | PASS |
| mem_reads | `oltp_distinct_range` | 4.70ms | 4.96ms | 1.055× | 1.05% | PASS |
| mem_reads | `oltp_index_scan` | 4.62ms | 6.28ms | 1.357× | 1.66% | PASS |
| mem_reads | `select_random_points` | 28.28ms | 32.34ms | 1.143× | 1.67% | PASS |
| mem_reads | `select_random_ranges` | 7.79ms | 9.05ms | 1.162× | 1.37% | PASS |
| mem_reads | `covering_index_scan` | 4.23ms | 4.48ms | 1.060× | 2.19% | PASS |
| mem_reads | `groupby_scan` | 36.52ms | 38.88ms | 1.065× | 1.11% | PASS |
| mem_reads | `index_join` | 8.24ms | 10.63ms | 1.290× | 1.90% | PASS |
| mem_reads | `index_join_scan` | 4.22ms | 5.49ms | 1.302× | 1.50% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.25s | 1.173× | 0.73% | PASS |
| mem_reads | `table_scan` | 1.25s | 1.40s | 1.124× | 1.11% | PASS |
| mem_reads | `oltp_read_only` | 150.63ms | 169.05ms | 1.122× | 1.34% | PASS |
| mem_writes | `oltp_bulk_insert` | 244.33ms | 352.49ms | 1.443× | 0.74% | PASS |
| mem_writes | `oltp_insert` | 19.05ms | 36.43ms | 1.913× | 0.79% | PASS |
| mem_writes | `oltp_update_index` | 67.74ms | 116.68ms | 1.722× | 1.15% | PASS |
| mem_writes | `oltp_update_non_index` | 50.38ms | 82.39ms | 1.635× | 1.00% | PASS |
| mem_writes | `oltp_delete_insert` | 49.65ms | 96.17ms | 1.937× | 0.91% | PASS |
| mem_writes | `oltp_write_only` | 26.92ms | 57.61ms | 2.140× | 1.31% | PASS |
| mem_writes | `types_delete_insert` | 32.25ms | 55.03ms | 1.707× | 1.41% | PASS |
| mem_writes | `oltp_read_write` | 102.03ms | 155.83ms | 1.527× | 1.71% | PASS |
| file_reads | `oltp_point_select` | 107.15ms | 65.96ms | 0.616× | 0.88% | PASS |
| file_reads | `oltp_range_select` | 27.09ms | 24.27ms | 0.896× | 1.56% | PASS |
| file_reads | `oltp_sum_range` | 26.17ms | 23.96ms | 0.916× | 1.26% | PASS |
| file_reads | `oltp_order_range` | 4.48ms | 4.26ms | 0.951× | 1.69% | PASS |
| file_reads | `oltp_distinct_range` | 5.65ms | 5.35ms | 0.947× | 0.91% | PASS |
| file_reads | `oltp_index_scan` | 12.43ms | 9.19ms | 0.739× | 1.60% | PASS |
| file_reads | `select_random_points` | 37.98ms | 36.33ms | 0.957× | 1.51% | PASS |
| file_reads | `select_random_ranges` | 15.67ms | 11.99ms | 0.765× | 1.27% | PASS |
| file_reads | `covering_index_scan` | 11.98ms | 7.18ms | 0.599× | 1.33% | PASS |
| file_reads | `groupby_scan` | 37.52ms | 39.45ms | 1.052× | 1.11% | PASS |
| file_reads | `index_join` | 12.69ms | 12.85ms | 1.013× | 1.22% | PASS |
| file_reads | `index_join_scan` | 5.21ms | 5.91ms | 1.134× | 1.78% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.23s | 1.187× | 0.47% | PASS |
| file_reads | `table_scan` | 1.22s | 1.40s | 1.147× | 1.09% | PASS |
| file_reads | `oltp_read_only` | 264.89ms | 210.61ms | 0.795× | 0.82% | PASS |
| file_writes | `oltp_bulk_insert` | 258.71ms | 374.75ms | 1.449× | 1.22% | PASS |
| file_writes | `oltp_insert` | 25.95ms | 46.08ms | 1.776× | 1.68% | PASS |
| file_writes | `oltp_update_index` | 97.19ms | 142.40ms | 1.465× | 1.14% | PASS |
| file_writes | `oltp_update_non_index` | 76.10ms | 103.69ms | 1.362× | 1.49% | PASS |
| file_writes | `oltp_delete_insert` | 77.06ms | 119.20ms | 1.547× | 1.30% | PASS |
| file_writes | `oltp_write_only` | 51.38ms | 77.28ms | 1.504× | 1.98% | PASS |
| file_writes | `types_delete_insert` | 49.16ms | 68.58ms | 1.395× | 1.14% | PASS |
| file_writes | `oltp_read_write` | 128.30ms | 175.86ms | 1.371× | 1.33% | PASS |
| ac_reads | `oltp_point_select` | 57.36ms | 65.98ms | 1.150× | 1.09% | PASS |
| ac_reads | `oltp_range_select` | 22.17ms | 24.21ms | 1.092× | 1.36% | PASS |
| ac_reads | `oltp_sum_range` | 20.89ms | 24.02ms | 1.150× | 1.31% | PASS |
| ac_reads | `oltp_order_range` | 3.97ms | 4.26ms | 1.074× | 1.37% | PASS |
| ac_reads | `oltp_distinct_range` | 5.08ms | 5.35ms | 1.054× | 1.52% | PASS |
| ac_reads | `oltp_index_scan` | 7.37ms | 9.13ms | 1.238× | 1.73% | PASS |
| ac_reads | `select_random_points` | 31.79ms | 36.33ms | 1.143× | 1.60% | PASS |
| ac_reads | `select_random_ranges` | 10.48ms | 11.99ms | 1.144× | 0.94% | PASS |
| ac_reads | `covering_index_scan` | 6.94ms | 7.15ms | 1.030× | 1.06% | PASS |
| ac_reads | `groupby_scan` | 36.83ms | 39.43ms | 1.071× | 0.82% | PASS |
| ac_reads | `index_join` | 9.94ms | 12.82ms | 1.291× | 1.39% | PASS |
| ac_reads | `index_join_scan` | 4.69ms | 5.94ms | 1.267× | 1.22% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.23s | 1.186× | 0.85% | PASS |
| ac_reads | `table_scan` | 1.21s | 1.40s | 1.151× | 0.93% | PASS |
| ac_reads | `oltp_read_only` | 189.59ms | 210.23ms | 1.109× | 1.13% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.01ms | 78.00ms | 3.713× | 4.90% | PASS |
| ac_writes | `oltp_insert_ac` | 23.96ms | 98.38ms | 4.107× | 4.95% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.32ms | 111.27ms | 4.395× | 4.70% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.10ms | 89.75ms | 4.060× | 5.63% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.11ms | 101.86ms | 4.407× | 3.65% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.48ms | 99.94ms | 4.257× | 3.85% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.57ms | 88.94ms | 4.324× | 4.60% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.65ms | 106.84ms | 3.485× | 3.91% | PASS |

</details>

## Version-control latency

Wall time: 2m 19s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 82.85ms | 200.00ms | 41.4% | 0.42% | PASS |
| `status_dirty_many_tables` | 86.07ms | 200.00ms | 43.0% | 0.37% | PASS |
| `diff_regular_working_one_table` | 78.72ms | 150.00ms | 52.5% | 0.33% | PASS |
| `diff_regular_working_many_tables` | 91.57ms | 200.00ms | 45.8% | 0.31% | PASS |
| `diff_stat_working_many_tables` | 91.54ms | 200.00ms | 45.8% | 0.40% | PASS |
| `diff_schema_working_many_tables` | 92.08ms | 200.00ms | 46.0% | 0.42% | PASS |
| `branch_list_many_branches` | 22.57ms | 100.00ms | 22.6% | 1.09% | PASS |
| `branch_create_delete` | 24.96ms | 100.00ms | 25.0% | 0.90% | PASS |
| `checkout_branch_clean` | 55.21ms | 200.00ms | 27.6% | 0.66% | PASS |
| `merge_data_no_conflicts` | 28.87ms | 150.00ms | 19.2% | 1.01% | PASS |
| `merge_schema_no_conflicts` | 22.62ms | 100.00ms | 22.6% | 1.32% | PASS |
| `merge_data_conflicts` | 126.77ms | 250.00ms | 50.7% | 0.30% | PASS |
| `merge_data_conflicts_with_resolve` | 126.97ms | 250.00ms | 50.8% | 0.30% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
