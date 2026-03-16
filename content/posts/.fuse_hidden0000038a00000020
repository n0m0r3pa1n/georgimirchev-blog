---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:38"
_last_editor_used_jetpack: block-editor
_oembed_0ac720d88e11106bcc767d8d6243ecb3: <div class="embed-youtube"><iframe title="#Pragma Conference 2016 - John Sundell - Backend-driven UIs" width="750" height="422" src="https://www.youtube.com/embed/ypk-72mhYBk?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe></div>
_oembed_2be36c8e059c2ca0aa270f29819a410d: <div class="embed-youtube"><iframe title="droidcon SF 2018 - Using server-driven UI to rapidly iterate across platforms" width="750" height="422" src="https://www.youtube.com/embed/Nr8roecD-oU?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe></div>
_oembed_4a9e5001b31b3ee41bc6691fa5f7f86d: '<span class="embed-youtube" style="text-align:center; display: block;"><iframe class="youtube-player" width="750" height="422" src="https://www.youtube.com/embed/rE9ZE0DzLFs?version=3&#038;rel=1&#038;showsearch=0&#038;showinfo=1&#038;iv_load_policy=1&#038;fs=1&#038;hl=en&#038;autohide=2&#038;wmode=transparent" allowfullscreen="true" style="border:0;" sandbox="allow-scripts allow-same-origin allow-popups allow-presentation allow-popups-to-escape-sandbox"></iframe></span>'
_oembed_258e575b166f5d1e394ef6dfabc7ab80: '{{unknown}}'
_oembed_1571970fa8269b0286edb5a18d2e1c8f: <div class="embed-youtube"><iframe title="#Pragma Conference 2016 - John Sundell - Backend-driven UIs" width="1100" height="619" src="https://www.youtube.com/embed/ypk-72mhYBk?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe></div>
_oembed_d8fa3844cbf40475b7de468abe44937c: <div class="embed-youtube"><iframe title="droidcon SF 2018 - Using server-driven UI to rapidly iterate across platforms" width="1100" height="619" src="https://www.youtube.com/embed/Nr8roecD-oU?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe></div>
_oembed_time_0ac720d88e11106bcc767d8d6243ecb3: "1730558621"
_oembed_time_2be36c8e059c2ca0aa270f29819a410d: "1730558621"
_oembed_time_4a9e5001b31b3ee41bc6691fa5f7f86d: "1730558622"
_oembed_time_1571970fa8269b0286edb5a18d2e1c8f: "1724988467"
_oembed_time_d8fa3844cbf40475b7de468abe44937c: "1724988467"
_publicize_job_id: "79772506449"
author: gmirchev90
categories:
  - android
  - code-architecture
date: "2022-12-19T07:46:38+00:00"
guid: https://georgimirchev.com/?p=683
parent_post_id: null
post_id: "683"
summary: Server-driven UI is not a new concept. Neither for Android, iOS, or the web. It is quite popular in recent years, especially with the introduction of component-driven architectures such as Flux / Redux and others. Now three projects ahead with server-driven architecture in place, I want to share with you some of the challenges that we faced, and the different approaches to solving them. And I will be really happy if you share your experience too.
tags:
  - server-driven-ui
timeline_notification: "1671436005"
title: The challenges of server-driven UI on Android
url: /2022/12/19/the-challenges-of-server-driven-ui-on-android/
wordads_ufa: s:wpcom-ufa-v3-beta:1671436157

---
Server-driven UI is not a new concept. Neither for Android, iOS, or the web. It is quite popular in recent years, especially with the introduction of component-driven architectures such as Flux / Redux and others. Now three projects ahead with server-driven architecture in place, I want to share with you some of the challenges that we faced, and the different approaches to solving them. And I will be really happy if you share your experience too.

# Before we start - a good list of materials to go through

There are a lot of materials on the web regarding server-driven UI, a lot of medium articles, and a lot of old classical talks where you can get a general overview of the challenges ahead. Here are some of them:

## Videos

- droidCon SF 2018 - Using server-driven UI to rapidly iterate across platforms

{{< youtube Nr8roecD-oU >}}

- Pragma Conference 2016 - John Sundell - Backend-driven UIs

{{< youtube ypk-72mhYBk >}}

- Server-driven UI with Jetpack Compose - although I don't find this video very useful, it shows that Jetpack Compose hooks very well into our scheme with dynamic views

{{< youtube rE9ZE0DzLFs >}}

These two videos above are amazing to get the general challenges before you start your project. They touch on several things like rows vs columns, click handling, caching, and many more. The language that you will be using doesn't matter, just watch them and take notes.

## Articles

- [https://medium.com/swlh/server-driven-ui-and-some-herbs-f17f01aa7794](https://medium.com/swlh/server-driven-ui-and-some-herbs-f17f01aa7794) \- Basics on part of the challenges of the server-driven UI but nothing regarding the UI part
- [https://doordash.engineering/2021/08/24/improving-development-velocity-with-generic-server-driven-ui-components/](https://doordash.engineering/2021/08/24/improving-development-velocity-with-generic-server-driven-ui-components/) \-  Server driven UI with Swift UI and Jetpack Compose, lessons learned - versioning, keeping state when necessary inside components
- [https://proandroiddev.com/dynamic-screens-using-server-driven-ui-in-android-262f1e7875c1](https://proandroiddev.com/dynamic-screens-using-server-driven-ui-in-android-262f1e7875c1) \- overall look over the architecture, looking at different libraries like Litho, Epoxy, Jetpack compose
- [https://proandroiddev.com/server-driven-ui-using-jetpack-compose-8fae9889bb2b](https://proandroiddev.com/server-driven-ui-using-jetpack-compose-8fae9889bb2b) \- Example with Jetpack Compose

## Github Projects

- [Lona](https://github.com/Lona/Lona) \- tool to generate .component files that could be shared across platforms. Just interesting to take a look at.
- [Compose Server-Driven UI](https://github.com/AmosKorir/ComposeServerDrivenUI) \- a simple example with Jetpack Compose
- [One more example](https://github.com/ivanalvarado/server-driven-ui/) \- it also includes dynamic styling and theming as part of the dynamic views

# What are the challenges related to server-driven UI?

The answer to the above question depends on the requirements that are presented to you by the management. In almost all of the projects that I was working on, the requirement to support Rows and Columns was never there. It would consist simply of:

- We have a limited set of view types
- The data for each component is attached to its view type
- For each view type - show a specific view

But this simple at first look requirement brings its own challenges. And here are the most challenging ones:

## API design

This is the most important thing in the whole architecture of the project. If the backend API design is bad, you can really easily end up with messy architecture and hacks on the frontend side. It is really important that each step is synced with your backend team and their responsibility is not only to provide a JSON file. They have to implement the right API calls that will match the management requirements.

You can actually split up API design into two really simple cases:

1. APIs that control everything - they return a JSON that contains views, validators (objects used to validate the input of the views), current page data, next page URL, buttons that do different requests like POST/GET, etc. Basically, these types of APIs tend to control not only the views on the screen but also their behavior.
1. APIs that don't control everything - for example, you receive a button with the "Favorite Video" and the endpoint is hardcoded in the app. You have to do the request yourself and handle the failure cases (ex. the case where a button click takes the API 40 seconds). This also includes the case where you need to call different APIs yourself based on the view displayed on the screen. The backend controls what is displayed on the screen but not how it should behave.

These two types of API design will have a huge influence on how you will have the app architecture laid out.

### Case 1 - Backend in full control

You will have a structure fully controlled by the API regarding navigation, data submissions, data validation, error handling, progress display, etc. The backend will tell you where the data needs to go and will return you a list of the new components that you need to display and this would happen on each page that is filled and submitted by the user. In a way, the app will be a really dumb presentation layer that follows the instructions provided by the backend.

Every action that happens inside the app is sent to the backend and the backend always responds with view components back. This way you don't have to handle the state yourself. The state will always come from the backend. Even the error state too.

Let's give an example, let's say you have a movies app that displays a list of movies. Backend will provide the list of movie views, the container for each movie consisting of the option to add it to favorites and rate it, the nextPageUrl, and the next button that will do the request using the nextPageUrl. Whenever the user presses the "Favorite" button, you will do a request to an endpoint that the backend already put in the JSON response for this case and when the request is done, it will return a new list of views that should be displayed on the screen. You use DiffUtil to show them and update the state of the favorite button from unchecked to checked. This is the way the backend is always responsible to handle the state for you.

### Case 2 - Backend has limited control

In this case, a lot of the logic is left to the app to handle. In the example above with the "Favorite" button, the endpoint points to another server and you have to handle the display of the favorite button as checked yourself. This implies keeping the list of current views on the screen, modifying the state yourself, and making sure it is reflected in the UI. This would indirectly affect your app architecture and the complexity you have in the project. And it also introduces a lot of architectural questions.

## Lazy loading items

Challenge number 2 is to decide whether you have a lot of items to display or you have just a limited number of them. In my case, one of the projects was called "Dynamic Application Forms". For it, we didn't have a lot of components to show to the user, we had to show mostly input boxes, textviews, and buttons. So for that number of components, rendering them inside a LinearLayout(orientation = vertical) was totally fine.

For another project, we had a huge amount of items that would be displayed horizontally so creating and inflating items would result in an Out Of Memory exception. That's why we had to stick to using a RecyclerView with its lazy loading nature. This would result in us having a big number of ViewHolders and each ViewHolder matches a certain type of view that needs to be displayed.

So yes, the ultimate question is how many items do you expect so you can decide to use a simple container like LinearLayout or a container similar to RecyclerView?

## Application Architecture

The server-driven application architecture can be split into 2 small subsets. Views architecture and business logic one.

### Views architecture

It is concerned only with the way that you will parse the UI data and present the expected views to the user.

#### Parsing UI data

You will have different UI types and for each one, you need to parse the data individually. Usually, this is done by having a single interface like this one which is implemented by different \*Json classes:

```
interface UiComponentJson {
  @UiComponentType
  val type: String
}
```

Of course, the above approach is purely for orientation. You could use [Kotlin's sealed classes with a JSON parsing library like Moshi](https://www.google.com/search?q=moshi+sealed+class&sxsrf=AOaemvKfd_qgMcq8KIMwiz24s_VN1SZTbQ%3A1641663133754&ei=ncrZYfmfLamOxc8P1_qTuAc&oq=moshi+sealed+cl&gs_lcp=Cgdnd3Mtd2l6EAMYADIFCAAQkQI6BwgjELADECc6BwgAEEcQsAM6BAgjECc6CwguEIAEEMcBEK8BOgUIABCABDoKCAAQgAQQhwIQFDoGCAAQFhAeSgQIQRgASgQIRhgAUIEWWM8cYK8maAFwAngAgAGOAYgBzweSAQMxLjiYAQCgAQHIAQnAAQE&sclient=gws-wiz) and then just use a simple when statement to transform the components from network ones to the appropriate UI ones.

#### Presenting views

Presenting views is the other important aspect of the views architecture. Imagine you have two apps that need to reuse both of these dynamic components. One of them is a TV app and the other one is a mobile app. You need to think about the best approach for reusing those views and not making the hierarchy unnecessarily complex and prone to memory leaks.

In the above case, using custom views as your basis may result in a RecyclerView that doesn't know how to work exactly with them and reuse them properly. This will lead to views being rendered even though they are not present on the screen.

We had a lot of issues with custom views, especially on the rendering side. We would have a custom view that would render 10 or more items as a single element in the RecyclerView. Meaning all the 10 items were rendered although they were not visible. One of the experiments you can do is to use the ViewHolder pattern. All of the custom views are actually Recycler.ViewHolder-s. This way we can let the RecyclerView decide how to exactly reuse them. They are also applicable for the TV app and we are on our way to see how they will behave there.

Giving up custom views also has some drawbacks. ViewHolder's are hardly aware of the lifecycle. They don't have onAttach / onDetach. Some of them are being recycled so if you have something important happening inside of the view - it is hard to know where exactly to release it and how to manage it properly. But more on that topic in the business logic architecture.

#### Optimizing the RecyclerView

There is this really good article how to optimize the RecyclerView for nested RecyclerViews. [Read it!](https://medium.com/nerd-for-tech/nested-recycler-in-android-done-right-b101744e2a9a)

### Business logic architecture

There will be a lot of architecture questions that you will need to answer. Most of them relate to handling your business logic properly. And as we all know, in modern Android development this logic is mostly hidden behind a clean architecture and contained in Interactors or ViewModels.

When your backend controls everything, the biggest complexity comes from validating the input data. Other than that, all the other things seem simple - you have to always display what the backend tells you to display. If you have any questions - you ask the backend what to do or show.

When you have case 2, where your backend is slow or unresponsive and you have to hardcode some of the requests that need to happen on your side and then it is normal to think about this complexity. Each of these requests can have an error state and you need to handle it yourself. Here are some of the questions that arise in this situation:

- ViewModel per component & Jetpack Compose components that are more easily controlled by ViewModels
- Similar to Redux architecture that uses reducers and a single tree as state
- Caching of items and when to invalidate them
- and many more that we will discuss below

Let's discuss them in detail.

#### The ViewModel question

The questions of how to manage business logic and where to keep it are some of the tough ones. The good thing is that you have a limited set of opportunities. The bad thing is to decide which is the correct implementation.

##### Single ViewModel per fragment / activity

Single ViewModel is the approach where I would use in legacy projects where it is hard to change views so they fit your needs. It is really easy to have a single ViewModel as part of the fragment / activity that grows really fast. Extending from it or even composing functionality inside of it can be tricky and it becomes a God object really fast. The benefit is that for older projects it uses the Lifecycle of the Fragment / Activity so you don't have to care when it is killed by the system.

##### ViewModel per component / Jetpack Compose

This is the approach I would follow nowadays. Why is it good? A lot of complexities are hidden especially for the case where your app is not fully controlled by the backend. Each individual view has its own ViewModel that contains the logic for you. You spread this logic across different ViewModels and you don't end up with a huge ViewModel that you have to manage.

Jetpack Compose allows you to have a ViewModel per component. Otherwise, you will have to manage it yourself which will lead to more complexity inside your project.

Another benefit of this approach is that your items become small composable functions. You don't care about custom views, viewholders or handling lifecycle, etc. You just reuse these composable functions and they can work for TV too.

#### Caching

Caching is a really important subject that in theory should not affect your architecture but in practice actually does affect it. There are several problems that touch on the caching issue.

- Do you want to store the state of the components in case the user suddenly kills the app?
- Do you want to keep the position of the selected item so that when the user opens the app again, he is scrolled down to the item he was lastly playing with?

If you want to store positions or state, you will have to work with multiple sources of components - cache & backend and decide which has priority over what. Maybe there is some state in the cache that has not been synced with the backend and it needs to take priority, like the user checking a box while backend says the box should be unchecked.

Also, an in-memory cache will work if you want to store things temporarily, but if the process is killed, you will need a DB or [DataStore](https://developer.android.com/topic/libraries/architecture/datastore).

You have to think also how long you want to cache certain data. If it is a form that is populated by the user, it doesn't make sense to keep it once it is submitted. If the user logs out, all of the caching should be wiped out too.

### Communication between Business and ViewModel layers

This is one of the challenges imposed by the server-driven UI. You will find yourself often in a situation where you need to communicate a certain click event that should trigger a specific business logic attached to it. How do you do that?

As you can guess, there are several approaches to achieve this. The worst one is to directly access your business logic inside your custom views or viewholder. **And this is something you should never do!** All of the business logic should be covered with unit tests and UI components should not know about it.

#### Event channels

One of the approaches we did in my experience was to introduce 2 channels of data - UiEvents flow and NavigationEvents flow. These two flows, as the names suggest, are responsible for UI and navigation events happening in the app. So let's say you click on the Favorite button, the view component calls: uiEvents.emit(FavoriteClick) and the ViewModel listens for uiEvents.filter(FavoriteClick::class).collect(). When the appropriate business logic successfully finishes, we just call navigationEvents.emit(DetailsScreen) and this is then observed by the fragment which navigates to the Details screen.

#### UIData sources

The challenge also comes when you want to reflect some state which takes a long time for backend to reflect. Let's say you press on the Favorite button but it takes 40 seconds for backend to respond to you or even worse, it never responds but actually does the change. In this case, you should notify the user immediately that the state change is successful and if it fails, show some kind of a dialog later. (Yeah, I know it may sound wrong but this was the requirement at the time).

We ended up having multiple UiData sources that returned Flow<UiData>. All of these sources returned their own type of UiData - whether it will be a list of favored items, a list of icons, or anything else that extends from UiData. We then combine all of these flow emissions into one. This way we end up having one big tree of state which is being constructed from different small sources. And if one of them changes, all of them emit.

So in the case of the favorite button being clicked, we emit the FavoriteClick event, this event is handled by the ViewModel which calls an interactor that ends up using the FavoriteMoviesUiDataSource which emits the request, looks up through the list of cached movies, updates the state of the movie with a given ID to be favorite: true and emits it back to the UiData sources list which will end up refreshing the whole tree. As we use DiffUtil for the RecyclerView, it will only update the part that has actually changed.

# Final words

As you can see, there is a lot going on for the server-driven UI topic. There are many challenges and the frontend and backend architectures are really tightly coupled. They depend on each other. And close collaboration is the key to success. Technologies like Jetpack Compose are the key to use and they can save you a lot of effort. Especially when it comes to relying on the android lifecycle package. Have you tried it out? Feel free to share your learnings and I will add them in this post. I hope that also my learnings helped you to develop better apps with server-driven UI.
