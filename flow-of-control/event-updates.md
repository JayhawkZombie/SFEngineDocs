## Event Updates

Events can occur at any time.  The engine will poll events at the beginning of every tick, and will invoke handlers for any that it sees can be used.  There is no need to respond to every event.

Every `Level` class has an `EventUpdate` method that takes an instance of `sf::Event` that the engine will invoke when an event comes in that the main GUI didn't consume.

The following events are the ones your level could be notified of:

`sf::Event::KeyPressed` - A keyboard key was pressed
`sf::Event::KeyReleased` - A keyboard key was released
`sf::Event::MouseMoved` - The mouse was moved
`sf::Event::MouseButtonPressed` - A button on the mouse was pressed
`sf::Event::MouseButtonReleased` - A button on the mouse was released
`sf::Event::Resized` - The window was resized

To read more on SFML events, read their [documentation](https://www.sfml-dev.org/documentation/2.4.2/classsf_1_1Event.php).

Your level will **not** be notified of the window closing.  Instead, `BasicLevel::OnShutDown` will be called, letting your level know that the engine is shutting down and you need to clean up.  This is pure virtual, so you _must_ override it if you make a custom level.  If you utilize an existing level, you shouldn't need to do anything, since the level will be made such that it knows what to clean up and how to clean it up.

You will **NEVER** be notified of an event in the middle of rendering, and you will **ALWAYS** be told of **ALL** events **BEFORE** `TickUpdate` is called for your level.  If an event occurs during `TickUpdate` or `Render` then you will be notified of it during the next frame.