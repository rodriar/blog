---
layout: post
title: "Android Navigation Component - Expectations, Conclusions & Tips"
date: 2020-01-12 10:00:00
categories: android, architecture components, jetpack, navigation component
author_id: mirland
featured_position: 1
featured_image: /images/tflite_coreml/featured.png
---

The new [Google's io](https://events.google.com/io/) is coming, so I think it's a good time to talk a about one of the biggest [Jetpack's Architecture component](https://developer.android.com/jetpack) introduced last year, the [Android Navigation Component](https://developer.android.com/guide/navigation).

The aim of this series is to talk about two important items, that can help you to decide to use this library or not:
1. The expectations and conclusions of using it for more than 6 months.
1. Some tips and helper classes that are very helpful if you want to start using it. 
These tips includes:
- A navigation extension to navigate safely and avoid some issues.
- A helper class to log the current graph path, useful when you want to check if your graph was built in the right way.
- A helper class to detect possible `TooLargeTransactionExceptions`.

# What we expected of a new navigation library?
This was the first question that came to my mind when we though about using it.
There are a bunch of navigation libraries, in that moment had a solid navigation architecture based on the router pattern, so, what we expect of a new library?

Our expectations were:
1. A library easy to integrate and debug.
1. A library which provides us a better understanding of the app's, what are the main flows and feature. 
1. Share data between views easily.
1. Improve the way of handling deeplinks.
1. A good integration between all Jetpack Components.

## What we found?

The **integration** is very easy, you can follow the [Android's documentation](https://developer.android.com/guide/navigation/navigation-getting-started) and check the [sample projects](https://github.com/android/architecture-components-samples).

One of my favorites features is the ability to define cool **transitions between fragments** by adding just a few lines.
If you add the right transitions, you can improve the app's UX a lot.
Furthermore, a couple of weeks ago, material released the [motion system](https://material.io/design/motion/the-motion-system.html), a set of transition patterns that can help users understand and navigate an app.
You can integrate these transitions easily in your app using the navigation library.

The app's **navigation graph** is awesome useful.
It have two key advantages:
- The first one, is that you can see the whole app flow represented as a navigation graph, it's useful for you, as long as for new team mates that are joining to your project.  
- The second good point is the ability to define subgraphs, so you can have big epics represented in a subgraph.
The login is a common example, suppose that the login epic contains a login screen, a register screen and a terms and condition screen.
In the main graph, instead of adding all these screens, you can include the login subgraph.
Using this approach I found some advantages: the main graph is easier to understand, you can reutilizes the epic in multiple places in the app and you can also change something of the flow only in one place with low effort.

One disadvantage we found is related of how AS saves the graph nodes position, it's saved in `.idea/navEditor.xml` file.
You have to track this file if you want to save the positions of all graphs nodes.
Sometimes this file is corrupted and you lost the positions of all nodes.
Moreover this file suffer a lot of git conflicts, so it's a bit tedious to maintain.

The library includes a [**testing** module](https://developer.android.com/guide/navigation/navigation-testing), which helps to test your app's navigation logic before you ship in order to verify that your application works as you expect.
The **debug** process is good, we found an awesome feature: 
Suppose you are working on a feature in same internal screen.
When you run the application you have to navigate in the app to open the screen and test your progress in that feature.
We realize that we waste a lot of time navigating in the app to test the working progress.
If you use this library, you can change the `startDestination` navigation's graph property, and set your screen, so when you run the app, the screen is opened, awesome right?.
On the other hand, debug the graph status is a bit hard, you cannot know what is the current graph path.
To solve this issue we created a cool tool to log the current path, I'll introduce it later.


# What are the advantages that we found of using the Navigation Component?


