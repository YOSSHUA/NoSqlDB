# Tarea 2 - MongoDB, queries con agregaciones
### Yosshua Eli Cisneros Villasana 179889
<br/>
<details><summary>DB de idiomas</summary>
<p>

## Abreviaciones
```javascript
db.primarydialects.insertMany([
	{"lang":"af", "locale":"af-ZA"},
	{"lang":"ar", "locale":"ar"},
	{"lang":"bg", "locale":"bg-BG"},
	{"lang":"ca", "locale":"ca-AD"},
	{"lang":"cs", "locale":"cs-CZ"},
	{"lang":"cy", "locale":"cy-GB"},
	{"lang":"da", "locale":"da-DK"},
	{"lang":"de", "locale":"de-DE"},
	{"lang":"el", "locale":"el-GR"},
	{"lang":"en", "locale":"en-US"},
	{"lang":"es", "locale":"es-ES"},
	{"lang":"et", "locale":"et-EE"},
	{"lang":"eu", "locale":"eu"},
	{"lang":"fa", "locale":"fa-IR"},
	{"lang":"fi", "locale":"fi-FI"},
	{"lang":"fr", "locale":"fr-FR"},
	{"lang":"he", "locale":"he-IL"},
	{"lang":"hi", "locale":"hi-IN"},
	{"lang":"hr", "locale":"hr-HR"},
	{"lang":"hu", "locale":"hu-HU"},
	{"lang":"id", "locale":"id-ID"},
	{"lang":"is", "locale":"is-IS"},
	{"lang":"it", "locale":"it-IT"},
	{"lang":"ja", "locale":"ja-JP"},
	{"lang":"km", "locale":"km-KH"},
	{"lang":"ko", "locale":"ko-KR"},
	{"lang":"la", "locale":"la"},
	{"lang":"lt", "locale":"lt-LT"},
	{"lang":"lv", "locale":"lv-LV"},
	{"lang":"mn", "locale":"mn-MN"},
	{"lang":"nb", "locale":"nb-NO"},
	{"lang":"nl", "locale":"nl-NL"},
	{"lang":"nn", "locale":"nn-NO"},
	{"lang":"pl", "locale":"pl-PL"},
	{"lang":"pt", "locale":"pt-PT"},
	{"lang":"ro", "locale":"ro-RO"},
	{"lang":"ru", "locale":"ru-RU"},
	{"lang":"sk", "locale":"sk-SK"},
	{"lang":"sl", "locale":"sl-SI"},
	{"lang":"sr", "locale":"sr-RS"},
	{"lang":"sv", "locale":"sv-SE"},
	{"lang":"th", "locale":"th-TH"},
	{"lang":"tr", "locale":"tr-TR"},
	{"lang":"uk", "locale":"uk-UA"},
	{"lang":"vi", "locale":"vi-VN"},
	{"lang":"zh", "locale":"zh-CN"}
]);
```
## Abreviación con nombre
```javascript
db.languagenames.insertMany([{"locale":"af-ZA", "languages":[
            "Afrikaans",
            "Afrikaans"
]},
{"locale":"ar", "languages":[
            "العربية",
            "Arabic"
]},
{"locale":"bg-BG", "languages":[
            "Български",
            "Bulgarian"
]},
{"locale":"ca-AD", "languages":[
            "Català",
            "Catalan"
]},
{"locale":"cs-CZ", "languages":[
            "Čeština",
            "Czech"
]},
{"locale":"cy-GB", "languages":[
            "Cymraeg",
            "Welsh"
]},
{"locale":"da-DK", "languages":[
            "Dansk",
            "Danish"
]},
{"locale":"de-AT", "languages":[
            "Deutsch (Österreich)",
            "German (Austria)"
]},
{"locale":"de-CH", "languages":[
            "Deutsch (Schweiz)",
            "German (Switzerland)"
]},
{"locale":"de-DE", "languages":[
            "Deutsch (Deutschland)",
            "German (Germany)"
]},
{"locale":"el-GR", "languages":[
            "Ελληνικά",
            "Greek"
]},
{"locale":"en-GB", "languages":[
            "English (UK)",
            "English (UK)"
]},
{"locale":"en-US", "languages":[
            "English (US)",
            "English (US)"
]},
{"locale":"es-CL", "languages":[
            "Español (Chile)",
            "Spanish (Chile)"
]},
{"locale":"es-ES", "languages":[
            "Español (España)",
            "Spanish (Spain)"
]},
{"locale":"es-MX", "languages":[
            "Español (México)",
            "Spanish (Mexico)"
]},
{"locale":"et-EE", "languages":[
            "Eesti keel",
            "Estonian"
]},
{"locale":"eu", "languages":[
            "Euskara",
            "Basque"
]},
{"locale":"fa-IR", "languages":[
            "فارسی",
            "Persian"
]},
{"locale":"fi-FI", "languages":[
            "Suomi",
            "Finnish"
]},
{"locale":"fr-CA", "languages":[
            "Français (Canada)",
            "French (Canada)"
]},
{"locale":"fr-FR", "languages":[
            "Français (France)",
            "French (France)"
]},
{"locale":"he-IL", "languages":[
            "עברית",
            "Hebrew"
]},
{"locale":"hi-IN", "languages":[
            "हिंदी",
            "Hindi"
]},
{"locale":"hr-HR", "languages":[
            "Hrvatski",
            "Croatian"
]},
{"locale":"hu-HU", "languages":[
            "Magyar",
            "Hungarian"
]},
{"locale":"id-ID", "languages":[
            "Bahasa Indonesia",
            "Indonesian"    
]},
{"locale":"is-IS", "languages":[
            "Íslenska",
            "Icelandic"
]},
{"locale":"it-IT", "languages":[
            "Italiano",
            "Italian"
]},
{"locale":"ja-JP", "languages":[
            "日本語",
            "Japanese"
]},
{"locale":"km-KH", "languages":[
            "ភាសាខ្មែរ",
            "Khmer"
]},
{"locale":"ko-KR", "languages":[
            "한국어",
            "Korean"
]},
{"locale":"la", "languages":[
            "Latina",
            "Latin"
]},
{"locale":"lt-LT", "languages":[
            "Lietuvių kalba",
            "Lithuanian"
]},
{"locale":"lv-LV", "languages":[
            "Latviešu",
            "Latvian"
]},
{"locale":"mn-MN", "languages":[
            "Монгол",
            "Mongolian"
]},
{"locale":"nb-NO", "languages":[
            "Norsk bokmål",
            "Norwegian (Bokmål)"
]},
{"locale":"nl-NL", "languages":[
            "Nederlands",
            "Dutch"
]},
{"locale":"nn-NO", "languages":[
            "Norsk nynorsk",
            "Norwegian (Nynorsk)"
]},
{"locale":"pl-PL", "languages":[
            "Polski",
            "Polish"
]},
{"locale":"pt-BR", "languages":[
            "Português (Brasil)",
            "Portuguese (Brazil)"
]},
{"locale":"pt-PT", "languages":[
            "Português (Portugal)",
            "Portuguese (Portugal)"
]},
{"locale":"ro-RO", "languages":[
            "Română",
            "Romanian"
]},
{"locale":"ru-RU", "languages":[
            "Русский",
            "Russian"
]},
{"locale":"sk-SK", "languages":[
            "Slovenčina",
            "Slovak"
]},
{"locale":"sl-SI", "languages":[
            "Slovenščina",
            "Slovenian"
]},
{"locale":"sr-RS", "languages":[
            "Српски / Srpski",
            "Serbian"
]},
{"locale":"sv-SE", "languages":[
            "Svenska",
            "Swedish"
]},
{"locale":"th-TH", "languages":[
            "ไทย",
            "Thai"
]},
{"locale":"tr-TR", "languages":[
            "Türkçe",
            "Turkish"
]},
{"locale":"uk-UA", "languages":[
            "Українська",
            "Ukrainian"
]},
{"locale":"vi-VN", "languages":[
            "Tiếng Việt",
            "Vietnamese"
]},
{"locale":"zh-CN", "languages":[
            "中文 (中国大陆)",
            "Chinese (PRC)"
]},
{"locale":"zh-TW", "languages":[
            "中文 (台灣)",
            "Chinese (Taiwan)"
        ]}]);
```
</p>
</details>

<details><summary>Ejemplos</summary>
<p>
- Qué idiomas base son los que más tuitean con hashtags? Cuál con URLs? Y con @?

```javascript
// Con Hashtags
db.tweets.aggregate([
	{$lookup: {from:"primarydialects","localField":"user.lang","foreignField":"lang","as":"language"}},
	{$lookup: {from:"languagenames","localField":"language.locale","foreignField":"locale","as":"fulllocale"}},
	{$match:{"entities.hashtags":{$not:{$size:0}}}},
	{$group: {_id:"$fulllocale.languages", "conteo": {$count:{}}}}
])

// Con URLs
db.tweets.aggregate([
	{$lookup: {from:"primarydialects","localField":"user.lang","foreignField":"lang","as":"language"}},
	{$lookup: {from:"languagenames","localField":"language.locale","foreignField":"locale","as":"fulllocale"}},
	{$match:{"entities.urls":{$not:{$size:0}}}},
	{$group: {_id:"$fulllocale.languages", "conteo": {$count:{}}}}
])

// Con User Mentions
db.tweets.aggregate([
	{$match:{"entities.user_mentions":{$not:{$size:0}}}},
	{$group: {_id:"$user.lang", "conteo": {$count:{}}}},
	{$lookup: {from:"primarydialects","localField":"_id","foreignField":"lang","as":"language"}},
	{$lookup: {from:"languagenames","localField":"language.locale","foreignField":"locale","as":"fulllocale"}},
])
```
- Qué idioma base es el que más hashtags usa en sus tuits?

```javascript
db.tweets.aggregate([
	{$group: {_id:"$user.lang", "totalHashtags": {$sum:{$size:"$entities.hashtags"}}}},
	{$lookup: {from:"primarydialects","localField":"_id","foreignField":"lang","as":"language"}},
	{$lookup: {from:"languagenames","localField":"language.locale","foreignField":"locale","as":"fulllocale"}},
	{$project:{"language":0}},
	{$sort:{"totalHashtags":-1}}
])
```
</p>
</details>

<br/>

## Ejercicios
<br/>

## 1) Cómo podemos saber si los tuiteros hispanohablantes interactúan más en las noches?
<br/>

Primero notamos que solo hay tweets a las 18, 19 y 20 horas en esta muestra.

```javascript
db.tweets.distinct("created_at").map(function(created_at){ return created_at.substring(11, 13)}).filter((v,i,a) => a.indexOf(v) == i);
```
Agrupamos por la hora.
```javascript
db.tweets.aggregate([ 
    { $group: { _id: { "lang": "$user.lang", "hour": { $substr: ["$created_at", 11, 2] } }, "counter": { $count: {} } } }, 
    { $match: { "_id.lang":"es"} }, 
    { $sort: {"counter":-1}}
]);
```

Entonces, los tuiteros hispanohablantes interactuan más en la noche.

<br/>

## 2) Cómo podemos saber de dónde son los tuiteros que más tiempo tienen en la plataforma?
<br/>


La idea es crear una fecha en formato ISODate a partir del string que tenemos del atributo user.created_at.
Una vez con ese campo, ejecutamos una diferencia en días entre hoy y la fecha que se obtuvo.
Finalmente, hacemos un join para saber los países y lo ordenamos de acuerdo a la antiguedad.
De esa manera, los primeros documentos serían los tuiteros más antiguos.

```javascript
db.tweets.aggregate([
    {$project: {"user.lang":1, "user.created_at":1, "_id":0}},
    {
        $addFields: {
            "fecha":{ 
                $dateFromParts:{
                    "year": { $toInt: {$substr: ["$user.created_at", 26, 4] }}, 
                    "month": { $switch : { 
                                    branches: [
                                        {case: {$eq: [ {$substr: ["$user.created_at", 4, 3]},  "Jan"]}, then: 1},
                                        {case: {$eq: [ {$substr: ["$user.created_at", 4, 3]},  "Feb"]}, then: 2},
                                        {case: {$eq: [ {$substr: ["$user.created_at", 4, 3]},  "Mar"]}, then: 3},
                                        {case: {$eq: [ {$substr: ["$user.created_at", 4, 3]},  "Apr"]}, then: 4},
                                        {case: {$eq: [ {$substr: ["$user.created_at", 4, 3]},  "May"]}, then: 5},
                                        {case: {$eq: [ {$substr: ["$user.created_at", 4, 3]},  "Jun"]}, then: 6},
                                        {case: {$eq: [ {$substr: ["$user.created_at", 4, 3]},  "Jul"]}, then: 7},
                                        {case: {$eq: [ {$substr: ["$user.created_at", 4, 3]},  "Aug"]}, then: 8},
                                        {case: {$eq: [ {$substr: ["$user.created_at", 4, 3]},  "Sep"]}, then: 9},
                                        {case: {$eq: [ {$substr: ["$user.created_at", 4, 3]},  "Oct"]}, then: 10},
                                        {case: {$eq: [ {$substr: ["$user.created_at", 4, 3]},  "Nov"]}, then: 11},
                                        {case: {$eq: [ {$substr: ["$user.created_at", 4, 3]},  "Dec"]}, then: 12}
                                    ]
                                }                                     
                    },
                    "day": { $toInt: {$substr: ["$user.created_at", 8, 2] }}
                }
            }
        }
    },
    {$addFields: { "antiguedad": { $dateDiff: { startDate: "$fecha", endDate: new ISODate(), unit: "day"} }}},
    {$sort:{antiguedad: -1}},
    {$limit: 5},
    {$lookup: {from:"primarydialects","localField":"user.lang","foreignField":"lang","as":"language"}},
    {$lookup: {from:"languagenames","localField":"language.locale","foreignField":"locale","as":"fulllocale"}},
    {$project: {antiguedad: 1, "fulllocale.languages":1}}    
]);
```
Como podemos ver los 5 usuarios con más tiempo de registrarse en la muestra son de US con 5539, 5496, 5467, 5463, 5435 días.


<br/>

## 3) En intervalos de 7:00:00pm a 6:59:59am y de 7:00:00am a 6:59:59pm, de qué paises la mayoría de los tuits?
<br/>


Extraemos la hora y la clasificamos en los intervalos dados.
Después agrupamos por país y por intervalo.
Finalmente ordenamos el conteo y los primeros serían los más altos.
```javascript
db.tweets.aggregate([
    {$project: {"user.lang":1, "created_at":1, "_id":0}},
    {
        $addFields: {
            "hora":{  $toInt: {$substr: ["$created_at", 11, 2] } }    
        }
    },
    {$addFields: 
        { "tipo":
            { 
                $switch : { 
                    branches: [
                        {case: { $or: [{$gte : ["$hora", 19]}, {$lte : ["$hora", 6]}] }, then: "7 PM - 6:59:59 AM"}                    
                    ],
                    default : "7 AM - 6:59:59 PM"
                }                                     
            }
        }
    },
    {$group: {_id: {"lang":"$user.lang", "Intervalo":"$tipo"}, "usuarios":{$count: {}}}},
    {$lookup: {from:"primarydialects","localField":"_id.lang","foreignField":"lang","as":"language"}},
    {$lookup: {from:"languagenames","localField":"language.locale","foreignField":"locale","as":"fulllocale"}},
    {$project: {_id:1, usuarios:1, "fulllocale.languages":1}},
    {$sort: {"usuarios": -1}}
]);
   
```

Para ambos intervalos, el país con más tweets es de US.


<br/>

## 4) De qué país son los tuiteros más famosos de nuestra colección?
<br/>

Aquí basta con ordenar respecto al número de seguidores y hacer un join para obtener el país.
```javascript
db.tweets.aggregate([         
    {$project: {"user.followers_count":1, "user.lang":1}}, 
    {$sort: {"user.followers_count":-1}},   
    {$limit: 10},
    {$lookup: {from:"primarydialects","localField":"user.lang","foreignField":"lang","as":"language"}},
    {$lookup: {from:"languagenames","localField":"language.locale","foreignField":"locale","as":"fulllocale"}},
    {$project: {_id:1,"user.followers_count":1, "fulllocale.languages":1}}
]);

```

El resultado del query saca a la luz que los más famosos son de US con 850k, 450k y 289k seguidores.

