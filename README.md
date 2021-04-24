# Learning Backbone

Just one of the things I'm learning. <https://github.com/hchiam/learning> and <https://github.com/hchiam/learning-frameworks>

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

## [Models, Collections, Views, Templates, and Events](https://adrianmejia.com/backbone-js-for-absolute-beginners-getting-started-part-2/)

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
// app.todoList = new app.TodoList();

collection.create({
  title: "Learn Backbone's Collection",
  // completed: false by default
});

collection.add(someModel);
console.log(collection.pluck("title"));
console.log(JSON.stringify(collection));
```

### `Backbone.View` and Templates:

Template:

```html
<script type="text/template" id="item-template">
  <div class="view">
    <input class="toggle" type="checkbox">
    <label><%- title %></label>
  </div>
</script>
```

View:

```js
app.TodoView = Backbone.View.extend({
  tagName: "li",
  template: _.template($("#item-template").html()),
  render: function () {
    this.$el.html(this.template(this.model.toJSON()));
    return this; // enable chained calls
  },
});

var view = new app.TodoView({ model: todo });
```

### `Backbone.Events`: `on`, `off`, and `trigger`:

**Delegated event**:

`{"<EVENT_TYPE> <ELEMENT_ID>": "<CALLBACK_FUNCTION>"}`

Example: `events: {'keypress #new_todo': 'createTodoOnEnter'}`

Equivalent in jQuery: `.on('keypress', '#new_todo', createTodoOnEnter)`

Example:

```js
app.AppView = Backbone.View.extend({
  // ...
  events: {
    "keypress #new-todo": "createTodoOnEnter",
  },
  createTodoOnEnter: function (e) {
    var hitEnterKey = e.which !== 13;
    var emptyInput = !this.input.val().trim();
    if (hitEnterKey || emptyInput) {
      return;
    }
    app.todoList.create(this.newAttributes());
    this.input.val("");
  },
});
app.appView = new app.AppView();
```

**`Backbone.Events`**: <https://backbonejs.org/#Events>

Example built-in Backbone.js event: `add` is triggered when a `Backbone.Model` is added to a `Backbone.Collection`: `todoList.on("add", this.handleAddEvent, this);`

Example built-in Backbone.js event: `reset` is triggered when bulk replacing all the `Backbone.Model`s in a `Backbone.Collection`.

```js
var context = this;
todoList.on("add", this.handleAddEvent, context);
```

```js
var arbitraryObject = {};
var customEventHandler = function (msg) {
  console.log("Triggered " + msg);
};

_.extend(arbitraryObject, Backbone.Events);

arbitraryObject.on("my_custom_event", customEventHandler);

arbitraryObject.trigger("my_custom_event", "my custom event");
```

```js
app.AppView = Backbone.View.extend({
  // ...
  initialize: function () {
    app.todoList = new app.TodoList();
    app.todoList.on("add", this.addOne, this);
  },
  addOne: function (todo) {
    // ...
  },
});
app.appView = new app.AppView();
```

## [Create, Read, Update, Delete](https://adrianmejia.com/backbonejs-for-absolute-beginners-getting-started-part-3/)

Create: (see earlier sections).

Read: (see earlier sections).

Update: `this.model.save({prop: value})`

Delete: `this.model.destroy()`

## [Backbone.Router](https://adrianmejia.com/backbone-js-for-absolute-beginners-getting-started-part-4/)

`todos/:id` --> parameter `id`

`file/*path` --> `*path` matches everything onwards, so is always used as the last matcher

```js
app.Router = Backbone.Router.extend({
  // some-path/:filter
  routes: {
    ":filter": "setFilter", // setFilter is a callback function!
  },
  setFilter: function (params) {
    // (maybe hide/show stuff like a single-page app)
  },
});

app.router = new app.Router();
Backbone.history.start();
app.appView = new app.AppView();
```
