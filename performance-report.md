# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-14 16:11 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261004.327.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/38582182807)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 16m 58s | 8.90s | 10.59s | 1.190× | 1.66% | **PASS** |
| textpk | 69 | 55 | 1h 18m 43s | 8.27s | 9.65s | 1.168× | 1.96% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 36s | 10.10s | 11.54s | 1.142× | 1.58% | **PASS** |
| compositepk | 69 | 55 | 1h 25m 54s | 10.21s | 11.67s | 1.142× | 1.03% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.19ms | 22.37ms | 1.108× | 1.64% | PASS |
| mem_reads | `oltp_range_select` | 9.11ms | 9.63ms | 1.058× | 2.20% | PASS |
| mem_reads | `oltp_sum_range` | 8.19ms | 9.19ms | 1.122× | 1.14% | PASS |
| mem_reads | `oltp_order_range` | 2.19ms | 2.31ms | 1.052× | 0.89% | PASS |
| mem_reads | `oltp_distinct_range` | 3.05ms | 3.13ms | 1.028× | 1.07% | PASS |
| mem_reads | `oltp_index_scan` | 3.27ms | 4.34ms | 1.328× | 2.36% | PASS |
| mem_reads | `select_random_points` | 9.38ms | 9.54ms | 1.017× | 2.62% | PASS |
| mem_reads | `select_random_ranges` | 2.63ms | 3.28ms | 1.248× | 1.09% | PASS |
| mem_reads | `covering_index_scan` | 3.40ms | 3.45ms | 1.016× | 2.26% | PASS |
| mem_reads | `groupby_scan` | 25.44ms | 27.21ms | 1.070× | 0.87% | PASS |
| mem_reads | `index_join` | 4.77ms | 6.78ms | 1.422× | 2.12% | PASS |
| mem_reads | `index_join_scan` | 2.91ms | 3.98ms | 1.367× | 1.62% | PASS |
| mem_reads | `types_table_scan` | 929.83ms | 1.01s | 1.089× | 2.53% | PASS |
| mem_reads | `table_scan` | 1.11s | 1.11s | 0.995× | 1.24% | PASS |
| mem_reads | `oltp_read_only` | 85.74ms | 92.14ms | 1.075× | 1.66% | PASS |
| mem_writes | `oltp_bulk_insert` | 139.89ms | 186.36ms | 1.332× | 0.84% | PASS |
| mem_writes | `oltp_insert` | 12.58ms | 22.14ms | 1.760× | 1.04% | PASS |
| mem_writes | `oltp_update_index` | 45.61ms | 93.08ms | 2.041× | 1.09% | PASS |
| mem_writes | `oltp_update_non_index` | 31.77ms | 48.99ms | 1.542× | 1.49% | PASS |
| mem_writes | `oltp_delete_insert` | 40.58ms | 66.69ms | 1.644× | 1.61% | PASS |
| mem_writes | `oltp_write_only` | 19.97ms | 37.33ms | 1.870× | 1.82% | PASS |
| mem_writes | `types_delete_insert` | 21.32ms | 31.87ms | 1.495× | 1.58% | PASS |
| mem_writes | `oltp_read_write` | 54.58ms | 81.58ms | 1.495× | 2.94% | PASS |
| file_reads | `oltp_point_select` | 95.25ms | 47.28ms | 0.496× | 0.88% | PASS |
| file_reads | `oltp_range_select` | 17.53ms | 12.34ms | 0.704× | 2.33% | PASS |
| file_reads | `oltp_sum_range` | 16.36ms | 12.01ms | 0.735× | 1.55% | PASS |
| file_reads | `oltp_order_range` | 3.09ms | 2.61ms | 0.843× | 0.96% | PASS |
| file_reads | `oltp_distinct_range` | 3.90ms | 3.39ms | 0.871× | 0.87% | PASS |
| file_reads | `oltp_index_scan` | 11.15ms | 7.03ms | 0.631× | 0.89% | PASS |
| file_reads | `select_random_points` | 17.43ms | 12.25ms | 0.702× | 2.14% | PASS |
| file_reads | `select_random_ranges` | 10.18ms | 5.74ms | 0.564× | 0.81% | PASS |
| file_reads | `covering_index_scan` | 11.40ms | 6.15ms | 0.540× | 0.56% | PASS |
| file_reads | `groupby_scan` | 26.45ms | 27.67ms | 1.046× | 0.84% | PASS |
| file_reads | `index_join` | 9.18ms | 8.51ms | 0.927× | 1.30% | PASS |
| file_reads | `index_join_scan` | 3.76ms | 4.26ms | 1.134× | 1.20% | PASS |
| file_reads | `types_table_scan` | 932.94ms | 1.01s | 1.087× | 2.18% | PASS |
| file_reads | `table_scan` | 1.08s | 1.10s | 1.017× | 3.13% | PASS |
| file_reads | `oltp_read_only` | 194.42ms | 127.68ms | 0.657× | 1.10% | PASS |
| file_writes | `oltp_bulk_insert` | 206.67ms | 261.00ms | 1.263× | 23.96% | PASS |
| file_writes | `oltp_insert` | 26.71ms | 37.59ms | 1.407× | 29.27% | PASS |
| file_writes | `oltp_update_index` | 190.22ms | 160.96ms | 0.846× | 22.98% | PASS |
| file_writes | `oltp_update_non_index` | 151.16ms | 118.87ms | 0.786× | 32.96% | PASS |
| file_writes | `oltp_delete_insert` | 141.22ms | 118.67ms | 0.840× | 24.39% | PASS |
| file_writes | `oltp_write_only` | 141.93ms | 90.49ms | 0.638× | 37.92% | PASS |
| file_writes | `types_delete_insert` | 92.61ms | 72.82ms | 0.786× | 32.28% | PASS |
| file_writes | `oltp_read_write` | 189.06ms | 146.56ms | 0.775× | 24.33% | PASS |
| ac_reads | `oltp_point_select` | 45.89ms | 48.08ms | 1.048× | 1.12% | PASS |
| ac_reads | `oltp_range_select` | 12.22ms | 12.24ms | 1.002× | 1.65% | PASS |
| ac_reads | `oltp_sum_range` | 11.33ms | 12.05ms | 1.064× | 1.98% | PASS |
| ac_reads | `oltp_order_range` | 2.62ms | 2.61ms | 0.995× | 0.90% | PASS |
| ac_reads | `oltp_distinct_range` | 3.40ms | 3.39ms | 0.998× | 0.53% | PASS |
| ac_reads | `oltp_index_scan` | 6.20ms | 7.03ms | 1.134× | 1.23% | PASS |
| ac_reads | `select_random_points` | 12.77ms | 12.72ms | 0.996× | 2.27% | PASS |
| ac_reads | `select_random_ranges` | 5.25ms | 5.77ms | 1.100× | 1.11% | PASS |
| ac_reads | `covering_index_scan` | 6.34ms | 6.15ms | 0.970× | 0.95% | PASS |
| ac_reads | `groupby_scan` | 25.82ms | 27.53ms | 1.066× | 0.95% | PASS |
| ac_reads | `index_join` | 6.67ms | 8.57ms | 1.285× | 1.91% | PASS |
| ac_reads | `index_join_scan` | 3.35ms | 4.29ms | 1.279× | 1.10% | PASS |
| ac_reads | `types_table_scan` | 921.28ms | 1.01s | 1.098× | 2.50% | PASS |
| ac_reads | `table_scan` | 1.10s | 1.10s | 1.002× | 2.49% | PASS |
| ac_reads | `oltp_read_only` | 126.26ms | 130.14ms | 1.031× | 1.74% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 26.51ms | 136.77ms | 5.160× | 62.83% | PASS |
| ac_writes | `oltp_insert_ac` | 60.24ms | 214.50ms | 3.561× | 66.39% | PASS |
| ac_writes | `oltp_update_index_ac` | 70.85ms | 246.88ms | 3.485× | 80.32% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 50.28ms | 179.67ms | 3.573× | 72.64% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 41.68ms | 248.42ms | 5.961× | 70.07% | PASS |
| ac_writes | `oltp_write_only_ac` | 79.75ms | 323.98ms | 4.062× | 80.57% | PASS |
| ac_writes | `types_delete_insert_ac` | 42.48ms | 224.88ms | 5.293× | 73.37% | PASS |
| ac_writes | `oltp_read_write_ac` | 76.07ms | 341.11ms | 4.484× | 58.63% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.93ms | 26.29ms | 1.146× | 1.23% | PASS |
| mem_reads | `oltp_range_select` | 11.15ms | 11.13ms | 0.998× | 2.02% | PASS |
| mem_reads | `oltp_sum_range` | 10.50ms | 10.62ms | 1.011× | 2.16% | PASS |
| mem_reads | `oltp_order_range` | 2.49ms | 2.63ms | 1.057× | 1.92% | PASS |
| mem_reads | `oltp_distinct_range` | 3.29ms | 3.42ms | 1.040× | 1.21% | PASS |
| mem_reads | `oltp_index_scan` | 3.56ms | 4.52ms | 1.268× | 3.02% | PASS |
| mem_reads | `select_random_points` | 14.43ms | 16.98ms | 1.177× | 1.86% | PASS |
| mem_reads | `select_random_ranges` | 3.02ms | 3.90ms | 1.289× | 2.09% | PASS |
| mem_reads | `covering_index_scan` | 3.32ms | 3.19ms | 0.961× | 3.06% | PASS |
| mem_reads | `groupby_scan` | 27.31ms | 27.32ms | 1.000× | 1.64% | PASS |
| mem_reads | `index_join` | 5.65ms | 7.04ms | 1.245× | 1.06% | PASS |
| mem_reads | `index_join_scan` | 3.64ms | 4.50ms | 1.236× | 3.08% | PASS |
| mem_reads | `types_table_scan` | 886.23ms | 1.02s | 1.152× | 0.59% | PASS |
| mem_reads | `table_scan` | 1.04s | 1.12s | 1.077× | 0.68% | PASS |
| mem_reads | `oltp_read_only` | 92.47ms | 102.22ms | 1.106× | 0.81% | PASS |
| mem_writes | `oltp_bulk_insert` | 161.43ms | 241.06ms | 1.493× | 1.96% | PASS |
| mem_writes | `oltp_insert` | 16.22ms | 27.75ms | 1.712× | 1.93% | PASS |
| mem_writes | `oltp_update_index` | 54.29ms | 99.49ms | 1.832× | 1.51% | PASS |
| mem_writes | `oltp_update_non_index` | 36.06ms | 61.63ms | 1.709× | 0.96% | PASS |
| mem_writes | `oltp_delete_insert` | 37.98ms | 74.21ms | 1.954× | 1.16% | PASS |
| mem_writes | `oltp_write_only` | 21.34ms | 43.77ms | 2.052× | 1.60% | PASS |
| mem_writes | `types_delete_insert` | 23.78ms | 38.64ms | 1.625× | 1.32% | PASS |
| mem_writes | `oltp_read_write` | 61.94ms | 99.09ms | 1.600× | 1.30% | PASS |
| file_reads | `oltp_point_select` | 49.22ms | 35.51ms | 0.721× | 1.29% | PASS |
| file_reads | `oltp_range_select` | 14.12ms | 12.36ms | 0.875× | 2.14% | PASS |
| file_reads | `oltp_sum_range` | 13.46ms | 11.74ms | 0.873× | 1.19% | PASS |
| file_reads | `oltp_order_range` | 2.90ms | 2.81ms | 0.970× | 1.56% | PASS |
| file_reads | `oltp_distinct_range` | 3.68ms | 3.56ms | 0.968× | 1.15% | PASS |
| file_reads | `oltp_index_scan` | 6.41ms | 5.72ms | 0.892× | 2.30% | PASS |
| file_reads | `select_random_points` | 17.44ms | 18.13ms | 1.039× | 1.19% | PASS |
| file_reads | `select_random_ranges` | 5.88ms | 4.97ms | 0.845× | 1.50% | PASS |
| file_reads | `covering_index_scan` | 6.14ms | 4.16ms | 0.677× | 2.75% | PASS |
| file_reads | `groupby_scan` | 27.28ms | 27.25ms | 0.999× | 0.65% | PASS |
| file_reads | `index_join` | 7.28ms | 7.96ms | 1.092× | 2.09% | PASS |
| file_reads | `index_join_scan` | 4.28ms | 4.99ms | 1.166× | 1.33% | PASS |
| file_reads | `types_table_scan` | 886.16ms | 1.02s | 1.153× | 0.64% | PASS |
| file_reads | `table_scan` | 1.03s | 1.12s | 1.081× | 0.71% | PASS |
| file_reads | `oltp_read_only` | 130.16ms | 114.94ms | 0.883× | 1.42% | PASS |
| file_writes | `oltp_bulk_insert` | 219.46ms | 308.46ms | 1.406× | 3.49% | PASS |
| file_writes | `oltp_insert` | 60.46ms | 51.79ms | 0.857× | 11.99% | PASS |
| file_writes | `oltp_update_index` | 190.78ms | 184.99ms | 0.970× | 2.50% | PASS |
| file_writes | `oltp_update_non_index` | 145.91ms | 123.53ms | 0.847× | 3.30% | PASS |
| file_writes | `oltp_delete_insert` | 155.05ms | 149.26ms | 0.963× | 3.47% | PASS |
| file_writes | `oltp_write_only` | 114.76ms | 103.23ms | 0.899× | 3.51% | PASS |
| file_writes | `types_delete_insert` | 90.27ms | 77.93ms | 0.863× | 9.20% | PASS |
| file_writes | `oltp_read_write` | 152.15ms | 158.84ms | 1.044× | 4.44% | PASS |
| ac_reads | `oltp_point_select` | 31.20ms | 34.93ms | 1.119× | 1.49% | PASS |
| ac_reads | `oltp_range_select` | 12.09ms | 12.19ms | 1.009× | 1.97% | PASS |
| ac_reads | `oltp_sum_range` | 11.61ms | 11.74ms | 1.011× | 2.10% | PASS |
| ac_reads | `oltp_order_range` | 2.72ms | 2.78ms | 1.025× | 1.95% | PASS |
| ac_reads | `oltp_distinct_range` | 3.51ms | 3.54ms | 1.008× | 1.98% | PASS |
| ac_reads | `oltp_index_scan` | 4.90ms | 5.71ms | 1.165× | 2.76% | PASS |
| ac_reads | `select_random_points` | 16.12ms | 18.32ms | 1.136× | 1.98% | PASS |
| ac_reads | `select_random_ranges` | 4.35ms | 5.09ms | 1.171× | 1.96% | PASS |
| ac_reads | `covering_index_scan` | 5.29ms | 4.63ms | 0.875× | 3.06% | PASS |
| ac_reads | `groupby_scan` | 28.20ms | 27.84ms | 0.987× | 1.45% | PASS |
| ac_reads | `index_join` | 7.26ms | 8.79ms | 1.211× | 1.95% | PASS |
| ac_reads | `index_join_scan` | 4.15ms | 4.87ms | 1.172× | 2.13% | PASS |
| ac_reads | `types_table_scan` | 895.46ms | 1.03s | 1.152× | 0.79% | PASS |
| ac_reads | `table_scan` | 1.03s | 1.11s | 1.081× | 0.55% | PASS |
| ac_reads | `oltp_read_only` | 106.05ms | 116.57ms | 1.099× | 1.02% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 25.84ms | 67.46ms | 2.610× | 5.51% | PASS |
| ac_writes | `oltp_insert_ac` | 28.21ms | 80.88ms | 2.867× | 5.62% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.84ms | 89.24ms | 3.095× | 5.73% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.73ms | 75.41ms | 2.930× | 3.83% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.97ms | 83.51ms | 3.096× | 5.66% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.91ms | 82.90ms | 3.080× | 6.77% | PASS |
| ac_writes | `types_delete_insert_ac` | 25.10ms | 75.41ms | 3.004× | 6.33% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.17ms | 88.84ms | 2.850× | 6.18% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.03ms | 34.18ms | 1.138× | 1.30% | PASS |
| mem_reads | `oltp_range_select` | 13.11ms | 13.48ms | 1.028× | 1.32% | PASS |
| mem_reads | `oltp_sum_range` | 12.01ms | 12.93ms | 1.077× | 1.20% | PASS |
| mem_reads | `oltp_order_range` | 3.08ms | 3.12ms | 1.013× | 1.64% | PASS |
| mem_reads | `oltp_distinct_range` | 4.14ms | 4.12ms | 0.995× | 0.72% | PASS |
| mem_reads | `oltp_index_scan` | 4.59ms | 5.77ms | 1.255× | 1.75% | PASS |
| mem_reads | `select_random_points` | 17.18ms | 19.42ms | 1.130× | 1.06% | PASS |
| mem_reads | `select_random_ranges` | 4.11ms | 5.14ms | 1.251× | 1.26% | PASS |
| mem_reads | `covering_index_scan` | 4.57ms | 4.33ms | 0.946× | 1.74% | PASS |
| mem_reads | `groupby_scan` | 33.42ms | 35.07ms | 1.049× | 0.90% | PASS |
| mem_reads | `index_join` | 6.86ms | 8.67ms | 1.263× | 1.22% | PASS |
| mem_reads | `index_join_scan` | 4.41ms | 5.64ms | 1.279× | 1.93% | PASS |
| mem_reads | `types_table_scan` | 1.14s | 1.25s | 1.099× | 1.50% | PASS |
| mem_reads | `table_scan` | 1.38s | 1.38s | 1.001× | 4.15% | PASS |
| mem_reads | `oltp_read_only` | 129.72ms | 132.90ms | 1.025× | 1.48% | PASS |
| mem_writes | `oltp_bulk_insert` | 238.88ms | 335.77ms | 1.406× | 0.84% | PASS |
| mem_writes | `oltp_insert` | 21.33ms | 40.20ms | 1.884× | 0.96% | PASS |
| mem_writes | `oltp_update_index` | 74.50ms | 139.83ms | 1.877× | 1.97% | PASS |
| mem_writes | `oltp_update_non_index` | 51.99ms | 87.81ms | 1.689× | 1.44% | PASS |
| mem_writes | `oltp_delete_insert` | 52.35ms | 108.00ms | 2.063× | 1.64% | PASS |
| mem_writes | `oltp_write_only` | 28.25ms | 62.85ms | 2.225× | 1.19% | PASS |
| mem_writes | `types_delete_insert` | 32.33ms | 52.53ms | 1.625× | 0.78% | PASS |
| mem_writes | `oltp_read_write` | 82.31ms | 134.01ms | 1.628× | 1.64% | PASS |
| file_reads | `oltp_point_select` | 126.68ms | 66.76ms | 0.527× | 1.16% | PASS |
| file_reads | `oltp_range_select` | 23.57ms | 16.84ms | 0.715× | 2.11% | PASS |
| file_reads | `oltp_sum_range` | 22.32ms | 16.35ms | 0.732× | 2.45% | PASS |
| file_reads | `oltp_order_range` | 4.12ms | 3.54ms | 0.859× | 3.18% | PASS |
| file_reads | `oltp_distinct_range` | 5.19ms | 4.53ms | 0.873× | 1.58% | PASS |
| file_reads | `oltp_index_scan` | 14.57ms | 9.41ms | 0.646× | 1.39% | PASS |
| file_reads | `select_random_points` | 28.18ms | 23.06ms | 0.818× | 1.48% | PASS |
| file_reads | `select_random_ranges` | 14.08ms | 8.50ms | 0.604× | 1.11% | PASS |
| file_reads | `covering_index_scan` | 15.11ms | 7.99ms | 0.529× | 1.88% | PASS |
| file_reads | `groupby_scan` | 34.79ms | 35.53ms | 1.022× | 0.82% | PASS |
| file_reads | `index_join` | 12.66ms | 11.36ms | 0.897× | 2.17% | PASS |
| file_reads | `index_join_scan` | 5.59ms | 6.06ms | 1.085× | 1.64% | PASS |
| file_reads | `types_table_scan` | 1.12s | 1.24s | 1.101× | 0.62% | PASS |
| file_reads | `table_scan` | 1.33s | 1.35s | 1.013× | 1.91% | PASS |
| file_reads | `oltp_read_only` | 258.39ms | 173.37ms | 0.671× | 0.57% | PASS |
| file_writes | `oltp_bulk_insert` | 261.29ms | 359.81ms | 1.377× | 0.91% | PASS |
| file_writes | `oltp_insert` | 31.95ms | 52.49ms | 1.643× | 1.38% | PASS |
| file_writes | `oltp_update_index` | 109.71ms | 170.97ms | 1.558× | 2.29% | PASS |
| file_writes | `oltp_update_non_index` | 82.20ms | 110.25ms | 1.341× | 2.52% | PASS |
| file_writes | `oltp_delete_insert` | 83.22ms | 132.58ms | 1.593× | 1.29% | PASS |
| file_writes | `oltp_write_only` | 59.71ms | 90.51ms | 1.516× | 2.90% | PASS |
| file_writes | `types_delete_insert` | 53.71ms | 72.50ms | 1.350× | 1.79% | PASS |
| file_writes | `oltp_read_write` | 111.27ms | 156.17ms | 1.404× | 1.46% | PASS |
| ac_reads | `oltp_point_select` | 62.25ms | 67.07ms | 1.077× | 1.36% | PASS |
| ac_reads | `oltp_range_select` | 17.62ms | 16.88ms | 0.958× | 2.30% | PASS |
| ac_reads | `oltp_sum_range` | 16.56ms | 16.49ms | 0.996× | 2.18% | PASS |
| ac_reads | `oltp_order_range` | 3.60ms | 3.55ms | 0.986× | 1.07% | PASS |
| ac_reads | `oltp_distinct_range` | 4.54ms | 4.52ms | 0.996× | 0.76% | PASS |
| ac_reads | `oltp_index_scan` | 8.33ms | 9.46ms | 1.136× | 1.67% | PASS |
| ac_reads | `select_random_points` | 22.00ms | 23.36ms | 1.062× | 1.49% | PASS |
| ac_reads | `select_random_ranges` | 7.67ms | 8.53ms | 1.112× | 1.20% | PASS |
| ac_reads | `covering_index_scan` | 8.57ms | 8.00ms | 0.934× | 1.84% | PASS |
| ac_reads | `groupby_scan` | 34.45ms | 35.63ms | 1.034× | 0.81% | PASS |
| ac_reads | `index_join` | 9.27ms | 11.40ms | 1.230× | 1.64% | PASS |
| ac_reads | `index_join_scan` | 5.05ms | 6.19ms | 1.225× | 1.96% | PASS |
| ac_reads | `types_table_scan` | 1.12s | 1.23s | 1.102× | 0.52% | PASS |
| ac_reads | `table_scan` | 1.29s | 1.34s | 1.044× | 0.49% | PASS |
| ac_reads | `oltp_read_only` | 168.89ms | 175.18ms | 1.037× | 1.60% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.93ms | 52.69ms | 3.307× | 4.10% | PASS |
| ac_writes | `oltp_insert_ac` | 18.71ms | 75.99ms | 4.061× | 2.63% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.36ms | 87.01ms | 4.073× | 4.57% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.33ms | 65.98ms | 4.041× | 5.62% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.11ms | 77.29ms | 4.268× | 4.04% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.44ms | 76.25ms | 4.136× | 3.93% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.73ms | 68.37ms | 4.346× | 5.28% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.87ms | 84.03ms | 3.379× | 3.36% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.91ms | 37.92ms | 1.118× | 1.47% | PASS |
| mem_reads | `oltp_range_select` | 19.74ms | 19.93ms | 1.010× | 1.00% | PASS |
| mem_reads | `oltp_sum_range` | 18.58ms | 19.29ms | 1.038× | 0.88% | PASS |
| mem_reads | `oltp_order_range` | 3.73ms | 3.78ms | 1.013× | 0.96% | PASS |
| mem_reads | `oltp_distinct_range` | 4.78ms | 4.81ms | 1.008× | 0.94% | PASS |
| mem_reads | `oltp_index_scan` | 4.68ms | 5.82ms | 1.243× | 1.73% | PASS |
| mem_reads | `select_random_points` | 27.47ms | 30.41ms | 1.107× | 0.88% | PASS |
| mem_reads | `select_random_ranges` | 7.63ms | 8.34ms | 1.094× | 0.95% | PASS |
| mem_reads | `covering_index_scan` | 4.39ms | 4.41ms | 1.006× | 2.43% | PASS |
| mem_reads | `groupby_scan` | 38.64ms | 39.96ms | 1.034× | 0.68% | PASS |
| mem_reads | `index_join` | 8.07ms | 10.16ms | 1.258× | 1.33% | PASS |
| mem_reads | `index_join_scan` | 4.23ms | 5.62ms | 1.328× | 1.26% | PASS |
| mem_reads | `types_table_scan` | 1.15s | 1.25s | 1.091× | 1.49% | PASS |
| mem_reads | `table_scan` | 1.35s | 1.37s | 1.017× | 2.31% | PASS |
| mem_reads | `oltp_read_only` | 150.86ms | 158.43ms | 1.050× | 0.91% | PASS |
| mem_writes | `oltp_bulk_insert` | 245.57ms | 338.92ms | 1.380× | 1.09% | PASS |
| mem_writes | `oltp_insert` | 19.59ms | 36.52ms | 1.864× | 0.71% | PASS |
| mem_writes | `oltp_update_index` | 69.65ms | 121.27ms | 1.741× | 1.19% | PASS |
| mem_writes | `oltp_update_non_index` | 51.49ms | 84.21ms | 1.636× | 0.92% | PASS |
| mem_writes | `oltp_delete_insert` | 49.51ms | 95.87ms | 1.936× | 0.76% | PASS |
| mem_writes | `oltp_write_only` | 27.19ms | 58.82ms | 2.163× | 0.74% | PASS |
| mem_writes | `types_delete_insert` | 33.12ms | 54.54ms | 1.647× | 1.12% | PASS |
| mem_writes | `oltp_read_write` | 100.73ms | 148.85ms | 1.478× | 0.92% | PASS |
| file_reads | `oltp_point_select` | 130.17ms | 69.89ms | 0.537× | 1.14% | PASS |
| file_reads | `oltp_range_select` | 29.98ms | 23.34ms | 0.779× | 1.37% | PASS |
| file_reads | `oltp_sum_range` | 28.20ms | 22.46ms | 0.796× | 0.83% | PASS |
| file_reads | `oltp_order_range` | 4.75ms | 4.13ms | 0.869× | 1.03% | PASS |
| file_reads | `oltp_distinct_range` | 5.82ms | 5.22ms | 0.897× | 0.95% | PASS |
| file_reads | `oltp_index_scan` | 14.56ms | 9.31ms | 0.639× | 1.25% | PASS |
| file_reads | `select_random_points` | 37.54ms | 33.68ms | 0.897× | 0.85% | PASS |
| file_reads | `select_random_ranges` | 17.36ms | 11.65ms | 0.671× | 1.24% | PASS |
| file_reads | `covering_index_scan` | 14.42ms | 7.70ms | 0.534× | 1.39% | PASS |
| file_reads | `groupby_scan` | 39.79ms | 40.59ms | 1.020× | 0.65% | PASS |
| file_reads | `index_join` | 13.59ms | 12.64ms | 0.930× | 1.71% | PASS |
| file_reads | `index_join_scan` | 5.30ms | 6.00ms | 1.132× | 1.01% | PASS |
| file_reads | `types_table_scan` | 1.11s | 1.24s | 1.113× | 0.39% | PASS |
| file_reads | `table_scan` | 1.29s | 1.35s | 1.048× | 0.28% | PASS |
| file_reads | `oltp_read_only` | 288.67ms | 205.10ms | 0.711× | 0.55% | PASS |
| file_writes | `oltp_bulk_insert` | 261.32ms | 361.12ms | 1.382× | 1.09% | PASS |
| file_writes | `oltp_insert` | 26.15ms | 46.28ms | 1.770× | 1.19% | PASS |
| file_writes | `oltp_update_index` | 97.84ms | 143.18ms | 1.463× | 0.81% | PASS |
| file_writes | `oltp_update_non_index` | 77.55ms | 105.04ms | 1.354× | 1.31% | PASS |
| file_writes | `oltp_delete_insert` | 76.84ms | 119.67ms | 1.558× | 1.28% | PASS |
| file_writes | `oltp_write_only` | 50.62ms | 79.24ms | 1.566× | 1.94% | PASS |
| file_writes | `types_delete_insert` | 50.27ms | 66.77ms | 1.328× | 1.12% | PASS |
| file_writes | `oltp_read_write` | 121.87ms | 166.98ms | 1.370× | 0.98% | PASS |
| ac_reads | `oltp_point_select` | 64.43ms | 69.47ms | 1.078× | 0.74% | PASS |
| ac_reads | `oltp_range_select` | 22.61ms | 23.24ms | 1.028× | 0.89% | PASS |
| ac_reads | `oltp_sum_range` | 21.23ms | 22.41ms | 1.056× | 0.81% | PASS |
| ac_reads | `oltp_order_range` | 4.11ms | 4.10ms | 1.000× | 1.19% | PASS |
| ac_reads | `oltp_distinct_range` | 5.13ms | 5.20ms | 1.014× | 0.93% | PASS |
| ac_reads | `oltp_index_scan` | 8.02ms | 9.24ms | 1.151× | 0.84% | PASS |
| ac_reads | `select_random_points` | 30.96ms | 33.69ms | 1.088× | 1.34% | PASS |
| ac_reads | `select_random_ranges` | 10.97ms | 11.71ms | 1.068× | 1.02% | PASS |
| ac_reads | `covering_index_scan` | 7.77ms | 7.63ms | 0.982× | 0.82% | PASS |
| ac_reads | `groupby_scan` | 38.64ms | 40.45ms | 1.047× | 0.66% | PASS |
| ac_reads | `index_join` | 10.18ms | 12.70ms | 1.248× | 1.00% | PASS |
| ac_reads | `index_join_scan` | 4.69ms | 6.10ms | 1.300× | 1.20% | PASS |
| ac_reads | `types_table_scan` | 1.12s | 1.24s | 1.112× | 0.69% | PASS |
| ac_reads | `table_scan` | 1.31s | 1.36s | 1.036× | 1.48% | PASS |
| ac_reads | `oltp_read_only` | 196.07ms | 205.14ms | 1.046× | 0.94% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.66ms | 51.66ms | 3.299× | 3.51% | PASS |
| ac_writes | `oltp_insert_ac` | 17.78ms | 73.09ms | 4.110× | 3.18% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.34ms | 84.58ms | 4.372× | 2.27% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 15.98ms | 63.26ms | 3.959× | 2.62% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.36ms | 75.54ms | 4.350× | 2.72% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.03ms | 74.80ms | 4.150× | 3.04% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.48ms | 63.73ms | 4.117× | 3.66% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.85ms | 84.12ms | 3.385× | 3.16% | PASS |

</details>

## Version-control latency

Wall time: 1m 50s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 60.59ms | 200.00ms | 30.3% | 1.04% | PASS |
| `status_dirty_many_tables` | 62.16ms | 200.00ms | 31.1% | 1.51% | PASS |
| `diff_regular_working_one_table` | 55.90ms | 150.00ms | 37.3% | 0.67% | PASS |
| `diff_regular_working_many_tables` | 63.89ms | 200.00ms | 31.9% | 0.97% | PASS |
| `diff_stat_working_many_tables` | 63.50ms | 200.00ms | 31.8% | 1.02% | PASS |
| `diff_schema_working_many_tables` | 64.11ms | 200.00ms | 32.1% | 1.10% | PASS |
| `branch_list_many_branches` | 18.24ms | 100.00ms | 18.2% | 2.83% | PASS |
| `branch_create_delete` | 20.77ms | 100.00ms | 20.8% | 1.77% | PASS |
| `checkout_branch_clean` | 78.22ms | 200.00ms | 39.1% | 1.68% | PASS |
| `merge_data_no_conflicts` | 31.25ms | 150.00ms | 20.8% | 1.60% | PASS |
| `merge_schema_no_conflicts` | 18.37ms | 100.00ms | 18.4% | 2.35% | PASS |
| `merge_data_conflicts` | 81.54ms | 250.00ms | 32.6% | 0.91% | PASS |
| `merge_data_conflicts_with_resolve` | 82.60ms | 250.00ms | 33.0% | 1.01% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
