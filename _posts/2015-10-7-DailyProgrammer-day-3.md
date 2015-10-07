---
published: true
layout: post
title: "[Dailyprogrammer] Challenge 3 - Challenge #234 [Intermediate] Red Squiggles"
date: 2015-10-07
summary: Practicing programming by solving challenge from r/DailyProgrammer
categories: Dailyprogrammer Challenge
---
## [r/DailyProgrammer](https://www.reddit.com/r/DailyProgrammer)

DailyProgrammer is a subreddit that provide challenge to programmer everyday. Each day i will solve one challenge from it to practice my skill on solving problem. 

### [2015-09-30] Challenge [#234](https://www.reddit.com/r/dailyprogrammer/comments/3n55f3/20150930_challenge_234_intermediate_red_squiggles/) [Intermediate] Red Squiggles

#### Description

Many of us are familiar with real-time spell checkers in our text editors. Two of the more popular editors Microsoft Word or Google Docs will insert a red squiggly line under a word as it's typed incorrectly to indicate you have a problem. (Back in my day you had to run spell check after the fact, and that was an extra feature you paid for. Real time was just a dream.) The lookup in a dictionary is dynamic. At some point, the error occurs and the number of possible words that it could be goes to zero.
For example, take the word foobar. Up until foo it could be words like foot, fool, food, etc. But once I type the b it's appearant that no words could possibly match, and Word throws a red squiggly line.
Your challenge today is to implement a real time spell checker and indicate where you would throw the red squiggle. For your dictionary use /usr/share/dict/words or the always useful [enable1.txt](http://norvig.com/ngrams/enable1.txt).

#### Input Description

You'll be given words, one per line. Examples:

~~~~~
foobar
garbgae
~~~~~

#### Output Description

Your program should emit an indicator for where you would flag the word as mispelled. Examples:

~~~~
foob<ar
garbg<ae
~~~~~
Here the \< indicates "This is the start of the mispelling". If the word is spelled correctly, indicate so.

#### Challenge Input

~~~~~~~~
accomodate
acknowlegement
arguemint 
comitmment 
deductabel
depindant
existanse
forworde
herrass
inadvartent
judgemant 
ocurrance
parogative
suparseed
~~~~~~~~

#### Challenge Input Solution

~~~~~~~~~~~
accomo<date
acknowleg<ement
arguem<int 
comitm<ment 
deducta<bel
depin<dant
exista<nse
forword<e
herra<ss
inadva<rtent
judgema<nt 
ocur<rance
parog<ative
supa<rseed
~~~~~~~~~~~

#### Note

When I run this on Linux's /usr/share/dict/words I get some slightly different output, for example the word "supari" is in OSX but not in enable1.txt. That might explain some of your differences at times.

## My solution

I read the dictionary from `/usr/share/dict/words` then make a copy of it in checkTypo function.
I use buffer string as an pointer so every time i read a new character in that word i will filter
the dictionary array with that buffer until it only have 1 word. Or if it do not contain any word then i will add the '<' to the buffer and add the rest of the word. Note that i use RegExp to check from the start of the word when the pointer still do not reach the end, and when it reach the end i check for the whole word.

### Javascript
{% highlight js %}
var fs = require('fs');

var challengeInput = ['accomodate',
    'acknowlegement',
    'arguemint',
    'comitmment',
    'deductabel',
    'depindant',
    'existanse',
    'forworde',
    'herrass',
    'inadvartent',
    'judgemant',
    'ocurrance', 
    'parogative', 
    'suparseed'
]

fs.readFile('/usr/share/dict/words', 'utf8', function(err, data) {
    var dict = data.split('\n');
    challengeInput.forEach(function(word){
        console.log(checkTypo(word, dict));
    });
});

function checkTypo(string, dictionary) {
    var buffer = '';
    var i = 1;
    var dict = dictionary.slice();
    while (dict.length > 1) {
        buffer = string.slice(0, i);
        dict = dict.filter(function(word) {
            if (i < string.length)
                return word.search(new RegExp('^' + buffer, 'i')) !== -1;
            else return word.search(new RegExp('^' + buffer + '$', 'i')) !== -1;
        });
        console.log(dict);
        if (dict.length == 0) {
            buffer += '<' + string.slice(i);
            break;
        }
        i++;
    }

    return dict.length == 1 ? string : buffer;
}{% endhighlight %}