### Default & Update behavior in SQL

- **Column condition**: `NOT NULL` + has a `DEFAULT` value  

1. **When column is not included in `UPDATE` statement**  
   - The column is set to its **default value**.  

2. **When `NULL` is explicitly updated**  
   - Since the column is `NOT NULL`,  
   - It does **not fall back to the default value**,  
   - Instead, an **error occurs**.  
