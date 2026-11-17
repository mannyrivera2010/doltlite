# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-17 20:18 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu26 20261111.196.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/44150763035)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 56m 16s | 6.58s | 7.52s | 1.143× | 2.68% | **PASS** |
| textpk | 69 | 55 | 1h 33m 7s | 10.29s | 11.80s | 1.147× | 1.40% | **PASS** |
| blobpk | 69 | 55 | 57m 53s | 6.96s | 8.07s | 1.159× | 1.63% | **PASS** |
| compositepk | 69 | 55 | 1h 24m 55s | 8.87s | 10.56s | 1.190× | 14.14% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 18.55ms | 19.07ms | 1.028× | 2.32% | PASS |
| mem_reads | `oltp_range_select` | 6.71ms | 7.30ms | 1.089× | 2.47% | PASS |
| mem_reads | `oltp_sum_range` | 6.43ms | 7.49ms | 1.165× | 2.32% | PASS |
| mem_reads | `oltp_order_range` | 1.81ms | 1.90ms | 1.050× | 2.31% | PASS |
| mem_reads | `oltp_distinct_range` | 2.33ms | 2.44ms | 1.045× | 1.69% | PASS |
| mem_reads | `oltp_index_scan` | 2.86ms | 3.73ms | 1.302× | 2.53% | PASS |
| mem_reads | `select_random_points` | 11.09ms | 9.65ms | 0.870× | 2.30% | PASS |
| mem_reads | `select_random_ranges` | 2.30ms | 2.61ms | 1.135× | 1.51% | PASS |
| mem_reads | `covering_index_scan` | 2.39ms | 2.58ms | 1.079× | 4.59% | PASS |
| mem_reads | `groupby_scan` | 18.89ms | 20.07ms | 1.063× | 1.14% | PASS |
| mem_reads | `index_join` | 3.81ms | 5.21ms | 1.368× | 5.47% | PASS |
| mem_reads | `index_join_scan` | 2.58ms | 3.91ms | 1.515× | 4.31% | PASS |
| mem_reads | `types_table_scan` | 716.63ms | 794.50ms | 1.109× | 1.62% | PASS |
| mem_reads | `table_scan` | 787.63ms | 864.94ms | 1.098× | 1.69% | PASS |
| mem_reads | `oltp_read_only` | 64.81ms | 68.91ms | 1.063× | 4.02% | PASS |
| mem_writes | `oltp_bulk_insert` | 110.29ms | 156.25ms | 1.417× | 1.65% | PASS |
| mem_writes | `oltp_insert` | 10.20ms | 20.42ms | 2.002× | 2.00% | PASS |
| mem_writes | `oltp_update_index` | 39.54ms | 89.85ms | 2.272× | 3.39% | PASS |
| mem_writes | `oltp_update_non_index` | 27.10ms | 46.30ms | 1.708× | 2.45% | PASS |
| mem_writes | `oltp_delete_insert` | 29.83ms | 56.29ms | 1.887× | 2.49% | PASS |
| mem_writes | `oltp_write_only` | 14.44ms | 32.61ms | 2.258× | 3.76% | PASS |
| mem_writes | `types_delete_insert` | 17.54ms | 29.16ms | 1.663× | 2.93% | PASS |
| mem_writes | `oltp_read_write` | 41.58ms | 67.67ms | 1.628× | 3.10% | PASS |
| file_reads | `oltp_point_select` | 71.86ms | 36.65ms | 0.510× | 1.75% | PASS |
| file_reads | `oltp_range_select` | 12.72ms | 9.40ms | 0.739× | 3.54% | PASS |
| file_reads | `oltp_sum_range` | 11.89ms | 9.23ms | 0.776× | 2.56% | PASS |
| file_reads | `oltp_order_range` | 2.39ms | 2.07ms | 0.866× | 1.60% | PASS |
| file_reads | `oltp_distinct_range` | 2.93ms | 2.60ms | 0.887× | 1.39% | PASS |
| file_reads | `oltp_index_scan` | 8.55ms | 5.70ms | 0.667× | 2.49% | PASS |
| file_reads | `select_random_points` | 15.71ms | 10.65ms | 0.678× | 2.68% | PASS |
| file_reads | `select_random_ranges` | 7.84ms | 4.53ms | 0.577× | 1.04% | PASS |
| file_reads | `covering_index_scan` | 8.38ms | 4.88ms | 0.582× | 3.19% | PASS |
| file_reads | `groupby_scan` | 19.59ms | 20.43ms | 1.043× | 1.28% | PASS |
| file_reads | `index_join` | 7.25ms | 6.88ms | 0.950× | 4.04% | PASS |
| file_reads | `index_join_scan` | 3.31ms | 4.09ms | 1.236× | 4.62% | PASS |
| file_reads | `types_table_scan` | 724.16ms | 794.87ms | 1.098× | 2.06% | PASS |
| file_reads | `table_scan` | 860.22ms | 880.25ms | 1.023× | 3.55% | PASS |
| file_reads | `oltp_read_only` | 146.65ms | 97.46ms | 0.665× | 1.15% | PASS |
| file_writes | `oltp_bulk_insert` | 149.86ms | 207.53ms | 1.385× | 2.20% | PASS |
| file_writes | `oltp_insert` | 19.68ms | 27.62ms | 1.403× | 5.89% | PASS |
| file_writes | `oltp_update_index` | 131.06ms | 141.85ms | 1.082× | 4.38% | PASS |
| file_writes | `oltp_update_non_index` | 105.76ms | 99.14ms | 0.937× | 4.45% | PASS |
| file_writes | `oltp_delete_insert` | 109.86ms | 109.67ms | 0.998× | 4.73% | PASS |
| file_writes | `oltp_write_only` | 80.85ms | 74.46ms | 0.921× | 11.73% | PASS |
| file_writes | `types_delete_insert` | 63.30ms | 55.05ms | 0.870× | 7.10% | PASS |
| file_writes | `oltp_read_write` | 110.11ms | 106.44ms | 0.967× | 3.69% | PASS |
| ac_reads | `oltp_point_select` | 34.10ms | 36.32ms | 1.065× | 3.81% | PASS |
| ac_reads | `oltp_range_select` | 8.07ms | 8.87ms | 1.098× | 2.54% | PASS |
| ac_reads | `oltp_sum_range` | 8.28ms | 9.26ms | 1.119× | 2.77% | PASS |
| ac_reads | `oltp_order_range` | 2.03ms | 2.06ms | 1.014× | 2.02% | PASS |
| ac_reads | `oltp_distinct_range` | 2.56ms | 2.62ms | 1.023× | 1.63% | PASS |
| ac_reads | `oltp_index_scan` | 5.21ms | 5.79ms | 1.111× | 3.10% | PASS |
| ac_reads | `select_random_points` | 13.16ms | 11.66ms | 0.886× | 4.01% | PASS |
| ac_reads | `select_random_ranges` | 4.73ms | 5.06ms | 1.070× | 1.41% | PASS |
| ac_reads | `covering_index_scan` | 5.58ms | 5.67ms | 1.016× | 2.07% | PASS |
| ac_reads | `groupby_scan` | 21.89ms | 22.99ms | 1.050× | 1.42% | PASS |
| ac_reads | `index_join` | 5.86ms | 7.80ms | 1.331× | 4.94% | PASS |
| ac_reads | `index_join_scan` | 3.13ms | 4.37ms | 1.393× | 5.32% | PASS |
| ac_reads | `types_table_scan` | 748.63ms | 811.38ms | 1.084× | 1.71% | PASS |
| ac_reads | `table_scan` | 820.18ms | 876.67ms | 1.069× | 2.99% | PASS |
| ac_reads | `oltp_read_only` | 89.04ms | 94.03ms | 1.056× | 1.70% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.94ms | 61.56ms | 2.683× | 4.17% | PASS |
| ac_writes | `oltp_insert_ac` | 24.81ms | 74.89ms | 3.018× | 3.14% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.15ms | 87.59ms | 3.349× | 2.46% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.71ms | 68.32ms | 3.008× | 3.56% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.91ms | 77.55ms | 3.243× | 3.86% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.17ms | 73.59ms | 3.177× | 4.15% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.77ms | 69.17ms | 3.331× | 4.17% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.25ms | 78.51ms | 2.991× | 4.63% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.68ms | 34.51ms | 1.163× | 1.50% | PASS |
| mem_reads | `oltp_range_select` | 14.18ms | 13.66ms | 0.963× | 1.16% | PASS |
| mem_reads | `oltp_sum_range` | 12.32ms | 13.39ms | 1.087× | 1.40% | PASS |
| mem_reads | `oltp_order_range` | 3.18ms | 3.24ms | 1.019× | 1.43% | PASS |
| mem_reads | `oltp_distinct_range` | 4.20ms | 4.24ms | 1.008× | 0.77% | PASS |
| mem_reads | `oltp_index_scan` | 4.53ms | 5.71ms | 1.260× | 1.47% | PASS |
| mem_reads | `select_random_points` | 17.59ms | 19.85ms | 1.128× | 1.53% | PASS |
| mem_reads | `select_random_ranges` | 4.12ms | 5.20ms | 1.260× | 1.50% | PASS |
| mem_reads | `covering_index_scan` | 4.85ms | 4.41ms | 0.910× | 2.57% | PASS |
| mem_reads | `groupby_scan` | 34.78ms | 35.02ms | 1.007× | 0.57% | PASS |
| mem_reads | `index_join` | 7.02ms | 8.79ms | 1.251× | 1.63% | PASS |
| mem_reads | `index_join_scan` | 4.88ms | 5.65ms | 1.159× | 2.22% | PASS |
| mem_reads | `types_table_scan` | 1.14s | 1.26s | 1.106× | 0.48% | PASS |
| mem_reads | `table_scan` | 1.34s | 1.37s | 1.022× | 0.98% | PASS |
| mem_reads | `oltp_read_only` | 121.27ms | 130.72ms | 1.078× | 1.20% | PASS |
| mem_writes | `oltp_bulk_insert` | 228.71ms | 348.03ms | 1.522× | 0.56% | PASS |
| mem_writes | `oltp_insert` | 22.25ms | 41.76ms | 1.877× | 0.67% | PASS |
| mem_writes | `oltp_update_index` | 73.43ms | 144.52ms | 1.968× | 0.94% | PASS |
| mem_writes | `oltp_update_non_index` | 50.84ms | 94.63ms | 1.861× | 0.91% | PASS |
| mem_writes | `oltp_delete_insert` | 53.14ms | 112.44ms | 2.116× | 0.82% | PASS |
| mem_writes | `oltp_write_only` | 30.54ms | 69.37ms | 2.271× | 0.96% | PASS |
| mem_writes | `types_delete_insert` | 33.74ms | 60.21ms | 1.784× | 0.76% | PASS |
| mem_writes | `oltp_read_write` | 87.27ms | 141.71ms | 1.624× | 1.80% | PASS |
| file_reads | `oltp_point_select` | 118.61ms | 64.20ms | 0.541× | 0.64% | PASS |
| file_reads | `oltp_range_select` | 23.12ms | 16.88ms | 0.730× | 2.15% | PASS |
| file_reads | `oltp_sum_range` | 21.12ms | 16.70ms | 0.791× | 1.99% | PASS |
| file_reads | `oltp_order_range` | 4.00ms | 3.61ms | 0.903× | 1.53% | PASS |
| file_reads | `oltp_distinct_range` | 5.26ms | 4.63ms | 0.880× | 2.71% | PASS |
| file_reads | `oltp_index_scan` | 13.45ms | 9.23ms | 0.686× | 1.40% | PASS |
| file_reads | `select_random_points` | 26.42ms | 23.16ms | 0.877× | 1.93% | PASS |
| file_reads | `select_random_ranges` | 12.76ms | 8.32ms | 0.652× | 0.86% | PASS |
| file_reads | `covering_index_scan` | 14.24ms | 7.85ms | 0.551× | 2.25% | PASS |
| file_reads | `groupby_scan` | 35.49ms | 35.73ms | 1.007× | 0.96% | PASS |
| file_reads | `index_join` | 11.99ms | 11.39ms | 0.950× | 1.52% | PASS |
| file_reads | `index_join_scan` | 6.00ms | 6.30ms | 1.051× | 2.61% | PASS |
| file_reads | `types_table_scan` | 1.14s | 1.27s | 1.112× | 0.74% | PASS |
| file_reads | `table_scan` | 1.34s | 1.37s | 1.026× | 0.78% | PASS |
| file_reads | `oltp_read_only` | 250.00ms | 173.71ms | 0.695× | 0.85% | PASS |
| file_writes | `oltp_bulk_insert` | 248.91ms | 382.70ms | 1.538× | 0.52% | PASS |
| file_writes | `oltp_insert` | 66.11ms | 55.66ms | 0.842× | 18.23% | PASS |
| file_writes | `oltp_update_index` | 119.43ms | 184.64ms | 1.546× | 1.27% | PASS |
| file_writes | `oltp_update_non_index` | 106.58ms | 124.36ms | 1.167× | 16.33% | PASS |
| file_writes | `oltp_delete_insert` | 93.32ms | 146.02ms | 1.565× | 0.84% | PASS |
| file_writes | `oltp_write_only` | 90.18ms | 95.56ms | 1.060× | 5.77% | PASS |
| file_writes | `types_delete_insert` | 56.84ms | 79.65ms | 1.401× | 1.67% | PASS |
| file_writes | `oltp_read_write` | 136.65ms | 166.97ms | 1.222× | 4.18% | PASS |
| ac_reads | `oltp_point_select` | 56.86ms | 64.23ms | 1.130× | 0.92% | PASS |
| ac_reads | `oltp_range_select` | 18.32ms | 16.90ms | 0.923× | 1.38% | PASS |
| ac_reads | `oltp_sum_range` | 15.88ms | 16.76ms | 1.055× | 1.11% | PASS |
| ac_reads | `oltp_order_range` | 3.71ms | 3.64ms | 0.981× | 1.60% | PASS |
| ac_reads | `oltp_distinct_range` | 4.71ms | 4.64ms | 0.985× | 1.26% | PASS |
| ac_reads | `oltp_index_scan` | 7.94ms | 9.25ms | 1.165× | 0.98% | PASS |
| ac_reads | `select_random_points` | 21.47ms | 23.43ms | 1.091× | 1.37% | PASS |
| ac_reads | `select_random_ranges` | 7.16ms | 8.31ms | 1.161× | 0.93% | PASS |
| ac_reads | `covering_index_scan` | 8.95ms | 7.84ms | 0.876× | 1.77% | PASS |
| ac_reads | `groupby_scan` | 35.42ms | 35.68ms | 1.007× | 0.72% | PASS |
| ac_reads | `index_join` | 9.61ms | 11.41ms | 1.188× | 1.42% | PASS |
| ac_reads | `index_join_scan` | 5.53ms | 6.29ms | 1.136× | 1.50% | PASS |
| ac_reads | `types_table_scan` | 1.14s | 1.27s | 1.108× | 0.72% | PASS |
| ac_reads | `table_scan` | 1.36s | 1.37s | 1.008× | 1.16% | PASS |
| ac_reads | `oltp_read_only` | 163.38ms | 174.02ms | 1.065× | 0.91% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.43ms | 53.29ms | 3.243× | 2.53% | PASS |
| ac_writes | `oltp_insert_ac` | 19.76ms | 71.46ms | 3.616× | 3.38% | PASS |
| ac_writes | `oltp_update_index_ac` | 22.67ms | 89.17ms | 3.933× | 4.89% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.62ms | 67.09ms | 4.038× | 5.31% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 19.42ms | 79.22ms | 4.079× | 4.58% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.16ms | 78.02ms | 4.072× | 3.31% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.89ms | 66.67ms | 4.196× | 5.14% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.69ms | 86.32ms | 3.360× | 3.93% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.37ms | 24.17ms | 1.186× | 0.85% | PASS |
| mem_reads | `oltp_range_select` | 9.86ms | 9.51ms | 0.964× | 2.93% | PASS |
| mem_reads | `oltp_sum_range` | 9.65ms | 9.18ms | 0.952× | 3.15% | PASS |
| mem_reads | `oltp_order_range` | 2.15ms | 2.21ms | 1.027× | 2.02% | PASS |
| mem_reads | `oltp_distinct_range` | 2.84ms | 2.90ms | 1.022× | 1.25% | PASS |
| mem_reads | `oltp_index_scan` | 3.30ms | 4.39ms | 1.332× | 1.89% | PASS |
| mem_reads | `select_random_points` | 14.06ms | 16.45ms | 1.170× | 0.93% | PASS |
| mem_reads | `select_random_ranges` | 2.91ms | 3.72ms | 1.281× | 1.38% | PASS |
| mem_reads | `covering_index_scan` | 2.80ms | 3.12ms | 1.114× | 1.44% | PASS |
| mem_reads | `groupby_scan` | 23.15ms | 24.50ms | 1.059× | 0.48% | PASS |
| mem_reads | `index_join` | 5.32ms | 7.03ms | 1.321× | 1.07% | PASS |
| mem_reads | `index_join_scan` | 3.38ms | 4.85ms | 1.433× | 2.25% | PASS |
| mem_reads | `types_table_scan` | 807.41ms | 918.10ms | 1.137× | 2.78% | PASS |
| mem_reads | `table_scan` | 1.12s | 1.01s | 0.905× | 9.36% | PASS |
| mem_reads | `oltp_read_only` | 87.09ms | 94.65ms | 1.087× | 1.33% | PASS |
| mem_writes | `oltp_bulk_insert` | 136.61ms | 206.41ms | 1.511× | 0.56% | PASS |
| mem_writes | `oltp_insert` | 12.95ms | 26.18ms | 2.021× | 0.58% | PASS |
| mem_writes | `oltp_update_index` | 48.58ms | 97.37ms | 2.004× | 1.57% | PASS |
| mem_writes | `oltp_update_non_index` | 30.88ms | 55.98ms | 1.813× | 1.90% | PASS |
| mem_writes | `oltp_delete_insert` | 32.83ms | 68.63ms | 2.091× | 0.76% | PASS |
| mem_writes | `oltp_write_only` | 19.42ms | 43.91ms | 2.261× | 1.15% | PASS |
| mem_writes | `types_delete_insert` | 21.80ms | 38.78ms | 1.778× | 1.26% | PASS |
| mem_writes | `oltp_read_write` | 63.08ms | 101.04ms | 1.602× | 1.17% | PASS |
| file_reads | `oltp_point_select` | 44.00ms | 32.77ms | 0.745× | 0.85% | PASS |
| file_reads | `oltp_range_select` | 13.79ms | 11.07ms | 0.803× | 1.34% | PASS |
| file_reads | `oltp_sum_range` | 13.91ms | 10.88ms | 0.783× | 1.14% | PASS |
| file_reads | `oltp_order_range` | 2.61ms | 2.44ms | 0.934× | 2.59% | PASS |
| file_reads | `oltp_distinct_range` | 3.17ms | 3.09ms | 0.974× | 2.09% | PASS |
| file_reads | `oltp_index_scan` | 5.57ms | 5.24ms | 0.940× | 1.63% | PASS |
| file_reads | `select_random_points` | 16.47ms | 17.54ms | 1.065× | 1.60% | PASS |
| file_reads | `select_random_ranges` | 5.08ms | 4.35ms | 0.856× | 1.92% | PASS |
| file_reads | `covering_index_scan` | 5.02ms | 3.83ms | 0.764× | 1.46% | PASS |
| file_reads | `groupby_scan` | 23.19ms | 24.71ms | 1.066× | 0.56% | PASS |
| file_reads | `index_join` | 6.63ms | 7.96ms | 1.201× | 3.09% | PASS |
| file_reads | `index_join_scan` | 3.73ms | 5.20ms | 1.394× | 3.47% | PASS |
| file_reads | `types_table_scan` | 808.51ms | 899.36ms | 1.112× | 1.02% | PASS |
| file_reads | `table_scan` | 902.33ms | 972.13ms | 1.077× | 1.22% | PASS |
| file_reads | `oltp_read_only` | 118.30ms | 103.41ms | 0.874× | 1.13% | PASS |
| file_writes | `oltp_bulk_insert` | 147.34ms | 221.41ms | 1.503× | 0.99% | PASS |
| file_writes | `oltp_insert` | 18.23ms | 32.19ms | 1.766× | 1.84% | PASS |
| file_writes | `oltp_update_index` | 62.90ms | 107.92ms | 1.716× | 1.97% | PASS |
| file_writes | `oltp_update_non_index` | 44.88ms | 69.63ms | 1.552× | 1.97% | PASS |
| file_writes | `oltp_delete_insert` | 49.36ms | 87.90ms | 1.781× | 1.34% | PASS |
| file_writes | `oltp_write_only` | 31.20ms | 55.16ms | 1.768× | 1.74% | PASS |
| file_writes | `types_delete_insert` | 31.58ms | 47.64ms | 1.509× | 2.53% | PASS |
| file_writes | `oltp_read_write` | 75.70ms | 112.75ms | 1.489× | 2.16% | PASS |
| ac_reads | `oltp_point_select` | 28.00ms | 31.80ms | 1.136× | 1.01% | PASS |
| ac_reads | `oltp_range_select` | 11.65ms | 10.83ms | 0.930× | 0.77% | PASS |
| ac_reads | `oltp_sum_range` | 11.87ms | 10.68ms | 0.900× | 1.43% | PASS |
| ac_reads | `oltp_order_range` | 2.46ms | 2.41ms | 0.979× | 1.92% | PASS |
| ac_reads | `oltp_distinct_range` | 3.05ms | 3.07ms | 1.008× | 1.75% | PASS |
| ac_reads | `oltp_index_scan` | 4.38ms | 5.39ms | 1.231× | 1.71% | PASS |
| ac_reads | `select_random_points` | 15.94ms | 18.19ms | 1.141× | 1.60% | PASS |
| ac_reads | `select_random_ranges` | 3.84ms | 4.41ms | 1.148× | 1.67% | PASS |
| ac_reads | `covering_index_scan` | 3.67ms | 3.92ms | 1.068× | 1.54% | PASS |
| ac_reads | `groupby_scan` | 23.35ms | 24.87ms | 1.065× | 0.62% | PASS |
| ac_reads | `index_join` | 5.96ms | 7.82ms | 1.312× | 1.91% | PASS |
| ac_reads | `index_join_scan` | 3.73ms | 4.95ms | 1.330× | 1.91% | PASS |
| ac_reads | `types_table_scan` | 803.70ms | 897.02ms | 1.116× | 1.16% | PASS |
| ac_reads | `table_scan` | 952.39ms | 979.20ms | 1.028× | 5.29% | PASS |
| ac_reads | `oltp_read_only` | 92.72ms | 101.83ms | 1.098× | 1.24% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 8.87ms | 30.13ms | 3.396× | 4.95% | PASS |
| ac_writes | `oltp_insert_ac` | 10.05ms | 44.11ms | 4.389× | 4.25% | PASS |
| ac_writes | `oltp_update_index_ac` | 10.88ms | 50.60ms | 4.649× | 4.43% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 9.02ms | 37.12ms | 4.114× | 5.24% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 11.49ms | 48.13ms | 4.188× | 8.08% | PASS |
| ac_writes | `oltp_write_only_ac` | 10.22ms | 45.77ms | 4.480× | 6.82% | PASS |
| ac_writes | `types_delete_insert_ac` | 8.78ms | 38.75ms | 4.416× | 5.81% | PASS |
| ac_writes | `oltp_read_write_ac` | 13.79ms | 54.36ms | 3.943× | 8.82% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.31ms | 30.36ms | 1.249× | 15.66% | PASS |
| mem_reads | `oltp_range_select` | 13.99ms | 14.45ms | 1.033× | 27.67% | PASS |
| mem_reads | `oltp_sum_range` | 14.59ms | 15.52ms | 1.064× | 22.51% | PASS |
| mem_reads | `oltp_order_range` | 2.58ms | 2.35ms | 0.910× | 25.69% | PASS |
| mem_reads | `oltp_distinct_range` | 3.88ms | 3.40ms | 0.876× | 39.32% | PASS |
| mem_reads | `oltp_index_scan` | 3.95ms | 5.15ms | 1.304× | 35.39% | PASS |
| mem_reads | `select_random_points` | 26.84ms | 28.07ms | 1.046× | 18.95% | PASS |
| mem_reads | `select_random_ranges` | 6.48ms | 6.65ms | 1.026× | 25.21% | PASS |
| mem_reads | `covering_index_scan` | 2.88ms | 3.70ms | 1.285× | 28.40% | PASS |
| mem_reads | `groupby_scan` | 31.92ms | 31.96ms | 1.001× | 16.95% | PASS |
| mem_reads | `index_join` | 6.82ms | 8.98ms | 1.317× | 18.99% | PASS |
| mem_reads | `index_join_scan` | 3.68ms | 5.60ms | 1.523× | 28.75% | PASS |
| mem_reads | `types_table_scan` | 876.06ms | 973.16ms | 1.111× | 1.51% | PASS |
| mem_reads | `table_scan` | 1.03s | 1.11s | 1.077× | 1.88% | PASS |
| mem_reads | `oltp_read_only` | 109.30ms | 118.88ms | 1.088× | 4.47% | PASS |
| mem_writes | `oltp_bulk_insert` | 206.48ms | 291.10ms | 1.410× | 3.37% | PASS |
| mem_writes | `oltp_insert` | 15.81ms | 32.33ms | 2.045× | 12.19% | PASS |
| mem_writes | `oltp_update_index` | 60.99ms | 116.49ms | 1.910× | 6.27% | PASS |
| mem_writes | `oltp_update_non_index` | 44.11ms | 77.74ms | 1.762× | 6.11% | PASS |
| mem_writes | `oltp_delete_insert` | 47.44ms | 100.67ms | 2.122× | 6.89% | PASS |
| mem_writes | `oltp_write_only` | 27.37ms | 63.17ms | 2.308× | 16.38% | PASS |
| mem_writes | `types_delete_insert` | 29.63ms | 54.18ms | 1.829× | 16.92% | PASS |
| mem_writes | `oltp_read_write` | 84.06ms | 132.77ms | 1.579× | 6.46% | PASS |
| file_reads | `oltp_point_select` | 94.03ms | 55.19ms | 0.587× | 6.20% | PASS |
| file_reads | `oltp_range_select` | 23.45ms | 19.35ms | 0.825× | 12.85% | PASS |
| file_reads | `oltp_sum_range` | 23.25ms | 19.84ms | 0.853× | 12.41% | PASS |
| file_reads | `oltp_order_range` | 4.40ms | 3.91ms | 0.889× | 27.71% | PASS |
| file_reads | `oltp_distinct_range` | 5.41ms | 4.76ms | 0.879× | 24.34% | PASS |
| file_reads | `oltp_index_scan` | 13.16ms | 8.98ms | 0.682× | 17.28% | PASS |
| file_reads | `select_random_points` | 34.28ms | 31.09ms | 0.907× | 13.82% | PASS |
| file_reads | `select_random_ranges` | 14.12ms | 9.94ms | 0.704× | 24.71% | PASS |
| file_reads | `covering_index_scan` | 9.61ms | 7.12ms | 0.741× | 22.64% | PASS |
| file_reads | `groupby_scan` | 33.46ms | 33.35ms | 0.997× | 13.99% | PASS |
| file_reads | `index_join` | 11.79ms | 11.61ms | 0.984× | 17.10% | PASS |
| file_reads | `index_join_scan` | 5.56ms | 6.43ms | 1.157× | 26.83% | PASS |
| file_reads | `types_table_scan` | 896.04ms | 988.12ms | 1.103× | 1.43% | PASS |
| file_reads | `table_scan` | 1.04s | 1.12s | 1.081× | 3.49% | PASS |
| file_reads | `oltp_read_only` | 250.31ms | 184.24ms | 0.736× | 3.64% | PASS |
| file_writes | `oltp_bulk_insert` | 277.55ms | 386.77ms | 1.393× | 3.58% | PASS |
| file_writes | `oltp_insert` | 29.43ms | 57.13ms | 1.941× | 19.05% | PASS |
| file_writes | `oltp_update_index` | 172.40ms | 206.72ms | 1.199× | 5.55% | PASS |
| file_writes | `oltp_update_non_index` | 150.07ms | 155.69ms | 1.037× | 7.64% | PASS |
| file_writes | `oltp_delete_insert` | 153.41ms | 178.43ms | 1.163× | 7.39% | PASS |
| file_writes | `oltp_write_only` | 109.15ms | 126.58ms | 1.160× | 7.24% | PASS |
| file_writes | `types_delete_insert` | 88.74ms | 97.97ms | 1.104× | 11.42% | PASS |
| file_writes | `oltp_read_write` | 159.64ms | 192.51ms | 1.206× | 6.06% | PASS |
| ac_reads | `oltp_point_select` | 51.29ms | 57.85ms | 1.128× | 8.65% | PASS |
| ac_reads | `oltp_range_select` | 20.04ms | 20.06ms | 1.001× | 21.91% | PASS |
| ac_reads | `oltp_sum_range` | 20.21ms | 19.66ms | 0.973× | 26.15% | PASS |
| ac_reads | `oltp_order_range` | 3.99ms | 3.82ms | 0.956× | 31.68% | PASS |
| ac_reads | `oltp_distinct_range` | 4.86ms | 4.59ms | 0.945× | 29.11% | PASS |
| ac_reads | `oltp_index_scan` | 8.30ms | 9.23ms | 1.112× | 17.60% | PASS |
| ac_reads | `select_random_points` | 28.75ms | 29.81ms | 1.037× | 11.30% | PASS |
| ac_reads | `select_random_ranges` | 10.27ms | 11.10ms | 1.081× | 22.69% | PASS |
| ac_reads | `covering_index_scan` | 6.17ms | 6.64ms | 1.076× | 19.10% | PASS |
| ac_reads | `groupby_scan` | 32.50ms | 31.50ms | 0.969× | 11.75% | PASS |
| ac_reads | `index_join` | 8.92ms | 11.24ms | 1.260× | 18.30% | PASS |
| ac_reads | `index_join_scan` | 4.69ms | 7.09ms | 1.511× | 25.42% | PASS |
| ac_reads | `types_table_scan` | 910.27ms | 1.00s | 1.104× | 1.40% | PASS |
| ac_reads | `table_scan` | 1.03s | 1.10s | 1.064× | 2.30% | PASS |
| ac_reads | `oltp_read_only` | 149.09ms | 156.50ms | 1.050× | 4.56% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 37.58ms | 96.94ms | 2.580× | 14.14% | PASS |
| ac_writes | `oltp_insert_ac` | 38.47ms | 122.97ms | 3.197× | 10.54% | PASS |
| ac_writes | `oltp_update_index_ac` | 41.61ms | 129.77ms | 3.119× | 9.85% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 38.68ms | 114.61ms | 2.963× | 15.16% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 35.58ms | 118.12ms | 3.320× | 13.27% | PASS |
| ac_writes | `oltp_write_only_ac` | 40.12ms | 126.56ms | 3.155× | 11.56% | PASS |
| ac_writes | `types_delete_insert_ac` | 39.15ms | 112.45ms | 2.872× | 15.99% | PASS |
| ac_writes | `oltp_read_write_ac` | 43.98ms | 135.77ms | 3.087× | 16.67% | PASS |

</details>

## Version-control latency

Wall time: 2m 10s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 80.16ms | 200.00ms | 40.1% | 0.40% | PASS |
| `status_dirty_many_tables` | 83.46ms | 200.00ms | 41.7% | 0.52% | PASS |
| `diff_regular_working_one_table` | 76.20ms | 150.00ms | 50.8% | 0.57% | PASS |
| `diff_regular_working_many_tables` | 89.01ms | 200.00ms | 44.5% | 0.44% | PASS |
| `diff_stat_working_many_tables` | 88.70ms | 200.00ms | 44.4% | 0.45% | PASS |
| `diff_schema_working_many_tables` | 89.29ms | 200.00ms | 44.6% | 0.45% | PASS |
| `branch_list_many_branches` | 19.62ms | 100.00ms | 19.6% | 1.70% | PASS |
| `branch_create_delete` | 21.85ms | 100.00ms | 21.9% | 1.97% | PASS |
| `checkout_branch_clean` | 52.68ms | 200.00ms | 26.3% | 0.82% | PASS |
| `merge_data_no_conflicts` | 26.21ms | 150.00ms | 17.5% | 1.75% | PASS |
| `merge_schema_no_conflicts` | 19.33ms | 100.00ms | 19.3% | 2.61% | PASS |
| `merge_data_conflicts` | 111.38ms | 250.00ms | 44.6% | 0.32% | PASS |
| `merge_data_conflicts_with_resolve` | 111.44ms | 250.00ms | 44.6% | 0.27% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
