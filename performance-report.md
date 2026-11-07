# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-07 17:23 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu26 20261101.186.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/42388340069)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 25s | 8.97s | 11.62s | 1.295× | 1.64% | **PASS** |
| textpk | 69 | 55 | 1h 29m 32s | 9.59s | 10.03s | 1.046× | 1.97% | **PASS** |
| blobpk | 69 | 55 | 1h 28m 41s | 9.18s | 10.16s | 1.107× | 2.11% | **PASS** |
| compositepk | 69 | 55 | 1h 25m 43s | 9.55s | 11.74s | 1.229× | 1.47% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.68ms | 29.47ms | 1.148× | 1.77% | PASS |
| mem_reads | `oltp_range_select` | 11.29ms | 12.59ms | 1.115× | 1.70% | PASS |
| mem_reads | `oltp_sum_range` | 10.06ms | 12.32ms | 1.225× | 1.00% | PASS |
| mem_reads | `oltp_order_range` | 2.76ms | 3.06ms | 1.109× | 1.23% | PASS |
| mem_reads | `oltp_distinct_range` | 3.70ms | 4.22ms | 1.139× | 0.94% | PASS |
| mem_reads | `oltp_index_scan` | 4.06ms | 5.21ms | 1.285× | 1.80% | PASS |
| mem_reads | `select_random_points` | 11.62ms | 11.21ms | 0.964× | 3.00% | PASS |
| mem_reads | `select_random_ranges` | 3.16ms | 3.98ms | 1.260× | 1.71% | PASS |
| mem_reads | `covering_index_scan` | 4.17ms | 4.17ms | 1.001× | 1.49% | PASS |
| mem_reads | `groupby_scan` | 31.54ms | 36.36ms | 1.153× | 1.92% | PASS |
| mem_reads | `index_join` | 6.03ms | 7.90ms | 1.310× | 1.00% | PASS |
| mem_reads | `index_join_scan` | 3.52ms | 4.43ms | 1.256× | 2.07% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.32s | 1.258× | 0.55% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.45s | 1.236× | 0.60% | PASS |
| mem_reads | `oltp_read_only` | 109.31ms | 125.84ms | 1.151× | 0.88% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.83ms | 253.73ms | 1.411× | 1.13% | PASS |
| mem_writes | `oltp_insert` | 15.36ms | 28.25ms | 1.839× | 0.54% | PASS |
| mem_writes | `oltp_update_index` | 51.48ms | 106.52ms | 2.069× | 0.90% | PASS |
| mem_writes | `oltp_update_non_index` | 35.30ms | 62.10ms | 1.759× | 1.62% | PASS |
| mem_writes | `oltp_delete_insert` | 45.31ms | 81.17ms | 1.791× | 1.06% | PASS |
| mem_writes | `oltp_write_only` | 22.19ms | 47.00ms | 2.118× | 1.18% | PASS |
| mem_writes | `types_delete_insert` | 26.04ms | 42.86ms | 1.646× | 1.55% | PASS |
| mem_writes | `oltp_read_write` | 70.37ms | 112.03ms | 1.592× | 1.41% | PASS |
| file_reads | `oltp_point_select` | 94.23ms | 52.75ms | 0.560× | 0.55% | PASS |
| file_reads | `oltp_range_select` | 18.38ms | 14.97ms | 0.814× | 1.31% | PASS |
| file_reads | `oltp_sum_range` | 17.27ms | 14.85ms | 0.860× | 1.58% | PASS |
| file_reads | `oltp_order_range` | 3.59ms | 3.35ms | 0.935× | 1.42% | PASS |
| file_reads | `oltp_distinct_range` | 4.53ms | 4.53ms | 1.001× | 0.95% | PASS |
| file_reads | `oltp_index_scan` | 11.42ms | 7.98ms | 0.699× | 2.27% | PASS |
| file_reads | `select_random_points` | 18.87ms | 13.66ms | 0.724× | 1.93% | PASS |
| file_reads | `select_random_ranges` | 10.14ms | 6.40ms | 0.631× | 0.98% | PASS |
| file_reads | `covering_index_scan` | 11.52ms | 6.87ms | 0.596× | 2.31% | PASS |
| file_reads | `groupby_scan` | 32.37ms | 36.70ms | 1.133× | 1.94% | PASS |
| file_reads | `index_join` | 10.07ms | 9.83ms | 0.976× | 1.99% | PASS |
| file_reads | `index_join_scan` | 4.46ms | 4.96ms | 1.112× | 2.67% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.35s | 1.305× | 0.38% | PASS |
| file_reads | `table_scan` | 1.15s | 1.44s | 1.249× | 0.54% | PASS |
| file_reads | `oltp_read_only` | 208.63ms | 160.26ms | 0.768× | 0.66% | PASS |
| file_writes | `oltp_bulk_insert` | 193.41ms | 273.64ms | 1.415× | 0.88% | PASS |
| file_writes | `oltp_insert` | 22.20ms | 34.75ms | 1.565× | 1.64% | PASS |
| file_writes | `oltp_update_index` | 80.79ms | 133.78ms | 1.656× | 2.14% | PASS |
| file_writes | `oltp_update_non_index` | 60.39ms | 85.79ms | 1.421× | 2.27% | PASS |
| file_writes | `oltp_delete_insert` | 70.41ms | 105.70ms | 1.501× | 2.02% | PASS |
| file_writes | `oltp_write_only` | 45.47ms | 65.05ms | 1.431× | 1.76% | PASS |
| file_writes | `types_delete_insert` | 41.46ms | 52.98ms | 1.278× | 1.63% | PASS |
| file_writes | `oltp_read_write` | 100.84ms | 133.18ms | 1.321× | 1.44% | PASS |
| ac_reads | `oltp_point_select` | 49.34ms | 53.73ms | 1.089× | 1.21% | PASS |
| ac_reads | `oltp_range_select` | 14.55ms | 15.33ms | 1.053× | 2.12% | PASS |
| ac_reads | `oltp_sum_range` | 13.13ms | 15.08ms | 1.148× | 1.47% | PASS |
| ac_reads | `oltp_order_range` | 3.22ms | 3.45ms | 1.071× | 1.97% | PASS |
| ac_reads | `oltp_distinct_range` | 4.09ms | 4.55ms | 1.113× | 1.12% | PASS |
| ac_reads | `oltp_index_scan` | 6.83ms | 8.14ms | 1.193× | 1.93% | PASS |
| ac_reads | `select_random_points` | 15.09ms | 13.93ms | 0.923× | 2.84% | PASS |
| ac_reads | `select_random_ranges` | 5.66ms | 6.44ms | 1.138× | 1.10% | PASS |
| ac_reads | `covering_index_scan` | 7.08ms | 6.99ms | 0.987× | 2.12% | PASS |
| ac_reads | `groupby_scan` | 32.20ms | 36.19ms | 1.124× | 1.82% | PASS |
| ac_reads | `index_join` | 8.09ms | 10.26ms | 1.269× | 2.01% | PASS |
| ac_reads | `index_join_scan` | 4.08ms | 5.04ms | 1.236× | 2.53% | PASS |
| ac_reads | `types_table_scan` | 1.09s | 1.37s | 1.258× | 1.67% | PASS |
| ac_reads | `table_scan` | 1.24s | 1.47s | 1.186× | 0.87% | PASS |
| ac_reads | `oltp_read_only` | 145.38ms | 162.43ms | 1.117× | 1.07% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.60ms | 64.04ms | 2.965× | 6.87% | PASS |
| ac_writes | `oltp_insert_ac` | 23.52ms | 80.78ms | 3.434× | 6.11% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.79ms | 96.41ms | 3.599× | 6.53% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.37ms | 76.94ms | 3.292× | 7.25% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.04ms | 89.99ms | 3.744× | 6.45% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.71ms | 89.01ms | 3.462× | 5.45% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.72ms | 79.25ms | 3.488× | 7.89% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.83ms | 94.40ms | 3.165× | 4.53% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.14ms | 28.61ms | 1.138× | 1.75% | PASS |
| mem_reads | `oltp_range_select` | 14.12ms | 11.31ms | 0.801× | 2.40% | PASS |
| mem_reads | `oltp_sum_range` | 11.19ms | 11.11ms | 0.993× | 1.47% | PASS |
| mem_reads | `oltp_order_range` | 2.85ms | 2.60ms | 0.912× | 1.71% | PASS |
| mem_reads | `oltp_distinct_range` | 3.58ms | 3.41ms | 0.952× | 0.86% | PASS |
| mem_reads | `oltp_index_scan` | 3.65ms | 4.76ms | 1.302× | 2.99% | PASS |
| mem_reads | `select_random_points` | 14.16ms | 16.09ms | 1.137× | 1.86% | PASS |
| mem_reads | `select_random_ranges` | 3.37ms | 4.22ms | 1.253× | 1.61% | PASS |
| mem_reads | `covering_index_scan` | 3.70ms | 3.44ms | 0.928× | 2.33% | PASS |
| mem_reads | `groupby_scan` | 27.20ms | 27.36ms | 1.006× | 0.84% | PASS |
| mem_reads | `index_join` | 5.69ms | 7.48ms | 1.314× | 2.51% | PASS |
| mem_reads | `index_join_scan` | 4.11ms | 4.78ms | 1.165× | 2.04% | PASS |
| mem_reads | `types_table_scan` | 945.59ms | 999.89ms | 1.057× | 2.81% | PASS |
| mem_reads | `table_scan` | 1.31s | 1.09s | 0.831× | 7.99% | PASS |
| mem_reads | `oltp_read_only` | 106.83ms | 105.55ms | 0.988× | 1.56% | PASS |
| mem_writes | `oltp_bulk_insert` | 177.72ms | 273.48ms | 1.539× | 1.29% | PASS |
| mem_writes | `oltp_insert` | 18.84ms | 32.61ms | 1.731× | 2.23% | PASS |
| mem_writes | `oltp_update_index` | 64.92ms | 119.94ms | 1.847× | 2.59% | PASS |
| mem_writes | `oltp_update_non_index` | 43.48ms | 75.84ms | 1.744× | 1.31% | PASS |
| mem_writes | `oltp_delete_insert` | 47.85ms | 92.11ms | 1.925× | 1.75% | PASS |
| mem_writes | `oltp_write_only` | 27.26ms | 55.70ms | 2.043× | 2.85% | PASS |
| mem_writes | `types_delete_insert` | 27.98ms | 47.90ms | 1.712× | 1.60% | PASS |
| mem_writes | `oltp_read_write` | 73.31ms | 110.84ms | 1.512× | 4.86% | PASS |
| file_reads | `oltp_point_select` | 92.34ms | 50.26ms | 0.544× | 0.69% | PASS |
| file_reads | `oltp_range_select` | 21.00ms | 13.43ms | 0.639× | 2.18% | PASS |
| file_reads | `oltp_sum_range` | 18.44ms | 13.47ms | 0.730× | 1.21% | PASS |
| file_reads | `oltp_order_range` | 3.60ms | 2.86ms | 0.794× | 1.17% | PASS |
| file_reads | `oltp_distinct_range` | 4.36ms | 3.65ms | 0.836× | 0.91% | PASS |
| file_reads | `oltp_index_scan` | 11.06ms | 7.39ms | 0.668× | 0.89% | PASS |
| file_reads | `select_random_points` | 22.16ms | 18.78ms | 0.847× | 1.56% | PASS |
| file_reads | `select_random_ranges` | 10.52ms | 6.59ms | 0.626× | 0.82% | PASS |
| file_reads | `covering_index_scan` | 12.14ms | 6.30ms | 0.519× | 1.15% | PASS |
| file_reads | `groupby_scan` | 28.61ms | 27.89ms | 0.975× | 0.90% | PASS |
| file_reads | `index_join` | 10.23ms | 9.22ms | 0.901× | 1.35% | PASS |
| file_reads | `index_join_scan` | 4.99ms | 5.16ms | 1.034× | 2.16% | PASS |
| file_reads | `types_table_scan` | 942.96ms | 1.00s | 1.062× | 3.41% | PASS |
| file_reads | `table_scan` | 1.10s | 1.06s | 0.964× | 3.70% | PASS |
| file_reads | `oltp_read_only` | 210.42ms | 139.23ms | 0.662× | 1.38% | PASS |
| file_writes | `oltp_bulk_insert` | 244.27ms | 363.19ms | 1.487× | 2.74% | PASS |
| file_writes | `oltp_insert` | 76.50ms | 68.39ms | 0.894× | 10.72% | PASS |
| file_writes | `oltp_update_index` | 228.14ms | 225.32ms | 0.988× | 2.71% | PASS |
| file_writes | `oltp_update_non_index` | 163.50ms | 151.73ms | 0.928× | 4.57% | PASS |
| file_writes | `oltp_delete_insert` | 182.70ms | 175.54ms | 0.961× | 2.54% | PASS |
| file_writes | `oltp_write_only` | 135.45ms | 123.75ms | 0.914× | 2.53% | PASS |
| file_writes | `types_delete_insert` | 107.26ms | 98.23ms | 0.916× | 7.47% | PASS |
| file_writes | `oltp_read_write` | 181.22ms | 177.16ms | 0.978× | 4.04% | PASS |
| ac_reads | `oltp_point_select` | 45.00ms | 50.29ms | 1.117× | 0.73% | PASS |
| ac_reads | `oltp_range_select` | 16.59ms | 13.46ms | 0.811× | 2.11% | PASS |
| ac_reads | `oltp_sum_range` | 13.63ms | 13.40ms | 0.983× | 1.97% | PASS |
| ac_reads | `oltp_order_range` | 3.21ms | 2.87ms | 0.895× | 1.18% | PASS |
| ac_reads | `oltp_distinct_range` | 3.92ms | 3.66ms | 0.933× | 0.73% | PASS |
| ac_reads | `oltp_index_scan` | 6.57ms | 7.49ms | 1.139× | 0.80% | PASS |
| ac_reads | `select_random_points` | 17.60ms | 18.78ms | 1.067× | 1.09% | PASS |
| ac_reads | `select_random_ranges` | 5.86ms | 6.58ms | 1.123× | 0.59% | PASS |
| ac_reads | `covering_index_scan` | 7.40ms | 6.27ms | 0.847× | 0.75% | PASS |
| ac_reads | `groupby_scan` | 28.02ms | 27.81ms | 0.992× | 0.76% | PASS |
| ac_reads | `index_join` | 8.09ms | 9.41ms | 1.163× | 1.28% | PASS |
| ac_reads | `index_join_scan` | 4.60ms | 5.17ms | 1.124× | 1.61% | PASS |
| ac_reads | `types_table_scan` | 960.10ms | 990.31ms | 1.031× | 5.20% | PASS |
| ac_reads | `table_scan` | 1.35s | 1.09s | 0.808× | 7.93% | PASS |
| ac_reads | `oltp_read_only` | 139.39ms | 138.70ms | 0.995× | 1.64% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 18.49ms | 65.22ms | 3.527× | 49.57% | PASS |
| ac_writes | `oltp_insert_ac` | 25.42ms | 83.69ms | 3.292× | 34.84% | PASS |
| ac_writes | `oltp_update_index_ac` | 24.71ms | 103.40ms | 4.184× | 45.34% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 19.40ms | 76.74ms | 3.956× | 40.77% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.77ms | 95.60ms | 3.709× | 37.08% | PASS |
| ac_writes | `oltp_write_only_ac` | 35.92ms | 115.74ms | 3.222× | 55.62% | PASS |
| ac_writes | `types_delete_insert_ac` | 19.36ms | 95.60ms | 4.939× | 35.52% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.90ms | 130.44ms | 4.676× | 58.52% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.05ms | 27.76ms | 1.108× | 2.51% | PASS |
| mem_reads | `oltp_range_select` | 11.65ms | 10.94ms | 0.939× | 3.61% | PASS |
| mem_reads | `oltp_sum_range` | 10.30ms | 10.60ms | 1.029× | 3.52% | PASS |
| mem_reads | `oltp_order_range` | 2.52ms | 2.55ms | 1.014× | 1.62% | PASS |
| mem_reads | `oltp_distinct_range` | 3.29ms | 3.34ms | 1.015× | 1.17% | PASS |
| mem_reads | `oltp_index_scan` | 3.72ms | 4.83ms | 1.299× | 2.11% | PASS |
| mem_reads | `select_random_points` | 15.53ms | 16.24ms | 1.046× | 3.83% | PASS |
| mem_reads | `select_random_ranges` | 3.58ms | 4.29ms | 1.199× | 1.55% | PASS |
| mem_reads | `covering_index_scan` | 3.83ms | 3.94ms | 1.028× | 3.86% | PASS |
| mem_reads | `groupby_scan` | 27.85ms | 27.73ms | 0.996× | 1.07% | PASS |
| mem_reads | `index_join` | 6.12ms | 8.31ms | 1.358× | 4.78% | PASS |
| mem_reads | `index_join_scan` | 4.30ms | 5.04ms | 1.172× | 4.32% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.05s | 1.022× | 1.60% | PASS |
| mem_reads | `table_scan` | 1.20s | 1.11s | 0.925× | 4.51% | PASS |
| mem_reads | `oltp_read_only` | 97.34ms | 102.14ms | 1.049× | 2.95% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.94ms | 268.56ms | 1.484× | 0.77% | PASS |
| mem_writes | `oltp_insert` | 15.56ms | 31.96ms | 2.054× | 1.09% | PASS |
| mem_writes | `oltp_update_index` | 57.27ms | 113.09ms | 1.975× | 1.84% | PASS |
| mem_writes | `oltp_update_non_index` | 42.57ms | 73.69ms | 1.731× | 1.25% | PASS |
| mem_writes | `oltp_delete_insert` | 46.51ms | 92.85ms | 1.996× | 1.81% | PASS |
| mem_writes | `oltp_write_only` | 26.28ms | 56.71ms | 2.158× | 4.45% | PASS |
| mem_writes | `types_delete_insert` | 27.99ms | 46.93ms | 1.677× | 1.52% | PASS |
| mem_writes | `oltp_read_write` | 73.55ms | 114.78ms | 1.561× | 2.33% | PASS |
| file_reads | `oltp_point_select` | 93.66ms | 50.68ms | 0.541× | 1.13% | PASS |
| file_reads | `oltp_range_select` | 18.84ms | 13.32ms | 0.707× | 2.74% | PASS |
| file_reads | `oltp_sum_range` | 17.88ms | 13.19ms | 0.738× | 1.82% | PASS |
| file_reads | `oltp_order_range` | 3.32ms | 2.83ms | 0.854× | 1.27% | PASS |
| file_reads | `oltp_distinct_range` | 4.15ms | 3.61ms | 0.870× | 0.94% | PASS |
| file_reads | `oltp_index_scan` | 10.99ms | 7.28ms | 0.662× | 0.86% | PASS |
| file_reads | `select_random_points` | 21.42ms | 18.40ms | 0.859× | 1.53% | PASS |
| file_reads | `select_random_ranges` | 10.47ms | 6.54ms | 0.625× | 0.68% | PASS |
| file_reads | `covering_index_scan` | 11.70ms | 6.23ms | 0.533× | 1.15% | PASS |
| file_reads | `groupby_scan` | 28.23ms | 27.99ms | 0.991× | 0.91% | PASS |
| file_reads | `index_join` | 10.21ms | 9.41ms | 0.921× | 1.26% | PASS |
| file_reads | `index_join_scan` | 4.63ms | 5.24ms | 1.131× | 2.50% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.05s | 1.006× | 2.56% | PASS |
| file_reads | `table_scan` | 1.12s | 1.09s | 0.971× | 6.05% | PASS |
| file_reads | `oltp_read_only` | 198.39ms | 137.00ms | 0.691× | 1.43% | PASS |
| file_writes | `oltp_bulk_insert` | 238.68ms | 358.60ms | 1.502× | 6.18% | PASS |
| file_writes | `oltp_insert` | 51.41ms | 61.96ms | 1.205× | 11.93% | PASS |
| file_writes | `oltp_update_index` | 199.34ms | 216.74ms | 1.087× | 4.50% | PASS |
| file_writes | `oltp_update_non_index` | 148.64ms | 144.71ms | 0.974× | 3.36% | PASS |
| file_writes | `oltp_delete_insert` | 160.85ms | 172.98ms | 1.075× | 9.55% | PASS |
| file_writes | `oltp_write_only` | 116.43ms | 120.63ms | 1.036× | 5.41% | PASS |
| file_writes | `types_delete_insert` | 99.65ms | 96.19ms | 0.965× | 10.85% | PASS |
| file_writes | `oltp_read_write` | 164.41ms | 178.23ms | 1.084× | 8.61% | PASS |
| ac_reads | `oltp_point_select` | 45.02ms | 50.31ms | 1.117× | 0.91% | PASS |
| ac_reads | `oltp_range_select` | 14.46ms | 13.46ms | 0.931× | 1.88% | PASS |
| ac_reads | `oltp_sum_range` | 13.00ms | 13.21ms | 1.016× | 1.94% | PASS |
| ac_reads | `oltp_order_range` | 2.92ms | 2.84ms | 0.975× | 1.26% | PASS |
| ac_reads | `oltp_distinct_range` | 3.65ms | 3.60ms | 0.986× | 0.99% | PASS |
| ac_reads | `oltp_index_scan` | 6.37ms | 7.28ms | 1.142× | 0.71% | PASS |
| ac_reads | `select_random_points` | 16.74ms | 18.19ms | 1.087× | 1.91% | PASS |
| ac_reads | `select_random_ranges` | 5.82ms | 6.55ms | 1.125× | 0.84% | PASS |
| ac_reads | `covering_index_scan` | 6.79ms | 6.19ms | 0.912× | 0.95% | PASS |
| ac_reads | `groupby_scan` | 27.48ms | 27.74ms | 1.009× | 1.38% | PASS |
| ac_reads | `index_join` | 7.47ms | 9.08ms | 1.216× | 1.51% | PASS |
| ac_reads | `index_join_scan` | 4.14ms | 5.11ms | 1.232× | 1.79% | PASS |
| ac_reads | `types_table_scan` | 963.25ms | 1.03s | 1.068× | 4.64% | PASS |
| ac_reads | `table_scan` | 1.04s | 1.07s | 1.032× | 3.23% | PASS |
| ac_reads | `oltp_read_only` | 126.48ms | 135.20ms | 1.069× | 2.11% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.39ms | 70.77ms | 3.471× | 52.19% | PASS |
| ac_writes | `oltp_insert_ac` | 20.54ms | 79.76ms | 3.882× | 10.45% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.22ms | 98.12ms | 3.890× | 19.37% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.65ms | 98.07ms | 4.531× | 30.64% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.54ms | 95.05ms | 3.721× | 41.62% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.11ms | 146.21ms | 5.601× | 39.15% | PASS |
| ac_writes | `types_delete_insert_ac` | 19.22ms | 80.25ms | 4.176× | 44.91% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.31ms | 102.78ms | 3.506× | 40.36% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.73ms | 39.94ms | 1.220× | 1.82% | PASS |
| mem_reads | `oltp_range_select` | 19.52ms | 21.02ms | 1.077× | 1.38% | PASS |
| mem_reads | `oltp_sum_range` | 17.81ms | 20.87ms | 1.172× | 1.30% | PASS |
| mem_reads | `oltp_order_range` | 3.60ms | 3.88ms | 1.076× | 1.43% | PASS |
| mem_reads | `oltp_distinct_range` | 4.66ms | 5.05ms | 1.084× | 1.14% | PASS |
| mem_reads | `oltp_index_scan` | 4.48ms | 6.03ms | 1.345× | 1.84% | PASS |
| mem_reads | `select_random_points` | 26.52ms | 30.65ms | 1.156× | 1.21% | PASS |
| mem_reads | `select_random_ranges` | 7.63ms | 8.85ms | 1.160× | 1.95% | PASS |
| mem_reads | `covering_index_scan` | 4.12ms | 4.26ms | 1.035× | 1.56% | PASS |
| mem_reads | `groupby_scan` | 37.75ms | 41.57ms | 1.101× | 1.73% | PASS |
| mem_reads | `index_join` | 8.14ms | 10.53ms | 1.293× | 1.93% | PASS |
| mem_reads | `index_join_scan` | 4.11ms | 5.38ms | 1.309× | 2.61% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.24s | 1.181× | 0.86% | PASS |
| mem_reads | `table_scan` | 1.19s | 1.34s | 1.121× | 0.86% | PASS |
| mem_reads | `oltp_read_only` | 152.13ms | 170.82ms | 1.123× | 0.80% | PASS |
| mem_writes | `oltp_bulk_insert` | 239.45ms | 353.21ms | 1.475× | 1.08% | PASS |
| mem_writes | `oltp_insert` | 18.56ms | 36.92ms | 1.989× | 0.74% | PASS |
| mem_writes | `oltp_update_index` | 66.55ms | 118.78ms | 1.785× | 1.18% | PASS |
| mem_writes | `oltp_update_non_index` | 50.22ms | 85.55ms | 1.703× | 1.37% | PASS |
| mem_writes | `oltp_delete_insert` | 48.91ms | 98.36ms | 2.011× | 1.05% | PASS |
| mem_writes | `oltp_write_only` | 26.96ms | 60.56ms | 2.246× | 1.07% | PASS |
| mem_writes | `types_delete_insert` | 32.66ms | 55.91ms | 1.712× | 0.80% | PASS |
| mem_writes | `oltp_read_write` | 102.56ms | 158.98ms | 1.550× | 1.23% | PASS |
| file_reads | `oltp_point_select` | 101.82ms | 63.37ms | 0.622× | 0.88% | PASS |
| file_reads | `oltp_range_select` | 27.35ms | 23.85ms | 0.872× | 1.75% | PASS |
| file_reads | `oltp_sum_range` | 25.51ms | 23.73ms | 0.930× | 0.92% | PASS |
| file_reads | `oltp_order_range` | 4.50ms | 4.23ms | 0.940× | 2.00% | PASS |
| file_reads | `oltp_distinct_range` | 5.60ms | 5.41ms | 0.965× | 1.48% | PASS |
| file_reads | `oltp_index_scan` | 11.66ms | 8.66ms | 0.743× | 2.10% | PASS |
| file_reads | `select_random_points` | 35.47ms | 34.29ms | 0.967× | 1.82% | PASS |
| file_reads | `select_random_ranges` | 15.04ms | 11.53ms | 0.767× | 1.60% | PASS |
| file_reads | `covering_index_scan` | 11.37ms | 6.84ms | 0.602× | 1.97% | PASS |
| file_reads | `groupby_scan` | 38.88ms | 42.83ms | 1.102× | 1.35% | PASS |
| file_reads | `index_join` | 12.20ms | 12.38ms | 1.015× | 1.29% | PASS |
| file_reads | `index_join_scan` | 5.15ms | 5.98ms | 1.162× | 2.46% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.23s | 1.171× | 0.56% | PASS |
| file_reads | `table_scan` | 1.20s | 1.33s | 1.116× | 1.01% | PASS |
| file_reads | `oltp_read_only` | 255.48ms | 207.40ms | 0.812× | 0.88% | PASS |
| file_writes | `oltp_bulk_insert` | 253.05ms | 376.55ms | 1.488× | 1.52% | PASS |
| file_writes | `oltp_insert` | 25.92ms | 47.25ms | 1.823× | 1.19% | PASS |
| file_writes | `oltp_update_index` | 97.48ms | 146.60ms | 1.504× | 1.56% | PASS |
| file_writes | `oltp_update_non_index` | 77.01ms | 107.15ms | 1.391× | 1.73% | PASS |
| file_writes | `oltp_delete_insert` | 77.32ms | 123.12ms | 1.592× | 1.71% | PASS |
| file_writes | `oltp_write_only` | 51.55ms | 80.69ms | 1.565× | 1.71% | PASS |
| file_writes | `types_delete_insert` | 49.57ms | 69.81ms | 1.408× | 1.47% | PASS |
| file_writes | `oltp_read_write` | 131.09ms | 179.35ms | 1.368× | 1.47% | PASS |
| ac_reads | `oltp_point_select` | 54.97ms | 63.82ms | 1.161× | 1.11% | PASS |
| ac_reads | `oltp_range_select` | 22.81ms | 24.00ms | 1.052× | 2.07% | PASS |
| ac_reads | `oltp_sum_range` | 20.54ms | 23.80ms | 1.159× | 1.34% | PASS |
| ac_reads | `oltp_order_range` | 4.02ms | 4.23ms | 1.053× | 1.80% | PASS |
| ac_reads | `oltp_distinct_range` | 5.06ms | 5.42ms | 1.071× | 1.31% | PASS |
| ac_reads | `oltp_index_scan` | 6.94ms | 8.67ms | 1.250× | 1.83% | PASS |
| ac_reads | `select_random_points` | 29.90ms | 34.47ms | 1.153× | 1.34% | PASS |
| ac_reads | `select_random_ranges` | 10.13ms | 11.54ms | 1.139× | 1.28% | PASS |
| ac_reads | `covering_index_scan` | 6.50ms | 6.92ms | 1.064× | 1.86% | PASS |
| ac_reads | `groupby_scan` | 38.28ms | 42.74ms | 1.117× | 1.43% | PASS |
| ac_reads | `index_join` | 9.69ms | 12.44ms | 1.284× | 1.69% | PASS |
| ac_reads | `index_join_scan` | 4.57ms | 5.97ms | 1.305× | 2.71% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.23s | 1.171× | 0.65% | PASS |
| ac_reads | `table_scan` | 1.19s | 1.34s | 1.123× | 0.97% | PASS |
| ac_reads | `oltp_read_only` | 186.99ms | 207.96ms | 1.112× | 1.12% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.84ms | 63.50ms | 3.047× | 4.73% | PASS |
| ac_writes | `oltp_insert_ac` | 23.30ms | 83.94ms | 3.602× | 4.52% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.34ms | 94.36ms | 3.723× | 4.14% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.54ms | 73.91ms | 3.279× | 6.38% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.74ms | 85.60ms | 3.606× | 3.70% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.69ms | 84.87ms | 3.437× | 5.25% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.29ms | 74.91ms | 3.360× | 6.21% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.90ms | 93.51ms | 3.127× | 4.74% | PASS |

</details>

## Version-control latency

Wall time: 1m 57s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 61.37ms | 200.00ms | 30.7% | 0.50% | PASS |
| `status_dirty_many_tables` | 63.86ms | 200.00ms | 31.9% | 0.50% | PASS |
| `diff_regular_working_one_table` | 58.28ms | 150.00ms | 38.9% | 0.69% | PASS |
| `diff_regular_working_many_tables` | 67.98ms | 200.00ms | 34.0% | 0.50% | PASS |
| `diff_stat_working_many_tables` | 68.17ms | 200.00ms | 34.1% | 0.55% | PASS |
| `diff_schema_working_many_tables` | 68.63ms | 200.00ms | 34.3% | 0.62% | PASS |
| `branch_list_many_branches` | 16.17ms | 100.00ms | 16.2% | 2.13% | PASS |
| `branch_create_delete` | 23.89ms | 100.00ms | 23.9% | 5.28% | PASS |
| `checkout_branch_clean` | 85.19ms | 200.00ms | 42.6% | 4.41% | PASS |
| `merge_data_no_conflicts` | 30.68ms | 150.00ms | 20.5% | 2.44% | PASS |
| `merge_schema_no_conflicts` | 16.11ms | 100.00ms | 16.1% | 3.44% | PASS |
| `merge_data_conflicts` | 94.91ms | 250.00ms | 38.0% | 0.43% | PASS |
| `merge_data_conflicts_with_resolve` | 94.91ms | 250.00ms | 38.0% | 0.28% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
