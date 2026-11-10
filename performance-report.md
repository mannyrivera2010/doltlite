# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-10 18:51 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu26 20261101.186.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/42862717852)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 52m 3s | 6.33s | 7.64s | 1.207× | 2.28% | **PASS** |
| textpk | 69 | 55 | 1h 34m 12s | 10.13s | 11.86s | 1.170× | 2.45% | **PASS** |
| blobpk | 69 | 55 | 1h 28m 20s | 9.20s | 10.11s | 1.099× | 1.65% | **PASS** |
| compositepk | 69 | 55 | 1h 19m 27s | 8.84s | 9.85s | 1.115× | 1.81% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 15.87ms | 17.00ms | 1.071× | 1.98% | PASS |
| mem_reads | `oltp_range_select` | 7.35ms | 7.96ms | 1.083× | 4.52% | PASS |
| mem_reads | `oltp_sum_range` | 7.20ms | 7.81ms | 1.085× | 2.01% | PASS |
| mem_reads | `oltp_order_range` | 1.84ms | 1.96ms | 1.062× | 1.76% | PASS |
| mem_reads | `oltp_distinct_range` | 2.43ms | 2.58ms | 1.062× | 1.11% | PASS |
| mem_reads | `oltp_index_scan` | 2.63ms | 3.31ms | 1.256× | 2.20% | PASS |
| mem_reads | `select_random_points` | 8.45ms | 8.42ms | 0.996× | 3.93% | PASS |
| mem_reads | `select_random_ranges` | 2.32ms | 2.45ms | 1.057× | 3.35% | PASS |
| mem_reads | `covering_index_scan` | 2.39ms | 2.47ms | 1.033× | 1.25% | PASS |
| mem_reads | `groupby_scan` | 20.46ms | 22.34ms | 1.091× | 0.71% | PASS |
| mem_reads | `index_join` | 4.12ms | 5.45ms | 1.321× | 1.11% | PASS |
| mem_reads | `index_join_scan` | 2.27ms | 3.51ms | 1.543× | 3.74% | PASS |
| mem_reads | `types_table_scan` | 717.45ms | 831.04ms | 1.158× | 0.54% | PASS |
| mem_reads | `table_scan` | 819.94ms | 902.65ms | 1.101× | 1.38% | PASS |
| mem_reads | `oltp_read_only` | 64.36ms | 72.95ms | 1.133× | 1.30% | PASS |
| mem_writes | `oltp_bulk_insert` | 98.00ms | 137.85ms | 1.407× | 0.90% | PASS |
| mem_writes | `oltp_insert` | 9.03ms | 16.36ms | 1.812× | 1.14% | PASS |
| mem_writes | `oltp_update_index` | 33.43ms | 68.32ms | 2.044× | 1.62% | PASS |
| mem_writes | `oltp_update_non_index` | 21.60ms | 37.46ms | 1.734× | 1.78% | PASS |
| mem_writes | `oltp_delete_insert` | 29.05ms | 48.83ms | 1.681× | 1.82% | PASS |
| mem_writes | `oltp_write_only` | 14.64ms | 28.79ms | 1.966× | 2.62% | PASS |
| mem_writes | `types_delete_insert` | 16.36ms | 25.56ms | 1.562× | 2.28% | PASS |
| mem_writes | `oltp_read_write` | 41.11ms | 65.27ms | 1.588× | 1.47% | PASS |
| file_reads | `oltp_point_select` | 36.08ms | 23.27ms | 0.645× | 1.36% | PASS |
| file_reads | `oltp_range_select` | 9.82ms | 8.80ms | 0.897× | 2.31% | PASS |
| file_reads | `oltp_sum_range` | 9.58ms | 8.72ms | 0.910× | 1.84% | PASS |
| file_reads | `oltp_order_range` | 2.10ms | 2.08ms | 0.991× | 2.22% | PASS |
| file_reads | `oltp_distinct_range` | 2.71ms | 2.71ms | 1.000× | 2.31% | PASS |
| file_reads | `oltp_index_scan` | 4.96ms | 4.21ms | 0.851× | 2.05% | PASS |
| file_reads | `select_random_points` | 11.28ms | 9.37ms | 0.831× | 4.02% | PASS |
| file_reads | `select_random_ranges` | 4.62ms | 3.17ms | 0.687× | 2.25% | PASS |
| file_reads | `covering_index_scan` | 4.52ms | 3.09ms | 0.685× | 1.22% | PASS |
| file_reads | `groupby_scan` | 21.07ms | 22.42ms | 1.064× | 1.18% | PASS |
| file_reads | `index_join` | 5.26ms | 5.82ms | 1.107× | 2.39% | PASS |
| file_reads | `index_join_scan` | 2.37ms | 3.35ms | 1.417× | 6.42% | PASS |
| file_reads | `types_table_scan` | 717.44ms | 838.82ms | 1.169× | 1.01% | PASS |
| file_reads | `table_scan` | 814.73ms | 902.39ms | 1.108× | 0.66% | PASS |
| file_reads | `oltp_read_only` | 94.26ms | 82.76ms | 0.878× | 1.19% | PASS |
| file_writes | `oltp_bulk_insert` | 134.18ms | 188.98ms | 1.408× | 2.87% | PASS |
| file_writes | `oltp_insert` | 19.97ms | 28.81ms | 1.442× | 1.37% | PASS |
| file_writes | `oltp_update_index` | 114.69ms | 119.56ms | 1.042× | 3.56% | PASS |
| file_writes | `oltp_update_non_index` | 94.16ms | 79.34ms | 0.843× | 2.67% | PASS |
| file_writes | `oltp_delete_insert` | 101.26ms | 101.27ms | 1.000× | 3.06% | PASS |
| file_writes | `oltp_write_only` | 71.31ms | 67.29ms | 0.944× | 7.37% | PASS |
| file_writes | `types_delete_insert` | 60.13ms | 49.09ms | 0.816× | 10.18% | PASS |
| file_writes | `oltp_read_write` | 100.15ms | 105.35ms | 1.052× | 5.87% | PASS |
| ac_reads | `oltp_point_select` | 22.27ms | 23.53ms | 1.056× | 0.90% | PASS |
| ac_reads | `oltp_range_select` | 8.09ms | 8.59ms | 1.062× | 4.35% | PASS |
| ac_reads | `oltp_sum_range` | 7.99ms | 8.62ms | 1.079× | 3.59% | PASS |
| ac_reads | `oltp_order_range` | 1.95ms | 2.04ms | 1.047× | 2.31% | PASS |
| ac_reads | `oltp_distinct_range` | 2.55ms | 2.69ms | 1.056× | 2.45% | PASS |
| ac_reads | `oltp_index_scan` | 3.40ms | 3.92ms | 1.156× | 5.01% | PASS |
| ac_reads | `select_random_points` | 10.36ms | 9.51ms | 0.919× | 2.89% | PASS |
| ac_reads | `select_random_ranges` | 3.11ms | 3.08ms | 0.990× | 3.26% | PASS |
| ac_reads | `covering_index_scan` | 3.13ms | 3.13ms | 1.001× | 1.68% | PASS |
| ac_reads | `groupby_scan` | 20.86ms | 22.39ms | 1.073× | 0.78% | PASS |
| ac_reads | `index_join` | 4.54ms | 5.79ms | 1.276× | 2.77% | PASS |
| ac_reads | `index_join_scan` | 2.41ms | 3.40ms | 1.409× | 5.15% | PASS |
| ac_reads | `types_table_scan` | 716.69ms | 839.81ms | 1.172× | 0.80% | PASS |
| ac_reads | `table_scan` | 815.13ms | 901.53ms | 1.106× | 0.78% | PASS |
| ac_reads | `oltp_read_only` | 72.95ms | 81.70ms | 1.120× | 1.45% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 30.23ms | 79.78ms | 2.639× | 3.96% | PASS |
| ac_writes | `oltp_insert_ac` | 32.20ms | 91.66ms | 2.847× | 4.05% | PASS |
| ac_writes | `oltp_update_index_ac` | 33.31ms | 101.24ms | 3.039× | 3.77% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 30.27ms | 86.20ms | 2.847× | 2.99% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 31.43ms | 93.16ms | 2.964× | 5.30% | PASS |
| ac_writes | `oltp_write_only_ac` | 31.33ms | 91.70ms | 2.927× | 4.50% | PASS |
| ac_writes | `types_delete_insert_ac` | 30.41ms | 93.34ms | 3.070× | 5.15% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.85ms | 97.42ms | 2.877× | 2.90% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.20ms | 38.52ms | 1.235× | 2.27% | PASS |
| mem_reads | `oltp_range_select` | 16.30ms | 15.25ms | 0.935× | 4.19% | PASS |
| mem_reads | `oltp_sum_range` | 12.43ms | 14.20ms | 1.142× | 4.97% | PASS |
| mem_reads | `oltp_order_range` | 3.00ms | 3.20ms | 1.064× | 2.01% | PASS |
| mem_reads | `oltp_distinct_range` | 4.04ms | 4.35ms | 1.076× | 1.46% | PASS |
| mem_reads | `oltp_index_scan` | 4.46ms | 6.14ms | 1.378× | 3.60% | PASS |
| mem_reads | `select_random_points` | 17.98ms | 20.65ms | 1.148× | 3.36% | PASS |
| mem_reads | `select_random_ranges` | 4.12ms | 5.31ms | 1.289× | 2.01% | PASS |
| mem_reads | `covering_index_scan` | 4.49ms | 4.61ms | 1.026× | 2.77% | PASS |
| mem_reads | `groupby_scan` | 33.01ms | 36.73ms | 1.113× | 2.25% | PASS |
| mem_reads | `index_join` | 6.92ms | 8.96ms | 1.294× | 2.04% | PASS |
| mem_reads | `index_join_scan` | 4.94ms | 5.75ms | 1.162× | 5.13% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.24s | 1.168× | 0.80% | PASS |
| mem_reads | `table_scan` | 1.20s | 1.34s | 1.116× | 0.90% | PASS |
| mem_reads | `oltp_read_only` | 122.77ms | 139.28ms | 1.134× | 1.40% | PASS |
| mem_writes | `oltp_bulk_insert` | 231.09ms | 364.30ms | 1.576× | 1.07% | PASS |
| mem_writes | `oltp_insert` | 21.70ms | 40.66ms | 1.873× | 1.42% | PASS |
| mem_writes | `oltp_update_index` | 75.42ms | 143.40ms | 1.901× | 1.96% | PASS |
| mem_writes | `oltp_update_non_index` | 51.06ms | 92.89ms | 1.819× | 2.22% | PASS |
| mem_writes | `oltp_delete_insert` | 54.02ms | 111.28ms | 2.060× | 1.81% | PASS |
| mem_writes | `oltp_write_only` | 30.78ms | 66.38ms | 2.157× | 1.34% | PASS |
| mem_writes | `types_delete_insert` | 34.72ms | 60.57ms | 1.745× | 1.49% | PASS |
| mem_writes | `oltp_read_write` | 95.18ms | 148.44ms | 1.560× | 2.45% | PASS |
| file_reads | `oltp_point_select` | 102.08ms | 62.77ms | 0.615× | 1.17% | PASS |
| file_reads | `oltp_range_select` | 23.35ms | 17.54ms | 0.751× | 2.45% | PASS |
| file_reads | `oltp_sum_range` | 20.69ms | 16.92ms | 0.818× | 2.33% | PASS |
| file_reads | `oltp_order_range` | 4.15ms | 3.55ms | 0.857× | 1.39% | PASS |
| file_reads | `oltp_distinct_range` | 5.19ms | 4.74ms | 0.914× | 1.47% | PASS |
| file_reads | `oltp_index_scan` | 12.27ms | 9.04ms | 0.737× | 1.50% | PASS |
| file_reads | `select_random_points` | 28.08ms | 25.40ms | 0.904× | 3.02% | PASS |
| file_reads | `select_random_ranges` | 11.59ms | 7.93ms | 0.684× | 1.84% | PASS |
| file_reads | `covering_index_scan` | 13.80ms | 7.64ms | 0.554× | 1.91% | PASS |
| file_reads | `groupby_scan` | 35.76ms | 37.92ms | 1.060× | 2.12% | PASS |
| file_reads | `index_join` | 11.89ms | 11.44ms | 0.962× | 3.66% | PASS |
| file_reads | `index_join_scan` | 5.84ms | 6.18ms | 1.059× | 3.91% | PASS |
| file_reads | `types_table_scan` | 1.07s | 1.24s | 1.165× | 0.85% | PASS |
| file_reads | `table_scan` | 1.42s | 1.37s | 0.969× | 4.52% | PASS |
| file_reads | `oltp_read_only` | 233.90ms | 179.23ms | 0.766× | 1.27% | PASS |
| file_writes | `oltp_bulk_insert` | 249.73ms | 397.52ms | 1.592× | 0.75% | PASS |
| file_writes | `oltp_insert` | 53.10ms | 54.69ms | 1.030× | 24.20% | PASS |
| file_writes | `oltp_update_index` | 117.91ms | 177.84ms | 1.508× | 1.87% | PASS |
| file_writes | `oltp_update_non_index` | 101.22ms | 120.86ms | 1.194× | 10.12% | PASS |
| file_writes | `oltp_delete_insert` | 95.41ms | 142.21ms | 1.490× | 2.63% | PASS |
| file_writes | `oltp_write_only` | 85.20ms | 91.29ms | 1.071× | 12.70% | PASS |
| file_writes | `types_delete_insert` | 58.15ms | 78.58ms | 1.351× | 1.53% | PASS |
| file_writes | `oltp_read_write` | 144.01ms | 170.89ms | 1.187× | 7.05% | PASS |
| ac_reads | `oltp_point_select` | 53.87ms | 62.45ms | 1.159× | 1.79% | PASS |
| ac_reads | `oltp_range_select` | 18.25ms | 17.38ms | 0.953× | 4.14% | PASS |
| ac_reads | `oltp_sum_range` | 15.65ms | 17.07ms | 1.091× | 2.19% | PASS |
| ac_reads | `oltp_order_range` | 3.64ms | 3.58ms | 0.984× | 2.82% | PASS |
| ac_reads | `oltp_distinct_range` | 4.66ms | 4.75ms | 1.020× | 2.04% | PASS |
| ac_reads | `oltp_index_scan` | 7.58ms | 9.03ms | 1.192× | 3.01% | PASS |
| ac_reads | `select_random_points` | 21.75ms | 24.42ms | 1.122× | 3.46% | PASS |
| ac_reads | `select_random_ranges` | 6.68ms | 7.85ms | 1.175× | 2.05% | PASS |
| ac_reads | `covering_index_scan` | 8.36ms | 7.38ms | 0.883× | 2.80% | PASS |
| ac_reads | `groupby_scan` | 34.10ms | 37.33ms | 1.095× | 2.36% | PASS |
| ac_reads | `index_join` | 9.87ms | 11.65ms | 1.181× | 4.93% | PASS |
| ac_reads | `index_join_scan` | 5.56ms | 6.29ms | 1.130× | 4.95% | PASS |
| ac_reads | `types_table_scan` | 1.16s | 1.27s | 1.098× | 4.47% | PASS |
| ac_reads | `table_scan` | 1.40s | 1.37s | 0.980× | 5.11% | PASS |
| ac_reads | `oltp_read_only` | 165.51ms | 178.74ms | 1.080× | 2.76% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.59ms | 66.15ms | 3.213× | 6.80% | PASS |
| ac_writes | `oltp_insert_ac` | 23.70ms | 79.20ms | 3.341× | 5.13% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.74ms | 95.95ms | 3.589× | 4.92% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.45ms | 75.33ms | 3.356× | 7.06% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.45ms | 91.19ms | 3.729× | 7.27% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.98ms | 87.15ms | 3.635× | 4.54% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.98ms | 81.73ms | 3.719× | 8.96% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.00ms | 95.42ms | 3.078× | 4.43% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.98ms | 27.41ms | 1.143× | 1.53% | PASS |
| mem_reads | `oltp_range_select` | 11.54ms | 10.95ms | 0.949× | 1.89% | PASS |
| mem_reads | `oltp_sum_range` | 10.11ms | 10.62ms | 1.050× | 2.97% | PASS |
| mem_reads | `oltp_order_range` | 2.50ms | 2.55ms | 1.019× | 0.75% | PASS |
| mem_reads | `oltp_distinct_range` | 3.26ms | 3.35ms | 1.026× | 1.07% | PASS |
| mem_reads | `oltp_index_scan` | 3.69ms | 4.84ms | 1.313× | 1.35% | PASS |
| mem_reads | `select_random_points` | 14.22ms | 15.95ms | 1.122× | 1.68% | PASS |
| mem_reads | `select_random_ranges` | 3.45ms | 4.23ms | 1.226× | 1.50% | PASS |
| mem_reads | `covering_index_scan` | 3.56ms | 3.71ms | 1.041× | 1.71% | PASS |
| mem_reads | `groupby_scan` | 27.31ms | 27.32ms | 1.000× | 0.89% | PASS |
| mem_reads | `index_join` | 5.45ms | 6.92ms | 1.269× | 3.17% | PASS |
| mem_reads | `index_join_scan` | 3.52ms | 4.59ms | 1.304× | 1.96% | PASS |
| mem_reads | `types_table_scan` | 879.64ms | 976.29ms | 1.110× | 1.24% | PASS |
| mem_reads | `table_scan` | 1.03s | 1.07s | 1.039× | 2.12% | PASS |
| mem_reads | `oltp_read_only` | 94.25ms | 101.33ms | 1.075× | 1.65% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.72ms | 266.49ms | 1.483× | 0.63% | PASS |
| mem_writes | `oltp_insert` | 15.43ms | 31.75ms | 2.059× | 0.75% | PASS |
| mem_writes | `oltp_update_index` | 53.88ms | 107.11ms | 1.988× | 1.54% | PASS |
| mem_writes | `oltp_update_non_index` | 39.01ms | 68.92ms | 1.767× | 1.02% | PASS |
| mem_writes | `oltp_delete_insert` | 39.21ms | 84.39ms | 2.152× | 1.04% | PASS |
| mem_writes | `oltp_write_only` | 22.81ms | 52.31ms | 2.293× | 2.14% | PASS |
| mem_writes | `types_delete_insert` | 25.54ms | 44.64ms | 1.748× | 1.48% | PASS |
| mem_writes | `oltp_read_write` | 64.73ms | 106.80ms | 1.650× | 2.33% | PASS |
| file_reads | `oltp_point_select` | 91.58ms | 49.60ms | 0.542× | 0.60% | PASS |
| file_reads | `oltp_range_select` | 17.98ms | 13.15ms | 0.731× | 1.51% | PASS |
| file_reads | `oltp_sum_range` | 16.60ms | 12.84ms | 0.773× | 1.33% | PASS |
| file_reads | `oltp_order_range` | 3.20ms | 2.82ms | 0.881× | 1.76% | PASS |
| file_reads | `oltp_distinct_range` | 4.00ms | 3.61ms | 0.902× | 1.32% | PASS |
| file_reads | `oltp_index_scan` | 10.75ms | 7.22ms | 0.672× | 1.10% | PASS |
| file_reads | `select_random_points` | 20.88ms | 17.94ms | 0.859× | 1.12% | PASS |
| file_reads | `select_random_ranges` | 10.35ms | 6.50ms | 0.628× | 0.86% | PASS |
| file_reads | `covering_index_scan` | 11.38ms | 6.17ms | 0.542× | 1.20% | PASS |
| file_reads | `groupby_scan` | 28.26ms | 27.62ms | 0.977× | 0.97% | PASS |
| file_reads | `index_join` | 9.84ms | 9.17ms | 0.933× | 1.40% | PASS |
| file_reads | `index_join_scan` | 4.44ms | 5.01ms | 1.129× | 0.99% | PASS |
| file_reads | `types_table_scan` | 912.17ms | 1.00s | 1.097× | 3.09% | PASS |
| file_reads | `table_scan` | 1.34s | 1.12s | 0.835× | 7.05% | PASS |
| file_reads | `oltp_read_only` | 211.35ms | 138.50ms | 0.655× | 4.45% | PASS |
| file_writes | `oltp_bulk_insert` | 239.37ms | 346.52ms | 1.448× | 2.37% | PASS |
| file_writes | `oltp_insert` | 53.41ms | 62.31ms | 1.167× | 18.56% | PASS |
| file_writes | `oltp_update_index` | 185.29ms | 203.63ms | 1.099× | 5.19% | PASS |
| file_writes | `oltp_update_non_index` | 147.92ms | 138.76ms | 0.938× | 13.26% | PASS |
| file_writes | `oltp_delete_insert` | 163.45ms | 177.92ms | 1.089× | 12.58% | PASS |
| file_writes | `oltp_write_only` | 118.34ms | 123.27ms | 1.042× | 9.46% | PASS |
| file_writes | `types_delete_insert` | 101.68ms | 97.48ms | 0.959× | 9.61% | PASS |
| file_writes | `oltp_read_write` | 178.85ms | 190.51ms | 1.065× | 7.65% | PASS |
| ac_reads | `oltp_point_select` | 45.34ms | 51.09ms | 1.127× | 3.27% | PASS |
| ac_reads | `oltp_range_select` | 13.70ms | 13.20ms | 0.963× | 0.91% | PASS |
| ac_reads | `oltp_sum_range` | 12.31ms | 12.89ms | 1.047× | 1.15% | PASS |
| ac_reads | `oltp_order_range` | 2.88ms | 2.83ms | 0.984× | 0.93% | PASS |
| ac_reads | `oltp_distinct_range` | 3.64ms | 3.63ms | 0.997× | 1.08% | PASS |
| ac_reads | `oltp_index_scan` | 6.42ms | 7.29ms | 1.135× | 0.99% | PASS |
| ac_reads | `select_random_points` | 17.08ms | 18.54ms | 1.086× | 1.71% | PASS |
| ac_reads | `select_random_ranges` | 5.78ms | 6.54ms | 1.132× | 1.16% | PASS |
| ac_reads | `covering_index_scan` | 6.82ms | 6.20ms | 0.909× | 0.88% | PASS |
| ac_reads | `groupby_scan` | 28.10ms | 27.84ms | 0.991× | 0.68% | PASS |
| ac_reads | `index_join` | 7.54ms | 9.18ms | 1.216× | 1.52% | PASS |
| ac_reads | `index_join_scan` | 4.14ms | 5.17ms | 1.248× | 1.84% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.04s | 1.009× | 1.56% | PASS |
| ac_reads | `table_scan` | 1.23s | 1.11s | 0.904× | 5.66% | PASS |
| ac_reads | `oltp_read_only` | 126.79ms | 134.87ms | 1.064× | 1.88% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 30.55ms | 85.50ms | 2.799× | 43.09% | PASS |
| ac_writes | `oltp_insert_ac` | 26.11ms | 98.88ms | 3.787× | 24.16% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.86ms | 99.40ms | 3.568× | 17.75% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.86ms | 83.38ms | 3.494× | 28.59% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 31.37ms | 163.20ms | 5.203× | 62.30% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.32ms | 118.93ms | 4.519× | 44.31% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.88ms | 95.71ms | 4.184× | 37.65% | PASS |
| ac_writes | `oltp_read_write_ac` | 41.87ms | 128.33ms | 3.065× | 43.29% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 28.51ms | 31.52ms | 1.106× | 2.09% | PASS |
| mem_reads | `oltp_range_select` | 16.83ms | 16.25ms | 0.965× | 2.62% | PASS |
| mem_reads | `oltp_sum_range` | 14.41ms | 15.56ms | 1.080× | 1.78% | PASS |
| mem_reads | `oltp_order_range` | 3.06ms | 3.06ms | 1.002× | 1.84% | PASS |
| mem_reads | `oltp_distinct_range` | 3.89ms | 3.86ms | 0.992× | 1.31% | PASS |
| mem_reads | `oltp_index_scan` | 3.91ms | 4.98ms | 1.275× | 2.00% | PASS |
| mem_reads | `select_random_points` | 23.06ms | 24.61ms | 1.067× | 3.83% | PASS |
| mem_reads | `select_random_ranges` | 6.44ms | 7.00ms | 1.087× | 1.77% | PASS |
| mem_reads | `covering_index_scan` | 3.53ms | 3.71ms | 1.051× | 4.22% | PASS |
| mem_reads | `groupby_scan` | 30.90ms | 31.50ms | 1.019× | 1.33% | PASS |
| mem_reads | `index_join` | 6.47ms | 7.92ms | 1.225× | 1.55% | PASS |
| mem_reads | `index_join_scan` | 3.45ms | 4.79ms | 1.387× | 2.39% | PASS |
| mem_reads | `types_table_scan` | 904.31ms | 1.00s | 1.110× | 1.42% | PASS |
| mem_reads | `table_scan` | 1.06s | 1.08s | 1.025× | 3.39% | PASS |
| mem_reads | `oltp_read_only` | 122.31ms | 126.26ms | 1.032× | 1.35% | PASS |
| mem_writes | `oltp_bulk_insert` | 189.64ms | 268.14ms | 1.414× | 0.94% | PASS |
| mem_writes | `oltp_insert` | 15.09ms | 29.36ms | 1.946× | 1.81% | PASS |
| mem_writes | `oltp_update_index` | 55.89ms | 98.20ms | 1.757× | 1.34% | PASS |
| mem_writes | `oltp_update_non_index` | 43.08ms | 70.16ms | 1.629× | 1.15% | PASS |
| mem_writes | `oltp_delete_insert` | 41.59ms | 79.92ms | 1.922× | 1.50% | PASS |
| mem_writes | `oltp_write_only` | 23.32ms | 50.26ms | 2.155× | 2.07% | PASS |
| mem_writes | `types_delete_insert` | 27.72ms | 44.85ms | 1.618× | 2.18% | PASS |
| mem_writes | `oltp_read_write` | 82.14ms | 122.26ms | 1.489× | 1.73% | PASS |
| file_reads | `oltp_point_select` | 98.17ms | 54.13ms | 0.551× | 1.22% | PASS |
| file_reads | `oltp_range_select` | 23.87ms | 18.59ms | 0.779× | 1.45% | PASS |
| file_reads | `oltp_sum_range` | 21.95ms | 18.14ms | 0.826× | 1.62% | PASS |
| file_reads | `oltp_order_range` | 3.88ms | 3.38ms | 0.869× | 1.33% | PASS |
| file_reads | `oltp_distinct_range` | 4.71ms | 4.19ms | 0.890× | 1.06% | PASS |
| file_reads | `oltp_index_scan` | 11.06ms | 7.47ms | 0.676× | 1.53% | PASS |
| file_reads | `select_random_points` | 28.37ms | 25.72ms | 0.907× | 1.86% | PASS |
| file_reads | `select_random_ranges` | 13.22ms | 9.04ms | 0.684× | 1.01% | PASS |
| file_reads | `covering_index_scan` | 11.09ms | 6.32ms | 0.569× | 1.40% | PASS |
| file_reads | `groupby_scan` | 33.10ms | 33.81ms | 1.021× | 1.97% | PASS |
| file_reads | `index_join` | 10.86ms | 10.38ms | 0.956× | 1.68% | PASS |
| file_reads | `index_join_scan` | 4.39ms | 5.11ms | 1.165× | 3.24% | PASS |
| file_reads | `types_table_scan` | 980.14ms | 1.04s | 1.062× | 2.22% | PASS |
| file_reads | `table_scan` | 1.21s | 1.09s | 0.905× | 5.01% | PASS |
| file_reads | `oltp_read_only` | 234.11ms | 167.01ms | 0.713× | 1.20% | PASS |
| file_writes | `oltp_bulk_insert` | 237.20ms | 332.48ms | 1.402× | 3.86% | PASS |
| file_writes | `oltp_insert` | 35.59ms | 54.74ms | 1.538× | 9.81% | PASS |
| file_writes | `oltp_update_index` | 175.45ms | 188.41ms | 1.074× | 10.11% | PASS |
| file_writes | `oltp_update_non_index` | 142.83ms | 133.76ms | 0.936× | 1.80% | PASS |
| file_writes | `oltp_delete_insert` | 139.11ms | 152.21ms | 1.094× | 2.94% | PASS |
| file_writes | `oltp_write_only` | 100.96ms | 109.53ms | 1.085× | 5.89% | PASS |
| file_writes | `types_delete_insert` | 90.43ms | 85.47ms | 0.945× | 3.68% | PASS |
| file_writes | `oltp_read_write` | 167.52ms | 184.79ms | 1.103× | 5.89% | PASS |
| ac_reads | `oltp_point_select` | 50.40ms | 55.60ms | 1.103× | 2.45% | PASS |
| ac_reads | `oltp_range_select` | 19.27ms | 18.50ms | 0.960× | 2.25% | PASS |
| ac_reads | `oltp_sum_range` | 17.43ms | 18.19ms | 1.043× | 1.48% | PASS |
| ac_reads | `oltp_order_range` | 3.50ms | 3.40ms | 0.970× | 1.89% | PASS |
| ac_reads | `oltp_distinct_range` | 4.24ms | 4.15ms | 0.979× | 1.54% | PASS |
| ac_reads | `oltp_index_scan` | 6.52ms | 7.46ms | 1.144× | 1.49% | PASS |
| ac_reads | `select_random_points` | 25.33ms | 26.55ms | 1.048× | 2.92% | PASS |
| ac_reads | `select_random_ranges` | 8.57ms | 9.10ms | 1.062× | 1.04% | PASS |
| ac_reads | `covering_index_scan` | 6.11ms | 6.15ms | 1.006× | 1.08% | PASS |
| ac_reads | `groupby_scan` | 30.58ms | 32.02ms | 1.047× | 1.16% | PASS |
| ac_reads | `index_join` | 8.12ms | 10.11ms | 1.245× | 1.16% | PASS |
| ac_reads | `index_join_scan` | 3.83ms | 5.08ms | 1.325× | 1.40% | PASS |
| ac_reads | `types_table_scan` | 873.71ms | 982.26ms | 1.124× | 0.99% | PASS |
| ac_reads | `table_scan` | 990.28ms | 1.05s | 1.064× | 0.47% | PASS |
| ac_reads | `oltp_read_only` | 146.42ms | 157.82ms | 1.078× | 0.96% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.91ms | 55.89ms | 3.121× | 12.29% | PASS |
| ac_writes | `oltp_insert_ac` | 19.70ms | 71.84ms | 3.647× | 9.97% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.62ms | 80.80ms | 3.919× | 9.47% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 18.30ms | 68.93ms | 3.767× | 18.86% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 19.95ms | 86.15ms | 4.319× | 20.86% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.76ms | 75.30ms | 3.811× | 15.46% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.34ms | 69.10ms | 3.985× | 16.71% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.57ms | 80.84ms | 3.162× | 11.03% | PASS |

</details>

## Version-control latency

Wall time: 2m 1s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 61.40ms | 200.00ms | 30.7% | 0.51% | PASS |
| `status_dirty_many_tables` | 63.49ms | 200.00ms | 31.7% | 0.56% | PASS |
| `diff_regular_working_one_table` | 57.76ms | 150.00ms | 38.5% | 0.50% | PASS |
| `diff_regular_working_many_tables` | 67.84ms | 200.00ms | 33.9% | 0.50% | PASS |
| `diff_stat_working_many_tables` | 68.16ms | 200.00ms | 34.1% | 0.58% | PASS |
| `diff_schema_working_many_tables` | 68.47ms | 200.00ms | 34.2% | 0.43% | PASS |
| `branch_list_many_branches` | 16.39ms | 100.00ms | 16.4% | 1.29% | PASS |
| `branch_create_delete` | 25.91ms | 100.00ms | 25.9% | 3.70% | PASS |
| `checkout_branch_clean` | 89.07ms | 200.00ms | 44.5% | 3.31% | PASS |
| `merge_data_no_conflicts` | 32.40ms | 150.00ms | 21.6% | 4.07% | PASS |
| `merge_schema_no_conflicts` | 16.56ms | 100.00ms | 16.6% | 5.22% | PASS |
| `merge_data_conflicts` | 94.81ms | 250.00ms | 37.9% | 0.31% | PASS |
| `merge_data_conflicts_with_resolve` | 95.20ms | 250.00ms | 38.1% | 0.28% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
