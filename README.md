-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-
` ` : Is Just A Placeholder Do Not Type It.
->  : Is Mean After Or Then.


-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-

//! - OverView --------------------------------------------------------------------:
 - Document : One Insertion Row.
 - Collection : Table(Group Of Documents).
 - Database : Database(Group Of Collections).

//! - Basics  ---------------------------------------------------------------------:
    1 - Establish A Connection : mongosh
    2 - Exit A Connection : exit
    3 - Show Databases : show dbs
    4 - Use Database Or Create New One If Not Exist : use `dbName`
    5 - Drop A Database :  use `dbName`  ->  db.dropDatabase()
    6 - Create A Collection(Or Table) : db.createCollection("nameOfCollection")
	7 - Show Collections inside a Database : use `dbName`  -> show collections
	8 - Drop A Collection : use `dbName`  -> db.collectionName.drop()
    9 - Show Documents(Rows) inside a Collection :  db.collectionName.find()
	10- Count Of Documents Inside a Collection : db.collectionName.find().count()

//! - Insert ---------------------------------------------------------------------:
    1 - Insert One :
        use `dbName`  ->  db.collectionName.insertOne({name:"Oussama",age:21,registerDate:new Date("2023-12-24T16:10:00")}) //If Date is Empty It Will Get The Current Time
    2 - Insert Many :
        use `dbName`  ->  db.collectionName.insertMany([
            {name:"Oussama",completed:true,projects:["p1","p2","p3"]},
            {name:"Ahmed",completed:false,projects:["p1"],languages:null},
            {name:"Ali",completed:True,projects:["p1","p2","p3"],languages:{primary:"Arabic",secondary:"English"}},
        ])

//! Sort -------------------------------------------------------------------------:
    1 [A->Z || 1->9] : db.collectionName.find().sort({fieldName:1})
    2 [Z->A || 9->1] : db.collectionName.find().sort({fieldName:-1})

//! Limit ------------------------------------------------------------------------:
    db.collectionName.find().limit(1) // This Will Show Only The First Document

//! Max - Min  -------------------------------------------------------------------:
    1- Max : db.collectionName.find().sort({fieldName:-1}).limit(1)
    2- Min : db.collectionName.find().sort({fieldName:1}).limit(1)

//! Find (Advanced 1) ------------------------------------------------------------:
    1- Where key=`value` : 
        db.collectionName.find({name:"oussama"}) // Return Only Documents With name of "oussama"
        db.collectionName.find({name:"oussama",age:21,languages:!null}) // Return Only Documents With name of "oussama" ,age of 21 and has at least one language
    2- Show Specific Fields (Projection) :
        db.collectionName.find({},{_id:false,name:true}) // Shows Only Names

//! Update(one)  ----------------------------------------------------------------:
	1 - Syntax :
		db.collectionName.updateOne({creteria},{$set:{field:"value"}})
	2 - Usage :
		db.collectionName.updateOne({name:"Oussama"},{$set:{completed:true}}) //By Name 
		db.collectionName.updateOne({_id:ObjectId("68c47e474d7")},{$set:{completed:true}}) //By Id
	3 - Unset Keyword :
		db.collectionName.updateOne({_id:ObjectId("68c47e474d7")},{$unset:{completed:""}}) //this will remove `completed` field

//! Update(many)  ----------------------------------------------------------------:
	1 - Syntax :
		db.collectionName.updateMany({creteria},{$set:{field:"value"}})
	2 - Usage :
		db.collectionName.updateMany({},{$set:{completed:true}}) //change all Collection `completed` field values
		db.collectionName.updateMany({completed:{$exist:false}},{$set:{completed:true}}) //add `completed` field to documents that not have it
		db.collectionName.updateMany({languages:!null},{$set:{completed:true}}) //change all documents `completed` field to true that have at least on language  
		
//! Delete  ----------------------------------------------------------------------:
	1 - Syntax :
		db.collectionName.deleteOne({creteria})
		db.collectionName.deleteMany({creteria})
	2 - Usage :
		db.collectionName.deleteOne({_id:ObjectId("68c47e474d7")}) // Remove The document with Specific Id
		db.collectionName.deleteMany({languages:null}) // Remove documents with no languages
		
//! Matematical Operators  -------------------------------------------------------:
	- exist : db.collectionName.find({completed:{$exist:true}}) // documents have  `completed` field
	- ne    : db.collectionName.find({name:{$ne:"Oussama"}}) // documents not have  `name` field equal to "Oussama"
	- lt    : db.collectionName.find({age:{$lt:20}}) // documents have  `age` field less than 20
	- lte   : db.collectionName.find({age:{$lte:20}}) // documents have  `age` field less than or equal to 20
	- gt    : db.collectionName.find({age:{$gt:20}}) // documents have  `age` field greater than 20
	- gte   : db.collectionName.find({age:{$gte:20}}) // documents have  `age` field greater than or equal to 20
		//Combine More Than One  Operator:
			db.collectionName.find({age:{$gte:20,$lte:40}}) //Age Between 20 and 40
	- in    : db.collectionName.find({name:{$in:["Oussama","Ahmed","Mouaade"]}}) // documents have `name` value present in the list
	- nin   : db.collectionName.find({name:{$nin:["Oussama","Ahmed","Mouaade"]}}) // documents have `name` value no present in the list
	
//! Logical  Operators  ---------------------------------------------------------:
	- and : db.collectionName.find({$and: [{completed:true},{age:{$gte:20}},{languages:!null}]})
		// Will Return Documents that completed and age bigger or equal to 20 and have at least on language.
	- or  : db.collectionName.find({$or: [{completed:true},{age:{$gte:20}},{languages:!null}]})
		// Will Return Documents that completed or age bigger or equal to 20 or have at least on language.
	- nor : db.collectionName.find({$nor: [{completed:true},{age:{$gte:20}},{languages:!null}]})
		// Will Return Documents that not completed and age less or equal to 20 and have no language.
	- not : db.collectionName.find({age:{$not:{$gte:20}}})
		// Will Return Documents that have age less then or equal to 20 .
	
//! Distinct(Unrepititive values)  ---------------------------------------------:
	- db.collectionName.distinct("age")

//! Wild Cards(Like)  ---------------------------------------------:
	- Contains :
		db.collectionName.find({name:/us/})
	- Starts With :
		db.collectionName.find({name:/^o/})
	- Ends With :
		db.collectionName.find({name:/o^/})

//! Joins(Aggregations)  ---------------------------------------------:
	- Example :
		- User Table :
			{
			  "_id": ObjectId("user1"),
			  "username": "JohnDoe"
			}
		- Ordres Table :
			{
			  "_id": ObjectId("order1"),
			  "userId": ObjectId("user1"),
			  "product": "Laptop"
			}
		- To Get All Orders Belongs To Specific User :
			db.users.aggregate([ // aggregate function Like (join)
			  {
				$match: { // Match Keyword For creteria like (where user.id=order.userId)
				  "_id": ObjectId("user1")
				}
			  },
			  {
				$lookup: { //lookup Keyword to specify the needed fields and Collections
				  from: "orders", // Results From
				  localField: "_id", // The id from first Table
				  foreignField: "userId", // The id from second Table
				  as: "userOrders" // The Result Container
				}
			  }
			])

//! Create Indexes(Increase Performance)  -------------------------------------:
	- db.collectionName.createIndex({name:1})
