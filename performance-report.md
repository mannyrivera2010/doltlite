# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-27 21:31 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260823.283.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/33110821344)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 22s | 9.41s | 11.08s | 1.178× | 1.41% | **PASS** |
| textpk | 69 | 55 | 1h 30m 59s | 10.25s | 11.60s | 1.132× | 0.92% | **PASS** |
| blobpk | 69 | 55 | 1h 32m 21s | 10.03s | 11.98s | 1.195× | 1.61% | **PASS** |
| compositepk | 69 | 55 | 1h 16m 44s | 8.42s | 9.67s | 1.149× | 0.87% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.31ms | 27.85ms | 1.100× | 2.44% | PASS |
| mem_reads | `oltp_range_select` | 11.05ms | 12.11ms | 1.095× | 2.16% | PASS |
| mem_reads | `oltp_sum_range` | 9.87ms | 11.64ms | 1.179× | 1.64% | PASS |
| mem_reads | `oltp_order_range` | 2.68ms | 2.90ms | 1.080× | 1.33% | PASS |
| mem_reads | `oltp_distinct_range` | 3.75ms | 3.90ms | 1.041× | 0.61% | PASS |
| mem_reads | `oltp_index_scan` | 4.02ms | 5.00ms | 1.244× | 2.15% | PASS |
| mem_reads | `select_random_points` | 10.92ms | 11.38ms | 1.043× | 3.26% | PASS |
| mem_reads | `select_random_ranges` | 3.12ms | 4.04ms | 1.292× | 1.55% | PASS |
| mem_reads | `covering_index_scan` | 4.37ms | 4.20ms | 0.960× | 3.05% | PASS |
| mem_reads | `groupby_scan` | 32.27ms | 34.47ms | 1.068× | 0.96% | PASS |
| mem_reads | `index_join` | 5.91ms | 7.90ms | 1.337× | 1.83% | PASS |
| mem_reads | `index_join_scan` | 3.49ms | 4.68ms | 1.338× | 1.38% | PASS |
| mem_reads | `types_table_scan` | 1.12s | 1.27s | 1.133× | 0.96% | PASS |
| mem_reads | `table_scan` | 1.28s | 1.38s | 1.075× | 1.40% | PASS |
| mem_reads | `oltp_read_only` | 105.08ms | 115.19ms | 1.096× | 1.41% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.62ms | 242.98ms | 1.353× | 0.99% | PASS |
| mem_writes | `oltp_insert` | 15.82ms | 28.29ms | 1.789× | 0.76% | PASS |
| mem_writes | `oltp_update_index` | 51.45ms | 106.00ms | 2.060× | 1.31% | PASS |
| mem_writes | `oltp_update_non_index` | 35.16ms | 59.12ms | 1.681× | 1.44% | PASS |
| mem_writes | `oltp_delete_insert` | 45.37ms | 78.80ms | 1.737× | 0.92% | PASS |
| mem_writes | `oltp_write_only` | 22.23ms | 45.54ms | 2.048× | 1.59% | PASS |
| mem_writes | `types_delete_insert` | 25.05ms | 40.07ms | 1.599× | 1.22% | PASS |
| mem_writes | `oltp_read_write` | 66.52ms | 105.13ms | 1.580× | 1.55% | PASS |
| file_reads | `oltp_point_select` | 119.94ms | 59.14ms | 0.493× | 0.87% | PASS |
| file_reads | `oltp_range_select` | 20.65ms | 15.19ms | 0.736× | 1.05% | PASS |
| file_reads | `oltp_sum_range` | 19.77ms | 14.98ms | 0.757× | 0.86% | PASS |
| file_reads | `oltp_order_range` | 3.75ms | 3.26ms | 0.871× | 0.99% | PASS |
| file_reads | `oltp_distinct_range` | 4.79ms | 4.25ms | 0.887× | 1.19% | PASS |
| file_reads | `oltp_index_scan` | 13.86ms | 8.54ms | 0.616× | 1.13% | PASS |
| file_reads | `select_random_points` | 20.96ms | 14.71ms | 0.702× | 1.77% | PASS |
| file_reads | `select_random_ranges` | 12.73ms | 7.21ms | 0.567× | 0.85% | PASS |
| file_reads | `covering_index_scan` | 14.36ms | 7.66ms | 0.533× | 0.91% | PASS |
| file_reads | `groupby_scan` | 33.20ms | 34.85ms | 1.050× | 0.89% | PASS |
| file_reads | `index_join` | 11.31ms | 10.20ms | 0.902× | 1.66% | PASS |
| file_reads | `index_join_scan` | 4.64ms | 5.13ms | 1.106× | 2.24% | PASS |
| file_reads | `types_table_scan` | 1.10s | 1.27s | 1.148× | 0.81% | PASS |
| file_reads | `table_scan` | 1.28s | 1.38s | 1.083× | 0.96% | PASS |
| file_reads | `oltp_read_only` | 243.03ms | 161.61ms | 0.665× | 0.81% | PASS |
| file_writes | `oltp_bulk_insert` | 195.32ms | 262.24ms | 1.343× | 0.99% | PASS |
| file_writes | `oltp_insert` | 22.32ms | 36.28ms | 1.625× | 1.58% | PASS |
| file_writes | `oltp_update_index` | 81.82ms | 132.50ms | 1.619× | 1.53% | PASS |
| file_writes | `oltp_update_non_index` | 61.54ms | 84.03ms | 1.365× | 1.64% | PASS |
| file_writes | `oltp_delete_insert` | 71.46ms | 101.36ms | 1.418× | 1.95% | PASS |
| file_writes | `oltp_write_only` | 45.19ms | 67.00ms | 1.483× | 1.78% | PASS |
| file_writes | `types_delete_insert` | 41.73ms | 54.38ms | 1.303× | 1.55% | PASS |
| file_writes | `oltp_read_write` | 91.04ms | 127.37ms | 1.399× | 2.13% | PASS |
| ac_reads | `oltp_point_select` | 56.39ms | 59.61ms | 1.057× | 0.98% | PASS |
| ac_reads | `oltp_range_select` | 14.30ms | 15.23ms | 1.065× | 1.40% | PASS |
| ac_reads | `oltp_sum_range` | 13.25ms | 14.88ms | 1.123× | 1.84% | PASS |
| ac_reads | `oltp_order_range` | 3.15ms | 3.26ms | 1.033× | 1.27% | PASS |
| ac_reads | `oltp_distinct_range` | 4.15ms | 4.24ms | 1.021× | 0.66% | PASS |
| ac_reads | `oltp_index_scan` | 7.38ms | 8.48ms | 1.150× | 1.17% | PASS |
| ac_reads | `select_random_points` | 14.38ms | 14.79ms | 1.028× | 1.67% | PASS |
| ac_reads | `select_random_ranges` | 6.32ms | 7.20ms | 1.138× | 1.41% | PASS |
| ac_reads | `covering_index_scan` | 7.90ms | 7.65ms | 0.969× | 1.85% | PASS |
| ac_reads | `groupby_scan` | 32.56ms | 34.92ms | 1.072× | 0.88% | PASS |
| ac_reads | `index_join` | 7.96ms | 10.23ms | 1.285× | 1.78% | PASS |
| ac_reads | `index_join_scan` | 4.05ms | 5.10ms | 1.260× | 1.67% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.27s | 1.145× | 0.74% | PASS |
| ac_reads | `table_scan` | 1.26s | 1.37s | 1.091× | 0.69% | PASS |
| ac_reads | `oltp_read_only` | 149.24ms | 160.33ms | 1.074× | 0.77% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 14.91ms | 60.39ms | 4.049× | 3.32% | PASS |
| ac_writes | `oltp_insert_ac` | 17.01ms | 77.46ms | 4.553× | 3.13% | PASS |
| ac_writes | `oltp_update_index_ac` | 18.72ms | 92.81ms | 4.958× | 2.83% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 15.23ms | 70.06ms | 4.599× | 5.13% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.10ms | 82.48ms | 4.822× | 2.50% | PASS |
| ac_writes | `oltp_write_only_ac` | 17.02ms | 82.37ms | 4.839× | 3.20% | PASS |
| ac_writes | `types_delete_insert_ac` | 14.88ms | 71.33ms | 4.794× | 5.19% | PASS |
| ac_writes | `oltp_read_write_ac` | 22.49ms | 89.12ms | 3.963× | 2.78% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.99ms | 35.17ms | 1.173× | 0.88% | PASS |
| mem_reads | `oltp_range_select` | 13.79ms | 13.66ms | 0.991× | 1.16% | PASS |
| mem_reads | `oltp_sum_range` | 12.08ms | 13.22ms | 1.094× | 1.09% | PASS |
| mem_reads | `oltp_order_range` | 3.12ms | 3.12ms | 1.000× | 1.05% | PASS |
| mem_reads | `oltp_distinct_range` | 4.19ms | 4.14ms | 0.988× | 1.13% | PASS |
| mem_reads | `oltp_index_scan` | 4.60ms | 5.82ms | 1.265× | 0.85% | PASS |
| mem_reads | `select_random_points` | 17.44ms | 19.65ms | 1.127× | 0.79% | PASS |
| mem_reads | `select_random_ranges` | 4.07ms | 5.15ms | 1.264× | 1.30% | PASS |
| mem_reads | `covering_index_scan` | 4.85ms | 4.37ms | 0.900× | 1.16% | PASS |
| mem_reads | `groupby_scan` | 33.84ms | 35.20ms | 1.040× | 0.60% | PASS |
| mem_reads | `index_join` | 7.03ms | 8.80ms | 1.252× | 0.76% | PASS |
| mem_reads | `index_join_scan` | 4.82ms | 5.53ms | 1.149× | 0.92% | PASS |
| mem_reads | `types_table_scan` | 1.14s | 1.25s | 1.095× | 0.38% | PASS |
| mem_reads | `table_scan` | 1.33s | 1.35s | 1.021× | 0.35% | PASS |
| mem_reads | `oltp_read_only` | 119.07ms | 129.45ms | 1.087× | 0.55% | PASS |
| mem_writes | `oltp_bulk_insert` | 232.09ms | 333.65ms | 1.438× | 0.79% | PASS |
| mem_writes | `oltp_insert` | 22.30ms | 39.14ms | 1.755× | 0.84% | PASS |
| mem_writes | `oltp_update_index` | 72.53ms | 134.85ms | 1.859× | 0.56% | PASS |
| mem_writes | `oltp_update_non_index` | 49.89ms | 87.24ms | 1.749× | 0.74% | PASS |
| mem_writes | `oltp_delete_insert` | 51.90ms | 104.78ms | 2.019× | 0.54% | PASS |
| mem_writes | `oltp_write_only` | 29.51ms | 63.19ms | 2.142× | 0.54% | PASS |
| mem_writes | `types_delete_insert` | 33.05ms | 53.96ms | 1.633× | 0.84% | PASS |
| mem_writes | `oltp_read_write` | 83.31ms | 135.47ms | 1.626× | 0.72% | PASS |
| file_reads | `oltp_point_select` | 126.46ms | 67.75ms | 0.536× | 0.80% | PASS |
| file_reads | `oltp_range_select` | 24.33ms | 17.07ms | 0.702× | 0.83% | PASS |
| file_reads | `oltp_sum_range` | 22.61ms | 16.64ms | 0.736× | 0.98% | PASS |
| file_reads | `oltp_order_range` | 4.22ms | 3.53ms | 0.836× | 1.02% | PASS |
| file_reads | `oltp_distinct_range` | 5.28ms | 4.52ms | 0.856× | 0.66% | PASS |
| file_reads | `oltp_index_scan` | 14.66ms | 9.44ms | 0.644× | 1.42% | PASS |
| file_reads | `select_random_points` | 28.37ms | 23.60ms | 0.832× | 1.11% | PASS |
| file_reads | `select_random_ranges` | 13.96ms | 8.51ms | 0.610× | 0.45% | PASS |
| file_reads | `covering_index_scan` | 15.76ms | 7.92ms | 0.503× | 0.97% | PASS |
| file_reads | `groupby_scan` | 35.24ms | 35.63ms | 1.011× | 0.61% | PASS |
| file_reads | `index_join` | 13.04ms | 11.35ms | 0.870× | 1.02% | PASS |
| file_reads | `index_join_scan` | 5.93ms | 6.01ms | 1.014× | 1.48% | PASS |
| file_reads | `types_table_scan` | 1.14s | 1.24s | 1.082× | 0.57% | PASS |
| file_reads | `table_scan` | 1.34s | 1.35s | 1.009× | 0.47% | PASS |
| file_reads | `oltp_read_only` | 260.13ms | 175.57ms | 0.675× | 0.76% | PASS |
| file_writes | `oltp_bulk_insert` | 252.94ms | 365.52ms | 1.445× | 0.98% | PASS |
| file_writes | `oltp_insert` | 49.68ms | 52.20ms | 1.051× | 25.59% | PASS |
| file_writes | `oltp_update_index` | 117.41ms | 173.82ms | 1.480× | 1.15% | PASS |
| file_writes | `oltp_update_non_index` | 95.97ms | 115.09ms | 1.199× | 9.09% | PASS |
| file_writes | `oltp_delete_insert` | 91.91ms | 136.58ms | 1.486× | 1.25% | PASS |
| file_writes | `oltp_write_only` | 92.85ms | 88.08ms | 0.949× | 10.10% | PASS |
| file_writes | `types_delete_insert` | 56.33ms | 74.77ms | 1.327× | 1.28% | PASS |
| file_writes | `oltp_read_write` | 143.21ms | 159.88ms | 1.116× | 4.26% | PASS |
| ac_reads | `oltp_point_select` | 61.61ms | 67.56ms | 1.096× | 0.72% | PASS |
| ac_reads | `oltp_range_select` | 17.72ms | 17.11ms | 0.966× | 0.96% | PASS |
| ac_reads | `oltp_sum_range` | 16.08ms | 16.73ms | 1.041× | 0.87% | PASS |
| ac_reads | `oltp_order_range` | 3.61ms | 3.55ms | 0.983× | 1.38% | PASS |
| ac_reads | `oltp_distinct_range` | 4.62ms | 4.54ms | 0.983× | 0.88% | PASS |
| ac_reads | `oltp_index_scan` | 8.18ms | 9.39ms | 1.148× | 0.80% | PASS |
| ac_reads | `select_random_points` | 21.67ms | 23.48ms | 1.084× | 0.99% | PASS |
| ac_reads | `select_random_ranges` | 7.44ms | 8.46ms | 1.137× | 0.71% | PASS |
| ac_reads | `covering_index_scan` | 9.15ms | 7.93ms | 0.867× | 0.92% | PASS |
| ac_reads | `groupby_scan` | 34.41ms | 35.72ms | 1.038× | 0.72% | PASS |
| ac_reads | `index_join` | 9.60ms | 11.37ms | 1.184× | 0.82% | PASS |
| ac_reads | `index_join_scan` | 5.31ms | 6.06ms | 1.141× | 1.10% | PASS |
| ac_reads | `types_table_scan` | 1.14s | 1.24s | 1.085× | 0.43% | PASS |
| ac_reads | `table_scan` | 1.34s | 1.35s | 1.009× | 0.41% | PASS |
| ac_reads | `oltp_read_only` | 167.30ms | 175.62ms | 1.050× | 0.56% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.73ms | 61.44ms | 3.907× | 3.00% | PASS |
| ac_writes | `oltp_insert_ac` | 18.64ms | 78.12ms | 4.192× | 4.04% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.61ms | 96.46ms | 4.680× | 3.76% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 15.75ms | 73.62ms | 4.675× | 2.22% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.89ms | 84.73ms | 4.736× | 1.60% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.42ms | 85.65ms | 4.651× | 4.24% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.29ms | 72.60ms | 4.747× | 3.17% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.49ms | 92.94ms | 3.646× | 2.62% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.54ms | 36.57ms | 1.238× | 1.40% | PASS |
| mem_reads | `oltp_range_select` | 11.99ms | 13.91ms | 1.160× | 1.75% | PASS |
| mem_reads | `oltp_sum_range` | 11.24ms | 13.78ms | 1.225× | 1.41% | PASS |
| mem_reads | `oltp_order_range` | 2.79ms | 3.11ms | 1.118× | 1.42% | PASS |
| mem_reads | `oltp_distinct_range` | 3.96ms | 4.21ms | 1.064× | 0.84% | PASS |
| mem_reads | `oltp_index_scan` | 4.47ms | 6.22ms | 1.393× | 1.94% | PASS |
| mem_reads | `select_random_points` | 17.13ms | 20.51ms | 1.197× | 1.97% | PASS |
| mem_reads | `select_random_ranges` | 3.91ms | 5.11ms | 1.307× | 2.43% | PASS |
| mem_reads | `covering_index_scan` | 4.38ms | 4.36ms | 0.994× | 1.37% | PASS |
| mem_reads | `groupby_scan` | 31.41ms | 33.65ms | 1.071× | 0.87% | PASS |
| mem_reads | `index_join` | 6.73ms | 8.82ms | 1.311× | 1.09% | PASS |
| mem_reads | `index_join_scan` | 4.04ms | 5.29ms | 1.311× | 2.29% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.22s | 1.180× | 0.78% | PASS |
| mem_reads | `table_scan` | 1.27s | 1.38s | 1.090× | 1.92% | PASS |
| mem_reads | `oltp_read_only` | 119.98ms | 136.49ms | 1.138× | 1.18% | PASS |
| mem_writes | `oltp_bulk_insert` | 241.36ms | 353.75ms | 1.466× | 1.10% | PASS |
| mem_writes | `oltp_insert` | 19.88ms | 39.93ms | 2.009× | 1.03% | PASS |
| mem_writes | `oltp_update_index` | 67.28ms | 129.81ms | 1.929× | 1.38% | PASS |
| mem_writes | `oltp_update_non_index` | 47.62ms | 84.04ms | 1.765× | 1.61% | PASS |
| mem_writes | `oltp_delete_insert` | 49.08ms | 104.49ms | 2.129× | 1.22% | PASS |
| mem_writes | `oltp_write_only` | 27.87ms | 62.48ms | 2.242× | 1.28% | PASS |
| mem_writes | `types_delete_insert` | 32.62ms | 55.27ms | 1.694× | 0.90% | PASS |
| mem_writes | `oltp_read_write` | 86.47ms | 140.90ms | 1.630× | 1.92% | PASS |
| file_reads | `oltp_point_select` | 106.84ms | 63.12ms | 0.591× | 1.10% | PASS |
| file_reads | `oltp_range_select` | 20.62ms | 16.76ms | 0.813× | 2.74% | PASS |
| file_reads | `oltp_sum_range` | 19.59ms | 16.72ms | 0.853× | 1.56% | PASS |
| file_reads | `oltp_order_range` | 3.69ms | 3.51ms | 0.953× | 2.23% | PASS |
| file_reads | `oltp_distinct_range` | 4.79ms | 4.62ms | 0.965× | 1.44% | PASS |
| file_reads | `oltp_index_scan` | 12.12ms | 9.15ms | 0.755× | 1.47% | PASS |
| file_reads | `select_random_points` | 25.80ms | 24.11ms | 0.934× | 1.34% | PASS |
| file_reads | `select_random_ranges` | 11.52ms | 7.84ms | 0.680× | 1.36% | PASS |
| file_reads | `covering_index_scan` | 12.09ms | 7.35ms | 0.608× | 0.82% | PASS |
| file_reads | `groupby_scan` | 32.21ms | 34.16ms | 1.061× | 1.05% | PASS |
| file_reads | `index_join` | 11.18ms | 11.29ms | 1.010× | 1.68% | PASS |
| file_reads | `index_join_scan` | 5.30ms | 5.84ms | 1.103× | 2.69% | PASS |
| file_reads | `types_table_scan` | 1.08s | 1.24s | 1.146× | 1.91% | PASS |
| file_reads | `table_scan` | 1.33s | 1.39s | 1.046× | 0.98% | PASS |
| file_reads | `oltp_read_only` | 239.18ms | 178.68ms | 0.747× | 1.08% | PASS |
| file_writes | `oltp_bulk_insert` | 261.35ms | 383.85ms | 1.469× | 1.16% | PASS |
| file_writes | `oltp_insert` | 32.54ms | 53.10ms | 1.632× | 1.97% | PASS |
| file_writes | `oltp_update_index` | 104.16ms | 165.19ms | 1.586× | 1.76% | PASS |
| file_writes | `oltp_update_non_index` | 81.12ms | 109.44ms | 1.349× | 1.77% | PASS |
| file_writes | `oltp_delete_insert` | 82.44ms | 132.20ms | 1.604× | 1.20% | PASS |
| file_writes | `oltp_write_only` | 57.26ms | 85.71ms | 1.497× | 2.10% | PASS |
| file_writes | `types_delete_insert` | 53.33ms | 73.89ms | 1.385× | 2.39% | PASS |
| file_writes | `oltp_read_write` | 119.11ms | 164.61ms | 1.382× | 1.74% | PASS |
| ac_reads | `oltp_point_select` | 56.53ms | 63.47ms | 1.123× | 1.31% | PASS |
| ac_reads | `oltp_range_select` | 17.01ms | 16.91ms | 0.995× | 1.90% | PASS |
| ac_reads | `oltp_sum_range` | 15.80ms | 16.99ms | 1.075× | 1.72% | PASS |
| ac_reads | `oltp_order_range` | 3.49ms | 3.57ms | 1.024× | 1.72% | PASS |
| ac_reads | `oltp_distinct_range` | 4.54ms | 4.67ms | 1.028× | 2.28% | PASS |
| ac_reads | `oltp_index_scan` | 7.76ms | 9.22ms | 1.188× | 1.37% | PASS |
| ac_reads | `select_random_points` | 22.55ms | 25.36ms | 1.125× | 1.92% | PASS |
| ac_reads | `select_random_ranges` | 7.03ms | 8.00ms | 1.139× | 1.48% | PASS |
| ac_reads | `covering_index_scan` | 8.12ms | 7.48ms | 0.921× | 2.04% | PASS |
| ac_reads | `groupby_scan` | 32.85ms | 34.74ms | 1.058× | 0.89% | PASS |
| ac_reads | `index_join` | 9.57ms | 11.79ms | 1.232× | 2.85% | PASS |
| ac_reads | `index_join_scan` | 4.91ms | 6.16ms | 1.255× | 3.40% | PASS |
| ac_reads | `types_table_scan` | 1.19s | 1.28s | 1.072× | 0.75% | PASS |
| ac_reads | `table_scan` | 1.44s | 1.43s | 0.992× | 0.47% | PASS |
| ac_reads | `oltp_read_only` | 170.68ms | 182.22ms | 1.068× | 1.11% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.48ms | 84.42ms | 3.595× | 9.39% | PASS |
| ac_writes | `oltp_insert_ac` | 26.57ms | 108.02ms | 4.065× | 6.08% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.28ms | 117.79ms | 4.166× | 7.18% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.08ms | 102.48ms | 4.086× | 11.12% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.82ms | 110.27ms | 4.271× | 6.22% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.41ms | 105.74ms | 4.162× | 4.52% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.53ms | 100.88ms | 4.477× | 6.77% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.29ms | 115.08ms | 3.564× | 4.38% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.07ms | 29.54ms | 1.133× | 1.28% | PASS |
| mem_reads | `oltp_range_select` | 15.56ms | 15.51ms | 0.997× | 0.94% | PASS |
| mem_reads | `oltp_sum_range` | 14.35ms | 14.76ms | 1.028× | 0.78% | PASS |
| mem_reads | `oltp_order_range` | 2.94ms | 2.93ms | 0.998× | 0.78% | PASS |
| mem_reads | `oltp_distinct_range` | 3.78ms | 3.75ms | 0.993× | 0.66% | PASS |
| mem_reads | `oltp_index_scan` | 3.68ms | 4.47ms | 1.214× | 1.23% | PASS |
| mem_reads | `select_random_points` | 21.34ms | 23.50ms | 1.101× | 0.63% | PASS |
| mem_reads | `select_random_ranges` | 5.95ms | 6.43ms | 1.081× | 0.96% | PASS |
| mem_reads | `covering_index_scan` | 3.40ms | 3.17ms | 0.934× | 0.78% | PASS |
| mem_reads | `groupby_scan` | 30.07ms | 31.12ms | 1.035× | 0.49% | PASS |
| mem_reads | `index_join` | 6.43ms | 7.76ms | 1.206× | 0.69% | PASS |
| mem_reads | `index_join_scan` | 3.42ms | 4.53ms | 1.324× | 1.06% | PASS |
| mem_reads | `types_table_scan` | 869.26ms | 967.80ms | 1.113× | 0.44% | PASS |
| mem_reads | `table_scan` | 997.70ms | 1.05s | 1.056× | 0.45% | PASS |
| mem_reads | `oltp_read_only` | 117.06ms | 122.64ms | 1.048× | 0.57% | PASS |
| mem_writes | `oltp_bulk_insert` | 191.04ms | 257.43ms | 1.347× | 0.67% | PASS |
| mem_writes | `oltp_insert` | 15.15ms | 27.10ms | 1.789× | 0.51% | PASS |
| mem_writes | `oltp_update_index` | 53.29ms | 88.94ms | 1.669× | 0.93% | PASS |
| mem_writes | `oltp_update_non_index` | 40.23ms | 63.15ms | 1.570× | 0.71% | PASS |
| mem_writes | `oltp_delete_insert` | 39.14ms | 71.94ms | 1.838× | 0.92% | PASS |
| mem_writes | `oltp_write_only` | 21.60ms | 43.99ms | 2.037× | 0.79% | PASS |
| mem_writes | `types_delete_insert` | 25.52ms | 39.64ms | 1.553× | 0.87% | PASS |
| mem_writes | `oltp_read_write` | 76.57ms | 111.61ms | 1.458× | 0.73% | PASS |
| file_reads | `oltp_point_select` | 100.69ms | 54.75ms | 0.544× | 0.75% | PASS |
| file_reads | `oltp_range_select` | 23.22ms | 18.13ms | 0.781× | 1.54% | PASS |
| file_reads | `oltp_sum_range` | 21.97ms | 17.53ms | 0.798× | 0.99% | PASS |
| file_reads | `oltp_order_range` | 3.67ms | 3.25ms | 0.886× | 1.30% | PASS |
| file_reads | `oltp_distinct_range` | 4.52ms | 4.08ms | 0.904× | 1.22% | PASS |
| file_reads | `oltp_index_scan` | 11.24ms | 7.46ms | 0.664× | 1.04% | PASS |
| file_reads | `select_random_points` | 29.17ms | 26.46ms | 0.907× | 0.62% | PASS |
| file_reads | `select_random_ranges` | 13.56ms | 9.18ms | 0.677× | 1.26% | PASS |
| file_reads | `covering_index_scan` | 11.06ms | 6.17ms | 0.558× | 0.99% | PASS |
| file_reads | `groupby_scan` | 30.91ms | 31.61ms | 1.023× | 0.87% | PASS |
| file_reads | `index_join` | 10.59ms | 9.99ms | 0.944× | 0.85% | PASS |
| file_reads | `index_join_scan` | 4.23ms | 4.91ms | 1.160× | 0.90% | PASS |
| file_reads | `types_table_scan` | 863.40ms | 963.82ms | 1.116× | 0.53% | PASS |
| file_reads | `table_scan` | 992.47ms | 1.05s | 1.057× | 0.35% | PASS |
| file_reads | `oltp_read_only` | 225.17ms | 158.35ms | 0.703× | 0.48% | PASS |
| file_writes | `oltp_bulk_insert` | 242.27ms | 323.89ms | 1.337× | 3.02% | PASS |
| file_writes | `oltp_insert` | 33.71ms | 49.40ms | 1.465× | 5.42% | PASS |
| file_writes | `oltp_update_index` | 161.22ms | 166.06ms | 1.030× | 2.85% | PASS |
| file_writes | `oltp_update_non_index` | 136.64ms | 121.84ms | 0.892× | 1.61% | PASS |
| file_writes | `oltp_delete_insert` | 132.85ms | 138.09ms | 1.039× | 1.67% | PASS |
| file_writes | `oltp_write_only` | 96.77ms | 99.76ms | 1.031× | 3.15% | PASS |
| file_writes | `types_delete_insert` | 86.19ms | 85.46ms | 0.991× | 7.55% | PASS |
| file_writes | `oltp_read_write` | 152.87ms | 166.50ms | 1.089× | 4.65% | PASS |
| ac_reads | `oltp_point_select` | 51.02ms | 54.41ms | 1.066× | 0.98% | PASS |
| ac_reads | `oltp_range_select` | 18.83ms | 18.26ms | 0.970× | 0.64% | PASS |
| ac_reads | `oltp_sum_range` | 17.40ms | 17.47ms | 1.004× | 0.75% | PASS |
| ac_reads | `oltp_order_range` | 3.41ms | 3.25ms | 0.953× | 0.98% | PASS |
| ac_reads | `oltp_distinct_range` | 4.21ms | 4.10ms | 0.973× | 0.89% | PASS |
| ac_reads | `oltp_index_scan` | 6.65ms | 7.47ms | 1.124× | 0.64% | PASS |
| ac_reads | `select_random_points` | 24.77ms | 26.50ms | 1.070× | 0.60% | PASS |
| ac_reads | `select_random_ranges` | 8.89ms | 9.20ms | 1.035× | 0.87% | PASS |
| ac_reads | `covering_index_scan` | 6.29ms | 6.17ms | 0.982× | 0.55% | PASS |
| ac_reads | `groupby_scan` | 30.50ms | 31.56ms | 1.035× | 0.58% | PASS |
| ac_reads | `index_join` | 8.28ms | 10.02ms | 1.210× | 0.61% | PASS |
| ac_reads | `index_join_scan` | 3.90ms | 4.93ms | 1.266× | 0.80% | PASS |
| ac_reads | `types_table_scan` | 861.98ms | 963.64ms | 1.118× | 0.50% | PASS |
| ac_reads | `table_scan` | 992.67ms | 1.05s | 1.058× | 0.46% | PASS |
| ac_reads | `oltp_read_only` | 153.96ms | 158.66ms | 1.031× | 0.57% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 29.17ms | 80.68ms | 2.766× | 14.39% | PASS |
| ac_writes | `oltp_insert_ac` | 31.63ms | 99.69ms | 3.152× | 12.60% | PASS |
| ac_writes | `oltp_update_index_ac` | 32.81ms | 126.77ms | 3.864× | 37.32% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 29.05ms | 91.41ms | 3.146× | 10.09% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 30.26ms | 99.74ms | 3.296× | 8.76% | PASS |
| ac_writes | `oltp_write_only_ac` | 31.56ms | 96.01ms | 3.042× | 10.32% | PASS |
| ac_writes | `types_delete_insert_ac` | 29.11ms | 90.83ms | 3.120× | 15.41% | PASS |
| ac_writes | `oltp_read_write_ac` | 37.19ms | 106.16ms | 2.855× | 7.72% | PASS |

</details>

## Version-control latency

Wall time: 2m 18s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 82.30ms | 200.00ms | 41.2% | 0.84% | PASS |
| `status_dirty_many_tables` | 85.23ms | 200.00ms | 42.6% | 0.61% | PASS |
| `diff_regular_working_one_table` | 78.33ms | 150.00ms | 52.2% | 1.08% | PASS |
| `diff_regular_working_many_tables` | 90.84ms | 200.00ms | 45.4% | 0.69% | PASS |
| `diff_stat_working_many_tables` | 90.89ms | 200.00ms | 45.4% | 0.74% | PASS |
| `diff_schema_working_many_tables` | 91.60ms | 200.00ms | 45.8% | 0.63% | PASS |
| `branch_list_many_branches` | 22.41ms | 100.00ms | 22.4% | 1.05% | PASS |
| `branch_create_delete` | 24.63ms | 100.00ms | 24.6% | 0.91% | PASS |
| `checkout_branch_clean` | 54.52ms | 200.00ms | 27.3% | 0.66% | PASS |
| `merge_data_no_conflicts` | 28.41ms | 150.00ms | 18.9% | 2.08% | PASS |
| `merge_schema_no_conflicts` | 22.40ms | 100.00ms | 22.4% | 2.84% | PASS |
| `merge_data_conflicts` | 126.33ms | 250.00ms | 50.5% | 0.41% | PASS |
| `merge_data_conflicts_with_resolve` | 126.15ms | 250.00ms | 50.5% | 0.58% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
