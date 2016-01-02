---
layout: post
title:  "Triple Looped Nightmare"
date:   2015-11-28 
categories: programmingproblems
---
Oh how I enjoy triple nested loops! Here’s a problem I did earlier that made me want to smash my monitor:

>A word is grouped if, for each letter in the word, all occurrences of that letter form exactly one consecutive sequence. In other words, no two equal letters are separated by one or more letters that are different. For example, the words “ccazzzzbb” and “code” are grouped, while “aabbbccb” and “topcoder” are not.

>You are given several words as a String[]. Return how many of them are grouped.

To put things into perspective, the fastest programmer to solve this problem did it in TWELVE seconds. He wrote the entire function out and submitted it in 12 seconds…. yeah okay, I guess I just suck, because I definitely spent a great deal longer than 12 seconds!

So the first thing I had to consider was the fact that we are passing in a String array, and we will have to iterate through it and grab each string:

{% highlight java %}
public int howMany(String[] words) {
int counter = 0;
 
for (int i = 0; i < words.length; i++) {
 
}
return counter;
 
}
{% endhighlight %}

Now for the not so fun part, we need to iterate through the String and look for ‘groups’ of characters. We’ll iterate through the String and compare each character with the character in front of it(one loop), if they’re different, then we know that specific grouping(which could only be 1 character) ends. The next step will be checking if that specific character shows up again in the String(another loop), and if it does, then this specific word isn’t grouped, so we don’t increment our counter. Phew! Okay, two more loops coming up:

{% highlight java %}
public static int howMany(String[] words) {
    int counter = 0;
 
    for (int i = 0; i < words.length; i++) {
        boolean isGrouped = true;
        for (int j = 0; j < words[i].length() - 1; j++) {
            if (words[i].charAt(j) != words[i].charAt(j + 1) && isGrouped == true) {
                for (int k = j + 2; k <= words[i].length() - 1; k++) {
                    if (words[i].charAt(j) == words[i].charAt(k)) {
                        isGrouped = false;
                    }
                }
            }
        }
        if (isGrouped == true) {
            counter++;
        }
    }
    return counter;
}
}
{% endhighlight %}

Annnnnd hazah:

![Test Cases](https://honestprogrammer.files.wordpress.com/2015/11/groups.png?w=625)


Looking at my solution, I don’t think I could even re-type that in 12 seconds. It's also a very inefficient solution, but that's a problem for another post.


