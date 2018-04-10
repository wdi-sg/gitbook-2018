# Database Hooks (Mongoose or Sequelize)

Hooks allow us to perform actions at different points throughout the life cycle of a model. For example, we could do something before an item is updated or after an item is created.

Most ORMs will support some form of hooks, including both [Mongoose](http://mongoosejs.com/docs/middleware.html) and [Sequelize](http://sequelize.readthedocs.org/en/latest/docs/hooks/)


**Example: normalize tags using beforeCreate**

If we want all of our tags to be stored in all caps we could use a beforeCreate hook. This way if someone adds a tag `Taco` it will be stored as `TACO` and all of our tags will be the same. The example below shows this code in Mongoose.

```js
//in models/tag.js -- tag model

//... some other model code above

tagSchema.pre('save', function(next) {
  var tag = this
  tag.name = tag.name.toUpperCase();
  next();
});

//... some other model code below

```

We can use pre save hooks to automatically hash a users password before it is created (AKA before the data is inserted into the database).
