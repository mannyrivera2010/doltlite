# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-04 16:30 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260927.320.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/37211091421)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 11m 48s | 8.71s | 11.29s | 1.297× | 1.28% | **PASS** |
| textpk | 69 | 55 | 1h 32m 15s | 9.65s | 11.72s | 1.214× | 1.87% | **PASS** |
| blobpk | 69 | 55 | 1h 6m 36s | 7.05s | 7.74s | 1.099× | 2.85% | **PASS** |
| compositepk | 69 | 55 | 1h 26m 5s | 10.22s | 11.70s | 1.146× | 1.00% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.84ms | 29.11ms | 1.275× | 1.12% | PASS |
| mem_reads | `oltp_range_select` | 9.25ms | 13.03ms | 1.409× | 1.75% | PASS |
| mem_reads | `oltp_sum_range` | 8.94ms | 12.25ms | 1.371× | 1.32% | PASS |
| mem_reads | `oltp_order_range` | 2.41ms | 2.96ms | 1.227× | 1.39% | PASS |
| mem_reads | `oltp_distinct_range` | 3.48ms | 4.01ms | 1.153× | 0.73% | PASS |
| mem_reads | `oltp_index_scan` | 3.70ms | 5.20ms | 1.405× | 1.22% | PASS |
| mem_reads | `select_random_points` | 8.95ms | 11.01ms | 1.230× | 2.24% | PASS |
| mem_reads | `select_random_ranges` | 2.74ms | 3.94ms | 1.436× | 1.91% | PASS |
| mem_reads | `covering_index_scan` | 4.25ms | 4.11ms | 0.968× | 0.89% | PASS |
| mem_reads | `groupby_scan` | 29.35ms | 33.20ms | 1.131× | 0.57% | PASS |
| mem_reads | `index_join` | 5.96ms | 8.01ms | 1.344× | 1.19% | PASS |
| mem_reads | `index_join_scan` | 3.24ms | 4.61ms | 1.421× | 3.22% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.33s | 1.286× | 0.46% | PASS |
| mem_reads | `table_scan` | 1.16s | 1.38s | 1.197× | 0.42% | PASS |
| mem_reads | `oltp_read_only` | 98.92ms | 122.76ms | 1.241× | 0.55% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.77ms | 251.26ms | 1.382× | 1.29% | PASS |
| mem_writes | `oltp_insert` | 15.48ms | 28.03ms | 1.811× | 0.84% | PASS |
| mem_writes | `oltp_update_index` | 48.67ms | 102.19ms | 2.100× | 0.88% | PASS |
| mem_writes | `oltp_update_non_index` | 32.77ms | 58.61ms | 1.789× | 1.14% | PASS |
| mem_writes | `oltp_delete_insert` | 43.32ms | 77.16ms | 1.781× | 0.94% | PASS |
| mem_writes | `oltp_write_only` | 20.91ms | 44.14ms | 2.111× | 0.87% | PASS |
| mem_writes | `types_delete_insert` | 23.99ms | 40.01ms | 1.668× | 1.60% | PASS |
| mem_writes | `oltp_read_write` | 64.11ms | 108.88ms | 1.698× | 0.91% | PASS |
| file_reads | `oltp_point_select` | 96.80ms | 54.78ms | 0.566× | 0.55% | PASS |
| file_reads | `oltp_range_select` | 17.05ms | 15.92ms | 0.933× | 2.32% | PASS |
| file_reads | `oltp_sum_range` | 16.81ms | 15.14ms | 0.900× | 1.88% | PASS |
| file_reads | `oltp_order_range` | 3.34ms | 3.35ms | 1.004× | 2.42% | PASS |
| file_reads | `oltp_distinct_range` | 4.40ms | 4.38ms | 0.997× | 2.42% | PASS |
| file_reads | `oltp_index_scan` | 11.37ms | 8.09ms | 0.712× | 2.47% | PASS |
| file_reads | `select_random_points` | 16.89ms | 14.03ms | 0.830× | 1.93% | PASS |
| file_reads | `select_random_ranges` | 10.29ms | 6.58ms | 0.639× | 2.28% | PASS |
| file_reads | `covering_index_scan` | 11.88ms | 6.86ms | 0.577× | 1.92% | PASS |
| file_reads | `groupby_scan` | 30.61ms | 33.83ms | 1.105× | 1.15% | PASS |
| file_reads | `index_join` | 10.23ms | 10.01ms | 0.979× | 1.04% | PASS |
| file_reads | `index_join_scan` | 4.37ms | 5.06ms | 1.159× | 1.95% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.32s | 1.279× | 0.40% | PASS |
| file_reads | `table_scan` | 1.16s | 1.39s | 1.198× | 0.31% | PASS |
| file_reads | `oltp_read_only` | 208.73ms | 160.60ms | 0.769× | 0.65% | PASS |
| file_writes | `oltp_bulk_insert` | 194.33ms | 271.02ms | 1.395× | 1.02% | PASS |
| file_writes | `oltp_insert` | 22.05ms | 35.33ms | 1.602× | 1.28% | PASS |
| file_writes | `oltp_update_index` | 76.23ms | 126.60ms | 1.661× | 1.43% | PASS |
| file_writes | `oltp_update_non_index` | 57.23ms | 80.59ms | 1.408× | 1.95% | PASS |
| file_writes | `oltp_delete_insert` | 67.06ms | 97.84ms | 1.459× | 0.99% | PASS |
| file_writes | `oltp_write_only` | 43.87ms | 63.38ms | 1.445× | 1.86% | PASS |
| file_writes | `types_delete_insert` | 39.87ms | 53.08ms | 1.331× | 1.19% | PASS |
| file_writes | `oltp_read_write` | 89.03ms | 128.50ms | 1.443× | 1.70% | PASS |
| ac_reads | `oltp_point_select` | 47.59ms | 54.91ms | 1.154× | 0.93% | PASS |
| ac_reads | `oltp_range_select` | 12.41ms | 15.87ms | 1.278× | 1.25% | PASS |
| ac_reads | `oltp_sum_range` | 12.01ms | 15.05ms | 1.253× | 1.23% | PASS |
| ac_reads | `oltp_order_range` | 2.86ms | 3.33ms | 1.164× | 2.32% | PASS |
| ac_reads | `oltp_distinct_range` | 3.85ms | 4.37ms | 1.135× | 1.47% | PASS |
| ac_reads | `oltp_index_scan` | 6.39ms | 7.95ms | 1.244× | 1.29% | PASS |
| ac_reads | `select_random_points` | 12.32ms | 13.94ms | 1.132× | 1.56% | PASS |
| ac_reads | `select_random_ranges` | 5.39ms | 6.56ms | 1.216× | 0.85% | PASS |
| ac_reads | `covering_index_scan` | 6.78ms | 6.78ms | 0.999× | 1.16% | PASS |
| ac_reads | `groupby_scan` | 29.78ms | 33.67ms | 1.131× | 0.77% | PASS |
| ac_reads | `index_join` | 7.43ms | 9.92ms | 1.335× | 1.13% | PASS |
| ac_reads | `index_join_scan` | 3.83ms | 5.06ms | 1.320× | 2.23% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.32s | 1.280× | 0.49% | PASS |
| ac_reads | `table_scan` | 1.15s | 1.39s | 1.201× | 0.33% | PASS |
| ac_reads | `oltp_read_only` | 136.21ms | 160.91ms | 1.181× | 0.51% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.57ms | 66.64ms | 3.090× | 3.09% | PASS |
| ac_writes | `oltp_insert_ac` | 23.84ms | 83.31ms | 3.494× | 3.36% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.99ms | 98.39ms | 3.786× | 3.17% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.71ms | 76.39ms | 3.363× | 5.36% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.65ms | 88.16ms | 3.727× | 3.23% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.82ms | 88.03ms | 3.696× | 2.57% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.37ms | 77.55ms | 3.628× | 7.15% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.62ms | 94.60ms | 3.305× | 3.27% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.22ms | 37.62ms | 1.245× | 0.69% | PASS |
| mem_reads | `oltp_range_select` | 13.67ms | 14.28ms | 1.044× | 2.83% | PASS |
| mem_reads | `oltp_sum_range` | 12.52ms | 14.26ms | 1.138× | 1.92% | PASS |
| mem_reads | `oltp_order_range` | 2.96ms | 3.17ms | 1.070× | 1.21% | PASS |
| mem_reads | `oltp_distinct_range` | 4.05ms | 4.25ms | 1.050× | 1.25% | PASS |
| mem_reads | `oltp_index_scan` | 4.49ms | 6.31ms | 1.405× | 1.71% | PASS |
| mem_reads | `select_random_points` | 18.28ms | 20.89ms | 1.143× | 2.66% | PASS |
| mem_reads | `select_random_ranges` | 3.98ms | 5.23ms | 1.315× | 1.75% | PASS |
| mem_reads | `covering_index_scan` | 4.55ms | 4.46ms | 0.979× | 1.87% | PASS |
| mem_reads | `groupby_scan` | 31.82ms | 34.41ms | 1.081× | 1.09% | PASS |
| mem_reads | `index_join` | 6.93ms | 9.13ms | 1.318× | 1.66% | PASS |
| mem_reads | `index_join_scan` | 4.93ms | 5.50ms | 1.115× | 4.32% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.23s | 1.161× | 0.76% | PASS |
| mem_reads | `table_scan` | 1.22s | 1.37s | 1.121× | 1.15% | PASS |
| mem_reads | `oltp_read_only` | 121.82ms | 137.18ms | 1.126× | 2.18% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.94ms | 357.48ms | 1.515× | 0.80% | PASS |
| mem_writes | `oltp_insert` | 22.65ms | 40.09ms | 1.770× | 1.41% | PASS |
| mem_writes | `oltp_update_index` | 74.60ms | 138.19ms | 1.852× | 2.60% | PASS |
| mem_writes | `oltp_update_non_index` | 49.26ms | 87.56ms | 1.778× | 1.20% | PASS |
| mem_writes | `oltp_delete_insert` | 51.56ms | 103.64ms | 2.010× | 1.30% | PASS |
| mem_writes | `oltp_write_only` | 29.12ms | 61.85ms | 2.124× | 0.86% | PASS |
| mem_writes | `types_delete_insert` | 33.35ms | 55.39ms | 1.661× | 1.62% | PASS |
| mem_writes | `oltp_read_write` | 87.82ms | 139.96ms | 1.594× | 1.90% | PASS |
| file_reads | `oltp_point_select` | 106.05ms | 64.06ms | 0.604× | 1.29% | PASS |
| file_reads | `oltp_range_select` | 22.66ms | 17.29ms | 0.763× | 2.57% | PASS |
| file_reads | `oltp_sum_range` | 20.60ms | 17.29ms | 0.839× | 2.06% | PASS |
| file_reads | `oltp_order_range` | 3.98ms | 3.62ms | 0.910× | 2.29% | PASS |
| file_reads | `oltp_distinct_range` | 5.07ms | 4.68ms | 0.923× | 2.19% | PASS |
| file_reads | `oltp_index_scan` | 12.39ms | 9.26ms | 0.747× | 1.34% | PASS |
| file_reads | `select_random_points` | 27.70ms | 24.64ms | 0.890× | 2.26% | PASS |
| file_reads | `select_random_ranges` | 11.59ms | 7.89ms | 0.681× | 1.52% | PASS |
| file_reads | `covering_index_scan` | 12.35ms | 7.46ms | 0.604× | 1.26% | PASS |
| file_reads | `groupby_scan` | 33.23ms | 34.95ms | 1.052× | 0.92% | PASS |
| file_reads | `index_join` | 11.38ms | 11.38ms | 1.000× | 1.94% | PASS |
| file_reads | `index_join_scan` | 5.96ms | 6.05ms | 1.015× | 3.46% | PASS |
| file_reads | `types_table_scan` | 1.07s | 1.23s | 1.150× | 1.58% | PASS |
| file_reads | `table_scan` | 1.22s | 1.37s | 1.117× | 1.05% | PASS |
| file_reads | `oltp_read_only` | 229.76ms | 175.03ms | 0.762× | 0.87% | PASS |
| file_writes | `oltp_bulk_insert` | 254.71ms | 386.63ms | 1.518× | 0.74% | PASS |
| file_writes | `oltp_insert` | 43.47ms | 52.74ms | 1.213× | 11.23% | PASS |
| file_writes | `oltp_update_index` | 119.29ms | 175.17ms | 1.468× | 1.26% | PASS |
| file_writes | `oltp_update_non_index` | 96.16ms | 114.20ms | 1.188× | 9.12% | PASS |
| file_writes | `oltp_delete_insert` | 90.05ms | 134.09ms | 1.489× | 1.53% | PASS |
| file_writes | `oltp_write_only` | 91.14ms | 85.64ms | 0.940× | 9.62% | PASS |
| file_writes | `types_delete_insert` | 56.67ms | 76.06ms | 1.342× | 1.49% | PASS |
| file_writes | `oltp_read_write` | 142.27ms | 165.05ms | 1.160× | 4.69% | PASS |
| ac_reads | `oltp_point_select` | 55.57ms | 63.72ms | 1.147× | 1.42% | PASS |
| ac_reads | `oltp_range_select` | 17.31ms | 17.20ms | 0.993× | 2.12% | PASS |
| ac_reads | `oltp_sum_range` | 16.09ms | 17.50ms | 1.087× | 1.95% | PASS |
| ac_reads | `oltp_order_range` | 3.58ms | 3.63ms | 1.014× | 2.45% | PASS |
| ac_reads | `oltp_distinct_range` | 4.61ms | 4.72ms | 1.024× | 1.39% | PASS |
| ac_reads | `oltp_index_scan` | 7.52ms | 9.24ms | 1.228× | 1.86% | PASS |
| ac_reads | `select_random_points` | 21.66ms | 24.56ms | 1.134× | 1.88% | PASS |
| ac_reads | `select_random_ranges` | 6.78ms | 7.90ms | 1.165× | 1.11% | PASS |
| ac_reads | `covering_index_scan` | 8.27ms | 7.47ms | 0.904× | 3.45% | PASS |
| ac_reads | `groupby_scan` | 32.49ms | 34.91ms | 1.075× | 1.06% | PASS |
| ac_reads | `index_join` | 9.30ms | 11.44ms | 1.230× | 3.51% | PASS |
| ac_reads | `index_join_scan` | 5.40ms | 5.95ms | 1.101× | 3.06% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.23s | 1.165× | 1.04% | PASS |
| ac_reads | `table_scan` | 1.24s | 1.37s | 1.105× | 2.01% | PASS |
| ac_reads | `oltp_read_only` | 155.80ms | 175.34ms | 1.125× | 1.18% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.27ms | 67.39ms | 2.897× | 7.55% | PASS |
| ac_writes | `oltp_insert_ac` | 26.50ms | 84.48ms | 3.188× | 7.25% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.96ms | 102.82ms | 3.677× | 6.36% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.08ms | 82.73ms | 3.435× | 5.97% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.30ms | 92.77ms | 3.667× | 7.11% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.01ms | 92.30ms | 3.417× | 4.61% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.25ms | 82.36ms | 3.542× | 5.09% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.82ms | 99.47ms | 3.031× | 4.79% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 21.59ms | 23.89ms | 1.107× | 2.82% | PASS |
| mem_reads | `oltp_range_select` | 10.26ms | 8.89ms | 0.867× | 4.09% | PASS |
| mem_reads | `oltp_sum_range` | 9.78ms | 9.01ms | 0.922× | 2.87% | PASS |
| mem_reads | `oltp_order_range` | 2.07ms | 1.96ms | 0.947× | 2.09% | PASS |
| mem_reads | `oltp_distinct_range` | 2.61ms | 2.47ms | 0.949× | 2.31% | PASS |
| mem_reads | `oltp_index_scan` | 3.30ms | 4.53ms | 1.373× | 3.48% | PASS |
| mem_reads | `select_random_points` | 13.70ms | 14.72ms | 1.074× | 3.75% | PASS |
| mem_reads | `select_random_ranges` | 3.05ms | 3.48ms | 1.142× | 2.30% | PASS |
| mem_reads | `covering_index_scan` | 2.42ms | 2.92ms | 1.206× | 4.24% | PASS |
| mem_reads | `groupby_scan` | 19.11ms | 18.81ms | 0.984× | 2.02% | PASS |
| mem_reads | `index_join` | 4.42ms | 6.88ms | 1.555× | 6.31% | PASS |
| mem_reads | `index_join_scan` | 3.39ms | 4.67ms | 1.376× | 3.58% | PASS |
| mem_reads | `types_table_scan` | 669.87ms | 724.92ms | 1.082× | 1.29% | PASS |
| mem_reads | `table_scan` | 848.37ms | 851.36ms | 1.004× | 8.05% | PASS |
| mem_reads | `oltp_read_only` | 66.61ms | 71.01ms | 1.066× | 3.89% | PASS |
| mem_writes | `oltp_bulk_insert` | 136.76ms | 199.03ms | 1.455× | 1.11% | PASS |
| mem_writes | `oltp_insert` | 11.77ms | 24.63ms | 2.092× | 1.94% | PASS |
| mem_writes | `oltp_update_index` | 44.43ms | 89.81ms | 2.021× | 2.01% | PASS |
| mem_writes | `oltp_update_non_index` | 33.67ms | 58.49ms | 1.737× | 2.85% | PASS |
| mem_writes | `oltp_delete_insert` | 29.66ms | 66.38ms | 2.238× | 2.52% | PASS |
| mem_writes | `oltp_write_only` | 17.42ms | 41.85ms | 2.402× | 2.21% | PASS |
| mem_writes | `types_delete_insert` | 20.62ms | 35.44ms | 1.719× | 2.06% | PASS |
| mem_writes | `oltp_read_write` | 52.97ms | 83.59ms | 1.578× | 4.33% | PASS |
| file_reads | `oltp_point_select` | 77.33ms | 42.03ms | 0.544× | 1.31% | PASS |
| file_reads | `oltp_range_select` | 14.61ms | 10.08ms | 0.690× | 1.89% | PASS |
| file_reads | `oltp_sum_range` | 13.87ms | 9.96ms | 0.718× | 1.66% | PASS |
| file_reads | `oltp_order_range` | 2.59ms | 2.12ms | 0.821× | 1.71% | PASS |
| file_reads | `oltp_distinct_range` | 3.10ms | 2.62ms | 0.844× | 1.09% | PASS |
| file_reads | `oltp_index_scan` | 8.86ms | 6.03ms | 0.681× | 1.62% | PASS |
| file_reads | `select_random_points` | 17.79ms | 14.63ms | 0.823× | 2.48% | PASS |
| file_reads | `select_random_ranges` | 8.77ms | 5.45ms | 0.621× | 2.21% | PASS |
| file_reads | `covering_index_scan` | 9.48ms | 5.26ms | 0.555× | 3.56% | PASS |
| file_reads | `groupby_scan` | 20.88ms | 19.82ms | 0.949× | 4.97% | PASS |
| file_reads | `index_join` | 8.37ms | 7.47ms | 0.893× | 4.53% | PASS |
| file_reads | `index_join_scan` | 3.96ms | 4.44ms | 1.123× | 4.84% | PASS |
| file_reads | `types_table_scan` | 770.98ms | 783.55ms | 1.016× | 5.44% | PASS |
| file_reads | `table_scan` | 1.02s | 920.21ms | 0.898× | 5.60% | PASS |
| file_reads | `oltp_read_only` | 151.69ms | 101.64ms | 0.670× | 2.59% | PASS |
| file_writes | `oltp_bulk_insert` | 192.72ms | 274.58ms | 1.425× | 3.25% | PASS |
| file_writes | `oltp_insert` | 34.05ms | 51.52ms | 1.513× | 5.25% | PASS |
| file_writes | `oltp_update_index` | 171.18ms | 186.36ms | 1.089× | 5.94% | PASS |
| file_writes | `oltp_update_non_index` | 130.66ms | 129.11ms | 0.988× | 6.19% | PASS |
| file_writes | `oltp_delete_insert` | 138.55ms | 142.99ms | 1.032× | 5.43% | PASS |
| file_writes | `oltp_write_only` | 99.23ms | 99.10ms | 0.999× | 3.80% | PASS |
| file_writes | `types_delete_insert` | 82.18ms | 79.97ms | 0.973× | 9.50% | PASS |
| file_writes | `oltp_read_write` | 140.77ms | 143.81ms | 1.022× | 4.06% | PASS |
| ac_reads | `oltp_point_select` | 38.55ms | 42.16ms | 1.094× | 2.51% | PASS |
| ac_reads | `oltp_range_select` | 11.09ms | 10.36ms | 0.934× | 2.52% | PASS |
| ac_reads | `oltp_sum_range` | 10.53ms | 10.28ms | 0.976× | 3.27% | PASS |
| ac_reads | `oltp_order_range` | 2.32ms | 2.17ms | 0.934× | 2.50% | PASS |
| ac_reads | `oltp_distinct_range` | 2.91ms | 2.71ms | 0.933× | 1.05% | PASS |
| ac_reads | `oltp_index_scan` | 5.62ms | 6.33ms | 1.126× | 2.35% | PASS |
| ac_reads | `select_random_points` | 14.59ms | 15.21ms | 1.043× | 2.08% | PASS |
| ac_reads | `select_random_ranges` | 4.87ms | 5.38ms | 1.105× | 2.35% | PASS |
| ac_reads | `covering_index_scan` | 5.48ms | 5.08ms | 0.926× | 1.80% | PASS |
| ac_reads | `groupby_scan` | 19.06ms | 18.81ms | 0.987× | 1.70% | PASS |
| ac_reads | `index_join` | 5.81ms | 7.47ms | 1.286× | 3.55% | PASS |
| ac_reads | `index_join_scan` | 3.42ms | 4.37ms | 1.276× | 1.91% | PASS |
| ac_reads | `types_table_scan` | 693.25ms | 743.29ms | 1.072× | 1.08% | PASS |
| ac_reads | `table_scan` | 798.94ms | 835.48ms | 1.046× | 1.72% | PASS |
| ac_reads | `oltp_read_only` | 96.95ms | 100.48ms | 1.036× | 2.60% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.86ms | 54.62ms | 2.618× | 3.45% | PASS |
| ac_writes | `oltp_insert_ac` | 23.81ms | 69.85ms | 2.934× | 3.69% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.43ms | 78.67ms | 3.094× | 4.77% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.19ms | 61.80ms | 3.061× | 4.27% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.11ms | 71.22ms | 3.221× | 4.48% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.56ms | 69.64ms | 3.088× | 4.87% | PASS |
| ac_writes | `types_delete_insert_ac` | 19.52ms | 62.50ms | 3.203× | 4.79% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.18ms | 72.55ms | 2.881× | 4.56% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.72ms | 37.87ms | 1.123× | 1.46% | PASS |
| mem_reads | `oltp_range_select` | 19.88ms | 20.08ms | 1.010× | 0.89% | PASS |
| mem_reads | `oltp_sum_range` | 18.37ms | 19.25ms | 1.048× | 0.98% | PASS |
| mem_reads | `oltp_order_range` | 3.75ms | 3.79ms | 1.010× | 0.81% | PASS |
| mem_reads | `oltp_distinct_range` | 4.80ms | 4.84ms | 1.008× | 0.81% | PASS |
| mem_reads | `oltp_index_scan` | 4.71ms | 5.90ms | 1.253× | 1.61% | PASS |
| mem_reads | `select_random_points` | 27.53ms | 30.30ms | 1.101× | 0.97% | PASS |
| mem_reads | `select_random_ranges` | 7.59ms | 8.24ms | 1.086× | 1.27% | PASS |
| mem_reads | `covering_index_scan` | 4.38ms | 4.16ms | 0.952× | 1.48% | PASS |
| mem_reads | `groupby_scan` | 38.49ms | 39.84ms | 1.035× | 0.51% | PASS |
| mem_reads | `index_join` | 8.10ms | 9.98ms | 1.232× | 0.78% | PASS |
| mem_reads | `index_join_scan` | 4.24ms | 5.55ms | 1.310× | 1.15% | PASS |
| mem_reads | `types_table_scan` | 1.13s | 1.25s | 1.110× | 0.64% | PASS |
| mem_reads | `table_scan` | 1.30s | 1.36s | 1.045× | 0.93% | PASS |
| mem_reads | `oltp_read_only` | 148.83ms | 157.64ms | 1.059× | 0.56% | PASS |
| mem_writes | `oltp_bulk_insert` | 245.38ms | 338.45ms | 1.379× | 0.82% | PASS |
| mem_writes | `oltp_insert` | 19.48ms | 36.05ms | 1.850× | 0.84% | PASS |
| mem_writes | `oltp_update_index` | 68.43ms | 118.52ms | 1.732× | 0.68% | PASS |
| mem_writes | `oltp_update_non_index` | 51.78ms | 84.16ms | 1.625× | 0.93% | PASS |
| mem_writes | `oltp_delete_insert` | 50.15ms | 96.41ms | 1.922× | 0.84% | PASS |
| mem_writes | `oltp_write_only` | 27.26ms | 58.73ms | 2.154× | 0.82% | PASS |
| mem_writes | `types_delete_insert` | 32.40ms | 53.08ms | 1.639× | 1.31% | PASS |
| mem_writes | `oltp_read_write` | 97.62ms | 145.84ms | 1.494× | 0.66% | PASS |
| file_reads | `oltp_point_select` | 128.42ms | 69.22ms | 0.539× | 1.79% | PASS |
| file_reads | `oltp_range_select` | 28.49ms | 23.35ms | 0.820× | 1.09% | PASS |
| file_reads | `oltp_sum_range` | 27.31ms | 22.48ms | 0.823× | 0.81% | PASS |
| file_reads | `oltp_order_range` | 4.56ms | 4.14ms | 0.906× | 1.77% | PASS |
| file_reads | `oltp_distinct_range` | 5.70ms | 5.20ms | 0.912× | 1.53% | PASS |
| file_reads | `oltp_index_scan` | 14.20ms | 9.35ms | 0.658× | 0.80% | PASS |
| file_reads | `select_random_points` | 37.11ms | 33.62ms | 0.906× | 0.78% | PASS |
| file_reads | `select_random_ranges` | 17.06ms | 11.65ms | 0.683× | 1.06% | PASS |
| file_reads | `covering_index_scan` | 14.05ms | 7.78ms | 0.554× | 0.72% | PASS |
| file_reads | `groupby_scan` | 39.58ms | 40.50ms | 1.023× | 0.87% | PASS |
| file_reads | `index_join` | 13.19ms | 12.44ms | 0.943× | 0.93% | PASS |
| file_reads | `index_join_scan` | 5.22ms | 6.07ms | 1.162× | 1.09% | PASS |
| file_reads | `types_table_scan` | 1.11s | 1.24s | 1.116× | 0.45% | PASS |
| file_reads | `table_scan` | 1.28s | 1.35s | 1.051× | 0.55% | PASS |
| file_reads | `oltp_read_only` | 286.29ms | 205.13ms | 0.717× | 0.52% | PASS |
| file_writes | `oltp_bulk_insert` | 262.24ms | 361.00ms | 1.377× | 1.06% | PASS |
| file_writes | `oltp_insert` | 26.26ms | 46.52ms | 1.772× | 1.64% | PASS |
| file_writes | `oltp_update_index` | 98.04ms | 145.21ms | 1.481× | 0.88% | PASS |
| file_writes | `oltp_update_non_index` | 77.15ms | 105.61ms | 1.369× | 1.48% | PASS |
| file_writes | `oltp_delete_insert` | 76.97ms | 120.27ms | 1.563× | 1.27% | PASS |
| file_writes | `oltp_write_only` | 50.66ms | 79.06ms | 1.561× | 2.05% | PASS |
| file_writes | `types_delete_insert` | 49.26ms | 66.88ms | 1.358× | 1.51% | PASS |
| file_writes | `oltp_read_write` | 120.36ms | 166.73ms | 1.385× | 0.84% | PASS |
| ac_reads | `oltp_point_select` | 64.10ms | 69.33ms | 1.082× | 0.89% | PASS |
| ac_reads | `oltp_range_select` | 23.03ms | 23.38ms | 1.015× | 1.22% | PASS |
| ac_reads | `oltp_sum_range` | 21.49ms | 22.57ms | 1.050× | 0.97% | PASS |
| ac_reads | `oltp_order_range` | 4.12ms | 4.15ms | 1.008× | 1.28% | PASS |
| ac_reads | `oltp_distinct_range` | 5.17ms | 5.21ms | 1.009× | 1.17% | PASS |
| ac_reads | `oltp_index_scan` | 8.13ms | 9.37ms | 1.153× | 1.19% | PASS |
| ac_reads | `select_random_points` | 31.52ms | 34.00ms | 1.079× | 1.20% | PASS |
| ac_reads | `select_random_ranges` | 11.04ms | 11.77ms | 1.067× | 0.90% | PASS |
| ac_reads | `covering_index_scan` | 7.97ms | 7.83ms | 0.982× | 0.57% | PASS |
| ac_reads | `groupby_scan` | 39.16ms | 40.54ms | 1.035× | 0.76% | PASS |
| ac_reads | `index_join` | 10.29ms | 12.63ms | 1.227× | 0.98% | PASS |
| ac_reads | `index_join_scan` | 4.78ms | 6.14ms | 1.284× | 1.16% | PASS |
| ac_reads | `types_table_scan` | 1.16s | 1.26s | 1.090× | 1.77% | PASS |
| ac_reads | `table_scan` | 1.35s | 1.37s | 1.012× | 3.27% | PASS |
| ac_reads | `oltp_read_only` | 200.86ms | 207.69ms | 1.034× | 1.00% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.89ms | 54.14ms | 3.206× | 2.61% | PASS |
| ac_writes | `oltp_insert_ac` | 18.35ms | 75.38ms | 4.108× | 3.40% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.34ms | 87.92ms | 4.322× | 3.39% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.55ms | 66.91ms | 4.041× | 6.74% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.07ms | 80.16ms | 4.437× | 4.72% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.49ms | 78.07ms | 4.006× | 4.32% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.29ms | 68.52ms | 3.962× | 8.19% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.85ms | 87.73ms | 3.268× | 4.61% | PASS |

</details>

## Version-control latency

Wall time: 2m 22s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.16ms | 200.00ms | 41.6% | 0.27% | PASS |
| `status_dirty_many_tables` | 86.54ms | 200.00ms | 43.3% | 0.34% | PASS |
| `diff_regular_working_one_table` | 79.10ms | 150.00ms | 52.7% | 0.31% | PASS |
| `diff_regular_working_many_tables` | 91.73ms | 200.00ms | 45.9% | 0.25% | PASS |
| `diff_stat_working_many_tables` | 91.90ms | 200.00ms | 46.0% | 0.38% | PASS |
| `diff_schema_working_many_tables` | 92.56ms | 200.00ms | 46.3% | 0.39% | PASS |
| `branch_list_many_branches` | 22.94ms | 100.00ms | 22.9% | 1.01% | PASS |
| `branch_create_delete` | 24.69ms | 100.00ms | 24.7% | 0.68% | PASS |
| `checkout_branch_clean` | 55.31ms | 200.00ms | 27.7% | 0.71% | PASS |
| `merge_data_no_conflicts` | 29.35ms | 150.00ms | 19.6% | 0.95% | PASS |
| `merge_schema_no_conflicts` | 22.62ms | 100.00ms | 22.6% | 1.21% | PASS |
| `merge_data_conflicts` | 127.53ms | 250.00ms | 51.0% | 0.24% | PASS |
| `merge_data_conflicts_with_resolve` | 127.61ms | 250.00ms | 51.0% | 0.20% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
