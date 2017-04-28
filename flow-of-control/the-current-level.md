## The Current Level

We can have as many levels as we want loaded into memory (whether or not you _should_ is a different story).  

The level that is currently active is internally stored as `static SharedLevel Engine::m_CurrentLevel` (`SharedLevel` is a typedef of `std::shared_ptr<Engine::BasicLevel>`).

It can be retrieved by calling `Engine::GetCurrentLevel()` and you will be given a `SharedPtr` that wraps the pointer to the current level.  Though you're unlikely to need this, there are cases for its use, and that is why it is exposed.

What you cannot do, however, is directly alter this variable.  The engine requires that there be a valid level instance at all times.  If at any time there is not, the engine will begin the shutdown process.

You can change levels by calling one of multiple methods:  

`void Engine::SwitchLevel(SharedLevel PtrToNewLevel)`
`void Engine::SwitchLevel(const SString &Name)`

If you use the latter, the level will be stored in `Engine::m_Levels`, which, like `m_CurrentLevel` is private and cannot directly be edited.  You can basically request that a level switch occur, but the engine can deny your request (like, for example, if the level doesn't actually exist).  If you need to load a level, you should use `ASyncLevelStreamer`, which will stream in your level from another thread.  The engine can asynchronously load OpenGL resources, so you should not need to worry about an issue arising from that.