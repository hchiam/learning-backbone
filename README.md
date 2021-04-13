# Learning Backbone

Just one of the things I'm learning. <https://github.com/hchiam/learning>

Backbone.js is a library that you call, not a framework (like React/Angular) that calls you. Library: more flexibility. Framework: less boilerplate.

Backbone.js modules: Views. Events. Models. Collections. Routers.

Backbone "views" = "controllers" in MVC.

Backbone relies on underscore.js and a little on jQuery.

<https://adrianmejia.com/backbone-dot-js-for-absolute-beginners-getting-started/#start>

Tutorial steps as commits: <https://github.com/amejiarosario/Backbone-tutorial/commits/>

## [Views and Templates](https://adrianmejia.com/backbone-dot-js-for-absolute-beginners-getting-started/#start)

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

```js
// underscore template:
_.template(templateString, [data], [settings]);
```

and then `templatesString` placeholders:

- `<%= %>` = data, HTML escape _NOT_ allowed
- `<%- %>` = data, HTML escape allowed
- `<% %>` = can run any JS code

## [Models, Collections, and Views](https://adrianmejia.com/backbone-js-for-absolute-beginners-getting-started-part-2/)

<!-- TODO -->

## [Create, Read, Update, Delete](https://adrianmejia.com/backbonejs-for-absolute-beginners-getting-started-part-3/)

<!-- TODO -->

## [Routers](https://adrianmejia.com/backbone-js-for-absolute-beginners-getting-started-part-4/)

<!-- TODO -->
