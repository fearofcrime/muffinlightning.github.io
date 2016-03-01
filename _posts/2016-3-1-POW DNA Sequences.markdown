---
layout: post
title:  "Programming Problem - DNA Sequence"
date:   2016-3-1
categories: programmingproblems
---

Haven't posted a problem in awhile, so here we go. Today's problem wasn't very difficult, but optimizing it may prove challenging.

### Problem Statement
>For the purposes of this problem, a DNA sequence is a string consisting of the letters A, C, G, and T. You have been hired by researchers at TopBiologist to help them with some programming tasks. The researchers have found a sequence of uppercase letters, which is given to you in the String sequence. They have asked you to write a program which find the length of the longest DNA sequence that is a substring of sequence. A substring can be obtained from sequence by deleting some (possibly zero) characters from the beginning or end. For example, suppose sequence is the string "TOPBOATER". Then "TOP", "PBOA", "T", and "AT" are some examples of substrings of sequence. Of these examples, "T" and "AT" are DNA sequences. The answer to the problem would be 2 because "AT" is the longest such sequence and its length is 2. Please find and return the length of the longest DNA sequence that is a substring of sequence.

### Constraints
-	sequence will contain between 1 and 50 characters, inclusive.
-	Each character of sequence will be an upper-case English letter ('A'-'Z').

### Solution

The question itself may be a little confusing when you first read it, so the first thing I'm going to do is break it down into simpler terms:

>We need to take a String, take all substrings of that String, and check if it is a DNA sequence. A DNA sequence can only contain the characters A, C, G, and T. Given all possible DNA sequences of a specific String, we need to return the length of the longest DNA sequence. 

So, what's the first thing we need to do? Let's start by defining some important variables. We need to eventually return the length of the longest sequence, so I will create a variable to hold that. In addition, I'll also create variables to hold the length of the given String, and also a String variable to hold the substring(sequence) itself.

We need all substrings of a given String, so let's start there. To obtain a substring, we can use Java's built in substring(beginIndex, endIndex) method, which does exactly what you think. But, we need EVERY possible substring, which CAN be only 1 character. The simple way of doing this is to use a doubly nested loop:

{% highlight java %}

public int longestDNASequence(String sequence) {

int length = sequence.length();
int longestDNA = 0;
String DNAstring = "";

for (int i = 0; i <= length; i++) {
			for (int j = i; j <= length; j++) {
					
				
				
			}
			
		}
		
		
		
}
{% endhighlight %}

Inside the loop, we will take the substring with beginIndex = i, and endIndex = j. This ensures that we will obtain every substring possible. Remember that the substrings can be only 1 character. If our given string is TOPBOATER , then this is what will be happening (enjoy my awesome paint rendition of i=0 and i=1): 

![DNA1](assets/dna1.png)

But wait! We don't want EVERY substring, we only want the ones that ONLY have characters A, C, G, and T! Right. So we will need to check for that.

- My initial thought was to use .contains() , but this has problems. We can check if it contains ACGT, but there might be a B or an X or something else somewhere inside which will throw us off.
- We could do something like !contains.(everyOtherCharacter). Yes, yes we could. That would take awhile to type.
- We could use REGEX! Probably the best and most simple way of doing this. Hey, I hate Regex as much as the next person but you have to admit it has its uses.

Last step is to just compare and see if the size of the DNA sequence is largest. Our nested if statements will look like:

{% highlight java %}

 if (sequence.substring(i, j).matches("[ACGT]+")) {
     DNAstring = sequence.substring(i, j);
     if (DNAstring.length() > longestDNA) {
      longestDNA = DNAstring.length();
     }

    }
{% endhighlight %}

Putting it all together:

{% highlight java %}
	public int longestDNASequence(String sequence) {
		
		int length = sequence.length();
		int longestDNA = 0;
		String DNAstring = "";

		for (int i = 0; i <= length; i++) {
			for (int j = i; j <= length; j++) {
					
				if (sequence.substring(i, j).matches("[ACGT]+")) {
					DNAstring = sequence.substring(i, j);
					if (DNAstring.length() > longestDNA) {
						longestDNA = DNAstring.length();
					}
					
				}
				
			}
			
		}
		
		
		return longestDNA;
		
	}
{% endhighlight %}

O(n3) OUCH! But before you panic, let's keep in mind that the sequence will contain between 1 and 50 characters, inclusive. Still...

### Testing

MOMENT OF TRUTH!

![DNA2](assets/dna2.png)

eZ