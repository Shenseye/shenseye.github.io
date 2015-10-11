---
published: true
layout: post
title: "[Dailyprogrammer] Challenge 7 - #231 [Intermediate] Set Game Solver"
date: 2015-10-11 18:44
summary: Practicing programming by solving challenge from r/DailyProgrammer
categories: Dailyprogrammer Challenge
---

## [r/DailyProgrammer](https://www.reddit.com/r/DailyProgrammer)

DailyProgrammer is a subreddit that provide challenge to programmer everyday. Each day i will solve one challenge from it to practice my skill on solving problem. 

### [2015-09-09] Challenge [#211](https://www.reddit.com/r/dailyprogrammer/comments/3ke4l6/20150909_challenge_231_intermediate_set_game/) [Intermediate] Set Game Solver

#### Description

Set is a card game where each card is defined by a combination of four attributes: shape (diamond, oval, or squiggle), color (red, purple, green), number (one, two, or three elements), and shading (open, hatched, or filled). The object of the game is to find sets in the 12 cards drawn at a time that are distinct in every way or identical in just one way (e.g. all of the same color). From Wikipedia: A set consists of three cards which satisfy all of these conditions:
- They all have the same number, or they have three different numbers.
- They all have the same symbol, or they have three different symbols.
- They all have the same shading, or they have three different shadings.
- They all have the same color, or they have three different colors.

The rules of Set are summarized by: If you can sort a group of three cards into "Two of ____ and one of _____," then it is not a set.
See [the Wikipedia page for the Set game](http://en.wikipedia.org/wiki/Set_(game)) for for more background.

#### Input Description

A game will present 12 cards described with four characters for shape, color, number, and shading: (D)iamond, (O)val, (S)quiggle; (R)ed, (P)urple, (G)reen; (1), (2), or (3); and (O)pen, (H)atched, (F)illed.

##### Output Description

Your program should list all of the possible sets in the game of 12 cards in sets of triplets.

##### Example Input

~~~~~
SP3F
DP3O
DR2F
SP3H
DG3O
SR1H
SG2O
SP1F
SP3O
OR3O
OR3H
OR2H
~~~~~


##### Example Output

~~~~~
SP3F SR1H SG2O
SP3F DG3O OR3H
SP3F SP3H SP3O
DR2F SR1H OR3O
DG3O SP1F OR2H
DG3O SP3O OR3O
~~~~~

##### Challenge Input

~~~~~
DP2H
DP1F
SR2F
SP1O
OG3F
SP3H
OR2O
SG3O
DG2H
DR2H
DR1O
DR3O
~~~~~

##### Challenge Output

~~~~~
DP1F SR2F OG3F
DP2H DG2H DR2H 
DP1F DG2H DR3O 
SR2F OR2O DR2H 
SP1O OG3F DR2H 
OG3F SP3H DR3O      
~~~~~


##### My solution

This challenge is a perfect fit for the new Set type in ES6. A Set is a type like array but can't
hold two element with the same value in it. You can think of it like they store and get by value 
not by index. So what i done here is loop through any possible combinations and then check if it
is a set or not.

##### Javascript

{% highlight js %}
var readline = require('readline');

var rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

console.log('Enter cards (Crtl + D to end): ');

rl.prompt();

var cards = [];

rl.on('line', function(card){
    cards.push(card);
    rl.prompt();
}).on('close', function(){
    console.log(solveSet(cards).sort().join('\n'));
});

function solveSet(cards){
    var sets = [];
    for (var i = 0; i < cards.length; i++) {
        for (var j = i; j < cards.length; j++) {
            for (var k = j; k < cards.length; k++) {
                if (i != j && j != k && i != k){
                    var isASet = true;
                    for (var l = 0; l < 4; l++) {
                        var set = new Set().add(cards[i][l]).add(cards[j][l]).add(cards[k][l]);
                        if (set.size == 2)
                            isASet = false;
                    };

                    if (isASet) {
                        sets.push(cards[i] + ' ' + cards[j] + ' ' + cards[k]);
                    };
                }
            };
        };
    };
    return sets;
}
{% endhighlight %}