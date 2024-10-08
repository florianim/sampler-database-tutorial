# PostgreSQL Visualization



## Adding Information with Barcharts, Sparklines and Gauges

Next we will explore Barcharts as well as Sparklines and Gauges. While Gauges give you the ability to see the current value of a statistic in a pleasant way, Sparklines allow you to see the history of a statistic for better, more comprehensive monitoring. We will demonstrate these three sampler features using our postgresql database.

## Create YAML configuration file

Create a file using the following command:

`touch postgres_vis.yml`{{exec}}

## Insert data to database

Log in to the PostgreSQL Shell:
`sudo -i -u postgres`{{exec}}

Access the PostgreSQL shell by typing:

`psql`{{exec}}

Set the password for the postgres user in the PostgreSQL Shell:
`ALTER USER postgres PASSWORD '123';`{{exec}}


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
  PGPASSWORD: 123
  postgres_connection: psql -h localhost -U postgres -d test_db --no-align --tuples-only -c 

barcharts:
  - title: PostgreSQL Metrics
    rate-ms: 1000
    scale: 0
    items:
      - label: Total Records
        sample: $postgres_connection "SELECT COUNT(*) FROM metrics;"
      - label: Table Size (kB)
        sample: $postgres_connection "SELECT pg_total_relation_size('metrics') / 1024;"
      - label: Reads
        sample: $postgres_connection "SELECT sum(blks_read) FROM pg_stat_database WHERE datname='test_db';"
      - label: Blocks Hit
        sample: $postgres_connection "SELECT sum(blks_hit) FROM pg_stat_database WHERE datname='test_db';"

sparklines:
  - title: PostgreSQL DB size (kB)
    rate-ms: 1000 
    scale: 4 
    sample: $postgres_connection "SELECT (pg_database_size('test_db'))/1000;"

gauges:
  - title: PostgreSQL Cache Hit Rate
    rate-ms: 500
    scale: 4
    percent-only: true
    color: 190
    cur:
      sample: $postgres_connection "SELECT (sum(blks_hit) / (sum(blks_hit) + sum(blks_read))) * 100 AS cache_hit_ratio FROM pg_stat_database;"
    max:
      sample: echo 100
    min:
      sample: echo 90
```{{copy}}

Detailed Explanation:

variables: Defines a variable named psql to simplify subsequent PostgreSQL command calls. The command to connect to the test_db database is specified here.

barcharts: Defines the barcharts as seen before in the mySQL example.

sparklines: Defines the sparkline graphic for the PostgreSQL database size.

gauges: This defines the historic gauge graph. Cur is used for the current statistic, whie max and min represent the maximum and minimum values that should be set for the bars. Additionally we set perent-only to true in order to only show the percentage value for the cache hit rate.

## Run sampler

`exit`{{exec}}



Finally, you can run sampler using `sampler --config postgres_vis.yml`{{exec}}

Feel free to play around with different configurations!

## Gauges