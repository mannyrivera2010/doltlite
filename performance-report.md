# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-24 15:07 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261023.334.3
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/40098468116)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 15s | 8.81s | 12.05s | 1.369× | 1.60% | **PASS** |
| textpk | 69 | 55 | 1h 34m 22s | 10.52s | 11.93s | 1.134× | 2.24% | **PASS** |
| blobpk | 69 | 55 | 1h 31m 41s | 9.89s | 11.80s | 1.194× | 1.76% | **PASS** |
| compositepk | 69 | 55 | 1h 9m 17s | 7.91s | 9.50s | 1.200× | 1.53% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.60ms | 29.45ms | 1.248× | 1.27% | PASS |
| mem_reads | `oltp_range_select` | 10.50ms | 13.45ms | 1.281× | 2.58% | PASS |
| mem_reads | `oltp_sum_range` | 9.53ms | 12.77ms | 1.340× | 2.45% | PASS |
| mem_reads | `oltp_order_range` | 2.69ms | 3.16ms | 1.174× | 3.25% | PASS |
| mem_reads | `oltp_distinct_range` | 3.71ms | 4.26ms | 1.147× | 2.32% | PASS |
| mem_reads | `oltp_index_scan` | 3.85ms | 5.23ms | 1.358× | 1.63% | PASS |
| mem_reads | `select_random_points` | 9.66ms | 11.18ms | 1.157× | 1.60% | PASS |
| mem_reads | `select_random_ranges` | 2.90ms | 3.96ms | 1.369× | 2.16% | PASS |
| mem_reads | `covering_index_scan` | 4.46ms | 4.13ms | 0.927× | 1.46% | PASS |
| mem_reads | `groupby_scan` | 30.24ms | 33.98ms | 1.124× | 1.56% | PASS |
| mem_reads | `index_join` | 6.04ms | 8.05ms | 1.332× | 0.98% | PASS |
| mem_reads | `index_join_scan` | 3.45ms | 4.59ms | 1.331× | 1.82% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.42s | 1.374× | 0.51% | PASS |
| mem_reads | `table_scan` | 1.16s | 1.51s | 1.302× | 0.54% | PASS |
| mem_reads | `oltp_read_only` | 103.22ms | 124.69ms | 1.208× | 0.98% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.56ms | 253.36ms | 1.403× | 1.09% | PASS |
| mem_writes | `oltp_insert` | 15.50ms | 28.66ms | 1.849× | 1.16% | PASS |
| mem_writes | `oltp_update_index` | 49.90ms | 104.66ms | 2.097× | 1.17% | PASS |
| mem_writes | `oltp_update_non_index` | 33.66ms | 59.55ms | 1.769× | 1.18% | PASS |
| mem_writes | `oltp_delete_insert` | 44.95ms | 79.34ms | 1.765× | 0.89% | PASS |
| mem_writes | `oltp_write_only` | 21.80ms | 45.28ms | 2.077× | 1.65% | PASS |
| mem_writes | `types_delete_insert` | 24.29ms | 40.43ms | 1.664× | 0.98% | PASS |
| mem_writes | `oltp_read_write` | 67.09ms | 112.01ms | 1.670× | 0.90% | PASS |
| file_reads | `oltp_point_select` | 98.29ms | 55.23ms | 0.562× | 0.83% | PASS |
| file_reads | `oltp_range_select` | 18.89ms | 16.84ms | 0.891× | 2.05% | PASS |
| file_reads | `oltp_sum_range` | 18.10ms | 16.25ms | 0.897× | 2.25% | PASS |
| file_reads | `oltp_order_range` | 4.13ms | 4.04ms | 0.978× | 6.05% | PASS |
| file_reads | `oltp_distinct_range` | 5.21ms | 5.16ms | 0.990× | 5.94% | PASS |
| file_reads | `oltp_index_scan` | 11.57ms | 8.29ms | 0.716× | 1.71% | PASS |
| file_reads | `select_random_points` | 17.46ms | 14.18ms | 0.812× | 1.57% | PASS |
| file_reads | `select_random_ranges` | 10.53ms | 6.73ms | 0.639× | 1.46% | PASS |
| file_reads | `covering_index_scan` | 12.54ms | 7.14ms | 0.570× | 1.71% | PASS |
| file_reads | `groupby_scan` | 32.06ms | 35.17ms | 1.097× | 1.22% | PASS |
| file_reads | `index_join` | 10.51ms | 10.23ms | 0.974× | 1.19% | PASS |
| file_reads | `index_join_scan` | 4.63ms | 5.18ms | 1.117× | 1.96% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.42s | 1.368× | 0.64% | PASS |
| file_reads | `table_scan` | 1.16s | 1.52s | 1.306× | 0.44% | PASS |
| file_reads | `oltp_read_only` | 216.18ms | 164.16ms | 0.759× | 0.66% | PASS |
| file_writes | `oltp_bulk_insert` | 195.22ms | 272.47ms | 1.396× | 1.20% | PASS |
| file_writes | `oltp_insert` | 22.32ms | 36.60ms | 1.640× | 2.51% | PASS |
| file_writes | `oltp_update_index` | 77.55ms | 129.04ms | 1.664× | 1.61% | PASS |
| file_writes | `oltp_update_non_index` | 57.70ms | 82.22ms | 1.425× | 1.13% | PASS |
| file_writes | `oltp_delete_insert` | 68.07ms | 100.01ms | 1.469× | 1.50% | PASS |
| file_writes | `oltp_write_only` | 44.70ms | 65.50ms | 1.465× | 1.51% | PASS |
| file_writes | `types_delete_insert` | 40.30ms | 54.29ms | 1.347× | 1.21% | PASS |
| file_writes | `oltp_read_write` | 91.79ms | 132.68ms | 1.445× | 1.35% | PASS |
| ac_reads | `oltp_point_select` | 48.45ms | 55.23ms | 1.140× | 1.06% | PASS |
| ac_reads | `oltp_range_select` | 14.33ms | 16.82ms | 1.174× | 2.89% | PASS |
| ac_reads | `oltp_sum_range` | 13.14ms | 16.32ms | 1.242× | 2.09% | PASS |
| ac_reads | `oltp_order_range` | 3.66ms | 4.07ms | 1.111× | 5.22% | PASS |
| ac_reads | `oltp_distinct_range` | 4.71ms | 5.15ms | 1.093× | 3.91% | PASS |
| ac_reads | `oltp_index_scan` | 6.72ms | 8.32ms | 1.238× | 1.34% | PASS |
| ac_reads | `select_random_points` | 13.15ms | 14.22ms | 1.081× | 1.95% | PASS |
| ac_reads | `select_random_ranges` | 5.66ms | 6.74ms | 1.190× | 1.60% | PASS |
| ac_reads | `covering_index_scan` | 7.57ms | 7.18ms | 0.948× | 1.70% | PASS |
| ac_reads | `groupby_scan` | 31.62ms | 35.55ms | 1.124× | 1.28% | PASS |
| ac_reads | `index_join` | 8.00ms | 10.37ms | 1.296× | 1.62% | PASS |
| ac_reads | `index_join_scan` | 4.22ms | 5.23ms | 1.240× | 2.57% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.42s | 1.369× | 0.50% | PASS |
| ac_reads | `table_scan` | 1.16s | 1.52s | 1.307× | 0.49% | PASS |
| ac_reads | `oltp_read_only` | 141.93ms | 164.27ms | 1.157× | 0.98% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.06ms | 67.26ms | 2.917× | 6.26% | PASS |
| ac_writes | `oltp_insert_ac` | 24.31ms | 85.66ms | 3.524× | 4.82% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.76ms | 100.05ms | 3.884× | 3.06% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.85ms | 78.66ms | 3.600× | 5.46% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.43ms | 91.15ms | 3.732× | 8.46% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.92ms | 89.34ms | 3.446× | 5.01% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.06ms | 79.03ms | 3.427× | 6.39% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.47ms | 96.79ms | 3.284× | 5.54% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.95ms | 37.71ms | 1.259× | 1.24% | PASS |
| mem_reads | `oltp_range_select` | 14.02ms | 14.23ms | 1.015× | 4.86% | PASS |
| mem_reads | `oltp_sum_range` | 12.39ms | 14.24ms | 1.149× | 2.39% | PASS |
| mem_reads | `oltp_order_range` | 3.06ms | 3.24ms | 1.059× | 1.46% | PASS |
| mem_reads | `oltp_distinct_range` | 4.05ms | 4.28ms | 1.056× | 1.03% | PASS |
| mem_reads | `oltp_index_scan` | 4.53ms | 6.34ms | 1.399× | 1.80% | PASS |
| mem_reads | `select_random_points` | 18.48ms | 21.40ms | 1.158× | 2.49% | PASS |
| mem_reads | `select_random_ranges` | 4.26ms | 5.27ms | 1.235× | 1.69% | PASS |
| mem_reads | `covering_index_scan` | 4.64ms | 4.75ms | 1.022× | 3.35% | PASS |
| mem_reads | `groupby_scan` | 31.94ms | 34.32ms | 1.075× | 0.91% | PASS |
| mem_reads | `index_join` | 6.95ms | 8.92ms | 1.284× | 1.72% | PASS |
| mem_reads | `index_join_scan` | 4.73ms | 5.39ms | 1.141× | 3.88% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.24s | 1.155× | 1.68% | PASS |
| mem_reads | `table_scan` | 1.49s | 1.40s | 0.941× | 2.65% | PASS |
| mem_reads | `oltp_read_only` | 132.29ms | 143.27ms | 1.083× | 3.29% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.46ms | 360.67ms | 1.525× | 1.12% | PASS |
| mem_writes | `oltp_insert` | 23.81ms | 41.00ms | 1.722× | 2.22% | PASS |
| mem_writes | `oltp_update_index` | 78.08ms | 142.54ms | 1.826× | 4.07% | PASS |
| mem_writes | `oltp_update_non_index` | 49.76ms | 87.75ms | 1.763× | 1.13% | PASS |
| mem_writes | `oltp_delete_insert` | 51.69ms | 104.29ms | 2.018× | 1.73% | PASS |
| mem_writes | `oltp_write_only` | 30.28ms | 63.04ms | 2.082× | 1.68% | PASS |
| mem_writes | `types_delete_insert` | 34.08ms | 56.54ms | 1.659× | 0.94% | PASS |
| mem_writes | `oltp_read_write` | 93.44ms | 144.38ms | 1.545× | 2.43% | PASS |
| file_reads | `oltp_point_select` | 107.80ms | 64.54ms | 0.599× | 0.98% | PASS |
| file_reads | `oltp_range_select` | 23.55ms | 17.41ms | 0.739× | 3.30% | PASS |
| file_reads | `oltp_sum_range` | 22.13ms | 17.68ms | 0.799× | 2.24% | PASS |
| file_reads | `oltp_order_range` | 4.27ms | 3.76ms | 0.880× | 1.87% | PASS |
| file_reads | `oltp_distinct_range` | 5.38ms | 4.83ms | 0.897× | 2.13% | PASS |
| file_reads | `oltp_index_scan` | 12.80ms | 9.52ms | 0.743× | 1.40% | PASS |
| file_reads | `select_random_points` | 29.30ms | 26.10ms | 0.891× | 3.04% | PASS |
| file_reads | `select_random_ranges` | 12.03ms | 7.99ms | 0.664× | 0.90% | PASS |
| file_reads | `covering_index_scan` | 13.95ms | 7.60ms | 0.545× | 2.40% | PASS |
| file_reads | `groupby_scan` | 34.17ms | 35.13ms | 1.028× | 0.97% | PASS |
| file_reads | `index_join` | 12.52ms | 11.66ms | 0.931× | 2.48% | PASS |
| file_reads | `index_join_scan` | 5.95ms | 6.05ms | 1.018× | 3.73% | PASS |
| file_reads | `types_table_scan` | 1.23s | 1.26s | 1.031× | 2.67% | PASS |
| file_reads | `table_scan` | 1.49s | 1.40s | 0.945× | 3.60% | PASS |
| file_reads | `oltp_read_only` | 239.79ms | 178.96ms | 0.746× | 0.95% | PASS |
| file_writes | `oltp_bulk_insert` | 255.66ms | 390.35ms | 1.527× | 1.05% | PASS |
| file_writes | `oltp_insert` | 54.61ms | 53.30ms | 0.976× | 25.10% | PASS |
| file_writes | `oltp_update_index` | 121.52ms | 174.73ms | 1.438× | 2.39% | PASS |
| file_writes | `oltp_update_non_index` | 106.11ms | 114.83ms | 1.082× | 15.64% | PASS |
| file_writes | `oltp_delete_insert` | 96.51ms | 138.62ms | 1.436× | 1.94% | PASS |
| file_writes | `oltp_write_only` | 83.44ms | 88.20ms | 1.057× | 10.27% | PASS |
| file_writes | `types_delete_insert` | 58.16ms | 76.85ms | 1.321× | 1.93% | PASS |
| file_writes | `oltp_read_write` | 148.28ms | 168.52ms | 1.137× | 8.70% | PASS |
| ac_reads | `oltp_point_select` | 56.72ms | 64.37ms | 1.135× | 1.28% | PASS |
| ac_reads | `oltp_range_select` | 17.56ms | 17.20ms | 0.980× | 2.70% | PASS |
| ac_reads | `oltp_sum_range` | 16.38ms | 17.34ms | 1.059× | 2.65% | PASS |
| ac_reads | `oltp_order_range` | 3.56ms | 3.59ms | 1.007× | 1.58% | PASS |
| ac_reads | `oltp_distinct_range` | 4.60ms | 4.68ms | 1.018× | 1.15% | PASS |
| ac_reads | `oltp_index_scan` | 7.76ms | 9.42ms | 1.213× | 1.44% | PASS |
| ac_reads | `select_random_points` | 22.28ms | 25.02ms | 1.123× | 2.20% | PASS |
| ac_reads | `select_random_ranges` | 6.95ms | 7.91ms | 1.138× | 1.24% | PASS |
| ac_reads | `covering_index_scan` | 8.66ms | 7.49ms | 0.865× | 2.20% | PASS |
| ac_reads | `groupby_scan` | 33.26ms | 35.10ms | 1.055× | 0.65% | PASS |
| ac_reads | `index_join` | 9.92ms | 11.60ms | 1.169× | 3.01% | PASS |
| ac_reads | `index_join_scan` | 5.55ms | 6.10ms | 1.099× | 3.63% | PASS |
| ac_reads | `types_table_scan` | 1.15s | 1.26s | 1.094× | 5.41% | PASS |
| ac_reads | `table_scan` | 1.22s | 1.36s | 1.120× | 0.77% | PASS |
| ac_reads | `oltp_read_only` | 156.42ms | 175.91ms | 1.125× | 0.98% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.88ms | 68.82ms | 2.883× | 6.73% | PASS |
| ac_writes | `oltp_insert_ac` | 26.87ms | 84.84ms | 3.157× | 5.28% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.21ms | 105.18ms | 3.601× | 6.48% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.84ms | 83.62ms | 3.507× | 4.79% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.86ms | 93.81ms | 3.628× | 5.57% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.38ms | 92.53ms | 3.508× | 5.30% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.59ms | 82.62ms | 3.657× | 7.33% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.04ms | 102.63ms | 3.106× | 6.78% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.99ms | 37.81ms | 1.182× | 1.19% | PASS |
| mem_reads | `oltp_range_select` | 13.37ms | 14.01ms | 1.047× | 2.88% | PASS |
| mem_reads | `oltp_sum_range` | 12.18ms | 13.86ms | 1.138× | 1.72% | PASS |
| mem_reads | `oltp_order_range` | 2.88ms | 3.13ms | 1.088× | 1.69% | PASS |
| mem_reads | `oltp_distinct_range` | 3.98ms | 4.19ms | 1.052× | 0.98% | PASS |
| mem_reads | `oltp_index_scan` | 4.52ms | 6.14ms | 1.358× | 0.91% | PASS |
| mem_reads | `select_random_points` | 18.12ms | 20.54ms | 1.133× | 1.91% | PASS |
| mem_reads | `select_random_ranges` | 4.07ms | 5.13ms | 1.261× | 1.87% | PASS |
| mem_reads | `covering_index_scan` | 4.41ms | 4.53ms | 1.027× | 2.38% | PASS |
| mem_reads | `groupby_scan` | 31.77ms | 34.16ms | 1.075× | 0.76% | PASS |
| mem_reads | `index_join` | 6.88ms | 9.33ms | 1.355× | 1.77% | PASS |
| mem_reads | `index_join_scan` | 4.53ms | 5.35ms | 1.182× | 2.13% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.24s | 1.156× | 2.47% | PASS |
| mem_reads | `table_scan` | 1.24s | 1.38s | 1.110× | 2.07% | PASS |
| mem_reads | `oltp_read_only` | 122.05ms | 136.24ms | 1.116× | 1.52% | PASS |
| mem_writes | `oltp_bulk_insert` | 242.84ms | 353.89ms | 1.457× | 0.82% | PASS |
| mem_writes | `oltp_insert` | 19.94ms | 39.22ms | 1.967× | 0.67% | PASS |
| mem_writes | `oltp_update_index` | 68.57ms | 128.83ms | 1.879× | 0.92% | PASS |
| mem_writes | `oltp_update_non_index` | 49.51ms | 84.14ms | 1.699× | 1.16% | PASS |
| mem_writes | `oltp_delete_insert` | 48.78ms | 101.63ms | 2.084× | 1.17% | PASS |
| mem_writes | `oltp_write_only` | 27.57ms | 60.62ms | 2.198× | 1.16% | PASS |
| mem_writes | `types_delete_insert` | 32.97ms | 53.54ms | 1.624× | 1.01% | PASS |
| mem_writes | `oltp_read_write` | 83.24ms | 137.90ms | 1.657× | 1.19% | PASS |
| file_reads | `oltp_point_select` | 106.29ms | 63.20ms | 0.595× | 0.88% | PASS |
| file_reads | `oltp_range_select` | 20.79ms | 16.91ms | 0.814× | 2.03% | PASS |
| file_reads | `oltp_sum_range` | 20.57ms | 16.97ms | 0.825× | 2.31% | PASS |
| file_reads | `oltp_order_range` | 3.99ms | 3.57ms | 0.893× | 1.51% | PASS |
| file_reads | `oltp_distinct_range` | 5.00ms | 4.60ms | 0.921× | 1.61% | PASS |
| file_reads | `oltp_index_scan` | 12.48ms | 9.21ms | 0.738× | 1.76% | PASS |
| file_reads | `select_random_points` | 27.12ms | 24.68ms | 0.910× | 1.89% | PASS |
| file_reads | `select_random_ranges` | 11.72ms | 7.89ms | 0.673× | 1.05% | PASS |
| file_reads | `covering_index_scan` | 12.04ms | 7.39ms | 0.614× | 2.02% | PASS |
| file_reads | `groupby_scan` | 32.93ms | 34.76ms | 1.056× | 1.14% | PASS |
| file_reads | `index_join` | 11.40ms | 11.38ms | 0.998× | 3.27% | PASS |
| file_reads | `index_join_scan` | 5.47ms | 5.79ms | 1.059× | 1.94% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.23s | 1.158× | 1.41% | PASS |
| file_reads | `table_scan` | 1.43s | 1.42s | 0.989× | 1.15% | PASS |
| file_reads | `oltp_read_only` | 239.70ms | 178.82ms | 0.746× | 0.84% | PASS |
| file_writes | `oltp_bulk_insert` | 261.66ms | 382.50ms | 1.462× | 0.68% | PASS |
| file_writes | `oltp_insert` | 33.33ms | 52.85ms | 1.586× | 1.91% | PASS |
| file_writes | `oltp_update_index` | 113.51ms | 171.18ms | 1.508× | 2.79% | PASS |
| file_writes | `oltp_update_non_index` | 82.80ms | 110.55ms | 1.335× | 1.28% | PASS |
| file_writes | `oltp_delete_insert` | 87.92ms | 135.26ms | 1.538× | 2.50% | PASS |
| file_writes | `oltp_write_only` | 60.45ms | 87.21ms | 1.443× | 2.06% | PASS |
| file_writes | `types_delete_insert` | 55.63ms | 74.58ms | 1.341× | 1.81% | PASS |
| file_writes | `oltp_read_write` | 118.59ms | 164.44ms | 1.387× | 1.52% | PASS |
| ac_reads | `oltp_point_select` | 55.98ms | 64.19ms | 1.147× | 1.01% | PASS |
| ac_reads | `oltp_range_select` | 16.15ms | 16.87ms | 1.045× | 1.44% | PASS |
| ac_reads | `oltp_sum_range` | 15.20ms | 17.02ms | 1.120× | 1.49% | PASS |
| ac_reads | `oltp_order_range` | 3.27ms | 3.52ms | 1.076× | 1.82% | PASS |
| ac_reads | `oltp_distinct_range` | 4.34ms | 4.59ms | 1.057× | 1.06% | PASS |
| ac_reads | `oltp_index_scan` | 7.60ms | 9.21ms | 1.212× | 2.46% | PASS |
| ac_reads | `select_random_points` | 22.11ms | 24.64ms | 1.114× | 2.11% | PASS |
| ac_reads | `select_random_ranges` | 6.85ms | 7.87ms | 1.149× | 1.27% | PASS |
| ac_reads | `covering_index_scan` | 7.76ms | 7.42ms | 0.957× | 2.36% | PASS |
| ac_reads | `groupby_scan` | 32.34ms | 34.85ms | 1.078× | 0.80% | PASS |
| ac_reads | `index_join` | 8.88ms | 11.41ms | 1.284× | 3.91% | PASS |
| ac_reads | `index_join_scan` | 4.93ms | 5.82ms | 1.180× | 2.03% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.23s | 1.163× | 1.37% | PASS |
| ac_reads | `table_scan` | 1.31s | 1.40s | 1.065× | 4.61% | PASS |
| ac_reads | `oltp_read_only` | 163.47ms | 177.48ms | 1.086× | 1.64% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.46ms | 68.39ms | 3.044× | 6.22% | PASS |
| ac_writes | `oltp_insert_ac` | 25.08ms | 89.29ms | 3.560× | 8.39% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.75ms | 100.39ms | 3.753× | 4.06% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.40ms | 78.80ms | 3.517× | 6.62% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.90ms | 92.63ms | 3.876× | 4.21% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.99ms | 93.52ms | 3.742× | 5.52% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.82ms | 84.30ms | 3.694× | 8.59% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.19ms | 98.44ms | 3.156× | 7.63% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.81ms | 27.06ms | 1.136× | 0.78% | PASS |
| mem_reads | `oltp_range_select` | 15.09ms | 16.03ms | 1.063× | 1.08% | PASS |
| mem_reads | `oltp_sum_range` | 13.91ms | 14.96ms | 1.076× | 1.90% | PASS |
| mem_reads | `oltp_order_range` | 2.83ms | 3.06ms | 1.082× | 1.61% | PASS |
| mem_reads | `oltp_distinct_range` | 3.67ms | 3.86ms | 1.050× | 1.01% | PASS |
| mem_reads | `oltp_index_scan` | 3.43ms | 4.33ms | 1.262× | 1.43% | PASS |
| mem_reads | `select_random_points` | 21.90ms | 24.29ms | 1.109× | 1.24% | PASS |
| mem_reads | `select_random_ranges` | 5.39ms | 6.21ms | 1.151× | 1.75% | PASS |
| mem_reads | `covering_index_scan` | 2.96ms | 3.07ms | 1.038× | 1.80% | PASS |
| mem_reads | `groupby_scan` | 29.45ms | 30.40ms | 1.032× | 0.91% | PASS |
| mem_reads | `index_join` | 6.30ms | 7.96ms | 1.264× | 0.97% | PASS |
| mem_reads | `index_join_scan` | 3.03ms | 4.43ms | 1.461× | 1.76% | PASS |
| mem_reads | `types_table_scan` | 870.61ms | 1.02s | 1.173× | 0.28% | PASS |
| mem_reads | `table_scan` | 990.77ms | 1.11s | 1.117× | 0.44% | PASS |
| mem_reads | `oltp_read_only` | 110.33ms | 121.71ms | 1.103× | 0.33% | PASS |
| mem_writes | `oltp_bulk_insert` | 170.28ms | 231.53ms | 1.360× | 0.80% | PASS |
| mem_writes | `oltp_insert` | 13.68ms | 24.41ms | 1.785× | 0.61% | PASS |
| mem_writes | `oltp_update_index` | 47.81ms | 81.64ms | 1.708× | 1.79% | PASS |
| mem_writes | `oltp_update_non_index` | 33.67ms | 55.81ms | 1.657× | 1.53% | PASS |
| mem_writes | `oltp_delete_insert` | 34.27ms | 65.06ms | 1.898× | 1.02% | PASS |
| mem_writes | `oltp_write_only` | 17.82ms | 38.55ms | 2.164× | 1.84% | PASS |
| mem_writes | `types_delete_insert` | 22.14ms | 35.82ms | 1.618× | 1.12% | PASS |
| mem_writes | `oltp_read_write` | 67.60ms | 103.04ms | 1.524× | 0.50% | PASS |
| file_reads | `oltp_point_select` | 49.10ms | 35.07ms | 0.714× | 0.73% | PASS |
| file_reads | `oltp_range_select` | 16.88ms | 16.82ms | 0.997× | 2.03% | PASS |
| file_reads | `oltp_sum_range` | 16.10ms | 15.78ms | 0.980× | 1.42% | PASS |
| file_reads | `oltp_order_range` | 3.08ms | 3.12ms | 1.013× | 1.75% | PASS |
| file_reads | `oltp_distinct_range` | 3.87ms | 3.91ms | 1.011× | 1.35% | PASS |
| file_reads | `oltp_index_scan` | 5.99ms | 5.07ms | 0.846× | 1.23% | PASS |
| file_reads | `select_random_points` | 24.11ms | 25.10ms | 1.041× | 1.62% | PASS |
| file_reads | `select_random_ranges` | 8.14ms | 7.17ms | 0.881× | 1.05% | PASS |
| file_reads | `covering_index_scan` | 5.71ms | 3.98ms | 0.697× | 1.55% | PASS |
| file_reads | `groupby_scan` | 29.19ms | 30.36ms | 1.040× | 0.81% | PASS |
| file_reads | `index_join` | 7.77ms | 8.59ms | 1.106× | 1.09% | PASS |
| file_reads | `index_join_scan` | 3.39ms | 4.61ms | 1.359× | 2.47% | PASS |
| file_reads | `types_table_scan` | 869.95ms | 1.02s | 1.178× | 0.23% | PASS |
| file_reads | `table_scan` | 990.87ms | 1.11s | 1.118× | 0.29% | PASS |
| file_reads | `oltp_read_only` | 148.71ms | 135.04ms | 0.908× | 0.58% | PASS |
| file_writes | `oltp_bulk_insert` | 218.51ms | 282.95ms | 1.295× | 1.94% | PASS |
| file_writes | `oltp_insert` | 20.39ms | 39.26ms | 1.926× | 6.22% | PASS |
| file_writes | `oltp_update_index` | 140.24ms | 142.74ms | 1.018× | 2.55% | PASS |
| file_writes | `oltp_update_non_index` | 119.40ms | 106.50ms | 0.892× | 4.75% | PASS |
| file_writes | `oltp_delete_insert` | 119.74ms | 117.26ms | 0.979× | 1.71% | PASS |
| file_writes | `oltp_write_only` | 80.01ms | 88.63ms | 1.108× | 1.89% | PASS |
| file_writes | `types_delete_insert` | 69.65ms | 69.34ms | 0.995× | 1.62% | PASS |
| file_writes | `oltp_read_write` | 129.72ms | 148.72ms | 1.146× | 2.75% | PASS |
| ac_reads | `oltp_point_select` | 31.27ms | 34.92ms | 1.117× | 1.32% | PASS |
| ac_reads | `oltp_range_select` | 15.14ms | 16.86ms | 1.113× | 2.17% | PASS |
| ac_reads | `oltp_sum_range` | 14.34ms | 15.86ms | 1.106× | 2.13% | PASS |
| ac_reads | `oltp_order_range` | 2.96ms | 3.16ms | 1.067× | 1.82% | PASS |
| ac_reads | `oltp_distinct_range` | 3.74ms | 3.93ms | 1.051× | 1.05% | PASS |
| ac_reads | `oltp_index_scan` | 4.35ms | 5.17ms | 1.189× | 1.69% | PASS |
| ac_reads | `select_random_points` | 22.30ms | 24.95ms | 1.119× | 1.28% | PASS |
| ac_reads | `select_random_ranges` | 6.16ms | 7.08ms | 1.149× | 1.45% | PASS |
| ac_reads | `covering_index_scan` | 3.88ms | 3.93ms | 1.013× | 1.55% | PASS |
| ac_reads | `groupby_scan` | 28.55ms | 30.25ms | 1.059× | 0.81% | PASS |
| ac_reads | `index_join` | 6.56ms | 8.27ms | 1.260× | 0.67% | PASS |
| ac_reads | `index_join_scan` | 3.01ms | 4.05ms | 1.347× | 3.76% | PASS |
| ac_reads | `types_table_scan` | 867.75ms | 1.02s | 1.180× | 0.16% | PASS |
| ac_reads | `table_scan` | 988.88ms | 1.11s | 1.120× | 0.23% | PASS |
| ac_reads | `oltp_read_only` | 121.66ms | 134.14ms | 1.103× | 0.31% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.34ms | 62.76ms | 2.689× | 2.48% | PASS |
| ac_writes | `oltp_insert_ac` | 24.70ms | 78.55ms | 3.180× | 4.69% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.81ms | 88.17ms | 3.416× | 5.88% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.39ms | 72.56ms | 3.102× | 3.73% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.41ms | 78.66ms | 3.222× | 3.44% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.27ms | 81.65ms | 3.364× | 4.12% | PASS |
| ac_writes | `types_delete_insert_ac` | 27.57ms | 69.45ms | 2.519× | 11.20% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.12ms | 87.10ms | 3.098× | 3.72% | PASS |

</details>

## Version-control latency

Wall time: 2m 20s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.83ms | 200.00ms | 41.9% | 0.51% | PASS |
| `status_dirty_many_tables` | 86.50ms | 200.00ms | 43.2% | 0.60% | PASS |
| `diff_regular_working_one_table` | 78.45ms | 150.00ms | 52.3% | 0.68% | PASS |
| `diff_regular_working_many_tables` | 92.36ms | 200.00ms | 46.2% | 0.49% | PASS |
| `diff_stat_working_many_tables` | 92.31ms | 200.00ms | 46.2% | 0.52% | PASS |
| `diff_schema_working_many_tables` | 91.77ms | 200.00ms | 45.9% | 0.54% | PASS |
| `branch_list_many_branches` | 22.75ms | 100.00ms | 22.7% | 1.91% | PASS |
| `branch_create_delete` | 24.60ms | 100.00ms | 24.6% | 1.90% | PASS |
| `checkout_branch_clean` | 55.29ms | 200.00ms | 27.6% | 1.49% | PASS |
| `merge_data_no_conflicts` | 28.93ms | 150.00ms | 19.3% | 1.35% | PASS |
| `merge_schema_no_conflicts` | 22.99ms | 100.00ms | 23.0% | 2.05% | PASS |
| `merge_data_conflicts` | 128.07ms | 250.00ms | 51.2% | 0.53% | PASS |
| `merge_data_conflicts_with_resolve` | 127.69ms | 250.00ms | 51.1% | 0.48% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
