## Create a new database
Just go aheand `use` the database as if exists.

`use animal_shelter`

This is the procedure how the database gets collected:
Mongo will automatically save the new database when:
* it has at least one collection
* and that collection at least one document

### Insert into a collection
```db.<collection name>.insert({....})```

Insert a new dog named Fluffy into the system:
```
db.animals.insert({
    'name':'Fluffy',
    'age':3,
    'breed':'Golden Retriever',
    'species':'Dog'
})
```
Once we done this step, if the database has not been saved,
it will be now saved.

### Inserting many into a document
```
db.animals.insertMany([
    {
        'name':'Muffin',
        'age': 10,
        'breed':'Orange Tabby',
        'species':'Cat'
    },
    {
        'name':'Dazzly',
        'age':3,
        'breed':'Bunny',
        'species':'Bunny'
    }
])
```

## Hands on solutions
```
use fake_school;

db.students.insertMany([
    {
        'name':'Jane Doe',
        'age': 13,
        'subjects':[
            'Defense against the Dark Arts',
            'Charms',
            'History of Magic'
        ],
        'date_enrolled':new Date('2016-05-13')
    },
    {
        'name':'James Verses',
        'age': 14,
        'subjects':[
            'Transfiguration',
            'Alchemy'
        ],
        'date_enrolled':new Date('2015-06-15')
    },
    {
        'name':'Jonathan Goh',
        'age': 12,
        'subjects':[
            'Divination',
            'Study of Ancient of Runes'
        ],
        'date_enrolled':new Date('2017-04-16')
    }
]);
```

## Update an existing document

### Update a document by its object id

#### 'Prestige method' aka PUT
Replaces the entire document but retain the identity
by keeping the old id.
```
db.students.update({
    '_id':ObjectId("60234f4061da21d6e116d54d")
}, {
    'name':'James Verses',
    'age': 15,
    'subjects':[
        'Transfiguration',
        'Alchemy'
    ],
    'date_enrolled': new Date('2015-06-15')
})
```

### Patch method
Updates one or more keys of the existing document
```
db.students.update({
    '_id':ObjectId("60234f4061da21d6e116d54d"),
}, {
    '$set':{
        'age': 16
    }
})
```

### Update many
Update all students whose age is 13 or higher
```
db.students.updateMany({
    'age': {
        '$gte': 13
    }
}, {
    '$set':{
        'fyp': true
    }
})
```

### Delete
```
db.students.remove({
    '_id':ObjectId("60234f4061da21d6e116d54e")
});
```

## Manage sub-documents

Suppose we have a document that looks like this:

```
db.animals.insert({
    'name':'Cookie',
    'age': 1,
    'breed':'Beagle',
    'species':'Dog',
    'tags':[
        'playful', 'toilet-trained', 'good with cats'
    ]
})
```

### Push to the back of the tags array for Cookie
```
db.animals.update({
    '_id':ObjectId("602358c861da21d6e116d54f")
},{
    '$push':{
        'tags':'good with kids'
    }
})
```

### Remove 'good with cats' from the tags array for Cookie

```
db.animals.update({
    '_id':ObjectId("602358c861da21d6e116d54f")
},{
    '$pull': {
        'tags':'good with cats'
    }
})
```

### Updating sub-documents

Imagine we add a sub-document to Cookie:

```
db.animals.update({
    '_id':ObjectId("602358c861da21d6e116d54f"),
},{
    '$set': {
        'vet': {
            'name': 'Dr Linda Khoo',
            'contact': 9999998,
            'address':'Sunset Way Blk 3 #01-02'
        }
    }
})
```

Update Dr. Linda Khoo's name 'Dr. Linda Tan'.

```
db.animals.update({
    '_id':ObjectId("602358c861da21d6e116d54f")
}, {
    '$set': {
        'vet.name':'Dr Linda Tan'
    }
})
```

### Updating a specific object in an array 

Assume that Cookie has a list of checkups that it has attended.

```
db.animals.update({
     '_id':ObjectId("602358c861da21d6e116d54f")
},{
    '$set': {
        'checkups': [
            {
                '_id':ObjectId(),
                'diagnosis':'Inflammation',
                'treatment':'Anti-inflammatory'
            },
            {
                '_id':ObjectId(),
                'diagnosis':'Seperation anxiety',
                'treatment':'Anti-anxiety medication'
            },
            {
                '_id':ObjectId(),
                'diagnosis':'Needs to be fixed',
                'treatment':'Neutering'
            }
        ]
    }
})
```

Update the checkup with seperation anxiety for the diagnosis,
and change its diagnosis to "Seperation anxiety and depression"
and the treatment to be "Anti-depressants".

```
// update the document that has inside it
checkups array, the specified _id 
db.animals.update({
   'checkups': {
       '$elemMatch':{
           '_id':ObjectId("60235c5961da21d6e116d551")
       }
   }
},{
    '$set': {
        'checkups.$.diagnosis':"Seperation anxiety and depression",
        'checkups.$.treatment':"Anti-depressants"
    }
})
```

## Hands on solution:

```
db.animals.insertMany([
    {
        "name" : "Jorden",
        "age" : 15,
        "breed" : "Golden Retriever",
        "species" : "Dog"
    },
    {
        "name" : "Dash",
        "age" : 3,
        "breed" : "Hamster",
        "species": "Hamster"
    },
    {
        "name" : "Carrot",
        "age" : 1.5,
        "breed" : "Australian Dwarf",
        "species" : "Rabbit"
    }
])
```

```
db.animals.update({
    '_id':ObjectId("602380a6c06ce48e91e1b7ef")
},{
    '$set':{
        'age':2.5
    }
})
```

```
db.animals.update({
    "_id" : ObjectId("602380a6c06ce48e91e1b7ee")
},{
    'name': 'Dash',
    'age': 4.5,
    'breed': 'Winter White',
    'species': 'hamster' 
})
```

```
db.animals.remove(
    {
        '_id':ObjectId("602380a6c06ce48e91e1b7ed")
})
```