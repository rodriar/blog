---
layout: post
title: "Android navigation using Navigation + ViewModel Arch components"
date: 2019-07-04 17:00:00
author: Matías Irland
categories: Android Navigation Components, Navigation, ViewModel 
author_id: mirland
---

As many of us know, some time ago Google started to develop a series of libraries to "design robust, testable, and maintainable apps".
It was called [Android Architecture Components](https://developer.android.com/topic/libraries/architecture) and personally I think they are making a great job.

When Google made the announce, the first question that come to my mind was: shall we change our core libraries (that are tested, well known and we were using in most of our projects) for these new libraries?
The first time that we had to answer this question was when we thought about changing the ORM.
The fact was that in most of our projects we were using [DbFlow] or [Realm] and we had to decide if we wanted to start using [Google's Room ORM] instead of these two well known projects.
In this post I'll not talk much about our decision, but I can tell you that it wasn't a simple choice.
Something that we found really interesting and it helped to answer it was that we found that all Architecture Components' libraries working together in combination worked like a charm.

A couple of months ago we had to answer same question with another component, the navigation component.
We have a solid structure to handle the navigation between the different apps' screens which are working fine, they are stable but, **shall we start using the Navigation component?**
I'm a bit anxious, so I'll spoiler the answer. 
In this post I will tell you why we decided start using it and why we don't regret that decision.


# Our Android navigation's history

I've been working on Android as my first class language for more than 4 years.
If you have some time working on android you know that the navigation between different screens can be a real headache.

When I stated working on Android, fragments were beginning to emerge.
So Android' apps were based basically on a set of activities.
Using that, we found some problems to manage the redirections, mainly if we wanted to share some data between activities.
In that case we had to create a parcelable class, put it in a a bundle and then create an intent to start the new activity.
The destination activity had to parse that data using a key value pattern. 
Does it good?
No, it doesn't.
Does it safe?
No, it doesn't, but it was the best way in that moment to navigate between activities.

Then the fragments emerged with all the well known properties, and we decided to start using it.
The architecture was easy, each screen was composed of one activity which has only one fragment.
We used a similar redirection flow as the previews one. 
However, it was even worst.
If we wanted to share data between fragments, the fragment had to prepare an Intent like in the previous one, then the destination activity had to parse it and prepare a new bundle with all these data to share to the new destination fragment.
As we can see, it's an intricate flow, it's not safe and we had to type a lot of code when we wanted to create a new screen.
To avoid some of these problems we started to use some 3th party libraries such us [Dart], [Fragment args] and [Parceler].

After that, Google proposed a single activity architecture application.
In that moment a new problem appeared, it's called [FragmentTransaction].
Manage only one fragment was easy, but manage multiple fragments in a single activity at the beginning was a real headache.
We created an awesome helper class, which we improved further with the Kotlin's extensions arrived, which makes the redirection flow much easier.

At the end we had this awesome helper class, combined with [Fragment args], Kotlin's parzelize feature and a navigation architecture based on the Router pattern.
It's not so bad, right?

# What can we improve?

As I mentioned before, we had a stable navigation flow.
So at the beginning when I tried the new Navigation Arch component I was not sure about what advantages it could provide us compared with our current implementation.

We created a list of existing issues that we had with our current implementation and we wanted to solve with the Navigation component:
- The navigation flow is not clear, we have a router which is called to perform the navigation.
However, it's not clear who calls that router, it's not clear which view can redirect to which view.
Related with that, if we want to change a redirection sometimes is hard, you have to know the application flow map.
- Improve the deeplink redirection.
- Share data between fragments of a same flow.
- In some projects we were using MVP, and it's not good that the view saves the Presenter state.
It'd be nice if the presenter can save the state itself.

## Clear navigation flow 

Have a clear navigation flow is very important in all apps, and in some cases is hard to follow.
As a developer we can create a good documentation about the flow and sometimes a graphic diagram is great asset to induce someone in a project.

The first impression of the Android Navigation component was excellent, the navigation graph is a very useful resource to understand the whole an application.
The diagram contains the different flows and if you want to change someone you can just change the old destination for a new one easily.
If you don't know how to do that, you can follow [Google's documentation](https://developer.android.com/guide/navigation/navigation-create-destinations).

I had only one problem, the app started to grow up and the graph started to be unmaintainable.
It was hard manage all these app's screens in my small laptop screen.
In that moment I started to use [nested navigation graphs](https://developer.android.com/guide/navigation/navigation-nested-graphs).
That was an excellent idea, because I had a main graph with all nested graphs.

Let's see an example. Do you remember [URL Splitting's] blog post?
It solves a similar situation in a web app.
Think of an e-commerce app.
This app lets users browse products, either by searching manually or looking through a catalog by category.
Naturally, users can fill their cart and then proceed to the checkout process, which is fairly complex: users have to specify their address, billing information, select what shipping option they want, and then confirm the purchase.
Also, the app has a section of static pages with a lot of information on how the business works, terms and conditions, FAQs, and more.

Let's traduce this problem to our navigation graph.

#### [Add image]

We can see all advantages that we expressed before, it's simple, clear, maintainable and very useful.  

## Deeplinking improvements

Deeplinks is an old feature that all clients want to have. 







[DbFlow]: https://github.com/agrosner/DBFlow
[Realm]: https://realm.io/
[Google's Room ORM]: https://developer.android.com/topic/libraries/architecture/room
[Dart]: https://github.com/f2prateek/dart
[Fragment args]: https://github.com/sockeqwe/fragmentargs
[Parceler]: https://github.com/johncarl81/parceler
[FragmentTransaction]: https://developer.android.com/reference/android/app/FragmentTransaction