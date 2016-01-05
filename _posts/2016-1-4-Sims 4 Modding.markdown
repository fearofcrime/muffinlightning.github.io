---
layout: post
title:  "Game Modding Monday: Sims 4"
date:   2016-1-4
categories: personalprojects
---

Today I thought I'd do something a little different, I'm going to take a game and demonstrate some simple modding. It's not necessarily programming, but in my opinion it's an important skill to be able to figure out how other programs work. Plus it's fun being able to make changes and then seeing them actually happen in-game!

![The Sims 4](/assets/sims1.png)

The game we will be modding is The Sims 4. If you haven't heard of the Sims, I don't know what to tell you. It's only the most popular(and only?) life-simulator/sandbox game out there. In the Sims 4, Sims who hate each other can fight one another, as long as they are the same age. However, that's not bloodthirsty enough for me, and it's something that has always bothered me. I want it so any Sim can fight one another, regardless of age or relationship. How am I supposed to make a Fight Club with all of these restrictions? Let's remove them.

This can be done by editing the game's .xml files. These files however aren't readily available to you. They must first be extracted from the game's binaries. I'm not about reinventing the wheel, so we will use the [Sims 4 XML Extractor](http://modthesims.info/t/534316). We target the Sims 4 installation folder and extract the XML files, and then you will end up with this mess:

![The Sims 4](/assets/sims2.png)

And inside the sub folders:

![The Sims 4](/assets/sims.png)

So, what do we do with this garbage? We use Windows' search tool. I just searched *fight*. When your search completes, you will see a file called **S4_E882D22F_00000000_000000000000661A.xml**. It seems to be about what we're looking for. Let's open it up. Now honestly, I have no idea about the developer's XML naming conventions, but it doesn't really matter. After a quick skim, I found this:

![The Sims 4](/assets/sims3.png)

Seems to be what we're looking for, this is what's stopping Teens and Pregnant people(oh god I sound horrible..) from fighting one another. So we can delete this entries.

![The Sims 4](/assets/sims4.png)

Oh and this is what's stopping Sims who are getting married from fighting one another. We'll remove that too.

![The Sims 4](/assets/sims5.png)

What? Sims need to have met each other to fight one another? NONSENSE. Random violence ftw! We'll 86 that.

Great. Now Sims can pick fights with each other regardless of age/relationship status. The next step is to save this .xml file and then convert it into a .package file. We can do this by using the [Sims 4 Package Editor](https://github.com/Kuree/Sims4Tools/releases). Save this .package file inside the game directory/Mods folder. 

And here's a picture of my Aaron Rodgers sim picking a fight with a little kid. I also found a way to make fighting lethal by decreasing the hunger motive to -100 after losing a fight, but I found that to be *little* too brutal.

![The Sims 4](/assets/sims6.png)
