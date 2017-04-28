### Engine Types

The engine typedefs many common types away so it is less verbose to use them.

Here are the typedefs for SFML types:

```cpp
/************************************************************************/
/* SFML basic type typedefs                                             */
/************************************************************************/
using SVector2F   = sf::Vector2<SFLOAT>;
using SVector2I   = sf::Vector2i;
using SVector2U   = sf::Vector2u;
using SVector2U32 = sf::Vector2<UINT32>;
using SVector2U64 = sf::Vector2<UINT64>;
using SVector2C   = sf::Vector2<char>;
using SVector3F   = sf::Vector3<SFLOAT>;
using SVector3I   = sf::Vector3i;
using SVector3U   = sf::Vector3<unsigned int>;
using SVector3C   = sf::Vector3<char>;
using SFLOATRECT  = sf::FloatRect;
using SINTRECT    = sf::IntRect;
using SSprite     = sf::Sprite;
```

Here are the typedefs for C++ standard library stuff

```cpp
/************************************************************************/
/* Standard library typedefs                                            */
/************************************************************************/
using SString = ::std::string;
template<typename T> using SPtrShared = std::shared_ptr<T>;
template<typename T> using SPtrWeak   = std::weak_ptr<T>;
template<typename T> using SPtrUnique = std::unique_ptr<T>;
template<typename T, typename U> using SPair      = std::pair<T, U>;
using SStringPair = SPair<SString, SString>;
using SStream     = ::std::stringstream;
using SSize_t     = std::size_t;
using SThread     = std::thread;
using SMutex      = std::mutex;
using SharedMutex = SPtrShared<SMutex>;
using SConditionVariable      = std::condition_variable;
using SharedConditionVariable = SPtrShared<SConditionVariable>;
using SPtrSharedMutex = SPtrShared<std::mutex>;
using SClockHigh = std::chrono::high_resolution_clock;
using SClockSys  = std::chrono::system_clock;
```