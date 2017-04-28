## BasicLevel Class

The [`BasicLevel`](https://github.com/JayhawkZombie/SFEngine/blob/master/Source/Public/Level/BasicLevel.h) class, which all derived levels must derive from, derives from [`BaseEngineInterface`](/engine-classes/baseengineinterface.md).

It, of course, overrides everything it is supposed to, and adds some additional virtual functions that you can override in your level.

```cpp
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

It also provides a number of variables that will be available for all levels to use:

```cpp
    /************************************************************************/
    /* Conditional variables                                                */
    /************************************************************************/
    bool m_DoUpdatePhysics = true;
    bool m_ShowGridLines   = false;

    /************************************************************************/
    /* Basic variables provided                                             */
    /************************************************************************/
    SString     m_ProjectPath;
    SFLOATRECT  m_CurrentView;
    SVector2U   m_Size;
    sw::TileMap m_TileMap;
    STexture    m_TileMapTexture;
    STDVector<SPtrShared<GameObject>> m_GameObjects;

    /************************************************************************/
    /* SFML Content - Textures/Autio/Etc                                    */
    /************************************************************************/
    SStringTextureMap     m_Textures;
    SStringSoundBufferMap m_SoundBuffers;
    ```
    
Ignoring the typo, these are all provided for levels to use.

As we will see in the next section, the [`GameObject`](engine-classes/gameobject-class.md) class is an abstract class that all of our game-like objects (Players, enemies, NPCs, guns, etc) will derive from.  This contains all of them loaded into this level. **NOTE**: This is not optimized, and a spacially-partitioned structure will be added.  You will want to use that when it is added.

There is almost no way to avoid the gross C++ template syntax, so we have tried our best to typedef them away for common types.