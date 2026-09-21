# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-21 17:36 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260907.300.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/35622584453)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 16s | 9.38s | 11.07s | 1.180× | 1.31% | **PASS** |
| textpk | 69 | 55 | 1h 34m 15s | 11.47s | 12.03s | 1.049× | 1.49% | **PASS** |
| blobpk | 69 | 55 | 1h 18m 49s | 8.22s | 9.92s | 1.207× | 2.66% | **PASS** |
| compositepk | 69 | 55 | 1h 11m 36s | 8.03s | 9.66s | 1.203× | 1.27% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.77ms | 27.93ms | 1.128× | 1.55% | PASS |
| mem_reads | `oltp_range_select` | 10.75ms | 12.01ms | 1.117× | 1.31% | PASS |
| mem_reads | `oltp_sum_range` | 9.77ms | 11.41ms | 1.168× | 1.17% | PASS |
| mem_reads | `oltp_order_range` | 2.67ms | 2.84ms | 1.066× | 1.23% | PASS |
| mem_reads | `oltp_distinct_range` | 3.77ms | 3.91ms | 1.038× | 0.91% | PASS |
| mem_reads | `oltp_index_scan` | 3.97ms | 4.88ms | 1.231× | 0.95% | PASS |
| mem_reads | `select_random_points` | 10.39ms | 11.21ms | 1.078× | 1.16% | PASS |
| mem_reads | `select_random_ranges` | 3.08ms | 3.98ms | 1.292× | 1.16% | PASS |
| mem_reads | `covering_index_scan` | 4.36ms | 4.07ms | 0.933× | 1.37% | PASS |
| mem_reads | `groupby_scan` | 31.68ms | 34.30ms | 1.082× | 0.58% | PASS |
| mem_reads | `index_join` | 5.93ms | 7.73ms | 1.304× | 1.51% | PASS |
| mem_reads | `index_join_scan` | 3.49ms | 4.70ms | 1.347× | 1.06% | PASS |
| mem_reads | `types_table_scan` | 1.11s | 1.26s | 1.136× | 0.53% | PASS |
| mem_reads | `table_scan` | 1.27s | 1.38s | 1.090× | 1.14% | PASS |
| mem_reads | `oltp_read_only` | 111.29ms | 119.03ms | 1.070× | 2.67% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.69ms | 240.06ms | 1.336× | 0.92% | PASS |
| mem_writes | `oltp_insert` | 15.81ms | 28.18ms | 1.783× | 0.87% | PASS |
| mem_writes | `oltp_update_index` | 51.42ms | 106.81ms | 2.077× | 1.22% | PASS |
| mem_writes | `oltp_update_non_index` | 35.37ms | 59.44ms | 1.681× | 1.06% | PASS |
| mem_writes | `oltp_delete_insert` | 47.04ms | 82.09ms | 1.745× | 1.96% | PASS |
| mem_writes | `oltp_write_only` | 22.61ms | 46.30ms | 2.048× | 1.43% | PASS |
| mem_writes | `types_delete_insert` | 25.29ms | 40.54ms | 1.603× | 1.54% | PASS |
| mem_writes | `oltp_read_write` | 65.21ms | 104.73ms | 1.606× | 1.06% | PASS |
| file_reads | `oltp_point_select` | 119.77ms | 59.09ms | 0.493× | 0.95% | PASS |
| file_reads | `oltp_range_select` | 20.36ms | 15.18ms | 0.746× | 2.91% | PASS |
| file_reads | `oltp_sum_range` | 19.86ms | 14.95ms | 0.753× | 1.65% | PASS |
| file_reads | `oltp_order_range` | 3.79ms | 3.27ms | 0.864× | 1.99% | PASS |
| file_reads | `oltp_distinct_range` | 4.68ms | 4.29ms | 0.918× | 1.10% | PASS |
| file_reads | `oltp_index_scan` | 13.51ms | 8.45ms | 0.625× | 1.49% | PASS |
| file_reads | `select_random_points` | 19.53ms | 14.59ms | 0.747× | 1.70% | PASS |
| file_reads | `select_random_ranges` | 12.54ms | 7.21ms | 0.575× | 1.29% | PASS |
| file_reads | `covering_index_scan` | 13.99ms | 7.54ms | 0.539× | 1.41% | PASS |
| file_reads | `groupby_scan` | 32.67ms | 34.85ms | 1.067× | 1.14% | PASS |
| file_reads | `index_join` | 10.99ms | 9.88ms | 0.899× | 1.18% | PASS |
| file_reads | `index_join_scan` | 4.63ms | 5.36ms | 1.158× | 1.94% | PASS |
| file_reads | `types_table_scan` | 1.11s | 1.26s | 1.143× | 0.82% | PASS |
| file_reads | `table_scan` | 1.26s | 1.38s | 1.088× | 0.64% | PASS |
| file_reads | `oltp_read_only` | 243.20ms | 161.54ms | 0.664× | 0.59% | PASS |
| file_writes | `oltp_bulk_insert` | 195.91ms | 261.60ms | 1.335× | 1.03% | PASS |
| file_writes | `oltp_insert` | 22.15ms | 36.24ms | 1.636× | 1.37% | PASS |
| file_writes | `oltp_update_index` | 79.79ms | 129.65ms | 1.625× | 1.22% | PASS |
| file_writes | `oltp_update_non_index` | 60.06ms | 82.69ms | 1.377× | 1.67% | PASS |
| file_writes | `oltp_delete_insert` | 68.90ms | 99.59ms | 1.446× | 1.77% | PASS |
| file_writes | `oltp_write_only` | 44.62ms | 66.54ms | 1.491× | 2.38% | PASS |
| file_writes | `types_delete_insert` | 40.83ms | 53.98ms | 1.322× | 1.93% | PASS |
| file_writes | `oltp_read_write` | 91.15ms | 128.86ms | 1.414× | 2.57% | PASS |
| ac_reads | `oltp_point_select` | 55.92ms | 59.49ms | 1.064× | 0.63% | PASS |
| ac_reads | `oltp_range_select` | 14.28ms | 15.23ms | 1.066× | 0.89% | PASS |
| ac_reads | `oltp_sum_range` | 13.32ms | 14.80ms | 1.112× | 0.99% | PASS |
| ac_reads | `oltp_order_range` | 3.18ms | 3.25ms | 1.023× | 1.37% | PASS |
| ac_reads | `oltp_distinct_range` | 4.15ms | 4.29ms | 1.036× | 0.93% | PASS |
| ac_reads | `oltp_index_scan` | 7.41ms | 8.49ms | 1.145× | 1.39% | PASS |
| ac_reads | `select_random_points` | 14.40ms | 14.82ms | 1.029× | 1.53% | PASS |
| ac_reads | `select_random_ranges` | 6.39ms | 7.24ms | 1.133× | 1.42% | PASS |
| ac_reads | `covering_index_scan` | 7.90ms | 7.64ms | 0.967× | 1.29% | PASS |
| ac_reads | `groupby_scan` | 32.20ms | 34.83ms | 1.081× | 0.71% | PASS |
| ac_reads | `index_join` | 7.99ms | 10.16ms | 1.272× | 1.68% | PASS |
| ac_reads | `index_join_scan` | 4.06ms | 5.31ms | 1.308× | 1.70% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.26s | 1.142× | 0.81% | PASS |
| ac_reads | `table_scan` | 1.27s | 1.38s | 1.087× | 0.89% | PASS |
| ac_reads | `oltp_read_only` | 149.63ms | 161.32ms | 1.078× | 0.69% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.70ms | 61.58ms | 3.922× | 3.14% | PASS |
| ac_writes | `oltp_insert_ac` | 17.59ms | 78.50ms | 4.463× | 3.29% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.46ms | 94.25ms | 4.844× | 3.93% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 15.86ms | 71.96ms | 4.536× | 3.59% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.65ms | 84.55ms | 4.790× | 4.12% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.08ms | 83.06ms | 4.594× | 3.37% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.48ms | 72.11ms | 4.375× | 4.45% | PASS |
| ac_writes | `oltp_read_write_ac` | 23.16ms | 90.31ms | 3.899× | 3.30% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.78ms | 35.13ms | 1.141× | 1.40% | PASS |
| mem_reads | `oltp_range_select` | 14.08ms | 13.70ms | 0.973× | 1.77% | PASS |
| mem_reads | `oltp_sum_range` | 12.49ms | 13.35ms | 1.069× | 1.65% | PASS |
| mem_reads | `oltp_order_range` | 3.18ms | 3.16ms | 0.992× | 1.13% | PASS |
| mem_reads | `oltp_distinct_range` | 4.23ms | 4.19ms | 0.991× | 1.04% | PASS |
| mem_reads | `oltp_index_scan` | 4.62ms | 5.80ms | 1.257× | 1.25% | PASS |
| mem_reads | `select_random_points` | 18.14ms | 20.19ms | 1.113× | 1.25% | PASS |
| mem_reads | `select_random_ranges` | 4.30ms | 5.32ms | 1.238× | 1.49% | PASS |
| mem_reads | `covering_index_scan` | 5.09ms | 4.74ms | 0.932× | 2.23% | PASS |
| mem_reads | `groupby_scan` | 34.64ms | 35.53ms | 1.026× | 0.95% | PASS |
| mem_reads | `index_join` | 7.49ms | 9.81ms | 1.309× | 2.49% | PASS |
| mem_reads | `index_join_scan` | 5.03ms | 5.79ms | 1.151× | 1.72% | PASS |
| mem_reads | `types_table_scan` | 1.17s | 1.26s | 1.076× | 1.45% | PASS |
| mem_reads | `table_scan` | 1.56s | 1.42s | 0.909× | 6.85% | PASS |
| mem_reads | `oltp_read_only` | 130.69ms | 133.12ms | 1.019× | 2.01% | PASS |
| mem_writes | `oltp_bulk_insert` | 232.47ms | 337.53ms | 1.452× | 0.73% | PASS |
| mem_writes | `oltp_insert` | 22.83ms | 39.61ms | 1.735× | 1.16% | PASS |
| mem_writes | `oltp_update_index` | 75.30ms | 138.96ms | 1.845× | 1.80% | PASS |
| mem_writes | `oltp_update_non_index` | 50.59ms | 88.09ms | 1.741× | 0.60% | PASS |
| mem_writes | `oltp_delete_insert` | 53.71ms | 107.56ms | 2.002× | 1.07% | PASS |
| mem_writes | `oltp_write_only` | 30.92ms | 65.46ms | 2.117× | 1.14% | PASS |
| mem_writes | `types_delete_insert` | 35.03ms | 56.54ms | 1.614× | 1.32% | PASS |
| mem_writes | `oltp_read_write` | 96.79ms | 143.50ms | 1.483× | 1.73% | PASS |
| file_reads | `oltp_point_select` | 128.95ms | 68.44ms | 0.531× | 0.94% | PASS |
| file_reads | `oltp_range_select` | 25.05ms | 17.13ms | 0.684× | 3.01% | PASS |
| file_reads | `oltp_sum_range` | 23.16ms | 16.85ms | 0.728× | 1.96% | PASS |
| file_reads | `oltp_order_range` | 4.28ms | 3.57ms | 0.833× | 1.85% | PASS |
| file_reads | `oltp_distinct_range` | 5.36ms | 4.58ms | 0.855× | 1.57% | PASS |
| file_reads | `oltp_index_scan` | 14.44ms | 9.48ms | 0.656× | 1.23% | PASS |
| file_reads | `select_random_points` | 28.06ms | 23.47ms | 0.836× | 1.76% | PASS |
| file_reads | `select_random_ranges` | 13.82ms | 8.51ms | 0.616× | 1.03% | PASS |
| file_reads | `covering_index_scan` | 14.98ms | 8.00ms | 0.534× | 1.61% | PASS |
| file_reads | `groupby_scan` | 35.26ms | 35.72ms | 1.013× | 1.04% | PASS |
| file_reads | `index_join` | 12.62ms | 11.43ms | 0.906× | 1.59% | PASS |
| file_reads | `index_join_scan` | 5.99ms | 6.15ms | 1.026× | 1.71% | PASS |
| file_reads | `types_table_scan` | 1.22s | 1.26s | 1.035× | 2.33% | PASS |
| file_reads | `table_scan` | 1.48s | 1.40s | 0.942× | 4.34% | PASS |
| file_reads | `oltp_read_only` | 262.33ms | 175.95ms | 0.671× | 1.11% | PASS |
| file_writes | `oltp_bulk_insert` | 254.70ms | 369.29ms | 1.450× | 1.17% | PASS |
| file_writes | `oltp_insert` | 46.00ms | 53.35ms | 1.160× | 12.68% | PASS |
| file_writes | `oltp_update_index` | 119.21ms | 175.45ms | 1.472× | 1.54% | PASS |
| file_writes | `oltp_update_non_index` | 100.82ms | 116.81ms | 1.159× | 12.77% | PASS |
| file_writes | `oltp_delete_insert` | 91.97ms | 138.33ms | 1.504× | 1.10% | PASS |
| file_writes | `oltp_write_only` | 86.48ms | 88.94ms | 1.028× | 11.08% | PASS |
| file_writes | `types_delete_insert` | 56.95ms | 75.91ms | 1.333× | 1.27% | PASS |
| file_writes | `oltp_read_write` | 139.89ms | 159.45ms | 1.140× | 5.54% | PASS |
| ac_reads | `oltp_point_select` | 62.59ms | 66.89ms | 1.069× | 0.87% | PASS |
| ac_reads | `oltp_range_select` | 18.52ms | 17.05ms | 0.920× | 1.46% | PASS |
| ac_reads | `oltp_sum_range` | 16.80ms | 16.77ms | 0.998× | 1.44% | PASS |
| ac_reads | `oltp_order_range` | 3.78ms | 3.56ms | 0.942× | 1.18% | PASS |
| ac_reads | `oltp_distinct_range` | 4.79ms | 4.57ms | 0.954× | 0.87% | PASS |
| ac_reads | `oltp_index_scan` | 8.58ms | 9.57ms | 1.116× | 1.11% | PASS |
| ac_reads | `select_random_points` | 22.87ms | 24.12ms | 1.055× | 1.18% | PASS |
| ac_reads | `select_random_ranges` | 7.80ms | 8.57ms | 1.098× | 1.27% | PASS |
| ac_reads | `covering_index_scan` | 9.85ms | 8.09ms | 0.821× | 1.22% | PASS |
| ac_reads | `groupby_scan` | 35.70ms | 36.19ms | 1.014× | 0.69% | PASS |
| ac_reads | `index_join` | 10.59ms | 11.99ms | 1.132× | 2.13% | PASS |
| ac_reads | `index_join_scan` | 5.72ms | 6.41ms | 1.120× | 1.94% | PASS |
| ac_reads | `types_table_scan` | 1.41s | 1.33s | 0.943× | 1.03% | PASS |
| ac_reads | `table_scan` | 1.74s | 1.45s | 0.835× | 0.96% | PASS |
| ac_reads | `oltp_read_only` | 184.58ms | 183.19ms | 0.992× | 0.96% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.68ms | 64.48ms | 3.865× | 4.91% | PASS |
| ac_writes | `oltp_insert_ac` | 20.44ms | 81.50ms | 3.987× | 4.38% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.44ms | 98.86ms | 4.611× | 4.26% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.68ms | 80.29ms | 4.540× | 7.04% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 19.75ms | 91.54ms | 4.636× | 6.13% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.51ms | 89.28ms | 4.576× | 4.57% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.80ms | 81.27ms | 4.838× | 5.83% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.43ms | 99.94ms | 3.644× | 4.01% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.45ms | 24.93ms | 1.111× | 1.44% | PASS |
| mem_reads | `oltp_range_select` | 9.68ms | 10.69ms | 1.104× | 5.70% | PASS |
| mem_reads | `oltp_sum_range` | 9.23ms | 9.98ms | 1.081× | 4.79% | PASS |
| mem_reads | `oltp_order_range` | 2.32ms | 2.52ms | 1.086× | 2.38% | PASS |
| mem_reads | `oltp_distinct_range` | 3.16ms | 3.33ms | 1.054× | 2.96% | PASS |
| mem_reads | `oltp_index_scan` | 3.36ms | 4.15ms | 1.235× | 2.47% | PASS |
| mem_reads | `select_random_points` | 13.77ms | 15.52ms | 1.128× | 3.69% | PASS |
| mem_reads | `select_random_ranges` | 2.88ms | 3.80ms | 1.316× | 3.61% | PASS |
| mem_reads | `covering_index_scan` | 3.13ms | 3.17ms | 1.011× | 2.57% | PASS |
| mem_reads | `groupby_scan` | 27.01ms | 27.11ms | 1.004× | 2.81% | PASS |
| mem_reads | `index_join` | 5.53ms | 7.08ms | 1.280× | 1.99% | PASS |
| mem_reads | `index_join_scan` | 3.24ms | 4.57ms | 1.411× | 3.40% | PASS |
| mem_reads | `types_table_scan` | 893.83ms | 1.03s | 1.156× | 0.64% | PASS |
| mem_reads | `table_scan` | 1.03s | 1.13s | 1.091× | 0.66% | PASS |
| mem_reads | `oltp_read_only` | 90.36ms | 100.50ms | 1.112× | 1.91% | PASS |
| mem_writes | `oltp_bulk_insert` | 164.75ms | 230.96ms | 1.402× | 1.28% | PASS |
| mem_writes | `oltp_insert` | 14.79ms | 27.51ms | 1.860× | 2.75% | PASS |
| mem_writes | `oltp_update_index` | 51.45ms | 95.36ms | 1.854× | 1.35% | PASS |
| mem_writes | `oltp_update_non_index` | 35.50ms | 59.66ms | 1.680× | 2.06% | PASS |
| mem_writes | `oltp_delete_insert` | 36.28ms | 72.05ms | 1.986× | 2.11% | PASS |
| mem_writes | `oltp_write_only` | 19.73ms | 42.21ms | 2.140× | 2.71% | PASS |
| mem_writes | `types_delete_insert` | 22.77ms | 36.30ms | 1.595× | 1.80% | PASS |
| mem_writes | `oltp_read_write` | 57.53ms | 94.48ms | 1.642× | 1.72% | PASS |
| file_reads | `oltp_point_select` | 48.21ms | 33.93ms | 0.704× | 1.37% | PASS |
| file_reads | `oltp_range_select` | 13.25ms | 12.03ms | 0.908× | 2.01% | PASS |
| file_reads | `oltp_sum_range` | 12.74ms | 11.40ms | 0.895× | 3.03% | PASS |
| file_reads | `oltp_order_range` | 2.76ms | 2.72ms | 0.984× | 2.90% | PASS |
| file_reads | `oltp_distinct_range` | 3.59ms | 3.50ms | 0.974× | 1.46% | PASS |
| file_reads | `oltp_index_scan` | 6.25ms | 5.43ms | 0.869× | 5.20% | PASS |
| file_reads | `select_random_points` | 17.36ms | 17.53ms | 1.010× | 2.66% | PASS |
| file_reads | `select_random_ranges` | 5.82ms | 4.96ms | 0.852× | 3.85% | PASS |
| file_reads | `covering_index_scan` | 5.98ms | 4.26ms | 0.713× | 2.94% | PASS |
| file_reads | `groupby_scan` | 27.37ms | 27.23ms | 0.995× | 1.50% | PASS |
| file_reads | `index_join` | 7.08ms | 8.21ms | 1.160× | 2.99% | PASS |
| file_reads | `index_join_scan` | 3.71ms | 4.90ms | 1.320× | 2.45% | PASS |
| file_reads | `types_table_scan` | 887.96ms | 1.03s | 1.163× | 0.53% | PASS |
| file_reads | `table_scan` | 1.03s | 1.12s | 1.093× | 0.59% | PASS |
| file_reads | `oltp_read_only` | 130.43ms | 114.17ms | 0.875× | 1.02% | PASS |
| file_writes | `oltp_bulk_insert` | 231.35ms | 309.82ms | 1.339× | 4.51% | PASS |
| file_writes | `oltp_insert` | 40.38ms | 55.78ms | 1.382× | 2.67% | PASS |
| file_writes | `oltp_update_index` | 173.88ms | 185.37ms | 1.066× | 3.38% | PASS |
| file_writes | `oltp_update_non_index` | 135.92ms | 124.04ms | 0.913× | 7.11% | PASS |
| file_writes | `oltp_delete_insert` | 143.38ms | 145.82ms | 1.017× | 5.78% | PASS |
| file_writes | `oltp_write_only` | 105.98ms | 103.44ms | 0.976× | 3.57% | PASS |
| file_writes | `types_delete_insert` | 90.45ms | 85.85ms | 0.949× | 9.19% | PASS |
| file_writes | `oltp_read_write` | 145.03ms | 158.38ms | 1.092× | 3.40% | PASS |
| ac_reads | `oltp_point_select` | 31.34ms | 34.40ms | 1.098× | 1.79% | PASS |
| ac_reads | `oltp_range_select` | 11.86ms | 12.21ms | 1.030× | 2.40% | PASS |
| ac_reads | `oltp_sum_range` | 11.54ms | 11.56ms | 1.002× | 2.30% | PASS |
| ac_reads | `oltp_order_range` | 2.65ms | 2.72ms | 1.029× | 2.56% | PASS |
| ac_reads | `oltp_distinct_range` | 3.46ms | 3.52ms | 1.018× | 1.50% | PASS |
| ac_reads | `oltp_index_scan` | 4.75ms | 5.56ms | 1.170× | 3.26% | PASS |
| ac_reads | `select_random_points` | 14.98ms | 16.89ms | 1.127× | 2.12% | PASS |
| ac_reads | `select_random_ranges` | 3.84ms | 4.71ms | 1.228× | 2.98% | PASS |
| ac_reads | `covering_index_scan` | 4.07ms | 4.04ms | 0.993× | 1.98% | PASS |
| ac_reads | `groupby_scan` | 26.62ms | 27.10ms | 1.018× | 1.86% | PASS |
| ac_reads | `index_join` | 5.96ms | 7.43ms | 1.245× | 2.35% | PASS |
| ac_reads | `index_join_scan` | 3.50ms | 4.72ms | 1.348× | 3.11% | PASS |
| ac_reads | `types_table_scan` | 885.28ms | 1.03s | 1.161× | 0.49% | PASS |
| ac_reads | `table_scan` | 1.03s | 1.12s | 1.095× | 0.58% | PASS |
| ac_reads | `oltp_read_only` | 103.50ms | 114.77ms | 1.109× | 2.60% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 32.44ms | 87.44ms | 2.695× | 6.75% | PASS |
| ac_writes | `oltp_insert_ac` | 36.34ms | 110.94ms | 3.053× | 9.74% | PASS |
| ac_writes | `oltp_update_index_ac` | 35.80ms | 120.17ms | 3.357× | 11.31% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 33.36ms | 106.05ms | 3.179× | 14.93% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 35.74ms | 116.16ms | 3.250× | 18.27% | PASS |
| ac_writes | `oltp_write_only_ac` | 35.27ms | 117.92ms | 3.344× | 19.20% | PASS |
| ac_writes | `types_delete_insert_ac` | 32.61ms | 112.26ms | 3.442× | 15.71% | PASS |
| ac_writes | `oltp_read_write_ac` | 46.06ms | 139.54ms | 3.030× | 30.23% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.04ms | 26.68ms | 1.158× | 1.55% | PASS |
| mem_reads | `oltp_range_select` | 14.26ms | 15.85ms | 1.111× | 2.20% | PASS |
| mem_reads | `oltp_sum_range` | 12.97ms | 14.74ms | 1.136× | 1.27% | PASS |
| mem_reads | `oltp_order_range` | 2.74ms | 3.03ms | 1.104× | 1.32% | PASS |
| mem_reads | `oltp_distinct_range` | 3.55ms | 3.82ms | 1.074× | 0.86% | PASS |
| mem_reads | `oltp_index_scan` | 3.32ms | 4.25ms | 1.277× | 1.60% | PASS |
| mem_reads | `select_random_points` | 21.40ms | 24.18ms | 1.130× | 0.84% | PASS |
| mem_reads | `select_random_ranges` | 5.43ms | 6.17ms | 1.136× | 0.89% | PASS |
| mem_reads | `covering_index_scan` | 2.94ms | 3.04ms | 1.034× | 1.82% | PASS |
| mem_reads | `groupby_scan` | 28.87ms | 30.18ms | 1.045× | 0.45% | PASS |
| mem_reads | `index_join` | 6.17ms | 7.89ms | 1.278× | 1.11% | PASS |
| mem_reads | `index_join_scan` | 2.88ms | 4.10ms | 1.425× | 1.13% | PASS |
| mem_reads | `types_table_scan` | 873.16ms | 1.02s | 1.174× | 0.26% | PASS |
| mem_reads | `table_scan` | 988.69ms | 1.11s | 1.118× | 0.27% | PASS |
| mem_reads | `oltp_read_only` | 111.08ms | 121.78ms | 1.096× | 0.35% | PASS |
| mem_writes | `oltp_bulk_insert` | 170.22ms | 230.84ms | 1.356× | 0.48% | PASS |
| mem_writes | `oltp_insert` | 13.79ms | 24.39ms | 1.769× | 0.74% | PASS |
| mem_writes | `oltp_update_index` | 47.44ms | 81.62ms | 1.721× | 1.26% | PASS |
| mem_writes | `oltp_update_non_index` | 33.91ms | 55.76ms | 1.645× | 1.81% | PASS |
| mem_writes | `oltp_delete_insert` | 34.78ms | 65.22ms | 1.875× | 1.18% | PASS |
| mem_writes | `oltp_write_only` | 18.26ms | 38.61ms | 2.115× | 2.03% | PASS |
| mem_writes | `types_delete_insert` | 22.36ms | 35.97ms | 1.609× | 0.92% | PASS |
| mem_writes | `oltp_read_write` | 68.55ms | 103.73ms | 1.513× | 1.16% | PASS |
| file_reads | `oltp_point_select` | 49.26ms | 35.56ms | 0.722× | 0.78% | PASS |
| file_reads | `oltp_range_select` | 17.06ms | 16.91ms | 0.991× | 1.54% | PASS |
| file_reads | `oltp_sum_range` | 16.22ms | 15.81ms | 0.975× | 1.72% | PASS |
| file_reads | `oltp_order_range` | 3.11ms | 3.14ms | 1.009× | 1.43% | PASS |
| file_reads | `oltp_distinct_range` | 3.95ms | 3.94ms | 0.999× | 1.05% | PASS |
| file_reads | `oltp_index_scan` | 6.07ms | 5.14ms | 0.847× | 1.34% | PASS |
| file_reads | `select_random_points` | 24.46ms | 25.18ms | 1.030× | 1.18% | PASS |
| file_reads | `select_random_ranges` | 8.06ms | 7.13ms | 0.884× | 0.68% | PASS |
| file_reads | `covering_index_scan` | 5.71ms | 3.93ms | 0.687× | 1.26% | PASS |
| file_reads | `groupby_scan` | 29.29ms | 30.28ms | 1.034× | 0.85% | PASS |
| file_reads | `index_join` | 7.68ms | 8.52ms | 1.110× | 0.61% | PASS |
| file_reads | `index_join_scan` | 3.31ms | 4.52ms | 1.364× | 1.66% | PASS |
| file_reads | `types_table_scan` | 871.45ms | 1.03s | 1.177× | 0.15% | PASS |
| file_reads | `table_scan` | 990.42ms | 1.11s | 1.118× | 0.17% | PASS |
| file_reads | `oltp_read_only` | 147.93ms | 134.67ms | 0.910× | 0.48% | PASS |
| file_writes | `oltp_bulk_insert` | 222.52ms | 290.46ms | 1.305× | 1.69% | PASS |
| file_writes | `oltp_insert` | 27.84ms | 44.90ms | 1.613× | 3.16% | PASS |
| file_writes | `oltp_update_index` | 150.75ms | 151.50ms | 1.005× | 0.78% | PASS |
| file_writes | `oltp_update_non_index` | 124.37ms | 111.08ms | 0.893× | 1.15% | PASS |
| file_writes | `oltp_delete_insert` | 127.57ms | 127.20ms | 0.997× | 11.97% | PASS |
| file_writes | `oltp_write_only` | 90.34ms | 90.11ms | 0.997× | 2.00% | PASS |
| file_writes | `types_delete_insert` | 82.07ms | 73.86ms | 0.900× | 6.86% | PASS |
| file_writes | `oltp_read_write` | 141.45ms | 155.36ms | 1.098× | 4.36% | PASS |
| ac_reads | `oltp_point_select` | 31.42ms | 35.36ms | 1.125× | 0.85% | PASS |
| ac_reads | `oltp_range_select` | 15.29ms | 16.87ms | 1.103× | 1.41% | PASS |
| ac_reads | `oltp_sum_range` | 14.56ms | 15.83ms | 1.087× | 0.94% | PASS |
| ac_reads | `oltp_order_range` | 2.94ms | 3.14ms | 1.068× | 1.44% | PASS |
| ac_reads | `oltp_distinct_range` | 3.78ms | 3.95ms | 1.046× | 1.27% | PASS |
| ac_reads | `oltp_index_scan` | 4.25ms | 5.15ms | 1.212× | 2.14% | PASS |
| ac_reads | `select_random_points` | 22.36ms | 25.08ms | 1.121× | 0.91% | PASS |
| ac_reads | `select_random_ranges` | 6.33ms | 7.12ms | 1.125× | 1.41% | PASS |
| ac_reads | `covering_index_scan` | 3.86ms | 3.92ms | 1.017× | 1.35% | PASS |
| ac_reads | `groupby_scan` | 29.30ms | 30.30ms | 1.034× | 1.30% | PASS |
| ac_reads | `index_join` | 6.80ms | 8.51ms | 1.251× | 1.36% | PASS |
| ac_reads | `index_join_scan` | 3.17ms | 4.46ms | 1.408× | 1.74% | PASS |
| ac_reads | `types_table_scan` | 870.03ms | 1.03s | 1.179× | 0.21% | PASS |
| ac_reads | `table_scan` | 989.87ms | 1.11s | 1.119× | 0.18% | PASS |
| ac_reads | `oltp_read_only` | 122.80ms | 134.52ms | 1.095× | 0.41% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 29.36ms | 78.52ms | 2.674× | 3.89% | PASS |
| ac_writes | `oltp_insert_ac` | 30.74ms | 90.20ms | 2.935× | 5.45% | PASS |
| ac_writes | `oltp_update_index_ac` | 31.15ms | 99.03ms | 3.179× | 5.67% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 29.54ms | 85.02ms | 2.878× | 5.06% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 30.00ms | 95.45ms | 3.182× | 6.51% | PASS |
| ac_writes | `oltp_write_only_ac` | 30.16ms | 92.96ms | 3.082× | 5.80% | PASS |
| ac_writes | `types_delete_insert_ac` | 28.97ms | 86.11ms | 2.973× | 9.13% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.93ms | 99.92ms | 2.945× | 6.44% | PASS |

</details>

## Version-control latency

Wall time: 2m 25s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 85.00ms | 200.00ms | 42.5% | 0.85% | PASS |
| `status_dirty_many_tables` | 87.96ms | 200.00ms | 44.0% | 0.79% | PASS |
| `diff_regular_working_one_table` | 80.67ms | 150.00ms | 53.8% | 0.91% | PASS |
| `diff_regular_working_many_tables` | 93.55ms | 200.00ms | 46.8% | 0.80% | PASS |
| `diff_stat_working_many_tables` | 94.07ms | 200.00ms | 47.0% | 1.08% | PASS |
| `diff_schema_working_many_tables` | 94.30ms | 200.00ms | 47.1% | 0.96% | PASS |
| `branch_list_many_branches` | 23.77ms | 100.00ms | 23.8% | 1.96% | PASS |
| `branch_create_delete` | 26.46ms | 100.00ms | 26.5% | 2.10% | PASS |
| `checkout_branch_clean` | 57.56ms | 200.00ms | 28.8% | 1.43% | PASS |
| `merge_data_no_conflicts` | 30.47ms | 150.00ms | 20.3% | 2.27% | PASS |
| `merge_schema_no_conflicts` | 24.65ms | 100.00ms | 24.6% | 2.44% | PASS |
| `merge_data_conflicts` | 129.02ms | 250.00ms | 51.6% | 0.45% | PASS |
| `merge_data_conflicts_with_resolve` | 128.79ms | 250.00ms | 51.5% | 0.50% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
