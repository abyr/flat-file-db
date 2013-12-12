# flat-file-db

Fast in-process flat file database for Node.js that supports JSON and caches all data in memory.
All data is persisted to an open file using a append-only algorithm ensuring compact file sizes and strong consistency.

	npm install flat-file-db

## Usage

Pass a database file to use to the flat-file-db constructor and wait for the database to open.
When it is open all data has been loaded into memory.

``` js
var flatfile = require('flat-file-db');
var db = flatfile('/tmp/my.db');

db.on('open', function() {
	db.put('hello', {world:1});  // store some data
	console.log(db.get('hello')) // prints {world:1}

	db.put('hey', {world:2}, function() {
		// 'hey' is now fully persisted
	});
});
```

If you don't want to wait for it to open use `flatfile.sync`

``` js
var db = flatfile.sync('/tmp/my.db');
console.log(db.get('hello')); // prints {world:1}
```

## API

* `db.put(key, val, [cb])` Insert or update new key

* `db.del(key, [cb])` Delete a key

* `db.get(key) -> doc` Get the value of a key

* `db.has(key) -> bool` True if db has key

* `db.keys() -> list` Get all keys as an array

* `db.close()` Close the database

## Events

* `db.on('open')` Fired when the db is open and ready for use.

* `db.on('close')` Fired when the db is fully closed

## License

MIT