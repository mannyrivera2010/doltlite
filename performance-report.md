# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-10 17:09 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261004.327.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/38063973518)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 52m 25s | 6.25s | 7.18s | 1.150× | 2.04% | **PASS** |
| textpk | 69 | 55 | 1h 34m 39s | 11.23s | 11.03s | 0.982× | 3.69% | **PASS** |
| blobpk | 69 | 55 | 1h 31m 16s | 9.76s | 11.79s | 1.208× | 1.65% | **PASS** |
| compositepk | 69 | 55 | 1h 27m 8s | 9.74s | 11.93s | 1.225× | 1.35% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 15.54ms | 17.55ms | 1.130× | 2.34% | PASS |
| mem_reads | `oltp_range_select` | 6.25ms | 7.09ms | 1.134× | 2.81% | PASS |
| mem_reads | `oltp_sum_range` | 5.71ms | 6.82ms | 1.194× | 3.60% | PASS |
| mem_reads | `oltp_order_range` | 1.55ms | 1.64ms | 1.062× | 3.92% | PASS |
| mem_reads | `oltp_distinct_range` | 2.08ms | 2.17ms | 1.047× | 1.44% | PASS |
| mem_reads | `oltp_index_scan` | 2.46ms | 3.32ms | 1.350× | 2.58% | PASS |
| mem_reads | `select_random_points` | 7.75ms | 8.43ms | 1.088× | 5.22% | PASS |
| mem_reads | `select_random_ranges` | 2.06ms | 2.62ms | 1.272× | 2.16% | PASS |
| mem_reads | `covering_index_scan` | 2.33ms | 2.35ms | 1.009× | 2.93% | PASS |
| mem_reads | `groupby_scan` | 16.36ms | 17.70ms | 1.082× | 1.29% | PASS |
| mem_reads | `index_join` | 3.48ms | 4.63ms | 1.331× | 1.35% | PASS |
| mem_reads | `index_join_scan` | 1.82ms | 3.43ms | 1.882× | 2.83% | PASS |
| mem_reads | `types_table_scan` | 668.38ms | 741.17ms | 1.109× | 1.19% | PASS |
| mem_reads | `table_scan` | 790.67ms | 859.74ms | 1.087× | 0.81% | PASS |
| mem_reads | `oltp_read_only` | 59.60ms | 64.75ms | 1.086× | 2.03% | PASS |
| mem_writes | `oltp_bulk_insert` | 110.14ms | 145.19ms | 1.318× | 1.74% | PASS |
| mem_writes | `oltp_insert` | 9.37ms | 17.46ms | 1.864× | 0.94% | PASS |
| mem_writes | `oltp_update_index` | 31.48ms | 69.09ms | 2.195× | 0.93% | PASS |
| mem_writes | `oltp_update_non_index` | 22.07ms | 39.14ms | 1.774× | 1.24% | PASS |
| mem_writes | `oltp_delete_insert` | 26.98ms | 50.15ms | 1.859× | 1.44% | PASS |
| mem_writes | `oltp_write_only` | 13.67ms | 30.14ms | 2.206× | 1.26% | PASS |
| mem_writes | `types_delete_insert` | 15.74ms | 26.14ms | 1.660× | 1.79% | PASS |
| mem_writes | `oltp_read_write` | 37.16ms | 62.13ms | 1.672× | 1.03% | PASS |
| file_reads | `oltp_point_select` | 73.16ms | 37.63ms | 0.514× | 1.17% | PASS |
| file_reads | `oltp_range_select` | 12.40ms | 9.07ms | 0.732× | 1.71% | PASS |
| file_reads | `oltp_sum_range` | 12.00ms | 9.21ms | 0.768× | 2.35% | PASS |
| file_reads | `oltp_order_range` | 2.32ms | 1.96ms | 0.842× | 1.86% | PASS |
| file_reads | `oltp_distinct_range` | 2.89ms | 2.54ms | 0.880× | 1.91% | PASS |
| file_reads | `oltp_index_scan` | 8.88ms | 5.95ms | 0.670× | 1.89% | PASS |
| file_reads | `select_random_points` | 14.86ms | 11.29ms | 0.760× | 3.09% | PASS |
| file_reads | `select_random_ranges` | 8.05ms | 4.70ms | 0.584× | 2.03% | PASS |
| file_reads | `covering_index_scan` | 8.94ms | 5.19ms | 0.581× | 1.73% | PASS |
| file_reads | `groupby_scan` | 18.09ms | 19.15ms | 1.058× | 2.05% | PASS |
| file_reads | `index_join` | 7.35ms | 7.59ms | 1.032× | 4.48% | PASS |
| file_reads | `index_join_scan` | 3.21ms | 4.28ms | 1.330× | 5.66% | PASS |
| file_reads | `types_table_scan` | 695.90ms | 768.16ms | 1.104× | 1.56% | PASS |
| file_reads | `table_scan` | 809.01ms | 867.05ms | 1.072× | 1.24% | PASS |
| file_reads | `oltp_read_only` | 141.69ms | 93.55ms | 0.660× | 1.69% | PASS |
| file_writes | `oltp_bulk_insert` | 149.94ms | 198.69ms | 1.325× | 4.47% | PASS |
| file_writes | `oltp_insert` | 19.85ms | 28.71ms | 1.446× | 1.35% | PASS |
| file_writes | `oltp_update_index` | 121.49ms | 128.93ms | 1.061× | 5.45% | PASS |
| file_writes | `oltp_update_non_index` | 100.97ms | 87.50ms | 0.867× | 4.52% | PASS |
| file_writes | `oltp_delete_insert` | 107.19ms | 108.68ms | 1.014× | 4.14% | PASS |
| file_writes | `oltp_write_only` | 71.48ms | 74.55ms | 1.043× | 4.39% | PASS |
| file_writes | `types_delete_insert` | 60.71ms | 55.32ms | 0.911× | 8.91% | PASS |
| file_writes | `oltp_read_write` | 101.85ms | 109.41ms | 1.074× | 2.67% | PASS |
| ac_reads | `oltp_point_select` | 34.37ms | 37.81ms | 1.100× | 2.04% | PASS |
| ac_reads | `oltp_range_select` | 8.51ms | 9.02ms | 1.061× | 2.91% | PASS |
| ac_reads | `oltp_sum_range` | 8.01ms | 9.07ms | 1.132× | 1.63% | PASS |
| ac_reads | `oltp_order_range` | 1.97ms | 1.96ms | 0.995× | 2.70% | PASS |
| ac_reads | `oltp_distinct_range` | 2.42ms | 2.41ms | 0.996× | 1.43% | PASS |
| ac_reads | `oltp_index_scan` | 4.73ms | 5.60ms | 1.183× | 1.24% | PASS |
| ac_reads | `select_random_points` | 9.65ms | 10.57ms | 1.094× | 4.62% | PASS |
| ac_reads | `select_random_ranges` | 3.92ms | 4.47ms | 1.143× | 1.06% | PASS |
| ac_reads | `covering_index_scan` | 4.22ms | 4.38ms | 1.039× | 1.67% | PASS |
| ac_reads | `groupby_scan` | 16.49ms | 17.77ms | 1.077× | 0.90% | PASS |
| ac_reads | `index_join` | 4.53ms | 6.11ms | 1.348× | 2.08% | PASS |
| ac_reads | `index_join_scan` | 2.52ms | 3.80ms | 1.512× | 4.02% | PASS |
| ac_reads | `types_table_scan` | 702.45ms | 768.85ms | 1.095× | 0.87% | PASS |
| ac_reads | `table_scan` | 782.34ms | 849.13ms | 1.085× | 0.88% | PASS |
| ac_reads | `oltp_read_only` | 83.26ms | 91.33ms | 1.097× | 1.14% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 19.45ms | 52.26ms | 2.687× | 3.35% | PASS |
| ac_writes | `oltp_insert_ac` | 21.47ms | 64.56ms | 3.007× | 2.72% | PASS |
| ac_writes | `oltp_update_index_ac` | 24.69ms | 81.79ms | 3.313× | 4.90% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.10ms | 63.71ms | 3.020× | 4.97% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 21.42ms | 69.18ms | 3.230× | 3.77% | PASS |
| ac_writes | `oltp_write_only_ac` | 21.41ms | 67.27ms | 3.142× | 3.37% | PASS |
| ac_writes | `types_delete_insert_ac` | 19.74ms | 67.20ms | 3.405× | 4.13% | PASS |
| ac_writes | `oltp_read_write_ac` | 23.98ms | 72.95ms | 3.042× | 4.48% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 28.06ms | 31.42ms | 1.120× | 3.17% | PASS |
| mem_reads | `oltp_range_select` | 19.68ms | 17.46ms | 0.887× | 16.48% | PASS |
| mem_reads | `oltp_sum_range` | 12.97ms | 12.48ms | 0.962× | 11.57% | PASS |
| mem_reads | `oltp_order_range` | 3.26ms | 3.52ms | 1.080× | 16.83% | PASS |
| mem_reads | `oltp_distinct_range` | 3.60ms | 3.41ms | 0.949× | 3.31% | PASS |
| mem_reads | `oltp_index_scan` | 3.79ms | 4.92ms | 1.297× | 2.38% | PASS |
| mem_reads | `select_random_points` | 14.97ms | 16.32ms | 1.090× | 2.85% | PASS |
| mem_reads | `select_random_ranges` | 3.51ms | 4.25ms | 1.209× | 2.65% | PASS |
| mem_reads | `covering_index_scan` | 4.01ms | 3.78ms | 0.942× | 2.85% | PASS |
| mem_reads | `groupby_scan` | 27.55ms | 27.95ms | 1.015× | 0.72% | PASS |
| mem_reads | `index_join` | 5.83ms | 7.37ms | 1.263× | 5.83% | PASS |
| mem_reads | `index_join_scan` | 4.58ms | 4.87ms | 1.062× | 3.48% | PASS |
| mem_reads | `types_table_scan` | 1.37s | 1.29s | 0.941× | 9.38% | PASS |
| mem_reads | `table_scan` | 1.36s | 1.15s | 0.847× | 9.90% | PASS |
| mem_reads | `oltp_read_only` | 105.71ms | 104.92ms | 0.993× | 1.71% | PASS |
| mem_writes | `oltp_bulk_insert` | 182.40ms | 262.41ms | 1.439× | 1.40% | PASS |
| mem_writes | `oltp_insert` | 20.06ms | 31.84ms | 1.588× | 3.00% | PASS |
| mem_writes | `oltp_update_index` | 73.13ms | 128.14ms | 1.752× | 5.66% | PASS |
| mem_writes | `oltp_update_non_index` | 51.68ms | 86.56ms | 1.675× | 3.22% | PASS |
| mem_writes | `oltp_delete_insert` | 55.95ms | 104.95ms | 1.876× | 3.95% | PASS |
| mem_writes | `oltp_write_only` | 33.22ms | 63.81ms | 1.921× | 3.69% | PASS |
| mem_writes | `types_delete_insert` | 32.71ms | 50.25ms | 1.536× | 4.60% | PASS |
| mem_writes | `oltp_read_write` | 83.43ms | 114.95ms | 1.378× | 2.50% | PASS |
| file_reads | `oltp_point_select` | 102.11ms | 55.13ms | 0.540× | 1.14% | PASS |
| file_reads | `oltp_range_select` | 21.72ms | 14.14ms | 0.651× | 2.40% | PASS |
| file_reads | `oltp_sum_range` | 19.68ms | 14.00ms | 0.711× | 1.83% | PASS |
| file_reads | `oltp_order_range` | 4.01ms | 3.05ms | 0.760× | 8.96% | PASS |
| file_reads | `oltp_distinct_range` | 5.06ms | 4.16ms | 0.823× | 8.26% | PASS |
| file_reads | `oltp_index_scan` | 12.58ms | 8.49ms | 0.674× | 2.67% | PASS |
| file_reads | `select_random_points` | 25.59ms | 21.58ms | 0.843× | 4.24% | PASS |
| file_reads | `select_random_ranges` | 11.25ms | 6.81ms | 0.606× | 0.94% | PASS |
| file_reads | `covering_index_scan` | 13.13ms | 6.52ms | 0.497× | 1.05% | PASS |
| file_reads | `groupby_scan` | 28.46ms | 28.27ms | 0.993× | 0.98% | PASS |
| file_reads | `index_join` | 10.76ms | 9.44ms | 0.877× | 1.44% | PASS |
| file_reads | `index_join_scan` | 5.19ms | 5.46ms | 1.052× | 2.72% | PASS |
| file_reads | `types_table_scan` | 1.47s | 1.32s | 0.895× | 6.23% | PASS |
| file_reads | `table_scan` | 1.27s | 1.11s | 0.874× | 8.51% | PASS |
| file_reads | `oltp_read_only` | 208.78ms | 138.80ms | 0.665× | 1.23% | PASS |
| file_writes | `oltp_bulk_insert` | 262.09ms | 392.30ms | 1.497× | 5.52% | PASS |
| file_writes | `oltp_insert` | 60.83ms | 63.51ms | 1.044× | 8.94% | PASS |
| file_writes | `oltp_update_index` | 232.40ms | 238.84ms | 1.028× | 5.85% | PASS |
| file_writes | `oltp_update_non_index` | 171.30ms | 159.04ms | 0.928× | 5.04% | PASS |
| file_writes | `oltp_delete_insert` | 188.69ms | 168.41ms | 0.893× | 9.71% | PASS |
| file_writes | `oltp_write_only` | 121.30ms | 109.33ms | 0.901× | 3.21% | PASS |
| file_writes | `types_delete_insert` | 106.25ms | 89.53ms | 0.843× | 12.92% | PASS |
| file_writes | `oltp_read_write` | 191.16ms | 198.16ms | 1.037× | 5.91% | PASS |
| ac_reads | `oltp_point_select` | 52.14ms | 56.28ms | 1.079× | 2.05% | PASS |
| ac_reads | `oltp_range_select` | 17.89ms | 15.20ms | 0.849× | 9.21% | PASS |
| ac_reads | `oltp_sum_range` | 14.59ms | 14.06ms | 0.964× | 5.62% | PASS |
| ac_reads | `oltp_order_range` | 3.10ms | 2.84ms | 0.915× | 1.21% | PASS |
| ac_reads | `oltp_distinct_range` | 3.81ms | 3.62ms | 0.951× | 1.01% | PASS |
| ac_reads | `oltp_index_scan` | 6.83ms | 7.68ms | 1.124× | 0.90% | PASS |
| ac_reads | `select_random_points` | 18.00ms | 19.17ms | 1.065× | 1.67% | PASS |
| ac_reads | `select_random_ranges` | 6.18ms | 6.79ms | 1.099× | 0.82% | PASS |
| ac_reads | `covering_index_scan` | 7.91ms | 6.53ms | 0.826× | 1.82% | PASS |
| ac_reads | `groupby_scan` | 28.33ms | 28.67ms | 1.012× | 1.06% | PASS |
| ac_reads | `index_join` | 9.69ms | 11.77ms | 1.214× | 3.04% | PASS |
| ac_reads | `index_join_scan` | 5.52ms | 6.88ms | 1.246× | 6.21% | PASS |
| ac_reads | `types_table_scan` | 1.10s | 1.04s | 0.947× | 3.35% | PASS |
| ac_reads | `table_scan` | 1.57s | 1.28s | 0.816× | 12.33% | PASS |
| ac_reads | `oltp_read_only` | 165.52ms | 180.35ms | 1.090× | 3.69% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.70ms | 60.69ms | 2.932× | 24.81% | PASS |
| ac_writes | `oltp_insert_ac` | 23.08ms | 77.63ms | 3.363× | 10.78% | PASS |
| ac_writes | `oltp_update_index_ac` | 24.50ms | 97.80ms | 3.992× | 21.13% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.94ms | 75.97ms | 3.628× | 24.85% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.72ms | 98.08ms | 4.317× | 33.50% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.24ms | 80.53ms | 3.322× | 17.73% | PASS |
| ac_writes | `types_delete_insert_ac` | 19.33ms | 76.94ms | 3.979× | 20.38% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.30ms | 99.24ms | 3.386× | 20.34% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.06ms | 36.45ms | 1.254× | 1.66% | PASS |
| mem_reads | `oltp_range_select` | 12.25ms | 13.96ms | 1.140× | 2.00% | PASS |
| mem_reads | `oltp_sum_range` | 11.49ms | 13.83ms | 1.204× | 1.70% | PASS |
| mem_reads | `oltp_order_range` | 2.85ms | 3.13ms | 1.098× | 1.83% | PASS |
| mem_reads | `oltp_distinct_range` | 3.92ms | 4.19ms | 1.068× | 1.31% | PASS |
| mem_reads | `oltp_index_scan` | 4.38ms | 6.13ms | 1.398× | 1.47% | PASS |
| mem_reads | `select_random_points` | 16.94ms | 20.47ms | 1.208× | 1.39% | PASS |
| mem_reads | `select_random_ranges` | 3.84ms | 5.12ms | 1.332× | 2.60% | PASS |
| mem_reads | `covering_index_scan` | 4.32ms | 4.38ms | 1.015× | 1.85% | PASS |
| mem_reads | `groupby_scan` | 31.37ms | 34.19ms | 1.090× | 0.86% | PASS |
| mem_reads | `index_join` | 6.75ms | 8.88ms | 1.317× | 1.65% | PASS |
| mem_reads | `index_join_scan` | 4.12ms | 5.40ms | 1.311× | 2.06% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.22s | 1.180× | 0.70% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.37s | 1.165× | 1.06% | PASS |
| mem_reads | `oltp_read_only` | 114.25ms | 133.64ms | 1.170× | 1.10% | PASS |
| mem_writes | `oltp_bulk_insert` | 237.15ms | 348.29ms | 1.469× | 0.92% | PASS |
| mem_writes | `oltp_insert` | 19.64ms | 38.91ms | 1.982× | 0.85% | PASS |
| mem_writes | `oltp_update_index` | 65.99ms | 127.97ms | 1.939× | 1.34% | PASS |
| mem_writes | `oltp_update_non_index` | 49.30ms | 86.70ms | 1.759× | 2.31% | PASS |
| mem_writes | `oltp_delete_insert` | 49.21ms | 104.77ms | 2.129× | 1.54% | PASS |
| mem_writes | `oltp_write_only` | 27.14ms | 61.02ms | 2.249× | 1.24% | PASS |
| mem_writes | `types_delete_insert` | 31.19ms | 53.00ms | 1.699× | 1.24% | PASS |
| mem_writes | `oltp_read_write` | 80.66ms | 137.35ms | 1.703× | 1.75% | PASS |
| file_reads | `oltp_point_select` | 105.38ms | 63.42ms | 0.602× | 1.26% | PASS |
| file_reads | `oltp_range_select` | 19.65ms | 16.83ms | 0.856× | 2.25% | PASS |
| file_reads | `oltp_sum_range` | 19.32ms | 16.78ms | 0.869× | 1.83% | PASS |
| file_reads | `oltp_order_range` | 3.71ms | 3.49ms | 0.939× | 1.86% | PASS |
| file_reads | `oltp_distinct_range` | 4.82ms | 4.61ms | 0.957× | 2.58% | PASS |
| file_reads | `oltp_index_scan` | 11.90ms | 9.08ms | 0.763× | 1.41% | PASS |
| file_reads | `select_random_points` | 26.08ms | 24.34ms | 0.933× | 2.49% | PASS |
| file_reads | `select_random_ranges` | 11.53ms | 7.85ms | 0.681× | 1.36% | PASS |
| file_reads | `covering_index_scan` | 12.29ms | 7.35ms | 0.598× | 1.64% | PASS |
| file_reads | `groupby_scan` | 32.85ms | 34.87ms | 1.061× | 1.31% | PASS |
| file_reads | `index_join` | 10.98ms | 11.28ms | 1.027× | 1.80% | PASS |
| file_reads | `index_join_scan` | 5.13ms | 5.86ms | 1.141× | 1.93% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.22s | 1.180× | 0.64% | PASS |
| file_reads | `table_scan` | 1.23s | 1.38s | 1.122× | 2.86% | PASS |
| file_reads | `oltp_read_only` | 234.53ms | 176.82ms | 0.754× | 1.12% | PASS |
| file_writes | `oltp_bulk_insert` | 255.66ms | 374.27ms | 1.464× | 1.12% | PASS |
| file_writes | `oltp_insert` | 31.39ms | 51.81ms | 1.650× | 2.48% | PASS |
| file_writes | `oltp_update_index` | 104.76ms | 163.44ms | 1.560× | 1.29% | PASS |
| file_writes | `oltp_update_non_index` | 79.83ms | 109.73ms | 1.375× | 1.46% | PASS |
| file_writes | `oltp_delete_insert` | 82.71ms | 132.98ms | 1.608× | 1.65% | PASS |
| file_writes | `oltp_write_only` | 57.12ms | 85.15ms | 1.491× | 2.53% | PASS |
| file_writes | `types_delete_insert` | 53.44ms | 73.77ms | 1.380× | 2.34% | PASS |
| file_writes | `oltp_read_write` | 122.70ms | 167.17ms | 1.362× | 2.00% | PASS |
| ac_reads | `oltp_point_select` | 55.34ms | 63.59ms | 1.149× | 1.01% | PASS |
| ac_reads | `oltp_range_select` | 16.56ms | 17.09ms | 1.032× | 1.72% | PASS |
| ac_reads | `oltp_sum_range` | 15.67ms | 17.03ms | 1.086× | 2.17% | PASS |
| ac_reads | `oltp_order_range` | 3.43ms | 3.54ms | 1.031× | 1.36% | PASS |
| ac_reads | `oltp_distinct_range` | 4.55ms | 4.66ms | 1.024× | 1.12% | PASS |
| ac_reads | `oltp_index_scan` | 7.78ms | 9.51ms | 1.223× | 1.49% | PASS |
| ac_reads | `select_random_points` | 22.49ms | 25.33ms | 1.126× | 2.20% | PASS |
| ac_reads | `select_random_ranges` | 7.00ms | 7.95ms | 1.134× | 0.92% | PASS |
| ac_reads | `covering_index_scan` | 7.99ms | 7.46ms | 0.934× | 1.62% | PASS |
| ac_reads | `groupby_scan` | 32.88ms | 35.15ms | 1.069× | 0.63% | PASS |
| ac_reads | `index_join` | 9.32ms | 11.68ms | 1.252× | 1.84% | PASS |
| ac_reads | `index_join_scan` | 4.99ms | 6.29ms | 1.261× | 1.87% | PASS |
| ac_reads | `types_table_scan` | 1.20s | 1.28s | 1.070× | 0.60% | PASS |
| ac_reads | `table_scan` | 1.43s | 1.42s | 0.993× | 0.84% | PASS |
| ac_reads | `oltp_read_only` | 166.03ms | 180.28ms | 1.086× | 1.42% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 24.48ms | 70.79ms | 2.892× | 7.06% | PASS |
| ac_writes | `oltp_insert_ac` | 26.02ms | 91.85ms | 3.530× | 7.38% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.62ms | 102.84ms | 3.723× | 6.01% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.74ms | 80.34ms | 3.247× | 8.77% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.71ms | 96.43ms | 3.611× | 7.56% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.14ms | 94.47ms | 3.614× | 5.31% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.76ms | 83.93ms | 3.687× | 8.81% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.10ms | 97.51ms | 3.136× | 4.10% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.29ms | 40.72ms | 1.187× | 1.73% | PASS |
| mem_reads | `oltp_range_select` | 19.29ms | 21.10ms | 1.094× | 1.85% | PASS |
| mem_reads | `oltp_sum_range` | 18.39ms | 20.94ms | 1.138× | 1.35% | PASS |
| mem_reads | `oltp_order_range` | 3.57ms | 3.91ms | 1.094× | 1.38% | PASS |
| mem_reads | `oltp_distinct_range` | 4.70ms | 4.95ms | 1.052× | 0.97% | PASS |
| mem_reads | `oltp_index_scan` | 4.60ms | 6.09ms | 1.323× | 1.60% | PASS |
| mem_reads | `select_random_points` | 28.55ms | 31.90ms | 1.117× | 2.02% | PASS |
| mem_reads | `select_random_ranges` | 7.78ms | 8.99ms | 1.156× | 1.66% | PASS |
| mem_reads | `covering_index_scan` | 4.25ms | 4.25ms | 1.000× | 2.42% | PASS |
| mem_reads | `groupby_scan` | 36.61ms | 39.27ms | 1.073× | 0.78% | PASS |
| mem_reads | `index_join` | 8.27ms | 10.40ms | 1.258× | 1.10% | PASS |
| mem_reads | `index_join_scan` | 4.20ms | 5.44ms | 1.294× | 1.80% | PASS |
| mem_reads | `types_table_scan` | 1.10s | 1.25s | 1.142× | 2.38% | PASS |
| mem_reads | `table_scan` | 1.26s | 1.41s | 1.120× | 4.51% | PASS |
| mem_reads | `oltp_read_only` | 149.62ms | 169.64ms | 1.134× | 0.82% | PASS |
| mem_writes | `oltp_bulk_insert` | 248.51ms | 355.46ms | 1.430× | 0.85% | PASS |
| mem_writes | `oltp_insert` | 19.30ms | 36.38ms | 1.885× | 0.68% | PASS |
| mem_writes | `oltp_update_index` | 68.26ms | 115.61ms | 1.694× | 1.43% | PASS |
| mem_writes | `oltp_update_non_index` | 50.84ms | 82.19ms | 1.617× | 1.13% | PASS |
| mem_writes | `oltp_delete_insert` | 49.83ms | 94.42ms | 1.895× | 0.98% | PASS |
| mem_writes | `oltp_write_only` | 27.15ms | 57.05ms | 2.101× | 1.07% | PASS |
| mem_writes | `types_delete_insert` | 33.24ms | 54.61ms | 1.643× | 1.42% | PASS |
| mem_writes | `oltp_read_write` | 103.90ms | 155.28ms | 1.495× | 1.64% | PASS |
| file_reads | `oltp_point_select` | 109.48ms | 67.37ms | 0.615× | 1.31% | PASS |
| file_reads | `oltp_range_select` | 27.58ms | 24.34ms | 0.883× | 1.52% | PASS |
| file_reads | `oltp_sum_range` | 26.47ms | 24.08ms | 0.910× | 1.09% | PASS |
| file_reads | `oltp_order_range` | 4.46ms | 4.30ms | 0.965× | 1.35% | PASS |
| file_reads | `oltp_distinct_range` | 5.65ms | 5.38ms | 0.952× | 1.23% | PASS |
| file_reads | `oltp_index_scan` | 12.32ms | 9.17ms | 0.745× | 1.15% | PASS |
| file_reads | `select_random_points` | 37.64ms | 35.87ms | 0.953× | 1.49% | PASS |
| file_reads | `select_random_ranges` | 15.76ms | 12.10ms | 0.768× | 1.17% | PASS |
| file_reads | `covering_index_scan` | 11.93ms | 7.23ms | 0.606× | 0.82% | PASS |
| file_reads | `groupby_scan` | 37.73ms | 40.08ms | 1.062× | 1.30% | PASS |
| file_reads | `index_join` | 12.56ms | 12.91ms | 1.027× | 1.06% | PASS |
| file_reads | `index_join_scan` | 5.30ms | 5.95ms | 1.124× | 1.46% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.23s | 1.187× | 0.46% | PASS |
| file_reads | `table_scan` | 1.19s | 1.39s | 1.169× | 0.69% | PASS |
| file_reads | `oltp_read_only` | 266.06ms | 212.01ms | 0.797× | 1.45% | PASS |
| file_writes | `oltp_bulk_insert` | 263.86ms | 379.09ms | 1.437× | 1.00% | PASS |
| file_writes | `oltp_insert` | 26.08ms | 46.28ms | 1.775× | 1.39% | PASS |
| file_writes | `oltp_update_index` | 98.15ms | 142.41ms | 1.451× | 0.85% | PASS |
| file_writes | `oltp_update_non_index` | 77.87ms | 104.47ms | 1.342× | 1.24% | PASS |
| file_writes | `oltp_delete_insert` | 79.52ms | 121.84ms | 1.532× | 1.74% | PASS |
| file_writes | `oltp_write_only` | 53.15ms | 78.86ms | 1.484× | 1.81% | PASS |
| file_writes | `types_delete_insert` | 51.43ms | 68.93ms | 1.340× | 1.55% | PASS |
| file_writes | `oltp_read_write` | 131.62ms | 177.79ms | 1.351× | 1.42% | PASS |
| ac_reads | `oltp_point_select` | 58.67ms | 67.42ms | 1.149× | 1.09% | PASS |
| ac_reads | `oltp_range_select` | 22.11ms | 24.28ms | 1.098× | 1.26% | PASS |
| ac_reads | `oltp_sum_range` | 20.78ms | 24.03ms | 1.157× | 0.85% | PASS |
| ac_reads | `oltp_order_range` | 3.99ms | 4.30ms | 1.078× | 1.91% | PASS |
| ac_reads | `oltp_distinct_range` | 5.08ms | 5.38ms | 1.057× | 1.14% | PASS |
| ac_reads | `oltp_index_scan` | 7.29ms | 9.20ms | 1.263× | 1.69% | PASS |
| ac_reads | `select_random_points` | 31.57ms | 35.89ms | 1.137× | 1.06% | PASS |
| ac_reads | `select_random_ranges` | 10.40ms | 12.06ms | 1.160× | 1.21% | PASS |
| ac_reads | `covering_index_scan` | 6.92ms | 7.20ms | 1.041× | 1.32% | PASS |
| ac_reads | `groupby_scan` | 36.62ms | 39.99ms | 1.092× | 0.91% | PASS |
| ac_reads | `index_join` | 9.88ms | 12.89ms | 1.304× | 1.28% | PASS |
| ac_reads | `index_join_scan` | 4.79ms | 5.98ms | 1.249× | 1.66% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.23s | 1.184× | 0.79% | PASS |
| ac_reads | `table_scan` | 1.24s | 1.40s | 1.131× | 2.69% | PASS |
| ac_reads | `oltp_read_only` | 197.49ms | 214.76ms | 1.087× | 1.19% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.39ms | 64.98ms | 3.038× | 5.75% | PASS |
| ac_writes | `oltp_insert_ac` | 23.68ms | 84.03ms | 3.548× | 6.21% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.23ms | 95.88ms | 3.800× | 6.09% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.66ms | 74.47ms | 3.439× | 4.08% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.11ms | 87.28ms | 3.776× | 5.31% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.40ms | 85.09ms | 3.636× | 3.91% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.57ms | 75.19ms | 3.656× | 5.52% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.71ms | 93.70ms | 3.051× | 4.52% | PASS |

</details>

## Version-control latency

Wall time: 1m 48s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 62.37ms | 200.00ms | 31.2% | 0.68% | PASS |
| `status_dirty_many_tables` | 64.87ms | 200.00ms | 32.4% | 0.74% | PASS |
| `diff_regular_working_one_table` | 58.31ms | 150.00ms | 38.9% | 0.61% | PASS |
| `diff_regular_working_many_tables` | 69.69ms | 200.00ms | 34.8% | 0.80% | PASS |
| `diff_stat_working_many_tables` | 69.80ms | 200.00ms | 34.9% | 0.61% | PASS |
| `diff_schema_working_many_tables` | 69.97ms | 200.00ms | 35.0% | 0.78% | PASS |
| `branch_list_many_branches` | 19.25ms | 100.00ms | 19.2% | 1.43% | PASS |
| `branch_create_delete` | 20.91ms | 100.00ms | 20.9% | 1.39% | PASS |
| `checkout_branch_clean` | 45.55ms | 200.00ms | 22.8% | 0.83% | PASS |
| `merge_data_no_conflicts` | 23.78ms | 150.00ms | 15.9% | 1.36% | PASS |
| `merge_schema_no_conflicts` | 18.96ms | 100.00ms | 19.0% | 1.13% | PASS |
| `merge_data_conflicts` | 74.99ms | 250.00ms | 30.0% | 0.63% | PASS |
| `merge_data_conflicts_with_resolve` | 75.17ms | 250.00ms | 30.1% | 0.54% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
