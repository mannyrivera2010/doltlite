# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-05 20:32 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260927.320.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/37358365251)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 3m 48s | 7.63s | 8.84s | 1.159× | 0.96% | **PASS** |
| textpk | 69 | 55 | 1h 10m 5s | 7.08s | 7.76s | 1.096× | 2.33% | **PASS** |
| blobpk | 69 | 55 | 1h 33m 41s | 10.43s | 12.06s | 1.156× | 1.00% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 26s | 9.73s | 11.97s | 1.230× | 1.29% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 19.32ms | 21.65ms | 1.121× | 1.75% | PASS |
| mem_reads | `oltp_range_select` | 8.32ms | 9.25ms | 1.111× | 1.18% | PASS |
| mem_reads | `oltp_sum_range` | 7.67ms | 8.91ms | 1.161× | 1.21% | PASS |
| mem_reads | `oltp_order_range` | 2.10ms | 2.22ms | 1.059× | 1.10% | PASS |
| mem_reads | `oltp_distinct_range` | 2.96ms | 3.03ms | 1.026× | 0.71% | PASS |
| mem_reads | `oltp_index_scan` | 3.13ms | 3.83ms | 1.224× | 1.20% | PASS |
| mem_reads | `select_random_points` | 8.37ms | 8.77ms | 1.048× | 0.92% | PASS |
| mem_reads | `select_random_ranges` | 2.46ms | 3.09ms | 1.258× | 1.29% | PASS |
| mem_reads | `covering_index_scan` | 3.39ms | 3.17ms | 0.935× | 1.59% | PASS |
| mem_reads | `groupby_scan` | 24.90ms | 26.85ms | 1.078× | 0.64% | PASS |
| mem_reads | `index_join` | 4.67ms | 6.05ms | 1.297× | 0.90% | PASS |
| mem_reads | `index_join_scan` | 2.78ms | 3.75ms | 1.348× | 0.90% | PASS |
| mem_reads | `types_table_scan` | 860.23ms | 981.98ms | 1.142× | 0.53% | PASS |
| mem_reads | `table_scan` | 973.86ms | 1.07s | 1.096× | 0.35% | PASS |
| mem_reads | `oltp_read_only` | 80.65ms | 89.50ms | 1.110× | 0.89% | PASS |
| mem_writes | `oltp_bulk_insert` | 139.25ms | 185.03ms | 1.329× | 0.83% | PASS |
| mem_writes | `oltp_insert` | 12.27ms | 21.14ms | 1.723× | 0.83% | PASS |
| mem_writes | `oltp_update_index` | 39.61ms | 78.75ms | 1.988× | 0.63% | PASS |
| mem_writes | `oltp_update_non_index` | 27.29ms | 43.47ms | 1.593× | 1.02% | PASS |
| mem_writes | `oltp_delete_insert` | 35.08ms | 58.69ms | 1.673× | 0.80% | PASS |
| mem_writes | `oltp_write_only` | 17.46ms | 33.19ms | 1.901× | 0.96% | PASS |
| mem_writes | `types_delete_insert` | 19.12ms | 29.39ms | 1.537× | 1.05% | PASS |
| mem_writes | `oltp_read_write` | 50.49ms | 79.48ms | 1.574× | 0.85% | PASS |
| file_reads | `oltp_point_select` | 93.44ms | 46.43ms | 0.497× | 0.67% | PASS |
| file_reads | `oltp_range_select` | 15.77ms | 11.87ms | 0.753× | 1.45% | PASS |
| file_reads | `oltp_sum_range` | 15.09ms | 11.61ms | 0.769× | 1.66% | PASS |
| file_reads | `oltp_order_range` | 2.88ms | 2.57ms | 0.893× | 1.88% | PASS |
| file_reads | `oltp_distinct_range` | 3.70ms | 3.35ms | 0.906× | 1.45% | PASS |
| file_reads | `oltp_index_scan` | 10.78ms | 6.77ms | 0.628× | 1.35% | PASS |
| file_reads | `select_random_points` | 15.80ms | 11.86ms | 0.750× | 1.85% | PASS |
| file_reads | `select_random_ranges` | 9.85ms | 5.70ms | 0.578× | 1.03% | PASS |
| file_reads | `covering_index_scan` | 11.09ms | 6.04ms | 0.545× | 1.01% | PASS |
| file_reads | `groupby_scan` | 25.65ms | 27.22ms | 1.061× | 0.77% | PASS |
| file_reads | `index_join` | 8.78ms | 8.08ms | 0.921× | 1.04% | PASS |
| file_reads | `index_join_scan` | 3.62ms | 4.16ms | 1.147× | 1.44% | PASS |
| file_reads | `types_table_scan` | 859.32ms | 980.91ms | 1.141× | 0.60% | PASS |
| file_reads | `table_scan` | 975.88ms | 1.07s | 1.093× | 0.35% | PASS |
| file_reads | `oltp_read_only` | 186.55ms | 124.52ms | 0.667× | 0.62% | PASS |
| file_writes | `oltp_bulk_insert` | 180.20ms | 238.63ms | 1.324× | 3.75% | PASS |
| file_writes | `oltp_insert` | 25.69ms | 34.59ms | 1.346× | 7.11% | PASS |
| file_writes | `oltp_update_index` | 131.26ms | 129.46ms | 0.986× | 0.42% | PASS |
| file_writes | `oltp_update_non_index` | 100.76ms | 98.46ms | 0.977× | 0.58% | PASS |
| file_writes | `oltp_delete_insert` | 110.96ms | 108.77ms | 0.980× | 0.27% | PASS |
| file_writes | `oltp_write_only` | 80.91ms | 79.10ms | 0.978× | 4.34% | PASS |
| file_writes | `types_delete_insert` | 60.60ms | 59.82ms | 0.987× | 7.41% | PASS |
| file_writes | `oltp_read_write` | 120.50ms | 123.40ms | 1.024× | 4.68% | PASS |
| ac_reads | `oltp_point_select` | 43.44ms | 46.32ms | 1.066× | 0.76% | PASS |
| ac_reads | `oltp_range_select` | 11.33ms | 11.93ms | 1.053× | 0.68% | PASS |
| ac_reads | `oltp_sum_range` | 10.55ms | 11.61ms | 1.100× | 0.75% | PASS |
| ac_reads | `oltp_order_range` | 2.53ms | 2.57ms | 1.019× | 0.87% | PASS |
| ac_reads | `oltp_distinct_range` | 3.32ms | 3.35ms | 1.008× | 0.89% | PASS |
| ac_reads | `oltp_index_scan` | 5.97ms | 6.79ms | 1.138× | 0.85% | PASS |
| ac_reads | `select_random_points` | 11.47ms | 11.83ms | 1.031× | 0.57% | PASS |
| ac_reads | `select_random_ranges` | 5.10ms | 5.67ms | 1.112× | 0.79% | PASS |
| ac_reads | `covering_index_scan` | 6.22ms | 6.04ms | 0.971× | 0.61% | PASS |
| ac_reads | `groupby_scan` | 25.29ms | 27.25ms | 1.077× | 0.53% | PASS |
| ac_reads | `index_join` | 6.41ms | 8.12ms | 1.266× | 1.28% | PASS |
| ac_reads | `index_join_scan` | 3.23ms | 4.14ms | 1.280× | 1.09% | PASS |
| ac_reads | `types_table_scan` | 858.67ms | 981.36ms | 1.143× | 0.53% | PASS |
| ac_reads | `table_scan` | 974.92ms | 1.07s | 1.094× | 0.40% | PASS |
| ac_reads | `oltp_read_only` | 115.35ms | 124.87ms | 1.082× | 0.56% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.69ms | 52.55ms | 3.149× | 6.42% | PASS |
| ac_writes | `oltp_insert_ac` | 19.14ms | 70.30ms | 3.673× | 8.17% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.77ms | 83.91ms | 4.041× | 13.54% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.77ms | 64.54ms | 3.632× | 10.49% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.75ms | 71.97ms | 3.837× | 6.53% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.00ms | 72.25ms | 3.802× | 7.35% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.36ms | 66.50ms | 4.065× | 5.41% | PASS |
| ac_writes | `oltp_read_write_ac` | 23.36ms | 80.55ms | 3.448× | 9.19% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.00ms | 23.74ms | 1.079× | 2.47% | PASS |
| mem_reads | `oltp_range_select` | 9.71ms | 8.88ms | 0.914× | 4.01% | PASS |
| mem_reads | `oltp_sum_range` | 9.98ms | 9.20ms | 0.922× | 4.43% | PASS |
| mem_reads | `oltp_order_range` | 2.24ms | 2.01ms | 0.897× | 2.85% | PASS |
| mem_reads | `oltp_distinct_range` | 2.73ms | 2.51ms | 0.920× | 2.44% | PASS |
| mem_reads | `oltp_index_scan` | 3.23ms | 4.33ms | 1.339× | 6.43% | PASS |
| mem_reads | `select_random_points` | 14.38ms | 15.31ms | 1.065× | 5.18% | PASS |
| mem_reads | `select_random_ranges` | 3.20ms | 3.52ms | 1.098× | 2.88% | PASS |
| mem_reads | `covering_index_scan` | 3.38ms | 3.30ms | 0.976× | 13.20% | PASS |
| mem_reads | `groupby_scan` | 20.49ms | 19.99ms | 0.975× | 2.33% | PASS |
| mem_reads | `index_join` | 5.15ms | 7.24ms | 1.405× | 6.29% | PASS |
| mem_reads | `index_join_scan` | 3.98ms | 4.52ms | 1.136× | 4.20% | PASS |
| mem_reads | `types_table_scan` | 702.71ms | 750.41ms | 1.068× | 1.96% | PASS |
| mem_reads | `table_scan` | 816.82ms | 851.50ms | 1.042× | 1.94% | PASS |
| mem_reads | `oltp_read_only` | 72.23ms | 75.40ms | 1.044× | 3.01% | PASS |
| mem_writes | `oltp_bulk_insert` | 144.45ms | 205.76ms | 1.424× | 2.05% | PASS |
| mem_writes | `oltp_insert` | 12.82ms | 23.85ms | 1.861× | 1.89% | PASS |
| mem_writes | `oltp_update_index` | 43.44ms | 86.23ms | 1.985× | 1.62% | PASS |
| mem_writes | `oltp_update_non_index` | 32.52ms | 57.87ms | 1.780× | 1.89% | PASS |
| mem_writes | `oltp_delete_insert` | 32.02ms | 67.31ms | 2.102× | 2.12% | PASS |
| mem_writes | `oltp_write_only` | 18.91ms | 42.30ms | 2.236× | 2.06% | PASS |
| mem_writes | `types_delete_insert` | 22.62ms | 36.71ms | 1.623× | 2.49% | PASS |
| mem_writes | `oltp_read_write` | 49.58ms | 80.73ms | 1.628× | 2.02% | PASS |
| file_reads | `oltp_point_select` | 77.05ms | 42.00ms | 0.545× | 1.46% | PASS |
| file_reads | `oltp_range_select` | 15.20ms | 10.63ms | 0.700× | 1.48% | PASS |
| file_reads | `oltp_sum_range` | 14.81ms | 10.58ms | 0.715× | 1.94% | PASS |
| file_reads | `oltp_order_range` | 2.82ms | 2.20ms | 0.781× | 1.70% | PASS |
| file_reads | `oltp_distinct_range` | 3.32ms | 2.67ms | 0.804× | 2.48% | PASS |
| file_reads | `oltp_index_scan` | 9.27ms | 6.00ms | 0.647× | 1.78% | PASS |
| file_reads | `select_random_points` | 18.71ms | 15.26ms | 0.816× | 1.68% | PASS |
| file_reads | `select_random_ranges` | 8.98ms | 5.52ms | 0.615× | 1.51% | PASS |
| file_reads | `covering_index_scan` | 9.96ms | 5.23ms | 0.525× | 2.12% | PASS |
| file_reads | `groupby_scan` | 20.76ms | 20.01ms | 0.964× | 1.90% | PASS |
| file_reads | `index_join` | 8.73ms | 7.75ms | 0.888× | 1.74% | PASS |
| file_reads | `index_join_scan` | 4.34ms | 4.55ms | 1.046× | 2.71% | PASS |
| file_reads | `types_table_scan` | 825.43ms | 786.45ms | 0.953× | 4.42% | PASS |
| file_reads | `table_scan` | 864.67ms | 851.69ms | 0.985× | 6.28% | PASS |
| file_reads | `oltp_read_only` | 146.20ms | 99.08ms | 0.678× | 2.16% | PASS |
| file_writes | `oltp_bulk_insert` | 200.90ms | 282.45ms | 1.406× | 4.52% | PASS |
| file_writes | `oltp_insert` | 49.92ms | 48.89ms | 0.979× | 5.06% | PASS |
| file_writes | `oltp_update_index` | 191.74ms | 179.30ms | 0.935× | 4.05% | PASS |
| file_writes | `oltp_update_non_index` | 141.94ms | 120.04ms | 0.846× | 3.19% | PASS |
| file_writes | `oltp_delete_insert` | 152.01ms | 141.20ms | 0.929× | 2.00% | PASS |
| file_writes | `oltp_write_only` | 119.39ms | 103.92ms | 0.870× | 3.10% | PASS |
| file_writes | `types_delete_insert` | 100.36ms | 79.99ms | 0.797× | 5.88% | PASS |
| file_writes | `oltp_read_write` | 151.13ms | 144.97ms | 0.959× | 2.51% | PASS |
| ac_reads | `oltp_point_select` | 36.51ms | 40.13ms | 1.099× | 1.49% | PASS |
| ac_reads | `oltp_range_select` | 10.55ms | 10.21ms | 0.968× | 1.13% | PASS |
| ac_reads | `oltp_sum_range` | 10.25ms | 10.20ms | 0.996× | 1.16% | PASS |
| ac_reads | `oltp_order_range` | 2.33ms | 2.14ms | 0.919× | 1.41% | PASS |
| ac_reads | `oltp_distinct_range` | 2.81ms | 2.61ms | 0.930× | 1.35% | PASS |
| ac_reads | `oltp_index_scan` | 5.12ms | 5.96ms | 1.164× | 2.56% | PASS |
| ac_reads | `select_random_points` | 14.35ms | 15.03ms | 1.047× | 1.88% | PASS |
| ac_reads | `select_random_ranges` | 4.84ms | 5.32ms | 1.099× | 1.22% | PASS |
| ac_reads | `covering_index_scan` | 5.06ms | 4.79ms | 0.946× | 2.09% | PASS |
| ac_reads | `groupby_scan` | 18.96ms | 19.00ms | 1.002× | 1.24% | PASS |
| ac_reads | `index_join` | 5.90ms | 7.27ms | 1.232× | 2.44% | PASS |
| ac_reads | `index_join_scan` | 3.75ms | 4.34ms | 1.159× | 1.30% | PASS |
| ac_reads | `types_table_scan` | 673.72ms | 724.02ms | 1.075× | 0.59% | PASS |
| ac_reads | `table_scan` | 811.15ms | 846.43ms | 1.043× | 1.45% | PASS |
| ac_reads | `oltp_read_only` | 96.85ms | 102.62ms | 1.060× | 1.20% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.64ms | 57.46ms | 2.656× | 5.41% | PASS |
| ac_writes | `oltp_insert_ac` | 24.40ms | 72.05ms | 2.952× | 7.69% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.33ms | 89.45ms | 3.397× | 10.13% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.24ms | 73.99ms | 3.183× | 8.61% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.12ms | 85.49ms | 3.403× | 8.14% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.63ms | 82.56ms | 3.352× | 9.57% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.44ms | 67.34ms | 3.141× | 8.07% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.38ms | 77.94ms | 2.955× | 6.20% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.81ms | 39.60ms | 1.138× | 1.53% | PASS |
| mem_reads | `oltp_range_select` | 14.84ms | 15.38ms | 1.037× | 1.61% | PASS |
| mem_reads | `oltp_sum_range` | 13.73ms | 14.91ms | 1.085× | 1.14% | PASS |
| mem_reads | `oltp_order_range` | 3.49ms | 3.73ms | 1.070× | 1.48% | PASS |
| mem_reads | `oltp_distinct_range` | 4.73ms | 4.86ms | 1.026× | 0.75% | PASS |
| mem_reads | `oltp_index_scan` | 5.24ms | 6.68ms | 1.274× | 1.15% | PASS |
| mem_reads | `select_random_points` | 19.34ms | 21.82ms | 1.128× | 1.00% | PASS |
| mem_reads | `select_random_ranges` | 4.58ms | 5.85ms | 1.277× | 1.03% | PASS |
| mem_reads | `covering_index_scan` | 5.26ms | 4.95ms | 0.942× | 1.21% | PASS |
| mem_reads | `groupby_scan` | 38.73ms | 40.81ms | 1.054× | 0.67% | PASS |
| mem_reads | `index_join` | 7.71ms | 9.82ms | 1.275× | 0.86% | PASS |
| mem_reads | `index_join_scan` | 4.77ms | 5.97ms | 1.253× | 0.76% | PASS |
| mem_reads | `types_table_scan` | 1.30s | 1.44s | 1.108× | 0.61% | PASS |
| mem_reads | `table_scan` | 1.49s | 1.57s | 1.049× | 0.48% | PASS |
| mem_reads | `oltp_read_only` | 134.14ms | 149.03ms | 1.111× | 0.64% | PASS |
| mem_writes | `oltp_bulk_insert` | 277.73ms | 382.89ms | 1.379× | 0.48% | PASS |
| mem_writes | `oltp_insert` | 23.57ms | 44.76ms | 1.899× | 0.41% | PASS |
| mem_writes | `oltp_update_index` | 77.74ms | 147.43ms | 1.896× | 0.74% | PASS |
| mem_writes | `oltp_update_non_index` | 55.52ms | 96.04ms | 1.730× | 0.86% | PASS |
| mem_writes | `oltp_delete_insert` | 55.92ms | 117.54ms | 2.102× | 0.56% | PASS |
| mem_writes | `oltp_write_only` | 31.58ms | 71.63ms | 2.268× | 0.63% | PASS |
| mem_writes | `types_delete_insert` | 36.40ms | 59.80ms | 1.643× | 0.95% | PASS |
| mem_writes | `oltp_read_write` | 91.25ms | 152.35ms | 1.670× | 0.92% | PASS |
| file_reads | `oltp_point_select` | 127.26ms | 67.29ms | 0.529× | 0.73% | PASS |
| file_reads | `oltp_range_select` | 22.53ms | 16.57ms | 0.736× | 2.88% | PASS |
| file_reads | `oltp_sum_range` | 22.10ms | 16.24ms | 0.735× | 1.87% | PASS |
| file_reads | `oltp_order_range` | 4.09ms | 3.60ms | 0.880× | 3.15% | PASS |
| file_reads | `oltp_distinct_range` | 5.12ms | 4.58ms | 0.895× | 2.21% | PASS |
| file_reads | `oltp_index_scan` | 14.44ms | 9.18ms | 0.636× | 1.64% | PASS |
| file_reads | `select_random_points` | 27.33ms | 22.79ms | 0.834× | 1.92% | PASS |
| file_reads | `select_random_ranges` | 13.80ms | 8.41ms | 0.609× | 1.83% | PASS |
| file_reads | `covering_index_scan` | 14.57ms | 7.85ms | 0.539× | 2.48% | PASS |
| file_reads | `groupby_scan` | 34.50ms | 35.53ms | 1.030× | 0.72% | PASS |
| file_reads | `index_join` | 12.09ms | 11.10ms | 0.918× | 1.57% | PASS |
| file_reads | `index_join_scan` | 5.53ms | 5.96ms | 1.078× | 2.39% | PASS |
| file_reads | `types_table_scan` | 1.12s | 1.24s | 1.100× | 0.81% | PASS |
| file_reads | `table_scan` | 1.31s | 1.36s | 1.036× | 2.47% | PASS |
| file_reads | `oltp_read_only` | 256.53ms | 174.34ms | 0.680× | 0.47% | PASS |
| file_writes | `oltp_bulk_insert` | 259.61ms | 357.13ms | 1.376× | 0.82% | PASS |
| file_writes | `oltp_insert` | 31.52ms | 51.62ms | 1.638× | 1.47% | PASS |
| file_writes | `oltp_update_index` | 105.38ms | 164.57ms | 1.562× | 0.94% | PASS |
| file_writes | `oltp_update_non_index` | 79.36ms | 108.91ms | 1.372× | 0.95% | PASS |
| file_writes | `oltp_delete_insert` | 81.95ms | 131.72ms | 1.607× | 0.92% | PASS |
| file_writes | `oltp_write_only` | 55.66ms | 86.03ms | 1.546× | 1.22% | PASS |
| file_writes | `types_delete_insert` | 52.00ms | 71.26ms | 1.370× | 1.39% | PASS |
| file_writes | `oltp_read_write` | 110.63ms | 156.11ms | 1.411× | 1.05% | PASS |
| ac_reads | `oltp_point_select` | 61.27ms | 66.03ms | 1.078× | 1.04% | PASS |
| ac_reads | `oltp_range_select` | 16.94ms | 16.54ms | 0.977× | 1.13% | PASS |
| ac_reads | `oltp_sum_range` | 15.73ms | 16.29ms | 1.036× | 0.90% | PASS |
| ac_reads | `oltp_order_range` | 3.47ms | 3.62ms | 1.044× | 0.87% | PASS |
| ac_reads | `oltp_distinct_range` | 4.48ms | 4.60ms | 1.027× | 0.93% | PASS |
| ac_reads | `oltp_index_scan` | 8.04ms | 9.21ms | 1.145× | 1.00% | PASS |
| ac_reads | `select_random_points` | 21.24ms | 22.84ms | 1.075× | 0.73% | PASS |
| ac_reads | `select_random_ranges` | 7.41ms | 8.39ms | 1.133× | 0.92% | PASS |
| ac_reads | `covering_index_scan` | 8.42ms | 7.86ms | 0.934× | 0.93% | PASS |
| ac_reads | `groupby_scan` | 33.94ms | 35.58ms | 1.048× | 0.54% | PASS |
| ac_reads | `index_join` | 9.13ms | 11.08ms | 1.213× | 0.85% | PASS |
| ac_reads | `index_join_scan` | 4.93ms | 5.96ms | 1.209× | 1.58% | PASS |
| ac_reads | `types_table_scan` | 1.12s | 1.23s | 1.101× | 0.50% | PASS |
| ac_reads | `table_scan` | 1.29s | 1.35s | 1.041× | 0.27% | PASS |
| ac_reads | `oltp_read_only` | 164.76ms | 174.62ms | 1.060× | 0.55% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.63ms | 51.40ms | 3.288× | 3.83% | PASS |
| ac_writes | `oltp_insert_ac` | 17.93ms | 73.81ms | 4.117× | 3.13% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.46ms | 84.89ms | 4.363× | 3.41% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.91ms | 64.99ms | 3.844× | 4.02% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.96ms | 79.26ms | 4.414× | 6.50% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.93ms | 76.21ms | 4.026× | 4.37% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.44ms | 66.92ms | 4.336× | 5.95% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.07ms | 83.64ms | 3.337× | 2.79% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.41ms | 41.21ms | 1.198× | 1.74% | PASS |
| mem_reads | `oltp_range_select` | 19.28ms | 21.32ms | 1.106× | 1.61% | PASS |
| mem_reads | `oltp_sum_range` | 18.03ms | 20.92ms | 1.160× | 1.04% | PASS |
| mem_reads | `oltp_order_range` | 3.58ms | 3.86ms | 1.078× | 1.07% | PASS |
| mem_reads | `oltp_distinct_range` | 4.68ms | 4.90ms | 1.048× | 0.66% | PASS |
| mem_reads | `oltp_index_scan` | 4.64ms | 6.33ms | 1.366× | 1.78% | PASS |
| mem_reads | `select_random_points` | 28.82ms | 32.86ms | 1.140× | 1.33% | PASS |
| mem_reads | `select_random_ranges` | 7.90ms | 9.13ms | 1.157× | 1.16% | PASS |
| mem_reads | `covering_index_scan` | 4.26ms | 4.32ms | 1.013× | 2.16% | PASS |
| mem_reads | `groupby_scan` | 36.40ms | 39.22ms | 1.077× | 0.79% | PASS |
| mem_reads | `index_join` | 8.21ms | 10.60ms | 1.290× | 1.88% | PASS |
| mem_reads | `index_join_scan` | 4.25ms | 5.54ms | 1.302× | 2.25% | PASS |
| mem_reads | `types_table_scan` | 1.08s | 1.25s | 1.158× | 2.56% | PASS |
| mem_reads | `table_scan` | 1.29s | 1.41s | 1.090× | 2.02% | PASS |
| mem_reads | `oltp_read_only` | 153.80ms | 169.89ms | 1.105× | 1.16% | PASS |
| mem_writes | `oltp_bulk_insert` | 248.54ms | 358.02ms | 1.440× | 1.04% | PASS |
| mem_writes | `oltp_insert` | 19.29ms | 36.67ms | 1.901× | 0.69% | PASS |
| mem_writes | `oltp_update_index` | 69.58ms | 118.84ms | 1.708× | 1.10% | PASS |
| mem_writes | `oltp_update_non_index` | 54.54ms | 88.23ms | 1.618× | 0.92% | PASS |
| mem_writes | `oltp_delete_insert` | 53.38ms | 102.25ms | 1.915× | 1.06% | PASS |
| mem_writes | `oltp_write_only` | 28.50ms | 60.11ms | 2.109× | 1.20% | PASS |
| mem_writes | `types_delete_insert` | 32.61ms | 54.57ms | 1.673× | 1.19% | PASS |
| mem_writes | `oltp_read_write` | 99.81ms | 158.88ms | 1.592× | 0.81% | PASS |
| file_reads | `oltp_point_select` | 109.23ms | 67.00ms | 0.613× | 0.82% | PASS |
| file_reads | `oltp_range_select` | 27.77ms | 24.40ms | 0.879× | 1.95% | PASS |
| file_reads | `oltp_sum_range` | 26.20ms | 23.98ms | 0.915× | 1.31% | PASS |
| file_reads | `oltp_order_range` | 4.59ms | 4.27ms | 0.930× | 2.07% | PASS |
| file_reads | `oltp_distinct_range` | 5.70ms | 5.36ms | 0.941× | 1.36% | PASS |
| file_reads | `oltp_index_scan` | 12.63ms | 9.16ms | 0.725× | 1.59% | PASS |
| file_reads | `select_random_points` | 38.24ms | 36.54ms | 0.955× | 1.40% | PASS |
| file_reads | `select_random_ranges` | 15.87ms | 11.93ms | 0.751× | 1.09% | PASS |
| file_reads | `covering_index_scan` | 12.10ms | 7.24ms | 0.598× | 1.38% | PASS |
| file_reads | `groupby_scan` | 37.29ms | 39.94ms | 1.071× | 0.90% | PASS |
| file_reads | `index_join` | 12.75ms | 12.73ms | 0.999× | 1.82% | PASS |
| file_reads | `index_join_scan` | 5.17ms | 5.91ms | 1.142× | 1.49% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.23s | 1.187× | 0.73% | PASS |
| file_reads | `table_scan` | 1.20s | 1.39s | 1.160× | 0.81% | PASS |
| file_reads | `oltp_read_only` | 265.17ms | 210.89ms | 0.795× | 1.19% | PASS |
| file_writes | `oltp_bulk_insert` | 263.78ms | 378.81ms | 1.436× | 0.94% | PASS |
| file_writes | `oltp_insert` | 26.53ms | 46.03ms | 1.735× | 1.43% | PASS |
| file_writes | `oltp_update_index` | 96.81ms | 142.53ms | 1.472× | 0.93% | PASS |
| file_writes | `oltp_update_non_index` | 76.38ms | 103.75ms | 1.358× | 1.61% | PASS |
| file_writes | `oltp_delete_insert` | 76.99ms | 118.69ms | 1.541× | 1.57% | PASS |
| file_writes | `oltp_write_only` | 51.45ms | 77.27ms | 1.502× | 1.58% | PASS |
| file_writes | `types_delete_insert` | 49.28ms | 68.31ms | 1.386× | 1.09% | PASS |
| file_writes | `oltp_read_write` | 128.38ms | 181.41ms | 1.413× | 1.21% | PASS |
| ac_reads | `oltp_point_select` | 58.45ms | 67.04ms | 1.147× | 1.12% | PASS |
| ac_reads | `oltp_range_select` | 22.22ms | 24.36ms | 1.096× | 1.14% | PASS |
| ac_reads | `oltp_sum_range` | 20.85ms | 23.94ms | 1.148× | 0.85% | PASS |
| ac_reads | `oltp_order_range` | 3.97ms | 4.27ms | 1.078× | 1.36% | PASS |
| ac_reads | `oltp_distinct_range` | 5.12ms | 5.32ms | 1.038× | 1.29% | PASS |
| ac_reads | `oltp_index_scan` | 7.65ms | 9.31ms | 1.217× | 0.97% | PASS |
| ac_reads | `select_random_points` | 33.31ms | 38.26ms | 1.149× | 1.48% | PASS |
| ac_reads | `select_random_ranges` | 10.75ms | 12.10ms | 1.125× | 1.52% | PASS |
| ac_reads | `covering_index_scan` | 7.29ms | 7.36ms | 1.010× | 0.94% | PASS |
| ac_reads | `groupby_scan` | 36.89ms | 39.90ms | 1.082× | 1.07% | PASS |
| ac_reads | `index_join` | 9.97ms | 12.72ms | 1.276× | 1.27% | PASS |
| ac_reads | `index_join_scan` | 4.63ms | 5.88ms | 1.271× | 1.50% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.23s | 1.188× | 0.42% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.39s | 1.152× | 1.05% | PASS |
| ac_reads | `oltp_read_only` | 186.36ms | 208.99ms | 1.121× | 0.81% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.91ms | 68.03ms | 2.845× | 5.20% | PASS |
| ac_writes | `oltp_insert_ac` | 25.07ms | 88.82ms | 3.542× | 5.78% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.45ms | 100.64ms | 3.667× | 4.80% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.44ms | 79.64ms | 3.398× | 5.10% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.29ms | 90.88ms | 3.741× | 6.39% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.69ms | 91.20ms | 3.549× | 5.04% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.00ms | 79.99ms | 3.477× | 4.87% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.88ms | 98.60ms | 3.093× | 6.08% | PASS |

</details>

## Version-control latency

Wall time: 2m 6s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 64.83ms | 200.00ms | 32.4% | 0.55% | PASS |
| `status_dirty_many_tables` | 67.20ms | 200.00ms | 33.6% | 0.61% | PASS |
| `diff_regular_working_one_table` | 61.18ms | 150.00ms | 40.8% | 0.49% | PASS |
| `diff_regular_working_many_tables` | 71.60ms | 200.00ms | 35.8% | 0.39% | PASS |
| `diff_stat_working_many_tables` | 71.74ms | 200.00ms | 35.9% | 0.46% | PASS |
| `diff_schema_working_many_tables` | 71.98ms | 200.00ms | 36.0% | 0.36% | PASS |
| `branch_list_many_branches` | 18.65ms | 100.00ms | 18.7% | 1.87% | PASS |
| `branch_create_delete` | 28.12ms | 100.00ms | 28.1% | 3.88% | PASS |
| `checkout_branch_clean` | 96.94ms | 200.00ms | 48.5% | 8.38% | PASS |
| `merge_data_no_conflicts` | 34.13ms | 150.00ms | 22.8% | 2.20% | PASS |
| `merge_schema_no_conflicts` | 18.35ms | 100.00ms | 18.4% | 3.17% | PASS |
| `merge_data_conflicts` | 99.83ms | 250.00ms | 39.9% | 0.21% | PASS |
| `merge_data_conflicts_with_resolve` | 99.55ms | 250.00ms | 39.8% | 0.19% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
