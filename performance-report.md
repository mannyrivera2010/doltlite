# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-09 15:32 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260831.293.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/34359677597)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 27s | 9.48s | 11.14s | 1.175× | 1.24% | **PASS** |
| textpk | 69 | 55 | 1h 37m 59s | 11.21s | 17.06s | 1.522× | 2.88% | **PASS** |
| blobpk | 69 | 55 | 1h 29m 7s | 8.89s | 10.37s | 1.166× | 1.52% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 48s | 9.77s | 12.12s | 1.241× | 1.27% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.16ms | 28.03ms | 1.114× | 1.73% | PASS |
| mem_reads | `oltp_range_select` | 11.26ms | 12.07ms | 1.072× | 1.89% | PASS |
| mem_reads | `oltp_sum_range` | 10.39ms | 11.75ms | 1.131× | 2.20% | PASS |
| mem_reads | `oltp_order_range` | 2.76ms | 2.91ms | 1.054× | 1.05% | PASS |
| mem_reads | `oltp_distinct_range` | 3.90ms | 3.91ms | 1.002× | 1.14% | PASS |
| mem_reads | `oltp_index_scan` | 3.98ms | 4.93ms | 1.241× | 1.63% | PASS |
| mem_reads | `select_random_points` | 10.68ms | 11.51ms | 1.077× | 2.84% | PASS |
| mem_reads | `select_random_ranges` | 3.05ms | 3.93ms | 1.291× | 1.16% | PASS |
| mem_reads | `covering_index_scan` | 4.36ms | 4.00ms | 0.919× | 1.14% | PASS |
| mem_reads | `groupby_scan` | 31.61ms | 34.25ms | 1.083× | 0.75% | PASS |
| mem_reads | `index_join` | 5.93ms | 7.68ms | 1.297× | 0.93% | PASS |
| mem_reads | `index_join_scan` | 3.49ms | 4.67ms | 1.340× | 2.22% | PASS |
| mem_reads | `types_table_scan` | 1.11s | 1.26s | 1.138× | 0.47% | PASS |
| mem_reads | `table_scan` | 1.26s | 1.38s | 1.089× | 1.11% | PASS |
| mem_reads | `oltp_read_only` | 111.00ms | 117.00ms | 1.054× | 1.45% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.22ms | 241.13ms | 1.345× | 0.87% | PASS |
| mem_writes | `oltp_insert` | 15.80ms | 28.27ms | 1.788× | 0.77% | PASS |
| mem_writes | `oltp_update_index` | 52.33ms | 107.93ms | 2.062× | 1.33% | PASS |
| mem_writes | `oltp_update_non_index` | 36.40ms | 60.20ms | 1.654× | 1.72% | PASS |
| mem_writes | `oltp_delete_insert` | 45.87ms | 79.89ms | 1.742× | 1.60% | PASS |
| mem_writes | `oltp_write_only` | 22.33ms | 45.46ms | 2.036× | 1.65% | PASS |
| mem_writes | `types_delete_insert` | 24.55ms | 39.66ms | 1.615× | 0.76% | PASS |
| mem_writes | `oltp_read_write` | 64.61ms | 103.91ms | 1.608× | 0.76% | PASS |
| file_reads | `oltp_point_select` | 119.21ms | 59.11ms | 0.496× | 0.55% | PASS |
| file_reads | `oltp_range_select` | 19.25ms | 15.26ms | 0.793× | 0.83% | PASS |
| file_reads | `oltp_sum_range` | 18.75ms | 14.85ms | 0.792× | 1.71% | PASS |
| file_reads | `oltp_order_range` | 3.49ms | 3.24ms | 0.928× | 0.84% | PASS |
| file_reads | `oltp_distinct_range` | 4.64ms | 4.24ms | 0.913× | 0.89% | PASS |
| file_reads | `oltp_index_scan` | 13.27ms | 8.29ms | 0.625× | 1.00% | PASS |
| file_reads | `select_random_points` | 19.09ms | 14.85ms | 0.778× | 0.71% | PASS |
| file_reads | `select_random_ranges` | 12.36ms | 7.19ms | 0.582× | 0.81% | PASS |
| file_reads | `covering_index_scan` | 13.96ms | 7.42ms | 0.531× | 1.24% | PASS |
| file_reads | `groupby_scan` | 32.49ms | 34.81ms | 1.071× | 1.04% | PASS |
| file_reads | `index_join` | 11.03ms | 10.00ms | 0.907× | 1.31% | PASS |
| file_reads | `index_join_scan` | 4.54ms | 5.30ms | 1.165× | 1.79% | PASS |
| file_reads | `types_table_scan` | 1.10s | 1.26s | 1.149× | 0.55% | PASS |
| file_reads | `table_scan` | 1.25s | 1.37s | 1.096× | 0.49% | PASS |
| file_reads | `oltp_read_only` | 240.68ms | 159.90ms | 0.664× | 0.59% | PASS |
| file_writes | `oltp_bulk_insert` | 194.12ms | 260.16ms | 1.340× | 1.18% | PASS |
| file_writes | `oltp_insert` | 22.09ms | 35.92ms | 1.627× | 1.18% | PASS |
| file_writes | `oltp_update_index` | 78.23ms | 129.47ms | 1.655× | 0.87% | PASS |
| file_writes | `oltp_update_non_index` | 58.84ms | 82.36ms | 1.400× | 1.26% | PASS |
| file_writes | `oltp_delete_insert` | 67.44ms | 98.82ms | 1.465× | 1.29% | PASS |
| file_writes | `oltp_write_only` | 43.68ms | 66.14ms | 1.514× | 2.59% | PASS |
| file_writes | `types_delete_insert` | 40.34ms | 53.67ms | 1.331× | 1.02% | PASS |
| file_writes | `oltp_read_write` | 89.69ms | 126.97ms | 1.416× | 1.96% | PASS |
| ac_reads | `oltp_point_select` | 55.67ms | 59.30ms | 1.065× | 1.17% | PASS |
| ac_reads | `oltp_range_select` | 14.32ms | 15.37ms | 1.073× | 2.60% | PASS |
| ac_reads | `oltp_sum_range` | 12.86ms | 14.88ms | 1.157× | 2.21% | PASS |
| ac_reads | `oltp_order_range` | 3.15ms | 3.27ms | 1.037× | 1.23% | PASS |
| ac_reads | `oltp_distinct_range` | 4.26ms | 4.27ms | 1.002× | 1.05% | PASS |
| ac_reads | `oltp_index_scan` | 7.51ms | 8.67ms | 1.153× | 1.56% | PASS |
| ac_reads | `select_random_points` | 15.87ms | 15.64ms | 0.985× | 2.97% | PASS |
| ac_reads | `select_random_ranges` | 6.51ms | 7.28ms | 1.119× | 0.92% | PASS |
| ac_reads | `covering_index_scan` | 8.15ms | 7.87ms | 0.966× | 1.61% | PASS |
| ac_reads | `groupby_scan` | 32.62ms | 35.04ms | 1.074× | 0.70% | PASS |
| ac_reads | `index_join` | 7.91ms | 10.19ms | 1.288× | 2.13% | PASS |
| ac_reads | `index_join_scan` | 3.99ms | 5.26ms | 1.317× | 2.26% | PASS |
| ac_reads | `types_table_scan` | 1.10s | 1.26s | 1.147× | 0.80% | PASS |
| ac_reads | `table_scan` | 1.41s | 1.42s | 1.010× | 0.56% | PASS |
| ac_reads | `oltp_read_only` | 156.51ms | 163.00ms | 1.041× | 2.14% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.99ms | 65.22ms | 4.079× | 4.62% | PASS |
| ac_writes | `oltp_insert_ac` | 17.84ms | 82.07ms | 4.601× | 4.57% | PASS |
| ac_writes | `oltp_update_index_ac` | 19.64ms | 97.21ms | 4.950× | 4.02% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.72ms | 79.08ms | 4.729× | 9.15% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.85ms | 94.16ms | 4.994× | 4.75% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.95ms | 91.39ms | 4.822× | 5.03% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.14ms | 80.22ms | 4.680× | 6.75% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.45ms | 99.78ms | 4.082× | 4.09% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 19.55ms | 22.69ms | 1.160× | 2.82% | PASS |
| mem_reads | `oltp_range_select` | 10.17ms | 9.32ms | 0.916× | 3.84% | PASS |
| mem_reads | `oltp_sum_range` | 9.79ms | 8.98ms | 0.917× | 4.10% | PASS |
| mem_reads | `oltp_order_range` | 2.17ms | 2.10ms | 0.969× | 2.60% | PASS |
| mem_reads | `oltp_distinct_range` | 2.84ms | 2.81ms | 0.988× | 1.57% | PASS |
| mem_reads | `oltp_index_scan` | 3.22ms | 3.98ms | 1.236× | 3.57% | PASS |
| mem_reads | `select_random_points` | 13.90ms | 15.99ms | 1.150× | 1.72% | PASS |
| mem_reads | `select_random_ranges` | 2.92ms | 3.44ms | 1.180× | 2.50% | PASS |
| mem_reads | `covering_index_scan` | 2.87ms | 2.61ms | 0.908× | 1.81% | PASS |
| mem_reads | `groupby_scan` | 20.78ms | 21.87ms | 1.053× | 1.00% | PASS |
| mem_reads | `index_join` | 5.31ms | 6.54ms | 1.231× | 1.51% | PASS |
| mem_reads | `index_join_scan` | 3.80ms | 4.69ms | 1.236× | 4.40% | PASS |
| mem_reads | `types_table_scan` | 767.30ms | 843.39ms | 1.099× | 2.44% | PASS |
| mem_reads | `table_scan` | 898.44ms | 920.80ms | 1.025× | 3.87% | PASS |
| mem_reads | `oltp_read_only` | 76.78ms | 83.48ms | 1.087× | 1.74% | PASS |
| mem_writes | `oltp_bulk_insert` | 130.37ms | 195.58ms | 1.500× | 1.04% | PASS |
| mem_writes | `oltp_insert` | 13.57ms | 23.14ms | 1.705× | 1.71% | PASS |
| mem_writes | `oltp_update_index` | 48.24ms | 86.71ms | 1.797× | 1.99% | PASS |
| mem_writes | `oltp_update_non_index` | 31.66ms | 53.56ms | 1.692× | 2.41% | PASS |
| mem_writes | `oltp_delete_insert` | 35.14ms | 67.40ms | 1.918× | 2.18% | PASS |
| mem_writes | `oltp_write_only` | 20.17ms | 39.39ms | 1.953× | 2.16% | PASS |
| mem_writes | `types_delete_insert` | 21.39ms | 34.94ms | 1.633× | 3.43% | PASS |
| mem_writes | `oltp_read_write` | 56.67ms | 87.95ms | 1.552× | 2.19% | PASS |
| file_reads | `oltp_point_select` | 43.23ms | 31.74ms | 0.734× | 2.61% | PASS |
| file_reads | `oltp_range_select` | 13.54ms | 10.82ms | 0.799× | 2.67% | PASS |
| file_reads | `oltp_sum_range` | 12.98ms | 10.49ms | 0.808× | 1.89% | PASS |
| file_reads | `oltp_order_range` | 2.60ms | 2.36ms | 0.909× | 4.34% | PASS |
| file_reads | `oltp_distinct_range` | 3.19ms | 2.96ms | 0.927× | 3.03% | PASS |
| file_reads | `oltp_index_scan` | 5.84ms | 5.39ms | 0.923× | 2.52% | PASS |
| file_reads | `select_random_points` | 16.77ms | 17.51ms | 1.044× | 1.93% | PASS |
| file_reads | `select_random_ranges` | 5.33ms | 4.48ms | 0.840× | 2.43% | PASS |
| file_reads | `covering_index_scan` | 6.59ms | 4.19ms | 0.636× | 2.43% | PASS |
| file_reads | `groupby_scan` | 21.55ms | 22.74ms | 1.055× | 2.03% | PASS |
| file_reads | `index_join` | 7.84ms | 8.28ms | 1.056× | 3.88% | PASS |
| file_reads | `index_join_scan` | 4.55ms | 5.27ms | 1.156× | 3.25% | PASS |
| file_reads | `types_table_scan` | 775.53ms | 857.28ms | 1.105× | 2.28% | PASS |
| file_reads | `table_scan` | 907.16ms | 931.11ms | 1.026× | 4.10% | PASS |
| file_reads | `oltp_read_only` | 114.38ms | 99.31ms | 0.868× | 1.34% | PASS |
| file_writes | `oltp_bulk_insert` | 326.55ms | 325.68ms | 0.997× | 31.96% | PASS |
| file_writes | `oltp_insert` | 175.73ms | 72.82ms | 0.414× | 68.77% | PASS |
| file_writes | `oltp_update_index` | 293.39ms | 226.43ms | 0.772× | 25.89% | PASS |
| file_writes | `oltp_update_non_index` | 280.43ms | 150.75ms | 0.538× | 35.20% | PASS |
| file_writes | `oltp_delete_insert` | 224.41ms | 171.71ms | 0.765× | 25.95% | PASS |
| file_writes | `oltp_write_only` | 283.24ms | 118.50ms | 0.418× | 48.55% | PASS |
| file_writes | `types_delete_insert` | 171.86ms | 110.74ms | 0.644× | 46.22% | PASS |
| file_writes | `oltp_read_write` | 315.98ms | 184.84ms | 0.585× | 33.70% | PASS |
| ac_reads | `oltp_point_select` | 26.58ms | 30.77ms | 1.158× | 1.80% | PASS |
| ac_reads | `oltp_range_select` | 12.31ms | 10.86ms | 0.882× | 2.54% | PASS |
| ac_reads | `oltp_sum_range` | 11.67ms | 10.53ms | 0.902× | 2.88% | PASS |
| ac_reads | `oltp_order_range` | 2.45ms | 2.34ms | 0.957× | 4.40% | PASS |
| ac_reads | `oltp_distinct_range` | 3.16ms | 3.02ms | 0.956× | 3.01% | PASS |
| ac_reads | `oltp_index_scan` | 4.70ms | 5.46ms | 1.162× | 3.55% | PASS |
| ac_reads | `select_random_points` | 15.57ms | 17.37ms | 1.115× | 2.66% | PASS |
| ac_reads | `select_random_ranges` | 3.95ms | 4.35ms | 1.102× | 2.90% | PASS |
| ac_reads | `covering_index_scan` | 4.89ms | 4.07ms | 0.831× | 3.14% | PASS |
| ac_reads | `groupby_scan` | 21.25ms | 22.30ms | 1.049× | 0.87% | PASS |
| ac_reads | `index_join` | 7.05ms | 8.23ms | 1.167× | 3.64% | PASS |
| ac_reads | `index_join_scan` | 4.43ms | 5.20ms | 1.175× | 2.54% | PASS |
| ac_reads | `types_table_scan` | 772.93ms | 851.31ms | 1.101× | 2.76% | PASS |
| ac_reads | `table_scan` | 905.04ms | 918.44ms | 1.015× | 3.30% | PASS |
| ac_reads | `oltp_read_only` | 88.91ms | 95.36ms | 1.073× | 1.62% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 400.46ms | 955.29ms | 2.385× | 56.63% | PASS |
| ac_writes | `oltp_insert_ac` | 379.69ms | 1.31s | 3.447× | 61.05% | PASS |
| ac_writes | `oltp_update_index_ac` | 255.97ms | 844.63ms | 3.300× | 66.78% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 389.45ms | 1.33s | 3.425× | 56.65% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 334.62ms | 939.42ms | 2.807× | 47.93% | PASS |
| ac_writes | `oltp_write_only_ac` | 394.24ms | 1.23s | 3.131× | 53.69% | PASS |
| ac_writes | `types_delete_insert_ac` | 420.44ms | 1.30s | 3.081× | 52.71% | PASS |
| ac_writes | `oltp_read_write_ac` | 538.12ms | 1.24s | 2.308× | 44.08% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 23.28ms | 26.96ms | 1.158× | 1.44% | PASS |
| mem_reads | `oltp_range_select` | 10.35ms | 10.66ms | 1.030× | 2.50% | PASS |
| mem_reads | `oltp_sum_range` | 10.16ms | 10.63ms | 1.046× | 1.34% | PASS |
| mem_reads | `oltp_order_range` | 2.53ms | 2.53ms | 0.999× | 1.83% | PASS |
| mem_reads | `oltp_distinct_range` | 3.38ms | 3.31ms | 0.977× | 0.76% | PASS |
| mem_reads | `oltp_index_scan` | 3.81ms | 4.89ms | 1.282× | 2.54% | PASS |
| mem_reads | `select_random_points` | 14.13ms | 16.01ms | 1.133× | 2.12% | PASS |
| mem_reads | `select_random_ranges` | 3.41ms | 4.21ms | 1.233× | 2.73% | PASS |
| mem_reads | `covering_index_scan` | 3.55ms | 3.45ms | 0.974× | 1.21% | PASS |
| mem_reads | `groupby_scan` | 26.11ms | 27.39ms | 1.049× | 0.44% | PASS |
| mem_reads | `index_join` | 5.48ms | 6.80ms | 1.242× | 1.22% | PASS |
| mem_reads | `index_join_scan` | 3.62ms | 4.62ms | 1.276× | 1.54% | PASS |
| mem_reads | `types_table_scan` | 871.00ms | 965.47ms | 1.108× | 0.74% | PASS |
| mem_reads | `table_scan` | 1.04s | 1.06s | 1.021× | 2.73% | PASS |
| mem_reads | `oltp_read_only` | 92.73ms | 100.07ms | 1.079× | 3.52% | PASS |
| mem_writes | `oltp_bulk_insert` | 183.66ms | 255.17ms | 1.389× | 0.98% | PASS |
| mem_writes | `oltp_insert` | 15.93ms | 29.33ms | 1.841× | 0.78% | PASS |
| mem_writes | `oltp_update_index` | 55.73ms | 100.61ms | 1.805× | 1.55% | PASS |
| mem_writes | `oltp_update_non_index` | 39.61ms | 64.44ms | 1.627× | 1.18% | PASS |
| mem_writes | `oltp_delete_insert` | 39.82ms | 79.20ms | 1.989× | 1.27% | PASS |
| mem_writes | `oltp_write_only` | 22.67ms | 47.37ms | 2.090× | 1.19% | PASS |
| mem_writes | `types_delete_insert` | 25.35ms | 39.52ms | 1.559× | 1.22% | PASS |
| mem_writes | `oltp_read_write` | 66.88ms | 104.47ms | 1.562× | 1.61% | PASS |
| file_reads | `oltp_point_select` | 98.47ms | 51.99ms | 0.528× | 0.66% | PASS |
| file_reads | `oltp_range_select` | 19.00ms | 13.27ms | 0.699× | 1.83% | PASS |
| file_reads | `oltp_sum_range` | 17.66ms | 13.05ms | 0.739× | 1.00% | PASS |
| file_reads | `oltp_order_range` | 3.33ms | 2.81ms | 0.844× | 1.22% | PASS |
| file_reads | `oltp_distinct_range` | 4.18ms | 3.58ms | 0.855× | 0.60% | PASS |
| file_reads | `oltp_index_scan` | 11.64ms | 7.54ms | 0.647× | 0.78% | PASS |
| file_reads | `select_random_points` | 22.06ms | 18.49ms | 0.838× | 1.20% | PASS |
| file_reads | `select_random_ranges` | 11.08ms | 6.72ms | 0.607× | 0.82% | PASS |
| file_reads | `covering_index_scan` | 12.08ms | 6.37ms | 0.528× | 0.77% | PASS |
| file_reads | `groupby_scan` | 27.41ms | 27.81ms | 1.015× | 0.98% | PASS |
| file_reads | `index_join` | 10.10ms | 9.13ms | 0.904× | 1.52% | PASS |
| file_reads | `index_join_scan` | 4.57ms | 5.08ms | 1.111× | 1.92% | PASS |
| file_reads | `types_table_scan` | 879.81ms | 961.90ms | 1.093× | 0.62% | PASS |
| file_reads | `table_scan` | 1.01s | 1.05s | 1.041× | 0.59% | PASS |
| file_reads | `oltp_read_only` | 203.78ms | 135.64ms | 0.666× | 0.95% | PASS |
| file_writes | `oltp_bulk_insert` | 259.58ms | 349.07ms | 1.345× | 11.75% | PASS |
| file_writes | `oltp_insert` | 61.56ms | 63.60ms | 1.033× | 37.07% | PASS |
| file_writes | `oltp_update_index` | 206.07ms | 215.34ms | 1.045× | 12.79% | PASS |
| file_writes | `oltp_update_non_index` | 178.57ms | 136.27ms | 0.763× | 15.71% | PASS |
| file_writes | `oltp_delete_insert` | 169.42ms | 162.61ms | 0.960× | 17.83% | PASS |
| file_writes | `oltp_write_only` | 136.77ms | 117.67ms | 0.860× | 18.71% | PASS |
| file_writes | `types_delete_insert` | 98.81ms | 94.18ms | 0.953× | 21.34% | PASS |
| file_writes | `oltp_read_write` | 174.64ms | 171.76ms | 0.984× | 21.70% | PASS |
| ac_reads | `oltp_point_select` | 48.67ms | 52.28ms | 1.074× | 0.85% | PASS |
| ac_reads | `oltp_range_select` | 14.21ms | 13.36ms | 0.940× | 2.45% | PASS |
| ac_reads | `oltp_sum_range` | 13.21ms | 13.12ms | 0.994× | 1.65% | PASS |
| ac_reads | `oltp_order_range` | 2.94ms | 2.81ms | 0.957× | 0.76% | PASS |
| ac_reads | `oltp_distinct_range` | 3.73ms | 3.60ms | 0.964× | 0.74% | PASS |
| ac_reads | `oltp_index_scan` | 6.77ms | 7.58ms | 1.120× | 1.21% | PASS |
| ac_reads | `select_random_points` | 17.44ms | 18.69ms | 1.072× | 1.52% | PASS |
| ac_reads | `select_random_ranges` | 6.20ms | 6.76ms | 1.089× | 0.98% | PASS |
| ac_reads | `covering_index_scan` | 7.14ms | 6.38ms | 0.894× | 1.11% | PASS |
| ac_reads | `groupby_scan` | 27.11ms | 27.91ms | 1.030× | 0.79% | PASS |
| ac_reads | `index_join` | 7.74ms | 9.25ms | 1.195× | 2.27% | PASS |
| ac_reads | `index_join_scan` | 4.21ms | 5.12ms | 1.214× | 1.86% | PASS |
| ac_reads | `types_table_scan` | 1.00s | 993.67ms | 0.989× | 6.87% | PASS |
| ac_reads | `table_scan` | 1.08s | 1.07s | 0.994× | 6.09% | PASS |
| ac_reads | `oltp_read_only` | 128.26ms | 135.15ms | 1.054× | 0.78% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 44.80ms | 125.38ms | 2.798× | 59.32% | PASS |
| ac_writes | `oltp_insert_ac` | 42.80ms | 190.59ms | 4.453× | 51.42% | PASS |
| ac_writes | `oltp_update_index_ac` | 64.28ms | 194.89ms | 3.032× | 45.55% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 40.25ms | 160.94ms | 3.998× | 50.28% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 40.03ms | 202.67ms | 5.063× | 47.61% | PASS |
| ac_writes | `oltp_write_only_ac` | 44.03ms | 211.84ms | 4.811× | 69.14% | PASS |
| ac_writes | `types_delete_insert_ac` | 35.65ms | 176.96ms | 4.964× | 65.27% | PASS |
| ac_writes | `oltp_read_write_ac` | 39.94ms | 155.59ms | 3.895× | 36.50% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 33.63ms | 40.91ms | 1.216× | 1.26% | PASS |
| mem_reads | `oltp_range_select` | 19.47ms | 21.20ms | 1.089× | 1.63% | PASS |
| mem_reads | `oltp_sum_range` | 18.34ms | 21.11ms | 1.151× | 1.27% | PASS |
| mem_reads | `oltp_order_range` | 3.60ms | 3.86ms | 1.073× | 0.70% | PASS |
| mem_reads | `oltp_distinct_range` | 4.73ms | 4.92ms | 1.038× | 1.13% | PASS |
| mem_reads | `oltp_index_scan` | 4.68ms | 6.30ms | 1.346× | 1.91% | PASS |
| mem_reads | `select_random_points` | 28.26ms | 32.32ms | 1.144× | 1.14% | PASS |
| mem_reads | `select_random_ranges` | 7.85ms | 9.12ms | 1.163× | 1.05% | PASS |
| mem_reads | `covering_index_scan` | 4.25ms | 4.35ms | 1.023× | 2.32% | PASS |
| mem_reads | `groupby_scan` | 36.62ms | 38.70ms | 1.057× | 0.63% | PASS |
| mem_reads | `index_join` | 8.23ms | 10.62ms | 1.290× | 1.52% | PASS |
| mem_reads | `index_join_scan` | 4.23ms | 5.50ms | 1.300× | 1.74% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.25s | 1.184× | 0.95% | PASS |
| mem_reads | `table_scan` | 1.23s | 1.40s | 1.141× | 1.03% | PASS |
| mem_reads | `oltp_read_only` | 153.53ms | 170.56ms | 1.111× | 1.28% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.09ms | 360.03ms | 1.445× | 1.14% | PASS |
| mem_writes | `oltp_insert` | 19.35ms | 36.77ms | 1.900× | 0.93% | PASS |
| mem_writes | `oltp_update_index` | 69.40ms | 118.91ms | 1.713× | 1.11% | PASS |
| mem_writes | `oltp_update_non_index` | 51.52ms | 84.40ms | 1.638× | 1.51% | PASS |
| mem_writes | `oltp_delete_insert` | 50.76ms | 97.38ms | 1.918× | 0.88% | PASS |
| mem_writes | `oltp_write_only` | 27.66ms | 58.75ms | 2.124× | 1.25% | PASS |
| mem_writes | `types_delete_insert` | 33.40ms | 55.82ms | 1.671× | 1.94% | PASS |
| mem_writes | `oltp_read_write` | 105.20ms | 158.67ms | 1.508× | 1.12% | PASS |
| file_reads | `oltp_point_select` | 109.61ms | 67.15ms | 0.613× | 0.95% | PASS |
| file_reads | `oltp_range_select` | 27.09ms | 24.26ms | 0.895× | 1.81% | PASS |
| file_reads | `oltp_sum_range` | 26.25ms | 24.10ms | 0.918× | 1.44% | PASS |
| file_reads | `oltp_order_range` | 4.47ms | 4.31ms | 0.964× | 2.14% | PASS |
| file_reads | `oltp_distinct_range` | 5.65ms | 5.39ms | 0.954× | 1.66% | PASS |
| file_reads | `oltp_index_scan` | 12.49ms | 9.25ms | 0.741× | 1.30% | PASS |
| file_reads | `select_random_points` | 37.74ms | 36.31ms | 0.962× | 1.11% | PASS |
| file_reads | `select_random_ranges` | 15.78ms | 12.07ms | 0.765× | 1.25% | PASS |
| file_reads | `covering_index_scan` | 11.96ms | 7.29ms | 0.609× | 1.15% | PASS |
| file_reads | `groupby_scan` | 37.53ms | 39.53ms | 1.053× | 1.09% | PASS |
| file_reads | `index_join` | 12.55ms | 13.01ms | 1.037× | 1.31% | PASS |
| file_reads | `index_join_scan` | 5.22ms | 6.06ms | 1.163× | 1.61% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.24s | 1.176× | 0.99% | PASS |
| file_reads | `table_scan` | 1.23s | 1.40s | 1.136× | 1.33% | PASS |
| file_reads | `oltp_read_only` | 269.01ms | 211.38ms | 0.786× | 0.81% | PASS |
| file_writes | `oltp_bulk_insert` | 264.30ms | 382.72ms | 1.448× | 0.94% | PASS |
| file_writes | `oltp_insert` | 26.36ms | 46.96ms | 1.781× | 1.80% | PASS |
| file_writes | `oltp_update_index` | 99.32ms | 145.18ms | 1.462× | 1.20% | PASS |
| file_writes | `oltp_update_non_index` | 78.11ms | 106.26ms | 1.360× | 1.39% | PASS |
| file_writes | `oltp_delete_insert` | 78.01ms | 121.64ms | 1.559× | 1.17% | PASS |
| file_writes | `oltp_write_only` | 51.96ms | 78.85ms | 1.518× | 2.11% | PASS |
| file_writes | `types_delete_insert` | 50.59ms | 69.76ms | 1.379× | 1.77% | PASS |
| file_writes | `oltp_read_write` | 130.28ms | 179.14ms | 1.375× | 1.20% | PASS |
| ac_reads | `oltp_point_select` | 59.60ms | 67.17ms | 1.127× | 0.91% | PASS |
| ac_reads | `oltp_range_select` | 23.05ms | 24.68ms | 1.071× | 1.16% | PASS |
| ac_reads | `oltp_sum_range` | 21.29ms | 24.21ms | 1.137× | 1.27% | PASS |
| ac_reads | `oltp_order_range` | 4.34ms | 4.49ms | 1.035× | 2.21% | PASS |
| ac_reads | `oltp_distinct_range` | 5.49ms | 5.63ms | 1.027× | 1.78% | PASS |
| ac_reads | `oltp_index_scan` | 7.72ms | 9.34ms | 1.210× | 1.67% | PASS |
| ac_reads | `select_random_points` | 32.04ms | 36.31ms | 1.133× | 1.10% | PASS |
| ac_reads | `select_random_ranges` | 10.70ms | 11.99ms | 1.121× | 0.79% | PASS |
| ac_reads | `covering_index_scan` | 7.26ms | 7.27ms | 1.002× | 1.36% | PASS |
| ac_reads | `groupby_scan` | 37.19ms | 39.60ms | 1.065× | 0.99% | PASS |
| ac_reads | `index_join` | 10.14ms | 13.00ms | 1.282× | 1.15% | PASS |
| ac_reads | `index_join_scan` | 4.77ms | 6.12ms | 1.284× | 1.78% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.23s | 1.184× | 1.09% | PASS |
| ac_reads | `table_scan` | 1.24s | 1.40s | 1.126× | 1.09% | PASS |
| ac_reads | `oltp_read_only` | 195.62ms | 212.88ms | 1.088× | 1.25% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 27.75ms | 95.88ms | 3.456× | 19.33% | PASS |
| ac_writes | `oltp_insert_ac` | 24.52ms | 100.13ms | 4.083× | 5.71% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.08ms | 111.83ms | 4.287× | 4.25% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.05ms | 90.39ms | 3.758× | 6.66% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.39ms | 101.87ms | 4.176× | 5.02% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.33ms | 102.95ms | 4.064× | 7.01% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.93ms | 92.08ms | 4.015× | 4.78% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.44ms | 110.30ms | 3.509× | 4.05% | PASS |

</details>

## Version-control latency

Wall time: 2m 19s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.29ms | 200.00ms | 41.6% | 0.99% | PASS |
| `status_dirty_many_tables` | 85.65ms | 200.00ms | 42.8% | 0.64% | PASS |
| `diff_regular_working_one_table` | 78.93ms | 150.00ms | 52.6% | 0.89% | PASS |
| `diff_regular_working_many_tables` | 91.77ms | 200.00ms | 45.9% | 0.42% | PASS |
| `diff_stat_working_many_tables` | 91.72ms | 200.00ms | 45.9% | 0.43% | PASS |
| `diff_schema_working_many_tables` | 91.77ms | 200.00ms | 45.9% | 0.72% | PASS |
| `branch_list_many_branches` | 22.50ms | 100.00ms | 22.5% | 1.16% | PASS |
| `branch_create_delete` | 24.59ms | 100.00ms | 24.6% | 1.28% | PASS |
| `checkout_branch_clean` | 54.97ms | 200.00ms | 27.5% | 0.91% | PASS |
| `merge_data_no_conflicts` | 29.17ms | 150.00ms | 19.4% | 2.27% | PASS |
| `merge_schema_no_conflicts` | 22.09ms | 100.00ms | 22.1% | 1.52% | PASS |
| `merge_data_conflicts` | 127.36ms | 250.00ms | 50.9% | 0.38% | PASS |
| `merge_data_conflicts_with_resolve` | 128.03ms | 250.00ms | 51.2% | 0.34% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
