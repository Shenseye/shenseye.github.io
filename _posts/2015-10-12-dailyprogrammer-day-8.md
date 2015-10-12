---
layout: post
title: "[Dailyprogrammer] Challenge 8 - Challenge #236 [Easy] Random Bag System"
date: 2015-10-12 22:31
summary: Practicing programming by solving challenge from r/DailyProgrammer
categories: Dailyprogrammer Challenge
published: true
---

## [r/DailyProgrammer](https://www.reddit.com/r/DailyProgrammer)

DailyProgrammer is a subreddit that provide challenge to programmer everyday. Each day i will solve one challenge from it to practice my skill on solving problem. 

### [2015-10-12] Challenge [#236](https://www.reddit.com/r/dailyprogrammer/comments/3ofsyb/20151012_challenge_236_easy_random_bag_system/) [Easy] Random Bag System

#### Description

Contrary to popular belief, [the tetromino pieces](http://i.imgur.com/65G37Aq.png) you are given in a game of [Tetris](https://en.wikipedia.org/wiki/Tetris) are not randomly selected. Instead, all seven pieces are placed into a "bag." A piece is randomly removed from the bag and presented to the player until the bag is empty. When the bag is empty, it is refilled and the process is repeated for any additional pieces that are needed.
In this way, it is assured that the player will never go too long without seeing a particular piece. It is possible for the player to receive two identical pieces in a row, but never three or more. Your task for today is to implement this system.

#### Input Description

None.

##### Output Description

Output a string signifying 50 tetromino pieces given to the player using the random bag system. This will be on a single line.
The pieces are as follows:

- `O`
- `I`
- `S`
- `Z`
- `L`
- `J`
- `T`

##### Example Input

None.


##### Example Output

- `LJOZISTTLOSZIJOSTJZILLTZISJOOJSIZLTZISOJTLIOJLTSZO`
- `OTJZSILILTZJOSOSIZTJLITZOJLSLZISTOJZTSIOJLZOSILJTS`
- `ITJLZOSILJZSOTTJLOSIZIOLTZSJOLSJZITOZTLJISTLSZOIJO`

##### Note

Although the output is semi-random, you can verify whether it is likely to be correct by making sure that pieces do not repeat within chunks of seven.

##### Bonus

Write a function that takes your output as input and verifies that it is a valid sequence of pieces.

##### My solution

This simple is really simple. I just need to make an array of those pieces. And then make a copy 
of them and use it as a pool to randomly pick one of them. Every time i run out of pieces 
i will make a new copy. And with the validate function i just do the same thing.

##### Javascript

{% highlight js %}
var pool = ['O','I','S','Z','L','J','T'];

function createPiecesString(strLen, pool){
    var poolCopy = pool.slice();
    var retStr = '';

    while(poolCopy.length > 0){
        var randomNum = poolCopy.length > 1 ? Math.floor(Math.random() * poolCopy.length) : 0;
        retStr += poolCopy[randomNum];
        poolCopy.splice(randomNum, 1 )
        if (retStr.length == 50)
            break;
        if (poolCopy.length == 0)
            poolCopy = pool.slice();
    }

    return retStr;
};

function validatePieces(piecesString, pool) {
    var i = 0;
    var j = 1;
    var poolCopy = pool.slice();
    while (i < piecesString.length){
        if (i < j * pool.length){
            if (poolCopy.indexOf(piecesString[i]) !== -1){
                poolCopy.splice(poolCopy.indexOf(piecesString[i]), 1);
            } else
                return false;
            i++;
        } else {
            if (poolCopy.length !== 0)
                return false;
            else{
                poolCopy = pool.slice();
                j++;
            }
        }
    }
    return true;
};

var answer = createPiecesString(50, pool);

console.log(answer);
console.log(validatePieces(answer, pool));
{% endhighlight %}

