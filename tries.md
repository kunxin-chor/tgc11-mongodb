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