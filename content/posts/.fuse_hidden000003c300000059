---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:22"
_oembed_b86188023b026549fb285de5487e99ef: '{{unknown}}'
_publicize_job_id: "43822865164"
author: gmirchev90
categories:
  - android
  - code-architecture
date: "2020-05-04T12:39:52+00:00"
guid: http://georgimirchev.com/?p=142
parent_post_id: null
post_id: "142"
summary: |-
  ## The use case

  I am working in a company here in Bulgaria where we are building a huge android app with over 250 screens. It was time to integrate Firebase push notifications in it. Here were the requirements for them:
tags:
  - architecture
timeline_notification: "1588595998"
title: Sample architecture to handle notifications in Android
url: /2020/05/04/sample-architecture-to-handle-notifications-in-android/

---
## The use case

I am working in a company here in Bulgaria where we are building a huge android app with over 250 screens. It was time to integrate Firebase push notifications in it. Here were the requirements for them:

- When the app is open notifications should
  - Appear on some screens
  - Not appear on other screens
  - Some notifications may be handled by fragments or activities
- Each notification has a type
- Each type of notification leads to different screens
- Notifications received when the app is open should not be visible when the app is closed
- Some notifications with a certain type will get the text from the server, others will have their own
- If a notification is received, but the user does not click on it and he manually opens the appropriate screen to which the notification should follow, the notification should disappear

## The idea

Our idea was to use a method very close how to how the [Touch Framework in Android works](http://nmp90.com/2017/01/mastering-android-touch-system/). It is implemented using inheritance. You can try a different approach by using composition.

Our main goals were:

- Not have any casting or type checking of the notifications in the app
- Ask each Activity/Fragment if it wants to handle the notification or not

So this is what we do. We ask each activity/fragment if they want to handle the notification by using our NotificationListener interface, if not – we let our default notification handling work. For different types of notifications, we have different handle methods.

```
interface NotificationListener {
  boolean handleTicketNotification(TicketNotification notification);
  boolean handlePaymentNotification(PaymentNotification notification);
}
```

## Overall structure

- MyFirebaseMessagingService – our service which extends FirebaseMessagingService and listens for incoming notifications. It then broadcasts them through the LocalBroadcastManager
- BaseActivity – a class extended by all activities with the common logic of handling notifications
- NotificationListener – an interface which has methods returning boolean for each type of notification. Its return type is used to determine whether an activity/fragment wants to override the display of a notification or not
- NotificationManager – small class to handle listening to the types of notification coming from the MyFirebaseMessagingService

{{< figure src="https://web.archive.org/web/20190102182103im%5F/http://nmp90.com/wp-content/uploads/2017/03/Tide-Notifications-Diagram.png" alt="" caption="" >}}

## Fighting the problems

### Helper class

We extracted the notification receiving logic in a small notification Manager class. It creates an intent filter for a given activity and subscribes to the incoming notifications using the LocalBroadcastManager:

```
public class NotificationManager {

    private final Activity activity;
    private BroadcastReceiver notificationsReceiver;

    public NotificationManager(Activity activity) {
        this.activity = activity;
    }

    public void start(NotificationListener notificationListener,
                      UnhandledNotificationListener onNotificationNotHandled) {
        notificationsReceiver = new BroadcastReceiver() {
            @Override
            public void onReceive(Context context, Intent intent) {
                if (intent.hasExtra(Constant.NOTIFICATION)
                        && Constant.ACTION_NOTIFICATION_RECEIVED.equals(intent.getAction())) {

                    BaseNotification notificationData = NotificationPaser.getNotificationData(intent.getExtras());
                    if(!notificationData.handleNotification(notificationListener)) {
                        notificationData.onNotificationNotHandled(onNotificationNotHandled, intent);
                    }
                }
            }
        };

        IntentFilter filter = new IntentFilter();
        filter.addAction(Constant.ACTION_NOTIFICATION_RECEIVED);

        LocalBroadcastManager.getInstance(activity).registerReceiver(notificationsReceiver, filter);
    }

    public void stop() {
        LocalBroadcastManager.getInstance(activity).unregisterReceiver(notificationsReceiver);
    }
}
```

### Notifications should appear on some screens and not on other screens

#### Activities

For activities, we have the BaseActivity class which is extended from each Activity. So it implements the NotificationListener interface and each activity which extends it, decides whether to override a given ticket method or not.

```
abstract class BaseActivity extends Activity implements NotificationListener {

    @Override
    void onStart() {
      // UnhandledNotificationListener is the default handling if the handleMethods return false
      notificationManager.start(this, new UnhandledNotificationListener() {
            @Override
            public void onTicketNotHandled(TicketNotification ticketNotification, Intent intent) {
                displayTicketNotification(ticketNotification, intent);
            }

            @Override
            public void onPaymentNotHandled(PaymentNotification paymentNotification, Intent intent) {
                displayPaymentNotification(paymentNotification, intent);
            }
      }
    }

    @Override
    public boolean handleTicketNotification(TicketNotification ticketNotification) {
        return false;
    }

    @Override
    public boolean handlePaymentNotification(PaymentNotification ticketNotification) {
        return false;
    }
}
```

#### Fragments

For fragments, we didn’t have such a base class as we had in the activities so we decided to let each fragment implement the NotificationListener interface. So we get the visible fragment in the activity, check if it an instance of a NotificationListener interface and pass the data to it by casting it.

```
class TabsActivity extends BaseActivity {
    @Override
    public boolean handlePaymentNotification(PaymentNotification paymentNotification) {
        if (getNotificationListenerFragment() != null) {
            return getNotificationListenerFragment().handlePaymentNotification(paymentNotification);
        }
        return false;
    }

    public NotificationListener getNotificationListenerFragment() {
        NavigationController controller = ((NavigationController) tabsController.getVisibleFragment());
        if (controller != null && controller.getVisibleFragment() instanceof NotificationListener) {
            return (NotificationListener) controller.getVisibleFragment();
        }

        return null;
    }
}
```

### Different text for different notifications

So on some screens user should not see the text for a notification, on other screens he should see it. So if you receive a notification for the update of your bank account number, while you are logged out, you should not see the text (you should not receive this type of notification, but that is for another post).

We user the strategy pattern for this case. We have three different ways of building notification text: VisibleTextBuilder, HiddenTextBuilder, GenericTextBuilder. All of these are called in the BaseActivity when the notification is received and they extend the NotificationTextBuilder interface. Based on the notification type, we use a different type of text builder.

So our displayTicketNotification and displayPaymentNotification methods use one of the mentioned text builders to display a given notification. Each of these methods is public, so an Activity can modify the text, notification pending activity and the data of the notification.

```
     protected void displayTicketNotification(TicketNotification ticketNotification, Intent intent) {
        // Dagger used to inject the appropriate text builder
        NotificationTextBuilder ticketNotificationTextBuilder = getComponent()
                .plus(new NotificationModule(ticketNotification.getMessageId(), ticketNotification.isAuthenticatedMessage()))
                .getNotificationTextBuilder();

        CustomNotificationBuilder builder = CustomNotificationBuilder
                .builder(ticketNotification, this)
                .setTicketNotificationTextBuilder(ticketNotificationTextBuilder)
                .setNotificationPendingActivity(ticketNotification.isAuthenticatedMessage() ? LoginSecurityCodeActivity.class : ContactActivity.class)
                .setIntent(intent);

        builder.showNotification(ticketNotification.getTicketId());
    }
```

### Navigation for different types

This is accomplished by Overriding the appropriate display\*\*\*\*Notification method in each activity which wants to modify the screen which should open when the notification is received. Each activity works with the builder and builds its own notification.

### Notification Types

#### Hierarchy

All of the received notifications have at least 3 common fields of data. That’s why we made the BaseNotification class. And by using Jackson, we can easily tell it what instance it should create based on the type of a certain field. The code below does the following:

- If the notificationType field equals “NEW\_QUEUED PAYMENT” create an object of PaymentNotification class
- If the notificationType field equals “MESSAGE\_RECEIVED” create an object of TicketNotification class
- Otherwise create an object of UnknownNotification class

PaymentNotification, TicketNotification and UnknownNotification all extend from the BaseNotification class.

```
@JsonTypeInfo(
        use = JsonTypeInfo.Id.NAME,
        include = JsonTypeInfo.As.PROPERTY,
        // if the field contains unknown value
        defaultImpl = UnknownNotification.class,
        property = "notificationType",
        // add the notificationType to the class, otherwise it won't
        visible = true
)
@JsonSubTypes({
        @JsonSubTypes.Type(value = PaymentNotification.class, name = "NEW_QUEUED_PAYMENT"),
        @JsonSubTypes.Type(value = TicketNotification.class, name = "MESSAGE_RECEIVED")
})
public abstract class BaseNotification implements Serializable {
    private NotificationType notificationType;
    private String title, body, sound;

    public abstract boolean handleNotification(NotificationListener notificationListener);

    public abstract void onNotificationNotHandled(UnhandledNotificationListener onNotificationNotHandled, Intent intent);

}
```

#### Escaping casting

To escape the casting we let the notification objects themselves to work with the NotificationListener object. This way we can pass an object of BaseNotification to the activity and let the activity pass itself to it. So we won’t cast to Payment or Ticket notification, we just call BaseNotification.handleNotification(this) method in the activity and escape the casting problem. Each subclass of BaseNotification decides how and when to call the notification listener with the parsed data.

```
public class TicketNotification extends BaseNotification implements Serializable {
    private boolean authenticatedMessage;
    private String ticketId, messageId;
    private int unreadMessageCount;

    @Override
    public boolean handleNotification(NotificationListener notificationListener) {
        return notificationListener.handleTicketNotification(this);
    }

    @Override
    public void onNotificationNotHandled(UnhandledNotificationListener onNotificationNotHandled, Intent intent) {
        onNotificationNotHandled.onTicketNotHandled(this, intent);
    }
}
```

### Close notifications which were not clicked

So if a user opens a screen for which he sees a notification, but doesn’t tap on it, the notification should disappear. The simple solution was to set a notificationId the same as the ticketId, so when the user opens the TicketDetails screen for a certain ticket, the screen calls notificationManager.cancel(ticketId) and the notification for this ticket is removed. Simple as that.

### Remove notifications when app is closed

To do that you need to register an [ActivityLifecycleCallback](https://developer.android.com/reference/android/app/Application.ActivityLifecycleCallbacks.html). All you need to do is call: registerActivityLifecycleCallbacks(new LogoutLifecycleListener(this)); in your custom application class. So then:

```
public class LogoutLifecycleListener implements Application.ActivityLifecycleCallbacks {
    @Override
    public void onActivityPaused(Activity activity) {
         NotificationManager nMgr = (NotificationManager) context.getSystemService(Context.NOTIFICATION_SERVICE);
         nMgr.cancelAll();
    }
}

```

## Closing words

In the current project I work on, we use inheritance a lot, which I really don’t like. But I think the approach we took to handling notifications, in this case, does its job really well. You can easily extend certain methods and now work with the notification yourself. I would prefer to use composition and an approach closer to OOP so I can delegate most of the work to the objects rather than to use inheritance, but this is how the touch framework in Android works and it does it’s job well.

I would be very happy if someone shares an alternative to our approach. I hope that this article helped someone to handle notifications in their project. Cheers!
