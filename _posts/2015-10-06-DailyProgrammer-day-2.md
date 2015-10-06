---
published: true
layout: post
title: "[Dailyprogrammer] Challenge 2 - #235 [Easy] Ruth-Aaron Pairs"
date: 2015-10-06 13:24:18
summary: Practicing programming by solving challenge from r/DailyProgrammer
categories: Dailyprogrammer Challenge
---
## [r/DailyProgrammer](https://www.reddit.com/r/DailyProgrammer)

DailyProgrammer is a subreddit that provide challenge to programmer everyday. Each day i will solve one challenge from it to practice my skill on solving problem. 

### [2015-10-05] Challenge [#235](https://www.reddit.com/r/dailyprogrammer/comments/3nkanm/20151005_challenge_235_easy_ruthaaron_pairs/) [Easy] Ruth-Aaron Pairs

#### Description

In mathematics, a Ruth–Aaron pair consists of two consecutive integers (e.g. 714 and 715) for which the sums of the distinct prime factors of each integer are equal. For example, we know that (714, 715) is a valid Ruth-Aaron pair because its distinct prime factors are:

~~~~~~~~~~~~
714 = 2 * 3 * 7 * 17
715 = 5 * 11 * 13
~~~~~~~~~~~~

and the sum of those is both 29:

~~~~~~~~~
2 + 3 + 7 + 17 = 5 + 11 + 13 = 29
~~~~~~~~~

The name was given by Carl Pomerance, a mathematician at the University of Georgia, for Babe Ruth and Hank Aaron, as Ruth's career regular-season home run total was 714, a record which Aaron eclipsed on April 8, 1974, when he hit his 715th career home run. A student of one of Pomerance's colleagues noticed that the sums of the distinct prime factors of 714 and 715 were equal.
For a little more on it, see [MathWorld](http://mathworld.wolfram.com/Ruth-AaronPair.html)
Your task today is to determine if a pair of numbers is indeed a valid Ruth-Aaron pair.

#### Input Description

You'll be given a single integer N on one line to tell you how many pairs to read, and then that many pairs as two-tuples. For example:

~~~~~
3
(714,715)
(77,78)
(20,21)
~~~~~

#### Output Description

Your program should emit if the numbers are valid Ruth-Aaron pairs. Example:

~~~~~~~~
(714,715) VALID
(77,78) VALID
(20,21) NOT VALID
~~~~~~~~

#### Challenge Input

~~~~~~~~
4
(5,6) 
(2107,2108) 
(492,493) 
(128,129) 
~~~~~~~~

#### Challenge Input Solution

~~~~~~~~~~~
(5,6) VALID
(2107,2108) VALID
(492,493) VALID
(128,129) NOT VALID
~~~~~~~~~~~

## My solution

My solution for this is really simple i get input from command line. Then with each number in a tuple i turn it to an array of distinct prime factors then compare their sum. Note that i use Node.js 4.1.1 for this so the way to get input will be a little difference with your. And because this is simpler than the last one i will make another implementation with Python (And also because tuple is a type in Python so it obviously easier).

### Javascript
{% highlight js %}
var readline = require('readline');

var rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

var numTuple, tuples = [];
rl.question("Enter the number of tuples: ", function(number) {
    numTuple = number;
    process.stdout.write("Enter " + number + " tuple: \n");
    rl.prompt();
});

var i = 0;

rl.on('line', function(line) {
    tuples.push(line.slice(1, -1).split(',').map(function(num) {
        return parseInt(num);
    }));
    i++;
    if (i == numTuple)
        rl.close();
    else rl.prompt();
}).on('close', function() {
    tuples.forEach(function(tuple) {
        if (isRuthAaronPairs(tuple))
            console.log('(' + tuple + ') ' + 'VALID');
        else console.log('(' + tuple + ') ' + 'NOT VALID');
    });
});

function isRuthAaronPairs(tuple) {
    if (tuple[0] === NaN || tuple[1] === NaN) {
        console.log("This isn't a tuple");
        process.exit(0);
    } else {
        return (toPrimes(tuple[0]).reduce(sum) === toPrimes(tuple[1]).reduce(sum)) 
                && tuple[0] !== tuple[1];
    }
    function sum (a,b) {
        return a + b;
    }
}

function toPrimes(number) {
    var primes = [];
    var i = 2;
    if (number == 1)
        return [1];
    if (number < 0)
        throw new Error("You can not enter negative number");

    while (number > 1) {
        if (number % i == 0) {
            if (primes.length == 0 || primes[primes.length-1] !== i){
                primes.push(i);
            }
            number = Math.floor(number / i);
        } else {
            i++;
        }
    }

    return primes;
}
{% endhighlight %}

### Python 3

{% highlight python %}
from ast import literal_eval

def isRuthAaronPairs(tuple):
    a,b = tuple
    return sum(toPrimes(a)) == sum(toPrimes(b)) and a != b

def toPrimes(number):
    primes = []
    i = 2
    while number > 1:
        if number % i == 0:
            if len(primes) == 0 or primes[-1] != i:
                primes.append(i)
            number = int(number / i)
        else: 
            i += 1

    return primes

numTuple = int(input('Enter the number of tuples:'))
tuples = []
print("Enter {0} tuples: ".format(numTuple))
for i in range(0,numTuple):
    tuples.append(literal_eval(input()))

for tuple in tuples:
    if isRuthAaronPairs(tuple):
        print(tuple, "VALID")
    else:
        print(tuple, "NOT VALID")
{% endhighlight %}

