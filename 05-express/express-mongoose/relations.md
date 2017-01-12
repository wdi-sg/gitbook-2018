# Mongoose Model Relationships
In Mongo there are two ways to modeling related data, and we can use both in Mongoose:

- via __embedding__
- via __referencing__ (linking)

## Sub-documents (MongoDB embedded documents)
Sub-documents are just what they sound like: documents with their own schemas nested in other documents. They take of the form of objects within an array.  You can think of this as a sort of `has_many` relationship - the context to use embedded documents is data entities need to be used/viewed in context of another.

Let's look at these two schemas below - we can embed `childSchema` into the property `children`:

```javascript
const childSchema = new mongoose.Schema({ name: 'string' });

const parentSchema = new mongoose.Schema({
  children: [childSchema]
});

const Parent = mongoose.model('Parent', parentSchema);

Parent.create({ children: [
  { name: 'Matt' },
  { name: 'Sarah' }
]}, function(err, parent) {
  if (err) {
    console.log(err);
    return;
  }
  console.log(parent);
});
```

#### Finding a sub-document

All documents in Mongoose have an `_id`, including sub-documents. Using the example above, we can find a specific sub-document from the array by using the `.id` function on the key we want to search.

```js
// in our first example, this should return one of the sub-documents
const doc = parent.children.id(idYouAreLookingFor);
```

#### Adding and Removing sub-documents

Mongoose sub-document collections are arrays, and therefore contain methods like as `.push()`, `.pop()`, and `.unshift()`.

```js
parent.children.push({ name: 'Ester' }); // pushes Ester
parent.children.pop(); // pops Ester
```

**Note that these functions don't save the instance, so you'll need to call `.save()` manually afterwards.**

## Population (MongoDB document references)

Storing references to other documents involves defining a specific model to reference, as well as the type of what's being stored. For example, referring to a User in a Book model would involve referencing the User model, as well as storing the user's `ObjectId`.

```js
const Schema = mongoose.Schema;

const bookSchema = Schema({
  author: { type: Schema.Types.ObjectId, ref: 'User' },
  title: String
});

const Book = mongoose.model('Book', bookSchema);

// creating a book and storing an author's id
Book.create({ title: 'Fahrenheit 451', author: author._id }, function(err, book) {
  // access book here
});
```

However, if we query for a book, the author's information won't automatically populate. The query would give back the `ObjectId` only! In order to populate the model's data, we can use the `.populate()` function after the query, as well as use `.exec()` to execute the callback function.

```js
Book.findOne({ title: 'Fahrenheit 451' })
.populate('author')
.exec(function(err, book) {
  // access book with author data here
});
```
