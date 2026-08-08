# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-08 11:40 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260720.247.2
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/31251954743)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 11m 51s | 8.86s | 11.54s | 1.302× | 1.17% | **PASS** |
| textpk | 69 | 55 | 1h 33m 16s | 9.97s | 11.99s | 1.202× | 1.56% | **PASS** |
| blobpk | 69 | 55 | 1h 32m 10s | 10.01s | 11.89s | 1.188× | 1.58% | **PASS** |
| compositepk | 69 | 55 | 1h 25m 54s | 10.16s | 11.79s | 1.161× | 0.96% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.80ms | 29.15ms | 1.225× | 1.17% | PASS |
| mem_reads | `oltp_range_select` | 10.03ms | 13.16ms | 1.312× | 0.98% | PASS |
| mem_reads | `oltp_sum_range` | 9.55ms | 12.32ms | 1.290× | 0.95% | PASS |
| mem_reads | `oltp_order_range` | 2.54ms | 2.99ms | 1.178× | 0.86% | PASS |
| mem_reads | `oltp_distinct_range` | 3.58ms | 4.02ms | 1.123× | 1.34% | PASS |
| mem_reads | `oltp_index_scan` | 3.90ms | 5.28ms | 1.353× | 1.47% | PASS |
| mem_reads | `select_random_points` | 9.91ms | 11.13ms | 1.124× | 1.58% | PASS |
| mem_reads | `select_random_ranges` | 2.96ms | 3.94ms | 1.330× | 1.46% | PASS |
| mem_reads | `covering_index_scan` | 4.19ms | 4.17ms | 0.996× | 1.39% | PASS |
| mem_reads | `groupby_scan` | 29.65ms | 33.11ms | 1.117× | 0.90% | PASS |
| mem_reads | `index_join` | 5.99ms | 8.03ms | 1.340× | 1.47% | PASS |
| mem_reads | `index_join_scan` | 3.43ms | 4.51ms | 1.316× | 1.88% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.34s | 1.287× | 0.51% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.40s | 1.191× | 0.54% | PASS |
| mem_reads | `oltp_read_only` | 101.78ms | 123.11ms | 1.210× | 1.05% | PASS |
| mem_writes | `oltp_bulk_insert` | 176.40ms | 247.96ms | 1.406× | 1.19% | PASS |
| mem_writes | `oltp_insert` | 15.29ms | 28.18ms | 1.843× | 0.97% | PASS |
| mem_writes | `oltp_update_index` | 49.51ms | 104.25ms | 2.106× | 0.90% | PASS |
| mem_writes | `oltp_update_non_index` | 33.50ms | 59.59ms | 1.779× | 1.64% | PASS |
| mem_writes | `oltp_delete_insert` | 44.35ms | 79.37ms | 1.790× | 0.93% | PASS |
| mem_writes | `oltp_write_only` | 21.37ms | 44.66ms | 2.090× | 1.29% | PASS |
| mem_writes | `types_delete_insert` | 23.96ms | 40.09ms | 1.674× | 0.93% | PASS |
| mem_writes | `oltp_read_write` | 66.28ms | 110.02ms | 1.660× | 1.06% | PASS |
| file_reads | `oltp_point_select` | 96.37ms | 54.18ms | 0.562× | 0.94% | PASS |
| file_reads | `oltp_range_select` | 17.68ms | 15.76ms | 0.891× | 0.69% | PASS |
| file_reads | `oltp_sum_range` | 17.21ms | 15.11ms | 0.878× | 1.16% | PASS |
| file_reads | `oltp_order_range` | 3.43ms | 3.33ms | 0.971× | 1.10% | PASS |
| file_reads | `oltp_distinct_range` | 4.44ms | 4.35ms | 0.980× | 1.48% | PASS |
| file_reads | `oltp_index_scan` | 11.56ms | 8.14ms | 0.705× | 0.87% | PASS |
| file_reads | `select_random_points` | 17.81ms | 13.81ms | 0.776× | 1.07% | PASS |
| file_reads | `select_random_ranges` | 10.33ms | 6.50ms | 0.629× | 0.76% | PASS |
| file_reads | `covering_index_scan` | 11.88ms | 6.98ms | 0.587× | 1.02% | PASS |
| file_reads | `groupby_scan` | 30.57ms | 33.45ms | 1.094× | 0.99% | PASS |
| file_reads | `index_join` | 10.28ms | 10.11ms | 0.983× | 1.17% | PASS |
| file_reads | `index_join_scan` | 4.44ms | 4.93ms | 1.110× | 1.81% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.33s | 1.284× | 0.60% | PASS |
| file_reads | `table_scan` | 1.18s | 1.40s | 1.192× | 0.87% | PASS |
| file_reads | `oltp_read_only` | 210.74ms | 160.86ms | 0.763× | 0.69% | PASS |
| file_writes | `oltp_bulk_insert` | 189.72ms | 266.10ms | 1.403× | 0.96% | PASS |
| file_writes | `oltp_insert` | 24.30ms | 35.20ms | 1.449× | 2.18% | PASS |
| file_writes | `oltp_update_index` | 77.36ms | 127.44ms | 1.647× | 0.99% | PASS |
| file_writes | `oltp_update_non_index` | 60.35ms | 80.66ms | 1.336× | 1.82% | PASS |
| file_writes | `oltp_delete_insert` | 70.67ms | 98.78ms | 1.398× | 1.47% | PASS |
| file_writes | `oltp_write_only` | 48.83ms | 63.58ms | 1.302× | 1.49% | PASS |
| file_writes | `types_delete_insert` | 40.81ms | 53.13ms | 1.302× | 1.42% | PASS |
| file_writes | `oltp_read_write` | 96.50ms | 129.55ms | 1.343× | 1.37% | PASS |
| ac_reads | `oltp_point_select` | 47.80ms | 54.24ms | 1.135× | 0.93% | PASS |
| ac_reads | `oltp_range_select` | 12.90ms | 15.77ms | 1.222× | 1.22% | PASS |
| ac_reads | `oltp_sum_range` | 12.24ms | 15.11ms | 1.234× | 1.03% | PASS |
| ac_reads | `oltp_order_range` | 2.92ms | 3.31ms | 1.137× | 1.43% | PASS |
| ac_reads | `oltp_distinct_range` | 3.90ms | 4.35ms | 1.115× | 1.15% | PASS |
| ac_reads | `oltp_index_scan` | 6.57ms | 8.14ms | 1.240× | 1.26% | PASS |
| ac_reads | `select_random_points` | 12.92ms | 13.86ms | 1.073× | 1.44% | PASS |
| ac_reads | `select_random_ranges` | 5.43ms | 6.48ms | 1.194× | 1.22% | PASS |
| ac_reads | `covering_index_scan` | 6.95ms | 6.98ms | 1.004× | 1.30% | PASS |
| ac_reads | `groupby_scan` | 29.88ms | 33.58ms | 1.124× | 0.84% | PASS |
| ac_reads | `index_join` | 7.60ms | 10.13ms | 1.333× | 1.34% | PASS |
| ac_reads | `index_join_scan` | 3.89ms | 4.92ms | 1.265× | 1.39% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.33s | 1.280× | 0.58% | PASS |
| ac_reads | `table_scan` | 1.19s | 1.41s | 1.184× | 0.74% | PASS |
| ac_reads | `oltp_read_only` | 139.06ms | 161.01ms | 1.158× | 0.99% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.85ms | 84.61ms | 3.547× | 6.53% | PASS |
| ac_writes | `oltp_insert_ac` | 26.94ms | 102.54ms | 3.806× | 8.02% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.55ms | 117.97ms | 4.131× | 6.03% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.27ms | 95.26ms | 4.093× | 7.02% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.92ms | 106.79ms | 4.286× | 7.25% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.81ms | 105.17ms | 4.075× | 6.26% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.88ms | 97.52ms | 4.084× | 8.91% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.97ms | 113.07ms | 3.650× | 7.54% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.70ms | 37.64ms | 1.267× | 1.30% | PASS |
| mem_reads | `oltp_range_select` | 13.08ms | 14.14ms | 1.081× | 1.30% | PASS |
| mem_reads | `oltp_sum_range` | 11.90ms | 14.12ms | 1.187× | 1.56% | PASS |
| mem_reads | `oltp_order_range` | 3.03ms | 3.18ms | 1.049× | 1.60% | PASS |
| mem_reads | `oltp_distinct_range` | 4.07ms | 4.25ms | 1.045× | 1.04% | PASS |
| mem_reads | `oltp_index_scan` | 4.73ms | 6.53ms | 1.381× | 2.46% | PASS |
| mem_reads | `select_random_points` | 18.05ms | 21.31ms | 1.181× | 2.19% | PASS |
| mem_reads | `select_random_ranges` | 4.04ms | 5.19ms | 1.284× | 1.65% | PASS |
| mem_reads | `covering_index_scan` | 4.58ms | 4.57ms | 0.998× | 1.54% | PASS |
| mem_reads | `groupby_scan` | 31.62ms | 34.17ms | 1.080× | 1.04% | PASS |
| mem_reads | `index_join` | 6.86ms | 9.12ms | 1.331× | 1.18% | PASS |
| mem_reads | `index_join_scan` | 4.54ms | 5.36ms | 1.182× | 1.53% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.23s | 1.166× | 0.76% | PASS |
| mem_reads | `table_scan` | 1.25s | 1.38s | 1.105× | 1.49% | PASS |
| mem_reads | `oltp_read_only` | 128.70ms | 141.02ms | 1.096× | 2.23% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.62ms | 358.26ms | 1.521× | 0.73% | PASS |
| mem_writes | `oltp_insert` | 21.88ms | 39.98ms | 1.827× | 1.23% | PASS |
| mem_writes | `oltp_update_index` | 72.35ms | 136.46ms | 1.886× | 1.71% | PASS |
| mem_writes | `oltp_update_non_index` | 49.55ms | 88.74ms | 1.791× | 1.45% | PASS |
| mem_writes | `oltp_delete_insert` | 52.22ms | 105.39ms | 2.018× | 1.98% | PASS |
| mem_writes | `oltp_write_only` | 28.91ms | 61.65ms | 2.132× | 1.28% | PASS |
| mem_writes | `types_delete_insert` | 32.79ms | 55.22ms | 1.684× | 1.35% | PASS |
| mem_writes | `oltp_read_write` | 85.84ms | 140.62ms | 1.638× | 1.42% | PASS |
| file_reads | `oltp_point_select` | 104.11ms | 63.78ms | 0.613× | 0.90% | PASS |
| file_reads | `oltp_range_select` | 21.11ms | 17.03ms | 0.806× | 1.68% | PASS |
| file_reads | `oltp_sum_range` | 19.78ms | 17.14ms | 0.867× | 1.70% | PASS |
| file_reads | `oltp_order_range` | 3.88ms | 3.53ms | 0.909× | 2.57% | PASS |
| file_reads | `oltp_distinct_range` | 4.87ms | 4.59ms | 0.943× | 1.90% | PASS |
| file_reads | `oltp_index_scan` | 12.12ms | 9.21ms | 0.760× | 1.30% | PASS |
| file_reads | `select_random_points` | 26.39ms | 24.93ms | 0.945× | 1.70% | PASS |
| file_reads | `select_random_ranges` | 11.61ms | 7.91ms | 0.681× | 1.04% | PASS |
| file_reads | `covering_index_scan` | 12.50ms | 7.41ms | 0.592× | 1.95% | PASS |
| file_reads | `groupby_scan` | 32.91ms | 34.68ms | 1.054× | 1.11% | PASS |
| file_reads | `index_join` | 12.19ms | 11.59ms | 0.951× | 2.27% | PASS |
| file_reads | `index_join_scan` | 5.59ms | 5.86ms | 1.049× | 1.27% | PASS |
| file_reads | `types_table_scan` | 1.07s | 1.23s | 1.156× | 1.23% | PASS |
| file_reads | `table_scan` | 1.31s | 1.39s | 1.057× | 2.97% | PASS |
| file_reads | `oltp_read_only` | 240.91ms | 181.59ms | 0.754× | 0.69% | PASS |
| file_writes | `oltp_bulk_insert` | 256.51ms | 390.08ms | 1.521× | 0.77% | PASS |
| file_writes | `oltp_insert` | 62.15ms | 53.68ms | 0.864× | 24.75% | PASS |
| file_writes | `oltp_update_index` | 121.71ms | 176.60ms | 1.451× | 1.51% | PASS |
| file_writes | `oltp_update_non_index` | 111.77ms | 116.51ms | 1.042× | 11.45% | PASS |
| file_writes | `oltp_delete_insert` | 96.23ms | 137.79ms | 1.432× | 1.37% | PASS |
| file_writes | `oltp_write_only` | 85.69ms | 88.02ms | 1.027× | 9.41% | PASS |
| file_writes | `types_delete_insert` | 57.95ms | 77.03ms | 1.329× | 1.86% | PASS |
| file_writes | `oltp_read_write` | 137.71ms | 170.65ms | 1.239× | 7.15% | PASS |
| ac_reads | `oltp_point_select` | 56.37ms | 65.09ms | 1.155× | 1.18% | PASS |
| ac_reads | `oltp_range_select` | 17.32ms | 17.45ms | 1.008× | 1.95% | PASS |
| ac_reads | `oltp_sum_range` | 15.88ms | 17.56ms | 1.106× | 1.33% | PASS |
| ac_reads | `oltp_order_range` | 3.51ms | 3.55ms | 1.010× | 1.14% | PASS |
| ac_reads | `oltp_distinct_range` | 4.52ms | 4.64ms | 1.025× | 1.16% | PASS |
| ac_reads | `oltp_index_scan` | 7.73ms | 9.39ms | 1.214× | 1.32% | PASS |
| ac_reads | `select_random_points` | 21.98ms | 25.76ms | 1.172× | 1.50% | PASS |
| ac_reads | `select_random_ranges` | 6.92ms | 7.99ms | 1.154× | 0.97% | PASS |
| ac_reads | `covering_index_scan` | 8.30ms | 7.46ms | 0.898× | 1.25% | PASS |
| ac_reads | `groupby_scan` | 32.30ms | 34.62ms | 1.072× | 0.85% | PASS |
| ac_reads | `index_join` | 9.40ms | 11.30ms | 1.202× | 1.90% | PASS |
| ac_reads | `index_join_scan` | 5.13ms | 5.87ms | 1.144× | 2.01% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.25s | 1.120× | 2.49% | PASS |
| ac_reads | `table_scan` | 1.34s | 1.38s | 1.035× | 3.40% | PASS |
| ac_reads | `oltp_read_only` | 155.17ms | 176.10ms | 1.135× | 1.64% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.71ms | 88.78ms | 3.593× | 7.21% | PASS |
| ac_writes | `oltp_insert_ac` | 27.34ms | 102.06ms | 3.732× | 7.31% | PASS |
| ac_writes | `oltp_update_index_ac` | 30.11ms | 122.19ms | 4.059× | 6.67% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.57ms | 100.68ms | 4.097× | 6.18% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.23ms | 113.28ms | 4.318× | 5.82% | PASS |
| ac_writes | `oltp_write_only_ac` | 28.17ms | 113.70ms | 4.036× | 5.95% | PASS |
| ac_writes | `types_delete_insert_ac` | 25.46ms | 103.19ms | 4.054× | 8.42% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.62ms | 118.47ms | 3.523× | 7.85% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.86ms | 36.71ms | 1.229× | 1.72% | PASS |
| mem_reads | `oltp_range_select` | 12.77ms | 14.05ms | 1.100× | 2.33% | PASS |
| mem_reads | `oltp_sum_range` | 11.88ms | 13.96ms | 1.175× | 1.68% | PASS |
| mem_reads | `oltp_order_range` | 2.86ms | 3.13ms | 1.095× | 1.48% | PASS |
| mem_reads | `oltp_distinct_range` | 3.91ms | 4.18ms | 1.069× | 1.50% | PASS |
| mem_reads | `oltp_index_scan` | 4.39ms | 6.06ms | 1.380× | 1.48% | PASS |
| mem_reads | `select_random_points` | 16.82ms | 20.41ms | 1.214× | 1.17% | PASS |
| mem_reads | `select_random_ranges` | 3.89ms | 5.13ms | 1.319× | 2.00% | PASS |
| mem_reads | `covering_index_scan` | 4.38ms | 4.38ms | 0.999× | 1.17% | PASS |
| mem_reads | `groupby_scan` | 31.28ms | 33.88ms | 1.083× | 1.11% | PASS |
| mem_reads | `index_join` | 6.75ms | 8.87ms | 1.314× | 1.75% | PASS |
| mem_reads | `index_join_scan` | 4.10ms | 5.29ms | 1.291× | 2.06% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.23s | 1.180× | 0.81% | PASS |
| mem_reads | `table_scan` | 1.34s | 1.40s | 1.042× | 3.66% | PASS |
| mem_reads | `oltp_read_only` | 119.02ms | 136.74ms | 1.149× | 2.11% | PASS |
| mem_writes | `oltp_bulk_insert` | 242.53ms | 351.91ms | 1.451× | 0.79% | PASS |
| mem_writes | `oltp_insert` | 19.83ms | 38.95ms | 1.964× | 0.63% | PASS |
| mem_writes | `oltp_update_index` | 66.47ms | 127.40ms | 1.916× | 1.06% | PASS |
| mem_writes | `oltp_update_non_index` | 48.12ms | 84.02ms | 1.746× | 1.53% | PASS |
| mem_writes | `oltp_delete_insert` | 49.54ms | 103.85ms | 2.096× | 0.79% | PASS |
| mem_writes | `oltp_write_only` | 28.73ms | 62.73ms | 2.184× | 1.86% | PASS |
| mem_writes | `types_delete_insert` | 32.34ms | 53.74ms | 1.662× | 1.78% | PASS |
| mem_writes | `oltp_read_write` | 84.88ms | 139.14ms | 1.639× | 1.51% | PASS |
| file_reads | `oltp_point_select` | 105.76ms | 63.98ms | 0.605× | 1.37% | PASS |
| file_reads | `oltp_range_select` | 21.69ms | 17.02ms | 0.785× | 3.02% | PASS |
| file_reads | `oltp_sum_range` | 20.39ms | 16.98ms | 0.832× | 1.80% | PASS |
| file_reads | `oltp_order_range` | 3.96ms | 3.54ms | 0.893× | 1.34% | PASS |
| file_reads | `oltp_distinct_range` | 5.03ms | 4.64ms | 0.921× | 1.24% | PASS |
| file_reads | `oltp_index_scan` | 12.69ms | 9.31ms | 0.734× | 1.20% | PASS |
| file_reads | `select_random_points` | 28.14ms | 25.05ms | 0.890× | 1.90% | PASS |
| file_reads | `select_random_ranges` | 11.68ms | 7.84ms | 0.671× | 1.11% | PASS |
| file_reads | `covering_index_scan` | 12.55ms | 7.37ms | 0.587× | 1.93% | PASS |
| file_reads | `groupby_scan` | 33.13ms | 34.73ms | 1.048× | 1.28% | PASS |
| file_reads | `index_join` | 11.38ms | 11.45ms | 1.006× | 2.09% | PASS |
| file_reads | `index_join_scan` | 5.34ms | 5.93ms | 1.110× | 3.49% | PASS |
| file_reads | `types_table_scan` | 1.13s | 1.25s | 1.110× | 2.16% | PASS |
| file_reads | `table_scan` | 1.37s | 1.40s | 1.027× | 0.93% | PASS |
| file_reads | `oltp_read_only` | 237.91ms | 179.10ms | 0.753× | 0.98% | PASS |
| file_writes | `oltp_bulk_insert` | 262.85ms | 382.95ms | 1.457× | 1.24% | PASS |
| file_writes | `oltp_insert` | 33.39ms | 53.11ms | 1.591× | 1.40% | PASS |
| file_writes | `oltp_update_index` | 108.28ms | 168.42ms | 1.555× | 1.23% | PASS |
| file_writes | `oltp_update_non_index` | 82.30ms | 110.88ms | 1.347× | 0.96% | PASS |
| file_writes | `oltp_delete_insert` | 86.12ms | 134.54ms | 1.562× | 1.47% | PASS |
| file_writes | `oltp_write_only` | 59.66ms | 86.78ms | 1.455× | 1.72% | PASS |
| file_writes | `types_delete_insert` | 54.31ms | 74.25ms | 1.367× | 1.78% | PASS |
| file_writes | `oltp_read_write` | 124.04ms | 167.62ms | 1.351× | 1.73% | PASS |
| ac_reads | `oltp_point_select` | 56.72ms | 64.68ms | 1.140× | 1.65% | PASS |
| ac_reads | `oltp_range_select` | 16.68ms | 17.28ms | 1.036× | 1.98% | PASS |
| ac_reads | `oltp_sum_range` | 15.75ms | 17.18ms | 1.091× | 1.58% | PASS |
| ac_reads | `oltp_order_range` | 3.45ms | 3.55ms | 1.030× | 1.57% | PASS |
| ac_reads | `oltp_distinct_range` | 4.45ms | 4.64ms | 1.042× | 1.16% | PASS |
| ac_reads | `oltp_index_scan` | 7.63ms | 9.30ms | 1.218× | 1.21% | PASS |
| ac_reads | `select_random_points` | 22.02ms | 25.05ms | 1.137× | 2.25% | PASS |
| ac_reads | `select_random_ranges` | 6.99ms | 7.91ms | 1.131× | 1.08% | PASS |
| ac_reads | `covering_index_scan` | 8.04ms | 7.44ms | 0.925× | 1.82% | PASS |
| ac_reads | `groupby_scan` | 32.72ms | 34.76ms | 1.063× | 0.80% | PASS |
| ac_reads | `index_join` | 9.35ms | 11.72ms | 1.254× | 2.10% | PASS |
| ac_reads | `index_join_scan` | 4.97ms | 6.17ms | 1.242× | 2.38% | PASS |
| ac_reads | `types_table_scan` | 1.14s | 1.25s | 1.099× | 0.87% | PASS |
| ac_reads | `table_scan` | 1.30s | 1.39s | 1.063× | 1.27% | PASS |
| ac_reads | `oltp_read_only` | 161.15ms | 177.65ms | 1.102× | 1.00% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.33ms | 74.27ms | 3.481× | 5.02% | PASS |
| ac_writes | `oltp_insert_ac` | 23.43ms | 96.44ms | 4.116× | 5.36% | PASS |
| ac_writes | `oltp_update_index_ac` | 24.80ms | 106.46ms | 4.293× | 6.36% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.77ms | 86.41ms | 3.969× | 7.46% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 21.43ms | 96.24ms | 4.492× | 4.37% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.29ms | 101.31ms | 4.170× | 6.36% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.30ms | 89.50ms | 4.203× | 6.52% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.64ms | 107.04ms | 3.612× | 8.23% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.28ms | 37.45ms | 1.125× | 1.50% | PASS |
| mem_reads | `oltp_range_select` | 19.78ms | 19.94ms | 1.008× | 1.22% | PASS |
| mem_reads | `oltp_sum_range` | 18.12ms | 19.00ms | 1.049× | 1.01% | PASS |
| mem_reads | `oltp_order_range` | 3.72ms | 3.94ms | 1.060× | 1.47% | PASS |
| mem_reads | `oltp_distinct_range` | 4.79ms | 5.02ms | 1.047× | 0.89% | PASS |
| mem_reads | `oltp_index_scan` | 4.68ms | 5.72ms | 1.223× | 1.61% | PASS |
| mem_reads | `select_random_points` | 27.41ms | 30.04ms | 1.096× | 1.11% | PASS |
| mem_reads | `select_random_ranges` | 7.56ms | 8.16ms | 1.080× | 1.45% | PASS |
| mem_reads | `covering_index_scan` | 4.37ms | 4.15ms | 0.949× | 1.69% | PASS |
| mem_reads | `groupby_scan` | 38.50ms | 40.03ms | 1.040× | 0.85% | PASS |
| mem_reads | `index_join` | 8.14ms | 9.97ms | 1.225× | 1.08% | PASS |
| mem_reads | `index_join_scan` | 4.29ms | 5.54ms | 1.292× | 1.18% | PASS |
| mem_reads | `types_table_scan` | 1.13s | 1.25s | 1.105× | 0.44% | PASS |
| mem_reads | `table_scan` | 1.32s | 1.37s | 1.033× | 1.02% | PASS |
| mem_reads | `oltp_read_only` | 151.27ms | 162.75ms | 1.076× | 0.98% | PASS |
| mem_writes | `oltp_bulk_insert` | 246.55ms | 339.46ms | 1.377× | 0.68% | PASS |
| mem_writes | `oltp_insert` | 19.59ms | 36.09ms | 1.842× | 0.55% | PASS |
| mem_writes | `oltp_update_index` | 68.15ms | 117.89ms | 1.730× | 0.63% | PASS |
| mem_writes | `oltp_update_non_index` | 50.87ms | 82.78ms | 1.627× | 0.85% | PASS |
| mem_writes | `oltp_delete_insert` | 49.93ms | 95.90ms | 1.921× | 0.66% | PASS |
| mem_writes | `oltp_write_only` | 27.22ms | 58.86ms | 2.163× | 0.76% | PASS |
| mem_writes | `types_delete_insert` | 32.52ms | 53.02ms | 1.630× | 0.82% | PASS |
| mem_writes | `oltp_read_write` | 97.92ms | 147.04ms | 1.502× | 0.78% | PASS |
| file_reads | `oltp_point_select` | 129.28ms | 69.56ms | 0.538× | 1.05% | PASS |
| file_reads | `oltp_range_select` | 29.88ms | 23.34ms | 0.781× | 0.87% | PASS |
| file_reads | `oltp_sum_range` | 28.29ms | 22.59ms | 0.799× | 0.75% | PASS |
| file_reads | `oltp_order_range` | 4.79ms | 4.28ms | 0.895× | 0.97% | PASS |
| file_reads | `oltp_distinct_range` | 5.81ms | 5.40ms | 0.928× | 1.17% | PASS |
| file_reads | `oltp_index_scan` | 14.55ms | 9.17ms | 0.630× | 1.44% | PASS |
| file_reads | `select_random_points` | 37.61ms | 33.63ms | 0.894× | 0.57% | PASS |
| file_reads | `select_random_ranges` | 17.39ms | 11.68ms | 0.671× | 0.89% | PASS |
| file_reads | `covering_index_scan` | 14.30ms | 7.57ms | 0.530× | 0.72% | PASS |
| file_reads | `groupby_scan` | 39.58ms | 40.67ms | 1.028× | 0.64% | PASS |
| file_reads | `index_join` | 13.51ms | 12.29ms | 0.910× | 1.57% | PASS |
| file_reads | `index_join_scan` | 5.24ms | 6.04ms | 1.153× | 1.14% | PASS |
| file_reads | `types_table_scan` | 1.11s | 1.24s | 1.114× | 0.41% | PASS |
| file_reads | `table_scan` | 1.29s | 1.35s | 1.052× | 0.56% | PASS |
| file_reads | `oltp_read_only` | 288.10ms | 208.44ms | 0.723× | 0.42% | PASS |
| file_writes | `oltp_bulk_insert` | 263.08ms | 363.35ms | 1.381× | 0.75% | PASS |
| file_writes | `oltp_insert` | 26.23ms | 46.48ms | 1.772× | 1.03% | PASS |
| file_writes | `oltp_update_index` | 98.56ms | 144.80ms | 1.469× | 0.96% | PASS |
| file_writes | `oltp_update_non_index` | 78.11ms | 105.68ms | 1.353× | 1.31% | PASS |
| file_writes | `oltp_delete_insert` | 77.91ms | 120.59ms | 1.548× | 1.33% | PASS |
| file_writes | `oltp_write_only` | 51.11ms | 80.18ms | 1.569× | 1.52% | PASS |
| file_writes | `types_delete_insert` | 50.38ms | 67.50ms | 1.340× | 1.11% | PASS |
| file_writes | `oltp_read_write` | 122.05ms | 169.06ms | 1.385× | 1.00% | PASS |
| ac_reads | `oltp_point_select` | 64.51ms | 69.63ms | 1.079× | 0.88% | PASS |
| ac_reads | `oltp_range_select` | 22.86ms | 23.32ms | 1.020× | 0.62% | PASS |
| ac_reads | `oltp_sum_range` | 21.33ms | 22.46ms | 1.053× | 0.62% | PASS |
| ac_reads | `oltp_order_range` | 4.09ms | 4.28ms | 1.047× | 0.87% | PASS |
| ac_reads | `oltp_distinct_range` | 5.14ms | 5.39ms | 1.048× | 0.90% | PASS |
| ac_reads | `oltp_index_scan` | 7.96ms | 9.16ms | 1.151× | 0.94% | PASS |
| ac_reads | `select_random_points` | 30.65ms | 33.68ms | 1.099× | 0.67% | PASS |
| ac_reads | `select_random_ranges` | 10.77ms | 11.68ms | 1.084× | 1.02% | PASS |
| ac_reads | `covering_index_scan` | 7.71ms | 7.56ms | 0.980× | 0.96% | PASS |
| ac_reads | `groupby_scan` | 38.65ms | 40.59ms | 1.050× | 0.66% | PASS |
| ac_reads | `index_join` | 10.02ms | 12.29ms | 1.226× | 1.09% | PASS |
| ac_reads | `index_join_scan` | 4.67ms | 6.07ms | 1.298× | 0.89% | PASS |
| ac_reads | `types_table_scan` | 1.11s | 1.24s | 1.115× | 0.50% | PASS |
| ac_reads | `table_scan` | 1.29s | 1.35s | 1.051× | 0.53% | PASS |
| ac_reads | `oltp_read_only` | 195.49ms | 209.13ms | 1.070× | 0.61% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.25ms | 67.27ms | 3.899× | 3.19% | PASS |
| ac_writes | `oltp_insert_ac` | 19.37ms | 88.56ms | 4.571× | 3.17% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.17ms | 99.53ms | 4.701× | 2.73% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.52ms | 77.98ms | 4.452× | 3.09% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 19.43ms | 91.57ms | 4.712× | 4.96% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.75ms | 89.20ms | 4.517× | 3.04% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.04ms | 77.91ms | 4.571× | 4.16% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.26ms | 99.61ms | 3.793× | 2.71% | PASS |

</details>

## Version-control latency

Wall time: 2m 20s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.52ms | 200.00ms | 41.8% | 0.51% | PASS |
| `status_dirty_many_tables` | 86.80ms | 200.00ms | 43.4% | 0.55% | PASS |
| `diff_regular_working_one_table` | 79.48ms | 150.00ms | 53.0% | 0.78% | PASS |
| `diff_regular_working_many_tables` | 92.69ms | 200.00ms | 46.3% | 0.53% | PASS |
| `diff_stat_working_many_tables` | 92.36ms | 200.00ms | 46.2% | 0.42% | PASS |
| `diff_schema_working_many_tables` | 92.82ms | 200.00ms | 46.4% | 0.53% | PASS |
| `branch_list_many_branches` | 23.54ms | 100.00ms | 23.5% | 1.79% | PASS |
| `branch_create_delete` | 25.87ms | 100.00ms | 25.9% | 1.75% | PASS |
| `checkout_branch_clean` | 55.81ms | 200.00ms | 27.9% | 1.13% | PASS |
| `merge_data_no_conflicts` | 29.54ms | 150.00ms | 19.7% | 1.45% | PASS |
| `merge_schema_no_conflicts` | 22.89ms | 100.00ms | 22.9% | 1.77% | PASS |
| `merge_data_conflicts` | 127.71ms | 250.00ms | 51.1% | 0.34% | PASS |
| `merge_data_conflicts_with_resolve` | 127.84ms | 250.00ms | 51.1% | 0.50% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
