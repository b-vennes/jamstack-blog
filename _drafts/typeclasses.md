---
layout: post
title: Typeclasses and Ad-Hoc Polymorphism
image: ''
imageAttributeUrl: ''
imageAttributeName: ''
categories: thoughts
---

### What Is A Typeclass?

Typeclass is a confusing name in the context of most languages, because they have little similarity to types or classes
in many object oriented languages.  At its core, a typeclass consists of some generic interface and an implementation
for a particular type.  The type can be any representation of a value (ex. `int`, `String`, `Person`, `Vehicle`...)
and the value itself can be anything.  The most important part of a typeclass is that the implementation exists
seperately from the implementation of the value itself.

### Typeclass Implementations Across Languages

For the following implementation examples, we will be implementing the
[Semigroup](https://typelevel.org/cats/typeclasses/semigroup.html) typeclass on integer, string, and list types.

Semigroup defines a single `combine` function that takes in two instances of type `T` and outputs their combined
value.

For example, calling `combine` with the integers `4` and `6` should return `10` if we are using addition as the
`combine` implementation (we could also implement this for multiplication`).

As an example of how typeclasses can be used within a library, I'll also be creating
a `combineAll` method that takes in a list of type `T`, a `Semigroup` instance, and
an initial value called `empty` and returns the combined value of the list. For the
list `1, 3, 5, 8`, the expected `combineAll` value would be `17`.

It is an implicit requirement that a semigroup's combine operation also be associative, meaning that combining a
group of values can occur in any order: `(1 + 2) + 3 = 1 + (2 + 3)`.

#### Scala

Scala will be the easiest example due to its built-in support for typeclass constructs.

``` Scala
trait Semigroup[T] {
    def combine(a: T, b: T): T
}
```

``` Scala
implicit val intAdditionSemigroup: Semigroup[Int] = _ + _
```

``` Scala
def combineAll[A](values: List[A], empty: A)(implicit semigroup: Semigroup[A]): A =
    values.foldLeft(empty)(semigroup.combine)

combineAll(List(1, 3, 5, 8), 0)
```

#### C#

Typeclasses in C# require some creativity because anonymous objects aren't a thing.  To access typeclass instances, we
will provide an `instance` static method which returns a single instantiated instance.  Although, this will make it difficult to use the `Semigroup<T>` typeclass in a generic context, it makes the code a little bit neater.

``` C#
public interface Semigroup<T>
{
    T Combine(T a, T b)
}
```

``` C#
public class IntSemigroup : Semigroup<int>
{
    public static IntSemigroup instance = new IntSemigroup()

    public int Combine(int a, int b) = a + b
}
```

``` C#
public T CombineAll<T>(Semigroup<T> instance, List<T> values, T empty) = values.Aggregate(empty, instance.combine)

CombineAll(IntSemigroup.instance, new List<int> { 1, 3, 5, 8 }, 0)
```

Note that because C# has no equivalent of the implicit scope found in Scala, the semigroup instance must be given
directly to the `CombineAll` function.

#### Typescript

Typescript's implementation is a little neater, but also requires passing the semigroup instance directly because an
`implicit` scope doesn't exist. Typescript *does* support instantiating anonymous objects, which makes creating the
typeclass instances easier.

``` Typescript
interface Semigroup<T> {
    combine(a: T, b: T): T
}
```

``` Typescript
const numberSemigroup = {
    combine(a: number, b: number): number {
        return a + b
    }
} as Semigroup<number>
```

``` Typescript
function combineAll<T>(instance: Semigroup<T>, values: T[], empty: T): T {
    return values.reduce((a, b, _) => instance.combine(a, b), empty)
}

combineAll(numberSemigroup, [1, 3, 5, 8], 0)
```

#### Rust

By default, `trait` implementations in Rust must be separated from the type definitions, which makes typeclass instances very simple.  In this case, we will be passing the semigroup type by refernece to make the implementation
of `combine_all` simpler.

``` Rust
trait Semigroup {
    fn combine(a: &Self, b: &Self) -> Self;
}
```

``` Rust
impl Semigroup for i32 {
    fn combine(a: &i32, b: &i32) -> i32 {
        a + b
    }
}
```

``` Rust
fn combine_all<T>(values: Vec<T>, empty: T) -> T where T: Semigroup {
    values.iter().fold(empty, |total, x| { T::combine(&total, &x) } )
}

combine_all(vec![1, 3, 5, 8], 0)
```

### Helpful Typeclasses

There are a number of common typeclasses that can be combined to implement similar behavior across all implementing
types.  `Semigroup`, `Eq`, and `Show` are simple typeclasses, but more complex ones like `Monoid`, `Monad`, and
`Functor` can provide a lot of functionality to types.  In these examples I will be working in Scala, though the
same rules apply to any of the other languages we used in the `Semigroup` example.

#### Eq

`Eq` provides a typesafe equals method `eqv`.  `Eq` should be used when we want to check that the value of two
values with the same type is the same.  Calling `eqv` with two values of different types should fail to compile.

``` Scala
trait Eq[T] {
    def eqv(a: T, b: T): Boolean
}
```

Because we provide a function that determines if two values are equal, we also get a function determining if two
values are not equal for free.

``` Scala
object Eq {
    def neqv[T](a: T, b: T)(implicit eq: Eq[T]) = !eq.eqv(a, b)
}
```

For implementing `neqv` I've created a companion class which takes an implicit `Eq` instance. The `neqv` method can
also be defined in the `Eq` trait itself.

#### Show

`Show` provides a method to get an explicit function for turning a value into a `String` type.  This is very helpful
when we want to print the state of a complex object to the console without having to override any existing toString
method.  Also, when a function wants to print the value of a generic type to the console, it can use its `Show`
implementation instead of relying on the built-in `toString` method.  Often, the default `toString` method will
print out garbage.

``` Scala
trait Show[T] {
    def show(value: T): String
}
```

Then, when we want to do some debugging from a function we write, we can require an explicit `Show` implementation
which the function caller provides.

``` Scala
implicit val showInt: Show[Int] = (value: Int) => s"Integer($value)" // ex. Integer(5)

def print(value: T)(implicit show: Show[T]): Unit = println(show.show(value))

print(500) // Integer(500)
```

#### Monoid

#### Monad

#### Functor

#### Foldable
