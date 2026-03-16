---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:34"
_last_editor_used_jetpack: block-editor
_oembed_3d7d8048282165485148a06fbf535ca2: '{{unknown}}'
_oembed_ae2f608a641a552bc79406d45e7a5752: '{{unknown}}'
_publicize_job_id: "59486272001"
_thumbnail_id: "502"
author: gmirchev90
categories:
  - android
cover:
  alt: cast-01
  image: /wp-content/uploads/2021/06/cast-01.png
date: "2021-06-11T20:48:54+00:00"
guid: https://georgimirchev.com/?p=478
parent_post_id: null
post_id: "478"
summary: |-
  Recently I played around with the integration of [Google Cast inside Exoplayer](https://github.com/google/ExoPlayer/tree/release-v2/demos/cast) and I was really amazed to see how bad it is and how much it sucks. I decided to write this post with the idea it will help someone else who is also struggling with the Google Cast integration for exoplayer.

  ## Requirements

  So the requirements I had for the integration of Google Cast are quite simple:

  - Play an item on cast
  - Play a list of items - when you press Next you go to the next item
  - Forward / Backwards with 30 seconds - so you can skip through the track
  - Play / Pause support
  - Switch between lists of items - you can player item 2 of List A, you should be able to switch to item 3 of List B
  - Switch between phone playback and cast playback - switch from cast to app and from app to cast
  - Update the progress when casting - time elapsed should also be updated
  - Volume up/down are supported
tags:
  - google-cast
timeline_notification: "1623444538"
title: Why integration of Google Cast alongside Exoplayer really sucks?
url: /2021/06/11/why-integration-of-google-cast-alongside-exoplayer-really-sucks/

---
Recently I played around with the integration of [Google Cast inside Exoplayer](https://github.com/google/ExoPlayer/tree/release-v2/demos/cast) and I was really amazed to see how bad it is and how much it sucks. I decided to write this post with the idea it will help someone else who is also struggling with the Google Cast integration for exoplayer.

## Requirements

So the requirements I had for the integration of Google Cast are quite simple:

- Play an item on cast
- Play a list of items - when you press Next you go to the next item
- Forward / Backwards with 30 seconds - so you can skip through the track
- Play / Pause support
- Switch between lists of items - you can player item 2 of List A, you should be able to switch to item 3 of List B
- Switch between phone playback and cast playback - switch from cast to app and from app to cast
- Update the progress when casting - time elapsed should also be updated
- Volume up/down are supported

## Implementation approach

The implementation is a bit tricky but there are some obvious things it needs to support:

1. When you switch from SimpleExoPlayer to CastPlayer, cast player should continue from where SimpleExoPlayer left - the episode you listened to (author, title, icon), the time you reached, the next items to be continued with
1. It should also supports all of the already present things like play, pause, seek, next
1. It should also pass data back to SimpleExoPlayer when the user switches back to it

Both SimpleExoPlayer and CastPlayer extend from this BasePlayer interface so all options in point 2 should work straight out of the box. But is it like that? Let's see.

Sample code of the implementation:

https://gist.github.com/n0m0r3pa1n/d3484dce93464433e81065767d516619

So basically we have this single player class who knows both about SimpleExoPlayer and CastPlayer. It keeps as state the currentPlayer that is playing and when you switch from one to the other, it just replaces the currentPlayer instance.

But as you can already see, not everything is written in the best way it could be.

## Issues

### Keeping state locally

One of the things you will see is the mediaItemsCastQueue array of media items. You would expect that when you pass to a player a list of MediaItems, it is the one that knows about it so you don't have to keep it yourself. Right? Well, NO.

#### currentMediaItem is empty

When you try to do CastPlayer.currentMediaItem and if you check the current implementation of CastTimeline, you will see a shit there: [https://github.com/google/ExoPlayer/issues/8212](https://github.com/google/ExoPlayer/issues/8212)

getWindow(windowIndex) is the one that is used to get the MediaItem object. This method just always returns a new media items with some kind of a random ID set inside the tag object. So whenever you call CastPlayer.currentMediaItem, it is actually a new MediaItem with a random tag. Total bullshit.

In order to make it work you need to keep the list of MediaItems that you passed to the player, locally in your own custom player implementation. FYI: [Google does the same total bullshit](https://github.com/google/ExoPlayer/blob/release-v2/demos/cast/src/main/java/com/google/android/exoplayer2/castdemo/PlayerManager.java#L93).

#### currentWindowIndex is 0

Let's say your current player is CastPlayer. You switch from casting to the local phone player meaning from CastPlayer to SimpleExoPlayer. You listened to episode 5 out of 10, you call getCurrentWindowIndex() and it returns 0 and you are like "WTF is that !?".

For some reason, maybe becase the cast player got detached, the currentWindowIndex position is always 0 and when you switch to SimpleExoPlayer, it always starts from episode 1. You know what is the solution, right? Keep the lastWindowIndex state yourself ...

### MediaItemConverter from MediaItem to MediaQueueItem

The guys from Cast decided to support the MediaItem data class that is already present in SimpleExoPlayer. Previously, they used to have MediaQueueItem class that offered things like author, title of episode, subtitle and image. MediaItem has only title. That's all. You can imagine how different a player looks when you have all the details + an image and when you have one title + controls and nothing else.

And in order to get this better look, you have to register a mapper when you create your cast player, that looks like this:

https://gist.github.com/n0m0r3pa1n/c87c3dc224096fe30d5fa16f03db4f93

And why!? Come on guys, you transition to a new structure but drop support for the cooler look? I am really amazed what things are required in order to implement a decent looking feature.

### onMediaItemTransition not called when loading items

Another issue that popped up is that when you call loadItems on the SimpleExoPlayer and you call seekTo(position = 5) let's say, it would call the onMediaItemTransition callback to notify that a media item is being player. Well, guess what? Cast player does not invoke the callback in this case at all. You have to call it yourself.

### Loading items the deprecated way

We used to have the following approach when loading items to play in the player:

- Get a list of the items
- Pass this list to Player.setMediaItems
- Call seekTo(position) to start from item 5 for example
- Set playWhenReady = true

Cast player doesn't work that way. You would get an Invalid Request code that doesn't have any meaningful explanation that you can use to fix it. The error happens because the items are still being loaded when you call seekTo. That's why I ended up using the loadMediaItems method on the cast player which also accepts the position it should start from. There is no mention in. the docs that if you call seekTo immediately after setMediaItems, the player would not work.

## Conclusion

The current implementation of Google Cast inside ExoPlayer sucks. There are a lot of improvements to be made. And the above issues that I listed - I am sure there are even a lot more. I am not sure to say that "I hope to see improvements" because I really don't think there is any place for hope here. If Google sells Google Cast devices for 45 euro and they are the main contributor to Exoplayer - they should invest at least 25 euro of each sell into improving the shit they have made. Until then I just hope to never have to work with this piece of s\*\*\*dk anywhere soon. Thanks!
