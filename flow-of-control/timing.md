## Timing

Timing is important, and we incorporated a timing library to help us with this.

##### [Kairos](https://github.com/Hapaxia/Kairos), by Hapaxia

We use library to assist with timing.  Most importantly, it allows us to specify a set frequency for our logic and physics updates and render as often as we want and achieve a smooth appearance.

Looking at [UpdatePass.cpp](https://github.com/JayhawkZombie/SFEngine/blob/master/Source/Private/Engine/UpdatePass.cpp), we see
```cpp
m_FPS.update();  
m_TimeStep.addFrame();  
```

Simply enough, it keeps track of our fps and adds a single frame timing to the time step's accumulator.

In the loop, we have 
```cpp
while (m_TimeStep.isUpdateRequired()) {
  float dt = m_TimeStep.getStepAsFloat();
  StepSimulation(dt);
}
```

While there is a full time sequence (logic update or physics update), we will loop and update it.  When there are no more full time steps to advance, we break out and return 
`float InterpolatedRemainingStep = m_TimeStep.getInterpolationAlphaAsFloat()`

The alpha here is the percentage of the way to another full time step was left in the accumulator, and we will use this alpha to interpolate the rendered positions of our objects to give a much smoother look.