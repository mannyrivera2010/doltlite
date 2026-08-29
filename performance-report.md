# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-08-29 16:11 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20260823.283.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/33258144496)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 25s | 9.46s | 11.11s | 1.175× | 1.25% | **PASS** |
| textpk | 69 | 55 | 1h 17m 51s | 8.22s | 9.71s | 1.181× | 2.56% | **PASS** |
| blobpk | 69 | 55 | 1h 29m 28s | 9.23s | 11.61s | 1.259× | 1.28% | **PASS** |
| compositepk | 69 | 55 | 1h 28m 18s | 10.05s | 12.21s | 1.215× | 1.93% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.34ms | 27.54ms | 1.131× | 1.72% | PASS |
| mem_reads | `oltp_range_select` | 10.61ms | 11.93ms | 1.125× | 1.34% | PASS |
| mem_reads | `oltp_sum_range` | 9.72ms | 11.35ms | 1.168× | 0.95% | PASS |
| mem_reads | `oltp_order_range` | 2.67ms | 2.83ms | 1.063× | 1.08% | PASS |
| mem_reads | `oltp_distinct_range` | 3.74ms | 3.85ms | 1.028× | 0.85% | PASS |
| mem_reads | `oltp_index_scan` | 3.98ms | 4.92ms | 1.236× | 1.70% | PASS |
| mem_reads | `select_random_points` | 10.58ms | 11.18ms | 1.057× | 1.38% | PASS |
| mem_reads | `select_random_ranges` | 3.14ms | 4.01ms | 1.277× | 1.20% | PASS |
| mem_reads | `covering_index_scan` | 4.36ms | 4.16ms | 0.954× | 1.67% | PASS |
| mem_reads | `groupby_scan` | 31.76ms | 34.44ms | 1.084× | 0.60% | PASS |
| mem_reads | `index_join` | 6.02ms | 8.30ms | 1.379× | 2.81% | PASS |
| mem_reads | `index_join_scan` | 3.57ms | 4.81ms | 1.346× | 1.61% | PASS |
| mem_reads | `types_table_scan` | 1.14s | 1.28s | 1.122× | 0.74% | PASS |
| mem_reads | `table_scan` | 1.26s | 1.37s | 1.085× | 0.83% | PASS |
| mem_reads | `oltp_read_only` | 104.03ms | 114.78ms | 1.103× | 0.87% | PASS |
| mem_writes | `oltp_bulk_insert` | 179.34ms | 241.67ms | 1.348× | 1.08% | PASS |
| mem_writes | `oltp_insert` | 15.79ms | 28.27ms | 1.791× | 1.15% | PASS |
| mem_writes | `oltp_update_index` | 50.80ms | 105.36ms | 2.074× | 0.69% | PASS |
| mem_writes | `oltp_update_non_index` | 34.69ms | 58.34ms | 1.682× | 0.68% | PASS |
| mem_writes | `oltp_delete_insert` | 44.37ms | 78.20ms | 1.763× | 0.59% | PASS |
| mem_writes | `oltp_write_only` | 21.70ms | 45.03ms | 2.075× | 0.94% | PASS |
| mem_writes | `types_delete_insert` | 24.57ms | 40.01ms | 1.628× | 1.18% | PASS |
| mem_writes | `oltp_read_write` | 65.02ms | 104.24ms | 1.603× | 0.90% | PASS |
| file_reads | `oltp_point_select` | 119.98ms | 58.93ms | 0.491× | 0.88% | PASS |
| file_reads | `oltp_range_select` | 20.68ms | 15.20ms | 0.735× | 1.59% | PASS |
| file_reads | `oltp_sum_range` | 19.66ms | 14.69ms | 0.747× | 1.48% | PASS |
| file_reads | `oltp_order_range` | 3.77ms | 3.25ms | 0.862× | 1.25% | PASS |
| file_reads | `oltp_distinct_range` | 4.81ms | 4.24ms | 0.881× | 0.82% | PASS |
| file_reads | `oltp_index_scan` | 13.86ms | 8.58ms | 0.619× | 0.80% | PASS |
| file_reads | `select_random_points` | 20.81ms | 14.74ms | 0.708× | 1.25% | PASS |
| file_reads | `select_random_ranges` | 12.71ms | 7.17ms | 0.564× | 0.75% | PASS |
| file_reads | `covering_index_scan` | 14.26ms | 7.51ms | 0.527× | 1.73% | PASS |
| file_reads | `groupby_scan` | 32.98ms | 34.88ms | 1.058× | 0.87% | PASS |
| file_reads | `index_join` | 11.23ms | 9.81ms | 0.873× | 1.40% | PASS |
| file_reads | `index_join_scan` | 4.49ms | 5.06ms | 1.128× | 1.52% | PASS |
| file_reads | `types_table_scan` | 1.10s | 1.26s | 1.142× | 0.64% | PASS |
| file_reads | `table_scan` | 1.25s | 1.37s | 1.092× | 0.57% | PASS |
| file_reads | `oltp_read_only` | 246.50ms | 163.48ms | 0.663× | 1.10% | PASS |
| file_writes | `oltp_bulk_insert` | 195.02ms | 263.87ms | 1.353× | 1.26% | PASS |
| file_writes | `oltp_insert` | 22.28ms | 36.38ms | 1.633× | 1.19% | PASS |
| file_writes | `oltp_update_index` | 80.28ms | 131.38ms | 1.637× | 0.92% | PASS |
| file_writes | `oltp_update_non_index` | 60.36ms | 82.72ms | 1.370× | 1.44% | PASS |
| file_writes | `oltp_delete_insert` | 69.49ms | 100.09ms | 1.440× | 1.27% | PASS |
| file_writes | `oltp_write_only` | 46.85ms | 68.33ms | 1.459× | 2.52% | PASS |
| file_writes | `types_delete_insert` | 42.58ms | 55.05ms | 1.293× | 1.49% | PASS |
| file_writes | `oltp_read_write` | 88.45ms | 125.98ms | 1.424× | 1.49% | PASS |
| ac_reads | `oltp_point_select` | 55.62ms | 59.24ms | 1.065× | 0.93% | PASS |
| ac_reads | `oltp_range_select` | 14.34ms | 15.18ms | 1.059× | 1.32% | PASS |
| ac_reads | `oltp_sum_range` | 13.40ms | 14.78ms | 1.102× | 1.39% | PASS |
| ac_reads | `oltp_order_range` | 3.17ms | 3.25ms | 1.024× | 0.99% | PASS |
| ac_reads | `oltp_distinct_range` | 4.16ms | 4.24ms | 1.019× | 1.04% | PASS |
| ac_reads | `oltp_index_scan` | 7.42ms | 8.57ms | 1.155× | 1.37% | PASS |
| ac_reads | `select_random_points` | 14.53ms | 14.77ms | 1.017× | 1.85% | PASS |
| ac_reads | `select_random_ranges` | 6.43ms | 7.24ms | 1.125× | 0.84% | PASS |
| ac_reads | `covering_index_scan` | 7.87ms | 7.72ms | 0.981× | 1.28% | PASS |
| ac_reads | `groupby_scan` | 32.59ms | 35.02ms | 1.074× | 0.89% | PASS |
| ac_reads | `index_join` | 8.13ms | 10.32ms | 1.269× | 1.41% | PASS |
| ac_reads | `index_join_scan` | 4.09ms | 5.26ms | 1.287× | 1.36% | PASS |
| ac_reads | `types_table_scan` | 1.12s | 1.27s | 1.141× | 1.14% | PASS |
| ac_reads | `table_scan` | 1.33s | 1.40s | 1.049× | 1.55% | PASS |
| ac_reads | `oltp_read_only` | 152.00ms | 161.36ms | 1.062× | 0.99% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.34ms | 64.15ms | 3.926× | 4.66% | PASS |
| ac_writes | `oltp_insert_ac` | 18.41ms | 82.18ms | 4.464× | 4.34% | PASS |
| ac_writes | `oltp_update_index_ac` | 20.14ms | 101.33ms | 5.030× | 4.10% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 17.13ms | 74.70ms | 4.361× | 5.29% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 18.09ms | 87.54ms | 4.838× | 4.55% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.72ms | 87.62ms | 4.680× | 4.51% | PASS |
| ac_writes | `types_delete_insert_ac` | 17.06ms | 75.84ms | 4.446× | 5.96% | PASS |
| ac_writes | `oltp_read_write_ac` | 23.66ms | 93.39ms | 3.947× | 2.60% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.34ms | 25.22ms | 1.129× | 1.60% | PASS |
| mem_reads | `oltp_range_select` | 10.27ms | 10.71ms | 1.044× | 5.10% | PASS |
| mem_reads | `oltp_sum_range` | 9.83ms | 10.09ms | 1.027× | 4.63% | PASS |
| mem_reads | `oltp_order_range` | 2.40ms | 2.50ms | 1.041× | 2.31% | PASS |
| mem_reads | `oltp_distinct_range` | 3.17ms | 3.29ms | 1.040× | 1.98% | PASS |
| mem_reads | `oltp_index_scan` | 3.39ms | 4.15ms | 1.222× | 2.51% | PASS |
| mem_reads | `select_random_points` | 13.42ms | 15.58ms | 1.161× | 2.66% | PASS |
| mem_reads | `select_random_ranges` | 2.63ms | 3.62ms | 1.378× | 3.63% | PASS |
| mem_reads | `covering_index_scan` | 3.30ms | 3.01ms | 0.911× | 0.76% | PASS |
| mem_reads | `groupby_scan` | 26.26ms | 26.66ms | 1.015× | 2.09% | PASS |
| mem_reads | `index_join` | 5.66ms | 7.01ms | 1.238× | 1.85% | PASS |
| mem_reads | `index_join_scan` | 3.47ms | 4.53ms | 1.303× | 8.68% | PASS |
| mem_reads | `types_table_scan` | 881.51ms | 1.02s | 1.153× | 0.20% | PASS |
| mem_reads | `table_scan` | 1.03s | 1.11s | 1.071× | 0.30% | PASS |
| mem_reads | `oltp_read_only` | 90.29ms | 100.32ms | 1.111× | 0.79% | PASS |
| mem_writes | `oltp_bulk_insert` | 157.15ms | 231.44ms | 1.473× | 0.42% | PASS |
| mem_writes | `oltp_insert` | 16.02ms | 26.94ms | 1.681× | 0.66% | PASS |
| mem_writes | `oltp_update_index` | 52.71ms | 94.71ms | 1.797× | 1.29% | PASS |
| mem_writes | `oltp_update_non_index` | 33.01ms | 58.54ms | 1.774× | 2.85% | PASS |
| mem_writes | `oltp_delete_insert` | 35.61ms | 71.39ms | 2.005× | 2.51% | PASS |
| mem_writes | `oltp_write_only` | 19.81ms | 41.83ms | 2.111× | 4.05% | PASS |
| mem_writes | `types_delete_insert` | 23.49ms | 37.46ms | 1.595× | 1.37% | PASS |
| mem_writes | `oltp_read_write` | 60.22ms | 96.31ms | 1.599× | 1.73% | PASS |
| file_reads | `oltp_point_select` | 48.05ms | 34.52ms | 0.719× | 1.36% | PASS |
| file_reads | `oltp_range_select` | 13.20ms | 12.02ms | 0.911× | 3.57% | PASS |
| file_reads | `oltp_sum_range` | 12.94ms | 11.47ms | 0.887× | 3.58% | PASS |
| file_reads | `oltp_order_range` | 2.65ms | 2.65ms | 0.999× | 3.62% | PASS |
| file_reads | `oltp_distinct_range` | 3.55ms | 3.47ms | 0.977× | 2.02% | PASS |
| file_reads | `oltp_index_scan` | 6.33ms | 5.61ms | 0.887× | 2.81% | PASS |
| file_reads | `select_random_points` | 17.83ms | 18.38ms | 1.031× | 1.34% | PASS |
| file_reads | `select_random_ranges` | 5.88ms | 5.00ms | 0.851× | 1.88% | PASS |
| file_reads | `covering_index_scan` | 6.32ms | 4.43ms | 0.702× | 5.42% | PASS |
| file_reads | `groupby_scan` | 27.44ms | 27.30ms | 0.995× | 1.60% | PASS |
| file_reads | `index_join` | 7.27ms | 7.77ms | 1.069× | 2.78% | PASS |
| file_reads | `index_join_scan` | 3.65ms | 4.71ms | 1.293× | 9.29% | PASS |
| file_reads | `types_table_scan` | 885.26ms | 1.02s | 1.152× | 0.28% | PASS |
| file_reads | `table_scan` | 1.03s | 1.11s | 1.079× | 0.33% | PASS |
| file_reads | `oltp_read_only` | 127.13ms | 112.81ms | 0.887× | 0.79% | PASS |
| file_writes | `oltp_bulk_insert` | 211.70ms | 307.93ms | 1.455× | 3.07% | PASS |
| file_writes | `oltp_insert` | 50.61ms | 54.89ms | 1.085× | 2.27% | PASS |
| file_writes | `oltp_update_index` | 191.99ms | 188.75ms | 0.983× | 4.93% | PASS |
| file_writes | `oltp_update_non_index` | 140.39ms | 124.69ms | 0.888× | 1.53% | PASS |
| file_writes | `oltp_delete_insert` | 154.30ms | 149.41ms | 0.968× | 2.14% | PASS |
| file_writes | `oltp_write_only` | 110.83ms | 103.66ms | 0.935× | 1.45% | PASS |
| file_writes | `types_delete_insert` | 90.12ms | 78.97ms | 0.876× | 2.89% | PASS |
| file_writes | `oltp_read_write` | 158.28ms | 161.36ms | 1.019× | 2.56% | PASS |
| ac_reads | `oltp_point_select` | 30.93ms | 34.80ms | 1.125× | 1.43% | PASS |
| ac_reads | `oltp_range_select` | 11.96ms | 12.23ms | 1.023× | 4.30% | PASS |
| ac_reads | `oltp_sum_range` | 11.14ms | 11.40ms | 1.023× | 4.08% | PASS |
| ac_reads | `oltp_order_range` | 2.54ms | 2.67ms | 1.053× | 3.62% | PASS |
| ac_reads | `oltp_distinct_range` | 3.23ms | 3.41ms | 1.057× | 2.33% | PASS |
| ac_reads | `oltp_index_scan` | 4.30ms | 5.14ms | 1.195× | 3.02% | PASS |
| ac_reads | `select_random_points` | 14.51ms | 16.70ms | 1.151× | 3.64% | PASS |
| ac_reads | `select_random_ranges` | 4.11ms | 4.84ms | 1.176× | 3.32% | PASS |
| ac_reads | `covering_index_scan` | 4.57ms | 4.19ms | 0.918× | 7.35% | PASS |
| ac_reads | `groupby_scan` | 27.59ms | 27.25ms | 0.988× | 1.16% | PASS |
| ac_reads | `index_join` | 6.54ms | 7.89ms | 1.207× | 2.69% | PASS |
| ac_reads | `index_join_scan` | 3.79ms | 4.60ms | 1.215× | 7.45% | PASS |
| ac_reads | `types_table_scan` | 884.33ms | 1.02s | 1.152× | 0.29% | PASS |
| ac_reads | `table_scan` | 1.03s | 1.11s | 1.077× | 0.31% | PASS |
| ac_reads | `oltp_read_only` | 101.64ms | 112.82ms | 1.110× | 0.83% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 30.66ms | 84.38ms | 2.752× | 5.48% | PASS |
| ac_writes | `oltp_insert_ac` | 33.69ms | 92.53ms | 2.746× | 7.69% | PASS |
| ac_writes | `oltp_update_index_ac` | 36.47ms | 111.53ms | 3.058× | 6.35% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 31.31ms | 92.10ms | 2.942× | 6.74% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 32.61ms | 101.27ms | 3.105× | 7.11% | PASS |
| ac_writes | `oltp_write_only_ac` | 32.33ms | 99.36ms | 3.073× | 4.29% | PASS |
| ac_writes | `types_delete_insert_ac` | 30.64ms | 91.85ms | 2.997× | 6.78% | PASS |
| ac_writes | `oltp_read_write_ac` | 35.61ms | 105.31ms | 2.958× | 4.55% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.13ms | 36.25ms | 1.244× | 1.01% | PASS |
| mem_reads | `oltp_range_select` | 11.88ms | 13.71ms | 1.154× | 2.08% | PASS |
| mem_reads | `oltp_sum_range` | 11.20ms | 13.64ms | 1.218× | 1.69% | PASS |
| mem_reads | `oltp_order_range` | 2.76ms | 3.09ms | 1.120× | 1.00% | PASS |
| mem_reads | `oltp_distinct_range` | 3.87ms | 4.18ms | 1.080× | 1.17% | PASS |
| mem_reads | `oltp_index_scan` | 4.32ms | 5.94ms | 1.374× | 1.49% | PASS |
| mem_reads | `select_random_points` | 16.63ms | 20.00ms | 1.203× | 1.25% | PASS |
| mem_reads | `select_random_ranges` | 3.73ms | 5.04ms | 1.351× | 2.06% | PASS |
| mem_reads | `covering_index_scan` | 4.36ms | 4.30ms | 0.987× | 1.03% | PASS |
| mem_reads | `groupby_scan` | 31.14ms | 33.49ms | 1.075× | 1.02% | PASS |
| mem_reads | `index_join` | 6.70ms | 8.60ms | 1.284× | 1.27% | PASS |
| mem_reads | `index_join_scan` | 3.94ms | 5.06ms | 1.284× | 2.18% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.22s | 1.171× | 0.65% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.36s | 1.158× | 0.59% | PASS |
| mem_reads | `oltp_read_only` | 113.66ms | 134.01ms | 1.179× | 0.99% | PASS |
| mem_writes | `oltp_bulk_insert` | 242.60ms | 351.10ms | 1.447× | 0.92% | PASS |
| mem_writes | `oltp_insert` | 19.82ms | 39.19ms | 1.978× | 0.62% | PASS |
| mem_writes | `oltp_update_index` | 66.45ms | 127.14ms | 1.913× | 1.14% | PASS |
| mem_writes | `oltp_update_non_index` | 47.04ms | 82.57ms | 1.755× | 0.89% | PASS |
| mem_writes | `oltp_delete_insert` | 47.76ms | 100.94ms | 2.113× | 1.14% | PASS |
| mem_writes | `oltp_write_only` | 26.78ms | 60.57ms | 2.262× | 0.83% | PASS |
| mem_writes | `types_delete_insert` | 31.33ms | 53.06ms | 1.694× | 1.06% | PASS |
| mem_writes | `oltp_read_write` | 79.94ms | 136.08ms | 1.702× | 1.09% | PASS |
| file_reads | `oltp_point_select` | 105.26ms | 62.65ms | 0.595× | 0.85% | PASS |
| file_reads | `oltp_range_select` | 20.20ms | 16.52ms | 0.818× | 2.91% | PASS |
| file_reads | `oltp_sum_range` | 19.72ms | 16.48ms | 0.836× | 2.05% | PASS |
| file_reads | `oltp_order_range` | 3.69ms | 3.44ms | 0.933× | 3.10% | PASS |
| file_reads | `oltp_distinct_range` | 4.90ms | 4.57ms | 0.933× | 1.90% | PASS |
| file_reads | `oltp_index_scan` | 12.10ms | 8.97ms | 0.741× | 2.07% | PASS |
| file_reads | `select_random_points` | 26.11ms | 23.72ms | 0.909× | 1.42% | PASS |
| file_reads | `select_random_ranges` | 11.56ms | 7.81ms | 0.675× | 1.70% | PASS |
| file_reads | `covering_index_scan` | 12.64ms | 7.28ms | 0.576× | 2.74% | PASS |
| file_reads | `groupby_scan` | 32.55ms | 34.04ms | 1.046× | 1.19% | PASS |
| file_reads | `index_join` | 11.08ms | 11.03ms | 0.996× | 2.96% | PASS |
| file_reads | `index_join_scan` | 5.11ms | 5.61ms | 1.098× | 2.46% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.21s | 1.171× | 0.42% | PASS |
| file_reads | `table_scan` | 1.18s | 1.36s | 1.152× | 0.37% | PASS |
| file_reads | `oltp_read_only` | 225.27ms | 172.72ms | 0.767× | 0.75% | PASS |
| file_writes | `oltp_bulk_insert` | 261.67ms | 375.93ms | 1.437× | 0.76% | PASS |
| file_writes | `oltp_insert` | 31.30ms | 51.69ms | 1.651× | 1.40% | PASS |
| file_writes | `oltp_update_index` | 101.73ms | 160.62ms | 1.579× | 1.13% | PASS |
| file_writes | `oltp_update_non_index` | 77.24ms | 106.70ms | 1.381× | 1.10% | PASS |
| file_writes | `oltp_delete_insert` | 79.30ms | 128.78ms | 1.624× | 0.95% | PASS |
| file_writes | `oltp_write_only` | 54.17ms | 83.13ms | 1.534× | 1.68% | PASS |
| file_writes | `types_delete_insert` | 51.26ms | 71.44ms | 1.394× | 1.77% | PASS |
| file_writes | `oltp_read_write` | 111.33ms | 158.73ms | 1.426× | 1.16% | PASS |
| ac_reads | `oltp_point_select` | 54.95ms | 62.60ms | 1.139× | 0.93% | PASS |
| ac_reads | `oltp_range_select` | 15.52ms | 16.55ms | 1.066× | 1.66% | PASS |
| ac_reads | `oltp_sum_range` | 14.71ms | 16.55ms | 1.125× | 1.45% | PASS |
| ac_reads | `oltp_order_range` | 3.20ms | 3.44ms | 1.076× | 1.28% | PASS |
| ac_reads | `oltp_distinct_range` | 4.26ms | 4.57ms | 1.071× | 1.04% | PASS |
| ac_reads | `oltp_index_scan` | 7.21ms | 8.96ms | 1.242× | 1.40% | PASS |
| ac_reads | `select_random_points` | 20.84ms | 23.66ms | 1.135× | 1.54% | PASS |
| ac_reads | `select_random_ranges` | 6.64ms | 7.78ms | 1.171× | 1.32% | PASS |
| ac_reads | `covering_index_scan` | 7.27ms | 7.31ms | 1.006× | 2.61% | PASS |
| ac_reads | `groupby_scan` | 31.68ms | 34.06ms | 1.075× | 1.01% | PASS |
| ac_reads | `index_join` | 8.56ms | 11.02ms | 1.287× | 1.70% | PASS |
| ac_reads | `index_join_scan` | 4.63ms | 5.60ms | 1.211× | 1.55% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.22s | 1.173× | 0.56% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.36s | 1.152× | 0.53% | PASS |
| ac_reads | `oltp_read_only` | 152.07ms | 172.82ms | 1.136× | 0.63% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.02ms | 77.05ms | 3.499× | 5.05% | PASS |
| ac_writes | `oltp_insert_ac` | 23.98ms | 98.36ms | 4.102× | 4.60% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.96ms | 109.27ms | 4.208× | 3.02% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.80ms | 90.09ms | 4.133× | 4.50% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.20ms | 100.46ms | 4.151× | 5.38% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.96ms | 99.00ms | 4.132× | 4.06% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.59ms | 91.58ms | 4.242× | 4.23% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.64ms | 105.70ms | 3.566× | 4.07% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.14ms | 41.30ms | 1.210× | 1.60% | PASS |
| mem_reads | `oltp_range_select` | 19.65ms | 21.52ms | 1.095× | 2.41% | PASS |
| mem_reads | `oltp_sum_range` | 18.35ms | 21.17ms | 1.153× | 1.35% | PASS |
| mem_reads | `oltp_order_range` | 3.62ms | 3.89ms | 1.076× | 1.25% | PASS |
| mem_reads | `oltp_distinct_range` | 4.75ms | 5.00ms | 1.052× | 1.15% | PASS |
| mem_reads | `oltp_index_scan` | 4.73ms | 6.44ms | 1.360× | 1.97% | PASS |
| mem_reads | `select_random_points` | 28.61ms | 32.49ms | 1.136× | 2.16% | PASS |
| mem_reads | `select_random_ranges` | 7.66ms | 9.11ms | 1.188× | 1.93% | PASS |
| mem_reads | `covering_index_scan` | 4.27ms | 4.49ms | 1.051× | 2.80% | PASS |
| mem_reads | `groupby_scan` | 36.58ms | 39.27ms | 1.074× | 0.88% | PASS |
| mem_reads | `index_join` | 8.24ms | 10.94ms | 1.328× | 1.68% | PASS |
| mem_reads | `index_join_scan` | 4.22ms | 5.58ms | 1.324× | 2.09% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.25s | 1.162× | 2.20% | PASS |
| mem_reads | `table_scan` | 1.30s | 1.41s | 1.088× | 3.76% | PASS |
| mem_reads | `oltp_read_only` | 158.25ms | 174.07ms | 1.100× | 0.89% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.91ms | 362.38ms | 1.450× | 1.25% | PASS |
| mem_writes | `oltp_insert` | 19.37ms | 37.08ms | 1.914× | 0.81% | PASS |
| mem_writes | `oltp_update_index` | 70.49ms | 121.99ms | 1.731× | 1.00% | PASS |
| mem_writes | `oltp_update_non_index` | 52.91ms | 86.31ms | 1.631× | 1.37% | PASS |
| mem_writes | `oltp_delete_insert` | 51.75ms | 99.83ms | 1.929× | 1.33% | PASS |
| mem_writes | `oltp_write_only` | 28.30ms | 60.37ms | 2.134× | 1.31% | PASS |
| mem_writes | `types_delete_insert` | 33.76ms | 56.79ms | 1.682× | 1.63% | PASS |
| mem_writes | `oltp_read_write` | 108.46ms | 160.50ms | 1.480× | 1.05% | PASS |
| file_reads | `oltp_point_select` | 110.50ms | 67.34ms | 0.609× | 1.30% | PASS |
| file_reads | `oltp_range_select` | 28.18ms | 24.80ms | 0.880× | 2.13% | PASS |
| file_reads | `oltp_sum_range` | 26.57ms | 24.38ms | 0.918× | 2.02% | PASS |
| file_reads | `oltp_order_range` | 4.56ms | 4.36ms | 0.955× | 2.62% | PASS |
| file_reads | `oltp_distinct_range` | 5.83ms | 5.49ms | 0.942× | 2.00% | PASS |
| file_reads | `oltp_index_scan` | 12.62ms | 9.33ms | 0.739× | 1.10% | PASS |
| file_reads | `select_random_points` | 38.53ms | 36.62ms | 0.951× | 2.47% | PASS |
| file_reads | `select_random_ranges` | 15.86ms | 12.18ms | 0.768× | 1.84% | PASS |
| file_reads | `covering_index_scan` | 12.21ms | 7.30ms | 0.598× | 1.58% | PASS |
| file_reads | `groupby_scan` | 37.83ms | 40.20ms | 1.063× | 0.76% | PASS |
| file_reads | `index_join` | 12.70ms | 13.14ms | 1.035× | 1.40% | PASS |
| file_reads | `index_join_scan` | 5.38ms | 6.22ms | 1.156× | 2.05% | PASS |
| file_reads | `types_table_scan` | 1.08s | 1.25s | 1.151× | 2.26% | PASS |
| file_reads | `table_scan` | 1.30s | 1.41s | 1.088× | 4.62% | PASS |
| file_reads | `oltp_read_only` | 272.34ms | 214.40ms | 0.787× | 1.30% | PASS |
| file_writes | `oltp_bulk_insert` | 264.11ms | 383.44ms | 1.452× | 1.03% | PASS |
| file_writes | `oltp_insert` | 26.47ms | 46.69ms | 1.764× | 1.40% | PASS |
| file_writes | `oltp_update_index` | 100.33ms | 147.86ms | 1.474× | 1.40% | PASS |
| file_writes | `oltp_update_non_index` | 78.93ms | 108.19ms | 1.371× | 1.95% | PASS |
| file_writes | `oltp_delete_insert` | 78.79ms | 123.65ms | 1.569× | 1.55% | PASS |
| file_writes | `oltp_write_only` | 52.63ms | 80.14ms | 1.523× | 1.94% | PASS |
| file_writes | `types_delete_insert` | 50.99ms | 70.53ms | 1.383× | 1.41% | PASS |
| file_writes | `oltp_read_write` | 135.73ms | 180.16ms | 1.327× | 1.53% | PASS |
| ac_reads | `oltp_point_select` | 59.58ms | 67.57ms | 1.134× | 0.92% | PASS |
| ac_reads | `oltp_range_select` | 22.75ms | 24.74ms | 1.087× | 2.76% | PASS |
| ac_reads | `oltp_sum_range` | 21.53ms | 24.46ms | 1.136× | 1.67% | PASS |
| ac_reads | `oltp_order_range` | 4.26ms | 4.48ms | 1.052× | 2.43% | PASS |
| ac_reads | `oltp_distinct_range` | 5.33ms | 5.57ms | 1.044× | 2.20% | PASS |
| ac_reads | `oltp_index_scan` | 7.64ms | 9.37ms | 1.227× | 2.53% | PASS |
| ac_reads | `select_random_points` | 33.33ms | 37.21ms | 1.117× | 2.26% | PASS |
| ac_reads | `select_random_ranges` | 10.86ms | 12.31ms | 1.133× | 1.86% | PASS |
| ac_reads | `covering_index_scan` | 7.38ms | 7.33ms | 0.992× | 2.03% | PASS |
| ac_reads | `groupby_scan` | 37.26ms | 40.40ms | 1.084× | 1.33% | PASS |
| ac_reads | `index_join` | 10.29ms | 13.20ms | 1.283× | 1.99% | PASS |
| ac_reads | `index_join_scan` | 4.78ms | 6.24ms | 1.305× | 1.90% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.24s | 1.168× | 2.32% | PASS |
| ac_reads | `table_scan` | 1.29s | 1.40s | 1.089× | 5.26% | PASS |
| ac_reads | `oltp_read_only` | 190.57ms | 213.25ms | 1.119× | 1.54% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.66ms | 81.29ms | 3.436× | 5.54% | PASS |
| ac_writes | `oltp_insert_ac` | 26.80ms | 103.52ms | 3.863× | 5.44% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.66ms | 116.41ms | 4.209× | 4.13% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.18ms | 92.44ms | 3.824× | 5.82% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.00ms | 107.36ms | 4.129× | 4.32% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.44ms | 104.83ms | 3.965× | 5.23% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.02ms | 94.64ms | 4.111× | 5.55% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.56ms | 112.84ms | 3.465× | 4.30% | PASS |

</details>

## Version-control latency

Wall time: 2m 31s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 93.85ms | 200.00ms | 46.9% | 0.63% | PASS |
| `status_dirty_many_tables` | 97.17ms | 200.00ms | 48.6% | 0.52% | PASS |
| `diff_regular_working_one_table` | 89.80ms | 150.00ms | 59.9% | 0.34% | PASS |
| `diff_regular_working_many_tables` | 103.66ms | 200.00ms | 51.8% | 0.27% | PASS |
| `diff_stat_working_many_tables` | 103.66ms | 200.00ms | 51.8% | 0.42% | PASS |
| `diff_schema_working_many_tables` | 103.95ms | 200.00ms | 52.0% | 0.44% | PASS |
| `branch_list_many_branches` | 25.69ms | 100.00ms | 25.7% | 1.44% | PASS |
| `branch_create_delete` | 27.37ms | 100.00ms | 27.4% | 1.05% | PASS |
| `checkout_branch_clean` | 61.01ms | 200.00ms | 30.5% | 0.71% | PASS |
| `merge_data_no_conflicts` | 32.01ms | 150.00ms | 21.3% | 1.94% | PASS |
| `merge_schema_no_conflicts` | 23.95ms | 100.00ms | 24.0% | 2.05% | PASS |
| `merge_data_conflicts` | 129.99ms | 250.00ms | 52.0% | 0.27% | PASS |
| `merge_data_conflicts_with_resolve` | 129.70ms | 250.00ms | 51.9% | 0.37% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
