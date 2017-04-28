

## Program Startup

Let's take a look at how the engine actually starts up, what goes on during that startup process, and how it eventually gets to running your levels.

First, let's look at `SFEngine.cpp`

At the top you'll see the usual copyright blah blah, and then a rather ugly sequence of macro checks.  It'll be moved later.  

Notice the `#include "Engine\Engine.h` - this includes the core engine class, the one that initiates the startup process.

But then fuuuuuuther down, you'll see the following:

```cpp
int main(int argc, char **argv)

```

It's `main`! And guess what? It's only one line long! Here it is in all its glory:  
```cpp
int main(int argc, char **argv)
{
  return SFEngine::Engine::Go(argc, argv);
} 
```

So what happens there?

Well, `SFEngine` is implemented as a singleton, so it is in this static method called `Go` that this singleton instance will be initialized.  

If you look at the implementation of `Go`, it's not  particularly exciting.

```cpp
UINT32 Engine::Go(int argc, char **argv)
{
  SFENGINE_ASSERT(m_StaticCurrentEngine == nullptr);

  m_StaticCurrentEngine = new Engine;

  return m_StaticCurrentEngine->Init(argc, argv);
}
```
So it makes sure the singleton hasn't already been created and then creates it. Then it returns whatever `Init` returns for that instance of `Engine`.
