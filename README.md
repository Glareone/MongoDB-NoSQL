# Introduction 
This is a repo created for storing any useful information about MongoDB.

<details>
<summary>Section - 1: Introduction</summary>

* MongoDB Data structure:  

![mongoDB](Section-1/intro-structure.jpg)

* MongoDB data format (document-oriented storage format):

![data-format](Section-1/2-data-format.jpg)

* BSON data-format and what is under the hood:

![BSON](Section-1/3-no-schema.jpg)

* MongoDB Ecosystem:

![Ecosystem](Section-1/4-ecosystem.jpg)

* Work with MongoDB:

![mongodb](Section-1/9-work-with-mongo.jpg)
![mongodb2](Section-1/10-inside.jpg)

* Implicit operations in Mongo:

![console output](Section-2/1-implicit.jpg)

## Start working with MongoDB

To add mongo command to your command line:  
<b> win - environment variables - advanced tab - environment variables</b>  
Add here a path to your mongoDB.
![image how to do that](Section-1/5-cmd-configuration.jpg)

[useful link](https://dangphongvanthanh.wordpress.com/2017/06/12/add-mongos-bin-folder-to-the-path-environment-variable/)

<b>BTW</b>, to continue working with course you have to stop MongoDB service and start db manually
using "mongo" command from console. Without it "mongo" command will open mongo service instead of real db.

to stop service - open CMD as admin and `net stop Mongo`

<b>Last step:</b>  
* To make default data storage location: create "data" folder in C: drive and put folder "db" within.
* Otherwise: create another directory (i.e. D:\mongodb-data\db) and put command in cmd:
`mongod --dbpath D:\mongodb-data\db`

<b>Pay attention:</b>
You have to leave your process running (cmd console should be opened) to work with mongoDB service.

* CMD - `mongo`

And now you are in the mongo shell where you can run your commands and queries.
</details>

<details>
<summary>Section - 1: Simple commands</summary>

* `show dbs` - will show existing dbs in selected repository (`--dbpath D:\mongodb-data\db`)
* `use Your_db_name` - will switch to db with selected name. If db does not exist - it will create it automatically.
* `db.products.insertOne({name: "A Book", price: 29.99})` - will create a table named products (it does not exist too) 
in db which we connected to and insert a document inside it.  
Pay attention on non-existing quotation mark in "keys" - you can use key naming without quotations, they will be added
under the hood.  

Here is a console output. InsertedId - generated uniqueId for this insert, acknowledged - verified that this data was inserted.
![console output](Section-1/7-console-output-after-insert.jpg)

* `db.products.find()` - retrieves you all data from collection (from table in SQL world).
* `db.products.find().pretty()` - show this data formatted.
![pretty](Section-1/8-find-pretty.jpg)
</details>

<details>
<summary>Section - 2: JSON-vs-BSON, Basics & CRUD operations</summary>

### Summary
![summary](Section-2/24-Summary.jpg)

### Json vs Bson:
![json-vs-bson](Section-2/2-json-vs-bson.jpg)

* You can set _id field manually and do not rely on autogenerated id.
BTW, you can't insert another document with the same _id.

![id](Section-2/3-_id-field.jpg)

## CRUD Operations:

![crud](Section-2/4-crud-1.jpg)
![crud](Section-2/5-crud-2.jpg)

### Read 
Simple filter: `db.flight.find({intercontinental: true}).pretty()`;
Greater than ($gt): `db.flight.find({distance: {$gt: 10000}}).pretty()`;
FindOne: `db.flight.findOne({distance: {$gt: 10000}})`

### InsertMany and Show results using find. Cursor  
Find does not show you all results, it shows you a cursor by default:  
![cursor](Section-2/11-insert-many.jpg)
![cursor](Section-2/12-find-cursor.jpg)

* Bare in mind that mongodb will increment Id to keep the proper element's order. First came element will contain minor identifier:

![crud](Section-2/6-insert-many.jpg)

### UpdateOne
`db.flight.updateOne({distance: 12000}, {$set: {marker: "new field delete"}})` - will update first document which contains distance: 12000.  
Pay attention on <b>$set</b> - all reserved words start from dollar sign. 
This operator means that you would like to update your document with new field.

### UpdateMany
`db.flight.updateMany({}, {$set: {marker: "to Delete!"}})` - empty curly braces `{}` mean all documents in collection.

### Update
`update` operation works like `updateMany`:  
![crud](Section-2/7-update.jpg)

As you may have noticed first modification using set to `delayed: true` has no modified results because our document already
has this value. When we changed the value to false - log shows us that our value has been changed.

The difference between them - you can use it without `$set` operator, update does accept this syntax.
But it works on another manner:
![crud](Section-2/8-update-2.jpg)

It will override all key-value pairs in document!
![crud](Section-2/9-update-3.jpg)

It works very close to `replaceOne`:
![crud](Section-2/10-replaceone.jpg)

### Delete
`db.flight.deleteOne({departureAirport: "TXL"})` - departureAirport: "TXL" will be used as filter to find what exactly
 we want to delete from collection. Only first found document with "TXL" will be deleted.

### forEach
It is possible to use .forEach operation after find() to do something with every element after filtering:
`db.passengers.find().forEach((passengerData) => {printjson(passengerData)})` - bare in mind forEach uses syntax according
your MongoDB driver. Shell uses Nodejs syntax.

Pay attention:
That's why you cant use `pretty()` after findOne() method - `pretty()` is a method of a Cursor, findOne does not return cursor,
(and `pretty()` does not exist for a single value), findOne returns a sole value.

</details>

<details>
<summary>Section - 2: Projection</summary>

### Projection
Projection means a mechanism to avoid overfetching from database.  
You can use it as a parameter in `find` method:
* `find({}, {name: 1})` - first parameter is a filter, the second is projection. 1 means - "include this data".

![projection](Section-2/13-projection-overfetching.jpg)
![projection](Section-2/14-projection-2.jpg)

By default it will send you objects with _id (because it is a default property) and "name".

* To exclude _id (or any other field) - `find({}, {_id: 0})` - 0 means exclude.
![projection](Section-2/15-projection-3.jpg)

To only name - `find({}, {name: 1, _id: 0})`/

</details>

<details>
<summary>Section - 2: Embedded Documents & Arrays</summary>

![embedded](Section-2/16-Embedded-doc.jpg)
![embedded](Section-2/17-Embedded-array.jpg)

## Array examples:
![arrays](Section-2/18-embedded-doc-example.jpg)
![arrays](Section-2/19-embedded-doc-example-2.jpg)
  
## Simple arrays with find method
![arrays](Section-2/20-arrays-of-string.jpg)
![arrays](Section-2/21-arrays-filter-by.jpg)

## Array of objects with find method
to use find in embedded document you have to use ".":
`find({"status.description": "your_value"})`

description is an embedded document inside status.  
Pay attention that you must use double quotation around `status.description`.

![arrays](Section-2/22-arrays-filter-by.jpg)
![arrays](Section-2/23-arrays-filter-by.jpg)

</details>

<details>
<summary>Section - 3: Schema Basis</summary>

![schema](Section-3/1-schema.jpg)
![schema](Section-3/2-schemaless-to-sqlworld.jpg)
![schema](Section-3/3-schemaless-to-sqlworld-2.jpg)

* SQL Approach (the same structure for all documents):  
You can assign null to your property. The value of such property will not be assign, but the property will be shown
in your data structure.
`db.products.insertOne({name: "Book", details: null})`

</details>

<details>
<summary>Section - 3: Data Schemas, Data Modelling</summary>

![data-modelling](Section-3/6-data-modelling.jpg)
</details>

<details>
<summary>Section - 3: Data Types, Limits and Statistic</summary>

[good link about how mongodb works inside](https://www.datadoghq.com/blog/monitoring-mongodb-performance-metrics-mmap/)

* Data Types:
![types](Section-3/4-Value-types.jpg)

* to get statistic from your database you have to use `stats()` command;
![stats](Section-3/5-stats.jpg)
To prove that it stores a number instead of float you can use `typeof db.numbers.findOne().a` command.

* MongoDB has a couple of hard limits - most importantly, a single document in a collection (including all embedded documents it might have) must be <= 16mb. Additionally, you may only have 100 levels of embedded documents.
[additional-info](https://docs.mongodb.com/manual/reference/bson-types/)

1. NumberDecimal creates a high-precision double value => NumberDecimal("12.99")
2. NumberInt creates a int32 value => NumberInt(55)
3. NumberLong creates a int64 value => NumberLong(7489729384792)

</details>

<details>
<summary>Section - 3: Relations</summary>

##One to One Relations
![relations](Section-3/7-relations-1.jpg)
![onetoone](Section-3/one-to-one/8-relations-one-to-one-1.jpg)
![onetoone](Section-3/one-to-one/9-relations-one-to-one-2.jpg)

* Example with one-to-one relations and call the data using two steps and variable:

![onetoone](Section-3/one-to-one/10-relations-one-to-one-3.jpg)

It's not the best option of storing data. In such case better to store data like embedded data inside patient document.
In most cases better to use embedded approach. 

* Another one-to-one examples, but using references. You still opt to use different collections: 
It could be possible useful if you try to analyze your data. And it's very good if your data stores in different
collections (for load balancing, for example. Or because we are interesting only in cars).

![onetoone](Section-3/one-to-one/11-relations-one-to-one-reference-4.jpg)
![onetoone](Section-3/one-to-one/12-relations-one-to-one-reference-5.jpg)

##One to Many Relations
![onetomany](Section-3/one-to-many/1-one-to-many-schema-1.jpg)
* And brief example of ref and embedded approaches:

![onetomany](Section-3/one-to-many/2-one-to-many-approaches.jpg)
* Additional example:

![onetomany](Section-3/one-to-many/3-additional-example.jpg)

##Many to Many Relations
![manytomany](Section-3/many-to-many/1-collection-relations.jpg)
* Sql World approach with 3 tables, one of them stores a joint data:

![manytomany](Section-3/many-to-many/2-sql-world-approach.jpg)

* MongoDB Approach:

![manytomany](Section-3/many-to-many/3-mongo-db-approach.jpg)

It allows us to use references within one of the data tables.
Advantages from sql and mongo worlds.
Also no reason to use fully embedded approach for some reasons (over-fetching, possible not up-to-date data and so on).

</details>