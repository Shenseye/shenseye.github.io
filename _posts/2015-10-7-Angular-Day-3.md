---
layout: post
title: "[100 Days Angularjs] Day 3 - Wikipedia Search"
date: 2015-10-07
summary: The 3th day of my 100 day challenge with AngularJS
categories: AngularJS
published: true
---


## Day 3 of 100 days with Angular

I spent my whole day study at school and doing [this project](http://codepen.io/chotmat/full/MamqEd/) from [freecodecamp.com](freecodecamp.com) so i don't have time to learn the tutorial. So i will so you what i learn this day from doing this project.
 
 
### Project Zipline - Wikipedia Search:

#### Cross-origin resource sharing (CORS) Problem:

When i was trying to get the json from Wikipedia API using `$http.get` like this:

{% highlight js %}
  app.service('wikiaSearchAPI', ['$http', function ($http) {
    return {
        searchFor: searchFor
    };

    function searchFor(title) {
        return $http.get('https://en.wikipedia.org/w/api.php?action=opensearch&format=json&search=' + encodeURIComponent(title));
    }
  }]);
{% endhighlight %}

It returned this error in the console: 

![]({{site.baseurl}}/images/Screenshot-2015-10-07.png)

I found out that this is a server problem because it do not support CORS so i have to find another way
to get the JSON. So after half an hour i found out that if i use `$http.jsonp` with the extra parameter `callback=JSON_CALLBACK` i wouldn't have to use CORS so i fixed that problem.

{% highlight js %}
  app.service('wikiaSearchAPI', ['$http', function ($http) {
    return {
        searchFor: searchFor
    };

    function searchFor(title) {
        return $http.jsonp('https://en.wikipedia.org/w/api.php?action=opensearch&format=json&callback=JSON_CALLBACK&search=' + encodeURIComponent(title));
    }
  }]);
{% endhighlight %}


#### Binding model to Controller of a directive:

So i have a input like this `<input id="search-box" ng-model="titleQuery" type="text" placeholder="Search for...">` and i have to bind it into the scope of my directive but it this model live in a different scope so i have to find a way to do it. And i found it, this is my directive at that time:

{% highlight js %}
app.directive('wikia', ['wikiaSearchAPI', function (wikiaSearchAPI) {
    return {
        restrict: 'E',
        scope: {
            titleQuery: '='
        },
        template: "<li ng-repeat='result in wikiaCtrl.searchresults' ng-show='wikiaCtrl.titleQuery' >"
                    + "<div class='result-card'>"
                        + "<a ng-href='{{ result.link }}'>" 
                            + "<h3>{{ result.name }}</h3>" 
                        + "</a>"
                        + "<p>{{ result.summary }}</p>" 
                    + "</div>"
                  + "</li>",
        controller: function($scope){
            var ctrl = this;
            ctrl.searchresults = [];
            ctrl.titleQuery = $scope.titleQuery || ''
            wikiaSearchAPI.searchFor(ctrl.titleQuery).then(function (response) {
                for (var i = 0; i < response.data[1].length; i++) {
                    ctrl.searchresults.push({
                        name: response.data[1][i],
                        summary: response.data[2][i],
                        link: response.data[3][i]
                    });
                }
            });
        },
        controllerAs: 'wikiaCtrl'
    };
}]);
{% endhighlight %}

I quickly found out that something is not right. It do not update the ctrl.searchresults. Maybe there is 
something wrong with the binding. I ask some guy in the Angular Buddie slack team and know that '=' in 
scope mean pass by reference and '@' is for passing string and it support {{}} expressions and there also a 
'&' one that pass function. And he also point out that my code supposed to be properly bounded. Because 
when he add this <span>&#123;&#123;ctrl.titleQuery&#125;&#125;</span> to the template and it reflect the model change immediately. So
the one that do not work here is the service. It do not know that the model was changed. I have to 
use a watcher for that. And now the code work properly.

{% highlight js %}
app.directive('wikia', ['wikiaSearchAPI', function (wikiaSearchAPI) {
    return {
        restrict: 'E',
        scope: {
            titleQuery: '='
        },
        template: "<li ng-repeat='result in wikiaCtrl.searchresults' ng-show='wikiaCtrl.titleQuery' >"
                    + "<div class='result-card'>"
                        + "<a ng-href='{{ result.link }}'>" 
                            + "<h3>{{ result.name }}</h3>" 
                        + "</a>"
                        + "<p>{{ result.summary }}</p>" 
                    + "</div>"
                  + "</li>",
        controller: function($scope){
            var ctrl = this;
            $scope.$watch('titleQuery',function(newTitle){
                ctrl.searchresults = [];
                ctrl.titleQuery = newTitle || ''
                wikiaSearchAPI.searchFor(ctrl.titleQuery).then(function (response) {
                for (var i = 0; i < response.data[1].length; i++) {
                    ctrl.searchresults.push({
                        name: response.data[1][i],
                        summary: response.data[2][i],
                        link: response.data[3][i]
                    });
                }
            });
            }, true);
        },
        controllerAs: 'wikiaCtrl'
    };
}]);
{% endhighlight %}

#### The end result

After playing with some styling and animation (.ng-enter, .ng-enter-active) here is the end
result:

![]({{site.baseurl}}/images/wikia-search.png)

You can see it live at this [codepen](http://codepen.io/chotmat/full/MamqEd/)