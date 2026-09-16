# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-16 15:56 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260907.300.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/35107551954)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 59m 41s | 7.49s | 9.33s | 1.245× | 1.76% | **PASS** |
| textpk | 69 | 55 | 1h 34m 51s | 11.03s | 12.33s | 1.118× | 2.39% | **PASS** |
| blobpk | 69 | 55 | 1h 31m 4s | 9.57s | 11.79s | 1.232× | 1.53% | **PASS** |
| compositepk | 69 | 55 | 1h 29m 29s | 10.58s | 12.60s | 1.191× | 2.41% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 18.41ms | 20.66ms | 1.122× | 1.92% | PASS |
| mem_reads | `oltp_range_select` | 8.30ms | 9.74ms | 1.174× | 2.77% | PASS |
| mem_reads | `oltp_sum_range` | 7.59ms | 9.10ms | 1.198× | 2.09% | PASS |
| mem_reads | `oltp_order_range` | 2.15ms | 2.42ms | 1.126× | 1.16% | PASS |
| mem_reads | `oltp_distinct_range` | 2.92ms | 3.19ms | 1.090× | 1.54% | PASS |
| mem_reads | `oltp_index_scan` | 3.01ms | 3.69ms | 1.226× | 1.65% | PASS |
| mem_reads | `select_random_points` | 8.54ms | 9.20ms | 1.077× | 1.73% | PASS |
| mem_reads | `select_random_ranges` | 2.27ms | 2.93ms | 1.290× | 3.07% | PASS |
| mem_reads | `covering_index_scan` | 2.95ms | 3.01ms | 1.020× | 1.91% | PASS |
| mem_reads | `groupby_scan` | 25.26ms | 26.78ms | 1.060× | 2.01% | PASS |
| mem_reads | `index_join` | 4.64ms | 6.24ms | 1.344× | 1.76% | PASS |
| mem_reads | `index_join_scan` | 2.56ms | 3.68ms | 1.439× | 2.87% | PASS |
| mem_reads | `types_table_scan` | 869.81ms | 1.04s | 1.198× | 0.73% | PASS |
| mem_reads | `table_scan` | 977.02ms | 1.13s | 1.156× | 0.65% | PASS |
| mem_reads | `oltp_read_only` | 81.24ms | 93.23ms | 1.148× | 1.22% | PASS |
| mem_writes | `oltp_bulk_insert` | 123.47ms | 165.02ms | 1.337× | 1.37% | PASS |
| mem_writes | `oltp_insert` | 11.13ms | 19.31ms | 1.735× | 2.10% | PASS |
| mem_writes | `oltp_update_index` | 37.54ms | 75.39ms | 2.008× | 1.07% | PASS |
| mem_writes | `oltp_update_non_index` | 24.25ms | 41.59ms | 1.715× | 1.65% | PASS |
| mem_writes | `oltp_delete_insert` | 33.57ms | 54.56ms | 1.625× | 1.51% | PASS |
| mem_writes | `oltp_write_only` | 15.90ms | 31.73ms | 1.995× | 1.58% | PASS |
| mem_writes | `types_delete_insert` | 17.83ms | 27.85ms | 1.562× | 1.38% | PASS |
| mem_writes | `oltp_read_write` | 47.48ms | 76.00ms | 1.601× | 1.37% | PASS |
| file_reads | `oltp_point_select` | 43.79ms | 28.56ms | 0.652× | 1.45% | PASS |
| file_reads | `oltp_range_select` | 10.85ms | 10.74ms | 0.990× | 2.22% | PASS |
| file_reads | `oltp_sum_range` | 10.13ms | 10.12ms | 0.999× | 1.90% | PASS |
| file_reads | `oltp_order_range` | 2.48ms | 2.55ms | 1.028× | 1.64% | PASS |
| file_reads | `oltp_distinct_range` | 3.31ms | 3.35ms | 1.011× | 1.54% | PASS |
| file_reads | `oltp_index_scan` | 5.92ms | 4.86ms | 0.821× | 2.56% | PASS |
| file_reads | `select_random_points` | 11.72ms | 10.60ms | 0.905× | 2.24% | PASS |
| file_reads | `select_random_ranges` | 5.01ms | 3.93ms | 0.783× | 2.13% | PASS |
| file_reads | `covering_index_scan` | 5.81ms | 4.06ms | 0.699× | 2.72% | PASS |
| file_reads | `groupby_scan` | 25.78ms | 27.11ms | 1.052× | 1.89% | PASS |
| file_reads | `index_join` | 6.20ms | 6.95ms | 1.120× | 2.57% | PASS |
| file_reads | `index_join_scan` | 2.86ms | 3.81ms | 1.330× | 2.09% | PASS |
| file_reads | `types_table_scan` | 852.43ms | 1.03s | 1.205× | 0.47% | PASS |
| file_reads | `table_scan` | 970.32ms | 1.12s | 1.157× | 0.37% | PASS |
| file_reads | `oltp_read_only` | 116.53ms | 104.58ms | 0.897× | 0.82% | PASS |
| file_writes | `oltp_bulk_insert` | 160.23ms | 209.13ms | 1.305× | 5.99% | PASS |
| file_writes | `oltp_insert` | 19.84ms | 28.88ms | 1.456× | 1.03% | PASS |
| file_writes | `oltp_update_index` | 121.14ms | 124.05ms | 1.024× | 3.58% | PASS |
| file_writes | `oltp_update_non_index` | 90.45ms | 89.06ms | 0.985× | 2.12% | PASS |
| file_writes | `oltp_delete_insert` | 100.85ms | 99.51ms | 0.987× | 0.94% | PASS |
| file_writes | `oltp_write_only` | 70.73ms | 69.62ms | 0.984× | 1.27% | PASS |
| file_writes | `types_delete_insert` | 60.15ms | 51.32ms | 0.853× | 2.81% | PASS |
| file_writes | `oltp_read_write` | 110.53ms | 109.55ms | 0.991× | 0.98% | PASS |
| ac_reads | `oltp_point_select` | 26.24ms | 28.58ms | 1.089× | 1.10% | PASS |
| ac_reads | `oltp_range_select` | 9.35ms | 10.86ms | 1.162× | 1.75% | PASS |
| ac_reads | `oltp_sum_range` | 8.91ms | 10.30ms | 1.156× | 2.60% | PASS |
| ac_reads | `oltp_order_range` | 2.28ms | 2.54ms | 1.115× | 1.59% | PASS |
| ac_reads | `oltp_distinct_range` | 3.10ms | 3.34ms | 1.079× | 2.10% | PASS |
| ac_reads | `oltp_index_scan` | 4.06ms | 4.68ms | 1.153× | 1.84% | PASS |
| ac_reads | `select_random_points` | 9.97ms | 10.43ms | 1.046× | 1.30% | PASS |
| ac_reads | `select_random_ranges` | 3.29ms | 3.86ms | 1.175× | 1.76% | PASS |
| ac_reads | `covering_index_scan` | 3.97ms | 3.98ms | 1.004× | 2.22% | PASS |
| ac_reads | `groupby_scan` | 24.91ms | 26.61ms | 1.068× | 0.85% | PASS |
| ac_reads | `index_join` | 5.30ms | 6.87ms | 1.297× | 1.87% | PASS |
| ac_reads | `index_join_scan` | 2.78ms | 3.83ms | 1.378× | 1.74% | PASS |
| ac_reads | `types_table_scan` | 859.64ms | 1.03s | 1.204× | 0.51% | PASS |
| ac_reads | `table_scan` | 979.36ms | 1.13s | 1.157× | 0.72% | PASS |
| ac_reads | `oltp_read_only` | 91.06ms | 104.69ms | 1.150× | 0.92% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 39.48ms | 104.61ms | 2.649× | 5.23% | PASS |
| ac_writes | `oltp_insert_ac` | 41.07ms | 115.98ms | 2.824× | 6.75% | PASS |
| ac_writes | `oltp_update_index_ac` | 40.29ms | 126.02ms | 3.128× | 5.37% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 37.37ms | 111.19ms | 2.975× | 7.06% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 40.81ms | 117.56ms | 2.881× | 7.07% | PASS |
| ac_writes | `oltp_write_only_ac` | 40.81ms | 120.25ms | 2.947× | 8.56% | PASS |
| ac_writes | `types_delete_insert_ac` | 37.75ms | 110.90ms | 2.938× | 5.55% | PASS |
| ac_writes | `oltp_read_write_ac` | 43.23ms | 122.56ms | 2.835× | 7.48% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.66ms | 37.76ms | 1.232× | 2.20% | PASS |
| mem_reads | `oltp_range_select` | 14.48ms | 14.40ms | 0.995× | 3.29% | PASS |
| mem_reads | `oltp_sum_range` | 12.73ms | 14.52ms | 1.141× | 3.04% | PASS |
| mem_reads | `oltp_order_range` | 3.14ms | 3.21ms | 1.022× | 1.60% | PASS |
| mem_reads | `oltp_distinct_range` | 4.14ms | 4.27ms | 1.031× | 1.23% | PASS |
| mem_reads | `oltp_index_scan` | 4.77ms | 6.56ms | 1.376× | 2.46% | PASS |
| mem_reads | `select_random_points` | 18.82ms | 21.40ms | 1.137× | 3.22% | PASS |
| mem_reads | `select_random_ranges` | 4.16ms | 5.26ms | 1.264× | 1.96% | PASS |
| mem_reads | `covering_index_scan` | 4.90ms | 4.76ms | 0.972× | 4.43% | PASS |
| mem_reads | `groupby_scan` | 32.71ms | 33.99ms | 1.039× | 0.98% | PASS |
| mem_reads | `index_join` | 7.35ms | 9.76ms | 1.327× | 4.03% | PASS |
| mem_reads | `index_join_scan` | 4.88ms | 5.54ms | 1.135× | 4.43% | PASS |
| mem_reads | `types_table_scan` | 1.21s | 1.27s | 1.051× | 2.09% | PASS |
| mem_reads | `table_scan` | 1.51s | 1.42s | 0.937× | 2.61% | PASS |
| mem_reads | `oltp_read_only` | 127.55ms | 140.20ms | 1.099× | 2.15% | PASS |
| mem_writes | `oltp_bulk_insert` | 230.37ms | 356.71ms | 1.548× | 1.33% | PASS |
| mem_writes | `oltp_insert` | 22.29ms | 40.81ms | 1.831× | 2.20% | PASS |
| mem_writes | `oltp_update_index` | 75.33ms | 140.03ms | 1.859× | 2.53% | PASS |
| mem_writes | `oltp_update_non_index` | 49.39ms | 89.47ms | 1.811× | 1.54% | PASS |
| mem_writes | `oltp_delete_insert` | 53.64ms | 107.94ms | 2.012× | 2.85% | PASS |
| mem_writes | `oltp_write_only` | 30.56ms | 64.08ms | 2.097× | 2.24% | PASS |
| mem_writes | `types_delete_insert` | 34.25ms | 57.24ms | 1.671× | 1.91% | PASS |
| mem_writes | `oltp_read_write` | 93.04ms | 144.47ms | 1.553× | 2.55% | PASS |
| file_reads | `oltp_point_select` | 106.68ms | 64.25ms | 0.602× | 1.18% | PASS |
| file_reads | `oltp_range_select` | 22.74ms | 17.35ms | 0.763× | 2.32% | PASS |
| file_reads | `oltp_sum_range` | 21.14ms | 17.56ms | 0.831× | 1.91% | PASS |
| file_reads | `oltp_order_range` | 4.22ms | 3.71ms | 0.879× | 2.59% | PASS |
| file_reads | `oltp_distinct_range` | 5.19ms | 4.75ms | 0.915× | 1.61% | PASS |
| file_reads | `oltp_index_scan` | 12.75ms | 9.20ms | 0.722× | 1.99% | PASS |
| file_reads | `select_random_points` | 27.80ms | 24.74ms | 0.890× | 2.39% | PASS |
| file_reads | `select_random_ranges` | 11.85ms | 7.90ms | 0.667× | 1.34% | PASS |
| file_reads | `covering_index_scan` | 13.47ms | 7.46ms | 0.554× | 1.72% | PASS |
| file_reads | `groupby_scan` | 34.01ms | 34.94ms | 1.027× | 1.12% | PASS |
| file_reads | `index_join` | 12.34ms | 11.72ms | 0.949× | 2.52% | PASS |
| file_reads | `index_join_scan` | 5.83ms | 6.10ms | 1.047× | 3.95% | PASS |
| file_reads | `types_table_scan` | 1.19s | 1.26s | 1.059× | 2.44% | PASS |
| file_reads | `table_scan` | 1.51s | 1.42s | 0.935× | 2.93% | PASS |
| file_reads | `oltp_read_only` | 241.64ms | 179.84ms | 0.744× | 1.69% | PASS |
| file_writes | `oltp_bulk_insert` | 250.17ms | 389.11ms | 1.555× | 0.82% | PASS |
| file_writes | `oltp_insert` | 55.45ms | 53.29ms | 0.961× | 14.80% | PASS |
| file_writes | `oltp_update_index` | 122.21ms | 175.11ms | 1.433× | 2.99% | PASS |
| file_writes | `oltp_update_non_index` | 101.97ms | 115.31ms | 1.131× | 10.23% | PASS |
| file_writes | `oltp_delete_insert` | 100.88ms | 137.94ms | 1.367× | 2.48% | PASS |
| file_writes | `oltp_write_only` | 85.86ms | 87.74ms | 1.022× | 12.62% | PASS |
| file_writes | `types_delete_insert` | 60.71ms | 76.41ms | 1.258× | 1.42% | PASS |
| file_writes | `oltp_read_write` | 145.03ms | 168.41ms | 1.161× | 5.93% | PASS |
| ac_reads | `oltp_point_select` | 56.10ms | 64.20ms | 1.144× | 1.44% | PASS |
| ac_reads | `oltp_range_select` | 17.55ms | 17.38ms | 0.991× | 2.49% | PASS |
| ac_reads | `oltp_sum_range` | 16.09ms | 17.39ms | 1.080× | 2.32% | PASS |
| ac_reads | `oltp_order_range` | 3.61ms | 3.63ms | 1.004× | 2.26% | PASS |
| ac_reads | `oltp_distinct_range` | 4.69ms | 4.72ms | 1.006× | 1.58% | PASS |
| ac_reads | `oltp_index_scan` | 7.68ms | 9.29ms | 1.209× | 1.54% | PASS |
| ac_reads | `select_random_points` | 22.07ms | 24.88ms | 1.127× | 2.47% | PASS |
| ac_reads | `select_random_ranges` | 6.81ms | 7.85ms | 1.153× | 0.95% | PASS |
| ac_reads | `covering_index_scan` | 8.46ms | 7.44ms | 0.879× | 1.38% | PASS |
| ac_reads | `groupby_scan` | 33.45ms | 34.85ms | 1.042× | 1.29% | PASS |
| ac_reads | `index_join` | 9.63ms | 11.62ms | 1.207× | 2.15% | PASS |
| ac_reads | `index_join_scan` | 5.32ms | 6.06ms | 1.140× | 4.25% | PASS |
| ac_reads | `types_table_scan` | 1.18s | 1.27s | 1.070× | 3.53% | PASS |
| ac_reads | `table_scan` | 1.50s | 1.42s | 0.948× | 2.15% | PASS |
| ac_reads | `oltp_read_only` | 165.01ms | 179.21ms | 1.086× | 1.66% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 31.05ms | 104.72ms | 3.373× | 8.28% | PASS |
| ac_writes | `oltp_insert_ac` | 32.37ms | 120.18ms | 3.713× | 5.26% | PASS |
| ac_writes | `oltp_update_index_ac` | 35.59ms | 138.26ms | 3.885× | 5.48% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 30.14ms | 114.89ms | 3.812× | 7.39% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 31.77ms | 132.29ms | 4.163× | 6.72% | PASS |
| ac_writes | `oltp_write_only_ac` | 31.13ms | 125.29ms | 4.024× | 9.51% | PASS |
| ac_writes | `types_delete_insert_ac` | 28.42ms | 117.60ms | 4.137× | 8.11% | PASS |
| ac_writes | `oltp_read_write_ac` | 38.29ms | 140.25ms | 3.663× | 9.96% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.69ms | 36.64ms | 1.234× | 1.37% | PASS |
| mem_reads | `oltp_range_select` | 12.38ms | 13.96ms | 1.127× | 1.47% | PASS |
| mem_reads | `oltp_sum_range` | 11.52ms | 13.82ms | 1.199× | 1.48% | PASS |
| mem_reads | `oltp_order_range` | 2.89ms | 3.14ms | 1.086× | 1.03% | PASS |
| mem_reads | `oltp_distinct_range` | 3.95ms | 4.21ms | 1.066× | 0.74% | PASS |
| mem_reads | `oltp_index_scan` | 4.46ms | 6.26ms | 1.404× | 1.79% | PASS |
| mem_reads | `select_random_points` | 17.34ms | 20.47ms | 1.181× | 1.42% | PASS |
| mem_reads | `select_random_ranges` | 3.99ms | 5.19ms | 1.300× | 0.59% | PASS |
| mem_reads | `covering_index_scan` | 4.39ms | 4.65ms | 1.059× | 1.45% | PASS |
| mem_reads | `groupby_scan` | 31.88ms | 33.70ms | 1.057× | 1.20% | PASS |
| mem_reads | `index_join` | 6.74ms | 9.29ms | 1.377× | 3.20% | PASS |
| mem_reads | `index_join_scan` | 4.07ms | 5.25ms | 1.288× | 1.97% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.22s | 1.176× | 0.80% | PASS |
| mem_reads | `table_scan` | 1.20s | 1.37s | 1.143× | 0.87% | PASS |
| mem_reads | `oltp_read_only` | 116.07ms | 135.34ms | 1.166× | 1.18% | PASS |
| mem_writes | `oltp_bulk_insert` | 241.77ms | 355.06ms | 1.469× | 1.06% | PASS |
| mem_writes | `oltp_insert` | 19.78ms | 39.64ms | 2.004× | 0.69% | PASS |
| mem_writes | `oltp_update_index` | 66.66ms | 129.08ms | 1.936× | 0.93% | PASS |
| mem_writes | `oltp_update_non_index` | 48.30ms | 84.63ms | 1.752× | 1.53% | PASS |
| mem_writes | `oltp_delete_insert` | 49.38ms | 104.36ms | 2.114× | 1.56% | PASS |
| mem_writes | `oltp_write_only` | 27.24ms | 61.89ms | 2.272× | 1.50% | PASS |
| mem_writes | `types_delete_insert` | 32.06ms | 54.28ms | 1.693× | 1.10% | PASS |
| mem_writes | `oltp_read_write` | 81.57ms | 138.45ms | 1.697× | 1.66% | PASS |
| file_reads | `oltp_point_select` | 105.99ms | 62.83ms | 0.593× | 1.05% | PASS |
| file_reads | `oltp_range_select` | 20.80ms | 16.89ms | 0.812× | 1.55% | PASS |
| file_reads | `oltp_sum_range` | 19.83ms | 16.78ms | 0.846× | 1.81% | PASS |
| file_reads | `oltp_order_range` | 3.74ms | 3.48ms | 0.933× | 1.70% | PASS |
| file_reads | `oltp_distinct_range` | 4.89ms | 4.58ms | 0.936× | 1.10% | PASS |
| file_reads | `oltp_index_scan` | 12.39ms | 9.17ms | 0.740× | 1.86% | PASS |
| file_reads | `select_random_points` | 26.56ms | 24.31ms | 0.915× | 2.63% | PASS |
| file_reads | `select_random_ranges` | 11.69ms | 7.87ms | 0.673× | 1.50% | PASS |
| file_reads | `covering_index_scan` | 12.40ms | 7.36ms | 0.593× | 1.51% | PASS |
| file_reads | `groupby_scan` | 32.55ms | 34.09ms | 1.047× | 1.17% | PASS |
| file_reads | `index_join` | 11.32ms | 11.37ms | 1.004× | 1.88% | PASS |
| file_reads | `index_join_scan` | 5.15ms | 5.78ms | 1.123× | 1.61% | PASS |
| file_reads | `types_table_scan` | 1.08s | 1.24s | 1.150× | 1.79% | PASS |
| file_reads | `table_scan` | 1.37s | 1.40s | 1.016× | 1.76% | PASS |
| file_reads | `oltp_read_only` | 238.29ms | 178.45ms | 0.749× | 1.00% | PASS |
| file_writes | `oltp_bulk_insert` | 261.25ms | 380.79ms | 1.458× | 0.83% | PASS |
| file_writes | `oltp_insert` | 31.49ms | 52.43ms | 1.665× | 1.50% | PASS |
| file_writes | `oltp_update_index` | 107.52ms | 166.68ms | 1.550× | 1.77% | PASS |
| file_writes | `oltp_update_non_index` | 80.58ms | 109.97ms | 1.365× | 1.94% | PASS |
| file_writes | `oltp_delete_insert` | 81.99ms | 131.83ms | 1.608× | 1.53% | PASS |
| file_writes | `oltp_write_only` | 57.41ms | 85.51ms | 1.490× | 1.75% | PASS |
| file_writes | `types_delete_insert` | 53.70ms | 73.93ms | 1.377× | 1.71% | PASS |
| file_writes | `oltp_read_write` | 115.65ms | 162.69ms | 1.407× | 1.89% | PASS |
| ac_reads | `oltp_point_select` | 55.93ms | 63.28ms | 1.131× | 1.35% | PASS |
| ac_reads | `oltp_range_select` | 16.45ms | 16.94ms | 1.030× | 2.11% | PASS |
| ac_reads | `oltp_sum_range` | 15.12ms | 16.84ms | 1.114× | 1.43% | PASS |
| ac_reads | `oltp_order_range` | 3.32ms | 3.49ms | 1.052× | 1.63% | PASS |
| ac_reads | `oltp_distinct_range` | 4.37ms | 4.60ms | 1.052× | 1.41% | PASS |
| ac_reads | `oltp_index_scan` | 7.51ms | 9.15ms | 1.219× | 1.82% | PASS |
| ac_reads | `select_random_points` | 21.48ms | 24.16ms | 1.125× | 2.00% | PASS |
| ac_reads | `select_random_ranges` | 6.85ms | 7.85ms | 1.147× | 1.44% | PASS |
| ac_reads | `covering_index_scan` | 7.84ms | 7.35ms | 0.938× | 1.91% | PASS |
| ac_reads | `groupby_scan` | 32.01ms | 34.05ms | 1.064× | 0.90% | PASS |
| ac_reads | `index_join` | 8.87ms | 11.32ms | 1.276× | 1.45% | PASS |
| ac_reads | `index_join_scan` | 4.74ms | 5.76ms | 1.215× | 2.61% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.22s | 1.174× | 1.18% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.37s | 1.140× | 0.82% | PASS |
| ac_reads | `oltp_read_only` | 155.63ms | 174.71ms | 1.123× | 1.04% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.38ms | 79.73ms | 3.729× | 5.79% | PASS |
| ac_writes | `oltp_insert_ac` | 23.90ms | 101.24ms | 4.237× | 5.43% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.30ms | 112.25ms | 4.269× | 6.04% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.93ms | 92.56ms | 4.221× | 6.30% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.57ms | 104.55ms | 4.436× | 5.73% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.25ms | 102.66ms | 4.234× | 4.93% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.38ms | 90.90ms | 4.251× | 5.92% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.61ms | 107.55ms | 3.514× | 6.08% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 35.27ms | 41.29ms | 1.171× | 2.80% | PASS |
| mem_reads | `oltp_range_select` | 20.44ms | 21.79ms | 1.066× | 3.24% | PASS |
| mem_reads | `oltp_sum_range` | 18.88ms | 21.43ms | 1.135× | 1.60% | PASS |
| mem_reads | `oltp_order_range` | 3.63ms | 3.88ms | 1.068× | 1.00% | PASS |
| mem_reads | `oltp_distinct_range` | 4.78ms | 4.96ms | 1.038× | 1.31% | PASS |
| mem_reads | `oltp_index_scan` | 4.73ms | 6.59ms | 1.395× | 3.59% | PASS |
| mem_reads | `select_random_points` | 29.69ms | 33.59ms | 1.131× | 3.85% | PASS |
| mem_reads | `select_random_ranges` | 8.02ms | 9.19ms | 1.146× | 2.49% | PASS |
| mem_reads | `covering_index_scan` | 4.25ms | 4.64ms | 1.091× | 4.81% | PASS |
| mem_reads | `groupby_scan` | 36.94ms | 38.88ms | 1.053× | 1.07% | PASS |
| mem_reads | `index_join` | 8.38ms | 11.03ms | 1.317× | 3.99% | PASS |
| mem_reads | `index_join_scan` | 4.46ms | 5.75ms | 1.287× | 4.62% | PASS |
| mem_reads | `types_table_scan` | 1.12s | 1.27s | 1.136× | 1.43% | PASS |
| mem_reads | `table_scan` | 1.37s | 1.43s | 1.050× | 1.18% | PASS |
| mem_reads | `oltp_read_only` | 163.98ms | 174.91ms | 1.067× | 1.43% | PASS |
| mem_writes | `oltp_bulk_insert` | 243.73ms | 360.15ms | 1.478× | 1.04% | PASS |
| mem_writes | `oltp_insert` | 19.19ms | 36.96ms | 1.926× | 1.20% | PASS |
| mem_writes | `oltp_update_index` | 72.74ms | 124.02ms | 1.705× | 4.68% | PASS |
| mem_writes | `oltp_update_non_index` | 53.38ms | 85.88ms | 1.609× | 2.54% | PASS |
| mem_writes | `oltp_delete_insert` | 51.59ms | 100.00ms | 1.939× | 2.43% | PASS |
| mem_writes | `oltp_write_only` | 28.46ms | 59.74ms | 2.099× | 1.82% | PASS |
| mem_writes | `types_delete_insert` | 34.26ms | 56.46ms | 1.648× | 1.86% | PASS |
| mem_writes | `oltp_read_write` | 108.19ms | 159.56ms | 1.475× | 2.36% | PASS |
| file_reads | `oltp_point_select` | 111.30ms | 67.33ms | 0.605× | 1.77% | PASS |
| file_reads | `oltp_range_select` | 28.42ms | 24.65ms | 0.867× | 2.26% | PASS |
| file_reads | `oltp_sum_range` | 27.07ms | 24.38ms | 0.900× | 3.28% | PASS |
| file_reads | `oltp_order_range` | 4.62ms | 4.29ms | 0.930× | 2.12% | PASS |
| file_reads | `oltp_distinct_range` | 5.82ms | 5.40ms | 0.928× | 2.35% | PASS |
| file_reads | `oltp_index_scan` | 12.95ms | 9.45ms | 0.730× | 3.28% | PASS |
| file_reads | `select_random_points` | 40.08ms | 37.73ms | 0.942× | 2.74% | PASS |
| file_reads | `select_random_ranges` | 16.09ms | 12.15ms | 0.755× | 2.09% | PASS |
| file_reads | `covering_index_scan` | 12.50ms | 7.29ms | 0.584× | 3.48% | PASS |
| file_reads | `groupby_scan` | 38.28ms | 39.75ms | 1.038× | 1.01% | PASS |
| file_reads | `index_join` | 13.25ms | 13.72ms | 1.035× | 5.56% | PASS |
| file_reads | `index_join_scan` | 5.51ms | 6.29ms | 1.142× | 5.30% | PASS |
| file_reads | `types_table_scan` | 1.12s | 1.26s | 1.121× | 0.94% | PASS |
| file_reads | `table_scan` | 1.37s | 1.43s | 1.046× | 1.13% | PASS |
| file_reads | `oltp_read_only` | 276.14ms | 215.37ms | 0.780× | 1.23% | PASS |
| file_writes | `oltp_bulk_insert` | 259.91ms | 383.79ms | 1.477× | 0.93% | PASS |
| file_writes | `oltp_insert` | 30.23ms | 47.25ms | 1.563× | 2.28% | PASS |
| file_writes | `oltp_update_index` | 105.12ms | 150.12ms | 1.428× | 2.76% | PASS |
| file_writes | `oltp_update_non_index` | 85.44ms | 107.84ms | 1.262× | 2.23% | PASS |
| file_writes | `oltp_delete_insert` | 84.78ms | 122.98ms | 1.451× | 2.41% | PASS |
| file_writes | `oltp_write_only` | 60.75ms | 80.06ms | 1.318× | 3.85% | PASS |
| file_writes | `types_delete_insert` | 54.72ms | 71.02ms | 1.298× | 2.20% | PASS |
| file_writes | `oltp_read_write` | 146.98ms | 184.45ms | 1.255× | 1.88% | PASS |
| ac_reads | `oltp_point_select` | 60.80ms | 68.07ms | 1.120× | 1.82% | PASS |
| ac_reads | `oltp_range_select` | 23.74ms | 24.89ms | 1.049× | 2.71% | PASS |
| ac_reads | `oltp_sum_range` | 22.07ms | 24.43ms | 1.107× | 1.92% | PASS |
| ac_reads | `oltp_order_range` | 4.17ms | 4.36ms | 1.047× | 2.82% | PASS |
| ac_reads | `oltp_distinct_range` | 5.28ms | 5.43ms | 1.029× | 2.50% | PASS |
| ac_reads | `oltp_index_scan` | 7.88ms | 9.43ms | 1.196× | 2.60% | PASS |
| ac_reads | `select_random_points` | 33.57ms | 38.27ms | 1.140× | 3.81% | PASS |
| ac_reads | `select_random_ranges` | 10.87ms | 12.17ms | 1.120× | 1.77% | PASS |
| ac_reads | `covering_index_scan` | 7.36ms | 7.32ms | 0.994× | 2.21% | PASS |
| ac_reads | `groupby_scan` | 37.67ms | 39.78ms | 1.056× | 1.08% | PASS |
| ac_reads | `index_join` | 10.41ms | 13.50ms | 1.297× | 3.97% | PASS |
| ac_reads | `index_join_scan` | 5.09ms | 6.36ms | 1.249× | 3.92% | PASS |
| ac_reads | `types_table_scan` | 1.13s | 1.26s | 1.113× | 1.15% | PASS |
| ac_reads | `table_scan` | 1.38s | 1.44s | 1.044× | 1.20% | PASS |
| ac_reads | `oltp_read_only` | 201.69ms | 216.05ms | 1.071× | 1.12% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 31.56ms | 111.34ms | 3.528× | 6.59% | PASS |
| ac_writes | `oltp_insert_ac` | 34.74ms | 128.33ms | 3.694× | 8.06% | PASS |
| ac_writes | `oltp_update_index_ac` | 37.35ms | 141.00ms | 3.775× | 5.42% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 32.76ms | 122.65ms | 3.744× | 8.93% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 34.73ms | 138.23ms | 3.980× | 9.05% | PASS |
| ac_writes | `oltp_write_only_ac` | 36.39ms | 136.15ms | 3.741× | 8.08% | PASS |
| ac_writes | `types_delete_insert_ac` | 32.62ms | 127.02ms | 3.894× | 10.08% | PASS |
| ac_writes | `oltp_read_write_ac` | 42.16ms | 144.92ms | 3.438× | 6.57% | PASS |

</details>

## Version-control latency

Wall time: 2m 28s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 92.49ms | 200.00ms | 46.2% | 0.29% | PASS |
| `status_dirty_many_tables` | 95.91ms | 200.00ms | 48.0% | 0.24% | PASS |
| `diff_regular_working_one_table` | 88.21ms | 150.00ms | 58.8% | 0.23% | PASS |
| `diff_regular_working_many_tables` | 101.42ms | 200.00ms | 50.7% | 0.21% | PASS |
| `diff_stat_working_many_tables` | 101.59ms | 200.00ms | 50.8% | 0.28% | PASS |
| `diff_schema_working_many_tables` | 102.26ms | 200.00ms | 51.1% | 0.24% | PASS |
| `branch_list_many_branches` | 24.10ms | 100.00ms | 24.1% | 1.51% | PASS |
| `branch_create_delete` | 26.16ms | 100.00ms | 26.2% | 1.10% | PASS |
| `checkout_branch_clean` | 59.92ms | 200.00ms | 30.0% | 0.63% | PASS |
| `merge_data_no_conflicts` | 31.69ms | 150.00ms | 21.1% | 2.08% | PASS |
| `merge_schema_no_conflicts` | 23.67ms | 100.00ms | 23.7% | 1.76% | PASS |
| `merge_data_conflicts` | 129.81ms | 250.00ms | 51.9% | 0.33% | PASS |
| `merge_data_conflicts_with_resolve` | 129.55ms | 250.00ms | 51.8% | 0.27% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
