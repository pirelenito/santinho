# Santinho

The simple Node.js authorisation framework **currently just in the idea form**.

Heavily inspired (if not translated) from [Pundit](https://github.com/elabs/pundit).


## Policies

A policy is defined as a simple constructor function that takes a user as a parameter.

```js
function ArticlePolicy (user) {
  this.user = user;
}
```

The policy can then define action permissions. Each **action must return a promise**.

The promise should be resolved with the 'processed' parameter in case of permission allowed, or rejected in case of permission denied.

```js
ArticlePolicy.prototype = {
  canList: function (query) {
    return new Promise(function () {
      // here you can change the query to filter only articles of the user
    });
  },

  canGet: function (model) {
    return new Promise(function () {
      // here you might check if the article belongs to the user
    });
  },

  canCreate: function (attributes) {
    return new Promise(function () {
      // checks if the user can create the Article
      // changes the attributes to make sure it doesn't do anything nasty
    });
  }
};
```

## Authorise

To use it, first you need to get a instance with some registered policies. The library matches the policies with resources using naming conventions.

```js
var santinho = new Santinho([ArticlePolicy, CategoryPolicy, PersonPolicy]),
    authorise = santinho.authorise;
```

Then you can use the `authorise` function directly in something like your controllers.


To authorise a query to a given action:

```js
authorise(user).to('list', Article.count()).then(function (scopedQuery) {

});
```

To get a model:

```js
authorise(user).to('get', model).then(function (model) {

});
```


To create a Article with a given set of attributes.

```js
authorise(user).to('create', Article, attributes).then(function (sanitisedAttributes) {

});
```
