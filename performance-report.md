# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-12 14:39 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260907.300.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/34695325079)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 25s | 9.42s | 11.11s | 1.180× | 1.30% | **PASS** |
| textpk | 69 | 55 | 1h 33m 31s | 10.44s | 12.00s | 1.150× | 1.82% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 8s | 9.33s | 11.72s | 1.256× | 1.58% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 2s | 9.64s | 12.06s | 1.250× | 1.31% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.20ms | 27.80ms | 1.103× | 1.40% | PASS |
| mem_reads | `oltp_range_select` | 11.55ms | 12.16ms | 1.053× | 2.90% | PASS |
| mem_reads | `oltp_sum_range` | 10.31ms | 11.69ms | 1.134× | 2.17% | PASS |
| mem_reads | `oltp_order_range` | 2.73ms | 2.91ms | 1.066× | 1.20% | PASS |
| mem_reads | `oltp_distinct_range` | 3.80ms | 3.92ms | 1.031× | 1.13% | PASS |
| mem_reads | `oltp_index_scan` | 4.11ms | 5.32ms | 1.295× | 1.91% | PASS |
| mem_reads | `select_random_points` | 11.39ms | 11.69ms | 1.026× | 2.81% | PASS |
| mem_reads | `select_random_ranges` | 3.15ms | 4.06ms | 1.290× | 1.70% | PASS |
| mem_reads | `covering_index_scan` | 4.36ms | 4.10ms | 0.940× | 1.59% | PASS |
| mem_reads | `groupby_scan` | 31.96ms | 34.39ms | 1.076× | 0.60% | PASS |
| mem_reads | `index_join` | 5.98ms | 8.30ms | 1.387× | 2.89% | PASS |
| mem_reads | `index_join_scan` | 3.52ms | 4.76ms | 1.352× | 1.98% | PASS |
| mem_reads | `types_table_scan` | 1.13s | 1.28s | 1.129× | 1.57% | PASS |
| mem_reads | `table_scan` | 1.30s | 1.39s | 1.069× | 2.05% | PASS |
| mem_reads | `oltp_read_only` | 103.69ms | 114.97ms | 1.109× | 1.19% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.38ms | 240.25ms | 1.339× | 0.94% | PASS |
| mem_writes | `oltp_insert` | 15.77ms | 28.20ms | 1.788× | 0.71% | PASS |
| mem_writes | `oltp_update_index` | 51.40ms | 105.80ms | 2.059× | 1.20% | PASS |
| mem_writes | `oltp_update_non_index` | 35.73ms | 59.55ms | 1.667× | 1.12% | PASS |
| mem_writes | `oltp_delete_insert` | 45.66ms | 79.42ms | 1.739× | 0.86% | PASS |
| mem_writes | `oltp_write_only` | 22.46ms | 46.10ms | 2.053× | 1.13% | PASS |
| mem_writes | `types_delete_insert` | 24.42ms | 39.66ms | 1.624× | 0.87% | PASS |
| mem_writes | `oltp_read_write` | 64.58ms | 104.10ms | 1.612× | 0.77% | PASS |
| file_reads | `oltp_point_select` | 119.51ms | 59.05ms | 0.494× | 0.59% | PASS |
| file_reads | `oltp_range_select` | 19.51ms | 15.06ms | 0.772× | 1.30% | PASS |
| file_reads | `oltp_sum_range` | 18.93ms | 14.65ms | 0.774× | 1.11% | PASS |
| file_reads | `oltp_order_range` | 3.62ms | 3.25ms | 0.899× | 2.10% | PASS |
| file_reads | `oltp_distinct_range` | 4.61ms | 4.22ms | 0.916× | 1.30% | PASS |
| file_reads | `oltp_index_scan` | 13.38ms | 8.38ms | 0.626× | 1.32% | PASS |
| file_reads | `select_random_points` | 19.52ms | 14.49ms | 0.742× | 1.60% | PASS |
| file_reads | `select_random_ranges` | 12.46ms | 7.18ms | 0.576× | 0.94% | PASS |
| file_reads | `covering_index_scan` | 13.97ms | 7.53ms | 0.539× | 1.03% | PASS |
| file_reads | `groupby_scan` | 32.74ms | 34.73ms | 1.061× | 0.58% | PASS |
| file_reads | `index_join` | 11.01ms | 10.06ms | 0.914× | 1.15% | PASS |
| file_reads | `index_join_scan` | 4.54ms | 5.09ms | 1.122× | 1.47% | PASS |
| file_reads | `types_table_scan` | 1.10s | 1.27s | 1.148× | 0.57% | PASS |
| file_reads | `table_scan` | 1.27s | 1.38s | 1.086× | 1.19% | PASS |
| file_reads | `oltp_read_only` | 241.81ms | 160.75ms | 0.665× | 0.96% | PASS |
| file_writes | `oltp_bulk_insert` | 195.82ms | 260.33ms | 1.329× | 1.13% | PASS |
| file_writes | `oltp_insert` | 22.26ms | 36.16ms | 1.624× | 1.31% | PASS |
| file_writes | `oltp_update_index` | 79.99ms | 130.44ms | 1.631× | 1.33% | PASS |
| file_writes | `oltp_update_non_index` | 59.98ms | 82.77ms | 1.380× | 1.53% | PASS |
| file_writes | `oltp_delete_insert` | 69.19ms | 99.90ms | 1.444× | 1.81% | PASS |
| file_writes | `oltp_write_only` | 45.34ms | 66.87ms | 1.475× | 2.17% | PASS |
| file_writes | `types_delete_insert` | 41.22ms | 54.17ms | 1.314× | 1.72% | PASS |
| file_writes | `oltp_read_write` | 88.14ms | 125.61ms | 1.425× | 1.35% | PASS |
| ac_reads | `oltp_point_select` | 55.48ms | 59.01ms | 1.064× | 0.75% | PASS |
| ac_reads | `oltp_range_select` | 14.15ms | 15.11ms | 1.068× | 1.10% | PASS |
| ac_reads | `oltp_sum_range` | 13.02ms | 14.66ms | 1.126× | 1.14% | PASS |
| ac_reads | `oltp_order_range` | 3.07ms | 3.23ms | 1.053× | 1.50% | PASS |
| ac_reads | `oltp_distinct_range` | 4.09ms | 4.22ms | 1.032× | 0.75% | PASS |
| ac_reads | `oltp_index_scan` | 7.25ms | 8.37ms | 1.154× | 1.31% | PASS |
| ac_reads | `select_random_points` | 13.91ms | 14.52ms | 1.044× | 0.74% | PASS |
| ac_reads | `select_random_ranges` | 6.23ms | 7.16ms | 1.149× | 1.19% | PASS |
| ac_reads | `covering_index_scan` | 7.67ms | 7.48ms | 0.975× | 0.99% | PASS |
| ac_reads | `groupby_scan` | 32.09ms | 34.69ms | 1.081× | 0.92% | PASS |
| ac_reads | `index_join` | 7.85ms | 9.93ms | 1.266× | 1.28% | PASS |
| ac_reads | `index_join_scan` | 4.06ms | 5.15ms | 1.269× | 1.59% | PASS |
| ac_reads | `types_table_scan` | 1.10s | 1.26s | 1.146× | 0.90% | PASS |
| ac_reads | `table_scan` | 1.27s | 1.38s | 1.092× | 1.26% | PASS |
| ac_reads | `oltp_read_only` | 149.82ms | 160.49ms | 1.071× | 0.76% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 15.82ms | 64.76ms | 4.093× | 4.75% | PASS |
| ac_writes | `oltp_insert_ac` | 18.62ms | 82.31ms | 4.420× | 5.54% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.09ms | 98.65ms | 4.678× | 5.80% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 15.96ms | 74.36ms | 4.660× | 4.02% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 19.02ms | 86.71ms | 4.558× | 6.07% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.41ms | 85.78ms | 4.659× | 4.86% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.33ms | 75.30ms | 4.612× | 5.11% | PASS |
| ac_writes | `oltp_read_write_ac` | 23.77ms | 93.34ms | 3.927× | 4.38% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.32ms | 37.69ms | 1.243× | 2.02% | PASS |
| mem_reads | `oltp_range_select` | 13.70ms | 14.06ms | 1.027× | 2.09% | PASS |
| mem_reads | `oltp_sum_range` | 12.32ms | 14.07ms | 1.142× | 2.73% | PASS |
| mem_reads | `oltp_order_range` | 2.97ms | 3.16ms | 1.064× | 1.61% | PASS |
| mem_reads | `oltp_distinct_range` | 3.99ms | 4.23ms | 1.060× | 1.20% | PASS |
| mem_reads | `oltp_index_scan` | 4.51ms | 6.32ms | 1.400× | 1.57% | PASS |
| mem_reads | `select_random_points` | 18.43ms | 20.79ms | 1.128× | 3.57% | PASS |
| mem_reads | `select_random_ranges` | 4.14ms | 5.21ms | 1.259× | 2.38% | PASS |
| mem_reads | `covering_index_scan` | 4.58ms | 4.59ms | 1.001× | 2.53% | PASS |
| mem_reads | `groupby_scan` | 32.22ms | 34.08ms | 1.058× | 1.13% | PASS |
| mem_reads | `index_join` | 8.20ms | 10.29ms | 1.254× | 10.74% | PASS |
| mem_reads | `index_join_scan` | 5.05ms | 5.50ms | 1.088× | 6.88% | PASS |
| mem_reads | `types_table_scan` | 1.30s | 1.30s | 0.994× | 3.22% | PASS |
| mem_reads | `table_scan` | 1.56s | 1.44s | 0.922× | 5.00% | PASS |
| mem_reads | `oltp_read_only` | 137.12ms | 143.09ms | 1.044× | 4.84% | PASS |
| mem_writes | `oltp_bulk_insert` | 235.83ms | 358.99ms | 1.522× | 1.20% | PASS |
| mem_writes | `oltp_insert` | 21.82ms | 40.36ms | 1.850× | 1.60% | PASS |
| mem_writes | `oltp_update_index` | 71.11ms | 133.51ms | 1.877× | 1.50% | PASS |
| mem_writes | `oltp_update_non_index` | 48.88ms | 86.66ms | 1.773× | 1.51% | PASS |
| mem_writes | `oltp_delete_insert` | 51.20ms | 103.86ms | 2.028× | 0.93% | PASS |
| mem_writes | `oltp_write_only` | 28.67ms | 61.60ms | 2.149× | 1.22% | PASS |
| mem_writes | `types_delete_insert` | 33.75ms | 55.69ms | 1.650× | 1.57% | PASS |
| mem_writes | `oltp_read_write` | 85.26ms | 139.77ms | 1.639× | 1.21% | PASS |
| file_reads | `oltp_point_select` | 106.47ms | 64.27ms | 0.604× | 0.88% | PASS |
| file_reads | `oltp_range_select` | 21.43ms | 16.88ms | 0.788× | 1.78% | PASS |
| file_reads | `oltp_sum_range` | 21.38ms | 17.24ms | 0.807× | 1.72% | PASS |
| file_reads | `oltp_order_range` | 3.91ms | 3.50ms | 0.895× | 1.78% | PASS |
| file_reads | `oltp_distinct_range` | 4.93ms | 4.61ms | 0.935× | 1.42% | PASS |
| file_reads | `oltp_index_scan` | 12.26ms | 9.22ms | 0.752× | 0.80% | PASS |
| file_reads | `select_random_points` | 27.07ms | 24.24ms | 0.896× | 1.82% | PASS |
| file_reads | `select_random_ranges` | 11.73ms | 7.90ms | 0.673× | 0.99% | PASS |
| file_reads | `covering_index_scan` | 12.31ms | 7.45ms | 0.605× | 1.47% | PASS |
| file_reads | `groupby_scan` | 33.14ms | 34.24ms | 1.033× | 1.20% | PASS |
| file_reads | `index_join` | 11.37ms | 11.33ms | 0.997× | 1.51% | PASS |
| file_reads | `index_join_scan` | 5.80ms | 5.83ms | 1.005× | 3.10% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.23s | 1.166× | 0.70% | PASS |
| file_reads | `table_scan` | 1.38s | 1.39s | 1.005× | 5.54% | PASS |
| file_reads | `oltp_read_only` | 243.74ms | 178.51ms | 0.732× | 0.87% | PASS |
| file_writes | `oltp_bulk_insert` | 255.00ms | 389.28ms | 1.527× | 0.91% | PASS |
| file_writes | `oltp_insert` | 51.44ms | 53.72ms | 1.044× | 22.97% | PASS |
| file_writes | `oltp_update_index` | 114.37ms | 171.22ms | 1.497× | 1.46% | PASS |
| file_writes | `oltp_update_non_index` | 94.61ms | 113.18ms | 1.196× | 8.96% | PASS |
| file_writes | `oltp_delete_insert` | 90.43ms | 134.71ms | 1.490× | 1.40% | PASS |
| file_writes | `oltp_write_only` | 90.18ms | 85.92ms | 0.953× | 9.15% | PASS |
| file_writes | `types_delete_insert` | 56.38ms | 74.88ms | 1.328× | 1.99% | PASS |
| file_writes | `oltp_read_write` | 138.69ms | 164.41ms | 1.185× | 4.27% | PASS |
| ac_reads | `oltp_point_select` | 56.06ms | 64.07ms | 1.143× | 1.25% | PASS |
| ac_reads | `oltp_range_select` | 17.13ms | 16.86ms | 0.984× | 2.41% | PASS |
| ac_reads | `oltp_sum_range` | 16.03ms | 17.13ms | 1.069× | 2.41% | PASS |
| ac_reads | `oltp_order_range` | 3.47ms | 3.51ms | 1.010× | 1.18% | PASS |
| ac_reads | `oltp_distinct_range` | 4.54ms | 4.60ms | 1.011× | 1.38% | PASS |
| ac_reads | `oltp_index_scan` | 7.67ms | 9.30ms | 1.212× | 2.00% | PASS |
| ac_reads | `select_random_points` | 22.04ms | 24.32ms | 1.104× | 1.98% | PASS |
| ac_reads | `select_random_ranges` | 6.87ms | 7.91ms | 1.151× | 1.53% | PASS |
| ac_reads | `covering_index_scan` | 8.60ms | 7.47ms | 0.869× | 2.53% | PASS |
| ac_reads | `groupby_scan` | 32.81ms | 34.29ms | 1.045× | 0.93% | PASS |
| ac_reads | `index_join` | 9.57ms | 11.43ms | 1.194× | 2.23% | PASS |
| ac_reads | `index_join_scan` | 5.54ms | 5.95ms | 1.075× | 4.41% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.23s | 1.168× | 1.68% | PASS |
| ac_reads | `table_scan` | 1.27s | 1.38s | 1.080× | 2.70% | PASS |
| ac_reads | `oltp_read_only` | 157.16ms | 174.82ms | 1.112× | 1.22% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.98ms | 80.97ms | 3.523× | 5.71% | PASS |
| ac_writes | `oltp_insert_ac` | 25.47ms | 97.05ms | 3.811× | 4.65% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.37ms | 112.63ms | 4.115× | 4.69% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.18ms | 93.76ms | 3.877× | 6.67% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.12ms | 105.20ms | 4.362× | 5.15% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.07ms | 104.53ms | 4.170× | 4.18% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.86ms | 95.72ms | 4.379× | 5.94% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.34ms | 112.51ms | 3.479× | 5.75% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.89ms | 36.60ms | 1.185× | 1.80% | PASS |
| mem_reads | `oltp_range_select` | 12.63ms | 13.98ms | 1.108× | 2.62% | PASS |
| mem_reads | `oltp_sum_range` | 11.79ms | 13.81ms | 1.172× | 1.60% | PASS |
| mem_reads | `oltp_order_range` | 2.83ms | 3.12ms | 1.104× | 1.16% | PASS |
| mem_reads | `oltp_distinct_range` | 3.91ms | 4.21ms | 1.076× | 1.24% | PASS |
| mem_reads | `oltp_index_scan` | 4.37ms | 6.09ms | 1.393× | 1.61% | PASS |
| mem_reads | `select_random_points` | 17.35ms | 20.28ms | 1.169× | 1.98% | PASS |
| mem_reads | `select_random_ranges` | 3.86ms | 5.09ms | 1.317× | 2.06% | PASS |
| mem_reads | `covering_index_scan` | 4.39ms | 4.42ms | 1.006× | 1.62% | PASS |
| mem_reads | `groupby_scan` | 31.43ms | 33.55ms | 1.067× | 0.87% | PASS |
| mem_reads | `index_join` | 6.85ms | 8.88ms | 1.297× | 1.19% | PASS |
| mem_reads | `index_join_scan` | 4.17ms | 5.26ms | 1.261× | 4.25% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.22s | 1.179× | 0.41% | PASS |
| mem_reads | `table_scan` | 1.25s | 1.38s | 1.106× | 4.67% | PASS |
| mem_reads | `oltp_read_only` | 117.27ms | 135.17ms | 1.153× | 1.37% | PASS |
| mem_writes | `oltp_bulk_insert` | 242.22ms | 351.84ms | 1.453× | 0.80% | PASS |
| mem_writes | `oltp_insert` | 20.00ms | 39.57ms | 1.979× | 0.79% | PASS |
| mem_writes | `oltp_update_index` | 67.98ms | 128.45ms | 1.889× | 1.05% | PASS |
| mem_writes | `oltp_update_non_index` | 48.87ms | 83.67ms | 1.712× | 1.05% | PASS |
| mem_writes | `oltp_delete_insert` | 50.75ms | 105.03ms | 2.070× | 2.05% | PASS |
| mem_writes | `oltp_write_only` | 28.13ms | 62.16ms | 2.210× | 1.33% | PASS |
| mem_writes | `types_delete_insert` | 32.99ms | 53.86ms | 1.632× | 0.93% | PASS |
| mem_writes | `oltp_read_write` | 83.46ms | 138.41ms | 1.658× | 1.62% | PASS |
| file_reads | `oltp_point_select` | 105.92ms | 62.63ms | 0.591× | 0.93% | PASS |
| file_reads | `oltp_range_select` | 20.66ms | 16.83ms | 0.815× | 1.87% | PASS |
| file_reads | `oltp_sum_range` | 19.89ms | 16.75ms | 0.843× | 1.44% | PASS |
| file_reads | `oltp_order_range` | 3.70ms | 3.51ms | 0.949× | 2.20% | PASS |
| file_reads | `oltp_distinct_range` | 4.82ms | 4.62ms | 0.958× | 1.34% | PASS |
| file_reads | `oltp_index_scan` | 12.09ms | 9.13ms | 0.755× | 1.29% | PASS |
| file_reads | `select_random_points` | 26.45ms | 24.38ms | 0.922× | 1.91% | PASS |
| file_reads | `select_random_ranges` | 11.55ms | 7.88ms | 0.682× | 1.25% | PASS |
| file_reads | `covering_index_scan` | 11.98ms | 7.35ms | 0.614× | 1.04% | PASS |
| file_reads | `groupby_scan` | 32.46ms | 34.19ms | 1.053× | 0.89% | PASS |
| file_reads | `index_join` | 11.09ms | 11.28ms | 1.017× | 1.52% | PASS |
| file_reads | `index_join_scan` | 5.37ms | 5.80ms | 1.081× | 1.83% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.22s | 1.180× | 0.61% | PASS |
| file_reads | `table_scan` | 1.17s | 1.36s | 1.158× | 0.50% | PASS |
| file_reads | `oltp_read_only` | 226.63ms | 174.84ms | 0.771× | 0.73% | PASS |
| file_writes | `oltp_bulk_insert` | 261.01ms | 377.19ms | 1.445× | 1.00% | PASS |
| file_writes | `oltp_insert` | 31.26ms | 52.11ms | 1.667× | 1.36% | PASS |
| file_writes | `oltp_update_index` | 101.59ms | 161.64ms | 1.591× | 1.30% | PASS |
| file_writes | `oltp_update_non_index` | 78.21ms | 107.43ms | 1.374× | 1.08% | PASS |
| file_writes | `oltp_delete_insert` | 80.08ms | 130.09ms | 1.625× | 1.33% | PASS |
| file_writes | `oltp_write_only` | 54.97ms | 84.37ms | 1.535× | 2.62% | PASS |
| file_writes | `types_delete_insert` | 52.21ms | 72.07ms | 1.380× | 1.09% | PASS |
| file_writes | `oltp_read_write` | 113.75ms | 162.15ms | 1.425× | 1.89% | PASS |
| ac_reads | `oltp_point_select` | 55.42ms | 63.13ms | 1.139× | 1.34% | PASS |
| ac_reads | `oltp_range_select` | 15.91ms | 17.14ms | 1.078× | 2.18% | PASS |
| ac_reads | `oltp_sum_range` | 15.09ms | 17.01ms | 1.127× | 2.06% | PASS |
| ac_reads | `oltp_order_range` | 3.40ms | 3.60ms | 1.059× | 2.71% | PASS |
| ac_reads | `oltp_distinct_range` | 4.39ms | 4.68ms | 1.065× | 1.73% | PASS |
| ac_reads | `oltp_index_scan` | 7.11ms | 9.26ms | 1.301× | 2.65% | PASS |
| ac_reads | `select_random_points` | 21.19ms | 24.54ms | 1.158× | 1.79% | PASS |
| ac_reads | `select_random_ranges` | 6.66ms | 7.91ms | 1.187× | 1.58% | PASS |
| ac_reads | `covering_index_scan` | 7.12ms | 7.38ms | 1.038× | 2.04% | PASS |
| ac_reads | `groupby_scan` | 31.97ms | 34.33ms | 1.074× | 0.68% | PASS |
| ac_reads | `index_join` | 8.61ms | 11.37ms | 1.320× | 2.07% | PASS |
| ac_reads | `index_join_scan` | 4.92ms | 5.91ms | 1.200× | 2.96% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.21s | 1.179× | 0.47% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.36s | 1.155× | 0.48% | PASS |
| ac_reads | `oltp_read_only` | 154.57ms | 175.11ms | 1.133× | 1.03% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.80ms | 80.85ms | 3.397× | 6.45% | PASS |
| ac_writes | `oltp_insert_ac` | 26.81ms | 104.54ms | 3.899× | 6.96% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.24ms | 116.30ms | 4.119× | 5.67% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.87ms | 94.32ms | 3.793× | 5.64% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.92ms | 105.44ms | 4.069× | 7.24% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.73ms | 105.63ms | 3.953× | 6.63% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.34ms | 96.53ms | 4.136× | 6.57% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.12ms | 111.94ms | 3.485× | 5.12% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.44ms | 40.40ms | 1.245× | 1.08% | PASS |
| mem_reads | `oltp_range_select` | 18.72ms | 21.17ms | 1.131× | 1.55% | PASS |
| mem_reads | `oltp_sum_range` | 17.83ms | 20.82ms | 1.168× | 1.28% | PASS |
| mem_reads | `oltp_order_range` | 3.50ms | 3.84ms | 1.097× | 1.58% | PASS |
| mem_reads | `oltp_distinct_range` | 4.60ms | 4.90ms | 1.065× | 1.40% | PASS |
| mem_reads | `oltp_index_scan` | 4.46ms | 6.08ms | 1.361× | 2.19% | PASS |
| mem_reads | `select_random_points` | 27.45ms | 31.88ms | 1.161× | 1.35% | PASS |
| mem_reads | `select_random_ranges` | 7.54ms | 8.99ms | 1.193× | 1.83% | PASS |
| mem_reads | `covering_index_scan` | 4.24ms | 4.15ms | 0.980× | 1.06% | PASS |
| mem_reads | `groupby_scan` | 36.35ms | 38.59ms | 1.062× | 0.90% | PASS |
| mem_reads | `index_join` | 8.19ms | 10.31ms | 1.258× | 1.32% | PASS |
| mem_reads | `index_join_scan` | 4.12ms | 5.38ms | 1.306× | 1.96% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.24s | 1.192× | 0.71% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.39s | 1.180× | 0.56% | PASS |
| mem_reads | `oltp_read_only` | 148.50ms | 169.19ms | 1.139× | 0.78% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.97ms | 355.52ms | 1.422× | 0.95% | PASS |
| mem_writes | `oltp_insert` | 19.26ms | 36.28ms | 1.884× | 0.71% | PASS |
| mem_writes | `oltp_update_index` | 67.19ms | 115.86ms | 1.724× | 1.27% | PASS |
| mem_writes | `oltp_update_non_index` | 49.65ms | 82.19ms | 1.655× | 1.04% | PASS |
| mem_writes | `oltp_delete_insert` | 49.21ms | 95.91ms | 1.949× | 0.88% | PASS |
| mem_writes | `oltp_write_only` | 26.80ms | 57.30ms | 2.138× | 1.56% | PASS |
| mem_writes | `types_delete_insert` | 32.27ms | 54.52ms | 1.690× | 1.17% | PASS |
| mem_writes | `oltp_read_write` | 99.92ms | 154.72ms | 1.549× | 0.97% | PASS |
| file_reads | `oltp_point_select` | 107.70ms | 66.34ms | 0.616× | 0.95% | PASS |
| file_reads | `oltp_range_select` | 26.20ms | 24.45ms | 0.933× | 1.37% | PASS |
| file_reads | `oltp_sum_range` | 25.73ms | 24.27ms | 0.943× | 1.49% | PASS |
| file_reads | `oltp_order_range` | 4.31ms | 4.25ms | 0.986× | 1.65% | PASS |
| file_reads | `oltp_distinct_range` | 5.50ms | 5.34ms | 0.971× | 0.95% | PASS |
| file_reads | `oltp_index_scan` | 12.03ms | 9.21ms | 0.766× | 1.36% | PASS |
| file_reads | `select_random_points` | 36.42ms | 36.49ms | 1.002× | 1.41% | PASS |
| file_reads | `select_random_ranges` | 15.51ms | 12.16ms | 0.784× | 1.56% | PASS |
| file_reads | `covering_index_scan` | 11.77ms | 7.22ms | 0.613× | 1.05% | PASS |
| file_reads | `groupby_scan` | 37.45ms | 39.45ms | 1.053× | 1.04% | PASS |
| file_reads | `index_join` | 12.41ms | 12.84ms | 1.034× | 1.04% | PASS |
| file_reads | `index_join_scan` | 5.20ms | 5.95ms | 1.145× | 2.21% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.24s | 1.176× | 1.07% | PASS |
| file_reads | `table_scan` | 1.28s | 1.41s | 1.096× | 1.29% | PASS |
| file_reads | `oltp_read_only` | 272.62ms | 213.84ms | 0.784× | 0.99% | PASS |
| file_writes | `oltp_bulk_insert` | 266.45ms | 382.12ms | 1.434× | 1.01% | PASS |
| file_writes | `oltp_insert` | 26.51ms | 46.82ms | 1.766× | 2.15% | PASS |
| file_writes | `oltp_update_index` | 98.00ms | 144.26ms | 1.472× | 1.18% | PASS |
| file_writes | `oltp_update_non_index` | 77.16ms | 105.15ms | 1.363× | 1.86% | PASS |
| file_writes | `oltp_delete_insert` | 78.26ms | 122.41ms | 1.564× | 1.31% | PASS |
| file_writes | `oltp_write_only` | 51.68ms | 79.12ms | 1.531× | 1.74% | PASS |
| file_writes | `types_delete_insert` | 50.54ms | 69.92ms | 1.384× | 1.46% | PASS |
| file_writes | `oltp_read_write` | 128.65ms | 178.02ms | 1.384× | 1.27% | PASS |
| ac_reads | `oltp_point_select` | 58.33ms | 66.97ms | 1.148× | 1.05% | PASS |
| ac_reads | `oltp_range_select` | 22.88ms | 24.47ms | 1.070× | 1.20% | PASS |
| ac_reads | `oltp_sum_range` | 21.36ms | 24.36ms | 1.140× | 1.49% | PASS |
| ac_reads | `oltp_order_range` | 4.01ms | 4.27ms | 1.065× | 1.68% | PASS |
| ac_reads | `oltp_distinct_range` | 5.08ms | 5.37ms | 1.057× | 1.15% | PASS |
| ac_reads | `oltp_index_scan` | 7.21ms | 9.24ms | 1.281× | 1.58% | PASS |
| ac_reads | `select_random_points` | 31.39ms | 36.73ms | 1.170× | 1.27% | PASS |
| ac_reads | `select_random_ranges` | 10.57ms | 12.22ms | 1.156× | 1.27% | PASS |
| ac_reads | `covering_index_scan` | 6.88ms | 7.22ms | 1.050× | 1.54% | PASS |
| ac_reads | `groupby_scan` | 37.01ms | 39.40ms | 1.065× | 1.23% | PASS |
| ac_reads | `index_join` | 10.15ms | 13.10ms | 1.291× | 2.30% | PASS |
| ac_reads | `index_join_scan` | 4.82ms | 6.02ms | 1.249× | 1.98% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.23s | 1.192× | 0.65% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.39s | 1.174× | 1.07% | PASS |
| ac_reads | `oltp_read_only` | 189.12ms | 211.12ms | 1.116× | 0.99% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.99ms | 80.45ms | 3.499× | 4.91% | PASS |
| ac_writes | `oltp_insert_ac` | 25.80ms | 101.25ms | 3.924× | 6.67% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.73ms | 112.26ms | 4.048× | 6.26% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.26ms | 92.16ms | 3.962× | 7.03% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.95ms | 105.43ms | 4.064× | 5.68% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.60ms | 102.76ms | 4.014× | 5.73% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.32ms | 95.91ms | 4.297× | 7.80% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.21ms | 111.25ms | 3.350× | 5.49% | PASS |

</details>

## Version-control latency

Wall time: 2m 22s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.63ms | 200.00ms | 41.8% | 0.56% | PASS |
| `status_dirty_many_tables` | 86.96ms | 200.00ms | 43.5% | 0.46% | PASS |
| `diff_regular_working_one_table` | 79.72ms | 150.00ms | 53.1% | 0.44% | PASS |
| `diff_regular_working_many_tables` | 92.98ms | 200.00ms | 46.5% | 0.54% | PASS |
| `diff_stat_working_many_tables` | 93.40ms | 200.00ms | 46.7% | 0.65% | PASS |
| `diff_schema_working_many_tables` | 93.33ms | 200.00ms | 46.7% | 0.56% | PASS |
| `branch_list_many_branches` | 23.62ms | 100.00ms | 23.6% | 2.24% | PASS |
| `branch_create_delete` | 26.16ms | 100.00ms | 26.2% | 1.95% | PASS |
| `checkout_branch_clean` | 56.39ms | 200.00ms | 28.2% | 0.72% | PASS |
| `merge_data_no_conflicts` | 30.25ms | 150.00ms | 20.2% | 2.56% | PASS |
| `merge_schema_no_conflicts` | 23.20ms | 100.00ms | 23.2% | 2.07% | PASS |
| `merge_data_conflicts` | 128.77ms | 250.00ms | 51.5% | 0.35% | PASS |
| `merge_data_conflicts_with_resolve` | 128.74ms | 250.00ms | 51.5% | 0.30% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
