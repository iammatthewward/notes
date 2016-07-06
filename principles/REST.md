# REST and RESTful APIs

REST stands for **RE**presentational **S**tate **T**ransfer and is a software architectural style designed as a way to organise your software architecture. REST is all about organising your routes, and keeping your routes within a widely used convention to allow a unified way of accessing web resources. Creating RESTful APIs allows other simplicity in access for other developers and systems.

REST is primarily used in the context of HTTP requests, and is tied very closely to [CRUD](https://github.com/iammatthewward/notes/blob/master/principles/CRUD.md) actions.

HTTP is a stateless protocol. We don't maintain any state between the client and the server, it's just a send and response. Inside that response is data that represents the state of your application. What you see on your screen is the data stored in your application.

### RESTful conventions for interacting with a resource

#### Creating new
* POST /resources

#### Viewing all
* GET /resources

#### Viewing record
* GET /resources/:ID

#### Updating record
* PUT /resources/:ID

#### Deleting record
* DELETE /resources/:ID
