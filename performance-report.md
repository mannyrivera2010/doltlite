# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-18 11:38 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260810.271.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/32124438678)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 30s | 9.52s | 11.11s | 1.167× | 1.50% | **PASS** |
| textpk | 69 | 55 | 1h 35m 52s | 11.18s | 12.25s | 1.096× | 1.81% | **PASS** |
| blobpk | 69 | 55 | 1h 25m 21s | 8.48s | 9.77s | 1.153× | 1.13% | **PASS** |
| compositepk | 69 | 55 | 1h 26m 32s | 9.49s | 12.00s | 1.265× | 1.42% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.95ms | 27.81ms | 1.115× | 2.11% | PASS |
| mem_reads | `oltp_range_select` | 10.75ms | 11.97ms | 1.113× | 2.35% | PASS |
| mem_reads | `oltp_sum_range` | 9.64ms | 11.35ms | 1.177× | 1.20% | PASS |
| mem_reads | `oltp_order_range` | 2.66ms | 2.85ms | 1.071× | 1.06% | PASS |
| mem_reads | `oltp_distinct_range` | 3.77ms | 3.89ms | 1.032× | 1.08% | PASS |
| mem_reads | `oltp_index_scan` | 4.00ms | 4.94ms | 1.235× | 1.70% | PASS |
| mem_reads | `select_random_points` | 11.02ms | 11.29ms | 1.025× | 4.12% | PASS |
| mem_reads | `select_random_ranges` | 3.14ms | 4.00ms | 1.274× | 1.58% | PASS |
| mem_reads | `covering_index_scan` | 4.37ms | 4.11ms | 0.941× | 1.53% | PASS |
| mem_reads | `groupby_scan` | 31.81ms | 34.41ms | 1.082× | 0.73% | PASS |
| mem_reads | `index_join` | 5.91ms | 7.89ms | 1.335× | 2.04% | PASS |
| mem_reads | `index_join_scan` | 3.55ms | 4.69ms | 1.321× | 1.78% | PASS |
| mem_reads | `types_table_scan` | 1.11s | 1.27s | 1.141× | 0.60% | PASS |
| mem_reads | `table_scan` | 1.28s | 1.38s | 1.073× | 1.71% | PASS |
| mem_reads | `oltp_read_only` | 111.88ms | 117.95ms | 1.054× | 1.58% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.94ms | 240.32ms | 1.328× | 1.20% | PASS |
| mem_writes | `oltp_insert` | 15.89ms | 28.28ms | 1.780× | 0.86% | PASS |
| mem_writes | `oltp_update_index` | 54.16ms | 111.03ms | 2.050× | 1.66% | PASS |
| mem_writes | `oltp_update_non_index` | 35.46ms | 58.89ms | 1.661× | 1.49% | PASS |
| mem_writes | `oltp_delete_insert` | 45.17ms | 78.21ms | 1.731× | 1.67% | PASS |
| mem_writes | `oltp_write_only` | 22.79ms | 45.45ms | 1.994× | 2.18% | PASS |
| mem_writes | `types_delete_insert` | 25.62ms | 40.14ms | 1.567× | 2.34% | PASS |
| mem_writes | `oltp_read_write` | 68.44ms | 106.40ms | 1.555× | 2.60% | PASS |
| file_reads | `oltp_point_select` | 120.14ms | 59.12ms | 0.492× | 0.88% | PASS |
| file_reads | `oltp_range_select` | 20.71ms | 15.26ms | 0.737× | 1.56% | PASS |
| file_reads | `oltp_sum_range` | 20.18ms | 14.94ms | 0.740× | 2.21% | PASS |
| file_reads | `oltp_order_range` | 3.73ms | 3.25ms | 0.872× | 1.48% | PASS |
| file_reads | `oltp_distinct_range` | 4.83ms | 4.27ms | 0.885× | 0.87% | PASS |
| file_reads | `oltp_index_scan` | 13.56ms | 8.37ms | 0.617× | 1.86% | PASS |
| file_reads | `select_random_points` | 19.87ms | 14.42ms | 0.726× | 2.89% | PASS |
| file_reads | `select_random_ranges` | 12.53ms | 7.16ms | 0.571× | 1.31% | PASS |
| file_reads | `covering_index_scan` | 13.99ms | 7.46ms | 0.533× | 1.53% | PASS |
| file_reads | `groupby_scan` | 32.62ms | 34.75ms | 1.066× | 0.95% | PASS |
| file_reads | `index_join` | 10.99ms | 9.84ms | 0.896× | 1.16% | PASS |
| file_reads | `index_join_scan` | 4.54ms | 5.02ms | 1.107× | 1.40% | PASS |
| file_reads | `types_table_scan` | 1.10s | 1.27s | 1.146× | 0.73% | PASS |
| file_reads | `table_scan` | 1.27s | 1.38s | 1.084× | 1.33% | PASS |
| file_reads | `oltp_read_only` | 240.80ms | 160.09ms | 0.665× | 0.78% | PASS |
| file_writes | `oltp_bulk_insert` | 195.66ms | 260.75ms | 1.333× | 0.94% | PASS |
| file_writes | `oltp_insert` | 22.24ms | 36.03ms | 1.620× | 1.97% | PASS |
| file_writes | `oltp_update_index` | 79.79ms | 129.78ms | 1.627× | 1.40% | PASS |
| file_writes | `oltp_update_non_index` | 60.28ms | 81.97ms | 1.360× | 1.17% | PASS |
| file_writes | `oltp_delete_insert` | 68.53ms | 98.82ms | 1.442× | 1.50% | PASS |
| file_writes | `oltp_write_only` | 44.89ms | 65.98ms | 1.470× | 2.39% | PASS |
| file_writes | `types_delete_insert` | 40.88ms | 53.49ms | 1.308× | 0.98% | PASS |
| file_writes | `oltp_read_write` | 91.24ms | 126.47ms | 1.386× | 1.92% | PASS |
| ac_reads | `oltp_point_select` | 56.39ms | 59.31ms | 1.052× | 0.85% | PASS |
| ac_reads | `oltp_range_select` | 13.59ms | 15.06ms | 1.108× | 1.63% | PASS |
| ac_reads | `oltp_sum_range` | 12.84ms | 14.63ms | 1.140× | 0.87% | PASS |
| ac_reads | `oltp_order_range` | 3.01ms | 3.23ms | 1.071× | 1.93% | PASS |
| ac_reads | `oltp_distinct_range` | 4.07ms | 4.24ms | 1.040× | 0.80% | PASS |
| ac_reads | `oltp_index_scan` | 7.18ms | 8.30ms | 1.155× | 1.11% | PASS |
| ac_reads | `select_random_points` | 13.85ms | 14.49ms | 1.046× | 1.96% | PASS |
| ac_reads | `select_random_ranges` | 6.26ms | 7.16ms | 1.145× | 0.98% | PASS |
| ac_reads | `covering_index_scan` | 7.66ms | 7.44ms | 0.971× | 1.11% | PASS |
| ac_reads | `groupby_scan` | 31.98ms | 34.77ms | 1.087× | 0.84% | PASS |
| ac_reads | `index_join` | 7.73ms | 9.83ms | 1.271× | 0.86% | PASS |
| ac_reads | `index_join_scan` | 3.96ms | 5.04ms | 1.272× | 0.88% | PASS |
| ac_reads | `types_table_scan` | 1.10s | 1.26s | 1.142× | 0.86% | PASS |
| ac_reads | `table_scan` | 1.38s | 1.40s | 1.019× | 1.28% | PASS |
| ac_reads | `oltp_read_only` | 150.30ms | 160.94ms | 1.071× | 1.44% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.82ms | 65.59ms | 3.900× | 5.52% | PASS |
| ac_writes | `oltp_insert_ac` | 18.42ms | 80.57ms | 4.373× | 3.87% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.82ms | 97.76ms | 4.931× | 5.71% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 15.91ms | 72.18ms | 4.537× | 3.14% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.75ms | 87.51ms | 4.930× | 3.16% | PASS |
| ac_writes | `oltp_write_only_ac` | 17.79ms | 85.13ms | 4.786× | 4.92% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.64ms | 74.47ms | 4.761× | 3.70% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.06ms | 93.57ms | 3.889× | 4.06% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.85ms | 38.49ms | 1.248× | 1.63% | PASS |
| mem_reads | `oltp_range_select` | 14.24ms | 14.32ms | 1.005× | 2.62% | PASS |
| mem_reads | `oltp_sum_range` | 12.89ms | 14.46ms | 1.122× | 3.06% | PASS |
| mem_reads | `oltp_order_range` | 3.07ms | 3.20ms | 1.040× | 1.36% | PASS |
| mem_reads | `oltp_distinct_range` | 4.10ms | 4.26ms | 1.040× | 1.23% | PASS |
| mem_reads | `oltp_index_scan` | 4.76ms | 6.59ms | 1.384× | 2.30% | PASS |
| mem_reads | `select_random_points` | 18.52ms | 21.36ms | 1.154× | 1.77% | PASS |
| mem_reads | `select_random_ranges` | 4.14ms | 5.25ms | 1.267× | 2.17% | PASS |
| mem_reads | `covering_index_scan` | 5.08ms | 4.81ms | 0.946× | 4.34% | PASS |
| mem_reads | `groupby_scan` | 32.71ms | 34.05ms | 1.041× | 0.95% | PASS |
| mem_reads | `index_join` | 7.25ms | 9.67ms | 1.334× | 2.89% | PASS |
| mem_reads | `index_join_scan` | 4.97ms | 5.53ms | 1.111× | 5.59% | PASS |
| mem_reads | `types_table_scan` | 1.20s | 1.27s | 1.053× | 1.45% | PASS |
| mem_reads | `table_scan` | 1.50s | 1.41s | 0.939× | 1.47% | PASS |
| mem_reads | `oltp_read_only` | 128.05ms | 140.68ms | 1.099× | 1.65% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.55ms | 360.57ms | 1.524× | 1.03% | PASS |
| mem_writes | `oltp_insert` | 22.87ms | 41.17ms | 1.801× | 1.48% | PASS |
| mem_writes | `oltp_update_index` | 75.78ms | 140.54ms | 1.855× | 1.79% | PASS |
| mem_writes | `oltp_update_non_index` | 50.74ms | 89.74ms | 1.769× | 1.47% | PASS |
| mem_writes | `oltp_delete_insert` | 54.36ms | 108.48ms | 1.996× | 1.79% | PASS |
| mem_writes | `oltp_write_only` | 30.43ms | 64.11ms | 2.107× | 2.67% | PASS |
| mem_writes | `types_delete_insert` | 33.83ms | 57.21ms | 1.691× | 1.33% | PASS |
| mem_writes | `oltp_read_write` | 92.84ms | 146.13ms | 1.574× | 2.02% | PASS |
| file_reads | `oltp_point_select` | 107.72ms | 65.27ms | 0.606× | 0.86% | PASS |
| file_reads | `oltp_range_select` | 22.78ms | 17.24ms | 0.757× | 2.80% | PASS |
| file_reads | `oltp_sum_range` | 21.57ms | 17.48ms | 0.810× | 2.04% | PASS |
| file_reads | `oltp_order_range` | 4.08ms | 3.57ms | 0.873× | 2.33% | PASS |
| file_reads | `oltp_distinct_range` | 5.12ms | 4.64ms | 0.906× | 1.33% | PASS |
| file_reads | `oltp_index_scan` | 12.79ms | 9.39ms | 0.734× | 1.02% | PASS |
| file_reads | `select_random_points` | 28.11ms | 25.31ms | 0.900× | 2.24% | PASS |
| file_reads | `select_random_ranges` | 12.07ms | 7.98ms | 0.661× | 1.24% | PASS |
| file_reads | `covering_index_scan` | 14.04ms | 7.54ms | 0.537× | 2.07% | PASS |
| file_reads | `groupby_scan` | 33.91ms | 34.56ms | 1.019× | 1.08% | PASS |
| file_reads | `index_join` | 12.62ms | 11.63ms | 0.921× | 2.83% | PASS |
| file_reads | `index_join_scan` | 5.83ms | 6.04ms | 1.037× | 2.90% | PASS |
| file_reads | `types_table_scan` | 1.21s | 1.26s | 1.045× | 1.61% | PASS |
| file_reads | `table_scan` | 1.51s | 1.41s | 0.933× | 1.49% | PASS |
| file_reads | `oltp_read_only` | 244.10ms | 181.84ms | 0.745× | 1.17% | PASS |
| file_writes | `oltp_bulk_insert` | 256.48ms | 391.59ms | 1.527× | 0.92% | PASS |
| file_writes | `oltp_insert` | 55.59ms | 54.12ms | 0.973× | 24.71% | PASS |
| file_writes | `oltp_update_index` | 121.80ms | 178.00ms | 1.461× | 1.70% | PASS |
| file_writes | `oltp_update_non_index` | 106.48ms | 117.92ms | 1.107× | 13.25% | PASS |
| file_writes | `oltp_delete_insert` | 96.86ms | 139.81ms | 1.444× | 1.81% | PASS |
| file_writes | `oltp_write_only` | 86.09ms | 89.34ms | 1.038× | 12.12% | PASS |
| file_writes | `types_delete_insert` | 58.69ms | 77.96ms | 1.328× | 1.68% | PASS |
| file_writes | `oltp_read_write` | 138.68ms | 171.21ms | 1.235× | 4.09% | PASS |
| ac_reads | `oltp_point_select` | 56.52ms | 65.13ms | 1.152× | 1.01% | PASS |
| ac_reads | `oltp_range_select` | 17.66ms | 17.30ms | 0.980× | 2.51% | PASS |
| ac_reads | `oltp_sum_range` | 16.26ms | 17.63ms | 1.084× | 2.24% | PASS |
| ac_reads | `oltp_order_range` | 3.51ms | 3.56ms | 1.013× | 1.30% | PASS |
| ac_reads | `oltp_distinct_range` | 4.55ms | 4.64ms | 1.021× | 1.18% | PASS |
| ac_reads | `oltp_index_scan` | 7.83ms | 9.47ms | 1.209× | 1.34% | PASS |
| ac_reads | `select_random_points` | 22.68ms | 25.68ms | 1.132× | 2.13% | PASS |
| ac_reads | `select_random_ranges` | 7.03ms | 8.01ms | 1.139× | 1.18% | PASS |
| ac_reads | `covering_index_scan` | 8.83ms | 7.56ms | 0.856× | 2.72% | PASS |
| ac_reads | `groupby_scan` | 33.39ms | 34.69ms | 1.039× | 0.92% | PASS |
| ac_reads | `index_join` | 9.88ms | 11.78ms | 1.192× | 2.48% | PASS |
| ac_reads | `index_join_scan` | 5.47ms | 6.19ms | 1.132× | 3.85% | PASS |
| ac_reads | `types_table_scan` | 1.29s | 1.28s | 0.994× | 1.04% | PASS |
| ac_reads | `table_scan` | 1.57s | 1.43s | 0.912× | 0.67% | PASS |
| ac_reads | `oltp_read_only` | 173.42ms | 185.20ms | 1.068× | 0.87% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 25.05ms | 89.98ms | 3.592× | 6.21% | PASS |
| ac_writes | `oltp_insert_ac` | 29.08ms | 106.58ms | 3.665× | 7.26% | PASS |
| ac_writes | `oltp_update_index_ac` | 31.56ms | 127.22ms | 4.031× | 6.51% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.07ms | 99.75ms | 4.144× | 9.27% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 28.11ms | 114.23ms | 4.064× | 6.87% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.95ms | 114.63ms | 4.102× | 5.06% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.23ms | 104.11ms | 4.298× | 10.44% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.37ms | 120.91ms | 3.623× | 5.59% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.84ms | 27.27ms | 1.144× | 1.85% | PASS |
| mem_reads | `oltp_range_select` | 10.75ms | 10.78ms | 1.002× | 1.79% | PASS |
| mem_reads | `oltp_sum_range` | 9.58ms | 10.37ms | 1.083× | 0.96% | PASS |
| mem_reads | `oltp_order_range` | 2.41ms | 2.48ms | 1.032× | 2.12% | PASS |
| mem_reads | `oltp_distinct_range` | 3.26ms | 3.27ms | 1.004× | 1.26% | PASS |
| mem_reads | `oltp_index_scan` | 3.68ms | 4.66ms | 1.266× | 1.41% | PASS |
| mem_reads | `select_random_points` | 13.88ms | 15.99ms | 1.152× | 1.20% | PASS |
| mem_reads | `select_random_ranges` | 3.41ms | 4.16ms | 1.220× | 1.13% | PASS |
| mem_reads | `covering_index_scan` | 3.56ms | 3.45ms | 0.969× | 2.42% | PASS |
| mem_reads | `groupby_scan` | 26.39ms | 27.62ms | 1.047× | 0.67% | PASS |
| mem_reads | `index_join` | 5.51ms | 7.09ms | 1.287× | 1.67% | PASS |
| mem_reads | `index_join_scan` | 3.63ms | 4.73ms | 1.300× | 1.76% | PASS |
| mem_reads | `types_table_scan` | 874.12ms | 969.51ms | 1.109× | 0.64% | PASS |
| mem_reads | `table_scan` | 1.02s | 1.06s | 1.035× | 0.65% | PASS |
| mem_reads | `oltp_read_only` | 92.73ms | 100.20ms | 1.081× | 0.79% | PASS |
| mem_writes | `oltp_bulk_insert` | 183.53ms | 256.11ms | 1.396× | 0.80% | PASS |
| mem_writes | `oltp_insert` | 15.92ms | 29.37ms | 1.845× | 0.88% | PASS |
| mem_writes | `oltp_update_index` | 54.65ms | 100.26ms | 1.834× | 1.01% | PASS |
| mem_writes | `oltp_update_non_index` | 38.98ms | 64.41ms | 1.653× | 0.90% | PASS |
| mem_writes | `oltp_delete_insert` | 39.17ms | 78.80ms | 2.012× | 1.00% | PASS |
| mem_writes | `oltp_write_only` | 22.59ms | 47.57ms | 2.106× | 0.72% | PASS |
| mem_writes | `types_delete_insert` | 25.47ms | 39.87ms | 1.565× | 1.07% | PASS |
| mem_writes | `oltp_read_write` | 64.02ms | 103.33ms | 1.614× | 0.82% | PASS |
| file_reads | `oltp_point_select` | 98.51ms | 52.19ms | 0.530× | 1.07% | PASS |
| file_reads | `oltp_range_select` | 18.62ms | 13.26ms | 0.712× | 1.26% | PASS |
| file_reads | `oltp_sum_range` | 17.52ms | 12.96ms | 0.740× | 1.25% | PASS |
| file_reads | `oltp_order_range` | 3.30ms | 2.81ms | 0.852× | 1.24% | PASS |
| file_reads | `oltp_distinct_range` | 4.13ms | 3.56ms | 0.864× | 0.94% | PASS |
| file_reads | `oltp_index_scan` | 11.56ms | 7.48ms | 0.647× | 0.80% | PASS |
| file_reads | `select_random_points` | 22.02ms | 18.50ms | 0.840× | 1.14% | PASS |
| file_reads | `select_random_ranges` | 11.07ms | 6.71ms | 0.606× | 0.93% | PASS |
| file_reads | `covering_index_scan` | 12.01ms | 6.34ms | 0.528× | 0.82% | PASS |
| file_reads | `groupby_scan` | 27.51ms | 27.95ms | 1.016× | 0.78% | PASS |
| file_reads | `index_join` | 10.09ms | 9.15ms | 0.907× | 1.38% | PASS |
| file_reads | `index_join_scan` | 4.53ms | 5.00ms | 1.105× | 1.28% | PASS |
| file_reads | `types_table_scan` | 872.03ms | 966.08ms | 1.108× | 0.76% | PASS |
| file_reads | `table_scan` | 1.02s | 1.05s | 1.036× | 0.72% | PASS |
| file_reads | `oltp_read_only` | 200.21ms | 135.20ms | 0.675× | 0.74% | PASS |
| file_writes | `oltp_bulk_insert` | 243.67ms | 346.61ms | 1.422× | 6.19% | PASS |
| file_writes | `oltp_insert` | 51.54ms | 61.22ms | 1.188× | 15.01% | PASS |
| file_writes | `oltp_update_index` | 187.20ms | 197.09ms | 1.053× | 4.86% | PASS |
| file_writes | `oltp_update_non_index` | 152.47ms | 134.43ms | 0.882× | 9.06% | PASS |
| file_writes | `oltp_delete_insert` | 168.84ms | 159.46ms | 0.944× | 9.26% | PASS |
| file_writes | `oltp_write_only` | 112.41ms | 112.93ms | 1.005× | 10.03% | PASS |
| file_writes | `types_delete_insert` | 93.91ms | 90.36ms | 0.962× | 7.93% | PASS |
| file_writes | `oltp_read_write` | 156.65ms | 168.21ms | 1.074× | 3.28% | PASS |
| ac_reads | `oltp_point_select` | 48.64ms | 51.95ms | 1.068× | 0.73% | PASS |
| ac_reads | `oltp_range_select` | 13.90ms | 13.24ms | 0.953× | 0.99% | PASS |
| ac_reads | `oltp_sum_range` | 12.66ms | 12.96ms | 1.024× | 1.19% | PASS |
| ac_reads | `oltp_order_range` | 2.88ms | 2.81ms | 0.974× | 0.96% | PASS |
| ac_reads | `oltp_distinct_range` | 3.67ms | 3.56ms | 0.972× | 0.85% | PASS |
| ac_reads | `oltp_index_scan` | 6.71ms | 7.53ms | 1.122× | 0.99% | PASS |
| ac_reads | `select_random_points` | 17.18ms | 18.46ms | 1.074× | 1.02% | PASS |
| ac_reads | `select_random_ranges` | 6.15ms | 6.67ms | 1.085× | 0.59% | PASS |
| ac_reads | `covering_index_scan` | 7.06ms | 6.33ms | 0.896× | 0.83% | PASS |
| ac_reads | `groupby_scan` | 27.02ms | 27.93ms | 1.034× | 0.57% | PASS |
| ac_reads | `index_join` | 7.73ms | 9.15ms | 1.184× | 1.90% | PASS |
| ac_reads | `index_join_scan` | 4.16ms | 5.03ms | 1.209× | 2.15% | PASS |
| ac_reads | `types_table_scan` | 872.17ms | 965.43ms | 1.107× | 0.58% | PASS |
| ac_reads | `table_scan` | 1.02s | 1.05s | 1.039× | 0.66% | PASS |
| ac_reads | `oltp_read_only` | 128.72ms | 135.34ms | 1.051× | 0.62% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 31.22ms | 85.07ms | 2.724× | 21.59% | PASS |
| ac_writes | `oltp_insert_ac` | 34.74ms | 117.54ms | 3.383× | 24.79% | PASS |
| ac_writes | `oltp_update_index_ac` | 35.48ms | 118.70ms | 3.345× | 12.81% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 30.88ms | 114.76ms | 3.716× | 40.73% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 31.49ms | 103.88ms | 3.299× | 11.85% | PASS |
| ac_writes | `oltp_write_only_ac` | 33.90ms | 115.64ms | 3.411× | 25.85% | PASS |
| ac_writes | `types_delete_insert_ac` | 29.05ms | 112.26ms | 3.864× | 22.75% | PASS |
| ac_writes | `oltp_read_write_ac` | 39.25ms | 125.63ms | 3.201× | 16.52% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.94ms | 40.46ms | 1.228× | 1.27% | PASS |
| mem_reads | `oltp_range_select` | 18.82ms | 21.13ms | 1.123× | 1.43% | PASS |
| mem_reads | `oltp_sum_range` | 17.77ms | 20.93ms | 1.177× | 1.32% | PASS |
| mem_reads | `oltp_order_range` | 3.49ms | 3.84ms | 1.100× | 1.18% | PASS |
| mem_reads | `oltp_distinct_range` | 4.63ms | 4.92ms | 1.063× | 1.15% | PASS |
| mem_reads | `oltp_index_scan` | 4.62ms | 6.29ms | 1.360× | 2.10% | PASS |
| mem_reads | `select_random_points` | 27.74ms | 31.77ms | 1.145× | 1.80% | PASS |
| mem_reads | `select_random_ranges` | 7.45ms | 9.07ms | 1.218× | 1.28% | PASS |
| mem_reads | `covering_index_scan` | 4.25ms | 4.14ms | 0.974× | 1.39% | PASS |
| mem_reads | `groupby_scan` | 36.17ms | 38.98ms | 1.078× | 0.83% | PASS |
| mem_reads | `index_join` | 8.26ms | 10.33ms | 1.251× | 1.14% | PASS |
| mem_reads | `index_join_scan` | 4.14ms | 5.42ms | 1.307× | 2.63% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.24s | 1.189× | 0.59% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.39s | 1.181× | 0.36% | PASS |
| mem_reads | `oltp_read_only` | 147.83ms | 168.50ms | 1.140× | 0.97% | PASS |
| mem_writes | `oltp_bulk_insert` | 248.51ms | 355.78ms | 1.432× | 1.02% | PASS |
| mem_writes | `oltp_insert` | 19.19ms | 36.36ms | 1.895× | 0.74% | PASS |
| mem_writes | `oltp_update_index` | 66.97ms | 115.77ms | 1.729× | 1.19% | PASS |
| mem_writes | `oltp_update_non_index` | 49.99ms | 82.59ms | 1.652× | 1.19% | PASS |
| mem_writes | `oltp_delete_insert` | 49.23ms | 95.06ms | 1.931× | 1.45% | PASS |
| mem_writes | `oltp_write_only` | 26.87ms | 57.25ms | 2.130× | 1.25% | PASS |
| mem_writes | `types_delete_insert` | 32.62ms | 54.43ms | 1.668× | 1.51% | PASS |
| mem_writes | `oltp_read_write` | 100.22ms | 154.34ms | 1.540× | 1.24% | PASS |
| file_reads | `oltp_point_select` | 109.17ms | 66.70ms | 0.611× | 1.05% | PASS |
| file_reads | `oltp_range_select` | 26.59ms | 24.33ms | 0.915× | 2.49% | PASS |
| file_reads | `oltp_sum_range` | 25.70ms | 24.14ms | 0.939× | 2.03% | PASS |
| file_reads | `oltp_order_range` | 4.41ms | 4.26ms | 0.967× | 2.00% | PASS |
| file_reads | `oltp_distinct_range` | 5.62ms | 5.38ms | 0.957× | 2.77% | PASS |
| file_reads | `oltp_index_scan` | 12.30ms | 9.13ms | 0.742× | 2.25% | PASS |
| file_reads | `select_random_points` | 37.51ms | 36.01ms | 0.960× | 1.86% | PASS |
| file_reads | `select_random_ranges` | 15.56ms | 12.09ms | 0.777× | 1.64% | PASS |
| file_reads | `covering_index_scan` | 11.95ms | 7.19ms | 0.602× | 1.88% | PASS |
| file_reads | `groupby_scan` | 37.16ms | 39.87ms | 1.073× | 1.08% | PASS |
| file_reads | `index_join` | 12.56ms | 12.72ms | 1.013× | 1.95% | PASS |
| file_reads | `index_join_scan` | 5.21ms | 5.96ms | 1.144× | 1.28% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.23s | 1.191× | 0.68% | PASS |
| file_reads | `table_scan` | 1.18s | 1.39s | 1.176× | 0.52% | PASS |
| file_reads | `oltp_read_only` | 261.83ms | 211.04ms | 0.806× | 0.92% | PASS |
| file_writes | `oltp_bulk_insert` | 263.25ms | 377.71ms | 1.435× | 1.06% | PASS |
| file_writes | `oltp_insert` | 26.15ms | 46.41ms | 1.775× | 1.78% | PASS |
| file_writes | `oltp_update_index` | 96.90ms | 142.55ms | 1.471× | 1.26% | PASS |
| file_writes | `oltp_update_non_index` | 76.79ms | 104.10ms | 1.356× | 1.42% | PASS |
| file_writes | `oltp_delete_insert` | 76.95ms | 119.39ms | 1.551× | 1.31% | PASS |
| file_writes | `oltp_write_only` | 51.20ms | 77.86ms | 1.521× | 1.73% | PASS |
| file_writes | `types_delete_insert` | 50.20ms | 68.53ms | 1.365× | 1.77% | PASS |
| file_writes | `oltp_read_write` | 128.05ms | 175.60ms | 1.371× | 1.80% | PASS |
| ac_reads | `oltp_point_select` | 57.55ms | 66.69ms | 1.159× | 0.95% | PASS |
| ac_reads | `oltp_range_select` | 21.65ms | 24.22ms | 1.118× | 1.48% | PASS |
| ac_reads | `oltp_sum_range` | 20.63ms | 24.10ms | 1.169× | 1.27% | PASS |
| ac_reads | `oltp_order_range` | 3.90ms | 4.28ms | 1.098× | 1.06% | PASS |
| ac_reads | `oltp_distinct_range` | 5.04ms | 5.37ms | 1.066× | 1.59% | PASS |
| ac_reads | `oltp_index_scan` | 7.20ms | 9.15ms | 1.270× | 1.61% | PASS |
| ac_reads | `select_random_points` | 31.15ms | 36.30ms | 1.165× | 1.72% | PASS |
| ac_reads | `select_random_ranges` | 10.33ms | 12.09ms | 1.170× | 0.95% | PASS |
| ac_reads | `covering_index_scan` | 6.90ms | 7.18ms | 1.040× | 1.52% | PASS |
| ac_reads | `groupby_scan` | 36.67ms | 39.83ms | 1.086× | 0.82% | PASS |
| ac_reads | `index_join` | 9.83ms | 12.79ms | 1.301× | 1.64% | PASS |
| ac_reads | `index_join_scan` | 4.68ms | 5.98ms | 1.280× | 1.49% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.23s | 1.191× | 0.71% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.39s | 1.173× | 0.62% | PASS |
| ac_reads | `oltp_read_only` | 186.36ms | 209.13ms | 1.122× | 1.08% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.74ms | 79.57ms | 3.660× | 5.72% | PASS |
| ac_writes | `oltp_insert_ac` | 24.11ms | 99.72ms | 4.135× | 4.10% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.85ms | 112.92ms | 4.206× | 4.99% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.23ms | 92.31ms | 3.973× | 6.38% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.86ms | 103.04ms | 4.319× | 5.05% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.12ms | 103.00ms | 4.100× | 4.11% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.99ms | 97.86ms | 4.450× | 7.39% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.24ms | 111.37ms | 3.351× | 6.69% | PASS |

</details>

## Version-control latency

Wall time: 1m 58s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 64.95ms | 200.00ms | 32.5% | 0.90% | PASS |
| `status_dirty_many_tables` | 66.41ms | 200.00ms | 33.2% | 1.53% | PASS |
| `diff_regular_working_one_table` | 58.76ms | 150.00ms | 39.2% | 1.34% | PASS |
| `diff_regular_working_many_tables` | 70.31ms | 200.00ms | 35.2% | 1.22% | PASS |
| `diff_stat_working_many_tables` | 71.32ms | 200.00ms | 35.7% | 1.11% | PASS |
| `diff_schema_working_many_tables` | 70.88ms | 200.00ms | 35.4% | 1.11% | PASS |
| `branch_list_many_branches` | 19.75ms | 100.00ms | 19.8% | 1.51% | PASS |
| `branch_create_delete` | 22.16ms | 100.00ms | 22.2% | 3.04% | PASS |
| `checkout_branch_clean` | 92.64ms | 200.00ms | 46.3% | 11.45% | PASS |
| `merge_data_no_conflicts` | 30.54ms | 150.00ms | 20.4% | 2.17% | PASS |
| `merge_schema_no_conflicts` | 22.58ms | 100.00ms | 22.6% | 12.75% | PASS |
| `merge_data_conflicts` | 74.27ms | 250.00ms | 29.7% | 0.95% | PASS |
| `merge_data_conflicts_with_resolve` | 73.71ms | 250.00ms | 29.5% | 1.31% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
