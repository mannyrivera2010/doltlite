# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-22 16:13 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261021.345.10
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/39797144073)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 14m 35s | 8.37s | 9.50s | 1.135× | 1.55% | **PASS** |
| textpk | 69 | 55 | 1h 33m 37s | 10.12s | 11.41s | 1.127× | 1.15% | **PASS** |
| blobpk | 69 | 55 | 1h 10m 25s | 7.30s | 8.04s | 1.102× | 2.98% | **PASS** |
| compositepk | 69 | 55 | 1h 30m 47s | 10.96s | 12.13s | 1.106× | 1.62% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.42ms | 22.29ms | 1.092× | 1.69% | PASS |
| mem_reads | `oltp_range_select` | 9.22ms | 9.64ms | 1.045× | 2.05% | PASS |
| mem_reads | `oltp_sum_range` | 8.46ms | 9.33ms | 1.103× | 1.81% | PASS |
| mem_reads | `oltp_order_range` | 2.23ms | 2.33ms | 1.043× | 1.02% | PASS |
| mem_reads | `oltp_distinct_range` | 3.04ms | 3.12ms | 1.023× | 0.93% | PASS |
| mem_reads | `oltp_index_scan` | 3.28ms | 4.39ms | 1.337× | 1.76% | PASS |
| mem_reads | `select_random_points` | 9.38ms | 9.59ms | 1.022× | 1.72% | PASS |
| mem_reads | `select_random_ranges` | 2.60ms | 3.27ms | 1.258× | 1.35% | PASS |
| mem_reads | `covering_index_scan` | 3.40ms | 3.51ms | 1.031× | 2.82% | PASS |
| mem_reads | `groupby_scan` | 25.59ms | 27.36ms | 1.069× | 0.58% | PASS |
| mem_reads | `index_join` | 4.76ms | 6.57ms | 1.382× | 3.41% | PASS |
| mem_reads | `index_join_scan` | 2.80ms | 3.87ms | 1.381× | 1.50% | PASS |
| mem_reads | `types_table_scan` | 869.77ms | 985.10ms | 1.133× | 0.89% | PASS |
| mem_reads | `table_scan` | 1.01s | 1.08s | 1.075× | 2.09% | PASS |
| mem_reads | `oltp_read_only` | 82.83ms | 90.60ms | 1.094× | 1.79% | PASS |
| mem_writes | `oltp_bulk_insert` | 139.41ms | 186.68ms | 1.339× | 0.87% | PASS |
| mem_writes | `oltp_insert` | 12.34ms | 21.60ms | 1.751× | 1.11% | PASS |
| mem_writes | `oltp_update_index` | 42.12ms | 84.03ms | 1.995× | 1.39% | PASS |
| mem_writes | `oltp_update_non_index` | 29.83ms | 46.46ms | 1.558× | 1.41% | PASS |
| mem_writes | `oltp_delete_insert` | 38.31ms | 63.41ms | 1.655× | 1.49% | PASS |
| mem_writes | `oltp_write_only` | 19.29ms | 36.74ms | 1.905× | 1.78% | PASS |
| mem_writes | `types_delete_insert` | 21.36ms | 31.73ms | 1.485× | 1.50% | PASS |
| mem_writes | `oltp_read_write` | 58.03ms | 85.49ms | 1.473× | 2.72% | PASS |
| file_reads | `oltp_point_select` | 93.89ms | 46.60ms | 0.496× | 0.61% | PASS |
| file_reads | `oltp_range_select` | 16.83ms | 12.03ms | 0.715× | 1.56% | PASS |
| file_reads | `oltp_sum_range` | 15.93ms | 11.75ms | 0.738× | 1.06% | PASS |
| file_reads | `oltp_order_range` | 3.06ms | 2.59ms | 0.848× | 0.94% | PASS |
| file_reads | `oltp_distinct_range` | 3.84ms | 3.35ms | 0.871× | 0.88% | PASS |
| file_reads | `oltp_index_scan` | 10.92ms | 6.88ms | 0.631× | 0.87% | PASS |
| file_reads | `select_random_points` | 16.81ms | 11.84ms | 0.704× | 1.55% | PASS |
| file_reads | `select_random_ranges` | 10.05ms | 5.71ms | 0.568× | 0.63% | PASS |
| file_reads | `covering_index_scan` | 11.31ms | 6.11ms | 0.540× | 0.84% | PASS |
| file_reads | `groupby_scan` | 26.40ms | 27.55ms | 1.044× | 0.83% | PASS |
| file_reads | `index_join` | 9.10ms | 8.36ms | 0.919× | 1.21% | PASS |
| file_reads | `index_join_scan` | 3.75ms | 4.28ms | 1.141× | 1.74% | PASS |
| file_reads | `types_table_scan` | 874.17ms | 998.59ms | 1.142× | 1.81% | PASS |
| file_reads | `table_scan` | 1.02s | 1.09s | 1.072× | 3.31% | PASS |
| file_reads | `oltp_read_only` | 193.23ms | 127.21ms | 0.658× | 0.92% | PASS |
| file_writes | `oltp_bulk_insert` | 218.24ms | 262.06ms | 1.201× | 14.12% | PASS |
| file_writes | `oltp_insert` | 49.74ms | 45.59ms | 0.917× | 42.41% | PASS |
| file_writes | `oltp_update_index` | 151.30ms | 167.07ms | 1.104× | 18.12% | PASS |
| file_writes | `oltp_update_non_index` | 130.08ms | 103.60ms | 0.796× | 23.07% | PASS |
| file_writes | `oltp_delete_insert` | 145.02ms | 119.73ms | 0.826× | 24.24% | PASS |
| file_writes | `oltp_write_only` | 110.68ms | 96.79ms | 0.875× | 23.81% | PASS |
| file_writes | `types_delete_insert` | 76.53ms | 75.77ms | 0.990× | 18.82% | PASS |
| file_writes | `oltp_read_write` | 166.12ms | 138.54ms | 0.834× | 18.96% | PASS |
| ac_reads | `oltp_point_select` | 44.52ms | 46.98ms | 1.055× | 1.36% | PASS |
| ac_reads | `oltp_range_select` | 11.89ms | 12.10ms | 1.018× | 1.22% | PASS |
| ac_reads | `oltp_sum_range` | 10.93ms | 11.71ms | 1.071× | 1.46% | PASS |
| ac_reads | `oltp_order_range` | 2.59ms | 2.58ms | 0.994× | 0.86% | PASS |
| ac_reads | `oltp_distinct_range` | 3.35ms | 3.36ms | 1.002× | 0.74% | PASS |
| ac_reads | `oltp_index_scan` | 6.07ms | 6.92ms | 1.140× | 1.09% | PASS |
| ac_reads | `select_random_points` | 12.23ms | 12.06ms | 0.986× | 2.36% | PASS |
| ac_reads | `select_random_ranges` | 5.23ms | 5.76ms | 1.102× | 1.03% | PASS |
| ac_reads | `covering_index_scan` | 6.53ms | 6.31ms | 0.966× | 1.10% | PASS |
| ac_reads | `groupby_scan` | 26.22ms | 27.85ms | 1.062× | 0.83% | PASS |
| ac_reads | `index_join` | 6.72ms | 8.69ms | 1.292× | 1.59% | PASS |
| ac_reads | `index_join_scan` | 3.39ms | 4.43ms | 1.308× | 1.70% | PASS |
| ac_reads | `types_table_scan` | 952.56ms | 1.03s | 1.077× | 0.66% | PASS |
| ac_reads | `table_scan` | 1.12s | 1.12s | 0.994× | 0.61% | PASS |
| ac_reads | `oltp_read_only` | 126.07ms | 130.29ms | 1.033× | 0.72% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 30.55ms | 104.80ms | 3.431× | 62.80% | PASS |
| ac_writes | `oltp_insert_ac` | 29.50ms | 97.03ms | 3.289× | 55.05% | PASS |
| ac_writes | `oltp_update_index_ac` | 24.83ms | 113.68ms | 4.578× | 51.86% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.41ms | 91.49ms | 4.482× | 37.73% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 21.45ms | 117.61ms | 5.484× | 58.56% | PASS |
| ac_writes | `oltp_write_only_ac` | 56.21ms | 135.12ms | 2.404× | 76.28% | PASS |
| ac_writes | `types_delete_insert_ac` | 31.20ms | 91.96ms | 2.947× | 63.94% | PASS |
| ac_writes | `oltp_read_write_ac` | 44.78ms | 131.19ms | 2.930× | 68.17% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.47ms | 30.21ms | 1.141× | 0.74% | PASS |
| mem_reads | `oltp_range_select` | 12.42ms | 12.55ms | 1.011× | 0.83% | PASS |
| mem_reads | `oltp_sum_range` | 11.96ms | 12.08ms | 1.010× | 1.15% | PASS |
| mem_reads | `oltp_order_range` | 2.79ms | 2.94ms | 1.052× | 0.61% | PASS |
| mem_reads | `oltp_distinct_range` | 3.70ms | 3.85ms | 1.040× | 0.64% | PASS |
| mem_reads | `oltp_index_scan` | 4.09ms | 5.36ms | 1.310× | 1.39% | PASS |
| mem_reads | `select_random_points` | 16.45ms | 19.34ms | 1.176× | 0.67% | PASS |
| mem_reads | `select_random_ranges` | 3.44ms | 4.51ms | 1.312× | 0.79% | PASS |
| mem_reads | `covering_index_scan` | 3.93ms | 4.02ms | 1.024× | 1.13% | PASS |
| mem_reads | `groupby_scan` | 31.57ms | 31.54ms | 0.999× | 0.57% | PASS |
| mem_reads | `index_join` | 6.48ms | 8.76ms | 1.352× | 1.21% | PASS |
| mem_reads | `index_join_scan` | 4.26ms | 5.22ms | 1.224× | 0.88% | PASS |
| mem_reads | `types_table_scan` | 1.09s | 1.21s | 1.115× | 1.78% | PASS |
| mem_reads | `table_scan` | 1.35s | 1.32s | 0.977× | 3.76% | PASS |
| mem_reads | `oltp_read_only` | 109.60ms | 118.90ms | 1.085× | 1.15% | PASS |
| mem_writes | `oltp_bulk_insert` | 183.81ms | 281.03ms | 1.529× | 0.56% | PASS |
| mem_writes | `oltp_insert` | 19.03ms | 34.87ms | 1.833× | 1.18% | PASS |
| mem_writes | `oltp_update_index` | 63.31ms | 126.67ms | 2.001× | 0.99% | PASS |
| mem_writes | `oltp_update_non_index` | 42.39ms | 77.38ms | 1.826× | 0.90% | PASS |
| mem_writes | `oltp_delete_insert` | 44.20ms | 93.83ms | 2.123× | 0.57% | PASS |
| mem_writes | `oltp_write_only` | 25.21ms | 56.67ms | 2.248× | 1.26% | PASS |
| mem_writes | `types_delete_insert` | 27.57ms | 49.81ms | 1.807× | 1.25% | PASS |
| mem_writes | `oltp_read_write` | 74.74ms | 122.65ms | 1.641× | 1.15% | PASS |
| file_reads | `oltp_point_select` | 55.50ms | 39.55ms | 0.713× | 0.53% | PASS |
| file_reads | `oltp_range_select` | 16.08ms | 13.88ms | 0.863× | 1.23% | PASS |
| file_reads | `oltp_sum_range` | 15.36ms | 13.27ms | 0.864× | 0.89% | PASS |
| file_reads | `oltp_order_range` | 3.27ms | 3.08ms | 0.943× | 1.11% | PASS |
| file_reads | `oltp_distinct_range` | 4.18ms | 3.98ms | 0.953× | 1.08% | PASS |
| file_reads | `oltp_index_scan` | 7.53ms | 6.51ms | 0.864× | 0.55% | PASS |
| file_reads | `select_random_points` | 20.39ms | 20.49ms | 1.005× | 0.75% | PASS |
| file_reads | `select_random_ranges` | 6.65ms | 5.57ms | 0.838× | 1.00% | PASS |
| file_reads | `covering_index_scan` | 7.88ms | 5.14ms | 0.652× | 1.05% | PASS |
| file_reads | `groupby_scan` | 32.43ms | 31.75ms | 0.979× | 0.65% | PASS |
| file_reads | `index_join` | 9.05ms | 9.89ms | 1.093× | 0.78% | PASS |
| file_reads | `index_join_scan` | 4.87ms | 5.67ms | 1.165× | 1.48% | PASS |
| file_reads | `types_table_scan` | 1.07s | 1.20s | 1.122× | 1.03% | PASS |
| file_reads | `table_scan` | 1.35s | 1.33s | 0.982× | 2.58% | PASS |
| file_reads | `oltp_read_only` | 157.19ms | 134.92ms | 0.858× | 0.86% | PASS |
| file_writes | `oltp_bulk_insert` | 257.72ms | 372.18ms | 1.444× | 2.98% | PASS |
| file_writes | `oltp_insert` | 67.24ms | 69.28ms | 1.030× | 4.55% | PASS |
| file_writes | `oltp_update_index` | 236.00ms | 238.60ms | 1.011× | 1.78% | PASS |
| file_writes | `oltp_update_non_index` | 169.15ms | 157.22ms | 0.929× | 1.33% | PASS |
| file_writes | `oltp_delete_insert` | 192.03ms | 183.81ms | 0.957× | 0.73% | PASS |
| file_writes | `oltp_write_only` | 144.18ms | 129.52ms | 0.898× | 1.70% | PASS |
| file_writes | `types_delete_insert` | 115.27ms | 104.72ms | 0.908× | 2.53% | PASS |
| file_writes | `oltp_read_write` | 193.86ms | 194.44ms | 1.003× | 1.66% | PASS |
| ac_reads | `oltp_point_select` | 36.02ms | 39.47ms | 1.096× | 0.74% | PASS |
| ac_reads | `oltp_range_select` | 13.89ms | 13.80ms | 0.993× | 0.75% | PASS |
| ac_reads | `oltp_sum_range` | 13.33ms | 13.26ms | 0.995× | 0.96% | PASS |
| ac_reads | `oltp_order_range` | 3.06ms | 3.08ms | 1.007× | 0.79% | PASS |
| ac_reads | `oltp_distinct_range` | 3.99ms | 4.01ms | 1.005× | 1.66% | PASS |
| ac_reads | `oltp_index_scan` | 5.89ms | 6.71ms | 1.137× | 1.69% | PASS |
| ac_reads | `select_random_points` | 18.73ms | 20.73ms | 1.107× | 1.17% | PASS |
| ac_reads | `select_random_ranges` | 4.78ms | 5.60ms | 1.171× | 1.29% | PASS |
| ac_reads | `covering_index_scan` | 5.88ms | 5.15ms | 0.876× | 1.34% | PASS |
| ac_reads | `groupby_scan` | 31.95ms | 31.67ms | 0.991× | 0.37% | PASS |
| ac_reads | `index_join` | 8.17ms | 9.91ms | 1.212× | 0.48% | PASS |
| ac_reads | `index_join_scan` | 4.73ms | 5.64ms | 1.194× | 1.34% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.20s | 1.135× | 1.06% | PASS |
| ac_reads | `table_scan` | 1.26s | 1.30s | 1.033× | 1.88% | PASS |
| ac_reads | `oltp_read_only` | 127.05ms | 133.82ms | 1.053× | 1.21% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 27.40ms | 72.48ms | 2.645× | 2.65% | PASS |
| ac_writes | `oltp_insert_ac` | 30.00ms | 84.84ms | 2.828× | 2.44% | PASS |
| ac_writes | `oltp_update_index_ac` | 31.12ms | 100.97ms | 3.244× | 3.81% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 27.25ms | 81.22ms | 2.980× | 2.37% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 29.12ms | 92.55ms | 3.178× | 3.18% | PASS |
| ac_writes | `oltp_write_only_ac` | 29.38ms | 92.07ms | 3.133× | 3.94% | PASS |
| ac_writes | `types_delete_insert_ac` | 26.75ms | 83.09ms | 3.107× | 5.29% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.87ms | 99.39ms | 2.935× | 3.39% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 18.50ms | 21.63ms | 1.169× | 2.98% | PASS |
| mem_reads | `oltp_range_select` | 7.65ms | 8.31ms | 1.087× | 3.67% | PASS |
| mem_reads | `oltp_sum_range` | 7.24ms | 8.37ms | 1.156× | 4.66% | PASS |
| mem_reads | `oltp_order_range` | 1.90ms | 1.99ms | 1.050× | 5.90% | PASS |
| mem_reads | `oltp_distinct_range` | 2.64ms | 2.54ms | 0.962× | 2.97% | PASS |
| mem_reads | `oltp_index_scan` | 3.06ms | 4.27ms | 1.392× | 2.70% | PASS |
| mem_reads | `select_random_points` | 12.81ms | 13.98ms | 1.091× | 3.34% | PASS |
| mem_reads | `select_random_ranges` | 3.01ms | 3.54ms | 1.176× | 4.02% | PASS |
| mem_reads | `covering_index_scan` | 2.56ms | 3.07ms | 1.200× | 4.97% | PASS |
| mem_reads | `groupby_scan` | 20.46ms | 20.22ms | 0.988× | 2.44% | PASS |
| mem_reads | `index_join` | 4.77ms | 7.68ms | 1.611× | 5.66% | PASS |
| mem_reads | `index_join_scan` | 3.77ms | 5.20ms | 1.378× | 4.49% | PASS |
| mem_reads | `types_table_scan` | 729.72ms | 770.82ms | 1.056× | 2.68% | PASS |
| mem_reads | `table_scan` | 883.63ms | 873.07ms | 0.988× | 4.54% | PASS |
| mem_reads | `oltp_read_only` | 72.15ms | 75.71ms | 1.049× | 3.45% | PASS |
| mem_writes | `oltp_bulk_insert` | 144.44ms | 215.82ms | 1.494× | 1.78% | PASS |
| mem_writes | `oltp_insert` | 12.36ms | 26.23ms | 2.123× | 1.69% | PASS |
| mem_writes | `oltp_update_index` | 45.76ms | 96.16ms | 2.102× | 4.24% | PASS |
| mem_writes | `oltp_update_non_index` | 36.43ms | 62.98ms | 1.729× | 1.87% | PASS |
| mem_writes | `oltp_delete_insert` | 34.66ms | 75.94ms | 2.191× | 3.58% | PASS |
| mem_writes | `oltp_write_only` | 19.64ms | 45.75ms | 2.329× | 2.85% | PASS |
| mem_writes | `types_delete_insert` | 22.70ms | 39.33ms | 1.733× | 2.96% | PASS |
| mem_writes | `oltp_read_write` | 61.59ms | 91.91ms | 1.492× | 3.34% | PASS |
| file_reads | `oltp_point_select` | 79.42ms | 43.71ms | 0.550× | 1.49% | PASS |
| file_reads | `oltp_range_select` | 14.80ms | 10.30ms | 0.696× | 1.84% | PASS |
| file_reads | `oltp_sum_range` | 14.21ms | 10.04ms | 0.706× | 1.58% | PASS |
| file_reads | `oltp_order_range` | 2.61ms | 2.13ms | 0.818× | 1.24% | PASS |
| file_reads | `oltp_distinct_range` | 3.28ms | 2.71ms | 0.826× | 1.49% | PASS |
| file_reads | `oltp_index_scan` | 9.27ms | 6.24ms | 0.673× | 1.97% | PASS |
| file_reads | `select_random_points` | 19.03ms | 15.55ms | 0.817× | 2.32% | PASS |
| file_reads | `select_random_ranges` | 8.88ms | 5.48ms | 0.617× | 1.25% | PASS |
| file_reads | `covering_index_scan` | 9.48ms | 5.25ms | 0.553× | 2.00% | PASS |
| file_reads | `groupby_scan` | 20.71ms | 19.95ms | 0.963× | 1.96% | PASS |
| file_reads | `index_join` | 8.50ms | 7.88ms | 0.927× | 2.92% | PASS |
| file_reads | `index_join_scan` | 3.99ms | 4.48ms | 1.121× | 2.52% | PASS |
| file_reads | `types_table_scan` | 725.08ms | 771.72ms | 1.064× | 2.14% | PASS |
| file_reads | `table_scan` | 896.77ms | 879.99ms | 0.981× | 7.32% | PASS |
| file_reads | `oltp_read_only` | 163.30ms | 106.69ms | 0.653× | 2.90% | PASS |
| file_writes | `oltp_bulk_insert` | 196.24ms | 279.63ms | 1.425× | 4.65% | PASS |
| file_writes | `oltp_insert` | 30.13ms | 48.81ms | 1.620× | 6.83% | PASS |
| file_writes | `oltp_update_index` | 171.82ms | 188.78ms | 1.099× | 4.20% | PASS |
| file_writes | `oltp_update_non_index` | 131.53ms | 129.06ms | 0.981× | 6.56% | PASS |
| file_writes | `oltp_delete_insert` | 141.79ms | 145.47ms | 1.026× | 6.63% | PASS |
| file_writes | `oltp_write_only` | 101.89ms | 109.88ms | 1.078× | 9.63% | PASS |
| file_writes | `types_delete_insert` | 90.51ms | 86.38ms | 0.954× | 5.61% | PASS |
| file_writes | `oltp_read_write` | 148.52ms | 155.50ms | 1.047× | 5.70% | PASS |
| ac_reads | `oltp_point_select` | 41.92ms | 45.02ms | 1.074× | 1.75% | PASS |
| ac_reads | `oltp_range_select` | 11.94ms | 11.13ms | 0.933× | 2.07% | PASS |
| ac_reads | `oltp_sum_range` | 12.32ms | 11.18ms | 0.908× | 2.48% | PASS |
| ac_reads | `oltp_order_range` | 2.48ms | 2.29ms | 0.923× | 1.09% | PASS |
| ac_reads | `oltp_distinct_range` | 3.04ms | 2.77ms | 0.910× | 1.36% | PASS |
| ac_reads | `oltp_index_scan` | 5.83ms | 6.54ms | 1.122× | 1.32% | PASS |
| ac_reads | `select_random_points` | 16.74ms | 16.91ms | 1.010× | 4.38% | PASS |
| ac_reads | `select_random_ranges` | 5.38ms | 5.73ms | 1.065× | 1.96% | PASS |
| ac_reads | `covering_index_scan` | 6.01ms | 5.49ms | 0.912× | 1.90% | PASS |
| ac_reads | `groupby_scan` | 21.18ms | 20.55ms | 0.970× | 1.57% | PASS |
| ac_reads | `index_join` | 6.85ms | 8.05ms | 1.175× | 2.91% | PASS |
| ac_reads | `index_join_scan` | 3.79ms | 4.64ms | 1.225× | 3.37% | PASS |
| ac_reads | `types_table_scan` | 732.73ms | 778.07ms | 1.062× | 2.39% | PASS |
| ac_reads | `table_scan` | 963.94ms | 905.80ms | 0.940× | 5.40% | PASS |
| ac_reads | `oltp_read_only` | 110.96ms | 109.53ms | 0.987× | 4.81% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.40ms | 58.49ms | 2.499× | 6.31% | PASS |
| ac_writes | `oltp_insert_ac` | 27.86ms | 76.37ms | 2.742× | 5.49% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.22ms | 84.70ms | 2.899× | 5.31% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.51ms | 67.65ms | 3.005× | 5.00% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.79ms | 80.61ms | 3.009× | 7.72% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.23ms | 78.36ms | 2.987× | 8.38% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.55ms | 67.25ms | 3.121× | 6.85% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.42ms | 81.88ms | 2.783× | 3.72% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.53ms | 38.83ms | 1.124× | 1.84% | PASS |
| mem_reads | `oltp_range_select` | 20.01ms | 20.06ms | 1.003× | 1.24% | PASS |
| mem_reads | `oltp_sum_range` | 19.02ms | 19.51ms | 1.026× | 1.15% | PASS |
| mem_reads | `oltp_order_range` | 3.89ms | 3.85ms | 0.989× | 1.23% | PASS |
| mem_reads | `oltp_distinct_range` | 4.88ms | 4.88ms | 0.999× | 0.95% | PASS |
| mem_reads | `oltp_index_scan` | 4.87ms | 6.21ms | 1.276× | 2.64% | PASS |
| mem_reads | `select_random_points` | 28.01ms | 30.61ms | 1.093× | 1.46% | PASS |
| mem_reads | `select_random_ranges` | 7.66ms | 8.36ms | 1.092× | 1.20% | PASS |
| mem_reads | `covering_index_scan` | 4.39ms | 4.19ms | 0.955× | 1.93% | PASS |
| mem_reads | `groupby_scan` | 38.99ms | 40.17ms | 1.030× | 0.81% | PASS |
| mem_reads | `index_join` | 8.20ms | 10.21ms | 1.245× | 1.29% | PASS |
| mem_reads | `index_join_scan` | 4.33ms | 5.86ms | 1.354× | 2.11% | PASS |
| mem_reads | `types_table_scan` | 1.13s | 1.25s | 1.107× | 0.93% | PASS |
| mem_reads | `table_scan` | 1.39s | 1.38s | 0.993× | 3.17% | PASS |
| mem_reads | `oltp_read_only` | 155.28ms | 161.03ms | 1.037× | 1.58% | PASS |
| mem_writes | `oltp_bulk_insert` | 246.02ms | 354.54ms | 1.441× | 1.01% | PASS |
| mem_writes | `oltp_insert` | 19.94ms | 40.06ms | 2.009× | 0.87% | PASS |
| mem_writes | `oltp_update_index` | 74.83ms | 138.78ms | 1.855× | 1.76% | PASS |
| mem_writes | `oltp_update_non_index` | 57.61ms | 98.63ms | 1.712× | 1.33% | PASS |
| mem_writes | `oltp_delete_insert` | 55.92ms | 114.16ms | 2.042× | 1.69% | PASS |
| mem_writes | `oltp_write_only` | 30.17ms | 70.59ms | 2.340× | 1.62% | PASS |
| mem_writes | `types_delete_insert` | 34.88ms | 60.26ms | 1.727× | 2.51% | PASS |
| mem_writes | `oltp_read_write` | 108.38ms | 161.62ms | 1.491× | 3.32% | PASS |
| file_reads | `oltp_point_select` | 124.92ms | 69.50ms | 0.556× | 0.73% | PASS |
| file_reads | `oltp_range_select` | 29.93ms | 23.42ms | 0.783× | 2.16% | PASS |
| file_reads | `oltp_sum_range` | 28.58ms | 22.72ms | 0.795× | 2.32% | PASS |
| file_reads | `oltp_order_range` | 4.73ms | 4.17ms | 0.881× | 2.47% | PASS |
| file_reads | `oltp_distinct_range` | 5.89ms | 5.27ms | 0.896× | 1.10% | PASS |
| file_reads | `oltp_index_scan` | 14.24ms | 9.49ms | 0.666× | 1.38% | PASS |
| file_reads | `select_random_points` | 38.57ms | 34.67ms | 0.899× | 2.55% | PASS |
| file_reads | `select_random_ranges` | 16.91ms | 11.62ms | 0.687× | 0.99% | PASS |
| file_reads | `covering_index_scan` | 13.75ms | 7.75ms | 0.564× | 1.44% | PASS |
| file_reads | `groupby_scan` | 39.47ms | 40.54ms | 1.027× | 0.93% | PASS |
| file_reads | `index_join` | 13.20ms | 12.97ms | 0.983× | 1.81% | PASS |
| file_reads | `index_join_scan` | 5.40ms | 6.41ms | 1.188× | 2.71% | PASS |
| file_reads | `types_table_scan` | 1.20s | 1.27s | 1.065× | 2.78% | PASS |
| file_reads | `table_scan` | 1.44s | 1.39s | 0.963× | 3.26% | PASS |
| file_reads | `oltp_read_only` | 286.33ms | 207.03ms | 0.723× | 0.76% | PASS |
| file_writes | `oltp_bulk_insert` | 263.39ms | 380.48ms | 1.445× | 0.89% | PASS |
| file_writes | `oltp_insert` | 26.73ms | 51.04ms | 1.910× | 1.84% | PASS |
| file_writes | `oltp_update_index` | 109.61ms | 171.63ms | 1.566× | 2.11% | PASS |
| file_writes | `oltp_update_non_index` | 84.36ms | 121.48ms | 1.440× | 1.82% | PASS |
| file_writes | `oltp_delete_insert` | 83.44ms | 137.97ms | 1.654× | 1.44% | PASS |
| file_writes | `oltp_write_only` | 54.98ms | 92.47ms | 1.682× | 1.60% | PASS |
| file_writes | `types_delete_insert` | 53.33ms | 74.76ms | 1.402× | 1.74% | PASS |
| file_writes | `oltp_read_write` | 133.82ms | 184.86ms | 1.381× | 1.19% | PASS |
| ac_reads | `oltp_point_select` | 65.87ms | 69.66ms | 1.058× | 1.31% | PASS |
| ac_reads | `oltp_range_select` | 24.90ms | 23.82ms | 0.957× | 1.77% | PASS |
| ac_reads | `oltp_sum_range` | 22.77ms | 22.98ms | 1.009× | 1.10% | PASS |
| ac_reads | `oltp_order_range` | 4.34ms | 4.18ms | 0.964× | 0.89% | PASS |
| ac_reads | `oltp_distinct_range` | 5.38ms | 5.27ms | 0.980× | 0.76% | PASS |
| ac_reads | `oltp_index_scan` | 8.36ms | 9.46ms | 1.131× | 1.40% | PASS |
| ac_reads | `select_random_points` | 33.23ms | 35.31ms | 1.063× | 2.06% | PASS |
| ac_reads | `select_random_ranges` | 11.19ms | 11.70ms | 1.045× | 0.86% | PASS |
| ac_reads | `covering_index_scan` | 7.99ms | 7.78ms | 0.973× | 0.67% | PASS |
| ac_reads | `groupby_scan` | 39.37ms | 40.74ms | 1.035× | 0.74% | PASS |
| ac_reads | `index_join` | 10.62ms | 13.17ms | 1.240× | 1.59% | PASS |
| ac_reads | `index_join_scan` | 5.13ms | 6.63ms | 1.291× | 2.97% | PASS |
| ac_reads | `types_table_scan` | 1.25s | 1.30s | 1.041× | 1.79% | PASS |
| ac_reads | `table_scan` | 1.56s | 1.42s | 0.906× | 1.79% | PASS |
| ac_reads | `oltp_read_only` | 208.10ms | 209.95ms | 1.009× | 0.92% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.85ms | 53.02ms | 3.147× | 4.87% | PASS |
| ac_writes | `oltp_insert_ac` | 18.71ms | 75.31ms | 4.024× | 5.99% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.10ms | 89.52ms | 4.453× | 4.31% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.47ms | 66.44ms | 3.803× | 5.53% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.75ms | 79.19ms | 4.462× | 2.72% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.13ms | 77.15ms | 4.256× | 4.01% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.32ms | 66.78ms | 4.092× | 8.03% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.11ms | 87.25ms | 3.342× | 4.69% | PASS |

</details>

## Version-control latency

Wall time: 1m 35s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 52.02ms | 200.00ms | 26.0% | 0.91% | PASS |
| `status_dirty_many_tables` | 54.09ms | 200.00ms | 27.0% | 1.34% | PASS |
| `diff_regular_working_one_table` | 49.28ms | 150.00ms | 32.9% | 0.85% | PASS |
| `diff_regular_working_many_tables` | 56.32ms | 200.00ms | 28.2% | 1.00% | PASS |
| `diff_stat_working_many_tables` | 56.33ms | 200.00ms | 28.2% | 1.11% | PASS |
| `diff_schema_working_many_tables` | 56.54ms | 200.00ms | 28.3% | 0.82% | PASS |
| `branch_list_many_branches` | 15.00ms | 100.00ms | 15.0% | 1.48% | PASS |
| `branch_create_delete` | 16.73ms | 100.00ms | 16.7% | 3.03% | PASS |
| `checkout_branch_clean` | 72.09ms | 200.00ms | 36.0% | 1.30% | PASS |
| `merge_data_no_conflicts` | 24.95ms | 150.00ms | 16.6% | 1.24% | PASS |
| `merge_schema_no_conflicts` | 14.66ms | 100.00ms | 14.7% | 2.02% | PASS |
| `merge_data_conflicts` | 71.32ms | 250.00ms | 28.5% | 1.36% | PASS |
| `merge_data_conflicts_with_resolve` | 71.15ms | 250.00ms | 28.5% | 1.09% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
