# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-14 17:22 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu26 20261111.196.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/43613004175)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 40s | 8.77s | 11.53s | 1.314× | 1.77% | **PASS** |
| textpk | 69 | 55 | 57m 27s | 6.73s | 7.89s | 1.172× | 1.93% | **PASS** |
| blobpk | 69 | 55 | 1h 20m 55s | 8.20s | 9.46s | 1.154× | 1.21% | **PASS** |
| compositepk | 69 | 55 | 1h 15m 44s | 8.26s | 9.53s | 1.155× | 1.28% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.50ms | 29.47ms | 1.156× | 2.25% | PASS |
| mem_reads | `oltp_range_select` | 11.05ms | 12.20ms | 1.104× | 2.15% | PASS |
| mem_reads | `oltp_sum_range` | 9.93ms | 12.02ms | 1.211× | 1.96% | PASS |
| mem_reads | `oltp_order_range` | 2.73ms | 3.07ms | 1.125× | 1.41% | PASS |
| mem_reads | `oltp_distinct_range` | 3.69ms | 4.15ms | 1.126× | 1.31% | PASS |
| mem_reads | `oltp_index_scan` | 3.98ms | 5.26ms | 1.322× | 2.58% | PASS |
| mem_reads | `select_random_points` | 11.29ms | 11.24ms | 0.996× | 4.40% | PASS |
| mem_reads | `select_random_ranges` | 3.09ms | 3.96ms | 1.279× | 1.83% | PASS |
| mem_reads | `covering_index_scan` | 4.15ms | 4.16ms | 1.001× | 1.77% | PASS |
| mem_reads | `groupby_scan` | 31.41ms | 35.72ms | 1.137× | 2.71% | PASS |
| mem_reads | `index_join` | 6.02ms | 8.02ms | 1.332× | 1.54% | PASS |
| mem_reads | `index_join_scan` | 3.43ms | 4.53ms | 1.322× | 2.53% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.31s | 1.269× | 0.44% | PASS |
| mem_reads | `table_scan` | 1.16s | 1.44s | 1.248× | 0.92% | PASS |
| mem_reads | `oltp_read_only` | 107.92ms | 124.96ms | 1.158× | 1.25% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.61ms | 252.67ms | 1.407× | 1.06% | PASS |
| mem_writes | `oltp_insert` | 15.26ms | 28.24ms | 1.851× | 0.83% | PASS |
| mem_writes | `oltp_update_index` | 50.64ms | 106.34ms | 2.100× | 1.04% | PASS |
| mem_writes | `oltp_update_non_index` | 35.27ms | 61.81ms | 1.752× | 2.04% | PASS |
| mem_writes | `oltp_delete_insert` | 44.77ms | 80.64ms | 1.801× | 1.39% | PASS |
| mem_writes | `oltp_write_only` | 21.89ms | 46.93ms | 2.144× | 1.66% | PASS |
| mem_writes | `types_delete_insert` | 25.89ms | 42.91ms | 1.657× | 1.25% | PASS |
| mem_writes | `oltp_read_write` | 69.55ms | 111.80ms | 1.607× | 2.02% | PASS |
| file_reads | `oltp_point_select` | 93.98ms | 52.86ms | 0.562× | 0.91% | PASS |
| file_reads | `oltp_range_select` | 17.96ms | 14.71ms | 0.819× | 1.32% | PASS |
| file_reads | `oltp_sum_range` | 17.06ms | 14.69ms | 0.861× | 1.81% | PASS |
| file_reads | `oltp_order_range` | 3.53ms | 3.35ms | 0.949× | 1.78% | PASS |
| file_reads | `oltp_distinct_range` | 4.51ms | 4.47ms | 0.993× | 1.24% | PASS |
| file_reads | `oltp_index_scan` | 11.20ms | 7.97ms | 0.712× | 1.86% | PASS |
| file_reads | `select_random_points` | 18.29ms | 13.76ms | 0.752× | 2.35% | PASS |
| file_reads | `select_random_ranges` | 10.12ms | 6.38ms | 0.630× | 1.45% | PASS |
| file_reads | `covering_index_scan` | 11.24ms | 6.86ms | 0.610× | 1.46% | PASS |
| file_reads | `groupby_scan` | 32.45ms | 35.28ms | 1.087× | 2.39% | PASS |
| file_reads | `index_join` | 9.91ms | 9.95ms | 1.003× | 1.45% | PASS |
| file_reads | `index_join_scan` | 4.39ms | 4.94ms | 1.125× | 2.13% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.35s | 1.303× | 0.45% | PASS |
| file_reads | `table_scan` | 1.16s | 1.44s | 1.245× | 0.67% | PASS |
| file_reads | `oltp_read_only` | 208.10ms | 160.04ms | 0.769× | 0.92% | PASS |
| file_writes | `oltp_bulk_insert` | 191.26ms | 272.27ms | 1.424× | 1.10% | PASS |
| file_writes | `oltp_insert` | 22.09ms | 34.65ms | 1.569× | 1.25% | PASS |
| file_writes | `oltp_update_index` | 77.46ms | 132.64ms | 1.712× | 1.55% | PASS |
| file_writes | `oltp_update_non_index` | 59.33ms | 85.36ms | 1.439× | 1.87% | PASS |
| file_writes | `oltp_delete_insert` | 67.58ms | 105.09ms | 1.555× | 1.23% | PASS |
| file_writes | `oltp_write_only` | 44.70ms | 64.77ms | 1.449× | 1.17% | PASS |
| file_writes | `types_delete_insert` | 41.12ms | 52.77ms | 1.283× | 2.08% | PASS |
| file_writes | `oltp_read_write` | 94.00ms | 129.37ms | 1.376× | 1.71% | PASS |
| ac_reads | `oltp_point_select` | 47.00ms | 52.84ms | 1.124× | 1.35% | PASS |
| ac_reads | `oltp_range_select` | 13.51ms | 14.75ms | 1.092× | 2.43% | PASS |
| ac_reads | `oltp_sum_range` | 12.58ms | 14.69ms | 1.168× | 1.87% | PASS |
| ac_reads | `oltp_order_range` | 3.09ms | 3.35ms | 1.083× | 1.91% | PASS |
| ac_reads | `oltp_distinct_range` | 4.02ms | 4.46ms | 1.111× | 1.28% | PASS |
| ac_reads | `oltp_index_scan` | 6.41ms | 8.01ms | 1.250× | 2.08% | PASS |
| ac_reads | `select_random_points` | 14.00ms | 13.80ms | 0.986× | 3.08% | PASS |
| ac_reads | `select_random_ranges` | 5.44ms | 6.39ms | 1.175× | 1.61% | PASS |
| ac_reads | `covering_index_scan` | 6.59ms | 6.80ms | 1.032× | 2.19% | PASS |
| ac_reads | `groupby_scan` | 31.80ms | 35.66ms | 1.121× | 1.73% | PASS |
| ac_reads | `index_join` | 7.48ms | 9.97ms | 1.332× | 1.85% | PASS |
| ac_reads | `index_join_scan` | 3.90ms | 4.95ms | 1.268× | 2.50% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.35s | 1.301× | 0.62% | PASS |
| ac_reads | `table_scan` | 1.16s | 1.44s | 1.248× | 0.94% | PASS |
| ac_reads | `oltp_read_only` | 140.26ms | 159.37ms | 1.136× | 0.85% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.39ms | 66.05ms | 3.088× | 5.77% | PASS |
| ac_writes | `oltp_insert_ac` | 24.11ms | 81.06ms | 3.363× | 6.96% | PASS |
| ac_writes | `oltp_update_index_ac` | 24.97ms | 94.47ms | 3.784× | 6.40% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.07ms | 74.18ms | 3.361× | 6.31% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.57ms | 85.67ms | 3.796× | 6.15% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.23ms | 83.60ms | 3.599× | 4.64% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.70ms | 74.75ms | 3.611× | 6.33% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.35ms | 91.12ms | 3.105× | 3.68% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 19.04ms | 23.10ms | 1.213× | 3.18% | PASS |
| mem_reads | `oltp_range_select` | 8.53ms | 9.48ms | 1.110× | 9.74% | PASS |
| mem_reads | `oltp_sum_range` | 8.98ms | 9.28ms | 1.033× | 7.46% | PASS |
| mem_reads | `oltp_order_range` | 1.99ms | 2.17ms | 1.087× | 4.36% | PASS |
| mem_reads | `oltp_distinct_range` | 2.66ms | 2.86ms | 1.073× | 3.26% | PASS |
| mem_reads | `oltp_index_scan` | 3.07ms | 4.00ms | 1.303× | 3.18% | PASS |
| mem_reads | `select_random_points` | 13.13ms | 15.79ms | 1.203× | 3.46% | PASS |
| mem_reads | `select_random_ranges` | 2.52ms | 3.39ms | 1.344× | 6.57% | PASS |
| mem_reads | `covering_index_scan` | 3.04ms | 2.76ms | 0.909× | 1.26% | PASS |
| mem_reads | `groupby_scan` | 22.38ms | 24.23ms | 1.083× | 1.04% | PASS |
| mem_reads | `index_join` | 5.25ms | 6.42ms | 1.223× | 1.93% | PASS |
| mem_reads | `index_join_scan` | 2.95ms | 3.83ms | 1.299× | 2.58% | PASS |
| mem_reads | `types_table_scan` | 817.64ms | 890.65ms | 1.089× | 2.39% | PASS |
| mem_reads | `table_scan` | 949.91ms | 968.03ms | 1.019× | 0.82% | PASS |
| mem_reads | `oltp_read_only` | 78.61ms | 88.27ms | 1.123× | 0.93% | PASS |
| mem_writes | `oltp_bulk_insert` | 134.49ms | 206.81ms | 1.538× | 0.41% | PASS |
| mem_writes | `oltp_insert` | 14.34ms | 24.64ms | 1.718× | 0.69% | PASS |
| mem_writes | `oltp_update_index` | 48.75ms | 90.83ms | 1.863× | 1.20% | PASS |
| mem_writes | `oltp_update_non_index` | 30.98ms | 57.37ms | 1.852× | 1.81% | PASS |
| mem_writes | `oltp_delete_insert` | 34.29ms | 68.96ms | 2.011× | 2.07% | PASS |
| mem_writes | `oltp_write_only` | 19.16ms | 41.50ms | 2.165× | 3.33% | PASS |
| mem_writes | `types_delete_insert` | 20.43ms | 36.55ms | 1.789× | 1.81% | PASS |
| mem_writes | `oltp_read_write` | 52.61ms | 89.14ms | 1.694× | 1.17% | PASS |
| file_reads | `oltp_point_select` | 40.86ms | 30.11ms | 0.737× | 0.79% | PASS |
| file_reads | `oltp_range_select` | 11.69ms | 10.26ms | 0.878× | 2.41% | PASS |
| file_reads | `oltp_sum_range` | 11.49ms | 10.08ms | 0.877× | 3.05% | PASS |
| file_reads | `oltp_order_range` | 2.37ms | 2.27ms | 0.959× | 2.56% | PASS |
| file_reads | `oltp_distinct_range` | 3.01ms | 2.95ms | 0.979× | 1.73% | PASS |
| file_reads | `oltp_index_scan` | 5.33ms | 4.70ms | 0.881× | 1.86% | PASS |
| file_reads | `select_random_points` | 15.91ms | 16.60ms | 1.044× | 2.62% | PASS |
| file_reads | `select_random_ranges` | 4.71ms | 4.08ms | 0.866× | 2.59% | PASS |
| file_reads | `covering_index_scan` | 5.25ms | 3.47ms | 0.661× | 0.94% | PASS |
| file_reads | `groupby_scan` | 22.79ms | 24.34ms | 1.068× | 0.81% | PASS |
| file_reads | `index_join` | 6.45ms | 7.05ms | 1.093× | 1.60% | PASS |
| file_reads | `index_join_scan` | 4.23ms | 5.07ms | 1.198× | 5.85% | PASS |
| file_reads | `types_table_scan` | 815.92ms | 897.34ms | 1.100× | 1.96% | PASS |
| file_reads | `table_scan` | 906.00ms | 969.66ms | 1.070× | 1.86% | PASS |
| file_reads | `oltp_read_only` | 108.74ms | 97.50ms | 0.897× | 1.26% | PASS |
| file_writes | `oltp_bulk_insert` | 145.20ms | 223.10ms | 1.537× | 0.76% | PASS |
| file_writes | `oltp_insert` | 22.06ms | 31.70ms | 1.437× | 1.85% | PASS |
| file_writes | `oltp_update_index` | 68.12ms | 108.99ms | 1.600× | 1.93% | PASS |
| file_writes | `oltp_update_non_index` | 47.30ms | 71.63ms | 1.514× | 1.22% | PASS |
| file_writes | `oltp_delete_insert` | 51.61ms | 83.78ms | 1.623× | 1.46% | PASS |
| file_writes | `oltp_write_only` | 33.37ms | 52.74ms | 1.580× | 1.81% | PASS |
| file_writes | `types_delete_insert` | 31.77ms | 44.88ms | 1.413× | 2.13% | PASS |
| file_writes | `oltp_read_write` | 69.41ms | 101.31ms | 1.460× | 1.27% | PASS |
| ac_reads | `oltp_point_select` | 26.11ms | 30.31ms | 1.161× | 1.56% | PASS |
| ac_reads | `oltp_range_select` | 10.55ms | 10.27ms | 0.974× | 3.21% | PASS |
| ac_reads | `oltp_sum_range` | 10.24ms | 10.11ms | 0.987× | 2.78% | PASS |
| ac_reads | `oltp_order_range` | 2.28ms | 2.29ms | 1.004× | 3.53% | PASS |
| ac_reads | `oltp_distinct_range` | 2.92ms | 2.94ms | 1.009× | 2.55% | PASS |
| ac_reads | `oltp_index_scan` | 3.88ms | 4.72ms | 1.215× | 1.78% | PASS |
| ac_reads | `select_random_points` | 14.96ms | 17.01ms | 1.137× | 1.32% | PASS |
| ac_reads | `select_random_ranges` | 3.76ms | 4.23ms | 1.127× | 1.66% | PASS |
| ac_reads | `covering_index_scan` | 3.84ms | 3.59ms | 0.934× | 1.46% | PASS |
| ac_reads | `groupby_scan` | 22.94ms | 24.39ms | 1.063× | 0.96% | PASS |
| ac_reads | `index_join` | 6.04ms | 7.17ms | 1.186× | 1.04% | PASS |
| ac_reads | `index_join_scan` | 4.01ms | 4.70ms | 1.171× | 4.93% | PASS |
| ac_reads | `types_table_scan` | 818.13ms | 895.49ms | 1.095× | 1.30% | PASS |
| ac_reads | `table_scan` | 904.50ms | 971.73ms | 1.074× | 1.55% | PASS |
| ac_reads | `oltp_read_only` | 89.98ms | 97.84ms | 1.087× | 0.52% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 8.61ms | 30.22ms | 3.510× | 5.47% | PASS |
| ac_writes | `oltp_insert_ac` | 10.30ms | 38.78ms | 3.765× | 4.92% | PASS |
| ac_writes | `oltp_update_index_ac` | 11.13ms | 49.69ms | 4.463× | 4.12% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 8.80ms | 36.49ms | 4.146× | 5.03% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 9.86ms | 44.25ms | 4.486× | 2.65% | PASS |
| ac_writes | `oltp_write_only_ac` | 10.05ms | 43.66ms | 4.344× | 2.69% | PASS |
| ac_writes | `types_delete_insert_ac` | 8.44ms | 36.69ms | 4.345× | 4.74% | PASS |
| ac_writes | `oltp_read_write_ac` | 13.16ms | 48.16ms | 3.660× | 3.73% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.42ms | 27.50ms | 1.174× | 1.65% | PASS |
| mem_reads | `oltp_range_select` | 10.90ms | 10.86ms | 0.997× | 2.00% | PASS |
| mem_reads | `oltp_sum_range` | 9.31ms | 10.43ms | 1.120× | 1.14% | PASS |
| mem_reads | `oltp_order_range` | 2.44ms | 2.52ms | 1.032× | 1.51% | PASS |
| mem_reads | `oltp_distinct_range` | 3.25ms | 3.30ms | 1.016× | 1.45% | PASS |
| mem_reads | `oltp_index_scan` | 3.59ms | 4.59ms | 1.279× | 2.12% | PASS |
| mem_reads | `select_random_points` | 13.67ms | 15.76ms | 1.153× | 1.93% | PASS |
| mem_reads | `select_random_ranges` | 3.28ms | 4.17ms | 1.273× | 1.03% | PASS |
| mem_reads | `covering_index_scan` | 3.51ms | 3.42ms | 0.975× | 2.19% | PASS |
| mem_reads | `groupby_scan` | 26.63ms | 27.19ms | 1.021× | 0.82% | PASS |
| mem_reads | `index_join` | 5.48ms | 7.03ms | 1.282× | 1.40% | PASS |
| mem_reads | `index_join_scan` | 3.66ms | 4.75ms | 1.298× | 1.19% | PASS |
| mem_reads | `types_table_scan` | 866.44ms | 975.42ms | 1.126× | 0.44% | PASS |
| mem_reads | `table_scan` | 1.00s | 1.06s | 1.055× | 0.81% | PASS |
| mem_reads | `oltp_read_only` | 92.42ms | 100.73ms | 1.090× | 1.54% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.76ms | 266.25ms | 1.473× | 0.59% | PASS |
| mem_writes | `oltp_insert` | 15.42ms | 31.54ms | 2.045× | 0.94% | PASS |
| mem_writes | `oltp_update_index` | 54.97ms | 106.60ms | 1.939× | 1.22% | PASS |
| mem_writes | `oltp_update_non_index` | 39.16ms | 69.06ms | 1.763× | 1.02% | PASS |
| mem_writes | `oltp_delete_insert` | 39.22ms | 83.90ms | 2.139× | 1.02% | PASS |
| mem_writes | `oltp_write_only` | 22.63ms | 51.87ms | 2.292× | 1.06% | PASS |
| mem_writes | `types_delete_insert` | 26.31ms | 43.79ms | 1.664× | 2.22% | PASS |
| mem_writes | `oltp_read_write` | 64.19ms | 107.14ms | 1.669× | 1.38% | PASS |
| file_reads | `oltp_point_select` | 91.99ms | 49.86ms | 0.542× | 0.69% | PASS |
| file_reads | `oltp_range_select` | 18.34ms | 13.14ms | 0.716× | 1.41% | PASS |
| file_reads | `oltp_sum_range` | 17.03ms | 12.88ms | 0.757× | 0.99% | PASS |
| file_reads | `oltp_order_range` | 3.28ms | 2.82ms | 0.860× | 1.46% | PASS |
| file_reads | `oltp_distinct_range` | 4.07ms | 3.58ms | 0.880× | 1.60% | PASS |
| file_reads | `oltp_index_scan` | 10.98ms | 7.30ms | 0.665× | 1.20% | PASS |
| file_reads | `select_random_points` | 21.31ms | 18.06ms | 0.847× | 1.27% | PASS |
| file_reads | `select_random_ranges` | 10.42ms | 6.52ms | 0.626× | 0.65% | PASS |
| file_reads | `covering_index_scan` | 11.36ms | 6.17ms | 0.543× | 1.13% | PASS |
| file_reads | `groupby_scan` | 27.56ms | 27.55ms | 1.000× | 0.76% | PASS |
| file_reads | `index_join` | 9.68ms | 9.12ms | 0.942× | 1.61% | PASS |
| file_reads | `index_join_scan` | 4.46ms | 5.03ms | 1.127× | 1.17% | PASS |
| file_reads | `types_table_scan` | 865.40ms | 977.58ms | 1.130× | 0.51% | PASS |
| file_reads | `table_scan` | 1.01s | 1.05s | 1.049× | 0.75% | PASS |
| file_reads | `oltp_read_only` | 191.01ms | 133.22ms | 0.697× | 0.61% | PASS |
| file_writes | `oltp_bulk_insert` | 228.87ms | 337.57ms | 1.475× | 2.26% | PASS |
| file_writes | `oltp_insert` | 49.08ms | 57.51ms | 1.172× | 6.43% | PASS |
| file_writes | `oltp_update_index` | 180.21ms | 202.32ms | 1.123× | 2.63% | PASS |
| file_writes | `oltp_update_non_index` | 139.74ms | 137.90ms | 0.987× | 0.28% | PASS |
| file_writes | `oltp_delete_insert` | 140.29ms | 156.55ms | 1.116× | 1.91% | PASS |
| file_writes | `oltp_write_only` | 100.09ms | 108.21ms | 1.081× | 1.05% | PASS |
| file_writes | `types_delete_insert` | 89.64ms | 87.08ms | 0.971× | 9.15% | PASS |
| file_writes | `oltp_read_write` | 143.11ms | 164.65ms | 1.150× | 2.36% | PASS |
| ac_reads | `oltp_point_select` | 44.59ms | 49.83ms | 1.117× | 0.66% | PASS |
| ac_reads | `oltp_range_select` | 13.73ms | 13.17ms | 0.960× | 1.21% | PASS |
| ac_reads | `oltp_sum_range` | 12.55ms | 12.98ms | 1.035× | 1.23% | PASS |
| ac_reads | `oltp_order_range` | 2.89ms | 2.83ms | 0.979× | 1.25% | PASS |
| ac_reads | `oltp_distinct_range` | 3.66ms | 3.60ms | 0.983× | 1.03% | PASS |
| ac_reads | `oltp_index_scan` | 6.35ms | 7.31ms | 1.151× | 0.83% | PASS |
| ac_reads | `select_random_points` | 16.75ms | 18.12ms | 1.082× | 0.67% | PASS |
| ac_reads | `select_random_ranges` | 5.77ms | 6.55ms | 1.135× | 0.92% | PASS |
| ac_reads | `covering_index_scan` | 6.70ms | 6.17ms | 0.922× | 0.76% | PASS |
| ac_reads | `groupby_scan` | 27.13ms | 27.55ms | 1.015× | 0.48% | PASS |
| ac_reads | `index_join` | 7.44ms | 9.09ms | 1.223× | 0.62% | PASS |
| ac_reads | `index_join_scan` | 4.07ms | 5.06ms | 1.243× | 1.45% | PASS |
| ac_reads | `types_table_scan` | 866.67ms | 977.28ms | 1.128× | 0.69% | PASS |
| ac_reads | `table_scan` | 1.01s | 1.05s | 1.047× | 0.84% | PASS |
| ac_reads | `oltp_read_only` | 123.18ms | 133.38ms | 1.083× | 0.80% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 19.32ms | 49.75ms | 2.575× | 8.22% | PASS |
| ac_writes | `oltp_insert_ac` | 23.15ms | 69.14ms | 2.986× | 8.41% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.03ms | 78.19ms | 3.124× | 5.56% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 19.02ms | 61.39ms | 3.228× | 7.80% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.12ms | 70.94ms | 3.207× | 6.26% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.13ms | 70.45ms | 3.183× | 8.89% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.54ms | 60.84ms | 3.679× | 8.43% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.74ms | 76.03ms | 2.954× | 6.58% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.11ms | 29.26ms | 1.120× | 1.27% | PASS |
| mem_reads | `oltp_range_select` | 15.41ms | 15.61ms | 1.013× | 1.64% | PASS |
| mem_reads | `oltp_sum_range` | 13.71ms | 15.20ms | 1.108× | 1.28% | PASS |
| mem_reads | `oltp_order_range` | 2.91ms | 2.98ms | 1.026× | 0.80% | PASS |
| mem_reads | `oltp_distinct_range` | 3.71ms | 3.77ms | 1.016× | 1.10% | PASS |
| mem_reads | `oltp_index_scan` | 3.69ms | 4.49ms | 1.217× | 1.82% | PASS |
| mem_reads | `select_random_points` | 20.63ms | 23.18ms | 1.124× | 1.50% | PASS |
| mem_reads | `select_random_ranges` | 5.93ms | 6.45ms | 1.088× | 1.70% | PASS |
| mem_reads | `covering_index_scan` | 3.33ms | 3.20ms | 0.961× | 1.51% | PASS |
| mem_reads | `groupby_scan` | 30.05ms | 30.90ms | 1.028× | 0.64% | PASS |
| mem_reads | `index_join` | 6.34ms | 7.83ms | 1.234× | 1.33% | PASS |
| mem_reads | `index_join_scan` | 3.34ms | 4.65ms | 1.394× | 1.45% | PASS |
| mem_reads | `types_table_scan` | 864.82ms | 977.64ms | 1.130× | 0.62% | PASS |
| mem_reads | `table_scan` | 987.39ms | 1.06s | 1.070× | 0.59% | PASS |
| mem_reads | `oltp_read_only` | 115.18ms | 123.56ms | 1.073× | 1.06% | PASS |
| mem_writes | `oltp_bulk_insert` | 187.75ms | 265.63ms | 1.415× | 0.99% | PASS |
| mem_writes | `oltp_insert` | 14.72ms | 28.57ms | 1.941× | 0.74% | PASS |
| mem_writes | `oltp_update_index` | 54.02ms | 94.21ms | 1.744× | 1.13% | PASS |
| mem_writes | `oltp_update_non_index` | 41.30ms | 67.63ms | 1.638× | 1.11% | PASS |
| mem_writes | `oltp_delete_insert` | 39.72ms | 77.22ms | 1.944× | 1.18% | PASS |
| mem_writes | `oltp_write_only` | 21.90ms | 48.37ms | 2.209× | 1.42% | PASS |
| mem_writes | `types_delete_insert` | 26.09ms | 42.82ms | 1.641× | 1.09% | PASS |
| mem_writes | `oltp_read_write` | 77.06ms | 115.98ms | 1.505× | 1.40% | PASS |
| file_reads | `oltp_point_select` | 94.40ms | 52.23ms | 0.553× | 0.59% | PASS |
| file_reads | `oltp_range_select` | 22.88ms | 18.03ms | 0.788× | 1.62% | PASS |
| file_reads | `oltp_sum_range` | 20.91ms | 17.55ms | 0.839× | 1.44% | PASS |
| file_reads | `oltp_order_range` | 3.69ms | 3.28ms | 0.888× | 2.06% | PASS |
| file_reads | `oltp_distinct_range` | 4.50ms | 4.10ms | 0.911× | 1.61% | PASS |
| file_reads | `oltp_index_scan` | 10.74ms | 7.25ms | 0.675× | 1.36% | PASS |
| file_reads | `select_random_points` | 28.10ms | 25.58ms | 0.910× | 1.14% | PASS |
| file_reads | `select_random_ranges` | 12.94ms | 8.90ms | 0.688× | 1.19% | PASS |
| file_reads | `covering_index_scan` | 10.51ms | 5.99ms | 0.570× | 1.11% | PASS |
| file_reads | `groupby_scan` | 30.89ms | 32.06ms | 1.038× | 1.35% | PASS |
| file_reads | `index_join` | 10.32ms | 9.82ms | 0.952× | 1.52% | PASS |
| file_reads | `index_join_scan` | 4.20ms | 4.91ms | 1.169× | 1.95% | PASS |
| file_reads | `types_table_scan` | 867.08ms | 980.88ms | 1.131× | 0.72% | PASS |
| file_reads | `table_scan` | 990.35ms | 1.05s | 1.062× | 0.59% | PASS |
| file_reads | `oltp_read_only` | 213.84ms | 157.03ms | 0.734× | 0.86% | PASS |
| file_writes | `oltp_bulk_insert` | 236.75ms | 325.44ms | 1.375× | 2.53% | PASS |
| file_writes | `oltp_insert` | 25.65ms | 46.83ms | 1.826× | 9.74% | PASS |
| file_writes | `oltp_update_index` | 151.42ms | 160.16ms | 1.058× | 0.98% | PASS |
| file_writes | `oltp_update_non_index` | 130.97ms | 120.73ms | 0.922× | 2.19% | PASS |
| file_writes | `oltp_delete_insert` | 130.95ms | 131.31ms | 1.003× | 3.13% | PASS |
| file_writes | `oltp_write_only` | 91.16ms | 98.78ms | 1.084× | 1.88% | PASS |
| file_writes | `types_delete_insert` | 80.33ms | 79.47ms | 0.989× | 2.39% | PASS |
| file_writes | `oltp_read_write` | 143.93ms | 168.97ms | 1.174× | 2.93% | PASS |
| ac_reads | `oltp_point_select` | 47.16ms | 51.90ms | 1.100× | 0.72% | PASS |
| ac_reads | `oltp_range_select` | 18.31ms | 18.11ms | 0.989× | 1.40% | PASS |
| ac_reads | `oltp_sum_range` | 16.40ms | 17.61ms | 1.074× | 1.16% | PASS |
| ac_reads | `oltp_order_range` | 3.30ms | 3.27ms | 0.993× | 0.89% | PASS |
| ac_reads | `oltp_distinct_range` | 4.09ms | 4.10ms | 1.004× | 1.12% | PASS |
| ac_reads | `oltp_index_scan` | 6.25ms | 7.24ms | 1.158× | 0.77% | PASS |
| ac_reads | `select_random_points` | 23.46ms | 25.56ms | 1.089× | 1.63% | PASS |
| ac_reads | `select_random_ranges` | 8.35ms | 8.86ms | 1.060× | 0.89% | PASS |
| ac_reads | `covering_index_scan` | 5.94ms | 6.02ms | 1.013× | 0.81% | PASS |
| ac_reads | `groupby_scan` | 30.58ms | 31.97ms | 1.046× | 0.68% | PASS |
| ac_reads | `index_join` | 8.00ms | 9.90ms | 1.238× | 1.17% | PASS |
| ac_reads | `index_join_scan` | 3.77ms | 4.91ms | 1.302× | 0.84% | PASS |
| ac_reads | `types_table_scan` | 868.66ms | 984.31ms | 1.133× | 0.72% | PASS |
| ac_reads | `table_scan` | 991.64ms | 1.05s | 1.061× | 0.82% | PASS |
| ac_reads | `oltp_read_only` | 147.06ms | 156.92ms | 1.067× | 0.97% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.97ms | 56.04ms | 2.673× | 11.13% | PASS |
| ac_writes | `oltp_insert_ac` | 24.00ms | 76.89ms | 3.203× | 8.50% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.32ms | 87.15ms | 3.311× | 10.14% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.21ms | 68.80ms | 3.404× | 10.50% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.73ms | 76.20ms | 3.212× | 5.88% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.14ms | 78.34ms | 3.245× | 7.94% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.22ms | 67.10ms | 2.771× | 23.28% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.59ms | 82.14ms | 2.977× | 9.71% | PASS |

</details>

## Version-control latency

Wall time: 1m 54s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 61.01ms | 200.00ms | 30.5% | 0.38% | PASS |
| `status_dirty_many_tables` | 63.57ms | 200.00ms | 31.8% | 0.76% | PASS |
| `diff_regular_working_one_table` | 58.25ms | 150.00ms | 38.8% | 0.36% | PASS |
| `diff_regular_working_many_tables` | 68.20ms | 200.00ms | 34.1% | 0.32% | PASS |
| `diff_stat_working_many_tables` | 68.31ms | 200.00ms | 34.2% | 0.48% | PASS |
| `diff_schema_working_many_tables` | 68.63ms | 200.00ms | 34.3% | 0.45% | PASS |
| `branch_list_many_branches` | 16.58ms | 100.00ms | 16.6% | 2.83% | PASS |
| `branch_create_delete` | 24.08ms | 100.00ms | 24.1% | 7.31% | PASS |
| `checkout_branch_clean` | 86.33ms | 200.00ms | 43.2% | 6.83% | PASS |
| `merge_data_no_conflicts` | 29.53ms | 150.00ms | 19.7% | 3.73% | PASS |
| `merge_schema_no_conflicts` | 15.96ms | 100.00ms | 16.0% | 3.49% | PASS |
| `merge_data_conflicts` | 94.44ms | 250.00ms | 37.8% | 0.25% | PASS |
| `merge_data_conflicts_with_resolve` | 94.56ms | 250.00ms | 37.8% | 0.41% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
