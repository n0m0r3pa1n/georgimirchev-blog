---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:31"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "50972768766"
author: gmirchev90
categories:
  - android
date: "2020-11-12T10:21:30+00:00"
guid: http://georgimirchev.com/?p=295
parent_post_id: null
post_id: "295"
summary: |-
  ## Testing data classes in Kotlin

  As we all know, data classes are final by default on Kotlin and if you try to use Mockito and mock such a class, you will get an exception. What you need to do is add this file:
tags:
  - android-studio
  - kotlin
  - rxjava
  - unit-tests
timeline_notification: "1605176491"
title: Tricky parts when implementing unit tests for Android in Kotlin
url: /2020/11/12/tricky-parts-when-implementing-unit-tests-for-android-in-kotlin/

---
## Testing data classes in Kotlin

As we all know, data classes are final by default on Kotlin and if you try to use Mockito and mock such a class, you will get an exception. What you need to do is add this file:

{{< figure src="/wp-content/uploads/2020/11/screenshot-2020-11-12-at-11.34.16.png?w=612" alt="" caption="" >}}

And inside this file you need the following content

**mock-maker-inline**

Only that row above. This will allow mocking final classes.

## Testing RxJava Completable

Sometimes using the verify() method for a Completable in mockito doesn't work as you would expect it to. You need to assert that the completable was subscribed to and executed. To do that you can use this class:

```
import io.reactivex.Completable
import io.reactivex.CompletableObserver
import org.junit.Assert.assertFalse
import org.junit.Assert.assertTrue

class TestCompletable(private val autocomplete: Boolean = true) : Completable() {

    private var subscribed = false

    private val wrapped = create {
        subscribed = true
        if (autocomplete) {
            it.onComplete()
        }
    }

    override fun subscribeActual(s: CompletableObserver) {
        wrapped.subscribe(s)
    }

    fun assertSubscribed() {
        assertTrue(subscribed)
    }

    fun assertNotSubscribed() {
        assertFalse(subscribed)
    }
}
```

So now, basically whenever you have a method that returns this TestCompletable, you can verify it was subscribed to like this:  

```
private val revokeCompletable = TestCompletable()
private val myApi: MyApi = mock {
    on(it.revokeToken(any(), any(), any())).thenReturn(revokeCompletable)
}

@Test
fun logOutUser_revokesTokens() {
    ***
    val testObserver = underTest.logOutUser().test()
    ***
    revokeCompletable.assertSubscribed()
}
```

## Tests for LiveData and final classes

More information can be found [HERE](https://georgimirchev.com/2020/11/12/running-unit-tests-for-livedata-with-jupiter-on-android/#more-290).

## Timezone Tests with ThreeTen

```
org.threeten.bp.zone.ZoneRulesException: No time-zone data files registered
```

To fix this exception you need to have the TZDB.dat file inside your resources folder for the unit tests. Then you need to call the initThreeTen method which does this:

```
import org.threeten.bp.zone.TzdbZoneRulesProvider
import org.threeten.bp.zone.ZoneRulesProvider

fun Any.initThreeTen() {
    if (ZoneRulesProvider.getAvailableZoneIds().isEmpty()) {
        val stream = this.javaClass.classLoader!!.getResourceAsStream("TZDB.dat")
        stream.use(::TzdbZoneRulesProvider).apply {
            ZoneRulesProvider.registerProvider(this)
        }
    }
}
```

1. To get the TZDB.dat file you can use [this utility](https://github.com/akashche/tzdbgen) to convert the timezone files into a single binary file that can be used by the Open JDK.
1. The TZDB.dat file needs to be added here:

{{< figure src="/wp-content/uploads/2020/11/screenshot-2020-11-12-at-12.08.56.png?w=612" alt="" caption="" >}}

What you need to do before each test is init the three ten library

```
@BeforeEach
fun initThreeTenLib() {
    initThreeTen()
}
```

## Mocking resources

Mocking resources every time can become nasty and annoying. So here are some helper functions that you can use when you want to mock the Resources class

```
/**
 * When a string without parameters is requested, return it in this form "{stringId}"
 *
 * When a string with one param is requested, return it in this form "{stringId}:{parameter}"
 *
 * When a plural with one param is requested, return it in this form "{stringId}:{quantity}:{parameter}"
 */
fun simulateStringFetching(resources: Resources) {
    fun join(invocation: InvocationOnMock) = invocation.arguments.joinToString(":")
    whenever(resources.getText(anyInt()))
        .then { join(it) }
    whenever(resources.getString(anyInt()))
        .then { join(it) }
    whenever(resources.getString(anyInt(), any()))
        .then { join(it) }
    whenever(resources.getString(anyInt(), any(), any()))
        .then { join(it) }
    whenever(resources.getString(anyInt(), any(), any(), any()))
        .then { join(it) }
    whenever(resources.getString(anyInt(), any(), any(), any(), any()))
        .then { join(it) }
    whenever(resources.getQuantityString(anyInt(), anyInt()))
        .then { it.arguments.joinToString(":") }
    whenever(resources.getQuantityString(anyInt(), anyInt(), anyVararg()))
        .then { it.arguments.joinToString(":") }
}

/**
 * When a string without parameters is requested, return it in this form "{stringId}"
 *
 * When a string with one param is requested, return it in this form "{stringId}:{parameter}"
 */
fun simulateStringFetching(context: Context) {
    fun join(invocation: InvocationOnMock) = invocation.arguments.joinToString(":")
    whenever(context.getText(anyInt()))
        .then { join(it) }
    whenever(context.getString(anyInt()))
        .then { join(it) }
    whenever(context.getString(anyInt(), any()))
        .then { join(it) }
    whenever(context.getString(anyInt(), any(), any()))
        .then { join(it) }
    whenever(context.getString(anyInt(), any(), any(), any()))
        .then { join(it) }
    whenever(context.getString(anyInt(), any(), any(), any(), any()))
        .then { join(it) }
}

fun mockContextWithStringsAndIntegersFetching(): Context {
    val context = mock(Context::class.java)
    simulateStringFetching(context)
    whenever(context.getColor(anyInt())).thenAnswer { it.arguments[0] }

    val resources = mockResourcesWithStringsAndIntegersFetching()
    whenever(context.resources).thenReturn(resources)
    return context
}

fun mockResourcesWithStringsAndIntegersFetching(): Resources {
    val resources = mock(Resources::class.java)
    simulateStringFetching(resources)

    whenever(resources.getColor(anyInt(), anyOrNull()))
        .then { it.arguments[0] }

    whenever(resources.getInteger(anyInt()))
        .then { it.arguments[0] }

    return resources
}
```

and then you can use them like this:

```
assertThat(underTest.moneyStringObservable.get())
  .isEqualTo("${R.string.money_fixed_value}:$ 100")
```
