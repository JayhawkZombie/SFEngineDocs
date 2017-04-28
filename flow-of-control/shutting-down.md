## Shutting Down

Every good thing must come to an end, and as such the engine must eventually shut down.
During this process all OpenGL resources **must** be released.  If they are not and they are released after the render window is destroyed, we will crash.  There is no way to avoid this, as there must be a valid OpenGL context to be able to reference these resources.

Therefore, we recommend you do one of two things to manage your resources:
1) Use the engine's `ResourceManager` namespace to load/unload resources for you
2) Manage your own, and ensure you de-allocate anything you allocated before the level is actually destroyed.
    
After all of the levels are cleaned up and destroyed, the two other threads will be shut down, the `Messenger` thread and the `ASyncLevelStreamer` thread.  Any logs saved up will be purged and saved to a file.  If there were errors, we will hope they were recorded here so we can diagnose the issue and fix it (if possible).

Crashing while shutting down is likely to be the most common crash if there is one - it likely means there were resources remaining after the window was destroyed.