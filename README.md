## Diapers — automatically generating DIAgrams of PERistent data Structures

This project aims to provide visualizations for common functional data structures used in Scala.
The visualizations are generated automatically from code, which allows to use them in an interactive fashion.
There are two visualization backends: `AsciiPlotter` and `DotPlotter`, which use ASCII art and graphviz respectively.


### Examples

First let’s look at the output from `AsciiPlotter`:

```scala
scala> import diapers.AsciiPlotter
import diapers.AsciiPlotter

scala> AsciiPlotter.plot(List(1, 2, 3))
 ┌─────────────────┐
 │Cons (1 | <Cons>)│
 └────────┬────────┘
          │         
          v         
 ┌─────────────────┐
 │Cons (2 | <Cons>)│
 └────────┬────────┘
          │         
          v         
 ┌────────────────┐ 
 │Cons (3 | <Nil>)│ 
 └────────┬───────┘ 
          │         
          v         
      ┌──────┐      
      │Nil ()│      
      └──────┘      
```

Not bad, huh? Still, I guess most people will prefer to use `DotPlotter`.

The following examples will assume these imports:
```scala
import scala.collection.immutable._
import java.nio.file.Paths
import diapers.DotPlotter
```

Since all the example code is actually run by [tut](https://github.com/tpolecat/tut),
you can find the resulting images in the `examples` directory.

#### Lists

```scala
val list1 = List(1, 2, 3, 4, 5)
val list2 = List(-1, -2) ++ list1.drop(2)

DotPlotter(Paths.get("examples", "lists.png")).plot(list1, list2)
```

<img src="examples/lists.png" height="500px" alt="Lists example" />

#### Queues

```scala
val queue = Queue(1, 2) :+ 3 :+ 4

DotPlotter(Paths.get("examples", "queue.png")).plot(queue)
```

<img src="examples/queue.png" height="500px" alt="Queue example" />

#### Vectors

```scala
 val vector = 1 +: Vector(10 to 43: _*) :+ 50

 DotPlotter(Paths.get("examples", "vector.png"), verticalSpacing = 2).plot(vector)
```

<img src="examples/vector.png" alt="Vectors example" />

#### HashSets

```scala
val set = HashSet(1L, 2L + 2L * Int.MaxValue, 3L, 4L)

DotPlotter(Paths.get("examples", "hashset.png")).plot(set)
```

<img src="examples/hashset.png" height="500px" alt="HashSet example" />

#### Case classes

Arbitrary case classes are supported automatically via
[shapeless’ Generic](https://github.com/milessabin/shapeless/wiki/Feature-overview:-shapeless-2.0.0#generic-representation-of-sealed-families-of-case-classes),
as long as the types or their fields are supported.

```scala
import com.softwaremill.quicklens._

case class Street(name: String, house: Int)
case class Address(street: Street, city: String)
case class Person(address: Address, age: Int)

val person1 = Person(Address(Street("Functional Rd.", 1), "London"), 35)
val person2 = person1.modify(_.address.street.house).using(_ + 3)

DotPlotter(Paths.get("examples", "case-classes.png")).plot(person1, person2)
```

<img src="examples/case-classes.png" alt="case classes example" />
