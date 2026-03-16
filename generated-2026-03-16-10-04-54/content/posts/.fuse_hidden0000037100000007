---
_elasticsearch_data_sharing_indexed_on: "2025-06-14 06:25:22"
_g_feedback_shortcode_0cf3966c1699c7207f50cc5fa358e48ae444f109: |-
  [contact-field label="Name" type="name"  required="true" /]
  				[contact-field label="Email" type="email" required="true" /]
  				[contact-field label="Website" type="url" /]
  				[contact-field label="Message" type="textarea" /]
_g_feedback_shortcode_atts_0cf3966c1699c7207f50cc5fa358e48ae444f109:
  block_template: null
  block_template_part: null
  className: null
  customThankyou: ""
  customThankyouHeading: Your message has been sent
  customThankyouMessage: Thank you for your submission!
  customThankyouRedirect: ""
  hiddenFields: null
  id: 2514
  jetpackCRM: true
  postToUrl: null
  salesforceData: null
  show_subject: "no"
  subject: '[Make Things Happen] '
  submit_button_text: Submit
  to: gmirchev90@gmail.com
  widget: 0
_last_editor_used_jetpack: block-editor
_publicize_job_id: "104889282661"
_publicize_shares: []
author: gmirchev90
categories:
  - android
date: "2025-06-14T06:25:48+00:00"
firehose_sent: "1749882349"
guid: https://georgimirchev.com/?p=2514
parent_post_id: null
post_id: "2514"
previously_liked_100282508: "1"
summary: Jetpack Compose Navigation goes hand in hand with Jetpack Compose. The issue comes when you are coming from XML navigation. Passing arguments between screens is very easy in XML and by extending from Java.Serializable interface, everything was working like magic. But now, you open the Pandora's box with Jetpack Compose and here is how you could fix it.
tags:
  - coding
  - jetpack-compose-navigation
  - kotlin
  - kotlinx-serialization
  - navigation-arguments
  - programming
  - technology
timeline_notification: "1749882349"
title: Making arguments work in Compose Navigation where there was XML
url: /2025/06/14/making-arguments-work-in-compose-navigation-where-there-was-xml/

---
Jetpack Compose Navigation goes hand in hand with Jetpack Compose. The issue comes when you are coming from XML navigation. Passing arguments between screens is very easy in XML and by extending from Java.Serializable interface, everything was working like magic. But now, you open the Pandora's box with Jetpack Compose and here is how you could fix it.

# Handling arguments between destinations

Let's say you had a serialisable object that you are passing between screens. You want to keep the same way of using navigation as you don't want to refactor it to use some kind of an in-memory repo or any other storage outside of your navigation routes.

Here is how it was:

```
data class Transaction(val id: Id, val paymentMethod: PaymentMethod) : Serializable

data class Id(val value=Int) : Serializable

sealed interface PaymentMethod {
    enum class Manual {
        CASH,
        CHECK
    } : PaymentMethod
    data object Online: PaymentMethod
}
```

To keep it working as before, you will need to:

1. Add Kotlinx Serialisation library
1. Make sure all your data classes that are transferred are marked with @Serlisable
1. If you have enums that extend sealed interface have a "custom" serialiser
1. Serialising them as arguments between destinations
1. Use the values in the ViewModel directly

## Adding kotlinx serialisation

Based on the [official documentation](https://kotlinlang.org/docs/serialization.html#formats), make sure you add both:

- Plugin - _kotlin("plugin.serialization") version "2.1.21"_
- Implementation in Gradle - _implementation("org.jetbrains.kotlinx:kotlinx-serialization-json:1.8.1")_

## Adding the Serializable annotation

If your data classes already extend from Java Serializable, make sure to give it an alias name and use that one.

```
import java.Serializable as JavaSerializable

data class Transaction() : JavaSerializable
```

Then adding the kotlinx serialisation annotation is quite straightforward.

```
import kotlinx.serialization.Serializable
import java.Serializable as JavaSerializable

@Serializable
data class Transaction(val id: Id, val paymentMethod: PaymentMethod) : JavaSerializable

@Serializable
data class Id(val value=Int) : JavaSerializable

@Serializable
sealed interface PaymentMethod {

    @Serializable
    enum class Manual {
        CASH,
        CHECK
    } : PaymentMethod

    @Serializable
    data object Online: PaymentMethod
}
```

## Custom serialiser for enums that extend sealed interfaces

By default, Kotlinx won't be able to handle the serialisation of enums that extend from sealed interfaces. The reason is that when deserialising it (to JSON for example), it needs to put a type value so it knows how to serialise it later. And enums don't have this type identifier val.

So we have to implement it ourselves. Fortunately, after a lot of research, there was some code shared on StackOverflow how it would work if you serialise it to a map.

```
import kotlinx.serialization.ExperimentalSerializationApi
import kotlinx.serialization.KSerializer
import kotlinx.serialization.descriptors.SerialDescriptor
import kotlinx.serialization.encoding.Decoder
import kotlinx.serialization.encoding.Encoder
import kotlinx.serialization.serializer

abstract class EnumAsMapSerializer<T>(sName: String) : KSerializer<T> {
    private val mapSerializer: KSerializer<Map<String, String>> = serializer()

    override val descriptor = SerialDescriptor(sName, mapSerializer.descriptor)

    abstract fun getTypeOf(value: T): String

    abstract fun enumFrom(type: String, value: String): T

    override fun serialize(encoder: Encoder, value: T) {
        encoder.encodeSerializableValue(
            mapSerializer,
            mapOf(
                "type" to getTypeOf(value),
                "value" to (value as Enum<*>).name,
            ),
        )
    }

    override fun deserialize(decoder: Decoder): T {
        val map = decoder.decodeSerializableValue(mapSerializer)
        return enumFrom(map["type"]!!, map["value"]!!)
    }

    @OptIn(ExperimentalSerializationApi::class)
    inline fun <reified T> getTypeSerialName() = serializer<T>().descriptor.serialName
}
```

So the usage of the above for our PaymentMethod would be like:

```
@Serializable(with = PaymentMethodSerialization::class)
sealed interface PaymentMethod { ... }class PaymentMethodSerialization : EnumAsMapSerializer<PaymentMethod>(PaymentMethod::class.java.simpleName) {
    override fun getTypeOf(value: PaymentMethod): String = when (value) {
        is Manual -> getTypeSerialName<Manual>()
        is Online -> Online.javaClass.simpleName
    }override fun enumFrom(type: String, value: String): PaymentMethod = when (type) {
        getTypeSerialName<Manual>() -> Manual.valueOf(value)
        Online.javaClass.simpleName -> Online
        else -> throw IllegalArgumentException("Undefined serialization for type: $type")
    }
}
```

By using this custom enum serialiser, we are now able to successfully serialise enums to map and back. This will allow us to pass those values as arguments in the navigation library and later use them in the ViewModel directly.

## Serialising the data as arguments between destinations

To be able to pass your data between your destinations, you need to serialise it. But there is a catch. Did you expect it will work automatically for you as you have already declared the Serializable annotation above your classes? Well, don't want to disappoint you but it won't.

You need to additionally tell the navigation library that "Hey, I have a type of TxId and PaymentMethod ,and because you are dumb as hell, I will tell you which serializer to use for those types as you cannot figure it out yourself".

So basically,y there is this typeMap that you need to pass to the composable route where you declare it.

```
composable<TransactionRoute>(TransactionRoute.typeMap) {
    TransactionScreen()
}

data class TransactionRoute(val transaction: Transaction) {
    companion object {
        val typeMap: Map<KType, @JvmSuppressWildcards NavType<*>> = mapOf(
            navigationCustomArgument<Id>(),
            navigationCustomArgument<PaymentMethod>(),
        )
    }
}

inline fun <reified T : Any> navigationCustomArgument(isNullable: Boolean = false): Pair<KType, NavType<T>> {
    val serializer: KSerializer<T> = serializer()
    return typeOf<T>() to JsonSerializableNavType(serializer, isNullable)
}

@Suppress("NOTHING_TO_INLINE")
data class JsonSerializableNavType<T : Any>(
    private val serializer: KSerializer<T>,
    override val isNullableAllowed: Boolean = false,
) : NavType<T>(isNullableAllowed = isNullableAllowed) {
    override val name: String = serializer.descriptor.serialName

    override fun put(bundle: Bundle, key: String, value: T) {
        bundle.putString(key, value.encodedAsString())
    }

    override fun get(bundle: Bundle, key: String): T? {
        return bundle.getString(key)?.let { parseValue(it) }
    }

    override fun serializeAsValue(value: T): String {
        return Uri.encode(value.encodedAsString())
    }

    override fun parseValue(value: String): T {
        return value.decodedFromString()
    }

    private inline fun T.encodedAsString(): String = Json.encodeToString(serializer, this)

    private inline fun String.decodedFromString(): T = Json.decodeFromString(serializer, this)
}
```

And because we already have the kotlinx serializers generated by the library, we need to pass it to the Compose Navigation library and create a connection between them. So how this works is by telling Compose Navigation library to always serialize them as Json by using the KSerializer that we provide and hook it to the existing NavType<> structure that it provides.

The above example is how you make things a bit more generic and reusable. Otherwise, duplication will kill the purpose of the whole thing.

## Using the argument in ViewModel by injecting SavedStateHandle

SavedStateHandle has a toRoute method that actually takes whatever is in there and makes an object out of it. So to have a TransactionRoute object, you need to call:

```
savedStateHandle.toRoute<TransactionRoute>(TransactionRoute.typeMap)
```

This way you get an object from your arguments.

But soon you will realise that unit testing this is shit. SavedStateHandle internally relies on Bundle class that cannot be mocked. To solve that issue, you will need a wrapper class:

```
class RouteArgsProvider @Inject constructor(
    private val savedStateHandle: SavedStateHandle,
) {
    fun <T : Any> getRouteArgs(
        route: kotlin.reflect.KClass<T>,
        typeMap: Map<kotlin.reflect.KType, @JvmSuppressWildcards androidx.navigation.NavType<*>> = emptyMap(),
    ) = savedStateHandle.toRoute(route, typeMap)
}
```

Now you can safely mock it and make it return whatever you need and test it.

# Summary

In short, Jetpack Compose Navigation works. By using this approach, you integrated it with KotlinX Serialization and pass the object as JSON string around. Whether it is good to pass big objects - NO. Whether people do it - YES. Whether you should do it - up to you.

There is a lot of boilerplate code that needs to be handled and makes it quite annoying. The fact there is no easy integration between the two libraries makes it even nastier.

Make sure to you navigation library **2.9.0** or above and .. Good luck!
