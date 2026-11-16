# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-16 21:17 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu26 20261111.196.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/43942797054)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 10m 19s | 8.76s | 10.62s | 1.212× | 1.31% | **PASS** |
| textpk | 69 | 55 | 1h 10m 24s | 7.06s | 7.76s | 1.099× | 2.35% | **PASS** |
| blobpk | 69 | 55 | 1h 25m 25s | 8.89s | 9.92s | 1.116× | 1.64% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 26s | 9.72s | 11.91s | 1.226× | 1.66% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.54ms | 24.30ms | 1.078× | 1.28% | PASS |
| mem_reads | `oltp_range_select` | 9.72ms | 11.69ms | 1.203× | 1.97% | PASS |
| mem_reads | `oltp_sum_range` | 9.28ms | 10.96ms | 1.181× | 1.32% | PASS |
| mem_reads | `oltp_order_range` | 2.53ms | 2.81ms | 1.109× | 1.55% | PASS |
| mem_reads | `oltp_distinct_range` | 3.37ms | 3.77ms | 1.120× | 1.08% | PASS |
| mem_reads | `oltp_index_scan` | 3.70ms | 4.55ms | 1.230× | 2.03% | PASS |
| mem_reads | `select_random_points` | 11.32ms | 10.95ms | 0.967× | 1.31% | PASS |
| mem_reads | `select_random_ranges` | 3.07ms | 3.45ms | 1.122× | 1.62% | PASS |
| mem_reads | `covering_index_scan` | 3.42ms | 3.51ms | 1.025× | 1.66% | PASS |
| mem_reads | `groupby_scan` | 30.86ms | 33.44ms | 1.083× | 0.49% | PASS |
| mem_reads | `index_join` | 5.42ms | 7.22ms | 1.334× | 1.79% | PASS |
| mem_reads | `index_join_scan` | 2.97ms | 4.18ms | 1.407× | 1.32% | PASS |
| mem_reads | `types_table_scan` | 1.01s | 1.20s | 1.190× | 0.47% | PASS |
| mem_reads | `table_scan` | 1.16s | 1.30s | 1.118× | 0.67% | PASS |
| mem_reads | `oltp_read_only` | 96.20ms | 109.97ms | 1.143× | 1.13% | PASS |
| mem_writes | `oltp_bulk_insert` | 142.21ms | 200.94ms | 1.413× | 0.48% | PASS |
| mem_writes | `oltp_insert` | 12.95ms | 23.52ms | 1.816× | 0.69% | PASS |
| mem_writes | `oltp_update_index` | 46.36ms | 95.10ms | 2.051× | 1.37% | PASS |
| mem_writes | `oltp_update_non_index` | 30.27ms | 53.08ms | 1.754× | 1.62% | PASS |
| mem_writes | `oltp_delete_insert` | 40.44ms | 69.58ms | 1.720× | 0.93% | PASS |
| mem_writes | `oltp_write_only` | 19.43ms | 40.31ms | 2.074× | 1.31% | PASS |
| mem_writes | `types_delete_insert` | 22.35ms | 36.19ms | 1.619× | 1.34% | PASS |
| mem_writes | `oltp_read_write` | 59.86ms | 96.64ms | 1.614× | 1.93% | PASS |
| file_reads | `oltp_point_select` | 51.87ms | 33.35ms | 0.643× | 0.95% | PASS |
| file_reads | `oltp_range_select` | 13.17ms | 12.67ms | 0.962× | 1.15% | PASS |
| file_reads | `oltp_sum_range` | 12.82ms | 12.17ms | 0.950× | 1.67% | PASS |
| file_reads | `oltp_order_range` | 2.94ms | 2.97ms | 1.013× | 1.36% | PASS |
| file_reads | `oltp_distinct_range` | 3.79ms | 3.94ms | 1.041× | 1.03% | PASS |
| file_reads | `oltp_index_scan` | 6.93ms | 5.80ms | 0.837× | 1.30% | PASS |
| file_reads | `select_random_points` | 14.92ms | 12.15ms | 0.814× | 1.34% | PASS |
| file_reads | `select_random_ranges` | 6.11ms | 4.41ms | 0.722× | 1.00% | PASS |
| file_reads | `covering_index_scan` | 6.62ms | 4.78ms | 0.722× | 1.35% | PASS |
| file_reads | `groupby_scan` | 31.33ms | 33.63ms | 1.074× | 0.48% | PASS |
| file_reads | `index_join` | 7.31ms | 8.42ms | 1.151× | 1.37% | PASS |
| file_reads | `index_join_scan` | 3.44ms | 4.44ms | 1.290× | 1.00% | PASS |
| file_reads | `types_table_scan` | 1.01s | 1.21s | 1.195× | 0.48% | PASS |
| file_reads | `table_scan` | 1.16s | 1.30s | 1.125× | 0.93% | PASS |
| file_reads | `oltp_read_only` | 137.67ms | 122.08ms | 0.887× | 0.83% | PASS |
| file_writes | `oltp_bulk_insert` | 190.13ms | 260.93ms | 1.372× | 4.07% | PASS |
| file_writes | `oltp_insert` | 27.71ms | 37.67ms | 1.359× | 27.99% | PASS |
| file_writes | `oltp_update_index` | 141.10ms | 160.10ms | 1.135× | 4.50% | PASS |
| file_writes | `oltp_update_non_index` | 110.46ms | 109.26ms | 0.989× | 1.22% | PASS |
| file_writes | `oltp_delete_insert` | 126.39ms | 126.59ms | 1.002× | 4.74% | PASS |
| file_writes | `oltp_write_only` | 90.12ms | 88.67ms | 0.984× | 4.23% | PASS |
| file_writes | `types_delete_insert` | 75.62ms | 69.97ms | 0.925× | 2.35% | PASS |
| file_writes | `oltp_read_write` | 123.27ms | 143.58ms | 1.165× | 2.56% | PASS |
| ac_reads | `oltp_point_select` | 32.06ms | 33.33ms | 1.040× | 0.70% | PASS |
| ac_reads | `oltp_range_select` | 11.02ms | 12.66ms | 1.149× | 0.97% | PASS |
| ac_reads | `oltp_sum_range` | 10.62ms | 12.13ms | 1.143× | 0.74% | PASS |
| ac_reads | `oltp_order_range` | 2.71ms | 2.96ms | 1.093× | 0.70% | PASS |
| ac_reads | `oltp_distinct_range` | 3.59ms | 3.94ms | 1.097× | 0.76% | PASS |
| ac_reads | `oltp_index_scan` | 5.06ms | 5.78ms | 1.142× | 1.18% | PASS |
| ac_reads | `select_random_points` | 13.03ms | 12.14ms | 0.931× | 0.94% | PASS |
| ac_reads | `select_random_ranges` | 4.24ms | 4.41ms | 1.041× | 1.05% | PASS |
| ac_reads | `covering_index_scan` | 4.60ms | 4.78ms | 1.039× | 1.32% | PASS |
| ac_reads | `groupby_scan` | 31.18ms | 33.67ms | 1.080× | 0.45% | PASS |
| ac_reads | `index_join` | 6.41ms | 8.44ms | 1.317× | 1.66% | PASS |
| ac_reads | `index_join_scan` | 3.27ms | 4.47ms | 1.364× | 1.21% | PASS |
| ac_reads | `types_table_scan` | 1.01s | 1.21s | 1.199× | 0.35% | PASS |
| ac_reads | `table_scan` | 1.16s | 1.31s | 1.122× | 0.88% | PASS |
| ac_reads | `oltp_read_only` | 109.06ms | 122.53ms | 1.123× | 1.00% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 27.95ms | 75.68ms | 2.708× | 3.85% | PASS |
| ac_writes | `oltp_insert_ac` | 30.45ms | 88.95ms | 2.921× | 4.37% | PASS |
| ac_writes | `oltp_update_index_ac` | 33.26ms | 100.95ms | 3.035× | 5.09% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 28.18ms | 83.34ms | 2.958× | 3.57% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 29.96ms | 96.85ms | 3.233× | 5.25% | PASS |
| ac_writes | `oltp_write_only_ac` | 29.84ms | 92.58ms | 3.103× | 4.58% | PASS |
| ac_writes | `types_delete_insert_ac` | 30.86ms | 84.98ms | 2.753× | 6.46% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.70ms | 100.01ms | 2.967× | 4.74% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 18.06ms | 21.52ms | 1.191× | 4.55% | PASS |
| mem_reads | `oltp_range_select` | 8.20ms | 8.38ms | 1.022× | 3.99% | PASS |
| mem_reads | `oltp_sum_range` | 7.84ms | 8.53ms | 1.087× | 4.06% | PASS |
| mem_reads | `oltp_order_range` | 1.90ms | 1.98ms | 1.039× | 2.80% | PASS |
| mem_reads | `oltp_distinct_range` | 2.47ms | 2.48ms | 1.004× | 2.17% | PASS |
| mem_reads | `oltp_index_scan` | 2.66ms | 3.69ms | 1.387× | 2.56% | PASS |
| mem_reads | `select_random_points` | 11.73ms | 13.26ms | 1.131× | 4.51% | PASS |
| mem_reads | `select_random_ranges` | 2.49ms | 3.30ms | 1.325× | 5.37% | PASS |
| mem_reads | `covering_index_scan` | 2.53ms | 2.62ms | 1.039× | 1.83% | PASS |
| mem_reads | `groupby_scan` | 20.11ms | 21.04ms | 1.046× | 1.79% | PASS |
| mem_reads | `index_join` | 4.33ms | 5.35ms | 1.235× | 1.45% | PASS |
| mem_reads | `index_join_scan` | 2.38ms | 4.21ms | 1.770× | 4.54% | PASS |
| mem_reads | `types_table_scan` | 694.91ms | 741.18ms | 1.067× | 1.35% | PASS |
| mem_reads | `table_scan` | 807.58ms | 842.12ms | 1.043× | 1.23% | PASS |
| mem_reads | `oltp_read_only` | 67.21ms | 73.16ms | 1.089× | 1.54% | PASS |
| mem_writes | `oltp_bulk_insert` | 136.89ms | 207.85ms | 1.518× | 1.32% | PASS |
| mem_writes | `oltp_insert` | 12.81ms | 25.48ms | 1.989× | 1.92% | PASS |
| mem_writes | `oltp_update_index` | 43.16ms | 89.37ms | 2.071× | 1.81% | PASS |
| mem_writes | `oltp_update_non_index` | 31.52ms | 60.29ms | 1.913× | 2.14% | PASS |
| mem_writes | `oltp_delete_insert` | 30.90ms | 68.58ms | 2.219× | 2.03% | PASS |
| mem_writes | `oltp_write_only` | 18.23ms | 43.72ms | 2.398× | 1.41% | PASS |
| mem_writes | `types_delete_insert` | 20.73ms | 37.56ms | 1.812× | 2.07% | PASS |
| mem_writes | `oltp_read_write` | 47.87ms | 81.67ms | 1.706× | 1.24% | PASS |
| file_reads | `oltp_point_select` | 71.05ms | 39.49ms | 0.556× | 1.32% | PASS |
| file_reads | `oltp_range_select` | 14.35ms | 10.01ms | 0.698× | 2.02% | PASS |
| file_reads | `oltp_sum_range` | 14.53ms | 10.42ms | 0.717× | 1.97% | PASS |
| file_reads | `oltp_order_range` | 2.82ms | 2.26ms | 0.800× | 1.44% | PASS |
| file_reads | `oltp_distinct_range` | 3.40ms | 2.78ms | 0.818× | 1.47% | PASS |
| file_reads | `oltp_index_scan` | 9.11ms | 6.25ms | 0.686× | 1.97% | PASS |
| file_reads | `select_random_points` | 19.37ms | 15.67ms | 0.809× | 3.53% | PASS |
| file_reads | `select_random_ranges` | 8.36ms | 5.32ms | 0.636× | 2.06% | PASS |
| file_reads | `covering_index_scan` | 9.46ms | 5.05ms | 0.534× | 2.05% | PASS |
| file_reads | `groupby_scan` | 22.63ms | 21.39ms | 0.945× | 2.35% | PASS |
| file_reads | `index_join` | 8.64ms | 7.82ms | 0.905× | 3.74% | PASS |
| file_reads | `index_join_scan` | 4.23ms | 4.51ms | 1.065× | 3.01% | PASS |
| file_reads | `types_table_scan` | 719.35ms | 763.89ms | 1.062× | 3.41% | PASS |
| file_reads | `table_scan` | 971.38ms | 864.07ms | 0.890× | 4.14% | PASS |
| file_reads | `oltp_read_only` | 143.34ms | 96.78ms | 0.675× | 2.04% | PASS |
| file_writes | `oltp_bulk_insert` | 199.39ms | 293.63ms | 1.473× | 4.06% | PASS |
| file_writes | `oltp_insert` | 50.23ms | 54.88ms | 1.093× | 8.38% | PASS |
| file_writes | `oltp_update_index` | 191.77ms | 189.53ms | 0.988× | 4.45% | PASS |
| file_writes | `oltp_update_non_index` | 140.92ms | 128.58ms | 0.912× | 2.13% | PASS |
| file_writes | `oltp_delete_insert` | 158.25ms | 149.13ms | 0.942× | 2.40% | PASS |
| file_writes | `oltp_write_only` | 121.19ms | 108.19ms | 0.893× | 3.11% | PASS |
| file_writes | `types_delete_insert` | 99.88ms | 82.92ms | 0.830× | 7.21% | PASS |
| file_writes | `oltp_read_write` | 151.15ms | 147.83ms | 0.978× | 1.28% | PASS |
| ac_reads | `oltp_point_select` | 35.97ms | 39.88ms | 1.109× | 1.26% | PASS |
| ac_reads | `oltp_range_select` | 11.41ms | 10.36ms | 0.908× | 2.51% | PASS |
| ac_reads | `oltp_sum_range` | 11.59ms | 10.74ms | 0.927× | 2.82% | PASS |
| ac_reads | `oltp_order_range` | 2.49ms | 2.25ms | 0.905× | 1.54% | PASS |
| ac_reads | `oltp_distinct_range` | 3.01ms | 2.70ms | 0.896× | 1.07% | PASS |
| ac_reads | `oltp_index_scan` | 5.57ms | 6.27ms | 1.127× | 1.48% | PASS |
| ac_reads | `select_random_points` | 14.75ms | 15.31ms | 1.038× | 2.92% | PASS |
| ac_reads | `select_random_ranges` | 4.58ms | 5.09ms | 1.113× | 2.12% | PASS |
| ac_reads | `covering_index_scan` | 4.90ms | 4.67ms | 0.952× | 4.20% | PASS |
| ac_reads | `groupby_scan` | 20.66ms | 20.55ms | 0.995× | 1.81% | PASS |
| ac_reads | `index_join` | 5.90ms | 6.54ms | 1.107× | 3.69% | PASS |
| ac_reads | `index_join_scan` | 3.87ms | 4.38ms | 1.131× | 1.94% | PASS |
| ac_reads | `types_table_scan` | 716.63ms | 761.76ms | 1.063× | 1.53% | PASS |
| ac_reads | `table_scan` | 810.10ms | 833.07ms | 1.028× | 4.03% | PASS |
| ac_reads | `oltp_read_only` | 91.43ms | 96.34ms | 1.054× | 2.39% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.72ms | 59.08ms | 2.851× | 9.04% | PASS |
| ac_writes | `oltp_insert_ac` | 25.50ms | 72.23ms | 2.833× | 10.75% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.22ms | 83.88ms | 3.199× | 8.04% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.96ms | 64.65ms | 2.943× | 5.61% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.58ms | 71.02ms | 3.146× | 7.67% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.33ms | 69.55ms | 3.115× | 7.31% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.27ms | 67.99ms | 3.197× | 7.98% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.77ms | 74.63ms | 2.897× | 5.38% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.80ms | 27.47ms | 1.154× | 2.00% | PASS |
| mem_reads | `oltp_range_select` | 11.30ms | 11.01ms | 0.974× | 1.69% | PASS |
| mem_reads | `oltp_sum_range` | 9.83ms | 10.64ms | 1.082× | 1.48% | PASS |
| mem_reads | `oltp_order_range` | 2.53ms | 2.55ms | 1.009× | 1.29% | PASS |
| mem_reads | `oltp_distinct_range` | 3.22ms | 3.28ms | 1.020× | 0.91% | PASS |
| mem_reads | `oltp_index_scan` | 3.58ms | 4.54ms | 1.268× | 2.90% | PASS |
| mem_reads | `select_random_points` | 13.88ms | 16.00ms | 1.152× | 1.58% | PASS |
| mem_reads | `select_random_ranges` | 3.37ms | 4.19ms | 1.245× | 1.56% | PASS |
| mem_reads | `covering_index_scan` | 3.59ms | 3.76ms | 1.047× | 1.78% | PASS |
| mem_reads | `groupby_scan` | 27.43ms | 27.40ms | 0.999× | 0.72% | PASS |
| mem_reads | `index_join` | 5.53ms | 7.81ms | 1.413× | 2.45% | PASS |
| mem_reads | `index_join_scan` | 3.68ms | 4.82ms | 1.307× | 1.83% | PASS |
| mem_reads | `types_table_scan` | 963.68ms | 1.02s | 1.062× | 2.04% | PASS |
| mem_reads | `table_scan` | 1.25s | 1.12s | 0.898× | 1.90% | PASS |
| mem_reads | `oltp_read_only` | 102.11ms | 105.33ms | 1.032× | 1.28% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.73ms | 266.24ms | 1.481× | 0.90% | PASS |
| mem_writes | `oltp_insert` | 15.55ms | 32.00ms | 2.057× | 0.97% | PASS |
| mem_writes | `oltp_update_index` | 56.47ms | 111.07ms | 1.967× | 1.63% | PASS |
| mem_writes | `oltp_update_non_index` | 40.18ms | 70.29ms | 1.749× | 1.43% | PASS |
| mem_writes | `oltp_delete_insert` | 40.64ms | 86.71ms | 2.134× | 1.66% | PASS |
| mem_writes | `oltp_write_only` | 22.21ms | 51.35ms | 2.312× | 1.02% | PASS |
| mem_writes | `types_delete_insert` | 24.86ms | 43.38ms | 1.745× | 1.13% | PASS |
| mem_writes | `oltp_read_write` | 63.27ms | 106.50ms | 1.683× | 1.90% | PASS |
| file_reads | `oltp_point_select` | 91.83ms | 49.64ms | 0.541× | 0.56% | PASS |
| file_reads | `oltp_range_select` | 18.09ms | 13.26ms | 0.733× | 1.11% | PASS |
| file_reads | `oltp_sum_range` | 16.79ms | 12.88ms | 0.767× | 1.19% | PASS |
| file_reads | `oltp_order_range` | 3.30ms | 2.83ms | 0.856× | 1.75% | PASS |
| file_reads | `oltp_distinct_range` | 4.09ms | 3.60ms | 0.880× | 0.81% | PASS |
| file_reads | `oltp_index_scan` | 10.90ms | 7.25ms | 0.665× | 0.74% | PASS |
| file_reads | `select_random_points` | 21.30ms | 18.13ms | 0.851× | 1.22% | PASS |
| file_reads | `select_random_ranges` | 10.39ms | 6.52ms | 0.627× | 0.64% | PASS |
| file_reads | `covering_index_scan` | 11.36ms | 6.15ms | 0.542× | 0.64% | PASS |
| file_reads | `groupby_scan` | 28.12ms | 27.55ms | 0.980× | 0.74% | PASS |
| file_reads | `index_join` | 9.88ms | 9.21ms | 0.932× | 1.25% | PASS |
| file_reads | `index_join_scan` | 4.48ms | 5.10ms | 1.137× | 1.69% | PASS |
| file_reads | `types_table_scan` | 906.75ms | 999.06ms | 1.102× | 1.90% | PASS |
| file_reads | `table_scan` | 1.15s | 1.11s | 0.960× | 5.78% | PASS |
| file_reads | `oltp_read_only` | 200.53ms | 137.71ms | 0.687× | 1.19% | PASS |
| file_writes | `oltp_bulk_insert` | 229.20ms | 338.24ms | 1.476× | 3.62% | PASS |
| file_writes | `oltp_insert` | 48.22ms | 57.37ms | 1.190× | 2.41% | PASS |
| file_writes | `oltp_update_index` | 180.38ms | 199.56ms | 1.106× | 5.08% | PASS |
| file_writes | `oltp_update_non_index` | 139.75ms | 137.94ms | 0.987× | 1.68% | PASS |
| file_writes | `oltp_delete_insert` | 140.55ms | 158.48ms | 1.128× | 7.68% | PASS |
| file_writes | `oltp_write_only` | 118.44ms | 126.05ms | 1.064× | 37.25% | PASS |
| file_writes | `types_delete_insert` | 89.76ms | 97.55ms | 1.087× | 8.51% | PASS |
| file_writes | `oltp_read_write` | 148.00ms | 167.47ms | 1.132× | 6.13% | PASS |
| ac_reads | `oltp_point_select` | 44.77ms | 50.06ms | 1.118× | 1.01% | PASS |
| ac_reads | `oltp_range_select` | 13.99ms | 13.39ms | 0.957× | 1.72% | PASS |
| ac_reads | `oltp_sum_range` | 12.72ms | 13.11ms | 1.030× | 1.22% | PASS |
| ac_reads | `oltp_order_range` | 2.94ms | 2.85ms | 0.971× | 1.19% | PASS |
| ac_reads | `oltp_distinct_range` | 3.70ms | 3.62ms | 0.977× | 0.84% | PASS |
| ac_reads | `oltp_index_scan` | 6.43ms | 7.38ms | 1.147× | 0.82% | PASS |
| ac_reads | `select_random_points` | 17.52ms | 18.97ms | 1.083× | 1.42% | PASS |
| ac_reads | `select_random_ranges` | 5.74ms | 6.53ms | 1.137× | 0.75% | PASS |
| ac_reads | `covering_index_scan` | 6.76ms | 6.20ms | 0.916× | 0.75% | PASS |
| ac_reads | `groupby_scan` | 27.71ms | 27.67ms | 0.999× | 0.87% | PASS |
| ac_reads | `index_join` | 7.50ms | 9.11ms | 1.215× | 1.08% | PASS |
| ac_reads | `index_join_scan` | 4.12ms | 5.10ms | 1.237× | 1.64% | PASS |
| ac_reads | `types_table_scan` | 896.20ms | 997.79ms | 1.113× | 1.72% | PASS |
| ac_reads | `table_scan` | 1.04s | 1.06s | 1.021× | 3.76% | PASS |
| ac_reads | `oltp_read_only` | 129.68ms | 136.02ms | 1.049× | 2.06% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.40ms | 77.22ms | 3.165× | 50.54% | PASS |
| ac_writes | `oltp_insert_ac` | 26.47ms | 84.10ms | 3.178× | 23.04% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.62ms | 94.44ms | 3.420× | 17.23% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.89ms | 88.03ms | 3.845× | 35.55% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 36.38ms | 124.00ms | 3.408× | 50.32% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.92ms | 86.50ms | 3.471× | 29.35% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.91ms | 83.54ms | 3.646× | 64.39% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.56ms | 93.95ms | 3.289× | 33.70% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.37ms | 40.98ms | 1.192× | 1.45% | PASS |
| mem_reads | `oltp_range_select` | 20.90ms | 21.68ms | 1.037× | 2.13% | PASS |
| mem_reads | `oltp_sum_range` | 18.50ms | 21.27ms | 1.150× | 1.41% | PASS |
| mem_reads | `oltp_order_range` | 3.72ms | 4.03ms | 1.083× | 1.72% | PASS |
| mem_reads | `oltp_distinct_range` | 4.76ms | 5.15ms | 1.082× | 1.14% | PASS |
| mem_reads | `oltp_index_scan` | 4.71ms | 6.10ms | 1.295× | 1.59% | PASS |
| mem_reads | `select_random_points` | 27.63ms | 31.02ms | 1.123× | 1.68% | PASS |
| mem_reads | `select_random_ranges` | 7.84ms | 8.98ms | 1.146× | 1.59% | PASS |
| mem_reads | `covering_index_scan` | 4.27ms | 4.29ms | 1.005× | 2.42% | PASS |
| mem_reads | `groupby_scan` | 38.09ms | 42.03ms | 1.103× | 1.98% | PASS |
| mem_reads | `index_join` | 8.32ms | 10.57ms | 1.270× | 1.56% | PASS |
| mem_reads | `index_join_scan` | 4.30ms | 5.51ms | 1.281× | 2.56% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.25s | 1.185× | 0.51% | PASS |
| mem_reads | `table_scan` | 1.21s | 1.35s | 1.120× | 1.21% | PASS |
| mem_reads | `oltp_read_only` | 155.75ms | 172.92ms | 1.110× | 1.03% | PASS |
| mem_writes | `oltp_bulk_insert` | 244.00ms | 362.25ms | 1.485× | 0.88% | PASS |
| mem_writes | `oltp_insert` | 18.92ms | 37.58ms | 1.986× | 0.87% | PASS |
| mem_writes | `oltp_update_index` | 69.10ms | 121.43ms | 1.757× | 1.46% | PASS |
| mem_writes | `oltp_update_non_index` | 52.19ms | 87.09ms | 1.669× | 0.92% | PASS |
| mem_writes | `oltp_delete_insert` | 50.68ms | 100.68ms | 1.987× | 0.95% | PASS |
| mem_writes | `oltp_write_only` | 27.86ms | 62.01ms | 2.225× | 0.64% | PASS |
| mem_writes | `types_delete_insert` | 33.96ms | 57.33ms | 1.688× | 1.27% | PASS |
| mem_writes | `oltp_read_write` | 106.93ms | 162.98ms | 1.524× | 1.52% | PASS |
| file_reads | `oltp_point_select` | 105.55ms | 64.40ms | 0.610× | 0.97% | PASS |
| file_reads | `oltp_range_select` | 28.96ms | 24.39ms | 0.842× | 1.66% | PASS |
| file_reads | `oltp_sum_range` | 26.88ms | 24.36ms | 0.906× | 1.41% | PASS |
| file_reads | `oltp_order_range` | 4.99ms | 4.70ms | 0.942× | 3.67% | PASS |
| file_reads | `oltp_distinct_range` | 6.09ms | 5.79ms | 0.951× | 2.48% | PASS |
| file_reads | `oltp_index_scan` | 12.36ms | 8.89ms | 0.719× | 1.59% | PASS |
| file_reads | `select_random_points` | 36.98ms | 34.74ms | 0.939× | 2.17% | PASS |
| file_reads | `select_random_ranges` | 15.50ms | 11.78ms | 0.760× | 1.46% | PASS |
| file_reads | `covering_index_scan` | 11.96ms | 7.12ms | 0.595× | 2.32% | PASS |
| file_reads | `groupby_scan` | 40.21ms | 43.37ms | 1.079× | 1.94% | PASS |
| file_reads | `index_join` | 13.02ms | 12.87ms | 0.989× | 2.31% | PASS |
| file_reads | `index_join_scan` | 5.46ms | 6.12ms | 1.121× | 2.71% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.25s | 1.177× | 0.88% | PASS |
| file_reads | `table_scan` | 1.23s | 1.35s | 1.104× | 1.61% | PASS |
| file_reads | `oltp_read_only` | 261.55ms | 212.21ms | 0.811× | 0.82% | PASS |
| file_writes | `oltp_bulk_insert` | 258.94ms | 384.43ms | 1.485× | 0.95% | PASS |
| file_writes | `oltp_insert` | 26.61ms | 48.46ms | 1.821× | 1.62% | PASS |
| file_writes | `oltp_update_index` | 101.16ms | 151.79ms | 1.501× | 1.69% | PASS |
| file_writes | `oltp_update_non_index` | 80.34ms | 109.87ms | 1.368× | 1.54% | PASS |
| file_writes | `oltp_delete_insert` | 79.32ms | 126.52ms | 1.595× | 1.69% | PASS |
| file_writes | `oltp_write_only` | 52.99ms | 82.07ms | 1.549× | 1.42% | PASS |
| file_writes | `types_delete_insert` | 51.35ms | 71.47ms | 1.392× | 1.27% | PASS |
| file_writes | `oltp_read_write` | 132.99ms | 183.15ms | 1.377× | 1.86% | PASS |
| ac_reads | `oltp_point_select` | 57.99ms | 64.47ms | 1.112× | 1.15% | PASS |
| ac_reads | `oltp_range_select` | 24.04ms | 24.47ms | 1.018× | 2.05% | PASS |
| ac_reads | `oltp_sum_range` | 21.57ms | 24.21ms | 1.123× | 1.70% | PASS |
| ac_reads | `oltp_order_range` | 4.39ms | 4.52ms | 1.031× | 2.46% | PASS |
| ac_reads | `oltp_distinct_range` | 5.68ms | 5.78ms | 1.018× | 2.78% | PASS |
| ac_reads | `oltp_index_scan` | 7.44ms | 8.91ms | 1.199× | 1.91% | PASS |
| ac_reads | `select_random_points` | 31.15ms | 34.78ms | 1.117× | 1.67% | PASS |
| ac_reads | `select_random_ranges` | 10.76ms | 11.84ms | 1.101× | 1.36% | PASS |
| ac_reads | `covering_index_scan` | 7.07ms | 7.10ms | 1.004× | 2.42% | PASS |
| ac_reads | `groupby_scan` | 39.06ms | 43.05ms | 1.102× | 1.50% | PASS |
| ac_reads | `index_join` | 10.20ms | 12.79ms | 1.254× | 1.76% | PASS |
| ac_reads | `index_join_scan` | 4.81ms | 6.03ms | 1.254× | 1.85% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.24s | 1.178× | 0.82% | PASS |
| ac_reads | `table_scan` | 1.22s | 1.36s | 1.115× | 1.75% | PASS |
| ac_reads | `oltp_read_only` | 191.96ms | 211.41ms | 1.101× | 1.48% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.78ms | 62.08ms | 2.988× | 5.27% | PASS |
| ac_writes | `oltp_insert_ac` | 23.81ms | 85.46ms | 3.589× | 4.89% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.27ms | 97.73ms | 3.720× | 6.00% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.78ms | 74.22ms | 3.408× | 5.18% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.02ms | 87.78ms | 3.812× | 5.84% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.89ms | 86.56ms | 3.623× | 4.81% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.48ms | 76.95ms | 3.583× | 8.51% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.12ms | 96.86ms | 3.112× | 6.46% | PASS |

</details>

## Version-control latency

Wall time: 1m 20s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 47.26ms | 200.00ms | 23.6% | 0.86% | PASS |
| `status_dirty_many_tables` | 49.23ms | 200.00ms | 24.6% | 0.42% | PASS |
| `diff_regular_working_one_table` | 44.27ms | 150.00ms | 29.5% | 0.52% | PASS |
| `diff_regular_working_many_tables` | 52.52ms | 200.00ms | 26.3% | 0.70% | PASS |
| `diff_stat_working_many_tables` | 52.38ms | 200.00ms | 26.2% | 0.57% | PASS |
| `diff_schema_working_many_tables` | 52.83ms | 200.00ms | 26.4% | 0.59% | PASS |
| `branch_list_many_branches` | 12.55ms | 100.00ms | 12.6% | 1.02% | PASS |
| `branch_create_delete` | 13.59ms | 100.00ms | 13.6% | 1.11% | PASS |
| `checkout_branch_clean` | 31.64ms | 200.00ms | 15.8% | 0.59% | PASS |
| `merge_data_no_conflicts` | 15.91ms | 150.00ms | 10.6% | 0.51% | PASS |
| `merge_schema_no_conflicts` | 11.98ms | 100.00ms | 12.0% | 0.65% | PASS |
| `merge_data_conflicts` | 56.88ms | 250.00ms | 22.8% | 0.63% | PASS |
| `merge_data_conflicts_with_resolve` | 57.08ms | 250.00ms | 22.8% | 0.57% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
