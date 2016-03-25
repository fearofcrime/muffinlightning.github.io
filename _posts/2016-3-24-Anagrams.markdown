---
layout: post
title:  "Programming Problem/Optimization - Valid Anagram"
date:   2016-3-24
categories: programmingproblems
---

I have yet to do problem and then try to optimize it. Today I'll look at a problem, its solution, and ways it can be optimized.

### Problem Statement
>Given two strings s and t, write a function to determine if t is an anagram of s.


### Examples
s = "anagram", t = "nagaram", return true.
s = "rat", t = "car", return false.

Note:
You may assume the string contains only lowercase alphabets.

### Solution

This is actually quite a simple problem if you think about it. We are given two strings and we need to see if they're anagrams of each other. In other words, we check if one word is a re-arrangement of the letters of another word. The most intuitive way of solving this problem consists of:

1. Converting the strings into a char array

2. Sorting the char arrays

3. Comparing the two arrays

Why does this work? Well, let's use one of their given examples.

s = "anagram"

t = "nagaram"

Now we sort s and t alphabetically.

s = "aaagmnr"

t = "aaagmnr"

They're the same! So they're anagrams. Translating this into Java code:

{% highlight java %}

public static boolean isAnagramIneff(String s, String t) {

    if (s.length() != t.length()) {
      return false;
    }

    char[] sSorted = s.toCharArray();
    char[] tSorted = t.toCharArray();
    Arrays.sort(sSorted);
    Arrays.sort(tSorted);

    
    for (int i = 0; i < s.length(); i++) {
      if (sSorted[i] != tSorted[i]) {
        return false;
      }

    }

    return true;

    
  }
{% endhighlight %}

No nested loops, so the average time complexity of this code will be whatever Arrays.sort() is, according to Java's doc: O(n log n). 

This code passes 32/32 test cases, and had a total runtime of 8ms. Not bad! But our method for comparing the two char arrays is a little primitive. We can shave off time by converting them back to strings and using Java's .equals() method:


{% highlight java %}

  public static boolean isAnagramIneff(String s, String t) {

    if (s.length() != t.length()) {
      return false;
    }

    char[] sSorted = s.toCharArray();
    char[] tSorted = t.toCharArray();
    Arrays.sort(sSorted);
    Arrays.sort(tSorted);
    
    String s2 = new String(sSorted);
    String t2 = new String (tSorted);
    
    
    
    if (s2.equals(t2)) {
      return true;
    }
    
    else {
      return false;
    }

    
  }
{% endhighlight %}

The average time complexity is still bounded by Arrays.sort() and thus will be the same. This code also passes 32/32 test cases, and has a run-time of 5ms. We also beat out 84.75% of java submissions, nice!

![one](https://i.gyazo.com/4a285715f9f021e62590cf5427e86c0a.png)

But technically this isn't the most efficient solution in terms of time complexity. We can get this down to O(n) using hash tables. The approach is as follows:

1. Create a hash table that tracks characters and the number of times the character appears in the String. For example, the word anagram will have a = 3, n = 1, g = 1, r = 1, m = 1. We use the character as the key. 

2. Loop through the characters of the first String. Each time a character appears, we will +1 the Integer value associated with it. This acts as a counter.

3. Loop through the characters of the second string, but this time we will decrement the Integer value associated with it.

4. If the hash table is empty by the end of it, then we have an anagram.

5. To save time, if the second String has a character that isn't present in the hash table, we know they aren't anagrams. We can instantly return false.

Translating this into Java code looks like:


{% highlight java %}

  public static boolean isAnagram(String s, String t) {
    
    HashMap<Character, Integer> h1 = new HashMap<Character, Integer>();
    
    for (Character ch : s.toCharArray()) {
      if (h1.get(ch)==null) {
        h1.put(ch, 1);
      }
      
      else {
        h1.put(ch, h1.get(ch)+1);
      }
    }
    
    
    for (Character ch : t.toCharArray()) {
      if (h1.get(ch)==null) {
        return false;
      }
      
      else if (h1.get(ch) == 1) {
        h1.remove(ch);
      }
      
      else {
        h1.put(ch, h1.get(ch)-1);
      }
    }
    
    
    return h1.isEmpty();
  }
{% endhighlight %}

This is an O(n) solution and is the most efficient solution we can achieve algebraically. However in practice, this solution is slow compared to the first two. It also passes 32/32 test cases but has an agonizingly slow runtime of 41ms. This solution still beats 22.48% of java submissions. Makes me wonder how you could do this any slower.


![two](https://i.gyazo.com/e20586a76ce19010f16bc11920b10ba7.png)


Why might this be? Well...

- Perhaps instantiating a HashMap takes more time than expected.
- Maybe input n is low enough such that all of these HashMap functions being run add up. This is unlikely, but I'm not given the test cases, so it's a possibility.

Either way, it's odd behavior.

