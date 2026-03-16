---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:23"
_publicize_job_id: "43823317139"
author: gmirchev90
categories:
  - android
  - code-architecture
date: "2020-05-04T12:54:35+00:00"
guid: http://georgimirchev.com/?p=155
parent_post_id: null
post_id: "155"
summary: |-
  I found Flux architecture for several months and I have written a couple apps with React & Flux and as an Android programmer also I think that it has to have an implementation.

  So I decided to do it. You can check it [HERE](https://github.com/n0m0r3pa1n/android-flux-example). I really believe that Flux for Android will be a “groundbreaker” (may be not my try but the whole concept :)).
tags:
  - flux
timeline_notification: "1588596878"
title: Android Flux Architecture Example
url: /2020/05/04/android-flux-architecture-example/

---
I found Flux architecture for several months and I have written a couple apps with React & Flux and as an Android programmer also I think that it has to have an implementation.

So I decided to do it. You can check it [HERE](https://github.com/n0m0r3pa1n/android-flux-example). I really believe that Flux for Android will be a “groundbreaker” (may be not my try but the whole concept :)).

Here is the explanation of the Github project for the interested:

n Flux we have: Actions, Dispatcher, Stores, Custom Components(React Components in example for web) and may be an Utils to generate the actions. I have implemented them as it follows:

1. Actions – We have a simple actions class containing just a constant. This action constants will help stores recognize if they need to respond to a given action.
1. Dispatcher – I have implemented the Otto bus as Dispatcher. It is used just to send events to all stores.
1. Dispatcher Events in Flux **WEB** – each object a dispatcher sends contains
   - ActionType – this is a server action or a view action (like load apps from server or user clicked the x button).
   - Action – this is the name of the action as a string. Stores know what to do by the action name – they filter it in a switch and decide if it is for them or not.
   - Data – an object with the received data (server data or ui).
1. Dispatcher Events in Flux **Android** (this is how I have implemented them)
   - We have a BaseEvent class and a Server/Ui Event classes. The BaseClass contains – actionType, action and data
   - Each store can subscribe to a Server or Ui event by using the @Subscribe annotation from Otto and expect one of the two classes
   - Data is transferred as String. Each store has to deserialize it to the class it expects using Gson.
   - Events can contain success and error message so a notification can be displayed if something has messed up.
1. Stores in **WEB**–
   - They use callbacks to notify the components which are subscribed to them that there is new data.
   - There is mainly one callback used which is ‘onChange’
   - When notified components get the new data from the Store from a public store method and update themselves.
1. Stores in **Android** (my implementation)
   - We have a BaseStore class which has one available listener – OnChangeListener. BaseStore has a list of callbacks implemented and they can be called with the onChange() method.
   - Each store can provide additional listener. I don’t think that more than 1 additional in rare cases will be needed.
   - Stores subscribe using the @Subscribe annotation from Otto. They can decide if they want to receive Server or Ui events.
   - ( **Update – 23.05**) I figured out that Stores are like the EventBus so I made an implementation which extends the Bus from Otto and doesn’t need listeners. Components subscribe to the bus using the Subscribe annotation for an event which is a public inner class within the store.
   - Stores filter if the event is about them by the event action string.
1. Custom components (in **WEB** they are React components) –
   - We have the AppsList custom component. It attaches to the AppStore in onAttachToWindow and removes in onDetachFromWindow methods.
   - The component rerenders by itself when it receives notification from the store.
   - I LOVE THE FLEXIBILITY OF THE COMPONENTS
1. Utils – we have the AppsAPI which simulates a server request.

You can check that when you load apps in the Component activity, they are loaded in the MainActivity also. Works like magic.

Feel free to make pull requests or comments. I will be very happy if we figure out a better design and architecture. I really enjoy the Flux idea and I think it is wonderful for Android. Events for the win!
