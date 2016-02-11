# automatic-search-behavior

Search behavior responding to keys

## Install

```
$ bower install --save automatic-search-behavior
```

## Usage

We have to create an element that implements this behavior

```html

<link rel="import" href="bower_components/automatic-search-behavior/automatic-search-behavior.html">

<dom-module id="my-input">
  <template>
    <style>
      :host {
        display: block;
      }
    </style>
    <input id="input" type="text" placeholder="Your search here...">
  </template>
</dom-module>

<script>
  (function () {
    Polymer({
      is: 'my-input',
      //We use the behavior
      behaviors: [ASB.AutomaticSearchBehavior],
      properties: {
        //We need to include two properties
        //The first is the url where we want to make the request
        url: {
          type: String,
          value: '/'
        },
        //The second is the configuration of the behavior
        asbConfig: {
          value: function () {
            return: {
              //the node of the input where we are looking for change events
              node: this.$.input
            }
          }
        }
      }
    })
  })()
</script>

```

We can use it now in our app

```html
<!doctype html>
<html>
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <title>search-box</title>
  <!-- We import polymer  -->
  <link rel="import" href="bower_components/polymer/polymer.html">
  <!-- We import our new element -->
  <link rel="import" href="my-input.html">
</head>
<body>

  <template id="app" is="dom-bind">
    <!-- Note that I'm using a tiny backend. When you make a request to /users
    it return an array of users like the following:
    [{name: 'Edgar', last: 'Allan Poe'}, {name: 'Walt', last: 'Whitman'} ...] -->
    <my-input
      url="http://localhost:3000/users"
      last-response="{{res}}"
      on-waiting="waiting"
      on-response="handleResponse"></my-input>
    <template is="dom-repeat" items="{{res}}" as="model">
      <div>
        <span>name: {{model.name}}</span>
        <span>last: {{model.last}}</span>
      </div>
    </template>
    <div id="loading"></div>
  </template>

  <script>
    document.addEventListener('dom-change', function () {
      var app = document.getElementById('app');
      var loading = document.getElementById('loading');
      app.waiting = function () {
        loading.innerHTML = 'loading...';
      }
      app.handleResponse = function () {
        loading.innerHTML = '';
      }
    });
  </script>

</body>
</html>
```
