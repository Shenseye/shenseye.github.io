---
layout: post
title: "[100 Days Angularjs] Day 8: Tutorial - 10 - Event Handlers"
date: 2015-10-13 14:17
summary: The 9th day of my 100 day challenge with AngularJS
categories: AngularJS
published: false
---

## Day 8 of 100 days with Angular

So the thing i learned today from the Angular tutorial is how to make a custom Filter. Let's 
break it down.

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



### E2E Test

#### `test/e2e/scenarios.js`:

{% highlight js %}
...
  describe('Phone detail view', function() {

...

    it('should display the first phone image as the main phone image', function() {
      expect(element(by.css('img.phone')).getAttribute('src')).toMatch(/img\/phones\/nexus-s.0.jpg/);
    });


    it('should swap main image if a thumbnail image is clicked on', function() {
      element(by.css('.phone-thumbs li:nth-child(3) img')).click();
      expect(element(by.css('img.phone')).getAttribute('src')).toMatch(/img\/phones\/nexus-s.2.jpg/);

      element(by.css('.phone-thumbs li:nth-child(1) img')).click();
      expect(element(by.css('img.phone')).getAttribute('src')).toMatch(/img\/phones\/nexus-s.0.jpg/);
    });
  });
{% endhighlight %}

### Unit Test

#### `test/unit/controllersSpec.js`:

...
  beforeEach(module('phonecatApp'));

...

 describe('PhoneDetailCtrl', function(){
    var scope, $httpBackend, ctrl,
        xyzPhoneData = function() {
          return {
            name: 'phone xyz',
            images: ['image/url1.png', 'image/url2.png']
          }
        };


    beforeEach(inject(function(_$httpBackend_, $rootScope, $routeParams, $controller) {
      $httpBackend = _$httpBackend_;
      $httpBackend.expectGET('phones/xyz.json').respond(xyzPhoneData());

      $routeParams.phoneId = 'xyz';
      scope = $rootScope.$new();
      ctrl = $controller('PhoneDetailCtrl', {$scope: scope});
    }));


    it('should fetch phone detail', function() {
      expect(scope.phone).toBeUndefined();
      $httpBackend.flush();

      expect(scope.phone).toEqual(xyzPhoneData());
    });
  });
{% endhighlight %}

