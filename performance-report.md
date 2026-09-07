# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-09-07 16:45 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260831.293.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/34136856393)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 4s | 9.31s | 11.09s | 1.191× | 1.09% | **PASS** |
| textpk | 69 | 55 | 1h 32m 12s | 9.56s | 11.83s | 1.238× | 1.68% | **PASS** |
| blobpk | 69 | 55 | 1h 15m 10s | 8.07s | 9.72s | 1.205× | 1.79% | **PASS** |
| compositepk | 69 | 55 | 1h 26m 48s | 9.56s | 12.02s | 1.257× | 1.59% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.25ms | 28.18ms | 1.116× | 1.56% | PASS |
| mem_reads | `oltp_range_select` | 10.96ms | 12.11ms | 1.104× | 1.35% | PASS |
| mem_reads | `oltp_sum_range` | 9.99ms | 11.63ms | 1.164× | 1.40% | PASS |
| mem_reads | `oltp_order_range` | 2.70ms | 2.88ms | 1.069× | 1.05% | PASS |
| mem_reads | `oltp_distinct_range` | 3.73ms | 3.90ms | 1.044× | 0.76% | PASS |
| mem_reads | `oltp_index_scan` | 3.95ms | 4.87ms | 1.232× | 1.24% | PASS |
| mem_reads | `select_random_points` | 10.54ms | 11.38ms | 1.080× | 1.47% | PASS |
| mem_reads | `select_random_ranges` | 3.09ms | 4.03ms | 1.303× | 1.53% | PASS |
| mem_reads | `covering_index_scan` | 4.36ms | 4.13ms | 0.946× | 1.76% | PASS |
| mem_reads | `groupby_scan` | 32.09ms | 34.34ms | 1.070× | 0.79% | PASS |
| mem_reads | `index_join` | 5.92ms | 7.72ms | 1.304× | 1.25% | PASS |
| mem_reads | `index_join_scan` | 3.50ms | 4.78ms | 1.366× | 1.18% | PASS |
| mem_reads | `types_table_scan` | 1.11s | 1.26s | 1.140× | 0.45% | PASS |
| mem_reads | `table_scan` | 1.26s | 1.37s | 1.092× | 0.42% | PASS |
| mem_reads | `oltp_read_only` | 102.88ms | 115.02ms | 1.118× | 1.06% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.32ms | 241.42ms | 1.346× | 1.19% | PASS |
| mem_writes | `oltp_insert` | 15.78ms | 28.13ms | 1.783× | 1.06% | PASS |
| mem_writes | `oltp_update_index` | 50.88ms | 106.05ms | 2.084× | 0.81% | PASS |
| mem_writes | `oltp_update_non_index` | 34.63ms | 58.48ms | 1.688× | 0.87% | PASS |
| mem_writes | `oltp_delete_insert` | 44.43ms | 78.55ms | 1.768× | 0.80% | PASS |
| mem_writes | `oltp_write_only` | 21.75ms | 45.12ms | 2.074× | 0.87% | PASS |
| mem_writes | `types_delete_insert` | 24.51ms | 39.83ms | 1.625× | 1.20% | PASS |
| mem_writes | `oltp_read_write` | 64.61ms | 104.90ms | 1.624× | 0.91% | PASS |
| file_reads | `oltp_point_select` | 120.07ms | 59.26ms | 0.494× | 0.86% | PASS |
| file_reads | `oltp_range_select` | 20.55ms | 15.20ms | 0.740× | 1.06% | PASS |
| file_reads | `oltp_sum_range` | 19.50ms | 14.77ms | 0.758× | 0.79% | PASS |
| file_reads | `oltp_order_range` | 3.74ms | 3.24ms | 0.867× | 0.90% | PASS |
| file_reads | `oltp_distinct_range` | 4.80ms | 4.27ms | 0.890× | 0.75% | PASS |
| file_reads | `oltp_index_scan` | 13.93ms | 8.56ms | 0.615× | 1.10% | PASS |
| file_reads | `select_random_points` | 20.85ms | 14.82ms | 0.711× | 1.40% | PASS |
| file_reads | `select_random_ranges` | 12.71ms | 7.23ms | 0.569× | 0.64% | PASS |
| file_reads | `covering_index_scan` | 14.35ms | 7.51ms | 0.523× | 0.85% | PASS |
| file_reads | `groupby_scan` | 33.30ms | 34.76ms | 1.044× | 0.73% | PASS |
| file_reads | `index_join` | 11.36ms | 9.95ms | 0.876× | 1.42% | PASS |
| file_reads | `index_join_scan` | 4.64ms | 5.23ms | 1.127× | 1.40% | PASS |
| file_reads | `types_table_scan` | 1.10s | 1.26s | 1.149× | 0.59% | PASS |
| file_reads | `table_scan` | 1.25s | 1.37s | 1.098× | 0.53% | PASS |
| file_reads | `oltp_read_only` | 239.61ms | 159.91ms | 0.667× | 0.73% | PASS |
| file_writes | `oltp_bulk_insert` | 194.63ms | 259.12ms | 1.331× | 0.82% | PASS |
| file_writes | `oltp_insert` | 22.30ms | 35.98ms | 1.613× | 1.51% | PASS |
| file_writes | `oltp_update_index` | 79.75ms | 129.58ms | 1.625× | 1.13% | PASS |
| file_writes | `oltp_update_non_index` | 60.03ms | 82.76ms | 1.379× | 1.71% | PASS |
| file_writes | `oltp_delete_insert` | 69.48ms | 101.16ms | 1.456× | 1.33% | PASS |
| file_writes | `oltp_write_only` | 45.60ms | 67.33ms | 1.477× | 2.17% | PASS |
| file_writes | `types_delete_insert` | 41.23ms | 54.33ms | 1.318× | 1.40% | PASS |
| file_writes | `oltp_read_write` | 89.53ms | 126.94ms | 1.418× | 1.45% | PASS |
| ac_reads | `oltp_point_select` | 55.85ms | 59.25ms | 1.061× | 0.81% | PASS |
| ac_reads | `oltp_range_select` | 14.50ms | 15.28ms | 1.054× | 1.57% | PASS |
| ac_reads | `oltp_sum_range` | 13.48ms | 14.94ms | 1.108× | 1.23% | PASS |
| ac_reads | `oltp_order_range` | 3.19ms | 3.27ms | 1.024× | 0.76% | PASS |
| ac_reads | `oltp_distinct_range` | 4.19ms | 4.28ms | 1.020× | 0.65% | PASS |
| ac_reads | `oltp_index_scan` | 7.63ms | 8.68ms | 1.137× | 0.88% | PASS |
| ac_reads | `select_random_points` | 14.75ms | 15.06ms | 1.021× | 1.07% | PASS |
| ac_reads | `select_random_ranges` | 6.46ms | 7.27ms | 1.126× | 0.90% | PASS |
| ac_reads | `covering_index_scan` | 8.00ms | 7.75ms | 0.968× | 0.94% | PASS |
| ac_reads | `groupby_scan` | 33.16ms | 35.04ms | 1.057× | 0.80% | PASS |
| ac_reads | `index_join` | 8.02ms | 10.17ms | 1.268× | 1.80% | PASS |
| ac_reads | `index_join_scan` | 4.01ms | 5.07ms | 1.265× | 1.29% | PASS |
| ac_reads | `types_table_scan` | 1.10s | 1.26s | 1.150× | 0.51% | PASS |
| ac_reads | `table_scan` | 1.25s | 1.37s | 1.098× | 0.59% | PASS |
| ac_reads | `oltp_read_only` | 148.38ms | 160.42ms | 1.081× | 1.09% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.97ms | 66.63ms | 3.927× | 5.08% | PASS |
| ac_writes | `oltp_insert_ac` | 18.76ms | 83.25ms | 4.439× | 4.35% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.32ms | 99.49ms | 4.897× | 4.54% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.11ms | 75.72ms | 4.426× | 5.22% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 19.61ms | 92.52ms | 4.719× | 7.31% | PASS |
| ac_writes | `oltp_write_only_ac` | 19.82ms | 91.99ms | 4.641× | 5.75% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.41ms | 79.26ms | 4.553× | 5.69% | PASS |
| ac_writes | `oltp_read_write_ac` | 26.03ms | 103.08ms | 3.960× | 6.34% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.15ms | 38.20ms | 1.267× | 2.29% | PASS |
| mem_reads | `oltp_range_select` | 14.71ms | 14.40ms | 0.979× | 5.07% | PASS |
| mem_reads | `oltp_sum_range` | 11.86ms | 14.51ms | 1.224× | 2.55% | PASS |
| mem_reads | `oltp_order_range` | 2.99ms | 3.19ms | 1.065× | 1.16% | PASS |
| mem_reads | `oltp_distinct_range` | 4.05ms | 4.30ms | 1.061× | 1.02% | PASS |
| mem_reads | `oltp_index_scan` | 4.32ms | 6.28ms | 1.455× | 1.68% | PASS |
| mem_reads | `select_random_points` | 17.42ms | 21.21ms | 1.217× | 2.10% | PASS |
| mem_reads | `select_random_ranges` | 3.74ms | 5.21ms | 1.393× | 2.36% | PASS |
| mem_reads | `covering_index_scan` | 4.50ms | 4.42ms | 0.982× | 1.61% | PASS |
| mem_reads | `groupby_scan` | 31.28ms | 33.78ms | 1.080× | 1.21% | PASS |
| mem_reads | `index_join` | 6.83ms | 9.10ms | 1.332× | 1.67% | PASS |
| mem_reads | `index_join_scan` | 4.70ms | 5.45ms | 1.161× | 4.33% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.23s | 1.174× | 0.61% | PASS |
| mem_reads | `table_scan` | 1.21s | 1.36s | 1.131× | 0.44% | PASS |
| mem_reads | `oltp_read_only` | 116.83ms | 138.28ms | 1.184× | 1.22% | PASS |
| mem_writes | `oltp_bulk_insert` | 234.94ms | 355.85ms | 1.515× | 0.52% | PASS |
| mem_writes | `oltp_insert` | 21.70ms | 40.10ms | 1.847× | 0.97% | PASS |
| mem_writes | `oltp_update_index` | 71.60ms | 134.43ms | 1.878× | 1.25% | PASS |
| mem_writes | `oltp_update_non_index` | 48.67ms | 86.53ms | 1.778× | 0.96% | PASS |
| mem_writes | `oltp_delete_insert` | 50.56ms | 103.50ms | 2.047× | 0.98% | PASS |
| mem_writes | `oltp_write_only` | 28.79ms | 61.79ms | 2.146× | 0.77% | PASS |
| mem_writes | `types_delete_insert` | 33.59ms | 55.08ms | 1.640× | 1.42% | PASS |
| mem_writes | `oltp_read_write` | 85.05ms | 140.47ms | 1.652× | 1.03% | PASS |
| file_reads | `oltp_point_select` | 104.07ms | 65.05ms | 0.625× | 1.24% | PASS |
| file_reads | `oltp_range_select` | 21.57ms | 17.46ms | 0.809× | 2.14% | PASS |
| file_reads | `oltp_sum_range` | 19.83ms | 17.59ms | 0.887× | 1.21% | PASS |
| file_reads | `oltp_order_range` | 3.85ms | 3.56ms | 0.924× | 1.64% | PASS |
| file_reads | `oltp_distinct_range` | 4.95ms | 4.68ms | 0.945× | 1.44% | PASS |
| file_reads | `oltp_index_scan` | 11.98ms | 9.35ms | 0.781× | 1.39% | PASS |
| file_reads | `select_random_points` | 26.37ms | 25.05ms | 0.950× | 1.67% | PASS |
| file_reads | `select_random_ranges` | 11.40ms | 8.04ms | 0.706× | 1.56% | PASS |
| file_reads | `covering_index_scan` | 12.06ms | 7.45ms | 0.618× | 1.56% | PASS |
| file_reads | `groupby_scan` | 32.53ms | 34.31ms | 1.055× | 0.93% | PASS |
| file_reads | `index_join` | 11.13ms | 11.59ms | 1.041× | 2.32% | PASS |
| file_reads | `index_join_scan` | 6.06ms | 6.05ms | 0.998× | 3.13% | PASS |
| file_reads | `types_table_scan` | 1.05s | 1.22s | 1.165× | 0.99% | PASS |
| file_reads | `table_scan` | 1.23s | 1.36s | 1.108× | 1.32% | PASS |
| file_reads | `oltp_read_only` | 230.53ms | 177.74ms | 0.771× | 1.22% | PASS |
| file_writes | `oltp_bulk_insert` | 256.19ms | 387.18ms | 1.511× | 0.92% | PASS |
| file_writes | `oltp_insert` | 49.47ms | 53.91ms | 1.090× | 24.52% | PASS |
| file_writes | `oltp_update_index` | 113.95ms | 171.51ms | 1.505× | 1.77% | PASS |
| file_writes | `oltp_update_non_index` | 97.09ms | 115.31ms | 1.188× | 11.23% | PASS |
| file_writes | `oltp_delete_insert` | 93.67ms | 136.33ms | 1.455× | 1.88% | PASS |
| file_writes | `oltp_write_only` | 84.11ms | 86.51ms | 1.028× | 10.34% | PASS |
| file_writes | `types_delete_insert` | 56.12ms | 74.94ms | 1.335× | 1.93% | PASS |
| file_writes | `oltp_read_write` | 140.51ms | 165.18ms | 1.176× | 8.15% | PASS |
| ac_reads | `oltp_point_select` | 54.46ms | 64.94ms | 1.192× | 1.06% | PASS |
| ac_reads | `oltp_range_select` | 17.50ms | 17.45ms | 0.997× | 2.27% | PASS |
| ac_reads | `oltp_sum_range` | 15.13ms | 17.69ms | 1.169× | 1.97% | PASS |
| ac_reads | `oltp_order_range` | 3.47ms | 3.56ms | 1.026× | 1.68% | PASS |
| ac_reads | `oltp_distinct_range` | 4.55ms | 4.68ms | 1.028× | 1.68% | PASS |
| ac_reads | `oltp_index_scan` | 7.16ms | 9.33ms | 1.302× | 2.62% | PASS |
| ac_reads | `select_random_points` | 21.27ms | 25.29ms | 1.189× | 1.98% | PASS |
| ac_reads | `select_random_ranges` | 6.59ms | 8.02ms | 1.218× | 1.22% | PASS |
| ac_reads | `covering_index_scan` | 7.52ms | 7.42ms | 0.987× | 3.24% | PASS |
| ac_reads | `groupby_scan` | 32.27ms | 34.40ms | 1.066× | 1.07% | PASS |
| ac_reads | `index_join` | 8.72ms | 11.54ms | 1.323× | 2.37% | PASS |
| ac_reads | `index_join_scan` | 5.41ms | 5.98ms | 1.105× | 3.07% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.22s | 1.168× | 0.49% | PASS |
| ac_reads | `table_scan` | 1.21s | 1.36s | 1.125× | 0.52% | PASS |
| ac_reads | `oltp_read_only` | 159.58ms | 179.66ms | 1.126× | 2.14% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.68ms | 84.71ms | 3.735× | 5.99% | PASS |
| ac_writes | `oltp_insert_ac` | 25.48ms | 100.85ms | 3.958× | 4.76% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.00ms | 117.19ms | 4.041× | 4.57% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.61ms | 93.08ms | 3.782× | 5.26% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.47ms | 105.93ms | 4.160× | 4.84% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.52ms | 111.03ms | 4.187× | 6.38% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.73ms | 98.94ms | 4.353× | 9.32% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.03ms | 114.87ms | 3.478× | 5.43% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 21.47ms | 24.23ms | 1.129× | 1.35% | PASS |
| mem_reads | `oltp_range_select` | 8.71ms | 10.44ms | 1.198× | 2.89% | PASS |
| mem_reads | `oltp_sum_range` | 8.21ms | 9.64ms | 1.175× | 3.28% | PASS |
| mem_reads | `oltp_order_range` | 2.16ms | 2.46ms | 1.140× | 2.17% | PASS |
| mem_reads | `oltp_distinct_range` | 3.00ms | 3.26ms | 1.088× | 1.87% | PASS |
| mem_reads | `oltp_index_scan` | 3.13ms | 3.94ms | 1.257× | 2.26% | PASS |
| mem_reads | `select_random_points` | 12.65ms | 14.77ms | 1.168× | 2.47% | PASS |
| mem_reads | `select_random_ranges` | 2.64ms | 3.68ms | 1.394× | 3.55% | PASS |
| mem_reads | `covering_index_scan` | 3.11ms | 3.09ms | 0.994× | 1.47% | PASS |
| mem_reads | `groupby_scan` | 25.54ms | 26.62ms | 1.042× | 1.45% | PASS |
| mem_reads | `index_join` | 5.37ms | 6.92ms | 1.288× | 1.13% | PASS |
| mem_reads | `index_join_scan` | 2.94ms | 4.37ms | 1.488× | 3.00% | PASS |
| mem_reads | `types_table_scan` | 872.07ms | 1.01s | 1.162× | 0.26% | PASS |
| mem_reads | `table_scan` | 1.01s | 1.10s | 1.095× | 0.24% | PASS |
| mem_reads | `oltp_read_only` | 88.24ms | 98.43ms | 1.115× | 0.77% | PASS |
| mem_writes | `oltp_bulk_insert` | 162.11ms | 225.86ms | 1.393× | 0.45% | PASS |
| mem_writes | `oltp_insert` | 14.37ms | 26.42ms | 1.838× | 1.41% | PASS |
| mem_writes | `oltp_update_index` | 47.68ms | 89.54ms | 1.878× | 1.49% | PASS |
| mem_writes | `oltp_update_non_index` | 33.49ms | 56.34ms | 1.682× | 1.61% | PASS |
| mem_writes | `oltp_delete_insert` | 34.17ms | 69.72ms | 2.041× | 1.21% | PASS |
| mem_writes | `oltp_write_only` | 17.77ms | 40.93ms | 2.303× | 1.84% | PASS |
| mem_writes | `types_delete_insert` | 22.68ms | 35.84ms | 1.580× | 2.00% | PASS |
| mem_writes | `oltp_read_write` | 56.95ms | 93.20ms | 1.637× | 1.96% | PASS |
| file_reads | `oltp_point_select` | 47.17ms | 33.00ms | 0.700× | 1.13% | PASS |
| file_reads | `oltp_range_select` | 11.45ms | 11.41ms | 0.996× | 1.66% | PASS |
| file_reads | `oltp_sum_range` | 11.38ms | 10.68ms | 0.939× | 1.98% | PASS |
| file_reads | `oltp_order_range` | 2.50ms | 2.58ms | 1.031× | 2.19% | PASS |
| file_reads | `oltp_distinct_range` | 3.30ms | 3.37ms | 1.021× | 1.10% | PASS |
| file_reads | `oltp_index_scan` | 5.92ms | 4.89ms | 0.826× | 1.74% | PASS |
| file_reads | `select_random_points` | 16.41ms | 16.62ms | 1.013× | 1.48% | PASS |
| file_reads | `select_random_ranges` | 5.51ms | 4.65ms | 0.844× | 1.84% | PASS |
| file_reads | `covering_index_scan` | 5.90ms | 3.94ms | 0.668× | 1.15% | PASS |
| file_reads | `groupby_scan` | 25.63ms | 26.62ms | 1.039× | 1.09% | PASS |
| file_reads | `index_join` | 6.65ms | 7.30ms | 1.098× | 1.27% | PASS |
| file_reads | `index_join_scan` | 3.58ms | 4.80ms | 1.341× | 2.30% | PASS |
| file_reads | `types_table_scan` | 876.36ms | 1.02s | 1.160× | 0.20% | PASS |
| file_reads | `table_scan` | 1.01s | 1.11s | 1.093× | 0.20% | PASS |
| file_reads | `oltp_read_only` | 125.95ms | 111.64ms | 0.886× | 0.55% | PASS |
| file_writes | `oltp_bulk_insert` | 221.31ms | 305.13ms | 1.379× | 2.67% | PASS |
| file_writes | `oltp_insert` | 39.63ms | 54.37ms | 1.372× | 2.57% | PASS |
| file_writes | `oltp_update_index` | 172.44ms | 178.79ms | 1.037× | 1.50% | PASS |
| file_writes | `oltp_update_non_index` | 133.91ms | 121.83ms | 0.910× | 2.53% | PASS |
| file_writes | `oltp_delete_insert` | 142.32ms | 144.36ms | 1.014× | 1.78% | PASS |
| file_writes | `oltp_write_only` | 103.71ms | 100.67ms | 0.971× | 1.22% | PASS |
| file_writes | `types_delete_insert` | 89.90ms | 81.45ms | 0.906× | 3.32% | PASS |
| file_writes | `oltp_read_write` | 145.03ms | 155.49ms | 1.072× | 1.00% | PASS |
| ac_reads | `oltp_point_select` | 30.86ms | 33.84ms | 1.097× | 1.15% | PASS |
| ac_reads | `oltp_range_select` | 11.93ms | 12.15ms | 1.019× | 2.11% | PASS |
| ac_reads | `oltp_sum_range` | 10.93ms | 11.21ms | 1.025× | 2.97% | PASS |
| ac_reads | `oltp_order_range` | 2.52ms | 2.65ms | 1.053× | 1.79% | PASS |
| ac_reads | `oltp_distinct_range` | 3.35ms | 3.46ms | 1.035× | 1.94% | PASS |
| ac_reads | `oltp_index_scan` | 4.63ms | 5.48ms | 1.184× | 1.55% | PASS |
| ac_reads | `select_random_points` | 15.84ms | 17.47ms | 1.103× | 1.80% | PASS |
| ac_reads | `select_random_ranges` | 3.91ms | 4.74ms | 1.211× | 1.98% | PASS |
| ac_reads | `covering_index_scan` | 4.08ms | 3.99ms | 0.977× | 1.96% | PASS |
| ac_reads | `groupby_scan` | 25.49ms | 26.61ms | 1.044× | 1.38% | PASS |
| ac_reads | `index_join` | 5.78ms | 7.25ms | 1.256× | 1.27% | PASS |
| ac_reads | `index_join_scan` | 2.93ms | 3.85ms | 1.316× | 1.91% | PASS |
| ac_reads | `types_table_scan` | 873.69ms | 1.02s | 1.163× | 0.29% | PASS |
| ac_reads | `table_scan` | 1.01s | 1.11s | 1.093× | 0.29% | PASS |
| ac_reads | `oltp_read_only` | 99.15ms | 110.44ms | 1.114× | 0.62% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 34.95ms | 97.49ms | 2.790× | 4.89% | PASS |
| ac_writes | `oltp_insert_ac` | 36.28ms | 111.19ms | 3.064× | 5.60% | PASS |
| ac_writes | `oltp_update_index_ac` | 37.02ms | 117.57ms | 3.175× | 5.05% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 35.12ms | 106.01ms | 3.019× | 5.71% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 36.01ms | 110.30ms | 3.063× | 3.42% | PASS |
| ac_writes | `oltp_write_only_ac` | 35.98ms | 110.76ms | 3.078× | 6.29% | PASS |
| ac_writes | `types_delete_insert_ac` | 36.35ms | 104.95ms | 2.887× | 6.56% | PASS |
| ac_writes | `oltp_read_write_ac` | 41.56ms | 120.44ms | 2.898× | 5.74% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.76ms | 40.32ms | 1.231× | 1.85% | PASS |
| mem_reads | `oltp_range_select` | 18.43ms | 21.05ms | 1.142× | 2.60% | PASS |
| mem_reads | `oltp_sum_range` | 17.55ms | 20.79ms | 1.185× | 1.77% | PASS |
| mem_reads | `oltp_order_range` | 3.52ms | 3.83ms | 1.088× | 2.11% | PASS |
| mem_reads | `oltp_distinct_range` | 4.71ms | 4.91ms | 1.044× | 1.07% | PASS |
| mem_reads | `oltp_index_scan` | 4.44ms | 6.12ms | 1.379× | 2.70% | PASS |
| mem_reads | `select_random_points` | 26.97ms | 31.90ms | 1.183× | 2.06% | PASS |
| mem_reads | `select_random_ranges` | 7.63ms | 9.07ms | 1.189× | 1.62% | PASS |
| mem_reads | `covering_index_scan` | 4.26ms | 4.43ms | 1.039× | 2.28% | PASS |
| mem_reads | `groupby_scan` | 36.61ms | 38.72ms | 1.058× | 1.24% | PASS |
| mem_reads | `index_join` | 8.19ms | 10.59ms | 1.293× | 1.62% | PASS |
| mem_reads | `index_join_scan` | 4.19ms | 5.42ms | 1.291× | 1.26% | PASS |
| mem_reads | `types_table_scan` | 1.05s | 1.24s | 1.185× | 1.10% | PASS |
| mem_reads | `table_scan` | 1.20s | 1.39s | 1.160× | 1.02% | PASS |
| mem_reads | `oltp_read_only` | 148.08ms | 168.29ms | 1.136× | 0.93% | PASS |
| mem_writes | `oltp_bulk_insert` | 248.66ms | 361.21ms | 1.453× | 1.08% | PASS |
| mem_writes | `oltp_insert` | 19.50ms | 36.87ms | 1.891× | 1.82% | PASS |
| mem_writes | `oltp_update_index` | 70.05ms | 120.03ms | 1.713× | 3.05% | PASS |
| mem_writes | `oltp_update_non_index` | 50.92ms | 83.19ms | 1.634× | 1.55% | PASS |
| mem_writes | `oltp_delete_insert` | 49.69ms | 96.24ms | 1.937× | 1.26% | PASS |
| mem_writes | `oltp_write_only` | 26.86ms | 57.47ms | 2.139× | 1.10% | PASS |
| mem_writes | `types_delete_insert` | 32.78ms | 55.11ms | 1.681× | 0.85% | PASS |
| mem_writes | `oltp_read_write` | 101.58ms | 155.35ms | 1.529× | 0.69% | PASS |
| file_reads | `oltp_point_select` | 109.14ms | 66.58ms | 0.610× | 0.90% | PASS |
| file_reads | `oltp_range_select` | 27.28ms | 24.23ms | 0.888× | 1.31% | PASS |
| file_reads | `oltp_sum_range` | 26.14ms | 24.13ms | 0.923× | 1.38% | PASS |
| file_reads | `oltp_order_range` | 4.52ms | 4.25ms | 0.941× | 1.61% | PASS |
| file_reads | `oltp_distinct_range` | 5.67ms | 5.33ms | 0.942× | 1.30% | PASS |
| file_reads | `oltp_index_scan` | 12.58ms | 9.08ms | 0.721× | 1.19% | PASS |
| file_reads | `select_random_points` | 38.09ms | 36.49ms | 0.958× | 2.01% | PASS |
| file_reads | `select_random_ranges` | 16.19ms | 12.23ms | 0.755× | 1.72% | PASS |
| file_reads | `covering_index_scan` | 12.46ms | 7.30ms | 0.586× | 2.08% | PASS |
| file_reads | `groupby_scan` | 37.85ms | 39.71ms | 1.049× | 1.03% | PASS |
| file_reads | `index_join` | 12.71ms | 12.73ms | 1.002× | 2.02% | PASS |
| file_reads | `index_join_scan` | 5.14ms | 5.90ms | 1.147× | 2.55% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.23s | 1.196× | 0.91% | PASS |
| file_reads | `table_scan` | 1.18s | 1.38s | 1.175× | 0.54% | PASS |
| file_reads | `oltp_read_only` | 259.03ms | 209.14ms | 0.807× | 1.10% | PASS |
| file_writes | `oltp_bulk_insert` | 262.34ms | 378.92ms | 1.444× | 0.85% | PASS |
| file_writes | `oltp_insert` | 26.40ms | 46.12ms | 1.747× | 1.71% | PASS |
| file_writes | `oltp_update_index` | 97.84ms | 143.40ms | 1.466× | 1.62% | PASS |
| file_writes | `oltp_update_non_index` | 76.71ms | 104.11ms | 1.357× | 1.61% | PASS |
| file_writes | `oltp_delete_insert` | 76.83ms | 118.89ms | 1.548× | 1.28% | PASS |
| file_writes | `oltp_write_only` | 51.05ms | 76.98ms | 1.508× | 2.01% | PASS |
| file_writes | `types_delete_insert` | 49.96ms | 67.88ms | 1.359× | 1.34% | PASS |
| file_writes | `oltp_read_write` | 127.01ms | 174.55ms | 1.374× | 1.45% | PASS |
| ac_reads | `oltp_point_select` | 58.23ms | 66.69ms | 1.145× | 1.37% | PASS |
| ac_reads | `oltp_range_select` | 21.73ms | 24.28ms | 1.117× | 2.96% | PASS |
| ac_reads | `oltp_sum_range` | 20.33ms | 24.15ms | 1.188× | 1.07% | PASS |
| ac_reads | `oltp_order_range` | 3.95ms | 4.26ms | 1.079× | 1.57% | PASS |
| ac_reads | `oltp_distinct_range` | 5.04ms | 5.33ms | 1.059× | 1.69% | PASS |
| ac_reads | `oltp_index_scan` | 7.06ms | 9.00ms | 1.275× | 1.71% | PASS |
| ac_reads | `select_random_points` | 30.57ms | 36.47ms | 1.193× | 1.66% | PASS |
| ac_reads | `select_random_ranges` | 10.18ms | 12.04ms | 1.183× | 0.82% | PASS |
| ac_reads | `covering_index_scan` | 6.78ms | 7.07ms | 1.042× | 1.59% | PASS |
| ac_reads | `groupby_scan` | 36.29ms | 39.28ms | 1.082× | 0.85% | PASS |
| ac_reads | `index_join` | 9.63ms | 12.74ms | 1.323× | 1.14% | PASS |
| ac_reads | `index_join_scan` | 4.49ms | 5.96ms | 1.328× | 2.49% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.23s | 1.190× | 0.51% | PASS |
| ac_reads | `table_scan` | 1.22s | 1.39s | 1.139× | 1.45% | PASS |
| ac_reads | `oltp_read_only` | 185.88ms | 209.57ms | 1.127× | 0.85% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.50ms | 79.98ms | 3.555× | 5.61% | PASS |
| ac_writes | `oltp_insert_ac` | 25.20ms | 103.06ms | 4.090× | 5.91% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.40ms | 111.46ms | 4.222× | 3.75% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.00ms | 91.48ms | 3.978× | 5.60% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.81ms | 102.76ms | 4.317× | 4.99% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.11ms | 103.74ms | 4.132× | 4.25% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.76ms | 91.64ms | 4.026× | 7.16% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.39ms | 112.16ms | 3.574× | 4.21% | PASS |

</details>

## Version-control latency

Wall time: 2m 1s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 66.20ms | 200.00ms | 33.1% | 0.67% | PASS |
| `status_dirty_many_tables` | 68.25ms | 200.00ms | 34.1% | 0.58% | PASS |
| `diff_regular_working_one_table` | 62.02ms | 150.00ms | 41.3% | 0.80% | PASS |
| `diff_regular_working_many_tables` | 71.64ms | 200.00ms | 35.8% | 0.39% | PASS |
| `diff_stat_working_many_tables` | 71.56ms | 200.00ms | 35.8% | 0.46% | PASS |
| `diff_schema_working_many_tables` | 72.08ms | 200.00ms | 36.0% | 0.52% | PASS |
| `branch_list_many_branches` | 18.75ms | 100.00ms | 18.7% | 1.77% | PASS |
| `branch_create_delete` | 20.49ms | 100.00ms | 20.5% | 2.29% | PASS |
| `checkout_branch_clean` | 81.03ms | 200.00ms | 40.5% | 3.29% | PASS |
| `merge_data_no_conflicts` | 31.18ms | 150.00ms | 20.8% | 2.64% | PASS |
| `merge_schema_no_conflicts` | 19.50ms | 100.00ms | 19.5% | 3.05% | PASS |
| `merge_data_conflicts` | 100.10ms | 250.00ms | 40.0% | 0.32% | PASS |
| `merge_data_conflicts_with_resolve` | 99.91ms | 250.00ms | 40.0% | 0.28% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
