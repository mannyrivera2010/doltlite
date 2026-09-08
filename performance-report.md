# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-08 15:24 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260831.293.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/34234131574)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 4s | 8.97s | 11.58s | 1.292× | 1.60% | **PASS** |
| textpk | 69 | 55 | 1h 34m 32s | 11.62s | 12.08s | 1.039× | 1.77% | **PASS** |
| blobpk | 69 | 55 | 1h 32m 10s | 9.99s | 12.00s | 1.201× | 1.69% | **PASS** |
| compositepk | 69 | 55 | 1h 26m 40s | 10.34s | 11.83s | 1.144× | 1.13% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.56ms | 29.91ms | 1.218× | 1.66% | PASS |
| mem_reads | `oltp_range_select` | 10.24ms | 13.18ms | 1.287× | 1.80% | PASS |
| mem_reads | `oltp_sum_range` | 9.61ms | 12.41ms | 1.292× | 1.60% | PASS |
| mem_reads | `oltp_order_range` | 2.57ms | 3.00ms | 1.167× | 1.04% | PASS |
| mem_reads | `oltp_distinct_range` | 3.67ms | 4.06ms | 1.107× | 1.48% | PASS |
| mem_reads | `oltp_index_scan` | 3.99ms | 5.38ms | 1.350× | 1.44% | PASS |
| mem_reads | `select_random_points` | 10.34ms | 11.32ms | 1.094× | 3.09% | PASS |
| mem_reads | `select_random_ranges` | 3.05ms | 4.03ms | 1.320× | 1.68% | PASS |
| mem_reads | `covering_index_scan` | 4.26ms | 4.34ms | 1.019× | 3.38% | PASS |
| mem_reads | `groupby_scan` | 29.97ms | 32.85ms | 1.096× | 1.34% | PASS |
| mem_reads | `index_join` | 6.09ms | 8.19ms | 1.345× | 2.16% | PASS |
| mem_reads | `index_join_scan` | 3.53ms | 4.62ms | 1.309× | 1.82% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.34s | 1.281× | 0.74% | PASS |
| mem_reads | `table_scan` | 1.19s | 1.40s | 1.179× | 1.25% | PASS |
| mem_reads | `oltp_read_only` | 102.26ms | 123.88ms | 1.211× | 1.62% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.74ms | 252.77ms | 1.399× | 1.23% | PASS |
| mem_writes | `oltp_insert` | 15.54ms | 28.61ms | 1.841× | 1.02% | PASS |
| mem_writes | `oltp_update_index` | 50.00ms | 104.97ms | 2.099× | 1.42% | PASS |
| mem_writes | `oltp_update_non_index` | 33.78ms | 59.61ms | 1.765× | 1.74% | PASS |
| mem_writes | `oltp_delete_insert` | 45.28ms | 79.02ms | 1.745× | 1.51% | PASS |
| mem_writes | `oltp_write_only` | 21.92ms | 44.92ms | 2.049× | 1.37% | PASS |
| mem_writes | `types_delete_insert` | 24.50ms | 40.48ms | 1.652× | 1.07% | PASS |
| mem_writes | `oltp_read_write` | 67.07ms | 110.63ms | 1.650× | 1.68% | PASS |
| file_reads | `oltp_point_select` | 98.95ms | 55.03ms | 0.556× | 0.91% | PASS |
| file_reads | `oltp_range_select` | 17.68ms | 15.83ms | 0.895× | 1.86% | PASS |
| file_reads | `oltp_sum_range` | 17.44ms | 15.10ms | 0.866× | 1.54% | PASS |
| file_reads | `oltp_order_range` | 3.38ms | 3.32ms | 0.980× | 1.89% | PASS |
| file_reads | `oltp_distinct_range` | 4.49ms | 4.39ms | 0.979× | 1.54% | PASS |
| file_reads | `oltp_index_scan` | 11.62ms | 8.24ms | 0.709× | 1.02% | PASS |
| file_reads | `select_random_points` | 17.66ms | 13.88ms | 0.786× | 2.31% | PASS |
| file_reads | `select_random_ranges` | 10.43ms | 6.60ms | 0.633× | 1.33% | PASS |
| file_reads | `covering_index_scan` | 11.89ms | 7.09ms | 0.596× | 1.46% | PASS |
| file_reads | `groupby_scan` | 30.78ms | 33.22ms | 1.079× | 1.09% | PASS |
| file_reads | `index_join` | 10.35ms | 10.16ms | 0.982× | 1.62% | PASS |
| file_reads | `index_join_scan` | 4.45ms | 4.96ms | 1.114× | 2.32% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.33s | 1.276× | 0.80% | PASS |
| file_reads | `table_scan` | 1.18s | 1.41s | 1.192× | 1.01% | PASS |
| file_reads | `oltp_read_only` | 213.92ms | 162.24ms | 0.758× | 1.24% | PASS |
| file_writes | `oltp_bulk_insert` | 194.85ms | 272.24ms | 1.397× | 1.24% | PASS |
| file_writes | `oltp_insert` | 22.30ms | 36.11ms | 1.619× | 1.61% | PASS |
| file_writes | `oltp_update_index` | 77.67ms | 128.96ms | 1.660× | 2.44% | PASS |
| file_writes | `oltp_update_non_index` | 58.35ms | 81.84ms | 1.403× | 2.00% | PASS |
| file_writes | `oltp_delete_insert` | 68.44ms | 99.79ms | 1.458× | 1.84% | PASS |
| file_writes | `oltp_write_only` | 45.66ms | 65.00ms | 1.424× | 2.30% | PASS |
| file_writes | `types_delete_insert` | 40.78ms | 54.23ms | 1.330× | 1.76% | PASS |
| file_writes | `oltp_read_write` | 97.09ms | 132.43ms | 1.364× | 1.74% | PASS |
| ac_reads | `oltp_point_select` | 49.81ms | 55.73ms | 1.119× | 1.32% | PASS |
| ac_reads | `oltp_range_select` | 13.38ms | 15.86ms | 1.185× | 1.28% | PASS |
| ac_reads | `oltp_sum_range` | 12.78ms | 15.21ms | 1.190× | 1.32% | PASS |
| ac_reads | `oltp_order_range` | 3.01ms | 3.34ms | 1.110× | 1.50% | PASS |
| ac_reads | `oltp_distinct_range` | 4.06ms | 4.40ms | 1.084× | 1.29% | PASS |
| ac_reads | `oltp_index_scan` | 6.89ms | 8.31ms | 1.205× | 1.44% | PASS |
| ac_reads | `select_random_points` | 13.32ms | 14.00ms | 1.051× | 1.93% | PASS |
| ac_reads | `select_random_ranges` | 5.64ms | 6.61ms | 1.171× | 1.17% | PASS |
| ac_reads | `covering_index_scan` | 7.17ms | 7.13ms | 0.995× | 1.17% | PASS |
| ac_reads | `groupby_scan` | 30.28ms | 33.27ms | 1.099× | 0.91% | PASS |
| ac_reads | `index_join` | 7.93ms | 10.26ms | 1.294× | 1.98% | PASS |
| ac_reads | `index_join_scan` | 3.99ms | 5.01ms | 1.258× | 1.76% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.34s | 1.268× | 0.74% | PASS |
| ac_reads | `table_scan` | 1.23s | 1.41s | 1.146× | 1.66% | PASS |
| ac_reads | `oltp_read_only` | 142.09ms | 162.54ms | 1.144× | 1.27% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.50ms | 86.72ms | 3.540× | 8.43% | PASS |
| ac_writes | `oltp_insert_ac` | 26.05ms | 101.32ms | 3.889× | 5.58% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.35ms | 116.37ms | 3.964× | 5.87% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.32ms | 95.07ms | 4.077× | 7.31% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.87ms | 107.34ms | 4.149× | 6.64% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.79ms | 107.84ms | 4.025× | 6.41% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.16ms | 97.02ms | 4.016× | 7.61% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.24ms | 113.14ms | 3.622× | 8.65% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.10ms | 36.57ms | 1.139× | 1.63% | PASS |
| mem_reads | `oltp_range_select` | 15.25ms | 14.21ms | 0.931× | 2.68% | PASS |
| mem_reads | `oltp_sum_range` | 13.25ms | 13.53ms | 1.022× | 1.91% | PASS |
| mem_reads | `oltp_order_range` | 3.27ms | 3.22ms | 0.986× | 1.32% | PASS |
| mem_reads | `oltp_distinct_range` | 4.29ms | 4.22ms | 0.983× | 0.89% | PASS |
| mem_reads | `oltp_index_scan` | 4.83ms | 6.34ms | 1.313× | 1.93% | PASS |
| mem_reads | `select_random_points` | 18.82ms | 21.05ms | 1.119× | 2.44% | PASS |
| mem_reads | `select_random_ranges` | 4.31ms | 5.35ms | 1.241× | 1.54% | PASS |
| mem_reads | `covering_index_scan` | 5.21ms | 4.83ms | 0.926× | 3.40% | PASS |
| mem_reads | `groupby_scan` | 34.76ms | 35.51ms | 1.022× | 0.74% | PASS |
| mem_reads | `index_join` | 7.42ms | 9.94ms | 1.341× | 2.76% | PASS |
| mem_reads | `index_join_scan` | 5.05ms | 5.86ms | 1.160× | 1.80% | PASS |
| mem_reads | `types_table_scan` | 1.31s | 1.30s | 0.992× | 4.89% | PASS |
| mem_reads | `table_scan` | 1.69s | 1.43s | 0.845× | 3.26% | PASS |
| mem_reads | `oltp_read_only` | 128.51ms | 133.43ms | 1.038× | 2.22% | PASS |
| mem_writes | `oltp_bulk_insert` | 232.05ms | 339.54ms | 1.463× | 0.79% | PASS |
| mem_writes | `oltp_insert` | 23.66ms | 40.29ms | 1.703× | 2.84% | PASS |
| mem_writes | `oltp_update_index` | 78.92ms | 145.27ms | 1.841× | 3.25% | PASS |
| mem_writes | `oltp_update_non_index` | 51.91ms | 91.18ms | 1.756× | 1.39% | PASS |
| mem_writes | `oltp_delete_insert` | 55.22ms | 110.31ms | 1.997× | 2.39% | PASS |
| mem_writes | `oltp_write_only` | 31.68ms | 66.47ms | 2.098× | 1.77% | PASS |
| mem_writes | `types_delete_insert` | 34.57ms | 56.78ms | 1.643× | 1.16% | PASS |
| mem_writes | `oltp_read_write` | 94.04ms | 143.57ms | 1.527× | 1.90% | PASS |
| file_reads | `oltp_point_select` | 127.88ms | 68.31ms | 0.534× | 0.96% | PASS |
| file_reads | `oltp_range_select` | 25.98ms | 17.39ms | 0.669× | 2.83% | PASS |
| file_reads | `oltp_sum_range` | 23.21ms | 16.85ms | 0.726× | 1.68% | PASS |
| file_reads | `oltp_order_range` | 4.34ms | 3.57ms | 0.823× | 1.62% | PASS |
| file_reads | `oltp_distinct_range` | 5.38ms | 4.57ms | 0.849× | 1.69% | PASS |
| file_reads | `oltp_index_scan` | 14.67ms | 9.58ms | 0.653× | 2.23% | PASS |
| file_reads | `select_random_points` | 28.89ms | 24.16ms | 0.836× | 1.47% | PASS |
| file_reads | `select_random_ranges` | 14.13ms | 8.62ms | 0.610× | 0.98% | PASS |
| file_reads | `covering_index_scan` | 16.24ms | 8.15ms | 0.502× | 0.84% | PASS |
| file_reads | `groupby_scan` | 36.28ms | 36.14ms | 0.996× | 0.78% | PASS |
| file_reads | `index_join` | 13.57ms | 11.98ms | 0.882× | 1.57% | PASS |
| file_reads | `index_join_scan` | 6.20ms | 6.45ms | 1.040× | 1.39% | PASS |
| file_reads | `types_table_scan` | 1.41s | 1.31s | 0.935× | 1.27% | PASS |
| file_reads | `table_scan` | 1.71s | 1.43s | 0.835× | 2.09% | PASS |
| file_reads | `oltp_read_only` | 269.79ms | 178.63ms | 0.662× | 1.35% | PASS |
| file_writes | `oltp_bulk_insert` | 254.40ms | 371.63ms | 1.461× | 0.93% | PASS |
| file_writes | `oltp_insert` | 55.44ms | 53.46ms | 0.964× | 23.52% | PASS |
| file_writes | `oltp_update_index` | 124.03ms | 180.77ms | 1.457× | 1.93% | PASS |
| file_writes | `oltp_update_non_index` | 104.31ms | 119.27ms | 1.143× | 13.50% | PASS |
| file_writes | `oltp_delete_insert` | 95.65ms | 141.50ms | 1.479× | 1.52% | PASS |
| file_writes | `oltp_write_only` | 83.82ms | 92.18ms | 1.100× | 6.97% | PASS |
| file_writes | `types_delete_insert` | 58.32ms | 77.43ms | 1.328× | 2.62% | PASS |
| file_writes | `oltp_read_write` | 151.53ms | 168.10ms | 1.109× | 10.47% | PASS |
| ac_reads | `oltp_point_select` | 62.82ms | 68.22ms | 1.086× | 1.97% | PASS |
| ac_reads | `oltp_range_select` | 19.13ms | 17.59ms | 0.920× | 2.22% | PASS |
| ac_reads | `oltp_sum_range` | 16.97ms | 17.27ms | 1.018× | 1.74% | PASS |
| ac_reads | `oltp_order_range` | 3.74ms | 3.57ms | 0.956× | 1.08% | PASS |
| ac_reads | `oltp_distinct_range` | 4.75ms | 4.58ms | 0.965× | 0.58% | PASS |
| ac_reads | `oltp_index_scan` | 8.45ms | 9.62ms | 1.138× | 1.02% | PASS |
| ac_reads | `select_random_points` | 22.44ms | 24.42ms | 1.088× | 0.97% | PASS |
| ac_reads | `select_random_ranges` | 7.72ms | 8.59ms | 1.112× | 1.24% | PASS |
| ac_reads | `covering_index_scan` | 9.67ms | 8.13ms | 0.841× | 1.00% | PASS |
| ac_reads | `groupby_scan` | 35.19ms | 36.00ms | 1.023× | 0.84% | PASS |
| ac_reads | `index_join` | 10.10ms | 11.83ms | 1.172× | 1.48% | PASS |
| ac_reads | `index_join_scan` | 5.52ms | 6.33ms | 1.147× | 1.08% | PASS |
| ac_reads | `types_table_scan` | 1.16s | 1.25s | 1.075× | 1.45% | PASS |
| ac_reads | `table_scan` | 1.42s | 1.37s | 0.966× | 2.82% | PASS |
| ac_reads | `oltp_read_only` | 166.01ms | 175.58ms | 1.058× | 0.65% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.61ms | 67.42ms | 3.828× | 5.55% | PASS |
| ac_writes | `oltp_insert_ac` | 21.67ms | 87.21ms | 4.025× | 6.31% | PASS |
| ac_writes | `oltp_update_index_ac` | 22.21ms | 102.63ms | 4.622× | 5.26% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.56ms | 81.74ms | 4.655× | 4.17% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 19.47ms | 94.58ms | 4.857× | 5.47% | PASS |
| ac_writes | `oltp_write_only_ac` | 21.00ms | 93.30ms | 4.444× | 6.30% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.12ms | 84.99ms | 4.963× | 7.38% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.21ms | 103.64ms | 3.809× | 5.22% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.61ms | 37.12ms | 1.213× | 1.42% | PASS |
| mem_reads | `oltp_range_select` | 13.35ms | 14.14ms | 1.059× | 1.74% | PASS |
| mem_reads | `oltp_sum_range` | 12.21ms | 14.05ms | 1.151× | 1.19% | PASS |
| mem_reads | `oltp_order_range` | 2.95ms | 3.15ms | 1.069× | 1.34% | PASS |
| mem_reads | `oltp_distinct_range` | 3.99ms | 4.22ms | 1.057× | 1.17% | PASS |
| mem_reads | `oltp_index_scan` | 4.59ms | 6.30ms | 1.371× | 2.02% | PASS |
| mem_reads | `select_random_points` | 18.11ms | 21.09ms | 1.165× | 2.36% | PASS |
| mem_reads | `select_random_ranges` | 4.17ms | 5.20ms | 1.249× | 1.36% | PASS |
| mem_reads | `covering_index_scan` | 4.57ms | 4.75ms | 1.038× | 2.60% | PASS |
| mem_reads | `groupby_scan` | 32.23ms | 33.80ms | 1.049× | 1.12% | PASS |
| mem_reads | `index_join` | 6.95ms | 9.75ms | 1.402× | 1.85% | PASS |
| mem_reads | `index_join_scan` | 4.23ms | 5.39ms | 1.274× | 1.99% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.23s | 1.186× | 0.89% | PASS |
| mem_reads | `table_scan` | 1.34s | 1.40s | 1.043× | 3.75% | PASS |
| mem_reads | `oltp_read_only` | 127.75ms | 141.17ms | 1.105× | 0.97% | PASS |
| mem_writes | `oltp_bulk_insert` | 242.36ms | 358.31ms | 1.478× | 0.99% | PASS |
| mem_writes | `oltp_insert` | 20.03ms | 40.02ms | 1.998× | 1.10% | PASS |
| mem_writes | `oltp_update_index` | 67.08ms | 130.00ms | 1.938× | 2.01% | PASS |
| mem_writes | `oltp_update_non_index` | 49.15ms | 85.93ms | 1.748× | 1.47% | PASS |
| mem_writes | `oltp_delete_insert` | 49.33ms | 104.82ms | 2.125× | 1.64% | PASS |
| mem_writes | `oltp_write_only` | 28.30ms | 63.05ms | 2.228× | 1.69% | PASS |
| mem_writes | `types_delete_insert` | 32.34ms | 54.66ms | 1.690× | 1.25% | PASS |
| mem_writes | `oltp_read_write` | 87.61ms | 142.78ms | 1.630× | 2.38% | PASS |
| file_reads | `oltp_point_select` | 107.25ms | 63.33ms | 0.591× | 1.00% | PASS |
| file_reads | `oltp_range_select` | 19.95ms | 16.76ms | 0.840× | 1.72% | PASS |
| file_reads | `oltp_sum_range` | 20.37ms | 16.86ms | 0.828× | 1.65% | PASS |
| file_reads | `oltp_order_range` | 3.83ms | 3.50ms | 0.914× | 2.26% | PASS |
| file_reads | `oltp_distinct_range` | 4.92ms | 4.60ms | 0.934× | 1.69% | PASS |
| file_reads | `oltp_index_scan` | 12.58ms | 9.15ms | 0.728× | 1.38% | PASS |
| file_reads | `select_random_points` | 27.30ms | 24.54ms | 0.899× | 1.95% | PASS |
| file_reads | `select_random_ranges` | 11.92ms | 7.86ms | 0.660× | 1.27% | PASS |
| file_reads | `covering_index_scan` | 12.39ms | 7.34ms | 0.593× | 1.56% | PASS |
| file_reads | `groupby_scan` | 32.91ms | 34.31ms | 1.042× | 0.99% | PASS |
| file_reads | `index_join` | 11.44ms | 11.35ms | 0.992× | 1.98% | PASS |
| file_reads | `index_join_scan` | 5.29ms | 5.88ms | 1.111× | 1.75% | PASS |
| file_reads | `types_table_scan` | 1.13s | 1.26s | 1.112× | 1.96% | PASS |
| file_reads | `table_scan` | 1.38s | 1.40s | 1.016× | 1.61% | PASS |
| file_reads | `oltp_read_only` | 237.28ms | 177.57ms | 0.748× | 0.71% | PASS |
| file_writes | `oltp_bulk_insert` | 261.05ms | 381.89ms | 1.463× | 0.97% | PASS |
| file_writes | `oltp_insert` | 32.69ms | 53.09ms | 1.624× | 2.20% | PASS |
| file_writes | `oltp_update_index` | 104.16ms | 164.29ms | 1.577× | 1.78% | PASS |
| file_writes | `oltp_update_non_index` | 79.56ms | 109.26ms | 1.373× | 1.87% | PASS |
| file_writes | `oltp_delete_insert` | 81.36ms | 130.85ms | 1.608× | 1.65% | PASS |
| file_writes | `oltp_write_only` | 55.96ms | 84.86ms | 1.516× | 2.48% | PASS |
| file_writes | `types_delete_insert` | 53.51ms | 73.14ms | 1.367× | 1.73% | PASS |
| file_writes | `oltp_read_write` | 123.88ms | 168.16ms | 1.357× | 1.37% | PASS |
| ac_reads | `oltp_point_select` | 56.78ms | 63.87ms | 1.125× | 1.13% | PASS |
| ac_reads | `oltp_range_select` | 16.37ms | 16.96ms | 1.036× | 1.62% | PASS |
| ac_reads | `oltp_sum_range` | 15.27ms | 16.95ms | 1.110× | 1.45% | PASS |
| ac_reads | `oltp_order_range` | 3.41ms | 3.54ms | 1.035× | 1.72% | PASS |
| ac_reads | `oltp_distinct_range` | 4.47ms | 4.65ms | 1.039× | 1.67% | PASS |
| ac_reads | `oltp_index_scan` | 7.61ms | 9.15ms | 1.202× | 1.37% | PASS |
| ac_reads | `select_random_points` | 21.62ms | 24.43ms | 1.130× | 1.34% | PASS |
| ac_reads | `select_random_ranges` | 6.97ms | 7.87ms | 1.129× | 1.13% | PASS |
| ac_reads | `covering_index_scan` | 7.89ms | 7.37ms | 0.934× | 1.62% | PASS |
| ac_reads | `groupby_scan` | 32.28ms | 34.22ms | 1.060× | 0.51% | PASS |
| ac_reads | `index_join` | 9.29ms | 11.63ms | 1.252× | 2.02% | PASS |
| ac_reads | `index_join_scan` | 4.96ms | 6.16ms | 1.242× | 1.81% | PASS |
| ac_reads | `types_table_scan` | 1.12s | 1.26s | 1.125× | 3.44% | PASS |
| ac_reads | `table_scan` | 1.28s | 1.40s | 1.093× | 4.33% | PASS |
| ac_reads | `oltp_read_only` | 158.98ms | 176.73ms | 1.112× | 1.68% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.79ms | 85.73ms | 3.458× | 7.69% | PASS |
| ac_writes | `oltp_insert_ac` | 26.77ms | 106.20ms | 3.967× | 4.97% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.87ms | 118.68ms | 4.111× | 5.42% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.07ms | 96.43ms | 3.847× | 5.29% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.03ms | 108.34ms | 4.162× | 9.14% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.68ms | 108.28ms | 4.216× | 5.72% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.54ms | 97.57ms | 4.328× | 5.74% | PASS |
| ac_writes | `oltp_read_write_ac` | 34.31ms | 117.97ms | 3.438× | 5.92% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.98ms | 38.67ms | 1.106× | 2.18% | PASS |
| mem_reads | `oltp_range_select` | 21.46ms | 20.37ms | 0.950× | 3.01% | PASS |
| mem_reads | `oltp_sum_range` | 18.87ms | 19.28ms | 1.022× | 1.00% | PASS |
| mem_reads | `oltp_order_range` | 3.84ms | 3.79ms | 0.986× | 0.90% | PASS |
| mem_reads | `oltp_distinct_range` | 4.90ms | 4.87ms | 0.994× | 0.92% | PASS |
| mem_reads | `oltp_index_scan` | 4.71ms | 5.86ms | 1.242× | 1.17% | PASS |
| mem_reads | `select_random_points` | 27.78ms | 31.03ms | 1.117× | 1.80% | PASS |
| mem_reads | `select_random_ranges` | 7.73ms | 8.37ms | 1.082× | 1.08% | PASS |
| mem_reads | `covering_index_scan` | 4.48ms | 4.80ms | 1.072× | 5.21% | PASS |
| mem_reads | `groupby_scan` | 40.30ms | 41.07ms | 1.019× | 1.21% | PASS |
| mem_reads | `index_join` | 8.26ms | 10.17ms | 1.232× | 1.56% | PASS |
| mem_reads | `index_join_scan` | 4.40ms | 5.82ms | 1.323× | 1.89% | PASS |
| mem_reads | `types_table_scan` | 1.16s | 1.25s | 1.084× | 1.22% | PASS |
| mem_reads | `table_scan` | 1.34s | 1.36s | 1.016× | 1.55% | PASS |
| mem_reads | `oltp_read_only` | 153.56ms | 159.94ms | 1.042× | 1.02% | PASS |
| mem_writes | `oltp_bulk_insert` | 245.15ms | 339.51ms | 1.385× | 0.62% | PASS |
| mem_writes | `oltp_insert` | 19.61ms | 36.44ms | 1.858× | 0.69% | PASS |
| mem_writes | `oltp_update_index` | 72.04ms | 125.27ms | 1.739× | 3.30% | PASS |
| mem_writes | `oltp_update_non_index` | 54.32ms | 88.24ms | 1.624× | 1.75% | PASS |
| mem_writes | `oltp_delete_insert` | 52.02ms | 99.51ms | 1.913× | 1.35% | PASS |
| mem_writes | `oltp_write_only` | 28.31ms | 60.91ms | 2.152× | 1.22% | PASS |
| mem_writes | `types_delete_insert` | 34.27ms | 54.99ms | 1.604× | 2.54% | PASS |
| mem_writes | `oltp_read_write` | 103.95ms | 152.25ms | 1.465× | 2.80% | PASS |
| file_reads | `oltp_point_select` | 130.38ms | 70.56ms | 0.541× | 0.81% | PASS |
| file_reads | `oltp_range_select` | 30.21ms | 23.50ms | 0.778× | 1.16% | PASS |
| file_reads | `oltp_sum_range` | 28.34ms | 22.75ms | 0.803× | 0.87% | PASS |
| file_reads | `oltp_order_range` | 4.73ms | 4.21ms | 0.888× | 1.42% | PASS |
| file_reads | `oltp_distinct_range` | 5.80ms | 5.28ms | 0.909× | 1.10% | PASS |
| file_reads | `oltp_index_scan` | 14.23ms | 9.44ms | 0.663× | 1.11% | PASS |
| file_reads | `select_random_points` | 37.54ms | 34.33ms | 0.914× | 1.28% | PASS |
| file_reads | `select_random_ranges` | 17.25ms | 11.75ms | 0.681× | 0.94% | PASS |
| file_reads | `covering_index_scan` | 14.04ms | 7.78ms | 0.554× | 0.92% | PASS |
| file_reads | `groupby_scan` | 40.45ms | 41.25ms | 1.020× | 0.95% | PASS |
| file_reads | `index_join` | 13.35ms | 12.60ms | 0.943× | 1.09% | PASS |
| file_reads | `index_join_scan` | 5.45ms | 6.19ms | 1.137× | 1.17% | PASS |
| file_reads | `types_table_scan` | 1.13s | 1.24s | 1.099× | 0.54% | PASS |
| file_reads | `table_scan` | 1.32s | 1.35s | 1.026× | 0.62% | PASS |
| file_reads | `oltp_read_only` | 290.65ms | 206.46ms | 0.710× | 0.57% | PASS |
| file_writes | `oltp_bulk_insert` | 262.25ms | 361.62ms | 1.379× | 0.85% | PASS |
| file_writes | `oltp_insert` | 26.37ms | 46.94ms | 1.780× | 1.36% | PASS |
| file_writes | `oltp_update_index` | 100.13ms | 147.07ms | 1.469× | 1.25% | PASS |
| file_writes | `oltp_update_non_index` | 80.39ms | 107.02ms | 1.331× | 1.62% | PASS |
| file_writes | `oltp_delete_insert` | 78.83ms | 121.54ms | 1.542× | 1.24% | PASS |
| file_writes | `oltp_write_only` | 51.78ms | 81.45ms | 1.573× | 1.41% | PASS |
| file_writes | `types_delete_insert` | 51.08ms | 67.96ms | 1.330× | 1.13% | PASS |
| file_writes | `oltp_read_write` | 124.43ms | 169.99ms | 1.366× | 1.08% | PASS |
| ac_reads | `oltp_point_select` | 65.23ms | 70.13ms | 1.075× | 0.98% | PASS |
| ac_reads | `oltp_range_select` | 23.92ms | 23.47ms | 0.981× | 1.04% | PASS |
| ac_reads | `oltp_sum_range` | 22.32ms | 22.70ms | 1.017× | 1.17% | PASS |
| ac_reads | `oltp_order_range` | 4.26ms | 4.17ms | 0.979× | 1.12% | PASS |
| ac_reads | `oltp_distinct_range` | 5.29ms | 5.25ms | 0.993× | 0.96% | PASS |
| ac_reads | `oltp_index_scan` | 8.12ms | 9.37ms | 1.154× | 1.05% | PASS |
| ac_reads | `select_random_points` | 31.38ms | 34.01ms | 1.084× | 0.95% | PASS |
| ac_reads | `select_random_ranges` | 11.08ms | 11.81ms | 1.066× | 0.85% | PASS |
| ac_reads | `covering_index_scan` | 7.80ms | 7.79ms | 0.999× | 1.05% | PASS |
| ac_reads | `groupby_scan` | 39.81ms | 41.25ms | 1.036× | 0.95% | PASS |
| ac_reads | `index_join` | 10.25ms | 12.69ms | 1.238× | 0.93% | PASS |
| ac_reads | `index_join_scan` | 4.85ms | 6.15ms | 1.267× | 1.03% | PASS |
| ac_reads | `types_table_scan` | 1.13s | 1.24s | 1.099× | 0.52% | PASS |
| ac_reads | `table_scan` | 1.32s | 1.36s | 1.025× | 0.51% | PASS |
| ac_reads | `oltp_read_only` | 199.44ms | 206.61ms | 1.036× | 0.77% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.74ms | 67.82ms | 3.824× | 3.31% | PASS |
| ac_writes | `oltp_insert_ac` | 19.57ms | 89.09ms | 4.552× | 5.35% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.65ms | 101.06ms | 4.669× | 6.87% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.70ms | 79.74ms | 4.505× | 7.17% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.63ms | 89.22ms | 4.788× | 3.11% | PASS |
| ac_writes | `oltp_write_only_ac` | 20.06ms | 89.28ms | 4.450× | 5.42% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.66ms | 81.47ms | 4.891× | 5.72% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.54ms | 98.57ms | 3.715× | 5.11% | PASS |

</details>

## Version-control latency

Wall time: 2m 21s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.04ms | 200.00ms | 41.5% | 0.37% | PASS |
| `status_dirty_many_tables` | 86.44ms | 200.00ms | 43.2% | 0.67% | PASS |
| `diff_regular_working_one_table` | 78.83ms | 150.00ms | 52.6% | 0.44% | PASS |
| `diff_regular_working_many_tables` | 92.10ms | 200.00ms | 46.0% | 0.45% | PASS |
| `diff_stat_working_many_tables` | 91.86ms | 200.00ms | 45.9% | 0.50% | PASS |
| `diff_schema_working_many_tables` | 92.29ms | 200.00ms | 46.1% | 0.44% | PASS |
| `branch_list_many_branches` | 23.04ms | 100.00ms | 23.0% | 1.39% | PASS |
| `branch_create_delete` | 25.22ms | 100.00ms | 25.2% | 1.82% | PASS |
| `checkout_branch_clean` | 55.92ms | 200.00ms | 28.0% | 1.26% | PASS |
| `merge_data_no_conflicts` | 29.61ms | 150.00ms | 19.7% | 1.77% | PASS |
| `merge_schema_no_conflicts` | 23.19ms | 100.00ms | 23.2% | 2.32% | PASS |
| `merge_data_conflicts` | 127.57ms | 250.00ms | 51.0% | 0.36% | PASS |
| `merge_data_conflicts_with_resolve` | 128.10ms | 250.00ms | 51.2% | 0.44% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
