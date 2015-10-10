---
layout: post
title: "[100 Days Angularjs] Day 6: Tutorial - 7 - Routing & Multiple Views"
date: 2015-10-10 22:16
summary: The 6th day of my 100 day challenge with AngularJS
categories: AngularJS
published: true
---


## Day 6 of 100 days with Angular

Yesterday i said that i will do a Tik Tak Toe Game but i really don't have time for that. So i will delay that project a 
little bit. Anyway the lesson today from the tutorial is pretty long so i will not go into details.


### Template

#### `app/index.html`:

{% highlight html %}
<!doctype html>
<html lang="en" ng-app="phonecatApp">
<head>
...
  <script src="bower_components/angular/angular.js"></script>
  <script src="bower_components/angular-route/angular-route.js"></script>
  <script src="js/app.js"></script>
  <script src="js/controllers.js"></script>
</head>
<body>

  <div ng-view></div>

</body>
</html>
{% endhighlight %}

The ngView directive will load the rendered template of the current route using the $route service to the main page.
So basically just like this jekyll page :smile:. All specific html are move to this file `app/partials/phone-list.html`.


#### The App Module
{% highlight js %}
app/js/app.js:
...
phonecatApp.config(['$routeProvider',
  function($routeProvider) {
    $routeProvider.
      when('/phones', {
        templateUrl: 'partials/phone-list.html',
        controller: 'PhoneListCtrl'
      }).
      when('/phones/:phoneId', {
        templateUrl: 'partials/phone-detail.html',
        controller: 'PhoneDetailCtrl'
      }).
      otherwise({
        redirectTo: '/phones'
      });
  }]);
{% endhighlight %}

Note here: 
- $routeProvider service is used for set up the route
- $routeProvider.when('\route:parameter', templateUrl, controller): When the path is `route` use template from `templateUrl`
  and `controller` to set up the page. The parameter is specify using the `:` simbol.
- $routeProvider.otherwise() redirect the page to some route when all route didn't match.

#### E2E Test

{% highlight js %}
...
   it('should redirect index.html to index.html#/phones', function() {
    // go to 'app/index.html' then check if it redirect correctly by getting the url
    browser.get('app/index.html');
    browser.getLocationAbsUrl().then(function(url) {
        expect(url).toEqual('/phones');
      });
  });

  describe('Phone list view', function() {
    beforeEach(function() {
      // go to 'app/index.html#/phones'
      browser.get('app/index.html#/phones');
    });
...

  describe('Phone detail view', function() {

    beforeEach(function() {
      // go to 'app/index.html#/phones/nexus-s'
      browser.get('app/index.html#/phones/nexus-s');
    });


    it('should display placeholder page with phoneId', function() {
      expect(element(by.binding('phoneId')).getText()).toBe('nexus-s');
    });
  });
{% endhighlight %}