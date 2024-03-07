# Apache Iceberg Spark Tutorial

## Spark and Iceberg Quickstart

Reference: https://iceberg.apache.org/spark-quickstart/#spark-and-iceberg-quickstart

```
docker-compose up
docker exec -it spark-iceberg spark-sql
```

On the container:

```
# Create table
CREATE TABLE demo.nyc.taxis
(
  vendor_id bigint,
  trip_id bigint,
  trip_distance float,
  fare_amount double,
  store_and_fwd_flag string
)
PARTITIONED BY (vendor_id);

# Add data
INSERT INTO demo.nyc.taxis
VALUES (1, 1000371, 1.8, 15.32, 'N'), (2, 1000372, 2.5, 22.15, 'N'), (2, 1000373, 0.9, 9.01, 'N'), (1, 1000374, 8.4, 42.13, 'Y');

# Read data
SELECT * FROM demo.nyc.taxis;
```


Adding a catalog:

```
docker exec -it spark-iceberg /bin/bash

spark-sql --packages org.apache.iceberg:iceberg-spark-runtime-3.5_2.12:1.4.3\
    --conf spark.sql.extensions=org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions \
    --conf spark.sql.catalog.spark_catalog=org.apache.iceberg.spark.SparkSessionCatalog \
    --conf spark.sql.catalog.spark_catalog.type=hive \
    --conf spark.sql.catalog.local=org.apache.iceberg.spark.SparkCatalog \
    --conf spark.sql.catalog.local.type=hadoop \
    --conf spark.sql.catalog.local.warehouse=$PWD/warehouse \
    --conf spark.sql.defaultCatalog=local
```