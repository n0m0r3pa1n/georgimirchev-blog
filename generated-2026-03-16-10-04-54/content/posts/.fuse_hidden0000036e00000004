---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:32"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "54691977974"
author: gmirchev90
categories:
  - android
date: "2021-02-12T21:10:34+00:00"
guid: http://georgimirchev.com/?p=359
parent_post_id: null
post_id: "359"
summary: 'Wonder how to check the network connectivity in Android? There is a new API that you can use but there aren''t many tutorials how to do it. Here is our simple way:'
tags:
  - network
  - rxjava
timeline_notification: "1613164237"
title: Detecting network change with ConnectivityManager &amp; RxJava
url: /2021/02/12/detecting-network-change-with-connectivitymanager-rxjava/

---
Wonder how to check the network connectivity in Android? There is a new API that you can use but there aren't many tutorials how to do it. Here is our simple way:

```
class NetworkControl @Inject constructor(context: Context) {

    private val cm = context.getSystemService(Context.CONNECTIVITY_SERVICE) as ConnectivityManager
    private val tm = context.getSystemService(Context.TELEPHONY_SERVICE) as TelephonyManager

    private val mobileNetworkOperator: String
        get() = "${tm.networkOperatorName} (${tm.networkOperator})"

    private val networkMonitorSubject = BehaviorSubject.create<State>()

    val stateObservable: Observable<State> =
        networkMonitorSubject
            .hide()
            .startWith(currentState)
            .distinctUntilChanged()
            .debounce(2, TimeUnit.SECONDS)

    val currentState: State
        get() = if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            cm.getNetworkCapabilities(cm.activeNetwork)?.run {
                createNetworkState(hasTransport(NetworkCapabilities.TRANSPORT_CELLULAR))
            } ?: createNetworkState()
        } else {
            cm.activeNetworkInfo?.run {
                createNetworkState(type == ConnectivityManager.TYPE_MOBILE)
            } ?: createNetworkState()
        }

    fun refreshNetworkState() {
        networkMonitorSubject.onNext(currentState)
    }

    private val isOnline: Boolean
        get() {
            val activeNetworkInfo = cm.activeNetworkInfo
            return activeNetworkInfo != null && activeNetworkInfo.isConnected
        }

    private fun createNetworkState(isCellular: Boolean = false): State = State(!isOnline, isCellular, mobileNetworkOperator)

    data class State(
        val isOffline: Boolean,
        val isMetered: Boolean,
        val mobileNetworkOperator: String
    ) {
        val isOnline: Boolean
            get() = !isOffline
    }
}
```
