# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-06 14:40 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260831.293.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/34034915805)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 7s | 8.97s | 11.54s | 1.286× | 1.46% | **PASS** |
| textpk | 69 | 55 | 1h 34m 28s | 10.50s | 12.14s | 1.156× | 1.91% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 10s | 10.11s | 11.61s | 1.149× | 1.01% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 17s | 10.68s | 11.93s | 1.117× | 1.12% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.09ms | 30.53ms | 1.217× | 1.76% | PASS |
| mem_reads | `oltp_range_select` | 10.77ms | 13.36ms | 1.240× | 1.76% | PASS |
| mem_reads | `oltp_sum_range` | 10.03ms | 12.60ms | 1.256× | 1.77% | PASS |
| mem_reads | `oltp_order_range` | 2.62ms | 3.01ms | 1.149× | 1.06% | PASS |
| mem_reads | `oltp_distinct_range` | 3.67ms | 4.09ms | 1.112× | 1.06% | PASS |
| mem_reads | `oltp_index_scan` | 4.09ms | 5.80ms | 1.418× | 2.17% | PASS |
| mem_reads | `select_random_points` | 11.03ms | 11.73ms | 1.064× | 3.02% | PASS |
| mem_reads | `select_random_ranges` | 3.15ms | 4.07ms | 1.291× | 1.62% | PASS |
| mem_reads | `covering_index_scan` | 4.35ms | 4.56ms | 1.049× | 1.51% | PASS |
| mem_reads | `groupby_scan` | 30.01ms | 32.90ms | 1.096× | 0.77% | PASS |
| mem_reads | `index_join` | 6.01ms | 8.29ms | 1.378× | 1.79% | PASS |
| mem_reads | `index_join_scan` | 3.44ms | 4.63ms | 1.348× | 1.59% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.33s | 1.284× | 0.67% | PASS |
| mem_reads | `table_scan` | 1.16s | 1.39s | 1.196× | 0.88% | PASS |
| mem_reads | `oltp_read_only` | 103.94ms | 124.35ms | 1.196× | 1.57% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.22ms | 251.40ms | 1.387× | 1.33% | PASS |
| mem_writes | `oltp_insert` | 15.52ms | 28.53ms | 1.838× | 1.09% | PASS |
| mem_writes | `oltp_update_index` | 50.21ms | 104.50ms | 2.081× | 1.25% | PASS |
| mem_writes | `oltp_update_non_index` | 34.45ms | 59.31ms | 1.722× | 1.43% | PASS |
| mem_writes | `oltp_delete_insert` | 45.34ms | 79.11ms | 1.745× | 1.23% | PASS |
| mem_writes | `oltp_write_only` | 22.15ms | 45.15ms | 2.038× | 0.88% | PASS |
| mem_writes | `types_delete_insert` | 24.68ms | 40.44ms | 1.638× | 1.75% | PASS |
| mem_writes | `oltp_read_write` | 67.69ms | 110.40ms | 1.631× | 1.41% | PASS |
| file_reads | `oltp_point_select` | 98.85ms | 55.73ms | 0.564× | 0.89% | PASS |
| file_reads | `oltp_range_select` | 18.81ms | 15.93ms | 0.847× | 1.22% | PASS |
| file_reads | `oltp_sum_range` | 18.04ms | 15.19ms | 0.842× | 0.94% | PASS |
| file_reads | `oltp_order_range` | 3.49ms | 3.38ms | 0.967× | 1.80% | PASS |
| file_reads | `oltp_distinct_range` | 4.60ms | 4.46ms | 0.970× | 1.54% | PASS |
| file_reads | `oltp_index_scan` | 11.92ms | 8.36ms | 0.701× | 1.01% | PASS |
| file_reads | `select_random_points` | 18.65ms | 14.02ms | 0.752× | 2.17% | PASS |
| file_reads | `select_random_ranges` | 10.62ms | 6.60ms | 0.621× | 0.79% | PASS |
| file_reads | `covering_index_scan` | 12.20ms | 7.10ms | 0.582× | 0.97% | PASS |
| file_reads | `groupby_scan` | 31.13ms | 33.35ms | 1.071× | 0.91% | PASS |
| file_reads | `index_join` | 10.62ms | 10.29ms | 0.969× | 0.98% | PASS |
| file_reads | `index_join_scan` | 4.53ms | 5.05ms | 1.115× | 1.97% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.33s | 1.280× | 0.62% | PASS |
| file_reads | `table_scan` | 1.25s | 1.42s | 1.134× | 0.90% | PASS |
| file_reads | `oltp_read_only` | 220.21ms | 165.36ms | 0.751× | 0.92% | PASS |
| file_writes | `oltp_bulk_insert` | 194.85ms | 272.69ms | 1.399× | 1.41% | PASS |
| file_writes | `oltp_insert` | 22.88ms | 36.12ms | 1.579× | 1.54% | PASS |
| file_writes | `oltp_update_index` | 80.78ms | 132.68ms | 1.642× | 0.87% | PASS |
| file_writes | `oltp_update_non_index` | 60.47ms | 83.33ms | 1.378× | 1.07% | PASS |
| file_writes | `oltp_delete_insert` | 70.91ms | 102.10ms | 1.440× | 1.64% | PASS |
| file_writes | `oltp_write_only` | 45.62ms | 64.88ms | 1.422× | 1.47% | PASS |
| file_writes | `types_delete_insert` | 41.08ms | 54.45ms | 1.326× | 1.17% | PASS |
| file_writes | `oltp_read_write` | 96.25ms | 132.99ms | 1.382× | 2.09% | PASS |
| ac_reads | `oltp_point_select` | 49.73ms | 56.44ms | 1.135× | 1.29% | PASS |
| ac_reads | `oltp_range_select` | 13.66ms | 15.95ms | 1.168× | 2.07% | PASS |
| ac_reads | `oltp_sum_range` | 12.64ms | 15.10ms | 1.195× | 1.45% | PASS |
| ac_reads | `oltp_order_range` | 2.98ms | 3.34ms | 1.122× | 1.56% | PASS |
| ac_reads | `oltp_distinct_range` | 4.00ms | 4.43ms | 1.107× | 1.06% | PASS |
| ac_reads | `oltp_index_scan` | 6.71ms | 8.34ms | 1.244× | 1.50% | PASS |
| ac_reads | `select_random_points` | 13.51ms | 14.05ms | 1.040× | 2.48% | PASS |
| ac_reads | `select_random_ranges` | 5.64ms | 6.62ms | 1.174× | 1.01% | PASS |
| ac_reads | `covering_index_scan` | 7.18ms | 7.14ms | 0.995× | 1.46% | PASS |
| ac_reads | `groupby_scan` | 30.40ms | 33.19ms | 1.092× | 0.90% | PASS |
| ac_reads | `index_join` | 7.66ms | 10.23ms | 1.336× | 1.62% | PASS |
| ac_reads | `index_join_scan` | 4.02ms | 5.13ms | 1.276× | 2.36% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.33s | 1.273× | 1.31% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.41s | 1.174× | 1.62% | PASS |
| ac_reads | `oltp_read_only` | 144.30ms | 164.08ms | 1.137× | 0.93% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.02ms | 80.52ms | 3.497× | 7.04% | PASS |
| ac_writes | `oltp_insert_ac` | 24.50ms | 97.19ms | 3.968× | 4.80% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.40ms | 111.69ms | 4.077× | 4.71% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.15ms | 89.62ms | 4.047× | 4.76% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.85ms | 101.88ms | 4.100× | 6.15% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.19ms | 101.86ms | 4.043× | 6.73% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.62ms | 91.22ms | 4.033× | 5.28% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.02ms | 110.27ms | 3.555× | 4.92% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.56ms | 37.65ms | 1.274× | 1.61% | PASS |
| mem_reads | `oltp_range_select` | 12.64ms | 14.12ms | 1.117× | 2.95% | PASS |
| mem_reads | `oltp_sum_range` | 11.84ms | 14.21ms | 1.200× | 1.73% | PASS |
| mem_reads | `oltp_order_range` | 3.04ms | 3.19ms | 1.049× | 1.17% | PASS |
| mem_reads | `oltp_distinct_range` | 4.12ms | 4.25ms | 1.031× | 0.98% | PASS |
| mem_reads | `oltp_index_scan` | 4.69ms | 6.52ms | 1.390× | 1.85% | PASS |
| mem_reads | `select_random_points` | 19.19ms | 22.02ms | 1.147× | 1.91% | PASS |
| mem_reads | `select_random_ranges` | 4.18ms | 5.33ms | 1.276× | 1.36% | PASS |
| mem_reads | `covering_index_scan` | 5.12ms | 4.82ms | 0.940× | 3.29% | PASS |
| mem_reads | `groupby_scan` | 32.43ms | 33.91ms | 1.046× | 1.03% | PASS |
| mem_reads | `index_join` | 7.08ms | 9.70ms | 1.369× | 3.31% | PASS |
| mem_reads | `index_join_scan` | 4.61ms | 5.55ms | 1.204× | 2.12% | PASS |
| mem_reads | `types_table_scan` | 1.16s | 1.27s | 1.102× | 5.08% | PASS |
| mem_reads | `table_scan` | 1.29s | 1.40s | 1.086× | 5.24% | PASS |
| mem_reads | `oltp_read_only` | 122.21ms | 137.91ms | 1.128× | 0.96% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.01ms | 357.14ms | 1.520× | 0.92% | PASS |
| mem_writes | `oltp_insert` | 21.40ms | 39.97ms | 1.867× | 0.94% | PASS |
| mem_writes | `oltp_update_index` | 69.52ms | 131.81ms | 1.896× | 0.90% | PASS |
| mem_writes | `oltp_update_non_index` | 48.93ms | 87.80ms | 1.794× | 1.37% | PASS |
| mem_writes | `oltp_delete_insert` | 50.32ms | 104.19ms | 2.071× | 0.85% | PASS |
| mem_writes | `oltp_write_only` | 29.79ms | 63.52ms | 2.132× | 1.46% | PASS |
| mem_writes | `types_delete_insert` | 33.90ms | 57.05ms | 1.683× | 0.85% | PASS |
| mem_writes | `oltp_read_write` | 91.82ms | 145.32ms | 1.583× | 1.81% | PASS |
| file_reads | `oltp_point_select` | 105.81ms | 64.14ms | 0.606× | 1.42% | PASS |
| file_reads | `oltp_range_select` | 21.35ms | 16.98ms | 0.795× | 2.90% | PASS |
| file_reads | `oltp_sum_range` | 20.19ms | 17.05ms | 0.845× | 2.16% | PASS |
| file_reads | `oltp_order_range` | 3.90ms | 3.58ms | 0.919× | 3.37% | PASS |
| file_reads | `oltp_distinct_range` | 4.87ms | 4.59ms | 0.944× | 2.45% | PASS |
| file_reads | `oltp_index_scan` | 12.31ms | 9.17ms | 0.745× | 2.61% | PASS |
| file_reads | `select_random_points` | 26.73ms | 24.71ms | 0.924× | 2.11% | PASS |
| file_reads | `select_random_ranges` | 11.71ms | 7.94ms | 0.678× | 0.82% | PASS |
| file_reads | `covering_index_scan` | 13.04ms | 7.46ms | 0.572× | 4.86% | PASS |
| file_reads | `groupby_scan` | 33.32ms | 34.44ms | 1.033× | 1.23% | PASS |
| file_reads | `index_join` | 11.91ms | 11.41ms | 0.958× | 3.64% | PASS |
| file_reads | `index_join_scan` | 5.72ms | 6.05ms | 1.058× | 2.48% | PASS |
| file_reads | `types_table_scan` | 1.07s | 1.23s | 1.149× | 1.73% | PASS |
| file_reads | `table_scan` | 1.29s | 1.39s | 1.081× | 5.03% | PASS |
| file_reads | `oltp_read_only` | 235.40ms | 177.15ms | 0.753× | 1.31% | PASS |
| file_writes | `oltp_bulk_insert` | 254.61ms | 389.51ms | 1.530× | 1.21% | PASS |
| file_writes | `oltp_insert` | 48.40ms | 53.90ms | 1.113× | 17.67% | PASS |
| file_writes | `oltp_update_index` | 117.28ms | 172.90ms | 1.474× | 1.86% | PASS |
| file_writes | `oltp_update_non_index` | 104.17ms | 115.01ms | 1.104× | 15.79% | PASS |
| file_writes | `oltp_delete_insert` | 93.40ms | 136.91ms | 1.466× | 1.45% | PASS |
| file_writes | `oltp_write_only` | 87.98ms | 86.97ms | 0.988× | 10.72% | PASS |
| file_writes | `types_delete_insert` | 57.58ms | 76.84ms | 1.334× | 1.48% | PASS |
| file_writes | `oltp_read_write` | 138.45ms | 167.64ms | 1.211× | 6.53% | PASS |
| ac_reads | `oltp_point_select` | 55.88ms | 64.40ms | 1.152× | 1.05% | PASS |
| ac_reads | `oltp_range_select` | 16.91ms | 17.02ms | 1.006× | 2.76% | PASS |
| ac_reads | `oltp_sum_range` | 16.02ms | 17.29ms | 1.079× | 2.44% | PASS |
| ac_reads | `oltp_order_range` | 3.58ms | 3.62ms | 1.010× | 1.82% | PASS |
| ac_reads | `oltp_distinct_range` | 4.66ms | 4.73ms | 1.016× | 1.59% | PASS |
| ac_reads | `oltp_index_scan` | 7.79ms | 9.39ms | 1.206× | 2.14% | PASS |
| ac_reads | `select_random_points` | 22.66ms | 25.77ms | 1.137× | 2.19% | PASS |
| ac_reads | `select_random_ranges` | 6.92ms | 7.97ms | 1.152× | 1.02% | PASS |
| ac_reads | `covering_index_scan` | 8.60ms | 7.51ms | 0.873× | 1.09% | PASS |
| ac_reads | `groupby_scan` | 33.25ms | 34.92ms | 1.050× | 0.88% | PASS |
| ac_reads | `index_join` | 9.74ms | 11.72ms | 1.203× | 2.70% | PASS |
| ac_reads | `index_join_scan` | 5.43ms | 6.17ms | 1.137× | 5.09% | PASS |
| ac_reads | `types_table_scan` | 1.30s | 1.30s | 1.001× | 0.80% | PASS |
| ac_reads | `table_scan` | 1.58s | 1.44s | 0.914× | 0.88% | PASS |
| ac_reads | `oltp_read_only` | 169.32ms | 182.52ms | 1.078× | 1.45% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.40ms | 84.52ms | 3.612× | 5.17% | PASS |
| ac_writes | `oltp_insert_ac` | 26.04ms | 98.76ms | 3.793× | 5.49% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.34ms | 118.05ms | 4.024× | 7.50% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.70ms | 97.33ms | 4.107× | 7.14% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.57ms | 108.70ms | 4.251× | 4.88% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.44ms | 104.75ms | 4.117× | 4.52% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.19ms | 98.16ms | 4.425× | 8.03% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.89ms | 121.89ms | 3.706× | 6.76% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.21ms | 34.42ms | 1.139× | 1.47% | PASS |
| mem_reads | `oltp_range_select` | 13.34ms | 13.61ms | 1.020× | 1.62% | PASS |
| mem_reads | `oltp_sum_range` | 12.04ms | 13.00ms | 1.080× | 1.10% | PASS |
| mem_reads | `oltp_order_range` | 3.06ms | 3.18ms | 1.039× | 0.78% | PASS |
| mem_reads | `oltp_distinct_range` | 4.17ms | 4.21ms | 1.009× | 0.89% | PASS |
| mem_reads | `oltp_index_scan` | 4.64ms | 5.90ms | 1.272× | 1.76% | PASS |
| mem_reads | `select_random_points` | 17.66ms | 19.76ms | 1.119× | 0.92% | PASS |
| mem_reads | `select_random_ranges` | 4.22ms | 5.21ms | 1.235× | 1.31% | PASS |
| mem_reads | `covering_index_scan` | 4.61ms | 4.46ms | 0.967× | 2.12% | PASS |
| mem_reads | `groupby_scan` | 33.82ms | 35.17ms | 1.040× | 0.78% | PASS |
| mem_reads | `index_join` | 6.88ms | 9.12ms | 1.325× | 2.20% | PASS |
| mem_reads | `index_join_scan` | 4.35ms | 5.55ms | 1.277× | 1.01% | PASS |
| mem_reads | `types_table_scan` | 1.13s | 1.24s | 1.099× | 0.82% | PASS |
| mem_reads | `table_scan` | 1.33s | 1.36s | 1.027× | 1.45% | PASS |
| mem_reads | `oltp_read_only` | 119.13ms | 128.93ms | 1.082× | 1.06% | PASS |
| mem_writes | `oltp_bulk_insert` | 237.42ms | 334.02ms | 1.407× | 1.01% | PASS |
| mem_writes | `oltp_insert` | 20.56ms | 39.50ms | 1.921× | 0.96% | PASS |
| mem_writes | `oltp_update_index` | 69.26ms | 130.84ms | 1.889× | 0.76% | PASS |
| mem_writes | `oltp_update_non_index` | 49.17ms | 84.64ms | 1.721× | 0.61% | PASS |
| mem_writes | `oltp_delete_insert` | 49.43ms | 103.60ms | 2.096× | 0.57% | PASS |
| mem_writes | `oltp_write_only` | 28.11ms | 62.99ms | 2.241× | 0.50% | PASS |
| mem_writes | `types_delete_insert` | 32.01ms | 52.42ms | 1.637× | 1.11% | PASS |
| mem_writes | `oltp_read_write` | 80.82ms | 133.34ms | 1.650× | 0.68% | PASS |
| file_reads | `oltp_point_select` | 126.61ms | 66.42ms | 0.525× | 0.74% | PASS |
| file_reads | `oltp_range_select` | 23.64ms | 16.86ms | 0.713× | 0.70% | PASS |
| file_reads | `oltp_sum_range` | 22.23ms | 16.33ms | 0.734× | 0.83% | PASS |
| file_reads | `oltp_order_range` | 4.12ms | 3.56ms | 0.864× | 0.89% | PASS |
| file_reads | `oltp_distinct_range` | 5.19ms | 4.56ms | 0.878× | 0.95% | PASS |
| file_reads | `oltp_index_scan` | 14.60ms | 9.28ms | 0.635× | 0.84% | PASS |
| file_reads | `select_random_points` | 27.94ms | 22.70ms | 0.813× | 1.26% | PASS |
| file_reads | `select_random_ranges` | 13.94ms | 8.45ms | 0.606× | 0.62% | PASS |
| file_reads | `covering_index_scan` | 15.11ms | 7.89ms | 0.523× | 0.74% | PASS |
| file_reads | `groupby_scan` | 34.65ms | 35.48ms | 1.024× | 0.53% | PASS |
| file_reads | `index_join` | 12.63ms | 11.18ms | 0.885× | 0.99% | PASS |
| file_reads | `index_join_scan` | 5.58ms | 6.05ms | 1.085× | 1.42% | PASS |
| file_reads | `types_table_scan` | 1.13s | 1.24s | 1.097× | 0.48% | PASS |
| file_reads | `table_scan` | 1.37s | 1.37s | 1.003× | 2.58% | PASS |
| file_reads | `oltp_read_only` | 265.69ms | 177.87ms | 0.669× | 0.88% | PASS |
| file_writes | `oltp_bulk_insert` | 261.00ms | 360.70ms | 1.382× | 0.81% | PASS |
| file_writes | `oltp_insert` | 32.55ms | 53.31ms | 1.638× | 1.21% | PASS |
| file_writes | `oltp_update_index` | 108.01ms | 167.11ms | 1.547× | 0.87% | PASS |
| file_writes | `oltp_update_non_index` | 80.80ms | 110.05ms | 1.362× | 1.27% | PASS |
| file_writes | `oltp_delete_insert` | 82.78ms | 133.46ms | 1.612× | 0.83% | PASS |
| file_writes | `oltp_write_only` | 56.51ms | 87.13ms | 1.542× | 1.43% | PASS |
| file_writes | `types_delete_insert` | 53.22ms | 72.08ms | 1.355× | 1.44% | PASS |
| file_writes | `oltp_read_write` | 111.78ms | 157.60ms | 1.410× | 1.53% | PASS |
| ac_reads | `oltp_point_select` | 62.11ms | 66.78ms | 1.075× | 0.75% | PASS |
| ac_reads | `oltp_range_select` | 17.25ms | 16.92ms | 0.981× | 1.09% | PASS |
| ac_reads | `oltp_sum_range` | 15.90ms | 16.43ms | 1.033× | 1.29% | PASS |
| ac_reads | `oltp_order_range` | 3.53ms | 3.56ms | 1.010× | 1.20% | PASS |
| ac_reads | `oltp_distinct_range` | 4.54ms | 4.56ms | 1.005× | 0.87% | PASS |
| ac_reads | `oltp_index_scan` | 8.20ms | 9.39ms | 1.145× | 0.81% | PASS |
| ac_reads | `select_random_points` | 21.76ms | 23.14ms | 1.064× | 1.06% | PASS |
| ac_reads | `select_random_ranges` | 7.50ms | 8.47ms | 1.129× | 1.17% | PASS |
| ac_reads | `covering_index_scan` | 8.44ms | 7.91ms | 0.937× | 0.78% | PASS |
| ac_reads | `groupby_scan` | 34.05ms | 35.61ms | 1.046× | 0.66% | PASS |
| ac_reads | `index_join` | 9.21ms | 11.29ms | 1.225× | 1.52% | PASS |
| ac_reads | `index_join_scan` | 4.95ms | 6.07ms | 1.226× | 0.85% | PASS |
| ac_reads | `types_table_scan` | 1.12s | 1.24s | 1.100× | 0.60% | PASS |
| ac_reads | `table_scan` | 1.33s | 1.36s | 1.024× | 1.23% | PASS |
| ac_reads | `oltp_read_only` | 168.79ms | 175.55ms | 1.040× | 0.82% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.91ms | 63.65ms | 3.765× | 3.99% | PASS |
| ac_writes | `oltp_insert_ac` | 18.16ms | 83.72ms | 4.610× | 3.89% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.41ms | 96.48ms | 4.727× | 3.41% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.21ms | 75.04ms | 4.628× | 6.84% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.48ms | 87.81ms | 4.753× | 5.21% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.26ms | 86.34ms | 4.484× | 5.28% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.87ms | 75.61ms | 4.765× | 5.01% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.93ms | 93.30ms | 3.743× | 4.30% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 35.42ms | 38.92ms | 1.099× | 1.13% | PASS |
| mem_reads | `oltp_range_select` | 20.25ms | 20.11ms | 0.993× | 1.90% | PASS |
| mem_reads | `oltp_sum_range` | 18.43ms | 19.26ms | 1.045× | 0.90% | PASS |
| mem_reads | `oltp_order_range` | 3.77ms | 3.79ms | 1.003× | 1.10% | PASS |
| mem_reads | `oltp_distinct_range` | 4.82ms | 4.89ms | 1.014× | 0.83% | PASS |
| mem_reads | `oltp_index_scan` | 4.76ms | 6.15ms | 1.291× | 2.45% | PASS |
| mem_reads | `select_random_points` | 28.24ms | 31.19ms | 1.105× | 1.70% | PASS |
| mem_reads | `select_random_ranges` | 7.58ms | 8.34ms | 1.100× | 1.19% | PASS |
| mem_reads | `covering_index_scan` | 4.40ms | 4.50ms | 1.022× | 3.08% | PASS |
| mem_reads | `groupby_scan` | 38.79ms | 40.06ms | 1.033× | 0.63% | PASS |
| mem_reads | `index_join` | 8.23ms | 10.58ms | 1.286× | 1.90% | PASS |
| mem_reads | `index_join_scan` | 4.35ms | 5.72ms | 1.316× | 1.87% | PASS |
| mem_reads | `types_table_scan` | 1.17s | 1.27s | 1.087× | 1.72% | PASS |
| mem_reads | `table_scan` | 1.43s | 1.38s | 0.968× | 4.77% | PASS |
| mem_reads | `oltp_read_only` | 153.45ms | 160.16ms | 1.044× | 1.04% | PASS |
| mem_writes | `oltp_bulk_insert` | 244.79ms | 342.84ms | 1.401× | 1.11% | PASS |
| mem_writes | `oltp_insert` | 19.67ms | 37.01ms | 1.881× | 0.78% | PASS |
| mem_writes | `oltp_update_index` | 73.57ms | 127.82ms | 1.737× | 1.27% | PASS |
| mem_writes | `oltp_update_non_index` | 55.40ms | 88.44ms | 1.596× | 0.95% | PASS |
| mem_writes | `oltp_delete_insert` | 53.47ms | 102.65ms | 1.920× | 1.14% | PASS |
| mem_writes | `oltp_write_only` | 29.17ms | 62.74ms | 2.151× | 1.61% | PASS |
| mem_writes | `types_delete_insert` | 33.20ms | 54.61ms | 1.645× | 0.95% | PASS |
| mem_writes | `oltp_read_write` | 102.05ms | 149.45ms | 1.465× | 1.05% | PASS |
| file_reads | `oltp_point_select` | 130.43ms | 70.49ms | 0.540× | 1.03% | PASS |
| file_reads | `oltp_range_select` | 30.73ms | 23.60ms | 0.768× | 1.31% | PASS |
| file_reads | `oltp_sum_range` | 29.50ms | 23.12ms | 0.784× | 1.18% | PASS |
| file_reads | `oltp_order_range` | 4.88ms | 4.18ms | 0.856× | 0.82% | PASS |
| file_reads | `oltp_distinct_range` | 5.94ms | 5.30ms | 0.893× | 0.89% | PASS |
| file_reads | `oltp_index_scan` | 14.85ms | 9.61ms | 0.647× | 0.96% | PASS |
| file_reads | `select_random_points` | 38.36ms | 34.65ms | 0.903× | 1.35% | PASS |
| file_reads | `select_random_ranges` | 17.38ms | 11.68ms | 0.672× | 0.96% | PASS |
| file_reads | `covering_index_scan` | 14.34ms | 7.66ms | 0.534× | 1.31% | PASS |
| file_reads | `groupby_scan` | 39.74ms | 40.65ms | 1.023× | 0.59% | PASS |
| file_reads | `index_join` | 13.51ms | 12.45ms | 0.922× | 1.11% | PASS |
| file_reads | `index_join_scan` | 5.39ms | 6.16ms | 1.144× | 1.47% | PASS |
| file_reads | `types_table_scan` | 1.12s | 1.24s | 1.112× | 0.52% | PASS |
| file_reads | `table_scan` | 1.29s | 1.35s | 1.046× | 0.44% | PASS |
| file_reads | `oltp_read_only` | 288.60ms | 206.05ms | 0.714× | 0.75% | PASS |
| file_writes | `oltp_bulk_insert` | 261.51ms | 363.38ms | 1.390× | 1.03% | PASS |
| file_writes | `oltp_insert` | 26.18ms | 46.96ms | 1.794× | 0.91% | PASS |
| file_writes | `oltp_update_index` | 99.14ms | 145.94ms | 1.472× | 0.71% | PASS |
| file_writes | `oltp_update_non_index` | 78.76ms | 106.50ms | 1.352× | 1.08% | PASS |
| file_writes | `oltp_delete_insert` | 78.56ms | 121.74ms | 1.550× | 0.77% | PASS |
| file_writes | `oltp_write_only` | 51.66ms | 81.13ms | 1.571× | 1.18% | PASS |
| file_writes | `types_delete_insert` | 50.47ms | 67.56ms | 1.339× | 1.16% | PASS |
| file_writes | `oltp_read_write` | 123.65ms | 168.97ms | 1.366× | 1.02% | PASS |
| ac_reads | `oltp_point_select` | 64.58ms | 69.69ms | 1.079× | 0.89% | PASS |
| ac_reads | `oltp_range_select` | 23.18ms | 23.39ms | 1.009× | 1.26% | PASS |
| ac_reads | `oltp_sum_range` | 21.74ms | 22.60ms | 1.039× | 0.92% | PASS |
| ac_reads | `oltp_order_range` | 4.18ms | 4.15ms | 0.993× | 1.12% | PASS |
| ac_reads | `oltp_distinct_range` | 5.17ms | 5.27ms | 1.020× | 0.79% | PASS |
| ac_reads | `oltp_index_scan` | 8.19ms | 9.44ms | 1.153× | 1.27% | PASS |
| ac_reads | `select_random_points` | 31.44ms | 34.44ms | 1.095× | 0.90% | PASS |
| ac_reads | `select_random_ranges` | 11.01ms | 11.88ms | 1.079× | 0.91% | PASS |
| ac_reads | `covering_index_scan` | 7.93ms | 7.81ms | 0.985× | 1.25% | PASS |
| ac_reads | `groupby_scan` | 38.97ms | 40.76ms | 1.046× | 0.68% | PASS |
| ac_reads | `index_join` | 10.30ms | 12.83ms | 1.245× | 1.32% | PASS |
| ac_reads | `index_join_scan` | 4.85ms | 6.29ms | 1.298× | 1.64% | PASS |
| ac_reads | `types_table_scan` | 1.19s | 1.28s | 1.071× | 2.26% | PASS |
| ac_reads | `table_scan` | 1.55s | 1.42s | 0.916× | 0.71% | PASS |
| ac_reads | `oltp_read_only` | 206.02ms | 211.07ms | 1.025× | 0.92% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.25ms | 62.77ms | 3.864× | 3.80% | PASS |
| ac_writes | `oltp_insert_ac` | 18.69ms | 83.88ms | 4.488× | 3.22% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.25ms | 95.80ms | 4.732× | 3.95% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.32ms | 73.68ms | 4.514× | 3.40% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.08ms | 86.06ms | 4.759× | 3.15% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.46ms | 84.94ms | 4.602× | 3.36% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.51ms | 74.57ms | 4.517× | 3.85% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.32ms | 93.34ms | 3.686× | 3.69% | PASS |

</details>

## Version-control latency

Wall time: 2m 17s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 82.12ms | 200.00ms | 41.1% | 0.60% | PASS |
| `status_dirty_many_tables` | 84.94ms | 200.00ms | 42.5% | 0.34% | PASS |
| `diff_regular_working_one_table` | 77.85ms | 150.00ms | 51.9% | 0.39% | PASS |
| `diff_regular_working_many_tables` | 90.62ms | 200.00ms | 45.3% | 0.34% | PASS |
| `diff_stat_working_many_tables` | 90.61ms | 200.00ms | 45.3% | 0.34% | PASS |
| `diff_schema_working_many_tables` | 91.16ms | 200.00ms | 45.6% | 0.47% | PASS |
| `branch_list_many_branches` | 22.48ms | 100.00ms | 22.5% | 1.29% | PASS |
| `branch_create_delete` | 24.53ms | 100.00ms | 24.5% | 1.59% | PASS |
| `checkout_branch_clean` | 54.72ms | 200.00ms | 27.4% | 0.73% | PASS |
| `merge_data_no_conflicts` | 28.61ms | 150.00ms | 19.1% | 0.93% | PASS |
| `merge_schema_no_conflicts` | 21.81ms | 100.00ms | 21.8% | 1.14% | PASS |
| `merge_data_conflicts` | 125.93ms | 250.00ms | 50.4% | 0.22% | PASS |
| `merge_data_conflicts_with_resolve` | 126.14ms | 250.00ms | 50.5% | 0.39% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
