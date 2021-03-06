# apisdk.js
##### Build a SDK from API document

### Get Started

```javascript
var api = new APISDK([
  // An API definition list here
  'POST /articles',
  'GET /articles/:id',
  'PUT /articles/:id',
  'DELETE /articles/:id'
], {
  'host': '/api',
  // 'promise' and 'http' MUST be provided
  'promise': Promise,
  'http': function(params) { console.log(params); }
});


var id = 123;
// { "method": "get", "url": "/api/articles/123", "data": { "token": 789 } }
api.articles(id).get({ token: 789 });

// Dynamic parameter is supported
var inc = 0;
var nextArticle = api.articles(function() { return inc++; });
// { method: "GET", url: "/api/articles/0", data: undefined }
nextArticle.get();
// { method: "GET", url: "/api/articles/1", data: undefined }
nextArticle.get();

// Asynchronous parameter is supportd
var asyncParam = new Promise(function(resolve){ setTimeout(resolve, 1000, 123); });

// The request will be launched after the promise resolved
api.articles(asyncParam).get();
```

### Friendly with RESTful API

```javascript
/**
 * GET /users/:user_id/
 * GET /users/:user_id/orders
 * POST /users/:user_id/orders
 * GET /users/:user_id/orders/:order_id
 * POST /users/:user_id/orders/:order_id/payment
 * POST /users/:user_id/orders/:order_id/rating
 * POST /users/:user_id/orders/:order_id/cancel
 * POST /users/:user_id/orders/:order_id/refunding
**/

var currentUserId = 123;

// Create a 'user' object for current user
var user = api.users(currentUserId);

// Get current user profile
user.get();

// Create an 'order' object
var order = user.orders(123);

// Get order information
order.get();

// Post a payment request for this order
order.payment.post();

// Post a rating for this order
order.rating.post();

// Post a order canceling request for this order
order.cancel.post();

// Post refunding request for this order
order.refunding.post();
```

### Install

```bash
bower install apisdk
```

```html
<script src="/bower_components/apisdk/apisdk.js"></script>
```
