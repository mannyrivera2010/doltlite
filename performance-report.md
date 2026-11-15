# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-15 18:07 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu26 20261111.196.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/43735863392)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 59m 31s | 7.29s | 8.89s | 1.218× | 1.30% | **PASS** |
| textpk | 69 | 55 | 1h 33m 48s | 10.29s | 11.85s | 1.152× | 1.97% | **PASS** |
| blobpk | 69 | 55 | 1h 27m 57s | 9.75s | 10.52s | 1.079× | 2.08% | **PASS** |
| compositepk | 69 | 55 | 1h 4m 21s | 7.17s | 7.92s | 1.105× | 2.78% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 18.87ms | 20.31ms | 1.076× | 1.01% | PASS |
| mem_reads | `oltp_range_select` | 8.45ms | 9.78ms | 1.157× | 1.29% | PASS |
| mem_reads | `oltp_sum_range` | 8.09ms | 9.39ms | 1.161× | 1.27% | PASS |
| mem_reads | `oltp_order_range` | 2.18ms | 2.48ms | 1.136× | 0.84% | PASS |
| mem_reads | `oltp_distinct_range` | 2.90ms | 3.27ms | 1.129× | 1.22% | PASS |
| mem_reads | `oltp_index_scan` | 3.13ms | 3.82ms | 1.220× | 1.80% | PASS |
| mem_reads | `select_random_points` | 9.50ms | 9.24ms | 0.972× | 1.77% | PASS |
| mem_reads | `select_random_ranges` | 2.61ms | 2.90ms | 1.113× | 2.22% | PASS |
| mem_reads | `covering_index_scan` | 2.85ms | 2.96ms | 1.041× | 1.68% | PASS |
| mem_reads | `groupby_scan` | 25.93ms | 27.89ms | 1.075× | 0.86% | PASS |
| mem_reads | `index_join` | 4.61ms | 6.15ms | 1.333× | 0.69% | PASS |
| mem_reads | `index_join_scan` | 2.55ms | 3.70ms | 1.454× | 0.98% | PASS |
| mem_reads | `types_table_scan` | 843.35ms | 1.01s | 1.196× | 0.45% | PASS |
| mem_reads | `table_scan` | 957.04ms | 1.08s | 1.133× | 0.62% | PASS |
| mem_reads | `oltp_read_only` | 79.45ms | 91.57ms | 1.153× | 0.58% | PASS |
| mem_writes | `oltp_bulk_insert` | 119.04ms | 167.33ms | 1.406× | 0.95% | PASS |
| mem_writes | `oltp_insert` | 10.79ms | 19.50ms | 1.808× | 0.58% | PASS |
| mem_writes | `oltp_update_index` | 38.69ms | 79.43ms | 2.053× | 0.82% | PASS |
| mem_writes | `oltp_update_non_index` | 25.03ms | 43.52ms | 1.739× | 0.83% | PASS |
| mem_writes | `oltp_delete_insert` | 33.62ms | 57.29ms | 1.704× | 1.11% | PASS |
| mem_writes | `oltp_write_only` | 16.10ms | 32.99ms | 2.050× | 1.36% | PASS |
| mem_writes | `types_delete_insert` | 18.72ms | 30.23ms | 1.615× | 0.60% | PASS |
| mem_writes | `oltp_read_write` | 48.65ms | 78.61ms | 1.616× | 0.96% | PASS |
| file_reads | `oltp_point_select` | 43.15ms | 27.85ms | 0.645× | 0.77% | PASS |
| file_reads | `oltp_range_select` | 11.16ms | 10.76ms | 0.965× | 1.52% | PASS |
| file_reads | `oltp_sum_range` | 10.59ms | 10.25ms | 0.968× | 1.20% | PASS |
| file_reads | `oltp_order_range` | 2.52ms | 2.59ms | 1.026× | 1.02% | PASS |
| file_reads | `oltp_distinct_range` | 3.25ms | 3.40ms | 1.045× | 0.96% | PASS |
| file_reads | `oltp_index_scan` | 5.76ms | 4.76ms | 0.826× | 1.81% | PASS |
| file_reads | `select_random_points` | 12.62ms | 10.43ms | 0.827× | 1.30% | PASS |
| file_reads | `select_random_ranges` | 5.24ms | 3.77ms | 0.720× | 1.68% | PASS |
| file_reads | `covering_index_scan` | 5.51ms | 3.90ms | 0.709× | 2.16% | PASS |
| file_reads | `groupby_scan` | 26.55ms | 28.18ms | 1.061× | 1.00% | PASS |
| file_reads | `index_join` | 6.19ms | 6.96ms | 1.125× | 1.99% | PASS |
| file_reads | `index_join_scan` | 2.96ms | 3.92ms | 1.326× | 1.60% | PASS |
| file_reads | `types_table_scan` | 842.22ms | 1.01s | 1.200× | 0.62% | PASS |
| file_reads | `table_scan` | 957.49ms | 1.09s | 1.136× | 0.82% | PASS |
| file_reads | `oltp_read_only` | 114.57ms | 102.45ms | 0.894× | 0.51% | PASS |
| file_writes | `oltp_bulk_insert` | 151.13ms | 219.18ms | 1.450× | 1.54% | PASS |
| file_writes | `oltp_insert` | 22.06ms | 28.77ms | 1.304× | 9.96% | PASS |
| file_writes | `oltp_update_index` | 121.66ms | 132.40ms | 1.088× | 2.22% | PASS |
| file_writes | `oltp_update_non_index` | 98.00ms | 96.17ms | 0.981× | 4.70% | PASS |
| file_writes | `oltp_delete_insert` | 101.60ms | 109.11ms | 1.074× | 1.66% | PASS |
| file_writes | `oltp_write_only` | 80.69ms | 69.44ms | 0.861× | 4.50% | PASS |
| file_writes | `types_delete_insert` | 60.74ms | 51.08ms | 0.841× | 3.27% | PASS |
| file_writes | `oltp_read_write` | 111.36ms | 115.41ms | 1.036× | 3.03% | PASS |
| ac_reads | `oltp_point_select` | 27.13ms | 27.97ms | 1.031× | 0.78% | PASS |
| ac_reads | `oltp_range_select` | 9.62ms | 10.79ms | 1.122× | 1.82% | PASS |
| ac_reads | `oltp_sum_range` | 9.17ms | 10.26ms | 1.119× | 1.94% | PASS |
| ac_reads | `oltp_order_range` | 2.36ms | 2.57ms | 1.088× | 1.22% | PASS |
| ac_reads | `oltp_distinct_range` | 3.09ms | 3.38ms | 1.095× | 0.70% | PASS |
| ac_reads | `oltp_index_scan` | 4.17ms | 4.74ms | 1.135× | 1.58% | PASS |
| ac_reads | `select_random_points` | 11.18ms | 10.47ms | 0.936× | 1.42% | PASS |
| ac_reads | `select_random_ranges` | 3.67ms | 3.79ms | 1.034× | 2.22% | PASS |
| ac_reads | `covering_index_scan` | 3.87ms | 3.94ms | 1.017× | 2.13% | PASS |
| ac_reads | `groupby_scan` | 26.34ms | 28.14ms | 1.068× | 0.54% | PASS |
| ac_reads | `index_join` | 5.41ms | 6.88ms | 1.272× | 1.95% | PASS |
| ac_reads | `index_join_scan` | 2.81ms | 3.86ms | 1.371× | 0.97% | PASS |
| ac_reads | `types_table_scan` | 842.72ms | 1.01s | 1.199× | 0.38% | PASS |
| ac_reads | `table_scan` | 958.07ms | 1.09s | 1.134× | 0.68% | PASS |
| ac_reads | `oltp_read_only` | 90.52ms | 102.45ms | 1.132× | 0.62% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 25.16ms | 65.90ms | 2.619× | 4.91% | PASS |
| ac_writes | `oltp_insert_ac` | 26.45ms | 77.03ms | 2.912× | 4.29% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.51ms | 88.58ms | 3.220× | 3.52% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.68ms | 74.60ms | 2.905× | 4.47% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.36ms | 81.49ms | 3.092× | 4.40% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.00ms | 82.21ms | 3.044× | 4.98% | PASS |
| ac_writes | `types_delete_insert_ac` | 25.11ms | 78.44ms | 3.124× | 9.03% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.06ms | 86.41ms | 2.874× | 4.16% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.21ms | 38.24ms | 1.266× | 1.43% | PASS |
| mem_reads | `oltp_range_select` | 14.21ms | 14.77ms | 1.040× | 2.46% | PASS |
| mem_reads | `oltp_sum_range` | 12.53ms | 14.09ms | 1.125× | 2.52% | PASS |
| mem_reads | `oltp_order_range` | 3.13ms | 3.26ms | 1.042× | 1.27% | PASS |
| mem_reads | `oltp_distinct_range` | 4.12ms | 4.40ms | 1.067× | 1.03% | PASS |
| mem_reads | `oltp_index_scan` | 4.58ms | 6.34ms | 1.385× | 1.97% | PASS |
| mem_reads | `select_random_points` | 18.23ms | 20.94ms | 1.148× | 1.87% | PASS |
| mem_reads | `select_random_ranges` | 4.02ms | 5.27ms | 1.312× | 2.01% | PASS |
| mem_reads | `covering_index_scan` | 4.61ms | 4.61ms | 0.999× | 3.67% | PASS |
| mem_reads | `groupby_scan` | 33.79ms | 37.46ms | 1.109× | 2.14% | PASS |
| mem_reads | `index_join` | 7.05ms | 9.46ms | 1.342× | 2.52% | PASS |
| mem_reads | `index_join_scan` | 4.62ms | 5.43ms | 1.175× | 2.57% | PASS |
| mem_reads | `types_table_scan` | 1.12s | 1.25s | 1.113× | 1.70% | PASS |
| mem_reads | `table_scan` | 1.37s | 1.36s | 0.990× | 2.93% | PASS |
| mem_reads | `oltp_read_only` | 127.71ms | 140.79ms | 1.102× | 1.85% | PASS |
| mem_writes | `oltp_bulk_insert` | 231.88ms | 364.27ms | 1.571× | 0.98% | PASS |
| mem_writes | `oltp_insert` | 21.90ms | 40.71ms | 1.859× | 1.50% | PASS |
| mem_writes | `oltp_update_index` | 73.69ms | 141.48ms | 1.920× | 2.09% | PASS |
| mem_writes | `oltp_update_non_index` | 49.65ms | 92.10ms | 1.855× | 1.35% | PASS |
| mem_writes | `oltp_delete_insert` | 52.34ms | 109.75ms | 2.097× | 1.68% | PASS |
| mem_writes | `oltp_write_only` | 29.64ms | 66.03ms | 2.228× | 1.88% | PASS |
| mem_writes | `types_delete_insert` | 33.95ms | 59.54ms | 1.754× | 1.79% | PASS |
| mem_writes | `oltp_read_write` | 92.05ms | 146.93ms | 1.596× | 1.95% | PASS |
| file_reads | `oltp_point_select` | 100.94ms | 62.61ms | 0.620× | 1.01% | PASS |
| file_reads | `oltp_range_select` | 22.31ms | 17.27ms | 0.774× | 2.59% | PASS |
| file_reads | `oltp_sum_range` | 20.30ms | 16.96ms | 0.835× | 1.68% | PASS |
| file_reads | `oltp_order_range` | 4.02ms | 3.60ms | 0.895× | 1.50% | PASS |
| file_reads | `oltp_distinct_range` | 5.06ms | 4.73ms | 0.935× | 1.12% | PASS |
| file_reads | `oltp_index_scan` | 12.02ms | 9.04ms | 0.752× | 1.70% | PASS |
| file_reads | `select_random_points` | 26.85ms | 24.35ms | 0.907× | 2.83% | PASS |
| file_reads | `select_random_ranges` | 11.15ms | 7.72ms | 0.692× | 1.14% | PASS |
| file_reads | `covering_index_scan` | 12.60ms | 7.23ms | 0.573× | 3.29% | PASS |
| file_reads | `groupby_scan` | 34.77ms | 37.71ms | 1.085× | 1.75% | PASS |
| file_reads | `index_join` | 11.72ms | 11.46ms | 0.978× | 3.12% | PASS |
| file_reads | `index_join_scan` | 5.88ms | 6.05ms | 1.029× | 3.86% | PASS |
| file_reads | `types_table_scan` | 1.12s | 1.25s | 1.119× | 2.35% | PASS |
| file_reads | `table_scan` | 1.38s | 1.36s | 0.989× | 2.30% | PASS |
| file_reads | `oltp_read_only` | 233.43ms | 177.95ms | 0.762× | 1.31% | PASS |
| file_writes | `oltp_bulk_insert` | 248.74ms | 394.93ms | 1.588× | 0.99% | PASS |
| file_writes | `oltp_insert` | 50.18ms | 54.42ms | 1.084× | 17.84% | PASS |
| file_writes | `oltp_update_index` | 118.31ms | 179.31ms | 1.516× | 2.32% | PASS |
| file_writes | `oltp_update_non_index` | 98.53ms | 120.78ms | 1.226× | 11.56% | PASS |
| file_writes | `oltp_delete_insert` | 94.80ms | 140.70ms | 1.484× | 1.97% | PASS |
| file_writes | `oltp_write_only` | 86.56ms | 90.76ms | 1.049× | 8.71% | PASS |
| file_writes | `types_delete_insert` | 56.96ms | 78.60ms | 1.380× | 2.46% | PASS |
| file_writes | `oltp_read_write` | 147.10ms | 173.82ms | 1.182× | 6.50% | PASS |
| ac_reads | `oltp_point_select` | 53.74ms | 63.11ms | 1.174× | 1.24% | PASS |
| ac_reads | `oltp_range_select` | 17.80ms | 17.26ms | 0.970× | 1.81% | PASS |
| ac_reads | `oltp_sum_range` | 15.69ms | 16.95ms | 1.080× | 1.23% | PASS |
| ac_reads | `oltp_order_range` | 3.58ms | 3.59ms | 1.001× | 1.11% | PASS |
| ac_reads | `oltp_distinct_range` | 4.59ms | 4.75ms | 1.036× | 1.53% | PASS |
| ac_reads | `oltp_index_scan` | 7.43ms | 9.05ms | 1.219× | 1.81% | PASS |
| ac_reads | `select_random_points` | 21.51ms | 24.41ms | 1.135× | 1.93% | PASS |
| ac_reads | `select_random_ranges` | 6.57ms | 7.70ms | 1.172× | 1.10% | PASS |
| ac_reads | `covering_index_scan` | 8.15ms | 7.26ms | 0.890× | 1.48% | PASS |
| ac_reads | `groupby_scan` | 34.50ms | 38.29ms | 1.110× | 2.53% | PASS |
| ac_reads | `index_join` | 9.44ms | 11.50ms | 1.218× | 1.88% | PASS |
| ac_reads | `index_join_scan` | 5.29ms | 5.96ms | 1.127× | 2.92% | PASS |
| ac_reads | `types_table_scan` | 1.12s | 1.25s | 1.112× | 1.95% | PASS |
| ac_reads | `table_scan` | 1.37s | 1.36s | 0.992× | 2.40% | PASS |
| ac_reads | `oltp_read_only` | 163.92ms | 177.91ms | 1.085× | 1.45% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.50ms | 65.25ms | 3.036× | 6.41% | PASS |
| ac_writes | `oltp_insert_ac` | 25.55ms | 80.43ms | 3.147× | 6.43% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.49ms | 100.22ms | 3.646× | 6.18% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.59ms | 79.14ms | 3.503× | 8.31% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.36ms | 90.85ms | 3.583× | 6.62% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.59ms | 91.64ms | 3.581× | 8.55% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.22ms | 83.49ms | 3.934× | 6.56% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.92ms | 99.06ms | 3.204× | 8.77% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 26.29ms | 29.51ms | 1.122× | 1.45% | PASS |
| mem_reads | `oltp_range_select` | 13.04ms | 12.02ms | 0.921× | 1.85% | PASS |
| mem_reads | `oltp_sum_range` | 11.19ms | 11.39ms | 1.018× | 2.60% | PASS |
| mem_reads | `oltp_order_range` | 2.59ms | 2.64ms | 1.018× | 1.29% | PASS |
| mem_reads | `oltp_distinct_range` | 3.33ms | 3.41ms | 1.022× | 0.99% | PASS |
| mem_reads | `oltp_index_scan` | 4.02ms | 5.21ms | 1.297× | 2.33% | PASS |
| mem_reads | `select_random_points` | 16.50ms | 17.48ms | 1.059× | 3.62% | PASS |
| mem_reads | `select_random_ranges` | 3.64ms | 4.35ms | 1.194× | 2.25% | PASS |
| mem_reads | `covering_index_scan` | 4.13ms | 4.02ms | 0.972× | 2.92% | PASS |
| mem_reads | `groupby_scan` | 27.89ms | 27.80ms | 0.997× | 0.71% | PASS |
| mem_reads | `index_join` | 6.37ms | 8.42ms | 1.323× | 5.05% | PASS |
| mem_reads | `index_join_scan` | 4.30ms | 5.09ms | 1.183× | 6.73% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.10s | 1.069× | 0.52% | PASS |
| mem_reads | `table_scan` | 1.28s | 1.20s | 0.939× | 0.46% | PASS |
| mem_reads | `oltp_read_only` | 110.19ms | 110.69ms | 1.005× | 0.92% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.25ms | 270.58ms | 1.509× | 0.46% | PASS |
| mem_writes | `oltp_insert` | 16.73ms | 32.83ms | 1.962× | 1.81% | PASS |
| mem_writes | `oltp_update_index` | 63.64ms | 118.36ms | 1.860× | 2.61% | PASS |
| mem_writes | `oltp_update_non_index` | 43.30ms | 73.40ms | 1.695× | 1.55% | PASS |
| mem_writes | `oltp_delete_insert` | 44.26ms | 90.65ms | 2.048× | 2.08% | PASS |
| mem_writes | `oltp_write_only` | 25.09ms | 55.56ms | 2.214× | 2.15% | PASS |
| mem_writes | `types_delete_insert` | 27.27ms | 46.63ms | 1.710× | 1.11% | PASS |
| mem_writes | `oltp_read_write` | 77.61ms | 117.57ms | 1.515× | 1.20% | PASS |
| file_reads | `oltp_point_select` | 88.50ms | 51.49ms | 0.582× | 2.85% | PASS |
| file_reads | `oltp_range_select` | 19.64ms | 14.09ms | 0.717× | 1.79% | PASS |
| file_reads | `oltp_sum_range` | 18.00ms | 13.45ms | 0.747× | 1.84% | PASS |
| file_reads | `oltp_order_range` | 3.33ms | 2.89ms | 0.866× | 1.08% | PASS |
| file_reads | `oltp_distinct_range` | 4.10ms | 3.65ms | 0.891× | 1.08% | PASS |
| file_reads | `oltp_index_scan` | 10.73ms | 7.39ms | 0.689× | 1.66% | PASS |
| file_reads | `select_random_points` | 23.12ms | 19.14ms | 0.828× | 2.46% | PASS |
| file_reads | `select_random_ranges` | 10.00ms | 6.54ms | 0.654× | 1.52% | PASS |
| file_reads | `covering_index_scan` | 11.41ms | 6.22ms | 0.545× | 2.52% | PASS |
| file_reads | `groupby_scan` | 28.67ms | 28.07ms | 0.979× | 0.84% | PASS |
| file_reads | `index_join` | 10.58ms | 9.48ms | 0.897× | 2.70% | PASS |
| file_reads | `index_join_scan` | 5.16ms | 5.21ms | 1.011× | 4.91% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.11s | 1.075× | 0.53% | PASS |
| file_reads | `table_scan` | 1.29s | 1.20s | 0.934× | 0.57% | PASS |
| file_reads | `oltp_read_only` | 199.09ms | 143.34ms | 0.720× | 1.57% | PASS |
| file_writes | `oltp_bulk_insert` | 238.74ms | 349.21ms | 1.463× | 2.95% | PASS |
| file_writes | `oltp_insert` | 51.49ms | 63.83ms | 1.240× | 5.17% | PASS |
| file_writes | `oltp_update_index` | 195.21ms | 214.51ms | 1.099× | 6.76% | PASS |
| file_writes | `oltp_update_non_index` | 150.37ms | 142.79ms | 0.950× | 2.87% | PASS |
| file_writes | `oltp_delete_insert` | 164.07ms | 174.22ms | 1.062× | 6.08% | PASS |
| file_writes | `oltp_write_only` | 116.01ms | 120.73ms | 1.041× | 3.26% | PASS |
| file_writes | `types_delete_insert` | 97.21ms | 95.97ms | 0.987× | 4.00% | PASS |
| file_writes | `oltp_read_write` | 170.31ms | 184.61ms | 1.084× | 8.24% | PASS |
| ac_reads | `oltp_point_select` | 46.41ms | 51.68ms | 1.114× | 1.42% | PASS |
| ac_reads | `oltp_range_select` | 15.39ms | 14.19ms | 0.922× | 2.09% | PASS |
| ac_reads | `oltp_sum_range` | 13.42ms | 13.58ms | 1.012× | 2.01% | PASS |
| ac_reads | `oltp_order_range` | 2.94ms | 2.89ms | 0.983× | 1.14% | PASS |
| ac_reads | `oltp_distinct_range` | 3.67ms | 3.67ms | 0.999× | 1.10% | PASS |
| ac_reads | `oltp_index_scan` | 6.53ms | 7.41ms | 1.135× | 1.85% | PASS |
| ac_reads | `select_random_points` | 18.11ms | 19.25ms | 1.063× | 2.00% | PASS |
| ac_reads | `select_random_ranges` | 5.81ms | 6.54ms | 1.126× | 1.34% | PASS |
| ac_reads | `covering_index_scan` | 7.02ms | 6.24ms | 0.889× | 2.98% | PASS |
| ac_reads | `groupby_scan` | 28.13ms | 28.09ms | 0.998× | 0.89% | PASS |
| ac_reads | `index_join` | 7.89ms | 9.54ms | 1.209× | 4.44% | PASS |
| ac_reads | `index_join_scan` | 4.24ms | 5.25ms | 1.241× | 4.09% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.11s | 1.070× | 0.55% | PASS |
| ac_reads | `table_scan` | 1.29s | 1.21s | 0.932× | 0.56% | PASS |
| ac_reads | `oltp_read_only` | 140.50ms | 144.51ms | 1.029× | 1.39% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 17.06ms | 54.64ms | 3.203× | 15.71% | PASS |
| ac_writes | `oltp_insert_ac` | 19.21ms | 70.73ms | 3.682× | 10.31% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.84ms | 82.96ms | 3.982× | 12.27% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 16.84ms | 63.06ms | 3.745× | 8.50% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.50ms | 71.97ms | 3.890× | 7.08% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.29ms | 75.19ms | 3.897× | 13.21% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.35ms | 63.72ms | 3.897× | 10.76% | PASS |
| ac_writes | `oltp_read_write_ac` | 23.95ms | 78.24ms | 3.268× | 8.20% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 21.92ms | 24.96ms | 1.139× | 2.52% | PASS |
| mem_reads | `oltp_range_select` | 11.68ms | 11.74ms | 1.006× | 2.01% | PASS |
| mem_reads | `oltp_sum_range` | 10.94ms | 11.45ms | 1.047× | 2.42% | PASS |
| mem_reads | `oltp_order_range` | 2.32ms | 2.27ms | 0.978× | 1.70% | PASS |
| mem_reads | `oltp_distinct_range` | 2.88ms | 2.81ms | 0.975× | 2.20% | PASS |
| mem_reads | `oltp_index_scan` | 3.14ms | 4.16ms | 1.323× | 4.82% | PASS |
| mem_reads | `select_random_points` | 17.80ms | 20.18ms | 1.134× | 3.23% | PASS |
| mem_reads | `select_random_ranges` | 4.77ms | 5.23ms | 1.095× | 1.95% | PASS |
| mem_reads | `covering_index_scan` | 2.35ms | 2.58ms | 1.098× | 4.64% | PASS |
| mem_reads | `groupby_scan` | 22.27ms | 22.94ms | 1.030× | 2.05% | PASS |
| mem_reads | `index_join` | 4.95ms | 6.29ms | 1.270× | 6.68% | PASS |
| mem_reads | `index_join_scan` | 3.02ms | 4.52ms | 1.498× | 5.43% | PASS |
| mem_reads | `types_table_scan` | 717.74ms | 774.69ms | 1.079× | 1.76% | PASS |
| mem_reads | `table_scan` | 890.57ms | 870.66ms | 0.978× | 4.10% | PASS |
| mem_reads | `oltp_read_only` | 90.45ms | 91.86ms | 1.016× | 2.17% | PASS |
| mem_writes | `oltp_bulk_insert` | 147.42ms | 214.20ms | 1.453× | 1.20% | PASS |
| mem_writes | `oltp_insert` | 11.58ms | 24.45ms | 2.112× | 1.29% | PASS |
| mem_writes | `oltp_update_index` | 44.46ms | 84.03ms | 1.890× | 2.52% | PASS |
| mem_writes | `oltp_update_non_index` | 34.42ms | 60.64ms | 1.762× | 2.37% | PASS |
| mem_writes | `oltp_delete_insert` | 31.25ms | 65.43ms | 2.094× | 2.29% | PASS |
| mem_writes | `oltp_write_only` | 17.17ms | 41.51ms | 2.417× | 2.78% | PASS |
| mem_writes | `types_delete_insert` | 21.82ms | 38.23ms | 1.751× | 2.12% | PASS |
| mem_writes | `oltp_read_write` | 59.44ms | 93.23ms | 1.569× | 3.67% | PASS |
| file_reads | `oltp_point_select` | 76.57ms | 43.48ms | 0.568× | 2.36% | PASS |
| file_reads | `oltp_range_select` | 17.81ms | 13.98ms | 0.785× | 1.64% | PASS |
| file_reads | `oltp_sum_range` | 17.63ms | 14.10ms | 0.800× | 1.53% | PASS |
| file_reads | `oltp_order_range` | 3.13ms | 2.64ms | 0.841× | 2.05% | PASS |
| file_reads | `oltp_distinct_range` | 3.60ms | 3.10ms | 0.860× | 1.56% | PASS |
| file_reads | `oltp_index_scan` | 8.87ms | 5.83ms | 0.657× | 1.89% | PASS |
| file_reads | `select_random_points` | 23.39ms | 21.48ms | 0.918× | 2.92% | PASS |
| file_reads | `select_random_ranges` | 10.48ms | 7.32ms | 0.699× | 0.99% | PASS |
| file_reads | `covering_index_scan` | 8.62ms | 5.16ms | 0.598× | 1.89% | PASS |
| file_reads | `groupby_scan` | 23.81ms | 24.00ms | 1.008× | 1.50% | PASS |
| file_reads | `index_join` | 8.72ms | 8.56ms | 0.981× | 5.47% | PASS |
| file_reads | `index_join_scan` | 3.63ms | 4.41ms | 1.215× | 3.45% | PASS |
| file_reads | `types_table_scan` | 714.58ms | 766.66ms | 1.073× | 1.14% | PASS |
| file_reads | `table_scan` | 840.21ms | 863.03ms | 1.027× | 2.71% | PASS |
| file_reads | `oltp_read_only` | 166.15ms | 117.65ms | 0.708× | 2.83% | PASS |
| file_writes | `oltp_bulk_insert` | 194.55ms | 268.47ms | 1.380× | 2.82% | PASS |
| file_writes | `oltp_insert` | 19.74ms | 38.92ms | 1.971× | 4.27% | PASS |
| file_writes | `oltp_update_index` | 150.59ms | 161.09ms | 1.070× | 4.46% | PASS |
| file_writes | `oltp_update_non_index` | 123.33ms | 114.03ms | 0.925× | 2.69% | PASS |
| file_writes | `oltp_delete_insert` | 120.82ms | 125.79ms | 1.041× | 4.06% | PASS |
| file_writes | `oltp_write_only` | 89.89ms | 98.08ms | 1.091× | 5.50% | PASS |
| file_writes | `types_delete_insert` | 79.14ms | 79.14ms | 1.000× | 11.25% | PASS |
| file_writes | `oltp_read_write` | 130.66ms | 148.85ms | 1.139× | 5.39% | PASS |
| ac_reads | `oltp_point_select` | 39.10ms | 42.55ms | 1.088× | 3.62% | PASS |
| ac_reads | `oltp_range_select` | 14.11ms | 13.64ms | 0.967× | 4.10% | PASS |
| ac_reads | `oltp_sum_range` | 13.34ms | 13.46ms | 1.009× | 4.16% | PASS |
| ac_reads | `oltp_order_range` | 2.68ms | 2.53ms | 0.944× | 1.90% | PASS |
| ac_reads | `oltp_distinct_range` | 3.27ms | 3.10ms | 0.949× | 1.37% | PASS |
| ac_reads | `oltp_index_scan` | 5.39ms | 6.27ms | 1.163× | 1.22% | PASS |
| ac_reads | `select_random_points` | 19.51ms | 21.30ms | 1.092× | 3.97% | PASS |
| ac_reads | `select_random_ranges` | 6.86ms | 7.27ms | 1.059× | 1.50% | PASS |
| ac_reads | `covering_index_scan` | 5.01ms | 5.17ms | 1.034× | 2.47% | PASS |
| ac_reads | `groupby_scan` | 23.38ms | 24.10ms | 1.031× | 1.40% | PASS |
| ac_reads | `index_join` | 6.84ms | 8.74ms | 1.278× | 5.96% | PASS |
| ac_reads | `index_join_scan` | 3.49ms | 4.70ms | 1.346× | 5.76% | PASS |
| ac_reads | `types_table_scan` | 800.06ms | 793.80ms | 0.992× | 2.83% | PASS |
| ac_reads | `table_scan` | 897.49ms | 866.29ms | 0.965× | 3.67% | PASS |
| ac_reads | `oltp_read_only` | 112.15ms | 117.31ms | 1.046× | 2.62% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.25ms | 57.19ms | 2.570× | 4.75% | PASS |
| ac_writes | `oltp_insert_ac` | 26.25ms | 71.62ms | 2.728× | 5.75% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.90ms | 80.85ms | 2.898× | 3.63% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.38ms | 69.25ms | 3.094× | 5.77% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.88ms | 75.59ms | 2.921× | 4.57% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.55ms | 75.63ms | 2.960× | 6.23% | PASS |
| ac_writes | `types_delete_insert_ac` | 25.74ms | 63.15ms | 2.454× | 7.25% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.92ms | 78.79ms | 2.822× | 5.32% | PASS |

</details>

## Version-control latency

Wall time: 1m 23s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 48.54ms | 200.00ms | 24.3% | 0.60% | PASS |
| `status_dirty_many_tables` | 50.48ms | 200.00ms | 25.2% | 0.90% | PASS |
| `diff_regular_working_one_table` | 44.71ms | 150.00ms | 29.8% | 0.74% | PASS |
| `diff_regular_working_many_tables` | 53.30ms | 200.00ms | 26.7% | 0.74% | PASS |
| `diff_stat_working_many_tables` | 53.74ms | 200.00ms | 26.9% | 0.71% | PASS |
| `diff_schema_working_many_tables` | 54.03ms | 200.00ms | 27.0% | 0.63% | PASS |
| `branch_list_many_branches` | 12.65ms | 100.00ms | 12.7% | 1.00% | PASS |
| `branch_create_delete` | 14.12ms | 100.00ms | 14.1% | 1.28% | PASS |
| `checkout_branch_clean` | 33.21ms | 200.00ms | 16.6% | 0.96% | PASS |
| `merge_data_no_conflicts` | 16.68ms | 150.00ms | 11.1% | 1.91% | PASS |
| `merge_schema_no_conflicts` | 12.57ms | 100.00ms | 12.6% | 1.29% | PASS |
| `merge_data_conflicts` | 58.49ms | 250.00ms | 23.4% | 0.61% | PASS |
| `merge_data_conflicts_with_resolve` | 58.60ms | 250.00ms | 23.4% | 0.66% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
