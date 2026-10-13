# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-13 18:39 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261004.327.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/38435693995)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 12m 57s | 8.97s | 11.46s | 1.278× | 1.39% | **PASS** |
| textpk | 69 | 55 | 1h 31m 26s | 10.23s | 11.60s | 1.134× | 0.88% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 47s | 9.56s | 11.69s | 1.222× | 1.55% | **PASS** |
| compositepk | 69 | 55 | 1h 3m 58s | 7.38s | 8.00s | 1.085× | 2.78% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.13ms | 29.71ms | 1.231× | 1.37% | PASS |
| mem_reads | `oltp_range_select` | 10.30ms | 13.20ms | 1.282× | 1.92% | PASS |
| mem_reads | `oltp_sum_range` | 9.60ms | 12.43ms | 1.294× | 0.97% | PASS |
| mem_reads | `oltp_order_range` | 2.58ms | 3.00ms | 1.164× | 1.05% | PASS |
| mem_reads | `oltp_distinct_range` | 3.66ms | 4.05ms | 1.106× | 0.84% | PASS |
| mem_reads | `oltp_index_scan` | 3.95ms | 5.38ms | 1.363× | 1.43% | PASS |
| mem_reads | `select_random_points` | 10.16ms | 11.28ms | 1.110× | 2.50% | PASS |
| mem_reads | `select_random_ranges` | 3.04ms | 4.00ms | 1.317× | 1.20% | PASS |
| mem_reads | `covering_index_scan` | 4.26ms | 4.35ms | 1.021× | 1.95% | PASS |
| mem_reads | `groupby_scan` | 29.98ms | 33.39ms | 1.114× | 0.95% | PASS |
| mem_reads | `index_join` | 6.06ms | 8.48ms | 1.399× | 1.45% | PASS |
| mem_reads | `index_join_scan` | 3.48ms | 4.59ms | 1.319× | 2.38% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.35s | 1.263× | 1.22% | PASS |
| mem_reads | `table_scan` | 1.21s | 1.41s | 1.158× | 1.02% | PASS |
| mem_reads | `oltp_read_only` | 105.51ms | 124.66ms | 1.181× | 1.05% | PASS |
| mem_writes | `oltp_bulk_insert` | 182.39ms | 251.75ms | 1.380× | 1.24% | PASS |
| mem_writes | `oltp_insert` | 15.66ms | 28.43ms | 1.815× | 0.98% | PASS |
| mem_writes | `oltp_update_index` | 51.36ms | 107.35ms | 2.090× | 1.36% | PASS |
| mem_writes | `oltp_update_non_index` | 34.43ms | 60.00ms | 1.743× | 1.32% | PASS |
| mem_writes | `oltp_delete_insert` | 45.53ms | 79.37ms | 1.743× | 1.20% | PASS |
| mem_writes | `oltp_write_only` | 21.92ms | 44.77ms | 2.042× | 1.45% | PASS |
| mem_writes | `types_delete_insert` | 24.48ms | 40.33ms | 1.647× | 1.39% | PASS |
| mem_writes | `oltp_read_write` | 67.32ms | 110.78ms | 1.646× | 1.01% | PASS |
| file_reads | `oltp_point_select` | 99.21ms | 55.04ms | 0.555× | 0.81% | PASS |
| file_reads | `oltp_range_select` | 18.07ms | 15.87ms | 0.878× | 1.34% | PASS |
| file_reads | `oltp_sum_range` | 17.62ms | 15.23ms | 0.865× | 1.43% | PASS |
| file_reads | `oltp_order_range` | 3.43ms | 3.35ms | 0.978× | 2.06% | PASS |
| file_reads | `oltp_distinct_range` | 4.59ms | 4.39ms | 0.957× | 1.36% | PASS |
| file_reads | `oltp_index_scan` | 11.70ms | 8.33ms | 0.712× | 1.17% | PASS |
| file_reads | `select_random_points` | 18.43ms | 14.05ms | 0.762× | 1.63% | PASS |
| file_reads | `select_random_ranges` | 10.67ms | 6.62ms | 0.621× | 0.82% | PASS |
| file_reads | `covering_index_scan` | 11.92ms | 7.12ms | 0.597× | 1.08% | PASS |
| file_reads | `groupby_scan` | 30.95ms | 33.76ms | 1.091× | 0.93% | PASS |
| file_reads | `index_join` | 10.38ms | 10.32ms | 0.994× | 1.65% | PASS |
| file_reads | `index_join_scan` | 4.51ms | 4.93ms | 1.094× | 2.55% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.33s | 1.277× | 0.63% | PASS |
| file_reads | `table_scan` | 1.20s | 1.40s | 1.165× | 0.93% | PASS |
| file_reads | `oltp_read_only` | 216.71ms | 164.38ms | 0.759× | 0.89% | PASS |
| file_writes | `oltp_bulk_insert` | 197.06ms | 272.81ms | 1.384× | 1.10% | PASS |
| file_writes | `oltp_insert` | 22.30ms | 35.67ms | 1.600× | 1.85% | PASS |
| file_writes | `oltp_update_index` | 77.37ms | 129.14ms | 1.669× | 1.10% | PASS |
| file_writes | `oltp_update_non_index` | 58.33ms | 81.18ms | 1.392× | 2.03% | PASS |
| file_writes | `oltp_delete_insert` | 68.14ms | 99.11ms | 1.455× | 1.40% | PASS |
| file_writes | `oltp_write_only` | 44.98ms | 64.43ms | 1.433× | 1.47% | PASS |
| file_writes | `types_delete_insert` | 40.27ms | 53.66ms | 1.333× | 1.54% | PASS |
| file_writes | `oltp_read_write` | 95.10ms | 133.01ms | 1.399× | 1.66% | PASS |
| ac_reads | `oltp_point_select` | 49.28ms | 55.20ms | 1.120× | 1.23% | PASS |
| ac_reads | `oltp_range_select` | 13.07ms | 15.79ms | 1.208× | 1.54% | PASS |
| ac_reads | `oltp_sum_range` | 12.48ms | 15.22ms | 1.219× | 1.54% | PASS |
| ac_reads | `oltp_order_range` | 2.95ms | 3.31ms | 1.122× | 1.63% | PASS |
| ac_reads | `oltp_distinct_range` | 4.04ms | 4.46ms | 1.102× | 0.96% | PASS |
| ac_reads | `oltp_index_scan` | 6.78ms | 8.34ms | 1.230× | 1.56% | PASS |
| ac_reads | `select_random_points` | 13.23ms | 14.02ms | 1.059× | 2.24% | PASS |
| ac_reads | `select_random_ranges` | 5.61ms | 6.62ms | 1.179× | 1.12% | PASS |
| ac_reads | `covering_index_scan` | 6.98ms | 7.10ms | 1.017× | 1.61% | PASS |
| ac_reads | `groupby_scan` | 30.33ms | 33.75ms | 1.113× | 0.59% | PASS |
| ac_reads | `index_join` | 7.80ms | 10.36ms | 1.328× | 1.48% | PASS |
| ac_reads | `index_join_scan` | 3.96ms | 5.03ms | 1.271× | 2.18% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.33s | 1.283× | 1.04% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.40s | 1.187× | 0.87% | PASS |
| ac_reads | `oltp_read_only` | 140.44ms | 162.95ms | 1.160× | 0.89% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.24ms | 69.74ms | 3.136× | 7.83% | PASS |
| ac_writes | `oltp_insert_ac` | 24.98ms | 86.40ms | 3.460× | 6.38% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.75ms | 100.33ms | 3.750× | 6.81% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.11ms | 78.03ms | 3.376× | 5.35% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.93ms | 90.68ms | 3.789× | 4.48% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.45ms | 91.21ms | 3.449× | 5.13% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.66ms | 81.39ms | 3.758× | 6.35% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.19ms | 98.77ms | 3.167× | 6.47% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 29.87ms | 34.82ms | 1.166× | 1.16% | PASS |
| mem_reads | `oltp_range_select` | 13.79ms | 13.73ms | 0.996× | 1.68% | PASS |
| mem_reads | `oltp_sum_range` | 12.76ms | 13.34ms | 1.045× | 2.33% | PASS |
| mem_reads | `oltp_order_range` | 3.17ms | 3.17ms | 1.000× | 0.87% | PASS |
| mem_reads | `oltp_distinct_range` | 4.21ms | 4.17ms | 0.989× | 0.92% | PASS |
| mem_reads | `oltp_index_scan` | 4.56ms | 5.76ms | 1.265× | 1.62% | PASS |
| mem_reads | `select_random_points` | 17.48ms | 19.69ms | 1.126× | 0.81% | PASS |
| mem_reads | `select_random_ranges` | 4.01ms | 5.14ms | 1.284× | 0.89% | PASS |
| mem_reads | `covering_index_scan` | 4.80ms | 4.31ms | 0.899× | 1.28% | PASS |
| mem_reads | `groupby_scan` | 33.57ms | 35.29ms | 1.051× | 0.81% | PASS |
| mem_reads | `index_join` | 7.04ms | 8.69ms | 1.234× | 0.86% | PASS |
| mem_reads | `index_join_scan` | 4.75ms | 5.46ms | 1.149× | 0.76% | PASS |
| mem_reads | `types_table_scan` | 1.13s | 1.25s | 1.099× | 0.77% | PASS |
| mem_reads | `table_scan` | 1.33s | 1.35s | 1.018× | 0.36% | PASS |
| mem_reads | `oltp_read_only` | 118.59ms | 128.14ms | 1.081× | 0.49% | PASS |
| mem_writes | `oltp_bulk_insert` | 232.66ms | 334.54ms | 1.438× | 0.81% | PASS |
| mem_writes | `oltp_insert` | 22.34ms | 39.03ms | 1.747× | 0.92% | PASS |
| mem_writes | `oltp_update_index` | 72.00ms | 133.92ms | 1.860× | 0.74% | PASS |
| mem_writes | `oltp_update_non_index` | 49.74ms | 87.04ms | 1.750× | 0.65% | PASS |
| mem_writes | `oltp_delete_insert` | 52.03ms | 104.67ms | 2.012× | 0.62% | PASS |
| mem_writes | `oltp_write_only` | 29.35ms | 62.84ms | 2.141× | 0.61% | PASS |
| mem_writes | `types_delete_insert` | 32.91ms | 53.52ms | 1.626× | 0.82% | PASS |
| mem_writes | `oltp_read_write` | 82.97ms | 133.99ms | 1.615× | 0.55% | PASS |
| file_reads | `oltp_point_select` | 126.32ms | 67.25ms | 0.532× | 0.80% | PASS |
| file_reads | `oltp_range_select` | 24.04ms | 16.98ms | 0.706× | 1.37% | PASS |
| file_reads | `oltp_sum_range` | 22.64ms | 16.77ms | 0.741× | 1.26% | PASS |
| file_reads | `oltp_order_range` | 4.19ms | 3.52ms | 0.840× | 1.03% | PASS |
| file_reads | `oltp_distinct_range` | 5.25ms | 4.52ms | 0.862× | 1.81% | PASS |
| file_reads | `oltp_index_scan` | 14.62ms | 9.35ms | 0.639× | 1.01% | PASS |
| file_reads | `select_random_points` | 28.20ms | 23.61ms | 0.837× | 2.20% | PASS |
| file_reads | `select_random_ranges` | 13.86ms | 8.48ms | 0.612× | 0.78% | PASS |
| file_reads | `covering_index_scan` | 15.76ms | 7.93ms | 0.503× | 0.87% | PASS |
| file_reads | `groupby_scan` | 34.96ms | 35.69ms | 1.021× | 0.92% | PASS |
| file_reads | `index_join` | 12.97ms | 11.23ms | 0.866× | 1.37% | PASS |
| file_reads | `index_join_scan` | 5.97ms | 6.16ms | 1.032× | 0.81% | PASS |
| file_reads | `types_table_scan` | 1.14s | 1.24s | 1.087× | 0.36% | PASS |
| file_reads | `table_scan` | 1.33s | 1.35s | 1.013× | 0.29% | PASS |
| file_reads | `oltp_read_only` | 259.03ms | 174.86ms | 0.675× | 0.47% | PASS |
| file_writes | `oltp_bulk_insert` | 253.45ms | 365.25ms | 1.441× | 0.71% | PASS |
| file_writes | `oltp_insert` | 46.63ms | 52.40ms | 1.124× | 20.08% | PASS |
| file_writes | `oltp_update_index` | 115.82ms | 173.34ms | 1.497× | 0.81% | PASS |
| file_writes | `oltp_update_non_index` | 94.04ms | 115.70ms | 1.230× | 6.05% | PASS |
| file_writes | `oltp_delete_insert` | 92.61ms | 137.55ms | 1.485× | 2.02% | PASS |
| file_writes | `oltp_write_only` | 92.16ms | 88.04ms | 0.955× | 9.59% | PASS |
| file_writes | `types_delete_insert` | 56.38ms | 75.13ms | 1.333× | 1.15% | PASS |
| file_writes | `oltp_read_write` | 141.31ms | 159.46ms | 1.128× | 4.65% | PASS |
| ac_reads | `oltp_point_select` | 61.54ms | 67.00ms | 1.089× | 1.09% | PASS |
| ac_reads | `oltp_range_select` | 17.61ms | 16.98ms | 0.964× | 0.72% | PASS |
| ac_reads | `oltp_sum_range` | 16.20ms | 16.78ms | 1.036× | 0.69% | PASS |
| ac_reads | `oltp_order_range` | 3.59ms | 3.53ms | 0.983× | 1.33% | PASS |
| ac_reads | `oltp_distinct_range` | 4.58ms | 4.54ms | 0.991× | 0.82% | PASS |
| ac_reads | `oltp_index_scan` | 8.11ms | 9.39ms | 1.158× | 0.82% | PASS |
| ac_reads | `select_random_points` | 21.65ms | 23.77ms | 1.098× | 0.70% | PASS |
| ac_reads | `select_random_ranges` | 7.39ms | 8.50ms | 1.150× | 0.88% | PASS |
| ac_reads | `covering_index_scan` | 8.99ms | 7.91ms | 0.880× | 0.75% | PASS |
| ac_reads | `groupby_scan` | 34.17ms | 35.79ms | 1.047× | 0.74% | PASS |
| ac_reads | `index_join` | 9.50ms | 11.32ms | 1.192× | 1.36% | PASS |
| ac_reads | `index_join_scan` | 5.40ms | 6.21ms | 1.150× | 0.74% | PASS |
| ac_reads | `types_table_scan` | 1.14s | 1.24s | 1.090× | 0.54% | PASS |
| ac_reads | `table_scan` | 1.34s | 1.35s | 1.012× | 0.80% | PASS |
| ac_reads | `oltp_read_only` | 168.03ms | 175.48ms | 1.044× | 1.57% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.51ms | 54.53ms | 3.304× | 4.86% | PASS |
| ac_writes | `oltp_insert_ac` | 19.83ms | 71.01ms | 3.581× | 5.36% | PASS |
| ac_writes | `oltp_update_index_ac` | 21.91ms | 97.43ms | 4.447× | 6.69% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.21ms | 82.00ms | 4.058× | 9.58% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.68ms | 98.54ms | 4.344× | 5.75% | PASS |
| ac_writes | `oltp_write_only_ac` | 21.61ms | 91.49ms | 4.235× | 9.79% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.52ms | 70.68ms | 4.277× | 6.14% | PASS |
| ac_writes | `oltp_read_write_ac` | 25.47ms | 88.60ms | 3.478× | 5.35% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.09ms | 37.91ms | 1.219× | 2.19% | PASS |
| mem_reads | `oltp_range_select` | 13.47ms | 14.33ms | 1.064× | 2.78% | PASS |
| mem_reads | `oltp_sum_range` | 12.63ms | 14.38ms | 1.139× | 2.40% | PASS |
| mem_reads | `oltp_order_range` | 2.98ms | 3.20ms | 1.072× | 1.24% | PASS |
| mem_reads | `oltp_distinct_range` | 4.07ms | 4.29ms | 1.055× | 1.27% | PASS |
| mem_reads | `oltp_index_scan` | 4.69ms | 6.51ms | 1.389× | 1.67% | PASS |
| mem_reads | `select_random_points` | 18.36ms | 21.18ms | 1.153× | 2.39% | PASS |
| mem_reads | `select_random_ranges` | 4.09ms | 5.21ms | 1.273× | 1.34% | PASS |
| mem_reads | `covering_index_scan` | 4.43ms | 4.71ms | 1.063× | 2.66% | PASS |
| mem_reads | `groupby_scan` | 31.91ms | 34.41ms | 1.078× | 1.22% | PASS |
| mem_reads | `index_join` | 6.89ms | 9.44ms | 1.369× | 2.44% | PASS |
| mem_reads | `index_join_scan` | 4.11ms | 5.41ms | 1.318× | 3.55% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 1.22s | 1.184× | 0.58% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.36s | 1.162× | 0.76% | PASS |
| mem_reads | `oltp_read_only` | 115.68ms | 135.94ms | 1.175× | 1.53% | PASS |
| mem_writes | `oltp_bulk_insert` | 242.63ms | 353.83ms | 1.458× | 0.83% | PASS |
| mem_writes | `oltp_insert` | 19.86ms | 38.96ms | 1.962× | 0.75% | PASS |
| mem_writes | `oltp_update_index` | 67.11ms | 127.29ms | 1.897× | 1.37% | PASS |
| mem_writes | `oltp_update_non_index` | 47.45ms | 82.94ms | 1.748× | 1.19% | PASS |
| mem_writes | `oltp_delete_insert` | 48.04ms | 101.27ms | 2.108× | 0.89% | PASS |
| mem_writes | `oltp_write_only` | 27.32ms | 60.60ms | 2.219× | 1.15% | PASS |
| mem_writes | `types_delete_insert` | 32.41ms | 53.16ms | 1.640× | 2.24% | PASS |
| mem_writes | `oltp_read_write` | 84.55ms | 140.09ms | 1.657× | 1.55% | PASS |
| file_reads | `oltp_point_select` | 105.90ms | 63.42ms | 0.599× | 0.78% | PASS |
| file_reads | `oltp_range_select` | 20.42ms | 16.94ms | 0.830× | 2.20% | PASS |
| file_reads | `oltp_sum_range` | 20.08ms | 17.01ms | 0.847× | 2.48% | PASS |
| file_reads | `oltp_order_range` | 3.66ms | 3.54ms | 0.966× | 2.51% | PASS |
| file_reads | `oltp_distinct_range` | 4.85ms | 4.64ms | 0.957× | 1.83% | PASS |
| file_reads | `oltp_index_scan` | 12.15ms | 9.24ms | 0.761× | 1.36% | PASS |
| file_reads | `select_random_points` | 25.97ms | 24.38ms | 0.939× | 1.45% | PASS |
| file_reads | `select_random_ranges` | 11.49ms | 7.87ms | 0.685× | 1.47% | PASS |
| file_reads | `covering_index_scan` | 12.04ms | 7.36ms | 0.611× | 1.09% | PASS |
| file_reads | `groupby_scan` | 32.34ms | 34.76ms | 1.075× | 1.04% | PASS |
| file_reads | `index_join` | 11.11ms | 11.40ms | 1.027× | 1.88% | PASS |
| file_reads | `index_join_scan` | 5.31ms | 5.92ms | 1.114× | 3.46% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.22s | 1.181× | 0.70% | PASS |
| file_reads | `table_scan` | 1.29s | 1.38s | 1.068× | 4.85% | PASS |
| file_reads | `oltp_read_only` | 234.10ms | 176.96ms | 0.756× | 0.90% | PASS |
| file_writes | `oltp_bulk_insert` | 261.80ms | 382.12ms | 1.460× | 1.12% | PASS |
| file_writes | `oltp_insert` | 31.61ms | 51.99ms | 1.645× | 1.88% | PASS |
| file_writes | `oltp_update_index` | 104.76ms | 164.94ms | 1.574× | 1.55% | PASS |
| file_writes | `oltp_update_non_index` | 78.70ms | 108.28ms | 1.376× | 1.07% | PASS |
| file_writes | `oltp_delete_insert` | 80.83ms | 130.10ms | 1.610× | 1.33% | PASS |
| file_writes | `oltp_write_only` | 55.27ms | 84.21ms | 1.524× | 1.43% | PASS |
| file_writes | `types_delete_insert` | 52.06ms | 72.60ms | 1.394× | 2.21% | PASS |
| file_writes | `oltp_read_write` | 118.01ms | 164.17ms | 1.391× | 2.09% | PASS |
| ac_reads | `oltp_point_select` | 55.74ms | 63.63ms | 1.142× | 1.18% | PASS |
| ac_reads | `oltp_range_select` | 16.26ms | 17.06ms | 1.049× | 1.95% | PASS |
| ac_reads | `oltp_sum_range` | 15.28ms | 17.12ms | 1.121× | 1.42% | PASS |
| ac_reads | `oltp_order_range` | 3.29ms | 3.52ms | 1.070× | 1.67% | PASS |
| ac_reads | `oltp_distinct_range` | 4.37ms | 4.62ms | 1.057× | 1.39% | PASS |
| ac_reads | `oltp_index_scan` | 7.49ms | 9.19ms | 1.227× | 2.47% | PASS |
| ac_reads | `select_random_points` | 21.26ms | 24.26ms | 1.141× | 1.61% | PASS |
| ac_reads | `select_random_ranges` | 6.72ms | 7.87ms | 1.170× | 1.23% | PASS |
| ac_reads | `covering_index_scan` | 7.57ms | 7.34ms | 0.970× | 2.25% | PASS |
| ac_reads | `groupby_scan` | 32.14ms | 34.75ms | 1.081× | 0.84% | PASS |
| ac_reads | `index_join` | 8.79ms | 11.42ms | 1.300× | 2.55% | PASS |
| ac_reads | `index_join_scan` | 4.77ms | 5.89ms | 1.236× | 1.33% | PASS |
| ac_reads | `types_table_scan` | 1.04s | 1.23s | 1.175× | 1.31% | PASS |
| ac_reads | `table_scan` | 1.33s | 1.39s | 1.042× | 1.88% | PASS |
| ac_reads | `oltp_read_only` | 158.78ms | 177.25ms | 1.116× | 1.38% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.65ms | 68.18ms | 2.883× | 6.38% | PASS |
| ac_writes | `oltp_insert_ac` | 25.91ms | 90.69ms | 3.500× | 6.06% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.12ms | 99.58ms | 3.541× | 6.48% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.67ms | 82.74ms | 3.354× | 8.11% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 24.68ms | 91.33ms | 3.701× | 4.65% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.53ms | 93.21ms | 3.513× | 6.21% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.15ms | 82.54ms | 3.565× | 4.85% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.90ms | 100.14ms | 3.139× | 5.04% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 22.04ms | 24.93ms | 1.131× | 2.63% | PASS |
| mem_reads | `oltp_range_select` | 12.02ms | 11.96ms | 0.995× | 2.63% | PASS |
| mem_reads | `oltp_sum_range` | 10.68ms | 11.61ms | 1.087× | 2.61% | PASS |
| mem_reads | `oltp_order_range` | 2.31ms | 2.25ms | 0.971× | 1.55% | PASS |
| mem_reads | `oltp_distinct_range` | 2.85ms | 2.79ms | 0.979× | 1.48% | PASS |
| mem_reads | `oltp_index_scan` | 3.13ms | 4.17ms | 1.329× | 2.90% | PASS |
| mem_reads | `select_random_points` | 18.86ms | 21.66ms | 1.149× | 2.77% | PASS |
| mem_reads | `select_random_ranges` | 4.91ms | 5.36ms | 1.092× | 1.47% | PASS |
| mem_reads | `covering_index_scan` | 2.51ms | 2.96ms | 1.178× | 5.27% | PASS |
| mem_reads | `groupby_scan` | 21.16ms | 22.61ms | 1.068× | 1.57% | PASS |
| mem_reads | `index_join` | 5.10ms | 6.26ms | 1.228× | 1.35% | PASS |
| mem_reads | `index_join_scan` | 2.41ms | 4.31ms | 1.792× | 6.34% | PASS |
| mem_reads | `types_table_scan` | 733.42ms | 788.42ms | 1.075× | 1.41% | PASS |
| mem_reads | `table_scan` | 917.15ms | 883.33ms | 0.963× | 4.62% | PASS |
| mem_reads | `oltp_read_only` | 90.84ms | 92.69ms | 1.020× | 2.77% | PASS |
| mem_writes | `oltp_bulk_insert` | 147.16ms | 205.69ms | 1.398× | 1.51% | PASS |
| mem_writes | `oltp_insert` | 11.48ms | 22.42ms | 1.953× | 2.15% | PASS |
| mem_writes | `oltp_update_index` | 45.63ms | 83.94ms | 1.840× | 3.20% | PASS |
| mem_writes | `oltp_update_non_index` | 36.22ms | 61.53ms | 1.699× | 2.06% | PASS |
| mem_writes | `oltp_delete_insert` | 33.07ms | 66.97ms | 2.025× | 2.18% | PASS |
| mem_writes | `oltp_write_only` | 18.04ms | 39.97ms | 2.216× | 2.79% | PASS |
| mem_writes | `types_delete_insert` | 21.41ms | 36.54ms | 1.707× | 3.54% | PASS |
| mem_writes | `oltp_read_write` | 57.91ms | 89.01ms | 1.537× | 3.24% | PASS |
| file_reads | `oltp_point_select` | 80.44ms | 44.93ms | 0.559× | 1.64% | PASS |
| file_reads | `oltp_range_select` | 19.14ms | 14.34ms | 0.749× | 2.41% | PASS |
| file_reads | `oltp_sum_range` | 17.85ms | 14.17ms | 0.794× | 1.55% | PASS |
| file_reads | `oltp_order_range` | 3.16ms | 2.60ms | 0.822× | 2.03% | PASS |
| file_reads | `oltp_distinct_range` | 3.71ms | 3.15ms | 0.847× | 1.39% | PASS |
| file_reads | `oltp_index_scan` | 9.75ms | 6.70ms | 0.687× | 1.72% | PASS |
| file_reads | `select_random_points` | 25.53ms | 23.35ms | 0.915× | 3.24% | PASS |
| file_reads | `select_random_ranges` | 10.93ms | 7.45ms | 0.682× | 1.12% | PASS |
| file_reads | `covering_index_scan` | 8.88ms | 5.15ms | 0.580× | 2.11% | PASS |
| file_reads | `groupby_scan` | 21.89ms | 22.22ms | 1.015× | 1.75% | PASS |
| file_reads | `index_join` | 8.82ms | 8.62ms | 0.977× | 4.03% | PASS |
| file_reads | `index_join_scan` | 3.71ms | 4.53ms | 1.220× | 3.31% | PASS |
| file_reads | `types_table_scan` | 720.83ms | 774.22ms | 1.074× | 1.21% | PASS |
| file_reads | `table_scan` | 887.52ms | 866.08ms | 0.976× | 4.34% | PASS |
| file_reads | `oltp_read_only` | 178.11ms | 122.29ms | 0.687× | 2.78% | PASS |
| file_writes | `oltp_bulk_insert` | 200.01ms | 277.41ms | 1.387× | 3.54% | PASS |
| file_writes | `oltp_insert` | 19.83ms | 39.48ms | 1.991× | 4.39% | PASS |
| file_writes | `oltp_update_index` | 151.09ms | 159.63ms | 1.057× | 5.25% | PASS |
| file_writes | `oltp_update_non_index` | 130.35ms | 119.19ms | 0.914× | 4.14% | PASS |
| file_writes | `oltp_delete_insert` | 121.22ms | 129.01ms | 1.064× | 3.86% | PASS |
| file_writes | `oltp_write_only` | 90.45ms | 98.22ms | 1.086× | 4.33% | PASS |
| file_writes | `types_delete_insert` | 80.10ms | 70.34ms | 0.878× | 4.80% | PASS |
| file_writes | `oltp_read_write` | 135.47ms | 149.51ms | 1.104× | 3.68% | PASS |
| ac_reads | `oltp_point_select` | 43.53ms | 46.93ms | 1.078× | 2.34% | PASS |
| ac_reads | `oltp_range_select` | 14.71ms | 14.16ms | 0.963× | 1.58% | PASS |
| ac_reads | `oltp_sum_range` | 13.25ms | 13.81ms | 1.042× | 2.84% | PASS |
| ac_reads | `oltp_order_range` | 2.54ms | 2.47ms | 0.969× | 2.10% | PASS |
| ac_reads | `oltp_distinct_range` | 3.14ms | 3.01ms | 0.956× | 1.29% | PASS |
| ac_reads | `oltp_index_scan` | 5.31ms | 6.34ms | 1.195× | 4.19% | PASS |
| ac_reads | `select_random_points` | 21.27ms | 23.20ms | 1.091× | 1.97% | PASS |
| ac_reads | `select_random_ranges` | 7.15ms | 7.53ms | 1.054× | 1.48% | PASS |
| ac_reads | `covering_index_scan` | 4.84ms | 5.12ms | 1.058× | 3.12% | PASS |
| ac_reads | `groupby_scan` | 22.00ms | 22.45ms | 1.020× | 1.37% | PASS |
| ac_reads | `index_join` | 6.85ms | 8.42ms | 1.230× | 5.05% | PASS |
| ac_reads | `index_join_scan` | 3.46ms | 4.52ms | 1.306× | 3.07% | PASS |
| ac_reads | `types_table_scan` | 747.07ms | 785.55ms | 1.052× | 2.57% | PASS |
| ac_reads | `table_scan` | 1.02s | 902.93ms | 0.889× | 4.76% | PASS |
| ac_reads | `oltp_read_only` | 124.99ms | 124.64ms | 0.997× | 2.02% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.79ms | 58.24ms | 2.673× | 4.47% | PASS |
| ac_writes | `oltp_insert_ac` | 24.40ms | 74.45ms | 3.051× | 5.33% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.47ms | 81.69ms | 3.087× | 4.40% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.46ms | 66.58ms | 3.103× | 4.22% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.24ms | 77.11ms | 3.317× | 6.21% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.65ms | 75.98ms | 3.212× | 4.66% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.71ms | 66.53ms | 2.929× | 8.85% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.00ms | 78.51ms | 2.907× | 3.35% | PASS |

</details>

## Version-control latency

Wall time: 2m 21s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.65ms | 200.00ms | 41.8% | 0.50% | PASS |
| `status_dirty_many_tables` | 86.37ms | 200.00ms | 43.2% | 0.43% | PASS |
| `diff_regular_working_one_table` | 78.97ms | 150.00ms | 52.6% | 0.42% | PASS |
| `diff_regular_working_many_tables` | 92.10ms | 200.00ms | 46.1% | 0.27% | PASS |
| `diff_stat_working_many_tables` | 92.45ms | 200.00ms | 46.2% | 0.46% | PASS |
| `diff_schema_working_many_tables` | 92.81ms | 200.00ms | 46.4% | 0.42% | PASS |
| `branch_list_many_branches` | 23.22ms | 100.00ms | 23.2% | 1.46% | PASS |
| `branch_create_delete` | 24.98ms | 100.00ms | 25.0% | 1.24% | PASS |
| `checkout_branch_clean` | 55.58ms | 200.00ms | 27.8% | 0.92% | PASS |
| `merge_data_no_conflicts` | 28.91ms | 150.00ms | 19.3% | 0.98% | PASS |
| `merge_schema_no_conflicts` | 22.18ms | 100.00ms | 22.2% | 1.25% | PASS |
| `merge_data_conflicts` | 127.21ms | 250.00ms | 50.9% | 0.21% | PASS |
| `merge_data_conflicts_with_resolve` | 127.38ms | 250.00ms | 51.0% | 0.30% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
