# PostgreSQL Visualization



## Adding Information with Textboxes

With Sampler's textboxes, you can display various statistical information about your PostgreSQL database in a dashboard. Next, we will demonstrate how to display basic PostgreSQL information, including the total number of rows and operation counts for the database.

## Create YAML configuration file

Create a file using the following command:

`touch postgres_vis.yml`{{exec}}

## Insert data to database

Log in to the PostgreSQL Shell:
`sudo -i -u postgres`{{exec}}

Access the PostgreSQL shell by typing:

`psql`{{exec}}

In the PostgreSQL Shell, create a test database and table and insert some data:

`CREATE DATABASE test_db;`{{exec}}


`\c test_db`{{exec}}

`CREATE TABLE metrics ( id SERIAL PRIMARY KEY, value FLOAT NOT NULL, created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP );`{{exec}}

`INSERT INTO metrics (value) VALUES (10.5), (15.3), (12.8), (9.4), (14.2);`{{exec}}

`\q`{{exec}}


## Create .yml configuration
Open the .yml file you created earlier in the editor and copy the following content to the file.

```
variables:
    psql: psql -U postgres -d test_db -t -c 

textboxes:
  - title: PostgreSQL Stats (Table)
    rate-ms: 1000 
    scale: 4 
    sample: |
      "Total Rows:        " + $psql "'SELECT COUNT(*) FROM metrics;'" + "\n" +
      "Table Size (kB):   " + $psql "'SELECT pg_total_relation_size(''metrics'');'" + "\n" +
      "Index Size (kB):   " + $psql "'SELECT pg_indexes_size(''metrics'');'" + "\n" +
      "Avg Row Length:    " + $psql "'SELECT pg_column_size(row(SELECT * FROM metrics LIMIT 1));'"
  - title: PostgreSQL Operations
    rate-ms: 1000
    scale: 4
    sample: |
      "Transactions: " + $psql "'SELECT sum(xact_commit) FROM pg_stat_database WHERE datname=''test_db'';'" + "\n" +
      "Reads:        " + $psql "'SELECT sum(blks_read) FROM pg_stat_database WHERE datname=''test_db'';'" + "\n" +
      "Blocks Hit:   " + $psql "'SELECT sum(blks_hit) FROM pg_stat_database WHERE datname=''test_db'';'"

```

Detailed Explanation:

variables: Defines a variable named psql to simplify subsequent PostgreSQL command calls. The command to connect to the test_db database is specified here.

textboxes: Defines two textboxes that display PostgreSQL statistics and operation counts.

The first textbox: PostgreSQL Stats (Table) displays basic statistics about the PostgreSQL database, such as the total number of rows, table size, index size, and so on.

The second text box: PostgreSQL Operations shows some operation statistics related to PostgreSQL, such as the number of transactions, the number of read blocks, the number of cache hit blocks, and so on.

sample: Runs a SQL query using the commands in the $psql variable and displays the results in a table with \n line breaks. For each statistic, a SQL query is executed to get and display database-specific information.

## Run sampler

`exit`{{exec}}



Finally, you can run sampler using `sampler --config postgres_vis.yml`{{exec}}

Feel free to play around with different configurations!

## Gauges