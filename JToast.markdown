---
layout: post
title:  "Side Project - JToast"
date:   2016-2-19
categories: sideprojects
---

![JToast](/assets/jt1.png)

JToast is a Java class created with JavaFX used to create toast notifications on the user’s screen. You may integrate these into your own Java/JavaFX programs.

## Features
- Easy to integrate into any Java/JavaFX project.
- Three built-in animation types: Pop, Fade, and Slide.
- The ability to create your own animation types!
- Support for custom images


## Quick start:

Toast notifications, yum. 

        {% highlight java %}
// Create a new JToast
        String title = "HEY YOU!";
        String message = "JToast is the coolest toast app ever!";
        Notification notification = Notifications.SUCCESS;

        JToast tray = new JToast();
        tray.setTitle(title);
        tray.setMessage(message);
        tray.setNotification(notification);
        tray.showAndWait();
{% endhighlight %}

It's very simple to integrate/use. Create a JToast object, set a title, message, notification type, and animation(optional). Then run showAndWait().

The structure is as follows:

- *Location.java* is used to get the location of the bottom right corner of the screen to display the notification.
- *CustomStage.java* uses Java's Rectangle2D class and handles the location/creation of the box itself.
- *JToast.java* is the main java file. The constructor is found here, this is where the object is created and where the values/parameters are set.
- *NotificationType.java* and *AnimationType.java* are [enum classes.](https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html)
- Finally, the files in the *animation* folder are well, for the built-in animation types.


### Screens
![JToast2](/assets/jt2.PNG)

![JToast3](/assets/jt3.png)

![JToast3](/assets/jt4.gif)

The project is on my GitHub, the direct link is [here.](https://github.com/MuffinLightning/JToast) check it out and feel free to use the library in your own apps.


