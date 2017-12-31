# Loading Archives

If we can save archives, then surely we can load them - otherwise there would be no point to any of this!

BUT, we **must** load the data in the exact same order it was saved.

That means `sf::RectangleShape` must load its parent class `sf::Shape` first, and `sf::Shape` must load its parent class's data first \(in the same order it was saved, so `sf::Transformable` first and them `sf::Drawable` - this order is arbitrary, but it must remain consistent\).

### Loading sf::Transformable

The `load` function for `sf::Transformable` looks like:

```
  template<class Archive>
  void load(Archive & ar, sf::Transformable & trs)
  {
    sf::Vector2f pos;
    float rot;
    sf::Vector2f scale;
    sf::Vector2f origin;

    ar(pos);
    ar(rot);
    ar(scale);
    ar(origin);

    trs.setPosition(pos);
    trs.setRotation(rot);
    trs.setScale(scale);
    trs.setOrigin(origin);
  }
```

Since we are loading data, we first create temporary variables to load the data into, and then use setters to assign the values to the object passed by reference.

### Loading sf::Drawable

The `load` function for `sf::Drawable` is empty, as was the `save` method:

```
  template<class Archive>
  void load(Archive & ar, sf::Drawable & Draw)
  {
    //Empty, sf::Drawable holds no unique data to serialize
  }
```

### Loading sf::Shape

The `load` function for sf::Shape should load the data for `sf::Transformable`, then the data for `sf::Drawable`, and then its own data, exactly matching the order it was saved in:

```
  template<class Archive>
  void load(Archive & ar, sf::Shape & Shape)
  {
    ar(cereal::base_class<sf::Transformable>(std::addressof(Shape)));
    ar(cereal::base_class<sf::Drawable>(std::addressof(Shape)));

    sf::Color   col;
    float       outline;
    sf::Color   outlineColor;
    size_t      nPts;
    sf::IntRect tRect;

    ar(col);
    ar(outline);
    ar(outlineColor);
    ar(nPts);
    ar(tRect);

    Shape.setFillColor(col);
    Shape.setOutlineColor(outlineColor);
    Shape.setOutlineThickness(outline);
    Shape.setTextureRect(tRect);
  }
```

The first 2 lines load the data for the parent classes:

```
ar(cereal::base_class<sf::Transformable>(std::addressof(Shape)));
ar(cereal::base_class<sf::Drawable>(std::addressof(Shape)));
```

The next 5 lines declare temporary variables to hold the loaded data in.

```
sf::Color col;
float outline;
sf::Color outlineColor;
size_t nPts;
sf::IntRect tRect;
```

The next 5 lines load that data \(in exactly the same order it was saved in\):

```
    ar(col);
    ar(outline);
    ar(outlineColor);
    ar(nPts);
    ar(tRect);
```

Finally, the next 4 lines \(nPts is unused\) use setters to assign the loaded values to the object:

```
Shape.setFillColor(col);
Shape.setOutlineColor(outlineColor);
Shape.setOutlineThickness(outline);
Shape.setTextureRect(tRect);
```

### Loading sf::RectangleShape

To load `sf::RectangleShape`, it must load the data for its parent class, and then its own, again maintaining consistency in the order items were saved and loaded:

```
  template<class Archive>
  void load(Archive & ar, sf::RectangleShape & Rect)
  {
    ar(cereal::base_class<sf::Shape>(std::addressof(Rect)));

    sf::Vector2f size;

    ar(size);

    Rect.setSize(size);
  }
```

The first line loads the data for the parent class:

```
ar(cereal::base_class<sf::Shape>(std::addressof(Rect)));
```

The next line declared a temporary to hold the data we are loading:

```
sf::Vector2f size;
```

The next line loads that data:

```
ar(size);
```

Finally, the last line uses a setter to assign the newly loaded value to the object:

```
Rect.setSize(size);
```



After loading properly, the object should be identical to the one that was saved initially \(at the least the values we could access\).

