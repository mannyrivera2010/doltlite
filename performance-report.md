# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-23 15:51 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261012.334.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/39968880374)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 3m 28s | 7.61s | 8.84s | 1.162× | 0.76% | **PASS** |
| textpk | 69 | 55 | 1h 16m 24s | 8.48s | 8.57s | 1.011× | 2.58% | **PASS** |
| blobpk | 69 | 55 | 1h 27m 16s | 9.11s | 10.24s | 1.124× | 2.10% | **PASS** |
| compositepk | 69 | 55 | 1h 26m 53s | 9.59s | 11.90s | 1.240× | 1.35% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 19.18ms | 21.58ms | 1.125× | 1.71% | PASS |
| mem_reads | `oltp_range_select` | 8.36ms | 9.28ms | 1.109× | 0.92% | PASS |
| mem_reads | `oltp_sum_range` | 7.63ms | 8.94ms | 1.173× | 0.76% | PASS |
| mem_reads | `oltp_order_range` | 2.09ms | 2.23ms | 1.068× | 1.02% | PASS |
| mem_reads | `oltp_distinct_range` | 2.96ms | 3.02ms | 1.023× | 0.80% | PASS |
| mem_reads | `oltp_index_scan` | 3.09ms | 3.82ms | 1.235× | 0.99% | PASS |
| mem_reads | `select_random_points` | 8.20ms | 8.80ms | 1.073× | 0.76% | PASS |
| mem_reads | `select_random_ranges` | 2.42ms | 3.12ms | 1.293× | 1.07% | PASS |
| mem_reads | `covering_index_scan` | 3.37ms | 3.15ms | 0.935× | 0.77% | PASS |
| mem_reads | `groupby_scan` | 24.77ms | 26.65ms | 1.076× | 0.44% | PASS |
| mem_reads | `index_join` | 4.71ms | 6.07ms | 1.288× | 0.68% | PASS |
| mem_reads | `index_join_scan` | 2.76ms | 3.78ms | 1.367× | 0.90% | PASS |
| mem_reads | `types_table_scan` | 860.74ms | 979.40ms | 1.138× | 0.36% | PASS |
| mem_reads | `table_scan` | 971.69ms | 1.06s | 1.096× | 0.26% | PASS |
| mem_reads | `oltp_read_only` | 80.65ms | 89.11ms | 1.105× | 0.62% | PASS |
| mem_writes | `oltp_bulk_insert` | 139.11ms | 183.99ms | 1.323× | 0.94% | PASS |
| mem_writes | `oltp_insert` | 12.27ms | 21.28ms | 1.735× | 0.66% | PASS |
| mem_writes | `oltp_update_index` | 39.58ms | 79.03ms | 1.997× | 0.64% | PASS |
| mem_writes | `oltp_update_non_index` | 27.20ms | 43.63ms | 1.604× | 0.61% | PASS |
| mem_writes | `oltp_delete_insert` | 34.84ms | 58.66ms | 1.684× | 0.70% | PASS |
| mem_writes | `oltp_write_only` | 17.23ms | 33.36ms | 1.936× | 0.76% | PASS |
| mem_writes | `types_delete_insert` | 19.30ms | 29.48ms | 1.527× | 0.66% | PASS |
| mem_writes | `oltp_read_write` | 50.39ms | 79.16ms | 1.571× | 0.67% | PASS |
| file_reads | `oltp_point_select` | 93.82ms | 46.37ms | 0.494× | 0.57% | PASS |
| file_reads | `oltp_range_select` | 16.62ms | 11.94ms | 0.718× | 0.63% | PASS |
| file_reads | `oltp_sum_range` | 15.70ms | 11.60ms | 0.739× | 0.45% | PASS |
| file_reads | `oltp_order_range` | 3.02ms | 2.57ms | 0.851× | 0.80% | PASS |
| file_reads | `oltp_distinct_range` | 3.85ms | 3.33ms | 0.865× | 0.76% | PASS |
| file_reads | `oltp_index_scan` | 10.96ms | 6.74ms | 0.615× | 0.82% | PASS |
| file_reads | `select_random_points` | 16.53ms | 11.70ms | 0.708× | 0.62% | PASS |
| file_reads | `select_random_ranges` | 10.06ms | 5.70ms | 0.567× | 0.76% | PASS |
| file_reads | `covering_index_scan` | 11.25ms | 5.95ms | 0.529× | 0.57% | PASS |
| file_reads | `groupby_scan` | 25.86ms | 27.06ms | 1.046× | 0.50% | PASS |
| file_reads | `index_join` | 8.98ms | 8.00ms | 0.891× | 0.99% | PASS |
| file_reads | `index_join_scan` | 3.70ms | 4.15ms | 1.121× | 0.92% | PASS |
| file_reads | `types_table_scan` | 852.15ms | 978.95ms | 1.149× | 0.37% | PASS |
| file_reads | `table_scan` | 966.88ms | 1.07s | 1.102× | 0.24% | PASS |
| file_reads | `oltp_read_only` | 187.01ms | 124.25ms | 0.664× | 0.48% | PASS |
| file_writes | `oltp_bulk_insert` | 180.55ms | 238.99ms | 1.324× | 3.82% | PASS |
| file_writes | `oltp_insert` | 19.79ms | 36.77ms | 1.858× | 6.40% | PASS |
| file_writes | `oltp_update_index` | 131.48ms | 129.73ms | 0.987× | 0.51% | PASS |
| file_writes | `oltp_update_non_index` | 100.94ms | 98.73ms | 0.978× | 0.35% | PASS |
| file_writes | `oltp_delete_insert` | 111.23ms | 109.07ms | 0.981× | 0.80% | PASS |
| file_writes | `oltp_write_only` | 82.58ms | 79.29ms | 0.960× | 2.62% | PASS |
| file_writes | `types_delete_insert` | 66.61ms | 59.94ms | 0.900× | 8.17% | PASS |
| file_writes | `oltp_read_write` | 125.88ms | 124.64ms | 0.990× | 7.02% | PASS |
| ac_reads | `oltp_point_select` | 43.50ms | 46.48ms | 1.068× | 0.70% | PASS |
| ac_reads | `oltp_range_select` | 11.39ms | 11.94ms | 1.047× | 0.94% | PASS |
| ac_reads | `oltp_sum_range` | 10.54ms | 11.64ms | 1.105× | 0.69% | PASS |
| ac_reads | `oltp_order_range` | 2.54ms | 2.58ms | 1.017× | 0.86% | PASS |
| ac_reads | `oltp_distinct_range` | 3.34ms | 3.33ms | 0.999× | 0.54% | PASS |
| ac_reads | `oltp_index_scan` | 5.92ms | 6.72ms | 1.136× | 1.15% | PASS |
| ac_reads | `select_random_points` | 11.41ms | 11.73ms | 1.028× | 0.59% | PASS |
| ac_reads | `select_random_ranges` | 5.06ms | 5.71ms | 1.128× | 0.79% | PASS |
| ac_reads | `covering_index_scan` | 6.14ms | 5.96ms | 0.970× | 0.52% | PASS |
| ac_reads | `groupby_scan` | 25.30ms | 27.07ms | 1.070× | 0.52% | PASS |
| ac_reads | `index_join` | 6.39ms | 8.00ms | 1.253× | 0.79% | PASS |
| ac_reads | `index_join_scan` | 3.24ms | 4.19ms | 1.293× | 0.77% | PASS |
| ac_reads | `types_table_scan` | 851.55ms | 979.73ms | 1.151× | 0.45% | PASS |
| ac_reads | `table_scan` | 966.12ms | 1.07s | 1.103× | 0.26% | PASS |
| ac_reads | `oltp_read_only` | 115.66ms | 124.35ms | 1.075× | 0.45% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.12ms | 54.77ms | 3.199× | 9.67% | PASS |
| ac_writes | `oltp_insert_ac` | 19.07ms | 68.80ms | 3.607× | 5.41% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.55ms | 84.97ms | 4.134× | 5.93% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.29ms | 62.85ms | 3.634× | 8.94% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.91ms | 76.16ms | 4.029× | 7.85% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.91ms | 75.83ms | 4.010× | 8.19% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.25ms | 68.29ms | 3.958× | 11.07% | PASS |
| ac_writes | `oltp_read_write_ac` | 23.48ms | 82.89ms | 3.531× | 15.32% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.14ms | 26.04ms | 1.125× | 1.84% | PASS |
| mem_reads | `oltp_range_select` | 12.01ms | 10.35ms | 0.862× | 3.57% | PASS |
| mem_reads | `oltp_sum_range` | 11.42ms | 10.60ms | 0.928× | 3.38% | PASS |
| mem_reads | `oltp_order_range` | 2.41ms | 2.18ms | 0.904× | 2.45% | PASS |
| mem_reads | `oltp_distinct_range` | 2.98ms | 2.71ms | 0.911× | 1.80% | PASS |
| mem_reads | `oltp_index_scan` | 3.84ms | 4.96ms | 1.292× | 2.97% | PASS |
| mem_reads | `select_random_points` | 16.52ms | 17.77ms | 1.076× | 2.91% | PASS |
| mem_reads | `select_random_ranges` | 3.39ms | 3.88ms | 1.146× | 2.85% | PASS |
| mem_reads | `covering_index_scan` | 3.81ms | 3.61ms | 0.947× | 6.50% | PASS |
| mem_reads | `groupby_scan` | 21.90ms | 21.09ms | 0.963× | 1.07% | PASS |
| mem_reads | `index_join` | 5.92ms | 8.46ms | 1.430× | 6.04% | PASS |
| mem_reads | `index_join_scan` | 4.35ms | 6.02ms | 1.384× | 4.73% | PASS |
| mem_reads | `types_table_scan` | 987.55ms | 904.98ms | 0.916× | 1.22% | PASS |
| mem_reads | `table_scan` | 1.32s | 1.04s | 0.787× | 1.20% | PASS |
| mem_reads | `oltp_read_only` | 105.62ms | 98.27ms | 0.930× | 1.76% | PASS |
| mem_writes | `oltp_bulk_insert` | 147.43ms | 231.43ms | 1.570× | 0.98% | PASS |
| mem_writes | `oltp_insert` | 17.24ms | 28.39ms | 1.646× | 3.03% | PASS |
| mem_writes | `oltp_update_index` | 65.85ms | 114.90ms | 1.745× | 2.79% | PASS |
| mem_writes | `oltp_update_non_index` | 40.64ms | 70.90ms | 1.745× | 1.67% | PASS |
| mem_writes | `oltp_delete_insert` | 45.15ms | 84.90ms | 1.881× | 2.38% | PASS |
| mem_writes | `oltp_write_only` | 25.24ms | 51.77ms | 2.051× | 3.19% | PASS |
| mem_writes | `types_delete_insert` | 26.97ms | 44.55ms | 1.652× | 1.59% | PASS |
| mem_writes | `oltp_read_write` | 76.61ms | 109.80ms | 1.433× | 2.16% | PASS |
| file_reads | `oltp_point_select` | 82.98ms | 45.80ms | 0.552× | 1.55% | PASS |
| file_reads | `oltp_range_select` | 16.20ms | 11.05ms | 0.682× | 1.76% | PASS |
| file_reads | `oltp_sum_range` | 15.51ms | 11.12ms | 0.717× | 2.24% | PASS |
| file_reads | `oltp_order_range` | 3.02ms | 2.34ms | 0.776× | 2.17% | PASS |
| file_reads | `oltp_distinct_range` | 3.59ms | 2.89ms | 0.806× | 1.85% | PASS |
| file_reads | `oltp_index_scan` | 10.08ms | 6.76ms | 0.671× | 1.45% | PASS |
| file_reads | `select_random_points` | 20.55ms | 16.73ms | 0.814× | 2.93% | PASS |
| file_reads | `select_random_ranges` | 9.12ms | 5.68ms | 0.623× | 1.32% | PASS |
| file_reads | `covering_index_scan` | 10.22ms | 5.38ms | 0.527× | 1.76% | PASS |
| file_reads | `groupby_scan` | 21.08ms | 20.16ms | 0.956× | 1.82% | PASS |
| file_reads | `index_join` | 8.82ms | 7.85ms | 0.890× | 2.42% | PASS |
| file_reads | `index_join_scan` | 4.47ms | 4.66ms | 1.041× | 2.35% | PASS |
| file_reads | `types_table_scan` | 726.65ms | 776.75ms | 1.069× | 1.72% | PASS |
| file_reads | `table_scan` | 890.84ms | 871.19ms | 0.978× | 7.40% | PASS |
| file_reads | `oltp_read_only` | 151.87ms | 101.20ms | 0.666× | 2.10% | PASS |
| file_writes | `oltp_bulk_insert` | 201.18ms | 298.00ms | 1.481× | 3.65% | PASS |
| file_writes | `oltp_insert` | 52.38ms | 54.84ms | 1.047× | 18.92% | PASS |
| file_writes | `oltp_update_index` | 202.20ms | 193.23ms | 0.956× | 5.63% | PASS |
| file_writes | `oltp_update_non_index` | 142.01ms | 129.52ms | 0.912× | 5.99% | PASS |
| file_writes | `oltp_delete_insert` | 163.56ms | 152.53ms | 0.933× | 5.32% | PASS |
| file_writes | `oltp_write_only` | 122.98ms | 109.37ms | 0.889× | 2.58% | PASS |
| file_writes | `types_delete_insert` | 100.81ms | 91.20ms | 0.905× | 8.49% | PASS |
| file_writes | `oltp_read_write` | 172.62ms | 168.81ms | 0.978× | 2.77% | PASS |
| ac_reads | `oltp_point_select` | 41.95ms | 45.04ms | 1.074× | 2.49% | PASS |
| ac_reads | `oltp_range_select` | 12.89ms | 11.28ms | 0.875× | 2.99% | PASS |
| ac_reads | `oltp_sum_range` | 12.30ms | 11.33ms | 0.921× | 2.48% | PASS |
| ac_reads | `oltp_order_range` | 2.66ms | 2.34ms | 0.877× | 1.11% | PASS |
| ac_reads | `oltp_distinct_range` | 3.20ms | 2.83ms | 0.887× | 1.12% | PASS |
| ac_reads | `oltp_index_scan` | 5.97ms | 6.61ms | 1.108× | 1.74% | PASS |
| ac_reads | `select_random_points` | 18.09ms | 17.81ms | 0.984× | 5.85% | PASS |
| ac_reads | `select_random_ranges` | 5.38ms | 5.79ms | 1.076× | 2.07% | PASS |
| ac_reads | `covering_index_scan` | 6.81ms | 5.62ms | 0.826× | 2.76% | PASS |
| ac_reads | `groupby_scan` | 21.83ms | 20.81ms | 0.953× | 1.50% | PASS |
| ac_reads | `index_join` | 7.31ms | 8.20ms | 1.123× | 3.00% | PASS |
| ac_reads | `index_join_scan` | 4.28ms | 4.73ms | 1.107× | 2.39% | PASS |
| ac_reads | `types_table_scan` | 765.87ms | 791.57ms | 1.034× | 3.65% | PASS |
| ac_reads | `table_scan` | 1.15s | 948.84ms | 0.824× | 2.47% | PASS |
| ac_reads | `oltp_read_only` | 120.65ms | 115.32ms | 0.956× | 3.01% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.36ms | 59.09ms | 2.643× | 4.30% | PASS |
| ac_writes | `oltp_insert_ac` | 24.61ms | 70.50ms | 2.864× | 5.27% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.14ms | 82.89ms | 3.171× | 5.43% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.39ms | 68.38ms | 3.054× | 5.29% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.77ms | 82.22ms | 3.319× | 9.06% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.35ms | 76.39ms | 3.137× | 8.49% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.57ms | 71.40ms | 3.163× | 5.76% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.64ms | 82.00ms | 2.864× | 3.83% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.56ms | 30.42ms | 1.145× | 1.82% | PASS |
| mem_reads | `oltp_range_select` | 12.08ms | 11.98ms | 0.992× | 2.30% | PASS |
| mem_reads | `oltp_sum_range` | 11.03ms | 11.72ms | 1.063× | 2.23% | PASS |
| mem_reads | `oltp_order_range` | 2.73ms | 2.86ms | 1.047× | 1.71% | PASS |
| mem_reads | `oltp_distinct_range` | 3.65ms | 3.66ms | 1.005× | 2.50% | PASS |
| mem_reads | `oltp_index_scan` | 4.06ms | 5.21ms | 1.285× | 1.82% | PASS |
| mem_reads | `select_random_points` | 15.47ms | 17.68ms | 1.143× | 2.55% | PASS |
| mem_reads | `select_random_ranges` | 3.74ms | 4.63ms | 1.237× | 1.88% | PASS |
| mem_reads | `covering_index_scan` | 3.79ms | 3.79ms | 1.000× | 3.58% | PASS |
| mem_reads | `groupby_scan` | 28.71ms | 29.61ms | 1.031× | 4.45% | PASS |
| mem_reads | `index_join` | 5.77ms | 7.45ms | 1.290× | 3.06% | PASS |
| mem_reads | `index_join_scan` | 3.83ms | 5.07ms | 1.324× | 3.97% | PASS |
| mem_reads | `types_table_scan` | 935.19ms | 1.03s | 1.100× | 0.97% | PASS |
| mem_reads | `table_scan` | 1.11s | 1.15s | 1.031× | 1.08% | PASS |
| mem_reads | `oltp_read_only` | 103.02ms | 110.81ms | 1.076× | 1.77% | PASS |
| mem_writes | `oltp_bulk_insert` | 199.78ms | 278.18ms | 1.392× | 1.55% | PASS |
| mem_writes | `oltp_insert` | 17.43ms | 32.22ms | 1.849× | 1.63% | PASS |
| mem_writes | `oltp_update_index` | 61.26ms | 111.81ms | 1.825× | 1.61% | PASS |
| mem_writes | `oltp_update_non_index` | 43.46ms | 71.14ms | 1.637× | 1.82% | PASS |
| mem_writes | `oltp_delete_insert` | 43.65ms | 87.35ms | 2.001× | 1.50% | PASS |
| mem_writes | `oltp_write_only` | 25.27ms | 52.90ms | 2.094× | 2.45% | PASS |
| mem_writes | `types_delete_insert` | 27.78ms | 42.67ms | 1.536× | 3.31% | PASS |
| mem_writes | `oltp_read_write` | 70.67ms | 111.03ms | 1.571× | 2.45% | PASS |
| file_reads | `oltp_point_select` | 107.25ms | 56.89ms | 0.530× | 3.74% | PASS |
| file_reads | `oltp_range_select` | 19.89ms | 13.76ms | 0.692× | 3.83% | PASS |
| file_reads | `oltp_sum_range` | 19.70ms | 14.26ms | 0.723× | 1.06% | PASS |
| file_reads | `oltp_order_range` | 3.66ms | 3.11ms | 0.849× | 1.97% | PASS |
| file_reads | `oltp_distinct_range` | 4.61ms | 4.00ms | 0.869× | 1.38% | PASS |
| file_reads | `oltp_index_scan` | 12.01ms | 7.88ms | 0.656× | 3.03% | PASS |
| file_reads | `select_random_points` | 23.06ms | 19.32ms | 0.838× | 2.53% | PASS |
| file_reads | `select_random_ranges` | 11.40ms | 6.91ms | 0.606× | 1.93% | PASS |
| file_reads | `covering_index_scan` | 12.30ms | 6.52ms | 0.530× | 3.06% | PASS |
| file_reads | `groupby_scan` | 28.50ms | 28.66ms | 1.006× | 3.01% | PASS |
| file_reads | `index_join` | 10.46ms | 9.68ms | 0.926× | 2.10% | PASS |
| file_reads | `index_join_scan` | 4.83ms | 5.42ms | 1.123× | 3.56% | PASS |
| file_reads | `types_table_scan` | 915.54ms | 997.39ms | 1.089× | 0.85% | PASS |
| file_reads | `table_scan` | 1.16s | 1.18s | 1.020× | 1.22% | PASS |
| file_reads | `oltp_read_only` | 224.01ms | 151.22ms | 0.675× | 1.57% | PASS |
| file_writes | `oltp_bulk_insert` | 261.58ms | 363.35ms | 1.389× | 3.23% | PASS |
| file_writes | `oltp_insert` | 53.67ms | 66.70ms | 1.243× | 6.28% | PASS |
| file_writes | `oltp_update_index` | 200.31ms | 214.42ms | 1.070× | 2.60% | PASS |
| file_writes | `oltp_update_non_index` | 154.87ms | 143.16ms | 0.924× | 2.95% | PASS |
| file_writes | `oltp_delete_insert` | 159.11ms | 169.77ms | 1.067× | 2.59% | PASS |
| file_writes | `oltp_write_only` | 116.58ms | 117.18ms | 1.005× | 1.76% | PASS |
| file_writes | `types_delete_insert` | 100.91ms | 98.47ms | 0.976× | 6.51% | PASS |
| file_writes | `oltp_read_write` | 164.76ms | 179.14ms | 1.087× | 3.15% | PASS |
| ac_reads | `oltp_point_select` | 53.26ms | 57.05ms | 1.071× | 2.04% | PASS |
| ac_reads | `oltp_range_select` | 15.80ms | 14.71ms | 0.931× | 1.52% | PASS |
| ac_reads | `oltp_sum_range` | 14.56ms | 14.37ms | 0.987× | 1.89% | PASS |
| ac_reads | `oltp_order_range` | 3.25ms | 3.15ms | 0.970× | 1.27% | PASS |
| ac_reads | `oltp_distinct_range` | 4.14ms | 4.02ms | 0.970× | 1.45% | PASS |
| ac_reads | `oltp_index_scan` | 7.58ms | 8.45ms | 1.115× | 2.63% | PASS |
| ac_reads | `select_random_points` | 19.46ms | 21.30ms | 1.094× | 1.69% | PASS |
| ac_reads | `select_random_ranges` | 6.97ms | 7.52ms | 1.078× | 1.57% | PASS |
| ac_reads | `covering_index_scan` | 7.95ms | 7.12ms | 0.896× | 1.63% | PASS |
| ac_reads | `groupby_scan` | 30.02ms | 30.89ms | 1.029× | 1.96% | PASS |
| ac_reads | `index_join` | 8.48ms | 10.15ms | 1.197× | 1.63% | PASS |
| ac_reads | `index_join_scan` | 4.60ms | 5.60ms | 1.218× | 2.07% | PASS |
| ac_reads | `types_table_scan` | 966.33ms | 1.05s | 1.092× | 0.97% | PASS |
| ac_reads | `table_scan` | 1.13s | 1.16s | 1.025× | 1.13% | PASS |
| ac_reads | `oltp_read_only` | 140.15ms | 147.83ms | 1.055× | 1.41% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.26ms | 59.30ms | 2.789× | 21.57% | PASS |
| ac_writes | `oltp_insert_ac` | 21.56ms | 88.60ms | 4.110× | 17.22% | PASS |
| ac_writes | `oltp_update_index_ac` | 23.11ms | 90.09ms | 3.899× | 14.51% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 19.08ms | 70.98ms | 3.720× | 12.93% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 20.56ms | 80.33ms | 3.907× | 12.46% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.20ms | 81.39ms | 3.666× | 17.32% | PASS |
| ac_writes | `types_delete_insert_ac` | 19.32ms | 70.97ms | 3.673× | 13.05% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.32ms | 89.98ms | 3.294× | 15.17% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.53ms | 40.65ms | 1.212× | 1.94% | PASS |
| mem_reads | `oltp_range_select` | 19.45ms | 21.32ms | 1.096× | 1.75% | PASS |
| mem_reads | `oltp_sum_range` | 18.38ms | 20.93ms | 1.139× | 1.55% | PASS |
| mem_reads | `oltp_order_range` | 3.54ms | 3.84ms | 1.086× | 1.63% | PASS |
| mem_reads | `oltp_distinct_range` | 4.70ms | 4.90ms | 1.043× | 1.12% | PASS |
| mem_reads | `oltp_index_scan` | 4.62ms | 6.35ms | 1.375× | 2.03% | PASS |
| mem_reads | `select_random_points` | 28.58ms | 31.98ms | 1.119× | 1.77% | PASS |
| mem_reads | `select_random_ranges` | 7.82ms | 9.07ms | 1.161× | 1.25% | PASS |
| mem_reads | `covering_index_scan` | 4.26ms | 4.37ms | 1.027× | 1.86% | PASS |
| mem_reads | `groupby_scan` | 36.47ms | 39.07ms | 1.071× | 0.84% | PASS |
| mem_reads | `index_join` | 8.34ms | 10.79ms | 1.294× | 1.83% | PASS |
| mem_reads | `index_join_scan` | 4.29ms | 5.46ms | 1.272× | 2.31% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.24s | 1.186× | 0.70% | PASS |
| mem_reads | `table_scan` | 1.19s | 1.39s | 1.166× | 0.84% | PASS |
| mem_reads | `oltp_read_only` | 151.60ms | 169.47ms | 1.118× | 1.40% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.05ms | 356.01ms | 1.429× | 0.68% | PASS |
| mem_writes | `oltp_insert` | 19.29ms | 36.37ms | 1.885× | 0.84% | PASS |
| mem_writes | `oltp_update_index` | 68.81ms | 115.86ms | 1.684× | 1.29% | PASS |
| mem_writes | `oltp_update_non_index` | 51.24ms | 82.80ms | 1.616× | 0.94% | PASS |
| mem_writes | `oltp_delete_insert` | 50.20ms | 94.87ms | 1.890× | 1.27% | PASS |
| mem_writes | `oltp_write_only` | 27.42ms | 57.22ms | 2.087× | 1.37% | PASS |
| mem_writes | `types_delete_insert` | 33.45ms | 54.31ms | 1.624× | 1.30% | PASS |
| mem_writes | `oltp_read_write` | 102.69ms | 155.28ms | 1.512× | 1.28% | PASS |
| file_reads | `oltp_point_select` | 109.46ms | 66.68ms | 0.609× | 0.90% | PASS |
| file_reads | `oltp_range_select` | 28.03ms | 24.27ms | 0.866× | 1.42% | PASS |
| file_reads | `oltp_sum_range` | 26.55ms | 23.96ms | 0.902× | 1.24% | PASS |
| file_reads | `oltp_order_range` | 4.52ms | 4.25ms | 0.940× | 1.63% | PASS |
| file_reads | `oltp_distinct_range` | 5.73ms | 5.34ms | 0.931× | 1.05% | PASS |
| file_reads | `oltp_index_scan` | 12.66ms | 9.15ms | 0.723× | 1.34% | PASS |
| file_reads | `select_random_points` | 38.69ms | 36.36ms | 0.940× | 1.31% | PASS |
| file_reads | `select_random_ranges` | 15.91ms | 12.02ms | 0.755× | 0.96% | PASS |
| file_reads | `covering_index_scan` | 12.26ms | 7.19ms | 0.586× | 1.17% | PASS |
| file_reads | `groupby_scan` | 37.70ms | 40.00ms | 1.061× | 0.93% | PASS |
| file_reads | `index_join` | 12.89ms | 12.79ms | 0.992× | 1.61% | PASS |
| file_reads | `index_join_scan` | 5.26ms | 5.98ms | 1.135× | 1.87% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.23s | 1.191× | 0.54% | PASS |
| file_reads | `table_scan` | 1.20s | 1.39s | 1.164× | 0.97% | PASS |
| file_reads | `oltp_read_only` | 266.56ms | 210.17ms | 0.788× | 0.94% | PASS |
| file_writes | `oltp_bulk_insert` | 264.22ms | 379.84ms | 1.438× | 0.94% | PASS |
| file_writes | `oltp_insert` | 26.77ms | 46.09ms | 1.722× | 1.14% | PASS |
| file_writes | `oltp_update_index` | 99.55ms | 142.99ms | 1.436× | 1.58% | PASS |
| file_writes | `oltp_update_non_index` | 79.13ms | 104.58ms | 1.322× | 1.35% | PASS |
| file_writes | `oltp_delete_insert` | 78.55ms | 119.73ms | 1.524× | 1.35% | PASS |
| file_writes | `oltp_write_only` | 52.52ms | 77.46ms | 1.475× | 1.66% | PASS |
| file_writes | `types_delete_insert` | 51.23ms | 69.11ms | 1.349× | 1.32% | PASS |
| file_writes | `oltp_read_write` | 131.68ms | 176.19ms | 1.338× | 1.56% | PASS |
| ac_reads | `oltp_point_select` | 58.70ms | 66.63ms | 1.135× | 1.16% | PASS |
| ac_reads | `oltp_range_select` | 22.22ms | 24.29ms | 1.093× | 1.52% | PASS |
| ac_reads | `oltp_sum_range` | 21.05ms | 24.04ms | 1.142× | 1.38% | PASS |
| ac_reads | `oltp_order_range` | 3.94ms | 4.25ms | 1.080× | 1.61% | PASS |
| ac_reads | `oltp_distinct_range` | 5.12ms | 5.37ms | 1.048× | 1.31% | PASS |
| ac_reads | `oltp_index_scan` | 7.34ms | 9.27ms | 1.263× | 1.55% | PASS |
| ac_reads | `select_random_points` | 31.94ms | 36.62ms | 1.147× | 1.39% | PASS |
| ac_reads | `select_random_ranges` | 10.47ms | 12.07ms | 1.152× | 1.01% | PASS |
| ac_reads | `covering_index_scan` | 6.92ms | 7.20ms | 1.041× | 1.25% | PASS |
| ac_reads | `groupby_scan` | 36.98ms | 40.07ms | 1.084× | 1.29% | PASS |
| ac_reads | `index_join` | 9.88ms | 12.85ms | 1.301× | 1.60% | PASS |
| ac_reads | `index_join_scan` | 4.75ms | 6.05ms | 1.273× | 2.33% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.23s | 1.189× | 0.57% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.39s | 1.159× | 0.97% | PASS |
| ac_reads | `oltp_read_only` | 189.55ms | 210.12ms | 1.109× | 1.26% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.57ms | 65.13ms | 3.020× | 3.71% | PASS |
| ac_writes | `oltp_insert_ac` | 24.10ms | 85.09ms | 3.531× | 3.38% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.91ms | 95.40ms | 3.682× | 4.10% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.21ms | 76.15ms | 3.428× | 4.45% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.81ms | 88.28ms | 3.707× | 4.82% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.77ms | 86.01ms | 3.618× | 4.21% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.52ms | 76.92ms | 3.575× | 5.81% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.69ms | 95.45ms | 3.110× | 3.71% | PASS |

</details>

## Version-control latency

Wall time: 2m 28s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 92.93ms | 200.00ms | 46.5% | 0.31% | PASS |
| `status_dirty_many_tables` | 96.39ms | 200.00ms | 48.2% | 0.30% | PASS |
| `diff_regular_working_one_table` | 88.74ms | 150.00ms | 59.2% | 0.32% | PASS |
| `diff_regular_working_many_tables` | 101.94ms | 200.00ms | 51.0% | 0.25% | PASS |
| `diff_stat_working_many_tables` | 101.75ms | 200.00ms | 50.9% | 0.26% | PASS |
| `diff_schema_working_many_tables` | 102.36ms | 200.00ms | 51.2% | 0.24% | PASS |
| `branch_list_many_branches` | 23.99ms | 100.00ms | 24.0% | 0.82% | PASS |
| `branch_create_delete` | 25.95ms | 100.00ms | 25.9% | 0.76% | PASS |
| `checkout_branch_clean` | 59.24ms | 200.00ms | 29.6% | 0.44% | PASS |
| `merge_data_no_conflicts` | 30.63ms | 150.00ms | 20.4% | 0.79% | PASS |
| `merge_schema_no_conflicts` | 22.80ms | 100.00ms | 22.8% | 0.62% | PASS |
| `merge_data_conflicts` | 129.39ms | 250.00ms | 51.8% | 0.18% | PASS |
| `merge_data_conflicts_with_resolve` | 129.15ms | 250.00ms | 51.7% | 0.12% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
