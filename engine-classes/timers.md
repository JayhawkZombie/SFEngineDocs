### Gameplay Timers

An engine would not be complete without a timer utility.

There is a `TimerManager` class that automatically takes care of storing all timers and keeping track of when they are due to expire.

Retrieve the global `TimerManager` instance and you can add timers as needed.

The function `TimerManager::AddTimer` is the only interface to add gameplay timers.

`void AddTimer(double duration, bool repeat, double delay, double scale, const std::function<void()> &callback)`

* duration - how far into the future the timer should expire \(in seconds\)
* repeat - whether or not the timer should restart immediately upon expiration
* delay - a delay \(in seconds\) that should be waited before starting the timer
* scale - a scale factor to slow down or speed up the timer's expiration
* callback - a function to call when the timer has expired



