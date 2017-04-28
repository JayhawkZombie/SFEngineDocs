## Tick Updates  

Every frame is a "tick" in the game.  If the engine is using VSync, this will relatively uniform each time it is called, but if it is not then they will vary.  In the engine, this is calculated using `double` and is timed as precisely as can be in the number of ms passed since the last frame update.  

Whenever it is time for a tick update, the `CurrentLevel` will have `TickUpdate` called.  It is in there that you do all of your updating.

```cpp
if (CurrentLevel) {
  CurentLevel->TickUpdate(STickDelta);
}
```
Internally, not a whole lot is done.  We do some performance monitoring, and the engine may decide to throttle something if tick updates are taking too long.

This is **not** where rendering should be done.  In fact, there is not even a rendering surface given to you to render on.