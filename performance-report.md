# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-20 15:16 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260907.300.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/35514139717)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 9m 12s | 8.58s | 10.60s | 1.235× | 1.01% | **PASS** |
| textpk | 69 | 55 | 1h 34m 32s | 10.34s | 12.09s | 1.170× | 1.66% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 44s | 9.20s | 10.39s | 1.129× | 1.43% | **PASS** |
| compositepk | 69 | 55 | 1h 20m 22s | 8.85s | 10.14s | 1.145× | 1.67% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.64ms | 23.16ms | 1.122× | 0.69% | PASS |
| mem_reads | `oltp_range_select` | 9.19ms | 11.01ms | 1.197× | 0.83% | PASS |
| mem_reads | `oltp_sum_range` | 8.55ms | 10.42ms | 1.219× | 1.02% | PASS |
| mem_reads | `oltp_order_range` | 2.43ms | 2.70ms | 1.113× | 1.03% | PASS |
| mem_reads | `oltp_distinct_range` | 3.33ms | 3.59ms | 1.077× | 0.63% | PASS |
| mem_reads | `oltp_index_scan` | 3.46ms | 4.18ms | 1.210× | 0.88% | PASS |
| mem_reads | `select_random_points` | 9.71ms | 10.29ms | 1.059× | 1.25% | PASS |
| mem_reads | `select_random_ranges` | 2.59ms | 3.31ms | 1.278× | 0.88% | PASS |
| mem_reads | `covering_index_scan` | 3.41ms | 3.39ms | 0.993× | 0.62% | PASS |
| mem_reads | `groupby_scan` | 28.76ms | 30.74ms | 1.069× | 0.37% | PASS |
| mem_reads | `index_join` | 5.22ms | 7.04ms | 1.349× | 0.74% | PASS |
| mem_reads | `index_join_scan` | 2.85ms | 4.07ms | 1.425× | 1.01% | PASS |
| mem_reads | `types_table_scan` | 988.53ms | 1.19s | 1.200× | 0.23% | PASS |
| mem_reads | `table_scan` | 1.12s | 1.29s | 1.150× | 0.29% | PASS |
| mem_reads | `oltp_read_only` | 90.91ms | 104.60ms | 1.151× | 0.46% | PASS |
| mem_writes | `oltp_bulk_insert` | 140.09ms | 186.76ms | 1.333× | 0.42% | PASS |
| mem_writes | `oltp_insert` | 12.67ms | 22.07ms | 1.742× | 0.65% | PASS |
| mem_writes | `oltp_update_index` | 42.38ms | 85.49ms | 2.017× | 0.91% | PASS |
| mem_writes | `oltp_update_non_index` | 27.35ms | 47.19ms | 1.725× | 0.89% | PASS |
| mem_writes | `oltp_delete_insert` | 37.64ms | 61.62ms | 1.637× | 0.76% | PASS |
| mem_writes | `oltp_write_only` | 17.70ms | 35.62ms | 2.012× | 1.10% | PASS |
| mem_writes | `types_delete_insert` | 19.97ms | 31.86ms | 1.595× | 0.65% | PASS |
| mem_writes | `oltp_read_write` | 53.69ms | 87.01ms | 1.621× | 0.94% | PASS |
| file_reads | `oltp_point_select` | 51.21ms | 33.49ms | 0.654× | 0.55% | PASS |
| file_reads | `oltp_range_select` | 12.32ms | 12.47ms | 1.012× | 1.35% | PASS |
| file_reads | `oltp_sum_range` | 11.99ms | 11.79ms | 0.983× | 1.07% | PASS |
| file_reads | `oltp_order_range` | 2.77ms | 2.88ms | 1.037× | 1.33% | PASS |
| file_reads | `oltp_distinct_range` | 3.69ms | 3.76ms | 1.019× | 1.21% | PASS |
| file_reads | `oltp_index_scan` | 6.67ms | 5.37ms | 0.806× | 1.46% | PASS |
| file_reads | `select_random_points` | 13.21ms | 11.71ms | 0.886× | 1.39% | PASS |
| file_reads | `select_random_ranges` | 5.77ms | 4.43ms | 0.769× | 1.01% | PASS |
| file_reads | `covering_index_scan` | 6.72ms | 4.64ms | 0.690× | 1.61% | PASS |
| file_reads | `groupby_scan` | 29.06ms | 30.94ms | 1.065× | 0.53% | PASS |
| file_reads | `index_join` | 7.13ms | 8.13ms | 1.140× | 2.36% | PASS |
| file_reads | `index_join_scan` | 3.31ms | 4.42ms | 1.336× | 1.30% | PASS |
| file_reads | `types_table_scan` | 1.00s | 1.20s | 1.198× | 0.32% | PASS |
| file_reads | `table_scan` | 1.16s | 1.32s | 1.131× | 0.82% | PASS |
| file_reads | `oltp_read_only` | 140.29ms | 123.23ms | 0.878× | 0.42% | PASS |
| file_writes | `oltp_bulk_insert` | 192.35ms | 251.03ms | 1.305× | 1.20% | PASS |
| file_writes | `oltp_insert` | 27.89ms | 40.13ms | 1.439× | 4.25% | PASS |
| file_writes | `oltp_update_index` | 149.49ms | 162.27ms | 1.085× | 1.48% | PASS |
| file_writes | `oltp_update_non_index` | 121.25ms | 107.46ms | 0.886× | 0.70% | PASS |
| file_writes | `oltp_delete_insert` | 129.53ms | 128.28ms | 0.990× | 1.19% | PASS |
| file_writes | `oltp_write_only` | 95.67ms | 88.99ms | 0.930× | 1.85% | PASS |
| file_writes | `types_delete_insert` | 81.06ms | 71.40ms | 0.881× | 5.21% | PASS |
| file_writes | `oltp_read_write` | 131.53ms | 140.41ms | 1.068× | 1.40% | PASS |
| ac_reads | `oltp_point_select` | 30.47ms | 34.01ms | 1.116× | 1.17% | PASS |
| ac_reads | `oltp_range_select` | 10.61ms | 12.71ms | 1.197× | 0.68% | PASS |
| ac_reads | `oltp_sum_range` | 10.24ms | 11.99ms | 1.171× | 1.02% | PASS |
| ac_reads | `oltp_order_range` | 2.61ms | 2.91ms | 1.113× | 0.79% | PASS |
| ac_reads | `oltp_distinct_range` | 3.53ms | 3.78ms | 1.071× | 0.83% | PASS |
| ac_reads | `oltp_index_scan` | 4.80ms | 5.78ms | 1.203× | 0.89% | PASS |
| ac_reads | `select_random_points` | 11.54ms | 12.07ms | 1.047× | 0.90% | PASS |
| ac_reads | `select_random_ranges` | 3.70ms | 4.47ms | 1.210× | 1.10% | PASS |
| ac_reads | `covering_index_scan` | 4.58ms | 4.78ms | 1.045× | 1.53% | PASS |
| ac_reads | `groupby_scan` | 29.08ms | 30.98ms | 1.065× | 0.42% | PASS |
| ac_reads | `index_join` | 6.08ms | 8.17ms | 1.344× | 1.49% | PASS |
| ac_reads | `index_join_scan` | 3.10ms | 4.34ms | 1.399× | 1.65% | PASS |
| ac_reads | `types_table_scan` | 986.20ms | 1.19s | 1.205× | 0.33% | PASS |
| ac_reads | `table_scan` | 1.12s | 1.30s | 1.155× | 0.29% | PASS |
| ac_reads | `oltp_read_only` | 104.53ms | 120.88ms | 1.156× | 0.34% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.97ms | 87.79ms | 3.662× | 10.41% | PASS |
| ac_writes | `oltp_insert_ac` | 25.91ms | 101.64ms | 3.923× | 6.96% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.18ms | 115.57ms | 4.252× | 8.76% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.12ms | 104.88ms | 4.174× | 17.10% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.33ms | 108.78ms | 4.295× | 7.75% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.38ms | 106.16ms | 4.183× | 8.55% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.63ms | 97.74ms | 4.136× | 8.45% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.96ms | 120.57ms | 4.163× | 11.43% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.65ms | 37.38ms | 1.261× | 1.01% | PASS |
| mem_reads | `oltp_range_select` | 13.11ms | 14.26ms | 1.088× | 1.07% | PASS |
| mem_reads | `oltp_sum_range` | 11.88ms | 14.27ms | 1.201× | 1.18% | PASS |
| mem_reads | `oltp_order_range` | 2.96ms | 3.18ms | 1.074× | 1.74% | PASS |
| mem_reads | `oltp_distinct_range` | 4.05ms | 4.25ms | 1.050× | 1.24% | PASS |
| mem_reads | `oltp_index_scan` | 4.47ms | 6.34ms | 1.417× | 1.66% | PASS |
| mem_reads | `select_random_points` | 17.68ms | 20.95ms | 1.185× | 1.18% | PASS |
| mem_reads | `select_random_ranges` | 3.94ms | 5.18ms | 1.314× | 1.16% | PASS |
| mem_reads | `covering_index_scan` | 4.55ms | 4.58ms | 1.007× | 1.82% | PASS |
| mem_reads | `groupby_scan` | 31.67ms | 34.08ms | 1.076× | 0.85% | PASS |
| mem_reads | `index_join` | 6.87ms | 9.16ms | 1.332× | 1.71% | PASS |
| mem_reads | `index_join_scan` | 4.54ms | 5.32ms | 1.172× | 1.83% | PASS |
| mem_reads | `types_table_scan` | 1.06s | 1.24s | 1.166× | 0.76% | PASS |
| mem_reads | `table_scan` | 1.25s | 1.38s | 1.100× | 1.08% | PASS |
| mem_reads | `oltp_read_only` | 118.70ms | 137.61ms | 1.159× | 1.04% | PASS |
| mem_writes | `oltp_bulk_insert` | 234.34ms | 358.47ms | 1.530× | 0.87% | PASS |
| mem_writes | `oltp_insert` | 22.34ms | 40.74ms | 1.824× | 1.45% | PASS |
| mem_writes | `oltp_update_index` | 77.64ms | 141.60ms | 1.824× | 1.76% | PASS |
| mem_writes | `oltp_update_non_index` | 51.10ms | 90.92ms | 1.779× | 1.18% | PASS |
| mem_writes | `oltp_delete_insert` | 55.18ms | 109.34ms | 1.982× | 1.72% | PASS |
| mem_writes | `oltp_write_only` | 31.04ms | 64.98ms | 2.094× | 1.45% | PASS |
| mem_writes | `types_delete_insert` | 34.41ms | 58.03ms | 1.686× | 1.36% | PASS |
| mem_writes | `oltp_read_write` | 94.05ms | 146.90ms | 1.562× | 1.44% | PASS |
| file_reads | `oltp_point_select` | 105.60ms | 63.91ms | 0.605× | 1.14% | PASS |
| file_reads | `oltp_range_select` | 22.18ms | 17.38ms | 0.784× | 1.75% | PASS |
| file_reads | `oltp_sum_range` | 20.72ms | 17.40ms | 0.840× | 1.57% | PASS |
| file_reads | `oltp_order_range` | 4.19ms | 3.75ms | 0.897× | 4.12% | PASS |
| file_reads | `oltp_distinct_range` | 5.34ms | 4.86ms | 0.910× | 3.45% | PASS |
| file_reads | `oltp_index_scan` | 12.62ms | 9.22ms | 0.731× | 1.50% | PASS |
| file_reads | `select_random_points` | 27.68ms | 24.79ms | 0.896× | 1.49% | PASS |
| file_reads | `select_random_ranges` | 11.76ms | 7.89ms | 0.671× | 1.09% | PASS |
| file_reads | `covering_index_scan` | 13.23ms | 7.43ms | 0.562× | 2.09% | PASS |
| file_reads | `groupby_scan` | 33.23ms | 34.90ms | 1.050× | 1.15% | PASS |
| file_reads | `index_join` | 11.74ms | 11.29ms | 0.962× | 2.76% | PASS |
| file_reads | `index_join_scan` | 5.63ms | 5.93ms | 1.052× | 1.15% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.23s | 1.163× | 0.97% | PASS |
| file_reads | `table_scan` | 1.47s | 1.41s | 0.955× | 3.05% | PASS |
| file_reads | `oltp_read_only` | 244.48ms | 182.48ms | 0.746× | 1.01% | PASS |
| file_writes | `oltp_bulk_insert` | 256.28ms | 393.42ms | 1.535× | 0.92% | PASS |
| file_writes | `oltp_insert` | 60.77ms | 54.94ms | 0.904× | 17.87% | PASS |
| file_writes | `oltp_update_index` | 127.25ms | 182.59ms | 1.435× | 1.67% | PASS |
| file_writes | `oltp_update_non_index` | 107.69ms | 117.25ms | 1.089× | 15.00% | PASS |
| file_writes | `oltp_delete_insert` | 97.31ms | 141.09ms | 1.450× | 2.32% | PASS |
| file_writes | `oltp_write_only` | 83.48ms | 90.50ms | 1.084× | 17.22% | PASS |
| file_writes | `types_delete_insert` | 59.59ms | 79.43ms | 1.333× | 2.00% | PASS |
| file_writes | `oltp_read_write` | 165.39ms | 176.05ms | 1.064× | 4.26% | PASS |
| ac_reads | `oltp_point_select` | 57.45ms | 66.18ms | 1.152× | 0.98% | PASS |
| ac_reads | `oltp_range_select` | 18.48ms | 17.88ms | 0.968× | 1.56% | PASS |
| ac_reads | `oltp_sum_range` | 16.68ms | 17.97ms | 1.077× | 1.27% | PASS |
| ac_reads | `oltp_order_range` | 3.73ms | 3.73ms | 1.002× | 4.19% | PASS |
| ac_reads | `oltp_distinct_range` | 4.69ms | 4.84ms | 1.032× | 3.24% | PASS |
| ac_reads | `oltp_index_scan` | 7.88ms | 9.53ms | 1.210× | 1.38% | PASS |
| ac_reads | `select_random_points` | 23.09ms | 26.06ms | 1.128× | 1.84% | PASS |
| ac_reads | `select_random_ranges` | 6.99ms | 7.97ms | 1.141× | 1.06% | PASS |
| ac_reads | `covering_index_scan` | 8.81ms | 7.55ms | 0.857× | 1.19% | PASS |
| ac_reads | `groupby_scan` | 33.46ms | 35.20ms | 1.052× | 0.72% | PASS |
| ac_reads | `index_join` | 10.01ms | 11.92ms | 1.191× | 1.84% | PASS |
| ac_reads | `index_join_scan` | 5.44ms | 6.30ms | 1.158× | 2.71% | PASS |
| ac_reads | `types_table_scan` | 1.26s | 1.27s | 1.007× | 1.07% | PASS |
| ac_reads | `table_scan` | 1.33s | 1.39s | 1.048× | 4.50% | PASS |
| ac_reads | `oltp_read_only` | 162.26ms | 178.21ms | 1.098× | 2.18% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 25.57ms | 86.69ms | 3.390× | 9.24% | PASS |
| ac_writes | `oltp_insert_ac` | 26.88ms | 103.44ms | 3.849× | 6.86% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.68ms | 117.89ms | 3.971× | 7.33% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.24ms | 103.00ms | 4.432× | 5.84% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.58ms | 111.50ms | 4.196× | 7.24% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.56ms | 109.88ms | 4.138× | 6.56% | PASS |
| ac_writes | `types_delete_insert_ac` | 24.03ms | 101.66ms | 4.231× | 7.27% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.59ms | 120.04ms | 3.574× | 5.78% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.81ms | 27.42ms | 1.152× | 1.91% | PASS |
| mem_reads | `oltp_range_select` | 10.72ms | 10.75ms | 1.003× | 1.64% | PASS |
| mem_reads | `oltp_sum_range` | 9.44ms | 10.38ms | 1.099× | 1.09% | PASS |
| mem_reads | `oltp_order_range` | 2.55ms | 2.47ms | 0.971× | 1.43% | PASS |
| mem_reads | `oltp_distinct_range` | 3.40ms | 3.27ms | 0.964× | 0.89% | PASS |
| mem_reads | `oltp_index_scan` | 3.65ms | 4.58ms | 1.255× | 1.10% | PASS |
| mem_reads | `select_random_points` | 13.79ms | 15.88ms | 1.152× | 0.56% | PASS |
| mem_reads | `select_random_ranges` | 3.40ms | 4.16ms | 1.224× | 1.11% | PASS |
| mem_reads | `covering_index_scan` | 3.59ms | 3.52ms | 0.980× | 3.34% | PASS |
| mem_reads | `groupby_scan` | 26.38ms | 27.75ms | 1.052× | 1.19% | PASS |
| mem_reads | `index_join` | 5.64ms | 7.98ms | 1.413× | 3.62% | PASS |
| mem_reads | `index_join_scan` | 3.78ms | 4.91ms | 1.299× | 3.62% | PASS |
| mem_reads | `types_table_scan` | 885.84ms | 975.29ms | 1.101× | 1.33% | PASS |
| mem_reads | `table_scan` | 1.05s | 1.07s | 1.011× | 3.64% | PASS |
| mem_reads | `oltp_read_only` | 95.10ms | 100.04ms | 1.052× | 0.83% | PASS |
| mem_writes | `oltp_bulk_insert` | 183.81ms | 255.06ms | 1.388× | 0.80% | PASS |
| mem_writes | `oltp_insert` | 15.87ms | 29.43ms | 1.854× | 0.91% | PASS |
| mem_writes | `oltp_update_index` | 54.58ms | 100.81ms | 1.847× | 1.16% | PASS |
| mem_writes | `oltp_update_non_index` | 38.85ms | 65.36ms | 1.682× | 1.15% | PASS |
| mem_writes | `oltp_delete_insert` | 39.47ms | 80.06ms | 2.028× | 1.30% | PASS |
| mem_writes | `oltp_write_only` | 23.26ms | 48.48ms | 2.084× | 2.07% | PASS |
| mem_writes | `types_delete_insert` | 25.83ms | 40.28ms | 1.560× | 1.60% | PASS |
| mem_writes | `oltp_read_write` | 65.81ms | 104.89ms | 1.594× | 1.81% | PASS |
| file_reads | `oltp_point_select` | 98.35ms | 52.42ms | 0.533× | 1.03% | PASS |
| file_reads | `oltp_range_select` | 18.77ms | 13.34ms | 0.711× | 0.92% | PASS |
| file_reads | `oltp_sum_range` | 17.61ms | 12.99ms | 0.738× | 1.54% | PASS |
| file_reads | `oltp_order_range` | 3.45ms | 2.81ms | 0.814× | 1.01% | PASS |
| file_reads | `oltp_distinct_range` | 4.25ms | 3.59ms | 0.844× | 1.48% | PASS |
| file_reads | `oltp_index_scan` | 11.53ms | 7.48ms | 0.649× | 1.17% | PASS |
| file_reads | `select_random_points` | 21.90ms | 18.46ms | 0.843× | 1.29% | PASS |
| file_reads | `select_random_ranges` | 11.07ms | 6.72ms | 0.607× | 0.74% | PASS |
| file_reads | `covering_index_scan` | 12.21ms | 6.39ms | 0.523× | 1.16% | PASS |
| file_reads | `groupby_scan` | 27.56ms | 28.09ms | 1.019× | 0.92% | PASS |
| file_reads | `index_join` | 10.13ms | 9.21ms | 0.909× | 1.51% | PASS |
| file_reads | `index_join_scan` | 4.58ms | 5.05ms | 1.104× | 2.19% | PASS |
| file_reads | `types_table_scan` | 1.02s | 1.03s | 1.011× | 4.11% | PASS |
| file_reads | `table_scan` | 1.32s | 1.13s | 0.859× | 0.78% | PASS |
| file_reads | `oltp_read_only` | 214.08ms | 141.19ms | 0.660× | 0.86% | PASS |
| file_writes | `oltp_bulk_insert` | 270.60ms | 355.92ms | 1.315× | 18.54% | PASS |
| file_writes | `oltp_insert` | 51.49ms | 63.19ms | 1.227× | 15.47% | PASS |
| file_writes | `oltp_update_index` | 199.20ms | 221.72ms | 1.113× | 13.02% | PASS |
| file_writes | `oltp_update_non_index` | 162.19ms | 151.38ms | 0.933× | 14.36% | PASS |
| file_writes | `oltp_delete_insert` | 182.67ms | 183.72ms | 1.006× | 18.04% | PASS |
| file_writes | `oltp_write_only` | 121.71ms | 124.96ms | 1.027× | 25.06% | PASS |
| file_writes | `types_delete_insert` | 106.37ms | 100.71ms | 0.947× | 17.75% | PASS |
| file_writes | `oltp_read_write` | 183.60ms | 186.20ms | 1.014× | 18.56% | PASS |
| ac_reads | `oltp_point_select` | 48.69ms | 52.31ms | 1.074× | 0.89% | PASS |
| ac_reads | `oltp_range_select` | 14.04ms | 13.34ms | 0.951× | 1.03% | PASS |
| ac_reads | `oltp_sum_range` | 12.71ms | 13.06ms | 1.027× | 0.83% | PASS |
| ac_reads | `oltp_order_range` | 3.02ms | 2.82ms | 0.932× | 0.62% | PASS |
| ac_reads | `oltp_distinct_range` | 3.81ms | 3.58ms | 0.941× | 0.72% | PASS |
| ac_reads | `oltp_index_scan` | 6.76ms | 7.56ms | 1.119× | 0.95% | PASS |
| ac_reads | `select_random_points` | 18.12ms | 19.44ms | 1.073× | 1.55% | PASS |
| ac_reads | `select_random_ranges` | 6.26ms | 6.77ms | 1.081× | 1.05% | PASS |
| ac_reads | `covering_index_scan` | 7.16ms | 6.39ms | 0.893× | 0.86% | PASS |
| ac_reads | `groupby_scan` | 27.25ms | 28.19ms | 1.035× | 1.01% | PASS |
| ac_reads | `index_join` | 7.73ms | 9.32ms | 1.205× | 1.89% | PASS |
| ac_reads | `index_join_scan` | 4.21ms | 5.22ms | 1.240× | 1.71% | PASS |
| ac_reads | `types_table_scan` | 910.10ms | 990.71ms | 1.089× | 2.43% | PASS |
| ac_reads | `table_scan` | 1.01s | 1.05s | 1.041× | 1.05% | PASS |
| ac_reads | `oltp_read_only` | 131.47ms | 135.32ms | 1.029× | 1.17% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 48.12ms | 145.00ms | 3.014× | 47.03% | PASS |
| ac_writes | `oltp_insert_ac` | 39.35ms | 130.59ms | 3.318× | 41.16% | PASS |
| ac_writes | `oltp_update_index_ac` | 41.54ms | 209.81ms | 5.051× | 47.45% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 32.72ms | 110.99ms | 3.392× | 36.98% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 36.73ms | 130.29ms | 3.548× | 33.54% | PASS |
| ac_writes | `oltp_write_only_ac` | 35.62ms | 131.50ms | 3.692× | 49.67% | PASS |
| ac_writes | `types_delete_insert_ac` | 37.63ms | 142.09ms | 3.775× | 48.83% | PASS |
| ac_writes | `oltp_read_write_ac` | 55.13ms | 197.37ms | 3.580× | 38.91% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 27.03ms | 29.80ms | 1.103× | 2.40% | PASS |
| mem_reads | `oltp_range_select` | 16.13ms | 15.79ms | 0.979× | 2.60% | PASS |
| mem_reads | `oltp_sum_range` | 14.87ms | 15.07ms | 1.014× | 1.88% | PASS |
| mem_reads | `oltp_order_range` | 2.96ms | 2.98ms | 1.007× | 1.07% | PASS |
| mem_reads | `oltp_distinct_range` | 3.84ms | 3.80ms | 0.988× | 1.27% | PASS |
| mem_reads | `oltp_index_scan` | 3.78ms | 4.75ms | 1.257× | 2.11% | PASS |
| mem_reads | `select_random_points` | 22.89ms | 24.29ms | 1.061× | 2.56% | PASS |
| mem_reads | `select_random_ranges` | 6.09ms | 6.58ms | 1.080× | 1.94% | PASS |
| mem_reads | `covering_index_scan` | 3.40ms | 3.42ms | 1.006× | 2.74% | PASS |
| mem_reads | `groupby_scan` | 30.15ms | 31.00ms | 1.028× | 0.65% | PASS |
| mem_reads | `index_join` | 6.54ms | 7.91ms | 1.210× | 1.36% | PASS |
| mem_reads | `index_join_scan` | 3.45ms | 4.58ms | 1.329× | 1.39% | PASS |
| mem_reads | `types_table_scan` | 880.96ms | 975.59ms | 1.107× | 1.16% | PASS |
| mem_reads | `table_scan` | 1.20s | 1.11s | 0.922× | 3.54% | PASS |
| mem_reads | `oltp_read_only` | 131.17ms | 128.68ms | 0.981× | 1.74% | PASS |
| mem_writes | `oltp_bulk_insert` | 190.62ms | 261.88ms | 1.374× | 0.87% | PASS |
| mem_writes | `oltp_insert` | 15.55ms | 27.77ms | 1.786× | 1.15% | PASS |
| mem_writes | `oltp_update_index` | 60.83ms | 100.08ms | 1.645× | 2.76% | PASS |
| mem_writes | `oltp_update_non_index` | 45.31ms | 67.07ms | 1.480× | 1.58% | PASS |
| mem_writes | `oltp_delete_insert` | 43.29ms | 77.27ms | 1.785× | 2.38% | PASS |
| mem_writes | `oltp_write_only` | 24.12ms | 47.63ms | 1.975× | 2.26% | PASS |
| mem_writes | `types_delete_insert` | 28.21ms | 42.58ms | 1.509× | 1.18% | PASS |
| mem_writes | `oltp_read_write` | 78.37ms | 113.10ms | 1.443× | 1.67% | PASS |
| file_reads | `oltp_point_select` | 103.24ms | 55.59ms | 0.538× | 1.32% | PASS |
| file_reads | `oltp_range_select` | 23.82ms | 18.30ms | 0.769× | 2.47% | PASS |
| file_reads | `oltp_sum_range` | 23.41ms | 17.96ms | 0.767× | 1.20% | PASS |
| file_reads | `oltp_order_range` | 3.89ms | 3.27ms | 0.841× | 1.15% | PASS |
| file_reads | `oltp_distinct_range` | 4.68ms | 4.11ms | 0.878× | 0.99% | PASS |
| file_reads | `oltp_index_scan` | 11.56ms | 7.46ms | 0.645× | 1.12% | PASS |
| file_reads | `select_random_points` | 29.76ms | 26.61ms | 0.894× | 1.53% | PASS |
| file_reads | `select_random_ranges` | 13.74ms | 9.21ms | 0.670× | 1.30% | PASS |
| file_reads | `covering_index_scan` | 11.07ms | 6.20ms | 0.560× | 0.84% | PASS |
| file_reads | `groupby_scan` | 31.26ms | 31.46ms | 1.006× | 0.89% | PASS |
| file_reads | `index_join` | 10.65ms | 10.06ms | 0.945× | 1.39% | PASS |
| file_reads | `index_join_scan` | 4.34ms | 4.99ms | 1.149× | 1.89% | PASS |
| file_reads | `types_table_scan` | 868.35ms | 965.59ms | 1.112× | 0.63% | PASS |
| file_reads | `table_scan` | 1.01s | 1.06s | 1.056× | 0.81% | PASS |
| file_reads | `oltp_read_only` | 230.71ms | 160.83ms | 0.697× | 1.33% | PASS |
| file_writes | `oltp_bulk_insert` | 251.35ms | 330.81ms | 1.316× | 6.64% | PASS |
| file_writes | `oltp_insert` | 27.53ms | 47.37ms | 1.721× | 30.26% | PASS |
| file_writes | `oltp_update_index` | 172.18ms | 169.49ms | 0.984× | 11.40% | PASS |
| file_writes | `oltp_update_non_index` | 137.39ms | 118.98ms | 0.866× | 8.46% | PASS |
| file_writes | `oltp_delete_insert` | 139.40ms | 129.95ms | 0.932× | 9.86% | PASS |
| file_writes | `oltp_write_only` | 96.19ms | 103.61ms | 1.077× | 10.16% | PASS |
| file_writes | `types_delete_insert` | 84.46ms | 84.17ms | 0.997× | 10.10% | PASS |
| file_writes | `oltp_read_write` | 160.26ms | 170.79ms | 1.066× | 6.47% | PASS |
| ac_reads | `oltp_point_select` | 52.18ms | 55.72ms | 1.068× | 1.16% | PASS |
| ac_reads | `oltp_range_select` | 19.64ms | 18.49ms | 0.942× | 1.72% | PASS |
| ac_reads | `oltp_sum_range` | 17.83ms | 17.95ms | 1.007× | 1.45% | PASS |
| ac_reads | `oltp_order_range` | 3.43ms | 3.27ms | 0.953× | 0.73% | PASS |
| ac_reads | `oltp_distinct_range` | 4.24ms | 4.10ms | 0.967× | 0.70% | PASS |
| ac_reads | `oltp_index_scan` | 6.79ms | 7.59ms | 1.117× | 1.11% | PASS |
| ac_reads | `select_random_points` | 26.60ms | 27.43ms | 1.031× | 2.51% | PASS |
| ac_reads | `select_random_ranges` | 8.97ms | 9.23ms | 1.029× | 1.38% | PASS |
| ac_reads | `covering_index_scan` | 6.42ms | 6.28ms | 0.977× | 1.54% | PASS |
| ac_reads | `groupby_scan` | 30.98ms | 31.66ms | 1.022× | 0.83% | PASS |
| ac_reads | `index_join` | 8.39ms | 10.37ms | 1.235× | 0.99% | PASS |
| ac_reads | `index_join_scan` | 3.95ms | 5.12ms | 1.298× | 1.73% | PASS |
| ac_reads | `types_table_scan` | 923.63ms | 986.50ms | 1.068× | 3.95% | PASS |
| ac_reads | `table_scan` | 1.01s | 1.07s | 1.061× | 1.57% | PASS |
| ac_reads | `oltp_read_only` | 152.89ms | 158.85ms | 1.039× | 0.81% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 35.20ms | 123.26ms | 3.502× | 38.31% | PASS |
| ac_writes | `oltp_insert_ac` | 42.54ms | 143.20ms | 3.366× | 53.11% | PASS |
| ac_writes | `oltp_update_index_ac` | 38.73ms | 244.90ms | 6.324× | 59.36% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 36.21ms | 133.49ms | 3.686× | 46.33% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 33.95ms | 113.83ms | 3.352× | 28.33% | PASS |
| ac_writes | `oltp_write_only_ac` | 33.47ms | 104.71ms | 3.129× | 16.73% | PASS |
| ac_writes | `types_delete_insert_ac` | 36.18ms | 99.65ms | 2.754× | 24.63% | PASS |
| ac_writes | `oltp_read_write_ac` | 40.22ms | 126.84ms | 3.154× | 29.76% | PASS |

</details>

## Version-control latency

Wall time: 1m 52s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 53.68ms | 200.00ms | 26.8% | 0.88% | PASS |
| `status_dirty_many_tables` | 55.62ms | 200.00ms | 27.8% | 1.06% | PASS |
| `diff_regular_working_one_table` | 51.45ms | 150.00ms | 34.3% | 0.92% | PASS |
| `diff_regular_working_many_tables` | 58.01ms | 200.00ms | 29.0% | 1.03% | PASS |
| `diff_stat_working_many_tables` | 56.76ms | 200.00ms | 28.4% | 1.15% | PASS |
| `diff_schema_working_many_tables` | 57.62ms | 200.00ms | 28.8% | 2.15% | PASS |
| `branch_list_many_branches` | 15.95ms | 100.00ms | 16.0% | 3.61% | PASS |
| `branch_create_delete` | 21.31ms | 100.00ms | 21.3% | 20.05% | PASS |
| `checkout_branch_clean` | 97.22ms | 200.00ms | 48.6% | 21.58% | PASS |
| `merge_data_no_conflicts` | 38.59ms | 150.00ms | 25.7% | 33.11% | PASS |
| `merge_schema_no_conflicts` | 16.40ms | 100.00ms | 16.4% | 10.41% | PASS |
| `merge_data_conflicts` | 70.65ms | 250.00ms | 28.3% | 0.90% | PASS |
| `merge_data_conflicts_with_resolve` | 71.06ms | 250.00ms | 28.4% | 1.11% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
