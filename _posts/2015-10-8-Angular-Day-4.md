---
layout: post
title: "[100 Days Angularjs] Day 4: Tutorial - 5 - XHRs & Dependency Injection"
date: 2015-10-08
summary: The 4th day of my 100 day challenge with AngularJS
categories: AngularJS
published: true
---


## Day 4 of 100 days with Angular

It is pretty late here when i post this so i kinda sleepy now :sleepy:. But i have to keep up with my challenge.
Never drop it down for any reason or i will lost my momentum.

### Controller

`app/js/controllers.js:`
{% highlight js %}
var phonecatApp = angular.module('phonecatApp', []);

phonecatApp.controller('PhoneListCtrl', function ($scope, $http) {
  $http.get('phones/phones.json').success(function(data) {
    $scope.phones = data;
  });

  $scope.orderProp = 'age';
});
{% endhighlight %}

If you read my last post you will notice that i already know how to use $http.get and $http.jsonp
so i will not dive deep in to this. But i still learn something about injector here. That because the 
Angular infers the controller's dependencies from the names of arguments to the controller's constructor 
function so if i minify the file it will cause problem. So there is two way to fix this:

- Create a $inject property on the controller function which holds an array of strings. Each string in the array is the name of the service to inject for the corresponding parameter. In our example we would write:
{% highlight js %}
function PhoneListCtrl($scope, $http) {...}
PhoneListCtrl.$inject = ['$scope', '$http'];
phonecatApp.controller('PhoneListCtrl', PhoneListCtrl);
{% endhighlight %}

- Use an inline annotation where, instead of just providing the function, you provide an array. This array contains a list of the service names, followed by the function itself.
{% highlight js %}
function PhoneListCtrl($scope, $http) {...}
phonecatApp.controller('PhoneListCtrl', ['$scope', '$http', PhoneListCtrl]);
{% endhighlight %}

### Unit test

{% highlight js %}
describe('PhoneCat controllers', function() {

    describe('PhoneListCtrl', function(){
        var scope, ctrl, $httpBackend;

        // Load our app module definition before each test.
        beforeEach(module('phonecatApp'));

        // The injector ignores leading and trailing underscores here (i.e. _$httpBackend_).
        // This allows us to inject a service but then attach it to a variable
        // with the same name as the service in order to avoid a name conflict.
        // Inject these service before each test
        beforeEach(inject(function(_$httpBackend_, $rootScope, $controller) {
        $httpBackend = _$httpBackend_;
        // make a mockup server it will return the parameter in respond each time a
        // get request 'phones/phones.json' fire.
        $httpBackend.expectGET('phones/phones.json').
            respond([{name: 'Nexus S'}, {name: 'Motorola DROID'}]);
        // Create a new scope for controller
        scope = $rootScope.$new();
        ctrl = $controller('PhoneListCtrl', {$scope: scope});
    }));

    it('should create "phones" model with 2 phones fetched from xhr', function() {
        // Expect that the phone model do not exist
        expect(scope.phones).toBeUndefined();
        // Flush the mockup backend server so it return the response
        $httpBackend.flush();

        expect(scope.phones).toEqual([{name: 'Nexus S'},{name: 'Motorola DROID'}]);
    });

    it('should set the default value of orderProp model', function() {
        // verify that the default value of orderProp is set correctly
        expect(scope.orderProp).toBe('age');
    });
...
{% endhighlight %}