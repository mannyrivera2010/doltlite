# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-06 17:47 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261004.327.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/37493623527)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 56s | 8.96s | 11.43s | 1.276× | 1.67% | **PASS** |
| textpk | 69 | 55 | 1h 33m 16s | 10.29s | 11.90s | 1.156× | 2.19% | **PASS** |
| blobpk | 69 | 55 | 1h 9m 33s | 7.38s | 8.09s | 1.097× | 2.93% | **PASS** |
| compositepk | 69 | 55 | 1h 28m 24s | 11.01s | 12.03s | 1.092× | 1.75% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.95ms | 29.84ms | 1.196× | 1.50% | PASS |
| mem_reads | `oltp_range_select` | 10.65ms | 13.39ms | 1.257× | 1.67% | PASS |
| mem_reads | `oltp_sum_range` | 10.08ms | 12.67ms | 1.257× | 1.48% | PASS |
| mem_reads | `oltp_order_range` | 2.63ms | 3.03ms | 1.152× | 1.28% | PASS |
| mem_reads | `oltp_distinct_range` | 3.68ms | 4.08ms | 1.110× | 0.88% | PASS |
| mem_reads | `oltp_index_scan` | 4.05ms | 5.69ms | 1.405× | 2.26% | PASS |
| mem_reads | `select_random_points` | 10.57ms | 11.52ms | 1.090× | 2.20% | PASS |
| mem_reads | `select_random_ranges` | 3.10ms | 4.05ms | 1.309× | 1.29% | PASS |
| mem_reads | `covering_index_scan` | 4.27ms | 4.44ms | 1.041× | 1.76% | PASS |
| mem_reads | `groupby_scan` | 30.14ms | 33.46ms | 1.110× | 0.81% | PASS |
| mem_reads | `index_join` | 6.07ms | 8.39ms | 1.384× | 1.73% | PASS |
| mem_reads | `index_join_scan` | 3.49ms | 4.62ms | 1.324× | 1.62% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.33s | 1.280× | 0.79% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.40s | 1.187× | 1.45% | PASS |
| mem_reads | `oltp_read_only` | 101.99ms | 123.73ms | 1.213× | 1.82% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.68ms | 251.44ms | 1.392× | 1.55% | PASS |
| mem_writes | `oltp_insert` | 15.51ms | 28.43ms | 1.833× | 0.93% | PASS |
| mem_writes | `oltp_update_index` | 50.26ms | 104.55ms | 2.080× | 1.50% | PASS |
| mem_writes | `oltp_update_non_index` | 34.24ms | 59.96ms | 1.751× | 1.43% | PASS |
| mem_writes | `oltp_delete_insert` | 44.97ms | 79.02ms | 1.757× | 1.35% | PASS |
| mem_writes | `oltp_write_only` | 21.92ms | 44.88ms | 2.048× | 1.41% | PASS |
| mem_writes | `types_delete_insert` | 24.50ms | 40.21ms | 1.641× | 1.88% | PASS |
| mem_writes | `oltp_read_write` | 68.38ms | 111.00ms | 1.623× | 2.33% | PASS |
| file_reads | `oltp_point_select` | 98.37ms | 55.19ms | 0.561× | 0.93% | PASS |
| file_reads | `oltp_range_select` | 17.56ms | 15.91ms | 0.906× | 1.95% | PASS |
| file_reads | `oltp_sum_range` | 17.09ms | 15.30ms | 0.895× | 1.92% | PASS |
| file_reads | `oltp_order_range` | 3.40ms | 3.36ms | 0.989× | 2.77% | PASS |
| file_reads | `oltp_distinct_range` | 4.53ms | 4.46ms | 0.985× | 2.14% | PASS |
| file_reads | `oltp_index_scan` | 11.61ms | 8.36ms | 0.720× | 1.29% | PASS |
| file_reads | `select_random_points` | 17.90ms | 13.91ms | 0.777× | 3.02% | PASS |
| file_reads | `select_random_ranges` | 10.51ms | 6.65ms | 0.632× | 1.29% | PASS |
| file_reads | `covering_index_scan` | 12.04ms | 7.13ms | 0.592× | 1.74% | PASS |
| file_reads | `groupby_scan` | 30.71ms | 33.86ms | 1.103× | 1.48% | PASS |
| file_reads | `index_join` | 10.30ms | 10.14ms | 0.985× | 1.59% | PASS |
| file_reads | `index_join_scan` | 4.44ms | 4.94ms | 1.111× | 1.73% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.33s | 1.280× | 0.54% | PASS |
| file_reads | `table_scan` | 1.19s | 1.40s | 1.173× | 1.55% | PASS |
| file_reads | `oltp_read_only` | 212.80ms | 162.32ms | 0.763× | 1.12% | PASS |
| file_writes | `oltp_bulk_insert` | 194.00ms | 269.54ms | 1.389× | 1.19% | PASS |
| file_writes | `oltp_insert` | 22.23ms | 35.92ms | 1.616× | 2.09% | PASS |
| file_writes | `oltp_update_index` | 79.03ms | 129.49ms | 1.639× | 1.72% | PASS |
| file_writes | `oltp_update_non_index` | 58.84ms | 82.23ms | 1.397× | 1.71% | PASS |
| file_writes | `oltp_delete_insert` | 68.53ms | 100.22ms | 1.462× | 1.70% | PASS |
| file_writes | `oltp_write_only` | 45.37ms | 65.19ms | 1.437× | 1.73% | PASS |
| file_writes | `types_delete_insert` | 40.67ms | 54.10ms | 1.330× | 1.65% | PASS |
| file_writes | `oltp_read_write` | 93.52ms | 131.71ms | 1.408× | 1.55% | PASS |
| ac_reads | `oltp_point_select` | 49.10ms | 55.24ms | 1.125× | 0.92% | PASS |
| ac_reads | `oltp_range_select` | 13.45ms | 16.08ms | 1.196× | 2.11% | PASS |
| ac_reads | `oltp_sum_range` | 13.01ms | 15.48ms | 1.190× | 2.27% | PASS |
| ac_reads | `oltp_order_range` | 3.17ms | 3.46ms | 1.093× | 3.09% | PASS |
| ac_reads | `oltp_distinct_range` | 4.18ms | 4.53ms | 1.084× | 1.99% | PASS |
| ac_reads | `oltp_index_scan` | 6.92ms | 8.41ms | 1.215× | 1.67% | PASS |
| ac_reads | `select_random_points` | 13.66ms | 14.23ms | 1.042× | 3.05% | PASS |
| ac_reads | `select_random_ranges` | 5.62ms | 6.67ms | 1.186× | 1.42% | PASS |
| ac_reads | `covering_index_scan` | 7.21ms | 7.15ms | 0.991× | 2.07% | PASS |
| ac_reads | `groupby_scan` | 30.57ms | 34.10ms | 1.115× | 1.14% | PASS |
| ac_reads | `index_join` | 7.81ms | 10.14ms | 1.299× | 2.58% | PASS |
| ac_reads | `index_join_scan` | 3.97ms | 4.98ms | 1.255× | 1.67% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.34s | 1.263× | 1.14% | PASS |
| ac_reads | `table_scan` | 1.24s | 1.41s | 1.139× | 1.13% | PASS |
| ac_reads | `oltp_read_only` | 141.90ms | 163.38ms | 1.151× | 1.52% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.02ms | 67.94ms | 3.085× | 6.26% | PASS |
| ac_writes | `oltp_insert_ac` | 24.98ms | 85.12ms | 3.407× | 5.89% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.38ms | 100.42ms | 3.668× | 5.15% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.06ms | 78.06ms | 3.386× | 5.63% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.16ms | 88.83ms | 3.677× | 6.15% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.25ms | 88.44ms | 3.647× | 3.80% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.96ms | 79.33ms | 3.613× | 4.87% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.94ms | 98.31ms | 3.178× | 5.12% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.86ms | 37.84ms | 1.267× | 1.41% | PASS |
| mem_reads | `oltp_range_select` | 13.49ms | 14.05ms | 1.041× | 2.87% | PASS |
| mem_reads | `oltp_sum_range` | 11.92ms | 14.17ms | 1.189× | 1.88% | PASS |
| mem_reads | `oltp_order_range` | 2.98ms | 3.16ms | 1.061× | 1.37% | PASS |
| mem_reads | `oltp_distinct_range` | 4.07ms | 4.25ms | 1.043× | 1.83% | PASS |
| mem_reads | `oltp_index_scan` | 4.51ms | 6.37ms | 1.412× | 1.22% | PASS |
| mem_reads | `select_random_points` | 17.75ms | 20.94ms | 1.180× | 2.73% | PASS |
| mem_reads | `select_random_ranges` | 4.01ms | 5.18ms | 1.293× | 1.17% | PASS |
| mem_reads | `covering_index_scan` | 4.50ms | 4.47ms | 0.994× | 1.72% | PASS |
| mem_reads | `groupby_scan` | 31.40ms | 34.14ms | 1.087× | 0.91% | PASS |
| mem_reads | `index_join` | 6.93ms | 9.42ms | 1.359× | 3.06% | PASS |
| mem_reads | `index_join_scan` | 4.75ms | 5.41ms | 1.140× | 4.68% | PASS |
| mem_reads | `types_table_scan` | 1.08s | 1.25s | 1.153× | 2.19% | PASS |
| mem_reads | `table_scan` | 1.50s | 1.41s | 0.941× | 4.20% | PASS |
| mem_reads | `oltp_read_only` | 126.93ms | 140.16ms | 1.104× | 2.29% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.50ms | 359.43ms | 1.520× | 0.74% | PASS |
| mem_writes | `oltp_insert` | 22.50ms | 40.11ms | 1.783× | 2.17% | PASS |
| mem_writes | `oltp_update_index` | 74.57ms | 138.01ms | 1.851× | 1.94% | PASS |
| mem_writes | `oltp_update_non_index` | 52.30ms | 91.78ms | 1.755× | 1.86% | PASS |
| mem_writes | `oltp_delete_insert` | 56.62ms | 110.68ms | 1.955× | 3.99% | PASS |
| mem_writes | `oltp_write_only` | 31.90ms | 65.40ms | 2.050× | 3.26% | PASS |
| mem_writes | `types_delete_insert` | 35.19ms | 58.69ms | 1.668× | 2.41% | PASS |
| mem_writes | `oltp_read_write` | 98.94ms | 152.00ms | 1.536× | 3.12% | PASS |
| file_reads | `oltp_point_select` | 108.19ms | 66.51ms | 0.615× | 1.24% | PASS |
| file_reads | `oltp_range_select` | 23.23ms | 17.65ms | 0.760× | 3.14% | PASS |
| file_reads | `oltp_sum_range` | 22.08ms | 17.96ms | 0.814× | 2.94% | PASS |
| file_reads | `oltp_order_range` | 4.26ms | 3.72ms | 0.874× | 3.47% | PASS |
| file_reads | `oltp_distinct_range` | 5.32ms | 4.76ms | 0.894× | 1.82% | PASS |
| file_reads | `oltp_index_scan` | 12.91ms | 9.48ms | 0.734× | 2.03% | PASS |
| file_reads | `select_random_points` | 28.82ms | 25.79ms | 0.895× | 2.86% | PASS |
| file_reads | `select_random_ranges` | 12.06ms | 8.09ms | 0.671× | 1.55% | PASS |
| file_reads | `covering_index_scan` | 13.97ms | 7.69ms | 0.550× | 3.11% | PASS |
| file_reads | `groupby_scan` | 33.21ms | 35.09ms | 1.057× | 1.82% | PASS |
| file_reads | `index_join` | 11.65ms | 11.51ms | 0.988× | 3.50% | PASS |
| file_reads | `index_join_scan` | 5.54ms | 5.80ms | 1.047× | 2.32% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.22s | 1.175× | 0.64% | PASS |
| file_reads | `table_scan` | 1.53s | 1.42s | 0.926× | 4.16% | PASS |
| file_reads | `oltp_read_only` | 235.13ms | 179.40ms | 0.763× | 1.92% | PASS |
| file_writes | `oltp_bulk_insert` | 255.86ms | 386.68ms | 1.511× | 0.93% | PASS |
| file_writes | `oltp_insert` | 47.28ms | 52.26ms | 1.105× | 17.50% | PASS |
| file_writes | `oltp_update_index` | 112.00ms | 169.63ms | 1.514× | 1.23% | PASS |
| file_writes | `oltp_update_non_index` | 94.31ms | 112.30ms | 1.191× | 13.03% | PASS |
| file_writes | `oltp_delete_insert` | 89.35ms | 133.69ms | 1.496× | 1.68% | PASS |
| file_writes | `oltp_write_only` | 85.81ms | 85.40ms | 0.995× | 11.10% | PASS |
| file_writes | `types_delete_insert` | 55.23ms | 74.26ms | 1.345× | 1.57% | PASS |
| file_writes | `oltp_read_write` | 142.33ms | 164.42ms | 1.155× | 8.12% | PASS |
| ac_reads | `oltp_point_select` | 54.52ms | 64.85ms | 1.189× | 1.24% | PASS |
| ac_reads | `oltp_range_select` | 16.46ms | 17.29ms | 1.051× | 2.04% | PASS |
| ac_reads | `oltp_sum_range` | 15.10ms | 17.50ms | 1.159× | 1.85% | PASS |
| ac_reads | `oltp_order_range` | 3.39ms | 3.56ms | 1.050× | 2.17% | PASS |
| ac_reads | `oltp_distinct_range` | 4.49ms | 4.65ms | 1.036× | 2.48% | PASS |
| ac_reads | `oltp_index_scan` | 7.29ms | 9.30ms | 1.277× | 1.73% | PASS |
| ac_reads | `select_random_points` | 20.98ms | 25.12ms | 1.197× | 1.43% | PASS |
| ac_reads | `select_random_ranges` | 6.72ms | 7.94ms | 1.182× | 1.20% | PASS |
| ac_reads | `covering_index_scan` | 8.30ms | 7.46ms | 0.898× | 1.52% | PASS |
| ac_reads | `groupby_scan` | 32.42ms | 34.93ms | 1.077× | 0.91% | PASS |
| ac_reads | `index_join` | 8.97ms | 11.48ms | 1.280× | 2.43% | PASS |
| ac_reads | `index_join_scan` | 5.19ms | 5.87ms | 1.130× | 2.25% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.22s | 1.172× | 0.47% | PASS |
| ac_reads | `table_scan` | 1.27s | 1.38s | 1.087× | 3.08% | PASS |
| ac_reads | `oltp_read_only` | 156.43ms | 177.75ms | 1.136× | 1.86% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.73ms | 70.27ms | 2.962× | 5.61% | PASS |
| ac_writes | `oltp_insert_ac` | 27.39ms | 85.77ms | 3.132× | 8.98% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.77ms | 106.21ms | 3.567× | 6.40% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.86ms | 84.91ms | 3.416× | 9.26% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.22ms | 97.62ms | 3.723× | 9.64% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.66ms | 96.32ms | 3.482× | 8.94% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.71ms | 83.21ms | 3.664× | 8.19% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.72ms | 101.18ms | 3.093× | 6.16% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.01ms | 24.31ms | 1.105× | 2.93% | PASS |
| mem_reads | `oltp_range_select` | 9.54ms | 9.28ms | 0.973× | 3.40% | PASS |
| mem_reads | `oltp_sum_range` | 9.43ms | 9.16ms | 0.972× | 3.54% | PASS |
| mem_reads | `oltp_order_range` | 2.06ms | 2.02ms | 0.983× | 2.53% | PASS |
| mem_reads | `oltp_distinct_range` | 2.72ms | 2.67ms | 0.981× | 1.90% | PASS |
| mem_reads | `oltp_index_scan` | 3.27ms | 4.66ms | 1.423× | 3.01% | PASS |
| mem_reads | `select_random_points` | 14.08ms | 15.27ms | 1.085× | 3.08% | PASS |
| mem_reads | `select_random_ranges` | 3.15ms | 3.66ms | 1.159× | 2.32% | PASS |
| mem_reads | `covering_index_scan` | 2.70ms | 3.39ms | 1.255× | 4.76% | PASS |
| mem_reads | `groupby_scan` | 20.74ms | 20.12ms | 0.970× | 1.50% | PASS |
| mem_reads | `index_join` | 5.00ms | 7.97ms | 1.596× | 5.23% | PASS |
| mem_reads | `index_join_scan` | 3.71ms | 5.20ms | 1.403× | 7.70% | PASS |
| mem_reads | `types_table_scan` | 800.51ms | 808.36ms | 1.010× | 4.93% | PASS |
| mem_reads | `table_scan` | 1.08s | 938.60ms | 0.870× | 2.46% | PASS |
| mem_reads | `oltp_read_only` | 85.57ms | 85.90ms | 1.004× | 3.29% | PASS |
| mem_writes | `oltp_bulk_insert` | 144.87ms | 213.99ms | 1.477× | 1.66% | PASS |
| mem_writes | `oltp_insert` | 12.66ms | 26.27ms | 2.075× | 1.90% | PASS |
| mem_writes | `oltp_update_index` | 50.83ms | 100.50ms | 1.977× | 3.61% | PASS |
| mem_writes | `oltp_update_non_index` | 36.45ms | 63.43ms | 1.740× | 2.10% | PASS |
| mem_writes | `oltp_delete_insert` | 36.67ms | 78.40ms | 2.138× | 3.03% | PASS |
| mem_writes | `oltp_write_only` | 21.58ms | 48.87ms | 2.265× | 2.92% | PASS |
| mem_writes | `types_delete_insert` | 24.01ms | 41.26ms | 1.718× | 2.22% | PASS |
| mem_writes | `oltp_read_write` | 67.83ms | 102.14ms | 1.506× | 3.27% | PASS |
| file_reads | `oltp_point_select` | 83.31ms | 46.66ms | 0.560× | 1.30% | PASS |
| file_reads | `oltp_range_select` | 16.50ms | 11.42ms | 0.692× | 2.53% | PASS |
| file_reads | `oltp_sum_range` | 16.36ms | 11.35ms | 0.694× | 3.00% | PASS |
| file_reads | `oltp_order_range` | 2.87ms | 2.31ms | 0.807× | 1.45% | PASS |
| file_reads | `oltp_distinct_range` | 3.43ms | 2.92ms | 0.852× | 1.54% | PASS |
| file_reads | `oltp_index_scan` | 9.72ms | 6.66ms | 0.685× | 2.05% | PASS |
| file_reads | `select_random_points` | 19.80ms | 16.46ms | 0.832× | 3.36% | PASS |
| file_reads | `select_random_ranges` | 9.19ms | 5.68ms | 0.619× | 1.78% | PASS |
| file_reads | `covering_index_scan` | 10.05ms | 5.50ms | 0.547× | 2.12% | PASS |
| file_reads | `groupby_scan` | 21.13ms | 20.33ms | 0.962× | 1.50% | PASS |
| file_reads | `index_join` | 8.76ms | 8.04ms | 0.918× | 2.79% | PASS |
| file_reads | `index_join_scan` | 3.98ms | 4.50ms | 1.131× | 4.61% | PASS |
| file_reads | `types_table_scan` | 726.76ms | 778.78ms | 1.072× | 1.26% | PASS |
| file_reads | `table_scan` | 870.05ms | 883.08ms | 1.015× | 2.99% | PASS |
| file_reads | `oltp_read_only` | 164.08ms | 108.63ms | 0.662× | 2.01% | PASS |
| file_writes | `oltp_bulk_insert` | 198.43ms | 284.50ms | 1.434× | 3.23% | PASS |
| file_writes | `oltp_insert` | 34.19ms | 51.47ms | 1.506× | 12.90% | PASS |
| file_writes | `oltp_update_index` | 167.33ms | 178.91ms | 1.069× | 5.97% | PASS |
| file_writes | `oltp_update_non_index` | 123.95ms | 123.33ms | 0.995× | 3.78% | PASS |
| file_writes | `oltp_delete_insert` | 131.03ms | 139.21ms | 1.062× | 4.12% | PASS |
| file_writes | `oltp_write_only` | 99.57ms | 99.21ms | 0.996× | 2.98% | PASS |
| file_writes | `types_delete_insert` | 88.90ms | 79.85ms | 0.898× | 6.85% | PASS |
| file_writes | `oltp_read_write` | 130.56ms | 139.98ms | 1.072× | 1.90% | PASS |
| ac_reads | `oltp_point_select` | 40.68ms | 44.06ms | 1.083× | 1.77% | PASS |
| ac_reads | `oltp_range_select` | 11.40ms | 10.88ms | 0.954× | 2.76% | PASS |
| ac_reads | `oltp_sum_range` | 11.91ms | 11.03ms | 0.926× | 3.49% | PASS |
| ac_reads | `oltp_order_range` | 2.57ms | 2.43ms | 0.946× | 2.37% | PASS |
| ac_reads | `oltp_distinct_range` | 3.12ms | 2.99ms | 0.957× | 1.73% | PASS |
| ac_reads | `oltp_index_scan` | 5.75ms | 6.52ms | 1.134× | 2.14% | PASS |
| ac_reads | `select_random_points` | 15.56ms | 16.02ms | 1.029× | 2.80% | PASS |
| ac_reads | `select_random_ranges` | 5.33ms | 5.74ms | 1.077× | 1.53% | PASS |
| ac_reads | `covering_index_scan` | 6.06ms | 5.58ms | 0.920× | 2.39% | PASS |
| ac_reads | `groupby_scan` | 21.30ms | 20.59ms | 0.966× | 1.25% | PASS |
| ac_reads | `index_join` | 6.96ms | 8.33ms | 1.196× | 2.51% | PASS |
| ac_reads | `index_join_scan` | 3.75ms | 4.62ms | 1.234× | 2.98% | PASS |
| ac_reads | `types_table_scan` | 716.13ms | 775.79ms | 1.083× | 1.77% | PASS |
| ac_reads | `table_scan` | 835.85ms | 879.48ms | 1.052× | 2.55% | PASS |
| ac_reads | `oltp_read_only` | 104.97ms | 108.34ms | 1.032× | 3.21% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.33ms | 56.60ms | 2.654× | 4.68% | PASS |
| ac_writes | `oltp_insert_ac` | 24.39ms | 72.11ms | 2.957× | 4.34% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.49ms | 80.95ms | 3.055× | 4.65% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.20ms | 63.36ms | 2.989× | 3.81% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.89ms | 71.63ms | 3.129× | 3.59% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.21ms | 72.91ms | 3.141× | 4.60% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.98ms | 66.75ms | 3.182× | 6.07% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.14ms | 77.26ms | 2.955× | 4.43% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.32ms | 37.92ms | 1.105× | 2.73% | PASS |
| mem_reads | `oltp_range_select` | 21.08ms | 20.14ms | 0.956× | 2.54% | PASS |
| mem_reads | `oltp_sum_range` | 19.23ms | 19.25ms | 1.001× | 1.75% | PASS |
| mem_reads | `oltp_order_range` | 3.90ms | 3.80ms | 0.974× | 1.02% | PASS |
| mem_reads | `oltp_distinct_range` | 4.90ms | 4.86ms | 0.991× | 1.00% | PASS |
| mem_reads | `oltp_index_scan` | 4.83ms | 6.02ms | 1.248× | 1.91% | PASS |
| mem_reads | `select_random_points` | 28.73ms | 31.02ms | 1.080× | 2.46% | PASS |
| mem_reads | `select_random_ranges` | 7.92ms | 8.41ms | 1.061× | 1.17% | PASS |
| mem_reads | `covering_index_scan` | 4.47ms | 4.63ms | 1.036× | 2.60% | PASS |
| mem_reads | `groupby_scan` | 39.39ms | 40.01ms | 1.016× | 1.03% | PASS |
| mem_reads | `index_join` | 8.40ms | 11.15ms | 1.327× | 3.13% | PASS |
| mem_reads | `index_join_scan` | 4.53ms | 5.87ms | 1.297× | 3.08% | PASS |
| mem_reads | `types_table_scan` | 1.23s | 1.29s | 1.051× | 1.37% | PASS |
| mem_reads | `table_scan` | 1.53s | 1.41s | 0.925× | 0.88% | PASS |
| mem_reads | `oltp_read_only` | 163.59ms | 163.94ms | 1.002× | 1.09% | PASS |
| mem_writes | `oltp_bulk_insert` | 245.29ms | 340.57ms | 1.388× | 0.90% | PASS |
| mem_writes | `oltp_insert` | 19.69ms | 36.51ms | 1.854× | 0.76% | PASS |
| mem_writes | `oltp_update_index` | 74.16ms | 128.38ms | 1.731× | 1.51% | PASS |
| mem_writes | `oltp_update_non_index` | 54.96ms | 87.15ms | 1.586× | 1.43% | PASS |
| mem_writes | `oltp_delete_insert` | 52.00ms | 99.41ms | 1.912× | 1.58% | PASS |
| mem_writes | `oltp_write_only` | 28.43ms | 60.51ms | 2.128× | 1.46% | PASS |
| mem_writes | `types_delete_insert` | 34.15ms | 55.23ms | 1.617× | 1.42% | PASS |
| mem_writes | `oltp_read_write` | 109.16ms | 152.89ms | 1.401× | 1.25% | PASS |
| file_reads | `oltp_point_select` | 131.92ms | 70.65ms | 0.536× | 1.10% | PASS |
| file_reads | `oltp_range_select` | 30.24ms | 23.46ms | 0.776× | 1.86% | PASS |
| file_reads | `oltp_sum_range` | 28.73ms | 22.72ms | 0.791× | 1.58% | PASS |
| file_reads | `oltp_order_range` | 4.77ms | 4.18ms | 0.876× | 2.86% | PASS |
| file_reads | `oltp_distinct_range` | 5.91ms | 5.25ms | 0.887× | 1.39% | PASS |
| file_reads | `oltp_index_scan` | 14.59ms | 9.33ms | 0.639× | 1.84% | PASS |
| file_reads | `select_random_points` | 37.58ms | 33.91ms | 0.902× | 2.35% | PASS |
| file_reads | `select_random_ranges` | 17.42ms | 11.73ms | 0.673× | 1.64% | PASS |
| file_reads | `covering_index_scan` | 14.32ms | 7.70ms | 0.538× | 2.30% | PASS |
| file_reads | `groupby_scan` | 39.70ms | 40.44ms | 1.019× | 0.78% | PASS |
| file_reads | `index_join` | 13.45ms | 12.52ms | 0.930× | 1.97% | PASS |
| file_reads | `index_join_scan` | 5.40ms | 6.07ms | 1.124× | 2.03% | PASS |
| file_reads | `types_table_scan` | 1.12s | 1.24s | 1.111× | 0.76% | PASS |
| file_reads | `table_scan` | 1.29s | 1.35s | 1.046× | 0.64% | PASS |
| file_reads | `oltp_read_only` | 294.10ms | 206.55ms | 0.702× | 1.16% | PASS |
| file_writes | `oltp_bulk_insert` | 262.20ms | 367.48ms | 1.402× | 1.24% | PASS |
| file_writes | `oltp_insert` | 27.43ms | 47.87ms | 1.745× | 1.93% | PASS |
| file_writes | `oltp_update_index` | 108.09ms | 156.00ms | 1.443× | 1.87% | PASS |
| file_writes | `oltp_update_non_index` | 84.54ms | 110.78ms | 1.310× | 1.98% | PASS |
| file_writes | `oltp_delete_insert` | 81.68ms | 124.72ms | 1.527× | 1.56% | PASS |
| file_writes | `oltp_write_only` | 54.67ms | 83.84ms | 1.534× | 2.06% | PASS |
| file_writes | `types_delete_insert` | 53.41ms | 70.24ms | 1.315× | 1.82% | PASS |
| file_writes | `oltp_read_write` | 137.31ms | 177.74ms | 1.294× | 2.10% | PASS |
| ac_reads | `oltp_point_select` | 67.19ms | 71.67ms | 1.067× | 0.98% | PASS |
| ac_reads | `oltp_range_select` | 26.09ms | 24.30ms | 0.931× | 1.67% | PASS |
| ac_reads | `oltp_sum_range` | 23.99ms | 23.46ms | 0.978× | 2.78% | PASS |
| ac_reads | `oltp_order_range` | 4.41ms | 4.21ms | 0.954× | 1.85% | PASS |
| ac_reads | `oltp_distinct_range` | 5.39ms | 5.29ms | 0.981× | 1.34% | PASS |
| ac_reads | `oltp_index_scan` | 8.69ms | 9.77ms | 1.124× | 2.72% | PASS |
| ac_reads | `select_random_points` | 34.20ms | 36.34ms | 1.063× | 3.74% | PASS |
| ac_reads | `select_random_ranges` | 11.46ms | 11.97ms | 1.044× | 1.40% | PASS |
| ac_reads | `covering_index_scan` | 8.20ms | 8.02ms | 0.978× | 1.38% | PASS |
| ac_reads | `groupby_scan` | 39.64ms | 40.90ms | 1.032× | 0.66% | PASS |
| ac_reads | `index_join` | 10.82ms | 13.33ms | 1.231× | 3.78% | PASS |
| ac_reads | `index_join_scan` | 5.10ms | 6.71ms | 1.317× | 3.76% | PASS |
| ac_reads | `types_table_scan` | 1.26s | 1.30s | 1.032× | 0.94% | PASS |
| ac_reads | `table_scan` | 1.56s | 1.43s | 0.919× | 0.98% | PASS |
| ac_reads | `oltp_read_only` | 216.53ms | 215.70ms | 0.996× | 0.78% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.40ms | 60.80ms | 3.494× | 11.23% | PASS |
| ac_writes | `oltp_insert_ac` | 20.07ms | 84.94ms | 4.232× | 10.34% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.36ms | 95.02ms | 4.447× | 6.78% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.38ms | 65.81ms | 4.017× | 5.67% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.25ms | 76.86ms | 4.210× | 4.85% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.32ms | 76.05ms | 4.151× | 5.42% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.68ms | 68.44ms | 4.104× | 5.52% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.68ms | 86.68ms | 3.375× | 4.06% | PASS |

</details>

## Version-control latency

Wall time: 2m 25s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 85.27ms | 200.00ms | 42.6% | 0.61% | PASS |
| `status_dirty_many_tables` | 88.88ms | 200.00ms | 44.4% | 0.59% | PASS |
| `diff_regular_working_one_table` | 80.86ms | 150.00ms | 53.9% | 0.57% | PASS |
| `diff_regular_working_many_tables` | 94.52ms | 200.00ms | 47.3% | 0.55% | PASS |
| `diff_stat_working_many_tables` | 94.61ms | 200.00ms | 47.3% | 0.66% | PASS |
| `diff_schema_working_many_tables` | 94.09ms | 200.00ms | 47.0% | 0.75% | PASS |
| `branch_list_many_branches` | 24.08ms | 100.00ms | 24.1% | 1.43% | PASS |
| `branch_create_delete` | 25.71ms | 100.00ms | 25.7% | 1.51% | PASS |
| `checkout_branch_clean` | 56.98ms | 200.00ms | 28.5% | 1.06% | PASS |
| `merge_data_no_conflicts` | 30.73ms | 150.00ms | 20.5% | 1.11% | PASS |
| `merge_schema_no_conflicts` | 22.62ms | 100.00ms | 22.6% | 1.38% | PASS |
| `merge_data_conflicts` | 128.36ms | 250.00ms | 51.3% | 0.35% | PASS |
| `merge_data_conflicts_with_resolve` | 128.51ms | 250.00ms | 51.4% | 0.44% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
