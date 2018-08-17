# Presto 123

---

main document: https://prestodb.io/docs/current/


## Installation

(a single machine for testing)

---

Create a folder for Presto, then under the new folder, run the following commands from shell.

```bash

## download presto server

wget https://repo1.maven.org/maven2/com/facebook/presto/presto-server/0.208/presto-server-0.208.tar.gz

tar xfvz presto-server-0.208.tar.gz

cd presto-server-0.208/


## download presto cli

wget https://repo1.maven.org/maven2/com/facebook/presto/presto-cli/0.208/presto-cli-0.208-executable.jar

mv presto-cli-0.208-executable.jar bin/presto

chmod +x bin/presto



mkdir etc/catalog -p

mkdir /var/presto/data -p

```

## Configuration

---

Add the following configuration files under etc directory: 

* __etc/config.properties__

> coordinator=true<br/>
> node-scheduler.include-coordinator=true<br/>
> http-server.http.port=8081<br/>
> query.max-memory=50GB<br/>
> query.max-memory-per-node=1GB<br/>
> discovery-server.enabled=true<br/>
> discovery.uri=http://localhost:8081<br/>

* __etc/node.properties__

> node.environment=production<br/>
> node.id=ffffffff-ffff-ffff-ffff-ffffffffffff<br/>
> node.data-dir=/var/presto/data<br/>

* __etc/jvm.config__

> -server<br/>
> -Xmx16G<br/>
> -XX:+UseG1GC<br/>
> -XX:G1HeapRegionSize=32M<br/>
> -XX:+UseGCOverheadLimit<br/>
> -XX:+ExplicitGCInvokesConcurrent<br/>
> -XX:+HeapDumpOnOutOfMemoryError<br/>
> -XX:+ExitOnOutOfMemoryError<br/>

* __etc/log.properties__

> com.facebook.presto=INFO

## Add Catalogs

document: https://prestodb.io/docs/current/connector.html

---

* __Hive: etc/catalog/hive.properties__

> connector.name=hive-hadoop2<br/>
> hive.metastore.uri=thrift://localhost:9083<br/>

* __PostgreSQL: etc/catalog/postgresql.properties__

> connector.name=postgresql<br/>
> connection-url=jdbc:postgresql://example.net:5432/database<br/>
> connection-user=root<br/>
> connection-password=secret<br/>

* __MySQL: etc/catalog/mysql.properties__

> connector.name=mysql<br/>
> connection-url=jdbc:mysql://example.net:3306<br/>
> connection-user=root<br/>
> connection-password=secret<br/>

* __Kafka: etc/catalog/kafka.properties__

> connector.name=kafka<br/>
> kafka.table-names=table1,table2<br/>
> kafka.nodes=host1:port,host2:port<br/>

## Start Presto Server

---

```bash
bin/launcher start

```

## Start Presto cli

---

```bash
bin/presto --server localhost:8081 --catalog mysql --schema default

```

Then run the following sql:

```sql

SHOW SCHEMAS FROM mysql;

SHOW TABLES FROM mysql.sys;

DESCRIBE mysql.sys.version;

SHOW COLUMNS FROM mysql.sys.version;

SELECT * FROM mysql.sys.version;

```