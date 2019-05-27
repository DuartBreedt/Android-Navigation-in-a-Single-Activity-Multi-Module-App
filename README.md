# Using The Navigation Component in a Single Activity Multi-Module Android App
## Summary

Google has made a point not to dictate how you structure your app. However, surprisingly, they have now made recommendations to help guide developers. Since Jetpack was announced in May 17, 2017 the way Google recommends you structure the project and the semantic meaning of the Android components has been evolving.

Each section below serves as an overview. Under each of the sections I provide all the resources I used to come to grips with what eventually became our architecture. So if the overviews are too brief please peruse the resources to get a better understanding.

- [Jetpack](https://developer.android.com/jetpack)

## Single Activity Architecture

One of the core components Jetpack introduced are the architecture components. The navigation component is one of these architecture components. The development of these architecture components lead to Google being more explicit with how to use Android components. Specifically, Google now officially recommend new Android applications are created as single activity applications.

> Today we are introducing the Navigation component as a framework for structuring your in-app UI, with a focus on making a single-Activity app the preferred architecture.

Google defines an activity as the entry point of your application. Therefore, having multiple activities indicates a user should be able to enter the app at the start of any one those activities. 

- [Single Activity: Why, When, and How](https://www.youtube.com/watch?v=2k8x8V77CrU&feature=youtu.be)
- [A Single-Activity Android Application. Why not?!](https://medium.com/rosberryapps/a-single-activity-android-application-why-not-fa2a5458a099)
- [Introducing Jetpack](https://android-developers.googleblog.com/2018/05/use-android-jetpack-to-accelerate-your.html?m=1)

## Multi-Module App

Google also introduced a new application serving model called Dynamic Delivery. This defers APK generation to Google Play. Google Play uses Android App Bundles to create better optimized APKs for wider support. This now also gives developers the opportunity to use dynamic feature modules. Allowing certain features and assets to be downloaded later as the users need them. 

Over and above the obvious benefits of modular applications Dynamic Delivery creates a large incentive for designing modular apps. Furthermore, Google also recommends you modularize your apps.

- [Build a Modular Android App Architecture (Google I/O'19)](https://www.youtube.com/watch?v=PZBg5DIzNww)
- [Modularize your app](https://developer.android.com/studio/projects/dynamic-delivery#modularize)
- [Intro to app modularization](https://proandroiddev.com/intro-to-app-modularization-42411e4c421e)
- [Optimize your app](https://developer.android.com/studio/build/optimize-your-build)

## Jetpack | Architecture | Navigation Component

> Jetpack is a suite of libraries, tools, and guidance to help developers write high-quality apps easier. These components help you follow best practices, free you from writing boilerplate code, and simplify complex tasks, so you can focus on the code you care about.

Google introduced the navigation component as part of the Jetpack architecture components. This component handles fragment transactions, back stack management, transitions and animations, deep linking, and UI patterns.

- [Navigation Getting Started](https://developer.android.com/guide/navigation/navigation-getting-started)
- [Deeplinking with navigation component](https://medium.com/incwell-innovations/deeplink-and-navigation-in-android-architecture-component-part-3-b882ed5d5b32)
- [Splash screen using the navigation component](https://proandroiddev.com/android-navigation-component-tips-tricks-implementing-splash-screen-f0f5ce046a09)
 
## Our solution

Given the latest developments in Android we wanted to adhere to best practices as far as possible. This meant creating a modular, single activity app using the latest architecture components. However, there wasn't a lot of official documentation on using all of these best practices and structural recommendations together. So we struggled. But we tried to do it incrementally. And this is what we found.

### Modularization

After some deliberation we settled on creating high-level feature modules with the intent of modularizing further when the need arises. We also had a module which depended on these feature modules and essentially coordinated them. Across the project there was a lot of code that was used by all the modules. Examples include the communication interfaces, navigation, input validations,  and authentication. Therefore, a common module was created called _core_. The architecture can be visualized below.

![Multi-module architecture](https://github.com/DuartBreedt/Android-Navigation-in-a-Single-Activity-Multi-Module-App/blob/master/resources/Android%20Architecture.png "Multi-Module Architecture")

_feature one_ has to be aware and therefore dependent on _feature two_ in order to navigate to it. Conversely, _feature two_ has to be aware and therefore dependent on _feature one_ in order to navigate to it. If this is a scenario then there will be circular dependency between the two features. Therefore, static objects were made in _core_ with interfaces defining navigation which could be consumed by the features. The _app_ feature then overrided the interface methods to define the behaviour. This was a gross way of solving navigation but it worked until we implemented the navigation component.

### Single Activity

Initially we had two activities. One for login and another for the app's main page. After doing some reading it seemed Google recommended creating a single activity and swapping out fragments within that activity using the Navigation component. Since we wanted to solve the hacky navigation job it seemed single activity app architecture was the way to go. 

First off, we had to convert our existing activities to fragments. Then we had to create a new activity called _MainActivity_ - very creative, I know - which would then swap these activities in and out. At first this seemed quite straight forward, but there were a few gotchas. 

Most notably, we could no longer navigate to another screen which in this case was now a fragment. If we did want to navigate between fragments we would need to use the `FragmentManager` and `FragmentTransactions`. But this was a lot of work to be done only to be replaced by the navigation component. Therefore, for the time being our app could simply not navigate. 

Another issue was that `setSupportedToolbar` is a method which belongs to activities. So how the hell do we add toolbars to fragments? We ignored this problem in hopes that navigation components will maagically solve this for us, which they thankfully did.

### Jetpack | Architecture | Navigation Component

Please go read up a bit on the navigational component if this doesn't make complete sense.

The navigational component requires the following to be defined: navigation host, navigation graph, and navigation controller. 

#### Navigation host

The navigation host is an empty container where the destinations are swapped in and out. This is typically an XML fragment. In our case the navigation host was located in the newly created _MainActivity_.

#### Navigation graph
The navigation graph is a resource which describes destinations (fragments) and those destinations' possible actions (moves to other destinations).
Note that this doesn't create a direct dependency to the fragments but simply catalogs them using their fully qualified name. 

All features and fragments need full awareness of this navigation graph if they wish to navigate to a fragment defined in it. Therefore, this made the _core_ module the optimal place to keep the navigation graph.

> ![Navigation Graph](https://github.com/DuartBreedt/Android-Navigation-in-a-Single-Activity-Multi-Module-App/blob/master/resources/nav_graph.png "Navigation Graph")

Typically you can use the navigation graph editor to detect the navigation host, add new destinations, and add new actions. 
However, due to the project being separated into multiple modules Android Studio isn't smart enough to detect the destinations and navigation host through the graph editor. Therefore, you do not have the luxury of using the graph editor but since the resources squash down to the same file, at compile time it all works itself out in the end.

### Navigation controller

The navigation controller is an object provided by the navigation host. This can be accessed by a fragment, view, or activity. This simply gives you the ability to navigate to one of the destinations described in the navigation graph. 

- [Using Navigation Architecture Component in a large banking app](https://medium.com/google-developer-experts/using-navigation-architecture-component-in-a-large-banking-app-ac84936a42c2)
- [A quick glance on single activity approach with navigation component](https://android.jlelse.eu/a-quick-glance-on-single-activity-approach-with-navigation-component-f3b96b3b0a58)
- [GitHub | Navigation Single Activity](https://github.com/GabrielSamojlo/navigation-single-activity)
