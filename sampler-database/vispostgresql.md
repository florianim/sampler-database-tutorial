# Sparklines and Gauges

Next we will explore Sparklines and Gauges. While Gauges give you the ability to see the current value of a statistic in a pleasant way, Sparklines allow you to see the history of a statistic for better, more comprehensive monitoring. We will demonstrate these two sampler features using our postgresql database.

## Add sample data to database
Switch to the database user:
`sudo -i -u postgres`{{exec}}

Go to the PostgreSQL shell:
`psql`{{exec}}

Create a table for our sample data:
```
CREATE TABLE numbers (
    id SERIAL PRIMARY KEY,
    value INTEGER
);
```{{exec}}

Insert dummy data:
```
INSERT INTO numbers (value) VALUES 
(1),
(2),
(3),
(10),
(20);
```{{exec}}

Exit the postgres shell ...
`exit`{{exec}}

... and return to the standard user:
`exit`{{exec}}


## Create YAML configuration file

Create a file using the following command:
`touch postgresql_vis.yml`{{exec}}

Now copy the following setup into your file:
```
sparklines:
  - title: PostgreSQL DB size
    rate-ms: 1000 
    scale: 4 
    multistep-inig: 
      - sudo -i -u postgres
      - psql
    sample: SELECT pg_size_pretty(pg_database_size('test_db'));
gauges:
  - title: PostgreSQL Cache Hit Rate
    rate-ms: 500
    scale: 4
    percent-only: false
    color: 190
    init: psql
    cur:
      sample: SELECT (sum(blks_hit) / (sum(blks_hit) + sum(blks_read))) * 100 AS cache_hit_ratio FROM pg_stat_database;
    max:
      sample: echo 100
    min:
      sample: echo 0
```{{copy}}

## Run sampler
Now you run sampler using `sampler --config postgresql_vis.yml`{{exec}}. Feel free to edit the YAML configuration to try other visualizations!

Sampler shows you the cache hit rate of PostgreSQL as well as the database size, to test the visualizations, try to insert more data into the database to see that the sparkline changes.