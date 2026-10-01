# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-01 18:17 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260927.320.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/36893580787)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 45s | 8.86s | 11.38s | 1.285× | 1.49% | **PASS** |
| textpk | 69 | 55 | 1h 34m 55s | 10.77s | 12.04s | 1.117× | 2.08% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 7s | 9.66s | 11.69s | 1.209× | 1.94% | **PASS** |
| compositepk | 69 | 55 | 1h 24m 16s | 10.14s | 11.66s | 1.150× | 1.07% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.27ms | 30.15ms | 1.193× | 1.42% | PASS |
| mem_reads | `oltp_range_select` | 10.53ms | 13.25ms | 1.259× | 1.00% | PASS |
| mem_reads | `oltp_sum_range` | 9.77ms | 12.39ms | 1.269× | 1.08% | PASS |
| mem_reads | `oltp_order_range` | 2.62ms | 3.02ms | 1.149× | 1.41% | PASS |
| mem_reads | `oltp_distinct_range` | 3.68ms | 4.06ms | 1.105× | 1.34% | PASS |
| mem_reads | `oltp_index_scan` | 3.99ms | 5.37ms | 1.346× | 1.38% | PASS |
| mem_reads | `select_random_points` | 10.20ms | 11.30ms | 1.107× | 1.10% | PASS |
| mem_reads | `select_random_ranges` | 3.06ms | 4.01ms | 1.309× | 1.08% | PASS |
| mem_reads | `covering_index_scan` | 4.24ms | 4.37ms | 1.031× | 2.20% | PASS |
| mem_reads | `groupby_scan` | 30.23ms | 33.47ms | 1.107× | 1.02% | PASS |
| mem_reads | `index_join` | 6.17ms | 8.69ms | 1.409× | 1.96% | PASS |
| mem_reads | `index_join_scan` | 3.52ms | 4.63ms | 1.314× | 2.05% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.33s | 1.278× | 0.74% | PASS |
| mem_reads | `table_scan` | 1.21s | 1.41s | 1.167× | 3.13% | PASS |
| mem_reads | `oltp_read_only` | 104.08ms | 124.25ms | 1.194× | 2.61% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.44ms | 253.88ms | 1.399× | 1.07% | PASS |
| mem_writes | `oltp_insert` | 15.55ms | 28.58ms | 1.838× | 0.96% | PASS |
| mem_writes | `oltp_update_index` | 50.71ms | 104.71ms | 2.065× | 1.54% | PASS |
| mem_writes | `oltp_update_non_index` | 34.79ms | 60.23ms | 1.732× | 1.49% | PASS |
| mem_writes | `oltp_delete_insert` | 46.50ms | 80.74ms | 1.736× | 2.10% | PASS |
| mem_writes | `oltp_write_only` | 22.00ms | 44.47ms | 2.021× | 2.23% | PASS |
| mem_writes | `types_delete_insert` | 24.40ms | 40.38ms | 1.655× | 1.21% | PASS |
| mem_writes | `oltp_read_write` | 67.80ms | 110.44ms | 1.629× | 1.87% | PASS |
| file_reads | `oltp_point_select` | 98.72ms | 55.30ms | 0.560× | 0.81% | PASS |
| file_reads | `oltp_range_select` | 18.08ms | 16.00ms | 0.885× | 1.61% | PASS |
| file_reads | `oltp_sum_range` | 17.38ms | 15.31ms | 0.881× | 1.31% | PASS |
| file_reads | `oltp_order_range` | 3.49ms | 3.48ms | 0.995× | 2.45% | PASS |
| file_reads | `oltp_distinct_range` | 4.56ms | 4.56ms | 1.000× | 2.04% | PASS |
| file_reads | `oltp_index_scan` | 11.92ms | 8.48ms | 0.711× | 1.68% | PASS |
| file_reads | `select_random_points` | 18.28ms | 14.26ms | 0.780× | 2.22% | PASS |
| file_reads | `select_random_ranges` | 10.37ms | 6.64ms | 0.641× | 1.16% | PASS |
| file_reads | `covering_index_scan` | 12.23ms | 7.18ms | 0.587× | 1.13% | PASS |
| file_reads | `groupby_scan` | 31.24ms | 34.01ms | 1.089× | 0.91% | PASS |
| file_reads | `index_join` | 10.71ms | 10.50ms | 0.981× | 1.40% | PASS |
| file_reads | `index_join_scan` | 4.60ms | 5.15ms | 1.119× | 1.84% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.32s | 1.279× | 0.42% | PASS |
| file_reads | `table_scan` | 1.17s | 1.39s | 1.187× | 0.73% | PASS |
| file_reads | `oltp_read_only` | 210.96ms | 161.40ms | 0.765× | 1.07% | PASS |
| file_writes | `oltp_bulk_insert` | 196.68ms | 271.61ms | 1.381× | 1.05% | PASS |
| file_writes | `oltp_insert` | 22.15ms | 35.34ms | 1.595× | 2.19% | PASS |
| file_writes | `oltp_update_index` | 75.67ms | 126.01ms | 1.665× | 1.50% | PASS |
| file_writes | `oltp_update_non_index` | 57.48ms | 80.89ms | 1.407× | 1.43% | PASS |
| file_writes | `oltp_delete_insert` | 67.64ms | 98.67ms | 1.459× | 1.54% | PASS |
| file_writes | `oltp_write_only` | 44.16ms | 63.72ms | 1.443× | 1.92% | PASS |
| file_writes | `types_delete_insert` | 39.62ms | 53.26ms | 1.344× | 1.66% | PASS |
| file_writes | `oltp_read_write` | 90.68ms | 128.95ms | 1.422× | 1.65% | PASS |
| ac_reads | `oltp_point_select` | 48.34ms | 54.99ms | 1.137× | 0.94% | PASS |
| ac_reads | `oltp_range_select` | 12.79ms | 15.95ms | 1.247× | 1.24% | PASS |
| ac_reads | `oltp_sum_range` | 12.18ms | 15.20ms | 1.248× | 1.16% | PASS |
| ac_reads | `oltp_order_range` | 2.96ms | 3.40ms | 1.148× | 1.98% | PASS |
| ac_reads | `oltp_distinct_range` | 4.00ms | 4.47ms | 1.117× | 1.41% | PASS |
| ac_reads | `oltp_index_scan` | 6.75ms | 8.38ms | 1.242× | 1.43% | PASS |
| ac_reads | `select_random_points` | 13.24ms | 14.23ms | 1.074× | 1.77% | PASS |
| ac_reads | `select_random_ranges` | 5.70ms | 6.68ms | 1.172× | 1.58% | PASS |
| ac_reads | `covering_index_scan` | 7.25ms | 7.20ms | 0.994× | 1.05% | PASS |
| ac_reads | `groupby_scan` | 30.39ms | 33.94ms | 1.117× | 1.04% | PASS |
| ac_reads | `index_join` | 7.85ms | 10.34ms | 1.317× | 1.66% | PASS |
| ac_reads | `index_join_scan` | 4.00ms | 5.13ms | 1.284× | 2.29% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.32s | 1.277× | 0.51% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.39s | 1.191× | 1.00% | PASS |
| ac_reads | `oltp_read_only` | 138.06ms | 162.03ms | 1.174× | 0.89% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.80ms | 69.42ms | 2.916× | 6.81% | PASS |
| ac_writes | `oltp_insert_ac` | 24.41ms | 85.61ms | 3.508× | 5.89% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.89ms | 100.97ms | 3.620× | 6.84% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.81ms | 78.21ms | 3.429× | 7.45% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.36ms | 90.93ms | 3.585× | 5.86% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.23ms | 93.29ms | 3.426× | 7.81% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.26ms | 83.94ms | 3.460× | 11.10% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.42ms | 96.70ms | 3.179× | 7.56% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.74ms | 38.41ms | 1.250× | 2.66% | PASS |
| mem_reads | `oltp_range_select` | 13.92ms | 14.28ms | 1.026× | 3.47% | PASS |
| mem_reads | `oltp_sum_range` | 12.19ms | 14.33ms | 1.176× | 3.09% | PASS |
| mem_reads | `oltp_order_range` | 2.96ms | 3.17ms | 1.070× | 1.89% | PASS |
| mem_reads | `oltp_distinct_range` | 4.06ms | 4.25ms | 1.047× | 1.10% | PASS |
| mem_reads | `oltp_index_scan` | 4.74ms | 6.51ms | 1.373× | 1.83% | PASS |
| mem_reads | `select_random_points` | 19.00ms | 21.63ms | 1.139× | 2.45% | PASS |
| mem_reads | `select_random_ranges` | 4.19ms | 5.26ms | 1.256× | 1.70% | PASS |
| mem_reads | `covering_index_scan` | 5.33ms | 4.84ms | 0.908× | 4.33% | PASS |
| mem_reads | `groupby_scan` | 32.90ms | 34.78ms | 1.057× | 0.97% | PASS |
| mem_reads | `index_join` | 7.58ms | 9.94ms | 1.311× | 3.28% | PASS |
| mem_reads | `index_join_scan` | 4.79ms | 5.51ms | 1.151× | 4.85% | PASS |
| mem_reads | `types_table_scan` | 1.15s | 1.26s | 1.095× | 2.51% | PASS |
| mem_reads | `table_scan` | 1.49s | 1.41s | 0.946× | 2.56% | PASS |
| mem_reads | `oltp_read_only` | 128.03ms | 140.46ms | 1.097× | 2.13% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.82ms | 363.68ms | 1.542× | 0.89% | PASS |
| mem_writes | `oltp_insert` | 22.26ms | 40.02ms | 1.797× | 2.03% | PASS |
| mem_writes | `oltp_update_index` | 75.13ms | 138.51ms | 1.844× | 1.79% | PASS |
| mem_writes | `oltp_update_non_index` | 51.11ms | 90.38ms | 1.768× | 1.83% | PASS |
| mem_writes | `oltp_delete_insert` | 56.22ms | 109.08ms | 1.940× | 2.37% | PASS |
| mem_writes | `oltp_write_only` | 31.91ms | 65.06ms | 2.039× | 1.46% | PASS |
| mem_writes | `types_delete_insert` | 35.34ms | 58.53ms | 1.656× | 1.22% | PASS |
| mem_writes | `oltp_read_write` | 98.80ms | 152.02ms | 1.539× | 1.33% | PASS |
| file_reads | `oltp_point_select` | 110.30ms | 67.10ms | 0.608× | 0.93% | PASS |
| file_reads | `oltp_range_select` | 23.82ms | 17.70ms | 0.743× | 1.94% | PASS |
| file_reads | `oltp_sum_range` | 22.07ms | 17.85ms | 0.809× | 1.39% | PASS |
| file_reads | `oltp_order_range` | 4.12ms | 3.58ms | 0.868× | 1.78% | PASS |
| file_reads | `oltp_distinct_range` | 5.16ms | 4.69ms | 0.907× | 1.13% | PASS |
| file_reads | `oltp_index_scan` | 13.04ms | 9.47ms | 0.727× | 1.22% | PASS |
| file_reads | `select_random_points` | 28.92ms | 25.90ms | 0.896× | 2.07% | PASS |
| file_reads | `select_random_ranges` | 12.09ms | 7.96ms | 0.658× | 0.93% | PASS |
| file_reads | `covering_index_scan` | 14.23ms | 7.62ms | 0.536× | 2.19% | PASS |
| file_reads | `groupby_scan` | 34.23ms | 35.48ms | 1.036× | 0.85% | PASS |
| file_reads | `index_join` | 13.16ms | 12.07ms | 0.917× | 2.60% | PASS |
| file_reads | `index_join_scan` | 6.20ms | 6.26ms | 1.009× | 3.85% | PASS |
| file_reads | `types_table_scan` | 1.25s | 1.28s | 1.023× | 1.23% | PASS |
| file_reads | `table_scan` | 1.55s | 1.43s | 0.924× | 0.87% | PASS |
| file_reads | `oltp_read_only` | 247.72ms | 184.24ms | 0.744× | 0.97% | PASS |
| file_writes | `oltp_bulk_insert` | 256.54ms | 393.67ms | 1.535× | 0.91% | PASS |
| file_writes | `oltp_insert` | 61.58ms | 53.96ms | 0.876× | 11.64% | PASS |
| file_writes | `oltp_update_index` | 127.53ms | 180.25ms | 1.413× | 2.04% | PASS |
| file_writes | `oltp_update_non_index` | 114.78ms | 119.04ms | 1.037× | 6.75% | PASS |
| file_writes | `oltp_delete_insert` | 95.99ms | 136.23ms | 1.419× | 1.49% | PASS |
| file_writes | `oltp_write_only` | 87.36ms | 86.50ms | 0.990× | 9.09% | PASS |
| file_writes | `types_delete_insert` | 58.09ms | 76.07ms | 1.310× | 2.01% | PASS |
| file_writes | `oltp_read_write` | 138.26ms | 167.61ms | 1.212× | 4.81% | PASS |
| ac_reads | `oltp_point_select` | 55.95ms | 64.55ms | 1.154× | 1.27% | PASS |
| ac_reads | `oltp_range_select` | 17.03ms | 17.18ms | 1.009× | 3.76% | PASS |
| ac_reads | `oltp_sum_range` | 15.52ms | 17.25ms | 1.112× | 2.27% | PASS |
| ac_reads | `oltp_order_range` | 3.45ms | 3.56ms | 1.031× | 1.76% | PASS |
| ac_reads | `oltp_distinct_range` | 4.51ms | 4.63ms | 1.026× | 1.45% | PASS |
| ac_reads | `oltp_index_scan` | 7.61ms | 9.30ms | 1.221× | 2.04% | PASS |
| ac_reads | `select_random_points` | 21.84ms | 24.63ms | 1.128× | 2.08% | PASS |
| ac_reads | `select_random_ranges` | 6.88ms | 7.87ms | 1.144× | 1.76% | PASS |
| ac_reads | `covering_index_scan` | 8.18ms | 7.44ms | 0.909× | 4.36% | PASS |
| ac_reads | `groupby_scan` | 32.76ms | 35.05ms | 1.070× | 1.07% | PASS |
| ac_reads | `index_join` | 10.19ms | 11.78ms | 1.157× | 3.32% | PASS |
| ac_reads | `index_join_scan` | 5.56ms | 6.13ms | 1.102× | 4.96% | PASS |
| ac_reads | `types_table_scan` | 1.14s | 1.26s | 1.099× | 3.96% | PASS |
| ac_reads | `table_scan` | 1.29s | 1.38s | 1.073× | 3.50% | PASS |
| ac_reads | `oltp_read_only` | 160.22ms | 178.13ms | 1.112× | 2.08% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.96ms | 68.46ms | 2.982× | 6.14% | PASS |
| ac_writes | `oltp_insert_ac` | 26.00ms | 82.71ms | 3.181× | 5.89% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.19ms | 102.71ms | 3.519× | 7.39% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.78ms | 82.52ms | 3.471× | 6.56% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.48ms | 92.79ms | 3.642× | 6.18% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.31ms | 91.49ms | 3.614× | 5.09% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.27ms | 84.84ms | 3.496× | 6.04% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.83ms | 100.37ms | 3.154× | 6.87% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.43ms | 36.60ms | 1.244× | 1.59% | PASS |
| mem_reads | `oltp_range_select` | 12.59ms | 14.02ms | 1.113× | 2.60% | PASS |
| mem_reads | `oltp_sum_range` | 11.76ms | 13.88ms | 1.180× | 2.51% | PASS |
| mem_reads | `oltp_order_range` | 2.90ms | 3.14ms | 1.084× | 1.94% | PASS |
| mem_reads | `oltp_distinct_range` | 4.03ms | 4.24ms | 1.052× | 1.25% | PASS |
| mem_reads | `oltp_index_scan` | 4.50ms | 6.18ms | 1.374× | 2.34% | PASS |
| mem_reads | `select_random_points` | 17.66ms | 20.79ms | 1.177× | 2.95% | PASS |
| mem_reads | `select_random_ranges` | 3.93ms | 5.12ms | 1.303× | 1.78% | PASS |
| mem_reads | `covering_index_scan` | 4.38ms | 4.40ms | 1.007× | 1.61% | PASS |
| mem_reads | `groupby_scan` | 31.55ms | 34.36ms | 1.089× | 0.97% | PASS |
| mem_reads | `index_join` | 6.80ms | 9.27ms | 1.363× | 2.36% | PASS |
| mem_reads | `index_join_scan` | 4.21ms | 5.46ms | 1.298× | 2.73% | PASS |
| mem_reads | `types_table_scan` | 1.10s | 1.24s | 1.137× | 1.68% | PASS |
| mem_reads | `table_scan` | 1.28s | 1.39s | 1.082× | 2.68% | PASS |
| mem_reads | `oltp_read_only` | 118.26ms | 135.38ms | 1.145× | 2.35% | PASS |
| mem_writes | `oltp_bulk_insert` | 237.19ms | 350.47ms | 1.478× | 0.82% | PASS |
| mem_writes | `oltp_insert` | 19.85ms | 39.23ms | 1.976× | 1.67% | PASS |
| mem_writes | `oltp_update_index` | 67.99ms | 131.12ms | 1.929× | 4.15% | PASS |
| mem_writes | `oltp_update_non_index` | 47.51ms | 83.98ms | 1.768× | 1.18% | PASS |
| mem_writes | `oltp_delete_insert` | 47.69ms | 101.57ms | 2.130× | 1.10% | PASS |
| mem_writes | `oltp_write_only` | 27.44ms | 60.92ms | 2.220× | 1.09% | PASS |
| mem_writes | `types_delete_insert` | 31.55ms | 53.41ms | 1.693× | 1.38% | PASS |
| mem_writes | `oltp_read_write` | 82.04ms | 137.81ms | 1.680× | 1.15% | PASS |
| file_reads | `oltp_point_select` | 104.79ms | 62.39ms | 0.595× | 1.16% | PASS |
| file_reads | `oltp_range_select` | 20.82ms | 16.80ms | 0.807× | 2.43% | PASS |
| file_reads | `oltp_sum_range` | 19.96ms | 16.79ms | 0.841× | 1.90% | PASS |
| file_reads | `oltp_order_range` | 3.83ms | 3.52ms | 0.920× | 1.81% | PASS |
| file_reads | `oltp_distinct_range` | 4.98ms | 4.64ms | 0.932× | 1.76% | PASS |
| file_reads | `oltp_index_scan` | 12.44ms | 9.09ms | 0.731× | 1.93% | PASS |
| file_reads | `select_random_points` | 27.29ms | 24.46ms | 0.896× | 3.00% | PASS |
| file_reads | `select_random_ranges` | 11.74ms | 7.82ms | 0.667× | 1.32% | PASS |
| file_reads | `covering_index_scan` | 12.58ms | 7.31ms | 0.581× | 2.16% | PASS |
| file_reads | `groupby_scan` | 32.78ms | 34.85ms | 1.063× | 1.25% | PASS |
| file_reads | `index_join` | 11.56ms | 11.36ms | 0.983× | 3.12% | PASS |
| file_reads | `index_join_scan` | 5.38ms | 5.98ms | 1.111× | 2.95% | PASS |
| file_reads | `types_table_scan` | 1.08s | 1.24s | 1.144× | 1.69% | PASS |
| file_reads | `table_scan` | 1.25s | 1.38s | 1.099× | 1.33% | PASS |
| file_reads | `oltp_read_only` | 235.28ms | 175.62ms | 0.746× | 1.24% | PASS |
| file_writes | `oltp_bulk_insert` | 255.41ms | 375.52ms | 1.470× | 0.86% | PASS |
| file_writes | `oltp_insert` | 31.61ms | 51.53ms | 1.630× | 2.78% | PASS |
| file_writes | `oltp_update_index` | 102.25ms | 161.35ms | 1.578× | 1.62% | PASS |
| file_writes | `oltp_update_non_index` | 77.81ms | 107.34ms | 1.379× | 2.16% | PASS |
| file_writes | `oltp_delete_insert` | 79.85ms | 128.68ms | 1.611× | 1.95% | PASS |
| file_writes | `oltp_write_only` | 54.90ms | 83.55ms | 1.522× | 2.05% | PASS |
| file_writes | `types_delete_insert` | 51.07ms | 71.34ms | 1.397× | 2.70% | PASS |
| file_writes | `oltp_read_write` | 114.58ms | 161.78ms | 1.412× | 1.83% | PASS |
| ac_reads | `oltp_point_select` | 55.42ms | 62.84ms | 1.134× | 1.14% | PASS |
| ac_reads | `oltp_range_select` | 16.10ms | 16.81ms | 1.044× | 2.28% | PASS |
| ac_reads | `oltp_sum_range` | 15.66ms | 16.99ms | 1.085× | 3.14% | PASS |
| ac_reads | `oltp_order_range` | 3.35ms | 3.52ms | 1.053× | 1.30% | PASS |
| ac_reads | `oltp_distinct_range` | 4.51ms | 4.63ms | 1.027× | 1.18% | PASS |
| ac_reads | `oltp_index_scan` | 7.47ms | 9.09ms | 1.217× | 1.80% | PASS |
| ac_reads | `select_random_points` | 22.14ms | 24.66ms | 1.114× | 3.54% | PASS |
| ac_reads | `select_random_ranges` | 6.92ms | 7.85ms | 1.135× | 1.79% | PASS |
| ac_reads | `covering_index_scan` | 7.64ms | 7.32ms | 0.958× | 2.30% | PASS |
| ac_reads | `groupby_scan` | 32.50ms | 34.98ms | 1.077× | 1.28% | PASS |
| ac_reads | `index_join` | 9.01ms | 11.40ms | 1.265× | 3.47% | PASS |
| ac_reads | `index_join_scan` | 4.88ms | 5.99ms | 1.230× | 3.07% | PASS |
| ac_reads | `types_table_scan` | 1.08s | 1.23s | 1.146× | 1.43% | PASS |
| ac_reads | `table_scan` | 1.25s | 1.38s | 1.102× | 2.12% | PASS |
| ac_reads | `oltp_read_only` | 159.99ms | 175.00ms | 1.094× | 1.52% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.49ms | 63.67ms | 2.963× | 6.22% | PASS |
| ac_writes | `oltp_insert_ac` | 24.88ms | 85.61ms | 3.441× | 5.74% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.74ms | 95.75ms | 3.719× | 3.80% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.45ms | 79.78ms | 3.264× | 5.63% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.37ms | 88.21ms | 3.620× | 7.11% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.66ms | 86.72ms | 3.516× | 5.17% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.84ms | 80.55ms | 3.866× | 7.74% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.64ms | 94.02ms | 3.171× | 4.53% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.90ms | 33.67ms | 1.126× | 1.56% | PASS |
| mem_reads | `oltp_range_select` | 18.50ms | 19.36ms | 1.047× | 1.27% | PASS |
| mem_reads | `oltp_sum_range` | 17.55ms | 18.11ms | 1.032× | 1.22% | PASS |
| mem_reads | `oltp_order_range` | 3.52ms | 3.67ms | 1.041× | 1.05% | PASS |
| mem_reads | `oltp_distinct_range` | 4.51ms | 4.62ms | 1.024× | 0.85% | PASS |
| mem_reads | `oltp_index_scan` | 4.29ms | 5.58ms | 1.299× | 1.77% | PASS |
| mem_reads | `select_random_points` | 27.84ms | 30.69ms | 1.102× | 1.35% | PASS |
| mem_reads | `select_random_ranges` | 6.81ms | 7.69ms | 1.129× | 1.16% | PASS |
| mem_reads | `covering_index_scan` | 3.57ms | 3.89ms | 1.088× | 2.00% | PASS |
| mem_reads | `groupby_scan` | 36.12ms | 36.72ms | 1.017× | 0.31% | PASS |
| mem_reads | `index_join` | 7.55ms | 10.19ms | 1.349× | 1.25% | PASS |
| mem_reads | `index_join_scan` | 3.71ms | 5.26ms | 1.418× | 1.63% | PASS |
| mem_reads | `types_table_scan` | 1.10s | 1.25s | 1.141× | 0.59% | PASS |
| mem_reads | `table_scan` | 1.32s | 1.37s | 1.038× | 0.84% | PASS |
| mem_reads | `oltp_read_only` | 141.39ms | 150.64ms | 1.065× | 0.82% | PASS |
| mem_writes | `oltp_bulk_insert` | 205.48ms | 284.86ms | 1.386× | 0.27% | PASS |
| mem_writes | `oltp_insert` | 16.68ms | 30.23ms | 1.812× | 0.43% | PASS |
| mem_writes | `oltp_update_index` | 60.82ms | 108.72ms | 1.788× | 1.37% | PASS |
| mem_writes | `oltp_update_non_index` | 43.85ms | 72.06ms | 1.643× | 1.09% | PASS |
| mem_writes | `oltp_delete_insert` | 44.10ms | 84.17ms | 1.908× | 1.03% | PASS |
| mem_writes | `oltp_write_only` | 23.72ms | 50.17ms | 2.115× | 1.01% | PASS |
| mem_writes | `types_delete_insert` | 27.88ms | 46.70ms | 1.675× | 1.19% | PASS |
| mem_writes | `oltp_read_write` | 90.57ms | 132.21ms | 1.460× | 2.19% | PASS |
| file_reads | `oltp_point_select` | 61.53ms | 44.05ms | 0.716× | 0.92% | PASS |
| file_reads | `oltp_range_select` | 21.70ms | 20.67ms | 0.953× | 1.19% | PASS |
| file_reads | `oltp_sum_range` | 21.14ms | 19.44ms | 0.919× | 1.82% | PASS |
| file_reads | `oltp_order_range` | 3.90ms | 3.80ms | 0.972× | 0.82% | PASS |
| file_reads | `oltp_distinct_range` | 4.92ms | 4.76ms | 0.968× | 0.56% | PASS |
| file_reads | `oltp_index_scan` | 7.74ms | 6.77ms | 0.875× | 0.75% | PASS |
| file_reads | `select_random_points` | 31.02ms | 31.66ms | 1.021× | 1.03% | PASS |
| file_reads | `select_random_ranges` | 10.20ms | 8.84ms | 0.867× | 0.68% | PASS |
| file_reads | `covering_index_scan` | 7.01ms | 5.13ms | 0.733× | 0.81% | PASS |
| file_reads | `groupby_scan` | 36.57ms | 36.91ms | 1.009× | 0.42% | PASS |
| file_reads | `index_join` | 9.52ms | 11.15ms | 1.171× | 1.04% | PASS |
| file_reads | `index_join_scan` | 4.16ms | 5.52ms | 1.325× | 1.15% | PASS |
| file_reads | `types_table_scan` | 1.13s | 1.26s | 1.113× | 0.93% | PASS |
| file_reads | `table_scan` | 1.36s | 1.38s | 1.016× | 0.70% | PASS |
| file_reads | `oltp_read_only` | 189.31ms | 166.84ms | 0.881× | 0.62% | PASS |
| file_writes | `oltp_bulk_insert` | 249.88ms | 339.67ms | 1.359× | 2.91% | PASS |
| file_writes | `oltp_insert` | 29.44ms | 39.33ms | 1.336× | 2.79% | PASS |
| file_writes | `oltp_update_index` | 151.20ms | 169.85ms | 1.123× | 1.01% | PASS |
| file_writes | `oltp_update_non_index` | 120.85ms | 119.40ms | 0.988× | 6.59% | PASS |
| file_writes | `oltp_delete_insert` | 127.47ms | 139.66ms | 1.096× | 4.16% | PASS |
| file_writes | `oltp_write_only` | 80.97ms | 99.11ms | 1.224× | 0.61% | PASS |
| file_writes | `types_delete_insert` | 80.19ms | 80.62ms | 1.005× | 2.26% | PASS |
| file_writes | `oltp_read_write` | 150.82ms | 179.25ms | 1.188× | 0.66% | PASS |
| ac_reads | `oltp_point_select` | 39.69ms | 44.01ms | 1.109× | 1.20% | PASS |
| ac_reads | `oltp_range_select` | 19.45ms | 20.62ms | 1.060× | 0.95% | PASS |
| ac_reads | `oltp_sum_range` | 18.38ms | 19.35ms | 1.053× | 1.05% | PASS |
| ac_reads | `oltp_order_range` | 3.69ms | 3.79ms | 1.027× | 0.46% | PASS |
| ac_reads | `oltp_distinct_range` | 4.69ms | 4.76ms | 1.016× | 0.49% | PASS |
| ac_reads | `oltp_index_scan` | 5.65ms | 6.77ms | 1.199× | 0.78% | PASS |
| ac_reads | `select_random_points` | 28.73ms | 31.73ms | 1.104× | 1.55% | PASS |
| ac_reads | `select_random_ranges` | 8.06ms | 8.83ms | 1.095× | 0.74% | PASS |
| ac_reads | `covering_index_scan` | 4.86ms | 5.16ms | 1.061× | 1.07% | PASS |
| ac_reads | `groupby_scan` | 36.28ms | 36.88ms | 1.017× | 0.48% | PASS |
| ac_reads | `index_join` | 8.51ms | 11.12ms | 1.306× | 1.16% | PASS |
| ac_reads | `index_join_scan` | 4.00ms | 5.54ms | 1.386× | 1.50% | PASS |
| ac_reads | `types_table_scan` | 1.09s | 1.25s | 1.147× | 0.63% | PASS |
| ac_reads | `table_scan` | 1.33s | 1.37s | 1.028× | 1.08% | PASS |
| ac_reads | `oltp_read_only` | 157.18ms | 166.65ms | 1.060× | 0.73% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 28.89ms | 74.29ms | 2.571× | 3.39% | PASS |
| ac_writes | `oltp_insert_ac` | 32.08ms | 95.25ms | 2.969× | 3.29% | PASS |
| ac_writes | `oltp_update_index_ac` | 34.30ms | 105.62ms | 3.079× | 3.81% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 28.97ms | 85.16ms | 2.939× | 4.68% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 31.31ms | 99.36ms | 3.173× | 3.99% | PASS |
| ac_writes | `oltp_write_only_ac` | 30.73ms | 96.29ms | 3.133× | 4.10% | PASS |
| ac_writes | `types_delete_insert_ac` | 32.98ms | 86.19ms | 2.614× | 9.40% | PASS |
| ac_writes | `oltp_read_write_ac` | 36.27ms | 104.00ms | 2.867× | 4.02% | PASS |

</details>

## Version-control latency

Wall time: 2m 20s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.05ms | 200.00ms | 41.5% | 0.29% | PASS |
| `status_dirty_many_tables` | 86.17ms | 200.00ms | 43.1% | 0.34% | PASS |
| `diff_regular_working_one_table` | 78.88ms | 150.00ms | 52.6% | 0.37% | PASS |
| `diff_regular_working_many_tables` | 91.69ms | 200.00ms | 45.8% | 0.39% | PASS |
| `diff_stat_working_many_tables` | 91.90ms | 200.00ms | 45.9% | 0.35% | PASS |
| `diff_schema_working_many_tables` | 92.14ms | 200.00ms | 46.1% | 0.45% | PASS |
| `branch_list_many_branches` | 22.66ms | 100.00ms | 22.7% | 1.10% | PASS |
| `branch_create_delete` | 24.64ms | 100.00ms | 24.6% | 1.01% | PASS |
| `checkout_branch_clean` | 55.11ms | 200.00ms | 27.6% | 0.81% | PASS |
| `merge_data_no_conflicts` | 28.82ms | 150.00ms | 19.2% | 1.13% | PASS |
| `merge_schema_no_conflicts` | 22.33ms | 100.00ms | 22.3% | 1.76% | PASS |
| `merge_data_conflicts` | 127.14ms | 250.00ms | 50.9% | 0.27% | PASS |
| `merge_data_conflicts_with_resolve` | 127.30ms | 250.00ms | 50.9% | 0.33% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
