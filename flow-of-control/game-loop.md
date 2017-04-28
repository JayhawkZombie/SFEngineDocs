## Game Loop  

Ugh, finally something exciting?  That depends on your definition of exciting, but we like it!  

It is here that all the timing is done.  It essentially is just one infinite loop with a break check to see if it should leave the loop.

```cpp
for (;;) {
  Closed = CEngine->m_Handler.PollEvents(m_CurrentRenderWindow, SEvent, true);
  if (Closed || CEngine->m_SignalForClose || !m_CurrentRenderWindow || !m_CurrentRenderWindow->isOpen())
        break;
 ...
 
 }
 ```
 
 K, let's break that down. `Closed` is a boolean that tells us if the event that we just polled closed the window.  
 There is also a global flag called `FlagForClose` (declared as `volatile`) that one can change from anywhere in the engine and it will cause the engine to initiate its shutdown process.  
 

The `!m_CurrentRenderWindow || !m_CurrentRenderWindow->isOpen())` is just checking to make sure the window didn't somehow get destroyed or closed without us knowing about it.  
It'll break out of the loop if any 4 of those conditions are satisfied.  

Now the next bit:  

```cpp
try
{
  CurrentTickStart = SClockHigh::now();
  STickDelta = STimeDuration<SFLOAT, std::milli>(CurrentTickStart - LastTickStart).count();
  ...
  LastTickStart = CurrentTickStart;
}
catch (EngineRuntimeError &e)
{

}
...
```

This is how much game time has passed since the last frame.  We just keep track of how much time has passed since the last frame and properly update.  We __do__ have fixed physics updates, so we do have to be careful that we update our physics properly.  We will dive more into that later.  

Here we see where the initial `TickUpdate` call is made

```cpp
if (CurrentLevel) {
  CurrentLevel->TickUpdate(STickDelta);
}
```

So if we have a valid `Level` instance to update, we update it.

After that we make the initial render call:  
```cpp
if (CurrentLevel) {
  CurrentLevel->RenderOnTexture(TextureTarget);
}
```

Here `TextureTarget` is an off-screen buffer we are rendering to instead of directly to the window's frame buffer.