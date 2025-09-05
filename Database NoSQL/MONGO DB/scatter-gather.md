- In un cluster shardato, una **scatter-gather query** (query costosa) è una query che **non può essere indirizzata a uno shard specifico**, perché **non usa la shard key**.

- il router **mongos** deve **inoltrare la query a tutti gli shard**, raccogliere i risultati parziali e poi unirli/ordinarli.


