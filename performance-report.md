# DoltLite Performance Report

> Nightly result: **FAIL**
>
> Generated: 2026-09-02 15:19 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260831.293.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/33637943688)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 39s | 9.90s | 11.28s | 1.139× | 1.57% | **PASS** |
| textpk | 69 | 55 | 1h 18m 12s | 8.63s | 10.13s | 1.174× | 1.32% | **PASS** |
| blobpk | 69 | 55 | 1h 9m 34s | 6.83s | 8.14s | 1.191× | 2.62% | **PASS** |
| compositepk | 69 | 55 | 1h 28m 2s | 10.82s | 12.04s | 1.113× | 1.49% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.15ms | 28.62ms | 1.094× | 1.91% | PASS |
| mem_reads | `oltp_range_select` | 11.58ms | 12.53ms | 1.081× | 2.41% | PASS |
| mem_reads | `oltp_sum_range` | 10.44ms | 11.80ms | 1.131× | 1.96% | PASS |
| mem_reads | `oltp_order_range` | 2.73ms | 2.93ms | 1.073× | 0.98% | PASS |
| mem_reads | `oltp_distinct_range` | 3.79ms | 3.93ms | 1.036× | 1.03% | PASS |
| mem_reads | `oltp_index_scan` | 4.16ms | 5.34ms | 1.283× | 2.04% | PASS |
| mem_reads | `select_random_points` | 11.59ms | 11.91ms | 1.028× | 2.55% | PASS |
| mem_reads | `select_random_ranges` | 3.23ms | 4.11ms | 1.271× | 1.93% | PASS |
| mem_reads | `covering_index_scan` | 4.46ms | 4.55ms | 1.020× | 2.11% | PASS |
| mem_reads | `groupby_scan` | 32.28ms | 34.67ms | 1.074× | 0.77% | PASS |
| mem_reads | `index_join` | 6.02ms | 8.51ms | 1.413× | 2.18% | PASS |
| mem_reads | `index_join_scan` | 3.59ms | 4.88ms | 1.359× | 2.16% | PASS |
| mem_reads | `types_table_scan` | 1.17s | 1.29s | 1.096× | 1.08% | PASS |
| mem_reads | `table_scan` | 1.39s | 1.41s | 1.013× | 0.91% | PASS |
| mem_reads | `oltp_read_only` | 109.72ms | 118.48ms | 1.080× | 1.46% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.72ms | 243.48ms | 1.355× | 1.05% | PASS |
| mem_writes | `oltp_insert` | 16.20ms | 29.07ms | 1.794× | 1.17% | PASS |
| mem_writes | `oltp_update_index` | 55.39ms | 114.50ms | 2.067× | 2.19% | PASS |
| mem_writes | `oltp_update_non_index` | 37.78ms | 62.08ms | 1.643× | 1.76% | PASS |
| mem_writes | `oltp_delete_insert` | 47.52ms | 81.44ms | 1.714× | 1.77% | PASS |
| mem_writes | `oltp_write_only` | 23.39ms | 47.70ms | 2.039× | 2.37% | PASS |
| mem_writes | `types_delete_insert` | 25.97ms | 41.27ms | 1.589× | 1.76% | PASS |
| mem_writes | `oltp_read_write` | 68.56ms | 107.49ms | 1.568× | 1.71% | PASS |
| file_reads | `oltp_point_select` | 121.06ms | 59.85ms | 0.494× | 0.88% | PASS |
| file_reads | `oltp_range_select` | 21.02ms | 15.42ms | 0.734× | 1.77% | PASS |
| file_reads | `oltp_sum_range` | 19.76ms | 14.94ms | 0.756× | 1.61% | PASS |
| file_reads | `oltp_order_range` | 3.69ms | 3.26ms | 0.884× | 1.27% | PASS |
| file_reads | `oltp_distinct_range` | 4.81ms | 4.27ms | 0.887× | 1.27% | PASS |
| file_reads | `oltp_index_scan` | 13.91ms | 8.65ms | 0.622× | 1.34% | PASS |
| file_reads | `select_random_points` | 21.53ms | 15.19ms | 0.705× | 2.06% | PASS |
| file_reads | `select_random_ranges` | 12.80ms | 7.25ms | 0.566× | 0.62% | PASS |
| file_reads | `covering_index_scan` | 14.46ms | 7.73ms | 0.535× | 1.05% | PASS |
| file_reads | `groupby_scan` | 33.31ms | 35.08ms | 1.053× | 0.89% | PASS |
| file_reads | `index_join` | 11.33ms | 10.27ms | 0.907× | 1.59% | PASS |
| file_reads | `index_join_scan` | 4.61ms | 5.12ms | 1.110× | 1.66% | PASS |
| file_reads | `types_table_scan` | 1.14s | 1.28s | 1.119× | 1.57% | PASS |
| file_reads | `table_scan` | 1.37s | 1.41s | 1.030× | 1.39% | PASS |
| file_reads | `oltp_read_only` | 248.73ms | 165.86ms | 0.667× | 0.66% | PASS |
| file_writes | `oltp_bulk_insert` | 194.94ms | 264.44ms | 1.357× | 1.21% | PASS |
| file_writes | `oltp_insert` | 22.78ms | 36.73ms | 1.612× | 1.49% | PASS |
| file_writes | `oltp_update_index` | 82.30ms | 133.72ms | 1.625× | 1.61% | PASS |
| file_writes | `oltp_update_non_index` | 62.44ms | 84.31ms | 1.350× | 1.25% | PASS |
| file_writes | `oltp_delete_insert` | 70.97ms | 102.31ms | 1.442× | 2.15% | PASS |
| file_writes | `oltp_write_only` | 46.88ms | 68.23ms | 1.456× | 1.67% | PASS |
| file_writes | `types_delete_insert` | 42.97ms | 55.54ms | 1.292× | 1.11% | PASS |
| file_writes | `oltp_read_write` | 97.57ms | 132.15ms | 1.354× | 1.44% | PASS |
| ac_reads | `oltp_point_select` | 57.52ms | 60.40ms | 1.050× | 1.20% | PASS |
| ac_reads | `oltp_range_select` | 14.78ms | 15.47ms | 1.046× | 1.84% | PASS |
| ac_reads | `oltp_sum_range` | 13.57ms | 15.03ms | 1.107× | 1.53% | PASS |
| ac_reads | `oltp_order_range` | 3.17ms | 3.27ms | 1.030× | 0.81% | PASS |
| ac_reads | `oltp_distinct_range` | 4.19ms | 4.27ms | 1.018× | 0.98% | PASS |
| ac_reads | `oltp_index_scan` | 7.62ms | 8.64ms | 1.134× | 1.01% | PASS |
| ac_reads | `select_random_points` | 15.31ms | 15.25ms | 0.996× | 2.39% | PASS |
| ac_reads | `select_random_ranges` | 6.47ms | 7.24ms | 1.118× | 0.91% | PASS |
| ac_reads | `covering_index_scan` | 8.02ms | 7.73ms | 0.964× | 1.34% | PASS |
| ac_reads | `groupby_scan` | 32.51ms | 34.97ms | 1.076× | 0.57% | PASS |
| ac_reads | `index_join` | 8.10ms | 10.23ms | 1.264× | 1.62% | PASS |
| ac_reads | `index_join_scan` | 4.08ms | 5.12ms | 1.255× | 1.53% | PASS |
| ac_reads | `types_table_scan` | 1.14s | 1.28s | 1.122× | 1.26% | PASS |
| ac_reads | `table_scan` | 1.38s | 1.41s | 1.023× | 0.99% | PASS |
| ac_reads | `oltp_read_only` | 156.02ms | 164.83ms | 1.056× | 0.91% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.72ms | 62.85ms | 3.997× | 3.90% | PASS |
| ac_writes | `oltp_insert_ac` | 18.40ms | 80.17ms | 4.357× | 4.46% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.87ms | 95.98ms | 4.830× | 3.70% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 15.77ms | 73.38ms | 4.653× | 4.18% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.59ms | 85.73ms | 4.873× | 5.85% | PASS |
| ac_writes | `oltp_write_only_ac` | 17.98ms | 84.23ms | 4.686× | 5.45% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.30ms | 72.95ms | 4.768× | 4.75% | PASS |
| ac_writes | `oltp_read_write_ac` | 23.27ms | 91.68ms | 3.940× | 4.19% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.59ms | 31.37ms | 1.226× | 0.96% | PASS |
| mem_reads | `oltp_range_select` | 11.29ms | 12.10ms | 1.072× | 0.98% | PASS |
| mem_reads | `oltp_sum_range` | 10.81ms | 12.08ms | 1.118× | 0.91% | PASS |
| mem_reads | `oltp_order_range` | 2.64ms | 2.83ms | 1.072× | 1.06% | PASS |
| mem_reads | `oltp_distinct_range` | 3.38ms | 3.64ms | 1.078× | 0.56% | PASS |
| mem_reads | `oltp_index_scan` | 3.90ms | 5.29ms | 1.357× | 1.37% | PASS |
| mem_reads | `select_random_points` | 15.75ms | 19.84ms | 1.260× | 1.30% | PASS |
| mem_reads | `select_random_ranges` | 3.38ms | 4.51ms | 1.336× | 0.60% | PASS |
| mem_reads | `covering_index_scan` | 4.00ms | 3.88ms | 0.971× | 2.46% | PASS |
| mem_reads | `groupby_scan` | 29.23ms | 30.35ms | 1.038× | 0.48% | PASS |
| mem_reads | `index_join` | 6.10ms | 8.26ms | 1.354× | 1.56% | PASS |
| mem_reads | `index_join_scan` | 3.81ms | 4.96ms | 1.301× | 0.74% | PASS |
| mem_reads | `types_table_scan` | 969.01ms | 1.10s | 1.132× | 0.77% | PASS |
| mem_reads | `table_scan` | 1.15s | 1.19s | 1.038× | 1.80% | PASS |
| mem_reads | `oltp_read_only` | 103.56ms | 118.27ms | 1.142× | 1.11% | PASS |
| mem_writes | `oltp_bulk_insert` | 199.97ms | 293.86ms | 1.470× | 1.01% | PASS |
| mem_writes | `oltp_insert` | 18.81ms | 33.19ms | 1.765× | 0.84% | PASS |
| mem_writes | `oltp_update_index` | 61.81ms | 115.11ms | 1.862× | 1.09% | PASS |
| mem_writes | `oltp_update_non_index` | 41.42ms | 70.84ms | 1.710× | 0.76% | PASS |
| mem_writes | `oltp_delete_insert` | 42.89ms | 86.68ms | 2.021× | 0.81% | PASS |
| mem_writes | `oltp_write_only` | 23.85ms | 50.84ms | 2.132× | 0.86% | PASS |
| mem_writes | `types_delete_insert` | 27.30ms | 45.93ms | 1.682× | 0.83% | PASS |
| mem_writes | `oltp_read_write` | 72.09ms | 118.30ms | 1.641× | 1.06% | PASS |
| file_reads | `oltp_point_select` | 58.09ms | 43.63ms | 0.751× | 0.91% | PASS |
| file_reads | `oltp_range_select` | 14.75ms | 13.67ms | 0.927× | 1.57% | PASS |
| file_reads | `oltp_sum_range` | 14.23ms | 13.69ms | 0.962× | 1.44% | PASS |
| file_reads | `oltp_order_range` | 3.01ms | 3.04ms | 1.011× | 1.47% | PASS |
| file_reads | `oltp_distinct_range` | 3.78ms | 3.86ms | 1.021× | 1.25% | PASS |
| file_reads | `oltp_index_scan` | 7.30ms | 6.90ms | 0.945× | 1.23% | PASS |
| file_reads | `select_random_points` | 19.35ms | 21.54ms | 1.113× | 1.26% | PASS |
| file_reads | `select_random_ranges` | 6.62ms | 5.82ms | 0.878× | 1.61% | PASS |
| file_reads | `covering_index_scan` | 7.38ms | 5.35ms | 0.725× | 1.69% | PASS |
| file_reads | `groupby_scan` | 29.55ms | 30.60ms | 1.036× | 0.75% | PASS |
| file_reads | `index_join` | 8.10ms | 9.53ms | 1.176× | 0.92% | PASS |
| file_reads | `index_join_scan` | 4.34ms | 5.26ms | 1.212× | 1.15% | PASS |
| file_reads | `types_table_scan` | 980.66ms | 1.11s | 1.127× | 1.56% | PASS |
| file_reads | `table_scan` | 1.20s | 1.21s | 1.008× | 2.44% | PASS |
| file_reads | `oltp_read_only` | 152.87ms | 138.40ms | 0.905× | 0.93% | PASS |
| file_writes | `oltp_bulk_insert` | 216.97ms | 317.64ms | 1.464× | 1.16% | PASS |
| file_writes | `oltp_insert` | 52.30ms | 42.54ms | 0.813× | 14.80% | PASS |
| file_writes | `oltp_update_index` | 89.26ms | 140.46ms | 1.574× | 1.63% | PASS |
| file_writes | `oltp_update_non_index` | 88.44ms | 89.43ms | 1.011× | 13.81% | PASS |
| file_writes | `oltp_delete_insert` | 66.94ms | 108.59ms | 1.622× | 1.58% | PASS |
| file_writes | `oltp_write_only` | 58.46ms | 67.58ms | 1.156× | 17.87% | PASS |
| file_writes | `types_delete_insert` | 40.31ms | 59.30ms | 1.471× | 1.56% | PASS |
| file_writes | `oltp_read_write` | 109.08ms | 133.83ms | 1.227× | 13.29% | PASS |
| ac_reads | `oltp_point_select` | 35.91ms | 43.78ms | 1.219× | 1.43% | PASS |
| ac_reads | `oltp_range_select` | 12.54ms | 13.68ms | 1.091× | 0.95% | PASS |
| ac_reads | `oltp_sum_range` | 12.13ms | 13.74ms | 1.132× | 1.17% | PASS |
| ac_reads | `oltp_order_range` | 2.80ms | 3.04ms | 1.088× | 1.25% | PASS |
| ac_reads | `oltp_distinct_range` | 3.56ms | 3.89ms | 1.094× | 1.42% | PASS |
| ac_reads | `oltp_index_scan` | 5.18ms | 6.87ms | 1.327× | 2.30% | PASS |
| ac_reads | `select_random_points` | 17.23ms | 21.61ms | 1.254× | 1.15% | PASS |
| ac_reads | `select_random_ranges` | 4.54ms | 5.84ms | 1.285× | 2.38% | PASS |
| ac_reads | `covering_index_scan` | 5.62ms | 5.35ms | 0.952× | 2.80% | PASS |
| ac_reads | `groupby_scan` | 29.66ms | 30.66ms | 1.034× | 0.52% | PASS |
| ac_reads | `index_join` | 7.26ms | 9.53ms | 1.312× | 2.24% | PASS |
| ac_reads | `index_join_scan` | 4.21ms | 5.27ms | 1.252× | 1.32% | PASS |
| ac_reads | `types_table_scan` | 981.38ms | 1.10s | 1.125× | 1.26% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.21s | 1.025× | 2.68% | PASS |
| ac_reads | `oltp_read_only` | 120.75ms | 138.87ms | 1.150× | 1.32% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.18ms | 56.15ms | 3.698× | 4.35% | PASS |
| ac_writes | `oltp_insert_ac` | 17.66ms | 69.02ms | 3.909× | 6.00% | PASS |
| ac_writes | `oltp_update_index_ac` | 18.96ms | 81.64ms | 4.305× | 5.36% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.02ms | 65.50ms | 4.088× | 7.28% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.06ms | 75.71ms | 4.437× | 5.72% | PASS |
| ac_writes | `oltp_write_only_ac` | 17.14ms | 75.11ms | 4.381× | 5.38% | PASS |
| ac_writes | `types_delete_insert_ac` | 14.81ms | 65.81ms | 4.444× | 8.31% | PASS |
| ac_writes | `oltp_read_write_ac` | 21.39ms | 81.50ms | 3.809× | 4.41% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 18.71ms | 21.29ms | 1.138× | 2.05% | PASS |
| mem_reads | `oltp_range_select` | 8.57ms | 8.49ms | 0.991× | 3.52% | PASS |
| mem_reads | `oltp_sum_range` | 8.58ms | 8.58ms | 1.000× | 2.24% | PASS |
| mem_reads | `oltp_order_range` | 1.91ms | 1.96ms | 1.024× | 3.84% | PASS |
| mem_reads | `oltp_distinct_range` | 2.50ms | 2.43ms | 0.973× | 1.89% | PASS |
| mem_reads | `oltp_index_scan` | 2.80ms | 3.85ms | 1.377× | 4.16% | PASS |
| mem_reads | `select_random_points` | 12.40ms | 13.48ms | 1.087× | 3.78% | PASS |
| mem_reads | `select_random_ranges` | 2.92ms | 3.40ms | 1.164× | 2.74% | PASS |
| mem_reads | `covering_index_scan` | 2.41ms | 2.69ms | 1.116× | 2.40% | PASS |
| mem_reads | `groupby_scan` | 19.00ms | 18.86ms | 0.993× | 1.40% | PASS |
| mem_reads | `index_join` | 4.32ms | 5.73ms | 1.326× | 4.74% | PASS |
| mem_reads | `index_join_scan` | 3.36ms | 4.40ms | 1.310× | 4.34% | PASS |
| mem_reads | `types_table_scan` | 684.32ms | 732.40ms | 1.070× | 1.22% | PASS |
| mem_reads | `table_scan` | 807.52ms | 836.78ms | 1.036× | 2.52% | PASS |
| mem_reads | `oltp_read_only` | 70.09ms | 71.97ms | 1.027× | 3.63% | PASS |
| mem_writes | `oltp_bulk_insert` | 137.37ms | 197.37ms | 1.437× | 1.37% | PASS |
| mem_writes | `oltp_insert` | 11.64ms | 24.48ms | 2.103× | 1.33% | PASS |
| mem_writes | `oltp_update_index` | 43.75ms | 89.16ms | 2.038× | 2.42% | PASS |
| mem_writes | `oltp_update_non_index` | 33.36ms | 59.00ms | 1.769× | 2.96% | PASS |
| mem_writes | `oltp_delete_insert` | 31.51ms | 70.55ms | 2.239× | 3.40% | PASS |
| mem_writes | `oltp_write_only` | 18.78ms | 44.07ms | 2.347× | 2.86% | PASS |
| mem_writes | `types_delete_insert` | 21.47ms | 37.05ms | 1.725× | 2.62% | PASS |
| mem_writes | `oltp_read_write` | 55.62ms | 87.73ms | 1.577× | 4.27% | PASS |
| file_reads | `oltp_point_select` | 77.42ms | 42.64ms | 0.551× | 2.17% | PASS |
| file_reads | `oltp_range_select` | 15.24ms | 10.68ms | 0.701× | 1.91% | PASS |
| file_reads | `oltp_sum_range` | 14.68ms | 10.34ms | 0.704× | 2.82% | PASS |
| file_reads | `oltp_order_range` | 2.74ms | 2.23ms | 0.811× | 1.70% | PASS |
| file_reads | `oltp_distinct_range` | 3.23ms | 2.65ms | 0.821× | 1.69% | PASS |
| file_reads | `oltp_index_scan` | 9.00ms | 6.11ms | 0.679× | 1.11% | PASS |
| file_reads | `select_random_points` | 18.37ms | 15.04ms | 0.819× | 2.72% | PASS |
| file_reads | `select_random_ranges` | 8.69ms | 5.37ms | 0.618× | 1.14% | PASS |
| file_reads | `covering_index_scan` | 9.29ms | 5.14ms | 0.553× | 1.84% | PASS |
| file_reads | `groupby_scan` | 20.25ms | 19.12ms | 0.944× | 1.63% | PASS |
| file_reads | `index_join` | 8.07ms | 7.46ms | 0.923× | 2.57% | PASS |
| file_reads | `index_join_scan` | 3.85ms | 4.38ms | 1.140× | 1.33% | PASS |
| file_reads | `types_table_scan` | 676.43ms | 727.71ms | 1.076× | 1.75% | PASS |
| file_reads | `table_scan` | 793.61ms | 834.00ms | 1.051× | 1.58% | PASS |
| file_reads | `oltp_read_only` | 155.90ms | 102.68ms | 0.659× | 1.95% | PASS |
| file_writes | `oltp_bulk_insert` | 192.77ms | 269.64ms | 1.399× | 4.59% | PASS |
| file_writes | `oltp_insert` | 29.98ms | 49.02ms | 1.635× | 7.24% | PASS |
| file_writes | `oltp_update_index` | 164.33ms | 177.39ms | 1.079× | 7.95% | PASS |
| file_writes | `oltp_update_non_index` | 130.46ms | 119.46ms | 0.916× | 5.08% | PASS |
| file_writes | `oltp_delete_insert` | 133.08ms | 136.06ms | 1.022× | 12.72% | PASS |
| file_writes | `oltp_write_only` | 99.72ms | 99.66ms | 0.999× | 15.70% | PASS |
| file_writes | `types_delete_insert` | 92.90ms | 81.63ms | 0.879× | 11.85% | PASS |
| file_writes | `oltp_read_write` | 140.63ms | 140.24ms | 0.997× | 10.09% | PASS |
| ac_reads | `oltp_point_select` | 39.13ms | 42.08ms | 1.076× | 1.96% | PASS |
| ac_reads | `oltp_range_select` | 10.70ms | 10.46ms | 0.977× | 2.25% | PASS |
| ac_reads | `oltp_sum_range` | 10.73ms | 10.34ms | 0.964× | 3.19% | PASS |
| ac_reads | `oltp_order_range` | 2.36ms | 2.17ms | 0.921× | 1.39% | PASS |
| ac_reads | `oltp_distinct_range` | 2.99ms | 2.73ms | 0.911× | 0.99% | PASS |
| ac_reads | `oltp_index_scan` | 5.67ms | 6.34ms | 1.119× | 1.81% | PASS |
| ac_reads | `select_random_points` | 15.27ms | 14.95ms | 0.979× | 3.62% | PASS |
| ac_reads | `select_random_ranges` | 5.00ms | 5.35ms | 1.072× | 1.10% | PASS |
| ac_reads | `covering_index_scan` | 5.53ms | 5.14ms | 0.930× | 3.29% | PASS |
| ac_reads | `groupby_scan` | 19.47ms | 19.01ms | 0.976× | 1.33% | PASS |
| ac_reads | `index_join` | 6.34ms | 7.55ms | 1.190× | 2.98% | PASS |
| ac_reads | `index_join_scan` | 3.48ms | 4.38ms | 1.260× | 2.12% | PASS |
| ac_reads | `types_table_scan` | 696.09ms | 743.98ms | 1.069× | 1.27% | PASS |
| ac_reads | `table_scan` | 798.96ms | 839.45ms | 1.051× | 1.85% | PASS |
| ac_reads | `oltp_read_only` | 95.30ms | 100.01ms | 1.049× | 2.50% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 47.11ms | 108.47ms | 2.303× | 39.41% | PASS |
| ac_writes | `oltp_insert_ac` | 39.72ms | 119.97ms | 3.020× | 30.58% | PASS |
| ac_writes | `oltp_update_index_ac` | 34.71ms | 132.95ms | 3.830× | 21.77% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 37.58ms | 249.23ms | 6.631× | 47.44% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 43.92ms | 144.03ms | 3.279× | 26.03% | PASS |
| ac_writes | `oltp_write_only_ac` | 37.83ms | 132.25ms | 3.496× | 33.73% | PASS |
| ac_writes | `types_delete_insert_ac` | 33.49ms | 109.62ms | 3.273× | 22.79% | PASS |
| ac_writes | `oltp_read_write_ac` | 35.32ms | 112.37ms | 3.181× | 11.46% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 35.84ms | 39.79ms | 1.110× | 1.88% | PASS |
| mem_reads | `oltp_range_select` | 21.74ms | 21.31ms | 0.980× | 2.00% | PASS |
| mem_reads | `oltp_sum_range` | 19.60ms | 19.85ms | 1.012× | 1.56% | PASS |
| mem_reads | `oltp_order_range` | 3.83ms | 3.85ms | 1.004× | 1.24% | PASS |
| mem_reads | `oltp_distinct_range` | 4.89ms | 4.87ms | 0.997× | 1.25% | PASS |
| mem_reads | `oltp_index_scan` | 4.92ms | 6.28ms | 1.278× | 2.73% | PASS |
| mem_reads | `select_random_points` | 28.95ms | 31.33ms | 1.082× | 1.63% | PASS |
| mem_reads | `select_random_ranges` | 7.82ms | 8.44ms | 1.079× | 1.26% | PASS |
| mem_reads | `covering_index_scan` | 4.41ms | 4.31ms | 0.977× | 2.45% | PASS |
| mem_reads | `groupby_scan` | 38.82ms | 40.19ms | 1.035× | 0.62% | PASS |
| mem_reads | `index_join` | 8.30ms | 10.61ms | 1.277× | 2.24% | PASS |
| mem_reads | `index_join_scan` | 4.45ms | 5.77ms | 1.297× | 1.91% | PASS |
| mem_reads | `types_table_scan` | 1.18s | 1.28s | 1.079× | 2.25% | PASS |
| mem_reads | `table_scan` | 1.40s | 1.38s | 0.985× | 4.38% | PASS |
| mem_reads | `oltp_read_only` | 156.65ms | 162.05ms | 1.034× | 1.49% | PASS |
| mem_writes | `oltp_bulk_insert` | 244.18ms | 339.51ms | 1.390× | 1.02% | PASS |
| mem_writes | `oltp_insert` | 19.56ms | 36.50ms | 1.866× | 0.76% | PASS |
| mem_writes | `oltp_update_index` | 71.68ms | 124.92ms | 1.743× | 1.25% | PASS |
| mem_writes | `oltp_update_non_index` | 54.30ms | 87.31ms | 1.608× | 1.31% | PASS |
| mem_writes | `oltp_delete_insert` | 53.65ms | 102.80ms | 1.916× | 1.23% | PASS |
| mem_writes | `oltp_write_only` | 28.91ms | 62.11ms | 2.148× | 1.35% | PASS |
| mem_writes | `types_delete_insert` | 34.53ms | 55.95ms | 1.620× | 1.46% | PASS |
| mem_writes | `oltp_read_write` | 108.87ms | 155.72ms | 1.430× | 1.45% | PASS |
| file_reads | `oltp_point_select` | 130.89ms | 70.36ms | 0.538× | 1.26% | PASS |
| file_reads | `oltp_range_select` | 30.49ms | 23.76ms | 0.779× | 2.07% | PASS |
| file_reads | `oltp_sum_range` | 28.65ms | 22.91ms | 0.800× | 1.62% | PASS |
| file_reads | `oltp_order_range` | 4.74ms | 4.19ms | 0.884× | 1.60% | PASS |
| file_reads | `oltp_distinct_range` | 5.89ms | 5.25ms | 0.892× | 1.22% | PASS |
| file_reads | `oltp_index_scan` | 14.69ms | 9.54ms | 0.649× | 1.35% | PASS |
| file_reads | `select_random_points` | 39.00ms | 34.80ms | 0.892× | 1.96% | PASS |
| file_reads | `select_random_ranges` | 17.71ms | 11.82ms | 0.668× | 0.97% | PASS |
| file_reads | `covering_index_scan` | 14.26ms | 7.87ms | 0.552× | 1.15% | PASS |
| file_reads | `groupby_scan` | 40.21ms | 41.02ms | 1.020× | 1.24% | PASS |
| file_reads | `index_join` | 13.39ms | 12.70ms | 0.948× | 1.07% | PASS |
| file_reads | `index_join_scan` | 5.37ms | 6.17ms | 1.150× | 1.57% | PASS |
| file_reads | `types_table_scan` | 1.16s | 1.26s | 1.088× | 2.32% | PASS |
| file_reads | `table_scan` | 1.52s | 1.40s | 0.918× | 3.01% | PASS |
| file_reads | `oltp_read_only` | 300.45ms | 211.22ms | 0.703× | 0.94% | PASS |
| file_writes | `oltp_bulk_insert` | 262.48ms | 369.96ms | 1.409× | 1.29% | PASS |
| file_writes | `oltp_insert` | 27.02ms | 48.01ms | 1.777× | 1.60% | PASS |
| file_writes | `oltp_update_index` | 106.95ms | 156.60ms | 1.464× | 2.07% | PASS |
| file_writes | `oltp_update_non_index` | 83.44ms | 110.71ms | 1.327× | 1.82% | PASS |
| file_writes | `oltp_delete_insert` | 81.71ms | 126.55ms | 1.549× | 0.99% | PASS |
| file_writes | `oltp_write_only` | 52.89ms | 83.37ms | 1.576× | 2.52% | PASS |
| file_writes | `types_delete_insert` | 52.77ms | 70.51ms | 1.336× | 1.12% | PASS |
| file_writes | `oltp_read_write` | 136.82ms | 177.24ms | 1.295× | 2.15% | PASS |
| ac_reads | `oltp_point_select` | 66.40ms | 71.47ms | 1.076× | 1.10% | PASS |
| ac_reads | `oltp_range_select` | 24.14ms | 23.86ms | 0.988× | 1.24% | PASS |
| ac_reads | `oltp_sum_range` | 22.21ms | 22.85ms | 1.029× | 1.02% | PASS |
| ac_reads | `oltp_order_range` | 4.25ms | 4.17ms | 0.981× | 1.28% | PASS |
| ac_reads | `oltp_distinct_range` | 5.30ms | 5.24ms | 0.989× | 1.35% | PASS |
| ac_reads | `oltp_index_scan` | 8.34ms | 9.44ms | 1.132× | 1.49% | PASS |
| ac_reads | `select_random_points` | 31.67ms | 34.05ms | 1.075× | 1.36% | PASS |
| ac_reads | `select_random_ranges` | 11.09ms | 11.76ms | 1.060× | 1.09% | PASS |
| ac_reads | `covering_index_scan` | 7.93ms | 7.80ms | 0.983× | 1.23% | PASS |
| ac_reads | `groupby_scan` | 39.12ms | 40.92ms | 1.046× | 0.93% | PASS |
| ac_reads | `index_join` | 10.33ms | 12.77ms | 1.236× | 1.28% | PASS |
| ac_reads | `index_join_scan` | 4.90ms | 6.25ms | 1.274× | 2.18% | PASS |
| ac_reads | `types_table_scan` | 1.16s | 1.26s | 1.090× | 1.60% | PASS |
| ac_reads | `table_scan` | 1.39s | 1.37s | 0.987× | 2.93% | PASS |
| ac_reads | `oltp_read_only` | 201.25ms | 207.73ms | 1.032× | 1.36% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.10ms | 63.42ms | 3.940× | 4.35% | PASS |
| ac_writes | `oltp_insert_ac` | 20.15ms | 90.82ms | 4.508× | 7.81% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.52ms | 100.16ms | 4.655× | 5.87% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.86ms | 78.10ms | 4.372× | 7.68% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 19.90ms | 92.40ms | 4.643× | 5.81% | PASS |
| ac_writes | `oltp_write_only_ac` | 20.04ms | 94.31ms | 4.706× | 5.57% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.39ms | 80.99ms | 4.656× | 7.45% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.43ms | 100.81ms | 3.675× | 6.36% | PASS |

</details>

## Version-control latency

Wall time: 2m 21s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 50.40ms | 200.00ms | 25.2% | 1.15% | PASS |
| `status_dirty_many_tables` | 52.12ms | 200.00ms | 26.1% | 1.15% | PASS |
| `diff_regular_working_one_table` | 46.90ms | 150.00ms | 31.3% | 1.38% | PASS |
| `diff_regular_working_many_tables` | 55.30ms | 200.00ms | 27.7% | 0.74% | PASS |
| `diff_stat_working_many_tables` | 56.50ms | 200.00ms | 28.2% | 1.72% | PASS |
| `diff_schema_working_many_tables` | 55.68ms | 200.00ms | 27.8% | 1.39% | PASS |
| `branch_list_many_branches` | 15.48ms | 100.00ms | 15.5% | 2.14% | PASS |
| `branch_create_delete` | 103.53ms | 100.00ms | 103.5% | 77.17% | FAIL |
| `checkout_branch_clean` | 165.16ms | 200.00ms | 82.6% | 33.02% | PASS |
| `merge_data_no_conflicts` | 124.18ms | 150.00ms | 82.8% | 57.78% | PASS |
| `merge_schema_no_conflicts` | 92.28ms | 100.00ms | 92.3% | 80.57% | PASS |
| `merge_data_conflicts` | 57.67ms | 250.00ms | 23.1% | 0.82% | PASS |
| `merge_data_conflicts_with_resolve` | 57.89ms | 250.00ms | 23.2% | 1.07% | PASS |

Version-control ceiling result: **FAIL**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
