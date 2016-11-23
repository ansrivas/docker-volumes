#### Save and load data from volumes in docker-compose files

1. `docker-compose up -d --build`

2. Save all the volumes locally:

  ```
  docker run --rm --volume dockervolumes_postgres-data:/mybackup -v $(pwd):/backup ubuntu tar czvf /backup/data.tar.gz /mybackup
  ```

  ```
  docker run --rm --volume dockervolumes_postgres-etc:/mybackup -v $(pwd):/backup ubuntu tar czvf /backup/etc.tar.gz /mybackup
  ```

  ```
  docker run --rm --volume dockervolumes_postgres-log:/mybackup -v $(pwd):/backup ubuntu tar czvf /backup/log.tar.gz /mybackup
  ```

3. Login into psql, write some data:

  ```
  # docker exec -it postgres_test psql -U postgres
  # \connect mydb
  # select * from test1;
  # insert into test1 values (1, '210101','DE') ;
  ```

4. Hit `docker-compose down -v` to remove all the data and volumes.

5. Reload all the volumes back:

  ```
  docker run --rm --volume dockervolumes_postgres-data:/mybackup -v $(pwd):/backup ubuntu bash -c "cd /mybackup && tar xzvf /backup/data.tar.gz --strip 1"
  ```

  ```
  docker run --rm --volume dockervolumes_postgres-etc:/mybackup -v $(pwd):/backup ubuntu bash -c "cd /mybackup && tar xzvf /backup/etc.tar.gz --strip 1"
  ```

  ```
  docker run --rm --volume dockervolumes_postgres-log:/mybackup -v $(pwd):/backup ubuntu bash -c "cd /mybackup && tar xzvf /backup/log.tar.gz --strip 1"
  ```

6. Bring up the containers again ( don't build them up)
  `docker-compose up -d`

7. Check if your data-persisted ?? :P

  ```
  # docker exec -it postgres_test psql -U postgres
  # \connect mydb
  # select * from test1;
  ```
