# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-15 11:25 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260810.271.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/31878044873)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 41s | 8.88s | 11.51s | 1.297× | 1.44% | **PASS** |
| textpk | 69 | 55 | 1h 29m 34s | 10.19s | 10.19s | 1.000× | 1.22% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 35s | 9.37s | 11.74s | 1.253× | 1.44% | **PASS** |
| compositepk | 69 | 55 | 1h 22m 16s | 8.77s | 10.04s | 1.145× | 1.46% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.94ms | 29.36ms | 1.227× | 1.81% | PASS |
| mem_reads | `oltp_range_select` | 10.20ms | 13.11ms | 1.285× | 2.38% | PASS |
| mem_reads | `oltp_sum_range` | 9.54ms | 12.34ms | 1.293× | 1.78% | PASS |
| mem_reads | `oltp_order_range` | 2.58ms | 2.97ms | 1.153× | 1.14% | PASS |
| mem_reads | `oltp_distinct_range` | 3.63ms | 4.01ms | 1.102× | 1.34% | PASS |
| mem_reads | `oltp_index_scan` | 3.88ms | 5.24ms | 1.350× | 1.78% | PASS |
| mem_reads | `select_random_points` | 9.85ms | 10.89ms | 1.106× | 2.35% | PASS |
| mem_reads | `select_random_ranges` | 2.94ms | 3.93ms | 1.337× | 2.65% | PASS |
| mem_reads | `covering_index_scan` | 4.23ms | 4.13ms | 0.976× | 1.05% | PASS |
| mem_reads | `groupby_scan` | 29.74ms | 32.62ms | 1.097× | 0.85% | PASS |
| mem_reads | `index_join` | 6.03ms | 7.99ms | 1.324× | 1.31% | PASS |
| mem_reads | `index_join_scan` | 3.54ms | 4.54ms | 1.283× | 1.80% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.35s | 1.271× | 0.84% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.40s | 1.192× | 1.32% | PASS |
| mem_reads | `oltp_read_only` | 102.83ms | 123.20ms | 1.198× | 1.31% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.46ms | 251.65ms | 1.394× | 1.58% | PASS |
| mem_writes | `oltp_insert` | 15.47ms | 28.44ms | 1.839× | 1.08% | PASS |
| mem_writes | `oltp_update_index` | 50.33ms | 103.39ms | 2.054× | 1.02% | PASS |
| mem_writes | `oltp_update_non_index` | 34.66ms | 58.83ms | 1.697× | 1.72% | PASS |
| mem_writes | `oltp_delete_insert` | 44.99ms | 78.37ms | 1.742× | 1.52% | PASS |
| mem_writes | `oltp_write_only` | 21.76ms | 44.49ms | 2.044× | 1.65% | PASS |
| mem_writes | `types_delete_insert` | 24.92ms | 40.22ms | 1.614× | 1.14% | PASS |
| mem_writes | `oltp_read_write` | 66.81ms | 109.97ms | 1.646× | 1.37% | PASS |
| file_reads | `oltp_point_select` | 99.08ms | 55.23ms | 0.557× | 0.77% | PASS |
| file_reads | `oltp_range_select` | 17.98ms | 15.81ms | 0.879× | 2.33% | PASS |
| file_reads | `oltp_sum_range` | 17.42ms | 15.12ms | 0.868× | 1.13% | PASS |
| file_reads | `oltp_order_range` | 3.44ms | 3.31ms | 0.965× | 1.60% | PASS |
| file_reads | `oltp_distinct_range` | 4.54ms | 4.38ms | 0.965× | 1.19% | PASS |
| file_reads | `oltp_index_scan` | 11.66ms | 8.23ms | 0.706× | 1.57% | PASS |
| file_reads | `select_random_points` | 18.02ms | 13.80ms | 0.766× | 1.66% | PASS |
| file_reads | `select_random_ranges` | 10.53ms | 6.56ms | 0.623× | 0.80% | PASS |
| file_reads | `covering_index_scan` | 11.89ms | 7.08ms | 0.596× | 1.07% | PASS |
| file_reads | `groupby_scan` | 30.96ms | 33.31ms | 1.076× | 0.93% | PASS |
| file_reads | `index_join` | 10.45ms | 10.14ms | 0.970× | 1.13% | PASS |
| file_reads | `index_join_scan` | 4.62ms | 4.97ms | 1.077× | 2.17% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.33s | 1.280× | 0.67% | PASS |
| file_reads | `table_scan` | 1.19s | 1.40s | 1.178× | 1.38% | PASS |
| file_reads | `oltp_read_only` | 214.64ms | 161.12ms | 0.751× | 1.02% | PASS |
| file_writes | `oltp_bulk_insert` | 194.77ms | 271.50ms | 1.394× | 1.44% | PASS |
| file_writes | `oltp_insert` | 22.17ms | 35.76ms | 1.613× | 1.92% | PASS |
| file_writes | `oltp_update_index` | 76.77ms | 126.89ms | 1.653× | 1.09% | PASS |
| file_writes | `oltp_update_non_index` | 58.75ms | 81.49ms | 1.387× | 3.56% | PASS |
| file_writes | `oltp_delete_insert` | 67.98ms | 98.71ms | 1.452× | 1.86% | PASS |
| file_writes | `oltp_write_only` | 44.67ms | 63.98ms | 1.432× | 2.29% | PASS |
| file_writes | `types_delete_insert` | 40.09ms | 53.27ms | 1.329× | 1.21% | PASS |
| file_writes | `oltp_read_write` | 91.51ms | 129.19ms | 1.412× | 1.27% | PASS |
| ac_reads | `oltp_point_select` | 49.00ms | 55.12ms | 1.125× | 0.79% | PASS |
| ac_reads | `oltp_range_select` | 13.33ms | 15.84ms | 1.188× | 1.94% | PASS |
| ac_reads | `oltp_sum_range` | 12.73ms | 15.12ms | 1.188× | 1.45% | PASS |
| ac_reads | `oltp_order_range` | 3.05ms | 3.35ms | 1.100× | 1.77% | PASS |
| ac_reads | `oltp_distinct_range` | 4.04ms | 4.40ms | 1.090× | 0.99% | PASS |
| ac_reads | `oltp_index_scan` | 7.08ms | 8.43ms | 1.190× | 1.40% | PASS |
| ac_reads | `select_random_points` | 14.40ms | 14.10ms | 0.979× | 2.10% | PASS |
| ac_reads | `select_random_ranges` | 5.74ms | 6.63ms | 1.156× | 1.40% | PASS |
| ac_reads | `covering_index_scan` | 7.41ms | 7.17ms | 0.967× | 1.34% | PASS |
| ac_reads | `groupby_scan` | 30.53ms | 33.49ms | 1.097× | 1.25% | PASS |
| ac_reads | `index_join` | 7.71ms | 10.08ms | 1.308× | 1.60% | PASS |
| ac_reads | `index_join_scan` | 3.94ms | 4.93ms | 1.249× | 1.27% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.33s | 1.283× | 0.64% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.39s | 1.194× | 0.50% | PASS |
| ac_reads | `oltp_read_only` | 143.03ms | 161.60ms | 1.130× | 1.62% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.10ms | 81.10ms | 3.844× | 7.50% | PASS |
| ac_writes | `oltp_insert_ac` | 23.79ms | 95.91ms | 4.031× | 5.40% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.11ms | 111.08ms | 4.254× | 5.08% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.75ms | 90.64ms | 3.983× | 6.70% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.16ms | 104.28ms | 4.146× | 5.81% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.37ms | 101.94ms | 4.183× | 6.27% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.65ms | 92.54ms | 4.275× | 7.14% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.43ms | 109.86ms | 3.610× | 6.08% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.15ms | 28.64ms | 1.139× | 1.56% | PASS |
| mem_reads | `oltp_range_select` | 12.45ms | 11.26ms | 0.905× | 1.61% | PASS |
| mem_reads | `oltp_sum_range` | 10.84ms | 11.09ms | 1.023× | 1.30% | PASS |
| mem_reads | `oltp_order_range` | 2.67ms | 2.58ms | 0.965× | 0.92% | PASS |
| mem_reads | `oltp_distinct_range` | 3.46ms | 3.35ms | 0.970× | 0.83% | PASS |
| mem_reads | `oltp_index_scan` | 3.85ms | 5.11ms | 1.328× | 1.24% | PASS |
| mem_reads | `select_random_points` | 15.46ms | 16.99ms | 1.099× | 1.12% | PASS |
| mem_reads | `select_random_ranges` | 3.57ms | 4.32ms | 1.209× | 0.98% | PASS |
| mem_reads | `covering_index_scan` | 3.91ms | 3.89ms | 0.996× | 2.74% | PASS |
| mem_reads | `groupby_scan` | 27.67ms | 28.24ms | 1.021× | 0.75% | PASS |
| mem_reads | `index_join` | 5.80ms | 8.11ms | 1.397× | 1.90% | PASS |
| mem_reads | `index_join_scan` | 4.29ms | 5.09ms | 1.185× | 1.03% | PASS |
| mem_reads | `types_table_scan` | 1.01s | 1.02s | 1.006× | 3.20% | PASS |
| mem_reads | `table_scan` | 1.39s | 1.12s | 0.801× | 1.22% | PASS |
| mem_reads | `oltp_read_only` | 103.42ms | 104.27ms | 1.008× | 1.38% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.29ms | 262.32ms | 1.447× | 0.83% | PASS |
| mem_writes | `oltp_insert` | 18.12ms | 30.71ms | 1.695× | 1.17% | PASS |
| mem_writes | `oltp_update_index` | 62.17ms | 110.88ms | 1.784× | 1.82% | PASS |
| mem_writes | `oltp_update_non_index` | 42.60ms | 70.46ms | 1.654× | 1.58% | PASS |
| mem_writes | `oltp_delete_insert` | 44.68ms | 85.93ms | 1.923× | 1.40% | PASS |
| mem_writes | `oltp_write_only` | 25.55ms | 50.89ms | 1.992× | 1.11% | PASS |
| mem_writes | `types_delete_insert` | 27.98ms | 42.85ms | 1.532× | 0.90% | PASS |
| mem_writes | `oltp_read_write` | 74.27ms | 109.01ms | 1.468× | 1.76% | PASS |
| file_reads | `oltp_point_select` | 100.49ms | 53.33ms | 0.531× | 0.88% | PASS |
| file_reads | `oltp_range_select` | 20.66ms | 13.72ms | 0.664× | 0.97% | PASS |
| file_reads | `oltp_sum_range` | 19.10ms | 13.51ms | 0.707× | 1.07% | PASS |
| file_reads | `oltp_order_range` | 3.54ms | 2.85ms | 0.804× | 0.96% | PASS |
| file_reads | `oltp_distinct_range` | 4.33ms | 3.62ms | 0.837× | 0.68% | PASS |
| file_reads | `oltp_index_scan` | 11.88ms | 7.70ms | 0.648× | 0.77% | PASS |
| file_reads | `select_random_points` | 23.80ms | 19.44ms | 0.817× | 1.22% | PASS |
| file_reads | `select_random_ranges` | 11.24ms | 6.76ms | 0.601× | 0.62% | PASS |
| file_reads | `covering_index_scan` | 12.84ms | 6.45ms | 0.502× | 0.74% | PASS |
| file_reads | `groupby_scan` | 28.64ms | 28.45ms | 0.994× | 0.68% | PASS |
| file_reads | `index_join` | 10.74ms | 9.50ms | 0.885× | 0.94% | PASS |
| file_reads | `index_join_scan` | 5.07ms | 5.29ms | 1.042× | 0.98% | PASS |
| file_reads | `types_table_scan` | 1.01s | 1.01s | 0.994× | 2.66% | PASS |
| file_reads | `table_scan` | 1.38s | 1.12s | 0.810× | 1.22% | PASS |
| file_reads | `oltp_read_only` | 214.09ms | 140.32ms | 0.655× | 0.87% | PASS |
| file_writes | `oltp_bulk_insert` | 247.80ms | 349.12ms | 1.409× | 2.91% | PASS |
| file_writes | `oltp_insert` | 61.23ms | 58.78ms | 0.960× | 6.57% | PASS |
| file_writes | `oltp_update_index` | 211.90ms | 208.64ms | 0.985× | 3.68% | PASS |
| file_writes | `oltp_update_non_index` | 159.67ms | 139.50ms | 0.874× | 6.32% | PASS |
| file_writes | `oltp_delete_insert` | 171.61ms | 168.26ms | 0.980× | 8.79% | PASS |
| file_writes | `oltp_write_only` | 127.83ms | 118.67ms | 0.928× | 2.24% | PASS |
| file_writes | `types_delete_insert` | 105.15ms | 106.64ms | 1.014× | 12.36% | PASS |
| file_writes | `oltp_read_write` | 179.09ms | 178.30ms | 0.996× | 4.86% | PASS |
| ac_reads | `oltp_point_select` | 49.96ms | 53.56ms | 1.072× | 0.81% | PASS |
| ac_reads | `oltp_range_select` | 15.67ms | 13.68ms | 0.873× | 1.28% | PASS |
| ac_reads | `oltp_sum_range` | 13.99ms | 13.51ms | 0.965× | 1.01% | PASS |
| ac_reads | `oltp_order_range` | 3.14ms | 2.86ms | 0.912× | 0.83% | PASS |
| ac_reads | `oltp_distinct_range` | 3.87ms | 3.64ms | 0.941× | 0.74% | PASS |
| ac_reads | `oltp_index_scan` | 6.94ms | 7.72ms | 1.112× | 0.57% | PASS |
| ac_reads | `select_random_points` | 18.70ms | 19.50ms | 1.043× | 1.30% | PASS |
| ac_reads | `select_random_ranges` | 6.33ms | 6.79ms | 1.072× | 0.78% | PASS |
| ac_reads | `covering_index_scan` | 7.87ms | 6.45ms | 0.820× | 0.71% | PASS |
| ac_reads | `groupby_scan` | 28.01ms | 28.45ms | 1.016× | 0.84% | PASS |
| ac_reads | `index_join` | 8.31ms | 9.54ms | 1.147× | 0.80% | PASS |
| ac_reads | `index_join_scan` | 4.63ms | 5.30ms | 1.145× | 0.78% | PASS |
| ac_reads | `types_table_scan` | 1.01s | 1.01s | 1.000× | 3.32% | PASS |
| ac_reads | `table_scan` | 1.38s | 1.12s | 0.809× | 1.06% | PASS |
| ac_reads | `oltp_read_only` | 140.24ms | 140.61ms | 1.003× | 1.29% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 28.96ms | 78.79ms | 2.720× | 6.67% | PASS |
| ac_writes | `oltp_insert_ac` | 32.96ms | 95.49ms | 2.897× | 6.45% | PASS |
| ac_writes | `oltp_update_index_ac` | 37.23ms | 124.24ms | 3.337× | 18.33% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 33.44ms | 100.69ms | 3.011× | 45.70% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 34.63ms | 127.58ms | 3.684× | 21.47% | PASS |
| ac_writes | `oltp_write_only_ac` | 36.60ms | 116.07ms | 3.171× | 41.87% | PASS |
| ac_writes | `types_delete_insert_ac` | 29.50ms | 95.04ms | 3.222× | 9.07% | PASS |
| ac_writes | `oltp_read_write_ac` | 39.29ms | 125.76ms | 3.201× | 18.08% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.34ms | 37.09ms | 1.183× | 2.43% | PASS |
| mem_reads | `oltp_range_select` | 13.02ms | 14.04ms | 1.078× | 3.84% | PASS |
| mem_reads | `oltp_sum_range` | 11.89ms | 13.94ms | 1.172× | 1.62% | PASS |
| mem_reads | `oltp_order_range` | 2.91ms | 3.15ms | 1.083× | 1.35% | PASS |
| mem_reads | `oltp_distinct_range` | 3.96ms | 4.19ms | 1.060× | 1.14% | PASS |
| mem_reads | `oltp_index_scan` | 4.43ms | 6.12ms | 1.382× | 1.53% | PASS |
| mem_reads | `select_random_points` | 17.02ms | 20.23ms | 1.189× | 2.06% | PASS |
| mem_reads | `select_random_ranges` | 3.98ms | 5.14ms | 1.291× | 2.00% | PASS |
| mem_reads | `covering_index_scan` | 4.37ms | 4.47ms | 1.024× | 1.97% | PASS |
| mem_reads | `groupby_scan` | 31.56ms | 33.50ms | 1.062× | 0.87% | PASS |
| mem_reads | `index_join` | 6.75ms | 8.80ms | 1.304× | 1.45% | PASS |
| mem_reads | `index_join_scan` | 4.30ms | 5.23ms | 1.215× | 2.88% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.24s | 1.157× | 2.35% | PASS |
| mem_reads | `table_scan` | 1.24s | 1.39s | 1.125× | 4.00% | PASS |
| mem_reads | `oltp_read_only` | 116.62ms | 134.97ms | 1.157× | 1.02% | PASS |
| mem_writes | `oltp_bulk_insert` | 241.20ms | 351.91ms | 1.459× | 1.02% | PASS |
| mem_writes | `oltp_insert` | 19.80ms | 39.69ms | 2.004× | 0.65% | PASS |
| mem_writes | `oltp_update_index` | 66.91ms | 128.73ms | 1.924× | 0.90% | PASS |
| mem_writes | `oltp_update_non_index` | 48.43ms | 84.06ms | 1.736× | 1.18% | PASS |
| mem_writes | `oltp_delete_insert` | 48.12ms | 101.98ms | 2.119× | 1.15% | PASS |
| mem_writes | `oltp_write_only` | 27.41ms | 61.46ms | 2.242× | 0.75% | PASS |
| mem_writes | `types_delete_insert` | 32.17ms | 53.74ms | 1.671× | 1.32% | PASS |
| mem_writes | `oltp_read_write` | 80.48ms | 137.79ms | 1.712× | 1.38% | PASS |
| file_reads | `oltp_point_select` | 105.15ms | 62.89ms | 0.598× | 1.15% | PASS |
| file_reads | `oltp_range_select` | 20.51ms | 16.77ms | 0.818× | 1.44% | PASS |
| file_reads | `oltp_sum_range` | 19.85ms | 16.77ms | 0.845× | 1.41% | PASS |
| file_reads | `oltp_order_range` | 3.78ms | 3.56ms | 0.941× | 1.45% | PASS |
| file_reads | `oltp_distinct_range` | 4.89ms | 4.63ms | 0.946× | 1.67% | PASS |
| file_reads | `oltp_index_scan` | 12.19ms | 9.07ms | 0.744× | 1.33% | PASS |
| file_reads | `select_random_points` | 26.51ms | 23.88ms | 0.901× | 1.82% | PASS |
| file_reads | `select_random_ranges` | 11.60ms | 7.86ms | 0.678× | 0.97% | PASS |
| file_reads | `covering_index_scan` | 12.04ms | 7.33ms | 0.609× | 0.71% | PASS |
| file_reads | `groupby_scan` | 32.59ms | 34.15ms | 1.048× | 0.81% | PASS |
| file_reads | `index_join` | 11.03ms | 11.02ms | 0.999× | 0.94% | PASS |
| file_reads | `index_join_scan` | 5.34ms | 5.71ms | 1.069× | 1.81% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.22s | 1.181× | 0.92% | PASS |
| file_reads | `table_scan` | 1.19s | 1.36s | 1.148× | 0.89% | PASS |
| file_reads | `oltp_read_only` | 229.67ms | 173.59ms | 0.756× | 1.06% | PASS |
| file_writes | `oltp_bulk_insert` | 260.97ms | 378.95ms | 1.452× | 1.09% | PASS |
| file_writes | `oltp_insert` | 31.66ms | 52.23ms | 1.650× | 1.06% | PASS |
| file_writes | `oltp_update_index` | 102.73ms | 161.87ms | 1.576× | 1.64% | PASS |
| file_writes | `oltp_update_non_index` | 78.12ms | 107.87ms | 1.381× | 0.91% | PASS |
| file_writes | `oltp_delete_insert` | 79.91ms | 130.38ms | 1.632× | 1.34% | PASS |
| file_writes | `oltp_write_only` | 54.67ms | 84.32ms | 1.542× | 1.47% | PASS |
| file_writes | `types_delete_insert` | 52.36ms | 72.29ms | 1.381× | 1.16% | PASS |
| file_writes | `oltp_read_write` | 113.28ms | 161.33ms | 1.424× | 1.57% | PASS |
| ac_reads | `oltp_point_select` | 55.27ms | 62.74ms | 1.135× | 1.24% | PASS |
| ac_reads | `oltp_range_select` | 16.07ms | 16.74ms | 1.041× | 1.46% | PASS |
| ac_reads | `oltp_sum_range` | 14.89ms | 16.68ms | 1.120× | 1.66% | PASS |
| ac_reads | `oltp_order_range` | 3.30ms | 3.54ms | 1.071× | 1.94% | PASS |
| ac_reads | `oltp_distinct_range` | 4.35ms | 4.60ms | 1.056× | 1.62% | PASS |
| ac_reads | `oltp_index_scan` | 7.30ms | 9.01ms | 1.234× | 1.38% | PASS |
| ac_reads | `select_random_points` | 21.69ms | 23.92ms | 1.102× | 1.73% | PASS |
| ac_reads | `select_random_ranges` | 6.76ms | 7.86ms | 1.162× | 1.28% | PASS |
| ac_reads | `covering_index_scan` | 7.49ms | 7.32ms | 0.978× | 2.64% | PASS |
| ac_reads | `groupby_scan` | 32.16ms | 34.12ms | 1.061× | 0.82% | PASS |
| ac_reads | `index_join` | 8.60ms | 11.07ms | 1.287× | 2.48% | PASS |
| ac_reads | `index_join_scan` | 4.81ms | 5.70ms | 1.186× | 1.63% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.22s | 1.180× | 0.44% | PASS |
| ac_reads | `table_scan` | 1.19s | 1.36s | 1.146× | 0.74% | PASS |
| ac_reads | `oltp_read_only` | 155.76ms | 173.38ms | 1.113× | 1.03% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.44ms | 76.83ms | 3.583× | 2.77% | PASS |
| ac_writes | `oltp_insert_ac` | 23.74ms | 99.86ms | 4.207× | 4.36% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.97ms | 112.63ms | 4.177× | 6.99% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.26ms | 89.60ms | 4.026× | 3.46% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.19ms | 100.90ms | 4.351× | 3.04% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.98ms | 101.57ms | 4.236× | 4.56% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.84ms | 96.65ms | 4.425× | 5.35% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.34ms | 111.33ms | 3.794× | 5.09% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.60ms | 30.14ms | 1.133× | 1.63% | PASS |
| mem_reads | `oltp_range_select` | 16.57ms | 15.94ms | 0.962× | 3.15% | PASS |
| mem_reads | `oltp_sum_range` | 14.76ms | 15.18ms | 1.029× | 1.74% | PASS |
| mem_reads | `oltp_order_range` | 2.99ms | 2.99ms | 0.999× | 1.23% | PASS |
| mem_reads | `oltp_distinct_range` | 3.82ms | 3.78ms | 0.990× | 0.93% | PASS |
| mem_reads | `oltp_index_scan` | 3.73ms | 4.71ms | 1.264× | 1.82% | PASS |
| mem_reads | `select_random_points` | 21.64ms | 24.28ms | 1.122× | 1.41% | PASS |
| mem_reads | `select_random_ranges` | 6.07ms | 6.66ms | 1.098× | 1.20% | PASS |
| mem_reads | `covering_index_scan` | 3.41ms | 3.32ms | 0.974× | 2.50% | PASS |
| mem_reads | `groupby_scan` | 30.35ms | 31.16ms | 1.027× | 0.92% | PASS |
| mem_reads | `index_join` | 6.50ms | 8.04ms | 1.237× | 1.68% | PASS |
| mem_reads | `index_join_scan` | 3.46ms | 4.59ms | 1.329× | 1.40% | PASS |
| mem_reads | `types_table_scan` | 880.66ms | 970.01ms | 1.101× | 0.39% | PASS |
| mem_reads | `table_scan` | 1.04s | 1.06s | 1.023× | 1.79% | PASS |
| mem_reads | `oltp_read_only` | 119.30ms | 123.83ms | 1.038× | 1.59% | PASS |
| mem_writes | `oltp_bulk_insert` | 191.27ms | 259.50ms | 1.357× | 0.89% | PASS |
| mem_writes | `oltp_insert` | 15.23ms | 27.42ms | 1.801× | 0.61% | PASS |
| mem_writes | `oltp_update_index` | 55.57ms | 91.14ms | 1.640× | 1.70% | PASS |
| mem_writes | `oltp_update_non_index` | 41.56ms | 64.11ms | 1.543× | 1.40% | PASS |
| mem_writes | `oltp_delete_insert` | 40.37ms | 73.40ms | 1.818× | 1.39% | PASS |
| mem_writes | `oltp_write_only` | 23.23ms | 47.35ms | 2.038× | 1.37% | PASS |
| mem_writes | `types_delete_insert` | 27.84ms | 41.92ms | 1.505× | 1.91% | PASS |
| mem_writes | `oltp_read_write` | 80.45ms | 113.83ms | 1.415× | 2.83% | PASS |
| file_reads | `oltp_point_select` | 104.35ms | 57.44ms | 0.550× | 1.53% | PASS |
| file_reads | `oltp_range_select` | 25.94ms | 19.02ms | 0.733× | 1.38% | PASS |
| file_reads | `oltp_sum_range` | 22.97ms | 17.95ms | 0.781× | 0.93% | PASS |
| file_reads | `oltp_order_range` | 3.87ms | 3.29ms | 0.850× | 1.42% | PASS |
| file_reads | `oltp_distinct_range` | 4.72ms | 4.12ms | 0.873× | 0.86% | PASS |
| file_reads | `oltp_index_scan` | 11.60ms | 7.64ms | 0.659× | 0.89% | PASS |
| file_reads | `select_random_points` | 30.39ms | 27.13ms | 0.893× | 1.26% | PASS |
| file_reads | `select_random_ranges` | 13.93ms | 9.41ms | 0.675× | 1.09% | PASS |
| file_reads | `covering_index_scan` | 11.49ms | 6.28ms | 0.546× | 0.79% | PASS |
| file_reads | `groupby_scan` | 31.80ms | 31.93ms | 1.004× | 0.78% | PASS |
| file_reads | `index_join` | 10.89ms | 10.29ms | 0.945× | 1.55% | PASS |
| file_reads | `index_join_scan` | 4.39ms | 5.04ms | 1.148× | 1.46% | PASS |
| file_reads | `types_table_scan` | 882.58ms | 972.60ms | 1.102× | 1.46% | PASS |
| file_reads | `table_scan` | 1.03s | 1.06s | 1.024× | 1.68% | PASS |
| file_reads | `oltp_read_only` | 227.28ms | 160.28ms | 0.705× | 0.84% | PASS |
| file_writes | `oltp_bulk_insert` | 244.51ms | 328.58ms | 1.344× | 4.46% | PASS |
| file_writes | `oltp_insert` | 35.61ms | 49.59ms | 1.393× | 5.18% | PASS |
| file_writes | `oltp_update_index` | 167.20ms | 169.57ms | 1.014× | 5.23% | PASS |
| file_writes | `oltp_update_non_index` | 163.21ms | 127.77ms | 0.783× | 13.50% | PASS |
| file_writes | `oltp_delete_insert` | 147.80ms | 143.47ms | 0.971× | 12.59% | PASS |
| file_writes | `oltp_write_only` | 98.70ms | 101.93ms | 1.033× | 4.00% | PASS |
| file_writes | `types_delete_insert` | 88.27ms | 89.97ms | 1.019× | 12.29% | PASS |
| file_writes | `oltp_read_write` | 176.49ms | 169.53ms | 0.961× | 14.08% | PASS |
| ac_reads | `oltp_point_select` | 51.09ms | 55.09ms | 1.078× | 0.98% | PASS |
| ac_reads | `oltp_range_select` | 19.14ms | 18.44ms | 0.964× | 1.30% | PASS |
| ac_reads | `oltp_sum_range` | 17.66ms | 17.82ms | 1.009× | 1.13% | PASS |
| ac_reads | `oltp_order_range` | 3.45ms | 3.31ms | 0.959× | 1.07% | PASS |
| ac_reads | `oltp_distinct_range` | 4.24ms | 4.12ms | 0.974× | 1.04% | PASS |
| ac_reads | `oltp_index_scan` | 6.70ms | 7.70ms | 1.150× | 0.80% | PASS |
| ac_reads | `select_random_points` | 25.20ms | 27.45ms | 1.089× | 1.60% | PASS |
| ac_reads | `select_random_ranges` | 8.94ms | 9.41ms | 1.052× | 1.01% | PASS |
| ac_reads | `covering_index_scan` | 6.43ms | 6.31ms | 0.981× | 1.08% | PASS |
| ac_reads | `groupby_scan` | 30.94ms | 31.73ms | 1.026× | 0.90% | PASS |
| ac_reads | `index_join` | 8.31ms | 10.21ms | 1.228× | 0.93% | PASS |
| ac_reads | `index_join_scan` | 3.92ms | 5.07ms | 1.293× | 1.62% | PASS |
| ac_reads | `types_table_scan` | 890.10ms | 975.36ms | 1.096× | 1.28% | PASS |
| ac_reads | `table_scan` | 1.04s | 1.06s | 1.024× | 3.01% | PASS |
| ac_reads | `oltp_read_only` | 153.63ms | 159.67ms | 1.039× | 0.68% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 37.89ms | 107.76ms | 2.844× | 33.40% | PASS |
| ac_writes | `oltp_insert_ac` | 36.19ms | 120.00ms | 3.315× | 23.84% | PASS |
| ac_writes | `oltp_update_index_ac` | 39.19ms | 134.84ms | 3.440× | 19.88% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 34.31ms | 128.87ms | 3.756× | 25.98% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 38.40ms | 132.55ms | 3.452× | 37.33% | PASS |
| ac_writes | `oltp_write_only_ac` | 36.90ms | 134.12ms | 3.635× | 22.74% | PASS |
| ac_writes | `types_delete_insert_ac` | 32.87ms | 128.09ms | 3.896× | 34.16% | PASS |
| ac_writes | `oltp_read_write_ac` | 48.33ms | 163.18ms | 3.376× | 46.44% | PASS |

</details>

## Version-control latency

Wall time: 2m 25s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 84.66ms | 200.00ms | 42.3% | 0.46% | PASS |
| `status_dirty_many_tables` | 87.68ms | 200.00ms | 43.8% | 0.34% | PASS |
| `diff_regular_working_one_table` | 80.14ms | 150.00ms | 53.4% | 0.41% | PASS |
| `diff_regular_working_many_tables` | 93.40ms | 200.00ms | 46.7% | 0.44% | PASS |
| `diff_stat_working_many_tables` | 93.64ms | 200.00ms | 46.8% | 0.40% | PASS |
| `diff_schema_working_many_tables` | 93.97ms | 200.00ms | 47.0% | 0.48% | PASS |
| `branch_list_many_branches` | 24.43ms | 100.00ms | 24.4% | 1.52% | PASS |
| `branch_create_delete` | 26.94ms | 100.00ms | 26.9% | 1.51% | PASS |
| `checkout_branch_clean` | 57.68ms | 200.00ms | 28.8% | 0.80% | PASS |
| `merge_data_no_conflicts` | 31.20ms | 150.00ms | 20.8% | 1.60% | PASS |
| `merge_schema_no_conflicts` | 24.16ms | 100.00ms | 24.2% | 1.42% | PASS |
| `merge_data_conflicts` | 129.09ms | 250.00ms | 51.6% | 0.22% | PASS |
| `merge_data_conflicts_with_resolve` | 129.08ms | 250.00ms | 51.6% | 0.31% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
