---
layout: post
title: "[100 Days Angularjs] Day 8: Tutorial - 9 - Filters"
date: 2015-10-12 22:11
summary: The 8th day of my 100 day challenge with AngularJS
categories: AngularJS
published: true
---

## Day 8 of 100 days with Angular

So the thing i learned today from the Angular tutorial is how to make a custom Filter. Let's 
break it down.

### Custom Filter

So in order to create a new filter, i have to make a new module and register a filter with this module:

#### `app/js/filters.js`:

{% highlight js %}
angular.module('phonecatFilters', []).filter('checkmark', function() {
  return function(input) {
    return input ? '\u2713' : '\u2718';
  };
});
{% endhighlight %}

So this filter convert boolean input from true to (\u2713 -> ✓) or false to (\u2718 -> ✘).

Really simple though. So a filter is basically a function that take thing piped ( | ) to it as 
argument and then return something base on that argument. I think that all. Now we can use that 
filter like anyother build in filter just remember to set it's module as a dependency of or app
and use `inject(function(filterModuleName) { /* doing something with the filter */ })` to use the 
filter in Unit test.

