# MongoDB_Notes


<h4> MongoDB Notes </h4>

- run commands below to create a running mongodb container

```docker pull mongo```
```docker run -d -p 27017:27017 -i --platform linux/arm64/v8   -t mongo ```

- download mongo db in your system 

```
brew list | grep mongodb-database-tools
brew tap mongodb/brew
brew install mongodb-database-tools
brew upgrade mongodb-database-tools

```

### Mongo DB Commands

- switch db

``` use <db>``` 

- create index in Restaurants collection 

``` db.Restaurants.createIndex(   { location : '2dsphere'  } );``` 

- get one item from collection

``` db.Restaurants.findOne();``` 

- find restauranst count near given point location within 400 m
``` 
db.Restaurants.find({    
    location : {
        $nearSphere : {
            $geometry : {
                    type : "Point",
                    coordinates : [ -73.93414657, 40.82302903 ]
             },
             $maxDistance : 400
        }
    }   
}).count();
```

- intersect a point with neighborhood and get one

```
var neighborhood = db.Neighborhoods.findOne( { 
   geometry: { 
      $geoIntersects: { 
           $geometry: { 
            type: "Point", 
            coordinates: [ -73.93414657, 40.82302903 ]
            } 
      } 
 } 
} );
console.log(neighborhood);
```

- find count of restaurants in the neighborhood find above

```
db.Restaurants.find( { 
  location: 
   { $geoWithin:
     { $geometry: neighborhood.geometry } 
   } 
} ).count()
```
