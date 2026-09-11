# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-11 15:23 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260907.300.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/34605993841)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 2s | 9.13s | 11.83s | 1.296× | 1.51% | **PASS** |
| textpk | 69 | 55 | 1h 33m 30s | 10.23s | 11.98s | 1.171× | 2.07% | **PASS** |
| blobpk | 69 | 55 | 1h 36m 24s | 10.23s | 11.98s | 1.172× | 2.59% | **PASS** |
| compositepk | 69 | 55 | 1h 15m 37s | 9.07s | 10.47s | 1.154× | 1.36% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.30ms | 29.48ms | 1.214× | 1.68% | PASS |
| mem_reads | `oltp_range_select` | 10.70ms | 13.22ms | 1.236× | 1.78% | PASS |
| mem_reads | `oltp_sum_range` | 10.06ms | 12.41ms | 1.234× | 2.22% | PASS |
| mem_reads | `oltp_order_range` | 2.61ms | 3.01ms | 1.153× | 1.20% | PASS |
| mem_reads | `oltp_distinct_range` | 3.69ms | 4.06ms | 1.099× | 0.90% | PASS |
| mem_reads | `oltp_index_scan` | 3.92ms | 5.39ms | 1.375× | 1.43% | PASS |
| mem_reads | `select_random_points` | 10.49ms | 11.40ms | 1.087× | 3.24% | PASS |
| mem_reads | `select_random_ranges` | 3.03ms | 4.01ms | 1.326× | 1.57% | PASS |
| mem_reads | `covering_index_scan` | 4.20ms | 4.25ms | 1.010× | 1.29% | PASS |
| mem_reads | `groupby_scan` | 30.00ms | 32.78ms | 1.093× | 0.90% | PASS |
| mem_reads | `index_join` | 6.06ms | 8.37ms | 1.380× | 1.74% | PASS |
| mem_reads | `index_join_scan` | 3.60ms | 4.75ms | 1.320× | 2.80% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.35s | 1.265× | 1.03% | PASS |
| mem_reads | `table_scan` | 1.23s | 1.42s | 1.159× | 0.61% | PASS |
| mem_reads | `oltp_read_only` | 104.33ms | 123.89ms | 1.188× | 1.28% | PASS |
| mem_writes | `oltp_bulk_insert` | 176.68ms | 250.56ms | 1.418× | 1.43% | PASS |
| mem_writes | `oltp_insert` | 15.39ms | 28.62ms | 1.860× | 1.08% | PASS |
| mem_writes | `oltp_update_index` | 51.31ms | 107.48ms | 2.095× | 2.00% | PASS |
| mem_writes | `oltp_update_non_index` | 34.55ms | 60.42ms | 1.749× | 1.85% | PASS |
| mem_writes | `oltp_delete_insert` | 46.28ms | 80.51ms | 1.740× | 1.35% | PASS |
| mem_writes | `oltp_write_only` | 22.30ms | 46.14ms | 2.069× | 1.34% | PASS |
| mem_writes | `types_delete_insert` | 24.74ms | 41.25ms | 1.667× | 1.75% | PASS |
| mem_writes | `oltp_read_write` | 69.35ms | 111.67ms | 1.610× | 1.66% | PASS |
| file_reads | `oltp_point_select` | 97.47ms | 54.69ms | 0.561× | 0.99% | PASS |
| file_reads | `oltp_range_select` | 18.34ms | 15.90ms | 0.867× | 1.50% | PASS |
| file_reads | `oltp_sum_range` | 17.68ms | 15.11ms | 0.855× | 1.31% | PASS |
| file_reads | `oltp_order_range` | 3.63ms | 3.43ms | 0.944× | 2.09% | PASS |
| file_reads | `oltp_distinct_range` | 4.69ms | 4.51ms | 0.963× | 1.93% | PASS |
| file_reads | `oltp_index_scan` | 11.68ms | 8.25ms | 0.706× | 1.01% | PASS |
| file_reads | `select_random_points` | 18.14ms | 14.06ms | 0.776× | 1.27% | PASS |
| file_reads | `select_random_ranges` | 10.43ms | 6.56ms | 0.629× | 0.72% | PASS |
| file_reads | `covering_index_scan` | 12.14ms | 7.05ms | 0.581× | 0.99% | PASS |
| file_reads | `groupby_scan` | 31.24ms | 33.36ms | 1.068× | 1.09% | PASS |
| file_reads | `index_join` | 10.54ms | 10.33ms | 0.980× | 1.67% | PASS |
| file_reads | `index_join_scan` | 4.52ms | 5.11ms | 1.131× | 1.47% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.34s | 1.268× | 0.80% | PASS |
| file_reads | `table_scan` | 1.22s | 1.42s | 1.156× | 0.75% | PASS |
| file_reads | `oltp_read_only` | 213.12ms | 161.54ms | 0.758× | 0.64% | PASS |
| file_writes | `oltp_bulk_insert` | 191.09ms | 268.92ms | 1.407× | 1.26% | PASS |
| file_writes | `oltp_insert` | 25.29ms | 36.05ms | 1.425× | 2.19% | PASS |
| file_writes | `oltp_update_index` | 79.68ms | 129.54ms | 1.626× | 1.45% | PASS |
| file_writes | `oltp_update_non_index` | 61.65ms | 81.59ms | 1.323× | 1.03% | PASS |
| file_writes | `oltp_delete_insert` | 74.44ms | 100.50ms | 1.350× | 2.37% | PASS |
| file_writes | `oltp_write_only` | 51.49ms | 64.88ms | 1.260× | 2.37% | PASS |
| file_writes | `types_delete_insert` | 41.75ms | 53.83ms | 1.289× | 1.64% | PASS |
| file_writes | `oltp_read_write` | 99.02ms | 131.10ms | 1.324× | 1.51% | PASS |
| ac_reads | `oltp_point_select` | 47.81ms | 54.69ms | 1.144× | 1.18% | PASS |
| ac_reads | `oltp_range_select` | 13.38ms | 15.91ms | 1.189× | 1.72% | PASS |
| ac_reads | `oltp_sum_range` | 12.52ms | 15.10ms | 1.206× | 1.74% | PASS |
| ac_reads | `oltp_order_range` | 3.00ms | 3.38ms | 1.126× | 1.73% | PASS |
| ac_reads | `oltp_distinct_range` | 4.03ms | 4.44ms | 1.102× | 1.62% | PASS |
| ac_reads | `oltp_index_scan` | 6.61ms | 8.24ms | 1.246× | 1.31% | PASS |
| ac_reads | `select_random_points` | 13.27ms | 14.05ms | 1.059× | 1.83% | PASS |
| ac_reads | `select_random_ranges` | 5.50ms | 6.54ms | 1.189× | 1.16% | PASS |
| ac_reads | `covering_index_scan` | 6.96ms | 7.01ms | 1.007× | 1.51% | PASS |
| ac_reads | `groupby_scan` | 30.38ms | 33.27ms | 1.095× | 1.08% | PASS |
| ac_reads | `index_join` | 7.73ms | 10.22ms | 1.322× | 1.63% | PASS |
| ac_reads | `index_join_scan` | 3.97ms | 5.09ms | 1.283× | 1.71% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.34s | 1.276× | 1.00% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.41s | 1.170× | 0.93% | PASS |
| ac_reads | `oltp_read_only` | 141.91ms | 161.81ms | 1.140× | 1.32% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 31.46ms | 109.02ms | 3.466× | 10.30% | PASS |
| ac_writes | `oltp_insert_ac` | 33.11ms | 127.48ms | 3.850× | 10.64% | PASS |
| ac_writes | `oltp_update_index_ac` | 34.42ms | 136.24ms | 3.958× | 6.99% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 31.58ms | 119.88ms | 3.796× | 10.11% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 33.57ms | 135.07ms | 4.023× | 8.85% | PASS |
| ac_writes | `oltp_write_only_ac` | 34.03ms | 133.53ms | 3.924× | 6.93% | PASS |
| ac_writes | `types_delete_insert_ac` | 32.63ms | 124.59ms | 3.818× | 8.31% | PASS |
| ac_writes | `oltp_read_write_ac` | 38.78ms | 138.85ms | 3.580× | 7.48% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.62ms | 38.07ms | 1.243× | 2.23% | PASS |
| mem_reads | `oltp_range_select` | 13.75ms | 14.32ms | 1.041× | 2.19% | PASS |
| mem_reads | `oltp_sum_range` | 12.71ms | 14.52ms | 1.143× | 2.06% | PASS |
| mem_reads | `oltp_order_range` | 3.06ms | 3.19ms | 1.042× | 1.04% | PASS |
| mem_reads | `oltp_distinct_range` | 4.05ms | 4.26ms | 1.050× | 0.92% | PASS |
| mem_reads | `oltp_index_scan` | 4.59ms | 6.50ms | 1.415× | 1.79% | PASS |
| mem_reads | `select_random_points` | 18.04ms | 21.27ms | 1.179× | 2.26% | PASS |
| mem_reads | `select_random_ranges` | 4.10ms | 5.24ms | 1.277× | 1.58% | PASS |
| mem_reads | `covering_index_scan` | 4.75ms | 4.69ms | 0.987× | 3.66% | PASS |
| mem_reads | `groupby_scan` | 32.29ms | 33.87ms | 1.049× | 1.12% | PASS |
| mem_reads | `index_join` | 6.98ms | 9.53ms | 1.365× | 3.91% | PASS |
| mem_reads | `index_join_scan` | 4.63ms | 5.41ms | 1.169× | 2.75% | PASS |
| mem_reads | `types_table_scan` | 1.11s | 1.25s | 1.129× | 2.68% | PASS |
| mem_reads | `table_scan` | 1.21s | 1.37s | 1.134× | 1.21% | PASS |
| mem_reads | `oltp_read_only` | 119.60ms | 137.19ms | 1.147× | 1.63% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.06ms | 356.66ms | 1.517× | 1.00% | PASS |
| mem_writes | `oltp_insert` | 21.85ms | 40.41ms | 1.850× | 0.99% | PASS |
| mem_writes | `oltp_update_index` | 72.78ms | 136.52ms | 1.876× | 1.91% | PASS |
| mem_writes | `oltp_update_non_index` | 50.35ms | 89.06ms | 1.769× | 1.29% | PASS |
| mem_writes | `oltp_delete_insert` | 53.90ms | 108.61ms | 2.015× | 2.95% | PASS |
| mem_writes | `oltp_write_only` | 31.04ms | 64.97ms | 2.093× | 1.43% | PASS |
| mem_writes | `types_delete_insert` | 33.64ms | 56.75ms | 1.687× | 1.14% | PASS |
| mem_writes | `oltp_read_write` | 92.47ms | 144.36ms | 1.561× | 1.86% | PASS |
| file_reads | `oltp_point_select` | 106.93ms | 64.94ms | 0.607× | 1.09% | PASS |
| file_reads | `oltp_range_select` | 22.82ms | 17.23ms | 0.755× | 2.08% | PASS |
| file_reads | `oltp_sum_range` | 21.50ms | 17.62ms | 0.820× | 1.50% | PASS |
| file_reads | `oltp_order_range` | 3.93ms | 3.51ms | 0.893× | 2.98% | PASS |
| file_reads | `oltp_distinct_range` | 4.85ms | 4.60ms | 0.948× | 0.92% | PASS |
| file_reads | `oltp_index_scan` | 12.29ms | 9.34ms | 0.760× | 1.37% | PASS |
| file_reads | `select_random_points` | 26.12ms | 24.84ms | 0.951× | 1.80% | PASS |
| file_reads | `select_random_ranges` | 11.36ms | 7.91ms | 0.696× | 0.99% | PASS |
| file_reads | `covering_index_scan` | 12.60ms | 7.47ms | 0.593× | 2.58% | PASS |
| file_reads | `groupby_scan` | 32.93ms | 34.36ms | 1.043× | 1.18% | PASS |
| file_reads | `index_join` | 11.48ms | 11.51ms | 1.002× | 3.02% | PASS |
| file_reads | `index_join_scan` | 5.80ms | 6.00ms | 1.035× | 2.23% | PASS |
| file_reads | `types_table_scan` | 1.21s | 1.26s | 1.042× | 2.43% | PASS |
| file_reads | `table_scan` | 1.49s | 1.42s | 0.954× | 2.07% | PASS |
| file_reads | `oltp_read_only` | 245.46ms | 184.35ms | 0.751× | 1.04% | PASS |
| file_writes | `oltp_bulk_insert` | 255.86ms | 393.33ms | 1.537× | 1.61% | PASS |
| file_writes | `oltp_insert` | 55.02ms | 54.14ms | 0.984× | 24.54% | PASS |
| file_writes | `oltp_update_index` | 116.37ms | 172.33ms | 1.481× | 1.75% | PASS |
| file_writes | `oltp_update_non_index` | 94.91ms | 113.93ms | 1.200× | 10.76% | PASS |
| file_writes | `oltp_delete_insert` | 93.35ms | 136.81ms | 1.465× | 1.30% | PASS |
| file_writes | `oltp_write_only` | 91.96ms | 86.88ms | 0.945× | 10.23% | PASS |
| file_writes | `types_delete_insert` | 55.88ms | 75.42ms | 1.350× | 2.05% | PASS |
| file_writes | `oltp_read_write` | 140.17ms | 166.74ms | 1.190× | 6.95% | PASS |
| ac_reads | `oltp_point_select` | 55.97ms | 64.38ms | 1.150× | 1.20% | PASS |
| ac_reads | `oltp_range_select` | 16.98ms | 17.10ms | 1.007× | 2.40% | PASS |
| ac_reads | `oltp_sum_range` | 15.50ms | 17.21ms | 1.110× | 2.34% | PASS |
| ac_reads | `oltp_order_range` | 3.35ms | 3.51ms | 1.047× | 1.91% | PASS |
| ac_reads | `oltp_distinct_range` | 4.36ms | 4.60ms | 1.055× | 2.14% | PASS |
| ac_reads | `oltp_index_scan` | 7.72ms | 9.35ms | 1.211× | 1.59% | PASS |
| ac_reads | `select_random_points` | 22.26ms | 24.95ms | 1.120× | 1.96% | PASS |
| ac_reads | `select_random_ranges` | 6.84ms | 7.90ms | 1.156× | 1.08% | PASS |
| ac_reads | `covering_index_scan` | 8.31ms | 7.43ms | 0.893× | 4.04% | PASS |
| ac_reads | `groupby_scan` | 32.60ms | 34.38ms | 1.055× | 1.05% | PASS |
| ac_reads | `index_join` | 9.39ms | 11.45ms | 1.219× | 2.79% | PASS |
| ac_reads | `index_join_scan` | 5.42ms | 5.87ms | 1.084× | 3.73% | PASS |
| ac_reads | `types_table_scan` | 1.13s | 1.25s | 1.109× | 3.67% | PASS |
| ac_reads | `table_scan` | 1.27s | 1.38s | 1.089× | 2.82% | PASS |
| ac_reads | `oltp_read_only` | 161.32ms | 179.19ms | 1.111× | 1.69% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.19ms | 78.75ms | 3.549× | 6.60% | PASS |
| ac_writes | `oltp_insert_ac` | 24.47ms | 92.25ms | 3.770× | 3.90% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.41ms | 110.56ms | 4.187× | 4.34% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.79ms | 89.95ms | 4.129× | 4.63% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.86ms | 101.20ms | 4.242× | 3.14% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.29ms | 99.90ms | 4.113× | 5.10% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.89ms | 90.79ms | 4.347× | 3.15% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.09ms | 106.95ms | 3.440× | 4.25% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.60ms | 27.29ms | 1.156× | 1.56% | PASS |
| mem_reads | `oltp_range_select` | 10.73ms | 10.83ms | 1.010× | 2.03% | PASS |
| mem_reads | `oltp_sum_range` | 10.42ms | 10.56ms | 1.014× | 3.59% | PASS |
| mem_reads | `oltp_order_range` | 2.47ms | 2.53ms | 1.025× | 2.82% | PASS |
| mem_reads | `oltp_distinct_range` | 3.39ms | 3.36ms | 0.991× | 1.27% | PASS |
| mem_reads | `oltp_index_scan` | 3.83ms | 5.00ms | 1.304× | 2.59% | PASS |
| mem_reads | `select_random_points` | 15.68ms | 16.45ms | 1.049× | 4.17% | PASS |
| mem_reads | `select_random_ranges` | 3.68ms | 4.29ms | 1.166× | 2.13% | PASS |
| mem_reads | `covering_index_scan` | 3.89ms | 3.98ms | 1.022× | 3.01% | PASS |
| mem_reads | `groupby_scan` | 27.14ms | 27.84ms | 1.026× | 1.06% | PASS |
| mem_reads | `index_join` | 5.57ms | 7.17ms | 1.288× | 3.27% | PASS |
| mem_reads | `index_join_scan` | 3.82ms | 4.72ms | 1.235× | 3.63% | PASS |
| mem_reads | `types_table_scan` | 889.79ms | 978.65ms | 1.100× | 1.31% | PASS |
| mem_reads | `table_scan` | 1.08s | 1.08s | 0.995× | 3.47% | PASS |
| mem_reads | `oltp_read_only` | 97.03ms | 100.91ms | 1.040× | 2.97% | PASS |
| mem_writes | `oltp_bulk_insert` | 183.63ms | 257.62ms | 1.403× | 0.95% | PASS |
| mem_writes | `oltp_insert` | 16.39ms | 30.21ms | 1.843× | 2.02% | PASS |
| mem_writes | `oltp_update_index` | 60.69ms | 107.05ms | 1.764× | 3.76% | PASS |
| mem_writes | `oltp_update_non_index` | 40.70ms | 65.53ms | 1.610× | 2.02% | PASS |
| mem_writes | `oltp_delete_insert` | 40.51ms | 79.83ms | 1.971× | 1.65% | PASS |
| mem_writes | `oltp_write_only` | 23.84ms | 48.86ms | 2.050× | 2.05% | PASS |
| mem_writes | `types_delete_insert` | 27.72ms | 42.02ms | 1.516× | 2.51% | PASS |
| mem_writes | `oltp_read_write` | 79.02ms | 112.19ms | 1.420× | 1.79% | PASS |
| file_reads | `oltp_point_select` | 101.01ms | 53.83ms | 0.533× | 0.85% | PASS |
| file_reads | `oltp_range_select` | 20.36ms | 13.65ms | 0.670× | 2.57% | PASS |
| file_reads | `oltp_sum_range` | 18.79ms | 13.32ms | 0.709× | 2.56% | PASS |
| file_reads | `oltp_order_range` | 3.42ms | 2.85ms | 0.831× | 1.36% | PASS |
| file_reads | `oltp_distinct_range` | 4.24ms | 3.64ms | 0.860× | 1.31% | PASS |
| file_reads | `oltp_index_scan` | 11.70ms | 7.61ms | 0.651× | 0.64% | PASS |
| file_reads | `select_random_points` | 23.10ms | 18.96ms | 0.821× | 2.97% | PASS |
| file_reads | `select_random_ranges` | 11.26ms | 6.78ms | 0.602× | 0.95% | PASS |
| file_reads | `covering_index_scan` | 12.25ms | 6.40ms | 0.522× | 1.10% | PASS |
| file_reads | `groupby_scan` | 28.11ms | 28.24ms | 1.005× | 1.17% | PASS |
| file_reads | `index_join` | 10.41ms | 9.50ms | 0.912× | 1.90% | PASS |
| file_reads | `index_join_scan` | 4.78ms | 5.41ms | 1.131× | 3.02% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.03s | 0.994× | 1.81% | PASS |
| file_reads | `table_scan` | 1.28s | 1.13s | 0.889× | 2.95% | PASS |
| file_reads | `oltp_read_only` | 214.45ms | 141.47ms | 0.660× | 1.31% | PASS |
| file_writes | `oltp_bulk_insert` | 292.34ms | 359.11ms | 1.228× | 23.17% | PASS |
| file_writes | `oltp_insert` | 60.59ms | 71.37ms | 1.178× | 51.91% | PASS |
| file_writes | `oltp_update_index` | 238.15ms | 223.49ms | 0.938× | 17.10% | PASS |
| file_writes | `oltp_update_non_index` | 211.60ms | 159.26ms | 0.753× | 26.16% | PASS |
| file_writes | `oltp_delete_insert` | 228.51ms | 184.35ms | 0.807× | 29.66% | PASS |
| file_writes | `oltp_write_only` | 242.66ms | 137.41ms | 0.566× | 34.54% | PASS |
| file_writes | `types_delete_insert` | 182.72ms | 138.74ms | 0.759× | 34.31% | PASS |
| file_writes | `oltp_read_write` | 211.52ms | 183.00ms | 0.865× | 20.48% | PASS |
| ac_reads | `oltp_point_select` | 51.00ms | 54.11ms | 1.061× | 1.91% | PASS |
| ac_reads | `oltp_range_select` | 15.23ms | 13.69ms | 0.899× | 4.17% | PASS |
| ac_reads | `oltp_sum_range` | 13.85ms | 13.38ms | 0.966× | 3.29% | PASS |
| ac_reads | `oltp_order_range` | 2.97ms | 2.84ms | 0.957× | 1.52% | PASS |
| ac_reads | `oltp_distinct_range` | 3.75ms | 3.62ms | 0.964× | 1.21% | PASS |
| ac_reads | `oltp_index_scan` | 6.89ms | 7.63ms | 1.108× | 1.54% | PASS |
| ac_reads | `select_random_points` | 18.22ms | 19.22ms | 1.055× | 2.86% | PASS |
| ac_reads | `select_random_ranges` | 6.35ms | 6.75ms | 1.063× | 1.32% | PASS |
| ac_reads | `covering_index_scan` | 7.23ms | 6.39ms | 0.884× | 1.51% | PASS |
| ac_reads | `groupby_scan` | 27.46ms | 28.20ms | 1.027× | 1.05% | PASS |
| ac_reads | `index_join` | 7.91ms | 9.38ms | 1.185× | 1.66% | PASS |
| ac_reads | `index_join_scan` | 4.28ms | 5.21ms | 1.216× | 2.84% | PASS |
| ac_reads | `types_table_scan` | 923.90ms | 995.31ms | 1.077× | 3.50% | PASS |
| ac_reads | `table_scan` | 1.22s | 1.12s | 0.920× | 4.13% | PASS |
| ac_reads | `oltp_read_only` | 138.53ms | 139.48ms | 1.007× | 1.88% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 77.43ms | 272.64ms | 3.521× | 58.83% | PASS |
| ac_writes | `oltp_insert_ac` | 88.92ms | 368.56ms | 4.145× | 68.38% | PASS |
| ac_writes | `oltp_update_index_ac` | 103.59ms | 332.84ms | 3.213× | 59.35% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 67.10ms | 250.09ms | 3.727× | 60.02% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 106.18ms | 411.63ms | 3.877× | 69.65% | PASS |
| ac_writes | `oltp_write_only_ac` | 98.34ms | 432.38ms | 4.397× | 60.65% | PASS |
| ac_writes | `types_delete_insert_ac` | 76.20ms | 271.42ms | 3.562× | 54.44% | PASS |
| ac_writes | `oltp_read_write_ac` | 64.41ms | 261.34ms | 4.057× | 42.53% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 28.53ms | 33.90ms | 1.188× | 0.92% | PASS |
| mem_reads | `oltp_range_select` | 16.42ms | 17.51ms | 1.067× | 0.80% | PASS |
| mem_reads | `oltp_sum_range` | 15.62ms | 17.17ms | 1.099× | 0.49% | PASS |
| mem_reads | `oltp_order_range` | 3.14ms | 3.33ms | 1.061× | 0.66% | PASS |
| mem_reads | `oltp_distinct_range` | 3.94ms | 4.16ms | 1.057× | 0.59% | PASS |
| mem_reads | `oltp_index_scan` | 4.03ms | 5.22ms | 1.296× | 1.77% | PASS |
| mem_reads | `select_random_points` | 24.93ms | 29.08ms | 1.167× | 0.94% | PASS |
| mem_reads | `select_random_ranges` | 6.57ms | 7.87ms | 1.198× | 0.81% | PASS |
| mem_reads | `covering_index_scan` | 3.65ms | 3.65ms | 0.998× | 1.35% | PASS |
| mem_reads | `groupby_scan` | 33.13ms | 33.82ms | 1.021× | 0.49% | PASS |
| mem_reads | `index_join` | 7.11ms | 9.56ms | 1.345× | 1.44% | PASS |
| mem_reads | `index_join_scan` | 3.43ms | 5.01ms | 1.459× | 0.78% | PASS |
| mem_reads | `types_table_scan` | 962.56ms | 1.09s | 1.132× | 0.86% | PASS |
| mem_reads | `table_scan` | 1.14s | 1.21s | 1.055× | 1.67% | PASS |
| mem_reads | `oltp_read_only` | 131.95ms | 143.53ms | 1.088× | 0.96% | PASS |
| mem_writes | `oltp_bulk_insert` | 213.93ms | 297.73ms | 1.392× | 1.07% | PASS |
| mem_writes | `oltp_insert` | 16.78ms | 31.14ms | 1.856× | 0.99% | PASS |
| mem_writes | `oltp_update_index` | 61.87ms | 109.00ms | 1.762× | 1.48% | PASS |
| mem_writes | `oltp_update_non_index` | 45.25ms | 72.25ms | 1.596× | 1.17% | PASS |
| mem_writes | `oltp_delete_insert` | 44.56ms | 85.04ms | 1.908× | 1.07% | PASS |
| mem_writes | `oltp_write_only` | 23.66ms | 50.07ms | 2.116× | 1.36% | PASS |
| mem_writes | `types_delete_insert` | 28.71ms | 47.78ms | 1.665× | 1.57% | PASS |
| mem_writes | `oltp_read_write` | 92.00ms | 135.46ms | 1.472× | 1.30% | PASS |
| file_reads | `oltp_point_select` | 65.07ms | 48.11ms | 0.739× | 1.04% | PASS |
| file_reads | `oltp_range_select` | 20.93ms | 19.45ms | 0.929× | 1.07% | PASS |
| file_reads | `oltp_sum_range` | 20.31ms | 19.10ms | 0.940× | 1.64% | PASS |
| file_reads | `oltp_order_range` | 3.66ms | 3.58ms | 0.977× | 1.71% | PASS |
| file_reads | `oltp_distinct_range` | 4.46ms | 4.42ms | 0.989× | 1.57% | PASS |
| file_reads | `oltp_index_scan` | 7.68ms | 7.05ms | 0.917× | 1.47% | PASS |
| file_reads | `select_random_points` | 30.45ms | 32.47ms | 1.066× | 1.92% | PASS |
| file_reads | `select_random_ranges` | 10.59ms | 9.58ms | 0.904× | 1.40% | PASS |
| file_reads | `covering_index_scan` | 7.18ms | 5.32ms | 0.740× | 1.13% | PASS |
| file_reads | `groupby_scan` | 34.07ms | 34.25ms | 1.005× | 0.57% | PASS |
| file_reads | `index_join` | 9.40ms | 11.15ms | 1.186× | 1.89% | PASS |
| file_reads | `index_join_scan` | 4.00ms | 5.38ms | 1.347× | 1.54% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.12s | 1.070× | 0.71% | PASS |
| file_reads | `table_scan` | 1.27s | 1.25s | 0.980× | 0.92% | PASS |
| file_reads | `oltp_read_only` | 187.70ms | 166.72ms | 0.888× | 0.62% | PASS |
| file_writes | `oltp_bulk_insert` | 226.99ms | 318.88ms | 1.405× | 1.58% | PASS |
| file_writes | `oltp_insert` | 21.74ms | 38.83ms | 1.786× | 1.29% | PASS |
| file_writes | `oltp_update_index` | 79.87ms | 127.19ms | 1.592× | 1.45% | PASS |
| file_writes | `oltp_update_non_index` | 61.14ms | 88.42ms | 1.446× | 1.70% | PASS |
| file_writes | `oltp_delete_insert` | 61.31ms | 101.72ms | 1.659× | 1.40% | PASS |
| file_writes | `oltp_write_only` | 38.11ms | 64.43ms | 1.690× | 2.63% | PASS |
| file_writes | `types_delete_insert` | 39.37ms | 57.81ms | 1.469× | 1.72% | PASS |
| file_writes | `oltp_read_write` | 106.81ms | 150.19ms | 1.406× | 1.30% | PASS |
| ac_reads | `oltp_point_select` | 40.23ms | 47.95ms | 1.192× | 1.45% | PASS |
| ac_reads | `oltp_range_select` | 18.26ms | 19.45ms | 1.065× | 1.28% | PASS |
| ac_reads | `oltp_sum_range` | 17.65ms | 19.15ms | 1.085× | 1.29% | PASS |
| ac_reads | `oltp_order_range` | 3.39ms | 3.58ms | 1.057× | 1.89% | PASS |
| ac_reads | `oltp_distinct_range` | 4.21ms | 4.47ms | 1.062× | 1.72% | PASS |
| ac_reads | `oltp_index_scan` | 5.47ms | 7.11ms | 1.300× | 1.19% | PASS |
| ac_reads | `select_random_points` | 28.02ms | 32.77ms | 1.170× | 1.43% | PASS |
| ac_reads | `select_random_ranges` | 8.02ms | 9.57ms | 1.194× | 1.27% | PASS |
| ac_reads | `covering_index_scan` | 4.91ms | 5.31ms | 1.081× | 1.26% | PASS |
| ac_reads | `groupby_scan` | 33.79ms | 34.21ms | 1.013× | 0.67% | PASS |
| ac_reads | `index_join` | 8.13ms | 11.16ms | 1.373× | 1.56% | PASS |
| ac_reads | `index_join_scan` | 3.76ms | 5.44ms | 1.445× | 1.83% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.12s | 1.072× | 1.44% | PASS |
| ac_reads | `table_scan` | 1.27s | 1.25s | 0.979× | 0.97% | PASS |
| ac_reads | `oltp_read_only` | 153.04ms | 167.33ms | 1.093× | 0.74% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.80ms | 60.79ms | 3.618× | 6.50% | PASS |
| ac_writes | `oltp_insert_ac` | 19.05ms | 76.82ms | 4.032× | 6.43% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.95ms | 86.97ms | 4.358× | 4.28% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.88ms | 70.59ms | 4.182× | 5.59% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 16.89ms | 79.95ms | 4.732× | 5.65% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.14ms | 77.55ms | 4.274× | 5.75% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.00ms | 70.06ms | 4.379× | 6.35% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.61ms | 90.49ms | 3.677× | 6.51% | PASS |

</details>

## Version-control latency

Wall time: 2m 18s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 82.06ms | 200.00ms | 41.0% | 0.46% | PASS |
| `status_dirty_many_tables` | 85.51ms | 200.00ms | 42.8% | 0.40% | PASS |
| `diff_regular_working_one_table` | 77.89ms | 150.00ms | 51.9% | 0.50% | PASS |
| `diff_regular_working_many_tables` | 90.83ms | 200.00ms | 45.4% | 0.33% | PASS |
| `diff_stat_working_many_tables` | 91.12ms | 200.00ms | 45.6% | 0.46% | PASS |
| `diff_schema_working_many_tables` | 91.81ms | 200.00ms | 45.9% | 0.57% | PASS |
| `branch_list_many_branches` | 22.53ms | 100.00ms | 22.5% | 1.11% | PASS |
| `branch_create_delete` | 25.05ms | 100.00ms | 25.0% | 1.23% | PASS |
| `checkout_branch_clean` | 55.34ms | 200.00ms | 27.7% | 1.28% | PASS |
| `merge_data_no_conflicts` | 28.63ms | 150.00ms | 19.1% | 1.12% | PASS |
| `merge_schema_no_conflicts` | 21.99ms | 100.00ms | 22.0% | 0.99% | PASS |
| `merge_data_conflicts` | 126.38ms | 250.00ms | 50.6% | 0.23% | PASS |
| `merge_data_conflicts_with_resolve` | 126.38ms | 250.00ms | 50.6% | 0.27% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
