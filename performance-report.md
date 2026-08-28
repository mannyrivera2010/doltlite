# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-28 22:34 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260823.283.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/33210476479)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 32s | 8.78s | 11.53s | 1.313× | 1.40% | **PASS** |
| textpk | 69 | 55 | 1h 33m 50s | 10.99s | 11.89s | 1.081× | 1.69% | **PASS** |
| blobpk | 69 | 55 | 1h 32m 27s | 10.92s | 11.97s | 1.096× | 1.74% | **PASS** |
| compositepk | 69 | 55 | 1h 25m 20s | 10.09s | 11.68s | 1.158× | 0.74% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.73ms | 29.64ms | 1.249× | 1.40% | PASS |
| mem_reads | `oltp_range_select` | 10.08ms | 13.13ms | 1.303× | 2.36% | PASS |
| mem_reads | `oltp_sum_range` | 9.55ms | 12.33ms | 1.292× | 1.20% | PASS |
| mem_reads | `oltp_order_range` | 2.53ms | 2.99ms | 1.184× | 0.97% | PASS |
| mem_reads | `oltp_distinct_range` | 3.60ms | 4.05ms | 1.124× | 1.28% | PASS |
| mem_reads | `oltp_index_scan` | 3.88ms | 5.28ms | 1.361× | 1.35% | PASS |
| mem_reads | `select_random_points` | 9.97ms | 11.13ms | 1.116× | 2.54% | PASS |
| mem_reads | `select_random_ranges` | 2.96ms | 3.99ms | 1.347× | 1.74% | PASS |
| mem_reads | `covering_index_scan` | 4.24ms | 4.17ms | 0.984× | 1.38% | PASS |
| mem_reads | `groupby_scan` | 29.69ms | 32.82ms | 1.105× | 0.99% | PASS |
| mem_reads | `index_join` | 6.01ms | 8.03ms | 1.336× | 0.65% | PASS |
| mem_reads | `index_join_scan` | 3.48ms | 4.66ms | 1.340× | 2.28% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.33s | 1.290× | 0.49% | PASS |
| mem_reads | `table_scan` | 1.16s | 1.40s | 1.203× | 0.52% | PASS |
| mem_reads | `oltp_read_only` | 101.66ms | 123.27ms | 1.213× | 0.96% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.62ms | 252.45ms | 1.398× | 1.23% | PASS |
| mem_writes | `oltp_insert` | 15.49ms | 28.58ms | 1.844× | 1.17% | PASS |
| mem_writes | `oltp_update_index` | 49.91ms | 104.48ms | 2.093× | 1.09% | PASS |
| mem_writes | `oltp_update_non_index` | 34.03ms | 59.32ms | 1.743× | 1.17% | PASS |
| mem_writes | `oltp_delete_insert` | 45.21ms | 78.97ms | 1.747× | 1.13% | PASS |
| mem_writes | `oltp_write_only` | 21.86ms | 44.81ms | 2.050× | 1.35% | PASS |
| mem_writes | `types_delete_insert` | 24.54ms | 40.32ms | 1.643× | 1.52% | PASS |
| mem_writes | `oltp_read_write` | 66.46ms | 110.28ms | 1.659× | 1.14% | PASS |
| file_reads | `oltp_point_select` | 97.66ms | 54.85ms | 0.562× | 0.84% | PASS |
| file_reads | `oltp_range_select` | 17.93ms | 15.77ms | 0.879× | 1.57% | PASS |
| file_reads | `oltp_sum_range` | 17.49ms | 15.11ms | 0.864× | 1.28% | PASS |
| file_reads | `oltp_order_range` | 3.47ms | 3.35ms | 0.965× | 2.33% | PASS |
| file_reads | `oltp_distinct_range` | 4.55ms | 4.42ms | 0.973× | 1.59% | PASS |
| file_reads | `oltp_index_scan` | 11.75ms | 8.18ms | 0.696× | 1.98% | PASS |
| file_reads | `select_random_points` | 18.17ms | 13.86ms | 0.762× | 2.51% | PASS |
| file_reads | `select_random_ranges` | 10.55ms | 6.63ms | 0.628× | 0.88% | PASS |
| file_reads | `covering_index_scan` | 12.06ms | 7.01ms | 0.581× | 2.07% | PASS |
| file_reads | `groupby_scan` | 30.69ms | 33.35ms | 1.087× | 1.19% | PASS |
| file_reads | `index_join` | 10.30ms | 10.04ms | 0.975× | 1.95% | PASS |
| file_reads | `index_join_scan` | 4.49ms | 5.11ms | 1.136× | 2.25% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.33s | 1.286× | 0.64% | PASS |
| file_reads | `table_scan` | 1.16s | 1.40s | 1.206× | 0.68% | PASS |
| file_reads | `oltp_read_only` | 211.56ms | 161.07ms | 0.761× | 0.91% | PASS |
| file_writes | `oltp_bulk_insert` | 194.69ms | 270.26ms | 1.388× | 1.69% | PASS |
| file_writes | `oltp_insert` | 22.65ms | 35.99ms | 1.589× | 1.71% | PASS |
| file_writes | `oltp_update_index` | 78.24ms | 128.26ms | 1.639× | 1.27% | PASS |
| file_writes | `oltp_update_non_index` | 58.01ms | 81.64ms | 1.407× | 1.77% | PASS |
| file_writes | `oltp_delete_insert` | 69.08ms | 99.62ms | 1.442× | 1.81% | PASS |
| file_writes | `oltp_write_only` | 45.13ms | 64.72ms | 1.434× | 1.68% | PASS |
| file_writes | `types_delete_insert` | 40.78ms | 53.80ms | 1.319× | 1.68% | PASS |
| file_writes | `oltp_read_write` | 92.62ms | 130.12ms | 1.405× | 1.72% | PASS |
| ac_reads | `oltp_point_select` | 48.39ms | 54.92ms | 1.135× | 0.94% | PASS |
| ac_reads | `oltp_range_select` | 12.86ms | 15.84ms | 1.231× | 1.50% | PASS |
| ac_reads | `oltp_sum_range` | 12.37ms | 15.11ms | 1.222× | 1.38% | PASS |
| ac_reads | `oltp_order_range` | 2.93ms | 3.38ms | 1.154× | 1.88% | PASS |
| ac_reads | `oltp_distinct_range` | 4.02ms | 4.43ms | 1.101× | 1.73% | PASS |
| ac_reads | `oltp_index_scan` | 6.61ms | 8.14ms | 1.231× | 2.19% | PASS |
| ac_reads | `select_random_points` | 13.06ms | 13.87ms | 1.062× | 1.85% | PASS |
| ac_reads | `select_random_ranges` | 5.51ms | 6.61ms | 1.200× | 1.27% | PASS |
| ac_reads | `covering_index_scan` | 6.90ms | 7.01ms | 1.016× | 1.25% | PASS |
| ac_reads | `groupby_scan` | 30.21ms | 33.36ms | 1.104× | 0.88% | PASS |
| ac_reads | `index_join` | 7.65ms | 10.04ms | 1.312× | 1.39% | PASS |
| ac_reads | `index_join_scan` | 3.94ms | 5.07ms | 1.287× | 2.58% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.32s | 1.280× | 0.48% | PASS |
| ac_reads | `table_scan` | 1.16s | 1.39s | 1.196× | 0.62% | PASS |
| ac_reads | `oltp_read_only` | 138.44ms | 160.90ms | 1.162× | 0.73% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.00ms | 85.41ms | 3.714× | 6.77% | PASS |
| ac_writes | `oltp_insert_ac` | 25.59ms | 104.02ms | 4.064× | 6.97% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.20ms | 122.03ms | 4.328× | 5.50% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.42ms | 97.45ms | 4.162× | 5.79% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.35ms | 111.67ms | 4.405× | 6.50% | PASS |
| ac_writes | `oltp_write_only_ac` | 28.09ms | 108.36ms | 3.857× | 9.36% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.12ms | 101.17ms | 4.375× | 6.81% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.74ms | 118.45ms | 3.854× | 7.80% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.04ms | 35.26ms | 1.136× | 1.79% | PASS |
| mem_reads | `oltp_range_select` | 14.95ms | 13.99ms | 0.935× | 2.53% | PASS |
| mem_reads | `oltp_sum_range` | 12.60ms | 13.44ms | 1.067× | 1.88% | PASS |
| mem_reads | `oltp_order_range` | 3.22ms | 3.21ms | 0.995× | 1.66% | PASS |
| mem_reads | `oltp_distinct_range` | 4.30ms | 4.23ms | 0.984× | 1.03% | PASS |
| mem_reads | `oltp_index_scan` | 4.76ms | 6.05ms | 1.273× | 2.13% | PASS |
| mem_reads | `select_random_points` | 18.87ms | 20.56ms | 1.090× | 2.09% | PASS |
| mem_reads | `select_random_ranges` | 4.35ms | 5.39ms | 1.238× | 1.46% | PASS |
| mem_reads | `covering_index_scan` | 5.21ms | 4.80ms | 0.921× | 3.04% | PASS |
| mem_reads | `groupby_scan` | 34.98ms | 35.94ms | 1.027× | 1.12% | PASS |
| mem_reads | `index_join` | 7.38ms | 9.76ms | 1.323× | 3.57% | PASS |
| mem_reads | `index_join_scan` | 5.18ms | 5.97ms | 1.151× | 4.61% | PASS |
| mem_reads | `types_table_scan` | 1.27s | 1.30s | 1.022× | 4.73% | PASS |
| mem_reads | `table_scan` | 1.56s | 1.40s | 0.899× | 6.69% | PASS |
| mem_reads | `oltp_read_only` | 124.90ms | 130.55ms | 1.045× | 1.58% | PASS |
| mem_writes | `oltp_bulk_insert` | 231.72ms | 337.20ms | 1.455× | 1.03% | PASS |
| mem_writes | `oltp_insert` | 22.84ms | 39.61ms | 1.734× | 1.38% | PASS |
| mem_writes | `oltp_update_index` | 75.17ms | 138.69ms | 1.845× | 0.87% | PASS |
| mem_writes | `oltp_update_non_index` | 52.25ms | 90.58ms | 1.734× | 1.57% | PASS |
| mem_writes | `oltp_delete_insert` | 55.13ms | 108.47ms | 1.967× | 1.74% | PASS |
| mem_writes | `oltp_write_only` | 32.52ms | 67.35ms | 2.071× | 1.93% | PASS |
| mem_writes | `types_delete_insert` | 34.79ms | 56.23ms | 1.616× | 1.79% | PASS |
| mem_writes | `oltp_read_write` | 87.81ms | 136.51ms | 1.555× | 1.64% | PASS |
| file_reads | `oltp_point_select` | 127.74ms | 67.64ms | 0.530× | 0.81% | PASS |
| file_reads | `oltp_range_select` | 24.89ms | 17.41ms | 0.699× | 2.83% | PASS |
| file_reads | `oltp_sum_range` | 23.14ms | 17.02ms | 0.735× | 2.70% | PASS |
| file_reads | `oltp_order_range` | 4.34ms | 3.60ms | 0.828× | 2.12% | PASS |
| file_reads | `oltp_distinct_range` | 5.39ms | 4.62ms | 0.856× | 1.41% | PASS |
| file_reads | `oltp_index_scan` | 14.67ms | 9.64ms | 0.657× | 1.69% | PASS |
| file_reads | `select_random_points` | 29.34ms | 24.27ms | 0.827× | 1.26% | PASS |
| file_reads | `select_random_ranges` | 14.07ms | 8.67ms | 0.616× | 0.90% | PASS |
| file_reads | `covering_index_scan` | 15.91ms | 8.15ms | 0.513× | 1.42% | PASS |
| file_reads | `groupby_scan` | 35.84ms | 36.14ms | 1.008× | 0.96% | PASS |
| file_reads | `index_join` | 13.46ms | 11.89ms | 0.884× | 1.98% | PASS |
| file_reads | `index_join_scan` | 6.33ms | 6.50ms | 1.027× | 4.27% | PASS |
| file_reads | `types_table_scan` | 1.15s | 1.25s | 1.084× | 0.87% | PASS |
| file_reads | `table_scan` | 1.47s | 1.38s | 0.941× | 4.11% | PASS |
| file_reads | `oltp_read_only` | 265.65ms | 177.48ms | 0.668× | 1.25% | PASS |
| file_writes | `oltp_bulk_insert` | 254.44ms | 369.44ms | 1.452× | 0.85% | PASS |
| file_writes | `oltp_insert` | 58.97ms | 53.51ms | 0.907× | 22.90% | PASS |
| file_writes | `oltp_update_index` | 123.47ms | 179.84ms | 1.457× | 1.65% | PASS |
| file_writes | `oltp_update_non_index` | 117.87ms | 119.53ms | 1.014× | 12.15% | PASS |
| file_writes | `oltp_delete_insert` | 95.37ms | 141.38ms | 1.482× | 1.40% | PASS |
| file_writes | `oltp_write_only` | 90.01ms | 90.93ms | 1.010× | 8.38% | PASS |
| file_writes | `types_delete_insert` | 59.40ms | 77.88ms | 1.311× | 1.75% | PASS |
| file_writes | `oltp_read_write` | 144.94ms | 167.30ms | 1.154× | 9.49% | PASS |
| ac_reads | `oltp_point_select` | 64.10ms | 68.68ms | 1.071× | 1.23% | PASS |
| ac_reads | `oltp_range_select` | 18.88ms | 17.39ms | 0.921× | 1.57% | PASS |
| ac_reads | `oltp_sum_range` | 16.96ms | 17.09ms | 1.008× | 1.60% | PASS |
| ac_reads | `oltp_order_range` | 3.81ms | 3.60ms | 0.944× | 0.98% | PASS |
| ac_reads | `oltp_distinct_range` | 4.80ms | 4.62ms | 0.962× | 0.92% | PASS |
| ac_reads | `oltp_index_scan` | 8.54ms | 9.60ms | 1.125× | 1.35% | PASS |
| ac_reads | `select_random_points` | 22.78ms | 24.09ms | 1.057× | 2.07% | PASS |
| ac_reads | `select_random_ranges` | 7.71ms | 8.63ms | 1.120× | 1.09% | PASS |
| ac_reads | `covering_index_scan` | 9.70ms | 8.14ms | 0.840× | 1.54% | PASS |
| ac_reads | `groupby_scan` | 35.15ms | 36.21ms | 1.030× | 0.76% | PASS |
| ac_reads | `index_join` | 10.07ms | 11.61ms | 1.153× | 1.33% | PASS |
| ac_reads | `index_join_scan` | 5.68ms | 6.28ms | 1.105× | 1.46% | PASS |
| ac_reads | `types_table_scan` | 1.17s | 1.25s | 1.072× | 1.21% | PASS |
| ac_reads | `table_scan` | 1.46s | 1.38s | 0.947× | 4.54% | PASS |
| ac_reads | `oltp_read_only` | 171.14ms | 177.57ms | 1.038× | 1.44% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.98ms | 66.07ms | 3.890× | 5.87% | PASS |
| ac_writes | `oltp_insert_ac` | 20.47ms | 80.77ms | 3.946× | 5.22% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.65ms | 100.62ms | 4.648× | 4.42% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.14ms | 76.17ms | 4.720× | 3.69% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.71ms | 88.88ms | 4.752× | 3.67% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.37ms | 88.37ms | 4.563× | 4.80% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.12ms | 79.73ms | 4.944× | 6.17% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.86ms | 99.45ms | 3.703× | 5.00% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.63ms | 37.20ms | 1.140× | 1.62% | PASS |
| mem_reads | `oltp_range_select` | 15.52ms | 14.15ms | 0.912× | 3.04% | PASS |
| mem_reads | `oltp_sum_range` | 13.36ms | 13.72ms | 1.027× | 2.40% | PASS |
| mem_reads | `oltp_order_range` | 3.27ms | 3.22ms | 0.986× | 1.17% | PASS |
| mem_reads | `oltp_distinct_range` | 4.35ms | 4.25ms | 0.977× | 0.86% | PASS |
| mem_reads | `oltp_index_scan` | 4.97ms | 6.40ms | 1.287× | 2.20% | PASS |
| mem_reads | `select_random_points` | 18.58ms | 20.75ms | 1.117× | 2.61% | PASS |
| mem_reads | `select_random_ranges` | 4.29ms | 5.30ms | 1.236× | 1.48% | PASS |
| mem_reads | `covering_index_scan` | 4.61ms | 4.65ms | 1.008× | 2.54% | PASS |
| mem_reads | `groupby_scan` | 34.50ms | 35.57ms | 1.031× | 1.00% | PASS |
| mem_reads | `index_join` | 7.05ms | 9.66ms | 1.370× | 2.51% | PASS |
| mem_reads | `index_join_scan` | 4.65ms | 5.90ms | 1.269× | 3.62% | PASS |
| mem_reads | `types_table_scan` | 1.14s | 1.25s | 1.097× | 1.13% | PASS |
| mem_reads | `table_scan` | 1.41s | 1.39s | 0.982× | 3.73% | PASS |
| mem_reads | `oltp_read_only` | 125.55ms | 131.49ms | 1.047× | 1.63% | PASS |
| mem_writes | `oltp_bulk_insert` | 237.62ms | 334.77ms | 1.409× | 1.03% | PASS |
| mem_writes | `oltp_insert` | 20.84ms | 39.96ms | 1.918× | 1.37% | PASS |
| mem_writes | `oltp_update_index` | 71.42ms | 134.87ms | 1.888× | 2.15% | PASS |
| mem_writes | `oltp_update_non_index` | 50.12ms | 85.77ms | 1.711× | 1.08% | PASS |
| mem_writes | `oltp_delete_insert` | 51.33ms | 105.97ms | 2.064× | 1.43% | PASS |
| mem_writes | `oltp_write_only` | 29.87ms | 65.53ms | 2.194× | 1.49% | PASS |
| mem_writes | `types_delete_insert` | 32.79ms | 52.92ms | 1.614× | 0.82% | PASS |
| mem_writes | `oltp_read_write` | 82.01ms | 134.02ms | 1.634× | 0.98% | PASS |
| file_reads | `oltp_point_select` | 130.24ms | 69.82ms | 0.536× | 1.12% | PASS |
| file_reads | `oltp_range_select` | 24.23ms | 17.12ms | 0.707× | 3.29% | PASS |
| file_reads | `oltp_sum_range` | 22.43ms | 16.56ms | 0.738× | 2.03% | PASS |
| file_reads | `oltp_order_range` | 4.24ms | 3.55ms | 0.838× | 1.89% | PASS |
| file_reads | `oltp_distinct_range` | 5.22ms | 4.58ms | 0.876× | 1.74% | PASS |
| file_reads | `oltp_index_scan` | 14.55ms | 9.40ms | 0.647× | 1.47% | PASS |
| file_reads | `select_random_points` | 27.96ms | 23.23ms | 0.831× | 2.21% | PASS |
| file_reads | `select_random_ranges` | 14.06ms | 8.52ms | 0.606× | 0.98% | PASS |
| file_reads | `covering_index_scan` | 15.25ms | 7.99ms | 0.524× | 1.63% | PASS |
| file_reads | `groupby_scan` | 35.05ms | 35.74ms | 1.020× | 1.00% | PASS |
| file_reads | `index_join` | 12.67ms | 11.44ms | 0.903× | 2.26% | PASS |
| file_reads | `index_join_scan` | 5.67ms | 6.23ms | 1.100× | 2.68% | PASS |
| file_reads | `types_table_scan` | 1.16s | 1.26s | 1.088× | 1.96% | PASS |
| file_reads | `table_scan` | 1.53s | 1.42s | 0.931× | 4.52% | PASS |
| file_reads | `oltp_read_only` | 268.67ms | 178.16ms | 0.663× | 1.03% | PASS |
| file_writes | `oltp_bulk_insert` | 260.37ms | 360.98ms | 1.386× | 0.78% | PASS |
| file_writes | `oltp_insert` | 32.37ms | 53.68ms | 1.658× | 1.78% | PASS |
| file_writes | `oltp_update_index` | 110.27ms | 173.18ms | 1.570× | 1.94% | PASS |
| file_writes | `oltp_update_non_index` | 83.90ms | 113.49ms | 1.353× | 1.34% | PASS |
| file_writes | `oltp_delete_insert` | 87.06ms | 138.36ms | 1.589× | 1.92% | PASS |
| file_writes | `oltp_write_only` | 59.17ms | 90.87ms | 1.536× | 2.64% | PASS |
| file_writes | `types_delete_insert` | 54.66ms | 74.26ms | 1.359× | 2.01% | PASS |
| file_writes | `oltp_read_write` | 117.91ms | 161.59ms | 1.370× | 2.60% | PASS |
| ac_reads | `oltp_point_select` | 64.90ms | 69.35ms | 1.068× | 1.01% | PASS |
| ac_reads | `oltp_range_select` | 18.82ms | 17.25ms | 0.916× | 2.05% | PASS |
| ac_reads | `oltp_sum_range` | 16.41ms | 16.55ms | 1.008× | 1.42% | PASS |
| ac_reads | `oltp_order_range` | 3.74ms | 3.58ms | 0.955× | 1.08% | PASS |
| ac_reads | `oltp_distinct_range` | 4.75ms | 4.59ms | 0.968× | 0.94% | PASS |
| ac_reads | `oltp_index_scan` | 8.53ms | 9.59ms | 1.125× | 0.88% | PASS |
| ac_reads | `select_random_points` | 23.34ms | 24.22ms | 1.037× | 1.61% | PASS |
| ac_reads | `select_random_ranges` | 7.96ms | 8.62ms | 1.083× | 1.32% | PASS |
| ac_reads | `covering_index_scan` | 9.00ms | 8.05ms | 0.895× | 1.17% | PASS |
| ac_reads | `groupby_scan` | 35.07ms | 36.02ms | 1.027× | 0.76% | PASS |
| ac_reads | `index_join` | 10.04ms | 11.93ms | 1.187× | 2.34% | PASS |
| ac_reads | `index_join_scan` | 5.40ms | 6.41ms | 1.188× | 2.95% | PASS |
| ac_reads | `types_table_scan` | 1.30s | 1.31s | 1.007× | 0.97% | PASS |
| ac_reads | `table_scan` | 1.60s | 1.44s | 0.901× | 1.56% | PASS |
| ac_reads | `oltp_read_only` | 180.79ms | 180.26ms | 0.997× | 1.38% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.57ms | 64.73ms | 3.907× | 4.97% | PASS |
| ac_writes | `oltp_insert_ac` | 19.41ms | 87.59ms | 4.512× | 4.35% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.99ms | 100.58ms | 4.574× | 4.41% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.65ms | 79.68ms | 4.515× | 4.87% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.91ms | 93.65ms | 4.953× | 4.65% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.73ms | 88.61ms | 4.492× | 4.34% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.51ms | 80.04ms | 4.847× | 5.69% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.36ms | 97.69ms | 3.705× | 4.17% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.30ms | 36.99ms | 1.111× | 1.57% | PASS |
| mem_reads | `oltp_range_select` | 19.30ms | 19.77ms | 1.024× | 0.89% | PASS |
| mem_reads | `oltp_sum_range` | 18.09ms | 18.83ms | 1.041× | 0.61% | PASS |
| mem_reads | `oltp_order_range` | 3.66ms | 3.72ms | 1.014× | 0.90% | PASS |
| mem_reads | `oltp_distinct_range` | 4.73ms | 4.75ms | 1.005× | 0.82% | PASS |
| mem_reads | `oltp_index_scan` | 4.66ms | 5.58ms | 1.198× | 1.00% | PASS |
| mem_reads | `select_random_points` | 27.12ms | 29.70ms | 1.095× | 0.54% | PASS |
| mem_reads | `select_random_ranges` | 7.50ms | 8.06ms | 1.075× | 0.90% | PASS |
| mem_reads | `covering_index_scan` | 4.39ms | 4.08ms | 0.928× | 0.73% | PASS |
| mem_reads | `groupby_scan` | 38.35ms | 39.90ms | 1.040× | 0.70% | PASS |
| mem_reads | `index_join` | 8.07ms | 9.92ms | 1.229× | 0.68% | PASS |
| mem_reads | `index_join_scan` | 4.22ms | 5.48ms | 1.298× | 0.79% | PASS |
| mem_reads | `types_table_scan` | 1.12s | 1.25s | 1.110× | 0.57% | PASS |
| mem_reads | `table_scan` | 1.29s | 1.35s | 1.052× | 0.44% | PASS |
| mem_reads | `oltp_read_only` | 149.45ms | 157.87ms | 1.056× | 0.61% | PASS |
| mem_writes | `oltp_bulk_insert` | 244.28ms | 336.35ms | 1.377× | 1.28% | PASS |
| mem_writes | `oltp_insert` | 19.45ms | 35.84ms | 1.843× | 0.66% | PASS |
| mem_writes | `oltp_update_index` | 67.53ms | 115.96ms | 1.717× | 0.59% | PASS |
| mem_writes | `oltp_update_non_index` | 50.63ms | 82.44ms | 1.628× | 0.87% | PASS |
| mem_writes | `oltp_delete_insert` | 49.39ms | 94.84ms | 1.920× | 0.64% | PASS |
| mem_writes | `oltp_write_only` | 26.92ms | 58.25ms | 2.164× | 0.47% | PASS |
| mem_writes | `types_delete_insert` | 32.16ms | 52.83ms | 1.643× | 0.51% | PASS |
| mem_writes | `oltp_read_write` | 97.48ms | 145.69ms | 1.495× | 0.70% | PASS |
| file_reads | `oltp_point_select` | 129.51ms | 69.35ms | 0.535× | 0.85% | PASS |
| file_reads | `oltp_range_select` | 29.44ms | 23.19ms | 0.788× | 0.60% | PASS |
| file_reads | `oltp_sum_range` | 27.92ms | 22.41ms | 0.803× | 0.79% | PASS |
| file_reads | `oltp_order_range` | 4.70ms | 4.11ms | 0.874× | 0.61% | PASS |
| file_reads | `oltp_distinct_range` | 5.75ms | 5.19ms | 0.902× | 0.53% | PASS |
| file_reads | `oltp_index_scan` | 14.54ms | 9.12ms | 0.627× | 0.64% | PASS |
| file_reads | `select_random_points` | 37.49ms | 33.45ms | 0.892× | 0.69% | PASS |
| file_reads | `select_random_ranges` | 17.34ms | 11.54ms | 0.666× | 0.75% | PASS |
| file_reads | `covering_index_scan` | 14.32ms | 7.53ms | 0.526× | 0.90% | PASS |
| file_reads | `groupby_scan` | 39.59ms | 40.61ms | 1.026× | 0.57% | PASS |
| file_reads | `index_join` | 13.43ms | 12.30ms | 0.916× | 0.74% | PASS |
| file_reads | `index_join_scan` | 5.32ms | 6.02ms | 1.131× | 0.88% | PASS |
| file_reads | `types_table_scan` | 1.12s | 1.24s | 1.111× | 0.37% | PASS |
| file_reads | `table_scan` | 1.29s | 1.35s | 1.048× | 0.41% | PASS |
| file_reads | `oltp_read_only` | 288.05ms | 205.26ms | 0.713× | 0.58% | PASS |
| file_writes | `oltp_bulk_insert` | 261.26ms | 360.36ms | 1.379× | 0.71% | PASS |
| file_writes | `oltp_insert` | 26.11ms | 46.38ms | 1.777× | 0.98% | PASS |
| file_writes | `oltp_update_index` | 98.69ms | 143.82ms | 1.457× | 0.89% | PASS |
| file_writes | `oltp_update_non_index` | 77.74ms | 104.94ms | 1.350× | 1.18% | PASS |
| file_writes | `oltp_delete_insert` | 77.67ms | 119.89ms | 1.543× | 1.07% | PASS |
| file_writes | `oltp_write_only` | 50.55ms | 79.08ms | 1.564× | 1.28% | PASS |
| file_writes | `types_delete_insert` | 49.87ms | 66.65ms | 1.337× | 1.03% | PASS |
| file_writes | `oltp_read_write` | 121.81ms | 167.05ms | 1.371× | 1.10% | PASS |
| ac_reads | `oltp_point_select` | 63.97ms | 69.23ms | 1.082× | 1.19% | PASS |
| ac_reads | `oltp_range_select` | 22.42ms | 23.16ms | 1.033× | 0.66% | PASS |
| ac_reads | `oltp_sum_range` | 21.14ms | 22.40ms | 1.060× | 0.90% | PASS |
| ac_reads | `oltp_order_range` | 4.03ms | 4.11ms | 1.020× | 0.85% | PASS |
| ac_reads | `oltp_distinct_range` | 5.04ms | 5.18ms | 1.027× | 0.74% | PASS |
| ac_reads | `oltp_index_scan` | 7.92ms | 9.15ms | 1.155× | 0.74% | PASS |
| ac_reads | `select_random_points` | 30.43ms | 33.89ms | 1.114× | 0.68% | PASS |
| ac_reads | `select_random_ranges` | 10.70ms | 11.58ms | 1.083× | 0.67% | PASS |
| ac_reads | `covering_index_scan` | 7.70ms | 7.55ms | 0.981× | 0.83% | PASS |
| ac_reads | `groupby_scan` | 38.54ms | 40.64ms | 1.055× | 0.59% | PASS |
| ac_reads | `index_join` | 10.00ms | 12.30ms | 1.229× | 0.49% | PASS |
| ac_reads | `index_join_scan` | 4.63ms | 6.03ms | 1.303× | 0.62% | PASS |
| ac_reads | `types_table_scan` | 1.12s | 1.24s | 1.112× | 0.58% | PASS |
| ac_reads | `table_scan` | 1.29s | 1.35s | 1.050× | 0.40% | PASS |
| ac_reads | `oltp_read_only` | 194.43ms | 205.36ms | 1.056× | 0.71% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.47ms | 60.84ms | 3.933× | 4.17% | PASS |
| ac_writes | `oltp_insert_ac` | 17.59ms | 81.08ms | 4.609× | 3.06% | PASS |
| ac_writes | `oltp_update_index_ac` | 18.90ms | 92.58ms | 4.899× | 3.18% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 15.67ms | 71.55ms | 4.566× | 2.77% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.08ms | 83.48ms | 4.888× | 2.16% | PASS |
| ac_writes | `oltp_write_only_ac` | 17.45ms | 83.77ms | 4.800× | 2.48% | PASS |
| ac_writes | `types_delete_insert_ac` | 15.28ms | 71.59ms | 4.687× | 4.91% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.61ms | 92.45ms | 3.757× | 3.58% | PASS |

</details>

## Version-control latency

Wall time: 2m 16s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 81.81ms | 200.00ms | 40.9% | 0.33% | PASS |
| `status_dirty_many_tables` | 84.98ms | 200.00ms | 42.5% | 0.33% | PASS |
| `diff_regular_working_one_table` | 77.61ms | 150.00ms | 51.7% | 0.32% | PASS |
| `diff_regular_working_many_tables` | 90.25ms | 200.00ms | 45.1% | 0.32% | PASS |
| `diff_stat_working_many_tables` | 90.20ms | 200.00ms | 45.1% | 0.43% | PASS |
| `diff_schema_working_many_tables` | 90.98ms | 200.00ms | 45.5% | 0.29% | PASS |
| `branch_list_many_branches` | 22.14ms | 100.00ms | 22.1% | 0.76% | PASS |
| `branch_create_delete` | 24.06ms | 100.00ms | 24.1% | 0.92% | PASS |
| `checkout_branch_clean` | 54.15ms | 200.00ms | 27.1% | 0.59% | PASS |
| `merge_data_no_conflicts` | 28.22ms | 150.00ms | 18.8% | 0.94% | PASS |
| `merge_schema_no_conflicts` | 21.49ms | 100.00ms | 21.5% | 0.97% | PASS |
| `merge_data_conflicts` | 125.33ms | 250.00ms | 50.1% | 0.19% | PASS |
| `merge_data_conflicts_with_resolve` | 125.36ms | 250.00ms | 50.1% | 0.27% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
