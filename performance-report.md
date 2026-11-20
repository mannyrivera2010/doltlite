# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-20 19:31 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu26 20261116.202.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/44788033205)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 54s | 9.20s | 11.69s | 1.271× | 1.73% | **PASS** |
| textpk | 69 | 55 | 1h 31m 40s | 9.49s | 11.64s | 1.226× | 1.87% | **PASS** |
| blobpk | 69 | 55 | 1h 10m 52s | 7.69s | 8.14s | 1.059× | 3.02% | **PASS** |
| compositepk | 69 | 55 | 1h 25m 32s | 10.17s | 11.47s | 1.128× | 0.86% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.79ms | 29.39ms | 1.186× | 2.59% | PASS |
| mem_reads | `oltp_range_select` | 11.19ms | 12.31ms | 1.100× | 1.61% | PASS |
| mem_reads | `oltp_sum_range` | 10.27ms | 12.22ms | 1.191× | 1.72% | PASS |
| mem_reads | `oltp_order_range` | 2.77ms | 3.08ms | 1.111× | 1.35% | PASS |
| mem_reads | `oltp_distinct_range` | 3.74ms | 4.24ms | 1.132× | 1.09% | PASS |
| mem_reads | `oltp_index_scan` | 4.15ms | 5.52ms | 1.329× | 2.22% | PASS |
| mem_reads | `select_random_points` | 11.87ms | 11.52ms | 0.971× | 3.63% | PASS |
| mem_reads | `select_random_ranges` | 3.25ms | 4.01ms | 1.235× | 1.67% | PASS |
| mem_reads | `covering_index_scan` | 4.16ms | 4.29ms | 1.031× | 2.34% | PASS |
| mem_reads | `groupby_scan` | 31.45ms | 36.04ms | 1.146× | 1.83% | PASS |
| mem_reads | `index_join` | 6.06ms | 8.16ms | 1.346× | 1.60% | PASS |
| mem_reads | `index_join_scan` | 3.49ms | 4.61ms | 1.319× | 2.23% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.32s | 1.255× | 1.05% | PASS |
| mem_reads | `table_scan` | 1.28s | 1.48s | 1.158× | 0.61% | PASS |
| mem_reads | `oltp_read_only` | 119.53ms | 131.45ms | 1.100× | 1.09% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.85ms | 258.41ms | 1.437× | 1.11% | PASS |
| mem_writes | `oltp_insert` | 15.71ms | 29.10ms | 1.852× | 1.39% | PASS |
| mem_writes | `oltp_update_index` | 56.23ms | 118.07ms | 2.100× | 1.71% | PASS |
| mem_writes | `oltp_update_non_index` | 37.73ms | 65.69ms | 1.741× | 2.09% | PASS |
| mem_writes | `oltp_delete_insert` | 48.27ms | 86.18ms | 1.786× | 1.62% | PASS |
| mem_writes | `oltp_write_only` | 23.50ms | 49.60ms | 2.111× | 1.73% | PASS |
| mem_writes | `types_delete_insert` | 27.19ms | 44.79ms | 1.647× | 1.94% | PASS |
| mem_writes | `oltp_read_write` | 80.54ms | 120.84ms | 1.500× | 1.93% | PASS |
| file_reads | `oltp_point_select` | 94.65ms | 53.71ms | 0.568× | 1.66% | PASS |
| file_reads | `oltp_range_select` | 17.92ms | 14.77ms | 0.824× | 2.74% | PASS |
| file_reads | `oltp_sum_range` | 17.22ms | 14.77ms | 0.857× | 2.01% | PASS |
| file_reads | `oltp_order_range` | 3.53ms | 3.38ms | 0.956× | 1.11% | PASS |
| file_reads | `oltp_distinct_range` | 4.59ms | 4.55ms | 0.992× | 1.51% | PASS |
| file_reads | `oltp_index_scan` | 11.15ms | 8.14ms | 0.730× | 1.80% | PASS |
| file_reads | `select_random_points` | 19.94ms | 14.34ms | 0.719× | 2.26% | PASS |
| file_reads | `select_random_ranges` | 10.20ms | 6.41ms | 0.629× | 1.67% | PASS |
| file_reads | `covering_index_scan` | 11.31ms | 6.94ms | 0.613× | 1.75% | PASS |
| file_reads | `groupby_scan` | 32.84ms | 35.80ms | 1.090× | 2.20% | PASS |
| file_reads | `index_join` | 10.24ms | 10.28ms | 1.003× | 1.73% | PASS |
| file_reads | `index_join_scan` | 4.51ms | 5.00ms | 1.109× | 3.19% | PASS |
| file_reads | `types_table_scan` | 1.07s | 1.35s | 1.265× | 1.10% | PASS |
| file_reads | `table_scan` | 1.19s | 1.44s | 1.217× | 1.56% | PASS |
| file_reads | `oltp_read_only` | 211.30ms | 161.39ms | 0.764× | 1.23% | PASS |
| file_writes | `oltp_bulk_insert` | 192.10ms | 274.18ms | 1.427× | 1.01% | PASS |
| file_writes | `oltp_insert` | 22.04ms | 34.75ms | 1.577× | 1.73% | PASS |
| file_writes | `oltp_update_index` | 78.27ms | 133.96ms | 1.712× | 1.49% | PASS |
| file_writes | `oltp_update_non_index` | 58.76ms | 85.88ms | 1.462× | 1.84% | PASS |
| file_writes | `oltp_delete_insert` | 69.52ms | 106.48ms | 1.532× | 1.51% | PASS |
| file_writes | `oltp_write_only` | 45.11ms | 65.41ms | 1.450× | 2.16% | PASS |
| file_writes | `types_delete_insert` | 41.47ms | 53.65ms | 1.294× | 1.57% | PASS |
| file_writes | `oltp_read_write` | 99.11ms | 132.81ms | 1.340× | 1.75% | PASS |
| ac_reads | `oltp_point_select` | 48.04ms | 53.22ms | 1.108× | 1.36% | PASS |
| ac_reads | `oltp_range_select` | 13.91ms | 14.92ms | 1.073× | 1.88% | PASS |
| ac_reads | `oltp_sum_range` | 13.04ms | 14.98ms | 1.149× | 1.77% | PASS |
| ac_reads | `oltp_order_range` | 3.23ms | 3.43ms | 1.060× | 1.81% | PASS |
| ac_reads | `oltp_distinct_range` | 4.17ms | 4.55ms | 1.091× | 1.70% | PASS |
| ac_reads | `oltp_index_scan` | 6.79ms | 8.17ms | 1.203× | 1.98% | PASS |
| ac_reads | `select_random_points` | 14.83ms | 14.13ms | 0.953× | 2.72% | PASS |
| ac_reads | `select_random_ranges` | 5.71ms | 6.45ms | 1.131× | 1.60% | PASS |
| ac_reads | `covering_index_scan` | 6.98ms | 7.01ms | 1.004× | 1.29% | PASS |
| ac_reads | `groupby_scan` | 32.48ms | 35.94ms | 1.106× | 1.98% | PASS |
| ac_reads | `index_join` | 7.97ms | 10.55ms | 1.325× | 1.71% | PASS |
| ac_reads | `index_join_scan` | 3.99ms | 5.04ms | 1.265× | 2.62% | PASS |
| ac_reads | `types_table_scan` | 1.10s | 1.36s | 1.239× | 1.28% | PASS |
| ac_reads | `table_scan` | 1.25s | 1.46s | 1.169× | 1.32% | PASS |
| ac_reads | `oltp_read_only` | 148.50ms | 164.47ms | 1.108× | 1.23% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.96ms | 64.98ms | 3.101× | 4.35% | PASS |
| ac_writes | `oltp_insert_ac` | 24.09ms | 81.98ms | 3.404× | 5.70% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.61ms | 98.90ms | 3.716× | 6.27% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.90ms | 74.83ms | 3.268× | 5.93% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.36ms | 87.15ms | 3.577× | 5.60% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.53ms | 85.76ms | 3.644× | 6.78% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.77ms | 74.28ms | 3.576× | 8.55% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.48ms | 92.04ms | 3.122× | 5.82% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 28.84ms | 37.53ms | 1.301× | 1.08% | PASS |
| mem_reads | `oltp_range_select` | 14.83ms | 14.75ms | 0.995× | 3.53% | PASS |
| mem_reads | `oltp_sum_range` | 11.29ms | 14.04ms | 1.243× | 2.73% | PASS |
| mem_reads | `oltp_order_range` | 2.84ms | 3.18ms | 1.119× | 0.82% | PASS |
| mem_reads | `oltp_distinct_range` | 3.89ms | 4.31ms | 1.108× | 1.70% | PASS |
| mem_reads | `oltp_index_scan` | 4.51ms | 6.19ms | 1.371× | 2.96% | PASS |
| mem_reads | `select_random_points` | 17.58ms | 20.68ms | 1.176× | 1.87% | PASS |
| mem_reads | `select_random_ranges` | 3.86ms | 5.19ms | 1.344× | 2.64% | PASS |
| mem_reads | `covering_index_scan` | 4.52ms | 4.46ms | 0.988× | 1.70% | PASS |
| mem_reads | `groupby_scan` | 33.17ms | 37.06ms | 1.117× | 2.34% | PASS |
| mem_reads | `index_join` | 6.93ms | 9.02ms | 1.302× | 1.92% | PASS |
| mem_reads | `index_join_scan` | 4.60ms | 5.44ms | 1.183× | 2.23% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.24s | 1.164× | 0.83% | PASS |
| mem_reads | `table_scan` | 1.20s | 1.34s | 1.113× | 0.72% | PASS |
| mem_reads | `oltp_read_only` | 119.88ms | 138.46ms | 1.155× | 0.83% | PASS |
| mem_writes | `oltp_bulk_insert` | 233.59ms | 360.33ms | 1.543× | 0.88% | PASS |
| mem_writes | `oltp_insert` | 21.48ms | 40.31ms | 1.877× | 0.82% | PASS |
| mem_writes | `oltp_update_index` | 70.45ms | 137.72ms | 1.955× | 1.12% | PASS |
| mem_writes | `oltp_update_non_index` | 48.41ms | 90.29ms | 1.865× | 1.22% | PASS |
| mem_writes | `oltp_delete_insert` | 50.85ms | 107.38ms | 2.112× | 0.97% | PASS |
| mem_writes | `oltp_write_only` | 28.78ms | 64.53ms | 2.242× | 1.10% | PASS |
| mem_writes | `types_delete_insert` | 32.67ms | 58.16ms | 1.781× | 1.03% | PASS |
| mem_writes | `oltp_read_write` | 85.41ms | 143.34ms | 1.678× | 0.76% | PASS |
| file_reads | `oltp_point_select` | 99.19ms | 61.41ms | 0.619× | 1.14% | PASS |
| file_reads | `oltp_range_select` | 21.53ms | 17.26ms | 0.802× | 3.73% | PASS |
| file_reads | `oltp_sum_range` | 19.54ms | 16.88ms | 0.864× | 2.12% | PASS |
| file_reads | `oltp_order_range` | 4.02ms | 3.56ms | 0.886× | 2.55% | PASS |
| file_reads | `oltp_distinct_range` | 5.08ms | 4.67ms | 0.920× | 0.99% | PASS |
| file_reads | `oltp_index_scan` | 11.99ms | 8.86ms | 0.739× | 2.59% | PASS |
| file_reads | `select_random_points` | 26.24ms | 24.14ms | 0.920× | 2.99% | PASS |
| file_reads | `select_random_ranges` | 11.13ms | 7.74ms | 0.695× | 1.83% | PASS |
| file_reads | `covering_index_scan` | 12.60ms | 7.18ms | 0.570× | 2.29% | PASS |
| file_reads | `groupby_scan` | 34.49ms | 37.46ms | 1.086× | 1.38% | PASS |
| file_reads | `index_join` | 11.63ms | 11.38ms | 0.979× | 3.61% | PASS |
| file_reads | `index_join_scan` | 5.59ms | 6.04ms | 1.081× | 2.26% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.23s | 1.164× | 1.05% | PASS |
| file_reads | `table_scan` | 1.21s | 1.34s | 1.106× | 0.85% | PASS |
| file_reads | `oltp_read_only` | 224.54ms | 174.61ms | 0.778× | 0.64% | PASS |
| file_writes | `oltp_bulk_insert` | 250.46ms | 390.50ms | 1.559× | 0.76% | PASS |
| file_writes | `oltp_insert` | 48.16ms | 53.91ms | 1.120× | 21.68% | PASS |
| file_writes | `oltp_update_index` | 112.07ms | 174.96ms | 1.561× | 1.63% | PASS |
| file_writes | `oltp_update_non_index` | 101.30ms | 117.09ms | 1.156× | 10.53% | PASS |
| file_writes | `oltp_delete_insert` | 89.62ms | 137.49ms | 1.534× | 1.14% | PASS |
| file_writes | `oltp_write_only` | 82.24ms | 88.64ms | 1.078× | 11.30% | PASS |
| file_writes | `types_delete_insert` | 55.25ms | 75.89ms | 1.374× | 2.12% | PASS |
| file_writes | `oltp_read_write` | 145.52ms | 167.31ms | 1.150× | 3.91% | PASS |
| ac_reads | `oltp_point_select` | 51.84ms | 61.62ms | 1.189× | 1.26% | PASS |
| ac_reads | `oltp_range_select` | 16.34ms | 17.17ms | 1.051× | 1.81% | PASS |
| ac_reads | `oltp_sum_range` | 14.36ms | 16.96ms | 1.181× | 1.67% | PASS |
| ac_reads | `oltp_order_range` | 3.38ms | 3.55ms | 1.049× | 1.32% | PASS |
| ac_reads | `oltp_distinct_range` | 4.41ms | 4.67ms | 1.058× | 1.93% | PASS |
| ac_reads | `oltp_index_scan` | 6.91ms | 8.90ms | 1.288× | 1.84% | PASS |
| ac_reads | `select_random_points` | 20.38ms | 24.48ms | 1.201× | 2.01% | PASS |
| ac_reads | `select_random_ranges` | 6.25ms | 7.71ms | 1.234× | 1.08% | PASS |
| ac_reads | `covering_index_scan` | 7.25ms | 7.25ms | 1.000× | 3.59% | PASS |
| ac_reads | `groupby_scan` | 33.59ms | 37.52ms | 1.117× | 2.49% | PASS |
| ac_reads | `index_join` | 8.60ms | 11.17ms | 1.299× | 3.40% | PASS |
| ac_reads | `index_join_scan` | 5.27ms | 6.12ms | 1.160× | 3.59% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.23s | 1.166× | 0.54% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.33s | 1.127× | 0.44% | PASS |
| ac_reads | `oltp_read_only` | 151.87ms | 173.81ms | 1.145× | 0.97% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.83ms | 64.40ms | 2.950× | 4.19% | PASS |
| ac_writes | `oltp_insert_ac` | 24.32ms | 79.65ms | 3.275× | 4.12% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.11ms | 97.33ms | 3.590× | 4.26% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.50ms | 76.66ms | 3.565× | 4.55% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.60ms | 87.11ms | 3.691× | 3.04% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.17ms | 87.03ms | 3.601× | 4.69% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.66ms | 75.74ms | 3.665× | 6.61% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.50ms | 94.63ms | 3.208× | 3.22% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.08ms | 22.46ms | 1.118× | 2.80% | PASS |
| mem_reads | `oltp_range_select` | 9.46ms | 9.07ms | 0.959× | 2.56% | PASS |
| mem_reads | `oltp_sum_range` | 9.21ms | 8.77ms | 0.952× | 2.56% | PASS |
| mem_reads | `oltp_order_range` | 2.08ms | 2.05ms | 0.986× | 1.68% | PASS |
| mem_reads | `oltp_distinct_range` | 2.63ms | 2.52ms | 0.959× | 1.03% | PASS |
| mem_reads | `oltp_index_scan` | 3.00ms | 4.13ms | 1.376× | 3.02% | PASS |
| mem_reads | `select_random_points` | 13.47ms | 14.35ms | 1.065× | 2.42% | PASS |
| mem_reads | `select_random_ranges` | 3.09ms | 3.59ms | 1.162× | 2.20% | PASS |
| mem_reads | `covering_index_scan` | 2.61ms | 3.28ms | 1.256× | 5.82% | PASS |
| mem_reads | `groupby_scan` | 21.95ms | 21.34ms | 0.972× | 1.73% | PASS |
| mem_reads | `index_join` | 4.78ms | 7.77ms | 1.625× | 5.27% | PASS |
| mem_reads | `index_join_scan` | 3.76ms | 5.14ms | 1.369× | 3.68% | PASS |
| mem_reads | `types_table_scan` | 859.77ms | 827.91ms | 0.963× | 2.85% | PASS |
| mem_reads | `table_scan` | 963.15ms | 911.57ms | 0.946× | 4.21% | PASS |
| mem_reads | `oltp_read_only` | 86.21ms | 83.77ms | 0.972× | 2.52% | PASS |
| mem_writes | `oltp_bulk_insert` | 141.51ms | 211.80ms | 1.497× | 1.78% | PASS |
| mem_writes | `oltp_insert` | 11.87ms | 26.80ms | 2.259× | 1.67% | PASS |
| mem_writes | `oltp_update_index` | 43.82ms | 93.39ms | 2.131× | 2.31% | PASS |
| mem_writes | `oltp_update_non_index` | 34.08ms | 62.61ms | 1.837× | 2.79% | PASS |
| mem_writes | `oltp_delete_insert` | 33.58ms | 76.45ms | 2.276× | 2.92% | PASS |
| mem_writes | `oltp_write_only` | 19.62ms | 47.87ms | 2.440× | 3.68% | PASS |
| mem_writes | `types_delete_insert` | 21.50ms | 39.79ms | 1.850× | 1.96% | PASS |
| mem_writes | `oltp_read_write` | 53.14ms | 88.61ms | 1.667× | 4.28% | PASS |
| file_reads | `oltp_point_select` | 73.51ms | 40.61ms | 0.552× | 2.21% | PASS |
| file_reads | `oltp_range_select` | 15.03ms | 10.68ms | 0.711× | 2.93% | PASS |
| file_reads | `oltp_sum_range` | 15.24ms | 10.73ms | 0.705× | 3.27% | PASS |
| file_reads | `oltp_order_range` | 2.72ms | 2.27ms | 0.835× | 1.87% | PASS |
| file_reads | `oltp_distinct_range` | 3.31ms | 2.76ms | 0.833× | 1.82% | PASS |
| file_reads | `oltp_index_scan` | 8.99ms | 6.23ms | 0.693× | 2.08% | PASS |
| file_reads | `select_random_points` | 19.74ms | 15.86ms | 0.804× | 4.07% | PASS |
| file_reads | `select_random_ranges` | 8.73ms | 5.52ms | 0.632× | 1.70% | PASS |
| file_reads | `covering_index_scan` | 9.46ms | 5.23ms | 0.552× | 1.55% | PASS |
| file_reads | `groupby_scan` | 22.90ms | 21.68ms | 0.947× | 1.70% | PASS |
| file_reads | `index_join` | 8.27ms | 7.84ms | 0.949× | 3.22% | PASS |
| file_reads | `index_join_scan` | 3.97ms | 4.53ms | 1.142× | 3.18% | PASS |
| file_reads | `types_table_scan` | 737.44ms | 775.34ms | 1.051× | 2.48% | PASS |
| file_reads | `table_scan` | 1.05s | 924.73ms | 0.884× | 3.58% | PASS |
| file_reads | `oltp_read_only` | 155.15ms | 103.48ms | 0.667× | 3.12% | PASS |
| file_writes | `oltp_bulk_insert` | 190.54ms | 280.03ms | 1.470× | 3.15% | PASS |
| file_writes | `oltp_insert` | 34.41ms | 53.57ms | 1.557× | 7.31% | PASS |
| file_writes | `oltp_update_index` | 178.99ms | 198.69ms | 1.110× | 4.11% | PASS |
| file_writes | `oltp_update_non_index` | 131.00ms | 128.74ms | 0.983× | 1.51% | PASS |
| file_writes | `oltp_delete_insert` | 136.15ms | 145.25ms | 1.067× | 3.59% | PASS |
| file_writes | `oltp_write_only` | 101.03ms | 108.53ms | 1.074× | 4.32% | PASS |
| file_writes | `types_delete_insert` | 90.24ms | 83.28ms | 0.923× | 5.56% | PASS |
| file_writes | `oltp_read_write` | 141.77ms | 150.12ms | 1.059× | 7.07% | PASS |
| ac_reads | `oltp_point_select` | 39.89ms | 42.91ms | 1.076× | 1.93% | PASS |
| ac_reads | `oltp_range_select` | 12.10ms | 11.02ms | 0.911× | 3.97% | PASS |
| ac_reads | `oltp_sum_range` | 11.82ms | 10.88ms | 0.921× | 4.28% | PASS |
| ac_reads | `oltp_order_range` | 2.49ms | 2.33ms | 0.937× | 0.87% | PASS |
| ac_reads | `oltp_distinct_range` | 3.02ms | 2.81ms | 0.931× | 0.98% | PASS |
| ac_reads | `oltp_index_scan` | 5.68ms | 6.32ms | 1.113× | 1.75% | PASS |
| ac_reads | `select_random_points` | 16.15ms | 16.01ms | 0.991× | 6.53% | PASS |
| ac_reads | `select_random_ranges` | 5.16ms | 5.54ms | 1.074× | 1.68% | PASS |
| ac_reads | `covering_index_scan` | 5.86ms | 5.29ms | 0.903× | 2.82% | PASS |
| ac_reads | `groupby_scan` | 22.63ms | 21.86ms | 0.966× | 1.33% | PASS |
| ac_reads | `index_join` | 6.73ms | 8.01ms | 1.190× | 4.83% | PASS |
| ac_reads | `index_join_scan` | 3.92ms | 4.81ms | 1.226× | 7.59% | PASS |
| ac_reads | `types_table_scan` | 724.81ms | 774.37ms | 1.068× | 1.36% | PASS |
| ac_reads | `table_scan` | 1.03s | 895.25ms | 0.872× | 3.53% | PASS |
| ac_reads | `oltp_read_only` | 101.28ms | 104.95ms | 1.036× | 5.60% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.69ms | 57.52ms | 2.535× | 4.96% | PASS |
| ac_writes | `oltp_insert_ac` | 26.94ms | 75.60ms | 2.806× | 8.97% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.41ms | 79.91ms | 2.813× | 4.64% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.95ms | 63.92ms | 2.912× | 7.08% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.38ms | 73.37ms | 2.891× | 12.72% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.96ms | 73.55ms | 2.833× | 5.93% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.08ms | 65.74ms | 3.119× | 8.68% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.94ms | 79.49ms | 2.746× | 5.76% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 28.82ms | 33.47ms | 1.161× | 0.84% | PASS |
| mem_reads | `oltp_range_select` | 16.65ms | 19.30ms | 1.159× | 1.88% | PASS |
| mem_reads | `oltp_sum_range` | 16.42ms | 18.34ms | 1.117× | 1.14% | PASS |
| mem_reads | `oltp_order_range` | 3.33ms | 3.62ms | 1.087× | 0.85% | PASS |
| mem_reads | `oltp_distinct_range` | 4.25ms | 4.61ms | 1.085× | 0.78% | PASS |
| mem_reads | `oltp_index_scan` | 4.16ms | 5.53ms | 1.332× | 1.25% | PASS |
| mem_reads | `select_random_points` | 25.04ms | 29.18ms | 1.165× | 0.65% | PASS |
| mem_reads | `select_random_ranges` | 6.53ms | 7.62ms | 1.167× | 0.56% | PASS |
| mem_reads | `covering_index_scan` | 3.43ms | 3.89ms | 1.134× | 0.90% | PASS |
| mem_reads | `groupby_scan` | 36.16ms | 38.24ms | 1.058× | 0.37% | PASS |
| mem_reads | `index_join` | 7.30ms | 10.04ms | 1.375× | 0.72% | PASS |
| mem_reads | `index_join_scan` | 3.59ms | 5.15ms | 1.436× | 0.74% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.20s | 1.119× | 0.65% | PASS |
| mem_reads | `table_scan` | 1.32s | 1.31s | 0.991× | 1.93% | PASS |
| mem_reads | `oltp_read_only` | 133.53ms | 149.37ms | 1.119× | 0.81% | PASS |
| mem_writes | `oltp_bulk_insert` | 197.01ms | 285.09ms | 1.447× | 0.59% | PASS |
| mem_writes | `oltp_insert` | 16.29ms | 30.99ms | 1.902× | 0.77% | PASS |
| mem_writes | `oltp_update_index` | 59.55ms | 110.36ms | 1.853× | 0.86% | PASS |
| mem_writes | `oltp_update_non_index` | 42.69ms | 75.09ms | 1.759× | 0.66% | PASS |
| mem_writes | `oltp_delete_insert` | 42.92ms | 87.35ms | 2.035× | 0.70% | PASS |
| mem_writes | `oltp_write_only` | 23.05ms | 52.50ms | 2.278× | 0.54% | PASS |
| mem_writes | `types_delete_insert` | 27.08ms | 47.75ms | 1.764× | 0.61% | PASS |
| mem_writes | `oltp_read_write` | 86.08ms | 133.68ms | 1.553× | 1.15% | PASS |
| file_reads | `oltp_point_select` | 58.97ms | 42.89ms | 0.727× | 0.86% | PASS |
| file_reads | `oltp_range_select` | 20.62ms | 20.24ms | 0.981× | 0.70% | PASS |
| file_reads | `oltp_sum_range` | 19.86ms | 19.46ms | 0.980× | 0.61% | PASS |
| file_reads | `oltp_order_range` | 3.78ms | 3.78ms | 1.001× | 0.82% | PASS |
| file_reads | `oltp_distinct_range` | 4.69ms | 4.77ms | 1.018× | 0.83% | PASS |
| file_reads | `oltp_index_scan` | 7.56ms | 6.63ms | 0.878× | 0.66% | PASS |
| file_reads | `select_random_points` | 28.91ms | 30.52ms | 1.056× | 0.67% | PASS |
| file_reads | `select_random_ranges` | 9.93ms | 8.68ms | 0.874× | 0.78% | PASS |
| file_reads | `covering_index_scan` | 6.75ms | 4.97ms | 0.736× | 0.82% | PASS |
| file_reads | `groupby_scan` | 36.63ms | 38.27ms | 1.045× | 0.39% | PASS |
| file_reads | `index_join` | 9.38ms | 10.92ms | 1.163× | 0.73% | PASS |
| file_reads | `index_join_scan` | 4.07ms | 5.45ms | 1.338× | 1.21% | PASS |
| file_reads | `types_table_scan` | 1.10s | 1.21s | 1.103× | 1.56% | PASS |
| file_reads | `table_scan` | 1.39s | 1.33s | 0.958× | 1.20% | PASS |
| file_reads | `oltp_read_only` | 178.80ms | 163.15ms | 0.913× | 0.73% | PASS |
| file_writes | `oltp_bulk_insert` | 256.13ms | 354.08ms | 1.382× | 1.08% | PASS |
| file_writes | `oltp_insert` | 32.74ms | 55.41ms | 1.692× | 1.34% | PASS |
| file_writes | `oltp_update_index` | 176.99ms | 190.59ms | 1.077× | 1.05% | PASS |
| file_writes | `oltp_update_non_index` | 146.53ms | 137.87ms | 0.941× | 0.96% | PASS |
| file_writes | `oltp_delete_insert` | 145.57ms | 157.03ms | 1.079× | 1.05% | PASS |
| file_writes | `oltp_write_only` | 104.75ms | 111.74ms | 1.067× | 1.13% | PASS |
| file_writes | `types_delete_insert` | 95.36ms | 89.79ms | 0.942× | 5.02% | PASS |
| file_writes | `oltp_read_write` | 169.34ms | 192.89ms | 1.139× | 1.21% | PASS |
| ac_reads | `oltp_point_select` | 38.16ms | 42.99ms | 1.127× | 1.03% | PASS |
| ac_reads | `oltp_range_select` | 18.15ms | 20.30ms | 1.118× | 1.00% | PASS |
| ac_reads | `oltp_sum_range` | 17.85ms | 19.48ms | 1.091× | 0.99% | PASS |
| ac_reads | `oltp_order_range` | 3.54ms | 3.79ms | 1.070× | 0.78% | PASS |
| ac_reads | `oltp_distinct_range` | 4.47ms | 4.76ms | 1.065× | 0.86% | PASS |
| ac_reads | `oltp_index_scan` | 5.58ms | 6.69ms | 1.200× | 0.91% | PASS |
| ac_reads | `select_random_points` | 26.82ms | 30.69ms | 1.144× | 0.69% | PASS |
| ac_reads | `select_random_ranges` | 7.81ms | 8.70ms | 1.114× | 0.73% | PASS |
| ac_reads | `covering_index_scan` | 4.67ms | 4.98ms | 1.067× | 1.03% | PASS |
| ac_reads | `groupby_scan` | 36.47ms | 38.30ms | 1.050× | 0.32% | PASS |
| ac_reads | `index_join` | 8.39ms | 11.01ms | 1.312× | 0.84% | PASS |
| ac_reads | `index_join_scan` | 3.91ms | 5.42ms | 1.386× | 1.17% | PASS |
| ac_reads | `types_table_scan` | 1.09s | 1.21s | 1.108× | 1.06% | PASS |
| ac_reads | `table_scan` | 1.33s | 1.31s | 0.991× | 1.60% | PASS |
| ac_reads | `oltp_read_only` | 149.30ms | 164.39ms | 1.101× | 0.59% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 28.10ms | 75.46ms | 2.686× | 3.66% | PASS |
| ac_writes | `oltp_insert_ac` | 30.61ms | 92.80ms | 3.031× | 3.61% | PASS |
| ac_writes | `oltp_update_index_ac` | 31.53ms | 102.17ms | 3.240× | 2.50% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 28.45ms | 83.94ms | 2.950× | 2.52% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 30.94ms | 95.60ms | 3.089× | 3.00% | PASS |
| ac_writes | `oltp_write_only_ac` | 30.17ms | 93.49ms | 3.098× | 3.07% | PASS |
| ac_writes | `types_delete_insert_ac` | 28.20ms | 85.73ms | 3.041× | 5.98% | PASS |
| ac_writes | `oltp_read_write_ac` | 35.24ms | 103.15ms | 2.927× | 2.86% | PASS |

</details>

## Version-control latency

Wall time: 1m 45s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 61.40ms | 200.00ms | 30.7% | 0.93% | PASS |
| `status_dirty_many_tables` | 63.77ms | 200.00ms | 31.9% | 1.05% | PASS |
| `diff_regular_working_one_table` | 57.03ms | 150.00ms | 38.0% | 1.02% | PASS |
| `diff_regular_working_many_tables` | 68.27ms | 200.00ms | 34.1% | 0.81% | PASS |
| `diff_stat_working_many_tables` | 68.06ms | 200.00ms | 34.0% | 0.85% | PASS |
| `diff_schema_working_many_tables` | 68.47ms | 200.00ms | 34.2% | 0.70% | PASS |
| `branch_list_many_branches` | 17.60ms | 100.00ms | 17.6% | 1.32% | PASS |
| `branch_create_delete` | 19.65ms | 100.00ms | 19.6% | 1.34% | PASS |
| `checkout_branch_clean` | 44.29ms | 200.00ms | 22.1% | 1.24% | PASS |
| `merge_data_no_conflicts` | 22.77ms | 150.00ms | 15.2% | 1.70% | PASS |
| `merge_schema_no_conflicts` | 17.79ms | 100.00ms | 17.8% | 1.47% | PASS |
| `merge_data_conflicts` | 74.18ms | 250.00ms | 29.7% | 0.64% | PASS |
| `merge_data_conflicts_with_resolve` | 74.05ms | 250.00ms | 29.6% | 0.48% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
