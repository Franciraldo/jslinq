jslinq v1.0.1
======

Another LINQ provider for Javascript

## Usage

#### Given the following source array:

```javascript
var data = [
	{ id: 1, name: "one", category: 'fruits', countries: ["Italy", "Austria"] },
	{ id: 2, name: "two", category: 'vegetables', countries: ["Italy", "Germany"] },
	{ id: 3, name: "three", category: 'vegetables', countries: ["Germany"] },
	{ id: 4, name: "four", category: 'fruits', countries: ["Japan"] },
	{ id: 5, name: "five", category: 'fruits', countries: ["Japan", "Italy"] }
];
```

#### Get *jslinq* queryable object
```javascript
var queryObj = jslinq(data);
```

#### Get *count* of elements
```javascript
var result = queryObj
	.count();

/*
result => 5
*/
```

#### Get all elements with *toList*
```javascript
var result = queryObj
	.toList();
	
/*
result => [
	{ id: 1, name: "one", ... },
	{ id: 2, name: "two", ... },
	{ id: 3, name: "three", ... },
	{ id: 4, name: "four", ... },
	{ id: 5, name: "five", ... }
];
*/
```

#### Get single element (or null) on list with *singleOrDefault*
```javascript
var result = queryObj
	.singleOrDefault(function(el){
		return el.name == "one";
	});

/*
result => { id: 1, name: "one", ... };
*/
```

#### Projection on one or more properties with *select*
```javascript
var result = queryObj
	.select(function(el){
		return el.id;
	})
	.toList();

/*
result => [1, 2, 3, 4, 5];
*/
```

#### Filter elements with *where*
```javascript
var result = queryObj
	.where(function(el){
		return el.name == 'two';
	})
	.toList();

/*
result => [{ id: 2, name: "two", ... }];
*/
```

#### Make a *groupBy* and count on each group
```javascript
var result = queryObj
	.groupBy(function(el){
		return el.category;
	})
	.toList();

/*
result => [
	{ key: 'vegetables', count: 2 }, 
	{ key: 'fruits', count: 3 }, 
];
*/
```

#### Merge two arrays with *join*
```javascript
var otherData = [
	{ id: 7, name: "seven", category: 'vegetables' }, 
	{ id: 8, name: "eight", category: 'fruit' }
];

var result = queryObj
	.join(otherData)
	.toList();
/*
result => [
	{ id: 1, name: "one", ... },
	{ id: 2, name: "two", ... },
	{ id: 3, name: "three", ... },
	{ id: 4, name: "four", ... },
	{ id: 5, name: "five", ... }, 
	{ id: 7, name: "seven", ... }, 
	{ id: 8, name: "eight", ... }
];
*/
```

#### Get *distinct* elements without repetitions
```javascript
var extraData = ["A", "B", "C", "B", "A", "D"];

var result = jslinq(extraData)
	.distinct()
	.toList();
/*
result => ["A", "B", "C", "D"];
*/
```

#### Sort ascending using *orderBy*
```javascript
var result = queryObj
	.orderBy(function(el){
		return el.name;
	})
	.toList();
/*
result => [
	{ id: 5, name: "five", ... },
	{ id: 4, name: "four", ... },
	{ id: 1, name: "one", ... },
	{ id: 3, name: "three", ... },
	{ id: 2, name: "two", ... }
];
*/
```

#### Sort descending using *orderByDescending*
```javascript
var result = queryObj
	.orderByDescending(function(el){
		return el.name;
	})
	.toList();
/*
result => [
	{ id: 2, name: "two", ... },
	{ id: 3, name: "three", ... },
	{ id: 1, name: "one", ... },
	{ id: 4, name: "four", ... },
	{ id: 5, name: "five", ... }
];
*/
```

#### Select multiple elements with *selectMany*
```javascript
var result = queryObj
	.selectMany(function(el){
		return el.countries;
	})
	.toList();
/*
result => [
	"Italy", "Austria", "Italy", "Germany", 
	"Germany", "Japan", "Japan", "Italy"] }
];
```

#### Get the first matching element with *firstOrDefault*
```javascript
var result = queryObj
	.firstOrDefault(function(el){
		return el.category == "vegetables";
	});
/*
result => { id: 2, name: "two", ... };
*/
```

#### Get the last matching element with *lastOrDefault*
```javascript
var result = queryObj
	.lastOrDefault(function(el){
		return el.category == "vegetables";
	});
/*
result => { id: 3, name: "three", ... };
*/
```

#### Check if at least one elements matchs expression with *any*
```javascript
var result = queryObj
	.any(function(el){
		return el.name == "two";
	});
/*
result => true;
*/
```

#### Check if all elements match expression with *all*
```javascript
var result = queryObj
	.all(function(el){
		return el.countries.length > 0;
	});
/*
result => true;
*/
```

#### You can also chain multiple methods
```javascript

var result = queryObj
	.where(function(el) { return el.category == 'fruits' })
	.select(function(el) { return el.id; })
	.toList();
/*
result => [1, 4, 5];
*/
```