# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-17 11:41 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260810.271.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/32018492291)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 1s | 8.87s | 11.66s | 1.315× | 1.42% | **PASS** |
| textpk | 69 | 55 | 1h 17m 55s | 8.21s | 9.63s | 1.173× | 1.70% | **PASS** |
| blobpk | 69 | 55 | 1h 32m 3s | 10.48s | 11.79s | 1.125× | 1.50% | **PASS** |
| compositepk | 69 | 55 | 1h 25m 43s | 9.93s | 11.79s | 1.187× | 1.32% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.45ms | 29.35ms | 1.252× | 1.46% | PASS |
| mem_reads | `oltp_range_select` | 10.04ms | 13.10ms | 1.305× | 1.78% | PASS |
| mem_reads | `oltp_sum_range` | 9.41ms | 12.32ms | 1.310× | 1.38% | PASS |
| mem_reads | `oltp_order_range` | 2.55ms | 2.98ms | 1.169× | 1.39% | PASS |
| mem_reads | `oltp_distinct_range` | 3.62ms | 4.06ms | 1.122× | 1.15% | PASS |
| mem_reads | `oltp_index_scan` | 3.88ms | 5.26ms | 1.358× | 2.02% | PASS |
| mem_reads | `select_random_points` | 9.81ms | 11.18ms | 1.139× | 2.66% | PASS |
| mem_reads | `select_random_ranges` | 2.92ms | 3.96ms | 1.357× | 1.44% | PASS |
| mem_reads | `covering_index_scan` | 4.17ms | 4.14ms | 0.993× | 1.46% | PASS |
| mem_reads | `groupby_scan` | 29.74ms | 32.94ms | 1.107× | 1.00% | PASS |
| mem_reads | `index_join` | 5.97ms | 8.01ms | 1.342× | 1.47% | PASS |
| mem_reads | `index_join_scan` | 3.44ms | 4.71ms | 1.368× | 1.53% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.34s | 1.290× | 0.47% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.40s | 1.192× | 0.86% | PASS |
| mem_reads | `oltp_read_only` | 101.67ms | 123.13ms | 1.211× | 1.44% | PASS |
| mem_writes | `oltp_bulk_insert` | 176.63ms | 247.98ms | 1.404× | 1.31% | PASS |
| mem_writes | `oltp_insert` | 15.28ms | 28.25ms | 1.850× | 0.92% | PASS |
| mem_writes | `oltp_update_index` | 49.40ms | 104.25ms | 2.110× | 1.04% | PASS |
| mem_writes | `oltp_update_non_index` | 33.39ms | 59.32ms | 1.776× | 1.20% | PASS |
| mem_writes | `oltp_delete_insert` | 44.31ms | 78.71ms | 1.776× | 1.37% | PASS |
| mem_writes | `oltp_write_only` | 21.52ms | 44.76ms | 2.080× | 1.16% | PASS |
| mem_writes | `types_delete_insert` | 23.90ms | 40.18ms | 1.681× | 1.17% | PASS |
| mem_writes | `oltp_read_write` | 66.08ms | 109.83ms | 1.662× | 1.31% | PASS |
| file_reads | `oltp_point_select` | 96.62ms | 54.52ms | 0.564× | 0.87% | PASS |
| file_reads | `oltp_range_select` | 17.83ms | 15.73ms | 0.882× | 1.30% | PASS |
| file_reads | `oltp_sum_range` | 17.10ms | 15.00ms | 0.877× | 1.62% | PASS |
| file_reads | `oltp_order_range` | 3.41ms | 3.30ms | 0.968× | 2.29% | PASS |
| file_reads | `oltp_distinct_range` | 4.46ms | 4.38ms | 0.983× | 1.57% | PASS |
| file_reads | `oltp_index_scan` | 11.38ms | 8.11ms | 0.712× | 2.16% | PASS |
| file_reads | `select_random_points` | 17.85ms | 13.83ms | 0.775× | 2.59% | PASS |
| file_reads | `select_random_ranges` | 10.26ms | 6.51ms | 0.634× | 1.33% | PASS |
| file_reads | `covering_index_scan` | 11.86ms | 6.98ms | 0.588× | 1.93% | PASS |
| file_reads | `groupby_scan` | 30.72ms | 33.45ms | 1.089× | 1.10% | PASS |
| file_reads | `index_join` | 10.16ms | 10.10ms | 0.995× | 1.79% | PASS |
| file_reads | `index_join_scan` | 4.46ms | 5.11ms | 1.145× | 2.00% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.33s | 1.285× | 0.67% | PASS |
| file_reads | `table_scan` | 1.17s | 1.40s | 1.192× | 0.56% | PASS |
| file_reads | `oltp_read_only` | 209.34ms | 159.99ms | 0.764× | 0.92% | PASS |
| file_writes | `oltp_bulk_insert` | 191.19ms | 266.71ms | 1.395× | 1.58% | PASS |
| file_writes | `oltp_insert` | 24.62ms | 35.53ms | 1.443× | 1.48% | PASS |
| file_writes | `oltp_update_index` | 77.71ms | 128.23ms | 1.650× | 1.47% | PASS |
| file_writes | `oltp_update_non_index` | 60.13ms | 80.55ms | 1.339× | 1.42% | PASS |
| file_writes | `oltp_delete_insert` | 70.58ms | 99.11ms | 1.404× | 1.39% | PASS |
| file_writes | `oltp_write_only` | 49.39ms | 64.17ms | 1.299× | 1.86% | PASS |
| file_writes | `types_delete_insert` | 40.55ms | 53.30ms | 1.315× | 1.64% | PASS |
| file_writes | `oltp_read_write` | 95.57ms | 129.39ms | 1.354× | 1.37% | PASS |
| ac_reads | `oltp_point_select` | 47.45ms | 54.31ms | 1.145× | 0.67% | PASS |
| ac_reads | `oltp_range_select` | 12.82ms | 15.74ms | 1.228× | 0.97% | PASS |
| ac_reads | `oltp_sum_range` | 12.25ms | 15.02ms | 1.227× | 1.10% | PASS |
| ac_reads | `oltp_order_range` | 2.92ms | 3.32ms | 1.135× | 1.57% | PASS |
| ac_reads | `oltp_distinct_range` | 3.97ms | 4.39ms | 1.107× | 1.40% | PASS |
| ac_reads | `oltp_index_scan` | 6.56ms | 8.10ms | 1.235× | 2.13% | PASS |
| ac_reads | `select_random_points` | 12.87ms | 13.95ms | 1.084× | 1.35% | PASS |
| ac_reads | `select_random_ranges` | 5.47ms | 6.54ms | 1.194× | 1.15% | PASS |
| ac_reads | `covering_index_scan` | 6.88ms | 6.96ms | 1.011× | 1.45% | PASS |
| ac_reads | `groupby_scan` | 30.05ms | 33.45ms | 1.113× | 0.92% | PASS |
| ac_reads | `index_join` | 7.61ms | 10.03ms | 1.317× | 1.30% | PASS |
| ac_reads | `index_join_scan` | 3.94ms | 5.14ms | 1.306× | 1.93% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.33s | 1.284× | 0.58% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.40s | 1.191× | 0.66% | PASS |
| ac_reads | `oltp_read_only` | 137.21ms | 160.08ms | 1.167× | 0.87% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 30.18ms | 104.69ms | 3.469× | 7.20% | PASS |
| ac_writes | `oltp_insert_ac` | 32.18ms | 123.69ms | 3.844× | 8.83% | PASS |
| ac_writes | `oltp_update_index_ac` | 33.89ms | 135.38ms | 3.995× | 11.06% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 28.63ms | 112.95ms | 3.945× | 8.26% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 31.55ms | 124.43ms | 3.943× | 7.73% | PASS |
| ac_writes | `oltp_write_only_ac` | 30.80ms | 128.48ms | 4.172× | 9.08% | PASS |
| ac_writes | `types_delete_insert_ac` | 30.25ms | 115.79ms | 3.827× | 10.41% | PASS |
| ac_writes | `oltp_read_write_ac` | 36.66ms | 128.94ms | 3.517× | 7.26% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 21.16ms | 24.50ms | 1.158× | 0.99% | PASS |
| mem_reads | `oltp_range_select` | 8.85ms | 10.47ms | 1.184× | 2.30% | PASS |
| mem_reads | `oltp_sum_range` | 7.95ms | 9.70ms | 1.220× | 1.98% | PASS |
| mem_reads | `oltp_order_range` | 2.12ms | 2.46ms | 1.161× | 1.86% | PASS |
| mem_reads | `oltp_distinct_range` | 2.95ms | 3.27ms | 1.110× | 1.64% | PASS |
| mem_reads | `oltp_index_scan` | 3.23ms | 4.06ms | 1.254× | 1.90% | PASS |
| mem_reads | `select_random_points` | 12.61ms | 14.74ms | 1.169× | 2.20% | PASS |
| mem_reads | `select_random_ranges` | 2.46ms | 3.53ms | 1.435× | 1.89% | PASS |
| mem_reads | `covering_index_scan` | 3.26ms | 3.05ms | 0.935× | 0.73% | PASS |
| mem_reads | `groupby_scan` | 25.43ms | 26.55ms | 1.044× | 1.30% | PASS |
| mem_reads | `index_join` | 5.39ms | 6.74ms | 1.250× | 1.51% | PASS |
| mem_reads | `index_join_scan` | 2.87ms | 3.57ms | 1.243× | 1.39% | PASS |
| mem_reads | `types_table_scan` | 879.54ms | 1.02s | 1.155× | 0.26% | PASS |
| mem_reads | `table_scan` | 1.03s | 1.11s | 1.077× | 0.21% | PASS |
| mem_reads | `oltp_read_only` | 88.22ms | 99.53ms | 1.128× | 0.84% | PASS |
| mem_writes | `oltp_bulk_insert` | 156.03ms | 229.55ms | 1.471× | 0.48% | PASS |
| mem_writes | `oltp_insert` | 15.27ms | 26.66ms | 1.745× | 1.44% | PASS |
| mem_writes | `oltp_update_index` | 50.55ms | 92.66ms | 1.833× | 1.46% | PASS |
| mem_writes | `oltp_update_non_index` | 31.95ms | 57.52ms | 1.800× | 1.73% | PASS |
| mem_writes | `oltp_delete_insert` | 34.96ms | 70.71ms | 2.023× | 1.87% | PASS |
| mem_writes | `oltp_write_only` | 19.63ms | 41.48ms | 2.113× | 2.42% | PASS |
| mem_writes | `types_delete_insert` | 23.32ms | 36.80ms | 1.578× | 1.50% | PASS |
| mem_writes | `oltp_read_write` | 56.81ms | 94.02ms | 1.655× | 1.28% | PASS |
| file_reads | `oltp_point_select` | 47.02ms | 33.31ms | 0.708× | 1.05% | PASS |
| file_reads | `oltp_range_select` | 11.78ms | 11.46ms | 0.973× | 2.23% | PASS |
| file_reads | `oltp_sum_range` | 10.98ms | 10.72ms | 0.976× | 1.84% | PASS |
| file_reads | `oltp_order_range` | 2.53ms | 2.58ms | 1.019× | 1.71% | PASS |
| file_reads | `oltp_distinct_range` | 3.38ms | 3.38ms | 1.001× | 1.54% | PASS |
| file_reads | `oltp_index_scan` | 5.96ms | 4.91ms | 0.824× | 1.68% | PASS |
| file_reads | `select_random_points` | 15.45ms | 15.57ms | 1.008× | 2.81% | PASS |
| file_reads | `select_random_ranges` | 5.28ms | 4.40ms | 0.833× | 1.85% | PASS |
| file_reads | `covering_index_scan` | 6.09ms | 3.96ms | 0.650× | 1.22% | PASS |
| file_reads | `groupby_scan` | 25.68ms | 26.59ms | 1.035× | 0.89% | PASS |
| file_reads | `index_join` | 6.77ms | 7.19ms | 1.063× | 1.45% | PASS |
| file_reads | `index_join_scan` | 3.27ms | 3.73ms | 1.140× | 1.74% | PASS |
| file_reads | `types_table_scan` | 883.49ms | 1.02s | 1.154× | 0.25% | PASS |
| file_reads | `table_scan` | 1.03s | 1.11s | 1.080× | 0.19% | PASS |
| file_reads | `oltp_read_only` | 125.07ms | 111.97ms | 0.895× | 0.51% | PASS |
| file_writes | `oltp_bulk_insert` | 221.83ms | 310.43ms | 1.399× | 2.78% | PASS |
| file_writes | `oltp_insert` | 55.77ms | 55.17ms | 0.989× | 6.90% | PASS |
| file_writes | `oltp_update_index` | 198.79ms | 184.12ms | 0.926× | 1.70% | PASS |
| file_writes | `oltp_update_non_index` | 143.26ms | 124.10ms | 0.866× | 1.13% | PASS |
| file_writes | `oltp_delete_insert` | 162.96ms | 145.54ms | 0.893× | 1.19% | PASS |
| file_writes | `oltp_write_only` | 120.79ms | 100.81ms | 0.835× | 1.67% | PASS |
| file_writes | `types_delete_insert` | 98.71ms | 82.94ms | 0.840× | 3.48% | PASS |
| file_writes | `oltp_read_write` | 159.81ms | 154.66ms | 0.968× | 4.43% | PASS |
| ac_reads | `oltp_point_select` | 29.08ms | 32.53ms | 1.118× | 1.15% | PASS |
| ac_reads | `oltp_range_select` | 9.89ms | 11.49ms | 1.161× | 2.10% | PASS |
| ac_reads | `oltp_sum_range` | 9.39ms | 10.83ms | 1.154× | 2.66% | PASS |
| ac_reads | `oltp_order_range` | 2.33ms | 2.60ms | 1.117× | 1.75% | PASS |
| ac_reads | `oltp_distinct_range` | 3.14ms | 3.37ms | 1.075× | 1.62% | PASS |
| ac_reads | `oltp_index_scan` | 4.14ms | 4.95ms | 1.196× | 1.74% | PASS |
| ac_reads | `select_random_points` | 14.15ms | 16.26ms | 1.149× | 1.73% | PASS |
| ac_reads | `select_random_ranges` | 3.46ms | 4.35ms | 1.255× | 1.88% | PASS |
| ac_reads | `covering_index_scan` | 4.22ms | 3.95ms | 0.935× | 1.18% | PASS |
| ac_reads | `groupby_scan` | 25.27ms | 26.44ms | 1.046× | 1.35% | PASS |
| ac_reads | `index_join` | 5.93ms | 7.29ms | 1.228× | 1.45% | PASS |
| ac_reads | `index_join_scan` | 3.88ms | 4.64ms | 1.196× | 4.98% | PASS |
| ac_reads | `types_table_scan` | 883.53ms | 1.02s | 1.152× | 0.20% | PASS |
| ac_reads | `table_scan` | 1.03s | 1.11s | 1.078× | 0.23% | PASS |
| ac_reads | `oltp_read_only` | 101.48ms | 112.62ms | 1.110× | 0.42% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 30.60ms | 82.07ms | 2.682× | 7.10% | PASS |
| ac_writes | `oltp_insert_ac` | 32.27ms | 91.59ms | 2.839× | 6.10% | PASS |
| ac_writes | `oltp_update_index_ac` | 32.67ms | 101.36ms | 3.103× | 3.92% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 30.05ms | 88.13ms | 2.933× | 5.49% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 31.64ms | 96.81ms | 3.060× | 4.61% | PASS |
| ac_writes | `oltp_write_only_ac` | 31.48ms | 97.45ms | 3.096× | 8.14% | PASS |
| ac_writes | `types_delete_insert_ac` | 29.84ms | 88.48ms | 2.965× | 11.01% | PASS |
| ac_writes | `oltp_read_write_ac` | 34.66ms | 102.82ms | 2.966× | 6.22% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.66ms | 35.81ms | 1.131× | 1.49% | PASS |
| mem_reads | `oltp_range_select` | 14.19ms | 13.83ms | 0.974× | 1.87% | PASS |
| mem_reads | `oltp_sum_range` | 12.69ms | 13.41ms | 1.057× | 1.30% | PASS |
| mem_reads | `oltp_order_range` | 3.14ms | 3.17ms | 1.010× | 1.01% | PASS |
| mem_reads | `oltp_distinct_range` | 4.22ms | 4.19ms | 0.993× | 1.19% | PASS |
| mem_reads | `oltp_index_scan` | 4.81ms | 6.24ms | 1.297× | 1.71% | PASS |
| mem_reads | `select_random_points` | 18.41ms | 20.41ms | 1.108× | 1.77% | PASS |
| mem_reads | `select_random_ranges` | 4.27ms | 5.28ms | 1.237× | 1.04% | PASS |
| mem_reads | `covering_index_scan` | 4.71ms | 4.76ms | 1.012× | 3.24% | PASS |
| mem_reads | `groupby_scan` | 34.18ms | 35.56ms | 1.040× | 0.85% | PASS |
| mem_reads | `index_join` | 7.16ms | 9.93ms | 1.386× | 1.98% | PASS |
| mem_reads | `index_join_scan` | 4.49ms | 5.84ms | 1.301× | 2.29% | PASS |
| mem_reads | `types_table_scan` | 1.16s | 1.26s | 1.084× | 3.26% | PASS |
| mem_reads | `table_scan` | 1.39s | 1.38s | 0.990× | 2.90% | PASS |
| mem_reads | `oltp_read_only` | 121.85ms | 129.89ms | 1.066× | 1.29% | PASS |
| mem_writes | `oltp_bulk_insert` | 237.44ms | 333.65ms | 1.405× | 0.76% | PASS |
| mem_writes | `oltp_insert` | 20.68ms | 39.55ms | 1.913× | 1.18% | PASS |
| mem_writes | `oltp_update_index` | 70.68ms | 134.01ms | 1.896× | 1.29% | PASS |
| mem_writes | `oltp_update_non_index` | 50.70ms | 86.22ms | 1.700× | 1.10% | PASS |
| mem_writes | `oltp_delete_insert` | 50.55ms | 105.76ms | 2.092× | 0.87% | PASS |
| mem_writes | `oltp_write_only` | 29.22ms | 65.11ms | 2.228× | 1.19% | PASS |
| mem_writes | `types_delete_insert` | 32.75ms | 53.68ms | 1.639× | 1.19% | PASS |
| mem_writes | `oltp_read_write` | 85.00ms | 137.94ms | 1.623× | 2.64% | PASS |
| file_reads | `oltp_point_select` | 127.53ms | 67.19ms | 0.527× | 0.83% | PASS |
| file_reads | `oltp_range_select` | 23.95ms | 16.90ms | 0.706× | 1.78% | PASS |
| file_reads | `oltp_sum_range` | 22.73ms | 16.67ms | 0.733× | 1.95% | PASS |
| file_reads | `oltp_order_range` | 4.20ms | 3.56ms | 0.849× | 1.36% | PASS |
| file_reads | `oltp_distinct_range` | 5.27ms | 4.57ms | 0.867× | 0.97% | PASS |
| file_reads | `oltp_index_scan` | 14.76ms | 9.51ms | 0.644× | 1.16% | PASS |
| file_reads | `select_random_points` | 28.86ms | 23.72ms | 0.822× | 1.97% | PASS |
| file_reads | `select_random_ranges` | 14.06ms | 8.50ms | 0.604× | 1.26% | PASS |
| file_reads | `covering_index_scan` | 15.28ms | 7.98ms | 0.522× | 1.29% | PASS |
| file_reads | `groupby_scan` | 34.92ms | 35.75ms | 1.024× | 1.08% | PASS |
| file_reads | `index_join` | 12.78ms | 11.65ms | 0.912× | 1.72% | PASS |
| file_reads | `index_join_scan` | 5.62ms | 6.34ms | 1.128× | 1.51% | PASS |
| file_reads | `types_table_scan` | 1.14s | 1.24s | 1.091× | 2.76% | PASS |
| file_reads | `table_scan` | 1.37s | 1.36s | 0.996× | 3.91% | PASS |
| file_reads | `oltp_read_only` | 266.43ms | 177.41ms | 0.666× | 0.99% | PASS |
| file_writes | `oltp_bulk_insert` | 260.75ms | 363.46ms | 1.394× | 1.07% | PASS |
| file_writes | `oltp_insert` | 32.38ms | 53.40ms | 1.649× | 1.95% | PASS |
| file_writes | `oltp_update_index` | 110.24ms | 171.38ms | 1.555× | 1.50% | PASS |
| file_writes | `oltp_update_non_index` | 83.15ms | 112.80ms | 1.357× | 1.46% | PASS |
| file_writes | `oltp_delete_insert` | 87.68ms | 138.50ms | 1.580× | 1.98% | PASS |
| file_writes | `oltp_write_only` | 58.78ms | 89.77ms | 1.527× | 2.13% | PASS |
| file_writes | `types_delete_insert` | 55.28ms | 75.16ms | 1.360× | 1.47% | PASS |
| file_writes | `oltp_read_write` | 118.99ms | 163.70ms | 1.376× | 2.29% | PASS |
| ac_reads | `oltp_point_select` | 63.90ms | 67.69ms | 1.059× | 1.30% | PASS |
| ac_reads | `oltp_range_select` | 17.90ms | 16.96ms | 0.947× | 1.74% | PASS |
| ac_reads | `oltp_sum_range` | 16.62ms | 16.77ms | 1.009× | 1.56% | PASS |
| ac_reads | `oltp_order_range` | 3.71ms | 3.58ms | 0.964× | 1.16% | PASS |
| ac_reads | `oltp_distinct_range` | 4.67ms | 4.58ms | 0.981× | 1.08% | PASS |
| ac_reads | `oltp_index_scan` | 8.49ms | 9.54ms | 1.124× | 0.93% | PASS |
| ac_reads | `select_random_points` | 22.89ms | 24.08ms | 1.052× | 1.95% | PASS |
| ac_reads | `select_random_ranges` | 7.85ms | 8.60ms | 1.095× | 1.23% | PASS |
| ac_reads | `covering_index_scan` | 9.06ms | 8.05ms | 0.889× | 1.26% | PASS |
| ac_reads | `groupby_scan` | 34.70ms | 35.95ms | 1.036× | 0.76% | PASS |
| ac_reads | `index_join` | 9.72ms | 11.71ms | 1.205× | 1.46% | PASS |
| ac_reads | `index_join_scan` | 5.14ms | 6.33ms | 1.231× | 1.87% | PASS |
| ac_reads | `types_table_scan` | 1.21s | 1.28s | 1.053× | 3.70% | PASS |
| ac_reads | `table_scan` | 1.46s | 1.41s | 0.963× | 5.63% | PASS |
| ac_reads | `oltp_read_only` | 167.15ms | 174.57ms | 1.044× | 1.44% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.34ms | 64.96ms | 3.974× | 4.25% | PASS |
| ac_writes | `oltp_insert_ac` | 19.11ms | 86.29ms | 4.515× | 3.76% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.43ms | 98.90ms | 4.614× | 3.85% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.12ms | 76.76ms | 4.482× | 5.57% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.48ms | 88.54ms | 4.790× | 3.33% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.65ms | 88.17ms | 4.728× | 2.46% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.28ms | 78.40ms | 4.815× | 5.14% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.13ms | 95.79ms | 3.812× | 2.35% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 28.99ms | 32.89ms | 1.135× | 1.12% | PASS |
| mem_reads | `oltp_range_select` | 18.06ms | 19.05ms | 1.054× | 1.04% | PASS |
| mem_reads | `oltp_sum_range` | 16.90ms | 17.83ms | 1.055× | 1.00% | PASS |
| mem_reads | `oltp_order_range` | 3.39ms | 3.56ms | 1.050× | 1.39% | PASS |
| mem_reads | `oltp_distinct_range` | 4.34ms | 4.51ms | 1.039× | 1.40% | PASS |
| mem_reads | `oltp_index_scan` | 4.19ms | 5.25ms | 1.252× | 1.23% | PASS |
| mem_reads | `select_random_points` | 26.46ms | 29.51ms | 1.115× | 0.98% | PASS |
| mem_reads | `select_random_ranges` | 6.55ms | 7.46ms | 1.138× | 1.33% | PASS |
| mem_reads | `covering_index_scan` | 3.51ms | 3.63ms | 1.035× | 1.46% | PASS |
| mem_reads | `groupby_scan` | 35.28ms | 36.01ms | 1.021× | 0.64% | PASS |
| mem_reads | `index_join` | 7.44ms | 9.72ms | 1.306× | 1.57% | PASS |
| mem_reads | `index_join_scan` | 3.68ms | 5.20ms | 1.415× | 1.48% | PASS |
| mem_reads | `types_table_scan` | 1.12s | 1.24s | 1.113× | 1.77% | PASS |
| mem_reads | `table_scan` | 1.31s | 1.35s | 1.033× | 1.64% | PASS |
| mem_reads | `oltp_read_only` | 137.09ms | 146.62ms | 1.070× | 0.81% | PASS |
| mem_writes | `oltp_bulk_insert` | 203.04ms | 282.05ms | 1.389× | 0.57% | PASS |
| mem_writes | `oltp_insert` | 16.66ms | 30.24ms | 1.815× | 0.81% | PASS |
| mem_writes | `oltp_update_index` | 60.50ms | 107.65ms | 1.779× | 1.14% | PASS |
| mem_writes | `oltp_update_non_index` | 43.45ms | 71.22ms | 1.639× | 0.81% | PASS |
| mem_writes | `oltp_delete_insert` | 43.25ms | 82.13ms | 1.899× | 0.86% | PASS |
| mem_writes | `oltp_write_only` | 23.20ms | 48.55ms | 2.093× | 1.00% | PASS |
| mem_writes | `types_delete_insert` | 27.22ms | 45.37ms | 1.667× | 0.89% | PASS |
| mem_writes | `oltp_read_write` | 86.71ms | 128.05ms | 1.477× | 1.02% | PASS |
| file_reads | `oltp_point_select` | 61.71ms | 44.32ms | 0.718× | 0.72% | PASS |
| file_reads | `oltp_range_select` | 21.65ms | 20.61ms | 0.952× | 0.67% | PASS |
| file_reads | `oltp_sum_range` | 20.99ms | 19.51ms | 0.930× | 1.07% | PASS |
| file_reads | `oltp_order_range` | 3.90ms | 3.79ms | 0.972× | 1.49% | PASS |
| file_reads | `oltp_distinct_range` | 4.88ms | 4.74ms | 0.971× | 1.04% | PASS |
| file_reads | `oltp_index_scan` | 7.88ms | 6.88ms | 0.872× | 1.01% | PASS |
| file_reads | `select_random_points` | 30.80ms | 31.46ms | 1.021× | 0.68% | PASS |
| file_reads | `select_random_ranges` | 10.33ms | 8.95ms | 0.866× | 0.82% | PASS |
| file_reads | `covering_index_scan` | 7.11ms | 5.19ms | 0.730× | 1.36% | PASS |
| file_reads | `groupby_scan` | 35.81ms | 36.40ms | 1.017× | 0.42% | PASS |
| file_reads | `index_join` | 9.45ms | 10.90ms | 1.154× | 1.13% | PASS |
| file_reads | `index_join_scan` | 4.10ms | 5.43ms | 1.325× | 1.70% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.22s | 1.175× | 0.53% | PASS |
| file_reads | `table_scan` | 1.28s | 1.35s | 1.057× | 2.10% | PASS |
| file_reads | `oltp_read_only` | 185.92ms | 164.62ms | 0.885× | 0.75% | PASS |
| file_writes | `oltp_bulk_insert` | 263.09ms | 352.75ms | 1.341× | 3.43% | PASS |
| file_writes | `oltp_insert` | 32.62ms | 53.84ms | 1.650× | 2.10% | PASS |
| file_writes | `oltp_update_index` | 176.69ms | 184.07ms | 1.042× | 2.62% | PASS |
| file_writes | `oltp_update_non_index` | 146.40ms | 133.76ms | 0.914× | 1.49% | PASS |
| file_writes | `oltp_delete_insert` | 145.64ms | 153.21ms | 1.052× | 1.22% | PASS |
| file_writes | `oltp_write_only` | 105.44ms | 108.81ms | 1.032× | 1.32% | PASS |
| file_writes | `types_delete_insert` | 95.46ms | 88.19ms | 0.924× | 5.14% | PASS |
| file_writes | `oltp_read_write` | 167.31ms | 186.24ms | 1.113× | 1.43% | PASS |
| ac_reads | `oltp_point_select` | 38.74ms | 44.22ms | 1.141× | 0.64% | PASS |
| ac_reads | `oltp_range_select` | 18.98ms | 20.42ms | 1.076× | 0.79% | PASS |
| ac_reads | `oltp_sum_range` | 18.01ms | 19.24ms | 1.068× | 0.73% | PASS |
| ac_reads | `oltp_order_range` | 3.62ms | 3.75ms | 1.036× | 1.52% | PASS |
| ac_reads | `oltp_distinct_range` | 4.57ms | 4.70ms | 1.027× | 1.47% | PASS |
| ac_reads | `oltp_index_scan` | 5.55ms | 6.76ms | 1.217× | 1.35% | PASS |
| ac_reads | `select_random_points` | 28.06ms | 31.14ms | 1.110× | 1.01% | PASS |
| ac_reads | `select_random_ranges` | 7.94ms | 8.87ms | 1.118× | 1.34% | PASS |
| ac_reads | `covering_index_scan` | 4.77ms | 5.16ms | 1.081× | 1.35% | PASS |
| ac_reads | `groupby_scan` | 35.41ms | 36.21ms | 1.023× | 0.90% | PASS |
| ac_reads | `index_join` | 8.24ms | 10.70ms | 1.299× | 1.38% | PASS |
| ac_reads | `index_join_scan` | 3.90ms | 5.42ms | 1.390× | 1.13% | PASS |
| ac_reads | `types_table_scan` | 1.07s | 1.24s | 1.158× | 1.35% | PASS |
| ac_reads | `table_scan` | 1.22s | 1.33s | 1.086× | 1.90% | PASS |
| ac_reads | `oltp_read_only` | 146.52ms | 160.62ms | 1.096× | 0.79% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 25.32ms | 96.03ms | 3.793× | 13.43% | PASS |
| ac_writes | `oltp_insert_ac` | 29.45ms | 120.42ms | 4.088× | 10.26% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.34ms | 126.29ms | 4.456× | 12.99% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 27.59ms | 108.58ms | 3.936× | 15.41% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 27.82ms | 119.74ms | 4.303× | 14.54% | PASS |
| ac_writes | `oltp_write_only_ac` | 31.41ms | 133.20ms | 4.241× | 18.54% | PASS |
| ac_writes | `types_delete_insert_ac` | 28.62ms | 122.94ms | 4.296× | 18.85% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.79ms | 131.64ms | 3.895× | 13.58% | PASS |

</details>

## Version-control latency

Wall time: 2m 22s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 84.16ms | 200.00ms | 42.1% | 0.49% | PASS |
| `status_dirty_many_tables` | 87.80ms | 200.00ms | 43.9% | 0.54% | PASS |
| `diff_regular_working_one_table` | 79.83ms | 150.00ms | 53.2% | 0.56% | PASS |
| `diff_regular_working_many_tables` | 92.69ms | 200.00ms | 46.3% | 0.59% | PASS |
| `diff_stat_working_many_tables` | 92.53ms | 200.00ms | 46.3% | 0.86% | PASS |
| `diff_schema_working_many_tables` | 93.29ms | 200.00ms | 46.6% | 0.66% | PASS |
| `branch_list_many_branches` | 24.14ms | 100.00ms | 24.1% | 1.51% | PASS |
| `branch_create_delete` | 25.21ms | 100.00ms | 25.2% | 1.76% | PASS |
| `checkout_branch_clean` | 56.09ms | 200.00ms | 28.0% | 1.29% | PASS |
| `merge_data_no_conflicts` | 28.94ms | 150.00ms | 19.3% | 1.64% | PASS |
| `merge_schema_no_conflicts` | 22.59ms | 100.00ms | 22.6% | 2.62% | PASS |
| `merge_data_conflicts` | 128.00ms | 250.00ms | 51.2% | 0.33% | PASS |
| `merge_data_conflicts_with_resolve` | 128.48ms | 250.00ms | 51.4% | 0.37% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
