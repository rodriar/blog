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

The aim of this serie is to talk about two important items, which can help you to decide to use this library or not:
1. The expectations and conclusions of using it for more than 6 months.
1. Some tips and helper classes that are very helpful if you want to start using it. 
For instance:
- A Safe navigation extension to avoid some issues.
- A helper class to log the current graph path, useful when you want to check if your graph was built in the right way.
- A helper class to log TooLargeTransactionExceptions.

# What we expected of a new navigation library?
This was the first question that we did when we though about using it.
There are a bunch of navigation libraries, in that moment had a solid navigation architecture, based on the router pattern, so, what we expect of a new library?

Our expectations were:
1. A library easy to integrate and debug.
1. A library which provides us a better understanding of the app's, what are the main flows and feature. 
1. Share data between views easily.
1. Improve the way of handling deeplinks.
1. A good integration between all Jetpack Components.

## What we found?

The **integration** is very easy, you can follow the [Android's documentation](https://developer.android.com/guide/navigation/navigation-getting-started) and check the [sample projects](https://github.com/android/architecture-components-samples).

Related to the **debug** process, we found an awesome feature: 
Supose you are working on feature in same internal screen.
When you run the application you have to navigate in the app to open the screen and test your feature.
We realize that we waste a lot of time navigating in the app to test all features.
If you use this library, you can change the `startDestination` navigation's graph property, and set your screen, so when you run the app, the screen is opens, awsome right?.
On the other hand, debug the graph status is a bit hard, you cannot know what is the current graph path.
To solve this issue we created a cool tool to log the current path, I'll introduce it later.





# What are the advantages that we found of using the Navigation Component?


