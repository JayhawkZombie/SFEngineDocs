# Building Archives

We will demonstrate how polymorphic serialization works by walking through how `sf::Shape` and 1 of its derived classes, `sf::RectangleShape`, are serialized.

Firstly, we can implement serialization methods in 2 ways - external to the class or as members.  Clearly, we can't add serialization members to the SFML classes without editing the SFML source, so we will do it using external methods.

We also do not have direct access to the data members.  We must use getters and setters.  Therefore we will have a `save` function to serialize our objects, and a `load` function to deserialize them.

In Cereal, these methods are templated, and the function format looks like:

For saving:

```
template<class Archive>
void save(Archive & ar, const TypeWeAreSerializing &Object)
```

For loading:

```
template<class Archive>
void load(Archive & ar, TypeWeAreLoading &Object)
```

The `save` function takes a const reference and the `load` function takes a non-const reference.

These classes are polymorphic, so let's walk the chain from the base class and then down.

### Saving sf::Transformable

`sf::Transformable` needs to save every member unique to it.  These are its position, scale, rotation, and origin.

To save it, we will need to extract those values and then save them to the archive.  This is what that looks like:

```
  template<class Archive>
  void save(Archive & ar, const sf::Transformable & trs)
  {
    auto pos = trs.getPosition();
    auto rot = trs.getRotation();
    auto scl = trs.getScale();
    auto org = trs.getOrigin();

    ar(pos);
    ar(rot);
    ar(scl);
    ar(org);
  }
```

Lets break that down.

The first 4 lines in the function are just getting the values for the members we need to save.

The next 4 lines save it to the archive.  If the types of these are not primitive types \(or a type Cereal knows how to save by default\), you will have to define the serialization methods for that type yourself.

We do not care the format Cereal uses to save these to the file.  There is some bookkeeping involved to keep track of the inheritance chain, but nearly all of the data saved is the values in a raw binary format.

### Saving sf::Drawable

`sf::Drawable` does not have any unique members.  The method is left empty, but still in-tact in case SFML later does add to this class.

```
  template<class Archive>
  void save(Archive & ar, const sf::Drawable & Draw)
  {
    //Empty, sf::Drawable holds no unique data to serialize
  }
```

### Saving sf::Shape

For `sf::Shape`, its save method will need to save into the archive everything that is unique to the class \(that we have access to\).

But remember how inherited classes are constructed.  The parent class must be initialized first, which means its save data must be archived first.

`sf::Shape` derives from both `sf::Drawable` and `sf::Transformable`, so both parent classes must be serialized before we serialize any members for the shape class.

Cereal has a function we can use for this, and using it looks like this:

```
    ar(cereal::base_class<sf::Transformable>(std::addressof(Shape)));
    ar(cereal::base_class<sf::Drawable>(std::addressof(Shape)));
```

Cool.  Now we have the parent classes done.  Now on to the shape class's data.

`sf::Shape` has 4 unique members we can serialize - its color, outline color, outline thickness, and its texture rect \(texture coordinates\).

We can use getters to retrieve the values for those members, and then archive those values:

```
    sf::Color   col = Shape.getFillColor();
    sf::Color   outlineColor = Shape.getOutlineColor();
    size_t      nPts = Shape.getPointCount();
    float       outline = Shape.getOutlineThickness();
    sf::IntRect tRect = Shape.getTextureRect();

    ar(col);
    ar(outline);
    ar(outlineColor);
    ar(nPts);
    ar(tRect);
```

_npts_ is archived only for consistency.  It doesn't serve any functional purpose.

And we're done.  Thus the entire `save` function for `sf::Shape` looks like:

```
  template<class Archive>
  void save(Archive & ar, const sf::Shape & Shape)
  {
    ar(cereal::base_class<sf::Transformable>(std::addressof(Shape)));
    ar(cereal::base_class<sf::Drawable>(std::addressof(Shape)));

    sf::Color   col = Shape.getFillColor();
    sf::Color   outlineColor = Shape.getOutlineColor();
    size_t      nPts = Shape.getPointCount();
    float       outline = Shape.getOutlineThickness();
    sf::IntRect tRect = Shape.getTextureRect();

    ar(col);
    ar(outline);
    ar(outlineColor);
    ar(nPts);
    ar(tRect);
  }
```

### Saving sf::RectangleShape

`sf::RectangleShape` derives from `sf::Shape`, so we must save the data for that class first, just like we saved `sf::Shape`'s parent class's data first.

```
ar(cereal::base_class<sf::Shape>(std::addressof(Rect)));
```

Then we must save the unique data from `sf::RectangleShape`, which is just its size:

```
    auto size = Rect.getSize();

    ar(size);
```

Thus its entire `save` function looks like:

```
  template<class Archive>
  void save(Archive & ar, const sf::RectangleShape & Rect)
  {
    ar(cereal::base_class<sf::Shape>(std::addressof(Rect)));

    auto size = Rect.getSize();

    ar(size);
  }
```



