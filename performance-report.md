# DoltLite Performance Report

> Nightly result: **FAIL**
>
> Generated: 2026-11-04 19:05 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261026.354.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/41877452078)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 11m 2s | 9.10s | 11.00s | 1.208× | 1.40% | **PASS** |
| textpk | 69 | 55 | 1h 36m 26s | 10.06s | 11.95s | 1.188× | 2.08% | **PASS** |
| blobpk | 69 | 55 | 1h 9m 19s | 7.25s | 7.90s | 1.089× | 3.14% | **FAIL** |
| compositepk | 69 | 55 | 1h 29m 40s | 9.59s | 12.08s | 1.259× | 1.63% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.02ms | 24.77ms | 1.125× | 1.53% | PASS |
| mem_reads | `oltp_range_select` | 9.96ms | 11.68ms | 1.172× | 1.37% | PASS |
| mem_reads | `oltp_sum_range` | 9.19ms | 11.04ms | 1.202× | 1.37% | PASS |
| mem_reads | `oltp_order_range` | 2.57ms | 2.87ms | 1.113× | 1.59% | PASS |
| mem_reads | `oltp_distinct_range` | 3.52ms | 3.79ms | 1.078× | 0.68% | PASS |
| mem_reads | `oltp_index_scan` | 3.65ms | 4.67ms | 1.280× | 1.73% | PASS |
| mem_reads | `select_random_points` | 10.29ms | 11.20ms | 1.088× | 1.24% | PASS |
| mem_reads | `select_random_ranges` | 2.75ms | 3.52ms | 1.282× | 1.17% | PASS |
| mem_reads | `covering_index_scan` | 3.54ms | 3.71ms | 1.047× | 2.28% | PASS |
| mem_reads | `groupby_scan` | 30.26ms | 32.13ms | 1.062× | 0.52% | PASS |
| mem_reads | `index_join` | 5.40ms | 7.68ms | 1.422× | 1.60% | PASS |
| mem_reads | `index_join_scan` | 2.97ms | 4.30ms | 1.447× | 1.54% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.24s | 1.195× | 0.66% | PASS |
| mem_reads | `table_scan` | 1.25s | 1.38s | 1.100× | 0.98% | PASS |
| mem_reads | `oltp_read_only` | 98.17ms | 110.89ms | 1.130× | 1.25% | PASS |
| mem_writes | `oltp_bulk_insert` | 144.61ms | 203.46ms | 1.407× | 0.79% | PASS |
| mem_writes | `oltp_insert` | 13.14ms | 23.98ms | 1.825× | 1.41% | PASS |
| mem_writes | `oltp_update_index` | 45.71ms | 99.81ms | 2.184× | 1.73% | PASS |
| mem_writes | `oltp_update_non_index` | 29.85ms | 55.03ms | 1.843× | 1.61% | PASS |
| mem_writes | `oltp_delete_insert` | 41.16ms | 72.24ms | 1.755× | 1.44% | PASS |
| mem_writes | `oltp_write_only` | 19.48ms | 41.64ms | 2.138× | 1.40% | PASS |
| mem_writes | `types_delete_insert` | 21.62ms | 37.22ms | 1.722× | 1.81% | PASS |
| mem_writes | `oltp_read_write` | 59.66ms | 98.66ms | 1.654× | 1.70% | PASS |
| file_reads | `oltp_point_select` | 51.99ms | 33.33ms | 0.641× | 0.83% | PASS |
| file_reads | `oltp_range_select` | 13.18ms | 12.73ms | 0.966× | 2.02% | PASS |
| file_reads | `oltp_sum_range` | 12.40ms | 12.17ms | 0.982× | 1.87% | PASS |
| file_reads | `oltp_order_range` | 2.99ms | 3.02ms | 1.011× | 0.71% | PASS |
| file_reads | `oltp_distinct_range` | 3.93ms | 3.94ms | 1.003× | 0.99% | PASS |
| file_reads | `oltp_index_scan` | 6.96ms | 5.83ms | 0.838× | 0.86% | PASS |
| file_reads | `select_random_points` | 14.14ms | 12.42ms | 0.878× | 1.46% | PASS |
| file_reads | `select_random_ranges` | 5.85ms | 4.50ms | 0.770× | 0.89% | PASS |
| file_reads | `covering_index_scan` | 6.84ms | 4.88ms | 0.714× | 1.06% | PASS |
| file_reads | `groupby_scan` | 30.84ms | 32.23ms | 1.045× | 0.39% | PASS |
| file_reads | `index_join` | 7.38ms | 8.65ms | 1.172× | 0.98% | PASS |
| file_reads | `index_join_scan` | 3.42ms | 4.56ms | 1.333× | 1.44% | PASS |
| file_reads | `types_table_scan` | 1.07s | 1.27s | 1.184× | 0.60% | PASS |
| file_reads | `table_scan` | 1.27s | 1.39s | 1.093× | 0.91% | PASS |
| file_reads | `oltp_read_only` | 144.54ms | 126.28ms | 0.874× | 0.84% | PASS |
| file_writes | `oltp_bulk_insert` | 180.44ms | 258.31ms | 1.432× | 3.01% | PASS |
| file_writes | `oltp_insert` | 19.85ms | 38.13ms | 1.921× | 2.17% | PASS |
| file_writes | `oltp_update_index` | 131.47ms | 158.97ms | 1.209× | 2.30% | PASS |
| file_writes | `oltp_update_non_index` | 100.95ms | 98.73ms | 0.978× | 3.03% | PASS |
| file_writes | `oltp_delete_insert` | 111.36ms | 128.50ms | 1.154× | 2.60% | PASS |
| file_writes | `oltp_write_only` | 81.17ms | 79.06ms | 0.974× | 0.63% | PASS |
| file_writes | `types_delete_insert` | 60.73ms | 61.41ms | 1.011× | 4.06% | PASS |
| file_writes | `oltp_read_write` | 121.17ms | 139.24ms | 1.149× | 3.61% | PASS |
| ac_reads | `oltp_point_select` | 32.11ms | 33.53ms | 1.044× | 1.03% | PASS |
| ac_reads | `oltp_range_select` | 11.25ms | 12.90ms | 1.147× | 0.94% | PASS |
| ac_reads | `oltp_sum_range` | 10.75ms | 12.21ms | 1.135× | 1.03% | PASS |
| ac_reads | `oltp_order_range` | 2.77ms | 3.02ms | 1.092× | 1.02% | PASS |
| ac_reads | `oltp_distinct_range` | 3.75ms | 3.96ms | 1.057× | 0.88% | PASS |
| ac_reads | `oltp_index_scan` | 5.01ms | 5.81ms | 1.160× | 1.00% | PASS |
| ac_reads | `select_random_points` | 12.12ms | 12.45ms | 1.027× | 2.21% | PASS |
| ac_reads | `select_random_ranges` | 3.90ms | 4.49ms | 1.150× | 1.43% | PASS |
| ac_reads | `covering_index_scan` | 4.77ms | 4.83ms | 1.012× | 1.02% | PASS |
| ac_reads | `groupby_scan` | 30.27ms | 31.90ms | 1.054× | 1.07% | PASS |
| ac_reads | `index_join` | 6.39ms | 8.68ms | 1.358× | 1.29% | PASS |
| ac_reads | `index_join_scan` | 3.26ms | 4.55ms | 1.396× | 1.65% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.26s | 1.196× | 0.92% | PASS |
| ac_reads | `table_scan` | 1.24s | 1.38s | 1.109× | 0.84% | PASS |
| ac_reads | `oltp_read_only` | 113.95ms | 125.69ms | 1.103× | 1.12% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 27.69ms | 73.20ms | 2.643× | 3.44% | PASS |
| ac_writes | `oltp_insert_ac` | 30.36ms | 88.88ms | 2.928× | 4.00% | PASS |
| ac_writes | `oltp_update_index_ac` | 31.66ms | 101.98ms | 3.222× | 2.99% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 28.33ms | 81.92ms | 2.892× | 3.10% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 30.03ms | 94.05ms | 3.132× | 3.73% | PASS |
| ac_writes | `oltp_write_only_ac` | 30.13ms | 94.69ms | 3.142× | 4.57% | PASS |
| ac_writes | `types_delete_insert_ac` | 28.14ms | 91.68ms | 3.258× | 4.52% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.49ms | 98.10ms | 2.929× | 3.04% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.40ms | 40.62ms | 1.216× | 1.81% | PASS |
| mem_reads | `oltp_range_select` | 14.82ms | 14.62ms | 0.986× | 2.35% | PASS |
| mem_reads | `oltp_sum_range` | 12.90ms | 14.61ms | 1.132× | 2.53% | PASS |
| mem_reads | `oltp_order_range` | 3.07ms | 3.20ms | 1.042× | 1.86% | PASS |
| mem_reads | `oltp_distinct_range` | 4.12ms | 4.28ms | 1.040× | 1.12% | PASS |
| mem_reads | `oltp_index_scan` | 4.66ms | 6.43ms | 1.381× | 2.68% | PASS |
| mem_reads | `select_random_points` | 18.23ms | 21.16ms | 1.161× | 3.05% | PASS |
| mem_reads | `select_random_ranges` | 4.16ms | 5.25ms | 1.263× | 2.72% | PASS |
| mem_reads | `covering_index_scan` | 4.76ms | 4.74ms | 0.995× | 3.42% | PASS |
| mem_reads | `groupby_scan` | 32.32ms | 34.65ms | 1.072× | 1.39% | PASS |
| mem_reads | `index_join` | 7.26ms | 9.79ms | 1.349× | 3.83% | PASS |
| mem_reads | `index_join_scan` | 4.79ms | 5.50ms | 1.148× | 3.13% | PASS |
| mem_reads | `types_table_scan` | 1.08s | 1.26s | 1.166× | 2.97% | PASS |
| mem_reads | `table_scan` | 1.24s | 1.38s | 1.110× | 2.56% | PASS |
| mem_reads | `oltp_read_only` | 117.15ms | 136.99ms | 1.169× | 1.27% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.21ms | 369.43ms | 1.564× | 0.68% | PASS |
| mem_writes | `oltp_insert` | 22.19ms | 42.66ms | 1.922× | 2.08% | PASS |
| mem_writes | `oltp_update_index` | 72.92ms | 145.98ms | 2.002× | 1.39% | PASS |
| mem_writes | `oltp_update_non_index` | 50.63ms | 95.82ms | 1.892× | 1.70% | PASS |
| mem_writes | `oltp_delete_insert` | 52.12ms | 112.64ms | 2.161× | 1.25% | PASS |
| mem_writes | `oltp_write_only` | 29.64ms | 68.06ms | 2.296× | 1.18% | PASS |
| mem_writes | `types_delete_insert` | 33.98ms | 61.32ms | 1.805× | 1.95% | PASS |
| mem_writes | `oltp_read_write` | 86.71ms | 147.49ms | 1.701× | 1.39% | PASS |
| file_reads | `oltp_point_select` | 102.22ms | 62.48ms | 0.611× | 1.36% | PASS |
| file_reads | `oltp_range_select` | 21.79ms | 17.05ms | 0.782× | 2.47% | PASS |
| file_reads | `oltp_sum_range` | 22.13ms | 17.67ms | 0.798× | 1.80% | PASS |
| file_reads | `oltp_order_range` | 4.16ms | 3.59ms | 0.862× | 1.95% | PASS |
| file_reads | `oltp_distinct_range` | 5.00ms | 4.65ms | 0.931× | 1.92% | PASS |
| file_reads | `oltp_index_scan` | 11.79ms | 9.17ms | 0.778× | 1.69% | PASS |
| file_reads | `select_random_points` | 26.40ms | 24.39ms | 0.924× | 2.23% | PASS |
| file_reads | `select_random_ranges` | 11.20ms | 7.72ms | 0.690× | 1.29% | PASS |
| file_reads | `covering_index_scan` | 12.16ms | 7.31ms | 0.601× | 2.00% | PASS |
| file_reads | `groupby_scan` | 34.58ms | 35.85ms | 1.037× | 1.11% | PASS |
| file_reads | `index_join` | 12.98ms | 12.28ms | 0.946× | 3.13% | PASS |
| file_reads | `index_join_scan` | 5.99ms | 6.11ms | 1.020× | 4.83% | PASS |
| file_reads | `types_table_scan` | 1.13s | 1.25s | 1.112× | 2.43% | PASS |
| file_reads | `table_scan` | 1.53s | 1.42s | 0.934× | 2.44% | PASS |
| file_reads | `oltp_read_only` | 235.13ms | 176.37ms | 0.750× | 1.39% | PASS |
| file_writes | `oltp_bulk_insert` | 256.49ms | 404.46ms | 1.577× | 0.76% | PASS |
| file_writes | `oltp_insert` | 52.58ms | 58.49ms | 1.112× | 18.69% | PASS |
| file_writes | `oltp_update_index` | 127.52ms | 191.10ms | 1.499× | 2.76% | PASS |
| file_writes | `oltp_update_non_index` | 109.30ms | 123.94ms | 1.134× | 13.84% | PASS |
| file_writes | `oltp_delete_insert` | 94.55ms | 146.45ms | 1.549× | 2.42% | PASS |
| file_writes | `oltp_write_only` | 83.73ms | 94.81ms | 1.132× | 15.73% | PASS |
| file_writes | `types_delete_insert` | 58.22ms | 80.84ms | 1.388× | 1.85% | PASS |
| file_writes | `oltp_read_write` | 158.36ms | 184.95ms | 1.168× | 9.76% | PASS |
| ac_reads | `oltp_point_select` | 55.79ms | 63.39ms | 1.136× | 1.34% | PASS |
| ac_reads | `oltp_range_select` | 19.35ms | 17.97ms | 0.929× | 2.04% | PASS |
| ac_reads | `oltp_sum_range` | 17.73ms | 18.29ms | 1.032× | 1.91% | PASS |
| ac_reads | `oltp_order_range` | 3.73ms | 3.67ms | 0.985× | 1.85% | PASS |
| ac_reads | `oltp_distinct_range` | 4.65ms | 4.68ms | 1.007× | 1.76% | PASS |
| ac_reads | `oltp_index_scan` | 7.72ms | 9.20ms | 1.192× | 1.27% | PASS |
| ac_reads | `select_random_points` | 22.51ms | 24.39ms | 1.084× | 3.45% | PASS |
| ac_reads | `select_random_ranges` | 6.84ms | 7.78ms | 1.137× | 1.71% | PASS |
| ac_reads | `covering_index_scan` | 8.12ms | 7.29ms | 0.898× | 4.30% | PASS |
| ac_reads | `groupby_scan` | 32.68ms | 35.18ms | 1.076× | 0.91% | PASS |
| ac_reads | `index_join` | 9.30ms | 11.41ms | 1.227× | 2.90% | PASS |
| ac_reads | `index_join_scan` | 5.38ms | 5.95ms | 1.106× | 3.82% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.22s | 1.172× | 0.65% | PASS |
| ac_reads | `table_scan` | 1.21s | 1.37s | 1.130× | 0.70% | PASS |
| ac_reads | `oltp_read_only` | 154.49ms | 173.62ms | 1.124× | 1.02% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.33ms | 61.68ms | 3.034× | 3.10% | PASS |
| ac_writes | `oltp_insert_ac` | 24.57ms | 76.67ms | 3.121× | 6.11% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.24ms | 93.63ms | 3.709× | 2.41% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.71ms | 73.31ms | 3.539× | 5.62% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.30ms | 84.54ms | 3.478× | 4.61% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.51ms | 84.33ms | 3.587× | 5.58% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.82ms | 76.62ms | 3.512× | 5.79% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.94ms | 92.41ms | 2.987× | 3.78% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 18.34ms | 21.38ms | 1.166× | 2.60% | PASS |
| mem_reads | `oltp_range_select` | 8.80ms | 8.57ms | 0.973× | 3.98% | PASS |
| mem_reads | `oltp_sum_range` | 8.55ms | 8.54ms | 1.000× | 3.80% | PASS |
| mem_reads | `oltp_order_range` | 2.02ms | 1.97ms | 0.977× | 2.73% | PASS |
| mem_reads | `oltp_distinct_range` | 2.35ms | 2.34ms | 0.993× | 2.92% | PASS |
| mem_reads | `oltp_index_scan` | 2.73ms | 3.68ms | 1.350× | 3.49% | PASS |
| mem_reads | `select_random_points` | 11.54ms | 12.91ms | 1.118× | 3.40% | PASS |
| mem_reads | `select_random_ranges` | 2.74ms | 3.35ms | 1.225× | 4.26% | PASS |
| mem_reads | `covering_index_scan` | 2.49ms | 2.75ms | 1.103× | 1.93% | PASS |
| mem_reads | `groupby_scan` | 19.09ms | 19.10ms | 1.001× | 2.14% | PASS |
| mem_reads | `index_join` | 4.36ms | 5.46ms | 1.252× | 2.50% | PASS |
| mem_reads | `index_join_scan` | 3.49ms | 4.67ms | 1.339× | 4.50% | PASS |
| mem_reads | `types_table_scan` | 681.88ms | 731.97ms | 1.073× | 1.88% | PASS |
| mem_reads | `table_scan` | 783.73ms | 819.08ms | 1.045× | 1.23% | PASS |
| mem_reads | `oltp_read_only` | 64.35ms | 69.81ms | 1.085× | 1.31% | PASS |
| mem_writes | `oltp_bulk_insert` | 137.75ms | 204.31ms | 1.483× | 1.41% | PASS |
| mem_writes | `oltp_insert` | 11.54ms | 25.54ms | 2.213× | 1.49% | PASS |
| mem_writes | `oltp_update_index` | 40.79ms | 88.58ms | 2.172× | 1.80% | PASS |
| mem_writes | `oltp_update_non_index` | 30.80ms | 58.72ms | 1.907× | 2.39% | PASS |
| mem_writes | `oltp_delete_insert` | 29.70ms | 69.63ms | 2.344× | 1.99% | PASS |
| mem_writes | `oltp_write_only` | 19.79ms | 50.77ms | 2.565× | 2.78% | FAIL |
| mem_writes | `types_delete_insert` | 21.65ms | 40.90ms | 1.889× | 4.84% | PASS |
| mem_writes | `oltp_read_write` | 47.67ms | 84.14ms | 1.765× | 3.52% | PASS |
| file_reads | `oltp_point_select` | 71.47ms | 39.41ms | 0.551× | 2.69% | PASS |
| file_reads | `oltp_range_select` | 14.45ms | 10.32ms | 0.714× | 4.10% | PASS |
| file_reads | `oltp_sum_range` | 15.26ms | 10.61ms | 0.695× | 5.31% | PASS |
| file_reads | `oltp_order_range` | 2.68ms | 2.16ms | 0.805× | 2.33% | PASS |
| file_reads | `oltp_distinct_range` | 3.16ms | 2.63ms | 0.833× | 1.52% | PASS |
| file_reads | `oltp_index_scan` | 8.82ms | 5.94ms | 0.674× | 2.03% | PASS |
| file_reads | `select_random_points` | 17.59ms | 14.76ms | 0.839× | 2.46% | PASS |
| file_reads | `select_random_ranges` | 8.39ms | 5.28ms | 0.629× | 1.79% | PASS |
| file_reads | `covering_index_scan` | 8.81ms | 4.80ms | 0.545× | 2.66% | PASS |
| file_reads | `groupby_scan` | 19.82ms | 18.66ms | 0.942× | 2.58% | PASS |
| file_reads | `index_join` | 8.16ms | 7.15ms | 0.876× | 3.28% | PASS |
| file_reads | `index_join_scan` | 3.97ms | 4.52ms | 1.140× | 4.37% | PASS |
| file_reads | `types_table_scan` | 708.96ms | 754.31ms | 1.064× | 3.32% | PASS |
| file_reads | `table_scan` | 914.63ms | 871.84ms | 0.953× | 4.37% | PASS |
| file_reads | `oltp_read_only` | 147.12ms | 98.61ms | 0.670× | 2.27% | PASS |
| file_writes | `oltp_bulk_insert` | 189.90ms | 280.09ms | 1.475× | 4.13% | PASS |
| file_writes | `oltp_insert` | 35.67ms | 57.25ms | 1.605× | 5.08% | PASS |
| file_writes | `oltp_update_index` | 176.17ms | 199.64ms | 1.133× | 3.36% | PASS |
| file_writes | `oltp_update_non_index` | 135.35ms | 132.44ms | 0.979× | 4.58% | PASS |
| file_writes | `oltp_delete_insert` | 131.28ms | 140.53ms | 1.070× | 6.79% | PASS |
| file_writes | `oltp_write_only` | 99.41ms | 100.05ms | 1.006× | 6.85% | PASS |
| file_writes | `types_delete_insert` | 89.33ms | 84.39ms | 0.945× | 10.87% | PASS |
| file_writes | `oltp_read_write` | 131.14ms | 143.56ms | 1.095× | 4.01% | PASS |
| ac_reads | `oltp_point_select` | 37.53ms | 39.83ms | 1.061× | 2.23% | PASS |
| ac_reads | `oltp_range_select` | 11.61ms | 10.51ms | 0.906× | 2.75% | PASS |
| ac_reads | `oltp_sum_range` | 11.69ms | 10.34ms | 0.885× | 4.19% | PASS |
| ac_reads | `oltp_order_range` | 2.42ms | 2.21ms | 0.910× | 1.91% | PASS |
| ac_reads | `oltp_distinct_range` | 2.91ms | 2.68ms | 0.921× | 1.74% | PASS |
| ac_reads | `oltp_index_scan` | 5.54ms | 6.22ms | 1.122× | 2.20% | PASS |
| ac_reads | `select_random_points` | 15.16ms | 15.56ms | 1.026× | 4.12% | PASS |
| ac_reads | `select_random_ranges` | 4.95ms | 5.22ms | 1.055× | 1.39% | PASS |
| ac_reads | `covering_index_scan` | 5.59ms | 4.98ms | 0.891× | 2.13% | PASS |
| ac_reads | `groupby_scan` | 20.15ms | 19.47ms | 0.966× | 2.12% | PASS |
| ac_reads | `index_join` | 6.85ms | 7.97ms | 1.164× | 4.01% | PASS |
| ac_reads | `index_join_scan` | 3.93ms | 4.78ms | 1.216× | 4.58% | PASS |
| ac_reads | `types_table_scan` | 841.75ms | 826.55ms | 0.982× | 3.14% | PASS |
| ac_reads | `table_scan` | 1.07s | 942.17ms | 0.883× | 2.56% | PASS |
| ac_reads | `oltp_read_only` | 116.62ms | 111.09ms | 0.953× | 2.16% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.28ms | 54.46ms | 2.444× | 6.80% | PASS |
| ac_writes | `oltp_insert_ac` | 26.17ms | 71.25ms | 2.722× | 9.64% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.34ms | 79.40ms | 2.802× | 6.29% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.00ms | 64.88ms | 2.949× | 10.92% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.84ms | 76.32ms | 3.073× | 11.73% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.50ms | 72.17ms | 2.830× | 5.36% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.75ms | 65.10ms | 3.138× | 5.73% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.42ms | 78.16ms | 2.750× | 6.94% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.38ms | 40.65ms | 1.218× | 1.48% | PASS |
| mem_reads | `oltp_range_select` | 19.13ms | 21.25ms | 1.111× | 1.48% | PASS |
| mem_reads | `oltp_sum_range` | 18.12ms | 21.03ms | 1.160× | 1.77% | PASS |
| mem_reads | `oltp_order_range` | 3.58ms | 3.94ms | 1.102× | 1.23% | PASS |
| mem_reads | `oltp_distinct_range` | 4.72ms | 5.03ms | 1.067× | 1.56% | PASS |
| mem_reads | `oltp_index_scan` | 4.63ms | 6.26ms | 1.351× | 1.39% | PASS |
| mem_reads | `select_random_points` | 28.64ms | 32.44ms | 1.133× | 2.20% | PASS |
| mem_reads | `select_random_ranges` | 7.94ms | 9.23ms | 1.162× | 1.11% | PASS |
| mem_reads | `covering_index_scan` | 4.31ms | 4.39ms | 1.019× | 3.65% | PASS |
| mem_reads | `groupby_scan` | 36.53ms | 39.34ms | 1.077× | 0.90% | PASS |
| mem_reads | `index_join` | 8.26ms | 10.51ms | 1.273× | 1.63% | PASS |
| mem_reads | `index_join_scan` | 4.16ms | 5.41ms | 1.301× | 2.05% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.24s | 1.191× | 0.59% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.39s | 1.184× | 0.63% | PASS |
| mem_reads | `oltp_read_only` | 149.01ms | 169.78ms | 1.139× | 1.16% | PASS |
| mem_writes | `oltp_bulk_insert` | 250.35ms | 370.29ms | 1.479× | 1.04% | PASS |
| mem_writes | `oltp_insert` | 20.38ms | 40.66ms | 1.995× | 3.93% | PASS |
| mem_writes | `oltp_update_index` | 80.41ms | 141.06ms | 1.754× | 4.47% | PASS |
| mem_writes | `oltp_update_non_index` | 57.86ms | 97.93ms | 1.693× | 2.89% | PASS |
| mem_writes | `oltp_delete_insert` | 59.47ms | 115.23ms | 1.938× | 3.95% | PASS |
| mem_writes | `oltp_write_only` | 31.64ms | 69.37ms | 2.193× | 5.43% | PASS |
| mem_writes | `types_delete_insert` | 37.05ms | 63.78ms | 1.721× | 3.54% | PASS |
| mem_writes | `oltp_read_write` | 124.21ms | 181.75ms | 1.463× | 4.16% | PASS |
| file_reads | `oltp_point_select` | 107.31ms | 65.84ms | 0.614× | 1.25% | PASS |
| file_reads | `oltp_range_select` | 28.73ms | 24.85ms | 0.865× | 1.48% | PASS |
| file_reads | `oltp_sum_range` | 27.86ms | 24.80ms | 0.890× | 2.02% | PASS |
| file_reads | `oltp_order_range` | 5.03ms | 4.53ms | 0.901× | 2.80% | PASS |
| file_reads | `oltp_distinct_range` | 6.09ms | 5.60ms | 0.920× | 2.60% | PASS |
| file_reads | `oltp_index_scan` | 12.38ms | 9.30ms | 0.751× | 3.04% | PASS |
| file_reads | `select_random_points` | 39.65ms | 37.31ms | 0.941× | 3.04% | PASS |
| file_reads | `select_random_ranges` | 15.48ms | 12.09ms | 0.781× | 1.66% | PASS |
| file_reads | `covering_index_scan` | 11.54ms | 7.07ms | 0.612× | 1.43% | PASS |
| file_reads | `groupby_scan` | 37.61ms | 40.15ms | 1.068× | 0.73% | PASS |
| file_reads | `index_join` | 12.35ms | 12.80ms | 1.036× | 1.60% | PASS |
| file_reads | `index_join_scan` | 5.21ms | 6.00ms | 1.152× | 1.97% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.23s | 1.193× | 0.72% | PASS |
| file_reads | `table_scan` | 1.18s | 1.39s | 1.177× | 0.92% | PASS |
| file_reads | `oltp_read_only` | 254.50ms | 207.67ms | 0.816× | 1.30% | PASS |
| file_writes | `oltp_bulk_insert` | 264.49ms | 391.41ms | 1.480× | 1.00% | PASS |
| file_writes | `oltp_insert` | 26.29ms | 49.09ms | 1.867× | 1.35% | PASS |
| file_writes | `oltp_update_index` | 97.41ms | 152.19ms | 1.562× | 1.52% | PASS |
| file_writes | `oltp_update_non_index` | 77.19ms | 110.85ms | 1.436× | 1.37% | PASS |
| file_writes | `oltp_delete_insert` | 77.17ms | 126.48ms | 1.639× | 1.39% | PASS |
| file_writes | `oltp_write_only` | 50.81ms | 83.75ms | 1.648× | 1.21% | PASS |
| file_writes | `types_delete_insert` | 49.65ms | 71.56ms | 1.441× | 1.47% | PASS |
| file_writes | `oltp_read_write` | 126.70ms | 181.64ms | 1.434× | 1.13% | PASS |
| ac_reads | `oltp_point_select` | 56.67ms | 64.15ms | 1.132× | 0.88% | PASS |
| ac_reads | `oltp_range_select` | 22.43ms | 24.33ms | 1.085× | 2.91% | PASS |
| ac_reads | `oltp_sum_range` | 21.46ms | 24.39ms | 1.136× | 2.13% | PASS |
| ac_reads | `oltp_order_range` | 4.37ms | 4.52ms | 1.035× | 3.46% | PASS |
| ac_reads | `oltp_distinct_range` | 5.39ms | 5.53ms | 1.027× | 2.10% | PASS |
| ac_reads | `oltp_index_scan` | 7.62ms | 9.22ms | 1.210× | 1.42% | PASS |
| ac_reads | `select_random_points` | 32.12ms | 36.31ms | 1.131× | 1.32% | PASS |
| ac_reads | `select_random_ranges` | 10.53ms | 11.96ms | 1.136× | 1.30% | PASS |
| ac_reads | `covering_index_scan` | 7.22ms | 7.14ms | 0.990× | 2.42% | PASS |
| ac_reads | `groupby_scan` | 37.19ms | 40.11ms | 1.079× | 0.88% | PASS |
| ac_reads | `index_join` | 9.90ms | 12.79ms | 1.292× | 2.12% | PASS |
| ac_reads | `index_join_scan` | 4.79ms | 6.00ms | 1.252× | 1.78% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.23s | 1.186× | 0.84% | PASS |
| ac_reads | `table_scan` | 1.19s | 1.39s | 1.168× | 1.42% | PASS |
| ac_reads | `oltp_read_only` | 198.58ms | 213.54ms | 1.075× | 2.34% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.31ms | 61.49ms | 2.886× | 6.49% | PASS |
| ac_writes | `oltp_insert_ac` | 23.00ms | 84.43ms | 3.670× | 5.05% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.08ms | 96.21ms | 3.690× | 5.14% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.20ms | 76.76ms | 3.309× | 6.41% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.92ms | 86.93ms | 3.792× | 4.33% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.70ms | 86.05ms | 3.630× | 4.94% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.47ms | 76.15ms | 3.547× | 5.55% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.55ms | 98.06ms | 3.013× | 4.38% | PASS |

</details>

## Version-control latency

Wall time: 1m 55s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 64.79ms | 200.00ms | 32.4% | 0.71% | PASS |
| `status_dirty_many_tables` | 67.52ms | 200.00ms | 33.8% | 0.59% | PASS |
| `diff_regular_working_one_table` | 60.74ms | 150.00ms | 40.5% | 0.66% | PASS |
| `diff_regular_working_many_tables` | 72.40ms | 200.00ms | 36.2% | 0.62% | PASS |
| `diff_stat_working_many_tables` | 72.34ms | 200.00ms | 36.2% | 0.71% | PASS |
| `diff_schema_working_many_tables` | 72.99ms | 200.00ms | 36.5% | 0.66% | PASS |
| `branch_list_many_branches` | 21.48ms | 100.00ms | 21.5% | 0.98% | PASS |
| `branch_create_delete` | 23.08ms | 100.00ms | 23.1% | 1.16% | PASS |
| `checkout_branch_clean` | 47.89ms | 200.00ms | 23.9% | 0.80% | PASS |
| `merge_data_no_conflicts` | 26.36ms | 150.00ms | 17.6% | 0.95% | PASS |
| `merge_schema_no_conflicts` | 21.24ms | 100.00ms | 21.2% | 1.13% | PASS |
| `merge_data_conflicts` | 77.57ms | 250.00ms | 31.0% | 0.44% | PASS |
| `merge_data_conflicts_with_resolve` | 77.42ms | 250.00ms | 31.0% | 0.53% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
