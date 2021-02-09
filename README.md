```
// see all databases associated with the user
show databases;
```

```
// to select a specific database
use sample_airbnb;
```

* Warning: If you use a database which name does not exist,
Mongo assumes that you want to create a new one. No warning/error 
messages will be shown!

## See collections in a database
```
show collections;
```

## Check the currently selected database
```
db
```

## Find documents from a collection

### Show all
```
db.listingsAndReviews.find()
```

### Limit the number of documents found
Limit to the first ten documents in the collection
```
db.listingsAndReviews.find().limit(10)
```

### Format the output nicely with .pretty()
```
db.listingsAndReviews.find().pretty().limit(10)
```
* `limit()` must always be the last

### Projecting
* Show only certain keys from the documents
```
// only shows the name, summary and address keys
// from the documents in listingsAndReviews 
db.listingsAndReviews.find({},{
    'name': 1,
    'summary':1,
    'address':1
}).pretty();
```

#### Projecting a sub-document (i.e, objects)
```
db.listingsAndReviews.find({},{
    'name': 1,
    'summary': 1,
    'address.street': 1,
    'address.country': 1
}).pretty();
```

### Searching for documents by critera

* Find all the listings that have exactly 2 beds.
```
db.listingsAndReviews.find({
    'beds':2
}, {
    'name':1, 'address':1, 'beds':1
}).pretty();
```

* Find all the listings that have exactly 2 beds and
has exactly 2 bedrooms

```
db.listingsAndReviews.find({
    'beds': 2,
    'bedrooms': 2
},{
    'name':1, 'address.country':1, 'beds':1, 'bedrooms':1
}).pretty();
```

* Find by country
```
db.listingsAndReviews.find({
    'address.country':'Brazil'
}, {
    'name':1, 'address.country':1, 'beds':1, 'bedrooms':1
}).pretty()
```

* Find all listings in Brazil that has exactly 2 beds, and exactly 2 bedrooms
```
db.listingsAndReviews.find({
    'beds': 2,
    'bedrooms': 2,
    'address.country': 'Brazil'
},{
    'name':1, 'address.country': 1, 'beds': 1, 'bedrooms': 1
}).pretty();
```

* Count number of results
```
db.listingsAndReviews.find({
    'address.country':'Brazil'
}, {
    'name':1, 'address.country':1, 'beds':1, 'bedrooms':1
}).count()
```