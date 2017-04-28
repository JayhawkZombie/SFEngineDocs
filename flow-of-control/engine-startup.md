## Engine Startup  

"Cool", you might think, "but that was pretty boring, it hasn't done anything cool".

No, no it hasn't.  Let's change that.  

Inside `Engine::Startup` is where the brunt of the initialization and setup work is done.  

This will load ini configs from `SFEngine.ini`, initialize the `TGUI` main GUI window, and initialize a level to show while loading the "MainMenu" level that will eventually be shown after this process.  

Note that the engine pretty much already sets this up for you.  We'll see later how to set up which level starts up first (no, you won't have to change some ugly engine code).  

It is also in here that the engine will expose its methods to the ChaiScript scripting engine, as well as request that other classes register their interfaces as well.  

And then it's off to the game loop!