## Run Locally with Docker-compose
Motivated by these repos:
- https://github.com/bitsondatadev/trino-getting-started/tree/main/hive/trino-minio
- https://github.com/sensei23/trino-hive-docker

Step 1 - Run with docker-compose
```bash
docker-compose up -d
```
Step 2 - Go to MinIO pod/container

mc alias set myminio http://localhost:9000 minio minio123
mc mb myminio/iceberg

Step 3 - Go to the running trino container
```bash
docker container exec -it iceberg-stack-docker-trino-coordinator-1 trino
```
Step 4 -  Create schema and table and play around with trino
```sql
CREATE SCHEMA iceberg.test
WITH (location = 's3a://iceberg/');

CREATE TABLE iceberg.test.customer
WITH (
    format = 'ORC'
) 
AS SELECT * FROM tpch.tiny.customer;
```
Step 5: See the Trino UI at localhost:8080.