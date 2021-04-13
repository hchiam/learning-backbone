# Learning Backbone

Just one of the things I'm learning. <https://github.com/hchiam/learning>

Backbone.js is a library that you call, not a framework (like React/Angular) that calls you. Library: more flexibility. Framework: less boilerplate.

Backbone.js modules: Views. Events. Models. Collections. Routers.

Backbone "views" = "controllers" in MVC.

Backbone relies on underscore.js and a little on jQuery.

<https://adrianmejia.com/backbone-dot-js-for-absolute-beginners-getting-started/#start>

## [Views and Templates](https://adrianmejia.com/backbone-dot-js-for-absolute-beginners-getting-started/#start)

Tutorial steps as commits: <https://github.com/amejiarosario/Backbone-tutorial/commits/>

```js
var AppView = Backbone.View.extend({
  el: "#container", // "root"
  template: _.template("<h3>Hello <%= who %></h3>"),
  initialize: function () {
    this.render();
  },
  render: function () {
    // $el is a cached jQuery object you can use jQuery on
    this.$el.html(this.template({ who: "world!" }));
  },
});

var appView = new AppView();
```

More properties: `tagName`, `className`, `id`, `attributes`. If none of these nor `el` is specified, then the `this.el` falls back to an empty `div`.

```js
// underscore template:
_.template(templateString, [data], [settings]);
```

and then `templatesString` placeholders:

- `<%= %>` = data, HTML escape _NOT_ allowed
- `<%- %>` = data, HTML escape allowed
- `<% %>` = can run any JS code

## [Models, Collections, and Views](https://adrianmejia.com/backbone-js-for-absolute-beginners-getting-started-part-2/)

### `Backbone.Model`:

```js
var app = {}; // namespace

app.Todo = Backbone.Model.extend({
  defaults: {
    title: "",
    completed: false,
  },
});

var someModelInstance = new app.Todo({
  title: "Learn Backbone.js",
  completed: false,
});

console.log(someModelInstance.get("title"));
someModelInstance.set("some_new_property", Date());
console.log(someModelInstance.get("some_new_property"));
```

### `Backbone.Collection`:

A "collection" in Backbone.js is an ordered set of models. Basically treat a models as "sub-models", with get (`pluck('propName')`), set (`add(someModel)`), listeners, and even `fetch()` for each model in the collection.

```js
app.TodoList = Backbone.Collection.extend({
  model: app.Todo,
  localStorage: new Store("backbone-todo"),
});

var collection = new app.TodoList();

collection.create({
  title: "Learn Backbone's Collection",
  // completed: false by default
});

collection.add(someModel);
console.log(collection.pluck("title"));
console.log(JSON.stringify(collection));
```

## [Create, Read, Update, Delete](https://adrianmejia.com/backbonejs-for-absolute-beginners-getting-started-part-3/)

<!-- TODO -->

## [Routers](https://adrianmejia.com/backbone-js-for-absolute-beginners-getting-started-part-4/)

<!-- TODO -->
