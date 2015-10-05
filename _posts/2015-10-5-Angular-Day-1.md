---
layout: post
title: "[100 Days Angularjs] Day 1 - Start with AngularJS Official Tutorial"
date: 2015-10-05 12:32:18
summary: Start my 100 day challenge with AngularJS
categories: AngularJS
published: true
---


## Day 1 of 100 days with Angular
 
I am starting to learn Angularjs so i set a challenge for myself to make a post per day to update what i learned in that day. Hope in the end of this challenge i will have a good understanding about Angularjs.
 
### So here is my first day
 
I will start learning Angularjs with [AngularJS Official Tutorial](https://docs.angularjs.org/tutorial)
 
I'm already done the lessons 0,1,2  so i'm gonna run straight to the lessons 3
 
### Lesson 3 - Filtering Repeaters
 
![angular-filter-chart]({{site.baseurl}}https://docs.angularjs.org/img/tutorial/tutorial_03.png)
 
Really simple and easy concept. ng-model initialize a new variable name query in the scope of the controller and sync it with the value of the input. And in the ng-repeat for each phone in phones pipe (or pass) it to the filter. The filter create a new array contain all the values that match the query variable.
 
#### E2E test
 
I have zero understanding of this so i will comment on each line of the code to translate it to english. 
 
{% highlight js %}
// Make a new test suit with the name 'PhoneCat App'
describe('PhoneCat App', function() {
  // Make a new test suit with the name 'Phone list view'
  describe('Phone list view', function() {
    // before each 'it' open this address 'app/index.html'
    beforeEach(function() {
      browser.get('app/index.html');
    });
 
    // define a new spec
    it('should filter the phone list as a user types into the search box', function() {
      // get all element by repeater. I think it get all the element with ng-repeat equal 'phone in phones'
      var phoneList = element.all(by.repeater('phone in phones'));
      // get the element has ng-model equal 'query'
      var query = element(by.model('query'));
      // literally expect the number of elements in phoneList to equal 3
      expect(phoneList.count()).toBe(3);
      
      // enter 'nexus' to the input
      query.sendKeys('nexus');
      // expect the number of elements in phoneList to equal 1
      expect(phoneList.count()).toBe(1);
      
      // clear input
      query.clear();
      // enter 'motorola to input'
      query.sendKeys('motorola');
      // expect the number of elements in phoneList to equal 2
      expect(phoneList.count()).toBe(2);
    });
  });
});
{% endhighlight %}

#### Some note on the Experiments
 
- The scope of the controller or app only live in the element where it is defined. If you use it outside it root then it will not work.
- Use ng-bind-template='template' to replace the element text in side element with the template 
- Use .toMatch(/regex/) to match string using regular expression



