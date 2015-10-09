---
layout: post
title: "[100 Days Angularjs] Day 5: Tutorial - 6 - Templating Links & Images"
date: 2015-10-09
summary: The 5th day of my 100 day challenge with AngularJS
categories: AngularJS
published: true
---


## Day 5 of 100 days with Angular

Today the lesson is really simple so will go fast through this. Tommorow i will do the Tik Tak Toe project.
The AI is really hard so i will have to research a lot of things. I am reading the Minimax algorithm hope that
in the end of the day i can finish the project with that algorithm.

### Data

#### `app/phones/phones.json` (sample snippet):
{% highlight json %}
[
  {
    ...
    "id": "motorola-defy-with-motoblur",
    "imageUrl": "img/phones/motorola-defy-with-motoblur.0.jpg",
    "name": "Motorola DEFY\u2122 with MOTOBLUR\u2122",
    ...
  },
  ...
]
{% endhighlight %}

### Template

#### `app/index.html:`

{% highlight html %}
...
        <ul class="phones">
          <li ng-repeat="phone in phones | filter:query | orderBy:orderProp" class="thumbnail">
            <a href="#/phones/{{phone.id}}" class="thumb"><img ng-src="{{phone.imageUrl}}"></a>
            <a href="#/phones/{{phone.id}}">{{phone.name}}</a>
            <p>{{phone.snippet}}</p>
          </li>
        </ul>
...
{% endhighlight %}

Use ng-src instead of src. End. :grin: . Nothing learned really.

### Test

#### `test/e2e/scenarios.js`:
{% highlight js %}
...
    it('should render phone specific links', function() {
      // select the element bind to model 'query'
      var query = element(by.model('query'));
      // enter 'nexus' to that element
      query.sendKeys('nexus');
      // select all `a` element inside a `li` that is inside a `div` with class `phones`
      // then click on the fisrt link
      element.all(by.css('.phones li a')).first().click();
      // check that the browser go to the correct url
      browser.getLocationAbsUrl().then(function(url) {
        expect(url).toBe('/phones/nexus-s');
      });
    });
...
{% endhighlight %}