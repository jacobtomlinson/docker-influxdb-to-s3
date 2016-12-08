# Docker InfluxDB to S3  [![](https://images.microbadger.com/badges/version/jacobtomlinson/influxdb-to-s3.svg)](https://microbadger.com/images/jacobtomlinson/influxdb-to-s3 "Get your own version badge on microbadger.com") [![](https://images.microbadger.com/badges/image/jacobtomlinson/influxdb-to-s3.svg)](https://microbadger.com/images/jacobtomlinson/influxdb-to-s3 "Get your own image badge on microbadger.com")

This container periodically runs a backup of an InfluxDB database to an S3 bucket. It also has the ability to restore.

## Usage

### Default cron (1am daily)

```shell
docker run \
    -e DATABASE=mydatabase \
    -e DATABASE_HOST=1.2.3.4 \
    -e S3_BUCKET=mybackupbucket \
    -e AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE \
    -e AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY \
    -e AWS_DEFAULT_REGION=us-west-2 \
    jacobtomlinson/influxdb-to-s3:latest
```

### Custom cron timing

```shell
docker run \
    -e DATABASE=mydatabase \
    -e DATABASE_HOST=1.2.3.4 \
    -e S3_BUCKET=mybackupbucket \
    -e AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE \
    -e AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY \
    -e AWS_DEFAULT_REGION=us-west-2 \
    jacobtomlinson/influxdb-to-s3:latest \
    cron "* * * * *"
```

### Run backup

```shell
docker run \
    -e DATABASE=mydatabase \
    -e DATABASE_HOST=1.2.3.4 \
    -e S3_BUCKET=mybackupbucket \
    -e AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE \
    -e AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY \
    -e AWS_DEFAULT_REGION=us-west-2 \
    jacobtomlinson/influxdb-to-s3:latest \
    backup
```

### Restore

```shell
docker run \
    -v /path/to/influxdb:/var/lib/influxdb \
    -e DATABASE=mydatabase \
    -e S3_BUCKET=mybackupbucket \
    -e AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE \
    -e AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY \
    -e AWS_DEFAULT_REGION=us-west-2 \
    jacobtomlinson/influxdb-to-s3:latest \
    restore
```

## Environment Variables

| Variable        | Description      | Example Usage  | Default   | Optional?  |
| --------------- |:---------------:| -----:| -----:| --------:|
| `S3_BUCKET`               | Name of bucket | `mybucketname` | None | No |
| `S3_KEY_PREFIX` | S3 directory to place files in | `backups` or `backups/sqlite` | None | Yes |
| `AWS_ACCESS_KEY_ID`       | AWS Access key | `AKIAIO...` | None      | Yes (if using instance role) |
| `AWS_SECRET_ACCESS_KEY`   |  AWS Secret Key |  `wJalrXUtnFE...` | None   | Yes (if using instance role) |
| `AWS_DEFAULT_REGION`   | AWS Default Region | `us-west-2`    | `us-west-1`   | Yes |
| `DATABASE` | Database to backup  | `telegraf` | None   | No |
| `DATABASE_HOST` | Hostname or IP of influxdb instance  | `1.2.3.4` | `localhost`   | Yes |
| `DATABASE_PORT` | Port of influxdb instance  | `8098` | `8088`   | Yes |
| `DATABASE_META_DIR` | Path to local influxdb meta dir  | `/path/to/influxdb/meta` | `/var/lib/influxdb/meta`   | Yes |
| `DATABASE_DATA_DIR` | Path to local influxdb data dir  | `/path/to/influxdb/data` | `/var/lib/influxdb/data`   | Yes |
| `BACKUP_PATH` | Path to write the backup (within the container)  | `/myvolume/mybackup.db` | `${DATABASE_PATH}.bak`   | Yes |
