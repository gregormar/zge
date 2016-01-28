###Import
Do eksperymentow uzylem niezbyt duzego pliku [restauracje.json]

**zaimportowanie do MongoDB**

```sh
time mongoimport -d restauracje -c restauracje < restauracje.json
2016-01-27T17:14:31.608+0100	connected to: localhost
2016-01-27T17:14:32.766+0100	imported 2548 documents

real	0m1.772s
user	0m0.176s
sys	0m0.020s
```

1.przykładowy json z kolekcji:

```sh

db.restauracje.findOne()
{
	"_id" : ObjectId("55f14312c7447c3da7051b26"),
	"URL" : "http://www.just-eat.co.uk/restaurants-cn-chinese-cardiff/menu",
	"address" : "228 City Road",
	"address line 2" : "Cardiff",
	"name" : ".CN Chinese",
	"outcode" : "CF24",
	"postcode" : "3JH",
	"rating" : 5,
	"type_of_food" : "Chinese"
}

```

2. zliczylem jsony

```sh
db.restauracje.count()
2548
```
wszystko ok, import przebiegl pomyslnie

3. Rodzaje restauracji

```sh
 db.restauracje.distinct("type_of_food").sort()
[
	"*NEW*",
	"Afghan",
	"African",
	"American",
	"Arabic",
	"Azerbaijan",
	"Bagels",
	"Bangladeshi",
	"Breakfast",
	"Burgers",
	"Cakes",
	"Caribbean",
	"Chicken",
	"Chinese",
	"Curry",
	"Desserts",
	"English",
	"Ethiopian",
	"Fish & Chips",
	"Greek",
	"Grill",
	"Healthy",
	"Ice Cream",
	"Japanese",
	"Kebab",
	"Korean",
	"Lebanese",
	"Mediterranean",
	"Mexican",
	"Middle Eastern",
	"Milkshakes",
	"Moroccan",
	"Nigerian",
	"Pakistani",
	"Pasta",
	"Peri Peri",
	"Persian",
	"Pick n Mix",
	"Pizza",
	"Polish",
	"Portuguese",
	"Punjabi",
	"Russian",
	"Sandwiches",
	"South Curry",
	"Spanish",
	"Sri-lankan",
	"Sushi",
	"Thai",
	"Turkish",
	"Vegetarian",
	"Vietnamese"
]
```

3. 10 restauracji zaczynajacych sie na "Best" nazwa - rodzaj jedzenia - ranking

```sh
db.restauracje.find({"name": /^Best/}, {_id: 0, name: 1, type_of_food: 1, rating: 1}).sort({name: 1}).limit(10)

{ "name" : "Best American Pizza", "rating" : 5, "type_of_food" : "Pizza" }
{ "name" : "Best American Pizza", "rating" : 5, "type_of_food" : "Pizza" }
{ "name" : "Best American Pizza", "rating" : 5, "type_of_food" : "Pizza" }
{ "name" : "Best Bite", "rating" : 5, "type_of_food" : "Pizza" }
{ "name" : "Best Bite", "rating" : 5, "type_of_food" : "Pizza" }
{ "name" : "Best Bite", "rating" : 5, "type_of_food" : "Pizza" }
{ "name" : "Best Buy Fisheries & Pizzeria", "rating" : 5, "type_of_food" : "Fish & Chips" }
{ "name" : "Best Charcoal Grill", "rating" : 5, "type_of_food" : "Turkish" }
{ "name" : "Best Charcoal Grill", "rating" : 5.5, "type_of_food" : "Turkish" }
{ "name" : "Best Charcoal Grill", "rating" : 5.5, "type_of_food" : "Turkish" }
> 
```
4. Restauracje oferujace potrawy Punjabi
```sh
db.restauracje.find({type_of_food: "Punjabi"},{_id: 0})

{ "URL" : "http://www.just-eat.co.uk/restaurants-absharindian-ig6/menu", 
"address" : "43 High Street", 
"address line 2" : "Ilford",
"name" : "Abshar Indian Cuisine",
"outcode" : "IG6",
"postcode" : "2AD",
"rating" : 6,
"type_of_food" : "Punjabi" }
```
...jedna ale konkretna

### AGREGACJE

1. 10 miejscowosci z restauracjami wystepujacych najwiecej razy:
```sh
db.restauracje.aggregate([
...   {"$group" : {"_id" : "$address line 2", "count" : {"$sum" : 1}}},
...   {"$sort" : {"count" : -1}},
...   {"$limit" : 10}])
{ "_id" : "London", "count" : 345 }
{ "_id" : "Birmingham", "count" : 85 }
{ "_id" : "Manchester", "count" : 58 }
{ "_id" : "Glasgow", "count" : 55 }
{ "_id" : "Essex", "count" : 40 }
{ "_id" : "Sheffield", "count" : 37 }
{ "_id" : "Leeds", "count" : 33 }
{ "_id" : "Liverpool", "count" : 32 }
{ "_id" : "Bradford", "count" : 30 }
{ "_id" : "Bristol", "count" : 30 }
```


2. Restauracje o najwyzszej ocenie:

```sh
 db.restauracje.aggregate([ { $group: {_id: "$name", avgRating: {$avg: "$rating"}}}, { $sort: {avgRating: -1 } } ])
{ "_id" : "Big Fish - Collection Only", "avgRating" : 6 }
{ "_id" : "Benny's - Collection Only", "avgRating" : 6 }
{ "_id" : "Best Fish & Chips", "avgRating" : 6 }
{ "_id" : "Beast Gourmet Burgers", "avgRating" : 6 }
{ "_id" : "Alex Plaice", "avgRating" : 6 }
{ "_id" : "Bearstead Fish Bar - Collection Only", "avgRating" : 6 }
{ "_id" : "Best Pizza & Kebab", "avgRating" : 6 }
{ "_id" : "Bath Sushi", "avgRating" : 6 }
{ "_id" : "Ark Burger & Salad Bar", "avgRating" : 6 }
{ "_id" : "Benny's Grill House", "avgRating" : 6 }
{ "_id" : "Ayubowan", "avgRating" : 6 }
{ "_id" : "Atlantic Gourmet", "avgRating" : 6 }
{ "_id" : "Athena B Fish Bar - Collection Only", "avgRating" : 6 }
{ "_id" : "Barnacle Bills", "avgRating" : 6 }
{ "_id" : "Asli Zaiqa", "avgRating" : 6 }
{ "_id" : "Aroma Bar Restaurant Takeaway - Collection Only", "avgRating" : 6 }
{ "_id" : "Arabesque", "avgRating" : 6 }
{ "_id" : "Ann's Thai@ The Bulls Head", "avgRating" : 6 }
{ "_id" : "Anis's - The Taste of Perfection", "avgRating" : 6 }
{ "_id" : "Biggapot Caribbean - Collection Only", "avgRating" : 6 }
Type "it" for more
```
3. Grupowanie według rankingu:

```sh
 db.restauracje.aggregate( [{"$group" : {"_id" : "$rating", "count" : {"$sum" : 1}}},{"$sort" : {"count" : -1}}, {"$limit" : 14}  ])
{ "_id" : 5, "count" : 1107 }
{ "_id" : 5.5, "count" : 600 }
{ "_id" : 4.5, "count" : 472 }
{ "_id" : 4, "count" : 167 }
{ "_id" : "Not yet rated", "count" : 63 }
{ "_id" : 3.5, "count" : 51 }
{ "_id" : 6, "count" : 49 }
{ "_id" : 3, "count" : 16 }
{ "_id" : 2.5, "count" : 13 }
{ "_id" : 1, "count" : 5 }
{ "_id" : 2, "count" : 3 }
{ "_id" : 1.5, "count" : 2 }
```
4. srednia ocen wedlug typu jedzenia:

```sh
 db.restauracje.aggregate([ { $group: {_id: "$type_of_food", avgRating: {$avg: "$rating"}} }, { $sort: {avgRating: -1 } }])
{ "_id" : "Pasta", "avgRating" : 6 }
{ "_id" : "Punjabi", "avgRating" : 6 }
{ "_id" : "Bagels", "avgRating" : 5.5 }
{ "_id" : "Ice Cream", "avgRating" : 5.5 }
{ "_id" : "Pick n Mix", "avgRating" : 5.5 }
{ "_id" : "Cakes", "avgRating" : 5.5 }
{ "_id" : "Bangladeshi", "avgRating" : 5.305555555555555 }
{ "_id" : "Persian", "avgRating" : 5.25 }
{ "_id" : "Polish", "avgRating" : 5.166666666666667 }
{ "_id" : "Mediterranean", "avgRating" : 5.166666666666667 }
{ "_id" : "Greek", "avgRating" : 5.142857142857143 }
{ "_id" : "Sushi", "avgRating" : 5.125 }
{ "_id" : "Fish & Chips", "avgRating" : 5.036697247706422 }
{ "_id" : "Curry", "avgRating" : 5.0361581920903955 }
{ "_id" : "Azerbaijan", "avgRating" : 5 }
{ "_id" : "Ethiopian", "avgRating" : 5 }
{ "_id" : "Korean", "avgRating" : 5 }
{ "_id" : "Portuguese", "avgRating" : 5 }
{ "_id" : "Breakfast", "avgRating" : 5 }
{ "_id" : "Afghan", "avgRating" : 4.966666666666667 }
Type "it" for more
> it
{ "_id" : "Burgers", "avgRating" : 4.954545454545454 }
{ "_id" : "Turkish", "avgRating" : 4.918918918918919 }
{ "_id" : "Pizza", "avgRating" : 4.914141414141414 }
{ "_id" : "Chinese", "avgRating" : 4.89367816091954 }
{ "_id" : "Kebab", "avgRating" : 4.88562091503268 }
{ "_id" : "Grill", "avgRating" : 4.875 }
{ "_id" : "Peri Peri", "avgRating" : 4.868421052631579 }
{ "_id" : "South Curry", "avgRating" : 4.833333333333333 }
{ "_id" : "Japanese", "avgRating" : 4.823529411764706 }
{ "_id" : "Lebanese", "avgRating" : 4.8059701492537314 }
{ "_id" : "Milkshakes", "avgRating" : 4.8 }
{ "_id" : "Mexican", "avgRating" : 4.7727272727272725 }
{ "_id" : "Sri-lankan", "avgRating" : 4.7 }
{ "_id" : "Moroccan", "avgRating" : 4.666666666666667 }
{ "_id" : "Sandwiches", "avgRating" : 4.666666666666667 }
{ "_id" : "Thai", "avgRating" : 4.65 }
{ "_id" : "American", "avgRating" : 4.617021276595745 }
{ "_id" : "Caribbean", "avgRating" : 4.583333333333333 }
{ "_id" : "Middle Eastern", "avgRating" : 4.535714285714286 }
{ "_id" : "Nigerian", "avgRating" : 4.5 }
```





