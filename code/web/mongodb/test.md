# Mongo

- run mongod.exe
- then open mongo in cmd
- bson - stands for binary json and is faster json at it encodes type and length formulation allowing it to be parsed faster
- Stores it as binary hex

---

- Collection is group of data items of various types.

---

db.<collection-name>.insert({key: value, key:value})

db.colelc tion

show collections -> view the collections

db.collectionName.find() > output all the data.

db.collectionName.find(query)

query example = {john  : "corgi"}

db.collectionName.updateOne({key: value}, {$set: {key:value}})





![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103190544411.png)

- use this for inserting multiple items put the square brackets first of course to make an array.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103191440973.png)

- update many matches any called Jake is now called Craig

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103192331946.png)

- on update you can add new key value pairs 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103192638738.png)

- appply multiple operators such as $currentDate to update the last modified time.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103194915903.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103194952815.png)

- delete a specific attribute or delete all in bottom.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103195303743.png)

- attributes themseleves can be non atomic as in be object literals themselves.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103195445040.png)

- find one shows this in the nicer format

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103195541670.png)

- Syntax for selecting nested attributes.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103195823807.png)

- find the ages greater than 8
- $gte - greater than or equal
- $lt - less than / 
- $lte - less than or equl
- $in - select out of a list of array values.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103200115785.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103200502702.png)

- use $or as follows above. 
- corresponding sequel is SELECT * FROM dogs WHERE age = 4 OR age > 10
- $nin - not in
- $ne - not equal.

# Mongoose

- Mongoose is an **object data mapper**
- ODMs such as mongoose map documents coming from a database into usable javascript objects
- mongoose provides methods for us to model out our application and define a schema 
- it offers easy methods to validate data and build complex queries from JS.

---

#### Models

- first define the blueprint of each type to variable names in a defined schema

- run node
- type .load <index.js> for example and it loads the file for looking at variables and shit.
- type modelName.save() to add an instance of the movies class to the database.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103210104553.png)

- Modelname.find then pass the parameters
- call the .then as its a query not a promise to fetch the data returned from the query.

*// Movie.findOne() // returns a single object not array*



*// use .exec() to return a proper promise to get the full stack trace*



*// can do find by id and pass in the primary key**// Movie.findOne() // returns a single object not array*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103211910152.png)

- Update one in mongoose using model.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103212206332.png)

- update all movies in the specified list to make their score 10.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103212421756.png)

- outputs the old version in the call-back data.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103212543770.png)

- add {new: true} in order to display in the callback the updated data value.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103214446113.png)

- find one and delete DELETEs then outputs what was deleted
- use delete and delete many to specific the conditions of deletion normally.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210103234942647.png)

- add a method to a schema 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210104003606026.png)

- set function virtual.
- william binded to first and rose binded to last.