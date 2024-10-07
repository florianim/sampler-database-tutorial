# Mongodb Visualization

## Sparklines

## Textboxes

You can also use simple textboxes to add info to the sampler dashboard. Here, we will add basic information about our MongoDB database to sampler.

## Create YAML configuration file

Create a file using the following command:
`touch mongodb_vis.yml`{{exec}}

## Insert data to database
Enter mongoDB shell:
`mongo`{{exec}}

Insert some dummy data into the database:
`for (let i = 1; i <= 100; i++) {
    db.test.insert({number: i});
}`{{exec}}

## Create .yml configuration
Open the .yml file you created earlier in the editor and copy the following content to the file.

```
variables:
    mongo: mongo --quiet --host=localhost test

textboxes:
  - title: MongoDB stats (Table)
    rate-ms: 1000 
    scale: 4 
    init: $mongo
    sample: |
      "Objects:            " + db.stats(1024).objects + "\n" +
      "Total size (kB):    " + db.stats(1024).totalSize + "\n" +
      "Avg obj size (kB):  " + db.stats(1024).avgObjSize + "\n" +
      "Indexes:            " + db.stats(1024).indexes + "\n" +
      "Index size (kB):    " + db.stats(1024).indexSize + "\n" +
      "Memory usage (kB):  " + db.serverStatus().mem.resident
  - title: MongoDB Operations 
    rate-ms: 1000
    scale: 4
    init: $mongo 
    sample: |
        "Inserts:  " + db.serverStatus().opcounters.insert
        "Queries:  " + db.serverStatus().opcounters.query
        "Updates:  " + db.serverStatus().opcounters.update
```

Detailed explanation:

variables: Defines a variable named mongo to simplify subsequent calls to MongoDB. This specifies the command to connect to MongoDB, which connects to the local test database.

textboxes: Defines two textboxes for displaying MongoDB statistics and operation counts.

The first textbox: MongoDB stats (Table) displays some basic MongoDB database statistics, such as the number of objects, total size, average object size, etc. This textbox will refresh every 1000 milliseconds. This textbox is refreshed every 1000 milliseconds.

The second text box, MongoDB Operations, shows the count of operations in the database, including inserts, queries, updates, and so on.

sample: defines specific commands for getting data from MongoDB. The db.stats() and db.serverStatus() methods are used to retrieve database statistics and operation counts, and output the results as a string. The \n in the output is used for line breaks so that the content is displayed as a table on the Sampler dashboard.

## Run sampler

Finally, you can run sampler using `sampler --config mongodb_vis.yml`{{exec}}.
Feel free to play around with different configurations!

## Gauges