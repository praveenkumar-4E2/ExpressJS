# stucture reference
for complete details visit [Mongoose documentation](https://mongoosejs.com/docs/api/model.html)
```js
Mongoose
├── Schemas
│   ├── Schema()
│   ├── Methods
│   │   ├── .add(obj) : Schema
│   │   ├── .path(path) : SchemaType
│   │   ├── .virtual(path) : VirtualType
│   │   ├── .method(name, fn) : void
│   │   ├── .static(name, fn) : void
│   │   ├── .index(fields, options) : void
│   │   ├── .pre(hook, fn) : void
│   │   └── .post(hook, fn) : void
│   └── Return Types: Schema, void, function, Object
│
├── Models
│   ├── model(name, schema) : Model
│   ├── Methods
│   │   ├── .find(query) : Promise<Document[]>
│   │   ├── .findById(id) : Promise<Document>
│   │   ├── .create(doc) : Promise<Document>
│   │   ├── .updateOne(filter, update) : Promise<Object>
│   │   ├── .deleteOne(filter) : Promise<Object>
│   │   └── .aggregate(pipeline) : Promise<Object[]>
│   └── Return Types: Model, Promise<Document>, Promise<Object[]>, Promise<Object>
│
├── Documents
│   ├── Properties
│   │   ├── doc._id : ObjectId
│   │   ├── doc.isNew : Boolean
│   │   └── doc.schema : Schema
│   ├── Methods
│   │   ├── .save() : Promise<Document>
│   │   ├── .remove() : Promise<void>
│   │   ├── .validate() : Promise<void>
│   │   ├── .populate(path) : Promise<Document>
│   │   └── .toJSON() : Object
│   └── Return Types: Promise<Document>, Promise<void>, Object
│
├── Queries
│   ├── Properties
│   │   ├── .where(path) : Query
│   │   ├── .limit(n) : Query
│   │   ├── .skip(n) : Query
│   │   └── .sort(fields) : Query
│   ├── Methods
│   │   ├── .exec() : Promise<Document[]>
│   │   ├── .countDocuments() : Promise<number>
│   │   ├── .findOne(query) : Promise<Document>
│   │   └── .findByIdAndUpdate(id, update) : Promise<Document>
│   └── Return Types: Query, Promise<Document>, Promise<number>, Promise<Document[]>
│
├── Connections
│   ├── mongoose.connect(uri, options) : Promise<Connection>
│   ├── Methods
│   │   ├── .disconnect() : Promise<void>
│   │   ├── .connection : Connection
│   │   └── .on(event, callback) : void
│   └── Return Types: Promise<Connection>, void
│
├── Validation
│   ├── Methods
│   │   ├── .validateSync() : Error | null
│   │   ├── .validate() : Promise<void>
│   │   └── required : Boolean
│   └── Return Types: Promise<void>, Error, Boolean, null
│
├── Aggregations
│   ├── Methods
│   │   ├── .aggregate(pipeline) : Aggregate
│   │   ├── .group() : Aggregate
│   │   ├── .match() : Aggregate
│   │   ├── .project() : Aggregate
│   │   └── .sort() : Aggregate
│   └── Return Types: Aggregate, Promise<Object[]>
│
├── Transactions
│   ├── Methods
│   │   ├── .startSession() : Promise<ClientSession>
│   │   └── .withTransaction(fn) : Promise<void>
│   └── Return Types: ClientSession, Promise<void>, Promise<any>
│
└── Miscellaneous
    ├── Plugins
    │   ├── .plugin(fn) : Schema
    ├── Indexes
    │   ├── .createIndex() : Promise<void>
    └── Return Types: Schema, Promise<void>
```