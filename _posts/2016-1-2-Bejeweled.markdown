---
layout: post
title:  "Bejeweled Bot"
date:   2016-1-2
categories: personalprojects
---

Bored Sunday afternoon, so I thought: why not do a Bejeweled bot? Actually what really happened was I was trying to sleep and for some reason I started to think about things to program. When morning came, I was determined to make a bejeweled bot. Specifically for [Popcap's Bejeweled](http://bejeweled.popcap.com/html5/0.9.12.9490/html5/Bejeweled.html). I broke this down into 3 problems:

1) Input - getting the location and colors of all the pieces

2) Moves - identifying groups/figuring out which move(s) to do

3) Output - apply the move(s) determined in step 2

I'll focus mostly on step 1, because steps 2 and 3 are quite trivial. I mean, this is Bejeweled, not chess. You match groups of gems. Of course, this can be taken to the next level and I could store a tree filled with moves and try to calculate how to get the highest possible score without ever running out of moves...but honestly, my Christmas break is nearly over. Don't got time for that! I was more interested in step 1 anyway.

So obviously, a 2D array will be needed to store the 8x8 board. When doing a screen bot, I like to get a "point or origin", some relative point on the board in which I can use to get the locations of all the pieces. In this case I went with the upper left corner, we can get this point using the handy dandy [Page Ruler Addon for Google Chrome](https://chrome.google.com/webstore/detail/page-ruler/jlpkojjdgbllmedoapgfodplfhcbnbpn?hl=en) 

![Page Ruler](/assets/bejew1.PNG)


Yes I hardcoded it, yes I could have used some sort of pixel/image scanning to find this point, if I ever decide to improve on this bot, I'd definitely change this.

Okay, we have our point of origin, awesome. But we need the location of each piece on the board. We will obtain this by:

>X or Y coordinate = Point of Origin + (Length/2) + (Length * x or y) 

Where x and y == the respective x or y piece on the board. We get this length the same way as we got the point of origin:

![Length Image](/assets/bejew2.png)

Great, looks like each square is 83 pixels long. RIGHT? RIGHT? WRONG! Upon closer examination, (using paint and the zoom tool) I found 82 pixels to be a better estimate. Although I tried 83, 81, and 80, each giving not-so-great results. In fact, these squares aren't actually exactly 82 pixels long, they're 81 point something-something-something. I'm not even sure if they're perfect squares! Curse you PopCap!

Thinking I had the correct length, identifying colors seemed like it would be easy. I'd be grabbing the pixel at the exact center of each square, so I should be able to hard code these colors in right? Using the BufferedImage class, we will use the getRGB(int x, int y) function which returns an "integer pixel in the default RGB color model".

### COLOR IDENTIFICATION - STUPID ATTEMPT #1

{% highlight java %}
int color = screenshot.getRGB(origin.x + (82 * x), origin.y + (82 * y));
{% endhighlight %}

With this approach, converting this integer value into its individual RGB values isn't necessary. Alright, so we iterate through each square and fetch this integer value, and I can then hardcode these colors in and bam, we have color identification, EASY PEAZY, or so I thought.


{% highlight java %}
for (int y = 0; y < 8; y++) {
 for (int x = 0; x < 8; x++) {
  int color = screenshot.getRGB(origin.x + (82 * x), origin.y + (82 * y));
 }
}
{% endhighlight %}

You see, I mentioned earlier that these squares weren't actually perfectly 82 pixels long. You can see why this would be problematic. After doing some comparisons, I found that this method still gave *mostly* correct answers. So, lazy me, I thought I could just get away with storing a few variants of each color into a set, and then checking that set for a color match. Oh, lazy me does stupid things sometimes. This leads to a myriad of other problems. For example the fire gems casting a glow on nearby gems, and the fact that it would take A LOT of test runs to get a perfect Set for each color. I did this for like 5 minutes before realizing how dumb this idea was.

### COLOR IDENTIFICATION - STUPID ATTEMPT #2

After giving up on my first plan, I gave in and decided to convert the integer value into its individual RGB values. Enter Java's Color class.

{% highlight java %}
int red = color.getRed();
int green = color.getGreen();
int blue = color.getBlue();
{% endhighlight %}

I would then use a series of if-statements to check these values and then try to identify the color that way. By using relational operators, I can check for a range of colors. This quickly turned into a mess of spaghetti code and not-so-accurate color identification, especially since the yellow and orange were giving nearly the exact same RGB values. Add in the fire gem and orange/yellow becomes white etc etc.


### <span style="color: green">COLOR IDENTIFICATION - WORKING SOLUTION!</span>

At this point I was starting to get a little frustrated. Finally, I decided to convert the color to the HSB [(Hue, Saturation, Brightness)](http://www.tomjewett.com/colors/hsb.html) format. 

{% highlight java %}
Color.RGBtoHSB(r, g, b, hsb);
{% endhighlight %}

This returns a float[] containing the hue, saturation, and brightness.

[I then used this HSL color picker as a reference point](http://hslpicker.com/) and built my if statements based on this. Being able to check for brightness/saturation is a huge benefit when trying to identify the color white. I then just built my own ranges of hue values to determine the other colors. Orange/Yellow gave similar values, but they were different enough to work with.

This post is getting quite long so I will leave you with the final product:

{::nomarkdown}
<iframe width="560" height="315" src="https://www.youtube.com/embed/hOTplw7yuuM" frameborder="0" allowfullscreen></iframe>
{:/nomarkdown}

It's far from perfect, as it takes screenshots of the board to determine the pieces, and sometimes it will take these screenshots as the gems are still falling, which obviously throws off the colors. Also, the fiery gems still throws things off a bit. Not too shabby for a day's work though! I'm not sure if I will continue to work on it, but I might work on a Java library to make color identification easier for the purposes of making screen bots.

Please ignore my friend complaining about NCIX :D
