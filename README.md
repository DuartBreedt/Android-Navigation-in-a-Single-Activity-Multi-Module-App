# Using The Navigation Component in a Single Activity Multi-Module Android App
## Summary

Google has made a point not to dictate how you structure your app. [Quote by android expert about not caring about architecture] However, they have made recommendations to help guide developers. Since Jetpack was announced in May 17, 2017 the way Google recommends you structure the project and the semantic meaning of the Android components has been evolving.

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

> TODO: Jetpack in general

Google introduced the navigation component as part of the Jetpack architecture components. This component handles fragment transactions, back stack management, transitions and animations, deep linking, and UI patterns.

- [Navigation Getting Started](https://developer.android.com/guide/navigation/navigation-getting-started)
- [Deeplinking with navigation component](https://medium.com/incwell-innovations/deeplink-and-navigation-in-android-architecture-component-part-3-b882ed5d5b32)
- [Splash screen using the navigation component](https://proandroiddev.com/android-navigation-component-tips-tricks-implementing-splash-screen-f0f5ce046a09)
 
## Our solution

Given the latest developments in Android we wanted to adhere to best practices as far as possible. This meant creating a modular, single activity app using the latest architecture components. However, there wasn't a lot of official documentation on using all of these best practices and structure recommendations together. So we struggled. And this is what we found.

### Modularization



### Single Activity

### Jetpack | Architecture | Navigation Component

Some useful features include being able to handle popping certain view off of the back stack upon navigating. This is useful for splash and login screens.

Setting up toolbars with NavigationUI.

Different approaches to achieving navigation in large apps

IDE cannot help you

- [Using Navigation Architecture Component in a large banking app](https://medium.com/google-developer-experts/using-navigation-architecture-component-in-a-large-banking-app-ac84936a42c2)
- [A quick glance on single activity approach with navigation component](https://android.jlelse.eu/a-quick-glance-on-single-activity-approach-with-navigation-component-f3b96b3b0a58)
- [GitHub | Navigation Single Activity](https://github.com/GabrielSamojlo/navigation-single-activity)
