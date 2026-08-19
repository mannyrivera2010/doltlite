# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-19 11:36 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260810.271.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/32240430850)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 11m 24s | 8.22s | 11.67s | 1.419× | 2.71% | **PASS** |
| textpk | 69 | 55 | 1h 32m 26s | 9.89s | 11.85s | 1.198× | 1.92% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 20s | 9.75s | 11.64s | 1.193× | 1.37% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 4s | 9.77s | 12.30s | 1.259× | 1.40% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 15.06ms | 16.70ms | 1.109× | 1.47% | PASS |
| mem_reads | `oltp_range_select` | 7.16ms | 7.93ms | 1.108× | 4.79% | PASS |
| mem_reads | `oltp_sum_range` | 7.09ms | 7.69ms | 1.084× | 4.34% | PASS |
| mem_reads | `oltp_order_range` | 1.85ms | 1.97ms | 1.063× | 2.49% | PASS |
| mem_reads | `oltp_distinct_range` | 2.46ms | 2.59ms | 1.049× | 1.64% | PASS |
| mem_reads | `oltp_index_scan` | 2.68ms | 3.25ms | 1.212× | 2.99% | PASS |
| mem_reads | `select_random_points` | 7.91ms | 8.25ms | 1.043× | 5.24% | PASS |
| mem_reads | `select_random_ranges` | 2.03ms | 2.42ms | 1.191× | 3.37% | PASS |
| mem_reads | `covering_index_scan` | 2.46ms | 2.42ms | 0.988× | 1.07% | PASS |
| mem_reads | `groupby_scan` | 19.35ms | 21.32ms | 1.102× | 0.73% | PASS |
| mem_reads | `index_join` | 4.10ms | 5.47ms | 1.336× | 1.74% | PASS |
| mem_reads | `index_join_scan` | 2.23ms | 3.53ms | 1.579× | 5.15% | PASS |
| mem_reads | `types_table_scan` | 746.95ms | 864.46ms | 1.157× | 1.08% | PASS |
| mem_reads | `table_scan` | 869.31ms | 956.24ms | 1.100× | 0.99% | PASS |
| mem_reads | `oltp_read_only` | 67.59ms | 76.53ms | 1.132× | 1.97% | PASS |
| mem_writes | `oltp_bulk_insert` | 104.69ms | 140.54ms | 1.343× | 0.91% | PASS |
| mem_writes | `oltp_insert` | 9.64ms | 17.22ms | 1.786× | 1.98% | PASS |
| mem_writes | `oltp_update_index` | 34.48ms | 70.30ms | 2.039× | 2.71% | PASS |
| mem_writes | `oltp_update_non_index` | 23.93ms | 41.04ms | 1.715× | 2.58% | PASS |
| mem_writes | `oltp_delete_insert` | 33.41ms | 55.64ms | 1.665× | 1.77% | PASS |
| mem_writes | `oltp_write_only` | 16.65ms | 31.96ms | 1.919× | 1.81% | PASS |
| mem_writes | `types_delete_insert` | 17.56ms | 27.55ms | 1.569× | 2.06% | PASS |
| mem_writes | `oltp_read_write` | 44.49ms | 70.73ms | 1.590× | 2.58% | PASS |
| file_reads | `oltp_point_select` | 40.07ms | 25.35ms | 0.633× | 2.35% | PASS |
| file_reads | `oltp_range_select` | 10.76ms | 9.74ms | 0.906× | 2.98% | PASS |
| file_reads | `oltp_sum_range` | 9.78ms | 9.32ms | 0.953× | 3.89% | PASS |
| file_reads | `oltp_order_range` | 2.20ms | 2.21ms | 1.003× | 2.40% | PASS |
| file_reads | `oltp_distinct_range` | 2.89ms | 2.83ms | 0.980× | 2.87% | PASS |
| file_reads | `oltp_index_scan` | 5.22ms | 4.40ms | 0.843× | 3.35% | PASS |
| file_reads | `select_random_points` | 11.04ms | 9.89ms | 0.895× | 3.44% | PASS |
| file_reads | `select_random_ranges` | 4.43ms | 3.34ms | 0.755× | 3.02% | PASS |
| file_reads | `covering_index_scan` | 4.96ms | 3.51ms | 0.707× | 3.47% | PASS |
| file_reads | `groupby_scan` | 20.75ms | 22.43ms | 1.081× | 1.17% | PASS |
| file_reads | `index_join` | 5.51ms | 6.41ms | 1.162× | 5.04% | PASS |
| file_reads | `index_join_scan` | 2.64ms | 3.71ms | 1.406× | 5.28% | PASS |
| file_reads | `types_table_scan` | 731.64ms | 843.78ms | 1.153× | 0.99% | PASS |
| file_reads | `table_scan` | 838.36ms | 916.94ms | 1.094× | 1.05% | PASS |
| file_reads | `oltp_read_only` | 96.30ms | 85.35ms | 0.886× | 1.44% | PASS |
| file_writes | `oltp_bulk_insert` | 241.86ms | 238.17ms | 0.985× | 27.83% | PASS |
| file_writes | `oltp_insert` | 53.11ms | 67.30ms | 1.267× | 79.27% | PASS |
| file_writes | `oltp_update_index` | 158.31ms | 148.05ms | 0.935× | 23.41% | PASS |
| file_writes | `oltp_update_non_index` | 95.97ms | 79.56ms | 0.829× | 20.66% | PASS |
| file_writes | `oltp_delete_insert` | 114.13ms | 107.83ms | 0.945× | 28.68% | PASS |
| file_writes | `oltp_write_only` | 189.40ms | 95.84ms | 0.506× | 43.32% | PASS |
| file_writes | `types_delete_insert` | 74.39ms | 85.86ms | 1.154× | 55.48% | PASS |
| file_writes | `oltp_read_write` | 166.14ms | 138.15ms | 0.831× | 40.54% | PASS |
| ac_reads | `oltp_point_select` | 21.97ms | 24.36ms | 1.109× | 0.97% | PASS |
| ac_reads | `oltp_range_select` | 8.80ms | 9.18ms | 1.043× | 1.50% | PASS |
| ac_reads | `oltp_sum_range` | 8.29ms | 8.94ms | 1.079× | 1.70% | PASS |
| ac_reads | `oltp_order_range` | 2.00ms | 2.15ms | 1.078× | 2.60% | PASS |
| ac_reads | `oltp_distinct_range` | 2.70ms | 2.76ms | 1.022× | 1.90% | PASS |
| ac_reads | `oltp_index_scan` | 3.81ms | 4.56ms | 1.198× | 2.64% | PASS |
| ac_reads | `select_random_points` | 9.75ms | 10.07ms | 1.033× | 2.23% | PASS |
| ac_reads | `select_random_ranges` | 2.87ms | 3.25ms | 1.132× | 2.46% | PASS |
| ac_reads | `covering_index_scan` | 3.40ms | 3.39ms | 0.997× | 3.39% | PASS |
| ac_reads | `groupby_scan` | 19.61ms | 21.58ms | 1.100× | 0.62% | PASS |
| ac_reads | `index_join` | 4.92ms | 6.67ms | 1.354× | 4.35% | PASS |
| ac_reads | `index_join_scan` | 2.63ms | 3.83ms | 1.456× | 2.80% | PASS |
| ac_reads | `types_table_scan` | 732.45ms | 845.02ms | 1.154× | 0.60% | PASS |
| ac_reads | `table_scan` | 837.79ms | 917.86ms | 1.096× | 0.94% | PASS |
| ac_reads | `oltp_read_only` | 75.60ms | 84.96ms | 1.124× | 1.30% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 63.98ms | 209.26ms | 3.271× | 41.74% | PASS |
| ac_writes | `oltp_insert_ac` | 260.30ms | 502.95ms | 1.932× | 42.07% | PASS |
| ac_writes | `oltp_update_index_ac` | 214.90ms | 640.36ms | 2.980× | 41.58% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 199.17ms | 503.08ms | 2.526× | 63.35% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 254.84ms | 805.31ms | 3.160× | 71.73% | PASS |
| ac_writes | `oltp_write_only_ac` | 244.24ms | 621.78ms | 2.546× | 53.73% | PASS |
| ac_writes | `types_delete_insert_ac` | 95.68ms | 312.02ms | 3.261× | 40.49% | PASS |
| ac_writes | `oltp_read_write_ac` | 227.36ms | 779.01ms | 3.426× | 55.26% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.23ms | 38.16ms | 1.222× | 3.49% | PASS |
| mem_reads | `oltp_range_select` | 14.40ms | 14.43ms | 1.002× | 5.32% | PASS |
| mem_reads | `oltp_sum_range` | 12.48ms | 14.27ms | 1.143× | 4.11% | PASS |
| mem_reads | `oltp_order_range` | 2.93ms | 3.18ms | 1.086× | 2.77% | PASS |
| mem_reads | `oltp_distinct_range` | 4.00ms | 4.26ms | 1.063× | 1.92% | PASS |
| mem_reads | `oltp_index_scan` | 4.55ms | 6.45ms | 1.418× | 3.27% | PASS |
| mem_reads | `select_random_points` | 19.07ms | 21.25ms | 1.114× | 3.51% | PASS |
| mem_reads | `select_random_ranges` | 4.20ms | 5.26ms | 1.252× | 1.85% | PASS |
| mem_reads | `covering_index_scan` | 4.99ms | 4.84ms | 0.970× | 6.67% | PASS |
| mem_reads | `groupby_scan` | 32.57ms | 34.13ms | 1.048× | 1.32% | PASS |
| mem_reads | `index_join` | 7.04ms | 9.22ms | 1.310× | 3.60% | PASS |
| mem_reads | `index_join_scan` | 4.68ms | 5.48ms | 1.172× | 3.19% | PASS |
| mem_reads | `types_table_scan` | 1.17s | 1.27s | 1.086× | 5.64% | PASS |
| mem_reads | `table_scan` | 1.47s | 1.42s | 0.964× | 3.64% | PASS |
| mem_reads | `oltp_read_only` | 121.27ms | 137.72ms | 1.136× | 3.22% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.82ms | 358.22ms | 1.519× | 1.05% | PASS |
| mem_writes | `oltp_insert` | 21.59ms | 40.03ms | 1.854× | 1.03% | PASS |
| mem_writes | `oltp_update_index` | 71.60ms | 135.71ms | 1.895× | 1.55% | PASS |
| mem_writes | `oltp_update_non_index` | 48.31ms | 86.18ms | 1.784× | 1.34% | PASS |
| mem_writes | `oltp_delete_insert` | 50.83ms | 103.86ms | 2.043× | 1.65% | PASS |
| mem_writes | `oltp_write_only` | 29.67ms | 63.62ms | 2.145× | 0.96% | PASS |
| mem_writes | `types_delete_insert` | 33.42ms | 55.63ms | 1.665× | 2.01% | PASS |
| mem_writes | `oltp_read_write` | 86.42ms | 141.49ms | 1.637× | 1.45% | PASS |
| file_reads | `oltp_point_select` | 105.86ms | 63.94ms | 0.604× | 0.81% | PASS |
| file_reads | `oltp_range_select` | 22.05ms | 17.13ms | 0.777× | 3.76% | PASS |
| file_reads | `oltp_sum_range` | 20.86ms | 17.29ms | 0.829× | 1.67% | PASS |
| file_reads | `oltp_order_range` | 3.86ms | 3.55ms | 0.919× | 2.97% | PASS |
| file_reads | `oltp_distinct_range` | 5.00ms | 4.62ms | 0.924× | 1.59% | PASS |
| file_reads | `oltp_index_scan` | 12.29ms | 9.23ms | 0.751× | 2.06% | PASS |
| file_reads | `select_random_points` | 27.34ms | 24.54ms | 0.897× | 1.90% | PASS |
| file_reads | `select_random_ranges` | 11.76ms | 7.91ms | 0.673× | 1.29% | PASS |
| file_reads | `covering_index_scan` | 12.37ms | 7.44ms | 0.602× | 1.37% | PASS |
| file_reads | `groupby_scan` | 33.11ms | 34.27ms | 1.035× | 0.86% | PASS |
| file_reads | `index_join` | 11.65ms | 11.43ms | 0.981× | 1.90% | PASS |
| file_reads | `index_join_scan` | 5.76ms | 5.94ms | 1.030× | 3.04% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.23s | 1.156× | 1.62% | PASS |
| file_reads | `table_scan` | 1.21s | 1.37s | 1.133× | 1.31% | PASS |
| file_reads | `oltp_read_only` | 225.72ms | 175.14ms | 0.776× | 0.90% | PASS |
| file_writes | `oltp_bulk_insert` | 254.80ms | 385.41ms | 1.513× | 0.78% | PASS |
| file_writes | `oltp_insert` | 45.30ms | 52.74ms | 1.164× | 12.92% | PASS |
| file_writes | `oltp_update_index` | 112.83ms | 169.76ms | 1.505× | 1.53% | PASS |
| file_writes | `oltp_update_non_index` | 101.05ms | 112.15ms | 1.110× | 15.69% | PASS |
| file_writes | `oltp_delete_insert` | 89.12ms | 133.82ms | 1.502× | 1.26% | PASS |
| file_writes | `oltp_write_only` | 81.84ms | 85.53ms | 1.045× | 13.93% | PASS |
| file_writes | `types_delete_insert` | 54.97ms | 74.51ms | 1.356× | 1.55% | PASS |
| file_writes | `oltp_read_write` | 137.90ms | 162.68ms | 1.180× | 6.51% | PASS |
| ac_reads | `oltp_point_select` | 54.63ms | 63.75ms | 1.167× | 0.99% | PASS |
| ac_reads | `oltp_range_select` | 16.35ms | 16.95ms | 1.037× | 1.80% | PASS |
| ac_reads | `oltp_sum_range` | 15.06ms | 17.08ms | 1.134× | 1.84% | PASS |
| ac_reads | `oltp_order_range` | 3.25ms | 3.49ms | 1.075× | 2.10% | PASS |
| ac_reads | `oltp_distinct_range` | 4.34ms | 4.60ms | 1.062× | 1.79% | PASS |
| ac_reads | `oltp_index_scan` | 7.32ms | 9.22ms | 1.259× | 2.65% | PASS |
| ac_reads | `select_random_points` | 21.19ms | 24.53ms | 1.158× | 1.54% | PASS |
| ac_reads | `select_random_ranges` | 6.59ms | 7.89ms | 1.196× | 1.46% | PASS |
| ac_reads | `covering_index_scan` | 7.84ms | 7.38ms | 0.942× | 4.29% | PASS |
| ac_reads | `groupby_scan` | 32.01ms | 34.20ms | 1.069× | 1.20% | PASS |
| ac_reads | `index_join` | 8.66ms | 11.37ms | 1.313× | 3.05% | PASS |
| ac_reads | `index_join_scan` | 5.24ms | 6.03ms | 1.150× | 2.47% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.22s | 1.168× | 0.37% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.36s | 1.135× | 0.47% | PASS |
| ac_reads | `oltp_read_only` | 152.97ms | 174.73ms | 1.142× | 0.94% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.32ms | 76.14ms | 3.572× | 1.97% | PASS |
| ac_writes | `oltp_insert_ac` | 24.44ms | 92.18ms | 3.772× | 3.91% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.80ms | 109.66ms | 4.092× | 4.02% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.80ms | 89.43ms | 4.102× | 3.32% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.79ms | 101.08ms | 4.248× | 3.17% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.02ms | 99.85ms | 4.158× | 3.05% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.84ms | 89.87ms | 4.314× | 3.32% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.79ms | 106.82ms | 3.586× | 3.22% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.82ms | 29.94ms | 1.116× | 1.20% | PASS |
| mem_reads | `oltp_range_select` | 12.21ms | 12.70ms | 1.040× | 1.01% | PASS |
| mem_reads | `oltp_sum_range` | 11.63ms | 11.93ms | 1.025× | 1.07% | PASS |
| mem_reads | `oltp_order_range` | 2.80ms | 2.94ms | 1.051× | 1.21% | PASS |
| mem_reads | `oltp_distinct_range` | 3.71ms | 3.81ms | 1.028× | 1.38% | PASS |
| mem_reads | `oltp_index_scan` | 4.10ms | 5.18ms | 1.264× | 1.77% | PASS |
| mem_reads | `select_random_points` | 16.65ms | 18.93ms | 1.137× | 1.35% | PASS |
| mem_reads | `select_random_ranges` | 3.44ms | 4.51ms | 1.310× | 1.61% | PASS |
| mem_reads | `covering_index_scan` | 3.65ms | 3.85ms | 1.055× | 1.77% | PASS |
| mem_reads | `groupby_scan` | 31.56ms | 31.88ms | 1.010× | 0.68% | PASS |
| mem_reads | `index_join` | 6.30ms | 8.63ms | 1.370× | 1.65% | PASS |
| mem_reads | `index_join_scan` | 3.74ms | 5.21ms | 1.394× | 1.68% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.21s | 1.141× | 0.54% | PASS |
| mem_reads | `table_scan` | 1.27s | 1.32s | 1.045× | 0.81% | PASS |
| mem_reads | `oltp_read_only` | 108.90ms | 118.72ms | 1.090× | 0.76% | PASS |
| mem_writes | `oltp_bulk_insert` | 191.46ms | 268.74ms | 1.404× | 0.34% | PASS |
| mem_writes | `oltp_insert` | 17.11ms | 32.02ms | 1.871× | 0.91% | PASS |
| mem_writes | `oltp_update_index` | 60.09ms | 115.02ms | 1.914× | 1.39% | PASS |
| mem_writes | `oltp_update_non_index` | 41.47ms | 70.35ms | 1.696× | 1.20% | PASS |
| mem_writes | `oltp_delete_insert` | 42.82ms | 88.63ms | 2.070× | 1.06% | PASS |
| mem_writes | `oltp_write_only` | 23.57ms | 52.12ms | 2.211× | 1.03% | PASS |
| mem_writes | `types_delete_insert` | 27.00ms | 44.92ms | 1.664× | 0.96% | PASS |
| mem_writes | `oltp_read_write` | 71.49ms | 116.22ms | 1.626× | 0.91% | PASS |
| file_reads | `oltp_point_select` | 59.13ms | 41.40ms | 0.700× | 0.49% | PASS |
| file_reads | `oltp_range_select` | 15.96ms | 14.24ms | 0.892× | 0.88% | PASS |
| file_reads | `oltp_sum_range` | 15.35ms | 13.42ms | 0.874× | 1.69% | PASS |
| file_reads | `oltp_order_range` | 3.25ms | 3.13ms | 0.964× | 1.52% | PASS |
| file_reads | `oltp_distinct_range` | 4.21ms | 4.04ms | 0.960× | 1.37% | PASS |
| file_reads | `oltp_index_scan` | 7.79ms | 6.67ms | 0.857× | 1.09% | PASS |
| file_reads | `select_random_points` | 21.03ms | 20.69ms | 0.984× | 1.15% | PASS |
| file_reads | `select_random_ranges` | 7.00ms | 5.78ms | 0.826× | 1.53% | PASS |
| file_reads | `covering_index_scan` | 7.59ms | 5.32ms | 0.701× | 1.56% | PASS |
| file_reads | `groupby_scan` | 32.30ms | 32.09ms | 0.993× | 0.77% | PASS |
| file_reads | `index_join` | 8.69ms | 9.96ms | 1.147× | 0.95% | PASS |
| file_reads | `index_join_scan` | 4.28ms | 5.57ms | 1.302× | 1.71% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.21s | 1.145× | 0.49% | PASS |
| file_reads | `table_scan` | 1.28s | 1.33s | 1.038× | 1.02% | PASS |
| file_reads | `oltp_read_only` | 158.86ms | 136.61ms | 0.860× | 0.73% | PASS |
| file_writes | `oltp_bulk_insert` | 258.60ms | 360.32ms | 1.393× | 4.39% | PASS |
| file_writes | `oltp_insert` | 46.60ms | 65.16ms | 1.398× | 2.50% | PASS |
| file_writes | `oltp_update_index` | 202.02ms | 214.79ms | 1.063× | 2.27% | PASS |
| file_writes | `oltp_update_non_index` | 156.15ms | 143.39ms | 0.918× | 1.64% | PASS |
| file_writes | `oltp_delete_insert` | 165.63ms | 172.62ms | 1.042× | 2.28% | PASS |
| file_writes | `oltp_write_only` | 120.97ms | 120.39ms | 0.995× | 1.00% | PASS |
| file_writes | `types_delete_insert` | 104.40ms | 99.36ms | 0.952× | 4.98% | PASS |
| file_writes | `oltp_read_write` | 171.38ms | 186.28ms | 1.087× | 2.03% | PASS |
| ac_reads | `oltp_point_select` | 37.09ms | 41.54ms | 1.120× | 1.16% | PASS |
| ac_reads | `oltp_range_select` | 13.71ms | 14.29ms | 1.042× | 1.16% | PASS |
| ac_reads | `oltp_sum_range` | 13.19ms | 13.46ms | 1.021× | 1.43% | PASS |
| ac_reads | `oltp_order_range` | 3.02ms | 3.15ms | 1.044× | 2.16% | PASS |
| ac_reads | `oltp_distinct_range` | 3.98ms | 4.07ms | 1.021× | 2.19% | PASS |
| ac_reads | `oltp_index_scan` | 5.64ms | 6.66ms | 1.182× | 1.25% | PASS |
| ac_reads | `select_random_points` | 18.67ms | 20.71ms | 1.109× | 1.20% | PASS |
| ac_reads | `select_random_ranges` | 4.80ms | 5.78ms | 1.205× | 2.03% | PASS |
| ac_reads | `covering_index_scan` | 5.33ms | 5.32ms | 0.999× | 1.37% | PASS |
| ac_reads | `groupby_scan` | 32.01ms | 32.11ms | 1.003× | 0.59% | PASS |
| ac_reads | `index_join` | 7.59ms | 9.89ms | 1.303× | 1.39% | PASS |
| ac_reads | `index_join_scan` | 4.14ms | 5.56ms | 1.343× | 1.86% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.20s | 1.153× | 0.48% | PASS |
| ac_reads | `table_scan` | 1.24s | 1.32s | 1.060× | 0.89% | PASS |
| ac_reads | `oltp_read_only` | 123.28ms | 135.55ms | 1.099× | 1.12% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 33.19ms | 132.72ms | 3.999× | 31.12% | PASS |
| ac_writes | `oltp_insert_ac` | 33.05ms | 135.42ms | 4.098× | 11.31% | PASS |
| ac_writes | `oltp_update_index_ac` | 34.96ms | 155.22ms | 4.440× | 16.54% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 28.03ms | 119.48ms | 4.263× | 17.22% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 29.18ms | 123.15ms | 4.221× | 11.09% | PASS |
| ac_writes | `oltp_write_only_ac` | 30.52ms | 124.58ms | 4.081× | 16.39% | PASS |
| ac_writes | `types_delete_insert_ac` | 28.91ms | 122.98ms | 4.254× | 23.01% | PASS |
| ac_writes | `oltp_read_write_ac` | 34.32ms | 131.92ms | 3.844× | 13.42% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.10ms | 41.22ms | 1.209× | 1.36% | PASS |
| mem_reads | `oltp_range_select` | 19.85ms | 21.42ms | 1.079× | 1.98% | PASS |
| mem_reads | `oltp_sum_range` | 18.64ms | 21.37ms | 1.146× | 1.79% | PASS |
| mem_reads | `oltp_order_range` | 3.64ms | 3.89ms | 1.069× | 0.93% | PASS |
| mem_reads | `oltp_distinct_range` | 4.80ms | 4.98ms | 1.037× | 1.03% | PASS |
| mem_reads | `oltp_index_scan` | 4.81ms | 6.81ms | 1.416× | 2.36% | PASS |
| mem_reads | `select_random_points` | 29.83ms | 33.88ms | 1.136× | 3.14% | PASS |
| mem_reads | `select_random_ranges` | 7.99ms | 9.28ms | 1.161× | 1.55% | PASS |
| mem_reads | `covering_index_scan` | 4.30ms | 4.67ms | 1.086× | 2.61% | PASS |
| mem_reads | `groupby_scan` | 37.03ms | 39.15ms | 1.057× | 0.93% | PASS |
| mem_reads | `index_join` | 8.42ms | 11.42ms | 1.357× | 2.98% | PASS |
| mem_reads | `index_join_scan` | 4.35ms | 5.70ms | 1.312× | 3.25% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.25s | 1.181× | 2.26% | PASS |
| mem_reads | `table_scan` | 1.29s | 1.41s | 1.099× | 2.76% | PASS |
| mem_reads | `oltp_read_only` | 154.13ms | 170.71ms | 1.108× | 1.25% | PASS |
| mem_writes | `oltp_bulk_insert` | 244.26ms | 355.28ms | 1.455× | 1.39% | PASS |
| mem_writes | `oltp_insert` | 19.08ms | 36.38ms | 1.906× | 0.77% | PASS |
| mem_writes | `oltp_update_index` | 69.29ms | 118.31ms | 1.708× | 1.28% | PASS |
| mem_writes | `oltp_update_non_index` | 51.51ms | 84.23ms | 1.635× | 1.45% | PASS |
| mem_writes | `oltp_delete_insert` | 50.34ms | 96.85ms | 1.924× | 1.34% | PASS |
| mem_writes | `oltp_write_only` | 27.20ms | 58.54ms | 2.152× | 1.70% | PASS |
| mem_writes | `types_delete_insert` | 32.78ms | 55.46ms | 1.692× | 1.96% | PASS |
| mem_writes | `oltp_read_write` | 104.21ms | 157.47ms | 1.511× | 1.48% | PASS |
| file_reads | `oltp_point_select` | 108.37ms | 67.10ms | 0.619× | 1.21% | PASS |
| file_reads | `oltp_range_select` | 27.55ms | 24.36ms | 0.884× | 1.46% | PASS |
| file_reads | `oltp_sum_range` | 26.47ms | 24.26ms | 0.917× | 1.40% | PASS |
| file_reads | `oltp_order_range` | 4.52ms | 4.25ms | 0.940× | 1.07% | PASS |
| file_reads | `oltp_distinct_range` | 5.72ms | 5.35ms | 0.935× | 1.12% | PASS |
| file_reads | `oltp_index_scan` | 12.51ms | 9.31ms | 0.744× | 1.17% | PASS |
| file_reads | `select_random_points` | 38.07ms | 36.07ms | 0.947× | 2.00% | PASS |
| file_reads | `select_random_ranges` | 15.77ms | 12.18ms | 0.772× | 1.07% | PASS |
| file_reads | `covering_index_scan` | 12.23ms | 7.24ms | 0.592× | 1.51% | PASS |
| file_reads | `groupby_scan` | 37.94ms | 39.66ms | 1.045× | 1.14% | PASS |
| file_reads | `index_join` | 12.87ms | 13.15ms | 1.022× | 1.73% | PASS |
| file_reads | `index_join_scan` | 5.28ms | 6.01ms | 1.139× | 2.14% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.25s | 1.182× | 2.05% | PASS |
| file_reads | `table_scan` | 1.19s | 1.39s | 1.170× | 1.07% | PASS |
| file_reads | `oltp_read_only` | 261.36ms | 209.04ms | 0.800× | 0.85% | PASS |
| file_writes | `oltp_bulk_insert` | 258.34ms | 374.80ms | 1.451× | 0.95% | PASS |
| file_writes | `oltp_insert` | 29.01ms | 46.14ms | 1.591× | 1.99% | PASS |
| file_writes | `oltp_update_index` | 97.62ms | 141.96ms | 1.454× | 1.12% | PASS |
| file_writes | `oltp_update_non_index` | 79.47ms | 103.61ms | 1.304× | 1.34% | PASS |
| file_writes | `oltp_delete_insert` | 80.32ms | 118.48ms | 1.475× | 1.36% | PASS |
| file_writes | `oltp_write_only` | 56.76ms | 77.08ms | 1.358× | 1.45% | PASS |
| file_writes | `types_delete_insert` | 50.84ms | 68.14ms | 1.340× | 1.86% | PASS |
| file_writes | `oltp_read_write` | 133.46ms | 175.33ms | 1.314× | 1.47% | PASS |
| ac_reads | `oltp_point_select` | 57.23ms | 66.31ms | 1.159× | 0.83% | PASS |
| ac_reads | `oltp_range_select` | 21.86ms | 24.34ms | 1.113× | 1.39% | PASS |
| ac_reads | `oltp_sum_range` | 20.71ms | 24.12ms | 1.165× | 0.96% | PASS |
| ac_reads | `oltp_order_range` | 3.90ms | 4.26ms | 1.092× | 1.32% | PASS |
| ac_reads | `oltp_distinct_range` | 5.07ms | 5.34ms | 1.052× | 0.86% | PASS |
| ac_reads | `oltp_index_scan` | 7.24ms | 9.29ms | 1.284× | 2.04% | PASS |
| ac_reads | `select_random_points` | 31.04ms | 35.99ms | 1.160× | 1.35% | PASS |
| ac_reads | `select_random_ranges` | 10.33ms | 12.10ms | 1.171× | 1.23% | PASS |
| ac_reads | `covering_index_scan` | 6.80ms | 7.16ms | 1.052× | 1.23% | PASS |
| ac_reads | `groupby_scan` | 36.65ms | 39.35ms | 1.073× | 0.60% | PASS |
| ac_reads | `index_join` | 9.78ms | 12.93ms | 1.322× | 1.41% | PASS |
| ac_reads | `index_join_scan` | 4.66ms | 6.10ms | 1.308× | 1.37% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.23s | 1.194× | 0.63% | PASS |
| ac_reads | `table_scan` | 1.19s | 1.39s | 1.172× | 0.84% | PASS |
| ac_reads | `oltp_read_only` | 185.88ms | 208.88ms | 1.124× | 1.07% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 28.37ms | 100.39ms | 3.539× | 6.80% | PASS |
| ac_writes | `oltp_insert_ac` | 30.16ms | 118.06ms | 3.914× | 7.36% | PASS |
| ac_writes | `oltp_update_index_ac` | 36.06ms | 139.24ms | 3.861× | 9.04% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 31.72ms | 124.16ms | 3.914× | 7.97% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 31.66ms | 126.17ms | 3.985× | 9.04% | PASS |
| ac_writes | `oltp_write_only_ac` | 34.42ms | 127.32ms | 3.699× | 7.11% | PASS |
| ac_writes | `types_delete_insert_ac` | 31.65ms | 126.46ms | 3.996× | 15.51% | PASS |
| ac_writes | `oltp_read_write_ac` | 46.47ms | 149.88ms | 3.225× | 14.08% | PASS |

</details>

## Version-control latency

Wall time: 2m 20s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.38ms | 200.00ms | 41.7% | 0.53% | PASS |
| `status_dirty_many_tables` | 86.40ms | 200.00ms | 43.2% | 0.46% | PASS |
| `diff_regular_working_one_table` | 79.06ms | 150.00ms | 52.7% | 0.63% | PASS |
| `diff_regular_working_many_tables` | 92.11ms | 200.00ms | 46.1% | 0.57% | PASS |
| `diff_stat_working_many_tables` | 92.87ms | 200.00ms | 46.4% | 0.97% | PASS |
| `diff_schema_working_many_tables` | 92.43ms | 200.00ms | 46.2% | 0.38% | PASS |
| `branch_list_many_branches` | 22.85ms | 100.00ms | 22.9% | 1.44% | PASS |
| `branch_create_delete` | 25.31ms | 100.00ms | 25.3% | 1.37% | PASS |
| `checkout_branch_clean` | 55.57ms | 200.00ms | 27.8% | 0.83% | PASS |
| `merge_data_no_conflicts` | 29.17ms | 150.00ms | 19.4% | 1.31% | PASS |
| `merge_schema_no_conflicts` | 22.79ms | 100.00ms | 22.8% | 2.03% | PASS |
| `merge_data_conflicts` | 127.17ms | 250.00ms | 50.9% | 0.32% | PASS |
| `merge_data_conflicts_with_resolve` | 127.72ms | 250.00ms | 51.1% | 0.51% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
