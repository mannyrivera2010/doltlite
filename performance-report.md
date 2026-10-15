# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-15 15:54 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261012.334.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/38750470174)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 46s | 8.96s | 11.42s | 1.274× | 1.50% | **PASS** |
| textpk | 69 | 55 | 1h 31m 39s | 9.62s | 11.72s | 1.219× | 1.55% | **PASS** |
| blobpk | 69 | 55 | 1h 22m 50s | 8.30s | 9.41s | 1.134× | 0.84% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 3s | 9.76s | 11.92s | 1.222× | 1.50% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.17ms | 29.96ms | 1.191× | 2.18% | PASS |
| mem_reads | `oltp_range_select` | 10.98ms | 13.44ms | 1.223× | 2.49% | PASS |
| mem_reads | `oltp_sum_range` | 10.19ms | 12.50ms | 1.227× | 1.73% | PASS |
| mem_reads | `oltp_order_range` | 2.69ms | 3.02ms | 1.122× | 1.49% | PASS |
| mem_reads | `oltp_distinct_range` | 3.75ms | 4.04ms | 1.078× | 0.95% | PASS |
| mem_reads | `oltp_index_scan` | 3.99ms | 5.47ms | 1.370× | 1.66% | PASS |
| mem_reads | `select_random_points` | 10.37ms | 11.13ms | 1.073× | 3.69% | PASS |
| mem_reads | `select_random_ranges` | 3.02ms | 3.98ms | 1.317× | 1.50% | PASS |
| mem_reads | `covering_index_scan` | 4.22ms | 4.17ms | 0.988× | 1.54% | PASS |
| mem_reads | `groupby_scan` | 29.80ms | 33.32ms | 1.118× | 0.58% | PASS |
| mem_reads | `index_join` | 6.02ms | 8.09ms | 1.343× | 1.78% | PASS |
| mem_reads | `index_join_scan` | 3.50ms | 4.54ms | 1.297× | 2.64% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.33s | 1.282× | 0.93% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.39s | 1.192× | 0.83% | PASS |
| mem_reads | `oltp_read_only` | 107.65ms | 125.14ms | 1.162× | 2.02% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.08ms | 253.04ms | 1.405× | 1.19% | PASS |
| mem_writes | `oltp_insert` | 15.40ms | 28.19ms | 1.831× | 0.88% | PASS |
| mem_writes | `oltp_update_index` | 50.95ms | 105.83ms | 2.077× | 1.22% | PASS |
| mem_writes | `oltp_update_non_index` | 34.52ms | 59.42ms | 1.722× | 1.15% | PASS |
| mem_writes | `oltp_delete_insert` | 45.61ms | 79.42ms | 1.741× | 1.06% | PASS |
| mem_writes | `oltp_write_only` | 22.45ms | 45.66ms | 2.034× | 1.24% | PASS |
| mem_writes | `types_delete_insert` | 25.16ms | 40.48ms | 1.609× | 1.37% | PASS |
| mem_writes | `oltp_read_write` | 69.55ms | 111.26ms | 1.600× | 2.45% | PASS |
| file_reads | `oltp_point_select` | 99.11ms | 55.35ms | 0.558× | 0.90% | PASS |
| file_reads | `oltp_range_select` | 18.56ms | 15.99ms | 0.861× | 1.81% | PASS |
| file_reads | `oltp_sum_range` | 17.91ms | 15.24ms | 0.851× | 1.50% | PASS |
| file_reads | `oltp_order_range` | 3.54ms | 3.35ms | 0.946× | 1.84% | PASS |
| file_reads | `oltp_distinct_range` | 4.62ms | 4.41ms | 0.954× | 0.99% | PASS |
| file_reads | `oltp_index_scan` | 11.70ms | 8.37ms | 0.716× | 0.81% | PASS |
| file_reads | `select_random_points` | 18.68ms | 13.98ms | 0.749× | 2.35% | PASS |
| file_reads | `select_random_ranges` | 10.59ms | 6.60ms | 0.624× | 1.03% | PASS |
| file_reads | `covering_index_scan` | 11.83ms | 7.12ms | 0.602× | 1.20% | PASS |
| file_reads | `groupby_scan` | 30.93ms | 33.77ms | 1.092× | 0.71% | PASS |
| file_reads | `index_join` | 10.25ms | 10.28ms | 1.002× | 1.83% | PASS |
| file_reads | `index_join_scan` | 4.53ms | 4.93ms | 1.089× | 2.27% | PASS |
| file_reads | `types_table_scan` | 1.07s | 1.34s | 1.251× | 1.97% | PASS |
| file_reads | `table_scan` | 1.19s | 1.40s | 1.178× | 1.42% | PASS |
| file_reads | `oltp_read_only` | 215.03ms | 161.89ms | 0.753× | 1.40% | PASS |
| file_writes | `oltp_bulk_insert` | 193.63ms | 272.65ms | 1.408× | 1.03% | PASS |
| file_writes | `oltp_insert` | 22.04ms | 35.63ms | 1.617× | 1.23% | PASS |
| file_writes | `oltp_update_index` | 78.23ms | 128.89ms | 1.648× | 1.00% | PASS |
| file_writes | `oltp_update_non_index` | 58.33ms | 81.57ms | 1.398× | 1.62% | PASS |
| file_writes | `oltp_delete_insert` | 68.41ms | 99.71ms | 1.457× | 1.41% | PASS |
| file_writes | `oltp_write_only` | 44.59ms | 64.38ms | 1.444× | 2.22% | PASS |
| file_writes | `types_delete_insert` | 40.65ms | 53.64ms | 1.320× | 1.68% | PASS |
| file_writes | `oltp_read_write` | 92.66ms | 129.66ms | 1.399× | 0.98% | PASS |
| ac_reads | `oltp_point_select` | 48.92ms | 55.20ms | 1.128× | 0.99% | PASS |
| ac_reads | `oltp_range_select` | 13.53ms | 15.92ms | 1.177× | 1.41% | PASS |
| ac_reads | `oltp_sum_range` | 12.91ms | 15.11ms | 1.170× | 1.13% | PASS |
| ac_reads | `oltp_order_range` | 3.00ms | 3.32ms | 1.106× | 1.03% | PASS |
| ac_reads | `oltp_distinct_range` | 4.05ms | 4.37ms | 1.079× | 1.29% | PASS |
| ac_reads | `oltp_index_scan` | 6.68ms | 8.33ms | 1.246× | 1.62% | PASS |
| ac_reads | `select_random_points` | 13.61ms | 14.01ms | 1.029× | 1.75% | PASS |
| ac_reads | `select_random_ranges` | 5.61ms | 6.60ms | 1.177× | 1.08% | PASS |
| ac_reads | `covering_index_scan` | 7.01ms | 7.11ms | 1.014× | 1.73% | PASS |
| ac_reads | `groupby_scan` | 30.27ms | 33.90ms | 1.120× | 0.98% | PASS |
| ac_reads | `index_join` | 7.83ms | 10.38ms | 1.325× | 1.69% | PASS |
| ac_reads | `index_join_scan` | 4.09ms | 5.04ms | 1.233× | 2.17% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.34s | 1.265× | 1.18% | PASS |
| ac_reads | `table_scan` | 1.22s | 1.41s | 1.157× | 1.64% | PASS |
| ac_reads | `oltp_read_only` | 143.82ms | 162.19ms | 1.128× | 1.12% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.13ms | 65.95ms | 3.121× | 3.95% | PASS |
| ac_writes | `oltp_insert_ac` | 23.48ms | 82.56ms | 3.516× | 5.76% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.91ms | 98.41ms | 3.798× | 7.14% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.78ms | 76.20ms | 3.498× | 8.08% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.32ms | 88.14ms | 3.779× | 5.72% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.78ms | 87.05ms | 3.660× | 3.78% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.37ms | 76.47ms | 3.578× | 5.73% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.90ms | 93.23ms | 3.226× | 3.23% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.10ms | 38.06ms | 1.264× | 1.77% | PASS |
| mem_reads | `oltp_range_select` | 13.41ms | 14.24ms | 1.062× | 3.18% | PASS |
| mem_reads | `oltp_sum_range` | 11.86ms | 14.21ms | 1.199× | 1.75% | PASS |
| mem_reads | `oltp_order_range` | 2.90ms | 3.15ms | 1.088× | 1.46% | PASS |
| mem_reads | `oltp_distinct_range` | 3.97ms | 4.22ms | 1.064× | 0.77% | PASS |
| mem_reads | `oltp_index_scan` | 4.46ms | 6.20ms | 1.392× | 1.97% | PASS |
| mem_reads | `select_random_points` | 17.79ms | 20.92ms | 1.176× | 2.72% | PASS |
| mem_reads | `select_random_ranges` | 3.88ms | 5.17ms | 1.332× | 1.07% | PASS |
| mem_reads | `covering_index_scan` | 4.52ms | 4.41ms | 0.975× | 1.35% | PASS |
| mem_reads | `groupby_scan` | 31.50ms | 34.61ms | 1.099× | 1.14% | PASS |
| mem_reads | `index_join` | 6.90ms | 8.96ms | 1.300× | 1.55% | PASS |
| mem_reads | `index_join_scan` | 4.62ms | 5.37ms | 1.162× | 3.03% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.23s | 1.165× | 0.73% | PASS |
| mem_reads | `table_scan` | 1.22s | 1.37s | 1.126× | 1.09% | PASS |
| mem_reads | `oltp_read_only` | 118.67ms | 138.10ms | 1.164× | 1.10% | PASS |
| mem_writes | `oltp_bulk_insert` | 234.82ms | 355.78ms | 1.515× | 1.04% | PASS |
| mem_writes | `oltp_insert` | 21.51ms | 39.29ms | 1.826× | 1.10% | PASS |
| mem_writes | `oltp_update_index` | 70.38ms | 132.18ms | 1.878× | 1.05% | PASS |
| mem_writes | `oltp_update_non_index` | 47.93ms | 85.85ms | 1.791× | 1.22% | PASS |
| mem_writes | `oltp_delete_insert` | 50.55ms | 102.73ms | 2.032× | 0.63% | PASS |
| mem_writes | `oltp_write_only` | 28.62ms | 61.13ms | 2.136× | 1.13% | PASS |
| mem_writes | `types_delete_insert` | 32.85ms | 54.73ms | 1.666× | 1.42% | PASS |
| mem_writes | `oltp_read_write` | 86.63ms | 141.78ms | 1.637× | 2.12% | PASS |
| file_reads | `oltp_point_select` | 106.06ms | 65.26ms | 0.615× | 1.04% | PASS |
| file_reads | `oltp_range_select` | 21.73ms | 17.44ms | 0.803× | 1.74% | PASS |
| file_reads | `oltp_sum_range` | 20.24ms | 17.58ms | 0.869× | 2.13% | PASS |
| file_reads | `oltp_order_range` | 3.80ms | 3.51ms | 0.926× | 1.55% | PASS |
| file_reads | `oltp_distinct_range` | 4.91ms | 4.63ms | 0.944× | 1.38% | PASS |
| file_reads | `oltp_index_scan` | 12.17ms | 9.36ms | 0.769× | 1.48% | PASS |
| file_reads | `select_random_points` | 26.85ms | 25.34ms | 0.944× | 1.97% | PASS |
| file_reads | `select_random_ranges` | 11.54ms | 7.96ms | 0.690× | 2.02% | PASS |
| file_reads | `covering_index_scan` | 12.23ms | 7.53ms | 0.615× | 1.39% | PASS |
| file_reads | `groupby_scan` | 32.81ms | 35.24ms | 1.074× | 1.25% | PASS |
| file_reads | `index_join` | 11.23ms | 11.50ms | 1.024× | 1.97% | PASS |
| file_reads | `index_join_scan` | 5.85ms | 6.04ms | 1.031× | 3.50% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.23s | 1.160× | 1.39% | PASS |
| file_reads | `table_scan` | 1.24s | 1.37s | 1.109× | 1.51% | PASS |
| file_reads | `oltp_read_only` | 229.69ms | 177.98ms | 0.775× | 1.07% | PASS |
| file_writes | `oltp_bulk_insert` | 254.49ms | 385.92ms | 1.516× | 1.08% | PASS |
| file_writes | `oltp_insert` | 46.35ms | 52.45ms | 1.131× | 13.81% | PASS |
| file_writes | `oltp_update_index` | 113.86ms | 169.21ms | 1.486× | 1.27% | PASS |
| file_writes | `oltp_update_non_index` | 97.38ms | 113.42ms | 1.165× | 10.91% | PASS |
| file_writes | `oltp_delete_insert` | 89.84ms | 134.70ms | 1.499× | 1.38% | PASS |
| file_writes | `oltp_write_only` | 89.68ms | 85.62ms | 0.955× | 12.21% | PASS |
| file_writes | `types_delete_insert` | 56.14ms | 75.33ms | 1.342× | 1.40% | PASS |
| file_writes | `oltp_read_write` | 142.96ms | 166.99ms | 1.168× | 3.84% | PASS |
| ac_reads | `oltp_point_select` | 55.89ms | 65.48ms | 1.171× | 1.30% | PASS |
| ac_reads | `oltp_range_select` | 16.92ms | 17.40ms | 1.029× | 2.38% | PASS |
| ac_reads | `oltp_sum_range` | 15.36ms | 17.56ms | 1.143× | 1.78% | PASS |
| ac_reads | `oltp_order_range` | 3.34ms | 3.54ms | 1.058× | 2.08% | PASS |
| ac_reads | `oltp_distinct_range` | 4.48ms | 4.63ms | 1.035× | 1.54% | PASS |
| ac_reads | `oltp_index_scan` | 7.33ms | 9.36ms | 1.276× | 2.41% | PASS |
| ac_reads | `select_random_points` | 21.50ms | 25.25ms | 1.175× | 1.63% | PASS |
| ac_reads | `select_random_ranges` | 6.72ms | 7.92ms | 1.179× | 1.67% | PASS |
| ac_reads | `covering_index_scan` | 7.94ms | 7.46ms | 0.940× | 3.11% | PASS |
| ac_reads | `groupby_scan` | 32.20ms | 35.31ms | 1.097× | 0.90% | PASS |
| ac_reads | `index_join` | 8.75ms | 11.57ms | 1.322× | 2.90% | PASS |
| ac_reads | `index_join_scan` | 5.36ms | 6.04ms | 1.126× | 2.82% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.23s | 1.156× | 1.00% | PASS |
| ac_reads | `table_scan` | 1.22s | 1.36s | 1.117× | 0.64% | PASS |
| ac_reads | `oltp_read_only` | 156.34ms | 178.25ms | 1.140× | 1.11% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.45ms | 67.20ms | 2.865× | 9.34% | PASS |
| ac_writes | `oltp_insert_ac` | 26.27ms | 83.14ms | 3.165× | 4.70% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.63ms | 102.10ms | 3.566× | 6.61% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.99ms | 79.76ms | 3.469× | 6.12% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.67ms | 90.85ms | 3.539× | 6.54% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.88ms | 91.44ms | 3.533× | 7.83% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.35ms | 81.67ms | 3.654× | 10.16% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.65ms | 99.00ms | 3.128× | 4.53% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.52ms | 26.96ms | 1.146× | 0.99% | PASS |
| mem_reads | `oltp_range_select` | 10.42ms | 10.58ms | 1.015× | 1.08% | PASS |
| mem_reads | `oltp_sum_range` | 9.43ms | 10.21ms | 1.083× | 0.90% | PASS |
| mem_reads | `oltp_order_range` | 2.39ms | 2.46ms | 1.030× | 1.49% | PASS |
| mem_reads | `oltp_distinct_range` | 3.24ms | 3.25ms | 1.001× | 0.80% | PASS |
| mem_reads | `oltp_index_scan` | 3.62ms | 4.50ms | 1.242× | 1.05% | PASS |
| mem_reads | `select_random_points` | 13.56ms | 15.50ms | 1.143× | 0.54% | PASS |
| mem_reads | `select_random_ranges` | 3.31ms | 4.07ms | 1.229× | 1.07% | PASS |
| mem_reads | `covering_index_scan` | 3.54ms | 3.38ms | 0.954× | 1.53% | PASS |
| mem_reads | `groupby_scan` | 26.33ms | 27.64ms | 1.050× | 0.49% | PASS |
| mem_reads | `index_join` | 5.46ms | 6.82ms | 1.249× | 0.94% | PASS |
| mem_reads | `index_join_scan` | 3.55ms | 4.56ms | 1.285× | 1.03% | PASS |
| mem_reads | `types_table_scan` | 871.73ms | 961.97ms | 1.104× | 0.51% | PASS |
| mem_reads | `table_scan` | 1.01s | 1.05s | 1.042× | 0.43% | PASS |
| mem_reads | `oltp_read_only` | 91.37ms | 99.13ms | 1.085× | 0.65% | PASS |
| mem_writes | `oltp_bulk_insert` | 182.73ms | 253.47ms | 1.387× | 0.70% | PASS |
| mem_writes | `oltp_insert` | 15.76ms | 29.19ms | 1.852× | 0.48% | PASS |
| mem_writes | `oltp_update_index` | 53.64ms | 98.55ms | 1.837× | 0.79% | PASS |
| mem_writes | `oltp_update_non_index` | 38.37ms | 63.50ms | 1.655× | 0.83% | PASS |
| mem_writes | `oltp_delete_insert` | 38.71ms | 77.49ms | 2.002× | 0.74% | PASS |
| mem_writes | `oltp_write_only` | 22.14ms | 46.89ms | 2.118× | 0.57% | PASS |
| mem_writes | `types_delete_insert` | 25.02ms | 38.99ms | 1.559× | 0.74% | PASS |
| mem_writes | `oltp_read_write` | 62.88ms | 101.89ms | 1.620× | 0.72% | PASS |
| file_reads | `oltp_point_select` | 97.89ms | 51.94ms | 0.531× | 0.74% | PASS |
| file_reads | `oltp_range_select` | 18.54ms | 13.23ms | 0.713× | 1.22% | PASS |
| file_reads | `oltp_sum_range` | 17.37ms | 12.92ms | 0.744× | 0.90% | PASS |
| file_reads | `oltp_order_range` | 3.23ms | 2.81ms | 0.869× | 1.51% | PASS |
| file_reads | `oltp_distinct_range` | 4.09ms | 3.58ms | 0.874× | 1.00% | PASS |
| file_reads | `oltp_index_scan` | 11.45ms | 7.42ms | 0.648× | 0.81% | PASS |
| file_reads | `select_random_points` | 21.73ms | 18.35ms | 0.845× | 0.96% | PASS |
| file_reads | `select_random_ranges` | 10.91ms | 6.66ms | 0.610× | 0.77% | PASS |
| file_reads | `covering_index_scan` | 11.78ms | 6.30ms | 0.534× | 1.00% | PASS |
| file_reads | `groupby_scan` | 27.43ms | 27.99ms | 1.020× | 0.86% | PASS |
| file_reads | `index_join` | 9.96ms | 9.06ms | 0.910× | 0.75% | PASS |
| file_reads | `index_join_scan` | 4.45ms | 4.96ms | 1.114× | 1.07% | PASS |
| file_reads | `types_table_scan` | 873.18ms | 960.93ms | 1.100× | 0.55% | PASS |
| file_reads | `table_scan` | 1.01s | 1.05s | 1.039× | 0.30% | PASS |
| file_reads | `oltp_read_only` | 199.41ms | 134.61ms | 0.675× | 0.34% | PASS |
| file_writes | `oltp_bulk_insert` | 242.97ms | 337.38ms | 1.389× | 4.06% | PASS |
| file_writes | `oltp_insert` | 51.60ms | 60.02ms | 1.163× | 5.12% | PASS |
| file_writes | `oltp_update_index` | 185.37ms | 195.09ms | 1.052× | 5.36% | PASS |
| file_writes | `oltp_update_non_index` | 144.64ms | 133.21ms | 0.921× | 9.22% | PASS |
| file_writes | `oltp_delete_insert` | 151.10ms | 159.52ms | 1.056× | 3.96% | PASS |
| file_writes | `oltp_write_only` | 112.45ms | 111.24ms | 0.989× | 3.76% | PASS |
| file_writes | `types_delete_insert` | 94.29ms | 89.83ms | 0.953× | 5.42% | PASS |
| file_writes | `oltp_read_write` | 156.67ms | 166.94ms | 1.066× | 6.34% | PASS |
| ac_reads | `oltp_point_select` | 48.05ms | 51.72ms | 1.076× | 0.61% | PASS |
| ac_reads | `oltp_range_select` | 13.78ms | 13.25ms | 0.961× | 0.60% | PASS |
| ac_reads | `oltp_sum_range` | 12.65ms | 12.90ms | 1.020× | 0.84% | PASS |
| ac_reads | `oltp_order_range` | 2.88ms | 2.80ms | 0.974× | 1.05% | PASS |
| ac_reads | `oltp_distinct_range` | 3.67ms | 3.58ms | 0.975× | 0.70% | PASS |
| ac_reads | `oltp_index_scan` | 6.66ms | 7.45ms | 1.119× | 0.77% | PASS |
| ac_reads | `select_random_points` | 16.95ms | 18.36ms | 1.083× | 0.66% | PASS |
| ac_reads | `select_random_ranges` | 6.10ms | 6.66ms | 1.092× | 0.66% | PASS |
| ac_reads | `covering_index_scan` | 6.94ms | 6.29ms | 0.907× | 0.55% | PASS |
| ac_reads | `groupby_scan` | 27.06ms | 27.96ms | 1.033× | 0.61% | PASS |
| ac_reads | `index_join` | 7.54ms | 9.08ms | 1.203× | 0.72% | PASS |
| ac_reads | `index_join_scan` | 4.07ms | 4.97ms | 1.222× | 0.84% | PASS |
| ac_reads | `types_table_scan` | 872.37ms | 958.70ms | 1.099× | 0.47% | PASS |
| ac_reads | `table_scan` | 1.00s | 1.05s | 1.043× | 0.34% | PASS |
| ac_reads | `oltp_read_only` | 127.22ms | 134.49ms | 1.057× | 0.52% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 18.21ms | 56.88ms | 3.124× | 35.33% | PASS |
| ac_writes | `oltp_insert_ac` | 20.52ms | 83.13ms | 4.050× | 20.63% | PASS |
| ac_writes | `oltp_update_index_ac` | 22.41ms | 85.94ms | 3.834× | 20.17% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.43ms | 66.43ms | 3.811× | 15.35% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.61ms | 72.30ms | 3.886× | 7.97% | PASS |
| ac_writes | `oltp_write_only_ac` | 20.01ms | 74.84ms | 3.739× | 9.41% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.69ms | 70.48ms | 3.983× | 17.62% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.67ms | 89.93ms | 3.645× | 33.04% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.02ms | 40.53ms | 1.191× | 1.88% | PASS |
| mem_reads | `oltp_range_select` | 19.74ms | 21.13ms | 1.070× | 1.58% | PASS |
| mem_reads | `oltp_sum_range` | 18.31ms | 20.96ms | 1.145× | 1.34% | PASS |
| mem_reads | `oltp_order_range` | 3.59ms | 3.86ms | 1.077× | 0.83% | PASS |
| mem_reads | `oltp_distinct_range` | 4.70ms | 4.94ms | 1.053× | 1.30% | PASS |
| mem_reads | `oltp_index_scan` | 4.68ms | 6.47ms | 1.382× | 1.54% | PASS |
| mem_reads | `select_random_points` | 29.03ms | 32.99ms | 1.136× | 1.64% | PASS |
| mem_reads | `select_random_ranges` | 7.88ms | 9.20ms | 1.167× | 1.12% | PASS |
| mem_reads | `covering_index_scan` | 4.28ms | 4.68ms | 1.092× | 2.46% | PASS |
| mem_reads | `groupby_scan` | 36.77ms | 39.43ms | 1.072× | 0.61% | PASS |
| mem_reads | `index_join` | 8.16ms | 10.88ms | 1.334× | 2.02% | PASS |
| mem_reads | `index_join_scan` | 4.14ms | 5.38ms | 1.298× | 2.11% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.24s | 1.185× | 1.81% | PASS |
| mem_reads | `table_scan` | 1.25s | 1.40s | 1.115× | 2.84% | PASS |
| mem_reads | `oltp_read_only` | 155.46ms | 172.10ms | 1.107× | 1.47% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.16ms | 357.58ms | 1.435× | 1.13% | PASS |
| mem_writes | `oltp_insert` | 19.21ms | 36.33ms | 1.891× | 0.82% | PASS |
| mem_writes | `oltp_update_index` | 67.50ms | 116.11ms | 1.720× | 1.40% | PASS |
| mem_writes | `oltp_update_non_index` | 50.13ms | 82.83ms | 1.652× | 1.21% | PASS |
| mem_writes | `oltp_delete_insert` | 49.60ms | 95.45ms | 1.924× | 1.10% | PASS |
| mem_writes | `oltp_write_only` | 26.96ms | 57.48ms | 2.132× | 1.00% | PASS |
| mem_writes | `types_delete_insert` | 32.49ms | 54.77ms | 1.686× | 1.32% | PASS |
| mem_writes | `oltp_read_write` | 103.59ms | 155.86ms | 1.505× | 1.50% | PASS |
| file_reads | `oltp_point_select` | 108.77ms | 66.76ms | 0.614× | 1.22% | PASS |
| file_reads | `oltp_range_select` | 27.27ms | 24.37ms | 0.894× | 2.30% | PASS |
| file_reads | `oltp_sum_range` | 26.02ms | 23.97ms | 0.921× | 1.60% | PASS |
| file_reads | `oltp_order_range` | 4.29ms | 4.21ms | 0.982× | 1.51% | PASS |
| file_reads | `oltp_distinct_range` | 5.58ms | 5.32ms | 0.953× | 1.74% | PASS |
| file_reads | `oltp_index_scan` | 12.32ms | 9.10ms | 0.739× | 1.91% | PASS |
| file_reads | `select_random_points` | 37.37ms | 36.07ms | 0.965× | 1.45% | PASS |
| file_reads | `select_random_ranges` | 15.55ms | 12.01ms | 0.772× | 1.26% | PASS |
| file_reads | `covering_index_scan` | 11.90ms | 7.17ms | 0.602× | 0.94% | PASS |
| file_reads | `groupby_scan` | 37.48ms | 39.86ms | 1.063× | 1.03% | PASS |
| file_reads | `index_join` | 12.63ms | 12.88ms | 1.019× | 1.57% | PASS |
| file_reads | `index_join_scan` | 5.21ms | 5.93ms | 1.139× | 1.96% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.24s | 1.186× | 1.26% | PASS |
| file_reads | `table_scan` | 1.35s | 1.41s | 1.049× | 1.78% | PASS |
| file_reads | `oltp_read_only` | 267.49ms | 211.81ms | 0.792× | 1.15% | PASS |
| file_writes | `oltp_bulk_insert` | 264.05ms | 383.62ms | 1.453× | 0.89% | PASS |
| file_writes | `oltp_insert` | 26.11ms | 46.55ms | 1.783× | 1.11% | PASS |
| file_writes | `oltp_update_index` | 96.63ms | 142.32ms | 1.473× | 1.20% | PASS |
| file_writes | `oltp_update_non_index` | 75.40ms | 103.78ms | 1.376× | 1.56% | PASS |
| file_writes | `oltp_delete_insert` | 77.77ms | 120.07ms | 1.544× | 1.39% | PASS |
| file_writes | `oltp_write_only` | 51.95ms | 78.00ms | 1.501× | 1.73% | PASS |
| file_writes | `types_delete_insert` | 49.43ms | 68.72ms | 1.390× | 1.00% | PASS |
| file_writes | `oltp_read_write` | 124.59ms | 173.43ms | 1.392× | 1.64% | PASS |
| ac_reads | `oltp_point_select` | 57.64ms | 66.39ms | 1.152× | 0.96% | PASS |
| ac_reads | `oltp_range_select` | 21.66ms | 24.14ms | 1.114× | 1.39% | PASS |
| ac_reads | `oltp_sum_range` | 21.21ms | 23.98ms | 1.131× | 1.50% | PASS |
| ac_reads | `oltp_order_range` | 3.95ms | 4.23ms | 1.071× | 0.85% | PASS |
| ac_reads | `oltp_distinct_range` | 5.09ms | 5.31ms | 1.045× | 1.29% | PASS |
| ac_reads | `oltp_index_scan` | 7.46ms | 9.13ms | 1.224× | 1.47% | PASS |
| ac_reads | `select_random_points` | 31.17ms | 36.41ms | 1.168× | 1.58% | PASS |
| ac_reads | `select_random_ranges` | 10.21ms | 11.99ms | 1.174× | 1.55% | PASS |
| ac_reads | `covering_index_scan` | 6.91ms | 7.10ms | 1.028× | 1.51% | PASS |
| ac_reads | `groupby_scan` | 36.78ms | 39.90ms | 1.085× | 0.79% | PASS |
| ac_reads | `index_join` | 10.00ms | 12.81ms | 1.281× | 1.70% | PASS |
| ac_reads | `index_join_scan` | 4.65ms | 5.95ms | 1.280× | 2.32% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.24s | 1.192× | 0.64% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.38s | 1.181× | 0.55% | PASS |
| ac_reads | `oltp_read_only` | 182.88ms | 208.46ms | 1.140× | 1.00% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.17ms | 63.81ms | 3.014× | 4.39% | PASS |
| ac_writes | `oltp_insert_ac` | 23.19ms | 83.98ms | 3.622× | 2.26% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.08ms | 93.31ms | 3.720× | 2.62% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.75ms | 73.98ms | 3.401× | 3.22% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.85ms | 84.24ms | 3.686× | 2.36% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.30ms | 84.56ms | 3.630× | 3.19% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.79ms | 74.08ms | 3.564× | 5.11% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.66ms | 92.43ms | 3.117× | 2.05% | PASS |

</details>

## Version-control latency

Wall time: 2m 23s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 84.75ms | 200.00ms | 42.4% | 0.43% | PASS |
| `status_dirty_many_tables` | 87.80ms | 200.00ms | 43.9% | 0.45% | PASS |
| `diff_regular_working_one_table` | 80.08ms | 150.00ms | 53.4% | 0.43% | PASS |
| `diff_regular_working_many_tables` | 93.56ms | 200.00ms | 46.8% | 0.87% | PASS |
| `diff_stat_working_many_tables` | 92.94ms | 200.00ms | 46.5% | 1.85% | PASS |
| `diff_schema_working_many_tables` | 92.09ms | 200.00ms | 46.0% | 0.58% | PASS |
| `branch_list_many_branches` | 22.95ms | 100.00ms | 23.0% | 1.65% | PASS |
| `branch_create_delete` | 24.92ms | 100.00ms | 24.9% | 1.87% | PASS |
| `checkout_branch_clean` | 56.17ms | 200.00ms | 28.1% | 0.95% | PASS |
| `merge_data_no_conflicts` | 28.89ms | 150.00ms | 19.3% | 0.93% | PASS |
| `merge_schema_no_conflicts` | 21.91ms | 100.00ms | 21.9% | 1.32% | PASS |
| `merge_data_conflicts` | 127.31ms | 250.00ms | 50.9% | 0.41% | PASS |
| `merge_data_conflicts_with_resolve` | 127.80ms | 250.00ms | 51.1% | 0.55% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
