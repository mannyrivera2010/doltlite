# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-17 16:00 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260907.300.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/35233345698)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 5m 41s | 7.91s | 9.20s | 1.163× | 1.65% | **PASS** |
| textpk | 69 | 55 | 1h 33m 5s | 10.15s | 11.98s | 1.181× | 1.86% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 54s | 10.47s | 11.76s | 1.123× | 1.57% | **PASS** |
| compositepk | 69 | 55 | 1h 28m 23s | 9.90s | 12.15s | 1.227× | 1.63% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 19.50ms | 21.92ms | 1.124× | 1.47% | PASS |
| mem_reads | `oltp_range_select` | 8.65ms | 9.38ms | 1.085× | 1.86% | PASS |
| mem_reads | `oltp_sum_range` | 8.07ms | 9.22ms | 1.142× | 3.87% | PASS |
| mem_reads | `oltp_order_range` | 2.18ms | 2.30ms | 1.054× | 1.57% | PASS |
| mem_reads | `oltp_distinct_range` | 3.01ms | 3.10ms | 1.030× | 0.93% | PASS |
| mem_reads | `oltp_index_scan` | 3.18ms | 3.94ms | 1.237× | 1.41% | PASS |
| mem_reads | `select_random_points` | 8.43ms | 9.10ms | 1.079× | 1.62% | PASS |
| mem_reads | `select_random_ranges` | 2.48ms | 3.19ms | 1.286× | 1.78% | PASS |
| mem_reads | `covering_index_scan` | 3.38ms | 3.18ms | 0.941× | 1.72% | PASS |
| mem_reads | `groupby_scan` | 24.87ms | 26.81ms | 1.078× | 0.61% | PASS |
| mem_reads | `index_join` | 4.68ms | 6.07ms | 1.297× | 0.71% | PASS |
| mem_reads | `index_join_scan` | 2.85ms | 3.83ms | 1.345× | 1.43% | PASS |
| mem_reads | `types_table_scan` | 864.82ms | 987.39ms | 1.142× | 0.72% | PASS |
| mem_reads | `table_scan` | 1.01s | 1.09s | 1.075× | 1.71% | PASS |
| mem_reads | `oltp_read_only` | 83.13ms | 91.05ms | 1.095× | 2.64% | PASS |
| mem_writes | `oltp_bulk_insert` | 140.63ms | 186.28ms | 1.325× | 0.87% | PASS |
| mem_writes | `oltp_insert` | 12.36ms | 21.28ms | 1.721× | 1.29% | PASS |
| mem_writes | `oltp_update_index` | 41.51ms | 81.01ms | 1.952× | 1.60% | PASS |
| mem_writes | `oltp_update_non_index` | 29.02ms | 46.51ms | 1.602× | 3.02% | PASS |
| mem_writes | `oltp_delete_insert` | 36.30ms | 59.86ms | 1.649× | 1.77% | PASS |
| mem_writes | `oltp_write_only` | 18.15ms | 34.06ms | 1.877× | 1.65% | PASS |
| mem_writes | `types_delete_insert` | 20.12ms | 30.36ms | 1.509× | 3.50% | PASS |
| mem_writes | `oltp_read_write` | 52.29ms | 80.72ms | 1.544× | 2.26% | PASS |
| file_reads | `oltp_point_select` | 93.74ms | 46.82ms | 0.499× | 1.22% | PASS |
| file_reads | `oltp_range_select` | 16.24ms | 12.07ms | 0.743× | 2.31% | PASS |
| file_reads | `oltp_sum_range` | 15.52ms | 11.78ms | 0.759× | 1.97% | PASS |
| file_reads | `oltp_order_range` | 3.00ms | 2.59ms | 0.862× | 2.44% | PASS |
| file_reads | `oltp_distinct_range` | 3.77ms | 3.40ms | 0.899× | 1.24% | PASS |
| file_reads | `oltp_index_scan` | 10.85ms | 6.91ms | 0.637× | 1.76% | PASS |
| file_reads | `select_random_points` | 16.59ms | 11.90ms | 0.717× | 2.45% | PASS |
| file_reads | `select_random_ranges` | 10.03ms | 5.72ms | 0.570× | 1.10% | PASS |
| file_reads | `covering_index_scan` | 11.12ms | 6.07ms | 0.546× | 1.28% | PASS |
| file_reads | `groupby_scan` | 25.83ms | 27.21ms | 1.054× | 0.65% | PASS |
| file_reads | `index_join` | 8.89ms | 8.23ms | 0.925× | 1.71% | PASS |
| file_reads | `index_join_scan` | 3.74ms | 4.32ms | 1.155× | 2.95% | PASS |
| file_reads | `types_table_scan` | 855.89ms | 982.29ms | 1.148× | 0.67% | PASS |
| file_reads | `table_scan` | 990.38ms | 1.07s | 1.083× | 1.44% | PASS |
| file_reads | `oltp_read_only` | 191.54ms | 126.46ms | 0.660× | 0.88% | PASS |
| file_writes | `oltp_bulk_insert` | 189.72ms | 246.40ms | 1.299× | 3.87% | PASS |
| file_writes | `oltp_insert` | 25.95ms | 39.66ms | 1.529× | 8.16% | PASS |
| file_writes | `oltp_update_index` | 143.36ms | 149.99ms | 1.046× | 2.74% | PASS |
| file_writes | `oltp_update_non_index` | 116.66ms | 103.46ms | 0.887× | 1.49% | PASS |
| file_writes | `oltp_delete_insert` | 123.03ms | 118.14ms | 0.960× | 1.41% | PASS |
| file_writes | `oltp_write_only` | 91.80ms | 85.61ms | 0.933× | 1.75% | PASS |
| file_writes | `types_delete_insert` | 78.78ms | 67.86ms | 0.861× | 5.41% | PASS |
| file_writes | `oltp_read_write` | 128.76ms | 134.52ms | 1.045× | 1.88% | PASS |
| ac_reads | `oltp_point_select` | 44.75ms | 47.28ms | 1.057× | 1.43% | PASS |
| ac_reads | `oltp_range_select` | 11.80ms | 12.11ms | 1.026× | 1.29% | PASS |
| ac_reads | `oltp_sum_range` | 10.88ms | 11.80ms | 1.085× | 1.21% | PASS |
| ac_reads | `oltp_order_range` | 2.62ms | 2.61ms | 0.995× | 1.77% | PASS |
| ac_reads | `oltp_distinct_range` | 3.40ms | 3.40ms | 1.002× | 1.00% | PASS |
| ac_reads | `oltp_index_scan` | 6.12ms | 6.96ms | 1.138× | 0.91% | PASS |
| ac_reads | `select_random_points` | 11.90ms | 11.97ms | 1.006× | 1.29% | PASS |
| ac_reads | `select_random_ranges` | 5.21ms | 5.74ms | 1.101× | 1.06% | PASS |
| ac_reads | `covering_index_scan` | 6.29ms | 6.08ms | 0.966× | 0.79% | PASS |
| ac_reads | `groupby_scan` | 25.52ms | 27.28ms | 1.069× | 0.74% | PASS |
| ac_reads | `index_join` | 6.57ms | 8.33ms | 1.267× | 1.69% | PASS |
| ac_reads | `index_join_scan` | 3.35ms | 4.30ms | 1.286× | 2.19% | PASS |
| ac_reads | `types_table_scan` | 851.01ms | 980.42ms | 1.152× | 0.39% | PASS |
| ac_reads | `table_scan` | 974.51ms | 1.07s | 1.096× | 0.56% | PASS |
| ac_reads | `oltp_read_only` | 116.36ms | 124.99ms | 1.074× | 0.56% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 31.26ms | 83.67ms | 2.676× | 5.77% | PASS |
| ac_writes | `oltp_insert_ac` | 33.72ms | 99.16ms | 2.941× | 5.60% | PASS |
| ac_writes | `oltp_update_index_ac` | 35.14ms | 109.93ms | 3.128× | 7.86% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 32.27ms | 93.05ms | 2.884× | 7.08% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 33.31ms | 101.10ms | 3.035× | 6.97% | PASS |
| ac_writes | `oltp_write_only_ac` | 33.87ms | 101.81ms | 3.006× | 6.69% | PASS |
| ac_writes | `types_delete_insert_ac` | 31.07ms | 93.58ms | 3.012× | 8.77% | PASS |
| ac_writes | `oltp_read_write_ac` | 38.16ms | 108.82ms | 2.851× | 5.78% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.38ms | 37.69ms | 1.240× | 2.15% | PASS |
| mem_reads | `oltp_range_select` | 13.36ms | 14.16ms | 1.060× | 2.80% | PASS |
| mem_reads | `oltp_sum_range` | 11.91ms | 14.17ms | 1.190× | 1.86% | PASS |
| mem_reads | `oltp_order_range` | 2.87ms | 3.13ms | 1.089× | 1.64% | PASS |
| mem_reads | `oltp_distinct_range` | 3.91ms | 4.20ms | 1.073× | 0.96% | PASS |
| mem_reads | `oltp_index_scan` | 4.35ms | 6.18ms | 1.421× | 1.54% | PASS |
| mem_reads | `select_random_points` | 17.40ms | 20.88ms | 1.200× | 1.46% | PASS |
| mem_reads | `select_random_ranges` | 3.77ms | 5.12ms | 1.359× | 1.54% | PASS |
| mem_reads | `covering_index_scan` | 4.51ms | 4.41ms | 0.976× | 1.29% | PASS |
| mem_reads | `groupby_scan` | 31.57ms | 33.72ms | 1.068× | 0.81% | PASS |
| mem_reads | `index_join` | 6.79ms | 8.80ms | 1.296× | 0.99% | PASS |
| mem_reads | `index_join_scan` | 4.49ms | 5.32ms | 1.184× | 5.17% | PASS |
| mem_reads | `types_table_scan` | 1.18s | 1.26s | 1.069× | 3.09% | PASS |
| mem_reads | `table_scan` | 1.50s | 1.43s | 0.951× | 1.49% | PASS |
| mem_reads | `oltp_read_only` | 128.48ms | 139.71ms | 1.087× | 1.47% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.63ms | 359.65ms | 1.526× | 0.87% | PASS |
| mem_writes | `oltp_insert` | 23.39ms | 41.41ms | 1.770× | 1.89% | PASS |
| mem_writes | `oltp_update_index` | 76.38ms | 138.24ms | 1.810× | 1.64% | PASS |
| mem_writes | `oltp_update_non_index` | 50.06ms | 88.65ms | 1.771× | 1.03% | PASS |
| mem_writes | `oltp_delete_insert` | 53.65ms | 107.26ms | 1.999× | 1.28% | PASS |
| mem_writes | `oltp_write_only` | 30.98ms | 63.98ms | 2.065× | 1.43% | PASS |
| mem_writes | `types_delete_insert` | 34.34ms | 56.83ms | 1.655× | 1.85% | PASS |
| mem_writes | `oltp_read_write` | 87.77ms | 140.44ms | 1.600× | 1.97% | PASS |
| file_reads | `oltp_point_select` | 106.21ms | 63.84ms | 0.601× | 1.20% | PASS |
| file_reads | `oltp_range_select` | 22.39ms | 17.04ms | 0.761× | 2.17% | PASS |
| file_reads | `oltp_sum_range` | 21.11ms | 17.32ms | 0.821× | 1.88% | PASS |
| file_reads | `oltp_order_range` | 3.94ms | 3.50ms | 0.887× | 2.41% | PASS |
| file_reads | `oltp_distinct_range` | 5.08ms | 4.66ms | 0.918× | 1.49% | PASS |
| file_reads | `oltp_index_scan` | 12.64ms | 9.17ms | 0.726× | 1.99% | PASS |
| file_reads | `select_random_points` | 27.75ms | 24.89ms | 0.897× | 2.14% | PASS |
| file_reads | `select_random_ranges` | 11.94ms | 7.91ms | 0.663× | 1.04% | PASS |
| file_reads | `covering_index_scan` | 13.40ms | 7.42ms | 0.553× | 2.42% | PASS |
| file_reads | `groupby_scan` | 33.34ms | 34.30ms | 1.029× | 1.44% | PASS |
| file_reads | `index_join` | 12.00ms | 11.26ms | 0.939× | 3.89% | PASS |
| file_reads | `index_join_scan` | 5.90ms | 5.89ms | 0.998× | 4.59% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.23s | 1.160× | 1.16% | PASS |
| file_reads | `table_scan` | 1.33s | 1.40s | 1.054× | 4.83% | PASS |
| file_reads | `oltp_read_only` | 231.81ms | 175.86ms | 0.759× | 1.52% | PASS |
| file_writes | `oltp_bulk_insert` | 255.66ms | 388.55ms | 1.520× | 1.09% | PASS |
| file_writes | `oltp_insert` | 48.86ms | 53.68ms | 1.099× | 19.04% | PASS |
| file_writes | `oltp_update_index` | 116.25ms | 171.67ms | 1.477× | 1.83% | PASS |
| file_writes | `oltp_update_non_index` | 93.94ms | 113.77ms | 1.211× | 7.43% | PASS |
| file_writes | `oltp_delete_insert` | 90.80ms | 134.82ms | 1.485× | 1.93% | PASS |
| file_writes | `oltp_write_only` | 90.81ms | 86.46ms | 0.952× | 7.12% | PASS |
| file_writes | `types_delete_insert` | 57.65ms | 76.44ms | 1.326× | 1.80% | PASS |
| file_writes | `oltp_read_write` | 140.19ms | 166.47ms | 1.187× | 7.42% | PASS |
| ac_reads | `oltp_point_select` | 56.25ms | 63.89ms | 1.136× | 1.15% | PASS |
| ac_reads | `oltp_range_select` | 17.10ms | 16.94ms | 0.991× | 1.84% | PASS |
| ac_reads | `oltp_sum_range` | 16.29ms | 17.21ms | 1.057× | 2.02% | PASS |
| ac_reads | `oltp_order_range` | 3.46ms | 3.51ms | 1.016× | 1.31% | PASS |
| ac_reads | `oltp_distinct_range` | 4.51ms | 4.61ms | 1.021× | 1.29% | PASS |
| ac_reads | `oltp_index_scan` | 7.51ms | 9.28ms | 1.234× | 1.98% | PASS |
| ac_reads | `select_random_points` | 21.88ms | 24.57ms | 1.123× | 2.44% | PASS |
| ac_reads | `select_random_ranges` | 6.75ms | 7.86ms | 1.164× | 1.30% | PASS |
| ac_reads | `covering_index_scan` | 7.75ms | 7.41ms | 0.956× | 2.42% | PASS |
| ac_reads | `groupby_scan` | 32.45ms | 34.32ms | 1.057× | 1.41% | PASS |
| ac_reads | `index_join` | 9.05ms | 11.21ms | 1.240× | 2.96% | PASS |
| ac_reads | `index_join_scan` | 5.43ms | 5.82ms | 1.071× | 2.09% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.22s | 1.168× | 0.52% | PASS |
| ac_reads | `table_scan` | 1.21s | 1.38s | 1.141× | 0.57% | PASS |
| ac_reads | `oltp_read_only` | 168.41ms | 180.27ms | 1.070× | 1.58% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.69ms | 81.76ms | 3.603× | 7.96% | PASS |
| ac_writes | `oltp_insert_ac` | 26.53ms | 98.11ms | 3.698× | 5.74% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.63ms | 116.34ms | 4.211× | 5.17% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.17ms | 94.05ms | 4.243× | 6.76% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.48ms | 105.86ms | 4.154× | 6.33% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.97ms | 102.10ms | 3.932× | 4.88% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.18ms | 94.06ms | 4.241× | 6.93% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.59ms | 110.77ms | 3.506× | 4.67% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.68ms | 34.58ms | 1.127× | 1.53% | PASS |
| mem_reads | `oltp_range_select` | 13.48ms | 13.58ms | 1.007× | 2.15% | PASS |
| mem_reads | `oltp_sum_range` | 12.39ms | 13.17ms | 1.063× | 1.27% | PASS |
| mem_reads | `oltp_order_range` | 3.13ms | 3.15ms | 1.007× | 1.57% | PASS |
| mem_reads | `oltp_distinct_range` | 4.17ms | 4.17ms | 1.001× | 1.01% | PASS |
| mem_reads | `oltp_index_scan` | 4.70ms | 5.95ms | 1.266× | 2.28% | PASS |
| mem_reads | `select_random_points` | 18.70ms | 19.98ms | 1.069× | 2.41% | PASS |
| mem_reads | `select_random_ranges` | 4.15ms | 5.17ms | 1.246× | 1.45% | PASS |
| mem_reads | `covering_index_scan` | 4.61ms | 4.56ms | 0.990× | 2.32% | PASS |
| mem_reads | `groupby_scan` | 33.66ms | 34.94ms | 1.038× | 0.77% | PASS |
| mem_reads | `index_join` | 6.87ms | 8.98ms | 1.307× | 2.33% | PASS |
| mem_reads | `index_join_scan` | 4.41ms | 5.59ms | 1.268× | 1.77% | PASS |
| mem_reads | `types_table_scan` | 1.14s | 1.25s | 1.099× | 1.18% | PASS |
| mem_reads | `table_scan` | 1.33s | 1.36s | 1.022× | 0.99% | PASS |
| mem_reads | `oltp_read_only` | 120.93ms | 128.50ms | 1.063× | 1.77% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.94ms | 330.28ms | 1.394× | 0.74% | PASS |
| mem_writes | `oltp_insert` | 20.65ms | 39.36ms | 1.906× | 0.65% | PASS |
| mem_writes | `oltp_update_index` | 70.95ms | 132.97ms | 1.874× | 0.95% | PASS |
| mem_writes | `oltp_update_non_index` | 49.64ms | 85.27ms | 1.718× | 1.02% | PASS |
| mem_writes | `oltp_delete_insert` | 50.16ms | 103.99ms | 2.073× | 1.05% | PASS |
| mem_writes | `oltp_write_only` | 28.31ms | 62.82ms | 2.219× | 1.08% | PASS |
| mem_writes | `types_delete_insert` | 32.17ms | 52.63ms | 1.636× | 1.17% | PASS |
| mem_writes | `oltp_read_write` | 82.22ms | 133.76ms | 1.627× | 1.69% | PASS |
| file_reads | `oltp_point_select` | 127.46ms | 67.00ms | 0.526× | 0.92% | PASS |
| file_reads | `oltp_range_select` | 23.61ms | 16.86ms | 0.714× | 1.87% | PASS |
| file_reads | `oltp_sum_range` | 22.37ms | 16.48ms | 0.737× | 1.74% | PASS |
| file_reads | `oltp_order_range` | 4.00ms | 3.52ms | 0.879× | 1.59% | PASS |
| file_reads | `oltp_distinct_range` | 5.08ms | 4.51ms | 0.888× | 1.93% | PASS |
| file_reads | `oltp_index_scan` | 14.22ms | 9.33ms | 0.656× | 1.81% | PASS |
| file_reads | `select_random_points` | 27.82ms | 23.00ms | 0.827× | 1.90% | PASS |
| file_reads | `select_random_ranges` | 14.02ms | 8.50ms | 0.606× | 0.84% | PASS |
| file_reads | `covering_index_scan` | 14.91ms | 7.94ms | 0.532× | 1.94% | PASS |
| file_reads | `groupby_scan` | 34.67ms | 35.44ms | 1.022× | 0.86% | PASS |
| file_reads | `index_join` | 12.64ms | 11.42ms | 0.904× | 1.28% | PASS |
| file_reads | `index_join_scan` | 5.61ms | 6.11ms | 1.090× | 1.68% | PASS |
| file_reads | `types_table_scan` | 1.23s | 1.28s | 1.042× | 1.65% | PASS |
| file_reads | `table_scan` | 1.39s | 1.37s | 0.983× | 3.40% | PASS |
| file_reads | `oltp_read_only` | 259.66ms | 174.38ms | 0.672× | 0.75% | PASS |
| file_writes | `oltp_bulk_insert` | 259.46ms | 357.35ms | 1.377× | 1.06% | PASS |
| file_writes | `oltp_insert` | 32.06ms | 52.64ms | 1.642× | 1.47% | PASS |
| file_writes | `oltp_update_index` | 106.95ms | 167.04ms | 1.562× | 1.04% | PASS |
| file_writes | `oltp_update_non_index` | 82.14ms | 111.70ms | 1.360× | 1.14% | PASS |
| file_writes | `oltp_delete_insert` | 85.22ms | 133.93ms | 1.572× | 2.10% | PASS |
| file_writes | `oltp_write_only` | 57.08ms | 87.53ms | 1.533× | 2.06% | PASS |
| file_writes | `types_delete_insert` | 53.98ms | 72.73ms | 1.347× | 1.55% | PASS |
| file_writes | `oltp_read_write` | 117.35ms | 160.60ms | 1.369× | 2.17% | PASS |
| ac_reads | `oltp_point_select` | 62.54ms | 66.50ms | 1.063× | 0.87% | PASS |
| ac_reads | `oltp_range_select` | 17.33ms | 16.83ms | 0.971× | 1.91% | PASS |
| ac_reads | `oltp_sum_range` | 16.02ms | 16.47ms | 1.028× | 1.69% | PASS |
| ac_reads | `oltp_order_range` | 3.56ms | 3.51ms | 0.985× | 1.19% | PASS |
| ac_reads | `oltp_distinct_range` | 4.57ms | 4.53ms | 0.989× | 1.06% | PASS |
| ac_reads | `oltp_index_scan` | 8.39ms | 9.38ms | 1.118× | 0.73% | PASS |
| ac_reads | `select_random_points` | 22.07ms | 23.27ms | 1.054× | 1.52% | PASS |
| ac_reads | `select_random_ranges` | 7.69ms | 8.48ms | 1.103× | 1.02% | PASS |
| ac_reads | `covering_index_scan` | 8.83ms | 7.96ms | 0.902× | 1.07% | PASS |
| ac_reads | `groupby_scan` | 34.40ms | 35.57ms | 1.034× | 0.71% | PASS |
| ac_reads | `index_join` | 9.62ms | 11.55ms | 1.201× | 1.53% | PASS |
| ac_reads | `index_join_scan` | 5.11ms | 6.17ms | 1.207× | 1.59% | PASS |
| ac_reads | `types_table_scan` | 1.24s | 1.29s | 1.035× | 1.94% | PASS |
| ac_reads | `table_scan` | 1.42s | 1.39s | 0.977× | 3.45% | PASS |
| ac_reads | `oltp_read_only` | 170.90ms | 177.00ms | 1.036× | 1.20% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.19ms | 64.27ms | 3.970× | 3.77% | PASS |
| ac_writes | `oltp_insert_ac` | 18.43ms | 85.76ms | 4.653× | 2.00% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.52ms | 97.72ms | 4.763× | 2.32% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.66ms | 76.01ms | 4.561× | 5.62% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 19.92ms | 93.02ms | 4.670× | 5.55% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.81ms | 88.71ms | 4.717× | 4.30% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.02ms | 79.36ms | 4.663× | 7.54% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.64ms | 97.14ms | 3.789× | 3.83% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.97ms | 40.84ms | 1.202× | 1.63% | PASS |
| mem_reads | `oltp_range_select` | 19.73ms | 21.27ms | 1.078× | 2.80% | PASS |
| mem_reads | `oltp_sum_range` | 18.38ms | 21.01ms | 1.143× | 1.23% | PASS |
| mem_reads | `oltp_order_range` | 3.63ms | 3.88ms | 1.071× | 1.38% | PASS |
| mem_reads | `oltp_distinct_range` | 4.72ms | 4.96ms | 1.052× | 1.12% | PASS |
| mem_reads | `oltp_index_scan` | 4.69ms | 6.31ms | 1.344× | 2.38% | PASS |
| mem_reads | `select_random_points` | 28.95ms | 32.32ms | 1.116× | 1.82% | PASS |
| mem_reads | `select_random_ranges` | 8.03ms | 9.19ms | 1.144× | 1.52% | PASS |
| mem_reads | `covering_index_scan` | 4.29ms | 4.56ms | 1.064× | 3.63% | PASS |
| mem_reads | `groupby_scan` | 36.88ms | 38.83ms | 1.053× | 1.06% | PASS |
| mem_reads | `index_join` | 8.39ms | 10.95ms | 1.304× | 2.46% | PASS |
| mem_reads | `index_join_scan` | 4.53ms | 5.88ms | 1.297× | 4.01% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.24s | 1.177× | 1.08% | PASS |
| mem_reads | `table_scan` | 1.20s | 1.40s | 1.160× | 1.23% | PASS |
| mem_reads | `oltp_read_only` | 150.73ms | 169.65ms | 1.126× | 1.60% | PASS |
| mem_writes | `oltp_bulk_insert` | 250.09ms | 361.06ms | 1.444× | 0.96% | PASS |
| mem_writes | `oltp_insert` | 19.31ms | 36.68ms | 1.900× | 0.90% | PASS |
| mem_writes | `oltp_update_index` | 68.89ms | 119.69ms | 1.737× | 1.52% | PASS |
| mem_writes | `oltp_update_non_index` | 51.83ms | 84.14ms | 1.623× | 2.19% | PASS |
| mem_writes | `oltp_delete_insert` | 50.82ms | 97.44ms | 1.917× | 1.23% | PASS |
| mem_writes | `oltp_write_only` | 27.48ms | 57.96ms | 2.109× | 1.30% | PASS |
| mem_writes | `types_delete_insert` | 33.62ms | 55.87ms | 1.662× | 2.10% | PASS |
| mem_writes | `oltp_read_write` | 105.56ms | 157.57ms | 1.493× | 2.05% | PASS |
| file_reads | `oltp_point_select` | 110.93ms | 67.28ms | 0.607× | 1.41% | PASS |
| file_reads | `oltp_range_select` | 27.58ms | 24.49ms | 0.888× | 1.91% | PASS |
| file_reads | `oltp_sum_range` | 26.75ms | 24.22ms | 0.905× | 2.41% | PASS |
| file_reads | `oltp_order_range` | 4.61ms | 4.34ms | 0.942× | 2.01% | PASS |
| file_reads | `oltp_distinct_range` | 5.76ms | 5.45ms | 0.946× | 1.40% | PASS |
| file_reads | `oltp_index_scan` | 12.97ms | 9.46ms | 0.729× | 1.76% | PASS |
| file_reads | `select_random_points` | 40.04ms | 37.57ms | 0.938× | 2.12% | PASS |
| file_reads | `select_random_ranges` | 16.13ms | 12.17ms | 0.754× | 1.32% | PASS |
| file_reads | `covering_index_scan` | 11.99ms | 7.31ms | 0.610× | 1.60% | PASS |
| file_reads | `groupby_scan` | 37.74ms | 39.50ms | 1.047× | 1.11% | PASS |
| file_reads | `index_join` | 12.78ms | 13.12ms | 1.027× | 1.32% | PASS |
| file_reads | `index_join_scan` | 5.46ms | 6.18ms | 1.134× | 1.92% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.24s | 1.177× | 1.44% | PASS |
| file_reads | `table_scan` | 1.25s | 1.40s | 1.123× | 2.67% | PASS |
| file_reads | `oltp_read_only` | 265.91ms | 210.82ms | 0.793× | 1.38% | PASS |
| file_writes | `oltp_bulk_insert` | 264.84ms | 383.26ms | 1.447× | 0.88% | PASS |
| file_writes | `oltp_insert` | 26.38ms | 46.87ms | 1.777× | 2.01% | PASS |
| file_writes | `oltp_update_index` | 100.73ms | 148.17ms | 1.471× | 1.49% | PASS |
| file_writes | `oltp_update_non_index` | 79.56ms | 106.24ms | 1.335× | 2.59% | PASS |
| file_writes | `oltp_delete_insert` | 79.22ms | 121.99ms | 1.540× | 1.13% | PASS |
| file_writes | `oltp_write_only` | 52.45ms | 79.88ms | 1.523× | 1.71% | PASS |
| file_writes | `types_delete_insert` | 51.96ms | 71.41ms | 1.374× | 1.46% | PASS |
| file_writes | `oltp_read_write` | 137.18ms | 183.97ms | 1.341× | 1.48% | PASS |
| ac_reads | `oltp_point_select` | 60.40ms | 68.00ms | 1.126× | 1.06% | PASS |
| ac_reads | `oltp_range_select` | 23.42ms | 24.75ms | 1.057× | 2.48% | PASS |
| ac_reads | `oltp_sum_range` | 21.81ms | 24.39ms | 1.118× | 1.73% | PASS |
| ac_reads | `oltp_order_range` | 4.20ms | 4.37ms | 1.040× | 1.77% | PASS |
| ac_reads | `oltp_distinct_range` | 5.32ms | 5.45ms | 1.025× | 1.48% | PASS |
| ac_reads | `oltp_index_scan` | 7.80ms | 9.38ms | 1.203× | 1.24% | PASS |
| ac_reads | `select_random_points` | 33.63ms | 36.91ms | 1.097× | 1.58% | PASS |
| ac_reads | `select_random_ranges` | 10.92ms | 12.18ms | 1.116× | 1.37% | PASS |
| ac_reads | `covering_index_scan` | 7.32ms | 7.34ms | 1.002× | 1.83% | PASS |
| ac_reads | `groupby_scan` | 37.23ms | 39.62ms | 1.064× | 0.88% | PASS |
| ac_reads | `index_join` | 10.52ms | 13.27ms | 1.262× | 2.75% | PASS |
| ac_reads | `index_join_scan` | 4.91ms | 6.18ms | 1.260× | 2.29% | PASS |
| ac_reads | `types_table_scan` | 1.07s | 1.25s | 1.164× | 1.08% | PASS |
| ac_reads | `table_scan` | 1.33s | 1.42s | 1.066× | 3.43% | PASS |
| ac_reads | `oltp_read_only` | 200.16ms | 215.49ms | 1.077× | 1.07% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.27ms | 80.06ms | 3.595× | 4.45% | PASS |
| ac_writes | `oltp_insert_ac` | 24.72ms | 100.43ms | 4.062× | 4.13% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.69ms | 111.18ms | 3.875× | 5.22% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.04ms | 91.95ms | 3.991× | 3.60% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.50ms | 101.96ms | 4.163× | 2.97% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.02ms | 101.94ms | 4.074× | 2.85% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.07ms | 90.96ms | 4.121× | 5.00% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.86ms | 110.33ms | 3.462× | 3.11% | PASS |

</details>

## Version-control latency

Wall time: 2m 21s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.81ms | 200.00ms | 41.9% | 0.61% | PASS |
| `status_dirty_many_tables` | 87.16ms | 200.00ms | 43.6% | 0.42% | PASS |
| `diff_regular_working_one_table` | 79.62ms | 150.00ms | 53.1% | 0.53% | PASS |
| `diff_regular_working_many_tables` | 92.80ms | 200.00ms | 46.4% | 0.59% | PASS |
| `diff_stat_working_many_tables` | 92.67ms | 200.00ms | 46.3% | 0.45% | PASS |
| `diff_schema_working_many_tables` | 93.18ms | 200.00ms | 46.6% | 0.54% | PASS |
| `branch_list_many_branches` | 23.65ms | 100.00ms | 23.6% | 1.04% | PASS |
| `branch_create_delete` | 25.91ms | 100.00ms | 25.9% | 1.63% | PASS |
| `checkout_branch_clean` | 56.13ms | 200.00ms | 28.1% | 0.69% | PASS |
| `merge_data_no_conflicts` | 29.86ms | 150.00ms | 19.9% | 1.61% | PASS |
| `merge_schema_no_conflicts` | 22.87ms | 100.00ms | 22.9% | 1.51% | PASS |
| `merge_data_conflicts` | 127.71ms | 250.00ms | 51.1% | 0.29% | PASS |
| `merge_data_conflicts_with_resolve` | 127.93ms | 250.00ms | 51.2% | 0.24% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
