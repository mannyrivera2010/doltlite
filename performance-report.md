# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-20 16:02 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261018.339.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/39439229829)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 6m 52s | 7.71s | 8.97s | 1.164× | 0.95% | **PASS** |
| textpk | 69 | 55 | 1h 32m 40s | 9.93s | 11.81s | 1.190× | 1.41% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 45s | 9.43s | 11.71s | 1.241× | 1.65% | **PASS** |
| compositepk | 69 | 55 | 1h 30m 45s | 10.61s | 12.34s | 1.162× | 1.91% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 19.52ms | 21.63ms | 1.108× | 1.32% | PASS |
| mem_reads | `oltp_range_select` | 8.45ms | 9.21ms | 1.091× | 0.87% | PASS |
| mem_reads | `oltp_sum_range` | 7.73ms | 8.90ms | 1.151× | 1.55% | PASS |
| mem_reads | `oltp_order_range` | 2.13ms | 2.28ms | 1.073× | 0.99% | PASS |
| mem_reads | `oltp_distinct_range` | 2.95ms | 3.04ms | 1.030× | 0.88% | PASS |
| mem_reads | `oltp_index_scan` | 3.17ms | 3.84ms | 1.213× | 1.78% | PASS |
| mem_reads | `select_random_points` | 8.43ms | 8.72ms | 1.035× | 0.94% | PASS |
| mem_reads | `select_random_ranges` | 2.48ms | 3.11ms | 1.252× | 0.83% | PASS |
| mem_reads | `covering_index_scan` | 3.41ms | 3.15ms | 0.924× | 1.23% | PASS |
| mem_reads | `groupby_scan` | 24.77ms | 27.07ms | 1.093× | 0.75% | PASS |
| mem_reads | `index_join` | 4.69ms | 6.01ms | 1.282× | 1.02% | PASS |
| mem_reads | `index_join_scan` | 2.85ms | 3.79ms | 1.330× | 1.66% | PASS |
| mem_reads | `types_table_scan` | 865.60ms | 984.09ms | 1.137× | 0.68% | PASS |
| mem_reads | `table_scan` | 974.35ms | 1.07s | 1.094× | 0.48% | PASS |
| mem_reads | `oltp_read_only` | 80.48ms | 89.37ms | 1.111× | 0.49% | PASS |
| mem_writes | `oltp_bulk_insert` | 140.13ms | 194.62ms | 1.389× | 0.93% | PASS |
| mem_writes | `oltp_insert` | 12.34ms | 22.39ms | 1.815× | 0.82% | PASS |
| mem_writes | `oltp_update_index` | 40.61ms | 85.81ms | 2.113× | 0.64% | PASS |
| mem_writes | `oltp_update_non_index` | 28.48ms | 48.95ms | 1.719× | 1.55% | PASS |
| mem_writes | `oltp_delete_insert` | 35.58ms | 64.25ms | 1.806× | 0.68% | PASS |
| mem_writes | `oltp_write_only` | 17.75ms | 37.56ms | 2.115× | 0.97% | PASS |
| mem_writes | `types_delete_insert` | 19.89ms | 33.66ms | 1.692× | 0.94% | PASS |
| mem_writes | `oltp_read_write` | 51.29ms | 83.88ms | 1.635× | 0.58% | PASS |
| file_reads | `oltp_point_select` | 88.04ms | 44.70ms | 0.508× | 0.58% | PASS |
| file_reads | `oltp_range_select` | 15.69ms | 11.74ms | 0.748× | 0.78% | PASS |
| file_reads | `oltp_sum_range` | 14.89ms | 11.47ms | 0.770× | 0.71% | PASS |
| file_reads | `oltp_order_range` | 2.93ms | 2.58ms | 0.881× | 1.28% | PASS |
| file_reads | `oltp_distinct_range` | 3.75ms | 3.35ms | 0.894× | 1.04% | PASS |
| file_reads | `oltp_index_scan` | 10.40ms | 6.69ms | 0.643× | 0.99% | PASS |
| file_reads | `select_random_points` | 16.16ms | 11.71ms | 0.724× | 1.78% | PASS |
| file_reads | `select_random_ranges` | 9.49ms | 5.53ms | 0.582× | 0.96% | PASS |
| file_reads | `covering_index_scan` | 10.75ms | 5.95ms | 0.553× | 0.96% | PASS |
| file_reads | `groupby_scan` | 26.00ms | 27.65ms | 1.063× | 0.76% | PASS |
| file_reads | `index_join` | 8.75ms | 8.12ms | 0.929× | 0.95% | PASS |
| file_reads | `index_join_scan` | 3.64ms | 4.14ms | 1.137× | 1.45% | PASS |
| file_reads | `types_table_scan` | 853.69ms | 981.94ms | 1.150× | 0.49% | PASS |
| file_reads | `table_scan` | 1.04s | 1.09s | 1.047× | 2.41% | PASS |
| file_reads | `oltp_read_only` | 184.73ms | 124.41ms | 0.673× | 0.57% | PASS |
| file_writes | `oltp_bulk_insert` | 181.25ms | 250.04ms | 1.380× | 4.38% | PASS |
| file_writes | `oltp_insert` | 19.91ms | 37.80ms | 1.898× | 4.15% | PASS |
| file_writes | `oltp_update_index` | 132.12ms | 149.64ms | 1.133× | 4.91% | PASS |
| file_writes | `oltp_update_non_index` | 111.29ms | 106.90ms | 0.961× | 6.99% | PASS |
| file_writes | `oltp_delete_insert` | 111.62ms | 128.36ms | 1.150× | 4.08% | PASS |
| file_writes | `oltp_write_only` | 91.17ms | 82.06ms | 0.900× | 6.34% | PASS |
| file_writes | `types_delete_insert` | 62.59ms | 68.37ms | 1.092× | 14.07% | PASS |
| file_writes | `oltp_read_write` | 121.30ms | 129.27ms | 1.066× | 2.85% | PASS |
| ac_reads | `oltp_point_select` | 42.61ms | 44.65ms | 1.048× | 0.70% | PASS |
| ac_reads | `oltp_range_select` | 11.22ms | 11.74ms | 1.047× | 0.78% | PASS |
| ac_reads | `oltp_sum_range` | 10.38ms | 11.46ms | 1.104× | 0.82% | PASS |
| ac_reads | `oltp_order_range` | 2.53ms | 2.57ms | 1.017× | 0.71% | PASS |
| ac_reads | `oltp_distinct_range` | 3.29ms | 3.34ms | 1.016× | 0.76% | PASS |
| ac_reads | `oltp_index_scan` | 5.86ms | 6.67ms | 1.137× | 0.74% | PASS |
| ac_reads | `select_random_points` | 11.46ms | 11.61ms | 1.013× | 0.82% | PASS |
| ac_reads | `select_random_ranges` | 4.96ms | 5.54ms | 1.117× | 0.68% | PASS |
| ac_reads | `covering_index_scan` | 6.14ms | 5.89ms | 0.959× | 0.86% | PASS |
| ac_reads | `groupby_scan` | 25.21ms | 27.46ms | 1.089× | 0.61% | PASS |
| ac_reads | `index_join` | 6.38ms | 8.05ms | 1.262× | 0.58% | PASS |
| ac_reads | `index_join_scan` | 3.24ms | 4.15ms | 1.281× | 1.39% | PASS |
| ac_reads | `types_table_scan` | 856.76ms | 981.50ms | 1.146× | 0.69% | PASS |
| ac_reads | `table_scan` | 972.20ms | 1.07s | 1.098× | 0.72% | PASS |
| ac_reads | `oltp_read_only` | 114.23ms | 122.35ms | 1.071× | 0.72% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.73ms | 50.68ms | 3.029× | 10.63% | PASS |
| ac_writes | `oltp_insert_ac` | 18.55ms | 71.63ms | 3.861× | 14.40% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.80ms | 78.59ms | 3.969× | 7.62% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.35ms | 63.94ms | 3.686× | 15.97% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.43ms | 75.11ms | 4.074× | 14.60% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.81ms | 68.63ms | 3.648× | 10.75% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.25ms | 64.65ms | 3.979× | 9.43% | PASS |
| ac_writes | `oltp_read_write_ac` | 22.40ms | 75.58ms | 3.374× | 5.97% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.28ms | 37.49ms | 1.280× | 0.82% | PASS |
| mem_reads | `oltp_range_select` | 12.82ms | 14.32ms | 1.117× | 1.54% | PASS |
| mem_reads | `oltp_sum_range` | 11.64ms | 14.31ms | 1.229× | 1.41% | PASS |
| mem_reads | `oltp_order_range` | 2.85ms | 3.16ms | 1.107× | 0.98% | PASS |
| mem_reads | `oltp_distinct_range` | 3.95ms | 4.23ms | 1.069× | 0.79% | PASS |
| mem_reads | `oltp_index_scan` | 4.42ms | 6.25ms | 1.417× | 1.32% | PASS |
| mem_reads | `select_random_points` | 17.14ms | 20.69ms | 1.207× | 1.61% | PASS |
| mem_reads | `select_random_ranges` | 3.82ms | 5.20ms | 1.360× | 1.96% | PASS |
| mem_reads | `covering_index_scan` | 4.49ms | 4.47ms | 0.995× | 1.47% | PASS |
| mem_reads | `groupby_scan` | 31.27ms | 34.46ms | 1.102× | 0.81% | PASS |
| mem_reads | `index_join` | 6.86ms | 8.94ms | 1.303× | 1.04% | PASS |
| mem_reads | `index_join_scan` | 4.53ms | 5.31ms | 1.172× | 1.01% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.23s | 1.174× | 0.77% | PASS |
| mem_reads | `table_scan` | 1.21s | 1.37s | 1.135× | 1.04% | PASS |
| mem_reads | `oltp_read_only` | 116.16ms | 136.54ms | 1.175× | 0.69% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.13ms | 358.02ms | 1.523× | 0.94% | PASS |
| mem_writes | `oltp_insert` | 21.49ms | 39.59ms | 1.843× | 0.95% | PASS |
| mem_writes | `oltp_update_index` | 70.06ms | 132.42ms | 1.890× | 0.76% | PASS |
| mem_writes | `oltp_update_non_index` | 47.83ms | 86.31ms | 1.805× | 0.68% | PASS |
| mem_writes | `oltp_delete_insert` | 50.23ms | 103.14ms | 2.054× | 0.74% | PASS |
| mem_writes | `oltp_write_only` | 28.64ms | 61.41ms | 2.145× | 0.60% | PASS |
| mem_writes | `types_delete_insert` | 32.57ms | 54.97ms | 1.688× | 1.15% | PASS |
| mem_writes | `oltp_read_write` | 84.57ms | 139.81ms | 1.653× | 1.48% | PASS |
| file_reads | `oltp_point_select` | 104.40ms | 63.60ms | 0.609× | 0.88% | PASS |
| file_reads | `oltp_range_select` | 20.80ms | 17.09ms | 0.822× | 2.70% | PASS |
| file_reads | `oltp_sum_range` | 19.66ms | 17.06ms | 0.868× | 1.48% | PASS |
| file_reads | `oltp_order_range` | 3.77ms | 3.54ms | 0.938× | 3.19% | PASS |
| file_reads | `oltp_distinct_range` | 4.94ms | 4.64ms | 0.939× | 1.89% | PASS |
| file_reads | `oltp_index_scan` | 12.24ms | 9.22ms | 0.753× | 2.08% | PASS |
| file_reads | `select_random_points` | 26.63ms | 24.47ms | 0.919× | 1.52% | PASS |
| file_reads | `select_random_ranges` | 11.68ms | 7.93ms | 0.679× | 0.96% | PASS |
| file_reads | `covering_index_scan` | 12.90ms | 7.48ms | 0.579× | 3.11% | PASS |
| file_reads | `groupby_scan` | 32.99ms | 35.05ms | 1.062× | 1.26% | PASS |
| file_reads | `index_join` | 11.84ms | 11.29ms | 0.954× | 4.08% | PASS |
| file_reads | `index_join_scan` | 5.59ms | 5.85ms | 1.046× | 1.28% | PASS |
| file_reads | `types_table_scan` | 1.09s | 1.25s | 1.145× | 3.66% | PASS |
| file_reads | `table_scan` | 1.26s | 1.39s | 1.103× | 4.88% | PASS |
| file_reads | `oltp_read_only` | 228.36ms | 175.93ms | 0.770× | 1.06% | PASS |
| file_writes | `oltp_bulk_insert` | 254.31ms | 386.96ms | 1.522× | 0.84% | PASS |
| file_writes | `oltp_insert` | 46.01ms | 52.38ms | 1.138× | 17.64% | PASS |
| file_writes | `oltp_update_index` | 113.22ms | 168.62ms | 1.489× | 1.67% | PASS |
| file_writes | `oltp_update_non_index` | 97.45ms | 112.42ms | 1.154× | 11.31% | PASS |
| file_writes | `oltp_delete_insert` | 90.69ms | 132.91ms | 1.466× | 1.56% | PASS |
| file_writes | `oltp_write_only` | 93.21ms | 85.13ms | 0.913× | 11.13% | PASS |
| file_writes | `types_delete_insert` | 55.13ms | 74.50ms | 1.351× | 1.39% | PASS |
| file_writes | `oltp_read_write` | 143.38ms | 164.00ms | 1.144× | 3.71% | PASS |
| ac_reads | `oltp_point_select` | 54.65ms | 63.88ms | 1.169× | 0.84% | PASS |
| ac_reads | `oltp_range_select` | 16.15ms | 17.10ms | 1.059× | 1.57% | PASS |
| ac_reads | `oltp_sum_range` | 15.03ms | 16.97ms | 1.129× | 1.02% | PASS |
| ac_reads | `oltp_order_range` | 3.35ms | 3.51ms | 1.047× | 1.06% | PASS |
| ac_reads | `oltp_distinct_range` | 4.44ms | 4.60ms | 1.038× | 1.26% | PASS |
| ac_reads | `oltp_index_scan` | 7.41ms | 9.17ms | 1.238× | 1.40% | PASS |
| ac_reads | `select_random_points` | 20.99ms | 24.28ms | 1.157× | 1.20% | PASS |
| ac_reads | `select_random_ranges` | 6.78ms | 7.96ms | 1.174× | 0.97% | PASS |
| ac_reads | `covering_index_scan` | 8.07ms | 7.45ms | 0.923× | 1.86% | PASS |
| ac_reads | `groupby_scan` | 32.11ms | 34.90ms | 1.087× | 0.77% | PASS |
| ac_reads | `index_join` | 9.30ms | 11.23ms | 1.208× | 1.69% | PASS |
| ac_reads | `index_join_scan` | 5.07ms | 5.82ms | 1.147× | 1.07% | PASS |
| ac_reads | `types_table_scan` | 1.09s | 1.25s | 1.145× | 2.81% | PASS |
| ac_reads | `table_scan` | 1.47s | 1.40s | 0.956× | 1.89% | PASS |
| ac_reads | `oltp_read_only` | 161.83ms | 178.79ms | 1.105× | 0.99% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.02ms | 68.36ms | 2.969× | 5.60% | PASS |
| ac_writes | `oltp_insert_ac` | 25.82ms | 84.42ms | 3.269× | 6.53% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.31ms | 101.15ms | 3.451× | 6.37% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.68ms | 80.61ms | 3.554× | 6.11% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.83ms | 92.36ms | 3.576× | 7.63% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.07ms | 92.80ms | 3.560× | 6.21% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.22ms | 82.00ms | 3.531× | 6.52% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.44ms | 99.12ms | 3.055× | 4.63% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.69ms | 36.85ms | 1.201× | 1.52% | PASS |
| mem_reads | `oltp_range_select` | 12.90ms | 13.98ms | 1.084× | 2.40% | PASS |
| mem_reads | `oltp_sum_range` | 12.15ms | 13.96ms | 1.149× | 2.17% | PASS |
| mem_reads | `oltp_order_range` | 2.91ms | 3.15ms | 1.084× | 1.33% | PASS |
| mem_reads | `oltp_distinct_range` | 3.99ms | 4.23ms | 1.060× | 1.21% | PASS |
| mem_reads | `oltp_index_scan` | 4.55ms | 6.28ms | 1.382× | 1.58% | PASS |
| mem_reads | `select_random_points` | 18.08ms | 20.87ms | 1.154× | 1.69% | PASS |
| mem_reads | `select_random_ranges` | 4.05ms | 5.17ms | 1.276× | 1.74% | PASS |
| mem_reads | `covering_index_scan` | 4.42ms | 4.43ms | 1.002× | 1.97% | PASS |
| mem_reads | `groupby_scan` | 31.84ms | 34.15ms | 1.073× | 0.61% | PASS |
| mem_reads | `index_join` | 6.81ms | 9.10ms | 1.337× | 1.87% | PASS |
| mem_reads | `index_join_scan` | 4.38ms | 5.39ms | 1.230× | 1.97% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.23s | 1.184× | 1.04% | PASS |
| mem_reads | `table_scan` | 1.23s | 1.38s | 1.130× | 2.66% | PASS |
| mem_reads | `oltp_read_only` | 118.83ms | 135.36ms | 1.139× | 1.39% | PASS |
| mem_writes | `oltp_bulk_insert` | 241.95ms | 354.78ms | 1.466× | 1.14% | PASS |
| mem_writes | `oltp_insert` | 20.04ms | 39.27ms | 1.959× | 0.68% | PASS |
| mem_writes | `oltp_update_index` | 68.46ms | 129.21ms | 1.887× | 1.26% | PASS |
| mem_writes | `oltp_update_non_index` | 49.28ms | 84.21ms | 1.709× | 1.21% | PASS |
| mem_writes | `oltp_delete_insert` | 49.14ms | 101.93ms | 2.074× | 0.94% | PASS |
| mem_writes | `oltp_write_only` | 27.96ms | 61.21ms | 2.189× | 1.11% | PASS |
| mem_writes | `types_delete_insert` | 33.16ms | 53.85ms | 1.624× | 1.76% | PASS |
| mem_writes | `oltp_read_write` | 85.32ms | 138.29ms | 1.621× | 2.11% | PASS |
| file_reads | `oltp_point_select` | 106.58ms | 63.41ms | 0.595× | 1.12% | PASS |
| file_reads | `oltp_range_select` | 21.09ms | 16.92ms | 0.802× | 2.15% | PASS |
| file_reads | `oltp_sum_range` | 20.39ms | 16.89ms | 0.828× | 1.68% | PASS |
| file_reads | `oltp_order_range` | 3.86ms | 3.54ms | 0.918× | 2.03% | PASS |
| file_reads | `oltp_distinct_range` | 4.96ms | 4.64ms | 0.937× | 1.42% | PASS |
| file_reads | `oltp_index_scan` | 12.30ms | 9.23ms | 0.750× | 1.21% | PASS |
| file_reads | `select_random_points` | 27.27ms | 24.56ms | 0.901× | 1.98% | PASS |
| file_reads | `select_random_ranges` | 11.76ms | 7.86ms | 0.668× | 1.60% | PASS |
| file_reads | `covering_index_scan` | 12.19ms | 7.34ms | 0.602× | 1.59% | PASS |
| file_reads | `groupby_scan` | 32.91ms | 34.72ms | 1.055× | 0.81% | PASS |
| file_reads | `index_join` | 11.17ms | 11.32ms | 1.014× | 1.55% | PASS |
| file_reads | `index_join_scan` | 5.48ms | 5.79ms | 1.056× | 1.96% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.23s | 1.175× | 0.93% | PASS |
| file_reads | `table_scan` | 1.21s | 1.37s | 1.135× | 1.34% | PASS |
| file_reads | `oltp_read_only` | 230.90ms | 174.80ms | 0.757× | 1.18% | PASS |
| file_writes | `oltp_bulk_insert` | 262.22ms | 379.77ms | 1.448× | 1.03% | PASS |
| file_writes | `oltp_insert` | 31.81ms | 51.80ms | 1.628× | 1.57% | PASS |
| file_writes | `oltp_update_index` | 103.68ms | 163.71ms | 1.579× | 1.50% | PASS |
| file_writes | `oltp_update_non_index` | 80.10ms | 108.26ms | 1.352× | 1.57% | PASS |
| file_writes | `oltp_delete_insert` | 81.48ms | 130.66ms | 1.604× | 1.55% | PASS |
| file_writes | `oltp_write_only` | 55.56ms | 84.14ms | 1.514× | 1.90% | PASS |
| file_writes | `types_delete_insert` | 53.96ms | 72.08ms | 1.336× | 1.44% | PASS |
| file_writes | `oltp_read_write` | 116.92ms | 161.07ms | 1.378× | 1.96% | PASS |
| ac_reads | `oltp_point_select` | 56.31ms | 63.43ms | 1.126× | 0.73% | PASS |
| ac_reads | `oltp_range_select` | 16.28ms | 16.88ms | 1.037× | 2.22% | PASS |
| ac_reads | `oltp_sum_range` | 15.49ms | 16.80ms | 1.085× | 2.38% | PASS |
| ac_reads | `oltp_order_range` | 3.36ms | 3.56ms | 1.062× | 2.30% | PASS |
| ac_reads | `oltp_distinct_range` | 4.45ms | 4.64ms | 1.043× | 2.00% | PASS |
| ac_reads | `oltp_index_scan` | 7.38ms | 9.23ms | 1.251× | 2.60% | PASS |
| ac_reads | `select_random_points` | 22.49ms | 24.53ms | 1.091× | 1.81% | PASS |
| ac_reads | `select_random_ranges` | 6.93ms | 7.86ms | 1.134× | 1.65% | PASS |
| ac_reads | `covering_index_scan` | 7.58ms | 7.39ms | 0.976× | 4.49% | PASS |
| ac_reads | `groupby_scan` | 32.26ms | 34.83ms | 1.080× | 0.80% | PASS |
| ac_reads | `index_join` | 8.68ms | 11.31ms | 1.304× | 2.36% | PASS |
| ac_reads | `index_join_scan` | 4.94ms | 5.75ms | 1.164× | 1.67% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.22s | 1.179× | 0.85% | PASS |
| ac_reads | `table_scan` | 1.21s | 1.37s | 1.136× | 1.06% | PASS |
| ac_reads | `oltp_read_only` | 158.48ms | 175.17ms | 1.105× | 1.14% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.68ms | 71.09ms | 2.880× | 8.35% | PASS |
| ac_writes | `oltp_insert_ac` | 25.86ms | 92.25ms | 3.567× | 8.96% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.95ms | 104.80ms | 3.749× | 5.37% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.37ms | 82.95ms | 3.269× | 6.68% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.58ms | 93.51ms | 3.655× | 6.08% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.47ms | 94.38ms | 3.706× | 5.49% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.79ms | 86.78ms | 3.808× | 6.52% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.71ms | 102.28ms | 3.225× | 5.25% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 35.05ms | 41.45ms | 1.182× | 1.86% | PASS |
| mem_reads | `oltp_range_select` | 20.29ms | 21.71ms | 1.070× | 1.67% | PASS |
| mem_reads | `oltp_sum_range` | 18.84ms | 21.41ms | 1.136× | 2.06% | PASS |
| mem_reads | `oltp_order_range` | 3.64ms | 3.95ms | 1.086× | 1.36% | PASS |
| mem_reads | `oltp_distinct_range` | 4.81ms | 5.01ms | 1.042× | 1.05% | PASS |
| mem_reads | `oltp_index_scan` | 4.84ms | 6.72ms | 1.387× | 1.62% | PASS |
| mem_reads | `select_random_points` | 29.22ms | 33.55ms | 1.148× | 2.52% | PASS |
| mem_reads | `select_random_ranges` | 8.09ms | 9.38ms | 1.160× | 1.26% | PASS |
| mem_reads | `covering_index_scan` | 4.33ms | 4.69ms | 1.084× | 3.26% | PASS |
| mem_reads | `groupby_scan` | 37.21ms | 39.77ms | 1.069× | 1.14% | PASS |
| mem_reads | `index_join` | 8.56ms | 11.56ms | 1.350× | 2.56% | PASS |
| mem_reads | `index_join_scan` | 4.50ms | 5.99ms | 1.332× | 2.91% | PASS |
| mem_reads | `types_table_scan` | 1.10s | 1.26s | 1.144× | 2.61% | PASS |
| mem_reads | `table_scan` | 1.40s | 1.44s | 1.022× | 1.76% | PASS |
| mem_reads | `oltp_read_only` | 164.74ms | 176.17ms | 1.069× | 1.91% | PASS |
| mem_writes | `oltp_bulk_insert` | 250.60ms | 367.50ms | 1.466× | 1.26% | PASS |
| mem_writes | `oltp_insert` | 20.02ms | 38.10ms | 1.902× | 1.57% | PASS |
| mem_writes | `oltp_update_index` | 72.99ms | 124.71ms | 1.709× | 1.71% | PASS |
| mem_writes | `oltp_update_non_index` | 55.11ms | 88.08ms | 1.598× | 1.81% | PASS |
| mem_writes | `oltp_delete_insert` | 52.60ms | 100.81ms | 1.917× | 1.27% | PASS |
| mem_writes | `oltp_write_only` | 29.50ms | 61.29ms | 2.077× | 1.68% | PASS |
| mem_writes | `types_delete_insert` | 35.00ms | 57.65ms | 1.647× | 2.16% | PASS |
| mem_writes | `oltp_read_write` | 111.60ms | 163.08ms | 1.461× | 2.30% | PASS |
| file_reads | `oltp_point_select` | 111.31ms | 68.44ms | 0.615× | 1.21% | PASS |
| file_reads | `oltp_range_select` | 29.35ms | 25.02ms | 0.852× | 1.27% | PASS |
| file_reads | `oltp_sum_range` | 27.32ms | 24.70ms | 0.904× | 1.81% | PASS |
| file_reads | `oltp_order_range` | 4.73ms | 4.43ms | 0.937× | 2.09% | PASS |
| file_reads | `oltp_distinct_range` | 5.91ms | 5.50ms | 0.929× | 1.52% | PASS |
| file_reads | `oltp_index_scan` | 12.79ms | 9.61ms | 0.751× | 1.82% | PASS |
| file_reads | `select_random_points` | 39.85ms | 37.79ms | 0.948× | 2.19% | PASS |
| file_reads | `select_random_ranges` | 16.27ms | 12.57ms | 0.773× | 1.35% | PASS |
| file_reads | `covering_index_scan` | 12.71ms | 7.46ms | 0.587× | 1.81% | PASS |
| file_reads | `groupby_scan` | 38.59ms | 40.60ms | 1.052× | 1.24% | PASS |
| file_reads | `index_join` | 13.01ms | 13.44ms | 1.033× | 2.36% | PASS |
| file_reads | `index_join_scan` | 5.54ms | 6.33ms | 1.144× | 2.72% | PASS |
| file_reads | `types_table_scan` | 1.15s | 1.27s | 1.102× | 1.51% | PASS |
| file_reads | `table_scan` | 1.33s | 1.42s | 1.065× | 5.30% | PASS |
| file_reads | `oltp_read_only` | 276.63ms | 216.52ms | 0.783× | 1.40% | PASS |
| file_writes | `oltp_bulk_insert` | 266.79ms | 392.12ms | 1.470× | 1.46% | PASS |
| file_writes | `oltp_insert` | 28.02ms | 47.91ms | 1.709× | 3.87% | PASS |
| file_writes | `oltp_update_index` | 108.41ms | 158.37ms | 1.461× | 2.30% | PASS |
| file_writes | `oltp_update_non_index` | 84.54ms | 112.93ms | 1.336× | 1.82% | PASS |
| file_writes | `oltp_delete_insert` | 85.97ms | 129.21ms | 1.503× | 2.19% | PASS |
| file_writes | `oltp_write_only` | 55.98ms | 81.69ms | 1.459× | 2.40% | PASS |
| file_writes | `types_delete_insert` | 53.56ms | 71.96ms | 1.344× | 2.31% | PASS |
| file_writes | `oltp_read_write` | 143.66ms | 188.33ms | 1.311× | 1.91% | PASS |
| ac_reads | `oltp_point_select` | 62.68ms | 70.72ms | 1.128× | 1.13% | PASS |
| ac_reads | `oltp_range_select` | 25.32ms | 25.84ms | 1.021× | 1.48% | PASS |
| ac_reads | `oltp_sum_range` | 23.16ms | 25.56ms | 1.104× | 1.34% | PASS |
| ac_reads | `oltp_order_range` | 4.43ms | 4.59ms | 1.036× | 2.89% | PASS |
| ac_reads | `oltp_distinct_range` | 5.59ms | 5.71ms | 1.022× | 2.77% | PASS |
| ac_reads | `oltp_index_scan` | 8.17ms | 9.79ms | 1.197× | 2.34% | PASS |
| ac_reads | `select_random_points` | 35.01ms | 39.42ms | 1.126× | 2.28% | PASS |
| ac_reads | `select_random_ranges` | 11.16ms | 12.54ms | 1.124× | 1.33% | PASS |
| ac_reads | `covering_index_scan` | 7.83ms | 7.53ms | 0.962× | 1.72% | PASS |
| ac_reads | `groupby_scan` | 37.96ms | 40.86ms | 1.076× | 1.33% | PASS |
| ac_reads | `index_join` | 10.73ms | 13.63ms | 1.271× | 2.46% | PASS |
| ac_reads | `index_join_scan` | 5.09ms | 6.42ms | 1.260× | 2.86% | PASS |
| ac_reads | `types_table_scan` | 1.16s | 1.27s | 1.100× | 2.14% | PASS |
| ac_reads | `table_scan` | 1.42s | 1.43s | 1.008× | 1.67% | PASS |
| ac_reads | `oltp_read_only` | 204.60ms | 219.94ms | 1.075× | 1.28% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.95ms | 71.50ms | 2.986× | 7.23% | PASS |
| ac_writes | `oltp_insert_ac` | 26.08ms | 92.20ms | 3.535× | 6.02% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.12ms | 104.75ms | 3.597× | 6.56% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.41ms | 82.07ms | 3.362× | 6.73% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.61ms | 93.78ms | 3.662× | 7.21% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.86ms | 94.19ms | 3.507× | 7.61% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.57ms | 86.21ms | 3.657× | 7.19% | PASS |
| ac_writes | `oltp_read_write_ac` | 34.58ms | 107.05ms | 3.096× | 8.87% | PASS |

</details>

## Version-control latency

Wall time: 1m 43s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 56.64ms | 200.00ms | 28.3% | 0.90% | PASS |
| `status_dirty_many_tables` | 59.06ms | 200.00ms | 29.5% | 0.75% | PASS |
| `diff_regular_working_one_table` | 53.09ms | 150.00ms | 35.4% | 0.75% | PASS |
| `diff_regular_working_many_tables` | 63.32ms | 200.00ms | 31.7% | 0.66% | PASS |
| `diff_stat_working_many_tables` | 63.92ms | 200.00ms | 32.0% | 0.59% | PASS |
| `diff_schema_working_many_tables` | 64.41ms | 200.00ms | 32.2% | 0.65% | PASS |
| `branch_list_many_branches` | 17.75ms | 100.00ms | 17.7% | 1.08% | PASS |
| `branch_create_delete` | 19.52ms | 100.00ms | 19.5% | 2.02% | PASS |
| `checkout_branch_clean` | 78.34ms | 200.00ms | 39.2% | 0.81% | PASS |
| `merge_data_no_conflicts` | 28.46ms | 150.00ms | 19.0% | 8.39% | PASS |
| `merge_schema_no_conflicts` | 17.69ms | 100.00ms | 17.7% | 1.41% | PASS |
| `merge_data_conflicts` | 65.88ms | 250.00ms | 26.4% | 0.59% | PASS |
| `merge_data_conflicts_with_resolve` | 65.46ms | 250.00ms | 26.2% | 0.46% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
