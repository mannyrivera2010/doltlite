# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-10 15:23 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260907.300.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/34484645296)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 1s | 9.00s | 11.60s | 1.288× | 1.53% | **PASS** |
| textpk | 69 | 55 | 1h 34m 52s | 10.37s | 12.10s | 1.167× | 2.79% | **PASS** |
| blobpk | 69 | 55 | 1h 31m 18s | 9.71s | 11.91s | 1.226× | 2.73% | **PASS** |
| compositepk | 69 | 55 | 1h 14m 29s | 7.88s | 9.91s | 1.257× | 2.54% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.20ms | 29.85ms | 1.234× | 2.41% | PASS |
| mem_reads | `oltp_range_select` | 10.33ms | 13.26ms | 1.284× | 2.61% | PASS |
| mem_reads | `oltp_sum_range` | 9.77ms | 12.50ms | 1.279× | 2.26% | PASS |
| mem_reads | `oltp_order_range` | 2.58ms | 3.00ms | 1.162× | 1.27% | PASS |
| mem_reads | `oltp_distinct_range` | 3.64ms | 4.07ms | 1.118× | 1.24% | PASS |
| mem_reads | `oltp_index_scan` | 3.92ms | 5.29ms | 1.350× | 1.28% | PASS |
| mem_reads | `select_random_points` | 9.83ms | 11.19ms | 1.138× | 2.26% | PASS |
| mem_reads | `select_random_ranges` | 2.96ms | 4.00ms | 1.355× | 1.57% | PASS |
| mem_reads | `covering_index_scan` | 4.25ms | 4.24ms | 0.998× | 1.83% | PASS |
| mem_reads | `groupby_scan` | 29.77ms | 32.71ms | 1.099× | 0.81% | PASS |
| mem_reads | `index_join` | 6.02ms | 8.28ms | 1.375× | 1.45% | PASS |
| mem_reads | `index_join_scan` | 3.47ms | 4.58ms | 1.320× | 1.12% | PASS |
| mem_reads | `types_table_scan` | 1.09s | 1.35s | 1.237× | 1.17% | PASS |
| mem_reads | `table_scan` | 1.28s | 1.42s | 1.108× | 1.36% | PASS |
| mem_reads | `oltp_read_only` | 105.71ms | 125.67ms | 1.189× | 0.90% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.29ms | 253.10ms | 1.396× | 1.14% | PASS |
| mem_writes | `oltp_insert` | 15.60ms | 28.81ms | 1.847× | 1.07% | PASS |
| mem_writes | `oltp_update_index` | 51.51ms | 108.43ms | 2.105× | 1.66% | PASS |
| mem_writes | `oltp_update_non_index` | 34.84ms | 61.20ms | 1.757× | 1.76% | PASS |
| mem_writes | `oltp_delete_insert` | 45.89ms | 80.84ms | 1.762× | 1.79% | PASS |
| mem_writes | `oltp_write_only` | 22.36ms | 45.88ms | 2.052× | 1.54% | PASS |
| mem_writes | `types_delete_insert` | 24.97ms | 40.82ms | 1.635× | 1.02% | PASS |
| mem_writes | `oltp_read_write` | 68.68ms | 112.78ms | 1.642× | 1.87% | PASS |
| file_reads | `oltp_point_select` | 99.40ms | 55.35ms | 0.557× | 1.01% | PASS |
| file_reads | `oltp_range_select` | 17.84ms | 15.88ms | 0.890× | 1.99% | PASS |
| file_reads | `oltp_sum_range` | 17.52ms | 15.19ms | 0.867× | 1.48% | PASS |
| file_reads | `oltp_order_range` | 3.49ms | 3.37ms | 0.964× | 2.19% | PASS |
| file_reads | `oltp_distinct_range` | 4.58ms | 4.44ms | 0.968× | 1.53% | PASS |
| file_reads | `oltp_index_scan` | 11.74ms | 8.27ms | 0.704× | 1.32% | PASS |
| file_reads | `select_random_points` | 17.95ms | 13.95ms | 0.777× | 2.19% | PASS |
| file_reads | `select_random_ranges` | 10.57ms | 6.62ms | 0.627× | 1.24% | PASS |
| file_reads | `covering_index_scan` | 12.17ms | 7.11ms | 0.584× | 0.95% | PASS |
| file_reads | `groupby_scan` | 30.96ms | 33.31ms | 1.076× | 1.21% | PASS |
| file_reads | `index_join` | 10.38ms | 10.11ms | 0.974× | 1.30% | PASS |
| file_reads | `index_join_scan` | 4.44ms | 4.92ms | 1.108× | 1.52% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.32s | 1.279× | 0.62% | PASS |
| file_reads | `table_scan` | 1.17s | 1.40s | 1.196× | 0.80% | PASS |
| file_reads | `oltp_read_only` | 211.20ms | 161.59ms | 0.765× | 0.66% | PASS |
| file_writes | `oltp_bulk_insert` | 194.92ms | 270.30ms | 1.387× | 1.54% | PASS |
| file_writes | `oltp_insert` | 22.39ms | 36.11ms | 1.613× | 2.49% | PASS |
| file_writes | `oltp_update_index` | 78.16ms | 128.96ms | 1.650× | 2.02% | PASS |
| file_writes | `oltp_update_non_index` | 57.81ms | 81.20ms | 1.405× | 2.30% | PASS |
| file_writes | `oltp_delete_insert` | 67.61ms | 99.75ms | 1.475× | 1.72% | PASS |
| file_writes | `oltp_write_only` | 44.88ms | 64.23ms | 1.431× | 2.29% | PASS |
| file_writes | `types_delete_insert` | 39.91ms | 53.58ms | 1.343× | 1.93% | PASS |
| file_writes | `oltp_read_write` | 90.70ms | 130.33ms | 1.437× | 1.65% | PASS |
| ac_reads | `oltp_point_select` | 48.36ms | 54.99ms | 1.137× | 0.62% | PASS |
| ac_reads | `oltp_range_select` | 12.91ms | 15.85ms | 1.228× | 1.51% | PASS |
| ac_reads | `oltp_sum_range` | 12.30ms | 15.11ms | 1.229× | 0.73% | PASS |
| ac_reads | `oltp_order_range` | 3.00ms | 3.37ms | 1.126× | 2.16% | PASS |
| ac_reads | `oltp_distinct_range` | 4.04ms | 4.49ms | 1.111× | 1.59% | PASS |
| ac_reads | `oltp_index_scan` | 6.70ms | 8.20ms | 1.223× | 1.48% | PASS |
| ac_reads | `select_random_points` | 12.76ms | 13.86ms | 1.086× | 1.56% | PASS |
| ac_reads | `select_random_ranges` | 5.55ms | 6.63ms | 1.193× | 1.11% | PASS |
| ac_reads | `covering_index_scan` | 7.01ms | 7.03ms | 1.003× | 1.16% | PASS |
| ac_reads | `groupby_scan` | 30.22ms | 33.22ms | 1.099× | 0.60% | PASS |
| ac_reads | `index_join` | 7.66ms | 10.07ms | 1.314× | 1.33% | PASS |
| ac_reads | `index_join_scan` | 3.92ms | 4.98ms | 1.268× | 2.33% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.33s | 1.278× | 0.74% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.40s | 1.196× | 0.78% | PASS |
| ac_reads | `oltp_read_only` | 138.01ms | 161.30ms | 1.169× | 0.87% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.37ms | 86.51ms | 3.550× | 6.60% | PASS |
| ac_writes | `oltp_insert_ac` | 26.67ms | 103.54ms | 3.882× | 5.80% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.50ms | 119.92ms | 4.208× | 7.02% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.17ms | 97.69ms | 4.041× | 5.80% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.29ms | 108.38ms | 4.122× | 6.95% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.66ms | 109.39ms | 4.102× | 5.36% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.86ms | 100.06ms | 4.193× | 7.15% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.56ms | 116.75ms | 3.585× | 5.14% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.65ms | 39.36ms | 1.205× | 2.92% | PASS |
| mem_reads | `oltp_range_select` | 16.13ms | 14.88ms | 0.923× | 4.25% | PASS |
| mem_reads | `oltp_sum_range` | 13.11ms | 14.74ms | 1.124× | 3.89% | PASS |
| mem_reads | `oltp_order_range` | 3.16ms | 3.23ms | 1.020× | 2.00% | PASS |
| mem_reads | `oltp_distinct_range` | 4.16ms | 4.32ms | 1.040× | 1.80% | PASS |
| mem_reads | `oltp_index_scan` | 4.80ms | 6.62ms | 1.380× | 3.41% | PASS |
| mem_reads | `select_random_points` | 19.35ms | 22.05ms | 1.140× | 3.38% | PASS |
| mem_reads | `select_random_ranges` | 4.20ms | 5.34ms | 1.271× | 2.04% | PASS |
| mem_reads | `covering_index_scan` | 5.25ms | 4.83ms | 0.920× | 6.58% | PASS |
| mem_reads | `groupby_scan` | 33.06ms | 34.27ms | 1.036× | 1.35% | PASS |
| mem_reads | `index_join` | 7.51ms | 9.86ms | 1.313× | 4.88% | PASS |
| mem_reads | `index_join_scan` | 5.25ms | 5.81ms | 1.107× | 6.67% | PASS |
| mem_reads | `types_table_scan` | 1.20s | 1.28s | 1.064× | 3.42% | PASS |
| mem_reads | `table_scan` | 1.60s | 1.46s | 0.910× | 1.67% | PASS |
| mem_reads | `oltp_read_only` | 137.88ms | 145.84ms | 1.058× | 1.68% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.70ms | 368.17ms | 1.562× | 0.93% | PASS |
| mem_writes | `oltp_insert` | 24.57ms | 42.02ms | 1.710× | 2.48% | PASS |
| mem_writes | `oltp_update_index` | 80.68ms | 145.73ms | 1.806× | 4.22% | PASS |
| mem_writes | `oltp_update_non_index` | 51.89ms | 90.97ms | 1.753× | 1.91% | PASS |
| mem_writes | `oltp_delete_insert` | 56.28ms | 109.22ms | 1.941× | 2.67% | PASS |
| mem_writes | `oltp_write_only` | 30.56ms | 64.48ms | 2.110× | 2.37% | PASS |
| mem_writes | `types_delete_insert` | 35.13ms | 57.54ms | 1.638× | 1.56% | PASS |
| mem_writes | `oltp_read_write` | 94.28ms | 146.66ms | 1.556× | 2.79% | PASS |
| file_reads | `oltp_point_select` | 108.69ms | 65.21ms | 0.600× | 1.21% | PASS |
| file_reads | `oltp_range_select` | 24.00ms | 17.70ms | 0.737× | 2.70% | PASS |
| file_reads | `oltp_sum_range` | 22.02ms | 17.62ms | 0.800× | 3.66% | PASS |
| file_reads | `oltp_order_range` | 4.30ms | 3.90ms | 0.906× | 4.41% | PASS |
| file_reads | `oltp_distinct_range` | 5.53ms | 4.98ms | 0.901× | 5.07% | PASS |
| file_reads | `oltp_index_scan` | 13.14ms | 9.51ms | 0.723× | 2.33% | PASS |
| file_reads | `select_random_points` | 29.55ms | 25.58ms | 0.866× | 2.33% | PASS |
| file_reads | `select_random_ranges` | 12.21ms | 8.08ms | 0.662× | 1.92% | PASS |
| file_reads | `covering_index_scan` | 13.90ms | 7.61ms | 0.547× | 3.50% | PASS |
| file_reads | `groupby_scan` | 33.99ms | 34.86ms | 1.026× | 1.46% | PASS |
| file_reads | `index_join` | 12.47ms | 11.58ms | 0.929× | 3.73% | PASS |
| file_reads | `index_join_scan` | 6.39ms | 6.34ms | 0.992× | 3.93% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.23s | 1.158× | 1.11% | PASS |
| file_reads | `table_scan` | 1.25s | 1.38s | 1.103× | 1.26% | PASS |
| file_reads | `oltp_read_only` | 251.23ms | 183.77ms | 0.731× | 1.11% | PASS |
| file_writes | `oltp_bulk_insert` | 256.03ms | 394.68ms | 1.542× | 1.14% | PASS |
| file_writes | `oltp_insert` | 61.38ms | 54.96ms | 0.895× | 15.95% | PASS |
| file_writes | `oltp_update_index` | 127.01ms | 179.99ms | 1.417× | 1.65% | PASS |
| file_writes | `oltp_update_non_index` | 105.77ms | 117.23ms | 1.108× | 12.51% | PASS |
| file_writes | `oltp_delete_insert` | 96.93ms | 140.45ms | 1.449× | 1.89% | PASS |
| file_writes | `oltp_write_only` | 82.26ms | 90.56ms | 1.101× | 11.67% | PASS |
| file_writes | `types_delete_insert` | 59.37ms | 79.15ms | 1.333× | 2.03% | PASS |
| file_writes | `oltp_read_write` | 166.22ms | 174.57ms | 1.050× | 5.07% | PASS |
| ac_reads | `oltp_point_select` | 57.12ms | 65.37ms | 1.144× | 1.31% | PASS |
| ac_reads | `oltp_range_select` | 18.51ms | 17.61ms | 0.951× | 4.24% | PASS |
| ac_reads | `oltp_sum_range` | 16.49ms | 17.63ms | 1.069× | 2.66% | PASS |
| ac_reads | `oltp_order_range` | 3.95ms | 3.96ms | 1.003× | 4.37% | PASS |
| ac_reads | `oltp_distinct_range` | 4.81ms | 4.88ms | 1.014× | 3.52% | PASS |
| ac_reads | `oltp_index_scan` | 8.01ms | 9.49ms | 1.186× | 2.61% | PASS |
| ac_reads | `select_random_points` | 22.66ms | 25.16ms | 1.110× | 1.83% | PASS |
| ac_reads | `select_random_ranges` | 6.93ms | 8.00ms | 1.153× | 1.67% | PASS |
| ac_reads | `covering_index_scan` | 7.90ms | 7.59ms | 0.961× | 2.01% | PASS |
| ac_reads | `groupby_scan` | 33.05ms | 34.85ms | 1.054× | 1.58% | PASS |
| ac_reads | `index_join` | 9.26ms | 11.79ms | 1.274× | 6.06% | PASS |
| ac_reads | `index_join_scan` | 5.59ms | 6.16ms | 1.103× | 3.69% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.22s | 1.161× | 0.86% | PASS |
| ac_reads | `table_scan` | 1.26s | 1.38s | 1.094× | 3.92% | PASS |
| ac_reads | `oltp_read_only` | 160.12ms | 177.52ms | 1.109× | 1.76% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.84ms | 79.86ms | 3.657× | 4.89% | PASS |
| ac_writes | `oltp_insert_ac` | 26.16ms | 97.32ms | 3.720× | 8.05% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.97ms | 115.01ms | 4.112× | 4.00% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.70ms | 93.55ms | 4.120× | 5.21% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.23ms | 104.01ms | 4.293× | 4.17% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.71ms | 103.81ms | 4.037× | 5.34% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.53ms | 95.42ms | 4.433× | 5.81% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.44ms | 111.85ms | 3.557× | 4.84% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.58ms | 37.57ms | 1.229× | 2.32% | PASS |
| mem_reads | `oltp_range_select` | 12.75ms | 14.11ms | 1.107× | 2.85% | PASS |
| mem_reads | `oltp_sum_range` | 12.01ms | 14.17ms | 1.180× | 2.76% | PASS |
| mem_reads | `oltp_order_range` | 2.97ms | 3.18ms | 1.072× | 1.87% | PASS |
| mem_reads | `oltp_distinct_range` | 4.06ms | 4.23ms | 1.043× | 1.57% | PASS |
| mem_reads | `oltp_index_scan` | 4.54ms | 6.34ms | 1.397× | 3.71% | PASS |
| mem_reads | `select_random_points` | 17.80ms | 21.14ms | 1.187× | 3.25% | PASS |
| mem_reads | `select_random_ranges` | 4.06ms | 5.22ms | 1.284× | 2.02% | PASS |
| mem_reads | `covering_index_scan` | 4.36ms | 4.46ms | 1.024× | 2.09% | PASS |
| mem_reads | `groupby_scan` | 31.99ms | 33.88ms | 1.059× | 0.93% | PASS |
| mem_reads | `index_join` | 6.79ms | 9.44ms | 1.391× | 3.15% | PASS |
| mem_reads | `index_join_scan` | 4.19ms | 5.51ms | 1.317× | 2.44% | PASS |
| mem_reads | `types_table_scan` | 1.08s | 1.24s | 1.151× | 1.45% | PASS |
| mem_reads | `table_scan` | 1.28s | 1.39s | 1.092× | 2.53% | PASS |
| mem_reads | `oltp_read_only` | 119.32ms | 137.13ms | 1.149× | 2.90% | PASS |
| mem_writes | `oltp_bulk_insert` | 240.31ms | 352.57ms | 1.467× | 1.05% | PASS |
| mem_writes | `oltp_insert` | 19.91ms | 39.80ms | 1.999× | 1.15% | PASS |
| mem_writes | `oltp_update_index` | 68.42ms | 131.93ms | 1.928× | 2.76% | PASS |
| mem_writes | `oltp_update_non_index` | 48.37ms | 85.58ms | 1.769× | 2.59% | PASS |
| mem_writes | `oltp_delete_insert` | 49.43ms | 105.51ms | 2.134× | 2.46% | PASS |
| mem_writes | `oltp_write_only` | 28.21ms | 62.89ms | 2.229× | 3.26% | PASS |
| mem_writes | `types_delete_insert` | 32.70ms | 54.87ms | 1.678× | 2.21% | PASS |
| mem_writes | `oltp_read_write` | 85.18ms | 141.47ms | 1.661× | 3.04% | PASS |
| file_reads | `oltp_point_select` | 104.89ms | 63.60ms | 0.606× | 1.17% | PASS |
| file_reads | `oltp_range_select` | 21.22ms | 17.23ms | 0.812× | 2.87% | PASS |
| file_reads | `oltp_sum_range` | 20.37ms | 17.29ms | 0.849× | 2.45% | PASS |
| file_reads | `oltp_order_range` | 3.92ms | 3.58ms | 0.914× | 3.69% | PASS |
| file_reads | `oltp_distinct_range` | 5.00ms | 4.66ms | 0.932× | 2.29% | PASS |
| file_reads | `oltp_index_scan` | 12.52ms | 9.37ms | 0.748× | 2.41% | PASS |
| file_reads | `select_random_points` | 27.52ms | 25.05ms | 0.910× | 2.76% | PASS |
| file_reads | `select_random_ranges` | 11.73ms | 7.88ms | 0.672× | 1.79% | PASS |
| file_reads | `covering_index_scan` | 12.69ms | 7.33ms | 0.578× | 1.68% | PASS |
| file_reads | `groupby_scan` | 33.08ms | 34.38ms | 1.039× | 1.36% | PASS |
| file_reads | `index_join` | 11.64ms | 11.75ms | 1.009× | 2.84% | PASS |
| file_reads | `index_join_scan` | 5.34ms | 5.99ms | 1.123× | 3.18% | PASS |
| file_reads | `types_table_scan` | 1.08s | 1.24s | 1.152× | 2.07% | PASS |
| file_reads | `table_scan` | 1.27s | 1.39s | 1.089× | 2.43% | PASS |
| file_reads | `oltp_read_only` | 233.09ms | 178.96ms | 0.768× | 1.90% | PASS |
| file_writes | `oltp_bulk_insert` | 258.77ms | 377.35ms | 1.458× | 1.37% | PASS |
| file_writes | `oltp_insert` | 32.22ms | 51.87ms | 1.610× | 2.75% | PASS |
| file_writes | `oltp_update_index` | 104.82ms | 166.74ms | 1.591× | 2.87% | PASS |
| file_writes | `oltp_update_non_index` | 79.46ms | 109.94ms | 1.384× | 2.73% | PASS |
| file_writes | `oltp_delete_insert` | 82.54ms | 133.17ms | 1.613× | 2.51% | PASS |
| file_writes | `oltp_write_only` | 56.66ms | 86.90ms | 1.534× | 3.53% | PASS |
| file_writes | `types_delete_insert` | 52.82ms | 73.08ms | 1.383× | 2.54% | PASS |
| file_writes | `oltp_read_write` | 117.16ms | 166.41ms | 1.420× | 2.74% | PASS |
| ac_reads | `oltp_point_select` | 55.03ms | 64.04ms | 1.164× | 2.29% | PASS |
| ac_reads | `oltp_range_select` | 16.19ms | 17.38ms | 1.073× | 2.87% | PASS |
| ac_reads | `oltp_sum_range` | 15.37ms | 17.48ms | 1.138× | 3.29% | PASS |
| ac_reads | `oltp_order_range` | 3.35ms | 3.57ms | 1.068× | 2.81% | PASS |
| ac_reads | `oltp_distinct_range` | 4.48ms | 4.69ms | 1.046× | 3.42% | PASS |
| ac_reads | `oltp_index_scan` | 7.56ms | 9.43ms | 1.248× | 2.51% | PASS |
| ac_reads | `select_random_points` | 21.68ms | 25.26ms | 1.165× | 3.52% | PASS |
| ac_reads | `select_random_ranges` | 6.82ms | 7.88ms | 1.156× | 2.20% | PASS |
| ac_reads | `covering_index_scan` | 7.75ms | 7.39ms | 0.954× | 3.37% | PASS |
| ac_reads | `groupby_scan` | 32.40ms | 34.31ms | 1.059× | 1.31% | PASS |
| ac_reads | `index_join` | 9.04ms | 11.83ms | 1.308× | 3.94% | PASS |
| ac_reads | `index_join_scan` | 4.85ms | 5.97ms | 1.233× | 3.85% | PASS |
| ac_reads | `types_table_scan` | 1.07s | 1.23s | 1.154× | 2.01% | PASS |
| ac_reads | `table_scan` | 1.27s | 1.38s | 1.085× | 2.47% | PASS |
| ac_reads | `oltp_read_only` | 158.03ms | 178.41ms | 1.129× | 2.33% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.01ms | 81.41ms | 3.391× | 5.88% | PASS |
| ac_writes | `oltp_insert_ac` | 25.36ms | 104.09ms | 4.105× | 7.01% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.74ms | 117.26ms | 4.080× | 8.27% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.04ms | 96.11ms | 3.999× | 6.04% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.93ms | 107.28ms | 4.303× | 6.55% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.10ms | 105.11ms | 4.188× | 4.90% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.44ms | 99.80ms | 4.448× | 6.28% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.82ms | 111.67ms | 3.623× | 4.03% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 21.61ms | 24.90ms | 1.152× | 3.26% | PASS |
| mem_reads | `oltp_range_select` | 11.57ms | 11.46ms | 0.991× | 2.78% | PASS |
| mem_reads | `oltp_sum_range` | 10.60ms | 11.50ms | 1.085× | 1.58% | PASS |
| mem_reads | `oltp_order_range` | 2.22ms | 2.19ms | 0.988× | 2.05% | PASS |
| mem_reads | `oltp_distinct_range` | 2.79ms | 2.77ms | 0.992× | 2.24% | PASS |
| mem_reads | `oltp_index_scan` | 3.01ms | 4.02ms | 1.333× | 4.18% | PASS |
| mem_reads | `select_random_points` | 18.38ms | 20.80ms | 1.132× | 3.47% | PASS |
| mem_reads | `select_random_ranges` | 4.67ms | 5.13ms | 1.098× | 2.19% | PASS |
| mem_reads | `covering_index_scan` | 2.35ms | 2.83ms | 1.205× | 4.78% | PASS |
| mem_reads | `groupby_scan` | 21.57ms | 22.04ms | 1.022× | 1.03% | PASS |
| mem_reads | `index_join` | 5.17ms | 7.95ms | 1.539× | 4.46% | PASS |
| mem_reads | `index_join_scan` | 3.15ms | 4.86ms | 1.544× | 3.84% | PASS |
| mem_reads | `types_table_scan` | 813.44ms | 798.27ms | 0.981× | 3.21% | PASS |
| mem_reads | `table_scan` | 1.02s | 899.61ms | 0.879× | 2.72% | PASS |
| mem_reads | `oltp_read_only` | 88.60ms | 91.78ms | 1.036× | 3.13% | PASS |
| mem_writes | `oltp_bulk_insert` | 149.12ms | 214.60ms | 1.439× | 1.73% | PASS |
| mem_writes | `oltp_insert` | 12.14ms | 24.29ms | 2.001× | 2.54% | PASS |
| mem_writes | `oltp_update_index` | 50.95ms | 94.59ms | 1.857× | 3.12% | PASS |
| mem_writes | `oltp_update_non_index` | 35.46ms | 60.47ms | 1.705× | 1.95% | PASS |
| mem_writes | `oltp_delete_insert` | 29.99ms | 59.41ms | 1.981× | 2.24% | PASS |
| mem_writes | `oltp_write_only` | 17.28ms | 38.79ms | 2.245× | 2.17% | PASS |
| mem_writes | `types_delete_insert` | 20.96ms | 35.84ms | 1.710× | 2.58% | PASS |
| mem_writes | `oltp_read_write` | 58.28ms | 89.83ms | 1.541× | 3.62% | PASS |
| file_reads | `oltp_point_select` | 76.08ms | 42.18ms | 0.554× | 1.65% | PASS |
| file_reads | `oltp_range_select` | 17.21ms | 13.39ms | 0.778× | 1.30% | PASS |
| file_reads | `oltp_sum_range` | 16.42ms | 13.40ms | 0.816× | 1.35% | PASS |
| file_reads | `oltp_order_range` | 2.95ms | 2.51ms | 0.848× | 1.33% | PASS |
| file_reads | `oltp_distinct_range` | 3.54ms | 3.06ms | 0.866× | 1.25% | PASS |
| file_reads | `oltp_index_scan` | 9.38ms | 6.37ms | 0.679× | 1.34% | PASS |
| file_reads | `select_random_points` | 24.93ms | 22.56ms | 0.905× | 2.51% | PASS |
| file_reads | `select_random_ranges` | 10.94ms | 7.50ms | 0.686× | 0.78% | PASS |
| file_reads | `covering_index_scan` | 9.01ms | 5.25ms | 0.583× | 1.38% | PASS |
| file_reads | `groupby_scan` | 22.27ms | 22.42ms | 1.006× | 1.21% | PASS |
| file_reads | `index_join` | 8.65ms | 8.35ms | 0.966× | 2.43% | PASS |
| file_reads | `index_join_scan` | 3.76ms | 4.55ms | 1.210× | 5.35% | PASS |
| file_reads | `types_table_scan` | 768.07ms | 774.87ms | 1.009× | 3.61% | PASS |
| file_reads | `table_scan` | 928.35ms | 854.40ms | 0.920× | 4.66% | PASS |
| file_reads | `oltp_read_only` | 169.63ms | 117.86ms | 0.695× | 1.74% | PASS |
| file_writes | `oltp_bulk_insert` | 224.81ms | 264.51ms | 1.177× | 12.55% | PASS |
| file_writes | `oltp_insert` | 23.72ms | 45.94ms | 1.936× | 29.55% | PASS |
| file_writes | `oltp_update_index` | 151.31ms | 155.55ms | 1.028× | 12.91% | PASS |
| file_writes | `oltp_update_non_index` | 145.26ms | 111.87ms | 0.770× | 26.61% | PASS |
| file_writes | `oltp_delete_insert` | 130.91ms | 128.07ms | 0.978× | 24.79% | PASS |
| file_writes | `oltp_write_only` | 104.56ms | 98.34ms | 0.941× | 18.61% | PASS |
| file_writes | `types_delete_insert` | 79.81ms | 79.28ms | 0.993× | 11.04% | PASS |
| file_writes | `oltp_read_write` | 159.34ms | 157.21ms | 0.987× | 16.45% | PASS |
| ac_reads | `oltp_point_select` | 41.45ms | 44.73ms | 1.079× | 2.19% | PASS |
| ac_reads | `oltp_range_select` | 14.09ms | 13.68ms | 0.971× | 2.07% | PASS |
| ac_reads | `oltp_sum_range` | 12.97ms | 13.49ms | 1.040× | 1.97% | PASS |
| ac_reads | `oltp_order_range` | 2.65ms | 2.51ms | 0.948× | 1.46% | PASS |
| ac_reads | `oltp_distinct_range` | 3.23ms | 3.06ms | 0.949× | 1.46% | PASS |
| ac_reads | `oltp_index_scan` | 5.54ms | 6.42ms | 1.159× | 1.81% | PASS |
| ac_reads | `select_random_points` | 20.49ms | 22.13ms | 1.080× | 2.35% | PASS |
| ac_reads | `select_random_ranges` | 6.89ms | 7.31ms | 1.060× | 1.05% | PASS |
| ac_reads | `covering_index_scan` | 4.99ms | 5.18ms | 1.038× | 1.43% | PASS |
| ac_reads | `groupby_scan` | 21.81ms | 22.04ms | 1.011× | 2.59% | PASS |
| ac_reads | `index_join` | 6.67ms | 8.23ms | 1.234× | 2.36% | PASS |
| ac_reads | `index_join_scan` | 3.21ms | 4.34ms | 1.349× | 2.92% | PASS |
| ac_reads | `types_table_scan` | 711.02ms | 754.64ms | 1.061× | 2.33% | PASS |
| ac_reads | `table_scan` | 803.34ms | 844.99ms | 1.052× | 2.15% | PASS |
| ac_reads | `oltp_read_only` | 107.36ms | 114.83ms | 1.070× | 1.39% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 56.16ms | 376.14ms | 6.698× | 76.01% | PASS |
| ac_writes | `oltp_insert_ac` | 98.81ms | 363.46ms | 3.678× | 65.05% | PASS |
| ac_writes | `oltp_update_index_ac` | 101.59ms | 315.79ms | 3.108× | 67.21% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 68.85ms | 406.02ms | 5.897× | 58.66% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 67.09ms | 256.89ms | 3.829× | 68.37% | PASS |
| ac_writes | `oltp_write_only_ac` | 59.85ms | 249.89ms | 4.175× | 78.73% | PASS |
| ac_writes | `types_delete_insert_ac` | 80.21ms | 316.66ms | 3.948× | 73.27% | PASS |
| ac_writes | `oltp_read_write_ac` | 87.77ms | 289.51ms | 3.299× | 67.56% | PASS |

</details>

## Version-control latency

Wall time: 2m 18s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 82.61ms | 200.00ms | 41.3% | 0.50% | PASS |
| `status_dirty_many_tables` | 85.70ms | 200.00ms | 42.8% | 0.48% | PASS |
| `diff_regular_working_one_table` | 78.39ms | 150.00ms | 52.3% | 0.46% | PASS |
| `diff_regular_working_many_tables` | 91.14ms | 200.00ms | 45.6% | 0.44% | PASS |
| `diff_stat_working_many_tables` | 91.45ms | 200.00ms | 45.7% | 0.46% | PASS |
| `diff_schema_working_many_tables` | 92.00ms | 200.00ms | 46.0% | 0.43% | PASS |
| `branch_list_many_branches` | 22.38ms | 100.00ms | 22.4% | 1.06% | PASS |
| `branch_create_delete` | 24.36ms | 100.00ms | 24.4% | 1.11% | PASS |
| `checkout_branch_clean` | 54.45ms | 200.00ms | 27.2% | 0.62% | PASS |
| `merge_data_no_conflicts` | 28.31ms | 150.00ms | 18.9% | 1.05% | PASS |
| `merge_schema_no_conflicts` | 21.86ms | 100.00ms | 21.9% | 1.11% | PASS |
| `merge_data_conflicts` | 126.18ms | 250.00ms | 50.5% | 0.31% | PASS |
| `merge_data_conflicts_with_resolve` | 126.33ms | 250.00ms | 50.5% | 0.34% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
