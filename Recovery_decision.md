```yaml
# Recovery Decisions

This document explains **why and how** each transformation was applied to recover the data ethically and deterministically.

## Transformation Decisions

| Step | Action Taken | Reasoning | Ethical Note |
|------|-------------|-----------|--------------|
| T001 | Normalized all delimiters to `;` | Mixed delimiters caused schema shifts | Did not infer missing values; only standardized separator |
| T002 | Replaced non-ASCII characters with ASCII equivalents | Ensures consistency for downstream processing | Characters removed only when corrupt, not arbitrarily changed |
| T003 | Trimmed all whitespace | Prevents subtle mismatches in joins and deduplication | Safe, deterministic |
| T004 | Split cleaned lines into fields | Enforces consistent schema | No assumptions made about missing fields |
| T005 | Deduplication using survivorship scoring | Keeps most complete record per `customer_id` | Transparent scoring system; preserved tie-break rules |
| T006 | Standardized email, phone, country, status | Supports uniqueness and MDM compliance | Cleaning only, no value guessing |
| T007 | Converted dates to standard format | Facilitates analytics without data loss | Preserves nulls for ambiguous dates |
| T008 | Gold aggregation | Summarizes data for validation | Clearly labeled non-authoritative |

## Notes

- All steps are **reversible** and traceable.  
- Silver layer represents **trusted canonical data (MDM principles)**.  
- Gold layer demonstrates **downstream effects** without asserting analytical correctness.  
```
