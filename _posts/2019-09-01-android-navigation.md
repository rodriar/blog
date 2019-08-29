---
layout: post
title: "Handle Android navigation using Navigation + ViewModel arch components"
date: 2019-07-04 17:00:00
author: Matías Irland
categories: Android Navigation Components, Navigation, ViewModel 
author_id: mirland
---

As many of us know, some time ago Google started to develop a series of libraries to "design robust, testable, and maintainable apps".
It was called [Android Architecture Components](https://developer.android.com/topic/libraries/architecture) and personally I think they made a grate job.

At the beginning, one question that come to my mind was, should we change our core libraries (that are tested, well known and we were using in most of our projects) for these new libraries?
An example of that was the ORM, in most of our projects we were using [DbFlow] or [Realm].
So we had to decide if we wanted to start using [Google's Room ORM] instead of these two well known projects.
There was not a simple answer for that.
However, something that we found really interesting and it helped to answer our question was we found that all Architecture Components' libraries working together in combination worked like a charm.

A couple of months ago we made the same question with another component.
We have a lot of extensions for handle navigation between fragments which are working fine, we have no major problems using that, they are stable but, shall we start using the Navigation component?
I'm a bit anxious, so I'll spoiler the answer. 
In this post I will tell you why we decided start using it and why we don't regret that decision.


# A bit of android navigation' history

I've been working on Android as first class language for more than 4 years.
If you have some time working on android you know that the navigation between views could be a real headache.

At the beginning of my experience, fragments were beginning to emerge.
So the the android apps were based basically by a set of activities.
Using that, we found some problems to manage the redirections, mainly if we wanted to share some data between activities.
In that case we had to create a parcelable class (make a parcelable class by hand has not any fun), build a bundle and then parse that bundle in the destination activity. 
Does it good?
No, it doesn't.
Does it safe?
No, it doesn't, but it was the way that we had to navigate between activities.

Then the fragments emerged with all the well known properties, and we decided to start using it.
The architecture was easy, each screen was composed of one activity which has only one fragment.
We used a similar redirection flow as the previews one. 
But it was even worst because if we wanted to share data between views, the fragment had to prepare an Intent with the bundle of data, then the destination activity had to parse it, it had to create a new bundle with all these data and share it to the new destination fragment.
As we can see it's an intricate flow, it's not safe and we had to type a lot of code when we wanted to create a new screen.
To avoid some of these problems we started to use some 3th party libraries such us [Dart] or [Fragment args] combined with [Parceler].

After that, Google proposed a single activity architecture application.
In that moment a new problem appeared, it's called [FragmentTransaction].
Manage only one fragment was easy, but manage multiple fragments in a single activity at the beginning was a real headache.
We created an awesome helper class, which we improved further with the kotlin arrived, which makes the redirection flow much easier.

Summarizing, at the end we had this awesome helper class, combined with [Fragment args], Kotlin's parzelize feature and an navigation architecture based on the Router pattern.
It's not so bad, right?

# What items do we want to improve?

We had a stable navigation flow, so at the beginning when I tried the new Architecture component I was not sure about what advantages it'll give us compared with our current implementation.

We created a list of issues that we had with our current implementation and we wanted to solve with the navigation component:
- The navigation flow is not clear, we have a router which is called to perform the navigation.
However, it's not clear who calls that router, it's not clear which view can redirect to which view.
Related with that, if we want to change a redirection sometimes is hard, you have to know the application flow map.
- Improve the deeplink redirection.
- Share data between fragments of a same flow.
- In some projects we were using MVP, and it's not good that the view saves the Presenter state.
It'd be nice if the presenter can save the state itself.

## Clear navigation flow 

Have a clear navigation flow is very important in all apps, and in some cases is hard to follow.
As a developer we can create a good documentation about the flow and sometimes a graphic diagram is very helpful asset to induce someone to a project.

The first impression of the Android Navigation component was excellent, the navigation graph is a very useful resource to understand hole an application.
The diagram contains the different flows and if you want to change someone you can just change the old destination for a new one easily.
If you don't know how to do that, you can follow [Google's documentation](https://developer.android.com/guide/navigation/navigation-create-destinations).

I had only one problem, the app started to grow up and the graph started to be unmaintainable, and it was hard manage all these app's screen in my small laptop screen.
In that moment I started to create [nested navigation graphs](https://developer.android.com/guide/navigation/navigation-nested-graphs).
That was an excellent idea, because I had a main graph with all nested graphs.

Do you remember [URL Splitting's] blog post?
It solves a similar situation in a web app.

Let’s introduce the same example: think of an e-commerce app.
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