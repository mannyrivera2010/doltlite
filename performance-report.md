# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-10-16 15:47 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu24 20261012.334.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/38913311731)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 35s | 9.19s | 11.55s | 1.257× | 1.84% | **PASS** |
| textpk | 69 | 55 | 1h 36m 21s | 10.95s | 10.81s | 0.987× | 1.95% | **PASS** |
| blobpk | 69 | 55 | 1h 30m 24s | 9.39s | 11.64s | 1.239× | 1.78% | **PASS** |
| compositepk | 69 | 55 | 1h 26m 41s | 9.49s | 11.89s | 1.253× | 1.24% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 24.38ms | 29.73ms | 1.219× | 1.62% | PASS |
| mem_reads | `oltp_range_select` | 10.42ms | 13.28ms | 1.274× | 1.86% | PASS |
| mem_reads | `oltp_sum_range` | 9.71ms | 12.45ms | 1.282× | 1.57% | PASS |
| mem_reads | `oltp_order_range` | 2.58ms | 3.00ms | 1.161× | 1.76% | PASS |
| mem_reads | `oltp_distinct_range` | 3.66ms | 4.06ms | 1.108× | 1.02% | PASS |
| mem_reads | `oltp_index_scan` | 4.00ms | 5.48ms | 1.369× | 1.85% | PASS |
| mem_reads | `select_random_points` | 10.41ms | 11.31ms | 1.086× | 1.95% | PASS |
| mem_reads | `select_random_ranges` | 3.08ms | 4.03ms | 1.309× | 1.78% | PASS |
| mem_reads | `covering_index_scan` | 4.25ms | 4.29ms | 1.010× | 2.99% | PASS |
| mem_reads | `groupby_scan` | 30.06ms | 33.35ms | 1.109× | 1.06% | PASS |
| mem_reads | `index_join` | 6.07ms | 8.24ms | 1.357× | 2.60% | PASS |
| mem_reads | `index_join_scan` | 3.52ms | 4.64ms | 1.316× | 2.40% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 1.34s | 1.258× | 1.25% | PASS |
| mem_reads | `table_scan` | 1.23s | 1.42s | 1.149× | 1.11% | PASS |
| mem_reads | `oltp_read_only` | 105.64ms | 125.25ms | 1.186× | 1.97% | PASS |
| mem_writes | `oltp_bulk_insert` | 181.45ms | 253.44ms | 1.397× | 1.14% | PASS |
| mem_writes | `oltp_insert` | 15.53ms | 28.60ms | 1.841× | 0.95% | PASS |
| mem_writes | `oltp_update_index` | 51.40ms | 107.52ms | 2.092× | 2.11% | PASS |
| mem_writes | `oltp_update_non_index` | 35.02ms | 60.72ms | 1.734× | 2.06% | PASS |
| mem_writes | `oltp_delete_insert` | 46.42ms | 80.98ms | 1.745× | 2.39% | PASS |
| mem_writes | `oltp_write_only` | 22.77ms | 46.29ms | 2.033× | 1.73% | PASS |
| mem_writes | `types_delete_insert` | 25.27ms | 40.95ms | 1.621× | 1.30% | PASS |
| mem_writes | `oltp_read_write` | 71.50ms | 114.03ms | 1.595× | 2.52% | PASS |
| file_reads | `oltp_point_select` | 100.32ms | 55.97ms | 0.558× | 0.88% | PASS |
| file_reads | `oltp_range_select` | 18.18ms | 15.96ms | 0.878× | 2.51% | PASS |
| file_reads | `oltp_sum_range` | 17.61ms | 15.22ms | 0.864× | 1.81% | PASS |
| file_reads | `oltp_order_range` | 3.49ms | 3.40ms | 0.975× | 2.88% | PASS |
| file_reads | `oltp_distinct_range` | 4.59ms | 4.48ms | 0.976× | 1.31% | PASS |
| file_reads | `oltp_index_scan` | 11.93ms | 8.39ms | 0.704× | 1.67% | PASS |
| file_reads | `select_random_points` | 18.62ms | 14.22ms | 0.763× | 2.02% | PASS |
| file_reads | `select_random_ranges` | 10.63ms | 6.67ms | 0.628× | 1.59% | PASS |
| file_reads | `covering_index_scan` | 12.14ms | 7.16ms | 0.590× | 1.27% | PASS |
| file_reads | `groupby_scan` | 30.94ms | 34.01ms | 1.099× | 1.29% | PASS |
| file_reads | `index_join` | 10.55ms | 10.37ms | 0.983× | 1.81% | PASS |
| file_reads | `index_join_scan` | 4.57ms | 5.00ms | 1.095× | 2.44% | PASS |
| file_reads | `types_table_scan` | 1.06s | 1.34s | 1.258× | 1.22% | PASS |
| file_reads | `table_scan` | 1.25s | 1.41s | 1.134× | 1.23% | PASS |
| file_reads | `oltp_read_only` | 221.16ms | 165.49ms | 0.748× | 1.34% | PASS |
| file_writes | `oltp_bulk_insert` | 196.41ms | 274.64ms | 1.398× | 1.09% | PASS |
| file_writes | `oltp_insert` | 22.93ms | 36.11ms | 1.575× | 3.08% | PASS |
| file_writes | `oltp_update_index` | 81.73ms | 134.68ms | 1.648× | 3.31% | PASS |
| file_writes | `oltp_update_non_index` | 61.23ms | 84.11ms | 1.374× | 1.84% | PASS |
| file_writes | `oltp_delete_insert` | 70.67ms | 102.80ms | 1.455× | 2.15% | PASS |
| file_writes | `oltp_write_only` | 46.37ms | 65.36ms | 1.410× | 2.47% | PASS |
| file_writes | `types_delete_insert` | 41.05ms | 54.31ms | 1.323× | 2.52% | PASS |
| file_writes | `oltp_read_write` | 96.74ms | 133.25ms | 1.377× | 2.72% | PASS |
| ac_reads | `oltp_point_select` | 50.05ms | 55.99ms | 1.119× | 1.23% | PASS |
| ac_reads | `oltp_range_select` | 13.50ms | 16.10ms | 1.192× | 2.17% | PASS |
| ac_reads | `oltp_sum_range` | 12.87ms | 15.43ms | 1.199× | 2.04% | PASS |
| ac_reads | `oltp_order_range` | 3.06ms | 3.42ms | 1.116× | 2.57% | PASS |
| ac_reads | `oltp_distinct_range` | 4.08ms | 4.45ms | 1.093× | 1.49% | PASS |
| ac_reads | `oltp_index_scan` | 6.92ms | 8.38ms | 1.210× | 1.55% | PASS |
| ac_reads | `select_random_points` | 13.66ms | 14.27ms | 1.044× | 1.54% | PASS |
| ac_reads | `select_random_ranges` | 5.75ms | 6.68ms | 1.163× | 1.17% | PASS |
| ac_reads | `covering_index_scan` | 7.23ms | 7.18ms | 0.993× | 1.43% | PASS |
| ac_reads | `groupby_scan` | 30.46ms | 34.16ms | 1.121× | 1.33% | PASS |
| ac_reads | `index_join` | 8.00ms | 10.40ms | 1.300× | 2.32% | PASS |
| ac_reads | `index_join_scan` | 4.03ms | 5.12ms | 1.271× | 2.33% | PASS |
| ac_reads | `types_table_scan` | 1.06s | 1.34s | 1.260× | 1.18% | PASS |
| ac_reads | `table_scan` | 1.26s | 1.42s | 1.124× | 1.15% | PASS |
| ac_reads | `oltp_read_only` | 144.42ms | 165.51ms | 1.146× | 1.26% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.75ms | 70.96ms | 3.120× | 5.47% | PASS |
| ac_writes | `oltp_insert_ac` | 25.35ms | 87.31ms | 3.444× | 6.07% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.66ms | 104.90ms | 3.660× | 5.82% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 25.25ms | 83.30ms | 3.299× | 8.84% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.75ms | 94.88ms | 3.684× | 6.72% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.54ms | 93.01ms | 3.642× | 5.37% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.26ms | 82.67ms | 3.554× | 8.68% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.22ms | 100.20ms | 3.209× | 7.00% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 27.14ms | 30.06ms | 1.108× | 2.35% | PASS |
| mem_reads | `oltp_range_select` | 13.81ms | 11.51ms | 0.833× | 2.98% | PASS |
| mem_reads | `oltp_sum_range` | 11.68ms | 11.29ms | 0.967× | 2.62% | PASS |
| mem_reads | `oltp_order_range` | 2.71ms | 2.58ms | 0.950× | 1.84% | PASS |
| mem_reads | `oltp_distinct_range` | 3.48ms | 3.35ms | 0.962× | 0.91% | PASS |
| mem_reads | `oltp_index_scan` | 4.03ms | 5.14ms | 1.277× | 3.66% | PASS |
| mem_reads | `select_random_points` | 16.25ms | 17.32ms | 1.066× | 2.98% | PASS |
| mem_reads | `select_random_ranges` | 3.61ms | 4.35ms | 1.206× | 1.32% | PASS |
| mem_reads | `covering_index_scan` | 4.99ms | 4.06ms | 0.815× | 4.42% | PASS |
| mem_reads | `groupby_scan` | 28.16ms | 28.25ms | 1.003× | 0.82% | PASS |
| mem_reads | `index_join` | 6.61ms | 8.24ms | 1.247× | 5.50% | PASS |
| mem_reads | `index_join_scan` | 4.41ms | 4.98ms | 1.130× | 5.04% | PASS |
| mem_reads | `types_table_scan` | 1.09s | 1.05s | 0.959× | 3.00% | PASS |
| mem_reads | `table_scan` | 1.44s | 1.16s | 0.804× | 1.05% | PASS |
| mem_reads | `oltp_read_only` | 109.50ms | 107.65ms | 0.983× | 2.56% | PASS |
| mem_writes | `oltp_bulk_insert` | 182.03ms | 268.09ms | 1.473× | 1.06% | PASS |
| mem_writes | `oltp_insert` | 19.54ms | 31.16ms | 1.595× | 2.44% | PASS |
| mem_writes | `oltp_update_index` | 63.44ms | 112.21ms | 1.769× | 2.07% | PASS |
| mem_writes | `oltp_update_non_index` | 42.39ms | 70.60ms | 1.665× | 1.21% | PASS |
| mem_writes | `oltp_delete_insert` | 45.58ms | 85.40ms | 1.874× | 2.17% | PASS |
| mem_writes | `oltp_write_only` | 26.16ms | 50.82ms | 1.943× | 2.14% | PASS |
| mem_writes | `types_delete_insert` | 28.11ms | 42.93ms | 1.527× | 2.29% | PASS |
| mem_writes | `oltp_read_write` | 74.86ms | 108.90ms | 1.455× | 1.95% | PASS |
| file_reads | `oltp_point_select` | 100.37ms | 53.84ms | 0.536× | 0.89% | PASS |
| file_reads | `oltp_range_select` | 20.86ms | 13.79ms | 0.661× | 2.08% | PASS |
| file_reads | `oltp_sum_range` | 19.77ms | 13.87ms | 0.701× | 1.85% | PASS |
| file_reads | `oltp_order_range` | 3.60ms | 2.87ms | 0.799× | 1.02% | PASS |
| file_reads | `oltp_distinct_range` | 4.40ms | 3.64ms | 0.829× | 1.09% | PASS |
| file_reads | `oltp_index_scan` | 12.04ms | 7.86ms | 0.653× | 0.88% | PASS |
| file_reads | `select_random_points` | 24.70ms | 20.34ms | 0.823× | 1.44% | PASS |
| file_reads | `select_random_ranges` | 11.42ms | 6.92ms | 0.605× | 0.82% | PASS |
| file_reads | `covering_index_scan` | 13.36ms | 6.59ms | 0.493× | 1.09% | PASS |
| file_reads | `groupby_scan` | 29.17ms | 28.61ms | 0.981× | 0.93% | PASS |
| file_reads | `index_join` | 11.19ms | 9.78ms | 0.874× | 1.71% | PASS |
| file_reads | `index_join_scan` | 5.14ms | 5.27ms | 1.025× | 1.80% | PASS |
| file_reads | `types_table_scan` | 1.08s | 1.03s | 0.952× | 1.79% | PASS |
| file_reads | `table_scan` | 1.42s | 1.14s | 0.799× | 1.46% | PASS |
| file_reads | `oltp_read_only` | 221.92ms | 144.66ms | 0.652× | 1.18% | PASS |
| file_writes | `oltp_bulk_insert` | 263.94ms | 374.45ms | 1.419× | 8.33% | PASS |
| file_writes | `oltp_insert` | 80.39ms | 64.94ms | 0.808× | 23.20% | PASS |
| file_writes | `oltp_update_index` | 251.98ms | 223.06ms | 0.885× | 11.86% | PASS |
| file_writes | `oltp_update_non_index` | 203.29ms | 151.28ms | 0.744× | 20.15% | PASS |
| file_writes | `oltp_delete_insert` | 205.75ms | 197.72ms | 0.961× | 14.43% | PASS |
| file_writes | `oltp_write_only` | 172.00ms | 127.44ms | 0.741× | 23.34% | PASS |
| file_writes | `types_delete_insert` | 122.65ms | 99.16ms | 0.808× | 21.17% | PASS |
| file_writes | `oltp_read_write` | 213.59ms | 182.56ms | 0.855× | 23.00% | PASS |
| ac_reads | `oltp_point_select` | 50.91ms | 54.66ms | 1.074× | 1.25% | PASS |
| ac_reads | `oltp_range_select` | 15.98ms | 13.93ms | 0.872× | 1.84% | PASS |
| ac_reads | `oltp_sum_range` | 14.53ms | 13.77ms | 0.947× | 2.43% | PASS |
| ac_reads | `oltp_order_range` | 3.13ms | 2.86ms | 0.914× | 0.88% | PASS |
| ac_reads | `oltp_distinct_range` | 3.88ms | 3.63ms | 0.935× | 1.03% | PASS |
| ac_reads | `oltp_index_scan` | 7.00ms | 7.80ms | 1.114× | 1.14% | PASS |
| ac_reads | `select_random_points` | 19.06ms | 19.81ms | 1.040× | 1.12% | PASS |
| ac_reads | `select_random_ranges` | 6.31ms | 6.85ms | 1.086× | 0.86% | PASS |
| ac_reads | `covering_index_scan` | 8.03ms | 6.54ms | 0.814× | 1.33% | PASS |
| ac_reads | `groupby_scan` | 28.29ms | 28.45ms | 1.006× | 0.96% | PASS |
| ac_reads | `index_join` | 8.57ms | 9.64ms | 1.125× | 1.75% | PASS |
| ac_reads | `index_join_scan` | 4.84ms | 5.39ms | 1.112× | 2.51% | PASS |
| ac_reads | `types_table_scan` | 1.14s | 1.05s | 0.916× | 1.42% | PASS |
| ac_reads | `table_scan` | 1.44s | 1.14s | 0.793× | 1.69% | PASS |
| ac_reads | `oltp_read_only` | 143.37ms | 142.41ms | 0.993× | 1.19% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.43ms | 80.36ms | 3.749× | 51.15% | PASS |
| ac_writes | `oltp_insert_ac` | 24.16ms | 109.43ms | 4.530× | 44.30% | PASS |
| ac_writes | `oltp_update_index_ac` | 27.58ms | 111.27ms | 4.034× | 56.45% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.03ms | 95.19ms | 4.526× | 42.33% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 35.34ms | 179.74ms | 5.086× | 77.37% | PASS |
| ac_writes | `oltp_write_only_ac` | 44.36ms | 137.61ms | 3.102× | 67.20% | PASS |
| ac_writes | `types_delete_insert_ac` | 69.59ms | 267.88ms | 3.850× | 71.78% | PASS |
| ac_writes | `oltp_read_write_ac` | 70.37ms | 214.20ms | 3.044× | 76.25% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.02ms | 36.81ms | 1.226× | 1.67% | PASS |
| mem_reads | `oltp_range_select` | 13.24ms | 13.91ms | 1.051× | 2.32% | PASS |
| mem_reads | `oltp_sum_range` | 12.22ms | 13.88ms | 1.136× | 2.60% | PASS |
| mem_reads | `oltp_order_range` | 2.92ms | 3.13ms | 1.072× | 1.37% | PASS |
| mem_reads | `oltp_distinct_range` | 3.98ms | 4.20ms | 1.053× | 1.26% | PASS |
| mem_reads | `oltp_index_scan` | 4.56ms | 6.20ms | 1.360× | 1.57% | PASS |
| mem_reads | `select_random_points` | 18.35ms | 20.59ms | 1.122× | 1.86% | PASS |
| mem_reads | `select_random_ranges` | 4.13ms | 5.18ms | 1.254× | 2.80% | PASS |
| mem_reads | `covering_index_scan` | 4.40ms | 4.48ms | 1.018× | 2.21% | PASS |
| mem_reads | `groupby_scan` | 31.95ms | 34.22ms | 1.071× | 0.87% | PASS |
| mem_reads | `index_join` | 6.80ms | 9.18ms | 1.351× | 1.92% | PASS |
| mem_reads | `index_join_scan` | 4.29ms | 5.30ms | 1.234× | 3.50% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.23s | 1.181× | 0.89% | PASS |
| mem_reads | `table_scan` | 1.18s | 1.36s | 1.157× | 0.75% | PASS |
| mem_reads | `oltp_read_only` | 119.39ms | 134.80ms | 1.129× | 1.69% | PASS |
| mem_writes | `oltp_bulk_insert` | 242.21ms | 353.74ms | 1.460× | 0.81% | PASS |
| mem_writes | `oltp_insert` | 20.08ms | 39.15ms | 1.950× | 0.91% | PASS |
| mem_writes | `oltp_update_index` | 69.25ms | 129.88ms | 1.876× | 1.72% | PASS |
| mem_writes | `oltp_update_non_index` | 49.78ms | 84.15ms | 1.691× | 1.40% | PASS |
| mem_writes | `oltp_delete_insert` | 49.18ms | 101.69ms | 2.068× | 1.05% | PASS |
| mem_writes | `oltp_write_only` | 28.24ms | 61.31ms | 2.171× | 1.30% | PASS |
| mem_writes | `types_delete_insert` | 33.44ms | 53.77ms | 1.608× | 1.54% | PASS |
| mem_writes | `oltp_read_write` | 85.78ms | 139.15ms | 1.622× | 1.78% | PASS |
| file_reads | `oltp_point_select` | 106.16ms | 62.82ms | 0.592× | 1.20% | PASS |
| file_reads | `oltp_range_select` | 21.06ms | 16.73ms | 0.795× | 2.88% | PASS |
| file_reads | `oltp_sum_range` | 20.28ms | 16.72ms | 0.825× | 2.07% | PASS |
| file_reads | `oltp_order_range` | 3.73ms | 3.53ms | 0.945× | 2.20% | PASS |
| file_reads | `oltp_distinct_range` | 4.87ms | 4.59ms | 0.942× | 1.21% | PASS |
| file_reads | `oltp_index_scan` | 12.19ms | 9.04ms | 0.742× | 0.96% | PASS |
| file_reads | `select_random_points` | 26.53ms | 24.13ms | 0.910× | 2.10% | PASS |
| file_reads | `select_random_ranges` | 11.63ms | 7.84ms | 0.674× | 1.02% | PASS |
| file_reads | `covering_index_scan` | 12.06ms | 7.34ms | 0.609× | 1.06% | PASS |
| file_reads | `groupby_scan` | 32.52ms | 34.78ms | 1.069× | 1.10% | PASS |
| file_reads | `index_join` | 11.02ms | 11.23ms | 1.019× | 1.45% | PASS |
| file_reads | `index_join_scan` | 5.32ms | 5.82ms | 1.094× | 2.07% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.22s | 1.178× | 0.68% | PASS |
| file_reads | `table_scan` | 1.19s | 1.36s | 1.146× | 1.12% | PASS |
| file_reads | `oltp_read_only` | 239.56ms | 175.58ms | 0.733× | 0.80% | PASS |
| file_writes | `oltp_bulk_insert` | 261.85ms | 380.56ms | 1.453× | 0.86% | PASS |
| file_writes | `oltp_insert` | 32.29ms | 52.01ms | 1.611× | 1.96% | PASS |
| file_writes | `oltp_update_index` | 107.80ms | 165.08ms | 1.531× | 1.88% | PASS |
| file_writes | `oltp_update_non_index` | 79.41ms | 107.87ms | 1.358× | 1.61% | PASS |
| file_writes | `oltp_delete_insert` | 79.81ms | 128.94ms | 1.616× | 2.04% | PASS |
| file_writes | `oltp_write_only` | 54.90ms | 83.16ms | 1.515× | 1.97% | PASS |
| file_writes | `types_delete_insert` | 54.03ms | 72.78ms | 1.347× | 1.76% | PASS |
| file_writes | `oltp_read_write` | 120.69ms | 162.95ms | 1.350× | 1.83% | PASS |
| ac_reads | `oltp_point_select` | 57.07ms | 63.14ms | 1.106× | 1.27% | PASS |
| ac_reads | `oltp_range_select` | 16.77ms | 16.76ms | 0.999× | 3.44% | PASS |
| ac_reads | `oltp_sum_range` | 15.09ms | 16.74ms | 1.109× | 1.82% | PASS |
| ac_reads | `oltp_order_range` | 3.31ms | 3.55ms | 1.071× | 2.64% | PASS |
| ac_reads | `oltp_distinct_range` | 4.37ms | 4.62ms | 1.059× | 1.67% | PASS |
| ac_reads | `oltp_index_scan` | 7.07ms | 9.10ms | 1.286× | 1.55% | PASS |
| ac_reads | `select_random_points` | 21.57ms | 24.20ms | 1.122× | 2.83% | PASS |
| ac_reads | `select_random_ranges` | 6.56ms | 7.86ms | 1.198× | 1.47% | PASS |
| ac_reads | `covering_index_scan` | 7.20ms | 7.35ms | 1.020× | 3.80% | PASS |
| ac_reads | `groupby_scan` | 32.59ms | 34.88ms | 1.070× | 0.88% | PASS |
| ac_reads | `index_join` | 9.49ms | 11.36ms | 1.198× | 2.70% | PASS |
| ac_reads | `index_join_scan` | 4.96ms | 5.88ms | 1.185× | 2.54% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.22s | 1.180× | 0.55% | PASS |
| ac_reads | `table_scan` | 1.23s | 1.37s | 1.118× | 1.94% | PASS |
| ac_reads | `oltp_read_only` | 162.44ms | 175.15ms | 1.078× | 2.06% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.97ms | 67.66ms | 2.945× | 4.63% | PASS |
| ac_writes | `oltp_insert_ac` | 26.20ms | 90.65ms | 3.460× | 6.01% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.15ms | 101.90ms | 3.620× | 6.52% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.88ms | 79.47ms | 3.328× | 7.61% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.33ms | 92.06ms | 3.635× | 7.12% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.56ms | 90.77ms | 3.551× | 7.12% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.73ms | 81.06ms | 3.565× | 6.81% | PASS |
| ac_writes | `oltp_read_write_ac` | 32.24ms | 100.05ms | 3.104× | 7.59% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.60ms | 40.23ms | 1.234× | 1.54% | PASS |
| mem_reads | `oltp_range_select` | 18.67ms | 21.11ms | 1.131× | 1.42% | PASS |
| mem_reads | `oltp_sum_range` | 17.86ms | 20.94ms | 1.173× | 1.21% | PASS |
| mem_reads | `oltp_order_range` | 3.48ms | 3.83ms | 1.101× | 1.18% | PASS |
| mem_reads | `oltp_distinct_range` | 4.62ms | 4.91ms | 1.063× | 1.24% | PASS |
| mem_reads | `oltp_index_scan` | 4.50ms | 6.15ms | 1.367× | 2.40% | PASS |
| mem_reads | `select_random_points` | 27.39ms | 31.85ms | 1.163× | 1.20% | PASS |
| mem_reads | `select_random_ranges` | 7.57ms | 9.05ms | 1.196× | 1.29% | PASS |
| mem_reads | `covering_index_scan` | 4.25ms | 4.17ms | 0.981× | 1.41% | PASS |
| mem_reads | `groupby_scan` | 36.09ms | 39.20ms | 1.086× | 0.80% | PASS |
| mem_reads | `index_join` | 8.18ms | 10.37ms | 1.268× | 1.33% | PASS |
| mem_reads | `index_join_scan` | 4.10ms | 5.46ms | 1.333× | 1.74% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.23s | 1.185× | 0.53% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.38s | 1.178× | 0.67% | PASS |
| mem_reads | `oltp_read_only` | 148.23ms | 168.61ms | 1.137× | 1.31% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.71ms | 356.08ms | 1.426× | 0.88% | PASS |
| mem_writes | `oltp_insert` | 19.30ms | 36.33ms | 1.883× | 0.89% | PASS |
| mem_writes | `oltp_update_index` | 66.99ms | 115.31ms | 1.721× | 1.00% | PASS |
| mem_writes | `oltp_update_non_index` | 49.95ms | 82.18ms | 1.645× | 1.16% | PASS |
| mem_writes | `oltp_delete_insert` | 49.12ms | 94.71ms | 1.928× | 0.84% | PASS |
| mem_writes | `oltp_write_only` | 26.71ms | 56.86ms | 2.129× | 0.85% | PASS |
| mem_writes | `types_delete_insert` | 32.46ms | 54.27ms | 1.672× | 1.20% | PASS |
| mem_writes | `oltp_read_write` | 100.40ms | 154.32ms | 1.537× | 1.48% | PASS |
| file_reads | `oltp_point_select` | 107.68ms | 66.74ms | 0.620× | 1.04% | PASS |
| file_reads | `oltp_range_select` | 26.29ms | 24.49ms | 0.931× | 1.49% | PASS |
| file_reads | `oltp_sum_range` | 25.38ms | 24.13ms | 0.951× | 1.09% | PASS |
| file_reads | `oltp_order_range` | 4.29ms | 4.24ms | 0.987× | 1.24% | PASS |
| file_reads | `oltp_distinct_range` | 5.48ms | 5.33ms | 0.973× | 0.99% | PASS |
| file_reads | `oltp_index_scan` | 12.04ms | 9.14ms | 0.759× | 1.17% | PASS |
| file_reads | `select_random_points` | 36.76ms | 36.18ms | 0.984× | 1.52% | PASS |
| file_reads | `select_random_ranges` | 15.30ms | 12.14ms | 0.793× | 1.37% | PASS |
| file_reads | `covering_index_scan` | 11.73ms | 7.21ms | 0.615× | 0.78% | PASS |
| file_reads | `groupby_scan` | 37.00ms | 39.87ms | 1.078× | 0.93% | PASS |
| file_reads | `index_join` | 12.39ms | 12.87ms | 1.039× | 1.48% | PASS |
| file_reads | `index_join_scan` | 5.16ms | 5.99ms | 1.163× | 1.85% | PASS |
| file_reads | `types_table_scan` | 1.03s | 1.23s | 1.187× | 0.54% | PASS |
| file_reads | `table_scan` | 1.18s | 1.38s | 1.174× | 0.51% | PASS |
| file_reads | `oltp_read_only` | 259.16ms | 210.18ms | 0.811× | 0.89% | PASS |
| file_writes | `oltp_bulk_insert` | 265.68ms | 379.66ms | 1.429× | 0.87% | PASS |
| file_writes | `oltp_insert` | 26.14ms | 46.29ms | 1.771× | 1.79% | PASS |
| file_writes | `oltp_update_index` | 96.05ms | 142.44ms | 1.483× | 1.48% | PASS |
| file_writes | `oltp_update_non_index` | 76.23ms | 104.05ms | 1.365× | 1.47% | PASS |
| file_writes | `oltp_delete_insert` | 76.60ms | 118.91ms | 1.552× | 0.94% | PASS |
| file_writes | `oltp_write_only` | 50.26ms | 77.19ms | 1.536× | 1.89% | PASS |
| file_writes | `types_delete_insert` | 49.56ms | 68.38ms | 1.380× | 1.72% | PASS |
| file_writes | `oltp_read_write` | 126.43ms | 175.06ms | 1.385× | 1.17% | PASS |
| ac_reads | `oltp_point_select` | 57.61ms | 66.59ms | 1.156× | 0.94% | PASS |
| ac_reads | `oltp_range_select` | 21.86ms | 24.65ms | 1.128× | 0.99% | PASS |
| ac_reads | `oltp_sum_range` | 20.67ms | 24.11ms | 1.166× | 1.14% | PASS |
| ac_reads | `oltp_order_range` | 3.90ms | 4.29ms | 1.100× | 1.65% | PASS |
| ac_reads | `oltp_distinct_range` | 5.07ms | 5.36ms | 1.057× | 1.56% | PASS |
| ac_reads | `oltp_index_scan` | 7.23ms | 9.22ms | 1.276× | 1.70% | PASS |
| ac_reads | `select_random_points` | 31.28ms | 36.55ms | 1.168× | 1.65% | PASS |
| ac_reads | `select_random_ranges` | 10.51ms | 12.17ms | 1.158× | 1.15% | PASS |
| ac_reads | `covering_index_scan` | 7.07ms | 7.29ms | 1.032× | 1.81% | PASS |
| ac_reads | `groupby_scan` | 36.77ms | 40.00ms | 1.088× | 0.89% | PASS |
| ac_reads | `index_join` | 9.78ms | 12.94ms | 1.323× | 2.06% | PASS |
| ac_reads | `index_join_scan` | 4.65ms | 6.02ms | 1.295× | 1.73% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 1.22s | 1.185× | 0.48% | PASS |
| ac_reads | `table_scan` | 1.18s | 1.38s | 1.173× | 0.87% | PASS |
| ac_reads | `oltp_read_only` | 185.51ms | 210.52ms | 1.135× | 0.70% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.31ms | 70.52ms | 3.026× | 5.62% | PASS |
| ac_writes | `oltp_insert_ac` | 26.30ms | 90.97ms | 3.459× | 5.27% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.79ms | 103.89ms | 3.609× | 6.12% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.36ms | 81.60ms | 3.350× | 4.75% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.18ms | 93.40ms | 3.710× | 5.78% | PASS |
| ac_writes | `oltp_write_only_ac` | 26.83ms | 92.12ms | 3.433× | 5.27% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.30ms | 81.53ms | 3.500× | 4.70% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.43ms | 101.16ms | 3.026× | 5.90% | PASS |

</details>

## Version-control latency

Wall time: 2m 23s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 83.83ms | 200.00ms | 41.9% | 0.37% | PASS |
| `status_dirty_many_tables` | 87.29ms | 200.00ms | 43.6% | 0.42% | PASS |
| `diff_regular_working_one_table` | 79.52ms | 150.00ms | 53.0% | 0.51% | PASS |
| `diff_regular_working_many_tables` | 92.48ms | 200.00ms | 46.2% | 0.75% | PASS |
| `diff_stat_working_many_tables` | 93.91ms | 200.00ms | 47.0% | 0.73% | PASS |
| `diff_schema_working_many_tables` | 94.04ms | 200.00ms | 47.0% | 0.80% | PASS |
| `branch_list_many_branches` | 23.86ms | 100.00ms | 23.9% | 1.85% | PASS |
| `branch_create_delete` | 25.46ms | 100.00ms | 25.5% | 2.12% | PASS |
| `checkout_branch_clean` | 56.89ms | 200.00ms | 28.4% | 1.50% | PASS |
| `merge_data_no_conflicts` | 30.14ms | 150.00ms | 20.1% | 2.18% | PASS |
| `merge_schema_no_conflicts` | 22.36ms | 100.00ms | 22.4% | 1.70% | PASS |
| `merge_data_conflicts` | 128.52ms | 250.00ms | 51.4% | 0.41% | PASS |
| `merge_data_conflicts_with_resolve` | 128.22ms | 250.00ms | 51.3% | 0.42% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
