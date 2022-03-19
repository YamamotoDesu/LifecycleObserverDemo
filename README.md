# Activity Lifecycle - DessertPusher 

This is the toy app for lesson 4 of the [Android App Development in Kotlin course on Udacity](https://classroom.udacity.com/courses/ud9012/lessons/e487c600-ed68-4576-a35a-12f211cf032e/concepts/6a155d63-8153-4a56-95cb-1dfdf06aa173).

## Handling Lifecycles with Lifecycle-Aware Components   
### Without Observer Pattern

MainActivity.kt 
```kt

class MainActivity : AppCompatActivity() {

    private var revenue = 0
    private var dessertsSold = 0
    private lateinit var dessertTimer: DessertTimer
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        dessertTimer = DessertTimer()
        
    }
    override fun onStart() {
        super.onStart()
        dessertTimer.startTimer()
        Timber.i("onStart Called")
    }


    override fun onStop() {
        super.onStop()
        dessertTimer.stopTimer()
        Timber.i("onStop Called")
    }
```

DessertTimer.kt
```kt
class DessertTimer {

    fun startTimer() {
        // Create the runnable action, which prints out a log and increments the seconds counter
        runnable = Runnable {
            secondsCount++
            Timber.i("Timer is at : $secondsCount")
            // postDelayed re-adds the action to the queue of actions the Handler is cycling
            // through. The delayMillis param tells the handler to run the runnable in
            // 1 second (1000ms)
            handler.postDelayed(runnable, 1000)
        }

        // This is what initially starts the timer
        handler.postDelayed(runnable, 1000)

        // Note that the Thread the handler runs on is determined by a class called Looper.
        // In this case, no looper is defined, and it defaults to the main or UI thread.
    }

    fun stopTimer() {
        // Removes all pending posts of runnable from the handler's queue, effectively stopping the
        // timer
        handler.removeCallbacks(runnable)
    }
 }

```


### With Observer Pattern
MainActivity.kt 
```kt

class MainActivity : AppCompatActivity(), LifecycleObserver {

    private var revenue = 0
    private var dessertsSold = 0
    private lateinit var dessertTimer: DessertTimer
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        dessertTimer = DessertTimer(this.lifecycle)
        
    }
    override fun onStart() {
        super.onStart()
        dessertTimer.startTimer()
        Timber.i("onStart Called")
    }


    override fun onStop() {
        super.onStop()
        dessertTimer.stopTimer()
        Timber.i("onStop Called")
    }
```

DessertTimer.kt
```kt

class DessertTimer(lifecycle: Lifecycle) : LifecycleObserver {

 
    init {
        lifecycle.addObserver(this)
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_START)
    fun startTimer() {
        // Create the runnable action, which prints out a log and increments the seconds counter
        runnable = Runnable {
            secondsCount++
            Timber.i("Timer is at : $secondsCount")
            // postDelayed re-adds the action to the queue of actions the Handler is cycling
            // through. The delayMillis param tells the handler to run the runnable in
            // 1 second (1000ms)
            handler.postDelayed(runnable, 1000)
        }

        // This is what initially starts the timer
        handler.postDelayed(runnable, 1000)

        // Note that the Thread the handler runs on is determined by a class called Looper.
        // In this case, no looper is defined, and it defaults to the main or UI thread.
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_STOP)
    fun stopTimer() {
        // Removes all pending posts of runnable from the handler's queue, effectively stopping the
        // timer
        handler.removeCallbacks(runnable)
    }
```

