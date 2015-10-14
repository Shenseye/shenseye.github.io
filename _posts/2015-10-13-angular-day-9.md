---
layout: post
title: "[100 Days Angularjs] Day 9: Tutorial - 10 - Event Handlers"
date: 2015-10-13 14:17
summary: The 9th day of my 100 day challenge with AngularJS
categories: AngularJS
published: true
---

## Day 9 of 100 days with Angular

So i notice that because they still don't teach the Controller As syntax so i have to inject the 
global `$scope` into my controller to public some property from that controller. And about event
handler because i have some background with C# so i know that an event handler is just some function that subscribe to some event. And from what i see here it is the same concept.

### Controller

#### `app/js/controllers.js`:

{% highlight js %}
...
var phonecatControllers = angular.module('phonecatControllers',[]);

phonecatControllers.controller('PhoneDetailCtrl', ['$scope', '$routeParams', '$http',
  function($scope, $routeParams, $http) {
    $http.get('phones/' + $routeParams.phoneId + '.json').success(function(data) {
      $scope.phone = data;
      $scope.mainImageUrl = data.images[0];
    });

    $scope.setImage = function(imageUrl) {
      $scope.mainImageUrl = imageUrl;
    };
  }]);
{% endhighlight %}

So here they public the setImage function to the global scope so we can use it in the template.

### Template
app/partials/phone-detail.html:

<img ng-src="{{mainImageUrl}}" class="phone">

...

<ul class="phone-thumbs">
  <li ng-repeat="img in phone.images">
    <img ng-src="{{img}}" ng-click="setImage(img)">
  </li>
</ul>
...

And here is where we subscribe that event. So everytime the click event is fired we will run this
function with `img` as an argument. And it will set the current `mainImageUrl` to that `img` variable.

