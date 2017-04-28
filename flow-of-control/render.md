## Rendering

After every tick update, of course, the game needs to be rendered again.  

Every `Level` object must override the method `void LevelClass::RenderOnTexture(SharedTexture TextureTarget)`

The engine has an off-screen buffer onto which objects will render.  This will then be processed if needed and drawn to the screen.  You _can_ technically render to `CurrentRenderWindow` but it will very likely not appear, since the off-screen buffer used for render on top of it.

Since SFML is used for our graphics (well, most of it, for now), it is very easy to render objects.  Have a sprite?  Just do `TextureTarget->draw(MySprite);` and it'll be drawn to whatever surface `TextureTarget` refers to.

Rendering calls are usually not too expensive, but avoid rendering an excessive number of items.

For example:  

Every `Level` instance has a `sw::TileMap` variable available for use.  This is for tile maps.  You _could_ render every tile as an individual sprite, requiring potentially hundreds of draw calls; or you could have every tile be represented by 4 vertices for its position in the level and 4 to represent the texture coordinates for the 4 corners.  If you store all of that information in one large vertex array, you can draw the entire tile map in a single render call. This will have a potentially huge impact and increase the framerate.  

This is exactly how Selba Ward does this, and also how Thor manages rendering for particles.  Are we going to draw 800 sprites for all the particles? No, of course not! We keep track of the texture coordinates for each particle and the position using the same kind of vertex array used for tile maps.  The result is a big decrease in the render cost, so we can render hundreds (or thousands) of additional particles.