# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-03 15:32 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260823.283.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/33763093958)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 17s | 9.02s | 11.57s | 1.282× | 1.52% | **PASS** |
| textpk | 69 | 55 | 1h 40m 44s | 11.48s | 16.22s | 1.412× | 2.23% | **PASS** |
| blobpk | 69 | 55 | 1h 41m 41s | 10.50s | 12.28s | 1.169× | 1.87% | **PASS** |
| compositepk | 69 | 55 | 1h 16m 29s | 8.45s | 9.64s | 1.142× | 0.94% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.19ms | 29.72ms | 1.281× | 1.81% | PASS |
| mem_reads | `oltp_range_select` | 9.54ms | 13.13ms | 1.376× | 1.90% | PASS |
| mem_reads | `oltp_sum_range` | 9.06ms | 12.26ms | 1.353× | 1.20% | PASS |
| mem_reads | `oltp_order_range` | 2.44ms | 2.96ms | 1.213× | 1.57% | PASS |
| mem_reads | `oltp_distinct_range` | 3.56ms | 4.00ms | 1.125× | 1.22% | PASS |
| mem_reads | `oltp_index_scan` | 3.74ms | 5.20ms | 1.392× | 0.96% | PASS |
| mem_reads | `select_random_points` | 9.12ms | 10.97ms | 1.202× | 1.32% | PASS |
| mem_reads | `select_random_ranges` | 2.76ms | 3.93ms | 1.424× | 1.95% | PASS |
| mem_reads | `covering_index_scan` | 4.23ms | 4.16ms | 0.982× | 1.09% | PASS |
| mem_reads | `groupby_scan` | 29.47ms | 32.60ms | 1.106× | 0.94% | PASS |
| mem_reads | `index_join` | 6.05ms | 8.43ms | 1.393× | 2.34% | PASS |
| mem_reads | `index_join_scan` | 3.51ms | 4.64ms | 1.322× | 1.81% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.35s | 1.261× | 1.46% | PASS |
| mem_reads | `table_scan` | 1.19s | 1.40s | 1.181× | 1.28% | PASS |
| mem_reads | `oltp_read_only` | 103.58ms | 124.89ms | 1.206× | 1.64% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.69ms | 254.40ms | 1.400× | 1.28% | PASS |
| mem_writes | `oltp_insert` | 15.44ms | 28.57ms | 1.851× | 1.03% | PASS |
| mem_writes | `oltp_update_index` | 50.48ms | 105.81ms | 2.096× | 1.26% | PASS |
| mem_writes | `oltp_update_non_index` | 34.34ms | 59.94ms | 1.746× | 1.62% | PASS |
| mem_writes | `oltp_delete_insert` | 45.13ms | 79.92ms | 1.771× | 1.19% | PASS |
| mem_writes | `oltp_write_only` | 21.91ms | 45.46ms | 2.074× | 1.65% | PASS |
| mem_writes | `types_delete_insert` | 24.68ms | 40.68ms | 1.648× | 1.11% | PASS |
| mem_writes | `oltp_read_write` | 69.65ms | 112.70ms | 1.618× | 2.15% | PASS |
| file_reads | `oltp_point_select` | 99.31ms | 55.50ms | 0.559× | 0.93% | PASS |
| file_reads | `oltp_range_select` | 18.25ms | 16.08ms | 0.881× | 1.99% | PASS |
| file_reads | `oltp_sum_range` | 17.64ms | 15.22ms | 0.863× | 1.60% | PASS |
| file_reads | `oltp_order_range` | 3.58ms | 3.52ms | 0.984× | 3.07% | PASS |
| file_reads | `oltp_distinct_range` | 4.94ms | 4.78ms | 0.967× | 2.23% | PASS |
| file_reads | `oltp_index_scan` | 11.81ms | 8.45ms | 0.715× | 1.63% | PASS |
| file_reads | `select_random_points` | 18.26ms | 14.15ms | 0.775× | 1.31% | PASS |
| file_reads | `select_random_ranges` | 10.64ms | 6.65ms | 0.625× | 1.12% | PASS |
| file_reads | `covering_index_scan` | 12.37ms | 7.20ms | 0.582× | 1.31% | PASS |
| file_reads | `groupby_scan` | 31.54ms | 33.76ms | 1.070× | 1.06% | PASS |
| file_reads | `index_join` | 10.52ms | 10.36ms | 0.985× | 1.03% | PASS |
| file_reads | `index_join_scan` | 4.55ms | 5.08ms | 1.115× | 1.88% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.34s | 1.272× | 0.73% | PASS |
| file_reads | `table_scan` | 1.21s | 1.40s | 1.163× | 1.06% | PASS |
| file_reads | `oltp_read_only` | 215.24ms | 163.20ms | 0.758× | 0.81% | PASS |
| file_writes | `oltp_bulk_insert` | 195.02ms | 273.89ms | 1.404× | 1.05% | PASS |
| file_writes | `oltp_insert` | 22.38ms | 36.15ms | 1.615× | 1.90% | PASS |
| file_writes | `oltp_update_index` | 78.95ms | 132.22ms | 1.675× | 2.21% | PASS |
| file_writes | `oltp_update_non_index` | 58.88ms | 82.97ms | 1.409× | 1.81% | PASS |
| file_writes | `oltp_delete_insert` | 69.35ms | 101.72ms | 1.467× | 1.90% | PASS |
| file_writes | `oltp_write_only` | 45.59ms | 65.93ms | 1.446× | 1.70% | PASS |
| file_writes | `types_delete_insert` | 40.94ms | 54.51ms | 1.331× | 1.67% | PASS |
| file_writes | `oltp_read_write` | 95.47ms | 133.47ms | 1.398× | 1.70% | PASS |
| ac_reads | `oltp_point_select` | 49.14ms | 55.88ms | 1.137× | 1.31% | PASS |
| ac_reads | `oltp_range_select` | 13.30ms | 16.00ms | 1.202× | 1.52% | PASS |
| ac_reads | `oltp_sum_range` | 12.58ms | 15.29ms | 1.216× | 1.50% | PASS |
| ac_reads | `oltp_order_range` | 3.13ms | 3.44ms | 1.099× | 2.20% | PASS |
| ac_reads | `oltp_distinct_range` | 4.19ms | 4.49ms | 1.071× | 1.51% | PASS |
| ac_reads | `oltp_index_scan` | 6.88ms | 8.41ms | 1.224× | 1.37% | PASS |
| ac_reads | `select_random_points` | 13.30ms | 14.06ms | 1.057× | 2.08% | PASS |
| ac_reads | `select_random_ranges` | 5.66ms | 6.61ms | 1.168× | 0.97% | PASS |
| ac_reads | `covering_index_scan` | 7.18ms | 7.13ms | 0.993× | 1.15% | PASS |
| ac_reads | `groupby_scan` | 30.39ms | 33.27ms | 1.095× | 0.79% | PASS |
| ac_reads | `index_join` | 7.83ms | 10.35ms | 1.322× | 1.11% | PASS |
| ac_reads | `index_join_scan` | 3.94ms | 4.99ms | 1.265× | 1.71% | PASS |
| ac_reads | `types_table_scan` | 1.07s | 1.34s | 1.255× | 1.47% | PASS |
| ac_reads | `table_scan` | 1.23s | 1.41s | 1.148× | 1.28% | PASS |
| ac_reads | `oltp_read_only` | 143.27ms | 163.98ms | 1.145× | 1.39% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.71ms | 80.11ms | 3.690× | 4.92% | PASS |
| ac_writes | `oltp_insert_ac` | 24.76ms | 95.57ms | 3.860× | 7.05% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.30ms | 109.28ms | 4.155× | 5.91% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.29ms | 89.35ms | 4.008× | 9.34% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.02ms | 103.55ms | 4.140× | 8.37% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.13ms | 101.58ms | 4.042× | 5.43% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.08ms | 89.60ms | 4.250× | 4.23% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.16ms | 107.31ms | 3.681× | 5.14% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.17ms | 25.85ms | 1.166× | 1.93% | PASS |
| mem_reads | `oltp_range_select` | 11.91ms | 10.89ms | 0.914× | 1.95% | PASS |
| mem_reads | `oltp_sum_range` | 11.49ms | 10.56ms | 0.919× | 2.49% | PASS |
| mem_reads | `oltp_order_range` | 2.51ms | 2.48ms | 0.990× | 1.53% | PASS |
| mem_reads | `oltp_distinct_range` | 3.21ms | 3.20ms | 0.997× | 2.45% | PASS |
| mem_reads | `oltp_index_scan` | 3.65ms | 4.70ms | 1.287× | 1.96% | PASS |
| mem_reads | `select_random_points` | 15.94ms | 18.28ms | 1.147× | 1.53% | PASS |
| mem_reads | `select_random_ranges` | 3.25ms | 3.98ms | 1.226× | 1.49% | PASS |
| mem_reads | `covering_index_scan` | 3.33ms | 3.32ms | 0.999× | 2.23% | PASS |
| mem_reads | `groupby_scan` | 24.30ms | 25.53ms | 1.050× | 1.33% | PASS |
| mem_reads | `index_join` | 6.07ms | 7.94ms | 1.309× | 2.37% | PASS |
| mem_reads | `index_join_scan` | 4.28ms | 5.29ms | 1.236× | 2.44% | PASS |
| mem_reads | `types_table_scan` | 880.89ms | 970.97ms | 1.102× | 2.08% | PASS |
| mem_reads | `table_scan` | 1.02s | 1.07s | 1.047× | 4.49% | PASS |
| mem_reads | `oltp_read_only` | 93.23ms | 98.27ms | 1.054× | 2.35% | PASS |
| mem_writes | `oltp_bulk_insert` | 150.12ms | 228.46ms | 1.522× | 1.19% | PASS |
| mem_writes | `oltp_insert` | 16.18ms | 27.96ms | 1.729× | 1.21% | PASS |
| mem_writes | `oltp_update_index` | 55.47ms | 101.99ms | 1.839× | 2.18% | PASS |
| mem_writes | `oltp_update_non_index` | 37.14ms | 64.79ms | 1.744× | 1.50% | PASS |
| mem_writes | `oltp_delete_insert` | 40.32ms | 78.71ms | 1.952× | 2.29% | PASS |
| mem_writes | `oltp_write_only` | 22.66ms | 45.70ms | 2.017× | 2.06% | PASS |
| mem_writes | `types_delete_insert` | 24.15ms | 40.70ms | 1.685× | 1.33% | PASS |
| mem_writes | `oltp_read_write` | 66.60ms | 103.84ms | 1.559× | 2.59% | PASS |
| file_reads | `oltp_point_select` | 49.31ms | 35.74ms | 0.725× | 1.51% | PASS |
| file_reads | `oltp_range_select` | 15.24ms | 12.17ms | 0.799× | 1.30% | PASS |
| file_reads | `oltp_sum_range` | 15.04ms | 11.94ms | 0.794× | 1.47% | PASS |
| file_reads | `oltp_order_range` | 2.96ms | 2.71ms | 0.913× | 2.13% | PASS |
| file_reads | `oltp_distinct_range` | 3.71ms | 3.42ms | 0.921× | 1.97% | PASS |
| file_reads | `oltp_index_scan` | 6.83ms | 6.08ms | 0.890× | 1.96% | PASS |
| file_reads | `select_random_points` | 19.02ms | 19.36ms | 1.018× | 2.04% | PASS |
| file_reads | `select_random_ranges` | 5.94ms | 4.94ms | 0.832× | 2.27% | PASS |
| file_reads | `covering_index_scan` | 7.15ms | 4.60ms | 0.643× | 2.54% | PASS |
| file_reads | `groupby_scan` | 24.10ms | 25.23ms | 1.047× | 1.58% | PASS |
| file_reads | `index_join` | 8.32ms | 9.12ms | 1.097× | 2.77% | PASS |
| file_reads | `index_join_scan` | 4.89ms | 5.68ms | 1.162× | 1.40% | PASS |
| file_reads | `types_table_scan` | 853.32ms | 952.17ms | 1.116× | 1.97% | PASS |
| file_reads | `table_scan` | 995.51ms | 1.03s | 1.037× | 4.36% | PASS |
| file_reads | `oltp_read_only` | 134.09ms | 114.15ms | 0.851× | 1.65% | PASS |
| file_writes | `oltp_bulk_insert` | 351.82ms | 385.74ms | 1.096× | 31.59% | PASS |
| file_writes | `oltp_insert` | 152.19ms | 66.55ms | 0.437× | 63.79% | PASS |
| file_writes | `oltp_update_index` | 249.68ms | 206.59ms | 0.827× | 22.37% | PASS |
| file_writes | `oltp_update_non_index` | 258.16ms | 150.65ms | 0.584× | 44.97% | PASS |
| file_writes | `oltp_delete_insert` | 240.98ms | 169.14ms | 0.702× | 33.78% | PASS |
| file_writes | `oltp_write_only` | 244.34ms | 120.51ms | 0.493× | 34.87% | PASS |
| file_writes | `types_delete_insert` | 124.63ms | 95.92ms | 0.770× | 30.31% | PASS |
| file_writes | `oltp_read_write` | 247.76ms | 169.00ms | 0.682× | 41.55% | PASS |
| ac_reads | `oltp_point_select` | 30.33ms | 35.22ms | 1.161× | 2.09% | PASS |
| ac_reads | `oltp_range_select` | 13.37ms | 12.27ms | 0.918× | 1.30% | PASS |
| ac_reads | `oltp_sum_range` | 13.02ms | 11.85ms | 0.911× | 2.35% | PASS |
| ac_reads | `oltp_order_range` | 2.75ms | 2.67ms | 0.972× | 2.48% | PASS |
| ac_reads | `oltp_distinct_range` | 3.47ms | 3.39ms | 0.977× | 1.88% | PASS |
| ac_reads | `oltp_index_scan` | 5.26ms | 6.05ms | 1.150× | 2.25% | PASS |
| ac_reads | `select_random_points` | 17.89ms | 19.69ms | 1.100× | 1.95% | PASS |
| ac_reads | `select_random_ranges` | 4.46ms | 4.92ms | 1.104× | 1.85% | PASS |
| ac_reads | `covering_index_scan` | 5.49ms | 4.62ms | 0.842× | 2.43% | PASS |
| ac_reads | `groupby_scan` | 24.19ms | 25.54ms | 1.056× | 1.10% | PASS |
| ac_reads | `index_join` | 7.79ms | 9.20ms | 1.180× | 2.22% | PASS |
| ac_reads | `index_join_scan` | 4.71ms | 5.53ms | 1.175× | 2.43% | PASS |
| ac_reads | `types_table_scan` | 855.61ms | 942.80ms | 1.102× | 1.55% | PASS |
| ac_reads | `table_scan` | 976.60ms | 1.02s | 1.043× | 3.84% | PASS |
| ac_reads | `oltp_read_only` | 98.69ms | 105.66ms | 1.071× | 1.58% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 268.41ms | 654.02ms | 2.437× | 62.89% | PASS |
| ac_writes | `oltp_insert_ac` | 259.34ms | 904.48ms | 3.488× | 56.55% | PASS |
| ac_writes | `oltp_update_index_ac` | 209.52ms | 608.68ms | 2.905× | 60.30% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 385.15ms | 861.78ms | 2.238× | 57.64% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 422.37ms | 951.97ms | 2.254× | 59.67% | PASS |
| ac_writes | `oltp_write_only_ac` | 412.76ms | 1.19s | 2.895× | 61.82% | PASS |
| ac_writes | `types_delete_insert_ac` | 430.23ms | 1.09s | 2.543× | 49.87% | PASS |
| ac_writes | `oltp_read_write_ac` | 496.11ms | 1.18s | 2.381× | 45.67% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.69ms | 28.65ms | 1.115× | 2.06% | PASS |
| mem_reads | `oltp_range_select` | 12.78ms | 11.27ms | 0.882× | 3.48% | PASS |
| mem_reads | `oltp_sum_range` | 11.14ms | 10.87ms | 0.975× | 2.94% | PASS |
| mem_reads | `oltp_order_range` | 2.59ms | 2.56ms | 0.989× | 1.87% | PASS |
| mem_reads | `oltp_distinct_range` | 3.39ms | 3.36ms | 0.990× | 1.31% | PASS |
| mem_reads | `oltp_index_scan` | 3.94ms | 5.09ms | 1.290× | 2.09% | PASS |
| mem_reads | `select_random_points` | 15.95ms | 17.30ms | 1.085× | 3.62% | PASS |
| mem_reads | `select_random_ranges` | 3.68ms | 4.31ms | 1.170× | 2.00% | PASS |
| mem_reads | `covering_index_scan` | 4.04ms | 3.99ms | 0.987× | 4.14% | PASS |
| mem_reads | `groupby_scan` | 27.50ms | 27.96ms | 1.017× | 0.74% | PASS |
| mem_reads | `index_join` | 6.12ms | 8.33ms | 1.360× | 4.63% | PASS |
| mem_reads | `index_join_scan` | 3.90ms | 5.06ms | 1.299× | 1.98% | PASS |
| mem_reads | `types_table_scan` | 1.00s | 1.04s | 1.036× | 2.06% | PASS |
| mem_reads | `table_scan` | 1.25s | 1.13s | 0.903× | 2.08% | PASS |
| mem_reads | `oltp_read_only` | 105.20ms | 105.18ms | 1.000× | 2.05% | PASS |
| mem_writes | `oltp_bulk_insert` | 183.11ms | 257.57ms | 1.407× | 1.16% | PASS |
| mem_writes | `oltp_insert` | 16.32ms | 30.49ms | 1.869× | 1.22% | PASS |
| mem_writes | `oltp_update_index` | 59.21ms | 108.02ms | 1.824× | 2.59% | PASS |
| mem_writes | `oltp_update_non_index` | 41.10ms | 66.87ms | 1.627× | 1.93% | PASS |
| mem_writes | `oltp_delete_insert` | 41.42ms | 82.94ms | 2.003× | 2.40% | PASS |
| mem_writes | `oltp_write_only` | 23.98ms | 49.82ms | 2.077× | 2.13% | PASS |
| mem_writes | `types_delete_insert` | 26.79ms | 41.50ms | 1.549× | 1.56% | PASS |
| mem_writes | `oltp_read_write` | 71.30ms | 107.42ms | 1.507× | 2.76% | PASS |
| file_reads | `oltp_point_select` | 99.05ms | 52.38ms | 0.529× | 1.02% | PASS |
| file_reads | `oltp_range_select` | 19.25ms | 13.35ms | 0.693× | 1.45% | PASS |
| file_reads | `oltp_sum_range` | 18.10ms | 13.01ms | 0.719× | 1.21% | PASS |
| file_reads | `oltp_order_range` | 3.38ms | 2.82ms | 0.834× | 0.84% | PASS |
| file_reads | `oltp_distinct_range` | 4.20ms | 3.61ms | 0.860× | 0.96% | PASS |
| file_reads | `oltp_index_scan` | 11.64ms | 7.53ms | 0.647× | 0.77% | PASS |
| file_reads | `select_random_points` | 22.15ms | 18.55ms | 0.837× | 1.36% | PASS |
| file_reads | `select_random_ranges` | 11.14ms | 6.74ms | 0.605× | 0.92% | PASS |
| file_reads | `covering_index_scan` | 12.14ms | 6.36ms | 0.524× | 0.61% | PASS |
| file_reads | `groupby_scan` | 27.46ms | 27.86ms | 1.015× | 0.99% | PASS |
| file_reads | `index_join` | 10.32ms | 9.20ms | 0.891× | 1.15% | PASS |
| file_reads | `index_join_scan` | 4.59ms | 5.07ms | 1.103× | 1.22% | PASS |
| file_reads | `types_table_scan` | 879.57ms | 968.08ms | 1.101× | 1.46% | PASS |
| file_reads | `table_scan` | 1.04s | 1.07s | 1.033× | 2.52% | PASS |
| file_reads | `oltp_read_only` | 200.93ms | 135.03ms | 0.672× | 1.11% | PASS |
| file_writes | `oltp_bulk_insert` | 335.09ms | 426.38ms | 1.272× | 26.16% | PASS |
| file_writes | `oltp_insert` | 222.45ms | 98.15ms | 0.441× | 62.99% | PASS |
| file_writes | `oltp_update_index` | 225.40ms | 221.98ms | 0.985× | 20.74% | PASS |
| file_writes | `oltp_update_non_index` | 230.94ms | 161.14ms | 0.698× | 36.90% | PASS |
| file_writes | `oltp_delete_insert` | 267.09ms | 197.99ms | 0.741× | 31.25% | PASS |
| file_writes | `oltp_write_only` | 212.69ms | 153.53ms | 0.722× | 43.99% | PASS |
| file_writes | `types_delete_insert` | 164.94ms | 105.56ms | 0.640× | 39.99% | PASS |
| file_writes | `oltp_read_write` | 335.35ms | 204.51ms | 0.610× | 33.14% | PASS |
| ac_reads | `oltp_point_select` | 48.54ms | 52.09ms | 1.073× | 1.08% | PASS |
| ac_reads | `oltp_range_select` | 14.36ms | 13.37ms | 0.931× | 1.36% | PASS |
| ac_reads | `oltp_sum_range` | 13.25ms | 13.04ms | 0.984× | 0.80% | PASS |
| ac_reads | `oltp_order_range` | 2.95ms | 2.83ms | 0.960× | 0.96% | PASS |
| ac_reads | `oltp_distinct_range` | 3.70ms | 3.61ms | 0.976× | 0.75% | PASS |
| ac_reads | `oltp_index_scan` | 6.70ms | 7.54ms | 1.126× | 1.00% | PASS |
| ac_reads | `select_random_points` | 17.13ms | 18.46ms | 1.078× | 0.75% | PASS |
| ac_reads | `select_random_ranges` | 6.18ms | 6.71ms | 1.086× | 0.62% | PASS |
| ac_reads | `covering_index_scan` | 7.10ms | 6.35ms | 0.894× | 0.87% | PASS |
| ac_reads | `groupby_scan` | 26.93ms | 27.77ms | 1.031× | 0.63% | PASS |
| ac_reads | `index_join` | 7.74ms | 9.15ms | 1.183× | 0.74% | PASS |
| ac_reads | `index_join_scan` | 4.16ms | 5.08ms | 1.222× | 0.94% | PASS |
| ac_reads | `types_table_scan` | 866.13ms | 961.24ms | 1.110× | 0.48% | PASS |
| ac_reads | `table_scan` | 993.86ms | 1.05s | 1.055× | 0.55% | PASS |
| ac_reads | `oltp_read_only` | 127.63ms | 134.43ms | 1.053× | 0.61% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 90.47ms | 250.36ms | 2.767× | 77.76% | PASS |
| ac_writes | `oltp_insert_ac` | 150.69ms | 353.44ms | 2.346× | 67.68% | PASS |
| ac_writes | `oltp_update_index_ac` | 188.16ms | 472.40ms | 2.511× | 78.66% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 58.95ms | 251.38ms | 4.264× | 55.27% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 125.28ms | 396.17ms | 3.162× | 70.15% | PASS |
| ac_writes | `oltp_write_only_ac` | 197.15ms | 433.87ms | 2.201× | 52.51% | PASS |
| ac_writes | `types_delete_insert_ac` | 125.11ms | 346.19ms | 2.767× | 69.92% | PASS |
| ac_writes | `oltp_read_write_ac` | 130.85ms | 415.72ms | 3.177× | 42.94% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.41ms | 29.66ms | 1.123× | 1.38% | PASS |
| mem_reads | `oltp_range_select` | 15.69ms | 15.65ms | 0.998× | 1.12% | PASS |
| mem_reads | `oltp_sum_range` | 14.46ms | 14.95ms | 1.034× | 0.78% | PASS |
| mem_reads | `oltp_order_range` | 2.97ms | 2.94ms | 0.991× | 1.05% | PASS |
| mem_reads | `oltp_distinct_range` | 3.81ms | 3.76ms | 0.988× | 0.83% | PASS |
| mem_reads | `oltp_index_scan` | 3.75ms | 4.53ms | 1.208× | 1.03% | PASS |
| mem_reads | `select_random_points` | 21.87ms | 24.11ms | 1.102× | 1.17% | PASS |
| mem_reads | `select_random_ranges` | 5.98ms | 6.49ms | 1.085× | 0.83% | PASS |
| mem_reads | `covering_index_scan` | 3.40ms | 3.19ms | 0.940× | 1.75% | PASS |
| mem_reads | `groupby_scan` | 30.29ms | 30.96ms | 1.022× | 0.60% | PASS |
| mem_reads | `index_join` | 6.51ms | 8.02ms | 1.231× | 1.16% | PASS |
| mem_reads | `index_join_scan` | 3.52ms | 4.82ms | 1.369× | 0.94% | PASS |
| mem_reads | `types_table_scan` | 874.44ms | 970.21ms | 1.110× | 0.41% | PASS |
| mem_reads | `table_scan` | 1.00s | 1.06s | 1.056× | 0.44% | PASS |
| mem_reads | `oltp_read_only` | 117.51ms | 123.70ms | 1.053× | 0.68% | PASS |
| mem_writes | `oltp_bulk_insert` | 191.03ms | 257.77ms | 1.349× | 0.52% | PASS |
| mem_writes | `oltp_insert` | 15.23ms | 27.13ms | 1.781× | 0.67% | PASS |
| mem_writes | `oltp_update_index` | 53.66ms | 89.56ms | 1.669× | 0.73% | PASS |
| mem_writes | `oltp_update_non_index` | 40.76ms | 63.65ms | 1.562× | 0.87% | PASS |
| mem_writes | `oltp_delete_insert` | 39.53ms | 72.58ms | 1.836× | 0.81% | PASS |
| mem_writes | `oltp_write_only` | 21.82ms | 44.23ms | 2.027× | 0.74% | PASS |
| mem_writes | `types_delete_insert` | 25.85ms | 40.37ms | 1.562× | 1.27% | PASS |
| mem_writes | `oltp_read_write` | 77.47ms | 113.32ms | 1.463× | 0.61% | PASS |
| file_reads | `oltp_point_select` | 100.92ms | 55.07ms | 0.546× | 0.91% | PASS |
| file_reads | `oltp_range_select` | 23.48ms | 18.30ms | 0.779× | 1.23% | PASS |
| file_reads | `oltp_sum_range` | 22.10ms | 17.74ms | 0.803× | 1.38% | PASS |
| file_reads | `oltp_order_range` | 3.84ms | 3.27ms | 0.853× | 1.90% | PASS |
| file_reads | `oltp_distinct_range` | 4.62ms | 4.11ms | 0.891× | 1.71% | PASS |
| file_reads | `oltp_index_scan` | 11.45ms | 7.62ms | 0.665× | 1.17% | PASS |
| file_reads | `select_random_points` | 29.59ms | 26.94ms | 0.911× | 1.51% | PASS |
| file_reads | `select_random_ranges` | 13.66ms | 9.29ms | 0.680× | 1.17% | PASS |
| file_reads | `covering_index_scan` | 11.20ms | 6.24ms | 0.557× | 0.77% | PASS |
| file_reads | `groupby_scan` | 31.20ms | 31.43ms | 1.007× | 0.63% | PASS |
| file_reads | `index_join` | 10.71ms | 10.22ms | 0.955× | 1.04% | PASS |
| file_reads | `index_join_scan` | 4.33ms | 5.18ms | 1.195× | 1.39% | PASS |
| file_reads | `types_table_scan` | 867.46ms | 967.40ms | 1.115× | 0.48% | PASS |
| file_reads | `table_scan` | 997.68ms | 1.05s | 1.055× | 0.36% | PASS |
| file_reads | `oltp_read_only` | 225.91ms | 159.94ms | 0.708× | 0.46% | PASS |
| file_writes | `oltp_bulk_insert` | 242.29ms | 326.20ms | 1.346× | 2.60% | PASS |
| file_writes | `oltp_insert` | 34.42ms | 49.41ms | 1.436× | 3.82% | PASS |
| file_writes | `oltp_update_index` | 163.30ms | 167.47ms | 1.026× | 5.67% | PASS |
| file_writes | `oltp_update_non_index` | 136.66ms | 123.35ms | 0.903× | 1.09% | PASS |
| file_writes | `oltp_delete_insert` | 133.12ms | 140.15ms | 1.053× | 1.95% | PASS |
| file_writes | `oltp_write_only` | 97.33ms | 99.79ms | 1.025× | 3.59% | PASS |
| file_writes | `types_delete_insert` | 86.31ms | 81.76ms | 0.947× | 6.71% | PASS |
| file_writes | `oltp_read_write` | 152.80ms | 171.75ms | 1.124× | 3.89% | PASS |
| ac_reads | `oltp_point_select` | 50.93ms | 55.34ms | 1.087× | 0.79% | PASS |
| ac_reads | `oltp_range_select` | 18.81ms | 18.29ms | 0.972× | 0.83% | PASS |
| ac_reads | `oltp_sum_range` | 17.38ms | 17.71ms | 1.019× | 0.77% | PASS |
| ac_reads | `oltp_order_range` | 3.41ms | 3.29ms | 0.963× | 0.79% | PASS |
| ac_reads | `oltp_distinct_range` | 4.21ms | 4.13ms | 0.981× | 0.79% | PASS |
| ac_reads | `oltp_index_scan` | 6.66ms | 7.62ms | 1.143× | 0.63% | PASS |
| ac_reads | `select_random_points` | 24.90ms | 27.06ms | 1.087× | 0.90% | PASS |
| ac_reads | `select_random_ranges` | 8.84ms | 9.28ms | 1.050× | 0.85% | PASS |
| ac_reads | `covering_index_scan` | 6.30ms | 6.22ms | 0.988× | 0.75% | PASS |
| ac_reads | `groupby_scan` | 30.67ms | 31.45ms | 1.025× | 0.62% | PASS |
| ac_reads | `index_join` | 8.27ms | 10.26ms | 1.240× | 0.89% | PASS |
| ac_reads | `index_join_scan` | 3.90ms | 5.08ms | 1.303× | 1.07% | PASS |
| ac_reads | `types_table_scan` | 866.07ms | 966.54ms | 1.116× | 0.54% | PASS |
| ac_reads | `table_scan` | 998.15ms | 1.05s | 1.054× | 0.40% | PASS |
| ac_reads | `oltp_read_only` | 153.41ms | 160.18ms | 1.044× | 0.48% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 28.38ms | 75.39ms | 2.657× | 3.63% | PASS |
| ac_writes | `oltp_insert_ac` | 30.61ms | 92.49ms | 3.021× | 5.32% | PASS |
| ac_writes | `oltp_update_index_ac` | 32.42ms | 100.12ms | 3.089× | 2.86% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 28.76ms | 84.97ms | 2.954× | 4.58% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 30.37ms | 92.21ms | 3.036× | 4.74% | PASS |
| ac_writes | `oltp_write_only_ac` | 30.46ms | 94.19ms | 3.092× | 4.37% | PASS |
| ac_writes | `types_delete_insert_ac` | 28.23ms | 86.01ms | 3.046× | 5.28% | PASS |
| ac_writes | `oltp_read_write_ac` | 36.30ms | 100.87ms | 2.779× | 4.33% | PASS |

</details>

## Version-control latency

Wall time: 2m 19s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.17ms | 200.00ms | 41.6% | 0.70% | PASS |
| `status_dirty_many_tables` | 85.95ms | 200.00ms | 43.0% | 0.68% | PASS |
| `diff_regular_working_one_table` | 78.67ms | 150.00ms | 52.4% | 0.87% | PASS |
| `diff_regular_working_many_tables` | 91.97ms | 200.00ms | 46.0% | 0.57% | PASS |
| `diff_stat_working_many_tables` | 92.08ms | 200.00ms | 46.0% | 0.57% | PASS |
| `diff_schema_working_many_tables` | 91.65ms | 200.00ms | 45.8% | 0.58% | PASS |
| `branch_list_many_branches` | 22.39ms | 100.00ms | 22.4% | 0.92% | PASS |
| `branch_create_delete` | 24.60ms | 100.00ms | 24.6% | 1.63% | PASS |
| `checkout_branch_clean` | 55.16ms | 200.00ms | 27.6% | 0.94% | PASS |
| `merge_data_no_conflicts` | 28.68ms | 150.00ms | 19.1% | 1.32% | PASS |
| `merge_schema_no_conflicts` | 22.23ms | 100.00ms | 22.2% | 1.75% | PASS |
| `merge_data_conflicts` | 127.35ms | 250.00ms | 50.9% | 0.42% | PASS |
| `merge_data_conflicts_with_resolve` | 127.83ms | 250.00ms | 51.1% | 0.55% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
