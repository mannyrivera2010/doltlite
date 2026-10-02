# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-02 17:30 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260927.320.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/37030429440)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 59m 1s | 7.33s | 9.02s | 1.231× | 1.31% | **PASS** |
| textpk | 69 | 55 | 1h 30m 44s | 8.91s | 10.11s | 1.135× | 1.48% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 41s | 9.86s | 11.46s | 1.162× | 1.13% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 10s | 10.69s | 11.86s | 1.109× | 1.47% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 18.11ms | 20.28ms | 1.120× | 1.00% | PASS |
| mem_reads | `oltp_range_select` | 8.34ms | 9.74ms | 1.168× | 1.13% | PASS |
| mem_reads | `oltp_sum_range` | 7.68ms | 9.18ms | 1.196× | 1.31% | PASS |
| mem_reads | `oltp_order_range` | 2.17ms | 2.45ms | 1.128× | 0.97% | PASS |
| mem_reads | `oltp_distinct_range` | 2.95ms | 3.23ms | 1.095× | 0.79% | PASS |
| mem_reads | `oltp_index_scan` | 3.04ms | 3.74ms | 1.231× | 1.31% | PASS |
| mem_reads | `select_random_points` | 8.60ms | 9.17ms | 1.067× | 0.99% | PASS |
| mem_reads | `select_random_ranges` | 2.31ms | 2.98ms | 1.289× | 2.49% | PASS |
| mem_reads | `covering_index_scan` | 2.95ms | 2.99ms | 1.013× | 1.61% | PASS |
| mem_reads | `groupby_scan` | 25.15ms | 26.72ms | 1.063× | 0.85% | PASS |
| mem_reads | `index_join` | 4.56ms | 6.23ms | 1.366× | 0.64% | PASS |
| mem_reads | `index_join_scan` | 2.53ms | 3.72ms | 1.472× | 0.85% | PASS |
| mem_reads | `types_table_scan` | 852.06ms | 1.03s | 1.205× | 0.42% | PASS |
| mem_reads | `table_scan` | 965.86ms | 1.12s | 1.157× | 0.44% | PASS |
| mem_reads | `oltp_read_only` | 79.58ms | 91.12ms | 1.145× | 0.54% | PASS |
| mem_writes | `oltp_bulk_insert` | 120.88ms | 161.47ms | 1.336× | 0.79% | PASS |
| mem_writes | `oltp_insert` | 10.93ms | 19.13ms | 1.749× | 0.69% | PASS |
| mem_writes | `oltp_update_index` | 37.41ms | 75.10ms | 2.008× | 0.86% | PASS |
| mem_writes | `oltp_update_non_index` | 24.72ms | 42.25ms | 1.709× | 0.77% | PASS |
| mem_writes | `oltp_delete_insert` | 33.93ms | 55.58ms | 1.638× | 0.68% | PASS |
| mem_writes | `oltp_write_only` | 16.03ms | 31.73ms | 1.979× | 0.57% | PASS |
| mem_writes | `types_delete_insert` | 17.61ms | 27.68ms | 1.572× | 0.83% | PASS |
| mem_writes | `oltp_read_write` | 47.35ms | 76.28ms | 1.611× | 1.28% | PASS |
| file_reads | `oltp_point_select` | 43.40ms | 28.36ms | 0.653× | 0.69% | PASS |
| file_reads | `oltp_range_select` | 10.71ms | 10.74ms | 1.003× | 1.76% | PASS |
| file_reads | `oltp_sum_range` | 9.90ms | 10.00ms | 1.010× | 2.10% | PASS |
| file_reads | `oltp_order_range` | 2.38ms | 2.48ms | 1.043× | 1.26% | PASS |
| file_reads | `oltp_distinct_range` | 3.21ms | 3.26ms | 1.015× | 1.14% | PASS |
| file_reads | `oltp_index_scan` | 5.76ms | 4.71ms | 0.818× | 1.47% | PASS |
| file_reads | `select_random_points` | 11.54ms | 10.29ms | 0.892× | 1.80% | PASS |
| file_reads | `select_random_ranges` | 4.95ms | 3.86ms | 0.779× | 1.82% | PASS |
| file_reads | `covering_index_scan` | 5.73ms | 3.95ms | 0.690× | 2.21% | PASS |
| file_reads | `groupby_scan` | 25.37ms | 26.84ms | 1.058× | 1.10% | PASS |
| file_reads | `index_join` | 6.12ms | 6.87ms | 1.122× | 1.33% | PASS |
| file_reads | `index_join_scan` | 2.88ms | 3.87ms | 1.345× | 1.51% | PASS |
| file_reads | `types_table_scan` | 851.07ms | 1.03s | 1.211× | 0.52% | PASS |
| file_reads | `table_scan` | 967.81ms | 1.12s | 1.160× | 0.31% | PASS |
| file_reads | `oltp_read_only` | 117.28ms | 104.16ms | 0.888× | 0.76% | PASS |
| file_writes | `oltp_bulk_insert` | 156.39ms | 209.62ms | 1.340× | 4.20% | PASS |
| file_writes | `oltp_insert` | 19.93ms | 28.75ms | 1.443× | 0.61% | PASS |
| file_writes | `oltp_update_index` | 123.53ms | 130.13ms | 1.053× | 6.09% | PASS |
| file_writes | `oltp_update_non_index` | 90.64ms | 92.96ms | 1.026× | 1.60% | PASS |
| file_writes | `oltp_delete_insert` | 106.41ms | 107.50ms | 1.010× | 3.20% | PASS |
| file_writes | `oltp_write_only` | 71.14ms | 71.53ms | 1.005× | 5.00% | PASS |
| file_writes | `types_delete_insert` | 60.49ms | 54.28ms | 0.897× | 7.55% | PASS |
| file_writes | `oltp_read_write` | 111.39ms | 116.72ms | 1.048× | 5.37% | PASS |
| ac_reads | `oltp_point_select` | 26.51ms | 28.72ms | 1.083× | 0.64% | PASS |
| ac_reads | `oltp_range_select` | 9.48ms | 10.99ms | 1.159× | 1.49% | PASS |
| ac_reads | `oltp_sum_range` | 9.07ms | 10.33ms | 1.139× | 1.51% | PASS |
| ac_reads | `oltp_order_range` | 2.36ms | 2.58ms | 1.094× | 1.80% | PASS |
| ac_reads | `oltp_distinct_range` | 3.15ms | 3.35ms | 1.062× | 1.17% | PASS |
| ac_reads | `oltp_index_scan` | 4.07ms | 4.71ms | 1.158× | 1.50% | PASS |
| ac_reads | `select_random_points` | 9.77ms | 10.36ms | 1.061× | 1.42% | PASS |
| ac_reads | `select_random_ranges` | 3.18ms | 3.77ms | 1.188× | 2.49% | PASS |
| ac_reads | `covering_index_scan` | 4.01ms | 4.04ms | 1.008× | 1.80% | PASS |
| ac_reads | `groupby_scan` | 24.68ms | 26.54ms | 1.075× | 1.24% | PASS |
| ac_reads | `index_join` | 5.26ms | 6.87ms | 1.306× | 1.68% | PASS |
| ac_reads | `index_join_scan` | 2.73ms | 3.87ms | 1.417× | 1.57% | PASS |
| ac_reads | `types_table_scan` | 850.31ms | 1.03s | 1.210× | 0.33% | PASS |
| ac_reads | `table_scan` | 968.08ms | 1.12s | 1.159× | 0.36% | PASS |
| ac_reads | `oltp_read_only` | 91.15ms | 103.87ms | 1.140× | 0.81% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 25.10ms | 65.60ms | 2.614× | 5.30% | PASS |
| ac_writes | `oltp_insert_ac` | 26.75ms | 77.81ms | 2.909× | 3.27% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.70ms | 89.46ms | 3.229× | 4.77% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 26.13ms | 75.92ms | 2.905× | 3.51% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 27.41ms | 84.55ms | 3.085× | 5.11% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.61ms | 81.77ms | 3.073× | 3.70% | PASS |
| ac_writes | `types_delete_insert_ac` | 25.18ms | 80.33ms | 3.190× | 5.46% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.19ms | 87.60ms | 3.001× | 3.89% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.18ms | 27.88ms | 1.153× | 1.49% | PASS |
| mem_reads | `oltp_range_select` | 11.29ms | 10.93ms | 0.968× | 1.81% | PASS |
| mem_reads | `oltp_sum_range` | 10.00ms | 10.77ms | 1.076× | 1.73% | PASS |
| mem_reads | `oltp_order_range` | 2.50ms | 2.52ms | 1.008× | 1.20% | PASS |
| mem_reads | `oltp_distinct_range` | 3.34ms | 3.30ms | 0.987× | 0.80% | PASS |
| mem_reads | `oltp_index_scan` | 3.72ms | 4.78ms | 1.285× | 1.27% | PASS |
| mem_reads | `select_random_points` | 14.99ms | 16.34ms | 1.090× | 1.74% | PASS |
| mem_reads | `select_random_ranges` | 3.44ms | 4.20ms | 1.221× | 2.06% | PASS |
| mem_reads | `covering_index_scan` | 3.79ms | 3.52ms | 0.930× | 1.64% | PASS |
| mem_reads | `groupby_scan` | 26.77ms | 27.78ms | 1.038× | 0.60% | PASS |
| mem_reads | `index_join` | 5.68ms | 7.07ms | 1.244× | 1.75% | PASS |
| mem_reads | `index_join_scan` | 4.08ms | 4.65ms | 1.141× | 0.96% | PASS |
| mem_reads | `types_table_scan` | 889.47ms | 972.26ms | 1.093× | 0.98% | PASS |
| mem_reads | `table_scan` | 1.04s | 1.06s | 1.011× | 1.00% | PASS |
| mem_reads | `oltp_read_only` | 93.99ms | 101.43ms | 1.079× | 1.28% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.67ms | 258.92ms | 1.425× | 0.82% | PASS |
| mem_writes | `oltp_insert` | 17.62ms | 29.75ms | 1.688× | 1.09% | PASS |
| mem_writes | `oltp_update_index` | 58.93ms | 104.43ms | 1.772× | 1.40% | PASS |
| mem_writes | `oltp_update_non_index` | 41.25ms | 67.56ms | 1.638× | 1.73% | PASS |
| mem_writes | `oltp_delete_insert` | 42.50ms | 81.08ms | 1.908× | 0.91% | PASS |
| mem_writes | `oltp_write_only` | 24.79ms | 48.88ms | 1.972× | 1.23% | PASS |
| mem_writes | `types_delete_insert` | 26.70ms | 40.96ms | 1.534× | 1.55% | PASS |
| mem_writes | `oltp_read_write` | 67.99ms | 103.81ms | 1.527× | 1.19% | PASS |
| file_reads | `oltp_point_select` | 99.25ms | 53.06ms | 0.535× | 0.79% | PASS |
| file_reads | `oltp_range_select` | 19.84ms | 13.56ms | 0.684× | 0.80% | PASS |
| file_reads | `oltp_sum_range` | 18.37ms | 13.39ms | 0.729× | 0.81% | PASS |
| file_reads | `oltp_order_range` | 3.51ms | 2.85ms | 0.813× | 1.46% | PASS |
| file_reads | `oltp_distinct_range` | 4.31ms | 3.62ms | 0.840× | 1.04% | PASS |
| file_reads | `oltp_index_scan` | 11.80ms | 7.76ms | 0.657× | 0.63% | PASS |
| file_reads | `select_random_points` | 23.04ms | 19.25ms | 0.836× | 1.31% | PASS |
| file_reads | `select_random_ranges` | 11.19ms | 6.81ms | 0.608× | 1.00% | PASS |
| file_reads | `covering_index_scan` | 13.38ms | 6.58ms | 0.492× | 2.09% | PASS |
| file_reads | `groupby_scan` | 29.29ms | 28.78ms | 0.983× | 1.36% | PASS |
| file_reads | `index_join` | 10.56ms | 9.30ms | 0.881× | 0.83% | PASS |
| file_reads | `index_join_scan` | 5.00ms | 5.12ms | 1.025× | 0.97% | PASS |
| file_reads | `types_table_scan` | 897.92ms | 966.25ms | 1.076× | 1.23% | PASS |
| file_reads | `table_scan` | 1.06s | 1.06s | 0.999× | 1.32% | PASS |
| file_reads | `oltp_read_only` | 203.13ms | 136.41ms | 0.672× | 0.42% | PASS |
| file_writes | `oltp_bulk_insert` | 256.30ms | 354.56ms | 1.383× | 5.13% | PASS |
| file_writes | `oltp_insert` | 62.65ms | 68.40ms | 1.092× | 18.44% | PASS |
| file_writes | `oltp_update_index` | 211.23ms | 207.98ms | 0.985× | 3.39% | PASS |
| file_writes | `oltp_update_non_index` | 161.34ms | 140.36ms | 0.870× | 11.96% | PASS |
| file_writes | `oltp_delete_insert` | 176.20ms | 173.33ms | 0.984× | 8.66% | PASS |
| file_writes | `oltp_write_only` | 164.11ms | 128.12ms | 0.781× | 34.76% | PASS |
| file_writes | `types_delete_insert` | 117.95ms | 108.12ms | 0.917× | 21.82% | PASS |
| file_writes | `oltp_read_write` | 200.79ms | 197.97ms | 0.986× | 10.34% | PASS |
| ac_reads | `oltp_point_select` | 54.37ms | 58.81ms | 1.082× | 1.48% | PASS |
| ac_reads | `oltp_range_select` | 17.69ms | 14.91ms | 0.843× | 2.60% | PASS |
| ac_reads | `oltp_sum_range` | 15.69ms | 14.83ms | 0.945× | 3.08% | PASS |
| ac_reads | `oltp_order_range` | 3.20ms | 2.92ms | 0.914× | 1.38% | PASS |
| ac_reads | `oltp_distinct_range` | 3.91ms | 3.67ms | 0.938× | 1.55% | PASS |
| ac_reads | `oltp_index_scan` | 7.11ms | 8.11ms | 1.140× | 2.84% | PASS |
| ac_reads | `select_random_points` | 20.25ms | 20.81ms | 1.027× | 4.35% | PASS |
| ac_reads | `select_random_ranges` | 6.45ms | 6.93ms | 1.074× | 1.39% | PASS |
| ac_reads | `covering_index_scan` | 8.22ms | 6.60ms | 0.803× | 1.41% | PASS |
| ac_reads | `groupby_scan` | 28.70ms | 28.80ms | 1.003× | 1.06% | PASS |
| ac_reads | `index_join` | 8.87ms | 9.99ms | 1.127× | 6.90% | PASS |
| ac_reads | `index_join_scan` | 4.67ms | 5.22ms | 1.117× | 1.69% | PASS |
| ac_reads | `types_table_scan` | 892.59ms | 965.50ms | 1.082× | 0.68% | PASS |
| ac_reads | `table_scan` | 1.06s | 1.06s | 0.995× | 2.80% | PASS |
| ac_reads | `oltp_read_only` | 130.45ms | 136.18ms | 1.044× | 0.60% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 26.92ms | 97.97ms | 3.639× | 50.62% | PASS |
| ac_writes | `oltp_insert_ac` | 53.38ms | 132.01ms | 2.473× | 51.64% | PASS |
| ac_writes | `oltp_update_index_ac` | 30.27ms | 146.33ms | 4.835× | 52.81% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.93ms | 102.63ms | 4.289× | 79.58% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.40ms | 106.18ms | 4.741× | 41.49% | PASS |
| ac_writes | `oltp_write_only_ac` | 34.88ms | 141.02ms | 4.044× | 56.01% | PASS |
| ac_writes | `types_delete_insert_ac` | 27.71ms | 156.79ms | 5.658× | 57.96% | PASS |
| ac_writes | `oltp_read_write_ac` | 58.64ms | 184.15ms | 3.140× | 43.93% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 27.30ms | 30.48ms | 1.117× | 0.99% | PASS |
| mem_reads | `oltp_range_select` | 12.47ms | 12.82ms | 1.028× | 0.77% | PASS |
| mem_reads | `oltp_sum_range` | 11.94ms | 12.19ms | 1.021× | 0.74% | PASS |
| mem_reads | `oltp_order_range` | 2.83ms | 2.97ms | 1.049× | 0.94% | PASS |
| mem_reads | `oltp_distinct_range` | 3.77ms | 3.90ms | 1.033× | 0.68% | PASS |
| mem_reads | `oltp_index_scan` | 4.14ms | 5.23ms | 1.262× | 1.50% | PASS |
| mem_reads | `select_random_points` | 16.82ms | 19.24ms | 1.144× | 0.84% | PASS |
| mem_reads | `select_random_ranges` | 3.51ms | 4.63ms | 1.319× | 0.88% | PASS |
| mem_reads | `covering_index_scan` | 3.72ms | 4.00ms | 1.075× | 2.09% | PASS |
| mem_reads | `groupby_scan` | 32.44ms | 32.44ms | 1.000× | 0.42% | PASS |
| mem_reads | `index_join` | 6.40ms | 8.95ms | 1.398× | 1.85% | PASS |
| mem_reads | `index_join_scan` | 3.78ms | 5.27ms | 1.396× | 0.95% | PASS |
| mem_reads | `types_table_scan` | 1.08s | 1.23s | 1.137× | 1.08% | PASS |
| mem_reads | `table_scan` | 1.33s | 1.35s | 1.016× | 1.73% | PASS |
| mem_reads | `oltp_read_only` | 110.19ms | 119.82ms | 1.087× | 1.32% | PASS |
| mem_writes | `oltp_bulk_insert` | 195.57ms | 273.22ms | 1.397× | 0.44% | PASS |
| mem_writes | `oltp_insert` | 17.40ms | 32.34ms | 1.859× | 0.82% | PASS |
| mem_writes | `oltp_update_index` | 59.93ms | 114.29ms | 1.907× | 1.34% | PASS |
| mem_writes | `oltp_update_non_index` | 41.70ms | 71.06ms | 1.704× | 1.10% | PASS |
| mem_writes | `oltp_delete_insert` | 42.92ms | 89.06ms | 2.075× | 1.01% | PASS |
| mem_writes | `oltp_write_only` | 23.67ms | 52.66ms | 2.225× | 0.97% | PASS |
| mem_writes | `types_delete_insert` | 27.49ms | 46.03ms | 1.675× | 1.04% | PASS |
| mem_writes | `oltp_read_write` | 73.50ms | 119.43ms | 1.625× | 1.75% | PASS |
| file_reads | `oltp_point_select` | 60.30ms | 42.09ms | 0.698× | 0.87% | PASS |
| file_reads | `oltp_range_select` | 16.40ms | 14.26ms | 0.870× | 2.66% | PASS |
| file_reads | `oltp_sum_range` | 15.74ms | 13.57ms | 0.862× | 1.90% | PASS |
| file_reads | `oltp_order_range` | 3.30ms | 3.16ms | 0.960× | 1.28% | PASS |
| file_reads | `oltp_distinct_range` | 4.25ms | 4.10ms | 0.963× | 1.00% | PASS |
| file_reads | `oltp_index_scan` | 7.94ms | 6.75ms | 0.850× | 0.89% | PASS |
| file_reads | `select_random_points` | 21.12ms | 20.90ms | 0.990× | 0.66% | PASS |
| file_reads | `select_random_ranges` | 7.15ms | 5.87ms | 0.821× | 1.00% | PASS |
| file_reads | `covering_index_scan` | 7.71ms | 5.39ms | 0.699× | 1.26% | PASS |
| file_reads | `groupby_scan` | 32.72ms | 32.49ms | 0.993× | 0.67% | PASS |
| file_reads | `index_join` | 8.72ms | 9.95ms | 1.141× | 1.13% | PASS |
| file_reads | `index_join_scan` | 4.32ms | 5.59ms | 1.295× | 1.67% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.22s | 1.152× | 0.56% | PASS |
| file_reads | `table_scan` | 1.24s | 1.33s | 1.073× | 1.43% | PASS |
| file_reads | `oltp_read_only` | 157.48ms | 136.24ms | 0.865× | 0.97% | PASS |
| file_writes | `oltp_bulk_insert` | 261.50ms | 356.66ms | 1.364× | 1.55% | PASS |
| file_writes | `oltp_insert` | 47.19ms | 65.29ms | 1.383× | 2.24% | PASS |
| file_writes | `oltp_update_index` | 202.16ms | 217.89ms | 1.078× | 2.82% | PASS |
| file_writes | `oltp_update_non_index` | 156.34ms | 143.47ms | 0.918× | 0.77% | PASS |
| file_writes | `oltp_delete_insert` | 165.98ms | 173.47ms | 1.045× | 3.07% | PASS |
| file_writes | `oltp_write_only` | 121.40ms | 121.92ms | 1.004× | 2.00% | PASS |
| file_writes | `types_delete_insert` | 104.86ms | 99.03ms | 0.944× | 5.37% | PASS |
| file_writes | `oltp_read_write` | 172.37ms | 187.59ms | 1.088× | 1.29% | PASS |
| ac_reads | `oltp_point_select` | 37.60ms | 42.17ms | 1.122× | 0.80% | PASS |
| ac_reads | `oltp_range_select` | 13.84ms | 14.31ms | 1.033× | 0.92% | PASS |
| ac_reads | `oltp_sum_range` | 13.24ms | 13.60ms | 1.027× | 0.77% | PASS |
| ac_reads | `oltp_order_range` | 3.05ms | 3.16ms | 1.035× | 1.12% | PASS |
| ac_reads | `oltp_distinct_range` | 4.03ms | 4.11ms | 1.021× | 1.17% | PASS |
| ac_reads | `oltp_index_scan` | 5.74ms | 6.76ms | 1.178× | 0.89% | PASS |
| ac_reads | `select_random_points` | 19.08ms | 21.02ms | 1.102× | 1.19% | PASS |
| ac_reads | `select_random_ranges` | 4.91ms | 5.90ms | 1.202× | 1.57% | PASS |
| ac_reads | `covering_index_scan` | 5.42ms | 5.40ms | 0.996× | 0.95% | PASS |
| ac_reads | `groupby_scan` | 32.50ms | 32.54ms | 1.001× | 0.45% | PASS |
| ac_reads | `index_join` | 7.71ms | 10.01ms | 1.298× | 1.21% | PASS |
| ac_reads | `index_join_scan` | 4.17ms | 5.59ms | 1.342× | 1.24% | PASS |
| ac_reads | `types_table_scan` | 1.05s | 1.22s | 1.155× | 0.58% | PASS |
| ac_reads | `table_scan` | 1.28s | 1.34s | 1.048× | 1.72% | PASS |
| ac_reads | `oltp_read_only` | 126.50ms | 137.50ms | 1.087× | 0.99% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 28.56ms | 76.33ms | 2.672× | 4.46% | PASS |
| ac_writes | `oltp_insert_ac` | 30.56ms | 95.32ms | 3.119× | 5.79% | PASS |
| ac_writes | `oltp_update_index_ac` | 32.27ms | 103.34ms | 3.203× | 3.92% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 29.00ms | 85.93ms | 2.964× | 4.87% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 31.04ms | 98.72ms | 3.181× | 5.64% | PASS |
| ac_writes | `oltp_write_only_ac` | 31.14ms | 95.84ms | 3.077× | 6.11% | PASS |
| ac_writes | `types_delete_insert_ac` | 28.85ms | 89.99ms | 3.119× | 5.14% | PASS |
| ac_writes | `oltp_read_write_ac` | 35.49ms | 103.70ms | 2.922× | 3.40% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.92ms | 37.65ms | 1.110× | 2.06% | PASS |
| mem_reads | `oltp_range_select` | 19.99ms | 20.12ms | 1.006× | 1.80% | PASS |
| mem_reads | `oltp_sum_range` | 18.53ms | 19.08ms | 1.030× | 1.45% | PASS |
| mem_reads | `oltp_order_range` | 3.78ms | 3.79ms | 1.002× | 0.97% | PASS |
| mem_reads | `oltp_distinct_range` | 4.84ms | 4.84ms | 1.000× | 0.93% | PASS |
| mem_reads | `oltp_index_scan` | 4.72ms | 5.75ms | 1.220× | 1.59% | PASS |
| mem_reads | `select_random_points` | 27.82ms | 30.34ms | 1.090× | 1.58% | PASS |
| mem_reads | `select_random_ranges` | 7.64ms | 8.26ms | 1.080× | 1.02% | PASS |
| mem_reads | `covering_index_scan` | 4.39ms | 4.23ms | 0.965× | 1.66% | PASS |
| mem_reads | `groupby_scan` | 38.75ms | 40.32ms | 1.041× | 0.78% | PASS |
| mem_reads | `index_join` | 8.14ms | 10.04ms | 1.233× | 1.47% | PASS |
| mem_reads | `index_join_scan` | 4.27ms | 5.61ms | 1.314× | 1.51% | PASS |
| mem_reads | `types_table_scan` | 1.14s | 1.26s | 1.104× | 1.06% | PASS |
| mem_reads | `table_scan` | 1.37s | 1.37s | 1.001× | 2.28% | PASS |
| mem_reads | `oltp_read_only` | 152.26ms | 158.78ms | 1.043× | 1.28% | PASS |
| mem_writes | `oltp_bulk_insert` | 245.83ms | 338.04ms | 1.375× | 0.72% | PASS |
| mem_writes | `oltp_insert` | 19.50ms | 36.24ms | 1.858× | 0.76% | PASS |
| mem_writes | `oltp_update_index` | 69.40ms | 119.11ms | 1.716× | 1.43% | PASS |
| mem_writes | `oltp_update_non_index` | 52.64ms | 84.18ms | 1.599× | 1.96% | PASS |
| mem_writes | `oltp_delete_insert` | 50.55ms | 96.80ms | 1.915× | 1.43% | PASS |
| mem_writes | `oltp_write_only` | 27.53ms | 59.39ms | 2.158× | 0.98% | PASS |
| mem_writes | `types_delete_insert` | 32.66ms | 53.69ms | 1.644× | 1.30% | PASS |
| mem_writes | `oltp_read_write` | 99.47ms | 148.15ms | 1.489× | 1.45% | PASS |
| file_reads | `oltp_point_select` | 129.51ms | 69.91ms | 0.540× | 1.20% | PASS |
| file_reads | `oltp_range_select` | 29.48ms | 23.43ms | 0.795× | 3.08% | PASS |
| file_reads | `oltp_sum_range` | 28.08ms | 22.59ms | 0.804× | 1.61% | PASS |
| file_reads | `oltp_order_range` | 4.70ms | 4.17ms | 0.887× | 1.91% | PASS |
| file_reads | `oltp_distinct_range` | 5.74ms | 5.25ms | 0.914× | 1.80% | PASS |
| file_reads | `oltp_index_scan` | 14.26ms | 9.41ms | 0.660× | 1.27% | PASS |
| file_reads | `select_random_points` | 37.82ms | 33.94ms | 0.897× | 1.88% | PASS |
| file_reads | `select_random_ranges` | 17.33ms | 11.62ms | 0.671× | 1.17% | PASS |
| file_reads | `covering_index_scan` | 13.99ms | 7.78ms | 0.557× | 0.83% | PASS |
| file_reads | `groupby_scan` | 39.54ms | 40.88ms | 1.034× | 0.77% | PASS |
| file_reads | `index_join` | 13.29ms | 12.55ms | 0.944× | 1.18% | PASS |
| file_reads | `index_join_scan` | 5.35ms | 6.10ms | 1.140× | 1.89% | PASS |
| file_reads | `types_table_scan` | 1.15s | 1.26s | 1.100× | 2.02% | PASS |
| file_reads | `table_scan` | 1.38s | 1.37s | 0.994× | 5.18% | PASS |
| file_reads | `oltp_read_only` | 291.82ms | 206.85ms | 0.709× | 1.11% | PASS |
| file_writes | `oltp_bulk_insert` | 262.79ms | 361.82ms | 1.377× | 1.07% | PASS |
| file_writes | `oltp_insert` | 26.19ms | 46.64ms | 1.781× | 1.33% | PASS |
| file_writes | `oltp_update_index` | 102.32ms | 149.53ms | 1.461× | 1.91% | PASS |
| file_writes | `oltp_update_non_index` | 84.29ms | 111.15ms | 1.319× | 1.82% | PASS |
| file_writes | `oltp_delete_insert` | 82.36ms | 126.57ms | 1.537× | 2.18% | PASS |
| file_writes | `oltp_write_only` | 53.59ms | 83.19ms | 1.552× | 2.27% | PASS |
| file_writes | `types_delete_insert` | 51.26ms | 68.70ms | 1.340× | 1.68% | PASS |
| file_writes | `oltp_read_write` | 127.25ms | 172.60ms | 1.356× | 2.16% | PASS |
| ac_reads | `oltp_point_select` | 65.31ms | 70.41ms | 1.078× | 1.04% | PASS |
| ac_reads | `oltp_range_select` | 23.84ms | 23.50ms | 0.986× | 1.67% | PASS |
| ac_reads | `oltp_sum_range` | 22.40ms | 22.78ms | 1.017× | 1.35% | PASS |
| ac_reads | `oltp_order_range` | 4.27ms | 4.17ms | 0.977× | 1.06% | PASS |
| ac_reads | `oltp_distinct_range` | 5.33ms | 5.25ms | 0.984× | 0.86% | PASS |
| ac_reads | `oltp_index_scan` | 8.36ms | 9.43ms | 1.128× | 1.29% | PASS |
| ac_reads | `select_random_points` | 32.24ms | 34.25ms | 1.062× | 1.51% | PASS |
| ac_reads | `select_random_ranges` | 11.13ms | 11.67ms | 1.048× | 1.04% | PASS |
| ac_reads | `covering_index_scan` | 8.08ms | 7.87ms | 0.973× | 1.09% | PASS |
| ac_reads | `groupby_scan` | 39.41ms | 41.10ms | 1.043× | 0.72% | PASS |
| ac_reads | `index_join` | 10.54ms | 12.93ms | 1.227× | 1.39% | PASS |
| ac_reads | `index_join_scan` | 4.89ms | 6.17ms | 1.263× | 2.38% | PASS |
| ac_reads | `types_table_scan` | 1.16s | 1.26s | 1.086× | 1.37% | PASS |
| ac_reads | `table_scan` | 1.54s | 1.41s | 0.915× | 1.06% | PASS |
| ac_reads | `oltp_read_only` | 206.22ms | 209.84ms | 1.018× | 1.54% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.78ms | 55.65ms | 3.316× | 5.77% | PASS |
| ac_writes | `oltp_insert_ac` | 19.07ms | 77.25ms | 4.052× | 4.97% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.52ms | 88.23ms | 4.300× | 5.24% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.61ms | 66.90ms | 3.798× | 6.23% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.24ms | 78.17ms | 4.286× | 4.19% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.06ms | 77.77ms | 4.080× | 4.80% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.45ms | 69.03ms | 3.957× | 5.32% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.79ms | 86.97ms | 3.246× | 5.14% | PASS |

</details>

## Version-control latency

Wall time: 2m 3s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 68.05ms | 200.00ms | 34.0% | 0.43% | PASS |
| `status_dirty_many_tables` | 70.53ms | 200.00ms | 35.3% | 0.50% | PASS |
| `diff_regular_working_one_table` | 63.65ms | 150.00ms | 42.4% | 0.52% | PASS |
| `diff_regular_working_many_tables` | 76.17ms | 200.00ms | 38.1% | 0.57% | PASS |
| `diff_stat_working_many_tables` | 75.94ms | 200.00ms | 38.0% | 0.70% | PASS |
| `diff_schema_working_many_tables` | 75.72ms | 200.00ms | 37.9% | 0.47% | PASS |
| `branch_list_many_branches` | 20.96ms | 100.00ms | 21.0% | 1.50% | PASS |
| `branch_create_delete` | 23.16ms | 100.00ms | 23.2% | 1.58% | PASS |
| `checkout_branch_clean` | 97.85ms | 200.00ms | 48.9% | 0.32% | PASS |
| `merge_data_no_conflicts` | 36.38ms | 150.00ms | 24.3% | 0.91% | PASS |
| `merge_schema_no_conflicts` | 21.72ms | 100.00ms | 21.7% | 1.23% | PASS |
| `merge_data_conflicts` | 79.06ms | 250.00ms | 31.6% | 0.63% | PASS |
| `merge_data_conflicts_with_resolve` | 78.94ms | 250.00ms | 31.6% | 0.54% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
