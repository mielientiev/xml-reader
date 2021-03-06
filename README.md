[![Build Status](https://travis-ci.org/lavrov/xml-reader.svg?branch=master)](https://travis-ci.org/lavrov/xml-reader)

# xml-reader
Xml reader helps to convert xml documents into domain classes.

```scala
import nl.elmar.xml.reader._
import XmlPath._
import cats.syntax.cartesian._

case class Author(name: String)
case class Book(title: String, year: Int, author: List[Author])
case class Library(book: List[Book])

implicit val authorReader: Reader[Author] = 
  __.read[String] map Author
  
implicit val bookReader: Reader[Book] = 
  (__ \ "title").read[String] |@|
  (__ \ "year").read[Int] |@|
  (__ \ "author").read[List[Author]] map Book
  
val libraryReader: Reader[Library] = 
  (__ \ "book").read[List[Book]] map Library
  
libraryReader(
  <library>
    <book>
      <title>Programming in Scala, Third Edition</title>
      <year>2016</year>
      <author>Martin Odersky</author>
      <author>Lex Spoon</author>
      <author>Bill Venners</author>
    </book>
  </library>
)
```

The same result could have been achieved with:

```scala
import nl.elmar.xml.reader._
import GenericReader._

implicit val authorReader: Reader[Author] = 
  __.read[String] map Author
  
implicit val bookReader = Reader.reader[Book]

val libraryReader = Reader.reader[Library]

```

Thanks to [davegurnell](https://github.com/davegurnell) for his awesome [shapeless guide](https://github.com/davegurnell/shapeless-guide).
