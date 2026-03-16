---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:40"
_last_editor_used_jetpack: block-editor
_oembed_3f478dbf4e281f95b91cff63345eaa0e: <div class="embed-youtube"><iframe title="Scrolling issue on Android TV" width="1100" height="619" src="https://www.youtube.com/embed/vLRSttu23K4?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe></div>
_oembed_82be122bea91cca6dfc3749efde80cf9: <div class="embed-youtube"><iframe title="Android TV scrolling issue fixed" width="750" height="422" src="https://www.youtube.com/embed/CuekpjKSpCM?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe></div>
_oembed_215ff4974d5658c86e1400541121a069: <div class="embed-youtube"><iframe title="Android TV scrolling issue fixed" width="1100" height="619" src="https://www.youtube.com/embed/CuekpjKSpCM?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe></div>
_oembed_0663dd56296375d1ee0b13585d986624: '{{unknown}}'
_oembed_619693e89f9469fee5fb737047f03848: '{{unknown}}'
_oembed_bcda7eeec7dcec1f37b51d4103b9509a: '{{unknown}}'
_oembed_ed8cc423eaf0b2985ce09dd9e419a500: <div class="embed-youtube"><iframe title="Scrolling issue on Android TV" width="750" height="422" src="https://www.youtube.com/embed/vLRSttu23K4?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe></div>
_oembed_time_3f478dbf4e281f95b91cff63345eaa0e: "1724998991"
_oembed_time_82be122bea91cca6dfc3749efde80cf9: "1730558624"
_oembed_time_215ff4974d5658c86e1400541121a069: "1724998992"
_oembed_time_ed8cc423eaf0b2985ce09dd9e419a500: "1730558623"
_publicize_job_id: "74456972614"
author: gmirchev90
categories:
  - android
date: "2022-07-07T08:58:03+00:00"
guid: https://georgimirchev.com/?p=837
parent_post_id: null
post_id: "837"
summary: |-
  We had a project where we were aiming to reuse the same codebase across mobile and TV. You have this care very often and there is this consideration of:

  > Should we use the Leanback Fragments?
  >
  > The experienced Android TV developer

  that is always ignored. We give the project a quick go and check how it works on the Android TV emulator and decide it is not worth the effort to work on TV specific codebase that may require additional devs to support it. And this is where hell breaks loose.
tags:
  - androidtv
  - fast-scroll
  - horizontalgridview
  - recyclerview
  - verticalgridview
timeline_notification: "1657184286"
title: RecyclerView loses focus when scrolling fast ... or how to use it on Android TV
url: /2022/07/07/recyclerview-loses-focus-when-scrolling-fast-or-how-to-use-it-on-android-tv/

---
We had a project where we were aiming to reuse the same codebase across mobile and TV. You have this care very often and there is this consideration of:

> Should we use the Leanback Fragments?
>
> The experienced Android TV developer

that is always ignored. We give the project a quick go and check how it works on the Android TV emulator and decide it is not worth the effort to work on TV specific codebase that may require additional devs to support it. And this is where hell breaks loose.

## The focus issue on Android TV

As you know, Android TV has a really different behavior from the Android mobile ecosystem. You don't have the idea of a scroll, instead, you have the idea of focus change. You use the DPAD to change focus from one view to the other. DPAD / Remote control is essentially your tool to navigate between views.

And usually, design for Android TV relies mostly on big images and a lot of rows to choose from. Whether it will be movies, categories, and others, the long text description is not your best friend in the TV world. And here comes our issue.

We have a vertical RecyclerView that holds many horizontal RecyclerViews which display their own content like Images and various other formats. In a way it looks like this:

{{< figure src="/wp-content/uploads/2022/07/image-2.png?w=1024" alt="" caption="" >}}

So now imagine you have to navigate on this page. You are on the first horizontal recycler, you press down, and you go to the element in the second recycler that is exactly below the first one.

But if you keep the down button pressed to simulate a fast scroll, then suddenly, the focus is lost and the side navigation menu is opened. And you are baffled? What the hell just happened, why can't I scroll normally and just continue scrolling til I reach the end of the vertical recycler?

https://youtu.be/vLRSttu23K4

## What happened?

Well, it is time to say hello to the amazing world of Android TV where the focus is king. Here, in the above design, there are several things that happen behind the scenes that you should consider. They are especially true if you use RecyclerView. It is not optimized well enough for Android TV and that's why you will get a lot of strange things happening.

## The tests

There were several tests to be done:

1. Vertical scrolling tests
1. Horizontal scrolling tests
1. Keeping the down button pressed
1. Pumping the down button fast
1. Keeping the right button pressed
1. Pumping the right button fast

In all of these tests, you could see different behavior - sometimes focus is lost and opens the navigation menu, sometimes it moves to the first element of the horizontal recycler view instead of loading the next elements, sometimes it goes up for no reason. But there was one general thing to be noticed. Focus is totally messed up. Whether you pump or hold the button down, it just doesn't work properly.

From the investigations that I did, I could see the onFocusSearchFailed getting called by the RecyclerView. This method would then search for the previous view which was selected, which in this case is the navigation view. So whenever we lose focus, it would try to focus the navigation view on the left side.

## Part of the issues we had in our codebase

1. Image loading - you may be using a library to load all of the images. In our case this was Glide. And we just didn't share properly the Glide instance and its caching strategies.
1. Inflation of horizontal RecyclerViews - have you played around with optimizing the inflation stage of these horizontal RecyclerViews. All of this takes time and when you do a fast scroll, it all gets messed up.
1. Custom scrolling behavior to keep the selected row vertically centered
1. RecyclerView does not exactly fit our needs - seems that elements like VerticalGridView and HorizontalGridView behave
1. Keeping the position where you scrolled and restoring it back - if you have scrolled any of the horizontal RecyclerViews you probably want to return the user back to the point where he scrolled whenever he goes up or down. This is not supported by default by the system and you have to do it yourself. But where to do it? We will check this out.
1. Diffing items properly - have you used ListAdapter that supports easy diff-ing. Well, it may cause you issues focus issues.

What exactly happens when you keep the down button pressed, is that you initiate the fast scroll behavior. Android starts loading fast all of the horizontal recycler views in the vertical one and because of all of the things happening like image loading, restoration of scrolling position, and others, focus just gets messed up.

## Losing focus on Android TV is a popular topic

1. [Glide loses focus](https://github.com/bumptech/glide/issues/3599)
1. [Prevent RecyclerView from losing focus](https://stackoverflow.com/questions/60223322/prevent-recyclerview-from-losing-focus)
1. [Android TV RecyclerView focus interaction](https://stackoverflow.com/questions/39247993/android-tv-recyclerview-focus-interaction)
1. [Slowing down Android GridView scroll speed](https://stackoverflow.com/questions/43215930/how-to-slow-down-android-gridview-scroll-speed-on-button-pressed-leanback)
1. [Horizontal RecyclerView focus going cyclic](https://stackoverflow.com/questions/48129423/horizontal-recyclerview-in-tv-dpad-focus-going-cyclic-on-fast-scroll)
1. [Wrong view to focus](https://issuetracker.google.com/issues/37067220?pli=1)

And manyothers. You can see this one is a popular problem. The solutions mentioned - they just never worked for us. I had to think about all of the mentioned problems and start fixing them one by one and hope this will give me good results. Otherwise, after banging my head for 3 days with different hacks, the only one that seemed to work 80% of the time is this ugly piece of bullshit code:

```
private val previousOnDpadDownTime = 0L
private val SCROLL_THRESHOLD = 250
override fun onKeyDown(keyCode: Int, event: KeyEvent?): Boolean {
        if (keyCode == KeyEvent.KEYCODE_DPAD_DOWN) {
            val current = System.currentTimeMillis()
            if (current - previousOnDpadDownTime > SCROLL_THRESHOLD) {
                previousOnDpadDownTime = current
                return super.onKeyDown(keyCode, event)
            }

            return true
        }
        return super.onKeyDown(keyCode, event)
    }
```

This delays the events that are received from the KEYCODE\_DPAD\_DOWN by 250 ms. So if you keep the button pressed, the events that have 250 ms between them will be received. This will give time for Glide to load the images and for the RecyclerView to inflate the views and continue down.

But this is ugly as shit and I hate it. That's why I decided to dig a bit more.

## Fixing the problems

### ImageLoading + caching

I could see Glide is a huge problem for us. If I put a static image inside the ImageView and not use Glide at all, the focus would not be lost 90% of the time. There was a rare case where it was lost but we could live with it. So I decided to look into our implementation of Glide and the RequestManager.

I created a CustomImageView that was used everywhere where we called RequestManager from Glide. This way I could introduce disk caching for the images and maybe this could speed up the whole process. The function call looks simple as this one:

```
fun loadImageWithUrl(url: String, hasRoundedCorners: Boolean = false) {
        val glideRequest = glideRequestManager
            .load(url)
            .placeholder(R.drawable.circular_progress)
            .diskCacheStrategy(DiskCacheStrategy.ALL)

        if (hasRoundedCorners) {
            glideRequest.apply(glideRequestOptions)
        }

        glideRequest.into(this)
    }
```

Another thing that helped us is setting the minHeight and minWidth of the ImageView. This helps the RecyclerView to expect the size of the image so it lays out views faster.

Small optimization is that I also put the placeholder in the init block of the ImageView. I wanted it to be there before we even call glide. By using setImageDrawable with the same placeholder as the one in Glide, later when the image was loaded, the placeholder was removed. But we had the placeholder upon the creation of the ImageView and not when loading is called. So this could speed up the process a bit.

```
init {
        minimumHeight = 100
        minimumWidth = 10
        setImageDrawable(ContextCompat.getDrawable(context, R.drawable.circular_progress))
    }
```

### Optimizing inflation of nested RecyclerViews

One thing to note in the design above is the fact that we have nested RecyclerViews. We have this one big vertical RecyclerView and many nested inside horizontal RecyclerViews. So the LinearLayoutManager has this property called **initialPrefetchItemCount**. Look at the beautiful documentation it has:

> Sets the number of items to prefetch in collectInitialPrefetchPositions(int, RecyclerView.LayoutManager.LayoutPrefetchRegistry), which defines how many inner items should be prefetched when this LayoutManager's RecyclerView is nested inside another RecyclerView.
>
> Set this value to the number of items this inner LayoutManager will display when it is first scrolled into the viewport. RecyclerView will attempt to prefetch that number of items so they are ready, avoiding jank as the inner RecyclerView is scrolled into the viewport.
>
> For example, take a vertically scrolling RecyclerView with horizontally scrolling inner RecyclerViews. The rows always have 4 items visible in them (or 5 if not aligned). Passing 4 to this method for each inner RecyclerView's LinearLayoutManager will enable RecyclerView's prefetching feature to do create/bind work for 4 views within a row early, before it is scrolled on screen, instead of just the default 2.
>
> Calling this method does nothing unless the LayoutManager is in a RecyclerView nested in another RecyclerView.
>
> Unknown God of RecyclerView development

As you see, you can prefetch items when RecyclerViews are nested. So I went to each of our horizontal RecyclerViews and just updated the layout manager to prefetch 10 items. This actually gave us a really huge performance boost and helped us fix one of the horizontal scroll issues, where scrolling to the right was causing the focus to be pushed back to the left. But it wasn't the only thing that helped.

### RecyclerView is not the best tool to use for TV

Even with the above optimizations, we still had this focus losing and focus misbehavior in place. I was wondering whether RecyclerView is the right tool for the job. I decided to try on of the StackOverflow suggestion to replace RecyclerView with VerticalGridView or HorizontalGridView. So here we go again.

I visited all RecyclerViews that we use in the project. And in the layout-television folder I created a separate XML for them. In that XML file I replaced the RecyclerView with either HorizontalGridView or VerticalGridView based on whether the LinearLayoutManager that we used was VERTICAL or HORIZONTAL. And this had a huge impact on the behavior of our app.

- The focus was greatly improved.
- It was scrolling more smoothly.
- Keeping the focused row in the middle of the screen did not require any more manual work. VerticalGridView was doing this for us.
- Keeping the focus inside the RecyclerView until you reach the last view either horizontal or vertical again remained hidden for us. Previously we had to do this on our own.

Overall I think this is the best thing each Android TV project should aim to use. Keep in mind that VerticalGridView and HorizontalGridView both extend from RecyclerView so even with a shared codebase, you can just treat both of them as RecyclerViews. TV and mobile implementations remain hidden.

So this refactor eliminated around 400 lines of code we had and this big dependency list where we passed all kinds of Scroller implementations that would try to center elements vertically or horizontally and all other kinds of voodoo magic.

### Keeping positions of scrolled horizontal recycler views & the ListAdapter issue

Until we replaced the RecyclerView with the Horizontal/Vertical GridViews from the point above, we couldn't get this scrolling behavior to work correctly. I felt like I am getting closer to the point with each small step that I do.

The way we kept the positions of the scrolled items and the way we restored them seemed totally okay to me. Let's say we have a HorizontalRecyclerView that needs to be laid out. In onBind where it receives its list of elements it needs to display, we take the position that needs to be focused and just call scrollTo. Something like this:

```
override fun bind(imageUrls: List<String>) {
   // Other magic happening here
    tvFormatCoverAdapter.submitList(items)
    binding.rvTvShows.post {
       binding.rvTvShows.scrollBy(previousSavedHorizontalOffset, 0)
    }
}
```

I had a colleague who would decline PRs which use the .post method. Bud sadly for us, I just couldn't figure out how to make the recycler view scroll to the wanted position. This puts the code at the end of the UI thread queue and probably this is what makes the magic happen.

But one thing I could notice again is that once we call the scrollBy section of the code, the focus gets lost. It seems like the scrollBy function is executed before the actual items are on the screen and this is why the focus is lost.

One thing to notice is that on our side, all adapters extend from the [ListAdapter](https://developer.android.com/reference/androidx/recyclerview/widget/ListAdapter) class. It is just very easy to use especially with the DiffItemCallback. But even if I decided to put the scrollBy code inside the commit callback of the submitList method that ListAdapter has, the focus would be lost again. This gave me the feeling there is something async happening in ListAdapter that would execute the callback before the items are laid out on the screen.

I could see that ListAdapter uses LiveData and 2 separate threads to do the diff-ing of the items. And I suspect that the commitCallback is called when the items difference is calculated but not when they are present or inflated. And this is why we lose our focus.

After a small play around with the ListAdapter, I extracted it and for TV we just use the simple DiffUtil callback. I reused the DiffUtil.ItemCallback that we already had in place and created a simple strategy pattern.

<script src="https://gist.github.com/n0m0r3pa1n/d8b82b61a59df3422ea55a3d62bd5f36.js"></script>

## The end result

In the end with all of these small optimizations, we ended up having a better behaving app. The issues with the focus being lost seem to be significantly reduced - actually we don't experience them anymore (for now). And I am quite happy with the result.

https://youtu.be/CuekpjKSpCM

The only thing that terrifies me in Android at this moment is the case where you may need to introduce custom focus handling. It is inevitable in a way and you have to be prepared but I haven't reached that point yet. When it comes ... I guess there will be another article about it.
