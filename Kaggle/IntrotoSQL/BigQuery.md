# BigQuery
> web service that can apply SQL to huge datasets
> Notes taken from Kaggle, [Intro to SQL](https://www.kaggle.com/code/dansbecker/getting-started-with-sql-and-bigquery)

---

### Step 1

```python
from google.cloud import bigquery
```

### Step 2
- create to retrieve info from BigQuery datasets
  
```python
# Create a "Client" object
client = bigquery.Client()
```

### Step 3
- In BigQuery, each dataset is contained in a corresponding project.
- In this case,  *hacker_news dataset* is contained in the **bigquery-public-data** project. To access the dataset,

```python
#Construct a reference to the "hacker_news" dataset with dataset() method
dataset_ref = client.dataset("hacker_news", project="bigquery-public-data")

# API request - fetch the dataset with get_dataset()
dataset = client.get_dataset(dataset_ref)
```

### Step 4
- Every dataset = a collection of tables
!!! think dataset --> spreadsheet files with multi tables --> list_tables()

```python
# List all the tables in the "hacker_news" dataset
tables = list(client.list_tables(dataset))

# Print names of all tables in the dataset (there are four!)
for table in tables:  
    print(table.table_id)
```

### Step 5 
- fetch a table ? try usin 'full'

```python
# Construct a reference to the "full" table
table_ref = dataset_ref.table("full")

# API request - fetch the table
table = client.get_table(table_ref)
```

### Step 6
**Table Schema** = structure of a table
  - understand the schema so can pull out data more effective

```python
# Print information on all the columns in the "full" table in the "hacker_news" dataset
table.schema
```
- Each SchemaField tells us about a specific column (which we also refer to as a field). In order, the information is:
> - The name of the column
> - The field type (or datatype) in the column
> - The mode of the column ('NULLABLE' means that a column allows NULL values, and is the default)
> - A description of the data in that column

Output:
```
SchemaField('by', 'string', 'NULLABLE', "The username of the item's author.",())
```

- The output tells us: 
> - the field (or column) is called by,
> - the data in this field is strings,
> - NULL values are allowed, and
> - it contains the usernames corresponding to each item's author.

### Step 7
- Use __list_rows()__ : check first 5 lines of full table to make sure this is right. 
- This returns a BigQuery **RowIterator** object that can quickly be converted to a pandas DataFrame with the **to_dataframe()** method.

```python
# Preview the first five lines of the "full" table
client.list_rows(table, max_results=5).to_dataframe()
```

search info in specific column:

```python
# Preview the first five entries in the "by" column of the "full" table
client.list_rows(table, selected_fields=table.schema[:1], max_results=5).to_dataframe()
```
