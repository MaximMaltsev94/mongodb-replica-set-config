Start data and config-db replicas:

```
mongod --config /sharding-config/shard-3/shard-3-secondary-1.conf
mongod --config /sharding-config/shard-3/shard-3-secondary-2.conf
mongod --config /sharding-config/shard-3/shard-3-primary.conf
```

Connect to primary replica
```
mongo --port <primary:port>
```

Initialise replication
```
rs.initiate();
rs.add({ host : "secondary-1:port"});
rs.add({ host : "secondary-1:port"});
```

Start `mongos`
```
mongos --configdb configdb-repl/localhost:27400,localhost:27401,localhost:27402 --port 27500

```

Connect to `mongos`
```
mongo --port 27500
```

Add shards
```
sh.addShard("repl1-name/primary:port,secondary:port")
sh.addShard("repl2-name/primary:port,secondary:port")
```

Enable sharding for collection
```
sh.enableSharding("bank")
sh.shardCollection("bank.customers", { "name" : 1 })
```
