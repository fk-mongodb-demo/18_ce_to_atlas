# CE to Atlas

## Install CE 4.4.6

Download the installer from this page [Archives Download](https://www.mongodb.com/try/download/community-edition/releases/archive)

![image](https://github.com/user-attachments/assets/c013bfea-7a3e-4ac5-87d4-2887dc915b36)

Install libssl1.1

```
wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.0g-2ubuntu4_amd64.deb
sudo dpkg -i libssl1.1_1.1.0g-2ubuntu4_amd64.deb
```

Install MongoDB 4.4.6

```
dpkg -i mongodb-org-server_4.4.6_amd64.deb
```

Install Mongo Shell 4.4.6

```
dpkg -i mongodb-org-shell_4.4.6_amd64.deb
```

Start Mongo Daemon

```
mongod -f /etc/mongod.conf --fork
```

Connect to Mongo database using Mongo Shell

```
mongo --host localhost --port 27017
```

Output
```
MongoDB shell version v4.4.6
connecting to: mongodb://localhost:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("ced33bd0-e92c-4c37-a0e9-ede4e1cb69d7") }
MongoDB server version: 4.4.6
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
        https://docs.mongodb.com/
Questions? Try the MongoDB Developer Community Forums
        https://community.mongodb.com
---
The server generated these startup warnings when booting:
        2025-04-12T06:26:17.096+00:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
        2025-04-12T06:26:17.817+00:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
        2025-04-12T06:26:17.817+00:00: You are running this process as the root user, which is not recommended
        2025-04-12T06:26:17.817+00:00: Soft rlimits too low
        2025-04-12T06:26:17.817+00:00:         currentValue: 1024
        2025-04-12T06:26:17.817+00:00:         recommendedMinimum: 64000
---
---
        Enable MongoDB's free cloud-based monitoring service, which will then receive and display
        metrics about your deployment (disk utilization, CPU, operation statistics, etc).

        The monitoring data will be available on a MongoDB website with a unique URL accessible to you
        and anyone you share the URL with. MongoDB may use this information to make product
        improvements and to suggest MongoDB products and deployment options to you.

        To enable free monitoring, run the following command: db.enableFreeMonitoring()
        To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
---
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> db.stats()
{
        "db" : "test",
        "collections" : 0,
        "views" : 0,
        "objects" : 0,
        "avgObjSize" : 0,
        "dataSize" : 0,
        "storageSize" : 0,
        "totalSize" : 0,
        "indexes" : 0,
        "indexSize" : 0,
        "scaleFactor" : 1,
        "fileSize" : 0,
        "fsUsedSize" : 0,
        "fsTotalSize" : 0,
        "ok" : 1
}
```
