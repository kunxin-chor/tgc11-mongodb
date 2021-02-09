```
use sample_supplies;
```

```
db.sales.find({
    'couponUsed':true,
    'items.name':'pens'
},{
    'couponUsed': 1,
    'items':1
}).pretty();
```

```
db.sales.find({
    'couponUsed':true,
    'items':{
        '$elemMatch': {
            'name':'pens'
        }
    }
},{
    'couponUsed': 1,
    'items': {
        '$elemMatch':{
            'name':'pens'
        }
    }
}).pretty();
```

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