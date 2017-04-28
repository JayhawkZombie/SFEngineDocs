## GUI Framework Basics

SFEngine uses [TGUI](https://github.com/texus/TGUI/tree/master/src/TGUI) as its in-game GUI library.  There was the potential to use [SFGUI](https://github.com/TankOs/SFGUI), but it was incompatable with the SFML version used by the engine when we were searching for a GUI library to integrate.

TGUI is more intuitive to use, in our opinion, and fits much more in line with the programming methods used in the engine.

Let's consider creation of a TGUI element v. a SFGUI element

#### SFGUI
```cpp
auto button = sfg::Button::Create( "Greet SFGUI!" );
button->GetSignal( sfg::Widget::OnLeftClick ).Connect( [label] { label->SetText( "Hello SFGUI, pleased to meet you!" ); } );
```
Cool. But that looks needlessly complex.

#### TGUI
```cpp
tgui::Button::Ptr Button = TGUITheme->load("button");
Button->connect("clicked", [label](){ label->setText("Hello!"); });
```

The lambda syntax in SFGUI's example is missing the empty parameter list.  This is valid, but in the engine it will **ALWYAS** be included for clarity.

To us, the TGUI way of doing things is clear.  For one, we are explicitly telling it to load up our pre-made button layout/theme from a theme file we wrote. We don't have to do any `setTexture` or `setSize` nonsense anymore.
The TGUI callback system allows more flexibility, which allows us to use the syntax we did.  We do not "get" signals.  We use a TGUI method to connect them.

### Global `Engine::GUI` Variable

So how do you actually use this?  Good question.  The engine has an internal `tgui::Gui` variable stored, and you can get a pointer to it by calling `Engine::GUI()`.  This will return to you a `SPtrShared<tgui::Gui>` and you can use that to change the GUI.

When your level first starts, `Level::ShowGUI()` will be called, telling you that it is OK to add your GUI elements to the GUI.  When the level is exiting, `Level::HideGUI()` will be called, telling you that you need to remove your elements from the GUI.  Failure to do so will lead to undefined behavior, since the elements will exist but any pointers used in connected lambda functions may be invalid and dereferencing them will likely lead to a crash.

Why do we make you go through a method to get to the GUI?  So you don't inadvertently _delete_ it.