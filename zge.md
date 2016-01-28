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






