# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-21 16:12 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261018.339.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/39618067459)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 33s | 8.84s | 11.38s | 1.287× | 1.58% | **PASS** |
| textpk | 69 | 55 | 1h 34m 40s | 9.73s | 11.82s | 1.215× | 1.83% | **PASS** |
| blobpk | 69 | 55 | 1h 32m 39s | 10.96s | 11.89s | 1.085× | 1.84% | **PASS** |
| compositepk | 69 | 55 | 1h 3m 1s | 7.24s | 7.83s | 1.081× | 2.39% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.10ms | 29.86ms | 1.239× | 1.58% | PASS |
| mem_reads | `oltp_range_select` | 10.47ms | 13.25ms | 1.265× | 2.36% | PASS |
| mem_reads | `oltp_sum_range` | 9.63ms | 12.31ms | 1.279× | 1.83% | PASS |
| mem_reads | `oltp_order_range` | 2.53ms | 2.99ms | 1.182× | 1.17% | PASS |
| mem_reads | `oltp_distinct_range` | 3.67ms | 4.04ms | 1.101× | 1.34% | PASS |
| mem_reads | `oltp_index_scan` | 3.93ms | 5.30ms | 1.351× | 1.24% | PASS |
| mem_reads | `select_random_points` | 10.14ms | 11.18ms | 1.103× | 2.69% | PASS |
| mem_reads | `select_random_ranges` | 2.94ms | 3.94ms | 1.340× | 1.17% | PASS |
| mem_reads | `covering_index_scan` | 4.24ms | 4.14ms | 0.977× | 1.69% | PASS |
| mem_reads | `groupby_scan` | 29.86ms | 33.52ms | 1.122× | 0.82% | PASS |
| mem_reads | `index_join` | 6.02ms | 8.03ms | 1.334× | 0.93% | PASS |
| mem_reads | `index_join_scan` | 3.51ms | 4.50ms | 1.281× | 2.28% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.33s | 1.286× | 0.62% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.39s | 1.195× | 0.65% | PASS |
| mem_reads | `oltp_read_only` | 103.86ms | 123.78ms | 1.192× | 1.45% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.27ms | 251.77ms | 1.389× | 1.21% | PASS |
| mem_writes | `oltp_insert` | 15.51ms | 28.41ms | 1.832× | 0.97% | PASS |
| mem_writes | `oltp_update_index` | 50.68ms | 103.69ms | 2.046× | 1.55% | PASS |
| mem_writes | `oltp_update_non_index` | 34.51ms | 59.59ms | 1.727× | 1.98% | PASS |
| mem_writes | `oltp_delete_insert` | 45.23ms | 78.20ms | 1.729× | 1.03% | PASS |
| mem_writes | `oltp_write_only` | 22.11ms | 44.61ms | 2.017× | 0.84% | PASS |
| mem_writes | `types_delete_insert` | 25.05ms | 40.35ms | 1.610× | 1.24% | PASS |
| mem_writes | `oltp_read_write` | 67.26ms | 110.47ms | 1.643× | 0.83% | PASS |
| file_reads | `oltp_point_select` | 98.44ms | 55.40ms | 0.563× | 0.80% | PASS |
| file_reads | `oltp_range_select` | 18.16ms | 15.93ms | 0.877× | 2.34% | PASS |
| file_reads | `oltp_sum_range` | 17.63ms | 15.09ms | 0.856× | 1.22% | PASS |
| file_reads | `oltp_order_range` | 3.44ms | 3.33ms | 0.968× | 2.09% | PASS |
| file_reads | `oltp_distinct_range` | 4.56ms | 4.40ms | 0.965× | 1.26% | PASS |
| file_reads | `oltp_index_scan` | 11.77ms | 8.23ms | 0.699× | 1.64% | PASS |
| file_reads | `select_random_points` | 18.10ms | 13.98ms | 0.772× | 2.60% | PASS |
| file_reads | `select_random_ranges` | 10.55ms | 6.60ms | 0.625× | 1.23% | PASS |
| file_reads | `covering_index_scan` | 12.13ms | 7.03ms | 0.580× | 1.56% | PASS |
| file_reads | `groupby_scan` | 30.78ms | 33.91ms | 1.102× | 0.93% | PASS |
| file_reads | `index_join` | 10.53ms | 10.06ms | 0.955× | 2.70% | PASS |
| file_reads | `index_join_scan` | 4.55ms | 4.96ms | 1.091× | 1.98% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.33s | 1.280× | 0.60% | PASS |
| file_reads | `table_scan` | 1.17s | 1.40s | 1.196× | 0.95% | PASS |
| file_reads | `oltp_read_only` | 214.07ms | 161.43ms | 0.754× | 0.88% | PASS |
| file_writes | `oltp_bulk_insert` | 195.03ms | 271.70ms | 1.393× | 1.48% | PASS |
| file_writes | `oltp_insert` | 22.18ms | 35.56ms | 1.604× | 1.61% | PASS |
| file_writes | `oltp_update_index` | 78.31ms | 127.72ms | 1.631× | 2.11% | PASS |
| file_writes | `oltp_update_non_index` | 58.63ms | 81.29ms | 1.387× | 1.85% | PASS |
| file_writes | `oltp_delete_insert` | 69.75ms | 99.91ms | 1.432× | 1.96% | PASS |
| file_writes | `oltp_write_only` | 45.35ms | 64.17ms | 1.415× | 2.34% | PASS |
| file_writes | `types_delete_insert` | 40.78ms | 53.33ms | 1.308× | 2.38% | PASS |
| file_writes | `oltp_read_write` | 92.53ms | 130.42ms | 1.410× | 1.52% | PASS |
| ac_reads | `oltp_point_select` | 48.84ms | 55.26ms | 1.131× | 1.03% | PASS |
| ac_reads | `oltp_range_select` | 13.07ms | 15.91ms | 1.217× | 1.68% | PASS |
| ac_reads | `oltp_sum_range` | 12.43ms | 15.15ms | 1.219× | 1.63% | PASS |
| ac_reads | `oltp_order_range` | 2.91ms | 3.33ms | 1.145× | 1.68% | PASS |
| ac_reads | `oltp_distinct_range` | 4.00ms | 4.36ms | 1.092× | 1.04% | PASS |
| ac_reads | `oltp_index_scan` | 6.54ms | 8.20ms | 1.254× | 1.95% | PASS |
| ac_reads | `select_random_points` | 13.17ms | 13.99ms | 1.063× | 2.03% | PASS |
| ac_reads | `select_random_ranges` | 5.50ms | 6.58ms | 1.195× | 1.34% | PASS |
| ac_reads | `covering_index_scan` | 6.91ms | 7.06ms | 1.022× | 1.88% | PASS |
| ac_reads | `groupby_scan` | 30.14ms | 33.91ms | 1.125× | 0.81% | PASS |
| ac_reads | `index_join` | 7.66ms | 10.12ms | 1.321× | 1.64% | PASS |
| ac_reads | `index_join_scan` | 3.97ms | 4.95ms | 1.245× | 1.97% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.33s | 1.273× | 1.13% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.40s | 1.187× | 0.91% | PASS |
| ac_reads | `oltp_read_only` | 140.36ms | 161.85ms | 1.153× | 1.01% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.49ms | 68.06ms | 3.026× | 10.72% | PASS |
| ac_writes | `oltp_insert_ac` | 25.08ms | 83.21ms | 3.318× | 6.04% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.92ms | 97.94ms | 3.638× | 6.65% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.43ms | 77.40ms | 3.450× | 8.02% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.52ms | 87.13ms | 3.704× | 6.08% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.70ms | 85.84ms | 3.476× | 6.40% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.55ms | 78.29ms | 3.472× | 8.40% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.83ms | 96.22ms | 3.121× | 6.92% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.21ms | 37.54ms | 1.242× | 1.98% | PASS |
| mem_reads | `oltp_range_select` | 13.45ms | 14.17ms | 1.054× | 3.29% | PASS |
| mem_reads | `oltp_sum_range` | 12.13ms | 14.22ms | 1.173× | 2.74% | PASS |
| mem_reads | `oltp_order_range` | 2.94ms | 3.17ms | 1.079× | 1.68% | PASS |
| mem_reads | `oltp_distinct_range` | 4.05ms | 4.25ms | 1.050× | 1.12% | PASS |
| mem_reads | `oltp_index_scan` | 4.52ms | 6.42ms | 1.421× | 2.00% | PASS |
| mem_reads | `select_random_points` | 17.95ms | 20.66ms | 1.151× | 2.90% | PASS |
| mem_reads | `select_random_ranges` | 3.94ms | 5.17ms | 1.313× | 1.71% | PASS |
| mem_reads | `covering_index_scan` | 4.53ms | 4.53ms | 1.000× | 2.05% | PASS |
| mem_reads | `groupby_scan` | 32.06ms | 34.37ms | 1.072× | 1.10% | PASS |
| mem_reads | `index_join` | 6.89ms | 9.29ms | 1.349× | 2.40% | PASS |
| mem_reads | `index_join_scan` | 4.64ms | 5.37ms | 1.156× | 4.49% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.23s | 1.162× | 1.47% | PASS |
| mem_reads | `table_scan` | 1.25s | 1.37s | 1.096× | 1.61% | PASS |
| mem_reads | `oltp_read_only` | 119.05ms | 137.85ms | 1.158× | 1.57% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.07ms | 371.74ms | 1.575× | 0.73% | PASS |
| mem_writes | `oltp_insert` | 21.88ms | 42.29ms | 1.933× | 1.03% | PASS |
| mem_writes | `oltp_update_index` | 72.48ms | 143.69ms | 1.983× | 1.31% | PASS |
| mem_writes | `oltp_update_non_index` | 49.81ms | 93.84ms | 1.884× | 2.04% | PASS |
| mem_writes | `oltp_delete_insert` | 51.58ms | 111.31ms | 2.158× | 1.65% | PASS |
| mem_writes | `oltp_write_only` | 29.76ms | 67.95ms | 2.283× | 1.27% | PASS |
| mem_writes | `types_delete_insert` | 33.93ms | 61.33ms | 1.808× | 1.32% | PASS |
| mem_writes | `oltp_read_write` | 86.21ms | 147.07ms | 1.706× | 1.78% | PASS |
| file_reads | `oltp_point_select` | 100.86ms | 62.31ms | 0.618× | 1.16% | PASS |
| file_reads | `oltp_range_select` | 21.27ms | 17.00ms | 0.799× | 2.80% | PASS |
| file_reads | `oltp_sum_range` | 19.89ms | 17.08ms | 0.858× | 2.44% | PASS |
| file_reads | `oltp_order_range` | 3.79ms | 3.50ms | 0.922× | 2.11% | PASS |
| file_reads | `oltp_distinct_range` | 4.90ms | 4.61ms | 0.940× | 1.31% | PASS |
| file_reads | `oltp_index_scan` | 11.82ms | 9.07ms | 0.768× | 1.32% | PASS |
| file_reads | `select_random_points` | 26.92ms | 24.85ms | 0.923× | 2.23% | PASS |
| file_reads | `select_random_ranges` | 11.16ms | 7.75ms | 0.694× | 1.76% | PASS |
| file_reads | `covering_index_scan` | 11.88ms | 7.24ms | 0.610× | 1.99% | PASS |
| file_reads | `groupby_scan` | 33.02ms | 34.80ms | 1.054× | 0.76% | PASS |
| file_reads | `index_join` | 11.21ms | 11.52ms | 1.027× | 1.83% | PASS |
| file_reads | `index_join_scan` | 5.80ms | 6.01ms | 1.036× | 3.73% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.23s | 1.156× | 1.07% | PASS |
| file_reads | `table_scan` | 1.28s | 1.38s | 1.078× | 2.97% | PASS |
| file_reads | `oltp_read_only` | 228.60ms | 176.00ms | 0.770× | 0.99% | PASS |
| file_writes | `oltp_bulk_insert` | 256.28ms | 402.56ms | 1.571× | 0.80% | PASS |
| file_writes | `oltp_insert` | 62.39ms | 56.07ms | 0.899× | 26.62% | PASS |
| file_writes | `oltp_update_index` | 114.88ms | 181.98ms | 1.584× | 1.38% | PASS |
| file_writes | `oltp_update_non_index` | 97.01ms | 122.47ms | 1.262× | 14.25% | PASS |
| file_writes | `oltp_delete_insert` | 91.80ms | 143.28ms | 1.561× | 1.17% | PASS |
| file_writes | `oltp_write_only` | 83.85ms | 93.23ms | 1.112× | 10.87% | PASS |
| file_writes | `types_delete_insert` | 56.97ms | 79.63ms | 1.398× | 1.58% | PASS |
| file_writes | `oltp_read_write` | 138.74ms | 173.45ms | 1.250× | 10.20% | PASS |
| ac_reads | `oltp_point_select` | 54.39ms | 62.44ms | 1.148× | 1.41% | PASS |
| ac_reads | `oltp_range_select` | 16.83ms | 16.96ms | 1.008× | 2.03% | PASS |
| ac_reads | `oltp_sum_range` | 15.55ms | 17.14ms | 1.102× | 1.97% | PASS |
| ac_reads | `oltp_order_range` | 3.38ms | 3.50ms | 1.036× | 1.48% | PASS |
| ac_reads | `oltp_distinct_range` | 4.42ms | 4.62ms | 1.044× | 1.12% | PASS |
| ac_reads | `oltp_index_scan` | 7.34ms | 9.14ms | 1.246× | 2.15% | PASS |
| ac_reads | `select_random_points` | 21.81ms | 24.96ms | 1.144× | 2.21% | PASS |
| ac_reads | `select_random_ranges` | 6.62ms | 7.72ms | 1.165× | 1.48% | PASS |
| ac_reads | `covering_index_scan` | 7.97ms | 7.27ms | 0.912× | 4.00% | PASS |
| ac_reads | `groupby_scan` | 32.57ms | 34.83ms | 1.069× | 1.14% | PASS |
| ac_reads | `index_join` | 9.33ms | 11.57ms | 1.240× | 3.70% | PASS |
| ac_reads | `index_join_scan` | 5.42ms | 5.98ms | 1.103× | 1.79% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.23s | 1.155× | 1.01% | PASS |
| ac_reads | `table_scan` | 1.25s | 1.37s | 1.094× | 1.44% | PASS |
| ac_reads | `oltp_read_only` | 158.03ms | 175.69ms | 1.112× | 1.38% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.90ms | 64.04ms | 2.925× | 8.10% | PASS |
| ac_writes | `oltp_insert_ac` | 23.79ms | 78.93ms | 3.319× | 4.69% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.41ms | 97.68ms | 3.698× | 5.25% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.74ms | 77.35ms | 3.558× | 6.27% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.05ms | 87.21ms | 3.783× | 4.65% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.06ms | 86.90ms | 3.612× | 5.61% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.53ms | 76.08ms | 3.706× | 4.70% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.97ms | 95.82ms | 3.094× | 5.11% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.89ms | 34.93ms | 1.131× | 2.50% | PASS |
| mem_reads | `oltp_range_select` | 13.51ms | 13.52ms | 1.001× | 1.69% | PASS |
| mem_reads | `oltp_sum_range` | 12.14ms | 13.03ms | 1.072× | 1.65% | PASS |
| mem_reads | `oltp_order_range` | 3.08ms | 3.14ms | 1.021× | 1.17% | PASS |
| mem_reads | `oltp_distinct_range` | 4.19ms | 4.17ms | 0.995× | 1.24% | PASS |
| mem_reads | `oltp_index_scan` | 4.66ms | 5.97ms | 1.282× | 2.37% | PASS |
| mem_reads | `select_random_points` | 18.56ms | 20.41ms | 1.099× | 3.26% | PASS |
| mem_reads | `select_random_ranges` | 4.39ms | 5.31ms | 1.210× | 1.76% | PASS |
| mem_reads | `covering_index_scan` | 4.78ms | 4.84ms | 1.014× | 3.68% | PASS |
| mem_reads | `groupby_scan` | 34.38ms | 33.96ms | 0.988× | 0.74% | PASS |
| mem_reads | `index_join` | 7.25ms | 9.95ms | 1.372× | 4.90% | PASS |
| mem_reads | `index_join_scan` | 4.60ms | 5.87ms | 1.276× | 3.57% | PASS |
| mem_reads | `types_table_scan` | 1.13s | 1.25s | 1.102× | 1.23% | PASS |
| mem_reads | `table_scan` | 1.45s | 1.40s | 0.963× | 5.70% | PASS |
| mem_reads | `oltp_read_only` | 123.30ms | 130.70ms | 1.060× | 2.22% | PASS |
| mem_writes | `oltp_bulk_insert` | 239.03ms | 334.14ms | 1.398× | 0.82% | PASS |
| mem_writes | `oltp_insert` | 20.93ms | 39.88ms | 1.906× | 1.56% | PASS |
| mem_writes | `oltp_update_index` | 73.62ms | 137.48ms | 1.867× | 1.82% | PASS |
| mem_writes | `oltp_update_non_index` | 51.34ms | 86.78ms | 1.690× | 1.50% | PASS |
| mem_writes | `oltp_delete_insert` | 51.47ms | 106.25ms | 2.064× | 1.48% | PASS |
| mem_writes | `oltp_write_only` | 29.39ms | 64.75ms | 2.203× | 2.07% | PASS |
| mem_writes | `types_delete_insert` | 33.02ms | 54.10ms | 1.639× | 1.30% | PASS |
| mem_writes | `oltp_read_write` | 91.60ms | 142.33ms | 1.554× | 2.12% | PASS |
| file_reads | `oltp_point_select` | 128.84ms | 68.12ms | 0.529× | 1.12% | PASS |
| file_reads | `oltp_range_select` | 24.94ms | 17.08ms | 0.685× | 2.94% | PASS |
| file_reads | `oltp_sum_range` | 22.72ms | 16.86ms | 0.742× | 1.96% | PASS |
| file_reads | `oltp_order_range` | 4.21ms | 3.56ms | 0.844× | 1.52% | PASS |
| file_reads | `oltp_distinct_range` | 5.27ms | 4.55ms | 0.863× | 1.21% | PASS |
| file_reads | `oltp_index_scan` | 14.87ms | 9.59ms | 0.645× | 2.01% | PASS |
| file_reads | `select_random_points` | 29.85ms | 24.01ms | 0.805× | 3.07% | PASS |
| file_reads | `select_random_ranges` | 14.11ms | 8.56ms | 0.607× | 1.39% | PASS |
| file_reads | `covering_index_scan` | 15.16ms | 8.05ms | 0.531× | 2.79% | PASS |
| file_reads | `groupby_scan` | 35.17ms | 34.24ms | 0.974× | 1.13% | PASS |
| file_reads | `index_join` | 12.61ms | 11.73ms | 0.930× | 5.00% | PASS |
| file_reads | `index_join_scan` | 5.66ms | 6.24ms | 1.103× | 5.04% | PASS |
| file_reads | `types_table_scan` | 1.23s | 1.27s | 1.036× | 0.88% | PASS |
| file_reads | `table_scan` | 1.52s | 1.41s | 0.930× | 1.87% | PASS |
| file_reads | `oltp_read_only` | 272.99ms | 182.39ms | 0.668× | 0.86% | PASS |
| file_writes | `oltp_bulk_insert` | 262.79ms | 367.11ms | 1.397× | 0.89% | PASS |
| file_writes | `oltp_insert` | 34.31ms | 54.38ms | 1.585× | 2.14% | PASS |
| file_writes | `oltp_update_index` | 111.85ms | 172.73ms | 1.544× | 1.68% | PASS |
| file_writes | `oltp_update_non_index` | 86.71ms | 115.83ms | 1.336× | 2.60% | PASS |
| file_writes | `oltp_delete_insert` | 90.39ms | 140.62ms | 1.556× | 1.84% | PASS |
| file_writes | `oltp_write_only` | 60.51ms | 91.53ms | 1.513× | 2.02% | PASS |
| file_writes | `types_delete_insert` | 55.85ms | 75.65ms | 1.354× | 1.05% | PASS |
| file_writes | `oltp_read_write` | 124.98ms | 167.66ms | 1.341× | 1.50% | PASS |
| ac_reads | `oltp_point_select` | 64.34ms | 68.61ms | 1.066× | 1.48% | PASS |
| ac_reads | `oltp_range_select` | 18.86ms | 17.25ms | 0.915× | 2.63% | PASS |
| ac_reads | `oltp_sum_range` | 16.98ms | 16.93ms | 0.997× | 1.52% | PASS |
| ac_reads | `oltp_order_range` | 3.65ms | 3.56ms | 0.976× | 0.85% | PASS |
| ac_reads | `oltp_distinct_range` | 4.64ms | 4.55ms | 0.982× | 0.75% | PASS |
| ac_reads | `oltp_index_scan` | 8.53ms | 9.53ms | 1.117× | 1.33% | PASS |
| ac_reads | `select_random_points` | 23.03ms | 23.94ms | 1.039× | 2.20% | PASS |
| ac_reads | `select_random_ranges` | 7.68ms | 8.54ms | 1.111× | 1.26% | PASS |
| ac_reads | `covering_index_scan` | 9.08ms | 8.12ms | 0.894× | 1.60% | PASS |
| ac_reads | `groupby_scan` | 34.58ms | 34.35ms | 0.993× | 0.71% | PASS |
| ac_reads | `index_join` | 9.31ms | 11.48ms | 1.233× | 1.84% | PASS |
| ac_reads | `index_join_scan` | 5.30ms | 6.24ms | 1.177× | 3.14% | PASS |
| ac_reads | `types_table_scan` | 1.24s | 1.28s | 1.032× | 2.42% | PASS |
| ac_reads | `table_scan` | 1.57s | 1.42s | 0.904× | 1.28% | PASS |
| ac_reads | `oltp_read_only` | 173.44ms | 178.73ms | 1.031× | 1.28% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 18.32ms | 57.99ms | 3.166× | 8.51% | PASS |
| ac_writes | `oltp_insert_ac` | 19.93ms | 80.70ms | 4.049× | 5.61% | PASS |
| ac_writes | `oltp_update_index_ac` | 22.66ms | 93.81ms | 4.140× | 5.77% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.86ms | 70.82ms | 3.965× | 9.14% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 19.78ms | 81.99ms | 4.146× | 5.08% | PASS |
| ac_writes | `oltp_write_only_ac` | 20.49ms | 83.37ms | 4.070× | 4.84% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.36ms | 72.47ms | 4.175× | 6.50% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.78ms | 92.28ms | 3.445× | 5.72% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 21.50ms | 24.75ms | 1.151× | 1.93% | PASS |
| mem_reads | `oltp_range_select` | 12.48ms | 12.22ms | 0.979× | 3.21% | PASS |
| mem_reads | `oltp_sum_range` | 11.01ms | 11.80ms | 1.073× | 2.89% | PASS |
| mem_reads | `oltp_order_range` | 2.32ms | 2.23ms | 0.965× | 2.21% | PASS |
| mem_reads | `oltp_distinct_range` | 2.92ms | 2.79ms | 0.956× | 1.49% | PASS |
| mem_reads | `oltp_index_scan` | 3.23ms | 4.54ms | 1.405× | 2.39% | PASS |
| mem_reads | `select_random_points` | 19.88ms | 22.12ms | 1.113× | 2.65% | PASS |
| mem_reads | `select_random_ranges` | 4.91ms | 5.39ms | 1.097× | 1.47% | PASS |
| mem_reads | `covering_index_scan` | 2.40ms | 2.71ms | 1.130× | 4.88% | PASS |
| mem_reads | `groupby_scan` | 20.93ms | 21.43ms | 1.024× | 1.79% | PASS |
| mem_reads | `index_join` | 5.07ms | 7.09ms | 1.400× | 6.29% | PASS |
| mem_reads | `index_join_scan` | 2.92ms | 4.34ms | 1.490× | 3.30% | PASS |
| mem_reads | `types_table_scan` | 715.30ms | 761.58ms | 1.065× | 2.84% | PASS |
| mem_reads | `table_scan` | 1.05s | 902.05ms | 0.861× | 2.46% | PASS |
| mem_reads | `oltp_read_only` | 87.25ms | 91.23ms | 1.046× | 2.11% | PASS |
| mem_writes | `oltp_bulk_insert` | 148.28ms | 207.92ms | 1.402× | 1.25% | PASS |
| mem_writes | `oltp_insert` | 11.47ms | 22.54ms | 1.964× | 1.63% | PASS |
| mem_writes | `oltp_update_index` | 44.15ms | 82.04ms | 1.858× | 3.37% | PASS |
| mem_writes | `oltp_update_non_index` | 34.73ms | 58.20ms | 1.676× | 2.06% | PASS |
| mem_writes | `oltp_delete_insert` | 32.72ms | 66.64ms | 2.037× | 2.01% | PASS |
| mem_writes | `oltp_write_only` | 18.35ms | 41.31ms | 2.251× | 1.89% | PASS |
| mem_writes | `types_delete_insert` | 22.08ms | 37.90ms | 1.717× | 2.19% | PASS |
| mem_writes | `oltp_read_write` | 63.21ms | 94.52ms | 1.495× | 3.53% | PASS |
| file_reads | `oltp_point_select` | 81.25ms | 45.84ms | 0.564× | 2.00% | PASS |
| file_reads | `oltp_range_select` | 18.34ms | 14.02ms | 0.764× | 2.13% | PASS |
| file_reads | `oltp_sum_range` | 16.92ms | 13.63ms | 0.806× | 1.70% | PASS |
| file_reads | `oltp_order_range` | 2.98ms | 2.48ms | 0.835× | 1.47% | PASS |
| file_reads | `oltp_distinct_range` | 3.53ms | 3.00ms | 0.849× | 0.88% | PASS |
| file_reads | `oltp_index_scan` | 9.50ms | 6.44ms | 0.678× | 1.83% | PASS |
| file_reads | `select_random_points` | 25.43ms | 22.46ms | 0.883× | 2.30% | PASS |
| file_reads | `select_random_ranges` | 10.76ms | 7.34ms | 0.682× | 1.15% | PASS |
| file_reads | `covering_index_scan` | 8.76ms | 5.15ms | 0.588× | 1.26% | PASS |
| file_reads | `groupby_scan` | 22.34ms | 22.27ms | 0.996× | 1.11% | PASS |
| file_reads | `index_join` | 8.47ms | 8.23ms | 0.972× | 3.24% | PASS |
| file_reads | `index_join_scan` | 3.61ms | 4.34ms | 1.204× | 2.52% | PASS |
| file_reads | `types_table_scan` | 709.88ms | 766.19ms | 1.079× | 1.52% | PASS |
| file_reads | `table_scan` | 829.56ms | 857.53ms | 1.034× | 3.73% | PASS |
| file_reads | `oltp_read_only` | 166.20ms | 116.25ms | 0.699× | 1.69% | PASS |
| file_writes | `oltp_bulk_insert` | 193.39ms | 255.03ms | 1.319× | 2.15% | PASS |
| file_writes | `oltp_insert` | 19.62ms | 38.96ms | 1.985× | 7.45% | PASS |
| file_writes | `oltp_update_index` | 141.64ms | 140.44ms | 0.992× | 2.10% | PASS |
| file_writes | `oltp_update_non_index` | 121.19ms | 110.30ms | 0.910× | 3.28% | PASS |
| file_writes | `oltp_delete_insert` | 112.93ms | 120.50ms | 1.067× | 3.10% | PASS |
| file_writes | `oltp_write_only` | 86.06ms | 93.31ms | 1.084× | 4.87% | PASS |
| file_writes | `types_delete_insert` | 78.48ms | 69.30ms | 0.883× | 9.87% | PASS |
| file_writes | `oltp_read_write` | 132.36ms | 140.20ms | 1.059× | 4.80% | PASS |
| ac_reads | `oltp_point_select` | 40.37ms | 43.99ms | 1.090× | 1.69% | PASS |
| ac_reads | `oltp_range_select` | 13.91ms | 13.84ms | 0.995× | 1.86% | PASS |
| ac_reads | `oltp_sum_range` | 13.32ms | 13.70ms | 1.029× | 1.99% | PASS |
| ac_reads | `oltp_order_range` | 2.69ms | 2.53ms | 0.938× | 1.07% | PASS |
| ac_reads | `oltp_distinct_range` | 3.21ms | 3.04ms | 0.947× | 1.02% | PASS |
| ac_reads | `oltp_index_scan` | 5.50ms | 6.45ms | 1.171× | 2.75% | PASS |
| ac_reads | `select_random_points` | 20.06ms | 21.60ms | 1.077× | 3.99% | PASS |
| ac_reads | `select_random_ranges` | 6.87ms | 7.36ms | 1.071× | 1.43% | PASS |
| ac_reads | `covering_index_scan` | 4.67ms | 5.02ms | 1.076× | 2.33% | PASS |
| ac_reads | `groupby_scan` | 21.47ms | 22.01ms | 1.025× | 1.11% | PASS |
| ac_reads | `index_join` | 6.57ms | 8.28ms | 1.260× | 2.40% | PASS |
| ac_reads | `index_join_scan` | 3.18ms | 4.34ms | 1.365× | 2.79% | PASS |
| ac_reads | `types_table_scan` | 765.18ms | 787.44ms | 1.029× | 3.62% | PASS |
| ac_reads | `table_scan` | 887.99ms | 860.97ms | 0.970× | 4.86% | PASS |
| ac_reads | `oltp_read_only` | 118.97ms | 120.47ms | 1.013× | 4.92% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.31ms | 56.16ms | 2.635× | 4.22% | PASS |
| ac_writes | `oltp_insert_ac` | 23.93ms | 70.59ms | 2.950× | 4.27% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.33ms | 80.48ms | 3.057× | 3.77% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.66ms | 62.78ms | 3.039× | 2.70% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.74ms | 72.71ms | 3.198× | 3.46% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.88ms | 72.27ms | 3.158× | 3.16% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.25ms | 63.57ms | 2.734× | 9.31% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.64ms | 76.91ms | 2.887× | 3.60% | PASS |

</details>

## Version-control latency

Wall time: 2m 23s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 84.46ms | 200.00ms | 42.2% | 0.44% | PASS |
| `status_dirty_many_tables` | 87.77ms | 200.00ms | 43.9% | 0.28% | PASS |
| `diff_regular_working_one_table` | 80.22ms | 150.00ms | 53.5% | 0.39% | PASS |
| `diff_regular_working_many_tables` | 93.45ms | 200.00ms | 46.7% | 0.43% | PASS |
| `diff_stat_working_many_tables` | 93.77ms | 200.00ms | 46.9% | 0.67% | PASS |
| `diff_schema_working_many_tables` | 93.63ms | 200.00ms | 46.8% | 0.39% | PASS |
| `branch_list_many_branches` | 24.07ms | 100.00ms | 24.1% | 1.59% | PASS |
| `branch_create_delete` | 25.97ms | 100.00ms | 26.0% | 1.18% | PASS |
| `checkout_branch_clean` | 56.41ms | 200.00ms | 28.2% | 0.79% | PASS |
| `merge_data_no_conflicts` | 30.14ms | 150.00ms | 20.1% | 1.45% | PASS |
| `merge_schema_no_conflicts` | 23.09ms | 100.00ms | 23.1% | 1.09% | PASS |
| `merge_data_conflicts` | 128.53ms | 250.00ms | 51.4% | 0.19% | PASS |
| `merge_data_conflicts_with_resolve` | 128.37ms | 250.00ms | 51.3% | 0.25% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
