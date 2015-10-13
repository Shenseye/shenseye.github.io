---
layout: post
title: "[Dailyprogrammer] Challenge 9 - #235 [Intermediate] Scoring a Bowling Game"
date: 2015-10-13 18:20
summary: Practicing programming by solving challenge from r/DailyProgrammer
categories: Dailyprogrammer Challenge
published: true
---

## [r/DailyProgrammer](https://www.reddit.com/r/DailyProgrammer)

DailyProgrammer is a subreddit that provide challenge to programmer everyday. Each day i will solve one challenge from it to practice my skill on solving problem. 

### [2015-10-07] Challenge [#235](https://www.reddit.com/r/dailyprogrammer/comments/3ntsni/20151007_challenge_235_intermediate_scoring_a/) [Intermediate] Scoring a Bowling Game

#### Description

The game of bowling is pretty simple: you have ten pins arranged in a triangle, and you roll a ball down a slick alley towards them and try and knock as many down as possible. In most frames (see below about the tenth frame) you get two attempts per "frame" before the remaining pins are cleared.
The bowler is allowed 10 frames in which to knock down pins, with frames one (1) through nine (9) being composed of up to two rolls. The tenth frame may be composed of up to three rolls: the bonus roll(s) following a strike or spare in the tenth (sometimes referred to as the eleventh and twelfth frames) are fill ball(s) used only to calculate the score of the mark rolled in the tenth.
Bowing scoring is a bit tricky (which is why this is an intermediate challenge). In addition to a gutter ball (which is 0 pins), you have strikes and spares as well as 1 to 9 pins being knocked down. Strikes and spares affect the next balls in different ways.
When all ten pins are knocked down with the first ball of a frame (called a strike and typically rendered as an "X" on a scoresheet), a player is awarded ten points, plus a bonus of whatever is scored with the next two balls. In this way, the points scored for the two balls after the strike are counted twice.
A "spare" is awarded when no pins are left standing after the second ball of a frame; i.e., a player uses both balls of a frame to clear all ten pins. A player achieving a spare is awarded ten points, plus a bonus of whatever is scored with the next ball (only the first ball is counted). It is typically rendered as a slash on scoresheets in place of the second pin count for a frame.
Your challenge today is to determine a bowling score from a score sheet.

#### Input Description

You'll be given a bowling sheet for a single person on a line, with the ten frames separated by a space. All scores are single characters: `-` for zero (aka a gutter ball), `1-9` for 1-9 pins knocked down, `/` for a spare, and `X` for a strike. If you see any two digit numbers (e.g. "63") that's actually the first and second ball scores (a six then a three). Example:

`X X X X X X X X X XXX`

##### Output Description

Your program should calculate the score for the sheet and report it. From our example:

`300`

Aka a perfect game.

##### Challenge Input

~~~
X -/ X 5- 8/ 9- X 81 1- 4/X
62 71  X 9- 8/  X  X 35 72 5/8
~~~

##### Challenge Output

~~~~
137
140
~~~~

##### Bonus ASCII Art

~~~~

                         ! ! ! !
                      ." ! ! !  /
                    ."   ! !   /
                  ."     !    /
                ."           /
              ."     o      /
            ."             /
          ."              /
        ."               /
      ."      . '.      /
    ."      '     '    /
  ."                  / grh
."     0 |           /
       |/
      /|
      / |
~~~~

##### My solution

I split the bowling sheet into a array of frames. For each frame if it is a `X` or contain `/`
make a new array contain all character from that frame including it self and then extract the 
amount of ball (character) needed after `X` or `/` (also including that `X` or `/`). And then 
pass them into `score` to caculate the score. For normal frame i just split them into array and pass to `score`.

##### Javascript

{% highlight js %}
var readline = require('readline');

var rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

var bowlingSheet, frameScores = [];

rl.question("Enter the bowling sheet: ", function(answer){
    bowlingSheet = answer.split(' ');
    rl.close();
});

rl.on('close', function(){
    for (var i = 0; i < bowlingSheet.length; i++) {
        if (bowlingSheet[i] == 'X'){
            var balls = bowlingSheet.slice(i).join('').split('');
            var twoBall = balls.splice(balls.indexOf('X'), 3);
            frameScores.push(score(twoBall));
        } else if (bowlingSheet[i].indexOf('/') != -1){
            var balls = bowlingSheet.slice(i).join('').split('');
            var oneBall = balls.splice(balls.indexOf('/'), 2);
            frameScores.push(score(oneBall));
        } else {
            var balls = bowlingSheet[i].split('');
            frameScores.push(score(balls));
        }
    };

    var finalScore = frameScores.reduce(function(a, b){
        return a + b;
    });

    console.log(frameScores);
    console.log(finalScore);
});

function score(balls){
    var score = 0;
    balls.forEach(function(ball){
        if (ball == 'X' || ball == '/')
            score+=10;
        else if (ball == '-')
            score+=0;
        else score+= parseInt(ball);
    });

    return score;
}
{% endhighlight %}

