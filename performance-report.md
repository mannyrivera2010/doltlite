# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-11 18:56 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu26 20261101.186.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/43067605804)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 54m 51s | 6.53s | 7.38s | 1.131× | 2.70% | **PASS** |
| textpk | 69 | 55 | 1h 30m 19s | 9.43s | 9.97s | 1.057× | 1.89% | **PASS** |
| blobpk | 69 | 55 | 1h 27m 56s | 9.28s | 10.21s | 1.100× | 3.65% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 24s | 9.79s | 11.87s | 1.212× | 1.44% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 16.25ms | 17.25ms | 1.062× | 6.53% | PASS |
| mem_reads | `oltp_range_select` | 6.48ms | 6.99ms | 1.078× | 3.16% | PASS |
| mem_reads | `oltp_sum_range` | 5.78ms | 6.98ms | 1.207× | 5.50% | PASS |
| mem_reads | `oltp_order_range` | 1.73ms | 1.80ms | 1.043× | 1.66% | PASS |
| mem_reads | `oltp_distinct_range` | 2.33ms | 2.40ms | 1.029× | 1.62% | PASS |
| mem_reads | `oltp_index_scan` | 2.83ms | 3.64ms | 1.285× | 3.09% | PASS |
| mem_reads | `select_random_points` | 10.81ms | 9.65ms | 0.893× | 2.62% | PASS |
| mem_reads | `select_random_ranges` | 2.39ms | 2.75ms | 1.153× | 1.53% | PASS |
| mem_reads | `covering_index_scan` | 2.44ms | 2.87ms | 1.175× | 2.69% | PASS |
| mem_reads | `groupby_scan` | 21.36ms | 20.82ms | 0.975× | 1.62% | PASS |
| mem_reads | `index_join` | 3.88ms | 5.30ms | 1.367× | 7.27% | PASS |
| mem_reads | `index_join_scan` | 2.72ms | 3.96ms | 1.455× | 4.42% | PASS |
| mem_reads | `types_table_scan` | 736.95ms | 801.82ms | 1.088× | 2.14% | PASS |
| mem_reads | `table_scan` | 856.72ms | 891.70ms | 1.041× | 3.55% | PASS |
| mem_reads | `oltp_read_only` | 57.96ms | 65.08ms | 1.123× | 1.89% | PASS |
| mem_writes | `oltp_bulk_insert` | 110.41ms | 151.70ms | 1.374× | 0.93% | PASS |
| mem_writes | `oltp_insert` | 9.44ms | 17.97ms | 1.903× | 1.22% | PASS |
| mem_writes | `oltp_update_index` | 34.68ms | 76.15ms | 2.196× | 2.11% | PASS |
| mem_writes | `oltp_update_non_index` | 25.22ms | 43.84ms | 1.738× | 1.84% | PASS |
| mem_writes | `oltp_delete_insert` | 29.64ms | 55.97ms | 1.888× | 2.26% | PASS |
| mem_writes | `oltp_write_only` | 15.37ms | 34.69ms | 2.257× | 2.84% | PASS |
| mem_writes | `types_delete_insert` | 18.46ms | 30.62ms | 1.659× | 2.85% | PASS |
| mem_writes | `oltp_read_write` | 43.26ms | 69.76ms | 1.613× | 5.01% | PASS |
| file_reads | `oltp_point_select` | 72.00ms | 36.47ms | 0.507× | 3.02% | PASS |
| file_reads | `oltp_range_select` | 12.41ms | 9.12ms | 0.734× | 2.37% | PASS |
| file_reads | `oltp_sum_range` | 12.04ms | 9.35ms | 0.777× | 2.65% | PASS |
| file_reads | `oltp_order_range` | 2.38ms | 2.06ms | 0.868× | 1.99% | PASS |
| file_reads | `oltp_distinct_range` | 2.89ms | 2.55ms | 0.884× | 1.76% | PASS |
| file_reads | `oltp_index_scan` | 8.83ms | 5.68ms | 0.644× | 3.32% | PASS |
| file_reads | `select_random_points` | 16.44ms | 11.09ms | 0.674× | 2.85% | PASS |
| file_reads | `select_random_ranges` | 8.01ms | 4.59ms | 0.574× | 1.61% | PASS |
| file_reads | `covering_index_scan` | 8.54ms | 4.97ms | 0.581× | 3.96% | PASS |
| file_reads | `groupby_scan` | 21.66ms | 20.62ms | 0.952× | 0.93% | PASS |
| file_reads | `index_join` | 7.30ms | 6.96ms | 0.954× | 4.71% | PASS |
| file_reads | `index_join_scan` | 3.30ms | 3.98ms | 1.207× | 3.60% | PASS |
| file_reads | `types_table_scan` | 747.47ms | 812.92ms | 1.088× | 1.12% | PASS |
| file_reads | `table_scan` | 889.56ms | 904.85ms | 1.017× | 2.10% | PASS |
| file_reads | `oltp_read_only` | 148.90ms | 99.81ms | 0.670× | 1.67% | PASS |
| file_writes | `oltp_bulk_insert` | 150.64ms | 214.33ms | 1.423× | 2.92% | PASS |
| file_writes | `oltp_insert` | 19.70ms | 28.54ms | 1.449× | 4.65% | PASS |
| file_writes | `oltp_update_index` | 130.90ms | 138.38ms | 1.057× | 6.00% | PASS |
| file_writes | `oltp_update_non_index` | 101.37ms | 88.87ms | 0.877× | 2.52% | PASS |
| file_writes | `oltp_delete_insert` | 108.50ms | 112.45ms | 1.036× | 7.49% | PASS |
| file_writes | `oltp_write_only` | 73.46ms | 78.28ms | 1.066× | 6.46% | PASS |
| file_writes | `types_delete_insert` | 61.40ms | 54.36ms | 0.885× | 8.47% | PASS |
| file_writes | `oltp_read_write` | 101.19ms | 108.14ms | 1.069× | 4.60% | PASS |
| ac_reads | `oltp_point_select` | 34.69ms | 36.51ms | 1.053× | 2.09% | PASS |
| ac_reads | `oltp_range_select` | 9.28ms | 9.27ms | 0.999× | 2.70% | PASS |
| ac_reads | `oltp_sum_range` | 8.43ms | 9.24ms | 1.097× | 1.92% | PASS |
| ac_reads | `oltp_order_range` | 2.04ms | 2.07ms | 1.016× | 1.13% | PASS |
| ac_reads | `oltp_distinct_range` | 2.60ms | 2.60ms | 0.998× | 1.04% | PASS |
| ac_reads | `oltp_index_scan` | 5.05ms | 5.68ms | 1.126× | 2.09% | PASS |
| ac_reads | `select_random_points` | 11.94ms | 10.76ms | 0.901× | 5.25% | PASS |
| ac_reads | `select_random_ranges` | 4.08ms | 4.54ms | 1.113× | 2.62% | PASS |
| ac_reads | `covering_index_scan` | 4.22ms | 4.39ms | 1.039× | 1.90% | PASS |
| ac_reads | `groupby_scan` | 20.62ms | 20.38ms | 0.988× | 1.59% | PASS |
| ac_reads | `index_join` | 4.85ms | 5.88ms | 1.211× | 2.80% | PASS |
| ac_reads | `index_join_scan` | 2.46ms | 3.74ms | 1.519× | 4.52% | PASS |
| ac_reads | `types_table_scan` | 665.40ms | 732.89ms | 1.101× | 0.79% | PASS |
| ac_reads | `table_scan` | 775.15ms | 839.78ms | 1.083× | 1.13% | PASS |
| ac_reads | `oltp_read_only` | 82.69ms | 90.96ms | 1.100× | 1.63% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.65ms | 54.68ms | 2.648× | 3.71% | PASS |
| ac_writes | `oltp_insert_ac` | 21.48ms | 65.24ms | 3.038× | 5.07% | PASS |
| ac_writes | `oltp_update_index_ac` | 22.02ms | 73.70ms | 3.347× | 5.17% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 19.98ms | 59.02ms | 2.954× | 4.01% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 21.28ms | 67.42ms | 3.169× | 4.83% | PASS |
| ac_writes | `oltp_write_only_ac` | 20.96ms | 66.42ms | 3.169× | 4.16% | PASS |
| ac_writes | `types_delete_insert_ac` | 19.19ms | 64.95ms | 3.385× | 5.73% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.50ms | 71.41ms | 2.914× | 5.09% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.28ms | 27.19ms | 1.120× | 2.39% | PASS |
| mem_reads | `oltp_range_select` | 12.66ms | 11.10ms | 0.876× | 3.61% | PASS |
| mem_reads | `oltp_sum_range` | 10.50ms | 10.78ms | 1.027× | 3.83% | PASS |
| mem_reads | `oltp_order_range` | 2.60ms | 2.56ms | 0.984× | 1.88% | PASS |
| mem_reads | `oltp_distinct_range` | 3.35ms | 3.32ms | 0.991× | 0.86% | PASS |
| mem_reads | `oltp_index_scan` | 3.64ms | 4.64ms | 1.276× | 1.88% | PASS |
| mem_reads | `select_random_points` | 14.27ms | 16.04ms | 1.124× | 2.24% | PASS |
| mem_reads | `select_random_ranges` | 3.33ms | 4.13ms | 1.238× | 1.17% | PASS |
| mem_reads | `covering_index_scan` | 3.75ms | 3.46ms | 0.924× | 2.08% | PASS |
| mem_reads | `groupby_scan` | 27.38ms | 27.38ms | 1.000× | 0.78% | PASS |
| mem_reads | `index_join` | 5.64ms | 7.05ms | 1.250× | 1.93% | PASS |
| mem_reads | `index_join_scan` | 4.24ms | 4.71ms | 1.110× | 3.40% | PASS |
| mem_reads | `types_table_scan` | 884.61ms | 979.37ms | 1.107× | 0.56% | PASS |
| mem_reads | `table_scan` | 1.04s | 1.06s | 1.025× | 0.87% | PASS |
| mem_reads | `oltp_read_only` | 96.51ms | 101.49ms | 1.052× | 1.49% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.19ms | 268.41ms | 1.498× | 1.00% | PASS |
| mem_writes | `oltp_insert` | 17.56ms | 31.55ms | 1.797× | 0.75% | PASS |
| mem_writes | `oltp_update_index` | 59.24ms | 110.44ms | 1.864× | 1.42% | PASS |
| mem_writes | `oltp_update_non_index` | 40.70ms | 71.60ms | 1.759× | 0.97% | PASS |
| mem_writes | `oltp_delete_insert` | 42.38ms | 85.69ms | 2.022× | 1.29% | PASS |
| mem_writes | `oltp_write_only` | 24.38ms | 52.13ms | 2.138× | 1.38% | PASS |
| mem_writes | `types_delete_insert` | 27.15ms | 45.71ms | 1.684× | 1.37% | PASS |
| mem_writes | `oltp_read_write` | 69.37ms | 108.68ms | 1.567× | 2.18% | PASS |
| file_reads | `oltp_point_select` | 92.35ms | 49.98ms | 0.541× | 0.76% | PASS |
| file_reads | `oltp_range_select` | 19.90ms | 13.53ms | 0.680× | 1.04% | PASS |
| file_reads | `oltp_sum_range` | 17.71ms | 13.18ms | 0.744× | 1.04% | PASS |
| file_reads | `oltp_order_range` | 3.47ms | 2.86ms | 0.824× | 1.22% | PASS |
| file_reads | `oltp_distinct_range` | 4.24ms | 3.62ms | 0.853× | 0.94% | PASS |
| file_reads | `oltp_index_scan` | 11.07ms | 7.31ms | 0.660× | 1.06% | PASS |
| file_reads | `select_random_points` | 22.18ms | 18.69ms | 0.842× | 1.26% | PASS |
| file_reads | `select_random_ranges` | 10.62ms | 6.61ms | 0.622× | 1.95% | PASS |
| file_reads | `covering_index_scan` | 13.25ms | 6.71ms | 0.506× | 5.57% | PASS |
| file_reads | `groupby_scan` | 30.08ms | 28.53ms | 0.949× | 1.74% | PASS |
| file_reads | `index_join` | 10.80ms | 9.46ms | 0.876× | 2.47% | PASS |
| file_reads | `index_join_scan` | 4.94ms | 5.10ms | 1.033× | 1.59% | PASS |
| file_reads | `types_table_scan` | 989.35ms | 1.01s | 1.020× | 6.24% | PASS |
| file_reads | `table_scan` | 1.40s | 1.13s | 0.804× | 1.87% | PASS |
| file_reads | `oltp_read_only` | 209.09ms | 139.61ms | 0.668× | 1.12% | PASS |
| file_writes | `oltp_bulk_insert` | 243.97ms | 367.28ms | 1.505× | 8.25% | PASS |
| file_writes | `oltp_insert` | 78.08ms | 66.17ms | 0.847× | 7.51% | PASS |
| file_writes | `oltp_update_index` | 226.03ms | 222.25ms | 0.983× | 6.35% | PASS |
| file_writes | `oltp_update_non_index` | 163.47ms | 152.22ms | 0.931× | 10.24% | PASS |
| file_writes | `oltp_delete_insert` | 200.67ms | 178.59ms | 0.890× | 14.45% | PASS |
| file_writes | `oltp_write_only` | 135.56ms | 123.88ms | 0.914× | 5.69% | PASS |
| file_writes | `types_delete_insert` | 108.26ms | 102.35ms | 0.945× | 12.78% | PASS |
| file_writes | `oltp_read_write` | 205.59ms | 185.87ms | 0.904× | 9.70% | PASS |
| ac_reads | `oltp_point_select` | 47.83ms | 52.57ms | 1.099× | 1.13% | PASS |
| ac_reads | `oltp_range_select` | 16.53ms | 14.01ms | 0.847× | 3.12% | PASS |
| ac_reads | `oltp_sum_range` | 13.56ms | 13.51ms | 0.997× | 1.87% | PASS |
| ac_reads | `oltp_order_range` | 3.08ms | 2.87ms | 0.931× | 1.00% | PASS |
| ac_reads | `oltp_distinct_range` | 3.83ms | 3.63ms | 0.949× | 0.91% | PASS |
| ac_reads | `oltp_index_scan` | 6.57ms | 7.43ms | 1.131× | 1.15% | PASS |
| ac_reads | `select_random_points` | 18.64ms | 19.43ms | 1.042× | 2.88% | PASS |
| ac_reads | `select_random_ranges` | 5.89ms | 6.62ms | 1.125× | 1.45% | PASS |
| ac_reads | `covering_index_scan` | 7.41ms | 6.24ms | 0.841× | 1.41% | PASS |
| ac_reads | `groupby_scan` | 28.34ms | 28.08ms | 0.991× | 0.94% | PASS |
| ac_reads | `index_join` | 8.04ms | 9.35ms | 1.162× | 1.89% | PASS |
| ac_reads | `index_join_scan` | 4.58ms | 5.18ms | 1.129× | 2.65% | PASS |
| ac_reads | `types_table_scan` | 955.42ms | 1.01s | 1.060× | 3.81% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.09s | 0.911× | 5.67% | PASS |
| ac_reads | `oltp_read_only` | 133.11ms | 137.08ms | 1.030× | 2.51% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.45ms | 65.40ms | 3.199× | 31.60% | PASS |
| ac_writes | `oltp_insert_ac` | 23.28ms | 79.81ms | 3.428× | 47.56% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.13ms | 107.91ms | 4.130× | 27.09% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 19.41ms | 80.59ms | 4.151× | 35.59% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.16ms | 79.53ms | 3.435× | 16.02% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.08ms | 91.57ms | 4.148× | 19.72% | PASS |
| ac_writes | `types_delete_insert_ac` | 18.48ms | 75.25ms | 4.073× | 19.06% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.00ms | 96.62ms | 3.332× | 18.48% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 28.62ms | 31.52ms | 1.101× | 4.13% | PASS |
| mem_reads | `oltp_range_select` | 18.50ms | 19.25ms | 1.041× | 19.04% | PASS |
| mem_reads | `oltp_sum_range` | 14.58ms | 16.22ms | 1.112× | 14.28% | PASS |
| mem_reads | `oltp_order_range` | 3.30ms | 3.46ms | 1.048× | 11.32% | PASS |
| mem_reads | `oltp_distinct_range` | 4.21ms | 4.25ms | 1.010× | 10.74% | PASS |
| mem_reads | `oltp_index_scan` | 4.48ms | 6.08ms | 1.357× | 4.75% | PASS |
| mem_reads | `select_random_points` | 15.60ms | 17.36ms | 1.113× | 3.75% | PASS |
| mem_reads | `select_random_ranges` | 3.56ms | 4.24ms | 1.191× | 2.55% | PASS |
| mem_reads | `covering_index_scan` | 3.66ms | 3.91ms | 1.069× | 3.60% | PASS |
| mem_reads | `groupby_scan` | 27.50ms | 27.75ms | 1.009× | 1.11% | PASS |
| mem_reads | `index_join` | 5.63ms | 7.87ms | 1.398× | 2.82% | PASS |
| mem_reads | `index_join_scan` | 3.73ms | 4.83ms | 1.295× | 2.42% | PASS |
| mem_reads | `types_table_scan` | 939.80ms | 1.01s | 1.071× | 3.15% | PASS |
| mem_reads | `table_scan` | 1.41s | 1.25s | 0.890× | 4.76% | PASS |
| mem_reads | `oltp_read_only` | 117.44ms | 122.47ms | 1.043× | 6.77% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.13ms | 269.74ms | 1.497× | 0.86% | PASS |
| mem_writes | `oltp_insert` | 15.87ms | 32.52ms | 2.050× | 1.11% | PASS |
| mem_writes | `oltp_update_index` | 59.57ms | 115.50ms | 1.939× | 2.87% | PASS |
| mem_writes | `oltp_update_non_index` | 41.30ms | 71.51ms | 1.732× | 1.58% | PASS |
| mem_writes | `oltp_delete_insert` | 42.09ms | 88.82ms | 2.110× | 3.15% | PASS |
| mem_writes | `oltp_write_only` | 23.64ms | 53.45ms | 2.261× | 2.42% | PASS |
| mem_writes | `types_delete_insert` | 26.42ms | 45.33ms | 1.716× | 2.40% | PASS |
| mem_writes | `oltp_read_write` | 67.78ms | 110.66ms | 1.633× | 2.13% | PASS |
| file_reads | `oltp_point_select` | 93.51ms | 51.05ms | 0.546× | 0.90% | PASS |
| file_reads | `oltp_range_select` | 19.17ms | 13.56ms | 0.708× | 2.35% | PASS |
| file_reads | `oltp_sum_range` | 23.53ms | 18.44ms | 0.784× | 12.34% | PASS |
| file_reads | `oltp_order_range` | 4.16ms | 3.86ms | 0.928× | 11.37% | PASS |
| file_reads | `oltp_distinct_range` | 5.31ms | 4.63ms | 0.872× | 8.78% | PASS |
| file_reads | `oltp_index_scan` | 12.61ms | 8.73ms | 0.692× | 5.54% | PASS |
| file_reads | `select_random_points` | 26.55ms | 22.98ms | 0.866× | 6.32% | PASS |
| file_reads | `select_random_ranges` | 10.63ms | 6.73ms | 0.633× | 1.51% | PASS |
| file_reads | `covering_index_scan` | 11.68ms | 6.25ms | 0.535× | 1.79% | PASS |
| file_reads | `groupby_scan` | 28.30ms | 27.92ms | 0.986× | 1.05% | PASS |
| file_reads | `index_join` | 10.14ms | 9.46ms | 0.933× | 3.00% | PASS |
| file_reads | `index_join_scan` | 4.70ms | 5.20ms | 1.107× | 3.65% | PASS |
| file_reads | `types_table_scan` | 908.24ms | 1.00s | 1.101× | 1.92% | PASS |
| file_reads | `table_scan` | 1.15s | 1.10s | 0.950× | 4.41% | PASS |
| file_reads | `oltp_read_only` | 197.87ms | 136.97ms | 0.692× | 1.37% | PASS |
| file_writes | `oltp_bulk_insert` | 271.47ms | 407.39ms | 1.501× | 4.40% | PASS |
| file_writes | `oltp_insert` | 53.67ms | 71.54ms | 1.333× | 5.20% | PASS |
| file_writes | `oltp_update_index` | 207.64ms | 247.27ms | 1.191× | 4.37% | PASS |
| file_writes | `oltp_update_non_index` | 160.94ms | 168.03ms | 1.044× | 5.25% | PASS |
| file_writes | `oltp_delete_insert` | 156.88ms | 171.14ms | 1.091× | 4.60% | PASS |
| file_writes | `oltp_write_only` | 114.48ms | 119.44ms | 1.043× | 3.32% | PASS |
| file_writes | `types_delete_insert` | 95.25ms | 95.23ms | 1.000× | 5.98% | PASS |
| file_writes | `oltp_read_write` | 160.71ms | 176.44ms | 1.098× | 2.20% | PASS |
| ac_reads | `oltp_point_select` | 46.06ms | 51.16ms | 1.111× | 1.32% | PASS |
| ac_reads | `oltp_range_select` | 14.24ms | 13.48ms | 0.947× | 2.11% | PASS |
| ac_reads | `oltp_sum_range` | 12.88ms | 13.15ms | 1.020× | 1.90% | PASS |
| ac_reads | `oltp_order_range` | 2.95ms | 2.89ms | 0.980× | 2.74% | PASS |
| ac_reads | `oltp_distinct_range` | 4.21ms | 3.91ms | 0.930× | 14.65% | PASS |
| ac_reads | `oltp_index_scan` | 6.94ms | 8.29ms | 1.195× | 6.01% | PASS |
| ac_reads | `select_random_points` | 19.14ms | 20.77ms | 1.085× | 9.08% | PASS |
| ac_reads | `select_random_ranges` | 6.11ms | 6.82ms | 1.116× | 5.49% | PASS |
| ac_reads | `covering_index_scan` | 6.85ms | 6.21ms | 0.906× | 1.21% | PASS |
| ac_reads | `groupby_scan` | 27.35ms | 27.82ms | 1.018× | 0.79% | PASS |
| ac_reads | `index_join` | 7.62ms | 9.28ms | 1.217× | 1.76% | PASS |
| ac_reads | `index_join_scan` | 4.17ms | 5.21ms | 1.250× | 2.87% | PASS |
| ac_reads | `types_table_scan` | 902.69ms | 999.42ms | 1.107× | 1.98% | PASS |
| ac_reads | `table_scan` | 1.14s | 1.09s | 0.962× | 3.98% | PASS |
| ac_reads | `oltp_read_only` | 131.31ms | 137.23ms | 1.045× | 1.86% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 18.89ms | 59.96ms | 3.174× | 7.31% | PASS |
| ac_writes | `oltp_insert_ac` | 20.82ms | 73.99ms | 3.554× | 8.99% | PASS |
| ac_writes | `oltp_update_index_ac` | 23.59ms | 98.74ms | 4.187× | 13.99% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 19.33ms | 72.77ms | 3.764× | 16.35% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 20.93ms | 82.69ms | 3.950× | 10.87% | PASS |
| ac_writes | `oltp_write_only_ac` | 21.06ms | 77.63ms | 3.686× | 9.82% | PASS |
| ac_writes | `types_delete_insert_ac` | 18.65ms | 70.64ms | 3.787× | 9.95% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.40ms | 92.96ms | 3.392× | 19.59% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.36ms | 40.36ms | 1.210× | 1.43% | PASS |
| mem_reads | `oltp_range_select` | 19.50ms | 21.39ms | 1.097× | 1.36% | PASS |
| mem_reads | `oltp_sum_range` | 17.93ms | 21.02ms | 1.172× | 1.10% | PASS |
| mem_reads | `oltp_order_range` | 3.70ms | 3.94ms | 1.065× | 0.90% | PASS |
| mem_reads | `oltp_distinct_range` | 4.72ms | 5.05ms | 1.071× | 0.84% | PASS |
| mem_reads | `oltp_index_scan` | 4.65ms | 6.28ms | 1.350× | 1.39% | PASS |
| mem_reads | `select_random_points` | 26.89ms | 30.96ms | 1.151× | 1.26% | PASS |
| mem_reads | `select_random_ranges` | 7.80ms | 9.05ms | 1.160× | 1.32% | PASS |
| mem_reads | `covering_index_scan` | 4.17ms | 4.32ms | 1.035× | 1.65% | PASS |
| mem_reads | `groupby_scan` | 37.87ms | 41.09ms | 1.085× | 1.76% | PASS |
| mem_reads | `index_join` | 8.19ms | 10.80ms | 1.319× | 1.56% | PASS |
| mem_reads | `index_join_scan` | 4.18ms | 5.65ms | 1.352× | 1.63% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.24s | 1.173× | 1.14% | PASS |
| mem_reads | `table_scan` | 1.20s | 1.35s | 1.119× | 2.80% | PASS |
| mem_reads | `oltp_read_only` | 152.72ms | 171.68ms | 1.124× | 1.07% | PASS |
| mem_writes | `oltp_bulk_insert` | 244.43ms | 358.51ms | 1.467× | 0.96% | PASS |
| mem_writes | `oltp_insert` | 18.86ms | 37.28ms | 1.977× | 0.88% | PASS |
| mem_writes | `oltp_update_index` | 66.44ms | 119.32ms | 1.796× | 0.92% | PASS |
| mem_writes | `oltp_update_non_index` | 49.86ms | 86.16ms | 1.728× | 0.90% | PASS |
| mem_writes | `oltp_delete_insert` | 49.37ms | 99.17ms | 2.009× | 1.10% | PASS |
| mem_writes | `oltp_write_only` | 26.95ms | 61.06ms | 2.266× | 1.01% | PASS |
| mem_writes | `types_delete_insert` | 32.35ms | 56.76ms | 1.755× | 1.08% | PASS |
| mem_writes | `oltp_read_write` | 102.95ms | 161.23ms | 1.566× | 1.29% | PASS |
| file_reads | `oltp_point_select` | 102.04ms | 64.17ms | 0.629× | 0.88% | PASS |
| file_reads | `oltp_range_select` | 26.40ms | 24.09ms | 0.913× | 1.83% | PASS |
| file_reads | `oltp_sum_range` | 25.03ms | 23.92ms | 0.956× | 1.65% | PASS |
| file_reads | `oltp_order_range` | 4.46ms | 4.29ms | 0.961× | 1.57% | PASS |
| file_reads | `oltp_distinct_range` | 5.60ms | 5.44ms | 0.971× | 1.63% | PASS |
| file_reads | `oltp_index_scan` | 11.87ms | 8.98ms | 0.756× | 1.38% | PASS |
| file_reads | `select_random_points` | 36.65ms | 35.23ms | 0.961× | 1.76% | PASS |
| file_reads | `select_random_ranges` | 15.37ms | 11.74ms | 0.764× | 1.84% | PASS |
| file_reads | `covering_index_scan` | 11.33ms | 7.06ms | 0.623× | 1.51% | PASS |
| file_reads | `groupby_scan` | 39.09ms | 42.79ms | 1.095× | 2.34% | PASS |
| file_reads | `index_join` | 12.36ms | 12.99ms | 1.051× | 1.86% | PASS |
| file_reads | `index_join_scan` | 5.21ms | 6.15ms | 1.180× | 1.64% | PASS |
| file_reads | `types_table_scan` | 1.11s | 1.25s | 1.131× | 1.23% | PASS |
| file_reads | `table_scan` | 1.29s | 1.35s | 1.045× | 4.06% | PASS |
| file_reads | `oltp_read_only` | 260.62ms | 211.20ms | 0.810× | 1.17% | PASS |
| file_writes | `oltp_bulk_insert` | 258.62ms | 383.27ms | 1.482× | 1.05% | PASS |
| file_writes | `oltp_insert` | 25.83ms | 47.91ms | 1.855× | 1.44% | PASS |
| file_writes | `oltp_update_index` | 97.97ms | 150.33ms | 1.534× | 1.31% | PASS |
| file_writes | `oltp_update_non_index` | 78.33ms | 109.54ms | 1.398× | 1.90% | PASS |
| file_writes | `oltp_delete_insert` | 78.81ms | 126.20ms | 1.601× | 1.86% | PASS |
| file_writes | `oltp_write_only` | 52.33ms | 82.36ms | 1.574× | 2.00% | PASS |
| file_writes | `types_delete_insert` | 49.90ms | 71.52ms | 1.433× | 1.62% | PASS |
| file_writes | `oltp_read_write` | 131.72ms | 182.80ms | 1.388× | 0.88% | PASS |
| ac_reads | `oltp_point_select` | 56.35ms | 64.89ms | 1.152× | 0.96% | PASS |
| ac_reads | `oltp_range_select` | 22.50ms | 24.05ms | 1.069× | 1.31% | PASS |
| ac_reads | `oltp_sum_range` | 20.76ms | 23.94ms | 1.153× | 0.87% | PASS |
| ac_reads | `oltp_order_range` | 4.13ms | 4.29ms | 1.038× | 1.85% | PASS |
| ac_reads | `oltp_distinct_range` | 5.19ms | 5.46ms | 1.052× | 1.75% | PASS |
| ac_reads | `oltp_index_scan` | 7.34ms | 9.00ms | 1.227× | 1.36% | PASS |
| ac_reads | `select_random_points` | 30.62ms | 34.89ms | 1.139× | 1.78% | PASS |
| ac_reads | `select_random_ranges` | 10.43ms | 11.80ms | 1.131× | 1.40% | PASS |
| ac_reads | `covering_index_scan` | 6.89ms | 7.09ms | 1.028× | 0.90% | PASS |
| ac_reads | `groupby_scan` | 38.41ms | 42.84ms | 1.115× | 1.39% | PASS |
| ac_reads | `index_join` | 9.98ms | 12.83ms | 1.285× | 1.18% | PASS |
| ac_reads | `index_join_scan` | 4.67ms | 6.10ms | 1.306× | 1.47% | PASS |
| ac_reads | `types_table_scan` | 1.07s | 1.24s | 1.163× | 1.01% | PASS |
| ac_reads | `table_scan` | 1.21s | 1.34s | 1.108× | 2.46% | PASS |
| ac_reads | `oltp_read_only` | 188.62ms | 210.75ms | 1.117× | 1.52% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.57ms | 64.03ms | 2.968× | 5.60% | PASS |
| ac_writes | `oltp_insert_ac` | 25.02ms | 85.82ms | 3.431× | 8.48% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.46ms | 97.16ms | 3.538× | 7.19% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.69ms | 76.00ms | 3.504× | 5.41% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.55ms | 86.80ms | 3.685× | 6.57% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.23ms | 87.46ms | 3.609× | 8.04% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.11ms | 77.33ms | 3.663× | 7.39% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.84ms | 96.48ms | 3.030× | 6.67% | PASS |

</details>

## Version-control latency

Wall time: 1m 38s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 54.52ms | 200.00ms | 27.3% | 1.34% | PASS |
| `status_dirty_many_tables` | 56.74ms | 200.00ms | 28.4% | 1.16% | PASS |
| `diff_regular_working_one_table` | 52.39ms | 150.00ms | 34.9% | 0.90% | PASS |
| `diff_regular_working_many_tables` | 60.63ms | 200.00ms | 30.3% | 1.04% | PASS |
| `diff_stat_working_many_tables` | 59.59ms | 200.00ms | 29.8% | 1.38% | PASS |
| `diff_schema_working_many_tables` | 58.71ms | 200.00ms | 29.4% | 0.94% | PASS |
| `branch_list_many_branches` | 14.89ms | 100.00ms | 14.9% | 1.76% | PASS |
| `branch_create_delete` | 30.20ms | 100.00ms | 30.2% | 42.09% | PASS |
| `checkout_branch_clean` | 72.86ms | 200.00ms | 36.4% | 4.09% | PASS |
| `merge_data_no_conflicts` | 24.28ms | 150.00ms | 16.2% | 2.80% | PASS |
| `merge_schema_no_conflicts` | 14.61ms | 100.00ms | 14.6% | 2.44% | PASS |
| `merge_data_conflicts` | 59.98ms | 250.00ms | 24.0% | 1.26% | PASS |
| `merge_data_conflicts_with_resolve` | 61.18ms | 250.00ms | 24.5% | 2.08% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
