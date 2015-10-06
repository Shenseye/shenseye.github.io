---
layout: post
title: "[100 Days Angularjs] Day 2 - Two-way Data Binding"
date: 2015-10-06 12:32:18
summary: The second day of my 100 day challenge with AngularJS
categories: AngularJS
published: true
---


## Day 2 of 100 days with Angular
 
Now i will learn the lesson 4 from [AngularJS Official Tutorial](https://docs.angularjs.org/tutorial)
 
### Lesson 4 - Two-way Data Binding

#### Template `app/index.html`:
{% highlight html %}
Search: <input ng-model="query">
Sort by:
<select ng-model="orderProp">
  <option value="name">Alphabetical</option>
  <option value="age">Newest</option>
</select>


<ul class="phones">
  <li ng-repeat="phone in phones | filter:query | orderBy:orderProp">
    <span>{{phone.name}}</span>
    <p>{{phone.snippet}}</p>
  </li>
</ul>
 {% endhighlight %}

![angular-filter-chart]({{site.baseurl}}https://docs.angularjs.org/img/tutorial/tutorial_04.png)
 
From my understanding what they done here is that they create a two-way binding between the `orderProp`model and the value in select. So when one changed the other one will also change. And then they basically tell the ng-repeat to orderBy the value of `orderProp` by chaining (or pipe - from my linux experience point of view) orderBy to the statement.

#### Controller `app/js/controllers.js`:

{% highlight js %}
var phonecatApp = angular.module('phonecatApp', []);

phonecatApp.controller('PhoneListCtrl', function ($scope) {
  $scope.phones = [
    {'name': 'Nexus S',
     'snippet': 'Fast just got faster with Nexus S.',
     'age': 1},
    {'name': 'Motorola XOOM™ with Wi-Fi',
     'snippet': 'The Next, Next Generation tablet.',
     'age': 2},
    {'name': 'MOTOROLA XOOM™',
     'snippet': 'The Next, Next Generation tablet.',
     'age': 3}
  ];

  $scope.orderProp = 'age';
});
{% endhighlight %}

So here we defined a array of phone in the scope of the controller and each of them will have a `age` property so we can sort by it. And it also set the default value of orderProp to `age` so we don't have a blank selection when we enter the page.
 

#### Unit Test `test/unit/controllersSpec.js`:

{% highlight js %}
// define a test suit name 'PhoneCat controllers'
describe('PhoneCat controllers', function() {
  // define a test suit name 'PhoneListCtrl'
  describe('PhoneListCtrl', function(){
    var scope, ctrl;
    // before each 'it' test load the module 'PhonecatApp'
    beforeEach(module('phonecatApp'));
    // before each 'it' test load the inject the $controller service to our test
    // end then create a new instance of 'PhoneListCtrl' and provide it an new and empty scope
    beforeEach(inject(function($controller) {
      scope = {};
      ctrl = $controller('PhoneListCtrl', {$scope:scope});
    }));
    // expect the number of phones to be equal 3
    it('should create "phones" model with 3 phones', function() {
      expect(scope.phones.length).toBe(3);
    });

    // expect the default value of orderProp to be properly set up
    it('should set the default value of orderProp model', function() {
      expect(scope.orderProp).toBe('age');
    });
  });
});
{% endhighlight %}

#### E2E test `test/e2e/scenarios.js`:
  
{% highlight js %}
...
// verify the thing below
it('should be possible to control phone order via the drop down select box', function() {
  // get all elements that come from repeater 'phone in phones' and in the column. What is a column
  // anyway, will need to ask someone about this.
  var phoneNameColumn = element.all(by.repeater('phone in phones').column('phone.name'));
  // get the element with model 'query'
  var query = element(by.model('query'));
  // define function getNames that return an array of texts from each element
  function getNames() {
    return phoneNameColumn.map(function(elm) {
      return elm.getText();
    });
  }
  // Enter 'tablet' to the query input
  query.sendKeys('tablet'); //let's narrow the dataset to make the test assertions shorter
  // expect the array that getNames return to equal this array
  expect(getNames()).toEqual([
    "Motorola XOOM\u2122 with Wi-Fi",
    "MOTOROLA XOOM\u2122"
  ]);
  // get element by the model 'orderProp' ( I do a search and found out that this element() function 
  // return a promise that resolve to element). the get the element in side that element that is a 
  // option element and has attribute value equal "name" (I think it using css selecter) then 
  // click on it
  element(by.model('orderProp')).element(by.css('option[value="name"]')).click();
  // expect the array that getNames return to equal this array
  expect(getNames()).toEqual([
    "MOTOROLA XOOM\u2122",
    "Motorola XOOM\u2122 with Wi-Fi"
  ]);
});...
{% endhighlight %}

#### Some note on the Experiments
 
- If we don't set the default value for `orderProp` then it will become blank and then list will be unordered
- We can reverse the order by adding - (negative sign to the value)



