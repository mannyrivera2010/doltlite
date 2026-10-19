# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-19 18:10 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261012.334.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/39286679490)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 8m 59s | 7.95s | 9.09s | 1.143× | 1.35% | **PASS** |
| textpk | 69 | 55 | 1h 28m 21s | 9.31s | 9.97s | 1.071× | 1.39% | **PASS** |
| blobpk | 69 | 55 | 1h 32m 20s | 10.46s | 12.13s | 1.160× | 2.09% | **PASS** |
| compositepk | 69 | 55 | 1h 1m 42s | 6.82s | 7.65s | 1.122× | 2.41% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.64ms | 22.38ms | 1.085× | 1.91% | PASS |
| mem_reads | `oltp_range_select` | 8.99ms | 9.61ms | 1.069× | 2.02% | PASS |
| mem_reads | `oltp_sum_range` | 8.15ms | 9.21ms | 1.129× | 1.85% | PASS |
| mem_reads | `oltp_order_range` | 2.18ms | 2.31ms | 1.058× | 1.39% | PASS |
| mem_reads | `oltp_distinct_range` | 3.01ms | 3.10ms | 1.031× | 0.77% | PASS |
| mem_reads | `oltp_index_scan` | 3.21ms | 4.04ms | 1.257× | 1.66% | PASS |
| mem_reads | `select_random_points` | 8.70ms | 9.25ms | 1.063× | 1.35% | PASS |
| mem_reads | `select_random_ranges` | 2.51ms | 3.17ms | 1.265× | 1.49% | PASS |
| mem_reads | `covering_index_scan` | 3.39ms | 3.19ms | 0.940× | 1.60% | PASS |
| mem_reads | `groupby_scan` | 24.97ms | 26.75ms | 1.071× | 0.61% | PASS |
| mem_reads | `index_join` | 4.74ms | 6.18ms | 1.305× | 1.62% | PASS |
| mem_reads | `index_join_scan` | 2.86ms | 3.87ms | 1.351× | 1.26% | PASS |
| mem_reads | `types_table_scan` | 869.33ms | 986.85ms | 1.135× | 0.86% | PASS |
| mem_reads | `table_scan` | 1.01s | 1.08s | 1.066× | 1.64% | PASS |
| mem_reads | `oltp_read_only` | 84.95ms | 91.41ms | 1.076× | 1.16% | PASS |
| mem_writes | `oltp_bulk_insert` | 139.63ms | 186.47ms | 1.335× | 0.83% | PASS |
| mem_writes | `oltp_insert` | 12.38ms | 21.70ms | 1.752× | 1.24% | PASS |
| mem_writes | `oltp_update_index` | 42.66ms | 85.38ms | 2.002× | 1.92% | PASS |
| mem_writes | `oltp_update_non_index` | 30.43ms | 47.52ms | 1.562× | 1.80% | PASS |
| mem_writes | `oltp_delete_insert` | 38.70ms | 63.79ms | 1.648× | 1.40% | PASS |
| mem_writes | `oltp_write_only` | 18.99ms | 36.32ms | 1.912× | 1.69% | PASS |
| mem_writes | `types_delete_insert` | 20.77ms | 31.23ms | 1.504× | 1.39% | PASS |
| mem_writes | `oltp_read_write` | 59.51ms | 86.29ms | 1.450× | 1.90% | PASS |
| file_reads | `oltp_point_select` | 95.84ms | 47.64ms | 0.497× | 0.89% | PASS |
| file_reads | `oltp_range_select` | 17.64ms | 12.22ms | 0.693× | 1.52% | PASS |
| file_reads | `oltp_sum_range` | 16.54ms | 11.99ms | 0.725× | 1.03% | PASS |
| file_reads | `oltp_order_range` | 3.06ms | 2.60ms | 0.850× | 0.86% | PASS |
| file_reads | `oltp_distinct_range` | 3.87ms | 3.36ms | 0.868× | 0.81% | PASS |
| file_reads | `oltp_index_scan` | 11.14ms | 6.95ms | 0.623× | 1.17% | PASS |
| file_reads | `select_random_points` | 17.00ms | 11.91ms | 0.701× | 1.35% | PASS |
| file_reads | `select_random_ranges` | 10.10ms | 5.69ms | 0.564× | 0.87% | PASS |
| file_reads | `covering_index_scan` | 11.27ms | 6.11ms | 0.542× | 0.71% | PASS |
| file_reads | `groupby_scan` | 25.95ms | 27.19ms | 1.048× | 0.69% | PASS |
| file_reads | `index_join` | 9.10ms | 8.18ms | 0.899× | 0.99% | PASS |
| file_reads | `index_join_scan` | 3.71ms | 4.17ms | 1.125× | 0.89% | PASS |
| file_reads | `types_table_scan` | 856.21ms | 980.41ms | 1.145× | 0.43% | PASS |
| file_reads | `table_scan` | 1.02s | 1.08s | 1.054× | 2.44% | PASS |
| file_reads | `oltp_read_only` | 189.97ms | 125.45ms | 0.660× | 0.54% | PASS |
| file_writes | `oltp_bulk_insert` | 186.19ms | 242.88ms | 1.305× | 4.78% | PASS |
| file_writes | `oltp_insert` | 27.80ms | 36.83ms | 1.325× | 5.76% | PASS |
| file_writes | `oltp_update_index` | 144.41ms | 150.93ms | 1.045× | 8.86% | PASS |
| file_writes | `oltp_update_non_index` | 120.73ms | 100.94ms | 0.836× | 13.26% | PASS |
| file_writes | `oltp_delete_insert` | 122.88ms | 122.91ms | 1.000× | 11.29% | PASS |
| file_writes | `oltp_write_only` | 100.79ms | 87.86ms | 0.872× | 14.25% | PASS |
| file_writes | `types_delete_insert` | 76.75ms | 74.27ms | 0.968× | 9.51% | PASS |
| file_writes | `oltp_read_write` | 169.81ms | 145.54ms | 0.857× | 25.66% | PASS |
| ac_reads | `oltp_point_select` | 44.61ms | 46.98ms | 1.053× | 1.17% | PASS |
| ac_reads | `oltp_range_select` | 11.61ms | 11.95ms | 1.030× | 1.23% | PASS |
| ac_reads | `oltp_sum_range` | 10.74ms | 11.70ms | 1.089× | 0.73% | PASS |
| ac_reads | `oltp_order_range` | 2.56ms | 2.57ms | 1.007× | 1.05% | PASS |
| ac_reads | `oltp_distinct_range` | 3.34ms | 3.36ms | 1.005× | 0.86% | PASS |
| ac_reads | `oltp_index_scan` | 6.06ms | 6.83ms | 1.127× | 0.77% | PASS |
| ac_reads | `select_random_points` | 11.84ms | 11.88ms | 1.004× | 0.93% | PASS |
| ac_reads | `select_random_ranges` | 5.16ms | 5.71ms | 1.106× | 0.81% | PASS |
| ac_reads | `covering_index_scan` | 6.31ms | 6.10ms | 0.966× | 0.79% | PASS |
| ac_reads | `groupby_scan` | 25.53ms | 27.24ms | 1.067× | 0.92% | PASS |
| ac_reads | `index_join` | 6.49ms | 8.10ms | 1.248× | 1.07% | PASS |
| ac_reads | `index_join_scan` | 3.27ms | 4.16ms | 1.273× | 0.91% | PASS |
| ac_reads | `types_table_scan` | 861.68ms | 985.37ms | 1.144× | 0.72% | PASS |
| ac_reads | `table_scan` | 1.00s | 1.08s | 1.077× | 1.86% | PASS |
| ac_reads | `oltp_read_only` | 119.23ms | 126.18ms | 1.058× | 1.14% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.96ms | 52.73ms | 2.936× | 16.93% | PASS |
| ac_writes | `oltp_insert_ac` | 21.29ms | 74.39ms | 3.495× | 22.35% | PASS |
| ac_writes | `oltp_update_index_ac` | 24.26ms | 96.41ms | 3.974× | 33.78% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 18.34ms | 73.82ms | 4.025× | 25.65% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 20.94ms | 95.62ms | 4.567× | 53.02% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.96ms | 81.40ms | 4.079× | 25.20% | PASS |
| ac_writes | `types_delete_insert_ac` | 19.52ms | 72.42ms | 3.709× | 41.27% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.93ms | 93.99ms | 3.624× | 22.43% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.02ms | 28.86ms | 1.154× | 1.72% | PASS |
| mem_reads | `oltp_range_select` | 12.32ms | 11.04ms | 0.896× | 3.25% | PASS |
| mem_reads | `oltp_sum_range` | 10.56ms | 10.78ms | 1.020× | 1.48% | PASS |
| mem_reads | `oltp_order_range` | 2.64ms | 2.54ms | 0.963× | 1.22% | PASS |
| mem_reads | `oltp_distinct_range` | 3.43ms | 3.32ms | 0.969× | 0.80% | PASS |
| mem_reads | `oltp_index_scan` | 3.85ms | 5.00ms | 1.298× | 1.68% | PASS |
| mem_reads | `select_random_points` | 15.18ms | 16.68ms | 1.099× | 1.83% | PASS |
| mem_reads | `select_random_ranges` | 3.49ms | 4.23ms | 1.211× | 1.41% | PASS |
| mem_reads | `covering_index_scan` | 3.99ms | 3.76ms | 0.942× | 2.83% | PASS |
| mem_reads | `groupby_scan` | 27.85ms | 27.86ms | 1.001× | 1.07% | PASS |
| mem_reads | `index_join` | 5.86ms | 7.71ms | 1.315× | 3.07% | PASS |
| mem_reads | `index_join_scan` | 4.19ms | 4.78ms | 1.140× | 4.55% | PASS |
| mem_reads | `types_table_scan` | 920.83ms | 983.70ms | 1.068× | 1.92% | PASS |
| mem_reads | `table_scan` | 1.05s | 1.06s | 1.008× | 0.39% | PASS |
| mem_reads | `oltp_read_only` | 98.31ms | 102.62ms | 1.044× | 2.70% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.17ms | 259.95ms | 1.435× | 0.68% | PASS |
| mem_writes | `oltp_insert` | 17.71ms | 29.88ms | 1.687× | 1.36% | PASS |
| mem_writes | `oltp_update_index` | 59.44ms | 105.27ms | 1.771× | 1.12% | PASS |
| mem_writes | `oltp_update_non_index` | 42.21ms | 69.58ms | 1.648× | 1.84% | PASS |
| mem_writes | `oltp_delete_insert` | 44.46ms | 84.17ms | 1.893× | 1.75% | PASS |
| mem_writes | `oltp_write_only` | 25.42ms | 49.75ms | 1.957× | 2.06% | PASS |
| mem_writes | `types_delete_insert` | 26.60ms | 41.02ms | 1.542× | 1.46% | PASS |
| mem_writes | `oltp_read_write` | 67.52ms | 103.77ms | 1.537× | 0.82% | PASS |
| file_reads | `oltp_point_select` | 98.93ms | 52.47ms | 0.530× | 0.55% | PASS |
| file_reads | `oltp_range_select` | 19.55ms | 13.39ms | 0.685× | 0.91% | PASS |
| file_reads | `oltp_sum_range` | 18.25ms | 13.15ms | 0.721× | 0.66% | PASS |
| file_reads | `oltp_order_range` | 3.45ms | 2.81ms | 0.815× | 0.86% | PASS |
| file_reads | `oltp_distinct_range` | 4.24ms | 3.58ms | 0.846× | 0.75% | PASS |
| file_reads | `oltp_index_scan` | 11.76ms | 7.54ms | 0.642× | 0.84% | PASS |
| file_reads | `select_random_points` | 22.82ms | 18.81ms | 0.824× | 0.85% | PASS |
| file_reads | `select_random_ranges` | 11.07ms | 6.74ms | 0.609× | 0.65% | PASS |
| file_reads | `covering_index_scan` | 12.59ms | 6.39ms | 0.508× | 0.69% | PASS |
| file_reads | `groupby_scan` | 28.29ms | 28.04ms | 0.991× | 0.53% | PASS |
| file_reads | `index_join` | 10.72ms | 9.31ms | 0.868× | 1.20% | PASS |
| file_reads | `index_join_scan` | 5.01ms | 5.14ms | 1.025× | 1.64% | PASS |
| file_reads | `types_table_scan` | 980.43ms | 1.05s | 1.073× | 2.80% | PASS |
| file_reads | `table_scan` | 1.31s | 1.16s | 0.886× | 3.96% | PASS |
| file_reads | `oltp_read_only` | 203.46ms | 136.23ms | 0.670× | 0.60% | PASS |
| file_writes | `oltp_bulk_insert` | 255.70ms | 354.70ms | 1.387× | 5.64% | PASS |
| file_writes | `oltp_insert` | 76.45ms | 62.21ms | 0.814× | 8.72% | PASS |
| file_writes | `oltp_update_index` | 226.35ms | 208.06ms | 0.919× | 4.26% | PASS |
| file_writes | `oltp_update_non_index` | 167.33ms | 145.24ms | 0.868× | 5.77% | PASS |
| file_writes | `oltp_delete_insert` | 179.78ms | 165.72ms | 0.922× | 3.34% | PASS |
| file_writes | `oltp_write_only` | 145.40ms | 116.05ms | 0.798× | 12.56% | PASS |
| file_writes | `types_delete_insert` | 108.44ms | 107.05ms | 0.987× | 14.93% | PASS |
| file_writes | `oltp_read_write` | 185.18ms | 173.89ms | 0.939× | 5.61% | PASS |
| ac_reads | `oltp_point_select` | 48.82ms | 52.71ms | 1.080× | 0.83% | PASS |
| ac_reads | `oltp_range_select` | 14.83ms | 13.36ms | 0.901× | 0.70% | PASS |
| ac_reads | `oltp_sum_range` | 13.36ms | 13.19ms | 0.988× | 0.80% | PASS |
| ac_reads | `oltp_order_range` | 3.06ms | 2.83ms | 0.922× | 0.96% | PASS |
| ac_reads | `oltp_distinct_range` | 3.81ms | 3.59ms | 0.941× | 0.74% | PASS |
| ac_reads | `oltp_index_scan` | 6.81ms | 7.58ms | 1.114× | 0.85% | PASS |
| ac_reads | `select_random_points` | 17.90ms | 18.80ms | 1.050× | 0.84% | PASS |
| ac_reads | `select_random_ranges` | 6.22ms | 6.75ms | 1.086× | 0.93% | PASS |
| ac_reads | `covering_index_scan` | 7.66ms | 6.40ms | 0.836× | 0.83% | PASS |
| ac_reads | `groupby_scan` | 27.73ms | 28.03ms | 1.011× | 0.50% | PASS |
| ac_reads | `index_join` | 8.12ms | 9.16ms | 1.128× | 0.86% | PASS |
| ac_reads | `index_join_scan` | 4.49ms | 5.05ms | 1.123× | 0.76% | PASS |
| ac_reads | `types_table_scan` | 913.91ms | 1.03s | 1.129× | 1.33% | PASS |
| ac_reads | `table_scan` | 1.19s | 1.14s | 0.958× | 1.08% | PASS |
| ac_reads | `oltp_read_only` | 135.77ms | 137.57ms | 1.013× | 1.39% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.05ms | 55.62ms | 3.263× | 13.29% | PASS |
| ac_writes | `oltp_insert_ac` | 20.23ms | 68.14ms | 3.368× | 13.42% | PASS |
| ac_writes | `oltp_update_index_ac` | 22.16ms | 85.12ms | 3.842× | 10.89% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 19.18ms | 81.41ms | 4.244× | 36.40% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 20.99ms | 88.69ms | 4.225× | 38.84% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.13ms | 95.15ms | 3.787× | 43.95% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.28ms | 67.43ms | 3.901× | 12.53% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.50ms | 87.34ms | 3.177× | 40.43% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.96ms | 37.56ms | 1.213× | 2.29% | PASS |
| mem_reads | `oltp_range_select` | 13.12ms | 14.16ms | 1.080× | 3.09% | PASS |
| mem_reads | `oltp_sum_range` | 12.26ms | 14.11ms | 1.151× | 2.48% | PASS |
| mem_reads | `oltp_order_range` | 2.99ms | 3.14ms | 1.052× | 2.09% | PASS |
| mem_reads | `oltp_distinct_range` | 4.07ms | 4.28ms | 1.051× | 1.38% | PASS |
| mem_reads | `oltp_index_scan` | 4.65ms | 6.44ms | 1.386× | 1.68% | PASS |
| mem_reads | `select_random_points` | 18.69ms | 21.44ms | 1.147× | 2.64% | PASS |
| mem_reads | `select_random_ranges` | 4.12ms | 5.22ms | 1.265× | 2.78% | PASS |
| mem_reads | `covering_index_scan` | 4.50ms | 4.71ms | 1.046× | 2.48% | PASS |
| mem_reads | `groupby_scan` | 32.18ms | 34.47ms | 1.071× | 0.85% | PASS |
| mem_reads | `index_join` | 7.00ms | 9.79ms | 1.399× | 2.71% | PASS |
| mem_reads | `index_join_scan` | 4.27ms | 5.39ms | 1.263× | 2.17% | PASS |
| mem_reads | `types_table_scan` | 1.14s | 1.27s | 1.109× | 3.90% | PASS |
| mem_reads | `table_scan` | 1.35s | 1.41s | 1.043× | 3.61% | PASS |
| mem_reads | `oltp_read_only` | 124.74ms | 138.91ms | 1.114× | 2.17% | PASS |
| mem_writes | `oltp_bulk_insert` | 236.78ms | 354.05ms | 1.495× | 1.00% | PASS |
| mem_writes | `oltp_insert` | 20.30ms | 39.96ms | 1.969× | 1.99% | PASS |
| mem_writes | `oltp_update_index` | 71.28ms | 135.42ms | 1.900× | 3.21% | PASS |
| mem_writes | `oltp_update_non_index` | 50.05ms | 87.44ms | 1.747× | 2.26% | PASS |
| mem_writes | `oltp_delete_insert` | 51.15ms | 106.61ms | 2.084× | 2.74% | PASS |
| mem_writes | `oltp_write_only` | 29.43ms | 63.47ms | 2.157× | 3.02% | PASS |
| mem_writes | `types_delete_insert` | 33.30ms | 55.45ms | 1.665× | 1.62% | PASS |
| mem_writes | `oltp_read_write` | 89.69ms | 144.00ms | 1.605× | 2.88% | PASS |
| file_reads | `oltp_point_select` | 106.91ms | 63.81ms | 0.597× | 1.32% | PASS |
| file_reads | `oltp_range_select` | 21.92ms | 16.98ms | 0.775× | 2.50% | PASS |
| file_reads | `oltp_sum_range` | 20.79ms | 16.97ms | 0.816× | 1.47% | PASS |
| file_reads | `oltp_order_range` | 3.95ms | 3.54ms | 0.896× | 1.81% | PASS |
| file_reads | `oltp_distinct_range` | 4.98ms | 4.63ms | 0.930× | 1.44% | PASS |
| file_reads | `oltp_index_scan` | 12.65ms | 9.18ms | 0.726× | 1.51% | PASS |
| file_reads | `select_random_points` | 27.36ms | 25.25ms | 0.923× | 2.73% | PASS |
| file_reads | `select_random_ranges` | 11.77ms | 7.82ms | 0.665× | 1.32% | PASS |
| file_reads | `covering_index_scan` | 12.72ms | 7.30ms | 0.573× | 2.63% | PASS |
| file_reads | `groupby_scan` | 32.94ms | 34.85ms | 1.058× | 1.19% | PASS |
| file_reads | `index_join` | 11.45ms | 11.23ms | 0.981× | 1.74% | PASS |
| file_reads | `index_join_scan` | 5.41ms | 5.93ms | 1.096× | 2.62% | PASS |
| file_reads | `types_table_scan` | 1.19s | 1.27s | 1.067× | 1.73% | PASS |
| file_reads | `table_scan` | 1.46s | 1.44s | 0.988× | 0.45% | PASS |
| file_reads | `oltp_read_only` | 247.95ms | 186.44ms | 0.752× | 0.93% | PASS |
| file_writes | `oltp_bulk_insert` | 255.82ms | 375.67ms | 1.468× | 0.97% | PASS |
| file_writes | `oltp_insert` | 37.50ms | 52.06ms | 1.388× | 2.09% | PASS |
| file_writes | `oltp_update_index` | 102.44ms | 160.39ms | 1.566× | 1.50% | PASS |
| file_writes | `oltp_update_non_index` | 83.86ms | 107.06ms | 1.277× | 1.13% | PASS |
| file_writes | `oltp_delete_insert` | 85.41ms | 128.64ms | 1.506× | 2.04% | PASS |
| file_writes | `oltp_write_only` | 63.98ms | 84.21ms | 1.316× | 1.56% | PASS |
| file_writes | `types_delete_insert` | 55.36ms | 72.74ms | 1.314× | 1.93% | PASS |
| file_writes | `oltp_read_write` | 118.73ms | 158.16ms | 1.332× | 1.34% | PASS |
| ac_reads | `oltp_point_select` | 54.09ms | 62.04ms | 1.147× | 1.03% | PASS |
| ac_reads | `oltp_range_select` | 15.42ms | 16.60ms | 1.077× | 0.92% | PASS |
| ac_reads | `oltp_sum_range` | 14.48ms | 16.60ms | 1.146× | 0.67% | PASS |
| ac_reads | `oltp_order_range` | 3.43ms | 3.57ms | 1.039× | 3.02% | PASS |
| ac_reads | `oltp_distinct_range` | 4.55ms | 4.72ms | 1.038× | 1.41% | PASS |
| ac_reads | `oltp_index_scan` | 7.52ms | 9.06ms | 1.204× | 1.27% | PASS |
| ac_reads | `select_random_points` | 21.49ms | 24.34ms | 1.133× | 2.39% | PASS |
| ac_reads | `select_random_ranges` | 6.78ms | 7.77ms | 1.145× | 1.42% | PASS |
| ac_reads | `covering_index_scan` | 7.75ms | 7.29ms | 0.940× | 1.29% | PASS |
| ac_reads | `groupby_scan` | 32.26ms | 34.87ms | 1.081× | 0.71% | PASS |
| ac_reads | `index_join` | 8.95ms | 11.22ms | 1.253× | 1.71% | PASS |
| ac_reads | `index_join_scan` | 4.75ms | 5.78ms | 1.216× | 2.16% | PASS |
| ac_reads | `types_table_scan` | 1.14s | 1.25s | 1.099× | 3.04% | PASS |
| ac_reads | `table_scan` | 1.36s | 1.40s | 1.035× | 2.58% | PASS |
| ac_reads | `oltp_read_only` | 175.75ms | 188.20ms | 1.071× | 0.98% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 34.70ms | 92.49ms | 2.665× | 9.68% | PASS |
| ac_writes | `oltp_insert_ac` | 35.05ms | 111.22ms | 3.173× | 9.43% | PASS |
| ac_writes | `oltp_update_index_ac` | 37.49ms | 120.57ms | 3.217× | 7.99% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 32.32ms | 97.25ms | 3.009× | 9.68% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 35.83ms | 117.16ms | 3.270× | 8.53% | PASS |
| ac_writes | `oltp_write_only_ac` | 35.74ms | 112.08ms | 3.136× | 9.86% | PASS |
| ac_writes | `types_delete_insert_ac` | 34.18ms | 104.61ms | 3.060× | 10.71% | PASS |
| ac_writes | `oltp_read_write_ac` | 42.49ms | 120.53ms | 2.837× | 7.26% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.10ms | 25.43ms | 1.151× | 2.30% | PASS |
| mem_reads | `oltp_range_select` | 11.75ms | 11.84ms | 1.007× | 2.12% | PASS |
| mem_reads | `oltp_sum_range` | 10.81ms | 11.75ms | 1.087× | 2.19% | PASS |
| mem_reads | `oltp_order_range` | 2.16ms | 2.20ms | 1.018× | 2.23% | PASS |
| mem_reads | `oltp_distinct_range` | 2.72ms | 2.73ms | 1.005× | 2.72% | PASS |
| mem_reads | `oltp_index_scan` | 2.75ms | 3.62ms | 1.313× | 3.30% | PASS |
| mem_reads | `select_random_points` | 16.93ms | 19.99ms | 1.181× | 3.55% | PASS |
| mem_reads | `select_random_ranges` | 4.65ms | 5.24ms | 1.127× | 1.79% | PASS |
| mem_reads | `covering_index_scan` | 2.33ms | 2.67ms | 1.148× | 4.29% | PASS |
| mem_reads | `groupby_scan` | 21.01ms | 21.55ms | 1.026× | 2.01% | PASS |
| mem_reads | `index_join` | 4.89ms | 6.32ms | 1.293× | 8.25% | PASS |
| mem_reads | `index_join_scan` | 2.78ms | 4.12ms | 1.483× | 5.31% | PASS |
| mem_reads | `types_table_scan` | 696.03ms | 748.35ms | 1.075× | 2.51% | PASS |
| mem_reads | `table_scan` | 822.55ms | 838.36ms | 1.019× | 3.47% | PASS |
| mem_reads | `oltp_read_only` | 81.59ms | 86.80ms | 1.064× | 2.30% | PASS |
| mem_writes | `oltp_bulk_insert` | 142.70ms | 200.19ms | 1.403× | 2.14% | PASS |
| mem_writes | `oltp_insert` | 11.07ms | 22.11ms | 1.998× | 2.35% | PASS |
| mem_writes | `oltp_update_index` | 41.98ms | 77.39ms | 1.844× | 2.41% | PASS |
| mem_writes | `oltp_update_non_index` | 32.70ms | 55.66ms | 1.702× | 1.76% | PASS |
| mem_writes | `oltp_delete_insert` | 30.92ms | 61.91ms | 2.002× | 2.20% | PASS |
| mem_writes | `oltp_write_only` | 17.05ms | 38.90ms | 2.282× | 2.39% | PASS |
| mem_writes | `types_delete_insert` | 20.61ms | 35.19ms | 1.707× | 2.83% | PASS |
| mem_writes | `oltp_read_write` | 58.50ms | 90.98ms | 1.555× | 3.18% | PASS |
| file_reads | `oltp_point_select` | 80.00ms | 44.96ms | 0.562× | 1.48% | PASS |
| file_reads | `oltp_range_select` | 17.99ms | 13.75ms | 0.764× | 2.25% | PASS |
| file_reads | `oltp_sum_range` | 16.83ms | 13.54ms | 0.805× | 1.77% | PASS |
| file_reads | `oltp_order_range` | 3.02ms | 2.53ms | 0.838× | 2.46% | PASS |
| file_reads | `oltp_distinct_range` | 3.52ms | 2.99ms | 0.850× | 1.90% | PASS |
| file_reads | `oltp_index_scan` | 9.13ms | 6.32ms | 0.693× | 2.03% | PASS |
| file_reads | `select_random_points` | 23.10ms | 21.12ms | 0.914× | 3.30% | PASS |
| file_reads | `select_random_ranges` | 10.27ms | 7.16ms | 0.697× | 1.56% | PASS |
| file_reads | `covering_index_scan` | 8.47ms | 4.96ms | 0.586× | 2.01% | PASS |
| file_reads | `groupby_scan` | 21.01ms | 21.28ms | 1.013× | 1.79% | PASS |
| file_reads | `index_join` | 8.23ms | 8.04ms | 0.978× | 2.42% | PASS |
| file_reads | `index_join_scan` | 3.46ms | 4.25ms | 1.229× | 1.95% | PASS |
| file_reads | `types_table_scan` | 716.76ms | 744.90ms | 1.039× | 3.47% | PASS |
| file_reads | `table_scan` | 815.98ms | 832.10ms | 1.020× | 4.57% | PASS |
| file_reads | `oltp_read_only` | 166.31ms | 116.38ms | 0.700× | 2.23% | PASS |
| file_writes | `oltp_bulk_insert` | 195.68ms | 258.87ms | 1.323× | 1.98% | PASS |
| file_writes | `oltp_insert` | 19.52ms | 38.64ms | 1.979× | 15.50% | PASS |
| file_writes | `oltp_update_index` | 140.68ms | 139.92ms | 0.995× | 2.29% | PASS |
| file_writes | `oltp_update_non_index` | 120.97ms | 109.01ms | 0.901× | 2.91% | PASS |
| file_writes | `oltp_delete_insert` | 120.63ms | 119.27ms | 0.989× | 1.79% | PASS |
| file_writes | `oltp_write_only` | 84.32ms | 90.31ms | 1.071× | 2.42% | PASS |
| file_writes | `types_delete_insert` | 77.31ms | 77.28ms | 0.999× | 4.58% | PASS |
| file_writes | `oltp_read_write` | 130.28ms | 147.43ms | 1.132× | 5.60% | PASS |
| ac_reads | `oltp_point_select` | 40.71ms | 44.94ms | 1.104× | 1.19% | PASS |
| ac_reads | `oltp_range_select` | 14.00ms | 13.77ms | 0.983× | 2.24% | PASS |
| ac_reads | `oltp_sum_range` | 12.37ms | 13.26ms | 1.072× | 2.51% | PASS |
| ac_reads | `oltp_order_range` | 2.56ms | 2.49ms | 0.973× | 2.32% | PASS |
| ac_reads | `oltp_distinct_range` | 3.13ms | 3.01ms | 0.962× | 2.74% | PASS |
| ac_reads | `oltp_index_scan` | 5.05ms | 6.23ms | 1.233× | 2.75% | PASS |
| ac_reads | `select_random_points` | 19.83ms | 21.66ms | 1.093× | 2.41% | PASS |
| ac_reads | `select_random_ranges` | 6.47ms | 7.05ms | 1.090× | 2.23% | PASS |
| ac_reads | `covering_index_scan` | 4.81ms | 5.00ms | 1.038× | 3.27% | PASS |
| ac_reads | `groupby_scan` | 20.95ms | 21.52ms | 1.027× | 1.80% | PASS |
| ac_reads | `index_join` | 6.45ms | 8.20ms | 1.272× | 3.18% | PASS |
| ac_reads | `index_join_scan` | 3.24ms | 4.34ms | 1.342× | 2.72% | PASS |
| ac_reads | `types_table_scan` | 683.45ms | 736.35ms | 1.077× | 1.17% | PASS |
| ac_reads | `table_scan` | 821.19ms | 856.51ms | 1.043× | 1.96% | PASS |
| ac_reads | `oltp_read_only` | 114.11ms | 118.88ms | 1.042× | 2.22% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.53ms | 58.24ms | 2.585× | 7.40% | PASS |
| ac_writes | `oltp_insert_ac` | 26.39ms | 74.78ms | 2.833× | 5.93% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.95ms | 85.69ms | 3.066× | 6.42% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.95ms | 67.17ms | 3.060× | 5.29% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.41ms | 74.27ms | 2.922× | 3.94% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.36ms | 75.01ms | 2.958× | 3.76% | PASS |
| ac_writes | `types_delete_insert_ac` | 25.90ms | 65.24ms | 2.519× | 8.92% | PASS |
| ac_writes | `oltp_read_write_ac` | 28.59ms | 84.40ms | 2.953× | 3.70% | PASS |

</details>

## Version-control latency

Wall time: 2m 15s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 67.06ms | 200.00ms | 33.5% | 0.32% | PASS |
| `status_dirty_many_tables` | 69.46ms | 200.00ms | 34.7% | 0.64% | PASS |
| `diff_regular_working_one_table` | 63.11ms | 150.00ms | 42.1% | 0.48% | PASS |
| `diff_regular_working_many_tables` | 73.45ms | 200.00ms | 36.7% | 0.43% | PASS |
| `diff_stat_working_many_tables` | 73.89ms | 200.00ms | 36.9% | 0.36% | PASS |
| `diff_schema_working_many_tables` | 74.58ms | 200.00ms | 37.3% | 0.30% | PASS |
| `branch_list_many_branches` | 20.60ms | 100.00ms | 20.6% | 0.62% | PASS |
| `branch_create_delete` | 27.41ms | 100.00ms | 27.4% | 21.01% | PASS |
| `checkout_branch_clean` | 92.17ms | 200.00ms | 46.1% | 10.26% | PASS |
| `merge_data_no_conflicts` | 31.64ms | 150.00ms | 21.1% | 10.51% | PASS |
| `merge_schema_no_conflicts` | 20.28ms | 100.00ms | 20.3% | 3.80% | PASS |
| `merge_data_conflicts` | 101.18ms | 250.00ms | 40.5% | 0.44% | PASS |
| `merge_data_conflicts_with_resolve` | 100.43ms | 250.00ms | 40.2% | 0.23% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
