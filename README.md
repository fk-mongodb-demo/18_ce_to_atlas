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

## Configure ReplicaSet with 1 member

Update mongod.conf with these additional lines
```
replication:
  replSetName: "fk0"
```

Initiate cluster
```
rs.initiate({
  _id: "fk0",
  members: [{
      _id: 0,
      host: "localhost:27017"
    }
  ]
})
```

Check RS status
```
rs.hello()
```

Output of RS status
```
fk0:PRIMARY> rs.hello()
{
        "topologyVersion" : {
                "processId" : ObjectId("67fa086466a8a78b82b19bd1"),
                "counter" : NumberLong(6)
        },
        "hosts" : [
                "localhost:27017"
        ],
        "setName" : "fk0",
        "setVersion" : 1,
        "isWritablePrimary" : true,
        "secondary" : false,
        "primary" : "localhost:27017",
        "me" : "localhost:27017",
        "electionId" : ObjectId("7fffffff0000000000000001"),
        "lastWrite" : {
                "opTime" : {
                        "ts" : Timestamp(1744439543, 1),
                        "t" : NumberLong(1)
                },
                "lastWriteDate" : ISODate("2025-04-12T06:32:23Z"),
                "majorityOpTime" : {
                        "ts" : Timestamp(1744439543, 1),
                        "t" : NumberLong(1)
                },
                "majorityWriteDate" : ISODate("2025-04-12T06:32:23Z")
        },
        "maxBsonObjectSize" : 16777216,
        "maxMessageSizeBytes" : 48000000,
        "maxWriteBatchSize" : 100000,
        "localTime" : ISODate("2025-04-12T06:32:26.156Z"),
        "logicalSessionTimeoutMinutes" : 30,
        "connectionId" : 3,
        "minWireVersion" : 0,
        "maxWireVersion" : 9,
        "readOnly" : false,
        "ok" : 1,
        "$clusterTime" : {
                "clusterTime" : Timestamp(1744439543, 1),
                "signature" : {
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
                        "keyId" : NumberLong(0)
                }
        },
        "operationTime" : Timestamp(1744439543, 1)
}
```

## Populate RS with sample documents

Execute this insert commands 
```
db.col1.insertOne({"code": 1})
{
        "acknowledged" : true,
        "insertedId" : ObjectId("67fa0953080413f2d1181c5d")
}

db.col1.insertOne({"code": 2})
{
        "acknowledged" : true,
        "insertedId" : ObjectId("67fa0955080413f2d1181c5e")
}

db.col1.insertOne({"code": 3})
{
        "acknowledged" : true,
        "insertedId" : ObjectId("67fa0956080413f2d1181c5f")
}

db.col1.insertOne({"code": 4})
{
        "acknowledged" : true,
        "insertedId" : ObjectId("67fa0957080413f2d1181c60")
}
```

Check the content of the collections
```
fk0:PRIMARY> db.col1.find()
{ "_id" : ObjectId("67fa0953080413f2d1181c5d"), "code" : 1 }
{ "_id" : ObjectId("67fa0955080413f2d1181c5e"), "code" : 2 }
{ "_id" : ObjectId("67fa0956080413f2d1181c5f"), "code" : 3 }
{ "_id" : ObjectId("67fa0957080413f2d1181c60"), "code" : 4 }
```

## Enable security

Create key file
```
openssl rand -base64 756 > /app/fk0_key
chmod 400 /app/fk0_key
chown mongodb /app/fk0_key
```

Add these lines into the /etc/mongod.conf
```
security:
authorization: enabled
keyFile: /app/fk0_key
```

Add admin user
```
use admin
db.createUser(
    {
        user: "xxxxx",
        pwd: "xxxxx",
        roles: [
            {
                "role": "root",
                "db": "admin"
            }
        ],
    }
)
```

Verify login using Mongo Shell
```
mongo --host localhost --port 27017 -u xxxxx -p xxxxx
```

## Run mongodump

Download tools and install
```
wget https://fastdl.mongodb.org/tools/db/mongodb-database-tools-ubuntu2404-x86_64-100.12.0.deb
dpkg -i mongodb-database-tools-ubuntu2404-x86_64-100.12.0.deb
```

Export documents from CE 4.4.6 (only 2 documents, with query)
```
mongodump --db=db1 --collection=col1 -q='{ "code" : {"$lte": 2} }' --host localhost --port 27017 -u fkadmin -p fkpassword --authenticationDatabase=admin -o partial
```

Output
```
2025-04-12T07:11:38.771+0000    writing db1.col1 to partial/db1/col1.bson
2025-04-12T07:11:38.772+0000    done dumping db1.col1 (2 documents)
```

## Run mongorestore

Import documents directly into Atlas
```
mongorestore --uri="mongodb+srv://xxxxx:xxxxx@prd.hixjx.mongodb.net/"  partial
```

Output 
```
2025-04-12T07:12:16.696+0000    preparing collections to restore from
2025-04-12T07:12:16.697+0000    don't know what to do with file "partial/db1/prelude.json", skipping...
2025-04-12T07:12:16.697+0000    reading metadata for db1.col1 from partial/db1/col1.metadata.json
2025-04-12T07:12:16.749+0000    restoring db1.col1 from partial/db1/col1.bson
2025-04-12T07:12:16.781+0000    finished restoring db1.col1 (2 documents, 0 failures)
2025-04-12T07:12:16.781+0000    no indexes to restore for collection db1.col1
2025-04-12T07:12:16.781+0000    2 document(s) restored successfully. 0 document(s) failed to restore.
```

## Verify documents in Atlas

![image](https://github.com/user-attachments/assets/71cff522-6223-462c-82bf-080aa15e8241)

