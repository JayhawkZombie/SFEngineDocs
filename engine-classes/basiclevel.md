<p align="center">
  <b>BasicLevel Class</b>
</p>

The BasicLevel class, which all derived levels must derive from, derives from [`BaseEngineInterface`](/engine-classes/baseengineinterface.md).

It, of course, overrides everything it is supposed to, and adds some additional virtual functions that you can override in your level.



```
    /************************************************************************/
    /* Basic functionality methods                                          */
    /************************************************************************/
    virtual void LoadLevel(const std::string &lvlfile);
    virtual void OnBegin();
    virtual void OnEnd();
    virtual void HideUI();
    virtual void ShowUI();
    virtual void Reset();
    virtual void CleanUp();
    virtual void SpawnActor(std::shared_ptr<GenericActor> Actor, const sf::Vector2f &Position);
    virtual void SpawnObject(std::shared_ptr<LevelObject> Object, const sf::Vector2f &Position);
    virtual void RenderOnTexture(SharedRTexture Texture) = 0;
```

