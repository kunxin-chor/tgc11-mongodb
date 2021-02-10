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