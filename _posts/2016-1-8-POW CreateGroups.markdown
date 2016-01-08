---
layout: post
title:  "Programming Problem - CreateGroups"
date:   2016-1-8 
categories: programmingproblems
---

Today I'll be giving my commentary on a problem I solved earlier that I found interesting. It's definitely not the hardest problem, but it did get me thinking and that's the whole point of these exercises --- to keep your mind sharp, and to prevent what I like to call *programming rust*. While the question doesn't involve any complex concepts, I would rate it harder than the majority of the *easier problems*. This is reflected in the 73% success rate, which is usually higher for this category of problems. So, without further adieu, here is my Problem of the Week:

### The Problem
>Your language school is starting a new semester, and each student must select a time schedule. You are given a int[] groups, where the i-th element is the number of students who selected the i-th schedule. Your school has a rule stating that the number of students assigned to each schedule must be between minSize and maxSize, inclusive. However, this rule was not enforced properly during the sign up phase. It is your job to reassign students in such a way that the rule is satisfied. A reassignment is defined as removing a student from one time schedule and placing him in a different time schedule. Return the minimal number of students you must reassign, or return -1 if it is impossible to satisfy the rule. Note that you may not create new schedules or delete existing schedules.

### Constraints
-   groups will contain between 1 and 50 elements, inclusive.
-   Each element of groups will be between 1 and 1,000,000, inclusive.
-   maxSize will be between 1 and 1,000,000, inclusive.
-   minSize will be between 1 and maxSize, inclusive.

### Solution

For these types of questions, I always like coding the "meat and potatoes" first. Get the base of the problem done, and then work on edge cases afterwards.

We know for an absolute fact that we will have to iterate through this array, so let's get that out of the way first:

{% highlight java %}

public class CreateGroups {

    
    public int minChanges(int[] groups, int minSize, int maxSize) {
        
        int counterMax = 0;
        int counterMin = 0;
        
        for (int i = 0; i < groups.length; i++) {
        }      
    }
}


{% endhighlight %}

Our program needs to return the total number of students that have to be reassigned. Students need to be reassigned into different schedules IF:

1) The schedule they are in is over the maximum, **maxSize**.
2) There exists another schedule in which there aren't enough students to satisfy the minimum size, **minSize** 

Alright, but we can't simply just add 1) and 2) together and call it a day. That wouldn't work because we can take students from an overfilled class and move them into an underfilled class, so it's not quite as simple as 1) + 2). Actually, it will be the maximum of 1 and 2. It makes sense if you think about it. 

Okay, so getting the total number of students sounds helpful, so let's make a variable, *totalStudents*, and add to it as we iterate through the array. In addition, I'll also store the number of students in the current schedule. We can use this to calculate our 1) and 2) I mentioned earlier. To clarify:

- If the size of the schdule is **greater than the  maximum size**, then we calculate the difference (which will be how many students need to be removed from this specific schedule)

- If the size of the schedule is **less than the minimum size**, then we can also calculate the difference (which will be how many students need to be added into this specific schedule).

What do we do with these differences? Well I mentioned earlier that we want the maximum of these cases, so we'll just add them up and then run Math.max() on them. Please ignore my unimaginative variable names :p



{% highlight java %}

public class CreateGroups {

    
    public int minChanges(int[] groups, int minSize, int maxSize) {
        
        int counterMax = 0;
        int counterMin = 0;
        int counter = 0;
        int totalStudents = 0;
        
        for (int i = 0; i < groups.length; i++) {
            int currentSize = groups[i];
            totalStudents += groups[i];

            if (currentSize < minSize) { // Check minimum case
                counterMin += minSize-currentSize;  
            }
            
            else if (currentSize > maxSize) { // Check maximum case
                counterMax += currentSize-maxSize;
            }
        }      
        counter = Math.max(counterMin, counterMax);


    }
}
{% endhighlight %}

We're almost done! Now we just need to handle the edge cases. If we have more students than our capacity (size of array * maxSize), or if we have less students than the minimum required (size of array * minSize), then we return -1.

{% highlight java %}

public class CreateGroups {

    public static int minChanges(int[] groups, int minSize, int maxSize) {
        
        int counterMax = 0;
        int counterMin = 0;
        int counter = 0;
        int capacity = maxSize * groups.length;
        int requirement = minSize * groups.length;
        int totalStudents = 0;
        
        for (int i = 0; i < groups.length; i++) {
            int currentSize = groups[i];
            totalStudents += groups[i];
            
            if (currentSize < minSize) { // Check minimum case
                counterMin += minSize-currentSize;  
            }
            
            else if (currentSize > maxSize) { // Check maximum case
                counterMax += currentSize-maxSize;
            }     
            
        }
        
        counter = Math.max(counterMin, counterMax);
        
        if (totalStudents > capacity || totalStudents < requirement) {
            return -1;
        }
        
        else {
            return counter; 
        }
        
            
    }    

}
{% endhighlight %}



![Test Cases 1](https://i.imgur.com/xldKvoI.png)
![Test Cases 2](https://i.imgur.com/FyBw4iW.png)
![Test Cases 3](https://i.imgur.com/vNhz4jq.png)


Done!
