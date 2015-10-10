---
published: true
layout: post
title: "[Dailyprogrammer] Challenge 6 - #213 [Easy] Cellular Automata: Rule 90"
date: 2015-10-10 21:52
summary: Practicing programming by solving challenge from r/DailyProgrammer
categories: Dailyprogrammer Challenge
---

## [r/DailyProgrammer](https://www.reddit.com/r/DailyProgrammer)

DailyProgrammer is a subreddit that provide challenge to programmer everyday. Each day i will solve one challenge from it to practice my skill on solving problem. 

### [2015-09-07] Challenge [#213](https://www.reddit.com/r/dailyprogrammer/comments/3jz8tt/20150907_challenge_213_easy_cellular_automata/) [Easy] Cellular Automata: Rule 90

#### Description

The development of cellular automata (CA) systems is typically attributed to Stanisław Ulam and John von Neumann, who were both researchers at the Los Alamos National Laboratory in New Mexico in the 1940s. Ulam was studying the growth of crystals and von Neumann was imagining a world of self-replicating robots. That’s right, robots that build copies of themselves. Once we see some examples of CA visualized, it’ll be clear how one might imagine modeling crystal growth; the robots idea is perhaps less obvious. Consider the design of a robot as a pattern on a grid of cells (think of filling in some squares on a piece of graph paper). Now consider a set of simple rules that would allow that pattern to create copies of itself on that grid. This is essentially the process of a CA that exhibits behavior similar to biological reproduction and evolution. (Incidentally, von Neumann’s cells had twenty-nine possible states.) Von Neumann’s work in self-replication and CA is conceptually similar to what is probably the most famous cellular automaton: Conways “Game of Life,” sometimes seen as a screen saver. CA has been pushed very hard by Stephen Wolfram (e.g. Mathematica, Worlram Alpha, and "A New Kind of Science").
CA has a number of simple "rules" that define system behavior, like "If my neighbors are both active, I am inactive" and the like. The rules are all given numbers, but they're not sequential for historical reasons.
The subject rule for this challenge, Rule 90, is one of the simplest, a simple neighbor XOR. That is, in a 1 dimensional CA system (e.g. a line), the next state for the cell in the middle of 3 is simply the result of the XOR of its left and right neighbors. E.g. "000" becomes "1" "0" in the next state, "100" becomes "1" in the next state and so on. You traverse the given line in windows of 3 cells and calculate the rule for the next iteration of the following row's center cell based on the current one while the two outer cells are influenced by their respective neighbors. Here are the rules showing the conversion from one set of cells to another:

| "111" | "101" | "010" | "000" | "110" | "100" | "011" | "001" |
|:------|:-----:|------:|------:|------:|------:|------:|------:|
|   0   |   0   |   0   |   0   |   1   |   1   |   1   |   1   |

#### Input Description

You'll be given an input line as a series of 0s and 1s. Example:
`1101010`

##### Output Description

Your program should emit the states of the celular automata for 25 steps. Example from above, in this case I replaced 0 with a blank and a 1 with an X:

~~~~
xx x x
xx    x
xxx  x
x xxx x
  x x
 x   x
~~~~

##### Challenge Input

`00000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000`

##### Challenge Output

I chose this input because it's one of the most well known, it yields a Serpinski triangle, a well known fractcal.

~~~~~

                                             x
                                            x x
                                           x   x
                                          x x x x
                                         x       x
                                        x x     x x
                                       x   x   x   x
                                      x x x x x x x x
                                     x               x
                                    x x             x x
                                   x   x           x   x
                                  x x x x         x x x x
                                 x       x       x       x
                                x x     x x     x x     x x
                               x   x   x   x   x   x   x   x
                              x x x x x x x x x x x x x x x x
                             x                               x
                            x x                             x x
                           x   x                           x   x
                          x x x x                         x x x x
                         x       x                       x       x
                        x x     x x                     x x     x x
                       x   x   x   x                   x   x   x   x
                      x x x x x x x x                 x x x x x x x x
                     x               x               x               x
                    x x             x x             x x             x x
                    
~~~~~

## My solution

my solution for this is add a `0` bit at the start and the end of the series and then loop from 1 to n - 1. And then 
simply using the Description code `newBit = oldBitLeft XOR oldBitRight`. End loop when print 25 times or all bits turn to 0.

### Javascript

{% highlight js %}
var readline = require('readline');

var rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

var series;

rl.question("Enter the series: ", function(answer) {
  series = answer.split('').map(function(bit) {
      return parseInt(bit);
  });
  rl.close();
});
rl.on('close', function() {
    series.unshift(0);
    series.push(0);

    for (var i = 0; i <= 25; i++) {
        printSeries(series);
        var tempSeries = series.slice();
        for (var j = 1; j < series.length -1; j++) {
            series[j] = tempSeries[j-1] ^ tempSeries[j+1];
        };
        if (!series.some(function(bit){ return bit == 1}))
            break;
    };
});

function printSeries(series) {
    var seriesStr = '';
    for (var i = 1; i < series.length - 1; i++) {
        seriesStr += series[i] == 0 ? ' ' : 'x';
    };

    console.log(seriesStr);
}
{% endhighlight %}