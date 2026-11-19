# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-19 19:56 UTC
>
> Commit: [`c741ef549a0c986248e405c3eee0625ff3b55613`](https://github.com/dolthub/doltlite/commit/c741ef549a0c986248e405c3eee0625ff3b55613)
>
> Runner: ubuntu26 20261111.196.1
>
> [GitHub Actions run](https://github.com/mannyrivera2010/doltlite/actions/runs/44588880794)

This report compares optimized DoltLite against stock SQLite on the same GitHub-hosted runner. Baseline and candidate execution order alternates on each repetition. Reported timings are medians; MAD is the median absolute deviation and describes run-to-run noise.

## SQL workload summary

| Key shape | Workloads | Samples/workload | Wall time | SQLite median total | DoltLite median total | Ratio | Median paired-ratio MAD | Result |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| int | 69 | 55 | 1h 13m 37s | 9.00s | 11.58s | 1.286× | 1.94% | **PASS** |
| textpk | 69 | 55 | 58m 45s | 6.75s | 7.98s | 1.182× | 1.51% | **PASS** |
| blobpk | 69 | 55 | 56m 50s | 6.70s | 7.91s | 1.181× | 1.54% | **PASS** |
| compositepk | 69 | 55 | 1h 21m 7s | 8.92s | 9.94s | 1.114× | 1.69% | **PASS** |

The absolute ceiling is 2.5× per ordinary workload and 2.0× for a section average. Durable autocommit writes use 10.0× and 5.0× ceilings respectively.

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.19ms | 29.48ms | 1.170× | 1.92% | PASS |
| mem_reads | `oltp_range_select` | 10.67ms | 12.27ms | 1.150× | 1.49% | PASS |
| mem_reads | `oltp_sum_range` | 9.94ms | 12.20ms | 1.228× | 1.92% | PASS |
| mem_reads | `oltp_order_range` | 2.69ms | 3.07ms | 1.141× | 1.38% | PASS |
| mem_reads | `oltp_distinct_range` | 3.78ms | 4.30ms | 1.139× | 1.95% | PASS |
| mem_reads | `oltp_index_scan` | 4.11ms | 5.35ms | 1.303× | 1.59% | PASS |
| mem_reads | `select_random_points` | 10.94ms | 11.40ms | 1.042× | 2.83% | PASS |
| mem_reads | `select_random_ranges` | 3.17ms | 4.02ms | 1.268× | 2.52% | PASS |
| mem_reads | `covering_index_scan` | 4.17ms | 4.29ms | 1.029× | 1.91% | PASS |
| mem_reads | `groupby_scan` | 31.44ms | 35.98ms | 1.145× | 2.58% | PASS |
| mem_reads | `index_join` | 6.09ms | 7.83ms | 1.286× | 0.96% | PASS |
| mem_reads | `index_join_scan` | 3.46ms | 4.63ms | 1.340× | 2.71% | PASS |
| mem_reads | `types_table_scan` | 1.04s | 1.31s | 1.258× | 0.84% | PASS |
| mem_reads | `table_scan` | 1.17s | 1.44s | 1.228× | 1.94% | PASS |
| mem_reads | `oltp_read_only` | 109.70ms | 125.85ms | 1.147× | 1.86% | PASS |
| mem_writes | `oltp_bulk_insert` | 180.14ms | 254.16ms | 1.411× | 1.22% | PASS |
| mem_writes | `oltp_insert` | 15.31ms | 28.29ms | 1.848× | 0.86% | PASS |
| mem_writes | `oltp_update_index` | 51.98ms | 108.87ms | 2.094× | 1.24% | PASS |
| mem_writes | `oltp_update_non_index` | 36.02ms | 63.05ms | 1.751× | 1.83% | PASS |
| mem_writes | `oltp_delete_insert` | 45.55ms | 82.59ms | 1.813× | 2.02% | PASS |
| mem_writes | `oltp_write_only` | 22.63ms | 47.69ms | 2.107× | 1.26% | PASS |
| mem_writes | `types_delete_insert` | 26.01ms | 43.23ms | 1.662× | 2.40% | PASS |
| mem_writes | `oltp_read_write` | 73.42ms | 115.89ms | 1.578× | 1.63% | PASS |
| file_reads | `oltp_point_select` | 95.64ms | 53.49ms | 0.559× | 1.06% | PASS |
| file_reads | `oltp_range_select` | 19.44ms | 15.42ms | 0.793× | 2.08% | PASS |
| file_reads | `oltp_sum_range` | 18.34ms | 15.44ms | 0.842× | 2.41% | PASS |
| file_reads | `oltp_order_range` | 4.01ms | 3.74ms | 0.932× | 3.88% | PASS |
| file_reads | `oltp_distinct_range` | 5.10ms | 4.89ms | 0.960× | 3.07% | PASS |
| file_reads | `oltp_index_scan` | 11.14ms | 8.07ms | 0.725× | 1.57% | PASS |
| file_reads | `select_random_points` | 17.67ms | 14.08ms | 0.797× | 2.75% | PASS |
| file_reads | `select_random_ranges` | 10.40ms | 6.53ms | 0.628× | 1.34% | PASS |
| file_reads | `covering_index_scan` | 11.53ms | 6.93ms | 0.601× | 1.16% | PASS |
| file_reads | `groupby_scan` | 32.98ms | 36.51ms | 1.107× | 2.74% | PASS |
| file_reads | `index_join` | 10.08ms | 9.97ms | 0.988× | 1.21% | PASS |
| file_reads | `index_join_scan` | 4.45ms | 5.11ms | 1.149× | 2.69% | PASS |
| file_reads | `types_table_scan` | 1.04s | 1.34s | 1.283× | 0.91% | PASS |
| file_reads | `table_scan` | 1.16s | 1.43s | 1.237× | 0.86% | PASS |
| file_reads | `oltp_read_only` | 212.04ms | 160.59ms | 0.757× | 1.22% | PASS |
| file_writes | `oltp_bulk_insert` | 192.15ms | 273.51ms | 1.423× | 1.20% | PASS |
| file_writes | `oltp_insert` | 22.59ms | 35.20ms | 1.558× | 1.97% | PASS |
| file_writes | `oltp_update_index` | 81.43ms | 138.97ms | 1.707× | 1.22% | PASS |
| file_writes | `oltp_update_non_index` | 61.20ms | 87.75ms | 1.434× | 1.93% | PASS |
| file_writes | `oltp_delete_insert` | 72.15ms | 109.43ms | 1.517× | 1.81% | PASS |
| file_writes | `oltp_write_only` | 49.47ms | 68.30ms | 1.381× | 1.61% | PASS |
| file_writes | `types_delete_insert` | 43.68ms | 55.11ms | 1.262× | 3.17% | PASS |
| file_writes | `oltp_read_write` | 103.59ms | 136.25ms | 1.315× | 2.26% | PASS |
| ac_reads | `oltp_point_select` | 49.40ms | 54.14ms | 1.096× | 1.54% | PASS |
| ac_reads | `oltp_range_select` | 15.14ms | 15.51ms | 1.024× | 2.34% | PASS |
| ac_reads | `oltp_sum_range` | 13.94ms | 15.58ms | 1.118× | 2.27% | PASS |
| ac_reads | `oltp_order_range` | 3.43ms | 3.58ms | 1.043× | 2.83% | PASS |
| ac_reads | `oltp_distinct_range` | 4.41ms | 4.74ms | 1.074× | 2.33% | PASS |
| ac_reads | `oltp_index_scan` | 6.90ms | 8.16ms | 1.182× | 1.65% | PASS |
| ac_reads | `select_random_points` | 15.21ms | 14.03ms | 0.923× | 3.93% | PASS |
| ac_reads | `select_random_ranges` | 5.78ms | 6.50ms | 1.126× | 1.64% | PASS |
| ac_reads | `covering_index_scan` | 7.12ms | 6.96ms | 0.978× | 2.33% | PASS |
| ac_reads | `groupby_scan` | 32.63ms | 36.63ms | 1.123× | 2.92% | PASS |
| ac_reads | `index_join` | 8.06ms | 10.16ms | 1.261× | 1.99% | PASS |
| ac_reads | `index_join_scan` | 4.00ms | 5.04ms | 1.259× | 2.56% | PASS |
| ac_reads | `types_table_scan` | 1.09s | 1.35s | 1.242× | 1.74% | PASS |
| ac_reads | `table_scan` | 1.24s | 1.46s | 1.180× | 1.69% | PASS |
| ac_reads | `oltp_read_only` | 149.97ms | 165.23ms | 1.102× | 1.47% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 20.90ms | 64.32ms | 3.078× | 5.33% | PASS |
| ac_writes | `oltp_insert_ac` | 23.62ms | 81.79ms | 3.462× | 5.48% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.44ms | 97.57ms | 3.691× | 6.16% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.58ms | 74.81ms | 3.314× | 6.89% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 22.47ms | 84.77ms | 3.773× | 5.13% | PASS |
| ac_writes | `oltp_write_only_ac` | 23.32ms | 82.54ms | 3.540× | 4.60% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.44ms | 74.86ms | 3.491× | 6.60% | PASS |
| ac_writes | `oltp_read_write_ac` | 29.78ms | 93.42ms | 3.137× | 5.24% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.71ms | 24.62ms | 1.189× | 1.09% | PASS |
| mem_reads | `oltp_range_select` | 10.90ms | 10.21ms | 0.937× | 1.93% | PASS |
| mem_reads | `oltp_sum_range` | 11.04ms | 10.08ms | 0.913× | 1.53% | PASS |
| mem_reads | `oltp_order_range` | 2.36ms | 2.37ms | 1.006× | 1.93% | PASS |
| mem_reads | `oltp_distinct_range` | 3.03ms | 3.06ms | 1.012× | 1.19% | PASS |
| mem_reads | `oltp_index_scan` | 3.43ms | 4.81ms | 1.401× | 1.58% | PASS |
| mem_reads | `select_random_points` | 14.90ms | 17.56ms | 1.178× | 1.38% | PASS |
| mem_reads | `select_random_ranges` | 2.99ms | 3.77ms | 1.262× | 0.98% | PASS |
| mem_reads | `covering_index_scan` | 3.03ms | 3.18ms | 1.049× | 1.66% | PASS |
| mem_reads | `groupby_scan` | 23.79ms | 24.94ms | 1.048× | 0.51% | PASS |
| mem_reads | `index_join` | 5.64ms | 7.21ms | 1.278× | 1.94% | PASS |
| mem_reads | `index_join_scan` | 4.08ms | 4.90ms | 1.203× | 1.88% | PASS |
| mem_reads | `types_table_scan` | 811.23ms | 891.84ms | 1.099× | 0.91% | PASS |
| mem_reads | `table_scan` | 903.56ms | 967.97ms | 1.071× | 0.78% | PASS |
| mem_reads | `oltp_read_only` | 81.17ms | 88.97ms | 1.096× | 0.80% | PASS |
| mem_writes | `oltp_bulk_insert` | 137.17ms | 211.38ms | 1.541× | 0.49% | PASS |
| mem_writes | `oltp_insert` | 14.63ms | 25.98ms | 1.776× | 0.66% | PASS |
| mem_writes | `oltp_update_index` | 51.65ms | 97.60ms | 1.890× | 1.40% | PASS |
| mem_writes | `oltp_update_non_index` | 33.68ms | 60.97ms | 1.810× | 1.49% | PASS |
| mem_writes | `oltp_delete_insert` | 35.25ms | 70.95ms | 2.013× | 0.86% | PASS |
| mem_writes | `oltp_write_only` | 20.05ms | 42.08ms | 2.098× | 1.64% | PASS |
| mem_writes | `types_delete_insert` | 22.11ms | 38.62ms | 1.747× | 1.08% | PASS |
| mem_writes | `oltp_read_write` | 59.41ms | 96.69ms | 1.628× | 1.18% | PASS |
| file_reads | `oltp_point_select` | 42.58ms | 31.88ms | 0.749× | 0.86% | PASS |
| file_reads | `oltp_range_select` | 12.61ms | 10.66ms | 0.845× | 1.43% | PASS |
| file_reads | `oltp_sum_range` | 12.55ms | 10.38ms | 0.826× | 1.45% | PASS |
| file_reads | `oltp_order_range` | 2.48ms | 2.30ms | 0.927× | 1.77% | PASS |
| file_reads | `oltp_distinct_range` | 3.08ms | 2.98ms | 0.970× | 1.17% | PASS |
| file_reads | `oltp_index_scan` | 5.53ms | 4.97ms | 0.898× | 1.85% | PASS |
| file_reads | `select_random_points` | 16.26ms | 16.97ms | 1.043× | 2.01% | PASS |
| file_reads | `select_random_ranges` | 5.13ms | 4.31ms | 0.842× | 2.21% | PASS |
| file_reads | `covering_index_scan` | 5.29ms | 3.80ms | 0.719× | 2.38% | PASS |
| file_reads | `groupby_scan` | 23.61ms | 24.84ms | 1.052× | 0.70% | PASS |
| file_reads | `index_join` | 6.87ms | 7.53ms | 1.095× | 2.08% | PASS |
| file_reads | `index_join_scan` | 4.13ms | 4.86ms | 1.178× | 5.44% | PASS |
| file_reads | `types_table_scan` | 815.58ms | 897.60ms | 1.101× | 0.83% | PASS |
| file_reads | `table_scan` | 911.89ms | 975.04ms | 1.069× | 2.17% | PASS |
| file_reads | `oltp_read_only` | 115.36ms | 100.79ms | 0.874× | 1.12% | PASS |
| file_writes | `oltp_bulk_insert` | 146.36ms | 224.98ms | 1.537× | 1.06% | PASS |
| file_writes | `oltp_insert` | 22.14ms | 32.73ms | 1.478× | 1.71% | PASS |
| file_writes | `oltp_update_index` | 69.17ms | 113.17ms | 1.636× | 1.60% | PASS |
| file_writes | `oltp_update_non_index` | 48.28ms | 73.99ms | 1.532× | 1.20% | PASS |
| file_writes | `oltp_delete_insert` | 52.70ms | 87.30ms | 1.656× | 2.00% | PASS |
| file_writes | `oltp_write_only` | 34.95ms | 56.23ms | 1.609× | 2.01% | PASS |
| file_writes | `types_delete_insert` | 32.21ms | 47.37ms | 1.471× | 1.51% | PASS |
| file_writes | `oltp_read_write` | 73.48ms | 107.00ms | 1.456× | 1.54% | PASS |
| ac_reads | `oltp_point_select` | 27.64ms | 31.77ms | 1.149× | 0.99% | PASS |
| ac_reads | `oltp_range_select` | 12.14ms | 10.94ms | 0.901× | 0.81% | PASS |
| ac_reads | `oltp_sum_range` | 12.04ms | 10.74ms | 0.892× | 1.32% | PASS |
| ac_reads | `oltp_order_range` | 2.50ms | 2.42ms | 0.967× | 1.64% | PASS |
| ac_reads | `oltp_distinct_range` | 3.11ms | 3.09ms | 0.993× | 1.19% | PASS |
| ac_reads | `oltp_index_scan` | 4.33ms | 5.27ms | 1.217× | 1.32% | PASS |
| ac_reads | `select_random_points` | 15.90ms | 18.23ms | 1.146× | 1.39% | PASS |
| ac_reads | `select_random_ranges` | 3.91ms | 4.42ms | 1.132× | 1.82% | PASS |
| ac_reads | `covering_index_scan` | 3.99ms | 3.94ms | 0.988× | 1.42% | PASS |
| ac_reads | `groupby_scan` | 23.63ms | 24.75ms | 1.048× | 0.50% | PASS |
| ac_reads | `index_join` | 6.29ms | 7.74ms | 1.229× | 2.70% | PASS |
| ac_reads | `index_join_scan` | 4.33ms | 4.87ms | 1.124× | 2.36% | PASS |
| ac_reads | `types_table_scan` | 815.90ms | 897.61ms | 1.100× | 0.84% | PASS |
| ac_reads | `table_scan` | 904.30ms | 971.53ms | 1.074× | 2.51% | PASS |
| ac_reads | `oltp_read_only` | 95.12ms | 102.34ms | 1.076× | 1.37% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 8.63ms | 29.02ms | 3.363× | 5.98% | PASS |
| ac_writes | `oltp_insert_ac` | 10.24ms | 38.87ms | 3.795× | 3.77% | PASS |
| ac_writes | `oltp_update_index_ac` | 11.13ms | 49.55ms | 4.453× | 4.20% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 8.79ms | 35.45ms | 4.035× | 5.70% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 10.02ms | 44.42ms | 4.433× | 3.75% | PASS |
| ac_writes | `oltp_write_only_ac` | 10.15ms | 43.56ms | 4.293× | 3.85% | PASS |
| ac_writes | `types_delete_insert_ac` | 8.97ms | 40.80ms | 4.547× | 8.14% | PASS |
| ac_writes | `oltp_read_write_ac` | 13.57ms | 49.53ms | 3.649× | 4.69% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 20.15ms | 23.90ms | 1.186× | 1.19% | PASS |
| mem_reads | `oltp_range_select` | 10.02ms | 9.74ms | 0.972× | 1.78% | PASS |
| mem_reads | `oltp_sum_range` | 10.22ms | 9.57ms | 0.936× | 1.61% | PASS |
| mem_reads | `oltp_order_range` | 2.23ms | 2.27ms | 1.022× | 1.56% | PASS |
| mem_reads | `oltp_distinct_range` | 2.88ms | 2.94ms | 1.023× | 1.29% | PASS |
| mem_reads | `oltp_index_scan` | 3.31ms | 4.43ms | 1.339× | 1.27% | PASS |
| mem_reads | `select_random_points` | 13.91ms | 15.99ms | 1.149× | 1.93% | PASS |
| mem_reads | `select_random_ranges` | 2.86ms | 3.54ms | 1.237× | 1.84% | PASS |
| mem_reads | `covering_index_scan` | 2.79ms | 2.90ms | 1.036× | 1.81% | PASS |
| mem_reads | `groupby_scan` | 23.09ms | 24.45ms | 1.059× | 0.51% | PASS |
| mem_reads | `index_join` | 5.29ms | 6.81ms | 1.286× | 1.16% | PASS |
| mem_reads | `index_join_scan` | 3.30ms | 4.67ms | 1.413× | 2.41% | PASS |
| mem_reads | `types_table_scan` | 794.67ms | 886.52ms | 1.116× | 1.18% | PASS |
| mem_reads | `table_scan` | 938.17ms | 970.38ms | 1.034× | 4.34% | PASS |
| mem_reads | `oltp_read_only` | 81.14ms | 89.83ms | 1.107× | 0.92% | PASS |
| mem_writes | `oltp_bulk_insert` | 135.74ms | 203.97ms | 1.503× | 0.58% | PASS |
| mem_writes | `oltp_insert` | 12.86ms | 25.10ms | 1.952× | 0.63% | PASS |
| mem_writes | `oltp_update_index` | 47.88ms | 92.92ms | 1.941× | 0.94% | PASS |
| mem_writes | `oltp_update_non_index` | 31.91ms | 57.82ms | 1.812× | 1.00% | PASS |
| mem_writes | `oltp_delete_insert` | 33.65ms | 71.41ms | 2.122× | 0.61% | PASS |
| mem_writes | `oltp_write_only` | 19.44ms | 43.64ms | 2.245× | 0.88% | PASS |
| mem_writes | `types_delete_insert` | 21.01ms | 37.64ms | 1.792× | 0.65% | PASS |
| mem_writes | `oltp_read_write` | 57.20ms | 95.14ms | 1.663× | 0.87% | PASS |
| file_reads | `oltp_point_select` | 42.83ms | 31.85ms | 0.744× | 0.83% | PASS |
| file_reads | `oltp_range_select` | 12.73ms | 10.80ms | 0.849× | 1.55% | PASS |
| file_reads | `oltp_sum_range` | 12.81ms | 10.59ms | 0.827× | 1.65% | PASS |
| file_reads | `oltp_order_range` | 2.46ms | 2.37ms | 0.965× | 2.35% | PASS |
| file_reads | `oltp_distinct_range` | 3.12ms | 3.04ms | 0.973× | 1.65% | PASS |
| file_reads | `oltp_index_scan` | 5.57ms | 5.24ms | 0.942× | 1.43% | PASS |
| file_reads | `select_random_points` | 16.84ms | 17.79ms | 1.056× | 1.17% | PASS |
| file_reads | `select_random_ranges` | 5.08ms | 4.36ms | 0.859× | 1.47% | PASS |
| file_reads | `covering_index_scan` | 5.03ms | 3.85ms | 0.765× | 2.13% | PASS |
| file_reads | `groupby_scan` | 23.15ms | 24.64ms | 1.065× | 0.44% | PASS |
| file_reads | `index_join` | 6.48ms | 7.33ms | 1.132× | 2.24% | PASS |
| file_reads | `index_join_scan` | 3.55ms | 4.86ms | 1.368× | 4.76% | PASS |
| file_reads | `types_table_scan` | 806.40ms | 892.83ms | 1.107× | 0.65% | PASS |
| file_reads | `table_scan` | 939.20ms | 972.85ms | 1.036× | 2.39% | PASS |
| file_reads | `oltp_read_only` | 112.31ms | 99.17ms | 0.883× | 1.26% | PASS |
| file_writes | `oltp_bulk_insert` | 145.21ms | 217.54ms | 1.498× | 0.55% | PASS |
| file_writes | `oltp_insert` | 17.70ms | 30.81ms | 1.740× | 1.49% | PASS |
| file_writes | `oltp_update_index` | 62.02ms | 105.23ms | 1.697× | 1.94% | PASS |
| file_writes | `oltp_update_non_index` | 43.54ms | 67.46ms | 1.549× | 2.11% | PASS |
| file_writes | `oltp_delete_insert` | 47.65ms | 84.71ms | 1.778× | 1.62% | PASS |
| file_writes | `oltp_write_only` | 30.93ms | 54.64ms | 1.766× | 1.66% | PASS |
| file_writes | `types_delete_insert` | 30.21ms | 46.13ms | 1.527× | 1.92% | PASS |
| file_writes | `oltp_read_write` | 68.50ms | 104.56ms | 1.527× | 1.25% | PASS |
| ac_reads | `oltp_point_select` | 27.01ms | 30.85ms | 1.142× | 1.22% | PASS |
| ac_reads | `oltp_range_select` | 10.76ms | 10.44ms | 0.970× | 1.63% | PASS |
| ac_reads | `oltp_sum_range` | 10.59ms | 10.18ms | 0.961× | 1.29% | PASS |
| ac_reads | `oltp_order_range` | 2.29ms | 2.28ms | 0.996× | 1.51% | PASS |
| ac_reads | `oltp_distinct_range` | 2.99ms | 3.00ms | 1.002× | 1.46% | PASS |
| ac_reads | `oltp_index_scan` | 4.04ms | 4.87ms | 1.205× | 2.13% | PASS |
| ac_reads | `select_random_points` | 14.72ms | 16.73ms | 1.136× | 1.53% | PASS |
| ac_reads | `select_random_ranges` | 3.71ms | 4.31ms | 1.161× | 1.54% | PASS |
| ac_reads | `covering_index_scan` | 3.59ms | 3.75ms | 1.042× | 2.46% | PASS |
| ac_reads | `groupby_scan` | 23.08ms | 24.65ms | 1.068× | 0.41% | PASS |
| ac_reads | `index_join` | 5.83ms | 7.32ms | 1.257× | 1.73% | PASS |
| ac_reads | `index_join_scan` | 3.64ms | 4.77ms | 1.311× | 3.05% | PASS |
| ac_reads | `types_table_scan` | 804.21ms | 891.64ms | 1.109× | 0.74% | PASS |
| ac_reads | `table_scan` | 892.48ms | 966.73ms | 1.083× | 0.97% | PASS |
| ac_reads | `oltp_read_only` | 90.88ms | 99.39ms | 1.094× | 1.00% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 8.69ms | 29.58ms | 3.403× | 5.54% | PASS |
| ac_writes | `oltp_insert_ac` | 10.14ms | 43.45ms | 4.286× | 6.35% | PASS |
| ac_writes | `oltp_update_index_ac` | 11.06ms | 51.38ms | 4.645× | 4.90% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 9.25ms | 38.44ms | 4.154× | 6.41% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 9.87ms | 45.22ms | 4.582× | 5.23% | PASS |
| ac_writes | `oltp_write_only_ac` | 9.74ms | 43.49ms | 4.465× | 3.73% | PASS |
| ac_writes | `types_delete_insert_ac` | 8.75ms | 38.94ms | 4.448× | 8.03% | PASS |
| ac_writes | `oltp_read_write_ac` | 13.95ms | 51.35ms | 3.681× | 7.31% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio MAD | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 28.27ms | 29.94ms | 1.059× | 2.18% | PASS |
| mem_reads | `oltp_range_select` | 16.36ms | 15.74ms | 0.962× | 1.37% | PASS |
| mem_reads | `oltp_sum_range` | 14.29ms | 15.73ms | 1.101× | 1.19% | PASS |
| mem_reads | `oltp_order_range` | 3.01ms | 3.02ms | 1.003× | 1.36% | PASS |
| mem_reads | `oltp_distinct_range` | 3.70ms | 3.76ms | 1.015× | 1.21% | PASS |
| mem_reads | `oltp_index_scan` | 3.71ms | 4.52ms | 1.220× | 2.14% | PASS |
| mem_reads | `select_random_points` | 21.35ms | 23.54ms | 1.103× | 3.69% | PASS |
| mem_reads | `select_random_ranges` | 6.09ms | 6.59ms | 1.082× | 1.52% | PASS |
| mem_reads | `covering_index_scan` | 3.39ms | 3.51ms | 1.036× | 3.09% | PASS |
| mem_reads | `groupby_scan` | 30.67ms | 31.26ms | 1.019× | 1.70% | PASS |
| mem_reads | `index_join` | 6.51ms | 8.38ms | 1.287× | 3.34% | PASS |
| mem_reads | `index_join_scan` | 3.55ms | 4.59ms | 1.293× | 2.18% | PASS |
| mem_reads | `types_table_scan` | 876.41ms | 985.27ms | 1.124× | 1.12% | PASS |
| mem_reads | `table_scan` | 1.10s | 1.08s | 0.986× | 4.04% | PASS |
| mem_reads | `oltp_read_only` | 122.54ms | 125.78ms | 1.026× | 2.71% | PASS |
| mem_writes | `oltp_bulk_insert` | 189.33ms | 268.39ms | 1.418× | 0.97% | PASS |
| mem_writes | `oltp_insert` | 15.11ms | 29.16ms | 1.930× | 0.76% | PASS |
| mem_writes | `oltp_update_index` | 56.73ms | 99.49ms | 1.754× | 2.06% | PASS |
| mem_writes | `oltp_update_non_index` | 43.34ms | 69.34ms | 1.600× | 1.83% | PASS |
| mem_writes | `oltp_delete_insert` | 41.62ms | 79.83ms | 1.918× | 1.15% | PASS |
| mem_writes | `oltp_write_only` | 23.11ms | 49.74ms | 2.153× | 1.86% | PASS |
| mem_writes | `types_delete_insert` | 27.83ms | 44.78ms | 1.609× | 1.23% | PASS |
| mem_writes | `oltp_read_write` | 86.36ms | 122.97ms | 1.424× | 2.16% | PASS |
| file_reads | `oltp_point_select` | 97.81ms | 53.98ms | 0.552× | 0.94% | PASS |
| file_reads | `oltp_range_select` | 24.25ms | 18.44ms | 0.760× | 1.61% | PASS |
| file_reads | `oltp_sum_range` | 21.62ms | 18.16ms | 0.840× | 1.16% | PASS |
| file_reads | `oltp_order_range` | 3.79ms | 3.29ms | 0.870× | 1.10% | PASS |
| file_reads | `oltp_distinct_range` | 4.57ms | 4.11ms | 0.900× | 1.34% | PASS |
| file_reads | `oltp_index_scan` | 10.89ms | 7.26ms | 0.667× | 1.12% | PASS |
| file_reads | `select_random_points` | 28.36ms | 25.65ms | 0.904× | 1.06% | PASS |
| file_reads | `select_random_ranges` | 13.12ms | 9.05ms | 0.689× | 0.81% | PASS |
| file_reads | `covering_index_scan` | 10.64ms | 6.03ms | 0.566× | 1.32% | PASS |
| file_reads | `groupby_scan` | 31.09ms | 32.12ms | 1.033× | 1.25% | PASS |
| file_reads | `index_join` | 10.36ms | 9.96ms | 0.962× | 1.56% | PASS |
| file_reads | `index_join_scan` | 4.23ms | 4.96ms | 1.175× | 1.71% | PASS |
| file_reads | `types_table_scan` | 946.23ms | 1.02s | 1.082× | 2.98% | PASS |
| file_reads | `table_scan` | 1.21s | 1.10s | 0.910× | 2.55% | PASS |
| file_reads | `oltp_read_only` | 225.87ms | 162.58ms | 0.720× | 1.05% | PASS |
| file_writes | `oltp_bulk_insert` | 240.29ms | 330.26ms | 1.374× | 4.77% | PASS |
| file_writes | `oltp_insert` | 27.58ms | 46.86ms | 1.699× | 15.40% | PASS |
| file_writes | `oltp_update_index` | 158.28ms | 169.48ms | 1.071× | 5.48% | PASS |
| file_writes | `oltp_update_non_index` | 136.72ms | 123.98ms | 0.907× | 1.95% | PASS |
| file_writes | `oltp_delete_insert` | 131.46ms | 134.75ms | 1.025× | 6.13% | PASS |
| file_writes | `oltp_write_only` | 100.32ms | 107.81ms | 1.075× | 25.84% | PASS |
| file_writes | `types_delete_insert` | 80.29ms | 80.78ms | 1.006× | 10.07% | PASS |
| file_writes | `oltp_read_write` | 150.50ms | 171.31ms | 1.138× | 7.49% | PASS |
| ac_reads | `oltp_point_select` | 47.80ms | 52.30ms | 1.094× | 0.98% | PASS |
| ac_reads | `oltp_range_select` | 18.28ms | 18.29ms | 1.000× | 1.60% | PASS |
| ac_reads | `oltp_sum_range` | 16.64ms | 18.08ms | 1.087× | 1.64% | PASS |
| ac_reads | `oltp_order_range` | 3.36ms | 3.31ms | 0.987× | 0.71% | PASS |
| ac_reads | `oltp_distinct_range` | 4.12ms | 4.13ms | 1.002× | 0.76% | PASS |
| ac_reads | `oltp_index_scan` | 6.42ms | 7.35ms | 1.145× | 1.43% | PASS |
| ac_reads | `select_random_points` | 24.37ms | 26.01ms | 1.067× | 2.70% | PASS |
| ac_reads | `select_random_ranges` | 8.48ms | 9.09ms | 1.071× | 1.18% | PASS |
| ac_reads | `covering_index_scan` | 6.06ms | 6.05ms | 0.998× | 1.25% | PASS |
| ac_reads | `groupby_scan` | 30.82ms | 32.23ms | 1.046× | 0.83% | PASS |
| ac_reads | `index_join` | 8.09ms | 10.02ms | 1.238× | 1.69% | PASS |
| ac_reads | `index_join_scan` | 3.80ms | 4.94ms | 1.302× | 1.04% | PASS |
| ac_reads | `types_table_scan` | 879.88ms | 986.35ms | 1.121× | 1.08% | PASS |
| ac_reads | `table_scan` | 1.11s | 1.08s | 0.967× | 5.46% | PASS |
| ac_reads | `oltp_read_only` | 145.50ms | 157.59ms | 1.083× | 1.10% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 21.63ms | 69.80ms | 3.226× | 25.73% | PASS |
| ac_writes | `oltp_insert_ac` | 26.25ms | 106.39ms | 4.054× | 53.73% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.33ms | 105.71ms | 3.732× | 38.32% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 20.76ms | 92.92ms | 4.476× | 31.88% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.64ms | 87.34ms | 3.279× | 40.11% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.00ms | 90.41ms | 3.617× | 28.95% | PASS |
| ac_writes | `types_delete_insert_ac` | 27.19ms | 78.42ms | 2.884× | 49.85% | PASS |
| ac_writes | `oltp_read_write_ac` | 30.74ms | 136.39ms | 4.438× | 26.70% | PASS |

</details>

## Version-control latency

Wall time: 2m 9s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 80.02ms | 200.00ms | 40.0% | 0.58% | PASS |
| `status_dirty_many_tables` | 82.62ms | 200.00ms | 41.3% | 0.44% | PASS |
| `diff_regular_working_one_table` | 75.55ms | 150.00ms | 50.4% | 0.41% | PASS |
| `diff_regular_working_many_tables` | 88.21ms | 200.00ms | 44.1% | 0.49% | PASS |
| `diff_stat_working_many_tables` | 88.26ms | 200.00ms | 44.1% | 0.40% | PASS |
| `diff_schema_working_many_tables` | 88.52ms | 200.00ms | 44.3% | 0.48% | PASS |
| `branch_list_many_branches` | 19.14ms | 100.00ms | 19.1% | 1.61% | PASS |
| `branch_create_delete` | 21.80ms | 100.00ms | 21.8% | 1.88% | PASS |
| `checkout_branch_clean` | 52.34ms | 200.00ms | 26.2% | 0.92% | PASS |
| `merge_data_no_conflicts` | 25.64ms | 150.00ms | 17.1% | 1.21% | PASS |
| `merge_schema_no_conflicts` | 18.64ms | 100.00ms | 18.6% | 2.27% | PASS |
| `merge_data_conflicts` | 110.73ms | 250.00ms | 44.3% | 0.34% | PASS |
| `merge_data_conflicts_with_resolve` | 110.64ms | 250.00ms | 44.3% | 0.29% | PASS |

Version-control ceiling result: **PASS**.

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh` and `test/vc_perf_ceiling.sh`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
