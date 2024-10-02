# Visualizing MySQL Databases

## Creating a YAML Configuration File

Create a .yml file to define the metrics to be displayed.

`touch mysql_visualization.yml`{{exec}}

## Barchart

Barcharts are used to show comparisons of multiple data items, usually in the form of bar charts for static or real-time data. In Sampler, the barcharts configuration can be used to display e.g. total number of records, maximum/minimum values, average values, etc.

Example

![Mysql Status](./img/bar.png)

## Insert Test Data

First open mysql

`mysql`{{exec}}

`CREATE TABLE metrics ( id INT AUTO_INCREMENT PRIMARY KEY, value FLOAT NOT NULL, created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP); INSERT INTO metrics (value) VALUES (10.5), (14.8), (12.1), (9.4), (15.7);`{{exec}}


## Barcharts configuration: extracting data from test_db

We will use Sampler's barcharts to display some basic statistics from the database, such as the total number of records in the metrics table, the maximum, minimum, and average values.

Please put this part of the code in the yml file

```
barcharts:
  - title: "Test Database Metrics Overview"
    rate-ms: 2000
    scale: 0
    items:
      - label: "Total Records"
        sample: "echo \"SELECT COUNT(*) FROM metrics;\" | mysql -u test_user -ppassword test_db -N"
      - label: "Max Value"
        sample: "echo \"SELECT MAX(value) FROM metrics;\" | mysql -u test_user -ppassword test_db -N"
      - label: "Min Value"
        sample: "echo \"SELECT MIN(value) FROM metrics;\" | mysql -u test_user -ppassword test_db -N"
      - label: "Average Value"
        sample: "echo \"SELECT AVG(value) FROM metrics;\" | mysql -u test_user -ppassword test_db -N"
```









