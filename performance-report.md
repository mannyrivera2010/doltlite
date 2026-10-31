# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-31 17:08 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261026.354.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/41214990196)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 33s | 8.72s | 11.35s | 1.302× | 1.37% | **PASS** |
| textpk | 69 | 55 | 1h 35m 28s | 10.00s | 11.95s | 1.195× | 1.98% | **PASS** |
| blobpk | 69 | 55 | 1h 37m 29s | 10.70s | 12.30s | 1.150× | 1.69% | **PASS** |
| compositepk | 69 | 55 | 1h 29m 46s | 10.00s | 12.15s | 1.214× | 1.55% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.46ms | 29.43ms | 1.255× | 1.19% | PASS |
| mem_reads | `oltp_range_select` | 9.85ms | 13.02ms | 1.322× | 1.77% | PASS |
| mem_reads | `oltp_sum_range` | 9.42ms | 12.27ms | 1.303× | 1.65% | PASS |
| mem_reads | `oltp_order_range` | 2.48ms | 2.97ms | 1.200× | 1.48% | PASS |
| mem_reads | `oltp_distinct_range` | 3.56ms | 4.04ms | 1.133× | 1.20% | PASS |
| mem_reads | `oltp_index_scan` | 3.84ms | 5.23ms | 1.362× | 1.91% | PASS |
| mem_reads | `select_random_points` | 9.58ms | 10.91ms | 1.139× | 2.52% | PASS |
| mem_reads | `select_random_ranges` | 2.88ms | 3.96ms | 1.375× | 1.99% | PASS |
| mem_reads | `covering_index_scan` | 4.24ms | 4.11ms | 0.970× | 1.49% | PASS |
| mem_reads | `groupby_scan` | 29.59ms | 33.17ms | 1.121× | 0.74% | PASS |
| mem_reads | `index_join` | 5.99ms | 8.06ms | 1.346× | 1.06% | PASS |
| mem_reads | `index_join_scan` | 3.41ms | 4.53ms | 1.328× | 1.78% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.33s | 1.287× | 0.50% | PASS |
| mem_reads | `table_scan` | 1.16s | 1.39s | 1.196× | 0.55% | PASS |
| mem_reads | `oltp_read_only` | 101.00ms | 123.59ms | 1.224× | 0.75% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.83ms | 261.50ms | 1.438× | 1.18% | PASS |
| mem_writes | `oltp_insert` | 15.50ms | 29.33ms | 1.893× | 1.12% | PASS |
| mem_writes | `oltp_update_index` | 49.90ms | 110.04ms | 2.205× | 0.87% | PASS |
| mem_writes | `oltp_update_non_index` | 33.87ms | 63.68ms | 1.880× | 1.05% | PASS |
| mem_writes | `oltp_delete_insert` | 44.75ms | 83.54ms | 1.867× | 0.99% | PASS |
| mem_writes | `oltp_write_only` | 21.75ms | 48.67ms | 2.238× | 0.94% | PASS |
| mem_writes | `types_delete_insert` | 24.54ms | 43.97ms | 1.792× | 1.72% | PASS |
| mem_writes | `oltp_read_write` | 65.55ms | 114.25ms | 1.743× | 1.01% | PASS |
| file_reads | `oltp_point_select` | 92.99ms | 52.92ms | 0.569× | 0.98% | PASS |
| file_reads | `oltp_range_select` | 17.28ms | 15.52ms | 0.898× | 1.84% | PASS |
| file_reads | `oltp_sum_range` | 16.87ms | 14.89ms | 0.882× | 1.75% | PASS |
| file_reads | `oltp_order_range` | 3.51ms | 3.38ms | 0.963× | 2.57% | PASS |
| file_reads | `oltp_distinct_range` | 4.57ms | 4.46ms | 0.977× | 1.11% | PASS |
| file_reads | `oltp_index_scan` | 11.12ms | 8.03ms | 0.722× | 1.21% | PASS |
| file_reads | `select_random_points` | 17.35ms | 13.62ms | 0.785× | 2.80% | PASS |
| file_reads | `select_random_ranges` | 10.10ms | 6.41ms | 0.635× | 1.33% | PASS |
| file_reads | `covering_index_scan` | 11.56ms | 6.83ms | 0.591× | 1.48% | PASS |
| file_reads | `groupby_scan` | 30.66ms | 33.78ms | 1.102× | 1.09% | PASS |
| file_reads | `index_join` | 10.13ms | 10.00ms | 0.987× | 1.14% | PASS |
| file_reads | `index_join_scan` | 4.43ms | 5.00ms | 1.130× | 2.24% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.32s | 1.285× | 0.45% | PASS |
| file_reads | `table_scan` | 1.16s | 1.39s | 1.200× | 0.50% | PASS |
| file_reads | `oltp_read_only` | 203.37ms | 158.70ms | 0.780× | 0.87% | PASS |
| file_writes | `oltp_bulk_insert` | 195.28ms | 281.95ms | 1.444× | 0.88% | PASS |
| file_writes | `oltp_insert` | 22.28ms | 35.81ms | 1.607× | 1.20% | PASS |
| file_writes | `oltp_update_index` | 77.59ms | 136.65ms | 1.761× | 1.49% | PASS |
| file_writes | `oltp_update_non_index` | 58.32ms | 83.40ms | 1.430× | 1.57% | PASS |
| file_writes | `oltp_delete_insert` | 68.90ms | 108.09ms | 1.569× | 1.32% | PASS |
| file_writes | `oltp_write_only` | 44.87ms | 66.65ms | 1.485× | 2.12% | PASS |
| file_writes | `types_delete_insert` | 40.51ms | 53.53ms | 1.322× | 2.29% | PASS |
| file_writes | `oltp_read_write` | 93.97ms | 133.49ms | 1.421× | 1.64% | PASS |
| ac_reads | `oltp_point_select` | 47.05ms | 53.11ms | 1.129× | 1.37% | PASS |
| ac_reads | `oltp_range_select` | 12.69ms | 15.64ms | 1.232× | 1.21% | PASS |
| ac_reads | `oltp_sum_range` | 12.03ms | 14.96ms | 1.244× | 1.33% | PASS |
| ac_reads | `oltp_order_range` | 3.06ms | 3.44ms | 1.124× | 3.24% | PASS |
| ac_reads | `oltp_distinct_range` | 4.04ms | 4.43ms | 1.097× | 2.22% | PASS |
| ac_reads | `oltp_index_scan` | 6.41ms | 8.00ms | 1.248× | 1.60% | PASS |
| ac_reads | `select_random_points` | 12.58ms | 13.61ms | 1.082× | 1.71% | PASS |
| ac_reads | `select_random_ranges` | 5.35ms | 6.42ms | 1.200× | 1.22% | PASS |
| ac_reads | `covering_index_scan` | 6.72ms | 6.84ms | 1.018× | 1.19% | PASS |
| ac_reads | `groupby_scan` | 29.98ms | 33.68ms | 1.123× | 0.93% | PASS |
| ac_reads | `index_join` | 7.47ms | 10.02ms | 1.342× | 1.43% | PASS |
| ac_reads | `index_join_scan` | 3.86ms | 4.99ms | 1.293× | 1.66% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.32s | 1.282× | 0.46% | PASS |
| ac_reads | `table_scan` | 1.16s | 1.39s | 1.194× | 0.58% | PASS |
| ac_reads | `oltp_read_only` | 135.86ms | 158.71ms | 1.168× | 0.99% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 18.88ms | 61.99ms | 3.284× | 5.79% | PASS |
| ac_writes | `oltp_insert_ac` | 21.68ms | 76.70ms | 3.538× | 3.70% | PASS |
| ac_writes | `oltp_update_index_ac` | 23.49ms | 92.21ms | 3.926× | 4.75% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 19.69ms | 70.72ms | 3.591× | 5.27% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 21.64ms | 84.72ms | 3.915× | 4.77% | PASS |
| ac_writes | `oltp_write_only_ac` | 21.65ms | 82.67ms | 3.818× | 6.25% | PASS |
| ac_writes | `types_delete_insert_ac` | 19.49ms | 73.59ms | 3.776× | 7.17% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.40ms | 87.99ms | 3.333× | 4.90% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.17ms | 38.14ms | 1.264× | 2.13% | PASS |
| mem_reads | `oltp_range_select` | 13.15ms | 14.24ms | 1.082× | 1.85% | PASS |
| mem_reads | `oltp_sum_range` | 11.99ms | 14.38ms | 1.199× | 2.37% | PASS |
| mem_reads | `oltp_order_range` | 2.94ms | 3.17ms | 1.076× | 1.98% | PASS |
| mem_reads | `oltp_distinct_range` | 4.01ms | 4.28ms | 1.065× | 1.52% | PASS |
| mem_reads | `oltp_index_scan` | 4.48ms | 6.33ms | 1.413× | 1.98% | PASS |
| mem_reads | `select_random_points` | 17.40ms | 21.16ms | 1.216× | 1.70% | PASS |
| mem_reads | `select_random_ranges` | 4.01ms | 5.20ms | 1.297× | 2.20% | PASS |
| mem_reads | `covering_index_scan` | 4.55ms | 4.65ms | 1.021× | 2.08% | PASS |
| mem_reads | `groupby_scan` | 32.46ms | 34.47ms | 1.062× | 0.96% | PASS |
| mem_reads | `index_join` | 7.13ms | 9.72ms | 1.364× | 2.51% | PASS |
| mem_reads | `index_join_scan` | 4.70ms | 5.51ms | 1.172× | 1.46% | PASS |
| mem_reads | `types_table_scan` | 1.11s | 1.25s | 1.124× | 4.03% | PASS |
| mem_reads | `table_scan` | 1.27s | 1.39s | 1.096× | 3.43% | PASS |
| mem_reads | `oltp_read_only` | 121.98ms | 140.15ms | 1.149× | 2.18% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.17ms | 369.25ms | 1.563× | 0.69% | PASS |
| mem_writes | `oltp_insert` | 21.85ms | 42.08ms | 1.926× | 0.97% | PASS |
| mem_writes | `oltp_update_index` | 71.41ms | 143.95ms | 2.016× | 1.00% | PASS |
| mem_writes | `oltp_update_non_index` | 48.77ms | 93.73ms | 1.922× | 0.96% | PASS |
| mem_writes | `oltp_delete_insert` | 51.49ms | 111.84ms | 2.172× | 1.44% | PASS |
| mem_writes | `oltp_write_only` | 32.02ms | 70.80ms | 2.211× | 2.78% | PASS |
| mem_writes | `types_delete_insert` | 33.74ms | 61.14ms | 1.812× | 1.12% | PASS |
| mem_writes | `oltp_read_write` | 86.52ms | 147.09ms | 1.700× | 1.29% | PASS |
| file_reads | `oltp_point_select` | 100.17ms | 61.87ms | 0.618× | 1.18% | PASS |
| file_reads | `oltp_range_select` | 20.93ms | 16.86ms | 0.806× | 3.19% | PASS |
| file_reads | `oltp_sum_range` | 20.10ms | 17.21ms | 0.856× | 2.17% | PASS |
| file_reads | `oltp_order_range` | 4.01ms | 3.51ms | 0.876× | 2.72% | PASS |
| file_reads | `oltp_distinct_range` | 4.93ms | 4.63ms | 0.940× | 2.06% | PASS |
| file_reads | `oltp_index_scan` | 12.05ms | 9.08ms | 0.754× | 2.42% | PASS |
| file_reads | `select_random_points` | 26.26ms | 25.10ms | 0.956× | 1.91% | PASS |
| file_reads | `select_random_ranges` | 11.05ms | 7.73ms | 0.699× | 1.50% | PASS |
| file_reads | `covering_index_scan` | 11.95ms | 7.26ms | 0.607× | 1.88% | PASS |
| file_reads | `groupby_scan` | 33.42ms | 35.13ms | 1.051× | 1.16% | PASS |
| file_reads | `index_join` | 12.01ms | 11.56ms | 0.962× | 2.14% | PASS |
| file_reads | `index_join_scan` | 5.90ms | 6.02ms | 1.019× | 3.54% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.24s | 1.178× | 1.66% | PASS |
| file_reads | `table_scan` | 1.53s | 1.43s | 0.929× | 1.14% | PASS |
| file_reads | `oltp_read_only` | 231.55ms | 177.31ms | 0.766× | 1.15% | PASS |
| file_writes | `oltp_bulk_insert` | 257.63ms | 403.84ms | 1.567× | 1.04% | PASS |
| file_writes | `oltp_insert` | 58.43ms | 56.70ms | 0.970× | 25.03% | PASS |
| file_writes | `oltp_update_index` | 114.24ms | 181.22ms | 1.586× | 1.77% | PASS |
| file_writes | `oltp_update_non_index` | 93.01ms | 121.73ms | 1.309× | 8.31% | PASS |
| file_writes | `oltp_delete_insert` | 91.78ms | 144.20ms | 1.571× | 1.85% | PASS |
| file_writes | `oltp_write_only` | 86.05ms | 94.56ms | 1.099× | 9.41% | PASS |
| file_writes | `types_delete_insert` | 57.38ms | 80.00ms | 1.394× | 1.19% | PASS |
| file_writes | `oltp_read_write` | 139.70ms | 173.98ms | 1.245× | 9.63% | PASS |
| ac_reads | `oltp_point_select` | 55.45ms | 62.94ms | 1.135× | 1.31% | PASS |
| ac_reads | `oltp_range_select` | 17.94ms | 17.11ms | 0.953× | 3.09% | PASS |
| ac_reads | `oltp_sum_range` | 15.85ms | 17.34ms | 1.094× | 2.26% | PASS |
| ac_reads | `oltp_order_range` | 3.47ms | 3.52ms | 1.014× | 2.12% | PASS |
| ac_reads | `oltp_distinct_range` | 4.53ms | 4.63ms | 1.023× | 1.36% | PASS |
| ac_reads | `oltp_index_scan` | 7.42ms | 9.05ms | 1.220× | 1.98% | PASS |
| ac_reads | `select_random_points` | 21.98ms | 25.09ms | 1.142× | 1.78% | PASS |
| ac_reads | `select_random_ranges` | 6.87ms | 7.76ms | 1.131× | 1.02% | PASS |
| ac_reads | `covering_index_scan` | 8.45ms | 7.31ms | 0.865× | 2.03% | PASS |
| ac_reads | `groupby_scan` | 33.35ms | 35.06ms | 1.051× | 1.14% | PASS |
| ac_reads | `index_join` | 9.85ms | 11.89ms | 1.206× | 2.14% | PASS |
| ac_reads | `index_join_scan` | 5.38ms | 6.13ms | 1.140× | 2.59% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.23s | 1.170× | 0.53% | PASS |
| ac_reads | `table_scan` | 1.21s | 1.37s | 1.133× | 0.88% | PASS |
| ac_reads | `oltp_read_only` | 154.37ms | 174.30ms | 1.129× | 1.05% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.35ms | 67.17ms | 3.005× | 4.84% | PASS |
| ac_writes | `oltp_insert_ac` | 25.45ms | 82.69ms | 3.249× | 4.06% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.14ms | 99.25ms | 3.657× | 4.52% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.50ms | 79.53ms | 3.535× | 5.65% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.96ms | 89.94ms | 3.603× | 6.50% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.45ms | 89.25ms | 3.507× | 5.80% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.45ms | 79.56ms | 3.709× | 7.10% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.72ms | 98.28ms | 3.098× | 5.04% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.46ms | 38.58ms | 1.189× | 1.98% | PASS |
| mem_reads | `oltp_range_select` | 14.06ms | 14.39ms | 1.023× | 2.32% | PASS |
| mem_reads | `oltp_sum_range` | 12.72ms | 14.30ms | 1.125× | 1.72% | PASS |
| mem_reads | `oltp_order_range` | 3.00ms | 3.20ms | 1.068× | 1.44% | PASS |
| mem_reads | `oltp_distinct_range` | 4.03ms | 4.29ms | 1.062× | 1.55% | PASS |
| mem_reads | `oltp_index_scan` | 4.84ms | 6.54ms | 1.351× | 1.47% | PASS |
| mem_reads | `select_random_points` | 19.33ms | 21.93ms | 1.134× | 2.61% | PASS |
| mem_reads | `select_random_ranges` | 4.29ms | 5.32ms | 1.241× | 1.72% | PASS |
| mem_reads | `covering_index_scan` | 4.86ms | 4.86ms | 1.000× | 2.17% | PASS |
| mem_reads | `groupby_scan` | 32.38ms | 34.67ms | 1.071× | 0.93% | PASS |
| mem_reads | `index_join` | 7.44ms | 10.20ms | 1.370× | 2.85% | PASS |
| mem_reads | `index_join_scan` | 4.47ms | 5.66ms | 1.268× | 3.86% | PASS |
| mem_reads | `types_table_scan` | 1.19s | 1.29s | 1.084× | 0.67% | PASS |
| mem_reads | `table_scan` | 1.44s | 1.43s | 0.998× | 0.64% | PASS |
| mem_reads | `oltp_read_only` | 130.13ms | 141.93ms | 1.091× | 1.06% | PASS |
| mem_writes | `oltp_bulk_insert` | 243.96ms | 372.18ms | 1.526× | 0.99% | PASS |
| mem_writes | `oltp_insert` | 21.55ms | 43.55ms | 2.021× | 1.89% | PASS |
| mem_writes | `oltp_update_index` | 76.97ms | 149.92ms | 1.948× | 1.75% | PASS |
| mem_writes | `oltp_update_non_index` | 53.06ms | 95.29ms | 1.796× | 1.14% | PASS |
| mem_writes | `oltp_delete_insert` | 54.36ms | 116.46ms | 2.143× | 1.35% | PASS |
| mem_writes | `oltp_write_only` | 31.18ms | 71.14ms | 2.282× | 1.41% | PASS |
| mem_writes | `types_delete_insert` | 35.16ms | 62.23ms | 1.770× | 1.36% | PASS |
| mem_writes | `oltp_read_write` | 94.87ms | 154.06ms | 1.624× | 0.99% | PASS |
| file_reads | `oltp_point_select` | 104.01ms | 64.46ms | 0.620× | 0.78% | PASS |
| file_reads | `oltp_range_select` | 22.18ms | 17.35ms | 0.782× | 1.77% | PASS |
| file_reads | `oltp_sum_range` | 20.76ms | 17.18ms | 0.828× | 1.49% | PASS |
| file_reads | `oltp_order_range` | 4.05ms | 3.64ms | 0.899× | 2.02% | PASS |
| file_reads | `oltp_distinct_range` | 5.11ms | 4.72ms | 0.924× | 1.65% | PASS |
| file_reads | `oltp_index_scan` | 12.46ms | 9.30ms | 0.746× | 1.05% | PASS |
| file_reads | `select_random_points` | 29.04ms | 25.82ms | 0.889× | 2.27% | PASS |
| file_reads | `select_random_ranges` | 11.63ms | 7.76ms | 0.667× | 1.17% | PASS |
| file_reads | `covering_index_scan` | 12.91ms | 7.29ms | 0.565× | 1.78% | PASS |
| file_reads | `groupby_scan` | 33.75ms | 35.35ms | 1.047× | 0.78% | PASS |
| file_reads | `index_join` | 12.03ms | 11.88ms | 0.987× | 1.83% | PASS |
| file_reads | `index_join_scan` | 5.82ms | 6.24ms | 1.071× | 4.52% | PASS |
| file_reads | `types_table_scan` | 1.19s | 1.29s | 1.081× | 0.96% | PASS |
| file_reads | `table_scan` | 1.45s | 1.44s | 0.991× | 0.60% | PASS |
| file_reads | `oltp_read_only` | 240.54ms | 183.26ms | 0.762× | 0.73% | PASS |
| file_writes | `oltp_bulk_insert` | 264.60ms | 404.26ms | 1.528× | 0.92% | PASS |
| file_writes | `oltp_insert` | 33.96ms | 56.86ms | 1.674× | 1.69% | PASS |
| file_writes | `oltp_update_index` | 113.90ms | 185.66ms | 1.630× | 1.47% | PASS |
| file_writes | `oltp_update_non_index` | 85.24ms | 121.33ms | 1.423× | 1.52% | PASS |
| file_writes | `oltp_delete_insert` | 88.48ms | 146.62ms | 1.657× | 1.29% | PASS |
| file_writes | `oltp_write_only` | 60.80ms | 95.76ms | 1.575× | 1.82% | PASS |
| file_writes | `types_delete_insert` | 56.49ms | 79.36ms | 1.405× | 1.60% | PASS |
| file_writes | `oltp_read_write` | 130.79ms | 182.35ms | 1.394× | 1.28% | PASS |
| ac_reads | `oltp_point_select` | 57.92ms | 65.40ms | 1.129× | 1.02% | PASS |
| ac_reads | `oltp_range_select` | 17.82ms | 17.63ms | 0.989× | 2.41% | PASS |
| ac_reads | `oltp_sum_range` | 16.50ms | 17.64ms | 1.069× | 1.54% | PASS |
| ac_reads | `oltp_order_range` | 3.66ms | 3.73ms | 1.018× | 3.08% | PASS |
| ac_reads | `oltp_distinct_range` | 4.72ms | 4.76ms | 1.009× | 2.08% | PASS |
| ac_reads | `oltp_index_scan` | 7.89ms | 9.39ms | 1.191× | 2.28% | PASS |
| ac_reads | `select_random_points` | 23.54ms | 26.13ms | 1.110× | 2.44% | PASS |
| ac_reads | `select_random_ranges` | 7.04ms | 7.82ms | 1.111× | 2.21% | PASS |
| ac_reads | `covering_index_scan` | 8.31ms | 7.37ms | 0.888× | 2.70% | PASS |
| ac_reads | `groupby_scan` | 33.41ms | 35.46ms | 1.061× | 0.95% | PASS |
| ac_reads | `index_join` | 9.78ms | 11.95ms | 1.223× | 3.16% | PASS |
| ac_reads | `index_join_scan` | 5.19ms | 6.22ms | 1.198× | 4.05% | PASS |
| ac_reads | `types_table_scan` | 1.20s | 1.29s | 1.079× | 0.69% | PASS |
| ac_reads | `table_scan` | 1.44s | 1.44s | 0.998× | 0.49% | PASS |
| ac_reads | `oltp_read_only` | 166.85ms | 181.66ms | 1.089× | 0.94% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.06ms | 65.54ms | 2.971× | 6.33% | PASS |
| ac_writes | `oltp_insert_ac` | 24.68ms | 88.60ms | 3.590× | 5.74% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.90ms | 100.08ms | 3.587× | 6.75% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.48ms | 76.83ms | 3.577× | 3.56% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.31ms | 90.32ms | 3.874× | 4.53% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.97ms | 89.14ms | 3.719× | 3.75% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.15ms | 78.90ms | 3.730× | 6.63% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.59ms | 96.65ms | 3.159× | 5.06% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.88ms | 40.82ms | 1.205× | 1.30% | PASS |
| mem_reads | `oltp_range_select` | 19.39ms | 21.19ms | 1.093× | 2.13% | PASS |
| mem_reads | `oltp_sum_range` | 18.20ms | 21.09ms | 1.159× | 1.00% | PASS |
| mem_reads | `oltp_order_range` | 3.61ms | 3.87ms | 1.072× | 1.40% | PASS |
| mem_reads | `oltp_distinct_range` | 4.75ms | 4.95ms | 1.043× | 1.11% | PASS |
| mem_reads | `oltp_index_scan` | 4.70ms | 6.38ms | 1.358× | 2.24% | PASS |
| mem_reads | `select_random_points` | 28.82ms | 32.72ms | 1.135× | 1.74% | PASS |
| mem_reads | `select_random_ranges` | 7.79ms | 9.13ms | 1.171× | 0.88% | PASS |
| mem_reads | `covering_index_scan` | 4.24ms | 4.49ms | 1.058× | 2.21% | PASS |
| mem_reads | `groupby_scan` | 36.72ms | 39.43ms | 1.074× | 0.97% | PASS |
| mem_reads | `index_join` | 8.26ms | 10.80ms | 1.307× | 2.10% | PASS |
| mem_reads | `index_join_scan` | 4.19ms | 5.49ms | 1.312× | 1.31% | PASS |
| mem_reads | `types_table_scan` | 1.08s | 1.25s | 1.159× | 0.91% | PASS |
| mem_reads | `table_scan` | 1.28s | 1.41s | 1.104× | 1.39% | PASS |
| mem_reads | `oltp_read_only` | 155.56ms | 171.91ms | 1.105× | 0.91% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.29ms | 371.64ms | 1.491× | 0.69% | PASS |
| mem_writes | `oltp_insert` | 19.39ms | 39.08ms | 2.015× | 0.90% | PASS |
| mem_writes | `oltp_update_index` | 70.55ms | 127.73ms | 1.811× | 1.85% | PASS |
| mem_writes | `oltp_update_non_index` | 52.58ms | 90.95ms | 1.730× | 1.32% | PASS |
| mem_writes | `oltp_delete_insert` | 51.44ms | 104.32ms | 2.028× | 1.40% | PASS |
| mem_writes | `oltp_write_only` | 28.03ms | 64.47ms | 2.300× | 1.75% | PASS |
| mem_writes | `types_delete_insert` | 33.63ms | 59.11ms | 1.758× | 1.92% | PASS |
| mem_writes | `oltp_read_write` | 106.63ms | 165.27ms | 1.550× | 1.38% | PASS |
| file_reads | `oltp_point_select` | 106.58ms | 65.32ms | 0.613× | 1.30% | PASS |
| file_reads | `oltp_range_select` | 27.47ms | 24.10ms | 0.877× | 1.18% | PASS |
| file_reads | `oltp_sum_range` | 26.25ms | 23.99ms | 0.914× | 1.16% | PASS |
| file_reads | `oltp_order_range` | 4.51ms | 4.22ms | 0.935× | 1.76% | PASS |
| file_reads | `oltp_distinct_range` | 5.68ms | 5.31ms | 0.936× | 1.32% | PASS |
| file_reads | `oltp_index_scan` | 12.17ms | 9.00ms | 0.740× | 1.39% | PASS |
| file_reads | `select_random_points` | 38.02ms | 36.33ms | 0.955× | 2.07% | PASS |
| file_reads | `select_random_ranges` | 15.63ms | 11.89ms | 0.761× | 1.77% | PASS |
| file_reads | `covering_index_scan` | 11.81ms | 7.13ms | 0.603× | 1.38% | PASS |
| file_reads | `groupby_scan` | 37.76ms | 40.11ms | 1.062× | 0.98% | PASS |
| file_reads | `index_join` | 12.55ms | 12.82ms | 1.021× | 1.69% | PASS |
| file_reads | `index_join_scan` | 5.27ms | 5.96ms | 1.131× | 2.20% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.24s | 1.168× | 1.06% | PASS |
| file_reads | `table_scan` | 1.30s | 1.40s | 1.084× | 1.74% | PASS |
| file_reads | `oltp_read_only` | 265.46ms | 209.27ms | 0.788× | 0.93% | PASS |
| file_writes | `oltp_bulk_insert` | 264.25ms | 395.76ms | 1.498× | 1.14% | PASS |
| file_writes | `oltp_insert` | 26.83ms | 49.64ms | 1.850× | 1.83% | PASS |
| file_writes | `oltp_update_index` | 102.26ms | 156.92ms | 1.535× | 1.77% | PASS |
| file_writes | `oltp_update_non_index` | 80.05ms | 113.22ms | 1.414× | 1.95% | PASS |
| file_writes | `oltp_delete_insert` | 80.79ms | 129.89ms | 1.608× | 1.24% | PASS |
| file_writes | `oltp_write_only` | 53.63ms | 85.37ms | 1.592× | 2.07% | PASS |
| file_writes | `types_delete_insert` | 51.76ms | 73.61ms | 1.422× | 1.51% | PASS |
| file_writes | `oltp_read_write` | 135.50ms | 186.72ms | 1.378× | 1.76% | PASS |
| ac_reads | `oltp_point_select` | 58.40ms | 65.22ms | 1.117× | 1.03% | PASS |
| ac_reads | `oltp_range_select` | 22.70ms | 24.10ms | 1.062× | 1.69% | PASS |
| ac_reads | `oltp_sum_range` | 21.35ms | 24.03ms | 1.125× | 1.56% | PASS |
| ac_reads | `oltp_order_range` | 4.07ms | 4.23ms | 1.041× | 1.33% | PASS |
| ac_reads | `oltp_distinct_range` | 5.14ms | 5.33ms | 1.036× | 1.54% | PASS |
| ac_reads | `oltp_index_scan` | 7.50ms | 9.03ms | 1.204× | 1.50% | PASS |
| ac_reads | `select_random_points` | 32.63ms | 36.62ms | 1.122× | 2.21% | PASS |
| ac_reads | `select_random_ranges` | 10.53ms | 11.88ms | 1.128× | 1.55% | PASS |
| ac_reads | `covering_index_scan` | 7.14ms | 7.10ms | 0.994× | 1.55% | PASS |
| ac_reads | `groupby_scan` | 37.13ms | 40.09ms | 1.080× | 0.74% | PASS |
| ac_reads | `index_join` | 10.06ms | 12.80ms | 1.273× | 1.63% | PASS |
| ac_reads | `index_join_scan` | 4.78ms | 6.00ms | 1.256× | 2.26% | PASS |
| ac_reads | `types_table_scan` | 1.07s | 1.24s | 1.163× | 0.88% | PASS |
| ac_reads | `table_scan` | 1.30s | 1.41s | 1.083× | 1.81% | PASS |
| ac_reads | `oltp_read_only` | 193.07ms | 209.34ms | 1.084× | 1.30% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.71ms | 68.61ms | 3.021× | 7.55% | PASS |
| ac_writes | `oltp_insert_ac` | 25.22ms | 88.45ms | 3.507× | 7.80% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.13ms | 99.86ms | 3.550× | 6.19% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.46ms | 80.18ms | 3.569× | 7.37% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.60ms | 90.13ms | 3.664× | 5.90% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.06ms | 92.79ms | 3.561× | 7.29% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.32ms | 81.27ms | 3.641× | 11.87% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.85ms | 98.16ms | 2.988× | 5.28% | PASS |

</details>

## Version-control latency

Wall time: 1m 46s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 57.04ms | 200.00ms | 28.5% | 0.96% | PASS |
| `status_dirty_many_tables` | 58.86ms | 200.00ms | 29.4% | 1.30% | PASS |
| `diff_regular_working_one_table` | 53.83ms | 150.00ms | 35.9% | 1.30% | PASS |
| `diff_regular_working_many_tables` | 60.83ms | 200.00ms | 30.4% | 1.75% | PASS |
| `diff_stat_working_many_tables` | 60.19ms | 200.00ms | 30.1% | 1.82% | PASS |
| `diff_schema_working_many_tables` | 59.70ms | 200.00ms | 29.8% | 1.11% | PASS |
| `branch_list_many_branches` | 16.61ms | 100.00ms | 16.6% | 1.77% | PASS |
| `branch_create_delete` | 18.79ms | 100.00ms | 18.8% | 2.17% | PASS |
| `checkout_branch_clean` | 78.32ms | 200.00ms | 39.2% | 0.83% | PASS |
| `merge_data_no_conflicts` | 30.85ms | 150.00ms | 20.6% | 3.26% | PASS |
| `merge_schema_no_conflicts` | 17.91ms | 100.00ms | 17.9% | 2.61% | PASS |
| `merge_data_conflicts` | 75.89ms | 250.00ms | 30.4% | 1.64% | PASS |
| `merge_data_conflicts_with_resolve` | 75.05ms | 250.00ms | 30.0% | 1.00% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
