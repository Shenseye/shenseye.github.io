---
published: true
layout: post
title: "[Dailyprogrammer] Challenge 4 - #230 [Easy] JSON treasure hunt"
date: 2015-10-08
summary: Practicing programming by solving challenge from r/DailyProgrammer
categories: Dailyprogrammer Challenge
---
## [r/DailyProgrammer](https://www.reddit.com/r/DailyProgrammer)

DailyProgrammer is a subreddit that provide challenge to programmer everyday. Each day i will solve one challenge from it to practice my skill on solving problem. 

### [2015-09-30] Challenge [#230](https://www.reddit.com/r/dailyprogrammer/comments/3j3pvm/20150831_challenge_230_easy_json_treasure_hunt/) [Easy] JSON treasure hunt

#### Description

One of the most common ways for computers to communicate with each other today, especially over the internet, is a format known as JSON. JSON stands for JavaScript Object Notation and has it's origins in JavaScript, but nowadays libraries exist to parse it in pretty much every language in common use. JSON is a pretty great tool for this, because it is very compact and easily parsable, yet it's also very easy for humans to read and write.
There are 6 different fundamental types in JSON:
null, which usually signifies the absense of a value
A number, like `1, 4, 5` or `2.718281828` (JSON makes no distinction between integers and floats)
A boolean, either `true` or `false`
A string, some number of characters between quotation marks: `"hello world"`, for instance
A list, which is an ordered list of JSON values: `[1, 3.14, [null, "popsicle"], false]` for instance.
An "object", which is an unordered list of key-value pairs. The keys are always strings, and the values can be any JSON object: `{"key1": [1,2,3], "key2": {"nestedKey": 14}}`, for instance.
In strict JSON, the "root" is always an object. Here's a JSON document for demonstration:

{% highlight json %}
{
    "name": "William Shakespeare",
    "birthYear" : 1564,
    "dead" : true,
    "deathYear" : 1616,
    "selectedWorks" : [
        {
            "name" : "The Tragedy of Macbeth",
            "written" : 1606,
            "isItAwesome" : true
        },
        {
            "name" : "Coriolanus",
            "written" : 1608,
            "isItAwesome" : "It's alright, but kinda fascist-y"
        }
    ],
    "wife" : {
        "name" : "Anne Hathaway",
        "birthYear" : 1555,
        "dead" : false,
        "deathYear" : "Fun fact, she's a vampire"
    },
    "favoriteWebsites" : [
        "dailysonneter",
        "dailyprogrammer",
        "vine (he's way into 6-second cat videos)"
    ],
    "facebookProfile" : null
}
{% endhighlight %}

Note that this JSON document has been pretty-printed, which means that a bunch of spaces and indentation has been added in to make it look nicer, but they make no difference. Whitespace that is outside a string has no meaning in JSON.
If you wish to find the name of the first play in the list of selected works, you could say that the "path" to it looks something like this:
`selectedWorks -> 0 -> name`
You would say that the value located at this path is `"The Tragedy of Macbeth"`. The value "dailyprogrammer" is located at:
`favoriteWebsites -> 1`
Notice that JSON lists are zero-based, so the first item in the list has index 0.
Your challenge today is as follows: you will be given a JSON object, and you will print out the search path that leads to the value "dailyprogrammer". You are allowed to use any JSON parsing libraries that you want to, today's challenge is not about parsing JSON, it's about finding a key hidden in a JSON object. If you wish to write a parser yourself, you are of course allowed to do so (though I personally think that would be a little nuts), but you are absolutely not required to do so in any way.

#### Formal inputs & outputs

##### Inputs

The input will be a JSON document which contains the string "dailyprogrammer" somewhere as a value. The JSON document is guaranteed to be valid and use no non-ASCII characters.

##### Outputs

The search-path for the string "dailyprogrammer", in the format described above. Each "element" of the path will either be an integer (if it's indexing a list) or a string (if it's indexing an object). The elements should be joined together with " -> " between them.

#### Sample inputs & outputs

##### Input 1
{% highlight json %}

{  
   "name":"William Shakespeare",
   "wife":{  
      "birthYear":1555,
      "deathYear":"Fun fact, she's a vampire",
      "name":"Anne Hathaway",
      "dead":false
   },
   "favoriteWebsites":[  
      "dailysonneter",
      "dailyprogrammer",
      "vine (he's way into 6-second cat videos)"
   ],
   "dead":true,
   "birthYear":1564,
   "facebookProfile":null,
   "selectedWorks":[  
      {  
         "written":1606,
         "name":"The Tragedy of Macbeth",
         "isItAwesome":true
      },
      {  
         "written":1608,
         "name":"Coriolanus",
         "isItAwesome":"It's alright, but kinda fascist-y"
      }
   ],
   "deathYear":1616
}

{% endhighlight %}

##### Output 1

`favoriteWebsites -> 1`

##### Input 2

{% highlight json %}

{  
   "dlpgcack":false,
   "indwqahe":null,
   "caki":{  
      "vvczskh":null,
      "tczqyzn":false,
      "qymizftua":"jfx",
      "cyd":{  
         "qembsejm":[  
            null,
            "dailyprogrammer",
            null
         ],
         "qtcgujuki":79,
         "ptlwe":"lrvogzcpw",
         "jivdwnqi":null,
         "nzjlfax":"xaiuf",
         "cqajfbn":true
      },
      "kbttv":"dapsvkdnxm",
      "gcfv":43.25503357696589
   },
   "cfqnknrm":null,
   "dtqx":"psuyc",
   "zkhreog":[  
      null,
      {  
         "txrhgu":false,
         "qkhe":false,
         "oqlzgmtmx":"xndcy",
         "khuwjmktox":48,
         "yoe":true,
         "xode":"hzxfgvw",
         "cgsciipn":20.075297532268902
      },
      "hducqtvon",
      false,
      [  
         null,
         76.8463226047357,
         "qctvnvo",
         null
      ],
      [  
         null,
         {  
            "nlp":false,
            "xebvtnvwbb":null,
            "uhfikxc":null,
            "eekejwjbe":false,
            "jmrkaqky":null,
            "oeyystp":false
         },
         [  
            null,
            10,
            "nyzfhaps",
            71,
            null
         ],
         40,
         null,
         13.737832677566875
      ],
      [  
         true,
         80,
         20,
         {  
            "weynlgnfro":40.25989193717965,
            "ggsirrt":17,
            "ztvbcpsba":12,
            "mljfh":false,
            "lihndukg":"bzebyljg",
            "pllpche":null
         },
         null,
         [  
            true,
            false,
            52.532666161803895,
            "mkmqrhg",
            "kgdqstfn",
            null,
            "szse"
         ],
         null,
         {  
            "qkhfufrgac":"vpmiicarn",
            "hguztz":"ocbmzpzon",
            "wprnlua":null
         }
      ],
      {  
         "drnj":[  
            null,
            false
         ],
         "jkjzvjuiw":false,
         "oupsmgjd":false,
         "kcwjy":null
      }
   ]
}

{% endhighlight %}

##### Output 2

`caki -> cyd -> qembsejm -> 1`

#### Challenge Input

##### Input 1

[This input](https://gist.githubusercontent.com/anonymous/8f35cc4fbbccf6d3f59f/raw/1f9786fc2fec9a7afa20cdd70d2d8afb7d3aecb9/challenge1.txt) (about 24 kilobytes)

##### Input 2

[This input](https://gist.githubusercontent.com/anonymous/b7733192c0d1004a084b/raw/b5f8df53469410c634034c12d99bbb8ccc46f102/challenge2.txt) (about 6.5 megabytes)

#### Challenge Input Solution

##### Output 1
`axvjf->tskgrsi->0->ozr->0->1->0`

##### Output 2
`myutkqsfzw->4->fxeu->1->0->1->2->7->ocjcjokh->xqfbrz->0`

## My solution

This my solution for this challenge is really simple. I just use recursion to check for every 
value in the `JSON Object` then print the correct path to console.

### Javascript
{% highlight js %}
function findValue(jsonObj, value, path) {
    if (jsonObj === null) {
        // do nothing
    } else if (typeof jsonObj == 'object') {
        for (key in jsonObj) {            
            findValue(jsonObj[key], value, path + key + '->');
        }
    } else if (jsonObj === value){
        console.log(path.slice(0,-2));
    }
}

var jsonObj1 = require("./challenge1.json");
var jsonObj2 = require("./challenge2.json");

findValue(jsonObj1, "dailyprogrammer", "");
findValue(jsonObj2, "dailyprogrammer", "");
{% endhighlight %}