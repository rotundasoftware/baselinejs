Basline is a small library that organizes application logic and data in node.js web applications into interdependent services that can be used both on the client and the server. It provides a small, simple, in memory data store to organize and access data on the client. The same in memory database and logic can also be used on the server side.

A baseline object may be  initialized both on the server and on the client. On the server, a baseline object is initialized on a per request basis. For example, in an express.js app,

```javascript
var baseline = require( 'baseline' );

app.use( function( req, res, cb ) {
	req.baseline = new Baseline( {
		rt : new RouteService( { routes : routeHash } )
	} );
} );
```

On the client, a single baseline object is initialized and attached to the `window` object.

```javascript
window.Wikkem.baseline = new Baseline( {
	rt : new RouteService(),
} );
```

## Anatomy

A baseline instance is made up of services. A service is a module that contains application logic and potentially accessors for in-memory data that is operated on by that logic. Services may depend on each other.



### Collection services

collectionServices contain the application logic and the means to access in-memory data that is relevant to a database collection or table.

When used on the client side, collectionServices are very similar to backbone.js collections, with a few key differences:

* collectionServices are the preferred place to put application logic that operates on the data in the collection, whereas backbone collection classes generally do not serve that purpose.
* While there may exist multiple backbone collections of the same type of object, there is always only one service (just like there is aways only one database table).
* While backbone collections contain models that wrap the underlying data, collectionServices provide access to the underlying data only through per-field getters and setters. Objects are referenced by their id, instead of with pointers to models. This approach allows baseline to reliably keep track of changes.

## Bootstrapping data from the server to the client

Baseline streamlines the process of bootstrapping data from the client to the server using steamer.



