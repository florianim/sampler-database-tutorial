# Postgres Visualization

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

As you can see, we define two textboxes, one for general MongoDB stats and one for showing the total amount of various operations executed. You can have multiple stats within one textbox. The example configuration also shows how you can create a simple table within a textbox!

## Run sampler

Finally, you can run sampler using `sampler --config mongodb_vis.yml`{{exec}}.
Feel free to play around with different configurations!

## Gauges