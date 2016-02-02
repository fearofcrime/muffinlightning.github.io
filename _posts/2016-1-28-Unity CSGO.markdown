---
layout: post
title:  "SideProject - Unity CSGO"
date:   2016-1-28 
categories: sideprojects
---

![unity](/assets/csgo2.jpg)

Haven't made a blog post in awhile, as I've been busy with both school and interviewing. Today I will be talking about a project I helped develop last summer, which has since then flourished and since then become a very successful piece of software. While I no longer have any affiliation to the project, I did spend an entire summer co-developing it. Here's how it all started.

### How it started
During the spring/summer of 2015, a friend of mine (who I wont name, for legal reasons-- but I will refer to him as P), who is currently a CS student at UC Berkeley, sent me some code he was working on. Now, I've known P for over 5 years now, and he was one of the people who actually got me interested in programming and computer science in general. So of course, if he came to me with something interesting, I'd always take a look. He explained that he had created an aim-assistance program in Valve's Counter-Strike: Global Offensive PC game. I blindly imported his project, compiled it, and quickly tested it in an AI game. Before I go any further, I want to stress that I have NEVER tested or run any of our code against actual players. I don't belive in jeopardizing competitive integrity, whether it be for learning purposes or not. 

Anyway, once I discovered that P wasn't kidding, his aim assist program was actually functional, we had a Skype call and he guided me through his code. This is where he caught my interest.

### How I got interested
There were many concepts P described that I had very little knowledge of, such as:

-   vtables
-   function hooking
-   the Win32 API 
-   the source sdk
-   memory editing

Needless to say, I was intrigued. P said he wanted to improve on the program, and that he had a couple friends who were interested in building upon his project. I saw this as an opportunity to *learn*, and to collaborate with other developers across the country. If nothing else, I was certain that they could teach me a thing or two. At that point, I was on-board.

### Development

Now, at that specific point in time, I had very little experience in software development methedology. P and his friends were all a year ahead of me in their CS programs, so I just followed. We had a Skype call and discussed what we wanted to create. Honestly, I just wanted to learn. Nothing else. I really didn't care about the final product. P had contacted another friend of mine (who is also a student at Dalhousie), Kenny. Since Kenny and I both lived in Halifax and went to the same school, we decided it would be best for the two of us to program together on the same feature. We were assigned to work on the ESP feature of the program. ESP = Extrasensory Perception. Basically, it's a wallhack. 

Neither Kenny or I had any experience actually programming something of this nature, so development was quite slow, and we had to ask P a lot of questions. Kenny in particular had very little programming experience, but I think this is what got him hooked on software development. I'm not going to go super in-depth in terms of code, but essentially we broke the problem down into a few steps:

We needed:

- User location (XYZ coordinates)
- Health
- Team location
- Enemy location

Obviously, all this information is stored within memory, and we just needed to find the locations of these. I'm not going to go into detail as to how we found them, but once we did, the rest of the development wasn't so bad. We just needed to learn the Windows API. Using the API, we then leveraged the RPM function to get the data we need.

{% highlight C++ %}
BOOL WINAPI ReadProcessMemory(
  _In_  HANDLE  hProcess,
  _In_  LPCVOID lpBaseAddress,
  _Out_ LPVOID  lpBuffer,
  _In_  SIZE_T  nSize,
  _Out_ SIZE_T  *lpNumberOfBytesRead
);
{% endhighlight %}

We then defined structs for both the user and the enemy players. RPM is used to fill our structs with actual data. It looks something like:

{% highlight C++ %}
struct MyPlayer_t  
{ 
    DWORD CLocalPlayer; 
    int Team; 
    int Health; 
    WorldToScreenMatrix_t WorldToScreenMatrix;
    float Position[3]; 
    int flickerCheck;
    void ReadInformation() 
    {

    // Reading out our Team to our "Team" Varible. 
        ReadProcessMemory (fProcess.__HandleProcess, (PBYTE*)(CLocalPlayer + dw_mTeamOffset), &Team, sizeof(int), 0);
    // Reading out our Health to our "Health" Varible.     
        ReadProcessMemory (fProcess.__HandleProcess, (PBYTE*)(CLocalPlayer + dw_Health), &Health, sizeof(int), 0);
{% endhighlight %}

Once we had all the info we needed, it was just a matter of using this information and drawing to the screen. Now, drawing can be accomplished in many ways, but the way we did it was to take advantage of the Source SDK. [The Source SDK is actually publically available on GitHub](https://github.com/ValveSoftware/source-sdk-2013). It amazed me that we could just import the Source SDK into our VC++ project and then use its functions.

After combing through the SDK a bit, we found a a couple neat functions:

- DrawSetColor()
- DrawFilledRect()

How convenient...I'm not kidding, you can find them being used [on the Source SDK wiki](https://developer.valvesoftware.com/wiki/ISurface).

Putting everything together, you end up with an ESP:

![csgo](/assets/csgo.PNG)

### Departure

Once we finished our feature, it was combined into the main program, and the other developers had completed a few other cool features. It was anything but clean. It was quite buggy actually, and would crash seemingly at random, but it worked. At this point Kenny and I were pretty done with the project, it was an interesting learning experience but we had no interest in actually extensively testing and debugging it. The other devs at Berkeley didn't seem particularly interested either, so we left it at that.

P however, continued to polish it. He'd update me on Skype about something new he fixed every week. "Okay... cool" I would reply. I wasn't exactly sure why he was spending so much time on something that everyone else had lost interest in. We had achieved our primary goal, which was to learn. And we learned a lot. P asked Kenny and I if we wanted to continue with what we had worked on, and we said no. P wanted to bypass the VAC anticheat and use the program against players. We had NO interest in this.


### The birth of UnityHacks, and unethical monetization

I'm not exactly sure what happened next, but what I do know is P set up a website for his program and brought on a couple new devs to help maintain it. He also began to monetize the software, breaking Valve's TOS. You can find his software online. While I no longer have access to the code-base, Kenny and I are still disappointed in how our code ended up being used maliciously, even if the current code probably looks nothing like it did nearly a year ago. All I know is, Unity is now a full-fledged, polished piece of software, but it is HIGHLY unethical (in my opinion).

We still talk to P about it from time to time, and I actually have a copy of his latest compiled build. But I have no interest in running it. 