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