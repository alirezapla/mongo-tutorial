# mongodb-tutorial

# Table of Contents  

- [Intro](#intro)  
- [Mongodb drivers](#mongodb-drivers)  
- [Basics](#basics)  
- [Relationships](#relationships)  
- [CRUD](#crud)  
  * [Find](#find)
  * [Insert](#insert)
  * [Update](#update)
  * [Remove](#remove)







## Intro

## Mongodb drivers

- [PyMongo](https://www.mongodb.com/docs/drivers/pymongo/) for synchronous Python applications.
- [Motor](https://www.mongodb.com/docs/drivers/motor/) for asynchronous Python applications.

****

### ODM

- [mongoengine](https://docs.mongoengine.org/) 

[more info](https://pymongo.readthedocs.io/en/stable/tools.html)
****

## Basics

An ObjectId is a 12-byte BSON 

- The first 4 bytes representing the seconds since the unix epoch
- The next 3 bytes are the machine identifier
- The next 2 bytes consists of process id
- The last 3 bytes are a random counter value

In MongoDB, a database is not created until it gets content, so if this is your first time creating a database,(create collection and create document) before you check if the database exists!

A collection in MongoDB is the same as a table in SQL databases.

A document in MongoDB is the same as a record in SQL databases.

In MongoDB, projection means selecting only the necessary data rather than selecting whole of the data of a document. If a document has 5 fields and you need to show only 3, then select only 3 fields from them.

To limit the records in MongoDB, you need to use limit() method. The method accepts one number type argument, which is the number of documents that you want to be displayed.

Apart from limit() method, there is one more method skip() which also accepts number type argument and is used to skip the number of documents.

To sort documents in MongoDB, you need to use sort() method. The method accepts a document containing a list of fields along with their sorting order. To specify sorting order 1 and -1 are used. 1 is used for ascending order while -1 is used for descending order.


| Name | Description  | Example| RDBMS Equivalent|
| :---: | :---: |:---: |:---: |
|$eq |Matches values that are equal to a specified value.| db.mycol.find({"by":"tutorial"}) | where by = 'tutorial'|
|$gt |	Matches values that are greater than a specified value. | db.mycol.find({"likes":{$gt:50}})|	where likes > 50|
|$gte	| Matches values that are greater than or equal to a specified value.  | db.mycol.find({"likes":{$gte:50}})|	where likes >= 50|
|$in | Matches any of the values specified in an array.|db.mycol.find({"name":{$in:["Raj", "Ram", "Raghu"]}}) | Where name matches any of the value in :["Raj", "Ram", "Raghu"]|
|$lt | Matches values that are less than a specified value.|db.mycol.find({"likes":{$lt:50}}).pretty()	|where likes < 50|
|$lte | Matches values that are less than or equal to a specified value.| db.mycol.find({"likes":{$lte:50}})|	where likes <= 50|
|$ne	| Matches all values that are not equal to a specified value.| db.mycol.find({"likes":{$ne:50}}) | where likes != 50|
|$nin | Matches none of the values specified in an array.|db.mycol.find({"name":{$nin:["Ramu", "Raghav"]}})|Where name values is not in the array :["Ramu", "Raghav"] or, doesn’t exist at all|


#### Aggregation

|Expression |	Description |	Example |
|:---: | :---: |:---: |
|$sum	|Sums up the defined value from all documents in the collection.|	db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : "$likes"}}}])|
|$avg|	Calculates the average of all given values from all documents in the collection.|db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$avg : "$likes"}}}])
|$min|	Gets the minimum of the corresponding values from all documents in the collection.|db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$min : "$likes"}}}])|
|$max|	Gets the maximum of the corresponding values from all documents in the collection.|db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$max : "$likes"}}}])|
|$push|	Inserts the value to an array in the resulting document.|db.mycol.aggregate([{$group : {_id : "$by_user", url : {$push: "$url"}}}])
|$addToSet|	Inserts the value to an array in the resulting document but does not create duplicates.|	db.mycol.aggregate([{$group : {_id : "$by_user", url : {$addToSet : "$url"}}}])|
|$first|	Gets the first document from the source documents according to the grouping. Typically this makes only sense together with some previously applied “$sort”-stage.|db.mycol.aggregate([{$group : {_id : "$by_user", first_url : {$first : "$url"}}}])|
|$last|	Gets the last document from the source documents according to the grouping. Typically this makes only sense together with some previously applied “$sort”-stage.|db.mycol.aggregate([{$group : {_id : "$by_user", last_url : {$last : "$url"}}}])|



````bash

db # shows default database

use <db> # switche to <db>

show dbs

db.dropDatabase() #delete the selected database. If not selected any database, delete default

show collections

db.createCollection("mycollection")

db.mycollection.drop()

db.mycollection.createIndex({KEY:1}) # Here key is the name of the field on which you want to create index 
				     # and 1 is for ascending order. 
                                     # To create index in descending order you need to use -1.

db.mycollection.dropIndex({KEY:1})

db.mycollection.dropIndexs({KEY:1})

db.mycollection.getIndexes()



````

## Relationships

Relationships in MongoDB represent how various documents are logically related to each other. Relationships can be modeled via Embedded and Referenced approaches. Such relationships can be either 1:1, 1:N, N:1 or N:N.

Let us consider the case of storing addresses for users. So, one user can have multiple addresses making this a 1:N relationship.

In the embedded approach, we will embed the address document inside the user document.

````bash

{
	"_id":ObjectId("52ffc33cd85242f436000001"),
	"contact": "987654321",
	"dob": "01-01-1991",
	"name": "Tom Benzamin",
	"address": [
		{
			"building": "22 A, Indiana Apt",
			"pincode": 123456,
			"city": "Los Angeles",
			"state": "California"
		},
		{
			"building": "170 A, Acropolis Apt",
			"pincode": 456789,
			"city": "Chicago",
			"state": "Illinois"
		}
	]
}
````


referenced relationships designing normalized relationship. In this approach, both the user and address documents will be maintained separately but the user document will contain a field that will reference the address document's id field

````bash
{
	   "_id":ObjectId("52ffc33cd85242f436000001"),
	   "contact": "987654321",
	   "dob": "01-01-1991",
	   "name": "Tom Benzamin",
	   "address_ids": [
	      ObjectId("52ffc4a5d85242602e000000"),
	      ObjectId("52ffc4a5d85242602e000001")
	   ]
}
````
****
## CRUD

- ### Insert

	````javascript
		db.collection.insertMany(<documents>);
	````
	````javascript
		db.collection.insertOne(<document>);

	````

-  ### Find

	The `pretty()` method displays the results in a formatted way.


	````javascript
		db.collection.find();
	````
	````javascript
		db.collection.find().pretty()
	````
	````javascript
		db.collection.findOne({<field>: <value>})
	````
	````javascript
		db.collection.find({ $or: [{<key1>: <value1>}, {<key2>:<value2>}]})
	````
	````javascript
		db.collection.find({ $and: [ {<key1>:<value1>}, { <key2>:<value2>} ] })
	````

	#### example: AND and OR Together

	````javascript
		db.collection.find({"likes": {$gt:10}, $or: [{"by": "whoami"},{"title": "MongoDB"}]}).pretty()
	````
	````bash
		{
		   "_id": ObjectId(7df78ad8902c),
		   "title": "MongoDB", 
		   "description": "MongoDB is no sql",
		   "by": "whoami",
		   "url": "http://www.web.com",
		   "tags": ["mongodb", "NoSQL"],
		   "likes": "20"
		}
	````
	````javascript
		db.collection.find({ $not: [{<key1>: <value1>}, {<key2>:<value2>}] })
	````
	example 
	````javascript
		db.collection.find({},{"title":1,_id:0})
	````
	````bash
		{"title":"MongoDB Overview"}
		{"title":"NoSQL Overview"}
		{"title":"Tutorials Point Overview"}
	````



- ### Update

	````javascript
		db.collection.update(SELECTION_CRITERIA, UPDATED_DATA)
	````
	By default, MongoDB will update only a single document. To update multiple documents, you need to set a parameter 'multi' to true.

	````javascript
		db.collection.update({<key>:<value>},{$set:{<key>:<value>}},{multi:true})
	````
	````javascript
		db.collection.save({_id:ObjectId(),{ <key> : <value> })
	````

	````javascript
		db.collection.findOneAndUpdate(SELECTIOIN_CRITERIA, UPDATED_DATA)
	````
	example
	````javascript
		db.collection.findOneAndUpdate({First_Name: 'root'},{ $set: { Age: '30',e_mail: 'whoami@email.com'}})
	````
	````bash
		{
			"_id" : ObjectId("5dd6636870fb13eec3963bf5"),
			"First_Name" : "root",
			"Last_Name" : "root",
			"Age" : "30",
			"e_mail" : "whoami@email.com",
			"phone" : "9000012345"
		}
	````
	```javascript
		db.collection.updateOne(<filter>, <update>)
	````

	```javascript
		db.collection.updateMany(<filter>, <update>)
	````

	example
	````javascript
		db.collection.updateMany({Age:{ $gt: "25" }},{ $set: { Age: '00'}})
	````

- ### Remove

	````javascript
		db.collection.remove(DELLETION_CRITTERIA) # remove all filterd items
	````
	````javascript
		db.collection.remove(DELLETION_CRITTERIA,1) # removel first record
	````
	````javascript
		db.collection.remove({}) # remove all
	````


