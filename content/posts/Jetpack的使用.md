---
title: Jetpack介绍
date: 2025-03-26
taxonomies:
  categories: ["Android"]
  tags: ["Android", "Jetpack"]
---
# 什么是Jetpack？

看一下官方的定义：

> Jetpack 是一个由多个库组成的套件，可帮助开发者遵循最佳做法、减少样板代码并编写可在各种 Android 版本和设备中一致运行的代码，让开发者可将精力集中于真正重要的编码工作。

简单来说，Jetpack就是Google官方推出的一套方便开发者的库。

那么，为什么要使用Jetpack呢？

原因就是因为Android在发展过程中出现了许多框架来帮助开发者快速进行开发，但随着这些框架越来越多，导致了开发越来越碎片化，这时候，大哥Google看不下去了，所以其在Goole I/O 2018大会上推出了Android Jetpack。Jetpack可帮助开发者遵循最佳做法，减少样板代码并编写可在各种 Android 版本和设备中一致运行的代码，让开发者精力集中编写重要的代码。

 Jetpack主要包括4个方面，如下图所示，分别是架构（Architecture）、界面（UI）、行为 （Behavior）和基础（Foundation）。

![img](https://redrock.feishu.cn/space/api/box/stream/download/asynccode/?code=YzFmMDQxYzkxYmQ0MzdiNTU0ZTUwNGQ3YzRmMDQ1ZTNfWWRaZURxRWdsS3ZlWmlLdFlJOWU1U3FwUURNNUxoN05fVG9rZW46VEtLaGJHd01KbzVJZVJ4aUxXOGNPRDhSblBsXzE3NDcyMDE5OTE6MTc0NzIwNTU5MV9WNA)

# Lifecycle

Lifecycle是很多Jetpack组件的基础，我们先来讲讲它。

## 什么是Lifecycle？

[Lifecycle](https://developer.android.com/topic/libraries/architecture/lifecycle?hl=zh-cn)是一个类，用于存储有关组件（如 activity 或 fragment）的生命周期状态的信息，并允许其他对象观测此状态。

## 为什么要用Lifecycle

我们在处理Activity和Fragment组件的生命周期相关时，我们一般会在onCreate中初始化某些成员，然后再其他生命周期中对这些成员进行特殊处理，最后在onDestroy中释放他们。我们在一些需求小的时候，这样写是没有问题的，但当你需求过多了，你把这些全都写到里面，会造成你的代码会非常复杂臃肿。为了解决上述问题，就推出了Lifecycle来帮助我们用一种统一的方式来监听 Activity、Fragment 的生命周期变化，并且会大大减少了业务代码发生内存泄漏的风险。

## Lifecycle的用法

> 注：@OnLifecycleEvent(Lifecycle.Event.ON_XXX)标签已经被弃用

### `DefaultLifecycleObserver`接口

```kotlin
class MyObserver: DefaultLifecycleObserver {
    override fun onCreate(owner: LifecycleOwner) {
        super.onCreate(owner)
        Log.d("MyObserver", "onCreate:")
    }

    override fun onStart(owner: LifecycleOwner) {
        super.onStart(owner)
        Log.d("MyObserver", "onStart:")
    }

    override fun onResume(owner: LifecycleOwner) {
        super.onResume(owner)
        Log.d("MyObserver", "onResume:")
    }

    override fun onPause(owner: LifecycleOwner) {
        super.onPause(owner)
        Log.d("MyObserver", "onPause:")
    }

    override fun onStop(owner: LifecycleOwner) {
        super.onStop(owner)
        Log.d("MyObserver", "onStop:")
    }

    override fun onDestroy(owner: LifecycleOwner) {
        super.onDestroy(owner)
        Log.d("MyObserver", "onDestroy:")
    }
}
```

在Activity中添加`lifecycle.addObserver(MyObserver())`

```kotlin
class LifecycleActivity: AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_lifecycle)
        lifecycle.addObserver(MyObserver())
    }
}
```

查看Log

![img](https://redrock.feishu.cn/space/api/box/stream/download/asynccode/?code=YzRhNTFlYjlmZTY2ZjNhMDE2ZjBjYTViN2I2M2Y5ZWRfU0o3NE05ZGV1cVhYb1RqQmxtcGhUTmEyWDlVWXpJdGpfVG9rZW46R05HWmJ4RnNCb1RGa2N4NXdvS2NDQ1ozbnNiXzE3NDcyMDE5OTE6MTc0NzIwNTU5MV9WNA)

### `LifecycleEventObserver`接口

```kotlin
class MyObserver2: LifecycleEventObserver {
    override fun onStateChanged(source: LifecycleOwner, event: Lifecycle.Event) {
        when (event) {
            Lifecycle.Event.ON_CREATE -> {
                Log.d("MyObserver2", "onCreate")
            }
            Lifecycle.Event.ON_START -> {
                Log.d("MyObserver2", "onStart")
            }
            Lifecycle.Event.ON_RESUME -> {
                Log.d("MyObserver2", "onResume")
            }
            Lifecycle.Event.ON_PAUSE -> {
                Log.d("MyObserver2", "onPause")
            }
            Lifecycle.Event.ON_STOP -> {
                Log.d("MyObserver2", "onStop")
            }
            Lifecycle.Event.ON_DESTROY -> {
                Log.d("MyObserver2", "onDestroy")
            }
            else -> {}
        }
    }
}
```

同样在Activity中添加`lifecycle.addObserver(MyObserver2())`

```kotlin
class LifecycleActivity: AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_lifecycle)
        lifecycle.addObserver(MyObserver2())
    }
}
```

![img](https://redrock.feishu.cn/space/api/box/stream/download/asynccode/?code=MWM4OWY5OTVmMzFlYWVmYWI1MjMzMmE0MjEyYjQ0ZTFfZU1nQlFBc1Q5TW1BVjNVbHlMck1yQ1VNZzZvZnpxZFVfVG9rZW46RURlZmJSN3Nvb1RjU294M1VvemNoSEpnblFmXzE3NDcyMDE5OTE6MTc0NzIwNTU5MV9WNA)

## Lifecycle的原理

大家是否有这样的疑问，为什么Lifecycle可以监听生命周期？所以带着这样的疑问，我们就来讲讲其原理。

Lifecycle整体上采用了观察者模式，提供了LifecycleOwner(被观察者)和LifecyleObserver(观察者)两个接口，通过这个其就可以对页面的生命周期进行监听。

回到我们的Activity中来，我们注册观察者主要是通过`lifecycle`调用`addObserver()`实现的，所以我们从这里下手看看这个`lifecycle`是咋来的

```kotlin
lifecycle.addObserver(MyObserver())
```

咱们按住Ctrl键，用鼠标点击`lifecycle`

```kotlin
override val lifecycle: Lifecycle
    get() = super.lifecycle
```

可以看到这个lifecycle拿的是其父类中的lifecycle，咱们再点进去

```kotlin
@RestrictTo(RestrictTo.Scope.LIBRARY_GROUP_PREFIX)
public open class ComponentActivity : Activity(), LifecycleOwner, KeyEventDispatcher.Component {
    ...
    @Suppress("LeakingThis") private val lifecycleRegistry = LifecycleRegistry(this)
    ...
    override val lifecycle: Lifecycle
        get() = lifecycleRegistry
    }
}
```

我们可以看到lifecycle拿到的是一个`LifecycleRegistry`对象，并且将当前的Activity传进去了

我们再来看看`LifecycleRegistry`对象

```Kotlin
open class LifecycleRegistry private constructor(
    provider: LifecycleOwner,
    private val enforceMainThread: Boolean
) : Lifecycle() {
    ...
}
```

可以看到LifecycleRegistry继承于Lifecycle这个类，并且在其构造方法中传进来一个`LifecycleOwner`，我们看看它

```kotlin
public interface LifecycleOwner {
    /**
     * Returns the Lifecycle of the provider.
     *
     * @return The lifecycle of the provider.
     */
    public val lifecycle: Lifecycle
}
```

可以看到它是一个接口，里面只有一个变量`lifecycle`，上面说了`LifecycleRegistry`将当前的Activity实例传了进来，所以Activity肯定是实现了`LifecycleOwner`这个接口的，所以，我们就知道`lifecycle`是怎么来的了。

聊完lifecycle怎么来的之后，我们就可以详细聊一聊它究竟怎么感知生命周期的了。

前面提到了`LifecycleRegistry`继承于`Lifecycle`这个类，我们先来看看它

```kotlin
public abstract class Lifecycle {
    // 添加观察者
    @MainThread
    public abstract fun addObserver(observer: LifecycleObserver)
    // 移除观察者
    @MainThread
    public abstract fun removeObserver(observer: LifecycleObserver)
    // 当前状态
    @get:MainThread
    public abstract val currentState: State
    // 生命周期事件，对应Activity生命周期方法
    public enum class Event {
        ON_CREATE,
        ON_START,
        ON_RESUME,
        ON_PAUSE,
        ON_STOP,
        ON_DESTROY,
        ON_ANY;
        // 发生事件后的状态
        public val targetState: State
            get() {
                when (this) {
                    ON_CREATE, ON_STOP -> return State.CREATED
                    ON_START, ON_PAUSE -> return State.STARTED
                    ON_RESUME -> return State.RESUMED
                    ON_DESTROY -> return State.DESTROYED
                    ON_ANY -> {}
                }
                throw IllegalArgumentException("$this has no target state")
            } 
    }
    //生命周期状态
    public enum class State {
        DESTROYED,
        INITIALIZED,
        CREATED,
        STARTED,
        RESUMED;
        //判断至少是某一状态
        public fun isAtLeast(state: State): Boolean {
            return compareTo(state) >= 0
        }
    }
}
```

可以看到Lifecycle是一个抽象类，里面定义了一些抽象方法以及两种枚举：**事件**和**状态**，Lifecycle正是用这两种枚举跟踪其关联组件的生命周期状态，当我们的事件发生后，我们会把Lifecycle的状态也改变。

ok，聊完Lifecycle这个类之后，我们上面了解到Activity是实现了`LifecycleOwner`这个接口的，那么处理生命周期的一些方法肯定在Activity中存在的，我们来看看*androidx.activity.ComponentActivity*

```Kotlin
open class ComponentActivity() :
    androidx.core.app.ComponentActivity(),
    ContextAware,
    LifecycleOwner {
    ...
    override fun onCreate(savedInstanceState: Bundle?) {
        savedStateRegistryController.performRestore(savedInstanceState)
        contextAwareHelper.dispatchOnContextAvailable(this)
        super.onCreate(savedInstanceState)
        ReportFragment.injectIfNeededIn(this)// 关键
        if (contentLayoutId != 0) {
            setContentView(contentLayoutId)
        }
    }

    @CallSuper
    override fun onSaveInstanceState(outState: Bundle) {
        if (lifecycle is LifecycleRegistry) {
            (lifecycle as LifecycleRegistry).currentState = Lifecycle.State.CREATED
        }
        super.onSaveInstanceState(outState)
        savedStateRegistryController.performSave(outState)
    }

    override val lifecycle: Lifecycle
        get() = super.lifecycle
}
```

我们可以看到，好像只是在`onSaveInstanceState`中设置了Lifecycle的State为CREATED，这是咋回事？其他的生命周期呢？其实是在onCreate中的`ReportFragment.injectIfNeededIn(this)`才是关键。

我们来点进去，还是有很多东西的

我们来慢慢看，首先我们先从`injectIfNeededIn`这个方法看起

```kotlin
        @JvmStatic
        fun injectIfNeededIn(activity: Activity) {
            // 当API 29 以上直接注册回调
            if (Build.VERSION.SDK_INT >= 29) {
                LifecycleCallbacks.registerIn(activity)
            }
            val manager = activity.fragmentManager
            if (manager.findFragmentByTag(REPORT_FRAGMENT_TAG) == null) {
                manager.beginTransaction().add(ReportFragment(), REPORT_FRAGMENT_TAG).commit()
                manager.executePendingTransactions()
            }
        }
```

可以看到这个方法是分了版本的，API 29之前是添加了一个`ReportFragment()`给当前Activity，之后使用了LifecycleCallbacks。咱们先来看看老版本的

```kotlin
@Suppress("DEPRECATION")
@RestrictTo(RestrictTo.Scope.LIBRARY_GROUP_PREFIX)
open class ReportFragment() : android.app.Fragment() {

    override fun onActivityCreated(savedInstanceState: Bundle?) {
        super.onActivityCreated(savedInstanceState)
        dispatchCreate(processListener)
        dispatch(Lifecycle.Event.ON_CREATE)
    }

    override fun onStart() {
        super.onStart()
        dispatchStart(processListener)
        dispatch(Lifecycle.Event.ON_START)
    }

    override fun onResume() {
        super.onResume()
        dispatchResume(processListener)
        dispatch(Lifecycle.Event.ON_RESUME)
    }

    override fun onPause() {
        super.onPause()
        dispatch(Lifecycle.Event.ON_PAUSE)
    }

    override fun onStop() {
        super.onStop()
        dispatch(Lifecycle.Event.ON_STOP)
    }

    override fun onDestroy() {
        super.onDestroy()
        dispatch(Lifecycle.Event.ON_DESTROY)
        // just want to be sure that we won't leak reference to an activity
        processListener = null
    }

    private fun dispatch(event: Lifecycle.Event) {
        if (Build.VERSION.SDK_INT < 29) {
            // Only dispatch events from ReportFragment on API levels prior
            // to API 29. On API 29+, this is handled by the ActivityLifecycleCallbacks
            // added in ReportFragment.injectIfNeededIn
            dispatch(activity, event)
        }
    }
 }
```

可以看到，都在当前生命周期中用到了`dispatch`方法，当生命周期被触发后，会传一个对应的事件给它，并且这个方法里面又用到了一个`dispatch`方法，我们后面再来看这个方法。

我们先来看看API 29以上是怎么处理的？

```kotlin
@RequiresApi(29)
internal class LifecycleCallbacks : Application.ActivityLifecycleCallbacks {
    override fun onActivityCreated(
        activity: Activity,
        bundle: Bundle?
    ) {}

    override fun onActivityPostCreated(
        activity: Activity,
        savedInstanceState: Bundle?
    ) {
        dispatch(activity, Lifecycle.Event.ON_CREATE)
    }

    override fun onActivityStarted(activity: Activity) {}

    override fun onActivityPostStarted(activity: Activity) {
        dispatch(activity, Lifecycle.Event.ON_START)
    }

    override fun onActivityResumed(activity: Activity) {}

    override fun onActivityPostResumed(activity: Activity) {
        dispatch(activity, Lifecycle.Event.ON_RESUME)
    }

    override fun onActivityPrePaused(activity: Activity) {
        dispatch(activity, Lifecycle.Event.ON_PAUSE)
    }

    override fun onActivityPaused(activity: Activity) {}

    override fun onActivityPreStopped(activity: Activity) {
        dispatch(activity, Lifecycle.Event.ON_STOP)
    }

    override fun onActivityStopped(activity: Activity) {}

    override fun onActivitySaveInstanceState(
        activity: Activity,
        bundle: Bundle
    ) {}

    override fun onActivityPreDestroyed(activity: Activity) {
        dispatch(activity, Lifecycle.Event.ON_DESTROY)
    }

    override fun onActivityDestroyed(activity: Activity) {}

    companion object {
        @JvmStatic
        fun registerIn(activity: Activity) {
            activity.registerActivityLifecycleCallbacks(LifecycleCallbacks())
        }
    }
}
```

我们可以看到，API 29以上同样也是用到了`dispacth`方法，只不过它是直接使用activity的registerActivityLifecycleCallbacks 直接注册了生命周期回调，然后给当前Activity添加ReportFragment。

所以，让我们来看看dispatch这个方法

```kotlin
@JvmStatic
internal fun dispatch(activity: Activity, event: Lifecycle.Event) {
    if (activity is LifecycleRegistryOwner) {
        activity.lifecycle.handleLifecycleEvent(event)
        return
    }
    if (activity is LifecycleOwner) {
        val lifecycle = (activity as LifecycleOwner).lifecycle
        if (lifecycle is LifecycleRegistry) {
            lifecycle.handleLifecycleEvent(event)
        }
    }
}
```

可以看到，在内部都是使用到了`lifecycle.handleLifecycleEvent(event)`，我们来看看它。

```kotlin
open fun handleLifecycleEvent(event: Event) {
    enforceMainThreadIfNeeded("handleLifecycleEvent")
    // 开始对当前事务发生后的State做处理
    moveToState(event.targetState)
}

private fun moveToState(next: State) {
    // 如果当前State与即将要发生变化的State一样，直接返回
    if (state == next) {
        return
    }
    check(!(state == State.INITIALIZED && next == State.DESTROYED)) {
        "no event down from $state in component ${lifecycleOwner.get()}"
    }
    // 赋值新State
    state = next
    if (handlingEvent || addingObserverCounter != 0) {
        newEventOccurred = true
        // we will figure out what to do on upper level.
        return
    }
    handlingEvent = true
    // 把新的State同步给所有的观察者
    sync()
    handlingEvent = false
    if (state == State.DESTROYED) {
        observerMap = FastSafeIterableMap()
    }
}
```

这里应该比较好理解，就是那边传了一个事务过来，我们去改变当前State，相同返回，不一样就要同步给所有的观察者，所以我们来看看sync这个方法

```kotlin
private val isSynced: Boolean
    get() {
        // observerMap就是 在activity中添加observer后 用于存放observer的map
        if (observerMap.size() == 0) {
            return true
        }
        val eldestObserverState = observerMap.eldest()!!.value.state
        val newestObserverState = observerMap.newest()!!.value.state
        return eldestObserverState == newestObserverState && state == newestObserverState
    }

private fun sync() {
    val lifecycleOwner = lifecycleOwner.get()
        ?: throw IllegalStateException(
            "LifecycleOwner of this LifecycleRegistry is already " +
                "garbage collected. It is too late to change lifecycle state."
        )
    // 一个循环，isSynced表示是否同步给了所有观察者
    while (!isSynced) {
        newEventOccurred = false
        if (state < observerMap.eldest()!!.value.state) {
            backwardPass(lifecycleOwner)
        }
        val newest = observerMap.newest()
        if (!newEventOccurred && newest != null && state > newest.value.state) {
            forwardPass(lifecycleOwner)
        }
    }
    newEventOccurred = false
}
```

可以看到，sync就是用来同步所有的observerMap中的观察者，那观察者是怎么被加进来的呢？答案自然是我们在Activity中使用的`addObserver()`

```kotlin
override fun addObserver(observer: LifecycleObserver) {
    enforceMainThreadIfNeeded("addObserver")
    val initialState = if (state == State.DESTROYED) State.DESTROYED else State.INITIALIZED
    // 这边用了一个ObserverWithState来管理observer的状态
    val statefulObserver = ObserverWithState(observer, initialState)
    // 添加观察者，如果不存在，返回null，存在，返回已经存在的值
    val previous = observerMap.putIfAbsent(observer, statefulObserver)
    // 如果存在，直接返回，避免重复添加
    if (previous != null) {
        return
    }
    val lifecycleOwner = lifecycleOwner.get()
        ?: // it is null we should be destroyed. Fallback quickly
        return
    // 下面代码的逻辑：通过while循环，把新的观察者的状态 连续地 同步到最新状态
    val isReentrance = addingObserverCounter != 0 || handlingEvent
    var targetState = calculateTargetState(observer)
    addingObserverCounter++
    while (statefulObserver.state < targetState && observerMap.contains(observer)) {
        pushParentState(statefulObserver.state)
        val event = Event.upFrom(statefulObserver.state)
            ?: throw IllegalStateException("no event up from ${statefulObserver.state}")
        statefulObserver.dispatchEvent(lifecycleOwner, event)
        popParentState()
        // mState / subling may have been changed recalculate
        targetState = calculateTargetState(observer)
    }
    // 这里如果不是嵌套的添加，我们就又调用sync这个方法
    if (!isReentrance) {
        // we do sync only on the top level.
        sync()
    }
    addingObserverCounter--
}
```

我们是在onCreate里面添加观察者的，那你有没有想过如果我在onStart，onResume中去添加观察者可不可以呢？答案是可以的，假如你在onResume中添加，那他会不会回调onCreate，onStart里面的方法呢，是会的，关键就在于`statefulObserver.dispatchEvent(lifecycleOwner, event)`，我们点进去看看

```kotlin
internal class ObserverWithState(observer: LifecycleObserver?, initialState: State) {
    var state: State
    var lifecycleObserver: LifecycleEventObserver

    init {
        lifecycleObserver = Lifecycling.lifecycleEventObserver(observer!!)
        state = initialState
    }

    fun dispatchEvent(owner: LifecycleOwner?, event: Event) {
        val newState = event.targetState
        state = min(state, newState)
        lifecycleObserver.onStateChanged(owner!!, event)
        state = newState
    }
}
```

有没有对`LifecycleEventObserver`这个类有点熟悉，我们的MyObserver2是不是实现了这个接口，所以这里就是将我们自己写的观察者传了进来。

好，这里知道了这个循环的作用，我们在来看看sync这个方法是咋同步observer的State的，

```kotlin
private fun sync() {
    val lifecycleOwner = lifecycleOwner.get()
        ?: throw IllegalStateException(
            "LifecycleOwner of this LifecycleRegistry is already " +
                "garbage collected. It is too late to change lifecycle state."
        )
    // 一个循环，isSynced表示是否同步给了所有观察者
    while (!isSynced) {
        newEventOccurred = false
        if (state < observerMap.eldest()!!.value.state) {
            backwardPass(lifecycleOwner)
        }
        val newest = observerMap.newest()
        if (!newEventOccurred && newest != null && state > newest.value.state) {
            forwardPass(lifecycleOwner)
        }
    }
    newEventOccurred = false
}
```

可以看到在这个循环中有两个方法`backwardPass(lifecycleOwner)`和`forwardPass(lifecycleOwner)`，其实这两个方法差不多，就是判断该观察者的State与当前的State进行判断，如果小于，即让其生命周期往前走（onCreate ---> onStart），否则，往后。我们来看看往后走的

```kotlin
private fun backwardPass(lifecycleOwner: LifecycleOwner) {
    val descendingIterator = observerMap.descendingIterator()
    while (descendingIterator.hasNext() && !newEventOccurred) {
        val (key, observer) = descendingIterator.next()
        while (observer.state > state && !newEventOccurred && observerMap.contains(key)
        ) {
            val event = Event.downFrom(observer.state)
                ?: throw IllegalStateException("no event down from ${observer.state}")
            pushParentState(event.targetState)
            observer.dispatchEvent(lifecycleOwner, event)
            popParentState()
        }
    }
}
```

可以看到这里还是调用了`observer.dispatchEvent(lifecycleOwner, event)`这个方法去同步，并且之前，得到了一个事件

```kotlin
@JvmStatic
public fun downFrom(state: State): Event? {
    return when (state) {
        State.CREATED -> ON_DESTROY
        State.STARTED -> ON_STOP
        State.RESUMED -> ON_PAUSE
        else -> null
    }
}
```

网上有张图，描述了事件与状态的变化

![img](https://redrock.feishu.cn/space/api/box/stream/download/asynccode/?code=ODNkNmNmNDU4ODRjYWQ0NjM3ZmQ0NmMwYjIzZjhjMmNfMXhiZWwwdWFWVTh0Mlg5WjlxRENkSFU2aEhCbWVIN0lfVG9rZW46WUxGUmJSYXhpb0tQeU94Snc2b2NETlA5blZmXzE3NDcyMDE5OTE6MTc0NzIwNTU5MV9WNA)

# LiveData

## 什么是LiveData？

官方定义：

> [LiveData ](https://developer.android.com/reference/android/arch/lifecycle/LiveData)是一种可观察的数据存储器类。与常规的可观察类不同，LiveData 具有生命周期感知能力，意指它遵循其他应用组件（如 Activity/Fragment）的生命周期。这种感知能力可确保 LiveData 仅更新处于活跃生命周期状态的应用组件观察者。

简单来说，LiveData就是一个数据的持有者，数据被包装后，可以被observer观察，当数据发生变化是observer可以感知到，但这种感知，只会发生在**LifecycleOwner的活跃生命周期状态（STARTED,RESUMED）**

## LiveData的好处

- 确保UI与数据状态匹配

当我们的数据发生变化时，可以通知到UI更新

- 避免内存泄漏

观察者绑定到lifecycle对象上，当与之绑定的lifecycle销毁时，他们会自动被清理

- 不会给已经停止的Activity发送消息

当所绑定的lifecycle处于非活跃状态时，比如处于栈中的Activity，不会受到任何LiveData的消息

- 不需要手动处理生命周期

LiveData具备生命周期感知能力，UI只需对相关的数据进行监听

- 系统配置更改时，进行数据的保存和恢复，及 UI 的恢复

当Activity与Fragment发生配置更改而重新构建时（如，旋转手机屏幕），会收到最新的消息

- 资源共享

我们可以用单例模式来扩展LiveData，当数据发生变化时，就会通知所有的观察者

## LiveData的用法

LiveData是一个抽象类，不能直接使用。通常我们使用的是它的直接子类`MutableLiveData`。

```kotlin
class LiveDataActivity: AppCompatActivity() {

    private val mLiveData = MutableLiveData<String>()
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_livedata)
        val mTextView: TextView = findViewById(R.id.textView)
        val button: Button = findViewById(R.id.button)
        button.setOnClickListener {
            mLiveData.value = "值被改变了"
        }
        mLiveData.observe(this) {
            mTextView.text = it
        }
    }
}
```

可以看到，用法还是挺简单的，先定义了一个MutableLiveData对象mLiveData，然后去改变里面所包装的数据，怎么改变的呢？提供了两种方法，`setValue`和`postValue`（子线程使用），然后我们后面观察，传入的是拥有生命周期的组件，还有一个当收到数据之后的回调，我们这里将TextView的文本进行改变。

## LiveData的原理

跟Lifecycle一样我们从observe这个方法看起，

```java
@MainThread
public void observe(@NonNull LifecycleOwner owner, @NonNull Observer<? super T> observer) {
    assertMainThread("observe");
    if (owner.getLifecycle().getCurrentState() == DESTROYED) {
        // ignore
        return;
    }
    LifecycleBoundObserver wrapper = new LifecycleBoundObserver(owner, observer);
    ObserverWrapper existing = mObservers.putIfAbsent(observer, wrapper);
    if (existing != null && !existing.isAttachedTo(owner)) {
        throw new IllegalArgumentException("Cannot add the same observer"
                + " with different lifecycles");
    }
    if (existing != null) {
        return;
    }
    owner.getLifecycle().addObserver(wrapper);
}
```

可以看到，当LifecycleOwner的状态为DESTROYED时，直接返回，然后用我们呢传进来的owner和observer组装成了一个新的类LifecycleBoundObserver，然后使用putIfAbsent方法observer-wrapper作为key-value添加到观察者列表mObservers中。（putIfAbsent意思是只有列表中没有这个observer时才会添加），最后用LifecycleOwner的Lifecycle添加observer的封装wrapper

既然我们最后是用Lifecycle将我们的观察者wrapper加进去的，所以我们来看看LifecycleBoundObserver类

```java
class LifecycleBoundObserver extends ObserverWrapper implements LifecycleEventObserver {
    @NonNull
    final LifecycleOwner mOwner;

    LifecycleBoundObserver(@NonNull LifecycleOwner owner, Observer<? super T> observer) {
        super(observer);
        mOwner = owner;
    }

    // STARTED状态或者RESUME状态
    @Override
    boolean shouldBeActive() {
        return mOwner.getLifecycle().getCurrentState().isAtLeast(STARTED);
    }

    @Override
    public void onStateChanged(@NonNull LifecycleOwner source,
            @NonNull Lifecycle.Event event) {
        Lifecycle.State currentState = mOwner.getLifecycle().getCurrentState();
        // 当当前状态为DESTROYED时，移除观察者
        if (currentState == DESTROYED) {
            removeObserver(mObserver);
            return;
        }
        Lifecycle.State prevState = null;
        while (prevState != currentState) {
            prevState = currentState;
            activeStateChanged(shouldBeActive());
            currentState = mOwner.getLifecycle().getCurrentState();
        }
    }

    @Override
    boolean isAttachedTo(LifecycleOwner owner) {
        return mOwner == owner;
    }

    @Override
    void detachObserver() {
        mOwner.getLifecycle().removeObserver(this);
    }
}
```

可以看到它实现了LifecycleEventObserver接口，并且重写了onStateChanged这个方法。当状态为DESTROYED时，会自动移除观察者，故而避免内存泄漏，如果不是，则会调用ObserverWrapper中的`activeStateChanged(shouldBeActive())`

```Java
void activeStateChanged(boolean newActive) {
    if (newActive == mActive) {
        return;
    }
    // immediately set active state, so we'd never dispatch anything to inactive
    // owner
    mActive = newActive;
    changeActiveCounter(mActive ? 1 : -1);
    if (mActive) {
        dispatchingValue(this);
    }
}
```

mActive是ObserverWrapper的属性，表示此观察者是否活跃，默认是false。所以，如果newActive也是false就不会去处理了，如果是活跃的，就会去出发changeActiveCounter(mActive ? 1 : -1)方法，用来改变LiveData中的mActiveCount（当前活跃观察者数量的计数器），我们主要看`dispatchingValue(this)`这个方法

```Java
@SuppressWarnings("WeakerAccess") /* synthetic access */
void dispatchingValue(@Nullable ObserverWrapper initiator) {
    // 如果事件正在分发，则这次分发没有用，则返回
    if (mDispatchingValue) {
        mDispatchInvalidated = true;
        return;
    }
    // 表示正在分发
    mDispatchingValue = true;
    do {
        mDispatchInvalidated = false;
        if (initiator != null) {
            considerNotify(initiator);
            initiator = null;
        } else {
            for (Iterator<Map.Entry<Observer<? super T>, ObserverWrapper>> iterator =
                    mObservers.iteratorWithAdditions(); iterator.hasNext(); ) {
                considerNotify(iterator.next().getValue());
                if (mDispatchInvalidated) {
                    break;
                }
            }
        }
    } while (mDispatchInvalidated);
    mDispatchingValue = false;
}
```

这里存在两个boolean变量，`mDispatchingValue`和`mDispatchInvalidated`，一个用来标记当前是否在分发事件，一个表示当前分发已经无效，下次需要重新分发，然后，当我们传进来的initiator不为空时，就会去专门通知这一个观察者，调用`considerNotify(initiator)`这个方法，如果为空则会去用一个循环去通知所有观察者，这里为什么可以为空，我们先不谈，咱们先来看看`considerNotify(initiator)`这个方法。

```Java
@SuppressWarnings("unchecked")
private void considerNotify(ObserverWrapper observer) {
    if (!observer.mActive) {
        return;
    }
    // Check latest state b4 dispatch. Maybe it changed state but we didn't get the event yet.
    //
    // we still first check observer.active to keep it as the entrance for events. So even if
    // the observer moved to an active state, if we've not received that event, we better not
    // notify for a more predictable notification order.
    if (!observer.shouldBeActive()) {
        observer.activeStateChanged(false);
        return;
    }
    if (observer.mLastVersion >= mVersion) {
        return;
    }
    observer.mLastVersion = mVersion;
    observer.mObserver.onChanged((T) mData);
}
```

先进行检查，如果当前观察者不是活跃状态返回，否则会再次会检测一下观察者对应的owner，看其是否是活跃状态，如果不是，再次调用activeStateChanged方法，并传入false，其内部会再次判断。然后，在判断版本号，如果observer所持有的最新版本号大于等于当前的版本，则表示是最新版本，不需要分发了，否则，分发事件。

到这里，逻辑应该通畅了许多。我们上面提到了版本号，这是在哪里改变的呢？答案就在setValue和postValue两个数据更新的方法里

```typescript
@MainThread
protected void setValue(T value) {
    assertMainThread("setValue");
    mVersion++;
    mData = value;
    dispatchingValue(null);
}

protected void postValue(T value) {
    boolean postTask;
    synchronized (mDataLock) {
        postTask = mPendingData == NOT_SET;
        mPendingData = value;
    }
    if (!postTask) {
        return;
    }
    //抛到主线程
    ArchTaskExecutor.getInstance().postToMainThread(mPostValueRunnable);
}
private final Runnable mPostValueRunnable = new Runnable() {
    @SuppressWarnings("unchecked")
    @Override
    public void run() {
        Object newValue;
        synchronized (mDataLock) {
            newValue = mPendingData;
            mPendingData = NOT_SET;
        }
        setValue((T) newValue);
    }
};
```

可以看到在setValue方法中有一个`mVersion++`，然后调用了`dispatchingValue(null)`方法，及上面提到的空的情况，所以一切都明了了。而postValue一样最后会调用setValue方法，只不过它用于在子线程中更新数据。

到这，关于LiveData的源码分析就到这，简单来说，就是LivaData通过observe()添加与LifecycleOwner绑定的观察者；观察者变为活跃时回调最新的数据；使用setValue()、postValue()更新数据时会通知回调所有的观察者。

# ViewModel

## 什么是ViewModel？

ViewModel，意为 视图模型，即 **为界面准备数据的模型**。

简单理解就是，ViewModel是一个为UI层提供数据的东西。

## ViewModel出现的背景

当我们的activity或fragment发生意外重构了（如屏幕旋转等），我们数据还会有吗？

答案是否定的，这个时候，viewmodel就发挥出用处了，原因就在于viewmodel的生命周期比activity的更长。

![img](https://redrock.feishu.cn/space/api/box/stream/download/asynccode/?code=ZmE5OTE5OTliNTc2Zjk4MjZmNWI2NjA0NjdiNDkwZDRfamlVSUV5UUliT0JyelFsazFxUnpGa1lCNW5xYkkxU0pfVG9rZW46VEZ0ZmJ1VVlUb0lJTlR4aUhMaWNQMEVIbnN6XzE3NDcyMDE5OTE6MTc0NzIwNTU5MV9WNA)

## ViewModel的使用

### 创建ViewModel

```kotlin
class TestViewmodel : ViewModel() {
    private val _data = MutableLiveData<String>()
    val data: LiveData<String>
        get() = _data

    fun getData() {
        _data.value = "获取到了数据"
    }
}
```

一般来说，我们不应该将MutableLiveData暴露给activity，而是在viewmodel里去修改数据，然后将其转化为LiveData传给activity。

### activity获取viewmodel

```kotlin
class ViewModelActivity : AppCompatActivity() {

    private val binding by lazy { ActivityViewmodelBinding.inflate(layoutInflater) }
    private val mViewModel by lazy {
        ViewModelProvider(this)[TestViewmodel::class.java]
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(binding.root)
        binding.btn.setOnClickListener {
            mViewModel.getData()
        }
        // 观察
        mViewModel.data.observe(this) {
            binding.tv.text = it
        }
    }
}
```

这里我们用了个ViewModelProvider，传了个Lifecycleowner进去，然后把我们的ViewModel写了进去。

然后通过点击事件，执行我们viewmodel里面的getData方法，然后观察完事！

当然，还提供了其他的创建方式

导入依赖

```kotlin
// activity
implementation("androidx.activity:activity-ktx:1.10.1")
// fragment
implementation("androidx.fragment:fragment-ktx:1.8.6")
```

然后就可以用下面的方式创建

```kotlin
private val mViewModel by viewModels<TestViewmodel>()
```

## ViewModel的原理

为什么当activity/fragment重建后，viewmodel还可以存在呢？

我们从`ViewModelProvider(this)[TestViewmodel::class.java]`下手，ViewModelProvider由名可知Viewmodel的提供者，我们来看看它的构造方法

```Kotlin
public constructor(
    owner: ViewModelStoreOwner
) : this(owner.viewModelStore, defaultFactory(owner), defaultCreationExtras(owner))

public constructor(owner: ViewModelStoreOwner, factory: Factory) : this(
    owner.viewModelStore,
    factory,
    defaultCreationExtras(owner)
)
```

介绍一下各个类

- `ViewModelStoreOwner`：ViewModel存储器拥有者
- `ViewModelStore`：ViewModel存储器
- `Factory`：创造ViewModel实例的工厂

我们先来看看ViewModelStoreOwner:

```Kotlin
interface ViewModelStoreOwner {

    /**
     * The owned [ViewModelStore]
     */
    val viewModelStore: ViewModelStore
}
```

可以看到其是一个接口，我们的Activity和Fragment都实现了这个接口，都是ViewModel存储器拥有者，我们先不管他们是咋获取到ViewModelStore的，我们先来看看ViewModelStore是咋存储Viewmodel的

```kotlin
open class ViewModelStore {

    private val map = mutableMapOf<String, ViewModel>()

    @RestrictTo(RestrictTo.Scope.LIBRARY_GROUP)
    fun put(key: String, viewModel: ViewModel) {
        val oldViewModel = map.put(key, viewModel)
        oldViewModel?.onCleared()
    }

    @RestrictTo(RestrictTo.Scope.LIBRARY_GROUP)
    operator fun get(key: String): ViewModel? {
        return map[key]
    }

    @RestrictTo(RestrictTo.Scope.LIBRARY_GROUP)
    fun keys(): Set<String> {
        return HashSet(map.keys)
    }

    fun clear() {
        for (vm in map.values) {
            vm.clear()
        }
        map.clear()
    }
}
```

可以看到，内部还是比较清晰的，viewModel作为Value存储在map中

我们再来看看Factory是咋生产ViewModel的

```kotlin
public interface Factory {

    public fun <T : ViewModel> create(modelClass: Class<T>): T {
        throw UnsupportedOperationException(
            "Factory.create(String) is unsupported.  This Factory requires " +
                "`CreationExtras` to be passed into `create` method."
        )
    }

    public fun <T : ViewModel> create(modelClass: Class<T>, extras: CreationExtras): T =
        create(modelClass)

    companion object {
        @JvmStatic
        fun from(vararg initializers: ViewModelInitializer<*>): Factory =
            InitializerViewModelFactory(*initializers)
    }
}
```

可以看到它是一个接口，而我们的实现类是`NewInstanceFactory` 

```kotlin
@Suppress("SingletonConstructor")
public open class NewInstanceFactory : Factory {
    @Suppress("DocumentExceptions")
    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        return try {
            modelClass.getDeclaredConstructor().newInstance()
        } catch (e: NoSuchMethodException) {
            throw RuntimeException("Cannot create an instance of $modelClass", e)
        } catch (e: InstantiationException) {
            throw RuntimeException("Cannot create an instance of $modelClass", e)
        } catch (e: IllegalAccessException) {
            throw RuntimeException("Cannot create an instance of $modelClass", e)
        }
    }
    ...
}
```

这边利用了反射机制，将传入的class通过反射获取Viewmodel.

> 反射是 Java 中的一个特性，它允许程序在运行时获取自身的信息，并动态地操作类或对象的属性、方法和构造函数。通过反射，我们可以在事先不知道确切类名的情况下实例化对象、调用方法和设置属性。

我们再来看看`ViewModelProvider(this)[TestViewmodel::class.java]`的中括号是什么，其实是一个get方法，这里用到了kotlin中的语法糖，重载了 `operator fun get()`，这样可以使用中括号代替get方法，我们来看看这个get方法

```kotlin
@MainThread
public open operator fun <T : ViewModel> get(modelClass: Class<T>): T {
    val canonicalName = modelClass.canonicalName
        ?: throw IllegalArgumentException("Local and anonymous classes can not be ViewModels")
    return get("$DEFAULT_KEY:$canonicalName", modelClass)
}

@Suppress("UNCHECKED_CAST")
@MainThread
public open operator fun <T : ViewModel> get(key: String, modelClass: Class<T>): T {
    val viewModel = store[key]
    if (modelClass.isInstance(viewModel)) {
        (factory as? OnRequeryFactory)?.onRequery(viewModel!!)
        return viewModel as T
    } else {
        @Suppress("ControlFlowWithEmptyBody")
        if (viewModel != null) {
            // TODO: log a warning.
        }
    }
    val extras = MutableCreationExtras(defaultCreationExtras)
    extras[VIEW_MODEL_KEY] = key
    // AGP has some desugaring issues associated with compileOnly dependencies so we need to
    // fall back to the other create method to keep from crashing.
    return try {
        factory.create(modelClass, extras)
    } catch (e: AbstractMethodError) {
        factory.create(modelClass)
    }.also { store.put(key, it) }
}
```

应该挺好知道的，就是将viewmodel从我们的ViewModelStore中取出来，如果没有，就会去用factory创造出来，然后再存入store里面。

ok，现在我们知道了Viewmodel是怎么被存储的，怎么被创造的，接下来就来聊聊我们一开始的那个问题，为什么viewmodel再activity/fragment异常重建后不会消失。

我们来看看activity/fragment是咋从获取到ViewmodelStore，

```kotlin
override val viewModelStore: ViewModelStore
    get() {
        // activity还没关联Application，即不能在onCreate之前去获取viewModel
        checkNotNull(application) {
            ("Your activity is not yet attached to the " +
                "Application instance. You can't request ViewModel before onCreate call.")
        }
        ensureViewModelStore()
        return _viewModelStore!!
    }

private fun ensureViewModelStore() {
    if (_viewModelStore == null) {
        val nc = lastNonConfigurationInstance as NonConfigurationInstances?
        if (nc != null) {
            // Restore the ViewModelStore from NonConfigurationInstances
            _viewModelStore = nc.viewModelStore
        }
        if (_viewModelStore == null) {
            _viewModelStore = ViewModelStore()
        }
    }
}
```

可以看到如果_viewModelStore为空的话，会从NonConfigurationInstance中获取我们的viewModelStore，如果它里面没有，就会重新定义一个ViewModelStore，那这个NonConfigurationInstance是个啥呢？翻译过来:**非配置实例**,即与系统配置无关的实例。所以屏幕旋转等的配置改变不会影响到这个实例,它是Android官方提供给因配置改变而要保存的数据的。activity中有个onRetainCustomNonConfigurationInstance方法，他会在onStop和onDeatory的时候会在onRetainCustomNonConfigurationInstance中保存的自定义非配置实例

```kotlin
@Suppress("deprecation")
final override fun onRetainNonConfigurationInstance(): Any? {
    // Maintain backward compatibility.
    val custom = onRetainCustomNonConfigurationInstance()
    var viewModelStore = _viewModelStore
    if (viewModelStore == null) {
        // No one called getViewModelStore(), so see if there was an existing
        // ViewModelStore from our last NonConfigurationInstance
        val nc = lastNonConfigurationInstance as NonConfigurationInstances?
        if (nc != null) {
            viewModelStore = nc.viewModelStore
        }
    }
    if (viewModelStore == null && custom == null) {
        return null
    }
    val nci = NonConfigurationInstances()
    nci.custom = custom
    nci.viewModelStore = viewModelStore
    return nci
}
```

我们就是利用这里去保存我们的ViewmodelStore。

# ViewBinding

## 从 手写`findViewById` 到 ViewBinding

如标题所示，ViewBinding的用处就是为了代替`findViewById`，部分与UI控件相关的代码可以在布局文件中完成。使用 ViewBinding后，不再需要findViewById()方法。 

## ViewBinding的使用

1. 启用Viewbinding，在app模块目录下 **`build.gradle`** 添加下述配置：

```Groovy
android {
    ...
    viewBinding{
        enable = true
    }
    // 或者
    buildFeatures{
        viewBinding = true
    }
}
```

配置过后，我们的每一个layout文件都会生成一个对应的绑定类，该绑定类的命名就是 layou 文件的名称转换为驼峰形式，并在末尾添加 Binding 一词。以 activity_main.xml 为例，其对应的绑定类名称为 ActivityMainBinding。

上面说了会为每个layout文件都生成一个绑定类，如果该布局文件不需要的话，可以使用 `tools:viewBinding-Ignore` 属性来设置

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    tools:viewBinding-Ignore="true"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

</androidx.constraintlayout.widget.ConstraintLayout>
```

1. 在Activity中使用ViewBinding

activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/tv_main"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="这是个文本"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

MainActivity中

```Kotlin
class MainActivity : AppCompatActivity() {

    // 首先声明变量
    private lateinit var binding: ActivityMainBinding
    
    // 或者使用懒加载
    private val binding by lazy {
        ActivityMainBinding.inflate(layoutInflater)
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        // 通过生成的binding去加载布局
        binding = ActivityMainBinding.inflate(layoutInflater)
        // 调用binding的getRoot()可以得到activity_main.xml的根布局
        setContentView(binding.root)
        
        // 使用binding实例获取到控件
        binding.tvMain.setOnClickListener {
            binding.tvMain.text = "我被点击了"
        }
    }
}
```

# DataBinding

ok，聊完了ViewBinding，我们来聊聊DataBinding

## 什么是DataBinding？

前面提到的ViewBinding其实可以看作是DataBinding的子集，他的功能基本上DataBinding都有，而且DataBinding还多了数据绑定的功能。

什么意思呢？举个简单例子

> 有一个计数器功能，有个变量num，一个显示的TextView，二者绑定，当num变化时，TextView自动刷新，不需我们手动setText了。

上面这种叫单向绑定，还有一种双向绑定，就是视图发生改变会使得数据源也发生变化。

## Databinding用法

与ViewBinding一样，先启用

在app模块目录下 **`build.gradle`** 添加下述配置：

```Groovy
android {
    ...
    dataBinding {
        enable = true
    }
    // 二选一即可
    buildFeatures {
        dataBinding = true
    }
}
```

写一个计数器的例子

activity中

```Kotlin
class DatabindingActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_databinding)
        var num = 0
        val tv = findViewById<TextView>(R.id.tv_databinding)
        
        findViewById<Button>(R.id.bt_databinding).setOnClickListener { 
            tv.text = "计数器：${++num}"
        }
    }
}
```

这是我们普通写法。

使用DataBinding

1. 来到layout文件，将鼠标放到根布局上，按Alt和Enter键，点击**`Convert to data binding layout`**

![img](https://redrock.feishu.cn/space/api/box/stream/download/asynccode/?code=MjIxM2E3YjE5Mjc3OWNlYzQyODQ3MmEzZDJlYzNlMDVfVkU0TXVqQ3c2WEpiRDFic0ZRd1NVMXlvRlRHaDlxSWJfVG9rZW46UEJCTWJVaGJBb2pvc0p4QThramNISWt5bkNiXzE3NDcyMDE5OTE6MTc0NzIwNTU5MV9WNA)

生成之后的文件

![img](https://redrock.feishu.cn/space/api/box/stream/download/asynccode/?code=YmZjZmIxMDQ5MGZiMTIwZGJjNmEzNjk2OGRlODdjZTBfZzBDeVp5R25LUFdZVjJkbHd3M3RmdlFHUTJvNDZzVVBfVG9rZW46RUt5QmJ0MmU2b3h6ZWp4amtra2M2dGdQbmpiXzE3NDcyMDE5OTE6MTc0NzIwNTU5MV9WNA)

接下来，在data标签中添加属性，并将TextView指向该属性

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android">

    <data>
        <variable
            name="mNum"
            type="int" />
    </data>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <TextView
            android:id="@+id/tv_databinding"
            android:layout_width="match_parent"
            android:layout_height="48dp"
            android:gravity="center"
            android:text="@{`计数器：`+ mNum}" />

        <Button
            android:id="@+id/bt_databinding"
            android:layout_width="100dp"
            android:layout_height="wrap_content"
            android:layout_gravity="center_horizontal"
            android:text="增加" />

    </LinearLayout>
</layout>
```

使用variable属性写我们的变量，在到TextView中的android:text指向属性

再到activity中

```Kotlin
class DatabindingActivity : AppCompatActivity() {
    private val binding by lazy {
        ActivityDatabindingBinding.inflate(layoutInflater)
    }
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(binding.root)
        binding.mNum = 0
        findViewById<Button>(R.id.bt_databinding).setOnClickListener {
            binding.mNum++
        }
    }
}
```

这样同样实现了计数器的效果。

ok，关于DataBinding，我们就讲这些，如果想要深入了解的同学可以网上自行找资料，或者看下面的文章

[Data Binding (中文文档)这篇文档将教你如何运用 Data Binding 类库来编写声明试布局，并且尽量减 - 掘金](https://juejin.cn/post/6844903550808489998?searchId=202503201011098523E66CDAABD5A5B413)

# Navigation

大家应该都用过Fragment，而Navigation是为了我们更好的管理Fragment而推出的。

它有如下优点：

- 可视化的页面导航图，便于我们理清页面间的关系。 
- 通过destination和action完成页面间的导航。 
- 方便添加页面切换动画。 
- 页面间类型安全的参数传递。
- 通过NavigationUI类，对菜单、底部导航、抽屉菜单导航进行统一的管理。 
- 支持深层链接DeepLink。 

## Navigation的使用

添加依赖

```Groovy
dependencies {
    val nav_version = "2.8.9"
    implementation("androidx.navigation:navigation-fragment-ktx:$nav_version")
    implementation("androidx.navigation:navigation-ui-ktx:$nav_version")
}
```

我们先来认识一下，Navigation的三个组成

- **Navigation Graph**：存放在`res/navigation`目录下，用来描述导航相关信息的 XML 资源
- **NavHostFragment**：显示导航图中目标的空白容器，其实就是用来放置 `fragment`的父容器
- **NavController**： 真正的用于导航动作实现的控制者，用于在代码中完成Navigation Graph中具体的页面切换工作。 

ok，认识完了之后，我们来学习一下用法

1. 创建Navigation Graph，新建`res/navigation`目录，依次选中res文件夹->New->Android Resource File

![img](https://redrock.feishu.cn/space/api/box/stream/download/asynccode/?code=NzA4MDU4YmFhNGUxZWMxYzM2MmJhZTEzMjhlZDFjMGFfdVQ1MFFsS3Y5aHBHaXNOS010NEpEQW1UeElSMXMzS3BfVG9rZW46UW5DZWJvWVlib1o2emt4dEZYYWMxTGt3blhiXzE3NDcyMDE5OTE6MTc0NzIwNTU5MV9WNA)

![img](https://redrock.feishu.cn/space/api/box/stream/download/asynccode/?code=NzI5M2YzMTMzMmMxN2I4OWVlZjQwYTgzNmIxNjIxNTJfWVZETEcwWVRWRGtKaGlUMklTYngyY0ExSERuenBJVDZfVG9rZW46UlI1UWJWa24wb1BINmN4MVZUZ2MyUE1jblVoXzE3NDcyMDE5OTE6MTc0NzIwNTU5MV9WNA)

Resource type选择navigation。

1. 然后新建一个`navigation`的`resource`文件

![img](https://redrock.feishu.cn/space/api/box/stream/download/asynccode/?code=NWU5YTFkZGJmMmQ1ZTIyMDViZWVhZjNmZTc3NWJkZjlfekhBdVQ3N01JZnNabEJqQ0c5dHZTR2JQWkR1OXQ2bWJfVG9rZW46SzR6cGIwWGI3b1psNUV4NVRETGNvbmFnbkJjXzE3NDcyMDE5OTE6MTc0NzIwNTU5MV9WNA)

1. 向其中添加我们的Fragment（这里准备了三个HomeFragment，BlankFragment，MineFragment）

```xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_graph"
    app:startDestination="@+id/fragment_home">

    <fragment
        android:id="@+id/fragment_home"
        android:name="com.sanhuzhen.jetpackdemo.fragment.HomeFragment"
        android:label="fragment_home"
        tools:layout="@layout/fragment_home"/>
    <fragment
        android:id="@+id/fragment_blank"
        android:name="com.sanhuzhen.jetpackdemo.fragment.BlankFragment"
        android:label="fragment_blank"
        tools:layout="@layout/fragment_blank" />

    <fragment
        android:id="@+id/fragment_mine"
        android:name="com.sanhuzhen.jetpackdemo.fragment.MineFragment"
        android:label="fragment_mine"
        tools:layout="@layout/fragment_mine"/>

</navigation>
```

加完之后，就可以用as进行可视化操作，实现不同fragment之间的跳转路径

![img](https://redrock.feishu.cn/space/api/box/stream/download/asynccode/?code=OTZjM2I0MGIxZjVjYTMxMTdjMmEyODRmM2FjODRiZjNfRWpNM05TN0tlWFBDZVNsODBMaWVXUEhWZ0JranhWSVZfVG9rZW46RjZaTGJnTjVub1ByanR4OGVjZWNnZEpybkFkXzE3NDcyMDE5OTE6MTc0NzIwNTU5MV9WNA)

之后可以看到xml文件中加了一点东西

```bash
<fragment
    android:id="@+id/fragment_home"
    android:name="com.sanhuzhen.jetpackdemo.fragment.HomeFragment"
    android:label="fragment_home"
    tools:layout="@layout/fragment_home">
    <action
        android:id="@+id/action_fragment_home_to_fragment_blank"
        app:destination="@id/fragment_blank" />
</fragment>
```

可以看到这里加了一个子结点action，子结点中的`destination`则定义了要跳转的目标`fragment`

1. 在activity布局中的处理

切换到activity的布局中来

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".activity.NavActivity">
    <fragment
        android:id="@+id/nav_host_fragment"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:defaultNavHost="true"
        app:navGraph="@navigation/nav_graph"/>
</androidx.constraintlayout.widget.ConstraintLayout>
```

在其布局文件中添加一个fragment或者fragmentContainerView，将其name属性设置为下面

```xml
android:name="androidx.navigation.fragment.NavHostFragment"
```

`NavHostFragment`是作为`Navigation`中`fragment`的显示载体来出现的

`app:defaultNavHost="true"`这个表面该Fragment会自动处理系统返回键。即当用户按下手机的返回按钮时，系统能自动将当前所展示的Fragment退出。

`app:navGraph`则是设置该Fragment对应的导航图

1. 跳转页面

来到我们的HomeFragment的布局中，添加一个按钮

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".fragment.HomeFragment">
    <TextView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text="@string/hello_blank_fragment" />
    <Button
        android:id="@+id/btn_test"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:text="跳转" />
</FrameLayout>
```

然后来到HomeFragment

```Kotlin
class HomeFragment : Fragment() {

    private var _binding: FragmentHomeBinding? = null
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        _binding = FragmentHomeBinding.inflate(inflater, container, false)
        return binding.root
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        binding.btn.setOnClickListener {
            Navigation.findNavController(it).navigate(R.id.action_fragment_home_to_fragment_blank)
        }
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null  // 避免内存泄漏
    }
}
```

这里使用ViewBinding，但Fragment存在时间比其视图长。请务必在 `Fragment` 的 `onDestroyView()` 方法中清除对绑定类实例的所有引用。

```Kotlin
Navigation.findNavController(it).navigate(R.id.action_fragment_home_to_fragment_blank)
```

使用上面这句进行页面跳转，这里的id即是我们的action的id

# Room

## 什么是Room？

Room是是用于处理本地数据库的库。Room 持久性库在 SQLite 上提供了一个抽象层，以便在充分利用 SQLite 的强大功能的同时，能够流畅地访问数据库。具体来说，Room 具有以下优势：

- 针对 SQL 查询的编译时验证。 
- 可最大限度减少重复和容易出错的样板代码的方便注解。 
- 简化了数据库迁移路径。 

## Room的组成

Room主要包括三个组件：

- **DataBase**：保存数据库并作为应用持久性数据底层连接的主要访问点
- **Entity**：用于表示应用的数据库中的表
- **Dao**：提供你的应用可用于查询、更新、插入和删除数据库中的数据的方法

我们在使用时一般都先通过DataBase获取到Dao，然后用Dao来操作Entity

## Room的使用

首先添加依赖

```Kotlin
dependencies {
    val room_version = "2.6.1"
    implementation("androidx.room:room-runtime:$room_version")
    annotationProcessor("androidx.room:room-compiler:$room_version")
}
```

### 建表

```kotlin
@Entity
data class User(
    @PrimaryKey // 主键
    var uId: Int,
    @ColumnInfo(name = "user_name")
    var userName: String,
    @ColumnInfo(name = "user_age")
    var userAge: Int,
    @Ignore
    var gender: String
)
```

- `@Entity`：用于将User类与Room中的数据表对应起来。tableName属性可以为数据表设置表 名，若不设置，则表名和类名相同。
- `@PrimaryKey`：用于指定该字段作为表的主键
- `@ColumnInfo`：用于设置该字段存储在数据库表中的名字。若不设置，则默认使用字段名称作为数据库中的列名称
- `@Ignore`：告诉Room忽略该字段或方法

当然，还有其他的，感兴趣可以点击下面链接

https://juejin.cn/post/7205417093973884983?searchId=202503221327580BFBF2FE3B9DF27C8C76

### 创建数据访问对象(DAO)

新建一个名为UserDao的接口文件，并在接口的上方添加@Dao标签。 

- `@Insert`：定义将其参数插入数据库中的相应表的方法。 
- `@Update`：定义用于更新数据库表中特定行的方法。 
- `@Delete`：定义用于从数据库表中删除特定行的方法。 
- `@Query`：定义用于自己编写 SQL 语句

```kotlin
@Dao
interface UserDao {
    /**
     * 插入数据，当主键冲突时选择直接覆盖原数据
     *
     * 可选数据：
     * OnConflictStrategy.REPLACE 如果发生冲突，直接覆盖已有数据，将表中现存的数据替换成插入的这条；
     * OnConflictStrategy.IGNORE 如果发生冲突，直接忽略此次插入操作
     * OnConflictStrategy.NONE 这个是默认的策略，它和ABORT作用是一致的，都是终止此次插入操作，并且抛出SQLiteConstraintException
     * OnConflictStrategy.ROLLBACK 这个表示如果发生冲突，终止插入操作，并且将事务回滚到最初的状态，在最新版本已经被标记@Deprecated推荐使用ABORT
     * OnConflictStrategy.ABORT 和NONE作用一致，这里就不过多介绍
     * OnConflictStrategy.FAIL 这个表示如果发生冲突，终止插入操作，并且抛出SQLiteConstraintException异常，在最新版本也是被标记@Deprecated，也是推荐使用ABORT。
     */
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    fun insert(user: User)

    /**
     * 删除数据
     */
    @Delete
    fun delete(user: User)

    /**
     * 删除所有用户，使用Query注解可以执行自定义的SQL语句
     */
    @Query("DELETE FROM user")
    fun deleteAll()

    /**
     * 更新数据
     */
    @Update
    fun update(user: User)

    /**
     * 查询数据
     */
    @Query("select * from user where uId=:id")
    fun queryById(id : Int): User?

    @Query("select * from user")
    fun queryAll(): List<User>
}
```

### 创建数据库

`@Database`用于告诉系统这是Room数据库对象。

`entities`属性用于指定该数据库有哪些表，若需要建立多张表，则表名以逗号隔开。

`version`属性用于指定数据库版本号，后面数据库的升级正是依据版本号进行判断的。 

数据库类必须是一个抽象类，用于拓展`RoomDatabase`。对于与数据库关联的每个DAO类，数据库类必须定义一个具有零参数的抽象方法，并返回 DAO 类的实例。 

```kotlin
@Database(entities = [User::class], version = 1)
abstract class AppDataBase: RoomDatabase() {
    abstract fun userDao(): UserDao
}
```

### 使用

创造数据库实例

```kotlin
class RoomActivity : AppCompatActivity() {

    private val userDataBase by lazy {
        Room.databaseBuilder(this, AppDataBase::class.java, "user.db").build()
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_room)

    }
}
```

通过懒加载创造数据库实例，然后就可以用Dao去处理相关数据

![img](https://redrock.feishu.cn/space/api/box/stream/download/asynccode/?code=MTk4NTQ1NDQ2YTQyZGVkMjU1ZGQyOWUzZTA2MTVlOTlfaHJZQ0ZrczhZbGdtS0hPc2kxbnlJVU1hckVPUUNza21fVG9rZW46RWRJQWI0WVlob25adnF4c2N5OGM2eEtRbkllXzE3NDcyMDE5OTE6MTc0NzIwNTU5MV9WNA)

除了上述方法，我们也可以在AppDatabase里封装实例化数据库的代码

```kotlin
companion object {
    private lateinit var INSTANCE: AppDataBase
    fun getDatabase(context: Context): AppDataBase {
        synchronized(AppDataBase::class) {
            if (!::INSTANCE.isInitialized) {
                INSTANCE = Room.databaseBuilder(
                    context, AppDataBase::class.java, "user.db"
                ).fallbackToDestructiveMigration().build()
            }
        }
        return INSTANCE
    }
}
```

## 升级数据库

当我们要增加或删除数据库中字段名，我们就需要升级数据库的版本，在Room中，我们可以通过`@DataBase`中的`autoMigrations`属性来完成，比如我们想将user_name列名改为userName

```kotlin
@Database(
    entities = [User::class],
    version = 2,
    autoMigrations = [AutoMigration(1, 2, ReNameColumnMigration::class)]
)
abstract class AppDataBase : RoomDatabase() {
    abstract fun userDao(): UserDao
}

@RenameColumn(fromColumnName = "user_name", tableName = "user", toColumnName = "userName")
class ReNameColumnMigration : AutoMigrationSpec
```

或者通过Migration 类来手动定义迁移路径，比如我们想加一列birthday

```kotlin
val MIGRATION_1_2 = object : Migration(1, 2) {
    override fun migrate(database: SupportSQLiteDatabase) {
        database.execSQL("ALTER TABLE user ADD COLUMN birthday TEXT")
    }
}
```

然后再到创建实例时

```kotlin
fun getDatabase(context: Context): AppDataBase {
    synchronized(AppDataBase::class) {
        if (!::INSTANCE.isInitialized) {
            INSTANCE = Room.databaseBuilder(
                context.applicationContext, AppDataBase::class.java, "user.db"
            ).addMigrations(MIGRATION_1_2).fallbackToDestructiveMigration().build()
        }
    }
    return INSTANCE
}
```