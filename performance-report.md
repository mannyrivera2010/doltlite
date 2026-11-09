# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-09 21:31 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu26 20261101.186.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/42688346219)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 32s | 8.77s | 11.50s | 1.311× | 1.52% | **PASS** |
| textpk | 69 | 55 | 1h 12m 6s | 7.45s | 8.34s | 1.119× | 2.73% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 0s | 9.37s | 11.59s | 1.237× | 1.60% | **PASS** |
| compositepk | 69 | 55 | 1h 23m 16s | 9.08s | 10.15s | 1.118× | 2.00% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.01ms | 29.41ms | 1.176× | 1.44% | PASS |
| mem_reads | `oltp_range_select` | 11.08ms | 12.40ms | 1.119× | 1.21% | PASS |
| mem_reads | `oltp_sum_range` | 9.78ms | 12.13ms | 1.241× | 1.04% | PASS |
| mem_reads | `oltp_order_range` | 2.73ms | 3.08ms | 1.129× | 1.13% | PASS |
| mem_reads | `oltp_distinct_range` | 3.73ms | 4.19ms | 1.124× | 0.86% | PASS |
| mem_reads | `oltp_index_scan` | 4.04ms | 5.34ms | 1.322× | 2.07% | PASS |
| mem_reads | `select_random_points` | 11.07ms | 11.16ms | 1.008× | 2.20% | PASS |
| mem_reads | `select_random_ranges` | 3.14ms | 3.98ms | 1.267× | 1.85% | PASS |
| mem_reads | `covering_index_scan` | 4.16ms | 4.20ms | 1.011× | 1.66% | PASS |
| mem_reads | `groupby_scan` | 31.45ms | 35.20ms | 1.119× | 2.72% | PASS |
| mem_reads | `index_join` | 6.07ms | 8.00ms | 1.317× | 1.62% | PASS |
| mem_reads | `index_join_scan` | 3.40ms | 4.42ms | 1.300× | 1.75% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.31s | 1.269× | 0.39% | PASS |
| mem_reads | `table_scan` | 1.15s | 1.44s | 1.259× | 0.57% | PASS |
| mem_reads | `oltp_read_only` | 108.33ms | 125.00ms | 1.154× | 1.64% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.77ms | 253.53ms | 1.410× | 0.85% | PASS |
| mem_writes | `oltp_insert` | 15.33ms | 28.23ms | 1.841× | 1.05% | PASS |
| mem_writes | `oltp_update_index` | 50.78ms | 106.58ms | 2.099× | 1.75% | PASS |
| mem_writes | `oltp_update_non_index` | 34.62ms | 62.06ms | 1.793× | 1.58% | PASS |
| mem_writes | `oltp_delete_insert` | 44.67ms | 81.38ms | 1.822× | 1.47% | PASS |
| mem_writes | `oltp_write_only` | 21.81ms | 47.02ms | 2.156× | 1.94% | PASS |
| mem_writes | `types_delete_insert` | 24.95ms | 42.76ms | 1.713× | 1.52% | PASS |
| mem_writes | `oltp_read_write` | 69.19ms | 112.01ms | 1.619× | 1.47% | PASS |
| file_reads | `oltp_point_select` | 93.61ms | 52.87ms | 0.565× | 0.91% | PASS |
| file_reads | `oltp_range_select` | 18.18ms | 14.94ms | 0.821× | 1.48% | PASS |
| file_reads | `oltp_sum_range` | 16.94ms | 14.77ms | 0.872× | 1.80% | PASS |
| file_reads | `oltp_order_range` | 3.52ms | 3.39ms | 0.963× | 1.39% | PASS |
| file_reads | `oltp_distinct_range` | 4.52ms | 4.49ms | 0.993× | 1.20% | PASS |
| file_reads | `oltp_index_scan` | 11.24ms | 7.97ms | 0.709× | 1.68% | PASS |
| file_reads | `select_random_points` | 18.50ms | 13.71ms | 0.741× | 2.38% | PASS |
| file_reads | `select_random_ranges` | 10.24ms | 6.36ms | 0.621× | 1.73% | PASS |
| file_reads | `covering_index_scan` | 11.40ms | 6.79ms | 0.595× | 2.34% | PASS |
| file_reads | `groupby_scan` | 32.54ms | 35.20ms | 1.082× | 1.60% | PASS |
| file_reads | `index_join` | 9.92ms | 9.83ms | 0.990× | 1.14% | PASS |
| file_reads | `index_join_scan` | 4.37ms | 4.87ms | 1.115× | 1.32% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.34s | 1.292× | 0.44% | PASS |
| file_reads | `table_scan` | 1.16s | 1.44s | 1.247× | 0.74% | PASS |
| file_reads | `oltp_read_only` | 208.77ms | 159.81ms | 0.766× | 0.84% | PASS |
| file_writes | `oltp_bulk_insert` | 192.34ms | 272.45ms | 1.417× | 0.80% | PASS |
| file_writes | `oltp_insert` | 21.80ms | 34.70ms | 1.592× | 1.36% | PASS |
| file_writes | `oltp_update_index` | 77.26ms | 131.93ms | 1.708× | 1.15% | PASS |
| file_writes | `oltp_update_non_index` | 58.30ms | 84.95ms | 1.457× | 1.79% | PASS |
| file_writes | `oltp_delete_insert` | 68.28ms | 105.55ms | 1.546× | 1.41% | PASS |
| file_writes | `oltp_write_only` | 45.05ms | 64.79ms | 1.438× | 2.34% | PASS |
| file_writes | `types_delete_insert` | 40.84ms | 53.21ms | 1.303× | 1.90% | PASS |
| file_writes | `oltp_read_write` | 95.04ms | 129.46ms | 1.362× | 1.85% | PASS |
| ac_reads | `oltp_point_select` | 47.56ms | 52.99ms | 1.114× | 1.16% | PASS |
| ac_reads | `oltp_range_select` | 13.61ms | 14.89ms | 1.094× | 1.18% | PASS |
| ac_reads | `oltp_sum_range` | 12.52ms | 14.75ms | 1.178× | 1.11% | PASS |
| ac_reads | `oltp_order_range` | 3.13ms | 3.38ms | 1.082× | 1.37% | PASS |
| ac_reads | `oltp_distinct_range` | 4.09ms | 4.50ms | 1.100× | 1.47% | PASS |
| ac_reads | `oltp_index_scan` | 6.58ms | 8.03ms | 1.219× | 1.86% | PASS |
| ac_reads | `select_random_points` | 13.92ms | 13.68ms | 0.983× | 2.11% | PASS |
| ac_reads | `select_random_ranges` | 5.59ms | 6.38ms | 1.141× | 1.28% | PASS |
| ac_reads | `covering_index_scan` | 6.74ms | 6.85ms | 1.016× | 2.01% | PASS |
| ac_reads | `groupby_scan` | 32.01ms | 36.57ms | 1.142× | 2.72% | PASS |
| ac_reads | `index_join` | 7.57ms | 9.82ms | 1.297× | 1.62% | PASS |
| ac_reads | `index_join_scan` | 3.84ms | 4.91ms | 1.278× | 1.39% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.34s | 1.292× | 0.58% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.45s | 1.235× | 1.06% | PASS |
| ac_reads | `oltp_read_only` | 141.90ms | 160.10ms | 1.128× | 1.04% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.23ms | 63.79ms | 3.005× | 5.93% | PASS |
| ac_writes | `oltp_insert_ac` | 23.75ms | 78.83ms | 3.319× | 4.59% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.02ms | 92.82ms | 3.710× | 5.06% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.26ms | 71.97ms | 3.385× | 4.15% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.04ms | 85.77ms | 3.723× | 5.13% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.37ms | 82.69ms | 3.538× | 3.89% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.43ms | 72.33ms | 3.374× | 5.48% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.91ms | 89.81ms | 3.217× | 3.31% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.04ms | 22.27ms | 1.112× | 3.07% | PASS |
| mem_reads | `oltp_range_select` | 9.60ms | 8.83ms | 0.920× | 2.32% | PASS |
| mem_reads | `oltp_sum_range` | 9.38ms | 9.08ms | 0.968× | 2.87% | PASS |
| mem_reads | `oltp_order_range` | 2.18ms | 2.06ms | 0.948× | 3.07% | PASS |
| mem_reads | `oltp_distinct_range` | 2.66ms | 2.49ms | 0.935× | 1.62% | PASS |
| mem_reads | `oltp_index_scan` | 3.01ms | 4.16ms | 1.382× | 2.84% | PASS |
| mem_reads | `select_random_points` | 13.44ms | 14.33ms | 1.067× | 2.28% | PASS |
| mem_reads | `select_random_ranges` | 2.98ms | 3.53ms | 1.188× | 2.19% | PASS |
| mem_reads | `covering_index_scan` | 2.61ms | 3.06ms | 1.171× | 3.89% | PASS |
| mem_reads | `groupby_scan` | 21.90ms | 21.34ms | 0.975× | 1.28% | PASS |
| mem_reads | `index_join` | 4.48ms | 5.57ms | 1.243× | 2.28% | PASS |
| mem_reads | `index_join_scan` | 2.52ms | 4.15ms | 1.646× | 5.26% | PASS |
| mem_reads | `types_table_scan` | 689.17ms | 796.47ms | 1.156× | 1.49% | PASS |
| mem_reads | `table_scan` | 821.30ms | 904.16ms | 1.101× | 1.84% | PASS |
| mem_reads | `oltp_read_only` | 74.09ms | 74.64ms | 1.007× | 3.22% | PASS |
| mem_writes | `oltp_bulk_insert` | 140.31ms | 213.63ms | 1.523× | 1.07% | PASS |
| mem_writes | `oltp_insert` | 13.44ms | 26.75ms | 1.991× | 1.07% | PASS |
| mem_writes | `oltp_update_index` | 48.24ms | 98.13ms | 2.034× | 2.02% | PASS |
| mem_writes | `oltp_update_non_index` | 33.45ms | 61.95ms | 1.852× | 1.82% | PASS |
| mem_writes | `oltp_delete_insert` | 33.28ms | 74.03ms | 2.225× | 1.50% | PASS |
| mem_writes | `oltp_write_only` | 19.47ms | 45.12ms | 2.318× | 1.26% | PASS |
| mem_writes | `types_delete_insert` | 22.17ms | 39.84ms | 1.797× | 3.02% | PASS |
| mem_writes | `oltp_read_write` | 55.76ms | 90.38ms | 1.621× | 2.89% | PASS |
| file_reads | `oltp_point_select` | 75.07ms | 42.02ms | 0.560× | 1.08% | PASS |
| file_reads | `oltp_range_select` | 15.38ms | 10.74ms | 0.698× | 2.20% | PASS |
| file_reads | `oltp_sum_range` | 15.41ms | 10.98ms | 0.712× | 2.23% | PASS |
| file_reads | `oltp_order_range` | 2.90ms | 2.31ms | 0.797× | 1.62% | PASS |
| file_reads | `oltp_distinct_range` | 3.50ms | 2.84ms | 0.813× | 1.83% | PASS |
| file_reads | `oltp_index_scan` | 9.58ms | 6.62ms | 0.692× | 1.82% | PASS |
| file_reads | `select_random_points` | 20.95ms | 16.71ms | 0.797× | 3.44% | PASS |
| file_reads | `select_random_ranges` | 8.67ms | 5.50ms | 0.634× | 1.50% | PASS |
| file_reads | `covering_index_scan` | 10.07ms | 5.27ms | 0.523× | 2.49% | PASS |
| file_reads | `groupby_scan` | 23.02ms | 21.76ms | 0.945× | 1.40% | PASS |
| file_reads | `index_join` | 8.71ms | 8.00ms | 0.918× | 2.92% | PASS |
| file_reads | `index_join_scan` | 4.47ms | 4.64ms | 1.038× | 4.67% | PASS |
| file_reads | `types_table_scan` | 725.41ms | 833.21ms | 1.149× | 2.73% | PASS |
| file_reads | `table_scan` | 1.11s | 992.11ms | 0.891× | 4.27% | PASS |
| file_reads | `oltp_read_only` | 167.74ms | 110.83ms | 0.661× | 1.66% | PASS |
| file_writes | `oltp_bulk_insert` | 199.26ms | 308.70ms | 1.549× | 3.40% | PASS |
| file_writes | `oltp_insert` | 50.49ms | 57.41ms | 1.137× | 3.93% | PASS |
| file_writes | `oltp_update_index` | 201.56ms | 198.41ms | 0.984× | 3.88% | PASS |
| file_writes | `oltp_update_non_index` | 134.74ms | 130.09ms | 0.966× | 6.20% | PASS |
| file_writes | `oltp_delete_insert` | 157.76ms | 148.89ms | 0.944× | 3.06% | PASS |
| file_writes | `oltp_write_only` | 120.41ms | 102.16ms | 0.848× | 2.55% | PASS |
| file_writes | `types_delete_insert` | 91.62ms | 83.51ms | 0.912× | 5.79% | PASS |
| file_writes | `oltp_read_write` | 160.48ms | 148.27ms | 0.924× | 4.26% | PASS |
| ac_reads | `oltp_point_select` | 38.43ms | 41.64ms | 1.084× | 2.00% | PASS |
| ac_reads | `oltp_range_select` | 12.85ms | 11.16ms | 0.868× | 2.54% | PASS |
| ac_reads | `oltp_sum_range` | 12.90ms | 11.79ms | 0.914× | 3.50% | PASS |
| ac_reads | `oltp_order_range` | 2.79ms | 2.50ms | 0.894× | 1.98% | PASS |
| ac_reads | `oltp_distinct_range` | 3.25ms | 2.91ms | 0.895× | 1.56% | PASS |
| ac_reads | `oltp_index_scan` | 5.80ms | 6.55ms | 1.128× | 1.25% | PASS |
| ac_reads | `select_random_points` | 17.18ms | 17.30ms | 1.007× | 3.71% | PASS |
| ac_reads | `select_random_ranges` | 5.25ms | 5.58ms | 1.063× | 1.96% | PASS |
| ac_reads | `covering_index_scan` | 6.39ms | 5.25ms | 0.821× | 2.94% | PASS |
| ac_reads | `groupby_scan` | 23.35ms | 22.37ms | 0.958× | 2.23% | PASS |
| ac_reads | `index_join` | 7.60ms | 8.58ms | 1.128× | 4.23% | PASS |
| ac_reads | `index_join_scan` | 4.44ms | 4.94ms | 1.112× | 7.05% | PASS |
| ac_reads | `types_table_scan` | 832.51ms | 886.28ms | 1.065× | 3.90% | PASS |
| ac_reads | `table_scan` | 808.52ms | 904.54ms | 1.119× | 1.50% | PASS |
| ac_reads | `oltp_read_only` | 92.15ms | 97.12ms | 1.054× | 1.55% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 25.38ms | 53.12ms | 2.093× | 6.21% | PASS |
| ac_writes | `oltp_insert_ac` | 28.25ms | 64.02ms | 2.266× | 8.51% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.33ms | 77.00ms | 2.625× | 5.21% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.29ms | 60.72ms | 2.401× | 7.55% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 27.72ms | 72.45ms | 2.613× | 7.80% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.75ms | 70.09ms | 2.526× | 8.44% | PASS |
| ac_writes | `types_delete_insert_ac` | 19.90ms | 62.09ms | 3.120× | 8.05% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.88ms | 74.81ms | 2.423× | 6.52% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.48ms | 36.64ms | 1.243× | 1.68% | PASS |
| mem_reads | `oltp_range_select` | 12.97ms | 14.15ms | 1.091× | 1.41% | PASS |
| mem_reads | `oltp_sum_range` | 11.18ms | 13.55ms | 1.212× | 1.25% | PASS |
| mem_reads | `oltp_order_range` | 2.90ms | 3.15ms | 1.088× | 1.09% | PASS |
| mem_reads | `oltp_distinct_range` | 4.00ms | 4.28ms | 1.068× | 1.20% | PASS |
| mem_reads | `oltp_index_scan` | 4.44ms | 6.06ms | 1.366× | 1.53% | PASS |
| mem_reads | `select_random_points` | 17.52ms | 19.97ms | 1.140× | 2.11% | PASS |
| mem_reads | `select_random_ranges` | 4.00ms | 5.19ms | 1.298× | 1.40% | PASS |
| mem_reads | `covering_index_scan` | 4.34ms | 4.52ms | 1.042× | 1.43% | PASS |
| mem_reads | `groupby_scan` | 32.79ms | 36.36ms | 1.109× | 2.33% | PASS |
| mem_reads | `index_join` | 6.76ms | 9.23ms | 1.365× | 1.63% | PASS |
| mem_reads | `index_join_scan` | 4.06ms | 5.18ms | 1.275× | 2.28% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.24s | 1.164× | 1.07% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.33s | 1.126× | 1.24% | PASS |
| mem_reads | `oltp_read_only` | 118.13ms | 135.35ms | 1.146× | 0.69% | PASS |
| mem_writes | `oltp_bulk_insert` | 237.99ms | 356.70ms | 1.499× | 0.83% | PASS |
| mem_writes | `oltp_insert` | 19.44ms | 40.09ms | 2.063× | 0.83% | PASS |
| mem_writes | `oltp_update_index` | 66.13ms | 131.85ms | 1.994× | 0.90% | PASS |
| mem_writes | `oltp_update_non_index` | 47.47ms | 85.65ms | 1.805× | 1.20% | PASS |
| mem_writes | `oltp_delete_insert` | 47.32ms | 104.47ms | 2.208× | 0.93% | PASS |
| mem_writes | `oltp_write_only` | 27.03ms | 63.21ms | 2.338× | 1.07% | PASS |
| mem_writes | `types_delete_insert` | 31.59ms | 56.54ms | 1.790× | 1.46% | PASS |
| mem_writes | `oltp_read_write` | 81.40ms | 140.00ms | 1.720× | 0.84% | PASS |
| file_reads | `oltp_point_select` | 99.29ms | 61.16ms | 0.616× | 1.34% | PASS |
| file_reads | `oltp_range_select` | 20.97ms | 16.82ms | 0.802× | 2.11% | PASS |
| file_reads | `oltp_sum_range` | 18.98ms | 16.28ms | 0.858× | 2.91% | PASS |
| file_reads | `oltp_order_range` | 3.77ms | 3.51ms | 0.931× | 2.64% | PASS |
| file_reads | `oltp_distinct_range` | 4.81ms | 4.64ms | 0.965× | 2.41% | PASS |
| file_reads | `oltp_index_scan` | 11.47ms | 8.80ms | 0.767× | 1.93% | PASS |
| file_reads | `select_random_points` | 25.47ms | 23.23ms | 0.912× | 2.83% | PASS |
| file_reads | `select_random_ranges` | 10.84ms | 7.62ms | 0.703× | 1.60% | PASS |
| file_reads | `covering_index_scan` | 11.47ms | 7.10ms | 0.619× | 2.65% | PASS |
| file_reads | `groupby_scan` | 33.81ms | 36.97ms | 1.094× | 2.43% | PASS |
| file_reads | `index_join` | 10.89ms | 11.18ms | 1.027× | 2.79% | PASS |
| file_reads | `index_join_scan` | 5.13ms | 5.67ms | 1.105× | 2.62% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.24s | 1.163× | 0.92% | PASS |
| file_reads | `table_scan` | 1.24s | 1.34s | 1.081× | 2.16% | PASS |
| file_reads | `oltp_read_only` | 226.80ms | 173.67ms | 0.766× | 0.97% | PASS |
| file_writes | `oltp_bulk_insert` | 252.34ms | 384.04ms | 1.522× | 1.06% | PASS |
| file_writes | `oltp_insert` | 31.31ms | 53.43ms | 1.706× | 1.87% | PASS |
| file_writes | `oltp_update_index` | 103.22ms | 169.21ms | 1.639× | 1.93% | PASS |
| file_writes | `oltp_update_non_index` | 78.74ms | 112.83ms | 1.433× | 0.96% | PASS |
| file_writes | `oltp_delete_insert` | 80.87ms | 135.19ms | 1.672× | 1.32% | PASS |
| file_writes | `oltp_write_only` | 55.16ms | 87.27ms | 1.582× | 2.16% | PASS |
| file_writes | `types_delete_insert` | 51.24ms | 72.92ms | 1.423× | 1.60% | PASS |
| file_writes | `oltp_read_write` | 113.54ms | 164.81ms | 1.452× | 1.40% | PASS |
| ac_reads | `oltp_point_select` | 52.74ms | 60.73ms | 1.151× | 0.79% | PASS |
| ac_reads | `oltp_range_select` | 16.48ms | 16.90ms | 1.026× | 1.33% | PASS |
| ac_reads | `oltp_sum_range` | 14.36ms | 16.34ms | 1.138× | 1.38% | PASS |
| ac_reads | `oltp_order_range` | 3.47ms | 3.49ms | 1.005× | 1.51% | PASS |
| ac_reads | `oltp_distinct_range` | 4.45ms | 4.63ms | 1.040× | 1.50% | PASS |
| ac_reads | `oltp_index_scan` | 7.07ms | 8.79ms | 1.243× | 1.65% | PASS |
| ac_reads | `select_random_points` | 20.80ms | 23.36ms | 1.123× | 1.64% | PASS |
| ac_reads | `select_random_ranges` | 6.52ms | 7.62ms | 1.168× | 1.53% | PASS |
| ac_reads | `covering_index_scan` | 7.23ms | 7.12ms | 0.984× | 3.01% | PASS |
| ac_reads | `groupby_scan` | 33.37ms | 35.82ms | 1.074× | 2.53% | PASS |
| ac_reads | `index_join` | 8.50ms | 11.12ms | 1.308× | 2.58% | PASS |
| ac_reads | `index_join_scan` | 4.66ms | 5.76ms | 1.237× | 1.67% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.23s | 1.166× | 0.57% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.33s | 1.130× | 1.00% | PASS |
| ac_reads | `oltp_read_only` | 152.27ms | 171.70ms | 1.128× | 0.65% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.15ms | 63.66ms | 2.874× | 8.26% | PASS |
| ac_writes | `oltp_insert_ac` | 23.35ms | 87.26ms | 3.737× | 6.49% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.78ms | 96.36ms | 3.738× | 6.08% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.56ms | 76.55ms | 3.393× | 7.09% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.17ms | 87.85ms | 3.792× | 7.26% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.86ms | 88.28ms | 3.551× | 8.52% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.94ms | 85.47ms | 3.726× | 7.97% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.67ms | 93.20ms | 3.039× | 5.53% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 27.46ms | 29.76ms | 1.084× | 2.90% | PASS |
| mem_reads | `oltp_range_select` | 16.67ms | 15.99ms | 0.959× | 3.31% | PASS |
| mem_reads | `oltp_sum_range` | 13.99ms | 15.30ms | 1.094× | 3.51% | PASS |
| mem_reads | `oltp_order_range` | 3.00ms | 3.04ms | 1.013× | 1.82% | PASS |
| mem_reads | `oltp_distinct_range` | 3.80ms | 3.84ms | 1.010× | 1.08% | PASS |
| mem_reads | `oltp_index_scan` | 3.85ms | 4.70ms | 1.221× | 3.31% | PASS |
| mem_reads | `select_random_points` | 22.82ms | 23.71ms | 1.039× | 3.58% | PASS |
| mem_reads | `select_random_ranges` | 6.15ms | 6.52ms | 1.060× | 2.37% | PASS |
| mem_reads | `covering_index_scan` | 3.38ms | 3.58ms | 1.061× | 2.97% | PASS |
| mem_reads | `groupby_scan` | 30.36ms | 30.91ms | 1.018× | 0.84% | PASS |
| mem_reads | `index_join` | 6.48ms | 7.93ms | 1.224× | 2.56% | PASS |
| mem_reads | `index_join_scan` | 3.51ms | 4.60ms | 1.310× | 1.60% | PASS |
| mem_reads | `types_table_scan` | 878.30ms | 983.13ms | 1.119× | 1.17% | PASS |
| mem_reads | `table_scan` | 1.10s | 1.08s | 0.982× | 6.21% | PASS |
| mem_reads | `oltp_read_only` | 127.34ms | 128.92ms | 1.012× | 1.70% | PASS |
| mem_writes | `oltp_bulk_insert` | 188.82ms | 270.33ms | 1.432× | 0.86% | PASS |
| mem_writes | `oltp_insert` | 15.06ms | 28.93ms | 1.921× | 1.23% | PASS |
| mem_writes | `oltp_update_index` | 55.30ms | 94.39ms | 1.707× | 1.69% | PASS |
| mem_writes | `oltp_update_non_index` | 43.07ms | 68.39ms | 1.588× | 2.32% | PASS |
| mem_writes | `oltp_delete_insert` | 41.06ms | 77.20ms | 1.880× | 1.81% | PASS |
| mem_writes | `oltp_write_only` | 23.11ms | 48.87ms | 2.114× | 1.83% | PASS |
| mem_writes | `types_delete_insert` | 28.25ms | 43.90ms | 1.554× | 1.39% | PASS |
| mem_writes | `oltp_read_write` | 84.64ms | 119.23ms | 1.409× | 1.91% | PASS |
| file_reads | `oltp_point_select` | 97.02ms | 53.34ms | 0.550× | 0.93% | PASS |
| file_reads | `oltp_range_select` | 24.02ms | 18.39ms | 0.765× | 2.28% | PASS |
| file_reads | `oltp_sum_range` | 21.83ms | 17.84ms | 0.817× | 1.77% | PASS |
| file_reads | `oltp_order_range` | 3.80ms | 3.31ms | 0.871× | 0.92% | PASS |
| file_reads | `oltp_distinct_range` | 4.63ms | 4.13ms | 0.893× | 0.75% | PASS |
| file_reads | `oltp_index_scan` | 11.23ms | 7.40ms | 0.659× | 1.19% | PASS |
| file_reads | `select_random_points` | 30.01ms | 26.18ms | 0.872× | 2.55% | PASS |
| file_reads | `select_random_ranges` | 13.23ms | 8.98ms | 0.679× | 0.90% | PASS |
| file_reads | `covering_index_scan` | 10.93ms | 6.12ms | 0.559× | 1.13% | PASS |
| file_reads | `groupby_scan` | 31.41ms | 32.19ms | 1.025× | 0.62% | PASS |
| file_reads | `index_join` | 10.74ms | 10.12ms | 0.942× | 2.65% | PASS |
| file_reads | `index_join_scan` | 4.39ms | 5.13ms | 1.167× | 3.15% | PASS |
| file_reads | `types_table_scan` | 902.50ms | 998.78ms | 1.107× | 2.00% | PASS |
| file_reads | `table_scan` | 1.12s | 1.09s | 0.972× | 4.52% | PASS |
| file_reads | `oltp_read_only` | 220.25ms | 159.72ms | 0.725× | 1.70% | PASS |
| file_writes | `oltp_bulk_insert` | 246.49ms | 340.13ms | 1.380× | 8.35% | PASS |
| file_writes | `oltp_insert` | 35.60ms | 52.60ms | 1.478× | 19.34% | PASS |
| file_writes | `oltp_update_index` | 173.27ms | 179.28ms | 1.035× | 4.84% | PASS |
| file_writes | `oltp_update_non_index` | 153.56ms | 133.80ms | 0.871× | 17.45% | PASS |
| file_writes | `oltp_delete_insert` | 157.99ms | 157.30ms | 0.996× | 20.08% | PASS |
| file_writes | `oltp_write_only` | 102.74ms | 107.94ms | 1.051× | 20.93% | PASS |
| file_writes | `types_delete_insert` | 90.18ms | 88.21ms | 0.978× | 10.87% | PASS |
| file_writes | `oltp_read_write` | 157.36ms | 176.93ms | 1.124× | 9.64% | PASS |
| ac_reads | `oltp_point_select` | 47.53ms | 52.52ms | 1.105× | 0.63% | PASS |
| ac_reads | `oltp_range_select` | 18.30ms | 18.17ms | 0.993× | 0.83% | PASS |
| ac_reads | `oltp_sum_range` | 16.38ms | 17.70ms | 1.081× | 0.64% | PASS |
| ac_reads | `oltp_order_range` | 3.33ms | 3.29ms | 0.987× | 0.77% | PASS |
| ac_reads | `oltp_distinct_range` | 4.15ms | 4.14ms | 0.997× | 1.16% | PASS |
| ac_reads | `oltp_index_scan` | 6.41ms | 7.38ms | 1.151× | 1.69% | PASS |
| ac_reads | `select_random_points` | 24.51ms | 26.31ms | 1.073× | 2.18% | PASS |
| ac_reads | `select_random_ranges` | 8.48ms | 9.02ms | 1.064× | 1.17% | PASS |
| ac_reads | `covering_index_scan` | 6.00ms | 6.07ms | 1.012× | 1.16% | PASS |
| ac_reads | `groupby_scan` | 30.51ms | 31.99ms | 1.048× | 0.79% | PASS |
| ac_reads | `index_join` | 8.23ms | 10.10ms | 1.228× | 1.98% | PASS |
| ac_reads | `index_join_scan` | 3.89ms | 5.19ms | 1.334× | 3.37% | PASS |
| ac_reads | `types_table_scan` | 1.01s | 1.04s | 1.030× | 1.10% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.09s | 0.933× | 5.10% | PASS |
| ac_reads | `oltp_read_only` | 156.64ms | 162.38ms | 1.037× | 1.25% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.69ms | 62.01ms | 2.997× | 26.15% | PASS |
| ac_writes | `oltp_insert_ac` | 25.16ms | 81.50ms | 3.240× | 37.50% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.79ms | 233.34ms | 8.105× | 64.50% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.93ms | 80.80ms | 3.242× | 58.98% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 30.53ms | 102.23ms | 3.349× | 53.95% | PASS |
| ac_writes | `oltp_write_only_ac` | 21.99ms | 84.56ms | 3.845× | 21.73% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.82ms | 99.92ms | 4.800× | 49.33% | PASS |
| ac_writes | `oltp_read_write_ac` | 38.58ms | 144.85ms | 3.755× | 37.70% | PASS |

</details>

## Version-control latency

Wall time: 2m 2s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 77.14ms | 200.00ms | 38.6% | 0.30% | PASS |
| `status_dirty_many_tables` | 80.18ms | 200.00ms | 40.1% | 0.38% | PASS |
| `diff_regular_working_one_table` | 72.97ms | 150.00ms | 48.6% | 0.33% | PASS |
| `diff_regular_working_many_tables` | 85.44ms | 200.00ms | 42.7% | 0.24% | PASS |
| `diff_stat_working_many_tables` | 85.53ms | 200.00ms | 42.8% | 0.44% | PASS |
| `diff_schema_working_many_tables` | 86.03ms | 200.00ms | 43.0% | 0.31% | PASS |
| `branch_list_many_branches` | 18.22ms | 100.00ms | 18.2% | 1.23% | PASS |
| `branch_create_delete` | 20.38ms | 100.00ms | 20.4% | 1.68% | PASS |
| `checkout_branch_clean` | 50.06ms | 200.00ms | 25.0% | 0.56% | PASS |
| `merge_data_no_conflicts` | 24.37ms | 150.00ms | 16.2% | 0.93% | PASS |
| `merge_schema_no_conflicts` | 17.67ms | 100.00ms | 17.7% | 1.04% | PASS |
| `merge_data_conflicts` | 108.63ms | 250.00ms | 43.5% | 0.26% | PASS |
| `merge_data_conflicts_with_resolve` | 108.48ms | 250.00ms | 43.4% | 0.18% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
