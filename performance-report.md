# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-03 16:03 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260927.320.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/37129486340)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 37s | 8.84s | 11.39s | 1.288× | 1.56% | **PASS** |
| textpk | 69 | 55 | 1h 35m 16s | 10.79s | 12.05s | 1.118× | 1.99% | **PASS** |
| blobpk | 69 | 55 | 1h 29m 45s | 9.22s | 11.54s | 1.252× | 1.48% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 55s | 10.09s | 12.29s | 1.218× | 1.55% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.76ms | 30.44ms | 1.138× | 1.03% | PASS |
| mem_reads | `oltp_range_select` | 11.39ms | 13.48ms | 1.183× | 1.23% | PASS |
| mem_reads | `oltp_sum_range` | 10.45ms | 12.71ms | 1.216× | 1.67% | PASS |
| mem_reads | `oltp_order_range` | 2.65ms | 3.02ms | 1.141× | 0.90% | PASS |
| mem_reads | `oltp_distinct_range` | 3.70ms | 4.09ms | 1.105× | 0.84% | PASS |
| mem_reads | `oltp_index_scan` | 4.17ms | 5.71ms | 1.370× | 1.56% | PASS |
| mem_reads | `select_random_points` | 12.27ms | 11.77ms | 0.960× | 1.85% | PASS |
| mem_reads | `select_random_ranges` | 3.19ms | 4.04ms | 1.264× | 0.95% | PASS |
| mem_reads | `covering_index_scan` | 4.29ms | 4.43ms | 1.034× | 1.19% | PASS |
| mem_reads | `groupby_scan` | 30.38ms | 33.47ms | 1.102× | 0.88% | PASS |
| mem_reads | `index_join` | 6.29ms | 8.74ms | 1.391× | 2.51% | PASS |
| mem_reads | `index_join_scan` | 3.75ms | 4.76ms | 1.267× | 2.91% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.33s | 1.289× | 0.44% | PASS |
| mem_reads | `table_scan` | 1.16s | 1.39s | 1.199× | 0.71% | PASS |
| mem_reads | `oltp_read_only` | 100.93ms | 122.69ms | 1.216× | 1.28% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.99ms | 250.73ms | 1.385× | 1.20% | PASS |
| mem_writes | `oltp_insert` | 15.51ms | 28.05ms | 1.809× | 0.97% | PASS |
| mem_writes | `oltp_update_index` | 49.76ms | 103.17ms | 2.073× | 0.92% | PASS |
| mem_writes | `oltp_update_non_index` | 33.89ms | 58.89ms | 1.738× | 1.74% | PASS |
| mem_writes | `oltp_delete_insert` | 44.40ms | 77.81ms | 1.753× | 1.00% | PASS |
| mem_writes | `oltp_write_only` | 21.09ms | 44.07ms | 2.089× | 0.76% | PASS |
| mem_writes | `types_delete_insert` | 24.90ms | 39.78ms | 1.598× | 0.71% | PASS |
| mem_writes | `oltp_read_write` | 65.01ms | 108.33ms | 1.666× | 0.71% | PASS |
| file_reads | `oltp_point_select` | 97.91ms | 54.99ms | 0.562× | 1.26% | PASS |
| file_reads | `oltp_range_select` | 17.89ms | 15.81ms | 0.884× | 2.95% | PASS |
| file_reads | `oltp_sum_range` | 17.67ms | 15.18ms | 0.859× | 2.67% | PASS |
| file_reads | `oltp_order_range` | 3.48ms | 3.32ms | 0.955× | 1.59% | PASS |
| file_reads | `oltp_distinct_range` | 4.57ms | 4.43ms | 0.969× | 1.70% | PASS |
| file_reads | `oltp_index_scan` | 11.91ms | 8.22ms | 0.690× | 1.80% | PASS |
| file_reads | `select_random_points` | 18.55ms | 13.88ms | 0.749× | 3.48% | PASS |
| file_reads | `select_random_ranges` | 10.65ms | 6.57ms | 0.617× | 1.48% | PASS |
| file_reads | `covering_index_scan` | 12.18ms | 7.07ms | 0.580× | 1.96% | PASS |
| file_reads | `groupby_scan` | 30.96ms | 33.84ms | 1.093× | 0.99% | PASS |
| file_reads | `index_join` | 10.54ms | 9.98ms | 0.947× | 2.30% | PASS |
| file_reads | `index_join_scan` | 4.54ms | 5.06ms | 1.115× | 2.97% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.32s | 1.284× | 0.82% | PASS |
| file_reads | `table_scan` | 1.17s | 1.39s | 1.192× | 0.69% | PASS |
| file_reads | `oltp_read_only` | 219.37ms | 163.68ms | 0.746× | 0.61% | PASS |
| file_writes | `oltp_bulk_insert` | 194.65ms | 273.81ms | 1.407× | 1.31% | PASS |
| file_writes | `oltp_insert` | 22.31ms | 35.76ms | 1.603× | 2.37% | PASS |
| file_writes | `oltp_update_index` | 79.84ms | 129.22ms | 1.618× | 1.48% | PASS |
| file_writes | `oltp_update_non_index` | 60.01ms | 82.04ms | 1.367× | 1.90% | PASS |
| file_writes | `oltp_delete_insert` | 70.43ms | 101.06ms | 1.435× | 1.77% | PASS |
| file_writes | `oltp_write_only` | 46.39ms | 64.93ms | 1.400× | 2.22% | PASS |
| file_writes | `types_delete_insert` | 41.72ms | 54.23ms | 1.300× | 1.80% | PASS |
| file_writes | `oltp_read_write` | 94.95ms | 130.17ms | 1.371× | 1.84% | PASS |
| ac_reads | `oltp_point_select` | 49.02ms | 55.14ms | 1.125× | 1.23% | PASS |
| ac_reads | `oltp_range_select` | 12.70ms | 15.75ms | 1.240× | 2.41% | PASS |
| ac_reads | `oltp_sum_range` | 12.14ms | 15.04ms | 1.239× | 1.14% | PASS |
| ac_reads | `oltp_order_range` | 2.85ms | 3.36ms | 1.176× | 2.29% | PASS |
| ac_reads | `oltp_distinct_range` | 3.92ms | 4.44ms | 1.133× | 1.28% | PASS |
| ac_reads | `oltp_index_scan` | 6.42ms | 8.03ms | 1.250× | 1.74% | PASS |
| ac_reads | `select_random_points` | 12.54ms | 13.89ms | 1.107× | 3.72% | PASS |
| ac_reads | `select_random_ranges` | 5.60ms | 6.60ms | 1.178× | 1.49% | PASS |
| ac_reads | `covering_index_scan` | 6.81ms | 6.95ms | 1.021× | 1.77% | PASS |
| ac_reads | `groupby_scan` | 30.32ms | 33.70ms | 1.112× | 0.86% | PASS |
| ac_reads | `index_join` | 7.54ms | 10.01ms | 1.329× | 1.46% | PASS |
| ac_reads | `index_join_scan` | 3.96ms | 4.93ms | 1.244× | 2.66% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.33s | 1.275× | 1.17% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.40s | 1.187× | 0.74% | PASS |
| ac_reads | `oltp_read_only` | 142.10ms | 161.86ms | 1.139× | 1.29% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.22ms | 71.11ms | 2.936× | 8.15% | PASS |
| ac_writes | `oltp_insert_ac` | 27.10ms | 88.19ms | 3.254× | 7.19% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.25ms | 104.32ms | 3.567× | 7.51% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.57ms | 80.84ms | 3.291× | 7.64% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.97ms | 92.47ms | 3.703× | 6.16% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.68ms | 88.66ms | 3.453× | 6.39% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.58ms | 80.40ms | 3.560× | 6.73% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.20ms | 98.05ms | 3.045× | 7.68% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.17ms | 37.91ms | 1.256× | 1.58% | PASS |
| mem_reads | `oltp_range_select` | 13.62ms | 14.20ms | 1.043× | 1.45% | PASS |
| mem_reads | `oltp_sum_range` | 12.24ms | 14.14ms | 1.156× | 2.01% | PASS |
| mem_reads | `oltp_order_range` | 3.03ms | 3.19ms | 1.053× | 1.56% | PASS |
| mem_reads | `oltp_distinct_range` | 4.07ms | 4.26ms | 1.047× | 0.83% | PASS |
| mem_reads | `oltp_index_scan` | 4.74ms | 6.59ms | 1.391× | 1.98% | PASS |
| mem_reads | `select_random_points` | 18.90ms | 21.36ms | 1.130× | 2.94% | PASS |
| mem_reads | `select_random_ranges` | 4.13ms | 5.22ms | 1.262× | 1.56% | PASS |
| mem_reads | `covering_index_scan` | 5.11ms | 4.81ms | 0.940× | 5.42% | PASS |
| mem_reads | `groupby_scan` | 32.76ms | 34.75ms | 1.061× | 0.82% | PASS |
| mem_reads | `index_join` | 7.55ms | 9.91ms | 1.312× | 2.95% | PASS |
| mem_reads | `index_join_scan` | 4.59ms | 5.41ms | 1.180× | 2.59% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.24s | 1.153× | 1.93% | PASS |
| mem_reads | `table_scan` | 1.58s | 1.44s | 0.909× | 0.71% | PASS |
| mem_reads | `oltp_read_only` | 136.19ms | 142.25ms | 1.044× | 3.29% | PASS |
| mem_writes | `oltp_bulk_insert` | 239.12ms | 368.07ms | 1.539× | 1.18% | PASS |
| mem_writes | `oltp_insert` | 25.50ms | 41.79ms | 1.639× | 4.61% | PASS |
| mem_writes | `oltp_update_index` | 89.11ms | 149.49ms | 1.678× | 5.65% | PASS |
| mem_writes | `oltp_update_non_index` | 50.88ms | 89.22ms | 1.753× | 1.74% | PASS |
| mem_writes | `oltp_delete_insert` | 54.58ms | 107.06ms | 1.961× | 2.65% | PASS |
| mem_writes | `oltp_write_only` | 30.88ms | 63.53ms | 2.057× | 3.17% | PASS |
| mem_writes | `types_delete_insert` | 34.33ms | 57.00ms | 1.660× | 1.68% | PASS |
| mem_writes | `oltp_read_write` | 94.03ms | 143.26ms | 1.523× | 2.49% | PASS |
| file_reads | `oltp_point_select` | 108.53ms | 64.78ms | 0.597× | 1.30% | PASS |
| file_reads | `oltp_range_select` | 23.53ms | 17.19ms | 0.731× | 1.90% | PASS |
| file_reads | `oltp_sum_range` | 22.31ms | 17.64ms | 0.790× | 1.90% | PASS |
| file_reads | `oltp_order_range` | 4.12ms | 3.59ms | 0.871× | 1.84% | PASS |
| file_reads | `oltp_distinct_range` | 5.16ms | 4.65ms | 0.900× | 1.07% | PASS |
| file_reads | `oltp_index_scan` | 12.79ms | 9.28ms | 0.726× | 1.77% | PASS |
| file_reads | `select_random_points` | 29.26ms | 25.53ms | 0.872× | 2.63% | PASS |
| file_reads | `select_random_ranges` | 11.88ms | 7.92ms | 0.667× | 1.19% | PASS |
| file_reads | `covering_index_scan` | 13.62ms | 7.44ms | 0.546× | 2.65% | PASS |
| file_reads | `groupby_scan` | 33.81ms | 35.12ms | 1.039× | 1.15% | PASS |
| file_reads | `index_join` | 12.31ms | 11.44ms | 0.929× | 3.28% | PASS |
| file_reads | `index_join_scan` | 5.84ms | 5.93ms | 1.017× | 3.01% | PASS |
| file_reads | `types_table_scan` | 1.09s | 1.24s | 1.143× | 2.63% | PASS |
| file_reads | `table_scan` | 1.55s | 1.43s | 0.922× | 1.03% | PASS |
| file_reads | `oltp_read_only` | 248.50ms | 181.10ms | 0.729× | 1.12% | PASS |
| file_writes | `oltp_bulk_insert` | 257.60ms | 389.00ms | 1.510× | 1.00% | PASS |
| file_writes | `oltp_insert` | 44.78ms | 53.02ms | 1.184× | 10.81% | PASS |
| file_writes | `oltp_update_index` | 116.02ms | 170.63ms | 1.471× | 1.56% | PASS |
| file_writes | `oltp_update_non_index` | 93.45ms | 114.53ms | 1.226× | 7.15% | PASS |
| file_writes | `oltp_delete_insert` | 94.86ms | 136.12ms | 1.435× | 1.99% | PASS |
| file_writes | `oltp_write_only` | 84.76ms | 86.21ms | 1.017× | 11.71% | PASS |
| file_writes | `types_delete_insert` | 57.34ms | 75.41ms | 1.315× | 1.70% | PASS |
| file_writes | `oltp_read_write` | 149.86ms | 169.72ms | 1.133× | 9.74% | PASS |
| ac_reads | `oltp_point_select` | 56.57ms | 64.12ms | 1.133× | 1.19% | PASS |
| ac_reads | `oltp_range_select` | 17.33ms | 17.11ms | 0.988× | 1.99% | PASS |
| ac_reads | `oltp_sum_range` | 16.18ms | 17.09ms | 1.056× | 1.54% | PASS |
| ac_reads | `oltp_order_range` | 3.55ms | 3.61ms | 1.016× | 2.17% | PASS |
| ac_reads | `oltp_distinct_range` | 4.62ms | 4.70ms | 1.018× | 1.45% | PASS |
| ac_reads | `oltp_index_scan` | 7.77ms | 9.32ms | 1.199× | 1.63% | PASS |
| ac_reads | `select_random_points` | 22.90ms | 25.52ms | 1.115× | 2.12% | PASS |
| ac_reads | `select_random_ranges` | 6.98ms | 7.93ms | 1.135× | 1.46% | PASS |
| ac_reads | `covering_index_scan` | 8.54ms | 7.45ms | 0.873× | 3.68% | PASS |
| ac_reads | `groupby_scan` | 32.68ms | 34.97ms | 1.070× | 0.82% | PASS |
| ac_reads | `index_join` | 9.90ms | 11.51ms | 1.163× | 4.48% | PASS |
| ac_reads | `index_join_scan` | 5.54ms | 6.22ms | 1.123× | 4.24% | PASS |
| ac_reads | `types_table_scan` | 1.26s | 1.28s | 1.020× | 1.35% | PASS |
| ac_reads | `table_scan` | 1.33s | 1.39s | 1.046× | 5.16% | PASS |
| ac_reads | `oltp_read_only` | 167.66ms | 179.63ms | 1.071× | 1.75% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.49ms | 73.66ms | 3.008× | 8.51% | PASS |
| ac_writes | `oltp_insert_ac` | 27.39ms | 88.05ms | 3.215× | 8.13% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.79ms | 108.63ms | 3.647× | 6.34% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.73ms | 84.05ms | 3.399× | 6.79% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.46ms | 97.12ms | 3.670× | 6.87% | PASS |
| ac_writes | `oltp_write_only_ac` | 28.59ms | 96.83ms | 3.386× | 7.25% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.14ms | 83.62ms | 3.464× | 7.78% | PASS |
| ac_writes | `oltp_read_write_ac` | 34.14ms | 103.50ms | 3.032× | 5.00% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.52ms | 36.63ms | 1.241× | 1.32% | PASS |
| mem_reads | `oltp_range_select` | 12.17ms | 13.93ms | 1.145× | 2.09% | PASS |
| mem_reads | `oltp_sum_range` | 11.40ms | 13.84ms | 1.214× | 1.42% | PASS |
| mem_reads | `oltp_order_range` | 2.81ms | 3.12ms | 1.112× | 1.31% | PASS |
| mem_reads | `oltp_distinct_range` | 3.89ms | 4.21ms | 1.081× | 1.10% | PASS |
| mem_reads | `oltp_index_scan` | 4.36ms | 6.08ms | 1.396× | 1.47% | PASS |
| mem_reads | `select_random_points` | 17.38ms | 20.44ms | 1.176× | 1.66% | PASS |
| mem_reads | `select_random_ranges` | 3.85ms | 5.09ms | 1.320× | 1.54% | PASS |
| mem_reads | `covering_index_scan` | 4.39ms | 4.38ms | 0.999× | 1.49% | PASS |
| mem_reads | `groupby_scan` | 31.23ms | 34.16ms | 1.094× | 0.66% | PASS |
| mem_reads | `index_join` | 6.72ms | 8.86ms | 1.319× | 1.28% | PASS |
| mem_reads | `index_join_scan` | 3.99ms | 5.22ms | 1.309× | 3.09% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.22s | 1.181× | 0.63% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.36s | 1.164× | 0.53% | PASS |
| mem_reads | `oltp_read_only` | 115.83ms | 134.86ms | 1.164× | 1.19% | PASS |
| mem_writes | `oltp_bulk_insert` | 243.14ms | 353.10ms | 1.452× | 1.00% | PASS |
| mem_writes | `oltp_insert` | 19.92ms | 39.00ms | 1.958× | 0.60% | PASS |
| mem_writes | `oltp_update_index` | 67.22ms | 128.08ms | 1.905× | 0.86% | PASS |
| mem_writes | `oltp_update_non_index` | 48.13ms | 82.82ms | 1.721× | 1.15% | PASS |
| mem_writes | `oltp_delete_insert` | 47.92ms | 100.83ms | 2.104× | 0.92% | PASS |
| mem_writes | `oltp_write_only` | 27.15ms | 60.26ms | 2.219× | 1.17% | PASS |
| mem_writes | `types_delete_insert` | 32.07ms | 52.96ms | 1.652× | 1.70% | PASS |
| mem_writes | `oltp_read_write` | 82.16ms | 137.57ms | 1.674× | 1.24% | PASS |
| file_reads | `oltp_point_select` | 105.32ms | 63.26ms | 0.601× | 1.00% | PASS |
| file_reads | `oltp_range_select` | 20.48ms | 16.84ms | 0.822× | 1.93% | PASS |
| file_reads | `oltp_sum_range` | 20.03ms | 16.95ms | 0.847× | 2.93% | PASS |
| file_reads | `oltp_order_range` | 3.85ms | 3.48ms | 0.905× | 1.92% | PASS |
| file_reads | `oltp_distinct_range` | 4.87ms | 4.57ms | 0.938× | 1.85% | PASS |
| file_reads | `oltp_index_scan` | 12.57ms | 9.13ms | 0.726× | 2.45% | PASS |
| file_reads | `select_random_points` | 27.00ms | 24.45ms | 0.905× | 2.61% | PASS |
| file_reads | `select_random_ranges` | 11.59ms | 7.83ms | 0.676× | 1.14% | PASS |
| file_reads | `covering_index_scan` | 12.74ms | 7.32ms | 0.574× | 2.39% | PASS |
| file_reads | `groupby_scan` | 32.62ms | 34.72ms | 1.064× | 0.97% | PASS |
| file_reads | `index_join` | 11.08ms | 11.29ms | 1.019× | 2.12% | PASS |
| file_reads | `index_join_scan` | 5.29ms | 5.83ms | 1.101× | 2.62% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.21s | 1.177× | 0.64% | PASS |
| file_reads | `table_scan` | 1.17s | 1.36s | 1.157× | 0.56% | PASS |
| file_reads | `oltp_read_only` | 227.24ms | 174.25ms | 0.767× | 1.00% | PASS |
| file_writes | `oltp_bulk_insert` | 262.19ms | 377.96ms | 1.442× | 0.86% | PASS |
| file_writes | `oltp_insert` | 31.44ms | 51.42ms | 1.635× | 1.52% | PASS |
| file_writes | `oltp_update_index` | 103.48ms | 161.45ms | 1.560× | 1.75% | PASS |
| file_writes | `oltp_update_non_index` | 79.27ms | 107.05ms | 1.350× | 1.56% | PASS |
| file_writes | `oltp_delete_insert` | 80.76ms | 129.29ms | 1.601× | 1.84% | PASS |
| file_writes | `oltp_write_only` | 55.65ms | 83.38ms | 1.498× | 2.26% | PASS |
| file_writes | `types_delete_insert` | 52.63ms | 71.49ms | 1.358× | 1.29% | PASS |
| file_writes | `oltp_read_write` | 112.91ms | 160.77ms | 1.424× | 1.69% | PASS |
| ac_reads | `oltp_point_select` | 54.88ms | 63.17ms | 1.151× | 0.90% | PASS |
| ac_reads | `oltp_range_select` | 15.47ms | 16.87ms | 1.091× | 1.45% | PASS |
| ac_reads | `oltp_sum_range` | 14.63ms | 16.78ms | 1.147× | 1.19% | PASS |
| ac_reads | `oltp_order_range` | 3.21ms | 3.47ms | 1.081× | 1.47% | PASS |
| ac_reads | `oltp_distinct_range` | 4.30ms | 4.57ms | 1.062× | 1.48% | PASS |
| ac_reads | `oltp_index_scan` | 7.16ms | 9.15ms | 1.277× | 2.08% | PASS |
| ac_reads | `select_random_points` | 21.23ms | 24.11ms | 1.136× | 1.82% | PASS |
| ac_reads | `select_random_ranges` | 6.59ms | 7.83ms | 1.190× | 1.44% | PASS |
| ac_reads | `covering_index_scan` | 7.13ms | 7.36ms | 1.031× | 2.22% | PASS |
| ac_reads | `groupby_scan` | 31.76ms | 34.63ms | 1.090× | 0.81% | PASS |
| ac_reads | `index_join` | 8.48ms | 11.19ms | 1.319× | 1.66% | PASS |
| ac_reads | `index_join_scan` | 4.78ms | 5.87ms | 1.228× | 2.18% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.21s | 1.180× | 0.49% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.35s | 1.157× | 0.47% | PASS |
| ac_reads | `oltp_read_only` | 154.81ms | 174.29ms | 1.126× | 0.93% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.73ms | 64.48ms | 2.968× | 3.96% | PASS |
| ac_writes | `oltp_insert_ac` | 23.92ms | 86.63ms | 3.621× | 3.68% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.73ms | 98.91ms | 3.700× | 5.09% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.09ms | 77.84ms | 3.523× | 3.37% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.26ms | 88.01ms | 3.783× | 3.08% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.42ms | 88.40ms | 3.620× | 5.59% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.74ms | 80.93ms | 3.559× | 7.11% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.95ms | 95.79ms | 3.095× | 3.71% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.59ms | 40.55ms | 1.207× | 1.71% | PASS |
| mem_reads | `oltp_range_select` | 20.21ms | 21.24ms | 1.051× | 1.87% | PASS |
| mem_reads | `oltp_sum_range` | 18.51ms | 21.15ms | 1.143× | 1.30% | PASS |
| mem_reads | `oltp_order_range` | 3.65ms | 3.86ms | 1.060× | 1.14% | PASS |
| mem_reads | `oltp_distinct_range` | 4.78ms | 4.93ms | 1.030× | 1.18% | PASS |
| mem_reads | `oltp_index_scan` | 4.74ms | 6.33ms | 1.336× | 1.94% | PASS |
| mem_reads | `select_random_points` | 28.72ms | 32.41ms | 1.128× | 1.45% | PASS |
| mem_reads | `select_random_ranges` | 7.83ms | 9.11ms | 1.164× | 1.31% | PASS |
| mem_reads | `covering_index_scan` | 4.22ms | 4.39ms | 1.040× | 2.51% | PASS |
| mem_reads | `groupby_scan` | 36.74ms | 39.26ms | 1.068× | 0.94% | PASS |
| mem_reads | `index_join` | 8.23ms | 10.60ms | 1.288× | 2.03% | PASS |
| mem_reads | `index_join_scan` | 4.33ms | 5.59ms | 1.292× | 2.29% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.25s | 1.166× | 1.14% | PASS |
| mem_reads | `table_scan` | 1.27s | 1.41s | 1.106× | 2.03% | PASS |
| mem_reads | `oltp_read_only` | 157.79ms | 171.12ms | 1.084× | 1.18% | PASS |
| mem_writes | `oltp_bulk_insert` | 245.77ms | 356.59ms | 1.451× | 0.92% | PASS |
| mem_writes | `oltp_insert` | 19.18ms | 36.55ms | 1.905× | 0.88% | PASS |
| mem_writes | `oltp_update_index` | 70.34ms | 119.19ms | 1.694× | 2.24% | PASS |
| mem_writes | `oltp_update_non_index` | 53.01ms | 83.96ms | 1.584× | 1.47% | PASS |
| mem_writes | `oltp_delete_insert` | 50.84ms | 97.44ms | 1.916× | 1.32% | PASS |
| mem_writes | `oltp_write_only` | 27.78ms | 58.57ms | 2.108× | 1.47% | PASS |
| mem_writes | `types_delete_insert` | 33.49ms | 55.44ms | 1.656× | 1.86% | PASS |
| mem_writes | `oltp_read_write` | 106.68ms | 156.80ms | 1.470× | 1.84% | PASS |
| file_reads | `oltp_point_select` | 109.04ms | 67.20ms | 0.616× | 0.98% | PASS |
| file_reads | `oltp_range_select` | 27.62ms | 24.35ms | 0.881× | 1.59% | PASS |
| file_reads | `oltp_sum_range` | 26.66ms | 24.30ms | 0.912× | 1.68% | PASS |
| file_reads | `oltp_order_range` | 4.53ms | 4.25ms | 0.938× | 1.96% | PASS |
| file_reads | `oltp_distinct_range` | 5.69ms | 5.38ms | 0.946× | 1.33% | PASS |
| file_reads | `oltp_index_scan` | 12.43ms | 9.31ms | 0.749× | 1.75% | PASS |
| file_reads | `select_random_points` | 38.17ms | 36.43ms | 0.954× | 1.60% | PASS |
| file_reads | `select_random_ranges` | 15.84ms | 12.24ms | 0.773× | 0.98% | PASS |
| file_reads | `covering_index_scan` | 11.88ms | 7.31ms | 0.615× | 1.18% | PASS |
| file_reads | `groupby_scan` | 37.67ms | 40.09ms | 1.064× | 0.89% | PASS |
| file_reads | `index_join` | 12.61ms | 13.00ms | 1.031× | 1.55% | PASS |
| file_reads | `index_join_scan` | 5.30ms | 6.14ms | 1.159× | 2.66% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.24s | 1.171× | 1.10% | PASS |
| file_reads | `table_scan` | 1.31s | 1.41s | 1.079× | 2.10% | PASS |
| file_reads | `oltp_read_only` | 268.25ms | 211.62ms | 0.789× | 0.84% | PASS |
| file_writes | `oltp_bulk_insert` | 260.65ms | 380.26ms | 1.459× | 0.95% | PASS |
| file_writes | `oltp_insert` | 29.35ms | 46.84ms | 1.596× | 2.10% | PASS |
| file_writes | `oltp_update_index` | 101.78ms | 145.31ms | 1.428× | 1.53% | PASS |
| file_writes | `oltp_update_non_index` | 82.47ms | 105.20ms | 1.276× | 1.28% | PASS |
| file_writes | `oltp_delete_insert` | 82.72ms | 120.20ms | 1.453× | 1.41% | PASS |
| file_writes | `oltp_write_only` | 58.43ms | 78.36ms | 1.341× | 1.77% | PASS |
| file_writes | `types_delete_insert` | 53.07ms | 69.45ms | 1.309× | 1.85% | PASS |
| file_writes | `oltp_read_write` | 140.11ms | 177.92ms | 1.270× | 1.80% | PASS |
| ac_reads | `oltp_point_select` | 58.63ms | 67.53ms | 1.152× | 1.77% | PASS |
| ac_reads | `oltp_range_select` | 22.78ms | 24.40ms | 1.071× | 1.49% | PASS |
| ac_reads | `oltp_sum_range` | 21.17ms | 24.35ms | 1.151× | 1.47% | PASS |
| ac_reads | `oltp_order_range` | 4.03ms | 4.25ms | 1.056× | 1.41% | PASS |
| ac_reads | `oltp_distinct_range` | 5.16ms | 5.36ms | 1.038× | 1.21% | PASS |
| ac_reads | `oltp_index_scan` | 7.58ms | 9.32ms | 1.229× | 1.36% | PASS |
| ac_reads | `select_random_points` | 33.19ms | 36.47ms | 1.099× | 2.26% | PASS |
| ac_reads | `select_random_ranges` | 10.65ms | 12.23ms | 1.148× | 1.44% | PASS |
| ac_reads | `covering_index_scan` | 7.15ms | 7.30ms | 1.022× | 1.09% | PASS |
| ac_reads | `groupby_scan` | 36.94ms | 39.97ms | 1.082× | 0.85% | PASS |
| ac_reads | `index_join` | 10.08ms | 12.97ms | 1.287× | 2.20% | PASS |
| ac_reads | `index_join_scan` | 4.81ms | 6.11ms | 1.271× | 1.66% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.24s | 1.177× | 0.93% | PASS |
| ac_reads | `table_scan` | 1.26s | 1.40s | 1.112× | 1.57% | PASS |
| ac_reads | `oltp_read_only` | 194.68ms | 213.10ms | 1.095× | 0.86% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 37.00ms | 97.89ms | 2.646× | 5.51% | PASS |
| ac_writes | `oltp_insert_ac` | 39.32ms | 118.57ms | 3.016× | 5.91% | PASS |
| ac_writes | `oltp_update_index_ac` | 42.14ms | 131.04ms | 3.109× | 9.57% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 39.34ms | 106.64ms | 2.711× | 9.67% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 39.25ms | 122.71ms | 3.126× | 7.35% | PASS |
| ac_writes | `oltp_write_only_ac` | 45.05ms | 130.30ms | 2.893× | 8.61% | PASS |
| ac_writes | `types_delete_insert_ac` | 39.47ms | 120.30ms | 3.048× | 8.08% | PASS |
| ac_writes | `oltp_read_write_ac` | 45.91ms | 128.75ms | 2.805× | 9.36% | PASS |

</details>

## Version-control latency

Wall time: 2m 19s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 82.88ms | 200.00ms | 41.4% | 0.41% | PASS |
| `status_dirty_many_tables` | 85.98ms | 200.00ms | 43.0% | 0.38% | PASS |
| `diff_regular_working_one_table` | 78.64ms | 150.00ms | 52.4% | 0.42% | PASS |
| `diff_regular_working_many_tables` | 91.64ms | 200.00ms | 45.8% | 0.36% | PASS |
| `diff_stat_working_many_tables` | 91.58ms | 200.00ms | 45.8% | 0.44% | PASS |
| `diff_schema_working_many_tables` | 91.95ms | 200.00ms | 46.0% | 0.36% | PASS |
| `branch_list_many_branches` | 22.71ms | 100.00ms | 22.7% | 1.15% | PASS |
| `branch_create_delete` | 24.45ms | 100.00ms | 24.5% | 1.15% | PASS |
| `checkout_branch_clean` | 54.94ms | 200.00ms | 27.5% | 0.95% | PASS |
| `merge_data_no_conflicts` | 28.40ms | 150.00ms | 18.9% | 1.11% | PASS |
| `merge_schema_no_conflicts` | 21.96ms | 100.00ms | 22.0% | 1.31% | PASS |
| `merge_data_conflicts` | 127.04ms | 250.00ms | 50.8% | 0.25% | PASS |
| `merge_data_conflicts_with_resolve` | 126.97ms | 250.00ms | 50.8% | 0.34% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
