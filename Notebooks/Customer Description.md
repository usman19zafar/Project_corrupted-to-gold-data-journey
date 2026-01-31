Customer Data Reconstruction Pipeline (PySpark + Delta Lake)

    This project demonstrates a full end‑to‑end reconstruction pipeline for a customer dataset that was not just dirty — it was practically corrupted.
    The source file contained inconsistent delimiters, malformed dates, mixed encodings, duplicate records, and broken schema structure.
    Instead of relying on standard CSV readers, the pipeline rebuilds the dataset from raw text using PySpark.

The goal: produce a clean, deduped, trustworthy Silver table suitable for analytics, MDM workflows, and downstream modeling.

Key Skills Demonstrated

__________________________________________________________________________________________________________________________________________________________________
1. Robust Raw Ingestion
    The pipeline ingests the file using spark.read.text(), treating each line as opaque text.
    This bypasses schema inference failures and allows full control over reconstruction.
    
__________________________________________________________________________________________________________________________________________________________________
2. Delimiter Normalization & Structural Repair
    The raw file contained tabs, commas, double‑delimiters, and date formats with embedded commas.

The pipeline:

    Normalizes all delimiters to semicolons
    Temporarily protects date formats
    Removes non‑ASCII characters
    Collapses repeated delimiters
    Reconstructs a consistent row structure

This shows the ability to handle severely corrupted input.
__________________________________________________________________________________________________________________________________________________________________
3. Manual Schema Reconstruction
After normalization, the pipeline splits each line into fields and maps them into columns.
This is a technique used when the source file is too broken for standard CSV parsing.


__________________________________________________________________________________________________________________________________________________________________
4. Domain‑Aware Data Cleaning
The pipeline applies targeted cleansing rules:

        Email normalization
        Phone number sanitization
        Country code standardization
        Status normalization

Multi‑format date parsing with fallback patterns
This demonstrates real‑world data quality engineering, not tutorial‑level cleaning.

__________________________________________________________________________________________________________________________________________________________________
5. Survivorship Scoring (MDM‑Style)
Duplicate customer records are evaluated using a scoring system:

        Valid email → +1
        Valid phone → +1
        Valid date → +1
        Valid status → +1

The highest‑scoring record per customer becomes the survivor.

This mirrors Master Data Management (MDM) survivorship logic.

__________________________________________________________________________________________________________________________________________________________________
6. Deterministic Deduplication with Window Functions

        A window function ranks duplicates by survivorship score.
        Only the top‑ranked record is retained.
        This ensures idempotent, deterministic deduplication.

__________________________________________________________________________________________________________________________________________________________________
7. Surrogate Key Resequencing
Customer IDs are resequenced into a clean, consistent format:

```Code
C001, C002, C003, ...
```
This is useful when the original IDs are corrupted or non‑unique.

__________________________________________________________________________________________________________________________________________________________________
8. Silver‑Layer Output (Delta Lake)
The final cleaned dataset is written to a Delta Lake Silver table, following medallion architecture best practices.

Pipeline Flow

```Code
RAW → Normalization → Schema Reconstruction → Cleansing → Survivorship Scoring
    → Deduplication → Resequencing → SILVER (Delta)
```
Technologies Used

    PySpark (DataFrame API, regex, window functions)
    Delta Lake (ACID storage, medallion architecture)
    Regex‑based text normalization
    MDM survivorship scoring
    Distributed data processing
