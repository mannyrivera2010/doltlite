# DoltLite Performance Report

> Nightly result: **FAIL**
>
> Generated: 2026-11-03 18:56 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261101.360.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/41682237908)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 15m 50s | 9.36s | 11.64s | 1.243× | 1.71% | **PASS** |
| textpk | 69 | 55 | 1h 38m 17s | 11.04s | 12.24s | 1.110× | 1.94% | **PASS** |
| blobpk | 69 | 55 | 1h 11m 48s | 7.79s | 8.27s | 1.061× | 3.12% | **FAIL** |
| compositepk | 69 | 55 | 1h 32m 35s | 10.78s | 12.50s | 1.159× | 1.73% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.96ms | 30.65ms | 1.181× | 1.89% | PASS |
| mem_reads | `oltp_range_select` | 10.81ms | 13.49ms | 1.247× | 2.01% | PASS |
| mem_reads | `oltp_sum_range` | 10.04ms | 12.77ms | 1.271× | 2.72% | PASS |
| mem_reads | `oltp_order_range` | 2.59ms | 3.01ms | 1.161× | 1.78% | PASS |
| mem_reads | `oltp_distinct_range` | 3.65ms | 4.07ms | 1.113× | 1.13% | PASS |
| mem_reads | `oltp_index_scan` | 4.05ms | 5.79ms | 1.430× | 2.86% | PASS |
| mem_reads | `select_random_points` | 11.14ms | 11.56ms | 1.037× | 3.33% | PASS |
| mem_reads | `select_random_ranges` | 3.09ms | 4.09ms | 1.322× | 1.57% | PASS |
| mem_reads | `covering_index_scan` | 4.32ms | 4.66ms | 1.079× | 2.64% | PASS |
| mem_reads | `groupby_scan` | 30.22ms | 33.67ms | 1.114× | 0.89% | PASS |
| mem_reads | `index_join` | 6.14ms | 8.93ms | 1.454× | 2.85% | PASS |
| mem_reads | `index_join_scan` | 3.56ms | 4.72ms | 1.325× | 3.28% | PASS |
| mem_reads | `types_table_scan` | 1.11s | 1.36s | 1.219× | 0.81% | PASS |
| mem_reads | `table_scan` | 1.27s | 1.42s | 1.125× | 0.72% | PASS |
| mem_reads | `oltp_read_only` | 109.36ms | 126.34ms | 1.155× | 1.56% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.39ms | 264.39ms | 1.458× | 0.88% | PASS |
| mem_writes | `oltp_insert` | 15.90ms | 30.10ms | 1.892× | 1.24% | PASS |
| mem_writes | `oltp_update_index` | 53.98ms | 118.70ms | 2.199× | 1.77% | PASS |
| mem_writes | `oltp_update_non_index` | 36.75ms | 66.65ms | 1.814× | 2.05% | PASS |
| mem_writes | `oltp_delete_insert` | 47.84ms | 87.01ms | 1.819× | 1.93% | PASS |
| mem_writes | `oltp_write_only` | 23.10ms | 50.38ms | 2.181× | 1.98% | PASS |
| mem_writes | `types_delete_insert` | 25.91ms | 45.64ms | 1.762× | 1.96% | PASS |
| mem_writes | `oltp_read_write` | 73.24ms | 120.57ms | 1.646× | 1.71% | PASS |
| file_reads | `oltp_point_select` | 95.85ms | 53.98ms | 0.563× | 1.17% | PASS |
| file_reads | `oltp_range_select` | 18.59ms | 16.15ms | 0.869× | 1.18% | PASS |
| file_reads | `oltp_sum_range` | 17.64ms | 15.28ms | 0.866× | 1.44% | PASS |
| file_reads | `oltp_order_range` | 3.43ms | 3.31ms | 0.964× | 1.66% | PASS |
| file_reads | `oltp_distinct_range` | 4.56ms | 4.39ms | 0.962× | 1.62% | PASS |
| file_reads | `oltp_index_scan` | 11.48ms | 8.26ms | 0.719× | 1.59% | PASS |
| file_reads | `select_random_points` | 18.30ms | 14.11ms | 0.771× | 1.80% | PASS |
| file_reads | `select_random_ranges` | 10.20ms | 6.52ms | 0.640× | 0.98% | PASS |
| file_reads | `covering_index_scan` | 11.87ms | 7.08ms | 0.596× | 2.39% | PASS |
| file_reads | `groupby_scan` | 31.08ms | 34.01ms | 1.094× | 1.05% | PASS |
| file_reads | `index_join` | 10.33ms | 10.36ms | 1.003× | 1.40% | PASS |
| file_reads | `index_join_scan` | 4.68ms | 5.17ms | 1.102× | 3.65% | PASS |
| file_reads | `types_table_scan` | 1.11s | 1.35s | 1.225× | 0.54% | PASS |
| file_reads | `table_scan` | 1.28s | 1.43s | 1.119× | 0.69% | PASS |
| file_reads | `oltp_read_only` | 212.90ms | 162.19ms | 0.762× | 1.13% | PASS |
| file_writes | `oltp_bulk_insert` | 195.81ms | 282.65ms | 1.444× | 1.38% | PASS |
| file_writes | `oltp_insert` | 22.98ms | 36.48ms | 1.588× | 2.05% | PASS |
| file_writes | `oltp_update_index` | 82.17ms | 148.41ms | 1.806× | 1.96% | PASS |
| file_writes | `oltp_update_non_index` | 61.42ms | 86.95ms | 1.415× | 1.91% | PASS |
| file_writes | `oltp_delete_insert` | 73.49ms | 113.76ms | 1.548× | 2.26% | PASS |
| file_writes | `oltp_write_only` | 46.68ms | 69.82ms | 1.496× | 1.87% | PASS |
| file_writes | `types_delete_insert` | 42.17ms | 55.39ms | 1.314× | 1.71% | PASS |
| file_writes | `oltp_read_write` | 99.91ms | 140.76ms | 1.409× | 2.42% | PASS |
| ac_reads | `oltp_point_select` | 49.09ms | 54.25ms | 1.105× | 1.62% | PASS |
| ac_reads | `oltp_range_select` | 13.92ms | 16.23ms | 1.166× | 1.57% | PASS |
| ac_reads | `oltp_sum_range` | 13.13ms | 15.58ms | 1.186× | 1.68% | PASS |
| ac_reads | `oltp_order_range` | 3.02ms | 3.33ms | 1.105× | 1.50% | PASS |
| ac_reads | `oltp_distinct_range` | 4.05ms | 4.42ms | 1.090× | 1.43% | PASS |
| ac_reads | `oltp_index_scan` | 6.99ms | 8.42ms | 1.204× | 1.29% | PASS |
| ac_reads | `select_random_points` | 14.48ms | 14.69ms | 1.015× | 3.11% | PASS |
| ac_reads | `select_random_ranges` | 5.59ms | 6.55ms | 1.171× | 1.14% | PASS |
| ac_reads | `covering_index_scan` | 7.29ms | 7.17ms | 0.983× | 1.66% | PASS |
| ac_reads | `groupby_scan` | 30.60ms | 34.17ms | 1.117× | 0.91% | PASS |
| ac_reads | `index_join` | 8.08ms | 10.48ms | 1.297× | 2.43% | PASS |
| ac_reads | `index_join_scan` | 4.04ms | 5.02ms | 1.245× | 2.80% | PASS |
| ac_reads | `types_table_scan` | 1.08s | 1.34s | 1.245× | 1.26% | PASS |
| ac_reads | `table_scan` | 1.27s | 1.43s | 1.121× | 0.71% | PASS |
| ac_reads | `oltp_read_only` | 146.91ms | 164.71ms | 1.121× | 0.97% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.63ms | 61.50ms | 2.981× | 6.44% | PASS |
| ac_writes | `oltp_insert_ac` | 22.37ms | 78.52ms | 3.510× | 3.21% | PASS |
| ac_writes | `oltp_update_index_ac` | 24.89ms | 94.10ms | 3.781× | 3.86% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.11ms | 70.38ms | 3.501× | 3.16% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.50ms | 84.90ms | 3.773× | 5.54% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.98ms | 83.26ms | 3.624× | 5.65% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.09ms | 73.01ms | 3.461× | 7.44% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.30ms | 90.57ms | 3.318× | 3.78% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.92ms | 38.33ms | 1.239× | 2.04% | PASS |
| mem_reads | `oltp_range_select` | 14.56ms | 14.33ms | 0.984× | 3.16% | PASS |
| mem_reads | `oltp_sum_range` | 12.70ms | 14.48ms | 1.140× | 1.47% | PASS |
| mem_reads | `oltp_order_range` | 3.14ms | 3.23ms | 1.027× | 1.48% | PASS |
| mem_reads | `oltp_distinct_range` | 4.16ms | 4.31ms | 1.037× | 1.20% | PASS |
| mem_reads | `oltp_index_scan` | 4.80ms | 6.64ms | 1.383× | 2.49% | PASS |
| mem_reads | `select_random_points` | 19.08ms | 21.59ms | 1.132× | 2.23% | PASS |
| mem_reads | `select_random_ranges` | 4.28ms | 5.34ms | 1.246× | 1.92% | PASS |
| mem_reads | `covering_index_scan` | 5.03ms | 4.83ms | 0.960× | 4.44% | PASS |
| mem_reads | `groupby_scan` | 32.26ms | 34.53ms | 1.070× | 1.25% | PASS |
| mem_reads | `index_join` | 7.05ms | 9.62ms | 1.365× | 3.35% | PASS |
| mem_reads | `index_join_scan` | 4.80ms | 5.68ms | 1.182× | 2.45% | PASS |
| mem_reads | `types_table_scan` | 1.21s | 1.28s | 1.057× | 2.46% | PASS |
| mem_reads | `table_scan` | 1.54s | 1.43s | 0.924× | 1.61% | PASS |
| mem_reads | `oltp_read_only` | 133.31ms | 143.19ms | 1.074× | 1.22% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.99ms | 374.79ms | 1.581× | 0.84% | PASS |
| mem_writes | `oltp_insert` | 23.29ms | 43.47ms | 1.867× | 2.14% | PASS |
| mem_writes | `oltp_update_index` | 78.63ms | 151.79ms | 1.930× | 1.94% | PASS |
| mem_writes | `oltp_update_non_index` | 51.03ms | 95.78ms | 1.877× | 1.78% | PASS |
| mem_writes | `oltp_delete_insert` | 54.45ms | 115.02ms | 2.112× | 2.08% | PASS |
| mem_writes | `oltp_write_only` | 30.94ms | 69.88ms | 2.258× | 2.04% | PASS |
| mem_writes | `types_delete_insert` | 35.35ms | 63.26ms | 1.790× | 1.72% | PASS |
| mem_writes | `oltp_read_write` | 96.31ms | 153.78ms | 1.597× | 2.04% | PASS |
| file_reads | `oltp_point_select` | 104.89ms | 64.06ms | 0.611× | 1.14% | PASS |
| file_reads | `oltp_range_select` | 22.97ms | 17.12ms | 0.745× | 2.47% | PASS |
| file_reads | `oltp_sum_range` | 20.77ms | 17.19ms | 0.828× | 1.83% | PASS |
| file_reads | `oltp_order_range` | 4.12ms | 3.61ms | 0.876× | 1.53% | PASS |
| file_reads | `oltp_distinct_range` | 5.22ms | 4.71ms | 0.904× | 1.97% | PASS |
| file_reads | `oltp_index_scan` | 12.35ms | 9.18ms | 0.743× | 1.37% | PASS |
| file_reads | `select_random_points` | 26.95ms | 24.54ms | 0.911× | 1.74% | PASS |
| file_reads | `select_random_ranges` | 11.49ms | 7.77ms | 0.676× | 1.17% | PASS |
| file_reads | `covering_index_scan` | 13.13ms | 7.33ms | 0.559× | 1.42% | PASS |
| file_reads | `groupby_scan` | 33.59ms | 35.11ms | 1.045× | 1.18% | PASS |
| file_reads | `index_join` | 12.20ms | 11.60ms | 0.950× | 2.15% | PASS |
| file_reads | `index_join_scan` | 5.95ms | 6.22ms | 1.045× | 3.79% | PASS |
| file_reads | `types_table_scan` | 1.11s | 1.26s | 1.127× | 3.34% | PASS |
| file_reads | `table_scan` | 1.42s | 1.40s | 0.987× | 4.36% | PASS |
| file_reads | `oltp_read_only` | 232.54ms | 176.46ms | 0.759× | 1.00% | PASS |
| file_writes | `oltp_bulk_insert` | 257.54ms | 406.02ms | 1.577× | 1.14% | PASS |
| file_writes | `oltp_insert` | 60.13ms | 57.73ms | 0.960× | 20.06% | PASS |
| file_writes | `oltp_update_index` | 120.61ms | 188.03ms | 1.559× | 1.40% | PASS |
| file_writes | `oltp_update_non_index` | 115.98ms | 126.84ms | 1.094× | 6.59% | PASS |
| file_writes | `oltp_delete_insert` | 96.50ms | 148.56ms | 1.539× | 1.68% | PASS |
| file_writes | `oltp_write_only` | 79.33ms | 95.95ms | 1.209× | 8.81% | PASS |
| file_writes | `types_delete_insert` | 59.74ms | 83.02ms | 1.390× | 1.78% | PASS |
| file_writes | `oltp_read_write` | 160.38ms | 184.76ms | 1.152× | 5.39% | PASS |
| ac_reads | `oltp_point_select` | 57.75ms | 65.40ms | 1.133× | 1.49% | PASS |
| ac_reads | `oltp_range_select` | 18.95ms | 17.82ms | 0.941× | 1.77% | PASS |
| ac_reads | `oltp_sum_range` | 16.97ms | 17.82ms | 1.050× | 1.86% | PASS |
| ac_reads | `oltp_order_range` | 3.68ms | 3.63ms | 0.987× | 2.26% | PASS |
| ac_reads | `oltp_distinct_range` | 4.72ms | 4.76ms | 1.007× | 1.81% | PASS |
| ac_reads | `oltp_index_scan` | 7.87ms | 9.41ms | 1.196× | 1.83% | PASS |
| ac_reads | `select_random_points` | 23.73ms | 26.37ms | 1.111× | 1.91% | PASS |
| ac_reads | `select_random_ranges` | 7.02ms | 7.93ms | 1.129× | 1.13% | PASS |
| ac_reads | `covering_index_scan` | 8.78ms | 7.45ms | 0.848× | 2.04% | PASS |
| ac_reads | `groupby_scan` | 33.68ms | 35.58ms | 1.057× | 1.14% | PASS |
| ac_reads | `index_join` | 9.98ms | 12.06ms | 1.208× | 3.47% | PASS |
| ac_reads | `index_join_scan` | 5.48ms | 6.29ms | 1.147× | 3.15% | PASS |
| ac_reads | `types_table_scan` | 1.26s | 1.29s | 1.023× | 1.40% | PASS |
| ac_reads | `table_scan` | 1.58s | 1.44s | 0.915× | 0.90% | PASS |
| ac_reads | `oltp_read_only` | 166.20ms | 179.00ms | 1.077× | 1.77% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.96ms | 66.42ms | 3.025× | 7.71% | PASS |
| ac_writes | `oltp_insert_ac` | 25.71ms | 85.76ms | 3.336× | 7.27% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.90ms | 101.42ms | 3.635× | 7.37% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.89ms | 81.36ms | 3.717× | 8.10% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.43ms | 91.70ms | 3.754× | 6.20% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.91ms | 91.80ms | 3.685× | 5.77% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.43ms | 84.47ms | 3.765× | 8.49% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.06ms | 98.39ms | 3.168× | 4.09% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.49ms | 23.16ms | 1.130× | 2.68% | PASS |
| mem_reads | `oltp_range_select` | 8.91ms | 8.78ms | 0.985× | 2.68% | PASS |
| mem_reads | `oltp_sum_range` | 8.54ms | 8.65ms | 1.013× | 3.12% | PASS |
| mem_reads | `oltp_order_range` | 2.04ms | 2.01ms | 0.982× | 2.53% | PASS |
| mem_reads | `oltp_distinct_range` | 2.64ms | 2.52ms | 0.953× | 1.96% | PASS |
| mem_reads | `oltp_index_scan` | 3.04ms | 4.25ms | 1.398× | 4.11% | PASS |
| mem_reads | `select_random_points` | 12.59ms | 13.89ms | 1.103× | 3.45% | PASS |
| mem_reads | `select_random_ranges` | 3.13ms | 3.63ms | 1.160× | 2.00% | PASS |
| mem_reads | `covering_index_scan` | 2.58ms | 3.26ms | 1.260× | 5.21% | PASS |
| mem_reads | `groupby_scan` | 20.15ms | 19.86ms | 0.986× | 1.75% | PASS |
| mem_reads | `index_join` | 4.58ms | 6.64ms | 1.448× | 7.41% | PASS |
| mem_reads | `index_join_scan` | 3.74ms | 5.04ms | 1.346× | 4.15% | PASS |
| mem_reads | `types_table_scan` | 758.32ms | 771.42ms | 1.017× | 3.79% | PASS |
| mem_reads | `table_scan` | 950.53ms | 897.19ms | 0.944× | 6.16% | PASS |
| mem_reads | `oltp_read_only` | 85.83ms | 82.62ms | 0.963× | 2.32% | PASS |
| mem_writes | `oltp_bulk_insert` | 145.74ms | 223.10ms | 1.531× | 2.10% | PASS |
| mem_writes | `oltp_insert` | 12.67ms | 28.23ms | 2.228× | 2.52% | PASS |
| mem_writes | `oltp_update_index` | 46.72ms | 100.02ms | 2.141× | 3.18% | PASS |
| mem_writes | `oltp_update_non_index` | 35.60ms | 67.18ms | 1.887× | 2.46% | PASS |
| mem_writes | `oltp_delete_insert` | 34.37ms | 79.92ms | 2.326× | 3.41% | PASS |
| mem_writes | `oltp_write_only` | 20.01ms | 50.05ms | 2.502× | 2.85% | FAIL |
| mem_writes | `types_delete_insert` | 22.97ms | 43.08ms | 1.875× | 2.49% | PASS |
| mem_writes | `oltp_read_write` | 57.84ms | 94.80ms | 1.639× | 3.62% | PASS |
| file_reads | `oltp_point_select` | 75.96ms | 42.13ms | 0.555× | 1.38% | PASS |
| file_reads | `oltp_range_select` | 15.82ms | 11.10ms | 0.702× | 2.18% | PASS |
| file_reads | `oltp_sum_range` | 15.31ms | 10.85ms | 0.709× | 1.96% | PASS |
| file_reads | `oltp_order_range` | 2.80ms | 2.30ms | 0.821× | 1.54% | PASS |
| file_reads | `oltp_distinct_range` | 3.38ms | 2.80ms | 0.826× | 0.85% | PASS |
| file_reads | `oltp_index_scan` | 9.40ms | 6.44ms | 0.686× | 2.39% | PASS |
| file_reads | `select_random_points` | 20.39ms | 16.59ms | 0.814× | 3.61% | PASS |
| file_reads | `select_random_ranges` | 8.76ms | 5.55ms | 0.633× | 1.26% | PASS |
| file_reads | `covering_index_scan` | 9.49ms | 5.29ms | 0.558× | 2.24% | PASS |
| file_reads | `groupby_scan` | 22.12ms | 20.64ms | 0.933× | 1.47% | PASS |
| file_reads | `index_join` | 8.87ms | 8.34ms | 0.940× | 3.23% | PASS |
| file_reads | `index_join_scan` | 4.34ms | 4.79ms | 1.105× | 6.63% | PASS |
| file_reads | `types_table_scan` | 829.06ms | 808.82ms | 0.976× | 3.42% | PASS |
| file_reads | `table_scan` | 1.14s | 980.98ms | 0.863× | 1.34% | PASS |
| file_reads | `oltp_read_only` | 172.45ms | 116.25ms | 0.674× | 2.14% | PASS |
| file_writes | `oltp_bulk_insert` | 198.66ms | 297.34ms | 1.497× | 2.85% | PASS |
| file_writes | `oltp_insert` | 35.28ms | 57.23ms | 1.622× | 5.23% | PASS |
| file_writes | `oltp_update_index` | 170.65ms | 198.13ms | 1.161× | 5.16% | PASS |
| file_writes | `oltp_update_non_index` | 135.68ms | 136.91ms | 1.009× | 3.36% | PASS |
| file_writes | `oltp_delete_insert` | 140.28ms | 153.74ms | 1.096× | 3.98% | PASS |
| file_writes | `oltp_write_only` | 100.11ms | 107.87ms | 1.078× | 2.60% | PASS |
| file_writes | `types_delete_insert` | 89.54ms | 81.98ms | 0.916× | 5.07% | PASS |
| file_writes | `oltp_read_write` | 131.37ms | 148.37ms | 1.129× | 4.29% | PASS |
| ac_reads | `oltp_point_select` | 39.06ms | 41.48ms | 1.062× | 2.60% | PASS |
| ac_reads | `oltp_range_select` | 11.57ms | 10.75ms | 0.929× | 2.73% | PASS |
| ac_reads | `oltp_sum_range` | 12.17ms | 11.05ms | 0.908× | 4.78% | PASS |
| ac_reads | `oltp_order_range` | 2.55ms | 2.37ms | 0.927× | 2.60% | PASS |
| ac_reads | `oltp_distinct_range` | 3.08ms | 2.81ms | 0.913× | 2.23% | PASS |
| ac_reads | `oltp_index_scan` | 5.68ms | 6.39ms | 1.125× | 2.19% | PASS |
| ac_reads | `select_random_points` | 16.22ms | 15.90ms | 0.980× | 4.34% | PASS |
| ac_reads | `select_random_ranges` | 5.17ms | 5.50ms | 1.063× | 1.65% | PASS |
| ac_reads | `covering_index_scan` | 5.81ms | 5.22ms | 0.899× | 3.16% | PASS |
| ac_reads | `groupby_scan` | 21.13ms | 20.10ms | 0.951× | 1.43% | PASS |
| ac_reads | `index_join` | 6.62ms | 8.01ms | 1.211× | 2.75% | PASS |
| ac_reads | `index_join_scan` | 3.80ms | 4.60ms | 1.212× | 4.13% | PASS |
| ac_reads | `types_table_scan` | 811.64ms | 803.07ms | 0.989× | 4.49% | PASS |
| ac_reads | `table_scan` | 938.70ms | 893.49ms | 0.952× | 5.25% | PASS |
| ac_reads | `oltp_read_only` | 107.75ms | 105.23ms | 0.977× | 3.09% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.36ms | 55.78ms | 2.495× | 5.79% | PASS |
| ac_writes | `oltp_insert_ac` | 26.07ms | 71.31ms | 2.735× | 6.95% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.99ms | 79.94ms | 2.856× | 5.86% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.71ms | 64.28ms | 2.961× | 4.86% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.57ms | 74.16ms | 2.900× | 3.96% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.77ms | 75.66ms | 2.936× | 7.90% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.52ms | 67.67ms | 3.144× | 7.66% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.76ms | 79.23ms | 2.755× | 5.67% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 35.11ms | 41.71ms | 1.188× | 2.42% | PASS |
| mem_reads | `oltp_range_select` | 20.82ms | 22.06ms | 1.060× | 2.13% | PASS |
| mem_reads | `oltp_sum_range` | 19.59ms | 21.89ms | 1.117× | 2.11% | PASS |
| mem_reads | `oltp_order_range` | 3.83ms | 4.24ms | 1.107× | 2.48% | PASS |
| mem_reads | `oltp_distinct_range` | 4.95ms | 5.32ms | 1.075× | 2.34% | PASS |
| mem_reads | `oltp_index_scan` | 4.78ms | 6.47ms | 1.354× | 2.86% | PASS |
| mem_reads | `select_random_points` | 30.05ms | 33.86ms | 1.127× | 1.77% | PASS |
| mem_reads | `select_random_ranges` | 8.17ms | 9.38ms | 1.148× | 1.93% | PASS |
| mem_reads | `covering_index_scan` | 4.42ms | 4.67ms | 1.057× | 3.02% | PASS |
| mem_reads | `groupby_scan` | 37.45ms | 39.99ms | 1.068× | 1.02% | PASS |
| mem_reads | `index_join` | 8.57ms | 11.50ms | 1.343× | 2.89% | PASS |
| mem_reads | `index_join_scan` | 4.41ms | 5.80ms | 1.314× | 3.04% | PASS |
| mem_reads | `types_table_scan` | 1.10s | 1.26s | 1.142× | 1.33% | PASS |
| mem_reads | `table_scan` | 1.37s | 1.43s | 1.044× | 1.56% | PASS |
| mem_reads | `oltp_read_only` | 160.37ms | 173.83ms | 1.084× | 1.29% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.83ms | 372.79ms | 1.492× | 0.88% | PASS |
| mem_writes | `oltp_insert` | 19.54ms | 39.65ms | 2.029× | 1.12% | PASS |
| mem_writes | `oltp_update_index` | 71.84ms | 131.17ms | 1.826× | 1.04% | PASS |
| mem_writes | `oltp_update_non_index` | 54.22ms | 93.56ms | 1.726× | 1.69% | PASS |
| mem_writes | `oltp_delete_insert` | 53.28ms | 107.75ms | 2.022× | 1.36% | PASS |
| mem_writes | `oltp_write_only` | 28.86ms | 65.85ms | 2.282× | 1.64% | PASS |
| mem_writes | `types_delete_insert` | 34.15ms | 59.72ms | 1.749× | 2.01% | PASS |
| mem_writes | `oltp_read_write` | 111.15ms | 169.02ms | 1.521× | 1.31% | PASS |
| file_reads | `oltp_point_select` | 106.86ms | 66.77ms | 0.625× | 1.02% | PASS |
| file_reads | `oltp_range_select` | 29.92ms | 25.04ms | 0.837× | 1.73% | PASS |
| file_reads | `oltp_sum_range` | 27.86ms | 24.84ms | 0.892× | 1.38% | PASS |
| file_reads | `oltp_order_range` | 5.16ms | 4.67ms | 0.906× | 2.93% | PASS |
| file_reads | `oltp_distinct_range` | 6.17ms | 5.78ms | 0.937× | 3.04% | PASS |
| file_reads | `oltp_index_scan` | 12.49ms | 9.35ms | 0.749× | 1.90% | PASS |
| file_reads | `select_random_points` | 41.60ms | 39.20ms | 0.942× | 1.39% | PASS |
| file_reads | `select_random_ranges` | 16.06ms | 12.26ms | 0.763× | 1.13% | PASS |
| file_reads | `covering_index_scan` | 12.58ms | 7.41ms | 0.588× | 1.43% | PASS |
| file_reads | `groupby_scan` | 39.24ms | 41.03ms | 1.046× | 0.96% | PASS |
| file_reads | `index_join` | 13.61ms | 14.04ms | 1.032× | 2.01% | PASS |
| file_reads | `index_join_scan` | 5.68ms | 6.54ms | 1.152× | 3.41% | PASS |
| file_reads | `types_table_scan` | 1.18s | 1.28s | 1.084× | 0.94% | PASS |
| file_reads | `table_scan` | 1.45s | 1.45s | 1.002× | 1.23% | PASS |
| file_reads | `oltp_read_only` | 271.74ms | 214.37ms | 0.789× | 1.02% | PASS |
| file_writes | `oltp_bulk_insert` | 266.50ms | 402.13ms | 1.509× | 1.04% | PASS |
| file_writes | `oltp_insert` | 27.59ms | 50.85ms | 1.843× | 1.36% | PASS |
| file_writes | `oltp_update_index` | 106.22ms | 164.74ms | 1.551× | 1.10% | PASS |
| file_writes | `oltp_update_non_index` | 84.11ms | 119.02ms | 1.415× | 2.05% | PASS |
| file_writes | `oltp_delete_insert` | 83.73ms | 134.52ms | 1.607× | 1.67% | PASS |
| file_writes | `oltp_write_only` | 54.19ms | 87.23ms | 1.610× | 1.49% | PASS |
| file_writes | `types_delete_insert` | 53.46ms | 76.10ms | 1.423× | 1.45% | PASS |
| file_writes | `oltp_read_write` | 139.84ms | 190.33ms | 1.361× | 1.61% | PASS |
| ac_reads | `oltp_point_select` | 59.22ms | 66.37ms | 1.121× | 1.39% | PASS |
| ac_reads | `oltp_range_select` | 23.87ms | 24.58ms | 1.030× | 1.54% | PASS |
| ac_reads | `oltp_sum_range` | 22.60ms | 24.56ms | 1.087× | 2.06% | PASS |
| ac_reads | `oltp_order_range` | 4.50ms | 4.52ms | 1.005× | 2.90% | PASS |
| ac_reads | `oltp_distinct_range` | 5.63ms | 5.76ms | 1.023× | 2.79% | PASS |
| ac_reads | `oltp_index_scan` | 7.91ms | 9.38ms | 1.186× | 1.47% | PASS |
| ac_reads | `select_random_points` | 36.72ms | 40.12ms | 1.093× | 2.23% | PASS |
| ac_reads | `select_random_ranges` | 11.28ms | 12.46ms | 1.105× | 2.23% | PASS |
| ac_reads | `covering_index_scan` | 8.04ms | 7.47ms | 0.929× | 2.55% | PASS |
| ac_reads | `groupby_scan` | 38.60ms | 41.06ms | 1.064× | 0.93% | PASS |
| ac_reads | `index_join` | 10.87ms | 13.95ms | 1.284× | 2.22% | PASS |
| ac_reads | `index_join_scan` | 5.30ms | 6.89ms | 1.298× | 4.15% | PASS |
| ac_reads | `types_table_scan` | 1.18s | 1.29s | 1.088× | 0.78% | PASS |
| ac_reads | `table_scan` | 1.48s | 1.47s | 0.995× | 0.73% | PASS |
| ac_reads | `oltp_read_only` | 212.58ms | 222.57ms | 1.047× | 1.16% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.07ms | 70.68ms | 3.063× | 6.57% | PASS |
| ac_writes | `oltp_insert_ac` | 25.52ms | 92.79ms | 3.637× | 7.49% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.97ms | 104.08ms | 3.721× | 4.64% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.24ms | 88.62ms | 3.511× | 7.04% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.91ms | 93.26ms | 3.599× | 5.76% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.50ms | 93.55ms | 3.668× | 7.86% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.05ms | 85.28ms | 3.867× | 7.54% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.92ms | 100.98ms | 3.068× | 4.79% | PASS |

</details>

## Version-control latency

Wall time: 1m 43s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 59.69ms | 200.00ms | 29.8% | 0.90% | PASS |
| `status_dirty_many_tables` | 61.80ms | 200.00ms | 30.9% | 0.90% | PASS |
| `diff_regular_working_one_table` | 56.13ms | 150.00ms | 37.4% | 1.57% | PASS |
| `diff_regular_working_many_tables` | 59.47ms | 200.00ms | 29.7% | 0.76% | PASS |
| `diff_stat_working_many_tables` | 59.95ms | 200.00ms | 30.0% | 0.87% | PASS |
| `diff_schema_working_many_tables` | 61.23ms | 200.00ms | 30.6% | 1.41% | PASS |
| `branch_list_many_branches` | 16.38ms | 100.00ms | 16.4% | 0.95% | PASS |
| `branch_create_delete` | 18.04ms | 100.00ms | 18.0% | 0.96% | PASS |
| `checkout_branch_clean` | 74.76ms | 200.00ms | 37.4% | 3.75% | PASS |
| `merge_data_no_conflicts` | 27.57ms | 150.00ms | 18.4% | 11.96% | PASS |
| `merge_schema_no_conflicts` | 15.73ms | 100.00ms | 15.7% | 1.89% | PASS |
| `merge_data_conflicts` | 75.92ms | 250.00ms | 30.4% | 1.33% | PASS |
| `merge_data_conflicts_with_resolve` | 75.75ms | 250.00ms | 30.3% | 0.90% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
