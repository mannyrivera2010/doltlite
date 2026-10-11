# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-11 17:29 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261004.327.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/38153187174)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 4m 30s | 8.08s | 9.95s | 1.232× | 2.08% | **PASS** |
| textpk | 69 | 55 | 1h 32m 51s | 9.91s | 11.81s | 1.192× | 1.85% | **PASS** |
| blobpk | 69 | 55 | 1h 7m 56s | 7.12s | 7.91s | 1.111× | 2.63% | **PASS** |
| compositepk | 69 | 55 | 1h 0m 56s | 6.81s | 7.63s | 1.120× | 2.24% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 19.86ms | 22.34ms | 1.125× | 2.01% | PASS |
| mem_reads | `oltp_range_select` | 9.02ms | 10.72ms | 1.188× | 2.57% | PASS |
| mem_reads | `oltp_sum_range` | 8.35ms | 10.08ms | 1.207× | 2.18% | PASS |
| mem_reads | `oltp_order_range` | 2.39ms | 2.65ms | 1.108× | 2.32% | PASS |
| mem_reads | `oltp_distinct_range` | 3.29ms | 3.51ms | 1.067× | 2.03% | PASS |
| mem_reads | `oltp_index_scan` | 3.38ms | 4.07ms | 1.205× | 1.64% | PASS |
| mem_reads | `select_random_points` | 9.48ms | 10.06ms | 1.062× | 2.34% | PASS |
| mem_reads | `select_random_ranges` | 2.54ms | 3.21ms | 1.264× | 2.20% | PASS |
| mem_reads | `covering_index_scan` | 3.31ms | 3.26ms | 0.986× | 3.11% | PASS |
| mem_reads | `groupby_scan` | 27.72ms | 29.56ms | 1.066× | 1.48% | PASS |
| mem_reads | `index_join` | 5.06ms | 6.77ms | 1.339× | 2.45% | PASS |
| mem_reads | `index_join_scan` | 2.79ms | 3.97ms | 1.424× | 2.25% | PASS |
| mem_reads | `types_table_scan` | 930.87ms | 1.12s | 1.204× | 0.57% | PASS |
| mem_reads | `table_scan` | 1.06s | 1.23s | 1.159× | 0.50% | PASS |
| mem_reads | `oltp_read_only` | 87.67ms | 100.56ms | 1.147× | 1.23% | PASS |
| mem_writes | `oltp_bulk_insert` | 133.99ms | 178.03ms | 1.329× | 0.90% | PASS |
| mem_writes | `oltp_insert` | 12.24ms | 21.25ms | 1.736× | 1.22% | PASS |
| mem_writes | `oltp_update_index` | 40.89ms | 82.42ms | 2.016× | 1.67% | PASS |
| mem_writes | `oltp_update_non_index` | 26.96ms | 46.18ms | 1.713× | 1.56% | PASS |
| mem_writes | `oltp_delete_insert` | 37.28ms | 61.43ms | 1.648× | 1.15% | PASS |
| mem_writes | `oltp_write_only` | 17.88ms | 35.51ms | 1.986× | 1.36% | PASS |
| mem_writes | `types_delete_insert` | 19.93ms | 31.77ms | 1.594× | 1.49% | PASS |
| mem_writes | `oltp_read_write` | 54.08ms | 87.72ms | 1.622× | 1.30% | PASS |
| file_reads | `oltp_point_select` | 49.94ms | 32.44ms | 0.650× | 1.90% | PASS |
| file_reads | `oltp_range_select` | 12.47ms | 12.33ms | 0.988× | 2.04% | PASS |
| file_reads | `oltp_sum_range` | 11.77ms | 11.68ms | 0.993× | 2.08% | PASS |
| file_reads | `oltp_order_range` | 2.76ms | 2.85ms | 1.032× | 2.23% | PASS |
| file_reads | `oltp_distinct_range` | 3.69ms | 3.72ms | 1.007× | 1.33% | PASS |
| file_reads | `oltp_index_scan` | 6.55ms | 5.58ms | 0.852× | 2.91% | PASS |
| file_reads | `select_random_points` | 13.13ms | 11.76ms | 0.895× | 1.62% | PASS |
| file_reads | `select_random_ranges` | 5.56ms | 4.32ms | 0.776× | 2.82% | PASS |
| file_reads | `covering_index_scan` | 6.38ms | 4.46ms | 0.700× | 3.14% | PASS |
| file_reads | `groupby_scan` | 28.69ms | 30.21ms | 1.053× | 1.39% | PASS |
| file_reads | `index_join` | 6.84ms | 7.73ms | 1.131× | 2.93% | PASS |
| file_reads | `index_join_scan` | 3.15ms | 4.18ms | 1.325× | 2.24% | PASS |
| file_reads | `types_table_scan` | 955.90ms | 1.16s | 1.210× | 0.74% | PASS |
| file_reads | `table_scan` | 1.09s | 1.27s | 1.161× | 0.52% | PASS |
| file_reads | `oltp_read_only` | 132.84ms | 118.52ms | 0.892× | 1.01% | PASS |
| file_writes | `oltp_bulk_insert` | 170.38ms | 238.64ms | 1.401× | 2.37% | PASS |
| file_writes | `oltp_insert` | 19.83ms | 28.70ms | 1.447× | 1.23% | PASS |
| file_writes | `oltp_update_index` | 127.92ms | 140.18ms | 1.096× | 5.61% | PASS |
| file_writes | `oltp_update_non_index` | 102.54ms | 99.32ms | 0.969× | 2.20% | PASS |
| file_writes | `oltp_delete_insert` | 110.74ms | 111.33ms | 1.005× | 4.23% | PASS |
| file_writes | `oltp_write_only` | 80.76ms | 76.61ms | 0.949× | 6.08% | PASS |
| file_writes | `types_delete_insert` | 60.55ms | 59.76ms | 0.987× | 2.54% | PASS |
| file_writes | `oltp_read_write` | 118.43ms | 129.51ms | 1.094× | 2.09% | PASS |
| ac_reads | `oltp_point_select` | 30.15ms | 32.48ms | 1.077× | 1.56% | PASS |
| ac_reads | `oltp_range_select` | 10.50ms | 12.29ms | 1.171× | 1.55% | PASS |
| ac_reads | `oltp_sum_range` | 10.11ms | 11.68ms | 1.155× | 2.13% | PASS |
| ac_reads | `oltp_order_range` | 2.61ms | 2.89ms | 1.108× | 2.89% | PASS |
| ac_reads | `oltp_distinct_range` | 3.49ms | 3.71ms | 1.063× | 2.55% | PASS |
| ac_reads | `oltp_index_scan` | 4.72ms | 5.44ms | 1.153× | 2.01% | PASS |
| ac_reads | `select_random_points` | 11.23ms | 11.74ms | 1.045× | 1.50% | PASS |
| ac_reads | `select_random_ranges` | 3.66ms | 4.29ms | 1.173× | 1.70% | PASS |
| ac_reads | `covering_index_scan` | 4.48ms | 4.54ms | 1.012× | 2.73% | PASS |
| ac_reads | `groupby_scan` | 28.28ms | 30.10ms | 1.064× | 1.50% | PASS |
| ac_reads | `index_join` | 5.87ms | 7.80ms | 1.329× | 2.16% | PASS |
| ac_reads | `index_join_scan` | 3.02ms | 4.20ms | 1.392× | 1.04% | PASS |
| ac_reads | `types_table_scan` | 931.87ms | 1.13s | 1.209× | 0.60% | PASS |
| ac_reads | `table_scan` | 1.06s | 1.23s | 1.164× | 0.62% | PASS |
| ac_reads | `oltp_read_only` | 100.61ms | 114.75ms | 1.140× | 1.00% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 26.59ms | 70.07ms | 2.635× | 4.73% | PASS |
| ac_writes | `oltp_insert_ac` | 27.98ms | 81.17ms | 2.901× | 4.53% | PASS |
| ac_writes | `oltp_update_index_ac` | 30.33ms | 95.19ms | 3.138× | 6.76% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 27.51ms | 80.28ms | 2.919× | 5.27% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 28.16ms | 87.94ms | 3.123× | 3.99% | PASS |
| ac_writes | `oltp_write_only_ac` | 28.43ms | 87.40ms | 3.074× | 5.38% | PASS |
| ac_writes | `types_delete_insert_ac` | 25.87ms | 84.33ms | 3.260× | 6.14% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.22ms | 93.89ms | 3.007× | 3.77% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.26ms | 38.26ms | 1.265× | 1.91% | PASS |
| mem_reads | `oltp_range_select` | 13.92ms | 14.26ms | 1.024× | 2.38% | PASS |
| mem_reads | `oltp_sum_range` | 12.56ms | 14.15ms | 1.126× | 1.86% | PASS |
| mem_reads | `oltp_order_range` | 3.04ms | 3.16ms | 1.039× | 1.41% | PASS |
| mem_reads | `oltp_distinct_range` | 4.06ms | 4.25ms | 1.045× | 1.50% | PASS |
| mem_reads | `oltp_index_scan` | 4.57ms | 6.43ms | 1.406× | 2.10% | PASS |
| mem_reads | `select_random_points` | 18.47ms | 20.95ms | 1.134× | 2.22% | PASS |
| mem_reads | `select_random_ranges` | 4.07ms | 5.18ms | 1.273× | 1.70% | PASS |
| mem_reads | `covering_index_scan` | 4.60ms | 4.52ms | 0.983× | 2.16% | PASS |
| mem_reads | `groupby_scan` | 32.13ms | 34.44ms | 1.072× | 0.90% | PASS |
| mem_reads | `index_join` | 7.09ms | 9.42ms | 1.330× | 2.05% | PASS |
| mem_reads | `index_join_scan` | 4.89ms | 5.42ms | 1.109× | 4.26% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.24s | 1.155× | 1.08% | PASS |
| mem_reads | `table_scan` | 1.27s | 1.38s | 1.089× | 1.32% | PASS |
| mem_reads | `oltp_read_only` | 122.55ms | 137.73ms | 1.124× | 1.11% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.58ms | 357.27ms | 1.517× | 0.87% | PASS |
| mem_writes | `oltp_insert` | 21.66ms | 39.69ms | 1.832× | 1.13% | PASS |
| mem_writes | `oltp_update_index` | 72.11ms | 133.83ms | 1.856× | 2.01% | PASS |
| mem_writes | `oltp_update_non_index` | 49.55ms | 87.27ms | 1.761× | 1.57% | PASS |
| mem_writes | `oltp_delete_insert` | 51.94ms | 105.02ms | 2.022× | 1.69% | PASS |
| mem_writes | `oltp_write_only` | 29.40ms | 62.19ms | 2.115× | 1.94% | PASS |
| mem_writes | `types_delete_insert` | 34.14ms | 55.49ms | 1.625× | 1.54% | PASS |
| mem_writes | `oltp_read_write` | 90.58ms | 142.02ms | 1.568× | 2.92% | PASS |
| file_reads | `oltp_point_select` | 106.67ms | 65.07ms | 0.610× | 1.22% | PASS |
| file_reads | `oltp_range_select` | 22.05ms | 17.14ms | 0.777× | 2.59% | PASS |
| file_reads | `oltp_sum_range` | 21.09ms | 17.10ms | 0.811× | 2.05% | PASS |
| file_reads | `oltp_order_range` | 4.04ms | 3.54ms | 0.876× | 1.81% | PASS |
| file_reads | `oltp_distinct_range` | 5.10ms | 4.62ms | 0.907× | 1.50% | PASS |
| file_reads | `oltp_index_scan` | 12.68ms | 9.38ms | 0.739× | 1.42% | PASS |
| file_reads | `select_random_points` | 28.00ms | 24.87ms | 0.888× | 1.73% | PASS |
| file_reads | `select_random_ranges` | 11.86ms | 7.94ms | 0.669× | 1.11% | PASS |
| file_reads | `covering_index_scan` | 13.09ms | 7.50ms | 0.573× | 2.30% | PASS |
| file_reads | `groupby_scan` | 33.58ms | 35.04ms | 1.043× | 1.03% | PASS |
| file_reads | `index_join` | 11.83ms | 11.47ms | 0.970× | 2.64% | PASS |
| file_reads | `index_join_scan` | 6.01ms | 6.00ms | 0.998× | 3.08% | PASS |
| file_reads | `types_table_scan` | 1.10s | 1.24s | 1.131× | 1.54% | PASS |
| file_reads | `table_scan` | 1.32s | 1.38s | 1.050× | 2.03% | PASS |
| file_reads | `oltp_read_only` | 236.45ms | 177.42ms | 0.750× | 1.17% | PASS |
| file_writes | `oltp_bulk_insert` | 255.29ms | 387.26ms | 1.517× | 0.83% | PASS |
| file_writes | `oltp_insert` | 49.56ms | 52.76ms | 1.064× | 26.89% | PASS |
| file_writes | `oltp_update_index` | 115.45ms | 170.70ms | 1.479× | 2.05% | PASS |
| file_writes | `oltp_update_non_index` | 93.75ms | 113.75ms | 1.213× | 8.34% | PASS |
| file_writes | `oltp_delete_insert` | 91.88ms | 135.05ms | 1.470× | 1.56% | PASS |
| file_writes | `oltp_write_only` | 91.50ms | 85.73ms | 0.937× | 9.86% | PASS |
| file_writes | `types_delete_insert` | 56.80ms | 75.50ms | 1.329× | 1.85% | PASS |
| file_writes | `oltp_read_write` | 138.21ms | 164.40ms | 1.189× | 4.12% | PASS |
| ac_reads | `oltp_point_select` | 55.80ms | 64.52ms | 1.156× | 1.32% | PASS |
| ac_reads | `oltp_range_select` | 16.81ms | 17.11ms | 1.018× | 1.41% | PASS |
| ac_reads | `oltp_sum_range` | 15.68ms | 17.14ms | 1.093× | 1.80% | PASS |
| ac_reads | `oltp_order_range` | 3.51ms | 3.54ms | 1.009× | 1.91% | PASS |
| ac_reads | `oltp_distinct_range` | 4.53ms | 4.62ms | 1.021× | 1.19% | PASS |
| ac_reads | `oltp_index_scan` | 7.64ms | 9.31ms | 1.220× | 1.77% | PASS |
| ac_reads | `select_random_points` | 22.30ms | 24.85ms | 1.114× | 1.81% | PASS |
| ac_reads | `select_random_ranges` | 6.98ms | 7.98ms | 1.144× | 0.90% | PASS |
| ac_reads | `covering_index_scan` | 8.31ms | 7.48ms | 0.900× | 3.16% | PASS |
| ac_reads | `groupby_scan` | 32.61ms | 34.97ms | 1.072× | 0.97% | PASS |
| ac_reads | `index_join` | 9.43ms | 11.40ms | 1.209× | 3.66% | PASS |
| ac_reads | `index_join_scan` | 5.33ms | 5.99ms | 1.123× | 2.70% | PASS |
| ac_reads | `types_table_scan` | 1.07s | 1.23s | 1.146× | 1.41% | PASS |
| ac_reads | `table_scan` | 1.27s | 1.38s | 1.083× | 1.60% | PASS |
| ac_reads | `oltp_read_only` | 160.01ms | 177.06ms | 1.107× | 1.35% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.92ms | 74.04ms | 2.971× | 7.79% | PASS |
| ac_writes | `oltp_insert_ac` | 26.88ms | 88.42ms | 3.290× | 5.25% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.80ms | 103.25ms | 3.585× | 5.63% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.29ms | 82.45ms | 3.395× | 7.15% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.68ms | 93.84ms | 3.654× | 7.33% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.35ms | 95.09ms | 3.609× | 4.44% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.23ms | 87.72ms | 3.776× | 7.09% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.80ms | 103.84ms | 3.072× | 5.94% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.24ms | 24.10ms | 1.084× | 2.58% | PASS |
| mem_reads | `oltp_range_select` | 9.51ms | 9.05ms | 0.952× | 1.76% | PASS |
| mem_reads | `oltp_sum_range` | 9.64ms | 9.05ms | 0.939× | 2.54% | PASS |
| mem_reads | `oltp_order_range` | 2.06ms | 1.99ms | 0.964× | 2.67% | PASS |
| mem_reads | `oltp_distinct_range` | 2.48ms | 2.39ms | 0.965× | 2.74% | PASS |
| mem_reads | `oltp_index_scan` | 3.02ms | 4.11ms | 1.358× | 4.01% | PASS |
| mem_reads | `select_random_points` | 12.70ms | 14.02ms | 1.104× | 3.71% | PASS |
| mem_reads | `select_random_ranges` | 3.02ms | 3.51ms | 1.163× | 2.72% | PASS |
| mem_reads | `covering_index_scan` | 2.56ms | 2.94ms | 1.146× | 4.25% | PASS |
| mem_reads | `groupby_scan` | 19.84ms | 19.86ms | 1.001× | 1.67% | PASS |
| mem_reads | `index_join` | 4.56ms | 5.85ms | 1.284× | 3.57% | PASS |
| mem_reads | `index_join_scan` | 3.36ms | 4.42ms | 1.317× | 4.49% | PASS |
| mem_reads | `types_table_scan` | 731.37ms | 775.29ms | 1.060× | 1.78% | PASS |
| mem_reads | `table_scan` | 834.28ms | 877.37ms | 1.052× | 2.23% | PASS |
| mem_reads | `oltp_read_only` | 74.47ms | 74.39ms | 0.999× | 4.77% | PASS |
| mem_writes | `oltp_bulk_insert` | 139.45ms | 202.63ms | 1.453× | 2.13% | PASS |
| mem_writes | `oltp_insert` | 12.33ms | 25.43ms | 2.062× | 1.85% | PASS |
| mem_writes | `oltp_update_index` | 42.55ms | 87.26ms | 2.050× | 2.04% | PASS |
| mem_writes | `oltp_update_non_index` | 32.12ms | 56.78ms | 1.768× | 3.05% | PASS |
| mem_writes | `oltp_delete_insert` | 30.96ms | 67.41ms | 2.178× | 1.97% | PASS |
| mem_writes | `oltp_write_only` | 18.35ms | 43.20ms | 2.354× | 2.04% | PASS |
| mem_writes | `types_delete_insert` | 21.37ms | 36.72ms | 1.718× | 2.83% | PASS |
| mem_writes | `oltp_read_write` | 50.85ms | 83.84ms | 1.649× | 3.18% | PASS |
| file_reads | `oltp_point_select` | 78.52ms | 42.83ms | 0.545× | 2.40% | PASS |
| file_reads | `oltp_range_select` | 15.65ms | 10.79ms | 0.689× | 3.31% | PASS |
| file_reads | `oltp_sum_range` | 15.90ms | 10.95ms | 0.688× | 4.10% | PASS |
| file_reads | `oltp_order_range` | 2.85ms | 2.29ms | 0.801× | 1.30% | PASS |
| file_reads | `oltp_distinct_range` | 3.39ms | 2.76ms | 0.815× | 1.45% | PASS |
| file_reads | `oltp_index_scan` | 9.62ms | 6.50ms | 0.675× | 1.53% | PASS |
| file_reads | `select_random_points` | 19.76ms | 15.78ms | 0.799× | 3.25% | PASS |
| file_reads | `select_random_ranges` | 8.96ms | 5.49ms | 0.613× | 2.00% | PASS |
| file_reads | `covering_index_scan` | 9.57ms | 5.27ms | 0.551× | 2.34% | PASS |
| file_reads | `groupby_scan` | 20.62ms | 19.93ms | 0.967× | 1.88% | PASS |
| file_reads | `index_join` | 8.44ms | 7.71ms | 0.914× | 2.31% | PASS |
| file_reads | `index_join_scan` | 3.97ms | 4.46ms | 1.125× | 2.53% | PASS |
| file_reads | `types_table_scan` | 784.38ms | 802.20ms | 1.023× | 2.08% | PASS |
| file_reads | `table_scan` | 970.98ms | 913.34ms | 0.941× | 3.17% | PASS |
| file_reads | `oltp_read_only` | 165.92ms | 106.86ms | 0.644× | 2.71% | PASS |
| file_writes | `oltp_bulk_insert` | 194.59ms | 279.41ms | 1.436× | 3.38% | PASS |
| file_writes | `oltp_insert` | 34.75ms | 51.90ms | 1.494× | 9.51% | PASS |
| file_writes | `oltp_update_index` | 170.72ms | 189.79ms | 1.112× | 5.01% | PASS |
| file_writes | `oltp_update_non_index` | 130.18ms | 122.05ms | 0.938× | 2.63% | PASS |
| file_writes | `oltp_delete_insert` | 134.33ms | 139.28ms | 1.037× | 2.33% | PASS |
| file_writes | `oltp_write_only` | 100.08ms | 98.91ms | 0.988× | 4.50% | PASS |
| file_writes | `types_delete_insert` | 89.62ms | 79.69ms | 0.889× | 8.30% | PASS |
| file_writes | `oltp_read_write` | 130.54ms | 139.05ms | 1.065× | 2.11% | PASS |
| ac_reads | `oltp_point_select` | 39.80ms | 43.06ms | 1.082× | 1.52% | PASS |
| ac_reads | `oltp_range_select` | 11.16ms | 10.74ms | 0.962× | 2.66% | PASS |
| ac_reads | `oltp_sum_range` | 11.39ms | 10.70ms | 0.939× | 2.92% | PASS |
| ac_reads | `oltp_order_range` | 2.43ms | 2.27ms | 0.936× | 1.96% | PASS |
| ac_reads | `oltp_distinct_range` | 2.98ms | 2.75ms | 0.922× | 1.43% | PASS |
| ac_reads | `oltp_index_scan` | 5.67ms | 6.39ms | 1.129× | 2.49% | PASS |
| ac_reads | `select_random_points` | 15.06ms | 15.50ms | 1.029× | 2.21% | PASS |
| ac_reads | `select_random_ranges` | 5.21ms | 5.60ms | 1.075× | 2.03% | PASS |
| ac_reads | `covering_index_scan` | 5.78ms | 5.33ms | 0.922× | 3.73% | PASS |
| ac_reads | `groupby_scan` | 20.42ms | 20.15ms | 0.987× | 1.46% | PASS |
| ac_reads | `index_join` | 6.40ms | 7.71ms | 1.205× | 2.68% | PASS |
| ac_reads | `index_join_scan` | 3.69ms | 4.44ms | 1.204× | 2.47% | PASS |
| ac_reads | `types_table_scan` | 703.53ms | 759.25ms | 1.079× | 1.23% | PASS |
| ac_reads | `table_scan` | 808.66ms | 851.00ms | 1.052× | 1.38% | PASS |
| ac_reads | `oltp_read_only` | 95.06ms | 97.52ms | 1.026× | 1.98% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.36ms | 56.93ms | 2.545× | 5.65% | PASS |
| ac_writes | `oltp_insert_ac` | 26.34ms | 74.27ms | 2.820× | 6.71% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.41ms | 84.15ms | 2.962× | 6.41% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.30ms | 66.58ms | 2.986× | 4.45% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.81ms | 79.74ms | 3.089× | 5.80% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.51ms | 74.08ms | 2.904× | 5.60% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.82ms | 66.19ms | 3.180× | 4.27% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.32ms | 80.52ms | 2.843× | 3.42% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.82ms | 23.54ms | 1.131× | 2.97% | PASS |
| mem_reads | `oltp_range_select` | 10.80ms | 11.06ms | 1.024× | 3.07% | PASS |
| mem_reads | `oltp_sum_range` | 10.43ms | 11.33ms | 1.086× | 2.28% | PASS |
| mem_reads | `oltp_order_range` | 2.13ms | 2.08ms | 0.978× | 2.32% | PASS |
| mem_reads | `oltp_distinct_range` | 2.79ms | 2.68ms | 0.963× | 2.18% | PASS |
| mem_reads | `oltp_index_scan` | 3.00ms | 3.83ms | 1.274× | 4.63% | PASS |
| mem_reads | `select_random_points` | 16.97ms | 19.81ms | 1.167× | 3.67% | PASS |
| mem_reads | `select_random_ranges` | 4.49ms | 5.01ms | 1.115× | 1.90% | PASS |
| mem_reads | `covering_index_scan` | 2.33ms | 2.38ms | 1.019× | 2.55% | PASS |
| mem_reads | `groupby_scan` | 19.96ms | 20.77ms | 1.040× | 2.22% | PASS |
| mem_reads | `index_join` | 4.71ms | 5.78ms | 1.226× | 2.28% | PASS |
| mem_reads | `index_join_scan` | 2.80ms | 4.16ms | 1.486× | 6.68% | PASS |
| mem_reads | `types_table_scan` | 726.47ms | 759.56ms | 1.046× | 2.02% | PASS |
| mem_reads | `table_scan` | 798.17ms | 838.79ms | 1.051× | 1.29% | PASS |
| mem_reads | `oltp_read_only` | 76.89ms | 83.19ms | 1.082× | 1.86% | PASS |
| mem_writes | `oltp_bulk_insert` | 143.60ms | 199.10ms | 1.387× | 1.14% | PASS |
| mem_writes | `oltp_insert` | 11.27ms | 21.90ms | 1.943× | 0.67% | PASS |
| mem_writes | `oltp_update_index` | 39.06ms | 71.04ms | 1.819× | 1.85% | PASS |
| mem_writes | `oltp_update_non_index` | 29.87ms | 51.35ms | 1.719× | 1.63% | PASS |
| mem_writes | `oltp_delete_insert` | 28.41ms | 56.90ms | 2.003× | 1.14% | PASS |
| mem_writes | `oltp_write_only` | 16.20ms | 36.88ms | 2.276× | 1.96% | PASS |
| mem_writes | `types_delete_insert` | 20.11ms | 33.33ms | 1.658× | 2.96% | PASS |
| mem_writes | `oltp_read_write` | 52.26ms | 81.36ms | 1.557× | 1.27% | PASS |
| file_reads | `oltp_point_select` | 73.80ms | 40.75ms | 0.552× | 1.08% | PASS |
| file_reads | `oltp_range_select` | 16.85ms | 13.16ms | 0.781× | 1.78% | PASS |
| file_reads | `oltp_sum_range` | 16.07ms | 13.10ms | 0.815× | 1.54% | PASS |
| file_reads | `oltp_order_range` | 2.90ms | 2.43ms | 0.838× | 1.18% | PASS |
| file_reads | `oltp_distinct_range` | 3.38ms | 2.90ms | 0.857× | 1.56% | PASS |
| file_reads | `oltp_index_scan` | 8.95ms | 5.76ms | 0.643× | 1.68% | PASS |
| file_reads | `select_random_points` | 23.41ms | 21.29ms | 0.909× | 1.86% | PASS |
| file_reads | `select_random_ranges` | 10.79ms | 7.30ms | 0.676× | 1.05% | PASS |
| file_reads | `covering_index_scan` | 8.85ms | 4.64ms | 0.524× | 1.04% | PASS |
| file_reads | `groupby_scan` | 21.33ms | 21.73ms | 1.019× | 1.06% | PASS |
| file_reads | `index_join` | 8.24ms | 7.43ms | 0.902× | 2.43% | PASS |
| file_reads | `index_join_scan` | 3.38ms | 4.13ms | 1.222× | 2.24% | PASS |
| file_reads | `types_table_scan` | 712.56ms | 764.86ms | 1.073× | 2.06% | PASS |
| file_reads | `table_scan` | 870.05ms | 871.38ms | 1.002× | 3.69% | PASS |
| file_reads | `oltp_read_only` | 164.40ms | 116.34ms | 0.708× | 1.32% | PASS |
| file_writes | `oltp_bulk_insert` | 196.48ms | 258.53ms | 1.316× | 2.88% | PASS |
| file_writes | `oltp_insert` | 19.46ms | 38.58ms | 1.983× | 13.62% | PASS |
| file_writes | `oltp_update_index` | 150.65ms | 158.59ms | 1.053× | 6.14% | PASS |
| file_writes | `oltp_update_non_index` | 120.42ms | 108.71ms | 0.903× | 3.30% | PASS |
| file_writes | `oltp_delete_insert` | 124.38ms | 118.96ms | 0.956× | 5.12% | PASS |
| file_writes | `oltp_write_only` | 90.01ms | 89.00ms | 0.989× | 6.38% | PASS |
| file_writes | `types_delete_insert` | 74.94ms | 73.14ms | 0.976× | 5.59% | PASS |
| file_writes | `oltp_read_write` | 120.22ms | 138.22ms | 1.150× | 3.23% | PASS |
| ac_reads | `oltp_point_select` | 39.33ms | 42.85ms | 1.089× | 2.25% | PASS |
| ac_reads | `oltp_range_select` | 13.48ms | 13.45ms | 0.998× | 1.56% | PASS |
| ac_reads | `oltp_sum_range` | 12.24ms | 12.86ms | 1.051× | 1.80% | PASS |
| ac_reads | `oltp_order_range` | 2.62ms | 2.47ms | 0.944× | 1.64% | PASS |
| ac_reads | `oltp_distinct_range` | 3.12ms | 2.97ms | 0.951× | 1.23% | PASS |
| ac_reads | `oltp_index_scan` | 5.16ms | 6.08ms | 1.179× | 2.36% | PASS |
| ac_reads | `select_random_points` | 19.39ms | 20.76ms | 1.071× | 2.89% | PASS |
| ac_reads | `select_random_ranges` | 6.60ms | 7.04ms | 1.067× | 1.85% | PASS |
| ac_reads | `covering_index_scan` | 4.51ms | 4.88ms | 1.084× | 3.29% | PASS |
| ac_reads | `groupby_scan` | 20.62ms | 21.62ms | 1.048× | 1.86% | PASS |
| ac_reads | `index_join` | 6.08ms | 7.70ms | 1.267× | 2.19% | PASS |
| ac_reads | `index_join_scan` | 3.14ms | 4.19ms | 1.336× | 2.58% | PASS |
| ac_reads | `types_table_scan` | 702.60ms | 754.56ms | 1.074× | 1.89% | PASS |
| ac_reads | `table_scan` | 780.00ms | 816.90ms | 1.047× | 1.78% | PASS |
| ac_reads | `oltp_read_only` | 107.39ms | 113.01ms | 1.052× | 2.26% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.86ms | 56.48ms | 2.583× | 3.55% | PASS |
| ac_writes | `oltp_insert_ac` | 26.07ms | 77.00ms | 2.954× | 5.85% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.67ms | 81.96ms | 2.963× | 4.10% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.64ms | 64.29ms | 2.971× | 3.79% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.05ms | 73.37ms | 2.929× | 3.47% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.08ms | 72.80ms | 2.902× | 3.90% | PASS |
| ac_writes | `types_delete_insert_ac` | 25.36ms | 63.71ms | 2.512× | 10.89% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.64ms | 78.36ms | 2.835× | 4.28% | PASS |

</details>

## Version-control latency

Wall time: 1m 39s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 53.97ms | 200.00ms | 27.0% | 0.66% | PASS |
| `status_dirty_many_tables` | 55.99ms | 200.00ms | 28.0% | 0.66% | PASS |
| `diff_regular_working_one_table` | 50.09ms | 150.00ms | 33.4% | 0.51% | PASS |
| `diff_regular_working_many_tables` | 60.26ms | 200.00ms | 30.1% | 0.57% | PASS |
| `diff_stat_working_many_tables` | 60.27ms | 200.00ms | 30.1% | 0.45% | PASS |
| `diff_schema_working_many_tables` | 60.45ms | 200.00ms | 30.2% | 0.43% | PASS |
| `branch_list_many_branches` | 17.13ms | 100.00ms | 17.1% | 1.10% | PASS |
| `branch_create_delete` | 26.05ms | 100.00ms | 26.1% | 0.97% | PASS |
| `checkout_branch_clean` | 83.55ms | 200.00ms | 41.8% | 0.53% | PASS |
| `merge_data_no_conflicts` | 31.18ms | 150.00ms | 20.8% | 1.22% | PASS |
| `merge_schema_no_conflicts` | 17.40ms | 100.00ms | 17.4% | 1.14% | PASS |
| `merge_data_conflicts` | 64.45ms | 250.00ms | 25.8% | 0.35% | PASS |
| `merge_data_conflicts_with_resolve` | 64.43ms | 250.00ms | 25.8% | 0.28% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
