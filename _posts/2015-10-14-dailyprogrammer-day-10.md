---
layout: post
title: "[Dailyprogrammer] Challenge 10 - #236 [Intermediate] Fibonacci-ish Sequence"
date: 2015-10-14 19:29
summary: Practicing programming by solving challenge from r/DailyProgrammer
categories: Dailyprogrammer Challenge
published: true
---

## [r/DailyProgrammer](https://www.reddit.com/r/DailyProgrammer)

DailyProgrammer is a subreddit that provide challenge to programmer everyday. Each day i will solve one challenge from it to practice my skill on solving problem. 

### [2015-10-14] Challenge [#236](https://www.reddit.com/r/dailyprogrammer/comments/3opin7/20151014_challenge_236_intermediate_fibonacciish/) [Intermediate] Fibonacci-ish Sequence

#### Description

[The Fibonacci Sequence](https://en.wikipedia.org/wiki/Fibonacci_number) is a famous integer series in the field of mathematics. The sequence is recursively defined for n > 1 by the formula `f(n) = f(n-1) + f(n-2)`. In plain english, each term in the sequence is found by adding the previous two terms together. Given the starting values of `f(0) = 0` and `f(1) = 1` the first ten terms of the sequence are:
`0 1 1 2 3 5 8 13 21 34`
We will notice however that some numbers are left out of the sequence and don't get any of the fame, 9 is an example. However, if we were to start the sequence with a different value for f(1) we will generate a new sequence of numbers. Here is the series for `f(1) = 3`:
`0 3 3 6 9 15 24 39 102 165`
We now have a sequence that contains the number 9. What joy!
Today you will write a program that will find the lowest positive integer for f(1) that will generate a Fibonacci-ish sequence containing the desired integer (let's call it x).

#### Input Description

Your input will be a single positive integer x.
Sample Input 1: `21`
Sample Input 2: `84`

##### Output Description

The sequence of integers generated using the recursion relation starting from 0 and ending at the desired integer x with the lowest value of f(1).
Sample Output 1: `0 1 1 2 3 5 8 13 21`
Sample Output 2: `0 4 4 8 12 20 32 52 84`

##### Challenge Input

Input 1: `0`
Input 2: `578`
Input 3: `123456789`

##### Notes/Hints

Large inputs (such as input 3) may take some time given your implementation. However, there is a relationship between sequences generated using f(1) > 1 and the classic sequence that can be exploited.

##### Bonus

Make your program run as fast as possible.

##### My solution

From the hint i noticed that `Fib-ish(i) = Fib(i)*Fib-ish(1)` so i just loop from 0 to input number and use it as `Fib-ish(1)` and generate a Fib-ish sequence from that. If the last number of that Fib-ish sequence equal input number i will return that. Because it take `14556ms` to process the `123456789`, i have to reduce runtime by checking if `inputNumber/Fib-ish(1)` is a Fibonacci number or not. If yes then i will generate the Fib-ish sequence if no then move to another number. And now the runtime is `375ms`, 40x faster pretty good right :smile:.

Here is the output and runtime:

~~~
0: [0] 14ms
578: [ 0, 17, 17, 34, 51, 85, 136, 221, 357, 578 ] 14ms
12345678: [ 0, 4115226, 4115226, 8230452, 12345678 ] 74ms;
123456789: [ 0, 41152263, 41152263, 82304526, 123456789 ] 375ms
~~~

##### Javascript

{% highlight js %}
var readline = require('readline');

var rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

var number;

rl.question('Enter the number: ', function(answer){
    number = parseInt(answer);
    rl.close();
});

rl.on('close', function(){
    console.time('generate-fib');
    console.log(generateFibIsh(number) || "Can't generate Fibonacci-ish number from this number.");
    console.timeEnd('generate-fib');
});

function generateFib(n){
    if (n < 2)
        return n;
    return generateFib(n - 1) + generateFib(n - 2);
}

function isPerfectSquare(number) {
    return Math.sqrt(number) == Math.floor(Math.sqrt(number));
}

function isPib(number){
    return isPerfectSquare(5*number*number + 4) || isPerfectSquare(5*number*number - 4);
}

function generateFibIsh(number){
    for (var i = 1; i < number; i++) {
        if (number % i == 0 && isPib(number/i)){
            var j = 0;
            var temp = 0;
            var fibIshSeq = [];
            while (temp < number){
                temp = generateFib(j) * i;
                fibIshSeq.push(temp);
                j++;
            }
            if (fibIshSeq[fibIshSeq.length -1] == number)
                return fibIshSeq;
        }
    };
    return number == 0 ? [0] : false;
}
{% endhighlight %}

