# enclosure

## getting started

This library is published for Scala 3-0-0-RC2, 3-0-0-RC1, 2.13, 2.12, both on the JVM, and JS platforms.

```scala
libraryDependencies ++= "com.lorandszakacs" %% "enclosure" % "0.0.1"
```

### snapshots

To fetch snapshots of the library add the following to your build:

```scala
resolvers += "Sonatype OSS Snapshots" at "https://oss.sonatype.org/content/repositories/snapshots"
```

## usage

This library provides one single type: `com.lorandszakacs.enclosure.Enclosure` which simply contains the fully qualified name of the enclosing module. Where module is either one of:

- class
- object
- package (object)

Simply add an implicit parameter to your methods/classes, and a macro will generate a value for `Enclosure` for you.

Similar in usage to [`munit.Location`](https://github.com/scalameta/munit/blob/main/munit/shared/src/main/scala/munit/Location.scala), or [`org.tpolecat.SourcePos`](https://github.com/tpolecat/SourcePos)

```scala
package myapp
import com.lorandszakacs.enclosure.

object Printer {
  def locatedPrintln(s: String)(implicit enc: Enclosure): Unit =  {
    println(s"[${enc.fullModuleName}] s")
  }
}

// -------
package myapp.module

object Main extends App {
  myapp.Printer("in main!")
  // prints out:
  // [myapp.module.Main] in main!
  nestedMethod()
  // idem

  def nestedMethod(): Unit = {
    myapp.Printer("in main!")
  }
}
```

## motivation

The library was created to generalize, and make available to end-users, the `log4s`, `log4cats` logger name from enclosing class behavior. But it can obviously find itself other usages now as well.
