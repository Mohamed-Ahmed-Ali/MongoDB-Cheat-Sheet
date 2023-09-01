
<details><summary>Databases Commands</summary><p>
  
# Databases Commands

```bash
show dbs                 # Show Databases
db                       # prints the current database
use <database_name>      # Switch Database
show collections         # Show Collections

```

--------------------------------------------------------------------------------------------------
<p>
</details>

<details><summary>Create and Update</summary><p>

## Create
--------------------------------------------------------------------------------------------------

```bash
db.coll.insertOne({name: "Mohamed"})                                    # InsertOne to insert one document
db.coll.Many([{name: "Mohamed"}, {name:"Ali"}])                         # InsertMany to insert ordered many (bulk) documents
db.coll.insert([{name: "Mohamed"}, {name:"Ali"}], {ordered: false})     # InsertMany to insert unordered many (bulk) documents
```
--------------------------------------------------------------------------------------------------

# Update
```bash

db.coll.update({"_id": 1}, {"year": 2016})                                          # Replaces the entire document with a new document that has only the year field set to 2016.
db.coll.update({"_id": 1}, {$set: {"year": 2016, name: "Mohamed"}})                 # Sets the year field to 2016 and name field to Mohamed.
db.coll.update({"_id": 1}, {$unset: {"year": 1}})                                   # Removes the year field from the document.
db.coll.update({"_id": 1}, {$rename: {"year": "date"} })                            # Renames the year field to date.
db.coll.update({"_id": 1}, {$inc: {"year": 5}})                                     # Increments the year field by 5.
db.coll.update({"_id": 1}, {$mul: {price: NumberDecimal("1.25"), qty: 2}})          # Multiplies the price field by 1.25 and qty field by 2.
db.coll.update({"_id": 1}, {$min: {"imdb": 5}})                                     # Sets the imdb field to 5 if it is less than 5.
db.coll.update({"_id": 1}, {$max: {"imdb": 8}})                                     # Sets the imdb field to 8 if it is greater than 8.
db.coll.update({"_id": 1}, {$currentDate: {"lastModified": true}})                  # Sets the lastModified field to the current date and time.
db.coll.update({"_id": 1}, {$currentDate: {"lastModified": {$type: "timestamp"}}})  # Sets the lastModified field to the current timestamp.
```

## ● Array
```bash

db.coll.update({"_id": 1}, {$push :{"array": 1}})                                   # Adds an element with value of 1 to the end of array field.
db.coll.update({"_id": 1}, {$pull :{"array": 1}})                                   # Removes all elements with value of 1 from array field.
db.coll.update({"_id": 1}, {$addToSet :{"array": 2}})                               # Adds an element with value of 2 to array field if it doesn’t already exist.
db.coll.update({"_id": 1}, {$pop: {"array": 1}})                                    # Removes and returns the last element from array field.
db.coll.update({"_id": 1}, {$pop: {"array": -1}})                                   # Removes and returns the first element from array field.
db.coll.update({"_id": 1}, {$pullAll: {"array" :[3, 4, 5]}})                        # Removes all elements with values of either 3,4 or,5 from array field.
db.coll.update({"_id": 1}, {$push: {scores: {$each: [90, 92, 85]}}})                # Adds multiple elements to scores array using $each modifier.
db.coll.updateOne({"_id": 1, "grades": 80}, {$set: {"grades.$":82}})                # Updates first element in grades array that matches query filter with a new value of $82.
db.coll.updateMany({}, {$inc: {"grades.$[]":10}})                                   # Increments all elements in grades array by 10using[] positional operator.
db.coll.update({}, {$set: {"grades.$[element]":100}}, 
        {multi:true, arrayFilters:[{"element":{"$gte" :100}}]})                     # Updates all documents where grades array contains an element greater than or equal to $100 with a new value of $100.
```
## ● Update many:
```bash

db.coll.update({"year":1999},{$set:{"decade":"90's"}},{"multi" :true})              # Sets decade field to “90’s” for all documents where year is equal to $1999.
db.coll.updateMany({"year" :1999},{$set:{"decade":"90's"}})                         # Same as above.
```

## ● FindOneAndUpdate:
```bash

db.coll.findOneAndUpdate({"name":"Max"},{$inc:{"points" :5}},{returnNewDocument:true})  

# Increments points field by $5 for first document that matches query filter and returns updated document.
```


# Upsert:
```bash

db.coll.update({"_id" :1},{$set:{item:"apple"},$setOnInsert:{defaultQty :100}},{upsert:true})   
# Inserts a new document with _id equal to $1,item equal to “apple”,and defaultQty equal to $100 if no document matches query filter.


```
# Replace

```bash


db.coll.replaceOne({"name": "Max"}, {"firstname": "Maxime", "surname": "Beugnet"})
# Replaces the document where name is Max with a new document that has firstname set to Maxime and surname set to Beugnet.

```
# Write concern
```bash


db.coll.update({}, {$set: {"x": 1}}, {"writeConcern": {"w": "majority", "wtimeout": 5000}})
# Sets x field to 1 for all documents in the collection with write concern set to majority and write timeout set to 5000 milliseconds.
```
--------------------------------------------------------------------------------------------------

<p>
</details>

<details><summary>Read (Find)</summary><p>

# Read

```bash
db.coll.findOne()                                                   # Returns a single document from the collection.
db.coll.find()                                                      # Returns all documents in the collection.
db.coll.find().pretty()                                             # Returns all documents in the collection in a formatted way.
db.coll.find({name: "Mohamed", age: 32})                            # Returns all documents where name is “Max” and age is 32.
db.coll.find({date: ISODate("2020-09-25T13:57:17.180Z")})           # Returns all documents where date is “2020-09-25T13:57:17.180Z”.
db.coll.find({name: "Mohamed", age: 32}).explain("executionStats")  # Returns statistics about the query execution.
db.coll.distinct("name")                                            # Returns an array of distinct values for the name field.
```

# Aggregation Pipeline: The following pipeline stages are used:
```bash
# Aggregation Pipeline
db.coll.aggregate([
  {$match: {status: "A"}},
  {$group: {_id: "$cust_id", total: {$sum: "$amount"}}},
  {$sort: {total: -1}}
  {$out:"aggdata"}
])

$match stage filters out only those documents whose status field equals “A”.
$group stage groups the matching documents by cust_id and calculates the total amount for each cust_id using $sum.
$sort stage sorts the resulting grouped data by total amount in descending order.
$out stage that save the aggregation pipeline output to new collication

```

# Text Search

```bash
db.coll.find({$text: {$search: "cake"}}, {score: {$meta: "textScore"}}).sort({score: {$meta: "textScore"}})

# Text search with a “text” index: Returns all documents that contain the word “cake” using a text index on the collection.

```

# Read Concern:

```bash

db.coll.find().readConcern("majority") # Returns all documents with read concern set to majority.

db.coll.find({}, { readConcern: { level: "majority" } }) 

```

--------------------------------------------------------------------------------------------------

<p>
</details>

<details><summary>Operators </summary><p>


## ● Count:
```bash


db.coll.count({age: 32})                                            # Returns an estimation of the number of documents where age is 32 based on collection metadata.
db.coll.estimatedDocumentCount()                                    # Returns an estimation of the number of documents in the collection based on collection metadata.
db.coll.countDocuments({age: 32})                                   # Returns an accurate count of the number of documents where age is 32 using an aggregation pipeline.


```
## ● Comparison:

```bash

db.coll.find({"year": {$gt: 1970}})                                 # Find where year is greater than 1970.
db.coll.find({"year": {$gte: 1970}})                                # Find where year is greater than or equal to 1970.
db.coll.find({"year": {$lt: 1970}})                                 # Find where year is less than 1970.
db.coll.find({"year": {$lte: 1970}})                                # Find where year is less than or equal to 1970.
db.coll.find({"year": {$ne: 1970}})                                 # Find where year is not equal to 1970.
db.coll.find({"year": {$in: [1958, 1959]}})                         # Find where year is either 1958 or 1959.
db.coll.find({"year": {$nin: [1958, 1959]}})                        # Find where year is neither 1958 nor 1959.

```

## ● Logical:
```bash

db.coll.find({name:{$not: {$eq: "Mohamed"}}})                       # Find where name is not “Mohamed”.
db.coll.find({$or: [{"year" : 1958}, {"year" : 1959}]})             # Find where year is either 1958 or 1959.
db.coll.find({$nor: [{price: 1.99}, {sale: true}]})                 # Find where price is not equal to $1.99 and sale is not true.
db.coll.find({$and: [{$or: [{qty: {$lt :10}}, {qty :{$gt: 50}}]},
                     {$or: [{sale: true}, {price: {$lt: 5 }}]}]})   # Find that satisfy both conditions.

```
## ● Element

```bash
db.coll.find({name: {$exists: true}})                             # Returns all documents where name exists.
db.coll.find({"zipCode": {$type: 2 }})                            # Returns all documents where zipCode is a string.
db.coll.find({"zipCode": {$type: "string"}})                      # Same as above.
```

## ● Regex

```bash

db.coll.find({name: /^Max/})      # Returns all documents where name starts with letter “M” (case sensitive).
db.coll.find({name: /^Max$/i})    # Returns all documents where name equals exactly to “Max” (case insensitive).

```

## ● Array:

```bash

db.coll.find({tags: {$all: ["Realm", "Charts"]}})   # Returns all documents where tags array contains both “Realm” and “Charts”.
db.coll.find({field: {$size: 2}})                   # Returns all documents where field array has a size of 2.

```
## ● Element:

```bash

db.coll.find({results: {$elemMatch: {product: "xyz", score: {$gte: 8}}}}) 

# Returns all documents where results array contains at least one element that has product equal to “xyz” and score greater than or equal to 8.
```

## ● Projections:
```bash
db.coll.find({"x": 1}, {"actors": 1})                # Returns all documents where x is 1 and includes actors and _id fields.
db.coll.find({"x": 1}, {"actors": 1, "_id": 0})      # Returns all documents where x is 1 and includes actors field but excludes _id field.
db.coll.find({"x": 1}, {"actors": 0, "summary": 0})  # Returns all documents where x is 1 and excludes actors and summary fields.
```

## ● Sort, skip, limit:

```bash

db.coll.find({}).sort({"year": 1, "rating": -1}).skip(10).limit(3) 

# Returns three documents sorted by year in ascending order and rating in descending order, skipping the first ten documents.
```

<p>
</details>

<details><summary>Save & Delete</summary><p>

# Save

```bash


db.coll.save({"item": "book", "qty": 40})
# Inserts a new document with item equal to book and qty equal to 40. If the document already exists, it will be replaced.


```


## Delete
```bash

db.coll.remove({name: "Max"})                       # Removes all documents where name is Max.
db.coll.remove({name: "Max"}, {justOne: true})      # Removes only one document where name is Max.
db.coll.remove({})                                  # Removes all documents in the collection. WARNING! This does not delete the collection itself and its index definitions.
db.coll.remove({name: "Max"}, {"writeConcern": {"w": "majority", "wtimeout": 5000}})
# Removes all documents where name is Max with write concern set to majority and write timeout set to 5000 milliseconds.
db.coll.findOneAndDelete({"name": "Max"})           # Finds a document where name is Max, removes it, and returns the removed document.
```

--------------------------------------------------------------------------------------------------

</p>

</details>

<details><summary>Databases and Collections
</summary><p>

-
# Create Collection



```bash

// Create collection with a $jsonschema
db.createCollection("contacts", {
   validator: {$jsonSchema: {
      bsonType: "object",
      required: ["phone"],
      properties: {
         phone: {
            bsonType: "string",
            description: "must be a string and is required"
         },
         email: {
            bsonType: "string",
            pattern: "@mongodb\.com$",
            description: "must be a string and match the regular expression pattern"
         },
         status: {
            enum: [ "Unknown", "Incomplete" ],
            description: "can only be one of the enum values"
         }
      }
   }}
})


The db.createCollection() command creates a new collection with a $jsonschema. The $jsonschema specifies the structure of the documents that can be inserted into the collection. In this case, the contacts collection must have a phone field that is a string and is required. It can also have an optional email field that must match the regular expression pattern @mongodb\.com$. The status field can only have one of the enum values: “Unknown” or “Incomplete”.

```
--------------------------------------------------------------------------------------------------
## Drop


```bash
The db.coll.drop() 
# command removes the collection and its index definitions. The db.dropDatabase() command removes the entire database. Please be careful while using these commands.
db.coll.stats()                             # Returns statistics about the collection.
db.coll.storageSize()                       # Returns the total size of the data in the collection.
db.coll.totalIndexSize()                    # Returns the total size of all indexes created on the collection.
db.coll.totalSize()                         # Returns the total size of the collection, including all indexes.
db.coll.validate({full: true})              # Validates the collection, including indexes and data, and returns a report of any errors.
db.coll.renameCollection("new_coll", true)  # Renames the collection to “new_coll”. The second parameter is set to true to drop the target collection if it exists.
```
--------------------------------------------------------------------------------------------------
</p>
</details>

<details><summary>Indexes</summary><p>

## List Indexes

```bash
db.coll.getIndexes()    # Returns an array that contains a document for each index in the collection.
db.coll.getIndexKeys()  # Returns an array that contains the names of all indexes in the collection.
```
--------------------------------------------------------------------------------------------------


## Create Indexes


```bash
db.coll.createIndex({"name": 1})                    # Creates a single field index on the name field.
db.coll.createIndex({"name": 1, "date": 1})         # Creates a compound index on the name and date fields.
db.coll.createIndex({foo: "text", bar: "text"})     # Creates a text index on the foo and bar fields.
db.coll.createIndex({"$**": "text"})                # Creates a wildcard text index on all fields in the collection.
db.coll.createIndex({"userMetadata.$**": 1})        # Creates a wildcard index on all fields in the userMetadata subdocument.
db.coll.createIndex({"loc": "2d"})                  # Creates a 2d index on the loc field.
db.coll.createIndex({"loc": "2dsphere"})            # Creates a 2dsphere index on the loc field.
db.coll.createIndex({"_id": "hashed"})              # Creates a hashed index on the _id field.

```

# Index Options:

```bash



db.coll.createIndex({"lastModifiedDate": 1}, {expireAfterSeconds: 3600})        # Creates a TTL (time-to-live) index on lastModifiedDate field that automatically removes documents after 3600 seconds (1 hour).
db.coll.createIndex({"name": 1}, {unique: true})                                # Creates a unique index on the name field.
db.coll.createIndex({"name": 1}, {partialFilterExpression: {age: {$gt: 18}}})   # Creates a partial index that only indexes documents where age is greater than 18.
db.coll.createIndex({"name": 1}, {collation: {locale: 'en', strength: 1}})      # Creates a case-insensitive index with strength equal to either 1 or 2. Strength of 1 is less strict than strength of 2.
db.coll.createIndex({"name": 1 }, {sparse: true})                               # Creates a sparse index on the name field.

```
--------------------------------------------------------------------------------------------------
## Drop Indexes

```bash
db.coll.dropIndex("name_1")        # Drops the index named “name_1” from the collection.
```
--------------------------------------------------------------------------------------------------
## Hide/Unhide Indexes
 

```bash
db.coll.hideIndex("name_1")     # Hides the index named “name_1” from the collection statistics.
db.coll.unhideIndex("name_1")   # Unhides the index named “name_1” in the collection statistics. 

```
--------------------------------------------------------------------------------------------------

</p>
</details>

<details><summary>Handy commands
</summary><p>

```bash

use admin                                                                         # Switches to the admin database.
db.createUser({"user": "root", "pwd": passwordPrompt(), "roles": ["root"]})       # Creates a new user with root role.
db.dropUser("root")                                                               # Deletes the user named root.
db.auth( "user", passwordPrompt() )                                               # Authenticates the user named user with a password prompt.
use test                                                                          # Switches to the test database.
db.getSiblingDB("dbname")                                                         # Returns a reference to the dbname database.
db.currentOp()                                                                    # Returns a document that contains information on in-progress operations for the database.
db.killOp(123)                                                                    # Kills the operation with opid equal to 123.
db.fsyncLock()                                                                    # Flushes all pending writes to disk and locks the database until fsyncUnlock() is called.
db.fsyncUnlock()                                                                  # Unlocks the database previously locked by fsyncLock().
db.getCollectionNames()                                                           # Returns an array of collection names in the current database.
db.getCollectionInfos()                                                           # Returns an array of documents that contain information on all collections in the current database.
db.printCollectionStats()                                                         # Prints statistics for all collections in the current database.
db.stats()                                                                        # Returns statistics for the current database.
db.getReplicationInfo()                                                           # Returns information on replication for the current database.
db.printReplicationInfo()                                                         # Prints information on replication for the current database.
db.isMaster()                                                                     # Returns information on whether this instance is a master or slave in a replica set.
db.hostInfo()                                                                     # Returns information on the host running this instance of MongoDB.
db.printShardingStatus()                                                          # Prints information on sharding for this instance of MongoDB.
db.shutdownServer()                                                               # Shuts down this instance of MongoDB cleanly and safely.
db.serverStatus()                                                                 # Returns statistics and other status information for this instance of MongoDB.
db.setSlaveOk()                                                                   # Allows read operations on a secondary node in a replica set.
db.getSlaveOk()                                                                   # Returns whether read operations are allowed on secondary nodes in a replica set.
db.getProfilingLevel()                                                            # Returns the current profiling level for this instance of MongoDB.
db.getProfilingStatus()                                                           # Returns whether profiling is enabled and if so, what level it is set to.
db.setProfilingLevel(1, 200)                                                      # Enables profiling at level 1 with a slowms value of 200 milliseconds.
db.enableFreeMonitoring()                                                         # Enables free monitoring for this instance of MongoDB.
db.disableFreeMonitoring()                                                        # Disables free monitoring for this instance of MongoDB.
db.getFreeMonitoringStatus()                                                      # Returns whether free monitoring is enabled or disabled.
db.createView("viewName", "sourceColl", [{$project:{department: 1}}])             # Creates a view named viewName based on sourceColl collection with department field projected.

```
--------------------------------------------------------------------------------------------------
</p>
</details>  
