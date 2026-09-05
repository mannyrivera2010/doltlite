# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-05 14:34 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260831.293.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/33967510153)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 9m 27s | 7.88s | 10.12s | 1.285× | 3.91% | **PASS** |
| textpk | 69 | 55 | 1h 33m 49s | 10.32s | 11.99s | 1.163× | 1.73% | **PASS** |
| blobpk | 69 | 55 | 1h 29m 37s | 9.35s | 11.73s | 1.255× | 1.47% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 48s | 9.88s | 12.16s | 1.231× | 1.49% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 18.75ms | 20.88ms | 1.113× | 4.80% | PASS |
| mem_reads | `oltp_range_select` | 8.61ms | 10.08ms | 1.171× | 4.05% | PASS |
| mem_reads | `oltp_sum_range` | 7.88ms | 9.39ms | 1.192× | 6.07% | PASS |
| mem_reads | `oltp_order_range` | 2.23ms | 2.47ms | 1.104× | 3.91% | PASS |
| mem_reads | `oltp_distinct_range` | 2.99ms | 3.24ms | 1.084× | 4.24% | PASS |
| mem_reads | `oltp_index_scan` | 3.15ms | 3.80ms | 1.204× | 4.10% | PASS |
| mem_reads | `select_random_points` | 8.53ms | 9.05ms | 1.060× | 3.66% | PASS |
| mem_reads | `select_random_ranges` | 2.30ms | 2.98ms | 1.297× | 3.72% | PASS |
| mem_reads | `covering_index_scan` | 3.03ms | 3.07ms | 1.013× | 2.70% | PASS |
| mem_reads | `groupby_scan` | 25.57ms | 27.20ms | 1.064× | 2.09% | PASS |
| mem_reads | `index_join` | 4.57ms | 6.22ms | 1.361× | 1.17% | PASS |
| mem_reads | `index_join_scan` | 2.52ms | 3.67ms | 1.455× | 2.34% | PASS |
| mem_reads | `types_table_scan` | 878.93ms | 1.05s | 1.200× | 1.08% | PASS |
| mem_reads | `table_scan` | 991.31ms | 1.15s | 1.161× | 1.05% | PASS |
| mem_reads | `oltp_read_only` | 82.73ms | 95.17ms | 1.150× | 2.51% | PASS |
| mem_writes | `oltp_bulk_insert` | 125.43ms | 167.45ms | 1.335× | 2.61% | PASS |
| mem_writes | `oltp_insert` | 11.50ms | 19.70ms | 1.713× | 4.92% | PASS |
| mem_writes | `oltp_update_index` | 39.03ms | 78.77ms | 2.018× | 2.64% | PASS |
| mem_writes | `oltp_update_non_index` | 25.54ms | 43.16ms | 1.690× | 3.34% | PASS |
| mem_writes | `oltp_delete_insert` | 34.51ms | 56.01ms | 1.623× | 3.20% | PASS |
| mem_writes | `oltp_write_only` | 16.24ms | 32.13ms | 1.979× | 3.15% | PASS |
| mem_writes | `types_delete_insert` | 18.21ms | 28.93ms | 1.588× | 3.06% | PASS |
| mem_writes | `oltp_read_write` | 49.27ms | 79.45ms | 1.612× | 2.67% | PASS |
| file_reads | `oltp_point_select` | 45.05ms | 29.32ms | 0.651× | 3.05% | PASS |
| file_reads | `oltp_range_select` | 11.54ms | 11.17ms | 0.968× | 3.55% | PASS |
| file_reads | `oltp_sum_range` | 10.60ms | 10.34ms | 0.975× | 3.98% | PASS |
| file_reads | `oltp_order_range` | 2.55ms | 2.59ms | 1.016× | 2.89% | PASS |
| file_reads | `oltp_distinct_range` | 3.36ms | 3.42ms | 1.020× | 4.29% | PASS |
| file_reads | `oltp_index_scan` | 5.97ms | 4.96ms | 0.831× | 5.60% | PASS |
| file_reads | `select_random_points` | 11.91ms | 10.73ms | 0.901× | 3.51% | PASS |
| file_reads | `select_random_ranges` | 5.18ms | 3.98ms | 0.769× | 5.86% | PASS |
| file_reads | `covering_index_scan` | 6.09ms | 4.08ms | 0.670× | 6.59% | PASS |
| file_reads | `groupby_scan` | 26.97ms | 27.70ms | 1.027× | 3.08% | PASS |
| file_reads | `index_join` | 6.39ms | 7.31ms | 1.144× | 4.11% | PASS |
| file_reads | `index_join_scan` | 3.04ms | 3.98ms | 1.312× | 4.94% | PASS |
| file_reads | `types_table_scan` | 883.09ms | 1.06s | 1.201× | 0.95% | PASS |
| file_reads | `table_scan` | 1.00s | 1.16s | 1.159× | 1.32% | PASS |
| file_reads | `oltp_read_only` | 119.80ms | 106.54ms | 0.889× | 1.75% | PASS |
| file_writes | `oltp_bulk_insert` | 176.69ms | 223.74ms | 1.266× | 7.39% | PASS |
| file_writes | `oltp_insert` | 24.71ms | 36.04ms | 1.458× | 23.78% | PASS |
| file_writes | `oltp_update_index` | 153.11ms | 163.36ms | 1.067× | 33.25% | PASS |
| file_writes | `oltp_update_non_index` | 109.42ms | 96.77ms | 0.884× | 15.48% | PASS |
| file_writes | `oltp_delete_insert` | 115.00ms | 123.58ms | 1.075× | 14.88% | PASS |
| file_writes | `oltp_write_only` | 84.43ms | 78.57ms | 0.931× | 3.80% | PASS |
| file_writes | `types_delete_insert` | 73.28ms | 62.52ms | 0.853× | 9.11% | PASS |
| file_writes | `oltp_read_write` | 129.48ms | 127.04ms | 0.981× | 11.34% | PASS |
| ac_reads | `oltp_point_select` | 26.84ms | 29.26ms | 1.090× | 2.06% | PASS |
| ac_reads | `oltp_range_select` | 9.43ms | 11.09ms | 1.177× | 3.14% | PASS |
| ac_reads | `oltp_sum_range` | 9.18ms | 10.47ms | 1.141× | 3.94% | PASS |
| ac_reads | `oltp_order_range` | 2.38ms | 2.59ms | 1.091× | 4.47% | PASS |
| ac_reads | `oltp_distinct_range` | 3.14ms | 3.42ms | 1.091× | 5.18% | PASS |
| ac_reads | `oltp_index_scan` | 4.22ms | 5.01ms | 1.186× | 3.39% | PASS |
| ac_reads | `select_random_points` | 10.60ms | 10.75ms | 1.015× | 4.22% | PASS |
| ac_reads | `select_random_ranges` | 3.40ms | 4.00ms | 1.174× | 5.68% | PASS |
| ac_reads | `covering_index_scan` | 4.15ms | 4.15ms | 1.001× | 3.44% | PASS |
| ac_reads | `groupby_scan` | 26.36ms | 27.82ms | 1.055× | 2.76% | PASS |
| ac_reads | `index_join` | 5.48ms | 7.30ms | 1.332× | 4.20% | PASS |
| ac_reads | `index_join_scan` | 2.82ms | 3.93ms | 1.395× | 2.91% | PASS |
| ac_reads | `types_table_scan` | 871.05ms | 1.05s | 1.207× | 0.88% | PASS |
| ac_reads | `table_scan` | 1.00s | 1.16s | 1.155× | 0.96% | PASS |
| ac_reads | `oltp_read_only` | 93.67ms | 107.23ms | 1.145× | 1.42% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 43.26ms | 142.31ms | 3.290× | 22.99% | PASS |
| ac_writes | `oltp_insert_ac` | 39.09ms | 125.04ms | 3.199× | 25.55% | PASS |
| ac_writes | `oltp_update_index_ac` | 63.95ms | 182.45ms | 2.853× | 48.83% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 45.77ms | 157.48ms | 3.440× | 32.32% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 47.95ms | 183.88ms | 3.835× | 35.39% | PASS |
| ac_writes | `oltp_write_only_ac` | 57.44ms | 220.36ms | 3.837× | 30.30% | PASS |
| ac_writes | `types_delete_insert_ac` | 51.41ms | 235.40ms | 4.579× | 44.41% | PASS |
| ac_writes | `oltp_read_write_ac` | 81.45ms | 169.70ms | 2.084× | 48.55% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.91ms | 37.55ms | 1.255× | 1.62% | PASS |
| mem_reads | `oltp_range_select` | 13.35ms | 14.17ms | 1.062× | 1.36% | PASS |
| mem_reads | `oltp_sum_range` | 12.25ms | 14.29ms | 1.167× | 1.41% | PASS |
| mem_reads | `oltp_order_range` | 3.01ms | 3.17ms | 1.054× | 1.70% | PASS |
| mem_reads | `oltp_distinct_range` | 4.08ms | 4.25ms | 1.042× | 1.05% | PASS |
| mem_reads | `oltp_index_scan` | 4.65ms | 6.50ms | 1.396× | 1.68% | PASS |
| mem_reads | `select_random_points` | 18.20ms | 21.32ms | 1.172× | 2.00% | PASS |
| mem_reads | `select_random_ranges` | 4.04ms | 5.24ms | 1.297× | 1.70% | PASS |
| mem_reads | `covering_index_scan` | 4.78ms | 4.72ms | 0.988× | 2.94% | PASS |
| mem_reads | `groupby_scan` | 31.73ms | 33.79ms | 1.065× | 0.87% | PASS |
| mem_reads | `index_join` | 6.94ms | 9.28ms | 1.338× | 2.23% | PASS |
| mem_reads | `index_join_scan` | 4.58ms | 5.41ms | 1.180× | 1.57% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.23s | 1.151× | 1.56% | PASS |
| mem_reads | `table_scan` | 1.27s | 1.38s | 1.087× | 2.63% | PASS |
| mem_reads | `oltp_read_only` | 117.83ms | 137.64ms | 1.168× | 1.78% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.34ms | 358.25ms | 1.522× | 0.66% | PASS |
| mem_writes | `oltp_insert` | 21.91ms | 40.63ms | 1.854× | 1.20% | PASS |
| mem_writes | `oltp_update_index` | 73.51ms | 137.93ms | 1.876× | 0.93% | PASS |
| mem_writes | `oltp_update_non_index` | 49.47ms | 88.36ms | 1.786× | 1.01% | PASS |
| mem_writes | `oltp_delete_insert` | 53.68ms | 109.41ms | 2.038× | 1.90% | PASS |
| mem_writes | `oltp_write_only` | 29.82ms | 63.85ms | 2.141× | 1.10% | PASS |
| mem_writes | `types_delete_insert` | 33.51ms | 56.97ms | 1.700× | 0.93% | PASS |
| mem_writes | `oltp_read_write` | 85.16ms | 141.30ms | 1.659× | 1.32% | PASS |
| file_reads | `oltp_point_select` | 105.40ms | 63.92ms | 0.606× | 0.64% | PASS |
| file_reads | `oltp_range_select` | 21.98ms | 17.15ms | 0.780× | 1.73% | PASS |
| file_reads | `oltp_sum_range` | 20.55ms | 17.08ms | 0.831× | 2.07% | PASS |
| file_reads | `oltp_order_range` | 3.99ms | 3.60ms | 0.902× | 2.26% | PASS |
| file_reads | `oltp_distinct_range` | 5.02ms | 4.66ms | 0.928× | 1.73% | PASS |
| file_reads | `oltp_index_scan` | 12.34ms | 9.28ms | 0.752× | 1.80% | PASS |
| file_reads | `select_random_points` | 27.35ms | 25.16ms | 0.920× | 1.51% | PASS |
| file_reads | `select_random_ranges` | 12.12ms | 7.98ms | 0.659× | 0.86% | PASS |
| file_reads | `covering_index_scan` | 13.87ms | 7.52ms | 0.542× | 1.61% | PASS |
| file_reads | `groupby_scan` | 33.70ms | 34.58ms | 1.026× | 0.91% | PASS |
| file_reads | `index_join` | 11.93ms | 11.41ms | 0.956× | 3.03% | PASS |
| file_reads | `index_join_scan` | 5.67ms | 5.92ms | 1.042× | 2.47% | PASS |
| file_reads | `types_table_scan` | 1.09s | 1.23s | 1.128× | 2.87% | PASS |
| file_reads | `table_scan` | 1.55s | 1.41s | 0.911× | 1.70% | PASS |
| file_reads | `oltp_read_only` | 245.46ms | 183.35ms | 0.747× | 1.26% | PASS |
| file_writes | `oltp_bulk_insert` | 255.85ms | 396.82ms | 1.551× | 1.04% | PASS |
| file_writes | `oltp_insert` | 51.35ms | 54.18ms | 1.055× | 21.64% | PASS |
| file_writes | `oltp_update_index` | 116.99ms | 172.64ms | 1.476× | 1.96% | PASS |
| file_writes | `oltp_update_non_index` | 103.33ms | 116.28ms | 1.125× | 14.37% | PASS |
| file_writes | `oltp_delete_insert` | 92.69ms | 137.49ms | 1.483× | 1.93% | PASS |
| file_writes | `oltp_write_only` | 93.26ms | 86.74ms | 0.930× | 10.05% | PASS |
| file_writes | `types_delete_insert` | 56.31ms | 76.55ms | 1.359× | 1.96% | PASS |
| file_writes | `oltp_read_write` | 140.64ms | 166.37ms | 1.183× | 5.63% | PASS |
| ac_reads | `oltp_point_select` | 54.97ms | 64.09ms | 1.166× | 0.69% | PASS |
| ac_reads | `oltp_range_select` | 16.80ms | 17.16ms | 1.021× | 1.97% | PASS |
| ac_reads | `oltp_sum_range` | 15.57ms | 17.32ms | 1.112× | 1.47% | PASS |
| ac_reads | `oltp_order_range` | 3.56ms | 3.61ms | 1.014× | 1.25% | PASS |
| ac_reads | `oltp_distinct_range` | 4.61ms | 4.69ms | 1.019× | 1.67% | PASS |
| ac_reads | `oltp_index_scan` | 7.71ms | 9.40ms | 1.220× | 1.87% | PASS |
| ac_reads | `select_random_points` | 22.38ms | 25.66ms | 1.147× | 1.87% | PASS |
| ac_reads | `select_random_ranges` | 6.94ms | 7.96ms | 1.147× | 1.04% | PASS |
| ac_reads | `covering_index_scan` | 8.61ms | 7.50ms | 0.871× | 1.57% | PASS |
| ac_reads | `groupby_scan` | 32.39ms | 34.45ms | 1.064× | 1.02% | PASS |
| ac_reads | `index_join` | 9.34ms | 11.42ms | 1.223× | 2.33% | PASS |
| ac_reads | `index_join_scan` | 5.13ms | 5.96ms | 1.163× | 1.76% | PASS |
| ac_reads | `types_table_scan` | 1.09s | 1.23s | 1.132× | 1.78% | PASS |
| ac_reads | `table_scan` | 1.42s | 1.39s | 0.978× | 4.07% | PASS |
| ac_reads | `oltp_read_only` | 158.17ms | 178.13ms | 1.126× | 1.01% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.34ms | 86.78ms | 3.719× | 7.30% | PASS |
| ac_writes | `oltp_insert_ac` | 27.78ms | 99.32ms | 3.575× | 7.33% | PASS |
| ac_writes | `oltp_update_index_ac` | 30.26ms | 121.05ms | 4.000× | 6.27% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.28ms | 98.11ms | 4.041× | 6.71% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.49ms | 108.91ms | 4.112× | 5.82% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.59ms | 108.84ms | 3.946× | 7.20% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.39ms | 99.09ms | 4.063× | 8.77% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.52ms | 118.84ms | 3.545× | 6.99% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.55ms | 36.41ms | 1.232× | 1.41% | PASS |
| mem_reads | `oltp_range_select` | 12.86ms | 13.89ms | 1.080× | 1.91% | PASS |
| mem_reads | `oltp_sum_range` | 11.72ms | 13.89ms | 1.185× | 1.45% | PASS |
| mem_reads | `oltp_order_range` | 2.85ms | 3.11ms | 1.091× | 1.11% | PASS |
| mem_reads | `oltp_distinct_range` | 3.93ms | 4.18ms | 1.063× | 0.90% | PASS |
| mem_reads | `oltp_index_scan` | 4.43ms | 6.08ms | 1.373× | 1.51% | PASS |
| mem_reads | `select_random_points` | 17.61ms | 20.45ms | 1.162× | 2.19% | PASS |
| mem_reads | `select_random_ranges` | 3.87ms | 5.12ms | 1.323× | 2.03% | PASS |
| mem_reads | `covering_index_scan` | 4.35ms | 4.38ms | 1.006× | 1.67% | PASS |
| mem_reads | `groupby_scan` | 31.47ms | 33.64ms | 1.069× | 0.89% | PASS |
| mem_reads | `index_join` | 6.81ms | 8.99ms | 1.321× | 1.74% | PASS |
| mem_reads | `index_join_scan` | 4.32ms | 5.35ms | 1.241× | 2.56% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.22s | 1.168× | 0.73% | PASS |
| mem_reads | `table_scan` | 1.20s | 1.37s | 1.139× | 0.49% | PASS |
| mem_reads | `oltp_read_only` | 116.60ms | 134.19ms | 1.151× | 1.12% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.37ms | 348.74ms | 1.475× | 0.85% | PASS |
| mem_writes | `oltp_insert` | 19.70ms | 39.42ms | 2.001× | 0.76% | PASS |
| mem_writes | `oltp_update_index` | 67.72ms | 128.61ms | 1.899× | 0.99% | PASS |
| mem_writes | `oltp_update_non_index` | 48.08ms | 83.45ms | 1.736× | 1.51% | PASS |
| mem_writes | `oltp_delete_insert` | 48.56ms | 102.11ms | 2.103× | 1.02% | PASS |
| mem_writes | `oltp_write_only` | 27.70ms | 61.36ms | 2.216× | 0.92% | PASS |
| mem_writes | `types_delete_insert` | 32.44ms | 53.91ms | 1.662× | 1.13% | PASS |
| mem_writes | `oltp_read_write` | 83.51ms | 137.48ms | 1.646× | 1.55% | PASS |
| file_reads | `oltp_point_select` | 105.29ms | 62.95ms | 0.598× | 0.94% | PASS |
| file_reads | `oltp_range_select` | 21.65ms | 16.67ms | 0.770× | 1.71% | PASS |
| file_reads | `oltp_sum_range` | 20.61ms | 16.75ms | 0.813× | 1.79% | PASS |
| file_reads | `oltp_order_range` | 3.84ms | 3.45ms | 0.899× | 1.46% | PASS |
| file_reads | `oltp_distinct_range` | 4.95ms | 4.56ms | 0.921× | 1.47% | PASS |
| file_reads | `oltp_index_scan` | 12.55ms | 9.07ms | 0.723× | 1.38% | PASS |
| file_reads | `select_random_points` | 27.63ms | 24.13ms | 0.873× | 2.11% | PASS |
| file_reads | `select_random_ranges` | 11.71ms | 7.82ms | 0.667× | 1.17% | PASS |
| file_reads | `covering_index_scan` | 12.75ms | 7.24ms | 0.568× | 1.84% | PASS |
| file_reads | `groupby_scan` | 33.04ms | 34.13ms | 1.033× | 0.90% | PASS |
| file_reads | `index_join` | 11.88ms | 11.14ms | 0.938× | 1.88% | PASS |
| file_reads | `index_join_scan` | 5.47ms | 5.86ms | 1.070× | 2.12% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.22s | 1.170× | 0.59% | PASS |
| file_reads | `table_scan` | 1.20s | 1.36s | 1.139× | 0.67% | PASS |
| file_reads | `oltp_read_only` | 225.84ms | 172.33ms | 0.763× | 1.10% | PASS |
| file_writes | `oltp_bulk_insert` | 255.32ms | 373.46ms | 1.463× | 0.85% | PASS |
| file_writes | `oltp_insert` | 31.79ms | 51.65ms | 1.624× | 2.38% | PASS |
| file_writes | `oltp_update_index` | 102.43ms | 161.31ms | 1.575× | 1.23% | PASS |
| file_writes | `oltp_update_non_index` | 78.34ms | 107.56ms | 1.373× | 1.55% | PASS |
| file_writes | `oltp_delete_insert` | 81.73ms | 129.46ms | 1.584× | 1.39% | PASS |
| file_writes | `oltp_write_only` | 55.78ms | 83.65ms | 1.500× | 2.21% | PASS |
| file_writes | `types_delete_insert` | 52.50ms | 71.68ms | 1.365× | 1.59% | PASS |
| file_writes | `oltp_read_write` | 113.84ms | 160.25ms | 1.408× | 1.36% | PASS |
| ac_reads | `oltp_point_select` | 54.53ms | 62.55ms | 1.147× | 0.92% | PASS |
| ac_reads | `oltp_range_select` | 15.68ms | 16.65ms | 1.062× | 2.22% | PASS |
| ac_reads | `oltp_sum_range` | 14.78ms | 16.67ms | 1.128× | 1.77% | PASS |
| ac_reads | `oltp_order_range` | 3.26ms | 3.47ms | 1.064× | 2.25% | PASS |
| ac_reads | `oltp_distinct_range` | 4.34ms | 4.54ms | 1.047× | 1.12% | PASS |
| ac_reads | `oltp_index_scan` | 7.17ms | 9.00ms | 1.255× | 1.53% | PASS |
| ac_reads | `select_random_points` | 21.03ms | 23.88ms | 1.135× | 1.57% | PASS |
| ac_reads | `select_random_ranges` | 6.61ms | 7.79ms | 1.178× | 1.19% | PASS |
| ac_reads | `covering_index_scan` | 7.15ms | 7.24ms | 1.012× | 1.13% | PASS |
| ac_reads | `groupby_scan` | 31.98ms | 34.04ms | 1.065× | 0.90% | PASS |
| ac_reads | `index_join` | 8.52ms | 11.19ms | 1.314× | 1.62% | PASS |
| ac_reads | `index_join_scan` | 4.81ms | 5.83ms | 1.214× | 1.74% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.22s | 1.175× | 0.70% | PASS |
| ac_reads | `table_scan` | 1.19s | 1.36s | 1.142× | 0.56% | PASS |
| ac_reads | `oltp_read_only` | 153.40ms | 172.14ms | 1.122× | 0.90% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.37ms | 83.36ms | 3.566× | 5.49% | PASS |
| ac_writes | `oltp_insert_ac` | 25.57ms | 107.55ms | 4.207× | 5.07% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.58ms | 120.80ms | 4.227× | 6.34% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.59ms | 97.35ms | 4.127× | 6.77% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.89ms | 106.33ms | 4.271× | 4.14% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.06ms | 106.01ms | 4.068× | 6.45% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.17ms | 97.23ms | 4.197× | 7.73% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.10ms | 113.60ms | 3.539× | 4.97% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.90ms | 40.43ms | 1.229× | 1.53% | PASS |
| mem_reads | `oltp_range_select` | 18.61ms | 21.15ms | 1.137× | 1.60% | PASS |
| mem_reads | `oltp_sum_range` | 17.81ms | 20.85ms | 1.171× | 1.42% | PASS |
| mem_reads | `oltp_order_range` | 3.46ms | 3.84ms | 1.111× | 1.36% | PASS |
| mem_reads | `oltp_distinct_range` | 4.62ms | 4.92ms | 1.066× | 1.55% | PASS |
| mem_reads | `oltp_index_scan` | 4.64ms | 6.25ms | 1.348× | 1.92% | PASS |
| mem_reads | `select_random_points` | 27.96ms | 32.25ms | 1.153× | 1.93% | PASS |
| mem_reads | `select_random_ranges` | 7.67ms | 9.10ms | 1.185× | 2.25% | PASS |
| mem_reads | `covering_index_scan` | 4.26ms | 4.21ms | 0.989× | 1.44% | PASS |
| mem_reads | `groupby_scan` | 36.50ms | 38.57ms | 1.057× | 1.09% | PASS |
| mem_reads | `index_join` | 8.26ms | 10.76ms | 1.303× | 1.46% | PASS |
| mem_reads | `index_join_scan` | 4.23ms | 5.52ms | 1.305× | 2.43% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.25s | 1.166× | 1.88% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.39s | 1.174× | 0.80% | PASS |
| mem_reads | `oltp_read_only` | 149.19ms | 169.90ms | 1.139× | 0.68% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.86ms | 356.94ms | 1.429× | 0.83% | PASS |
| mem_writes | `oltp_insert` | 19.32ms | 36.72ms | 1.900× | 0.54% | PASS |
| mem_writes | `oltp_update_index` | 67.42ms | 116.40ms | 1.726× | 0.77% | PASS |
| mem_writes | `oltp_update_non_index` | 50.36ms | 83.05ms | 1.649× | 0.65% | PASS |
| mem_writes | `oltp_delete_insert` | 49.58ms | 95.98ms | 1.936× | 0.69% | PASS |
| mem_writes | `oltp_write_only` | 26.93ms | 57.78ms | 2.146× | 0.93% | PASS |
| mem_writes | `types_delete_insert` | 32.47ms | 54.89ms | 1.691× | 1.03% | PASS |
| mem_writes | `oltp_read_write` | 101.25ms | 155.84ms | 1.539× | 0.77% | PASS |
| file_reads | `oltp_point_select` | 110.26ms | 67.18ms | 0.609× | 1.16% | PASS |
| file_reads | `oltp_range_select` | 28.28ms | 24.64ms | 0.871× | 2.05% | PASS |
| file_reads | `oltp_sum_range` | 27.22ms | 24.52ms | 0.901× | 1.48% | PASS |
| file_reads | `oltp_order_range` | 4.57ms | 4.32ms | 0.943× | 1.84% | PASS |
| file_reads | `oltp_distinct_range` | 5.78ms | 5.42ms | 0.938× | 1.45% | PASS |
| file_reads | `oltp_index_scan` | 12.87ms | 9.35ms | 0.726× | 1.79% | PASS |
| file_reads | `select_random_points` | 40.80ms | 38.38ms | 0.941× | 1.50% | PASS |
| file_reads | `select_random_ranges` | 16.32ms | 12.36ms | 0.757× | 1.10% | PASS |
| file_reads | `covering_index_scan` | 12.34ms | 7.31ms | 0.593× | 1.49% | PASS |
| file_reads | `groupby_scan` | 37.90ms | 39.76ms | 1.049× | 0.93% | PASS |
| file_reads | `index_join` | 12.98ms | 13.23ms | 1.019× | 2.24% | PASS |
| file_reads | `index_join_scan` | 5.34ms | 6.09ms | 1.141× | 1.99% | PASS |
| file_reads | `types_table_scan` | 1.12s | 1.26s | 1.119× | 1.19% | PASS |
| file_reads | `table_scan` | 1.37s | 1.43s | 1.041× | 1.24% | PASS |
| file_reads | `oltp_read_only` | 273.34ms | 213.41ms | 0.781× | 1.22% | PASS |
| file_writes | `oltp_bulk_insert` | 264.42ms | 381.23ms | 1.442× | 0.67% | PASS |
| file_writes | `oltp_insert` | 26.84ms | 46.86ms | 1.746× | 2.22% | PASS |
| file_writes | `oltp_update_index` | 98.85ms | 145.05ms | 1.467× | 1.42% | PASS |
| file_writes | `oltp_update_non_index` | 77.60ms | 105.56ms | 1.360× | 1.75% | PASS |
| file_writes | `oltp_delete_insert` | 77.47ms | 119.78ms | 1.546× | 1.95% | PASS |
| file_writes | `oltp_write_only` | 51.59ms | 77.68ms | 1.506× | 1.77% | PASS |
| file_writes | `types_delete_insert` | 50.81ms | 69.93ms | 1.376× | 2.16% | PASS |
| file_writes | `oltp_read_write` | 129.61ms | 176.29ms | 1.360× | 1.45% | PASS |
| ac_reads | `oltp_point_select` | 59.34ms | 67.30ms | 1.134× | 1.41% | PASS |
| ac_reads | `oltp_range_select` | 22.23ms | 24.47ms | 1.101× | 1.81% | PASS |
| ac_reads | `oltp_sum_range` | 20.70ms | 24.10ms | 1.165× | 1.42% | PASS |
| ac_reads | `oltp_order_range` | 3.92ms | 4.26ms | 1.087× | 1.94% | PASS |
| ac_reads | `oltp_distinct_range` | 5.06ms | 5.35ms | 1.058× | 1.62% | PASS |
| ac_reads | `oltp_index_scan` | 7.26ms | 9.06ms | 1.247× | 1.89% | PASS |
| ac_reads | `select_random_points` | 31.15ms | 36.22ms | 1.163× | 1.08% | PASS |
| ac_reads | `select_random_ranges` | 10.46ms | 12.09ms | 1.157× | 1.51% | PASS |
| ac_reads | `covering_index_scan` | 6.98ms | 7.10ms | 1.017× | 1.85% | PASS |
| ac_reads | `groupby_scan` | 36.55ms | 39.35ms | 1.077× | 0.87% | PASS |
| ac_reads | `index_join` | 9.81ms | 12.60ms | 1.284× | 1.21% | PASS |
| ac_reads | `index_join_scan` | 4.68ms | 6.04ms | 1.291× | 1.35% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.24s | 1.188× | 1.49% | PASS |
| ac_reads | `table_scan` | 1.19s | 1.39s | 1.169× | 1.55% | PASS |
| ac_reads | `oltp_read_only` | 187.81ms | 211.57ms | 1.126× | 1.13% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.32ms | 85.03ms | 3.496× | 9.60% | PASS |
| ac_writes | `oltp_insert_ac` | 26.87ms | 109.02ms | 4.058× | 8.62% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.59ms | 117.16ms | 4.098× | 7.70% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.86ms | 97.93ms | 4.104× | 6.02% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.18ms | 109.72ms | 4.357× | 4.71% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.27ms | 106.90ms | 4.069× | 6.13% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.56ms | 96.74ms | 4.107× | 10.33% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.76ms | 112.84ms | 3.444× | 6.23% | PASS |

</details>

## Version-control latency

Wall time: 2m 34s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 95.24ms | 200.00ms | 47.6% | 0.33% | PASS |
| `status_dirty_many_tables` | 98.41ms | 200.00ms | 49.2% | 0.34% | PASS |
| `diff_regular_working_one_table` | 89.34ms | 150.00ms | 59.6% | 0.58% | PASS |
| `diff_regular_working_many_tables` | 102.41ms | 200.00ms | 51.2% | 0.66% | PASS |
| `diff_stat_working_many_tables` | 103.70ms | 200.00ms | 51.9% | 1.00% | PASS |
| `diff_schema_working_many_tables` | 104.91ms | 200.00ms | 52.5% | 0.56% | PASS |
| `branch_list_many_branches` | 26.41ms | 100.00ms | 26.4% | 2.56% | PASS |
| `branch_create_delete` | 28.96ms | 100.00ms | 29.0% | 2.10% | PASS |
| `checkout_branch_clean` | 61.64ms | 200.00ms | 30.8% | 0.96% | PASS |
| `merge_data_no_conflicts` | 33.08ms | 150.00ms | 22.1% | 0.99% | PASS |
| `merge_schema_no_conflicts` | 25.02ms | 100.00ms | 25.0% | 1.37% | PASS |
| `merge_data_conflicts` | 131.42ms | 250.00ms | 52.6% | 0.25% | PASS |
| `merge_data_conflicts_with_resolve` | 131.19ms | 250.00ms | 52.5% | 0.24% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
