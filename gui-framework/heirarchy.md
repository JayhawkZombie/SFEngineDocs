## GUI Heirarchy

The heirarchy of GUI elements important to take into account when creating your GUI.

The main GUI can store anything you need it to, but it's probably best to store all of that in a `tgui::Panel` or a similar wrapper.  When you do this, the main GUI will only have immediate knowledge of this panel/wrapper, but won't have immediate knowledge of any of the panel's child elements.  That is ideal, since you can now have high control over everything that the GUI will be holding/displaying.

To add something to the main GUI, you can call `tgui::Gui::addElement` and pass in a pointer to your element.  You can also optionally pass in a string to identify that element with, and you can use this string to retrieve it later if you lost your pointer to it somehow.
You can also used reserved element names to retrieve intrinsic engine GUI elements (like the console), since they are not used/displayed/enabled unless one explicitly asks for it to be used.

So there is the main `tgui::Gui` object owned by the engine core.  You can program under the assumption that this will  be valid throughout the lifetime of your objects, so long as your objects die when they are supposed to.

Wrapper classes (can hold child elements):

* `tgui::ChildWindow` - can hold pretty much anything
* `tgui::MessageBox` - can hold pretty much anything
* `tgui::Panel` - a little more restrictive, behaves like a panel would, but not disconnected

Elements can be invisible, however, and you can use this to your advantage.

