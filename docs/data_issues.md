```yaml
# Data Issues Catalog

This document catalogs all known issues detected in the raw data. It serves as a reference for recovery decisions and demonstrates the need for forensic analysis.

## Raw Data Issues

| Issue ID | Description | Sample Example | Impact |
|----------|-------------|----------------|--------|
| D001 | Inconsistent delimiters (mix of `,`, `;`, `\t`) | `C001,John;Doe\tjohn@example.com` | Parsing errors, schema shifts |
| D002 | Broken quoting | `"John Doe, john@example.com"` vs `John Doe, john@example.com` | Field splitting may fail |
| D003 | Mixed date formats | `2024-01-01`, `01/01/2024`, `January 1, 2024` | Date parsing errors |
| D004 | Duplicate rows | Multiple rows with same `customer_id` | Analytics bias, inflated counts |
| D005 | Nulls in key columns | `customer_id = null` | Joins and deduplication fail |
| D006 | Encoding noise / non-ASCII characters | `Jöhn Dœ` | Data standardization issues |
| D007 | Header rows inside file | `customer_id, first_name, ...` repeated in middle | Interferes with schema inference |

## Notes

- Issues are **observed, not inferred**.
- This catalog was created before any recovery transformations.
- Each entry informs deterministic recovery in the Silver layer.
```
