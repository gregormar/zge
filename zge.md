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


