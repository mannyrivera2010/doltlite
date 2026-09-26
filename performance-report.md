# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-26 15:25 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260920.314.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/36246413454)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 52m 37s | 6.22s | 7.11s | 1.145× | 2.03% | **PASS** |
| textpk | 69 | 55 | 1h 33m 43s | 10.43s | 11.91s | 1.141× | 1.66% | **PASS** |
| blobpk | 69 | 55 | 1h 32m 8s | 10.94s | 11.85s | 1.084× | 1.46% | **PASS** |
| compositepk | 69 | 55 | 1h 20m 42s | 8.75s | 9.83s | 1.122× | 1.23% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 13.29ms | 16.27ms | 1.224× | 2.14% | PASS |
| mem_reads | `oltp_range_select` | 5.33ms | 6.56ms | 1.230× | 2.91% | PASS |
| mem_reads | `oltp_sum_range` | 5.51ms | 6.78ms | 1.230× | 2.98% | PASS |
| mem_reads | `oltp_order_range` | 1.55ms | 1.68ms | 1.082× | 2.60% | PASS |
| mem_reads | `oltp_distinct_range` | 1.99ms | 2.15ms | 1.081× | 2.56% | PASS |
| mem_reads | `oltp_index_scan` | 2.15ms | 2.95ms | 1.371× | 1.36% | PASS |
| mem_reads | `select_random_points` | 6.22ms | 7.75ms | 1.246× | 5.76% | PASS |
| mem_reads | `select_random_ranges` | 1.89ms | 2.50ms | 1.328× | 4.54% | PASS |
| mem_reads | `covering_index_scan` | 2.33ms | 2.41ms | 1.034× | 2.38% | PASS |
| mem_reads | `groupby_scan` | 17.08ms | 18.08ms | 1.058× | 2.09% | PASS |
| mem_reads | `index_join` | 3.60ms | 4.62ms | 1.284× | 2.08% | PASS |
| mem_reads | `index_join_scan` | 1.86ms | 3.47ms | 1.870× | 2.97% | PASS |
| mem_reads | `types_table_scan` | 673.70ms | 734.85ms | 1.091× | 1.84% | PASS |
| mem_reads | `table_scan` | 774.16ms | 828.02ms | 1.070× | 1.42% | PASS |
| mem_reads | `oltp_read_only` | 55.54ms | 63.15ms | 1.137× | 1.29% | PASS |
| mem_writes | `oltp_bulk_insert` | 105.74ms | 142.17ms | 1.344× | 1.85% | PASS |
| mem_writes | `oltp_insert` | 9.13ms | 16.67ms | 1.825× | 0.95% | PASS |
| mem_writes | `oltp_update_index` | 29.24ms | 65.05ms | 2.224× | 1.46% | PASS |
| mem_writes | `oltp_update_non_index` | 20.46ms | 38.18ms | 1.866× | 1.54% | PASS |
| mem_writes | `oltp_delete_insert` | 25.38ms | 47.96ms | 1.890× | 1.54% | PASS |
| mem_writes | `oltp_write_only` | 12.66ms | 29.25ms | 2.311× | 1.67% | PASS |
| mem_writes | `types_delete_insert` | 14.94ms | 25.33ms | 1.695× | 2.12% | PASS |
| mem_writes | `oltp_read_write` | 37.40ms | 61.29ms | 1.639× | 1.84% | PASS |
| file_reads | `oltp_point_select` | 70.62ms | 36.32ms | 0.514× | 1.61% | PASS |
| file_reads | `oltp_range_select` | 11.87ms | 8.70ms | 0.733× | 1.52% | PASS |
| file_reads | `oltp_sum_range` | 11.40ms | 8.65ms | 0.758× | 1.36% | PASS |
| file_reads | `oltp_order_range` | 2.28ms | 1.92ms | 0.841× | 1.25% | PASS |
| file_reads | `oltp_distinct_range` | 2.78ms | 2.40ms | 0.866× | 1.67% | PASS |
| file_reads | `oltp_index_scan` | 8.29ms | 5.05ms | 0.609× | 1.30% | PASS |
| file_reads | `select_random_points` | 13.16ms | 9.72ms | 0.739× | 1.29% | PASS |
| file_reads | `select_random_ranges` | 7.50ms | 4.37ms | 0.583× | 1.90% | PASS |
| file_reads | `covering_index_scan` | 8.54ms | 4.75ms | 0.556× | 4.29% | PASS |
| file_reads | `groupby_scan` | 17.74ms | 18.62ms | 1.050× | 1.62% | PASS |
| file_reads | `index_join` | 7.24ms | 6.99ms | 0.965× | 7.21% | PASS |
| file_reads | `index_join_scan` | 3.25ms | 3.93ms | 1.212× | 7.24% | PASS |
| file_reads | `types_table_scan` | 695.30ms | 765.66ms | 1.101× | 2.48% | PASS |
| file_reads | `table_scan` | 792.59ms | 846.78ms | 1.068× | 2.43% | PASS |
| file_reads | `oltp_read_only` | 147.88ms | 96.94ms | 0.656× | 1.82% | PASS |
| file_writes | `oltp_bulk_insert` | 145.87ms | 201.87ms | 1.384× | 4.20% | PASS |
| file_writes | `oltp_insert` | 19.73ms | 28.33ms | 1.436× | 2.02% | PASS |
| file_writes | `oltp_update_index` | 121.21ms | 129.09ms | 1.065× | 4.80% | PASS |
| file_writes | `oltp_update_non_index` | 100.53ms | 88.39ms | 0.879× | 1.35% | PASS |
| file_writes | `oltp_delete_insert` | 101.87ms | 98.61ms | 0.968× | 2.03% | PASS |
| file_writes | `oltp_write_only` | 80.67ms | 69.23ms | 0.858× | 2.09% | PASS |
| file_writes | `types_delete_insert` | 60.52ms | 50.85ms | 0.840× | 8.55% | PASS |
| file_writes | `oltp_read_write` | 105.22ms | 99.82ms | 0.949× | 4.77% | PASS |
| ac_reads | `oltp_point_select` | 33.94ms | 37.50ms | 1.105× | 1.17% | PASS |
| ac_reads | `oltp_range_select` | 8.18ms | 9.04ms | 1.106× | 1.80% | PASS |
| ac_reads | `oltp_sum_range` | 7.75ms | 9.03ms | 1.165× | 1.26% | PASS |
| ac_reads | `oltp_order_range` | 1.85ms | 1.94ms | 1.050× | 1.27% | PASS |
| ac_reads | `oltp_distinct_range` | 2.44ms | 2.49ms | 1.021× | 1.19% | PASS |
| ac_reads | `oltp_index_scan` | 5.02ms | 5.88ms | 1.171× | 1.52% | PASS |
| ac_reads | `select_random_points` | 10.80ms | 11.11ms | 1.028× | 3.02% | PASS |
| ac_reads | `select_random_ranges` | 4.04ms | 4.64ms | 1.149× | 2.01% | PASS |
| ac_reads | `covering_index_scan` | 4.73ms | 4.89ms | 1.034× | 2.36% | PASS |
| ac_reads | `groupby_scan` | 17.14ms | 18.43ms | 1.076× | 1.96% | PASS |
| ac_reads | `index_join` | 4.72ms | 6.27ms | 1.328× | 3.47% | PASS |
| ac_reads | `index_join_scan` | 2.43ms | 3.69ms | 1.520× | 3.25% | PASS |
| ac_reads | `types_table_scan` | 693.01ms | 763.39ms | 1.102× | 1.09% | PASS |
| ac_reads | `table_scan` | 793.37ms | 856.42ms | 1.079× | 1.93% | PASS |
| ac_reads | `oltp_read_only` | 86.45ms | 93.40ms | 1.080× | 1.71% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.38ms | 54.73ms | 2.685× | 4.22% | PASS |
| ac_writes | `oltp_insert_ac` | 23.30ms | 71.64ms | 3.075× | 4.32% | PASS |
| ac_writes | `oltp_update_index_ac` | 24.06ms | 79.66ms | 3.310× | 4.47% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.35ms | 66.82ms | 3.130× | 5.63% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.68ms | 76.61ms | 3.235× | 3.48% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.07ms | 71.27ms | 3.229× | 3.27% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.19ms | 70.43ms | 3.324× | 7.58% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.94ms | 80.12ms | 3.088× | 4.82% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.62ms | 37.75ms | 1.275× | 1.60% | PASS |
| mem_reads | `oltp_range_select` | 12.37ms | 14.06ms | 1.137× | 2.05% | PASS |
| mem_reads | `oltp_sum_range` | 11.45ms | 14.15ms | 1.236× | 1.44% | PASS |
| mem_reads | `oltp_order_range` | 2.93ms | 3.17ms | 1.082× | 1.93% | PASS |
| mem_reads | `oltp_distinct_range` | 3.92ms | 4.22ms | 1.076× | 1.47% | PASS |
| mem_reads | `oltp_index_scan` | 4.36ms | 6.17ms | 1.417× | 1.89% | PASS |
| mem_reads | `select_random_points` | 16.92ms | 20.69ms | 1.223× | 1.42% | PASS |
| mem_reads | `select_random_ranges` | 3.78ms | 5.15ms | 1.364× | 2.02% | PASS |
| mem_reads | `covering_index_scan` | 4.51ms | 4.39ms | 0.974× | 1.33% | PASS |
| mem_reads | `groupby_scan` | 31.77ms | 34.42ms | 1.083× | 1.15% | PASS |
| mem_reads | `index_join` | 7.42ms | 9.86ms | 1.329× | 2.79% | PASS |
| mem_reads | `index_join_scan` | 4.58ms | 5.43ms | 1.187× | 3.02% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.23s | 1.175× | 0.88% | PASS |
| mem_reads | `table_scan` | 1.56s | 1.43s | 0.920× | 0.87% | PASS |
| mem_reads | `oltp_read_only` | 136.32ms | 145.58ms | 1.068× | 1.25% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.77ms | 364.25ms | 1.545× | 0.98% | PASS |
| mem_writes | `oltp_insert` | 24.09ms | 41.31ms | 1.715× | 1.48% | PASS |
| mem_writes | `oltp_update_index` | 81.88ms | 145.38ms | 1.775× | 1.98% | PASS |
| mem_writes | `oltp_update_non_index` | 51.24ms | 90.69ms | 1.770× | 1.34% | PASS |
| mem_writes | `oltp_delete_insert` | 57.22ms | 111.10ms | 1.942× | 1.36% | PASS |
| mem_writes | `oltp_write_only` | 31.72ms | 65.16ms | 2.054× | 1.73% | PASS |
| mem_writes | `types_delete_insert` | 34.52ms | 58.11ms | 1.684× | 1.66% | PASS |
| mem_writes | `oltp_read_write` | 99.77ms | 152.25ms | 1.526× | 2.85% | PASS |
| file_reads | `oltp_point_select` | 108.49ms | 65.53ms | 0.604× | 1.68% | PASS |
| file_reads | `oltp_range_select` | 22.63ms | 17.26ms | 0.763× | 2.11% | PASS |
| file_reads | `oltp_sum_range` | 21.40ms | 17.51ms | 0.818× | 1.14% | PASS |
| file_reads | `oltp_order_range` | 4.00ms | 3.56ms | 0.891× | 1.40% | PASS |
| file_reads | `oltp_distinct_range` | 5.12ms | 4.65ms | 0.910× | 1.24% | PASS |
| file_reads | `oltp_index_scan` | 12.57ms | 9.33ms | 0.742× | 2.28% | PASS |
| file_reads | `select_random_points` | 25.96ms | 24.42ms | 0.941× | 2.46% | PASS |
| file_reads | `select_random_ranges` | 11.78ms | 7.94ms | 0.674× | 1.36% | PASS |
| file_reads | `covering_index_scan` | 12.71ms | 7.45ms | 0.586× | 2.49% | PASS |
| file_reads | `groupby_scan` | 33.21ms | 34.90ms | 1.051× | 0.93% | PASS |
| file_reads | `index_join` | 11.86ms | 11.45ms | 0.966× | 1.82% | PASS |
| file_reads | `index_join_scan` | 5.74ms | 6.04ms | 1.052× | 1.65% | PASS |
| file_reads | `types_table_scan` | 1.22s | 1.26s | 1.035× | 3.48% | PASS |
| file_reads | `table_scan` | 1.51s | 1.41s | 0.938× | 2.75% | PASS |
| file_reads | `oltp_read_only` | 244.12ms | 182.72ms | 0.748× | 1.25% | PASS |
| file_writes | `oltp_bulk_insert` | 256.22ms | 390.24ms | 1.523× | 0.93% | PASS |
| file_writes | `oltp_insert` | 50.84ms | 53.24ms | 1.047× | 18.16% | PASS |
| file_writes | `oltp_update_index` | 114.72ms | 172.14ms | 1.501× | 2.25% | PASS |
| file_writes | `oltp_update_non_index` | 93.02ms | 113.63ms | 1.222× | 7.33% | PASS |
| file_writes | `oltp_delete_insert` | 93.73ms | 136.19ms | 1.453× | 1.72% | PASS |
| file_writes | `oltp_write_only` | 90.04ms | 86.58ms | 0.962× | 7.18% | PASS |
| file_writes | `types_delete_insert` | 55.86ms | 75.11ms | 1.344× | 1.26% | PASS |
| file_writes | `oltp_read_write` | 139.55ms | 164.78ms | 1.181× | 6.27% | PASS |
| ac_reads | `oltp_point_select` | 54.55ms | 63.26ms | 1.160× | 0.84% | PASS |
| ac_reads | `oltp_range_select` | 16.18ms | 16.98ms | 1.049× | 1.28% | PASS |
| ac_reads | `oltp_sum_range` | 15.03ms | 17.08ms | 1.137× | 1.09% | PASS |
| ac_reads | `oltp_order_range` | 3.39ms | 3.56ms | 1.047× | 1.66% | PASS |
| ac_reads | `oltp_distinct_range` | 4.42ms | 4.61ms | 1.043× | 1.06% | PASS |
| ac_reads | `oltp_index_scan` | 7.46ms | 9.18ms | 1.230× | 1.03% | PASS |
| ac_reads | `select_random_points` | 21.18ms | 24.49ms | 1.156× | 1.12% | PASS |
| ac_reads | `select_random_ranges` | 6.78ms | 7.89ms | 1.164× | 1.08% | PASS |
| ac_reads | `covering_index_scan` | 8.27ms | 7.42ms | 0.898× | 1.40% | PASS |
| ac_reads | `groupby_scan` | 32.80ms | 34.88ms | 1.063× | 0.89% | PASS |
| ac_reads | `index_join` | 9.25ms | 11.33ms | 1.225× | 2.68% | PASS |
| ac_reads | `index_join_scan` | 5.14ms | 5.91ms | 1.150× | 2.41% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.23s | 1.155× | 0.95% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.36s | 1.139× | 0.87% | PASS |
| ac_reads | `oltp_read_only` | 156.82ms | 176.80ms | 1.127× | 1.48% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.61ms | 63.88ms | 2.956× | 4.68% | PASS |
| ac_writes | `oltp_insert_ac` | 25.39ms | 79.73ms | 3.141× | 6.16% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.92ms | 97.62ms | 3.766× | 6.93% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.35ms | 76.62ms | 3.589× | 7.05% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.05ms | 88.17ms | 3.667× | 6.96% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.51ms | 87.16ms | 3.556× | 6.46% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.86ms | 79.60ms | 3.816× | 8.85% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.01ms | 96.65ms | 3.221× | 7.66% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.58ms | 34.71ms | 1.135× | 1.82% | PASS |
| mem_reads | `oltp_range_select` | 13.79ms | 13.68ms | 0.992× | 1.54% | PASS |
| mem_reads | `oltp_sum_range` | 12.34ms | 13.15ms | 1.066× | 1.25% | PASS |
| mem_reads | `oltp_order_range` | 3.12ms | 3.18ms | 1.022× | 0.89% | PASS |
| mem_reads | `oltp_distinct_range` | 4.17ms | 4.20ms | 1.007× | 0.89% | PASS |
| mem_reads | `oltp_index_scan` | 4.65ms | 6.04ms | 1.300× | 1.40% | PASS |
| mem_reads | `select_random_points` | 17.72ms | 19.76ms | 1.115× | 1.41% | PASS |
| mem_reads | `select_random_ranges` | 4.33ms | 5.29ms | 1.223× | 1.12% | PASS |
| mem_reads | `covering_index_scan` | 4.58ms | 4.66ms | 1.017× | 1.79% | PASS |
| mem_reads | `groupby_scan` | 34.34ms | 35.56ms | 1.036× | 0.92% | PASS |
| mem_reads | `index_join` | 7.13ms | 9.79ms | 1.372× | 2.09% | PASS |
| mem_reads | `index_join_scan` | 4.50ms | 5.80ms | 1.288× | 1.67% | PASS |
| mem_reads | `types_table_scan` | 1.27s | 1.31s | 1.027× | 1.00% | PASS |
| mem_reads | `table_scan` | 1.58s | 1.43s | 0.904× | 0.84% | PASS |
| mem_reads | `oltp_read_only` | 129.74ms | 134.07ms | 1.033× | 1.10% | PASS |
| mem_writes | `oltp_bulk_insert` | 238.26ms | 334.50ms | 1.404× | 0.92% | PASS |
| mem_writes | `oltp_insert` | 21.67ms | 40.36ms | 1.862× | 1.69% | PASS |
| mem_writes | `oltp_update_index` | 74.40ms | 138.38ms | 1.860× | 2.15% | PASS |
| mem_writes | `oltp_update_non_index` | 51.27ms | 86.99ms | 1.697× | 1.60% | PASS |
| mem_writes | `oltp_delete_insert` | 51.94ms | 107.15ms | 2.063× | 1.62% | PASS |
| mem_writes | `oltp_write_only` | 28.95ms | 64.97ms | 2.244× | 1.80% | PASS |
| mem_writes | `types_delete_insert` | 32.66ms | 53.53ms | 1.639× | 1.09% | PASS |
| mem_writes | `oltp_read_write` | 85.31ms | 136.60ms | 1.601× | 1.95% | PASS |
| file_reads | `oltp_point_select` | 127.25ms | 67.11ms | 0.527× | 0.91% | PASS |
| file_reads | `oltp_range_select` | 24.39ms | 17.07ms | 0.700× | 2.32% | PASS |
| file_reads | `oltp_sum_range` | 22.39ms | 16.43ms | 0.734× | 1.46% | PASS |
| file_reads | `oltp_order_range` | 4.05ms | 3.54ms | 0.873× | 1.92% | PASS |
| file_reads | `oltp_distinct_range` | 5.18ms | 4.53ms | 0.875× | 1.32% | PASS |
| file_reads | `oltp_index_scan` | 14.40ms | 9.41ms | 0.653× | 1.47% | PASS |
| file_reads | `select_random_points` | 28.26ms | 23.53ms | 0.833× | 2.49% | PASS |
| file_reads | `select_random_ranges` | 13.71ms | 8.46ms | 0.617× | 1.59% | PASS |
| file_reads | `covering_index_scan` | 14.55ms | 7.97ms | 0.548× | 1.45% | PASS |
| file_reads | `groupby_scan` | 34.52ms | 35.60ms | 1.031× | 0.56% | PASS |
| file_reads | `index_join` | 12.23ms | 11.38ms | 0.931× | 1.56% | PASS |
| file_reads | `index_join_scan` | 5.46ms | 6.19ms | 1.133× | 1.86% | PASS |
| file_reads | `types_table_scan` | 1.14s | 1.25s | 1.091× | 2.11% | PASS |
| file_reads | `table_scan` | 1.44s | 1.39s | 0.965× | 4.86% | PASS |
| file_reads | `oltp_read_only` | 260.64ms | 175.08ms | 0.672× | 0.92% | PASS |
| file_writes | `oltp_bulk_insert` | 261.52ms | 360.05ms | 1.377× | 0.90% | PASS |
| file_writes | `oltp_insert` | 31.28ms | 52.24ms | 1.670× | 1.65% | PASS |
| file_writes | `oltp_update_index` | 108.29ms | 169.65ms | 1.567× | 1.87% | PASS |
| file_writes | `oltp_update_non_index` | 80.92ms | 110.28ms | 1.363× | 0.94% | PASS |
| file_writes | `oltp_delete_insert` | 83.62ms | 133.68ms | 1.599× | 1.13% | PASS |
| file_writes | `oltp_write_only` | 56.02ms | 88.33ms | 1.577× | 1.48% | PASS |
| file_writes | `types_delete_insert` | 53.21ms | 73.13ms | 1.374× | 1.63% | PASS |
| file_writes | `oltp_read_write` | 112.59ms | 158.70ms | 1.410× | 1.28% | PASS |
| ac_reads | `oltp_point_select` | 62.07ms | 66.51ms | 1.071× | 0.99% | PASS |
| ac_reads | `oltp_range_select` | 17.54ms | 16.92ms | 0.965× | 1.12% | PASS |
| ac_reads | `oltp_sum_range` | 16.06ms | 16.49ms | 1.027× | 1.30% | PASS |
| ac_reads | `oltp_order_range` | 3.59ms | 3.54ms | 0.984× | 0.97% | PASS |
| ac_reads | `oltp_distinct_range` | 4.65ms | 4.55ms | 0.979× | 0.92% | PASS |
| ac_reads | `oltp_index_scan` | 8.42ms | 9.42ms | 1.118× | 1.28% | PASS |
| ac_reads | `select_random_points` | 22.95ms | 23.89ms | 1.041× | 2.01% | PASS |
| ac_reads | `select_random_ranges` | 7.75ms | 8.49ms | 1.095× | 1.02% | PASS |
| ac_reads | `covering_index_scan` | 8.84ms | 7.98ms | 0.903× | 0.94% | PASS |
| ac_reads | `groupby_scan` | 34.38ms | 35.64ms | 1.037× | 0.95% | PASS |
| ac_reads | `index_join` | 9.54ms | 11.50ms | 1.205× | 1.37% | PASS |
| ac_reads | `index_join_scan` | 5.08ms | 6.22ms | 1.223× | 1.63% | PASS |
| ac_reads | `types_table_scan` | 1.18s | 1.26s | 1.072× | 1.40% | PASS |
| ac_reads | `table_scan` | 1.57s | 1.43s | 0.909× | 1.05% | PASS |
| ac_reads | `oltp_read_only` | 174.84ms | 177.78ms | 1.017× | 0.88% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.75ms | 54.38ms | 3.246× | 5.73% | PASS |
| ac_writes | `oltp_insert_ac` | 19.83ms | 79.44ms | 4.006× | 6.81% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.27ms | 89.76ms | 4.221× | 4.01% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.41ms | 68.21ms | 3.918× | 5.94% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 19.13ms | 80.64ms | 4.216× | 6.78% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.25ms | 78.67ms | 4.086× | 4.46% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.88ms | 68.45ms | 4.055× | 6.89% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.63ms | 85.27ms | 3.327× | 3.77% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 27.04ms | 30.19ms | 1.116× | 2.00% | PASS |
| mem_reads | `oltp_range_select` | 15.88ms | 15.69ms | 0.988× | 1.70% | PASS |
| mem_reads | `oltp_sum_range` | 14.51ms | 14.93ms | 1.029× | 0.99% | PASS |
| mem_reads | `oltp_order_range` | 2.95ms | 2.95ms | 0.998× | 1.38% | PASS |
| mem_reads | `oltp_distinct_range` | 3.80ms | 3.76ms | 0.988× | 0.66% | PASS |
| mem_reads | `oltp_index_scan` | 3.78ms | 4.64ms | 1.228× | 2.03% | PASS |
| mem_reads | `select_random_points` | 21.87ms | 23.96ms | 1.096× | 1.00% | PASS |
| mem_reads | `select_random_ranges` | 6.06ms | 6.51ms | 1.074× | 1.30% | PASS |
| mem_reads | `covering_index_scan` | 3.41ms | 3.25ms | 0.951× | 1.26% | PASS |
| mem_reads | `groupby_scan` | 30.21ms | 30.95ms | 1.025× | 0.56% | PASS |
| mem_reads | `index_join` | 6.42ms | 7.77ms | 1.211× | 0.55% | PASS |
| mem_reads | `index_join_scan` | 3.52ms | 4.54ms | 1.292× | 2.16% | PASS |
| mem_reads | `types_table_scan` | 876.74ms | 974.03ms | 1.111× | 0.93% | PASS |
| mem_reads | `table_scan` | 1.01s | 1.06s | 1.052× | 1.14% | PASS |
| mem_reads | `oltp_read_only` | 120.98ms | 124.92ms | 1.033× | 1.43% | PASS |
| mem_writes | `oltp_bulk_insert` | 191.27ms | 258.20ms | 1.350× | 0.62% | PASS |
| mem_writes | `oltp_insert` | 15.21ms | 27.10ms | 1.782× | 0.59% | PASS |
| mem_writes | `oltp_update_index` | 53.59ms | 90.27ms | 1.684× | 1.09% | PASS |
| mem_writes | `oltp_update_non_index` | 40.90ms | 64.09ms | 1.567× | 1.45% | PASS |
| mem_writes | `oltp_delete_insert` | 40.83ms | 75.40ms | 1.847× | 1.20% | PASS |
| mem_writes | `oltp_write_only` | 22.50ms | 45.87ms | 2.038× | 0.92% | PASS |
| mem_writes | `types_delete_insert` | 25.91ms | 40.61ms | 1.567× | 1.31% | PASS |
| mem_writes | `oltp_read_write` | 81.81ms | 116.05ms | 1.418× | 2.25% | PASS |
| file_reads | `oltp_point_select` | 102.27ms | 55.15ms | 0.539× | 1.07% | PASS |
| file_reads | `oltp_range_select` | 23.91ms | 18.25ms | 0.763× | 1.24% | PASS |
| file_reads | `oltp_sum_range` | 22.47ms | 17.66ms | 0.786× | 0.91% | PASS |
| file_reads | `oltp_order_range` | 3.85ms | 3.26ms | 0.846× | 0.92% | PASS |
| file_reads | `oltp_distinct_range` | 4.68ms | 4.10ms | 0.876× | 1.15% | PASS |
| file_reads | `oltp_index_scan` | 11.57ms | 7.51ms | 0.649× | 1.21% | PASS |
| file_reads | `select_random_points` | 30.06ms | 26.73ms | 0.889× | 0.96% | PASS |
| file_reads | `select_random_ranges` | 13.75ms | 9.18ms | 0.668× | 0.76% | PASS |
| file_reads | `covering_index_scan` | 11.32ms | 6.22ms | 0.549× | 0.96% | PASS |
| file_reads | `groupby_scan` | 31.12ms | 31.44ms | 1.010× | 0.88% | PASS |
| file_reads | `index_join` | 10.72ms | 10.05ms | 0.938× | 0.90% | PASS |
| file_reads | `index_join_scan` | 4.37ms | 4.96ms | 1.135× | 1.32% | PASS |
| file_reads | `types_table_scan` | 869.72ms | 967.80ms | 1.113× | 0.63% | PASS |
| file_reads | `table_scan` | 1.07s | 1.07s | 1.005× | 2.77% | PASS |
| file_reads | `oltp_read_only` | 229.02ms | 160.62ms | 0.701× | 0.73% | PASS |
| file_writes | `oltp_bulk_insert` | 245.07ms | 328.06ms | 1.339× | 4.16% | PASS |
| file_writes | `oltp_insert` | 33.70ms | 50.62ms | 1.502× | 7.89% | PASS |
| file_writes | `oltp_update_index` | 171.24ms | 174.73ms | 1.020× | 17.64% | PASS |
| file_writes | `oltp_update_non_index` | 142.74ms | 125.85ms | 0.882× | 10.24% | PASS |
| file_writes | `oltp_delete_insert` | 139.15ms | 143.18ms | 1.029× | 16.24% | PASS |
| file_writes | `oltp_write_only` | 115.57ms | 108.59ms | 0.940× | 22.47% | PASS |
| file_writes | `types_delete_insert` | 88.58ms | 88.32ms | 0.997× | 13.23% | PASS |
| file_writes | `oltp_read_write` | 158.89ms | 171.96ms | 1.082× | 4.42% | PASS |
| ac_reads | `oltp_point_select` | 51.35ms | 55.11ms | 1.073× | 1.17% | PASS |
| ac_reads | `oltp_range_select` | 18.80ms | 18.32ms | 0.974× | 1.13% | PASS |
| ac_reads | `oltp_sum_range` | 17.41ms | 17.74ms | 1.019× | 0.83% | PASS |
| ac_reads | `oltp_order_range` | 3.40ms | 3.27ms | 0.961× | 0.64% | PASS |
| ac_reads | `oltp_distinct_range` | 4.19ms | 4.09ms | 0.975× | 0.88% | PASS |
| ac_reads | `oltp_index_scan` | 6.68ms | 7.53ms | 1.127× | 0.79% | PASS |
| ac_reads | `select_random_points` | 25.64ms | 27.25ms | 1.063× | 1.34% | PASS |
| ac_reads | `select_random_ranges` | 8.82ms | 9.21ms | 1.044× | 1.04% | PASS |
| ac_reads | `covering_index_scan` | 6.34ms | 6.24ms | 0.983× | 0.80% | PASS |
| ac_reads | `groupby_scan` | 30.70ms | 31.42ms | 1.024× | 0.90% | PASS |
| ac_reads | `index_join` | 8.28ms | 10.08ms | 1.218× | 1.00% | PASS |
| ac_reads | `index_join_scan` | 3.91ms | 4.96ms | 1.269× | 1.52% | PASS |
| ac_reads | `types_table_scan` | 899.42ms | 986.92ms | 1.097× | 1.77% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.10s | 0.932× | 3.13% | PASS |
| ac_reads | `oltp_read_only` | 159.56ms | 162.08ms | 1.016× | 1.23% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.04ms | 68.64ms | 3.425× | 22.72% | PASS |
| ac_writes | `oltp_insert_ac` | 23.38ms | 90.61ms | 3.875× | 62.19% | PASS |
| ac_writes | `oltp_update_index_ac` | 23.66ms | 101.30ms | 4.282× | 22.51% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 19.70ms | 104.71ms | 5.317× | 47.43% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.82ms | 125.53ms | 5.269× | 61.27% | PASS |
| ac_writes | `oltp_write_only_ac` | 21.68ms | 90.43ms | 4.170× | 40.56% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.17ms | 81.66ms | 4.049× | 35.78% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.76ms | 109.49ms | 3.243× | 55.75% | PASS |

</details>

## Version-control latency

Wall time: 2m 19s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 82.89ms | 200.00ms | 41.4% | 0.47% | PASS |
| `status_dirty_many_tables` | 85.93ms | 200.00ms | 43.0% | 0.48% | PASS |
| `diff_regular_working_one_table` | 79.32ms | 150.00ms | 52.9% | 0.83% | PASS |
| `diff_regular_working_many_tables` | 91.25ms | 200.00ms | 45.6% | 0.49% | PASS |
| `diff_stat_working_many_tables` | 91.33ms | 200.00ms | 45.7% | 0.39% | PASS |
| `diff_schema_working_many_tables` | 91.68ms | 200.00ms | 45.8% | 0.35% | PASS |
| `branch_list_many_branches` | 22.80ms | 100.00ms | 22.8% | 1.02% | PASS |
| `branch_create_delete` | 24.69ms | 100.00ms | 24.7% | 1.18% | PASS |
| `checkout_branch_clean` | 55.34ms | 200.00ms | 27.7% | 1.45% | PASS |
| `merge_data_no_conflicts` | 28.86ms | 150.00ms | 19.2% | 1.32% | PASS |
| `merge_schema_no_conflicts` | 21.66ms | 100.00ms | 21.7% | 1.41% | PASS |
| `merge_data_conflicts` | 126.53ms | 250.00ms | 50.6% | 0.31% | PASS |
| `merge_data_conflicts_with_resolve` | 126.76ms | 250.00ms | 50.7% | 0.36% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
