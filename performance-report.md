# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-12 12:12 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260720.247.2
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/31588157151)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 21s | 8.90s | 11.54s | 1.298× | 2.23% | **PASS** |
| textpk | 69 | 55 | 1h 33m 13s | 10.00s | 11.94s | 1.194× | 1.94% | **PASS** |
| blobpk | 69 | 55 | 1h 11m 43s | 6.87s | 8.20s | 1.195× | 1.74% | **PASS** |
| compositepk | 69 | 55 | 1h 26m 12s | 10.11s | 12.09s | 1.195× | 1.18% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.89ms | 29.60ms | 1.239× | 3.21% | PASS |
| mem_reads | `oltp_range_select` | 9.79ms | 13.16ms | 1.344× | 3.95% | PASS |
| mem_reads | `oltp_sum_range` | 9.28ms | 12.36ms | 1.332× | 3.19% | PASS |
| mem_reads | `oltp_order_range` | 2.49ms | 2.97ms | 1.191× | 1.75% | PASS |
| mem_reads | `oltp_distinct_range` | 3.52ms | 4.01ms | 1.141× | 1.66% | PASS |
| mem_reads | `oltp_index_scan` | 3.84ms | 5.28ms | 1.374× | 1.73% | PASS |
| mem_reads | `select_random_points` | 9.39ms | 11.04ms | 1.176× | 3.71% | PASS |
| mem_reads | `select_random_ranges` | 3.12ms | 4.06ms | 1.303× | 3.28% | PASS |
| mem_reads | `covering_index_scan` | 4.22ms | 4.14ms | 0.979× | 1.74% | PASS |
| mem_reads | `groupby_scan` | 29.74ms | 33.45ms | 1.125× | 1.04% | PASS |
| mem_reads | `index_join` | 6.03ms | 8.03ms | 1.330× | 1.90% | PASS |
| mem_reads | `index_join_scan` | 3.52ms | 4.61ms | 1.307× | 4.16% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.34s | 1.285× | 1.12% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.40s | 1.191× | 1.58% | PASS |
| mem_reads | `oltp_read_only` | 102.94ms | 123.47ms | 1.200× | 2.93% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.28ms | 253.71ms | 1.400× | 1.31% | PASS |
| mem_writes | `oltp_insert` | 15.55ms | 28.31ms | 1.821× | 0.62% | PASS |
| mem_writes | `oltp_update_index` | 50.27ms | 104.11ms | 2.071× | 1.74% | PASS |
| mem_writes | `oltp_update_non_index` | 33.90ms | 59.26ms | 1.748× | 1.92% | PASS |
| mem_writes | `oltp_delete_insert` | 45.91ms | 79.97ms | 1.742× | 1.78% | PASS |
| mem_writes | `oltp_write_only` | 21.96ms | 45.08ms | 2.053× | 1.70% | PASS |
| mem_writes | `types_delete_insert` | 25.26ms | 40.37ms | 1.598× | 1.25% | PASS |
| mem_writes | `oltp_read_write` | 73.84ms | 114.93ms | 1.556× | 3.06% | PASS |
| file_reads | `oltp_point_select` | 97.87ms | 55.34ms | 0.565× | 1.27% | PASS |
| file_reads | `oltp_range_select` | 18.34ms | 16.18ms | 0.882× | 2.36% | PASS |
| file_reads | `oltp_sum_range` | 17.47ms | 15.38ms | 0.880× | 2.48% | PASS |
| file_reads | `oltp_order_range` | 3.55ms | 3.46ms | 0.975× | 3.04% | PASS |
| file_reads | `oltp_distinct_range` | 4.56ms | 4.53ms | 0.995× | 2.79% | PASS |
| file_reads | `oltp_index_scan` | 11.54ms | 8.18ms | 0.709× | 2.08% | PASS |
| file_reads | `select_random_points` | 17.84ms | 14.15ms | 0.793× | 4.45% | PASS |
| file_reads | `select_random_ranges` | 10.44ms | 6.63ms | 0.635× | 1.63% | PASS |
| file_reads | `covering_index_scan` | 12.27ms | 6.88ms | 0.561× | 2.60% | PASS |
| file_reads | `groupby_scan` | 31.38ms | 34.30ms | 1.093× | 1.40% | PASS |
| file_reads | `index_join` | 10.37ms | 10.18ms | 0.982× | 2.06% | PASS |
| file_reads | `index_join_scan` | 4.47ms | 5.03ms | 1.127× | 2.23% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.33s | 1.267× | 1.89% | PASS |
| file_reads | `table_scan` | 1.16s | 1.40s | 1.198× | 1.15% | PASS |
| file_reads | `oltp_read_only` | 213.05ms | 163.05ms | 0.765× | 1.19% | PASS |
| file_writes | `oltp_bulk_insert` | 195.14ms | 271.84ms | 1.393× | 1.42% | PASS |
| file_writes | `oltp_insert` | 22.33ms | 35.71ms | 1.599× | 1.88% | PASS |
| file_writes | `oltp_update_index` | 77.24ms | 127.30ms | 1.648× | 1.72% | PASS |
| file_writes | `oltp_update_non_index` | 59.63ms | 81.78ms | 1.371× | 2.47% | PASS |
| file_writes | `oltp_delete_insert` | 69.06ms | 99.84ms | 1.446× | 2.23% | PASS |
| file_writes | `oltp_write_only` | 46.98ms | 65.61ms | 1.397× | 2.30% | PASS |
| file_writes | `types_delete_insert` | 40.92ms | 53.59ms | 1.310× | 2.02% | PASS |
| file_writes | `oltp_read_write` | 92.86ms | 130.96ms | 1.410× | 2.16% | PASS |
| ac_reads | `oltp_point_select` | 50.32ms | 55.84ms | 1.110× | 1.44% | PASS |
| ac_reads | `oltp_range_select` | 12.68ms | 16.00ms | 1.262× | 3.60% | PASS |
| ac_reads | `oltp_sum_range` | 12.19ms | 15.37ms | 1.261× | 2.94% | PASS |
| ac_reads | `oltp_order_range` | 2.96ms | 3.41ms | 1.155× | 2.38% | PASS |
| ac_reads | `oltp_distinct_range` | 3.98ms | 4.48ms | 1.125× | 2.72% | PASS |
| ac_reads | `oltp_index_scan` | 6.46ms | 8.22ms | 1.273× | 1.43% | PASS |
| ac_reads | `select_random_points` | 12.82ms | 14.16ms | 1.105× | 4.68% | PASS |
| ac_reads | `select_random_ranges` | 5.68ms | 6.69ms | 1.179× | 1.81% | PASS |
| ac_reads | `covering_index_scan` | 7.08ms | 7.01ms | 0.990× | 2.21% | PASS |
| ac_reads | `groupby_scan` | 30.02ms | 33.95ms | 1.131× | 0.82% | PASS |
| ac_reads | `index_join` | 7.79ms | 10.40ms | 1.334× | 2.53% | PASS |
| ac_reads | `index_join_scan` | 4.24ms | 5.20ms | 1.226× | 4.06% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.34s | 1.277× | 1.16% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.40s | 1.171× | 2.54% | PASS |
| ac_reads | `oltp_read_only` | 143.06ms | 163.81ms | 1.145× | 2.30% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.85ms | 81.51ms | 3.418× | 7.34% | PASS |
| ac_writes | `oltp_insert_ac` | 25.88ms | 102.25ms | 3.951× | 5.07% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.02ms | 117.44ms | 4.192× | 7.73% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.70ms | 92.11ms | 4.057× | 6.82% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.19ms | 104.67ms | 4.326× | 5.71% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.56ms | 103.10ms | 4.033× | 6.41% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.87ms | 91.19ms | 4.169× | 6.78% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.98ms | 110.14ms | 3.674× | 4.03% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.78ms | 37.88ms | 1.231× | 1.94% | PASS |
| mem_reads | `oltp_range_select` | 13.91ms | 14.35ms | 1.032× | 4.48% | PASS |
| mem_reads | `oltp_sum_range` | 12.90ms | 14.35ms | 1.112× | 2.37% | PASS |
| mem_reads | `oltp_order_range` | 3.08ms | 3.21ms | 1.042× | 1.48% | PASS |
| mem_reads | `oltp_distinct_range` | 4.09ms | 4.28ms | 1.045× | 1.01% | PASS |
| mem_reads | `oltp_index_scan` | 4.86ms | 6.61ms | 1.360× | 1.95% | PASS |
| mem_reads | `select_random_points` | 19.70ms | 21.75ms | 1.104× | 2.11% | PASS |
| mem_reads | `select_random_ranges` | 4.22ms | 5.25ms | 1.243× | 2.55% | PASS |
| mem_reads | `covering_index_scan` | 4.51ms | 4.44ms | 0.985× | 1.73% | PASS |
| mem_reads | `groupby_scan` | 31.91ms | 34.23ms | 1.073× | 1.00% | PASS |
| mem_reads | `index_join` | 7.11ms | 9.39ms | 1.321× | 2.75% | PASS |
| mem_reads | `index_join_scan` | 4.82ms | 5.48ms | 1.137× | 4.03% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.23s | 1.167× | 0.80% | PASS |
| mem_reads | `table_scan` | 1.24s | 1.38s | 1.115× | 1.22% | PASS |
| mem_reads | `oltp_read_only` | 118.56ms | 136.77ms | 1.154× | 1.74% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.59ms | 357.23ms | 1.510× | 1.04% | PASS |
| mem_writes | `oltp_insert` | 21.83ms | 39.59ms | 1.813× | 1.04% | PASS |
| mem_writes | `oltp_update_index` | 71.41ms | 132.16ms | 1.851× | 1.04% | PASS |
| mem_writes | `oltp_update_non_index` | 49.42ms | 86.94ms | 1.759× | 1.32% | PASS |
| mem_writes | `oltp_delete_insert` | 51.82ms | 102.94ms | 1.987× | 1.00% | PASS |
| mem_writes | `oltp_write_only` | 28.95ms | 61.29ms | 2.117× | 1.02% | PASS |
| mem_writes | `types_delete_insert` | 33.83ms | 54.42ms | 1.609× | 1.38% | PASS |
| mem_writes | `oltp_read_write` | 84.88ms | 139.25ms | 1.640× | 1.15% | PASS |
| file_reads | `oltp_point_select` | 103.80ms | 63.72ms | 0.614× | 0.99% | PASS |
| file_reads | `oltp_range_select` | 20.67ms | 17.38ms | 0.841× | 1.19% | PASS |
| file_reads | `oltp_sum_range` | 19.65ms | 17.39ms | 0.885× | 0.96% | PASS |
| file_reads | `oltp_order_range` | 3.71ms | 3.52ms | 0.947× | 2.15% | PASS |
| file_reads | `oltp_distinct_range` | 4.77ms | 4.64ms | 0.972× | 1.20% | PASS |
| file_reads | `oltp_index_scan` | 11.97ms | 9.16ms | 0.766× | 1.56% | PASS |
| file_reads | `select_random_points` | 26.26ms | 24.92ms | 0.949× | 1.50% | PASS |
| file_reads | `select_random_ranges` | 11.44ms | 7.93ms | 0.693× | 1.41% | PASS |
| file_reads | `covering_index_scan` | 13.83ms | 7.52ms | 0.544× | 2.79% | PASS |
| file_reads | `groupby_scan` | 33.99ms | 34.96ms | 1.029× | 0.86% | PASS |
| file_reads | `index_join` | 12.83ms | 11.68ms | 0.910× | 2.98% | PASS |
| file_reads | `index_join_scan` | 6.14ms | 6.05ms | 0.985× | 4.89% | PASS |
| file_reads | `types_table_scan` | 1.11s | 1.25s | 1.131× | 3.11% | PASS |
| file_reads | `table_scan` | 1.27s | 1.38s | 1.084× | 2.95% | PASS |
| file_reads | `oltp_read_only` | 225.84ms | 175.50ms | 0.777× | 0.99% | PASS |
| file_writes | `oltp_bulk_insert` | 258.30ms | 386.33ms | 1.496× | 0.85% | PASS |
| file_writes | `oltp_insert` | 58.27ms | 53.17ms | 0.913× | 27.28% | PASS |
| file_writes | `oltp_update_index` | 115.59ms | 171.75ms | 1.486× | 1.62% | PASS |
| file_writes | `oltp_update_non_index` | 95.10ms | 114.71ms | 1.206× | 9.77% | PASS |
| file_writes | `oltp_delete_insert` | 95.97ms | 137.88ms | 1.437× | 1.77% | PASS |
| file_writes | `oltp_write_only` | 89.47ms | 87.06ms | 0.973× | 8.20% | PASS |
| file_writes | `types_delete_insert` | 58.42ms | 76.93ms | 1.317× | 1.92% | PASS |
| file_writes | `oltp_read_write` | 140.15ms | 167.37ms | 1.194× | 6.85% | PASS |
| ac_reads | `oltp_point_select` | 55.60ms | 63.60ms | 1.144× | 1.28% | PASS |
| ac_reads | `oltp_range_select` | 16.30ms | 17.28ms | 1.060× | 2.04% | PASS |
| ac_reads | `oltp_sum_range` | 15.68ms | 17.20ms | 1.097× | 2.47% | PASS |
| ac_reads | `oltp_order_range` | 3.48ms | 3.55ms | 1.018× | 1.97% | PASS |
| ac_reads | `oltp_distinct_range` | 4.43ms | 4.67ms | 1.054× | 1.63% | PASS |
| ac_reads | `oltp_index_scan` | 7.44ms | 9.24ms | 1.242× | 2.81% | PASS |
| ac_reads | `select_random_points` | 21.22ms | 25.02ms | 1.179× | 1.67% | PASS |
| ac_reads | `select_random_ranges` | 6.67ms | 7.93ms | 1.189× | 2.09% | PASS |
| ac_reads | `covering_index_scan` | 8.38ms | 7.48ms | 0.892× | 2.41% | PASS |
| ac_reads | `groupby_scan` | 32.74ms | 34.82ms | 1.063× | 1.31% | PASS |
| ac_reads | `index_join` | 8.94ms | 11.39ms | 1.275× | 1.71% | PASS |
| ac_reads | `index_join_scan` | 5.33ms | 6.01ms | 1.127× | 3.04% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.23s | 1.168× | 1.03% | PASS |
| ac_reads | `table_scan` | 1.47s | 1.41s | 0.955× | 8.98% | PASS |
| ac_reads | `oltp_read_only` | 165.68ms | 179.03ms | 1.081× | 2.50% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.32ms | 84.35ms | 3.616× | 5.81% | PASS |
| ac_writes | `oltp_insert_ac` | 26.50ms | 100.48ms | 3.792× | 5.70% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.57ms | 119.69ms | 4.190× | 4.24% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.03ms | 96.51ms | 4.191× | 6.02% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.62ms | 107.42ms | 4.194× | 7.63% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.07ms | 105.56ms | 4.210× | 6.95% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.25ms | 97.77ms | 4.206× | 5.96% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.09ms | 115.89ms | 3.612× | 4.27% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 19.19ms | 21.87ms | 1.140× | 3.07% | PASS |
| mem_reads | `oltp_range_select` | 8.04ms | 8.44ms | 1.049× | 3.73% | PASS |
| mem_reads | `oltp_sum_range` | 7.97ms | 8.41ms | 1.055× | 3.67% | PASS |
| mem_reads | `oltp_order_range` | 1.89ms | 1.95ms | 1.032× | 3.07% | PASS |
| mem_reads | `oltp_distinct_range` | 2.44ms | 2.46ms | 1.009× | 3.08% | PASS |
| mem_reads | `oltp_index_scan` | 2.77ms | 3.83ms | 1.383× | 3.26% | PASS |
| mem_reads | `select_random_points` | 11.15ms | 13.41ms | 1.202× | 2.49% | PASS |
| mem_reads | `select_random_ranges` | 2.74ms | 3.42ms | 1.248× | 5.53% | PASS |
| mem_reads | `covering_index_scan` | 2.56ms | 2.74ms | 1.073× | 2.08% | PASS |
| mem_reads | `groupby_scan` | 19.24ms | 19.51ms | 1.014× | 1.41% | PASS |
| mem_reads | `index_join` | 4.44ms | 5.53ms | 1.246× | 1.44% | PASS |
| mem_reads | `index_join_scan` | 2.48ms | 4.45ms | 1.793× | 6.13% | PASS |
| mem_reads | `types_table_scan` | 702.97ms | 754.06ms | 1.073× | 0.68% | PASS |
| mem_reads | `table_scan` | 804.06ms | 847.78ms | 1.054× | 0.74% | PASS |
| mem_reads | `oltp_read_only` | 67.96ms | 72.87ms | 1.072× | 1.68% | PASS |
| mem_writes | `oltp_bulk_insert` | 148.78ms | 204.71ms | 1.376× | 0.73% | PASS |
| mem_writes | `oltp_insert` | 12.25ms | 24.66ms | 2.012× | 0.76% | PASS |
| mem_writes | `oltp_update_index` | 42.82ms | 85.36ms | 1.993× | 1.14% | PASS |
| mem_writes | `oltp_update_non_index` | 31.73ms | 56.50ms | 1.781× | 0.96% | PASS |
| mem_writes | `oltp_delete_insert` | 30.77ms | 67.15ms | 2.182× | 1.58% | PASS |
| mem_writes | `oltp_write_only` | 17.67ms | 42.11ms | 2.384× | 1.47% | PASS |
| mem_writes | `types_delete_insert` | 20.54ms | 34.67ms | 1.688× | 2.74% | PASS |
| mem_writes | `oltp_read_write` | 48.60ms | 81.20ms | 1.671× | 1.57% | PASS |
| file_reads | `oltp_point_select` | 75.91ms | 41.81ms | 0.551× | 0.78% | PASS |
| file_reads | `oltp_range_select` | 14.50ms | 10.51ms | 0.725× | 1.47% | PASS |
| file_reads | `oltp_sum_range` | 14.52ms | 10.39ms | 0.715× | 1.16% | PASS |
| file_reads | `oltp_order_range` | 2.74ms | 2.20ms | 0.802× | 1.21% | PASS |
| file_reads | `oltp_distinct_range` | 3.28ms | 2.69ms | 0.822× | 0.96% | PASS |
| file_reads | `oltp_index_scan` | 9.21ms | 6.10ms | 0.662× | 1.70% | PASS |
| file_reads | `select_random_points` | 17.91ms | 15.06ms | 0.841× | 1.40% | PASS |
| file_reads | `select_random_ranges` | 8.81ms | 5.52ms | 0.626× | 1.08% | PASS |
| file_reads | `covering_index_scan` | 9.34ms | 5.00ms | 0.535× | 2.20% | PASS |
| file_reads | `groupby_scan` | 20.34ms | 19.89ms | 0.978× | 1.03% | PASS |
| file_reads | `index_join` | 8.23ms | 7.44ms | 0.904× | 2.58% | PASS |
| file_reads | `index_join_scan` | 3.88ms | 4.43ms | 1.142× | 1.37% | PASS |
| file_reads | `types_table_scan` | 697.99ms | 749.98ms | 1.074× | 0.68% | PASS |
| file_reads | `table_scan` | 802.58ms | 842.36ms | 1.050× | 0.88% | PASS |
| file_reads | `oltp_read_only` | 149.16ms | 101.25ms | 0.679× | 0.90% | PASS |
| file_writes | `oltp_bulk_insert` | 200.05ms | 283.22ms | 1.416× | 7.42% | PASS |
| file_writes | `oltp_insert` | 36.97ms | 53.04ms | 1.434× | 16.54% | PASS |
| file_writes | `oltp_update_index` | 169.01ms | 176.89ms | 1.047× | 13.26% | PASS |
| file_writes | `oltp_update_non_index` | 133.31ms | 122.06ms | 0.916× | 5.30% | PASS |
| file_writes | `oltp_delete_insert` | 135.53ms | 135.59ms | 1.000× | 9.56% | PASS |
| file_writes | `oltp_write_only` | 99.91ms | 96.94ms | 0.970× | 12.35% | PASS |
| file_writes | `types_delete_insert` | 93.87ms | 88.21ms | 0.940× | 22.49% | PASS |
| file_writes | `oltp_read_write` | 136.54ms | 137.61ms | 1.008× | 13.33% | PASS |
| ac_reads | `oltp_point_select` | 38.28ms | 41.90ms | 1.095× | 0.92% | PASS |
| ac_reads | `oltp_range_select` | 10.47ms | 10.44ms | 0.997× | 1.19% | PASS |
| ac_reads | `oltp_sum_range` | 10.51ms | 10.39ms | 0.988× | 1.38% | PASS |
| ac_reads | `oltp_order_range` | 2.27ms | 2.19ms | 0.965× | 1.90% | PASS |
| ac_reads | `oltp_distinct_range` | 2.83ms | 2.69ms | 0.951× | 1.18% | PASS |
| ac_reads | `oltp_index_scan` | 5.13ms | 6.10ms | 1.188× | 2.84% | PASS |
| ac_reads | `select_random_points` | 14.15ms | 15.06ms | 1.065× | 1.74% | PASS |
| ac_reads | `select_random_ranges` | 4.93ms | 5.50ms | 1.116× | 1.56% | PASS |
| ac_reads | `covering_index_scan` | 4.87ms | 5.00ms | 1.027× | 2.63% | PASS |
| ac_reads | `groupby_scan` | 19.70ms | 19.74ms | 1.002× | 0.93% | PASS |
| ac_reads | `index_join` | 5.92ms | 7.32ms | 1.236× | 3.92% | PASS |
| ac_reads | `index_join_scan` | 3.51ms | 4.42ms | 1.257× | 2.46% | PASS |
| ac_reads | `types_table_scan` | 693.15ms | 746.79ms | 1.077× | 0.66% | PASS |
| ac_reads | `table_scan` | 793.46ms | 836.23ms | 1.054× | 0.64% | PASS |
| ac_reads | `oltp_read_only` | 94.41ms | 100.35ms | 1.063× | 0.47% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 38.76ms | 127.61ms | 3.292× | 46.41% | PASS |
| ac_writes | `oltp_insert_ac` | 38.95ms | 145.19ms | 3.728× | 23.37% | PASS |
| ac_writes | `oltp_update_index_ac` | 37.87ms | 140.96ms | 3.722× | 31.14% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 56.64ms | 250.06ms | 4.415× | 63.24% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 33.51ms | 120.23ms | 3.588× | 24.41% | PASS |
| ac_writes | `oltp_write_only_ac` | 32.45ms | 100.38ms | 3.093× | 13.85% | PASS |
| ac_writes | `types_delete_insert_ac` | 30.37ms | 99.79ms | 3.286× | 17.34% | PASS |
| ac_writes | `oltp_read_write_ac` | 36.88ms | 119.24ms | 3.233× | 15.71% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.89ms | 37.83ms | 1.116× | 1.29% | PASS |
| mem_reads | `oltp_range_select` | 19.99ms | 20.18ms | 1.009× | 1.15% | PASS |
| mem_reads | `oltp_sum_range` | 18.40ms | 19.20ms | 1.043× | 0.81% | PASS |
| mem_reads | `oltp_order_range` | 3.71ms | 3.77ms | 1.016× | 0.56% | PASS |
| mem_reads | `oltp_distinct_range` | 4.91ms | 4.93ms | 1.003× | 1.00% | PASS |
| mem_reads | `oltp_index_scan` | 4.69ms | 5.82ms | 1.240× | 2.58% | PASS |
| mem_reads | `select_random_points` | 27.30ms | 30.00ms | 1.099× | 0.87% | PASS |
| mem_reads | `select_random_ranges` | 7.51ms | 8.17ms | 1.088× | 1.35% | PASS |
| mem_reads | `covering_index_scan` | 4.37ms | 4.16ms | 0.952× | 1.80% | PASS |
| mem_reads | `groupby_scan` | 38.61ms | 39.93ms | 1.034× | 0.51% | PASS |
| mem_reads | `index_join` | 8.09ms | 9.90ms | 1.223× | 0.97% | PASS |
| mem_reads | `index_join_scan` | 4.25ms | 5.61ms | 1.320× | 1.28% | PASS |
| mem_reads | `types_table_scan` | 1.13s | 1.30s | 1.155× | 0.83% | PASS |
| mem_reads | `table_scan` | 1.31s | 1.43s | 1.089× | 1.30% | PASS |
| mem_reads | `oltp_read_only` | 150.46ms | 158.97ms | 1.057× | 0.70% | PASS |
| mem_writes | `oltp_bulk_insert` | 246.52ms | 339.26ms | 1.376× | 0.86% | PASS |
| mem_writes | `oltp_insert` | 19.58ms | 36.33ms | 1.856× | 0.66% | PASS |
| mem_writes | `oltp_update_index` | 70.08ms | 121.95ms | 1.740× | 1.12% | PASS |
| mem_writes | `oltp_update_non_index` | 52.88ms | 85.27ms | 1.613× | 1.26% | PASS |
| mem_writes | `oltp_delete_insert` | 50.80ms | 97.35ms | 1.916× | 1.16% | PASS |
| mem_writes | `oltp_write_only` | 27.18ms | 58.64ms | 2.158× | 0.74% | PASS |
| mem_writes | `types_delete_insert` | 33.09ms | 54.45ms | 1.645× | 0.91% | PASS |
| mem_writes | `oltp_read_write` | 101.92ms | 149.55ms | 1.467× | 1.44% | PASS |
| file_reads | `oltp_point_select` | 128.55ms | 69.61ms | 0.541× | 1.36% | PASS |
| file_reads | `oltp_range_select` | 29.54ms | 23.40ms | 0.792× | 2.89% | PASS |
| file_reads | `oltp_sum_range` | 27.92ms | 22.46ms | 0.804× | 1.60% | PASS |
| file_reads | `oltp_order_range` | 4.68ms | 4.18ms | 0.894× | 3.33% | PASS |
| file_reads | `oltp_distinct_range` | 5.67ms | 5.27ms | 0.930× | 2.71% | PASS |
| file_reads | `oltp_index_scan` | 14.43ms | 9.14ms | 0.634× | 2.00% | PASS |
| file_reads | `select_random_points` | 36.32ms | 33.61ms | 0.925× | 2.02% | PASS |
| file_reads | `select_random_ranges` | 17.30ms | 11.62ms | 0.672× | 1.39% | PASS |
| file_reads | `covering_index_scan` | 14.20ms | 7.56ms | 0.533× | 1.59% | PASS |
| file_reads | `groupby_scan` | 39.51ms | 40.65ms | 1.029× | 0.81% | PASS |
| file_reads | `index_join` | 13.17ms | 12.34ms | 0.937× | 1.18% | PASS |
| file_reads | `index_join_scan` | 5.18ms | 6.03ms | 1.164× | 1.31% | PASS |
| file_reads | `types_table_scan` | 1.11s | 1.30s | 1.164× | 0.63% | PASS |
| file_reads | `table_scan` | 1.28s | 1.42s | 1.103× | 0.56% | PASS |
| file_reads | `oltp_read_only` | 287.73ms | 205.84ms | 0.715× | 0.64% | PASS |
| file_writes | `oltp_bulk_insert` | 262.61ms | 361.46ms | 1.376× | 0.81% | PASS |
| file_writes | `oltp_insert` | 26.36ms | 46.50ms | 1.764× | 0.88% | PASS |
| file_writes | `oltp_update_index` | 97.98ms | 143.65ms | 1.466× | 1.30% | PASS |
| file_writes | `oltp_update_non_index` | 77.76ms | 105.02ms | 1.351× | 1.41% | PASS |
| file_writes | `oltp_delete_insert` | 76.89ms | 119.64ms | 1.556× | 1.96% | PASS |
| file_writes | `oltp_write_only` | 50.48ms | 79.27ms | 1.570× | 1.31% | PASS |
| file_writes | `types_delete_insert` | 49.28ms | 66.82ms | 1.356× | 1.63% | PASS |
| file_writes | `oltp_read_write` | 120.60ms | 167.02ms | 1.385× | 1.44% | PASS |
| ac_reads | `oltp_point_select` | 64.01ms | 69.62ms | 1.088× | 0.78% | PASS |
| ac_reads | `oltp_range_select` | 22.89ms | 23.34ms | 1.020× | 0.56% | PASS |
| ac_reads | `oltp_sum_range` | 21.20ms | 22.44ms | 1.058× | 0.78% | PASS |
| ac_reads | `oltp_order_range` | 4.09ms | 4.18ms | 1.022× | 0.99% | PASS |
| ac_reads | `oltp_distinct_range` | 5.18ms | 5.26ms | 1.015× | 1.22% | PASS |
| ac_reads | `oltp_index_scan` | 7.90ms | 9.12ms | 1.156× | 1.62% | PASS |
| ac_reads | `select_random_points` | 30.42ms | 33.63ms | 1.106× | 0.57% | PASS |
| ac_reads | `select_random_ranges` | 10.68ms | 11.57ms | 1.083× | 0.88% | PASS |
| ac_reads | `covering_index_scan` | 7.66ms | 7.54ms | 0.984× | 0.86% | PASS |
| ac_reads | `groupby_scan` | 38.73ms | 40.64ms | 1.049× | 0.58% | PASS |
| ac_reads | `index_join` | 9.95ms | 12.25ms | 1.231× | 0.88% | PASS |
| ac_reads | `index_join_scan` | 4.66ms | 6.04ms | 1.294× | 1.09% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.30s | 1.167× | 0.41% | PASS |
| ac_reads | `table_scan` | 1.28s | 1.41s | 1.104× | 0.42% | PASS |
| ac_reads | `oltp_read_only` | 195.51ms | 206.05ms | 1.054× | 0.55% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.84ms | 61.99ms | 3.913× | 3.01% | PASS |
| ac_writes | `oltp_insert_ac` | 17.89ms | 82.46ms | 4.610× | 2.93% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.59ms | 94.49ms | 4.825× | 2.59% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.21ms | 73.11ms | 4.509× | 3.55% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.57ms | 85.00ms | 4.838× | 3.49% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.09ms | 85.93ms | 4.750× | 3.97% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.52ms | 73.23ms | 4.718× | 2.70% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.83ms | 93.33ms | 3.759× | 2.67% | PASS |

</details>

## Version-control latency

Wall time: 2m 32s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 94.46ms | 200.00ms | 47.2% | 0.41% | PASS |
| `status_dirty_many_tables` | 98.04ms | 200.00ms | 49.0% | 0.30% | PASS |
| `diff_regular_working_one_table` | 90.10ms | 150.00ms | 60.1% | 0.27% | PASS |
| `diff_regular_working_many_tables` | 103.61ms | 200.00ms | 51.8% | 0.24% | PASS |
| `diff_stat_working_many_tables` | 104.05ms | 200.00ms | 52.0% | 0.40% | PASS |
| `diff_schema_working_many_tables` | 104.18ms | 200.00ms | 52.1% | 0.24% | PASS |
| `branch_list_many_branches` | 25.01ms | 100.00ms | 25.0% | 0.84% | PASS |
| `branch_create_delete` | 26.99ms | 100.00ms | 27.0% | 0.83% | PASS |
| `checkout_branch_clean` | 60.58ms | 200.00ms | 30.3% | 0.54% | PASS |
| `merge_data_no_conflicts` | 31.60ms | 150.00ms | 21.1% | 0.95% | PASS |
| `merge_schema_no_conflicts` | 23.54ms | 100.00ms | 23.5% | 0.83% | PASS |
| `merge_data_conflicts` | 130.20ms | 250.00ms | 52.1% | 0.18% | PASS |
| `merge_data_conflicts_with_resolve` | 129.81ms | 250.00ms | 51.9% | 0.15% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
