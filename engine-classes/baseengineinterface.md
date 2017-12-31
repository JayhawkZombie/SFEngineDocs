### SFEngine Classes

The engine comes with a set of core classes from which you can extend/derive.

The most basic of these is [`BaseEngineInterface`](https://github.com/JayhawkZombie/SFEngine/blob/master/Source/Public/Engine/BaseEngineInterface.h)

Everything that is to communicate with the engine must derive from this.

It has a set of pure virtual methods:

* `virtual SPtrShared<BaseEngineInterface> Clone() = 0;`
* `virtual void TickUpdate(const SFLOAT &TickDelta) = 0;`
* `virtual void Render(SharedRTexture Texture) = 0;`
* `virtual void OnShutDown() = 0;`
* `virtual SString GetClass() const = 0;`

  
Other methods are virtual, but not pure virtual.

* `virtual void EventUpdate(sf::Event event);`

* `virtual void HandleInputEvent(const UserEvent &event);`
* `virtual SString GetID() const;`
* `virtual UINT32 GetInternalID() const;`
* `virtual void SetInternalID(const UINT32 &Uid);`
* `virtual void SetID(const SString &ID);`

Aaaaand that's about it.

This class comes with 3 member variables:

* `EventHandler Handler`
* `SString ItemID`
* `UINT32 InternalID`  

`ItemID` and `InternalID` are initialized to `0` and `""`, respectively.

Basically every class we see after this will derive from this.

