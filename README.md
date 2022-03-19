# Activity Lifecycle - DessertPusher 

This is the toy app for lesson 4 of the [Android App Development in Kotlin course on Udacity](https://classroom.udacity.com/courses/ud9012/lessons/e487c600-ed68-4576-a35a-12f211cf032e/concepts/6a155d63-8153-4a56-95cb-1dfdf06aa173).

## Handling Lifecycles with Lifecycle-Aware Components   
### Without Observer Pattern

MainActivity.kt 
```kt

class MainActivity : AppCompatActivity(), LifecycleObserver {

    private var revenue = 0
    private var dessertsSold = 0
    private lateinit var dessertTimer: DessertTimer
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        dessertTimer = DessertTimer()
        
    }
    override fun onStart() {
        super.onStart()
        Timber.i("onStart Called")
    }


    override fun onStop() {
        super.onStop()
        Timber.i("onStop Called")
    }
```

