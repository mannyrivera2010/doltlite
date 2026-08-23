# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-23 11:28 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260816.277.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/32632248107)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 20s | 9.00s | 11.62s | 1.292× | 1.71% | **PASS** |
| textpk | 69 | 55 | 1h 31m 11s | 9.42s | 11.75s | 1.247× | 1.74% | **PASS** |
| blobpk | 69 | 55 | 1h 17m 36s | 8.07s | 9.87s | 1.222× | 1.51% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 46s | 9.76s | 12.08s | 1.238× | 1.50% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.03ms | 29.50ms | 1.228× | 1.30% | PASS |
| mem_reads | `oltp_range_select` | 10.24ms | 13.18ms | 1.287× | 2.02% | PASS |
| mem_reads | `oltp_sum_range` | 9.53ms | 12.38ms | 1.299× | 1.83% | PASS |
| mem_reads | `oltp_order_range` | 2.57ms | 3.00ms | 1.169× | 1.63% | PASS |
| mem_reads | `oltp_distinct_range` | 3.67ms | 4.07ms | 1.108× | 1.23% | PASS |
| mem_reads | `oltp_index_scan` | 4.03ms | 5.59ms | 1.385× | 1.75% | PASS |
| mem_reads | `select_random_points` | 10.42ms | 11.35ms | 1.090× | 2.17% | PASS |
| mem_reads | `select_random_ranges` | 3.04ms | 4.02ms | 1.324× | 1.31% | PASS |
| mem_reads | `covering_index_scan` | 4.26ms | 4.39ms | 1.030× | 2.49% | PASS |
| mem_reads | `groupby_scan` | 29.94ms | 32.83ms | 1.096× | 0.76% | PASS |
| mem_reads | `index_join` | 6.13ms | 8.78ms | 1.432× | 2.57% | PASS |
| mem_reads | `index_join_scan` | 3.51ms | 4.71ms | 1.340× | 1.83% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.35s | 1.273× | 1.36% | PASS |
| mem_reads | `table_scan` | 1.19s | 1.41s | 1.178× | 1.53% | PASS |
| mem_reads | `oltp_read_only` | 103.86ms | 124.67ms | 1.200× | 1.68% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.68ms | 252.13ms | 1.395× | 1.19% | PASS |
| mem_writes | `oltp_insert` | 15.47ms | 28.54ms | 1.845× | 0.96% | PASS |
| mem_writes | `oltp_update_index` | 51.16ms | 107.78ms | 2.107× | 1.35% | PASS |
| mem_writes | `oltp_update_non_index` | 34.57ms | 60.48ms | 1.749× | 1.76% | PASS |
| mem_writes | `oltp_delete_insert` | 45.44ms | 80.37ms | 1.769× | 1.35% | PASS |
| mem_writes | `oltp_write_only` | 22.01ms | 45.86ms | 2.084× | 1.35% | PASS |
| mem_writes | `types_delete_insert` | 24.60ms | 40.80ms | 1.659× | 1.16% | PASS |
| mem_writes | `oltp_read_write` | 68.02ms | 111.68ms | 1.642× | 2.36% | PASS |
| file_reads | `oltp_point_select` | 99.13ms | 55.33ms | 0.558× | 1.01% | PASS |
| file_reads | `oltp_range_select` | 18.32ms | 16.02ms | 0.875× | 2.25% | PASS |
| file_reads | `oltp_sum_range` | 17.44ms | 15.26ms | 0.875× | 1.35% | PASS |
| file_reads | `oltp_order_range` | 3.48ms | 3.41ms | 0.982× | 2.11% | PASS |
| file_reads | `oltp_distinct_range` | 4.57ms | 4.45ms | 0.974× | 1.29% | PASS |
| file_reads | `oltp_index_scan` | 11.79ms | 8.35ms | 0.709× | 1.63% | PASS |
| file_reads | `select_random_points` | 18.32ms | 14.10ms | 0.770× | 1.97% | PASS |
| file_reads | `select_random_ranges` | 10.54ms | 6.61ms | 0.627× | 0.94% | PASS |
| file_reads | `covering_index_scan` | 12.09ms | 7.12ms | 0.589× | 1.21% | PASS |
| file_reads | `groupby_scan` | 31.00ms | 33.35ms | 1.076× | 0.79% | PASS |
| file_reads | `index_join` | 10.45ms | 10.38ms | 0.993× | 2.06% | PASS |
| file_reads | `index_join_scan` | 4.47ms | 5.10ms | 1.142× | 1.78% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.34s | 1.274× | 1.43% | PASS |
| file_reads | `table_scan` | 1.18s | 1.41s | 1.188× | 0.92% | PASS |
| file_reads | `oltp_read_only` | 213.81ms | 162.67ms | 0.761× | 0.99% | PASS |
| file_writes | `oltp_bulk_insert` | 194.91ms | 271.95ms | 1.395× | 1.52% | PASS |
| file_writes | `oltp_insert` | 22.30ms | 36.00ms | 1.614× | 2.03% | PASS |
| file_writes | `oltp_update_index` | 78.73ms | 130.37ms | 1.656× | 2.25% | PASS |
| file_writes | `oltp_update_non_index` | 58.25ms | 81.36ms | 1.397× | 2.35% | PASS |
| file_writes | `oltp_delete_insert` | 69.70ms | 100.11ms | 1.436× | 1.80% | PASS |
| file_writes | `oltp_write_only` | 44.87ms | 64.68ms | 1.441× | 2.33% | PASS |
| file_writes | `types_delete_insert` | 39.99ms | 53.74ms | 1.344× | 1.52% | PASS |
| file_writes | `oltp_read_write` | 95.00ms | 132.30ms | 1.393× | 2.05% | PASS |
| ac_reads | `oltp_point_select` | 49.21ms | 55.76ms | 1.133× | 1.00% | PASS |
| ac_reads | `oltp_range_select` | 13.21ms | 16.02ms | 1.213× | 1.78% | PASS |
| ac_reads | `oltp_sum_range` | 12.40ms | 15.22ms | 1.227× | 1.85% | PASS |
| ac_reads | `oltp_order_range` | 3.03ms | 3.40ms | 1.122× | 2.24% | PASS |
| ac_reads | `oltp_distinct_range` | 4.07ms | 4.46ms | 1.098× | 1.71% | PASS |
| ac_reads | `oltp_index_scan` | 6.76ms | 8.32ms | 1.230× | 2.02% | PASS |
| ac_reads | `select_random_points` | 12.97ms | 14.03ms | 1.081× | 1.32% | PASS |
| ac_reads | `select_random_ranges` | 5.61ms | 6.63ms | 1.182× | 0.98% | PASS |
| ac_reads | `covering_index_scan` | 7.11ms | 7.12ms | 1.002× | 2.19% | PASS |
| ac_reads | `groupby_scan` | 30.30ms | 33.42ms | 1.103× | 1.00% | PASS |
| ac_reads | `index_join` | 7.68ms | 10.31ms | 1.341× | 1.34% | PASS |
| ac_reads | `index_join_scan` | 3.97ms | 5.09ms | 1.284× | 2.57% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.34s | 1.267× | 1.14% | PASS |
| ac_reads | `table_scan` | 1.23s | 1.42s | 1.156× | 1.44% | PASS |
| ac_reads | `oltp_read_only` | 141.90ms | 163.51ms | 1.152× | 1.11% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.84ms | 85.24ms | 3.732× | 5.16% | PASS |
| ac_writes | `oltp_insert_ac` | 25.14ms | 100.34ms | 3.992× | 5.96% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.05ms | 115.38ms | 4.114× | 5.47% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.17ms | 95.28ms | 3.943× | 6.52% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.68ms | 107.88ms | 4.201× | 5.69% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.16ms | 105.16ms | 3.871× | 8.91% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.31ms | 94.75ms | 4.248× | 5.53% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.09ms | 111.59ms | 3.709× | 3.71% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 28.79ms | 37.97ms | 1.319× | 1.16% | PASS |
| mem_reads | `oltp_range_select` | 12.12ms | 14.16ms | 1.168× | 1.91% | PASS |
| mem_reads | `oltp_sum_range` | 11.35ms | 14.07ms | 1.240× | 1.65% | PASS |
| mem_reads | `oltp_order_range` | 2.80ms | 3.13ms | 1.119× | 1.57% | PASS |
| mem_reads | `oltp_distinct_range` | 3.86ms | 4.20ms | 1.087× | 1.37% | PASS |
| mem_reads | `oltp_index_scan` | 4.31ms | 6.14ms | 1.425× | 1.13% | PASS |
| mem_reads | `select_random_points` | 16.87ms | 20.85ms | 1.235× | 1.58% | PASS |
| mem_reads | `select_random_ranges` | 3.72ms | 5.16ms | 1.388× | 2.21% | PASS |
| mem_reads | `covering_index_scan` | 4.49ms | 4.34ms | 0.967× | 1.11% | PASS |
| mem_reads | `groupby_scan` | 31.18ms | 33.68ms | 1.080× | 0.59% | PASS |
| mem_reads | `index_join` | 6.80ms | 8.83ms | 1.299× | 1.40% | PASS |
| mem_reads | `index_join_scan` | 4.28ms | 5.39ms | 1.259× | 2.91% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.23s | 1.182× | 0.73% | PASS |
| mem_reads | `table_scan` | 1.19s | 1.36s | 1.143× | 0.53% | PASS |
| mem_reads | `oltp_read_only` | 115.08ms | 136.14ms | 1.183× | 0.92% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.17ms | 355.41ms | 1.511× | 0.91% | PASS |
| mem_writes | `oltp_insert` | 21.40ms | 39.83ms | 1.861× | 0.58% | PASS |
| mem_writes | `oltp_update_index` | 69.09ms | 132.01ms | 1.911× | 0.79% | PASS |
| mem_writes | `oltp_update_non_index` | 47.25ms | 85.63ms | 1.812× | 1.02% | PASS |
| mem_writes | `oltp_delete_insert` | 49.68ms | 103.22ms | 2.078× | 0.84% | PASS |
| mem_writes | `oltp_write_only` | 28.24ms | 61.59ms | 2.181× | 0.72% | PASS |
| mem_writes | `types_delete_insert` | 31.99ms | 54.38ms | 1.700× | 0.94% | PASS |
| mem_writes | `oltp_read_write` | 82.39ms | 138.85ms | 1.685× | 1.07% | PASS |
| file_reads | `oltp_point_select` | 104.80ms | 63.58ms | 0.607× | 0.81% | PASS |
| file_reads | `oltp_range_select` | 21.03ms | 17.34ms | 0.825× | 3.01% | PASS |
| file_reads | `oltp_sum_range` | 19.56ms | 17.15ms | 0.877× | 2.09% | PASS |
| file_reads | `oltp_order_range` | 3.74ms | 3.54ms | 0.945× | 3.26% | PASS |
| file_reads | `oltp_distinct_range` | 5.00ms | 4.64ms | 0.927× | 2.14% | PASS |
| file_reads | `oltp_index_scan` | 12.10ms | 9.17ms | 0.758× | 1.73% | PASS |
| file_reads | `select_random_points` | 26.20ms | 24.65ms | 0.941× | 2.29% | PASS |
| file_reads | `select_random_ranges` | 11.68ms | 7.93ms | 0.679× | 2.52% | PASS |
| file_reads | `covering_index_scan` | 12.39ms | 7.49ms | 0.604× | 2.18% | PASS |
| file_reads | `groupby_scan` | 32.72ms | 34.40ms | 1.051× | 1.32% | PASS |
| file_reads | `index_join` | 11.15ms | 11.51ms | 1.032× | 2.10% | PASS |
| file_reads | `index_join_scan` | 5.60ms | 5.99ms | 1.070× | 1.99% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.22s | 1.172× | 0.76% | PASS |
| file_reads | `table_scan` | 1.20s | 1.36s | 1.137× | 0.59% | PASS |
| file_reads | `oltp_read_only` | 225.76ms | 174.83ms | 0.774× | 0.66% | PASS |
| file_writes | `oltp_bulk_insert` | 253.85ms | 384.28ms | 1.514× | 1.05% | PASS |
| file_writes | `oltp_insert` | 43.13ms | 52.99ms | 1.229× | 15.25% | PASS |
| file_writes | `oltp_update_index` | 110.61ms | 169.86ms | 1.536× | 1.01% | PASS |
| file_writes | `oltp_update_non_index` | 97.30ms | 112.03ms | 1.151× | 11.95% | PASS |
| file_writes | `oltp_delete_insert` | 88.48ms | 134.23ms | 1.517× | 1.74% | PASS |
| file_writes | `oltp_write_only` | 86.13ms | 86.05ms | 0.999× | 10.89% | PASS |
| file_writes | `types_delete_insert` | 54.45ms | 74.36ms | 1.366× | 1.86% | PASS |
| file_writes | `oltp_read_write` | 139.72ms | 163.07ms | 1.167× | 5.90% | PASS |
| ac_reads | `oltp_point_select` | 55.13ms | 63.85ms | 1.158× | 0.73% | PASS |
| ac_reads | `oltp_range_select` | 16.62ms | 17.16ms | 1.033× | 2.55% | PASS |
| ac_reads | `oltp_sum_range` | 14.97ms | 17.06ms | 1.139× | 1.75% | PASS |
| ac_reads | `oltp_order_range` | 3.42ms | 3.62ms | 1.057× | 2.44% | PASS |
| ac_reads | `oltp_distinct_range` | 4.47ms | 4.64ms | 1.037× | 1.63% | PASS |
| ac_reads | `oltp_index_scan` | 7.47ms | 9.23ms | 1.235× | 2.43% | PASS |
| ac_reads | `select_random_points` | 21.20ms | 24.72ms | 1.166× | 1.78% | PASS |
| ac_reads | `select_random_ranges` | 6.84ms | 7.99ms | 1.168× | 2.29% | PASS |
| ac_reads | `covering_index_scan` | 8.13ms | 7.46ms | 0.918× | 3.30% | PASS |
| ac_reads | `groupby_scan` | 32.45ms | 34.51ms | 1.063× | 1.00% | PASS |
| ac_reads | `index_join` | 9.20ms | 11.50ms | 1.249× | 3.65% | PASS |
| ac_reads | `index_join_scan` | 5.13ms | 6.02ms | 1.172× | 2.83% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.22s | 1.173× | 0.69% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.36s | 1.135× | 0.44% | PASS |
| ac_reads | `oltp_read_only` | 153.22ms | 174.99ms | 1.142× | 0.85% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.98ms | 77.54ms | 3.527× | 4.57% | PASS |
| ac_writes | `oltp_insert_ac` | 24.72ms | 94.10ms | 3.807× | 6.07% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.58ms | 112.58ms | 4.082× | 6.28% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.78ms | 91.27ms | 4.190× | 4.60% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.07ms | 103.77ms | 4.311× | 5.08% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.44ms | 103.98ms | 4.254× | 5.24% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.83ms | 97.35ms | 4.674× | 7.27% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.00ms | 109.00ms | 3.634× | 3.43% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.73ms | 25.17ms | 1.108× | 1.12% | PASS |
| mem_reads | `oltp_range_select` | 10.62ms | 10.74ms | 1.012× | 1.06% | PASS |
| mem_reads | `oltp_sum_range` | 9.82ms | 9.99ms | 1.017× | 2.74% | PASS |
| mem_reads | `oltp_order_range` | 2.41ms | 2.54ms | 1.052× | 0.81% | PASS |
| mem_reads | `oltp_distinct_range` | 3.25ms | 3.35ms | 1.032× | 0.84% | PASS |
| mem_reads | `oltp_index_scan` | 3.52ms | 4.33ms | 1.232× | 2.18% | PASS |
| mem_reads | `select_random_points` | 14.20ms | 16.09ms | 1.134× | 0.99% | PASS |
| mem_reads | `select_random_ranges` | 2.99ms | 3.86ms | 1.291× | 1.90% | PASS |
| mem_reads | `covering_index_scan` | 3.11ms | 3.20ms | 1.030× | 1.65% | PASS |
| mem_reads | `groupby_scan` | 27.18ms | 26.84ms | 0.988× | 0.52% | PASS |
| mem_reads | `index_join` | 5.49ms | 7.02ms | 1.280× | 0.83% | PASS |
| mem_reads | `index_join_scan` | 3.21ms | 4.53ms | 1.411× | 2.53% | PASS |
| mem_reads | `types_table_scan` | 871.89ms | 1.01s | 1.161× | 0.18% | PASS |
| mem_reads | `table_scan` | 1.02s | 1.11s | 1.089× | 0.36% | PASS |
| mem_reads | `oltp_read_only` | 90.30ms | 99.69ms | 1.104× | 0.45% | PASS |
| mem_writes | `oltp_bulk_insert` | 162.20ms | 226.85ms | 1.399× | 0.46% | PASS |
| mem_writes | `oltp_insert` | 14.59ms | 26.81ms | 1.838× | 0.48% | PASS |
| mem_writes | `oltp_update_index` | 50.52ms | 92.54ms | 1.832× | 1.22% | PASS |
| mem_writes | `oltp_update_non_index` | 34.48ms | 57.75ms | 1.675× | 1.39% | PASS |
| mem_writes | `oltp_delete_insert` | 35.86ms | 71.68ms | 1.999× | 0.97% | PASS |
| mem_writes | `oltp_write_only` | 19.98ms | 42.45ms | 2.125× | 0.97% | PASS |
| mem_writes | `types_delete_insert` | 22.83ms | 36.94ms | 1.618× | 1.37% | PASS |
| mem_writes | `oltp_read_write` | 59.07ms | 95.42ms | 1.615× | 0.97% | PASS |
| file_reads | `oltp_point_select` | 48.38ms | 34.31ms | 0.709× | 0.63% | PASS |
| file_reads | `oltp_range_select` | 13.47ms | 12.04ms | 0.894× | 1.68% | PASS |
| file_reads | `oltp_sum_range` | 13.10ms | 11.46ms | 0.875× | 1.34% | PASS |
| file_reads | `oltp_order_range` | 2.79ms | 2.72ms | 0.972× | 1.49% | PASS |
| file_reads | `oltp_distinct_range` | 3.59ms | 3.50ms | 0.975× | 0.61% | PASS |
| file_reads | `oltp_index_scan` | 6.28ms | 5.43ms | 0.865× | 1.85% | PASS |
| file_reads | `select_random_points` | 17.38ms | 17.73ms | 1.020× | 1.51% | PASS |
| file_reads | `select_random_ranges` | 5.81ms | 4.88ms | 0.841× | 1.77% | PASS |
| file_reads | `covering_index_scan` | 5.91ms | 4.35ms | 0.736× | 3.00% | PASS |
| file_reads | `groupby_scan` | 27.55ms | 27.16ms | 0.986× | 0.76% | PASS |
| file_reads | `index_join` | 7.10ms | 8.43ms | 1.188× | 2.03% | PASS |
| file_reads | `index_join_scan` | 3.73ms | 4.94ms | 1.327× | 1.85% | PASS |
| file_reads | `types_table_scan` | 873.74ms | 1.01s | 1.160× | 0.22% | PASS |
| file_reads | `table_scan` | 1.01s | 1.11s | 1.091× | 0.26% | PASS |
| file_reads | `oltp_read_only` | 128.57ms | 112.97ms | 0.879× | 0.58% | PASS |
| file_writes | `oltp_bulk_insert` | 208.24ms | 297.78ms | 1.430× | 7.66% | PASS |
| file_writes | `oltp_insert` | 39.52ms | 55.92ms | 1.415× | 3.93% | PASS |
| file_writes | `oltp_update_index` | 167.79ms | 183.33ms | 1.093× | 11.87% | PASS |
| file_writes | `oltp_update_non_index` | 130.25ms | 123.48ms | 0.948× | 4.69% | PASS |
| file_writes | `oltp_delete_insert` | 133.48ms | 140.18ms | 1.050× | 7.34% | PASS |
| file_writes | `oltp_write_only` | 90.23ms | 97.08ms | 1.076× | 6.90% | PASS |
| file_writes | `types_delete_insert` | 79.69ms | 81.01ms | 1.017× | 11.92% | PASS |
| file_writes | `oltp_read_write` | 134.62ms | 149.40ms | 1.110× | 2.52% | PASS |
| ac_reads | `oltp_point_select` | 30.91ms | 33.92ms | 1.097× | 1.06% | PASS |
| ac_reads | `oltp_range_select` | 11.58ms | 11.94ms | 1.031× | 1.86% | PASS |
| ac_reads | `oltp_sum_range` | 11.25ms | 11.30ms | 1.005× | 2.07% | PASS |
| ac_reads | `oltp_order_range` | 2.63ms | 2.73ms | 1.036× | 1.27% | PASS |
| ac_reads | `oltp_distinct_range` | 3.47ms | 3.51ms | 1.014× | 1.10% | PASS |
| ac_reads | `oltp_index_scan` | 4.60ms | 5.48ms | 1.191× | 2.85% | PASS |
| ac_reads | `select_random_points` | 15.44ms | 17.24ms | 1.117× | 1.99% | PASS |
| ac_reads | `select_random_ranges` | 4.06ms | 4.82ms | 1.187× | 1.78% | PASS |
| ac_reads | `covering_index_scan` | 4.17ms | 4.25ms | 1.017× | 3.87% | PASS |
| ac_reads | `groupby_scan` | 27.00ms | 26.94ms | 0.998× | 0.66% | PASS |
| ac_reads | `index_join` | 6.35ms | 7.97ms | 1.256× | 2.56% | PASS |
| ac_reads | `index_join_scan` | 3.58ms | 4.84ms | 1.353× | 1.36% | PASS |
| ac_reads | `types_table_scan` | 872.93ms | 1.01s | 1.161× | 0.28% | PASS |
| ac_reads | `table_scan` | 1.02s | 1.11s | 1.092× | 0.32% | PASS |
| ac_reads | `oltp_read_only` | 104.21ms | 113.89ms | 1.093× | 0.64% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 36.25ms | 99.20ms | 2.737× | 17.81% | PASS |
| ac_writes | `oltp_insert_ac` | 41.39ms | 121.91ms | 2.946× | 16.30% | PASS |
| ac_writes | `oltp_update_index_ac` | 40.87ms | 131.00ms | 3.205× | 16.19% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 37.13ms | 116.02ms | 3.125× | 17.59% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 37.67ms | 137.77ms | 3.657× | 19.02% | PASS |
| ac_writes | `oltp_write_only_ac` | 45.73ms | 157.89ms | 3.453× | 49.39% | PASS |
| ac_writes | `types_delete_insert_ac` | 35.33ms | 111.53ms | 3.156× | 20.67% | PASS |
| ac_writes | `oltp_read_write_ac` | 40.16ms | 133.93ms | 3.335× | 19.36% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.61ms | 41.38ms | 1.196× | 1.59% | PASS |
| mem_reads | `oltp_range_select` | 20.02ms | 21.94ms | 1.096× | 2.44% | PASS |
| mem_reads | `oltp_sum_range` | 18.51ms | 21.17ms | 1.144× | 1.57% | PASS |
| mem_reads | `oltp_order_range` | 3.66ms | 3.96ms | 1.080× | 1.28% | PASS |
| mem_reads | `oltp_distinct_range` | 4.75ms | 4.99ms | 1.051× | 1.01% | PASS |
| mem_reads | `oltp_index_scan` | 4.75ms | 6.49ms | 1.365× | 2.13% | PASS |
| mem_reads | `select_random_points` | 30.26ms | 33.27ms | 1.100× | 2.68% | PASS |
| mem_reads | `select_random_ranges` | 7.92ms | 9.25ms | 1.167× | 1.37% | PASS |
| mem_reads | `covering_index_scan` | 4.26ms | 4.21ms | 0.987× | 2.27% | PASS |
| mem_reads | `groupby_scan` | 36.73ms | 39.03ms | 1.063× | 1.39% | PASS |
| mem_reads | `index_join` | 8.40ms | 10.99ms | 1.307× | 1.76% | PASS |
| mem_reads | `index_join_scan` | 4.32ms | 5.71ms | 1.322× | 2.62% | PASS |
| mem_reads | `types_table_scan` | 1.08s | 1.25s | 1.157× | 0.76% | PASS |
| mem_reads | `table_scan` | 1.32s | 1.41s | 1.074× | 1.58% | PASS |
| mem_reads | `oltp_read_only` | 161.72ms | 175.83ms | 1.087× | 1.33% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.51ms | 364.85ms | 1.462× | 0.82% | PASS |
| mem_writes | `oltp_insert` | 19.98ms | 37.60ms | 1.882× | 1.16% | PASS |
| mem_writes | `oltp_update_index` | 75.32ms | 127.32ms | 1.690× | 1.40% | PASS |
| mem_writes | `oltp_update_non_index` | 55.43ms | 88.63ms | 1.599× | 1.86% | PASS |
| mem_writes | `oltp_delete_insert` | 52.41ms | 99.32ms | 1.895× | 1.22% | PASS |
| mem_writes | `oltp_write_only` | 28.23ms | 59.87ms | 2.121× | 3.26% | PASS |
| mem_writes | `types_delete_insert` | 36.49ms | 59.52ms | 1.631× | 2.66% | PASS |
| mem_writes | `oltp_read_write` | 109.87ms | 160.69ms | 1.463× | 1.28% | PASS |
| file_reads | `oltp_point_select` | 111.92ms | 67.86ms | 0.606× | 1.30% | PASS |
| file_reads | `oltp_range_select` | 28.52ms | 24.89ms | 0.873× | 1.65% | PASS |
| file_reads | `oltp_sum_range` | 26.83ms | 24.36ms | 0.908× | 1.10% | PASS |
| file_reads | `oltp_order_range` | 4.62ms | 4.34ms | 0.939× | 1.36% | PASS |
| file_reads | `oltp_distinct_range` | 5.78ms | 5.43ms | 0.940× | 1.49% | PASS |
| file_reads | `oltp_index_scan` | 12.48ms | 9.12ms | 0.731× | 2.29% | PASS |
| file_reads | `select_random_points` | 38.01ms | 36.16ms | 0.951× | 2.20% | PASS |
| file_reads | `select_random_ranges` | 15.49ms | 12.08ms | 0.780× | 1.32% | PASS |
| file_reads | `covering_index_scan` | 11.81ms | 7.07ms | 0.599× | 1.64% | PASS |
| file_reads | `groupby_scan` | 36.90ms | 39.47ms | 1.070× | 0.89% | PASS |
| file_reads | `index_join` | 12.51ms | 12.68ms | 1.014× | 1.33% | PASS |
| file_reads | `index_join_scan` | 5.21ms | 5.84ms | 1.121× | 2.14% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.23s | 1.190× | 0.53% | PASS |
| file_reads | `table_scan` | 1.18s | 1.39s | 1.178× | 0.60% | PASS |
| file_reads | `oltp_read_only` | 261.37ms | 210.36ms | 0.805× | 0.89% | PASS |
| file_writes | `oltp_bulk_insert` | 263.25ms | 381.06ms | 1.448× | 1.06% | PASS |
| file_writes | `oltp_insert` | 26.14ms | 46.44ms | 1.777× | 1.45% | PASS |
| file_writes | `oltp_update_index` | 100.79ms | 146.26ms | 1.451× | 1.50% | PASS |
| file_writes | `oltp_update_non_index` | 78.54ms | 107.57ms | 1.370× | 1.98% | PASS |
| file_writes | `oltp_delete_insert` | 80.07ms | 123.29ms | 1.540× | 1.55% | PASS |
| file_writes | `oltp_write_only` | 51.57ms | 77.37ms | 1.500× | 1.48% | PASS |
| file_writes | `types_delete_insert` | 50.45ms | 68.79ms | 1.363× | 1.77% | PASS |
| file_writes | `oltp_read_write` | 125.48ms | 174.86ms | 1.393× | 0.86% | PASS |
| ac_reads | `oltp_point_select` | 57.24ms | 66.41ms | 1.160× | 0.89% | PASS |
| ac_reads | `oltp_range_select` | 22.25ms | 24.69ms | 1.110× | 2.62% | PASS |
| ac_reads | `oltp_sum_range` | 20.92ms | 24.16ms | 1.155× | 1.17% | PASS |
| ac_reads | `oltp_order_range` | 4.06ms | 4.29ms | 1.059× | 0.85% | PASS |
| ac_reads | `oltp_distinct_range` | 5.18ms | 5.39ms | 1.040× | 1.32% | PASS |
| ac_reads | `oltp_index_scan` | 7.49ms | 9.20ms | 1.229× | 1.61% | PASS |
| ac_reads | `select_random_points` | 31.80ms | 36.30ms | 1.142× | 1.24% | PASS |
| ac_reads | `select_random_ranges` | 10.54ms | 12.11ms | 1.150× | 1.29% | PASS |
| ac_reads | `covering_index_scan` | 7.10ms | 7.23ms | 1.018× | 2.09% | PASS |
| ac_reads | `groupby_scan` | 36.98ms | 39.78ms | 1.076× | 0.82% | PASS |
| ac_reads | `index_join` | 10.19ms | 12.91ms | 1.267× | 1.95% | PASS |
| ac_reads | `index_join_scan` | 4.68ms | 5.87ms | 1.255× | 2.04% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.23s | 1.184× | 0.97% | PASS |
| ac_reads | `table_scan` | 1.20s | 1.39s | 1.156× | 1.95% | PASS |
| ac_reads | `oltp_read_only` | 189.47ms | 211.31ms | 1.115× | 1.20% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.33ms | 78.11ms | 3.661× | 6.47% | PASS |
| ac_writes | `oltp_insert_ac` | 23.13ms | 97.18ms | 4.201× | 5.91% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.33ms | 108.21ms | 4.271× | 4.26% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.75ms | 86.55ms | 3.978× | 4.38% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.25ms | 97.72ms | 4.204× | 5.65% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.15ms | 97.94ms | 4.230× | 3.76% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.65ms | 88.65ms | 4.292× | 5.42% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.41ms | 105.09ms | 3.573× | 3.35% | PASS |

</details>

## Version-control latency

Wall time: 2m 29s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 93.46ms | 200.00ms | 46.7% | 0.43% | PASS |
| `status_dirty_many_tables` | 96.97ms | 200.00ms | 48.5% | 0.30% | PASS |
| `diff_regular_working_one_table` | 89.38ms | 150.00ms | 59.6% | 0.30% | PASS |
| `diff_regular_working_many_tables` | 102.42ms | 200.00ms | 51.2% | 0.35% | PASS |
| `diff_stat_working_many_tables` | 102.54ms | 200.00ms | 51.3% | 0.46% | PASS |
| `diff_schema_working_many_tables` | 103.03ms | 200.00ms | 51.5% | 0.32% | PASS |
| `branch_list_many_branches` | 24.55ms | 100.00ms | 24.5% | 1.15% | PASS |
| `branch_create_delete` | 26.39ms | 100.00ms | 26.4% | 1.11% | PASS |
| `checkout_branch_clean` | 59.52ms | 200.00ms | 29.8% | 0.71% | PASS |
| `merge_data_no_conflicts` | 31.02ms | 150.00ms | 20.7% | 1.26% | PASS |
| `merge_schema_no_conflicts` | 23.22ms | 100.00ms | 23.2% | 1.08% | PASS |
| `merge_data_conflicts` | 129.69ms | 250.00ms | 51.9% | 0.18% | PASS |
| `merge_data_conflicts_with_resolve` | 129.40ms | 250.00ms | 51.8% | 0.21% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
