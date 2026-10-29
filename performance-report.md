# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-29 18:00 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261026.354.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/40910809545)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 14m 21s | 9.01s | 11.49s | 1.275× | 1.55% | **PASS** |
| textpk | 69 | 55 | 1h 35m 0s | 10.26s | 11.90s | 1.160× | 1.72% | **PASS** |
| blobpk | 69 | 55 | 1h 27m 51s | 9.05s | 9.88s | 1.091× | 1.65% | **PASS** |
| compositepk | 69 | 55 | 1h 29m 36s | 10.63s | 12.00s | 1.129× | 1.20% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.46ms | 29.59ms | 1.261× | 1.52% | PASS |
| mem_reads | `oltp_range_select` | 10.13ms | 13.28ms | 1.311× | 2.18% | PASS |
| mem_reads | `oltp_sum_range` | 9.70ms | 12.43ms | 1.281× | 1.60% | PASS |
| mem_reads | `oltp_order_range` | 2.58ms | 3.05ms | 1.181× | 1.22% | PASS |
| mem_reads | `oltp_distinct_range` | 3.67ms | 4.09ms | 1.117× | 1.47% | PASS |
| mem_reads | `oltp_index_scan` | 3.91ms | 5.36ms | 1.371× | 1.85% | PASS |
| mem_reads | `select_random_points` | 10.15ms | 11.29ms | 1.113× | 1.99% | PASS |
| mem_reads | `select_random_ranges` | 3.07ms | 4.06ms | 1.321× | 2.07% | PASS |
| mem_reads | `covering_index_scan` | 4.21ms | 4.16ms | 0.989× | 1.66% | PASS |
| mem_reads | `groupby_scan` | 29.73ms | 33.23ms | 1.118× | 0.82% | PASS |
| mem_reads | `index_join` | 6.01ms | 8.17ms | 1.359× | 1.44% | PASS |
| mem_reads | `index_join_scan` | 3.52ms | 4.63ms | 1.317× | 2.69% | PASS |
| mem_reads | `types_table_scan` | 1.09s | 1.35s | 1.241× | 0.90% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.39s | 1.191× | 0.66% | PASS |
| mem_reads | `oltp_read_only` | 101.88ms | 124.24ms | 1.219× | 1.12% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.85ms | 262.59ms | 1.444× | 1.45% | PASS |
| mem_writes | `oltp_insert` | 15.57ms | 29.45ms | 1.892× | 0.88% | PASS |
| mem_writes | `oltp_update_index` | 50.76ms | 111.80ms | 2.202× | 1.30% | PASS |
| mem_writes | `oltp_update_non_index` | 35.08ms | 65.18ms | 1.858× | 1.55% | PASS |
| mem_writes | `oltp_delete_insert` | 46.61ms | 85.97ms | 1.845× | 1.32% | PASS |
| mem_writes | `oltp_write_only` | 23.03ms | 50.60ms | 2.197× | 1.28% | PASS |
| mem_writes | `types_delete_insert` | 26.10ms | 45.75ms | 1.753× | 1.87% | PASS |
| mem_writes | `oltp_read_write` | 73.39ms | 119.99ms | 1.635× | 2.17% | PASS |
| file_reads | `oltp_point_select` | 95.04ms | 54.51ms | 0.574× | 0.87% | PASS |
| file_reads | `oltp_range_select` | 18.39ms | 15.98ms | 0.869× | 1.63% | PASS |
| file_reads | `oltp_sum_range` | 17.61ms | 15.23ms | 0.865× | 1.28% | PASS |
| file_reads | `oltp_order_range` | 3.36ms | 3.34ms | 0.994× | 2.19% | PASS |
| file_reads | `oltp_distinct_range` | 4.49ms | 4.41ms | 0.982× | 1.61% | PASS |
| file_reads | `oltp_index_scan` | 11.30ms | 8.25ms | 0.730× | 1.30% | PASS |
| file_reads | `select_random_points` | 18.07ms | 13.99ms | 0.774× | 1.87% | PASS |
| file_reads | `select_random_ranges` | 9.95ms | 6.45ms | 0.648× | 1.16% | PASS |
| file_reads | `covering_index_scan` | 11.34ms | 6.92ms | 0.610× | 1.07% | PASS |
| file_reads | `groupby_scan` | 30.73ms | 33.84ms | 1.101× | 1.18% | PASS |
| file_reads | `index_join` | 10.21ms | 10.31ms | 1.010× | 2.06% | PASS |
| file_reads | `index_join_scan` | 4.52ms | 5.05ms | 1.117× | 1.85% | PASS |
| file_reads | `types_table_scan` | 1.09s | 1.35s | 1.234× | 1.20% | PASS |
| file_reads | `table_scan` | 1.27s | 1.42s | 1.119× | 1.03% | PASS |
| file_reads | `oltp_read_only` | 210.27ms | 162.24ms | 0.772× | 0.63% | PASS |
| file_writes | `oltp_bulk_insert` | 196.17ms | 281.30ms | 1.434× | 1.38% | PASS |
| file_writes | `oltp_insert` | 22.21ms | 36.24ms | 1.632× | 1.33% | PASS |
| file_writes | `oltp_update_index` | 79.08ms | 140.57ms | 1.778× | 1.16% | PASS |
| file_writes | `oltp_update_non_index` | 58.69ms | 85.26ms | 1.453× | 1.42% | PASS |
| file_writes | `oltp_delete_insert` | 68.70ms | 110.46ms | 1.608× | 2.06% | PASS |
| file_writes | `oltp_write_only` | 45.03ms | 67.43ms | 1.498× | 1.65% | PASS |
| file_writes | `types_delete_insert` | 40.76ms | 54.35ms | 1.334× | 1.63% | PASS |
| file_writes | `oltp_read_write` | 96.83ms | 135.78ms | 1.402× | 2.09% | PASS |
| ac_reads | `oltp_point_select` | 48.20ms | 54.01ms | 1.120× | 1.11% | PASS |
| ac_reads | `oltp_range_select` | 12.92ms | 15.72ms | 1.217× | 1.56% | PASS |
| ac_reads | `oltp_sum_range` | 12.40ms | 15.01ms | 1.211× | 1.74% | PASS |
| ac_reads | `oltp_order_range` | 2.97ms | 3.35ms | 1.128× | 1.69% | PASS |
| ac_reads | `oltp_distinct_range` | 4.03ms | 4.41ms | 1.095× | 2.23% | PASS |
| ac_reads | `oltp_index_scan` | 6.64ms | 8.19ms | 1.233× | 1.44% | PASS |
| ac_reads | `select_random_points` | 12.92ms | 13.77ms | 1.066× | 1.70% | PASS |
| ac_reads | `select_random_ranges` | 5.52ms | 6.43ms | 1.164× | 1.55% | PASS |
| ac_reads | `covering_index_scan` | 7.03ms | 6.93ms | 0.987× | 1.56% | PASS |
| ac_reads | `groupby_scan` | 30.21ms | 33.78ms | 1.118× | 0.89% | PASS |
| ac_reads | `index_join` | 7.71ms | 10.12ms | 1.312× | 1.32% | PASS |
| ac_reads | `index_join_scan` | 3.99ms | 5.02ms | 1.258× | 2.13% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.32s | 1.283× | 0.52% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.39s | 1.191× | 0.54% | PASS |
| ac_reads | `oltp_read_only` | 135.95ms | 159.37ms | 1.172× | 0.93% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.47ms | 63.50ms | 2.958× | 3.55% | PASS |
| ac_writes | `oltp_insert_ac` | 22.59ms | 79.59ms | 3.523× | 4.45% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.48ms | 95.35ms | 3.743× | 4.45% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.76ms | 72.18ms | 3.477× | 3.70% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.45ms | 86.11ms | 3.672× | 5.91% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.81ms | 84.96ms | 3.568× | 4.16% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.16ms | 73.34ms | 3.638× | 4.31% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.59ms | 90.33ms | 3.274× | 4.80% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.02ms | 37.74ms | 1.257× | 1.69% | PASS |
| mem_reads | `oltp_range_select` | 13.58ms | 14.25ms | 1.050× | 2.64% | PASS |
| mem_reads | `oltp_sum_range` | 12.24ms | 14.47ms | 1.182× | 1.23% | PASS |
| mem_reads | `oltp_order_range` | 3.00ms | 3.16ms | 1.051× | 1.51% | PASS |
| mem_reads | `oltp_distinct_range` | 4.09ms | 4.25ms | 1.038× | 0.90% | PASS |
| mem_reads | `oltp_index_scan` | 4.55ms | 6.42ms | 1.413× | 1.67% | PASS |
| mem_reads | `select_random_points` | 17.79ms | 20.76ms | 1.167× | 1.66% | PASS |
| mem_reads | `select_random_ranges` | 4.03ms | 5.18ms | 1.287× | 1.30% | PASS |
| mem_reads | `covering_index_scan` | 4.71ms | 4.64ms | 0.984× | 2.92% | PASS |
| mem_reads | `groupby_scan` | 32.36ms | 34.35ms | 1.062× | 0.95% | PASS |
| mem_reads | `index_join` | 7.01ms | 9.66ms | 1.378× | 2.58% | PASS |
| mem_reads | `index_join_scan` | 4.67ms | 5.44ms | 1.165× | 3.65% | PASS |
| mem_reads | `types_table_scan` | 1.10s | 1.25s | 1.139× | 1.96% | PASS |
| mem_reads | `table_scan` | 1.28s | 1.38s | 1.081× | 3.75% | PASS |
| mem_reads | `oltp_read_only` | 118.95ms | 137.12ms | 1.153× | 1.38% | PASS |
| mem_writes | `oltp_bulk_insert` | 231.05ms | 366.69ms | 1.587× | 0.81% | PASS |
| mem_writes | `oltp_insert` | 21.56ms | 41.88ms | 1.943× | 1.23% | PASS |
| mem_writes | `oltp_update_index` | 71.15ms | 143.01ms | 2.010× | 1.30% | PASS |
| mem_writes | `oltp_update_non_index` | 48.20ms | 92.72ms | 1.924× | 1.11% | PASS |
| mem_writes | `oltp_delete_insert` | 50.39ms | 109.96ms | 2.182× | 1.33% | PASS |
| mem_writes | `oltp_write_only` | 28.98ms | 66.98ms | 2.311× | 1.42% | PASS |
| mem_writes | `types_delete_insert` | 32.87ms | 60.42ms | 1.838× | 1.14% | PASS |
| mem_writes | `oltp_read_write` | 83.64ms | 145.35ms | 1.738× | 1.80% | PASS |
| file_reads | `oltp_point_select` | 100.31ms | 61.28ms | 0.611× | 1.24% | PASS |
| file_reads | `oltp_range_select` | 20.15ms | 16.72ms | 0.830× | 1.81% | PASS |
| file_reads | `oltp_sum_range` | 19.31ms | 16.87ms | 0.874× | 2.63% | PASS |
| file_reads | `oltp_order_range` | 3.72ms | 3.44ms | 0.923× | 1.62% | PASS |
| file_reads | `oltp_distinct_range` | 4.86ms | 4.54ms | 0.934× | 1.33% | PASS |
| file_reads | `oltp_index_scan` | 11.65ms | 8.88ms | 0.763× | 1.37% | PASS |
| file_reads | `select_random_points` | 25.85ms | 24.19ms | 0.936× | 2.11% | PASS |
| file_reads | `select_random_ranges` | 11.01ms | 7.59ms | 0.689× | 1.23% | PASS |
| file_reads | `covering_index_scan` | 11.85ms | 7.13ms | 0.602× | 1.71% | PASS |
| file_reads | `groupby_scan` | 32.87ms | 34.79ms | 1.059× | 1.39% | PASS |
| file_reads | `index_join` | 11.09ms | 11.28ms | 1.017× | 2.88% | PASS |
| file_reads | `index_join_scan` | 5.51ms | 5.92ms | 1.075× | 4.63% | PASS |
| file_reads | `types_table_scan` | 1.08s | 1.24s | 1.149× | 1.58% | PASS |
| file_reads | `table_scan` | 1.40s | 1.39s | 0.991× | 6.87% | PASS |
| file_reads | `oltp_read_only` | 236.88ms | 178.41ms | 0.753× | 1.07% | PASS |
| file_writes | `oltp_bulk_insert` | 251.02ms | 402.19ms | 1.602× | 1.07% | PASS |
| file_writes | `oltp_insert` | 52.01ms | 58.23ms | 1.120× | 14.73% | PASS |
| file_writes | `oltp_update_index` | 122.68ms | 188.98ms | 1.540× | 1.72% | PASS |
| file_writes | `oltp_update_non_index` | 112.54ms | 124.53ms | 1.107× | 12.03% | PASS |
| file_writes | `oltp_delete_insert` | 94.78ms | 148.17ms | 1.563× | 3.53% | PASS |
| file_writes | `oltp_write_only` | 87.55ms | 94.56ms | 1.080× | 11.30% | PASS |
| file_writes | `types_delete_insert` | 56.59ms | 80.01ms | 1.414× | 1.72% | PASS |
| file_writes | `oltp_read_write` | 140.00ms | 172.75ms | 1.234× | 6.71% | PASS |
| ac_reads | `oltp_point_select` | 53.78ms | 61.63ms | 1.146× | 1.35% | PASS |
| ac_reads | `oltp_range_select` | 16.86ms | 16.88ms | 1.001× | 3.42% | PASS |
| ac_reads | `oltp_sum_range` | 15.81ms | 17.07ms | 1.080× | 2.24% | PASS |
| ac_reads | `oltp_order_range` | 3.44ms | 3.48ms | 1.010× | 1.21% | PASS |
| ac_reads | `oltp_distinct_range` | 4.58ms | 4.61ms | 1.006× | 1.26% | PASS |
| ac_reads | `oltp_index_scan` | 7.54ms | 9.07ms | 1.203× | 1.30% | PASS |
| ac_reads | `select_random_points` | 21.55ms | 24.37ms | 1.131× | 1.99% | PASS |
| ac_reads | `select_random_ranges` | 6.67ms | 7.62ms | 1.143× | 1.38% | PASS |
| ac_reads | `covering_index_scan` | 8.48ms | 7.22ms | 0.852× | 1.45% | PASS |
| ac_reads | `groupby_scan` | 32.98ms | 34.95ms | 1.060× | 0.80% | PASS |
| ac_reads | `index_join` | 9.64ms | 11.62ms | 1.206× | 2.31% | PASS |
| ac_reads | `index_join_scan` | 5.19ms | 5.96ms | 1.148× | 3.62% | PASS |
| ac_reads | `types_table_scan` | 1.16s | 1.26s | 1.085× | 4.08% | PASS |
| ac_reads | `table_scan` | 1.48s | 1.40s | 0.947× | 4.13% | PASS |
| ac_reads | `oltp_read_only` | 159.49ms | 173.13ms | 1.086× | 1.60% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.10ms | 62.12ms | 2.943× | 5.98% | PASS |
| ac_writes | `oltp_insert_ac` | 24.37ms | 75.79ms | 3.110× | 5.82% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.71ms | 93.33ms | 3.630× | 5.49% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.72ms | 72.72ms | 3.510× | 7.40% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 21.74ms | 83.74ms | 3.852× | 3.23% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.30ms | 83.09ms | 3.566× | 5.48% | PASS |
| ac_writes | `types_delete_insert_ac` | 19.12ms | 72.28ms | 3.780× | 6.33% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.83ms | 90.69ms | 3.040× | 5.24% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.16ms | 27.58ms | 1.142× | 1.70% | PASS |
| mem_reads | `oltp_range_select` | 11.19ms | 10.88ms | 0.972× | 3.31% | PASS |
| mem_reads | `oltp_sum_range` | 10.12ms | 10.65ms | 1.053× | 1.51% | PASS |
| mem_reads | `oltp_order_range` | 2.55ms | 2.55ms | 1.002× | 1.43% | PASS |
| mem_reads | `oltp_distinct_range` | 3.42ms | 3.36ms | 0.983× | 0.83% | PASS |
| mem_reads | `oltp_index_scan` | 3.94ms | 5.16ms | 1.307× | 2.08% | PASS |
| mem_reads | `select_random_points` | 15.24ms | 17.05ms | 1.119× | 1.59% | PASS |
| mem_reads | `select_random_ranges` | 3.56ms | 4.25ms | 1.196× | 1.73% | PASS |
| mem_reads | `covering_index_scan` | 3.70ms | 3.92ms | 1.062× | 3.25% | PASS |
| mem_reads | `groupby_scan` | 27.03ms | 27.88ms | 1.031× | 0.93% | PASS |
| mem_reads | `index_join` | 5.70ms | 8.28ms | 1.452× | 2.40% | PASS |
| mem_reads | `index_join_scan` | 3.85ms | 5.01ms | 1.300× | 1.70% | PASS |
| mem_reads | `types_table_scan` | 928.16ms | 1.01s | 1.087× | 2.93% | PASS |
| mem_reads | `table_scan` | 1.26s | 1.11s | 0.887× | 2.42% | PASS |
| mem_reads | `oltp_read_only` | 106.16ms | 107.24ms | 1.010× | 1.96% | PASS |
| mem_writes | `oltp_bulk_insert` | 185.63ms | 274.31ms | 1.478× | 1.01% | PASS |
| mem_writes | `oltp_insert` | 17.11ms | 34.24ms | 2.002× | 2.13% | PASS |
| mem_writes | `oltp_update_index` | 64.48ms | 123.29ms | 1.912× | 2.47% | PASS |
| mem_writes | `oltp_update_non_index` | 42.70ms | 74.90ms | 1.754× | 1.54% | PASS |
| mem_writes | `oltp_delete_insert` | 44.58ms | 93.85ms | 2.105× | 1.32% | PASS |
| mem_writes | `oltp_write_only` | 25.36ms | 57.11ms | 2.252× | 2.36% | PASS |
| mem_writes | `types_delete_insert` | 26.13ms | 45.36ms | 1.736× | 1.31% | PASS |
| mem_writes | `oltp_read_write` | 71.01ms | 114.03ms | 1.606× | 1.65% | PASS |
| file_reads | `oltp_point_select` | 94.33ms | 51.47ms | 0.546× | 1.15% | PASS |
| file_reads | `oltp_range_select` | 19.08ms | 13.28ms | 0.696× | 1.62% | PASS |
| file_reads | `oltp_sum_range` | 17.76ms | 12.98ms | 0.731× | 2.24% | PASS |
| file_reads | `oltp_order_range` | 3.35ms | 2.81ms | 0.840× | 0.92% | PASS |
| file_reads | `oltp_distinct_range` | 4.19ms | 3.57ms | 0.853× | 0.83% | PASS |
| file_reads | `oltp_index_scan` | 11.08ms | 7.40ms | 0.668× | 0.81% | PASS |
| file_reads | `select_random_points` | 21.71ms | 18.55ms | 0.854× | 1.35% | PASS |
| file_reads | `select_random_ranges` | 10.57ms | 6.57ms | 0.621× | 0.68% | PASS |
| file_reads | `covering_index_scan` | 11.49ms | 6.20ms | 0.540× | 0.58% | PASS |
| file_reads | `groupby_scan` | 27.83ms | 28.08ms | 1.009× | 0.65% | PASS |
| file_reads | `index_join` | 9.98ms | 9.27ms | 0.929× | 1.31% | PASS |
| file_reads | `index_join_scan` | 4.58ms | 5.09ms | 1.112× | 1.67% | PASS |
| file_reads | `types_table_scan` | 876.52ms | 967.70ms | 1.104× | 0.82% | PASS |
| file_reads | `table_scan` | 1.03s | 1.06s | 1.024× | 1.29% | PASS |
| file_reads | `oltp_read_only` | 195.24ms | 133.68ms | 0.685× | 1.03% | PASS |
| file_writes | `oltp_bulk_insert` | 256.88ms | 364.37ms | 1.418× | 6.86% | PASS |
| file_writes | `oltp_insert` | 44.57ms | 64.69ms | 1.451× | 13.95% | PASS |
| file_writes | `oltp_update_index` | 195.22ms | 224.43ms | 1.150× | 12.00% | PASS |
| file_writes | `oltp_update_non_index` | 149.64ms | 144.84ms | 0.968× | 8.39% | PASS |
| file_writes | `oltp_delete_insert` | 157.98ms | 170.42ms | 1.079× | 4.87% | PASS |
| file_writes | `oltp_write_only` | 116.46ms | 121.42ms | 1.043× | 1.60% | PASS |
| file_writes | `types_delete_insert` | 109.81ms | 102.45ms | 0.933× | 9.87% | PASS |
| file_writes | `oltp_read_write` | 165.55ms | 180.15ms | 1.088× | 7.19% | PASS |
| ac_reads | `oltp_point_select` | 47.48ms | 50.78ms | 1.070× | 0.80% | PASS |
| ac_reads | `oltp_range_select` | 14.67ms | 13.31ms | 0.907× | 1.46% | PASS |
| ac_reads | `oltp_sum_range` | 13.03ms | 12.94ms | 0.993× | 1.77% | PASS |
| ac_reads | `oltp_order_range` | 2.92ms | 2.81ms | 0.961× | 1.07% | PASS |
| ac_reads | `oltp_distinct_range` | 3.73ms | 3.56ms | 0.955× | 0.52% | PASS |
| ac_reads | `oltp_index_scan` | 6.64ms | 7.44ms | 1.120× | 0.96% | PASS |
| ac_reads | `select_random_points` | 17.48ms | 18.98ms | 1.086× | 1.16% | PASS |
| ac_reads | `select_random_ranges` | 6.11ms | 6.58ms | 1.076× | 0.64% | PASS |
| ac_reads | `covering_index_scan` | 6.95ms | 6.22ms | 0.895× | 0.81% | PASS |
| ac_reads | `groupby_scan` | 27.29ms | 28.02ms | 1.027× | 0.65% | PASS |
| ac_reads | `index_join` | 7.72ms | 9.27ms | 1.201× | 1.60% | PASS |
| ac_reads | `index_join_scan` | 4.17ms | 5.20ms | 1.246× | 1.84% | PASS |
| ac_reads | `types_table_scan` | 922.31ms | 991.00ms | 1.074× | 2.01% | PASS |
| ac_reads | `table_scan` | 1.25s | 1.11s | 0.888× | 2.84% | PASS |
| ac_reads | `oltp_read_only` | 136.71ms | 136.95ms | 1.002× | 1.26% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.84ms | 55.52ms | 3.113× | 18.34% | PASS |
| ac_writes | `oltp_insert_ac` | 20.75ms | 77.09ms | 3.715× | 14.57% | PASS |
| ac_writes | `oltp_update_index_ac` | 22.83ms | 90.58ms | 3.967× | 22.38% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 18.52ms | 68.29ms | 3.688× | 15.27% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 20.28ms | 78.59ms | 3.876× | 13.29% | PASS |
| ac_writes | `oltp_write_only_ac` | 20.09ms | 77.78ms | 3.871× | 32.74% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.94ms | 66.87ms | 3.947× | 17.18% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.73ms | 86.63ms | 3.503× | 16.09% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.68ms | 37.49ms | 1.113× | 1.52% | PASS |
| mem_reads | `oltp_range_select` | 19.88ms | 20.11ms | 1.012× | 1.30% | PASS |
| mem_reads | `oltp_sum_range` | 18.40ms | 19.06ms | 1.036× | 1.20% | PASS |
| mem_reads | `oltp_order_range` | 3.74ms | 3.78ms | 1.011× | 0.74% | PASS |
| mem_reads | `oltp_distinct_range` | 4.82ms | 4.82ms | 1.001× | 0.86% | PASS |
| mem_reads | `oltp_index_scan` | 4.70ms | 5.90ms | 1.257× | 1.21% | PASS |
| mem_reads | `select_random_points` | 27.67ms | 30.33ms | 1.096× | 0.82% | PASS |
| mem_reads | `select_random_ranges` | 7.56ms | 8.24ms | 1.090× | 1.32% | PASS |
| mem_reads | `covering_index_scan` | 4.37ms | 4.32ms | 0.989× | 1.97% | PASS |
| mem_reads | `groupby_scan` | 38.76ms | 40.10ms | 1.035× | 0.62% | PASS |
| mem_reads | `index_join` | 8.17ms | 10.12ms | 1.239× | 0.91% | PASS |
| mem_reads | `index_join_scan` | 4.33ms | 5.65ms | 1.304× | 1.20% | PASS |
| mem_reads | `types_table_scan` | 1.14s | 1.25s | 1.104× | 0.59% | PASS |
| mem_reads | `table_scan` | 1.35s | 1.37s | 1.017× | 0.89% | PASS |
| mem_reads | `oltp_read_only` | 152.20ms | 158.83ms | 1.044× | 1.03% | PASS |
| mem_writes | `oltp_bulk_insert` | 246.11ms | 351.94ms | 1.430× | 0.82% | PASS |
| mem_writes | `oltp_insert` | 19.64ms | 39.32ms | 2.002× | 0.62% | PASS |
| mem_writes | `oltp_update_index` | 70.34ms | 130.59ms | 1.857× | 1.18% | PASS |
| mem_writes | `oltp_update_non_index` | 53.16ms | 92.87ms | 1.747× | 1.05% | PASS |
| mem_writes | `oltp_delete_insert` | 51.58ms | 105.91ms | 2.053× | 0.65% | PASS |
| mem_writes | `oltp_write_only` | 28.44ms | 67.63ms | 2.378× | 0.62% | PASS |
| mem_writes | `types_delete_insert` | 33.71ms | 58.52ms | 1.736× | 0.91% | PASS |
| mem_writes | `oltp_read_write` | 100.94ms | 156.28ms | 1.548× | 0.84% | PASS |
| file_reads | `oltp_point_select` | 122.15ms | 67.78ms | 0.555× | 1.33% | PASS |
| file_reads | `oltp_range_select` | 29.27ms | 23.30ms | 0.796× | 1.57% | PASS |
| file_reads | `oltp_sum_range` | 27.51ms | 22.41ms | 0.815× | 1.09% | PASS |
| file_reads | `oltp_order_range` | 4.72ms | 4.15ms | 0.881× | 1.76% | PASS |
| file_reads | `oltp_distinct_range` | 5.81ms | 5.23ms | 0.900× | 1.53% | PASS |
| file_reads | `oltp_index_scan` | 13.64ms | 9.22ms | 0.676× | 1.59% | PASS |
| file_reads | `select_random_points` | 37.28ms | 33.93ms | 0.910× | 1.36% | PASS |
| file_reads | `select_random_ranges` | 16.74ms | 11.47ms | 0.685× | 1.40% | PASS |
| file_reads | `covering_index_scan` | 13.32ms | 7.56ms | 0.568× | 1.94% | PASS |
| file_reads | `groupby_scan` | 39.59ms | 40.71ms | 1.028× | 0.92% | PASS |
| file_reads | `index_join` | 13.02ms | 12.53ms | 0.963× | 1.06% | PASS |
| file_reads | `index_join_scan` | 5.33ms | 6.21ms | 1.165× | 1.67% | PASS |
| file_reads | `types_table_scan` | 1.12s | 1.24s | 1.113× | 0.60% | PASS |
| file_reads | `table_scan` | 1.29s | 1.35s | 1.047× | 0.74% | PASS |
| file_reads | `oltp_read_only` | 279.77ms | 202.72ms | 0.725× | 0.99% | PASS |
| file_writes | `oltp_bulk_insert` | 263.61ms | 378.83ms | 1.437× | 0.98% | PASS |
| file_writes | `oltp_insert` | 26.80ms | 50.67ms | 1.890× | 1.92% | PASS |
| file_writes | `oltp_update_index` | 104.38ms | 165.45ms | 1.585× | 1.32% | PASS |
| file_writes | `oltp_update_non_index` | 81.08ms | 117.82ms | 1.453× | 1.30% | PASS |
| file_writes | `oltp_delete_insert` | 81.07ms | 135.53ms | 1.672× | 1.35% | PASS |
| file_writes | `oltp_write_only` | 51.41ms | 89.24ms | 1.736× | 1.84% | PASS |
| file_writes | `types_delete_insert` | 50.09ms | 72.23ms | 1.442× | 1.59% | PASS |
| file_writes | `oltp_read_write` | 123.82ms | 176.58ms | 1.426× | 1.15% | PASS |
| ac_reads | `oltp_point_select` | 63.09ms | 68.01ms | 1.078× | 0.78% | PASS |
| ac_reads | `oltp_range_select` | 23.15ms | 23.37ms | 1.009× | 1.20% | PASS |
| ac_reads | `oltp_sum_range` | 21.41ms | 22.42ms | 1.047× | 0.85% | PASS |
| ac_reads | `oltp_order_range` | 4.22ms | 4.15ms | 0.982× | 1.46% | PASS |
| ac_reads | `oltp_distinct_range` | 5.32ms | 5.24ms | 0.984× | 0.95% | PASS |
| ac_reads | `oltp_index_scan` | 8.48ms | 9.52ms | 1.122× | 1.42% | PASS |
| ac_reads | `select_random_points` | 34.45ms | 36.46ms | 1.058× | 2.07% | PASS |
| ac_reads | `select_random_ranges` | 11.18ms | 11.68ms | 1.045× | 1.04% | PASS |
| ac_reads | `covering_index_scan` | 8.13ms | 7.87ms | 0.968× | 1.67% | PASS |
| ac_reads | `groupby_scan` | 39.84ms | 41.31ms | 1.037× | 0.81% | PASS |
| ac_reads | `index_join` | 10.77ms | 13.62ms | 1.265× | 2.65% | PASS |
| ac_reads | `index_join_scan` | 5.04ms | 6.65ms | 1.321× | 3.64% | PASS |
| ac_reads | `types_table_scan` | 1.26s | 1.31s | 1.041× | 0.96% | PASS |
| ac_reads | `table_scan` | 1.56s | 1.43s | 0.915× | 0.98% | PASS |
| ac_reads | `oltp_read_only` | 214.00ms | 214.73ms | 1.003× | 0.96% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.05ms | 52.45ms | 3.268× | 5.59% | PASS |
| ac_writes | `oltp_insert_ac` | 18.74ms | 78.28ms | 4.177× | 4.12% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.04ms | 88.37ms | 4.410× | 3.45% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 15.84ms | 64.64ms | 4.081× | 4.37% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.75ms | 78.70ms | 4.435× | 5.20% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.29ms | 78.44ms | 4.067× | 6.42% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.26ms | 65.01ms | 4.261× | 6.01% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.26ms | 85.40ms | 3.381× | 2.76% | PASS |

</details>

## Version-control latency

Wall time: 2m 5s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 66.33ms | 200.00ms | 33.2% | 0.59% | PASS |
| `status_dirty_many_tables` | 68.58ms | 200.00ms | 34.3% | 0.67% | PASS |
| `diff_regular_working_one_table` | 62.63ms | 150.00ms | 41.8% | 0.55% | PASS |
| `diff_regular_working_many_tables` | 73.30ms | 200.00ms | 36.7% | 0.53% | PASS |
| `diff_stat_working_many_tables` | 73.15ms | 200.00ms | 36.6% | 0.74% | PASS |
| `diff_schema_working_many_tables` | 73.42ms | 200.00ms | 36.7% | 0.75% | PASS |
| `branch_list_many_branches` | 19.81ms | 100.00ms | 19.8% | 1.21% | PASS |
| `branch_create_delete` | 26.94ms | 100.00ms | 26.9% | 5.20% | PASS |
| `checkout_branch_clean` | 103.31ms | 200.00ms | 51.7% | 11.26% | PASS |
| `merge_data_no_conflicts` | 33.86ms | 150.00ms | 22.6% | 3.08% | PASS |
| `merge_schema_no_conflicts` | 19.69ms | 100.00ms | 19.7% | 4.83% | PASS |
| `merge_data_conflicts` | 100.73ms | 250.00ms | 40.3% | 0.35% | PASS |
| `merge_data_conflicts_with_resolve` | 100.89ms | 250.00ms | 40.4% | 0.51% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
