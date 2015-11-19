# Introduction #

The _DummyCreator_ contains the following static method to create a new object of the given class:
```
public <T> T create(final Class<T> clazz)
```

To call it, just use the class you want:
```
Integer integer = DummyCreator.create(Integer.class);
```

You can create objects of any type this way, but for some types you need to provide the _DummyCreator_ with extra information what to do in those cases.

The following cases need extra information:

  * Abstract types for which a specific implementation should be instantiated
  * Interfaces types idem
  * Cases where you want to provide your own logic to produce an object
  * Cases where you want to provide a specific Method or Constructor reference

You can use Class bindings to do this. Create a new ClassBindings object or use the static default getter to provide some default bindings (such as using ArrayLists for List types).

```
ClassBindings classBindings = ClassBindings.defaultBindings();
classBindings.add(Fruit.class, new ClassBasedFactory<Apple>(Apple.class));
DummyCreator dummyCreator = new DummyCreator(classBindings);

Fruit dummy = dummyCreator.create(Fruit.class); // dummy will be an Apple

// alternatively, you can create an object which contains a field of type Fruit 
FruitBasket fruitBasket = dummyCreator.create(FruitBasket.class);
```

But this works too:

```
ClassBindings classBindings = ClassBindings.defaultBindings();
classBindings.add(Fruit.class, new MethodBasedFactory<Apple>(MyFruitFactory.class.getMethod("randomApple", int.class)));
DummyCreator dummyCreator = new DummyCreator(classBindings);

Fruit dummy = dummyCreator.create(Fruit.class); // dummy will be whatever the factory returns
```

Or, if you want to use a specific a constructor:

```
ClassBindings classBindings = ClassBindings.defaultBindings();
classBindings.add(Fruit.class, new ConstructorBasedFactory<Apple>(Apple.class.getConstructor(String.class, Banana.class)));
DummyCreator dummyCreator = new DummyCreator(classBindings);

Fruit dummy = dummyCreator.create(Fruit.class); // dummy will be a named Apple created with a reference to a banana
```