```yaml
### Dirty Data Generation

The raw CSV preview included in this repository was intentionally corrupted.

“The source data wasn’t just dirty — it was effectively corrupted and required full reconstruction before processing.”  

The corruption was generated using a deterministic script (`dirty_data_generator.py`) to simulate real-world issues such as:

- Mixed delimiters (`;`, `,`, `\t`)  
- Broken quoting and escape sequences  
- Mixed date formats  
- Duplicate or conflicting records  
- Nulls in key columns  
- Non-ASCII characters and encoding noise  
- Header rows mid-file and schema drift
- Missing Values even for primary key  

This script ensures that **every corruption observed in the raw preview is reproducible**, so no issue is left undocumented.
```
