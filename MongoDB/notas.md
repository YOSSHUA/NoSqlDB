# Sintaxis MongoDB
<br/>

# Insertar documentos en la bd
<br/>

## db.db_name.insertOne(documento_json)
<br/>
<br/>

## db.db_name.insertMany(documento_json)

documento_json:
- [{..}, {..} , ... , {..}]

<br/>
<br/>

# Obtener documentos de la bd
<br/>

## db.db_name.find( filter_json, attributes_json ) - busca documentos


### filter_json
- {"attrib" : value } -> Documentos donde "col" tenga como valor $value$.

### attributes_json - project
- {"attrib" : 0 } -> Todos los atributos menos los que tienen 0.
- {"attrib" : 1, ... , "attrib_n":1 } -> Todos los atributos que tienen 1.

<br/>

## Equivalencias con SQL

| Operación              | Sintaxis                    | E.g.                                                    | Equivalencia RDBMS                                                                    |
|------------------------|---------------------------|----------------------------------------------------------------|------------------------------------------------------------------------------------|
| Igual a X              | `{"key":[value]}`     | `db.tweets.find({"source":"web"})`                | where source = 'web'                                                       |
| AND en el WHERE              | `{"key1":[value1],"key2":[value2]}`     | `db.tweets.find({"source":"web","favorited":false})`                | where source = 'web' **and** favorited = false                                                       |
| Menor que              | `{"key":{$lt:[value]}}`     | `db.tweets.find({"user.friends_count":{$lt:50}})` | where user.friends_count < 50 (aquí estamos "viajando" del documento principal al documento anidado `user` y de ahí a su atributo `friends_count`                                                                   |
| Menor o igual a       | `{"key":{$lte:[value]}}`    | `db.tweets.find({"user.friends_count":{$lte:50}})` | where user.friends_count <= 50                                                             |
| Mayor que           | `{"key":{$gt:[value]}}`     | `db.tweets.find({"user.friends_count":{$gt:50}})`  | where user.friends_count > 50                                                                   |
| Mayor o igual a    | `{"key":{$gte:[value]}}`    | `db.tweets.find({"user.friends_count":{$gte:50}})` | where user.friends_count >= 50                                                                  |
| Diferente a             | `{"key":{$ne:[value]}}`     | `db.tweets.find({"user.friends_count":{$ne:50}})`  | where user.friends_count != 50                                                                  |
| Valores presentes en array     | `{"key":{$in:[value1,value2...valueN]}}` | `db.tweets.find({"entities.urls.indices":{$in:[54,74]}})` | where entities.urls.indices **in** (54,74)                    |
| Valores ausentes en array | `{"key":{$nin:[value]}}`                 | `db.tweets.find({"entities.urls.indices":{$nin:[54,74]}})`     | where entities.urls.indices **not in** (54,74) |

<br/>
<br/>

### Nota: si trabajamos con regex {attrib : /regex/ } (en general cualquier condición sobre un atributo) y estamos haciendo match con arrays y hay al menos un elemento que hace match, entonces nos va a regresar todo el array.
<br/>


## $elemMatch
Sirve para seleccionar documentos en colleciones que tienen arreglos para que hagan match perfecto.  
``` javascript
db.collection_name.find({ key : { $elemMatch : { "key" : val } } })
```
Si no es arreglo podemos usar { "key" : val }.

## $slice
Sirve para traer un cierto número de elementos cuando el atributo tiene como valor un arreglo.<br/>
- db.collection_name.find({},{"attrib":{$slice:#objects}})

# Ejercicios

4)
``` javascript
db.restaurants.find({}, {"restaurant_id":1, "name":1, "borough":1, "address.zipcode":1, "_id":0})
```
5)
``` javascript
db.restaurants.find({"borough": "Bronx"})
```
6)
``` javascript
db.restaurants.find({"borough": "Bronx"}).limit(5)
```
7)
``` javascript
db.restaurants.find({"borough": "Bronx"}).skip(5).limit(5)
```
8)
``` javascript
db.restaurants.find({"grades.score":{$gte:90}}, {"grades.score":1})
```
9)
``` javascript
db.restaurants.find({"grades":{$elemMatch:{"score":{$gte:80, $lte:100}}}}, {"grades.score":1})
```
10)
``` javascript
db.restaurants.find({"address.coord.0": {$lte:-95.754168}}, {"address.coord":1})
```
11)
``` javascript
db.restaurants.find({"cuisine":{$ne:"American"},"grades.score":{$gt:70} , "address.coord.0": {$lte:-65.754168} })
```
<br/>

# Agrupaciones

## $match 
Filtrado de todos los documentos que nos interesan para el query (como el WHERE en SQL). Se puede conjuntar con $project.
<br/>

## $group
Agrega los renglones seleccionados respecto a cierta llave, previo a aplicar algun operador.
<br/>
Aspectos a tomar en cuenta:<br/>
- El atributo de agrupación se llama ```_id```.<br/>
- El atributo por el que vamos a agregar debe ir con $. (```$name```)
<br/>

<details><summary>DB de universidades</summary>
<p>

```javascript
use 3tdb;
db.universities.insertMany([
{
  country : 'Spain',
  city : 'Salamanca',
  name : 'USAL',
  location : {
    type : 'Point',
    coordinates : [ -5.6722512,17, 40.9607792 ]
  },
  students : [
    { year : 2014, number : 24774 },
    { year : 2015, number : 23166 },
    { year : 2016, number : 21913 },
    { year : 2017, number : 21715 }
  ]
},
{
  country : 'Spain',
  city : 'Salamanca',
  name : 'UPSA',
  location : {
    type : 'Point',
    coordinates : [ -5.6691191,17, 40.9631732 ]
  },
  students : [
    { year : 2014, number : 4788 },
    { year : 2015, number : 4821 },
    { year : 2016, number : 6550 },
    { year : 2017, number : 6125 }
  ]
}
]);
db.courses.insertMany([
{
  university : 'USAL',
  name : 'Computer Science',
  level : 'Excellent'
},
{
  university : 'USAL',
  name : 'Electronics',
  level : 'Intermediate'
},
{
  university : 'USAL',
  name : 'Communication',
  level : 'Excellent'
}
]);
```

</p>
</details>

### Ejemplo
```javascript 
 db.universities.aggregate([ 
    { $match:{country: 'Spain', city: 'Salamanca'} },
    { $project:{_id : 0, country : 1, city : 1, name : 1} },
    { $group:{_id: "$name", conteo:{$sum:1}} },
    { $project:{_id : 0, "uni" : "$_id", conteo:1} }
 ])
 ```
 *Notar que el count se hace como $sum:1, sino sería como conteo:{$count:{}}
<br/>

## $sort
Ordena los resultados de acuerdo a un criterio

## $out
Toma la ejecución de toda la salida del pipeline y lo guarda en otra colección.<br/>
Se agrega 
```javascript
 $out : "nameNewCollection"
``` 
al final del pipeline.
<br/>

## $unwind
Si tenemos un arreglo, esta instrucción sirve para "desenrrollar" un array, ya que ```$group``` no permite trabajar con arrays. Como un producto entre todos los elementos del array y el objeto completo. Al final queda un objeto por cada elemento del array para cada elemento de la colección.

### Ejemplo
```javascript 
db.universities.aggregate([
	{ $unwind: '$students' },
	{ $project: { _id: 0, "name": 1, 'students.year': 1, 'students.number': 1 } },
	{ $group: { _id: "$name", promedioAlumnos: { $avg: "$students.number" } } },
	{$project:{_id:0,"uni":"$_id",promedioAlumnos:1}}
])
 ```

## Funciones de agregación

|Función|Descrip|
|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| $addToSet     | Después de agrupar, agrega elementos individuales a un array|
| $avg          | Promedio|
| $count        | Conteo (igual a `{$sum:1}`|
| $first        | Regresa el 1er documento o diccionario de cada grupo. ⚠️No confundir con el operador `$first` aplicable a arrays. Este operador no se ocupa del orden, eso se garantiza desde el stage `$sort` del pipeline |
| $last         | Regresa el último documento o diccionario de cada grupo. Mismas reglas y observaciones que `$first`|
| $max          | Regresa el máximo de cada grupo|
| $mergeObjects | Después de armar los grupos, combinar los objetos/diccionarios/documentos que correspondan al grupo en uno solo|
| $min          | Regresa el mínimo de cada grupo|
| $stdDevPop    | Regresa la [desviación estándar de la población](https://statistics.laerd.com/statistical-guides/measures-of-spread-standard-deviation.php) (entre _n_) de cada grupo|
| $stdDevSamp   | Regresa la [desviación estándar de la muestra](https://statistics.laerd.com/statistical-guides/measures-of-spread-standard-deviation.php) (entre _n-1_) de cada grupo|
| $sum          | Suma acumulativa de cada grupo|

## Funciones para fechas
A continuación los operadores más comunes sobre `ISODate`:

| Función | Descripción y Ejemplo|
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| $dateAdd        | `{ $dateAdd: {startDate: ISODate("2020-10-31T12:10:05Z"), unit: "month", amount: 1} }` - Agrega `amount` al campo `unit` de la fecha `startDate`                                                                                                                            |
| $dateDiff       | `{ $dateDiff: { startDate: ISODate("2014-01-01T08:00:00Z"), endDate: ISODate("2014-02-03T09:00:00Z"), unit: "day"} }` - Regresa la diferencia en `unit` entre `startDate` y `endDate`  |
| $dateFromString | `{ $dateFromString: {dateString: "15-06-2018", format: "%d-%m-%Y"} }` - Parsea el string `dateString` representando una fecha en formato `format` para convertirlo en un objeto `ISODate` que contenga esa misma fecha.                                                                                                                            |
| $dateSubtract   | `{ $dateSubtract: {startDate: ISODate("2020-10-31T12:10:05Z"), unit: "month", amount: 1} }` - Susbtrae `amount` al campo `unit` de la fecha `startDate`                                                                                                                     |
| $dateToParts    | `$dateToParts: { date: ISODate("2017-01-01T01:29:09.123Z") }` - Descompone el `date` en sus partes. Retorna `"date" : {"year" : 2017, "month" : 1, "day" : 1, "hour" : 1, "minute" : 29, "second" : 9, "millisecond" : 123}`                                                                                                          |
| $dateToString   | `{ $dateToString: { format: "%Y-%m-%d %H:%M:%S", date: ISODate("2014-01-01T08:15:39.736Z") } }` - Convierte un `ISODate` en un string con una fecha formateada por `format`. Retorna `"2014-01-01 03:15:39"`. Ver [opciones de formato](https://docs.mongodb.com/manual/reference/operator/aggregation/dateToString/).                                                                                                                           |
| $dayOfMonth     | Los siguientes operadores tienen la sintaxis `{ $[operador]: [objeto ISODate] }`. Regresa un numérico entre 1 y 31 del objeto `ISODate`.                                                                                                    |
| $dayOfWeek      | Regresa un numérico entre 1 (Domingo) y 7 (Sábado) del objeto `ISODate`. |
| $dayOfYear      | Regresa un numérico entre 1 y 366 (bisiesto) del objeto `ISODate`. |
| $hour           | Regresa un numérico entre 0 y 23 del objeto `ISODate`. |
| $isoDayOfWeek   | Regresa un numérico entre 1 (Lunes) y 7 (Domingo) del objeto `ISODate`. No confundir con `$dayOfWeek` |
| $isoWeek        | Regresa un numérico entre 1 y 53 del objeto `ISODate`.  |
| $millisecond    | Regresa un numérico entre 0 y 999 del objeto `ISODate`. |
| $minute         | Reegresa un numérico entre 0 y 59 del objeto `ISODate`. |
| $month          | Regresa un numérico entre 1 (Enero) y 12 (Diciembre) del objeto `ISODate`. |
| $second         | Regresa un numérico entre 0 y 60 (cuando es _leap second_) del objeto `ISODate`. |
| $year           | Regresa el valor del año del objeto `ISODate`|


<details><summary>DB para el siguiente ejemplo</summary>
<p>

```javascript
db.sales.insert( [
   { _id: 1, year: 2017, item: "A", quantity: { "2017Q1": 500, "2017Q2": 500 } },
   { _id: 2, year: 2016, item: "A", quantity: { "2016Q1": 400, "2016Q2": 300, "2016Q3": 0, "2016Q4": 0 } } ,
   { _id: 3, year: 2017, item: "B", quantity: { "2017Q1": 300 } },
   { _id: 4, year: 2016, item: "B", quantity: { "2016Q3": 100, "2016Q4": 250 } }
])
```

</p>
</details>

```javascript
// Ejemplo para agrupar por item y hacer merge de los atributos quantity
db.sales.aggregate([
   { $group : { _id: "$item", "mergedSales": {$mergeObjects: "$quantity"} }}
]);
```
<br/>

## $addField
Crea campos nuevos basados en agregaciones de atributos de cada documento.

### Ejemplo
``` javascript
db.restaurants.aggregate([
	{$unwind:"$grades"},
	{$project:{"grades.score":1, "name":1}},
	{$group:{_id:"$name", "gradeArray":{$push:"$grades.score"}}},
	{$project:{"name":"$_id",_id:0,"gradeArray":1}},
	{$addFields:{"totalScore":{$sum:"$gradeArray"},"avgScore":{$avg:"$gradeArray"}}}
]);
// {$match:{totalScore: {$gte: 100}}} to match something in the new column
```
<br/>

## $sortByCount
Es un operador que funge como si tuviéramos lo siguiente:
``` javascript
db.collection.aggregate([
	{ $group: { _id: <expression>, count: { $sum: 1 } } },
	{ $sort: { count: -1 } }
])
```

<details><summary>DB de obras de arte</summary>
<p>

```javascript
db.artwork.insertMany([
	{ "_id" : 1, "title" : "The Pillars of Society", "artist" : "Grosz", "year" : 1926, "tags" : [ "painting", "satire", "Expressionism", "caricature" ] },
	{ "_id" : 2, "title" : "Melancholy III", "artist" : "Munch", "year" : 1902, "tags" : [ "woodcut", "Expressionism" ] },
	{ "_id" : 3, "title" : "Dancer", "artist" : "Miro", "year" : 1925, "tags" : [ "oil", "Surrealism", "painting" ] },
	{ "_id" : 4, "title" : "The Great Wave off Kanagawa", "artist" : "Hokusai", "tags" : [ "woodblock", "ukiyo-e" ] },
	{ "_id" : 5, "title" : "The Persistence of Memory", "artist" : "Dali", "year" : 1931, "tags" : [ "Surrealism", "painting", "oil" ] },
	{ "_id" : 6, "title" : "Composition VII", "artist" : "Kandinsky", "year" : 1913, "tags" : [ "oil", "painting", "abstract" ] },
	{ "_id" : 7, "title" : "The Scream", "artist" : "Munch", "year" : 1893, "tags" : [ "Expressionism", "painting", "oil" ] },
	{ "_id" : 8, "title" : "Blue Flower", "artist" : "O'Keefe", "year" : 1918, "tags" : [ "abstract", "painting" ] },
])
```
</p>
</details>

### Ejemplo

```javascript
db.artwork.aggregate( [ { $unwind: "$tags" },  { $sortByCount: "$tags" } ] )
```
1) Qué idiomas base son los que más tuitean con hashtags? Cuál con URLs? Y con @?
```javascript
db.tweets.aggregate(  
  {
    $match:{
      "entities.user_mentions":{$not:{$size: 0}}
    }
  },
  {
    $group:{
      _id:"$user.lang", conteo:{$count:{}}
    }
  },
  {
    $lookup:{
      from: "primarydialects",
      localField: "_id",
      foreignField: "lang",
      as: "language"
    }
  },
  {
    $lookup:{
      from: "languagenames",
      localField: "language.locale",
      foreignField: "locale",
      as: "fulllocale"
    }
  }
);

db.tweets.aggregate(  
  {
    $match:{
      "entities.user_mentions":{$not:{$size: 0}}
    }
  },
  {
    $group:{
      _id:"$user.lang", conteo:{$count:{}}
    }
  },
  {
    $lookup:{
      from: "primarydialects",
      localField: "_id",
      foreignField: "lang",
      as: "language"
    }
  },
  {
    $lookup:{
      from: "languagenames",
      localField: "language.locale",
      foreignField: "locale",
      as: "fulllocale"
    }
  }
);
```
2) 
```javascript
db.tweets.aggregate(  
 
  {
    $group:{
      _id:"$user.lang", "totalHashtag" : {
        $sum:{
          $size:"$entities.hashtags"}
      }
    }
  },
  {
    $lookup:{
      from: "primarydialects",
      localField: "_id",
      foreignField: "lang",
      as: "language"
    }
  },
  {
    $lookup:{
      from: "languagenames",
      localField: "language.locale",
      foreignField: "locale",
      as: "fulllocale"
    }
  },
  {
    $project:{
      language:0
    }
  },
  {
    $sort:{
      totalHashtag:-1
    }
  }
);
```
