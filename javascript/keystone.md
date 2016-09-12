# Keystone

## Project structure
```
|--app
|  Static files (css, js, images, etc.) that are publicly available
|--import
|  Import data json files to populate page and model data in the DB
|--keystone
|  Contains index file to set up route bindings
|  |--config
|  |  Config files which specify database details, init processes, view engine and view location. Also connects nav to data population and migration scripts found in updates directory
|  |--db
|  |  Database queries as functions
|  |--middleware
|  |  Middleware scripts
|  |--routing
|  |--views
|  |  View controllers which gather relevant db data from db queries and create and populate the view
|--models
|  Your application's database models
|  scripts
|  Bash scripts
|--templates
|  |--views
|  |  Your application's view templates
|  |  |--helpers
|  |  |  Helper functions for views
|  |  |--layouts
|  |  |--Default handlebars template that views are loaded into as partials
|  |  |--partials
|  |  |--Partial templates referenced from main view templates
|  |--theme
|  |  Base theme files
|--updates
|  Data population and migration scripts
|--package.json
|  Project configuration for npm
|--keystone.js
|  Main script that starts your application
```

## Steps for adding a page

### 1. Create any relevant models in the `models` directory
Models do not need to be exported, but do need to be registered with Keystone. Models can broadly follow the below template, with any changes made as necessary.

Relationships between models are also defined here, and are created by adding relevant fields with a relationship type. In the below example, a relationship to the Post model is set up as a field called 'posts'. The `type` parameter sets the type as a relationship and the `ref` parameter sets the related model. Many other relationship options exist.

`posts: { type: Types.Relationship, ref: 'Post' }`

**Full model example**
```
var keystone = require('keystone');
var Types = keystone.Field.Types;

/**
 * *ModelName* Model
 * ==========
 */

// Create the new model in Keystone
var *ModelName* = new keystone.List('*ModelName*');

// Add all fields to model
*ModelName*.add({
    name: { type: Types.Name, required: true},
    email: { type: Types.Email, initial: true, required: true, index: true },
    date: {type: Types.Date},
    text: {type: String}
});

// Add any default columns
*ModelName*.defaultColumns = 'name';

// Register the model with the above details
*ModelName*.register();
```

### 2. Create a database query function in the `keystone/db` directory
In order to pass model data from the database to views via a controller, database query functions must be set up to query the correct data. This is done by creating and exporting a function (that may or may not take any number of arguments) which returns data from the database.

Query functions use Mongoose queries to get data from the db. In the below example, the query looks at all `Post` data, and returns posts where the author matches the authorId passed in when the query is called. The author is also populated, allowing us to get author data via each returned post. The returned data can also be sorted, and a limit can be set on how many items will be returned.

```
var keystone = require('keystone');

exports = module.exports = function (authorId) {

    //
    return keystone.list('Post')
        .model.find()
        .where('author', authorId)
        .populate('author')
        .sort('-publishedAt')
        .limit(5)
};

```

### 3. Create a controller in the `keystone/routing/views` directory

```
var keystone = require('keystone');

// Require the base page database query, and any required for the page
var getPage = require('../../db/page');
var *modelData* = require('../../db/*fileName*');

exports = module.exports = function (req, res) {

    // Create a new view
    var view = new keystone.View(req, res);
    // Initialises the standard view locals
    var locals = res.locals;

    // Defines an anonymous function that is run on init
    view.on('init', function (next) {

        // Load the model data from the database query
        *modelData*().exec(function (err, result) {
            // Stores data from the response in the locals as the name of the model data
            locals.data.*modelData* = result;
            console.log(result);
            // Call the next method when all methods are complete, including any callbacks
            next();
        });

    });

    // Load page content
    view.on('init', function (next) {

        getPage('*page-name*').exec(function (err, result) {
            console.log('have we found a page');
            console.log(result);
            // Stores the data from the response in the locals as content
            locals.content = result;
            next(err);
        })

    });

    // Renders the view when all data has been returned
    view.render('about-us-2');
};
```

### 4. Add the route binding into the `keystone/routing/index.js` file
All controllers are imported into this file, and as new ones are made they need to bound to their route. To do this, add a new line into the `    // Views` section like so: `app.get('/route-slug', routes.views.*routeName*);`

### 5. Create the handlebars template in the `templates/views` directory
This view template is what is referenced in the controller at the `view.render('*template-name*');` line. Typically create routes that are made up of partials, with each view and partial having their own `.scss` file and `.js` file if needed. `.scss` and `.js` files should be created and stored in their own folders within the `app/js` and `app/scss` directories respectively, and should be included in the `bundle` files located in the root of each of these directories.
