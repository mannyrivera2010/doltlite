# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-28 17:50 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261026.354.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/40723722016)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 14m 39s | 9.00s | 11.47s | 1.275× | 1.69% | **PASS** |
| textpk | 69 | 55 | 1h 14m 35s | 7.58s | 8.24s | 1.087× | 3.20% | **PASS** |
| blobpk | 69 | 55 | 1h 33m 30s | 9.68s | 11.80s | 1.219× | 1.65% | **PASS** |
| compositepk | 69 | 55 | 1h 11m 18s | 8.01s | 9.64s | 1.204× | 1.14% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.28ms | 29.63ms | 1.221× | 2.12% | PASS |
| mem_reads | `oltp_range_select` | 9.90ms | 13.08ms | 1.322× | 3.23% | PASS |
| mem_reads | `oltp_sum_range` | 9.40ms | 12.40ms | 1.319× | 2.18% | PASS |
| mem_reads | `oltp_order_range` | 2.60ms | 2.98ms | 1.149× | 1.65% | PASS |
| mem_reads | `oltp_distinct_range` | 3.68ms | 4.04ms | 1.100× | 1.33% | PASS |
| mem_reads | `oltp_index_scan` | 3.88ms | 5.29ms | 1.363× | 1.62% | PASS |
| mem_reads | `select_random_points` | 9.89ms | 11.22ms | 1.134× | 3.13% | PASS |
| mem_reads | `select_random_ranges` | 2.94ms | 3.96ms | 1.345× | 3.32% | PASS |
| mem_reads | `covering_index_scan` | 4.23ms | 4.10ms | 0.970× | 1.23% | PASS |
| mem_reads | `groupby_scan` | 29.75ms | 33.27ms | 1.118× | 0.75% | PASS |
| mem_reads | `index_join` | 6.01ms | 7.98ms | 1.327× | 1.31% | PASS |
| mem_reads | `index_join_scan` | 3.52ms | 4.63ms | 1.317× | 1.73% | PASS |
| mem_reads | `types_table_scan` | 1.12s | 1.36s | 1.213× | 0.91% | PASS |
| mem_reads | `table_scan` | 1.22s | 1.41s | 1.153× | 2.72% | PASS |
| mem_reads | `oltp_read_only` | 103.89ms | 123.82ms | 1.192× | 1.17% | PASS |
| mem_writes | `oltp_bulk_insert` | 183.25ms | 263.08ms | 1.436× | 1.23% | PASS |
| mem_writes | `oltp_insert` | 15.76ms | 29.92ms | 1.899× | 0.75% | PASS |
| mem_writes | `oltp_update_index` | 51.09ms | 112.90ms | 2.210× | 1.58% | PASS |
| mem_writes | `oltp_update_non_index` | 35.38ms | 65.51ms | 1.852× | 1.60% | PASS |
| mem_writes | `oltp_delete_insert` | 45.71ms | 84.70ms | 1.853× | 1.36% | PASS |
| mem_writes | `oltp_write_only` | 22.24ms | 48.99ms | 2.203× | 1.50% | PASS |
| mem_writes | `types_delete_insert` | 25.01ms | 44.37ms | 1.774× | 1.61% | PASS |
| mem_writes | `oltp_read_write` | 73.84ms | 120.44ms | 1.631× | 2.02% | PASS |
| file_reads | `oltp_point_select` | 96.53ms | 55.32ms | 0.573× | 0.96% | PASS |
| file_reads | `oltp_range_select` | 18.81ms | 16.18ms | 0.860× | 1.64% | PASS |
| file_reads | `oltp_sum_range` | 17.99ms | 15.43ms | 0.858× | 2.38% | PASS |
| file_reads | `oltp_order_range` | 3.61ms | 3.39ms | 0.939× | 1.32% | PASS |
| file_reads | `oltp_distinct_range` | 4.70ms | 4.46ms | 0.950× | 2.39% | PASS |
| file_reads | `oltp_index_scan` | 11.03ms | 8.12ms | 0.736× | 1.96% | PASS |
| file_reads | `select_random_points` | 16.78ms | 13.81ms | 0.823× | 1.54% | PASS |
| file_reads | `select_random_ranges` | 10.07ms | 6.47ms | 0.643× | 1.23% | PASS |
| file_reads | `covering_index_scan` | 11.80ms | 7.27ms | 0.616× | 1.97% | PASS |
| file_reads | `groupby_scan` | 31.57ms | 34.16ms | 1.082× | 0.97% | PASS |
| file_reads | `index_join` | 10.26ms | 10.23ms | 0.998× | 1.58% | PASS |
| file_reads | `index_join_scan` | 4.40ms | 4.99ms | 1.134× | 1.55% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.32s | 1.283× | 0.77% | PASS |
| file_reads | `table_scan` | 1.16s | 1.39s | 1.197× | 0.43% | PASS |
| file_reads | `oltp_read_only` | 203.23ms | 159.46ms | 0.785× | 0.71% | PASS |
| file_writes | `oltp_bulk_insert` | 196.56ms | 281.80ms | 1.434× | 1.10% | PASS |
| file_writes | `oltp_insert` | 22.79ms | 36.55ms | 1.604× | 2.07% | PASS |
| file_writes | `oltp_update_index` | 82.56ms | 145.89ms | 1.767× | 1.90% | PASS |
| file_writes | `oltp_update_non_index` | 60.24ms | 86.02ms | 1.428× | 2.53% | PASS |
| file_writes | `oltp_delete_insert` | 69.81ms | 110.21ms | 1.579× | 1.75% | PASS |
| file_writes | `oltp_write_only` | 46.62ms | 68.14ms | 1.462× | 2.14% | PASS |
| file_writes | `types_delete_insert` | 41.78ms | 54.23ms | 1.298× | 1.69% | PASS |
| file_writes | `oltp_read_write` | 96.05ms | 135.47ms | 1.410× | 2.13% | PASS |
| ac_reads | `oltp_point_select` | 49.41ms | 54.39ms | 1.101× | 1.32% | PASS |
| ac_reads | `oltp_range_select` | 14.04ms | 16.12ms | 1.148× | 1.99% | PASS |
| ac_reads | `oltp_sum_range` | 13.20ms | 15.42ms | 1.168× | 1.78% | PASS |
| ac_reads | `oltp_order_range` | 3.08ms | 3.37ms | 1.094× | 1.39% | PASS |
| ac_reads | `oltp_distinct_range` | 4.11ms | 4.43ms | 1.078× | 1.78% | PASS |
| ac_reads | `oltp_index_scan` | 6.65ms | 8.24ms | 1.239× | 2.88% | PASS |
| ac_reads | `select_random_points` | 14.36ms | 14.17ms | 0.987× | 3.93% | PASS |
| ac_reads | `select_random_ranges` | 5.70ms | 6.60ms | 1.157× | 1.48% | PASS |
| ac_reads | `covering_index_scan` | 7.35ms | 7.13ms | 0.970× | 1.68% | PASS |
| ac_reads | `groupby_scan` | 30.76ms | 34.13ms | 1.110× | 0.75% | PASS |
| ac_reads | `index_join` | 8.00ms | 10.32ms | 1.291× | 1.90% | PASS |
| ac_reads | `index_join_scan` | 4.05ms | 5.10ms | 1.258× | 2.97% | PASS |
| ac_reads | `types_table_scan` | 1.08s | 1.34s | 1.240× | 2.17% | PASS |
| ac_reads | `table_scan` | 1.19s | 1.40s | 1.183× | 1.51% | PASS |
| ac_reads | `oltp_read_only` | 135.56ms | 158.81ms | 1.172× | 0.69% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 19.31ms | 60.52ms | 3.134× | 5.04% | PASS |
| ac_writes | `oltp_insert_ac` | 21.68ms | 76.54ms | 3.531× | 5.73% | PASS |
| ac_writes | `oltp_update_index_ac` | 24.03ms | 91.25ms | 3.798× | 4.31% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 19.59ms | 70.09ms | 3.578× | 6.36% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 21.68ms | 82.66ms | 3.813× | 5.85% | PASS |
| ac_writes | `oltp_write_only_ac` | 21.79ms | 81.24ms | 3.729× | 4.27% | PASS |
| ac_writes | `types_delete_insert_ac` | 19.85ms | 71.02ms | 3.578× | 6.27% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.25ms | 88.60ms | 3.251× | 6.23% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.64ms | 24.92ms | 1.100× | 3.19% | PASS |
| mem_reads | `oltp_range_select` | 11.38ms | 10.29ms | 0.905× | 3.59% | PASS |
| mem_reads | `oltp_sum_range` | 11.14ms | 10.26ms | 0.921× | 3.24% | PASS |
| mem_reads | `oltp_order_range` | 2.40ms | 2.18ms | 0.911× | 1.49% | PASS |
| mem_reads | `oltp_distinct_range` | 2.92ms | 2.69ms | 0.923× | 1.37% | PASS |
| mem_reads | `oltp_index_scan` | 3.55ms | 4.79ms | 1.347× | 2.29% | PASS |
| mem_reads | `select_random_points` | 16.16ms | 16.79ms | 1.039× | 1.92% | PASS |
| mem_reads | `select_random_ranges` | 3.48ms | 3.96ms | 1.139× | 1.73% | PASS |
| mem_reads | `covering_index_scan` | 3.62ms | 3.66ms | 1.013× | 7.40% | PASS |
| mem_reads | `groupby_scan` | 22.54ms | 21.89ms | 0.971× | 1.47% | PASS |
| mem_reads | `index_join` | 5.53ms | 8.05ms | 1.457× | 5.50% | PASS |
| mem_reads | `index_join_scan` | 4.64ms | 5.57ms | 1.199× | 3.11% | PASS |
| mem_reads | `types_table_scan` | 790.65ms | 824.01ms | 1.042× | 4.55% | PASS |
| mem_reads | `table_scan` | 876.01ms | 885.90ms | 1.011× | 3.20% | PASS |
| mem_reads | `oltp_read_only` | 75.95ms | 78.43ms | 1.033× | 2.93% | PASS |
| mem_writes | `oltp_bulk_insert` | 146.25ms | 227.64ms | 1.556× | 0.95% | PASS |
| mem_writes | `oltp_insert` | 14.67ms | 28.98ms | 1.976× | 2.99% | PASS |
| mem_writes | `oltp_update_index` | 55.42ms | 110.32ms | 1.991× | 4.40% | PASS |
| mem_writes | `oltp_update_non_index` | 37.99ms | 71.13ms | 1.872× | 2.19% | PASS |
| mem_writes | `oltp_delete_insert` | 41.43ms | 84.54ms | 2.041× | 3.06% | PASS |
| mem_writes | `oltp_write_only` | 20.38ms | 48.38ms | 2.374× | 4.04% | PASS |
| mem_writes | `types_delete_insert` | 23.06ms | 41.51ms | 1.800× | 4.65% | PASS |
| mem_writes | `oltp_read_write` | 57.74ms | 93.81ms | 1.625× | 4.85% | PASS |
| file_reads | `oltp_point_select` | 77.18ms | 43.09ms | 0.558× | 2.21% | PASS |
| file_reads | `oltp_range_select` | 16.39ms | 11.15ms | 0.680× | 2.91% | PASS |
| file_reads | `oltp_sum_range` | 17.78ms | 12.02ms | 0.676× | 2.45% | PASS |
| file_reads | `oltp_order_range` | 3.10ms | 2.39ms | 0.771× | 2.62% | PASS |
| file_reads | `oltp_distinct_range` | 3.77ms | 3.01ms | 0.800× | 1.91% | PASS |
| file_reads | `oltp_index_scan` | 9.91ms | 6.79ms | 0.685× | 2.02% | PASS |
| file_reads | `select_random_points` | 20.53ms | 16.64ms | 0.811× | 2.63% | PASS |
| file_reads | `select_random_ranges` | 8.99ms | 5.69ms | 0.633× | 1.25% | PASS |
| file_reads | `covering_index_scan` | 10.22ms | 5.43ms | 0.531× | 2.65% | PASS |
| file_reads | `groupby_scan` | 22.13ms | 21.02ms | 0.950× | 1.57% | PASS |
| file_reads | `index_join` | 9.32ms | 8.23ms | 0.883× | 4.32% | PASS |
| file_reads | `index_join_scan` | 4.75ms | 4.91ms | 1.034× | 4.80% | PASS |
| file_reads | `types_table_scan` | 851.87ms | 818.51ms | 0.961× | 4.81% | PASS |
| file_reads | `table_scan` | 938.68ms | 890.04ms | 0.948× | 8.69% | PASS |
| file_reads | `oltp_read_only` | 152.23ms | 103.09ms | 0.677× | 2.60% | PASS |
| file_writes | `oltp_bulk_insert` | 204.93ms | 300.33ms | 1.466× | 3.85% | PASS |
| file_writes | `oltp_insert` | 51.51ms | 58.37ms | 1.133× | 5.77% | PASS |
| file_writes | `oltp_update_index` | 201.28ms | 204.50ms | 1.016× | 3.27% | PASS |
| file_writes | `oltp_update_non_index` | 137.54ms | 137.78ms | 1.002× | 3.22% | PASS |
| file_writes | `oltp_delete_insert` | 161.63ms | 159.03ms | 0.984× | 2.25% | PASS |
| file_writes | `oltp_write_only` | 121.22ms | 109.68ms | 0.905× | 2.67% | PASS |
| file_writes | `types_delete_insert` | 93.07ms | 89.25ms | 0.959× | 10.91% | PASS |
| file_writes | `oltp_read_write` | 161.40ms | 154.41ms | 0.957× | 4.47% | PASS |
| ac_reads | `oltp_point_select` | 40.07ms | 43.02ms | 1.074× | 1.24% | PASS |
| ac_reads | `oltp_range_select` | 12.71ms | 11.18ms | 0.880× | 3.35% | PASS |
| ac_reads | `oltp_sum_range` | 11.95ms | 11.01ms | 0.922× | 3.38% | PASS |
| ac_reads | `oltp_order_range` | 2.57ms | 2.27ms | 0.883× | 2.12% | PASS |
| ac_reads | `oltp_distinct_range` | 3.11ms | 2.80ms | 0.899× | 2.21% | PASS |
| ac_reads | `oltp_index_scan` | 5.41ms | 6.21ms | 1.147× | 3.61% | PASS |
| ac_reads | `select_random_points` | 16.67ms | 16.36ms | 0.981× | 4.45% | PASS |
| ac_reads | `select_random_ranges` | 5.21ms | 5.51ms | 1.056× | 1.42% | PASS |
| ac_reads | `covering_index_scan` | 6.47ms | 5.28ms | 0.816× | 3.09% | PASS |
| ac_reads | `groupby_scan` | 21.22ms | 20.47ms | 0.965× | 1.70% | PASS |
| ac_reads | `index_join` | 7.06ms | 7.85ms | 1.112× | 4.92% | PASS |
| ac_reads | `index_join_scan` | 4.18ms | 4.61ms | 1.103× | 4.42% | PASS |
| ac_reads | `types_table_scan` | 720.31ms | 773.91ms | 1.074× | 0.93% | PASS |
| ac_reads | `table_scan` | 866.96ms | 887.13ms | 1.023× | 2.96% | PASS |
| ac_reads | `oltp_read_only` | 100.49ms | 102.48ms | 1.020× | 3.32% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 26.54ms | 56.54ms | 2.130× | 8.18% | PASS |
| ac_writes | `oltp_insert_ac` | 30.84ms | 72.65ms | 2.356× | 9.92% | PASS |
| ac_writes | `oltp_update_index_ac` | 31.53ms | 81.67ms | 2.591× | 6.38% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.84ms | 62.30ms | 2.411× | 5.92% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 28.80ms | 72.09ms | 2.503× | 5.82% | PASS |
| ac_writes | `oltp_write_only_ac` | 29.39ms | 72.20ms | 2.456× | 4.84% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.13ms | 63.95ms | 3.177× | 3.48% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.59ms | 82.24ms | 2.448× | 7.00% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.10ms | 36.70ms | 1.219× | 1.68% | PASS |
| mem_reads | `oltp_range_select` | 12.65ms | 13.85ms | 1.094× | 2.23% | PASS |
| mem_reads | `oltp_sum_range` | 11.82ms | 13.86ms | 1.173× | 2.32% | PASS |
| mem_reads | `oltp_order_range` | 2.87ms | 3.12ms | 1.086× | 1.30% | PASS |
| mem_reads | `oltp_distinct_range` | 3.98ms | 4.21ms | 1.058× | 1.23% | PASS |
| mem_reads | `oltp_index_scan` | 4.50ms | 6.16ms | 1.366× | 1.43% | PASS |
| mem_reads | `select_random_points` | 17.67ms | 20.41ms | 1.155× | 2.28% | PASS |
| mem_reads | `select_random_ranges` | 3.96ms | 5.10ms | 1.286× | 1.27% | PASS |
| mem_reads | `covering_index_scan` | 4.40ms | 4.51ms | 1.025× | 1.65% | PASS |
| mem_reads | `groupby_scan` | 31.61ms | 34.18ms | 1.081× | 0.97% | PASS |
| mem_reads | `index_join` | 6.80ms | 9.16ms | 1.347× | 1.99% | PASS |
| mem_reads | `index_join_scan` | 4.30ms | 5.22ms | 1.214× | 2.29% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.23s | 1.174× | 0.93% | PASS |
| mem_reads | `table_scan` | 1.22s | 1.38s | 1.128× | 1.93% | PASS |
| mem_reads | `oltp_read_only` | 125.51ms | 139.50ms | 1.112× | 1.58% | PASS |
| mem_writes | `oltp_bulk_insert` | 243.40ms | 367.21ms | 1.509× | 0.84% | PASS |
| mem_writes | `oltp_insert` | 20.27ms | 42.65ms | 2.104× | 0.69% | PASS |
| mem_writes | `oltp_update_index` | 70.42ms | 141.35ms | 2.007× | 1.59% | PASS |
| mem_writes | `oltp_update_non_index` | 50.25ms | 91.13ms | 1.814× | 0.89% | PASS |
| mem_writes | `oltp_delete_insert` | 49.97ms | 110.58ms | 2.213× | 1.27% | PASS |
| mem_writes | `oltp_write_only` | 28.72ms | 67.25ms | 2.342× | 1.15% | PASS |
| mem_writes | `types_delete_insert` | 33.16ms | 59.16ms | 1.784× | 1.89% | PASS |
| mem_writes | `oltp_read_write` | 84.97ms | 144.98ms | 1.706× | 1.31% | PASS |
| file_reads | `oltp_point_select` | 101.92ms | 61.03ms | 0.599× | 1.14% | PASS |
| file_reads | `oltp_range_select` | 20.48ms | 16.46ms | 0.804× | 2.54% | PASS |
| file_reads | `oltp_sum_range` | 19.64ms | 16.62ms | 0.846× | 2.19% | PASS |
| file_reads | `oltp_order_range` | 3.79ms | 3.44ms | 0.910× | 1.59% | PASS |
| file_reads | `oltp_distinct_range` | 4.88ms | 4.55ms | 0.932× | 1.32% | PASS |
| file_reads | `oltp_index_scan` | 12.07ms | 8.88ms | 0.736× | 1.57% | PASS |
| file_reads | `select_random_points` | 26.91ms | 23.76ms | 0.883× | 2.48% | PASS |
| file_reads | `select_random_ranges` | 11.28ms | 7.61ms | 0.675× | 1.36% | PASS |
| file_reads | `covering_index_scan` | 12.15ms | 7.16ms | 0.589× | 2.70% | PASS |
| file_reads | `groupby_scan` | 32.76ms | 34.69ms | 1.059× | 0.95% | PASS |
| file_reads | `index_join` | 11.64ms | 11.21ms | 0.963× | 2.03% | PASS |
| file_reads | `index_join_scan` | 5.45ms | 5.86ms | 1.076× | 3.13% | PASS |
| file_reads | `types_table_scan` | 1.09s | 1.24s | 1.139× | 1.37% | PASS |
| file_reads | `table_scan` | 1.34s | 1.39s | 1.042× | 1.48% | PASS |
| file_reads | `oltp_read_only` | 234.09ms | 174.28ms | 0.745× | 0.86% | PASS |
| file_writes | `oltp_bulk_insert` | 264.30ms | 395.23ms | 1.495× | 1.25% | PASS |
| file_writes | `oltp_insert` | 32.47ms | 55.68ms | 1.715× | 1.82% | PASS |
| file_writes | `oltp_update_index` | 108.85ms | 177.21ms | 1.628× | 1.59% | PASS |
| file_writes | `oltp_update_non_index` | 81.73ms | 117.25ms | 1.435× | 1.45% | PASS |
| file_writes | `oltp_delete_insert` | 83.40ms | 140.51ms | 1.685× | 1.40% | PASS |
| file_writes | `oltp_write_only` | 58.13ms | 91.98ms | 1.582× | 1.76% | PASS |
| file_writes | `types_delete_insert` | 53.66ms | 76.11ms | 1.418× | 1.33% | PASS |
| file_writes | `oltp_read_write` | 118.66ms | 170.24ms | 1.435× | 1.84% | PASS |
| ac_reads | `oltp_point_select` | 55.09ms | 61.10ms | 1.109× | 1.19% | PASS |
| ac_reads | `oltp_range_select` | 16.40ms | 16.57ms | 1.010× | 2.26% | PASS |
| ac_reads | `oltp_sum_range` | 15.31ms | 16.57ms | 1.082× | 1.65% | PASS |
| ac_reads | `oltp_order_range` | 3.34ms | 3.45ms | 1.033× | 1.30% | PASS |
| ac_reads | `oltp_distinct_range` | 4.42ms | 4.56ms | 1.033× | 1.32% | PASS |
| ac_reads | `oltp_index_scan` | 7.50ms | 8.94ms | 1.192× | 1.72% | PASS |
| ac_reads | `select_random_points` | 22.26ms | 24.19ms | 1.087× | 2.19% | PASS |
| ac_reads | `select_random_ranges` | 6.85ms | 7.70ms | 1.123× | 1.51% | PASS |
| ac_reads | `covering_index_scan` | 7.99ms | 7.19ms | 0.900× | 2.94% | PASS |
| ac_reads | `groupby_scan` | 32.44ms | 34.83ms | 1.073× | 0.87% | PASS |
| ac_reads | `index_join` | 9.09ms | 11.27ms | 1.239× | 2.65% | PASS |
| ac_reads | `index_join_scan` | 4.93ms | 5.86ms | 1.188× | 2.32% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.23s | 1.166× | 1.00% | PASS |
| ac_reads | `table_scan` | 1.26s | 1.38s | 1.092× | 1.78% | PASS |
| ac_reads | `oltp_read_only` | 158.22ms | 172.09ms | 1.088× | 1.73% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.94ms | 64.36ms | 3.073× | 5.92% | PASS |
| ac_writes | `oltp_insert_ac` | 22.58ms | 83.72ms | 3.708× | 4.20% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.09ms | 94.50ms | 3.767× | 6.03% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.97ms | 73.59ms | 3.350× | 7.06% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.59ms | 87.44ms | 3.871× | 4.91% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.66ms | 85.19ms | 3.600× | 5.83% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.05ms | 78.53ms | 3.916× | 7.55% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.17ms | 94.30ms | 3.126× | 5.96% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.61ms | 27.09ms | 1.148× | 1.05% | PASS |
| mem_reads | `oltp_range_select` | 14.93ms | 16.03ms | 1.073× | 1.00% | PASS |
| mem_reads | `oltp_sum_range` | 14.22ms | 15.09ms | 1.061× | 0.79% | PASS |
| mem_reads | `oltp_order_range` | 2.90ms | 3.08ms | 1.064× | 1.44% | PASS |
| mem_reads | `oltp_distinct_range` | 3.73ms | 3.86ms | 1.035× | 0.93% | PASS |
| mem_reads | `oltp_index_scan` | 3.48ms | 4.37ms | 1.257× | 1.46% | PASS |
| mem_reads | `select_random_points` | 22.15ms | 24.89ms | 1.124× | 0.94% | PASS |
| mem_reads | `select_random_ranges` | 5.59ms | 6.37ms | 1.138× | 0.88% | PASS |
| mem_reads | `covering_index_scan` | 2.95ms | 3.07ms | 1.040× | 2.28% | PASS |
| mem_reads | `groupby_scan` | 29.84ms | 30.43ms | 1.020× | 0.57% | PASS |
| mem_reads | `index_join` | 6.34ms | 8.14ms | 1.284× | 0.80% | PASS |
| mem_reads | `index_join_scan` | 3.12ms | 4.55ms | 1.461× | 0.89% | PASS |
| mem_reads | `types_table_scan` | 874.92ms | 1.03s | 1.174× | 0.30% | PASS |
| mem_reads | `table_scan` | 992.17ms | 1.11s | 1.119× | 0.33% | PASS |
| mem_reads | `oltp_read_only` | 111.51ms | 122.36ms | 1.097× | 0.58% | PASS |
| mem_writes | `oltp_bulk_insert` | 170.90ms | 239.40ms | 1.401× | 0.86% | PASS |
| mem_writes | `oltp_insert` | 13.94ms | 26.06ms | 1.870× | 0.77% | PASS |
| mem_writes | `oltp_update_index` | 49.20ms | 88.74ms | 1.804× | 1.00% | PASS |
| mem_writes | `oltp_update_non_index` | 35.46ms | 60.99ms | 1.720× | 0.85% | PASS |
| mem_writes | `oltp_delete_insert` | 35.73ms | 70.65ms | 1.978× | 1.34% | PASS |
| mem_writes | `oltp_write_only` | 19.14ms | 42.78ms | 2.235× | 1.60% | PASS |
| mem_writes | `types_delete_insert` | 22.97ms | 39.37ms | 1.714× | 1.09% | PASS |
| mem_writes | `oltp_read_write` | 70.99ms | 109.03ms | 1.536× | 0.78% | PASS |
| file_reads | `oltp_point_select` | 49.28ms | 35.50ms | 0.720× | 0.80% | PASS |
| file_reads | `oltp_range_select` | 17.75ms | 17.08ms | 0.962× | 0.84% | PASS |
| file_reads | `oltp_sum_range` | 16.66ms | 16.00ms | 0.960× | 1.14% | PASS |
| file_reads | `oltp_order_range` | 3.21ms | 3.17ms | 0.987× | 1.42% | PASS |
| file_reads | `oltp_distinct_range` | 4.04ms | 4.00ms | 0.989× | 1.79% | PASS |
| file_reads | `oltp_index_scan` | 6.18ms | 5.37ms | 0.869× | 1.94% | PASS |
| file_reads | `select_random_points` | 24.99ms | 25.98ms | 1.040× | 0.82% | PASS |
| file_reads | `select_random_ranges` | 8.26ms | 7.25ms | 0.878× | 1.21% | PASS |
| file_reads | `covering_index_scan` | 5.64ms | 3.92ms | 0.694× | 0.88% | PASS |
| file_reads | `groupby_scan` | 29.69ms | 30.51ms | 1.028× | 0.42% | PASS |
| file_reads | `index_join` | 7.82ms | 8.78ms | 1.123× | 1.86% | PASS |
| file_reads | `index_join_scan` | 3.48ms | 4.76ms | 1.365× | 2.11% | PASS |
| file_reads | `types_table_scan` | 875.72ms | 1.03s | 1.179× | 0.88% | PASS |
| file_reads | `table_scan` | 998.22ms | 1.11s | 1.117× | 0.43% | PASS |
| file_reads | `oltp_read_only` | 149.33ms | 134.35ms | 0.900× | 0.52% | PASS |
| file_writes | `oltp_bulk_insert` | 219.11ms | 288.71ms | 1.318× | 3.38% | PASS |
| file_writes | `oltp_insert` | 20.15ms | 38.84ms | 1.928× | 7.67% | PASS |
| file_writes | `oltp_update_index` | 140.45ms | 149.53ms | 1.065× | 1.60% | PASS |
| file_writes | `oltp_update_non_index` | 120.12ms | 108.94ms | 0.907× | 2.36% | PASS |
| file_writes | `oltp_delete_insert` | 120.12ms | 119.74ms | 0.997× | 7.23% | PASS |
| file_writes | `oltp_write_only` | 80.63ms | 88.71ms | 1.100× | 0.79% | PASS |
| file_writes | `types_delete_insert` | 76.15ms | 76.61ms | 1.006× | 6.06% | PASS |
| file_writes | `oltp_read_write` | 130.44ms | 158.58ms | 1.216× | 1.78% | PASS |
| ac_reads | `oltp_point_select` | 32.17ms | 35.00ms | 1.088× | 1.05% | PASS |
| ac_reads | `oltp_range_select` | 16.12ms | 17.05ms | 1.058× | 1.52% | PASS |
| ac_reads | `oltp_sum_range` | 15.36ms | 16.07ms | 1.047× | 1.34% | PASS |
| ac_reads | `oltp_order_range` | 3.09ms | 3.18ms | 1.030× | 1.64% | PASS |
| ac_reads | `oltp_distinct_range` | 3.94ms | 4.00ms | 1.014× | 0.95% | PASS |
| ac_reads | `oltp_index_scan` | 4.66ms | 5.43ms | 1.166× | 1.99% | PASS |
| ac_reads | `select_random_points` | 23.67ms | 26.14ms | 1.104× | 0.92% | PASS |
| ac_reads | `select_random_ranges` | 6.75ms | 7.36ms | 1.090× | 1.25% | PASS |
| ac_reads | `covering_index_scan` | 4.00ms | 4.07ms | 1.019× | 2.49% | PASS |
| ac_reads | `groupby_scan` | 30.30ms | 30.67ms | 1.012× | 0.52% | PASS |
| ac_reads | `index_join` | 7.12ms | 9.01ms | 1.267× | 1.71% | PASS |
| ac_reads | `index_join_scan` | 3.40ms | 4.76ms | 1.402× | 1.58% | PASS |
| ac_reads | `types_table_scan` | 874.12ms | 1.03s | 1.177× | 0.30% | PASS |
| ac_reads | `table_scan` | 996.60ms | 1.12s | 1.120× | 0.49% | PASS |
| ac_reads | `oltp_read_only` | 124.28ms | 134.00ms | 1.078× | 0.78% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.79ms | 66.36ms | 2.677× | 3.91% | PASS |
| ac_writes | `oltp_insert_ac` | 29.17ms | 81.48ms | 2.793× | 4.84% | PASS |
| ac_writes | `oltp_update_index_ac` | 30.31ms | 89.77ms | 2.962× | 5.04% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.09ms | 74.74ms | 2.979× | 3.82% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 27.95ms | 84.68ms | 3.030× | 3.79% | PASS |
| ac_writes | `oltp_write_only_ac` | 28.67ms | 83.86ms | 2.925× | 4.79% | PASS |
| ac_writes | `types_delete_insert_ac` | 29.07ms | 74.14ms | 2.551× | 10.51% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.61ms | 89.75ms | 2.932× | 5.29% | PASS |

</details>

## Version-control latency

Wall time: 2m 23s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 84.78ms | 200.00ms | 42.4% | 0.47% | PASS |
| `status_dirty_many_tables` | 88.02ms | 200.00ms | 44.0% | 0.45% | PASS |
| `diff_regular_working_one_table` | 80.43ms | 150.00ms | 53.6% | 0.50% | PASS |
| `diff_regular_working_many_tables` | 93.70ms | 200.00ms | 46.8% | 0.59% | PASS |
| `diff_stat_working_many_tables` | 93.95ms | 200.00ms | 47.0% | 0.68% | PASS |
| `diff_schema_working_many_tables` | 94.61ms | 200.00ms | 47.3% | 0.64% | PASS |
| `branch_list_many_branches` | 23.45ms | 100.00ms | 23.4% | 0.81% | PASS |
| `branch_create_delete` | 25.77ms | 100.00ms | 25.8% | 1.32% | PASS |
| `checkout_branch_clean` | 57.46ms | 200.00ms | 28.7% | 1.08% | PASS |
| `merge_data_no_conflicts` | 30.36ms | 150.00ms | 20.2% | 1.29% | PASS |
| `merge_schema_no_conflicts` | 23.02ms | 100.00ms | 23.0% | 1.29% | PASS |
| `merge_data_conflicts` | 128.54ms | 250.00ms | 51.4% | 0.30% | PASS |
| `merge_data_conflicts_with_resolve` | 128.54ms | 250.00ms | 51.4% | 0.31% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
