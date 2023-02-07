# mongo-tutorial

## mongodb drivers

- [PyMongo](https://www.mongodb.com/docs/drivers/pymongo/) for synchronous Python applications.
- [Motor](https://www.mongodb.com/docs/drivers/motor/) for asynchronous Python applications.

****

## ODM

- [mongoengine](https://docs.mongoengine.org/) 

[more info](https://pymongo.readthedocs.io/en/stable/tools.html)
****

## basics

In MongoDB, a database is not created until it gets content, so if this is your first time creating a database,(create collection and create document) before you check if the database exists!

A collection in MongoDB is the same as a table in SQL databases.

A document in MongoDB is the same as a record in SQL databases.

In MongoDB, projection means selecting only the necessary data rather than selecting whole of the data of a document. If a document has 5 fields and you need to show only 3, then select only 3 fields from them.

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



````bash

db # shows default database
use <db> # switche to <db>
show dbs
db.dropDatabase() #delete the selected database. If not selected any database, delete default
show collections
db.createCollection(<mycollection>)
db.mycollection.drop()

````


## commands

- Insert

````javascript
db.collection.insertMany(<documents>);
````
````javascript
db.collection.insertOne(<document>);

````

- Find

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


- Filter

````javascript
db.collection.find({<field>: <value>});
````

- Update

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

- Remove

````javascript
db.collection.remove(DELLETION_CRITTERIA) # remove all filterd items
````
````javascript
db.collection.remove(DELLETION_CRITTERIA,1) # removel first record
````
````javascript
db.collection.remove({}) # remove all
````


