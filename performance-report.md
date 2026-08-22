# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-22 11:30 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260816.277.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/32566107069)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 58m 57s | 7.37s | 9.09s | 1.233× | 1.29% | **PASS** |
| textpk | 69 | 55 | 1h 34m 56s | 11.85s | 12.12s | 1.023× | 1.73% | **PASS** |
| blobpk | 69 | 55 | 1h 34m 3s | 10.60s | 12.26s | 1.157× | 2.24% | **PASS** |
| compositepk | 69 | 55 | 1h 28m 58s | 11.33s | 12.18s | 1.075× | 1.32% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 17.83ms | 19.96ms | 1.120× | 1.19% | PASS |
| mem_reads | `oltp_range_select` | 8.05ms | 9.61ms | 1.194× | 2.32% | PASS |
| mem_reads | `oltp_sum_range` | 7.39ms | 8.98ms | 1.215× | 2.11% | PASS |
| mem_reads | `oltp_order_range` | 2.12ms | 2.42ms | 1.142× | 1.34% | PASS |
| mem_reads | `oltp_distinct_range` | 2.92ms | 3.14ms | 1.076× | 1.35% | PASS |
| mem_reads | `oltp_index_scan` | 2.99ms | 3.67ms | 1.227× | 1.70% | PASS |
| mem_reads | `select_random_points` | 8.39ms | 8.96ms | 1.068× | 1.36% | PASS |
| mem_reads | `select_random_ranges` | 2.25ms | 2.92ms | 1.295× | 2.62% | PASS |
| mem_reads | `covering_index_scan` | 2.94ms | 2.97ms | 1.011× | 2.27% | PASS |
| mem_reads | `groupby_scan` | 25.02ms | 26.48ms | 1.058× | 0.47% | PASS |
| mem_reads | `index_join` | 4.55ms | 6.14ms | 1.351× | 0.92% | PASS |
| mem_reads | `index_join_scan` | 2.49ms | 3.62ms | 1.457× | 1.51% | PASS |
| mem_reads | `types_table_scan` | 853.08ms | 1.02s | 1.200× | 0.18% | PASS |
| mem_reads | `table_scan` | 964.55ms | 1.11s | 1.155× | 0.23% | PASS |
| mem_reads | `oltp_read_only` | 79.33ms | 91.03ms | 1.147× | 0.30% | PASS |
| mem_writes | `oltp_bulk_insert` | 120.81ms | 160.57ms | 1.329× | 0.46% | PASS |
| mem_writes | `oltp_insert` | 10.97ms | 19.00ms | 1.732× | 0.66% | PASS |
| mem_writes | `oltp_update_index` | 37.07ms | 74.57ms | 2.011× | 0.81% | PASS |
| mem_writes | `oltp_update_non_index` | 24.11ms | 41.36ms | 1.715× | 0.99% | PASS |
| mem_writes | `oltp_delete_insert` | 33.33ms | 54.26ms | 1.628× | 0.78% | PASS |
| mem_writes | `oltp_write_only` | 15.66ms | 31.28ms | 1.997× | 1.38% | PASS |
| mem_writes | `types_delete_insert` | 17.73ms | 27.94ms | 1.575× | 0.79% | PASS |
| mem_writes | `oltp_read_write` | 47.19ms | 76.19ms | 1.615× | 0.98% | PASS |
| file_reads | `oltp_point_select` | 43.48ms | 28.40ms | 0.653× | 0.75% | PASS |
| file_reads | `oltp_range_select` | 10.92ms | 10.84ms | 0.992× | 1.52% | PASS |
| file_reads | `oltp_sum_range` | 10.29ms | 10.21ms | 0.992× | 1.38% | PASS |
| file_reads | `oltp_order_range` | 2.45ms | 2.51ms | 1.025× | 1.04% | PASS |
| file_reads | `oltp_distinct_range` | 3.26ms | 3.31ms | 1.017× | 0.83% | PASS |
| file_reads | `oltp_index_scan` | 5.75ms | 4.63ms | 0.806× | 1.68% | PASS |
| file_reads | `select_random_points` | 11.49ms | 10.26ms | 0.893× | 1.24% | PASS |
| file_reads | `select_random_ranges` | 4.91ms | 3.77ms | 0.769× | 1.17% | PASS |
| file_reads | `covering_index_scan` | 5.71ms | 3.90ms | 0.682× | 1.07% | PASS |
| file_reads | `groupby_scan` | 25.09ms | 26.62ms | 1.061× | 0.53% | PASS |
| file_reads | `index_join` | 6.08ms | 6.81ms | 1.120× | 1.29% | PASS |
| file_reads | `index_join_scan` | 2.85ms | 3.81ms | 1.336× | 1.07% | PASS |
| file_reads | `types_table_scan` | 851.22ms | 1.03s | 1.205× | 0.29% | PASS |
| file_reads | `table_scan` | 964.86ms | 1.12s | 1.157× | 0.31% | PASS |
| file_reads | `oltp_read_only` | 115.63ms | 103.55ms | 0.896× | 0.27% | PASS |
| file_writes | `oltp_bulk_insert` | 157.86ms | 210.39ms | 1.333× | 2.21% | PASS |
| file_writes | `oltp_insert` | 19.89ms | 29.03ms | 1.459× | 2.63% | PASS |
| file_writes | `oltp_update_index` | 123.39ms | 129.30ms | 1.048× | 5.14% | PASS |
| file_writes | `oltp_update_non_index` | 97.41ms | 89.25ms | 0.916× | 1.85% | PASS |
| file_writes | `oltp_delete_insert` | 103.36ms | 102.67ms | 0.993× | 4.74% | PASS |
| file_writes | `oltp_write_only` | 80.15ms | 69.62ms | 0.869× | 7.18% | PASS |
| file_writes | `types_delete_insert` | 60.17ms | 51.31ms | 0.853× | 3.86% | PASS |
| file_writes | `oltp_read_write` | 111.08ms | 112.80ms | 1.015× | 2.85% | PASS |
| ac_reads | `oltp_point_select` | 26.36ms | 28.48ms | 1.081× | 0.59% | PASS |
| ac_reads | `oltp_range_select` | 9.27ms | 10.84ms | 1.168× | 1.43% | PASS |
| ac_reads | `oltp_sum_range` | 8.77ms | 10.16ms | 1.159× | 2.02% | PASS |
| ac_reads | `oltp_order_range` | 2.27ms | 2.51ms | 1.106× | 1.01% | PASS |
| ac_reads | `oltp_distinct_range` | 3.07ms | 3.30ms | 1.075× | 1.23% | PASS |
| ac_reads | `oltp_index_scan` | 4.04ms | 4.62ms | 1.144× | 2.29% | PASS |
| ac_reads | `select_random_points` | 9.99ms | 10.28ms | 1.029× | 1.36% | PASS |
| ac_reads | `select_random_ranges` | 3.22ms | 3.76ms | 1.168× | 1.47% | PASS |
| ac_reads | `covering_index_scan` | 3.91ms | 3.87ms | 0.990× | 1.26% | PASS |
| ac_reads | `groupby_scan` | 25.11ms | 26.61ms | 1.059× | 0.71% | PASS |
| ac_reads | `index_join` | 5.26ms | 6.78ms | 1.289× | 1.16% | PASS |
| ac_reads | `index_join_scan` | 2.76ms | 3.84ms | 1.392× | 1.20% | PASS |
| ac_reads | `types_table_scan` | 850.83ms | 1.03s | 1.206× | 0.18% | PASS |
| ac_reads | `table_scan` | 965.95ms | 1.12s | 1.156× | 0.34% | PASS |
| ac_reads | `oltp_read_only` | 90.89ms | 103.67ms | 1.141× | 0.43% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 30.60ms | 81.61ms | 2.667× | 7.03% | PASS |
| ac_writes | `oltp_insert_ac` | 31.64ms | 93.77ms | 2.964× | 4.86% | PASS |
| ac_writes | `oltp_update_index_ac` | 32.87ms | 106.49ms | 3.239× | 6.61% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 30.69ms | 87.70ms | 2.858× | 4.50% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 31.53ms | 99.34ms | 3.150× | 6.81% | PASS |
| ac_writes | `oltp_write_only_ac` | 31.70ms | 99.18ms | 3.129× | 6.20% | PASS |
| ac_writes | `types_delete_insert_ac` | 30.23ms | 91.16ms | 3.015× | 8.27% | PASS |
| ac_writes | `oltp_read_write_ac` | 34.40ms | 106.13ms | 3.085× | 5.14% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.02ms | 35.30ms | 1.138× | 3.08% | PASS |
| mem_reads | `oltp_range_select` | 14.63ms | 13.73ms | 0.939× | 4.52% | PASS |
| mem_reads | `oltp_sum_range` | 12.93ms | 13.32ms | 1.030× | 2.95% | PASS |
| mem_reads | `oltp_order_range` | 3.21ms | 3.17ms | 0.988× | 1.18% | PASS |
| mem_reads | `oltp_distinct_range` | 4.26ms | 4.19ms | 0.983× | 0.90% | PASS |
| mem_reads | `oltp_index_scan` | 4.78ms | 6.08ms | 1.272× | 2.05% | PASS |
| mem_reads | `select_random_points` | 18.79ms | 20.00ms | 1.065× | 3.05% | PASS |
| mem_reads | `select_random_ranges` | 4.14ms | 5.24ms | 1.266× | 1.87% | PASS |
| mem_reads | `covering_index_scan` | 4.82ms | 4.44ms | 0.922× | 1.91% | PASS |
| mem_reads | `groupby_scan` | 34.28ms | 35.57ms | 1.038× | 1.38% | PASS |
| mem_reads | `index_join` | 7.14ms | 8.99ms | 1.259× | 1.25% | PASS |
| mem_reads | `index_join_scan` | 5.53ms | 5.72ms | 1.035× | 2.64% | PASS |
| mem_reads | `types_table_scan` | 1.23s | 1.29s | 1.044× | 4.91% | PASS |
| mem_reads | `table_scan` | 1.63s | 1.42s | 0.870× | 2.87% | PASS |
| mem_reads | `oltp_read_only` | 125.22ms | 128.99ms | 1.030× | 2.83% | PASS |
| mem_writes | `oltp_bulk_insert` | 231.86ms | 342.25ms | 1.476× | 0.64% | PASS |
| mem_writes | `oltp_insert` | 24.16ms | 40.22ms | 1.664× | 2.07% | PASS |
| mem_writes | `oltp_update_index` | 81.80ms | 143.63ms | 1.756× | 1.67% | PASS |
| mem_writes | `oltp_update_non_index` | 52.84ms | 89.84ms | 1.700× | 1.49% | PASS |
| mem_writes | `oltp_delete_insert` | 58.69ms | 111.89ms | 1.906× | 1.73% | PASS |
| mem_writes | `oltp_write_only` | 32.37ms | 66.22ms | 2.046× | 1.56% | PASS |
| mem_writes | `types_delete_insert` | 35.53ms | 56.22ms | 1.582× | 0.94% | PASS |
| mem_writes | `oltp_read_write` | 99.65ms | 145.99ms | 1.465× | 2.01% | PASS |
| file_reads | `oltp_point_select` | 128.37ms | 67.79ms | 0.528× | 0.87% | PASS |
| file_reads | `oltp_range_select` | 26.25ms | 17.32ms | 0.660× | 2.13% | PASS |
| file_reads | `oltp_sum_range` | 23.66ms | 17.04ms | 0.720× | 1.65% | PASS |
| file_reads | `oltp_order_range` | 4.36ms | 3.57ms | 0.819× | 1.20% | PASS |
| file_reads | `oltp_distinct_range` | 5.38ms | 4.57ms | 0.850× | 1.08% | PASS |
| file_reads | `oltp_index_scan` | 14.90ms | 9.52ms | 0.639× | 1.08% | PASS |
| file_reads | `select_random_points` | 29.85ms | 24.29ms | 0.814× | 1.71% | PASS |
| file_reads | `select_random_ranges` | 14.10ms | 8.57ms | 0.608× | 0.98% | PASS |
| file_reads | `covering_index_scan` | 16.01ms | 8.08ms | 0.505× | 1.32% | PASS |
| file_reads | `groupby_scan` | 35.81ms | 36.25ms | 1.012× | 0.65% | PASS |
| file_reads | `index_join` | 13.48ms | 11.55ms | 0.857× | 1.87% | PASS |
| file_reads | `index_join_scan` | 6.27ms | 6.34ms | 1.011× | 2.88% | PASS |
| file_reads | `types_table_scan` | 1.37s | 1.32s | 0.958× | 2.17% | PASS |
| file_reads | `table_scan` | 1.71s | 1.44s | 0.841× | 1.57% | PASS |
| file_reads | `oltp_read_only` | 271.82ms | 179.13ms | 0.659× | 1.08% | PASS |
| file_writes | `oltp_bulk_insert` | 253.87ms | 372.59ms | 1.468× | 0.91% | PASS |
| file_writes | `oltp_insert` | 56.85ms | 53.55ms | 0.942× | 24.21% | PASS |
| file_writes | `oltp_update_index` | 125.30ms | 182.01ms | 1.453× | 1.39% | PASS |
| file_writes | `oltp_update_non_index` | 114.88ms | 120.99ms | 1.053× | 10.83% | PASS |
| file_writes | `oltp_delete_insert` | 100.72ms | 145.66ms | 1.446× | 1.87% | PASS |
| file_writes | `oltp_write_only` | 87.56ms | 91.18ms | 1.041× | 15.86% | PASS |
| file_writes | `types_delete_insert` | 58.54ms | 77.25ms | 1.320× | 1.57% | PASS |
| file_writes | `oltp_read_write` | 161.81ms | 168.15ms | 1.039× | 6.06% | PASS |
| ac_reads | `oltp_point_select` | 63.48ms | 68.13ms | 1.073× | 1.52% | PASS |
| ac_reads | `oltp_range_select` | 18.88ms | 17.17ms | 0.910× | 2.54% | PASS |
| ac_reads | `oltp_sum_range` | 16.92ms | 16.85ms | 0.996× | 3.01% | PASS |
| ac_reads | `oltp_order_range` | 3.75ms | 3.56ms | 0.949× | 0.76% | PASS |
| ac_reads | `oltp_distinct_range` | 4.74ms | 4.60ms | 0.970× | 0.93% | PASS |
| ac_reads | `oltp_index_scan` | 8.52ms | 9.59ms | 1.126× | 0.94% | PASS |
| ac_reads | `select_random_points` | 23.37ms | 24.51ms | 1.049× | 1.03% | PASS |
| ac_reads | `select_random_ranges` | 7.70ms | 8.59ms | 1.116× | 0.73% | PASS |
| ac_reads | `covering_index_scan` | 9.62ms | 8.08ms | 0.840× | 0.98% | PASS |
| ac_reads | `groupby_scan` | 35.05ms | 36.17ms | 1.032× | 0.64% | PASS |
| ac_reads | `index_join` | 10.00ms | 11.42ms | 1.141× | 2.59% | PASS |
| ac_reads | `index_join_scan` | 5.59ms | 6.14ms | 1.098× | 1.16% | PASS |
| ac_reads | `types_table_scan` | 1.19s | 1.26s | 1.055× | 3.45% | PASS |
| ac_reads | `table_scan` | 1.72s | 1.44s | 0.840× | 0.71% | PASS |
| ac_reads | `oltp_read_only` | 183.75ms | 183.46ms | 0.998× | 1.05% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.67ms | 63.02ms | 3.781× | 3.60% | PASS |
| ac_writes | `oltp_insert_ac` | 19.20ms | 79.44ms | 4.136× | 3.09% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.71ms | 98.79ms | 4.550× | 4.57% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.27ms | 75.63ms | 4.649× | 6.00% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.87ms | 90.68ms | 4.805× | 5.31% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.10ms | 86.30ms | 4.519× | 3.85% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.96ms | 78.62ms | 4.925× | 4.55% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.34ms | 95.51ms | 3.625× | 3.82% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.24ms | 36.48ms | 1.248× | 1.32% | PASS |
| mem_reads | `oltp_range_select` | 12.15ms | 13.85ms | 1.140× | 2.33% | PASS |
| mem_reads | `oltp_sum_range` | 11.32ms | 13.91ms | 1.229× | 2.44% | PASS |
| mem_reads | `oltp_order_range` | 2.76ms | 3.08ms | 1.117× | 1.65% | PASS |
| mem_reads | `oltp_distinct_range` | 3.86ms | 4.15ms | 1.075× | 1.15% | PASS |
| mem_reads | `oltp_index_scan` | 4.30ms | 6.02ms | 1.401× | 1.82% | PASS |
| mem_reads | `select_random_points` | 16.89ms | 20.38ms | 1.206× | 1.79% | PASS |
| mem_reads | `select_random_ranges` | 3.71ms | 5.05ms | 1.362× | 2.20% | PASS |
| mem_reads | `covering_index_scan` | 4.37ms | 4.33ms | 0.993× | 0.99% | PASS |
| mem_reads | `groupby_scan` | 31.07ms | 33.50ms | 1.078× | 0.77% | PASS |
| mem_reads | `index_join` | 6.79ms | 9.18ms | 1.352× | 2.16% | PASS |
| mem_reads | `index_join_scan` | 4.35ms | 5.38ms | 1.238× | 4.31% | PASS |
| mem_reads | `types_table_scan` | 1.16s | 1.27s | 1.094× | 1.88% | PASS |
| mem_reads | `table_scan` | 1.45s | 1.43s | 0.993× | 0.71% | PASS |
| mem_reads | `oltp_read_only` | 132.26ms | 142.80ms | 1.080× | 1.54% | PASS |
| mem_writes | `oltp_bulk_insert` | 242.59ms | 362.81ms | 1.496× | 0.77% | PASS |
| mem_writes | `oltp_insert` | 21.78ms | 40.89ms | 1.877× | 4.30% | PASS |
| mem_writes | `oltp_update_index` | 75.32ms | 139.88ms | 1.857× | 4.13% | PASS |
| mem_writes | `oltp_update_non_index` | 52.35ms | 89.19ms | 1.704× | 1.86% | PASS |
| mem_writes | `oltp_delete_insert` | 55.56ms | 110.62ms | 1.991× | 2.29% | PASS |
| mem_writes | `oltp_write_only` | 31.55ms | 65.55ms | 2.078× | 2.77% | PASS |
| mem_writes | `types_delete_insert` | 34.62ms | 57.08ms | 1.649× | 1.78% | PASS |
| mem_writes | `oltp_read_write` | 95.50ms | 147.72ms | 1.547× | 1.67% | PASS |
| file_reads | `oltp_point_select` | 110.07ms | 65.39ms | 0.594× | 1.16% | PASS |
| file_reads | `oltp_range_select` | 23.27ms | 17.46ms | 0.750× | 1.60% | PASS |
| file_reads | `oltp_sum_range` | 21.71ms | 17.51ms | 0.806× | 2.97% | PASS |
| file_reads | `oltp_order_range` | 4.02ms | 3.57ms | 0.890× | 2.85% | PASS |
| file_reads | `oltp_distinct_range` | 5.10ms | 4.62ms | 0.907× | 1.76% | PASS |
| file_reads | `oltp_index_scan` | 13.13ms | 9.39ms | 0.715× | 1.79% | PASS |
| file_reads | `select_random_points` | 29.31ms | 25.79ms | 0.880× | 1.81% | PASS |
| file_reads | `select_random_ranges` | 12.26ms | 7.97ms | 0.651× | 1.48% | PASS |
| file_reads | `covering_index_scan` | 13.57ms | 7.53ms | 0.555× | 2.23% | PASS |
| file_reads | `groupby_scan` | 33.77ms | 34.70ms | 1.028× | 0.71% | PASS |
| file_reads | `index_join` | 12.45ms | 11.81ms | 0.948× | 2.46% | PASS |
| file_reads | `index_join_scan` | 5.78ms | 6.23ms | 1.077× | 3.16% | PASS |
| file_reads | `types_table_scan` | 1.17s | 1.28s | 1.089× | 1.43% | PASS |
| file_reads | `table_scan` | 1.43s | 1.43s | 0.997× | 1.21% | PASS |
| file_reads | `oltp_read_only` | 245.71ms | 182.20ms | 0.742× | 1.36% | PASS |
| file_writes | `oltp_bulk_insert` | 262.10ms | 388.02ms | 1.480× | 0.88% | PASS |
| file_writes | `oltp_insert` | 33.77ms | 53.65ms | 1.589× | 2.87% | PASS |
| file_writes | `oltp_update_index` | 115.68ms | 175.47ms | 1.517× | 2.51% | PASS |
| file_writes | `oltp_update_non_index` | 83.61ms | 114.27ms | 1.367× | 1.82% | PASS |
| file_writes | `oltp_delete_insert` | 88.69ms | 136.99ms | 1.545× | 2.89% | PASS |
| file_writes | `oltp_write_only` | 59.95ms | 88.13ms | 1.470× | 1.88% | PASS |
| file_writes | `types_delete_insert` | 56.02ms | 75.72ms | 1.352× | 2.24% | PASS |
| file_writes | `oltp_read_write` | 127.24ms | 173.38ms | 1.363× | 3.40% | PASS |
| ac_reads | `oltp_point_select` | 57.52ms | 64.64ms | 1.124× | 1.52% | PASS |
| ac_reads | `oltp_range_select` | 17.42ms | 17.07ms | 0.980× | 3.38% | PASS |
| ac_reads | `oltp_sum_range` | 16.05ms | 17.24ms | 1.075× | 2.90% | PASS |
| ac_reads | `oltp_order_range` | 3.47ms | 3.53ms | 1.017× | 2.28% | PASS |
| ac_reads | `oltp_distinct_range` | 4.51ms | 4.62ms | 1.024× | 1.11% | PASS |
| ac_reads | `oltp_index_scan` | 7.78ms | 9.31ms | 1.197× | 2.84% | PASS |
| ac_reads | `select_random_points` | 22.81ms | 25.84ms | 1.133× | 3.36% | PASS |
| ac_reads | `select_random_ranges` | 7.10ms | 7.91ms | 1.115× | 2.78% | PASS |
| ac_reads | `covering_index_scan` | 8.24ms | 7.44ms | 0.903× | 2.93% | PASS |
| ac_reads | `groupby_scan` | 32.85ms | 34.65ms | 1.055× | 0.90% | PASS |
| ac_reads | `index_join` | 9.61ms | 11.72ms | 1.219× | 2.63% | PASS |
| ac_reads | `index_join_scan` | 5.06ms | 6.08ms | 1.202× | 5.13% | PASS |
| ac_reads | `types_table_scan` | 1.16s | 1.28s | 1.107× | 2.88% | PASS |
| ac_reads | `table_scan` | 1.42s | 1.43s | 1.004× | 1.36% | PASS |
| ac_reads | `oltp_read_only` | 168.22ms | 182.34ms | 1.084× | 2.89% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.84ms | 82.45ms | 3.610× | 6.66% | PASS |
| ac_writes | `oltp_insert_ac` | 27.96ms | 107.05ms | 3.829× | 6.04% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.72ms | 118.72ms | 3.995× | 7.66% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.55ms | 96.47ms | 3.929× | 5.52% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.44ms | 109.28ms | 4.133× | 7.01% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.93ms | 109.21ms | 4.056× | 7.64% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.50ms | 99.10ms | 4.216× | 4.29% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.31ms | 113.46ms | 3.512× | 6.03% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 35.35ms | 39.26ms | 1.111× | 1.62% | PASS |
| mem_reads | `oltp_range_select` | 21.07ms | 20.58ms | 0.977× | 1.42% | PASS |
| mem_reads | `oltp_sum_range` | 19.16ms | 19.34ms | 1.010× | 1.38% | PASS |
| mem_reads | `oltp_order_range` | 3.81ms | 4.15ms | 1.090× | 0.96% | PASS |
| mem_reads | `oltp_distinct_range` | 4.82ms | 5.16ms | 1.070× | 1.20% | PASS |
| mem_reads | `oltp_index_scan` | 4.76ms | 6.05ms | 1.272× | 1.76% | PASS |
| mem_reads | `select_random_points` | 28.90ms | 30.75ms | 1.064× | 1.72% | PASS |
| mem_reads | `select_random_ranges` | 7.85ms | 8.36ms | 1.065× | 1.24% | PASS |
| mem_reads | `covering_index_scan` | 4.48ms | 4.74ms | 1.057× | 2.58% | PASS |
| mem_reads | `groupby_scan` | 39.14ms | 40.33ms | 1.030× | 0.77% | PASS |
| mem_reads | `index_join` | 8.32ms | 10.61ms | 1.275× | 2.02% | PASS |
| mem_reads | `index_join_scan` | 4.46ms | 5.83ms | 1.308× | 1.57% | PASS |
| mem_reads | `types_table_scan` | 1.24s | 1.30s | 1.043× | 1.31% | PASS |
| mem_reads | `table_scan` | 1.51s | 1.41s | 0.931× | 0.81% | PASS |
| mem_reads | `oltp_read_only` | 161.66ms | 171.19ms | 1.059× | 1.13% | PASS |
| mem_writes | `oltp_bulk_insert` | 244.87ms | 343.36ms | 1.402× | 1.01% | PASS |
| mem_writes | `oltp_insert` | 19.72ms | 36.97ms | 1.874× | 0.85% | PASS |
| mem_writes | `oltp_update_index` | 75.29ms | 129.90ms | 1.725× | 2.01% | PASS |
| mem_writes | `oltp_update_non_index` | 55.23ms | 88.24ms | 1.598× | 1.30% | PASS |
| mem_writes | `oltp_delete_insert` | 53.04ms | 101.40ms | 1.912× | 1.25% | PASS |
| mem_writes | `oltp_write_only` | 29.00ms | 61.92ms | 2.135× | 1.31% | PASS |
| mem_writes | `types_delete_insert` | 34.27ms | 55.49ms | 1.619× | 1.55% | PASS |
| mem_writes | `oltp_read_write` | 108.22ms | 153.20ms | 1.416× | 2.41% | PASS |
| file_reads | `oltp_point_select` | 131.87ms | 70.85ms | 0.537× | 1.07% | PASS |
| file_reads | `oltp_range_select` | 31.13ms | 23.91ms | 0.768× | 1.22% | PASS |
| file_reads | `oltp_sum_range` | 29.36ms | 22.82ms | 0.777× | 1.32% | PASS |
| file_reads | `oltp_order_range` | 4.87ms | 4.47ms | 0.917× | 1.10% | PASS |
| file_reads | `oltp_distinct_range` | 5.91ms | 5.51ms | 0.932× | 0.96% | PASS |
| file_reads | `oltp_index_scan` | 14.97ms | 9.59ms | 0.641× | 1.39% | PASS |
| file_reads | `select_random_points` | 40.35ms | 34.90ms | 0.865× | 1.72% | PASS |
| file_reads | `select_random_ranges` | 17.73ms | 11.79ms | 0.665× | 1.04% | PASS |
| file_reads | `covering_index_scan` | 14.62ms | 7.95ms | 0.544× | 1.11% | PASS |
| file_reads | `groupby_scan` | 40.28ms | 41.02ms | 1.018× | 0.65% | PASS |
| file_reads | `index_join` | 14.06ms | 13.04ms | 0.927× | 2.76% | PASS |
| file_reads | `index_join_scan` | 5.84ms | 6.38ms | 1.093× | 2.29% | PASS |
| file_reads | `types_table_scan` | 1.24s | 1.28s | 1.034× | 0.94% | PASS |
| file_reads | `table_scan` | 1.54s | 1.41s | 0.913× | 0.81% | PASS |
| file_reads | `oltp_read_only` | 299.26ms | 216.31ms | 0.723× | 0.91% | PASS |
| file_writes | `oltp_bulk_insert` | 262.05ms | 367.23ms | 1.401× | 1.38% | PASS |
| file_writes | `oltp_insert` | 27.04ms | 47.41ms | 1.754× | 1.44% | PASS |
| file_writes | `oltp_update_index` | 105.40ms | 152.82ms | 1.450× | 1.25% | PASS |
| file_writes | `oltp_update_non_index` | 82.77ms | 110.08ms | 1.330× | 1.11% | PASS |
| file_writes | `oltp_delete_insert` | 82.54ms | 125.81ms | 1.524× | 1.94% | PASS |
| file_writes | `oltp_write_only` | 54.70ms | 83.38ms | 1.524× | 2.10% | PASS |
| file_writes | `types_delete_insert` | 53.40ms | 69.88ms | 1.309× | 1.34% | PASS |
| file_writes | `oltp_read_write` | 136.08ms | 175.46ms | 1.289× | 1.52% | PASS |
| ac_reads | `oltp_point_select` | 66.84ms | 71.19ms | 1.065× | 1.08% | PASS |
| ac_reads | `oltp_range_select` | 24.90ms | 23.90ms | 0.960× | 1.10% | PASS |
| ac_reads | `oltp_sum_range` | 22.76ms | 22.96ms | 1.009× | 1.36% | PASS |
| ac_reads | `oltp_order_range` | 4.25ms | 4.46ms | 1.050× | 0.69% | PASS |
| ac_reads | `oltp_distinct_range` | 5.28ms | 5.54ms | 1.049× | 0.89% | PASS |
| ac_reads | `oltp_index_scan` | 8.47ms | 9.53ms | 1.125× | 1.27% | PASS |
| ac_reads | `select_random_points` | 33.24ms | 34.64ms | 1.042× | 1.79% | PASS |
| ac_reads | `select_random_ranges` | 11.25ms | 11.77ms | 1.047× | 0.99% | PASS |
| ac_reads | `covering_index_scan` | 8.17ms | 8.02ms | 0.981× | 1.45% | PASS |
| ac_reads | `groupby_scan` | 39.45ms | 41.01ms | 1.040× | 0.66% | PASS |
| ac_reads | `index_join` | 11.17ms | 13.33ms | 1.194× | 1.92% | PASS |
| ac_reads | `index_join_scan` | 5.18ms | 6.53ms | 1.261× | 4.07% | PASS |
| ac_reads | `types_table_scan` | 1.22s | 1.28s | 1.046× | 1.22% | PASS |
| ac_reads | `table_scan` | 1.54s | 1.41s | 0.914× | 0.70% | PASS |
| ac_reads | `oltp_read_only` | 206.09ms | 216.08ms | 1.048× | 1.32% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.78ms | 66.07ms | 3.938× | 5.07% | PASS |
| ac_writes | `oltp_insert_ac` | 19.89ms | 89.13ms | 4.482× | 6.11% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.50ms | 100.98ms | 4.697× | 6.20% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.82ms | 78.47ms | 4.404× | 4.87% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 19.36ms | 89.87ms | 4.642× | 5.45% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.79ms | 89.89ms | 4.543× | 5.46% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.79ms | 78.61ms | 4.683× | 6.19% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.97ms | 98.48ms | 3.652× | 3.87% | PASS |

</details>

## Version-control latency

Wall time: 2m 0s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 60.62ms | 200.00ms | 30.3% | 2.28% | PASS |
| `status_dirty_many_tables` | 63.46ms | 200.00ms | 31.7% | 2.59% | PASS |
| `diff_regular_working_one_table` | 56.78ms | 150.00ms | 37.9% | 1.72% | PASS |
| `diff_regular_working_many_tables` | 67.33ms | 200.00ms | 33.7% | 1.20% | PASS |
| `diff_stat_working_many_tables` | 67.78ms | 200.00ms | 33.9% | 1.25% | PASS |
| `diff_schema_working_many_tables` | 67.36ms | 200.00ms | 33.7% | 1.69% | PASS |
| `branch_list_many_branches` | 18.49ms | 100.00ms | 18.5% | 3.08% | PASS |
| `branch_create_delete` | 28.94ms | 100.00ms | 28.9% | 4.34% | PASS |
| `checkout_branch_clean` | 89.73ms | 200.00ms | 44.9% | 2.52% | PASS |
| `merge_data_no_conflicts` | 53.24ms | 150.00ms | 35.5% | 36.80% | PASS |
| `merge_schema_no_conflicts` | 19.85ms | 100.00ms | 19.9% | 6.39% | PASS |
| `merge_data_conflicts` | 69.06ms | 250.00ms | 27.6% | 1.72% | PASS |
| `merge_data_conflicts_with_resolve` | 68.64ms | 250.00ms | 27.5% | 1.62% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
