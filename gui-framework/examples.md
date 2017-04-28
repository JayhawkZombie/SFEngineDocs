## GUI Examples

Consider the sample startup level:

![](/assets/EngineSampleStartGUI.PNG)

Here we see 5 buttons.

1 is disabled (though there is no visual difference - it just doesn't respond to interaction).

If we click on `select test level` then we are taken to this screen

![](/assets/SelectTestLevel.PNG)

Clicking on those will take you to another level (all of which we currently use for testing, so they aren't all that exciting).
Clicking on `back` will take you back to the previous screen.

Clicking on `credits` will take you to this page

![](/assets/PageWithText.PNG)

This page credits the developers that have worked on this (as of 4/28/2017) and those whose music and assets we have used.

As with the level select screen, clicking `Back` will take you to the previous screen.

Each of these screens, however, is not added to the GUI element-by-element.  Instead, a `tgui::Panel` is created for each, and we transition between those panels when the appropriate button is clicked.

Like this:

```cpp
tgui::Panel::Ptr LevelSelectPanel = MenuTheme->load("panel");

tgui::Button::Ptr MainToLevelSelectButton = MenuTheme->load("button");
//Position it and whatnot
MainToLevelSelectButton->connect(
    "clicked", 
    [this]() 
    { 
      this->MainPanel->hideWithEffect(...); 
      this->LevelSelectPanel->ShowWithEffect(...);  
    });
```

So now clicking on `MainToLevelSelectButton` will hide the main panel and show the select panel.  You can do a similar process on the level select panel to go back to the main panel.