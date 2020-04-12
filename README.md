# MapReduce with mongoDB
__Task:__ Find the top 10 hashtags used in the given tweets, the dataset can be found [here](https://github.com/ozlerhakan/mongodb-json-files/blob/master/datasets/tweets.zip).
## Map impl.
```javascript
map = function(){
    if (this.hasOwnProperty("entities"))
    {
        var hashtags = this.entities.hashtags;
        hashtags.forEach(function(data){
            emit(data.text, 1);
            });
    }};
```
The if clause was needed, because not all entries had the "entities" field.

## Reduce impl.
```javascript
reduce = function(key, values){
    return Array.sum(values);
}
```

## Mapreduce
```javascript
db.tweet.mapReduce(map,reduce,{out: "map_reduce_result"})
```
Save the output in a new collection, so it can be queried later.

## Sorting
```javascript
db.map_reduce_result.find().sort({value: -1}).limit(10);
```

## Result
```json
{ "_id" : "FCBLive", "value" : 27 }
{ "_id" : "AngularJS", "value" : 21 } 
{ "_id" : "nodejs", "value" : 20 }
{ "_id" : "LFC", "value" : 19 }
{ "_id" : "EspanyolFCB", "value" : 18 
{ "_id" : "IWCI", "value" : 16 }
{ "_id" : "webinar", "value" : 16 }
{ "_id" : "GlobalMoms", "value" : 14 }
{ "_id" : "javascript", "value" : 14 }
{ "_id" : "RedBizUK", "value" : 12 }
```
