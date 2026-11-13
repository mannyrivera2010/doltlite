# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-13 18:51 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu26 20261111.196.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/43471656947)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 11m 59s | 8.73s | 11.67s | 1.336× | 1.57% | **PASS** |
| textpk | 69 | 55 | 1h 16m 9s | 8.16s | 8.95s | 1.097× | 3.07% | **PASS** |
| blobpk | 69 | 55 | 1h 28m 49s | 8.53s | 9.87s | 1.157× | 1.70% | **PASS** |
| compositepk | 69 | 55 | 1h 0m 53s | 6.85s | 7.46s | 1.090× | 2.00% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.51ms | 29.25ms | 1.194× | 2.10% | PASS |
| mem_reads | `oltp_range_select` | 10.82ms | 12.24ms | 1.131× | 1.66% | PASS |
| mem_reads | `oltp_sum_range` | 9.76ms | 12.06ms | 1.235× | 1.88% | PASS |
| mem_reads | `oltp_order_range` | 2.72ms | 3.04ms | 1.121× | 1.22% | PASS |
| mem_reads | `oltp_distinct_range` | 3.67ms | 4.17ms | 1.137× | 1.08% | PASS |
| mem_reads | `oltp_index_scan` | 3.97ms | 5.20ms | 1.311× | 1.89% | PASS |
| mem_reads | `select_random_points` | 11.12ms | 11.27ms | 1.013× | 2.63% | PASS |
| mem_reads | `select_random_ranges` | 3.06ms | 3.92ms | 1.282× | 2.09% | PASS |
| mem_reads | `covering_index_scan` | 4.12ms | 4.08ms | 0.989× | 1.29% | PASS |
| mem_reads | `groupby_scan` | 31.38ms | 35.09ms | 1.118× | 1.96% | PASS |
| mem_reads | `index_join` | 6.01ms | 7.88ms | 1.309× | 1.20% | PASS |
| mem_reads | `index_join_scan` | 3.47ms | 4.52ms | 1.302× | 3.59% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.34s | 1.297× | 0.51% | PASS |
| mem_reads | `table_scan` | 1.15s | 1.47s | 1.282× | 0.40% | PASS |
| mem_reads | `oltp_read_only` | 106.75ms | 124.72ms | 1.168× | 0.94% | PASS |
| mem_writes | `oltp_bulk_insert` | 175.11ms | 248.65ms | 1.420× | 0.77% | PASS |
| mem_writes | `oltp_insert` | 15.02ms | 27.89ms | 1.857× | 0.74% | PASS |
| mem_writes | `oltp_update_index` | 50.65ms | 105.89ms | 2.091× | 1.26% | PASS |
| mem_writes | `oltp_update_non_index` | 35.01ms | 61.29ms | 1.751× | 1.55% | PASS |
| mem_writes | `oltp_delete_insert` | 44.96ms | 80.73ms | 1.796× | 1.12% | PASS |
| mem_writes | `oltp_write_only` | 21.85ms | 47.00ms | 2.151× | 1.21% | PASS |
| mem_writes | `types_delete_insert` | 25.55ms | 42.43ms | 1.661× | 1.25% | PASS |
| mem_writes | `oltp_read_write` | 67.96ms | 111.21ms | 1.636× | 0.95% | PASS |
| file_reads | `oltp_point_select` | 93.56ms | 52.35ms | 0.560× | 0.64% | PASS |
| file_reads | `oltp_range_select` | 18.50ms | 14.81ms | 0.800× | 1.88% | PASS |
| file_reads | `oltp_sum_range` | 17.51ms | 14.79ms | 0.845× | 1.14% | PASS |
| file_reads | `oltp_order_range` | 3.67ms | 3.38ms | 0.920× | 1.10% | PASS |
| file_reads | `oltp_distinct_range` | 4.65ms | 4.53ms | 0.976× | 1.61% | PASS |
| file_reads | `oltp_index_scan` | 11.29ms | 7.82ms | 0.693× | 1.57% | PASS |
| file_reads | `select_random_points` | 19.21ms | 13.69ms | 0.713× | 1.71% | PASS |
| file_reads | `select_random_ranges` | 10.22ms | 6.32ms | 0.618× | 1.25% | PASS |
| file_reads | `covering_index_scan` | 11.53ms | 6.67ms | 0.578× | 1.39% | PASS |
| file_reads | `groupby_scan` | 32.62ms | 36.06ms | 1.105× | 2.13% | PASS |
| file_reads | `index_join` | 10.20ms | 9.79ms | 0.960× | 1.76% | PASS |
| file_reads | `index_join_scan` | 4.48ms | 5.05ms | 1.127× | 2.33% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.37s | 1.321× | 0.46% | PASS |
| file_reads | `table_scan` | 1.15s | 1.47s | 1.276× | 0.51% | PASS |
| file_reads | `oltp_read_only` | 207.31ms | 159.23ms | 0.768× | 0.83% | PASS |
| file_writes | `oltp_bulk_insert` | 188.25ms | 269.54ms | 1.432× | 1.40% | PASS |
| file_writes | `oltp_insert` | 22.30ms | 34.56ms | 1.550× | 1.38% | PASS |
| file_writes | `oltp_update_index` | 78.53ms | 131.56ms | 1.675× | 1.53% | PASS |
| file_writes | `oltp_update_non_index` | 58.92ms | 84.25ms | 1.430× | 1.75% | PASS |
| file_writes | `oltp_delete_insert` | 70.07ms | 104.53ms | 1.492× | 1.52% | PASS |
| file_writes | `oltp_write_only` | 46.11ms | 64.30ms | 1.395× | 1.36% | PASS |
| file_writes | `types_delete_insert` | 41.66ms | 52.34ms | 1.256× | 1.85% | PASS |
| file_writes | `oltp_read_write` | 95.23ms | 129.66ms | 1.361× | 1.90% | PASS |
| ac_reads | `oltp_point_select` | 46.90ms | 52.45ms | 1.118× | 1.15% | PASS |
| ac_reads | `oltp_range_select` | 13.37ms | 14.86ms | 1.111× | 2.21% | PASS |
| ac_reads | `oltp_sum_range` | 12.48ms | 14.75ms | 1.182× | 1.72% | PASS |
| ac_reads | `oltp_order_range` | 3.15ms | 3.40ms | 1.079× | 1.72% | PASS |
| ac_reads | `oltp_distinct_range` | 4.12ms | 4.54ms | 1.101× | 1.92% | PASS |
| ac_reads | `oltp_index_scan` | 6.36ms | 7.82ms | 1.230× | 1.61% | PASS |
| ac_reads | `select_random_points` | 14.07ms | 13.75ms | 0.977× | 2.30% | PASS |
| ac_reads | `select_random_ranges` | 5.43ms | 6.35ms | 1.169× | 1.19% | PASS |
| ac_reads | `covering_index_scan` | 6.49ms | 6.68ms | 1.030× | 1.46% | PASS |
| ac_reads | `groupby_scan` | 31.96ms | 36.40ms | 1.139× | 2.41% | PASS |
| ac_reads | `index_join` | 7.44ms | 9.80ms | 1.317× | 1.67% | PASS |
| ac_reads | `index_join_scan` | 3.87ms | 5.00ms | 1.294× | 1.79% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.37s | 1.323× | 0.53% | PASS |
| ac_reads | `table_scan` | 1.15s | 1.47s | 1.278× | 0.47% | PASS |
| ac_reads | `oltp_read_only` | 140.34ms | 159.61ms | 1.137× | 1.04% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.67ms | 65.58ms | 3.026× | 6.93% | PASS |
| ac_writes | `oltp_insert_ac` | 23.81ms | 82.75ms | 3.475× | 6.30% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.55ms | 98.23ms | 3.699× | 5.50% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.45ms | 76.13ms | 3.549× | 5.26% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.18ms | 88.69ms | 3.667× | 6.88% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.92ms | 87.50ms | 3.511× | 5.98% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.73ms | 75.90ms | 3.661× | 7.54% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.92ms | 95.54ms | 3.193× | 6.59% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.25ms | 26.21ms | 1.127× | 2.57% | PASS |
| mem_reads | `oltp_range_select` | 11.35ms | 10.34ms | 0.911× | 3.81% | PASS |
| mem_reads | `oltp_sum_range` | 10.42ms | 10.61ms | 1.019× | 3.52% | PASS |
| mem_reads | `oltp_order_range` | 2.31ms | 2.15ms | 0.930× | 1.66% | PASS |
| mem_reads | `oltp_distinct_range` | 2.82ms | 2.65ms | 0.941× | 1.26% | PASS |
| mem_reads | `oltp_index_scan` | 3.21ms | 4.37ms | 1.361× | 3.40% | PASS |
| mem_reads | `select_random_points` | 13.36ms | 14.42ms | 1.079× | 4.02% | PASS |
| mem_reads | `select_random_ranges` | 3.29ms | 3.83ms | 1.166× | 1.80% | PASS |
| mem_reads | `covering_index_scan` | 2.87ms | 3.32ms | 1.155× | 5.90% | PASS |
| mem_reads | `groupby_scan` | 23.40ms | 23.32ms | 0.996× | 1.52% | PASS |
| mem_reads | `index_join` | 5.81ms | 8.38ms | 1.443× | 6.50% | PASS |
| mem_reads | `index_join_scan` | 4.74ms | 5.65ms | 1.190× | 4.06% | PASS |
| mem_reads | `types_table_scan` | 934.53ms | 1.01s | 1.079× | 3.06% | PASS |
| mem_reads | `table_scan` | 1.22s | 1.01s | 0.827× | 1.91% | PASS |
| mem_reads | `oltp_read_only` | 102.89ms | 96.00ms | 0.933× | 2.06% | PASS |
| mem_writes | `oltp_bulk_insert` | 144.00ms | 232.14ms | 1.612× | 1.58% | PASS |
| mem_writes | `oltp_insert` | 16.00ms | 29.24ms | 1.827× | 3.78% | PASS |
| mem_writes | `oltp_update_index` | 63.03ms | 115.83ms | 1.838× | 3.39% | PASS |
| mem_writes | `oltp_update_non_index` | 38.48ms | 70.22ms | 1.825× | 2.50% | PASS |
| mem_writes | `oltp_delete_insert` | 43.15ms | 87.08ms | 2.018× | 3.09% | PASS |
| mem_writes | `oltp_write_only` | 24.51ms | 53.74ms | 2.192× | 3.14% | PASS |
| mem_writes | `types_delete_insert` | 24.93ms | 44.87ms | 1.800× | 2.63% | PASS |
| mem_writes | `oltp_read_write` | 68.52ms | 102.36ms | 1.494× | 3.07% | PASS |
| file_reads | `oltp_point_select` | 77.86ms | 44.01ms | 0.565× | 2.38% | PASS |
| file_reads | `oltp_range_select` | 16.43ms | 11.27ms | 0.686× | 2.35% | PASS |
| file_reads | `oltp_sum_range` | 16.84ms | 12.76ms | 0.758× | 3.23% | PASS |
| file_reads | `oltp_order_range` | 3.17ms | 2.50ms | 0.789× | 1.91% | PASS |
| file_reads | `oltp_distinct_range` | 3.65ms | 2.96ms | 0.810× | 1.88% | PASS |
| file_reads | `oltp_index_scan` | 9.59ms | 6.64ms | 0.692× | 1.50% | PASS |
| file_reads | `select_random_points` | 20.42ms | 17.12ms | 0.838× | 3.07% | PASS |
| file_reads | `select_random_ranges` | 9.04ms | 5.71ms | 0.632× | 1.40% | PASS |
| file_reads | `covering_index_scan` | 10.30ms | 5.39ms | 0.523× | 2.67% | PASS |
| file_reads | `groupby_scan` | 23.78ms | 23.14ms | 0.973× | 1.51% | PASS |
| file_reads | `index_join` | 9.27ms | 8.39ms | 0.906× | 3.17% | PASS |
| file_reads | `index_join_scan` | 4.57ms | 4.89ms | 1.070× | 2.57% | PASS |
| file_reads | `types_table_scan` | 757.14ms | 949.82ms | 1.254× | 3.29% | PASS |
| file_reads | `table_scan` | 1.06s | 933.52ms | 0.880× | 5.20% | PASS |
| file_reads | `oltp_read_only` | 173.02ms | 114.16ms | 0.660× | 2.39% | PASS |
| file_writes | `oltp_bulk_insert` | 208.06ms | 309.24ms | 1.486× | 3.31% | PASS |
| file_writes | `oltp_insert` | 52.09ms | 57.01ms | 1.094× | 2.91% | PASS |
| file_writes | `oltp_update_index` | 201.88ms | 207.91ms | 1.030× | 5.07% | PASS |
| file_writes | `oltp_update_non_index` | 150.66ms | 138.31ms | 0.918× | 4.75% | PASS |
| file_writes | `oltp_delete_insert` | 163.77ms | 160.24ms | 0.978× | 4.45% | PASS |
| file_writes | `oltp_write_only` | 124.17ms | 118.05ms | 0.951× | 2.48% | PASS |
| file_writes | `types_delete_insert` | 100.16ms | 90.24ms | 0.901× | 10.11% | PASS |
| file_writes | `oltp_read_write` | 171.60ms | 174.56ms | 1.017× | 3.25% | PASS |
| ac_reads | `oltp_point_select` | 38.48ms | 42.78ms | 1.112× | 2.11% | PASS |
| ac_reads | `oltp_range_select` | 11.64ms | 10.76ms | 0.924× | 3.87% | PASS |
| ac_reads | `oltp_sum_range` | 11.44ms | 11.85ms | 1.036× | 2.28% | PASS |
| ac_reads | `oltp_order_range` | 2.68ms | 2.39ms | 0.891× | 2.38% | PASS |
| ac_reads | `oltp_distinct_range` | 3.17ms | 2.89ms | 0.913× | 2.04% | PASS |
| ac_reads | `oltp_index_scan` | 5.79ms | 6.50ms | 1.122× | 1.95% | PASS |
| ac_reads | `select_random_points` | 16.27ms | 16.64ms | 1.023× | 2.67% | PASS |
| ac_reads | `select_random_ranges` | 5.06ms | 5.53ms | 1.093× | 2.27% | PASS |
| ac_reads | `covering_index_scan` | 6.32ms | 5.40ms | 0.856× | 3.22% | PASS |
| ac_reads | `groupby_scan` | 23.48ms | 23.30ms | 0.992× | 1.67% | PASS |
| ac_reads | `index_join` | 7.25ms | 8.20ms | 1.131× | 4.68% | PASS |
| ac_reads | `index_join_scan` | 4.00ms | 4.55ms | 1.138× | 1.63% | PASS |
| ac_reads | `types_table_scan` | 736.10ms | 922.13ms | 1.253× | 2.79% | PASS |
| ac_reads | `table_scan` | 837.66ms | 859.45ms | 1.026× | 2.41% | PASS |
| ac_reads | `oltp_read_only` | 109.82ms | 107.46ms | 0.979× | 3.72% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.09ms | 55.33ms | 2.624× | 3.54% | PASS |
| ac_writes | `oltp_insert_ac` | 23.82ms | 67.13ms | 2.818× | 6.97% | PASS |
| ac_writes | `oltp_update_index_ac` | 24.21ms | 78.53ms | 3.244× | 3.63% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.46ms | 66.09ms | 3.080× | 6.20% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.34ms | 75.49ms | 3.234× | 7.08% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.10ms | 72.72ms | 3.148× | 3.78% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.63ms | 66.09ms | 3.056× | 8.24% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.96ms | 78.24ms | 2.902× | 4.60% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.06ms | 27.59ms | 1.147× | 1.99% | PASS |
| mem_reads | `oltp_range_select` | 11.49ms | 11.08ms | 0.964× | 2.10% | PASS |
| mem_reads | `oltp_sum_range` | 10.02ms | 10.65ms | 1.062× | 2.44% | PASS |
| mem_reads | `oltp_order_range` | 2.53ms | 2.56ms | 1.014× | 1.94% | PASS |
| mem_reads | `oltp_distinct_range` | 3.28ms | 3.34ms | 1.017× | 1.26% | PASS |
| mem_reads | `oltp_index_scan` | 3.69ms | 4.84ms | 1.310× | 2.09% | PASS |
| mem_reads | `select_random_points` | 14.32ms | 16.01ms | 1.118× | 2.54% | PASS |
| mem_reads | `select_random_ranges` | 3.42ms | 4.23ms | 1.236× | 2.25% | PASS |
| mem_reads | `covering_index_scan` | 3.50ms | 3.51ms | 1.002× | 2.62% | PASS |
| mem_reads | `groupby_scan` | 26.87ms | 27.30ms | 1.016× | 0.98% | PASS |
| mem_reads | `index_join` | 5.47ms | 7.11ms | 1.301× | 1.84% | PASS |
| mem_reads | `index_join_scan` | 3.64ms | 4.78ms | 1.314× | 1.68% | PASS |
| mem_reads | `types_table_scan` | 865.98ms | 974.75ms | 1.126× | 0.76% | PASS |
| mem_reads | `table_scan` | 1.02s | 1.07s | 1.047× | 2.54% | PASS |
| mem_reads | `oltp_read_only` | 94.51ms | 102.55ms | 1.085× | 1.63% | PASS |
| mem_writes | `oltp_bulk_insert` | 185.24ms | 272.01ms | 1.468× | 1.04% | PASS |
| mem_writes | `oltp_insert` | 15.87ms | 32.06ms | 2.019× | 1.54% | PASS |
| mem_writes | `oltp_update_index` | 55.89ms | 109.11ms | 1.952× | 1.33% | PASS |
| mem_writes | `oltp_update_non_index` | 40.06ms | 69.84ms | 1.743× | 1.09% | PASS |
| mem_writes | `oltp_delete_insert` | 39.76ms | 84.60ms | 2.128× | 1.09% | PASS |
| mem_writes | `oltp_write_only` | 23.30ms | 52.49ms | 2.253× | 1.65% | PASS |
| mem_writes | `types_delete_insert` | 25.77ms | 44.52ms | 1.727× | 1.65% | PASS |
| mem_writes | `oltp_read_write` | 65.54ms | 108.44ms | 1.654× | 1.37% | PASS |
| file_reads | `oltp_point_select` | 92.88ms | 50.36ms | 0.542× | 0.94% | PASS |
| file_reads | `oltp_range_select` | 18.62ms | 13.27ms | 0.712× | 2.04% | PASS |
| file_reads | `oltp_sum_range` | 17.19ms | 13.01ms | 0.757× | 1.47% | PASS |
| file_reads | `oltp_order_range` | 3.29ms | 2.83ms | 0.860× | 1.93% | PASS |
| file_reads | `oltp_distinct_range` | 4.10ms | 3.61ms | 0.881× | 1.70% | PASS |
| file_reads | `oltp_index_scan` | 11.03ms | 7.29ms | 0.661× | 1.18% | PASS |
| file_reads | `select_random_points` | 21.57ms | 18.26ms | 0.847× | 2.52% | PASS |
| file_reads | `select_random_ranges` | 10.40ms | 6.55ms | 0.630× | 1.30% | PASS |
| file_reads | `covering_index_scan` | 11.38ms | 6.19ms | 0.544× | 1.41% | PASS |
| file_reads | `groupby_scan` | 28.04ms | 27.68ms | 0.987× | 1.25% | PASS |
| file_reads | `index_join` | 9.80ms | 9.33ms | 0.952× | 1.83% | PASS |
| file_reads | `index_join_scan` | 4.58ms | 5.23ms | 1.141× | 2.03% | PASS |
| file_reads | `types_table_scan` | 891.23ms | 989.65ms | 1.110× | 1.84% | PASS |
| file_reads | `table_scan` | 1.06s | 1.07s | 1.008× | 3.12% | PASS |
| file_reads | `oltp_read_only` | 193.68ms | 134.28ms | 0.693× | 1.00% | PASS |
| file_writes | `oltp_bulk_insert` | 249.66ms | 358.46ms | 1.436× | 8.74% | PASS |
| file_writes | `oltp_insert` | 52.45ms | 66.70ms | 1.272× | 16.79% | PASS |
| file_writes | `oltp_update_index` | 189.36ms | 218.73ms | 1.155× | 13.38% | PASS |
| file_writes | `oltp_update_non_index` | 174.62ms | 159.02ms | 0.911× | 19.44% | PASS |
| file_writes | `oltp_delete_insert` | 160.90ms | 179.62ms | 1.116× | 14.67% | PASS |
| file_writes | `oltp_write_only` | 124.62ms | 128.94ms | 1.035× | 18.40% | PASS |
| file_writes | `types_delete_insert` | 100.15ms | 96.74ms | 0.966× | 12.91% | PASS |
| file_writes | `oltp_read_write` | 199.00ms | 186.90ms | 0.939× | 17.14% | PASS |
| ac_reads | `oltp_point_select` | 45.20ms | 50.27ms | 1.112× | 0.95% | PASS |
| ac_reads | `oltp_range_select` | 14.11ms | 13.31ms | 0.944× | 1.72% | PASS |
| ac_reads | `oltp_sum_range` | 12.67ms | 13.06ms | 1.031× | 1.30% | PASS |
| ac_reads | `oltp_order_range` | 2.94ms | 2.84ms | 0.963× | 1.27% | PASS |
| ac_reads | `oltp_distinct_range` | 3.70ms | 3.62ms | 0.979× | 1.23% | PASS |
| ac_reads | `oltp_index_scan` | 6.49ms | 7.35ms | 1.133× | 1.09% | PASS |
| ac_reads | `select_random_points` | 17.14ms | 18.45ms | 1.076× | 1.26% | PASS |
| ac_reads | `select_random_ranges` | 5.83ms | 6.55ms | 1.123× | 0.79% | PASS |
| ac_reads | `covering_index_scan` | 6.79ms | 6.20ms | 0.912× | 1.01% | PASS |
| ac_reads | `groupby_scan` | 27.55ms | 27.69ms | 1.005× | 0.95% | PASS |
| ac_reads | `index_join` | 7.53ms | 9.32ms | 1.238× | 1.62% | PASS |
| ac_reads | `index_join_scan` | 4.15ms | 5.11ms | 1.233× | 1.49% | PASS |
| ac_reads | `types_table_scan` | 878.28ms | 985.52ms | 1.122× | 0.89% | PASS |
| ac_reads | `table_scan` | 1.01s | 1.06s | 1.043× | 1.30% | PASS |
| ac_reads | `oltp_read_only` | 124.14ms | 134.07ms | 1.080× | 0.76% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.03ms | 65.30ms | 3.105× | 24.93% | PASS |
| ac_writes | `oltp_insert_ac` | 26.02ms | 115.04ms | 4.421× | 66.64% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.95ms | 124.65ms | 4.625× | 54.58% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.06ms | 87.38ms | 3.790× | 51.53% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 21.19ms | 85.20ms | 4.020× | 16.62% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.33ms | 86.82ms | 3.888× | 26.46% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.30ms | 87.30ms | 4.099× | 20.13% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.59ms | 85.49ms | 3.216× | 14.01% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 21.29ms | 23.55ms | 1.106× | 1.32% | PASS |
| mem_reads | `oltp_range_select` | 11.14ms | 11.92ms | 1.070× | 1.14% | PASS |
| mem_reads | `oltp_sum_range` | 10.81ms | 11.48ms | 1.062× | 1.89% | PASS |
| mem_reads | `oltp_order_range` | 2.22ms | 2.24ms | 1.008× | 1.45% | PASS |
| mem_reads | `oltp_distinct_range` | 2.74ms | 2.72ms | 0.992× | 1.18% | PASS |
| mem_reads | `oltp_index_scan` | 2.96ms | 3.90ms | 1.316× | 3.66% | PASS |
| mem_reads | `select_random_points` | 16.65ms | 19.23ms | 1.155× | 2.00% | PASS |
| mem_reads | `select_random_ranges` | 4.57ms | 5.01ms | 1.096× | 2.18% | PASS |
| mem_reads | `covering_index_scan` | 2.33ms | 2.88ms | 1.235× | 7.55% | PASS |
| mem_reads | `groupby_scan` | 22.07ms | 24.18ms | 1.095× | 1.40% | PASS |
| mem_reads | `index_join` | 4.84ms | 6.50ms | 1.343× | 8.13% | PASS |
| mem_reads | `index_join_scan` | 2.15ms | 3.90ms | 1.812× | 4.15% | PASS |
| mem_reads | `types_table_scan` | 666.82ms | 722.37ms | 1.083× | 0.62% | PASS |
| mem_reads | `table_scan` | 790.80ms | 813.60ms | 1.029× | 3.46% | PASS |
| mem_reads | `oltp_read_only` | 85.36ms | 88.21ms | 1.033× | 2.25% | PASS |
| mem_writes | `oltp_bulk_insert` | 139.13ms | 199.81ms | 1.436× | 0.86% | PASS |
| mem_writes | `oltp_insert` | 10.86ms | 22.72ms | 2.092× | 0.97% | PASS |
| mem_writes | `oltp_update_index` | 41.30ms | 77.89ms | 1.886× | 1.49% | PASS |
| mem_writes | `oltp_update_non_index` | 32.60ms | 57.41ms | 1.761× | 2.55% | PASS |
| mem_writes | `oltp_delete_insert` | 30.96ms | 64.24ms | 2.075× | 1.26% | PASS |
| mem_writes | `oltp_write_only` | 17.67ms | 41.78ms | 2.364× | 2.97% | PASS |
| mem_writes | `types_delete_insert` | 20.89ms | 36.55ms | 1.750× | 3.37% | PASS |
| mem_writes | `oltp_read_write` | 57.13ms | 88.75ms | 1.554× | 1.48% | PASS |
| file_reads | `oltp_point_select` | 72.12ms | 40.72ms | 0.565× | 1.64% | PASS |
| file_reads | `oltp_range_select` | 17.03ms | 14.10ms | 0.828× | 2.88% | PASS |
| file_reads | `oltp_sum_range` | 15.74ms | 12.73ms | 0.809× | 1.63% | PASS |
| file_reads | `oltp_order_range` | 2.81ms | 2.46ms | 0.874× | 1.58% | PASS |
| file_reads | `oltp_distinct_range` | 3.32ms | 2.93ms | 0.882× | 1.17% | PASS |
| file_reads | `oltp_index_scan` | 8.51ms | 5.80ms | 0.682× | 3.57% | PASS |
| file_reads | `select_random_points` | 22.49ms | 20.49ms | 0.911× | 2.11% | PASS |
| file_reads | `select_random_ranges` | 10.21ms | 6.94ms | 0.680× | 2.00% | PASS |
| file_reads | `covering_index_scan` | 8.60ms | 4.86ms | 0.565× | 4.08% | PASS |
| file_reads | `groupby_scan` | 23.86ms | 25.75ms | 1.080× | 2.23% | PASS |
| file_reads | `index_join` | 8.53ms | 8.22ms | 0.963× | 3.83% | PASS |
| file_reads | `index_join_scan` | 3.59ms | 4.44ms | 1.238× | 6.26% | PASS |
| file_reads | `types_table_scan` | 747.36ms | 747.08ms | 1.000× | 1.48% | PASS |
| file_reads | `table_scan` | 994.93ms | 868.65ms | 0.873× | 1.17% | PASS |
| file_reads | `oltp_read_only` | 165.00ms | 115.53ms | 0.700× | 2.19% | PASS |
| file_writes | `oltp_bulk_insert` | 187.77ms | 249.25ms | 1.327× | 2.45% | PASS |
| file_writes | `oltp_insert` | 19.50ms | 38.17ms | 1.958× | 0.55% | PASS |
| file_writes | `oltp_update_index` | 140.64ms | 139.43ms | 0.991× | 0.28% | PASS |
| file_writes | `oltp_update_non_index` | 120.11ms | 104.20ms | 0.868× | 3.78% | PASS |
| file_writes | `oltp_delete_insert` | 120.26ms | 110.03ms | 0.915× | 3.77% | PASS |
| file_writes | `oltp_write_only` | 80.54ms | 88.43ms | 1.098× | 0.57% | PASS |
| file_writes | `types_delete_insert` | 74.45ms | 69.11ms | 0.928× | 4.66% | PASS |
| file_writes | `oltp_read_write` | 120.33ms | 135.50ms | 1.126× | 2.77% | PASS |
| ac_reads | `oltp_point_select` | 35.08ms | 38.85ms | 1.108× | 0.94% | PASS |
| ac_reads | `oltp_range_select` | 12.05ms | 13.41ms | 1.113× | 1.06% | PASS |
| ac_reads | `oltp_sum_range` | 11.36ms | 12.33ms | 1.085× | 1.04% | PASS |
| ac_reads | `oltp_order_range` | 2.28ms | 2.32ms | 1.017× | 1.30% | PASS |
| ac_reads | `oltp_distinct_range` | 2.82ms | 2.83ms | 1.003× | 1.24% | PASS |
| ac_reads | `oltp_index_scan` | 4.47ms | 5.20ms | 1.163× | 1.12% | PASS |
| ac_reads | `select_random_points` | 16.59ms | 18.87ms | 1.137× | 1.23% | PASS |
| ac_reads | `select_random_ranges` | 6.27ms | 6.88ms | 1.096× | 0.99% | PASS |
| ac_reads | `covering_index_scan` | 3.98ms | 4.13ms | 1.038× | 1.10% | PASS |
| ac_reads | `groupby_scan` | 21.03ms | 23.90ms | 1.136× | 1.07% | PASS |
| ac_reads | `index_join` | 5.76ms | 6.87ms | 1.192× | 1.65% | PASS |
| ac_reads | `index_join_scan` | 2.81ms | 4.03ms | 1.431× | 2.10% | PASS |
| ac_reads | `types_table_scan` | 673.98ms | 721.38ms | 1.070× | 1.14% | PASS |
| ac_reads | `table_scan` | 785.59ms | 813.68ms | 1.036× | 1.52% | PASS |
| ac_reads | `oltp_read_only` | 105.63ms | 111.82ms | 1.059× | 2.33% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.20ms | 51.69ms | 2.438× | 3.10% | PASS |
| ac_writes | `oltp_insert_ac` | 25.18ms | 69.44ms | 2.757× | 5.08% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.71ms | 74.88ms | 2.804× | 3.42% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.89ms | 60.81ms | 2.911× | 3.69% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.03ms | 70.87ms | 2.949× | 5.40% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.44ms | 70.39ms | 2.880× | 4.12% | PASS |
| ac_writes | `types_delete_insert_ac` | 25.10ms | 59.51ms | 2.371× | 9.29% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.51ms | 73.92ms | 2.789× | 2.90% | PASS |

</details>

## Version-control latency

Wall time: 2m 8s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 62.41ms | 200.00ms | 31.2% | 0.69% | PASS |
| `status_dirty_many_tables` | 64.77ms | 200.00ms | 32.4% | 0.51% | PASS |
| `diff_regular_working_one_table` | 58.58ms | 150.00ms | 39.1% | 0.37% | PASS |
| `diff_regular_working_many_tables` | 68.98ms | 200.00ms | 34.5% | 0.57% | PASS |
| `diff_stat_working_many_tables` | 68.83ms | 200.00ms | 34.4% | 0.50% | PASS |
| `diff_schema_working_many_tables` | 69.24ms | 200.00ms | 34.6% | 0.60% | PASS |
| `branch_list_many_branches` | 16.90ms | 100.00ms | 16.9% | 2.25% | PASS |
| `branch_create_delete` | 25.87ms | 100.00ms | 25.9% | 30.18% | PASS |
| `checkout_branch_clean` | 87.15ms | 200.00ms | 43.6% | 14.84% | PASS |
| `merge_data_no_conflicts` | 34.84ms | 150.00ms | 23.2% | 31.02% | PASS |
| `merge_schema_no_conflicts` | 16.62ms | 100.00ms | 16.6% | 8.69% | PASS |
| `merge_data_conflicts` | 94.47ms | 250.00ms | 37.8% | 0.28% | PASS |
| `merge_data_conflicts_with_resolve` | 94.78ms | 250.00ms | 37.9% | 0.42% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
