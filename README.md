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

#### Find by Inequality

* find all listings that have more than 3 beds

```
db.listingsAndReviews.find({
    'beds': {
        '$gt': 3
    }
},{
    'name': 1, 'beds':1
});
```

* find all listings that have 3 or more beds:
```
db.listingsAndReviews.find({
    'beds': {
        '$gte': 3
    }
},{
    'name': 1, 'beds':1
});
```

* find all listings that have less than 10 beds:
```
db.listingsAndReviews.find({
    'beds':{
        '$lt': 6
    }
},{
  'name': 1, 'beds': 1
});
```

* find all listings that have between 3 to 6 beds:
```
db.listingsAndReviews.find({
    'beds':{
        '$gte': 3,
        '$lte': 6
    }
},{
    'name': 1, 'beds': 1
});
```

# Answers to Hands On 1

1. ```
   db.companies.find({
       'founded_year':2006
   }, {
       'name':1,
       'founded_year':1
   })
   ```

2. ```
    db.companies.find({
        'founded_year':{
            '$gt': 2000
        }
    }, {
        'name':1,
        'founded_year':1
    })
    ```

3. ```
   db.companies.find({
       'founded_year': {
           '$gte':1900,
           '$lte':2010
       }
   },{
       'name': 1,
       'founded_year': 1
   });
   ```

4. ```
    db.companies.find({
        'ipo.valuation_amount':{
            '$gt':100000000
        }
    },{
        'name': 1,
        'ipo.valuation_amount': 1,
        'ipo.valuation_currency_code':1
    }).pretty();
    ```

5.
```
db.companies.find({
    'ipo.valuation_amount': {
        '$gt':100000000
    },
    'ipo.valuation_currency_code':'USD'
},{
    'name': 1,
    'ipo.valuation_amount':1,
    'ipo.valuation_currency_code': 1
}).pretty();
```

6. 
```
db.inspections.find({
    'result':'Violation Issued'
}, {
    'business_name': 1,
    'result': 1
})
```

### Find by elements within array
Find all listings that include 'Pets Allowed' in the array of amenities.

```
db.listingsAndReviews.find({
    'amenities':'Pets allowed'
},{
    'name': 1,
    'amenities': 1
}).pretty()
```

Find all listings that have *both* 'Pets allowed'
and 'Pets live on this property'

```
db.listingsAndReviews.find({
    'amenities': {
        '$all':['Pets allowed', 'Pets live on this property']
    }
},{
    'name': 1,
    'amenities':1
}).pretty();
```

Find all listings that have either wifi or internet:

```
db.listingsAndReviews.find({
    'amenities': {
        '$in':['Wifi', 'Internet']
    }
}, {
    'name': 1,
    'amenities': 1
}).pretty();
```

Find all listings that have been reviewed by the
reviewer id: 6184569

```
db.listingsAndReviews.find({
    'reviews': {
        '$elemMatch': {
            'reviewer_id': '5969944'
        }
    }
}, {
    'name':1,
    'reviews': {
        '$elemMatch': {
            'reviewer_id': '5969944'
        }
    }
}).limit(5).pretty();
```

## Hands On Solutions
1. 
```
db.accounts.find({
    'products':'InvestmentStock'
}, {
    'account_id':1,
    'products': 1
}).pretty();
```

2.
```
db.accounts.find({
    'products':{
        '$all':['InvestmentStock', 'Commodity']
    }
}, {
    'account_id':1,
    'products': 1
}).pretty();
```

3.
```
db.accounts.find({
    'products': {
        '$in':['Commodity', 'CurrencyService']
    }
}, {
    'account_id':1,
    'products':1
}).pretty()
```

4.
```
db.accounts.find({
    'products':{
        '$ne':'CurrencyService'
    }
},{
    'account_id': 1,
    'products': 1
}).pretty();
```

*OR*

```
db.accounts.find({
    'products':{
        '$nin':['CurrencyService', 'Commodity']
    }
},{
    'account_id': 1,
    'products': 1
}).pretty()
```

6.
```
db.accounts.find({
    'limit': {
        '$gt':10000
    },
    'products':{
        '$all':['InvestmentStock', 'InvestmentFund']
    }
}, {
    'account_id': 1,
    'limit': 1,
    'products': 1
}).pretty();
```

### Compounds criteria

Find all listings that are in Brazil or Canada
```
db.listingsAndReviews.find({
    '$or':[
        {
            'address.country':'Brazil'
        },
        {
            'address.country':'Canada'
        }
    ]
}, {
    'name': 1,
    'address.country': 1
}).pretty();
```

Find listings in Brazil or Canada, but if from Brazil,
have more than beds.

```
db.listingsAndReviews.find({
    '$or':[
        {
            'address.country':'Brazil',
            'beds': {
                '$gt': 4
            }
        },
        {
            'address.country':'Canada'
        }
    ]
}, {
    'name': 1,
    'address.country': 1,
    'beds': 1
}).pretty();
```

Find all listings that are from either Brazil or Canada, 
with beds greater than 4.
```
db.listingsAndReviews.find({
    '$and': [
        {
            'beds': {
                '$gt': 4
            }
        },
        {
            '$or':[
                {
                    'address.country':'Brazil'
                },
                {
                    'address.country':'Canada'
                }
            ]
        }
    ]
}, {
    'name': 1,
    'address.country': 1,
    'beds': 1
}).pretty();
```
Find listings that are not from Brazil and not
having more than 4 bedrooms.

```
db.listingsAndReviews.find({
    'address.country': {
        '$not': {
            $in:['Brazil', 'Canada']
        }
    },
    'beds': {
        '$not' : {
            '$gt':4
        }
    }
}, {
    'name': 1,
    'address.country': 1,
    'beds': 1
}).pretty()
```

### Select by ObjectId

```
db.restaurants.find({
    '_id': ObjectId('5eb3d668b31de5d588f4292a')
});
```

### Pattern search, case insensitive
```
db.restaurants.find({
    'name': {
        '$regex':"steak", '$options':'i'
    }
}, {
    'name': 1
});
```

### Find by date
```
db.listingsAndReviews.find({
    'last_review': {
        '$lt': new Date('2019-01-01')
    }
}, {
   'name': 1,
   'last_review': 1  
}).pretty();
```
```
db.listingsAndReviews.find({
    'last_review': {
        '$gte': new Date('2019-01-01')
    }
}, {
   'name': 1,
   'last_review': 1  
}).pretty();
```