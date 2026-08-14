# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-14 12:10 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260810.271.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/31792741951)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 40s | 8.86s | 11.47s | 1.295× | 1.36% | **PASS** |
| textpk | 69 | 55 | 1h 32m 56s | 9.97s | 11.92s | 1.195× | 2.04% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 29s | 9.36s | 11.74s | 1.255× | 1.92% | **PASS** |
| compositepk | 69 | 55 | 1h 26m 29s | 9.60s | 12.35s | 1.286× | 1.18% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.05ms | 29.77ms | 1.188× | 1.77% | PASS |
| mem_reads | `oltp_range_select` | 10.57ms | 13.22ms | 1.251× | 2.02% | PASS |
| mem_reads | `oltp_sum_range` | 10.01ms | 12.42ms | 1.241× | 2.30% | PASS |
| mem_reads | `oltp_order_range` | 2.60ms | 3.00ms | 1.152× | 1.08% | PASS |
| mem_reads | `oltp_distinct_range` | 3.67ms | 4.04ms | 1.098× | 0.92% | PASS |
| mem_reads | `oltp_index_scan` | 4.00ms | 5.45ms | 1.363× | 1.44% | PASS |
| mem_reads | `select_random_points` | 10.93ms | 11.34ms | 1.037× | 2.26% | PASS |
| mem_reads | `select_random_ranges` | 3.07ms | 4.01ms | 1.304× | 1.33% | PASS |
| mem_reads | `covering_index_scan` | 4.26ms | 4.36ms | 1.023× | 1.86% | PASS |
| mem_reads | `groupby_scan` | 30.08ms | 32.74ms | 1.088× | 0.94% | PASS |
| mem_reads | `index_join` | 6.04ms | 8.32ms | 1.377× | 1.72% | PASS |
| mem_reads | `index_join_scan` | 3.52ms | 4.55ms | 1.296× | 1.94% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.33s | 1.286× | 0.51% | PASS |
| mem_reads | `table_scan` | 1.19s | 1.40s | 1.180× | 0.73% | PASS |
| mem_reads | `oltp_read_only` | 105.23ms | 124.36ms | 1.182× | 1.17% | PASS |
| mem_writes | `oltp_bulk_insert` | 182.10ms | 253.11ms | 1.390× | 1.15% | PASS |
| mem_writes | `oltp_insert` | 15.56ms | 28.52ms | 1.833× | 1.10% | PASS |
| mem_writes | `oltp_update_index` | 50.33ms | 104.24ms | 2.071× | 1.39% | PASS |
| mem_writes | `oltp_update_non_index` | 34.75ms | 59.57ms | 1.714× | 1.77% | PASS |
| mem_writes | `oltp_delete_insert` | 44.98ms | 78.54ms | 1.746× | 1.06% | PASS |
| mem_writes | `oltp_write_only` | 21.89ms | 44.74ms | 2.044× | 1.36% | PASS |
| mem_writes | `types_delete_insert` | 25.15ms | 40.51ms | 1.611× | 1.43% | PASS |
| mem_writes | `oltp_read_write` | 68.13ms | 110.70ms | 1.625× | 1.67% | PASS |
| file_reads | `oltp_point_select` | 99.13ms | 55.40ms | 0.559× | 0.73% | PASS |
| file_reads | `oltp_range_select` | 18.28ms | 15.81ms | 0.865× | 1.93% | PASS |
| file_reads | `oltp_sum_range` | 17.69ms | 15.08ms | 0.852× | 1.48% | PASS |
| file_reads | `oltp_order_range` | 3.40ms | 3.31ms | 0.976× | 1.42% | PASS |
| file_reads | `oltp_distinct_range` | 4.53ms | 4.37ms | 0.966× | 1.07% | PASS |
| file_reads | `oltp_index_scan` | 11.70ms | 8.31ms | 0.710× | 1.08% | PASS |
| file_reads | `select_random_points` | 18.44ms | 13.91ms | 0.754× | 2.25% | PASS |
| file_reads | `select_random_ranges` | 10.58ms | 6.61ms | 0.625× | 0.71% | PASS |
| file_reads | `covering_index_scan` | 11.85ms | 7.13ms | 0.602× | 1.09% | PASS |
| file_reads | `groupby_scan` | 30.81ms | 33.14ms | 1.075× | 0.86% | PASS |
| file_reads | `index_join` | 10.31ms | 10.13ms | 0.983× | 1.38% | PASS |
| file_reads | `index_join_scan` | 4.55ms | 4.95ms | 1.088× | 2.00% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.32s | 1.279× | 0.74% | PASS |
| file_reads | `table_scan` | 1.17s | 1.39s | 1.195× | 0.34% | PASS |
| file_reads | `oltp_read_only` | 212.73ms | 161.84ms | 0.761× | 0.83% | PASS |
| file_writes | `oltp_bulk_insert` | 195.43ms | 272.11ms | 1.392× | 1.37% | PASS |
| file_writes | `oltp_insert` | 21.99ms | 35.81ms | 1.628× | 1.79% | PASS |
| file_writes | `oltp_update_index` | 76.69ms | 127.41ms | 1.661× | 1.25% | PASS |
| file_writes | `oltp_update_non_index` | 58.34ms | 81.49ms | 1.397× | 1.82% | PASS |
| file_writes | `oltp_delete_insert` | 69.62ms | 101.00ms | 1.451× | 1.50% | PASS |
| file_writes | `oltp_write_only` | 44.99ms | 65.17ms | 1.449× | 1.17% | PASS |
| file_writes | `types_delete_insert` | 40.56ms | 54.20ms | 1.336× | 1.21% | PASS |
| file_writes | `oltp_read_write` | 93.39ms | 131.18ms | 1.405× | 1.10% | PASS |
| ac_reads | `oltp_point_select` | 49.57ms | 55.37ms | 1.117× | 1.32% | PASS |
| ac_reads | `oltp_range_select` | 13.54ms | 15.86ms | 1.171× | 1.45% | PASS |
| ac_reads | `oltp_sum_range` | 12.81ms | 15.16ms | 1.184× | 1.21% | PASS |
| ac_reads | `oltp_order_range` | 2.92ms | 3.31ms | 1.133× | 1.36% | PASS |
| ac_reads | `oltp_distinct_range` | 3.97ms | 4.39ms | 1.104× | 1.26% | PASS |
| ac_reads | `oltp_index_scan` | 6.60ms | 8.28ms | 1.254× | 1.67% | PASS |
| ac_reads | `select_random_points` | 13.44ms | 13.99ms | 1.041× | 2.25% | PASS |
| ac_reads | `select_random_ranges` | 5.60ms | 6.60ms | 1.179× | 1.17% | PASS |
| ac_reads | `covering_index_scan` | 6.96ms | 7.07ms | 1.016× | 1.36% | PASS |
| ac_reads | `groupby_scan` | 30.22ms | 33.12ms | 1.096× | 1.13% | PASS |
| ac_reads | `index_join` | 7.64ms | 10.11ms | 1.323× | 1.78% | PASS |
| ac_reads | `index_join_scan` | 4.00ms | 4.95ms | 1.238× | 1.33% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.33s | 1.281× | 0.76% | PASS |
| ac_reads | `table_scan` | 1.19s | 1.40s | 1.180× | 1.26% | PASS |
| ac_reads | `oltp_read_only` | 143.92ms | 163.74ms | 1.138× | 0.95% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.34ms | 78.63ms | 3.685× | 4.49% | PASS |
| ac_writes | `oltp_insert_ac` | 24.44ms | 94.80ms | 3.879× | 5.40% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.78ms | 109.81ms | 4.259× | 3.46% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.83ms | 87.15ms | 3.992× | 3.45% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.57ms | 99.37ms | 4.216× | 5.60% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.73ms | 98.99ms | 4.003× | 5.19% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.61ms | 90.30ms | 3.995× | 6.50% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.89ms | 104.36ms | 3.612× | 4.19% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.56ms | 38.01ms | 1.244× | 1.52% | PASS |
| mem_reads | `oltp_range_select` | 13.62ms | 14.38ms | 1.056× | 1.67% | PASS |
| mem_reads | `oltp_sum_range` | 12.13ms | 14.25ms | 1.175× | 3.82% | PASS |
| mem_reads | `oltp_order_range` | 2.91ms | 3.18ms | 1.093× | 2.45% | PASS |
| mem_reads | `oltp_distinct_range` | 3.89ms | 4.23ms | 1.088× | 1.91% | PASS |
| mem_reads | `oltp_index_scan` | 4.31ms | 6.15ms | 1.429× | 1.52% | PASS |
| mem_reads | `select_random_points` | 17.04ms | 20.94ms | 1.229× | 1.64% | PASS |
| mem_reads | `select_random_ranges` | 3.90ms | 5.14ms | 1.318× | 3.21% | PASS |
| mem_reads | `covering_index_scan` | 4.48ms | 4.38ms | 0.979× | 1.35% | PASS |
| mem_reads | `groupby_scan` | 31.69ms | 34.10ms | 1.076× | 1.25% | PASS |
| mem_reads | `index_join` | 6.85ms | 9.03ms | 1.318× | 2.04% | PASS |
| mem_reads | `index_join_scan` | 4.19ms | 5.33ms | 1.273× | 5.01% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.24s | 1.181× | 0.95% | PASS |
| mem_reads | `table_scan` | 1.20s | 1.37s | 1.142× | 0.74% | PASS |
| mem_reads | `oltp_read_only` | 114.93ms | 137.10ms | 1.193× | 1.18% | PASS |
| mem_writes | `oltp_bulk_insert` | 237.36ms | 356.68ms | 1.503× | 0.83% | PASS |
| mem_writes | `oltp_insert` | 21.67ms | 39.58ms | 1.826× | 0.87% | PASS |
| mem_writes | `oltp_update_index` | 70.12ms | 132.91ms | 1.895× | 0.99% | PASS |
| mem_writes | `oltp_update_non_index` | 48.27ms | 86.57ms | 1.794× | 1.51% | PASS |
| mem_writes | `oltp_delete_insert` | 51.88ms | 104.98ms | 2.023× | 1.56% | PASS |
| mem_writes | `oltp_write_only` | 28.53ms | 61.47ms | 2.155× | 1.07% | PASS |
| mem_writes | `types_delete_insert` | 32.95ms | 55.18ms | 1.675× | 1.91% | PASS |
| mem_writes | `oltp_read_write` | 84.41ms | 139.49ms | 1.653× | 2.07% | PASS |
| file_reads | `oltp_point_select` | 105.03ms | 63.72ms | 0.607× | 0.84% | PASS |
| file_reads | `oltp_range_select` | 22.50ms | 17.26ms | 0.767× | 1.52% | PASS |
| file_reads | `oltp_sum_range` | 21.21ms | 17.32ms | 0.817× | 1.85% | PASS |
| file_reads | `oltp_order_range` | 4.00ms | 3.59ms | 0.897× | 1.65% | PASS |
| file_reads | `oltp_distinct_range` | 5.03ms | 4.69ms | 0.932× | 1.82% | PASS |
| file_reads | `oltp_index_scan` | 12.73ms | 9.28ms | 0.729× | 0.82% | PASS |
| file_reads | `select_random_points` | 27.72ms | 24.93ms | 0.899× | 2.10% | PASS |
| file_reads | `select_random_ranges` | 11.88ms | 7.90ms | 0.665× | 0.91% | PASS |
| file_reads | `covering_index_scan` | 13.45ms | 7.47ms | 0.555× | 1.21% | PASS |
| file_reads | `groupby_scan` | 33.62ms | 34.90ms | 1.038× | 0.87% | PASS |
| file_reads | `index_join` | 12.01ms | 11.38ms | 0.947× | 2.05% | PASS |
| file_reads | `index_join_scan` | 5.96ms | 6.01ms | 1.008× | 3.07% | PASS |
| file_reads | `types_table_scan` | 1.18s | 1.27s | 1.069× | 2.26% | PASS |
| file_reads | `table_scan` | 1.30s | 1.39s | 1.070× | 5.78% | PASS |
| file_reads | `oltp_read_only` | 223.90ms | 175.62ms | 0.784× | 1.07% | PASS |
| file_writes | `oltp_bulk_insert` | 257.20ms | 387.20ms | 1.505× | 0.80% | PASS |
| file_writes | `oltp_insert` | 54.00ms | 52.94ms | 0.980× | 24.71% | PASS |
| file_writes | `oltp_update_index` | 114.14ms | 169.61ms | 1.486× | 1.91% | PASS |
| file_writes | `oltp_update_non_index` | 97.09ms | 112.69ms | 1.161× | 13.74% | PASS |
| file_writes | `oltp_delete_insert` | 91.36ms | 134.25ms | 1.470× | 2.39% | PASS |
| file_writes | `oltp_write_only` | 86.04ms | 84.73ms | 0.985× | 10.11% | PASS |
| file_writes | `types_delete_insert` | 55.90ms | 74.84ms | 1.339× | 1.33% | PASS |
| file_writes | `oltp_read_write` | 140.88ms | 164.20ms | 1.166× | 6.90% | PASS |
| ac_reads | `oltp_point_select` | 56.17ms | 63.85ms | 1.137× | 1.36% | PASS |
| ac_reads | `oltp_range_select` | 17.02ms | 17.16ms | 1.008× | 2.71% | PASS |
| ac_reads | `oltp_sum_range` | 15.85ms | 17.15ms | 1.082× | 2.34% | PASS |
| ac_reads | `oltp_order_range` | 3.52ms | 3.62ms | 1.027× | 2.20% | PASS |
| ac_reads | `oltp_distinct_range` | 4.47ms | 4.65ms | 1.041× | 2.12% | PASS |
| ac_reads | `oltp_index_scan` | 7.54ms | 9.26ms | 1.228× | 3.16% | PASS |
| ac_reads | `select_random_points` | 21.55ms | 24.93ms | 1.157× | 3.08% | PASS |
| ac_reads | `select_random_ranges` | 6.94ms | 7.92ms | 1.141× | 1.20% | PASS |
| ac_reads | `covering_index_scan` | 8.88ms | 7.46ms | 0.840× | 2.35% | PASS |
| ac_reads | `groupby_scan` | 32.41ms | 34.67ms | 1.070× | 1.43% | PASS |
| ac_reads | `index_join` | 8.74ms | 11.28ms | 1.292× | 2.37% | PASS |
| ac_reads | `index_join_scan` | 5.33ms | 6.00ms | 1.125× | 2.42% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.23s | 1.168× | 0.83% | PASS |
| ac_reads | `table_scan` | 1.42s | 1.40s | 0.988× | 5.52% | PASS |
| ac_reads | `oltp_read_only` | 159.95ms | 177.40ms | 1.109× | 2.12% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.23ms | 80.11ms | 3.604× | 5.36% | PASS |
| ac_writes | `oltp_insert_ac` | 25.96ms | 97.24ms | 3.746× | 5.17% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.76ms | 114.41ms | 4.122× | 3.76% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.42ms | 94.48ms | 4.215× | 6.25% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.85ms | 104.72ms | 4.213× | 5.06% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.28ms | 103.89ms | 4.109× | 5.86% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.56ms | 95.06ms | 4.409× | 6.37% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.67ms | 112.47ms | 3.551× | 3.67% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.42ms | 37.15ms | 1.221× | 1.92% | PASS |
| mem_reads | `oltp_range_select` | 12.97ms | 13.95ms | 1.075× | 2.53% | PASS |
| mem_reads | `oltp_sum_range` | 12.35ms | 14.00ms | 1.134× | 2.36% | PASS |
| mem_reads | `oltp_order_range` | 2.88ms | 3.13ms | 1.084× | 1.19% | PASS |
| mem_reads | `oltp_distinct_range` | 3.98ms | 4.21ms | 1.055× | 1.26% | PASS |
| mem_reads | `oltp_index_scan` | 4.65ms | 6.40ms | 1.377× | 1.99% | PASS |
| mem_reads | `select_random_points` | 18.37ms | 20.77ms | 1.131× | 2.24% | PASS |
| mem_reads | `select_random_ranges` | 4.05ms | 5.18ms | 1.279× | 2.02% | PASS |
| mem_reads | `covering_index_scan` | 4.50ms | 4.68ms | 1.040× | 2.20% | PASS |
| mem_reads | `groupby_scan` | 32.05ms | 33.82ms | 1.055× | 0.70% | PASS |
| mem_reads | `index_join` | 6.86ms | 9.23ms | 1.345× | 2.49% | PASS |
| mem_reads | `index_join_scan` | 4.38ms | 5.33ms | 1.219× | 3.12% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.23s | 1.161× | 1.79% | PASS |
| mem_reads | `table_scan` | 1.23s | 1.38s | 1.118× | 3.05% | PASS |
| mem_reads | `oltp_read_only` | 119.85ms | 135.77ms | 1.133× | 1.62% | PASS |
| mem_writes | `oltp_bulk_insert` | 241.88ms | 355.27ms | 1.469× | 0.88% | PASS |
| mem_writes | `oltp_insert` | 20.29ms | 40.23ms | 1.983× | 1.14% | PASS |
| mem_writes | `oltp_update_index` | 72.79ms | 135.82ms | 1.866× | 2.52% | PASS |
| mem_writes | `oltp_update_non_index` | 50.60ms | 86.58ms | 1.711× | 1.32% | PASS |
| mem_writes | `oltp_delete_insert` | 50.87ms | 105.02ms | 2.065× | 1.78% | PASS |
| mem_writes | `oltp_write_only` | 28.83ms | 63.35ms | 2.197× | 1.69% | PASS |
| mem_writes | `types_delete_insert` | 32.46ms | 53.66ms | 1.653× | 1.31% | PASS |
| mem_writes | `oltp_read_write` | 82.26ms | 137.22ms | 1.668× | 1.20% | PASS |
| file_reads | `oltp_point_select` | 104.78ms | 63.13ms | 0.602× | 0.90% | PASS |
| file_reads | `oltp_range_select` | 20.29ms | 16.66ms | 0.821× | 2.40% | PASS |
| file_reads | `oltp_sum_range` | 19.81ms | 16.73ms | 0.844× | 2.32% | PASS |
| file_reads | `oltp_order_range` | 3.71ms | 3.44ms | 0.926× | 2.43% | PASS |
| file_reads | `oltp_distinct_range` | 4.78ms | 4.56ms | 0.954× | 1.32% | PASS |
| file_reads | `oltp_index_scan` | 12.18ms | 9.11ms | 0.748× | 1.69% | PASS |
| file_reads | `select_random_points` | 26.74ms | 24.24ms | 0.906× | 2.45% | PASS |
| file_reads | `select_random_ranges` | 11.60ms | 7.86ms | 0.678× | 1.28% | PASS |
| file_reads | `covering_index_scan` | 12.09ms | 7.35ms | 0.607× | 1.98% | PASS |
| file_reads | `groupby_scan` | 33.08ms | 34.20ms | 1.034× | 0.75% | PASS |
| file_reads | `index_join` | 11.25ms | 11.26ms | 1.000× | 1.63% | PASS |
| file_reads | `index_join_scan` | 5.32ms | 5.71ms | 1.074× | 2.49% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.22s | 1.184× | 0.92% | PASS |
| file_reads | `table_scan` | 1.19s | 1.36s | 1.149× | 1.24% | PASS |
| file_reads | `oltp_read_only` | 230.27ms | 174.53ms | 0.758× | 0.98% | PASS |
| file_writes | `oltp_bulk_insert` | 260.47ms | 378.67ms | 1.454× | 0.96% | PASS |
| file_writes | `oltp_insert` | 31.41ms | 51.91ms | 1.653× | 1.84% | PASS |
| file_writes | `oltp_update_index` | 103.44ms | 163.53ms | 1.581× | 1.86% | PASS |
| file_writes | `oltp_update_non_index` | 79.04ms | 108.47ms | 1.372× | 1.55% | PASS |
| file_writes | `oltp_delete_insert` | 80.19ms | 130.46ms | 1.627× | 1.76% | PASS |
| file_writes | `oltp_write_only` | 53.49ms | 83.91ms | 1.569× | 1.60% | PASS |
| file_writes | `types_delete_insert` | 51.84ms | 71.62ms | 1.382× | 1.96% | PASS |
| file_writes | `oltp_read_write` | 111.77ms | 160.50ms | 1.436× | 1.70% | PASS |
| ac_reads | `oltp_point_select` | 54.41ms | 63.44ms | 1.166× | 0.97% | PASS |
| ac_reads | `oltp_range_select` | 15.47ms | 16.93ms | 1.094× | 4.56% | PASS |
| ac_reads | `oltp_sum_range` | 14.92ms | 16.85ms | 1.129× | 2.92% | PASS |
| ac_reads | `oltp_order_range` | 3.18ms | 3.47ms | 1.093× | 2.11% | PASS |
| ac_reads | `oltp_distinct_range` | 4.24ms | 4.58ms | 1.079× | 2.16% | PASS |
| ac_reads | `oltp_index_scan` | 7.19ms | 9.16ms | 1.274× | 2.91% | PASS |
| ac_reads | `select_random_points` | 21.42ms | 24.30ms | 1.135× | 2.35% | PASS |
| ac_reads | `select_random_ranges` | 6.87ms | 7.85ms | 1.143× | 1.22% | PASS |
| ac_reads | `covering_index_scan` | 7.74ms | 7.33ms | 0.947× | 2.14% | PASS |
| ac_reads | `groupby_scan` | 32.51ms | 34.23ms | 1.053× | 0.94% | PASS |
| ac_reads | `index_join` | 9.10ms | 11.32ms | 1.244× | 2.96% | PASS |
| ac_reads | `index_join_scan` | 5.07ms | 5.87ms | 1.158× | 3.34% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.22s | 1.175× | 0.97% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.36s | 1.163× | 0.49% | PASS |
| ac_reads | `oltp_read_only` | 151.91ms | 175.03ms | 1.152× | 0.99% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.67ms | 77.94ms | 3.596× | 4.32% | PASS |
| ac_writes | `oltp_insert_ac` | 23.85ms | 99.66ms | 4.179× | 3.74% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.16ms | 109.29ms | 4.178× | 3.91% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.59ms | 91.41ms | 3.875× | 5.30% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.55ms | 102.32ms | 4.344× | 3.42% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.97ms | 100.64ms | 4.198× | 3.69% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.29ms | 92.56ms | 4.347× | 4.94% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.26ms | 106.96ms | 3.656× | 3.00% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.01ms | 40.41ms | 1.224× | 1.36% | PASS |
| mem_reads | `oltp_range_select` | 19.08ms | 21.16ms | 1.109× | 1.44% | PASS |
| mem_reads | `oltp_sum_range` | 17.88ms | 20.93ms | 1.170× | 1.18% | PASS |
| mem_reads | `oltp_order_range` | 3.51ms | 3.89ms | 1.108× | 0.87% | PASS |
| mem_reads | `oltp_distinct_range` | 4.68ms | 4.98ms | 1.064× | 1.08% | PASS |
| mem_reads | `oltp_index_scan` | 4.58ms | 6.31ms | 1.378× | 1.52% | PASS |
| mem_reads | `select_random_points` | 28.17ms | 32.30ms | 1.147× | 2.36% | PASS |
| mem_reads | `select_random_ranges` | 7.63ms | 9.07ms | 1.189× | 1.32% | PASS |
| mem_reads | `covering_index_scan` | 4.21ms | 4.26ms | 1.014× | 2.45% | PASS |
| mem_reads | `groupby_scan` | 36.79ms | 39.22ms | 1.066× | 1.09% | PASS |
| mem_reads | `index_join` | 8.16ms | 10.57ms | 1.294× | 2.20% | PASS |
| mem_reads | `index_join_scan` | 4.18ms | 5.53ms | 1.321× | 2.23% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.24s | 1.197× | 0.95% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.40s | 1.186× | 1.33% | PASS |
| mem_reads | `oltp_read_only` | 145.24ms | 168.63ms | 1.161× | 0.62% | PASS |
| mem_writes | `oltp_bulk_insert` | 243.43ms | 350.99ms | 1.442× | 0.85% | PASS |
| mem_writes | `oltp_insert` | 18.99ms | 36.29ms | 1.911× | 0.61% | PASS |
| mem_writes | `oltp_update_index` | 67.34ms | 117.34ms | 1.742× | 1.48% | PASS |
| mem_writes | `oltp_update_non_index` | 48.94ms | 81.94ms | 1.674× | 1.30% | PASS |
| mem_writes | `oltp_delete_insert` | 47.77ms | 93.82ms | 1.964× | 0.91% | PASS |
| mem_writes | `oltp_write_only` | 26.17ms | 56.62ms | 2.163× | 1.18% | PASS |
| mem_writes | `types_delete_insert` | 31.10ms | 53.59ms | 1.723× | 1.09% | PASS |
| mem_writes | `oltp_read_write` | 97.05ms | 152.26ms | 1.569× | 0.97% | PASS |
| file_reads | `oltp_point_select` | 104.26ms | 65.16ms | 0.625× | 1.09% | PASS |
| file_reads | `oltp_range_select` | 26.43ms | 23.96ms | 0.907× | 1.43% | PASS |
| file_reads | `oltp_sum_range` | 25.43ms | 23.84ms | 0.937× | 2.14% | PASS |
| file_reads | `oltp_order_range` | 4.41ms | 4.23ms | 0.961× | 1.20% | PASS |
| file_reads | `oltp_distinct_range` | 5.56ms | 5.32ms | 0.956× | 1.00% | PASS |
| file_reads | `oltp_index_scan` | 12.13ms | 8.67ms | 0.715× | 1.18% | PASS |
| file_reads | `select_random_points` | 36.67ms | 35.71ms | 0.974× | 1.09% | PASS |
| file_reads | `select_random_ranges` | 15.28ms | 11.83ms | 0.774× | 1.15% | PASS |
| file_reads | `covering_index_scan` | 11.73ms | 6.89ms | 0.587× | 1.94% | PASS |
| file_reads | `groupby_scan` | 37.25ms | 39.66ms | 1.065× | 0.88% | PASS |
| file_reads | `index_join` | 12.46ms | 12.28ms | 0.986× | 1.08% | PASS |
| file_reads | `index_join_scan` | 5.17ms | 5.93ms | 1.149× | 1.55% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.23s | 1.193× | 0.99% | PASS |
| file_reads | `table_scan` | 1.19s | 1.39s | 1.167× | 1.41% | PASS |
| file_reads | `oltp_read_only` | 255.37ms | 208.02ms | 0.815× | 0.97% | PASS |
| file_writes | `oltp_bulk_insert` | 258.38ms | 371.46ms | 1.438× | 0.86% | PASS |
| file_writes | `oltp_insert` | 29.04ms | 45.98ms | 1.584× | 1.45% | PASS |
| file_writes | `oltp_update_index` | 97.05ms | 141.05ms | 1.453× | 1.28% | PASS |
| file_writes | `oltp_update_non_index` | 78.27ms | 102.29ms | 1.307× | 1.15% | PASS |
| file_writes | `oltp_delete_insert` | 79.34ms | 116.69ms | 1.471× | 1.11% | PASS |
| file_writes | `oltp_write_only` | 57.06ms | 77.84ms | 1.364× | 1.88% | PASS |
| file_writes | `types_delete_insert` | 50.61ms | 67.53ms | 1.334× | 2.00% | PASS |
| file_writes | `oltp_read_write` | 133.84ms | 174.93ms | 1.307× | 1.02% | PASS |
| ac_reads | `oltp_point_select` | 58.16ms | 66.03ms | 1.135× | 1.35% | PASS |
| ac_reads | `oltp_range_select` | 22.47ms | 24.36ms | 1.084× | 1.05% | PASS |
| ac_reads | `oltp_sum_range` | 21.12ms | 24.15ms | 1.144× | 0.77% | PASS |
| ac_reads | `oltp_order_range` | 3.98ms | 4.27ms | 1.071× | 0.92% | PASS |
| ac_reads | `oltp_distinct_range` | 5.08ms | 5.36ms | 1.055× | 0.77% | PASS |
| ac_reads | `oltp_index_scan` | 7.42ms | 9.13ms | 1.230× | 1.00% | PASS |
| ac_reads | `select_random_points` | 31.86ms | 36.49ms | 1.145× | 1.46% | PASS |
| ac_reads | `select_random_ranges` | 10.44ms | 11.91ms | 1.141× | 1.14% | PASS |
| ac_reads | `covering_index_scan` | 7.14ms | 7.17ms | 1.005× | 0.96% | PASS |
| ac_reads | `groupby_scan` | 36.69ms | 39.77ms | 1.084× | 0.67% | PASS |
| ac_reads | `index_join` | 9.81ms | 12.66ms | 1.291× | 1.48% | PASS |
| ac_reads | `index_join_scan` | 4.62ms | 6.00ms | 1.297× | 2.06% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.24s | 1.200× | 0.65% | PASS |
| ac_reads | `table_scan` | 1.17s | 1.39s | 1.187× | 0.45% | PASS |
| ac_reads | `oltp_read_only` | 182.94ms | 208.46ms | 1.140× | 1.12% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 38.69ms | 125.55ms | 3.245× | 11.15% | PASS |
| ac_writes | `oltp_insert_ac` | 39.81ms | 145.52ms | 3.655× | 8.20% | PASS |
| ac_writes | `oltp_update_index_ac` | 33.92ms | 141.56ms | 4.174× | 6.88% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 39.92ms | 141.93ms | 3.555× | 10.54% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 42.84ms | 152.29ms | 3.555× | 10.62% | PASS |
| ac_writes | `oltp_write_only_ac` | 40.71ms | 142.49ms | 3.500× | 8.62% | PASS |
| ac_writes | `types_delete_insert_ac` | 34.80ms | 132.35ms | 3.803× | 10.26% | PASS |
| ac_writes | `oltp_read_write_ac` | 45.77ms | 151.65ms | 3.314× | 9.41% | PASS |

</details>

## Version-control latency

Wall time: 2m 27s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 92.80ms | 200.00ms | 46.4% | 0.28% | PASS |
| `status_dirty_many_tables` | 96.14ms | 200.00ms | 48.1% | 0.21% | PASS |
| `diff_regular_working_one_table` | 88.52ms | 150.00ms | 59.0% | 0.25% | PASS |
| `diff_regular_working_many_tables` | 101.75ms | 200.00ms | 50.9% | 0.21% | PASS |
| `diff_stat_working_many_tables` | 101.73ms | 200.00ms | 50.9% | 0.23% | PASS |
| `diff_schema_working_many_tables` | 102.20ms | 200.00ms | 51.1% | 0.18% | PASS |
| `branch_list_many_branches` | 24.10ms | 100.00ms | 24.1% | 0.83% | PASS |
| `branch_create_delete` | 26.07ms | 100.00ms | 26.1% | 0.88% | PASS |
| `checkout_branch_clean` | 59.53ms | 200.00ms | 29.8% | 0.43% | PASS |
| `merge_data_no_conflicts` | 30.62ms | 150.00ms | 20.4% | 0.58% | PASS |
| `merge_schema_no_conflicts` | 22.76ms | 100.00ms | 22.8% | 0.66% | PASS |
| `merge_data_conflicts` | 129.17ms | 250.00ms | 51.7% | 0.15% | PASS |
| `merge_data_conflicts_with_resolve` | 128.92ms | 250.00ms | 51.6% | 0.12% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
