# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-04 15:22 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260831.293.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/33879525184)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 2m 44s | 7.70s | 9.59s | 1.245× | 4.01% | **PASS** |
| textpk | 69 | 55 | 1h 36m 43s | 11.11s | 12.30s | 1.107× | 2.60% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 8s | 10.17s | 11.64s | 1.144× | 1.50% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 28s | 9.76s | 12.12s | 1.242× | 1.59% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 18.42ms | 20.71ms | 1.125× | 4.52% | PASS |
| mem_reads | `oltp_range_select` | 8.16ms | 9.86ms | 1.207× | 5.49% | PASS |
| mem_reads | `oltp_sum_range` | 7.50ms | 9.33ms | 1.244× | 4.28% | PASS |
| mem_reads | `oltp_order_range` | 2.16ms | 2.43ms | 1.128× | 3.94% | PASS |
| mem_reads | `oltp_distinct_range` | 3.00ms | 3.23ms | 1.076× | 3.76% | PASS |
| mem_reads | `oltp_index_scan` | 2.99ms | 3.69ms | 1.233× | 6.42% | PASS |
| mem_reads | `select_random_points` | 8.51ms | 9.00ms | 1.059× | 4.39% | PASS |
| mem_reads | `select_random_ranges` | 2.27ms | 2.92ms | 1.288× | 4.74% | PASS |
| mem_reads | `covering_index_scan` | 3.02ms | 3.09ms | 1.021× | 4.52% | PASS |
| mem_reads | `groupby_scan` | 25.33ms | 27.29ms | 1.077× | 3.06% | PASS |
| mem_reads | `index_join` | 4.57ms | 6.17ms | 1.350× | 2.96% | PASS |
| mem_reads | `index_join_scan` | 2.39ms | 3.33ms | 1.394× | 5.70% | PASS |
| mem_reads | `types_table_scan` | 884.80ms | 1.06s | 1.201× | 0.82% | PASS |
| mem_reads | `table_scan` | 998.06ms | 1.16s | 1.157× | 0.86% | PASS |
| mem_reads | `oltp_read_only` | 80.98ms | 94.30ms | 1.164× | 3.01% | PASS |
| mem_writes | `oltp_bulk_insert` | 125.59ms | 164.91ms | 1.313× | 1.99% | PASS |
| mem_writes | `oltp_insert` | 11.10ms | 19.18ms | 1.729× | 4.22% | PASS |
| mem_writes | `oltp_update_index` | 36.90ms | 75.01ms | 2.033× | 2.76% | PASS |
| mem_writes | `oltp_update_non_index` | 24.53ms | 41.51ms | 1.692× | 3.21% | PASS |
| mem_writes | `oltp_delete_insert` | 34.08ms | 55.23ms | 1.621× | 3.31% | PASS |
| mem_writes | `oltp_write_only` | 16.13ms | 32.02ms | 1.985× | 4.05% | PASS |
| mem_writes | `types_delete_insert` | 18.28ms | 28.79ms | 1.575× | 4.18% | PASS |
| mem_writes | `oltp_read_write` | 49.29ms | 79.26ms | 1.608× | 3.58% | PASS |
| file_reads | `oltp_point_select` | 45.23ms | 29.56ms | 0.654× | 3.19% | PASS |
| file_reads | `oltp_range_select` | 10.98ms | 11.03ms | 1.005× | 5.72% | PASS |
| file_reads | `oltp_sum_range` | 10.69ms | 10.49ms | 0.981× | 5.12% | PASS |
| file_reads | `oltp_order_range` | 2.55ms | 2.57ms | 1.010× | 4.48% | PASS |
| file_reads | `oltp_distinct_range` | 3.34ms | 3.41ms | 1.021× | 3.40% | PASS |
| file_reads | `oltp_index_scan` | 5.83ms | 4.77ms | 0.818× | 3.95% | PASS |
| file_reads | `select_random_points` | 11.69ms | 10.49ms | 0.897× | 4.43% | PASS |
| file_reads | `select_random_ranges` | 5.00ms | 3.84ms | 0.768× | 3.55% | PASS |
| file_reads | `covering_index_scan` | 5.97ms | 3.96ms | 0.663× | 4.26% | PASS |
| file_reads | `groupby_scan` | 26.08ms | 27.45ms | 1.053× | 3.25% | PASS |
| file_reads | `index_join` | 6.17ms | 6.85ms | 1.109× | 3.14% | PASS |
| file_reads | `index_join_scan` | 2.79ms | 3.75ms | 1.345× | 4.75% | PASS |
| file_reads | `types_table_scan` | 885.75ms | 1.07s | 1.208× | 0.84% | PASS |
| file_reads | `table_scan` | 1.01s | 1.17s | 1.159× | 0.80% | PASS |
| file_reads | `oltp_read_only` | 120.03ms | 108.18ms | 0.901× | 2.13% | PASS |
| file_writes | `oltp_bulk_insert` | 173.67ms | 221.94ms | 1.278× | 4.01% | PASS |
| file_writes | `oltp_insert` | 23.65ms | 35.40ms | 1.497× | 6.64% | PASS |
| file_writes | `oltp_update_index` | 130.07ms | 137.60ms | 1.058× | 2.12% | PASS |
| file_writes | `oltp_update_non_index` | 103.94ms | 91.41ms | 0.879× | 1.58% | PASS |
| file_writes | `oltp_delete_insert` | 112.42ms | 109.46ms | 0.974× | 2.44% | PASS |
| file_writes | `oltp_write_only` | 83.75ms | 76.33ms | 0.911× | 3.47% | PASS |
| file_writes | `types_delete_insert` | 72.06ms | 62.68ms | 0.870× | 8.53% | PASS |
| file_writes | `oltp_read_write` | 116.49ms | 124.13ms | 1.066× | 4.67% | PASS |
| ac_reads | `oltp_point_select` | 26.81ms | 29.18ms | 1.088× | 3.90% | PASS |
| ac_reads | `oltp_range_select` | 8.96ms | 11.04ms | 1.233× | 6.57% | PASS |
| ac_reads | `oltp_sum_range` | 8.49ms | 10.49ms | 1.235× | 5.32% | PASS |
| ac_reads | `oltp_order_range` | 2.28ms | 2.57ms | 1.127× | 5.89% | PASS |
| ac_reads | `oltp_distinct_range` | 3.17ms | 3.36ms | 1.061× | 3.85% | PASS |
| ac_reads | `oltp_index_scan` | 3.94ms | 4.63ms | 1.175× | 4.48% | PASS |
| ac_reads | `select_random_points` | 9.44ms | 9.96ms | 1.055× | 4.98% | PASS |
| ac_reads | `select_random_ranges` | 3.07ms | 3.73ms | 1.214× | 4.01% | PASS |
| ac_reads | `covering_index_scan` | 3.95ms | 3.96ms | 1.003× | 4.72% | PASS |
| ac_reads | `groupby_scan` | 25.02ms | 27.28ms | 1.091× | 3.48% | PASS |
| ac_reads | `index_join` | 5.27ms | 6.87ms | 1.303× | 3.56% | PASS |
| ac_reads | `index_join_scan` | 2.67ms | 3.72ms | 1.392× | 3.86% | PASS |
| ac_reads | `types_table_scan` | 880.75ms | 1.06s | 1.207× | 0.98% | PASS |
| ac_reads | `table_scan` | 998.87ms | 1.16s | 1.161× | 1.03% | PASS |
| ac_reads | `oltp_read_only` | 93.58ms | 106.64ms | 1.140× | 1.50% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 35.62ms | 100.28ms | 2.815× | 20.10% | PASS |
| ac_writes | `oltp_insert_ac` | 42.34ms | 118.62ms | 2.801× | 23.94% | PASS |
| ac_writes | `oltp_update_index_ac` | 35.29ms | 118.36ms | 3.354× | 15.64% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 36.18ms | 109.17ms | 3.017× | 24.34% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 36.42ms | 110.13ms | 3.024× | 20.40% | PASS |
| ac_writes | `oltp_write_only_ac` | 35.95ms | 130.86ms | 3.640× | 34.35% | PASS |
| ac_writes | `types_delete_insert_ac` | 32.38ms | 105.85ms | 3.269× | 21.19% | PASS |
| ac_writes | `oltp_read_write_ac` | 40.64ms | 122.67ms | 3.018× | 28.67% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.92ms | 37.55ms | 1.255× | 1.17% | PASS |
| mem_reads | `oltp_range_select` | 14.39ms | 14.54ms | 1.010× | 3.04% | PASS |
| mem_reads | `oltp_sum_range` | 12.90ms | 14.45ms | 1.120× | 3.23% | PASS |
| mem_reads | `oltp_order_range` | 3.26ms | 3.31ms | 1.017× | 2.03% | PASS |
| mem_reads | `oltp_distinct_range` | 4.29ms | 4.36ms | 1.016× | 1.36% | PASS |
| mem_reads | `oltp_index_scan` | 5.10ms | 6.74ms | 1.320× | 2.60% | PASS |
| mem_reads | `select_random_points` | 20.12ms | 22.63ms | 1.125× | 1.75% | PASS |
| mem_reads | `select_random_ranges` | 4.34ms | 5.31ms | 1.224× | 2.16% | PASS |
| mem_reads | `covering_index_scan` | 5.67ms | 4.92ms | 0.868× | 4.21% | PASS |
| mem_reads | `groupby_scan` | 33.35ms | 34.75ms | 1.042× | 1.25% | PASS |
| mem_reads | `index_join` | 8.37ms | 10.47ms | 1.250× | 3.93% | PASS |
| mem_reads | `index_join_scan` | 5.16ms | 5.97ms | 1.158× | 3.73% | PASS |
| mem_reads | `types_table_scan` | 1.29s | 1.30s | 1.001× | 1.68% | PASS |
| mem_reads | `table_scan` | 1.46s | 1.41s | 0.966× | 4.59% | PASS |
| mem_reads | `oltp_read_only` | 131.34ms | 141.69ms | 1.079× | 2.30% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.83ms | 363.08ms | 1.540× | 0.56% | PASS |
| mem_writes | `oltp_insert` | 23.81ms | 42.03ms | 1.766× | 2.86% | PASS |
| mem_writes | `oltp_update_index` | 83.37ms | 146.47ms | 1.757× | 2.66% | PASS |
| mem_writes | `oltp_update_non_index` | 52.66ms | 92.65ms | 1.759× | 1.52% | PASS |
| mem_writes | `oltp_delete_insert` | 58.12ms | 111.83ms | 1.924× | 2.75% | PASS |
| mem_writes | `oltp_write_only` | 31.38ms | 64.85ms | 2.066× | 3.09% | PASS |
| mem_writes | `types_delete_insert` | 34.80ms | 58.39ms | 1.678× | 1.36% | PASS |
| mem_writes | `oltp_read_write` | 98.43ms | 150.73ms | 1.531× | 2.68% | PASS |
| file_reads | `oltp_point_select` | 108.40ms | 65.27ms | 0.602× | 1.12% | PASS |
| file_reads | `oltp_range_select` | 23.64ms | 18.00ms | 0.761× | 1.96% | PASS |
| file_reads | `oltp_sum_range` | 21.43ms | 17.86ms | 0.833× | 2.43% | PASS |
| file_reads | `oltp_order_range` | 4.44ms | 3.93ms | 0.885× | 3.69% | PASS |
| file_reads | `oltp_distinct_range` | 5.58ms | 5.04ms | 0.903× | 3.03% | PASS |
| file_reads | `oltp_index_scan` | 13.05ms | 9.43ms | 0.723× | 1.43% | PASS |
| file_reads | `select_random_points` | 30.37ms | 26.66ms | 0.878× | 1.56% | PASS |
| file_reads | `select_random_ranges` | 12.21ms | 8.04ms | 0.658× | 1.28% | PASS |
| file_reads | `covering_index_scan` | 14.43ms | 7.68ms | 0.532× | 1.55% | PASS |
| file_reads | `groupby_scan` | 35.00ms | 35.73ms | 1.021× | 1.07% | PASS |
| file_reads | `index_join` | 12.46ms | 11.82ms | 0.948× | 2.31% | PASS |
| file_reads | `index_join_scan` | 5.98ms | 6.02ms | 1.006× | 3.53% | PASS |
| file_reads | `types_table_scan` | 1.22s | 1.27s | 1.041× | 3.38% | PASS |
| file_reads | `table_scan` | 1.35s | 1.39s | 1.032× | 5.37% | PASS |
| file_reads | `oltp_read_only` | 243.04ms | 180.08ms | 0.741× | 1.55% | PASS |
| file_writes | `oltp_bulk_insert` | 256.78ms | 388.30ms | 1.512× | 1.05% | PASS |
| file_writes | `oltp_insert` | 57.62ms | 54.15ms | 0.940× | 22.80% | PASS |
| file_writes | `oltp_update_index` | 117.08ms | 174.04ms | 1.487× | 2.69% | PASS |
| file_writes | `oltp_update_non_index` | 100.89ms | 114.98ms | 1.140× | 12.06% | PASS |
| file_writes | `oltp_delete_insert` | 98.21ms | 141.03ms | 1.436× | 1.67% | PASS |
| file_writes | `oltp_write_only` | 86.13ms | 88.87ms | 1.032× | 10.73% | PASS |
| file_writes | `types_delete_insert` | 57.47ms | 77.26ms | 1.344× | 1.72% | PASS |
| file_writes | `oltp_read_write` | 144.06ms | 169.67ms | 1.178× | 6.29% | PASS |
| ac_reads | `oltp_point_select` | 57.87ms | 65.30ms | 1.128× | 1.27% | PASS |
| ac_reads | `oltp_range_select` | 18.62ms | 18.42ms | 0.989× | 3.08% | PASS |
| ac_reads | `oltp_sum_range` | 16.70ms | 18.13ms | 1.086× | 2.24% | PASS |
| ac_reads | `oltp_order_range` | 4.25ms | 4.16ms | 0.979× | 4.08% | PASS |
| ac_reads | `oltp_distinct_range` | 5.26ms | 5.17ms | 0.984× | 3.97% | PASS |
| ac_reads | `oltp_index_scan` | 7.91ms | 9.30ms | 1.176× | 1.91% | PASS |
| ac_reads | `select_random_points` | 23.68ms | 26.47ms | 1.118× | 2.65% | PASS |
| ac_reads | `select_random_ranges` | 7.29ms | 8.06ms | 1.106× | 1.43% | PASS |
| ac_reads | `covering_index_scan` | 9.53ms | 7.74ms | 0.813× | 2.08% | PASS |
| ac_reads | `groupby_scan` | 34.82ms | 35.95ms | 1.033× | 1.04% | PASS |
| ac_reads | `index_join` | 10.67ms | 12.54ms | 1.175× | 2.41% | PASS |
| ac_reads | `index_join_scan` | 5.81ms | 6.46ms | 1.111× | 3.14% | PASS |
| ac_reads | `types_table_scan` | 1.28s | 1.29s | 1.007× | 1.18% | PASS |
| ac_reads | `table_scan` | 1.57s | 1.43s | 0.912× | 1.41% | PASS |
| ac_reads | `oltp_read_only` | 173.12ms | 184.71ms | 1.067× | 1.78% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.69ms | 86.21ms | 3.639× | 6.77% | PASS |
| ac_writes | `oltp_insert_ac` | 27.45ms | 104.87ms | 3.820× | 7.50% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.75ms | 124.15ms | 4.174× | 5.96% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.65ms | 99.76ms | 4.047× | 6.83% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 27.98ms | 115.25ms | 4.119× | 5.85% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.75ms | 112.27ms | 4.046× | 4.83% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.89ms | 106.90ms | 4.475× | 5.63% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.94ms | 120.77ms | 3.558× | 6.17% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.90ms | 34.97ms | 1.132× | 1.57% | PASS |
| mem_reads | `oltp_range_select` | 13.90ms | 13.66ms | 0.983× | 1.77% | PASS |
| mem_reads | `oltp_sum_range` | 12.57ms | 13.25ms | 1.054× | 2.13% | PASS |
| mem_reads | `oltp_order_range` | 3.10ms | 3.17ms | 1.021× | 0.85% | PASS |
| mem_reads | `oltp_distinct_range` | 4.16ms | 4.18ms | 1.005× | 1.15% | PASS |
| mem_reads | `oltp_index_scan` | 4.65ms | 5.91ms | 1.272× | 1.97% | PASS |
| mem_reads | `select_random_points` | 17.59ms | 19.56ms | 1.112× | 2.19% | PASS |
| mem_reads | `select_random_ranges` | 4.30ms | 5.25ms | 1.222× | 1.53% | PASS |
| mem_reads | `covering_index_scan` | 4.59ms | 4.49ms | 0.977× | 2.86% | PASS |
| mem_reads | `groupby_scan` | 33.42ms | 35.06ms | 1.049× | 0.68% | PASS |
| mem_reads | `index_join` | 6.85ms | 8.74ms | 1.276× | 1.52% | PASS |
| mem_reads | `index_join_scan` | 4.44ms | 5.67ms | 1.278× | 1.51% | PASS |
| mem_reads | `types_table_scan` | 1.15s | 1.26s | 1.095× | 2.58% | PASS |
| mem_reads | `table_scan` | 1.48s | 1.39s | 0.941× | 3.05% | PASS |
| mem_reads | `oltp_read_only` | 121.63ms | 129.87ms | 1.068× | 1.94% | PASS |
| mem_writes | `oltp_bulk_insert` | 237.34ms | 330.65ms | 1.393× | 1.08% | PASS |
| mem_writes | `oltp_insert` | 20.49ms | 39.13ms | 1.910× | 0.86% | PASS |
| mem_writes | `oltp_update_index` | 71.06ms | 133.19ms | 1.874× | 1.16% | PASS |
| mem_writes | `oltp_update_non_index` | 50.82ms | 86.12ms | 1.695× | 1.03% | PASS |
| mem_writes | `oltp_delete_insert` | 50.87ms | 104.73ms | 2.059× | 1.14% | PASS |
| mem_writes | `oltp_write_only` | 28.77ms | 63.92ms | 2.222× | 0.79% | PASS |
| mem_writes | `types_delete_insert` | 33.23ms | 54.28ms | 1.633× | 1.12% | PASS |
| mem_writes | `oltp_read_write` | 85.75ms | 136.99ms | 1.598× | 1.46% | PASS |
| file_reads | `oltp_point_select` | 128.26ms | 67.43ms | 0.526× | 0.64% | PASS |
| file_reads | `oltp_range_select` | 24.14ms | 16.95ms | 0.702× | 1.60% | PASS |
| file_reads | `oltp_sum_range` | 22.23ms | 16.47ms | 0.741× | 1.50% | PASS |
| file_reads | `oltp_order_range` | 4.12ms | 3.52ms | 0.855× | 2.18% | PASS |
| file_reads | `oltp_distinct_range` | 5.11ms | 4.51ms | 0.883× | 1.70% | PASS |
| file_reads | `oltp_index_scan` | 14.37ms | 9.37ms | 0.652× | 2.17% | PASS |
| file_reads | `select_random_points` | 27.01ms | 23.13ms | 0.856× | 2.91% | PASS |
| file_reads | `select_random_ranges` | 13.60ms | 8.48ms | 0.624× | 1.78% | PASS |
| file_reads | `covering_index_scan` | 14.61ms | 7.91ms | 0.542× | 2.56% | PASS |
| file_reads | `groupby_scan` | 34.48ms | 35.54ms | 1.031× | 0.84% | PASS |
| file_reads | `index_join` | 12.26ms | 11.41ms | 0.931× | 1.46% | PASS |
| file_reads | `index_join_scan` | 5.46ms | 6.14ms | 1.126× | 1.75% | PASS |
| file_reads | `types_table_scan` | 1.12s | 1.24s | 1.106× | 0.67% | PASS |
| file_reads | `table_scan` | 1.28s | 1.35s | 1.049× | 0.53% | PASS |
| file_reads | `oltp_read_only` | 257.40ms | 173.53ms | 0.674× | 0.62% | PASS |
| file_writes | `oltp_bulk_insert` | 259.49ms | 359.21ms | 1.384× | 0.71% | PASS |
| file_writes | `oltp_insert` | 31.24ms | 52.22ms | 1.672× | 1.52% | PASS |
| file_writes | `oltp_update_index` | 106.93ms | 167.42ms | 1.566× | 1.37% | PASS |
| file_writes | `oltp_update_non_index` | 79.58ms | 109.47ms | 1.376× | 1.03% | PASS |
| file_writes | `oltp_delete_insert` | 82.97ms | 133.28ms | 1.606× | 1.61% | PASS |
| file_writes | `oltp_write_only` | 55.52ms | 86.95ms | 1.566× | 1.28% | PASS |
| file_writes | `types_delete_insert` | 51.70ms | 71.75ms | 1.388× | 1.63% | PASS |
| file_writes | `oltp_read_write` | 110.79ms | 157.07ms | 1.418× | 1.45% | PASS |
| ac_reads | `oltp_point_select` | 61.68ms | 66.45ms | 1.077× | 0.78% | PASS |
| ac_reads | `oltp_range_select` | 17.32ms | 16.83ms | 0.972× | 1.48% | PASS |
| ac_reads | `oltp_sum_range` | 15.62ms | 16.43ms | 1.052× | 1.71% | PASS |
| ac_reads | `oltp_order_range` | 3.53ms | 3.51ms | 0.994× | 1.17% | PASS |
| ac_reads | `oltp_distinct_range` | 4.56ms | 4.53ms | 0.993× | 1.34% | PASS |
| ac_reads | `oltp_index_scan` | 8.09ms | 9.37ms | 1.158× | 1.67% | PASS |
| ac_reads | `select_random_points` | 21.43ms | 23.26ms | 1.085× | 1.24% | PASS |
| ac_reads | `select_random_ranges` | 7.41ms | 8.47ms | 1.142× | 0.82% | PASS |
| ac_reads | `covering_index_scan` | 8.34ms | 7.90ms | 0.947× | 1.32% | PASS |
| ac_reads | `groupby_scan` | 33.98ms | 35.47ms | 1.044× | 0.76% | PASS |
| ac_reads | `index_join` | 9.07ms | 11.34ms | 1.249× | 1.40% | PASS |
| ac_reads | `index_join_scan` | 4.96ms | 6.15ms | 1.240× | 1.25% | PASS |
| ac_reads | `types_table_scan` | 1.12s | 1.24s | 1.107× | 0.63% | PASS |
| ac_reads | `table_scan` | 1.34s | 1.37s | 1.025× | 3.18% | PASS |
| ac_reads | `oltp_read_only` | 165.65ms | 174.16ms | 1.051× | 1.40% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.45ms | 63.77ms | 3.877× | 4.35% | PASS |
| ac_writes | `oltp_insert_ac` | 18.24ms | 84.74ms | 4.646× | 3.01% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.23ms | 98.51ms | 4.868× | 4.08% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.02ms | 74.68ms | 4.660× | 3.86% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.82ms | 87.39ms | 4.903× | 3.01% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.13ms | 87.12ms | 4.805× | 3.02% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.73ms | 75.35ms | 4.789× | 3.71% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.20ms | 93.30ms | 3.855× | 2.64% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.86ms | 40.15ms | 1.260× | 1.13% | PASS |
| mem_reads | `oltp_range_select` | 17.66ms | 20.95ms | 1.186× | 1.42% | PASS |
| mem_reads | `oltp_sum_range` | 17.37ms | 20.76ms | 1.195× | 2.02% | PASS |
| mem_reads | `oltp_order_range` | 3.57ms | 3.86ms | 1.081× | 1.32% | PASS |
| mem_reads | `oltp_distinct_range` | 4.75ms | 4.98ms | 1.049× | 1.15% | PASS |
| mem_reads | `oltp_index_scan` | 4.62ms | 6.22ms | 1.347× | 2.69% | PASS |
| mem_reads | `select_random_points` | 28.09ms | 31.94ms | 1.137× | 2.49% | PASS |
| mem_reads | `select_random_ranges` | 7.64ms | 9.04ms | 1.183× | 1.93% | PASS |
| mem_reads | `covering_index_scan` | 4.25ms | 4.36ms | 1.026× | 2.40% | PASS |
| mem_reads | `groupby_scan` | 36.51ms | 39.44ms | 1.080× | 0.95% | PASS |
| mem_reads | `index_join` | 8.17ms | 10.29ms | 1.259× | 1.49% | PASS |
| mem_reads | `index_join_scan` | 4.18ms | 5.38ms | 1.289× | 2.16% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.24s | 1.184× | 0.65% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.38s | 1.179× | 0.61% | PASS |
| mem_reads | `oltp_read_only` | 148.72ms | 169.17ms | 1.138× | 1.53% | PASS |
| mem_writes | `oltp_bulk_insert` | 248.14ms | 357.58ms | 1.441× | 0.84% | PASS |
| mem_writes | `oltp_insert` | 19.32ms | 36.84ms | 1.907× | 0.86% | PASS |
| mem_writes | `oltp_update_index` | 68.72ms | 118.38ms | 1.723× | 1.13% | PASS |
| mem_writes | `oltp_update_non_index` | 49.67ms | 81.82ms | 1.647× | 1.08% | PASS |
| mem_writes | `oltp_delete_insert` | 49.20ms | 94.84ms | 1.928× | 1.01% | PASS |
| mem_writes | `oltp_write_only` | 26.85ms | 57.30ms | 2.134× | 1.33% | PASS |
| mem_writes | `types_delete_insert` | 32.68ms | 54.86ms | 1.679× | 1.49% | PASS |
| mem_writes | `oltp_read_write` | 102.89ms | 156.11ms | 1.517× | 1.25% | PASS |
| file_reads | `oltp_point_select` | 108.57ms | 66.87ms | 0.616× | 1.31% | PASS |
| file_reads | `oltp_range_select` | 26.18ms | 24.33ms | 0.929× | 1.78% | PASS |
| file_reads | `oltp_sum_range` | 25.27ms | 24.14ms | 0.956× | 1.57% | PASS |
| file_reads | `oltp_order_range` | 4.66ms | 4.57ms | 0.982× | 2.83% | PASS |
| file_reads | `oltp_distinct_range` | 6.05ms | 5.81ms | 0.960× | 2.89% | PASS |
| file_reads | `oltp_index_scan` | 12.51ms | 9.56ms | 0.765× | 1.89% | PASS |
| file_reads | `select_random_points` | 39.57ms | 37.69ms | 0.953× | 2.88% | PASS |
| file_reads | `select_random_ranges` | 15.95ms | 12.46ms | 0.781× | 2.01% | PASS |
| file_reads | `covering_index_scan` | 12.38ms | 7.40ms | 0.597× | 1.54% | PASS |
| file_reads | `groupby_scan` | 38.05ms | 40.67ms | 1.069× | 1.08% | PASS |
| file_reads | `index_join` | 12.61ms | 12.95ms | 1.027× | 1.64% | PASS |
| file_reads | `index_join_scan` | 5.54ms | 6.48ms | 1.170× | 3.05% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.23s | 1.194× | 0.95% | PASS |
| file_reads | `table_scan` | 1.19s | 1.39s | 1.170× | 0.93% | PASS |
| file_reads | `oltp_read_only` | 260.53ms | 210.29ms | 0.807× | 0.84% | PASS |
| file_writes | `oltp_bulk_insert` | 264.74ms | 385.47ms | 1.456× | 1.13% | PASS |
| file_writes | `oltp_insert` | 26.86ms | 47.73ms | 1.777× | 1.68% | PASS |
| file_writes | `oltp_update_index` | 100.52ms | 148.60ms | 1.478× | 1.15% | PASS |
| file_writes | `oltp_update_non_index` | 77.92ms | 106.67ms | 1.369× | 1.42% | PASS |
| file_writes | `oltp_delete_insert` | 79.01ms | 124.39ms | 1.574× | 1.59% | PASS |
| file_writes | `oltp_write_only` | 52.88ms | 80.83ms | 1.529× | 2.06% | PASS |
| file_writes | `types_delete_insert` | 50.52ms | 69.57ms | 1.377× | 1.51% | PASS |
| file_writes | `oltp_read_write` | 129.89ms | 179.08ms | 1.379× | 1.81% | PASS |
| ac_reads | `oltp_point_select` | 59.16ms | 68.39ms | 1.156× | 0.89% | PASS |
| ac_reads | `oltp_range_select` | 22.22ms | 24.54ms | 1.104× | 1.73% | PASS |
| ac_reads | `oltp_sum_range` | 20.67ms | 24.23ms | 1.172× | 1.18% | PASS |
| ac_reads | `oltp_order_range` | 4.02ms | 4.33ms | 1.078× | 1.61% | PASS |
| ac_reads | `oltp_distinct_range` | 5.11ms | 5.42ms | 1.059× | 1.26% | PASS |
| ac_reads | `oltp_index_scan` | 7.51ms | 9.29ms | 1.237× | 2.28% | PASS |
| ac_reads | `select_random_points` | 30.89ms | 36.41ms | 1.178× | 1.66% | PASS |
| ac_reads | `select_random_ranges` | 10.51ms | 12.24ms | 1.165× | 1.55% | PASS |
| ac_reads | `covering_index_scan` | 7.05ms | 7.24ms | 1.027× | 1.96% | PASS |
| ac_reads | `groupby_scan` | 36.47ms | 39.91ms | 1.094× | 0.99% | PASS |
| ac_reads | `index_join` | 9.76ms | 12.73ms | 1.304× | 1.49% | PASS |
| ac_reads | `index_join_scan` | 4.57ms | 5.92ms | 1.295× | 1.68% | PASS |
| ac_reads | `types_table_scan` | 1.10s | 1.25s | 1.138× | 2.53% | PASS |
| ac_reads | `table_scan` | 1.34s | 1.41s | 1.052× | 3.85% | PASS |
| ac_reads | `oltp_read_only` | 192.76ms | 213.24ms | 1.106× | 1.68% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.37ms | 83.06ms | 3.554× | 7.29% | PASS |
| ac_writes | `oltp_insert_ac` | 25.99ms | 106.25ms | 4.088× | 4.85% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.43ms | 114.27ms | 4.166× | 6.14% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.74ms | 96.19ms | 4.052× | 5.65% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.43ms | 106.08ms | 4.171× | 6.49% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.43ms | 108.29ms | 4.097× | 6.24% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.62ms | 96.24ms | 4.254× | 5.02% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.41ms | 110.56ms | 3.520× | 4.26% | PASS |

</details>

## Version-control latency

Wall time: 2m 5s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 66.60ms | 200.00ms | 33.3% | 0.69% | PASS |
| `status_dirty_many_tables` | 69.38ms | 200.00ms | 34.7% | 0.65% | PASS |
| `diff_regular_working_one_table` | 62.27ms | 150.00ms | 41.5% | 0.83% | PASS |
| `diff_regular_working_many_tables` | 74.44ms | 200.00ms | 37.2% | 0.56% | PASS |
| `diff_stat_working_many_tables` | 74.50ms | 200.00ms | 37.2% | 0.68% | PASS |
| `diff_schema_working_many_tables` | 74.97ms | 200.00ms | 37.5% | 0.60% | PASS |
| `branch_list_many_branches` | 20.64ms | 100.00ms | 20.6% | 1.11% | PASS |
| `branch_create_delete` | 31.36ms | 100.00ms | 31.4% | 1.98% | PASS |
| `checkout_branch_clean` | 100.44ms | 200.00ms | 50.2% | 1.78% | PASS |
| `merge_data_no_conflicts` | 38.22ms | 150.00ms | 25.5% | 1.62% | PASS |
| `merge_schema_no_conflicts` | 21.75ms | 100.00ms | 21.7% | 2.36% | PASS |
| `merge_data_conflicts` | 78.70ms | 250.00ms | 31.5% | 0.59% | PASS |
| `merge_data_conflicts_with_resolve` | 78.67ms | 250.00ms | 31.5% | 0.64% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
