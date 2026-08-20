# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-20 11:44 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260816.277.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/32356837548)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 1m 35s | 7.92s | 9.71s | 1.226× | 1.12% | **PASS** |
| textpk | 69 | 55 | 1h 40m 3s | 11.54s | 11.85s | 1.026× | 2.86% | **PASS** |
| blobpk | 69 | 55 | 1h 18m 50s | 9.09s | 10.42s | 1.146× | 1.38% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 23s | 9.68s | 12.13s | 1.253× | 1.38% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.85ms | 23.92ms | 1.147× | 0.93% | PASS |
| mem_reads | `oltp_range_select` | 9.10ms | 10.52ms | 1.156× | 1.00% | PASS |
| mem_reads | `oltp_sum_range` | 8.62ms | 10.48ms | 1.215× | 1.00% | PASS |
| mem_reads | `oltp_order_range` | 2.35ms | 2.59ms | 1.104× | 0.80% | PASS |
| mem_reads | `oltp_distinct_range` | 3.09ms | 3.39ms | 1.099× | 0.48% | PASS |
| mem_reads | `oltp_index_scan` | 3.44ms | 4.54ms | 1.320× | 1.74% | PASS |
| mem_reads | `select_random_points` | 9.37ms | 10.48ms | 1.118× | 1.00% | PASS |
| mem_reads | `select_random_ranges` | 2.64ms | 3.45ms | 1.309× | 0.99% | PASS |
| mem_reads | `covering_index_scan` | 3.62ms | 3.62ms | 1.001× | 1.84% | PASS |
| mem_reads | `groupby_scan` | 27.37ms | 29.42ms | 1.075× | 0.47% | PASS |
| mem_reads | `index_join` | 5.14ms | 7.48ms | 1.456× | 0.99% | PASS |
| mem_reads | `index_join_scan` | 2.77ms | 4.17ms | 1.505× | 0.75% | PASS |
| mem_reads | `types_table_scan` | 944.85ms | 1.11s | 1.180× | 0.66% | PASS |
| mem_reads | `table_scan` | 1.10s | 1.23s | 1.118× | 0.82% | PASS |
| mem_reads | `oltp_read_only` | 90.32ms | 103.79ms | 1.149× | 0.79% | PASS |
| mem_writes | `oltp_bulk_insert` | 154.47ms | 208.73ms | 1.351× | 1.44% | PASS |
| mem_writes | `oltp_insert` | 13.37ms | 23.60ms | 1.766× | 1.21% | PASS |
| mem_writes | `oltp_update_index` | 44.03ms | 90.56ms | 2.057× | 1.14% | PASS |
| mem_writes | `oltp_update_non_index` | 29.20ms | 50.02ms | 1.713× | 1.04% | PASS |
| mem_writes | `oltp_delete_insert` | 39.47ms | 66.85ms | 1.694× | 0.79% | PASS |
| mem_writes | `oltp_write_only` | 18.41ms | 37.86ms | 2.056× | 1.09% | PASS |
| mem_writes | `types_delete_insert` | 21.26ms | 33.75ms | 1.588× | 0.96% | PASS |
| mem_writes | `oltp_read_write` | 57.52ms | 94.03ms | 1.635× | 1.42% | PASS |
| file_reads | `oltp_point_select` | 52.56ms | 35.60ms | 0.677× | 0.53% | PASS |
| file_reads | `oltp_range_select` | 12.33ms | 11.98ms | 0.972× | 0.99% | PASS |
| file_reads | `oltp_sum_range` | 11.96ms | 11.98ms | 1.002× | 1.23% | PASS |
| file_reads | `oltp_order_range` | 2.76ms | 2.77ms | 1.006× | 0.97% | PASS |
| file_reads | `oltp_distinct_range` | 3.51ms | 3.59ms | 1.023× | 0.87% | PASS |
| file_reads | `oltp_index_scan` | 6.84ms | 6.07ms | 0.887× | 1.27% | PASS |
| file_reads | `select_random_points` | 13.02ms | 11.92ms | 0.915× | 1.99% | PASS |
| file_reads | `select_random_ranges` | 5.78ms | 4.63ms | 0.801× | 1.10% | PASS |
| file_reads | `covering_index_scan` | 6.91ms | 5.03ms | 0.727× | 1.25% | PASS |
| file_reads | `groupby_scan` | 27.74ms | 29.71ms | 1.071× | 0.44% | PASS |
| file_reads | `index_join` | 7.06ms | 8.49ms | 1.203× | 1.16% | PASS |
| file_reads | `index_join_scan` | 3.22ms | 4.37ms | 1.356× | 1.89% | PASS |
| file_reads | `types_table_scan` | 958.28ms | 1.13s | 1.176× | 0.97% | PASS |
| file_reads | `table_scan` | 1.12s | 1.24s | 1.104× | 0.70% | PASS |
| file_reads | `oltp_read_only` | 138.37ms | 122.38ms | 0.884× | 0.73% | PASS |
| file_writes | `oltp_bulk_insert` | 166.33ms | 223.41ms | 1.343× | 1.88% | PASS |
| file_writes | `oltp_insert` | 17.53ms | 28.38ms | 1.619× | 1.42% | PASS |
| file_writes | `oltp_update_index` | 60.43ms | 107.92ms | 1.786× | 1.28% | PASS |
| file_writes | `oltp_update_non_index` | 42.62ms | 64.46ms | 1.512× | 1.72% | PASS |
| file_writes | `oltp_delete_insert` | 51.59ms | 83.07ms | 1.610× | 1.01% | PASS |
| file_writes | `oltp_write_only` | 31.38ms | 50.55ms | 1.611× | 1.69% | PASS |
| file_writes | `types_delete_insert` | 30.48ms | 42.18ms | 1.384× | 1.39% | PASS |
| file_writes | `oltp_read_write` | 71.34ms | 107.48ms | 1.507× | 1.10% | PASS |
| ac_reads | `oltp_point_select` | 31.13ms | 35.74ms | 1.148× | 0.96% | PASS |
| ac_reads | `oltp_range_select` | 10.21ms | 11.96ms | 1.171× | 1.44% | PASS |
| ac_reads | `oltp_sum_range` | 9.80ms | 12.07ms | 1.231× | 1.24% | PASS |
| ac_reads | `oltp_order_range` | 2.51ms | 2.77ms | 1.103× | 1.25% | PASS |
| ac_reads | `oltp_distinct_range` | 3.26ms | 3.60ms | 1.102× | 0.72% | PASS |
| ac_reads | `oltp_index_scan` | 4.70ms | 6.09ms | 1.297× | 1.22% | PASS |
| ac_reads | `select_random_points` | 10.88ms | 12.00ms | 1.103× | 1.58% | PASS |
| ac_reads | `select_random_ranges` | 3.70ms | 4.62ms | 1.250× | 1.12% | PASS |
| ac_reads | `covering_index_scan` | 4.76ms | 5.04ms | 1.059× | 1.14% | PASS |
| ac_reads | `groupby_scan` | 27.58ms | 29.73ms | 1.078× | 0.42% | PASS |
| ac_reads | `index_join` | 5.99ms | 8.63ms | 1.441× | 1.13% | PASS |
| ac_reads | `index_join_scan` | 3.03ms | 4.41ms | 1.456× | 1.16% | PASS |
| ac_reads | `types_table_scan` | 961.04ms | 1.13s | 1.172× | 0.58% | PASS |
| ac_reads | `table_scan` | 1.14s | 1.25s | 1.094× | 0.43% | PASS |
| ac_reads | `oltp_read_only` | 107.80ms | 124.22ms | 1.152× | 0.57% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.82ms | 58.72ms | 3.711× | 8.77% | PASS |
| ac_writes | `oltp_insert_ac` | 17.52ms | 71.36ms | 4.073× | 6.89% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.46ms | 84.14ms | 4.325× | 5.65% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 15.37ms | 65.30ms | 4.248× | 6.83% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.47ms | 76.30ms | 4.367× | 4.77% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.15ms | 75.53ms | 4.162× | 6.43% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.26ms | 65.73ms | 4.308× | 7.17% | PASS |
| ac_writes | `oltp_read_write_ac` | 21.04ms | 80.46ms | 3.825× | 5.53% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.68ms | 34.44ms | 1.160× | 1.95% | PASS |
| mem_reads | `oltp_range_select` | 16.27ms | 15.09ms | 0.928× | 9.87% | PASS |
| mem_reads | `oltp_sum_range` | 13.68ms | 14.38ms | 1.052× | 9.52% | PASS |
| mem_reads | `oltp_order_range` | 3.19ms | 3.35ms | 1.048× | 11.47% | PASS |
| mem_reads | `oltp_distinct_range` | 4.20ms | 4.12ms | 0.981× | 5.99% | PASS |
| mem_reads | `oltp_index_scan` | 4.41ms | 5.74ms | 1.301× | 3.06% | PASS |
| mem_reads | `select_random_points` | 17.89ms | 20.24ms | 1.132× | 2.38% | PASS |
| mem_reads | `select_random_ranges` | 4.30ms | 5.21ms | 1.212× | 2.38% | PASS |
| mem_reads | `covering_index_scan` | 4.72ms | 4.40ms | 0.932× | 5.41% | PASS |
| mem_reads | `groupby_scan` | 33.59ms | 36.14ms | 1.076× | 4.83% | PASS |
| mem_reads | `index_join` | 6.86ms | 8.81ms | 1.284× | 3.47% | PASS |
| mem_reads | `index_join_scan` | 5.13ms | 6.24ms | 1.216× | 3.72% | PASS |
| mem_reads | `types_table_scan` | 1.09s | 1.16s | 1.073× | 1.89% | PASS |
| mem_reads | `table_scan` | 1.41s | 1.30s | 0.920× | 3.50% | PASS |
| mem_reads | `oltp_read_only` | 118.68ms | 124.67ms | 1.050× | 3.40% | PASS |
| mem_writes | `oltp_bulk_insert` | 212.80ms | 308.27ms | 1.449× | 1.59% | PASS |
| mem_writes | `oltp_insert` | 21.19ms | 35.62ms | 1.681× | 1.69% | PASS |
| mem_writes | `oltp_update_index` | 72.74ms | 127.94ms | 1.759× | 2.07% | PASS |
| mem_writes | `oltp_update_non_index` | 49.32ms | 82.68ms | 1.676× | 2.54% | PASS |
| mem_writes | `oltp_delete_insert` | 50.66ms | 97.93ms | 1.933× | 2.50% | PASS |
| mem_writes | `oltp_write_only` | 29.43ms | 58.37ms | 1.983× | 2.26% | PASS |
| mem_writes | `types_delete_insert` | 32.27ms | 49.53ms | 1.535× | 2.01% | PASS |
| mem_writes | `oltp_read_write` | 84.52ms | 124.66ms | 1.475× | 2.66% | PASS |
| file_reads | `oltp_point_select` | 115.99ms | 61.55ms | 0.531× | 1.10% | PASS |
| file_reads | `oltp_range_select` | 24.73ms | 16.05ms | 0.649× | 4.43% | PASS |
| file_reads | `oltp_sum_range` | 22.95ms | 16.71ms | 0.728× | 4.35% | PASS |
| file_reads | `oltp_order_range` | 4.27ms | 3.39ms | 0.794× | 3.93% | PASS |
| file_reads | `oltp_distinct_range` | 5.16ms | 4.25ms | 0.825× | 2.86% | PASS |
| file_reads | `oltp_index_scan` | 13.79ms | 9.05ms | 0.657× | 1.86% | PASS |
| file_reads | `select_random_points` | 27.45ms | 22.87ms | 0.833× | 1.38% | PASS |
| file_reads | `select_random_ranges` | 13.29ms | 8.05ms | 0.606× | 1.46% | PASS |
| file_reads | `covering_index_scan` | 15.36ms | 7.66ms | 0.499× | 2.11% | PASS |
| file_reads | `groupby_scan` | 35.24ms | 35.27ms | 1.001× | 2.25% | PASS |
| file_reads | `index_join` | 12.91ms | 11.42ms | 0.885× | 2.47% | PASS |
| file_reads | `index_join_scan` | 6.21ms | 6.63ms | 1.067× | 3.95% | PASS |
| file_reads | `types_table_scan` | 1.11s | 1.18s | 1.059× | 1.56% | PASS |
| file_reads | `table_scan` | 1.93s | 1.44s | 0.746× | 4.48% | PASS |
| file_reads | `oltp_read_only` | 243.37ms | 160.28ms | 0.659× | 1.47% | PASS |
| file_writes | `oltp_bulk_insert` | 278.73ms | 390.89ms | 1.402× | 2.77% | PASS |
| file_writes | `oltp_insert` | 64.41ms | 66.62ms | 1.034× | 8.19% | PASS |
| file_writes | `oltp_update_index` | 223.46ms | 244.14ms | 1.093× | 4.59% | PASS |
| file_writes | `oltp_update_non_index` | 171.59ms | 153.16ms | 0.893× | 2.59% | PASS |
| file_writes | `oltp_delete_insert` | 181.62ms | 181.93ms | 1.002× | 4.64% | PASS |
| file_writes | `oltp_write_only` | 132.12ms | 129.22ms | 0.978× | 1.08% | PASS |
| file_writes | `types_delete_insert` | 112.90ms | 100.28ms | 0.888× | 7.66% | PASS |
| file_writes | `oltp_read_write` | 192.62ms | 198.95ms | 1.033× | 4.37% | PASS |
| ac_reads | `oltp_point_select` | 58.30ms | 62.00ms | 1.063× | 1.32% | PASS |
| ac_reads | `oltp_range_select` | 18.54ms | 15.84ms | 0.854× | 3.96% | PASS |
| ac_reads | `oltp_sum_range` | 16.50ms | 15.78ms | 0.957× | 3.03% | PASS |
| ac_reads | `oltp_order_range` | 3.67ms | 3.33ms | 0.906× | 1.65% | PASS |
| ac_reads | `oltp_distinct_range` | 4.65ms | 4.24ms | 0.912× | 2.42% | PASS |
| ac_reads | `oltp_index_scan` | 8.09ms | 9.02ms | 1.115× | 1.74% | PASS |
| ac_reads | `select_random_points` | 21.48ms | 22.79ms | 1.061× | 1.76% | PASS |
| ac_reads | `select_random_ranges` | 7.43ms | 8.03ms | 1.080× | 1.39% | PASS |
| ac_reads | `covering_index_scan` | 9.36ms | 7.58ms | 0.811× | 1.79% | PASS |
| ac_reads | `groupby_scan` | 33.34ms | 33.91ms | 1.017× | 1.94% | PASS |
| ac_reads | `index_join` | 10.21ms | 11.52ms | 1.128× | 3.41% | PASS |
| ac_reads | `index_join_scan` | 5.70ms | 6.66ms | 1.169× | 2.02% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.17s | 1.048× | 3.46% | PASS |
| ac_reads | `table_scan` | 1.51s | 1.26s | 0.837× | 10.48% | PASS |
| ac_reads | `oltp_read_only` | 157.67ms | 160.04ms | 1.015× | 2.65% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 35.63ms | 96.04ms | 2.695× | 8.15% | PASS |
| ac_writes | `oltp_insert_ac` | 39.39ms | 116.15ms | 2.948× | 10.62% | PASS |
| ac_writes | `oltp_update_index_ac` | 44.30ms | 138.92ms | 3.136× | 13.96% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 37.27ms | 112.53ms | 3.019× | 10.89% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 38.61ms | 139.12ms | 3.604× | 21.54% | PASS |
| ac_writes | `oltp_write_only_ac` | 39.15ms | 126.82ms | 3.239× | 12.23% | PASS |
| ac_writes | `types_delete_insert_ac` | 37.14ms | 122.73ms | 3.304× | 18.72% | PASS |
| ac_writes | `oltp_read_write_ac` | 47.37ms | 134.80ms | 2.846× | 8.13% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 27.32ms | 32.42ms | 1.187× | 1.40% | PASS |
| mem_reads | `oltp_range_select` | 11.82ms | 12.26ms | 1.037× | 1.50% | PASS |
| mem_reads | `oltp_sum_range` | 11.44ms | 12.28ms | 1.073× | 1.28% | PASS |
| mem_reads | `oltp_order_range` | 2.64ms | 2.81ms | 1.063× | 1.23% | PASS |
| mem_reads | `oltp_distinct_range` | 3.41ms | 3.61ms | 1.060× | 0.70% | PASS |
| mem_reads | `oltp_index_scan` | 4.02ms | 5.53ms | 1.377× | 1.61% | PASS |
| mem_reads | `select_random_points` | 16.48ms | 20.38ms | 1.237× | 2.42% | PASS |
| mem_reads | `select_random_ranges` | 3.45ms | 4.54ms | 1.318× | 1.28% | PASS |
| mem_reads | `covering_index_scan` | 3.89ms | 4.12ms | 1.059× | 1.35% | PASS |
| mem_reads | `groupby_scan` | 29.30ms | 30.17ms | 1.030× | 0.52% | PASS |
| mem_reads | `index_join` | 5.94ms | 8.61ms | 1.448× | 1.94% | PASS |
| mem_reads | `index_join_scan` | 3.46ms | 5.01ms | 1.446× | 1.23% | PASS |
| mem_reads | `types_table_scan` | 1.02s | 1.12s | 1.098× | 2.18% | PASS |
| mem_reads | `table_scan` | 1.28s | 1.25s | 0.973× | 0.33% | PASS |
| mem_reads | `oltp_read_only` | 110.06ms | 122.97ms | 1.117× | 0.49% | PASS |
| mem_writes | `oltp_bulk_insert` | 205.38ms | 297.14ms | 1.447× | 1.38% | PASS |
| mem_writes | `oltp_insert` | 17.81ms | 34.20ms | 1.920× | 1.36% | PASS |
| mem_writes | `oltp_update_index` | 62.77ms | 121.42ms | 1.934× | 1.47% | PASS |
| mem_writes | `oltp_update_non_index` | 43.19ms | 73.19ms | 1.695× | 1.17% | PASS |
| mem_writes | `oltp_delete_insert` | 43.77ms | 91.99ms | 2.102× | 1.29% | PASS |
| mem_writes | `oltp_write_only` | 24.25ms | 53.79ms | 2.218× | 1.25% | PASS |
| mem_writes | `types_delete_insert` | 28.22ms | 47.82ms | 1.695× | 1.10% | PASS |
| mem_writes | `oltp_read_write` | 77.55ms | 125.48ms | 1.618× | 1.27% | PASS |
| file_reads | `oltp_point_select` | 60.11ms | 44.83ms | 0.746× | 1.20% | PASS |
| file_reads | `oltp_range_select` | 15.15ms | 13.59ms | 0.897× | 1.39% | PASS |
| file_reads | `oltp_sum_range` | 14.95ms | 13.70ms | 0.916× | 1.29% | PASS |
| file_reads | `oltp_order_range` | 2.97ms | 2.99ms | 1.009× | 1.44% | PASS |
| file_reads | `oltp_distinct_range` | 3.77ms | 3.80ms | 1.008× | 1.25% | PASS |
| file_reads | `oltp_index_scan` | 7.28ms | 6.80ms | 0.934× | 1.41% | PASS |
| file_reads | `select_random_points` | 19.44ms | 21.19ms | 1.090× | 1.40% | PASS |
| file_reads | `select_random_ranges` | 6.68ms | 5.78ms | 0.864× | 1.22% | PASS |
| file_reads | `covering_index_scan` | 7.28ms | 5.37ms | 0.737× | 1.76% | PASS |
| file_reads | `groupby_scan` | 29.65ms | 30.44ms | 1.027× | 0.60% | PASS |
| file_reads | `index_join` | 8.45ms | 9.84ms | 1.164× | 1.83% | PASS |
| file_reads | `index_join_scan` | 4.08ms | 5.44ms | 1.333× | 2.23% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.14s | 1.069× | 0.43% | PASS |
| file_reads | `table_scan` | 1.29s | 1.25s | 0.972× | 0.37% | PASS |
| file_reads | `oltp_read_only` | 159.37ms | 141.76ms | 0.889× | 0.75% | PASS |
| file_writes | `oltp_bulk_insert` | 219.92ms | 315.19ms | 1.433× | 1.17% | PASS |
| file_writes | `oltp_insert` | 25.00ms | 42.96ms | 1.718× | 2.75% | PASS |
| file_writes | `oltp_update_index` | 81.62ms | 140.44ms | 1.721× | 1.53% | PASS |
| file_writes | `oltp_update_non_index` | 58.95ms | 87.39ms | 1.482× | 1.26% | PASS |
| file_writes | `oltp_delete_insert` | 60.14ms | 107.52ms | 1.788× | 1.72% | PASS |
| file_writes | `oltp_write_only` | 38.00ms | 66.86ms | 1.760× | 2.11% | PASS |
| file_writes | `types_delete_insert` | 39.15ms | 58.58ms | 1.496× | 1.35% | PASS |
| file_writes | `oltp_read_write` | 89.83ms | 136.66ms | 1.521× | 1.57% | PASS |
| ac_reads | `oltp_point_select` | 37.81ms | 45.01ms | 1.190× | 1.30% | PASS |
| ac_reads | `oltp_range_select` | 12.94ms | 13.75ms | 1.063× | 1.32% | PASS |
| ac_reads | `oltp_sum_range` | 12.74ms | 13.91ms | 1.092× | 1.73% | PASS |
| ac_reads | `oltp_order_range` | 2.81ms | 3.05ms | 1.086× | 1.56% | PASS |
| ac_reads | `oltp_distinct_range` | 3.59ms | 3.86ms | 1.075× | 1.49% | PASS |
| ac_reads | `oltp_index_scan` | 5.46ms | 6.96ms | 1.274× | 1.35% | PASS |
| ac_reads | `select_random_points` | 18.42ms | 22.50ms | 1.222× | 1.60% | PASS |
| ac_reads | `select_random_ranges` | 4.72ms | 5.83ms | 1.235× | 1.63% | PASS |
| ac_reads | `covering_index_scan` | 5.46ms | 5.42ms | 0.992× | 1.19% | PASS |
| ac_reads | `groupby_scan` | 29.77ms | 30.66ms | 1.030× | 0.50% | PASS |
| ac_reads | `index_join` | 7.28ms | 9.94ms | 1.366× | 2.12% | PASS |
| ac_reads | `index_join_scan` | 3.91ms | 5.46ms | 1.396× | 1.39% | PASS |
| ac_reads | `types_table_scan` | 1.07s | 1.14s | 1.072× | 0.55% | PASS |
| ac_reads | `table_scan` | 1.27s | 1.24s | 0.977× | 0.73% | PASS |
| ac_reads | `oltp_read_only` | 123.15ms | 139.46ms | 1.132× | 1.38% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.78ms | 58.60ms | 3.714× | 5.23% | PASS |
| ac_writes | `oltp_insert_ac` | 17.70ms | 77.12ms | 4.357× | 5.84% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.69ms | 85.99ms | 4.367× | 5.67% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 15.80ms | 68.10ms | 4.311× | 5.69% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.04ms | 78.81ms | 4.626× | 7.15% | PASS |
| ac_writes | `oltp_write_only_ac` | 17.40ms | 76.47ms | 4.395× | 4.35% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.49ms | 71.79ms | 4.635× | 8.59% | PASS |
| ac_writes | `oltp_read_write_ac` | 21.44ms | 83.65ms | 3.902× | 5.14% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 35.00ms | 41.34ms | 1.181× | 1.73% | PASS |
| mem_reads | `oltp_range_select` | 19.48ms | 21.41ms | 1.099× | 2.11% | PASS |
| mem_reads | `oltp_sum_range` | 17.95ms | 20.96ms | 1.168× | 1.38% | PASS |
| mem_reads | `oltp_order_range` | 3.50ms | 3.86ms | 1.102× | 1.20% | PASS |
| mem_reads | `oltp_distinct_range` | 4.64ms | 4.94ms | 1.064× | 1.19% | PASS |
| mem_reads | `oltp_index_scan` | 4.44ms | 6.13ms | 1.381× | 2.12% | PASS |
| mem_reads | `select_random_points` | 26.33ms | 31.74ms | 1.206× | 0.72% | PASS |
| mem_reads | `select_random_ranges` | 7.62ms | 9.01ms | 1.182× | 2.48% | PASS |
| mem_reads | `covering_index_scan` | 4.27ms | 4.48ms | 1.050× | 2.17% | PASS |
| mem_reads | `groupby_scan` | 36.82ms | 38.83ms | 1.055× | 1.06% | PASS |
| mem_reads | `index_join` | 8.29ms | 10.84ms | 1.306× | 1.97% | PASS |
| mem_reads | `index_join_scan` | 4.17ms | 5.43ms | 1.301× | 2.26% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.23s | 1.186× | 0.72% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.39s | 1.182× | 0.81% | PASS |
| mem_reads | `oltp_read_only` | 147.02ms | 169.17ms | 1.151× | 0.79% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.05ms | 357.18ms | 1.434× | 0.97% | PASS |
| mem_writes | `oltp_insert` | 19.20ms | 36.30ms | 1.891× | 0.67% | PASS |
| mem_writes | `oltp_update_index` | 66.51ms | 115.81ms | 1.741× | 1.17% | PASS |
| mem_writes | `oltp_update_non_index` | 50.73ms | 82.91ms | 1.634× | 1.47% | PASS |
| mem_writes | `oltp_delete_insert` | 49.25ms | 95.68ms | 1.943× | 1.13% | PASS |
| mem_writes | `oltp_write_only` | 26.18ms | 56.84ms | 2.171× | 0.95% | PASS |
| mem_writes | `types_delete_insert` | 32.10ms | 54.43ms | 1.695× | 1.00% | PASS |
| mem_writes | `oltp_read_write` | 98.83ms | 153.78ms | 1.556× | 1.08% | PASS |
| file_reads | `oltp_point_select` | 108.93ms | 66.87ms | 0.614× | 1.00% | PASS |
| file_reads | `oltp_range_select` | 27.07ms | 24.20ms | 0.894× | 1.24% | PASS |
| file_reads | `oltp_sum_range` | 26.30ms | 24.03ms | 0.914× | 1.00% | PASS |
| file_reads | `oltp_order_range` | 4.55ms | 4.29ms | 0.943× | 1.20% | PASS |
| file_reads | `oltp_distinct_range` | 5.74ms | 5.46ms | 0.951× | 1.51% | PASS |
| file_reads | `oltp_index_scan` | 12.64ms | 9.28ms | 0.734× | 1.18% | PASS |
| file_reads | `select_random_points` | 37.83ms | 36.12ms | 0.955× | 1.05% | PASS |
| file_reads | `select_random_ranges` | 15.81ms | 12.16ms | 0.769× | 1.48% | PASS |
| file_reads | `covering_index_scan` | 12.18ms | 7.23ms | 0.593× | 1.18% | PASS |
| file_reads | `groupby_scan` | 37.47ms | 39.40ms | 1.051× | 1.29% | PASS |
| file_reads | `index_join` | 12.80ms | 12.74ms | 0.995× | 1.67% | PASS |
| file_reads | `index_join_scan` | 5.20ms | 5.89ms | 1.133× | 1.44% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.23s | 1.193× | 0.51% | PASS |
| file_reads | `table_scan` | 1.18s | 1.39s | 1.181× | 0.80% | PASS |
| file_reads | `oltp_read_only` | 261.63ms | 210.06ms | 0.803× | 1.34% | PASS |
| file_writes | `oltp_bulk_insert` | 264.36ms | 378.22ms | 1.431× | 1.00% | PASS |
| file_writes | `oltp_insert` | 26.85ms | 47.15ms | 1.756× | 2.07% | PASS |
| file_writes | `oltp_update_index` | 102.17ms | 147.91ms | 1.448× | 1.70% | PASS |
| file_writes | `oltp_update_non_index` | 80.52ms | 108.07ms | 1.342× | 1.79% | PASS |
| file_writes | `oltp_delete_insert` | 77.97ms | 120.33ms | 1.543× | 1.97% | PASS |
| file_writes | `oltp_write_only` | 52.99ms | 79.33ms | 1.497× | 2.94% | PASS |
| file_writes | `types_delete_insert` | 52.23ms | 71.00ms | 1.359× | 1.29% | PASS |
| file_writes | `oltp_read_write` | 138.63ms | 181.66ms | 1.310× | 2.46% | PASS |
| ac_reads | `oltp_point_select` | 59.83ms | 68.12ms | 1.139× | 1.07% | PASS |
| ac_reads | `oltp_range_select` | 22.85ms | 24.52ms | 1.073× | 1.39% | PASS |
| ac_reads | `oltp_sum_range` | 20.87ms | 24.11ms | 1.155× | 2.22% | PASS |
| ac_reads | `oltp_order_range` | 3.90ms | 4.27ms | 1.092× | 1.66% | PASS |
| ac_reads | `oltp_distinct_range` | 5.06ms | 5.39ms | 1.065× | 1.80% | PASS |
| ac_reads | `oltp_index_scan` | 7.24ms | 9.25ms | 1.278× | 2.01% | PASS |
| ac_reads | `select_random_points` | 30.80ms | 36.10ms | 1.172× | 1.49% | PASS |
| ac_reads | `select_random_ranges` | 10.27ms | 12.07ms | 1.175× | 1.18% | PASS |
| ac_reads | `covering_index_scan` | 6.86ms | 7.14ms | 1.042× | 1.37% | PASS |
| ac_reads | `groupby_scan` | 36.42ms | 39.30ms | 1.079× | 1.02% | PASS |
| ac_reads | `index_join` | 9.81ms | 12.87ms | 1.313× | 1.31% | PASS |
| ac_reads | `index_join_scan` | 4.75ms | 5.95ms | 1.253× | 2.40% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.24s | 1.168× | 1.00% | PASS |
| ac_reads | `table_scan` | 1.29s | 1.41s | 1.092× | 2.97% | PASS |
| ac_reads | `oltp_read_only` | 195.33ms | 214.48ms | 1.098× | 1.01% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.30ms | 85.92ms | 3.536× | 6.02% | PASS |
| ac_writes | `oltp_insert_ac` | 28.10ms | 109.51ms | 3.898× | 9.28% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.35ms | 126.95ms | 4.325× | 8.85% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.28ms | 105.10ms | 4.158× | 7.42% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.72ms | 112.56ms | 4.212× | 6.25% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.96ms | 109.32ms | 4.054× | 5.89% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.45ms | 97.95ms | 4.363× | 6.68% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.99ms | 118.97ms | 3.500× | 7.33% | PASS |

</details>

## Version-control latency

Wall time: 1m 46s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 55.00ms | 200.00ms | 27.5% | 1.10% | PASS |
| `status_dirty_many_tables` | 56.83ms | 200.00ms | 28.4% | 1.01% | PASS |
| `diff_regular_working_one_table` | 51.58ms | 150.00ms | 34.4% | 1.09% | PASS |
| `diff_regular_working_many_tables` | 61.91ms | 200.00ms | 31.0% | 0.62% | PASS |
| `diff_stat_working_many_tables` | 61.53ms | 200.00ms | 30.8% | 0.66% | PASS |
| `diff_schema_working_many_tables` | 61.48ms | 200.00ms | 30.7% | 0.63% | PASS |
| `branch_list_many_branches` | 17.09ms | 100.00ms | 17.1% | 1.16% | PASS |
| `branch_create_delete` | 26.27ms | 100.00ms | 26.3% | 1.80% | PASS |
| `checkout_branch_clean` | 88.52ms | 200.00ms | 44.3% | 4.70% | PASS |
| `merge_data_no_conflicts` | 32.45ms | 150.00ms | 21.6% | 3.56% | PASS |
| `merge_schema_no_conflicts` | 18.01ms | 100.00ms | 18.0% | 2.05% | PASS |
| `merge_data_conflicts` | 64.77ms | 250.00ms | 25.9% | 0.51% | PASS |
| `merge_data_conflicts_with_resolve` | 64.53ms | 250.00ms | 25.8% | 0.56% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
