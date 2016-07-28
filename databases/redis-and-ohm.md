# Redis and Ohm

## Redis
Redis is a key - value store, rather than a relational database (like Postgres). However Redis is more than just a key - value store. It also supports:
  * Lists, sets and hashses
  * Persistence (snapshot and log-based modes)
  * Pub/Sub mechanism
  * Clustering, partitioning and high availability


## Ohm
Ohm is used for object-hash mapping for Redis (NOT an ORM), working on two facets:
  * Managing objects in the Ruby memory space
  * Managing Redis hashes, indices, and other data structures that correspond to those Ruby objects

### Attributes
The attribute class macro stores attributes of a model as a string (lambdas can be passed as a second argument to change the type:

    `attribute :age, lambda { |x| x.to_i }`)

To set the value of an attribute, call the `update` method on it:

    `user.update(email: 'email@gmail.com')`

To get the value of an attribute, use dot notation. This gets the value from the locally cached object:

    `user.email`

To get the value of an attribute directly from the Redis store, use the `get` method, passing in a key:

    `user.get(:email)`

### Indexing
In order to make an attribute searchable, it must be indexed. This is done with the index class macro:

    `index :l_name`

To search for an object by its searchable attribute, the `find` method can be used. This returns an `Ohm::Set` that can be interacted with in similar ways to Ruby arrays:

    `Author.find(l_name: "Bloggs").size`

### Uniques
An attribute can be set as unique using the `unique` class macro:

    `unique :email`

### Model Creation
New model objects can be created with the `create` class method, as with Active Record:

    `Model.create(first_attr: ‘Hello’, second_attr: ‘goodbye’`

This returns an object with a hash of attributes.
An id is also assigned to the object, equivalent to a primary key.

### Deleting Models
An object can be deleted by calling the `delete` class method on it. This will delete it from the Redis store, however by default it will not delete it from the application's memory space. This means a deleted object can be reanimated if the `update` method is called on it, re-adding it to the Redis store.

### Associations
Object relationships are defined using the `collection` and `reference` macros.
* `collection` is the equivalent of the `has_many` relationship in Active Record.
* `reference` is the similar to the `has_a` or `belongs_to` relationship in Active Record.

When `reference` is used on a model, an id attribute for the referenced object is added, a foreign key reference to the referenced object.
