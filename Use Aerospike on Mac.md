# Introduce Aerospike

## Aerospike Features:
* Key-Value Store: key-value store operations associate keys with a set of named values called records
* Flexible Data Model, schema-less, NonSQL database
* User-defined Functions
* Geospatial: Aerospike can store GeoJSON objects and execute various quries
* Aggregations: allow simple statistics and highly parallel map/reduce style computations
* Geographic Replication: Aerospike’s integrated geographic replication system copies data among cluster nodes automatically and continuously – for internet scale and disaster-proof applications.

## Terminology Map between Aerospike and RDBMS

|Aerospike|RDBMS|
|---------|-----|
|namespace|database|
|set      |table   |
|record   |row     |
|bin      |column  |

## More Explanations on Aerospike Features
* Aerospike is schemaless. You don't have to define sets or bins. 
* Aerospike is key-value store. Records are indexed by a key that applications used to retrieve record data
* Bin values are strongly typed, but the bins themselves are not. In Aerospike, different records can have different value types in the same bin. 
* Each record also contains metadata that tracks the generation count (data modification).

## Aerospike Daemon Management

The Aerospike daemon can be controlled using the SysV init script located at /etc/init.d/aerospike which supports the following options:

* start: to start the Aerospike database service: $/etc/init.d/aerospike start or $ service aerospike start
* coldstart
* status: to determine if the Aerospike daemon is currently running, use the status command: $/etc/init.d/aerospike status
* restart
* stop

## Configure Aerospike Namespace
Aerospike uses a single file to configure a database node which can be found at /etc/aerospike/aerospike.conf

$ vim /etc/aerospike/aerospike.conf

namespace <myfirstdb> {

    # memory-size 4G           # 4GB of memory to be used for index and data
    
    # replication-factor 2     # For multiple nodes, keep 2 copies of the data
    
    # high-water-memory-pct 60 # Evict non-zero TTL data if capacity exceeds
  
                               # 60% of 4GB
                               
    # stop-writes-pct 90       # Stop writes if capacity exceeds 90% of 4GB
    
    # default-ttl 0            # Writes from client that do not provide a TTL
    
                               # will default to 0 or never expire
                               
    # storage-engine memory    # Store data in memory only
}

The above is to create a new namespace (database) named as 'myfirstdb'.

## Aerospike Query Language: AQL
The aql tool provides an SQL-like command line interface for database, UDF and index management. Aerospike does not support SQL as a query or management language, but instead provides aql to provide an interface similar to tools that utilize SQL.

Help document URL: http://www.aerospike.com/docs/tools/aql

### aql Data Management
* List Namespaces: show namespaces
* List Sets: show sets
* List Bins: show bins

### Insert A Record

        INSERT INTO <ns>[.<set>] (PK, <bins>) VALUES (<key>, <values>)

Where 

- \<ns\> is the namespace for the record.
- \<set\> is the set name for the record.
- \<key\> is the record's primary key.
- \<bins\> is a comma-separated list of bin names.
- \<values\> is comma-separated list of bin values, which may include type cast expressions. Set to NULL (case insensitive & w/o quotes) to delete the bin.

Example:

        aql> INSERT INTO myfirstdb.testset (PK, a, b) VALUES ('xyz', 'abc', 123)

### Delete a Record

        DELETE FROM <ns>[.<set>] WHERE PK=<key>

Where

- \<ns\> is the namespace for the record.
- \<set\> is the set name for the record.
- \<key\> is the record's primary key.

Example:

        aql> DELETE FROM myfirstdb.testset WHERE PK='xyz'

### Querying Records

***Scan All Records in a Set***

The following is a query that scans all records from a specific namespace and set:

        SELECT * FROM <ns>[.<set>]
        
Where

- \<ns\> is the namespace for the records to be queried.
- \<set\> is the set name for the record to be queried.

Example:

        aql> SELECT * FROM myfirstdb.testset
    

***Project Specific Bins***

The following is the syntax for a query to project bins from all records in a specific namespace and set.

        SELECT <bin>[, <bin>[, ...]] FROM <ns>[.<set>]
        
Where

- \<ns\> is the namespace for the records to be queried.
- \<set\> is the set name for the record to be queried.
- \<bin\> are one or more bins to project from the records.

Example:

        aql> select a, b from myfirstdb.testset

## Example
* Step 0: enter aql and list all the existing namespaces: 

        $ aql
        $ show namespaces
        There will be two default namespaces: 'test' and 'bar'
* Step 1: exit aql and configure a new namespace 'myfirstdb' by editing /etc/aerospike/aerospike.conf
* Step 2: restart Aerospike server through

        $ sudo service aerospike restart
* Step 3: check the status

        $ service aerospike status
* Step 4: enter aql and show all the namespaces again. This time, one new namespace 'myfirstdb' is created
* Step 5: then you can insert, delete or query records within aql on 'myfirstdb'







