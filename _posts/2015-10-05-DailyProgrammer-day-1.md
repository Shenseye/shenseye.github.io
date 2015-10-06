---
published: true
layout: post
title: "[Dailyprogrammer] Challenge 1 - #234 [Easy] Vampire Numbers"
date: 2015-10-05 16:46:18
summary: Practicing programming by solving challenge from r/DailyProgrammer
categories: Dailyprogrammer Challenge
---
## [r/DailyProgrammer](https://www.reddit.com/r/DailyProgrammer)

DailyProgrammer is a subreddit that provide challenge to programmer everyday. And from now on each day i will solve one challenge from it to practice my skill on solving problem. 

### [2015-09-28] Challenge [#234](https://www.reddit.com/r/dailyprogrammer/comments/3moxid/20150928_challenge_234_easy_vampire_numbers/) [Easy] Vampire Numbers

#### Description

A vampire number v is a number v=xy with an even number n of digits formed by multiplying a pair of n/2-digit numbers (where the digits are taken from the original number in any order) x and y together. Pairs of trailing zeros are not allowed. If v is a vampire number, then x and y are called its "fangs."

**EDIT FOR CLARITY** Vampire numbers were original 2 two-digit number (fangs) that multiplied to form a four digit number. We can extend this concept to an arbitrary number of two digit numbers. For this challenge we'll work with three two-digit numbers (the fangs) to create six digit numbers with the same property - we conserve the digits we have on both sides of the equation.
Additional information can be found [here](http://www.primepuzzles.net/puzzles/puzz_199.htm)

#### Input Description

Two digits on one line indicating n, the number of digits in the number to factor and find if it is a vampire number, and m, the number of fangs. Example: `4 2`

#### Output Description

A list of all vampire numbers of n digits, you should emit the number and its factors (or "fangs"). Example:

~~~
1260=21*60
1395=15*93
1435=41*35
1530=51*30
1827=87*21
2187=27*81
6880=86*80
~~~

#### Challenge Input

`6 3`

#### Challenge Input Solution

~~~
114390=41*31*90
121695=21*61*95
127428=21*74*82
127680=21*76*80
127980=20*79*81
137640=31*74*60
163680=66*31*80
178920=71*90*28
197925=91*75*29
198450=81*49*50
247680=40*72*86
294768=46*72*89
376680=73*60*86
397575=93*75*57
457968=56*94*87
479964=74*94*69
498960=99*84*60
~~~

## My solution

I come up with this brute force algorithm using some help on stackoverflow about [make a nested loop with array](http://stackoverflow.com/questions/426878/is-there-any-way-to-do-n-level-nested-loops-in-java) from 'user2478398'. It is really over complicated and slow but it work.

{% highlight js %}
function vampireNumber(n, m) {

    if (n % m != 0){
        console.log("No Vampire Number like that exist.");
        return;
    } else {
        var numLen = n / m;
    }

    var maxNum = '';

    for (var i = 0; i < numLen; i++) {
        maxNum+='9';
    };

    maxNum = parseInt(maxNum);

    findVampireNumber(m, maxNum, n).forEach(function(vam) {
        console.log(vam);
    });
}

function findVampireNumber(n, max, len){
    var indices = new Array(n),
        ranges = new Array(n),
        answer = [],
        vamNums = [];

    for (var i = 0; i < n; i++) {
        ranges[i] = max + 1;
        indices[i] = 1;
    };

    do {
        var vamNum = indices.reduce(function(a, b) {
            return a * b;
        });
        var vamStr = vamNum + '=' + indices.join('*');
        if ((vamNum).toString().length == len 
                && onlyOneEndWithZero(indices) && isVampireNumber(vamNum,indices)
                && vamNums.indexOf(vamNum) == -1){
            answer.push(vamStr);
            vamNums.push(vamNum);
        }
        turn();
    } while (!allMax());

    return answer.sort();

    function turn() {
        for (var i = n - 1; i >= 0; i--) {
            if (indices[i] + 1 == ranges[i])
                indices[i] = 1;
            else {
                ++indices[i];
                break;
            }
        };
    }

    function allMax() {
        for(var i=n-1; i>=0; i--) {
            if(indices[i] != ranges[i]-1) {
                return false;
            }
        }
        return true;
    }
}

function isVampireNumber (number, indices) {
    return number.toString().split('').sort().join('') == indices.join('').split('').sort().join('');
}

function onlyOneEndWithZero(indices){

    var onlyOne = 1;

    indices.forEach(function(arg) {
        if (arg % 10 == 0)
            --onlyOne;
    });

    return onlyOne < 0 ? false : true;
}

console.log("Test with 4 and 2");
vampireNumber(4, 2);

console.log("Test with 6 and 3");
vampireNumber(6, 3);
{% endhighlight %}