# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-18 19:55 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu26 20261111.196.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/44368409062)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 42s | 8.82s | 11.53s | 1.307× | 1.77% | **PASS** |
| textpk | 69 | 55 | 1h 32m 48s | 10.10s | 10.35s | 1.024× | 1.72% | **PASS** |
| blobpk | 69 | 55 | 1h 28m 6s | 8.78s | 10.10s | 1.150× | 1.96% | **PASS** |
| compositepk | 69 | 55 | 1h 17m 5s | 8.34s | 9.58s | 1.150× | 0.92% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.62ms | 29.59ms | 1.155× | 1.70% | PASS |
| mem_reads | `oltp_range_select` | 11.22ms | 12.53ms | 1.117× | 2.25% | PASS |
| mem_reads | `oltp_sum_range` | 9.96ms | 12.16ms | 1.221× | 2.13% | PASS |
| mem_reads | `oltp_order_range` | 2.75ms | 3.08ms | 1.119× | 0.88% | PASS |
| mem_reads | `oltp_distinct_range` | 3.72ms | 4.22ms | 1.133× | 1.09% | PASS |
| mem_reads | `oltp_index_scan` | 4.00ms | 5.28ms | 1.319× | 1.84% | PASS |
| mem_reads | `select_random_points` | 11.00ms | 11.15ms | 1.013× | 3.20% | PASS |
| mem_reads | `select_random_ranges` | 3.21ms | 3.97ms | 1.236× | 2.51% | PASS |
| mem_reads | `covering_index_scan` | 4.17ms | 4.19ms | 1.004× | 1.97% | PASS |
| mem_reads | `groupby_scan` | 31.44ms | 36.53ms | 1.162× | 2.39% | PASS |
| mem_reads | `index_join` | 6.06ms | 7.97ms | 1.315× | 2.09% | PASS |
| mem_reads | `index_join_scan` | 3.52ms | 4.59ms | 1.305× | 2.25% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.32s | 1.264× | 0.74% | PASS |
| mem_reads | `table_scan` | 1.16s | 1.44s | 1.240× | 1.59% | PASS |
| mem_reads | `oltp_read_only` | 107.50ms | 124.96ms | 1.162× | 1.03% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.31ms | 252.58ms | 1.401× | 1.06% | PASS |
| mem_writes | `oltp_insert` | 15.23ms | 28.05ms | 1.842× | 1.02% | PASS |
| mem_writes | `oltp_update_index` | 50.11ms | 105.85ms | 2.113× | 1.61% | PASS |
| mem_writes | `oltp_update_non_index` | 34.18ms | 61.47ms | 1.798× | 1.41% | PASS |
| mem_writes | `oltp_delete_insert` | 43.34ms | 80.12ms | 1.849× | 1.27% | PASS |
| mem_writes | `oltp_write_only` | 21.66ms | 47.12ms | 2.176× | 1.27% | PASS |
| mem_writes | `types_delete_insert` | 25.44ms | 42.92ms | 1.687× | 2.56% | PASS |
| mem_writes | `oltp_read_write` | 72.44ms | 113.48ms | 1.567× | 2.23% | PASS |
| file_reads | `oltp_point_select` | 94.47ms | 53.12ms | 0.562× | 1.21% | PASS |
| file_reads | `oltp_range_select` | 18.11ms | 14.98ms | 0.827× | 1.52% | PASS |
| file_reads | `oltp_sum_range` | 17.01ms | 14.72ms | 0.866× | 1.77% | PASS |
| file_reads | `oltp_order_range` | 3.57ms | 3.35ms | 0.939× | 1.93% | PASS |
| file_reads | `oltp_distinct_range` | 4.56ms | 4.51ms | 0.989× | 1.45% | PASS |
| file_reads | `oltp_index_scan` | 11.29ms | 7.94ms | 0.703× | 1.77% | PASS |
| file_reads | `select_random_points` | 18.44ms | 13.66ms | 0.741× | 2.90% | PASS |
| file_reads | `select_random_ranges` | 10.23ms | 6.40ms | 0.625× | 1.59% | PASS |
| file_reads | `covering_index_scan` | 11.42ms | 6.80ms | 0.596× | 1.81% | PASS |
| file_reads | `groupby_scan` | 32.34ms | 36.99ms | 1.144× | 2.28% | PASS |
| file_reads | `index_join` | 10.18ms | 9.88ms | 0.971× | 1.66% | PASS |
| file_reads | `index_join_scan` | 4.49ms | 4.93ms | 1.098× | 1.53% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.34s | 1.295× | 0.59% | PASS |
| file_reads | `table_scan` | 1.17s | 1.44s | 1.233× | 0.96% | PASS |
| file_reads | `oltp_read_only` | 212.61ms | 161.32ms | 0.759× | 1.01% | PASS |
| file_writes | `oltp_bulk_insert` | 193.82ms | 274.51ms | 1.416× | 0.92% | PASS |
| file_writes | `oltp_insert` | 22.30ms | 34.98ms | 1.569× | 1.80% | PASS |
| file_writes | `oltp_update_index` | 81.03ms | 136.70ms | 1.687× | 1.60% | PASS |
| file_writes | `oltp_update_non_index` | 59.00ms | 86.35ms | 1.463× | 2.31% | PASS |
| file_writes | `oltp_delete_insert` | 69.82ms | 106.35ms | 1.523× | 1.86% | PASS |
| file_writes | `oltp_write_only` | 44.29ms | 64.42ms | 1.454× | 1.96% | PASS |
| file_writes | `types_delete_insert` | 40.83ms | 52.82ms | 1.294× | 2.50% | PASS |
| file_writes | `oltp_read_write` | 99.31ms | 131.07ms | 1.320× | 1.34% | PASS |
| ac_reads | `oltp_point_select` | 47.53ms | 52.92ms | 1.113× | 1.55% | PASS |
| ac_reads | `oltp_range_select` | 13.79ms | 15.06ms | 1.092× | 2.18% | PASS |
| ac_reads | `oltp_sum_range` | 12.67ms | 14.82ms | 1.170× | 1.65% | PASS |
| ac_reads | `oltp_order_range` | 3.10ms | 3.38ms | 1.091× | 1.03% | PASS |
| ac_reads | `oltp_distinct_range` | 4.09ms | 4.51ms | 1.103× | 1.41% | PASS |
| ac_reads | `oltp_index_scan` | 6.45ms | 7.99ms | 1.239× | 2.07% | PASS |
| ac_reads | `select_random_points` | 13.74ms | 13.66ms | 0.994× | 1.73% | PASS |
| ac_reads | `select_random_ranges` | 5.58ms | 6.43ms | 1.151× | 0.97% | PASS |
| ac_reads | `covering_index_scan` | 6.74ms | 6.90ms | 1.023× | 2.14% | PASS |
| ac_reads | `groupby_scan` | 32.13ms | 37.15ms | 1.156× | 2.05% | PASS |
| ac_reads | `index_join` | 7.70ms | 9.96ms | 1.294× | 2.19% | PASS |
| ac_reads | `index_join_scan` | 3.90ms | 4.98ms | 1.277× | 2.64% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.35s | 1.294× | 0.93% | PASS |
| ac_reads | `table_scan` | 1.15s | 1.43s | 1.249× | 0.46% | PASS |
| ac_reads | `oltp_read_only` | 141.79ms | 160.75ms | 1.134× | 1.38% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.28ms | 63.01ms | 2.962× | 5.72% | PASS |
| ac_writes | `oltp_insert_ac` | 22.95ms | 79.37ms | 3.458× | 7.17% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.37ms | 94.84ms | 3.738× | 4.89% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.65ms | 73.56ms | 3.397× | 7.54% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.94ms | 85.11ms | 3.555× | 4.62% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.44ms | 84.51ms | 3.457× | 4.75% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.77ms | 73.56ms | 3.542× | 6.99% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.65ms | 90.23ms | 3.149× | 5.03% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.73ms | 27.39ms | 1.154× | 1.60% | PASS |
| mem_reads | `oltp_range_select` | 12.50ms | 11.02ms | 0.881× | 3.79% | PASS |
| mem_reads | `oltp_sum_range` | 10.25ms | 10.79ms | 1.053× | 2.16% | PASS |
| mem_reads | `oltp_order_range` | 2.68ms | 2.63ms | 0.982× | 1.72% | PASS |
| mem_reads | `oltp_distinct_range` | 3.47ms | 3.38ms | 0.975× | 0.94% | PASS |
| mem_reads | `oltp_index_scan` | 3.83ms | 4.98ms | 1.300× | 1.67% | PASS |
| mem_reads | `select_random_points` | 15.25ms | 16.74ms | 1.097× | 1.69% | PASS |
| mem_reads | `select_random_ranges` | 3.46ms | 4.25ms | 1.229× | 1.10% | PASS |
| mem_reads | `covering_index_scan` | 4.21ms | 3.90ms | 0.927× | 2.41% | PASS |
| mem_reads | `groupby_scan` | 28.20ms | 27.85ms | 0.988× | 1.07% | PASS |
| mem_reads | `index_join` | 6.01ms | 8.05ms | 1.339× | 2.46% | PASS |
| mem_reads | `index_join_scan` | 4.38ms | 5.19ms | 1.186× | 1.92% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.03s | 0.966× | 9.40% | PASS |
| mem_reads | `table_scan` | 1.27s | 1.09s | 0.857× | 8.38% | PASS |
| mem_reads | `oltp_read_only` | 105.15ms | 104.76ms | 0.996× | 1.59% | PASS |
| mem_writes | `oltp_bulk_insert` | 177.75ms | 269.71ms | 1.517× | 1.07% | PASS |
| mem_writes | `oltp_insert` | 17.69ms | 31.75ms | 1.795× | 1.59% | PASS |
| mem_writes | `oltp_update_index` | 58.67ms | 111.24ms | 1.896× | 1.93% | PASS |
| mem_writes | `oltp_update_non_index` | 41.67ms | 73.83ms | 1.772× | 1.72% | PASS |
| mem_writes | `oltp_delete_insert` | 44.18ms | 88.23ms | 1.997× | 1.23% | PASS |
| mem_writes | `oltp_write_only` | 24.56ms | 52.60ms | 2.141× | 1.05% | PASS |
| mem_writes | `types_delete_insert` | 27.59ms | 46.36ms | 1.680× | 1.93% | PASS |
| mem_writes | `oltp_read_write` | 67.61ms | 107.67ms | 1.592× | 1.13% | PASS |
| file_reads | `oltp_point_select` | 92.59ms | 50.40ms | 0.544× | 0.89% | PASS |
| file_reads | `oltp_range_select` | 20.35ms | 13.57ms | 0.667× | 0.93% | PASS |
| file_reads | `oltp_sum_range` | 17.72ms | 13.31ms | 0.751× | 0.94% | PASS |
| file_reads | `oltp_order_range` | 3.47ms | 2.87ms | 0.826× | 0.95% | PASS |
| file_reads | `oltp_distinct_range` | 4.27ms | 3.64ms | 0.852× | 0.96% | PASS |
| file_reads | `oltp_index_scan` | 11.10ms | 7.39ms | 0.666× | 0.56% | PASS |
| file_reads | `select_random_points` | 22.14ms | 18.56ms | 0.838× | 1.02% | PASS |
| file_reads | `select_random_ranges` | 10.46ms | 6.55ms | 0.626× | 0.50% | PASS |
| file_reads | `covering_index_scan` | 12.07ms | 6.25ms | 0.518× | 0.82% | PASS |
| file_reads | `groupby_scan` | 28.56ms | 27.95ms | 0.979× | 0.75% | PASS |
| file_reads | `index_join` | 10.41ms | 9.34ms | 0.898× | 1.21% | PASS |
| file_reads | `index_join_scan` | 5.08ms | 5.32ms | 1.047× | 1.95% | PASS |
| file_reads | `types_table_scan` | 987.86ms | 1.02s | 1.032× | 5.53% | PASS |
| file_reads | `table_scan` | 1.06s | 1.06s | 1.001× | 1.78% | PASS |
| file_reads | `oltp_read_only` | 197.08ms | 135.12ms | 0.686× | 1.00% | PASS |
| file_writes | `oltp_bulk_insert` | 258.80ms | 368.61ms | 1.424× | 8.54% | PASS |
| file_writes | `oltp_insert` | 68.89ms | 67.56ms | 0.981× | 17.35% | PASS |
| file_writes | `oltp_update_index` | 240.02ms | 219.84ms | 0.916× | 11.31% | PASS |
| file_writes | `oltp_update_non_index` | 174.29ms | 154.16ms | 0.885× | 5.72% | PASS |
| file_writes | `oltp_delete_insert` | 196.76ms | 174.21ms | 0.885× | 8.14% | PASS |
| file_writes | `oltp_write_only` | 217.98ms | 124.50ms | 0.571× | 28.44% | PASS |
| file_writes | `types_delete_insert` | 121.50ms | 111.87ms | 0.921× | 16.55% | PASS |
| file_writes | `oltp_read_write` | 203.31ms | 180.28ms | 0.887× | 17.07% | PASS |
| ac_reads | `oltp_point_select` | 45.19ms | 50.65ms | 1.121× | 0.89% | PASS |
| ac_reads | `oltp_range_select` | 16.06ms | 13.67ms | 0.851× | 1.51% | PASS |
| ac_reads | `oltp_sum_range` | 13.71ms | 13.68ms | 0.998× | 1.23% | PASS |
| ac_reads | `oltp_order_range` | 3.13ms | 2.91ms | 0.929× | 1.14% | PASS |
| ac_reads | `oltp_distinct_range` | 3.85ms | 3.66ms | 0.951× | 0.76% | PASS |
| ac_reads | `oltp_index_scan` | 6.65ms | 7.56ms | 1.138× | 1.21% | PASS |
| ac_reads | `select_random_points` | 18.27ms | 19.43ms | 1.063× | 1.82% | PASS |
| ac_reads | `select_random_ranges` | 5.87ms | 6.61ms | 1.126× | 0.63% | PASS |
| ac_reads | `covering_index_scan` | 7.56ms | 6.30ms | 0.834× | 0.96% | PASS |
| ac_reads | `groupby_scan` | 28.32ms | 28.04ms | 0.990× | 0.93% | PASS |
| ac_reads | `index_join` | 8.14ms | 9.46ms | 1.162× | 1.91% | PASS |
| ac_reads | `index_join_scan` | 4.67ms | 5.35ms | 1.144× | 2.25% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.02s | 0.960× | 6.37% | PASS |
| ac_reads | `table_scan` | 1.51s | 1.14s | 0.751× | 2.95% | PASS |
| ac_reads | `oltp_read_only` | 134.88ms | 139.52ms | 1.034× | 2.88% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 28.55ms | 76.09ms | 2.665× | 38.55% | PASS |
| ac_writes | `oltp_insert_ac` | 31.08ms | 120.37ms | 3.873× | 49.23% | PASS |
| ac_writes | `oltp_update_index_ac` | 34.81ms | 140.35ms | 4.032× | 50.29% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 34.58ms | 139.60ms | 4.037× | 61.59% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 42.48ms | 138.31ms | 3.256× | 60.72% | PASS |
| ac_writes | `oltp_write_only_ac` | 32.90ms | 142.71ms | 4.337× | 51.46% | PASS |
| ac_writes | `types_delete_insert_ac` | 19.80ms | 114.81ms | 5.798× | 56.04% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.51ms | 106.77ms | 3.618× | 31.17% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.25ms | 27.04ms | 1.163× | 2.26% | PASS |
| mem_reads | `oltp_range_select` | 10.96ms | 10.71ms | 0.977× | 2.68% | PASS |
| mem_reads | `oltp_sum_range` | 9.38ms | 10.45ms | 1.114× | 2.79% | PASS |
| mem_reads | `oltp_order_range` | 2.40ms | 2.50ms | 1.038× | 1.47% | PASS |
| mem_reads | `oltp_distinct_range` | 3.25ms | 3.33ms | 1.025× | 1.14% | PASS |
| mem_reads | `oltp_index_scan` | 3.59ms | 4.68ms | 1.305× | 1.96% | PASS |
| mem_reads | `select_random_points` | 13.55ms | 15.47ms | 1.142× | 2.00% | PASS |
| mem_reads | `select_random_ranges` | 3.24ms | 4.13ms | 1.275× | 1.23% | PASS |
| mem_reads | `covering_index_scan` | 3.51ms | 3.65ms | 1.039× | 2.89% | PASS |
| mem_reads | `groupby_scan` | 27.40ms | 27.47ms | 1.002× | 0.96% | PASS |
| mem_reads | `index_join` | 5.55ms | 7.34ms | 1.323× | 3.61% | PASS |
| mem_reads | `index_join_scan` | 3.99ms | 4.87ms | 1.220× | 5.18% | PASS |
| mem_reads | `types_table_scan` | 965.73ms | 1.01s | 1.050× | 3.06% | PASS |
| mem_reads | `table_scan` | 1.05s | 1.07s | 1.015× | 2.80% | PASS |
| mem_reads | `oltp_read_only` | 94.09ms | 101.04ms | 1.074× | 1.96% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.03ms | 265.12ms | 1.473× | 0.79% | PASS |
| mem_writes | `oltp_insert` | 15.56ms | 31.77ms | 2.042× | 1.31% | PASS |
| mem_writes | `oltp_update_index` | 56.30ms | 110.09ms | 1.956× | 1.39% | PASS |
| mem_writes | `oltp_update_non_index` | 39.62ms | 69.20ms | 1.746× | 1.46% | PASS |
| mem_writes | `oltp_delete_insert` | 39.00ms | 83.44ms | 2.140× | 1.32% | PASS |
| mem_writes | `oltp_write_only` | 23.29ms | 52.67ms | 2.262× | 1.75% | PASS |
| mem_writes | `types_delete_insert` | 25.93ms | 44.43ms | 1.714× | 3.09% | PASS |
| mem_writes | `oltp_read_write` | 68.11ms | 107.91ms | 1.584× | 4.36% | PASS |
| file_reads | `oltp_point_select` | 91.86ms | 49.97ms | 0.544× | 0.64% | PASS |
| file_reads | `oltp_range_select` | 18.73ms | 13.25ms | 0.707× | 1.43% | PASS |
| file_reads | `oltp_sum_range` | 16.82ms | 12.91ms | 0.768× | 0.82% | PASS |
| file_reads | `oltp_order_range` | 3.27ms | 2.82ms | 0.862× | 1.30% | PASS |
| file_reads | `oltp_distinct_range` | 4.10ms | 3.61ms | 0.880× | 1.22% | PASS |
| file_reads | `oltp_index_scan` | 10.99ms | 7.34ms | 0.668× | 1.45% | PASS |
| file_reads | `select_random_points` | 21.46ms | 18.25ms | 0.851× | 1.92% | PASS |
| file_reads | `select_random_ranges` | 10.40ms | 6.59ms | 0.634× | 0.99% | PASS |
| file_reads | `covering_index_scan` | 11.36ms | 6.20ms | 0.546× | 1.55% | PASS |
| file_reads | `groupby_scan` | 28.09ms | 27.74ms | 0.988× | 1.02% | PASS |
| file_reads | `index_join` | 9.83ms | 9.17ms | 0.932× | 1.56% | PASS |
| file_reads | `index_join_scan` | 4.49ms | 5.03ms | 1.119× | 1.72% | PASS |
| file_reads | `types_table_scan` | 931.43ms | 1.00s | 1.074× | 4.04% | PASS |
| file_reads | `table_scan` | 1.02s | 1.06s | 1.035× | 2.10% | PASS |
| file_reads | `oltp_read_only` | 193.16ms | 133.82ms | 0.693× | 1.22% | PASS |
| file_writes | `oltp_bulk_insert` | 240.89ms | 344.71ms | 1.431× | 4.37% | PASS |
| file_writes | `oltp_insert` | 51.55ms | 63.78ms | 1.237× | 18.30% | PASS |
| file_writes | `oltp_update_index` | 195.21ms | 214.64ms | 1.100× | 12.25% | PASS |
| file_writes | `oltp_update_non_index` | 150.37ms | 148.93ms | 0.990× | 22.05% | PASS |
| file_writes | `oltp_delete_insert` | 156.89ms | 169.32ms | 1.079× | 7.93% | PASS |
| file_writes | `oltp_write_only` | 116.66ms | 121.70ms | 1.043× | 21.94% | PASS |
| file_writes | `types_delete_insert` | 101.65ms | 99.87ms | 0.983× | 14.78% | PASS |
| file_writes | `oltp_read_write` | 188.40ms | 189.72ms | 1.007× | 23.94% | PASS |
| ac_reads | `oltp_point_select` | 45.53ms | 50.46ms | 1.108× | 1.98% | PASS |
| ac_reads | `oltp_range_select` | 13.90ms | 13.24ms | 0.953× | 1.39% | PASS |
| ac_reads | `oltp_sum_range` | 12.48ms | 13.05ms | 1.046× | 1.22% | PASS |
| ac_reads | `oltp_order_range` | 2.91ms | 2.83ms | 0.970× | 1.10% | PASS |
| ac_reads | `oltp_distinct_range` | 3.65ms | 3.61ms | 0.989× | 0.89% | PASS |
| ac_reads | `oltp_index_scan` | 6.36ms | 7.34ms | 1.154× | 0.88% | PASS |
| ac_reads | `select_random_points` | 17.22ms | 18.46ms | 1.072× | 2.64% | PASS |
| ac_reads | `select_random_ranges` | 5.74ms | 6.57ms | 1.144× | 0.53% | PASS |
| ac_reads | `covering_index_scan` | 6.69ms | 6.20ms | 0.926× | 0.83% | PASS |
| ac_reads | `groupby_scan` | 27.75ms | 27.97ms | 1.008× | 1.30% | PASS |
| ac_reads | `index_join` | 7.62ms | 9.23ms | 1.211× | 1.68% | PASS |
| ac_reads | `index_join_scan` | 4.08ms | 5.00ms | 1.227× | 1.96% | PASS |
| ac_reads | `types_table_scan` | 876.02ms | 980.16ms | 1.119× | 0.81% | PASS |
| ac_reads | `table_scan` | 1.11s | 1.08s | 0.972× | 8.27% | PASS |
| ac_reads | `oltp_read_only` | 132.64ms | 138.62ms | 1.045× | 2.00% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 19.63ms | 63.76ms | 3.248× | 52.88% | PASS |
| ac_writes | `oltp_insert_ac` | 22.57ms | 101.71ms | 4.506× | 45.16% | PASS |
| ac_writes | `oltp_update_index_ac` | 31.62ms | 106.82ms | 3.378× | 53.39% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 65.25ms | 272.38ms | 4.175× | 85.18% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 51.60ms | 116.01ms | 2.248× | 62.72% | PASS |
| ac_writes | `oltp_write_only_ac` | 21.41ms | 107.85ms | 5.037× | 42.93% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.16ms | 97.06ms | 4.587× | 51.99% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.32ms | 97.19ms | 3.315× | 36.31% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.88ms | 28.84ms | 1.115× | 1.32% | PASS |
| mem_reads | `oltp_range_select` | 15.11ms | 15.60ms | 1.032× | 1.10% | PASS |
| mem_reads | `oltp_sum_range` | 13.44ms | 15.07ms | 1.121× | 1.23% | PASS |
| mem_reads | `oltp_order_range` | 2.93ms | 2.96ms | 1.012× | 1.16% | PASS |
| mem_reads | `oltp_distinct_range` | 3.78ms | 3.75ms | 0.992× | 0.91% | PASS |
| mem_reads | `oltp_index_scan` | 3.66ms | 4.42ms | 1.208× | 1.14% | PASS |
| mem_reads | `select_random_points` | 20.25ms | 22.73ms | 1.123× | 0.99% | PASS |
| mem_reads | `select_random_ranges` | 5.84ms | 6.37ms | 1.091× | 1.16% | PASS |
| mem_reads | `covering_index_scan` | 3.33ms | 3.18ms | 0.956× | 1.41% | PASS |
| mem_reads | `groupby_scan` | 29.95ms | 30.82ms | 1.029× | 0.65% | PASS |
| mem_reads | `index_join` | 6.34ms | 7.77ms | 1.226× | 0.96% | PASS |
| mem_reads | `index_join_scan` | 3.28ms | 4.46ms | 1.357× | 0.76% | PASS |
| mem_reads | `types_table_scan` | 869.59ms | 977.59ms | 1.124× | 0.52% | PASS |
| mem_reads | `table_scan` | 996.50ms | 1.06s | 1.064× | 0.70% | PASS |
| mem_reads | `oltp_read_only` | 115.59ms | 122.99ms | 1.064× | 0.64% | PASS |
| mem_writes | `oltp_bulk_insert` | 187.96ms | 264.08ms | 1.405× | 0.80% | PASS |
| mem_writes | `oltp_insert` | 14.75ms | 28.32ms | 1.920× | 0.82% | PASS |
| mem_writes | `oltp_update_index` | 53.16ms | 93.18ms | 1.753× | 0.89% | PASS |
| mem_writes | `oltp_update_non_index` | 40.47ms | 67.22ms | 1.661× | 0.90% | PASS |
| mem_writes | `oltp_delete_insert` | 39.12ms | 76.24ms | 1.949× | 0.85% | PASS |
| mem_writes | `oltp_write_only` | 21.55ms | 47.77ms | 2.217× | 1.17% | PASS |
| mem_writes | `types_delete_insert` | 25.57ms | 42.35ms | 1.656× | 1.24% | PASS |
| mem_writes | `oltp_read_write` | 75.33ms | 115.72ms | 1.536× | 0.92% | PASS |
| file_reads | `oltp_point_select` | 94.34ms | 52.14ms | 0.553× | 0.49% | PASS |
| file_reads | `oltp_range_select` | 22.94ms | 18.09ms | 0.789× | 0.99% | PASS |
| file_reads | `oltp_sum_range` | 21.04ms | 17.59ms | 0.836× | 0.92% | PASS |
| file_reads | `oltp_order_range` | 3.80ms | 3.28ms | 0.864× | 1.04% | PASS |
| file_reads | `oltp_distinct_range` | 4.64ms | 4.10ms | 0.885× | 1.20% | PASS |
| file_reads | `oltp_index_scan` | 10.93ms | 7.25ms | 0.663× | 0.71% | PASS |
| file_reads | `select_random_points` | 28.06ms | 25.49ms | 0.908× | 0.75% | PASS |
| file_reads | `select_random_ranges` | 13.07ms | 8.89ms | 0.680× | 0.92% | PASS |
| file_reads | `covering_index_scan` | 10.61ms | 5.99ms | 0.565× | 0.51% | PASS |
| file_reads | `groupby_scan` | 30.98ms | 31.82ms | 1.027× | 0.90% | PASS |
| file_reads | `index_join` | 10.40ms | 9.85ms | 0.947× | 0.53% | PASS |
| file_reads | `index_join_scan` | 4.20ms | 4.86ms | 1.158× | 0.76% | PASS |
| file_reads | `types_table_scan` | 875.82ms | 983.27ms | 1.123× | 0.72% | PASS |
| file_reads | `table_scan` | 1.00s | 1.06s | 1.055× | 0.49% | PASS |
| file_reads | `oltp_read_only` | 216.58ms | 157.53ms | 0.727× | 0.60% | PASS |
| file_writes | `oltp_bulk_insert` | 238.68ms | 332.36ms | 1.392× | 2.21% | PASS |
| file_writes | `oltp_insert` | 31.96ms | 50.81ms | 1.590× | 5.42% | PASS |
| file_writes | `oltp_update_index` | 167.50ms | 173.57ms | 1.036× | 11.54% | PASS |
| file_writes | `oltp_update_non_index` | 138.68ms | 127.73ms | 0.921× | 4.86% | PASS |
| file_writes | `oltp_delete_insert` | 135.10ms | 144.16ms | 1.067× | 4.99% | PASS |
| file_writes | `oltp_write_only` | 96.93ms | 103.69ms | 1.070× | 2.00% | PASS |
| file_writes | `types_delete_insert` | 88.21ms | 83.26ms | 0.944× | 3.67% | PASS |
| file_writes | `oltp_read_write` | 154.60ms | 173.49ms | 1.122× | 2.13% | PASS |
| ac_reads | `oltp_point_select` | 47.19ms | 52.24ms | 1.107× | 0.87% | PASS |
| ac_reads | `oltp_range_select` | 18.02ms | 18.11ms | 1.005× | 1.06% | PASS |
| ac_reads | `oltp_sum_range` | 16.19ms | 17.59ms | 1.086× | 0.87% | PASS |
| ac_reads | `oltp_order_range` | 3.37ms | 3.28ms | 0.973× | 0.77% | PASS |
| ac_reads | `oltp_distinct_range` | 4.18ms | 4.09ms | 0.978× | 1.00% | PASS |
| ac_reads | `oltp_index_scan` | 6.28ms | 7.25ms | 1.154× | 0.67% | PASS |
| ac_reads | `select_random_points` | 23.29ms | 25.44ms | 1.092× | 0.83% | PASS |
| ac_reads | `select_random_ranges` | 8.33ms | 8.86ms | 1.063× | 0.74% | PASS |
| ac_reads | `covering_index_scan` | 5.94ms | 6.01ms | 1.013× | 0.59% | PASS |
| ac_reads | `groupby_scan` | 30.47ms | 31.73ms | 1.041× | 0.73% | PASS |
| ac_reads | `index_join` | 8.04ms | 9.84ms | 1.224× | 0.71% | PASS |
| ac_reads | `index_join_scan` | 3.78ms | 4.86ms | 1.287× | 0.96% | PASS |
| ac_reads | `types_table_scan` | 875.46ms | 983.85ms | 1.124× | 0.72% | PASS |
| ac_reads | `table_scan` | 1.01s | 1.06s | 1.050× | 0.67% | PASS |
| ac_reads | `oltp_read_only` | 147.27ms | 157.38ms | 1.069× | 0.81% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.84ms | 55.09ms | 3.089× | 11.05% | PASS |
| ac_writes | `oltp_insert_ac` | 21.29ms | 75.11ms | 3.529× | 12.02% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.40ms | 94.17ms | 4.401× | 29.02% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 18.01ms | 64.13ms | 3.561× | 10.20% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 19.44ms | 73.61ms | 3.786× | 9.39% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.45ms | 72.74ms | 3.740× | 8.55% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.22ms | 64.92ms | 3.770× | 8.64% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.69ms | 79.73ms | 3.229× | 5.79% | PASS |

</details>

## Version-control latency

Wall time: 1m 57s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 61.71ms | 200.00ms | 30.9% | 0.26% | PASS |
| `status_dirty_many_tables` | 63.80ms | 200.00ms | 31.9% | 0.25% | PASS |
| `diff_regular_working_one_table` | 57.38ms | 150.00ms | 38.3% | 0.39% | PASS |
| `diff_regular_working_many_tables` | 67.20ms | 200.00ms | 33.6% | 0.34% | PASS |
| `diff_stat_working_many_tables` | 67.23ms | 200.00ms | 33.6% | 0.27% | PASS |
| `diff_schema_working_many_tables` | 67.59ms | 200.00ms | 33.8% | 0.28% | PASS |
| `branch_list_many_branches` | 15.36ms | 100.00ms | 15.4% | 0.79% | PASS |
| `branch_create_delete` | 22.00ms | 100.00ms | 22.0% | 4.55% | PASS |
| `checkout_branch_clean` | 87.11ms | 200.00ms | 43.6% | 8.95% | PASS |
| `merge_data_no_conflicts` | 28.44ms | 150.00ms | 19.0% | 3.91% | PASS |
| `merge_schema_no_conflicts` | 15.07ms | 100.00ms | 15.1% | 2.99% | PASS |
| `merge_data_conflicts` | 93.73ms | 250.00ms | 37.5% | 0.24% | PASS |
| `merge_data_conflicts_with_resolve` | 94.06ms | 250.00ms | 37.6% | 0.29% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
